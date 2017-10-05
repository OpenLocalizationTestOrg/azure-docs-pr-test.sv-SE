---
title: "Simulerade hybrid cloud testmiljön | Microsoft Docs"
description: "Skapa en simulerad hybrid cloud miljö för IT pro eller utveckling testning, med två virtuella Azure-nätverk och en VNet-till-VNet-anslutning."
services: virtual-machines-windows
documentationcenter: 
author: JoeDavies-MSFT
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: ca5bf294-8172-44a9-8fed-d6f38d345364
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 09/30/2016
ms.author: josephd
ms.openlocfilehash: 4ec6f079b762a25894d822bfc098ea5442a1f7e4
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="set-up-a-simulated-hybrid-cloud-environment-for-testing"></a>Konfigurera en simulerad hybridmiljö i molnet för testning
Den här artikeln vägleder dig genom att skapa en simulerad hybrid cloud-miljö med Microsoft Azure med hjälp av två virtuella Azure-nätverk. Det här är den resulterande konfigurationen.

![](./media/ps-hybrid-cloud-test-env-sim/virtual-machines-windows-ps-hybrid-cloud-test-env-sim-ph4.png)

Det här simulerar en hybrid cloud-produktionsmiljö och består av:

* En simulerad och förenklat lokalt nätverk finns i Azure-nätverk (testlabb virtuellt nätverk).
* En simulerad mellan lokala virtuella nätverk finns i Azure (TestVNET).
* En VNet-till-VNet-anslutning mellan två virtuella nätverk.
* En sekundär domänkontrollant i TestVNET virtuella nätverk.

Detta ger en grund och vanliga startar punkt där du kan:

* Utveckla och testa program i en simulerad hybrid molnmiljö.
* Skapa test konfigurationer för datorer, en del i det virtuella nätverket testlabb och några i det virtuella nätverket i TestVNET, att simulera hybrid molnbaserade IT-arbetsbelastningar.

Det finns fyra huvudsakliga faser för att skapa den här hybrid cloud testmiljö:

1. Konfigurera det virtuella nätverket testlabb.
2. Skapa det virtuella nätverket mellan platser.
3. Skapa VNet-till-VNet VPN-anslutningen.
4. Konfigurera DC2. 

Den här konfigurationen krävs en Azure-prenumeration. Om du har en prenumeration med MSDN eller Visual Studio, se [månatliga Azure-kredit för Visual Studio-prenumeranter](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/).

> [!NOTE]
> Virtuella datorer och virtuella nätverks-gateway i Azure innebära en pågående kostnaden när de körs. Den här kostnaden debiteras mot din MSDN eller betald prenumeration. En Azure VPN-gateway implementeras som en uppsättning två virtuella Azure-datorer. Skapa testmiljön för att minimera kostnaderna och utför nödvändiga test och demonstration så snabbt som möjligt.
> 
> 

## <a name="phase-1-configure-the-testlab-virtual-network"></a>Fas 1: Konfigurera det virtuella nätverket testlabb
Följ instruktionerna i den [baskonfiguration testmiljö](https://technet.microsoft.com/library/mt771177.aspx) avsnittet för att konfigurera DC1, APP1 och CLIENT1-datorerna i virtuella Azure-nätverket med namnet testlabb. 

Starta sedan en Azure PowerShell-Kommandotolken.

> [!NOTE]
> Följande kommando anger Använd Azure PowerShell 1.0 och senare.
> 
> 

Logga in på ditt konto.

    Login-AzureRMAccount

Hämta din prenumerationsnamn med följande kommando.

    Get-AzureRMSubscription | Sort SubscriptionName | Select SubscriptionName

Ange din Azure-prenumeration. Använd samma prenumeration som du använde för att skapa den grundläggande konfigurationen i fas 1. Ersätt allt inom citattecken, inklusive den < och > tecken, med rätt namn.

    $subscr="<subscription name>"
    Get-AzureRmSubscription –SubscriptionName $subscr | Select-AzureRmSubscription

Sedan lägger till en gateway-undernät till det virtuella nätverket testlabb för grundläggande konfiguration, som ska användas som värd för Azure-gateway.

    $rgName="<name of your resource group that you used for your TestLab virtual network>"
    $locName="<Azure location name where you placed the TestLab virtual network, such as West US>"
    $vnet=Get-AzureRmVirtualNetwork -ResourceGroupName $rgName -Name TestLab
    Add-AzureRmVirtualNetworkSubnetConfig -Name "GatewaySubnet" -AddressPrefix 10.255.255.248/29 -VirtualNetwork $vnet
    Set-AzureRmVirtualNetwork -VirtualNetwork $vnet

Begär offentlig IP-adress tilldelas till gateway för det virtuella nätverket testlabb.

    $gwpip=New-AzureRmPublicIpAddress -Name TestLab_pip -ResourceGroupName $rgName -Location $locName -AllocationMethod Dynamic

Skapa sedan din gateway.

    $vnet=Get-AzureRmVirtualNetwork -Name TestLab -ResourceGroupName $rgName
    $subnet=Get-AzureRmVirtualNetworkSubnetConfig -Name "GatewaySubnet" -VirtualNetwork $vnet
    $gwipconfig=New-AzureRmVirtualNetworkGatewayIpConfig -Name TestLab_GWConfig -SubnetId $subnet.Id -PublicIpAddressId $gwpip.Id 
    New-AzureRmVirtualNetworkGateway -Name TestLab_GW -ResourceGroupName $rgName -Location $locName -IpConfigurations $gwipconfig -GatewayType Vpn -VpnType RouteBased

Tänk på att nya gatewayer kan ta minst 20 minuter att skapa.

Anslut till DC1 med autentiseringsuppgifter för corp\användare1 från Azure-portalen på den lokala datorn. Konfigurera CORP-domänen så att datorer och användare använda sina lokala domänkontrollanten för autentisering genom att köra dessa kommandon från en behörighet på Windows PowerShell-kommandotolk på DC1.

    New-ADReplicationSite -Name "TestLab" 
    New-ADReplicationSite -Name "TestVNET"
    New-ADReplicationSubnet -Name "10.0.0.0/8" -Site "TestLab"
    New-ADReplicationSubnet -Name "192.168.0.0/16" -Site "TestVNET"

Det här är din aktuella konfiguration.

![](./media/ps-hybrid-cloud-test-env-sim/virtual-machines-windows-ps-hybrid-cloud-test-env-sim-ph1.png)

## <a name="phase-2-create-the-testvnet-virtual-network"></a>Fas 2: Skapa TestVNET virtuella nätverk
Först skapar det virtuella nätverket TestVNET och skyddar dem med en nätverkssäkerhetsgrupp.

    $rgName="<name of the resource group that you used for your TestLab virtual network>"
    $locName="<Azure location name where you placed the TestLab virtual network, such as West US>"
    $locShortName="<Azure location name from $locName in all lowercase letters with spaces removed. Example:  westus>"
    $testSubnet=New-AzureRMVirtualNetworkSubnetConfig -Name "TestSubnet" -AddressPrefix 192.168.0.0/24
    $gatewaySubnet=New-AzureRmVirtualNetworkSubnetConfig -Name "GatewaySubnet" -AddressPrefix 192.168.255.248/29
    New-AzureRMVirtualNetwork -Name "TestVNET" -ResourceGroupName $rgName -Location $locName -AddressPrefix 192.168.0.0/16 -Subnet $testSubnet,$gatewaySubnet –DNSServer 10.0.0.4
    $rule1=New-AzureRMNetworkSecurityRuleConfig -Name "RDPTraffic" -Description "Allow RDP to all VMs on the subnet" -Access Allow -Protocol Tcp -Direction Inbound -Priority 100 -SourceAddressPrefix Internet -SourcePortRange * -DestinationAddressPrefix * -DestinationPortRange 3389
    New-AzureRMNetworkSecurityGroup -Name "TestSubnet" -ResourceGroupName $rgName -Location $locShortName -SecurityRules $rule1
    $vnet=Get-AzureRMVirtualNetwork -ResourceGroupName $rgName -Name TestVNET
    $nsg=Get-AzureRMNetworkSecurityGroup -Name "TestSubnet" -ResourceGroupName $rgName
    Set-AzureRMVirtualNetworkSubnetConfig -VirtualNetwork $vnet -Name "TestSubnet" -AddressPrefix 192.168.0.0/24 -NetworkSecurityGroup $nsg

Begär offentlig IP-adress tilldelas gatewayen för det virtuella nätverket TestVNET och skapa din gateway.

    $gwpip=New-AzureRmPublicIpAddress -Name TestVNET_pip -ResourceGroupName $rgName -Location $locName -AllocationMethod Dynamic
    $vnet=Get-AzureRmVirtualNetwork -Name TestVNET -ResourceGroupName $rgName
    $subnet=Get-AzureRmVirtualNetworkSubnetConfig -Name "GatewaySubnet" -VirtualNetwork $vnet
    $gwipconfig=New-AzureRmVirtualNetworkGatewayIpConfig -Name "TestVNET_GWConfig" -SubnetId $subnet.Id -PublicIpAddressId $gwpip.Id
    New-AzureRmVirtualNetworkGateway -Name "TestVNET_GW" -ResourceGroupName $rgName -Location $locName -IpConfigurations $gwipconfig -GatewayType Vpn -VpnType RouteBased

Det här är din aktuella konfiguration.

![](./media/ps-hybrid-cloud-test-env-sim/virtual-machines-windows-ps-hybrid-cloud-test-env-sim-ph2.png)

## <a name="phase-3-create-the-vnet-to-vnet-connection"></a>Fas 3: Skapa VNet-till-VNet-anslutningen
Skaffa först en slumpmässig kryptografiskt starkt, 32 tecken i förväg delad nyckel från nätverks- eller administratören. Alternativt kan använda informationen på [skapar en slumpmässig sträng för en i förväg delad nyckel för IPSec-](http://social.technet.microsoft.com/wiki/contents/articles/32330.create-a-random-string-for-an-ipsec-preshared-key.aspx) hämta en i förväg delad nyckel.

Använd dessa kommandon för att skapa VNet-till-VNet VPN-anslutningen, vilket kan ta lite tid att slutföra.

    $sharedKey="<pre-shared key value>"
    $gwTestLab=Get-AzureRmVirtualNetworkGateway -Name TestLab_GW -ResourceGroupName $rgName
    $gwTestVNET=Get-AzureRmVirtualNetworkGateway -Name TestVNET_GW -ResourceGroupName $rgName
    New-AzureRmVirtualNetworkGatewayConnection -Name TestLab_to_TestVNET -ResourceGroupName $rgName -VirtualNetworkGateway1 $gwTestLab -VirtualNetworkGateway2 $gwTestVNET -Location $locName -ConnectionType Vnet2Vnet -SharedKey $sharedKey
    New-AzureRmVirtualNetworkGatewayConnection -Name TestVNET_to_TestLab -ResourceGroupName $rgName -VirtualNetworkGateway1 $gwTestVNET -VirtualNetworkGateway2 $gwTestLab -Location $locName -ConnectionType Vnet2Vnet -SharedKey $sharedKey

Efter några minuter bör du upprätta en anslutning.

Det här är din aktuella konfiguration.

![](./media/ps-hybrid-cloud-test-env-sim/virtual-machines-windows-ps-hybrid-cloud-test-env-sim-ph3.png)

## <a name="phase-4-configure-dc2"></a>Steg 4: Konfigurera DC2
Först skapa en virtuell dator för DC2. Kommandona körs i Azure PowerShell-Kommandotolken på den lokala datorn.

    $rgName="<your resource group name>"
    $locName="<your Azure location, such as West US>"
    $saName="<the storage account name for the base configuration>"
    $vnet=Get-AzureRMVirtualNetwork -Name TestVNET -ResourceGroupName $rgName
    $pip=New-AzureRMPublicIpAddress -Name DC2-NIC -ResourceGroupName $rgName -Location $locName -AllocationMethod Dynamic
    $nic=New-AzureRMNetworkInterface -Name DC2-NIC -ResourceGroupName $rgName -Location $locName -SubnetId $vnet.Subnets[0].Id -PublicIpAddressId $pip.Id -PrivateIpAddress 192.168.0.4
    $vm=New-AzureRMVMConfig -VMName DC2 -VMSize Standard_A1
    $storageAcc=Get-AzureRMStorageAccount -ResourceGroupName $rgName -Name $saName
    $vhdURI=$storageAcc.PrimaryEndpoints.Blob.ToString() + "vhds/DC2-TestVNET-ADDSDisk.vhd"
    Add-AzureRMVMDataDisk -VM $vm -Name ADDS-Data -DiskSizeInGB 20 -VhdUri $vhdURI  -CreateOption empty
    $cred=Get-Credential -Message "Type the name and password of the local administrator account for DC2."
    $vm=Set-AzureRMVMOperatingSystem -VM $vm -Windows -ComputerName DC2 -Credential $cred -ProvisionVMAgent -EnableAutoUpdate
    $vm=Set-AzureRMVMSourceImage -VM $vm -PublisherName MicrosoftWindowsServer -Offer WindowsServer -Skus 2012-R2-Datacenter -Version "latest"
    $vm=Add-AzureRMVMNetworkInterface -VM $vm -Id $nic.Id
    $osDiskUri=$storageAcc.PrimaryEndpoints.Blob.ToString() + "vhds/DC2-TestLab-OSDisk.vhd"
    $vm=Set-AzureRMVMOSDisk -VM $vm -Name DC2-TestVNET-OSDisk -VhdUri $osDiskUri -CreateOption fromImage
    New-AzureRMVM -ResourceGroupName $rgName -Location $locName -VM $vm

Anslut sedan till den nya DC2 virtuella datorn från Azure-portalen.

Konfigurera en regel för Windows-brandväggen tillåter trafik för att testa grundläggande anslutningsmöjligheter. Kör följande kommandon från en behörighet på Windows PowerShell-Kommandotolken på DC2.

    Set-NetFirewallRule -DisplayName "File and Printer Sharing (Echo Request - ICMPv4-In)" -enabled True
    ping dc1.corp.contoso.com

Ping-kommandot ska resultera i fyra svar från IP-adressen 10.0.0.4. Det här är ett test av trafik via VNet-till-VNet-anslutningen.

Lägg sedan till datadisken extra på DC2 som en ny volym med enhetsbokstaven F:.

1. I den vänstra rutan i Serverhanteraren klickar du på **fil- och lagringstjänster**, och klicka sedan på **diskar**.
2. I innehållsrutan i den **diskar** klickar du på **disk 2** (med den **Partition** inställd på **okänd**).
3. Klicka på **uppgifter**, och klicka sedan på **ny volym**.
4. På sidan innan du börjar i guiden Ny volym, klickar på **nästa**.
5. Välj server och disk-sidan, klicka på **Disk 2**, och klicka sedan på **nästa**. När du uppmanas, klickar du på **OK**.
6. Klicka på Ange storleken på volymen sidan **nästa**.
7. Tilldela till en enhet enhetsbeteckning eller mapp, klicka på **nästa**.
8. På sidan Välj fil system inställningar **nästa**.
9. På sidan Bekräfta val **skapa**.
10. När du är klar klickar du på **Stäng**.

Konfigurera DC2 sedan som en replika-domänkontroller för domänen corp.contoso.com. Du kan köra dessa kommandon i Windows PowerShell-Kommandotolken på DC2.

    Install-WindowsFeature AD-Domain-Services -IncludeManagementTools
    Install-ADDSDomainController -Credential (Get-Credential CORP\User1) -DomainName "corp.contoso.com" -InstallDns:$true -DatabasePath "F:\NTDS" -LogPath "F:\Logs" -SysvolPath "F:\SYSVOL"

Observera att du uppmanas att ange både corp\användare1 lösenord och ett Directory återställningsläge för katalogtjänster (DSRM) och starta om DC2.

Nu när det virtuella nätverket TestVNET har en egen DNS-server (DC2), måste du konfigurera det virtuella nätverket TestVNET om du vill använda den här DNS-servern.

1. Klicka på ikonen virtuella nätverk i den vänstra rutan i Azure-portalen och klicka sedan på **TestVNET**.
2. På den **inställningar** klickar du på **DNS-servrar**.
3. I **primär DNS-server**, typen **192.168.0.4** ersätta 10.0.0.4.
4. Klicka på **Spara**.

Det här är din aktuella konfiguration. 

![](./media/ps-hybrid-cloud-test-env-sim/virtual-machines-windows-ps-hybrid-cloud-test-env-sim-ph4.png)

Molnmiljön simulerade hybrid är nu redo för testning.

## <a name="next-step"></a>Nästa steg
* Konfigurera en [webbaserade driftsapplikationer](ps-hybrid-cloud-test-env-lob.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) i den här miljön.

