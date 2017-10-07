---
title: aaaAzure CLI skript-skapa en Azure Cosmos DB DocumentDB API-kontot, databas och samling | Microsoft Docs
description: "Azure CLI-skript exempel – skapa ett Azure Cosmos DB DocumentDB API-kontot, databas och samling"
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
ms.date: 06/06/2017
ms.author: mimig
ms.openlocfilehash: 53919a849e04fa69680219e51c0289b9f2affe07
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-cosmos-db-create-an-documentdb-api-account-using-cli"></a><span data-ttu-id="4682d-103">Azure Cosmos DB: Skapa ett DocumentDB-API-konto med hjälp av CLI</span><span class="sxs-lookup"><span data-stu-id="4682d-103">Azure Cosmos DB: Create an DocumentDB API account using CLI</span></span>

<span data-ttu-id="4682d-104">Det här exempelskriptet CLI skapar ett Azure Cosmos DB DocumentDB API-konto, databas och samling.</span><span class="sxs-lookup"><span data-stu-id="4682d-104">This sample CLI script creates an Azure Cosmos DB DocumentDB API account, database, and collection.</span></span>  

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

<span data-ttu-id="4682d-105">Om du väljer tooinstall och använda hello CLI lokalt kräver i det här avsnittet att du kör hello Azure CLI version 2.0 eller senare.</span><span class="sxs-lookup"><span data-stu-id="4682d-105">If you choose tooinstall and use hello CLI locally, this topic requires that you are running hello Azure CLI version 2.0 or later.</span></span> <span data-ttu-id="4682d-106">Kör `az --version` toofind hello version.</span><span class="sxs-lookup"><span data-stu-id="4682d-106">Run `az --version` toofind hello version.</span></span> <span data-ttu-id="4682d-107">Om du behöver tooinstall eller uppgradering, se [installera Azure CLI 2.0]( /cli/azure/install-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="4682d-107">If you need tooinstall or upgrade, see [Install Azure CLI 2.0]( /cli/azure/install-azure-cli).</span></span> 

## <a name="sample-script"></a><span data-ttu-id="4682d-108">Exempelskript</span><span class="sxs-lookup"><span data-stu-id="4682d-108">Sample script</span></span>

[!code-azurecli-interactive[main](../../../cli_scripts/cosmosdb/create-cosmosdb-account-database/create-cosmosdb-account-database.sh?highlight=15-35 "Create an Azure Cosmos DB DocumentDB API account, database, and collection")]

## <a name="clean-up-deployment"></a><span data-ttu-id="4682d-109">Rensa distribution</span><span class="sxs-lookup"><span data-stu-id="4682d-109">Clean up deployment</span></span>

<span data-ttu-id="4682d-110">Efter hello skriptexempel har körts, kan det vara hello följande kommando används tooremove hello resursgruppen och alla resurser som är kopplade till den.</span><span class="sxs-lookup"><span data-stu-id="4682d-110">After hello script sample has been run, hello following command can be used tooremove hello resource group and all resources associated with it.</span></span>

```azurecli-interactive
az group delete --name myResourceGroup
```

## <a name="script-explanation"></a><span data-ttu-id="4682d-111">Skriptet förklaring</span><span class="sxs-lookup"><span data-stu-id="4682d-111">Script explanation</span></span>

<span data-ttu-id="4682d-112">Det här skriptet använder hello följande kommandon.</span><span class="sxs-lookup"><span data-stu-id="4682d-112">This script uses hello following commands.</span></span> <span data-ttu-id="4682d-113">Varje kommando i hello tabellen länkar toocommand viss dokumentation.</span><span class="sxs-lookup"><span data-stu-id="4682d-113">Each command in hello table links toocommand specific documentation.</span></span>

| <span data-ttu-id="4682d-114">Kommando</span><span class="sxs-lookup"><span data-stu-id="4682d-114">Command</span></span> | <span data-ttu-id="4682d-115">Anteckningar</span><span class="sxs-lookup"><span data-stu-id="4682d-115">Notes</span></span> |
|---|---|
| [<span data-ttu-id="4682d-116">Skapa AZ grupp</span><span class="sxs-lookup"><span data-stu-id="4682d-116">az group create</span></span>](/cli/azure/group#create) | <span data-ttu-id="4682d-117">Skapar en resursgrupp som är lagrade i alla resurser.</span><span class="sxs-lookup"><span data-stu-id="4682d-117">Creates a resource group in which all resources are stored.</span></span> |
| [<span data-ttu-id="4682d-118">Skapa AZ cosmosdb</span><span class="sxs-lookup"><span data-stu-id="4682d-118">az cosmosdb create</span></span>](/cli/azure/cosmosdb#create) | <span data-ttu-id="4682d-119">Skapar ett Azure DB som Cosmos-konto.</span><span class="sxs-lookup"><span data-stu-id="4682d-119">Creates an Azure Cosmos DB account.</span></span> |
| [<span data-ttu-id="4682d-120">ta bort grupp AZ</span><span class="sxs-lookup"><span data-stu-id="4682d-120">az group delete</span></span>](/cli/azure/resource#delete) | <span data-ttu-id="4682d-121">Tar bort en resursgrupp, inklusive alla kapslade resurser.</span><span class="sxs-lookup"><span data-stu-id="4682d-121">Deletes a resource group including all nested resources.</span></span> |

## <a name="next-steps"></a><span data-ttu-id="4682d-122">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="4682d-122">Next steps</span></span>

<span data-ttu-id="4682d-123">Mer information om hello Azure CLI finns [Azure CLI dokumentationen](https://docs.microsoft.com/cli/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="4682d-123">For more information on hello Azure CLI, see [Azure CLI documentation](https://docs.microsoft.com/cli/azure/overview).</span></span>

<span data-ttu-id="4682d-124">Ytterligare Azure Cosmos DB CLI skriptexempel finns i hello [Azure Cosmos DB CLI dokumentationen](../cli-samples.md).</span><span class="sxs-lookup"><span data-stu-id="4682d-124">Additional Azure Cosmos DB CLI script samples can be found in hello [Azure Cosmos DB CLI documentation](../cli-samples.md).</span></span>
