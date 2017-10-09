---
title: "aaaAzure PowerShell skriptexempel - Peer-två virtuella nätverk | Microsoft Docs"
description: "Azure PowerShell skriptexempel - Peer två virtuella nätverk"
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
ms.openlocfilehash: 8b66085c35de2fc30bcef57a00d7d370911d1f3c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="peer-two-virtual-networks"></a><span data-ttu-id="95ca9-103">Peer-två virtuella nätverk</span><span class="sxs-lookup"><span data-stu-id="95ca9-103">Peer two virtual networks</span></span>

<span data-ttu-id="95ca9-104">Det här skriptet skapar och ansluter två virtuella nätverk i hello samma region trhough hello Azure-nätverk.</span><span class="sxs-lookup"><span data-stu-id="95ca9-104">This script creates and connects two virtual networks in hello same region trhough hello Azure network.</span></span> <span data-ttu-id="95ca9-105">När du har kört hello skript skapar du en peering mellan två virtuella nätverk.</span><span class="sxs-lookup"><span data-stu-id="95ca9-105">After running hello script, you will create a peering between two virtual networks.</span></span>

<span data-ttu-id="95ca9-106">Om det behövs installerar du Azure PowerShell med hjälp av hello-instruktion finns i hello hello [Azure PowerShell guiden](https://docs.microsoft.com/powershell/azureps-cmdlets-docs/), och kör sedan `Login-AzureRmAccount` toocreate en anslutning till Azure.</span><span class="sxs-lookup"><span data-stu-id="95ca9-106">If needed, install hello Azure PowerShell using hello instruction found in hello [Azure PowerShell guide](https://docs.microsoft.com/powershell/azureps-cmdlets-docs/), and then run `Login-AzureRmAccount` toocreate a connection with Azure.</span></span>

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a><span data-ttu-id="95ca9-107">Exempelskript</span><span class="sxs-lookup"><span data-stu-id="95ca9-107">Sample script</span></span>

[!code-azurepowershell[main](../../../powershell_scripts/virtual-network/peer-two-virtual-networks/peer-two-virtual-networks.ps1 "Peer two networks")]

## <a name="clean-up-deployment"></a><span data-ttu-id="95ca9-108">Rensa distribution</span><span class="sxs-lookup"><span data-stu-id="95ca9-108">Clean up deployment</span></span> 

<span data-ttu-id="95ca9-109">Kör följande kommando tooremove hello resursgrupp, VM och alla relaterade resurser hello.</span><span class="sxs-lookup"><span data-stu-id="95ca9-109">Run hello following command tooremove hello resource group, VM, and all related resources.</span></span>

```powershell
Remove-AzureRmResourceGroup -Name myResourceGroup
```

## <a name="script-explanation"></a><span data-ttu-id="95ca9-110">Skriptet förklaring</span><span class="sxs-lookup"><span data-stu-id="95ca9-110">Script explanation</span></span>

<span data-ttu-id="95ca9-111">Det här skriptet använder hello följande kommandon toocreate en resursgrupp, en virtuell dator, och alla relaterade resurser.</span><span class="sxs-lookup"><span data-stu-id="95ca9-111">This script uses hello following commands toocreate a resource group, virtual machine, and all related resources.</span></span> <span data-ttu-id="95ca9-112">Varje kommando i hello tabellen länkar toocommand viss dokumentation.</span><span class="sxs-lookup"><span data-stu-id="95ca9-112">Each command in hello table links toocommand specific documentation.</span></span>

| <span data-ttu-id="95ca9-113">Kommando</span><span class="sxs-lookup"><span data-stu-id="95ca9-113">Command</span></span> | <span data-ttu-id="95ca9-114">Anteckningar</span><span class="sxs-lookup"><span data-stu-id="95ca9-114">Notes</span></span> |
|---|---|
| [<span data-ttu-id="95ca9-115">Ny AzureRmResourceGroup</span><span class="sxs-lookup"><span data-stu-id="95ca9-115">New-AzureRmResourceGroup</span></span>](/powershell/module/azurerm.resources/new-azurermresourcegroup) | <span data-ttu-id="95ca9-116">Skapar en resursgrupp som är lagrade i alla resurser.</span><span class="sxs-lookup"><span data-stu-id="95ca9-116">Creates a resource group in which all resources are stored.</span></span> | 
| [<span data-ttu-id="95ca9-117">Ny AzureRmVirtualNetwork</span><span class="sxs-lookup"><span data-stu-id="95ca9-117">New-AzureRmVirtualNetwork</span></span>](/powershell/module/azurerm.network/new-azurermvirtualnetwork)| <span data-ttu-id="95ca9-118">Skapar ett virtuellt Azure-nätverk och undernät.</span><span class="sxs-lookup"><span data-stu-id="95ca9-118">Creates an Azure virtual network and subnet.</span></span> |
| [<span data-ttu-id="95ca9-119">Lägg till AzureRmVirtualNetworkPeering</span><span class="sxs-lookup"><span data-stu-id="95ca9-119">Add-AzureRmVirtualNetworkPeering</span></span>](/powershell/module/azurerm.network/add-azurermvirtualnetworkpeering) | <span data-ttu-id="95ca9-120">Skapar en peering mellan två virtuella nätverk.</span><span class="sxs-lookup"><span data-stu-id="95ca9-120">Creates a peering between two virtual networks.</span></span>  |
| [<span data-ttu-id="95ca9-121">Ta bort AzureRmResourceGroup</span><span class="sxs-lookup"><span data-stu-id="95ca9-121">Remove-AzureRmResourceGroup</span></span>](/powershell/module/azurerm.resources/remove-azurermresourcegroup) | <span data-ttu-id="95ca9-122">Tar bort en resursgrupp, inklusive alla kapslade resurser.</span><span class="sxs-lookup"><span data-stu-id="95ca9-122">Deletes a resource group including all nested resources.</span></span> |

## <a name="next-steps"></a><span data-ttu-id="95ca9-123">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="95ca9-123">Next steps</span></span>

<span data-ttu-id="95ca9-124">Mer information om hello Azure PowerShell finns [Azure PowerShell dokumentationen](https://docs.microsoft.com/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="95ca9-124">For more information on hello Azure PowerShell, see [Azure PowerShell documentation](https://docs.microsoft.com/powershell/azure/overview).</span></span>

<span data-ttu-id="95ca9-125">Ytterligare nätverk PowerShell-skript-exempel finns i hello [dokumentation för Azure-nätverk – översikt](../powershell-samples.md?toc=%2fazure%2fnetworking%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="95ca9-125">Additional networking PowerShell script samples can be found in hello [Azure Networking Overview documentation](../powershell-samples.md?toc=%2fazure%2fnetworking%2ftoc.json).</span></span>
