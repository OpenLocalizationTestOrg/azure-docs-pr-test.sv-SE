---
title: "Skapa en Funktionsapp och distribuera Funktionskoden från GitHub | Microsoft Docs"
description: "Azure CLI skriptexempel - skapa en Funktionsapp och distribuera Funktionskoden från GitHub"
services: functions
keywords: 
author: syntaxc4
ms.author: cfowler
ms.date: 04/27/2017
ms.topic: sample
ms.service: functions
ms.custom: mvc
ms.openlocfilehash: d67e85f91c80efe464fceb1105243bedfba83a0f
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="create-a-function-app-and-deploy-function-code-from-github"></a><span data-ttu-id="4b86c-103">Skapa en funktionsapp och distribuera Funktionskoden från GitHub</span><span class="sxs-lookup"><span data-stu-id="4b86c-103">Create a function app and deploy function code from GitHub</span></span>

<span data-ttu-id="4b86c-104">Det här exempelskriptet skapar en funktion app med hjälp av den [förbrukning plan](../functions-scale.md#consumption-plan) med dess relaterade resurser, och distribuerar Funktionskoden från en offentlig GitHub-databas (utan kontinuerlig distribution).</span><span class="sxs-lookup"><span data-stu-id="4b86c-104">This sample script creates a function app using the [consumption plan](../functions-scale.md#consumption-plan) with its related resources, and deploys your function code from a public GitHub repository (without continuous deployment).</span></span> <span data-ttu-id="4b86c-105">Kontinuerlig leverans av Funktionskoden från GitHub läsa [skapa en funktionsapp och distribuera kontinuerligt från GitHub](functions-cli-create-function-app-github-continuous.md)</span><span class="sxs-lookup"><span data-stu-id="4b86c-105">For continuous delivery of function code from GitHub, read [Create a function app and continuously deploy from GitHub](functions-cli-create-function-app-github-continuous.md)</span></span>

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

<span data-ttu-id="4b86c-106">Om du väljer att installera och använda CLI lokalt måste du köra Azure CLI version 2.0 eller senare.</span><span class="sxs-lookup"><span data-stu-id="4b86c-106">If you choose to install and use the CLI locally, this topic requires that you are running the Azure CLI version 2.0 or later.</span></span> <span data-ttu-id="4b86c-107">Kör `az --version` för att hitta versionen.</span><span class="sxs-lookup"><span data-stu-id="4b86c-107">Run `az --version` to find the version.</span></span> <span data-ttu-id="4b86c-108">Om du behöver installera eller uppgradera kan du läsa [Installera Azure CLI 2.0]( /cli/azure/install-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="4b86c-108">If you need to install or upgrade, see [Install Azure CLI 2.0]( /cli/azure/install-azure-cli).</span></span> 

## <a name="sample-script"></a><span data-ttu-id="4b86c-109">Exempelskript</span><span class="sxs-lookup"><span data-stu-id="4b86c-109">Sample script</span></span>

<span data-ttu-id="4b86c-110">Det här exemplet skapar en app i Azure-funktion och distribuerar Funktionskoden från GitHub.</span><span class="sxs-lookup"><span data-stu-id="4b86c-110">This sample creates an Azure Function app and deploys function code from GitHub.</span></span>

<span data-ttu-id="4b86c-111">[!code-azurecli-interactive[huvudsakliga](../../../cli_scripts/azure-functions/deploy-function-app-with-function-github/deploy-function-app-with-function-github.sh?highlight=3 "skapa en funktionsapp med distribution från GitHub")]</span><span class="sxs-lookup"><span data-stu-id="4b86c-111">[!code-azurecli-interactive[main](../../../cli_scripts/azure-functions/deploy-function-app-with-function-github/deploy-function-app-with-function-github.sh?highlight=3 "Create a function app with deployment from GitHub")]</span></span>

[!INCLUDE [cli-script-clean-up](../../../includes/cli-script-clean-up.md)]

## <a name="script-explanation"></a><span data-ttu-id="4b86c-112">Skriptet förklaring</span><span class="sxs-lookup"><span data-stu-id="4b86c-112">Script explanation</span></span>

<span data-ttu-id="4b86c-113">Varje kommando i tabellen länkar till kommandot viss dokumentation.</span><span class="sxs-lookup"><span data-stu-id="4b86c-113">Each command in the table links to command specific documentation.</span></span> <span data-ttu-id="4b86c-114">Det här skriptet använder följande kommandon:</span><span class="sxs-lookup"><span data-stu-id="4b86c-114">This script uses the following commands:</span></span>

| <span data-ttu-id="4b86c-115">Kommando</span><span class="sxs-lookup"><span data-stu-id="4b86c-115">Command</span></span> | <span data-ttu-id="4b86c-116">Anteckningar</span><span class="sxs-lookup"><span data-stu-id="4b86c-116">Notes</span></span> |
|---|---|
| [<span data-ttu-id="4b86c-117">Skapa AZ grupp</span><span class="sxs-lookup"><span data-stu-id="4b86c-117">az group create</span></span>](https://docs.microsoft.com/cli/azure/group#create) | <span data-ttu-id="4b86c-118">Skapar en resursgrupp som är lagrade i alla resurser.</span><span class="sxs-lookup"><span data-stu-id="4b86c-118">Creates a resource group in which all resources are stored.</span></span> |
| [<span data-ttu-id="4b86c-119">Skapa AZ storage-konto</span><span class="sxs-lookup"><span data-stu-id="4b86c-119">az storage account create</span></span>](https://docs.microsoft.com/cli/azure/appservice/plan#create) | <span data-ttu-id="4b86c-120">Skapar en App Service-plan.</span><span class="sxs-lookup"><span data-stu-id="4b86c-120">Creates an App Service plan.</span></span> |
| [<span data-ttu-id="4b86c-121">Skapa AZ functionapp</span><span class="sxs-lookup"><span data-stu-id="4b86c-121">az functionapp create</span></span>](https://docs.microsoft.com/cli/azure/appservice/web#delete) | <span data-ttu-id="4b86c-122">Skapar en funktionsapp i Azure.</span><span class="sxs-lookup"><span data-stu-id="4b86c-122">Creates an Azure Function app.</span></span> |
| [<span data-ttu-id="4b86c-123">AZ apptjänst Webbkonfiguration källkontroll</span><span class="sxs-lookup"><span data-stu-id="4b86c-123">az appservice web source-control config</span></span>](https://docs.microsoft.com/cli/azure/appservice/web/source-control#config) | <span data-ttu-id="4b86c-124">Associerar en funktionsapp med en Git eller ett.</span><span class="sxs-lookup"><span data-stu-id="4b86c-124">Associates a function app with a Git or Mercurial repository.</span></span> |

## <a name="next-steps"></a><span data-ttu-id="4b86c-125">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="4b86c-125">Next steps</span></span>

<span data-ttu-id="4b86c-126">Mer information om Azure CLI finns [Azure CLI dokumentationen](https://docs.microsoft.com/cli/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="4b86c-126">For more information on the Azure CLI, see [Azure CLI documentation](https://docs.microsoft.com/cli/azure/overview).</span></span>

<span data-ttu-id="4b86c-127">Ytterligare Azure Functions CLI skriptexempel finns i den [Azure Functions dokumentationen](../functions-cli-samples.md).</span><span class="sxs-lookup"><span data-stu-id="4b86c-127">Additional Azure Functions CLI script samples can be found in the [Azure Functions documentation](../functions-cli-samples.md).</span></span>
