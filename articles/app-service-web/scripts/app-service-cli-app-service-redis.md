---
title: "Azure CLI-skript exempel – ansluta en webbapp till ett redis-cache | Microsoft Docs"
description: "Azure CLI-skript exempel – ansluta en webbapp till ett redis-cache"
services: appservice
documentationcenter: appservice
author: syntaxc4
manager: erikre
editor: 
tags: azure-service-management
ms.assetid: bc8345b2-8487-40c6-a91f-77414e8688e6
ms.service: app-service
ms.devlang: azurecli
ms.topic: sample
ms.tgt_pltfrm: na
ms.workload: web
ms.date: 06/19/2017
ms.author: cfowler
ms.custom: mvc
ms.openlocfilehash: b697c8508a6c3422b6b0d0ca36843a9c884b505f
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="connect-a-web-app-to-a-redis-cache"></a><span data-ttu-id="c7aea-103">Ansluta en webbapp till ett redis-cache</span><span class="sxs-lookup"><span data-stu-id="c7aea-103">Connect a web app to a redis cache</span></span>

<span data-ttu-id="c7aea-104">I det här scenariot du lära dig hur du skapar ett Azure redis-cache och en Azure-webbapp.</span><span class="sxs-lookup"><span data-stu-id="c7aea-104">In this scenario you will learn how to create an Azure redis cache and an Azure web app.</span></span> <span data-ttu-id="c7aea-105">Du kan länka redis-cache till det webbprogram som använder app-inställningar.</span><span class="sxs-lookup"><span data-stu-id="c7aea-105">Then you will link the redis cache to the web app using app settings.</span></span>

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a><span data-ttu-id="c7aea-106">Exempelskript</span><span class="sxs-lookup"><span data-stu-id="c7aea-106">Sample script</span></span>

<span data-ttu-id="c7aea-107">[!code-azurecli-interactive[huvudsakliga](../../../cli_scripts/app-service/connect-to-redis/connect-to-redis.sh "Azure Redis-Cache")]</span><span class="sxs-lookup"><span data-stu-id="c7aea-107">[!code-azurecli-interactive[main](../../../cli_scripts/app-service/connect-to-redis/connect-to-redis.sh "Azure Redis Cache")]</span></span>

[!INCLUDE [cli-script-clean-up](../../../includes/cli-script-clean-up.md)]

## <a name="script-explanation"></a><span data-ttu-id="c7aea-108">Skriptet förklaring</span><span class="sxs-lookup"><span data-stu-id="c7aea-108">Script explanation</span></span>

<span data-ttu-id="c7aea-109">Skriptet använder följande kommandon för att skapa en resursgrupp, webbprogram, redis-cache och alla relaterade resurser.</span><span class="sxs-lookup"><span data-stu-id="c7aea-109">This script uses the following commands to create a resource group, web app, redis cache and all related resources.</span></span> <span data-ttu-id="c7aea-110">Varje kommando i tabellen länkar till kommandot viss dokumentation.</span><span class="sxs-lookup"><span data-stu-id="c7aea-110">Each command in the table links to command specific documentation.</span></span>

| <span data-ttu-id="c7aea-111">Kommando</span><span class="sxs-lookup"><span data-stu-id="c7aea-111">Command</span></span> | <span data-ttu-id="c7aea-112">Anteckningar</span><span class="sxs-lookup"><span data-stu-id="c7aea-112">Notes</span></span> |
|---|---|
| [<span data-ttu-id="c7aea-113">Skapa AZ grupp</span><span class="sxs-lookup"><span data-stu-id="c7aea-113">az group create</span></span>](https://docs.microsoft.com/cli/azure/group#create) | <span data-ttu-id="c7aea-114">Skapar en resursgrupp som är lagrade i alla resurser.</span><span class="sxs-lookup"><span data-stu-id="c7aea-114">Creates a resource group in which all resources are stored.</span></span> |
| [<span data-ttu-id="c7aea-115">Skapa AZ programtjänstplan</span><span class="sxs-lookup"><span data-stu-id="c7aea-115">az appservice plan create</span></span>](https://docs.microsoft.com/cli/azure/appservice/plan#create) | <span data-ttu-id="c7aea-116">Skapar en App Service-plan.</span><span class="sxs-lookup"><span data-stu-id="c7aea-116">Creates an App Service plan.</span></span> <span data-ttu-id="c7aea-117">Detta påminner om en servergrupp för din Azure webbapp.</span><span class="sxs-lookup"><span data-stu-id="c7aea-117">This is like a server farm for your Azure web app.</span></span> |
| [<span data-ttu-id="c7aea-118">Skapa AZ webapp</span><span class="sxs-lookup"><span data-stu-id="c7aea-118">az webapp create</span></span>](https://docs.microsoft.com/cli/azure/webapp#create) | <span data-ttu-id="c7aea-119">Skapar ett Azure-webbapp.</span><span class="sxs-lookup"><span data-stu-id="c7aea-119">Creates an Azure web app.</span></span> |
| [<span data-ttu-id="c7aea-120">Skapa AZ redis</span><span class="sxs-lookup"><span data-stu-id="c7aea-120">az redis create</span></span>](https://docs.microsoft.com/en-us/cli/azure/redis#create) | <span data-ttu-id="c7aea-121">Skapa en ny Redis-Cache-instans.</span><span class="sxs-lookup"><span data-stu-id="c7aea-121">Create new Redis Cache instance.</span></span> <span data-ttu-id="c7aea-122">Detta är där data ska lagras.</span><span class="sxs-lookup"><span data-stu-id="c7aea-122">This is where the data will be stored.</span></span> |
| [<span data-ttu-id="c7aea-123">AZ redis lista nycklar</span><span class="sxs-lookup"><span data-stu-id="c7aea-123">az redis list-keys</span></span>](https://docs.microsoft.com/en-us/cli/azure/redis#list-keys) | <span data-ttu-id="c7aea-124">Visar åtkomstnycklarna för redis-cacheinstansen.</span><span class="sxs-lookup"><span data-stu-id="c7aea-124">Lists the access keys for the redis cache instance.</span></span> |
| [<span data-ttu-id="c7aea-125">AZ webapp appsettings konfigurationsuppsättning</span><span class="sxs-lookup"><span data-stu-id="c7aea-125">az webapp config appsettings set</span></span>](https://docs.microsoft.com/cli/azure/webapp/config/appsettings#set) | <span data-ttu-id="c7aea-126">Skapar eller uppdaterar en appinställning för ett Azure-webbapp.</span><span class="sxs-lookup"><span data-stu-id="c7aea-126">Creates or updates an app setting for an Azure web app.</span></span> <span data-ttu-id="c7aea-127">App-inställningar visas som miljövariabler för din app.</span><span class="sxs-lookup"><span data-stu-id="c7aea-127">App settings are exposed as environment variables for your app.</span></span> |

## <a name="next-steps"></a><span data-ttu-id="c7aea-128">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="c7aea-128">Next steps</span></span>

<span data-ttu-id="c7aea-129">Mer information om Azure CLI finns [Azure CLI dokumentationen](https://docs.microsoft.com/cli/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="c7aea-129">For more information on the Azure CLI, see [Azure CLI documentation](https://docs.microsoft.com/cli/azure/overview).</span></span>

<span data-ttu-id="c7aea-130">Ytterligare App Service CLI skriptexempel finns i den [dokumentation för Azure App Service](../app-service-cli-samples.md).</span><span class="sxs-lookup"><span data-stu-id="c7aea-130">Additional App Service CLI script samples can be found in the [Azure App Service documentation](../app-service-cli-samples.md).</span></span>
