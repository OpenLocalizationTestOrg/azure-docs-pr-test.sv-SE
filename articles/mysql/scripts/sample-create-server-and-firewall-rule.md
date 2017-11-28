---
title: "aaa ”Azure CLI Script - skapa en Azure-databas för MySQL | Microsoft Docs ”"
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
ms.openlocfilehash: 1d619ee0547efd8275eaf7c1347b6c3427025c3f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-mysql-server-and-configure-a-firewall-rule-using-hello-azure-cli"></a><span data-ttu-id="26b66-103">Skapa en MySQL-server och konfigurera en brandväggsregel som använder hello Azure CLI</span><span class="sxs-lookup"><span data-stu-id="26b66-103">Create a MySQL server and configure a firewall rule using hello Azure CLI</span></span>
<span data-ttu-id="26b66-104">Det här exempelskriptet CLI skapar en Azure-databas för MySQL-server och konfigurerar en brandväggsregel på servernivå.</span><span class="sxs-lookup"><span data-stu-id="26b66-104">This sample CLI script creates an Azure Database for MySQL server and configures a server-level firewall rule.</span></span> <span data-ttu-id="26b66-105">När hello skriptet körs utan problem, hello MySQL-servern kan nås av alla Azure-tjänster och hello konfigurerade IP-adress.</span><span class="sxs-lookup"><span data-stu-id="26b66-105">Once hello script runs successfully, hello MySQL server is accessible by all Azure services and hello configured IP address.</span></span>

[!INCLUDE [cloud-shell-try-it](../../../includes/cloud-shell-try-it.md)]

<span data-ttu-id="26b66-106">Om du väljer tooinstall och använda hello CLI lokalt kräver i det här avsnittet att du kör hello Azure CLI version 2.0 eller senare.</span><span class="sxs-lookup"><span data-stu-id="26b66-106">If you choose tooinstall and use hello CLI locally, this topic requires that you are running hello Azure CLI version 2.0 or later.</span></span> <span data-ttu-id="26b66-107">Kör `az --version` toofind hello version.</span><span class="sxs-lookup"><span data-stu-id="26b66-107">Run `az --version` toofind hello version.</span></span> <span data-ttu-id="26b66-108">Om du behöver tooinstall eller uppgradering, se [installera Azure CLI 2.0]( /cli/azure/install-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="26b66-108">If you need tooinstall or upgrade, see [Install Azure CLI 2.0]( /cli/azure/install-azure-cli).</span></span> 

## <a name="sample-script"></a><span data-ttu-id="26b66-109">Exempelskript</span><span class="sxs-lookup"><span data-stu-id="26b66-109">Sample script</span></span>
<span data-ttu-id="26b66-110">I det här exempelskriptet redigera hello markerade rader toocustomize hello admin användarnamn och lösenord.</span><span class="sxs-lookup"><span data-stu-id="26b66-110">In this sample script, edit hello highlighted lines toocustomize hello admin username and password.</span></span>
[!code-azurecli-interactive[main](../../../cli_scripts/mysql/create-mysql-server-and-firewall-rule/create-mysql-server-and-firewall-rule.sh?highlight=15-16 "Create an Azure Database for MySQL, and server-level firewall rule.")]

## <a name="clean-up-deployment"></a><span data-ttu-id="26b66-111">Rensa distribution</span><span class="sxs-lookup"><span data-stu-id="26b66-111">Clean up deployment</span></span>
<span data-ttu-id="26b66-112">Efter hello skriptexempel har körts, kan det vara hello följande kommando används tooremove hello resursgruppen och alla resurser som är kopplade till den.</span><span class="sxs-lookup"><span data-stu-id="26b66-112">After hello script sample has been run, hello following command can be used tooremove hello resource group and all resources associated with it.</span></span>
[!code-azurecli-interactive[main](../../../cli_scripts/mysql/create-mysql-server-and-firewall-rule/delete-mysql.sh "Delete hello resource group.")]

## <a name="script-explanation"></a><span data-ttu-id="26b66-113">Skriptet förklaring</span><span class="sxs-lookup"><span data-stu-id="26b66-113">Script explanation</span></span>
<span data-ttu-id="26b66-114">Det här skriptet använder hello följande kommandon.</span><span class="sxs-lookup"><span data-stu-id="26b66-114">This script uses hello following commands.</span></span> <span data-ttu-id="26b66-115">Varje kommando i hello tabellen länkar toocommand viss dokumentation.</span><span class="sxs-lookup"><span data-stu-id="26b66-115">Each command in hello table links toocommand specific documentation.</span></span>

| <span data-ttu-id="26b66-116">**Kommandot**</span><span class="sxs-lookup"><span data-stu-id="26b66-116">**Command**</span></span> | <span data-ttu-id="26b66-117">**Anteckningar**</span><span class="sxs-lookup"><span data-stu-id="26b66-117">**Notes**</span></span> |
|---|---|
| [<span data-ttu-id="26b66-118">Skapa AZ grupp</span><span class="sxs-lookup"><span data-stu-id="26b66-118">az group create</span></span>](/cli/azure/group#create) | <span data-ttu-id="26b66-119">Skapar en resursgrupp som är lagrade i alla resurser.</span><span class="sxs-lookup"><span data-stu-id="26b66-119">Creates a resource group in which all resources are stored.</span></span> |
| [<span data-ttu-id="26b66-120">Skapa AZ mysql-server</span><span class="sxs-lookup"><span data-stu-id="26b66-120">az mysql server create</span></span>](/cli/azure/mysql/server#create) | <span data-ttu-id="26b66-121">Skapar en MySQL-server som är värd för hello databaser.</span><span class="sxs-lookup"><span data-stu-id="26b66-121">Creates a MySQL server that hosts hello databases.</span></span> |
| [<span data-ttu-id="26b66-122">Skapa AZ mysql serverbrandvägg</span><span class="sxs-lookup"><span data-stu-id="26b66-122">az mysql server firewall create</span></span>](/cli/azure/mysql/server/firewall-rule#create) | <span data-ttu-id="26b66-123">Skapar en regel tooallow åtkomst toohello brandväggsserver och databaserna under den från hello angivna IP-adressintervall.</span><span class="sxs-lookup"><span data-stu-id="26b66-123">Creates a firewall rule tooallow access toohello server and databases under it from hello entered IP address range.</span></span> |
| [<span data-ttu-id="26b66-124">ta bort grupp AZ</span><span class="sxs-lookup"><span data-stu-id="26b66-124">az group delete</span></span>](/cli/azure/group#delete) | <span data-ttu-id="26b66-125">Tar bort en resursgrupp, inklusive alla kapslade resurser.</span><span class="sxs-lookup"><span data-stu-id="26b66-125">Deletes a resource group including all nested resources.</span></span> |

## <a name="next-steps"></a><span data-ttu-id="26b66-126">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="26b66-126">Next steps</span></span>
- <span data-ttu-id="26b66-127">Läs mer om hello Azure CLI: [Azure CLI dokumentationen](/cli/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="26b66-127">Read more information on hello Azure CLI: [Azure CLI documentation](/cli/azure/overview).</span></span>
- <span data-ttu-id="26b66-128">Försök ytterligare skript: [Azure CLI-exempel för Azure-databas för MySQL](../sample-scripts-azure-cli.md)</span><span class="sxs-lookup"><span data-stu-id="26b66-128">Try additional scripts: [Azure CLI samples for Azure Database for MySQL](../sample-scripts-azure-cli.md)</span></span>
