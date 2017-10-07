---
title: "aaaAzure CLI skriptexempel - skapa en webbapp med kontinuerlig distribution från GitHub | Microsoft Docs"
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
ms.openlocfilehash: 6adb06a35ceea8ea64723c9887c25c50f046e280
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-web-app-with-continuous-deployment-from-github"></a><span data-ttu-id="254d4-103">Skapa en webbapp med kontinuerlig distribution från GitHub</span><span class="sxs-lookup"><span data-stu-id="254d4-103">Create a web app with continuous deployment from GitHub</span></span>

<span data-ttu-id="254d4-104">Det här exempelskriptet skapar en webbapp i App Service med dess relaterade resurser och sedan konfigurerar kontinuerlig distribution från en GitHub-databas.</span><span class="sxs-lookup"><span data-stu-id="254d4-104">This sample script creates a web app in App Service with its related resources, and then sets up continuous deployment from a GitHub repository.</span></span> <span data-ttu-id="254d4-105">GitHub distribution utan kontinuerlig distribution finns [skapa en webbapp och distribuera kod från GitHub](app-service-cli-deploy-github.md).</span><span class="sxs-lookup"><span data-stu-id="254d4-105">For GitHub deployment without continuous deployment, see [Create a web app and deploy code from GitHub](app-service-cli-deploy-github.md).</span></span> <span data-ttu-id="254d4-106">I det här exemplet behöver du:</span><span class="sxs-lookup"><span data-stu-id="254d4-106">In this sample, you will need:</span></span>

* <span data-ttu-id="254d4-107">En GitHub-databas med programkod som du har administrativa behörigheter för.</span><span class="sxs-lookup"><span data-stu-id="254d4-107">A GitHub repository with application code, that you have administrative permissions for.</span></span>
* <span data-ttu-id="254d4-108">En [personlig åtkomst-Token (PATRIK)](https://help.github.com/articles/creating-an-access-token-for-command-line-use) för ditt GitHub-konto.</span><span class="sxs-lookup"><span data-stu-id="254d4-108">A [Personal Access Token (PAT)](https://help.github.com/articles/creating-an-access-token-for-command-line-use) for your GitHub account.</span></span>

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

<span data-ttu-id="254d4-109">Om du väljer tooinstall och använda hello CLI lokalt kräver i det här avsnittet att du kör hello Azure CLI version 2.0 eller senare.</span><span class="sxs-lookup"><span data-stu-id="254d4-109">If you choose tooinstall and use hello CLI locally, this topic requires that you are running hello Azure CLI version 2.0 or later.</span></span> <span data-ttu-id="254d4-110">Kör `az --version` toofind hello version.</span><span class="sxs-lookup"><span data-stu-id="254d4-110">Run `az --version` toofind hello version.</span></span> <span data-ttu-id="254d4-111">Om du behöver tooinstall eller uppgradering, se [installera Azure CLI 2.0]( /cli/azure/install-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="254d4-111">If you need tooinstall or upgrade, see [Install Azure CLI 2.0]( /cli/azure/install-azure-cli).</span></span> 

## <a name="sample-script"></a><span data-ttu-id="254d4-112">Exempelskript</span><span class="sxs-lookup"><span data-stu-id="254d4-112">Sample script</span></span>

[!code-azurecli-interactive[main](../../../cli_scripts/app-service/deploy-github-continuous/deploy-github-continuous.sh?highlight=3-4 "Create a web app with continuous deployment from GitHub")]

[!INCLUDE [cli-script-clean-up](../../../includes/cli-script-clean-up.md)]

## <a name="script-explanation"></a><span data-ttu-id="254d4-113">Skriptet förklaring</span><span class="sxs-lookup"><span data-stu-id="254d4-113">Script explanation</span></span>

<span data-ttu-id="254d4-114">Det här skriptet använder hello följande kommandon.</span><span class="sxs-lookup"><span data-stu-id="254d4-114">This script uses hello following commands.</span></span> <span data-ttu-id="254d4-115">Varje kommando i hello tabellen länkar toocommand viss dokumentation.</span><span class="sxs-lookup"><span data-stu-id="254d4-115">Each command in hello table links toocommand specific documentation.</span></span>

| <span data-ttu-id="254d4-116">Kommando</span><span class="sxs-lookup"><span data-stu-id="254d4-116">Command</span></span> | <span data-ttu-id="254d4-117">Anteckningar</span><span class="sxs-lookup"><span data-stu-id="254d4-117">Notes</span></span> |
|---|---|
| [<span data-ttu-id="254d4-118">Skapa AZ grupp</span><span class="sxs-lookup"><span data-stu-id="254d4-118">az group create</span></span>](https://docs.microsoft.com/cli/azure/group#create) | <span data-ttu-id="254d4-119">Skapar en resursgrupp som är lagrade i alla resurser.</span><span class="sxs-lookup"><span data-stu-id="254d4-119">Creates a resource group in which all resources are stored.</span></span> |
| [<span data-ttu-id="254d4-120">Skapa AZ programtjänstplan</span><span class="sxs-lookup"><span data-stu-id="254d4-120">az appservice plan create</span></span>](https://docs.microsoft.com/cli/azure/appservice/plan#create) | <span data-ttu-id="254d4-121">Skapar en App Service-plan.</span><span class="sxs-lookup"><span data-stu-id="254d4-121">Creates an App Service plan.</span></span> |
| [<span data-ttu-id="254d4-122">Skapa AZ webapp</span><span class="sxs-lookup"><span data-stu-id="254d4-122">az webapp create</span></span>](https://docs.microsoft.com/cli/azure/webapp#create) | <span data-ttu-id="254d4-123">Skapar ett Azure-webbapp.</span><span class="sxs-lookup"><span data-stu-id="254d4-123">Creates an Azure web app.</span></span> |
| [<span data-ttu-id="254d4-124">AZ webapp distributionskonfiguration källa</span><span class="sxs-lookup"><span data-stu-id="254d4-124">az webapp deployment source config</span></span>](https://docs.microsoft.com/cli/azure/webapp/deployment/source#config) | <span data-ttu-id="254d4-125">Associerar en Azure-app med en Git eller ett lagringsplats.</span><span class="sxs-lookup"><span data-stu-id="254d4-125">Associates an Azure web app with a Git or Mercurial repository.</span></span> |
| [<span data-ttu-id="254d4-126">AZ webapp Bläddra</span><span class="sxs-lookup"><span data-stu-id="254d4-126">az webapp browse</span></span>](https://docs.microsoft.com/cli/azure/webapp#browse) | <span data-ttu-id="254d4-127">Öppna en Azure-app i en webbläsare.</span><span class="sxs-lookup"><span data-stu-id="254d4-127">Open an Azure web app in a browser.</span></span> |

## <a name="next-steps"></a><span data-ttu-id="254d4-128">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="254d4-128">Next steps</span></span>

<span data-ttu-id="254d4-129">Mer information om hello Azure CLI finns [Azure CLI dokumentationen](https://docs.microsoft.com/cli/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="254d4-129">For more information on hello Azure CLI, see [Azure CLI documentation](https://docs.microsoft.com/cli/azure/overview).</span></span>

<span data-ttu-id="254d4-130">Ytterligare App Service CLI skriptexempel finns i hello [dokumentation för Azure App Service](../app-service-cli-samples.md).</span><span class="sxs-lookup"><span data-stu-id="254d4-130">Additional App Service CLI script samples can be found in hello [Azure App Service documentation](../app-service-cli-samples.md).</span></span>
