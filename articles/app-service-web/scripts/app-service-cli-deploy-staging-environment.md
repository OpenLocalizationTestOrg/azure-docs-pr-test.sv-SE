---
title: "aaaAzure CLI skriptexempel - skapa en webbapp och distribuera kod tooa mellanlagring miljö | Microsoft Docs"
description: "Azure CLI skriptexempel - skapa en webbapp och distribuera kod tooa mellanlagring miljö"
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
ms.openlocfilehash: cd07f5eda31041effd7b7334f5ecc04e6c1a0514
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-web-app-and-deploy-code-tooa-staging-environment"></a><span data-ttu-id="4a744-103">Skapa en webbapp och distribuera kod tooa mellanlagring miljö</span><span class="sxs-lookup"><span data-stu-id="4a744-103">Create a web app and deploy code tooa staging environment</span></span>

<span data-ttu-id="4a744-104">Det här exempelskriptet skapar en webbapp i App Service med en ytterligare distributionsplatsen som kallas ”mellanlagring” och sedan distribuerar ett exempel app toohello ”mellanlagringsplatsen”.</span><span class="sxs-lookup"><span data-stu-id="4a744-104">This sample script creates a web app in App Service with an additional deployment slot called "staging", and then deploys a sample app toohello "staging" slot.</span></span>

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]


[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

<span data-ttu-id="4a744-105">Om du väljer tooinstall och använda hello CLI lokalt kräver i det här avsnittet att du kör hello Azure CLI version 2.0 eller senare.</span><span class="sxs-lookup"><span data-stu-id="4a744-105">If you choose tooinstall and use hello CLI locally, this topic requires that you are running hello Azure CLI version 2.0 or later.</span></span> <span data-ttu-id="4a744-106">Kör `az --version` toofind hello version.</span><span class="sxs-lookup"><span data-stu-id="4a744-106">Run `az --version` toofind hello version.</span></span> <span data-ttu-id="4a744-107">Om du behöver tooinstall eller uppgradering, se [installera Azure CLI 2.0]( /cli/azure/install-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="4a744-107">If you need tooinstall or upgrade, see [Install Azure CLI 2.0]( /cli/azure/install-azure-cli).</span></span> 

## <a name="sample-script"></a><span data-ttu-id="4a744-108">Exempelskript</span><span class="sxs-lookup"><span data-stu-id="4a744-108">Sample script</span></span>

[!code-azurecli-interactive[main](../../../cli_scripts/app-service/deploy-deployment-slot/deploy-deployment-slot.sh "Create a web app and deploy code tooa staging environment")]

[!INCLUDE [cli-script-clean-up](../../../includes/cli-script-clean-up.md)]

## <a name="script-explanation"></a><span data-ttu-id="4a744-109">Skriptet förklaring</span><span class="sxs-lookup"><span data-stu-id="4a744-109">Script explanation</span></span>

<span data-ttu-id="4a744-110">Det här skriptet använder hello följande kommandon.</span><span class="sxs-lookup"><span data-stu-id="4a744-110">This script uses hello following commands.</span></span> <span data-ttu-id="4a744-111">Varje kommando i hello tabellen länkar toocommand viss dokumentation.</span><span class="sxs-lookup"><span data-stu-id="4a744-111">Each command in hello table links toocommand specific documentation.</span></span>

| <span data-ttu-id="4a744-112">Kommando</span><span class="sxs-lookup"><span data-stu-id="4a744-112">Command</span></span> | <span data-ttu-id="4a744-113">Anteckningar</span><span class="sxs-lookup"><span data-stu-id="4a744-113">Notes</span></span> |
|---|---|
| [<span data-ttu-id="4a744-114">Skapa AZ grupp</span><span class="sxs-lookup"><span data-stu-id="4a744-114">az group create</span></span>](https://docs.microsoft.com/cli/azure/group#create) | <span data-ttu-id="4a744-115">Skapar en resursgrupp som är lagrade i alla resurser.</span><span class="sxs-lookup"><span data-stu-id="4a744-115">Creates a resource group in which all resources are stored.</span></span> |
| [<span data-ttu-id="4a744-116">Skapa AZ programtjänstplan</span><span class="sxs-lookup"><span data-stu-id="4a744-116">az appservice plan create</span></span>](https://docs.microsoft.com/cli/azure/appservice/plan#create) | <span data-ttu-id="4a744-117">Skapar en App Service-plan.</span><span class="sxs-lookup"><span data-stu-id="4a744-117">Creates an App Service plan.</span></span> |
| [<span data-ttu-id="4a744-118">Skapa AZ webapp</span><span class="sxs-lookup"><span data-stu-id="4a744-118">az webapp create</span></span>](https://docs.microsoft.com/cli/azure/webapp#create) | <span data-ttu-id="4a744-119">Skapar ett Azure-webbapp.</span><span class="sxs-lookup"><span data-stu-id="4a744-119">Creates an Azure web app.</span></span> |
| [<span data-ttu-id="4a744-120">Skapa AZ webapp distributionsplatsen</span><span class="sxs-lookup"><span data-stu-id="4a744-120">az webapp deployment slot create</span></span>](https://docs.microsoft.com/cli/azure/webapp/deployment/slot#create) | <span data-ttu-id="4a744-121">Skapa en distributionsplats.</span><span class="sxs-lookup"><span data-stu-id="4a744-121">Create a deployment slot.</span></span> |
| [<span data-ttu-id="4a744-122">AZ webapp distributionskonfiguration källa</span><span class="sxs-lookup"><span data-stu-id="4a744-122">az webapp deployment source config</span></span>](https://docs.microsoft.com/cli/azure/webapp/deployment/source#config) | <span data-ttu-id="4a744-123">Associerar en Azure-app med en Git eller ett lagringsplats.</span><span class="sxs-lookup"><span data-stu-id="4a744-123">Associates an Azure web app with a Git or Mercurial repository.</span></span> |
| [<span data-ttu-id="4a744-124">AZ webapp Bläddra</span><span class="sxs-lookup"><span data-stu-id="4a744-124">az webapp browse</span></span>](https://docs.microsoft.com/cli/azure/webapp#browse) | <span data-ttu-id="4a744-125">Öppna en Azure-app i en webbläsare.</span><span class="sxs-lookup"><span data-stu-id="4a744-125">Open an Azure web app in a browser.</span></span> |
| [<span data-ttu-id="4a744-126">AZ webapp distribution fack växlingsutrymme</span><span class="sxs-lookup"><span data-stu-id="4a744-126">az webapp deployment slot swap</span></span>](https://docs.microsoft.com/cli/azure/webapp/deployment/slot#swap) | <span data-ttu-id="4a744-127">Växla angivna distributionsplatsen till produktionen.</span><span class="sxs-lookup"><span data-stu-id="4a744-127">Swap a specified deployment slot into production.</span></span> |

## <a name="next-steps"></a><span data-ttu-id="4a744-128">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="4a744-128">Next steps</span></span>

<span data-ttu-id="4a744-129">Mer information om hello Azure CLI finns [Azure CLI dokumentationen](https://docs.microsoft.com/cli/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="4a744-129">For more information on hello Azure CLI, see [Azure CLI documentation](https://docs.microsoft.com/cli/azure/overview).</span></span>

<span data-ttu-id="4a744-130">Ytterligare App Service CLI skriptexempel finns i hello [dokumentation för Azure App Service](../app-service-cli-samples.md).</span><span class="sxs-lookup"><span data-stu-id="4a744-130">Additional App Service CLI script samples can be found in hello [Azure App Service documentation](../app-service-cli-samples.md).</span></span>
