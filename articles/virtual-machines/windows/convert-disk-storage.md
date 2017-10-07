---
title: "aaaConvert Azure hanterade diskar lagring från standard toopremium och vice versa | Microsoft Docs"
description: "Hur hanteras tooconvert Azure diskar från standard toopremium och vice versa med hjälp av Azure PowerShell."
services: virtual-machines-windows
documentationcenter: 
author: ramankum
manager: kavithag
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 08/07/2017
ms.author: ramankum
ms.openlocfilehash: 11f35cde216e91c0599d3619682686e8eb162fad
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="convert-azure-managed-disks-storage-from-standard-toopremium-and-vice-versa"></a><span data-ttu-id="2c5d2-103">Konvertera Azure hanterade diskar lagring från standard toopremium och vice versa</span><span class="sxs-lookup"><span data-stu-id="2c5d2-103">Convert Azure managed disks storage from standard toopremium, and vice versa</span></span>

<span data-ttu-id="2c5d2-104">Hanterade diskar erbjuder två alternativ för lagring: [Premium](../../storage/storage-premium-storage.md) (SSD-baserad) och [Standard](../../storage/storage-standard-storage.md) (HDD-baserat).</span><span class="sxs-lookup"><span data-stu-id="2c5d2-104">Managed disks offers two storage options: [Premium](../../storage/storage-premium-storage.md) (SSD-based) and [Standard](../../storage/storage-standard-storage.md) (HDD-based).</span></span> <span data-ttu-id="2c5d2-105">Det gör att du tooeasily växla mellan hello två alternativ med minimal avbrottstid baserat på dina prestandabehov.</span><span class="sxs-lookup"><span data-stu-id="2c5d2-105">It allows you tooeasily switch between hello two options with minimal downtime based on your performance needs.</span></span> <span data-ttu-id="2c5d2-106">Den här funktionen är inte tillgänglig för ohanterade diskar.</span><span class="sxs-lookup"><span data-stu-id="2c5d2-106">This capability is not available for unmanaged disks.</span></span> <span data-ttu-id="2c5d2-107">Men du kan enkelt [konvertera toomanaged diskar](convert-unmanaged-to-managed-disks.md) tooeasily växla mellan hello två alternativ.</span><span class="sxs-lookup"><span data-stu-id="2c5d2-107">But you can easily [convert toomanaged disks](convert-unmanaged-to-managed-disks.md) tooeasily switch between hello two options.</span></span>

<span data-ttu-id="2c5d2-108">Den här artikeln visar hur tooconvert hanterade diskar från standard toopremium och vice versa med hjälp av Azure PowerShell.</span><span class="sxs-lookup"><span data-stu-id="2c5d2-108">This article shows you how tooconvert managed disks from standard toopremium, and vice versa by using Azure PowerShell.</span></span> <span data-ttu-id="2c5d2-109">Om du behöver tooinstall eller uppgradera den [installera och konfigurera Azure PowerShell](/powershell/azure/install-azurerm-ps.md).</span><span class="sxs-lookup"><span data-stu-id="2c5d2-109">If you need tooinstall or upgrade it, see [Install and configure Azure PowerShell](/powershell/azure/install-azurerm-ps.md).</span></span>

## <a name="before-you-begin"></a><span data-ttu-id="2c5d2-110">Innan du börjar</span><span class="sxs-lookup"><span data-stu-id="2c5d2-110">Before you begin</span></span>

* <span data-ttu-id="2c5d2-111">konvertering av hello kräver en omstart av hello VM, så schemalägga hello migrering av diskar lagringen för en befintlig underhållsfönster.</span><span class="sxs-lookup"><span data-stu-id="2c5d2-111">hello conversion requires a restart of hello VM, so schedule hello migration of your disks storage during a pre-existing maintenance window.</span></span> 
* <span data-ttu-id="2c5d2-112">Om du använder ohanterade diskar först [konvertera toomanaged diskar](convert-unmanaged-to-managed-disks.md) toouse den här artikeln tooswitch mellan hello två alternativ för lagring.</span><span class="sxs-lookup"><span data-stu-id="2c5d2-112">If you are using unmanaged disks, first [convert toomanaged disks](convert-unmanaged-to-managed-disks.md) toouse this article tooswitch between hello two storage options.</span></span> 


## <a name="convert-all-hello-managed-disks-of-a-vm-from-standard-toopremium-and-vice-versa"></a><span data-ttu-id="2c5d2-113">Konvertera alla hello hanterade diskar på en virtuell dator från standard toopremium och vice versa</span><span class="sxs-lookup"><span data-stu-id="2c5d2-113">Convert all hello managed disks of a VM from standard toopremium, and vice versa</span></span>

<span data-ttu-id="2c5d2-114">I följande exempel hello, visar vi hur tooswitch alla hello diskar på en virtuell dator från standard toopremium lagring.</span><span class="sxs-lookup"><span data-stu-id="2c5d2-114">In hello following example, we show how tooswitch all hello disks of a VM from standard toopremium storage.</span></span> <span data-ttu-id="2c5d2-115">toouse premium hanterade diskar måste den virtuella datorn använda en [VM-storlek](sizes.md) som har stöd för premium-lagring.</span><span class="sxs-lookup"><span data-stu-id="2c5d2-115">toouse premium managed disks, your VM must use a [VM size](sizes.md) that supports premium storage.</span></span> <span data-ttu-id="2c5d2-116">Det här exemplet växlar även tooa storlek som har stöd för premium-lagring.</span><span class="sxs-lookup"><span data-stu-id="2c5d2-116">This example also switches tooa size that supports premium storage.</span></span>

```powershell
# Name of hello resource group that contains hello VM
$rgName = 'yourResourceGroup'

# Name of hello your virtual machine
$vmName = 'yourVM'

# Choose between StandardLRS and PremiumLRS based on your scenario
$storageType = 'PremiumLRS'

# Premium capable size
# Required only if converting storage from standard toopremium
$size = 'Standard_DS2_v2'
$vm = Get-AzureRmVM -Name $vmName -resourceGroupName $rgName

# Stop and deallocate hello VM before changing hello size
Stop-AzureRmVM -ResourceGroupName $rgName -Name $vmName -Force

# Change hello VM size tooa size that supports premium storage
# Skip this step if converting storage from premium toostandard
$vm.HardwareProfile.VmSize = $size
Update-AzureRmVM -VM $vm -ResourceGroupName $rgName

# Get all disks in hello resource group of hello VM
$vmDisks = Get-AzureRmDisk -ResourceGroupName $rgName 

# For disks that belong toohello selected VM, convert toopremium storage
foreach ($disk in $vmDisks)
{
    if ($disk.OwnerId -eq $vm.Id)
    {
        $diskUpdateConfig = New-AzureRmDiskUpdateConfig –AccountType $storageType
        Update-AzureRmDisk -DiskUpdate $diskUpdateConfig -ResourceGroupName $rgName `
        -DiskName $disk.Name
    }
}

Start-AzureRmVM -ResourceGroupName $rgName -Name $vmName
```
## <a name="convert-a-managed-disk-from-standard-toopremium-and-vice-versa"></a><span data-ttu-id="2c5d2-117">Konverterar en hanterad disk från standard toopremium och vice versa</span><span class="sxs-lookup"><span data-stu-id="2c5d2-117">Convert a managed disk from standard toopremium, and vice versa</span></span>

<span data-ttu-id="2c5d2-118">Du kanske vill dina kostnader toohave blandning av standard-och premium tooreduce för arbetsbelastningen för utveckling och testning.</span><span class="sxs-lookup"><span data-stu-id="2c5d2-118">For your dev/test workload, you may want toohave mixture of standard and premium disks tooreduce your cost.</span></span> <span data-ttu-id="2c5d2-119">Du kan göra det genom att uppgradera toopremium lagring, endast hello-diskar som kräver bättre prestanda.</span><span class="sxs-lookup"><span data-stu-id="2c5d2-119">You can accomplish it by upgrading toopremium storage, only hello disks that require better performance.</span></span> <span data-ttu-id="2c5d2-120">I följande exempel hello, visar vi hur tooswitch en enda disk av en virtuell dator från standard toopremium lagring och vice versa.</span><span class="sxs-lookup"><span data-stu-id="2c5d2-120">In hello following example, we show how tooswitch a single disk of a VM from standard toopremium storage, and vice versa.</span></span> <span data-ttu-id="2c5d2-121">toouse premium hanterade diskar måste den virtuella datorn använda en [VM-storlek](sizes.md) som har stöd för premium-lagring.</span><span class="sxs-lookup"><span data-stu-id="2c5d2-121">toouse premium managed disks, your VM must use a [VM size](sizes.md) that supports premium storage.</span></span> <span data-ttu-id="2c5d2-122">Det här exemplet växlar även tooa storlek som har stöd för premium-lagring.</span><span class="sxs-lookup"><span data-stu-id="2c5d2-122">This example also switches tooa size that supports premium storage.</span></span>

```powershell

$diskName = 'yourDiskName'
# resource group that contains hello managed disk
$rgName = 'yourResourceGroupName'
# Choose between StandardLRS and PremiumLRS based on your scenario
$storageType = 'PremiumLRS'
# Premium capable size 
$size = 'Standard_DS2_v2'

$disk = Get-AzureRmDisk -DiskName $diskName -ResourceGroupName $rgName

# Get hello ARM resource tooget name and resource group of hello VM
$vmResource = Get-AzureRmResource -ResourceId $disk.OwnerId
$vm = Get-AzureRmVM $vmResource.ResourceGroupName -Name $vmResource.ResourceName 

# Stop and deallocate hello VM before changing hello storage type
Stop-AzureRmVM -ResourceGroupName $vm.ResourceGroupName -Name $vm.Name -Force

# Change hello VM size tooa size that supports premium storage
# Skip this step if converting storage from premium toostandard
$vm.HardwareProfile.VmSize = $size
Update-AzureRmVM -VM $vm -ResourceGroupName $rgName

# Update hello storage type
$diskUpdateConfig = New-AzureRmDiskUpdateConfig –AccountType $storageType
Update-AzureRmDisk -DiskUpdate $diskUpdateConfig -ResourceGroupName $rgName `
-DiskName $disk.Name

Start-AzureRmVM -ResourceGroupName $vm.ResourceGroupName -Name $vm.Name
```

## <a name="next-steps"></a><span data-ttu-id="2c5d2-123">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="2c5d2-123">Next steps</span></span>

<span data-ttu-id="2c5d2-124">Ta en skrivskyddad kopia av en virtuell dator med hjälp av [ögonblicksbilder](snapshot-copy-managed-disk.md).</span><span class="sxs-lookup"><span data-stu-id="2c5d2-124">Take a read-only copy of a VM by using [snapshots](snapshot-copy-managed-disk.md).</span></span>

