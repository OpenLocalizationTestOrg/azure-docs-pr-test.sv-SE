---
title: "aaaTutorial - skapa ett program som har hög tillgänglighet på Azure Virtual Machines | Microsoft Docs"
description: "Lär dig hur toocreate ett program för hög tillgänglighet och säkert över tre virtuella Windows-datorer med en belastningsutjämnare i Azure"
services: virtual-machines-windows
documentationcenter: virtual-machines
author: davidmu1
manager: timlt
editor: tysonn
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 03/30/2017
ms.author: davidmu
ms.openlocfilehash: f9eff96be4f3999651c4108f0334e4eaa1a39c0c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="build-a-load-balanced-highly-available-application-on-windows-virtual-machines-in-azure"></a><span data-ttu-id="a1909-103">Skapa en belastningsutjämnad, hög tillgänglighet program på Windows-datorer i Azure</span><span class="sxs-lookup"><span data-stu-id="a1909-103">Build a load balanced, highly available application on Windows virtual machines in Azure</span></span>

<span data-ttu-id="a1909-104">I den här kursen kan du skapa ett program för hög tillgänglighet som är flexibel toomaintenance händelser.</span><span class="sxs-lookup"><span data-stu-id="a1909-104">In this tutorial, you create a highly available application that is resilient toomaintenance events.</span></span> <span data-ttu-id="a1909-105">hello appen använder en belastningsutjämnare, en tillgänglighetsuppsättning och tre virtuella Windows-datorer (VM).</span><span class="sxs-lookup"><span data-stu-id="a1909-105">hello app uses a load balancer, an availability set, and three Windows virtual machines (VMs).</span></span> <span data-ttu-id="a1909-106">Den här självstudiekursen installerar IIS, även om du kan använda den här självstudiekursen toodeploy med ett annat program framework hello samma komponenter för hög tillgänglighet och riktlinjer.</span><span class="sxs-lookup"><span data-stu-id="a1909-106">This tutorial installs IIS, though you can use this tutorial toodeploy a different application framework using hello same high availability components and guidelines.</span></span> 

## <a name="step-1---azure-prerequisites"></a><span data-ttu-id="a1909-107">Steg 1 – krav för Azure</span><span class="sxs-lookup"><span data-stu-id="a1909-107">Step 1 - Azure prerequisites</span></span>

<span data-ttu-id="a1909-108">toocomplete självstudiekursen, se till att du har installerat hello senaste [Azure PowerShell](/powershell/azure/overview) modul.</span><span class="sxs-lookup"><span data-stu-id="a1909-108">toocomplete this tutorial, make sure that you have installed hello latest [Azure PowerShell](/powershell/azure/overview) module.</span></span>

<span data-ttu-id="a1909-109">Först logga in tooyour Azure-prenumeration med hello Login-AzureRmAccount kommandot och följ hello på skärmen riktningar.</span><span class="sxs-lookup"><span data-stu-id="a1909-109">First, log in tooyour Azure subscription with hello Login-AzureRmAccount command and follow hello on-screen directions.</span></span>

```powershell
Login-AzureRmAccount
```

<span data-ttu-id="a1909-110">En Azure-resursgrupp är en logisk behållare där Azure-resurser distribueras och hanteras.</span><span class="sxs-lookup"><span data-stu-id="a1909-110">An Azure resource group is a logical container into which Azure resources are deployed and managed.</span></span> <span data-ttu-id="a1909-111">Innan du kan skapa andra Azure-resurser, behöver du toocreate en resursgrupp med [New-AzureRmResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup).</span><span class="sxs-lookup"><span data-stu-id="a1909-111">Before you can create any other Azure resources, you need toocreate a resource group with [New-AzureRmResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup).</span></span> <span data-ttu-id="a1909-112">hello följande exempel skapar en resursgrupp med namnet `myResourceGroup` i hello `westeurope` region:</span><span class="sxs-lookup"><span data-stu-id="a1909-112">hello following example creates a resource group named `myResourceGroup` in hello `westeurope` region:</span></span> 

```powershell
New-AzureRmResourceGroup -ResourceGroupName myResourceGroup -Location westeurope
```

## <a name="step-2---create-availability-set"></a><span data-ttu-id="a1909-113">Steg 2 – Skapa tillgänglighetsuppsättning</span><span class="sxs-lookup"><span data-stu-id="a1909-113">Step 2 - Create availability set</span></span>

<span data-ttu-id="a1909-114">Virtuella datorer kan skapa logiska fel och uppdatera domäner.</span><span class="sxs-lookup"><span data-stu-id="a1909-114">Virtual machines can be created across logical fault and update domains.</span></span> <span data-ttu-id="a1909-115">Varje logiska domän representerar en del av maskinvaran i hello underliggande Azure-datacenter.</span><span class="sxs-lookup"><span data-stu-id="a1909-115">Each logical domain represents a portion of hardware in hello underlying Azure datacenter.</span></span> <span data-ttu-id="a1909-116">När du skapar två eller flera virtuella datorer distribueras resurserna beräkning och lagring i dessa domäner.</span><span class="sxs-lookup"><span data-stu-id="a1909-116">When you create two or more VMs, your compute and storage resources are distributed across these domains.</span></span> <span data-ttu-id="a1909-117">Den här distributionen underhåller hello tillgängligheten för din app om en maskinvarukomponent måste underhåll.</span><span class="sxs-lookup"><span data-stu-id="a1909-117">This distribution maintains hello availability of your app if a hardware component needs maintenance.</span></span> <span data-ttu-id="a1909-118">Tillgänglighetsuppsättningar kan du definiera dessa logiska fel- och update-domäner.</span><span class="sxs-lookup"><span data-stu-id="a1909-118">Availability sets let you define these logical fault and update domains.</span></span>

<span data-ttu-id="a1909-119">Skapa en tillgänglighetsuppsättning med [ny AzureRmAvailabilitySet](/powershell/module/azurerm.compute/new-azurermavailabilityset).</span><span class="sxs-lookup"><span data-stu-id="a1909-119">Create an availability set with [New-AzureRmAvailabilitySet](/powershell/module/azurerm.compute/new-azurermavailabilityset).</span></span> <span data-ttu-id="a1909-120">hello följande exempel skapas en tillgänglighetsuppsättning namngivna `myAvailabilitySet`:</span><span class="sxs-lookup"><span data-stu-id="a1909-120">hello following example creates an availability set named `myAvailabilitySet`:</span></span>

```powershell
$availabilitySet = New-AzureRmAvailabilitySet `
  -ResourceGroupName myResourceGroup `
  -Name myAvailabilitySet `
  -Location westeurope `
  -Managed `
  -PlatformFaultDomainCount 3 `
  -PlatformUpdateDomainCount 2
```

## <a name="step-3---create-load-balancer"></a><span data-ttu-id="a1909-121">Steg 3 – skapa belastningen belastningsutjämnare</span><span class="sxs-lookup"><span data-stu-id="a1909-121">Step 3 - Create load balancer</span></span>

<span data-ttu-id="a1909-122">En Azure belastningsutjämnare distribuerar trafik över en uppsättning definierade virtuella datorer med hjälp av regler för inläsning av belastningsutjämnaren.</span><span class="sxs-lookup"><span data-stu-id="a1909-122">An Azure load balancer distributes traffic across a set of defined VMs using load balancer rules.</span></span> <span data-ttu-id="a1909-123">En hälsoavsökningen övervakar en viss port på varje virtuell dator och distribuerar endast trafik tooan operativa VM.</span><span class="sxs-lookup"><span data-stu-id="a1909-123">A health probe monitors a given port on each VM and only distributes traffic tooan operational VM.</span></span>

### <a name="create-public-ip-address"></a><span data-ttu-id="a1909-124">Skapa offentlig IP-adress</span><span class="sxs-lookup"><span data-stu-id="a1909-124">Create public IP address</span></span>

<span data-ttu-id="a1909-125">tooaccess din app på hello Internet, tilldela offentliga IP-adress toohello belastningsutjämning.</span><span class="sxs-lookup"><span data-stu-id="a1909-125">tooaccess your app on hello Internet, assign a public IP address toohello load balancer.</span></span> <span data-ttu-id="a1909-126">Skapa en offentlig IP-adress med [ny AzureRmPublicIpAddress](/powershell/module/azurerm.network/new-azurermpublicipaddress).</span><span class="sxs-lookup"><span data-stu-id="a1909-126">Create a public IP address with [New-AzureRmPublicIpAddress](/powershell/module/azurerm.network/new-azurermpublicipaddress).</span></span> <span data-ttu-id="a1909-127">hello följande exempel skapas en offentlig IP-adress med namnet `myPublicIP`:</span><span class="sxs-lookup"><span data-stu-id="a1909-127">hello following example creates a public IP address named `myPublicIP`:</span></span>

```powershell
$pip = New-AzureRmPublicIpAddress `
  -ResourceGroupName myResourceGroup `
  -Location westeurope `
  -AllocationMethod Static `
  -Name myPublicIP
```

### <a name="create-load-balancer"></a><span data-ttu-id="a1909-128">Skapa belastningsutjämnare</span><span class="sxs-lookup"><span data-stu-id="a1909-128">Create load balancer</span></span>

<span data-ttu-id="a1909-129">Skapa en IP-adress för klientdel med [ny AzureRmLoadBalancerFrontendIpConfig](/powershell/module/azurerm.network/new-azurermloadbalancerfrontendipconfig).</span><span class="sxs-lookup"><span data-stu-id="a1909-129">Create a frontend IP address with [New-AzureRmLoadBalancerFrontendIpConfig](/powershell/module/azurerm.network/new-azurermloadbalancerfrontendipconfig).</span></span> <span data-ttu-id="a1909-130">hello följande exempel skapar en IP-adress för klientdel som heter `myFrontEndPool`:</span><span class="sxs-lookup"><span data-stu-id="a1909-130">hello following example creates a frontend IP address named `myFrontEndPool`:</span></span> 

```powershell
$frontendIP = New-AzureRmLoadBalancerFrontendIpConfig -Name myFrontEndPool -PublicIpAddress $pip
```

<span data-ttu-id="a1909-131">Skapa en backend-adresspool med [ny AzureRmLoadBalancerBackendAddressPoolConfig](/powershell/module/azurerm.network/new-azurermloadbalancerbackendaddresspoolconfig).</span><span class="sxs-lookup"><span data-stu-id="a1909-131">Create a backend address pool with [New-AzureRmLoadBalancerBackendAddressPoolConfig](/powershell/module/azurerm.network/new-azurermloadbalancerbackendaddresspoolconfig).</span></span> <span data-ttu-id="a1909-132">hello följande exempel skapas en backend-adresspool som heter `myBackEndPool`:</span><span class="sxs-lookup"><span data-stu-id="a1909-132">hello following example creates a backend address pool named `myBackEndPool`:</span></span>

```powershell
$backendPool = New-AzureRmLoadBalancerBackendAddressPoolConfig -Name myBackEndPool
```

<span data-ttu-id="a1909-133">Nu ska du skapa hello belastningsutjämnare med [ny AzureRmLoadBalancer](/powershell/module/azurerm.network/new-azurermloadbalancer).</span><span class="sxs-lookup"><span data-stu-id="a1909-133">Now, create hello load balancer with [New-AzureRmLoadBalancer](/powershell/module/azurerm.network/new-azurermloadbalancer).</span></span> <span data-ttu-id="a1909-134">hello följande exempel skapas en belastningsutjämnare med namnet `myLoadBalancer` med hello `myPublicIP` adress:</span><span class="sxs-lookup"><span data-stu-id="a1909-134">hello following example creates a load balancer named `myLoadBalancer` using hello `myPublicIP` address:</span></span>

```powershell
$lb = New-AzureRmLoadBalancer `
  -ResourceGroupName myResourceGroup `
  -Name myLoadBalancer `
  -Location westeurope `
  -FrontendIpConfiguration $frontendIP `
  -BackendAddressPool $backendPool
```

### <a name="create-health-probe"></a><span data-ttu-id="a1909-135">Skapa hälsoavsökningen</span><span class="sxs-lookup"><span data-stu-id="a1909-135">Create health probe</span></span>

<span data-ttu-id="a1909-136">tooallow hello belastningen belastningsutjämnaren toomonitor hello statusen för din app, använda en hälsoavsökningen.</span><span class="sxs-lookup"><span data-stu-id="a1909-136">tooallow hello load balancer toomonitor hello status of your app, you use a health probe.</span></span> <span data-ttu-id="a1909-137">Hej hälsoavsökningen dynamiskt lägger till eller tar bort virtuella datorer från hello belastningen belastningsutjämnaren rotation baserat på deras svar toohealth kontroller.</span><span class="sxs-lookup"><span data-stu-id="a1909-137">hello health probe dynamically adds or removes VMs from hello load balancer rotation based on their response toohealth checks.</span></span> <span data-ttu-id="a1909-138">En virtuell dator tas bort från hello belastningsutjämnaren belastningsdistribution efter två på varandra följande fel intervaller 15 sekunder som standard.</span><span class="sxs-lookup"><span data-stu-id="a1909-138">By default, a VM is removed from hello load balancer distribution after two consecutive failures at 15-second intervals.</span></span>

<span data-ttu-id="a1909-139">Skapa en hälsoavsökningen med [Lägg till AzureRmLoadBalancerProbeConfig](/powershell/module/azurerm.network/add-azurermloadbalancerprobeconfig).</span><span class="sxs-lookup"><span data-stu-id="a1909-139">Create a health probe with [Add-AzureRmLoadBalancerProbeConfig](/powershell/module/azurerm.network/add-azurermloadbalancerprobeconfig).</span></span> <span data-ttu-id="a1909-140">hello följande exempel skapas en hälsoavsökningen med namnet `myHealthProbe` som övervakar varje virtuell dator:</span><span class="sxs-lookup"><span data-stu-id="a1909-140">hello following example creates a health probe named `myHealthProbe` that monitors each VM:</span></span>

```powershell
Add-AzureRmLoadBalancerProbeConfig -Name myHealthProbe `
  -LoadBalancer $lb `
  -Protocol tcp `
  -Port 80 `
  -IntervalInSeconds 15 `
  -ProbeCount 2
```

### <a name="create-load-balancer-rule"></a><span data-ttu-id="a1909-141">Skapa regel för belastningsutjämnare</span><span class="sxs-lookup"><span data-stu-id="a1909-141">Create load balancer rule</span></span>

<span data-ttu-id="a1909-142">En regel för belastningsutjämnare är används toodefine hur trafiken är distribuerade toohello virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="a1909-142">A load balancer rule is used toodefine how traffic is distributed toohello VMs.</span></span>

<span data-ttu-id="a1909-143">Skapa en regel för belastningsutjämnare med [Lägg till AzureRmLoadBalancerRuleConfig](/powershell/module/azurerm.network/add-azurermloadbalancerruleconfig).</span><span class="sxs-lookup"><span data-stu-id="a1909-143">Create a load balancer rule with [Add-AzureRmLoadBalancerRuleConfig](/powershell/module/azurerm.network/add-azurermloadbalancerruleconfig).</span></span> <span data-ttu-id="a1909-144">hello följande exempel skapas en regel för belastningsutjämnare med namnet `myLoadBalancerRule` och balanserar trafik på port `80`:</span><span class="sxs-lookup"><span data-stu-id="a1909-144">hello following example creates a load balancer rule named `myLoadBalancerRule` and balances traffic on port `80`:</span></span>

```powershell
Add-AzureRmLoadBalancerRuleConfig -Name myLoadBalancerRule `
  -LoadBalancer $lb `
  -FrontendIpConfiguration $lb.FrontendIpConfigurations[0] `
  -BackendAddressPool $lb.BackendAddressPools[0] `
  -Protocol Tcp `
  -FrontendPort 80 `
  -BackendPort 80
```

<span data-ttu-id="a1909-145">Uppdatera hello belastningsutjämnare med [Set AzureRmLoadBalancer](/powershell/module/azurerm.network/set-azurermloadbalancer):</span><span class="sxs-lookup"><span data-stu-id="a1909-145">Update hello load balancer with [Set-AzureRmLoadBalancer](/powershell/module/azurerm.network/set-azurermloadbalancer):</span></span>

```powershell
Set-AzureRmLoadBalancer -LoadBalancer $lb
```

## <a name="step-4---configure-networking"></a><span data-ttu-id="a1909-146">Steg 4 – Konfigurera nätverk</span><span class="sxs-lookup"><span data-stu-id="a1909-146">Step 4 - Configure networking</span></span>

<span data-ttu-id="a1909-147">Varje virtuell dator har en eller flera virtuella nätverkskort (NIC) som ansluter tooa virtuellt nätverk.</span><span class="sxs-lookup"><span data-stu-id="a1909-147">Each VM has one or more virtual network interface cards (NICs) that connect tooa virtual network.</span></span> <span data-ttu-id="a1909-148">Det här virtuella nätverket skyddas toofilter trafik baserat på definierade åtkomstregler.</span><span class="sxs-lookup"><span data-stu-id="a1909-148">This virtual network is secured toofilter traffic based on defined access rules.</span></span>

### <a name="create-virtual-network"></a><span data-ttu-id="a1909-149">Skapa det virtuella nätverket</span><span class="sxs-lookup"><span data-stu-id="a1909-149">Create virtual network</span></span>

<span data-ttu-id="a1909-150">Konfigurera först ett undernät med [ny AzureRmVirtualNetworkSubnetConfig](/powershell/module/azurerm.network/new-azurermvirtualnetworksubnetconfig).</span><span class="sxs-lookup"><span data-stu-id="a1909-150">First, configure a subnet with [New-AzureRmVirtualNetworkSubnetConfig](/powershell/module/azurerm.network/new-azurermvirtualnetworksubnetconfig).</span></span> <span data-ttu-id="a1909-151">hello följande exempel skapas ett undernät med namnet `mySubnet`:</span><span class="sxs-lookup"><span data-stu-id="a1909-151">hello following example creates a subnet named `mySubnet`:</span></span>

```powershell
$subnetConfig = New-AzureRmVirtualNetworkSubnetConfig -Name mySubnet -AddressPrefix 192.168.1.0/24
```

<span data-ttu-id="a1909-152">tooprovide network connectivity tooyour virtuella datorer, skapa ett virtuellt nätverk med [New-AzureRmVirtualNetwork](/powershell/module/azurerm.network/new-azurermvirtualnetwork).</span><span class="sxs-lookup"><span data-stu-id="a1909-152">tooprovide network connectivity tooyour VMs, create a virtual network with [New-AzureRmVirtualNetwork](/powershell/module/azurerm.network/new-azurermvirtualnetwork).</span></span> <span data-ttu-id="a1909-153">hello följande exempel skapas ett virtuellt nätverk med namnet `myVnet` med `mySubnet`:</span><span class="sxs-lookup"><span data-stu-id="a1909-153">hello following example creates a virtual network named `myVnet` with `mySubnet`:</span></span>

```powershell
$vnet = New-AzureRmVirtualNetwork `
  -ResourceGroupName myResourceGroup `
  -Location westeurope `
  -Name myVnet `
  -AddressPrefix 192.168.0.0/16 `
  -Subnet $subnetConfig
```

### <a name="configure-network-security"></a><span data-ttu-id="a1909-154">Konfigurera nätverkssäkerhet</span><span class="sxs-lookup"><span data-stu-id="a1909-154">Configure network security</span></span>

<span data-ttu-id="a1909-155">En Azure [nätverkssäkerhetsgruppen](../../virtual-network/virtual-networks-nsg.md) (NSG) styr inkommande och utgående trafik för en eller flera virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="a1909-155">An Azure [network security group](../../virtual-network/virtual-networks-nsg.md) (NSG) controls inbound and outbound traffic for one or many virtual machines.</span></span> <span data-ttu-id="a1909-156">Regler för nätverkssäkerhetsgrupper Tillåt eller neka nätverkstrafik på en specifik port eller ett intervall.</span><span class="sxs-lookup"><span data-stu-id="a1909-156">Network security group rules allow or deny network traffic on a specific port or port range.</span></span> <span data-ttu-id="a1909-157">De här reglerna kan också inkludera en källadress-prefix så att endast trafik i en fördefinierad källa kan kommunicera med en virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="a1909-157">These rules can also include a source address prefix so that only traffic originating at a predefined source can communicate with a virtual machine.</span></span>

<span data-ttu-id="a1909-158">tooallow trafik tooreach din webbapp, skapa en grupp nätverkssäkerhetsregeln med [ny AzureRmNetworkSecurityRuleConfig](/powershell/module/azurerm.network/new-azurermnetworksecurityruleconfig).</span><span class="sxs-lookup"><span data-stu-id="a1909-158">tooallow web traffic tooreach your app, create a network security group rule with [New-AzureRmNetworkSecurityRuleConfig](/powershell/module/azurerm.network/new-azurermnetworksecurityruleconfig).</span></span> <span data-ttu-id="a1909-159">hello följande exempel skapar en grupp nätverkssäkerhetsregeln med namnet `myNetworkSecurityGroupRule`:</span><span class="sxs-lookup"><span data-stu-id="a1909-159">hello following example creates a network security group rule named `myNetworkSecurityGroupRule`:</span></span>

```powershell
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
```

<span data-ttu-id="a1909-160">Skapa en nätverkssäkerhetsgrupp med [ny AzureRmNetworkSecurityGroup](/powershell/module/azurerm.network/new-azurermnetworksecuritygroup).</span><span class="sxs-lookup"><span data-stu-id="a1909-160">Create a network security group with [New-AzureRmNetworkSecurityGroup](/powershell/module/azurerm.network/new-azurermnetworksecuritygroup).</span></span> <span data-ttu-id="a1909-161">hello följande exempel skapas en NSG som heter `myNetworkSecurityGroup`:</span><span class="sxs-lookup"><span data-stu-id="a1909-161">hello following example creates an NSG named `myNetworkSecurityGroup`:</span></span>

```powershell
$nsg = New-AzureRmNetworkSecurityGroup `
  -ResourceGroupName myResourceGroup `
  -Location westeurope `
  -Name myNetworkSecurityGroup `
  -SecurityRules $nsgRule
```

<span data-ttu-id="a1909-162">Lägg till hello säkerhet grupp toohello undernät med [Set AzureRmVirtualNetworkSubnetConfig](/powershell/module/azurerm.network/set-azurermvirtualnetworksubnetconfig):</span><span class="sxs-lookup"><span data-stu-id="a1909-162">Add hello network security group toohello subnet with [Set-AzureRmVirtualNetworkSubnetConfig](/powershell/module/azurerm.network/set-azurermvirtualnetworksubnetconfig):</span></span>

```powershell
Set-AzureRmVirtualNetworkSubnetConfig -VirtualNetwork $vnet `
  -Name mySubnet `
  -NetworkSecurityGroup $nsg `
  -AddressPrefix 192.168.1.0/24
```

<span data-ttu-id="a1909-163">Uppdatera hello virtuellt nätverk med [Set-AzureRmVirtualNetwork](/powershell/module/azurerm.network/set-azurermvirtualnetwork):</span><span class="sxs-lookup"><span data-stu-id="a1909-163">Update hello virtual network with [Set-AzureRmVirtualNetwork](/powershell/module/azurerm.network/set-azurermvirtualnetwork):</span></span>

```powershell
Set-AzureRmVirtualNetwork -VirtualNetwork $vnet
```

### <a name="create-virtual-network-interface-cards"></a><span data-ttu-id="a1909-164">Skapa virtuellt nätverkskort</span><span class="sxs-lookup"><span data-stu-id="a1909-164">Create virtual network interface cards</span></span>

<span data-ttu-id="a1909-165">Läsa in belastningsutjämning funktionen med hello virtuell NIC-resurs i stället för hello faktiska VM.</span><span class="sxs-lookup"><span data-stu-id="a1909-165">Load balancers function with hello virtual NIC resource rather than hello actual VM.</span></span> <span data-ttu-id="a1909-166">hello virtuella nätverkskortet är anslutet toohello belastningsutjämnare och kopplade tooa VM.</span><span class="sxs-lookup"><span data-stu-id="a1909-166">hello virtual NIC is connected toohello load balancer, and then attached tooa VM.</span></span>

<span data-ttu-id="a1909-167">Skapa ett virtuellt nätverkskort med [ny AzureRmNetworkInterface](/powershell/module/azurerm.network/new-azurermnetworkinterface).</span><span class="sxs-lookup"><span data-stu-id="a1909-167">Create a virtual NIC with [New-AzureRmNetworkInterface](/powershell/module/azurerm.network/new-azurermnetworkinterface).</span></span> <span data-ttu-id="a1909-168">hello skapas följande exempel tre virtuella nätverkskort.</span><span class="sxs-lookup"><span data-stu-id="a1909-168">hello following example creates three virtual NICs.</span></span> <span data-ttu-id="a1909-169">(Ett virtuellt nätverkskort för varje virtuell dator som du skapar för din app i hello följande steg):</span><span class="sxs-lookup"><span data-stu-id="a1909-169">(One virtual NIC for each VM you create for your app in hello following steps):</span></span>


```powershell
for ($i=1; $i -le 3; $i++)
{
   New-AzureRmNetworkInterface -ResourceGroupName myResourceGroup `
     -Name myNic$i `
     -Location westeurope `
     -Subnet $vnet.Subnets[0] `
     -LoadBalancerBackendAddressPool $lb.BackendAddressPools[0]
}

```

## <a name="step-5---create-virtual-machines"></a><span data-ttu-id="a1909-170">Steg 5 – Skapa virtuella datorer</span><span class="sxs-lookup"><span data-stu-id="a1909-170">Step 5 - Create virtual machines</span></span>

<span data-ttu-id="a1909-171">Med alla hello underliggande komponenter på plats, kan du nu skapa högtillgängliga virtuella datorer toorun din app.</span><span class="sxs-lookup"><span data-stu-id="a1909-171">With all hello underlying components in place, you can now create highly available VMs toorun your app.</span></span> 

<span data-ttu-id="a1909-172">Hämta hello användarnamn och lösenord för administratörskontot för hello på hello virtuell dator med [Get-Credential](https://msdn.microsoft.com/powershell/reference/5.1/microsoft.powershell.security/Get-Credential):</span><span class="sxs-lookup"><span data-stu-id="a1909-172">Get hello username and password needed for hello administrator account on hello virtual machine with [Get-Credential](https://msdn.microsoft.com/powershell/reference/5.1/microsoft.powershell.security/Get-Credential):</span></span>

```powershell
$cred = Get-Credential
```

<span data-ttu-id="a1909-173">Skapa hello virtuella datorer med [ny AzureRmVMConfig](https://docs.microsoft.com/powershell/resourcemanager/azurerm.compute/v2.8.0/new-azurermvmconfig), [Set AzureRmVMOperatingSystem](https://docs.microsoft.com/powershell/resourcemanager/azurerm.compute/v2.8.0/set-azurermvmoperatingsystem), [Set AzureRmVMSourceImage](https://docs.microsoft.com/powershell/resourcemanager/azurerm.compute/v2.8.0/set-azurermvmsourceimage), [Set-AzureRmVMOSDisk](https://docs.microsoft.com/powershell/resourcemanager/azurerm.compute/v2.8.0/set-azurermvmosdisk), [Lägga till AzureRmVMNetworkInterface](https://docs.microsoft.com/powershell/resourcemanager/azurerm.compute/v2.8.0/add-azurermvmnetworkinterface), och [nya AzureRmVM](/powershell/module/azurerm.compute/new-azurermvm).</span><span class="sxs-lookup"><span data-stu-id="a1909-173">Create hello VMs with [New-AzureRmVMConfig](https://docs.microsoft.com/powershell/resourcemanager/azurerm.compute/v2.8.0/new-azurermvmconfig), [Set-AzureRmVMOperatingSystem](https://docs.microsoft.com/powershell/resourcemanager/azurerm.compute/v2.8.0/set-azurermvmoperatingsystem), [Set-AzureRmVMSourceImage](https://docs.microsoft.com/powershell/resourcemanager/azurerm.compute/v2.8.0/set-azurermvmsourceimage), [Set-AzureRmVMOSDisk](https://docs.microsoft.com/powershell/resourcemanager/azurerm.compute/v2.8.0/set-azurermvmosdisk), [Add-AzureRmVMNetworkInterface](https://docs.microsoft.com/powershell/resourcemanager/azurerm.compute/v2.8.0/add-azurermvmnetworkinterface), and [New-AzureRmVM](/powershell/module/azurerm.compute/new-azurermvm).</span></span> <span data-ttu-id="a1909-174">hello följande exempel skapas tre virtuella datorer:</span><span class="sxs-lookup"><span data-stu-id="a1909-174">hello following example creates three VMs:</span></span>

```powershell
for ($i=1; $i -le 3; $i++)
{
   $vm = New-AzureRmVMConfig -VMName myVM$i -VMSize Standard_D1 -AvailabilitySetId $availabilitySet.Id
   $vm = Set-AzureRmVMOperatingSystem -VM $vm -Windows -ComputerName myVM$i -Credential $cred -ProvisionVMAgent -EnableAutoUpdate
   $vm = Set-AzureRmVMSourceImage -VM $vm -PublisherName MicrosoftWindowsServer -Offer WindowsServer -Skus 2016-Datacenter -Version latest
   $vm = Set-AzureRmVMOSDisk -VM $vm -Name myOsDisk$i -StorageAccountType StandardLRS -DiskSizeInGB 128 -CreateOption FromImage -Caching ReadWrite
   $nic = Get-AzureRmNetworkInterface -ResourceGroupName myResourceGroup -Name myNic$i
   $vm = Add-AzureRmVMNetworkInterface -VM $vm -Id $nic.Id
   New-AzureRmVM -ResourceGroupName myResourceGroup -Location westeurope -VM $vm
}

```

<span data-ttu-id="a1909-175">Det tar flera minuter toocreate och konfigurera alla tre virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="a1909-175">It takes several minutes toocreate and configure all three VMs.</span></span> <span data-ttu-id="a1909-176">hello identifierar belastningsutjämnaren, hälsoavsökningen automatiskt när hello appen körs på varje virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="a1909-176">hello load balancer health probe automatically detects when hello app is running on each VM.</span></span> <span data-ttu-id="a1909-177">När hello appen körs startar hello-regel för belastningsutjämnare toodistribute trafik.</span><span class="sxs-lookup"><span data-stu-id="a1909-177">Once hello app is running, hello load balancer rule starts toodistribute traffic.</span></span>

### <a name="install-hello-app"></a><span data-ttu-id="a1909-178">Installera hello app</span><span class="sxs-lookup"><span data-stu-id="a1909-178">Install hello app</span></span> 

<span data-ttu-id="a1909-179">Tillägg för virtuell dator i Azure är används tooautomate virtuella konfigurationsåtgärder, till exempel installera program och konfigurera hello-operativsystemet.</span><span class="sxs-lookup"><span data-stu-id="a1909-179">Azure virtual machine extensions are used tooautomate virtual machine configuration tasks such as installing applications and configuring hello operating system.</span></span> <span data-ttu-id="a1909-180">Hej [tillägget för anpassat skript för Windows](./../virtual-machines-windows-extensions-customscript.md) är används toorun alla PowerShell-skript på hello virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="a1909-180">hello [custom script extension for Windows](./../virtual-machines-windows-extensions-customscript.md) is used toorun any PowerShell script on hello virtual machine.</span></span> <span data-ttu-id="a1909-181">hello skript kan lagras i Azure storage, alla tillgängliga HTTP-slutpunkten eller inbäddat i hello konfiguration för anpassat skript.</span><span class="sxs-lookup"><span data-stu-id="a1909-181">hello script can be stored in Azure storage, any accessible HTTP endpoint, or embedded in hello custom script extension configuration.</span></span> <span data-ttu-id="a1909-182">När du använder tillägget för anpassat skript för hello hanterar hello skriptkörningen hello Azure VM-agenten.</span><span class="sxs-lookup"><span data-stu-id="a1909-182">When using hello custom script extension, hello Azure VM agent manages hello script execution.</span></span>

<span data-ttu-id="a1909-183">Använd [Set AzureRmVMExtension](/powershell/module/azurerm.compute/set-azurermvmextension) tooinstall hello tillägget för anpassat skript.</span><span class="sxs-lookup"><span data-stu-id="a1909-183">Use [Set-AzureRmVMExtension](/powershell/module/azurerm.compute/set-azurermvmextension) tooinstall hello custom script extension.</span></span> <span data-ttu-id="a1909-184">Hej tillägget körs `powershell Add-WindowsFeature Web-Server` tooinstall hello IIS-webbserver:</span><span class="sxs-lookup"><span data-stu-id="a1909-184">hello extension runs `powershell Add-WindowsFeature Web-Server` tooinstall hello IIS webserver:</span></span>

```powershell
for ($i=1; $i -le 3; $i++)
{
   Set-AzureRmVMExtension -ResourceGroupName myResourceGroup `
     -ExtensionName IIS `
     -VMName myVM$i `
     -Publisher Microsoft.Compute `
     -ExtensionType CustomScriptExtension `
     -TypeHandlerVersion 1.4 `
     -SettingString '{"commandToExecute":"powershell Add-WindowsFeature Web-Server"}' `
     -Location westeurope
}
```

### <a name="test-your-app"></a><span data-ttu-id="a1909-185">Testa din app</span><span class="sxs-lookup"><span data-stu-id="a1909-185">Test your app</span></span>

<span data-ttu-id="a1909-186">Hämta hello offentliga IP-adressen för din belastningsutjämnare med [Get-AzureRmPublicIPAddress](/powershell/module/azurerm.network/get-azurermpublicipaddress).</span><span class="sxs-lookup"><span data-stu-id="a1909-186">Obtain hello public IP address of your load balancer with [Get-AzureRmPublicIPAddress](/powershell/module/azurerm.network/get-azurermpublicipaddress).</span></span> <span data-ttu-id="a1909-187">hello följande exempel hämtar hello IP-adress för `myPublicIP` skapade tidigare:</span><span class="sxs-lookup"><span data-stu-id="a1909-187">hello following example obtains hello IP address for `myPublicIP` created earlier:</span></span>

```powershell
Get-AzureRmPublicIPAddress -ResourceGroupName myResourceGroup -Name myPublicIP | select IpAddress
```

<span data-ttu-id="a1909-188">Ange hello offentliga IP-adressen i tooa webbläsare.</span><span class="sxs-lookup"><span data-stu-id="a1909-188">Enter hello public IP address in tooa web browser.</span></span> <span data-ttu-id="a1909-189">Hello IIS-standardwebbplatsen visas med hello NSG regeln på plats.</span><span class="sxs-lookup"><span data-stu-id="a1909-189">With hello NSG rule in place, hello default IIS website is displayed.</span></span> 

![Standardwebbplatsen i IIS](media/load-balanced-iis-tutorial/iis.png)

## <a name="step-6--management-tasks"></a><span data-ttu-id="a1909-191">Steg 6 – administrativa uppgifter</span><span class="sxs-lookup"><span data-stu-id="a1909-191">Step 6 – Management tasks</span></span>

<span data-ttu-id="a1909-192">Du kan behöva tooperform Underhåll på hello virtuella datorer som kör appen, till exempel installera uppdateringar av OS.</span><span class="sxs-lookup"><span data-stu-id="a1909-192">You may need tooperform maintenance on hello VMs running your app, such as installing OS updates.</span></span> <span data-ttu-id="a1909-193">toodeal med ökat tooyour app som du kan behöva tooadd ytterligare virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="a1909-193">toodeal with increased traffic tooyour app, you may need tooadd additional VMs.</span></span> <span data-ttu-id="a1909-194">Det här avsnittet beskrivs hur du tooremove eller lägga till en virtuell dator från hello belastningsutjämnaren.</span><span class="sxs-lookup"><span data-stu-id="a1909-194">This section shows you how tooremove or add a VM from hello load balancer.</span></span> 

### <a name="remove-a-vm-from-hello-load-balancer"></a><span data-ttu-id="a1909-195">Ta bort en virtuell dator från hello belastningsutjämnare</span><span class="sxs-lookup"><span data-stu-id="a1909-195">Remove a VM from hello load balancer</span></span>

<span data-ttu-id="a1909-196">Ta bort en virtuell dator från hello serverdelen för adresspoolen genom att återställa hello LoadBalancerBackendAddressPools-egenskapen för hello-nätverkskort.</span><span class="sxs-lookup"><span data-stu-id="a1909-196">Remove a VM from hello backend address pool by resetting hello LoadBalancerBackendAddressPools property of hello network interface card.</span></span>

<span data-ttu-id="a1909-197">Hämta hello nätverkskort med [Get-AzureRmNetworkInterface](/powershell/module/azurerm.network/get-azurermnetworkinterface):</span><span class="sxs-lookup"><span data-stu-id="a1909-197">Get hello network interface card with [Get-AzureRmNetworkInterface](/powershell/module/azurerm.network/get-azurermnetworkinterface):</span></span>

```powershell
$nic = Get-AzureRmNetworkInterface -ResourceGroupName myResourceGroup -Name myNic2
``` 

<span data-ttu-id="a1909-198">Egenskapen hello LoadBalancerBackendAddressPools för hello nätverkskort för$ null:</span><span class="sxs-lookup"><span data-stu-id="a1909-198">Set hello LoadBalancerBackendAddressPools property of hello network interface card too$null:</span></span>

```powershell
$nic.Ipconfigurations[0].LoadBalancerBackendAddressPools=$null
```

<span data-ttu-id="a1909-199">Uppdatera hello nätverkskort:</span><span class="sxs-lookup"><span data-stu-id="a1909-199">Update hello network interface card:</span></span>

```powershell
Set-AzureRmNetworkInterface -NetworkInterface $nic
```

### <a name="add-a-vm-toohello-load-balancer"></a><span data-ttu-id="a1909-200">Lägga till en belastningsutjämnare för VM-toohello</span><span class="sxs-lookup"><span data-stu-id="a1909-200">Add a VM toohello load balancer</span></span>

<span data-ttu-id="a1909-201">När du utför VM underhåll eller om du behöver tooexpand kapacitet, lägger du till hello nätverkskort på en VM toohello serverdelsadresspool för belastningsutjämnare hello.</span><span class="sxs-lookup"><span data-stu-id="a1909-201">After performing VM maintenance, or if you need tooexpand capacity, adding hello NIC of a VM toohello backend address pool of hello load balancer.</span></span>

<span data-ttu-id="a1909-202">Hämta hello belastningsutjämnare:</span><span class="sxs-lookup"><span data-stu-id="a1909-202">Get hello load balancer:</span></span>

```powershell
$lb = Get-AzureRMLoadBalancer -ResourceGroupName myResourceGroup -Name myLoadBalancer 
```

<span data-ttu-id="a1909-203">Lägg till hello serverdelsadresspool för hello belastningen belastningsutjämnaren toohello nätverkskort:</span><span class="sxs-lookup"><span data-stu-id="a1909-203">Add hello backend address pool of hello load balancer toohello network interface card:</span></span>

```powershell
$nic.IpConfigurations[0].LoadBalancerBackendAddressPools=$lb.BackendAddressPools[0]
```

<span data-ttu-id="a1909-204">Uppdatera hello nätverkskort:</span><span class="sxs-lookup"><span data-stu-id="a1909-204">Update hello network interface card:</span></span>

```powershell
Set-AzureRmNetworkInterface -NetworkInterface $nic
```

## <a name="next-steps"></a><span data-ttu-id="a1909-205">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="a1909-205">Next Steps</span></span>

<span data-ttu-id="a1909-206">Exempel – [exempel Azure virtuella PowerShell-skript](./../virtual-machines-windows-powershell-samples.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)</span><span class="sxs-lookup"><span data-stu-id="a1909-206">Samples – [Azure Virtual Machine PowerShell sample scripts](./../virtual-machines-windows-powershell-samples.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)</span></span>
