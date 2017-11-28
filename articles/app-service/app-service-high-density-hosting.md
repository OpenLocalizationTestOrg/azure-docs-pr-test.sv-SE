---
title: "värd för aaaHigh densitet i Azure App Service | Microsoft Docs"
description: "Hög densitet värd i Azure App Service"
author: btardif
manager: erikre
editor: 
services: app-service\web
documentationcenter: 
ms.assetid: a903cb78-4927-47b0-8427-56412c4e3e64
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: multiple
ms.topic: article
ms.date: 06/12/2017
ms.author: byvinyal
ms.openlocfilehash: a10cb81ace13ba6992b572a44361061ecf72b266
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="high-density-hosting-on-azure-app-service"></a><span data-ttu-id="91b79-103">Hög densitet värd i Azure App Service</span><span class="sxs-lookup"><span data-stu-id="91b79-103">High density hosting on Azure App Service</span></span>
<span data-ttu-id="91b79-104">När du använder App Service, frikopplas programmet hello kapacitet som tilldelas tooit av två begrepp:</span><span class="sxs-lookup"><span data-stu-id="91b79-104">When using App Service, your application is decoupled from hello capacity allocated tooit by two concepts:</span></span>

* <span data-ttu-id="91b79-105">**hello program:** representerar hello appen och dess runtime-konfiguration.</span><span class="sxs-lookup"><span data-stu-id="91b79-105">**hello Application:** Represents hello app and its runtime configuration.</span></span> <span data-ttu-id="91b79-106">Innehåller till exempel hello version av .NET som hello runtime ska läsas in, hello app-inställningar.</span><span class="sxs-lookup"><span data-stu-id="91b79-106">For example, it includes hello version of .NET that hello runtime should load, hello app settings.</span></span>
* <span data-ttu-id="91b79-107">**Hej Apptjänstplan:** definierar hello egenskaper hello kapacitet, tillgängliga funktioner och plats för hello program.</span><span class="sxs-lookup"><span data-stu-id="91b79-107">**hello App Service Plan:** Defines hello characteristics of hello capacity, available feature set, and locality of hello application.</span></span> <span data-ttu-id="91b79-108">Egenskaper kan till exempel vara stora (fyra kärnor)-dator, fyra instanser, Premium-funktioner i östra USA.</span><span class="sxs-lookup"><span data-stu-id="91b79-108">For example, characteristics might be large (four cores) machine, four instances, Premium features in East US.</span></span>

<span data-ttu-id="91b79-109">En app är alltid länkade tooan App Service-plan, men en apptjänstplan kan ge kapacitet tooone eller fler appar.</span><span class="sxs-lookup"><span data-stu-id="91b79-109">An app is always linked tooan App Service plan, but an App Service plan can provide capacity tooone or more apps.</span></span>

<span data-ttu-id="91b79-110">Därför hello-plattformen ger hello flexibilitet tooisolate en enda app eller har flera appar som delar resurser genom att dela en App Service-plan.</span><span class="sxs-lookup"><span data-stu-id="91b79-110">As a result, hello platform provides hello flexibility tooisolate a single app or have multiple apps share resources by sharing an App Service plan.</span></span>

<span data-ttu-id="91b79-111">När flera appar delar en apptjänstplan, kör en instans av appen på varje förekomst av den App Service-planen.</span><span class="sxs-lookup"><span data-stu-id="91b79-111">However, when multiple apps share an App Service plan, an instance of that app runs on every instance of that App Service plan.</span></span>

## <a name="per-app-scaling"></a><span data-ttu-id="91b79-112">Per app skalning</span><span class="sxs-lookup"><span data-stu-id="91b79-112">Per app scaling</span></span>
<span data-ttu-id="91b79-113">*Per app skalning* är en funktion som kan aktiveras på nivån för App Service-plan och sedan används per program.</span><span class="sxs-lookup"><span data-stu-id="91b79-113">*Per app scaling* is a feature that can be enabled at the App Service plan level and then used per application.</span></span>

<span data-ttu-id="91b79-114">Per app skalas skalning appen oberoende av App Service-plan som är värd för den.</span><span class="sxs-lookup"><span data-stu-id="91b79-114">Per app scaling scales an app independently from the App Service plan that hosts it.</span></span> <span data-ttu-id="91b79-115">På så sätt kan en Apptjänst planen kan skalas too10 instanser, men bara fem toouse kan anges till en app.</span><span class="sxs-lookup"><span data-stu-id="91b79-115">This way, an App Service plan can be scaled too10 instances, but an app can be set toouse only five.</span></span>

   >[!NOTE]
   ><span data-ttu-id="91b79-116">Per app skalning är bara tillgängligt för **Premium** SKU App Service-planer</span><span class="sxs-lookup"><span data-stu-id="91b79-116">Per app scaling is available only for **Premium** SKU App Service plans</span></span>
   >

### <a name="per-app-scaling-using-powershell"></a><span data-ttu-id="91b79-117">Per app skalning med PowerShell</span><span class="sxs-lookup"><span data-stu-id="91b79-117">Per app scaling using PowerShell</span></span>

<span data-ttu-id="91b79-118">Du kan skapa en plan som konfigurerats som en *Per app skalning* plan genom att passera i hello ```-perSiteScaling $true``` attributet toohello ```New-AzureRmAppServicePlan``` cmdleten igen</span><span class="sxs-lookup"><span data-stu-id="91b79-118">You can create a plan configured as a *Per app scaling* plan by passing in hello ```-perSiteScaling $true``` attribute toohello ```New-AzureRmAppServicePlan``` commandlet</span></span>

```
New-AzureRmAppServicePlan -ResourceGroupName $ResourceGroup -Name $AppServicePlan `
                            -Location $Location `
                            -Tier Premium -WorkerSize Small `
                            -NumberofWorkers 5 -PerSiteScaling $true
```

<span data-ttu-id="91b79-119">Om du vill tooupdate en befintlig App Service-plan toouse funktionen:</span><span class="sxs-lookup"><span data-stu-id="91b79-119">If you want tooupdate an existing App Service plan toouse this feature:</span></span> 

- <span data-ttu-id="91b79-120">Hämta hello mål plan```Get-AzureRmAppServicePlan```</span><span class="sxs-lookup"><span data-stu-id="91b79-120">get hello target plan ```Get-AzureRmAppServicePlan```</span></span>
- <span data-ttu-id="91b79-121">Ändra hello egenskapen lokalt```$newASP.PerSiteScaling = $true```</span><span class="sxs-lookup"><span data-stu-id="91b79-121">modifying hello property locally ```$newASP.PerSiteScaling = $true```</span></span>
- <span data-ttu-id="91b79-122">Skicka dina ändringar tillbaka tooazure```Set-AzureRmAppServicePlan```</span><span class="sxs-lookup"><span data-stu-id="91b79-122">posting your changes back tooazure ```Set-AzureRmAppServicePlan```</span></span> 

```
# Get hello new App Service Plan and modify hello "PerSiteScaling" property.
$newASP = Get-AzureRmAppServicePlan -ResourceGroupName $ResourceGroup -Name $AppServicePlan
$newASP

#Modify hello local copy toouse "PerSiteScaling" property.
$newASP.PerSiteScaling = $true
$newASP
    
#Post updated app service plan back tooazure
Set-AzureRmAppServicePlan $newASP
```

<span data-ttu-id="91b79-123">På hello app-nivå behöver vi tooconfigure hello antal instanser hello app kan använda i hello app service-plan.</span><span class="sxs-lookup"><span data-stu-id="91b79-123">At hello app level, we need tooconfigure hello number of instances hello app can use in hello app service plan.</span></span>

<span data-ttu-id="91b79-124">I hello exemplet nedan hello appen är begränsad tootwo instanser oavsett hur många instanser hello underliggande app service-plan skalbar ut.</span><span class="sxs-lookup"><span data-stu-id="91b79-124">In hello example below, hello app is limited tootwo instances regardless of how many instances hello underlying app service plan scales out to.</span></span>

```
# Get hello app we want tooconfigure toouse "PerSiteScaling"
$newapp = Get-AzureRmWebApp -ResourceGroupName $ResourceGroup -Name $webapp
    
# Modify hello NumberOfWorkers setting toohello desired value.
$newapp.SiteConfig.NumberOfWorkers = 2
    
# Post updated app back tooazure
Set-AzureRmWebApp $newapp
```

> [!IMPORTANT]
> <span data-ttu-id="91b79-125">$newapp. SiteConfig.NumberOfWorkers är olika formulär $newapp. MaxNumberOfWorkers.</span><span class="sxs-lookup"><span data-stu-id="91b79-125">$newapp.SiteConfig.NumberOfWorkers is different form $newapp.MaxNumberOfWorkers.</span></span> <span data-ttu-id="91b79-126">Per app använder skalning $newapp. SiteConfig.NumberOfWorkers toodetermine hello skala egenskaper hello app.</span><span class="sxs-lookup"><span data-stu-id="91b79-126">Per app scaling uses $newapp.SiteConfig.NumberOfWorkers toodetermine hello scale characteristics of hello app.</span></span>

### <a name="per-app-scaling-using-azure-resource-manager"></a><span data-ttu-id="91b79-127">Per app skalning med Azure Resource Manager</span><span class="sxs-lookup"><span data-stu-id="91b79-127">Per app scaling using Azure Resource Manager</span></span>

<span data-ttu-id="91b79-128">hello följande *Azure Resource Manager-mall* skapar:</span><span class="sxs-lookup"><span data-stu-id="91b79-128">hello following *Azure Resource Manager template* creates:</span></span>

- <span data-ttu-id="91b79-129">En apptjänstplan utskalad too10 instanser</span><span class="sxs-lookup"><span data-stu-id="91b79-129">An App Service plan that's scaled out too10 instances</span></span>
- <span data-ttu-id="91b79-130">en app som har konfigurerats tooscale tooa maximalt fem instanser.</span><span class="sxs-lookup"><span data-stu-id="91b79-130">an app that's configured tooscale tooa max of five instances.</span></span>

<span data-ttu-id="91b79-131">Hej programtjänstplanen inställningen hello **PerSiteScaling** egenskapen tootrue ```"perSiteScaling": true```.</span><span class="sxs-lookup"><span data-stu-id="91b79-131">hello App Service plan is setting hello **PerSiteScaling** property tootrue ```"perSiteScaling": true```.</span></span> <span data-ttu-id="91b79-132">hello app inställningen hello **antal arbetare** toouse too5 ```"properties": { "numberOfWorkers": "5" }```.</span><span class="sxs-lookup"><span data-stu-id="91b79-132">hello app is setting hello **number of workers** toouse too5 ```"properties": { "numberOfWorkers": "5" }```.</span></span>

```
{
    "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters":{
        "appServicePlanName": { "type": "string" },
        "appName": { "type": "string" }
        },
    "resources": [
    {
        "comments": "App Service Plan with per site perSiteScaling = true",
        "type": "Microsoft.Web/serverFarms",
        "sku": {
            "name": "P1",
            "tier": "Premium",
            "size": "P1",
            "family": "P",
            "capacity": 10
            },
        "name": "[parameters('appServicePlanName')]",
        "apiVersion": "2015-08-01",
        "location": "West US",
        "properties": {
            "name": "[parameters('appServicePlanName')]",
            "perSiteScaling": true
        }
    },
    {
        "type": "Microsoft.Web/sites",
        "name": "[parameters('appName')]",
        "apiVersion": "2015-08-01-preview",
        "location": "West US",
        "dependsOn": [ "[resourceId('Microsoft.Web/serverFarms', parameters('appServicePlanName'))]" ],
        "properties": { "serverFarmId": "[resourceId('Microsoft.Web/serverFarms', parameters('appServicePlanName'))]" },
        "resources": [ {
                "comments": "",
                "type": "config",
                "name": "web",
                "apiVersion": "2015-08-01",
                "location": "West US",
                "dependsOn": [ "[resourceId('Microsoft.Web/Sites', parameters('appName'))]" ],
                "properties": { "numberOfWorkers": "5" }
            } ]
        }]
}
```

## <a name="recommended-configuration-for-high-density-hosting"></a><span data-ttu-id="91b79-133">Rekommenderad konfiguration för hög densitet värd</span><span class="sxs-lookup"><span data-stu-id="91b79-133">Recommended configuration for high density hosting</span></span>
<span data-ttu-id="91b79-134">Är en funktion som är aktiverad i både globala Azure-regioner och Apptjänstmiljöer per app skalning.</span><span class="sxs-lookup"><span data-stu-id="91b79-134">Per app scaling is a feature that is enabled in both global Azure regions and App Service Environments.</span></span> <span data-ttu-id="91b79-135">Du bör dock hello strategi är att använda Apptjänstmiljöer tootake nytta av de avancerade funktionerna och hello större pooler med kapacitet.</span><span class="sxs-lookup"><span data-stu-id="91b79-135">However, hello recommended strategy is to use App Service Environments tootake advantage of their advanced features and hello larger pools of capacity.</span></span>  

<span data-ttu-id="91b79-136">Följ dessa steg tooconfigure hög densitet värd för dina appar:</span><span class="sxs-lookup"><span data-stu-id="91b79-136">Follow these steps tooconfigure high density hosting for your apps:</span></span>

1. <span data-ttu-id="91b79-137">Konfigurera hello Apptjänst-miljön och välj en arbetspool som är dedikerad toohello hög densitet som värd för scenariot.</span><span class="sxs-lookup"><span data-stu-id="91b79-137">Configure hello App Service Environment and choose a worker pool that is dedicated toohello high density hosting scenario.</span></span>
1. <span data-ttu-id="91b79-138">Skapa en enkel App Service-plan och skala den toouse alla hello tillgänglig kapacitet i hello arbetspool.</span><span class="sxs-lookup"><span data-stu-id="91b79-138">Create a single App Service plan, and scale it toouse all hello available capacity on hello worker pool.</span></span>
1. <span data-ttu-id="91b79-139">Ange hello PerSiteScaling flaggan tootrue på hello App Service-plan.</span><span class="sxs-lookup"><span data-stu-id="91b79-139">Set hello PerSiteScaling flag tootrue on hello App Service plan.</span></span>
1. <span data-ttu-id="91b79-140">Nya appar skapas och tilldelas toothat App Service-plan med de **numberOfWorkers** egenskapsuppsättning för**1**.</span><span class="sxs-lookup"><span data-stu-id="91b79-140">New apps are created and assigned toothat App Service plan with the **numberOfWorkers** property set too**1**.</span></span> <span data-ttu-id="91b79-141">Med den här konfigurationen ger hello högsta densitet möjliga i den här arbetspool.</span><span class="sxs-lookup"><span data-stu-id="91b79-141">Using this configuration yields hello highest density possible on this worker pool.</span></span>
1. <span data-ttu-id="91b79-142">hello antalet arbetare kan konfigureras oberoende per app toogrant ytterligare resurser efter behov.</span><span class="sxs-lookup"><span data-stu-id="91b79-142">hello number of workers can be configured independently per app toogrant additional resources as needed.</span></span> <span data-ttu-id="91b79-143">Exempel:</span><span class="sxs-lookup"><span data-stu-id="91b79-143">For example:</span></span>
    - <span data-ttu-id="91b79-144">En hög användning app kan ange **numberOfWorkers** för**3** toohave mer bearbetning av kapacitet för appen.</span><span class="sxs-lookup"><span data-stu-id="91b79-144">A high-use app can set **numberOfWorkers** too**3** toohave more processing capacity for that app.</span></span> 
    - <span data-ttu-id="91b79-145">Använd låg appar skulle ange **numberOfWorkers** för**1**.</span><span class="sxs-lookup"><span data-stu-id="91b79-145">Low-use apps would set **numberOfWorkers** too**1**.</span></span>

## <a name="next-steps"></a><span data-ttu-id="91b79-146">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="91b79-146">Next Steps</span></span>

- [<span data-ttu-id="91b79-147">Azure App Service-planer djupgående översikt</span><span class="sxs-lookup"><span data-stu-id="91b79-147">Azure App Service plans in-depth overview</span></span>](azure-web-sites-web-hosting-plans-in-depth-overview.md)
- [<span data-ttu-id="91b79-148">Introduktion tooApp-miljö</span><span class="sxs-lookup"><span data-stu-id="91b79-148">Introduction tooApp Service Environment</span></span>](../app-service-web/app-service-app-service-environment-intro.md)
