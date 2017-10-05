---
title: "CLI exempel-övervakaren-skala-enda Azure SQL database | Microsoft Docs"
description: "Azure CLI-exempelskript för att övervaka och skala en enskild Azure SQL-databas"
services: sql-database
documentationcenter: sql-database
author: janeng
manager: jstrauss
editor: carlrab
tags: azure-service-management
ms.assetid: 
ms.service: sql-database
ms.custom: monitor & tune
ms.devlang: azurecli
ms.topic: sample
ms.tgt_pltfrm: sql-database
ms.workload: database
ms.date: 06/23/2017
ms.author: janeng
ms.openlocfilehash: 01911b85268244a8fddb32aa726f8a870abbaf77
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="use-cli-to-monitor-and-scale-a-single-sql-database"></a><span data-ttu-id="ac841-103">Använd CLI för att övervaka och skala en enskild SQL-databas</span><span class="sxs-lookup"><span data-stu-id="ac841-103">Use CLI to monitor and scale a single SQL database</span></span>

<span data-ttu-id="ac841-104">Detta exempel på Azure CLI-skript skalar en enskild Azure SQL-databas till en annan prestandanivå efter hämtar Storleksinformation i databasen.</span><span class="sxs-lookup"><span data-stu-id="ac841-104">This Azure CLI script example scales a single Azure SQL database to a different performance level after querying the size information of the database.</span></span> 

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

<span data-ttu-id="ac841-105">Om du väljer att installera och använda CLI lokalt måste du köra Azure CLI version 2.0 eller senare.</span><span class="sxs-lookup"><span data-stu-id="ac841-105">If you choose to install and use the CLI locally, this topic requires that you are running the Azure CLI version 2.0 or later.</span></span> <span data-ttu-id="ac841-106">Kör `az --version` för att hitta versionen.</span><span class="sxs-lookup"><span data-stu-id="ac841-106">Run `az --version` to find the version.</span></span> <span data-ttu-id="ac841-107">Om du behöver installera eller uppgradera kan du läsa [Installera Azure CLI 2.0]( /cli/azure/install-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="ac841-107">If you need to install or upgrade, see [Install Azure CLI 2.0]( /cli/azure/install-azure-cli).</span></span> 

## <a name="sample-script"></a><span data-ttu-id="ac841-108">Exempelskript</span><span class="sxs-lookup"><span data-stu-id="ac841-108">Sample script</span></span>

<span data-ttu-id="ac841-109">[!code-azurecli-interactive[huvudsakliga](../../../cli_scripts/sql-database/monitor-and-scale-database/monitor-and-scale-database.sh "Övervakare och skala en SQL-databas")]</span><span class="sxs-lookup"><span data-stu-id="ac841-109">[!code-azurecli-interactive[main](../../../cli_scripts/sql-database/monitor-and-scale-database/monitor-and-scale-database.sh "Monitor and scale single SQL Database")]</span></span>

## <a name="clean-up-deployment"></a><span data-ttu-id="ac841-110">Rensa distribution</span><span class="sxs-lookup"><span data-stu-id="ac841-110">Clean up deployment</span></span>

<span data-ttu-id="ac841-111">Följande kommando kan användas för att ta bort resursgruppen och alla resurser som är associerade med den efter skriptexempel har körts.</span><span class="sxs-lookup"><span data-stu-id="ac841-111">After the script sample has been run, the following command can be used to remove the resource group and all resources associated with it.</span></span>

```azurecli-interactive
az group delete --name myResourceGroup
```

## <a name="script-explanation"></a><span data-ttu-id="ac841-112">Skriptet förklaring</span><span class="sxs-lookup"><span data-stu-id="ac841-112">Script explanation</span></span>

<span data-ttu-id="ac841-113">Det här skriptet använder följande kommandon.</span><span class="sxs-lookup"><span data-stu-id="ac841-113">This script uses the following commands.</span></span> <span data-ttu-id="ac841-114">Varje kommando i tabellen länkar till kommandot viss dokumentation.</span><span class="sxs-lookup"><span data-stu-id="ac841-114">Each command in the table links to command specific documentation.</span></span>

| <span data-ttu-id="ac841-115">Kommando</span><span class="sxs-lookup"><span data-stu-id="ac841-115">Command</span></span> | <span data-ttu-id="ac841-116">Anteckningar</span><span class="sxs-lookup"><span data-stu-id="ac841-116">Notes</span></span> |
|---|---|
| [<span data-ttu-id="ac841-117">Skapa AZ grupp</span><span class="sxs-lookup"><span data-stu-id="ac841-117">az group create</span></span>](https://docs.microsoft.com/cli/azure/group#create) | <span data-ttu-id="ac841-118">Skapar en resursgrupp som är lagrade i alla resurser.</span><span class="sxs-lookup"><span data-stu-id="ac841-118">Creates a resource group in which all resources are stored.</span></span> |
| [<span data-ttu-id="ac841-119">Skapa AZ SQLServer</span><span class="sxs-lookup"><span data-stu-id="ac841-119">az sql server create</span></span>](https://docs.microsoft.com/cli/azure/sql/server#create) | <span data-ttu-id="ac841-120">Skapar en logisk server som värd för en databas.</span><span class="sxs-lookup"><span data-stu-id="ac841-120">Creates a logical server that hosts a database.</span></span> |
| [<span data-ttu-id="ac841-121">AZ sql db visa-användning</span><span class="sxs-lookup"><span data-stu-id="ac841-121">az sql db show-usage</span></span>](https://docs.microsoft.com/cli/azure/sql/db#show-usage) | <span data-ttu-id="ac841-122">Visar användningsinformation storlek för en databas.</span><span class="sxs-lookup"><span data-stu-id="ac841-122">Shows the size usage information for a database.</span></span> |
| [<span data-ttu-id="ac841-123">AZ sql db-uppdateringen</span><span class="sxs-lookup"><span data-stu-id="ac841-123">az sql db update</span></span>](https://docs.microsoft.com/cli/azure/sql/db#update) | <span data-ttu-id="ac841-124">Uppdaterar Databasegenskaper (till exempel nivå eller prestanda servicenivån) eller flyttar en databas i, slut på eller mellan elastiska pooler.</span><span class="sxs-lookup"><span data-stu-id="ac841-124">Updates database properties (such as the service tier or performance level) or moves a database into, out of, or between elastic pools.</span></span> |
| [<span data-ttu-id="ac841-125">ta bort grupp AZ</span><span class="sxs-lookup"><span data-stu-id="ac841-125">az group delete</span></span>](https://docs.microsoft.com/cli/azure/vm/extension#set) | <span data-ttu-id="ac841-126">Tar bort en resursgrupp, inklusive alla kapslade resurser.</span><span class="sxs-lookup"><span data-stu-id="ac841-126">Deletes a resource group including all nested resources.</span></span> |
|||

## <a name="next-steps"></a><span data-ttu-id="ac841-127">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="ac841-127">Next steps</span></span>

<span data-ttu-id="ac841-128">Mer information om Azure CLI finns [Azure CLI dokumentationen](https://docs.microsoft.com/cli/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="ac841-128">For more information on the Azure CLI, see [Azure CLI documentation](https://docs.microsoft.com/cli/azure/overview).</span></span>

<span data-ttu-id="ac841-129">Ytterligare SQL-databas CLI skriptexempel finns i den [dokumentation för Azure SQL Database](../sql-database-cli-samples.md).</span><span class="sxs-lookup"><span data-stu-id="ac841-129">Additional SQL Database CLI script samples can be found in the [Azure SQL Database documentation](../sql-database-cli-samples.md).</span></span>
