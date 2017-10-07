---
title: "aaaCreate en Funktionsapp och distribuera Funktionskoden från GitHub | Microsoft Docs"
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
ms.openlocfilehash: 4d44204b899b32af464260db51ed98bcf00eb2bc
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-an-app-service"></a><span data-ttu-id="93cea-103">Skapa en App Service</span><span class="sxs-lookup"><span data-stu-id="93cea-103">Create an App Service</span></span>

<span data-ttu-id="93cea-104">Det här exempelskriptet skapar en funktionsapp med hello [förbrukning plan](../functions-scale.md#consumption-plan) med dess relaterade resurser och kontinuerligt distribuerar Funktionskoden från en GitHub-databas.</span><span class="sxs-lookup"><span data-stu-id="93cea-104">This sample script creates a function app using hello [consumption plan](../functions-scale.md#consumption-plan) with its related resources, and continuously deploys your function code from a GitHub repository.</span></span> <span data-ttu-id="93cea-105">I det här exemplet behöver du:</span><span class="sxs-lookup"><span data-stu-id="93cea-105">In this sample, you need:</span></span>

* <span data-ttu-id="93cea-106">En GitHub-databas med funktioner kod som du har administrativa behörigheter för.</span><span class="sxs-lookup"><span data-stu-id="93cea-106">A GitHub repository with functions code, that you have administrative permissions for.</span></span>
* <span data-ttu-id="93cea-107">En [personlig åtkomst-Token (PATRIK)](https://help.github.com/articles/creating-an-access-token-for-command-line-use) för ditt GitHub-konto.</span><span class="sxs-lookup"><span data-stu-id="93cea-107">A [Personal Access Token (PAT)](https://help.github.com/articles/creating-an-access-token-for-command-line-use) for your GitHub account.</span></span>

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

<span data-ttu-id="93cea-108">Om du väljer tooinstall och använda hello CLI lokalt kräver i det här avsnittet att du kör hello Azure CLI version 2.0 eller senare.</span><span class="sxs-lookup"><span data-stu-id="93cea-108">If you choose tooinstall and use hello CLI locally, this topic requires that you are running hello Azure CLI version 2.0 or later.</span></span> <span data-ttu-id="93cea-109">Kör `az --version` toofind hello version.</span><span class="sxs-lookup"><span data-stu-id="93cea-109">Run `az --version` toofind hello version.</span></span> <span data-ttu-id="93cea-110">Om du behöver tooinstall eller uppgradering, se [installera Azure CLI 2.0]( /cli/azure/install-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="93cea-110">If you need tooinstall or upgrade, see [Install Azure CLI 2.0]( /cli/azure/install-azure-cli).</span></span> 

## <a name="sample-script"></a><span data-ttu-id="93cea-111">Exempelskript</span><span class="sxs-lookup"><span data-stu-id="93cea-111">Sample script</span></span>

<span data-ttu-id="93cea-112">Det här exemplet skapar en app i Azure-funktion och distribuerar Funktionskoden från GitHub.</span><span class="sxs-lookup"><span data-stu-id="93cea-112">This sample creates an Azure Function app and deploys function code from GitHub.</span></span>

[!code-azurecli-interactive[main](../../../cli_scripts/azure-functions/deploy-function-app-with-function-github-continuous/deploy-function-app-with-function-github-continuous.sh?highlight=3-4 "Azure Service")]

[!INCLUDE [cli-script-clean-up](../../../includes/cli-script-clean-up.md)]

## <a name="script-explanation"></a><span data-ttu-id="93cea-113">Skriptet förklaring</span><span class="sxs-lookup"><span data-stu-id="93cea-113">Script explanation</span></span>

<span data-ttu-id="93cea-114">Varje kommando i hello tabellen länkar toocommand viss dokumentation.</span><span class="sxs-lookup"><span data-stu-id="93cea-114">Each command in hello table links toocommand specific documentation.</span></span> <span data-ttu-id="93cea-115">Det här skriptet använder hello följande:</span><span class="sxs-lookup"><span data-stu-id="93cea-115">This script uses hello following:</span></span>

| <span data-ttu-id="93cea-116">Kommando</span><span class="sxs-lookup"><span data-stu-id="93cea-116">Command</span></span> | <span data-ttu-id="93cea-117">Anteckningar</span><span class="sxs-lookup"><span data-stu-id="93cea-117">Notes</span></span> |
|---|---|
| [<span data-ttu-id="93cea-118">Skapa AZ grupp</span><span class="sxs-lookup"><span data-stu-id="93cea-118">az group create</span></span>](https://docs.microsoft.com/cli/azure/group#create) | <span data-ttu-id="93cea-119">Skapar en resursgrupp som är lagrade i alla resurser.</span><span class="sxs-lookup"><span data-stu-id="93cea-119">Creates a resource group in which all resources are stored.</span></span> |
| [<span data-ttu-id="93cea-120">Skapa AZ storage-konto</span><span class="sxs-lookup"><span data-stu-id="93cea-120">az storage account create</span></span>](https://docs.microsoft.com/cli/azure/appservice/plan#create) | <span data-ttu-id="93cea-121">Skapar en App Service-plan.</span><span class="sxs-lookup"><span data-stu-id="93cea-121">Creates an App Service plan.</span></span> |
| [<span data-ttu-id="93cea-122">Skapa AZ functionapp</span><span class="sxs-lookup"><span data-stu-id="93cea-122">az functionapp create</span></span>](https://docs.microsoft.com/cli/azure/appservice/web#delete) |
| [<span data-ttu-id="93cea-123">AZ apptjänst Webbkonfiguration källkontroll</span><span class="sxs-lookup"><span data-stu-id="93cea-123">az appservice web source-control config</span></span>](https://docs.microsoft.com/cli/azure/appservice/web/source-control#config) | <span data-ttu-id="93cea-124">Associerar en funktionsapp med en Git eller ett.</span><span class="sxs-lookup"><span data-stu-id="93cea-124">Associates a function app with a Git or Mercurial repository.</span></span> |

## <a name="next-steps"></a><span data-ttu-id="93cea-125">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="93cea-125">Next steps</span></span>

<span data-ttu-id="93cea-126">Mer information om hello Azure CLI finns [Azure CLI dokumentationen](https://docs.microsoft.com/cli/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="93cea-126">For more information on hello Azure CLI, see [Azure CLI documentation](https://docs.microsoft.com/cli/azure/overview).</span></span>

<span data-ttu-id="93cea-127">Ytterligare Azure Functions CLI skriptexempel finns i hello [Azure Functions dokumentationen](../functions-cli-samples.md).</span><span class="sxs-lookup"><span data-stu-id="93cea-127">Additional Azure Functions CLI script samples can be found in hello [Azure Functions documentation](../functions-cli-samples.md).</span></span>
