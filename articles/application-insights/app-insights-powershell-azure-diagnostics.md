---
title: "Ställa in Application Insights i en Azure med hjälp av PowerShell | Microsoft Docs"
description: "Automatisera överföringen av Azure Diagnostics-data till Application Insights."
services: application-insights
documentationcenter: .net
author: sbtron
manager: carmonm
ms.assetid: 4ac803a8-f424-4c0c-b18f-4b9c189a64a5
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 11/17/2015
ms.author: bwren
ms.openlocfilehash: 3b6da89cc33cda713b483a2af3cbb493a03d6bec
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/18/2017
---
# <a name="using-powershell-to-set-up-application-insights-for-an-azure-web-app"></a><span data-ttu-id="5936e-103">Ställa in Application Insights i en Azure-webbapp med hjälp av PowerShell</span><span class="sxs-lookup"><span data-stu-id="5936e-103">Using PowerShell to set up Application Insights for an Azure web app</span></span>
<span data-ttu-id="5936e-104">[Microsoft Azure](https://azure.com) kan [konfigureras att skicka Azure Diagnostics-data](app-insights-azure-diagnostics.md) till [Azure Application Insights](app-insights-overview.md).</span><span class="sxs-lookup"><span data-stu-id="5936e-104">[Microsoft Azure](https://azure.com) can be [configured to send Azure Diagnostics](app-insights-azure-diagnostics.md) to [Azure Application Insights](app-insights-overview.md).</span></span> <span data-ttu-id="5936e-105">Diagnostiken gäller Azure Cloud Services och virtuella datorer i Azure.</span><span class="sxs-lookup"><span data-stu-id="5936e-105">The diagnostics relate to Azure Cloud Services and Azure VMs.</span></span> <span data-ttu-id="5936e-106">De kompletterar telemetrin som du skickar inifrån appen med hjälp av Application Insights SDK.</span><span class="sxs-lookup"><span data-stu-id="5936e-106">They complement the telemetry that you send from within the app using the Application Insights SDK.</span></span> <span data-ttu-id="5936e-107">Som en del av automatiseringen av processen för att skapa nya resurser i Azure kan du konfigurera diagnostik med hjälp av PowerShell.</span><span class="sxs-lookup"><span data-stu-id="5936e-107">As part of automating the process of creating new resources in Azure, you can configure diagnostics using PowerShell.</span></span>

## <a name="azure-template"></a><span data-ttu-id="5936e-108">Azure-mall</span><span class="sxs-lookup"><span data-stu-id="5936e-108">Azure template</span></span>
<span data-ttu-id="5936e-109">Om webbappen finns i Azure och du skapar dina resurser med hjälp av en Azure Resource Manager-mall kan du konfigurera Application Insights genom att lägga till följande till resursnoden:</span><span class="sxs-lookup"><span data-stu-id="5936e-109">If the web app is in Azure and you create your resources using an Azure Resource Manager template, you can configure Application Insights by adding this to the resources node:</span></span>

    {
      resources: [
        /* Create Application Insights resource */
        {
          "apiVersion": "2015-05-01",
          "type": "microsoft.insights/components",
          "name": "nameOfAIAppResource",
          "location": "centralus",
          "kind": "web",
          "properties": { "ApplicationId": "nameOfAIAppResource" },
          "dependsOn": [
            "[concat('Microsoft.Web/sites/', myWebAppName)]"
          ]
        }
       ]
     } 

* <span data-ttu-id="5936e-110">`nameOfAIAppResource` – ett namn för Application Insights-resursen</span><span class="sxs-lookup"><span data-stu-id="5936e-110">`nameOfAIAppResource` - a name for the Application Insights resource</span></span>
* <span data-ttu-id="5936e-111">`myWebAppName` – webbappens ID</span><span class="sxs-lookup"><span data-stu-id="5936e-111">`myWebAppName` - the id of the web app</span></span>

## <a name="enable-diagnostics-extension-as-part-of-deploying-a-cloud-service"></a><span data-ttu-id="5936e-112">Aktivera diagnostiktillägget som en del av distributionen av en molntjänst</span><span class="sxs-lookup"><span data-stu-id="5936e-112">Enable diagnostics extension as part of deploying a Cloud Service</span></span>
<span data-ttu-id="5936e-113">`New-AzureDeployment`-cmdleten har en parameter, `ExtensionConfiguration`, som stöder en rad diagnostikkonfigurationer.</span><span class="sxs-lookup"><span data-stu-id="5936e-113">The `New-AzureDeployment` cmdlet has a parameter `ExtensionConfiguration`, which takes an array of diagnostics configurations.</span></span> <span data-ttu-id="5936e-114">Dessa kan skapas med hjälp av cmdleten `New-AzureServiceDiagnosticsExtensionConfig`.</span><span class="sxs-lookup"><span data-stu-id="5936e-114">These can be created using the `New-AzureServiceDiagnosticsExtensionConfig` cmdlet.</span></span> <span data-ttu-id="5936e-115">Till exempel:</span><span class="sxs-lookup"><span data-stu-id="5936e-115">For example:</span></span>

```ps

    $service_package = "CloudService.cspkg"
    $service_config = "ServiceConfiguration.Cloud.cscfg"
    $diagnostics_storagename = "myservicediagnostics"
    $webrole_diagconfigpath = "MyService.WebRole.PubConfig.xml" 
    $workerrole_diagconfigpath = "MyService.WorkerRole.PubConfig.xml"

    $primary_storagekey = (Get-AzureStorageKey `
     -StorageAccountName "$diagnostics_storagename").Primary
    $storage_context = New-AzureStorageContext `
       -StorageAccountName $diagnostics_storagename `
       -StorageAccountKey $primary_storagekey

    $webrole_diagconfig = `
     New-AzureServiceDiagnosticsExtensionConfig `
      -Role "WebRole" -Storage_context $storageContext `
      -DiagnosticsConfigurationPath $webrole_diagconfigpath
    $workerrole_diagconfig = `
     New-AzureServiceDiagnosticsExtensionConfig `
      -Role "WorkerRole" `
      -StorageContext $storage_context `
      -DiagnosticsConfigurationPath $workerrole_diagconfigpath

    New-AzureDeployment `
      -ServiceName $service_name `
      -Slot Production `
      -Package $service_package `
      -Configuration $service_config `
      -ExtensionConfiguration @($webrole_diagconfig,$workerrole_diagconfig)

``` 

## <a name="enable-diagnostics-extension-on-an-existing-cloud-service"></a><span data-ttu-id="5936e-116">Aktivera diagnostiktillägg i en befintlig molntjänst</span><span class="sxs-lookup"><span data-stu-id="5936e-116">Enable diagnostics extension on an existing Cloud Service</span></span>
<span data-ttu-id="5936e-117">I en befintlig tjänst använder du `Set-AzureServiceDiagnosticsExtension`.</span><span class="sxs-lookup"><span data-stu-id="5936e-117">On an existing service, use `Set-AzureServiceDiagnosticsExtension`.</span></span>

```ps

    $service_name = "MyService"
    $diagnostics_storagename = "myservicediagnostics"
    $webrole_diagconfigpath = "MyService.WebRole.PubConfig.xml" 
    $workerrole_diagconfigpath = "MyService.WorkerRole.PubConfig.xml"
    $primary_storagekey = (Get-AzureStorageKey `
         -StorageAccountName "$diagnostics_storagename").Primary
    $storage_context = New-AzureStorageContext `
        -StorageAccountName $diagnostics_storagename `
        -StorageAccountKey $primary_storagekey

    Set-AzureServiceDiagnosticsExtension `
        -StorageContext $storage_context `
        -DiagnosticsConfigurationPath $webrole_diagconfigpath `
        -ServiceName $service_name `
        -Slot Production `
        -Role "WebRole" 
    Set-AzureServiceDiagnosticsExtension `
        -StorageContext $storage_context `
        -DiagnosticsConfigurationPath $workerrole_diagconfigpath `
        -ServiceName $service_name `
        -Slot Production `
        -Role "WorkerRole"
```

## <a name="get-current-diagnostics-extension-configuration"></a><span data-ttu-id="5936e-118">Hämta den aktuella konfigurationen för diagnostiktillägg</span><span class="sxs-lookup"><span data-stu-id="5936e-118">Get current diagnostics extension configuration</span></span>
```ps

    Get-AzureServiceDiagnosticsExtension -ServiceName "MyService"
```


## <a name="remove-diagnostics-extension"></a><span data-ttu-id="5936e-119">Ta bort diagnostiktillägg</span><span class="sxs-lookup"><span data-stu-id="5936e-119">Remove diagnostics extension</span></span>
```ps

    Remove-AzureServiceDiagnosticsExtension -ServiceName "MyService"
```

<span data-ttu-id="5936e-120">Om du aktiverade diagnostiktillägget med `Set-AzureServiceDiagnosticsExtension` eller `New-AzureServiceDiagnosticsExtensionConfig` utan rollparametern så kan du ta bort tillägget med hjälp av `Remove-AzureServiceDiagnosticsExtension` utan rollparametern.</span><span class="sxs-lookup"><span data-stu-id="5936e-120">If you enabled the diagnostics extension using either `Set-AzureServiceDiagnosticsExtension` or `New-AzureServiceDiagnosticsExtensionConfig` without the Role parameter, then you can remove the extension using `Remove-AzureServiceDiagnosticsExtension` without the Role parameter.</span></span> <span data-ttu-id="5936e-121">Om rollparametern användes när du aktiverade tillägget måste den också användas när du tar bort tillägget.</span><span class="sxs-lookup"><span data-stu-id="5936e-121">If the Role parameter was used when enabling the extension then it must also be used when removing the extension.</span></span>

<span data-ttu-id="5936e-122">Så här tar du bort diagnostiktillägget från varje enskild roll:</span><span class="sxs-lookup"><span data-stu-id="5936e-122">To remove the diagnostics extension from each individual role:</span></span>

```ps

    Remove-AzureServiceDiagnosticsExtension -ServiceName "MyService" -Role "WebRole"
```


## <a name="see-also"></a><span data-ttu-id="5936e-123">Se även</span><span class="sxs-lookup"><span data-stu-id="5936e-123">See also</span></span>
* [<span data-ttu-id="5936e-124">Övervaka Azure Cloud Services-appar med Application Insights</span><span class="sxs-lookup"><span data-stu-id="5936e-124">Monitor Azure Cloud Services apps with Application Insights</span></span>](app-insights-cloudservices.md)
* [<span data-ttu-id="5936e-125">Skicka Azure Diagnostics-data till Application Insights</span><span class="sxs-lookup"><span data-stu-id="5936e-125">Send Azure Diagnostics to Application Insights</span></span>](app-insights-azure-diagnostics.md)
* [<span data-ttu-id="5936e-126">Automatisera konfigurationen av aviseringar</span><span class="sxs-lookup"><span data-stu-id="5936e-126">Automate configuring alerts</span></span>](app-insights-powershell-alerts.md)

