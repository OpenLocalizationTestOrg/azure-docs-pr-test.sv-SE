---
title: "aaaEnable diagnostik i Azure Cloud Services med hjälp av PowerShell | Microsoft Docs"
description: "Lär dig hur tooenable diagnostik för cloud services med hjälp av PowerShell"
services: cloud-services
documentationcenter: .net
author: Thraka
manager: timlt
editor: 
ms.assetid: 66e08754-8639-4022-ae18-4237749ba17d
ms.service: cloud-services
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 09/06/2016
ms.author: adegeo
ms.openlocfilehash: 7c7444df13edc8d7f5663e20ec7558d36aac45d4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="enable-diagnostics-in-azure-cloud-services-using-powershell"></a>Aktivera diagnostik i Azure Cloud Services med hjälp av PowerShell
Du kan samla in diagnostikdata som programloggarna, prestandaräknare etc. från en molnbaserad tjänst som använder hello Azure Diagnostics tillägg. Den här artikeln beskriver hur tooenable hello Azure Diagnostics tillägget för en tjänst i molnet med hjälp av PowerShell.  Se [hur tooinstall och konfigurera Azure PowerShell](/powershell/azure/overview) för hello kraven för den här artikeln.

## <a name="enable-diagnostics-extension-as-part-of-deploying-a-cloud-service"></a>Aktivera diagnostiktillägget som en del av distributionen av en molntjänst
Den här metoden är tillämpliga toocontinuous integrering scenarier där hello diagnostik tillägget kan aktiveras som en del av distribuera hello tjänst i molnet. När du skapar en ny molntjänst-distribution kan du aktivera hello diagnostik tillägg genom att passera i hello *ExtensionConfiguration* parametern toohello [ny AzureDeployment](/powershell/module/azure/new-azuredeployment?view=azuresmps-3.7.0) cmdlet. Hej *ExtensionConfiguration* parameter tar en matris av diagnostik-konfigurationer som kan skapas med hjälp av hello [ny AzureServiceDiagnosticsExtensionConfig](/powershell/module/azure/new-azureservicediagnosticsextensionconfig?view=azuresmps-3.7.0) cmdlet.

hello följande exempel visas hur du kan aktivera diagnostik för en molnbaserad tjänst med en WebRole och WorkerRole, var och en med en annan diagnostik-konfiguration.

```powershell
$service_name = "MyService"
$service_package = "CloudService.cspkg"
$service_config = "ServiceConfiguration.Cloud.cscfg"
$webrole_diagconfigpath = "MyService.WebRole.PubConfig.xml"
$workerrole_diagconfigpath = "MyService.WorkerRole.PubConfig.xml"

$webrole_diagconfig = New-AzureServiceDiagnosticsExtensionConfig -Role "WebRole" -DiagnosticsConfigurationPath $webrole_diagconfigpath
$workerrole_diagconfig = New-AzureServiceDiagnosticsExtensionConfig -Role "WorkerRole" -DiagnosticsConfigurationPath $workerrole_diagconfigpath

New-AzureDeployment -ServiceName $service_name -Slot Production -Package $service_package -Configuration $service_config -ExtensionConfiguration @($webrole_diagconfig,$workerrole_diagconfig)
```

Om hello diagnostik konfigurationsfil anger en `StorageAccount` element med namnet på ett lagringskonto, sedan hello `New-AzureServiceDiagnosticsExtensionConfig` cmdlet används automatiskt som lagringskontot. För den här toowork hello lagringskonto måste toobe i hello samma prenumeration som hello Molntjänsten som distribueras.

Från Azure SDK 2.6 konfigurationsfiler för vidare hello genereras av hello MSBuild publicera innehåller utdata hello lagringskontonamn baserat på hello diagnostik konfigurationssträngen i hello tjänstekonfigurationsfilen (.cscfg). hello-skriptet nedan visar hur tooparse hello konfigurationsfiler från hello publicera utdata och konfigurera diagnostik för varje roll när du distribuerar hello-Molntjänsten.

```powershell
$service_name = "MyService"
$service_package = "C:\build\output\CloudService.cspkg"
$service_config = "C:\build\output\ServiceConfiguration.Cloud.cscfg"

#Find hello Extensions path based on service configuration file
$extensionsSearchPath = Join-Path -Path (Split-Path -Parent $service_config) -ChildPath "Extensions"

$diagnosticsExtensions = Get-ChildItem -Path $extensionsSearchPath -Filter "PaaSDiagnostics.*.PubConfig.xml"
$diagnosticsConfigurations = @()
foreach ($extPath in $diagnosticsExtensions)
{
    #Find hello RoleName based on file naming convention PaaSDiagnostics.<RoleName>.PubConfig.xml
    $roleName = ""
    $roles = $extPath -split ".",0,"simplematch"
    if ($roles -is [system.array] -and $roles.Length -gt 1)
    {
        $roleName = $roles[1]
        $x = 2
        while ($x -le $roles.Length)
            {
               if ($roles[$x] -ne "PubConfig")
                {
                    $roleName = $roleName + "." + $roles[$x]
                }
                else
                {
                    break
                }
                $x++
            }
        $fullExtPath = Join-Path -path $extensionsSearchPath -ChildPath $extPath
        $diagnosticsconfig = New-AzureServiceDiagnosticsExtensionConfig -Role $roleName -DiagnosticsConfigurationPath $fullExtPath
        $diagnosticsConfigurations += $diagnosticsconfig
    }
}
New-AzureDeployment -ServiceName $service_name -Slot Production -Package $service_package -Configuration $service_config -ExtensionConfiguration $diagnosticsConfigurations
```

Visual Studio Online använder ett liknande sätt för automatisk distribution av molntjänster med hello diagnostik tillägg. Se [publicera AzureCloudDeployment.ps1](https://github.com/Microsoft/vso-agent-tasks/blob/master/Tasks/AzureCloudPowerShellDeployment/Publish-AzureCloudDeployment.ps1) för en komplett exempel.

Om inget `StorageAccount` har angetts i hello diagnostik konfigurationen måste toopass i hello *StorageAccountName* parametern toohello cmdlet. Om hello *StorageAccountName* parametern anges, kommer hello cmdlet alltid använda hello storage-konto som har angetts i parametern hello och inte hello ett som har angetts i konfigurationsfilen för hello diagnostik.

Om hello diagnostik storage-konto är i en annan prenumeration från hello molnbaserad tjänst bör tooexplicitly måste skicka in hello *StorageAccountName* och *StorageAccountKey* parametrar toohello cmdlet. Hej *StorageAccountKey* parametern behövs inte när hello diagnostik storage-konto är i hello samma prenumeration som hello cmdlet automatiskt kan fråga och ange hello nyckelvärdet när du aktiverar hello diagnostik tillägg. Om hello diagnostiklagringskonto finns i en annan prenumeration och sedan hello cmdlet kanske inte kan tooget hello nyckeln automatiskt och du behöver tooexplicitly ange hello nyckel via hello *StorageAccountKey* parameter.

```powershell
$webrole_diagconfig = New-AzureServiceDiagnosticsExtensionConfig -Role "WebRole" -DiagnosticsConfigurationPath $webrole_diagconfigpath -StorageAccountName $diagnosticsstorage_name -StorageAccountKey $diagnosticsstorage_key
$workerrole_diagconfig = New-AzureServiceDiagnosticsExtensionConfig -Role "WorkerRole" -DiagnosticsConfigurationPath $workerrole_diagconfigpath -StorageAccountName $diagnosticsstorage_name -StorageAccountKey $diagnosticsstorage_key
```

## <a name="enable-diagnostics-extension-on-an-existing-cloud-service"></a>Aktivera diagnostiktillägg i en befintlig molntjänst
Du kan använda hello [Set AzureServiceDiagnosticsExtension](/powershell/module/azure/set-azureservicediagnosticsextension?view=azuresmps-3.7.0) cmdlet tooenable eller uppdatera diagnostik-konfiguration på en molntjänst som redan körs.

[!INCLUDE [cloud-services-wad-warning](../../includes/cloud-services-wad-warning.md)]

```powershell
$service_name = "MyService"
$webrole_diagconfigpath = "MyService.WebRole.PubConfig.xml"
$workerrole_diagconfigpath = "MyService.WorkerRole.PubConfig.xml"

$webrole_diagconfig = New-AzureServiceDiagnosticsExtensionConfig -Role "WebRole" -DiagnosticsConfigurationPath $webrole_diagconfigpath
$workerrole_diagconfig = New-AzureServiceDiagnosticsExtensionConfig -Role "WorkerRole" -DiagnosticsConfigurationPath $workerrole_diagconfigpath

Set-AzureServiceDiagnosticsExtension -DiagnosticsConfiguration @($webrole_diagconfig,$workerrole_diagconfig) -ServiceName $service_name
```

## <a name="get-current-diagnostics-extension-configuration"></a>Hämta den aktuella konfigurationen för diagnostiktillägg
Använd hello [Get-AzureServiceDiagnosticsExtension](/powershell/module/azure/get-azureservicediagnosticsextension?view=azuresmps-3.7.0) cmdlet tooget hello aktuella diagnostik konfigurationen för en tjänst i molnet.

```powershell
Get-AzureServiceDiagnosticsExtension -ServiceName "MyService"
```

## <a name="remove-diagnostics-extension"></a>Ta bort diagnostiktillägg
tooturn av diagnostik i ett moln-tjänst du kan använda hello [ta bort AzureServiceDiagnosticsExtension](/powershell/module/azure/remove-azureservicediagnosticsextension?view=azuresmps-3.7.0) cmdlet.

```powershell
Remove-AzureServiceDiagnosticsExtension -ServiceName "MyService"
```

Om du har aktiverat hello diagnostik tillägget med hjälp av antingen *Set AzureServiceDiagnosticsExtension* eller hello *ny AzureServiceDiagnosticsExtensionConfig* utan hello *roll* parameter och du kan ta bort hello tillägget med hjälp av *ta bort AzureServiceDiagnosticsExtension* utan hello *rollen* parameter. Om hello *rollen* parametern används när du aktiverar tillägget Hej då måste användas när du tar bort hello tillägget.

tooremove hello diagnostik tillägg från varje enskild roll:

```powershell
Remove-AzureServiceDiagnosticsExtension -ServiceName "MyService" -Role "WebRole"
```

## <a name="next-steps"></a>Nästa steg
* Mer information om hur du använder Azure-diagnostik och andra tekniker tootroubleshoot problem finns [aktiverar diagnostik i Azure-molntjänster och virtuella datorer](cloud-services-dotnet-diagnostics.md).
* Hej [diagnostik Konfigurationsschemat](https://msdn.microsoft.com/library/azure/dn782207.aspx) förklarar hello olika konfigurationer för XML-alternativ för hello diagnostik tillägg.
* hur tooenable hello diagnostik tillägg för virtuella datorer, se toolearn [skapa en Windows virtuell dator med övervakning och diagnostik med Azure Resource Manager-mall](../virtual-machines/windows/extensions-diagnostics-template.md)
