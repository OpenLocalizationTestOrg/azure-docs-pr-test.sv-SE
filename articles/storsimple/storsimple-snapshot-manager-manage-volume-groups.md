---
title: aaaStorSimple Snapshot Manager volym grupper | Microsoft Docs
description: "Beskriver hur toouse hello StorSimple Snapshot Manager MMC snapin-modulen toocreate och hantera grupper för volymen."
services: storsimple
documentationcenter: NA
author: SharS
manager: carmonm
editor: 
ms.assetid: 7a232414-6a28-4b81-bd7b-cf61e28b33d7
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 06/05/2017
ms.author: v-sharos
ms.openlocfilehash: f7830b62761db7aa5139456b555b6168fb6a85de
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="use-storsimple-snapshot-manager-toocreate-and-manage-volume-groups"></a><span data-ttu-id="723d6-103">Använda StorSimple Snapshot Manager toocreate och hantera grupper av volym</span><span class="sxs-lookup"><span data-stu-id="723d6-103">Use StorSimple Snapshot Manager toocreate and manage volume groups</span></span>
## <a name="overview"></a><span data-ttu-id="723d6-104">Översikt</span><span class="sxs-lookup"><span data-stu-id="723d6-104">Overview</span></span>
<span data-ttu-id="723d6-105">Du kan använda hello **volym grupper** nod på hello **omfång** tooassign volymer toovolume grupper, visa information om en volym-grupp, schemalägga säkerhetskopieringar och redigera volymen grupper.</span><span class="sxs-lookup"><span data-stu-id="723d6-105">You can use hello **Volume Groups** node on hello **Scope** pane tooassign volumes toovolume groups, view information about a volume group, schedule backups, and edit volume groups.</span></span>

<span data-ttu-id="723d6-106">Volymen grupper är pooler för relaterade volymer används tooensure att säkerhetskopiorna är programkonsekventa.</span><span class="sxs-lookup"><span data-stu-id="723d6-106">Volume groups are pools of related volumes used tooensure that backups are application-consistent.</span></span> <span data-ttu-id="723d6-107">Mer information finns i [volymer och volymen grupper](storsimple-what-is-snapshot-manager.md#volumes-and-volume-groups) och [integrering med Windows Volume Shadow Copy-tjänsten](storsimple-what-is-snapshot-manager.md#integration-with-windows-volume-shadow-copy-service).</span><span class="sxs-lookup"><span data-stu-id="723d6-107">For more information, see [Volumes and volume groups](storsimple-what-is-snapshot-manager.md#volumes-and-volume-groups) and [Integration with Windows Volume Shadow Copy Service](storsimple-what-is-snapshot-manager.md#integration-with-windows-volume-shadow-copy-service).</span></span>

> [!IMPORTANT]
> * <span data-ttu-id="723d6-108">Alla volymer i en volym-grupp måste komma från en enda molntjänstleverantören.</span><span class="sxs-lookup"><span data-stu-id="723d6-108">All volumes in a volume group must come from a single cloud service provider.</span></span>
> * <span data-ttu-id="723d6-109">När du konfigurerar volymen grupper Blanda inte klusterdelade volymer (CSV) och icke-CSV: er i hello samma grupp av volymen.</span><span class="sxs-lookup"><span data-stu-id="723d6-109">When you configure volume groups, do not mix cluster-shared volumes (CSVs) and non-CSVs in hello same volume group.</span></span> <span data-ttu-id="723d6-110">StorSimple Snapshot Manager stöder inte en blandning av CSV: er och icke-CSV: er i hello samma ögonblicksbild.</span><span class="sxs-lookup"><span data-stu-id="723d6-110">StorSimple Snapshot Manager does not support a mix of CSVs and non-CSVs in hello same snapshot.</span></span>

![Volymen Gruppnod](./media/storsimple-snapshot-manager-manage-volume-groups/HCS_SSM_Volume_groups.png)

<span data-ttu-id="723d6-112">**Bild 1: StorSimple Snapshot Manager volym Gruppnod**</span><span class="sxs-lookup"><span data-stu-id="723d6-112">**Figure 1: StorSimple Snapshot Manager Volume Groups node**</span></span> 

<span data-ttu-id="723d6-113">Den här självstudiekursen beskriver hur du kan använda StorSimple Snapshot Manager till:</span><span class="sxs-lookup"><span data-stu-id="723d6-113">This tutorial explains how you can use StorSimple Snapshot Manager to:</span></span>

* <span data-ttu-id="723d6-114">Visa information om volym-grupper</span><span class="sxs-lookup"><span data-stu-id="723d6-114">View information about your volume groups</span></span>
* <span data-ttu-id="723d6-115">Skapa en volym-grupp</span><span class="sxs-lookup"><span data-stu-id="723d6-115">Create a volume group</span></span>
* <span data-ttu-id="723d6-116">Säkerhetskopiera en volym-grupp</span><span class="sxs-lookup"><span data-stu-id="723d6-116">Back up a volume group</span></span>
* <span data-ttu-id="723d6-117">Redigera en grupp av volym</span><span class="sxs-lookup"><span data-stu-id="723d6-117">Edit a volume group</span></span>
* <span data-ttu-id="723d6-118">Ta bort en volym-grupp</span><span class="sxs-lookup"><span data-stu-id="723d6-118">Delete a volume group</span></span>

<span data-ttu-id="723d6-119">Alla dessa åtgärder är också tillgängliga på hello **åtgärder** fönstret.</span><span class="sxs-lookup"><span data-stu-id="723d6-119">All of these actions are also available on hello **Actions** pane.</span></span>

## <a name="view-volume-groups"></a><span data-ttu-id="723d6-120">Visa grupper av volym</span><span class="sxs-lookup"><span data-stu-id="723d6-120">View volume groups</span></span>
<span data-ttu-id="723d6-121">Om du klickar på hello **volym grupper** nod, hello **resultat** visar hello följande information om varje volym-grupp, beroende på hello kolumnmarkeringarna du gör.</span><span class="sxs-lookup"><span data-stu-id="723d6-121">If you click hello **Volume Groups** node, hello **Results** pane shows hello following information about each volume group, depending on hello column selections you make.</span></span> <span data-ttu-id="723d6-122">(hello kolumner i hello **resultat** rutan kan konfigureras.</span><span class="sxs-lookup"><span data-stu-id="723d6-122">(hello columns in hello **Results** pane are configurable.</span></span> <span data-ttu-id="723d6-123">Högerklicka på hello **volymer** väljer **visa**, och välj sedan **Lägg till/ta bort kolumner**.)</span><span class="sxs-lookup"><span data-stu-id="723d6-123">Right-click hello **Volumes** node, select **View**, and then select **Add/Remove Columns**.)</span></span>

| <span data-ttu-id="723d6-124">Resultaten kolumn</span><span class="sxs-lookup"><span data-stu-id="723d6-124">Results column</span></span> | <span data-ttu-id="723d6-125">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="723d6-125">Description</span></span> |
|:--- |:--- |
| <span data-ttu-id="723d6-126">Namn</span><span class="sxs-lookup"><span data-stu-id="723d6-126">Name</span></span> |<span data-ttu-id="723d6-127">Hej **namn** kolumnen innehåller hello hello volym gruppens namn.</span><span class="sxs-lookup"><span data-stu-id="723d6-127">hello **Name** column contains hello name of hello volume group.</span></span> |
| <span data-ttu-id="723d6-128">Program</span><span class="sxs-lookup"><span data-stu-id="723d6-128">Application</span></span> |<span data-ttu-id="723d6-129">Hej **program** kolumnen visar hello antal VSS-skrivare som är installerade och körs på hello Windows-värd.</span><span class="sxs-lookup"><span data-stu-id="723d6-129">hello **Applications** column shows hello number of VSS writers currently installed and running on hello Windows host.</span></span> |
| <span data-ttu-id="723d6-130">Vald</span><span class="sxs-lookup"><span data-stu-id="723d6-130">Selected</span></span> |<span data-ttu-id="723d6-131">Hej **valda** kolumnen visar hello antalet volymer som ingår i gruppen för hello-volym.</span><span class="sxs-lookup"><span data-stu-id="723d6-131">hello **Selected** column shows hello number of volumes that are contained in hello volume group.</span></span> <span data-ttu-id="723d6-132">Noll (0) anger att inget program som är associerad med hello volymer i hello volym grupp.</span><span class="sxs-lookup"><span data-stu-id="723d6-132">A zero (0) indicates that no application is associated with hello volumes in hello volume group.</span></span> |
| <span data-ttu-id="723d6-133">Importera</span><span class="sxs-lookup"><span data-stu-id="723d6-133">Imported</span></span> |<span data-ttu-id="723d6-134">Hej **importerade** kolumnen visar hello antalet importerade volymer.</span><span class="sxs-lookup"><span data-stu-id="723d6-134">hello **Imported** column shows hello number of imported volumes.</span></span> <span data-ttu-id="723d6-135">När värdet för**SANT**, denna kolumn indikerar att en volym-grupp har importerats från hello Azure-portalen och inte har skapats i StorSimple Snapshot Manager.</span><span class="sxs-lookup"><span data-stu-id="723d6-135">When set too**True**, this column indicates that a volume group was imported from hello Azure portal and was not created in StorSimple Snapshot Manager.</span></span> |

> [!NOTE]
> <span data-ttu-id="723d6-136">StorSimple Snapshot Manager volym grupper visas också på hello **Säkerhetskopieringsprinciper** fliken i hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="723d6-136">StorSimple Snapshot Manager volume groups are also displayed on hello **Backup Policies** tab in hello Azure portal.</span></span>
> 
> 

## <a name="create-a-volume-group"></a><span data-ttu-id="723d6-137">Skapa en volym-grupp</span><span class="sxs-lookup"><span data-stu-id="723d6-137">Create a volume group</span></span>
<span data-ttu-id="723d6-138">Använd hello följa proceduren toocreate en volym-grupp.</span><span class="sxs-lookup"><span data-stu-id="723d6-138">Use hello following procedure toocreate a volume group.</span></span>

#### <a name="toocreate-a-volume-group"></a><span data-ttu-id="723d6-139">toocreate en volym-grupp</span><span class="sxs-lookup"><span data-stu-id="723d6-139">toocreate a volume group</span></span>
1. <span data-ttu-id="723d6-140">Klicka på hello skrivbordsikon toostart StorSimple Snapshot Manager.</span><span class="sxs-lookup"><span data-stu-id="723d6-140">Click hello desktop icon toostart StorSimple Snapshot Manager.</span></span>
2. <span data-ttu-id="723d6-141">I hello **omfång** fönstret högerklickar du på **volym grupper**, och klicka sedan på **skapa volymen grupp**.</span><span class="sxs-lookup"><span data-stu-id="723d6-141">In hello **Scope** pane, right-click **Volume Groups**, and then click **Create Volume Group**.</span></span>
   
    ![Skapa volymen grupp](./media/storsimple-snapshot-manager-manage-volume-groups/HCS_SSM_Create_volume_group.png)
   
    <span data-ttu-id="723d6-143">Hej **skapa en volym grupp** dialogrutan visas.</span><span class="sxs-lookup"><span data-stu-id="723d6-143">hello **Create a volume group** dialog box appears.</span></span>
   
    ![Skapa en volym grupp dialog](./media/storsimple-snapshot-manager-manage-volume-groups/HCS_SSM_CreateVolumeGroup_dialog.png)
3. <span data-ttu-id="723d6-145">Ange hello följande information:</span><span class="sxs-lookup"><span data-stu-id="723d6-145">Enter hello following information:</span></span>
   
   1. <span data-ttu-id="723d6-146">I hello **namn** Skriv ett unikt namn för hello ny volym grupp.</span><span class="sxs-lookup"><span data-stu-id="723d6-146">In hello **Name** box, type a unique name for hello new volume group.</span></span>
   2. <span data-ttu-id="723d6-147">I hello **program** rutan, Välj program som är associerade med hello volymer som du lägger till toohello volym grupp.</span><span class="sxs-lookup"><span data-stu-id="723d6-147">In hello **Applications** box, select applications associated with hello volumes that you will be adding toohello volume group.</span></span>
      
       <span data-ttu-id="723d6-148">Hej **program** Listar de program som använder StorSimple-volymer och VSS-skrivare har aktiverats för dem.</span><span class="sxs-lookup"><span data-stu-id="723d6-148">hello **Applications** box lists only those applications that use StorSimple volumes and have VSS writers enabled for them.</span></span> <span data-ttu-id="723d6-149">En VSS-skrivare aktiveras bara om alla hello volymer att hello-skrivaren är medveten om är StorSimple-volymer.</span><span class="sxs-lookup"><span data-stu-id="723d6-149">A VSS writer is enabled only if all hello volumes that hello writer is aware of are StorSimple volumes.</span></span> <span data-ttu-id="723d6-150">Om hello program rutan är tom, installeras inga program som använder Azure StorSimple-volymer och har stöd för VSS-skrivare.</span><span class="sxs-lookup"><span data-stu-id="723d6-150">If hello Applications box is empty, then no applications that use Azure StorSimple volumes and have supported VSS writers are installed.</span></span> <span data-ttu-id="723d6-151">(För närvarande Azure StorSimple stöder Microsoft Exchange och SQL Server.) Mer information om VSS-skrivare finns [integrering med Windows Volume Shadow Copy-tjänsten](storsimple-what-is-snapshot-manager.md#integration-with-windows-volume-shadow-copy-service).</span><span class="sxs-lookup"><span data-stu-id="723d6-151">(Currently, Azure StorSimple supports Microsoft Exchange and SQL Server.) For more information about VSS writers, see [Integration with Windows Volume Shadow Copy Service](storsimple-what-is-snapshot-manager.md#integration-with-windows-volume-shadow-copy-service).</span></span>
      
       <span data-ttu-id="723d6-152">Om du väljer ett program, väljs automatiskt alla volymer som är kopplade till den.</span><span class="sxs-lookup"><span data-stu-id="723d6-152">If you select an application, all volumes associated with it are automatically selected.</span></span> <span data-ttu-id="723d6-153">Å andra sidan om du väljer volymer som är associerade med ett specifikt program hello programmet väljs automatiskt i hello **program** rutan.</span><span class="sxs-lookup"><span data-stu-id="723d6-153">Conversely, if you select volumes associated with a specific application, hello application is automatically selected in hello **Applications** box.</span></span> 
   3. <span data-ttu-id="723d6-154">I hello **volymer** väljer StorSimple-volymer tooadd toohello volym gruppen.</span><span class="sxs-lookup"><span data-stu-id="723d6-154">In hello **Volumes** box, select StorSimple volumes tooadd toohello volume group.</span></span> 
      
      * <span data-ttu-id="723d6-155">Du kan inkludera volymer med en eller flera partitioner.</span><span class="sxs-lookup"><span data-stu-id="723d6-155">You can include volumes with single or multiple partitions.</span></span> <span data-ttu-id="723d6-156">(Flera volymer för partitionen kan vara dynamiska diskar eller standarddiskar med flera partitioner.) En volym som innehåller flera partitioner behandlas som en enda enhet.</span><span class="sxs-lookup"><span data-stu-id="723d6-156">(Multiple partition volumes can be dynamic disks or basic disks with multiple partitions.) A volume that contains multiple partitions is treated as a single unit.</span></span> <span data-ttu-id="723d6-157">Därför, om du lägger till endast en av hello partitioner tooa volym gruppen Alla hello andra partitioner är automatiskt tillagda toothat volym gruppera vid hello samma tid.</span><span class="sxs-lookup"><span data-stu-id="723d6-157">Consequently, if you add only one of hello partitions tooa volume group, all hello other partitions are automatically added toothat volume group at hello same time.</span></span> <span data-ttu-id="723d6-158">När du har lagt till en flera partition volym tooa volym grupp hello flera partition volym fortsätter toobe behandlas som en enda enhet.</span><span class="sxs-lookup"><span data-stu-id="723d6-158">After you add a multiple partition volume tooa volume group, hello multiple partition volume continues toobe treated as a single unit.</span></span>
      * <span data-ttu-id="723d6-159">Du kan skapa grupper med tomma volym genom att inte tilldela alla volymer toothem.</span><span class="sxs-lookup"><span data-stu-id="723d6-159">You can create empty volume groups by not assigning any volumes toothem.</span></span> 
      * <span data-ttu-id="723d6-160">Blanda inte klusterdelade volymer (CSV) och icke-CSV: er i hello samma grupp av volymen.</span><span class="sxs-lookup"><span data-stu-id="723d6-160">Do not mix cluster-shared volumes (CSVs) and non-CSVs in hello same volume group.</span></span> <span data-ttu-id="723d6-161">StorSimple Snapshot Manager stöder inte en blandning av CSV-volymer och icke-CSV-volymer i hello samma ögonblicksbild.</span><span class="sxs-lookup"><span data-stu-id="723d6-161">StorSimple Snapshot Manager does not support a mix of CSV volumes and non-CSV volumes in hello same snapshot.</span></span>
4. <span data-ttu-id="723d6-162">Klicka på **OK** toosave hello volym grupp.</span><span class="sxs-lookup"><span data-stu-id="723d6-162">Click **OK** toosave hello volume group.</span></span>

## <a name="back-up-a-volume-group"></a><span data-ttu-id="723d6-163">Säkerhetskopiera en volym-grupp</span><span class="sxs-lookup"><span data-stu-id="723d6-163">Back up a volume group</span></span>
<span data-ttu-id="723d6-164">Använd hello följa proceduren tooback upp en volym-grupp.</span><span class="sxs-lookup"><span data-stu-id="723d6-164">Use hello following procedure tooback up a volume group.</span></span>

#### <a name="tooback-up-a-volume-group"></a><span data-ttu-id="723d6-165">tooback upp en volym-grupp</span><span class="sxs-lookup"><span data-stu-id="723d6-165">tooback up a volume group</span></span>
1. <span data-ttu-id="723d6-166">Klicka på hello skrivbordsikon toostart StorSimple Snapshot Manager.</span><span class="sxs-lookup"><span data-stu-id="723d6-166">Click hello desktop icon toostart StorSimple Snapshot Manager.</span></span>
2. <span data-ttu-id="723d6-167">I hello **omfång** rutan Expandera hello **volym grupper** , högerklickar du på namnet på en volym grupp och klicka sedan på **ta säkerhetskopia**.</span><span class="sxs-lookup"><span data-stu-id="723d6-167">In hello **Scope** pane, expand hello **Volume Groups** node, right-click a volume group name, and then click **Take Backup**.</span></span>
   
    ![Säkerhetskopiera volym grupp omedelbart](./media/storsimple-snapshot-manager-manage-volume-groups/HCS_SSM_Take_backup.png)
3. <span data-ttu-id="723d6-169">I hello **ta säkerhetskopia** dialogrutan **lokal ögonblicksbild** eller **ögonblicksbild i molnet**, och klicka sedan på **skapa**.</span><span class="sxs-lookup"><span data-stu-id="723d6-169">In hello **Take Backup** dialog box, select **Local Snapshot** or **Cloud Snapshot**, and then click **Create**.</span></span>
   
    ![Ta säkerhetskopiering](./media/storsimple-snapshot-manager-manage-volume-groups/HCS_SSM_TakeBackup_dialog.png)
4. <span data-ttu-id="723d6-171">tooconfirm hello säkerhetskopiering körs, expandera hello **jobb** noden och klicka sedan på **kör**.</span><span class="sxs-lookup"><span data-stu-id="723d6-171">tooconfirm that hello backup is running, expand hello **Jobs** node, and then click **Running**.</span></span> <span data-ttu-id="723d6-172">hello säkerhetskopiering ska visas.</span><span class="sxs-lookup"><span data-stu-id="723d6-172">hello backup should be listed.</span></span>
5. <span data-ttu-id="723d6-173">tooview hello slutförts ögonblicksbild, expandera hello **säkerhetskopieringskatalog** nod, expanderar hello volym gruppnamnet och klickar sedan **lokal ögonblicksbild** eller **ögonblicksbild i molnet**.</span><span class="sxs-lookup"><span data-stu-id="723d6-173">tooview hello completed snapshot, expand hello **Backup Catalog** node, expand hello volume group name, and then click **Local Snapshot** or **Cloud Snapshot**.</span></span> <span data-ttu-id="723d6-174">hello säkerhetskopiering visas om den har slutförts.</span><span class="sxs-lookup"><span data-stu-id="723d6-174">hello backup will be listed if it finished successfully.</span></span>

## <a name="edit-a-volume-group"></a><span data-ttu-id="723d6-175">Redigera en grupp av volym</span><span class="sxs-lookup"><span data-stu-id="723d6-175">Edit a volume group</span></span>
<span data-ttu-id="723d6-176">Använd hello följa proceduren tooedit en volym-grupp.</span><span class="sxs-lookup"><span data-stu-id="723d6-176">Use hello following procedure tooedit a volume group.</span></span>

#### <a name="tooedit-a-volume-group"></a><span data-ttu-id="723d6-177">tooedit en volym-grupp</span><span class="sxs-lookup"><span data-stu-id="723d6-177">tooedit a volume group</span></span>
1. <span data-ttu-id="723d6-178">Klicka på hello skrivbordsikon toostart StorSimple Snapshot Manager.</span><span class="sxs-lookup"><span data-stu-id="723d6-178">Click hello desktop icon toostart StorSimple Snapshot Manager.</span></span>
2. <span data-ttu-id="723d6-179">I hello **omfång** rutan Expandera hello **volym grupper** , högerklickar du på namnet på en volym grupp och klicka sedan på **redigera**.</span><span class="sxs-lookup"><span data-stu-id="723d6-179">In hello **Scope** pane, expand hello **Volume Groups** node, right-click a volume group name, and then click **Edit**.</span></span>
3. <span data-ttu-id="723d6-180">hello ** skapa en volym grupp ** dialogrutan visas.</span><span class="sxs-lookup"><span data-stu-id="723d6-180">hello **Create a volume group **dialog box appears.</span></span> <span data-ttu-id="723d6-181">Du kan ändra hello **namn**, **program**, och **volymer** poster.</span><span class="sxs-lookup"><span data-stu-id="723d6-181">You can change hello **Name**, **Applications**, and **Volumes** entries.</span></span>
4. <span data-ttu-id="723d6-182">Klicka på **OK** toosave ändringarna.</span><span class="sxs-lookup"><span data-stu-id="723d6-182">Click **OK** toosave your changes.</span></span>

## <a name="delete-a-volume-group"></a><span data-ttu-id="723d6-183">Ta bort en volym-grupp</span><span class="sxs-lookup"><span data-stu-id="723d6-183">Delete a volume group</span></span>
<span data-ttu-id="723d6-184">Använd hello följa proceduren toodelete en volym-grupp.</span><span class="sxs-lookup"><span data-stu-id="723d6-184">Use hello following procedure toodelete a volume group.</span></span> 

> [!WARNING]
> <span data-ttu-id="723d6-185">Det här tas även bort alla hello säkerhetskopior som är associerat med hello volym grupp.</span><span class="sxs-lookup"><span data-stu-id="723d6-185">This also deletes all hello backups associated with hello volume group.</span></span>
> 
> 

#### <a name="toodelete-a-volume-group"></a><span data-ttu-id="723d6-186">toodelete en volym-grupp</span><span class="sxs-lookup"><span data-stu-id="723d6-186">toodelete a volume group</span></span>
1. <span data-ttu-id="723d6-187">Klicka på hello skrivbordsikon toostart StorSimple Snapshot Manager.</span><span class="sxs-lookup"><span data-stu-id="723d6-187">Click hello desktop icon toostart StorSimple Snapshot Manager.</span></span>
2. <span data-ttu-id="723d6-188">I hello **omfång** rutan Expandera hello **volym grupper** , högerklickar du på namnet på en volym grupp och klicka sedan på **ta bort**.</span><span class="sxs-lookup"><span data-stu-id="723d6-188">In hello **Scope** pane, expand hello **Volume Groups** node, right-click a volume group name, and then click **Delete**.</span></span>
3. <span data-ttu-id="723d6-189">Hej **ta bort volymen grupp** dialogrutan visas.</span><span class="sxs-lookup"><span data-stu-id="723d6-189">hello **Delete Volume Group** dialog box appears.</span></span> <span data-ttu-id="723d6-190">Typen **Bekräfta** i hello textrutan och klicka sedan på **OK**.</span><span class="sxs-lookup"><span data-stu-id="723d6-190">Type **Confirm** in hello text box, and then click **OK**.</span></span>
   
    <span data-ttu-id="723d6-191">hello bort volymen grupp försvinner hello listan i hello **resultat** rutan och alla säkerhetskopior som är associerade med gruppen volym tas bort från hello säkerhetskopieringskatalogen.</span><span class="sxs-lookup"><span data-stu-id="723d6-191">hello deleted volume group vanishes from hello list in hello **Results** pane and all backups that are associated with that volume group are deleted from hello backup catalog.</span></span>

## <a name="next-steps"></a><span data-ttu-id="723d6-192">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="723d6-192">Next steps</span></span>
* <span data-ttu-id="723d6-193">Lär dig hur för[med StorSimple Snapshot Manager tooadminister din StorSimple-lösning](storsimple-snapshot-manager-admin.md).</span><span class="sxs-lookup"><span data-stu-id="723d6-193">Learn how too[use StorSimple Snapshot Manager tooadminister your StorSimple solution](storsimple-snapshot-manager-admin.md).</span></span>
* <span data-ttu-id="723d6-194">Lär dig hur för[använda StorSimple Snapshot Manager toocreate och hantera principer för säkerhetskopiering](storsimple-snapshot-manager-manage-backup-policies.md).</span><span class="sxs-lookup"><span data-stu-id="723d6-194">Learn how too[use StorSimple Snapshot Manager toocreate and manage backup policies](storsimple-snapshot-manager-manage-backup-policies.md).</span></span>

