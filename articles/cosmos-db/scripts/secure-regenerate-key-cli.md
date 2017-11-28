---
title: aaaAzure CLI skript generera Azure Cosmos DB kontonyckel | Microsoft Docs
description: "Skriptexempel Azure CLI - Generera en nyckel för Azure DB som Cosmos-konto"
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
ms.openlocfilehash: ca77e05039775c90d7541899eeffc45a76d60657
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="regenerate-an-azure-cosmos-db-account-key-using-hello-azure-cli"></a><span data-ttu-id="093d7-103">Återskapa ett Azure Cosmos DB kontonyckel med hello Azure CLI</span><span class="sxs-lookup"><span data-stu-id="093d7-103">Regenerate an Azure Cosmos DB account key using hello Azure CLI</span></span>

<span data-ttu-id="093d7-104">Det här exemplet återskapar alla slags Azure Cosmos DB kontonyckel med hello Azure CLI.</span><span class="sxs-lookup"><span data-stu-id="093d7-104">This sample regenerates any kind of Azure Cosmos DB account key using hello Azure CLI.</span></span> 

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

<span data-ttu-id="093d7-105">Om du väljer tooinstall och använda hello CLI lokalt kräver i det här avsnittet att du kör hello Azure CLI version 2.0 eller senare.</span><span class="sxs-lookup"><span data-stu-id="093d7-105">If you choose tooinstall and use hello CLI locally, this topic requires that you are running hello Azure CLI version 2.0 or later.</span></span> <span data-ttu-id="093d7-106">Kör `az --version` toofind hello version.</span><span class="sxs-lookup"><span data-stu-id="093d7-106">Run `az --version` toofind hello version.</span></span> <span data-ttu-id="093d7-107">Om du behöver tooinstall eller uppgradering, se [installera Azure CLI 2.0]( /cli/azure/install-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="093d7-107">If you need tooinstall or upgrade, see [Install Azure CLI 2.0]( /cli/azure/install-azure-cli).</span></span> 

## <a name="sample-script"></a><span data-ttu-id="093d7-108">Exempelskript</span><span class="sxs-lookup"><span data-stu-id="093d7-108">Sample script</span></span>

[!code-azurecli-interactive[main](../../../cli_scripts/cosmosdb/secure-cosmosdb-regenerate-keys/secure-cosmosdb-regenerate-keys.sh?highlight=27-31 "Regenerate Azure Cosmos DB account keys")]

## <a name="clean-up-deployment"></a><span data-ttu-id="093d7-109">Rensa distribution</span><span class="sxs-lookup"><span data-stu-id="093d7-109">Clean up deployment</span></span>

<span data-ttu-id="093d7-110">Efter hello skriptexempel har körts, kan det vara hello följande kommando används tooremove hello resursgruppen och alla resurser som är kopplade till den.</span><span class="sxs-lookup"><span data-stu-id="093d7-110">After hello script sample has been run, hello following command can be used tooremove hello resource group and all resources associated with it.</span></span>

```azurecli-interactive
az group delete --name myResourceGroup
```

## <a name="script-explanation"></a><span data-ttu-id="093d7-111">Skriptet förklaring</span><span class="sxs-lookup"><span data-stu-id="093d7-111">Script explanation</span></span>

<span data-ttu-id="093d7-112">Det här skriptet använder hello följande kommandon.</span><span class="sxs-lookup"><span data-stu-id="093d7-112">This script uses hello following commands.</span></span> <span data-ttu-id="093d7-113">Varje kommando i hello tabellen länkar toocommand viss dokumentation.</span><span class="sxs-lookup"><span data-stu-id="093d7-113">Each command in hello table links toocommand specific documentation.</span></span>

| <span data-ttu-id="093d7-114">Kommando</span><span class="sxs-lookup"><span data-stu-id="093d7-114">Command</span></span> | <span data-ttu-id="093d7-115">Anteckningar</span><span class="sxs-lookup"><span data-stu-id="093d7-115">Notes</span></span> |
|---|---|
| [<span data-ttu-id="093d7-116">Skapa AZ grupp</span><span class="sxs-lookup"><span data-stu-id="093d7-116">az group create</span></span>](/cli/azure/group#create) | <span data-ttu-id="093d7-117">Skapar en resursgrupp som är lagrade i alla resurser.</span><span class="sxs-lookup"><span data-stu-id="093d7-117">Creates a resource group in which all resources are stored.</span></span> |
| [<span data-ttu-id="093d7-118">Skapa AZ cosmosdb</span><span class="sxs-lookup"><span data-stu-id="093d7-118">az cosmosdb create</span></span>](https://docs.microsoft.com/cli/azure/cosmosdb#create) | <span data-ttu-id="093d7-119">Uppdaterar ett Azure DB som Cosmos-konto.</span><span class="sxs-lookup"><span data-stu-id="093d7-119">Updates an Azure Cosmos DB account.</span></span> |
| [<span data-ttu-id="093d7-120">AZ cosmosdb Generera nyckel</span><span class="sxs-lookup"><span data-stu-id="093d7-120">az cosmosdb regenerate-key</span></span>](/cli/azure/cosmosdb/regenerate-key) | <span data-ttu-id="093d7-121">Regeneratates Azure Cosmos DB nycklar.</span><span class="sxs-lookup"><span data-stu-id="093d7-121">Regeneratates Azure Cosmos DB account keys.</span></span> |
| [<span data-ttu-id="093d7-122">ta bort grupp AZ</span><span class="sxs-lookup"><span data-stu-id="093d7-122">az group delete</span></span>](https://docs.microsoft.com/cli/azure/group#delete) | <span data-ttu-id="093d7-123">Tar bort en resursgrupp, inklusive alla kapslade resurser.</span><span class="sxs-lookup"><span data-stu-id="093d7-123">Deletes a resource group including all nested resources.</span></span> |

## <a name="next-steps"></a><span data-ttu-id="093d7-124">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="093d7-124">Next steps</span></span>

<span data-ttu-id="093d7-125">Mer information om hello Azure CLI finns [Azure CLI dokumentationen](https://docs.microsoft.com/cli/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="093d7-125">For more information on hello Azure CLI, see [Azure CLI documentation](https://docs.microsoft.com/cli/azure/overview).</span></span>

<span data-ttu-id="093d7-126">Ytterligare Azure Cosmos DB CLI skriptexempel finns i hello [Azure Cosmos DB CLI dokumentationen](../cli-samples.md).</span><span class="sxs-lookup"><span data-stu-id="093d7-126">Additional Azure Cosmos DB CLI script samples can be found in hello [Azure Cosmos DB CLI documentation](../cli-samples.md).</span></span>
