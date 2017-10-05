---
title: "Skapa en hanterad disk från en virtuell Hårddisk i Azure | Microsoft Docs"
description: "Skapa en hanterad disk från en virtuell Hårddisk som för närvarande i ett Azure storage-konto med hjälp av Resource Manager-distributionsmodellen."
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
ms.openlocfilehash: c03ebf73f1090b595149daf2eb3e274b05822f4f
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="create-managed-disks-from-unmanaged-disks-in-a-storage-account"></a>Skapa hanterade diskar från ohanterade diskar i ett lagringskonto

Hanterade diskar kan skapas från en befintlig datadisk eller en OS-disk som används för närvarande i ett Azure storage-konto. Du kan också skapa en tom disk som kan användas som en ny datadisk för en virtuell dator. 

## <a name="before-you-begin"></a>Innan du börjar
Om du använder PowerShell kan du kontrollera att du har den senaste versionen av AzureRM.Compute PowerShell-modulen. Kör följande kommando för att installera den.

```powershell
Install-Module AzureRM.Compute -RequiredVersion 2.6.0
```
Mer information finns i [Azure PowerShell versionshantering](/powershell/azure/overview).


## <a name="create-a-managed-disk-from-a-vhd-in-an-azure-storage-account"></a>Skapa en hanterad disk från en virtuell Hårddisk i ett Azure storage-konto

I exemplet med vi skapar en disk från en virtuell Hårddisk som hanterade diskar och kopplar den till parametern **disk 1 $** för senare användning. 

Den hantera disken kommer att skapas i den **västra USA** plats i en resursgrupp med namnet **myResourceGroup**. Disken får namnet **myDisk** och kommer att skapas från en VHD-fil med namnet **myDisk.vhd** vi tidigare har överförts till ett lagringskonto med namnet **mittlagringskonto**. Den hantera disken ska skapas i premium lokalt redundant lagring (LRS). StandardLRS och PremiumLRS är den enda **- AccountType** alternativ som är tillgängliga för hanterade diskar. 

1.  Ange vissa parametrar

    ```powershell
    $rgName = "myResourceGroup"
    $location = "West Central US"
    $diskName = "myDisk"
    $vhdUri = "https://mystorageaccount.blob.core.windows.net/vhds/myDisk.vhd"
    ```

2. Skapa datadisk för. 
    ```powershell
    $disk1 = New-AzureRmDisk -DiskName $diskName -Disk (New-AzureRmDiskConfig -AccountType PremiumLRS -Location $location -CreateOption Import -SourceUri $vhdUri) -ResourceGroupName $rgName
    ```
    
    

## <a name="create-an-empty-data-disk-as-a-managed-disk"></a>Skapa en tom datadisk som en hanterad disk

I exemplet med vi skapar en tom datadisk som hanterade diskar och kopplar den till parametern **$dataDisk2** för senare användning. En tom datadisk måste vara initierad logga in på den virtuella datorn och använder diskmgmt.msc eller [med WinRM och ett skript](attach-disk-ps.md#initialize-the-disk), när den är kopplad till en aktiv virtuell dator.

Tomma datadisken kommer att skapas i den **Väst centrala oss** plats i en resursgrupp med namnet **myResourceGroup**. Disken får namnet **myEmptyDataDisk**. Tom disk kommer att skapas i premium lokalt redundant lagring (LRS). StandardLRS och PremiumLRS är den enda **- AccountType** alternativ som är tillgängliga för hanterade diskar.

Diskens storlek i det här exemplet är 128GB, men du bör välja en storlek som uppfyller behoven för alla program som körs på den virtuella datorn.

1.  Ange vissa parametrar

    ```powershell
    $rgName = "myResourceGroup"
    $location = "West Central US"
    $dataDiskName = "myEmptyDataDisk"
    ```

2. Skapa datadisk för.
    ```powershell
    $dataDisk2 = New-AzureRmDisk -DiskName $dataDiskName -Disk (New-AzureRmDiskConfig -AccountType PremiumLRS -Location $location -CreateOption Empty -DiskSizeGB 128) -ResourceGroupName $rgName
    ```
    
## <a name="next-steps"></a>Nästa steg   
- Om du redan har en virtuell dator kan du [ansluta en datadisk](attach-disk-portal.md).
