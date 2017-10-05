---
title: "Azure CLI skript-skapa en brandvägg för Azure Cosmos DB | Microsoft Docs"
description: "Azure CLI-skript Sample - skapa en brandvägg för Azure Cosmos DB"
services: cosmos-db
documentationcenter: cosmosdb
author: mimig1
manager: jhubbard
editor: 
tags: azure-service-management
ms.assetid: 
ms.service: cosmos-db
ms.custom: sammvcple
ms.devlang: azurecli
ms.topic: sample
ms.tgt_pltfrm: cosmosdb
ms.workload: database
ms.date: 06/02/2017
ms.author: mimig
ms.openlocfilehash: 51f61901e1b1615e48582690dea701a01a56ebca
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/03/2017
---
# <a name="azure-cosmos-db-create-a-firewall-using-the-azure-cli"></a><span data-ttu-id="b15c6-103">Azure Cosmos DB: Skapa en brandvägg som använder Azure CLI</span><span class="sxs-lookup"><span data-stu-id="b15c6-103">Azure Cosmos DB: Create a firewall using the Azure CLI</span></span>

<span data-ttu-id="b15c6-104">Det här exempelskriptet CLI skapar en brandväggsprincip för alla typer av Azure DB som Cosmos-konto.</span><span class="sxs-lookup"><span data-stu-id="b15c6-104">This sample CLI script creates a firewall policy for any kind of Azure Cosmos DB account.</span></span> 

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

<span data-ttu-id="b15c6-105">Om du väljer att installera och använda CLI lokalt måste du köra Azure CLI version 2.0 eller senare.</span><span class="sxs-lookup"><span data-stu-id="b15c6-105">If you choose to install and use the CLI locally, this topic requires that you are running the Azure CLI version 2.0 or later.</span></span> <span data-ttu-id="b15c6-106">Kör `az --version` för att hitta versionen.</span><span class="sxs-lookup"><span data-stu-id="b15c6-106">Run `az --version` to find the version.</span></span> <span data-ttu-id="b15c6-107">Om du behöver installera eller uppgradera kan du läsa [Installera Azure CLI 2.0]( /cli/azure/install-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="b15c6-107">If you need to install or upgrade, see [Install Azure CLI 2.0]( /cli/azure/install-azure-cli).</span></span> 

## <a name="sample-script"></a><span data-ttu-id="b15c6-108">Exempelskript</span><span class="sxs-lookup"><span data-stu-id="b15c6-108">Sample script</span></span>

<span data-ttu-id="b15c6-109">[!code-azurecli-interactive[huvudsakliga](../../../cli_scripts/cosmosdb/secure-cosmosdb-create-firewall/secure-cosmosdb-create-firewall.sh?highlight=38-42 "skapa en Azure DB som Cosmos-brandvägg")]</span><span class="sxs-lookup"><span data-stu-id="b15c6-109">[!code-azurecli-interactive[main](../../../cli_scripts/cosmosdb/secure-cosmosdb-create-firewall/secure-cosmosdb-create-firewall.sh?highlight=38-42 "Create an Azure Cosmos DB firewall")]</span></span>

## <a name="clean-up-deployment"></a><span data-ttu-id="b15c6-110">Rensa distribution</span><span class="sxs-lookup"><span data-stu-id="b15c6-110">Clean up deployment</span></span>

<span data-ttu-id="b15c6-111">Följande kommando kan användas för att ta bort resursgruppen och alla resurser som är associerade med den efter skriptexempel har körts.</span><span class="sxs-lookup"><span data-stu-id="b15c6-111">After the script sample has been run, the following command can be used to remove the resource group and all resources associated with it.</span></span>

```azurecli-interactive
az group delete --name myResourceGroup
```

## <a name="script-explanation"></a><span data-ttu-id="b15c6-112">Skriptet förklaring</span><span class="sxs-lookup"><span data-stu-id="b15c6-112">Script explanation</span></span>

<span data-ttu-id="b15c6-113">Det här skriptet använder följande kommandon.</span><span class="sxs-lookup"><span data-stu-id="b15c6-113">This script uses the following commands.</span></span> <span data-ttu-id="b15c6-114">Varje kommando i tabellen länkar till kommandot viss dokumentation.</span><span class="sxs-lookup"><span data-stu-id="b15c6-114">Each command in the table links to command specific documentation.</span></span>

| <span data-ttu-id="b15c6-115">Kommando</span><span class="sxs-lookup"><span data-stu-id="b15c6-115">Command</span></span> | <span data-ttu-id="b15c6-116">Anteckningar</span><span class="sxs-lookup"><span data-stu-id="b15c6-116">Notes</span></span> |
|---|---|
| [<span data-ttu-id="b15c6-117">Skapa AZ grupp</span><span class="sxs-lookup"><span data-stu-id="b15c6-117">az group create</span></span>](https://docs.microsoft.com/cli/azure/group#create) | <span data-ttu-id="b15c6-118">Skapar en resursgrupp som är lagrade i alla resurser.</span><span class="sxs-lookup"><span data-stu-id="b15c6-118">Creates a resource group in which all resources are stored.</span></span> |
| [<span data-ttu-id="b15c6-119">Skapa AZ cosmosdb</span><span class="sxs-lookup"><span data-stu-id="b15c6-119">az cosmosdb create</span></span>](https://docs.microsoft.com/cli/azure/cosmosdb#create) | <span data-ttu-id="b15c6-120">Skapar ett CosmosDB för Azure-konto.</span><span class="sxs-lookup"><span data-stu-id="b15c6-120">Creates an Azure CosmosDB account.</span></span> |
| [<span data-ttu-id="b15c6-121">AZ cosmosdb uppdatering</span><span class="sxs-lookup"><span data-stu-id="b15c6-121">az cosmosdb update</span></span>](https://docs.microsoft.com/cli/azure/cosmosdb#update) | <span data-ttu-id="b15c6-122">Uppdaterar ett Azure CosmosDB konto om du vill inkludera brandväggsinställningar.</span><span class="sxs-lookup"><span data-stu-id="b15c6-122">Updates an Azure CosmosDB account to include firewall settings.</span></span> |
| [<span data-ttu-id="b15c6-123">ta bort grupp AZ</span><span class="sxs-lookup"><span data-stu-id="b15c6-123">az group delete</span></span>](https://docs.microsoft.com/cli/azure/group#delete) | <span data-ttu-id="b15c6-124">Tar bort en resursgrupp, inklusive alla kapslade resurser.</span><span class="sxs-lookup"><span data-stu-id="b15c6-124">Deletes a resource group including all nested resources.</span></span> |

## <a name="next-steps"></a><span data-ttu-id="b15c6-125">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="b15c6-125">Next steps</span></span>

<span data-ttu-id="b15c6-126">Mer information om Azure CLI finns [Azure CLI dokumentationen](https://docs.microsoft.com/cli/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="b15c6-126">For more information on the Azure CLI, see [Azure CLI documentation](https://docs.microsoft.com/cli/azure/overview).</span></span>

<span data-ttu-id="b15c6-127">Ytterligare Azure Cosmos DB CLI skriptexempel finns i den [Azure Cosmos DB CLI dokumentationen](../cli-samples.md).</span><span class="sxs-lookup"><span data-stu-id="b15c6-127">Additional Azure Cosmos DB CLI script samples can be found in the [Azure Cosmos DB CLI documentation](../cli-samples.md).</span></span>
