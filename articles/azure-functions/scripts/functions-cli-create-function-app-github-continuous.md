---
title: "Skapa en Funktionsapp och distribuera Funktionskoden från GitHub | Microsoft Docs"
description: "Skapa en Funktionsapp och distribuera Funktionskoden från GitHub"
services: functions
ms.service: functions
keywords: 
ms.devlang: azurecli
author: syntaxc4
ms.author: cfowler
ms.date: 04/27/2017
ms.topic: sample
ms.custom: mvc
ms.openlocfilehash: 67eb41d89328ab57741c419d8b718e19b947dab1
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/29/2017
---
# <a name="create-an-app-service"></a><span data-ttu-id="a57d8-103">Skapa en App Service</span><span class="sxs-lookup"><span data-stu-id="a57d8-103">Create an App Service</span></span>

<span data-ttu-id="a57d8-104">Det här exempelskriptet skapar en funktion app med hjälp av den [förbrukning plan](../functions-scale.md#consumption-plan) med dess relaterade resurser och kontinuerligt distribuerar Funktionskoden från en GitHub-databas.</span><span class="sxs-lookup"><span data-stu-id="a57d8-104">This sample script creates a function app using the [consumption plan](../functions-scale.md#consumption-plan) with its related resources, and continuously deploys your function code from a GitHub repository.</span></span> <span data-ttu-id="a57d8-105">I det här exemplet behöver du:</span><span class="sxs-lookup"><span data-stu-id="a57d8-105">In this sample, you need:</span></span>

* <span data-ttu-id="a57d8-106">En GitHub-databas med funktioner kod som du har administrativa behörigheter för.</span><span class="sxs-lookup"><span data-stu-id="a57d8-106">A GitHub repository with functions code, that you have administrative permissions for.</span></span>
* <span data-ttu-id="a57d8-107">En [personlig åtkomst-Token (PATRIK)](https://help.github.com/articles/creating-an-access-token-for-command-line-use) för ditt GitHub-konto.</span><span class="sxs-lookup"><span data-stu-id="a57d8-107">A [Personal Access Token (PAT)](https://help.github.com/articles/creating-an-access-token-for-command-line-use) for your GitHub account.</span></span>

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

<span data-ttu-id="a57d8-108">Om du väljer att installera och använda CLI lokalt måste du köra Azure CLI version 2.0 eller senare.</span><span class="sxs-lookup"><span data-stu-id="a57d8-108">If you choose to install and use the CLI locally, this topic requires that you are running the Azure CLI version 2.0 or later.</span></span> <span data-ttu-id="a57d8-109">Kör `az --version` för att hitta versionen.</span><span class="sxs-lookup"><span data-stu-id="a57d8-109">Run `az --version` to find the version.</span></span> <span data-ttu-id="a57d8-110">Om du behöver installera eller uppgradera kan du läsa [Installera Azure CLI 2.0]( /cli/azure/install-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="a57d8-110">If you need to install or upgrade, see [Install Azure CLI 2.0]( /cli/azure/install-azure-cli).</span></span> 

## <a name="sample-script"></a><span data-ttu-id="a57d8-111">Exempelskript</span><span class="sxs-lookup"><span data-stu-id="a57d8-111">Sample script</span></span>

<span data-ttu-id="a57d8-112">Det här exemplet skapar en app i Azure-funktion och distribuerar Funktionskoden från GitHub.</span><span class="sxs-lookup"><span data-stu-id="a57d8-112">This sample creates an Azure Function app and deploys function code from GitHub.</span></span>

<span data-ttu-id="a57d8-113">[!code-azurecli-interactive[huvudsakliga](../../../cli_scripts/azure-functions/deploy-function-app-with-function-github-continuous/deploy-function-app-with-function-github-continuous.sh?highlight=3-4 "Azure Service")]</span><span class="sxs-lookup"><span data-stu-id="a57d8-113">[!code-azurecli-interactive[main](../../../cli_scripts/azure-functions/deploy-function-app-with-function-github-continuous/deploy-function-app-with-function-github-continuous.sh?highlight=3-4 "Azure Service")]</span></span>

[!INCLUDE [cli-script-clean-up](../../../includes/cli-script-clean-up.md)]

## <a name="script-explanation"></a><span data-ttu-id="a57d8-114">Skriptet förklaring</span><span class="sxs-lookup"><span data-stu-id="a57d8-114">Script explanation</span></span>

<span data-ttu-id="a57d8-115">Varje kommando i tabellen länkar till kommandot viss dokumentation.</span><span class="sxs-lookup"><span data-stu-id="a57d8-115">Each command in the table links to command specific documentation.</span></span> <span data-ttu-id="a57d8-116">Det här skriptet använder följande:</span><span class="sxs-lookup"><span data-stu-id="a57d8-116">This script uses the following:</span></span>

| <span data-ttu-id="a57d8-117">Kommando</span><span class="sxs-lookup"><span data-stu-id="a57d8-117">Command</span></span> | <span data-ttu-id="a57d8-118">Anteckningar</span><span class="sxs-lookup"><span data-stu-id="a57d8-118">Notes</span></span> |
|---|---|
| [<span data-ttu-id="a57d8-119">Skapa AZ grupp</span><span class="sxs-lookup"><span data-stu-id="a57d8-119">az group create</span></span>](https://docs.microsoft.com/cli/azure/group#create) | <span data-ttu-id="a57d8-120">Skapar en resursgrupp som är lagrade i alla resurser.</span><span class="sxs-lookup"><span data-stu-id="a57d8-120">Creates a resource group in which all resources are stored.</span></span> |
| [<span data-ttu-id="a57d8-121">Skapa AZ storage-konto</span><span class="sxs-lookup"><span data-stu-id="a57d8-121">az storage account create</span></span>](https://docs.microsoft.com/cli/azure/appservice/plan#create) | <span data-ttu-id="a57d8-122">Skapar en App Service-plan.</span><span class="sxs-lookup"><span data-stu-id="a57d8-122">Creates an App Service plan.</span></span> |
| [<span data-ttu-id="a57d8-123">Skapa AZ functionapp</span><span class="sxs-lookup"><span data-stu-id="a57d8-123">az functionapp create</span></span>](https://docs.microsoft.com/cli/azure/appservice/web#delete) |
| [<span data-ttu-id="a57d8-124">AZ apptjänst Webbkonfiguration källkontroll</span><span class="sxs-lookup"><span data-stu-id="a57d8-124">az appservice web source-control config</span></span>](https://docs.microsoft.com/cli/azure/appservice/web/source-control#config) | <span data-ttu-id="a57d8-125">Associerar en funktionsapp med en Git eller ett.</span><span class="sxs-lookup"><span data-stu-id="a57d8-125">Associates a function app with a Git or Mercurial repository.</span></span> |

## <a name="next-steps"></a><span data-ttu-id="a57d8-126">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="a57d8-126">Next steps</span></span>

<span data-ttu-id="a57d8-127">Mer information om Azure CLI finns [Azure CLI dokumentationen](https://docs.microsoft.com/cli/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="a57d8-127">For more information on the Azure CLI, see [Azure CLI documentation](https://docs.microsoft.com/cli/azure/overview).</span></span>

<span data-ttu-id="a57d8-128">Ytterligare Azure Functions CLI skriptexempel finns i den [Azure Functions dokumentationen](../functions-cli-samples.md).</span><span class="sxs-lookup"><span data-stu-id="a57d8-128">Additional Azure Functions CLI script samples can be found in the [Azure Functions documentation](../functions-cli-samples.md).</span></span>
