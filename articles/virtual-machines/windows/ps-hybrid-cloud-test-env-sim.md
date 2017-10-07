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
# <a name="set-up-a-simulated-hybrid-cloud-environment-for-testing"></a><span data-ttu-id="6fcc2-103">Konfigurera en simulerad hybridmiljö i molnet för testning</span><span class="sxs-lookup"><span data-stu-id="6fcc2-103">Set up a simulated hybrid cloud environment for testing</span></span>
<span data-ttu-id="6fcc2-104">Den här artikeln vägleder dig genom att skapa en simulerad hybrid cloud-miljö med Microsoft Azure med hjälp av två virtuella Azure-nätverk.</span><span class="sxs-lookup"><span data-stu-id="6fcc2-104">This article steps you through creating a simulated hybrid cloud environment with Microsoft Azure using two Azure virtual networks.</span></span> <span data-ttu-id="6fcc2-105">Här är hello resulterande konfigurationen.</span><span class="sxs-lookup"><span data-stu-id="6fcc2-105">Here is hello resulting configuration.</span></span>

![](./media/ps-hybrid-cloud-test-env-sim/virtual-machines-windows-ps-hybrid-cloud-test-env-sim-ph4.png)

<span data-ttu-id="6fcc2-106">Det här simulerar en hybrid cloud-produktionsmiljö och består av:</span><span class="sxs-lookup"><span data-stu-id="6fcc2-106">This simulates a hybrid cloud production environment and consists of:</span></span>

* <span data-ttu-id="6fcc2-107">En simulerad och förenklat lokalt nätverk finns i Azure-nätverk (hello testlabb virtuellt nätverk).</span><span class="sxs-lookup"><span data-stu-id="6fcc2-107">A simulated and simplified on-premises network hosted in an Azure virtual network (hello TestLab virtual network).</span></span>
* <span data-ttu-id="6fcc2-108">En simulerad mellan lokala virtuella nätverk finns i Azure (TestVNET).</span><span class="sxs-lookup"><span data-stu-id="6fcc2-108">A simulated cross-premises virtual network hosted in Azure (TestVNET).</span></span>
* <span data-ttu-id="6fcc2-109">En VNet-till-VNet-anslutning mellan hello två virtuella nätverk.</span><span class="sxs-lookup"><span data-stu-id="6fcc2-109">A VNet-to-VNet connection between hello two virtual networks.</span></span>
* <span data-ttu-id="6fcc2-110">En sekundär domänkontrollant i hello TestVNET virtuella nätverk.</span><span class="sxs-lookup"><span data-stu-id="6fcc2-110">A secondary domain controller in hello TestVNET virtual network.</span></span>

<span data-ttu-id="6fcc2-111">Detta ger en grund och vanliga startar punkt där du kan:</span><span class="sxs-lookup"><span data-stu-id="6fcc2-111">This provides a basis and common starting point from which you can:</span></span>

* <span data-ttu-id="6fcc2-112">Utveckla och testa program i en simulerad hybrid molnmiljö.</span><span class="sxs-lookup"><span data-stu-id="6fcc2-112">Develop and test applications in a simulated hybrid cloud environment.</span></span>
* <span data-ttu-id="6fcc2-113">Skapa test konfigurationer för datorer, vissa hello testlabb virtuellt nätverk och vissa hello TestVNET virtuellt nätverk, toosimulate hybrid molnbaserade IT-arbetsbelastningar.</span><span class="sxs-lookup"><span data-stu-id="6fcc2-113">Create test configurations of computers, some within hello TestLab virtual network and some within hello TestVNET virtual network, toosimulate hybrid cloud-based IT workloads.</span></span>

<span data-ttu-id="6fcc2-114">Det finns fyra huvudsakliga faser toosetting den här hybrid cloud testmiljö:</span><span class="sxs-lookup"><span data-stu-id="6fcc2-114">There are four major phases toosetting up this hybrid cloud test environment:</span></span>

1. <span data-ttu-id="6fcc2-115">Konfigurera hello testlabb virtuellt nätverk.</span><span class="sxs-lookup"><span data-stu-id="6fcc2-115">Configure hello TestLab virtual network.</span></span>
2. <span data-ttu-id="6fcc2-116">Skapa virtuellt nätverk för hello mellan platser.</span><span class="sxs-lookup"><span data-stu-id="6fcc2-116">Create hello cross-premises virtual network.</span></span>
3. <span data-ttu-id="6fcc2-117">Skapa hello VNet-till-VNet VPN-anslutning.</span><span class="sxs-lookup"><span data-stu-id="6fcc2-117">Create hello VNet-to-VNet VPN connection.</span></span>
4. <span data-ttu-id="6fcc2-118">Konfigurera DC2.</span><span class="sxs-lookup"><span data-stu-id="6fcc2-118">Configure DC2.</span></span> 

<span data-ttu-id="6fcc2-119">Den här konfigurationen krävs en Azure-prenumeration.</span><span class="sxs-lookup"><span data-stu-id="6fcc2-119">This configuration requires an Azure subscription.</span></span> <span data-ttu-id="6fcc2-120">Om du har en prenumeration med MSDN eller Visual Studio, se [månatliga Azure-kredit för Visual Studio-prenumeranter](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/).</span><span class="sxs-lookup"><span data-stu-id="6fcc2-120">If you have an MSDN or Visual Studio subscription, see [Monthly Azure credit for Visual Studio subscribers](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/).</span></span>

> [!NOTE]
> <span data-ttu-id="6fcc2-121">Virtuella datorer och virtuella nätverks-gateway i Azure innebära en pågående kostnaden när de körs.</span><span class="sxs-lookup"><span data-stu-id="6fcc2-121">Virtual machines and virtual network gateways in Azure incur an ongoing monetary cost when they are running.</span></span> <span data-ttu-id="6fcc2-122">Den här kostnaden debiteras mot din MSDN eller betald prenumeration.</span><span class="sxs-lookup"><span data-stu-id="6fcc2-122">This cost is billed against your MSDN or paid subscription.</span></span> <span data-ttu-id="6fcc2-123">En Azure VPN-gateway implementeras som en uppsättning två virtuella Azure-datorer.</span><span class="sxs-lookup"><span data-stu-id="6fcc2-123">An Azure VPN gateway is implemented as a set of two Azure virtual machines.</span></span> <span data-ttu-id="6fcc2-124">toominimize hello kostnader, skapa hello testmiljö och utföra nödvändiga test och demonstration så snabbt som möjligt.</span><span class="sxs-lookup"><span data-stu-id="6fcc2-124">toominimize hello costs, create hello test environment and perform your needed testing and demonstration as quickly as possible.</span></span>
> 
> 

## <a name="phase-1-configure-hello-testlab-virtual-network"></a><span data-ttu-id="6fcc2-125">Fas 1: Konfigurera hello testlabb virtuellt nätverk</span><span class="sxs-lookup"><span data-stu-id="6fcc2-125">Phase 1: Configure hello TestLab virtual network</span></span>
<span data-ttu-id="6fcc2-126">Använda hello instruktioner hello [baskonfiguration testmiljö](https://technet.microsoft.com/library/mt771177.aspx) avsnittet tooconfigure hello DC1, APP1 och CLIENT1 datorer i hello Azure-nätverk med namnet testlabb.</span><span class="sxs-lookup"><span data-stu-id="6fcc2-126">Use hello instructions in hello [Base Configuration test environment](https://technet.microsoft.com/library/mt771177.aspx) topic tooconfigure hello DC1, APP1, and CLIENT1 computers in hello Azure virtual network named TestLab.</span></span> 

<span data-ttu-id="6fcc2-127">Starta sedan en Azure PowerShell-Kommandotolken.</span><span class="sxs-lookup"><span data-stu-id="6fcc2-127">Next, start an Azure PowerShell prompt.</span></span>

> [!NOTE]
> <span data-ttu-id="6fcc2-128">följande kommando anger hello använda Azure PowerShell 1.0 och senare.</span><span class="sxs-lookup"><span data-stu-id="6fcc2-128">hello following command sets use Azure PowerShell 1.0 and later.</span></span>
> 
> 

<span data-ttu-id="6fcc2-129">Logga in tooyour konto.</span><span class="sxs-lookup"><span data-stu-id="6fcc2-129">Sign in tooyour account.</span></span>

    Login-AzureRMAccount

<span data-ttu-id="6fcc2-130">Hämta din prenumerationsnamn med hello följande kommando.</span><span class="sxs-lookup"><span data-stu-id="6fcc2-130">Get your subscription name using hello following command.</span></span>

    Get-AzureRMSubscription | Sort SubscriptionName | Select SubscriptionName

<span data-ttu-id="6fcc2-131">Ange din Azure-prenumeration.</span><span class="sxs-lookup"><span data-stu-id="6fcc2-131">Set your Azure subscription.</span></span> <span data-ttu-id="6fcc2-132">Använd hello samma prenumeration som du använt toobuild din baskonfigurationen i fas 1.</span><span class="sxs-lookup"><span data-stu-id="6fcc2-132">Use hello same subscription that you used toobuild your base configuration in Phase 1.</span></span> <span data-ttu-id="6fcc2-133">Ersätt allt inom hello citattecken, inklusive Hej < och > tecken, med hello rätt namn.</span><span class="sxs-lookup"><span data-stu-id="6fcc2-133">Replace everything within hello quotes, including hello < and > characters, with hello correct name.</span></span>

    $subscr="<subscription name>"
    Get-AzureRmSubscription –SubscriptionName $subscr | Select-AzureRmSubscription

<span data-ttu-id="6fcc2-134">Lägg till en gateway toohello testlabb virtuella undernätverk om grundläggande konfiguration, som kommer att använda toohost hello Azure gateway.</span><span class="sxs-lookup"><span data-stu-id="6fcc2-134">Next, add a gateway subnet toohello TestLab virtual network of your base configuration, which will be used toohost hello Azure gateway.</span></span>

    $rgName="<name of your resource group that you used for your TestLab virtual network>"
    $locName="<Azure location name where you placed hello TestLab virtual network, such as West US>"
    $vnet=Get-AzureRmVirtualNetwork -ResourceGroupName $rgName -Name TestLab
    Add-AzureRmVirtualNetworkSubnetConfig -Name "GatewaySubnet" -AddressPrefix 10.255.255.248/29 -VirtualNetwork $vnet
    Set-AzureRmVirtualNetwork -VirtualNetwork $vnet

<span data-ttu-id="6fcc2-135">Begär offentlig IP-adress tooassign toohello gateway för virtuellt nätverk för hello testlabb.</span><span class="sxs-lookup"><span data-stu-id="6fcc2-135">Next, request a public IP address tooassign toohello gateway for hello TestLab virtual network.</span></span>

    $gwpip=New-AzureRmPublicIpAddress -Name TestLab_pip -ResourceGroupName $rgName -Location $locName -AllocationMethod Dynamic

<span data-ttu-id="6fcc2-136">Skapa sedan din gateway.</span><span class="sxs-lookup"><span data-stu-id="6fcc2-136">Next, create your gateway.</span></span>

    $vnet=Get-AzureRmVirtualNetwork -Name TestLab -ResourceGroupName $rgName
    $subnet=Get-AzureRmVirtualNetworkSubnetConfig -Name "GatewaySubnet" -VirtualNetwork $vnet
    $gwipconfig=New-AzureRmVirtualNetworkGatewayIpConfig -Name TestLab_GWConfig -SubnetId $subnet.Id -PublicIpAddressId $gwpip.Id 
    New-AzureRmVirtualNetworkGateway -Name TestLab_GW -ResourceGroupName $rgName -Location $locName -IpConfigurations $gwipconfig -GatewayType Vpn -VpnType RouteBased

<span data-ttu-id="6fcc2-137">Tänk på att nya gatewayer ta 20 minuter eller mer toocreate.</span><span class="sxs-lookup"><span data-stu-id="6fcc2-137">Keep in mind that new gateways can take 20 minutes or more toocreate.</span></span>

<span data-ttu-id="6fcc2-138">Hello Azure-portalen på den lokala datorn, ansluter du tooDC1 med hello autentiseringsuppgifter för corp\användare1.</span><span class="sxs-lookup"><span data-stu-id="6fcc2-138">From hello Azure portal on your local computer, connect tooDC1 with hello CORP\User1 credentials.</span></span> <span data-ttu-id="6fcc2-139">tooconfigure hello CORP-domänen så att datorer och användare använda sina lokala domänkontrollanten för autentisering, köra dessa kommandon från en behörighet på Windows PowerShell-Kommandotolken på DC1.</span><span class="sxs-lookup"><span data-stu-id="6fcc2-139">tooconfigure hello CORP domain so that computers and users use their local domain controller for authentication, run these commands from an administrator-level Windows PowerShell command prompt on DC1.</span></span>

    New-ADReplicationSite -Name "TestLab" 
    New-ADReplicationSite -Name "TestVNET"
    New-ADReplicationSubnet -Name "10.0.0.0/8" -Site "TestLab"
    New-ADReplicationSubnet -Name "192.168.0.0/16" -Site "TestVNET"

<span data-ttu-id="6fcc2-140">Det här är din aktuella konfiguration.</span><span class="sxs-lookup"><span data-stu-id="6fcc2-140">This is your current configuration.</span></span>

![](./media/ps-hybrid-cloud-test-env-sim/virtual-machines-windows-ps-hybrid-cloud-test-env-sim-ph1.png)

## <a name="phase-2-create-hello-testvnet-virtual-network"></a><span data-ttu-id="6fcc2-141">Fas 2: Skapa hello TestVNET virtuellt nätverk</span><span class="sxs-lookup"><span data-stu-id="6fcc2-141">Phase 2: Create hello TestVNET virtual network</span></span>
<span data-ttu-id="6fcc2-142">Först skapar hello TestVNET virtuella nätverk och skyddar dem med en nätverkssäkerhetsgrupp.</span><span class="sxs-lookup"><span data-stu-id="6fcc2-142">First, create hello TestVNET virtual network and protect it with a network security group.</span></span>

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

<span data-ttu-id="6fcc2-143">Sedan begäran en offentlig IP-adress toobe allokerats toohello gateway för virtuellt nätverk för hello TestVNET och skapa din gateway.</span><span class="sxs-lookup"><span data-stu-id="6fcc2-143">Next, request a public IP address toobe allocated toohello gateway for hello TestVNET virtual network and create your gateway.</span></span>

    $gwpip=New-AzureRmPublicIpAddress -Name TestVNET_pip -ResourceGroupName $rgName -Location $locName -AllocationMethod Dynamic
    $vnet=Get-AzureRmVirtualNetwork -Name TestVNET -ResourceGroupName $rgName
    $subnet=Get-AzureRmVirtualNetworkSubnetConfig -Name "GatewaySubnet" -VirtualNetwork $vnet
    $gwipconfig=New-AzureRmVirtualNetworkGatewayIpConfig -Name "TestVNET_GWConfig" -SubnetId $subnet.Id -PublicIpAddressId $gwpip.Id
    New-AzureRmVirtualNetworkGateway -Name "TestVNET_GW" -ResourceGroupName $rgName -Location $locName -IpConfigurations $gwipconfig -GatewayType Vpn -VpnType RouteBased

<span data-ttu-id="6fcc2-144">Det här är din aktuella konfiguration.</span><span class="sxs-lookup"><span data-stu-id="6fcc2-144">This is your current configuration.</span></span>

![](./media/ps-hybrid-cloud-test-env-sim/virtual-machines-windows-ps-hybrid-cloud-test-env-sim-ph2.png)

## <a name="phase-3-create-hello-vnet-to-vnet-connection"></a><span data-ttu-id="6fcc2-145">Fas 3: Skapa hello VNet-till-VNet-anslutning</span><span class="sxs-lookup"><span data-stu-id="6fcc2-145">Phase 3: Create hello VNet-to-VNet connection</span></span>
<span data-ttu-id="6fcc2-146">Skaffa först en slumpmässig kryptografiskt starkt, 32 tecken i förväg delad nyckel från nätverks- eller administratören.</span><span class="sxs-lookup"><span data-stu-id="6fcc2-146">First, obtain a random, cryptographically strong, 32-character pre-shared key from your network or security administrator.</span></span> <span data-ttu-id="6fcc2-147">Alternativt kan använda hello information [skapar en slumpmässig sträng för en i förväg delad nyckel för IPSec-](http://social.technet.microsoft.com/wiki/contents/articles/32330.create-a-random-string-for-an-ipsec-preshared-key.aspx) tooobtain en i förväg delad nyckel.</span><span class="sxs-lookup"><span data-stu-id="6fcc2-147">Alternately, use hello information at [Create a random string for an IPsec preshared key](http://social.technet.microsoft.com/wiki/contents/articles/32330.create-a-random-string-for-an-ipsec-preshared-key.aspx) tooobtain a pre-shared key.</span></span>

<span data-ttu-id="6fcc2-148">Använd sedan dessa kommandon toocreate hello VNet-till-VNet VPN-anslutningen, vilket kan ta viss tid toocomplete.</span><span class="sxs-lookup"><span data-stu-id="6fcc2-148">Next, use these commands toocreate hello VNet-to-VNet VPN connection, which can take some time toocomplete.</span></span>

    $sharedKey="<pre-shared key value>"
    $gwTestLab=Get-AzureRmVirtualNetworkGateway -Name TestLab_GW -ResourceGroupName $rgName
    $gwTestVNET=Get-AzureRmVirtualNetworkGateway -Name TestVNET_GW -ResourceGroupName $rgName
    New-AzureRmVirtualNetworkGatewayConnection -Name TestLab_to_TestVNET -ResourceGroupName $rgName -VirtualNetworkGateway1 $gwTestLab -VirtualNetworkGateway2 $gwTestVNET -Location $locName -ConnectionType Vnet2Vnet -SharedKey $sharedKey
    New-AzureRmVirtualNetworkGatewayConnection -Name TestVNET_to_TestLab -ResourceGroupName $rgName -VirtualNetworkGateway1 $gwTestVNET -VirtualNetworkGateway2 $gwTestLab -Location $locName -ConnectionType Vnet2Vnet -SharedKey $sharedKey

<span data-ttu-id="6fcc2-149">Efter några minuter ska hello anslutningen upprättas.</span><span class="sxs-lookup"><span data-stu-id="6fcc2-149">After a few minutes, hello connection should be established.</span></span>

<span data-ttu-id="6fcc2-150">Det här är din aktuella konfiguration.</span><span class="sxs-lookup"><span data-stu-id="6fcc2-150">This is your current configuration.</span></span>

![](./media/ps-hybrid-cloud-test-env-sim/virtual-machines-windows-ps-hybrid-cloud-test-env-sim-ph3.png)

## <a name="phase-4-configure-dc2"></a><span data-ttu-id="6fcc2-151">Steg 4: Konfigurera DC2</span><span class="sxs-lookup"><span data-stu-id="6fcc2-151">Phase 4: Configure DC2</span></span>
<span data-ttu-id="6fcc2-152">Först skapa en virtuell dator för DC2.</span><span class="sxs-lookup"><span data-stu-id="6fcc2-152">First, create a virtual machine for DC2.</span></span> <span data-ttu-id="6fcc2-153">Du kan köra dessa kommandon i Kommandotolken för hello Azure PowerShell på den lokala datorn.</span><span class="sxs-lookup"><span data-stu-id="6fcc2-153">Run these commands at hello Azure PowerShell command prompt on your local computer.</span></span>

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

<span data-ttu-id="6fcc2-154">Därefter ansluta toohello ny DC2 virtuell dator från hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="6fcc2-154">Next, connect toohello new DC2 virtual machine from hello Azure portal.</span></span>

<span data-ttu-id="6fcc2-155">Konfigurera Windows-brandväggen regeln tooallow trafik för att testa grundläggande anslutningsmöjligheter.</span><span class="sxs-lookup"><span data-stu-id="6fcc2-155">Next, configure a Windows Firewall rule tooallow traffic for basic connectivity testing.</span></span> <span data-ttu-id="6fcc2-156">Kör följande kommandon från en behörighet på Windows PowerShell-Kommandotolken på DC2.</span><span class="sxs-lookup"><span data-stu-id="6fcc2-156">From an administrator-level Windows PowerShell command prompt on DC2, run these commands.</span></span>

    Set-NetFirewallRule -DisplayName "File and Printer Sharing (Echo Request - ICMPv4-In)" -enabled True
    ping dc1.corp.contoso.com

<span data-ttu-id="6fcc2-157">hello ping-kommandot resulterar i fyra svar från IP-adressen 10.0.0.4.</span><span class="sxs-lookup"><span data-stu-id="6fcc2-157">hello ping command should result in four successful replies from IP address 10.0.0.4.</span></span> <span data-ttu-id="6fcc2-158">Det här är ett test av trafik över hello VNet-till-VNet-anslutningen.</span><span class="sxs-lookup"><span data-stu-id="6fcc2-158">This is a test of traffic across hello VNet-to-VNet connection.</span></span>

<span data-ttu-id="6fcc2-159">Lägg till hello extra datadisk på DC2 som en ny volym med enhetsbokstaven för hello F:.</span><span class="sxs-lookup"><span data-stu-id="6fcc2-159">Next, add hello extra data disk on DC2 as a new volume with hello drive letter F:.</span></span>

1. <span data-ttu-id="6fcc2-160">Hello vänster i Serverhanteraren klickar du på **fil- och lagringstjänster**, och klicka sedan på **diskar**.</span><span class="sxs-lookup"><span data-stu-id="6fcc2-160">In hello left pane of Server Manager, click **File and Storage Services**, and then click **Disks**.</span></span>
2. <span data-ttu-id="6fcc2-161">I hello innehållsrutan i hello **diskar** klickar du på **disk 2** (med hello **Partition** ställa in också**okänd**).</span><span class="sxs-lookup"><span data-stu-id="6fcc2-161">In hello contents pane, in hello **Disks** group, click **disk 2** (with hello **Partition** set too**Unknown**).</span></span>
3. <span data-ttu-id="6fcc2-162">Klicka på **uppgifter**, och klicka sedan på **ny volym**.</span><span class="sxs-lookup"><span data-stu-id="6fcc2-162">Click **Tasks**, and then click **New Volume**.</span></span>
4. <span data-ttu-id="6fcc2-163">Hello sidan innan du börjar av hello guiden Ny volym, klicka på **nästa**.</span><span class="sxs-lookup"><span data-stu-id="6fcc2-163">On hello Before you begin page of hello New Volume Wizard, click **Next**.</span></span>
5. <span data-ttu-id="6fcc2-164">Hello väljer hello server och disk-sidan, klicka på **Disk 2**, och klicka sedan på **nästa**.</span><span class="sxs-lookup"><span data-stu-id="6fcc2-164">On hello Select hello server and disk page, click **Disk 2**, and then click **Next**.</span></span> <span data-ttu-id="6fcc2-165">När du uppmanas, klickar du på **OK**.</span><span class="sxs-lookup"><span data-stu-id="6fcc2-165">When prompted, click **OK**.</span></span>
6. <span data-ttu-id="6fcc2-166">Klicka på hello ange hello storleken på hello volym sida, **nästa**.</span><span class="sxs-lookup"><span data-stu-id="6fcc2-166">On hello Specify hello size of hello volume page, click **Next**.</span></span>
7. <span data-ttu-id="6fcc2-167">På hello tilldela tooa enhet enhetsbeteckning eller mapp klickar du på **nästa**.</span><span class="sxs-lookup"><span data-stu-id="6fcc2-167">On hello Assign tooa drive letter or folder page, click **Next**.</span></span>
8. <span data-ttu-id="6fcc2-168">På hello Välj fil system inställningar klickar du på **nästa**.</span><span class="sxs-lookup"><span data-stu-id="6fcc2-168">On hello Select file system settings page, click **Next**.</span></span>
9. <span data-ttu-id="6fcc2-169">På hello bekräfta val klickar du på **skapa**.</span><span class="sxs-lookup"><span data-stu-id="6fcc2-169">On hello Confirm selections page, click **Create**.</span></span>
10. <span data-ttu-id="6fcc2-170">När du är klar klickar du på **Stäng**.</span><span class="sxs-lookup"><span data-stu-id="6fcc2-170">When complete, click **Close**.</span></span>

<span data-ttu-id="6fcc2-171">Konfigurera DC2 sedan som en replika-domänkontroller för hello corp.contoso.com domänen.</span><span class="sxs-lookup"><span data-stu-id="6fcc2-171">Next, configure DC2 as a replica domain controller for hello corp.contoso.com domain.</span></span> <span data-ttu-id="6fcc2-172">Du kan köra dessa kommandon från hello Windows PowerShell-Kommandotolken på DC2.</span><span class="sxs-lookup"><span data-stu-id="6fcc2-172">Run these commands from hello Windows PowerShell command prompt on DC2.</span></span>

    Install-WindowsFeature AD-Domain-Services -IncludeManagementTools
    Install-ADDSDomainController -Credential (Get-Credential CORP\User1) -DomainName "corp.contoso.com" -InstallDns:$true -DatabasePath "F:\NTDS" -LogPath "F:\Logs" -SysvolPath "F:\SYSVOL"

<span data-ttu-id="6fcc2-173">Observera att du tillfrågas toosupply både hello corp\användare1 lösenord och ett Directory återställningsläge för katalogtjänster (DSRM) lösenord och toorestart DC2.</span><span class="sxs-lookup"><span data-stu-id="6fcc2-173">Note that you are prompted toosupply both hello CORP\User1 password and a Directory Services Restore Mode (DSRM) password, and toorestart DC2.</span></span>

<span data-ttu-id="6fcc2-174">Nu när hello TestVNET virtuellt nätverk har en egen DNS-server (DC2), måste du konfigurera hello TestVNET virtuellt nätverk toouse DNS-servern.</span><span class="sxs-lookup"><span data-stu-id="6fcc2-174">Now that hello TestVNET virtual network has its own DNS server (DC2), you must configure hello TestVNET virtual network toouse this DNS server.</span></span>

1. <span data-ttu-id="6fcc2-175">Hello vänster i hello Azure-portalen, klicka hello virtuella nätverk ikonen och klickar sedan på **TestVNET**.</span><span class="sxs-lookup"><span data-stu-id="6fcc2-175">In hello left pane of hello Azure portal, click hello virtual networks icon, and then click **TestVNET**.</span></span>
2. <span data-ttu-id="6fcc2-176">På hello **inställningar** klickar du på **DNS-servrar**.</span><span class="sxs-lookup"><span data-stu-id="6fcc2-176">On hello **Settings** tab, click **DNS servers**.</span></span>
3. <span data-ttu-id="6fcc2-177">I **primär DNS-server**, typen **192.168.0.4** tooreplace 10.0.0.4.</span><span class="sxs-lookup"><span data-stu-id="6fcc2-177">In **Primary DNS server**, type **192.168.0.4** tooreplace 10.0.0.4.</span></span>
4. <span data-ttu-id="6fcc2-178">Klicka på **Spara**.</span><span class="sxs-lookup"><span data-stu-id="6fcc2-178">Click **Save**.</span></span>

<span data-ttu-id="6fcc2-179">Det här är din aktuella konfiguration.</span><span class="sxs-lookup"><span data-stu-id="6fcc2-179">This is your current configuration.</span></span> 

![](./media/ps-hybrid-cloud-test-env-sim/virtual-machines-windows-ps-hybrid-cloud-test-env-sim-ph4.png)

<span data-ttu-id="6fcc2-180">Molnmiljön simulerade hybrid är nu redo för testning.</span><span class="sxs-lookup"><span data-stu-id="6fcc2-180">Your simulated hybrid cloud environment is now ready for testing.</span></span>

## <a name="next-step"></a><span data-ttu-id="6fcc2-181">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="6fcc2-181">Next step</span></span>
* <span data-ttu-id="6fcc2-182">Konfigurera en [webbaserade driftsapplikationer](ps-hybrid-cloud-test-env-lob.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) i den här miljön.</span><span class="sxs-lookup"><span data-stu-id="6fcc2-182">Set up a [web-based, line of business application](ps-hybrid-cloud-test-env-lob.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) in this environment.</span></span>

