---
title: "Azure CLI skript-skapa en princip för växling vid fel för hög tillgänglighet | Microsoft Docs"
description: "Azure CLI-skript Sample - skapa en princip för växling vid fel för hög tillgänglighet"
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
ms.openlocfilehash: 96083d66cc1a2ef179f9313c1b3ed04162c1c048
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="create-a-failover-policy-for-high-availability-using-the-azure-cli"></a><span data-ttu-id="f506d-103">Skapa en princip för växling vid fel för hög tillgänglighet med hjälp av Azure CLI</span><span class="sxs-lookup"><span data-stu-id="f506d-103">Create a failover policy for high availability using the Azure CLI</span></span>

<span data-ttu-id="f506d-104">Det här exempelskriptet CLI skapar ett konto i Azure Cosmos DB och konfigurerar den för hög tillgänglighet.</span><span class="sxs-lookup"><span data-stu-id="f506d-104">This sample CLI script creates an Azure Cosmos DB account, and then configures it for high availability.</span></span>

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

<span data-ttu-id="f506d-105">Om du väljer att installera och använda CLI lokalt måste du köra Azure CLI version 2.0 eller senare.</span><span class="sxs-lookup"><span data-stu-id="f506d-105">If you choose to install and use the CLI locally, this topic requires that you are running the Azure CLI version 2.0 or later.</span></span> <span data-ttu-id="f506d-106">Kör `az --version` för att hitta versionen.</span><span class="sxs-lookup"><span data-stu-id="f506d-106">Run `az --version` to find the version.</span></span> <span data-ttu-id="f506d-107">Om du behöver installera eller uppgradera kan du läsa [Installera Azure CLI 2.0]( /cli/azure/install-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="f506d-107">If you need to install or upgrade, see [Install Azure CLI 2.0]( /cli/azure/install-azure-cli).</span></span> 

## <a name="sample-script"></a><span data-ttu-id="f506d-108">Exempelskript</span><span class="sxs-lookup"><span data-stu-id="f506d-108">Sample script</span></span>

<span data-ttu-id="f506d-109">[!code-azurecli-interactive[huvudsakliga](../../../cli_scripts/cosmosdb/high-availability-cosmosdb-configure-failover/high-availability-cosmosdb-configure-failover.sh?highlight=23-27 "skapa en princip för Azure Cosmos DB växling vid fel")]</span><span class="sxs-lookup"><span data-stu-id="f506d-109">[!code-azurecli-interactive[main](../../../cli_scripts/cosmosdb/high-availability-cosmosdb-configure-failover/high-availability-cosmosdb-configure-failover.sh?highlight=23-27 "Create an Azure Cosmos DB failover policy")]</span></span>

## <a name="clean-up-deployment"></a><span data-ttu-id="f506d-110">Rensa distribution</span><span class="sxs-lookup"><span data-stu-id="f506d-110">Clean up deployment</span></span>

<span data-ttu-id="f506d-111">Följande kommando kan användas för att ta bort resursgruppen och alla resurser som är associerade med den efter skriptexempel har körts.</span><span class="sxs-lookup"><span data-stu-id="f506d-111">After the script sample has been run, the following command can be used to remove the resource group and all resources associated with it.</span></span>

```azurecli-interactive
az group delete --name myResourceGroup
```

## <a name="script-explanation"></a><span data-ttu-id="f506d-112">Skriptet förklaring</span><span class="sxs-lookup"><span data-stu-id="f506d-112">Script explanation</span></span>

<span data-ttu-id="f506d-113">Det här skriptet använder följande kommandon.</span><span class="sxs-lookup"><span data-stu-id="f506d-113">This script uses the following commands.</span></span> <span data-ttu-id="f506d-114">Varje kommando i tabellen länkar till kommandot viss dokumentation.</span><span class="sxs-lookup"><span data-stu-id="f506d-114">Each command in the table links to command specific documentation.</span></span>

| <span data-ttu-id="f506d-115">Kommando</span><span class="sxs-lookup"><span data-stu-id="f506d-115">Command</span></span> | <span data-ttu-id="f506d-116">Anteckningar</span><span class="sxs-lookup"><span data-stu-id="f506d-116">Notes</span></span> |
|---|---|
| [<span data-ttu-id="f506d-117">Skapa AZ grupp</span><span class="sxs-lookup"><span data-stu-id="f506d-117">az group create</span></span>](/cli/azure/group#create) | <span data-ttu-id="f506d-118">Skapar en resursgrupp som är lagrade i alla resurser.</span><span class="sxs-lookup"><span data-stu-id="f506d-118">Creates a resource group in which all resources are stored.</span></span> |
| [<span data-ttu-id="f506d-119">Skapa AZ cosmosdb</span><span class="sxs-lookup"><span data-stu-id="f506d-119">az cosmosdb create</span></span>](/cli/azure/sql/server#create) | <span data-ttu-id="f506d-120">Skapar ett Azure DB som Cosmos-konto.</span><span class="sxs-lookup"><span data-stu-id="f506d-120">Creates an Azure Cosmos DB account.</span></span> |
| [<span data-ttu-id="f506d-121">AZ cosmosdb uppdatering</span><span class="sxs-lookup"><span data-stu-id="f506d-121">az cosmosdb update</span></span>](/cli/azure/cosmosdb#update) | <span data-ttu-id="f506d-122">Uppdaterar Azure DB som Cosmos-konto.</span><span class="sxs-lookup"><span data-stu-id="f506d-122">Updates Azure Cosmos DB account.</span></span> |
| [<span data-ttu-id="f506d-123">ta bort grupp AZ</span><span class="sxs-lookup"><span data-stu-id="f506d-123">az group delete</span></span>](/cli/azure/resource#delete) | <span data-ttu-id="f506d-124">Tar bort en resursgrupp, inklusive alla kapslade resurser.</span><span class="sxs-lookup"><span data-stu-id="f506d-124">Deletes a resource group including all nested resources.</span></span> |

## <a name="next-steps"></a><span data-ttu-id="f506d-125">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="f506d-125">Next steps</span></span>

<span data-ttu-id="f506d-126">Mer information om Azure CLI finns [Azure CLI dokumentationen](https://docs.microsoft.com/cli/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="f506d-126">For more information on the Azure CLI, see [Azure CLI documentation](https://docs.microsoft.com/cli/azure/overview).</span></span>

<span data-ttu-id="f506d-127">Ytterligare Azure Cosmos DB CLI skriptexempel finns i den [Azure Cosmos DB CLI dokumentationen](../cli-samples.md).</span><span class="sxs-lookup"><span data-stu-id="f506d-127">Additional Azure Cosmos DB CLI script samples can be found in the [Azure Cosmos DB CLI documentation](../cli-samples.md).</span></span>
