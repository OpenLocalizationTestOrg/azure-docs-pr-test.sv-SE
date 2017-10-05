---
title: "Hög densitet värd i Azure App Service | Microsoft Docs"
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
ms.openlocfilehash: 459a310a719695f6366470976d857ec2f9d6f4a1
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="high-density-hosting-on-azure-app-service"></a><span data-ttu-id="49a92-103">Hög densitet värd i Azure App Service</span><span class="sxs-lookup"><span data-stu-id="49a92-103">High density hosting on Azure App Service</span></span>
<span data-ttu-id="49a92-104">När du använder App Service, frikopplas programmet från den kapacitet som tilldelas av två begrepp:</span><span class="sxs-lookup"><span data-stu-id="49a92-104">When using App Service, your application is decoupled from the capacity allocated to it by two concepts:</span></span>

* <span data-ttu-id="49a92-105">**Program:** representerar appen och dess runtime-konfiguration.</span><span class="sxs-lookup"><span data-stu-id="49a92-105">**The Application:** Represents the app and its runtime configuration.</span></span> <span data-ttu-id="49a92-106">Den innehåller till exempel versioner av .NET som körningen ska läsas in, app-inställningar.</span><span class="sxs-lookup"><span data-stu-id="49a92-106">For example, it includes the version of .NET that the runtime should load, the app settings.</span></span>
* <span data-ttu-id="49a92-107">**Programtjänstplanen:** definierar egenskaperna för kapacitet, tillgängliga funktioner och ort för programmet.</span><span class="sxs-lookup"><span data-stu-id="49a92-107">**The App Service Plan:** Defines the characteristics of the capacity, available feature set, and locality of the application.</span></span> <span data-ttu-id="49a92-108">Egenskaper kan till exempel vara stora (fyra kärnor)-dator, fyra instanser, Premium-funktioner i östra USA.</span><span class="sxs-lookup"><span data-stu-id="49a92-108">For example, characteristics might be large (four cores) machine, four instances, Premium features in East US.</span></span>

<span data-ttu-id="49a92-109">En app alltid är kopplad till en App Service-plan, men en apptjänstplan kan ge kapacitet till en eller flera appar.</span><span class="sxs-lookup"><span data-stu-id="49a92-109">An app is always linked to an App Service plan, but an App Service plan can provide capacity to one or more apps.</span></span>

<span data-ttu-id="49a92-110">Därför ger plattformen flexibilitet att isolera en enda app eller har flera appar som delar resurser genom att dela en App Service-plan.</span><span class="sxs-lookup"><span data-stu-id="49a92-110">As a result, the platform provides the flexibility to isolate a single app or have multiple apps share resources by sharing an App Service plan.</span></span>

<span data-ttu-id="49a92-111">När flera appar delar en apptjänstplan, kör en instans av appen på varje förekomst av den App Service-planen.</span><span class="sxs-lookup"><span data-stu-id="49a92-111">However, when multiple apps share an App Service plan, an instance of that app runs on every instance of that App Service plan.</span></span>

## <a name="per-app-scaling"></a><span data-ttu-id="49a92-112">Per app skalning</span><span class="sxs-lookup"><span data-stu-id="49a92-112">Per app scaling</span></span>
<span data-ttu-id="49a92-113">*Per app skalning* är en funktion som kan aktiveras på nivån för App Service-plan och sedan används per program.</span><span class="sxs-lookup"><span data-stu-id="49a92-113">*Per app scaling* is a feature that can be enabled at the App Service plan level and then used per application.</span></span>

<span data-ttu-id="49a92-114">Per app skalas skalning appen oberoende av App Service-plan som är värd för den.</span><span class="sxs-lookup"><span data-stu-id="49a92-114">Per app scaling scales an app independently from the App Service plan that hosts it.</span></span> <span data-ttu-id="49a92-115">På så sätt kan en App Service-plan kan skalas till 10 instanser, men en app kan ställas in att använda bara fem.</span><span class="sxs-lookup"><span data-stu-id="49a92-115">This way, an App Service plan can be scaled to 10 instances, but an app can be set to use only five.</span></span>

   >[!NOTE]
   ><span data-ttu-id="49a92-116">Per app skalning är bara tillgängligt för **Premium** SKU App Service-planer</span><span class="sxs-lookup"><span data-stu-id="49a92-116">Per app scaling is available only for **Premium** SKU App Service plans</span></span>
   >

### <a name="per-app-scaling-using-powershell"></a><span data-ttu-id="49a92-117">Per app skalning med PowerShell</span><span class="sxs-lookup"><span data-stu-id="49a92-117">Per app scaling using PowerShell</span></span>

<span data-ttu-id="49a92-118">Du kan skapa en plan som konfigurerats som en *Per app skalning* plan genom att passera i den ```-perSiteScaling $true``` attribut till den ```New-AzureRmAppServicePlan``` cmdleten igen</span><span class="sxs-lookup"><span data-stu-id="49a92-118">You can create a plan configured as a *Per app scaling* plan by passing in the ```-perSiteScaling $true``` attribute to the ```New-AzureRmAppServicePlan``` commandlet</span></span>

```
New-AzureRmAppServicePlan -ResourceGroupName $ResourceGroup -Name $AppServicePlan `
                            -Location $Location `
                            -Tier Premium -WorkerSize Small `
                            -NumberofWorkers 5 -PerSiteScaling $true
```

<span data-ttu-id="49a92-119">Om du vill uppdatera en befintlig programtjänstplan att använda den här funktionen:</span><span class="sxs-lookup"><span data-stu-id="49a92-119">If you want to update an existing App Service plan to use this feature:</span></span> 

- <span data-ttu-id="49a92-120">Hämta mål-plan```Get-AzureRmAppServicePlan```</span><span class="sxs-lookup"><span data-stu-id="49a92-120">get the target plan ```Get-AzureRmAppServicePlan```</span></span>
- <span data-ttu-id="49a92-121">ändra egenskapen lokalt```$newASP.PerSiteScaling = $true```</span><span class="sxs-lookup"><span data-stu-id="49a92-121">modifying the property locally ```$newASP.PerSiteScaling = $true```</span></span>
- <span data-ttu-id="49a92-122">skicka ändringarna till azure```Set-AzureRmAppServicePlan```</span><span class="sxs-lookup"><span data-stu-id="49a92-122">posting your changes back to azure ```Set-AzureRmAppServicePlan```</span></span> 

```
# Get the new App Service Plan and modify the "PerSiteScaling" property.
$newASP = Get-AzureRmAppServicePlan -ResourceGroupName $ResourceGroup -Name $AppServicePlan
$newASP

#Modify the local copy to use "PerSiteScaling" property.
$newASP.PerSiteScaling = $true
$newASP
    
#Post updated app service plan back to azure
Set-AzureRmAppServicePlan $newASP
```

<span data-ttu-id="49a92-123">Vi behöver konfigurera antalet instanser som appen kan använda i app service-plan på app-nivå.</span><span class="sxs-lookup"><span data-stu-id="49a92-123">At the app level, we need to configure the number of instances the app can use in the app service plan.</span></span>

<span data-ttu-id="49a92-124">Appen är begränsad till två instanser oavsett hur många instanser underliggande apptjänstplan skalas ut till i exemplet nedan.</span><span class="sxs-lookup"><span data-stu-id="49a92-124">In the example below, the app is limited to two instances regardless of how many instances the underlying app service plan scales out to.</span></span>

```
# Get the app we want to configure to use "PerSiteScaling"
$newapp = Get-AzureRmWebApp -ResourceGroupName $ResourceGroup -Name $webapp
    
# Modify the NumberOfWorkers setting to the desired value.
$newapp.SiteConfig.NumberOfWorkers = 2
    
# Post updated app back to azure
Set-AzureRmWebApp $newapp
```

> [!IMPORTANT]
> <span data-ttu-id="49a92-125">$newapp. SiteConfig.NumberOfWorkers är olika formulär $newapp. MaxNumberOfWorkers.</span><span class="sxs-lookup"><span data-stu-id="49a92-125">$newapp.SiteConfig.NumberOfWorkers is different form $newapp.MaxNumberOfWorkers.</span></span> <span data-ttu-id="49a92-126">Per app använder skalning $newapp. SiteConfig.NumberOfWorkers till bestämmer skala egenskaper för appen.</span><span class="sxs-lookup"><span data-stu-id="49a92-126">Per app scaling uses $newapp.SiteConfig.NumberOfWorkers to determine the scale characteristics of the app.</span></span>

### <a name="per-app-scaling-using-azure-resource-manager"></a><span data-ttu-id="49a92-127">Per app skalning med Azure Resource Manager</span><span class="sxs-lookup"><span data-stu-id="49a92-127">Per app scaling using Azure Resource Manager</span></span>

<span data-ttu-id="49a92-128">Följande *Azure Resource Manager-mall* skapar:</span><span class="sxs-lookup"><span data-stu-id="49a92-128">The following *Azure Resource Manager template* creates:</span></span>

- <span data-ttu-id="49a92-129">En apptjänstplan skalas ut till 10 instanser</span><span class="sxs-lookup"><span data-stu-id="49a92-129">An App Service plan that's scaled out to 10 instances</span></span>
- <span data-ttu-id="49a92-130">en app som är konfigurerad att skala upp till högst fem instanser.</span><span class="sxs-lookup"><span data-stu-id="49a92-130">an app that's configured to scale to a max of five instances.</span></span>

<span data-ttu-id="49a92-131">Programtjänstplanen inställningen i **PerSiteScaling** egenskap till true ```"perSiteScaling": true```.</span><span class="sxs-lookup"><span data-stu-id="49a92-131">The App Service plan is setting the **PerSiteScaling** property to true ```"perSiteScaling": true```.</span></span> <span data-ttu-id="49a92-132">Appen inställningen i **antal arbetare** att använda till 5 ```"properties": { "numberOfWorkers": "5" }```.</span><span class="sxs-lookup"><span data-stu-id="49a92-132">The app is setting the **number of workers** to use to 5 ```"properties": { "numberOfWorkers": "5" }```.</span></span>

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

## <a name="recommended-configuration-for-high-density-hosting"></a><span data-ttu-id="49a92-133">Rekommenderad konfiguration för hög densitet värd</span><span class="sxs-lookup"><span data-stu-id="49a92-133">Recommended configuration for high density hosting</span></span>
<span data-ttu-id="49a92-134">Är en funktion som är aktiverad i både globala Azure-regioner och Apptjänstmiljöer per app skalning.</span><span class="sxs-lookup"><span data-stu-id="49a92-134">Per app scaling is a feature that is enabled in both global Azure regions and App Service Environments.</span></span> <span data-ttu-id="49a92-135">Den rekommenderade strategin är dock att använda Apptjänstmiljöer för att dra nytta av de avancerade funktionerna och större pooler med kapacitet.</span><span class="sxs-lookup"><span data-stu-id="49a92-135">However, the recommended strategy is to use App Service Environments to take advantage of their advanced features and the larger pools of capacity.</span></span>  

<span data-ttu-id="49a92-136">Följ dessa steg om du vill konfigurera hög densitet som värd för dina appar:</span><span class="sxs-lookup"><span data-stu-id="49a92-136">Follow these steps to configure high density hosting for your apps:</span></span>

1. <span data-ttu-id="49a92-137">Konfigurera Apptjänst-miljön och välj en arbetspool som är dedikerad till hög densitet värd scenariot.</span><span class="sxs-lookup"><span data-stu-id="49a92-137">Configure the App Service Environment and choose a worker pool that is dedicated to the high density hosting scenario.</span></span>
1. <span data-ttu-id="49a92-138">Skapa en enkel App Service-plan och skala och använda den tillgängliga kapaciteten i poolen worker.</span><span class="sxs-lookup"><span data-stu-id="49a92-138">Create a single App Service plan, and scale it to use all the available capacity on the worker pool.</span></span>
1. <span data-ttu-id="49a92-139">Ställ in flaggan PerSiteScaling på true för App Service-plan.</span><span class="sxs-lookup"><span data-stu-id="49a92-139">Set the PerSiteScaling flag to true on the App Service plan.</span></span>
1. <span data-ttu-id="49a92-140">Nya appar skapas och tilldelas till den App Service-planen med de **numberOfWorkers** egenskapen **1**.</span><span class="sxs-lookup"><span data-stu-id="49a92-140">New apps are created and assigned to that App Service plan with the **numberOfWorkers** property set to **1**.</span></span> <span data-ttu-id="49a92-141">Med den här konfigurationen ger högsta densiteten möjliga i den här arbetspool.</span><span class="sxs-lookup"><span data-stu-id="49a92-141">Using this configuration yields the highest density possible on this worker pool.</span></span>
1. <span data-ttu-id="49a92-142">Antalet arbetare kan konfigureras oberoende per app för att bevilja ytterligare resurser som behövs.</span><span class="sxs-lookup"><span data-stu-id="49a92-142">The number of workers can be configured independently per app to grant additional resources as needed.</span></span> <span data-ttu-id="49a92-143">Exempel:</span><span class="sxs-lookup"><span data-stu-id="49a92-143">For example:</span></span>
    - <span data-ttu-id="49a92-144">En hög användning app kan ange **numberOfWorkers** till **3** har flera bearbetningskapacitet för appen.</span><span class="sxs-lookup"><span data-stu-id="49a92-144">A high-use app can set **numberOfWorkers** to **3** to have more processing capacity for that app.</span></span> 
    - <span data-ttu-id="49a92-145">Använd låg appar skulle ange **numberOfWorkers** till **1**.</span><span class="sxs-lookup"><span data-stu-id="49a92-145">Low-use apps would set **numberOfWorkers** to **1**.</span></span>

## <a name="next-steps"></a><span data-ttu-id="49a92-146">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="49a92-146">Next Steps</span></span>

- [<span data-ttu-id="49a92-147">Azure App Service-planer djupgående översikt</span><span class="sxs-lookup"><span data-stu-id="49a92-147">Azure App Service plans in-depth overview</span></span>](azure-web-sites-web-hosting-plans-in-depth-overview.md)
- [<span data-ttu-id="49a92-148">Introduktion till App Service-miljöer</span><span class="sxs-lookup"><span data-stu-id="49a92-148">Introduction to App Service Environment</span></span>](../app-service-web/app-service-app-service-environment-intro.md)