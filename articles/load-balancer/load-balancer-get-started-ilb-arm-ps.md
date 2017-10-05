---
title: "Skapa en intern Azure-belastningsutjämnare – PowerShell | Microsoft Docs"
description: "Ta reda på hur du skapar en intern belastningsutjämnare med PowerShell i Resource Manager"
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
ms.openlocfilehash: 7bd31ab8f52ec5e81f6966000554be46eaa59396
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="create-an-internal-load-balancer-using-powershell"></a><span data-ttu-id="fa2c2-103">Skapa en intern belastningsutjämnare med PowerShell</span><span class="sxs-lookup"><span data-stu-id="fa2c2-103">Create an internal load balancer using PowerShell</span></span>

> [!div class="op_single_selector"]
> * [<span data-ttu-id="fa2c2-104">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="fa2c2-104">Azure Portal</span></span>](../load-balancer/load-balancer-get-started-ilb-arm-portal.md)
> * [<span data-ttu-id="fa2c2-105">PowerShell</span><span class="sxs-lookup"><span data-stu-id="fa2c2-105">PowerShell</span></span>](../load-balancer/load-balancer-get-started-ilb-arm-ps.md)
> * [<span data-ttu-id="fa2c2-106">Azure CLI</span><span class="sxs-lookup"><span data-stu-id="fa2c2-106">Azure CLI</span></span>](../load-balancer/load-balancer-get-started-ilb-arm-cli.md)
> * [<span data-ttu-id="fa2c2-107">Mall</span><span class="sxs-lookup"><span data-stu-id="fa2c2-107">Template</span></span>](../load-balancer/load-balancer-get-started-ilb-arm-template.md)

[!INCLUDE [load-balancer-get-started-ilb-intro-include.md](../../includes/load-balancer-get-started-ilb-intro-include.md)]

> [!NOTE]
> <span data-ttu-id="fa2c2-108">Azure har två olika distributionsmodeller för att skapa och arbeta med resurser: [Resource Manager och klassisk](../azure-resource-manager/resource-manager-deployment-model.md).</span><span class="sxs-lookup"><span data-stu-id="fa2c2-108">Azure has two different deployment models for creating and working with resources:  [Resource Manager and classic](../azure-resource-manager/resource-manager-deployment-model.md).</span></span>  <span data-ttu-id="fa2c2-109">Den här artikeln beskriver Resource Manager-distributionsmodellen, som Microsoft rekommenderar för de flesta nya distributioner i stället för [den klassiska distributionsmodellen](load-balancer-get-started-ilb-classic-ps.md).</span><span class="sxs-lookup"><span data-stu-id="fa2c2-109">This article covers using the Resource Manager deployment model, which Microsoft recommends for most new deployments instead of the [classic deployment model](load-balancer-get-started-ilb-classic-ps.md).</span></span>

[!INCLUDE [load-balancer-get-started-ilb-scenario-include.md](../../includes/load-balancer-get-started-ilb-scenario-include.md)]

[!INCLUDE [azure-ps-prerequisites-include.md](../../includes/azure-ps-prerequisites-include.md)]

<span data-ttu-id="fa2c2-110">Följande steg beskriver hur du skapar en intern belastningsutjämnare med hjälp av Azure Resource Manager och PowerShell.</span><span class="sxs-lookup"><span data-stu-id="fa2c2-110">The following steps explain how to create an internal load balancer using Azure Resource Manager with PowerShell.</span></span> <span data-ttu-id="fa2c2-111">Med Azure Resource Manager konfigureras objekten som behövs för att skapa en intern belastningsutjämnare för sig och sedan kombineras de för att skapa en belastningsutjämnare.</span><span class="sxs-lookup"><span data-stu-id="fa2c2-111">With Azure Resource Manager, the items to create a Internal load balancer are configured individually and then combined to create a load balancer.</span></span>

<span data-ttu-id="fa2c2-112">Du måste skapa och konfigurera följande objekt för att distribuera en belastningsutjämnare:</span><span class="sxs-lookup"><span data-stu-id="fa2c2-112">You need to create and configure the following objects to deploy a load balancer:</span></span>

* <span data-ttu-id="fa2c2-113">IP-konfiguration på klientsidan – konfigurerar den offentliga IP-adressen för inkommande nätverkstrafik</span><span class="sxs-lookup"><span data-stu-id="fa2c2-113">Front end IP configuration - will configure the private IP address for incoming network traffic</span></span>
* <span data-ttu-id="fa2c2-114">Serverdelsadresspool – konfigurerar nätverksgränssnitt som tar emot belastningsutjämnad trafik från IP-poolen på klientsidan</span><span class="sxs-lookup"><span data-stu-id="fa2c2-114">Backend address pool - will configure the network interfaces which will receive the load balanced traffic coming from front end IP pool</span></span>
* <span data-ttu-id="fa2c2-115">Belastningsutjämningsregler – käll- och lokalportkonfiguration för belastningsutjämnaren.</span><span class="sxs-lookup"><span data-stu-id="fa2c2-115">Load balancing rules - source and local port configuration for the load balancer.</span></span>
* <span data-ttu-id="fa2c2-116">Avsökningar – konfigurerar hälsoavsökningar för virtuella datorinstanser.</span><span class="sxs-lookup"><span data-stu-id="fa2c2-116">Probes - configures the health status probe for the Virtual Machine instances.</span></span>
* <span data-ttu-id="fa2c2-117">Ingående NAT-regler – konfigurerar portreglerna för direktåtkomst till någon av de virtuella datorinstanserna.</span><span class="sxs-lookup"><span data-stu-id="fa2c2-117">Inbound NAT rules - configures the port rules to directly access one of the Virtual Machine instances.</span></span>

<span data-ttu-id="fa2c2-118">Mer information om belastningsutjämningskomponenter med Azure Resource Manager finns i [Azure Resource Manager-stöd för belastningsutjämnare](load-balancer-arm.md).</span><span class="sxs-lookup"><span data-stu-id="fa2c2-118">You can get more information about load balancer components with Azure resource manager at [Azure Resource Manager support for load balancer](load-balancer-arm.md).</span></span>

<span data-ttu-id="fa2c2-119">Följande steg beskriver hur du konfigurerar en belastningsutjämnare mellan två virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="fa2c2-119">The following steps explain how to configure a load balancer between two virtual machines.</span></span>

## <a name="setup-powershell-to-use-resource-manager"></a><span data-ttu-id="fa2c2-120">Konfigurera PowerShell för användning med Resource Manager</span><span class="sxs-lookup"><span data-stu-id="fa2c2-120">Setup PowerShell to use Resource Manager</span></span>

<span data-ttu-id="fa2c2-121">Kontrollera att du har den senaste produktionsversionen av Azure-modulen för PowerShell, och konfigurera PowerShell på rätt sätt för att få åtkomst till din Azure-prenumeration.</span><span class="sxs-lookup"><span data-stu-id="fa2c2-121">Make sure you have the latest production version of the Azure module for PowerShell, and have PowerShell setup correctly to access your Azure subscription.</span></span>

### <a name="step-1"></a><span data-ttu-id="fa2c2-122">Steg 1</span><span class="sxs-lookup"><span data-stu-id="fa2c2-122">Step 1</span></span>

```powershell
Login-AzureRmAccount
```

### <a name="step-2"></a><span data-ttu-id="fa2c2-123">Steg 2</span><span class="sxs-lookup"><span data-stu-id="fa2c2-123">Step 2</span></span>

<span data-ttu-id="fa2c2-124">Kontrollera kontots prenumerationer</span><span class="sxs-lookup"><span data-stu-id="fa2c2-124">Check the subscriptions for the account</span></span>

```powershell
Get-AzureRmSubscription
```

<span data-ttu-id="fa2c2-125">Du uppmanas att autentisera dig med dina autentiseringsuppgifter.</span><span class="sxs-lookup"><span data-stu-id="fa2c2-125">You will be prompted to Authenticate with your credentials.</span></span>

### <a name="step-3"></a><span data-ttu-id="fa2c2-126">Steg 3</span><span class="sxs-lookup"><span data-stu-id="fa2c2-126">Step 3</span></span>

<span data-ttu-id="fa2c2-127">Välj vilka av dina Azure-prenumerationer som du vill använda.</span><span class="sxs-lookup"><span data-stu-id="fa2c2-127">Choose which of your Azure subscriptions to use.</span></span>

```powershell
Select-AzureRmSubscription -Subscriptionid "GUID of subscription"
```

### <a name="create-resource-group-for-load-balancer"></a><span data-ttu-id="fa2c2-128">Skapa resursgrupp för belastningsutjämnare</span><span class="sxs-lookup"><span data-stu-id="fa2c2-128">Create Resource Group for load balancer</span></span>

<span data-ttu-id="fa2c2-129">Skapa en ny resursgrupp (hoppa över detta steg om du använder en befintlig resursgrupp)</span><span class="sxs-lookup"><span data-stu-id="fa2c2-129">Create a new resource group (skip this step if using an existing resource group)</span></span>

```powershell
New-AzureRmResourceGroup -Name NRP-RG -location "West US"
```

<span data-ttu-id="fa2c2-130">Azure Resource Manager kräver att alla resursgrupper anger en plats.</span><span class="sxs-lookup"><span data-stu-id="fa2c2-130">Azure Resource Manager requires that all resource groups specify a location.</span></span> <span data-ttu-id="fa2c2-131">Detta används som standardplatsen för resurser i resursgruppen.</span><span class="sxs-lookup"><span data-stu-id="fa2c2-131">This is used as the default location for resources in that resource group.</span></span> <span data-ttu-id="fa2c2-132">Se till att alla kommandon för att skapa en belastningsutjämnare använder samma resursgrupp.</span><span class="sxs-lookup"><span data-stu-id="fa2c2-132">Make sure all commands to create a load balancer will use the same resource group.</span></span>

<span data-ttu-id="fa2c2-133">I exemplet ovan skapade vi resursgruppen NRP-RG och platsen Västra USA.</span><span class="sxs-lookup"><span data-stu-id="fa2c2-133">In the example above we created a resource group called "NRP-RG" and location "West US".</span></span>

## <a name="create-virtual-network-and-a-private-ip-address-for-front-end-ip-pool"></a><span data-ttu-id="fa2c2-134">Skapa virtuella nätverk och en privat IP-adress för IP-pooler på klientsidan</span><span class="sxs-lookup"><span data-stu-id="fa2c2-134">Create Virtual Network and a private IP address for front end IP pool</span></span>

<span data-ttu-id="fa2c2-135">Skapar ett undernät för det virtuella nätverket som tilldelas till variabeln $backendSubnet</span><span class="sxs-lookup"><span data-stu-id="fa2c2-135">Creates a subnet for the virtual network and assigns to variable $backendSubnet</span></span>

```powershell
$backendSubnet = New-AzureRmVirtualNetworkSubnetConfig -Name LB-Subnet-BE -AddressPrefix 10.0.2.0/24
```

<span data-ttu-id="fa2c2-136">Skapa ett virtuellt nätverk:</span><span class="sxs-lookup"><span data-stu-id="fa2c2-136">Create a virtual network:</span></span>

```powershell
$vnet= New-AzureRmVirtualNetwork -Name NRPVNet -ResourceGroupName NRP-RG -Location "West US" -AddressPrefix 10.0.0.0/16 -Subnet $backendSubnet
```

<span data-ttu-id="fa2c2-137">Skapar det virtuella nätverket och lägger till undernätet lb-subnet-be i det virtuella nätverket NRPVNet som tilldelas till variabeln $vnet</span><span class="sxs-lookup"><span data-stu-id="fa2c2-137">Creates the virtual network and adds the subnet lb-subnet-be to the virtual network NRPVNet and assigns to variable $vnet</span></span>

## <a name="create-front-end-ip-pool-and-backend-address-pool"></a><span data-ttu-id="fa2c2-138">Skapa en IP-pool på klientsidan och en serverdelsadresspool</span><span class="sxs-lookup"><span data-stu-id="fa2c2-138">Create Front end IP pool and backend address pool</span></span>

<span data-ttu-id="fa2c2-139">Konfigurera en IP-pool på klientsidan för inkommande belastningsutjämningstrafik och en serverdelsadresspool för att ta emot belastningsutjämnad trafik.</span><span class="sxs-lookup"><span data-stu-id="fa2c2-139">Setting up a front end IP pool for the incoming load balancer network traffic and backend address pool to receive the load balanced traffic.</span></span>

### <a name="step-1"></a><span data-ttu-id="fa2c2-140">Steg 1</span><span class="sxs-lookup"><span data-stu-id="fa2c2-140">Step 1</span></span>

<span data-ttu-id="fa2c2-141">Skapa en IP-pool på klientsidan med den privata IP-adressen 10.0.2.5 för undernätet 10.0.2.0/24 som blir slutpunkten för den inkommande nätverkstrafiken.</span><span class="sxs-lookup"><span data-stu-id="fa2c2-141">Create a front end IP pool using the private IP address 10.0.2.5 for the subnet 10.0.2.0/24 which will be the incoming network traffic endpoint.</span></span>

```powershell
$frontendIP = New-AzureRmLoadBalancerFrontendIpConfig -Name LB-Frontend -PrivateIpAddress 10.0.2.5 -SubnetId $vnet.subnets[0].Id
```

### <a name="step-2"></a><span data-ttu-id="fa2c2-142">Steg 2</span><span class="sxs-lookup"><span data-stu-id="fa2c2-142">Step 2</span></span>

<span data-ttu-id="fa2c2-143">Konfigurera en serverdelsadresspool som används för att ta emot inkommande trafik från IP-poolen på klientsidan:</span><span class="sxs-lookup"><span data-stu-id="fa2c2-143">Set up a back end address pool used to receive incoming traffic from front end IP pool:</span></span>

```powershell
$beaddresspool= New-AzureRmLoadBalancerBackendAddressPoolConfig -Name "LB-backend"
```

## <a name="create-lb-rules-nat-rules-probe-and-load-balancer"></a><span data-ttu-id="fa2c2-144">Skapa LB-regler, NAT-regler, avsökning och belastningsutjämnare</span><span class="sxs-lookup"><span data-stu-id="fa2c2-144">Create LB rules, NAT rules, probe and load balancer</span></span>

<span data-ttu-id="fa2c2-145">När du har skapat IP-poolen på klientsidan och serverdelsadresspoolen behöver du skapa reglerna som hör till belastningsutjämningsresursen:</span><span class="sxs-lookup"><span data-stu-id="fa2c2-145">After creating the front end IP pool and the backend address pool, you will need to create the rules which will belong to the load balancer resource:</span></span>

### <a name="step-1"></a><span data-ttu-id="fa2c2-146">Steg 1</span><span class="sxs-lookup"><span data-stu-id="fa2c2-146">Step 1</span></span>

```powershell
$inboundNATRule1= New-AzureRmLoadBalancerInboundNatRuleConfig -Name "RDP1" -FrontendIpConfiguration $frontendIP -Protocol TCP -FrontendPort 3441 -BackendPort 3389

$inboundNATRule2= New-AzureRmLoadBalancerInboundNatRuleConfig -Name "RDP2" -FrontendIpConfiguration $frontendIP -Protocol TCP -FrontendPort 3442 -BackendPort 3389

$healthProbe = New-AzureRmLoadBalancerProbeConfig -Name "HealthProbe" -RequestPath "HealthProbe.aspx" -Protocol http -Port 80 -IntervalInSeconds 15 -ProbeCount 2

$lbrule = New-AzureRmLoadBalancerRuleConfig -Name "HTTP" -FrontendIpConfiguration $frontendIP -BackendAddressPool $beAddressPool -Probe $healthProbe -Protocol Tcp -FrontendPort 80 -BackendPort 80
```

<span data-ttu-id="fa2c2-147">I exemplet ovan skapar du följande objekt:</span><span class="sxs-lookup"><span data-stu-id="fa2c2-147">The example above is creating the following items:</span></span>

* <span data-ttu-id="fa2c2-148">NAT-regel som anger att all inkommande trafik till port 3441 skickas till port 3389.</span><span class="sxs-lookup"><span data-stu-id="fa2c2-148">NAT rule which all incoming traffic to port 3441 will go to port 3389.</span></span>
* <span data-ttu-id="fa2c2-149">en andra NAT-regel som anger att all inkommande trafik till port 3442 skickas till port 3389.</span><span class="sxs-lookup"><span data-stu-id="fa2c2-149">a second NAT rule which all incoming traffic to port 3442 will go to port 3389.</span></span>
* <span data-ttu-id="fa2c2-150">en belastningsutjämningsregel som balanserar all inkommande trafik på den offentliga porten 80 till den lokala porten 80 i serverdelsadresspoolen.</span><span class="sxs-lookup"><span data-stu-id="fa2c2-150">a load balancer rule which will load balance all incoming traffic on public port 80 to local port 80 in the back end address pool.</span></span>
* <span data-ttu-id="fa2c2-151">en avsökningsregel som kontrollerar hälsotillståndet för sökvägen HealthProbe.aspx</span><span class="sxs-lookup"><span data-stu-id="fa2c2-151">a probe rule which will check the health status for path "HealthProbe.aspx"</span></span>

### <a name="step-2"></a><span data-ttu-id="fa2c2-152">Steg 2</span><span class="sxs-lookup"><span data-stu-id="fa2c2-152">Step 2</span></span>

<span data-ttu-id="fa2c2-153">Skapa belastningsutjämnaren genom att lägga samman alla objekt (NAT-regler, belastningsutjämningsregler, avsökningskonfigurationer):</span><span class="sxs-lookup"><span data-stu-id="fa2c2-153">Create the load balancer adding all objects (NAT rules, Load balancer rules, probe configurations) together:</span></span>

```powershell
$NRPLB = New-AzureRmLoadBalancer -ResourceGroupName "NRP-RG" -Name "NRP-LB" -Location "West US" -FrontendIpConfiguration $frontendIP -InboundNatRule $inboundNATRule1,$inboundNatRule2 -LoadBalancingRule $lbrule -BackendAddressPool $beAddressPool -Probe $healthProbe
```

## <a name="create-network-interfaces"></a><span data-ttu-id="fa2c2-154">Skapa nätverksgränssnitt</span><span class="sxs-lookup"><span data-stu-id="fa2c2-154">Create network interfaces</span></span>

<span data-ttu-id="fa2c2-155">När du har skapat den interna belastningsutjämnaren måste du definiera vilka nätverksgränssnitt som tar emot den inkommande belastningsutjämnade nätverkstrafiken, NAT-reglerna och avsökningen.</span><span class="sxs-lookup"><span data-stu-id="fa2c2-155">After creating the internal load balancer, you need define which network interfaces will be receiving the incoming load balanced network traffic, NAT rules and probe.</span></span> <span data-ttu-id="fa2c2-156">Nätverksgränssnittet konfigureras i det här fallet separat och kan tilldelas till en virtuell dator senare.</span><span class="sxs-lookup"><span data-stu-id="fa2c2-156">The network interface in this case is configured individually and can be assigned to a virtual machine later on.</span></span>

### <a name="step-1"></a><span data-ttu-id="fa2c2-157">Steg 1</span><span class="sxs-lookup"><span data-stu-id="fa2c2-157">Step 1</span></span>

<span data-ttu-id="fa2c2-158">Hämta det virtuella resursnätverket och undernätet för att skapa nätverksgränssnitt:</span><span class="sxs-lookup"><span data-stu-id="fa2c2-158">Get the resource virtual network and subnet to create network interfaces:</span></span>

```powershell
$vnet = Get-AzureRmVirtualNetwork -Name NRPVNet -ResourceGroupName NRP-RG

$backendSubnet = Get-AzureRmVirtualNetworkSubnetConfig -Name LB-Subnet-BE -VirtualNetwork $vnet
```

<span data-ttu-id="fa2c2-159">Det här steget skapar ett nätverksgränssnitt som tillhör belastningsutjämnarens serverdelspool och associerar den första NAT-regeln för RDP för det här nätverksgränssnittet:</span><span class="sxs-lookup"><span data-stu-id="fa2c2-159">This step creates a network interface which will belong to the load balancer back end pool and associate the first NAT rule for RDP for this network interface:</span></span>

```powershell
$backendnic1= New-AzureRmNetworkInterface -ResourceGroupName "NRP-RG" -Name lb-nic1-be -Location "West US" -PrivateIpAddress 10.0.2.6 -Subnet $backendSubnet -LoadBalancerBackendAddressPool $nrplb.BackendAddressPools[0] -LoadBalancerInboundNatRule $nrplb.InboundNatRules[0]
```

### <a name="step-2"></a><span data-ttu-id="fa2c2-160">Steg 2</span><span class="sxs-lookup"><span data-stu-id="fa2c2-160">Step 2</span></span>

<span data-ttu-id="fa2c2-161">Skapa ett andra nätverksgränssnitt som kallas LB-Nic2-BE:</span><span class="sxs-lookup"><span data-stu-id="fa2c2-161">Create a second network interface called LB-Nic2-BE:</span></span>

<span data-ttu-id="fa2c2-162">Det här steget skapar ett andra nätverksgränssnitt, som tilldelas samma serverdelspool för belastningsutjämnaren och associerar den andra NAT-regeln som skapats för RDP:</span><span class="sxs-lookup"><span data-stu-id="fa2c2-162">This step creates a second network interface, assigning to the same load balancer back end pool and associating the second NAT rule created for RDP:</span></span>

```powershell
$backendnic2= New-AzureRmNetworkInterface -ResourceGroupName "NRP-RG" -Name lb-nic2-be -Location "West US" -PrivateIpAddress 10.0.2.7 -Subnet $backendSubnet -LoadBalancerBackendAddressPool $nrplb.BackendAddressPools[0] -LoadBalancerInboundNatRule $nrplb.InboundNatRules[1]
```

<span data-ttu-id="fa2c2-163">Slutresultatet visar följande:</span><span class="sxs-lookup"><span data-stu-id="fa2c2-163">The end result will show the following:</span></span>

    $backendnic1

<span data-ttu-id="fa2c2-164">Förväntad utdata:</span><span class="sxs-lookup"><span data-stu-id="fa2c2-164">Expected output:</span></span>

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



### <a name="step-3"></a><span data-ttu-id="fa2c2-165">Steg 3</span><span class="sxs-lookup"><span data-stu-id="fa2c2-165">Step 3</span></span>

<span data-ttu-id="fa2c2-166">Använd kommandot Add-AzureRmVMNetworkInterface för att tilldela nätverkskortet till en virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="fa2c2-166">Use the command Add-AzureRmVMNetworkInterface to assign the NIC to a virtual Machine.</span></span>

<span data-ttu-id="fa2c2-167">Du kan hitta detaljerade anvisningarna för att skapa en virtuell dator och tilldela den till ett nätverkskort enligt dokumentationen: [Skapa en virtuell Azure-dator med PowerShell](../virtual-machines/virtual-machines-windows-ps-create.md?toc=%2fazure%2fload-balancer%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="fa2c2-167">You can find the step by step instructions to create a virtual machine and assign to a NIC following the documentation: [Create an Azure VM using PowerShell](../virtual-machines/virtual-machines-windows-ps-create.md?toc=%2fazure%2fload-balancer%2ftoc.json).</span></span>

## <a name="add-the-network-interface"></a><span data-ttu-id="fa2c2-168">Lägga till nätverksgränssnittet</span><span class="sxs-lookup"><span data-stu-id="fa2c2-168">Add the network interface</span></span>

<span data-ttu-id="fa2c2-169">Om du redan har skapat en virtuell dator kan du lägga till nätverksgränssnittet med följande steg:</span><span class="sxs-lookup"><span data-stu-id="fa2c2-169">If you already have a virtual machine created, you can add the network interface with the following steps:</span></span>

### <a name="step-1"></a><span data-ttu-id="fa2c2-170">Steg 1</span><span class="sxs-lookup"><span data-stu-id="fa2c2-170">Step 1</span></span>

<span data-ttu-id="fa2c2-171">Läs in belastningsutjämningsresursen i en variabel (om du inte har gjort det redan).</span><span class="sxs-lookup"><span data-stu-id="fa2c2-171">Load the load balancer resource into a variable (if you haven't done that yet).</span></span> <span data-ttu-id="fa2c2-172">Variabeln som används kallas $lb och använder samma namn från belastningsutjämningsresursen som skapades ovan.</span><span class="sxs-lookup"><span data-stu-id="fa2c2-172">The variable used is called $lb and use the same names from the load balancer resource created above.</span></span>

```powershell
$lb = Get-AzureRmLoadBalancer –name NRP-LB -resourcegroupname NRP-RG
```

### <a name="step-2"></a><span data-ttu-id="fa2c2-173">Steg 2</span><span class="sxs-lookup"><span data-stu-id="fa2c2-173">Step 2</span></span>

<span data-ttu-id="fa2c2-174">Läs in serverdelskonfigurationen i en variabel.</span><span class="sxs-lookup"><span data-stu-id="fa2c2-174">Load the backend configuration to a variable.</span></span>

```powershell
$backend = Get-AzureRmLoadBalancerBackendAddressPoolConfig -name backendpool1 -LoadBalancer $lb
```

### <a name="step-3"></a><span data-ttu-id="fa2c2-175">Steg 3</span><span class="sxs-lookup"><span data-stu-id="fa2c2-175">Step 3</span></span>

<span data-ttu-id="fa2c2-176">Läs in nätverksgränssnittet som du redan skapat i en variabel.</span><span class="sxs-lookup"><span data-stu-id="fa2c2-176">Load the already created network interface into a variable.</span></span> <span data-ttu-id="fa2c2-177">variabelnamnet som används är $nic.</span><span class="sxs-lookup"><span data-stu-id="fa2c2-177">the variable name used is $nic.</span></span> <span data-ttu-id="fa2c2-178">Namnet på nätverksgränssnittet som används är detsamma som i exemplet ovan.</span><span class="sxs-lookup"><span data-stu-id="fa2c2-178">The network interface name used is the same from the example above.</span></span>

```powershell
$nic = Get-AzureRmNetworkInterface –name lb-nic1-be -resourcegroupname NRP-RG
```

### <a name="step-4"></a><span data-ttu-id="fa2c2-179">Steg 4</span><span class="sxs-lookup"><span data-stu-id="fa2c2-179">Step 4</span></span>

<span data-ttu-id="fa2c2-180">Ändra serverdelskonfigurationen i nätverksgränssnittet.</span><span class="sxs-lookup"><span data-stu-id="fa2c2-180">Change the backend configuration on the network interface.</span></span>

```powershell
$nic.IpConfigurations[0].LoadBalancerBackendAddressPools=$backend
```

### <a name="step-5"></a><span data-ttu-id="fa2c2-181">Steg 5</span><span class="sxs-lookup"><span data-stu-id="fa2c2-181">Step 5</span></span>

<span data-ttu-id="fa2c2-182">Spara objektet för nätverksgränssnittet.</span><span class="sxs-lookup"><span data-stu-id="fa2c2-182">Save the network interface object.</span></span>

```powershell
Set-AzureRmNetworkInterface -NetworkInterface $nic
```

<span data-ttu-id="fa2c2-183">När ett nätverksgränssnitt har lagts till i serverdelspoolen för belastningsutjämnaren börjar det tar emot nätverkstrafik baserat på reglerna för belastningsutjämning för den belastningsutjämningsresursen.</span><span class="sxs-lookup"><span data-stu-id="fa2c2-183">After a network interface is added to the load balancer backend pool, it starts receiving network traffic based on the load balancing rules for that load balancer resource.</span></span>

## <a name="update-an-existing-load-balancer"></a><span data-ttu-id="fa2c2-184">Uppdatera en befintlig belastningsutjämnare</span><span class="sxs-lookup"><span data-stu-id="fa2c2-184">Update an existing load balancer</span></span>

### <a name="step-1"></a><span data-ttu-id="fa2c2-185">Steg 1</span><span class="sxs-lookup"><span data-stu-id="fa2c2-185">Step 1</span></span>
<span data-ttu-id="fa2c2-186">Med hjälp av belastningsutjämnaren från exemplet ovan tilldelar du belastningsutjämningsobjektet till variabeln $slb med hjälp av Get-AzureRmLoadBalancer</span><span class="sxs-lookup"><span data-stu-id="fa2c2-186">Using the load balancer from the example above, assign load balancer object to variable $slb using Get-AzureRmLoadBalancer</span></span>

```powershell
$slb = Get-AzureRmLoadBalancer -Name NRPLB -ResourceGroupName NRP-RG
```

### <a name="step-2"></a><span data-ttu-id="fa2c2-187">Steg 2</span><span class="sxs-lookup"><span data-stu-id="fa2c2-187">Step 2</span></span>

<span data-ttu-id="fa2c2-188">I följande exempel lägger du till en ny inkommande NAT-regel som använder port 81 på klientsidan och port 8181 för serverdelspoolen till en befintlig belastningsutjämnare</span><span class="sxs-lookup"><span data-stu-id="fa2c2-188">In the following example, you will add a new Inbound NAT rule using port 81 in the front end and port 8181 for the back end pool to an existing load balancer</span></span>

```powershell
$slb | Add-AzureRmLoadBalancerInboundNatRuleConfig -Name NewRule -FrontendIpConfiguration $slb.FrontendIpConfigurations[0] -FrontendPort 81  -BackendPort 8181 -Protocol Tcp
```

### <a name="step-3"></a><span data-ttu-id="fa2c2-189">Steg 3</span><span class="sxs-lookup"><span data-stu-id="fa2c2-189">Step 3</span></span>

<span data-ttu-id="fa2c2-190">Spara den nya konfigurationen med hjälp av Set-AzureLoadBalancer</span><span class="sxs-lookup"><span data-stu-id="fa2c2-190">Save the new configuration using Set-AzureLoadBalancer</span></span>

```powershell
$slb | Set-AzureRmLoadBalancer
```

## <a name="remove-a-load-balancer"></a><span data-ttu-id="fa2c2-191">Ta bort en belastningsutjämnare</span><span class="sxs-lookup"><span data-stu-id="fa2c2-191">Remove a load balancer</span></span>

<span data-ttu-id="fa2c2-192">Använd kommandot Remove-AzureRmLoadBalancer för att ta bort en tidigare skapad belastningsutjämnare med namnet NRP-LB från resursgruppen NRP-RG</span><span class="sxs-lookup"><span data-stu-id="fa2c2-192">Use the command Remove-AzureRmLoadBalancer to delete a previously created load balancer named "NRP-LB"  in a resource group called "NRP-RG"</span></span>

```powershell
Remove-AzureRmLoadBalancer -Name NRPLB -ResourceGroupName NRP-RG
```

> [!NOTE]
> <span data-ttu-id="fa2c2-193">Du kan använda den valfria växeln -Force (Framtvinga) om du inte vill uppmanas att bekräfta borttagningen.</span><span class="sxs-lookup"><span data-stu-id="fa2c2-193">You can use the optional switch -Force to avoid the prompt for deletion.</span></span>

## <a name="next-steps"></a><span data-ttu-id="fa2c2-194">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="fa2c2-194">Next steps</span></span>

[<span data-ttu-id="fa2c2-195">Konfigurera ett distributionsläge för belastningsutjämnare</span><span class="sxs-lookup"><span data-stu-id="fa2c2-195">Configure a Load balancer distribution mode</span></span>](load-balancer-distribution-mode.md)

[<span data-ttu-id="fa2c2-196">Konfigurera timeout-inställningar för inaktiv TCP för en belastningsutjämnare</span><span class="sxs-lookup"><span data-stu-id="fa2c2-196">Configure idle TCP timeout settings for your load balancer</span></span>](load-balancer-tcp-idle-timeout.md)
