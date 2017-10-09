---
title: "aaaAzure CLI skriptexempel - skapa en webbapp med kontinuerlig distribution från Visual Studio Team Services | Microsoft Docs"
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
ms.openlocfilehash: f8d0c2645ec5311296ca9b2df20f97e77bab2283
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-web-app-with-continuous-deployment-from-visual-studio-team-services"></a><span data-ttu-id="bc07f-103">Skapa en webbapp med kontinuerlig distribution från Visual Studio Team Services</span><span class="sxs-lookup"><span data-stu-id="bc07f-103">Create a web app with continuous deployment from Visual Studio Team Services</span></span>

<span data-ttu-id="bc07f-104">Det här exempelskriptet skapar en webbapp i App Service med dess relaterade resurser och ställer sedan in kontinuerlig distribution från Visual Studio Team Services-databasen.</span><span class="sxs-lookup"><span data-stu-id="bc07f-104">This sample script creates a web app in App Service with its related resources, and then sets up continuous deployment from a Visual Studio Team Services repository.</span></span> <span data-ttu-id="bc07f-105">För det här exemplet behöver du:</span><span class="sxs-lookup"><span data-stu-id="bc07f-105">For this sample, you will need:</span></span>

* <span data-ttu-id="bc07f-106">Ett Visual Studio Team Services-databas med programkod som du har administrativa behörigheter för.</span><span class="sxs-lookup"><span data-stu-id="bc07f-106">A Visual Studio Team Services repository with application code, that you have administrative permissions for.</span></span>
* <span data-ttu-id="bc07f-107">En [personlig åtkomst-Token (PATRIK)](https://www.visualstudio.com/docs/setup-admin/team-services/use-personal-access-tokens-to-authenticate) för Visual Studio Team Services-konto.</span><span class="sxs-lookup"><span data-stu-id="bc07f-107">A [Personal Access Token (PAT)](https://www.visualstudio.com/docs/setup-admin/team-services/use-personal-access-tokens-to-authenticate) for your Visual Studio Team Services account.</span></span>

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]


[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

<span data-ttu-id="bc07f-108">Om du väljer tooinstall och använda hello CLI lokalt kräver i det här avsnittet att du kör hello Azure CLI version 2.0 eller senare.</span><span class="sxs-lookup"><span data-stu-id="bc07f-108">If you choose tooinstall and use hello CLI locally, this topic requires that you are running hello Azure CLI version 2.0 or later.</span></span> <span data-ttu-id="bc07f-109">Kör `az --version` toofind hello version.</span><span class="sxs-lookup"><span data-stu-id="bc07f-109">Run `az --version` toofind hello version.</span></span> <span data-ttu-id="bc07f-110">Om du behöver tooinstall eller uppgradering, se [installera Azure CLI 2.0]( /cli/azure/install-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="bc07f-110">If you need tooinstall or upgrade, see [Install Azure CLI 2.0]( /cli/azure/install-azure-cli).</span></span> 

## <a name="sample-script"></a><span data-ttu-id="bc07f-111">Exempelskript</span><span class="sxs-lookup"><span data-stu-id="bc07f-111">Sample script</span></span>

[!code-azurecli-interactive[main](../../../cli_scripts/app-service/deploy-vsts-continuous/deploy-vsts-continuous.sh?highlight=3-4 "Create a web app with continuous deployment from Visual Studio Team Services")]

[!INCLUDE [cli-script-clean-up](../../../includes/cli-script-clean-up.md)]

## <a name="script-explanation"></a><span data-ttu-id="bc07f-112">Skriptet förklaring</span><span class="sxs-lookup"><span data-stu-id="bc07f-112">Script explanation</span></span>

<span data-ttu-id="bc07f-113">Det här skriptet använder hello följande kommandon.</span><span class="sxs-lookup"><span data-stu-id="bc07f-113">This script uses hello following commands.</span></span> <span data-ttu-id="bc07f-114">Varje kommando i hello tabellen länkar toocommand viss dokumentation.</span><span class="sxs-lookup"><span data-stu-id="bc07f-114">Each command in hello table links toocommand specific documentation.</span></span>

| <span data-ttu-id="bc07f-115">Kommando</span><span class="sxs-lookup"><span data-stu-id="bc07f-115">Command</span></span> | <span data-ttu-id="bc07f-116">Anteckningar</span><span class="sxs-lookup"><span data-stu-id="bc07f-116">Notes</span></span> |
|---|---|
| [<span data-ttu-id="bc07f-117">Skapa AZ grupp</span><span class="sxs-lookup"><span data-stu-id="bc07f-117">az group create</span></span>](https://docs.microsoft.com/cli/azure/group#create) | <span data-ttu-id="bc07f-118">Skapar en resursgrupp som är lagrade i alla resurser.</span><span class="sxs-lookup"><span data-stu-id="bc07f-118">Creates a resource group in which all resources are stored.</span></span> |
| [<span data-ttu-id="bc07f-119">Skapa AZ programtjänstplan</span><span class="sxs-lookup"><span data-stu-id="bc07f-119">az appservice plan create</span></span>](https://docs.microsoft.com/cli/azure/appservice/plan#create) | <span data-ttu-id="bc07f-120">Skapar en App Service-plan.</span><span class="sxs-lookup"><span data-stu-id="bc07f-120">Creates an App Service plan.</span></span> |
| [<span data-ttu-id="bc07f-121">Skapa AZ webapp</span><span class="sxs-lookup"><span data-stu-id="bc07f-121">az webapp create</span></span>](https://docs.microsoft.com/cli/azure/webapp#create) | <span data-ttu-id="bc07f-122">Skapar ett Azure-webbapp.</span><span class="sxs-lookup"><span data-stu-id="bc07f-122">Creates an Azure web app.</span></span> |
| [<span data-ttu-id="bc07f-123">AZ webapp distributionskonfiguration källa</span><span class="sxs-lookup"><span data-stu-id="bc07f-123">az webapp deployment source config</span></span>](https://docs.microsoft.com/cli/azure/webapp/deployment/source#config) | <span data-ttu-id="bc07f-124">Associerar en Azure-app med en Git eller ett lagringsplats.</span><span class="sxs-lookup"><span data-stu-id="bc07f-124">Associates an Azure web app with a Git or Mercurial repository.</span></span> |
| [<span data-ttu-id="bc07f-125">AZ webapp Bläddra</span><span class="sxs-lookup"><span data-stu-id="bc07f-125">az webapp browse</span></span>](https://docs.microsoft.com/cli/azure/webapp#browse) | <span data-ttu-id="bc07f-126">Öppna en Azure-app i en webbläsare.</span><span class="sxs-lookup"><span data-stu-id="bc07f-126">Open an Azure web app in a browser.</span></span> |

## <a name="next-steps"></a><span data-ttu-id="bc07f-127">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="bc07f-127">Next steps</span></span>

<span data-ttu-id="bc07f-128">Mer information om hello Azure CLI finns [Azure CLI dokumentationen](https://docs.microsoft.com/cli/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="bc07f-128">For more information on hello Azure CLI, see [Azure CLI documentation](https://docs.microsoft.com/cli/azure/overview).</span></span>

<span data-ttu-id="bc07f-129">Ytterligare App Service CLI skriptexempel finns i hello [dokumentation för Azure App Service](../app-service-cli-samples.md).</span><span class="sxs-lookup"><span data-stu-id="bc07f-129">Additional App Service CLI script samples can be found in hello [Azure App Service documentation](../app-service-cli-samples.md).</span></span>
