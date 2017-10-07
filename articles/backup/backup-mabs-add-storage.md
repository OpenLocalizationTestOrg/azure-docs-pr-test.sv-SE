---
title: aaaUse moderna Backup Storage med Azure Backup Server v2 | Microsoft Docs
description: "Läs mer om hello nya funktioner i Azure Backup Server v2. Den här artikeln beskriver hur tooupgrade Backup Server-installationen."
services: backup
documentationcenter: 
author: markgalioto
manager: carmonm
editor: 
ms.assetid: 
ms.service: backup
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/15/2017
ms.author: masaran;markgal
ms.openlocfilehash: b2a1ed27a6a682bd611fea1d2df9ef93314404e0
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="add-storage-tooazure-backup-server-v2"></a>Lägga till lagring tooAzure Backup Server v2

Azure Backup Server v2 levereras med System Center 2016 Data Protection Manager Modern Backup Storage. Moderna Backup Storage erbjuder lagringsbesparingar på 50 procent, säkerhetskopior som är tre gånger snabbare och effektivare lagring. Den erbjuder också arbetsbelastningen-medvetna lagring. 

> [!NOTE]
> toouse moderna Backup Storage, måste du köra Backup Server v2 på Windows Server 2016. Om du kör Backup Server v2 på en tidigare version av Windows Server Azure Backup Server kan inte utnyttja moderna Backup Storage. I stället skyddar arbetsbelastningar som med Backup Server v1. Mer information finns i hello säkerhetskopiering serverversionen [skydd matrisen](backup-mabs-protection-matrix.md).

## <a name="volumes-in-backup-server-v2"></a>Volymer i reservserver v2

Backup Server v2 accepterar lagringsvolymer. När du lägger till en volym formaterar Backup Server hello volym tooResilient filsystem (ReFS), vilket kräver att moderna Backup Storage. tooadd en volym och tooexpand senare om du behöver vi rekommenderar att du använder det här arbetsflödet:

1.  Konfigurera säkerhetskopiering v2 på en virtuell dator.
2.  Skapa en volym på en virtuell disk i en lagringspool:
    1.  Lägg till en disk tooa lagringspool och skapa en virtuell disk med enkel layout.
    2.  Lägga till ytterligare diskar och utöka hello virtuell disk.
    3.  Skapa volymer på hello virtuell disk.
3.  Lägg till hello volymer tooBackup Server.
4.  Konfigurera lagring av arbetsbelastning-medveten.

## <a name="create-a-volume-for-modern-backup-storage"></a>Skapa en volym för moderna Backup Storage

Med Backup Server v2 med volymer som disklagring kan hjälpa dig att behålla kontrollen över lagringen. En volym kan vara en enskild disk. Men om du vill tooextend lagring i hello framtida, skapa en volym från en disk som skapats med hjälp av lagringsutrymmen. Det kan hjälpa dig om du vill tooexpand hello volym för lagring av säkerhetskopior. Det här avsnittet innehåller metodtips för att skapa en volym med de här inställningarna.

1. I Serverhanteraren väljer **fil- och lagringstjänster** > **volymer** > **lagringspooler**. Under **fysiska DISKAR**väljer **ny Lagringspool**. 

    ![Skapa en ny lagringspool](./media/backup-mabs-add-storage/mabs-add-storage-1.png)

2. I hello **uppgifter** listrutan, Välj **ny virtuell Disk**.

    ![Lägg till en virtuell disk](./media/backup-mabs-add-storage/mabs-add-storage-2.png)

3. Välj hello lagringspoolen och välj sedan **Lägg till fysisk Disk**.

    ![Lägg till en fysisk disk](./media/backup-mabs-add-storage/mabs-add-storage-3.png)

4. Välj hello fysisk disk och välj sedan **Utöka virtuell Disk**.

    ![Utöka hello virtuell disk](./media/backup-mabs-add-storage/mabs-add-storage-4.png)

5. Välj hello virtuell disk och välj sedan **ny volym**.

    ![Skapa en ny volym](./media/backup-mabs-add-storage/mabs-add-storage-5.png)

6. I hello **Välj hello server och disk** dialogrutan, Välj hello server och hello ny disk. Markera **nästa**.

    ![Välj hello server och disk](./media/backup-mabs-add-storage/mabs-add-storage-6.png)

## <a name="add-volumes-toobackup-server-disk-storage"></a>Lägg till diskutrymme för volymer tooBackup Server

tooadd volym tooBackup-servern i hello **Management** rutan skanna hello lagring och välj sedan **Lägg till**. En lista över alla hello volymer tillgängliga toobe lagts till för Backup Server Storage visas. När tillgängliga volymerna har lagts till toohello listan med valda volymer, kan du ge dem ett eget namn toohelp hanteras. tooformat dessa volymer tooReFS så Backup Server kan använda hello fördelarna med moderna Backup Storage Välj **OK**.

![Lägg till tillgängliga volymer](./media/backup-mabs-add-storage/mabs-add-storage-7.png)

## <a name="set-up-workload-aware-storage"></a>Konfigurera arbetsbelastning-medvetna lagring

Du kan välja hello volymer som lagrar företrädesvis vissa typer av arbetsbelastningar med arbetsbelastning-medvetna lagring. Exempelvis kan du ange dyra volymer som stöder ett stort antal i/o-åtgärder per andra (IOPS) toostore endast hello arbetsbelastningar som kräver ofta, högvolyms-säkerhetskopieringar. Ett exempel är SQL Server med transaktionsloggar. Andra arbetsbelastningar som säkerhetskopieras mindre ofta som virtuella datorer, kan säkerhetskopieras toolow kostnaden volymer.

### <a name="update-dpmdiskstorage"></a>Uppdatera DPMDiskStorage

Du kan ställa in arbetsbelastningen-medvetna lagring med hello PowerShell-cmdlet Update-DPMDiskStorage som uppdaterar hello egenskaper för en volym i lagringspoolen för hello på en server med Data Protection Manager.

Syntax:

`Parameter Set: Volume`

```
Update-DPMDiskStorage [-Volume] <Volume> [[-FriendlyName] <String> ] [[-DatasourceType] <VolumeTag[]> ] [-Confirm] [-WhatIf] [ <CommonParameters>]
```
hello följande skärmbild visar hello uppdatering DPMDiskStorage cmdlet i hello PowerShell-fönster.

![hello uppdatering DPMDiskStorage kommandot i hello PowerShell-fönster](./media/backup-mabs-add-storage/mabs-add-storage-8.png)

hello ändringar du gör med hjälp av PowerShell återspeglas i hello Backup Server-administratörskonsolen.

![Diskar och volymer i hello-administratörskonsolen](./media/backup-mabs-add-storage/mabs-add-storage-9.png)

## <a name="next-steps"></a>Nästa steg
När du har installerat Backup Server lär du dig hur tooprepare servern, eller börja skydda en arbetsbelastning.

- [Förbereda säkerhetskopiering serverarbetsbelastningar](backup-azure-microsoft-azure-backup.md)
- [Använda Backup Server tooback in en VMware-server](backup-azure-backup-server-vmware.md)
- [Använda Backup Server tooback in SQL Server](backup-azure-sql-mabs.md)

