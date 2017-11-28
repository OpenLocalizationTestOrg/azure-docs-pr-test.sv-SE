---
title: "aaaAzure CLI skriptexempel - skapa en webbapp med distribution från GitHub | Microsoft Docs"
description: "Azure CLI-skript Sample - skapa en webbapp med distribution från GitHub"
services: app-service\web
documentationcenter: 
author: cephalin
manager: erikre
editor: 
tags: azure-service-management
ms.assetid: 0205c991-0989-4ca3-bb41-237dcc964460
ms.service: app-service-web
ms.workload: web
ms.devlang: azurecli
ms.tgt_pltfrm: sample
ms.topic: sample
ms.date: 06/19/2017
ms.author: cephalin
ms.custom: mvc
ms.openlocfilehash: eb7231aa5c6a7e23d76885107e733008382f7487
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-web-app-with-deployment-from-github"></a><span data-ttu-id="3ad7c-103">Skapa en webbapp med distribution från GitHub</span><span class="sxs-lookup"><span data-stu-id="3ad7c-103">Create a web app with deployment from GitHub</span></span>

<span data-ttu-id="3ad7c-104">Det här exempelskriptet skapar en webbapp i App Service med dess relaterade resurser och distribuerar sedan web app-kod från en offentlig GitHub-databas (utan kontinuerlig distribution).</span><span class="sxs-lookup"><span data-stu-id="3ad7c-104">This sample script creates a web app in App Service with its related resources, and then deploys your web app code from a public GitHub repository (without continuous deployment).</span></span> <span data-ttu-id="3ad7c-105">GitHub-distribution med kontinuerlig distribution finns [skapa ett webbprogram med kontinuerlig distribution från GitHub](app-service-cli-continuous-deployment-github.md).</span><span class="sxs-lookup"><span data-stu-id="3ad7c-105">For GitHub deployment with continuous deployment, see [Create a web app with continuous deployment from GitHub](app-service-cli-continuous-deployment-github.md).</span></span>

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

<span data-ttu-id="3ad7c-106">Om du väljer tooinstall och använda hello CLI lokalt kräver i det här avsnittet att du kör hello Azure CLI version 2.0 eller senare.</span><span class="sxs-lookup"><span data-stu-id="3ad7c-106">If you choose tooinstall and use hello CLI locally, this topic requires that you are running hello Azure CLI version 2.0 or later.</span></span> <span data-ttu-id="3ad7c-107">Kör `az --version` toofind hello version.</span><span class="sxs-lookup"><span data-stu-id="3ad7c-107">Run `az --version` toofind hello version.</span></span> <span data-ttu-id="3ad7c-108">Om du behöver tooinstall eller uppgradering, se [installera Azure CLI 2.0]( /cli/azure/install-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="3ad7c-108">If you need tooinstall or upgrade, see [Install Azure CLI 2.0]( /cli/azure/install-azure-cli).</span></span> 

## <a name="sample-script"></a><span data-ttu-id="3ad7c-109">Exempelskript</span><span class="sxs-lookup"><span data-stu-id="3ad7c-109">Sample script</span></span>

[!code-azurecli-interactive[main](../../../cli_scripts/app-service/deploy-github/deploy-github.sh?highlight=3 "Create a web app with deployment from GitHub")]

[!INCLUDE [cli-script-clean-up](../../../includes/cli-script-clean-up.md)]

## <a name="script-explanation"></a><span data-ttu-id="3ad7c-110">Skriptet förklaring</span><span class="sxs-lookup"><span data-stu-id="3ad7c-110">Script explanation</span></span> 

<span data-ttu-id="3ad7c-111">Det här skriptet använder hello följande kommandon.</span><span class="sxs-lookup"><span data-stu-id="3ad7c-111">This script uses hello following commands.</span></span> <span data-ttu-id="3ad7c-112">Varje kommando i hello tabellen länkar toocommand viss dokumentation.</span><span class="sxs-lookup"><span data-stu-id="3ad7c-112">Each command in hello table links toocommand specific documentation.</span></span>

| <span data-ttu-id="3ad7c-113">Kommando</span><span class="sxs-lookup"><span data-stu-id="3ad7c-113">Command</span></span> | <span data-ttu-id="3ad7c-114">Anteckningar</span><span class="sxs-lookup"><span data-stu-id="3ad7c-114">Notes</span></span> |
|---|---|
| [<span data-ttu-id="3ad7c-115">Skapa AZ grupp</span><span class="sxs-lookup"><span data-stu-id="3ad7c-115">az group create</span></span>](https://docs.microsoft.com/cli/azure/group#create) | <span data-ttu-id="3ad7c-116">Skapar en resursgrupp som är lagrade i alla resurser.</span><span class="sxs-lookup"><span data-stu-id="3ad7c-116">Creates a resource group in which all resources are stored.</span></span> |
| [<span data-ttu-id="3ad7c-117">Skapa AZ programtjänstplan</span><span class="sxs-lookup"><span data-stu-id="3ad7c-117">az appservice plan create</span></span>](https://docs.microsoft.com/cli/azure/appservice/plan#create) | <span data-ttu-id="3ad7c-118">Skapar en App Service-plan.</span><span class="sxs-lookup"><span data-stu-id="3ad7c-118">Creates an App Service plan.</span></span> |
| [<span data-ttu-id="3ad7c-119">Skapa AZ webapp</span><span class="sxs-lookup"><span data-stu-id="3ad7c-119">az webapp create</span></span>](https://docs.microsoft.com/cli/azure/webapp#create) | <span data-ttu-id="3ad7c-120">Skapar ett Azure-webbapp.</span><span class="sxs-lookup"><span data-stu-id="3ad7c-120">Creates an Azure web app.</span></span> |
| [<span data-ttu-id="3ad7c-121">AZ webapp distributionskonfiguration källa</span><span class="sxs-lookup"><span data-stu-id="3ad7c-121">az webapp deployment source config</span></span>](https://docs.microsoft.com/cli/azure/webapp/deployment/source#config) | <span data-ttu-id="3ad7c-122">Associerar en Azure-app med en Git eller ett lagringsplats.</span><span class="sxs-lookup"><span data-stu-id="3ad7c-122">Associates an Azure web app with a Git or Mercurial repository.</span></span> |
| [<span data-ttu-id="3ad7c-123">AZ webapp Bläddra</span><span class="sxs-lookup"><span data-stu-id="3ad7c-123">az webapp browse</span></span>](https://docs.microsoft.com/cli/azure/webapp#browse) | <span data-ttu-id="3ad7c-124">Öppna en Azure-app i en webbläsare.</span><span class="sxs-lookup"><span data-stu-id="3ad7c-124">Open an Azure web app in a browser.</span></span> |

## <a name="next-steps"></a><span data-ttu-id="3ad7c-125">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="3ad7c-125">Next steps</span></span>

<span data-ttu-id="3ad7c-126">Mer information om hello Azure CLI finns [Azure CLI dokumentationen](https://docs.microsoft.com/cli/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="3ad7c-126">For more information on hello Azure CLI, see [Azure CLI documentation](https://docs.microsoft.com/cli/azure/overview).</span></span>

<span data-ttu-id="3ad7c-127">Ytterligare App Service CLI skriptexempel finns i hello [dokumentation för Azure App Service](../app-service-cli-samples.md).</span><span class="sxs-lookup"><span data-stu-id="3ad7c-127">Additional App Service CLI script samples can be found in hello [Azure App Service documentation](../app-service-cli-samples.md).</span></span>
