---
title: aaaCLI exempel flytta Azure SQL database-elastisk SQL-pool | Microsoft Docs
description: Azure CLI exempel skriptet toomove en SQL-databas i en elastisk SQL-pool
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
ms.date: 07/05/2017
ms.author: janeng
ms.openlocfilehash: 841eb57d2d49612c3fadd3a6424a2b0309c69719
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="use-cli-toomove-an-azure-sql-database-in-a-sql-elastic-pool"></a><span data-ttu-id="73350-103">Använd CLI toomove Azure SQL-databas i en elastisk SQL-pool</span><span class="sxs-lookup"><span data-stu-id="73350-103">Use CLI toomove an Azure SQL database in a SQL elastic pool</span></span>

<span data-ttu-id="73350-104">Detta exempel på Azure CLI-skript skapar två elastiska pooler flyttar en Azure SQL-databas från en elastiska SQL-poolen till en annan elastisk SQL-pool och sedan flyttas hello databasen utanför elastisk pool tooa enda Azure databasens prestandanivå.</span><span class="sxs-lookup"><span data-stu-id="73350-104">This Azure CLI script example creates two elastic pools and moves an Azure SQL database from one SQL elastic pool into another SQL elastic pool, and then moves hello database out of elastic pool tooa single Azure database performance level.</span></span> 

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

<span data-ttu-id="73350-105">Om du väljer tooinstall och använda hello CLI lokalt kräver i det här avsnittet att du kör hello Azure CLI version 2.0 eller senare.</span><span class="sxs-lookup"><span data-stu-id="73350-105">If you choose tooinstall and use hello CLI locally, this topic requires that you are running hello Azure CLI version 2.0 or later.</span></span> <span data-ttu-id="73350-106">Kör `az --version` toofind hello version.</span><span class="sxs-lookup"><span data-stu-id="73350-106">Run `az --version` toofind hello version.</span></span> <span data-ttu-id="73350-107">Om du behöver tooinstall eller uppgradering, se [installera Azure CLI 2.0]( /cli/azure/install-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="73350-107">If you need tooinstall or upgrade, see [Install Azure CLI 2.0]( /cli/azure/install-azure-cli).</span></span> 

## <a name="sample-script"></a><span data-ttu-id="73350-108">Exempelskript</span><span class="sxs-lookup"><span data-stu-id="73350-108">Sample script</span></span>

[!code-azurecli-interactive[main](../../../cli_scripts/sql-database/move-database-between-pools/move-database-between-pools.sh "Move database between pools")]

## <a name="clean-up-deployment"></a><span data-ttu-id="73350-109">Rensa distribution</span><span class="sxs-lookup"><span data-stu-id="73350-109">Clean up deployment</span></span>

<span data-ttu-id="73350-110">Efter hello skriptexempel har körts, kan det vara hello följande kommando används tooremove hello resursgruppen och alla resurser som är kopplade till den.</span><span class="sxs-lookup"><span data-stu-id="73350-110">After hello script sample has been run, hello following command can be used tooremove hello resource group and all resources associated with it.</span></span>

```azurecli-interactive
az group delete --name myResourceGroup
```

## <a name="script-explanation"></a><span data-ttu-id="73350-111">Skriptet förklaring</span><span class="sxs-lookup"><span data-stu-id="73350-111">Script explanation</span></span>

<span data-ttu-id="73350-112">Det här skriptet använder hello följande kommandon.</span><span class="sxs-lookup"><span data-stu-id="73350-112">This script uses hello following commands.</span></span> <span data-ttu-id="73350-113">Varje kommando i hello tabellen länkar toocommand viss dokumentation.</span><span class="sxs-lookup"><span data-stu-id="73350-113">Each command in hello table links toocommand specific documentation.</span></span>

| <span data-ttu-id="73350-114">Kommando</span><span class="sxs-lookup"><span data-stu-id="73350-114">Command</span></span> | <span data-ttu-id="73350-115">Anteckningar</span><span class="sxs-lookup"><span data-stu-id="73350-115">Notes</span></span> |
|---|---|
| [<span data-ttu-id="73350-116">Skapa AZ grupp</span><span class="sxs-lookup"><span data-stu-id="73350-116">az group create</span></span>](https://docs.microsoft.com/cli/azure/group#create) | <span data-ttu-id="73350-117">Skapar en resursgrupp som är lagrade i alla resurser.</span><span class="sxs-lookup"><span data-stu-id="73350-117">Creates a resource group in which all resources are stored.</span></span> |
| [<span data-ttu-id="73350-118">Skapa AZ SQLServer</span><span class="sxs-lookup"><span data-stu-id="73350-118">az sql server create</span></span>](https://docs.microsoft.com/cli/azure/sql/server#create) | <span data-ttu-id="73350-119">Skapar en logisk server som är värd för en databas eller elastisk pool.</span><span class="sxs-lookup"><span data-stu-id="73350-119">Creates a logical server that hosts a database or elastic pool.</span></span> |
| [<span data-ttu-id="73350-120">Skapa AZ sql elastisk-pooler</span><span class="sxs-lookup"><span data-stu-id="73350-120">az sql elastic-pools create</span></span>](https://docs.microsoft.com/cli/azure/sql/elastic-pool#create) | <span data-ttu-id="73350-121">Skapar en elastisk pool i hello logiska server.</span><span class="sxs-lookup"><span data-stu-id="73350-121">Creates an elastic pool within hello logical server.</span></span> |
| [<span data-ttu-id="73350-122">Skapa AZ sql-databas</span><span class="sxs-lookup"><span data-stu-id="73350-122">az sql db create</span></span>](https://docs.microsoft.com/cli/azure/sql/db#create) | <span data-ttu-id="73350-123">Skapar en databas i en logisk server som en enda eller en delad databas.</span><span class="sxs-lookup"><span data-stu-id="73350-123">Creates a database in a logical server as a single or a pooled database.</span></span> |
| [<span data-ttu-id="73350-124">AZ sql db-uppdateringen</span><span class="sxs-lookup"><span data-stu-id="73350-124">az sql db update</span></span>](https://docs.microsoft.com/cli/azure/sql/db#update) | <span data-ttu-id="73350-125">Uppdaterar Databasegenskaper eller flyttar en databas i, slut på eller mellan elastiska pooler.</span><span class="sxs-lookup"><span data-stu-id="73350-125">Updates database properties or moves a database into, out of, or between elastic pools.</span></span> |
| [<span data-ttu-id="73350-126">ta bort grupp AZ</span><span class="sxs-lookup"><span data-stu-id="73350-126">az group delete</span></span>](https://docs.microsoft.com/cli/azure/vm/extension#set) | <span data-ttu-id="73350-127">Tar bort en resursgrupp, inklusive alla kapslade resurser.</span><span class="sxs-lookup"><span data-stu-id="73350-127">Deletes a resource group including all nested resources.</span></span> |

## <a name="next-steps"></a><span data-ttu-id="73350-128">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="73350-128">Next steps</span></span>

<span data-ttu-id="73350-129">Mer information om hello Azure CLI finns [Azure CLI dokumentationen](https://docs.microsoft.com/cli/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="73350-129">For more information on hello Azure CLI, see [Azure CLI documentation](https://docs.microsoft.com/cli/azure/overview).</span></span>

<span data-ttu-id="73350-130">Ytterligare SQL-databas CLI skriptexempel finns i hello [dokumentation för Azure SQL Database](../sql-database-cli-samples.md).</span><span class="sxs-lookup"><span data-stu-id="73350-130">Additional SQL Database CLI script samples can be found in hello [Azure SQL Database documentation](../sql-database-cli-samples.md).</span></span>


