---
title: "aaaCreate ett Azure Internetriktade belastningsutjämnare - PowerShell | Microsoft Docs"
description: "Lär dig hur toocreate en Internetriktade belastningsutjämnare i Resource Manager med hjälp av PowerShell"
services: load-balancer
documentationcenter: na
author: kumudd
manager: timlt
tags: azure-resource-manager
ms.assetid: 8257f548-7019-417f-b15f-d004a1eec826
ms.service: load-balancer
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 01/23/2017
ms.author: kumud
ms.openlocfilehash: e4e0e5271bc83c23fc62c0910e784c57d2b30065
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <span data-ttu-id="73945-103"><a name="get-started"></a>Skapa en Internetuppkopplad belastningsutjämnare i Resource Manager med hjälp av PowerShell</span><span class="sxs-lookup"><span data-stu-id="73945-103"><a name="get-started"></a>Creating an Internet-facing load balancer in Resource Manager by using PowerShell</span></span>

> [!div class="op_single_selector"]
> * [<span data-ttu-id="73945-104">Portal</span><span class="sxs-lookup"><span data-stu-id="73945-104">Portal</span></span>](../load-balancer/load-balancer-get-started-internet-portal.md)
> * [<span data-ttu-id="73945-105">PowerShell</span><span class="sxs-lookup"><span data-stu-id="73945-105">PowerShell</span></span>](../load-balancer/load-balancer-get-started-internet-arm-ps.md)
> * [<span data-ttu-id="73945-106">Azure CLI</span><span class="sxs-lookup"><span data-stu-id="73945-106">Azure CLI</span></span>](../load-balancer/load-balancer-get-started-internet-arm-cli.md)
> * [<span data-ttu-id="73945-107">Mall</span><span class="sxs-lookup"><span data-stu-id="73945-107">Template</span></span>](../load-balancer/load-balancer-get-started-internet-arm-template.md)

[!INCLUDE [load-balancer-get-started-internet-intro-include.md](../../includes/load-balancer-get-started-internet-intro-include.md)]

[!INCLUDE [azure-arm-classic-important-include](../../includes/azure-arm-classic-important-include.md)]

<span data-ttu-id="73945-108">Den här artikeln beskriver hello Resource Manager-modellen.</span><span class="sxs-lookup"><span data-stu-id="73945-108">This article covers hello Resource Manager deployment model.</span></span> <span data-ttu-id="73945-109">Du kan också [Lär dig hur toocreate en Internetriktade belastningsutjämnare med hjälp av hello klassiska distributionsmodellen](load-balancer-get-started-internet-classic-cli.md).</span><span class="sxs-lookup"><span data-stu-id="73945-109">You can also [learn how toocreate an Internet-facing load balancer by using hello classic deployment model](load-balancer-get-started-internet-classic-cli.md).</span></span>

[!INCLUDE [load-balancer-get-started-internet-scenario-include.md](../../includes/load-balancer-get-started-internet-scenario-include.md)]

## <a name="deploying-hello-solution-by-using-azure-powershell"></a><span data-ttu-id="73945-110">Distribuera hello-lösning med hjälp av Azure PowerShell</span><span class="sxs-lookup"><span data-stu-id="73945-110">Deploying hello solution by using Azure PowerShell</span></span>

<span data-ttu-id="73945-111">hello följande procedurer beskrivs hur toocreate en Internetriktade belastningsutjämnare med hjälp av Azure Resource Manager med PowerShell.</span><span class="sxs-lookup"><span data-stu-id="73945-111">hello following procedures explain how toocreate an Internet-facing load balancer by using Azure Resource Manager with PowerShell.</span></span> <span data-ttu-id="73945-112">Med Azure Resource Manager varje resurs har skapats och konfigurerats individuellt och sedan sätta ihop toocreate belastningsutjämning.</span><span class="sxs-lookup"><span data-stu-id="73945-112">With Azure Resource Manager, each resource is created and configured individually, and then put together toocreate a load balancer.</span></span>

<span data-ttu-id="73945-113">Du måste skapa och konfigurera hello följande objekt toodeploy en belastningsutjämnare:</span><span class="sxs-lookup"><span data-stu-id="73945-113">You must create and configure hello following objects toodeploy a load balancer:</span></span>

* <span data-ttu-id="73945-114">IP-konfiguration på klientsidan: innehåller offentliga IP-adresser för inkommande nätverkstrafik.</span><span class="sxs-lookup"><span data-stu-id="73945-114">Front-end IP configuration: contains public IP (PIP) addresses for incoming network traffic.</span></span>
* <span data-ttu-id="73945-115">Backend-adresspool: innehåller nätverksgränssnitt (NIC) för virtuella datorer hello tooreceive nätverkstrafik från hello belastningsutjämnaren.</span><span class="sxs-lookup"><span data-stu-id="73945-115">Back-end address pool: contains network interfaces (NICs) for hello virtual machines tooreceive network traffic from hello load balancer.</span></span>
* <span data-ttu-id="73945-116">Regler för belastningsutjämning: innehåller regler som mappar en offentlig port på hello belastningen belastningsutjämnaren tooa port i hello backend-adresspool.</span><span class="sxs-lookup"><span data-stu-id="73945-116">Load-balancing rules: contains rules that map a public port on hello load balancer tooa port in hello back-end address pool.</span></span>
* <span data-ttu-id="73945-117">Inkommande NAT-regler: innehåller regler som mappar en offentlig port på hello belastningen belastningsutjämnaren tooa port för en specifik virtuell dator i hello backend-adresspool.</span><span class="sxs-lookup"><span data-stu-id="73945-117">Inbound NAT rules: contains rules that map a public port on hello load balancer tooa port for a specific virtual machine in hello back-end address pool.</span></span>
* <span data-ttu-id="73945-118">Avsökningar: innehåller hälsa-avsökningar som används toocheck tillgängligheten för en virtuell datorinstans i hello backend-adresspool.</span><span class="sxs-lookup"><span data-stu-id="73945-118">Probes: contains health probes used toocheck availability of virtual machine instances in hello back-end address pool.</span></span>

<span data-ttu-id="73945-119">Mer information finns i [Azure Resource Manager-stöd för belastningsutjämnare](load-balancer-arm.md).</span><span class="sxs-lookup"><span data-stu-id="73945-119">For more information, see [Azure Resource Manager support for Load Balancer](load-balancer-arm.md).</span></span>

## <a name="set-up-powershell-toouse-resource-manager"></a><span data-ttu-id="73945-120">Konfigurera PowerShell toouse Resource Manager</span><span class="sxs-lookup"><span data-stu-id="73945-120">Set up PowerShell toouse Resource Manager</span></span>

<span data-ttu-id="73945-121">Kontrollera att du har hello senaste produktionsversionen av hello Azure Resource Manager-modul för PowerShell:</span><span class="sxs-lookup"><span data-stu-id="73945-121">Make sure you have hello latest production version of hello Azure Resource Manager module for PowerShell:</span></span>

1. <span data-ttu-id="73945-122">Logga in tooAzure.</span><span class="sxs-lookup"><span data-stu-id="73945-122">Sign in tooAzure.</span></span>

    ```powershell
    Login-AzureRmAccount
    ```

    <span data-ttu-id="73945-123">Ange dina autentiseringsuppgifter när du uppmanas att göra det.</span><span class="sxs-lookup"><span data-stu-id="73945-123">Enter your credentials when prompted.</span></span>

2. <span data-ttu-id="73945-124">Kontrollera hello prenumerationer för hello-kontot.</span><span class="sxs-lookup"><span data-stu-id="73945-124">Check hello subscriptions for hello account.</span></span>

    ```powershell
    Get-AzureRmSubscription
    ```

3. <span data-ttu-id="73945-125">Välj vilka av dina Azure-prenumerationer toouse.</span><span class="sxs-lookup"><span data-stu-id="73945-125">Choose which of your Azure subscriptions toouse.</span></span>

    ```powershell
    Select-AzureRmSubscription -SubscriptionId 'GUID of subscription'
    ```

4. <span data-ttu-id="73945-126">Skapa en resursgrupp.</span><span class="sxs-lookup"><span data-stu-id="73945-126">Create a resource group.</span></span> <span data-ttu-id="73945-127">(Hoppa över det här steget om du använder en befintlig resursgrupp.)</span><span class="sxs-lookup"><span data-stu-id="73945-127">(Skip this step if you're using an existing resource group.)</span></span>

    ```powershell
    New-AzureRmResourceGroup -Name NRP-RG -location "West US"
    ```

## <a name="create-a-virtual-network-and-a-public-ip-address-for-hello-front-end-ip-pool"></a><span data-ttu-id="73945-128">Skapa ett virtuellt nätverk och en offentlig IP-adress för hello frontend IP-adresspool</span><span class="sxs-lookup"><span data-stu-id="73945-128">Create a virtual network and a public IP address for hello front-end IP pool</span></span>

1. <span data-ttu-id="73945-129">Skapa ett undernät och ett virtuellt nätverk.</span><span class="sxs-lookup"><span data-stu-id="73945-129">Create a subnet and a virtual network.</span></span>

    ```powershell
    $backendSubnet = New-AzureRmVirtualNetworkSubnetConfig -Name LB-Subnet-BE -AddressPrefix 10.0.2.0/24
    New-AzureRmvirtualNetwork -Name NRPVNet -ResourceGroupName NRP-RG -Location 'West US' -AddressPrefix 10.0.0.0/16 -Subnet $backendSubnet
    ```

2. <span data-ttu-id="73945-130">Skapa en Azure offentliga IP-adressresurs med namnet **PublicIP**, toobe som används av en frontend IP-adresspool med hello DNS-namnet **loadbalancernrp.westus.cloudapp.azure.com**. hello följande kommando använder hello av statiska allokeringstypen.</span><span class="sxs-lookup"><span data-stu-id="73945-130">Create an Azure public IP address resource, named **PublicIP**, toobe used by a front-end IP pool with hello DNS name **loadbalancernrp.westus.cloudapp.azure.com**. hello following command uses hello static allocation type.</span></span>

    ```powershell
    $publicIP = New-AzureRmPublicIpAddress -Name PublicIp -ResourceGroupName NRP-RG -Location 'West US' -AllocationMethod Static -DomainNameLabel loadbalancernrp
    ```

   > [!IMPORTANT]
   > <span data-ttu-id="73945-131">hello belastningsutjämnare använder hello domänetikett hello offentlig IP-adress som ett prefix för dess FQDN.</span><span class="sxs-lookup"><span data-stu-id="73945-131">hello load balancer uses hello domain label of hello public IP as a prefix for its FQDN.</span></span> <span data-ttu-id="73945-132">Detta skiljer sig från hello klassiska distributionsmodellen, som använder hello molnbaserad tjänst som hello belastningsutjämnaren FQDN.</span><span class="sxs-lookup"><span data-stu-id="73945-132">This is different from hello classic deployment model, which uses hello cloud service as hello load balancer FQDN.</span></span>
   > <span data-ttu-id="73945-133">I det här exemplet hello FQDN är **loadbalancernrp.westus.cloudapp.azure.com**.</span><span class="sxs-lookup"><span data-stu-id="73945-133">In this example, hello FQDN is **loadbalancernrp.westus.cloudapp.azure.com**.</span></span>

## <a name="create-a-front-end-ip-pool-and-a-back-end-address-pool"></a><span data-ttu-id="73945-134">Skapa en IP-adresspool på klientsidan och en backend-adresspool</span><span class="sxs-lookup"><span data-stu-id="73945-134">Create a front-end IP pool and a back-end address pool</span></span>

1. <span data-ttu-id="73945-135">Skapa en frontend IP-pool med namnet **LB-klientdel** som använder hello **PublicIp** resurs.</span><span class="sxs-lookup"><span data-stu-id="73945-135">Create a front-end IP pool named **LB-Frontend** that uses hello **PublicIp** resource.</span></span>

    ```powershell
    $frontendIP = New-AzureRmLoadBalancerFrontendIpConfig -Name LB-Frontend -PublicIpAddress $publicIP
    ```

2. <span data-ttu-id="73945-136">Skapa en backend-adresspool med namnet **LB-backend**.</span><span class="sxs-lookup"><span data-stu-id="73945-136">Create a back-end address pool named **LB-backend**.</span></span>

    ```powershell
    $beaddresspool = New-AzureRmLoadBalancerBackendAddressPoolConfig -Name LB-backend
    ```

## <a name="create-nat-rules-a-load-balancer-rule-a-probe-and-a-load-balancer"></a><span data-ttu-id="73945-137">Skapa NAT-regler, en belastningsutjämningsregel, en avsökning och en belastningsutjämnare</span><span class="sxs-lookup"><span data-stu-id="73945-137">Create NAT rules, a load balancer rule, a probe, and a load balancer</span></span>

<span data-ttu-id="73945-138">Det här exemplet skapar hello följande objekt:</span><span class="sxs-lookup"><span data-stu-id="73945-138">This example creates hello following items:</span></span>

* <span data-ttu-id="73945-139">En NAT-regel tootranslate all inkommande trafik på port 3441 tooport 3389</span><span class="sxs-lookup"><span data-stu-id="73945-139">A NAT rule tootranslate all incoming traffic on port 3441 tooport 3389</span></span>
* <span data-ttu-id="73945-140">En NAT-regel tootranslate all inkommande trafik på port 3442 tooport 3389</span><span class="sxs-lookup"><span data-stu-id="73945-140">A NAT rule tootranslate all incoming traffic on port 3442 tooport 3389</span></span>
* <span data-ttu-id="73945-141">En avsökning regeln toocheck hello hälsostatus på en sida med namnet **HealthProbe.aspx**</span><span class="sxs-lookup"><span data-stu-id="73945-141">A probe rule toocheck hello health status on a page named **HealthProbe.aspx**</span></span>
* <span data-ttu-id="73945-142">En belastningen belastningsutjämnaren regeln toobalance all inkommande trafik på port 80 tooport 80 på hello-adresser i hello backend-adresspool</span><span class="sxs-lookup"><span data-stu-id="73945-142">A load balancer rule toobalance all incoming traffic on port 80 tooport 80 on hello addresses in hello back-end pool</span></span>
* <span data-ttu-id="73945-143">En belastningsutjämnare som använder alla dessa objekt</span><span class="sxs-lookup"><span data-stu-id="73945-143">A load balancer that uses all these objects</span></span>

<span data-ttu-id="73945-144">Följ dessa steg:</span><span class="sxs-lookup"><span data-stu-id="73945-144">Use these steps:</span></span>

1. <span data-ttu-id="73945-145">Skapa hello NAT-regler.</span><span class="sxs-lookup"><span data-stu-id="73945-145">Create hello NAT rules.</span></span>

    ```powershell
    $inboundNATRule1= New-AzureRmLoadBalancerInboundNatRuleConfig -Name RDP1 -FrontendIpConfiguration $frontendIP -Protocol TCP -FrontendPort 3441 -BackendPort 3389

    $inboundNATRule2= New-AzureRmLoadBalancerInboundNatRuleConfig -Name RDP2 -FrontendIpConfiguration $frontendIP -Protocol TCP -FrontendPort 3442 -BackendPort 3389
    ```

2. <span data-ttu-id="73945-146">Skapa en hälsoavsökning.</span><span class="sxs-lookup"><span data-stu-id="73945-146">Create a health probe.</span></span> <span data-ttu-id="73945-147">Det finns två sätt tooconfigure en avsökning:</span><span class="sxs-lookup"><span data-stu-id="73945-147">There are two ways tooconfigure a probe:</span></span>

    <span data-ttu-id="73945-148">HTTP-avsökning</span><span class="sxs-lookup"><span data-stu-id="73945-148">HTTP probe</span></span>

    ```powershell
    $healthProbe = New-AzureRmLoadBalancerProbeConfig -Name HealthProbe -RequestPath 'HealthProbe.aspx' -Protocol http -Port 80 -IntervalInSeconds 15 -ProbeCount 2
    ```

    <span data-ttu-id="73945-149">TCP-avsökning</span><span class="sxs-lookup"><span data-stu-id="73945-149">TCP probe</span></span>

    ```powershell
    $healthProbe = New-AzureRmLoadBalancerProbeConfig -Name HealthProbe -Protocol Tcp -Port 80 -IntervalInSeconds 15 -ProbeCount 2
    ```

3. <span data-ttu-id="73945-150">Skapa en belastningsutjämningsregel.</span><span class="sxs-lookup"><span data-stu-id="73945-150">Create a load balancer rule.</span></span>

    ```powershell
    $lbrule = New-AzureRmLoadBalancerRuleConfig -Name HTTP -FrontendIpConfiguration $frontendIP -BackendAddressPool  $beAddressPool -Probe $healthProbe -Protocol Tcp -FrontendPort 80 -BackendPort 80
    ```

4. <span data-ttu-id="73945-151">Skapa hello belastningsutjämnaren med hello som tidigare har skapat objekt.</span><span class="sxs-lookup"><span data-stu-id="73945-151">Create hello load balancer by using hello previously created objects.</span></span>

    ```powershell
    $NRPLB = New-AzureRmLoadBalancer -ResourceGroupName NRP-RG -Name NRP-LB -Location 'West US' -FrontendIpConfiguration $frontendIP -InboundNatRule $inboundNATRule1,$inboundNatRule2 -LoadBalancingRule $lbrule -BackendAddressPool $beAddressPool -Probe $healthProbe
    ```

## <a name="create-nics"></a><span data-ttu-id="73945-152">Skapa nätverkskort</span><span class="sxs-lookup"><span data-stu-id="73945-152">Create NICs</span></span>

<span data-ttu-id="73945-153">Skapa nätverksgränssnitt (eller ändra befintliga) och sedan koppla dem tooNAT regler, regler för inläsning av belastningsutjämnare och avsökningar:</span><span class="sxs-lookup"><span data-stu-id="73945-153">Create network interfaces (or modify existing ones) and then associate them tooNAT rules, load balancer rules, and probes:</span></span>

1. <span data-ttu-id="73945-154">Hämta hello virtuella nätverk och undernät för ett virtuellt nätverk där hello nätverkskort måste toobe skapas.</span><span class="sxs-lookup"><span data-stu-id="73945-154">Get hello virtual network and a virtual network subnet, where hello NICs need toobe created.</span></span>

    ```powershell
    $vnet = Get-AzureRmVirtualNetwork -Name NRPVNet -ResourceGroupName NRP-RG
    $backendSubnet = Get-AzureRmVirtualNetworkSubnetConfig -Name LB-Subnet-BE -VirtualNetwork $vnet
    ```

2. <span data-ttu-id="73945-155">Skapa ett nätverkskort med namnet **lb nic1 vara**, och koppla den till hello första NAT-regeln och hello första (och endast) backend-adresspool.</span><span class="sxs-lookup"><span data-stu-id="73945-155">Create a NIC named **lb-nic1-be**, and associate it with hello first NAT rule and hello first (and only) back-end address pool.</span></span>

    ```powershell
    $backendnic1= New-AzureRmNetworkInterface -ResourceGroupName NRP-RG -Name lb-nic1-be -Location 'West US' -PrivateIpAddress 10.0.2.6 -Subnet $backendSubnet -LoadBalancerBackendAddressPool $nrplb.BackendAddressPools[0] -LoadBalancerInboundNatRule $nrplb.InboundNatRules[0]
    ```

3. <span data-ttu-id="73945-156">Skapa ett nätverkskort med namnet **lb nic2 vara**, och koppla den till hello andra NAT-regeln och hello första (och endast) backend-adresspool.</span><span class="sxs-lookup"><span data-stu-id="73945-156">Create a NIC named **lb-nic2-be**, and associate it with hello second NAT rule and hello first (and only) back-end address pool.</span></span>

    ```powershell
    $backendnic2= New-AzureRmNetworkInterface -ResourceGroupName NRP-RG -Name lb-nic2-be -Location 'West US' -PrivateIpAddress 10.0.2.7 -Subnet $backendSubnet -LoadBalancerBackendAddressPool $nrplb.BackendAddressPools[0] -LoadBalancerInboundNatRule $nrplb.InboundNatRules[1]
    ```

4. <span data-ttu-id="73945-157">Kontrollera hello nätverkskort.</span><span class="sxs-lookup"><span data-stu-id="73945-157">Check hello NICs.</span></span>

        $backendnic1

    <span data-ttu-id="73945-158">Förväntad utdata:</span><span class="sxs-lookup"><span data-stu-id="73945-158">Expected output:</span></span>

        Name                 : lb-nic1-be
        ResourceGroupName    : NRP-RG
        Location             : westus
        Id                   : /subscriptions/f50504a2-1865-4541-823a-b32842e3e0ee/resourceGroups/NRP-RG/providers/Microsoft.Network/networkInterfaces/lb-nic1-be
        Etag                 : W/"d448256a-e1df-413a-9103-a137e07276d1"
        ResourceGuid         : 896cac4f-152a-40b9-b079-3e2201a5906e
        ProvisioningState    : Succeeded
        Tags                 :
        VirtualMachine       : null
        IpConfigurations     : [
                            {
                            "Name": "ipconfig1",
                            "Etag": "W/\"d448256a-e1df-413a-9103-a137e07276d1\"",
                            "Id": "/subscriptions/f50504a2-1865-4541-823a-b32842e3e0ee/resourceGroups/NRP-RG/providers/Microsoft.Network/networkInterfaces/lb-nic1-be/ipConfigurations/ipconfig1",
                            "PrivateIpAddress": "10.0.2.6",
                            "PrivateIpAllocationMethod": "Static",
                            "Subnet": {
                                "Id": "/subscriptions/f50504a2-1865-4541-823a-b32842e3e0ee/resourceGroups/NRP-RG/providers/Microsoft.Network/virtualNetworks/NRPVNet/subnets/LB-Subnet-BE"
                            },
                            "ProvisioningState": "Succeeded",
                            "PrivateIpAddressVersion": "IPv4",
                            "PublicIpAddress": {
                                "Id": null
                            },
                            "LoadBalancerBackendAddressPools": [
                                {
                                "Id": "/subscriptions/f50504a2-1865-4541-823a-b32842e3e0ee/resourceGroups/NRP-RG/providers/Microsoft.Network/loadBalancers/NRPlb/backendAddressPools/LB-backend"
                                }
                            ],
                            "LoadBalancerInboundNatRules": [
                                {
                                "Id": "/subscriptions/f50504a2-1865-4541-823a-b32842e3e0ee/resourceGroups/NRP-RG/providers/Microsoft.Network/loadBalancers/NRPlb/inboundNatRules/RDP1"
                                }
                            ],
                            "Primary": true,
                            "ApplicationGatewayBackendAddressPools": []
                            }
                        ]
        DnsSettings          : {
                            "DnsServers": [],
                            "AppliedDnsServers": [],
                            "InternalDomainNameSuffix": "prcwibzcuvie5hnxav0yjks2cd.dx.internal.cloudapp.net"
                        }
        EnableIPForwarding   : False
        NetworkSecurityGroup : null
        Primary              :

5. <span data-ttu-id="73945-159">Använd hello `Add-AzureRmVMNetworkInterface` cmdlet tooassign hello nätverkskort toodifferent virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="73945-159">Use hello `Add-AzureRmVMNetworkInterface` cmdlet tooassign hello NICs toodifferent VMs.</span></span>

## <a name="create-a-virtual-machine"></a><span data-ttu-id="73945-160">Skapa en virtuell dator</span><span class="sxs-lookup"><span data-stu-id="73945-160">Create a virtual machine</span></span>

<span data-ttu-id="73945-161">Vägledning om hur du skapar en virtuell dator och tilldelar ett nätverkskort finns i avsnittet om hur du [skapar en virtuell dator i Azure med hjälp av PowerShell](../virtual-machines/virtual-machines-windows-ps-create.md?toc=%2fazure%2fload-balancer%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="73945-161">For guidance on creating a virtual machine and assigning a NIC, see [Create an Azure VM using PowerShell](../virtual-machines/virtual-machines-windows-ps-create.md?toc=%2fazure%2fload-balancer%2ftoc.json).</span></span>

## <a name="add-hello-network-interface-toohello-load-balancer"></a><span data-ttu-id="73945-162">Lägga till hello network interface toohello belastningsutjämnare</span><span class="sxs-lookup"><span data-stu-id="73945-162">Add hello network interface toohello load balancer</span></span>

1. <span data-ttu-id="73945-163">Hämta hello belastningsutjämnaren från Azure.</span><span class="sxs-lookup"><span data-stu-id="73945-163">Retrieve hello load balancer from Azure.</span></span>

    <span data-ttu-id="73945-164">Läs in hello belastningsutjämningsresurs i en variabel (om du inte har gjort det ännu).</span><span class="sxs-lookup"><span data-stu-id="73945-164">Load hello load balancer resource into a variable (if you haven't done that yet).</span></span> <span data-ttu-id="73945-165">hello variabeln kallas **$lb**. Använd hello samma namn från hello belastningsutjämningsresurs som du skapade tidigare.</span><span class="sxs-lookup"><span data-stu-id="73945-165">hello variable is called **$lb**. Use hello same names from hello load balancer resource that you created earlier.</span></span>

    ```powershell
    $lb= get-azurermloadbalancer -name NRP-LB -resourcegroupname NRP-RG
    ```

2. <span data-ttu-id="73945-166">Läsa in hello backend-konfiguration tooa variabeln.</span><span class="sxs-lookup"><span data-stu-id="73945-166">Load hello back-end configuration tooa variable.</span></span>

    ```powershell
    $backend=Get-AzureRmLoadBalancerBackendAddressPoolConfig -name LB-backend -LoadBalancer $lb
    ```

3. <span data-ttu-id="73945-167">Läs in hello redan skapat nätverksgränssnitt i en variabel.</span><span class="sxs-lookup"><span data-stu-id="73945-167">Load hello already created network interface into a variable.</span></span> <span data-ttu-id="73945-168">hello variabelnamnet är **$nic**.</span><span class="sxs-lookup"><span data-stu-id="73945-168">hello variable name is **$nic**.</span></span> <span data-ttu-id="73945-169">hello nätverksgränssnittets namn är hello samma från hello tidigare exemplet.</span><span class="sxs-lookup"><span data-stu-id="73945-169">hello network interface name is hello same one from hello earlier example.</span></span>

    ```powershell
    $nic =get-azurermnetworkinterface -name lb-nic1-be -resourcegroupname NRP-RG
    ```

4. <span data-ttu-id="73945-170">Ändra hello backend-konfiguration på hello nätverksgränssnitt.</span><span class="sxs-lookup"><span data-stu-id="73945-170">Change hello back-end configuration on hello network interface.</span></span>

    ```powershell
    $nic.IpConfigurations[0].LoadBalancerBackendAddressPools=$backend
    ```

5. <span data-ttu-id="73945-171">Spara hello objektet för nätverksgränssnittet.</span><span class="sxs-lookup"><span data-stu-id="73945-171">Save hello network interface object.</span></span>

    ```powershell
    Set-AzureRmNetworkInterface -NetworkInterface $nic
    ```

    <span data-ttu-id="73945-172">När ett nätverksgränssnitt läggs toohello belastningen belastningsutjämnaren backend-adresspool, startar den tar emot nätverkstrafik baserat på hello belastningsutjämning regler för att belastningsutjämningsresurs.</span><span class="sxs-lookup"><span data-stu-id="73945-172">After a network interface is added toohello load balancer back-end pool, it starts receiving network traffic based on hello load-balancing rules for that load balancer resource.</span></span>

## <a name="update-an-existing-load-balancer"></a><span data-ttu-id="73945-173">Uppdatera en befintlig belastningsutjämnare</span><span class="sxs-lookup"><span data-stu-id="73945-173">Update an existing load balancer</span></span>

1. <span data-ttu-id="73945-174">Med hjälp av hello belastningsutjämnaren från Hej tidigare exemplet, tilldela en belastningen belastningsutjämnaren toohello objektvariabel **$slb** med hjälp av `Get-AzureLoadBalancer`.</span><span class="sxs-lookup"><span data-stu-id="73945-174">By using hello load balancer from hello earlier example, assign a load balancer object toohello variable **$slb** by using `Get-AzureLoadBalancer`.</span></span>

    ```powershell
    $slb = get-AzureRmLoadBalancer -Name NRP-LB -ResourceGroupName NRP-RG
    ```

2. <span data-ttu-id="73945-175">I följande exempel hello, du lägger till en inkommande NAT-regel--med port 81 i hello frontend-pool och port 8181 för hello backend-poolen--tooan befintliga belastningsutjämnaren.</span><span class="sxs-lookup"><span data-stu-id="73945-175">In hello following example, you add an inbound NAT rule--by using port 81 in hello front-end pool and port 8181 for hello back-end pool--tooan existing load balancer.</span></span>

    ```powershell
    $slb | Add-AzureRmLoadBalancerInboundNatRuleConfig -Name NewRule -FrontendIpConfiguration $slb.FrontendIpConfigurations[0] -FrontendPort 81  -BackendPort 8181 -Protocol TCP
    ```

3. <span data-ttu-id="73945-176">Spara hello nya konfigurationen med hjälp av `Set-AzureLoadBalancer`.</span><span class="sxs-lookup"><span data-stu-id="73945-176">Save hello new configuration by using `Set-AzureLoadBalancer`.</span></span>

    ```powershell
    $slb | Set-AzureRmLoadBalancer
    ```

## <a name="remove-a-load-balancer"></a><span data-ttu-id="73945-177">Ta bort en belastningsutjämnare</span><span class="sxs-lookup"><span data-stu-id="73945-177">Remove a load balancer</span></span>

<span data-ttu-id="73945-178">Kommandot hello `Remove-AzureLoadBalancer` toodelete tidigare skapade belastningsutjämning med namnet **NRP-LB** i en resursgrupp kallas **NRP RG**.</span><span class="sxs-lookup"><span data-stu-id="73945-178">Use hello command `Remove-AzureLoadBalancer` toodelete a previously created load balancer named **NRP-LB** in a resource group called **NRP-RG**.</span></span>

```powershell
Remove-AzureRmLoadBalancer -Name NRP-LB -ResourceGroupName NRP-RG
```

> [!NOTE]
> <span data-ttu-id="73945-179">Du kan använda hello valfri växel **-Force** tooavoid hello prompt för borttagning.</span><span class="sxs-lookup"><span data-stu-id="73945-179">You can use hello optional switch **-Force** tooavoid hello prompt for deletion.</span></span>

## <a name="next-steps"></a><span data-ttu-id="73945-180">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="73945-180">Next steps</span></span>

[<span data-ttu-id="73945-181">Komma igång med att konfigurera en intern belastningsutjämnare</span><span class="sxs-lookup"><span data-stu-id="73945-181">Get started configuring an internal load balancer</span></span>](load-balancer-get-started-ilb-arm-ps.md)

[<span data-ttu-id="73945-182">Konfigurera ett distributionsläge för belastningsutjämnare</span><span class="sxs-lookup"><span data-stu-id="73945-182">Configure a load balancer distribution mode</span></span>](load-balancer-distribution-mode.md)

[<span data-ttu-id="73945-183">Konfigurera timeout-inställningar för inaktiv TCP för en belastningsutjämnare</span><span class="sxs-lookup"><span data-stu-id="73945-183">Configure idle TCP timeout settings for your load balancer</span></span>](load-balancer-tcp-idle-timeout.md)
