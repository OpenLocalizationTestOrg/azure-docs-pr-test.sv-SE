---
title: aaaInstall Azure Backup Server v2 | Microsoft Docs
description: "Azure Backup-Server v2 ger förbättrad säkerhetskopiering för att skydda virtuella datorer, filer och mappar, arbetsbelastningar och mer. Lär dig hur tooinstall eller uppgradera tooAzure Backup Server v2."
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
ms.openlocfilehash: 5b1699dadd3a173f1c0ef91a1a600bc5e12f20ad
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="install-azure-backup-server-v2"></a><span data-ttu-id="4e69a-104">Installera Azure Backup-Server v2</span><span class="sxs-lookup"><span data-stu-id="4e69a-104">Install Azure Backup Server v2</span></span>

<span data-ttu-id="4e69a-105">Azure Backup-Server skyddar dina virtuella datorer (VM), arbetsbelastningar, filer och mappar och mer.</span><span class="sxs-lookup"><span data-stu-id="4e69a-105">Azure Backup Server helps protect your virtual machines (VMs), workloads, files and folders, and more.</span></span> <span data-ttu-id="4e69a-106">Azure Backup Server v2 bygger på Azure Backup Server v1 och innehåller nya funktioner som inte är tillgängliga i v1.</span><span class="sxs-lookup"><span data-stu-id="4e69a-106">Azure Backup Server v2 builds on Azure Backup Server v1, and gives you new features that are not available in v1.</span></span> <span data-ttu-id="4e69a-107">En jämförelse av funktioner mellan v1 och v2 finns [Azure Backup Server protection matrisen](backup-mabs-protection-matrix.md).</span><span class="sxs-lookup"><span data-stu-id="4e69a-107">For a comparison of features between v1 and v2, see [Azure Backup Server protection matrix](backup-mabs-protection-matrix.md).</span></span> 

<span data-ttu-id="4e69a-108">hello ytterligare funktioner i Backup Server v2 är en uppgradering från Backup Server v1.</span><span class="sxs-lookup"><span data-stu-id="4e69a-108">hello additional features in Backup Server v2 are an upgrade from Backup Server v1.</span></span> <span data-ttu-id="4e69a-109">Backup Server v1 är dock inte ett krav för att installera Backup Server v2.</span><span class="sxs-lookup"><span data-stu-id="4e69a-109">However, Backup Server v1 is not a prerequisite for installing Backup Server v2.</span></span> <span data-ttu-id="4e69a-110">Om du vill tooupgrade från Backup Server v1 tooBackup v2 för Server, installera Backup Server v2 på hello Backup Server protection-server.</span><span class="sxs-lookup"><span data-stu-id="4e69a-110">If you want tooupgrade from Backup Server v1 tooBackup Server v2, install Backup Server v2 on hello Backup Server protection server.</span></span> <span data-ttu-id="4e69a-111">De befintliga inställningarna för säkerhetskopiering förblir intakta.</span><span class="sxs-lookup"><span data-stu-id="4e69a-111">Your existing Backup Server settings remain intact.</span></span>

<span data-ttu-id="4e69a-112">Du kan installera Backup Server v2 på Windows Server 2012 R2 eller Windows Server 2016.</span><span class="sxs-lookup"><span data-stu-id="4e69a-112">You can install Backup Server v2 on Windows Server 2012 R2 or Windows Server 2016.</span></span> <span data-ttu-id="4e69a-113">tootake nytta av nya funktioner som System Center 2016 Data Protection Manager Modern Backup Storage, måste du installera Backup Server v2 på Windows Server 2016.</span><span class="sxs-lookup"><span data-stu-id="4e69a-113">tootake advantage of new features like System Center 2016 Data Protection Manager Modern Backup Storage, you must install Backup Server v2 on Windows Server 2016.</span></span> <span data-ttu-id="4e69a-114">Innan du uppgraderar tooor installera Backup Server v2 Läs om hello [installationskrav](https://docs.microsoft.com/system-center/dpm/install-dpm#setup-prerequisites).</span><span class="sxs-lookup"><span data-stu-id="4e69a-114">Before you upgrade tooor install Backup Server v2, read about hello [installation prerequisites](https://docs.microsoft.com/system-center/dpm/install-dpm#setup-prerequisites).</span></span>

> [!NOTE]
> <span data-ttu-id="4e69a-115">Azure Backup-Server har hello samma kodbas som System Center Data Protection Manager.</span><span class="sxs-lookup"><span data-stu-id="4e69a-115">Azure Backup Server has hello same code base as System Center Data Protection Manager.</span></span> <span data-ttu-id="4e69a-116">Backup Server v1 är likvärdiga tooData Protection Manager 2012 R2 och Backup Server v2 är likvärdiga tooData Protection Manager 2016.</span><span class="sxs-lookup"><span data-stu-id="4e69a-116">Backup Server v1 is equivalent tooData Protection Manager 2012 R2, and Backup Server v2 is equivalent tooData Protection Manager 2016.</span></span> <span data-ttu-id="4e69a-117">Ibland refererar hello Data Protection Manager-dokumentationen till den här artikeln.</span><span class="sxs-lookup"><span data-stu-id="4e69a-117">This article occasionally references hello Data Protection Manager documentation.</span></span>
>
>

## <a name="upgrade-backup-server-toov2"></a><span data-ttu-id="4e69a-118">Uppgradera reservserver toov2</span><span class="sxs-lookup"><span data-stu-id="4e69a-118">Upgrade Backup Server toov2</span></span>
<span data-ttu-id="4e69a-119">tooupgrade från Backup Server v1 tooBackup Server v2, kontrollera att installationen har hello krävs uppdateringar:</span><span class="sxs-lookup"><span data-stu-id="4e69a-119">tooupgrade from Backup Server v1 tooBackup Server v2, make sure your installation has hello required updates:</span></span>

- <span data-ttu-id="4e69a-120">[Uppdatera skyddsagenter hello](backup-mabs-upgrade-to-v2.md#update-the-dpm-protection-agent) på hello skyddade servrar.</span><span class="sxs-lookup"><span data-stu-id="4e69a-120">[Update hello protection agents](backup-mabs-upgrade-to-v2.md#update-the-dpm-protection-agent) on hello protected servers.</span></span>
- <span data-ttu-id="4e69a-121">Uppgradera Windows Server 2012 R2 tooWindows Server 2016.</span><span class="sxs-lookup"><span data-stu-id="4e69a-121">Upgrade Windows Server 2012 R2 tooWindows Server 2016.</span></span>
- <span data-ttu-id="4e69a-122">Uppgradera Azure Backup Server fjärradministratör på alla produktionsservrar.</span><span class="sxs-lookup"><span data-stu-id="4e69a-122">Upgrade Azure Backup Server Remote Administrator on all production servers.</span></span>
- <span data-ttu-id="4e69a-123">Se till att säkerhetskopieringen angetts toocontinue utan att starta om produktionsservern för.</span><span class="sxs-lookup"><span data-stu-id="4e69a-123">Ensure that backups are set toocontinue without restarting your production server.</span></span>


### <a name="upgrade-steps-for-backup-server-v2"></a><span data-ttu-id="4e69a-124">Uppgradering för säkerhetskopiering v2</span><span class="sxs-lookup"><span data-stu-id="4e69a-124">Upgrade steps for Backup Server v2</span></span>

1. <span data-ttu-id="4e69a-125">I hello Download Center [hämta hello uppgradera installer](https://go.microsoft.com/fwlink/?LinkId=626082).</span><span class="sxs-lookup"><span data-stu-id="4e69a-125">In hello Download Center, [download hello upgrade installer](https://go.microsoft.com/fwlink/?LinkId=626082).</span></span>

2. <span data-ttu-id="4e69a-126">När du har extraherat hello installationsguiden Se **köra setup.exe** är markerad och välj sedan **Slutför**.</span><span class="sxs-lookup"><span data-stu-id="4e69a-126">After you extract hello setup wizard, make sure that **Execute setup.exe** is selected, and then select **Finish**.</span></span>

  ![Installationsprogram - konfigurationen](./media/backup-mabs-upgrade-to-v2/run-setup.png)

3. <span data-ttu-id="4e69a-128">Hello i guiden för Microsoft Azure Backup Server under **installera**väljer **Microsoft Azure Backup Server**.</span><span class="sxs-lookup"><span data-stu-id="4e69a-128">In hello Microsoft Azure Backup Server wizard, under **Install**, select **Microsoft Azure Backup Server**.</span></span>

  ![Installationsprogram - väljer installera](./media/backup-mabs-upgrade-to-v2/mabs-installer-s1.png)

4. <span data-ttu-id="4e69a-130">På hello **Välkommen** granskar hello varningar, och välj sedan **nästa**.</span><span class="sxs-lookup"><span data-stu-id="4e69a-130">On hello **Welcome** page, review hello warnings, and then select **Next**.</span></span>

  ![Installationsprogram – välkomstsida](./media/backup-mabs-upgrade-to-v2/mabs-installer-s2.png)

5. <span data-ttu-id="4e69a-132">Installationsguiden för hello utför kravkontroller toomake att uppgradera din miljö.</span><span class="sxs-lookup"><span data-stu-id="4e69a-132">hello setup wizard performs prerequisite checks toomake sure your environment can upgrade.</span></span> <span data-ttu-id="4e69a-133">På hello **nödvändiga kontrollerar** väljer **Kontrollera**.</span><span class="sxs-lookup"><span data-stu-id="4e69a-133">On hello **Prerequisite Checks** page, select **Check**.</span></span>

  ![Installationsprogram - nödvändiga kontrollerar sida](./media/backup-mabs-upgrade-to-v2/mabs-installer-s3-perform-checks.png)

6. <span data-ttu-id="4e69a-135">Din miljö måste klara hello nödvändiga kontroller.</span><span class="sxs-lookup"><span data-stu-id="4e69a-135">Your environment must pass hello prerequisite checks.</span></span> <span data-ttu-id="4e69a-136">Om din miljö inte klarar hello kontroller, notera hello problem och åtgärda dem.</span><span class="sxs-lookup"><span data-stu-id="4e69a-136">If your environment doesn't pass hello checks, note hello issues and fix them.</span></span> <span data-ttu-id="4e69a-137">Markera **kontrollen igen**.</span><span class="sxs-lookup"><span data-stu-id="4e69a-137">Then, select **Check Again**.</span></span> <span data-ttu-id="4e69a-138">När du skickar hello nödvändiga kontroller, Välj **nästa**.</span><span class="sxs-lookup"><span data-stu-id="4e69a-138">After you pass hello prerequisite checks, select **Next**.</span></span>

  ![Installationsprogram - knappen Kontrollera igen](./media/backup-mabs-upgrade-to-v2/mabs-installer-s4-pass-checks.png)

7. <span data-ttu-id="4e69a-140">På hello **SQL-inställningar** väljer hello önskat alternativ för SQL-installationen, och välj sedan **kontrollera och installera**.</span><span class="sxs-lookup"><span data-stu-id="4e69a-140">On hello **SQL Settings** page, select hello relevant option for your SQL installation, and then select **Check and Install**.</span></span>

  ![Installationsprogram - sidan SQL-inställningar](./media/backup-mabs-upgrade-to-v2/mabs-installer-s5-sql-settings.png)

  <span data-ttu-id="4e69a-142">hello kontroller kan ta några minuter.</span><span class="sxs-lookup"><span data-stu-id="4e69a-142">hello checks might take a few minutes.</span></span> <span data-ttu-id="4e69a-143">När hello kontroller är klar, Välj **nästa**.</span><span class="sxs-lookup"><span data-stu-id="4e69a-143">When hello checks are finished, select **Next**.</span></span>

  ![Installationsprogram – kontrollera om SQL-inställningar och installera](./media/backup-mabs-upgrade-to-v2/mabs-installer-s5a-check-and fix-settings.png)

8. <span data-ttu-id="4e69a-145">På hello **installationsinställningar** gör eventuella ändringar toohello plats där Backup Server är installerat eller toohello Scratch plats.</span><span class="sxs-lookup"><span data-stu-id="4e69a-145">On hello **Installation Settings** page, make any changes toohello location where Backup Server is installed, or toohello Scratch Location.</span></span> <span data-ttu-id="4e69a-146">Välj **nästa**.</span><span class="sxs-lookup"><span data-stu-id="4e69a-146">Select **Next**.</span></span>

  ![Installationsprogram - installationsinställningssidan](./media/backup-mabs-upgrade-to-v2/mabs-installer-s6-installation-settings.png)

9. <span data-ttu-id="4e69a-148">toofinish hello installationsguiden väljer **Slutför**.</span><span class="sxs-lookup"><span data-stu-id="4e69a-148">toofinish hello setup wizard, select **Finish**.</span></span>

  ![Installationsprogram - Slutför](./media/backup-mabs-upgrade-to-v2/run-setup.png)



## <a name="add-storage-for-modern-backup-storage"></a><span data-ttu-id="4e69a-150">Lägga till lagringsenheter till moderna Backup Storage</span><span class="sxs-lookup"><span data-stu-id="4e69a-150">Add storage for Modern Backup Storage</span></span>

<span data-ttu-id="4e69a-151">tooimprove säkerhetskopieringslagring effektivitet, reservserver v2 lägger till stöd för volymer.</span><span class="sxs-lookup"><span data-stu-id="4e69a-151">tooimprove backup storage efficiency, Backup Server v2 adds support for volumes.</span></span> <span data-ttu-id="4e69a-152">Som reservserver v1, v2 Backup Server har stöd för diskar.</span><span class="sxs-lookup"><span data-stu-id="4e69a-152">Like Backup Server v1, Backup Server v2 supports disks.</span></span>

### <a name="add-volumes-and-disks"></a><span data-ttu-id="4e69a-153">Lägg till volymer och diskar</span><span class="sxs-lookup"><span data-stu-id="4e69a-153">Add volumes and disks</span></span>
<span data-ttu-id="4e69a-154">Om du kör reservserver v2 på Windows Server 2016 kan du använda volymer toostore säkerhetskopierade data.</span><span class="sxs-lookup"><span data-stu-id="4e69a-154">If you run Backup Server v2 on Windows Server 2016, you can use volumes toostore backup data.</span></span> <span data-ttu-id="4e69a-155">Volymer ger lagringsbesparingar och snabbare säkerhetskopieringar.</span><span class="sxs-lookup"><span data-stu-id="4e69a-155">Volumes offer storage savings and faster backups.</span></span> <span data-ttu-id="4e69a-156">Eftersom volymer är ny tooBackup Server, måste du lägga till dem.</span><span class="sxs-lookup"><span data-stu-id="4e69a-156">Because volumes are new tooBackup Server, you must add them.</span></span> 

<span data-ttu-id="4e69a-157">När du lägger till en volym tooBackup Server kan du ge hello volym ett eget namn.</span><span class="sxs-lookup"><span data-stu-id="4e69a-157">When you add a volume tooBackup Server, you can give hello volume a friendly name.</span></span> <span data-ttu-id="4e69a-158">Klicka på hello **eget namn** kolumnen hello volym som du vill tooname.</span><span class="sxs-lookup"><span data-stu-id="4e69a-158">Click hello **Friendly Name** column of hello volume you want tooname.</span></span> <span data-ttu-id="4e69a-159">Du kan ändra namnet hello senare, om det behövs.</span><span class="sxs-lookup"><span data-stu-id="4e69a-159">You can change hello name later, if necessary.</span></span> <span data-ttu-id="4e69a-160">Du kan också använda PowerShell tooadd eller ändra eget namn för volymer.</span><span class="sxs-lookup"><span data-stu-id="4e69a-160">You also can use PowerShell tooadd or change friendly names for volumes.</span></span>

<span data-ttu-id="4e69a-161">tooadd en volym i hello-administratörskonsolen:</span><span class="sxs-lookup"><span data-stu-id="4e69a-161">tooadd a volume in hello Administrator Console:</span></span>

1. <span data-ttu-id="4e69a-162">Välj i hello Azure Backup Server-administratörskonsolen, **Management** > **disklagring** > **Lägg till**.</span><span class="sxs-lookup"><span data-stu-id="4e69a-162">In hello Azure Backup Server Administrator Console, select **Management** > **Disk Storage** > **Add**.</span></span>

    ![Öppna hello lägger till disklagring guiden](./media//backup-mabs-upgrade-to-v2/open-add-disk-storage-wizard.png)

    <span data-ttu-id="4e69a-164">Hello lägger till disklagring guiden öppnas.</span><span class="sxs-lookup"><span data-stu-id="4e69a-164">This opens hello Add Disk Storage wizard.</span></span>

2. <span data-ttu-id="4e69a-165">På hello **lägger till disklagring** i hello sidan **tillgängliga volymer** rutan, Välj en volym och välj sedan **Lägg till**.</span><span class="sxs-lookup"><span data-stu-id="4e69a-165">On hello **Add Disk Storage** page, in hello **Available volumes** box, select a volume, and then select **Add**.</span></span>
3. <span data-ttu-id="4e69a-166">I hello **valda volymer** , ange ett eget namn för hello volymen och välj sedan **OK**.</span><span class="sxs-lookup"><span data-stu-id="4e69a-166">In hello **Selected volumes** box, enter a friendly name for hello volume, and then select **OK**.</span></span>

      ![Disklagring guiden Lägg till – Lägg till volym](./media/backup-mabs-upgrade-to-v2/add-volume.png)

  <span data-ttu-id="4e69a-168">Om du vill tooadd en disk, tillhör hello disken tooa skyddsgrupp som har äldre lagring.</span><span class="sxs-lookup"><span data-stu-id="4e69a-168">If you want tooadd a disk, hello disk must belong tooa protection group that has legacy storage.</span></span> <span data-ttu-id="4e69a-169">Diskarna kan endast användas för dessa skyddsgrupper.</span><span class="sxs-lookup"><span data-stu-id="4e69a-169">These disks can only be used for these protection groups.</span></span> <span data-ttu-id="4e69a-170">Om servern för säkerhetskopiering inte har datakällor som har äldre skydd, finns hello disk inte.</span><span class="sxs-lookup"><span data-stu-id="4e69a-170">If Backup Server doesn't have sources that have legacy protection, hello disk isn't listed.</span></span>

  <span data-ttu-id="4e69a-171">Läs mer om att lägga till diskar i [lägga till diskar tooincrease äldre lagring](http://docs.microsoft.com/system-center/dpm/upgrade-to-dpm-2016#adding-disks-to-increase-legacy-storage).</span><span class="sxs-lookup"><span data-stu-id="4e69a-171">For more information about adding disks, see [Adding disks tooincrease legacy storage](http://docs.microsoft.com/system-center/dpm/upgrade-to-dpm-2016#adding-disks-to-increase-legacy-storage).</span></span> <span data-ttu-id="4e69a-172">Du kan inte ge en disk ett eget namn.</span><span class="sxs-lookup"><span data-stu-id="4e69a-172">You can't give a disk a friendly name.</span></span>


### <a name="assign-workloads-toovolumes"></a><span data-ttu-id="4e69a-173">Tilldela arbetsbelastningar toovolumes</span><span class="sxs-lookup"><span data-stu-id="4e69a-173">Assign workloads toovolumes</span></span>

<span data-ttu-id="4e69a-174">I Backup Server anger du vilka arbetsbelastningar tilldelas toowhich volymer.</span><span class="sxs-lookup"><span data-stu-id="4e69a-174">In Backup Server, you specify which workloads are assigned toowhich volumes.</span></span> <span data-ttu-id="4e69a-175">Exempelvis kan du ange dyra volymer som stöder ett stort antal i/o-åtgärder per andra (IOPS) toostore endast arbetsbelastningar som kräver ofta, högvolyms-säkerhetskopieringar.</span><span class="sxs-lookup"><span data-stu-id="4e69a-175">For example, you can set expensive volumes that support a high number of input/output operations per second (IOPS) toostore only workloads that require frequent, high-volume backups.</span></span> <span data-ttu-id="4e69a-176">Ett exempel är SQL Server med transaktionsloggar.</span><span class="sxs-lookup"><span data-stu-id="4e69a-176">An example is SQL Server with transaction logs.</span></span>

#### <a name="update-dpmdiskstorage"></a><span data-ttu-id="4e69a-177">Uppdatera DPMDiskStorage</span><span class="sxs-lookup"><span data-stu-id="4e69a-177">Update-DPMDiskStorage</span></span>

<span data-ttu-id="4e69a-178">tooupdate hello egenskaper för en volym i lagringspoolen för hello i Backup Server, Använd hello PowerShell-cmdlet Update DPMDiskStorage.</span><span class="sxs-lookup"><span data-stu-id="4e69a-178">tooupdate hello properties of a volume in hello storage pool in Backup Server, use hello PowerShell cmdlet Update-DPMDiskStorage.</span></span>

<span data-ttu-id="4e69a-179">Syntax:</span><span class="sxs-lookup"><span data-stu-id="4e69a-179">Syntax:</span></span>

`Parameter Set: Volume`

```
Update-DPMDiskStorage [-Volume] <Volume> [[-FriendlyName] <String> ] [[-DatasourceType] <VolumeTag[]> ] [-Confirm] [-WhatIf] [ <CommonParameters>]
```

<span data-ttu-id="4e69a-180">Alla ändringar du gör med hjälp av PowerShell återspeglas i hello Användargränssnittet.</span><span class="sxs-lookup"><span data-stu-id="4e69a-180">All changes that you make by using PowerShell are reflected in hello UI.</span></span>


## <a name="protect-data-sources"></a><span data-ttu-id="4e69a-181">Skydda datakällor</span><span class="sxs-lookup"><span data-stu-id="4e69a-181">Protect data sources</span></span>
<span data-ttu-id="4e69a-182">toobegin skyddar datakällor, skapar en skyddsgrupp.</span><span class="sxs-lookup"><span data-stu-id="4e69a-182">toobegin protecting data sources, create a protection group.</span></span> <span data-ttu-id="4e69a-183">hello följande steg Markera ändringar eller tillägg toohello guiden Ny Skyddsgrupp.</span><span class="sxs-lookup"><span data-stu-id="4e69a-183">hello following steps highlight changes or additions toohello New Protection Group wizard.</span></span>

<span data-ttu-id="4e69a-184">toocreate en skyddsgrupp:</span><span class="sxs-lookup"><span data-stu-id="4e69a-184">toocreate a protection group:</span></span>

1. <span data-ttu-id="4e69a-185">I hello Backup Server-administratörskonsolen, Välj **skydd**.</span><span class="sxs-lookup"><span data-stu-id="4e69a-185">In hello Backup Server Administrator Console, select **Protection**.</span></span>

2. <span data-ttu-id="4e69a-186">Markera på verktygsfliken hello **ny**.</span><span class="sxs-lookup"><span data-stu-id="4e69a-186">On hello tool ribbon, select **New**.</span></span>

    <span data-ttu-id="4e69a-187">Hello Skapa ny Skyddsgrupp guiden öppnas.</span><span class="sxs-lookup"><span data-stu-id="4e69a-187">This opens hello Create New Protection Group wizard.</span></span>

  ![Guiden Skapa ny Skyddsgrupp](./media/backup-mabs-upgrade-to-v2/create-a-protection-group-1.png)

3. <span data-ttu-id="4e69a-189">På hello **Välkommen** väljer **nästa**.</span><span class="sxs-lookup"><span data-stu-id="4e69a-189">On hello **Welcome** page, select **Next**.</span></span>
4. <span data-ttu-id="4e69a-190">På hello **Välj typ av Skyddsgrupp** väljer hello typ av skyddsgrupp du vill toocreate och välj sedan **nästa**.</span><span class="sxs-lookup"><span data-stu-id="4e69a-190">On hello **Select Protection Group Type** page, select hello type of protection group you want toocreate, and then select **Next**.</span></span>

  ![Sidan Välj Skyddsgruppen](./media/backup-mabs-upgrade-to-v2/create-a-protection-group-2.png)

5. <span data-ttu-id="4e69a-192">På hello **Välj gruppmedlemmar** i hello sidan **tillgängliga medlemmar** rutan, hello medlemmar med protection Agent finns.</span><span class="sxs-lookup"><span data-stu-id="4e69a-192">On hello **Select Group Members** page, in hello **Available members** pane, hello members with protection agents are listed.</span></span> <span data-ttu-id="4e69a-193">I det här exemplet väljer du volym D:\ och E:\ och Lägg till dem toohello **markerade medlemmar** fönstret.</span><span class="sxs-lookup"><span data-stu-id="4e69a-193">For this example, select volume D:\ and E:\ and add them toohello **Selected members** pane.</span></span> <span data-ttu-id="4e69a-194">Välj **nästa**.</span><span class="sxs-lookup"><span data-stu-id="4e69a-194">Select **Next**.</span></span>

  ![Välj grupp för medlemmar sida](./media/backup-mabs-upgrade-to-v2/create-a-protection-group-3.png)

6. <span data-ttu-id="4e69a-196">På hello **Välj Dataskyddsmetod** anger en **skyddsgruppnamn**, Välj hello skyddsmetod och välj sedan **nästa**.</span><span class="sxs-lookup"><span data-stu-id="4e69a-196">On hello **Select Data Protection Method** page, enter a **Protection group name**, select hello protection method, and then select **Next**.</span></span> <span data-ttu-id="4e69a-197">Om du vill ha kortvarigt skydd, måste du välja hello **Disk** metoden att säkerhetskopiera.</span><span class="sxs-lookup"><span data-stu-id="4e69a-197">If you want short-term protection, you must select hello **Disk** backup method.</span></span>

  ![Välj Dataskyddsmetod sida](./media/backup-mabs-upgrade-to-v2/create-a-protection-group-4.png)

7. <span data-ttu-id="4e69a-199">På hello **ange kortvariga mål** sidan Välj hello information för **Kvarhållningsintervall** och **Synkroniseringsfrekvens**.</span><span class="sxs-lookup"><span data-stu-id="4e69a-199">On hello **Specify Short-Term Goals** page, select hello details for **Retention range** and **Synchronization frequency**.</span></span> <span data-ttu-id="4e69a-200">Markera **nästa**.</span><span class="sxs-lookup"><span data-stu-id="4e69a-200">Then, select **Next**.</span></span> <span data-ttu-id="4e69a-201">Du kan också toochange hello schema för när återställningspunkter kan vidtas, Välj **ändra**.</span><span class="sxs-lookup"><span data-stu-id="4e69a-201">Optionally, toochange hello schedule for when recovery points are taken, select **Modify**.</span></span>

  ![Ange kortvariga mål-sida](./media/backup-mabs-upgrade-to-v2/create-a-protection-group-5.png)

8. <span data-ttu-id="4e69a-203">På hello **granska Disklagringstilldelning** granskar information om hello datakällor som du har valt, storlek och värden för hello utrymme toobe etablerad och hello lagring målvolymen.</span><span class="sxs-lookup"><span data-stu-id="4e69a-203">On hello **Review Disk Storage Allocation** page, review details about hello data sources you selected, their size, and  values for hello space toobe provisioned and hello target storage volume.</span></span>

  ![Granska Disklagringstilldelning sidan](./media/backup-mabs-upgrade-to-v2/create-a-protection-group-6.png)

  <span data-ttu-id="4e69a-205">Lagringsvolymer baseras på hello arbetsbelastning volym allokering (anges med hjälp av PowerShell) och hello tillgängligt lagringsutrymme.</span><span class="sxs-lookup"><span data-stu-id="4e69a-205">Storage volumes are based on hello workload volume allocation (set by using PowerShell) and hello available storage.</span></span> <span data-ttu-id="4e69a-206">Du kan ändra hello lagringsvolymer genom att välja andra volymer i hello nedrullningsbara menyn.</span><span class="sxs-lookup"><span data-stu-id="4e69a-206">You can change hello storage volumes by selecting other volumes in hello drop-down menu.</span></span> <span data-ttu-id="4e69a-207">Om du ändrar hello värdet för **Mållagringsenhet**, hello värde för **ledigt lagringsutrymme** ändras dynamiskt tooreflect värden under **ledigt utrymme** och **Underprovisioned utrymme**.</span><span class="sxs-lookup"><span data-stu-id="4e69a-207">If you change hello value for **Target Storage**, hello value for **Available disk storage** dynamically changes tooreflect values under **Free Space** and **Underprovisioned Space**.</span></span>

  <span data-ttu-id="4e69a-208">Om hello datakällor växer som planerat, hello värde för hello **Underprovisioned utrymme** kolumn i **ledigt lagringsutrymme** visar hello mängden ytterligare lagringsutrymme som behövs.</span><span class="sxs-lookup"><span data-stu-id="4e69a-208">If hello data sources grow as planned, hello value for hello **Underprovisioned Space** column in **Available disk storage** reflects hello amount of additional storage that's needed.</span></span> <span data-ttu-id="4e69a-209">Använd värdet toohelp planen lagringsbehoven för smooth säkerhetskopieringar.</span><span class="sxs-lookup"><span data-stu-id="4e69a-209">Use this value toohelp plan your storage needs for smooth backups.</span></span> <span data-ttu-id="4e69a-210">Om hello värdet noll, finns det inga problem med lagring i hello förutsägbar framtid.</span><span class="sxs-lookup"><span data-stu-id="4e69a-210">If hello value is zero, there are no potential problems with storage in hello foreseeable future.</span></span> <span data-ttu-id="4e69a-211">Om hello värde är ett tal som inte är noll, du har inte tillräckligt lagringsutrymme som allokerats (baserat på din skydd principen och hello datastorleken för dina skyddade medlemmar).</span><span class="sxs-lookup"><span data-stu-id="4e69a-211">If hello value is a number other than zero, you do not have sufficient storage allocated  (based on your protection policy and hello data size of your protected members).</span></span>

  ![Underbelagd disklagring](./media/backup-mabs-upgrade-to-v2/create-a-protection-group-7.png)

   <span data-ttu-id="4e69a-213">Skapa din grupp, fullständig hello skyddsguiden toofinish.</span><span class="sxs-lookup"><span data-stu-id="4e69a-213">toofinish creating your protection group, complete hello wizard.</span></span>

## <a name="migrate-legacy-storage-toomodern-backup-storage"></a><span data-ttu-id="4e69a-214">Migrera äldre lagring tooModern Backup Storage</span><span class="sxs-lookup"><span data-stu-id="4e69a-214">Migrate legacy storage tooModern Backup Storage</span></span>
<span data-ttu-id="4e69a-215">När du har uppgraderat tooor installera Backup Server v2 och uppgradera hello operativsystemet tooWindows Server 2016 uppdatera ditt skydd grupper toouse moderna Backup Storage.</span><span class="sxs-lookup"><span data-stu-id="4e69a-215">After you upgrade tooor install Backup Server v2 and upgrade hello operating system tooWindows Server 2016, update your protection groups toouse Modern Backup Storage.</span></span> <span data-ttu-id="4e69a-216">Som standard ändras inte skyddsgrupper.</span><span class="sxs-lookup"><span data-stu-id="4e69a-216">By default, protection groups are not changed.</span></span> <span data-ttu-id="4e69a-217">De fortsätta toofunction som de ursprungligen har ställts in.</span><span class="sxs-lookup"><span data-stu-id="4e69a-217">They continue toofunction as they were initially set up.</span></span> 

<span data-ttu-id="4e69a-218">Uppdatera skydd grupper toouse moderna Backup Storage är valfria.</span><span class="sxs-lookup"><span data-stu-id="4e69a-218">Updating protection groups toouse Modern Backup Storage is optional.</span></span> <span data-ttu-id="4e69a-219">tooupdate hello skyddsgrupp, avbryter du skyddet för alla datakällor med hjälp av hello behålla dataalternativet.</span><span class="sxs-lookup"><span data-stu-id="4e69a-219">tooupdate hello protection group, stop protection of all data sources by using hello retain data option.</span></span> <span data-ttu-id="4e69a-220">Lägg sedan till hello data sources tooa ny skyddsgrupp.</span><span class="sxs-lookup"><span data-stu-id="4e69a-220">Then, add hello data sources tooa new protection group.</span></span>

1. <span data-ttu-id="4e69a-221">Välj hello i hello-administratörskonsolen, **skydd** funktion.</span><span class="sxs-lookup"><span data-stu-id="4e69a-221">In hello Administrator Console, select hello **Protection** feature.</span></span> <span data-ttu-id="4e69a-222">I hello **Skyddsgruppsmedlem** högerklickar hello medlem, och välj sedan **stoppa skyddet av medlem**.</span><span class="sxs-lookup"><span data-stu-id="4e69a-222">In hello **Protection Group Member** list, right-click hello member, and then select **Stop protection of member**.</span></span>

  ![Stoppa skyddet av medlem](http://docs.microsoft.com/system-center/dpm/media/upgrade-to-dpm-2016/dpm-2016-stop-protection1.png)

2. <span data-ttu-id="4e69a-224">I hello **ta bort från gruppen** dialogrutan, granska hello används diskutrymme och hello ledigt utrymme för hello lagringspoolen.</span><span class="sxs-lookup"><span data-stu-id="4e69a-224">In hello **Remove from Group** dialog box, review hello used disk space and hello available free space for hello storage pool.</span></span> <span data-ttu-id="4e69a-225">hello standard är tooleave hello Återställningspunkter på disk hello och låta dem tooexpire per deras associerade bevarandeprincip.</span><span class="sxs-lookup"><span data-stu-id="4e69a-225">hello default is tooleave hello recovery points on hello disk and allow them tooexpire per their associated retention policy.</span></span> <span data-ttu-id="4e69a-226">Klicka på **OK**.</span><span class="sxs-lookup"><span data-stu-id="4e69a-226">Click **OK**.</span></span>

  <span data-ttu-id="4e69a-227">Om du vill tooimmediately returnerade hello används utrymme toohello ledigt disklagringspoolen markerar du hello **ta bort replik på disk** kryssrutan toodelete hello säkerhetskopierade data (och återställningspunkter) som är associerade med medlemmen.</span><span class="sxs-lookup"><span data-stu-id="4e69a-227">If you want tooimmediately return hello used disk space toohello free storage pool, select hello **Delete replica on disk** check box toodelete hello backup data (and recovery points) associated with that member.</span></span>

  ![Ta bort från dialogrutan grupp](http://docs.microsoft.com/system-center/dpm/media/upgrade-to-dpm-2016/dpm-2016-retain-data.png)

3. <span data-ttu-id="4e69a-229">Skapa en skyddsgrupp som använder Modern Backup Storage.</span><span class="sxs-lookup"><span data-stu-id="4e69a-229">Create a protection group that uses Modern Backup Storage.</span></span> <span data-ttu-id="4e69a-230">Inkludera hello oskyddad datakällor.</span><span class="sxs-lookup"><span data-stu-id="4e69a-230">Include hello unprotected data sources.</span></span>


## <a name="add-disks-tooincrease-legacy-storage"></a><span data-ttu-id="4e69a-231">Lägga till diskar tooincrease äldre lagringsutrymme</span><span class="sxs-lookup"><span data-stu-id="4e69a-231">Add disks tooincrease legacy storage</span></span>

<span data-ttu-id="4e69a-232">Om du vill toouse äldre lagring med Backup Server kan behöva du tooadd diskar tooincrease äldre lagring.</span><span class="sxs-lookup"><span data-stu-id="4e69a-232">If you want toouse legacy storage with Backup Server, you might need tooadd disks tooincrease legacy storage.</span></span> 

<span data-ttu-id="4e69a-233">tooadd disklagring:</span><span class="sxs-lookup"><span data-stu-id="4e69a-233">tooadd disk storage:</span></span>

1. <span data-ttu-id="4e69a-234">Välj i hello-administratörskonsolen, **Management** > **disklagring** > **Lägg till**.</span><span class="sxs-lookup"><span data-stu-id="4e69a-234">In hello Administrator Console, select **Management** > **Disk Storage** > **Add**.</span></span>

    ![Disklagring dialogrutan Lägg till](http://docs.microsoft.com/system-center/dpm/media/upgrade-to-dpm-2016/dpm-2016-add-disk-storage.png)

4. <span data-ttu-id="4e69a-236">I hello **lägger till disklagring** markerar **lägga till diskar**.</span><span class="sxs-lookup"><span data-stu-id="4e69a-236">In hello **Add Disk Storage** dialog, select **Add disks**.</span></span>

5. <span data-ttu-id="4e69a-237">Välj hello diskar som du vill tooadd, väljer i hello lista över tillgängliga diskar, **Lägg till**, och välj sedan **OK**.</span><span class="sxs-lookup"><span data-stu-id="4e69a-237">In hello list of available disks, select hello disks you want tooadd, select **Add**, and then select **OK**.</span></span>

## <a name="update-hello-data-protection-manager-protection-agent"></a><span data-ttu-id="4e69a-238">Uppdatera hello Data Protection Manager protection agent</span><span class="sxs-lookup"><span data-stu-id="4e69a-238">Update hello Data Protection Manager protection agent</span></span>

<span data-ttu-id="4e69a-239">Backup-servern använder hello System Center Data Protection Manager protection agent för uppdateringar.</span><span class="sxs-lookup"><span data-stu-id="4e69a-239">Backup Server uses hello System Center Data Protection Manager protection agent for updates.</span></span> <span data-ttu-id="4e69a-240">Om du uppgraderar en skyddsagent som inte är anslutna toohello nätverk, kan du inte använda hello Data Protection Manager Administrator Console toocomplete en ansluten agent-uppgradering.</span><span class="sxs-lookup"><span data-stu-id="4e69a-240">If you are upgrading a protection agent that is not connected toohello network, you cannot use hello Data Protection Manager Administrator Console toocomplete a connected agent upgrade.</span></span> <span data-ttu-id="4e69a-241">Du måste uppgradera hello skyddsagenten i en domänmiljö som inaktiva.</span><span class="sxs-lookup"><span data-stu-id="4e69a-241">You must upgrade hello protection agent in a nonactive domain environment.</span></span> <span data-ttu-id="4e69a-242">Tills hello klientdatorn är anslutna toohello nätverk, väntar hello Data Protection Manager-administratörskonsolen visar hello protection agent uppdateringen.</span><span class="sxs-lookup"><span data-stu-id="4e69a-242">Until hello client computer is connected toohello network, hello Data Protection Manager Administrator Console shows that hello protection agent update is pending.</span></span>

<span data-ttu-id="4e69a-243">hello följande avsnitt beskrivs hur tooupdate skyddsagenter för klientdatorer som är anslutna och klientdatorer som inte är anslutna.</span><span class="sxs-lookup"><span data-stu-id="4e69a-243">hello following sections describe how tooupdate protection agents for client computers that are connected and client computers that are not connected.</span></span>

### <a name="update-a-protection-agent-for-a-connected-client-computer"></a><span data-ttu-id="4e69a-244">Uppdatera en skyddsagent för en ansluten klientdator</span><span class="sxs-lookup"><span data-stu-id="4e69a-244">Update a protection agent for a connected client computer</span></span>

1. <span data-ttu-id="4e69a-245">I hello Backup Server-administratörskonsolen, Välj **Management** > **agenter**.</span><span class="sxs-lookup"><span data-stu-id="4e69a-245">In hello Backup Server Administrator Console, select **Management** > **Agents**.</span></span>

2. <span data-ttu-id="4e69a-246">Välj hello klientdatorer som du vill tooupdate hello-skyddsagenten i hello visningsfönstret.</span><span class="sxs-lookup"><span data-stu-id="4e69a-246">In hello display pane, select hello client computers for which you want tooupdate hello protection agent.</span></span>

  > [!NOTE]
  > <span data-ttu-id="4e69a-247">Hej **Agentuppdateringar** kolumnen visas när en uppdatering till skyddsagenten är tillgänglig för varje skyddad dator.</span><span class="sxs-lookup"><span data-stu-id="4e69a-247">hello **Agent Updates** column indicates when a protection agent update is available for each protected computer.</span></span> <span data-ttu-id="4e69a-248">I hello **åtgärder** rutan, hello **uppdatering** åtgärden är tillgänglig endast när en skyddad dator är markerad och uppdateringar är tillgängliga.</span><span class="sxs-lookup"><span data-stu-id="4e69a-248">In hello **Actions** pane, hello **Update** action is available only when a protected computer is selected and updates are available.</span></span>
  >
  >

3. <span data-ttu-id="4e69a-249">tooinstall uppdateras skyddsagenter på hello valda datorer i hello **åtgärder** väljer **uppdatering**.</span><span class="sxs-lookup"><span data-stu-id="4e69a-249">tooinstall updated protection agents on hello selected computers, in hello **Actions** pane, select **Update**.</span></span>

### <a name="update-a-protection-agent-on-a-client-computer-that-is-not-connected"></a><span data-ttu-id="4e69a-250">Uppdatera en skyddsagent på en klientdator som inte är ansluten</span><span class="sxs-lookup"><span data-stu-id="4e69a-250">Update a protection agent on a client computer that is not connected</span></span>

1. <span data-ttu-id="4e69a-251">I hello Backup Server-administratörskonsolen, Välj **Management** > **agenter**.</span><span class="sxs-lookup"><span data-stu-id="4e69a-251">In hello Backup Server Administrator Console, select **Management** > **Agents**.</span></span>

2. <span data-ttu-id="4e69a-252">Välj hello klientdatorer som du vill tooupdate hello-skyddsagenten i hello visningsfönstret.</span><span class="sxs-lookup"><span data-stu-id="4e69a-252">In hello display pane, select hello client computers for which you want tooupdate hello protection agent.</span></span>

  > [!NOTE]
   > <span data-ttu-id="4e69a-253">Hej **Agentuppdateringar** kolumnen visas när en uppdatering till skyddsagenten är tillgänglig för varje skyddad dator.</span><span class="sxs-lookup"><span data-stu-id="4e69a-253">hello **Agent Updates** column indicates when a protection agent update is available for each protected computer.</span></span> <span data-ttu-id="4e69a-254">I hello **åtgärder** rutan, hello **uppdatering** åtgärden är inte tillgänglig när en skyddad dator markeras om det finns uppdateringar.</span><span class="sxs-lookup"><span data-stu-id="4e69a-254">In hello **Actions** pane, hello **Update** action is not available when a protected computer is selected unless updates are available.</span></span>
  >
  >

3. <span data-ttu-id="4e69a-255">tooinstall uppdateras skyddsagenter på hello valda datorer, Välj **uppdatering**.</span><span class="sxs-lookup"><span data-stu-id="4e69a-255">tooinstall updated protection agents on hello selected computers, select **Update**.</span></span>

4. <span data-ttu-id="4e69a-256">För en klientdator som inte är anslutna till toohello, tills hello datorn är ansluten toohello nätverk, hello **Agentstatus** kolumnen visar statusen **väntande uppdatering**.</span><span class="sxs-lookup"><span data-stu-id="4e69a-256">For a client computer that is not connected toohello network, until hello computer is connected toohello network, hello **Agent Status** column shows a status of **Update Pending**.</span></span>

  <span data-ttu-id="4e69a-257">När en klientdator är ansluten toohello nätverk, hello **Agentuppdateringar** kolumn för hello klientdator visar statusen **uppdatering**.</span><span class="sxs-lookup"><span data-stu-id="4e69a-257">After a client computer is connected toohello network, hello **Agent Updates** column for hello client computer shows a status of **Updating**.</span></span>
  
### <a name="move-legacy-protection-groups-from-old-version-and-sync-hello-new-version-with-azure"></a><span data-ttu-id="4e69a-258">Flytta äldre skyddsgrupper från gamla och nya sync hello-versionen med Azure</span><span class="sxs-lookup"><span data-stu-id="4e69a-258">Move legacy Protection groups from old version and sync hello new version with Azure</span></span>

<span data-ttu-id="4e69a-259">När både Azure Backup Server och hello OS uppdateras, är du redo tooprotect nya datakällor med moderna Backup Storage.</span><span class="sxs-lookup"><span data-stu-id="4e69a-259">Once Azure Backup Server and hello OS are both updated, you are ready tooprotect new data sources using Modern Backup Storage.</span></span> <span data-ttu-id="4e69a-260">Men redan skyddade datakällor fortsätter toobe skyddas i hello äldre sätt som de befann sig i Azure Backup Server men alla nytt skydd med moderna Backup Storage.</span><span class="sxs-lookup"><span data-stu-id="4e69a-260">However already protected data sources will continue toobe protected in hello legacy way as they were in Azure Backup Server but all new protection will use Modern Backup Storage.</span></span>

<span data-ttu-id="4e69a-261">Är toomigrate datakällor från bakåtkompatibelt läge för skydd tooModern säkerhetskopieringslagring nedanstående steg.</span><span class="sxs-lookup"><span data-stu-id="4e69a-261">Below steps are toomigrate data sources from legacy  mode of protection tooModern backup storage.</span></span>

<span data-ttu-id="4e69a-262">• Lägg till hello nya volymerna toohello DPM-lagringspoolen och tilldela eget namn och data källkod om så önskas.</span><span class="sxs-lookup"><span data-stu-id="4e69a-262">• Add hello new volume(s) toohello DPM storage pool and assign friendly names and data source tags if desired.</span></span>
<span data-ttu-id="4e69a-263">• För varje datakälla som är i bakåtkompatibelt läge, Avbryt skyddet av hello datakällor och ”Kvarhåll skyddade Data”.</span><span class="sxs-lookup"><span data-stu-id="4e69a-263">• For each data source that is in legacy mode, stop protection of hello data sources and “Retain Protected Data”.</span></span>  <span data-ttu-id="4e69a-264">Detta gör att återställning av den äldre återställningspunkter efter migreringen.</span><span class="sxs-lookup"><span data-stu-id="4e69a-264">This will allow recovery of old recovery points after migration.</span></span>

<span data-ttu-id="4e69a-265">• Skapa en ny sida och välj hello datakällor som är toobe lagras med nya format.</span><span class="sxs-lookup"><span data-stu-id="4e69a-265">• Create a new PG and select hello data sources that are toobe stored using new format.</span></span>
<span data-ttu-id="4e69a-266">• DPM gör en kopia av replik från hello äldre säkerhetskopieringslagring till hello moderna säkerhetskopiering lagringsvolym lokalt.</span><span class="sxs-lookup"><span data-stu-id="4e69a-266">• DPM will do a replica copy from hello legacy backup storage into hello Modern Backup Storage volume locally.</span></span>
<span data-ttu-id="4e69a-267">Obs: Detta visas som en återställning efter jobb • alla nya synkronisering och återställningspunkter sparas i den moderna Backup Storage.</span><span class="sxs-lookup"><span data-stu-id="4e69a-267">Note: This will be seen as a post-recovery operation job • All new sync and recovery points will then be stored in Modern Backup Storage.</span></span>
<span data-ttu-id="4e69a-268">• Gamla återställningspunkterna rensas ut eftersom de går ut och slutligen Frigör hello diskutrymme.</span><span class="sxs-lookup"><span data-stu-id="4e69a-268">• Old recovery points will be pruned out as they expire and eventually free up hello disk space.</span></span>
<span data-ttu-id="4e69a-269">• När alla hello äldre volymer tas bort från hello gamla lagringsenheter, hello disk kan tas bort från Azure backup och hello system.</span><span class="sxs-lookup"><span data-stu-id="4e69a-269">• Once all hello legacy volumes are deleted from hello old storage, hello disk can be removed from Azure backup and hello system.</span></span>
<span data-ttu-id="4e69a-270">• Ta en säkerhetskopia av hello Azure DPMDB.</span><span class="sxs-lookup"><span data-stu-id="4e69a-270">• Take a backup of hello  Azure DPMDB.</span></span>

<span data-ttu-id="4e69a-271">Del 2:-viktiga objekt > Hej nya servern behöver toobe samma namn som hello ursprungliga Azure Backup-server.</span><span class="sxs-lookup"><span data-stu-id="4e69a-271">Part 2: -Important items> hello new server will need toobe named same as hello original Azure Backup server.</span></span> <span data-ttu-id="4e69a-272">Du kan inte ändra hello namnet på hello nya Azure backup-server om du vill toouse gamla lagringspoolen och DPMDB tooretain återställningspunkter - måste ha säkerhetskopiering av DPM-databasen behöver återställas toobe</span><span class="sxs-lookup"><span data-stu-id="4e69a-272">You cannot change hello name of hello new Azure backup server if you want toouse old storage pool and DPMDB tooretain recovery points -Must have backup of DPMDB  as it will need toobe restored</span></span>

1) <span data-ttu-id="4e69a-273">Avstängning hello ursprungliga Azure backup-server eller starta hello-överföring.</span><span class="sxs-lookup"><span data-stu-id="4e69a-273">Shutdown hello original Azure backup server or take it off hello wire.</span></span>
2) <span data-ttu-id="4e69a-274">Återställa hello datorkonto i active directory.</span><span class="sxs-lookup"><span data-stu-id="4e69a-274">Reset hello machine account in active directory.</span></span>
3) <span data-ttu-id="4e69a-275">Installera Server 2016 på den nya datorn och namn som den hello samma datornamn som hello ursprungliga Azure Backup-server.</span><span class="sxs-lookup"><span data-stu-id="4e69a-275">Install Server 2016 on new machine and name it hello same machine name as hello original Azure Backup server.</span></span>
4) <span data-ttu-id="4e69a-276">Anslut hello domän</span><span class="sxs-lookup"><span data-stu-id="4e69a-276">Join hello Domain</span></span>
5) <span data-ttu-id="4e69a-277">Installera Azure Backup server V2 (flytta DPM-lagringspooldiskar från den gamla servern och importera)</span><span class="sxs-lookup"><span data-stu-id="4e69a-277">Install Azure Backup server V2 (Move DPM Storage pool disks from old server and import)</span></span>
6) <span data-ttu-id="4e69a-278">Återställa hello DPMDB tas från slutet av del 2</span><span class="sxs-lookup"><span data-stu-id="4e69a-278">Restore hello DPMDB taken from end of part 2</span></span>
7) <span data-ttu-id="4e69a-279">Bifoga hello lagring från hello ursprungliga reservserver toohello ny server.</span><span class="sxs-lookup"><span data-stu-id="4e69a-279">Attach hello storage from hello original backup server toohello new server.</span></span>
8) <span data-ttu-id="4e69a-280">Från SQL återställa hello DPMDB</span><span class="sxs-lookup"><span data-stu-id="4e69a-280">From SQL Restore hello DPMDB</span></span>
9) <span data-ttu-id="4e69a-281">Admin kommandoraden på nya server cd tooMicrosoft Azure Backup för att installera platsen och bin-mappen</span><span class="sxs-lookup"><span data-stu-id="4e69a-281">From admin command line on new server cd tooMicrosoft Azure Backup install location and bin folder</span></span>

<span data-ttu-id="4e69a-282">Exempel på sökvägen: C:\windows\system32 > cd ”c:\Program Files\Microsoft Azure Backup\DPM\DPM\bin\\</span><span class="sxs-lookup"><span data-stu-id="4e69a-282">Path example: C:\windows\system32>cd "c:\Program Files\Microsoft Azure Backup\DPM\DPM\bin\\</span></span>
<span data-ttu-id="4e69a-283">tooAzure säkerhetskopiering kör DPMSYNC-SYNC</span><span class="sxs-lookup"><span data-stu-id="4e69a-283">tooAzure backup Run DPMSYNC -SYNC</span></span>

10) <span data-ttu-id="4e69a-284">Kör DPMSYNC-SYNC Obs Om du har lagt till nya diskar toohello DPM lagringspoolen i stället för att flytta hello gamla, kör DPMSYNC - Reallocatereplica</span><span class="sxs-lookup"><span data-stu-id="4e69a-284">Run DPMSYNC -SYNC Note If you have added NEW disks toohello DPM Storage pool instead of moving hello old ones, then run DPMSYNC -Reallocatereplica</span></span>

## <a name="new-powershell-cmdlets-in-v2"></a><span data-ttu-id="4e69a-285">Nya PowerShell-cmdlets i v2</span><span class="sxs-lookup"><span data-stu-id="4e69a-285">New PowerShell cmdlets in v2</span></span>

<span data-ttu-id="4e69a-286">Två nya cmdlets finns tillgängliga när du installerar Azure Backup Server v2:</span><span class="sxs-lookup"><span data-stu-id="4e69a-286">When you install Azure Backup Server v2, two new cmdlets are available:</span></span> 
* [<span data-ttu-id="4e69a-287">Montera DPMRecoveryPoint</span><span class="sxs-lookup"><span data-stu-id="4e69a-287">Mount-DPMRecoveryPoint</span></span>](https://technet.microsoft.com/library/mt787159.aspx)
* [<span data-ttu-id="4e69a-288">Demontera DPMRecoveryPoint</span><span class="sxs-lookup"><span data-stu-id="4e69a-288">Dismount-DPMRecoveryPoint</span></span>](https://technet.microsoft.com/library/mt787158.aspx)

## <a name="next-steps"></a><span data-ttu-id="4e69a-289">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="4e69a-289">Next steps</span></span>

<span data-ttu-id="4e69a-290">Lär dig hur tooprepare servern eller börja skydda en arbetsbelastning:</span><span class="sxs-lookup"><span data-stu-id="4e69a-290">Learn how tooprepare your server or begin protecting a workload:</span></span>
- [<span data-ttu-id="4e69a-291">Förbereda säkerhetskopiering serverarbetsbelastningar</span><span class="sxs-lookup"><span data-stu-id="4e69a-291">Prepare Backup Server workloads</span></span>](backup-azure-microsoft-azure-backup.md)
- [<span data-ttu-id="4e69a-292">Använda Backup Server tooback in en VMware-server</span><span class="sxs-lookup"><span data-stu-id="4e69a-292">Use Backup Server tooback up a VMware server</span></span>](backup-azure-backup-server-vmware.md)
- [<span data-ttu-id="4e69a-293">Använda Backup Server tooback in SQL Server</span><span class="sxs-lookup"><span data-stu-id="4e69a-293">Use Backup Server tooback up SQL Server</span></span>](backup-azure-sql-mabs.md)
- [<span data-ttu-id="4e69a-294">Använda moderna lagring för säkerhetskopiering med Backup-Server</span><span class="sxs-lookup"><span data-stu-id="4e69a-294">Use Modern Backup Storage with Backup Server</span></span>](backup-mabs-add-storage.md)

