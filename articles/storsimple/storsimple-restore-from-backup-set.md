---
title: "aaaRestore en StorSimple-volym från en säkerhetskopia | Microsoft Docs"
description: "Förklarar hur hello toouse StorSimple Manager service säkerhetskopieringskatalogen sidan toorestore en StorSimple-volym från en säkerhetskopia."
services: storsimple
documentationcenter: NA
author: alkohli
manager: carmonm
editor: 
ms.assetid: b979782e-3184-4465-ad5f-e4516a5885d2
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 08/17/2016
ms.author: alkohli
ms.openlocfilehash: e0efa74b14603be41af0cfc5400de3c39ab8824e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="restore-a-storsimple-volume-from-a-backup-set"></a><span data-ttu-id="c13aa-103">Återställa en StorSimple-volym från en säkerhetskopia</span><span class="sxs-lookup"><span data-stu-id="c13aa-103">Restore a StorSimple volume from a backup set</span></span>
[!INCLUDE [storsimple-version-selector-restore-from-backup](../../includes/storsimple-version-selector-restore-from-backup.md)]

## <a name="overview"></a><span data-ttu-id="c13aa-104">Översikt</span><span class="sxs-lookup"><span data-stu-id="c13aa-104">Overview</span></span>
<span data-ttu-id="c13aa-105">Hej **säkerhetskopieringskatalogen** sidan visas alla hello säkerhetskopior som skapas när manuell eller automatisk säkerhetskopiering utförs.</span><span class="sxs-lookup"><span data-stu-id="c13aa-105">hello **Backup Catalog** page displays all hello backup sets that are created when manual or automated backups are taken.</span></span> <span data-ttu-id="c13aa-106">Du kan använda den här sidan toolist alla hello säkerhetskopior för en princip för säkerhetskopiering eller en volym, Välj eller ta bort eller använda en säkerhetskopiering toorestore eller klona en volym.</span><span class="sxs-lookup"><span data-stu-id="c13aa-106">You can use this page toolist all hello backups for a backup policy or a volume, select or delete backups, or use a backup toorestore or clone a volume.</span></span>

 ![Säkerhetskopiera katalog](./media/storsimple-restore-from-backup-set/HCS_BackupCatalog.png)

<span data-ttu-id="c13aa-108">Den här självstudiekursen beskrivs hur toouse hello **säkerhetskopieringskatalogen** sidan toorestore en volym på din enhet från en säkerhetskopia.</span><span class="sxs-lookup"><span data-stu-id="c13aa-108">This tutorial explains how toouse hello **Backup Catalog** page toorestore a volume on your device from a backup set.</span></span>

## <a name="how-toouse-hello-backup-catalog"></a><span data-ttu-id="c13aa-109">Hur toouse hello säkerhetskopieringskatalogen</span><span class="sxs-lookup"><span data-stu-id="c13aa-109">How toouse hello backup catalog</span></span>
<span data-ttu-id="c13aa-110">Hej **säkerhetskopieringskatalogen** innehåller en fråga som hjälper dig att toonarrow valet av säkerhetskopian.</span><span class="sxs-lookup"><span data-stu-id="c13aa-110">hello **Backup Catalog** page provides a query that helps you toonarrow your backup set selection.</span></span> <span data-ttu-id="c13aa-111">Du kan filtrera hello säkerhetskopior som hämtas baserat på hello följande parametrar:</span><span class="sxs-lookup"><span data-stu-id="c13aa-111">You can filter hello backup sets that are retrieved based on hello following parameters:</span></span>

* <span data-ttu-id="c13aa-112">**Enheten** – hello enhet på vilken hello säkerhetskopian skapades.</span><span class="sxs-lookup"><span data-stu-id="c13aa-112">**Device** – hello device on which hello backup set was created.</span></span>
* <span data-ttu-id="c13aa-113">**Säkerhetskopiera princip** eller **volym** – hello säkerhetskopieringsprincip eller volym som är associerade med den här säkerhetskopian.</span><span class="sxs-lookup"><span data-stu-id="c13aa-113">**Backup policy** or **volume** – hello backup policy or volume associated with this backup set.</span></span>
* <span data-ttu-id="c13aa-114">**Från** och **till** – hello intervallet för datum och tid när hello säkerhetskopian skapades.</span><span class="sxs-lookup"><span data-stu-id="c13aa-114">**From** and **To** – hello date and time range when hello backup set was created.</span></span>

<span data-ttu-id="c13aa-115">hello filtrerade säkerhetskopior visas sedan som en tabell baserad på hello följande attribut:</span><span class="sxs-lookup"><span data-stu-id="c13aa-115">hello filtered backup sets are then tabulated based on hello following attributes:</span></span>

* <span data-ttu-id="c13aa-116">**Namnet** – hello namn på hello säkerhetskopieringsprincip eller volym som är associerade med hello säkerhetskopia.</span><span class="sxs-lookup"><span data-stu-id="c13aa-116">**Name** – hello name of hello backup policy or volume associated with hello backup set.</span></span>
* <span data-ttu-id="c13aa-117">**Storlek** – hello verkliga storleken hos hello säkerhetskopia.</span><span class="sxs-lookup"><span data-stu-id="c13aa-117">**Size** – hello actual size of hello backup set.</span></span>
* <span data-ttu-id="c13aa-118">**Skapas på** – hello datum och tid då hello säkerhetskopior skapades.</span><span class="sxs-lookup"><span data-stu-id="c13aa-118">**Created on** – hello date and time when hello backups were created.</span></span> 
* <span data-ttu-id="c13aa-119">**Typen** – säkerhetskopior kan lokala ögonblicksbilder eller molnbaserade ögonblicksbilder.</span><span class="sxs-lookup"><span data-stu-id="c13aa-119">**Type** – Backup sets can be local snapshots or cloud snapshots.</span></span> <span data-ttu-id="c13aa-120">En lokal ögonblicksbild är en säkerhetskopia av din volymdata som lagras lokalt på hello enhet, medan en ögonblicksbild i molnet refererar toohello säkerhetskopiering av volymdata som finns i molnet hello.</span><span class="sxs-lookup"><span data-stu-id="c13aa-120">A local snapshot is a backup of all your volume data stored locally on hello device, whereas a cloud snapshot refers toohello backup of volume data residing in hello cloud.</span></span> <span data-ttu-id="c13aa-121">Lokala ögonblicksbilder ger snabbare åtkomst medan molnögonblicksbilder väljs för dataåterhämtning.</span><span class="sxs-lookup"><span data-stu-id="c13aa-121">Local snapshots provide faster access, whereas cloud snapshots are chosen for data resiliency.</span></span>
* <span data-ttu-id="c13aa-122">**Initierad av** – hello säkerhetskopieringar kan initieras automatiskt enligt tooa schema eller manuellt av en användare.</span><span class="sxs-lookup"><span data-stu-id="c13aa-122">**Initiated by** – hello backups can be initiated automatically according tooa schedule or manually by a user.</span></span> <span data-ttu-id="c13aa-123">(Du kan använda en princip för säkerhetskopiering tooschedule säkerhetskopior.</span><span class="sxs-lookup"><span data-stu-id="c13aa-123">(You can use a backup policy tooschedule backups.</span></span> <span data-ttu-id="c13aa-124">Du kan också använda hello **ta säkerhetskopia** alternativet tootake en interaktiv säkerhetskopiering.)</span><span class="sxs-lookup"><span data-stu-id="c13aa-124">Alternatively, you can use hello **Take backup** option tootake an interactive backup.)</span></span>

## <a name="how-toorestore-your-storsimple-volume-from-a-backup"></a><span data-ttu-id="c13aa-125">Hur toorestore StorSimple-volym från en säkerhetskopia</span><span class="sxs-lookup"><span data-stu-id="c13aa-125">How toorestore your StorSimple volume from a backup</span></span>
<span data-ttu-id="c13aa-126">Du kan använda hello **säkerhetskopieringskatalogen** sidan toorestore StorSimple-volym från en specifik säkerhetskopia.</span><span class="sxs-lookup"><span data-stu-id="c13aa-126">You can use hello **Backup Catalog** page toorestore your StorSimple volume from a specific backup.</span></span> 

> [!WARNING]
> <span data-ttu-id="c13aa-127">Återställa från en säkerhetskopia ersätter hello befintliga volymer från hello säkerhetskopia.</span><span class="sxs-lookup"><span data-stu-id="c13aa-127">Restoring from a backup will replace hello existing volumes from hello backup.</span></span> <span data-ttu-id="c13aa-128">Detta kan orsaka hello förlust av alla data som har skrivits när hello säkerhetskopian skapades.</span><span class="sxs-lookup"><span data-stu-id="c13aa-128">This may cause hello loss of any data that was written after hello backup was taken.</span></span>
> 
> 

<span data-ttu-id="c13aa-129">Se till att hello volym är offline innan du startar en återställning på en volym.</span><span class="sxs-lookup"><span data-stu-id="c13aa-129">Before you initiate a restore on a volume, ensure that hello volume is offline.</span></span> <span data-ttu-id="c13aa-130">Du behöver tootake hello volym offline på värden för hello först och sedan hello enhet.</span><span class="sxs-lookup"><span data-stu-id="c13aa-130">You will need tootake hello volume offline on hello host first and then hello device.</span></span> <span data-ttu-id="c13aa-131">Gör så hello i [kopplar från en volym](storsimple-manage-volumes.md#take-a-volume-offline).</span><span class="sxs-lookup"><span data-stu-id="c13aa-131">Follow hello steps in [Take a volume offline](storsimple-manage-volumes.md#take-a-volume-offline).</span></span> <span data-ttu-id="c13aa-132">Utför följande steg toorestore en volym från en säkerhetskopia hello.</span><span class="sxs-lookup"><span data-stu-id="c13aa-132">Perform hello following steps toorestore a volume from a backup set.</span></span>

### <a name="toorestore-from-a-backup-set"></a><span data-ttu-id="c13aa-133">toorestore från en säkerhetskopia</span><span class="sxs-lookup"><span data-stu-id="c13aa-133">toorestore from a backup set</span></span>
1. <span data-ttu-id="c13aa-134">På hello StorSimple Manager-tjänsten klickar du på hello **säkerhetskopieringskatalog** fliken.</span><span class="sxs-lookup"><span data-stu-id="c13aa-134">On hello StorSimple Manager service page, click hello **Backup catalog** tab.</span></span>
   
    ![Säkerhetskopieringskatalogen](./media/storsimple-restore-from-backup-set/HCS_Restore.png)
2. <span data-ttu-id="c13aa-136">Välj en säkerhetskopia på följande sätt:</span><span class="sxs-lookup"><span data-stu-id="c13aa-136">Select a backup set as follows:</span></span>
   
   1. <span data-ttu-id="c13aa-137">Välj lämplig hello-enhet.</span><span class="sxs-lookup"><span data-stu-id="c13aa-137">Select hello appropriate device.</span></span>
   2. <span data-ttu-id="c13aa-138">Välj hello volym eller säkerhetskopiering princip för säkerhetskopiering hello tooselect gärna i hello nedrullningsbara listan.</span><span class="sxs-lookup"><span data-stu-id="c13aa-138">In hello drop-down list, choose hello volume or backup policy for hello backup that you wish tooselect.</span></span>
   3. <span data-ttu-id="c13aa-139">Ange hello tidsintervall.</span><span class="sxs-lookup"><span data-stu-id="c13aa-139">Specify hello time range.</span></span>
   4. <span data-ttu-id="c13aa-140">Klicka på kryssikonen hello</span><span class="sxs-lookup"><span data-stu-id="c13aa-140">Click hello check icon</span></span> ![kryssikon](./media/storsimple-restore-from-backup-set/HCS_CheckIcon.png) <span data-ttu-id="c13aa-142">tooexecute den här frågan.</span><span class="sxs-lookup"><span data-stu-id="c13aa-142">tooexecute this query.</span></span>
      
      <span data-ttu-id="c13aa-143">hello ska säkerhetskopior som är associerade med principen för hello markerad volym eller säkerhetskopiering visas i hello lista över säkerhetskopior.</span><span class="sxs-lookup"><span data-stu-id="c13aa-143">hello backups associated with hello selected volume or backup policy should appear in hello list of backup sets.</span></span>
3. <span data-ttu-id="c13aa-144">Expandera hello säkerhetskopia tooview hello associerade volymer.</span><span class="sxs-lookup"><span data-stu-id="c13aa-144">Expand hello backup set tooview hello associated volumes.</span></span> <span data-ttu-id="c13aa-145">Dessa volymer måste vara offline på hello värd och enheten innan du kan återställa dem.</span><span class="sxs-lookup"><span data-stu-id="c13aa-145">These volumes must be taken offline on hello host and device before you can restore them.</span></span> <span data-ttu-id="c13aa-146">Gör så hello i [kopplar från en volym](storsimple-manage-volumes.md#take-a-volume-offline).</span><span class="sxs-lookup"><span data-stu-id="c13aa-146">Follow hello steps in [Take a volume offline](storsimple-manage-volumes.md#take-a-volume-offline).</span></span>
   
   > [!IMPORTANT]
   > <span data-ttu-id="c13aa-147">Kontrollera att du har vidtagit hello volymer offline på värden för hello först innan du utför hello volymer på hello enhet.</span><span class="sxs-lookup"><span data-stu-id="c13aa-147">Make sure that you have taken hello volumes offline on hello host first, before you take hello volumes offline on hello device.</span></span> <span data-ttu-id="c13aa-148">Om du inte vidtar hello volymer offline på värden för hello leda potentiellt toodata skadas.</span><span class="sxs-lookup"><span data-stu-id="c13aa-148">If you do not take hello volumes offline on hello host, it could potentially lead toodata corruption.</span></span>
   > 
   > 
4. <span data-ttu-id="c13aa-149">Välj en säkerhetskopia.</span><span class="sxs-lookup"><span data-stu-id="c13aa-149">Select a backup set.</span></span> <span data-ttu-id="c13aa-150">Klicka på **återställa** på hello hello sidans nederkant.</span><span class="sxs-lookup"><span data-stu-id="c13aa-150">Click **Restore** at hello bottom of hello page.</span></span>
5. <span data-ttu-id="c13aa-151">Du uppmanas att bekräfta.</span><span class="sxs-lookup"><span data-stu-id="c13aa-151">You will be prompted for confirmation.</span></span> 
   
    ![Bekräftelsesida](./media/storsimple-restore-from-backup-set/HCS_ConfirmRestore.png)
6. <span data-ttu-id="c13aa-153">Granska hello återställning information och klicka på kryssikonen hello ![kryssikon](./media/storsimple-restore-from-backup-set/HCS_CheckIcon.png).</span><span class="sxs-lookup"><span data-stu-id="c13aa-153">Review hello restore information and click hello check icon ![check icon](./media/storsimple-restore-from-backup-set/HCS_CheckIcon.png).</span></span> <span data-ttu-id="c13aa-154">Detta kommer att initiera en återställningsjobbet som du kan visa genom att öppna hello **jobb** sidan.</span><span class="sxs-lookup"><span data-stu-id="c13aa-154">This will initiate a restore job that you can view by accessing hello **Jobs** page.</span></span> 
7. <span data-ttu-id="c13aa-155">När hello återställningen är slutförd, kan du kontrollera att hello innehållet i volymerna som ersätts av volymer från hello säkerhetskopia.</span><span class="sxs-lookup"><span data-stu-id="c13aa-155">After hello restore is complete, you can verify that hello contents of your volumes are replaced by volumes from hello backup.</span></span>

<span data-ttu-id="c13aa-156">![Video tillgänglig](./media/storsimple-restore-from-backup-set/Video_icon.png) **Video tillgänglig**</span><span class="sxs-lookup"><span data-stu-id="c13aa-156">![Video available](./media/storsimple-restore-from-backup-set/Video_icon.png) **Video available**</span></span>

<span data-ttu-id="c13aa-157">toowatch en video som visar hur du kan använda hello klona och återställa funktioner i StorSimple toorecover borttagna filer, klickar du på [här](https://azure.microsoft.com/documentation/videos/storsimple-recover-deleted-files-with-storsimple/).</span><span class="sxs-lookup"><span data-stu-id="c13aa-157">toowatch a video that demonstrates how you can use hello clone and restore features in StorSimple toorecover deleted files, click [here](https://azure.microsoft.com/documentation/videos/storsimple-recover-deleted-files-with-storsimple/).</span></span>

## <a name="next-steps"></a><span data-ttu-id="c13aa-158">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="c13aa-158">Next steps</span></span>
* <span data-ttu-id="c13aa-159">Lär dig hur för[hantera StorSimple-volymer](storsimple-manage-volumes.md).</span><span class="sxs-lookup"><span data-stu-id="c13aa-159">Learn how too[Manage StorSimple volumes](storsimple-manage-volumes.md).</span></span>
* <span data-ttu-id="c13aa-160">Lär dig hur för[Använd hello StorSimple Manager service tooadminister StorSimple-enheten](storsimple-manager-service-administration.md).</span><span class="sxs-lookup"><span data-stu-id="c13aa-160">Learn how too[use hello StorSimple Manager service tooadminister your StorSimple device](storsimple-manager-service-administration.md).</span></span>

