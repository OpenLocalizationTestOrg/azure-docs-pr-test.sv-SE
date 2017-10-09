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
# <a name="increase-hello-size-of-a-data-disk-attached-tooa-windows-vm"></a><span data-ttu-id="8fdd4-103">Öka hello storleken för en data på en disk ansluten-tooa Windows VM</span><span class="sxs-lookup"><span data-stu-id="8fdd4-103">Increase hello size of a data disk attached tooa Windows VM</span></span>

<span data-ttu-id="8fdd4-104">Om du behöver tooincrease hello storleken på hello data disk ansluten tooyour virtuella datorn, kan du öka hello storlek med hjälp av PowerShell.</span><span class="sxs-lookup"><span data-stu-id="8fdd4-104">If you need tooincrease hello size of hello data disk attached tooyour virtual machine, you can increase hello size using PowerShell.</span></span> <span data-ttu-id="8fdd4-105">När du ökar hello storleken på disken för hello-data i inställningarna för hello Azure VM, behöver du också tooallocate hello nya diskutrymmet inom hello VM.</span><span class="sxs-lookup"><span data-stu-id="8fdd4-105">After you increase hello size of hello data disk in hello Azure VM settings, you also need tooallocate hello new disk space within hello VM.</span></span>


## <a name="use-powershell-tooincrease-hello-size-of-a-managed-data-disk"></a><span data-ttu-id="8fdd4-106">Använd Powershell tooincrease hello storleken på en hanterad datadisk</span><span class="sxs-lookup"><span data-stu-id="8fdd4-106">Use Powershell tooincrease hello size of a managed data disk</span></span>

<span data-ttu-id="8fdd4-107">tooincrease hello storlek på en hanterad datadisk Använd hello följande PowerShell-cmdlets:</span><span class="sxs-lookup"><span data-stu-id="8fdd4-107">tooincrease hello size of a managed data disk, use hello following PowerShell cmdlets:</span></span>

|                                                                    |                                                            |
|--------------------------------------------------------------------|------------------------------------------------------------|
| [<span data-ttu-id="8fdd4-108">Get-AzureRMReseourceGroup</span><span class="sxs-lookup"><span data-stu-id="8fdd4-108">Get-AzureRMReseourceGroup</span></span>](/powershell/module/azurerm.resources/get-azurermresourcegroup) | [<span data-ttu-id="8fdd4-109">Get-AzureRMVM</span><span class="sxs-lookup"><span data-stu-id="8fdd4-109">Get-AzureRMVM</span></span>](/powershell/module/azurerm.compute/get-azurermvm)                 |
| [<span data-ttu-id="8fdd4-110">Stoppa AzureRMVM</span><span class="sxs-lookup"><span data-stu-id="8fdd4-110">Stop-AzureRMVM</span></span>](/powershell/module/azurerm.compute/stop-azurermvm)                        | [<span data-ttu-id="8fdd4-111">Uppdatera AzureRmDisk</span><span class="sxs-lookup"><span data-stu-id="8fdd4-111">Update-AzureRmDisk</span></span>](/powershell/module/azurerm.compute/Update-AzureRmDisk) |
 | [<span data-ttu-id="8fdd4-112">Start-AzureRmVM</span><span class="sxs-lookup"><span data-stu-id="8fdd4-112">Start-AzureRmVM</span></span>](/powershell/module/azurerm.compute/start-azurermvm)             |
<br>

<span data-ttu-id="8fdd4-113">hello följande skript beskriver hur du hämtar information om hello VM, välja hello datadisk och att ange hello nya storlek.</span><span class="sxs-lookup"><span data-stu-id="8fdd4-113">hello following script will walk you through getting hello VM information, selecting hello data disk and specifying hello new size.</span></span>

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

## <a name="use-powershell-tooincrease-hello-size-of-an-unmanaged-data-disk"></a><span data-ttu-id="8fdd4-114">Använd PowerShell tooincrease hello storleken på en ohanterad datadisk</span><span class="sxs-lookup"><span data-stu-id="8fdd4-114">Use PowerShell tooincrease hello size of an unmanaged data disk</span></span>

<span data-ttu-id="8fdd4-115">tooincrease hello storleken på datadiskar som ohanterad i ett lagringskonto, Använd hello följande PowerShell-cmdlets:</span><span class="sxs-lookup"><span data-stu-id="8fdd4-115">tooincrease hello size of unmanaged data disks in a storage account, use hello following PowerShell cmdlets:</span></span>

|                                                                    |                                                            |
|--------------------------------------------------------------------|------------------------------------------------------------|
| [<span data-ttu-id="8fdd4-116">Get-AzureRMStorageAccount</span><span class="sxs-lookup"><span data-stu-id="8fdd4-116">Get-AzureRMStorageAccount</span></span>](/powershell/module/azurerm.storage/get-azurermstorageaccount) | [<span data-ttu-id="8fdd4-117">Get-AzureRMVM</span><span class="sxs-lookup"><span data-stu-id="8fdd4-117">Get-AzureRMVM</span></span>](/powershell/module/azurerm.compute/get-azurermvm)                 |
| [<span data-ttu-id="8fdd4-118">Stoppa AzureRMVM</span><span class="sxs-lookup"><span data-stu-id="8fdd4-118">Stop-AzureRMVM</span></span>](/powershell/module/azurerm.compute/stop-azurermvm)                       | [<span data-ttu-id="8fdd4-119">Ange AzureRmVMDataDisk</span><span class="sxs-lookup"><span data-stu-id="8fdd4-119">Set-AzureRmVMDataDisk</span></span>](/powershell/module/azurerm.compute/set-azurermvmdatadisk) |
| [<span data-ttu-id="8fdd4-120">Uppdatera AzureRmVM</span><span class="sxs-lookup"><span data-stu-id="8fdd4-120">Update-AzureRmVM</span></span>](/powershell/module/azurerm.compute/update-azurermvm)                   | [<span data-ttu-id="8fdd4-121">Start-AzureRmVM</span><span class="sxs-lookup"><span data-stu-id="8fdd4-121">Start-AzureRmVM</span></span>](/powershell/module/azurerm.compute/start-azurermvm)             |

<br>

<span data-ttu-id="8fdd4-122">hello följande skript beskriver hur du hämtar hello kontoinformation för virtuell dator och lagring, välja hello datadisk och ange ny storlek för hello.</span><span class="sxs-lookup"><span data-stu-id="8fdd4-122">hello following script will walk you through getting hello VM and storage account information, selecting hello data disk and specifying hello new size.</span></span>

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

## <a name="allocate-hello-unallocated-disk-space"></a><span data-ttu-id="8fdd4-123">Allokera hello ledigt diskutrymme</span><span class="sxs-lookup"><span data-stu-id="8fdd4-123">Allocate hello unallocated disk space</span></span>

<span data-ttu-id="8fdd4-124">När du har gjort hello enhet större, måste tooallocate hello nytt ledigt utrymme från inom hello VM.</span><span class="sxs-lookup"><span data-stu-id="8fdd4-124">Once you have made hello drive larger, you need tooallocate hello new unallocated space from within hello VM.</span></span> <span data-ttu-id="8fdd4-125">Du kan ansluta toohello VM använda Diskhantering (diskmgmt.msc) tooallocate hello utrymme.</span><span class="sxs-lookup"><span data-stu-id="8fdd4-125">tooallocate hello space, you can connect toohello VM use Disk Management (diskmgmt.msc).</span></span> <span data-ttu-id="8fdd4-126">Eller, om du har aktiverat WinRM och ett certifikat på hello VM när du skapade den, kan du använda PowerShell tooinitialize hello fjärrdisk.</span><span class="sxs-lookup"><span data-stu-id="8fdd4-126">Or, if you enabled WinRM and a certificate on hello VM when you created it, you can use remote PowerShell tooinitialize hello disk.</span></span> <span data-ttu-id="8fdd4-127">Du kan också använda tillägget för anpassat skript:</span><span class="sxs-lookup"><span data-stu-id="8fdd4-127">You can also use a custom script extension:</span></span>

```powershell
    $location = "location-name"
    $scriptName = "script-name"
    $fileName = "script-file-name"
    Set-AzureRmVMCustomScriptExtension -ResourceGroupName $rgName -Location $locName -VMName $vmName -Name $scriptName -TypeHandlerVersion "1.4" -StorageAccountName "mystore1" -StorageAccountKey "primary-key" -FileName $fileName -ContainerName "scripts"
```

<span data-ttu-id="8fdd4-128">hello skriptfilen kan innehålla ungefär som den här koden tooincrease hello enhet allokering toohello maxstorleken hello diskar:</span><span class="sxs-lookup"><span data-stu-id="8fdd4-128">hello script file can contain something like this code tooincrease hello drive allocation toohello maximum size hello disks:</span></span>

```powershell
$driveLetter= "F"

$MaxSize = (Get-PartitionSupportedSize -DriveLetter $driveLetter).sizeMax

Resize-Partition -DriveLetter $driveLetter -Size $MaxSize
```

## <a name="next-steps"></a><span data-ttu-id="8fdd4-129">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="8fdd4-129">Next Steps</span></span>
- [<span data-ttu-id="8fdd4-130">Mer information om diskar och virtuella hårddiskar</span><span class="sxs-lookup"><span data-stu-id="8fdd4-130">Learn more about disks and VHDs</span></span>](../../storage/storage-about-disks-and-vhds-windows.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)
