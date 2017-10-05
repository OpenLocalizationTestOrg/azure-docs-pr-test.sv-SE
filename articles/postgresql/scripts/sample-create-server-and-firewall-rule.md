---
title: "Azure CLI Script - skapa en Azure-databas för PostgreSQL | Microsoft Docs"
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
ms.openlocfilehash: e545b568cd57fdcf28ab33a5ebfa34a495111c7f
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/29/2017
---
# <a name="create-an-azure-database-for-postgresql-server-and-configure-a-firewall-rule-using-the-azure-cli"></a><span data-ttu-id="a6228-103">Skapa en Azure-databas för PostgreSQL-servern och konfigurera en brandväggsregel med hjälp av Azure CLI</span><span class="sxs-lookup"><span data-stu-id="a6228-103">Create an Azure Database for PostgreSQL server and configure a firewall rule using the Azure CLI</span></span>
<span data-ttu-id="a6228-104">Det här exempelskriptet CLI skapar en Azure-databas för PostgreSQL-server och konfigurerar en brandväggsregel på servernivå.</span><span class="sxs-lookup"><span data-stu-id="a6228-104">This sample CLI script creates an Azure Database for PostgreSQL server and configures a server-level firewall rule.</span></span> <span data-ttu-id="a6228-105">När skriptet har körts utan problem, kan PostgreSQL-servern nås från alla Azure-tjänster och den konfigurerade IP-adressen.</span><span class="sxs-lookup"><span data-stu-id="a6228-105">Once the script has been successfully run, the PostgreSQL server can be accessed from all Azure services and the configured IP address.</span></span>

[!INCLUDE [cloud-shell-try-it](../../../includes/cloud-shell-try-it.md)]

<span data-ttu-id="a6228-106">Om du väljer att installera och använda CLI lokalt måste du köra Azure CLI version 2.0 eller senare.</span><span class="sxs-lookup"><span data-stu-id="a6228-106">If you choose to install and use the CLI locally, this topic requires that you are running the Azure CLI version 2.0 or later.</span></span> <span data-ttu-id="a6228-107">Kör `az --version` för att hitta versionen.</span><span class="sxs-lookup"><span data-stu-id="a6228-107">Run `az --version` to find the version.</span></span> <span data-ttu-id="a6228-108">Om du behöver installera eller uppgradera kan du läsa [Installera Azure CLI 2.0]( /cli/azure/install-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="a6228-108">If you need to install or upgrade, see [Install Azure CLI 2.0]( /cli/azure/install-azure-cli).</span></span> 

## <a name="sample-script"></a><span data-ttu-id="a6228-109">Exempelskript</span><span class="sxs-lookup"><span data-stu-id="a6228-109">Sample script</span></span>
<span data-ttu-id="a6228-110">I det här exempelskriptet Redigera markerade rader om du vill anpassa admin användarnamn och lösenord.</span><span class="sxs-lookup"><span data-stu-id="a6228-110">In this sample script, edit the highlighted lines to customize the admin username and password.</span></span>
<span data-ttu-id="a6228-111">[!code-azurecli-interactive[huvudsakliga](../../../cli_scripts/postgresql/create-postgresql-server-and-firewall-rule/create-postgresql-server-and-firewall-rule.sh?highlight=15-16 "skapa en Azure-databas för PostgreSQL och brandväggsregel på servernivå.")]</span><span class="sxs-lookup"><span data-stu-id="a6228-111">[!code-azurecli-interactive[main](../../../cli_scripts/postgresql/create-postgresql-server-and-firewall-rule/create-postgresql-server-and-firewall-rule.sh?highlight=15-16 "Create an Azure Database for PostgreSQL, and server-level firewall rule.")]</span></span>

## <a name="clean-up-deployment"></a><span data-ttu-id="a6228-112">Rensa distribution</span><span class="sxs-lookup"><span data-stu-id="a6228-112">Clean up deployment</span></span>
<span data-ttu-id="a6228-113">Följande kommando kan användas för att ta bort resursgruppen och alla resurser som är associerade med den efter skriptexempel har körts.</span><span class="sxs-lookup"><span data-stu-id="a6228-113">After the script sample has been run, the following command can be used to remove the resource group and all resources associated with it.</span></span>
<span data-ttu-id="a6228-114">[!code-azurecli-interactive[huvudsakliga](../../../cli_scripts/postgresql/create-postgresql-server-and-firewall-rule/delete-postgresql.sh "ta bort resursgruppen.")]</span><span class="sxs-lookup"><span data-stu-id="a6228-114">[!code-azurecli-interactive[main](../../../cli_scripts/postgresql/create-postgresql-server-and-firewall-rule/delete-postgresql.sh "Delete the resource group.")]</span></span>

## <a name="script-explanation"></a><span data-ttu-id="a6228-115">Skriptet förklaring</span><span class="sxs-lookup"><span data-stu-id="a6228-115">Script explanation</span></span>
<span data-ttu-id="a6228-116">Det här skriptet använder följande kommandon.</span><span class="sxs-lookup"><span data-stu-id="a6228-116">This script uses the following commands.</span></span> <span data-ttu-id="a6228-117">Varje kommando i tabellen länkar till kommandot viss dokumentation.</span><span class="sxs-lookup"><span data-stu-id="a6228-117">Each command in the table links to command specific documentation.</span></span>

| <span data-ttu-id="a6228-118">**Kommandot**</span><span class="sxs-lookup"><span data-stu-id="a6228-118">**Command**</span></span> | <span data-ttu-id="a6228-119">**Anteckningar**</span><span class="sxs-lookup"><span data-stu-id="a6228-119">**Notes**</span></span> |
|---|---|
| [<span data-ttu-id="a6228-120">Skapa AZ grupp</span><span class="sxs-lookup"><span data-stu-id="a6228-120">az group create</span></span>](/cli/azure/group#create) | <span data-ttu-id="a6228-121">Skapar en resursgrupp som är lagrade i alla resurser.</span><span class="sxs-lookup"><span data-stu-id="a6228-121">Creates a resource group in which all resources are stored.</span></span> |
| [<span data-ttu-id="a6228-122">Skapa AZ postgres server</span><span class="sxs-lookup"><span data-stu-id="a6228-122">az postgres server create</span></span>](/cli/azure/postgres/server#create) | <span data-ttu-id="a6228-123">Skapar en PostgreSQL-server som är värd för databaserna.</span><span class="sxs-lookup"><span data-stu-id="a6228-123">Creates a PostgreSQL server that hosts the databases.</span></span> |
| [<span data-ttu-id="a6228-124">Skapa AZ postgres serverbrandvägg</span><span class="sxs-lookup"><span data-stu-id="a6228-124">az postgres server firewall create</span></span>](/cli/azure/postgres/server/firewall-rule#create) | <span data-ttu-id="a6228-125">Skapar en brandväggsregel för att tillåta åtkomst till servern och databaserna under den från det angivna IP-adressintervallet.</span><span class="sxs-lookup"><span data-stu-id="a6228-125">Creates a firewall rule to allow access to the server and databases under it from the entered IP address range.</span></span> |
| [<span data-ttu-id="a6228-126">ta bort grupp AZ</span><span class="sxs-lookup"><span data-stu-id="a6228-126">az group delete</span></span>](/cli/azure/group#delete) | <span data-ttu-id="a6228-127">Tar bort en resursgrupp, inklusive alla kapslade resurser.</span><span class="sxs-lookup"><span data-stu-id="a6228-127">Deletes a resource group including all nested resources.</span></span> |

## <a name="next-steps"></a><span data-ttu-id="a6228-128">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="a6228-128">Next steps</span></span>
- <span data-ttu-id="a6228-129">Läs mer om Azure CLI: [Azure CLI-dokumentation](/cli/azure/overview)</span><span class="sxs-lookup"><span data-stu-id="a6228-129">Read more information on the Azure CLI: [Azure CLI documentation](/cli/azure/overview)</span></span>
- <span data-ttu-id="a6228-130">Försök ytterligare skript: [Azure CLI-exempel för Azure-databas för PostgreSQL](../sample-scripts-azure-cli.md)</span><span class="sxs-lookup"><span data-stu-id="a6228-130">Try additional scripts: [Azure CLI samples for Azure Database for PostgreSQL](../sample-scripts-azure-cli.md)</span></span>
