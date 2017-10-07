---
title: "aaaAzure PowerShell skriptexempel - belastningsutjämning för flera webbplatser med Azure PowerShell | Microsoft Docs"
description: "Azure PowerShell skriptexempel - belastningsutjämning för flera webbplatser toohello samma virtuella dator"
services: load-balancer
documentationcenter: load-balancer
author: georgewallace
manager: timlt
editor: tysonn
tags: 
ms.assetid: 
ms.service: load-balancer
ms.devlang: powershell
ms.topic: article
ms.tgt_pltfrm: 
ms.workload: infrastructure
ms.date: 05/16/2017
ms.author: gwallace
ms.openlocfilehash: 316818964eb6928fe4163ef69eb7f05da2dc9636
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="load-balance-multiple-websites"></a><span data-ttu-id="15dd2-103">Belastningsutjämning för flera webbplatser</span><span class="sxs-lookup"><span data-stu-id="15dd2-103">Load balance multiple websites</span></span>

<span data-ttu-id="15dd2-104">Det här exemplet i skriptet skapar ett virtuellt nätverk med två virtuella datorer (VM) som är medlemmar i en tillgänglighetsuppsättning.</span><span class="sxs-lookup"><span data-stu-id="15dd2-104">This script sample creates a virtual network with two virtual machines (VM) that are members of an availability set.</span></span> <span data-ttu-id="15dd2-105">En belastningsutjämnare dirigerar trafik för två separata IP-adresser toohello två virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="15dd2-105">A load balancer directs traffic for two separate IP addresses toohello two VMs.</span></span> <span data-ttu-id="15dd2-106">Efter köra hello skriptet kan du distribuera web server programvara toohello virtuella datorer och vara värd för flera webbplatser, var och en med sin egen IP-adress.</span><span class="sxs-lookup"><span data-stu-id="15dd2-106">After running hello script, you could deploy web server software toohello VMs and host multiple web sites, each with its own IP address.</span></span>

<span data-ttu-id="15dd2-107">Om det behövs installerar du Azure PowerShell med hjälp av hello-instruktion finns i hello hello [Azure PowerShell guiden](https://docs.microsoft.com/powershell/azureps-cmdlets-docs/), och kör sedan `Login-AzureRmAccount` toocreate en anslutning till Azure.</span><span class="sxs-lookup"><span data-stu-id="15dd2-107">If needed, install hello Azure PowerShell using hello instruction found in hello [Azure PowerShell guide](https://docs.microsoft.com/powershell/azureps-cmdlets-docs/), and then run `Login-AzureRmAccount` toocreate a connection with Azure.</span></span>

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a><span data-ttu-id="15dd2-108">Exempelskript</span><span class="sxs-lookup"><span data-stu-id="15dd2-108">Sample script</span></span>

[!code-powershell[main](../../../powershell_scripts/load-balancer/load-balance-multiple-web-sites-vm/load-balance-multiple-web-sites-vm.ps1  "Load balance multiple web sites")]

## <a name="clean-up-deployment"></a><span data-ttu-id="15dd2-109">Rensa distribution</span><span class="sxs-lookup"><span data-stu-id="15dd2-109">Clean up deployment</span></span> 

<span data-ttu-id="15dd2-110">Kör följande kommando tooremove hello resursgrupp, VM och alla relaterade resurser hello.</span><span class="sxs-lookup"><span data-stu-id="15dd2-110">Run hello following command tooremove hello resource group, VM, and all related resources.</span></span>

```powershell
Remove-AzureRmResourceGroup -Name myResourceGroup
```

## <a name="script-explanation"></a><span data-ttu-id="15dd2-111">Skriptet förklaring</span><span class="sxs-lookup"><span data-stu-id="15dd2-111">Script explanation</span></span>

<span data-ttu-id="15dd2-112">Det här skriptet använder hello följande kommandon toocreate en resursgrupp, virtuella nätverk, läsa in belastningsutjämning och alla relaterade resurser.</span><span class="sxs-lookup"><span data-stu-id="15dd2-112">This script uses hello following commands toocreate a resource group, virtual network, load balancer, and all related resources.</span></span> <span data-ttu-id="15dd2-113">Varje kommando i hello tabellen länkar toocommand viss dokumentation.</span><span class="sxs-lookup"><span data-stu-id="15dd2-113">Each command in hello table links toocommand specific documentation.</span></span>

| <span data-ttu-id="15dd2-114">Kommando</span><span class="sxs-lookup"><span data-stu-id="15dd2-114">Command</span></span> | <span data-ttu-id="15dd2-115">Anteckningar</span><span class="sxs-lookup"><span data-stu-id="15dd2-115">Notes</span></span> |
|---|---|
| [<span data-ttu-id="15dd2-116">Ny AzureRmResourceGroup</span><span class="sxs-lookup"><span data-stu-id="15dd2-116">New-AzureRmResourceGroup</span></span>](/powershell/module/azurerm.resources/new-azurermresourcegroup) | <span data-ttu-id="15dd2-117">Skapar en resursgrupp som är lagrade i alla resurser.</span><span class="sxs-lookup"><span data-stu-id="15dd2-117">Creates a resource group in which all resources are stored.</span></span> |
| [<span data-ttu-id="15dd2-118">Ny AzureRmAvailabilitySet</span><span class="sxs-lookup"><span data-stu-id="15dd2-118">New-AzureRmAvailabilitySet</span></span>](/powershell/module/azurerm.compute/new-azurermavailabilityset) | <span data-ttu-id="15dd2-119">Skapar ett Azure tillgänglighet ange tooprovide hög tillgänglighet.</span><span class="sxs-lookup"><span data-stu-id="15dd2-119">Creates an Azure availability set tooprovide high availability.</span></span> |
| [<span data-ttu-id="15dd2-120">Ny AzureRmVirtualNetworkSubnetConfig</span><span class="sxs-lookup"><span data-stu-id="15dd2-120">New-AzureRmVirtualNetworkSubnetConfig</span></span>](/powershell/module/azurerm.network/new-azurermvirtualnetworksubnetconfig) | <span data-ttu-id="15dd2-121">Skapar en konfiguration av undernät.</span><span class="sxs-lookup"><span data-stu-id="15dd2-121">Creates a subnet configuration.</span></span> <span data-ttu-id="15dd2-122">Den här konfigurationen används med hello processen för att skapa virtuella nätverk.</span><span class="sxs-lookup"><span data-stu-id="15dd2-122">This configuration is used with hello virtual network creation process.</span></span> |
| [<span data-ttu-id="15dd2-123">Ny AzureRmVirtualNetwork</span><span class="sxs-lookup"><span data-stu-id="15dd2-123">New-AzureRmVirtualNetwork</span></span>](/powershell/module/azurerm.network/new-azurermvirtualnetwork) | <span data-ttu-id="15dd2-124">Skapar ett virtuellt nätverk.</span><span class="sxs-lookup"><span data-stu-id="15dd2-124">Creates a virtual network.</span></span> |
| [<span data-ttu-id="15dd2-125">Ny AzureRmPublicIpAddress</span><span class="sxs-lookup"><span data-stu-id="15dd2-125">New-AzureRmPublicIpAddress</span></span>](/powershell/module/azurerm.network/new-azurermpublicipaddress) | <span data-ttu-id="15dd2-126">Skapar en offentlig IP-adress.</span><span class="sxs-lookup"><span data-stu-id="15dd2-126">Creates a public IP address.</span></span> |
| [<span data-ttu-id="15dd2-127">Ny AzureRmLoadBalancerFrontendIpConfig</span><span class="sxs-lookup"><span data-stu-id="15dd2-127">New-AzureRmLoadBalancerFrontendIpConfig</span></span>](/powershell/module/azurerm.network/new-azurermloadbalancerfrontendipconfig) | <span data-ttu-id="15dd2-128">Skapar en klientdel IP-konfigurationen för en belastningsutjämnare.</span><span class="sxs-lookup"><span data-stu-id="15dd2-128">Creates a front end IP config for a load balancer.</span></span> |
| [<span data-ttu-id="15dd2-129">Ny AzureRmLoadBalancerBackendAddressPoolConfig</span><span class="sxs-lookup"><span data-stu-id="15dd2-129">New-AzureRmLoadBalancerBackendAddressPoolConfig</span></span>](/powershell/module/azurerm.network/new-azurermloadbalancerbackendaddresspoolconfig) | <span data-ttu-id="15dd2-130">Skapar en backend-pool-adresskonfiguration för en belastningsutjämnare.</span><span class="sxs-lookup"><span data-stu-id="15dd2-130">Creates a backend address pool configuration for a load balancer.</span></span> |
| [<span data-ttu-id="15dd2-131">Ny AzureRmLoadBalancerProbeConfig</span><span class="sxs-lookup"><span data-stu-id="15dd2-131">New-AzureRmLoadBalancerProbeConfig</span></span>](/powershell/module/azurerm.network/new-azurermloadbalancerprobeconfig) | <span data-ttu-id="15dd2-132">Skapar ett NLB-avsökning.</span><span class="sxs-lookup"><span data-stu-id="15dd2-132">Creates an NLB probe.</span></span> <span data-ttu-id="15dd2-133">En NLB-avsökning har använt toomonitor varje virtuell dator i hello NLB uppsättningen.</span><span class="sxs-lookup"><span data-stu-id="15dd2-133">An NLB probe is used toomonitor each VM in hello NLB set.</span></span> <span data-ttu-id="15dd2-134">Om någon virtuell dator blir otillgänglig, är inte trafik dirigeras toohello VM.</span><span class="sxs-lookup"><span data-stu-id="15dd2-134">If any VM becomes inaccessible, traffic is not routed toohello VM.</span></span> |
| [<span data-ttu-id="15dd2-135">Ny AzureRmLoadBalancerRuleConfig</span><span class="sxs-lookup"><span data-stu-id="15dd2-135">New-AzureRmLoadBalancerRuleConfig</span></span>](/powershell/module/azurerm.network/new-azurermloadbalancerruleconfig) | <span data-ttu-id="15dd2-136">Skapar en regel för Utjämning av nätverksbelastning.</span><span class="sxs-lookup"><span data-stu-id="15dd2-136">Creates an NLB rule.</span></span> <span data-ttu-id="15dd2-137">I det här exemplet skapas en regel för port 80.</span><span class="sxs-lookup"><span data-stu-id="15dd2-137">In this sample, a rule is created for port 80.</span></span> <span data-ttu-id="15dd2-138">HTTP-trafik anländer till hello Utjämning av nätverksbelastning, är det routade tooport 80 en hello virtuella datorer i hello NLB uppsättningen.</span><span class="sxs-lookup"><span data-stu-id="15dd2-138">As HTTP traffic arrives at hello NLB, it is routed tooport 80 one of hello VMs in hello NLB set.</span></span> |
| [<span data-ttu-id="15dd2-139">Ny AzureRmLoadBalancer</span><span class="sxs-lookup"><span data-stu-id="15dd2-139">New-AzureRmLoadBalancer</span></span>](/powershell/module/azurerm.network/new-azurermloadbalancer) | <span data-ttu-id="15dd2-140">Skapar en belastningsutjämnare.</span><span class="sxs-lookup"><span data-stu-id="15dd2-140">Creates a load balancer.</span></span> |
| [<span data-ttu-id="15dd2-141">Ny AzureRmNetworkInterfaceIpConfig</span><span class="sxs-lookup"><span data-stu-id="15dd2-141">New-AzureRmNetworkInterfaceIpConfig</span></span>](/powershell/module/azurerm.network/new-azurermnetworkinterfaceipconfig) | <span data-ttu-id="15dd2-142">Definierar avancerade funktioner för ett virtuellt nätverksgränssnitt.</span><span class="sxs-lookup"><span data-stu-id="15dd2-142">Defines advanced features for a virtual network interface.</span></span> |
| [<span data-ttu-id="15dd2-143">Ny AzureRmNetworkInterface</span><span class="sxs-lookup"><span data-stu-id="15dd2-143">New-AzureRmNetworkInterface</span></span>](/powershell/module/azurerm.network/new-azurermnetworkinterface) | <span data-ttu-id="15dd2-144">Skapar ett nätverksgränssnitt.</span><span class="sxs-lookup"><span data-stu-id="15dd2-144">Creates a network interface.</span></span> |
| [<span data-ttu-id="15dd2-145">Ny AzureRmVMConfig</span><span class="sxs-lookup"><span data-stu-id="15dd2-145">New-AzureRmVMConfig</span></span>](/powershell/module/azurerm.compute/new-azurermvmconfig) | <span data-ttu-id="15dd2-146">Skapar en VM-konfiguration.</span><span class="sxs-lookup"><span data-stu-id="15dd2-146">Creates a VM configuration.</span></span> <span data-ttu-id="15dd2-147">Den här konfigurationen omfattar information som VM-namn, operativsystem och administrativa autentiseringsuppgifter.</span><span class="sxs-lookup"><span data-stu-id="15dd2-147">This configuration includes information such as VM name, operating system, and administrative credentials.</span></span> <span data-ttu-id="15dd2-148">hello konfiguration används under skapande av virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="15dd2-148">hello configuration is used during VM creation.</span></span> |
| [<span data-ttu-id="15dd2-149">Ny AzureRmVM</span><span class="sxs-lookup"><span data-stu-id="15dd2-149">New-AzureRmVM</span></span>](/powershell/module/azurerm.compute/new-azurermvm) | <span data-ttu-id="15dd2-150">Skapa en virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="15dd2-150">Create a virtual machine.</span></span> |
|[<span data-ttu-id="15dd2-151">Ta bort AzureRmResourceGroup</span><span class="sxs-lookup"><span data-stu-id="15dd2-151">Remove-AzureRmResourceGroup</span></span>](/powershell/module/azurerm.resources/remove-azurermresourcegroup) | <span data-ttu-id="15dd2-152">Tar bort en resursgrupp och alla resurser som ingår i.</span><span class="sxs-lookup"><span data-stu-id="15dd2-152">Removes a resource group and all resources contained within.</span></span> |

## <a name="next-steps"></a><span data-ttu-id="15dd2-153">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="15dd2-153">Next steps</span></span>

<span data-ttu-id="15dd2-154">Mer information om hello Azure PowerShell finns [Azure PowerShell dokumentationen](https://docs.microsoft.com/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="15dd2-154">For more information on hello Azure PowerShell, see [Azure PowerShell documentation](https://docs.microsoft.com/powershell/azure/overview).</span></span>

<span data-ttu-id="15dd2-155">Ytterligare nätverk PowerShell-skript-exempel finns i hello [dokumentation för Azure-nätverk – översikt](../powershell-samples.md?toc=%2fazure%2fnetworking%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="15dd2-155">Additional networking PowerShell script samples can be found in hello [Azure Networking Overview documentation](../powershell-samples.md?toc=%2fazure%2fnetworking%2ftoc.json).</span></span>
