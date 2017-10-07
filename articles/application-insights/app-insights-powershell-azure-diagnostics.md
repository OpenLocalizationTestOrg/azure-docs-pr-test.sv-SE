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
# <a name="using-powershell-tooset-up-application-insights-for-an-azure-web-app"></a>Med hjälp av PowerShell tooset in Programinsikter för en Azure-webbapp
[Microsoft Azure](https://azure.com) kan vara [konfigurerats toosend Azure Diagnostics](app-insights-azure-diagnostics.md) för[Azure Application Insights](app-insights-overview.md). hello diagnostik relatera tooAzure molntjänster och virtuella Azure-datorer. De kompletterar hello telemetri som du skickar inifrån hello-app med hello Application Insights SDK. Som en del av automatiserade hello processen att skapa nya resurser i Azure, kan du konfigurera diagnostik med hjälp av PowerShell.

## <a name="azure-template"></a>Azure-mall
Om hello-webbapp i Azure och du kan skapa dina resurser med hjälp av en Azure Resource Manager-mall, kan du konfigurera Application Insights genom att lägga till den här noden för toohello resurser:

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

* `nameOfAIAppResource`-ett namn för hello Application Insights-resurs
* `myWebAppName`-hello-id för hello webbapp

## <a name="enable-diagnostics-extension-as-part-of-deploying-a-cloud-service"></a>Aktivera diagnostiktillägget som en del av distributionen av en molntjänst
Hej `New-AzureDeployment` cmdlet har en parameter `ExtensionConfiguration`, som tar en matris av diagnostik-konfigurationer. De kan skapas med hjälp av hello `New-AzureServiceDiagnosticsExtensionConfig` cmdlet. Exempel:

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

## <a name="enable-diagnostics-extension-on-an-existing-cloud-service"></a>Aktivera diagnostiktillägg i en befintlig molntjänst
I en befintlig tjänst använder du `Set-AzureServiceDiagnosticsExtension`.

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

## <a name="get-current-diagnostics-extension-configuration"></a>Hämta den aktuella konfigurationen för diagnostiktillägg
```ps

    Get-AzureServiceDiagnosticsExtension -ServiceName "MyService"
```


## <a name="remove-diagnostics-extension"></a>Ta bort diagnostiktillägg
```ps

    Remove-AzureServiceDiagnosticsExtension -ServiceName "MyService"
```

Om du har aktiverat hello diagnostik tillägget med hjälp av antingen `Set-AzureServiceDiagnosticsExtension` eller `New-AzureServiceDiagnosticsExtensionConfig` utan hello rollen parametern sedan kan du ta bort hello tillägget med hjälp av `Remove-AzureServiceDiagnosticsExtension` utan hello rollen parametern. Om hello rollen parametern används när du aktiverar hello tillägget måste det också används när du tar bort hello-tillägget.

tooremove hello diagnostik tillägg från varje enskild roll:

```ps

    Remove-AzureServiceDiagnosticsExtension -ServiceName "MyService" -Role "WebRole"
```


## <a name="see-also"></a>Se även
* [Övervaka Azure Cloud Services-appar med Application Insights](app-insights-cloudservices.md)
* [Skicka Azure Diagnostics tooApplication insikter](app-insights-azure-diagnostics.md)
* [Automatisera konfigurationen av aviseringar](app-insights-powershell-alerts.md)

