---
title: "Azure PowerShell skriptexempel - Peer två virtuella nätverk | Microsoft Docs"
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
ms.openlocfilehash: 51c0b98727e148671cfd7ab2b31ffd1c705d8a4e
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="peer-two-virtual-networks"></a><span data-ttu-id="82dae-103">Peer-två virtuella nätverk</span><span class="sxs-lookup"><span data-stu-id="82dae-103">Peer two virtual networks</span></span>

<span data-ttu-id="82dae-104">Det här skriptet skapar och ansluter två virtuella nätverk i samma region-trhough Azure-nätverk.</span><span class="sxs-lookup"><span data-stu-id="82dae-104">This script creates and connects two virtual networks in the same region trhough the Azure network.</span></span> <span data-ttu-id="82dae-105">När du har kört skriptet skapar du en peering mellan två virtuella nätverk.</span><span class="sxs-lookup"><span data-stu-id="82dae-105">After running the script, you will create a peering between two virtual networks.</span></span>

<span data-ttu-id="82dae-106">Om det behövs installerar du Azure PowerShell med hjälp av anvisningarna i den [Azure PowerShell guiden](https://docs.microsoft.com/powershell/azureps-cmdlets-docs/), och kör sedan `Login-AzureRmAccount` att skapa en anslutning med Azure.</span><span class="sxs-lookup"><span data-stu-id="82dae-106">If needed, install the Azure PowerShell using the instruction found in the [Azure PowerShell guide](https://docs.microsoft.com/powershell/azureps-cmdlets-docs/), and then run `Login-AzureRmAccount` to create a connection with Azure.</span></span>

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a><span data-ttu-id="82dae-107">Exempelskript</span><span class="sxs-lookup"><span data-stu-id="82dae-107">Sample script</span></span>

<span data-ttu-id="82dae-108">[!code-azurepowershell[huvudsakliga](../../../powershell_scripts/virtual-network/peer-two-virtual-networks/peer-two-virtual-networks.ps1 "Peer två nätverk")]</span><span class="sxs-lookup"><span data-stu-id="82dae-108">[!code-azurepowershell[main](../../../powershell_scripts/virtual-network/peer-two-virtual-networks/peer-two-virtual-networks.ps1 "Peer two networks")]</span></span>

## <a name="clean-up-deployment"></a><span data-ttu-id="82dae-109">Rensa distribution</span><span class="sxs-lookup"><span data-stu-id="82dae-109">Clean up deployment</span></span> 

<span data-ttu-id="82dae-110">Kör följande kommando för att ta bort resursgruppen, virtuell dator och alla relaterade resurser.</span><span class="sxs-lookup"><span data-stu-id="82dae-110">Run the following command to remove the resource group, VM, and all related resources.</span></span>

```powershell
Remove-AzureRmResourceGroup -Name myResourceGroup
```

## <a name="script-explanation"></a><span data-ttu-id="82dae-111">Skriptet förklaring</span><span class="sxs-lookup"><span data-stu-id="82dae-111">Script explanation</span></span>

<span data-ttu-id="82dae-112">Det här skriptet använder följande kommandon för att skapa en resursgrupp, virtuell dator och alla relaterade resurser.</span><span class="sxs-lookup"><span data-stu-id="82dae-112">This script uses the following commands to create a resource group, virtual machine, and all related resources.</span></span> <span data-ttu-id="82dae-113">Varje kommando i tabellen länkar till kommandot viss dokumentation.</span><span class="sxs-lookup"><span data-stu-id="82dae-113">Each command in the table links to command specific documentation.</span></span>

| <span data-ttu-id="82dae-114">Kommando</span><span class="sxs-lookup"><span data-stu-id="82dae-114">Command</span></span> | <span data-ttu-id="82dae-115">Anteckningar</span><span class="sxs-lookup"><span data-stu-id="82dae-115">Notes</span></span> |
|---|---|
| [<span data-ttu-id="82dae-116">Ny AzureRmResourceGroup</span><span class="sxs-lookup"><span data-stu-id="82dae-116">New-AzureRmResourceGroup</span></span>](/powershell/module/azurerm.resources/new-azurermresourcegroup) | <span data-ttu-id="82dae-117">Skapar en resursgrupp som är lagrade i alla resurser.</span><span class="sxs-lookup"><span data-stu-id="82dae-117">Creates a resource group in which all resources are stored.</span></span> | 
| [<span data-ttu-id="82dae-118">Ny AzureRmVirtualNetwork</span><span class="sxs-lookup"><span data-stu-id="82dae-118">New-AzureRmVirtualNetwork</span></span>](/powershell/module/azurerm.network/new-azurermvirtualnetwork)| <span data-ttu-id="82dae-119">Skapar ett virtuellt Azure-nätverk och undernät.</span><span class="sxs-lookup"><span data-stu-id="82dae-119">Creates an Azure virtual network and subnet.</span></span> |
| [<span data-ttu-id="82dae-120">Lägg till AzureRmVirtualNetworkPeering</span><span class="sxs-lookup"><span data-stu-id="82dae-120">Add-AzureRmVirtualNetworkPeering</span></span>](/powershell/module/azurerm.network/add-azurermvirtualnetworkpeering) | <span data-ttu-id="82dae-121">Skapar en peering mellan två virtuella nätverk.</span><span class="sxs-lookup"><span data-stu-id="82dae-121">Creates a peering between two virtual networks.</span></span>  |
| [<span data-ttu-id="82dae-122">Ta bort AzureRmResourceGroup</span><span class="sxs-lookup"><span data-stu-id="82dae-122">Remove-AzureRmResourceGroup</span></span>](/powershell/module/azurerm.resources/remove-azurermresourcegroup) | <span data-ttu-id="82dae-123">Tar bort en resursgrupp, inklusive alla kapslade resurser.</span><span class="sxs-lookup"><span data-stu-id="82dae-123">Deletes a resource group including all nested resources.</span></span> |

## <a name="next-steps"></a><span data-ttu-id="82dae-124">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="82dae-124">Next steps</span></span>

<span data-ttu-id="82dae-125">Mer information om Azure PowerShell finns [Azure PowerShell dokumentationen](https://docs.microsoft.com/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="82dae-125">For more information on the Azure PowerShell, see [Azure PowerShell documentation](https://docs.microsoft.com/powershell/azure/overview).</span></span>

<span data-ttu-id="82dae-126">Ytterligare nätverk PowerShell-skript-exempel finns i den [dokumentation för Azure-nätverk – översikt](../powershell-samples.md?toc=%2fazure%2fnetworking%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="82dae-126">Additional networking PowerShell script samples can be found in the [Azure Networking Overview documentation](../powershell-samples.md?toc=%2fazure%2fnetworking%2ftoc.json).</span></span>