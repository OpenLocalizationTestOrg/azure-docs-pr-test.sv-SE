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
# <a name="convert-azure-managed-disks-storage-from-standard-toopremium-and-vice-versa"></a>Konvertera Azure hanterade diskar lagring från standard toopremium och vice versa

Hanterade diskar erbjuder två alternativ för lagring: [Premium](../../storage/storage-premium-storage.md) (SSD-baserad) och [Standard](../../storage/storage-standard-storage.md) (HDD-baserat). Det gör att du tooeasily växla mellan hello två alternativ med minimal avbrottstid baserat på dina prestandabehov. Den här funktionen är inte tillgänglig för ohanterade diskar. Men du kan enkelt [konvertera toomanaged diskar](convert-unmanaged-to-managed-disks.md) tooeasily växla mellan hello två alternativ.

Den här artikeln visar hur tooconvert hanterade diskar från standard toopremium och vice versa med hjälp av Azure PowerShell. Om du behöver tooinstall eller uppgradera den [installera och konfigurera Azure PowerShell](/powershell/azure/install-azurerm-ps.md).

## <a name="before-you-begin"></a>Innan du börjar

* konvertering av hello kräver en omstart av hello VM, så schemalägga hello migrering av diskar lagringen för en befintlig underhållsfönster. 
* Om du använder ohanterade diskar först [konvertera toomanaged diskar](convert-unmanaged-to-managed-disks.md) toouse den här artikeln tooswitch mellan hello två alternativ för lagring. 


## <a name="convert-all-hello-managed-disks-of-a-vm-from-standard-toopremium-and-vice-versa"></a>Konvertera alla hello hanterade diskar på en virtuell dator från standard toopremium och vice versa

I följande exempel hello, visar vi hur tooswitch alla hello diskar på en virtuell dator från standard toopremium lagring. toouse premium hanterade diskar måste den virtuella datorn använda en [VM-storlek](sizes.md) som har stöd för premium-lagring. Det här exemplet växlar även tooa storlek som har stöd för premium-lagring.

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
## <a name="convert-a-managed-disk-from-standard-toopremium-and-vice-versa"></a>Konverterar en hanterad disk från standard toopremium och vice versa

Du kanske vill dina kostnader toohave blandning av standard-och premium tooreduce för arbetsbelastningen för utveckling och testning. Du kan göra det genom att uppgradera toopremium lagring, endast hello-diskar som kräver bättre prestanda. I följande exempel hello, visar vi hur tooswitch en enda disk av en virtuell dator från standard toopremium lagring och vice versa. toouse premium hanterade diskar måste den virtuella datorn använda en [VM-storlek](sizes.md) som har stöd för premium-lagring. Det här exemplet växlar även tooa storlek som har stöd för premium-lagring.

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

## <a name="next-steps"></a>Nästa steg

Ta en skrivskyddad kopia av en virtuell dator med hjälp av [ögonblicksbilder](snapshot-copy-managed-disk.md).

