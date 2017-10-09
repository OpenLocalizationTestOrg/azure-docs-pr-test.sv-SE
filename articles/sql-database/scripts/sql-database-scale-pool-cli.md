---
title: aaaCLI exempel skalar en SQL elastisk pool Azure SQL Database | Microsoft Docs
description: Azure CLI exempel skriptet tooscale en elastisk SQL-pool i Azure SQL Database
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
ms.openlocfilehash: 436128b8183213f78b9abc2ec46efe2a3ed3c37c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="use-cli-tooscale-a-sql-elastic-pool-in-azure-sql-database"></a><span data-ttu-id="ceaef-103">Använda CLI tooscale en elastisk SQL-pool i Azure SQL Database</span><span class="sxs-lookup"><span data-stu-id="ceaef-103">Use CLI tooscale a SQL elastic pool in Azure SQL Database</span></span>

<span data-ttu-id="ceaef-104">Detta exempel på Azure CLI-skript skapar SQL elastiska pooler, flyttar grupperade databaser och ändrar elastisk pool prestandanivåer.</span><span class="sxs-lookup"><span data-stu-id="ceaef-104">This Azure CLI script example creates SQL elastic pools, moves pooled databases, and changes elastic pool performance levels.</span></span> 

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

<span data-ttu-id="ceaef-105">Om du väljer tooinstall och använda hello CLI lokalt kräver i det här avsnittet att du kör hello Azure CLI version 2.0 eller senare.</span><span class="sxs-lookup"><span data-stu-id="ceaef-105">If you choose tooinstall and use hello CLI locally, this topic requires that you are running hello Azure CLI version 2.0 or later.</span></span> <span data-ttu-id="ceaef-106">Kör `az --version` toofind hello version.</span><span class="sxs-lookup"><span data-stu-id="ceaef-106">Run `az --version` toofind hello version.</span></span> <span data-ttu-id="ceaef-107">Om du behöver tooinstall eller uppgradering, se [installera Azure CLI 2.0]( /cli/azure/install-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="ceaef-107">If you need tooinstall or upgrade, see [Install Azure CLI 2.0]( /cli/azure/install-azure-cli).</span></span> 

## <a name="sample-script"></a><span data-ttu-id="ceaef-108">Exempelskript</span><span class="sxs-lookup"><span data-stu-id="ceaef-108">Sample script</span></span>

[!code-azurecli-interactive[main](../../../cli_scripts/sql-database/scale-pool/scale-pool.sh "Move database between pools")]

## <a name="clean-up-deployment"></a><span data-ttu-id="ceaef-109">Rensa distribution</span><span class="sxs-lookup"><span data-stu-id="ceaef-109">Clean up deployment</span></span>

<span data-ttu-id="ceaef-110">Efter hello skriptexempel har körts, kan det vara hello följande kommando används tooremove hello resursgruppen och alla resurser som är kopplade till den.</span><span class="sxs-lookup"><span data-stu-id="ceaef-110">After hello script sample has been run, hello following command can be used tooremove hello resource group and all resources associated with it.</span></span>

```azurecli-interactive
az group delete --name myResourceGroup
```

## <a name="script-explanation"></a><span data-ttu-id="ceaef-111">Skriptet förklaring</span><span class="sxs-lookup"><span data-stu-id="ceaef-111">Script explanation</span></span>

<span data-ttu-id="ceaef-112">Det här skriptet använder hello följande kommandon toocreate en resursgrupp, logisk server, SQL-databas och brandväggsregler.</span><span class="sxs-lookup"><span data-stu-id="ceaef-112">This script uses hello following commands toocreate a resource group, logical server, SQL Database, and firewall rules.</span></span> <span data-ttu-id="ceaef-113">Varje kommando i hello tabellen länkar toocommand viss dokumentation.</span><span class="sxs-lookup"><span data-stu-id="ceaef-113">Each command in hello table links toocommand specific documentation.</span></span>

| <span data-ttu-id="ceaef-114">Kommando</span><span class="sxs-lookup"><span data-stu-id="ceaef-114">Command</span></span> | <span data-ttu-id="ceaef-115">Anteckningar</span><span class="sxs-lookup"><span data-stu-id="ceaef-115">Notes</span></span> |
|---|---|
| [<span data-ttu-id="ceaef-116">Skapa AZ grupp</span><span class="sxs-lookup"><span data-stu-id="ceaef-116">az group create</span></span>](https://docs.microsoft.com/cli/azure/group#create) | <span data-ttu-id="ceaef-117">Skapar en resursgrupp som är lagrade i alla resurser.</span><span class="sxs-lookup"><span data-stu-id="ceaef-117">Creates a resource group in which all resources are stored.</span></span> |
| [<span data-ttu-id="ceaef-118">Skapa AZ SQLServer</span><span class="sxs-lookup"><span data-stu-id="ceaef-118">az sql server create</span></span>](https://docs.microsoft.com/cli/azure/sql/server#create) | <span data-ttu-id="ceaef-119">Skapar en logisk server att värdar hello SQL-databas.</span><span class="sxs-lookup"><span data-stu-id="ceaef-119">Creates a logical server that hosts hello SQL Database.</span></span> |
| [<span data-ttu-id="ceaef-120">Skapa AZ sql elastisk-pooler</span><span class="sxs-lookup"><span data-stu-id="ceaef-120">az sql elastic-pools create</span></span>](https://docs.microsoft.com/cli/azure/sql/elastic-pool#create) | <span data-ttu-id="ceaef-121">Skapar en elastisk databaspool inom hello logiska server.</span><span class="sxs-lookup"><span data-stu-id="ceaef-121">Creates an elastic database pool within hello logical server.</span></span> |
| [<span data-ttu-id="ceaef-122">Skapa AZ sql-databas</span><span class="sxs-lookup"><span data-stu-id="ceaef-122">az sql db create</span></span>](https://docs.microsoft.com/cli/azure/sql/db#create) | <span data-ttu-id="ceaef-123">Skapar hello SQL-databas i hello logiska server.</span><span class="sxs-lookup"><span data-stu-id="ceaef-123">Creates hello SQL Database in hello logical server.</span></span> |
| [<span data-ttu-id="ceaef-124">AZ sql elastiska pooler uppdateringen</span><span class="sxs-lookup"><span data-stu-id="ceaef-124">az sql elastic-pools update</span></span>](https://docs.microsoft.com/cli/azure/sql/elastic-pool#update) | <span data-ttu-id="ceaef-125">Uppdaterar en elastisk databaspool i det här exemplet ändringar hello tilldelade eDTU.</span><span class="sxs-lookup"><span data-stu-id="ceaef-125">Updates an elastic database pool, in this example changes hello assigned eDTU.</span></span> |
| [<span data-ttu-id="ceaef-126">ta bort grupp AZ</span><span class="sxs-lookup"><span data-stu-id="ceaef-126">az group delete</span></span>](https://docs.microsoft.com/cli/azure/vm/extension#set) | <span data-ttu-id="ceaef-127">Tar bort en resursgrupp, inklusive alla kapslade resurser.</span><span class="sxs-lookup"><span data-stu-id="ceaef-127">Deletes a resource group including all nested resources.</span></span> |

## <a name="next-steps"></a><span data-ttu-id="ceaef-128">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="ceaef-128">Next steps</span></span>

<span data-ttu-id="ceaef-129">Mer information om hello Azure CLI finns [Azure CLI dokumentationen](https://docs.microsoft.com/cli/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="ceaef-129">For more information on hello Azure CLI, see [Azure CLI documentation](https://docs.microsoft.com/cli/azure/overview).</span></span>

<span data-ttu-id="ceaef-130">Ytterligare SQL-databas CLI skriptexempel finns i hello [dokumentation för Azure SQL Database](../sql-database-cli-samples.md).</span><span class="sxs-lookup"><span data-stu-id="ceaef-130">Additional SQL Database CLI script samples can be found in hello [Azure SQL Database documentation](../sql-database-cli-samples.md).</span></span>
