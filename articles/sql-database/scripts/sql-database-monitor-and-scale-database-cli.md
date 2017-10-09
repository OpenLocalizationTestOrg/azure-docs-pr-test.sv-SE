---
title: "aaaCLI exempel-övervakaren-skala-enda Azure SQL database | Microsoft Docs"
description: Azure CLI exempel skriptet toomonitor och skala en enskild Azure SQL-databas
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
ms.openlocfilehash: 36031ddd46a947a80fe37884858a84eb66217270
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="use-cli-toomonitor-and-scale-a-single-sql-database"></a><span data-ttu-id="9e849-103">Använd CLI toomonitor och skala en enskild SQL-databas</span><span class="sxs-lookup"><span data-stu-id="9e849-103">Use CLI toomonitor and scale a single SQL database</span></span>

<span data-ttu-id="9e849-104">Exemplet Azure CLI skript skalar en enda Azure SQL database tooa olika prestandanivå efter hämtar information om hello storleken på hello-databasen.</span><span class="sxs-lookup"><span data-stu-id="9e849-104">This Azure CLI script example scales a single Azure SQL database tooa different performance level after querying hello size information of hello database.</span></span> 

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

<span data-ttu-id="9e849-105">Om du väljer tooinstall och använda hello CLI lokalt kräver i det här avsnittet att du kör hello Azure CLI version 2.0 eller senare.</span><span class="sxs-lookup"><span data-stu-id="9e849-105">If you choose tooinstall and use hello CLI locally, this topic requires that you are running hello Azure CLI version 2.0 or later.</span></span> <span data-ttu-id="9e849-106">Kör `az --version` toofind hello version.</span><span class="sxs-lookup"><span data-stu-id="9e849-106">Run `az --version` toofind hello version.</span></span> <span data-ttu-id="9e849-107">Om du behöver tooinstall eller uppgradering, se [installera Azure CLI 2.0]( /cli/azure/install-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="9e849-107">If you need tooinstall or upgrade, see [Install Azure CLI 2.0]( /cli/azure/install-azure-cli).</span></span> 

## <a name="sample-script"></a><span data-ttu-id="9e849-108">Exempelskript</span><span class="sxs-lookup"><span data-stu-id="9e849-108">Sample script</span></span>

[!code-azurecli-interactive[main](../../../cli_scripts/sql-database/monitor-and-scale-database/monitor-and-scale-database.sh "Monitor and scale single SQL Database")]

## <a name="clean-up-deployment"></a><span data-ttu-id="9e849-109">Rensa distribution</span><span class="sxs-lookup"><span data-stu-id="9e849-109">Clean up deployment</span></span>

<span data-ttu-id="9e849-110">Efter hello skriptexempel har körts, kan det vara hello följande kommando används tooremove hello resursgruppen och alla resurser som är kopplade till den.</span><span class="sxs-lookup"><span data-stu-id="9e849-110">After hello script sample has been run, hello following command can be used tooremove hello resource group and all resources associated with it.</span></span>

```azurecli-interactive
az group delete --name myResourceGroup
```

## <a name="script-explanation"></a><span data-ttu-id="9e849-111">Skriptet förklaring</span><span class="sxs-lookup"><span data-stu-id="9e849-111">Script explanation</span></span>

<span data-ttu-id="9e849-112">Det här skriptet använder hello följande kommandon.</span><span class="sxs-lookup"><span data-stu-id="9e849-112">This script uses hello following commands.</span></span> <span data-ttu-id="9e849-113">Varje kommando i hello tabellen länkar toocommand viss dokumentation.</span><span class="sxs-lookup"><span data-stu-id="9e849-113">Each command in hello table links toocommand specific documentation.</span></span>

| <span data-ttu-id="9e849-114">Kommando</span><span class="sxs-lookup"><span data-stu-id="9e849-114">Command</span></span> | <span data-ttu-id="9e849-115">Anteckningar</span><span class="sxs-lookup"><span data-stu-id="9e849-115">Notes</span></span> |
|---|---|
| [<span data-ttu-id="9e849-116">Skapa AZ grupp</span><span class="sxs-lookup"><span data-stu-id="9e849-116">az group create</span></span>](https://docs.microsoft.com/cli/azure/group#create) | <span data-ttu-id="9e849-117">Skapar en resursgrupp som är lagrade i alla resurser.</span><span class="sxs-lookup"><span data-stu-id="9e849-117">Creates a resource group in which all resources are stored.</span></span> |
| [<span data-ttu-id="9e849-118">Skapa AZ SQLServer</span><span class="sxs-lookup"><span data-stu-id="9e849-118">az sql server create</span></span>](https://docs.microsoft.com/cli/azure/sql/server#create) | <span data-ttu-id="9e849-119">Skapar en logisk server som värd för en databas.</span><span class="sxs-lookup"><span data-stu-id="9e849-119">Creates a logical server that hosts a database.</span></span> |
| [<span data-ttu-id="9e849-120">AZ sql db visa-användning</span><span class="sxs-lookup"><span data-stu-id="9e849-120">az sql db show-usage</span></span>](https://docs.microsoft.com/cli/azure/sql/db#show-usage) | <span data-ttu-id="9e849-121">Visar information om användning av hello storlek för en databas.</span><span class="sxs-lookup"><span data-stu-id="9e849-121">Shows hello size usage information for a database.</span></span> |
| [<span data-ttu-id="9e849-122">AZ sql db-uppdateringen</span><span class="sxs-lookup"><span data-stu-id="9e849-122">az sql db update</span></span>](https://docs.microsoft.com/cli/azure/sql/db#update) | <span data-ttu-id="9e849-123">Uppdaterar Databasegenskaper (till exempel hello nivå eller prestanda servicenivå) eller flyttar en databas i, slut på eller mellan elastiska pooler.</span><span class="sxs-lookup"><span data-stu-id="9e849-123">Updates database properties (such as hello service tier or performance level) or moves a database into, out of, or between elastic pools.</span></span> |
| [<span data-ttu-id="9e849-124">ta bort grupp AZ</span><span class="sxs-lookup"><span data-stu-id="9e849-124">az group delete</span></span>](https://docs.microsoft.com/cli/azure/vm/extension#set) | <span data-ttu-id="9e849-125">Tar bort en resursgrupp, inklusive alla kapslade resurser.</span><span class="sxs-lookup"><span data-stu-id="9e849-125">Deletes a resource group including all nested resources.</span></span> |
|||

## <a name="next-steps"></a><span data-ttu-id="9e849-126">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="9e849-126">Next steps</span></span>

<span data-ttu-id="9e849-127">Mer information om hello Azure CLI finns [Azure CLI dokumentationen](https://docs.microsoft.com/cli/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="9e849-127">For more information on hello Azure CLI, see [Azure CLI documentation](https://docs.microsoft.com/cli/azure/overview).</span></span>

<span data-ttu-id="9e849-128">Ytterligare SQL-databas CLI skriptexempel finns i hello [dokumentation för Azure SQL Database](../sql-database-cli-samples.md).</span><span class="sxs-lookup"><span data-stu-id="9e849-128">Additional SQL Database CLI script samples can be found in hello [Azure SQL Database documentation](../sql-database-cli-samples.md).</span></span>
