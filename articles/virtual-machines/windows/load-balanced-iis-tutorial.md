---
title: "Självstudiekurs – skapa ett program som har hög tillgänglighet på Azure Virtual Machines | Microsoft Docs"
description: "Lär dig hur du skapar ett program med hög tillgänglighet och säkert över tre virtuella Windows-datorer med en belastningsutjämnare i Azure"
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
ms.openlocfilehash: 4b8690a11ec0e711782a112622e1193c24292289
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="build-a-load-balanced-highly-available-application-on-windows-virtual-machines-in-azure"></a><span data-ttu-id="e8831-103">Skapa en belastningsutjämnad, hög tillgänglighet program på Windows-datorer i Azure</span><span class="sxs-lookup"><span data-stu-id="e8831-103">Build a load balanced, highly available application on Windows virtual machines in Azure</span></span>

<span data-ttu-id="e8831-104">I den här kursen kan du skapa ett program för hög tillgänglighet som är motståndskraftiga mot underhållshändelser.</span><span class="sxs-lookup"><span data-stu-id="e8831-104">In this tutorial, you create a highly available application that is resilient to maintenance events.</span></span> <span data-ttu-id="e8831-105">Appen använder en belastningsutjämnare, en tillgänglighetsuppsättning och tre virtuella Windows-datorer (VM).</span><span class="sxs-lookup"><span data-stu-id="e8831-105">The app uses a load balancer, an availability set, and three Windows virtual machines (VMs).</span></span> <span data-ttu-id="e8831-106">Den här självstudiekursen installerar IIS, även om du kan använda den här självstudiekursen för att distribuera ett programramverk för olika som använder samma komponenter för hög tillgänglighet och riktlinjer.</span><span class="sxs-lookup"><span data-stu-id="e8831-106">This tutorial installs IIS, though you can use this tutorial to deploy a different application framework using the same high availability components and guidelines.</span></span> 

## <a name="step-1---azure-prerequisites"></a><span data-ttu-id="e8831-107">Steg 1 – krav för Azure</span><span class="sxs-lookup"><span data-stu-id="e8831-107">Step 1 - Azure prerequisites</span></span>

<span data-ttu-id="e8831-108">För att slutföra den här självstudiekursen måste du kontrollera att du har installerat senast [Azure PowerShell](/powershell/azure/overview) modul.</span><span class="sxs-lookup"><span data-stu-id="e8831-108">To complete this tutorial, make sure that you have installed the latest [Azure PowerShell](/powershell/azure/overview) module.</span></span>

<span data-ttu-id="e8831-109">Logga in på Azure-prenumerationen med kommandot Login-AzureRmAccount först och följ de på skärmen riktningar.</span><span class="sxs-lookup"><span data-stu-id="e8831-109">First, log in to your Azure subscription with the Login-AzureRmAccount command and follow the on-screen directions.</span></span>

```powershell
Login-AzureRmAccount
```

<span data-ttu-id="e8831-110">En Azure-resursgrupp är en logisk behållare där Azure-resurser distribueras och hanteras.</span><span class="sxs-lookup"><span data-stu-id="e8831-110">An Azure resource group is a logical container into which Azure resources are deployed and managed.</span></span> <span data-ttu-id="e8831-111">Innan du kan skapa andra Azure-resurser, måste du skapa en resursgrupp med [New-AzureRmResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup).</span><span class="sxs-lookup"><span data-stu-id="e8831-111">Before you can create any other Azure resources, you need to create a resource group with [New-AzureRmResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup).</span></span> <span data-ttu-id="e8831-112">I följande exempel skapas en resursgrupp med namnet `myResourceGroup` i den `westeurope` region:</span><span class="sxs-lookup"><span data-stu-id="e8831-112">The following example creates a resource group named `myResourceGroup` in the `westeurope` region:</span></span> 

```powershell
New-AzureRmResourceGroup -ResourceGroupName myResourceGroup -Location westeurope
```

## <a name="step-2---create-availability-set"></a><span data-ttu-id="e8831-113">Steg 2 – Skapa tillgänglighetsuppsättning</span><span class="sxs-lookup"><span data-stu-id="e8831-113">Step 2 - Create availability set</span></span>

<span data-ttu-id="e8831-114">Virtuella datorer kan skapa logiska fel och uppdatera domäner.</span><span class="sxs-lookup"><span data-stu-id="e8831-114">Virtual machines can be created across logical fault and update domains.</span></span> <span data-ttu-id="e8831-115">Varje logiska domän representerar en del av maskinvara i det underliggande Azure-datacentret.</span><span class="sxs-lookup"><span data-stu-id="e8831-115">Each logical domain represents a portion of hardware in the underlying Azure datacenter.</span></span> <span data-ttu-id="e8831-116">När du skapar två eller flera virtuella datorer distribueras resurserna beräkning och lagring i dessa domäner.</span><span class="sxs-lookup"><span data-stu-id="e8831-116">When you create two or more VMs, your compute and storage resources are distributed across these domains.</span></span> <span data-ttu-id="e8831-117">Den här distributionen underhåller tillgängligheten för din app om en maskinvarukomponent måste underhåll.</span><span class="sxs-lookup"><span data-stu-id="e8831-117">This distribution maintains the availability of your app if a hardware component needs maintenance.</span></span> <span data-ttu-id="e8831-118">Tillgänglighetsuppsättningar kan du definiera dessa logiska fel- och update-domäner.</span><span class="sxs-lookup"><span data-stu-id="e8831-118">Availability sets let you define these logical fault and update domains.</span></span>

<span data-ttu-id="e8831-119">Skapa en tillgänglighetsuppsättning med [ny AzureRmAvailabilitySet](/powershell/module/azurerm.compute/new-azurermavailabilityset).</span><span class="sxs-lookup"><span data-stu-id="e8831-119">Create an availability set with [New-AzureRmAvailabilitySet](/powershell/module/azurerm.compute/new-azurermavailabilityset).</span></span> <span data-ttu-id="e8831-120">I följande exempel skapas en tillgänglighetsuppsättning namngivna `myAvailabilitySet`:</span><span class="sxs-lookup"><span data-stu-id="e8831-120">The following example creates an availability set named `myAvailabilitySet`:</span></span>

```powershell
$availabilitySet = New-AzureRmAvailabilitySet `
  -ResourceGroupName myResourceGroup `
  -Name myAvailabilitySet `
  -Location westeurope `
  -Managed `
  -PlatformFaultDomainCount 3 `
  -PlatformUpdateDomainCount 2
```

## <a name="step-3---create-load-balancer"></a><span data-ttu-id="e8831-121">Steg 3 – skapa belastningen belastningsutjämnare</span><span class="sxs-lookup"><span data-stu-id="e8831-121">Step 3 - Create load balancer</span></span>

<span data-ttu-id="e8831-122">En Azure belastningsutjämnare distribuerar trafik över en uppsättning definierade virtuella datorer med hjälp av regler för inläsning av belastningsutjämnaren.</span><span class="sxs-lookup"><span data-stu-id="e8831-122">An Azure load balancer distributes traffic across a set of defined VMs using load balancer rules.</span></span> <span data-ttu-id="e8831-123">En hälsoavsökningen övervakar en viss port på varje virtuell dator och distribuerar endast trafik till en virtuell dator i drift.</span><span class="sxs-lookup"><span data-stu-id="e8831-123">A health probe monitors a given port on each VM and only distributes traffic to an operational VM.</span></span>

### <a name="create-public-ip-address"></a><span data-ttu-id="e8831-124">Skapa offentlig IP-adress</span><span class="sxs-lookup"><span data-stu-id="e8831-124">Create public IP address</span></span>

<span data-ttu-id="e8831-125">Tilldela en offentlig IP-adress till belastningsutjämnaren för att komma åt din app på Internet.</span><span class="sxs-lookup"><span data-stu-id="e8831-125">To access your app on the Internet, assign a public IP address to the load balancer.</span></span> <span data-ttu-id="e8831-126">Skapa en offentlig IP-adress med [ny AzureRmPublicIpAddress](/powershell/module/azurerm.network/new-azurermpublicipaddress).</span><span class="sxs-lookup"><span data-stu-id="e8831-126">Create a public IP address with [New-AzureRmPublicIpAddress](/powershell/module/azurerm.network/new-azurermpublicipaddress).</span></span> <span data-ttu-id="e8831-127">I följande exempel skapas en offentlig IP-adress med namnet `myPublicIP`:</span><span class="sxs-lookup"><span data-stu-id="e8831-127">The following example creates a public IP address named `myPublicIP`:</span></span>

```powershell
$pip = New-AzureRmPublicIpAddress `
  -ResourceGroupName myResourceGroup `
  -Location westeurope `
  -AllocationMethod Static `
  -Name myPublicIP
```

### <a name="create-load-balancer"></a><span data-ttu-id="e8831-128">Skapa belastningsutjämnare</span><span class="sxs-lookup"><span data-stu-id="e8831-128">Create load balancer</span></span>

<span data-ttu-id="e8831-129">Skapa en IP-adress för klientdel med [ny AzureRmLoadBalancerFrontendIpConfig](/powershell/module/azurerm.network/new-azurermloadbalancerfrontendipconfig).</span><span class="sxs-lookup"><span data-stu-id="e8831-129">Create a frontend IP address with [New-AzureRmLoadBalancerFrontendIpConfig](/powershell/module/azurerm.network/new-azurermloadbalancerfrontendipconfig).</span></span> <span data-ttu-id="e8831-130">I följande exempel skapas en IP-adress för klientdel som heter `myFrontEndPool`:</span><span class="sxs-lookup"><span data-stu-id="e8831-130">The following example creates a frontend IP address named `myFrontEndPool`:</span></span> 

```powershell
$frontendIP = New-AzureRmLoadBalancerFrontendIpConfig -Name myFrontEndPool -PublicIpAddress $pip
```

<span data-ttu-id="e8831-131">Skapa en backend-adresspool med [ny AzureRmLoadBalancerBackendAddressPoolConfig](/powershell/module/azurerm.network/new-azurermloadbalancerbackendaddresspoolconfig).</span><span class="sxs-lookup"><span data-stu-id="e8831-131">Create a backend address pool with [New-AzureRmLoadBalancerBackendAddressPoolConfig](/powershell/module/azurerm.network/new-azurermloadbalancerbackendaddresspoolconfig).</span></span> <span data-ttu-id="e8831-132">I följande exempel skapas en backend-adresspool som heter `myBackEndPool`:</span><span class="sxs-lookup"><span data-stu-id="e8831-132">The following example creates a backend address pool named `myBackEndPool`:</span></span>

```powershell
$backendPool = New-AzureRmLoadBalancerBackendAddressPoolConfig -Name myBackEndPool
```

<span data-ttu-id="e8831-133">Nu ska du skapa belastningsutjämnaren med [ny AzureRmLoadBalancer](/powershell/module/azurerm.network/new-azurermloadbalancer).</span><span class="sxs-lookup"><span data-stu-id="e8831-133">Now, create the load balancer with [New-AzureRmLoadBalancer](/powershell/module/azurerm.network/new-azurermloadbalancer).</span></span> <span data-ttu-id="e8831-134">I följande exempel skapas en belastningsutjämnare med namnet `myLoadBalancer` med hjälp av den `myPublicIP` adress:</span><span class="sxs-lookup"><span data-stu-id="e8831-134">The following example creates a load balancer named `myLoadBalancer` using the `myPublicIP` address:</span></span>

```powershell
$lb = New-AzureRmLoadBalancer `
  -ResourceGroupName myResourceGroup `
  -Name myLoadBalancer `
  -Location westeurope `
  -FrontendIpConfiguration $frontendIP `
  -BackendAddressPool $backendPool
```

### <a name="create-health-probe"></a><span data-ttu-id="e8831-135">Skapa hälsoavsökningen</span><span class="sxs-lookup"><span data-stu-id="e8831-135">Create health probe</span></span>

<span data-ttu-id="e8831-136">Om du vill att belastningsutjämnaren ska övervaka status för din app kan du använda en hälsoavsökningen.</span><span class="sxs-lookup"><span data-stu-id="e8831-136">To allow the load balancer to monitor the status of your app, you use a health probe.</span></span> <span data-ttu-id="e8831-137">Avsökningen hälsa dynamiskt lägger till eller tar bort virtuella datorer från belastningen belastningsutjämnaren rotationen baserat på deras svar på hälsokontroller.</span><span class="sxs-lookup"><span data-stu-id="e8831-137">The health probe dynamically adds or removes VMs from the load balancer rotation based on their response to health checks.</span></span> <span data-ttu-id="e8831-138">En virtuell dator tas bort från belastningsutjämnaren belastningsdistribution efter två på varandra följande fel intervaller 15 sekunder som standard.</span><span class="sxs-lookup"><span data-stu-id="e8831-138">By default, a VM is removed from the load balancer distribution after two consecutive failures at 15-second intervals.</span></span>

<span data-ttu-id="e8831-139">Skapa en hälsoavsökningen med [Lägg till AzureRmLoadBalancerProbeConfig](/powershell/module/azurerm.network/add-azurermloadbalancerprobeconfig).</span><span class="sxs-lookup"><span data-stu-id="e8831-139">Create a health probe with [Add-AzureRmLoadBalancerProbeConfig](/powershell/module/azurerm.network/add-azurermloadbalancerprobeconfig).</span></span> <span data-ttu-id="e8831-140">I följande exempel skapas en hälsoavsökningen med namnet `myHealthProbe` som övervakar varje virtuell dator:</span><span class="sxs-lookup"><span data-stu-id="e8831-140">The following example creates a health probe named `myHealthProbe` that monitors each VM:</span></span>

```powershell
Add-AzureRmLoadBalancerProbeConfig -Name myHealthProbe `
  -LoadBalancer $lb `
  -Protocol tcp `
  -Port 80 `
  -IntervalInSeconds 15 `
  -ProbeCount 2
```

### <a name="create-load-balancer-rule"></a><span data-ttu-id="e8831-141">Skapa regel för belastningsutjämnare</span><span class="sxs-lookup"><span data-stu-id="e8831-141">Create load balancer rule</span></span>

<span data-ttu-id="e8831-142">En regel för belastningsutjämnare används för att definiera hur trafiken distribueras till de virtuella datorerna.</span><span class="sxs-lookup"><span data-stu-id="e8831-142">A load balancer rule is used to define how traffic is distributed to the VMs.</span></span>

<span data-ttu-id="e8831-143">Skapa en regel för belastningsutjämnare med [Lägg till AzureRmLoadBalancerRuleConfig](/powershell/module/azurerm.network/add-azurermloadbalancerruleconfig).</span><span class="sxs-lookup"><span data-stu-id="e8831-143">Create a load balancer rule with [Add-AzureRmLoadBalancerRuleConfig](/powershell/module/azurerm.network/add-azurermloadbalancerruleconfig).</span></span> <span data-ttu-id="e8831-144">I följande exempel skapas en regel för belastningsutjämnare med namnet `myLoadBalancerRule` och balanserar trafik på port `80`:</span><span class="sxs-lookup"><span data-stu-id="e8831-144">The following example creates a load balancer rule named `myLoadBalancerRule` and balances traffic on port `80`:</span></span>

```powershell
Add-AzureRmLoadBalancerRuleConfig -Name myLoadBalancerRule `
  -LoadBalancer $lb `
  -FrontendIpConfiguration $lb.FrontendIpConfigurations[0] `
  -BackendAddressPool $lb.BackendAddressPools[0] `
  -Protocol Tcp `
  -FrontendPort 80 `
  -BackendPort 80
```

<span data-ttu-id="e8831-145">Uppdaterar belastningsutjämnaren med [Set AzureRmLoadBalancer](/powershell/module/azurerm.network/set-azurermloadbalancer):</span><span class="sxs-lookup"><span data-stu-id="e8831-145">Update the load balancer with [Set-AzureRmLoadBalancer](/powershell/module/azurerm.network/set-azurermloadbalancer):</span></span>

```powershell
Set-AzureRmLoadBalancer -LoadBalancer $lb
```

## <a name="step-4---configure-networking"></a><span data-ttu-id="e8831-146">Steg 4 – Konfigurera nätverk</span><span class="sxs-lookup"><span data-stu-id="e8831-146">Step 4 - Configure networking</span></span>

<span data-ttu-id="e8831-147">Varje virtuell dator har en eller flera virtuella nätverkskort (NIC) som ansluter till ett virtuellt nätverk.</span><span class="sxs-lookup"><span data-stu-id="e8831-147">Each VM has one or more virtual network interface cards (NICs) that connect to a virtual network.</span></span> <span data-ttu-id="e8831-148">Det här virtuella nätverket skyddas för att filtrera trafik baserat på definierade åtkomstregler.</span><span class="sxs-lookup"><span data-stu-id="e8831-148">This virtual network is secured to filter traffic based on defined access rules.</span></span>

### <a name="create-virtual-network"></a><span data-ttu-id="e8831-149">Skapa det virtuella nätverket</span><span class="sxs-lookup"><span data-stu-id="e8831-149">Create virtual network</span></span>

<span data-ttu-id="e8831-150">Konfigurera först ett undernät med [ny AzureRmVirtualNetworkSubnetConfig](/powershell/module/azurerm.network/new-azurermvirtualnetworksubnetconfig).</span><span class="sxs-lookup"><span data-stu-id="e8831-150">First, configure a subnet with [New-AzureRmVirtualNetworkSubnetConfig](/powershell/module/azurerm.network/new-azurermvirtualnetworksubnetconfig).</span></span> <span data-ttu-id="e8831-151">I följande exempel skapas ett undernät med namnet `mySubnet`:</span><span class="sxs-lookup"><span data-stu-id="e8831-151">The following example creates a subnet named `mySubnet`:</span></span>

```powershell
$subnetConfig = New-AzureRmVirtualNetworkSubnetConfig -Name mySubnet -AddressPrefix 192.168.1.0/24
```

<span data-ttu-id="e8831-152">För att ge nätverksanslutning till dina virtuella datorer, skapa ett virtuellt nätverk med [New-AzureRmVirtualNetwork](/powershell/module/azurerm.network/new-azurermvirtualnetwork).</span><span class="sxs-lookup"><span data-stu-id="e8831-152">To provide network connectivity to your VMs, create a virtual network with [New-AzureRmVirtualNetwork](/powershell/module/azurerm.network/new-azurermvirtualnetwork).</span></span> <span data-ttu-id="e8831-153">I följande exempel skapas ett virtuellt nätverk med namnet `myVnet` med `mySubnet`:</span><span class="sxs-lookup"><span data-stu-id="e8831-153">The following example creates a virtual network named `myVnet` with `mySubnet`:</span></span>

```powershell
$vnet = New-AzureRmVirtualNetwork `
  -ResourceGroupName myResourceGroup `
  -Location westeurope `
  -Name myVnet `
  -AddressPrefix 192.168.0.0/16 `
  -Subnet $subnetConfig
```

### <a name="configure-network-security"></a><span data-ttu-id="e8831-154">Konfigurera nätverkssäkerhet</span><span class="sxs-lookup"><span data-stu-id="e8831-154">Configure network security</span></span>

<span data-ttu-id="e8831-155">En Azure [nätverkssäkerhetsgruppen](../../virtual-network/virtual-networks-nsg.md) (NSG) styr inkommande och utgående trafik för en eller flera virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="e8831-155">An Azure [network security group](../../virtual-network/virtual-networks-nsg.md) (NSG) controls inbound and outbound traffic for one or many virtual machines.</span></span> <span data-ttu-id="e8831-156">Regler för nätverkssäkerhetsgrupper Tillåt eller neka nätverkstrafik på en specifik port eller ett intervall.</span><span class="sxs-lookup"><span data-stu-id="e8831-156">Network security group rules allow or deny network traffic on a specific port or port range.</span></span> <span data-ttu-id="e8831-157">De här reglerna kan också inkludera en källadress-prefix så att endast trafik i en fördefinierad källa kan kommunicera med en virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="e8831-157">These rules can also include a source address prefix so that only traffic originating at a predefined source can communicate with a virtual machine.</span></span>

<span data-ttu-id="e8831-158">För att tillåta webbtrafik att nå din app, skapa en grupp nätverkssäkerhetsregeln med [ny AzureRmNetworkSecurityRuleConfig](/powershell/module/azurerm.network/new-azurermnetworksecurityruleconfig).</span><span class="sxs-lookup"><span data-stu-id="e8831-158">To allow web traffic to reach your app, create a network security group rule with [New-AzureRmNetworkSecurityRuleConfig](/powershell/module/azurerm.network/new-azurermnetworksecurityruleconfig).</span></span> <span data-ttu-id="e8831-159">I följande exempel skapas en grupp nätverkssäkerhetsregeln med namnet `myNetworkSecurityGroupRule`:</span><span class="sxs-lookup"><span data-stu-id="e8831-159">The following example creates a network security group rule named `myNetworkSecurityGroupRule`:</span></span>

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

<span data-ttu-id="e8831-160">Skapa en nätverkssäkerhetsgrupp med [ny AzureRmNetworkSecurityGroup](/powershell/module/azurerm.network/new-azurermnetworksecuritygroup).</span><span class="sxs-lookup"><span data-stu-id="e8831-160">Create a network security group with [New-AzureRmNetworkSecurityGroup](/powershell/module/azurerm.network/new-azurermnetworksecuritygroup).</span></span> <span data-ttu-id="e8831-161">I följande exempel skapas en NSG som heter `myNetworkSecurityGroup`:</span><span class="sxs-lookup"><span data-stu-id="e8831-161">The following example creates an NSG named `myNetworkSecurityGroup`:</span></span>

```powershell
$nsg = New-AzureRmNetworkSecurityGroup `
  -ResourceGroupName myResourceGroup `
  -Location westeurope `
  -Name myNetworkSecurityGroup `
  -SecurityRules $nsgRule
```

<span data-ttu-id="e8831-162">Lägga till nätverkssäkerhetsgruppen till undernät med [Set AzureRmVirtualNetworkSubnetConfig](/powershell/module/azurerm.network/set-azurermvirtualnetworksubnetconfig):</span><span class="sxs-lookup"><span data-stu-id="e8831-162">Add the network security group to the subnet with [Set-AzureRmVirtualNetworkSubnetConfig](/powershell/module/azurerm.network/set-azurermvirtualnetworksubnetconfig):</span></span>

```powershell
Set-AzureRmVirtualNetworkSubnetConfig -VirtualNetwork $vnet `
  -Name mySubnet `
  -NetworkSecurityGroup $nsg `
  -AddressPrefix 192.168.1.0/24
```

<span data-ttu-id="e8831-163">Uppdatera det virtuella nätverket med [Set-AzureRmVirtualNetwork](/powershell/module/azurerm.network/set-azurermvirtualnetwork):</span><span class="sxs-lookup"><span data-stu-id="e8831-163">Update the virtual network with [Set-AzureRmVirtualNetwork](/powershell/module/azurerm.network/set-azurermvirtualnetwork):</span></span>

```powershell
Set-AzureRmVirtualNetwork -VirtualNetwork $vnet
```

### <a name="create-virtual-network-interface-cards"></a><span data-ttu-id="e8831-164">Skapa virtuellt nätverkskort</span><span class="sxs-lookup"><span data-stu-id="e8831-164">Create virtual network interface cards</span></span>

<span data-ttu-id="e8831-165">Läsa in belastningsutjämning funktionen med virtuell NIC-resurs i stället för verkliga VM.</span><span class="sxs-lookup"><span data-stu-id="e8831-165">Load balancers function with the virtual NIC resource rather than the actual VM.</span></span> <span data-ttu-id="e8831-166">Det virtuella nätverkskortet är anslutet till belastningsutjämnaren och bifogas till en virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="e8831-166">The virtual NIC is connected to the load balancer, and then attached to a VM.</span></span>

<span data-ttu-id="e8831-167">Skapa ett virtuellt nätverkskort med [ny AzureRmNetworkInterface](/powershell/module/azurerm.network/new-azurermnetworkinterface).</span><span class="sxs-lookup"><span data-stu-id="e8831-167">Create a virtual NIC with [New-AzureRmNetworkInterface](/powershell/module/azurerm.network/new-azurermnetworkinterface).</span></span> <span data-ttu-id="e8831-168">I följande exempel skapas tre virtuella nätverkskort.</span><span class="sxs-lookup"><span data-stu-id="e8831-168">The following example creates three virtual NICs.</span></span> <span data-ttu-id="e8831-169">(Ett virtuellt nätverkskort för varje virtuell du skapa för din app nedan):</span><span class="sxs-lookup"><span data-stu-id="e8831-169">(One virtual NIC for each VM you create for your app in the following steps):</span></span>


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

## <a name="step-5---create-virtual-machines"></a><span data-ttu-id="e8831-170">Steg 5 – Skapa virtuella datorer</span><span class="sxs-lookup"><span data-stu-id="e8831-170">Step 5 - Create virtual machines</span></span>

<span data-ttu-id="e8831-171">Du kan nu skapa högtillgängliga virtuella datorer om du vill köra appen med alla de underliggande komponenterna på plats.</span><span class="sxs-lookup"><span data-stu-id="e8831-171">With all the underlying components in place, you can now create highly available VMs to run your app.</span></span> 

<span data-ttu-id="e8831-172">Hämta användarnamn och lösenord för administratörskontot på den virtuella datorn med [Get-Credential](https://msdn.microsoft.com/powershell/reference/5.1/microsoft.powershell.security/Get-Credential):</span><span class="sxs-lookup"><span data-stu-id="e8831-172">Get the username and password needed for the administrator account on the virtual machine with [Get-Credential](https://msdn.microsoft.com/powershell/reference/5.1/microsoft.powershell.security/Get-Credential):</span></span>

```powershell
$cred = Get-Credential
```

<span data-ttu-id="e8831-173">Skapa de virtuella datorerna med [nya AzureRmVMConfig](https://docs.microsoft.com/powershell/resourcemanager/azurerm.compute/v2.8.0/new-azurermvmconfig), [Set AzureRmVMOperatingSystem](https://docs.microsoft.com/powershell/resourcemanager/azurerm.compute/v2.8.0/set-azurermvmoperatingsystem), [Set AzureRmVMSourceImage](https://docs.microsoft.com/powershell/resourcemanager/azurerm.compute/v2.8.0/set-azurermvmsourceimage), [Set AzureRmVMOSDisk](https://docs.microsoft.com/powershell/resourcemanager/azurerm.compute/v2.8.0/set-azurermvmosdisk), [lägga till AzureRmVMNetworkInterface](https://docs.microsoft.com/powershell/resourcemanager/azurerm.compute/v2.8.0/add-azurermvmnetworkinterface), och [nya AzureRmVM](/powershell/module/azurerm.compute/new-azurermvm).</span><span class="sxs-lookup"><span data-stu-id="e8831-173">Create the VMs with [New-AzureRmVMConfig](https://docs.microsoft.com/powershell/resourcemanager/azurerm.compute/v2.8.0/new-azurermvmconfig), [Set-AzureRmVMOperatingSystem](https://docs.microsoft.com/powershell/resourcemanager/azurerm.compute/v2.8.0/set-azurermvmoperatingsystem), [Set-AzureRmVMSourceImage](https://docs.microsoft.com/powershell/resourcemanager/azurerm.compute/v2.8.0/set-azurermvmsourceimage), [Set-AzureRmVMOSDisk](https://docs.microsoft.com/powershell/resourcemanager/azurerm.compute/v2.8.0/set-azurermvmosdisk), [Add-AzureRmVMNetworkInterface](https://docs.microsoft.com/powershell/resourcemanager/azurerm.compute/v2.8.0/add-azurermvmnetworkinterface), and [New-AzureRmVM](/powershell/module/azurerm.compute/new-azurermvm).</span></span> <span data-ttu-id="e8831-174">I följande exempel skapas tre virtuella datorer:</span><span class="sxs-lookup"><span data-stu-id="e8831-174">The following example creates three VMs:</span></span>

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

<span data-ttu-id="e8831-175">Det tar flera minuter att skapa och konfigurera alla tre virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="e8831-175">It takes several minutes to create and configure all three VMs.</span></span> <span data-ttu-id="e8831-176">I belastningsutjämnaren, hälsoavsökningen identifierar automatiskt när appen körs på varje virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="e8831-176">The load balancer health probe automatically detects when the app is running on each VM.</span></span> <span data-ttu-id="e8831-177">När appen körs börjar belastningsutjämningsregeln distribuera trafiken.</span><span class="sxs-lookup"><span data-stu-id="e8831-177">Once the app is running, the load balancer rule starts to distribute traffic.</span></span>

### <a name="install-the-app"></a><span data-ttu-id="e8831-178">Installera appen</span><span class="sxs-lookup"><span data-stu-id="e8831-178">Install the app</span></span> 

<span data-ttu-id="e8831-179">Tillägg för virtuella Azure-datorn används för att automatisera åtgärder för konfiguration av virtuell dator, till exempel installera program och konfigurera operativsystemet.</span><span class="sxs-lookup"><span data-stu-id="e8831-179">Azure virtual machine extensions are used to automate virtual machine configuration tasks such as installing applications and configuring the operating system.</span></span> <span data-ttu-id="e8831-180">Den [tillägget för anpassat skript för Windows](./../virtual-machines-windows-extensions-customscript.md) används för att köra PowerShell-skript på den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="e8831-180">The [custom script extension for Windows](./../virtual-machines-windows-extensions-customscript.md) is used to run any PowerShell script on the virtual machine.</span></span> <span data-ttu-id="e8831-181">Skriptet kan lagras i Azure storage, valfri tillgänglig HTTP-slutpunkt och inbäddade i konfigurationen för anpassat skript för tillägget.</span><span class="sxs-lookup"><span data-stu-id="e8831-181">The script can be stored in Azure storage, any accessible HTTP endpoint, or embedded in the custom script extension configuration.</span></span> <span data-ttu-id="e8831-182">När du använder tillägget för anpassat skript, hanterar Virtuella Azure-agenten körningen av skriptet.</span><span class="sxs-lookup"><span data-stu-id="e8831-182">When using the custom script extension, the Azure VM agent manages the script execution.</span></span>

<span data-ttu-id="e8831-183">Använd [Set AzureRmVMExtension](/powershell/module/azurerm.compute/set-azurermvmextension) att installera tillägget för anpassat skript.</span><span class="sxs-lookup"><span data-stu-id="e8831-183">Use [Set-AzureRmVMExtension](/powershell/module/azurerm.compute/set-azurermvmextension) to install the custom script extension.</span></span> <span data-ttu-id="e8831-184">Tillägget körs `powershell Add-WindowsFeature Web-Server` att installera IIS-webbserver:</span><span class="sxs-lookup"><span data-stu-id="e8831-184">The extension runs `powershell Add-WindowsFeature Web-Server` to install the IIS webserver:</span></span>

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

### <a name="test-your-app"></a><span data-ttu-id="e8831-185">Testa din app</span><span class="sxs-lookup"><span data-stu-id="e8831-185">Test your app</span></span>

<span data-ttu-id="e8831-186">Hämta offentlig IP-adressen för din belastningsutjämnare med [Get-AzureRmPublicIPAddress](/powershell/module/azurerm.network/get-azurermpublicipaddress).</span><span class="sxs-lookup"><span data-stu-id="e8831-186">Obtain the public IP address of your load balancer with [Get-AzureRmPublicIPAddress](/powershell/module/azurerm.network/get-azurermpublicipaddress).</span></span> <span data-ttu-id="e8831-187">I följande exempel hämtar IP-adressen för `myPublicIP` skapade tidigare:</span><span class="sxs-lookup"><span data-stu-id="e8831-187">The following example obtains the IP address for `myPublicIP` created earlier:</span></span>

```powershell
Get-AzureRmPublicIPAddress -ResourceGroupName myResourceGroup -Name myPublicIP | select IpAddress
```

<span data-ttu-id="e8831-188">Ange den offentliga IP-adressen i en webbläsare.</span><span class="sxs-lookup"><span data-stu-id="e8831-188">Enter the public IP address in to a web browser.</span></span> <span data-ttu-id="e8831-189">IIS-standardwebbplatsen visas med NSG-regeln på plats.</span><span class="sxs-lookup"><span data-stu-id="e8831-189">With the NSG rule in place, the default IIS website is displayed.</span></span> 

![Standardwebbplatsen i IIS](media/load-balanced-iis-tutorial/iis.png)

## <a name="step-6--management-tasks"></a><span data-ttu-id="e8831-191">Steg 6 – administrativa uppgifter</span><span class="sxs-lookup"><span data-stu-id="e8831-191">Step 6 – Management tasks</span></span>

<span data-ttu-id="e8831-192">Du kan behöva utföra underhåll på virtuella datorer som kör appen, till exempel installera uppdateringar av OS.</span><span class="sxs-lookup"><span data-stu-id="e8831-192">You may need to perform maintenance on the VMs running your app, such as installing OS updates.</span></span> <span data-ttu-id="e8831-193">Du kan behöva lägga till ytterligare virtuella datorer för att hantera den ökade trafiken till din app.</span><span class="sxs-lookup"><span data-stu-id="e8831-193">To deal with increased traffic to your app, you may need to add additional VMs.</span></span> <span data-ttu-id="e8831-194">Det här avsnittet visar hur du tar bort eller lägga till en virtuell dator från belastningsutjämnaren.</span><span class="sxs-lookup"><span data-stu-id="e8831-194">This section shows you how to remove or add a VM from the load balancer.</span></span> 

### <a name="remove-a-vm-from-the-load-balancer"></a><span data-ttu-id="e8831-195">Ta bort en virtuell dator från belastningsutjämnaren</span><span class="sxs-lookup"><span data-stu-id="e8831-195">Remove a VM from the load balancer</span></span>

<span data-ttu-id="e8831-196">Ta bort en virtuell dator från backend-adresspool genom att återställa egenskapen LoadBalancerBackendAddressPools för nätverkskortet.</span><span class="sxs-lookup"><span data-stu-id="e8831-196">Remove a VM from the backend address pool by resetting the LoadBalancerBackendAddressPools property of the network interface card.</span></span>

<span data-ttu-id="e8831-197">Hämta nätverkskort med [Get-AzureRmNetworkInterface](/powershell/module/azurerm.network/get-azurermnetworkinterface):</span><span class="sxs-lookup"><span data-stu-id="e8831-197">Get the network interface card with [Get-AzureRmNetworkInterface](/powershell/module/azurerm.network/get-azurermnetworkinterface):</span></span>

```powershell
$nic = Get-AzureRmNetworkInterface -ResourceGroupName myResourceGroup -Name myNic2
``` 

<span data-ttu-id="e8831-198">Egenskapen LoadBalancerBackendAddressPools för nätverkskortet till $null:</span><span class="sxs-lookup"><span data-stu-id="e8831-198">Set the LoadBalancerBackendAddressPools property of the network interface card to $null:</span></span>

```powershell
$nic.Ipconfigurations[0].LoadBalancerBackendAddressPools=$null
```

<span data-ttu-id="e8831-199">Uppdatera nätverkskortet:</span><span class="sxs-lookup"><span data-stu-id="e8831-199">Update the network interface card:</span></span>

```powershell
Set-AzureRmNetworkInterface -NetworkInterface $nic
```

### <a name="add-a-vm-to-the-load-balancer"></a><span data-ttu-id="e8831-200">Lägga till en virtuell dator till belastningsutjämnaren</span><span class="sxs-lookup"><span data-stu-id="e8831-200">Add a VM to the load balancer</span></span>

<span data-ttu-id="e8831-201">När du utför VM underhåll eller om du behöver Utöka kapaciteten att lägga till nätverkskort på en virtuell dator serverdelsadresspool för belastningsutjämnaren.</span><span class="sxs-lookup"><span data-stu-id="e8831-201">After performing VM maintenance, or if you need to expand capacity, adding the NIC of a VM to the backend address pool of the load balancer.</span></span>

<span data-ttu-id="e8831-202">Hämta belastningsutjämnaren:</span><span class="sxs-lookup"><span data-stu-id="e8831-202">Get the load balancer:</span></span>

```powershell
$lb = Get-AzureRMLoadBalancer -ResourceGroupName myResourceGroup -Name myLoadBalancer 
```

<span data-ttu-id="e8831-203">Lägg till backend-adresspool för belastningsutjämnaren nätverkskortet:</span><span class="sxs-lookup"><span data-stu-id="e8831-203">Add the backend address pool of the load balancer to the network interface card:</span></span>

```powershell
$nic.IpConfigurations[0].LoadBalancerBackendAddressPools=$lb.BackendAddressPools[0]
```

<span data-ttu-id="e8831-204">Uppdatera nätverkskortet:</span><span class="sxs-lookup"><span data-stu-id="e8831-204">Update the network interface card:</span></span>

```powershell
Set-AzureRmNetworkInterface -NetworkInterface $nic
```

## <a name="next-steps"></a><span data-ttu-id="e8831-205">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="e8831-205">Next Steps</span></span>

<span data-ttu-id="e8831-206">Exempel – [exempel Azure virtuella PowerShell-skript](./../virtual-machines-windows-powershell-samples.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)</span><span class="sxs-lookup"><span data-stu-id="e8831-206">Samples – [Azure Virtual Machine PowerShell sample scripts](./../virtual-machines-windows-powershell-samples.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)</span></span>
