---
title: "aaaAzure CLI skriptexempel - skala ett webbprogram över hela världen med en hög availabilty-arkitektur | Microsoft Docs"
description: "Skriptexempel Azure CLI - skala en webbapp över hela världen med en hög availabilty-arkitektur"
services: appservice
documentationcenter: appservice
author: syntaxc4
manager: erikre
editor: 
tags: azure-service-management
ms.assetid: e4033a50-0e05-4505-8ce8-c876204b2acc
ms.service: app-service
ms.devlang: azurecli
ms.topic: sample
ms.tgt_pltfrm: na
ms.workload: web
ms.date: 06/19/2017
ms.author: cfowler
ms.custom: mvc
ms.openlocfilehash: b72fbccd7f2aaab58e4b4721e14dca14146c7c72
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="scale-a-web-app-worldwide-with-a-high-availability-architecture"></a><span data-ttu-id="89705-103">Skala ett webbprogram över hela världen med en arkitektur med hög tillgänglighet</span><span class="sxs-lookup"><span data-stu-id="89705-103">Scale a web app worldwide with a high-availability architecture</span></span>

<span data-ttu-id="89705-104">I det här scenariot skapar du en resursgrupp, två apptjänstplaner, två webbappar, en traffic manager-profil och två traffic manager-slutpunkter.</span><span class="sxs-lookup"><span data-stu-id="89705-104">In this scenario you will create a resource group, two app service plans, two web apps, a traffic manager profile, and two traffic manager endpoints.</span></span> <span data-ttu-id="89705-105">När hello övningen är klar har du en hög tillgänglighet arkitektur som gör att tillhandahåller globala tillgängligheten hos ditt webbprogram baserat på hello lägsta Nätverksfördröjningen.</span><span class="sxs-lookup"><span data-stu-id="89705-105">Once hello exercise is complete you will have a high-available architecture which allows provides global availability of your web app based on hello lowest network latency.</span></span>

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

<span data-ttu-id="89705-106">Om du väljer tooinstall och använda hello CLI lokalt kräver i det här avsnittet att du kör hello Azure CLI version 2.0 eller senare.</span><span class="sxs-lookup"><span data-stu-id="89705-106">If you choose tooinstall and use hello CLI locally, this topic requires that you are running hello Azure CLI version 2.0 or later.</span></span> <span data-ttu-id="89705-107">Kör `az --version` toofind hello version.</span><span class="sxs-lookup"><span data-stu-id="89705-107">Run `az --version` toofind hello version.</span></span> <span data-ttu-id="89705-108">Om du behöver tooinstall eller uppgradering, se [installera Azure CLI 2.0]( /cli/azure/install-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="89705-108">If you need tooinstall or upgrade, see [Install Azure CLI 2.0]( /cli/azure/install-azure-cli).</span></span> 


## <a name="sample-script"></a><span data-ttu-id="89705-109">Exempelskript</span><span class="sxs-lookup"><span data-stu-id="89705-109">Sample script</span></span>

[!code-azurecli-interactive[main](../../../cli_scripts/app-service/scale-geographic/scale-geographic.sh "Geographic Scale")]

[!INCLUDE [cli-script-clean-up](../../../includes/cli-script-clean-up.md)]

## <a name="script-explanation"></a><span data-ttu-id="89705-110">Skriptet förklaring</span><span class="sxs-lookup"><span data-stu-id="89705-110">Script explanation</span></span>

<span data-ttu-id="89705-111">Det här skriptet använder följande kommandon toocreate en resursgrupp, webbprogram, trafikhanterarprofilen hello och alla relaterade resurser.</span><span class="sxs-lookup"><span data-stu-id="89705-111">This script uses hello following commands toocreate a resource group, web app, traffic manager profile, and all related resources.</span></span> <span data-ttu-id="89705-112">Varje kommando i hello tabellen länkar toocommand viss dokumentation.</span><span class="sxs-lookup"><span data-stu-id="89705-112">Each command in hello table links toocommand specific documentation.</span></span>

| <span data-ttu-id="89705-113">Kommando</span><span class="sxs-lookup"><span data-stu-id="89705-113">Command</span></span> | <span data-ttu-id="89705-114">Anteckningar</span><span class="sxs-lookup"><span data-stu-id="89705-114">Notes</span></span> |
|---|---|
| [<span data-ttu-id="89705-115">Skapa AZ grupp</span><span class="sxs-lookup"><span data-stu-id="89705-115">az group create</span></span>](https://docs.microsoft.com/cli/azure/group#create) | <span data-ttu-id="89705-116">Skapar en resursgrupp som är lagrade i alla resurser.</span><span class="sxs-lookup"><span data-stu-id="89705-116">Creates a resource group in which all resources are stored.</span></span> |
| [<span data-ttu-id="89705-117">Skapa AZ programtjänstplan</span><span class="sxs-lookup"><span data-stu-id="89705-117">az appservice plan create</span></span>](https://docs.microsoft.com/cli/azure/appservice/plan#create) | <span data-ttu-id="89705-118">Skapar en App Service-plan.</span><span class="sxs-lookup"><span data-stu-id="89705-118">Creates an App Service plan.</span></span> <span data-ttu-id="89705-119">Detta påminner om en servergrupp för din Azure webbapp.</span><span class="sxs-lookup"><span data-stu-id="89705-119">This is like a server farm for your Azure web app.</span></span> |
| [<span data-ttu-id="89705-120">Skapa AZ webapp</span><span class="sxs-lookup"><span data-stu-id="89705-120">az webapp create</span></span>](https://docs.microsoft.com/cli/azure/webapp#create) | <span data-ttu-id="89705-121">Skapar ett Azure-webbapp.</span><span class="sxs-lookup"><span data-stu-id="89705-121">Creates an Azure web app.</span></span> |
| [<span data-ttu-id="89705-122">Skapa AZ network traffic manager-profilen</span><span class="sxs-lookup"><span data-stu-id="89705-122">az network traffic-manager profile create</span></span>](https://docs.microsoft.com/cli/azure/network/traffic-manager/profile#create) | <span data-ttu-id="89705-123">Skapar en Azure Traffic Manager-profil.</span><span class="sxs-lookup"><span data-stu-id="89705-123">Creates an Azure Traffic Manager profile.</span></span> |
| [<span data-ttu-id="89705-124">Skapa AZ network traffic manager-slutpunkt</span><span class="sxs-lookup"><span data-stu-id="89705-124">az network traffic-manager endpoint create</span></span>](https://docs.microsoft.com/cli/azure/network/traffic-manager/endpoint#create) | <span data-ttu-id="89705-125">Lägger till en slutpunkt tooan Azure Traffic Manager-profilen.</span><span class="sxs-lookup"><span data-stu-id="89705-125">Adds a endpoint tooan Azure Traffic Manager Profile.</span></span> |

## <a name="next-steps"></a><span data-ttu-id="89705-126">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="89705-126">Next steps</span></span>

<span data-ttu-id="89705-127">Mer information om hello Azure CLI finns [Azure CLI dokumentationen](https://docs.microsoft.com/cli/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="89705-127">For more information on hello Azure CLI, see [Azure CLI documentation](https://docs.microsoft.com/cli/azure/overview).</span></span>

<span data-ttu-id="89705-128">Ytterligare App Service CLI skriptexempel finns i hello [dokumentation för Azure App Service](../app-service-cli-samples.md).</span><span class="sxs-lookup"><span data-stu-id="89705-128">Additional App Service CLI script samples can be found in hello [Azure App Service documentation](../app-service-cli-samples.md).</span></span>
