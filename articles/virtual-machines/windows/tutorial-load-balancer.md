---
title: aaaHow tooload balansera Windows virtuella datorer i Azure | Microsoft Docs
description: "Lär dig hur toouse hello Azure läsa in belastningsutjämning toocreate ett program med hög tillgänglighet och säkert över tre virtuella Windows-datorer"
services: virtual-machines-windows
documentationcenter: virtual-machines
author: iainfoulds
manager: timlt
editor: tysonn
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 08/11/2017
ms.author: iainfou
ms.custom: mvc
ms.openlocfilehash: bfbd6a24e90a33e60d7d4f7b0c471af5d4e71f45
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="how-tooload-balance-windows-virtual-machines-in-azure-toocreate-a-highly-available-application"></a><span data-ttu-id="c1895-103">Hur balansera tooload Windows-datorer i Azure toocreate ett program med hög tillgänglighet</span><span class="sxs-lookup"><span data-stu-id="c1895-103">How tooload balance Windows virtual machines in Azure toocreate a highly available application</span></span>
<span data-ttu-id="c1895-104">Belastningsutjämning ger högre tillgänglighet genom att sprida inkommande begäranden över flera virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="c1895-104">Load balancing provides a higher level of availability by spreading incoming requests across multiple virtual machines.</span></span> <span data-ttu-id="c1895-105">I kursen får information du om hello olika komponenterna i hello Azure belastningsutjämnare som distribuerar trafik och ger hög tillgänglighet.</span><span class="sxs-lookup"><span data-stu-id="c1895-105">In this tutorial, you learn about hello different components of hello Azure load balancer that distribute traffic and provide high availability.</span></span> <span data-ttu-id="c1895-106">Lär dig att:</span><span class="sxs-lookup"><span data-stu-id="c1895-106">You learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="c1895-107">Skapa en Azure belastningsutjämnare</span><span class="sxs-lookup"><span data-stu-id="c1895-107">Create an Azure load balancer</span></span>
> * <span data-ttu-id="c1895-108">Skapa en belastningsutjämnaren, hälsoavsökningen</span><span class="sxs-lookup"><span data-stu-id="c1895-108">Create a load balancer health probe</span></span>
> * <span data-ttu-id="c1895-109">Skapa regler för nätverkstrafik för belastningsutjämnare</span><span class="sxs-lookup"><span data-stu-id="c1895-109">Create load balancer traffic rules</span></span>
> * <span data-ttu-id="c1895-110">Använd hello tillägget för anpassat skript toocreate en grundläggande IIS-webbplats</span><span class="sxs-lookup"><span data-stu-id="c1895-110">Use hello Custom Script Extension toocreate a basic IIS site</span></span>
> * <span data-ttu-id="c1895-111">Skapa virtuella datorer och bifoga tooa belastningsutjämnare</span><span class="sxs-lookup"><span data-stu-id="c1895-111">Create virtual machines and attach tooa load balancer</span></span>
> * <span data-ttu-id="c1895-112">Visa en belastningsutjämnare i praktiken</span><span class="sxs-lookup"><span data-stu-id="c1895-112">View a load balancer in action</span></span>
> * <span data-ttu-id="c1895-113">Lägga till och ta bort virtuella datorer från en belastningsutjämnare</span><span class="sxs-lookup"><span data-stu-id="c1895-113">Add and remove VMs from a load balancer</span></span>

<span data-ttu-id="c1895-114">Den här kursen kräver hello Azure PowerShell module 3,6 eller senare.</span><span class="sxs-lookup"><span data-stu-id="c1895-114">This tutorial requires hello Azure PowerShell module version 3.6 or later.</span></span> <span data-ttu-id="c1895-115">Kör ` Get-Module -ListAvailable AzureRM` toofind hello version.</span><span class="sxs-lookup"><span data-stu-id="c1895-115">Run ` Get-Module -ListAvailable AzureRM` toofind hello version.</span></span> <span data-ttu-id="c1895-116">Om du behöver tooupgrade finns [installera Azure PowerShell-modulen](/powershell/azure/install-azurerm-ps).</span><span class="sxs-lookup"><span data-stu-id="c1895-116">If you need tooupgrade, see [Install Azure PowerShell module](/powershell/azure/install-azurerm-ps).</span></span>


## <a name="azure-load-balancer-overview"></a><span data-ttu-id="c1895-117">Översikt över Azure belastningen belastningsutjämnare</span><span class="sxs-lookup"><span data-stu-id="c1895-117">Azure load balancer overview</span></span>
<span data-ttu-id="c1895-118">En Azure belastningsutjämnare är en belastningsutjämnare för Layer-4 (TCP, UDP) som ger hög tillgänglighet genom att distribuera inkommande trafik mellan felfri virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="c1895-118">An Azure load balancer is a Layer-4 (TCP, UDP) load balancer that provides high availability by distributing incoming traffic among healthy VMs.</span></span> <span data-ttu-id="c1895-119">En belastningsutjämnaren, hälsoavsökningen övervakar en viss port på varje virtuell dator och distribuerar endast trafik tooan operativa VM.</span><span class="sxs-lookup"><span data-stu-id="c1895-119">A load balancer health probe monitors a given port on each VM and only distributes traffic tooan operational VM.</span></span>

<span data-ttu-id="c1895-120">Du kan definiera en frontend IP-konfiguration som innehåller en eller flera offentliga IP-adresser.</span><span class="sxs-lookup"><span data-stu-id="c1895-120">You define a front-end IP configuration that contains one or more public IP addresses.</span></span> <span data-ttu-id="c1895-121">Den här frontend IP-konfiguration kan din load balancer och program toobe tillgänglig via hello Internet.</span><span class="sxs-lookup"><span data-stu-id="c1895-121">This front-end IP configuration allows your load balancer and applications toobe accessible over hello Internet.</span></span> 

<span data-ttu-id="c1895-122">Virtuella datorer ansluta tooa belastningsutjämning med hjälp av deras virtuella nätverksgränssnittskortet (NIC).</span><span class="sxs-lookup"><span data-stu-id="c1895-122">Virtual machines connect tooa load balancer using their virtual network interface card (NIC).</span></span> <span data-ttu-id="c1895-123">toodistribute trafik toohello virtuella datorer, en backend-adresspool innehåller hello IP-adresser för hello virtuella (NIC) anslutna toohello belastningsutjämnare.</span><span class="sxs-lookup"><span data-stu-id="c1895-123">toodistribute traffic toohello VMs, a back-end address pool contains hello IP addresses of hello virtual (NICs) connected toohello load balancer.</span></span>

<span data-ttu-id="c1895-124">toocontrol hello trafikflödet, definiera regler för inläsning av belastningsutjämning för specifika portar och protokoll som mappar tooyour virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="c1895-124">toocontrol hello flow of traffic, you define load balancer rules for specific ports and protocols that map tooyour VMs.</span></span>


## <a name="create-azure-load-balancer"></a><span data-ttu-id="c1895-125">Skapa Azure belastningsutjämnare</span><span class="sxs-lookup"><span data-stu-id="c1895-125">Create Azure load balancer</span></span>
<span data-ttu-id="c1895-126">Det här avsnittet beskrivs hur du kan skapa och konfigurera varje komponent av hello belastningsutjämnare.</span><span class="sxs-lookup"><span data-stu-id="c1895-126">This section details how you can create and configure each component of hello load balancer.</span></span> <span data-ttu-id="c1895-127">Innan du kan skapa din belastningsutjämnare, skapa en resursgrupp med [New-AzureRmResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup).</span><span class="sxs-lookup"><span data-stu-id="c1895-127">Before you can create your load balancer, create a resource group with [New-AzureRmResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup).</span></span> <span data-ttu-id="c1895-128">hello följande exempel skapar en resursgrupp med namnet *myResourceGroupLoadBalancer* i hello *EastUS* plats:</span><span class="sxs-lookup"><span data-stu-id="c1895-128">hello following example creates a resource group named *myResourceGroupLoadBalancer* in hello *EastUS* location:</span></span>

```powershell
New-AzureRmResourceGroup `
  -ResourceGroupName myResourceGroupLoadBalancer `
  -Location EastUS
```

### <a name="create-a-public-ip-address"></a><span data-ttu-id="c1895-129">Skapa en offentlig IP-adress</span><span class="sxs-lookup"><span data-stu-id="c1895-129">Create a public IP address</span></span>
<span data-ttu-id="c1895-130">tooaccess din app på hello Internet, måste en offentlig IP-adress för hello belastningsutjämnaren.</span><span class="sxs-lookup"><span data-stu-id="c1895-130">tooaccess your app on hello Internet, you need a public IP address for hello load balancer.</span></span> <span data-ttu-id="c1895-131">Skapa en offentlig IP-adress med [ny AzureRmPublicIpAddress](/powershell/module/azurerm.network/new-azurermpublicipaddress).</span><span class="sxs-lookup"><span data-stu-id="c1895-131">Create a public IP address with [New-AzureRmPublicIpAddress](/powershell/module/azurerm.network/new-azurermpublicipaddress).</span></span> <span data-ttu-id="c1895-132">hello följande exempel skapas en offentlig IP-adress med namnet *myPublicIP* i hello *myResourceGroupLoadBalancer* resursgrupp:</span><span class="sxs-lookup"><span data-stu-id="c1895-132">hello following example creates a public IP address named *myPublicIP* in hello *myResourceGroupLoadBalancer* resource group:</span></span>

```powershell
$publicIP = New-AzureRmPublicIpAddress `
  -ResourceGroupName myResourceGroupLoadBalancer `
  -Location EastUS `
  -AllocationMethod Static `
  -Name myPublicIP
```

### <a name="create-a-load-balancer"></a><span data-ttu-id="c1895-133">Skapa en belastningsutjämnare</span><span class="sxs-lookup"><span data-stu-id="c1895-133">Create a load balancer</span></span>
<span data-ttu-id="c1895-134">Skapa en IP-adress för klientdel med [ny AzureRmLoadBalancerFrontendIpConfig](/powershell/module/azurerm.network/new-azurermloadbalancerfrontendipconfig).</span><span class="sxs-lookup"><span data-stu-id="c1895-134">Create a frontend IP address with [New-AzureRmLoadBalancerFrontendIpConfig](/powershell/module/azurerm.network/new-azurermloadbalancerfrontendipconfig).</span></span> <span data-ttu-id="c1895-135">hello följande exempel skapar en IP-adress för klientdel som heter *myFrontEndPool*:</span><span class="sxs-lookup"><span data-stu-id="c1895-135">hello following example creates a frontend IP address named *myFrontEndPool*:</span></span> 

```powershell
$frontendIP = New-AzureRmLoadBalancerFrontendIpConfig `
  -Name myFrontEndPool `
  -PublicIpAddress $publicIP
```

<span data-ttu-id="c1895-136">Skapa en backend-adresspool med [ny AzureRmLoadBalancerBackendAddressPoolConfig](/powershell/module/azurerm.network/new-azurermloadbalancerbackendaddresspoolconfig).</span><span class="sxs-lookup"><span data-stu-id="c1895-136">Create a backend address pool with [New-AzureRmLoadBalancerBackendAddressPoolConfig](/powershell/module/azurerm.network/new-azurermloadbalancerbackendaddresspoolconfig).</span></span> <span data-ttu-id="c1895-137">hello följande exempel skapas en backend-adresspool med namnet *myBackEndPool*:</span><span class="sxs-lookup"><span data-stu-id="c1895-137">hello following example creates a backend address pool named *myBackEndPool*:</span></span>

```powershell
$backendPool = New-AzureRmLoadBalancerBackendAddressPoolConfig -Name myBackEndPool
```

<span data-ttu-id="c1895-138">Nu ska du skapa hello belastningsutjämnare med [ny AzureRmLoadBalancer](/powershell/module/azurerm.network/new-azurermloadbalancer).</span><span class="sxs-lookup"><span data-stu-id="c1895-138">Now, create hello load balancer with [New-AzureRmLoadBalancer](/powershell/module/azurerm.network/new-azurermloadbalancer).</span></span> <span data-ttu-id="c1895-139">hello följande exempel skapas en belastningsutjämnare med namnet *myLoadBalancer* med hello *myPublicIP* adress:</span><span class="sxs-lookup"><span data-stu-id="c1895-139">hello following example creates a load balancer named *myLoadBalancer* using hello *myPublicIP* address:</span></span>

```powershell
$lb = New-AzureRmLoadBalancer `
  -ResourceGroupName myResourceGroupLoadBalancer `
  -Name myLoadBalancer `
  -Location EastUS `
  -FrontendIpConfiguration $frontendIP `
  -BackendAddressPool $backendPool
```

### <a name="create-a-health-probe"></a><span data-ttu-id="c1895-140">Skapa en hälsoavsökningen</span><span class="sxs-lookup"><span data-stu-id="c1895-140">Create a health probe</span></span>
<span data-ttu-id="c1895-141">tooallow hello belastningen belastningsutjämnaren toomonitor hello statusen för din app, använda en hälsoavsökningen.</span><span class="sxs-lookup"><span data-stu-id="c1895-141">tooallow hello load balancer toomonitor hello status of your app, you use a health probe.</span></span> <span data-ttu-id="c1895-142">Hej hälsoavsökningen dynamiskt lägger till eller tar bort virtuella datorer från hello belastningen belastningsutjämnaren rotation baserat på deras svar toohealth kontroller.</span><span class="sxs-lookup"><span data-stu-id="c1895-142">hello health probe dynamically adds or removes VMs from hello load balancer rotation based on their response toohealth checks.</span></span> <span data-ttu-id="c1895-143">En virtuell dator tas bort från hello belastningsutjämnaren belastningsdistribution efter två på varandra följande fel intervaller 15 sekunder som standard.</span><span class="sxs-lookup"><span data-stu-id="c1895-143">By default, a VM is removed from hello load balancer distribution after two consecutive failures at 15-second intervals.</span></span> <span data-ttu-id="c1895-144">Du skapar en hälsoavsökningen baserat på ett protokoll eller ett visst hälsotillstånd sidan för kontroll för din app.</span><span class="sxs-lookup"><span data-stu-id="c1895-144">You create a health probe based on a protocol or a specific health check page for your app.</span></span> 

<span data-ttu-id="c1895-145">hello följande exempel skapar en TCP-avsökning.</span><span class="sxs-lookup"><span data-stu-id="c1895-145">hello following example creates a TCP probe.</span></span> <span data-ttu-id="c1895-146">Du kan också skapa anpassade HTTP-avsökningar mer detaljerade hälsokontroller.</span><span class="sxs-lookup"><span data-stu-id="c1895-146">You can also create custom HTTP probes for more fine grained health checks.</span></span> <span data-ttu-id="c1895-147">När du använder en anpassad HTTP-avsökningen, måste du skapa hello hälsa sidan kontroll som *healthcheck.aspx*.</span><span class="sxs-lookup"><span data-stu-id="c1895-147">When using a custom HTTP probe, you must create hello health check page, such as *healthcheck.aspx*.</span></span> <span data-ttu-id="c1895-148">hello avsökning måste returnera ett **HTTP 200 OK** svar för hello belastningen belastningsutjämnaren tookeep hello värd i rotation.</span><span class="sxs-lookup"><span data-stu-id="c1895-148">hello probe must return an **HTTP 200 OK** response for hello load balancer tookeep hello host in rotation.</span></span>

<span data-ttu-id="c1895-149">toocreate en TCP-hälsoavsökningen du använder [Lägg till AzureRmLoadBalancerProbeConfig](/powershell/module/azurerm.network/add-azurermloadbalancerprobeconfig).</span><span class="sxs-lookup"><span data-stu-id="c1895-149">toocreate a TCP health probe, you use [Add-AzureRmLoadBalancerProbeConfig](/powershell/module/azurerm.network/add-azurermloadbalancerprobeconfig).</span></span> <span data-ttu-id="c1895-150">hello följande exempel skapas en hälsoavsökningen med namnet *myHealthProbe* som övervakar varje virtuell dator:</span><span class="sxs-lookup"><span data-stu-id="c1895-150">hello following example creates a health probe named *myHealthProbe* that monitors each VM:</span></span>

```powershell
Add-AzureRmLoadBalancerProbeConfig `
  -Name myHealthProbe `
  -LoadBalancer $lb `
  -Protocol tcp `
  -Port 80 `
  -IntervalInSeconds 15 `
  -ProbeCount 2
```

<span data-ttu-id="c1895-151">Uppdatera hello belastningsutjämnare med [Set AzureRmLoadBalancer](/powershell/module/azurerm.network/set-azurermloadbalancer):</span><span class="sxs-lookup"><span data-stu-id="c1895-151">Update hello load balancer with [Set-AzureRmLoadBalancer](/powershell/module/azurerm.network/set-azurermloadbalancer):</span></span>

```powershell
Set-AzureRmLoadBalancer -LoadBalancer $lb
```

### <a name="create-a-load-balancer-rule"></a><span data-ttu-id="c1895-152">Skapa en regel för belastningsutjämnare</span><span class="sxs-lookup"><span data-stu-id="c1895-152">Create a load balancer rule</span></span>
<span data-ttu-id="c1895-153">En regel för belastningsutjämnare är används toodefine hur trafiken är distribuerade toohello virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="c1895-153">A load balancer rule is used toodefine how traffic is distributed toohello VMs.</span></span> <span data-ttu-id="c1895-154">Du kan definiera hello frontend IP-konfiguration för hello inkommande trafik och hello backend-IP-adresspool tooreceive hello trafik, tillsammans med hello krävs käll- och målport.</span><span class="sxs-lookup"><span data-stu-id="c1895-154">You define hello front-end IP configuration for hello incoming traffic and hello back-end IP pool tooreceive hello traffic, along with hello required source and destination port.</span></span> <span data-ttu-id="c1895-155">toomake att endast felfri virtuella datorer ta emot trafik, du också definiera hello hälsa avsökningen toouse.</span><span class="sxs-lookup"><span data-stu-id="c1895-155">toomake sure only healthy VMs receive traffic, you also define hello health probe toouse.</span></span>

<span data-ttu-id="c1895-156">Skapa en regel för belastningsutjämnare med [Lägg till AzureRmLoadBalancerRuleConfig](/powershell/module/azurerm.network/add-azurermloadbalancerruleconfig).</span><span class="sxs-lookup"><span data-stu-id="c1895-156">Create a load balancer rule with [Add-AzureRmLoadBalancerRuleConfig](/powershell/module/azurerm.network/add-azurermloadbalancerruleconfig).</span></span> <span data-ttu-id="c1895-157">hello följande exempel skapas en regel för belastningsutjämnare med namnet *myLoadBalancerRule* och balanserar trafik på port *80*:</span><span class="sxs-lookup"><span data-stu-id="c1895-157">hello following example creates a load balancer rule named *myLoadBalancerRule* and balances traffic on port *80*:</span></span>

```powershell
$probe = Get-AzureRmLoadBalancerProbeConfig -LoadBalancer $lb -Name myHealthProbe

Add-AzureRmLoadBalancerRuleConfig `
  -Name myLoadBalancerRule `
  -LoadBalancer $lb `
  -FrontendIpConfiguration $lb.FrontendIpConfigurations[0] `
  -BackendAddressPool $lb.BackendAddressPools[0] `
  -Protocol Tcp `
  -FrontendPort 80 `
  -BackendPort 80 `
  -Probe $probe
```

<span data-ttu-id="c1895-158">Uppdatera hello belastningsutjämnare med [Set AzureRmLoadBalancer](/powershell/module/azurerm.network/set-azurermloadbalancer):</span><span class="sxs-lookup"><span data-stu-id="c1895-158">Update hello load balancer with [Set-AzureRmLoadBalancer](/powershell/module/azurerm.network/set-azurermloadbalancer):</span></span>

```powershell
Set-AzureRmLoadBalancer -LoadBalancer $lb
```


## <a name="configure-virtual-network"></a><span data-ttu-id="c1895-159">Konfigurera ett virtuellt nätverk</span><span class="sxs-lookup"><span data-stu-id="c1895-159">Configure virtual network</span></span>
<span data-ttu-id="c1895-160">Innan du distribuerar vissa virtuella datorer och testa din belastningsutjämnare, skapa hello stöder nätverksresurser på virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="c1895-160">Before you deploy some VMs and can test your balancer, create hello supporting virtual network resources.</span></span> <span data-ttu-id="c1895-161">Mer information om virtuella nätverk finns hello [hantera virtuella Azure-nätverk](tutorial-virtual-network.md) kursen.</span><span class="sxs-lookup"><span data-stu-id="c1895-161">For more information about virtual networks, see hello [Manage Azure Virtual Networks](tutorial-virtual-network.md) tutorial.</span></span>

### <a name="create-network-resources"></a><span data-ttu-id="c1895-162">Skapa nätverksresurser</span><span class="sxs-lookup"><span data-stu-id="c1895-162">Create network resources</span></span>
<span data-ttu-id="c1895-163">Skapa ett virtuellt nätverk med [New-AzureRmVirtualNetwork](/powershell/module/azurerm.network/new-azurermvirtualnetwork).</span><span class="sxs-lookup"><span data-stu-id="c1895-163">Create a virtual network with [New-AzureRmVirtualNetwork](/powershell/module/azurerm.network/new-azurermvirtualnetwork).</span></span> <span data-ttu-id="c1895-164">hello följande exempel skapas ett virtuellt nätverk med namnet *myVnet* med *mySubnet*:</span><span class="sxs-lookup"><span data-stu-id="c1895-164">hello following example creates a virtual network named *myVnet* with *mySubnet*:</span></span>

```powershell
# Create subnet config
$subnetConfig = New-AzureRmVirtualNetworkSubnetConfig `
  -Name mySubnet `
  -AddressPrefix 192.168.1.0/24

# Create hello virtual network
$vnet = New-AzureRmVirtualNetwork `
  -ResourceGroupName myResourceGroupLoadBalancer `
  -Location EastUS `
  -Name myVnet `
  -AddressPrefix 192.168.0.0/16 `
  -Subnet $subnetConfig
```

<span data-ttu-id="c1895-165">Skapa en grupp nätverkssäkerhetsregeln med [ny AzureRmNetworkSecurityRuleConfig](/powershell/module/azurerm.network/new-azurermnetworksecurityruleconfig), skapa en nätverkssäkerhetsgrupp med [ny AzureRmNetworkSecurityGroup](/powershell/module/azurerm.network/new-azurermnetworksecuritygroup).</span><span class="sxs-lookup"><span data-stu-id="c1895-165">Create a network security group rule with [New-AzureRmNetworkSecurityRuleConfig](/powershell/module/azurerm.network/new-azurermnetworksecurityruleconfig), then create a network security group with [New-AzureRmNetworkSecurityGroup](/powershell/module/azurerm.network/new-azurermnetworksecuritygroup).</span></span> <span data-ttu-id="c1895-166">Lägg till hello säkerhet grupp toohello undernät med [Set AzureRmVirtualNetworkSubnetConfig](/powershell/module/azurerm.network/set-azurermvirtualnetworksubnetconfig) och uppdatera sedan hello virtuellt nätverk med [Set-AzureRmVirtualNetwork](/powershell/module/azurerm.network/set-azurermvirtualnetwork).</span><span class="sxs-lookup"><span data-stu-id="c1895-166">Add hello network security group toohello subnet with [Set-AzureRmVirtualNetworkSubnetConfig](/powershell/module/azurerm.network/set-azurermvirtualnetworksubnetconfig) and then update hello virtual network with [Set-AzureRmVirtualNetwork](/powershell/module/azurerm.network/set-azurermvirtualnetwork).</span></span> 

<span data-ttu-id="c1895-167">hello följande exempel skapar en grupp nätverkssäkerhetsregeln med namnet *myNetworkSecurityGroup* och omfattar även*mySubnet*:</span><span class="sxs-lookup"><span data-stu-id="c1895-167">hello following example creates a network security group rule named *myNetworkSecurityGroup* and applies it too*mySubnet*:</span></span>

```powershell
# Create security rule config
$nsgRule = New-AzureRmNetworkSecurityRuleConfig `
  -Name myNetworkSecurityGroupRule `
  -Protocol Tcp `
  -Direction Inbound `
  -Priority 1001 `
  -SourceAddressPrefix * `
  -SourcePortRange * `
  -DestinationAddressPrefix * `
  -DestinationPortRange 80 `
  -Access Allow

# Create hello network security group
$nsg = New-AzureRmNetworkSecurityGroup `
  -ResourceGroupName myResourceGroupLoadBalancer `
  -Location EastUS `
  -Name myNetworkSecurityGroup `
  -SecurityRules $nsgRule

# Apply hello network security group tooa subnet
Set-AzureRmVirtualNetworkSubnetConfig `
  -VirtualNetwork $vnet `
  -Name mySubnet `
  -NetworkSecurityGroup $nsg `
  -AddressPrefix 192.168.1.0/24

# Update hello virtual network
Set-AzureRmVirtualNetwork -VirtualNetwork $vnet
```

<span data-ttu-id="c1895-168">Virtuella nätverkskort skapas med [ny AzureRmNetworkInterface](/powershell/module/azurerm.network/new-azurermnetworkinterface).</span><span class="sxs-lookup"><span data-stu-id="c1895-168">Virtual NICs are created with [New-AzureRmNetworkInterface](/powershell/module/azurerm.network/new-azurermnetworkinterface).</span></span> <span data-ttu-id="c1895-169">hello skapas följande exempel tre virtuella nätverkskort.</span><span class="sxs-lookup"><span data-stu-id="c1895-169">hello following example creates three virtual NICs.</span></span> <span data-ttu-id="c1895-170">(Ett virtuellt nätverkskort för varje virtuell dator som du skapar för din app i hello följande steg).</span><span class="sxs-lookup"><span data-stu-id="c1895-170">(One virtual NIC for each VM you create for your app in hello following steps).</span></span> <span data-ttu-id="c1895-171">Du kan skapa ytterligare virtuella nätverkskort och virtuella datorer när som helst och lägga till dem toohello belastningsutjämnare:</span><span class="sxs-lookup"><span data-stu-id="c1895-171">You can create additional virtual NICs and VMs at any time and add them toohello load balancer:</span></span>

```powershell
for ($i=1; $i -le 3; $i++)
{
   New-AzureRmNetworkInterface `
     -ResourceGroupName myResourceGroupLoadBalancer `
     -Name myNic$i `
     -Location EastUS `
     -Subnet $vnet.Subnets[0] `
     -LoadBalancerBackendAddressPool $lb.BackendAddressPools[0]
}
```

## <a name="create-virtual-machines"></a><span data-ttu-id="c1895-172">Skapa virtuella datorer</span><span class="sxs-lookup"><span data-stu-id="c1895-172">Create virtual machines</span></span>
<span data-ttu-id="c1895-173">tooimprove hello hög tillgänglighet för din app, placera dina virtuella datorer i en tillgänglighetsuppsättning.</span><span class="sxs-lookup"><span data-stu-id="c1895-173">tooimprove hello high availability of your app, place your VMs in an availability set.</span></span>

<span data-ttu-id="c1895-174">Skapa en tillgänglighetsuppsättning med [ny AzureRmAvailabilitySet](/powershell/module/azurerm.compute/new-azurermavailabilityset).</span><span class="sxs-lookup"><span data-stu-id="c1895-174">Create an availability set with [New-AzureRmAvailabilitySet](/powershell/module/azurerm.compute/new-azurermavailabilityset).</span></span> <span data-ttu-id="c1895-175">hello följande exempel skapas en tillgänglighetsuppsättning namngivna *myAvailabilitySet*:</span><span class="sxs-lookup"><span data-stu-id="c1895-175">hello following example creates an availability set named *myAvailabilitySet*:</span></span>

```powershell
$availabilitySet = New-AzureRmAvailabilitySet `
  -ResourceGroupName myResourceGroupLoadBalancer `
  -Name myAvailabilitySet `
  -Location EastUS `
  -Managed `
  -PlatformFaultDomainCount 3 `
  -PlatformUpdateDomainCount 2
```

<span data-ttu-id="c1895-176">Ange en administratör användarnamn och lösenord för hello virtuella datorer med [Get-Credential](https://msdn.microsoft.com/powershell/reference/5.1/microsoft.powershell.security/Get-Credential):</span><span class="sxs-lookup"><span data-stu-id="c1895-176">Set an administrator username and password for hello VMs with [Get-Credential](https://msdn.microsoft.com/powershell/reference/5.1/microsoft.powershell.security/Get-Credential):</span></span>

```powershell
$cred = Get-Credential
```

<span data-ttu-id="c1895-177">Nu kan du skapa hello virtuella datorer med [ny AzureRmVM](/powershell/module/azurerm.compute/new-azurermvm).</span><span class="sxs-lookup"><span data-stu-id="c1895-177">Now you can create hello VMs with [New-AzureRmVM](/powershell/module/azurerm.compute/new-azurermvm).</span></span> <span data-ttu-id="c1895-178">hello följande exempel skapas tre virtuella datorer:</span><span class="sxs-lookup"><span data-stu-id="c1895-178">hello following example creates three VMs:</span></span>

```powershell
for ($i=1; $i -le 3; $i++)
{
  $vm = New-AzureRmVMConfig `
    -VMName myVM$i `
    -VMSize Standard_D1 `
    -AvailabilitySetId $availabilitySet.Id
  $vm = Set-AzureRmVMOperatingSystem `
    -VM $vm `
    -Windows `
    -ComputerName myVM$i `
    -Credential $cred `
    -ProvisionVMAgent `
    -EnableAutoUpdate
  $vm = Set-AzureRmVMSourceImage `
    -VM $vm `
    -PublisherName MicrosoftWindowsServer `
    -Offer WindowsServer `
    -Skus 2016-Datacenter `
    -Version latest
  $vm = Set-AzureRmVMOSDisk `
    -VM $vm `
    -Name myOsDisk$i `
    -DiskSizeInGB 128 `
    -CreateOption FromImage `
    -Caching ReadWrite
  $nic = Get-AzureRmNetworkInterface `
    -ResourceGroupName myResourceGroupLoadBalancer `
    -Name myNic$i
  $vm = Add-AzureRmVMNetworkInterface -VM $vm -Id $nic.Id
  New-AzureRmVM `
    -ResourceGroupName myResourceGroupLoadBalancer `
    -Location EastUS `
    -VM $vm
}
```

<span data-ttu-id="c1895-179">Det tar några minuter toocreate och konfigurera alla tre virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="c1895-179">It takes a few minutes toocreate and configure all three VMs.</span></span>

### <a name="install-iis-with-custom-script-extension"></a><span data-ttu-id="c1895-180">Installera IIS med tillägget för anpassat skript</span><span class="sxs-lookup"><span data-stu-id="c1895-180">Install IIS with Custom Script Extension</span></span>
<span data-ttu-id="c1895-181">I en tidigare självstudiekurs om [hur toocustomize Windows-dator](tutorial-automate-vm-deployment.md), du har lärt dig hur tooautomate VM anpassning med hello anpassade skript tillägget för Windows.</span><span class="sxs-lookup"><span data-stu-id="c1895-181">In a previous tutorial on [How toocustomize a Windows virtual machine](tutorial-automate-vm-deployment.md), you learned how tooautomate VM customization with hello Custom Script Extension for Windows.</span></span> <span data-ttu-id="c1895-182">Du kan använda hello samma hanterar tooinstall och konfigurera IIS på dina virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="c1895-182">You can use hello same approach tooinstall and configure IIS on your VMs.</span></span>

<span data-ttu-id="c1895-183">Använd [Set AzureRmVMExtension](/powershell/module/azurerm.compute/set-azurermvmextension) tooinstall hello tillägget för anpassat skript.</span><span class="sxs-lookup"><span data-stu-id="c1895-183">Use [Set-AzureRmVMExtension](/powershell/module/azurerm.compute/set-azurermvmextension) tooinstall hello Custom Script Extension.</span></span> <span data-ttu-id="c1895-184">Hej tillägget körs `powershell Add-WindowsFeature Web-Server` tooinstall hello IIS-webbserver och uppdateringar hello *Default.htm* sidan tooshow hello värdnamnet för hello VM:</span><span class="sxs-lookup"><span data-stu-id="c1895-184">hello extension runs `powershell Add-WindowsFeature Web-Server` tooinstall hello IIS webserver and then updates hello *Default.htm* page tooshow hello hostname of hello VM:</span></span>

```powershell
for ($i=1; $i -le 3; $i++)
{
   Set-AzureRmVMExtension `
     -ResourceGroupName myResourceGroupLoadBalancer `
     -ExtensionName IIS `
     -VMName myVM$i `
     -Publisher Microsoft.Compute `
     -ExtensionType CustomScriptExtension `
     -TypeHandlerVersion 1.4 `
     -SettingString '{"commandToExecute":"powershell Add-WindowsFeature Web-Server; powershell Add-Content -Path \"C:\\inetpub\\wwwroot\\Default.htm\" -Value $($env:computername)"}' `
     -Location EastUS
}
```

## <a name="test-load-balancer"></a><span data-ttu-id="c1895-185">Testa belastningsutjämnare</span><span class="sxs-lookup"><span data-stu-id="c1895-185">Test load balancer</span></span>
<span data-ttu-id="c1895-186">Hämta hello offentliga IP-adressen för din belastningsutjämnare med [Get-AzureRmPublicIPAddress](/powershell/module/azurerm.network/get-azurermpublicipaddress).</span><span class="sxs-lookup"><span data-stu-id="c1895-186">Obtain hello public IP address of your load balancer with [Get-AzureRmPublicIPAddress](/powershell/module/azurerm.network/get-azurermpublicipaddress).</span></span> <span data-ttu-id="c1895-187">hello följande exempel hämtar hello IP-adress för *myPublicIP* skapade tidigare:</span><span class="sxs-lookup"><span data-stu-id="c1895-187">hello following example obtains hello IP address for *myPublicIP* created earlier:</span></span>

```powershell
Get-AzureRmPublicIPAddress `
  -ResourceGroupName myResourceGroupLoadBalancer `
  -Name myPublicIP | select IpAddress
```

<span data-ttu-id="c1895-188">Du kan sedan ange hello offentliga IP-adressen i tooa webbläsare.</span><span class="sxs-lookup"><span data-stu-id="c1895-188">You can then enter hello public IP address in tooa web browser.</span></span> <span data-ttu-id="c1895-189">hello webbplatsen visas, inklusive hello värdnamnet för hello VM som hello belastningsutjämnaren distribuerade trafik tooas i följande exempel hello:</span><span class="sxs-lookup"><span data-stu-id="c1895-189">hello website is displayed, including hello hostname of hello VM that hello load balancer distributed traffic tooas in hello following example:</span></span>

![Kör IIS-webbplats](./media/tutorial-load-balancer/running-iis-website.png)

<span data-ttu-id="c1895-191">toosee hello belastningsutjämnare distribuerar trafik över alla tre virtuella datorer som kör appen, du kan framtvinga uppdatera webbläsaren.</span><span class="sxs-lookup"><span data-stu-id="c1895-191">toosee hello load balancer distribute traffic across all three VMs running your app, you can force-refresh your web browser.</span></span>


## <a name="add-and-remove-vms"></a><span data-ttu-id="c1895-192">Lägga till och ta bort virtuella datorer</span><span class="sxs-lookup"><span data-stu-id="c1895-192">Add and remove VMs</span></span>
<span data-ttu-id="c1895-193">Du kan behöva tooperform Underhåll på hello virtuella datorer som kör appen, till exempel installera uppdateringar av OS.</span><span class="sxs-lookup"><span data-stu-id="c1895-193">You may need tooperform maintenance on hello VMs running your app, such as installing OS updates.</span></span> <span data-ttu-id="c1895-194">toodeal med ökat tooyour app som du kan behöva tooadd ytterligare virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="c1895-194">toodeal with increased traffic tooyour app, you may need tooadd additional VMs.</span></span> <span data-ttu-id="c1895-195">Det här avsnittet beskrivs hur du tooremove eller lägga till en virtuell dator från hello belastningsutjämnaren.</span><span class="sxs-lookup"><span data-stu-id="c1895-195">This section shows you how tooremove or add a VM from hello load balancer.</span></span>

### <a name="remove-a-vm-from-hello-load-balancer"></a><span data-ttu-id="c1895-196">Ta bort en virtuell dator från hello belastningsutjämnare</span><span class="sxs-lookup"><span data-stu-id="c1895-196">Remove a VM from hello load balancer</span></span>
<span data-ttu-id="c1895-197">Hämta hello nätverkskort med [Get-AzureRmNetworkInterface](/powershell/module/azurerm.network/get-azurermnetworkinterface), sedan ange hello *LoadBalancerBackendAddressPools* -egenskapen för hello virtuella nätverkskortet för*$null*.</span><span class="sxs-lookup"><span data-stu-id="c1895-197">Get hello network interface card with [Get-AzureRmNetworkInterface](/powershell/module/azurerm.network/get-azurermnetworkinterface), then set hello *LoadBalancerBackendAddressPools* property of hello virtual NIC too*$null*.</span></span> <span data-ttu-id="c1895-198">Slutligen uppdatera hello virtuellt nätverkskort.:</span><span class="sxs-lookup"><span data-stu-id="c1895-198">Finally, update hello virtual NIC.:</span></span>

```powershell
$nic = Get-AzureRmNetworkInterface `
    -ResourceGroupName myResourceGroupLoadBalancer `
    -Name myNic2
$nic.Ipconfigurations[0].LoadBalancerBackendAddressPools=$null
Set-AzureRmNetworkInterface -NetworkInterface $nic
```

<span data-ttu-id="c1895-199">toosee hello belastningsutjämnare distribuerar trafik över hello återstående två virtuella datorer som kör appen du kan framtvinga uppdatera webbläsaren.</span><span class="sxs-lookup"><span data-stu-id="c1895-199">toosee hello load balancer distribute traffic across hello remaining two VMs running your app you can force-refresh your web browser.</span></span> <span data-ttu-id="c1895-200">Du kan nu utföra underhåll på hello VM, till exempel installera uppdateringar av operativsystem eller utföra en VM-omstart.</span><span class="sxs-lookup"><span data-stu-id="c1895-200">You can now perform maintenance on hello VM, such as installing OS updates or performing a VM reboot.</span></span>

### <a name="add-a-vm-toohello-load-balancer"></a><span data-ttu-id="c1895-201">Lägga till en belastningsutjämnare för VM-toohello</span><span class="sxs-lookup"><span data-stu-id="c1895-201">Add a VM toohello load balancer</span></span>
<span data-ttu-id="c1895-202">Efter utföra VM underhåll eller om du behöver tooexpand kapacitet, ange hello *LoadBalancerBackendAddressPools* -egenskapen för hello virtuella nätverkskort toohello *BackendAddressPool* från [ Get-AzureRMLoadBalancer](/powershell/module/azurerm.network/get-azurermloadbalancer):</span><span class="sxs-lookup"><span data-stu-id="c1895-202">After performing VM maintenance, or if you need tooexpand capacity, set hello *LoadBalancerBackendAddressPools* property of hello virtual NIC toohello *BackendAddressPool* from [Get-AzureRMLoadBalancer](/powershell/module/azurerm.network/get-azurermloadbalancer):</span></span>

<span data-ttu-id="c1895-203">Hämta hello belastningsutjämnare:</span><span class="sxs-lookup"><span data-stu-id="c1895-203">Get hello load balancer:</span></span>

```powershell
$lb = Get-AzureRMLoadBalancer `
    -ResourceGroupName myResourceGroupLoadBalancer `
    -Name myLoadBalancer 
$nic.IpConfigurations[0].LoadBalancerBackendAddressPools=$lb.BackendAddressPools[0]
Set-AzureRmNetworkInterface -NetworkInterface $nic
```

## <a name="next-steps"></a><span data-ttu-id="c1895-204">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="c1895-204">Next steps</span></span>

<span data-ttu-id="c1895-205">I den här självstudiekursen, skapas en belastningsutjämnare och kopplade tooit för virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="c1895-205">In this tutorial, you created a load balancer and attached VMs tooit.</span></span> <span data-ttu-id="c1895-206">Du har lärt dig att:</span><span class="sxs-lookup"><span data-stu-id="c1895-206">You learned how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="c1895-207">Skapa en Azure belastningsutjämnare</span><span class="sxs-lookup"><span data-stu-id="c1895-207">Create an Azure load balancer</span></span>
> * <span data-ttu-id="c1895-208">Skapa en belastningsutjämnaren, hälsoavsökningen</span><span class="sxs-lookup"><span data-stu-id="c1895-208">Create a load balancer health probe</span></span>
> * <span data-ttu-id="c1895-209">Skapa regler för nätverkstrafik för belastningsutjämnare</span><span class="sxs-lookup"><span data-stu-id="c1895-209">Create load balancer traffic rules</span></span>
> * <span data-ttu-id="c1895-210">Använd hello tillägget för anpassat skript toocreate en grundläggande IIS-webbplats</span><span class="sxs-lookup"><span data-stu-id="c1895-210">Use hello Custom Script Extension toocreate a basic IIS site</span></span>
> * <span data-ttu-id="c1895-211">Skapa virtuella datorer och bifoga tooa belastningsutjämnare</span><span class="sxs-lookup"><span data-stu-id="c1895-211">Create virtual machines and attach tooa load balancer</span></span>
> * <span data-ttu-id="c1895-212">Visa en belastningsutjämnare i praktiken</span><span class="sxs-lookup"><span data-stu-id="c1895-212">View a load balancer in action</span></span>
> * <span data-ttu-id="c1895-213">Lägga till och ta bort virtuella datorer från en belastningsutjämnare</span><span class="sxs-lookup"><span data-stu-id="c1895-213">Add and remove VMs from a load balancer</span></span>

<span data-ttu-id="c1895-214">I förväg toohello nästa självstudiekurs toolearn hur toomanage VM-nätverk.</span><span class="sxs-lookup"><span data-stu-id="c1895-214">Advance toohello next tutorial toolearn how toomanage VM networking.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="c1895-215">Hantera virtuella datorer och virtuella nätverk</span><span class="sxs-lookup"><span data-stu-id="c1895-215">Manage VMs and virtual networks</span></span>](./tutorial-virtual-network.md)
