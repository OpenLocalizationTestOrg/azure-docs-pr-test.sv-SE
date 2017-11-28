---
title: aaaAzure CLI skriptexempel - ansluta en web app tooa SQL-databas | Microsoft Docs
description: Azure CLI-skript Sample - Anslut en web app tooa SQL-databas
services: appservice
documentationcenter: appservice
author: syntaxc4
manager: erikre
editor: 
tags: azure-service-management
ms.assetid: 7c2efdd0-f553-4038-a77a-e953021b3f77
ms.service: app-service
ms.devlang: azurecli
ms.topic: sample
ms.tgt_pltfrm: na
ms.workload: web
ms.date: 06/19/2017
ms.author: cfowler
ms.custom: mvc
ms.openlocfilehash: adee42cd659d977b49e71d974d240324f68f67f5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="connect-a-web-app-tooa-sql-database"></a><span data-ttu-id="1d3d0-103">Ansluta en web app tooa SQL-databas</span><span class="sxs-lookup"><span data-stu-id="1d3d0-103">Connect a web app tooa SQL database</span></span>

<span data-ttu-id="1d3d0-104">I det här scenariot du lära dig hur toocreate Azure SQL-databas och ett Azure web app.</span><span class="sxs-lookup"><span data-stu-id="1d3d0-104">In this scenario you will learn how toocreate an Azure SQL database and an Azure web app.</span></span> <span data-ttu-id="1d3d0-105">Du kan länka hello SQL-databasen toohello webbapp med app-inställningar.</span><span class="sxs-lookup"><span data-stu-id="1d3d0-105">Then you will link hello SQL database toohello web app using app settings.</span></span>


[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

<span data-ttu-id="1d3d0-106">Om du väljer tooinstall och använda hello CLI lokalt kräver i det här avsnittet att du kör hello Azure CLI version 2.0 eller senare.</span><span class="sxs-lookup"><span data-stu-id="1d3d0-106">If you choose tooinstall and use hello CLI locally, this topic requires that you are running hello Azure CLI version 2.0 or later.</span></span> <span data-ttu-id="1d3d0-107">Kör `az --version` toofind hello version.</span><span class="sxs-lookup"><span data-stu-id="1d3d0-107">Run `az --version` toofind hello version.</span></span> <span data-ttu-id="1d3d0-108">Om du behöver tooinstall eller uppgradering, se [installera Azure CLI 2.0]( /cli/azure/install-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="1d3d0-108">If you need tooinstall or upgrade, see [Install Azure CLI 2.0]( /cli/azure/install-azure-cli).</span></span> 

## <a name="sample-script"></a><span data-ttu-id="1d3d0-109">Exempelskript</span><span class="sxs-lookup"><span data-stu-id="1d3d0-109">Sample script</span></span>

[!code-azurecli-interactive[main](../../../cli_scripts/app-service/connect-to-sql/connect-to-sql.sh?highlight=9-10 "SQL Database")]

[!INCLUDE [cli-script-clean-up](../../../includes/cli-script-clean-up.md)]

## <a name="script-explanation"></a><span data-ttu-id="1d3d0-110">Skriptet förklaring</span><span class="sxs-lookup"><span data-stu-id="1d3d0-110">Script explanation</span></span>

<span data-ttu-id="1d3d0-111">Det här skriptet använder hello följande kommandon toocreate en resursgrupp, webbprogram, SQL-databasen och alla relaterade resurser.</span><span class="sxs-lookup"><span data-stu-id="1d3d0-111">This script uses hello following commands toocreate a resource group, web app, SQL database and all related resources.</span></span> <span data-ttu-id="1d3d0-112">Varje kommando i hello tabellen länkar toocommand viss dokumentation.</span><span class="sxs-lookup"><span data-stu-id="1d3d0-112">Each command in hello table links toocommand specific documentation.</span></span>

| <span data-ttu-id="1d3d0-113">Kommando</span><span class="sxs-lookup"><span data-stu-id="1d3d0-113">Command</span></span> | <span data-ttu-id="1d3d0-114">Anteckningar</span><span class="sxs-lookup"><span data-stu-id="1d3d0-114">Notes</span></span> |
|---|---|
| [<span data-ttu-id="1d3d0-115">Skapa AZ grupp</span><span class="sxs-lookup"><span data-stu-id="1d3d0-115">az group create</span></span>](https://docs.microsoft.com/cli/azure/group#create) | <span data-ttu-id="1d3d0-116">Skapar en resursgrupp som är lagrade i alla resurser.</span><span class="sxs-lookup"><span data-stu-id="1d3d0-116">Creates a resource group in which all resources are stored.</span></span> |
| [<span data-ttu-id="1d3d0-117">Skapa AZ programtjänstplan</span><span class="sxs-lookup"><span data-stu-id="1d3d0-117">az appservice plan create</span></span>](https://docs.microsoft.com/cli/azure/appservice/plan#create) | <span data-ttu-id="1d3d0-118">Skapar en App Service-plan.</span><span class="sxs-lookup"><span data-stu-id="1d3d0-118">Creates an App Service plan.</span></span> <span data-ttu-id="1d3d0-119">Detta påminner om en servergrupp för din Azure webbapp.</span><span class="sxs-lookup"><span data-stu-id="1d3d0-119">This is like a server farm for your Azure web app.</span></span> |
| [<span data-ttu-id="1d3d0-120">Skapa AZ webapp</span><span class="sxs-lookup"><span data-stu-id="1d3d0-120">az webapp create</span></span>](https://docs.microsoft.com/cli/azure/webapp#create) | <span data-ttu-id="1d3d0-121">Skapar ett Azure-webbapp.</span><span class="sxs-lookup"><span data-stu-id="1d3d0-121">Creates an Azure web app.</span></span> |
| [<span data-ttu-id="1d3d0-122">Skapa AZ SQLServer</span><span class="sxs-lookup"><span data-stu-id="1d3d0-122">az sql server create</span></span>](https://docs.microsoft.com/cli/azure/sql/server#create) | <span data-ttu-id="1d3d0-123">Skapar en SQL-databasservern.</span><span class="sxs-lookup"><span data-stu-id="1d3d0-123">Creates a SQL Database Server.</span></span>  |
| [<span data-ttu-id="1d3d0-124">Skapa AZ sql-databas</span><span class="sxs-lookup"><span data-stu-id="1d3d0-124">az sql db create</span></span>](https://docs.microsoft.com/cli/azure/sql/db#create) | <span data-ttu-id="1d3d0-125">Skapar en ny databas med hello SQL-databasservern.</span><span class="sxs-lookup"><span data-stu-id="1d3d0-125">Creates a new database with hello SQL Database Server.</span></span> |
| [<span data-ttu-id="1d3d0-126">AZ webapp appsettings konfigurationsuppsättning</span><span class="sxs-lookup"><span data-stu-id="1d3d0-126">az webapp config appsettings set</span></span>](https://docs.microsoft.com/cli/azure/webapp/config/appsettings#set) | <span data-ttu-id="1d3d0-127">Skapar eller uppdaterar en appinställning för ett Azure-webbapp.</span><span class="sxs-lookup"><span data-stu-id="1d3d0-127">Creates or updates an app setting for an Azure web app.</span></span> <span data-ttu-id="1d3d0-128">App-inställningar visas som miljövariabler för din app.</span><span class="sxs-lookup"><span data-stu-id="1d3d0-128">App settings are exposed as environment variables for your app.</span></span> |

## <a name="next-steps"></a><span data-ttu-id="1d3d0-129">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="1d3d0-129">Next steps</span></span>

<span data-ttu-id="1d3d0-130">Mer information om hello Azure CLI finns [Azure CLI dokumentationen](https://docs.microsoft.com/cli/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="1d3d0-130">For more information on hello Azure CLI, see [Azure CLI documentation](https://docs.microsoft.com/cli/azure/overview).</span></span>

<span data-ttu-id="1d3d0-131">Ytterligare App Service CLI skriptexempel finns i hello [dokumentation för Azure App Service](../app-service-cli-samples.md).</span><span class="sxs-lookup"><span data-stu-id="1d3d0-131">Additional App Service CLI script samples can be found in hello [Azure App Service documentation](../app-service-cli-samples.md).</span></span>
