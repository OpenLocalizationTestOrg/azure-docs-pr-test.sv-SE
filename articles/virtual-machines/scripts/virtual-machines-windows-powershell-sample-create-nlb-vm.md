---
title: aaaAzure PowerShell skriptexempel - skapa en virtuell Windows-NLB | Microsoft Docs
description: Azure PowerShell-skript Sample - skapa en virtuell Windows-NLB
services: virtual-machines-windows
documentationcenter: virtual-machines
author: neilpeterson
manager: timlt
editor: tysonn
tags: azure-service-management
ms.assetid: 
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: sample
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 06/05/2017
ms.author: nepeters
ms.custom: mvc
ms.openlocfilehash: 60a48c5f243c8ff3ab886437ae45b21ce069ea4e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="load-balance-traffic-between-highly-available-virtual-machines"></a><span data-ttu-id="f4559-103">Läs in Utjämna trafiken mellan virtuella datorer med hög tillgänglighet</span><span class="sxs-lookup"><span data-stu-id="f4559-103">Load balance traffic between highly available virtual machines</span></span>

<span data-ttu-id="f4559-104">Det här exemplet i skriptet skapar allt som behövs för toorun flera Windows Server 2016 virtuella datorer som konfigurerats i en hög tillgänglighet och läsa in den belastningsutjämnade konfiguration.</span><span class="sxs-lookup"><span data-stu-id="f4559-104">This script sample creates everything needed toorun several Windows Server 2016 virtual machines configured in a highly available and load balanced configuration.</span></span> <span data-ttu-id="f4559-105">När du köra hello-skriptet kommer du ha tre virtuella datorer kopplade tooan Azure Tillgänglighetsuppsättning och kan nås via en Azure belastningsutjämnare.</span><span class="sxs-lookup"><span data-stu-id="f4559-105">After running hello script, you will have three virtual machines, joined tooan Azure Availability Set, and accessible through an Azure Load Balancer.</span></span>

[!INCLUDE [sample-powershell-install](../../../includes/sample-powershell-install-no-ssh.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a><span data-ttu-id="f4559-106">Exempelskript</span><span class="sxs-lookup"><span data-stu-id="f4559-106">Sample script</span></span>

[!code-powershell[main](../../../powershell_scripts/virtual-machine/create-vm-nlb/create-vm-nlb.ps1 "Create VM NLB")]

## <a name="clean-up-deployment"></a><span data-ttu-id="f4559-107">Rensa distribution</span><span class="sxs-lookup"><span data-stu-id="f4559-107">Clean up deployment</span></span> 

<span data-ttu-id="f4559-108">Kör följande kommando tooremove hello resursgrupp, VM och alla relaterade resurser hello.</span><span class="sxs-lookup"><span data-stu-id="f4559-108">Run hello following command tooremove hello resource group, VM, and all related resources.</span></span>

```powershell
Remove-AzureRmResourceGroup -Name myResourceGroup
```

## <a name="script-explanation"></a><span data-ttu-id="f4559-109">Skriptet förklaring</span><span class="sxs-lookup"><span data-stu-id="f4559-109">Script explanation</span></span>

<span data-ttu-id="f4559-110">Det här skriptet använder hello följande kommandon toocreate hello distribution.</span><span class="sxs-lookup"><span data-stu-id="f4559-110">This script uses hello following commands toocreate hello deployment.</span></span> <span data-ttu-id="f4559-111">Varje objekt i hello tabellen länkar toocommand viss dokumentation.</span><span class="sxs-lookup"><span data-stu-id="f4559-111">Each item in hello table links toocommand specific documentation.</span></span>

| <span data-ttu-id="f4559-112">Kommando</span><span class="sxs-lookup"><span data-stu-id="f4559-112">Command</span></span> | <span data-ttu-id="f4559-113">Anteckningar</span><span class="sxs-lookup"><span data-stu-id="f4559-113">Notes</span></span> |
|---|---|
| [<span data-ttu-id="f4559-114">Ny AzureRmResourceGroup</span><span class="sxs-lookup"><span data-stu-id="f4559-114">New-AzureRmResourceGroup</span></span>](/powershell/module/azurerm.resources/new-azurermresourcegroup) | <span data-ttu-id="f4559-115">Skapar en resursgrupp som är lagrade i alla resurser.</span><span class="sxs-lookup"><span data-stu-id="f4559-115">Creates a resource group in which all resources are stored.</span></span> |
| [<span data-ttu-id="f4559-116">Ny AzureRmVirtualNetworkSubnetConfig</span><span class="sxs-lookup"><span data-stu-id="f4559-116">New-AzureRmVirtualNetworkSubnetConfig</span></span>](/powershell/module/azurerm.network/new-azurermvirtualnetworksubnetconfig) | <span data-ttu-id="f4559-117">Skapar en konfiguration av undernät.</span><span class="sxs-lookup"><span data-stu-id="f4559-117">Creates a subnet configuration.</span></span> <span data-ttu-id="f4559-118">Den här konfigurationen används med hello processen för att skapa virtuella nätverk.</span><span class="sxs-lookup"><span data-stu-id="f4559-118">This configuration is used with hello virtual network creation process.</span></span> |
| [<span data-ttu-id="f4559-119">Ny AzureRmVirtualNetwork</span><span class="sxs-lookup"><span data-stu-id="f4559-119">New-AzureRmVirtualNetwork</span></span>](/powershell/module/azurerm.network/new-azurermvirtualnetwork) | <span data-ttu-id="f4559-120">Skapar ett virtuellt nätverk.</span><span class="sxs-lookup"><span data-stu-id="f4559-120">Creates a virtual network.</span></span> |
| [<span data-ttu-id="f4559-121">Ny AzureRmPublicIpAddress</span><span class="sxs-lookup"><span data-stu-id="f4559-121">New-AzureRmPublicIpAddress</span></span>](/powershell/module/azurerm.network/new-azurermpublicipaddress) | <span data-ttu-id="f4559-122">Skapar en offentlig IP-adress.</span><span class="sxs-lookup"><span data-stu-id="f4559-122">Creates a public IP address.</span></span> |
| [<span data-ttu-id="f4559-123">Ny AzureRmLoadBalancerFrontendIpConfig</span><span class="sxs-lookup"><span data-stu-id="f4559-123">New-AzureRmLoadBalancerFrontendIpConfig</span></span>](/powershell/module/azurerm.network/new-azurermloadbalancerfrontendipconfig) | <span data-ttu-id="f4559-124">Skapar en frontend IP-konfiguration för en belastningsutjämnare.</span><span class="sxs-lookup"><span data-stu-id="f4559-124">Creates a front-end IP configuration for a load balancer.</span></span> |
| [<span data-ttu-id="f4559-125">Ny AzureRmLoadBalancerBackendAddressPoolConfig</span><span class="sxs-lookup"><span data-stu-id="f4559-125">New-AzureRmLoadBalancerBackendAddressPoolConfig</span></span>](/powershell/module/azurerm.network/new-azurermloadbalancerbackendaddresspoolconfig) | <span data-ttu-id="f4559-126">Skapar en backend-pool-adresskonfiguration för en belastningsutjämnare.</span><span class="sxs-lookup"><span data-stu-id="f4559-126">Creates a backend address pool configuration for a load balancer.</span></span> |
| [<span data-ttu-id="f4559-127">Ny AzureRmLoadBalancerProbeConfig</span><span class="sxs-lookup"><span data-stu-id="f4559-127">New-AzureRmLoadBalancerProbeConfig</span></span>](/powershell/module/azurerm.network/new-azurermloadbalancerprobeconfig) | <span data-ttu-id="f4559-128">Skapar en avsökning konfiguration för en belastningsutjämnare.</span><span class="sxs-lookup"><span data-stu-id="f4559-128">Creates a probe configuration for a load balancer.</span></span> |
| [<span data-ttu-id="f4559-129">Ny AzureRmLoadBalancerRuleConfig</span><span class="sxs-lookup"><span data-stu-id="f4559-129">New-AzureRmLoadBalancerRuleConfig</span></span>](/powershell/module/azurerm.network/new-azurermloadbalancerruleconfig) | <span data-ttu-id="f4559-130">Skapar en Regelkonfiguration för en belastningsutjämnare.</span><span class="sxs-lookup"><span data-stu-id="f4559-130">Creates a rule configuration for a load balancer.</span></span> |
| [<span data-ttu-id="f4559-131">Ny AzureRmLoadBalancerInboundNatRuleConfig</span><span class="sxs-lookup"><span data-stu-id="f4559-131">New-AzureRmLoadBalancerInboundNatRuleConfig</span></span>](/powershell/module/azurerm.network/new-azurermloadbalancerinboundnatruleconfig) | <span data-ttu-id="f4559-132">Skapar en inkommande NAT-Regelkonfiguration för en belastningsutjämnare.</span><span class="sxs-lookup"><span data-stu-id="f4559-132">Creates an inbound NAT rule configuration for a load balancer.</span></span> |
| [<span data-ttu-id="f4559-133">Ny AzureRmLoadBalancer</span><span class="sxs-lookup"><span data-stu-id="f4559-133">New-AzureRmLoadBalancer</span></span>](/powershell/module/azurerm.network/new-azurermloadbalancer) | <span data-ttu-id="f4559-134">Skapar en belastningsutjämnare.</span><span class="sxs-lookup"><span data-stu-id="f4559-134">Creates a load balancer.</span></span> |
| [<span data-ttu-id="f4559-135">Ny AzureRmNetworkSecurityRuleConfig</span><span class="sxs-lookup"><span data-stu-id="f4559-135">New-AzureRmNetworkSecurityRuleConfig</span></span>](/powershell/module/azurerm.network/new-azurermnetworksecurityruleconfig) | <span data-ttu-id="f4559-136">Skapar en grupp regeln nätverkssäkerhetskonfigurationen.</span><span class="sxs-lookup"><span data-stu-id="f4559-136">Creates a network security group rule configuration.</span></span> <span data-ttu-id="f4559-137">Den här konfigurationen är används toocreate en NSG regel när hello NSG skapas.</span><span class="sxs-lookup"><span data-stu-id="f4559-137">This configuration is used toocreate an NSG rule when hello NSG is created.</span></span> |
| [<span data-ttu-id="f4559-138">Ny AzureRmNetworkSecurityGroup</span><span class="sxs-lookup"><span data-stu-id="f4559-138">New-AzureRmNetworkSecurityGroup</span></span>](/powershell/module/azurerm.network/new-azurermnetworksecuritygroup) | <span data-ttu-id="f4559-139">Skapar en nätverkssäkerhetsgrupp.</span><span class="sxs-lookup"><span data-stu-id="f4559-139">Creates a network security group.</span></span> |
| [<span data-ttu-id="f4559-140">Get-AzureRmVirtualNetworkSubnetConfig</span><span class="sxs-lookup"><span data-stu-id="f4559-140">Get-AzureRmVirtualNetworkSubnetConfig</span></span>](/powershell/module/azurerm.network/get-azurermvirtualnetworksubnetconfig) | <span data-ttu-id="f4559-141">Hämtar information om undernät.</span><span class="sxs-lookup"><span data-stu-id="f4559-141">Gets subnet information.</span></span> <span data-ttu-id="f4559-142">Den här informationen används när du skapar ett nätverksgränssnitt.</span><span class="sxs-lookup"><span data-stu-id="f4559-142">This information is used when creating a network interface.</span></span> |
| [<span data-ttu-id="f4559-143">Ny AzureRmNetworkInterface</span><span class="sxs-lookup"><span data-stu-id="f4559-143">New-AzureRmNetworkInterface</span></span>](/powershell/module/azurerm.network/new-azurermnetworkinterface) | <span data-ttu-id="f4559-144">Skapar ett nätverksgränssnitt.</span><span class="sxs-lookup"><span data-stu-id="f4559-144">Creates a network interface.</span></span> |
| [<span data-ttu-id="f4559-145">Ny AzureRmVMConfig</span><span class="sxs-lookup"><span data-stu-id="f4559-145">New-AzureRmVMConfig</span></span>](/powershell/module/azurerm.compute/new-azurermvmconfig) | <span data-ttu-id="f4559-146">Skapar en VM-konfiguration.</span><span class="sxs-lookup"><span data-stu-id="f4559-146">Creates a VM configuration.</span></span> <span data-ttu-id="f4559-147">Den här konfigurationen omfattar information som VM-namn, operativsystem och administrativa autentiseringsuppgifter.</span><span class="sxs-lookup"><span data-stu-id="f4559-147">This configuration includes information such as VM name, operating system, and administrative credentials.</span></span> <span data-ttu-id="f4559-148">hello konfiguration används under skapande av virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="f4559-148">hello configuration is used during VM creation.</span></span> |
| [<span data-ttu-id="f4559-149">Ny AzureRmVM</span><span class="sxs-lookup"><span data-stu-id="f4559-149">New-AzureRmVM</span></span>](/powershell/module/azurerm.compute/new-azurermvm) | <span data-ttu-id="f4559-150">Skapa en virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="f4559-150">Create a virtual machine.</span></span> |
|[<span data-ttu-id="f4559-151">Ta bort AzureRmResourceGroup</span><span class="sxs-lookup"><span data-stu-id="f4559-151">Remove-AzureRmResourceGroup</span></span>](/powershell/module/azurerm.resources/remove-azurermresourcegroup) | <span data-ttu-id="f4559-152">Tar bort en resursgrupp och alla resurser som ingår i.</span><span class="sxs-lookup"><span data-stu-id="f4559-152">Removes a resource group and all resources contained within.</span></span> |

## <a name="next-steps"></a><span data-ttu-id="f4559-153">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="f4559-153">Next steps</span></span>

<span data-ttu-id="f4559-154">Mer information om hello Azure PowerShell-modulen finns [Azure PowerShell dokumentationen](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="f4559-154">For more information on hello Azure PowerShell module, see [Azure PowerShell documentation](/powershell/azure/overview).</span></span>

<span data-ttu-id="f4559-155">Ytterligare virtuella PowerShell-skript-exempel finns i hello [Virtuella för Windows Azure-dokumentationen](../windows/powershell-samples.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="f4559-155">Additional virtual machine PowerShell script samples can be found in hello [Azure Windows VM documentation](../windows/powershell-samples.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span>
