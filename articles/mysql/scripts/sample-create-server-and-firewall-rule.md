---
title: "Azure CLI Script - skapa en Azure-databas för MySQL | Microsoft Docs"
description: "Det här exempelskriptet CLI skapar en Azure-databas för MySQL-server och konfigurerar en brandväggsregel på servernivå."
services: mysql
author: v-chenyh
ms.author: v-chenyh
manager: jhubbard
editor: jasonwhowell
ms.service: mysql-database
ms.devlang: azure-cli
ms.custom: mvc
ms.topic: sample
ms.date: 05/31/2017
ms.openlocfilehash: 201db294ce362ef3e09cbe62f48bd51c8ea94dbb
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="create-a-mysql-server-and-configure-a-firewall-rule-using-the-azure-cli"></a><span data-ttu-id="e37e0-103">Skapa en MySQL-server och konfigurera en brandväggsregel med hjälp av Azure CLI</span><span class="sxs-lookup"><span data-stu-id="e37e0-103">Create a MySQL server and configure a firewall rule using the Azure CLI</span></span>
<span data-ttu-id="e37e0-104">Det här exempelskriptet CLI skapar en Azure-databas för MySQL-server och konfigurerar en brandväggsregel på servernivå.</span><span class="sxs-lookup"><span data-stu-id="e37e0-104">This sample CLI script creates an Azure Database for MySQL server and configures a server-level firewall rule.</span></span> <span data-ttu-id="e37e0-105">När skriptet har körts, är MySQL-servern tillgänglig för alla Azure-tjänster och den konfigurerade IP-adressen.</span><span class="sxs-lookup"><span data-stu-id="e37e0-105">Once the script runs successfully, the MySQL server is accessible by all Azure services and the configured IP address.</span></span>

[!INCLUDE [cloud-shell-try-it](../../../includes/cloud-shell-try-it.md)]

<span data-ttu-id="e37e0-106">Om du väljer att installera och använda CLI lokalt måste du köra Azure CLI version 2.0 eller senare.</span><span class="sxs-lookup"><span data-stu-id="e37e0-106">If you choose to install and use the CLI locally, this topic requires that you are running the Azure CLI version 2.0 or later.</span></span> <span data-ttu-id="e37e0-107">Kör `az --version` för att hitta versionen.</span><span class="sxs-lookup"><span data-stu-id="e37e0-107">Run `az --version` to find the version.</span></span> <span data-ttu-id="e37e0-108">Om du behöver installera eller uppgradera kan du läsa [Installera Azure CLI 2.0]( /cli/azure/install-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="e37e0-108">If you need to install or upgrade, see [Install Azure CLI 2.0]( /cli/azure/install-azure-cli).</span></span> 

## <a name="sample-script"></a><span data-ttu-id="e37e0-109">Exempelskript</span><span class="sxs-lookup"><span data-stu-id="e37e0-109">Sample script</span></span>
<span data-ttu-id="e37e0-110">I det här exempelskriptet Redigera markerade rader om du vill anpassa admin användarnamn och lösenord.</span><span class="sxs-lookup"><span data-stu-id="e37e0-110">In this sample script, edit the highlighted lines to customize the admin username and password.</span></span>
<span data-ttu-id="e37e0-111">[!code-azurecli-interactive[huvudsakliga](../../../cli_scripts/mysql/create-mysql-server-and-firewall-rule/create-mysql-server-and-firewall-rule.sh?highlight=15-16 "skapa en Azure-databas för MySQL och brandväggsregel på servernivå.")]</span><span class="sxs-lookup"><span data-stu-id="e37e0-111">[!code-azurecli-interactive[main](../../../cli_scripts/mysql/create-mysql-server-and-firewall-rule/create-mysql-server-and-firewall-rule.sh?highlight=15-16 "Create an Azure Database for MySQL, and server-level firewall rule.")]</span></span>

## <a name="clean-up-deployment"></a><span data-ttu-id="e37e0-112">Rensa distribution</span><span class="sxs-lookup"><span data-stu-id="e37e0-112">Clean up deployment</span></span>
<span data-ttu-id="e37e0-113">Följande kommando kan användas för att ta bort resursgruppen och alla resurser som är associerade med den efter skriptexempel har körts.</span><span class="sxs-lookup"><span data-stu-id="e37e0-113">After the script sample has been run, the following command can be used to remove the resource group and all resources associated with it.</span></span>
<span data-ttu-id="e37e0-114">[!code-azurecli-interactive[huvudsakliga](../../../cli_scripts/mysql/create-mysql-server-and-firewall-rule/delete-mysql.sh "ta bort resursgruppen.")]</span><span class="sxs-lookup"><span data-stu-id="e37e0-114">[!code-azurecli-interactive[main](../../../cli_scripts/mysql/create-mysql-server-and-firewall-rule/delete-mysql.sh "Delete the resource group.")]</span></span>

## <a name="script-explanation"></a><span data-ttu-id="e37e0-115">Skriptet förklaring</span><span class="sxs-lookup"><span data-stu-id="e37e0-115">Script explanation</span></span>
<span data-ttu-id="e37e0-116">Det här skriptet använder följande kommandon.</span><span class="sxs-lookup"><span data-stu-id="e37e0-116">This script uses the following commands.</span></span> <span data-ttu-id="e37e0-117">Varje kommando i tabellen länkar till kommandot viss dokumentation.</span><span class="sxs-lookup"><span data-stu-id="e37e0-117">Each command in the table links to command specific documentation.</span></span>

| <span data-ttu-id="e37e0-118">**Kommandot**</span><span class="sxs-lookup"><span data-stu-id="e37e0-118">**Command**</span></span> | <span data-ttu-id="e37e0-119">**Anteckningar**</span><span class="sxs-lookup"><span data-stu-id="e37e0-119">**Notes**</span></span> |
|---|---|
| [<span data-ttu-id="e37e0-120">Skapa AZ grupp</span><span class="sxs-lookup"><span data-stu-id="e37e0-120">az group create</span></span>](/cli/azure/group#create) | <span data-ttu-id="e37e0-121">Skapar en resursgrupp som är lagrade i alla resurser.</span><span class="sxs-lookup"><span data-stu-id="e37e0-121">Creates a resource group in which all resources are stored.</span></span> |
| [<span data-ttu-id="e37e0-122">Skapa AZ mysql-server</span><span class="sxs-lookup"><span data-stu-id="e37e0-122">az mysql server create</span></span>](/cli/azure/mysql/server#create) | <span data-ttu-id="e37e0-123">Skapar en MySQL-server som är värd för databaserna.</span><span class="sxs-lookup"><span data-stu-id="e37e0-123">Creates a MySQL server that hosts the databases.</span></span> |
| [<span data-ttu-id="e37e0-124">Skapa AZ mysql serverbrandvägg</span><span class="sxs-lookup"><span data-stu-id="e37e0-124">az mysql server firewall create</span></span>](/cli/azure/mysql/server/firewall-rule#create) | <span data-ttu-id="e37e0-125">Skapar en brandväggsregel för att tillåta åtkomst till servern och databaserna under den från det angivna IP-adressintervallet.</span><span class="sxs-lookup"><span data-stu-id="e37e0-125">Creates a firewall rule to allow access to the server and databases under it from the entered IP address range.</span></span> |
| [<span data-ttu-id="e37e0-126">ta bort grupp AZ</span><span class="sxs-lookup"><span data-stu-id="e37e0-126">az group delete</span></span>](/cli/azure/group#delete) | <span data-ttu-id="e37e0-127">Tar bort en resursgrupp, inklusive alla kapslade resurser.</span><span class="sxs-lookup"><span data-stu-id="e37e0-127">Deletes a resource group including all nested resources.</span></span> |

## <a name="next-steps"></a><span data-ttu-id="e37e0-128">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="e37e0-128">Next steps</span></span>
- <span data-ttu-id="e37e0-129">Läs mer om Azure CLI: [Azure CLI dokumentationen](/cli/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="e37e0-129">Read more information on the Azure CLI: [Azure CLI documentation](/cli/azure/overview).</span></span>
- <span data-ttu-id="e37e0-130">Försök ytterligare skript: [Azure CLI-exempel för Azure-databas för MySQL](../sample-scripts-azure-cli.md)</span><span class="sxs-lookup"><span data-stu-id="e37e0-130">Try additional scripts: [Azure CLI samples for Azure Database for MySQL](../sample-scripts-azure-cli.md)</span></span>
