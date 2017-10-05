---
title: "Azure CLI-skript Sample - skapa en webbapp med kontinuerlig distribution från GitHub | Microsoft Docs"
description: "Azure CLI-skript Sample - skapa en webbapp med kontinuerlig distribution från GitHub"
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
ms.tgt_pltfrm: na
ms.topic: sample
ms.date: 06/19/2017
ms.author: cephalin
ms.custom: mvc
ms.openlocfilehash: a12085a7a8146c22d6b079381542d4fe3a8e6e87
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="create-a-web-app-with-continuous-deployment-from-github"></a><span data-ttu-id="c68a1-103">Skapa en webbapp med kontinuerlig distribution från GitHub</span><span class="sxs-lookup"><span data-stu-id="c68a1-103">Create a web app with continuous deployment from GitHub</span></span>

<span data-ttu-id="c68a1-104">Det här exempelskriptet skapar en webbapp i App Service med dess relaterade resurser och sedan konfigurerar kontinuerlig distribution från en GitHub-databas.</span><span class="sxs-lookup"><span data-stu-id="c68a1-104">This sample script creates a web app in App Service with its related resources, and then sets up continuous deployment from a GitHub repository.</span></span> <span data-ttu-id="c68a1-105">GitHub distribution utan kontinuerlig distribution finns [skapa en webbapp och distribuera kod från GitHub](app-service-cli-deploy-github.md).</span><span class="sxs-lookup"><span data-stu-id="c68a1-105">For GitHub deployment without continuous deployment, see [Create a web app and deploy code from GitHub](app-service-cli-deploy-github.md).</span></span> <span data-ttu-id="c68a1-106">I det här exemplet behöver du:</span><span class="sxs-lookup"><span data-stu-id="c68a1-106">In this sample, you will need:</span></span>

* <span data-ttu-id="c68a1-107">En GitHub-databas med programkod som du har administrativa behörigheter för.</span><span class="sxs-lookup"><span data-stu-id="c68a1-107">A GitHub repository with application code, that you have administrative permissions for.</span></span>
* <span data-ttu-id="c68a1-108">En [personlig åtkomst-Token (PATRIK)](https://help.github.com/articles/creating-an-access-token-for-command-line-use) för ditt GitHub-konto.</span><span class="sxs-lookup"><span data-stu-id="c68a1-108">A [Personal Access Token (PAT)](https://help.github.com/articles/creating-an-access-token-for-command-line-use) for your GitHub account.</span></span>

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

<span data-ttu-id="c68a1-109">Om du väljer att installera och använda CLI lokalt måste du köra Azure CLI version 2.0 eller senare.</span><span class="sxs-lookup"><span data-stu-id="c68a1-109">If you choose to install and use the CLI locally, this topic requires that you are running the Azure CLI version 2.0 or later.</span></span> <span data-ttu-id="c68a1-110">Kör `az --version` för att hitta versionen.</span><span class="sxs-lookup"><span data-stu-id="c68a1-110">Run `az --version` to find the version.</span></span> <span data-ttu-id="c68a1-111">Om du behöver installera eller uppgradera kan du läsa [Installera Azure CLI 2.0]( /cli/azure/install-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="c68a1-111">If you need to install or upgrade, see [Install Azure CLI 2.0]( /cli/azure/install-azure-cli).</span></span> 

## <a name="sample-script"></a><span data-ttu-id="c68a1-112">Exempelskript</span><span class="sxs-lookup"><span data-stu-id="c68a1-112">Sample script</span></span>

<span data-ttu-id="c68a1-113">[!code-azurecli-interactive[huvudsakliga](../../../cli_scripts/app-service/deploy-github-continuous/deploy-github-continuous.sh?highlight=3-4 "skapa ett webbprogram med kontinuerlig distribution från GitHub")]</span><span class="sxs-lookup"><span data-stu-id="c68a1-113">[!code-azurecli-interactive[main](../../../cli_scripts/app-service/deploy-github-continuous/deploy-github-continuous.sh?highlight=3-4 "Create a web app with continuous deployment from GitHub")]</span></span>

[!INCLUDE [cli-script-clean-up](../../../includes/cli-script-clean-up.md)]

## <a name="script-explanation"></a><span data-ttu-id="c68a1-114">Skriptet förklaring</span><span class="sxs-lookup"><span data-stu-id="c68a1-114">Script explanation</span></span>

<span data-ttu-id="c68a1-115">Det här skriptet använder följande kommandon.</span><span class="sxs-lookup"><span data-stu-id="c68a1-115">This script uses the following commands.</span></span> <span data-ttu-id="c68a1-116">Varje kommando i tabellen länkar till kommandot viss dokumentation.</span><span class="sxs-lookup"><span data-stu-id="c68a1-116">Each command in the table links to command specific documentation.</span></span>

| <span data-ttu-id="c68a1-117">Kommando</span><span class="sxs-lookup"><span data-stu-id="c68a1-117">Command</span></span> | <span data-ttu-id="c68a1-118">Anteckningar</span><span class="sxs-lookup"><span data-stu-id="c68a1-118">Notes</span></span> |
|---|---|
| [<span data-ttu-id="c68a1-119">Skapa AZ grupp</span><span class="sxs-lookup"><span data-stu-id="c68a1-119">az group create</span></span>](https://docs.microsoft.com/cli/azure/group#create) | <span data-ttu-id="c68a1-120">Skapar en resursgrupp som är lagrade i alla resurser.</span><span class="sxs-lookup"><span data-stu-id="c68a1-120">Creates a resource group in which all resources are stored.</span></span> |
| [<span data-ttu-id="c68a1-121">Skapa AZ programtjänstplan</span><span class="sxs-lookup"><span data-stu-id="c68a1-121">az appservice plan create</span></span>](https://docs.microsoft.com/cli/azure/appservice/plan#create) | <span data-ttu-id="c68a1-122">Skapar en App Service-plan.</span><span class="sxs-lookup"><span data-stu-id="c68a1-122">Creates an App Service plan.</span></span> |
| [<span data-ttu-id="c68a1-123">Skapa AZ webapp</span><span class="sxs-lookup"><span data-stu-id="c68a1-123">az webapp create</span></span>](https://docs.microsoft.com/cli/azure/webapp#create) | <span data-ttu-id="c68a1-124">Skapar ett Azure-webbapp.</span><span class="sxs-lookup"><span data-stu-id="c68a1-124">Creates an Azure web app.</span></span> |
| [<span data-ttu-id="c68a1-125">AZ webapp distributionskonfiguration källa</span><span class="sxs-lookup"><span data-stu-id="c68a1-125">az webapp deployment source config</span></span>](https://docs.microsoft.com/cli/azure/webapp/deployment/source#config) | <span data-ttu-id="c68a1-126">Associerar en Azure-app med en Git eller ett lagringsplats.</span><span class="sxs-lookup"><span data-stu-id="c68a1-126">Associates an Azure web app with a Git or Mercurial repository.</span></span> |
| [<span data-ttu-id="c68a1-127">AZ webapp Bläddra</span><span class="sxs-lookup"><span data-stu-id="c68a1-127">az webapp browse</span></span>](https://docs.microsoft.com/cli/azure/webapp#browse) | <span data-ttu-id="c68a1-128">Öppna en Azure-app i en webbläsare.</span><span class="sxs-lookup"><span data-stu-id="c68a1-128">Open an Azure web app in a browser.</span></span> |

## <a name="next-steps"></a><span data-ttu-id="c68a1-129">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="c68a1-129">Next steps</span></span>

<span data-ttu-id="c68a1-130">Mer information om Azure CLI finns [Azure CLI dokumentationen](https://docs.microsoft.com/cli/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="c68a1-130">For more information on the Azure CLI, see [Azure CLI documentation](https://docs.microsoft.com/cli/azure/overview).</span></span>

<span data-ttu-id="c68a1-131">Ytterligare App Service CLI skriptexempel finns i den [dokumentation för Azure App Service](../app-service-cli-samples.md).</span><span class="sxs-lookup"><span data-stu-id="c68a1-131">Additional App Service CLI script samples can be found in the [Azure App Service documentation](../app-service-cli-samples.md).</span></span>
