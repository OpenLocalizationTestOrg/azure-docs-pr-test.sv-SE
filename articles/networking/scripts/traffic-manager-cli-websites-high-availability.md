---
title: "aaaAzure skriptexempel CLI - vägtrafik för hög tillgänglighet för program | Microsoft Docs"
description: "Skriptexempel Azure CLI - vägtrafik för hög tillgänglighet för program"
services: traffic-manager
documentationcenter: traffic-manager
author: KumudD
manager: timlt
editor: tysonn
tags: azure-infrastructure
ms.assetid: 
ms.service: traffic-manager
ms.devlang: azurecli
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: traffic-manager
ms.date: 07/07/2017
ms.author: kumud
ms.openlocfilehash: 2142c8bbec1dffc2f12b5500df142a429393a145
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="route-traffic-for-high-availability-of-applications"></a><span data-ttu-id="60f27-103">Vidarebefordra trafik för hög tillgänglighet för program</span><span class="sxs-lookup"><span data-stu-id="60f27-103">Route traffic for high availability of applications</span></span>

<span data-ttu-id="60f27-104">Det här skriptet skapar en resursgrupp, två apptjänstplaner, två webbappar, en traffic manager-profil och två traffic manager-slutpunkter.</span><span class="sxs-lookup"><span data-stu-id="60f27-104">This script creates a resource group, two app service plans, two web apps, a traffic manager profile, and two traffic manager endpoints.</span></span> <span data-ttu-id="60f27-105">Traffic Manager dirigerar trafik toohello programmet i en region som hello primär region och toohello sekundär region när programmet hello i hello primära regionen är inte tillgänglig.</span><span class="sxs-lookup"><span data-stu-id="60f27-105">Traffic Manager directs traffic toohello application in one region as hello primary region, and toohello secondary region when hello application in hello primary region is unavailable.</span></span> <span data-ttu-id="60f27-106">Innan du kör skriptet hello måste du ändra hello MyWebApp, MyWebAppL1 och MyWebAppL2 värdena toounique värdena i Azure.</span><span class="sxs-lookup"><span data-stu-id="60f27-106">Before executing hello script, you must change hello MyWebApp, MyWebAppL1 and MyWebAppL2 values toounique values across Azure.</span></span> <span data-ttu-id="60f27-107">Du kan komma åt hello app i hello primär region med hello URL mywebapp.trafficmanager.net när du har kört hello skript.</span><span class="sxs-lookup"><span data-stu-id="60f27-107">After running hello script, you can access hello app in hello primary region with hello URL mywebapp.trafficmanager.net.</span></span>

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a><span data-ttu-id="60f27-108">Exempelskript</span><span class="sxs-lookup"><span data-stu-id="60f27-108">Sample script</span></span>

[!code-azurecli-interactive[main](../../../cli_scripts/traffic-manager/direct-traffic-for-increased-application-availability/direct-traffic-for-increased-application-availability.sh "Route traffic for high availability")]


## <a name="clean-up-deployment"></a><span data-ttu-id="60f27-109">Rensa distribution</span><span class="sxs-lookup"><span data-stu-id="60f27-109">Clean up deployment</span></span> 

<span data-ttu-id="60f27-110">Efter hello skriptexempel har körts kan hello Följ kommandot vara används tooremove hello resursgrupp, App Service-appen och alla relaterade resurser.</span><span class="sxs-lookup"><span data-stu-id="60f27-110">After hello script sample has been run, hello follow command can be used tooremove hello resource group, App Service app, and all related resources.</span></span>

```azurecli
az group delete --name myResourceGroup1 --yes
az group delete --name myResourceGroup2 --yes
```

## <a name="script-explanation"></a><span data-ttu-id="60f27-111">Skriptet förklaring</span><span class="sxs-lookup"><span data-stu-id="60f27-111">Script explanation</span></span>

<span data-ttu-id="60f27-112">Det här skriptet använder följande kommandon toocreate en resursgrupp, webbprogram, trafikhanterarprofilen hello och alla relaterade resurser.</span><span class="sxs-lookup"><span data-stu-id="60f27-112">This script uses hello following commands toocreate a resource group, web app, traffic manager profile, and all related resources.</span></span> <span data-ttu-id="60f27-113">Varje kommando i hello tabellen länkar toocommand viss dokumentation.</span><span class="sxs-lookup"><span data-stu-id="60f27-113">Each command in hello table links toocommand specific documentation.</span></span>

| <span data-ttu-id="60f27-114">Kommando</span><span class="sxs-lookup"><span data-stu-id="60f27-114">Command</span></span> | <span data-ttu-id="60f27-115">Anteckningar</span><span class="sxs-lookup"><span data-stu-id="60f27-115">Notes</span></span> |
|---|---|
| [<span data-ttu-id="60f27-116">Skapa AZ grupp</span><span class="sxs-lookup"><span data-stu-id="60f27-116">az group create</span></span>](https://docs.microsoft.com/cli/azure/group#create) | <span data-ttu-id="60f27-117">Skapar en resursgrupp som är lagrade i alla resurser.</span><span class="sxs-lookup"><span data-stu-id="60f27-117">Creates a resource group in which all resources are stored.</span></span> |
| [<span data-ttu-id="60f27-118">Skapa AZ programtjänstplan</span><span class="sxs-lookup"><span data-stu-id="60f27-118">az appservice plan create</span></span>](https://docs.microsoft.com/cli/azure/appservice/plan#create) | <span data-ttu-id="60f27-119">Skapar en App Service-plan.</span><span class="sxs-lookup"><span data-stu-id="60f27-119">Creates an App Service plan.</span></span> <span data-ttu-id="60f27-120">Detta påminner om en servergrupp för din Azure webbapp.</span><span class="sxs-lookup"><span data-stu-id="60f27-120">This is like a server farm for your Azure web app.</span></span> |
| [<span data-ttu-id="60f27-121">Skapa AZ apptjänst web</span><span class="sxs-lookup"><span data-stu-id="60f27-121">az appservice web create</span></span>](https://docs.microsoft.com/cli/azure/appservice/web#create) | <span data-ttu-id="60f27-122">Skapar en Azure webbapp i hello App Service-plan.</span><span class="sxs-lookup"><span data-stu-id="60f27-122">Creates an Azure web app within hello App Service plan.</span></span> |
| [<span data-ttu-id="60f27-123">Skapa AZ network traffic manager-profilen</span><span class="sxs-lookup"><span data-stu-id="60f27-123">az network traffic-manager profile create</span></span>](https://docs.microsoft.com/cli/azure/network/traffic-manager/profile#create) | <span data-ttu-id="60f27-124">Skapar en Azure Traffic Manager-profil.</span><span class="sxs-lookup"><span data-stu-id="60f27-124">Creates an Azure Traffic Manager profile.</span></span> |
| [<span data-ttu-id="60f27-125">Skapa AZ network traffic manager-slutpunkt</span><span class="sxs-lookup"><span data-stu-id="60f27-125">az network traffic-manager endpoint create</span></span>](https://docs.microsoft.com/cli/azure/network/traffic-manager/endpoint#create) | <span data-ttu-id="60f27-126">Lägger till en slutpunkt tooan Azure Traffic Manager-profilen.</span><span class="sxs-lookup"><span data-stu-id="60f27-126">Adds a endpoint tooan Azure Traffic Manager Profile.</span></span> |

## <a name="next-steps"></a><span data-ttu-id="60f27-127">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="60f27-127">Next steps</span></span>

<span data-ttu-id="60f27-128">Mer information om hello Azure CLI finns [Azure CLI dokumentationen](https://docs.microsoft.com/cli/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="60f27-128">For more information on hello Azure CLI, see [Azure CLI documentation](https://docs.microsoft.com/cli/azure/overview).</span></span>

<span data-ttu-id="60f27-129">Ytterligare App Service CLI skriptexempel finns i hello [Azure nätverk dokumentationen](../cli-samples.md).</span><span class="sxs-lookup"><span data-stu-id="60f27-129">Additional App Service CLI script samples can be found in hello [Azure Networking documentation](../cli-samples.md).</span></span>
