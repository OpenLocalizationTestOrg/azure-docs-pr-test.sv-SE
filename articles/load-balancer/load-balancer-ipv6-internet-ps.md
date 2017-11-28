---
title: "aaaCreate ett Azure Internetriktade belastningsutjämnare med IPv6 - PowerShell | Microsoft Docs"
description: "Lär dig hur toocreate Internet-riktade belastningsutjämnaren med IPv6 med hjälp av PowerShell för Resource Manager"
services: load-balancer
documentationcenter: na
author: kumudd
manager: timlt
tags: azure-resource-manager
keywords: "IPv6, azure belastningsutjämnare, dual stack, offentlig IP-adress, inbyggd ipv6, mobil, iot"
ms.assetid: d4c649e3-84ad-4343-8b6a-0e89f0b9e518
ms.service: load-balancer
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 01/23/2017
ms.author: kumud
ms.openlocfilehash: 6ebb108399b070e06dddc33b7a774481eb44d717
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-creating-an-internet-facing-load-balancer-with-ipv6-using-powershell-for-resource-manager"></a><span data-ttu-id="c999c-104">Komma igång med en Internetuppkopplad belastningsutjämnare med IPv6 med hjälp av PowerShell för Resource Manager</span><span class="sxs-lookup"><span data-stu-id="c999c-104">Get started creating an Internet facing load balancer with IPv6 using PowerShell for Resource Manager</span></span>

> [!div class="op_single_selector"]
> * [<span data-ttu-id="c999c-105">PowerShell</span><span class="sxs-lookup"><span data-stu-id="c999c-105">PowerShell</span></span>](load-balancer-ipv6-internet-ps.md)
> * [<span data-ttu-id="c999c-106">Azure CLI</span><span class="sxs-lookup"><span data-stu-id="c999c-106">Azure CLI</span></span>](load-balancer-ipv6-internet-cli.md)
> * [<span data-ttu-id="c999c-107">Mall</span><span class="sxs-lookup"><span data-stu-id="c999c-107">Template</span></span>](load-balancer-ipv6-internet-template.md)

<span data-ttu-id="c999c-108">En Azure belastningsutjämnare är en Layer 4-belastningsutjämnare (TCP, UDP).</span><span class="sxs-lookup"><span data-stu-id="c999c-108">An Azure load balancer is a Layer-4 (TCP, UDP) load balancer.</span></span> <span data-ttu-id="c999c-109">hello belastningsutjämnare ger hög tillgänglighet genom att distribuera inkommande trafik mellan felfri tjänstinstanser i molntjänster eller virtuella datorer i en belastningen belastningsutjämnaren.</span><span class="sxs-lookup"><span data-stu-id="c999c-109">hello load balancer provides high availability by distributing incoming traffic among healthy service instances in cloud services or virtual machines in a load balancer set.</span></span> <span data-ttu-id="c999c-110">Azure belastningsutjämnare kan även presentera dessa tjänster på flera portar, flera IP-adresser eller både och.</span><span class="sxs-lookup"><span data-stu-id="c999c-110">Azure Load Balancer can also present those services on multiple ports, multiple IP addresses, or both.</span></span>

## <a name="example-deployment-scenario"></a><span data-ttu-id="c999c-111">Exempelscenario för distribution</span><span class="sxs-lookup"><span data-stu-id="c999c-111">Example deployment scenario</span></span>

<span data-ttu-id="c999c-112">hello illustrerar följande diagram hello belastningsutjämning lösning som distribueras i den här artikeln.</span><span class="sxs-lookup"><span data-stu-id="c999c-112">hello following diagram illustrates hello load balancing solution being deployed in this article.</span></span>

![Belastningsutjämningsscenario](./media/load-balancer-ipv6-internet-ps/lb-ipv6-scenario.png)

<span data-ttu-id="c999c-114">I det här scenariot skapar du hello följande Azure-resurser:</span><span class="sxs-lookup"><span data-stu-id="c999c-114">In this scenario you will create hello following Azure resources:</span></span>

* <span data-ttu-id="c999c-115">en Internetriktade belastningsutjämnare med en IPv4 och IPv6-offentlig IP-adress</span><span class="sxs-lookup"><span data-stu-id="c999c-115">an Internet-facing Load Balancer with an IPv4 and an IPv6 Public IP address</span></span>
* <span data-ttu-id="c999c-116">två läsa in belastningsutjämning regler toomap hello offentliga VIP toohello privata slutpunkter</span><span class="sxs-lookup"><span data-stu-id="c999c-116">two load balancing rules toomap hello public VIPs toohello private endpoints</span></span>
* <span data-ttu-id="c999c-117">en Tillgänglighetsuppsättning toothat innehåller hello två virtuella datorer</span><span class="sxs-lookup"><span data-stu-id="c999c-117">an Availability Set toothat contains hello two VMs</span></span>
* <span data-ttu-id="c999c-118">två virtuella datorer (VM)</span><span class="sxs-lookup"><span data-stu-id="c999c-118">two virtual machines (VMs)</span></span>
* <span data-ttu-id="c999c-119">ett virtuellt nätverksgränssnitt för varje virtuell dator med både IPv4 och IPv6-adresser som tilldelats</span><span class="sxs-lookup"><span data-stu-id="c999c-119">a virtual network interface for each VM with both IPv4 and IPv6 addresses assigned</span></span>

## <a name="deploying-hello-solution-using-hello-azure-powershell"></a><span data-ttu-id="c999c-120">Distribuera hello-lösning med hjälp av hello Azure PowerShell</span><span class="sxs-lookup"><span data-stu-id="c999c-120">Deploying hello solution using hello Azure PowerShell</span></span>

<span data-ttu-id="c999c-121">hello följande steg visar hur toocreate Internet-riktade belastningsutjämnaren med Azure Resource Manager med PowerShell.</span><span class="sxs-lookup"><span data-stu-id="c999c-121">hello following steps show how toocreate an Internet facing load balancer using Azure Resource Manager with PowerShell.</span></span> <span data-ttu-id="c999c-122">Med Azure Resource Manager varje resurs har skapats och konfigurerats individuellt sedan sätta ihop toocreate en resurs.</span><span class="sxs-lookup"><span data-stu-id="c999c-122">With Azure Resource Manager, each resource is created and configured individually, then put together toocreate a resource.</span></span>

<span data-ttu-id="c999c-123">toodeploy en belastningsutjämnare som du skapar och konfigurerar hello följande objekt:</span><span class="sxs-lookup"><span data-stu-id="c999c-123">toodeploy a load balancer, you create and configure hello following objects:</span></span>

* <span data-ttu-id="c999c-124">IP-konfiguration på klientsidan – innehåller offentliga IP-adresser för inkommande nätverkstrafik.</span><span class="sxs-lookup"><span data-stu-id="c999c-124">Front-end IP configuration - contains public IP addresses for incoming network traffic.</span></span>
* <span data-ttu-id="c999c-125">Backend-adresspool - innehåller nätverksgränssnitt (NIC) för virtuella datorer hello tooreceive nätverkstrafik från hello belastningsutjämnaren.</span><span class="sxs-lookup"><span data-stu-id="c999c-125">Back-end address pool - contains network interfaces (NICs) for hello virtual machines tooreceive network traffic from hello load balancer.</span></span>
* <span data-ttu-id="c999c-126">Belastningsutjämning regler - innehåller regler för mappning av en offentlig port på hello belastningen belastningsutjämnaren tooport i hello backend-adresspool.</span><span class="sxs-lookup"><span data-stu-id="c999c-126">Load balancing rules - contains rules mapping a public port on hello load balancer tooport in hello back-end address pool.</span></span>
* <span data-ttu-id="c999c-127">Inkommande NAT-regler - innehåller regler för mappning av en offentlig port på hello belastningen belastningsutjämnaren tooa port för en specifik virtuell dator i hello backend-adresspool.</span><span class="sxs-lookup"><span data-stu-id="c999c-127">Inbound NAT rules - contains rules mapping a public port on hello load balancer tooa port for a specific virtual machine in hello back-end address pool.</span></span>
* <span data-ttu-id="c999c-128">Avsökningar - innehåller hälsa-avsökningar som används toocheck tillgängligheten för virtuella datorer instanser i hello backend-adresspool.</span><span class="sxs-lookup"><span data-stu-id="c999c-128">Probes - contains health probes used toocheck availability of virtual machines instances in hello back-end address pool.</span></span>

<span data-ttu-id="c999c-129">Mer information finns i [Azure Resource Manager-stöd för belastningsutjämnare](load-balancer-arm.md).</span><span class="sxs-lookup"><span data-stu-id="c999c-129">For more information, see [Azure Resource Manager support for Load Balancer](load-balancer-arm.md).</span></span>

## <a name="set-up-powershell-toouse-resource-manager"></a><span data-ttu-id="c999c-130">Konfigurera PowerShell toouse Resource Manager</span><span class="sxs-lookup"><span data-stu-id="c999c-130">Set up PowerShell toouse Resource Manager</span></span>

<span data-ttu-id="c999c-131">Kontrollera att du har hello senaste produktionsversionen av hello Azure Resource Manager-modul för PowerShell.</span><span class="sxs-lookup"><span data-stu-id="c999c-131">Make sure you have hello latest production version of hello Azure Resource Manager module for PowerShell.</span></span>

1. <span data-ttu-id="c999c-132">Logga in på Azure</span><span class="sxs-lookup"><span data-stu-id="c999c-132">Sign into Azure</span></span>

    ```powershell
    Login-AzureRmAccount
    ```

    <span data-ttu-id="c999c-133">Ange dina autentiseringsuppgifter när du uppmanas att göra det.</span><span class="sxs-lookup"><span data-stu-id="c999c-133">Enter your credentials when prompted.</span></span>

2. <span data-ttu-id="c999c-134">Kontrollera hello prenumerationer för hello-konto</span><span class="sxs-lookup"><span data-stu-id="c999c-134">Check hello subscriptions for hello account</span></span>

    ```powershell
    Get-AzureRmSubscription
    ```

3. <span data-ttu-id="c999c-135">Välj vilka av dina Azure-prenumerationer toouse.</span><span class="sxs-lookup"><span data-stu-id="c999c-135">Choose which of your Azure subscriptions toouse.</span></span>

    ```powershell
    Select-AzureRmSubscription -SubscriptionId 'GUID of subscription'
    ```

4. <span data-ttu-id="c999c-136">Skapa en resursgrupp (hoppa över detta steg om du använder en befintlig resursgrupp)</span><span class="sxs-lookup"><span data-stu-id="c999c-136">Create a resource group (skip this step if using an existing resource group)</span></span>

    ```powershell
    New-AzureRmResourceGroup -Name NRP-RG -location "West US"
    ```

## <a name="create-a-virtual-network-and-a-public-ip-address-for-hello-front-end-ip-pool"></a><span data-ttu-id="c999c-137">Skapa ett virtuellt nätverk och en offentlig IP-adress för hello frontend IP-adresspool</span><span class="sxs-lookup"><span data-stu-id="c999c-137">Create a virtual network and a public IP address for hello front-end IP pool</span></span>

1. <span data-ttu-id="c999c-138">Skapa ett virtuellt nätverk med ett undernät.</span><span class="sxs-lookup"><span data-stu-id="c999c-138">Create a virtual network with a subnet.</span></span>

    ```powershell
    $backendSubnet = New-AzureRmVirtualNetworkSubnetConfig -Name LB-Subnet-BE -AddressPrefix 10.0.2.0/24
    $vnet = New-AzureRmvirtualNetwork -Name VNet -ResourceGroupName NRP-RG -Location 'West US' -AddressPrefix 10.0.0.0/16 -Subnet $backendSubnet
    ```

2. <span data-ttu-id="c999c-139">Skapa Azure offentlig IP-adress (PIP) resurser för hello frontend IP-adresspool.</span><span class="sxs-lookup"><span data-stu-id="c999c-139">Create Azure Public IP address (PIP) resources for hello front-end IP address pool.</span></span>

    ```powershell
    $publicIPv4 = New-AzureRmPublicIpAddress -Name 'pub-ipv4' -ResourceGroupName NRP-RG -Location 'West US' -AllocationMethod Static -IpAddressVersion IPv4 -DomainNameLabel lbnrpipv4
    $publicIPv6 = New-AzureRmPublicIpAddress -Name 'pub-ipv6' -ResourceGroupName NRP-RG -Location 'West US' -AllocationMethod Dynamic -IpAddressVersion IPv6 -DomainNameLabel lbnrpipv6
    ```

    > [!IMPORTANT]
    > <span data-ttu-id="c999c-140">hello belastningsutjämnare använder hello domänetikett hello offentlig IP-adress som prefix för dess FQDN.</span><span class="sxs-lookup"><span data-stu-id="c999c-140">hello load balancer uses hello domain label of hello public IP as prefix for its FQDN.</span></span> <span data-ttu-id="c999c-141">I det här exemplet hello FQDN är *lbnrpipv4.westus.cloudapp.azure.com* och *lbnrpipv6.westus.cloudapp.azure.com*.</span><span class="sxs-lookup"><span data-stu-id="c999c-141">In this example, hello FQDNs are *lbnrpipv4.westus.cloudapp.azure.com* and *lbnrpipv6.westus.cloudapp.azure.com*.</span></span>

## <a name="create-a-front-end-ip-configurations-and-a-back-end-address-pool"></a><span data-ttu-id="c999c-142">Skapa en frontend-IP-konfigurationer och en backend-adresspool</span><span class="sxs-lookup"><span data-stu-id="c999c-142">Create a Front-End IP configurations and a Back-End Address Pool</span></span>

1. <span data-ttu-id="c999c-143">Skapa frontend-adresskonfiguration som använder hello offentliga IP-adresser som du skapade.</span><span class="sxs-lookup"><span data-stu-id="c999c-143">Create front-end address configuration that uses hello Public IP addresses you created.</span></span>

    ```powershell
    $FEIPConfigv4 = New-AzureRmLoadBalancerFrontendIpConfig -Name "LB-Frontendv4" -PublicIpAddress $publicIPv4
    $FEIPConfigv6 = New-AzureRmLoadBalancerFrontendIpConfig -Name "LB-Frontendv6" -PublicIpAddress $publicIPv6
    ```

2. <span data-ttu-id="c999c-144">Skapa backend-adresspool.</span><span class="sxs-lookup"><span data-stu-id="c999c-144">Create back-end address pools.</span></span>

    ```powershell
    $backendpoolipv4 = New-AzureRmLoadBalancerBackendAddressPoolConfig -Name "BackendPoolIPv4"
    $backendpoolipv6 = New-AzureRmLoadBalancerBackendAddressPoolConfig -Name "BackendPoolIPv6"
    ```

## <a name="create-lb-rules-nat-rules-a-probe-and-a-load-balancer"></a><span data-ttu-id="c999c-145">Skapa LB-regler, NAT-regler, en avsökning och belastningsutjämning</span><span class="sxs-lookup"><span data-stu-id="c999c-145">Create LB rules, NAT rules, a probe, and a load balancer</span></span>

<span data-ttu-id="c999c-146">Det här exemplet skapar hello följande objekt:</span><span class="sxs-lookup"><span data-stu-id="c999c-146">This example creates hello following items:</span></span>

* <span data-ttu-id="c999c-147">en NAT-regel tootranslate all inkommande trafik på port 443 tooport 4443</span><span class="sxs-lookup"><span data-stu-id="c999c-147">a NAT rule tootranslate all incoming traffic on port 443 tooport 4443</span></span>
* <span data-ttu-id="c999c-148">en belastningen belastningsutjämnaren regeln toobalance all inkommande trafik på port 80 tooport 80 på hello adresser i hello backend-adresspool.</span><span class="sxs-lookup"><span data-stu-id="c999c-148">a load balancer rule toobalance all incoming traffic on port 80 tooport 80 on hello addresses in hello back-end pool.</span></span>
* <span data-ttu-id="c999c-149">en belastningen belastningsutjämnaren regeln tooallow RDP-anslutning toohello virtuella datorer på port 3389.</span><span class="sxs-lookup"><span data-stu-id="c999c-149">a load balancer rule tooallow RDP connection toohello VMs on port 3389.</span></span>
* <span data-ttu-id="c999c-150">en avsökning regeln toocheck hello hälsostatus på en sida med namnet *HealthProbe.aspx* eller en tjänst på port 8080</span><span class="sxs-lookup"><span data-stu-id="c999c-150">a probe rule toocheck hello health status on a page named *HealthProbe.aspx* or a service on port 8080</span></span>
* <span data-ttu-id="c999c-151">en belastningsutjämnare som använder dessa objekt</span><span class="sxs-lookup"><span data-stu-id="c999c-151">a load balancer that uses all these objects</span></span>

1. <span data-ttu-id="c999c-152">Skapa hello NAT-regler.</span><span class="sxs-lookup"><span data-stu-id="c999c-152">Create hello NAT rules.</span></span>

    ```powershell
    $inboundNATRule1v4 = New-AzureRmLoadBalancerInboundNatRuleConfig -Name "NicNatRulev4" -FrontendIpConfiguration $FEIPConfigv4 -Protocol TCP -FrontendPort 443 -BackendPort 4443
    $inboundNATRule1v6 = New-AzureRmLoadBalancerInboundNatRuleConfig -Name "NicNatRulev6" -FrontendIpConfiguration $FEIPConfigv6 -Protocol TCP -FrontendPort 443 -BackendPort 4443
    ```

2. <span data-ttu-id="c999c-153">Skapa en hälsoavsökning.</span><span class="sxs-lookup"><span data-stu-id="c999c-153">Create a health probe.</span></span> <span data-ttu-id="c999c-154">Det finns två sätt tooconfigure en avsökning:</span><span class="sxs-lookup"><span data-stu-id="c999c-154">There are two ways tooconfigure a probe:</span></span>

    <span data-ttu-id="c999c-155">HTTP-avsökning</span><span class="sxs-lookup"><span data-stu-id="c999c-155">HTTP probe</span></span>

    ```powershell
    $healthProbe = New-AzureRmLoadBalancerProbeConfig -Name 'HealthProbe-v4v6' -RequestPath 'HealthProbe.aspx' -Protocol http -Port 80 -IntervalInSeconds 15 -ProbeCount 2
    ```

    <span data-ttu-id="c999c-156">eller TCP-avsökning</span><span class="sxs-lookup"><span data-stu-id="c999c-156">or TCP probe</span></span>

    ```powershell
    $healthProbe = New-AzureRmLoadBalancerProbeConfig -Name 'HealthProbe-v4v6' -Protocol Tcp -Port 8080 -IntervalInSeconds 15 -ProbeCount 2
    $RDPprobe = New-AzureRmLoadBalancerProbeConfig -Name 'RDPprobe' -Protocol Tcp -Port 3389 -IntervalInSeconds 15 -ProbeCount 2
    ```

    <span data-ttu-id="c999c-157">I det här exemplet ska vi ska toouse hello TCP-avsökningar.</span><span class="sxs-lookup"><span data-stu-id="c999c-157">For this example, we are going toouse hello TCP probes.</span></span>

3. <span data-ttu-id="c999c-158">Skapa en belastningsutjämningsregel.</span><span class="sxs-lookup"><span data-stu-id="c999c-158">Create a load balancer rule.</span></span>

    ```powershell
    $lbrule1v4 = New-AzureRmLoadBalancerRuleConfig -Name "HTTPv4" -FrontendIpConfiguration $FEIPConfigv4 -BackendAddressPool $backendpoolipv4 -Probe $healthProbe -Protocol Tcp -FrontendPort 80 -BackendPort 8080
    $lbrule1v6 = New-AzureRmLoadBalancerRuleConfig -Name "HTTPv6" -FrontendIpConfiguration $FEIPConfigv6 -BackendAddressPool $backendpoolipv6 -Probe $healthProbe -Protocol Tcp -FrontendPort 80 -BackendPort 8080
    $RDPrule = New-AzureRmLoadBalancerRuleConfig -Name "RDPrule" -FrontendIpConfiguration $FEIPConfigv4 -BackendAddressPool $backendpoolipv4 -Probe $RDPprobe -Protocol Tcp -FrontendPort 3389 -BackendPort 3389
    ```

4. <span data-ttu-id="c999c-159">Skapa hello belastningsutjämnare använder hello som tidigare har skapat objekt.</span><span class="sxs-lookup"><span data-stu-id="c999c-159">Create hello load balancer using hello previously created objects.</span></span>

    ```powershell
    $NRPLB = New-AzureRmLoadBalancer -ResourceGroupName NRP-RG -Name 'myNrpIPv6LB' -Location 'West US' -FrontendIpConfiguration $FEIPConfigv4,$FEIPConfigv6 -InboundNatRule $inboundNATRule1v6,$inboundNATRule1v4 -BackendAddressPool $backendpoolipv4,$backendpoolipv6 -Probe $healthProbe,$RDPprobe -LoadBalancingRule $lbrule1v4,$lbrule1v6,$RDPrule
    ```

## <a name="create-nics-for-hello-back-end-vms"></a><span data-ttu-id="c999c-160">Skapa nätverkskort för hello backend-virtuella datorer</span><span class="sxs-lookup"><span data-stu-id="c999c-160">Create NICs for hello back-end VMs</span></span>

1. <span data-ttu-id="c999c-161">Hämta hello virtuella nätverk och virtuella undernät i nätverket, där hello nätverkskort måste toobe skapas.</span><span class="sxs-lookup"><span data-stu-id="c999c-161">Get hello Virtual Network and Virtual Network Subnet, where hello NICs need toobe created.</span></span>

    ```powershell
    $vnet = Get-AzureRmVirtualNetwork -Name VNet -ResourceGroupName NRP-RG
    $backendSubnet = Get-AzureRmVirtualNetworkSubnetConfig -Name LB-Subnet-BE -VirtualNetwork $vnet
    ```

2. <span data-ttu-id="c999c-162">Skapa IP-konfigurationer och nätverkskort för hello virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="c999c-162">Create IP configurations and NICs for hello VMs.</span></span>

    ```powershell
    $nic1IPv4 = New-AzureRmNetworkInterfaceIpConfig -Name "IPv4IPConfig" -PrivateIpAddressVersion "IPv4" -Subnet $backendSubnet -LoadBalancerBackendAddressPool $backendpoolipv4 -LoadBalancerInboundNatRule $inboundNATRule1v4
    $nic1IPv6 = New-AzureRmNetworkInterfaceIpConfig -Name "IPv6IPConfig" -PrivateIpAddressVersion "IPv6" -LoadBalancerBackendAddressPool $backendpoolipv6 -LoadBalancerInboundNatRule $inboundNATRule1v6
    $nic1 = New-AzureRmNetworkInterface -Name 'myNrpIPv6Nic0' -IpConfiguration $nic1IPv4,$nic1IPv6 -ResourceGroupName NRP-RG -Location 'West US'

    $nic2IPv4 = New-AzureRmNetworkInterfaceIpConfig -Name "IPv4IPConfig" -PrivateIpAddressVersion "IPv4" -Subnet $backendSubnet -LoadBalancerBackendAddressPool $backendpoolipv4
    $nic2IPv6 = New-AzureRmNetworkInterfaceIpConfig -Name "IPv6IPConfig" -PrivateIpAddressVersion "IPv6" -LoadBalancerBackendAddressPool $backendpoolipv6
    $nic2 = New-AzureRmNetworkInterface -Name 'myNrpIPv6Nic1' -IpConfiguration $nic2IPv4,$nic2IPv6 -ResourceGroupName NRP-RG -Location 'West US'
    ```

## <a name="create-virtual-machines-and-assign-hello-newly-created-nics"></a><span data-ttu-id="c999c-163">Skapa virtuella datorer och tilldela hello nyskapad nätverkskort</span><span class="sxs-lookup"><span data-stu-id="c999c-163">Create virtual machines and assign hello newly created NICs</span></span>

<span data-ttu-id="c999c-164">Mer information om hur du skapar en virtuell dator finns [skapa och förkonfigurera en virtuell Windows-dator med Resource Manager och Azure PowerShell](../virtual-machines/virtual-machines-windows-ps-create.md?toc=%2fazure%2fload-balancer%2ftoc.json)</span><span class="sxs-lookup"><span data-stu-id="c999c-164">For more information about creating a VM, see [Create and preconfigure a Windows Virtual Machine with Resource Manager and Azure PowerShell](../virtual-machines/virtual-machines-windows-ps-create.md?toc=%2fazure%2fload-balancer%2ftoc.json)</span></span>

1. <span data-ttu-id="c999c-165">Skapa en Tillgänglighetsuppsättning och lagring</span><span class="sxs-lookup"><span data-stu-id="c999c-165">Create an Availability Set and Storage account</span></span>

    ```powershell
    New-AzureRmAvailabilitySet -Name 'myNrpIPv6AvSet' -ResourceGroupName NRP-RG -location 'West US'
    $availabilitySet = Get-AzureRmAvailabilitySet -Name 'myNrpIPv6AvSet' -ResourceGroupName NRP-RG
    New-AzureRmStorageAccount -ResourceGroupName NRP-RG -Name 'mynrpipv6stacct' -Location 'West US' -SkuName "Standard_LRS"
    $CreatedStorageAccount = Get-AzureRmStorageAccount -ResourceGroupName NRP-RG -Name 'mynrpipv6stacct'
    ```

2. <span data-ttu-id="c999c-166">Skapa varje virtuell dator och tilldela hello tidigare skapade nätverkskort</span><span class="sxs-lookup"><span data-stu-id="c999c-166">Create each VM and assign hello previous created NICs</span></span>

    ```powershell
    $mySecureCredentials= Get-Credential -Message "Type hello username and password of hello local administrator account."

    $vm1 = New-AzureRmVMConfig -VMName 'myNrpIPv6VM0' -VMSize 'Standard_G1' -AvailabilitySetId $availabilitySet.Id
    $vm1 = Set-AzureRmVMOperatingSystem -VM $vm1 -Windows -ComputerName 'myNrpIPv6VM0' -Credential $mySecureCredentials -ProvisionVMAgent -EnableAutoUpdate
    $vm1 = Set-AzureRmVMSourceImage -VM $vm1 -PublisherName MicrosoftWindowsServer -Offer WindowsServer -Skus 2012-R2-Datacenter -Version "latest"
    $vm1 = Add-AzureRmVMNetworkInterface -VM $vm1 -Id $nic1.Id -Primary
    $osDisk1Uri = $CreatedStorageAccount.PrimaryEndpoints.Blob.ToString() + "vhds/myNrpIPv6VM0osdisk.vhd"
    $vm1 = Set-AzureRmVMOSDisk -VM $vm1 -Name 'myNrpIPv6VM0osdisk' -VhdUri $osDisk1Uri -CreateOption FromImage
    New-AzureRmVM -ResourceGroupName NRP-RG -Location 'West US' -VM $vm1

    $vm2 = New-AzureRmVMConfig -VMName 'myNrpIPv6VM1' -VMSize 'Standard_G1' -AvailabilitySetId $availabilitySet.Id
    $vm2 = Set-AzureRmVMOperatingSystem -VM $vm2 -Windows -ComputerName 'myNrpIPv6VM1' -Credential $mySecureCredentials -ProvisionVMAgent -EnableAutoUpdate
    $vm2 = Set-AzureRmVMSourceImage -VM $vm2 -PublisherName MicrosoftWindowsServer -Offer WindowsServer -Skus 2012-R2-Datacenter -Version "latest"
    $vm2 = Add-AzureRmVMNetworkInterface -VM $vm2 -Id $nic2.Id -Primary
    $osDisk2Uri = $CreatedStorageAccount.PrimaryEndpoints.Blob.ToString() + "vhds/myNrpIPv6VM1osdisk.vhd"
    $vm2 = Set-AzureRmVMOSDisk -VM $vm2 -Name 'myNrpIPv6VM1osdisk' -VhdUri $osDisk2Uri -CreateOption FromImage
    New-AzureRmVM -ResourceGroupName NRP-RG -Location 'West US' -VM $vm2
    ```

## <a name="next-steps"></a><span data-ttu-id="c999c-167">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="c999c-167">Next steps</span></span>

[<span data-ttu-id="c999c-168">Komma igång med att konfigurera en intern belastningsutjämnare</span><span class="sxs-lookup"><span data-stu-id="c999c-168">Get started configuring an internal load balancer</span></span>](load-balancer-get-started-ilb-arm-ps.md)

[<span data-ttu-id="c999c-169">Konfigurera ett distributionsläge för belastningsutjämnare</span><span class="sxs-lookup"><span data-stu-id="c999c-169">Configure a load balancer distribution mode</span></span>](load-balancer-distribution-mode.md)

[<span data-ttu-id="c999c-170">Konfigurera timeout-inställningar för inaktiv TCP för en belastningsutjämnare</span><span class="sxs-lookup"><span data-stu-id="c999c-170">Configure idle TCP timeout settings for your load balancer</span></span>](load-balancer-tcp-idle-timeout.md)
