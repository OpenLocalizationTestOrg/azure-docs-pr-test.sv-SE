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
# <a name="enable-diagnostics-in-azure-cloud-services-using-powershell"></a><span data-ttu-id="d3c1e-103">Aktivera diagnostik i Azure Cloud Services med hjälp av PowerShell</span><span class="sxs-lookup"><span data-stu-id="d3c1e-103">Enable diagnostics in Azure Cloud Services using PowerShell</span></span>
<span data-ttu-id="d3c1e-104">Du kan samla in diagnostikdata som programloggarna, prestandaräknare etc. från en molnbaserad tjänst som använder hello Azure Diagnostics tillägg.</span><span class="sxs-lookup"><span data-stu-id="d3c1e-104">You can collect diagnostic data like application logs, performance counters etc. from a Cloud Service using hello Azure Diagnostics extension.</span></span> <span data-ttu-id="d3c1e-105">Den här artikeln beskriver hur tooenable hello Azure Diagnostics tillägget för en tjänst i molnet med hjälp av PowerShell.</span><span class="sxs-lookup"><span data-stu-id="d3c1e-105">This article describes how tooenable hello Azure Diagnostics extension for a Cloud Service using PowerShell.</span></span>  <span data-ttu-id="d3c1e-106">Se [hur tooinstall och konfigurera Azure PowerShell](/powershell/azure/overview) för hello kraven för den här artikeln.</span><span class="sxs-lookup"><span data-stu-id="d3c1e-106">See [How tooinstall and configure Azure PowerShell](/powershell/azure/overview) for hello prerequisites needed for this article.</span></span>

## <a name="enable-diagnostics-extension-as-part-of-deploying-a-cloud-service"></a><span data-ttu-id="d3c1e-107">Aktivera diagnostiktillägget som en del av distributionen av en molntjänst</span><span class="sxs-lookup"><span data-stu-id="d3c1e-107">Enable diagnostics extension as part of deploying a Cloud Service</span></span>
<span data-ttu-id="d3c1e-108">Den här metoden är tillämpliga toocontinuous integrering scenarier där hello diagnostik tillägget kan aktiveras som en del av distribuera hello tjänst i molnet.</span><span class="sxs-lookup"><span data-stu-id="d3c1e-108">This approach is applicable toocontinuous integration type of scenarios, where hello diagnostics extension can be enabled as part of deploying hello cloud service.</span></span> <span data-ttu-id="d3c1e-109">När du skapar en ny molntjänst-distribution kan du aktivera hello diagnostik tillägg genom att passera i hello *ExtensionConfiguration* parametern toohello [ny AzureDeployment](/powershell/module/azure/new-azuredeployment?view=azuresmps-3.7.0) cmdlet.</span><span class="sxs-lookup"><span data-stu-id="d3c1e-109">When creating a new Cloud Service deployment you can enable hello diagnostics extension by passing in hello *ExtensionConfiguration* parameter toohello [New-AzureDeployment](/powershell/module/azure/new-azuredeployment?view=azuresmps-3.7.0) cmdlet.</span></span> <span data-ttu-id="d3c1e-110">Hej *ExtensionConfiguration* parameter tar en matris av diagnostik-konfigurationer som kan skapas med hjälp av hello [ny AzureServiceDiagnosticsExtensionConfig](/powershell/module/azure/new-azureservicediagnosticsextensionconfig?view=azuresmps-3.7.0) cmdlet.</span><span class="sxs-lookup"><span data-stu-id="d3c1e-110">hello *ExtensionConfiguration* parameter takes an array of diagnostics configurations that can be created using hello [New-AzureServiceDiagnosticsExtensionConfig](/powershell/module/azure/new-azureservicediagnosticsextensionconfig?view=azuresmps-3.7.0) cmdlet.</span></span>

<span data-ttu-id="d3c1e-111">hello följande exempel visas hur du kan aktivera diagnostik för en molnbaserad tjänst med en WebRole och WorkerRole, var och en med en annan diagnostik-konfiguration.</span><span class="sxs-lookup"><span data-stu-id="d3c1e-111">hello following example shows how you can enable diagnostics for a cloud service with a WebRole and WorkerRole, each having a different diagnostics configuration.</span></span>

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

<span data-ttu-id="d3c1e-112">Om hello diagnostik konfigurationsfil anger en `StorageAccount` element med namnet på ett lagringskonto, sedan hello `New-AzureServiceDiagnosticsExtensionConfig` cmdlet används automatiskt som lagringskontot.</span><span class="sxs-lookup"><span data-stu-id="d3c1e-112">If hello diagnostics configuration file specifies a `StorageAccount` element with a storage account name, then hello `New-AzureServiceDiagnosticsExtensionConfig` cmdlet will automatically use that storage account.</span></span> <span data-ttu-id="d3c1e-113">För den här toowork hello lagringskonto måste toobe i hello samma prenumeration som hello Molntjänsten som distribueras.</span><span class="sxs-lookup"><span data-stu-id="d3c1e-113">For this toowork, hello storage account needs toobe in hello same subscription as hello Cloud Service being deployed.</span></span>

<span data-ttu-id="d3c1e-114">Från Azure SDK 2.6 konfigurationsfiler för vidare hello genereras av hello MSBuild publicera innehåller utdata hello lagringskontonamn baserat på hello diagnostik konfigurationssträngen i hello tjänstekonfigurationsfilen (.cscfg).</span><span class="sxs-lookup"><span data-stu-id="d3c1e-114">From Azure SDK 2.6 onward hello extension configuration files generated by hello MSBuild publish target output will include hello storage account name based on hello diagnostics configuration string specified in hello service configuration file (.cscfg).</span></span> <span data-ttu-id="d3c1e-115">hello-skriptet nedan visar hur tooparse hello konfigurationsfiler från hello publicera utdata och konfigurera diagnostik för varje roll när du distribuerar hello-Molntjänsten.</span><span class="sxs-lookup"><span data-stu-id="d3c1e-115">hello script below shows you how tooparse hello Extension configuration files from hello publish target output and configure diagnostics extension for each role when deploying hello cloud service.</span></span>

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

<span data-ttu-id="d3c1e-116">Visual Studio Online använder ett liknande sätt för automatisk distribution av molntjänster med hello diagnostik tillägg.</span><span class="sxs-lookup"><span data-stu-id="d3c1e-116">Visual Studio Online uses a similar approach for automated deployments of Cloud Services with hello diagnostics extension.</span></span> <span data-ttu-id="d3c1e-117">Se [publicera AzureCloudDeployment.ps1](https://github.com/Microsoft/vso-agent-tasks/blob/master/Tasks/AzureCloudPowerShellDeployment/Publish-AzureCloudDeployment.ps1) för en komplett exempel.</span><span class="sxs-lookup"><span data-stu-id="d3c1e-117">See [Publish-AzureCloudDeployment.ps1](https://github.com/Microsoft/vso-agent-tasks/blob/master/Tasks/AzureCloudPowerShellDeployment/Publish-AzureCloudDeployment.ps1) for a complete example.</span></span>

<span data-ttu-id="d3c1e-118">Om inget `StorageAccount` har angetts i hello diagnostik konfigurationen måste toopass i hello *StorageAccountName* parametern toohello cmdlet.</span><span class="sxs-lookup"><span data-stu-id="d3c1e-118">If no `StorageAccount` was specified in hello diagnostics configuration, then you need toopass in hello *StorageAccountName* parameter toohello cmdlet.</span></span> <span data-ttu-id="d3c1e-119">Om hello *StorageAccountName* parametern anges, kommer hello cmdlet alltid använda hello storage-konto som har angetts i parametern hello och inte hello ett som har angetts i konfigurationsfilen för hello diagnostik.</span><span class="sxs-lookup"><span data-stu-id="d3c1e-119">If hello *StorageAccountName* parameter is specified, then hello cmdlet will always use hello storage account that is specified in hello parameter and not hello one that is specified in hello diagnostics configuration file.</span></span>

<span data-ttu-id="d3c1e-120">Om hello diagnostik storage-konto är i en annan prenumeration från hello molnbaserad tjänst bör tooexplicitly måste skicka in hello *StorageAccountName* och *StorageAccountKey* parametrar toohello cmdlet.</span><span class="sxs-lookup"><span data-stu-id="d3c1e-120">If hello diagnostics storage account is in a different subscription from hello Cloud Service, then you need tooexplicitly pass in hello *StorageAccountName* and *StorageAccountKey* parameters toohello cmdlet.</span></span> <span data-ttu-id="d3c1e-121">Hej *StorageAccountKey* parametern behövs inte när hello diagnostik storage-konto är i hello samma prenumeration som hello cmdlet automatiskt kan fråga och ange hello nyckelvärdet när du aktiverar hello diagnostik tillägg.</span><span class="sxs-lookup"><span data-stu-id="d3c1e-121">hello *StorageAccountKey* parameter is not needed when hello diagnostics storage account is in hello same subscription, as hello cmdlet can automatically query and set hello key value when enabling hello diagnostics extension.</span></span> <span data-ttu-id="d3c1e-122">Om hello diagnostiklagringskonto finns i en annan prenumeration och sedan hello cmdlet kanske inte kan tooget hello nyckeln automatiskt och du behöver tooexplicitly ange hello nyckel via hello *StorageAccountKey* parameter.</span><span class="sxs-lookup"><span data-stu-id="d3c1e-122">However, if hello diagnostics storage account is in a different subscription, then hello cmdlet might not be able tooget hello key automatically and you need tooexplicitly specify hello key through hello *StorageAccountKey* parameter.</span></span>

```powershell
$webrole_diagconfig = New-AzureServiceDiagnosticsExtensionConfig -Role "WebRole" -DiagnosticsConfigurationPath $webrole_diagconfigpath -StorageAccountName $diagnosticsstorage_name -StorageAccountKey $diagnosticsstorage_key
$workerrole_diagconfig = New-AzureServiceDiagnosticsExtensionConfig -Role "WorkerRole" -DiagnosticsConfigurationPath $workerrole_diagconfigpath -StorageAccountName $diagnosticsstorage_name -StorageAccountKey $diagnosticsstorage_key
```

## <a name="enable-diagnostics-extension-on-an-existing-cloud-service"></a><span data-ttu-id="d3c1e-123">Aktivera diagnostiktillägg i en befintlig molntjänst</span><span class="sxs-lookup"><span data-stu-id="d3c1e-123">Enable diagnostics extension on an existing Cloud Service</span></span>
<span data-ttu-id="d3c1e-124">Du kan använda hello [Set AzureServiceDiagnosticsExtension](/powershell/module/azure/set-azureservicediagnosticsextension?view=azuresmps-3.7.0) cmdlet tooenable eller uppdatera diagnostik-konfiguration på en molntjänst som redan körs.</span><span class="sxs-lookup"><span data-stu-id="d3c1e-124">You can use hello [Set-AzureServiceDiagnosticsExtension](/powershell/module/azure/set-azureservicediagnosticsextension?view=azuresmps-3.7.0) cmdlet tooenable or update diagnostics configuration on a Cloud Service that is already running.</span></span>

[!INCLUDE [cloud-services-wad-warning](../../includes/cloud-services-wad-warning.md)]

```powershell
$service_name = "MyService"
$webrole_diagconfigpath = "MyService.WebRole.PubConfig.xml"
$workerrole_diagconfigpath = "MyService.WorkerRole.PubConfig.xml"

$webrole_diagconfig = New-AzureServiceDiagnosticsExtensionConfig -Role "WebRole" -DiagnosticsConfigurationPath $webrole_diagconfigpath
$workerrole_diagconfig = New-AzureServiceDiagnosticsExtensionConfig -Role "WorkerRole" -DiagnosticsConfigurationPath $workerrole_diagconfigpath

Set-AzureServiceDiagnosticsExtension -DiagnosticsConfiguration @($webrole_diagconfig,$workerrole_diagconfig) -ServiceName $service_name
```

## <a name="get-current-diagnostics-extension-configuration"></a><span data-ttu-id="d3c1e-125">Hämta den aktuella konfigurationen för diagnostiktillägg</span><span class="sxs-lookup"><span data-stu-id="d3c1e-125">Get current diagnostics extension configuration</span></span>
<span data-ttu-id="d3c1e-126">Använd hello [Get-AzureServiceDiagnosticsExtension](/powershell/module/azure/get-azureservicediagnosticsextension?view=azuresmps-3.7.0) cmdlet tooget hello aktuella diagnostik konfigurationen för en tjänst i molnet.</span><span class="sxs-lookup"><span data-stu-id="d3c1e-126">Use hello [Get-AzureServiceDiagnosticsExtension](/powershell/module/azure/get-azureservicediagnosticsextension?view=azuresmps-3.7.0) cmdlet tooget hello current diagnostics configuration for a cloud service.</span></span>

```powershell
Get-AzureServiceDiagnosticsExtension -ServiceName "MyService"
```

## <a name="remove-diagnostics-extension"></a><span data-ttu-id="d3c1e-127">Ta bort diagnostiktillägg</span><span class="sxs-lookup"><span data-stu-id="d3c1e-127">Remove diagnostics extension</span></span>
<span data-ttu-id="d3c1e-128">tooturn av diagnostik i ett moln-tjänst du kan använda hello [ta bort AzureServiceDiagnosticsExtension](/powershell/module/azure/remove-azureservicediagnosticsextension?view=azuresmps-3.7.0) cmdlet.</span><span class="sxs-lookup"><span data-stu-id="d3c1e-128">tooturn off diagnostics on a cloud service you can use hello [Remove-AzureServiceDiagnosticsExtension](/powershell/module/azure/remove-azureservicediagnosticsextension?view=azuresmps-3.7.0) cmdlet.</span></span>

```powershell
Remove-AzureServiceDiagnosticsExtension -ServiceName "MyService"
```

<span data-ttu-id="d3c1e-129">Om du har aktiverat hello diagnostik tillägget med hjälp av antingen *Set AzureServiceDiagnosticsExtension* eller hello *ny AzureServiceDiagnosticsExtensionConfig* utan hello *roll* parameter och du kan ta bort hello tillägget med hjälp av *ta bort AzureServiceDiagnosticsExtension* utan hello *rollen* parameter.</span><span class="sxs-lookup"><span data-stu-id="d3c1e-129">If you enabled hello diagnostics extension using either *Set-AzureServiceDiagnosticsExtension* or hello *New-AzureServiceDiagnosticsExtensionConfig* without hello *Role* parameter then you can remove hello extension using *Remove-AzureServiceDiagnosticsExtension* without hello *Role* parameter.</span></span> <span data-ttu-id="d3c1e-130">Om hello *rollen* parametern används när du aktiverar tillägget Hej då måste användas när du tar bort hello tillägget.</span><span class="sxs-lookup"><span data-stu-id="d3c1e-130">If hello *Role* parameter was used when enabling hello extension then it must also be used when removing hello extension.</span></span>

<span data-ttu-id="d3c1e-131">tooremove hello diagnostik tillägg från varje enskild roll:</span><span class="sxs-lookup"><span data-stu-id="d3c1e-131">tooremove hello diagnostics extension from each individual role:</span></span>

```powershell
Remove-AzureServiceDiagnosticsExtension -ServiceName "MyService" -Role "WebRole"
```

## <a name="next-steps"></a><span data-ttu-id="d3c1e-132">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="d3c1e-132">Next Steps</span></span>
* <span data-ttu-id="d3c1e-133">Mer information om hur du använder Azure-diagnostik och andra tekniker tootroubleshoot problem finns [aktiverar diagnostik i Azure-molntjänster och virtuella datorer](cloud-services-dotnet-diagnostics.md).</span><span class="sxs-lookup"><span data-stu-id="d3c1e-133">For additional guidance on using Azure diagnostics and other techniques tootroubleshoot problems, see [Enabling Diagnostics in Azure Cloud Services and Virtual Machines](cloud-services-dotnet-diagnostics.md).</span></span>
* <span data-ttu-id="d3c1e-134">Hej [diagnostik Konfigurationsschemat](https://msdn.microsoft.com/library/azure/dn782207.aspx) förklarar hello olika konfigurationer för XML-alternativ för hello diagnostik tillägg.</span><span class="sxs-lookup"><span data-stu-id="d3c1e-134">hello [Diagnostics Configuration Schema](https://msdn.microsoft.com/library/azure/dn782207.aspx) explains hello various xml configurations options for hello diagnostics extension.</span></span>
* <span data-ttu-id="d3c1e-135">hur tooenable hello diagnostik tillägg för virtuella datorer, se toolearn [skapa en Windows virtuell dator med övervakning och diagnostik med Azure Resource Manager-mall](../virtual-machines/windows/extensions-diagnostics-template.md)</span><span class="sxs-lookup"><span data-stu-id="d3c1e-135">toolearn how tooenable hello diagnostics extension for Virtual Machines, see [Create a Windows Virtual machine with monitoring and diagnostics using Azure Resource Manager Template](../virtual-machines/windows/extensions-diagnostics-template.md)</span></span>
