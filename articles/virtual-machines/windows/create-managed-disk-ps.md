---
title: "aaaCreate hanterade diskar från en virtuell Hårddisk i Azure | Microsoft Docs"
description: "Skapa en hanterad disk från en VHD som för närvarande i ett Azure storage-konto med hello Resource Manager-modellen."
services: virtual-machines-windows
documentationcenter: 
author: cynthn
manager: timlt
editor: tysonn
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 02/05/2017
ms.author: cynthn
ms.openlocfilehash: 77adaac5419186ff85039fe2c4752f021aa5e448
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-managed-disks-from-unmanaged-disks-in-a-storage-account"></a>Skapa hanterade diskar från ohanterade diskar i ett lagringskonto

Hanterade diskar kan skapas från en befintlig datadisk eller en OS-disk som används för närvarande i ett Azure storage-konto. Du kan också skapa en tom disk som kan användas som en ny datadisk för en virtuell dator. 

## <a name="before-you-begin"></a>Innan du börjar
Om du använder PowerShell kan du se till att du har hello senaste versionen av hello AzureRM.Compute PowerShell-modulen. Kör följande kommando tooinstall hello den.

```powershell
Install-Module AzureRM.Compute -RequiredVersion 2.6.0
```
Mer information finns i [Azure PowerShell versionshantering](/powershell/azure/overview).


## <a name="create-a-managed-disk-from-a-vhd-in-an-azure-storage-account"></a>Skapa en hanterad disk från en virtuell Hårddisk i ett Azure storage-konto

I hello exempel vi skapar en disk från en virtuell Hårddisk som hanterade diskar och tilldelar den toohello parametern **disk 1 $** toouse senare. 

hello hanterade diskar kommer att skapas i hello **västra USA** plats i en resursgrupp med namnet **myResourceGroup**. hello disk namnges **myDisk** och kommer att skapas från en VHD-fil med namnet **myDisk.vhd** vi överfört tidigare tooa lagringskontonamnet **mittlagringskonto**. hello hanterade diskar kommer att skapas i premium lokalt redundant lagring (LRS). StandardLRS och PremiumLRS är endast hello **- AccountType** alternativ som är tillgängliga för hanterade diskar. 

1.  Ange vissa parametrar

    ```powershell
    $rgName = "myResourceGroup"
    $location = "West Central US"
    $diskName = "myDisk"
    $vhdUri = "https://mystorageaccount.blob.core.windows.net/vhds/myDisk.vhd"
    ```

2. Skapa hello datadisk. 
    ```powershell
    $disk1 = New-AzureRmDisk -DiskName $diskName -Disk (New-AzureRmDiskConfig -AccountType PremiumLRS -Location $location -CreateOption Import -SourceUri $vhdUri) -ResourceGroupName $rgName
    ```
    
    

## <a name="create-an-empty-data-disk-as-a-managed-disk"></a>Skapa en tom datadisk som en hanterad disk

I hello exempel vi skapa en tom datadisk som hanterade diskar och tilldela den toohello parametern **$dataDisk2** toouse senare. En tom datadisk måste toobe initiera loggning i toohello VM och använder diskmgmt.msc eller [med WinRM och ett skript](attach-disk-ps.md#initialize-the-disk), när den är ansluten tooa använder VM.

hello tom datadisk kommer att skapas i hello **Väst centrala oss** plats i en resursgrupp med namnet **myResourceGroup**. hello disk namnges **myEmptyDataDisk**. hello tom disk kommer att skapas i premium lokalt redundant lagring (LRS). StandardLRS och PremiumLRS är endast hello **- AccountType** alternativ som är tillgängliga för hanterade diskar.

hello diskens storlek i det här exemplet är 128GB, men du bör välja en storlek som uppfyller hello behoven för alla program som körs på den virtuella datorn.

1.  Ange vissa parametrar

    ```powershell
    $rgName = "myResourceGroup"
    $location = "West Central US"
    $dataDiskName = "myEmptyDataDisk"
    ```

2. Skapa hello datadisk.
    ```powershell
    $dataDisk2 = New-AzureRmDisk -DiskName $dataDiskName -Disk (New-AzureRmDiskConfig -AccountType PremiumLRS -Location $location -CreateOption Empty -DiskSizeGB 128) -ResourceGroupName $rgName
    ```
    
## <a name="next-steps"></a>Nästa steg   
- Om du redan har en virtuell dator kan du [ansluta en datadisk](attach-disk-portal.md).
