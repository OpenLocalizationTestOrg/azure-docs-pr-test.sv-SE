---
title: "aaaAzure CLI skriptexempel - Peer-två virtuella nätverk | Microsoft Docs"
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
ms.openlocfilehash: 54dabb2b9e05951d10f1b6b4f61ca592ce11d364
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="peer-two-virtual-networks"></a><span data-ttu-id="17cc2-103">Peer-två virtuella nätverk</span><span class="sxs-lookup"><span data-stu-id="17cc2-103">Peer two virtual networks</span></span>

<span data-ttu-id="17cc2-104">Det här skriptet skapar och ansluter två virtuella nätverk i hello samma region trhough hello Azure-nätverk.</span><span class="sxs-lookup"><span data-stu-id="17cc2-104">This script creates and connects two virtual networks in hello same region trhough hello Azure network.</span></span> <span data-ttu-id="17cc2-105">När du har kört hello skript skapar du en peering mellan två virtuella nätverk.</span><span class="sxs-lookup"><span data-stu-id="17cc2-105">After running hello script, you will create a peering between two virtual networks.</span></span>

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]


## <a name="sample-script"></a><span data-ttu-id="17cc2-106">Exempelskript</span><span class="sxs-lookup"><span data-stu-id="17cc2-106">Sample script</span></span>

[!code-azurecli-interactive[main](../../../cli_scripts/virtual-network/peer-two-virtual-networks/peer-two-virtual-networks.sh "Peer two networks")]

## <a name="clean-up-deployment"></a><span data-ttu-id="17cc2-107">Rensa distribution</span><span class="sxs-lookup"><span data-stu-id="17cc2-107">Clean up deployment</span></span> 

<span data-ttu-id="17cc2-108">Kör följande kommando tooremove hello resursgrupp, VM och alla relaterade resurser hello.</span><span class="sxs-lookup"><span data-stu-id="17cc2-108">Run hello following command tooremove hello resource group, VM, and all related resources.</span></span>

```azurecli
az group delete --name myResourceGroup --yes
```

## <a name="script-explanation"></a><span data-ttu-id="17cc2-109">Skriptet förklaring</span><span class="sxs-lookup"><span data-stu-id="17cc2-109">Script explanation</span></span>

<span data-ttu-id="17cc2-110">Det här skriptet använder hello följande kommandon toocreate en resursgrupp, en virtuell dator, och alla relaterade resurser.</span><span class="sxs-lookup"><span data-stu-id="17cc2-110">This script uses hello following commands toocreate a resource group, virtual machine, and all related resources.</span></span> <span data-ttu-id="17cc2-111">Varje kommando i hello tabellen länkar toocommand viss dokumentation.</span><span class="sxs-lookup"><span data-stu-id="17cc2-111">Each command in hello table links toocommand specific documentation.</span></span>

| <span data-ttu-id="17cc2-112">Kommando</span><span class="sxs-lookup"><span data-stu-id="17cc2-112">Command</span></span> | <span data-ttu-id="17cc2-113">Anteckningar</span><span class="sxs-lookup"><span data-stu-id="17cc2-113">Notes</span></span> |
|---|---|
| [<span data-ttu-id="17cc2-114">Skapa AZ grupp</span><span class="sxs-lookup"><span data-stu-id="17cc2-114">az group create</span></span>](https://docs.microsoft.com/cli/azure/group#create) | <span data-ttu-id="17cc2-115">Skapar en resursgrupp som är lagrade i alla resurser.</span><span class="sxs-lookup"><span data-stu-id="17cc2-115">Creates a resource group in which all resources are stored.</span></span> |
| [<span data-ttu-id="17cc2-116">Skapa AZ network vnet</span><span class="sxs-lookup"><span data-stu-id="17cc2-116">az network vnet create</span></span>](https://docs.microsoft.com/cli/azure/network/vnet#create) | <span data-ttu-id="17cc2-117">Skapar ett virtuellt Azure-nätverk och undernät.</span><span class="sxs-lookup"><span data-stu-id="17cc2-117">Creates an Azure virtual network and subnet.</span></span> |
| [<span data-ttu-id="17cc2-118">Skapa AZ network vnet-peering</span><span class="sxs-lookup"><span data-stu-id="17cc2-118">az network vnet peering create</span></span>](https://docs.microsoft.com/cli/azure/network/vnet/peering#create) | <span data-ttu-id="17cc2-119">Skapar en peering mellan två virtuella nätverk.</span><span class="sxs-lookup"><span data-stu-id="17cc2-119">Creates a peering between two virtual networks.</span></span>  |
| [<span data-ttu-id="17cc2-120">ta bort grupp AZ</span><span class="sxs-lookup"><span data-stu-id="17cc2-120">az group delete</span></span>](https://docs.microsoft.com/cli/azure/vm/extension#set) | <span data-ttu-id="17cc2-121">Tar bort en resursgrupp, inklusive alla kapslade resurser.</span><span class="sxs-lookup"><span data-stu-id="17cc2-121">Deletes a resource group including all nested resources.</span></span> |

## <a name="next-steps"></a><span data-ttu-id="17cc2-122">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="17cc2-122">Next steps</span></span>

<span data-ttu-id="17cc2-123">Mer information om hello Azure CLI finns [Azure CLI dokumentationen](https://docs.microsoft.com/cli/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="17cc2-123">For more information on hello Azure CLI, see [Azure CLI documentation](https://docs.microsoft.com/cli/azure/overview).</span></span>

<span data-ttu-id="17cc2-124">Ytterligare nätverk CLI skriptexempel finns i hello [dokumentation för Azure-nätverk – översikt](../cli-samples.md).</span><span class="sxs-lookup"><span data-stu-id="17cc2-124">Additional networking CLI script samples can be found in hello [Azure Networking Overview documentation](../cli-samples.md).</span></span>
