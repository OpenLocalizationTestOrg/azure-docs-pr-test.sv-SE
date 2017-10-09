---
title: aaaExpand en datadisk kopplade tooa Windows VM i Azure | Microsoft Docs
description: "Expandera hello storleken på datadisk som är bifogade tooa Windows-dator med hjälp av PowerShell."
services: virtual-machines-windows
documentationcenter: na
author: cynthn
manager: timlt
editor: 
tags: 
ms.assetid: 
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/02/2017
ms.author: cynthn
ms.openlocfilehash: b16ad0da9cff9dfffc9dc9ec7dd72891e7ddd745
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="increase-hello-size-of-a-data-disk-attached-tooa-windows-vm"></a>Öka hello storleken för en data på en disk ansluten-tooa Windows VM

Om du behöver tooincrease hello storleken på hello data disk ansluten tooyour virtuella datorn, kan du öka hello storlek med hjälp av PowerShell. När du ökar hello storleken på disken för hello-data i inställningarna för hello Azure VM, behöver du också tooallocate hello nya diskutrymmet inom hello VM.


## <a name="use-powershell-tooincrease-hello-size-of-a-managed-data-disk"></a>Använd Powershell tooincrease hello storleken på en hanterad datadisk

tooincrease hello storlek på en hanterad datadisk Använd hello följande PowerShell-cmdlets:

|                                                                    |                                                            |
|--------------------------------------------------------------------|------------------------------------------------------------|
| [Get-AzureRMReseourceGroup](/powershell/module/azurerm.resources/get-azurermresourcegroup) | [Get-AzureRMVM](/powershell/module/azurerm.compute/get-azurermvm)                 |
| [Stoppa AzureRMVM](/powershell/module/azurerm.compute/stop-azurermvm)                        | [Uppdatera AzureRmDisk](/powershell/module/azurerm.compute/Update-AzureRmDisk) |
 | [Start-AzureRmVM](/powershell/module/azurerm.compute/start-azurermvm)             |
<br>

hello följande skript beskriver hur du hämtar information om hello VM, välja hello datadisk och att ange hello nya storlek.

```powershell
# Select resource group

    $rg = Get-AzureRMResourceGroup | Out-GridView `
        -Title "Select hello resource group" `
        -PassThru

    $rgName = $rg.ResourceGroupName

# Select hello VM

    $vm = Get-AzureRMVM -ResourceGroupName $rgName `
        | Out-GridView `
            -Title "Select a VM" `
             -PassThru

# Select data disk

    $disk = $vm.StorageProfile.DataDisks | Out-GridView `
        -Title "Select a data disk" `
        -PassThru

# Specify a larger size for hello data disk

    $size =  Read-Host `
        -Prompt "New size in GB"

# Stop and Deallocate VM prior tooresizing data disk

    $vm | Stop-AzureRMVM -Force

# Set hello new disk size

    $diskUpdateConfig = New-AzureRmDiskUpdateConfig -DiskSizeGB $size

# Update hello configuration in Azure

    $managedDisk = Get-AzureRmResource -ResourceId $disk.ManagedDisk.Id
    Update-AzureRmDisk -DiskName $managedDisk.ResourceName -ResourceGroupName $managedDisk.ResourceGroupName -DiskUpdate $diskUpdateConfig

# Start hello VM

    Start-AzureRmVM -ResourceGroupName $rgName -VMName $vm.name
```

## <a name="use-powershell-tooincrease-hello-size-of-an-unmanaged-data-disk"></a>Använd PowerShell tooincrease hello storleken på en ohanterad datadisk

tooincrease hello storleken på datadiskar som ohanterad i ett lagringskonto, Använd hello följande PowerShell-cmdlets:

|                                                                    |                                                            |
|--------------------------------------------------------------------|------------------------------------------------------------|
| [Get-AzureRMStorageAccount](/powershell/module/azurerm.storage/get-azurermstorageaccount) | [Get-AzureRMVM](/powershell/module/azurerm.compute/get-azurermvm)                 |
| [Stoppa AzureRMVM](/powershell/module/azurerm.compute/stop-azurermvm)                       | [Ange AzureRmVMDataDisk](/powershell/module/azurerm.compute/set-azurermvmdatadisk) |
| [Uppdatera AzureRmVM](/powershell/module/azurerm.compute/update-azurermvm)                   | [Start-AzureRmVM](/powershell/module/azurerm.compute/start-azurermvm)             |

<br>

hello följande skript beskriver hur du hämtar hello kontoinformation för virtuell dator och lagring, välja hello datadisk och ange ny storlek för hello.

```powershell

# Select Azure Storage Account

    $storageAccount =
        Get-AzureRMStorageAccount | Out-GridView `
            -Title "Select Azure Storage Account" `
            -PassThru

    $rgName = $storageAccount.ResourceGroupName

# Select hello VM

    $vm = Get-AzureRMVM `
    -ResourceGroupName $rgName | Out-GridView `
            -Title "Select a VM …" `
            -PassThru

# Select Data Disk tooresize

    $disk =
        $vm.DataDiskNames | Out-GridView `
            -Title "Select a data disk tooresize" `
            -PassThru


# Specify a larger data disk size

    $size =  Read-Host `
        -Prompt "New size in GB"

# Stop and Deallocate VM prior tooresizing data disk

    $vm | Stop-AzureRMVM -Force

# Set hello new disk size

    Set-AzureRmVMDataDisk -VM $vm -Name "$disk" `
        -DiskSizeInGB $size

# Update hello configuration in Azure

    Update-AzureRmVM -VM $vm -ResourceGroupName $rgName

# Start hello VM
    Start-AzureRmVM -ResourceGroupName $rgName `
    -VMName $vm.name

```

## <a name="allocate-hello-unallocated-disk-space"></a>Allokera hello ledigt diskutrymme

När du har gjort hello enhet större, måste tooallocate hello nytt ledigt utrymme från inom hello VM. Du kan ansluta toohello VM använda Diskhantering (diskmgmt.msc) tooallocate hello utrymme. Eller, om du har aktiverat WinRM och ett certifikat på hello VM när du skapade den, kan du använda PowerShell tooinitialize hello fjärrdisk. Du kan också använda tillägget för anpassat skript:

```powershell
    $location = "location-name"
    $scriptName = "script-name"
    $fileName = "script-file-name"
    Set-AzureRmVMCustomScriptExtension -ResourceGroupName $rgName -Location $locName -VMName $vmName -Name $scriptName -TypeHandlerVersion "1.4" -StorageAccountName "mystore1" -StorageAccountKey "primary-key" -FileName $fileName -ContainerName "scripts"
```

hello skriptfilen kan innehålla ungefär som den här koden tooincrease hello enhet allokering toohello maxstorleken hello diskar:

```powershell
$driveLetter= "F"

$MaxSize = (Get-PartitionSupportedSize -DriveLetter $driveLetter).sizeMax

Resize-Partition -DriveLetter $driveLetter -Size $MaxSize
```

## <a name="next-steps"></a>Nästa steg
- [Mer information om diskar och virtuella hårddiskar](../../storage/storage-about-disks-and-vhds-windows.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)
