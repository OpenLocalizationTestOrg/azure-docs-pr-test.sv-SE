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
# <a name="powershell-script-toocreate-an-application-insights-resource"></a><span data-ttu-id="d4a20-103">PowerShell-skriptet toocreate Application Insights-resurs</span><span class="sxs-lookup"><span data-stu-id="d4a20-103">PowerShell script toocreate an Application Insights resource</span></span>


<span data-ttu-id="d4a20-104">När du vill toomonitor ett nytt program- eller en ny version av ett program - med [Azure Application Insights](https://azure.microsoft.com/services/application-insights/), du ställer in en ny resurs i Microsoft Azure.</span><span class="sxs-lookup"><span data-stu-id="d4a20-104">When you want toomonitor a new application - or a new version of an application - with [Azure Application Insights](https://azure.microsoft.com/services/application-insights/), you set up a new resource in Microsoft Azure.</span></span> <span data-ttu-id="d4a20-105">Den här resursen är där hello telemetridata från din app analyseras och visas.</span><span class="sxs-lookup"><span data-stu-id="d4a20-105">This resource is where hello telemetry data from your app is analyzed and displayed.</span></span> 

<span data-ttu-id="d4a20-106">Du kan automatisera hello skapandet av en ny resurs med hjälp av PowerShell.</span><span class="sxs-lookup"><span data-stu-id="d4a20-106">You can automate hello creation of a new resource by using PowerShell.</span></span>

<span data-ttu-id="d4a20-107">Till exempel om du utvecklar en app för mobila enheter, är förmodligen, när som helst, kommer att flera publicerade versioner av appen används av dina kunder.</span><span class="sxs-lookup"><span data-stu-id="d4a20-107">For example, if you are developing a mobile device app, it's likely that, at any time, there will be several published versions of your app in use by your customers.</span></span> <span data-ttu-id="d4a20-108">Vill du inte tooget hello telemetri resultat från olika versioner upp.</span><span class="sxs-lookup"><span data-stu-id="d4a20-108">You don't want tooget hello telemetry results from different versions mixed up.</span></span> <span data-ttu-id="d4a20-109">Så får du din build processen toocreate en ny resurs för varje version.</span><span class="sxs-lookup"><span data-stu-id="d4a20-109">So you get your build process toocreate a new resource for each build.</span></span>

> [!NOTE]
> <span data-ttu-id="d4a20-110">Om du vill toocreate en uppsättning resurser på hello samma tid bör du överväga att [skapar hello resurser med hjälp av en Azure-mall](app-insights-powershell.md).</span><span class="sxs-lookup"><span data-stu-id="d4a20-110">If you want toocreate a set of resources all at hello same time, consider [creating hello resources using an Azure template](app-insights-powershell.md).</span></span>
> 
> 

## <a name="script-toocreate-an-application-insights-resource"></a><span data-ttu-id="d4a20-111">Skriptet toocreate Application Insights-resurs</span><span class="sxs-lookup"><span data-stu-id="d4a20-111">Script toocreate an Application Insights resource</span></span>
<span data-ttu-id="d4a20-112">Se hello relevanta cmdlet specifikationer:</span><span class="sxs-lookup"><span data-stu-id="d4a20-112">See hello relevant cmdlet specs:</span></span>

* [<span data-ttu-id="d4a20-113">Ny AzureRmResource</span><span class="sxs-lookup"><span data-stu-id="d4a20-113">New-AzureRmResource</span></span>](https://msdn.microsoft.com/library/mt652510.aspx)
* [<span data-ttu-id="d4a20-114">New-AzureRmRoleAssignment</span><span class="sxs-lookup"><span data-stu-id="d4a20-114">New-AzureRmRoleAssignment</span></span>](https://msdn.microsoft.com/library/mt678995.aspx)

<span data-ttu-id="d4a20-115">*PowerShell-skript*</span><span class="sxs-lookup"><span data-stu-id="d4a20-115">*PowerShell Script*</span></span>  

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

## <a name="what-toodo-with-hello-ikey"></a><span data-ttu-id="d4a20-116">Vilka toodo med hello iKey</span><span class="sxs-lookup"><span data-stu-id="d4a20-116">What toodo with hello iKey</span></span>
<span data-ttu-id="d4a20-117">Varje resurs identifieras av dess instrumentation nyckel (iKey).</span><span class="sxs-lookup"><span data-stu-id="d4a20-117">Each resource is identified by its instrumentation key (iKey).</span></span> <span data-ttu-id="d4a20-118">Hej iKey är en utdata från skriptet hello resurs.</span><span class="sxs-lookup"><span data-stu-id="d4a20-118">hello iKey is an output of hello resource creation script.</span></span> <span data-ttu-id="d4a20-119">Skapa skriptet bör ge hello iKey toohello Application Insights SDK inbäddade i appen.</span><span class="sxs-lookup"><span data-stu-id="d4a20-119">Your build script should provide hello iKey toohello Application Insights SDK embedded in your app.</span></span>

<span data-ttu-id="d4a20-120">Det finns två sätt toomake hello iKey tillgängliga toohello SDK:</span><span class="sxs-lookup"><span data-stu-id="d4a20-120">There are two ways toomake hello iKey available toohello SDK:</span></span>

* <span data-ttu-id="d4a20-121">I [ApplicationInsights.config](app-insights-configuration-with-applicationinsights-config.md):</span><span class="sxs-lookup"><span data-stu-id="d4a20-121">In [ApplicationInsights.config](app-insights-configuration-with-applicationinsights-config.md):</span></span> 
  * <span data-ttu-id="d4a20-122">`<instrumentationkey>`*ikey*`</instrumentationkey>`</span><span class="sxs-lookup"><span data-stu-id="d4a20-122">`<instrumentationkey>`*ikey*`</instrumentationkey>`</span></span>
* <span data-ttu-id="d4a20-123">Eller i [initieringskod](app-insights-api-custom-events-metrics.md):</span><span class="sxs-lookup"><span data-stu-id="d4a20-123">Or in [initialization code](app-insights-api-custom-events-metrics.md):</span></span> 
  * <span data-ttu-id="d4a20-124">`Microsoft.ApplicationInsights.Extensibility.
    TelemetryConfiguration.Active.InstrumentationKey = "`*iKey*`";`</span><span class="sxs-lookup"><span data-stu-id="d4a20-124">`Microsoft.ApplicationInsights.Extensibility.
TelemetryConfiguration.Active.InstrumentationKey = "`*iKey*`";`</span></span>

## <a name="see-also"></a><span data-ttu-id="d4a20-125">Se även</span><span class="sxs-lookup"><span data-stu-id="d4a20-125">See also</span></span>
* [<span data-ttu-id="d4a20-126">Skapa Application Insights och testa webbresurser från mallar</span><span class="sxs-lookup"><span data-stu-id="d4a20-126">Create Application Insights and web test resources from templates</span></span>](app-insights-powershell.md)
* [<span data-ttu-id="d4a20-127">Konfigurera övervakning av Azure diagnostics med PowerShell</span><span class="sxs-lookup"><span data-stu-id="d4a20-127">Set up monitoring of Azure diagnostics with PowerShell</span></span>](app-insights-powershell-azure-diagnostics.md) 
* [<span data-ttu-id="d4a20-128">Ange aviseringar med hjälp av PowerShell</span><span class="sxs-lookup"><span data-stu-id="d4a20-128">Set alerts by using PowerShell</span></span>](app-insights-powershell-alerts.md)

