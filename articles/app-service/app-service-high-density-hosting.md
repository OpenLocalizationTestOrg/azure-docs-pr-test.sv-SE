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
# <a name="high-density-hosting-on-azure-app-service"></a>Hög densitet värd i Azure App Service
När du använder App Service, frikopplas programmet hello kapacitet som tilldelas tooit av två begrepp:

* **hello program:** representerar hello appen och dess runtime-konfiguration. Innehåller till exempel hello version av .NET som hello runtime ska läsas in, hello app-inställningar.
* **Hej Apptjänstplan:** definierar hello egenskaper hello kapacitet, tillgängliga funktioner och plats för hello program. Egenskaper kan till exempel vara stora (fyra kärnor)-dator, fyra instanser, Premium-funktioner i östra USA.

En app är alltid länkade tooan App Service-plan, men en apptjänstplan kan ge kapacitet tooone eller fler appar.

Därför hello-plattformen ger hello flexibilitet tooisolate en enda app eller har flera appar som delar resurser genom att dela en App Service-plan.

När flera appar delar en apptjänstplan, kör en instans av appen på varje förekomst av den App Service-planen.

## <a name="per-app-scaling"></a>Per app skalning
*Per app skalning* är en funktion som kan aktiveras på nivån för App Service-plan och sedan används per program.

Per app skalas skalning appen oberoende av App Service-plan som är värd för den. På så sätt kan en Apptjänst planen kan skalas too10 instanser, men bara fem toouse kan anges till en app.

   >[!NOTE]
   >Per app skalning är bara tillgängligt för **Premium** SKU App Service-planer
   >

### <a name="per-app-scaling-using-powershell"></a>Per app skalning med PowerShell

Du kan skapa en plan som konfigurerats som en *Per app skalning* plan genom att passera i hello ```-perSiteScaling $true``` attributet toohello ```New-AzureRmAppServicePlan``` cmdleten igen

```
New-AzureRmAppServicePlan -ResourceGroupName $ResourceGroup -Name $AppServicePlan `
                            -Location $Location `
                            -Tier Premium -WorkerSize Small `
                            -NumberofWorkers 5 -PerSiteScaling $true
```

Om du vill tooupdate en befintlig App Service-plan toouse funktionen: 

- Hämta hello mål plan```Get-AzureRmAppServicePlan```
- Ändra hello egenskapen lokalt```$newASP.PerSiteScaling = $true```
- Skicka dina ändringar tillbaka tooazure```Set-AzureRmAppServicePlan``` 

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

På hello app-nivå behöver vi tooconfigure hello antal instanser hello app kan använda i hello app service-plan.

I hello exemplet nedan hello appen är begränsad tootwo instanser oavsett hur många instanser hello underliggande app service-plan skalbar ut.

```
# Get hello app we want tooconfigure toouse "PerSiteScaling"
$newapp = Get-AzureRmWebApp -ResourceGroupName $ResourceGroup -Name $webapp
    
# Modify hello NumberOfWorkers setting toohello desired value.
$newapp.SiteConfig.NumberOfWorkers = 2
    
# Post updated app back tooazure
Set-AzureRmWebApp $newapp
```

> [!IMPORTANT]
> $newapp. SiteConfig.NumberOfWorkers är olika formulär $newapp. MaxNumberOfWorkers. Per app använder skalning $newapp. SiteConfig.NumberOfWorkers toodetermine hello skala egenskaper hello app.

### <a name="per-app-scaling-using-azure-resource-manager"></a>Per app skalning med Azure Resource Manager

hello följande *Azure Resource Manager-mall* skapar:

- En apptjänstplan utskalad too10 instanser
- en app som har konfigurerats tooscale tooa maximalt fem instanser.

Hej programtjänstplanen inställningen hello **PerSiteScaling** egenskapen tootrue ```"perSiteScaling": true```. hello app inställningen hello **antal arbetare** toouse too5 ```"properties": { "numberOfWorkers": "5" }```.

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

## <a name="recommended-configuration-for-high-density-hosting"></a>Rekommenderad konfiguration för hög densitet värd
Är en funktion som är aktiverad i både globala Azure-regioner och Apptjänstmiljöer per app skalning. Du bör dock hello strategi är att använda Apptjänstmiljöer tootake nytta av de avancerade funktionerna och hello större pooler med kapacitet.  

Följ dessa steg tooconfigure hög densitet värd för dina appar:

1. Konfigurera hello Apptjänst-miljön och välj en arbetspool som är dedikerad toohello hög densitet som värd för scenariot.
1. Skapa en enkel App Service-plan och skala den toouse alla hello tillgänglig kapacitet i hello arbetspool.
1. Ange hello PerSiteScaling flaggan tootrue på hello App Service-plan.
1. Nya appar skapas och tilldelas toothat App Service-plan med de **numberOfWorkers** egenskapsuppsättning för**1**. Med den här konfigurationen ger hello högsta densitet möjliga i den här arbetspool.
1. hello antalet arbetare kan konfigureras oberoende per app toogrant ytterligare resurser efter behov. Exempel:
    - En hög användning app kan ange **numberOfWorkers** för**3** toohave mer bearbetning av kapacitet för appen. 
    - Använd låg appar skulle ange **numberOfWorkers** för**1**.

## <a name="next-steps"></a>Nästa steg

- [Azure App Service-planer djupgående översikt](azure-web-sites-web-hosting-plans-in-depth-overview.md)
- [Introduktion tooApp-miljö](../app-service-web/app-service-app-service-environment-intro.md)
