---
title: aaaUsing PowerShell toosetup Application Insights i en Azure | Microsoft Docs
description: Automatisera konfigurera Azure-diagnostik toopipe tooApplication insikter.
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
ms.openlocfilehash: c48a5d8eb23df162522860935af876063aaa6976
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="using-powershell-tooset-up-application-insights-for-an-azure-web-app"></a><span data-ttu-id="99438-103">Med hjälp av PowerShell tooset in Programinsikter för en Azure-webbapp</span><span class="sxs-lookup"><span data-stu-id="99438-103">Using PowerShell tooset up Application Insights for an Azure web app</span></span>
<span data-ttu-id="99438-104">[Microsoft Azure](https://azure.com) kan vara [konfigurerats toosend Azure Diagnostics](app-insights-azure-diagnostics.md) för[Azure Application Insights](app-insights-overview.md).</span><span class="sxs-lookup"><span data-stu-id="99438-104">[Microsoft Azure](https://azure.com) can be [configured toosend Azure Diagnostics](app-insights-azure-diagnostics.md) too[Azure Application Insights](app-insights-overview.md).</span></span> <span data-ttu-id="99438-105">hello diagnostik relatera tooAzure molntjänster och virtuella Azure-datorer.</span><span class="sxs-lookup"><span data-stu-id="99438-105">hello diagnostics relate tooAzure Cloud Services and Azure VMs.</span></span> <span data-ttu-id="99438-106">De kompletterar hello telemetri som du skickar inifrån hello-app med hello Application Insights SDK.</span><span class="sxs-lookup"><span data-stu-id="99438-106">They complement hello telemetry that you send from within hello app using hello Application Insights SDK.</span></span> <span data-ttu-id="99438-107">Som en del av automatiserade hello processen att skapa nya resurser i Azure, kan du konfigurera diagnostik med hjälp av PowerShell.</span><span class="sxs-lookup"><span data-stu-id="99438-107">As part of automating hello process of creating new resources in Azure, you can configure diagnostics using PowerShell.</span></span>

## <a name="azure-template"></a><span data-ttu-id="99438-108">Azure-mall</span><span class="sxs-lookup"><span data-stu-id="99438-108">Azure template</span></span>
<span data-ttu-id="99438-109">Om hello-webbapp i Azure och du kan skapa dina resurser med hjälp av en Azure Resource Manager-mall, kan du konfigurera Application Insights genom att lägga till den här noden för toohello resurser:</span><span class="sxs-lookup"><span data-stu-id="99438-109">If hello web app is in Azure and you create your resources using an Azure Resource Manager template, you can configure Application Insights by adding this toohello resources node:</span></span>

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

* <span data-ttu-id="99438-110">`nameOfAIAppResource`-ett namn för hello Application Insights-resurs</span><span class="sxs-lookup"><span data-stu-id="99438-110">`nameOfAIAppResource` - a name for hello Application Insights resource</span></span>
* <span data-ttu-id="99438-111">`myWebAppName`-hello-id för hello webbapp</span><span class="sxs-lookup"><span data-stu-id="99438-111">`myWebAppName` - hello id of hello web app</span></span>

## <a name="enable-diagnostics-extension-as-part-of-deploying-a-cloud-service"></a><span data-ttu-id="99438-112">Aktivera diagnostiktillägget som en del av distributionen av en molntjänst</span><span class="sxs-lookup"><span data-stu-id="99438-112">Enable diagnostics extension as part of deploying a Cloud Service</span></span>
<span data-ttu-id="99438-113">Hej `New-AzureDeployment` cmdlet har en parameter `ExtensionConfiguration`, som tar en matris av diagnostik-konfigurationer.</span><span class="sxs-lookup"><span data-stu-id="99438-113">hello `New-AzureDeployment` cmdlet has a parameter `ExtensionConfiguration`, which takes an array of diagnostics configurations.</span></span> <span data-ttu-id="99438-114">De kan skapas med hjälp av hello `New-AzureServiceDiagnosticsExtensionConfig` cmdlet.</span><span class="sxs-lookup"><span data-stu-id="99438-114">These can be created using hello `New-AzureServiceDiagnosticsExtensionConfig` cmdlet.</span></span> <span data-ttu-id="99438-115">Exempel:</span><span class="sxs-lookup"><span data-stu-id="99438-115">For example:</span></span>

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

## <a name="enable-diagnostics-extension-on-an-existing-cloud-service"></a><span data-ttu-id="99438-116">Aktivera diagnostiktillägg i en befintlig molntjänst</span><span class="sxs-lookup"><span data-stu-id="99438-116">Enable diagnostics extension on an existing Cloud Service</span></span>
<span data-ttu-id="99438-117">I en befintlig tjänst använder du `Set-AzureServiceDiagnosticsExtension`.</span><span class="sxs-lookup"><span data-stu-id="99438-117">On an existing service, use `Set-AzureServiceDiagnosticsExtension`.</span></span>

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

## <a name="get-current-diagnostics-extension-configuration"></a><span data-ttu-id="99438-118">Hämta den aktuella konfigurationen för diagnostiktillägg</span><span class="sxs-lookup"><span data-stu-id="99438-118">Get current diagnostics extension configuration</span></span>
```ps

    Get-AzureServiceDiagnosticsExtension -ServiceName "MyService"
```


## <a name="remove-diagnostics-extension"></a><span data-ttu-id="99438-119">Ta bort diagnostiktillägg</span><span class="sxs-lookup"><span data-stu-id="99438-119">Remove diagnostics extension</span></span>
```ps

    Remove-AzureServiceDiagnosticsExtension -ServiceName "MyService"
```

<span data-ttu-id="99438-120">Om du har aktiverat hello diagnostik tillägget med hjälp av antingen `Set-AzureServiceDiagnosticsExtension` eller `New-AzureServiceDiagnosticsExtensionConfig` utan hello rollen parametern sedan kan du ta bort hello tillägget med hjälp av `Remove-AzureServiceDiagnosticsExtension` utan hello rollen parametern.</span><span class="sxs-lookup"><span data-stu-id="99438-120">If you enabled hello diagnostics extension using either `Set-AzureServiceDiagnosticsExtension` or `New-AzureServiceDiagnosticsExtensionConfig` without hello Role parameter, then you can remove hello extension using `Remove-AzureServiceDiagnosticsExtension` without hello Role parameter.</span></span> <span data-ttu-id="99438-121">Om hello rollen parametern används när du aktiverar hello tillägget måste det också används när du tar bort hello-tillägget.</span><span class="sxs-lookup"><span data-stu-id="99438-121">If hello Role parameter was used when enabling hello extension then it must also be used when removing hello extension.</span></span>

<span data-ttu-id="99438-122">tooremove hello diagnostik tillägg från varje enskild roll:</span><span class="sxs-lookup"><span data-stu-id="99438-122">tooremove hello diagnostics extension from each individual role:</span></span>

```ps

    Remove-AzureServiceDiagnosticsExtension -ServiceName "MyService" -Role "WebRole"
```


## <a name="see-also"></a><span data-ttu-id="99438-123">Se även</span><span class="sxs-lookup"><span data-stu-id="99438-123">See also</span></span>
* [<span data-ttu-id="99438-124">Övervaka Azure Cloud Services-appar med Application Insights</span><span class="sxs-lookup"><span data-stu-id="99438-124">Monitor Azure Cloud Services apps with Application Insights</span></span>](app-insights-cloudservices.md)
* [<span data-ttu-id="99438-125">Skicka Azure Diagnostics tooApplication insikter</span><span class="sxs-lookup"><span data-stu-id="99438-125">Send Azure Diagnostics tooApplication Insights</span></span>](app-insights-azure-diagnostics.md)
* [<span data-ttu-id="99438-126">Automatisera konfigurationen av aviseringar</span><span class="sxs-lookup"><span data-stu-id="99438-126">Automate configuring alerts</span></span>](app-insights-powershell-alerts.md)

