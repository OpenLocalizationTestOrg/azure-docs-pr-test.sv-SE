---
title: "aaaCreate en Funktionsapp och distribuera Funktionskoden från Visual Studio Team Services | Microsoft Docs"
description: "Skapa en Funktionsapp och distribuera Funktionskoden från Visual Studio Team Services"
services: functions
keywords: 
author: syntaxc4
ms.author: cfowler
ms.date: 04/28/2017
ms.topic: sample
ms.service: functions
ms.custom: mvc
ms.openlocfilehash: 774bee73025cc9ac46f8b2a6c10edbfa3c2d353b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-an-app-service"></a><span data-ttu-id="dd185-103">Skapa en App Service</span><span class="sxs-lookup"><span data-stu-id="dd185-103">Create an App Service</span></span>

<span data-ttu-id="dd185-104">I det här scenariot får du lära dig hur toocreate en funktion appen med hello [förbrukning plan](../functions-scale.md#consumption-plan) med dess relaterade resurser och kontinuerligt distribuerar Funktionskoden från en Visual Studio Team Services VSTS ()-databas.</span><span class="sxs-lookup"><span data-stu-id="dd185-104">In this scenario you will learn how toocreate a function app using hello [consumption plan](../functions-scale.md#consumption-plan) with its related resources, and continuously deploys your function code from a Visual Studio Team Services (VSTS) repository.</span></span> <span data-ttu-id="dd185-105">I det här exemplet behöver du:</span><span class="sxs-lookup"><span data-stu-id="dd185-105">In this sample, you will need:</span></span>

* <span data-ttu-id="dd185-106">En VSTS databas med funktioner kod som du har administrativa behörigheter för.</span><span class="sxs-lookup"><span data-stu-id="dd185-106">A VSTS repository with functions code, that you have administrative permissions for.</span></span>
* <span data-ttu-id="dd185-107">En [personlig åtkomst-Token (PATRIK)](https://help.github.com/articles/creating-an-access-token-for-command-line-use) för ditt GitHub-konto.</span><span class="sxs-lookup"><span data-stu-id="dd185-107">A [Personal Access Token (PAT)](https://help.github.com/articles/creating-an-access-token-for-command-line-use) for your GitHub account.</span></span>

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

<span data-ttu-id="dd185-108">Om du väljer tooinstall och använda hello CLI lokalt kräver i det här avsnittet att du kör hello Azure CLI version 2.0 eller senare.</span><span class="sxs-lookup"><span data-stu-id="dd185-108">If you choose tooinstall and use hello CLI locally, this topic requires that you are running hello Azure CLI version 2.0 or later.</span></span> <span data-ttu-id="dd185-109">Kör `az --version` toofind hello version.</span><span class="sxs-lookup"><span data-stu-id="dd185-109">Run `az --version` toofind hello version.</span></span> <span data-ttu-id="dd185-110">Om du behöver tooinstall eller uppgradering, se [installera Azure CLI 2.0]( /cli/azure/install-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="dd185-110">If you need tooinstall or upgrade, see [Install Azure CLI 2.0]( /cli/azure/install-azure-cli).</span></span> 

## <a name="sample-script"></a><span data-ttu-id="dd185-111">Exempelskript</span><span class="sxs-lookup"><span data-stu-id="dd185-111">Sample script</span></span>

<span data-ttu-id="dd185-112">Det här exemplet skapar en app i Azure-funktion och distribuerar Funktionskoden från Visual Studio Team Services.</span><span class="sxs-lookup"><span data-stu-id="dd185-112">This sample creates an Azure Function app and deploys function code from Visual Studio Team Services.</span></span>

[!code-azurecli-interactive[main](../../../cli_scripts/azure-functions/deploy-function-app-with-function-vsts/deploy-function-app-with-function-vsts.sh?highlight=3-4 "Azure Service")]

[!INCLUDE [cli-script-clean-up](../../../includes/cli-script-clean-up.md)]

## <a name="script-explanation"></a><span data-ttu-id="dd185-113">Skriptet förklaring</span><span class="sxs-lookup"><span data-stu-id="dd185-113">Script explanation</span></span>

<span data-ttu-id="dd185-114">Det här skriptet använder hello följande kommandon toocreate en resursgrupp, webbprogram, documentdb och alla relaterade resurser.</span><span class="sxs-lookup"><span data-stu-id="dd185-114">This script uses hello following commands toocreate a resource group, web app, documentdb and all related resources.</span></span> <span data-ttu-id="dd185-115">Varje kommando i hello tabellen länkar toocommand viss dokumentation.</span><span class="sxs-lookup"><span data-stu-id="dd185-115">Each command in hello table links toocommand specific documentation.</span></span>

| <span data-ttu-id="dd185-116">Kommando</span><span class="sxs-lookup"><span data-stu-id="dd185-116">Command</span></span> | <span data-ttu-id="dd185-117">Anteckningar</span><span class="sxs-lookup"><span data-stu-id="dd185-117">Notes</span></span> |
|---|---|
| [<span data-ttu-id="dd185-118">Skapa AZ grupp</span><span class="sxs-lookup"><span data-stu-id="dd185-118">az group create</span></span>](https://docs.microsoft.com/cli/azure/group#create) | <span data-ttu-id="dd185-119">Skapar en resursgrupp som är lagrade i alla resurser.</span><span class="sxs-lookup"><span data-stu-id="dd185-119">Creates a resource group in which all resources are stored.</span></span> |
| [<span data-ttu-id="dd185-120">Skapa AZ storage-konto</span><span class="sxs-lookup"><span data-stu-id="dd185-120">az storage account create</span></span>](https://docs.microsoft.com/cli/azure/appservice/plan#create) | <span data-ttu-id="dd185-121">Skapar en App Service-plan.</span><span class="sxs-lookup"><span data-stu-id="dd185-121">Creates an App Service plan.</span></span> |
| [<span data-ttu-id="dd185-122">Skapa AZ functionapp</span><span class="sxs-lookup"><span data-stu-id="dd185-122">az functionapp create</span></span>](https://docs.microsoft.com/cli/azure/appservice/web#delete) |
| [<span data-ttu-id="dd185-123">AZ apptjänst Webbkonfiguration källkontroll</span><span class="sxs-lookup"><span data-stu-id="dd185-123">az appservice web source-control config</span></span>](https://docs.microsoft.com/cli/azure/appservice/web/source-control#config) | <span data-ttu-id="dd185-124">Associerar en funktionsapp med en Git eller ett.</span><span class="sxs-lookup"><span data-stu-id="dd185-124">Associates a function app with a Git or Mercurial repository.</span></span> |

## <a name="next-steps"></a><span data-ttu-id="dd185-125">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="dd185-125">Next steps</span></span>

<span data-ttu-id="dd185-126">Mer information om hello Azure CLI finns [Azure CLI dokumentationen](https://docs.microsoft.com/cli/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="dd185-126">For more information on hello Azure CLI, see [Azure CLI documentation](https://docs.microsoft.com/cli/azure/overview).</span></span>

<span data-ttu-id="dd185-127">Ytterligare Azure Functions CLI skriptexempel finns i hello [Azure Functions dokumentationen](../functions-cli-samples.md).</span><span class="sxs-lookup"><span data-stu-id="dd185-127">Additional Azure Functions CLI script samples can be found in hello [Azure Functions documentation](../functions-cli-samples.md).</span></span>
