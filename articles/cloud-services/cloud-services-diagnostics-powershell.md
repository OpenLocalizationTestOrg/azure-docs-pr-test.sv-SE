---
title: "Aktivera diagnostik i Azure Cloud Services med hjälp av PowerShell | Microsoft Docs"
description: "Lär dig hur du aktiverar diagnostik för molntjänster med hjälp av PowerShell"
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
ms.openlocfilehash: 8dd9724981860c9cd4ccc443cc2bfdc465811e7c
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="enable-diagnostics-in-azure-cloud-services-using-powershell"></a><span data-ttu-id="ba4aa-103">Aktivera diagnostik i Azure Cloud Services med hjälp av PowerShell</span><span class="sxs-lookup"><span data-stu-id="ba4aa-103">Enable diagnostics in Azure Cloud Services using PowerShell</span></span>
<span data-ttu-id="ba4aa-104">Du kan samla in diagnostikdata som programloggarna, prestandaräknare etc. från en tjänst i molnet med Azure-diagnostik.</span><span class="sxs-lookup"><span data-stu-id="ba4aa-104">You can collect diagnostic data like application logs, performance counters etc. from a Cloud Service using the Azure Diagnostics extension.</span></span> <span data-ttu-id="ba4aa-105">Den här artikeln beskriver hur du aktiverar tillägget för Azure-diagnostik för en tjänst i molnet med hjälp av PowerShell.</span><span class="sxs-lookup"><span data-stu-id="ba4aa-105">This article describes how to enable the Azure Diagnostics extension for a Cloud Service using PowerShell.</span></span>  <span data-ttu-id="ba4aa-106">Se [hur du installerar och konfigurerar du Azure PowerShell](/powershell/azure/overview) för kraven för den här artikeln.</span><span class="sxs-lookup"><span data-stu-id="ba4aa-106">See [How to install and configure Azure PowerShell](/powershell/azure/overview) for the prerequisites needed for this article.</span></span>

## <a name="enable-diagnostics-extension-as-part-of-deploying-a-cloud-service"></a><span data-ttu-id="ba4aa-107">Aktivera diagnostiktillägget som en del av distributionen av en molntjänst</span><span class="sxs-lookup"><span data-stu-id="ba4aa-107">Enable diagnostics extension as part of deploying a Cloud Service</span></span>
<span data-ttu-id="ba4aa-108">Den här metoden kan användas för kontinuerlig Integrationstyp av scenarier där tillägget diagnostics kan aktiveras som en del av distribution av Molntjänsten.</span><span class="sxs-lookup"><span data-stu-id="ba4aa-108">This approach is applicable to continuous integration type of scenarios, where the diagnostics extension can be enabled as part of deploying the cloud service.</span></span> <span data-ttu-id="ba4aa-109">När du skapar en ny molntjänst distribution du kan aktivera diagnostik-tillägg genom att passera i den *ExtensionConfiguration* parametern till den [ny AzureDeployment](/powershell/module/azure/new-azuredeployment?view=azuresmps-3.7.0) cmdlet.</span><span class="sxs-lookup"><span data-stu-id="ba4aa-109">When creating a new Cloud Service deployment you can enable the diagnostics extension by passing in the *ExtensionConfiguration* parameter to the [New-AzureDeployment](/powershell/module/azure/new-azuredeployment?view=azuresmps-3.7.0) cmdlet.</span></span> <span data-ttu-id="ba4aa-110">Den *ExtensionConfiguration* parameter tar en matris av diagnostik-konfigurationer som kan skapas med hjälp av den [ny AzureServiceDiagnosticsExtensionConfig](/powershell/module/azure/new-azureservicediagnosticsextensionconfig?view=azuresmps-3.7.0) cmdlet.</span><span class="sxs-lookup"><span data-stu-id="ba4aa-110">The *ExtensionConfiguration* parameter takes an array of diagnostics configurations that can be created using the [New-AzureServiceDiagnosticsExtensionConfig](/powershell/module/azure/new-azureservicediagnosticsextensionconfig?view=azuresmps-3.7.0) cmdlet.</span></span>

<span data-ttu-id="ba4aa-111">I följande exempel visas hur du kan aktivera diagnostik för en molnbaserad tjänst med en WebRole och WorkerRole, var och en med en annan diagnostik-konfiguration.</span><span class="sxs-lookup"><span data-stu-id="ba4aa-111">The following example shows how you can enable diagnostics for a cloud service with a WebRole and WorkerRole, each having a different diagnostics configuration.</span></span>

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

<span data-ttu-id="ba4aa-112">Om konfigurationsfilen diagnostik anger en `StorageAccount` element med namnet på ett lagringskonto, sedan `New-AzureServiceDiagnosticsExtensionConfig` cmdlet använder automatiskt det storage-kontot.</span><span class="sxs-lookup"><span data-stu-id="ba4aa-112">If the diagnostics configuration file specifies a `StorageAccount` element with a storage account name, then the `New-AzureServiceDiagnosticsExtensionConfig` cmdlet will automatically use that storage account.</span></span> <span data-ttu-id="ba4aa-113">Lagringskontot måste vara i samma prenumeration som den molntjänst som distribueras för detta ska fungera.</span><span class="sxs-lookup"><span data-stu-id="ba4aa-113">For this to work, the storage account needs to be in the same subscription as the Cloud Service being deployed.</span></span>

<span data-ttu-id="ba4aa-114">Från Azure SDK 2.6 och framåt tillägget konfigurationsfilerna som genereras av MSBuild publicera innehåller utdata lagringskontots namn baserat på diagnostik configuration strängen som anges i tjänstekonfigurationsfilen (.cscfg).</span><span class="sxs-lookup"><span data-stu-id="ba4aa-114">From Azure SDK 2.6 onward the extension configuration files generated by the MSBuild publish target output will include the storage account name based on the diagnostics configuration string specified in the service configuration file (.cscfg).</span></span> <span data-ttu-id="ba4aa-115">Skriptet nedan visar hur du tolka konfigurationsfiler tillägget från den publish utdatan och konfigurera diagnostik för varje roll vid distribution av Molntjänsten.</span><span class="sxs-lookup"><span data-stu-id="ba4aa-115">The script below shows you how to parse the Extension configuration files from the publish target output and configure diagnostics extension for each role when deploying the cloud service.</span></span>

```powershell
$service_name = "MyService"
$service_package = "C:\build\output\CloudService.cspkg"
$service_config = "C:\build\output\ServiceConfiguration.Cloud.cscfg"

#Find the Extensions path based on service configuration file
$extensionsSearchPath = Join-Path -Path (Split-Path -Parent $service_config) -ChildPath "Extensions"

$diagnosticsExtensions = Get-ChildItem -Path $extensionsSearchPath -Filter "PaaSDiagnostics.*.PubConfig.xml"
$diagnosticsConfigurations = @()
foreach ($extPath in $diagnosticsExtensions)
{
    #Find the RoleName based on file naming convention PaaSDiagnostics.<RoleName>.PubConfig.xml
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

<span data-ttu-id="ba4aa-116">Visual Studio Online använder ett liknande sätt för automatisk distribution av molntjänster med filnamnstillägget diagnostik.</span><span class="sxs-lookup"><span data-stu-id="ba4aa-116">Visual Studio Online uses a similar approach for automated deployments of Cloud Services with the diagnostics extension.</span></span> <span data-ttu-id="ba4aa-117">Se [publicera AzureCloudDeployment.ps1](https://github.com/Microsoft/vso-agent-tasks/blob/master/Tasks/AzureCloudPowerShellDeployment/Publish-AzureCloudDeployment.ps1) för en komplett exempel.</span><span class="sxs-lookup"><span data-stu-id="ba4aa-117">See [Publish-AzureCloudDeployment.ps1](https://github.com/Microsoft/vso-agent-tasks/blob/master/Tasks/AzureCloudPowerShellDeployment/Publish-AzureCloudDeployment.ps1) for a complete example.</span></span>

<span data-ttu-id="ba4aa-118">Om inget `StorageAccount` har angetts i diagnostik-konfigurationen måste du skicka in den *StorageAccountName* parameter för cmdlet.</span><span class="sxs-lookup"><span data-stu-id="ba4aa-118">If no `StorageAccount` was specified in the diagnostics configuration, then you need to pass in the *StorageAccountName* parameter to the cmdlet.</span></span> <span data-ttu-id="ba4aa-119">Om den *StorageAccountName* parametern anges, kommer cmdleten använder alltid storage-konto som har angetts i parametern och inte det som har angetts i konfigurationsfilen diagnostik.</span><span class="sxs-lookup"><span data-stu-id="ba4aa-119">If the *StorageAccountName* parameter is specified, then the cmdlet will always use the storage account that is specified in the parameter and not the one that is specified in the diagnostics configuration file.</span></span>

<span data-ttu-id="ba4aa-120">Om diagnostik storage-konto är i en annan prenumeration från Molntjänsten, måste du uttryckligen skicka in den *StorageAccountName* och *StorageAccountKey* parametrar för cmdleten.</span><span class="sxs-lookup"><span data-stu-id="ba4aa-120">If the diagnostics storage account is in a different subscription from the Cloud Service, then you need to explicitly pass in the *StorageAccountName* and *StorageAccountKey* parameters to the cmdlet.</span></span> <span data-ttu-id="ba4aa-121">Den *StorageAccountKey* parametern behövs inte när diagnostiklagringskonto är i samma prenumeration som cmdleten automatiskt kan fråga och ange värdet för nyckeln när du aktiverar tillägget diagnostik.</span><span class="sxs-lookup"><span data-stu-id="ba4aa-121">The *StorageAccountKey* parameter is not needed when the diagnostics storage account is in the same subscription, as the cmdlet can automatically query and set the key value when enabling the diagnostics extension.</span></span> <span data-ttu-id="ba4aa-122">Om lagringskontot för gatewaydiagnostik är i en annan prenumeration sedan cmdleten kanske inte går att hämta nyckeln automatiskt och måste du uttryckligen ange nyckel via den *StorageAccountKey* parameter.</span><span class="sxs-lookup"><span data-stu-id="ba4aa-122">However, if the diagnostics storage account is in a different subscription, then the cmdlet might not be able to get the key automatically and you need to explicitly specify the key through the *StorageAccountKey* parameter.</span></span>

```powershell
$webrole_diagconfig = New-AzureServiceDiagnosticsExtensionConfig -Role "WebRole" -DiagnosticsConfigurationPath $webrole_diagconfigpath -StorageAccountName $diagnosticsstorage_name -StorageAccountKey $diagnosticsstorage_key
$workerrole_diagconfig = New-AzureServiceDiagnosticsExtensionConfig -Role "WorkerRole" -DiagnosticsConfigurationPath $workerrole_diagconfigpath -StorageAccountName $diagnosticsstorage_name -StorageAccountKey $diagnosticsstorage_key
```

## <a name="enable-diagnostics-extension-on-an-existing-cloud-service"></a><span data-ttu-id="ba4aa-123">Aktivera diagnostiktillägg i en befintlig molntjänst</span><span class="sxs-lookup"><span data-stu-id="ba4aa-123">Enable diagnostics extension on an existing Cloud Service</span></span>
<span data-ttu-id="ba4aa-124">Du kan använda den [Set AzureServiceDiagnosticsExtension](/powershell/module/azure/set-azureservicediagnosticsextension?view=azuresmps-3.7.0) för att aktivera eller uppdatera diagnostik konfigurationen på en molntjänst som redan körs.</span><span class="sxs-lookup"><span data-stu-id="ba4aa-124">You can use the [Set-AzureServiceDiagnosticsExtension](/powershell/module/azure/set-azureservicediagnosticsextension?view=azuresmps-3.7.0) cmdlet to enable or update diagnostics configuration on a Cloud Service that is already running.</span></span>

[!INCLUDE [cloud-services-wad-warning](../../includes/cloud-services-wad-warning.md)]

```powershell
$service_name = "MyService"
$webrole_diagconfigpath = "MyService.WebRole.PubConfig.xml"
$workerrole_diagconfigpath = "MyService.WorkerRole.PubConfig.xml"

$webrole_diagconfig = New-AzureServiceDiagnosticsExtensionConfig -Role "WebRole" -DiagnosticsConfigurationPath $webrole_diagconfigpath
$workerrole_diagconfig = New-AzureServiceDiagnosticsExtensionConfig -Role "WorkerRole" -DiagnosticsConfigurationPath $workerrole_diagconfigpath

Set-AzureServiceDiagnosticsExtension -DiagnosticsConfiguration @($webrole_diagconfig,$workerrole_diagconfig) -ServiceName $service_name
```

## <a name="get-current-diagnostics-extension-configuration"></a><span data-ttu-id="ba4aa-125">Hämta den aktuella konfigurationen för diagnostiktillägg</span><span class="sxs-lookup"><span data-stu-id="ba4aa-125">Get current diagnostics extension configuration</span></span>
<span data-ttu-id="ba4aa-126">Använd den [Get-AzureServiceDiagnosticsExtension](/powershell/module/azure/get-azureservicediagnosticsextension?view=azuresmps-3.7.0) för att hämta den aktuella diagnostik-konfigurationen för en tjänst i molnet.</span><span class="sxs-lookup"><span data-stu-id="ba4aa-126">Use the [Get-AzureServiceDiagnosticsExtension](/powershell/module/azure/get-azureservicediagnosticsextension?view=azuresmps-3.7.0) cmdlet to get the current diagnostics configuration for a cloud service.</span></span>

```powershell
Get-AzureServiceDiagnosticsExtension -ServiceName "MyService"
```

## <a name="remove-diagnostics-extension"></a><span data-ttu-id="ba4aa-127">Ta bort diagnostiktillägg</span><span class="sxs-lookup"><span data-stu-id="ba4aa-127">Remove diagnostics extension</span></span>
<span data-ttu-id="ba4aa-128">Inaktivera diagnostik på en molnbaserad tjänst som du kan använda den [ta bort AzureServiceDiagnosticsExtension](/powershell/module/azure/remove-azureservicediagnosticsextension?view=azuresmps-3.7.0) cmdlet.</span><span class="sxs-lookup"><span data-stu-id="ba4aa-128">To turn off diagnostics on a cloud service you can use the [Remove-AzureServiceDiagnosticsExtension](/powershell/module/azure/remove-azureservicediagnosticsextension?view=azuresmps-3.7.0) cmdlet.</span></span>

```powershell
Remove-AzureServiceDiagnosticsExtension -ServiceName "MyService"
```

<span data-ttu-id="ba4aa-129">Om du har aktiverat diagnostik-tillägget med hjälp av antingen *Set AzureServiceDiagnosticsExtension* eller *ny AzureServiceDiagnosticsExtensionConfig* utan den *rollen* parameter och du kan ta bort tillägget med hjälp av *ta bort AzureServiceDiagnosticsExtension* utan den *rollen* parameter.</span><span class="sxs-lookup"><span data-stu-id="ba4aa-129">If you enabled the diagnostics extension using either *Set-AzureServiceDiagnosticsExtension* or the *New-AzureServiceDiagnosticsExtensionConfig* without the *Role* parameter then you can remove the extension using *Remove-AzureServiceDiagnosticsExtension* without the *Role* parameter.</span></span> <span data-ttu-id="ba4aa-130">Om den *rollen* parametern används när du aktiverar tillägget och den måste användas när du tar bort tillägget.</span><span class="sxs-lookup"><span data-stu-id="ba4aa-130">If the *Role* parameter was used when enabling the extension then it must also be used when removing the extension.</span></span>

<span data-ttu-id="ba4aa-131">Så här tar du bort diagnostiktillägget från varje enskild roll:</span><span class="sxs-lookup"><span data-stu-id="ba4aa-131">To remove the diagnostics extension from each individual role:</span></span>

```powershell
Remove-AzureServiceDiagnosticsExtension -ServiceName "MyService" -Role "WebRole"
```

## <a name="next-steps"></a><span data-ttu-id="ba4aa-132">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="ba4aa-132">Next Steps</span></span>
* <span data-ttu-id="ba4aa-133">Mer information om hur du använder Azure-diagnostik och andra tekniker för att felsöka problem finns [aktiverar diagnostik i Azure-molntjänster och virtuella datorer](cloud-services-dotnet-diagnostics.md).</span><span class="sxs-lookup"><span data-stu-id="ba4aa-133">For additional guidance on using Azure diagnostics and other techniques to troubleshoot problems, see [Enabling Diagnostics in Azure Cloud Services and Virtual Machines](cloud-services-dotnet-diagnostics.md).</span></span>
* <span data-ttu-id="ba4aa-134">Den [diagnostik Konfigurationsschemat](https://msdn.microsoft.com/library/azure/dn782207.aspx) förklarar alternativen olika XML-konfigurationer för diagnostik-tillägg.</span><span class="sxs-lookup"><span data-stu-id="ba4aa-134">The [Diagnostics Configuration Schema](https://msdn.microsoft.com/library/azure/dn782207.aspx) explains the various xml configurations options for the diagnostics extension.</span></span>
* <span data-ttu-id="ba4aa-135">Information om hur du aktiverar tillägget diagnostik för virtuella datorer finns [skapa en Windows virtuell dator med övervakning och diagnostik med Azure Resource Manager-mall](../virtual-machines/windows/extensions-diagnostics-template.md)</span><span class="sxs-lookup"><span data-stu-id="ba4aa-135">To learn how to enable the diagnostics extension for Virtual Machines, see [Create a Windows Virtual machine with monitoring and diagnostics using Azure Resource Manager Template](../virtual-machines/windows/extensions-diagnostics-template.md)</span></span>
