---
title: "Azure CLI-skript Sample - skapa en webbapp med kontinuerlig distribution från Visual Studio Team Services | Microsoft Docs"
description: "Azure CLI-skript Sample - skapa en webbapp med kontinuerlig distribution från Visual Studio Team Services"
services: app-service\web
documentationcenter: 
author: syntaxc4
manager: erikre
editor: 
tags: azure-service-management
ms.assetid: 389d3bd3-cd8e-4715-a3a1-031ec061d385
ms.service: app-service-web
ms.workload: web
ms.devlang: azurecli
ms.tgt_pltfrm: na
ms.topic: sample
ms.date: 06/19/2017
ms.author: cfowler
ms.custom: mvc
ms.openlocfilehash: 2b983616757ca3c4226c12876f5fd4c285067318
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="create-a-web-app-with-continuous-deployment-from-visual-studio-team-services"></a><span data-ttu-id="848e1-103">Skapa en webbapp med kontinuerlig distribution från Visual Studio Team Services</span><span class="sxs-lookup"><span data-stu-id="848e1-103">Create a web app with continuous deployment from Visual Studio Team Services</span></span>

<span data-ttu-id="848e1-104">Det här exempelskriptet skapar en webbapp i App Service med dess relaterade resurser och ställer sedan in kontinuerlig distribution från Visual Studio Team Services-databasen.</span><span class="sxs-lookup"><span data-stu-id="848e1-104">This sample script creates a web app in App Service with its related resources, and then sets up continuous deployment from a Visual Studio Team Services repository.</span></span> <span data-ttu-id="848e1-105">För det här exemplet behöver du:</span><span class="sxs-lookup"><span data-stu-id="848e1-105">For this sample, you will need:</span></span>

* <span data-ttu-id="848e1-106">Ett Visual Studio Team Services-databas med programkod som du har administrativa behörigheter för.</span><span class="sxs-lookup"><span data-stu-id="848e1-106">A Visual Studio Team Services repository with application code, that you have administrative permissions for.</span></span>
* <span data-ttu-id="848e1-107">En [personlig åtkomst-Token (PATRIK)](https://www.visualstudio.com/docs/setup-admin/team-services/use-personal-access-tokens-to-authenticate) för Visual Studio Team Services-konto.</span><span class="sxs-lookup"><span data-stu-id="848e1-107">A [Personal Access Token (PAT)](https://www.visualstudio.com/docs/setup-admin/team-services/use-personal-access-tokens-to-authenticate) for your Visual Studio Team Services account.</span></span>

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]


[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

<span data-ttu-id="848e1-108">Om du väljer att installera och använda CLI lokalt måste du köra Azure CLI version 2.0 eller senare.</span><span class="sxs-lookup"><span data-stu-id="848e1-108">If you choose to install and use the CLI locally, this topic requires that you are running the Azure CLI version 2.0 or later.</span></span> <span data-ttu-id="848e1-109">Kör `az --version` för att hitta versionen.</span><span class="sxs-lookup"><span data-stu-id="848e1-109">Run `az --version` to find the version.</span></span> <span data-ttu-id="848e1-110">Om du behöver installera eller uppgradera kan du läsa [Installera Azure CLI 2.0]( /cli/azure/install-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="848e1-110">If you need to install or upgrade, see [Install Azure CLI 2.0]( /cli/azure/install-azure-cli).</span></span> 

## <a name="sample-script"></a><span data-ttu-id="848e1-111">Exempelskript</span><span class="sxs-lookup"><span data-stu-id="848e1-111">Sample script</span></span>

<span data-ttu-id="848e1-112">[!code-azurecli-interactive[huvudsakliga](../../../cli_scripts/app-service/deploy-vsts-continuous/deploy-vsts-continuous.sh?highlight=3-4 "skapa ett webbprogram med kontinuerlig distribution från Visual Studio Team Services")]</span><span class="sxs-lookup"><span data-stu-id="848e1-112">[!code-azurecli-interactive[main](../../../cli_scripts/app-service/deploy-vsts-continuous/deploy-vsts-continuous.sh?highlight=3-4 "Create a web app with continuous deployment from Visual Studio Team Services")]</span></span>

[!INCLUDE [cli-script-clean-up](../../../includes/cli-script-clean-up.md)]

## <a name="script-explanation"></a><span data-ttu-id="848e1-113">Skriptet förklaring</span><span class="sxs-lookup"><span data-stu-id="848e1-113">Script explanation</span></span>

<span data-ttu-id="848e1-114">Det här skriptet använder följande kommandon.</span><span class="sxs-lookup"><span data-stu-id="848e1-114">This script uses the following commands.</span></span> <span data-ttu-id="848e1-115">Varje kommando i tabellen länkar till kommandot viss dokumentation.</span><span class="sxs-lookup"><span data-stu-id="848e1-115">Each command in the table links to command specific documentation.</span></span>

| <span data-ttu-id="848e1-116">Kommando</span><span class="sxs-lookup"><span data-stu-id="848e1-116">Command</span></span> | <span data-ttu-id="848e1-117">Anteckningar</span><span class="sxs-lookup"><span data-stu-id="848e1-117">Notes</span></span> |
|---|---|
| [<span data-ttu-id="848e1-118">Skapa AZ grupp</span><span class="sxs-lookup"><span data-stu-id="848e1-118">az group create</span></span>](https://docs.microsoft.com/cli/azure/group#create) | <span data-ttu-id="848e1-119">Skapar en resursgrupp som är lagrade i alla resurser.</span><span class="sxs-lookup"><span data-stu-id="848e1-119">Creates a resource group in which all resources are stored.</span></span> |
| [<span data-ttu-id="848e1-120">Skapa AZ programtjänstplan</span><span class="sxs-lookup"><span data-stu-id="848e1-120">az appservice plan create</span></span>](https://docs.microsoft.com/cli/azure/appservice/plan#create) | <span data-ttu-id="848e1-121">Skapar en App Service-plan.</span><span class="sxs-lookup"><span data-stu-id="848e1-121">Creates an App Service plan.</span></span> |
| [<span data-ttu-id="848e1-122">Skapa AZ webapp</span><span class="sxs-lookup"><span data-stu-id="848e1-122">az webapp create</span></span>](https://docs.microsoft.com/cli/azure/webapp#create) | <span data-ttu-id="848e1-123">Skapar ett Azure-webbapp.</span><span class="sxs-lookup"><span data-stu-id="848e1-123">Creates an Azure web app.</span></span> |
| [<span data-ttu-id="848e1-124">AZ webapp distributionskonfiguration källa</span><span class="sxs-lookup"><span data-stu-id="848e1-124">az webapp deployment source config</span></span>](https://docs.microsoft.com/cli/azure/webapp/deployment/source#config) | <span data-ttu-id="848e1-125">Associerar en Azure-app med en Git eller ett lagringsplats.</span><span class="sxs-lookup"><span data-stu-id="848e1-125">Associates an Azure web app with a Git or Mercurial repository.</span></span> |
| [<span data-ttu-id="848e1-126">AZ webapp Bläddra</span><span class="sxs-lookup"><span data-stu-id="848e1-126">az webapp browse</span></span>](https://docs.microsoft.com/cli/azure/webapp#browse) | <span data-ttu-id="848e1-127">Öppna en Azure-app i en webbläsare.</span><span class="sxs-lookup"><span data-stu-id="848e1-127">Open an Azure web app in a browser.</span></span> |

## <a name="next-steps"></a><span data-ttu-id="848e1-128">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="848e1-128">Next steps</span></span>

<span data-ttu-id="848e1-129">Mer information om Azure CLI finns [Azure CLI dokumentationen](https://docs.microsoft.com/cli/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="848e1-129">For more information on the Azure CLI, see [Azure CLI documentation](https://docs.microsoft.com/cli/azure/overview).</span></span>

<span data-ttu-id="848e1-130">Ytterligare App Service CLI skriptexempel finns i den [dokumentation för Azure App Service](../app-service-cli-samples.md).</span><span class="sxs-lookup"><span data-stu-id="848e1-130">Additional App Service CLI script samples can be found in the [Azure App Service documentation](../app-service-cli-samples.md).</span></span>
