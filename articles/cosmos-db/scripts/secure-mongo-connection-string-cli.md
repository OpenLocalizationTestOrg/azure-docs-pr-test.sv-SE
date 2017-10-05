---
title: "Azure CLI skript-hämta Azure Cosmos DB-anslutningssträngen för MongoDB-appar | Microsoft Docs"
description: "Skriptexempel Azure CLI - hämta Azure Cosmos DB-anslutningssträngen för MongoDB-appar"
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
ms.openlocfilehash: 916c92cab39306352fdf9dff0e0685fd61832d16
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/03/2017
---
# <a name="get-an-azure-cosmos-db-connection-string-for-mongodb-apps-using-the-azure-cli"></a><span data-ttu-id="d7fa7-103">Hämta en Azure Cosmos DB-anslutningssträngen för MongoDB-appar som använder Azure CLI</span><span class="sxs-lookup"><span data-stu-id="d7fa7-103">Get an Azure Cosmos DB connection string for MongoDB apps using the Azure CLI</span></span>

<span data-ttu-id="d7fa7-104">Det här exemplet hämtar en Azure Cosmos DB-anslutningssträng för MongoDB-appar som använder Azure CLI.</span><span class="sxs-lookup"><span data-stu-id="d7fa7-104">This sample gets an Azure Cosmos DB connection string for MongoDB apps using the Azure CLI.</span></span> 

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

<span data-ttu-id="d7fa7-105">Om du väljer att installera och använda CLI lokalt måste du köra Azure CLI version 2.0 eller senare.</span><span class="sxs-lookup"><span data-stu-id="d7fa7-105">If you choose to install and use the CLI locally, this topic requires that you are running the Azure CLI version 2.0 or later.</span></span> <span data-ttu-id="d7fa7-106">Kör `az --version` för att hitta versionen.</span><span class="sxs-lookup"><span data-stu-id="d7fa7-106">Run `az --version` to find the version.</span></span> <span data-ttu-id="d7fa7-107">Om du behöver installera eller uppgradera kan du läsa [Installera Azure CLI 2.0]( /cli/azure/install-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="d7fa7-107">If you need to install or upgrade, see [Install Azure CLI 2.0]( /cli/azure/install-azure-cli).</span></span> 

## <a name="sample-script"></a><span data-ttu-id="d7fa7-108">Exempelskript</span><span class="sxs-lookup"><span data-stu-id="d7fa7-108">Sample script</span></span>

<span data-ttu-id="d7fa7-109">[!code-azurecli-interactive[huvudsakliga](../../../cli_scripts/cosmosdb/secure-cosmosdb-get-mongodb-connection-string/secure-cosmosdb-get-mongodb-connection-string.sh?highlight=36-39 "hämta Azure Cosmos-DB-anslutningssträngen för MongoDB-appar")]</span><span class="sxs-lookup"><span data-stu-id="d7fa7-109">[!code-azurecli-interactive[main](../../../cli_scripts/cosmosdb/secure-cosmosdb-get-mongodb-connection-string/secure-cosmosdb-get-mongodb-connection-string.sh?highlight=36-39 "Get Azure Cosmos DB connection string for MongoDB apps")]</span></span>

## <a name="clean-up-deployment"></a><span data-ttu-id="d7fa7-110">Rensa distribution</span><span class="sxs-lookup"><span data-stu-id="d7fa7-110">Clean up deployment</span></span>

<span data-ttu-id="d7fa7-111">Följande kommando kan användas för att ta bort resursgruppen och alla resurser som är associerade med den efter skriptexempel har körts.</span><span class="sxs-lookup"><span data-stu-id="d7fa7-111">After the script sample has been run, the following command can be used to remove the resource group and all resources associated with it.</span></span>

```azurecli-interactive
az group delete --name myResourceGroup
```

## <a name="script-explanation"></a><span data-ttu-id="d7fa7-112">Skriptet förklaring</span><span class="sxs-lookup"><span data-stu-id="d7fa7-112">Script explanation</span></span>

<span data-ttu-id="d7fa7-113">Det här skriptet använder följande kommandon.</span><span class="sxs-lookup"><span data-stu-id="d7fa7-113">This script uses the following commands.</span></span> <span data-ttu-id="d7fa7-114">Varje kommando i tabellen länkar till kommandot viss dokumentation.</span><span class="sxs-lookup"><span data-stu-id="d7fa7-114">Each command in the table links to command specific documentation.</span></span>

| <span data-ttu-id="d7fa7-115">Kommando</span><span class="sxs-lookup"><span data-stu-id="d7fa7-115">Command</span></span> | <span data-ttu-id="d7fa7-116">Anteckningar</span><span class="sxs-lookup"><span data-stu-id="d7fa7-116">Notes</span></span> |
|---|---|
| [<span data-ttu-id="d7fa7-117">Skapa AZ grupp</span><span class="sxs-lookup"><span data-stu-id="d7fa7-117">az group create</span></span>](https://docs.microsoft.com/cli/azure/group#create) | <span data-ttu-id="d7fa7-118">Skapar en resursgrupp som är lagrade i alla resurser.</span><span class="sxs-lookup"><span data-stu-id="d7fa7-118">Creates a resource group in which all resources are stored.</span></span> |
| [<span data-ttu-id="d7fa7-119">AZ cosmosdb uppdatering</span><span class="sxs-lookup"><span data-stu-id="d7fa7-119">az cosmosdb update</span></span>](https://docs.microsoft.com/cli/azure/cosmosdb#update) | <span data-ttu-id="d7fa7-120">Uppdaterar ett Azure DB som Cosmos-konto.</span><span class="sxs-lookup"><span data-stu-id="d7fa7-120">Updates an Azure Cosmos DB account.</span></span> |
| [<span data-ttu-id="d7fa7-121">AZ cosmosdb lista--anslutningssträngar</span><span class="sxs-lookup"><span data-stu-id="d7fa7-121">az cosmosdb list-connection-strings</span></span>](https://docs.microsoft.com/cli/azure/cosmosdb#list-connection-strings) | <span data-ttu-id="d7fa7-122">Hämtar anslutningssträngen för kontot.</span><span class="sxs-lookup"><span data-stu-id="d7fa7-122">Gets the connection string for the account.</span></span>|
| [<span data-ttu-id="d7fa7-123">ta bort grupp AZ</span><span class="sxs-lookup"><span data-stu-id="d7fa7-123">az group delete</span></span>](https://docs.microsoft.com/cli/azure/group#delete) | <span data-ttu-id="d7fa7-124">Tar bort en resursgrupp, inklusive alla kapslade resurser.</span><span class="sxs-lookup"><span data-stu-id="d7fa7-124">Deletes a resource group including all nested resources.</span></span> |

## <a name="next-steps"></a><span data-ttu-id="d7fa7-125">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="d7fa7-125">Next steps</span></span>

<span data-ttu-id="d7fa7-126">Mer information om Azure CLI finns [Azure CLI dokumentationen](https://docs.microsoft.com/cli/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="d7fa7-126">For more information on the Azure CLI, see [Azure CLI documentation](https://docs.microsoft.com/cli/azure/overview).</span></span>

<span data-ttu-id="d7fa7-127">Ytterligare Azure Cosmos DB CLI skriptexempel finns i den [Azure Cosmos DB CLI dokumentationen](../cli-samples.md).</span><span class="sxs-lookup"><span data-stu-id="d7fa7-127">Additional Azure Cosmos DB CLI script samples can be found in the [Azure Cosmos DB CLI documentation](../cli-samples.md).</span></span>
