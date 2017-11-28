---
title: "aaaAttach tooa en data-disk Windows VM i Azure med hjälp av PowerShell | Microsoft Docs"
description: "Hur tooattach nya eller befintliga data disk tooa Windows VM använda PowerShell med hello Resource Manager-modellen."
services: virtual-machines-windows
documentationcenter: 
author: cynthn
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 02/07/2017
ms.author: cynthn
ms.openlocfilehash: 12ffdd4ced791ba0948047d3af24ad73e36c7ad6
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="attach-a-data-disk-tooa-windows-vm-using-powershell"></a><span data-ttu-id="35a8a-103">Koppla en virtuell Windows-dator med data disk tooa med hjälp av PowerShell</span><span class="sxs-lookup"><span data-stu-id="35a8a-103">Attach a data disk tooa Windows VM using PowerShell</span></span>

<span data-ttu-id="35a8a-104">Den här artikeln visar hur tooattach både nya och befintliga diskar tooa Windows-dator med hjälp av PowerShell.</span><span class="sxs-lookup"><span data-stu-id="35a8a-104">This article shows you how tooattach both new and existing disks tooa Windows virtual machine using PowerShell.</span></span> <span data-ttu-id="35a8a-105">Om den virtuella datorn använder hanterade diskar, kan du koppla ytterligare data för hanterade diskar.</span><span class="sxs-lookup"><span data-stu-id="35a8a-105">If your VM uses managed disks, you can attach additional managed data disks.</span></span> <span data-ttu-id="35a8a-106">Du kan också koppla ohanterade data diskar tooa VM som använder ohanterade diskar i ett lagringskonto.</span><span class="sxs-lookup"><span data-stu-id="35a8a-106">You can also attach unmanaged data disks tooa VM that uses unmanaged disks in a storage account.</span></span>

<span data-ttu-id="35a8a-107">Innan du gör detta kan du granska dessa tips:</span><span class="sxs-lookup"><span data-stu-id="35a8a-107">Before you do this, review these tips:</span></span>
* <span data-ttu-id="35a8a-108">hello storleken på hello virtuella styr hur många datadiskar som du kan bifoga.</span><span class="sxs-lookup"><span data-stu-id="35a8a-108">hello size of hello virtual machine controls how many data disks you can attach.</span></span> <span data-ttu-id="35a8a-109">Mer information finns i [storlekar för virtuella datorer](sizes.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="35a8a-109">For details, see [Sizes for virtual machines](sizes.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span>
* <span data-ttu-id="35a8a-110">toouse Premium-lagring, behöver du en Premium-lagring aktiverat VM-storlek som hello DS-serien eller GS-serien virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="35a8a-110">toouse Premium storage, you'll need a Premium Storage enabled VM size like hello DS-series or GS-series virtual machine.</span></span> <span data-ttu-id="35a8a-111">Du kan använda diskar från Premium- och Standard storage-konton med dessa virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="35a8a-111">You can use disks from both Premium and Standard storage accounts with these virtual machines.</span></span> <span data-ttu-id="35a8a-112">Premium-lagring är tillgänglig i vissa regioner.</span><span class="sxs-lookup"><span data-stu-id="35a8a-112">Premium storage is available in certain regions.</span></span> <span data-ttu-id="35a8a-113">Mer information finns i [Premium-lagring: högpresterande lagring för arbetsbelastningar på virtuella Azure](../../storage/common/storage-premium-storage.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="35a8a-113">For details, see [Premium Storage: High-Performance Storage for Azure Virtual Machine Workloads](../../storage/common/storage-premium-storage.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span>

## <a name="before-you-begin"></a><span data-ttu-id="35a8a-114">Innan du börjar</span><span class="sxs-lookup"><span data-stu-id="35a8a-114">Before you begin</span></span>
<span data-ttu-id="35a8a-115">Om du använder PowerShell kan du se till att du har hello senaste versionen av hello AzureRM.Compute PowerShell-modulen.</span><span class="sxs-lookup"><span data-stu-id="35a8a-115">If you use PowerShell, make sure that you have hello latest version of hello AzureRM.Compute PowerShell module.</span></span> <span data-ttu-id="35a8a-116">Kör följande kommando tooinstall hello den.</span><span class="sxs-lookup"><span data-stu-id="35a8a-116">Run hello following command tooinstall it.</span></span>

```powershell
Install-Module AzureRM.Compute -RequiredVersion 2.6.0
```
<span data-ttu-id="35a8a-117">Mer information finns i [Azure PowerShell versionshantering](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="35a8a-117">For more information, see [Azure PowerShell Versioning](/powershell/azure/overview).</span></span>


## <a name="add-an-empty-data-disk-tooa-virtual-machine"></a><span data-ttu-id="35a8a-118">Lägg till en tom disk tooa virtuell dator</span><span class="sxs-lookup"><span data-stu-id="35a8a-118">Add an empty data disk tooa virtual machine</span></span>

<span data-ttu-id="35a8a-119">Det här exemplet visar hur tooadd en tom data disk tooan befintlig virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="35a8a-119">This example shows how tooadd an empty data disk tooan existing virtual machine.</span></span>

### <a name="using-managed-disks"></a><span data-ttu-id="35a8a-120">Med hjälp av hanterade diskar</span><span class="sxs-lookup"><span data-stu-id="35a8a-120">Using managed disks</span></span>

```powershell
$rgName = 'myResourceGroup'
$vmName = 'myVM'
$location = 'West Central US' 
$storageType = 'PremiumLRS'
$dataDiskName = $vmName + '_datadisk1'

$diskConfig = New-AzureRmDiskConfig -AccountType $storageType -Location $location -CreateOption Empty -DiskSizeGB 128

$dataDisk1 = New-AzureRmDisk -DiskName $dataDiskName -Disk $diskConfig -ResourceGroupName $rgName

$vm = Get-AzureRmVM -Name $vmName -ResourceGroupName $rgName 

$vm = Add-AzureRmVMDataDisk -VM $vm -Name $dataDiskName -CreateOption Attach -ManagedDiskId $dataDisk1.Id -Lun 1

Update-AzureRmVM -VM $vm -ResourceGroupName $rgName
```

### <a name="using-unmanaged-disks-in-a-storage-account"></a><span data-ttu-id="35a8a-121">Med hjälp av ohanterade diskar i ett lagringskonto</span><span class="sxs-lookup"><span data-stu-id="35a8a-121">Using unmanaged disks in a storage account</span></span>

```powershell
    $vm = Get-AzureRmVM -ResourceGroupName $rgName -Name $vmName
    Add-AzureRmVMDataDisk -VM $vm -Name "disk-name" -VhdUri "https://mystore1.blob.core.windows.net/vhds/datadisk1.vhd" -LUN 0 -Caching ReadWrite -DiskSizeinGB 1 -CreateOption Empty
    Update-AzureRmVM -ResourceGroupName $rgName -VM $vm
```


### <a name="initialize-hello-disk"></a><span data-ttu-id="35a8a-122">Initiera hello disk</span><span class="sxs-lookup"><span data-stu-id="35a8a-122">Initialize hello disk</span></span>

<span data-ttu-id="35a8a-123">När du lägger till en tom disk måste tooinitialize den.</span><span class="sxs-lookup"><span data-stu-id="35a8a-123">After you add an empty disk, you need tooinitialize it.</span></span> <span data-ttu-id="35a8a-124">tooinitialize hello disk som du kan logga in tooa VM och använda Diskhantering.</span><span class="sxs-lookup"><span data-stu-id="35a8a-124">tooinitialize hello disk, you can log in tooa VM and use disk management.</span></span> <span data-ttu-id="35a8a-125">Om du har aktiverat WinRM och ett certifikat på hello VM när du skapade den, kan du använda PowerShell tooinitialize hello fjärrdisk.</span><span class="sxs-lookup"><span data-stu-id="35a8a-125">If you enabled WinRM and a certificate on hello VM when you created it, you can use remote PowerShell tooinitialize hello disk.</span></span> <span data-ttu-id="35a8a-126">Du kan också använda tillägget för anpassat skript:</span><span class="sxs-lookup"><span data-stu-id="35a8a-126">You can also use a custom script extension:</span></span> 

```powershell
    $location = "location-name"
    $scriptName = "script-name"
    $fileName = "script-file-name"
    Set-AzureRmVMCustomScriptExtension -ResourceGroupName $rgName -Location $locName -VMName $vmName -Name $scriptName -TypeHandlerVersion "1.4" -StorageAccountName "mystore1" -StorageAccountKey "primary-key" -FileName $fileName -ContainerName "scripts"
```
        
<span data-ttu-id="35a8a-127">hello skriptfilen kan innehålla ungefär som den här koden tooinitialize hello diskar:</span><span class="sxs-lookup"><span data-stu-id="35a8a-127">hello script file can contain something like this code tooinitialize hello disks:</span></span>

```powershell
    $disks = Get-Disk | Where partitionstyle -eq 'raw' | sort number

    $letters = 70..89 | ForEach-Object { [char]$_ }
    $count = 0
    $labels = "data1","data2"

    foreach ($disk in $disks) {
        $driveLetter = $letters[$count].ToString()
        $disk | 
        Initialize-Disk -PartitionStyle MBR -PassThru |
        New-Partition -UseMaximumSize -DriveLetter $driveLetter |
        Format-Volume -FileSystem NTFS -NewFileSystemLabel $labels[$count] -Confirm:$false -Force
    $count++
    }
```


## <a name="attach-an-existing-data-disk-tooa-vm"></a><span data-ttu-id="35a8a-128">Bifoga en befintlig data disk tooa VM</span><span class="sxs-lookup"><span data-stu-id="35a8a-128">Attach an existing data disk tooa VM</span></span>

<span data-ttu-id="35a8a-129">Du kan även bifoga en befintlig virtuell Hårddisk som en hanterad disk tooa virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="35a8a-129">You can also attach an existing VHD as a managed data disk tooa virtual machine.</span></span> 

### <a name="using-managed-disks"></a><span data-ttu-id="35a8a-130">Med hjälp av hanterade diskar</span><span class="sxs-lookup"><span data-stu-id="35a8a-130">Using managed disks</span></span>

```powershell
$rgName = 'myRG'
$vmName = 'ContosoMdPir3'
$location = 'West Central US' 
$storageType = 'PremiumLRS'
$dataDiskName = $vmName + '_datadisk2'
$dataVhdUri = 'https://mystorageaccount.blob.core.windows.net/vhds/managed_data_disk.vhd' 

$diskConfig = New-AzureRmDiskConfig -AccountType $storageType -Location $location -CreateOption Import -SourceUri $dataVhdUri -DiskSizeGB 128

$dataDisk2 = New-AzureRmDisk -DiskName $dataDiskName -Disk $diskConfig -ResourceGroupName $rgName

$vm = Get-AzureRmVM -Name $vmName -ResourceGroupName $rgName 

$vm = Add-AzureRmVMDataDisk -VM $vm -Name $dataDiskName -CreateOption Attach -ManagedDiskId $dataDisk2.Id -Lun 2

Update-AzureRmVM -VM $vm -ResourceGroupName $rgName
```

## <a name="next-steps"></a><span data-ttu-id="35a8a-131">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="35a8a-131">Next steps</span></span>

<span data-ttu-id="35a8a-132">Skapa en [ögonblicksbild](snapshot-copy-managed-disk.md).</span><span class="sxs-lookup"><span data-stu-id="35a8a-132">Create a [snapshot](snapshot-copy-managed-disk.md).</span></span>
