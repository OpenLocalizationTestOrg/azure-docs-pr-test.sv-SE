---
title: "testmiljö för aaaSimulated hybrid cloud | Microsoft Docs"
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
ms.openlocfilehash: 6063022e373c2b3564e040c4fbee122e2438974b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="set-up-a-simulated-hybrid-cloud-environment-for-testing"></a>Konfigurera en simulerad hybridmiljö i molnet för testning
Den här artikeln vägleder dig genom att skapa en simulerad hybrid cloud-miljö med Microsoft Azure med hjälp av två virtuella Azure-nätverk. Här är hello resulterande konfigurationen.

![](./media/ps-hybrid-cloud-test-env-sim/virtual-machines-windows-ps-hybrid-cloud-test-env-sim-ph4.png)

Det här simulerar en hybrid cloud-produktionsmiljö och består av:

* En simulerad och förenklat lokalt nätverk finns i Azure-nätverk (hello testlabb virtuellt nätverk).
* En simulerad mellan lokala virtuella nätverk finns i Azure (TestVNET).
* En VNet-till-VNet-anslutning mellan hello två virtuella nätverk.
* En sekundär domänkontrollant i hello TestVNET virtuella nätverk.

Detta ger en grund och vanliga startar punkt där du kan:

* Utveckla och testa program i en simulerad hybrid molnmiljö.
* Skapa test konfigurationer för datorer, vissa hello testlabb virtuellt nätverk och vissa hello TestVNET virtuellt nätverk, toosimulate hybrid molnbaserade IT-arbetsbelastningar.

Det finns fyra huvudsakliga faser toosetting den här hybrid cloud testmiljö:

1. Konfigurera hello testlabb virtuellt nätverk.
2. Skapa virtuellt nätverk för hello mellan platser.
3. Skapa hello VNet-till-VNet VPN-anslutning.
4. Konfigurera DC2. 

Den här konfigurationen krävs en Azure-prenumeration. Om du har en prenumeration med MSDN eller Visual Studio, se [månatliga Azure-kredit för Visual Studio-prenumeranter](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/).

> [!NOTE]
> Virtuella datorer och virtuella nätverks-gateway i Azure innebära en pågående kostnaden när de körs. Den här kostnaden debiteras mot din MSDN eller betald prenumeration. En Azure VPN-gateway implementeras som en uppsättning två virtuella Azure-datorer. toominimize hello kostnader, skapa hello testmiljö och utföra nödvändiga test och demonstration så snabbt som möjligt.
> 
> 

## <a name="phase-1-configure-hello-testlab-virtual-network"></a>Fas 1: Konfigurera hello testlabb virtuellt nätverk
Använda hello instruktioner hello [baskonfiguration testmiljö](https://technet.microsoft.com/library/mt771177.aspx) avsnittet tooconfigure hello DC1, APP1 och CLIENT1 datorer i hello Azure-nätverk med namnet testlabb. 

Starta sedan en Azure PowerShell-Kommandotolken.

> [!NOTE]
> följande kommando anger hello använda Azure PowerShell 1.0 och senare.
> 
> 

Logga in tooyour konto.

    Login-AzureRMAccount

Hämta din prenumerationsnamn med hello följande kommando.

    Get-AzureRMSubscription | Sort SubscriptionName | Select SubscriptionName

Ange din Azure-prenumeration. Använd hello samma prenumeration som du använt toobuild din baskonfigurationen i fas 1. Ersätt allt inom hello citattecken, inklusive Hej < och > tecken, med hello rätt namn.

    $subscr="<subscription name>"
    Get-AzureRmSubscription –SubscriptionName $subscr | Select-AzureRmSubscription

Lägg till en gateway toohello testlabb virtuella undernätverk om grundläggande konfiguration, som kommer att använda toohost hello Azure gateway.

    $rgName="<name of your resource group that you used for your TestLab virtual network>"
    $locName="<Azure location name where you placed hello TestLab virtual network, such as West US>"
    $vnet=Get-AzureRmVirtualNetwork -ResourceGroupName $rgName -Name TestLab
    Add-AzureRmVirtualNetworkSubnetConfig -Name "GatewaySubnet" -AddressPrefix 10.255.255.248/29 -VirtualNetwork $vnet
    Set-AzureRmVirtualNetwork -VirtualNetwork $vnet

Begär offentlig IP-adress tooassign toohello gateway för virtuellt nätverk för hello testlabb.

    $gwpip=New-AzureRmPublicIpAddress -Name TestLab_pip -ResourceGroupName $rgName -Location $locName -AllocationMethod Dynamic

Skapa sedan din gateway.

    $vnet=Get-AzureRmVirtualNetwork -Name TestLab -ResourceGroupName $rgName
    $subnet=Get-AzureRmVirtualNetworkSubnetConfig -Name "GatewaySubnet" -VirtualNetwork $vnet
    $gwipconfig=New-AzureRmVirtualNetworkGatewayIpConfig -Name TestLab_GWConfig -SubnetId $subnet.Id -PublicIpAddressId $gwpip.Id 
    New-AzureRmVirtualNetworkGateway -Name TestLab_GW -ResourceGroupName $rgName -Location $locName -IpConfigurations $gwipconfig -GatewayType Vpn -VpnType RouteBased

Tänk på att nya gatewayer ta 20 minuter eller mer toocreate.

Hello Azure-portalen på den lokala datorn, ansluter du tooDC1 med hello autentiseringsuppgifter för corp\användare1. tooconfigure hello CORP-domänen så att datorer och användare använda sina lokala domänkontrollanten för autentisering, köra dessa kommandon från en behörighet på Windows PowerShell-Kommandotolken på DC1.

    New-ADReplicationSite -Name "TestLab" 
    New-ADReplicationSite -Name "TestVNET"
    New-ADReplicationSubnet -Name "10.0.0.0/8" -Site "TestLab"
    New-ADReplicationSubnet -Name "192.168.0.0/16" -Site "TestVNET"

Det här är din aktuella konfiguration.

![](./media/ps-hybrid-cloud-test-env-sim/virtual-machines-windows-ps-hybrid-cloud-test-env-sim-ph1.png)

## <a name="phase-2-create-hello-testvnet-virtual-network"></a>Fas 2: Skapa hello TestVNET virtuellt nätverk
Först skapar hello TestVNET virtuella nätverk och skyddar dem med en nätverkssäkerhetsgrupp.

    $rgName="<name of hello resource group that you used for your TestLab virtual network>"
    $locName="<Azure location name where you placed hello TestLab virtual network, such as West US>"
    $locShortName="<Azure location name from $locName in all lowercase letters with spaces removed. Example:  westus>"
    $testSubnet=New-AzureRMVirtualNetworkSubnetConfig -Name "TestSubnet" -AddressPrefix 192.168.0.0/24
    $gatewaySubnet=New-AzureRmVirtualNetworkSubnetConfig -Name "GatewaySubnet" -AddressPrefix 192.168.255.248/29
    New-AzureRMVirtualNetwork -Name "TestVNET" -ResourceGroupName $rgName -Location $locName -AddressPrefix 192.168.0.0/16 -Subnet $testSubnet,$gatewaySubnet –DNSServer 10.0.0.4
    $rule1=New-AzureRMNetworkSecurityRuleConfig -Name "RDPTraffic" -Description "Allow RDP tooall VMs on hello subnet" -Access Allow -Protocol Tcp -Direction Inbound -Priority 100 -SourceAddressPrefix Internet -SourcePortRange * -DestinationAddressPrefix * -DestinationPortRange 3389
    New-AzureRMNetworkSecurityGroup -Name "TestSubnet" -ResourceGroupName $rgName -Location $locShortName -SecurityRules $rule1
    $vnet=Get-AzureRMVirtualNetwork -ResourceGroupName $rgName -Name TestVNET
    $nsg=Get-AzureRMNetworkSecurityGroup -Name "TestSubnet" -ResourceGroupName $rgName
    Set-AzureRMVirtualNetworkSubnetConfig -VirtualNetwork $vnet -Name "TestSubnet" -AddressPrefix 192.168.0.0/24 -NetworkSecurityGroup $nsg

Sedan begäran en offentlig IP-adress toobe allokerats toohello gateway för virtuellt nätverk för hello TestVNET och skapa din gateway.

    $gwpip=New-AzureRmPublicIpAddress -Name TestVNET_pip -ResourceGroupName $rgName -Location $locName -AllocationMethod Dynamic
    $vnet=Get-AzureRmVirtualNetwork -Name TestVNET -ResourceGroupName $rgName
    $subnet=Get-AzureRmVirtualNetworkSubnetConfig -Name "GatewaySubnet" -VirtualNetwork $vnet
    $gwipconfig=New-AzureRmVirtualNetworkGatewayIpConfig -Name "TestVNET_GWConfig" -SubnetId $subnet.Id -PublicIpAddressId $gwpip.Id
    New-AzureRmVirtualNetworkGateway -Name "TestVNET_GW" -ResourceGroupName $rgName -Location $locName -IpConfigurations $gwipconfig -GatewayType Vpn -VpnType RouteBased

Det här är din aktuella konfiguration.

![](./media/ps-hybrid-cloud-test-env-sim/virtual-machines-windows-ps-hybrid-cloud-test-env-sim-ph2.png)

## <a name="phase-3-create-hello-vnet-to-vnet-connection"></a>Fas 3: Skapa hello VNet-till-VNet-anslutning
Skaffa först en slumpmässig kryptografiskt starkt, 32 tecken i förväg delad nyckel från nätverks- eller administratören. Alternativt kan använda hello information [skapar en slumpmässig sträng för en i förväg delad nyckel för IPSec-](http://social.technet.microsoft.com/wiki/contents/articles/32330.create-a-random-string-for-an-ipsec-preshared-key.aspx) tooobtain en i förväg delad nyckel.

Använd sedan dessa kommandon toocreate hello VNet-till-VNet VPN-anslutningen, vilket kan ta viss tid toocomplete.

    $sharedKey="<pre-shared key value>"
    $gwTestLab=Get-AzureRmVirtualNetworkGateway -Name TestLab_GW -ResourceGroupName $rgName
    $gwTestVNET=Get-AzureRmVirtualNetworkGateway -Name TestVNET_GW -ResourceGroupName $rgName
    New-AzureRmVirtualNetworkGatewayConnection -Name TestLab_to_TestVNET -ResourceGroupName $rgName -VirtualNetworkGateway1 $gwTestLab -VirtualNetworkGateway2 $gwTestVNET -Location $locName -ConnectionType Vnet2Vnet -SharedKey $sharedKey
    New-AzureRmVirtualNetworkGatewayConnection -Name TestVNET_to_TestLab -ResourceGroupName $rgName -VirtualNetworkGateway1 $gwTestVNET -VirtualNetworkGateway2 $gwTestLab -Location $locName -ConnectionType Vnet2Vnet -SharedKey $sharedKey

Efter några minuter ska hello anslutningen upprättas.

Det här är din aktuella konfiguration.

![](./media/ps-hybrid-cloud-test-env-sim/virtual-machines-windows-ps-hybrid-cloud-test-env-sim-ph3.png)

## <a name="phase-4-configure-dc2"></a>Steg 4: Konfigurera DC2
Först skapa en virtuell dator för DC2. Du kan köra dessa kommandon i Kommandotolken för hello Azure PowerShell på den lokala datorn.

    $rgName="<your resource group name>"
    $locName="<your Azure location, such as West US>"
    $saName="<hello storage account name for hello base configuration>"
    $vnet=Get-AzureRMVirtualNetwork -Name TestVNET -ResourceGroupName $rgName
    $pip=New-AzureRMPublicIpAddress -Name DC2-NIC -ResourceGroupName $rgName -Location $locName -AllocationMethod Dynamic
    $nic=New-AzureRMNetworkInterface -Name DC2-NIC -ResourceGroupName $rgName -Location $locName -SubnetId $vnet.Subnets[0].Id -PublicIpAddressId $pip.Id -PrivateIpAddress 192.168.0.4
    $vm=New-AzureRMVMConfig -VMName DC2 -VMSize Standard_A1
    $storageAcc=Get-AzureRMStorageAccount -ResourceGroupName $rgName -Name $saName
    $vhdURI=$storageAcc.PrimaryEndpoints.Blob.ToString() + "vhds/DC2-TestVNET-ADDSDisk.vhd"
    Add-AzureRMVMDataDisk -VM $vm -Name ADDS-Data -DiskSizeInGB 20 -VhdUri $vhdURI  -CreateOption empty
    $cred=Get-Credential -Message "Type hello name and password of hello local administrator account for DC2."
    $vm=Set-AzureRMVMOperatingSystem -VM $vm -Windows -ComputerName DC2 -Credential $cred -ProvisionVMAgent -EnableAutoUpdate
    $vm=Set-AzureRMVMSourceImage -VM $vm -PublisherName MicrosoftWindowsServer -Offer WindowsServer -Skus 2012-R2-Datacenter -Version "latest"
    $vm=Add-AzureRMVMNetworkInterface -VM $vm -Id $nic.Id
    $osDiskUri=$storageAcc.PrimaryEndpoints.Blob.ToString() + "vhds/DC2-TestLab-OSDisk.vhd"
    $vm=Set-AzureRMVMOSDisk -VM $vm -Name DC2-TestVNET-OSDisk -VhdUri $osDiskUri -CreateOption fromImage
    New-AzureRMVM -ResourceGroupName $rgName -Location $locName -VM $vm

Därefter ansluta toohello ny DC2 virtuell dator från hello Azure-portalen.

Konfigurera Windows-brandväggen regeln tooallow trafik för att testa grundläggande anslutningsmöjligheter. Kör följande kommandon från en behörighet på Windows PowerShell-Kommandotolken på DC2.

    Set-NetFirewallRule -DisplayName "File and Printer Sharing (Echo Request - ICMPv4-In)" -enabled True
    ping dc1.corp.contoso.com

hello ping-kommandot resulterar i fyra svar från IP-adressen 10.0.0.4. Det här är ett test av trafik över hello VNet-till-VNet-anslutningen.

Lägg till hello extra datadisk på DC2 som en ny volym med enhetsbokstaven för hello F:.

1. Hello vänster i Serverhanteraren klickar du på **fil- och lagringstjänster**, och klicka sedan på **diskar**.
2. I hello innehållsrutan i hello **diskar** klickar du på **disk 2** (med hello **Partition** ställa in också**okänd**).
3. Klicka på **uppgifter**, och klicka sedan på **ny volym**.
4. Hello sidan innan du börjar av hello guiden Ny volym, klicka på **nästa**.
5. Hello väljer hello server och disk-sidan, klicka på **Disk 2**, och klicka sedan på **nästa**. När du uppmanas, klickar du på **OK**.
6. Klicka på hello ange hello storleken på hello volym sida, **nästa**.
7. På hello tilldela tooa enhet enhetsbeteckning eller mapp klickar du på **nästa**.
8. På hello Välj fil system inställningar klickar du på **nästa**.
9. På hello bekräfta val klickar du på **skapa**.
10. När du är klar klickar du på **Stäng**.

Konfigurera DC2 sedan som en replika-domänkontroller för hello corp.contoso.com domänen. Du kan köra dessa kommandon från hello Windows PowerShell-Kommandotolken på DC2.

    Install-WindowsFeature AD-Domain-Services -IncludeManagementTools
    Install-ADDSDomainController -Credential (Get-Credential CORP\User1) -DomainName "corp.contoso.com" -InstallDns:$true -DatabasePath "F:\NTDS" -LogPath "F:\Logs" -SysvolPath "F:\SYSVOL"

Observera att du tillfrågas toosupply både hello corp\användare1 lösenord och ett Directory återställningsläge för katalogtjänster (DSRM) lösenord och toorestart DC2.

Nu när hello TestVNET virtuellt nätverk har en egen DNS-server (DC2), måste du konfigurera hello TestVNET virtuellt nätverk toouse DNS-servern.

1. Hello vänster i hello Azure-portalen, klicka hello virtuella nätverk ikonen och klickar sedan på **TestVNET**.
2. På hello **inställningar** klickar du på **DNS-servrar**.
3. I **primär DNS-server**, typen **192.168.0.4** tooreplace 10.0.0.4.
4. Klicka på **Spara**.

Det här är din aktuella konfiguration. 

![](./media/ps-hybrid-cloud-test-env-sim/virtual-machines-windows-ps-hybrid-cloud-test-env-sim-ph4.png)

Molnmiljön simulerade hybrid är nu redo för testning.

## <a name="next-step"></a>Nästa steg
* Konfigurera en [webbaserade driftsapplikationer](ps-hybrid-cloud-test-env-lob.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) i den här miljön.

