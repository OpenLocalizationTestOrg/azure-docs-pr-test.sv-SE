---
title: "Skriptexempel Azure CLI - skala ett webbprogram manuellt med hjälp av Azure CLI 2.0 | Microsoft Docs"
description: "Skriptexempel Azure CLI - skala ett webbprogram manuellt med hjälp av Azure CLI 2.0"
services: appservice
documentationcenter: appservice
author: syntaxc4
manager: erikre
editor: 
tags: azure-service-management
ms.assetid: 251d9074-8fff-4121-ad16-9eca9556ac96
ms.service: app-service
ms.devlang: azurecli
ms.topic: sample
ms.tgt_pltfrm: na
ms.workload: web
ms.date: 06/19/2017
ms.author: cfowler
ms.custom: mvc
ms.openlocfilehash: fe05661eb4e2d5c37aebdbfde002b34588db69e7
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="scale-a-web-app-manually"></a><span data-ttu-id="4507a-103">Skala ett webbprogram manuellt</span><span class="sxs-lookup"><span data-stu-id="4507a-103">Scale a web app manually</span></span>

<span data-ttu-id="4507a-104">I det här scenariot du lär dig att skapa en resursgrupp, en app service-plan och webb-app.</span><span class="sxs-lookup"><span data-stu-id="4507a-104">In this scenario you will learn to create a resource group, app service plan and web app.</span></span> <span data-ttu-id="4507a-105">Du kommer sedan att skala i App Service-Plan från en enda instans till flera instanser.</span><span class="sxs-lookup"><span data-stu-id="4507a-105">You will then scale the App Service Plan from a single instance to multiple instances.</span></span>

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

<span data-ttu-id="4507a-106">Om du väljer att installera och använda CLI lokalt måste du köra Azure CLI version 2.0 eller senare.</span><span class="sxs-lookup"><span data-stu-id="4507a-106">If you choose to install and use the CLI locally, this topic requires that you are running the Azure CLI version 2.0 or later.</span></span> <span data-ttu-id="4507a-107">Kör `az --version` för att hitta versionen.</span><span class="sxs-lookup"><span data-stu-id="4507a-107">Run `az --version` to find the version.</span></span> <span data-ttu-id="4507a-108">Om du behöver installera eller uppgradera kan du läsa [Installera Azure CLI 2.0]( /cli/azure/install-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="4507a-108">If you need to install or upgrade, see [Install Azure CLI 2.0]( /cli/azure/install-azure-cli).</span></span> 

## <a name="sample-script"></a><span data-ttu-id="4507a-109">Exempelskript</span><span class="sxs-lookup"><span data-stu-id="4507a-109">Sample script</span></span>

<span data-ttu-id="4507a-110">[!code-azurecli-interactive[huvudsakliga](../../../cli_scripts/app-service/scale-manual/scale-manual.sh "manuell skala")]</span><span class="sxs-lookup"><span data-stu-id="4507a-110">[!code-azurecli-interactive[main](../../../cli_scripts/app-service/scale-manual/scale-manual.sh "Manual Scale")]</span></span>

[!INCLUDE [cli-script-clean-up](../../../includes/cli-script-clean-up.md)]

## <a name="script-explanation"></a><span data-ttu-id="4507a-111">Skriptet förklaring</span><span class="sxs-lookup"><span data-stu-id="4507a-111">Script explanation</span></span>

<span data-ttu-id="4507a-112">Det här skriptet använder följande kommandon för att skapa en resursgrupp, webbprogram och alla relaterade resurser.</span><span class="sxs-lookup"><span data-stu-id="4507a-112">This script uses the following commands to create a resource group, web app, and all related resources.</span></span> <span data-ttu-id="4507a-113">Varje kommando i tabellen länkar till kommandot viss dokumentation.</span><span class="sxs-lookup"><span data-stu-id="4507a-113">Each command in the table links to command specific documentation.</span></span>

| <span data-ttu-id="4507a-114">Kommando</span><span class="sxs-lookup"><span data-stu-id="4507a-114">Command</span></span> | <span data-ttu-id="4507a-115">Anteckningar</span><span class="sxs-lookup"><span data-stu-id="4507a-115">Notes</span></span> |
|---|---|
| [<span data-ttu-id="4507a-116">Skapa AZ grupp</span><span class="sxs-lookup"><span data-stu-id="4507a-116">az group create</span></span>](https://docs.microsoft.com/cli/azure/group#create) | <span data-ttu-id="4507a-117">Skapar en resursgrupp som är lagrade i alla resurser.</span><span class="sxs-lookup"><span data-stu-id="4507a-117">Creates a resource group in which all resources are stored.</span></span> |
| [<span data-ttu-id="4507a-118">Skapa AZ programtjänstplan</span><span class="sxs-lookup"><span data-stu-id="4507a-118">az appservice plan create</span></span>](https://docs.microsoft.com/cli/azure/appservice/plan#create) | <span data-ttu-id="4507a-119">Skapar en App Service-plan.</span><span class="sxs-lookup"><span data-stu-id="4507a-119">Creates an App Service plan.</span></span> <span data-ttu-id="4507a-120">Detta påminner om en servergrupp för din Azure webbapp.</span><span class="sxs-lookup"><span data-stu-id="4507a-120">This is like a server farm for your Azure web app.</span></span> |
| [<span data-ttu-id="4507a-121">Skapa AZ webapp</span><span class="sxs-lookup"><span data-stu-id="4507a-121">az webapp create</span></span>](https://docs.microsoft.com/cli/azure/webapp#create) | <span data-ttu-id="4507a-122">Skapar ett Azure-webbapp.</span><span class="sxs-lookup"><span data-stu-id="4507a-122">Creates an Azure web app.</span></span> |
| [<span data-ttu-id="4507a-123">AZ apptjänst plan uppdatering</span><span class="sxs-lookup"><span data-stu-id="4507a-123">az appservice plan update</span></span>](https://docs.microsoft.com/cli/azure/appservice/plan#update) | <span data-ttu-id="4507a-124">Uppdaterar egenskaperna för App Service-plan.</span><span class="sxs-lookup"><span data-stu-id="4507a-124">Updates properties of the App Service plan.</span></span> |

## <a name="next-steps"></a><span data-ttu-id="4507a-125">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="4507a-125">Next steps</span></span>

<span data-ttu-id="4507a-126">Mer information om Azure CLI finns [Azure CLI dokumentationen](https://docs.microsoft.com/cli/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="4507a-126">For more information on the Azure CLI, see [Azure CLI documentation](https://docs.microsoft.com/cli/azure/overview).</span></span>

<span data-ttu-id="4507a-127">Ytterligare App Service CLI skriptexempel finns i den [dokumentation för Azure App Service](../app-service-cli-samples.md).</span><span class="sxs-lookup"><span data-stu-id="4507a-127">Additional App Service CLI script samples can be found in the [Azure App Service documentation](../app-service-cli-samples.md).</span></span>
