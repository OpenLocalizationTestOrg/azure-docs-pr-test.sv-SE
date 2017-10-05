---
title: "Azure CLI-skript exempel – ansluta en webbapp till Cosmos DB | Microsoft Docs"
description: "Azure CLI-skript exempel – ansluta en webbapp till Cosmos DB"
services: appservice
documentationcenter: appservice
author: syntaxc4
manager: erikre
editor: 
tags: azure-service-management
ms.assetid: bbbdbc42-efb5-4b4f-8ba6-c03c9d16a7ea
ms.service: app-service
ms.devlang: azurecli
ms.topic: sample
ms.tgt_pltfrm: na
ms.workload: web
ms.date: 06/19/2017
ms.author: cfowler
ms.custom: mvc
ms.openlocfilehash: ff5e7a794033cc51120831e09b055a86affb28a4
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="connect-a-web-app-to-cosmos-db"></a><span data-ttu-id="90a95-103">Ansluta en webbapp till Cosmos DB</span><span class="sxs-lookup"><span data-stu-id="90a95-103">Connect a web app to Cosmos DB</span></span>

<span data-ttu-id="90a95-104">I det här scenariot du lära dig hur du skapar ett Azure DB som Cosmos-konto och ett Azure-webbapp.</span><span class="sxs-lookup"><span data-stu-id="90a95-104">In this scenario you will learn how to create an Azure Cosmos DB account and an Azure web app.</span></span> <span data-ttu-id="90a95-105">Du kan länka Cosmos-DB till det webbprogram som använder app-inställningar.</span><span class="sxs-lookup"><span data-stu-id="90a95-105">Then you will link the Cosmos DB to the web app using app settings.</span></span>


[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]


[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

<span data-ttu-id="90a95-106">Om du väljer att installera och använda CLI lokalt måste du köra Azure CLI version 2.0 eller senare.</span><span class="sxs-lookup"><span data-stu-id="90a95-106">If you choose to install and use the CLI locally, this topic requires that you are running the Azure CLI version 2.0 or later.</span></span> <span data-ttu-id="90a95-107">Kör `az --version` för att hitta versionen.</span><span class="sxs-lookup"><span data-stu-id="90a95-107">Run `az --version` to find the version.</span></span> <span data-ttu-id="90a95-108">Om du behöver installera eller uppgradera kan du läsa [Installera Azure CLI 2.0]( /cli/azure/install-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="90a95-108">If you need to install or upgrade, see [Install Azure CLI 2.0]( /cli/azure/install-azure-cli).</span></span> 

## <a name="sample-script"></a><span data-ttu-id="90a95-109">Exempelskript</span><span class="sxs-lookup"><span data-stu-id="90a95-109">Sample script</span></span>

<span data-ttu-id="90a95-110">[!code-azurecli-interactive[huvudsakliga](../../../cli_scripts/app-service/connect-to-documentdb/connect-to-documentdb.sh "Azure Cosmos DB")]</span><span class="sxs-lookup"><span data-stu-id="90a95-110">[!code-azurecli-interactive[main](../../../cli_scripts/app-service/connect-to-documentdb/connect-to-documentdb.sh "Azure Cosmos DB")]</span></span>

[!INCLUDE [cli-script-clean-up](../../../includes/cli-script-clean-up.md)]

## <a name="script-explanation"></a><span data-ttu-id="90a95-111">Skriptet förklaring</span><span class="sxs-lookup"><span data-stu-id="90a95-111">Script explanation</span></span>

<span data-ttu-id="90a95-112">Det här skriptet använder följande kommandon för att skapa en resursgrupp, webbapp Cosmos DB och alla relaterade resurser.</span><span class="sxs-lookup"><span data-stu-id="90a95-112">This script uses the following commands to create a resource group, web app, Cosmos DB and all related resources.</span></span> <span data-ttu-id="90a95-113">Varje kommando i tabellen länkar till kommandot viss dokumentation.</span><span class="sxs-lookup"><span data-stu-id="90a95-113">Each command in the table links to command specific documentation.</span></span>

| <span data-ttu-id="90a95-114">Kommando</span><span class="sxs-lookup"><span data-stu-id="90a95-114">Command</span></span> | <span data-ttu-id="90a95-115">Anteckningar</span><span class="sxs-lookup"><span data-stu-id="90a95-115">Notes</span></span> |
|---|---|
| [<span data-ttu-id="90a95-116">Skapa AZ grupp</span><span class="sxs-lookup"><span data-stu-id="90a95-116">az group create</span></span>](https://docs.microsoft.com/cli/azure/group#create) | <span data-ttu-id="90a95-117">Skapar en resursgrupp som är lagrade i alla resurser.</span><span class="sxs-lookup"><span data-stu-id="90a95-117">Creates a resource group in which all resources are stored.</span></span> |
| [<span data-ttu-id="90a95-118">Skapa AZ programtjänstplan</span><span class="sxs-lookup"><span data-stu-id="90a95-118">az appservice plan create</span></span>](https://docs.microsoft.com/cli/azure/appservice/plan#create) | <span data-ttu-id="90a95-119">Skapar en App Service-plan.</span><span class="sxs-lookup"><span data-stu-id="90a95-119">Creates an App Service plan.</span></span> <span data-ttu-id="90a95-120">Detta påminner om en servergrupp för din Azure webbapp.</span><span class="sxs-lookup"><span data-stu-id="90a95-120">This is like a server farm for your Azure web app.</span></span> |
| [<span data-ttu-id="90a95-121">Skapa AZ webapp</span><span class="sxs-lookup"><span data-stu-id="90a95-121">az webapp create</span></span>](https://docs.microsoft.com/cli/azure/webapp#create) | <span data-ttu-id="90a95-122">Skapar ett Azure-webbapp.</span><span class="sxs-lookup"><span data-stu-id="90a95-122">Creates an Azure web app.</span></span> |
| [<span data-ttu-id="90a95-123">Skapa AZ cosmosdb</span><span class="sxs-lookup"><span data-stu-id="90a95-123">az cosmosdb create</span></span>](https://docs.microsoft.com/en-us/cli/azure/cosmosdb#create) | <span data-ttu-id="90a95-124">Skapar en Cosmos-DB-konto.</span><span class="sxs-lookup"><span data-stu-id="90a95-124">Creates a Cosmos DB account.</span></span> <span data-ttu-id="90a95-125">Detta är där data ska lagras.</span><span class="sxs-lookup"><span data-stu-id="90a95-125">This is where the data will be stored.</span></span> |
| [<span data-ttu-id="90a95-126">AZ cosmosdb lista nycklar</span><span class="sxs-lookup"><span data-stu-id="90a95-126">az cosmosdb list-keys</span></span>](https://docs.microsoft.com/en-us/cli/azure/cosmosdb#list-keys) | <span data-ttu-id="90a95-127">Visar en lista över åtkomstnycklarna för det angivna Cosmos-DB-kontot.</span><span class="sxs-lookup"><span data-stu-id="90a95-127">Lists the access keys for the specified Cosmos DB account.</span></span> |
| [<span data-ttu-id="90a95-128">AZ webapp appsettings konfigurationsuppsättning</span><span class="sxs-lookup"><span data-stu-id="90a95-128">az webapp config appsettings set</span></span>](https://docs.microsoft.com/cli/azure/webapp/config/appsettings#set) | <span data-ttu-id="90a95-129">Skapar eller uppdaterar en appinställning för ett Azure-webbapp.</span><span class="sxs-lookup"><span data-stu-id="90a95-129">Creates or updates an app setting for an Azure web app.</span></span> <span data-ttu-id="90a95-130">App-inställningar visas som miljövariabler för din app.</span><span class="sxs-lookup"><span data-stu-id="90a95-130">App settings are exposed as environment variables for your app.</span></span> |

## <a name="next-steps"></a><span data-ttu-id="90a95-131">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="90a95-131">Next steps</span></span>

<span data-ttu-id="90a95-132">Mer information om Azure CLI finns [Azure CLI dokumentationen](https://docs.microsoft.com/cli/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="90a95-132">For more information on the Azure CLI, see [Azure CLI documentation](https://docs.microsoft.com/cli/azure/overview).</span></span>

<span data-ttu-id="90a95-133">Ytterligare App Service CLI skriptexempel finns i den [dokumentation för Azure App Service](../app-service-cli-samples.md).</span><span class="sxs-lookup"><span data-stu-id="90a95-133">Additional App Service CLI script samples can be found in the [Azure App Service documentation](../app-service-cli-samples.md).</span></span>
