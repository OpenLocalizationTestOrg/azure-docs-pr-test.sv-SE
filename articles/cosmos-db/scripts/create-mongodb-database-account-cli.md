---
title: Azure CLI skript-skapa ett Azure Cosmos DB MongoDB API-konto, databas och samling | Microsoft Docs
description: "Azure CLI-skript exempel – skapa ett Azure Cosmos DB MongoDB API-kontot, databas och samling"
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
ms.openlocfilehash: b0bf637db90cfcb987ad43ed34cb8065d28b0fcf
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="azure-cosmos-db-create-an-mongodb-api-account-using-the-azure-cli"></a><span data-ttu-id="37b75-103">Azure Cosmos DB: Skapa ett MongoDB API-konto med hjälp av Azure CLI</span><span class="sxs-lookup"><span data-stu-id="37b75-103">Azure Cosmos DB: Create an MongoDB API account using the Azure CLI</span></span>

<span data-ttu-id="37b75-104">Det här exempelskriptet CLI skapar ett Azure Cosmos DB MongoDB API-konto, databas och samling.</span><span class="sxs-lookup"><span data-stu-id="37b75-104">This sample CLI script creates an Azure Cosmos DB MongoDB API account, database, and collection.</span></span> 

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

<span data-ttu-id="37b75-105">Om du väljer att installera och använda CLI lokalt måste du köra Azure CLI version 2.0 eller senare.</span><span class="sxs-lookup"><span data-stu-id="37b75-105">If you choose to install and use the CLI locally, this topic requires that you are running the Azure CLI version 2.0 or later.</span></span> <span data-ttu-id="37b75-106">Kör `az --version` för att hitta versionen.</span><span class="sxs-lookup"><span data-stu-id="37b75-106">Run `az --version` to find the version.</span></span> <span data-ttu-id="37b75-107">Om du behöver installera eller uppgradera kan du läsa [Installera Azure CLI 2.0]( /cli/azure/install-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="37b75-107">If you need to install or upgrade, see [Install Azure CLI 2.0]( /cli/azure/install-azure-cli).</span></span> 

## <a name="sample-script"></a><span data-ttu-id="37b75-108">Exempelskript</span><span class="sxs-lookup"><span data-stu-id="37b75-108">Sample script</span></span>

<span data-ttu-id="37b75-109">[!code-azurecli-interactive[huvudsakliga](../../../cli_scripts/cosmosdb/create-cosmosdb-mongodb-account/create-cosmosdb-mongodb-account.sh?highlight=15-35 "skapa ett Azure Cosmos DB MongoDB API-kontot, databas och samling")]</span><span class="sxs-lookup"><span data-stu-id="37b75-109">[!code-azurecli-interactive[main](../../../cli_scripts/cosmosdb/create-cosmosdb-mongodb-account/create-cosmosdb-mongodb-account.sh?highlight=15-35 "Create an Azure Cosmos DB MongoDB API account, database, and collection")]</span></span>

## <a name="clean-up-deployment"></a><span data-ttu-id="37b75-110">Rensa distribution</span><span class="sxs-lookup"><span data-stu-id="37b75-110">Clean up deployment</span></span>

<span data-ttu-id="37b75-111">Följande kommando kan användas för att ta bort resursgruppen och alla resurser som är associerade med den efter skriptexempel har körts.</span><span class="sxs-lookup"><span data-stu-id="37b75-111">After the script sample has been run, the following command can be used to remove the resource group and all resources associated with it.</span></span>

```azurecli-interactive
az group delete --name myResourceGroup
```

## <a name="script-explanation"></a><span data-ttu-id="37b75-112">Skriptet förklaring</span><span class="sxs-lookup"><span data-stu-id="37b75-112">Script explanation</span></span>

<span data-ttu-id="37b75-113">Det här skriptet använder följande kommandon.</span><span class="sxs-lookup"><span data-stu-id="37b75-113">This script uses the following commands.</span></span> <span data-ttu-id="37b75-114">Varje kommando i tabellen länkar till kommandot viss dokumentation.</span><span class="sxs-lookup"><span data-stu-id="37b75-114">Each command in the table links to command specific documentation.</span></span>

| <span data-ttu-id="37b75-115">Kommando</span><span class="sxs-lookup"><span data-stu-id="37b75-115">Command</span></span> | <span data-ttu-id="37b75-116">Anteckningar</span><span class="sxs-lookup"><span data-stu-id="37b75-116">Notes</span></span> |
|---|---|
| [<span data-ttu-id="37b75-117">Skapa AZ grupp</span><span class="sxs-lookup"><span data-stu-id="37b75-117">az group create</span></span>](/cli/azure/group#create) | <span data-ttu-id="37b75-118">Skapar en resursgrupp som är lagrade i alla resurser.</span><span class="sxs-lookup"><span data-stu-id="37b75-118">Creates a resource group in which all resources are stored.</span></span> |
| [<span data-ttu-id="37b75-119">Skapa AZ cosmosdb</span><span class="sxs-lookup"><span data-stu-id="37b75-119">az cosmosdb create</span></span>](/cli/azure/cosmosdb#create) | <span data-ttu-id="37b75-120">Skapar ett Azure DB som Cosmos-konto.</span><span class="sxs-lookup"><span data-stu-id="37b75-120">Creates an Azure Cosmos DB account.</span></span> |
| [<span data-ttu-id="37b75-121">ta bort grupp AZ</span><span class="sxs-lookup"><span data-stu-id="37b75-121">az group delete</span></span>](/cli/azure/resource#delete) | <span data-ttu-id="37b75-122">Tar bort en resursgrupp, inklusive alla kapslade resurser.</span><span class="sxs-lookup"><span data-stu-id="37b75-122">Deletes a resource group including all nested resources.</span></span> |

## <a name="next-steps"></a><span data-ttu-id="37b75-123">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="37b75-123">Next steps</span></span>

<span data-ttu-id="37b75-124">Mer information om Azure CLI finns [Azure CLI dokumentationen](https://docs.microsoft.com/cli/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="37b75-124">For more information on the Azure CLI, see [Azure CLI documentation](https://docs.microsoft.com/cli/azure/overview).</span></span>

<span data-ttu-id="37b75-125">Ytterligare Azure Cosmos DB CLI skriptexempel finns i den [Azure Cosmos DB CLI dokumentationen](../cli-samples.md).</span><span class="sxs-lookup"><span data-stu-id="37b75-125">Additional Azure Cosmos DB CLI script samples can be found in the [Azure Cosmos DB CLI documentation](../cli-samples.md).</span></span>
