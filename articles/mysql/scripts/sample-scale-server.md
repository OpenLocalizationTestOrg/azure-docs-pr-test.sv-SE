---
title: "aaaAzure CLI exempel tooscale en Azure-databas för MySQL-server | Microsoft Docs"
description: "Det här exempelskriptet CLI skalas Azure-databas för MySQL servernivå tooa olika prestanda när du frågar hello mått."
services: mysql
author: v-chenyh
ms.author: v-chenyh
manager: jhubbard
editor: jasonwhowell
ms.service: mysql-database
ms.devlang: azure-cli
ms.topic: sample
ms.custom: mvc
ms.date: 05/31/2017
ms.openlocfilehash: 721ef9db35a5f3be7a38438c1abb724187b18b75
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="monitor-and-scale-an-azure-database-for-mysql-server-using-azure-cli"></a><span data-ttu-id="aae9c-103">Övervaka och skala en Azure-databas för MySQL-server med Azure CLI</span><span class="sxs-lookup"><span data-stu-id="aae9c-103">Monitor and scale an Azure Database for MySQL server using Azure CLI</span></span>
<span data-ttu-id="aae9c-104">Det här exempelskriptet CLI skalar en Azure-databas för MySQL servernivå tooa olika prestanda när du frågar hello mått.</span><span class="sxs-lookup"><span data-stu-id="aae9c-104">This sample CLI script scales a single Azure Database for MySQL server tooa different performance level after querying hello metrics.</span></span>

[!INCLUDE [cloud-shell-try-it](../../../includes/cloud-shell-try-it.md)]

<span data-ttu-id="aae9c-105">Om du väljer tooinstall och använda hello CLI lokalt kräver i det här avsnittet att du kör hello Azure CLI version 2.0 eller senare.</span><span class="sxs-lookup"><span data-stu-id="aae9c-105">If you choose tooinstall and use hello CLI locally, this topic requires that you are running hello Azure CLI version 2.0 or later.</span></span> <span data-ttu-id="aae9c-106">Kör `az --version` toofind hello version.</span><span class="sxs-lookup"><span data-stu-id="aae9c-106">Run `az --version` toofind hello version.</span></span> <span data-ttu-id="aae9c-107">Om du behöver tooinstall eller uppgradering, se [installera Azure CLI 2.0]( /cli/azure/install-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="aae9c-107">If you need tooinstall or upgrade, see [Install Azure CLI 2.0]( /cli/azure/install-azure-cli).</span></span> 

## <a name="sample-script"></a><span data-ttu-id="aae9c-108">Exempelskript</span><span class="sxs-lookup"><span data-stu-id="aae9c-108">Sample script</span></span>
<span data-ttu-id="aae9c-109">I det här exempelskriptet ändra hello markerade rader toocustomize hello admin användarnamn och lösenord.</span><span class="sxs-lookup"><span data-stu-id="aae9c-109">In this sample script, change hello highlighted lines toocustomize hello admin username and password.</span></span> <span data-ttu-id="aae9c-110">Ersätt hello prenumerations-id som används i hello az övervakaren kommandon med din egen prenumerations-id.[!code-azurecli-interactive[main](../../../cli_scripts/mysql/scale-mysql-server/scale-mysql-server.sh?highlight=15-16 "Create and scale Azure Database for MySQL.")]</span><span class="sxs-lookup"><span data-stu-id="aae9c-110">Replace hello subscription id used in hello az monitor commands with your own subscription id. [!code-azurecli-interactive[main](../../../cli_scripts/mysql/scale-mysql-server/scale-mysql-server.sh?highlight=15-16 "Create and scale Azure Database for MySQL.")]</span></span>

## <a name="clean-up-deployment"></a><span data-ttu-id="aae9c-111">Rensa distribution</span><span class="sxs-lookup"><span data-stu-id="aae9c-111">Clean up deployment</span></span>
<span data-ttu-id="aae9c-112">Efter hello skriptexempel har körts, kan det vara hello följande kommando används tooremove hello resursgruppen och alla resurser som är kopplade till den.</span><span class="sxs-lookup"><span data-stu-id="aae9c-112">After hello script sample has been run, hello following command can be used tooremove hello resource group and all resources associated with it.</span></span>
[!code-azurecli-interactive[main](../../../cli_scripts/mysql/scale-mysql-server/delete-mysql.sh  "Delete hello resource group.")]

## <a name="script-explanation"></a><span data-ttu-id="aae9c-113">Skriptet förklaring</span><span class="sxs-lookup"><span data-stu-id="aae9c-113">Script explanation</span></span>
<span data-ttu-id="aae9c-114">Det här skriptet använder hello följande kommandon.</span><span class="sxs-lookup"><span data-stu-id="aae9c-114">This script uses hello following commands.</span></span> <span data-ttu-id="aae9c-115">Varje kommando i hello tabellen länkar toocommand viss dokumentation.</span><span class="sxs-lookup"><span data-stu-id="aae9c-115">Each command in hello table links toocommand specific documentation.</span></span>

| <span data-ttu-id="aae9c-116">**Kommandot**</span><span class="sxs-lookup"><span data-stu-id="aae9c-116">**Command**</span></span> | <span data-ttu-id="aae9c-117">**Anteckningar**</span><span class="sxs-lookup"><span data-stu-id="aae9c-117">**Notes**</span></span> |
|---|---|
| [<span data-ttu-id="aae9c-118">Skapa AZ grupp</span><span class="sxs-lookup"><span data-stu-id="aae9c-118">az group create</span></span>](/cli/azure/group#create) | <span data-ttu-id="aae9c-119">Skapar en resursgrupp som är lagrade i alla resurser.</span><span class="sxs-lookup"><span data-stu-id="aae9c-119">Creates a resource group in which all resources are stored.</span></span> |
| [<span data-ttu-id="aae9c-120">Skapa AZ mysql-server</span><span class="sxs-lookup"><span data-stu-id="aae9c-120">az mysql server create</span></span>](/cli/azure/mysql/server#create) | <span data-ttu-id="aae9c-121">Skapar en MySQL-server som är värd för hello databaser.</span><span class="sxs-lookup"><span data-stu-id="aae9c-121">Creates a MySQL server that hosts hello databases.</span></span> |
| [<span data-ttu-id="aae9c-122">AZ övervakaren mått lista</span><span class="sxs-lookup"><span data-stu-id="aae9c-122">az monitor metrics list</span></span>](/cli/azure/monitor/metrics#list) | <span data-ttu-id="aae9c-123">Lista över hello måttet för hello resurser.</span><span class="sxs-lookup"><span data-stu-id="aae9c-123">List hello metric value for hello resources.</span></span> |
| [<span data-ttu-id="aae9c-124">ta bort grupp AZ</span><span class="sxs-lookup"><span data-stu-id="aae9c-124">az group delete</span></span>](/cli/azure/group#delete) | <span data-ttu-id="aae9c-125">Tar bort en resursgrupp, inklusive alla kapslade resurser.</span><span class="sxs-lookup"><span data-stu-id="aae9c-125">Deletes a resource group including all nested resources.</span></span> |

## <a name="next-steps"></a><span data-ttu-id="aae9c-126">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="aae9c-126">Next steps</span></span>
- <span data-ttu-id="aae9c-127">Läs mer om hello Azure CLI: [Azure CLI dokumentationen](/cli/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="aae9c-127">Read more information on hello Azure CLI: [Azure CLI documentation](/cli/azure/overview).</span></span>
- <span data-ttu-id="aae9c-128">Försök ytterligare skript: [Azure CLI-exempel för Azure-databas för MySQL](../sample-scripts-azure-cli.md)</span><span class="sxs-lookup"><span data-stu-id="aae9c-128">Try additional scripts: [Azure CLI samples for Azure Database for MySQL](../sample-scripts-azure-cli.md)</span></span>
- <span data-ttu-id="aae9c-129">Läs mer om att skala [tjänstnivåer](../concepts-service-tiers.md) och [Compute enheter och lagringsenheter](../concepts-compute-unit-and-storage.md).</span><span class="sxs-lookup"><span data-stu-id="aae9c-129">For more information on scaling, see [Service Tiers](../concepts-service-tiers.md) and [Compute Units and Storage Units](../concepts-compute-unit-and-storage.md).</span></span>
