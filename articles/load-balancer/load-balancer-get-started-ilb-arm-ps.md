---
title: "aaaCreate en Azure-intern belastningsutjämnare - PowerShell | Microsoft Docs"
description: "Lär dig hur toocreate en intern belastningsutjämnare med PowerShell i Resource Manager"
services: load-balancer
documentationcenter: na
author: kumudd
manager: timlt
tags: azure-resource-manager
ms.assetid: c6c98981-df9d-4dd7-a94b-cc7d1dc99369
ms.service: load-balancer
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 01/23/2017
ms.author: kumud
ms.openlocfilehash: 4b9657c77aa32a142de49ff7871ed6a396b22223
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-an-internal-load-balancer-using-powershell"></a><span data-ttu-id="80475-103">Skapa en intern belastningsutjämnare med PowerShell</span><span class="sxs-lookup"><span data-stu-id="80475-103">Create an internal load balancer using PowerShell</span></span>

> [!div class="op_single_selector"]
> * [<span data-ttu-id="80475-104">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="80475-104">Azure Portal</span></span>](../load-balancer/load-balancer-get-started-ilb-arm-portal.md)
> * [<span data-ttu-id="80475-105">PowerShell</span><span class="sxs-lookup"><span data-stu-id="80475-105">PowerShell</span></span>](../load-balancer/load-balancer-get-started-ilb-arm-ps.md)
> * [<span data-ttu-id="80475-106">Azure CLI</span><span class="sxs-lookup"><span data-stu-id="80475-106">Azure CLI</span></span>](../load-balancer/load-balancer-get-started-ilb-arm-cli.md)
> * [<span data-ttu-id="80475-107">Mall</span><span class="sxs-lookup"><span data-stu-id="80475-107">Template</span></span>](../load-balancer/load-balancer-get-started-ilb-arm-template.md)

[!INCLUDE [load-balancer-get-started-ilb-intro-include.md](../../includes/load-balancer-get-started-ilb-intro-include.md)]

> [!NOTE]
> <span data-ttu-id="80475-108">Azure har två olika distributionsmodeller för att skapa och arbeta med resurser: [Resource Manager och klassisk](../azure-resource-manager/resource-manager-deployment-model.md).</span><span class="sxs-lookup"><span data-stu-id="80475-108">Azure has two different deployment models for creating and working with resources:  [Resource Manager and classic](../azure-resource-manager/resource-manager-deployment-model.md).</span></span>  <span data-ttu-id="80475-109">Den här artikeln täcker hello Resource Manager-distributionsmodellen, som Microsoft rekommenderar för de flesta nya distributioner i stället för hello [klassiska distributionsmodellen](load-balancer-get-started-ilb-classic-ps.md).</span><span class="sxs-lookup"><span data-stu-id="80475-109">This article covers using hello Resource Manager deployment model, which Microsoft recommends for most new deployments instead of hello [classic deployment model](load-balancer-get-started-ilb-classic-ps.md).</span></span>

[!INCLUDE [load-balancer-get-started-ilb-scenario-include.md](../../includes/load-balancer-get-started-ilb-scenario-include.md)]

[!INCLUDE [azure-ps-prerequisites-include.md](../../includes/azure-ps-prerequisites-include.md)]

<span data-ttu-id="80475-110">hello följande steg förklarar hur toocreate en intern belastningsutjämnare med Azure Resource Manager med PowerShell.</span><span class="sxs-lookup"><span data-stu-id="80475-110">hello following steps explain how toocreate an internal load balancer using Azure Resource Manager with PowerShell.</span></span> <span data-ttu-id="80475-111">Med Azure Resource Manager hello objekt toocreate en intern belastningsutjämnare konfigureras separat och kombinerade toocreate belastningsutjämning.</span><span class="sxs-lookup"><span data-stu-id="80475-111">With Azure Resource Manager, hello items toocreate a Internal load balancer are configured individually and then combined toocreate a load balancer.</span></span>

<span data-ttu-id="80475-112">Du behöver toocreate och konfigurera hello följande objekt toodeploy en belastningsutjämnare:</span><span class="sxs-lookup"><span data-stu-id="80475-112">You need toocreate and configure hello following objects toodeploy a load balancer:</span></span>

* <span data-ttu-id="80475-113">Front end IP-konfiguration – konfigurerar hello privata IP-adressen för inkommande trafik</span><span class="sxs-lookup"><span data-stu-id="80475-113">Front end IP configuration - will configure hello private IP address for incoming network traffic</span></span>
* <span data-ttu-id="80475-114">Serverdelen för adresspoolen - konfigurerar hello nätverksgränssnitt som tar emot hello belastningsutjämnade trafik som kommer från klientdelen IP-pool</span><span class="sxs-lookup"><span data-stu-id="80475-114">Backend address pool - will configure hello network interfaces which will receive hello load balanced traffic coming from front end IP pool</span></span>
* <span data-ttu-id="80475-115">Regler för belastningsutjämning - käll- och lokala portkonfiguration för hello belastningsutjämnare.</span><span class="sxs-lookup"><span data-stu-id="80475-115">Load balancing rules - source and local port configuration for hello load balancer.</span></span>
* <span data-ttu-id="80475-116">Avsökningar - konfigurerar hello status hälsoavsökningen för hello virtuella instanser.</span><span class="sxs-lookup"><span data-stu-id="80475-116">Probes - configures hello health status probe for hello Virtual Machine instances.</span></span>
* <span data-ttu-id="80475-117">Inkommande NAT-regler – konfigurerar hello port regler toodirectly åtkomst en av hello instanser för virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="80475-117">Inbound NAT rules - configures hello port rules toodirectly access one of hello Virtual Machine instances.</span></span>

<span data-ttu-id="80475-118">Mer information om belastningsutjämningskomponenter med Azure Resource Manager finns i [Azure Resource Manager-stöd för belastningsutjämnare](load-balancer-arm.md).</span><span class="sxs-lookup"><span data-stu-id="80475-118">You can get more information about load balancer components with Azure resource manager at [Azure Resource Manager support for load balancer](load-balancer-arm.md).</span></span>

<span data-ttu-id="80475-119">hello följande steg förklarar hur tooconfigure belastningsutjämning mellan två virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="80475-119">hello following steps explain how tooconfigure a load balancer between two virtual machines.</span></span>

## <a name="setup-powershell-toouse-resource-manager"></a><span data-ttu-id="80475-120">Installationsprogrammet för PowerShell toouse Resource Manager</span><span class="sxs-lookup"><span data-stu-id="80475-120">Setup PowerShell toouse Resource Manager</span></span>

<span data-ttu-id="80475-121">Kontrollera att du har hello senaste produktionsversionen av hello Azure-modulen för PowerShell och har PowerShell installationsprogrammet korrekt tooaccess din Azure-prenumeration.</span><span class="sxs-lookup"><span data-stu-id="80475-121">Make sure you have hello latest production version of hello Azure module for PowerShell, and have PowerShell setup correctly tooaccess your Azure subscription.</span></span>

### <a name="step-1"></a><span data-ttu-id="80475-122">Steg 1</span><span class="sxs-lookup"><span data-stu-id="80475-122">Step 1</span></span>

```powershell
Login-AzureRmAccount
```

### <a name="step-2"></a><span data-ttu-id="80475-123">Steg 2</span><span class="sxs-lookup"><span data-stu-id="80475-123">Step 2</span></span>

<span data-ttu-id="80475-124">Kontrollera hello prenumerationer för hello-konto</span><span class="sxs-lookup"><span data-stu-id="80475-124">Check hello subscriptions for hello account</span></span>

```powershell
Get-AzureRmSubscription
```

<span data-ttu-id="80475-125">Du kan ange tooAuthenticate med dina autentiseringsuppgifter.</span><span class="sxs-lookup"><span data-stu-id="80475-125">You will be prompted tooAuthenticate with your credentials.</span></span>

### <a name="step-3"></a><span data-ttu-id="80475-126">Steg 3</span><span class="sxs-lookup"><span data-stu-id="80475-126">Step 3</span></span>

<span data-ttu-id="80475-127">Välj vilka av dina Azure-prenumerationer toouse.</span><span class="sxs-lookup"><span data-stu-id="80475-127">Choose which of your Azure subscriptions toouse.</span></span>

```powershell
Select-AzureRmSubscription -Subscriptionid "GUID of subscription"
```

### <a name="create-resource-group-for-load-balancer"></a><span data-ttu-id="80475-128">Skapa resursgrupp för belastningsutjämnare</span><span class="sxs-lookup"><span data-stu-id="80475-128">Create Resource Group for load balancer</span></span>

<span data-ttu-id="80475-129">Skapa en ny resursgrupp (hoppa över detta steg om du använder en befintlig resursgrupp)</span><span class="sxs-lookup"><span data-stu-id="80475-129">Create a new resource group (skip this step if using an existing resource group)</span></span>

```powershell
New-AzureRmResourceGroup -Name NRP-RG -location "West US"
```

<span data-ttu-id="80475-130">Azure Resource Manager kräver att alla resursgrupper anger en plats.</span><span class="sxs-lookup"><span data-stu-id="80475-130">Azure Resource Manager requires that all resource groups specify a location.</span></span> <span data-ttu-id="80475-131">Detta används som hello standardplatsen för resurser i resursgruppen.</span><span class="sxs-lookup"><span data-stu-id="80475-131">This is used as hello default location for resources in that resource group.</span></span> <span data-ttu-id="80475-132">Kontrollera att alla kommandon som använder en belastningsutjämnare toocreate hello samma resursgrupp.</span><span class="sxs-lookup"><span data-stu-id="80475-132">Make sure all commands toocreate a load balancer will use hello same resource group.</span></span>

<span data-ttu-id="80475-133">I hello skapa exemplet ovan vi en resursgrupp med namnet ”NRP-RG” och plats ”West US”.</span><span class="sxs-lookup"><span data-stu-id="80475-133">In hello example above we created a resource group called "NRP-RG" and location "West US".</span></span>

## <a name="create-virtual-network-and-a-private-ip-address-for-front-end-ip-pool"></a><span data-ttu-id="80475-134">Skapa virtuella nätverk och en privat IP-adress för IP-pooler på klientsidan</span><span class="sxs-lookup"><span data-stu-id="80475-134">Create Virtual Network and a private IP address for front end IP pool</span></span>

<span data-ttu-id="80475-135">Skapar ett undernät för virtuellt nätverk för hello och tilldelar toovariable $backendSubnet</span><span class="sxs-lookup"><span data-stu-id="80475-135">Creates a subnet for hello virtual network and assigns toovariable $backendSubnet</span></span>

```powershell
$backendSubnet = New-AzureRmVirtualNetworkSubnetConfig -Name LB-Subnet-BE -AddressPrefix 10.0.2.0/24
```

<span data-ttu-id="80475-136">Skapa ett virtuellt nätverk:</span><span class="sxs-lookup"><span data-stu-id="80475-136">Create a virtual network:</span></span>

```powershell
$vnet= New-AzureRmVirtualNetwork -Name NRPVNet -ResourceGroupName NRP-RG -Location "West US" -AddressPrefix 10.0.0.0/16 -Subnet $backendSubnet
```

<span data-ttu-id="80475-137">Skapar hello virtuella nätverk och lägger till hello lb undernät vara toohello virtuella undernätverk NRPVNet och tilldelar toovariable $vnet</span><span class="sxs-lookup"><span data-stu-id="80475-137">Creates hello virtual network and adds hello subnet lb-subnet-be toohello virtual network NRPVNet and assigns toovariable $vnet</span></span>

## <a name="create-front-end-ip-pool-and-backend-address-pool"></a><span data-ttu-id="80475-138">Skapa en IP-pool på klientsidan och en serverdelsadresspool</span><span class="sxs-lookup"><span data-stu-id="80475-138">Create Front end IP pool and backend address pool</span></span>

<span data-ttu-id="80475-139">Ställa in en IP-adresspool för klientdelen för inkommande hälsningspaket att läsa in belastningsutjämning nätverkstrafik och backend-adresspoolen tooreceive hello belastningsutjämnade trafik.</span><span class="sxs-lookup"><span data-stu-id="80475-139">Setting up a front end IP pool for hello incoming load balancer network traffic and backend address pool tooreceive hello load balanced traffic.</span></span>

### <a name="step-1"></a><span data-ttu-id="80475-140">Steg 1</span><span class="sxs-lookup"><span data-stu-id="80475-140">Step 1</span></span>

<span data-ttu-id="80475-141">Skapa en IP-adresspool för klientdelen som använder hello privat IP-adress 10.0.2.5 för hello undernät 10.0.2.0/24 som ska vara hello inkommande trafik nätverksslutpunkten.</span><span class="sxs-lookup"><span data-stu-id="80475-141">Create a front end IP pool using hello private IP address 10.0.2.5 for hello subnet 10.0.2.0/24 which will be hello incoming network traffic endpoint.</span></span>

```powershell
$frontendIP = New-AzureRmLoadBalancerFrontendIpConfig -Name LB-Frontend -PrivateIpAddress 10.0.2.5 -SubnetId $vnet.subnets[0].Id
```

### <a name="step-2"></a><span data-ttu-id="80475-142">Steg 2</span><span class="sxs-lookup"><span data-stu-id="80475-142">Step 2</span></span>

<span data-ttu-id="80475-143">Ställ in en backend-adresspool används tooreceive inkommande trafik från klientdelen IP-adresspool:</span><span class="sxs-lookup"><span data-stu-id="80475-143">Set up a back end address pool used tooreceive incoming traffic from front end IP pool:</span></span>

```powershell
$beaddresspool= New-AzureRmLoadBalancerBackendAddressPoolConfig -Name "LB-backend"
```

## <a name="create-lb-rules-nat-rules-probe-and-load-balancer"></a><span data-ttu-id="80475-144">Skapa LB-regler, NAT-regler, avsökning och belastningsutjämnare</span><span class="sxs-lookup"><span data-stu-id="80475-144">Create LB rules, NAT rules, probe and load balancer</span></span>

<span data-ttu-id="80475-145">När du har skapat hello klientdelen IP-adresspool och hello serverdelen för adresspoolen, behöver du toocreate hello regler som kommer att tillhöra toohello belastningsutjämningsresurs:</span><span class="sxs-lookup"><span data-stu-id="80475-145">After creating hello front end IP pool and hello backend address pool, you will need toocreate hello rules which will belong toohello load balancer resource:</span></span>

### <a name="step-1"></a><span data-ttu-id="80475-146">Steg 1</span><span class="sxs-lookup"><span data-stu-id="80475-146">Step 1</span></span>

```powershell
$inboundNATRule1= New-AzureRmLoadBalancerInboundNatRuleConfig -Name "RDP1" -FrontendIpConfiguration $frontendIP -Protocol TCP -FrontendPort 3441 -BackendPort 3389

$inboundNATRule2= New-AzureRmLoadBalancerInboundNatRuleConfig -Name "RDP2" -FrontendIpConfiguration $frontendIP -Protocol TCP -FrontendPort 3442 -BackendPort 3389

$healthProbe = New-AzureRmLoadBalancerProbeConfig -Name "HealthProbe" -RequestPath "HealthProbe.aspx" -Protocol http -Port 80 -IntervalInSeconds 15 -ProbeCount 2

$lbrule = New-AzureRmLoadBalancerRuleConfig -Name "HTTP" -FrontendIpConfiguration $frontendIP -BackendAddressPool $beAddressPool -Probe $healthProbe -Protocol Tcp -FrontendPort 80 -BackendPort 80
```

<span data-ttu-id="80475-147">hello-exemplet ovan skapar hello följande objekt:</span><span class="sxs-lookup"><span data-stu-id="80475-147">hello example above is creating hello following items:</span></span>

* <span data-ttu-id="80475-148">NAT-regel som all inkommande trafik tooport 3441 går tooport 3389.</span><span class="sxs-lookup"><span data-stu-id="80475-148">NAT rule which all incoming traffic tooport 3441 will go tooport 3389.</span></span>
* <span data-ttu-id="80475-149">en annan NAT-regel som all inkommande trafik tooport 3442 går tooport 3389.</span><span class="sxs-lookup"><span data-stu-id="80475-149">a second NAT rule which all incoming traffic tooport 3442 will go tooport 3389.</span></span>
* <span data-ttu-id="80475-150">en regel för belastningsutjämnare som läser in balansera all inkommande trafik på port 80 av offentlig port 80 toolocal i hello backend-adresspool.</span><span class="sxs-lookup"><span data-stu-id="80475-150">a load balancer rule which will load balance all incoming traffic on public port 80 toolocal port 80 in hello back end address pool.</span></span>
* <span data-ttu-id="80475-151">en avsökning regel som kontrollerar hello hälsostatus för sökvägen ”HealthProbe.aspx”</span><span class="sxs-lookup"><span data-stu-id="80475-151">a probe rule which will check hello health status for path "HealthProbe.aspx"</span></span>

### <a name="step-2"></a><span data-ttu-id="80475-152">Steg 2</span><span class="sxs-lookup"><span data-stu-id="80475-152">Step 2</span></span>

<span data-ttu-id="80475-153">Skapa hello belastningsutjämnaren om du lägger till alla objekt (NAT-regler, regler för inläsning av belastningsutjämnare, avsökningen konfigurationer) tillsammans:</span><span class="sxs-lookup"><span data-stu-id="80475-153">Create hello load balancer adding all objects (NAT rules, Load balancer rules, probe configurations) together:</span></span>

```powershell
$NRPLB = New-AzureRmLoadBalancer -ResourceGroupName "NRP-RG" -Name "NRP-LB" -Location "West US" -FrontendIpConfiguration $frontendIP -InboundNatRule $inboundNATRule1,$inboundNatRule2 -LoadBalancingRule $lbrule -BackendAddressPool $beAddressPool -Probe $healthProbe
```

## <a name="create-network-interfaces"></a><span data-ttu-id="80475-154">Skapa nätverksgränssnitt</span><span class="sxs-lookup"><span data-stu-id="80475-154">Create network interfaces</span></span>

<span data-ttu-id="80475-155">När du har skapat hello intern belastningsutjämnare, måste du definiera vilka nätverksgränssnitt får hello inkommande belastningsutjämnade trafik, NAT-regler och avsökning.</span><span class="sxs-lookup"><span data-stu-id="80475-155">After creating hello internal load balancer, you need define which network interfaces will be receiving hello incoming load balanced network traffic, NAT rules and probe.</span></span> <span data-ttu-id="80475-156">hello-nätverksgränssnitt i det här fallet konfigureras individuellt och kan tilldelas tooa virtuell dator vid ett senare tillfälle.</span><span class="sxs-lookup"><span data-stu-id="80475-156">hello network interface in this case is configured individually and can be assigned tooa virtual machine later on.</span></span>

### <a name="step-1"></a><span data-ttu-id="80475-157">Steg 1</span><span class="sxs-lookup"><span data-stu-id="80475-157">Step 1</span></span>

<span data-ttu-id="80475-158">Hämta hello resurs virtuella nätverk och undernät toocreate nätverksgränssnitt:</span><span class="sxs-lookup"><span data-stu-id="80475-158">Get hello resource virtual network and subnet toocreate network interfaces:</span></span>

```powershell
$vnet = Get-AzureRmVirtualNetwork -Name NRPVNet -ResourceGroupName NRP-RG

$backendSubnet = Get-AzureRmVirtualNetworkSubnetConfig -Name LB-Subnet-BE -VirtualNetwork $vnet
```

<span data-ttu-id="80475-159">Det här steget skapar ett nätverksgränssnitt vilket tillhör toohello belastningen belastningsutjämnarens serverdelspool och associera hello första NAT-regeln för RDP för det här nätverksgränssnittet:</span><span class="sxs-lookup"><span data-stu-id="80475-159">This step creates a network interface which will belong toohello load balancer back end pool and associate hello first NAT rule for RDP for this network interface:</span></span>

```powershell
$backendnic1= New-AzureRmNetworkInterface -ResourceGroupName "NRP-RG" -Name lb-nic1-be -Location "West US" -PrivateIpAddress 10.0.2.6 -Subnet $backendSubnet -LoadBalancerBackendAddressPool $nrplb.BackendAddressPools[0] -LoadBalancerInboundNatRule $nrplb.InboundNatRules[0]
```

### <a name="step-2"></a><span data-ttu-id="80475-160">Steg 2</span><span class="sxs-lookup"><span data-stu-id="80475-160">Step 2</span></span>

<span data-ttu-id="80475-161">Skapa ett andra nätverksgränssnitt som kallas LB-Nic2-BE:</span><span class="sxs-lookup"><span data-stu-id="80475-161">Create a second network interface called LB-Nic2-BE:</span></span>

<span data-ttu-id="80475-162">Det här steget skapar ett andra nätverksgränssnitt, tilldela toohello samma belastningsutjämnare tillbaka avslutas poolen och associera hello andra NAT-regel skapas för RDP:</span><span class="sxs-lookup"><span data-stu-id="80475-162">This step creates a second network interface, assigning toohello same load balancer back end pool and associating hello second NAT rule created for RDP:</span></span>

```powershell
$backendnic2= New-AzureRmNetworkInterface -ResourceGroupName "NRP-RG" -Name lb-nic2-be -Location "West US" -PrivateIpAddress 10.0.2.7 -Subnet $backendSubnet -LoadBalancerBackendAddressPool $nrplb.BackendAddressPools[0] -LoadBalancerInboundNatRule $nrplb.InboundNatRules[1]
```

<span data-ttu-id="80475-163">hello slutresultatet visar hello följande:</span><span class="sxs-lookup"><span data-stu-id="80475-163">hello end result will show hello following:</span></span>

    $backendnic1

<span data-ttu-id="80475-164">Förväntad utdata:</span><span class="sxs-lookup"><span data-stu-id="80475-164">Expected output:</span></span>

    Name                 : lb-nic1-be
    ResourceGroupName    : NRP-RG
    Location             : westus
    Id                   : /subscriptions/f50504a2-1865-4541-823a-b32842e3e0ee/resourceGroups/NRP-RG/providers/Microsoft.Network/networkInterfaces/lb-nic1-be
    Etag                 : W/"d448256a-e1df-413a-9103-a137e07276d1"
    ProvisioningState    : Succeeded
    Tags                 :
    VirtualMachine       : null
    IpConfigurations     : [
                         {
                           "PrivateIpAddress": "10.0.2.6",
                           "PrivateIpAllocationMethod": "Static",
                           "Subnet": {
                             "Id": "/subscriptions/f50504a2-1865-4541-823a-b32842e3e0ee/resourceGroups/NRP-RG/providers/Microsoft.Network/virtualNetworks/NRPVNet/subnets/LB-Subnet-BE"
                           },
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
                           "ProvisioningState": "Succeeded",
                           "Name": "ipconfig1",
                           "Etag": "W/\"d448256a-e1df-413a-9103-a137e07276d1\"",
                           "Id": "/subscriptions/f50504a2-1865-4541-823a-b32842e3e0ee/resourceGroups/NRP-RG/providers/Microsoft.Network/networkInterfaces/lb-nic1-be/ipConfigurations/ipconfig1"
                         }
                       ]
    DnsSettings          : {
                         "DnsServers": [],
                         "AppliedDnsServers": []
                       }
    AppliedDnsSettings   :
    NetworkSecurityGroup : null
    Primary              : False



### <a name="step-3"></a><span data-ttu-id="80475-165">Steg 3</span><span class="sxs-lookup"><span data-stu-id="80475-165">Step 3</span></span>

<span data-ttu-id="80475-166">Använda hello kommandot Lägg till AzureRmVMNetworkInterface tooassign hello NIC tooa virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="80475-166">Use hello command Add-AzureRmVMNetworkInterface tooassign hello NIC tooa virtual Machine.</span></span>

<span data-ttu-id="80475-167">Du kan hitta hello steg-för-steg-instruktioner toocreate en virtuell dator och tilldela tooa NIC följande hello-dokumentationen: [skapa en virtuell Azure-dator med hjälp av PowerShell](../virtual-machines/virtual-machines-windows-ps-create.md?toc=%2fazure%2fload-balancer%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="80475-167">You can find hello step by step instructions toocreate a virtual machine and assign tooa NIC following hello documentation: [Create an Azure VM using PowerShell](../virtual-machines/virtual-machines-windows-ps-create.md?toc=%2fazure%2fload-balancer%2ftoc.json).</span></span>

## <a name="add-hello-network-interface"></a><span data-ttu-id="80475-168">Lägga till hello nätverksgränssnitt</span><span class="sxs-lookup"><span data-stu-id="80475-168">Add hello network interface</span></span>

<span data-ttu-id="80475-169">Du kan lägga till hello nätverksgränssnitt med hello följande steg om du redan har en virtuell dator som skapats:</span><span class="sxs-lookup"><span data-stu-id="80475-169">If you already have a virtual machine created, you can add hello network interface with hello following steps:</span></span>

### <a name="step-1"></a><span data-ttu-id="80475-170">Steg 1</span><span class="sxs-lookup"><span data-stu-id="80475-170">Step 1</span></span>

<span data-ttu-id="80475-171">Läs in hello belastningsutjämningsresurs i en variabel (om du inte har gjort det ännu).</span><span class="sxs-lookup"><span data-stu-id="80475-171">Load hello load balancer resource into a variable (if you haven't done that yet).</span></span> <span data-ttu-id="80475-172">hello-variabel som används är kallas $lb och Använd hello samma namn från hello belastningsutjämningsresurs skapade ovan.</span><span class="sxs-lookup"><span data-stu-id="80475-172">hello variable used is called $lb and use hello same names from hello load balancer resource created above.</span></span>

```powershell
$lb = Get-AzureRmLoadBalancer –name NRP-LB -resourcegroupname NRP-RG
```

### <a name="step-2"></a><span data-ttu-id="80475-173">Steg 2</span><span class="sxs-lookup"><span data-stu-id="80475-173">Step 2</span></span>

<span data-ttu-id="80475-174">Läsa in hello backend configuration tooa variabeln.</span><span class="sxs-lookup"><span data-stu-id="80475-174">Load hello backend configuration tooa variable.</span></span>

```powershell
$backend = Get-AzureRmLoadBalancerBackendAddressPoolConfig -name backendpool1 -LoadBalancer $lb
```

### <a name="step-3"></a><span data-ttu-id="80475-175">Steg 3</span><span class="sxs-lookup"><span data-stu-id="80475-175">Step 3</span></span>

<span data-ttu-id="80475-176">Läs in hello redan skapat nätverksgränssnitt i en variabel.</span><span class="sxs-lookup"><span data-stu-id="80475-176">Load hello already created network interface into a variable.</span></span> <span data-ttu-id="80475-177">hello variabelnamnet som används är $nic.</span><span class="sxs-lookup"><span data-stu-id="80475-177">hello variable name used is $nic.</span></span> <span data-ttu-id="80475-178">hello nätverksgränssnittets namn används hello samma från hello-exemplet ovan.</span><span class="sxs-lookup"><span data-stu-id="80475-178">hello network interface name used is hello same from hello example above.</span></span>

```powershell
$nic = Get-AzureRmNetworkInterface –name lb-nic1-be -resourcegroupname NRP-RG
```

### <a name="step-4"></a><span data-ttu-id="80475-179">Steg 4</span><span class="sxs-lookup"><span data-stu-id="80475-179">Step 4</span></span>

<span data-ttu-id="80475-180">Ändra hello backend-konfiguration på hello nätverksgränssnitt.</span><span class="sxs-lookup"><span data-stu-id="80475-180">Change hello backend configuration on hello network interface.</span></span>

```powershell
$nic.IpConfigurations[0].LoadBalancerBackendAddressPools=$backend
```

### <a name="step-5"></a><span data-ttu-id="80475-181">Steg 5</span><span class="sxs-lookup"><span data-stu-id="80475-181">Step 5</span></span>

<span data-ttu-id="80475-182">Spara hello objektet för nätverksgränssnittet.</span><span class="sxs-lookup"><span data-stu-id="80475-182">Save hello network interface object.</span></span>

```powershell
Set-AzureRmNetworkInterface -NetworkInterface $nic
```

<span data-ttu-id="80475-183">När ett nätverksgränssnitt läggs toohello belastningsutjämnarens serverdelspool, startar den tar emot nätverkstrafik baserat på hello regler för att belastningsutjämningsresurs för belastningsutjämning.</span><span class="sxs-lookup"><span data-stu-id="80475-183">After a network interface is added toohello load balancer backend pool, it starts receiving network traffic based on hello load balancing rules for that load balancer resource.</span></span>

## <a name="update-an-existing-load-balancer"></a><span data-ttu-id="80475-184">Uppdatera en befintlig belastningsutjämnare</span><span class="sxs-lookup"><span data-stu-id="80475-184">Update an existing load balancer</span></span>

### <a name="step-1"></a><span data-ttu-id="80475-185">Steg 1</span><span class="sxs-lookup"><span data-stu-id="80475-185">Step 1</span></span>
<span data-ttu-id="80475-186">Använda hello belastningsutjämnare från hello-exemplet ovan, tilldela load balancer objektet toovariable $slb med hjälp av Get-AzureRmLoadBalancer</span><span class="sxs-lookup"><span data-stu-id="80475-186">Using hello load balancer from hello example above, assign load balancer object toovariable $slb using Get-AzureRmLoadBalancer</span></span>

```powershell
$slb = Get-AzureRmLoadBalancer -Name NRPLB -ResourceGroupName NRP-RG
```

### <a name="step-2"></a><span data-ttu-id="80475-187">Steg 2</span><span class="sxs-lookup"><span data-stu-id="80475-187">Step 2</span></span>

<span data-ttu-id="80475-188">I följande exempel hello, lägger du till en ny inkommande NAT-regel som använder port 81 i hello klientdelen och port 8181 för hello tillbaka sluta poolen tooan befintliga belastningsutjämnare</span><span class="sxs-lookup"><span data-stu-id="80475-188">In hello following example, you will add a new Inbound NAT rule using port 81 in hello front end and port 8181 for hello back end pool tooan existing load balancer</span></span>

```powershell
$slb | Add-AzureRmLoadBalancerInboundNatRuleConfig -Name NewRule -FrontendIpConfiguration $slb.FrontendIpConfigurations[0] -FrontendPort 81  -BackendPort 8181 -Protocol Tcp
```

### <a name="step-3"></a><span data-ttu-id="80475-189">Steg 3</span><span class="sxs-lookup"><span data-stu-id="80475-189">Step 3</span></span>

<span data-ttu-id="80475-190">Spara hello nya konfigurationen med hjälp av Set-AzureLoadBalancer</span><span class="sxs-lookup"><span data-stu-id="80475-190">Save hello new configuration using Set-AzureLoadBalancer</span></span>

```powershell
$slb | Set-AzureRmLoadBalancer
```

## <a name="remove-a-load-balancer"></a><span data-ttu-id="80475-191">Ta bort en belastningsutjämnare</span><span class="sxs-lookup"><span data-stu-id="80475-191">Remove a load balancer</span></span>

<span data-ttu-id="80475-192">Använd hello kommandot Remove-AzureRmLoadBalancer toodelete tidigare skapade belastningsutjämning med namnet ”NRP-LB” i en resursgrupp kallas ”NRP-RG”</span><span class="sxs-lookup"><span data-stu-id="80475-192">Use hello command Remove-AzureRmLoadBalancer toodelete a previously created load balancer named "NRP-LB"  in a resource group called "NRP-RG"</span></span>

```powershell
Remove-AzureRmLoadBalancer -Name NRPLB -ResourceGroupName NRP-RG
```

> [!NOTE]
> <span data-ttu-id="80475-193">Du kan använda valfri hello växla - Force tooavoid hello prompt för borttagning.</span><span class="sxs-lookup"><span data-stu-id="80475-193">You can use hello optional switch -Force tooavoid hello prompt for deletion.</span></span>

## <a name="next-steps"></a><span data-ttu-id="80475-194">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="80475-194">Next steps</span></span>

[<span data-ttu-id="80475-195">Konfigurera ett distributionsläge för belastningsutjämnare</span><span class="sxs-lookup"><span data-stu-id="80475-195">Configure a Load balancer distribution mode</span></span>](load-balancer-distribution-mode.md)

[<span data-ttu-id="80475-196">Konfigurera timeout-inställningar för inaktiv TCP för en belastningsutjämnare</span><span class="sxs-lookup"><span data-stu-id="80475-196">Configure idle TCP timeout settings for your load balancer</span></span>](load-balancer-tcp-idle-timeout.md)
