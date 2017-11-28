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
# <a name="add-storage-tooazure-backup-server-v2"></a><span data-ttu-id="b0b40-104">Lägga till lagring tooAzure Backup Server v2</span><span class="sxs-lookup"><span data-stu-id="b0b40-104">Add storage tooAzure Backup Server v2</span></span>

<span data-ttu-id="b0b40-105">Azure Backup Server v2 levereras med System Center 2016 Data Protection Manager Modern Backup Storage.</span><span class="sxs-lookup"><span data-stu-id="b0b40-105">Azure Backup Server v2 comes with System Center 2016 Data Protection Manager Modern Backup Storage.</span></span> <span data-ttu-id="b0b40-106">Moderna Backup Storage erbjuder lagringsbesparingar på 50 procent, säkerhetskopior som är tre gånger snabbare och effektivare lagring.</span><span class="sxs-lookup"><span data-stu-id="b0b40-106">Modern Backup Storage offers storage savings of 50 percent, backups that are three times faster, and more efficient storage.</span></span> <span data-ttu-id="b0b40-107">Den erbjuder också arbetsbelastningen-medvetna lagring.</span><span class="sxs-lookup"><span data-stu-id="b0b40-107">It also offers workload-aware storage.</span></span> 

> [!NOTE]
> <span data-ttu-id="b0b40-108">toouse moderna Backup Storage, måste du köra Backup Server v2 på Windows Server 2016.</span><span class="sxs-lookup"><span data-stu-id="b0b40-108">toouse Modern Backup Storage, you must run Backup Server v2 on Windows Server 2016.</span></span> <span data-ttu-id="b0b40-109">Om du kör Backup Server v2 på en tidigare version av Windows Server Azure Backup Server kan inte utnyttja moderna Backup Storage.</span><span class="sxs-lookup"><span data-stu-id="b0b40-109">If you run Backup Server v2 on an earlier version of Windows Server, Azure Backup Server can't take advantage of Modern Backup Storage.</span></span> <span data-ttu-id="b0b40-110">I stället skyddar arbetsbelastningar som med Backup Server v1.</span><span class="sxs-lookup"><span data-stu-id="b0b40-110">Instead, it protects workloads as it does with Backup Server v1.</span></span> <span data-ttu-id="b0b40-111">Mer information finns i hello säkerhetskopiering serverversionen [skydd matrisen](backup-mabs-protection-matrix.md).</span><span class="sxs-lookup"><span data-stu-id="b0b40-111">For more information, see hello Backup Server version [protection matrix](backup-mabs-protection-matrix.md).</span></span>

## <a name="volumes-in-backup-server-v2"></a><span data-ttu-id="b0b40-112">Volymer i reservserver v2</span><span class="sxs-lookup"><span data-stu-id="b0b40-112">Volumes in Backup Server v2</span></span>

<span data-ttu-id="b0b40-113">Backup Server v2 accepterar lagringsvolymer.</span><span class="sxs-lookup"><span data-stu-id="b0b40-113">Backup Server v2 accepts storage volumes.</span></span> <span data-ttu-id="b0b40-114">När du lägger till en volym formaterar Backup Server hello volym tooResilient filsystem (ReFS), vilket kräver att moderna Backup Storage.</span><span class="sxs-lookup"><span data-stu-id="b0b40-114">When you add a volume, Backup Server formats hello volume tooResilient File System (ReFS), which Modern Backup Storage requires.</span></span> <span data-ttu-id="b0b40-115">tooadd en volym och tooexpand senare om du behöver vi rekommenderar att du använder det här arbetsflödet:</span><span class="sxs-lookup"><span data-stu-id="b0b40-115">tooadd a volume, and tooexpand it later if you need to, we suggest that you use this workflow:</span></span>

1.  <span data-ttu-id="b0b40-116">Konfigurera säkerhetskopiering v2 på en virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="b0b40-116">Set up Backup Server v2 on a VM.</span></span>
2.  <span data-ttu-id="b0b40-117">Skapa en volym på en virtuell disk i en lagringspool:</span><span class="sxs-lookup"><span data-stu-id="b0b40-117">Create a volume on a virtual disk in a storage pool:</span></span>
    1.  <span data-ttu-id="b0b40-118">Lägg till en disk tooa lagringspool och skapa en virtuell disk med enkel layout.</span><span class="sxs-lookup"><span data-stu-id="b0b40-118">Add a disk tooa storage pool and create a virtual disk with simple layout.</span></span>
    2.  <span data-ttu-id="b0b40-119">Lägga till ytterligare diskar och utöka hello virtuell disk.</span><span class="sxs-lookup"><span data-stu-id="b0b40-119">Add any additional disks, and extend hello virtual disk.</span></span>
    3.  <span data-ttu-id="b0b40-120">Skapa volymer på hello virtuell disk.</span><span class="sxs-lookup"><span data-stu-id="b0b40-120">Create volumes on hello virtual disk.</span></span>
3.  <span data-ttu-id="b0b40-121">Lägg till hello volymer tooBackup Server.</span><span class="sxs-lookup"><span data-stu-id="b0b40-121">Add hello volumes tooBackup Server.</span></span>
4.  <span data-ttu-id="b0b40-122">Konfigurera lagring av arbetsbelastning-medveten.</span><span class="sxs-lookup"><span data-stu-id="b0b40-122">Configure workload-aware storage.</span></span>

## <a name="create-a-volume-for-modern-backup-storage"></a><span data-ttu-id="b0b40-123">Skapa en volym för moderna Backup Storage</span><span class="sxs-lookup"><span data-stu-id="b0b40-123">Create a volume for Modern Backup Storage</span></span>

<span data-ttu-id="b0b40-124">Med Backup Server v2 med volymer som disklagring kan hjälpa dig att behålla kontrollen över lagringen.</span><span class="sxs-lookup"><span data-stu-id="b0b40-124">Using Backup Server v2 with volumes as disk storage can help you maintain control over storage.</span></span> <span data-ttu-id="b0b40-125">En volym kan vara en enskild disk.</span><span class="sxs-lookup"><span data-stu-id="b0b40-125">A volume can be a single disk.</span></span> <span data-ttu-id="b0b40-126">Men om du vill tooextend lagring i hello framtida, skapa en volym från en disk som skapats med hjälp av lagringsutrymmen.</span><span class="sxs-lookup"><span data-stu-id="b0b40-126">However, if you want tooextend storage in hello future, create a volume out of a disk created by using storage spaces.</span></span> <span data-ttu-id="b0b40-127">Det kan hjälpa dig om du vill tooexpand hello volym för lagring av säkerhetskopior.</span><span class="sxs-lookup"><span data-stu-id="b0b40-127">This can help if you want tooexpand hello volume for backup storage.</span></span> <span data-ttu-id="b0b40-128">Det här avsnittet innehåller metodtips för att skapa en volym med de här inställningarna.</span><span class="sxs-lookup"><span data-stu-id="b0b40-128">This section offers best practices for creating a volume with this setup.</span></span>

1. <span data-ttu-id="b0b40-129">I Serverhanteraren väljer **fil- och lagringstjänster** > **volymer** > **lagringspooler**.</span><span class="sxs-lookup"><span data-stu-id="b0b40-129">In Server Manager, select **File and Storage Services** > **Volumes** > **Storage Pools**.</span></span> <span data-ttu-id="b0b40-130">Under **fysiska DISKAR**väljer **ny Lagringspool**.</span><span class="sxs-lookup"><span data-stu-id="b0b40-130">Under **PHYSICAL DISKS**, select **New Storage Pool**.</span></span> 

    ![Skapa en ny lagringspool](./media/backup-mabs-add-storage/mabs-add-storage-1.png)

2. <span data-ttu-id="b0b40-132">I hello **uppgifter** listrutan, Välj **ny virtuell Disk**.</span><span class="sxs-lookup"><span data-stu-id="b0b40-132">In hello **TASKS** drop-down box, select **New Virtual Disk**.</span></span>

    ![Lägg till en virtuell disk](./media/backup-mabs-add-storage/mabs-add-storage-2.png)

3. <span data-ttu-id="b0b40-134">Välj hello lagringspoolen och välj sedan **Lägg till fysisk Disk**.</span><span class="sxs-lookup"><span data-stu-id="b0b40-134">Select hello storage pool, and then select **Add Physical Disk**.</span></span>

    ![Lägg till en fysisk disk](./media/backup-mabs-add-storage/mabs-add-storage-3.png)

4. <span data-ttu-id="b0b40-136">Välj hello fysisk disk och välj sedan **Utöka virtuell Disk**.</span><span class="sxs-lookup"><span data-stu-id="b0b40-136">Select hello physical disk, and then select **Extend Virtual Disk**.</span></span>

    ![Utöka hello virtuell disk](./media/backup-mabs-add-storage/mabs-add-storage-4.png)

5. <span data-ttu-id="b0b40-138">Välj hello virtuell disk och välj sedan **ny volym**.</span><span class="sxs-lookup"><span data-stu-id="b0b40-138">Select hello virtual disk, and then select **New Volume**.</span></span>

    ![Skapa en ny volym](./media/backup-mabs-add-storage/mabs-add-storage-5.png)

6. <span data-ttu-id="b0b40-140">I hello **Välj hello server och disk** dialogrutan, Välj hello server och hello ny disk.</span><span class="sxs-lookup"><span data-stu-id="b0b40-140">In hello **Select hello server and disk** dialog, select hello server and hello new disk.</span></span> <span data-ttu-id="b0b40-141">Markera **nästa**.</span><span class="sxs-lookup"><span data-stu-id="b0b40-141">Then, select **Next**.</span></span>

    ![Välj hello server och disk](./media/backup-mabs-add-storage/mabs-add-storage-6.png)

## <a name="add-volumes-toobackup-server-disk-storage"></a><span data-ttu-id="b0b40-143">Lägg till diskutrymme för volymer tooBackup Server</span><span class="sxs-lookup"><span data-stu-id="b0b40-143">Add volumes tooBackup Server disk storage</span></span>

<span data-ttu-id="b0b40-144">tooadd volym tooBackup-servern i hello **Management** rutan skanna hello lagring och välj sedan **Lägg till**.</span><span class="sxs-lookup"><span data-stu-id="b0b40-144">tooadd a volume tooBackup Server, in hello **Management** pane, rescan hello storage, and then select **Add**.</span></span> <span data-ttu-id="b0b40-145">En lista över alla hello volymer tillgängliga toobe lagts till för Backup Server Storage visas.</span><span class="sxs-lookup"><span data-stu-id="b0b40-145">A list of all hello volumes available toobe added for Backup Server Storage appears.</span></span> <span data-ttu-id="b0b40-146">När tillgängliga volymerna har lagts till toohello listan med valda volymer, kan du ge dem ett eget namn toohelp hanteras.</span><span class="sxs-lookup"><span data-stu-id="b0b40-146">After available volumes are added toohello list of selected volumes, you can give them a friendly name toohelp you manage them.</span></span> <span data-ttu-id="b0b40-147">tooformat dessa volymer tooReFS så Backup Server kan använda hello fördelarna med moderna Backup Storage Välj **OK**.</span><span class="sxs-lookup"><span data-stu-id="b0b40-147">tooformat these volumes tooReFS so Backup Server can use hello benefits of Modern Backup Storage, select **OK**.</span></span>

![Lägg till tillgängliga volymer](./media/backup-mabs-add-storage/mabs-add-storage-7.png)

## <a name="set-up-workload-aware-storage"></a><span data-ttu-id="b0b40-149">Konfigurera arbetsbelastning-medvetna lagring</span><span class="sxs-lookup"><span data-stu-id="b0b40-149">Set up workload-aware storage</span></span>

<span data-ttu-id="b0b40-150">Du kan välja hello volymer som lagrar företrädesvis vissa typer av arbetsbelastningar med arbetsbelastning-medvetna lagring.</span><span class="sxs-lookup"><span data-stu-id="b0b40-150">With workload-aware storage, you can select hello volumes that preferentially store certain kinds of workloads.</span></span> <span data-ttu-id="b0b40-151">Exempelvis kan du ange dyra volymer som stöder ett stort antal i/o-åtgärder per andra (IOPS) toostore endast hello arbetsbelastningar som kräver ofta, högvolyms-säkerhetskopieringar.</span><span class="sxs-lookup"><span data-stu-id="b0b40-151">For example, you can set expensive volumes that support a high number of input/output operations per second (IOPS) toostore only hello workloads that require frequent, high-volume backups.</span></span> <span data-ttu-id="b0b40-152">Ett exempel är SQL Server med transaktionsloggar.</span><span class="sxs-lookup"><span data-stu-id="b0b40-152">An example is SQL Server with transaction logs.</span></span> <span data-ttu-id="b0b40-153">Andra arbetsbelastningar som säkerhetskopieras mindre ofta som virtuella datorer, kan säkerhetskopieras toolow kostnaden volymer.</span><span class="sxs-lookup"><span data-stu-id="b0b40-153">Other workloads that are backed up less frequently, like VMs, can be backed up toolow-cost volumes.</span></span>

### <a name="update-dpmdiskstorage"></a><span data-ttu-id="b0b40-154">Uppdatera DPMDiskStorage</span><span class="sxs-lookup"><span data-stu-id="b0b40-154">Update-DPMDiskStorage</span></span>

<span data-ttu-id="b0b40-155">Du kan ställa in arbetsbelastningen-medvetna lagring med hello PowerShell-cmdlet Update-DPMDiskStorage som uppdaterar hello egenskaper för en volym i lagringspoolen för hello på en server med Data Protection Manager.</span><span class="sxs-lookup"><span data-stu-id="b0b40-155">You can set up workload-aware storage by using hello PowerShell cmdlet Update-DPMDiskStorage, which updates hello properties of a volume in hello storage pool on a Data Protection Manager server.</span></span>

<span data-ttu-id="b0b40-156">Syntax:</span><span class="sxs-lookup"><span data-stu-id="b0b40-156">Syntax:</span></span>

`Parameter Set: Volume`

```
Update-DPMDiskStorage [-Volume] <Volume> [[-FriendlyName] <String> ] [[-DatasourceType] <VolumeTag[]> ] [-Confirm] [-WhatIf] [ <CommonParameters>]
```
<span data-ttu-id="b0b40-157">hello följande skärmbild visar hello uppdatering DPMDiskStorage cmdlet i hello PowerShell-fönster.</span><span class="sxs-lookup"><span data-stu-id="b0b40-157">hello following screenshot shows hello Update-DPMDiskStorage cmdlet in hello PowerShell window.</span></span>

![hello uppdatering DPMDiskStorage kommandot i hello PowerShell-fönster](./media/backup-mabs-add-storage/mabs-add-storage-8.png)

<span data-ttu-id="b0b40-159">hello ändringar du gör med hjälp av PowerShell återspeglas i hello Backup Server-administratörskonsolen.</span><span class="sxs-lookup"><span data-stu-id="b0b40-159">hello changes you make by using PowerShell are reflected in hello Backup Server Administrator Console.</span></span>

![Diskar och volymer i hello-administratörskonsolen](./media/backup-mabs-add-storage/mabs-add-storage-9.png)

## <a name="next-steps"></a><span data-ttu-id="b0b40-161">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="b0b40-161">Next steps</span></span>
<span data-ttu-id="b0b40-162">När du har installerat Backup Server lär du dig hur tooprepare servern, eller börja skydda en arbetsbelastning.</span><span class="sxs-lookup"><span data-stu-id="b0b40-162">After you install Backup Server, learn how tooprepare your server, or begin protecting a workload.</span></span>

- [<span data-ttu-id="b0b40-163">Förbereda säkerhetskopiering serverarbetsbelastningar</span><span class="sxs-lookup"><span data-stu-id="b0b40-163">Prepare Backup Server workloads</span></span>](backup-azure-microsoft-azure-backup.md)
- [<span data-ttu-id="b0b40-164">Använda Backup Server tooback in en VMware-server</span><span class="sxs-lookup"><span data-stu-id="b0b40-164">Use Backup Server tooback up a VMware server</span></span>](backup-azure-backup-server-vmware.md)
- [<span data-ttu-id="b0b40-165">Använda Backup Server tooback in SQL Server</span><span class="sxs-lookup"><span data-stu-id="b0b40-165">Use Backup Server tooback up SQL Server</span></span>](backup-azure-sql-mabs.md)

