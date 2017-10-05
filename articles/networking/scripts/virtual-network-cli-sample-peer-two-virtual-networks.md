---
title: "Skriptexempel Azure CLI - Peer två virtuella nätverk | Microsoft Docs"
description: "Skriptexempel Azure CLI - Peer två virtuella nätverk"
services: virtual-network
documentationcenter: virtual-network
author: KumudD
manager: timlt
editor: tysonn
tags: 
ms.assetid: 
ms.service: virtual-network
ms.devlang: azurecli
ms.topic: article
ms.tgt_pltfrm: 
ms.workload: infrastructure
ms.date: 07/07/2017
ms.author: kumud
ms.openlocfilehash: a2b8fda288072e2fb0087988bbd68d3e4d9031d8
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="peer-two-virtual-networks"></a><span data-ttu-id="93400-103">Peer-två virtuella nätverk</span><span class="sxs-lookup"><span data-stu-id="93400-103">Peer two virtual networks</span></span>

<span data-ttu-id="93400-104">Det här skriptet skapar och ansluter två virtuella nätverk i samma region-trhough Azure-nätverk.</span><span class="sxs-lookup"><span data-stu-id="93400-104">This script creates and connects two virtual networks in the same region trhough the Azure network.</span></span> <span data-ttu-id="93400-105">När du har kört skriptet skapar du en peering mellan två virtuella nätverk.</span><span class="sxs-lookup"><span data-stu-id="93400-105">After running the script, you will create a peering between two virtual networks.</span></span>

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]


## <a name="sample-script"></a><span data-ttu-id="93400-106">Exempelskript</span><span class="sxs-lookup"><span data-stu-id="93400-106">Sample script</span></span>

<span data-ttu-id="93400-107">[!code-azurecli-interactive[huvudsakliga](../../../cli_scripts/virtual-network/peer-two-virtual-networks/peer-two-virtual-networks.sh "Peer två nätverk")]</span><span class="sxs-lookup"><span data-stu-id="93400-107">[!code-azurecli-interactive[main](../../../cli_scripts/virtual-network/peer-two-virtual-networks/peer-two-virtual-networks.sh "Peer two networks")]</span></span>

## <a name="clean-up-deployment"></a><span data-ttu-id="93400-108">Rensa distribution</span><span class="sxs-lookup"><span data-stu-id="93400-108">Clean up deployment</span></span> 

<span data-ttu-id="93400-109">Kör följande kommando för att ta bort resursgruppen, virtuell dator och alla relaterade resurser.</span><span class="sxs-lookup"><span data-stu-id="93400-109">Run the following command to remove the resource group, VM, and all related resources.</span></span>

```azurecli
az group delete --name myResourceGroup --yes
```

## <a name="script-explanation"></a><span data-ttu-id="93400-110">Skriptet förklaring</span><span class="sxs-lookup"><span data-stu-id="93400-110">Script explanation</span></span>

<span data-ttu-id="93400-111">Det här skriptet använder följande kommandon för att skapa en resursgrupp, virtuell dator och alla relaterade resurser.</span><span class="sxs-lookup"><span data-stu-id="93400-111">This script uses the following commands to create a resource group, virtual machine, and all related resources.</span></span> <span data-ttu-id="93400-112">Varje kommando i tabellen länkar till kommandot viss dokumentation.</span><span class="sxs-lookup"><span data-stu-id="93400-112">Each command in the table links to command specific documentation.</span></span>

| <span data-ttu-id="93400-113">Kommando</span><span class="sxs-lookup"><span data-stu-id="93400-113">Command</span></span> | <span data-ttu-id="93400-114">Anteckningar</span><span class="sxs-lookup"><span data-stu-id="93400-114">Notes</span></span> |
|---|---|
| [<span data-ttu-id="93400-115">Skapa AZ grupp</span><span class="sxs-lookup"><span data-stu-id="93400-115">az group create</span></span>](https://docs.microsoft.com/cli/azure/group#create) | <span data-ttu-id="93400-116">Skapar en resursgrupp som är lagrade i alla resurser.</span><span class="sxs-lookup"><span data-stu-id="93400-116">Creates a resource group in which all resources are stored.</span></span> |
| [<span data-ttu-id="93400-117">Skapa AZ network vnet</span><span class="sxs-lookup"><span data-stu-id="93400-117">az network vnet create</span></span>](https://docs.microsoft.com/cli/azure/network/vnet#create) | <span data-ttu-id="93400-118">Skapar ett virtuellt Azure-nätverk och undernät.</span><span class="sxs-lookup"><span data-stu-id="93400-118">Creates an Azure virtual network and subnet.</span></span> |
| [<span data-ttu-id="93400-119">Skapa AZ network vnet-peering</span><span class="sxs-lookup"><span data-stu-id="93400-119">az network vnet peering create</span></span>](https://docs.microsoft.com/cli/azure/network/vnet/peering#create) | <span data-ttu-id="93400-120">Skapar en peering mellan två virtuella nätverk.</span><span class="sxs-lookup"><span data-stu-id="93400-120">Creates a peering between two virtual networks.</span></span>  |
| [<span data-ttu-id="93400-121">ta bort grupp AZ</span><span class="sxs-lookup"><span data-stu-id="93400-121">az group delete</span></span>](https://docs.microsoft.com/cli/azure/vm/extension#set) | <span data-ttu-id="93400-122">Tar bort en resursgrupp, inklusive alla kapslade resurser.</span><span class="sxs-lookup"><span data-stu-id="93400-122">Deletes a resource group including all nested resources.</span></span> |

## <a name="next-steps"></a><span data-ttu-id="93400-123">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="93400-123">Next steps</span></span>

<span data-ttu-id="93400-124">Mer information om Azure CLI finns [Azure CLI dokumentationen](https://docs.microsoft.com/cli/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="93400-124">For more information on the Azure CLI, see [Azure CLI documentation](https://docs.microsoft.com/cli/azure/overview).</span></span>

<span data-ttu-id="93400-125">Ytterligare nätverk CLI skriptexempel finns i den [dokumentation för Azure-nätverk – översikt](../cli-samples.md).</span><span class="sxs-lookup"><span data-stu-id="93400-125">Additional networking CLI script samples can be found in the [Azure Networking Overview documentation](../cli-samples.md).</span></span>
