---
title: "aaaAzure PowerShell skriptexempel - vägtrafik via en virtuell nätverksenhet | Microsoft Docs"
description: "Azure PowerShell skriptexempel - vägtrafik via en virtuell nätverksenhet för brandväggen."
services: virtual-network
documentationcenter: virtual-network
author: georgewallace
manager: timlt
editor: tysonn
tags: 
ms.assetid: 
ms.service: virtual-network
ms.devlang: powershell
ms.topic: article
ms.tgt_pltfrm: 
ms.workload: infrastructure
ms.date: 05/16/2017
ms.author: gwallace
ms.openlocfilehash: 3b999f3289d654c00d5becb973e2883896457d52
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="route-traffic-through-a-network-virtual-appliance"></a><span data-ttu-id="b12ab-103">Vidarebefordra trafik via en virtuell nätverksenhet</span><span class="sxs-lookup"><span data-stu-id="b12ab-103">Route traffic through a network virtual appliance</span></span>

<span data-ttu-id="b12ab-104">Det här exemplet i skriptet skapar ett virtuellt nätverk med frontend och backend-undernät.</span><span class="sxs-lookup"><span data-stu-id="b12ab-104">This script sample creates a virtual network with front-end and back-end subnets.</span></span> <span data-ttu-id="b12ab-105">Dessutom skapas en virtuell dator med IP-vidarebefordring aktiverade tooroute trafik mellan hello två undernät.</span><span class="sxs-lookup"><span data-stu-id="b12ab-105">It also creates a VM with IP forwarding enabled tooroute traffic between hello two subnets.</span></span> <span data-ttu-id="b12ab-106">Du kan distribuera programvara för nätverk, till exempel en brandvägg programmet toohello VM när du har kört hello skript.</span><span class="sxs-lookup"><span data-stu-id="b12ab-106">After running hello script you can deploy network software, such as a firewall application, toohello VM.</span></span>

<span data-ttu-id="b12ab-107">Om det behövs installerar du Azure PowerShell med hjälp av hello-instruktion finns i hello hello [Azure PowerShell guiden](https://docs.microsoft.com/powershell/azureps-cmdlets-docs/), och kör sedan `Login-AzureRmAccount` toocreate en anslutning till Azure.</span><span class="sxs-lookup"><span data-stu-id="b12ab-107">If needed, install hello Azure PowerShell using hello instruction found in hello [Azure PowerShell guide](https://docs.microsoft.com/powershell/azureps-cmdlets-docs/), and then run `Login-AzureRmAccount` toocreate a connection with Azure.</span></span>

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a><span data-ttu-id="b12ab-108">Exempelskript</span><span class="sxs-lookup"><span data-stu-id="b12ab-108">Sample script</span></span>


[!code-powershell[main](../../../powershell_scripts/virtual-network/route-traffic-through-nva/route-traffic-through-nva.ps1 "Route traffic through a network virtual appliance")]

## <a name="clean-up-deployment"></a><span data-ttu-id="b12ab-109">Rensa distribution</span><span class="sxs-lookup"><span data-stu-id="b12ab-109">Clean up deployment</span></span> 

<span data-ttu-id="b12ab-110">Kör följande kommando tooremove hello resursgrupp, VM och alla relaterade resurser hello.</span><span class="sxs-lookup"><span data-stu-id="b12ab-110">Run hello following command tooremove hello resource group, VM, and all related resources.</span></span>

```powershell
Remove-AzureRmResourceGroup -Name myResourceGroup
```
## <a name="script-explanation"></a><span data-ttu-id="b12ab-111">Skriptet förklaring</span><span class="sxs-lookup"><span data-stu-id="b12ab-111">Script explanation</span></span>

<span data-ttu-id="b12ab-112">Det här skriptet använder följande kommandon toocreate hello en resursgrupp, virtuella nätverk och nätverkssäkerhetsgrupper.</span><span class="sxs-lookup"><span data-stu-id="b12ab-112">This script uses hello following commands toocreate a resource group, virtual network,  and network security groups.</span></span> <span data-ttu-id="b12ab-113">Varje kommando i hello tabellen länkar toocommand specifika-dokumentationen.</span><span class="sxs-lookup"><span data-stu-id="b12ab-113">Each command in hello table links toocommand-specific documentation.</span></span>

| <span data-ttu-id="b12ab-114">Kommando</span><span class="sxs-lookup"><span data-stu-id="b12ab-114">Command</span></span> | <span data-ttu-id="b12ab-115">Anteckningar</span><span class="sxs-lookup"><span data-stu-id="b12ab-115">Notes</span></span> |
|---|---|
| [<span data-ttu-id="b12ab-116">Ny AzureRmResourceGroup</span><span class="sxs-lookup"><span data-stu-id="b12ab-116">New-AzureRmResourceGroup</span></span>](/powershell/module/azurerm.resources/new-azurermresourcegroup)  | <span data-ttu-id="b12ab-117">Skapar en resursgrupp som är lagrade i alla resurser.</span><span class="sxs-lookup"><span data-stu-id="b12ab-117">Creates a resource group in which all resources are stored.</span></span> |
| [<span data-ttu-id="b12ab-118">Ny AzureRmVirtualNetwork</span><span class="sxs-lookup"><span data-stu-id="b12ab-118">New-AzureRmVirtualNetwork</span></span>](/powershell/module/azurerm.network/new-azurermvirtualnetwork) | <span data-ttu-id="b12ab-119">Skapar en Azure-nätverket och frontend-undernät.</span><span class="sxs-lookup"><span data-stu-id="b12ab-119">Creates an Azure virtual network and front-end subnet.</span></span> |
| [<span data-ttu-id="b12ab-120">Ny AzureRmVirtualNetworkSubnetConfig</span><span class="sxs-lookup"><span data-stu-id="b12ab-120">New-AzureRmVirtualNetworkSubnetConfig</span></span>](/powershell/module/azurerm.network/new-azurermvirtualnetworksubnetconfig) | <span data-ttu-id="b12ab-121">Skapar backend- och DMZ undernät.</span><span class="sxs-lookup"><span data-stu-id="b12ab-121">Creates back-end and DMZ subnets.</span></span> |
| [<span data-ttu-id="b12ab-122">Ny AzureRmPublicIpAddress</span><span class="sxs-lookup"><span data-stu-id="b12ab-122">New-AzureRmPublicIpAddress</span></span>](/powershell/module/azurerm.network/new-azurermpublicipaddress) | <span data-ttu-id="b12ab-123">Skapar en offentlig IP-adress tooaccess hello VM från hello Internet.</span><span class="sxs-lookup"><span data-stu-id="b12ab-123">Creates a public IP address tooaccess hello VM from hello Internet.</span></span> |
| [<span data-ttu-id="b12ab-124">Ny AzureRmNetworkInterface</span><span class="sxs-lookup"><span data-stu-id="b12ab-124">New-AzureRmNetworkInterface</span></span>](/powershell/module/azurerm.network/new-azurermnetworkinterface) | <span data-ttu-id="b12ab-125">Skapar ett virtuellt nätverksgränssnitt och aktivera IP-vidarebefordring för den.</span><span class="sxs-lookup"><span data-stu-id="b12ab-125">Creates a virtual network interface and enable IP forwarding for it.</span></span> |
| [<span data-ttu-id="b12ab-126">Ny AzureRmNetworkSecurityGroup</span><span class="sxs-lookup"><span data-stu-id="b12ab-126">New-AzureRmNetworkSecurityGroup</span></span>](/powershell/module/azurerm.network/new-azurermnetworksecuritygroup) | <span data-ttu-id="b12ab-127">Skapar en nätverkssäkerhetsgrupp (NSG).</span><span class="sxs-lookup"><span data-stu-id="b12ab-127">Creates a network security group (NSG).</span></span> |
| [<span data-ttu-id="b12ab-128">Ny AzureRmNetworkSecurityRuleConfig</span><span class="sxs-lookup"><span data-stu-id="b12ab-128">New-AzureRmNetworkSecurityRuleConfig</span></span>](/powershell/module/azurerm.network/new-azurermnetworksecurityruleconfig) | <span data-ttu-id="b12ab-129">Skapar NSG-regler som tillåter HTTP och HTTPS-portar inkommande toohello VM.</span><span class="sxs-lookup"><span data-stu-id="b12ab-129">Creates NSG rules that allow HTTP and HTTPS ports inbound toohello VM.</span></span> |
| [<span data-ttu-id="b12ab-130">Ange AzureRmVirtualNetworkSubnetConfig</span><span class="sxs-lookup"><span data-stu-id="b12ab-130">Set-AzureRmVirtualNetworkSubnetConfig</span></span>](/powershell/module/azurerm.network/set-azurermvirtualnetworksubnetconfig)| <span data-ttu-id="b12ab-131">Associerats hello NSG: er och väg tabeller toosubnets.</span><span class="sxs-lookup"><span data-stu-id="b12ab-131">Associates hello NSGs and route tables toosubnets.</span></span> |
| [<span data-ttu-id="b12ab-132">Ny AzureRmRouteTable</span><span class="sxs-lookup"><span data-stu-id="b12ab-132">New-AzureRmRouteTable</span></span>](/powershell/module/azurerm.network/new-azurermroutetable)| <span data-ttu-id="b12ab-133">Skapar en routningstabell för alla flöden.</span><span class="sxs-lookup"><span data-stu-id="b12ab-133">Creates a route table for all routes.</span></span> |
| [<span data-ttu-id="b12ab-134">Ny AzureRMRouteConfig</span><span class="sxs-lookup"><span data-stu-id="b12ab-134">New-AzureRMRouteConfig</span></span>](/powershell/module/azurerm.network/new-azurermrouteconfig)| <span data-ttu-id="b12ab-135">Skapar vägar tooroute trafik mellan undernät och hello Internet via hello VM.</span><span class="sxs-lookup"><span data-stu-id="b12ab-135">Creates routes tooroute traffic between subnets and hello Internet through hello VM.</span></span> |
| [<span data-ttu-id="b12ab-136">Ny AzureRmVM</span><span class="sxs-lookup"><span data-stu-id="b12ab-136">New-AzureRmVM</span></span>](/powershell/module/azurerm.compute/new-azurermvm) | <span data-ttu-id="b12ab-137">Skapar en virtuell dator och bifogar hello NIC tooit.</span><span class="sxs-lookup"><span data-stu-id="b12ab-137">Creates a virtual machine and attaches hello NIC tooit.</span></span> <span data-ttu-id="b12ab-138">Det här kommandot anger också hello virtuella avbildningen toouse och administrativa autentiseringsuppgifter.</span><span class="sxs-lookup"><span data-stu-id="b12ab-138">This command also specifies hello virtual machine image toouse and administrative credentials.</span></span> |
| [<span data-ttu-id="b12ab-139">Ta bort AzureRmResourceGroup</span><span class="sxs-lookup"><span data-stu-id="b12ab-139">Remove-AzureRmResourceGroup</span></span>](/powershell/module/azurerm.resources/remove-azurermresourcegroup)  | <span data-ttu-id="b12ab-140">Tar bort en resursgrupp och alla resurser som den innehåller.</span><span class="sxs-lookup"><span data-stu-id="b12ab-140">Deletes a resource group and all resources it contains.</span></span> |

## <a name="next-steps"></a><span data-ttu-id="b12ab-141">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="b12ab-141">Next steps</span></span>

<span data-ttu-id="b12ab-142">Mer information om hello Azure PowerShell finns [Azure PowerShell dokumentationen](https://docs.microsoft.com/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="b12ab-142">For more information on hello Azure PowerShell, see [Azure PowerShell documentation](https://docs.microsoft.com/powershell/azure/overview).</span></span>

<span data-ttu-id="b12ab-143">Ytterligare nätverk PowerShell-skript-exempel finns i hello [dokumentation för Azure-nätverk – översikt](../powershell-samples.md?toc=%2fazure%2fnetworking%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="b12ab-143">Additional networking PowerShell script samples can be found in hello [Azure Networking Overview documentation](../powershell-samples.md?toc=%2fazure%2fnetworking%2ftoc.json).</span></span>
