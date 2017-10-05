---
title: "Skriptexempel Azure CLI - vägtrafik för hög tillgänglighet för program | Microsoft Docs"
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
ms.openlocfilehash: 0593d063a4935d02aae124d83b62b11e37aa3c33
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="route-traffic-for-high-availability-of-applications"></a><span data-ttu-id="2463c-103">Vidarebefordra trafik för hög tillgänglighet för program</span><span class="sxs-lookup"><span data-stu-id="2463c-103">Route traffic for high availability of applications</span></span>

<span data-ttu-id="2463c-104">Det här skriptet skapar en resursgrupp, två apptjänstplaner, två webbappar, en traffic manager-profil och två traffic manager-slutpunkter.</span><span class="sxs-lookup"><span data-stu-id="2463c-104">This script creates a resource group, two app service plans, two web apps, a traffic manager profile, and two traffic manager endpoints.</span></span> <span data-ttu-id="2463c-105">Traffic Manager dirigerar trafik till program i en region som den primära regionen och till den sekundära regionen när programmet i den primära regionen är inte tillgänglig.</span><span class="sxs-lookup"><span data-stu-id="2463c-105">Traffic Manager directs traffic to the application in one region as the primary region, and to the secondary region when the application in the primary region is unavailable.</span></span> <span data-ttu-id="2463c-106">Innan du kör skriptet måste du ändra värdena MyWebApp, MyWebAppL1 och MyWebAppL2 till unika värden i Azure.</span><span class="sxs-lookup"><span data-stu-id="2463c-106">Before executing the script, you must change the MyWebApp, MyWebAppL1 and MyWebAppL2 values to unique values across Azure.</span></span> <span data-ttu-id="2463c-107">Du kan komma åt appen i den primära regionen med URL-mywebapp.trafficmanager.net när skriptet har körts.</span><span class="sxs-lookup"><span data-stu-id="2463c-107">After running the script, you can access the app in the primary region with the URL mywebapp.trafficmanager.net.</span></span>

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a><span data-ttu-id="2463c-108">Exempelskript</span><span class="sxs-lookup"><span data-stu-id="2463c-108">Sample script</span></span>

<span data-ttu-id="2463c-109">[!code-azurecli-interactive[huvudsakliga](../../../cli_scripts/traffic-manager/direct-traffic-for-increased-application-availability/direct-traffic-for-increased-application-availability.sh "dirigera trafik för hög tillgänglighet")]</span><span class="sxs-lookup"><span data-stu-id="2463c-109">[!code-azurecli-interactive[main](../../../cli_scripts/traffic-manager/direct-traffic-for-increased-application-availability/direct-traffic-for-increased-application-availability.sh "Route traffic for high availability")]</span></span>


## <a name="clean-up-deployment"></a><span data-ttu-id="2463c-110">Rensa distribution</span><span class="sxs-lookup"><span data-stu-id="2463c-110">Clean up deployment</span></span> 

<span data-ttu-id="2463c-111">Efter skriptexempel har körts, följ-kommando kan användas för att ta bort resursgruppen, App Service-appen och alla relaterade resurser.</span><span class="sxs-lookup"><span data-stu-id="2463c-111">After the script sample has been run, the follow command can be used to remove the resource group, App Service app, and all related resources.</span></span>

```azurecli
az group delete --name myResourceGroup1 --yes
az group delete --name myResourceGroup2 --yes
```

## <a name="script-explanation"></a><span data-ttu-id="2463c-112">Skriptet förklaring</span><span class="sxs-lookup"><span data-stu-id="2463c-112">Script explanation</span></span>

<span data-ttu-id="2463c-113">Det här skriptet använder följande kommandon för att skapa en resursgrupp, webbprogram, trafikhanterarprofilen och alla relaterade resurser.</span><span class="sxs-lookup"><span data-stu-id="2463c-113">This script uses the following commands to create a resource group, web app, traffic manager profile, and all related resources.</span></span> <span data-ttu-id="2463c-114">Varje kommando i tabellen länkar till kommandot viss dokumentation.</span><span class="sxs-lookup"><span data-stu-id="2463c-114">Each command in the table links to command specific documentation.</span></span>

| <span data-ttu-id="2463c-115">Kommando</span><span class="sxs-lookup"><span data-stu-id="2463c-115">Command</span></span> | <span data-ttu-id="2463c-116">Anteckningar</span><span class="sxs-lookup"><span data-stu-id="2463c-116">Notes</span></span> |
|---|---|
| [<span data-ttu-id="2463c-117">Skapa AZ grupp</span><span class="sxs-lookup"><span data-stu-id="2463c-117">az group create</span></span>](https://docs.microsoft.com/cli/azure/group#create) | <span data-ttu-id="2463c-118">Skapar en resursgrupp som är lagrade i alla resurser.</span><span class="sxs-lookup"><span data-stu-id="2463c-118">Creates a resource group in which all resources are stored.</span></span> |
| [<span data-ttu-id="2463c-119">Skapa AZ programtjänstplan</span><span class="sxs-lookup"><span data-stu-id="2463c-119">az appservice plan create</span></span>](https://docs.microsoft.com/cli/azure/appservice/plan#create) | <span data-ttu-id="2463c-120">Skapar en App Service-plan.</span><span class="sxs-lookup"><span data-stu-id="2463c-120">Creates an App Service plan.</span></span> <span data-ttu-id="2463c-121">Detta påminner om en servergrupp för din Azure webbapp.</span><span class="sxs-lookup"><span data-stu-id="2463c-121">This is like a server farm for your Azure web app.</span></span> |
| [<span data-ttu-id="2463c-122">Skapa AZ apptjänst web</span><span class="sxs-lookup"><span data-stu-id="2463c-122">az appservice web create</span></span>](https://docs.microsoft.com/cli/azure/appservice/web#create) | <span data-ttu-id="2463c-123">Skapar ett Azure-webbapp i App Service-plan.</span><span class="sxs-lookup"><span data-stu-id="2463c-123">Creates an Azure web app within the App Service plan.</span></span> |
| [<span data-ttu-id="2463c-124">Skapa AZ network traffic manager-profilen</span><span class="sxs-lookup"><span data-stu-id="2463c-124">az network traffic-manager profile create</span></span>](https://docs.microsoft.com/cli/azure/network/traffic-manager/profile#create) | <span data-ttu-id="2463c-125">Skapar en Azure Traffic Manager-profil.</span><span class="sxs-lookup"><span data-stu-id="2463c-125">Creates an Azure Traffic Manager profile.</span></span> |
| [<span data-ttu-id="2463c-126">Skapa AZ network traffic manager-slutpunkt</span><span class="sxs-lookup"><span data-stu-id="2463c-126">az network traffic-manager endpoint create</span></span>](https://docs.microsoft.com/cli/azure/network/traffic-manager/endpoint#create) | <span data-ttu-id="2463c-127">Lägger till en slutpunkt för en Azure Traffic Manager-profilen.</span><span class="sxs-lookup"><span data-stu-id="2463c-127">Adds a endpoint to an Azure Traffic Manager Profile.</span></span> |

## <a name="next-steps"></a><span data-ttu-id="2463c-128">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="2463c-128">Next steps</span></span>

<span data-ttu-id="2463c-129">Mer information om Azure CLI finns [Azure CLI dokumentationen](https://docs.microsoft.com/cli/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="2463c-129">For more information on the Azure CLI, see [Azure CLI documentation](https://docs.microsoft.com/cli/azure/overview).</span></span>

<span data-ttu-id="2463c-130">Ytterligare App Service CLI skriptexempel finns i den [Azure nätverk dokumentationen](../cli-samples.md).</span><span class="sxs-lookup"><span data-stu-id="2463c-130">Additional App Service CLI script samples can be found in the [Azure Networking documentation](../cli-samples.md).</span></span>
