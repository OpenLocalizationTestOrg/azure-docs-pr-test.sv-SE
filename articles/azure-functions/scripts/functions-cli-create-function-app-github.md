---
title: "aaaCreate en Funktionsapp och distribuera Funktionskoden från GitHub | Microsoft Docs"
description: "Azure CLI skriptexempel - skapa en Funktionsapp och distribuera Funktionskoden från GitHub"
services: functions
keywords: 
author: syntaxc4
ms.author: cfowler
ms.date: 04/27/2017
ms.topic: sample
ms.service: functions
ms.custom: mvc
ms.openlocfilehash: 026886f11909149db695d9a52d0aa37f109f64e4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-function-app-and-deploy-function-code-from-github"></a><span data-ttu-id="b5f64-103">Skapa en funktionsapp och distribuera Funktionskoden från GitHub</span><span class="sxs-lookup"><span data-stu-id="b5f64-103">Create a function app and deploy function code from GitHub</span></span>

<span data-ttu-id="b5f64-104">Det här exempelskriptet skapar en funktionsapp med hello [förbrukning plan](../functions-scale.md#consumption-plan) med dess relaterade resurser, och distribuerar Funktionskoden från en offentlig GitHub-databas (utan kontinuerlig distribution).</span><span class="sxs-lookup"><span data-stu-id="b5f64-104">This sample script creates a function app using hello [consumption plan](../functions-scale.md#consumption-plan) with its related resources, and deploys your function code from a public GitHub repository (without continuous deployment).</span></span> <span data-ttu-id="b5f64-105">Kontinuerlig leverans av Funktionskoden från GitHub läsa [skapa en funktionsapp och distribuera kontinuerligt från GitHub](functions-cli-create-function-app-github-continuous.md)</span><span class="sxs-lookup"><span data-stu-id="b5f64-105">For continuous delivery of function code from GitHub, read [Create a function app and continuously deploy from GitHub](functions-cli-create-function-app-github-continuous.md)</span></span>

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

<span data-ttu-id="b5f64-106">Om du väljer tooinstall och använda hello CLI lokalt kräver i det här avsnittet att du kör hello Azure CLI version 2.0 eller senare.</span><span class="sxs-lookup"><span data-stu-id="b5f64-106">If you choose tooinstall and use hello CLI locally, this topic requires that you are running hello Azure CLI version 2.0 or later.</span></span> <span data-ttu-id="b5f64-107">Kör `az --version` toofind hello version.</span><span class="sxs-lookup"><span data-stu-id="b5f64-107">Run `az --version` toofind hello version.</span></span> <span data-ttu-id="b5f64-108">Om du behöver tooinstall eller uppgradering, se [installera Azure CLI 2.0]( /cli/azure/install-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="b5f64-108">If you need tooinstall or upgrade, see [Install Azure CLI 2.0]( /cli/azure/install-azure-cli).</span></span> 

## <a name="sample-script"></a><span data-ttu-id="b5f64-109">Exempelskript</span><span class="sxs-lookup"><span data-stu-id="b5f64-109">Sample script</span></span>

<span data-ttu-id="b5f64-110">Det här exemplet skapar en app i Azure-funktion och distribuerar Funktionskoden från GitHub.</span><span class="sxs-lookup"><span data-stu-id="b5f64-110">This sample creates an Azure Function app and deploys function code from GitHub.</span></span>

[!code-azurecli-interactive[main](../../../cli_scripts/azure-functions/deploy-function-app-with-function-github/deploy-function-app-with-function-github.sh?highlight=3 "Create a function app with deployment from GitHub")]

[!INCLUDE [cli-script-clean-up](../../../includes/cli-script-clean-up.md)]

## <a name="script-explanation"></a><span data-ttu-id="b5f64-111">Skriptet förklaring</span><span class="sxs-lookup"><span data-stu-id="b5f64-111">Script explanation</span></span>

<span data-ttu-id="b5f64-112">Varje kommando i hello tabellen länkar toocommand viss dokumentation.</span><span class="sxs-lookup"><span data-stu-id="b5f64-112">Each command in hello table links toocommand specific documentation.</span></span> <span data-ttu-id="b5f64-113">Det här skriptet använder hello följande kommandon:</span><span class="sxs-lookup"><span data-stu-id="b5f64-113">This script uses hello following commands:</span></span>

| <span data-ttu-id="b5f64-114">Kommando</span><span class="sxs-lookup"><span data-stu-id="b5f64-114">Command</span></span> | <span data-ttu-id="b5f64-115">Anteckningar</span><span class="sxs-lookup"><span data-stu-id="b5f64-115">Notes</span></span> |
|---|---|
| [<span data-ttu-id="b5f64-116">Skapa AZ grupp</span><span class="sxs-lookup"><span data-stu-id="b5f64-116">az group create</span></span>](https://docs.microsoft.com/cli/azure/group#create) | <span data-ttu-id="b5f64-117">Skapar en resursgrupp som är lagrade i alla resurser.</span><span class="sxs-lookup"><span data-stu-id="b5f64-117">Creates a resource group in which all resources are stored.</span></span> |
| [<span data-ttu-id="b5f64-118">Skapa AZ storage-konto</span><span class="sxs-lookup"><span data-stu-id="b5f64-118">az storage account create</span></span>](https://docs.microsoft.com/cli/azure/appservice/plan#create) | <span data-ttu-id="b5f64-119">Skapar en App Service-plan.</span><span class="sxs-lookup"><span data-stu-id="b5f64-119">Creates an App Service plan.</span></span> |
| [<span data-ttu-id="b5f64-120">Skapa AZ functionapp</span><span class="sxs-lookup"><span data-stu-id="b5f64-120">az functionapp create</span></span>](https://docs.microsoft.com/cli/azure/appservice/web#delete) | <span data-ttu-id="b5f64-121">Skapar en funktionsapp i Azure.</span><span class="sxs-lookup"><span data-stu-id="b5f64-121">Creates an Azure Function app.</span></span> |
| [<span data-ttu-id="b5f64-122">AZ apptjänst Webbkonfiguration källkontroll</span><span class="sxs-lookup"><span data-stu-id="b5f64-122">az appservice web source-control config</span></span>](https://docs.microsoft.com/cli/azure/appservice/web/source-control#config) | <span data-ttu-id="b5f64-123">Associerar en funktionsapp med en Git eller ett.</span><span class="sxs-lookup"><span data-stu-id="b5f64-123">Associates a function app with a Git or Mercurial repository.</span></span> |

## <a name="next-steps"></a><span data-ttu-id="b5f64-124">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="b5f64-124">Next steps</span></span>

<span data-ttu-id="b5f64-125">Mer information om hello Azure CLI finns [Azure CLI dokumentationen](https://docs.microsoft.com/cli/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="b5f64-125">For more information on hello Azure CLI, see [Azure CLI documentation](https://docs.microsoft.com/cli/azure/overview).</span></span>

<span data-ttu-id="b5f64-126">Ytterligare Azure Functions CLI skriptexempel finns i hello [Azure Functions dokumentationen](../functions-cli-samples.md).</span><span class="sxs-lookup"><span data-stu-id="b5f64-126">Additional Azure Functions CLI script samples can be found in hello [Azure Functions documentation](../functions-cli-samples.md).</span></span>
