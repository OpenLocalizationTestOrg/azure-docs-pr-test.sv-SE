---
title: "Skriptexempel Azure CLI - karta en anpassad domän till en webbapp | Microsoft Docs"
description: "Skriptexempel Azure CLI - karta en anpassad domän till en webbapp"
services: app-service\web
documentationcenter: 
author: cephalin
manager: erikre
editor: 
tags: azure-service-management
ms.assetid: 5ac4a680-cc73-4578-bcd6-8668c08802c2
ms.service: app-service-web
ms.workload: web
ms.devlang: azurecli
ms.tgt_pltfrm: na
ms.topic: sample
ms.date: 06/19/2017
ms.author: cephalin
ms.custom: mvc
ms.openlocfilehash: 6712be8a551731fbafd92ef19564e89399e23e76
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="map-a-custom-domain-to-a-web-app"></a><span data-ttu-id="fb612-103">Mappa en anpassad domän till en webbapp</span><span class="sxs-lookup"><span data-stu-id="fb612-103">Map a custom domain to a web app</span></span>

<span data-ttu-id="fb612-104">Det här exempelskriptet skapar en webbapp i App Service med dess relaterade resurser och sedan mappa `www.<yourdomain>` till den.</span><span class="sxs-lookup"><span data-stu-id="fb612-104">This sample script creates a web app in App Service with its related resources, and then maps `www.<yourdomain>` to it.</span></span>

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

<span data-ttu-id="fb612-105">Om du väljer att installera och använda CLI lokalt måste du köra Azure CLI version 2.0 eller senare.</span><span class="sxs-lookup"><span data-stu-id="fb612-105">If you choose to install and use the CLI locally, this topic requires that you are running the Azure CLI version 2.0 or later.</span></span> <span data-ttu-id="fb612-106">Kör `az --version` för att hitta versionen.</span><span class="sxs-lookup"><span data-stu-id="fb612-106">Run `az --version` to find the version.</span></span> <span data-ttu-id="fb612-107">Om du behöver installera eller uppgradera kan du läsa [Installera Azure CLI 2.0]( /cli/azure/install-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="fb612-107">If you need to install or upgrade, see [Install Azure CLI 2.0]( /cli/azure/install-azure-cli).</span></span> 

## <a name="sample-script"></a><span data-ttu-id="fb612-108">Exempelskript</span><span class="sxs-lookup"><span data-stu-id="fb612-108">Sample script</span></span>

<span data-ttu-id="fb612-109">[!code-azurecli-interactive[huvudsakliga](../../../cli_scripts/app-service/configure-custom-domain/configure-custom-domain.sh?highlight=3 "mappa en anpassad domän till en webbapp")]</span><span class="sxs-lookup"><span data-stu-id="fb612-109">[!code-azurecli-interactive[main](../../../cli_scripts/app-service/configure-custom-domain/configure-custom-domain.sh?highlight=3 "Map a custom domain to a web app")]</span></span>

[!INCLUDE [cli-script-clean-up](../../../includes/cli-script-clean-up.md)]

## <a name="script-explanation"></a><span data-ttu-id="fb612-110">Skriptet förklaring</span><span class="sxs-lookup"><span data-stu-id="fb612-110">Script explanation</span></span>

<span data-ttu-id="fb612-111">Det här skriptet använder följande kommandon.</span><span class="sxs-lookup"><span data-stu-id="fb612-111">This script uses the following commands.</span></span> <span data-ttu-id="fb612-112">Varje kommando i tabellen länkar till kommandot viss dokumentation.</span><span class="sxs-lookup"><span data-stu-id="fb612-112">Each command in the table links to command specific documentation.</span></span>

| <span data-ttu-id="fb612-113">Kommando</span><span class="sxs-lookup"><span data-stu-id="fb612-113">Command</span></span> | <span data-ttu-id="fb612-114">Anteckningar</span><span class="sxs-lookup"><span data-stu-id="fb612-114">Notes</span></span> |
|---|---|
| [<span data-ttu-id="fb612-115">Skapa AZ grupp</span><span class="sxs-lookup"><span data-stu-id="fb612-115">az group create</span></span>](https://docs.microsoft.com/cli/azure/group#create) | <span data-ttu-id="fb612-116">Skapar en resursgrupp som är lagrade i alla resurser.</span><span class="sxs-lookup"><span data-stu-id="fb612-116">Creates a resource group in which all resources are stored.</span></span> |
| [<span data-ttu-id="fb612-117">Skapa AZ programtjänstplan</span><span class="sxs-lookup"><span data-stu-id="fb612-117">az appservice plan create</span></span>](https://docs.microsoft.com/cli/azure/appservice/plan#create) | <span data-ttu-id="fb612-118">Skapar en App Service-plan.</span><span class="sxs-lookup"><span data-stu-id="fb612-118">Creates an App Service plan.</span></span> |
| [<span data-ttu-id="fb612-119">Skapa AZ webapp</span><span class="sxs-lookup"><span data-stu-id="fb612-119">az webapp create</span></span>](https://docs.microsoft.com/cli/azure/webapp#create) | <span data-ttu-id="fb612-120">Skapar ett Azure-webbapp.</span><span class="sxs-lookup"><span data-stu-id="fb612-120">Creates an Azure web app.</span></span> |
| [<span data-ttu-id="fb612-121">Lägg till AZ webapp config värdnamn</span><span class="sxs-lookup"><span data-stu-id="fb612-121">az webapp config hostname add</span></span>](https://docs.microsoft.com/cli/azure/webapp/config/hostname#add) | <span data-ttu-id="fb612-122">Avbildar en anpassad domän till en webbapp.</span><span class="sxs-lookup"><span data-stu-id="fb612-122">Maps a custom domain to a web app.</span></span> |

## <a name="next-steps"></a><span data-ttu-id="fb612-123">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="fb612-123">Next steps</span></span>

<span data-ttu-id="fb612-124">Mer information om Azure CLI finns [Azure CLI dokumentationen](https://docs.microsoft.com/cli/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="fb612-124">For more information on the Azure CLI, see [Azure CLI documentation](https://docs.microsoft.com/cli/azure/overview).</span></span>

<span data-ttu-id="fb612-125">Ytterligare App Service CLI skriptexempel finns i den [dokumentation för Azure App Service](../app-service-cli-samples.md).</span><span class="sxs-lookup"><span data-stu-id="fb612-125">Additional App Service CLI script samples can be found in the [Azure App Service documentation](../app-service-cli-samples.md).</span></span>
