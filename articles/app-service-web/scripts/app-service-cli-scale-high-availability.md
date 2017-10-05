---
title: "Skriptexempel Azure CLI - skala en webbapp över hela världen med en hög availabilty-arkitektur | Microsoft Docs"
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
ms.openlocfilehash: c368bdc48f197ff5b491d1796d85abfd339051a6
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="scale-a-web-app-worldwide-with-a-high-availability-architecture"></a><span data-ttu-id="65124-103">Skala ett webbprogram över hela världen med en arkitektur med hög tillgänglighet</span><span class="sxs-lookup"><span data-stu-id="65124-103">Scale a web app worldwide with a high-availability architecture</span></span>

<span data-ttu-id="65124-104">I det här scenariot skapar du en resursgrupp, två apptjänstplaner, två webbappar, en traffic manager-profil och två traffic manager-slutpunkter.</span><span class="sxs-lookup"><span data-stu-id="65124-104">In this scenario you will create a resource group, two app service plans, two web apps, a traffic manager profile, and two traffic manager endpoints.</span></span> <span data-ttu-id="65124-105">När den här övningen är klar har du en hög tillgänglighet arkitektur som gör att tillhandahåller globala tillgängligheten hos ditt webbprogram baserat på den lägsta Nätverksfördröjningen.</span><span class="sxs-lookup"><span data-stu-id="65124-105">Once the exercise is complete you will have a high-available architecture which allows provides global availability of your web app based on the lowest network latency.</span></span>

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

<span data-ttu-id="65124-106">Om du väljer att installera och använda CLI lokalt måste du köra Azure CLI version 2.0 eller senare.</span><span class="sxs-lookup"><span data-stu-id="65124-106">If you choose to install and use the CLI locally, this topic requires that you are running the Azure CLI version 2.0 or later.</span></span> <span data-ttu-id="65124-107">Kör `az --version` för att hitta versionen.</span><span class="sxs-lookup"><span data-stu-id="65124-107">Run `az --version` to find the version.</span></span> <span data-ttu-id="65124-108">Om du behöver installera eller uppgradera kan du läsa [Installera Azure CLI 2.0]( /cli/azure/install-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="65124-108">If you need to install or upgrade, see [Install Azure CLI 2.0]( /cli/azure/install-azure-cli).</span></span> 


## <a name="sample-script"></a><span data-ttu-id="65124-109">Exempelskript</span><span class="sxs-lookup"><span data-stu-id="65124-109">Sample script</span></span>

<span data-ttu-id="65124-110">[!code-azurecli-interactive[huvudsakliga](../../../cli_scripts/app-service/scale-geographic/scale-geographic.sh "geografiska skala")]</span><span class="sxs-lookup"><span data-stu-id="65124-110">[!code-azurecli-interactive[main](../../../cli_scripts/app-service/scale-geographic/scale-geographic.sh "Geographic Scale")]</span></span>

[!INCLUDE [cli-script-clean-up](../../../includes/cli-script-clean-up.md)]

## <a name="script-explanation"></a><span data-ttu-id="65124-111">Skriptet förklaring</span><span class="sxs-lookup"><span data-stu-id="65124-111">Script explanation</span></span>

<span data-ttu-id="65124-112">Det här skriptet använder följande kommandon för att skapa en resursgrupp, webbprogram, trafikhanterarprofilen och alla relaterade resurser.</span><span class="sxs-lookup"><span data-stu-id="65124-112">This script uses the following commands to create a resource group, web app, traffic manager profile, and all related resources.</span></span> <span data-ttu-id="65124-113">Varje kommando i tabellen länkar till kommandot viss dokumentation.</span><span class="sxs-lookup"><span data-stu-id="65124-113">Each command in the table links to command specific documentation.</span></span>

| <span data-ttu-id="65124-114">Kommando</span><span class="sxs-lookup"><span data-stu-id="65124-114">Command</span></span> | <span data-ttu-id="65124-115">Anteckningar</span><span class="sxs-lookup"><span data-stu-id="65124-115">Notes</span></span> |
|---|---|
| [<span data-ttu-id="65124-116">Skapa AZ grupp</span><span class="sxs-lookup"><span data-stu-id="65124-116">az group create</span></span>](https://docs.microsoft.com/cli/azure/group#create) | <span data-ttu-id="65124-117">Skapar en resursgrupp som är lagrade i alla resurser.</span><span class="sxs-lookup"><span data-stu-id="65124-117">Creates a resource group in which all resources are stored.</span></span> |
| [<span data-ttu-id="65124-118">Skapa AZ programtjänstplan</span><span class="sxs-lookup"><span data-stu-id="65124-118">az appservice plan create</span></span>](https://docs.microsoft.com/cli/azure/appservice/plan#create) | <span data-ttu-id="65124-119">Skapar en App Service-plan.</span><span class="sxs-lookup"><span data-stu-id="65124-119">Creates an App Service plan.</span></span> <span data-ttu-id="65124-120">Detta påminner om en servergrupp för din Azure webbapp.</span><span class="sxs-lookup"><span data-stu-id="65124-120">This is like a server farm for your Azure web app.</span></span> |
| [<span data-ttu-id="65124-121">Skapa AZ webapp</span><span class="sxs-lookup"><span data-stu-id="65124-121">az webapp create</span></span>](https://docs.microsoft.com/cli/azure/webapp#create) | <span data-ttu-id="65124-122">Skapar ett Azure-webbapp.</span><span class="sxs-lookup"><span data-stu-id="65124-122">Creates an Azure web app.</span></span> |
| [<span data-ttu-id="65124-123">Skapa AZ network traffic manager-profilen</span><span class="sxs-lookup"><span data-stu-id="65124-123">az network traffic-manager profile create</span></span>](https://docs.microsoft.com/cli/azure/network/traffic-manager/profile#create) | <span data-ttu-id="65124-124">Skapar en Azure Traffic Manager-profil.</span><span class="sxs-lookup"><span data-stu-id="65124-124">Creates an Azure Traffic Manager profile.</span></span> |
| [<span data-ttu-id="65124-125">Skapa AZ network traffic manager-slutpunkt</span><span class="sxs-lookup"><span data-stu-id="65124-125">az network traffic-manager endpoint create</span></span>](https://docs.microsoft.com/cli/azure/network/traffic-manager/endpoint#create) | <span data-ttu-id="65124-126">Lägger till en slutpunkt för en Azure Traffic Manager-profilen.</span><span class="sxs-lookup"><span data-stu-id="65124-126">Adds a endpoint to an Azure Traffic Manager Profile.</span></span> |

## <a name="next-steps"></a><span data-ttu-id="65124-127">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="65124-127">Next steps</span></span>

<span data-ttu-id="65124-128">Mer information om Azure CLI finns [Azure CLI dokumentationen](https://docs.microsoft.com/cli/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="65124-128">For more information on the Azure CLI, see [Azure CLI documentation](https://docs.microsoft.com/cli/azure/overview).</span></span>

<span data-ttu-id="65124-129">Ytterligare App Service CLI skriptexempel finns i den [dokumentation för Azure App Service](../app-service-cli-samples.md).</span><span class="sxs-lookup"><span data-stu-id="65124-129">Additional App Service CLI script samples can be found in the [Azure App Service documentation](../app-service-cli-samples.md).</span></span>
