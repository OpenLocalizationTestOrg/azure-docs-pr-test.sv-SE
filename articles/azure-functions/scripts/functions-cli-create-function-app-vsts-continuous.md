---
title: "Skapa en Funktionsapp och distribuera Funktionskoden från Visual Studio Team Services | Microsoft Docs"
description: "Skapa en Funktionsapp och distribuera Funktionskoden från Visual Studio Team Services"
services: functions
keywords: 
author: syntaxc4
ms.author: cfowler
ms.date: 04/28/2017
ms.topic: sample
ms.service: functions
ms.custom: mvc
ms.openlocfilehash: 2ef177b55ad7ffd351156821f429e6ff8fbeccc7
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="create-an-app-service"></a><span data-ttu-id="cdc9d-103">Skapa en App Service</span><span class="sxs-lookup"><span data-stu-id="cdc9d-103">Create an App Service</span></span>

<span data-ttu-id="cdc9d-104">I det här scenariot du lära dig hur du skapar en funktion app med hjälp av den [förbrukning plan](../functions-scale.md#consumption-plan) med dess relaterade resurser och kontinuerligt distribuerar Funktionskoden från en Visual Studio Team Services VSTS ()-databas.</span><span class="sxs-lookup"><span data-stu-id="cdc9d-104">In this scenario you will learn how to create a function app using the [consumption plan](../functions-scale.md#consumption-plan) with its related resources, and continuously deploys your function code from a Visual Studio Team Services (VSTS) repository.</span></span> <span data-ttu-id="cdc9d-105">I det här exemplet behöver du:</span><span class="sxs-lookup"><span data-stu-id="cdc9d-105">In this sample, you will need:</span></span>

* <span data-ttu-id="cdc9d-106">En VSTS databas med funktioner kod som du har administrativa behörigheter för.</span><span class="sxs-lookup"><span data-stu-id="cdc9d-106">A VSTS repository with functions code, that you have administrative permissions for.</span></span>
* <span data-ttu-id="cdc9d-107">En [personlig åtkomst-Token (PATRIK)](https://help.github.com/articles/creating-an-access-token-for-command-line-use) för ditt GitHub-konto.</span><span class="sxs-lookup"><span data-stu-id="cdc9d-107">A [Personal Access Token (PAT)](https://help.github.com/articles/creating-an-access-token-for-command-line-use) for your GitHub account.</span></span>

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

<span data-ttu-id="cdc9d-108">Om du väljer att installera och använda CLI lokalt måste du köra Azure CLI version 2.0 eller senare.</span><span class="sxs-lookup"><span data-stu-id="cdc9d-108">If you choose to install and use the CLI locally, this topic requires that you are running the Azure CLI version 2.0 or later.</span></span> <span data-ttu-id="cdc9d-109">Kör `az --version` för att hitta versionen.</span><span class="sxs-lookup"><span data-stu-id="cdc9d-109">Run `az --version` to find the version.</span></span> <span data-ttu-id="cdc9d-110">Om du behöver installera eller uppgradera kan du läsa [Installera Azure CLI 2.0]( /cli/azure/install-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="cdc9d-110">If you need to install or upgrade, see [Install Azure CLI 2.0]( /cli/azure/install-azure-cli).</span></span> 

## <a name="sample-script"></a><span data-ttu-id="cdc9d-111">Exempelskript</span><span class="sxs-lookup"><span data-stu-id="cdc9d-111">Sample script</span></span>

<span data-ttu-id="cdc9d-112">Det här exemplet skapar en app i Azure-funktion och distribuerar Funktionskoden från Visual Studio Team Services.</span><span class="sxs-lookup"><span data-stu-id="cdc9d-112">This sample creates an Azure Function app and deploys function code from Visual Studio Team Services.</span></span>

<span data-ttu-id="cdc9d-113">[!code-azurecli-interactive[huvudsakliga](../../../cli_scripts/azure-functions/deploy-function-app-with-function-vsts/deploy-function-app-with-function-vsts.sh?highlight=3-4 "Azure Service")]</span><span class="sxs-lookup"><span data-stu-id="cdc9d-113">[!code-azurecli-interactive[main](../../../cli_scripts/azure-functions/deploy-function-app-with-function-vsts/deploy-function-app-with-function-vsts.sh?highlight=3-4 "Azure Service")]</span></span>

[!INCLUDE [cli-script-clean-up](../../../includes/cli-script-clean-up.md)]

## <a name="script-explanation"></a><span data-ttu-id="cdc9d-114">Skriptet förklaring</span><span class="sxs-lookup"><span data-stu-id="cdc9d-114">Script explanation</span></span>

<span data-ttu-id="cdc9d-115">Det här skriptet använder följande kommandon för att skapa en resursgrupp, webbprogram, documentdb och alla relaterade resurser.</span><span class="sxs-lookup"><span data-stu-id="cdc9d-115">This script uses the following commands to create a resource group, web app, documentdb and all related resources.</span></span> <span data-ttu-id="cdc9d-116">Varje kommando i tabellen länkar till kommandot viss dokumentation.</span><span class="sxs-lookup"><span data-stu-id="cdc9d-116">Each command in the table links to command specific documentation.</span></span>

| <span data-ttu-id="cdc9d-117">Kommando</span><span class="sxs-lookup"><span data-stu-id="cdc9d-117">Command</span></span> | <span data-ttu-id="cdc9d-118">Anteckningar</span><span class="sxs-lookup"><span data-stu-id="cdc9d-118">Notes</span></span> |
|---|---|
| [<span data-ttu-id="cdc9d-119">Skapa AZ grupp</span><span class="sxs-lookup"><span data-stu-id="cdc9d-119">az group create</span></span>](https://docs.microsoft.com/cli/azure/group#create) | <span data-ttu-id="cdc9d-120">Skapar en resursgrupp som är lagrade i alla resurser.</span><span class="sxs-lookup"><span data-stu-id="cdc9d-120">Creates a resource group in which all resources are stored.</span></span> |
| [<span data-ttu-id="cdc9d-121">Skapa AZ storage-konto</span><span class="sxs-lookup"><span data-stu-id="cdc9d-121">az storage account create</span></span>](https://docs.microsoft.com/cli/azure/appservice/plan#create) | <span data-ttu-id="cdc9d-122">Skapar en App Service-plan.</span><span class="sxs-lookup"><span data-stu-id="cdc9d-122">Creates an App Service plan.</span></span> |
| [<span data-ttu-id="cdc9d-123">Skapa AZ functionapp</span><span class="sxs-lookup"><span data-stu-id="cdc9d-123">az functionapp create</span></span>](https://docs.microsoft.com/cli/azure/appservice/web#delete) |
| [<span data-ttu-id="cdc9d-124">AZ apptjänst Webbkonfiguration källkontroll</span><span class="sxs-lookup"><span data-stu-id="cdc9d-124">az appservice web source-control config</span></span>](https://docs.microsoft.com/cli/azure/appservice/web/source-control#config) | <span data-ttu-id="cdc9d-125">Associerar en funktionsapp med en Git eller ett.</span><span class="sxs-lookup"><span data-stu-id="cdc9d-125">Associates a function app with a Git or Mercurial repository.</span></span> |

## <a name="next-steps"></a><span data-ttu-id="cdc9d-126">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="cdc9d-126">Next steps</span></span>

<span data-ttu-id="cdc9d-127">Mer information om Azure CLI finns [Azure CLI dokumentationen](https://docs.microsoft.com/cli/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="cdc9d-127">For more information on the Azure CLI, see [Azure CLI documentation](https://docs.microsoft.com/cli/azure/overview).</span></span>

<span data-ttu-id="cdc9d-128">Ytterligare Azure Functions CLI skriptexempel finns i den [Azure Functions dokumentationen](../functions-cli-samples.md).</span><span class="sxs-lookup"><span data-stu-id="cdc9d-128">Additional Azure Functions CLI script samples can be found in the [Azure Functions documentation](../functions-cli-samples.md).</span></span>
