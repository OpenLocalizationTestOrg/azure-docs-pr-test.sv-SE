---
title: "Azure CLI-skript exempel – ansluta en webbapp till en SQL-databas | Microsoft Docs"
description: "Azure CLI-skript exempel – ansluta en webbapp till en SQL-databas"
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
ms.openlocfilehash: ec5e22bfacc12a89f1fb5882487df4829369777c
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="connect-a-web-app-to-a-sql-database"></a><span data-ttu-id="5ca88-103">Ansluta en webbapp till en SQL-databas</span><span class="sxs-lookup"><span data-stu-id="5ca88-103">Connect a web app to a SQL database</span></span>

<span data-ttu-id="5ca88-104">I det här scenariot du lära dig hur du skapar en Azure SQL database och en Azure-webbapp.</span><span class="sxs-lookup"><span data-stu-id="5ca88-104">In this scenario you will learn how to create an Azure SQL database and an Azure web app.</span></span> <span data-ttu-id="5ca88-105">Du kan länka SQL-databasen till det webbprogram som använder app-inställningar.</span><span class="sxs-lookup"><span data-stu-id="5ca88-105">Then you will link the SQL database to the web app using app settings.</span></span>


[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

<span data-ttu-id="5ca88-106">Om du väljer att installera och använda CLI lokalt måste du köra Azure CLI version 2.0 eller senare.</span><span class="sxs-lookup"><span data-stu-id="5ca88-106">If you choose to install and use the CLI locally, this topic requires that you are running the Azure CLI version 2.0 or later.</span></span> <span data-ttu-id="5ca88-107">Kör `az --version` för att hitta versionen.</span><span class="sxs-lookup"><span data-stu-id="5ca88-107">Run `az --version` to find the version.</span></span> <span data-ttu-id="5ca88-108">Om du behöver installera eller uppgradera kan du läsa [Installera Azure CLI 2.0]( /cli/azure/install-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="5ca88-108">If you need to install or upgrade, see [Install Azure CLI 2.0]( /cli/azure/install-azure-cli).</span></span> 

## <a name="sample-script"></a><span data-ttu-id="5ca88-109">Exempelskript</span><span class="sxs-lookup"><span data-stu-id="5ca88-109">Sample script</span></span>

<span data-ttu-id="5ca88-110">[!code-azurecli-interactive[huvudsakliga](../../../cli_scripts/app-service/connect-to-sql/connect-to-sql.sh?highlight=9-10 "SQL-databas")]</span><span class="sxs-lookup"><span data-stu-id="5ca88-110">[!code-azurecli-interactive[main](../../../cli_scripts/app-service/connect-to-sql/connect-to-sql.sh?highlight=9-10 "SQL Database")]</span></span>

[!INCLUDE [cli-script-clean-up](../../../includes/cli-script-clean-up.md)]

## <a name="script-explanation"></a><span data-ttu-id="5ca88-111">Skriptet förklaring</span><span class="sxs-lookup"><span data-stu-id="5ca88-111">Script explanation</span></span>

<span data-ttu-id="5ca88-112">Det här skriptet använder följande kommandon för att skapa en resursgrupp, webbprogram, SQL-databasen och alla relaterade resurser.</span><span class="sxs-lookup"><span data-stu-id="5ca88-112">This script uses the following commands to create a resource group, web app, SQL database and all related resources.</span></span> <span data-ttu-id="5ca88-113">Varje kommando i tabellen länkar till kommandot viss dokumentation.</span><span class="sxs-lookup"><span data-stu-id="5ca88-113">Each command in the table links to command specific documentation.</span></span>

| <span data-ttu-id="5ca88-114">Kommando</span><span class="sxs-lookup"><span data-stu-id="5ca88-114">Command</span></span> | <span data-ttu-id="5ca88-115">Anteckningar</span><span class="sxs-lookup"><span data-stu-id="5ca88-115">Notes</span></span> |
|---|---|
| [<span data-ttu-id="5ca88-116">Skapa AZ grupp</span><span class="sxs-lookup"><span data-stu-id="5ca88-116">az group create</span></span>](https://docs.microsoft.com/cli/azure/group#create) | <span data-ttu-id="5ca88-117">Skapar en resursgrupp som är lagrade i alla resurser.</span><span class="sxs-lookup"><span data-stu-id="5ca88-117">Creates a resource group in which all resources are stored.</span></span> |
| [<span data-ttu-id="5ca88-118">Skapa AZ programtjänstplan</span><span class="sxs-lookup"><span data-stu-id="5ca88-118">az appservice plan create</span></span>](https://docs.microsoft.com/cli/azure/appservice/plan#create) | <span data-ttu-id="5ca88-119">Skapar en App Service-plan.</span><span class="sxs-lookup"><span data-stu-id="5ca88-119">Creates an App Service plan.</span></span> <span data-ttu-id="5ca88-120">Detta påminner om en servergrupp för din Azure webbapp.</span><span class="sxs-lookup"><span data-stu-id="5ca88-120">This is like a server farm for your Azure web app.</span></span> |
| [<span data-ttu-id="5ca88-121">Skapa AZ webapp</span><span class="sxs-lookup"><span data-stu-id="5ca88-121">az webapp create</span></span>](https://docs.microsoft.com/cli/azure/webapp#create) | <span data-ttu-id="5ca88-122">Skapar ett Azure-webbapp.</span><span class="sxs-lookup"><span data-stu-id="5ca88-122">Creates an Azure web app.</span></span> |
| [<span data-ttu-id="5ca88-123">Skapa AZ SQLServer</span><span class="sxs-lookup"><span data-stu-id="5ca88-123">az sql server create</span></span>](https://docs.microsoft.com/cli/azure/sql/server#create) | <span data-ttu-id="5ca88-124">Skapar en SQL-databasservern.</span><span class="sxs-lookup"><span data-stu-id="5ca88-124">Creates a SQL Database Server.</span></span>  |
| [<span data-ttu-id="5ca88-125">Skapa AZ sql-databas</span><span class="sxs-lookup"><span data-stu-id="5ca88-125">az sql db create</span></span>](https://docs.microsoft.com/cli/azure/sql/db#create) | <span data-ttu-id="5ca88-126">Skapar en ny databas med SQL-databasservern.</span><span class="sxs-lookup"><span data-stu-id="5ca88-126">Creates a new database with the SQL Database Server.</span></span> |
| [<span data-ttu-id="5ca88-127">AZ webapp appsettings konfigurationsuppsättning</span><span class="sxs-lookup"><span data-stu-id="5ca88-127">az webapp config appsettings set</span></span>](https://docs.microsoft.com/cli/azure/webapp/config/appsettings#set) | <span data-ttu-id="5ca88-128">Skapar eller uppdaterar en appinställning för ett Azure-webbapp.</span><span class="sxs-lookup"><span data-stu-id="5ca88-128">Creates or updates an app setting for an Azure web app.</span></span> <span data-ttu-id="5ca88-129">App-inställningar visas som miljövariabler för din app.</span><span class="sxs-lookup"><span data-stu-id="5ca88-129">App settings are exposed as environment variables for your app.</span></span> |

## <a name="next-steps"></a><span data-ttu-id="5ca88-130">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="5ca88-130">Next steps</span></span>

<span data-ttu-id="5ca88-131">Mer information om Azure CLI finns [Azure CLI dokumentationen](https://docs.microsoft.com/cli/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="5ca88-131">For more information on the Azure CLI, see [Azure CLI documentation](https://docs.microsoft.com/cli/azure/overview).</span></span>

<span data-ttu-id="5ca88-132">Ytterligare App Service CLI skriptexempel finns i den [dokumentation för Azure App Service](../app-service-cli-samples.md).</span><span class="sxs-lookup"><span data-stu-id="5ca88-132">Additional App Service CLI script samples can be found in the [Azure App Service documentation](../app-service-cli-samples.md).</span></span>
