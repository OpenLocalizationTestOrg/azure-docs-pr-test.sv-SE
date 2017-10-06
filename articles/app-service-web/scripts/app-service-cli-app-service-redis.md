---
title: aaaAzure CLI skriptexempel - ansluta en web app tooa redis-cache | Microsoft Docs
description: Azure CLI-skript Sample - Anslut en web app tooa redis-cache
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
ms.openlocfilehash: b911e6643591b8f07aeb64d4d62876c0fa156a8a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="connect-a-web-app-tooa-redis-cache"></a><span data-ttu-id="f1843-103">Ansluta en web app tooa redis-cache</span><span class="sxs-lookup"><span data-stu-id="f1843-103">Connect a web app tooa redis cache</span></span>

<span data-ttu-id="f1843-104">I det här scenariot du lära dig hur toocreate en Azure redis-cache och en Azure-webbapp.</span><span class="sxs-lookup"><span data-stu-id="f1843-104">In this scenario you will learn how toocreate an Azure redis cache and an Azure web app.</span></span> <span data-ttu-id="f1843-105">Du kan länka hello redis-cache toohello webbapp med app-inställningar.</span><span class="sxs-lookup"><span data-stu-id="f1843-105">Then you will link hello redis cache toohello web app using app settings.</span></span>

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a><span data-ttu-id="f1843-106">Exempelskript</span><span class="sxs-lookup"><span data-stu-id="f1843-106">Sample script</span></span>

[!code-azurecli-interactive[main](../../../cli_scripts/app-service/connect-to-redis/connect-to-redis.sh "Azure Redis Cache")]

[!INCLUDE [cli-script-clean-up](../../../includes/cli-script-clean-up.md)]

## <a name="script-explanation"></a><span data-ttu-id="f1843-107">Skriptet förklaring</span><span class="sxs-lookup"><span data-stu-id="f1843-107">Script explanation</span></span>

<span data-ttu-id="f1843-108">Det här skriptet använder hello följande kommandon toocreate en resursgrupp, webbprogram, redis-cache och alla relaterade resurser.</span><span class="sxs-lookup"><span data-stu-id="f1843-108">This script uses hello following commands toocreate a resource group, web app, redis cache and all related resources.</span></span> <span data-ttu-id="f1843-109">Varje kommando i hello tabellen länkar toocommand viss dokumentation.</span><span class="sxs-lookup"><span data-stu-id="f1843-109">Each command in hello table links toocommand specific documentation.</span></span>

| <span data-ttu-id="f1843-110">Kommando</span><span class="sxs-lookup"><span data-stu-id="f1843-110">Command</span></span> | <span data-ttu-id="f1843-111">Anteckningar</span><span class="sxs-lookup"><span data-stu-id="f1843-111">Notes</span></span> |
|---|---|
| [<span data-ttu-id="f1843-112">Skapa AZ grupp</span><span class="sxs-lookup"><span data-stu-id="f1843-112">az group create</span></span>](https://docs.microsoft.com/cli/azure/group#create) | <span data-ttu-id="f1843-113">Skapar en resursgrupp som är lagrade i alla resurser.</span><span class="sxs-lookup"><span data-stu-id="f1843-113">Creates a resource group in which all resources are stored.</span></span> |
| [<span data-ttu-id="f1843-114">Skapa AZ programtjänstplan</span><span class="sxs-lookup"><span data-stu-id="f1843-114">az appservice plan create</span></span>](https://docs.microsoft.com/cli/azure/appservice/plan#create) | <span data-ttu-id="f1843-115">Skapar en App Service-plan.</span><span class="sxs-lookup"><span data-stu-id="f1843-115">Creates an App Service plan.</span></span> <span data-ttu-id="f1843-116">Detta påminner om en servergrupp för din Azure webbapp.</span><span class="sxs-lookup"><span data-stu-id="f1843-116">This is like a server farm for your Azure web app.</span></span> |
| [<span data-ttu-id="f1843-117">Skapa AZ webapp</span><span class="sxs-lookup"><span data-stu-id="f1843-117">az webapp create</span></span>](https://docs.microsoft.com/cli/azure/webapp#create) | <span data-ttu-id="f1843-118">Skapar ett Azure-webbapp.</span><span class="sxs-lookup"><span data-stu-id="f1843-118">Creates an Azure web app.</span></span> |
| [<span data-ttu-id="f1843-119">Skapa AZ redis</span><span class="sxs-lookup"><span data-stu-id="f1843-119">az redis create</span></span>](https://docs.microsoft.com/en-us/cli/azure/redis#create) | <span data-ttu-id="f1843-120">Skapa en ny Redis-Cache-instans.</span><span class="sxs-lookup"><span data-stu-id="f1843-120">Create new Redis Cache instance.</span></span> <span data-ttu-id="f1843-121">Detta är där hello data ska lagras.</span><span class="sxs-lookup"><span data-stu-id="f1843-121">This is where hello data will be stored.</span></span> |
| [<span data-ttu-id="f1843-122">AZ redis lista nycklar</span><span class="sxs-lookup"><span data-stu-id="f1843-122">az redis list-keys</span></span>](https://docs.microsoft.com/en-us/cli/azure/redis#list-keys) | <span data-ttu-id="f1843-123">Visar hello snabbtangenter för hello redis cache-instans.</span><span class="sxs-lookup"><span data-stu-id="f1843-123">Lists hello access keys for hello redis cache instance.</span></span> |
| [<span data-ttu-id="f1843-124">AZ webapp appsettings konfigurationsuppsättning</span><span class="sxs-lookup"><span data-stu-id="f1843-124">az webapp config appsettings set</span></span>](https://docs.microsoft.com/cli/azure/webapp/config/appsettings#set) | <span data-ttu-id="f1843-125">Skapar eller uppdaterar en appinställning för ett Azure-webbapp.</span><span class="sxs-lookup"><span data-stu-id="f1843-125">Creates or updates an app setting for an Azure web app.</span></span> <span data-ttu-id="f1843-126">App-inställningar visas som miljövariabler för din app.</span><span class="sxs-lookup"><span data-stu-id="f1843-126">App settings are exposed as environment variables for your app.</span></span> |

## <a name="next-steps"></a><span data-ttu-id="f1843-127">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="f1843-127">Next steps</span></span>

<span data-ttu-id="f1843-128">Mer information om hello Azure CLI finns [Azure CLI dokumentationen](https://docs.microsoft.com/cli/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="f1843-128">For more information on hello Azure CLI, see [Azure CLI documentation](https://docs.microsoft.com/cli/azure/overview).</span></span>

<span data-ttu-id="f1843-129">Ytterligare App Service CLI skriptexempel finns i hello [dokumentation för Azure App Service](../app-service-cli-samples.md).</span><span class="sxs-lookup"><span data-stu-id="f1843-129">Additional App Service CLI script samples can be found in hello [Azure App Service documentation](../app-service-cli-samples.md).</span></span>
