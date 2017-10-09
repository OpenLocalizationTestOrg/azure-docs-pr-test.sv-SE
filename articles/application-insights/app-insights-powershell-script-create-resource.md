---
title: aaaPowerShell skript toocreate Application Insights-resurs | Microsoft Docs
description: Automatisera skapandet av Application Insights-resurser.
services: application-insights
documentationcenter: windows
author: CFreemanwa
manager: carmonm
ms.assetid: f0082c9b-43ad-4576-a417-4ea8e0daf3d9
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 11/19/2016
ms.author: bwren
ms.openlocfilehash: 2ac00376d38026d64c2c5deabfaca60588924510
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="powershell-script-toocreate-an-application-insights-resource"></a>PowerShell-skriptet toocreate Application Insights-resurs


När du vill toomonitor ett nytt program- eller en ny version av ett program - med [Azure Application Insights](https://azure.microsoft.com/services/application-insights/), du ställer in en ny resurs i Microsoft Azure. Den här resursen är där hello telemetridata från din app analyseras och visas. 

Du kan automatisera hello skapandet av en ny resurs med hjälp av PowerShell.

Till exempel om du utvecklar en app för mobila enheter, är förmodligen, när som helst, kommer att flera publicerade versioner av appen används av dina kunder. Vill du inte tooget hello telemetri resultat från olika versioner upp. Så får du din build processen toocreate en ny resurs för varje version.

> [!NOTE]
> Om du vill toocreate en uppsättning resurser på hello samma tid bör du överväga att [skapar hello resurser med hjälp av en Azure-mall](app-insights-powershell.md).
> 
> 

## <a name="script-toocreate-an-application-insights-resource"></a>Skriptet toocreate Application Insights-resurs
Se hello relevanta cmdlet specifikationer:

* [Ny AzureRmResource](https://msdn.microsoft.com/library/mt652510.aspx)
* [New-AzureRmRoleAssignment](https://msdn.microsoft.com/library/mt678995.aspx)

*PowerShell-skript*  

```PowerShell


###########################################
# Set Values
###########################################

# If running manually, uncomment before hello first 
# execution toologin toohello Azure Portal:

# Add-AzureRmAccount / Login-AzureRmAccount

# Set hello name of hello Application Insights Resource

$appInsightsName = "TestApp"

# Set hello application name used for hello value of hello Tag "AppInsightsApp" 

$applicationTagName = "MyApp"

# Set hello name of hello Resource Group toouse.  
# Default is hello application name.
$resourceGroupName = "MyAppResourceGroup"

###################################################
# Create hello Resource and Output hello name and iKey
###################################################

# Select hello azure subscription
Select-AzureSubscription -SubscriptionName "MySubscription"

# Create hello App Insights Resource


$resource = New-AzureRmResource `
  -ResourceName $appInsightsName `
  -ResourceGroupName $resourceGroupName `
  -Tag @{ applicationType = "web"; applicationName = $applicationTagName} `
  -ResourceType "Microsoft.Insights/components" `
  -Location "East US" `  # or North Europe, West Europe, South Central US
  -PropertyObject @{"Application_Type"="web"} `
  -Force

# Give owner access toohello team

New-AzureRmRoleAssignment `
  -SignInName "myteam@fabrikam.com" `
  -RoleDefinitionName Owner `
  -Scope $resource.ResourceId 


# Display iKey
Write-Host "App Insights Name = " $resource.Name
Write-Host "IKey = " $resource.Properties.InstrumentationKey

```

## <a name="what-toodo-with-hello-ikey"></a>Vilka toodo med hello iKey
Varje resurs identifieras av dess instrumentation nyckel (iKey). Hej iKey är en utdata från skriptet hello resurs. Skapa skriptet bör ge hello iKey toohello Application Insights SDK inbäddade i appen.

Det finns två sätt toomake hello iKey tillgängliga toohello SDK:

* I [ApplicationInsights.config](app-insights-configuration-with-applicationinsights-config.md): 
  * `<instrumentationkey>`*ikey*`</instrumentationkey>`
* Eller i [initieringskod](app-insights-api-custom-events-metrics.md): 
  * `Microsoft.ApplicationInsights.Extensibility.
    TelemetryConfiguration.Active.InstrumentationKey = "`*iKey*`";`

## <a name="see-also"></a>Se även
* [Skapa Application Insights och testa webbresurser från mallar](app-insights-powershell.md)
* [Konfigurera övervakning av Azure diagnostics med PowerShell](app-insights-powershell-azure-diagnostics.md) 
* [Ange aviseringar med hjälp av PowerShell](app-insights-powershell-alerts.md)

