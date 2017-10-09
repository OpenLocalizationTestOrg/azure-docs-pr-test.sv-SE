---
title: "aaaUse Azure PowerShell tooenable diagnostik på en virtuell Windows-dator | Microsoft Docs"
services: virtual-machines-windows
documentationcenter: 
description: "Lär dig hur toouse PowerShell tooenable Azure-diagnostik i en virtuell dator som kör Windows"
author: sbtron
manager: timlt
editor: 
ms.assetid: 2e6d88f2-1980-4a24-827e-a81616a0d247
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 12/15/2015
ms.author: saurabh
ms.openlocfilehash: e945f0de154b5ba600f845f0d577b48e2254573b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="use-powershell-tooenable-azure-diagnostics-in-a-virtual-machine-running-windows"></a><span data-ttu-id="59ef9-103">Använd PowerShell tooenable Azure-diagnostik i en virtuell dator som kör Windows</span><span class="sxs-lookup"><span data-stu-id="59ef9-103">Use PowerShell tooenable Azure Diagnostics in a virtual machine running Windows</span></span>
[!INCLUDE [learn-about-deployment-models](../../../includes/learn-about-deployment-models-both-include.md)]

<span data-ttu-id="59ef9-104">Azure-diagnostik är hello funktion i Azure som gör att hello insamlingen av diagnostiska data på ett distribuerat program.</span><span class="sxs-lookup"><span data-stu-id="59ef9-104">Azure Diagnostics is hello capability within Azure that enables hello collection of diagnostic data on a deployed application.</span></span> <span data-ttu-id="59ef9-105">Du kan använda hello diagnostik tillägget toocollect diagnostikdata som programloggarna eller prestandaräknare från en Azure-dator (VM) som kör Windows.</span><span class="sxs-lookup"><span data-stu-id="59ef9-105">You can use hello diagnostics extension toocollect diagnostic data like application logs or performance counters from an Azure virtual machine (VM) that is running Windows.</span></span> <span data-ttu-id="59ef9-106">Den här artikeln beskriver hur toouse Windows PowerShell tooenable hello diagnostik tillägget för en virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="59ef9-106">This article describes how toouse Windows PowerShell tooenable hello diagnostics extension for a VM.</span></span> <span data-ttu-id="59ef9-107">Se [hur tooinstall och konfigurera Azure PowerShell](/powershell/azure/overview) för hello kraven för den här artikeln.</span><span class="sxs-lookup"><span data-stu-id="59ef9-107">See [How tooinstall and configure Azure PowerShell](/powershell/azure/overview) for hello prerequisites needed for this article.</span></span>

## <a name="enable-hello-diagnostics-extension-if-you-use-hello-resource-manager-deployment-model"></a><span data-ttu-id="59ef9-108">Aktivera hello diagnostik tillägget om du använder hello Resource Manager-distributionsmodellen</span><span class="sxs-lookup"><span data-stu-id="59ef9-108">Enable hello diagnostics extension if you use hello Resource Manager deployment model</span></span>
<span data-ttu-id="59ef9-109">Du kan aktivera hello diagnostik tillägg när du skapar en virtuell Windows-dator via hello Azure Resource Manager-modellen genom att lägga till hello-tillägget configuration toohello Resource Manager-mall.</span><span class="sxs-lookup"><span data-stu-id="59ef9-109">You can enable hello diagnostics extension while you create a Windows VM through hello Azure Resource Manager deployment model by adding hello extension configuration toohello Resource Manager template.</span></span> <span data-ttu-id="59ef9-110">Se [skapa en virtuell Windows-dator med övervakning och diagnostik med hello Azure Resource Manager-mall](extensions-diagnostics-template.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="59ef9-110">See [Create a Windows virtual machine with monitoring and diagnostics by using hello Azure Resource Manager template](extensions-diagnostics-template.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span>

<span data-ttu-id="59ef9-111">tooenable hello diagnostik tillägg på en befintlig virtuell dator som har skapats via hello Resource Manager-distributionsmodellen, som du kan använda hello [Set AzureRMVMDiagnosticsExtension](/powershell/module/azurerm.compute/set-azurermvmdiagnosticsextension) PowerShell-cmdlet enligt nedan.</span><span class="sxs-lookup"><span data-stu-id="59ef9-111">tooenable hello diagnostics extension on an existing VM that was created through hello Resource Manager deployment model, you can use hello [Set-AzureRMVMDiagnosticsExtension](/powershell/module/azurerm.compute/set-azurermvmdiagnosticsextension) PowerShell cmdlet as shown below.</span></span>

    $vm_resourcegroup = "myvmresourcegroup"
    $vm_name = "myvm"
    $diagnosticsconfig_path = "DiagnosticsPubConfig.xml"

    Set-AzureRmVMDiagnosticsExtension -ResourceGroupName $vm_resourcegroup -VMName $vm_name -DiagnosticsConfigurationPath $diagnosticsconfig_path


<span data-ttu-id="59ef9-112">*$diagnosticsconfig_path* är hello sökvägen toohello fil som innehåller hello diagnostik konfigurationen i XML, enligt beskrivningen i hello [exempel](#sample-diagnostics-configuration) nedan.</span><span class="sxs-lookup"><span data-stu-id="59ef9-112">*$diagnosticsconfig_path* is hello path toohello file that contains hello diagnostics configuration in XML, as described in hello [sample](#sample-diagnostics-configuration) below.</span></span>  

<span data-ttu-id="59ef9-113">Om hello diagnostik konfigurationsfil anger en **StorageAccount** element med namnet på ett lagringskonto, sedan hello *Set AzureRMVMDiagnosticsExtension* hello ställer automatiskt in skript tillägget toosend diagnostikdata toothat diagnostiklagringskonto.</span><span class="sxs-lookup"><span data-stu-id="59ef9-113">If hello diagnostics configuration file specifies a **StorageAccount** element with a storage account name, then hello *Set-AzureRMVMDiagnosticsExtension* script will automatically set hello diagnostics extension toosend diagnostic data toothat storage account.</span></span> <span data-ttu-id="59ef9-114">För den här toowork hello lagringskonto måste toobe i hello samma prenumeration som hello VM.</span><span class="sxs-lookup"><span data-stu-id="59ef9-114">For this toowork, hello storage account needs toobe in hello same subscription as hello VM.</span></span>

<span data-ttu-id="59ef9-115">Om inget **StorageAccount** har angetts i hello diagnostik konfigurationen måste toopass i hello *StorageAccountName* parametern toohello cmdlet.</span><span class="sxs-lookup"><span data-stu-id="59ef9-115">If no **StorageAccount** was specified in hello diagnostics configuration, then you need toopass in hello *StorageAccountName* parameter toohello cmdlet.</span></span> <span data-ttu-id="59ef9-116">Om hello *StorageAccountName* parametern anges, kommer hello cmdlet alltid använda hello storage-konto som har angetts i parametern hello och inte hello ett som har angetts i konfigurationsfilen för hello diagnostik.</span><span class="sxs-lookup"><span data-stu-id="59ef9-116">If hello *StorageAccountName* parameter is specified, then hello cmdlet will always use hello storage account that is specified in hello parameter and not hello one that is specified in hello diagnostics configuration file.</span></span>

<span data-ttu-id="59ef9-117">Om hello diagnostik storage-konto är i en annan prenumeration från hello VM, måste tooexplicitly skicka in hello *StorageAccountName* och *StorageAccountKey* parametrar toohello cmdlet.</span><span class="sxs-lookup"><span data-stu-id="59ef9-117">If hello diagnostics storage account is in a different subscription from hello VM, then you need tooexplicitly pass in hello *StorageAccountName* and *StorageAccountKey* parameters toohello cmdlet.</span></span> <span data-ttu-id="59ef9-118">Hej *StorageAccountKey* parametern behövs inte när hello diagnostik storage-konto är i hello samma prenumeration som hello cmdlet automatiskt kan fråga och ange hello nyckelvärdet när du aktiverar hello diagnostik tillägg.</span><span class="sxs-lookup"><span data-stu-id="59ef9-118">hello *StorageAccountKey* parameter is not needed when hello diagnostics storage account is in hello same subscription, as hello cmdlet can automatically query and set hello key value when enabling hello diagnostics extension.</span></span> <span data-ttu-id="59ef9-119">Om hello diagnostiklagringskonto finns i en annan prenumeration och sedan hello cmdlet kanske inte kan tooget hello nyckeln automatiskt och du behöver tooexplicitly ange hello nyckel via hello *StorageAccountKey* parameter.</span><span class="sxs-lookup"><span data-stu-id="59ef9-119">However, if hello diagnostics storage account is in a different subscription, then hello cmdlet might not be able tooget hello key automatically and you need tooexplicitly specify hello key through hello *StorageAccountKey* parameter.</span></span>  

    Set-AzureRmVMDiagnosticsExtension -ResourceGroupName $vm_resourcegroup -VMName $vm_name -DiagnosticsConfigurationPath $diagnosticsconfig_path -StorageAccountName $diagnosticsstorage_name -StorageAccountKey $diagnosticsstorage_key

<span data-ttu-id="59ef9-120">När hello diagnostik tillägget har aktiverats på en virtuell dator måste du hello aktuella inställningar med hjälp av hello [Get-AzureRMVmDiagnosticsExtension](/powershell/module/azurerm.compute/get-azurermvmdiagnosticsextension) cmdlet.</span><span class="sxs-lookup"><span data-stu-id="59ef9-120">Once hello diagnostics extension is enabled on a VM, you can get hello current settings by using hello [Get-AzureRMVmDiagnosticsExtension](/powershell/module/azurerm.compute/get-azurermvmdiagnosticsextension) cmdlet.</span></span>

    Get-AzureRmVMDiagnosticsExtension -ResourceGroupName $vm_resourcegroup -VMName $vm_name

<span data-ttu-id="59ef9-121">hello cmdlet returnerar *PublicSettings*, som innehåller hello diagnostik konfiguration.</span><span class="sxs-lookup"><span data-stu-id="59ef9-121">hello cmdlet returns *PublicSettings*, which contains hello diagnostics configuration.</span></span> <span data-ttu-id="59ef9-122">Det finns två typer av konfiguration som stöds, WadCfg och xmlCfg.</span><span class="sxs-lookup"><span data-stu-id="59ef9-122">There are two kinds of configuration supported, WadCfg and xmlCfg.</span></span> <span data-ttu-id="59ef9-123">WadCfg är JSON-konfiguration och xmlCfg är XML-konfiguration i en Base64-kodat format.</span><span class="sxs-lookup"><span data-stu-id="59ef9-123">WadCfg is JSON configuration, and xmlCfg is XML configuration in a Base64-encoded format.</span></span> <span data-ttu-id="59ef9-124">tooread hello XML, måste du toodecode den.</span><span class="sxs-lookup"><span data-stu-id="59ef9-124">tooread hello XML, you need toodecode it.</span></span>

    $publicsettings = (Get-AzureRmVMDiagnosticsExtension -ResourceGroupName $vm_resourcegroup -VMName $vm_name).PublicSettings
    $encodedconfig = (ConvertFrom-Json -InputObject $publicsettings).xmlCfg
    $xmlconfig = [System.Text.Encoding]::UTF8.GetString([System.Convert]::FromBase64String($encodedconfig))
    Write-Host $xmlconfig

<span data-ttu-id="59ef9-125">Hej [ta bort AzureRMVmDiagnosticsExtension](/powershell/module/azurerm.compute/remove-azurermvmdiagnosticsextension) kan vara används tooremove hello diagnostik tillägget från hello VM.</span><span class="sxs-lookup"><span data-stu-id="59ef9-125">hello [Remove-AzureRMVmDiagnosticsExtension](/powershell/module/azurerm.compute/remove-azurermvmdiagnosticsextension) cmdlet can be used tooremove hello diagnostics extension from hello VM.</span></span>  

## <a name="enable-hello-diagnostics-extension-if-you-use-hello-classic-deployment-model"></a><span data-ttu-id="59ef9-126">Aktivera hello diagnostik tillägget om du använder hello klassiska distributionsmodellen</span><span class="sxs-lookup"><span data-stu-id="59ef9-126">Enable hello diagnostics extension if you use hello classic deployment model</span></span>
<span data-ttu-id="59ef9-127">Du kan använda hello [Set AzureVMDiagnosticsExtension](/powershell/module/azure/set-azurevmdiagnosticsextension) cmdlet tooenable filnamnstillägget diagnostik på en virtuell dator som du skapar genom hello klassiska distributionsmodellen.</span><span class="sxs-lookup"><span data-stu-id="59ef9-127">You can use hello [Set-AzureVMDiagnosticsExtension](/powershell/module/azure/set-azurevmdiagnosticsextension) cmdlet tooenable a diagnostics extension on a VM that you create through hello classic deployment model.</span></span> <span data-ttu-id="59ef9-128">hello visar följande exempel hur toocreate en ny virtuell dator via hello klassiska distributionsmodellen med hello diagnostik tillägget aktiverat.</span><span class="sxs-lookup"><span data-stu-id="59ef9-128">hello following example shows how toocreate a new VM through hello classic deployment model with hello diagnostics extension enabled.</span></span>

    $VM = New-AzureVMConfig -Name $VM -InstanceSize Small -ImageName $VMImage
    $VM = Add-AzureProvisioningConfig -VM $VM -AdminUsername $Username -Password $Password -Windows
    $VM = Set-AzureVMDiagnosticsExtension -DiagnosticsConfigurationPath $Config_Path -VM $VM -StorageContext $Storage_Context
    New-AzureVM -Location $Location -ServiceName $Service_Name -VM $VM

<span data-ttu-id="59ef9-129">tooenable hello diagnostik tillägg på en befintlig virtuell dator som har skapats via hello klassiska distributionsmodellen, första användning hello [Get-AzureVM](/powershell/module/azure/get-azurevm) cmdlet tooget hello VM-konfiguration.</span><span class="sxs-lookup"><span data-stu-id="59ef9-129">tooenable hello diagnostics extension on an existing VM that was created through hello classic deployment model, first use hello [Get-AzureVM](/powershell/module/azure/get-azurevm) cmdlet tooget hello VM configuration.</span></span> <span data-ttu-id="59ef9-130">Uppdatera sedan hello VM configuration tooinclude hello diagnostik-tillägget med hjälp av hello [Set AzureVMDiagnosticsExtension](/powershell/module/azure/set-azurevmdiagnosticsextension) cmdlet.</span><span class="sxs-lookup"><span data-stu-id="59ef9-130">Then update hello VM configuration tooinclude hello diagnostics extension by using hello [Set-AzureVMDiagnosticsExtension](/powershell/module/azure/set-azurevmdiagnosticsextension) cmdlet.</span></span> <span data-ttu-id="59ef9-131">Slutligen tillämpa hello uppdateras configuration toohello VM med hjälp av [uppdatering AzureVM](/powershell/module/azure/update-azurevm).</span><span class="sxs-lookup"><span data-stu-id="59ef9-131">Finally, apply hello updated configuration toohello VM by using [Update-AzureVM](/powershell/module/azure/update-azurevm).</span></span>

    $VM = Get-AzureVM -ServiceName $Service_Name -Name $VM_Name
    $VM_Update = Set-AzureVMDiagnosticsExtension -DiagnosticsConfigurationPath $Config_Path -VM $VM -StorageContext $Storage_Context
    Update-AzureVM -ServiceName $Service_Name -Name $VM_Name -VM $VM_Update.VM

## <a name="sample-diagnostics-configuration"></a><span data-ttu-id="59ef9-132">Diagnostik exempelkonfiguration</span><span class="sxs-lookup"><span data-stu-id="59ef9-132">Sample diagnostics configuration</span></span>
<span data-ttu-id="59ef9-133">hello följande XML kan användas för hello diagnostik offentliga konfiguration med hello ovan skript.</span><span class="sxs-lookup"><span data-stu-id="59ef9-133">hello following XML can be used for hello diagnostics public configuration with hello above scripts.</span></span> <span data-ttu-id="59ef9-134">Den här exempelkonfiguration överför olika prestandaräknare toohello diagnostiklagringskonto, tillsammans med fel från hello program, säkerhet och systemkanaler i händelseloggarna i Windows hello och eventuella fel från hello diagnostik infrastruktur för loggar.</span><span class="sxs-lookup"><span data-stu-id="59ef9-134">This sample configuration will transfer various performance counters toohello diagnostics storage account, along with errors from hello application, security, and system channels in hello Windows event logs and any errors from hello diagnostics infrastructure logs.</span></span>

<span data-ttu-id="59ef9-135">hello konfiguration måste toobe uppdaterade tooinclude hello följande:</span><span class="sxs-lookup"><span data-stu-id="59ef9-135">hello configuration needs toobe updated tooinclude hello following:</span></span>

* <span data-ttu-id="59ef9-136">Hej *resourceID* attribut för hello **mått** element måste toobe uppdateras med hello resurs-ID för hello VM.</span><span class="sxs-lookup"><span data-stu-id="59ef9-136">hello *resourceID* attribute of hello **Metrics** element needs toobe updated with hello resource ID for hello VM.</span></span>
  
  * <span data-ttu-id="59ef9-137">hello resurs-ID kan konstrueras med hjälp av hello följer mönstret ”: /subscriptions/ {*prenumerations-ID för hello prenumerationen med hello VM*} /resourceGroups/ {*hello namn för hello VM*} / providers/Microsoft.Compute/virtualMachines/ {*hello namn på virtuell*} ”.</span><span class="sxs-lookup"><span data-stu-id="59ef9-137">hello resource ID can be constructed by using hello following pattern: "/subscriptions/{*subscription ID for hello subscription with hello VM*}/resourceGroups/{*hello resourcegroup name for hello VM*}/providers/Microsoft.Compute/virtualMachines/{*hello VM Name*}".</span></span>
  * <span data-ttu-id="59ef9-138">Till exempel om hello prenumerations-ID för hello prenumeration där hello VM körs är **11111111-1111-1111-1111-111111111111**, hello resursgruppens namn för resursgruppen hello används **MyResourceGroup**, och hello VM namn är **MyWindowsVM**, hello värde för *resourceID* skulle vara:</span><span class="sxs-lookup"><span data-stu-id="59ef9-138">For example, if hello subscription ID for hello subscription where hello VM is running is **11111111-1111-1111-1111-111111111111**, hello resource group name for hello resource group is **MyResourceGroup**, and hello VM Name is **MyWindowsVM**, then hello value for *resourceID* would be:</span></span>
    
      ```
      <Metrics resourceId="/subscriptions/11111111-1111-1111-1111-111111111111/resourceGroups/MyResourceGroup/providers/Microsoft.Compute/virtualMachines/MyWindowsVM" >
      ```
  * <span data-ttu-id="59ef9-139">Mer information om hur är genererade utifrån hello prestandaräknare och mått konfiguration finns [Azure Diagnostics mått tabellen i lagring](extensions-diagnostics-template.md#wadmetrics-tables-in-storage).</span><span class="sxs-lookup"><span data-stu-id="59ef9-139">For more information on how metrics are generated based on hello performance counters and metrics configuration, see [Azure Diagnostics metrics table in storage](extensions-diagnostics-template.md#wadmetrics-tables-in-storage).</span></span>
* <span data-ttu-id="59ef9-140">Hej **StorageAccount** element måste uppdateras med hello namnet på hello diagnostiklagringskonto toobe.</span><span class="sxs-lookup"><span data-stu-id="59ef9-140">hello **StorageAccount** element needs toobe updated with hello name of hello diagnostics storage account.</span></span>
  
    ```
    <?xml version="1.0" encoding="utf-8"?>
    <PublicConfig xmlns="http://schemas.microsoft.com/ServiceHosting/2010/10/DiagnosticsConfiguration">
        <WadCfg>
          <DiagnosticMonitorConfiguration overallQuotaInMB="4096">
            <DiagnosticInfrastructureLogs scheduledTransferLogLevelFilter="Error"/>
            <PerformanceCounters scheduledTransferPeriod="PT1M">
          <PerformanceCounterConfiguration counterSpecifier="\Processor(_Total)\% Processor Time" sampleRate="PT15S" unit="Percent">
            <annotation displayName="CPU utilization" locale="en-us"/>
          </PerformanceCounterConfiguration>
          <PerformanceCounterConfiguration counterSpecifier="\Processor(_Total)\% Privileged Time" sampleRate="PT15S" unit="Percent">
            <annotation displayName="CPU privileged time" locale="en-us"/>
          </PerformanceCounterConfiguration>
          <PerformanceCounterConfiguration counterSpecifier="\Processor(_Total)\% User Time" sampleRate="PT15S" unit="Percent">
            <annotation displayName="CPU user time" locale="en-us"/>
          </PerformanceCounterConfiguration>
          <PerformanceCounterConfiguration counterSpecifier="\Processor Information(_Total)\Processor Frequency" sampleRate="PT15S" unit="Count">
            <annotation displayName="CPU frequency" locale="en-us"/>
          </PerformanceCounterConfiguration>
          <PerformanceCounterConfiguration counterSpecifier="\System\Processes" sampleRate="PT15S" unit="Count">
            <annotation displayName="Processes" locale="en-us"/>
          </PerformanceCounterConfiguration>
          <PerformanceCounterConfiguration counterSpecifier="\Process(_Total)\Thread Count" sampleRate="PT15S" unit="Count">
            <annotation displayName="Threads" locale="en-us"/>
          </PerformanceCounterConfiguration>
          <PerformanceCounterConfiguration counterSpecifier="\Process(_Total)\Handle Count" sampleRate="PT15S" unit="Count">
            <annotation displayName="Handles" locale="en-us"/>
          </PerformanceCounterConfiguration>
          <PerformanceCounterConfiguration counterSpecifier="\Memory\% Committed Bytes In Use" sampleRate="PT15S" unit="Percent">
            <annotation displayName="Memory usage" locale="en-us"/>
          </PerformanceCounterConfiguration>
          <PerformanceCounterConfiguration counterSpecifier="\Memory\Available Bytes" sampleRate="PT15S" unit="Bytes">
            <annotation displayName="Memory available" locale="en-us"/>
          </PerformanceCounterConfiguration>
          <PerformanceCounterConfiguration counterSpecifier="\Memory\Committed Bytes" sampleRate="PT15S" unit="Bytes">
            <annotation displayName="Memory committed" locale="en-us"/>
          </PerformanceCounterConfiguration>
          <PerformanceCounterConfiguration counterSpecifier="\Memory\Commit Limit" sampleRate="PT15S" unit="Bytes">
            <annotation displayName="Memory commit limit" locale="en-us"/>
          </PerformanceCounterConfiguration>
          <PerformanceCounterConfiguration counterSpecifier="\Memory\Pool Paged Bytes" sampleRate="PT15S" unit="Bytes">
            <annotation displayName="Memory paged pool" locale="en-us"/>
          </PerformanceCounterConfiguration>
          <PerformanceCounterConfiguration counterSpecifier="\Memory\Pool Nonpaged Bytes" sampleRate="PT15S" unit="Bytes">
            <annotation displayName="Memory non-paged pool" locale="en-us"/>
          </PerformanceCounterConfiguration>
          <PerformanceCounterConfiguration counterSpecifier="\PhysicalDisk(_Total)\% Disk Time" sampleRate="PT15S" unit="Percent">
            <annotation displayName="Disk active time" locale="en-us"/>
          </PerformanceCounterConfiguration>
          <PerformanceCounterConfiguration counterSpecifier="\PhysicalDisk(_Total)\% Disk Read Time" sampleRate="PT15S" unit="Percent">
            <annotation displayName="Disk active read time" locale="en-us"/>
          </PerformanceCounterConfiguration>
          <PerformanceCounterConfiguration counterSpecifier="\PhysicalDisk(_Total)\% Disk Write Time" sampleRate="PT15S" unit="Percent">
            <annotation displayName="Disk active write time" locale="en-us"/>
          </PerformanceCounterConfiguration>
          <PerformanceCounterConfiguration counterSpecifier="\PhysicalDisk(_Total)\Disk Transfers/sec" sampleRate="PT15S" unit="CountPerSecond">
            <annotation displayName="Disk operations" locale="en-us"/>
          </PerformanceCounterConfiguration>
          <PerformanceCounterConfiguration counterSpecifier="\PhysicalDisk(_Total)\Disk Reads/sec" sampleRate="PT15S" unit="CountPerSecond">
            <annotation displayName="Disk read operations" locale="en-us"/>
          </PerformanceCounterConfiguration>
          <PerformanceCounterConfiguration counterSpecifier="\PhysicalDisk(_Total)\Disk Writes/sec" sampleRate="PT15S" unit="CountPerSecond">
            <annotation displayName="Disk write operations" locale="en-us"/>
          </PerformanceCounterConfiguration>
          <PerformanceCounterConfiguration counterSpecifier="\PhysicalDisk(_Total)\Disk Bytes/sec" sampleRate="PT15S" unit="BytesPerSecond">
            <annotation displayName="Disk speed" locale="en-us"/>
          </PerformanceCounterConfiguration>
          <PerformanceCounterConfiguration counterSpecifier="\PhysicalDisk(_Total)\Disk Read Bytes/sec" sampleRate="PT15S" unit="BytesPerSecond">
            <annotation displayName="Disk read speed" locale="en-us"/>
          </PerformanceCounterConfiguration>
          <PerformanceCounterConfiguration counterSpecifier="\PhysicalDisk(_Total)\Disk Write Bytes/sec" sampleRate="PT15S" unit="BytesPerSecond">
            <annotation displayName="Disk write speed" locale="en-us"/>
          </PerformanceCounterConfiguration>
          <PerformanceCounterConfiguration counterSpecifier="\PhysicalDisk(_Total)\Avg. Disk Queue Length" sampleRate="PT15S" unit="Count">
            <annotation displayName="Disk average queue length" locale="en-us"/>
          </PerformanceCounterConfiguration>
          <PerformanceCounterConfiguration counterSpecifier="\PhysicalDisk(_Total)\Avg. Disk Read Queue Length" sampleRate="PT15S" unit="Count">
            <annotation displayName="Disk average read queue length" locale="en-us"/>
          </PerformanceCounterConfiguration>
          <PerformanceCounterConfiguration counterSpecifier="\PhysicalDisk(_Total)\Avg. Disk Write Queue Length" sampleRate="PT15S" unit="Count">
            <annotation displayName="Disk average write queue length" locale="en-us"/>
          </PerformanceCounterConfiguration>
          <PerformanceCounterConfiguration counterSpecifier="\LogicalDisk(_Total)\% Free Space" sampleRate="PT15S" unit="Percent">
            <annotation displayName="Disk free space (percentage)" locale="en-us"/>
          </PerformanceCounterConfiguration>
          <PerformanceCounterConfiguration counterSpecifier="\LogicalDisk(_Total)\Free Megabytes" sampleRate="PT15S" unit="Count">
            <annotation displayName="Disk free space (MB)" locale="en-us"/>
          </PerformanceCounterConfiguration>
        </PerformanceCounters>
        <Metrics resourceId="(Update with resource ID for hello VM)" >
            <MetricAggregation scheduledTransferPeriod="PT1H"/>
            <MetricAggregation scheduledTransferPeriod="PT1M"/>
        </Metrics>
        <WindowsEventLog scheduledTransferPeriod="PT1M">
          <DataSource name="Application!*[System[(Level = 1 or Level = 2)]]"/>
          <DataSource name="Security!*[System[(Level = 1 or Level = 2)]"/>
          <DataSource name="System!*[System[(Level = 1 or Level = 2)]]"/>
        </WindowsEventLog>
          </DiagnosticMonitorConfiguration>
        </WadCfg>
        <StorageAccount>(Update with diagnostics storage account name)</StorageAccount>
    </PublicConfig>
    ```

## <a name="next-steps"></a><span data-ttu-id="59ef9-141">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="59ef9-141">Next steps</span></span>
* <span data-ttu-id="59ef9-142">Mer information om hur du använder hello Azure Diagnostics kapaciteten och andra tekniker tootroubleshoot problem finns [aktiverar diagnostik i Azure-molntjänster och virtuella datorer](../../cloud-services/cloud-services-dotnet-diagnostics.md).</span><span class="sxs-lookup"><span data-stu-id="59ef9-142">For additional guidance on using hello Azure Diagnostics capability and other techniques tootroubleshoot problems, see [Enabling Diagnostics in Azure Cloud Services and Virtual Machines](../../cloud-services/cloud-services-dotnet-diagnostics.md).</span></span>
* <span data-ttu-id="59ef9-143">[Diagnostik konfigurationer schemat](https://msdn.microsoft.com/library/azure/mt634524.aspx) förklarar hello olika konfigurationer för XML-alternativ för hello diagnostik tillägg.</span><span class="sxs-lookup"><span data-stu-id="59ef9-143">[Diagnostics configurations schema](https://msdn.microsoft.com/library/azure/mt634524.aspx) explains hello various XML configurations options for hello diagnostics extension.</span></span>

