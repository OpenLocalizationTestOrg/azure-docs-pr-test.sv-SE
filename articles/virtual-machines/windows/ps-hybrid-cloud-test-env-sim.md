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
# <a name="set-up-a-simulated-hybrid-cloud-environment-for-testing"></a><span data-ttu-id="6acc0-103">Konfigurera en simulerad hybridmiljö i molnet för testning</span><span class="sxs-lookup"><span data-stu-id="6acc0-103">Set up a simulated hybrid cloud environment for testing</span></span>
<span data-ttu-id="6acc0-104">Den här artikeln vägleder dig genom att skapa en simulerad hybrid cloud-miljö med Microsoft Azure med hjälp av två virtuella Azure-nätverk.</span><span class="sxs-lookup"><span data-stu-id="6acc0-104">This article steps you through creating a simulated hybrid cloud environment with Microsoft Azure using two Azure virtual networks.</span></span> <span data-ttu-id="6acc0-105">Det här är den resulterande konfigurationen.</span><span class="sxs-lookup"><span data-stu-id="6acc0-105">Here is the resulting configuration.</span></span>

![](./media/ps-hybrid-cloud-test-env-sim/virtual-machines-windows-ps-hybrid-cloud-test-env-sim-ph4.png)

<span data-ttu-id="6acc0-106">Det här simulerar en hybrid cloud-produktionsmiljö och består av:</span><span class="sxs-lookup"><span data-stu-id="6acc0-106">This simulates a hybrid cloud production environment and consists of:</span></span>

* <span data-ttu-id="6acc0-107">En simulerad och förenklat lokalt nätverk finns i Azure-nätverk (testlabb virtuellt nätverk).</span><span class="sxs-lookup"><span data-stu-id="6acc0-107">A simulated and simplified on-premises network hosted in an Azure virtual network (the TestLab virtual network).</span></span>
* <span data-ttu-id="6acc0-108">En simulerad mellan lokala virtuella nätverk finns i Azure (TestVNET).</span><span class="sxs-lookup"><span data-stu-id="6acc0-108">A simulated cross-premises virtual network hosted in Azure (TestVNET).</span></span>
* <span data-ttu-id="6acc0-109">En VNet-till-VNet-anslutning mellan två virtuella nätverk.</span><span class="sxs-lookup"><span data-stu-id="6acc0-109">A VNet-to-VNet connection between the two virtual networks.</span></span>
* <span data-ttu-id="6acc0-110">En sekundär domänkontrollant i TestVNET virtuella nätverk.</span><span class="sxs-lookup"><span data-stu-id="6acc0-110">A secondary domain controller in the TestVNET virtual network.</span></span>

<span data-ttu-id="6acc0-111">Detta ger en grund och vanliga startar punkt där du kan:</span><span class="sxs-lookup"><span data-stu-id="6acc0-111">This provides a basis and common starting point from which you can:</span></span>

* <span data-ttu-id="6acc0-112">Utveckla och testa program i en simulerad hybrid molnmiljö.</span><span class="sxs-lookup"><span data-stu-id="6acc0-112">Develop and test applications in a simulated hybrid cloud environment.</span></span>
* <span data-ttu-id="6acc0-113">Skapa test konfigurationer för datorer, en del i det virtuella nätverket testlabb och några i det virtuella nätverket i TestVNET, att simulera hybrid molnbaserade IT-arbetsbelastningar.</span><span class="sxs-lookup"><span data-stu-id="6acc0-113">Create test configurations of computers, some within the TestLab virtual network and some within the TestVNET virtual network, to simulate hybrid cloud-based IT workloads.</span></span>

<span data-ttu-id="6acc0-114">Det finns fyra huvudsakliga faser för att skapa den här hybrid cloud testmiljö:</span><span class="sxs-lookup"><span data-stu-id="6acc0-114">There are four major phases to setting up this hybrid cloud test environment:</span></span>

1. <span data-ttu-id="6acc0-115">Konfigurera det virtuella nätverket testlabb.</span><span class="sxs-lookup"><span data-stu-id="6acc0-115">Configure the TestLab virtual network.</span></span>
2. <span data-ttu-id="6acc0-116">Skapa det virtuella nätverket mellan platser.</span><span class="sxs-lookup"><span data-stu-id="6acc0-116">Create the cross-premises virtual network.</span></span>
3. <span data-ttu-id="6acc0-117">Skapa VNet-till-VNet VPN-anslutningen.</span><span class="sxs-lookup"><span data-stu-id="6acc0-117">Create the VNet-to-VNet VPN connection.</span></span>
4. <span data-ttu-id="6acc0-118">Konfigurera DC2.</span><span class="sxs-lookup"><span data-stu-id="6acc0-118">Configure DC2.</span></span> 

<span data-ttu-id="6acc0-119">Den här konfigurationen krävs en Azure-prenumeration.</span><span class="sxs-lookup"><span data-stu-id="6acc0-119">This configuration requires an Azure subscription.</span></span> <span data-ttu-id="6acc0-120">Om du har en prenumeration med MSDN eller Visual Studio, se [månatliga Azure-kredit för Visual Studio-prenumeranter](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/).</span><span class="sxs-lookup"><span data-stu-id="6acc0-120">If you have an MSDN or Visual Studio subscription, see [Monthly Azure credit for Visual Studio subscribers](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/).</span></span>

> [!NOTE]
> <span data-ttu-id="6acc0-121">Virtuella datorer och virtuella nätverks-gateway i Azure innebära en pågående kostnaden när de körs.</span><span class="sxs-lookup"><span data-stu-id="6acc0-121">Virtual machines and virtual network gateways in Azure incur an ongoing monetary cost when they are running.</span></span> <span data-ttu-id="6acc0-122">Den här kostnaden debiteras mot din MSDN eller betald prenumeration.</span><span class="sxs-lookup"><span data-stu-id="6acc0-122">This cost is billed against your MSDN or paid subscription.</span></span> <span data-ttu-id="6acc0-123">En Azure VPN-gateway implementeras som en uppsättning två virtuella Azure-datorer.</span><span class="sxs-lookup"><span data-stu-id="6acc0-123">An Azure VPN gateway is implemented as a set of two Azure virtual machines.</span></span> <span data-ttu-id="6acc0-124">Skapa testmiljön för att minimera kostnaderna och utför nödvändiga test och demonstration så snabbt som möjligt.</span><span class="sxs-lookup"><span data-stu-id="6acc0-124">To minimize the costs, create the test environment and perform your needed testing and demonstration as quickly as possible.</span></span>
> 
> 

## <a name="phase-1-configure-the-testlab-virtual-network"></a><span data-ttu-id="6acc0-125">Fas 1: Konfigurera det virtuella nätverket testlabb</span><span class="sxs-lookup"><span data-stu-id="6acc0-125">Phase 1: Configure the TestLab virtual network</span></span>
<span data-ttu-id="6acc0-126">Följ instruktionerna i den [baskonfiguration testmiljö](https://technet.microsoft.com/library/mt771177.aspx) avsnittet för att konfigurera DC1, APP1 och CLIENT1-datorerna i virtuella Azure-nätverket med namnet testlabb.</span><span class="sxs-lookup"><span data-stu-id="6acc0-126">Use the instructions in the [Base Configuration test environment](https://technet.microsoft.com/library/mt771177.aspx) topic to configure the DC1, APP1, and CLIENT1 computers in the Azure virtual network named TestLab.</span></span> 

<span data-ttu-id="6acc0-127">Starta sedan en Azure PowerShell-Kommandotolken.</span><span class="sxs-lookup"><span data-stu-id="6acc0-127">Next, start an Azure PowerShell prompt.</span></span>

> [!NOTE]
> <span data-ttu-id="6acc0-128">Följande kommando anger Använd Azure PowerShell 1.0 och senare.</span><span class="sxs-lookup"><span data-stu-id="6acc0-128">The following command sets use Azure PowerShell 1.0 and later.</span></span>
> 
> 

<span data-ttu-id="6acc0-129">Logga in på ditt konto.</span><span class="sxs-lookup"><span data-stu-id="6acc0-129">Sign in to your account.</span></span>

    Login-AzureRMAccount

<span data-ttu-id="6acc0-130">Hämta din prenumerationsnamn med följande kommando.</span><span class="sxs-lookup"><span data-stu-id="6acc0-130">Get your subscription name using the following command.</span></span>

    Get-AzureRMSubscription | Sort SubscriptionName | Select SubscriptionName

<span data-ttu-id="6acc0-131">Ange din Azure-prenumeration.</span><span class="sxs-lookup"><span data-stu-id="6acc0-131">Set your Azure subscription.</span></span> <span data-ttu-id="6acc0-132">Använd samma prenumeration som du använde för att skapa den grundläggande konfigurationen i fas 1.</span><span class="sxs-lookup"><span data-stu-id="6acc0-132">Use the same subscription that you used to build your base configuration in Phase 1.</span></span> <span data-ttu-id="6acc0-133">Ersätt allt inom citattecken, inklusive den < och > tecken, med rätt namn.</span><span class="sxs-lookup"><span data-stu-id="6acc0-133">Replace everything within the quotes, including the < and > characters, with the correct name.</span></span>

    $subscr="<subscription name>"
    Get-AzureRmSubscription –SubscriptionName $subscr | Select-AzureRmSubscription

<span data-ttu-id="6acc0-134">Sedan lägger till en gateway-undernät till det virtuella nätverket testlabb för grundläggande konfiguration, som ska användas som värd för Azure-gateway.</span><span class="sxs-lookup"><span data-stu-id="6acc0-134">Next, add a gateway subnet to the TestLab virtual network of your base configuration, which will be used to host the Azure gateway.</span></span>

    $rgName="<name of your resource group that you used for your TestLab virtual network>"
    $locName="<Azure location name where you placed the TestLab virtual network, such as West US>"
    $vnet=Get-AzureRmVirtualNetwork -ResourceGroupName $rgName -Name TestLab
    Add-AzureRmVirtualNetworkSubnetConfig -Name "GatewaySubnet" -AddressPrefix 10.255.255.248/29 -VirtualNetwork $vnet
    Set-AzureRmVirtualNetwork -VirtualNetwork $vnet

<span data-ttu-id="6acc0-135">Begär offentlig IP-adress tilldelas till gateway för det virtuella nätverket testlabb.</span><span class="sxs-lookup"><span data-stu-id="6acc0-135">Next, request a public IP address to assign to the gateway for the TestLab virtual network.</span></span>

    $gwpip=New-AzureRmPublicIpAddress -Name TestLab_pip -ResourceGroupName $rgName -Location $locName -AllocationMethod Dynamic

<span data-ttu-id="6acc0-136">Skapa sedan din gateway.</span><span class="sxs-lookup"><span data-stu-id="6acc0-136">Next, create your gateway.</span></span>

    $vnet=Get-AzureRmVirtualNetwork -Name TestLab -ResourceGroupName $rgName
    $subnet=Get-AzureRmVirtualNetworkSubnetConfig -Name "GatewaySubnet" -VirtualNetwork $vnet
    $gwipconfig=New-AzureRmVirtualNetworkGatewayIpConfig -Name TestLab_GWConfig -SubnetId $subnet.Id -PublicIpAddressId $gwpip.Id 
    New-AzureRmVirtualNetworkGateway -Name TestLab_GW -ResourceGroupName $rgName -Location $locName -IpConfigurations $gwipconfig -GatewayType Vpn -VpnType RouteBased

<span data-ttu-id="6acc0-137">Tänk på att nya gatewayer kan ta minst 20 minuter att skapa.</span><span class="sxs-lookup"><span data-stu-id="6acc0-137">Keep in mind that new gateways can take 20 minutes or more to create.</span></span>

<span data-ttu-id="6acc0-138">Anslut till DC1 med autentiseringsuppgifter för corp\användare1 från Azure-portalen på den lokala datorn.</span><span class="sxs-lookup"><span data-stu-id="6acc0-138">From the Azure portal on your local computer, connect to DC1 with the CORP\User1 credentials.</span></span> <span data-ttu-id="6acc0-139">Konfigurera CORP-domänen så att datorer och användare använda sina lokala domänkontrollanten för autentisering genom att köra dessa kommandon från en behörighet på Windows PowerShell-kommandotolk på DC1.</span><span class="sxs-lookup"><span data-stu-id="6acc0-139">To configure the CORP domain so that computers and users use their local domain controller for authentication, run these commands from an administrator-level Windows PowerShell command prompt on DC1.</span></span>

    New-ADReplicationSite -Name "TestLab" 
    New-ADReplicationSite -Name "TestVNET"
    New-ADReplicationSubnet -Name "10.0.0.0/8" -Site "TestLab"
    New-ADReplicationSubnet -Name "192.168.0.0/16" -Site "TestVNET"

<span data-ttu-id="6acc0-140">Det här är din aktuella konfiguration.</span><span class="sxs-lookup"><span data-stu-id="6acc0-140">This is your current configuration.</span></span>

![](./media/ps-hybrid-cloud-test-env-sim/virtual-machines-windows-ps-hybrid-cloud-test-env-sim-ph1.png)

## <a name="phase-2-create-the-testvnet-virtual-network"></a><span data-ttu-id="6acc0-141">Fas 2: Skapa TestVNET virtuella nätverk</span><span class="sxs-lookup"><span data-stu-id="6acc0-141">Phase 2: Create the TestVNET virtual network</span></span>
<span data-ttu-id="6acc0-142">Först skapar det virtuella nätverket TestVNET och skyddar dem med en nätverkssäkerhetsgrupp.</span><span class="sxs-lookup"><span data-stu-id="6acc0-142">First, create the TestVNET virtual network and protect it with a network security group.</span></span>

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

<span data-ttu-id="6acc0-143">Begär offentlig IP-adress tilldelas gatewayen för det virtuella nätverket TestVNET och skapa din gateway.</span><span class="sxs-lookup"><span data-stu-id="6acc0-143">Next, request a public IP address to be allocated to the gateway for the TestVNET virtual network and create your gateway.</span></span>

    $gwpip=New-AzureRmPublicIpAddress -Name TestVNET_pip -ResourceGroupName $rgName -Location $locName -AllocationMethod Dynamic
    $vnet=Get-AzureRmVirtualNetwork -Name TestVNET -ResourceGroupName $rgName
    $subnet=Get-AzureRmVirtualNetworkSubnetConfig -Name "GatewaySubnet" -VirtualNetwork $vnet
    $gwipconfig=New-AzureRmVirtualNetworkGatewayIpConfig -Name "TestVNET_GWConfig" -SubnetId $subnet.Id -PublicIpAddressId $gwpip.Id
    New-AzureRmVirtualNetworkGateway -Name "TestVNET_GW" -ResourceGroupName $rgName -Location $locName -IpConfigurations $gwipconfig -GatewayType Vpn -VpnType RouteBased

<span data-ttu-id="6acc0-144">Det här är din aktuella konfiguration.</span><span class="sxs-lookup"><span data-stu-id="6acc0-144">This is your current configuration.</span></span>

![](./media/ps-hybrid-cloud-test-env-sim/virtual-machines-windows-ps-hybrid-cloud-test-env-sim-ph2.png)

## <a name="phase-3-create-the-vnet-to-vnet-connection"></a><span data-ttu-id="6acc0-145">Fas 3: Skapa VNet-till-VNet-anslutningen</span><span class="sxs-lookup"><span data-stu-id="6acc0-145">Phase 3: Create the VNet-to-VNet connection</span></span>
<span data-ttu-id="6acc0-146">Skaffa först en slumpmässig kryptografiskt starkt, 32 tecken i förväg delad nyckel från nätverks- eller administratören.</span><span class="sxs-lookup"><span data-stu-id="6acc0-146">First, obtain a random, cryptographically strong, 32-character pre-shared key from your network or security administrator.</span></span> <span data-ttu-id="6acc0-147">Alternativt kan använda informationen på [skapar en slumpmässig sträng för en i förväg delad nyckel för IPSec-](http://social.technet.microsoft.com/wiki/contents/articles/32330.create-a-random-string-for-an-ipsec-preshared-key.aspx) hämta en i förväg delad nyckel.</span><span class="sxs-lookup"><span data-stu-id="6acc0-147">Alternately, use the information at [Create a random string for an IPsec preshared key](http://social.technet.microsoft.com/wiki/contents/articles/32330.create-a-random-string-for-an-ipsec-preshared-key.aspx) to obtain a pre-shared key.</span></span>

<span data-ttu-id="6acc0-148">Använd dessa kommandon för att skapa VNet-till-VNet VPN-anslutningen, vilket kan ta lite tid att slutföra.</span><span class="sxs-lookup"><span data-stu-id="6acc0-148">Next, use these commands to create the VNet-to-VNet VPN connection, which can take some time to complete.</span></span>

    $sharedKey="<pre-shared key value>"
    $gwTestLab=Get-AzureRmVirtualNetworkGateway -Name TestLab_GW -ResourceGroupName $rgName
    $gwTestVNET=Get-AzureRmVirtualNetworkGateway -Name TestVNET_GW -ResourceGroupName $rgName
    New-AzureRmVirtualNetworkGatewayConnection -Name TestLab_to_TestVNET -ResourceGroupName $rgName -VirtualNetworkGateway1 $gwTestLab -VirtualNetworkGateway2 $gwTestVNET -Location $locName -ConnectionType Vnet2Vnet -SharedKey $sharedKey
    New-AzureRmVirtualNetworkGatewayConnection -Name TestVNET_to_TestLab -ResourceGroupName $rgName -VirtualNetworkGateway1 $gwTestVNET -VirtualNetworkGateway2 $gwTestLab -Location $locName -ConnectionType Vnet2Vnet -SharedKey $sharedKey

<span data-ttu-id="6acc0-149">Efter några minuter bör du upprätta en anslutning.</span><span class="sxs-lookup"><span data-stu-id="6acc0-149">After a few minutes, the connection should be established.</span></span>

<span data-ttu-id="6acc0-150">Det här är din aktuella konfiguration.</span><span class="sxs-lookup"><span data-stu-id="6acc0-150">This is your current configuration.</span></span>

![](./media/ps-hybrid-cloud-test-env-sim/virtual-machines-windows-ps-hybrid-cloud-test-env-sim-ph3.png)

## <a name="phase-4-configure-dc2"></a><span data-ttu-id="6acc0-151">Steg 4: Konfigurera DC2</span><span class="sxs-lookup"><span data-stu-id="6acc0-151">Phase 4: Configure DC2</span></span>
<span data-ttu-id="6acc0-152">Först skapa en virtuell dator för DC2.</span><span class="sxs-lookup"><span data-stu-id="6acc0-152">First, create a virtual machine for DC2.</span></span> <span data-ttu-id="6acc0-153">Kommandona körs i Azure PowerShell-Kommandotolken på den lokala datorn.</span><span class="sxs-lookup"><span data-stu-id="6acc0-153">Run these commands at the Azure PowerShell command prompt on your local computer.</span></span>

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

<span data-ttu-id="6acc0-154">Anslut sedan till den nya DC2 virtuella datorn från Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="6acc0-154">Next, connect to the new DC2 virtual machine from the Azure portal.</span></span>

<span data-ttu-id="6acc0-155">Konfigurera en regel för Windows-brandväggen tillåter trafik för att testa grundläggande anslutningsmöjligheter.</span><span class="sxs-lookup"><span data-stu-id="6acc0-155">Next, configure a Windows Firewall rule to allow traffic for basic connectivity testing.</span></span> <span data-ttu-id="6acc0-156">Kör följande kommandon från en behörighet på Windows PowerShell-Kommandotolken på DC2.</span><span class="sxs-lookup"><span data-stu-id="6acc0-156">From an administrator-level Windows PowerShell command prompt on DC2, run these commands.</span></span>

    Set-NetFirewallRule -DisplayName "File and Printer Sharing (Echo Request - ICMPv4-In)" -enabled True
    ping dc1.corp.contoso.com

<span data-ttu-id="6acc0-157">Ping-kommandot ska resultera i fyra svar från IP-adressen 10.0.0.4.</span><span class="sxs-lookup"><span data-stu-id="6acc0-157">The ping command should result in four successful replies from IP address 10.0.0.4.</span></span> <span data-ttu-id="6acc0-158">Det här är ett test av trafik via VNet-till-VNet-anslutningen.</span><span class="sxs-lookup"><span data-stu-id="6acc0-158">This is a test of traffic across the VNet-to-VNet connection.</span></span>

<span data-ttu-id="6acc0-159">Lägg sedan till datadisken extra på DC2 som en ny volym med enhetsbokstaven F:.</span><span class="sxs-lookup"><span data-stu-id="6acc0-159">Next, add the extra data disk on DC2 as a new volume with the drive letter F:.</span></span>

1. <span data-ttu-id="6acc0-160">I den vänstra rutan i Serverhanteraren klickar du på **fil- och lagringstjänster**, och klicka sedan på **diskar**.</span><span class="sxs-lookup"><span data-stu-id="6acc0-160">In the left pane of Server Manager, click **File and Storage Services**, and then click **Disks**.</span></span>
2. <span data-ttu-id="6acc0-161">I innehållsrutan i den **diskar** klickar du på **disk 2** (med den **Partition** inställd på **okänd**).</span><span class="sxs-lookup"><span data-stu-id="6acc0-161">In the contents pane, in the **Disks** group, click **disk 2** (with the **Partition** set to **Unknown**).</span></span>
3. <span data-ttu-id="6acc0-162">Klicka på **uppgifter**, och klicka sedan på **ny volym**.</span><span class="sxs-lookup"><span data-stu-id="6acc0-162">Click **Tasks**, and then click **New Volume**.</span></span>
4. <span data-ttu-id="6acc0-163">På sidan innan du börjar i guiden Ny volym, klickar på **nästa**.</span><span class="sxs-lookup"><span data-stu-id="6acc0-163">On the Before you begin page of the New Volume Wizard, click **Next**.</span></span>
5. <span data-ttu-id="6acc0-164">Välj server och disk-sidan, klicka på **Disk 2**, och klicka sedan på **nästa**.</span><span class="sxs-lookup"><span data-stu-id="6acc0-164">On the Select the server and disk page, click **Disk 2**, and then click **Next**.</span></span> <span data-ttu-id="6acc0-165">När du uppmanas, klickar du på **OK**.</span><span class="sxs-lookup"><span data-stu-id="6acc0-165">When prompted, click **OK**.</span></span>
6. <span data-ttu-id="6acc0-166">Klicka på Ange storleken på volymen sidan **nästa**.</span><span class="sxs-lookup"><span data-stu-id="6acc0-166">On the Specify the size of the volume page, click **Next**.</span></span>
7. <span data-ttu-id="6acc0-167">Tilldela till en enhet enhetsbeteckning eller mapp, klicka på **nästa**.</span><span class="sxs-lookup"><span data-stu-id="6acc0-167">On the Assign to a drive letter or folder page, click **Next**.</span></span>
8. <span data-ttu-id="6acc0-168">På sidan Välj fil system inställningar **nästa**.</span><span class="sxs-lookup"><span data-stu-id="6acc0-168">On the Select file system settings page, click **Next**.</span></span>
9. <span data-ttu-id="6acc0-169">På sidan Bekräfta val **skapa**.</span><span class="sxs-lookup"><span data-stu-id="6acc0-169">On the Confirm selections page, click **Create**.</span></span>
10. <span data-ttu-id="6acc0-170">När du är klar klickar du på **Stäng**.</span><span class="sxs-lookup"><span data-stu-id="6acc0-170">When complete, click **Close**.</span></span>

<span data-ttu-id="6acc0-171">Konfigurera DC2 sedan som en replika-domänkontroller för domänen corp.contoso.com.</span><span class="sxs-lookup"><span data-stu-id="6acc0-171">Next, configure DC2 as a replica domain controller for the corp.contoso.com domain.</span></span> <span data-ttu-id="6acc0-172">Du kan köra dessa kommandon i Windows PowerShell-Kommandotolken på DC2.</span><span class="sxs-lookup"><span data-stu-id="6acc0-172">Run these commands from the Windows PowerShell command prompt on DC2.</span></span>

    Install-WindowsFeature AD-Domain-Services -IncludeManagementTools
    Install-ADDSDomainController -Credential (Get-Credential CORP\User1) -DomainName "corp.contoso.com" -InstallDns:$true -DatabasePath "F:\NTDS" -LogPath "F:\Logs" -SysvolPath "F:\SYSVOL"

<span data-ttu-id="6acc0-173">Observera att du uppmanas att ange både corp\användare1 lösenord och ett Directory återställningsläge för katalogtjänster (DSRM) och starta om DC2.</span><span class="sxs-lookup"><span data-stu-id="6acc0-173">Note that you are prompted to supply both the CORP\User1 password and a Directory Services Restore Mode (DSRM) password, and to restart DC2.</span></span>

<span data-ttu-id="6acc0-174">Nu när det virtuella nätverket TestVNET har en egen DNS-server (DC2), måste du konfigurera det virtuella nätverket TestVNET om du vill använda den här DNS-servern.</span><span class="sxs-lookup"><span data-stu-id="6acc0-174">Now that the TestVNET virtual network has its own DNS server (DC2), you must configure the TestVNET virtual network to use this DNS server.</span></span>

1. <span data-ttu-id="6acc0-175">Klicka på ikonen virtuella nätverk i den vänstra rutan i Azure-portalen och klicka sedan på **TestVNET**.</span><span class="sxs-lookup"><span data-stu-id="6acc0-175">In the left pane of the Azure portal, click the virtual networks icon, and then click **TestVNET**.</span></span>
2. <span data-ttu-id="6acc0-176">På den **inställningar** klickar du på **DNS-servrar**.</span><span class="sxs-lookup"><span data-stu-id="6acc0-176">On the **Settings** tab, click **DNS servers**.</span></span>
3. <span data-ttu-id="6acc0-177">I **primär DNS-server**, typen **192.168.0.4** ersätta 10.0.0.4.</span><span class="sxs-lookup"><span data-stu-id="6acc0-177">In **Primary DNS server**, type **192.168.0.4** to replace 10.0.0.4.</span></span>
4. <span data-ttu-id="6acc0-178">Klicka på **Spara**.</span><span class="sxs-lookup"><span data-stu-id="6acc0-178">Click **Save**.</span></span>

<span data-ttu-id="6acc0-179">Det här är din aktuella konfiguration.</span><span class="sxs-lookup"><span data-stu-id="6acc0-179">This is your current configuration.</span></span> 

![](./media/ps-hybrid-cloud-test-env-sim/virtual-machines-windows-ps-hybrid-cloud-test-env-sim-ph4.png)

<span data-ttu-id="6acc0-180">Molnmiljön simulerade hybrid är nu redo för testning.</span><span class="sxs-lookup"><span data-stu-id="6acc0-180">Your simulated hybrid cloud environment is now ready for testing.</span></span>

## <a name="next-step"></a><span data-ttu-id="6acc0-181">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="6acc0-181">Next step</span></span>
* <span data-ttu-id="6acc0-182">Konfigurera en [webbaserade driftsapplikationer](ps-hybrid-cloud-test-env-lob.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) i den här miljön.</span><span class="sxs-lookup"><span data-stu-id="6acc0-182">Set up a [web-based, line of business application](ps-hybrid-cloud-test-env-lob.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) in this environment.</span></span>

