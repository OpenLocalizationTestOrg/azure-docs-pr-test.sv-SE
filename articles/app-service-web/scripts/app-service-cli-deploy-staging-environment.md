---
title: "Azure CLI skriptexempel - skapa en webbapp och distribuera kod till en mellanlagringsmiljön | Microsoft Docs"
description: "Azure CLI skriptexempel - skapa en webbapp och distribuera kod till en mellanlagringsmiljön"
services: app-service\web
documentationcenter: 
author: cephalin
manager: erikre
editor: 
tags: azure-service-management
ms.assetid: 2b995dcd-e471-4355-9fda-00babcdb156e
ms.service: app-service-web
ms.workload: web
ms.devlang: azurecli
ms.tgt_pltfrm: na
ms.topic: sample
ms.date: 06/19/2017
ms.author: cephalin
ms.custom: mvc
ms.openlocfilehash: d586b50258c32e44f55859aad0a89475e9e4d2eb
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="create-a-web-app-and-deploy-code-to-a-staging-environment"></a><span data-ttu-id="932ed-103">Skapa en webbapp och distribuera kod till en mellanlagringsmiljön</span><span class="sxs-lookup"><span data-stu-id="932ed-103">Create a web app and deploy code to a staging environment</span></span>

<span data-ttu-id="932ed-104">Det här exempelskriptet skapar en webbapp i App Service med en ytterligare distributionsplatsen som kallas ”mellanlagring” och sedan distribuerar en exempelapp till ”” mellanlagringsplatsen.</span><span class="sxs-lookup"><span data-stu-id="932ed-104">This sample script creates a web app in App Service with an additional deployment slot called "staging", and then deploys a sample app to the "staging" slot.</span></span>

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]


[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

<span data-ttu-id="932ed-105">Om du väljer att installera och använda CLI lokalt måste du köra Azure CLI version 2.0 eller senare.</span><span class="sxs-lookup"><span data-stu-id="932ed-105">If you choose to install and use the CLI locally, this topic requires that you are running the Azure CLI version 2.0 or later.</span></span> <span data-ttu-id="932ed-106">Kör `az --version` för att hitta versionen.</span><span class="sxs-lookup"><span data-stu-id="932ed-106">Run `az --version` to find the version.</span></span> <span data-ttu-id="932ed-107">Om du behöver installera eller uppgradera kan du läsa [Installera Azure CLI 2.0]( /cli/azure/install-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="932ed-107">If you need to install or upgrade, see [Install Azure CLI 2.0]( /cli/azure/install-azure-cli).</span></span> 

## <a name="sample-script"></a><span data-ttu-id="932ed-108">Exempelskript</span><span class="sxs-lookup"><span data-stu-id="932ed-108">Sample script</span></span>

<span data-ttu-id="932ed-109">[!code-azurecli-interactive[huvudsakliga](../../../cli_scripts/app-service/deploy-deployment-slot/deploy-deployment-slot.sh "skapa en webbapp och distribuera kod till en mellanlagringsmiljön")]</span><span class="sxs-lookup"><span data-stu-id="932ed-109">[!code-azurecli-interactive[main](../../../cli_scripts/app-service/deploy-deployment-slot/deploy-deployment-slot.sh "Create a web app and deploy code to a staging environment")]</span></span>

[!INCLUDE [cli-script-clean-up](../../../includes/cli-script-clean-up.md)]

## <a name="script-explanation"></a><span data-ttu-id="932ed-110">Skriptet förklaring</span><span class="sxs-lookup"><span data-stu-id="932ed-110">Script explanation</span></span>

<span data-ttu-id="932ed-111">Det här skriptet använder följande kommandon.</span><span class="sxs-lookup"><span data-stu-id="932ed-111">This script uses the following commands.</span></span> <span data-ttu-id="932ed-112">Varje kommando i tabellen länkar till kommandot viss dokumentation.</span><span class="sxs-lookup"><span data-stu-id="932ed-112">Each command in the table links to command specific documentation.</span></span>

| <span data-ttu-id="932ed-113">Kommando</span><span class="sxs-lookup"><span data-stu-id="932ed-113">Command</span></span> | <span data-ttu-id="932ed-114">Anteckningar</span><span class="sxs-lookup"><span data-stu-id="932ed-114">Notes</span></span> |
|---|---|
| [<span data-ttu-id="932ed-115">Skapa AZ grupp</span><span class="sxs-lookup"><span data-stu-id="932ed-115">az group create</span></span>](https://docs.microsoft.com/cli/azure/group#create) | <span data-ttu-id="932ed-116">Skapar en resursgrupp som är lagrade i alla resurser.</span><span class="sxs-lookup"><span data-stu-id="932ed-116">Creates a resource group in which all resources are stored.</span></span> |
| [<span data-ttu-id="932ed-117">Skapa AZ programtjänstplan</span><span class="sxs-lookup"><span data-stu-id="932ed-117">az appservice plan create</span></span>](https://docs.microsoft.com/cli/azure/appservice/plan#create) | <span data-ttu-id="932ed-118">Skapar en App Service-plan.</span><span class="sxs-lookup"><span data-stu-id="932ed-118">Creates an App Service plan.</span></span> |
| [<span data-ttu-id="932ed-119">Skapa AZ webapp</span><span class="sxs-lookup"><span data-stu-id="932ed-119">az webapp create</span></span>](https://docs.microsoft.com/cli/azure/webapp#create) | <span data-ttu-id="932ed-120">Skapar ett Azure-webbapp.</span><span class="sxs-lookup"><span data-stu-id="932ed-120">Creates an Azure web app.</span></span> |
| [<span data-ttu-id="932ed-121">Skapa AZ webapp distributionsplatsen</span><span class="sxs-lookup"><span data-stu-id="932ed-121">az webapp deployment slot create</span></span>](https://docs.microsoft.com/cli/azure/webapp/deployment/slot#create) | <span data-ttu-id="932ed-122">Skapa en distributionsplats.</span><span class="sxs-lookup"><span data-stu-id="932ed-122">Create a deployment slot.</span></span> |
| [<span data-ttu-id="932ed-123">AZ webapp distributionskonfiguration källa</span><span class="sxs-lookup"><span data-stu-id="932ed-123">az webapp deployment source config</span></span>](https://docs.microsoft.com/cli/azure/webapp/deployment/source#config) | <span data-ttu-id="932ed-124">Associerar en Azure-app med en Git eller ett lagringsplats.</span><span class="sxs-lookup"><span data-stu-id="932ed-124">Associates an Azure web app with a Git or Mercurial repository.</span></span> |
| [<span data-ttu-id="932ed-125">AZ webapp Bläddra</span><span class="sxs-lookup"><span data-stu-id="932ed-125">az webapp browse</span></span>](https://docs.microsoft.com/cli/azure/webapp#browse) | <span data-ttu-id="932ed-126">Öppna en Azure-app i en webbläsare.</span><span class="sxs-lookup"><span data-stu-id="932ed-126">Open an Azure web app in a browser.</span></span> |
| [<span data-ttu-id="932ed-127">AZ webapp distribution fack växlingsutrymme</span><span class="sxs-lookup"><span data-stu-id="932ed-127">az webapp deployment slot swap</span></span>](https://docs.microsoft.com/cli/azure/webapp/deployment/slot#swap) | <span data-ttu-id="932ed-128">Växla angivna distributionsplatsen till produktionen.</span><span class="sxs-lookup"><span data-stu-id="932ed-128">Swap a specified deployment slot into production.</span></span> |

## <a name="next-steps"></a><span data-ttu-id="932ed-129">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="932ed-129">Next steps</span></span>

<span data-ttu-id="932ed-130">Mer information om Azure CLI finns [Azure CLI dokumentationen](https://docs.microsoft.com/cli/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="932ed-130">For more information on the Azure CLI, see [Azure CLI documentation](https://docs.microsoft.com/cli/azure/overview).</span></span>

<span data-ttu-id="932ed-131">Ytterligare App Service CLI skriptexempel finns i den [dokumentation för Azure App Service](../app-service-cli-samples.md).</span><span class="sxs-lookup"><span data-stu-id="932ed-131">Additional App Service CLI script samples can be found in the [Azure App Service documentation](../app-service-cli-samples.md).</span></span>
