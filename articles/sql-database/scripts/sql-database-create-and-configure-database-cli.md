---
title: aaaCLI exempel-skapa en Azure SQL database | Microsoft Docs
description: Azure CLI exempel skriptet toocreate en SQL-databas
services: sql-database
documentationcenter: sql-database
author: janeng
manager: jstrauss
editor: carlrab
tags: azure-service-management
ms.assetid: 
ms.service: sql-database
ms.custom: DBs & servers
ms.devlang: azurecli
ms.topic: sample
ms.tgt_pltfrm: sql-database
ms.workload: database
ms.date: 06/23/2017
ms.author: janeng
ms.openlocfilehash: 0d54e284e19f16387813e24d7beb7ab048a39263
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="use-cli-toocreate-a-single-azure-sql-database-and-configure-a-firewall-rule"></a><span data-ttu-id="4e54f-103">Använd CLI toocreate en Azure SQL-databas och konfigurera en brandväggsregel</span><span class="sxs-lookup"><span data-stu-id="4e54f-103">Use CLI toocreate a single Azure SQL database and configure a firewall rule</span></span>

<span data-ttu-id="4e54f-104">Detta exempel på Azure CLI-skript skapar en Azure SQL database och konfigurera en brandväggsregel på servernivå.</span><span class="sxs-lookup"><span data-stu-id="4e54f-104">This Azure CLI script example creates an Azure SQL database and configure a server-level firewall rule.</span></span> <span data-ttu-id="4e54f-105">När hello skriptet har körts utan problem, konfigurerade hello SQL-databas kan nås från alla Azure-tjänster och hello IP-adress.</span><span class="sxs-lookup"><span data-stu-id="4e54f-105">Once hello script has been successfully run, hello SQL Database can be accessed from all Azure services and hello configured IP address.</span></span> 

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

<span data-ttu-id="4e54f-106">Om du väljer tooinstall och använda hello CLI lokalt kräver i det här avsnittet att du kör hello Azure CLI version 2.0 eller senare.</span><span class="sxs-lookup"><span data-stu-id="4e54f-106">If you choose tooinstall and use hello CLI locally, this topic requires that you are running hello Azure CLI version 2.0 or later.</span></span> <span data-ttu-id="4e54f-107">Kör `az --version` toofind hello version.</span><span class="sxs-lookup"><span data-stu-id="4e54f-107">Run `az --version` toofind hello version.</span></span> <span data-ttu-id="4e54f-108">Om du behöver tooinstall eller uppgradering, se [installera Azure CLI 2.0]( /cli/azure/install-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="4e54f-108">If you need tooinstall or upgrade, see [Install Azure CLI 2.0]( /cli/azure/install-azure-cli).</span></span> 

## <a name="sample-script"></a><span data-ttu-id="4e54f-109">Exempelskript</span><span class="sxs-lookup"><span data-stu-id="4e54f-109">Sample script</span></span>

[!code-azurecli-interactive[main](../../../cli_scripts/sql-database/create-and-configure-database/create-and-configure-database.sh?highlight=9-10 "Create SQL Database")]

## <a name="clean-up-deployment"></a><span data-ttu-id="4e54f-110">Rensa distribution</span><span class="sxs-lookup"><span data-stu-id="4e54f-110">Clean up deployment</span></span>

<span data-ttu-id="4e54f-111">Efter hello skriptexempel har körts, kan det vara hello följande kommando används tooremove hello resursgruppen och alla resurser som är kopplade till den.</span><span class="sxs-lookup"><span data-stu-id="4e54f-111">After hello script sample has been run, hello following command can be used tooremove hello resource group and all resources associated with it.</span></span>

```azurecli-interactive
az group delete --name myResourceGroup
```

## <a name="script-explanation"></a><span data-ttu-id="4e54f-112">Skriptet förklaring</span><span class="sxs-lookup"><span data-stu-id="4e54f-112">Script explanation</span></span>

<span data-ttu-id="4e54f-113">Det här skriptet använder hello följande kommandon.</span><span class="sxs-lookup"><span data-stu-id="4e54f-113">This script uses hello following commands.</span></span> <span data-ttu-id="4e54f-114">Varje kommando i hello tabellen länkar toocommand viss dokumentation.</span><span class="sxs-lookup"><span data-stu-id="4e54f-114">Each command in hello table links toocommand specific documentation.</span></span>

| <span data-ttu-id="4e54f-115">Kommando</span><span class="sxs-lookup"><span data-stu-id="4e54f-115">Command</span></span> | <span data-ttu-id="4e54f-116">Anteckningar</span><span class="sxs-lookup"><span data-stu-id="4e54f-116">Notes</span></span> |
|---|---|
| [<span data-ttu-id="4e54f-117">Skapa AZ grupp</span><span class="sxs-lookup"><span data-stu-id="4e54f-117">az group create</span></span>](/cli/azure/group#create) | <span data-ttu-id="4e54f-118">Skapar en resursgrupp som är lagrade i alla resurser.</span><span class="sxs-lookup"><span data-stu-id="4e54f-118">Creates a resource group in which all resources are stored.</span></span> |
| [<span data-ttu-id="4e54f-119">Skapa AZ SQLServer</span><span class="sxs-lookup"><span data-stu-id="4e54f-119">az sql server create</span></span>](/cli/azure/sql/server#create) | <span data-ttu-id="4e54f-120">Skapar en logisk server att värdar hello SQL-databas.</span><span class="sxs-lookup"><span data-stu-id="4e54f-120">Creates a logical server that hosts hello SQL Database.</span></span> |
| [<span data-ttu-id="4e54f-121">Skapa AZ sql server-brandvägg</span><span class="sxs-lookup"><span data-stu-id="4e54f-121">az sql server firewall create</span></span>](/cli/azure/sql/server/firewall-rule#create) | <span data-ttu-id="4e54f-122">Skapar en brandvägg regeln tooallow åtkomst tooall SQL-databaser på hello-servern från hello angivna IP-adressintervall.</span><span class="sxs-lookup"><span data-stu-id="4e54f-122">Creates a firewall rule tooallow access tooall SQL Databases on hello server from hello entered IP address range.</span></span> |
| [<span data-ttu-id="4e54f-123">Skapa AZ sql-databas</span><span class="sxs-lookup"><span data-stu-id="4e54f-123">az sql db create</span></span>](/cli/azure/sql/db#create) | <span data-ttu-id="4e54f-124">Skapar hello SQL-databas i hello logiska server.</span><span class="sxs-lookup"><span data-stu-id="4e54f-124">Creates hello SQL Database in hello logical server.</span></span> |
| [<span data-ttu-id="4e54f-125">ta bort grupp AZ</span><span class="sxs-lookup"><span data-stu-id="4e54f-125">az group delete</span></span>](/cli/azure/resource#delete) | <span data-ttu-id="4e54f-126">Tar bort en resursgrupp, inklusive alla kapslade resurser.</span><span class="sxs-lookup"><span data-stu-id="4e54f-126">Deletes a resource group including all nested resources.</span></span> |

## <a name="next-steps"></a><span data-ttu-id="4e54f-127">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="4e54f-127">Next steps</span></span>

<span data-ttu-id="4e54f-128">Mer information om hello Azure CLI finns [Azure CLI dokumentationen](https://docs.microsoft.com/cli/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="4e54f-128">For more information on hello Azure CLI, see [Azure CLI documentation](https://docs.microsoft.com/cli/azure/overview).</span></span>

<span data-ttu-id="4e54f-129">Ytterligare SQL-databas CLI skriptexempel finns i hello [dokumentation för Azure SQL Database](../sql-database-cli-samples.md).</span><span class="sxs-lookup"><span data-stu-id="4e54f-129">Additional SQL Database CLI script samples can be found in hello [Azure SQL Database documentation](../sql-database-cli-samples.md).</span></span>

