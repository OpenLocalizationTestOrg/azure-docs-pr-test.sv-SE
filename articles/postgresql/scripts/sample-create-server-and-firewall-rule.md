---
title: "aaa ”Azure CLI Script - skapa en Azure-databas för PostgreSQL | Microsoft Docs ”"
description: "Azure CLI skriptexempel - skapar en Azure-databas för PostgreSQL-server och konfigurerar en brandväggsregel på servernivå."
services: postgresql
author: salonisonpal
ms.author: salonis
manager: jhubbard
editor: jasonwhowell
ms.service: postgresql
ms.custom: mvc
ms.devlang: azure-cli
ms.topic: sample
ms.date: 05/31/2017
ms.openlocfilehash: bbe31f77283aa4a3bb36d1fd9171c280594a8267
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-an-azure-database-for-postgresql-server-and-configure-a-firewall-rule-using-hello-azure-cli"></a><span data-ttu-id="1d405-103">Skapa en Azure-databas för PostgreSQL-servern och konfigurera en brandväggsregel som använder hello Azure CLI</span><span class="sxs-lookup"><span data-stu-id="1d405-103">Create an Azure Database for PostgreSQL server and configure a firewall rule using hello Azure CLI</span></span>
<span data-ttu-id="1d405-104">Det här exempelskriptet CLI skapar en Azure-databas för PostgreSQL-server och konfigurerar en brandväggsregel på servernivå.</span><span class="sxs-lookup"><span data-stu-id="1d405-104">This sample CLI script creates an Azure Database for PostgreSQL server and configures a server-level firewall rule.</span></span> <span data-ttu-id="1d405-105">När hello skriptet har körts utan problem, hello PostgreSQL-servern kan nås från alla Azure-tjänster och hello konfigurerade IP-adress.</span><span class="sxs-lookup"><span data-stu-id="1d405-105">Once hello script has been successfully run, hello PostgreSQL server can be accessed from all Azure services and hello configured IP address.</span></span>

[!INCLUDE [cloud-shell-try-it](../../../includes/cloud-shell-try-it.md)]

<span data-ttu-id="1d405-106">Om du väljer tooinstall och använda hello CLI lokalt kräver i det här avsnittet att du kör hello Azure CLI version 2.0 eller senare.</span><span class="sxs-lookup"><span data-stu-id="1d405-106">If you choose tooinstall and use hello CLI locally, this topic requires that you are running hello Azure CLI version 2.0 or later.</span></span> <span data-ttu-id="1d405-107">Kör `az --version` toofind hello version.</span><span class="sxs-lookup"><span data-stu-id="1d405-107">Run `az --version` toofind hello version.</span></span> <span data-ttu-id="1d405-108">Om du behöver tooinstall eller uppgradering, se [installera Azure CLI 2.0]( /cli/azure/install-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="1d405-108">If you need tooinstall or upgrade, see [Install Azure CLI 2.0]( /cli/azure/install-azure-cli).</span></span> 

## <a name="sample-script"></a><span data-ttu-id="1d405-109">Exempelskript</span><span class="sxs-lookup"><span data-stu-id="1d405-109">Sample script</span></span>
<span data-ttu-id="1d405-110">I det här exempelskriptet redigera hello markerade rader toocustomize hello admin användarnamn och lösenord.</span><span class="sxs-lookup"><span data-stu-id="1d405-110">In this sample script, edit hello highlighted lines toocustomize hello admin username and password.</span></span>
[!code-azurecli-interactive[main](../../../cli_scripts/postgresql/create-postgresql-server-and-firewall-rule/create-postgresql-server-and-firewall-rule.sh?highlight=15-16 "Create an Azure Database for PostgreSQL, and server-level firewall rule.")]

## <a name="clean-up-deployment"></a><span data-ttu-id="1d405-111">Rensa distribution</span><span class="sxs-lookup"><span data-stu-id="1d405-111">Clean up deployment</span></span>
<span data-ttu-id="1d405-112">Efter hello skriptexempel har körts, kan det vara hello följande kommando används tooremove hello resursgruppen och alla resurser som är kopplade till den.</span><span class="sxs-lookup"><span data-stu-id="1d405-112">After hello script sample has been run, hello following command can be used tooremove hello resource group and all resources associated with it.</span></span>
[!code-azurecli-interactive[main](../../../cli_scripts/postgresql/create-postgresql-server-and-firewall-rule/delete-postgresql.sh "Delete hello resource group.")]

## <a name="script-explanation"></a><span data-ttu-id="1d405-113">Skriptet förklaring</span><span class="sxs-lookup"><span data-stu-id="1d405-113">Script explanation</span></span>
<span data-ttu-id="1d405-114">Det här skriptet använder hello följande kommandon.</span><span class="sxs-lookup"><span data-stu-id="1d405-114">This script uses hello following commands.</span></span> <span data-ttu-id="1d405-115">Varje kommando i hello tabellen länkar toocommand viss dokumentation.</span><span class="sxs-lookup"><span data-stu-id="1d405-115">Each command in hello table links toocommand specific documentation.</span></span>

| <span data-ttu-id="1d405-116">**Kommandot**</span><span class="sxs-lookup"><span data-stu-id="1d405-116">**Command**</span></span> | <span data-ttu-id="1d405-117">**Anteckningar**</span><span class="sxs-lookup"><span data-stu-id="1d405-117">**Notes**</span></span> |
|---|---|
| [<span data-ttu-id="1d405-118">Skapa AZ grupp</span><span class="sxs-lookup"><span data-stu-id="1d405-118">az group create</span></span>](/cli/azure/group#create) | <span data-ttu-id="1d405-119">Skapar en resursgrupp som är lagrade i alla resurser.</span><span class="sxs-lookup"><span data-stu-id="1d405-119">Creates a resource group in which all resources are stored.</span></span> |
| [<span data-ttu-id="1d405-120">Skapa AZ postgres server</span><span class="sxs-lookup"><span data-stu-id="1d405-120">az postgres server create</span></span>](/cli/azure/postgres/server#create) | <span data-ttu-id="1d405-121">Skapar en PostgreSQL-server som är värd för hello databaser.</span><span class="sxs-lookup"><span data-stu-id="1d405-121">Creates a PostgreSQL server that hosts hello databases.</span></span> |
| [<span data-ttu-id="1d405-122">Skapa AZ postgres serverbrandvägg</span><span class="sxs-lookup"><span data-stu-id="1d405-122">az postgres server firewall create</span></span>](/cli/azure/postgres/server/firewall-rule#create) | <span data-ttu-id="1d405-123">Skapar en regel tooallow åtkomst toohello brandväggsserver och databaserna under den från hello angivna IP-adressintervall.</span><span class="sxs-lookup"><span data-stu-id="1d405-123">Creates a firewall rule tooallow access toohello server and databases under it from hello entered IP address range.</span></span> |
| [<span data-ttu-id="1d405-124">ta bort grupp AZ</span><span class="sxs-lookup"><span data-stu-id="1d405-124">az group delete</span></span>](/cli/azure/group#delete) | <span data-ttu-id="1d405-125">Tar bort en resursgrupp, inklusive alla kapslade resurser.</span><span class="sxs-lookup"><span data-stu-id="1d405-125">Deletes a resource group including all nested resources.</span></span> |

## <a name="next-steps"></a><span data-ttu-id="1d405-126">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="1d405-126">Next steps</span></span>
- <span data-ttu-id="1d405-127">Läs mer om hello Azure CLI: [Azure CLI-dokumentation](/cli/azure/overview)</span><span class="sxs-lookup"><span data-stu-id="1d405-127">Read more information on hello Azure CLI: [Azure CLI documentation](/cli/azure/overview)</span></span>
- <span data-ttu-id="1d405-128">Försök ytterligare skript: [Azure CLI-exempel för Azure-databas för PostgreSQL](../sample-scripts-azure-cli.md)</span><span class="sxs-lookup"><span data-stu-id="1d405-128">Try additional scripts: [Azure CLI samples for Azure Database for PostgreSQL](../sample-scripts-azure-cli.md)</span></span>
