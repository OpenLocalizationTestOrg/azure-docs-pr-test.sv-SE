---
title: "aaaAzure CLI skriptexempel - skapa en ASP.NET Core webbprogram i en dockerbehållare från registret för Azure-behållare | Microsoft Docs"
description: "Azure CLI-skript exempel – skapa en ASP.NET Core webbprogram i en dockerbehållare från registret för Azure-behållare"
services: appservice
documentationcenter: appservice
author: syntaxc4
manager: erikre
editor: 
tags: azure-service-management
ms.assetid: 3a2d1983-ff7b-476a-ac44-49ec2aabb31a
ms.service: app-service
ms.devlang: azurecli
ms.topic: sample
ms.tgt_pltfrm: na
ms.workload: web
ms.date: 06/19/2017
ms.author: cfowler
ms.custom: mvc
ms.openlocfilehash: 0d4b1e706c2401ef813f48ef4de3d17fa2b6c9e0
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-an-aspnet-core-web-app-in-a-docker-container-from-azure-container-registry"></a><span data-ttu-id="e7a7f-103">Skapa en ASP.NET Core webbprogram i en dockerbehållare från registret för Azure-behållare</span><span class="sxs-lookup"><span data-stu-id="e7a7f-103">Create an ASP.NET Core web app in a Docker container from Azure Container Registry</span></span>

<span data-ttu-id="e7a7f-104">I det här scenariot du lära dig hur toocreate en resursgrupp, Linux app service-plan och webbapp och distribuera ett ASP.NET Core-program med hjälp av en Docker-behållare från hello Azure Container registret.</span><span class="sxs-lookup"><span data-stu-id="e7a7f-104">In this scenario you will learn how toocreate a resource group, Linux app service plan, and web app, and deploy an ASP.NET Core application using a Docker Container from hello Azure Container Registry.</span></span>


[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

<span data-ttu-id="e7a7f-105">Om du väljer tooinstall och använda hello CLI lokalt kräver i det här avsnittet att du kör hello Azure CLI version 2.0 eller senare.</span><span class="sxs-lookup"><span data-stu-id="e7a7f-105">If you choose tooinstall and use hello CLI locally, this topic requires that you are running hello Azure CLI version 2.0 or later.</span></span> <span data-ttu-id="e7a7f-106">Kör `az --version` toofind hello version.</span><span class="sxs-lookup"><span data-stu-id="e7a7f-106">Run `az --version` toofind hello version.</span></span> <span data-ttu-id="e7a7f-107">Om du behöver tooinstall eller uppgradering, se [installera Azure CLI 2.0]( /cli/azure/install-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="e7a7f-107">If you need tooinstall or upgrade, see [Install Azure CLI 2.0]( /cli/azure/install-azure-cli).</span></span> 

## <a name="sample-script"></a><span data-ttu-id="e7a7f-108">Exempelskript</span><span class="sxs-lookup"><span data-stu-id="e7a7f-108">Sample script</span></span>

[!code-azurecli-interactive[main](../../../cli_scripts/app-service/deploy-linux-acr/deploy-linux-acr.sh?highlight=6-9 "Linux Azure Container Registry")]

[!INCLUDE [cli-script-clean-up](../../../includes/cli-script-clean-up.md)]

## <a name="script-explanation"></a><span data-ttu-id="e7a7f-109">Skriptet förklaring</span><span class="sxs-lookup"><span data-stu-id="e7a7f-109">Script explanation</span></span>

<span data-ttu-id="e7a7f-110">Det här skriptet använder hello följande kommandon toocreate en resursgrupp, webbprogram och alla relaterade resurser.</span><span class="sxs-lookup"><span data-stu-id="e7a7f-110">This script uses hello following commands toocreate a resource group, web app, and all related resources.</span></span> <span data-ttu-id="e7a7f-111">Varje kommando i hello tabellen länkar toocommand viss dokumentation.</span><span class="sxs-lookup"><span data-stu-id="e7a7f-111">Each command in hello table links toocommand specific documentation.</span></span>

| <span data-ttu-id="e7a7f-112">Kommando</span><span class="sxs-lookup"><span data-stu-id="e7a7f-112">Command</span></span> | <span data-ttu-id="e7a7f-113">Anteckningar</span><span class="sxs-lookup"><span data-stu-id="e7a7f-113">Notes</span></span> |
|---|---|
| [<span data-ttu-id="e7a7f-114">Skapa AZ grupp</span><span class="sxs-lookup"><span data-stu-id="e7a7f-114">az group create</span></span>](https://docs.microsoft.com/cli/azure/group#create) | <span data-ttu-id="e7a7f-115">Skapar en resursgrupp som är lagrade i alla resurser.</span><span class="sxs-lookup"><span data-stu-id="e7a7f-115">Creates a resource group in which all resources are stored.</span></span> |
| [<span data-ttu-id="e7a7f-116">Skapa AZ programtjänstplan</span><span class="sxs-lookup"><span data-stu-id="e7a7f-116">az appservice plan create</span></span>](https://docs.microsoft.com/cli/azure/appservice/plan#create) | <span data-ttu-id="e7a7f-117">Skapar en App Service-plan.</span><span class="sxs-lookup"><span data-stu-id="e7a7f-117">Creates an App Service plan.</span></span> <span data-ttu-id="e7a7f-118">Detta påminner om en servergrupp för din Azure webbapp.</span><span class="sxs-lookup"><span data-stu-id="e7a7f-118">This is like a server farm for your Azure web app.</span></span> |
| [<span data-ttu-id="e7a7f-119">Skapa AZ webapp</span><span class="sxs-lookup"><span data-stu-id="e7a7f-119">az webapp create</span></span>](https://docs.microsoft.com/cli/azure/webapp#create) | <span data-ttu-id="e7a7f-120">Skapar ett Azure-webbapp.</span><span class="sxs-lookup"><span data-stu-id="e7a7f-120">Creates an Azure web app.</span></span> |
| [<span data-ttu-id="e7a7f-121">AZ webapp konfigurationsuppsättning behållare</span><span class="sxs-lookup"><span data-stu-id="e7a7f-121">az webapp config container set</span></span>](https://docs.microsoft.com/cli/azure/webapp/config/container#set) | <span data-ttu-id="e7a7f-122">Anger hello dockerbehållare för hello Azure webbapp.</span><span class="sxs-lookup"><span data-stu-id="e7a7f-122">Sets hello Docker container for hello Azure web app.</span></span> |

## <a name="next-steps"></a><span data-ttu-id="e7a7f-123">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="e7a7f-123">Next steps</span></span>

<span data-ttu-id="e7a7f-124">Mer information om hello Azure CLI finns [Azure CLI dokumentationen](https://docs.microsoft.com/cli/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="e7a7f-124">For more information on hello Azure CLI, see [Azure CLI documentation](https://docs.microsoft.com/cli/azure/overview).</span></span>

<span data-ttu-id="e7a7f-125">Ytterligare App Service CLI skriptexempel finns i hello [dokumentation för Azure App Service](../app-service-cli-samples.md).</span><span class="sxs-lookup"><span data-stu-id="e7a7f-125">Additional App Service CLI script samples can be found in hello [Azure App Service documentation](../app-service-cli-samples.md).</span></span>
