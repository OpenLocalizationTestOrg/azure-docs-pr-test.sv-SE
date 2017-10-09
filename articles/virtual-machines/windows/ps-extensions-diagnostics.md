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
# <a name="use-powershell-tooenable-azure-diagnostics-in-a-virtual-machine-running-windows"></a>Använd PowerShell tooenable Azure-diagnostik i en virtuell dator som kör Windows
[!INCLUDE [learn-about-deployment-models](../../../includes/learn-about-deployment-models-both-include.md)]

Azure-diagnostik är hello funktion i Azure som gör att hello insamlingen av diagnostiska data på ett distribuerat program. Du kan använda hello diagnostik tillägget toocollect diagnostikdata som programloggarna eller prestandaräknare från en Azure-dator (VM) som kör Windows. Den här artikeln beskriver hur toouse Windows PowerShell tooenable hello diagnostik tillägget för en virtuell dator. Se [hur tooinstall och konfigurera Azure PowerShell](/powershell/azure/overview) för hello kraven för den här artikeln.

## <a name="enable-hello-diagnostics-extension-if-you-use-hello-resource-manager-deployment-model"></a>Aktivera hello diagnostik tillägget om du använder hello Resource Manager-distributionsmodellen
Du kan aktivera hello diagnostik tillägg när du skapar en virtuell Windows-dator via hello Azure Resource Manager-modellen genom att lägga till hello-tillägget configuration toohello Resource Manager-mall. Se [skapa en virtuell Windows-dator med övervakning och diagnostik med hello Azure Resource Manager-mall](extensions-diagnostics-template.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

tooenable hello diagnostik tillägg på en befintlig virtuell dator som har skapats via hello Resource Manager-distributionsmodellen, som du kan använda hello [Set AzureRMVMDiagnosticsExtension](/powershell/module/azurerm.compute/set-azurermvmdiagnosticsextension) PowerShell-cmdlet enligt nedan.

    $vm_resourcegroup = "myvmresourcegroup"
    $vm_name = "myvm"
    $diagnosticsconfig_path = "DiagnosticsPubConfig.xml"

    Set-AzureRmVMDiagnosticsExtension -ResourceGroupName $vm_resourcegroup -VMName $vm_name -DiagnosticsConfigurationPath $diagnosticsconfig_path


*$diagnosticsconfig_path* är hello sökvägen toohello fil som innehåller hello diagnostik konfigurationen i XML, enligt beskrivningen i hello [exempel](#sample-diagnostics-configuration) nedan.  

Om hello diagnostik konfigurationsfil anger en **StorageAccount** element med namnet på ett lagringskonto, sedan hello *Set AzureRMVMDiagnosticsExtension* hello ställer automatiskt in skript tillägget toosend diagnostikdata toothat diagnostiklagringskonto. För den här toowork hello lagringskonto måste toobe i hello samma prenumeration som hello VM.

Om inget **StorageAccount** har angetts i hello diagnostik konfigurationen måste toopass i hello *StorageAccountName* parametern toohello cmdlet. Om hello *StorageAccountName* parametern anges, kommer hello cmdlet alltid använda hello storage-konto som har angetts i parametern hello och inte hello ett som har angetts i konfigurationsfilen för hello diagnostik.

Om hello diagnostik storage-konto är i en annan prenumeration från hello VM, måste tooexplicitly skicka in hello *StorageAccountName* och *StorageAccountKey* parametrar toohello cmdlet. Hej *StorageAccountKey* parametern behövs inte när hello diagnostik storage-konto är i hello samma prenumeration som hello cmdlet automatiskt kan fråga och ange hello nyckelvärdet när du aktiverar hello diagnostik tillägg. Om hello diagnostiklagringskonto finns i en annan prenumeration och sedan hello cmdlet kanske inte kan tooget hello nyckeln automatiskt och du behöver tooexplicitly ange hello nyckel via hello *StorageAccountKey* parameter.  

    Set-AzureRmVMDiagnosticsExtension -ResourceGroupName $vm_resourcegroup -VMName $vm_name -DiagnosticsConfigurationPath $diagnosticsconfig_path -StorageAccountName $diagnosticsstorage_name -StorageAccountKey $diagnosticsstorage_key

När hello diagnostik tillägget har aktiverats på en virtuell dator måste du hello aktuella inställningar med hjälp av hello [Get-AzureRMVmDiagnosticsExtension](/powershell/module/azurerm.compute/get-azurermvmdiagnosticsextension) cmdlet.

    Get-AzureRmVMDiagnosticsExtension -ResourceGroupName $vm_resourcegroup -VMName $vm_name

hello cmdlet returnerar *PublicSettings*, som innehåller hello diagnostik konfiguration. Det finns två typer av konfiguration som stöds, WadCfg och xmlCfg. WadCfg är JSON-konfiguration och xmlCfg är XML-konfiguration i en Base64-kodat format. tooread hello XML, måste du toodecode den.

    $publicsettings = (Get-AzureRmVMDiagnosticsExtension -ResourceGroupName $vm_resourcegroup -VMName $vm_name).PublicSettings
    $encodedconfig = (ConvertFrom-Json -InputObject $publicsettings).xmlCfg
    $xmlconfig = [System.Text.Encoding]::UTF8.GetString([System.Convert]::FromBase64String($encodedconfig))
    Write-Host $xmlconfig

Hej [ta bort AzureRMVmDiagnosticsExtension](/powershell/module/azurerm.compute/remove-azurermvmdiagnosticsextension) kan vara används tooremove hello diagnostik tillägget från hello VM.  

## <a name="enable-hello-diagnostics-extension-if-you-use-hello-classic-deployment-model"></a>Aktivera hello diagnostik tillägget om du använder hello klassiska distributionsmodellen
Du kan använda hello [Set AzureVMDiagnosticsExtension](/powershell/module/azure/set-azurevmdiagnosticsextension) cmdlet tooenable filnamnstillägget diagnostik på en virtuell dator som du skapar genom hello klassiska distributionsmodellen. hello visar följande exempel hur toocreate en ny virtuell dator via hello klassiska distributionsmodellen med hello diagnostik tillägget aktiverat.

    $VM = New-AzureVMConfig -Name $VM -InstanceSize Small -ImageName $VMImage
    $VM = Add-AzureProvisioningConfig -VM $VM -AdminUsername $Username -Password $Password -Windows
    $VM = Set-AzureVMDiagnosticsExtension -DiagnosticsConfigurationPath $Config_Path -VM $VM -StorageContext $Storage_Context
    New-AzureVM -Location $Location -ServiceName $Service_Name -VM $VM

tooenable hello diagnostik tillägg på en befintlig virtuell dator som har skapats via hello klassiska distributionsmodellen, första användning hello [Get-AzureVM](/powershell/module/azure/get-azurevm) cmdlet tooget hello VM-konfiguration. Uppdatera sedan hello VM configuration tooinclude hello diagnostik-tillägget med hjälp av hello [Set AzureVMDiagnosticsExtension](/powershell/module/azure/set-azurevmdiagnosticsextension) cmdlet. Slutligen tillämpa hello uppdateras configuration toohello VM med hjälp av [uppdatering AzureVM](/powershell/module/azure/update-azurevm).

    $VM = Get-AzureVM -ServiceName $Service_Name -Name $VM_Name
    $VM_Update = Set-AzureVMDiagnosticsExtension -DiagnosticsConfigurationPath $Config_Path -VM $VM -StorageContext $Storage_Context
    Update-AzureVM -ServiceName $Service_Name -Name $VM_Name -VM $VM_Update.VM

## <a name="sample-diagnostics-configuration"></a>Diagnostik exempelkonfiguration
hello följande XML kan användas för hello diagnostik offentliga konfiguration med hello ovan skript. Den här exempelkonfiguration överför olika prestandaräknare toohello diagnostiklagringskonto, tillsammans med fel från hello program, säkerhet och systemkanaler i händelseloggarna i Windows hello och eventuella fel från hello diagnostik infrastruktur för loggar.

hello konfiguration måste toobe uppdaterade tooinclude hello följande:

* Hej *resourceID* attribut för hello **mått** element måste toobe uppdateras med hello resurs-ID för hello VM.
  
  * hello resurs-ID kan konstrueras med hjälp av hello följer mönstret ”: /subscriptions/ {*prenumerations-ID för hello prenumerationen med hello VM*} /resourceGroups/ {*hello namn för hello VM*} / providers/Microsoft.Compute/virtualMachines/ {*hello namn på virtuell*} ”.
  * Till exempel om hello prenumerations-ID för hello prenumeration där hello VM körs är **11111111-1111-1111-1111-111111111111**, hello resursgruppens namn för resursgruppen hello används **MyResourceGroup**, och hello VM namn är **MyWindowsVM**, hello värde för *resourceID* skulle vara:
    
      ```
      <Metrics resourceId="/subscriptions/11111111-1111-1111-1111-111111111111/resourceGroups/MyResourceGroup/providers/Microsoft.Compute/virtualMachines/MyWindowsVM" >
      ```
  * Mer information om hur är genererade utifrån hello prestandaräknare och mått konfiguration finns [Azure Diagnostics mått tabellen i lagring](extensions-diagnostics-template.md#wadmetrics-tables-in-storage).
* Hej **StorageAccount** element måste uppdateras med hello namnet på hello diagnostiklagringskonto toobe.
  
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

## <a name="next-steps"></a>Nästa steg
* Mer information om hur du använder hello Azure Diagnostics kapaciteten och andra tekniker tootroubleshoot problem finns [aktiverar diagnostik i Azure-molntjänster och virtuella datorer](../../cloud-services/cloud-services-dotnet-diagnostics.md).
* [Diagnostik konfigurationer schemat](https://msdn.microsoft.com/library/azure/mt634524.aspx) förklarar hello olika konfigurationer för XML-alternativ för hello diagnostik tillägg.

