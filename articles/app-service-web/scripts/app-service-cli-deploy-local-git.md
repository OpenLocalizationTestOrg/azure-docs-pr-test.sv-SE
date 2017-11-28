---
title: "aaaAzure CLI skriptexempel - skapa en webbapp och distribuera kod från en lokal Git-lagringsplats | Microsoft Docs"
description: "Azure CLI skriptexempel - skapa en webbapp och distribuera kod från en lokal Git-lagringsplats"
services: app-service\web
documentationcenter: 
author: cephalin
manager: erikre
editor: 
tags: azure-service-management
ms.assetid: 048f98aa-f708-44cb-9b9e-953f67dc6da8
ms.service: app-service-web
ms.workload: web
ms.devlang: azurecli
ms.tgt_pltfrm: na
ms.topic: sample
ms.date: 06/19/2017
ms.author: cephalin
ms.custom: mvc
ms.openlocfilehash: 5ad75394c40025d8941282eabeaf34c19c72ee1a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-web-app-and-deploy-code-from-a-local-git-repository"></a><span data-ttu-id="383f2-103">Skapa en webbapp och distribuera kod från en lokal Git-lagringsplats</span><span class="sxs-lookup"><span data-stu-id="383f2-103">Create a web app and deploy code from a local Git repository</span></span>

<span data-ttu-id="383f2-104">Det här exempelskriptet skapar en webbapp i App Service med dess relaterade resurser och distribuerar sedan web app-kod i en lokal Git-lagringsplats.</span><span class="sxs-lookup"><span data-stu-id="383f2-104">This sample script creates a web app in App Service with its related resources, and then deploys your web app code in a local Git repository.</span></span>


[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

<span data-ttu-id="383f2-105">Om du väljer tooinstall och använda hello CLI lokalt kräver i det här avsnittet att du kör hello Azure CLI version 2.0 eller senare.</span><span class="sxs-lookup"><span data-stu-id="383f2-105">If you choose tooinstall and use hello CLI locally, this topic requires that you are running hello Azure CLI version 2.0 or later.</span></span> <span data-ttu-id="383f2-106">Kör `az --version` toofind hello version.</span><span class="sxs-lookup"><span data-stu-id="383f2-106">Run `az --version` toofind hello version.</span></span> <span data-ttu-id="383f2-107">Om du behöver tooinstall eller uppgradering, se [installera Azure CLI 2.0]( /cli/azure/install-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="383f2-107">If you need tooinstall or upgrade, see [Install Azure CLI 2.0]( /cli/azure/install-azure-cli).</span></span> 

## <a name="sample-script"></a><span data-ttu-id="383f2-108">Exempelskript</span><span class="sxs-lookup"><span data-stu-id="383f2-108">Sample script</span></span>

[!code-azurecli-interactive[main](../../../cli_scripts/app-service/deploy-local-git/deploy-local-git.sh?highlight=3-5 "Create a web app and deploy code from a local Git repository")]

[!INCLUDE [cli-script-clean-up](../../../includes/cli-script-clean-up.md)]

## <a name="script-explanation"></a><span data-ttu-id="383f2-109">Skriptet förklaring</span><span class="sxs-lookup"><span data-stu-id="383f2-109">Script explanation</span></span>

<span data-ttu-id="383f2-110">Det här skriptet använder hello följande kommandon.</span><span class="sxs-lookup"><span data-stu-id="383f2-110">This script uses hello following commands.</span></span> <span data-ttu-id="383f2-111">Varje kommando i hello tabellen länkar toocommand viss dokumentation.</span><span class="sxs-lookup"><span data-stu-id="383f2-111">Each command in hello table links toocommand specific documentation.</span></span>

| <span data-ttu-id="383f2-112">Kommando</span><span class="sxs-lookup"><span data-stu-id="383f2-112">Command</span></span> | <span data-ttu-id="383f2-113">Anteckningar</span><span class="sxs-lookup"><span data-stu-id="383f2-113">Notes</span></span> |
|---|---|
| [<span data-ttu-id="383f2-114">Skapa AZ grupp</span><span class="sxs-lookup"><span data-stu-id="383f2-114">az group create</span></span>](https://docs.microsoft.com/cli/azure/group#create) | <span data-ttu-id="383f2-115">Skapar en resursgrupp som är lagrade i alla resurser.</span><span class="sxs-lookup"><span data-stu-id="383f2-115">Creates a resource group in which all resources are stored.</span></span> |
| [<span data-ttu-id="383f2-116">Skapa AZ programtjänstplan</span><span class="sxs-lookup"><span data-stu-id="383f2-116">az appservice plan create</span></span>](https://docs.microsoft.com/cli/azure/appservice/plan#create) | <span data-ttu-id="383f2-117">Skapar en App Service-plan.</span><span class="sxs-lookup"><span data-stu-id="383f2-117">Creates an App Service plan.</span></span> |
| [<span data-ttu-id="383f2-118">Skapa AZ webapp</span><span class="sxs-lookup"><span data-stu-id="383f2-118">az webapp create</span></span>](https://docs.microsoft.com/cli/azure/webapp#create) | <span data-ttu-id="383f2-119">Skapar ett Azure-webbapp.</span><span class="sxs-lookup"><span data-stu-id="383f2-119">Creates an Azure web app.</span></span> |
| [<span data-ttu-id="383f2-120">AZ webapp distributionsanvändare har angetts</span><span class="sxs-lookup"><span data-stu-id="383f2-120">az webapp deployment user set</span></span>](https://review.docs.microsoft.com/cli/azure/webapp/deployment/user#set) | <span data-ttu-id="383f2-121">Anger autentiseringsuppgifter för hello kontonivå distribution för Apptjänst.</span><span class="sxs-lookup"><span data-stu-id="383f2-121">Sets hello account-level deployment credentials for App Service.</span></span> |
| [<span data-ttu-id="383f2-122">AZ webapp distribution källa config-lokal-git</span><span class="sxs-lookup"><span data-stu-id="383f2-122">az webapp deployment source config-local-git</span></span>](https://review.docs.microsoft.com/cli/azure/webapp/deployment/source#config-local-git) | <span data-ttu-id="383f2-123">Skapar en konfiguration för kontroll av källan för en lokal Git-lagringsplats.</span><span class="sxs-lookup"><span data-stu-id="383f2-123">Creates a source control configuration for a local Git repository.</span></span> |
| [<span data-ttu-id="383f2-124">AZ webapp Bläddra</span><span class="sxs-lookup"><span data-stu-id="383f2-124">az webapp browse</span></span>](https://docs.microsoft.com/cli/azure/webapp#browse) | <span data-ttu-id="383f2-125">Öppna en Azure-app i en webbläsare.</span><span class="sxs-lookup"><span data-stu-id="383f2-125">Open an Azure web app in a browser.</span></span> |

## <a name="next-steps"></a><span data-ttu-id="383f2-126">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="383f2-126">Next steps</span></span>

<span data-ttu-id="383f2-127">Mer information om hello Azure CLI finns [Azure CLI dokumentationen](https://docs.microsoft.com/cli/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="383f2-127">For more information on hello Azure CLI, see [Azure CLI documentation](https://docs.microsoft.com/cli/azure/overview).</span></span>

<span data-ttu-id="383f2-128">Ytterligare App Service CLI skriptexempel finns i hello [dokumentation för Azure App Service](../app-service-cli-samples.md).</span><span class="sxs-lookup"><span data-stu-id="383f2-128">Additional App Service CLI script samples can be found in hello [Azure App Service documentation](../app-service-cli-samples.md).</span></span>
