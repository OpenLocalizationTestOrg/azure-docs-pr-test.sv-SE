---
title: "aaaAzure CLI skript skala Azure Cosmos DB behållaren genomströmning | Microsoft Docs"
description: "Skriptexempel Azure CLI - skala Azure Cosmos DB contianer genomflöde"
services: cosmos-db
documentationcenter: cosmosdb
author: mimig1
manager: jhubbard
editor: 
tags: azure-service-management
ms.assetid: 
ms.service: cosmos-db
ms.custom: mvc
ms.devlang: azurecli
ms.topic: sample
ms.tgt_pltfrm: cosmosdb
ms.workload: database
ms.date: 06/02/2017
ms.author: mimig
ms.openlocfilehash: b1a60feaf43f555a9f6ba20e5e0617f73521c0a6
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="scale-azure-cosmos-db-container-throughput-using-hello-azure-cli"></a><span data-ttu-id="18262-103">Skala Azure Cosmos DB behållaren dataflöde med hello Azure CLI</span><span class="sxs-lookup"><span data-stu-id="18262-103">Scale Azure Cosmos DB container throughput using hello Azure CLI</span></span>

<span data-ttu-id="18262-104">Det här exemplet skalas behållaren genomströmning för alla typer av Azure DB som Cosmos-behållare.</span><span class="sxs-lookup"><span data-stu-id="18262-104">This sample scales container throughput for any kind of Azure Cosmos DB container.</span></span>  

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

<span data-ttu-id="18262-105">Om du väljer tooinstall och använda hello CLI lokalt kräver i det här avsnittet att du kör hello Azure CLI version 2.0 eller senare.</span><span class="sxs-lookup"><span data-stu-id="18262-105">If you choose tooinstall and use hello CLI locally, this topic requires that you are running hello Azure CLI version 2.0 or later.</span></span> <span data-ttu-id="18262-106">Kör `az --version` toofind hello version.</span><span class="sxs-lookup"><span data-stu-id="18262-106">Run `az --version` toofind hello version.</span></span> <span data-ttu-id="18262-107">Om du behöver tooinstall eller uppgradering, se [installera Azure CLI 2.0]( /cli/azure/install-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="18262-107">If you need tooinstall or upgrade, see [Install Azure CLI 2.0]( /cli/azure/install-azure-cli).</span></span> 

## <a name="sample-script"></a><span data-ttu-id="18262-108">Exempelskript</span><span class="sxs-lookup"><span data-stu-id="18262-108">Sample script</span></span>

[!code-azurecli-interactive[main](../../../cli_scripts/cosmosdb/scale-cosmosdb-throughput/scale-cosmosdb-throughput.sh?highlight=40-46 "Scale Azure Cosmos DB throughput")]

## <a name="clean-up-deployment"></a><span data-ttu-id="18262-109">Rensa distribution</span><span class="sxs-lookup"><span data-stu-id="18262-109">Clean up deployment</span></span>

<span data-ttu-id="18262-110">Efter hello skriptexempel har körts, kan det vara hello följande kommando används tooremove hello resursgruppen och alla resurser som är kopplade till den.</span><span class="sxs-lookup"><span data-stu-id="18262-110">After hello script sample has been run, hello following command can be used tooremove hello resource group and all resources associated with it.</span></span>

```azurecli-interactive
az group delete --name myResourceGroup
```

## <a name="script-explanation"></a><span data-ttu-id="18262-111">Skriptet förklaring</span><span class="sxs-lookup"><span data-stu-id="18262-111">Script explanation</span></span>

<span data-ttu-id="18262-112">Det här skriptet använder hello följande kommandon.</span><span class="sxs-lookup"><span data-stu-id="18262-112">This script uses hello following commands.</span></span> <span data-ttu-id="18262-113">Varje kommando i hello tabellen länkar toocommand viss dokumentation.</span><span class="sxs-lookup"><span data-stu-id="18262-113">Each command in hello table links toocommand specific documentation.</span></span>

| <span data-ttu-id="18262-114">Kommando</span><span class="sxs-lookup"><span data-stu-id="18262-114">Command</span></span> | <span data-ttu-id="18262-115">Anteckningar</span><span class="sxs-lookup"><span data-stu-id="18262-115">Notes</span></span> |
|---|---|
| [<span data-ttu-id="18262-116">Skapa AZ grupp</span><span class="sxs-lookup"><span data-stu-id="18262-116">az group create</span></span>](/cli/azure/group#create) | <span data-ttu-id="18262-117">Skapar en resursgrupp som är lagrade i alla resurser.</span><span class="sxs-lookup"><span data-stu-id="18262-117">Creates a resource group in which all resources are stored.</span></span> |
| [<span data-ttu-id="18262-118">AZ cosmosdb uppdatering</span><span class="sxs-lookup"><span data-stu-id="18262-118">az cosmosdb update</span></span>](https://docs.microsoft.com/cli/azure/cosmosdb#update) | <span data-ttu-id="18262-119">Uppdaterar ett Azure DB som Cosmos-konto.</span><span class="sxs-lookup"><span data-stu-id="18262-119">Updates an Azure Cosmos DB account.</span></span> |
| [<span data-ttu-id="18262-120">ta bort grupp AZ</span><span class="sxs-lookup"><span data-stu-id="18262-120">az group delete</span></span>](https://docs.microsoft.com/cli/azure/group#delete) | <span data-ttu-id="18262-121">Tar bort en resursgrupp, inklusive alla kapslade resurser.</span><span class="sxs-lookup"><span data-stu-id="18262-121">Deletes a resource group including all nested resources.</span></span> |

## <a name="next-steps"></a><span data-ttu-id="18262-122">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="18262-122">Next steps</span></span>

<span data-ttu-id="18262-123">Mer information om hello Azure CLI finns [Azure CLI dokumentationen](https://docs.microsoft.com/cli/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="18262-123">For more information on hello Azure CLI, see [Azure CLI documentation](https://docs.microsoft.com/cli/azure/overview).</span></span>

<span data-ttu-id="18262-124">Ytterligare Azure Cosmos DB CLI skriptexempel finns i hello [Azure Cosmos DB CLI dokumentationen](../cli-samples.md).</span><span class="sxs-lookup"><span data-stu-id="18262-124">Additional Azure Cosmos DB CLI script samples can be found in hello [Azure Cosmos DB CLI documentation](../cli-samples.md).</span></span>
