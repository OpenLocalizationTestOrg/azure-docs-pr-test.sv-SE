---
title: "aaaAzure CLI skriptexempel - skala ett webbprogram manuellt med hjälp av Azure CLI 2.0 | Microsoft Docs"
description: "Skriptexempel Azure CLI - skala ett webbprogram manuellt med hjälp av Azure CLI 2.0"
services: appservice
documentationcenter: appservice
author: syntaxc4
manager: erikre
editor: 
tags: azure-service-management
ms.assetid: 251d9074-8fff-4121-ad16-9eca9556ac96
ms.service: app-service
ms.devlang: azurecli
ms.topic: sample
ms.tgt_pltfrm: na
ms.workload: web
ms.date: 06/19/2017
ms.author: cfowler
ms.custom: mvc
ms.openlocfilehash: 64464c8a44522fdc2c8f3d0192388302a1d12667
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="scale-a-web-app-manually"></a><span data-ttu-id="e59a9-103">Skala ett webbprogram manuellt</span><span class="sxs-lookup"><span data-stu-id="e59a9-103">Scale a web app manually</span></span>

<span data-ttu-id="e59a9-104">I det här scenariot får du lära dig toocreate en resursgrupp, apptjänst planerings- och webb-app.</span><span class="sxs-lookup"><span data-stu-id="e59a9-104">In this scenario you will learn toocreate a resource group, app service plan and web app.</span></span> <span data-ttu-id="e59a9-105">Du kommer sedan att skala hello App Service-Plan från en enda instans toomultiple instanser.</span><span class="sxs-lookup"><span data-stu-id="e59a9-105">You will then scale hello App Service Plan from a single instance toomultiple instances.</span></span>

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

<span data-ttu-id="e59a9-106">Om du väljer tooinstall och använda hello CLI lokalt kräver i det här avsnittet att du kör hello Azure CLI version 2.0 eller senare.</span><span class="sxs-lookup"><span data-stu-id="e59a9-106">If you choose tooinstall and use hello CLI locally, this topic requires that you are running hello Azure CLI version 2.0 or later.</span></span> <span data-ttu-id="e59a9-107">Kör `az --version` toofind hello version.</span><span class="sxs-lookup"><span data-stu-id="e59a9-107">Run `az --version` toofind hello version.</span></span> <span data-ttu-id="e59a9-108">Om du behöver tooinstall eller uppgradering, se [installera Azure CLI 2.0]( /cli/azure/install-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="e59a9-108">If you need tooinstall or upgrade, see [Install Azure CLI 2.0]( /cli/azure/install-azure-cli).</span></span> 

## <a name="sample-script"></a><span data-ttu-id="e59a9-109">Exempelskript</span><span class="sxs-lookup"><span data-stu-id="e59a9-109">Sample script</span></span>

[!code-azurecli-interactive[main](../../../cli_scripts/app-service/scale-manual/scale-manual.sh "Manual Scale")]

[!INCLUDE [cli-script-clean-up](../../../includes/cli-script-clean-up.md)]

## <a name="script-explanation"></a><span data-ttu-id="e59a9-110">Skriptet förklaring</span><span class="sxs-lookup"><span data-stu-id="e59a9-110">Script explanation</span></span>

<span data-ttu-id="e59a9-111">Det här skriptet använder hello följande kommandon toocreate en resursgrupp, webbprogram och alla relaterade resurser.</span><span class="sxs-lookup"><span data-stu-id="e59a9-111">This script uses hello following commands toocreate a resource group, web app, and all related resources.</span></span> <span data-ttu-id="e59a9-112">Varje kommando i hello tabellen länkar toocommand viss dokumentation.</span><span class="sxs-lookup"><span data-stu-id="e59a9-112">Each command in hello table links toocommand specific documentation.</span></span>

| <span data-ttu-id="e59a9-113">Kommando</span><span class="sxs-lookup"><span data-stu-id="e59a9-113">Command</span></span> | <span data-ttu-id="e59a9-114">Anteckningar</span><span class="sxs-lookup"><span data-stu-id="e59a9-114">Notes</span></span> |
|---|---|
| [<span data-ttu-id="e59a9-115">Skapa AZ grupp</span><span class="sxs-lookup"><span data-stu-id="e59a9-115">az group create</span></span>](https://docs.microsoft.com/cli/azure/group#create) | <span data-ttu-id="e59a9-116">Skapar en resursgrupp som är lagrade i alla resurser.</span><span class="sxs-lookup"><span data-stu-id="e59a9-116">Creates a resource group in which all resources are stored.</span></span> |
| [<span data-ttu-id="e59a9-117">Skapa AZ programtjänstplan</span><span class="sxs-lookup"><span data-stu-id="e59a9-117">az appservice plan create</span></span>](https://docs.microsoft.com/cli/azure/appservice/plan#create) | <span data-ttu-id="e59a9-118">Skapar en App Service-plan.</span><span class="sxs-lookup"><span data-stu-id="e59a9-118">Creates an App Service plan.</span></span> <span data-ttu-id="e59a9-119">Detta påminner om en servergrupp för din Azure webbapp.</span><span class="sxs-lookup"><span data-stu-id="e59a9-119">This is like a server farm for your Azure web app.</span></span> |
| [<span data-ttu-id="e59a9-120">Skapa AZ webapp</span><span class="sxs-lookup"><span data-stu-id="e59a9-120">az webapp create</span></span>](https://docs.microsoft.com/cli/azure/webapp#create) | <span data-ttu-id="e59a9-121">Skapar ett Azure-webbapp.</span><span class="sxs-lookup"><span data-stu-id="e59a9-121">Creates an Azure web app.</span></span> |
| [<span data-ttu-id="e59a9-122">AZ apptjänst plan uppdatering</span><span class="sxs-lookup"><span data-stu-id="e59a9-122">az appservice plan update</span></span>](https://docs.microsoft.com/cli/azure/appservice/plan#update) | <span data-ttu-id="e59a9-123">Uppdaterar egenskaperna för hello App Service-plan.</span><span class="sxs-lookup"><span data-stu-id="e59a9-123">Updates properties of hello App Service plan.</span></span> |

## <a name="next-steps"></a><span data-ttu-id="e59a9-124">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="e59a9-124">Next steps</span></span>

<span data-ttu-id="e59a9-125">Mer information om hello Azure CLI finns [Azure CLI dokumentationen](https://docs.microsoft.com/cli/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="e59a9-125">For more information on hello Azure CLI, see [Azure CLI documentation](https://docs.microsoft.com/cli/azure/overview).</span></span>

<span data-ttu-id="e59a9-126">Ytterligare App Service CLI skriptexempel finns i hello [dokumentation för Azure App Service](../app-service-cli-samples.md).</span><span class="sxs-lookup"><span data-stu-id="e59a9-126">Additional App Service CLI script samples can be found in hello [Azure App Service documentation](../app-service-cli-samples.md).</span></span>
