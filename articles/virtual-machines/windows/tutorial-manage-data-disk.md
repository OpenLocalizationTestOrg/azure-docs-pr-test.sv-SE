---
title: aaaManage Azure diskar med hello Azure PowerShell | Microsoft Docs
description: "Självstudiekurs – hantera Azure-diskarna med hello Azure PowerShell"
services: virtual-machines-windows
documentationcenter: virtual-machines
author: neilpeterson
manager: timlt
editor: tysonn
tags: azure-service-management
ms.assetid: 
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 05/02/2017
ms.author: nepeters
ms.custom: mvc
ms.openlocfilehash: 2f61ad18bc94bab527d7ae593da603c6073adc89
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="manage-azure-disks-with-powershell"></a>Hantera Azure-diskar med PowerShell

Azure virtual machines använder diskar toostore hello virtuella datorer, operativsystem, program och data. När du skapar en virtuell dator är det viktigt toochoose storleken för en disk och konfiguration lämpliga toohello förväntades arbetsbelastning. Den här kursen ingår distribuerar och hanterar Virtuella diskar. Du lär dig mer om:

> [!div class="checklist"]
> * OS-diskar och tillfällig diskar
> * Datadiskar
> * Standard- och Premium-diskar
> * Diskprestanda
> * Koppla och förbereda datadiskar

Den här kursen kräver hello Azure PowerShell module 3,6 eller senare. Kör ` Get-Module -ListAvailable AzureRM` toofind hello version. Om du behöver tooupgrade finns [installera Azure PowerShell-modulen](/powershell/azure/install-azurerm-ps).

## <a name="default-azure-disks"></a>Standard Azure-diskar

När en virtuell dator i Azure skapas är två diskar automatiskt bifogade toohello virtuella datorn. 

**Operativsystemdisken** - operativsystemet diskar kan storleksändras in too1 terabyte och värdar hello operativsystemet för virtuella datorer.  hello OS-disken har kopplats enhetsbeteckningen för *c:* som standard. hello disk cachelagring konfigurationen av hello OS-disken har optimerats för OS-prestanda. hello OS-disk **bör inte** värd för program eller data. Använd en datadisk, vilket beskrivs senare i den här artikeln för program och data.

**Diskutrymme** -tillfälliga diskar använder ett SSD-enhet som finns på hello samma Azure-värd som hello VM. Temporär diskar har hög performant och kan användas för åtgärder som till exempel temporär databearbetning. Om hello VM är flyttade tooa nya värden kan bort data som lagrats på en tillfällig disk. hello hello tillfälliga diskens storlek bestäms av hello VM-storlek. Tillfällig diskar tilldelas en enhetsbeteckning för *d:* som standard.

### <a name="temporary-disk-sizes"></a>Tillfällig diskstorlekar

| Typ | VM-storlek | Maxstorlek för temporär disk (GB) |
|----|----|----|
| [Generellt syfte](sizes-general.md) | A och D-serien | 800 |
| [Beräkningsoptimerad](sizes-compute.md) | F-serien | 800 |
| [Minnesoptimerad](../virtual-machines-windows-sizes-memory.md) | D och G serien | 6144 |
| [Lagringsoptimerad](../virtual-machines-windows-sizes-storage.md) | Serie L | 5630 |
| [GPU](sizes-gpu.md) | N serien | 1440 |
| [Hög prestanda](sizes-hpc.md) | A och H serien | 2000 |

## <a name="azure-data-disks"></a>Azure datadiskar

Ytterligare datadiskar kan läggas till för att installera program och lagra data. Datadiskar som ska användas i en situation där varaktiga och känslig datalagring önskas. Varje datadisk har en maximal kapacitet för 1 terabyte. hello storleken på hello virtuella datorn anger hur många datadiskar kan vara anslutna tooa VM. För varje VM-core kan två diskar kopplas. 

### <a name="max-data-disks-per-vm"></a>Maximalt antal datadiskar per VM

| Typ | VM-storlek | Maximalt antal datadiskar per VM |
|----|----|----|
| [Generellt syfte](sizes-general.md) | A och D-serien | 32 |
| [Beräkningsoptimerad](sizes-compute.md) | F-serien | 32 |
| [Minnesoptimerad](../virtual-machines-windows-sizes-memory.md) | D och G serien | 64 |
| [Lagringsoptimerad](../virtual-machines-windows-sizes-storage.md) | Serie L | 64 |
| [GPU](sizes-gpu.md) | N serien | 48 |
| [Hög prestanda](sizes-hpc.md) | A och H serien | 32 |

## <a name="vm-disk-types"></a>VM-disktyper

Azure tillhandahåller två typer av disk.

### <a name="standard-disk"></a>Standard disk

Standard Storage stöds av hårddiskar och levererar kostnadseffektiv lagring samtidigt som det är högpresterande. Standarddiskar är idealiskt för ett kostnadseffektivt dev och testa arbetsbelastning.

### <a name="premium-disk"></a>Premium-disk

Premiumdiskar backas upp av SSD-baserad hög prestanda, låg latens disk. Perfekt för virtuella datorer som kör produktion arbetsbelastning. Premium-lagring stöder DS-serien, DSv2-serien GS-serien och FS-serien virtuella datorer. Premiumdiskar finns i tre olika typer (P10 P20, P30), hello hello diskens storlek avgör hello disktyp. När du väljer, avrundat ett värde för disk hello toohello nästa typen. Till exempel om hello storlek är lägre än 128 GB hello disktyp blir P10, mellan 129 och 512 P20 och över 512 P30. 

### <a name="premium-disk-performance"></a>Premium-diskprestanda

|Premium-lagring disktyp | P10 | P20 | P30 |
| --- | --- | --- | --- |
| Diskens storlek (avrunda uppåt) | 128 GB | 512 GB | 1 024 GB (1 TB) |
| IOPS per disk | 500 | 2,300 | 5,000 |
Dataflöde per disk | 100 MB/s | 150 MB/s | 200 MB/s |

Medan hello ovanför tabell identifierar högsta IOPS per disk, kan en högre nivå av prestanda uppnås genom striping flera datadiskar. 64 data diskar kan vara kopplad till exempel tooStandard_GS5 VM. Om var och en av dessa diskar har storlek som en P30 kan högst 80 000 IOPS uppnås. Detaljerad information om högsta IOPS per VM finns [Linux VM-storlekar](./sizes.md).

## <a name="create-and-attach-disks"></a>Skapa och koppla diskar

toocomplete hello exempel i den här självstudiekursen, måste du ha en befintlig virtuell dator. Om det behövs, detta [skriptexempel](../scripts/virtual-machines-windows-powershell-sample-create-vm.md) kan skapa en åt dig. När gå igenom självstudiekursen hello ersätter namn hello resursgrupp och VM där det behövs.

Skapa hello startkonfiguration med [ny AzureRmDiskConfig](/powershell/module/azurerm.compute/new-azurermdiskconfig). hello följande exempel konfigurerar en disk som är 128 GB i storlek.

```powershell
$diskConfig = New-AzureRmDiskConfig -Location EastUS -CreateOption Empty -DiskSizeGB 128
```

Skapa hello-datadisk med hello [ny AzureRmDisk](/powershell/module/azurerm.compute/new-azurermdisk) kommando.

```powershell
$dataDisk = New-AzureRmDisk -ResourceGroupName myResourceGroup -DiskName myDataDisk -Disk $diskConfig
```

Get hello virtuell dator som du vill tooadd hello data disk toowith hello [Get-AzureRmVM](/powershell/module/azurerm.compute/get-azurermvm) kommando.

```powershell
$vm = Get-AzureRmVM -ResourceGroupName myResourceGroup -Name myVM
```

Lägg till hello data disk toohello konfiguration av virtuell dator med hello [Lägg till AzureRmVMDataDisk](/powershell/module/azurerm.compute/add-azurermvmdatadisk) kommando.

```powershell
$vm = Add-AzureRmVMDataDisk -VM $vm -Name myDataDisk -CreateOption Attach -ManagedDiskId $dataDisk.Id -Lun 1
```

Uppdatera hello virtuella datorn med hello [uppdatering AzureRmVM](/powershell/module/azurerm.compute/add-azurermvmdatadisk) kommando.

```powershell
Update-AzureRmVM -ResourceGroupName myResourceGroup -VM $vm
```

## <a name="prepare-data-disks"></a>Förbereda datadiskar

När en disk ansluten toohello virtuell dator, måste hello operativsystemet toobe konfigurerats toouse hello disk. hello följande exempel visar hur toomanually konfigurera hello första diskar som lagts toohello VM. Den här processen kan också automatiseras med hjälp av hello [tillägget för anpassat skript](./tutorial-automate-vm-deployment.md).

### <a name="manual-configuration"></a>Manuell konfiguration

Skapa en RDP-anslutning med hello virtuell dator. Öppna PowerShell och kör det här skriptet.

```powershell
Get-Disk | Where partitionstyle -eq 'raw' | `
Initialize-Disk -PartitionStyle MBR -PassThru | `
New-Partition -AssignDriveLetter -UseMaximumSize | `
Format-Volume -FileSystem NTFS -NewFileSystemLabel "myDataDisk" -Confirm:$false
```

## <a name="next-steps"></a>Nästa steg

I kursen får du lärt dig om Virtuella diskar ämnen som:

> [!div class="checklist"]
> * OS-diskar och tillfällig diskar
> * Datadiskar
> * Standard- och Premium-diskar
> * Diskprestanda
> * Koppla och förbereda datadiskar

Avancera toohello nästa självstudiekurs toolearn om hur du automatiserar VM-konfiguration.

> [!div class="nextstepaction"]
> [Automatisera konfiguration av virtuella datorer](./tutorial-automate-vm-deployment.md)
