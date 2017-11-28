---
title: "aaaManage StorSimple säkerhetskopiering katalogen | Microsoft Docs"
description: "Förklarar hur toouse hello StorSimple Manager-tjänsten säkerhetskopieringskatalogen sidan toolist markerar och tar bort säkerhetskopior för en volym."
services: storsimple
documentationcenter: NA
author: SharS
manager: carmonm
editor: 
ms.assetid: ad81bee9-fe43-40b3-a384-b15fb274ecd9
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 04/28/2016
ms.author: v-sharos
ms.openlocfilehash: 14f565c174a10da2c9e2f934a533a5e493f77226
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="use-hello-storsimple-manager-service-toomanage-your-backup-catalog"></a><span data-ttu-id="347d2-103">Använd hello StorSimple Manager-tjänsten toomanage katalogen för säkerhetskopieringar</span><span class="sxs-lookup"><span data-stu-id="347d2-103">Use hello StorSimple Manager service toomanage your backup catalog</span></span>
## <a name="overview"></a><span data-ttu-id="347d2-104">Översikt</span><span class="sxs-lookup"><span data-stu-id="347d2-104">Overview</span></span>
<span data-ttu-id="347d2-105">Hej StorSimple Manager-tjänsten **säkerhetskopieringskatalogen** sidan visas alla hello säkerhetskopior som skapas vid manuell eller schemalagda säkerhetskopieringar.</span><span class="sxs-lookup"><span data-stu-id="347d2-105">hello StorSimple Manager service **Backup Catalog** page displays all hello backup sets that are created when manual or scheduled backups are taken.</span></span> <span data-ttu-id="347d2-106">Du kan använda den här sidan toolist alla hello säkerhetskopior för en princip för säkerhetskopiering eller en volym, Välj eller ta bort eller använda en säkerhetskopiering toorestore eller klona en volym.</span><span class="sxs-lookup"><span data-stu-id="347d2-106">You can use this page toolist all hello backups for a backup policy or a volume, select or delete backups, or use a backup toorestore or clone a volume.</span></span>

<span data-ttu-id="347d2-107">Den här självstudiekursen beskrivs hur toolist markerar och ta bort en säkerhetskopia.</span><span class="sxs-lookup"><span data-stu-id="347d2-107">This tutorial explains how toolist, select, and delete a backup set.</span></span> <span data-ttu-id="347d2-108">toolearn hur toorestore enheten från en säkerhetskopia, gå för[återställa enheten från en säkerhetskopia](storsimple-restore-from-backup-set.md).</span><span class="sxs-lookup"><span data-stu-id="347d2-108">toolearn how toorestore your device from backup, go too[Restore your device from a backup set](storsimple-restore-from-backup-set.md).</span></span> <span data-ttu-id="347d2-109">hur tooclone en volym, gå för toolearn[klona en StorSimple-volym](storsimple-clone-volume.md).</span><span class="sxs-lookup"><span data-stu-id="347d2-109">toolearn how tooclone a volume, go too[Clone a StorSimple volume](storsimple-clone-volume.md).</span></span>

![Säkerhetskopieringskatalogen](./media/storsimple-manage-backup-catalog/backupcatalog.png) 

<span data-ttu-id="347d2-111">Hej **säkerhetskopieringskatalogen** innehåller en fråga toonarrow valet av säkerhetskopian.</span><span class="sxs-lookup"><span data-stu-id="347d2-111">hello **Backup Catalog** page provides a query toonarrow your backup set selection.</span></span> <span data-ttu-id="347d2-112">Du kan filtrera hello säkerhetskopior som hämtas, baserat på hello följande parametrar:</span><span class="sxs-lookup"><span data-stu-id="347d2-112">You can filter hello backup sets that are retrieved, based on hello following parameters:</span></span>

* <span data-ttu-id="347d2-113">**Enheten** – hello enhet på vilken hello säkerhetskopian skapades.</span><span class="sxs-lookup"><span data-stu-id="347d2-113">**Device** – hello device on which hello backup set was created.</span></span>
* <span data-ttu-id="347d2-114">**Säkerhetskopiera princip- eller** – hello säkerhetskopieringsprincip eller volym som är associerade med den här säkerhetskopian.</span><span class="sxs-lookup"><span data-stu-id="347d2-114">**Backup Policy or Volume** – hello backup policy or volume associated with this backup set.</span></span>
* <span data-ttu-id="347d2-115">**Från och till** – hello intervallet för datum och tid när hello säkerhetskopian skapades.</span><span class="sxs-lookup"><span data-stu-id="347d2-115">**From and To** – hello date and time range when hello backup set was created.</span></span>

<span data-ttu-id="347d2-116">hello filtrerade säkerhetskopior visas sedan som en tabell baserad på hello följande attribut:</span><span class="sxs-lookup"><span data-stu-id="347d2-116">hello filtered backup sets are then tabulated based on hello following attributes:</span></span>

* <span data-ttu-id="347d2-117">**Namnet** – hello namn på hello säkerhetskopieringsprincip eller volym som är associerade med hello säkerhetskopia.</span><span class="sxs-lookup"><span data-stu-id="347d2-117">**Name** – hello name of hello backup policy or volume associated with hello backup set.</span></span>
* <span data-ttu-id="347d2-118">**Storlek** – hello verkliga storleken hos hello säkerhetskopia.</span><span class="sxs-lookup"><span data-stu-id="347d2-118">**Size** – hello actual size of hello backup set.</span></span>
* <span data-ttu-id="347d2-119">**Skapad den** – hello datum och tid då hello säkerhetskopior skapades.</span><span class="sxs-lookup"><span data-stu-id="347d2-119">**Created On** – hello date and time when hello backups were created.</span></span> 
* <span data-ttu-id="347d2-120">**Typen** – säkerhetskopior kan lokala ögonblicksbilder eller molnbaserade ögonblicksbilder.</span><span class="sxs-lookup"><span data-stu-id="347d2-120">**Type** – Backup sets can be local snapshots or cloud snapshots.</span></span> <span data-ttu-id="347d2-121">En lokal ögonblicksbild är en säkerhetskopia av din volymdata som lagras lokalt på hello enhet, medan en ögonblicksbild i molnet refererar toohello säkerhetskopiering av volymdata som finns i molnet hello.</span><span class="sxs-lookup"><span data-stu-id="347d2-121">A local snapshot is a backup of all your volume data stored locally on hello device, whereas a cloud snapshot refers toohello backup of volume data residing in hello cloud.</span></span> <span data-ttu-id="347d2-122">Lokala ögonblicksbilder ger snabbare åtkomst medan molnögonblicksbilder väljs för dataåterhämtning.</span><span class="sxs-lookup"><span data-stu-id="347d2-122">Local snapshots provide faster access, whereas cloud snapshots are chosen for data resiliency.</span></span>
* <span data-ttu-id="347d2-123">**Initierades av** – hello säkerhetskopieringar kan initieras automatiskt av ett schema eller manuellt av en användare.</span><span class="sxs-lookup"><span data-stu-id="347d2-123">**Initiated By** – hello backups can be initiated automatically by a schedule or manually by a user.</span></span> <span data-ttu-id="347d2-124">Du kan använda en princip för säkerhetskopiering tooschedule säkerhetskopior.</span><span class="sxs-lookup"><span data-stu-id="347d2-124">You can use a backup policy tooschedule backups.</span></span> <span data-ttu-id="347d2-125">Du kan också använda hello **ta säkerhetskopia** alternativet tootake en manuell säkerhetskopiering.</span><span class="sxs-lookup"><span data-stu-id="347d2-125">Alternatively, you can use hello **Take backup** option tootake a manual backup.</span></span>

## <a name="list-backup-sets-for-a-volume"></a><span data-ttu-id="347d2-126">Lista över säkerhetskopior för en volym</span><span class="sxs-lookup"><span data-stu-id="347d2-126">List backup sets for a volume</span></span>
<span data-ttu-id="347d2-127">Slutför följande steg toolist hello alla hello säkerhetskopior för en volym.</span><span class="sxs-lookup"><span data-stu-id="347d2-127">Complete hello following steps toolist all hello backups for a volume.</span></span>

#### <a name="toolist-backup-sets"></a><span data-ttu-id="347d2-128">toolist säkerhetskopior</span><span class="sxs-lookup"><span data-stu-id="347d2-128">toolist backup sets</span></span>
1. <span data-ttu-id="347d2-129">På hello StorSimple Manager-tjänsten klickar du på hello **säkerhetskopieringskatalog** fliken.</span><span class="sxs-lookup"><span data-stu-id="347d2-129">On hello StorSimple Manager service page, click hello **Backup catalog** tab.</span></span>
2. <span data-ttu-id="347d2-130">Filtrera hello val på följande sätt:</span><span class="sxs-lookup"><span data-stu-id="347d2-130">Filter hello selections as follows:</span></span>
   
   1. <span data-ttu-id="347d2-131">Välj lämplig hello-enhet.</span><span class="sxs-lookup"><span data-stu-id="347d2-131">Select hello appropriate device.</span></span>
   2. <span data-ttu-id="347d2-132">Välj en volym tooview hello motsvarande hello säkerhetskopieringar i hello nedrullningsbara listan.</span><span class="sxs-lookup"><span data-stu-id="347d2-132">In hello drop-down list, choose a volume tooview hello corresponding hello backups.</span></span>
   3. <span data-ttu-id="347d2-133">Ange hello tidsintervall.</span><span class="sxs-lookup"><span data-stu-id="347d2-133">Specify hello time range.</span></span>
   4. <span data-ttu-id="347d2-134">Klicka på kryssikonen hello</span><span class="sxs-lookup"><span data-stu-id="347d2-134">Click hello check icon</span></span> ![Kryssikon](./media/storsimple-manage-backup-catalog/HCS_CheckIcon.png) <span data-ttu-id="347d2-136">tooexecute den här frågan.</span><span class="sxs-lookup"><span data-stu-id="347d2-136">tooexecute this query.</span></span>
      
      <span data-ttu-id="347d2-137">hello säkerhetskopior som är associerade med hello valda volymen ska visas i hello lista över säkerhetskopior.</span><span class="sxs-lookup"><span data-stu-id="347d2-137">hello backups associated with hello selected volume should appear in hello list of backup sets.</span></span>

## <a name="select-a-backup-set"></a><span data-ttu-id="347d2-138">Välj en säkerhetskopia</span><span class="sxs-lookup"><span data-stu-id="347d2-138">Select a backup set</span></span>
<span data-ttu-id="347d2-139">Slutför hello följande steg tooselect en säkerhetskopia av en princip för volym eller säkerhetskopiering.</span><span class="sxs-lookup"><span data-stu-id="347d2-139">Complete hello following steps tooselect a backup set for a volume or backup policy.</span></span>

#### <a name="tooselect-a-backup-set"></a><span data-ttu-id="347d2-140">tooselect en säkerhetskopia</span><span class="sxs-lookup"><span data-stu-id="347d2-140">tooselect a backup set</span></span>
1. <span data-ttu-id="347d2-141">På hello StorSimple Manager-tjänsten klickar du på hello **säkerhetskopieringskatalog** fliken.</span><span class="sxs-lookup"><span data-stu-id="347d2-141">On hello StorSimple Manager service page, click hello **Backup catalog** tab.</span></span>
2. <span data-ttu-id="347d2-142">Filtrera hello val på följande sätt:</span><span class="sxs-lookup"><span data-stu-id="347d2-142">Filter hello selections as follows:</span></span>
   
   1. <span data-ttu-id="347d2-143">Välj lämplig hello-enhet.</span><span class="sxs-lookup"><span data-stu-id="347d2-143">Select hello appropriate device.</span></span>
   2. <span data-ttu-id="347d2-144">Välj hello volym eller säkerhetskopiering princip för säkerhetskopiering hello tooselect gärna i hello nedrullningsbara listan.</span><span class="sxs-lookup"><span data-stu-id="347d2-144">In hello drop-down list, choose hello volume or backup policy for hello backup that you wish tooselect.</span></span>
   3. <span data-ttu-id="347d2-145">Ange hello tidsintervall.</span><span class="sxs-lookup"><span data-stu-id="347d2-145">Specify hello time range.</span></span>
   4. <span data-ttu-id="347d2-146">Klicka på kryssikonen hello</span><span class="sxs-lookup"><span data-stu-id="347d2-146">Click hello check icon</span></span> ![Kryssikon](./media/storsimple-manage-backup-catalog/HCS_CheckIcon.png) <span data-ttu-id="347d2-148">tooexecute den här frågan.</span><span class="sxs-lookup"><span data-stu-id="347d2-148">tooexecute this query.</span></span>
      
      <span data-ttu-id="347d2-149">hello ska säkerhetskopior som är associerade med principen för hello markerad volym eller säkerhetskopiering visas i hello lista över säkerhetskopior.</span><span class="sxs-lookup"><span data-stu-id="347d2-149">hello backups associated with hello selected volume or backup policy should appear in hello list of backup sets.</span></span>
3. <span data-ttu-id="347d2-150">Markera och utöka en säkerhetskopia.</span><span class="sxs-lookup"><span data-stu-id="347d2-150">Select and expand a backup set.</span></span> <span data-ttu-id="347d2-151">Hej **återställa** och **ta bort** alternativ visas på hello hello sidans nederkant.</span><span class="sxs-lookup"><span data-stu-id="347d2-151">hello **Restore** and **Delete** options are displayed at hello bottom of hello page.</span></span> <span data-ttu-id="347d2-152">Du kan utföra någon av dessa åtgärder på hello säkerhetskopia som du har valt.</span><span class="sxs-lookup"><span data-stu-id="347d2-152">You can perform either of these actions on hello backup set that you selected.</span></span>

## <a name="delete-a-backup-set"></a><span data-ttu-id="347d2-153">Ta bort en säkerhetskopia</span><span class="sxs-lookup"><span data-stu-id="347d2-153">Delete a backup set</span></span>
<span data-ttu-id="347d2-154">Ta bort en säkerhetskopiering när du inte längre vill tooretain hello data som är associerade med den.</span><span class="sxs-lookup"><span data-stu-id="347d2-154">Delete a backup when you no longer wish tooretain hello data associated with it.</span></span> <span data-ttu-id="347d2-155">Utföra hello följande steg toodelete en säkerhetskopia.</span><span class="sxs-lookup"><span data-stu-id="347d2-155">Perform hello following steps toodelete a backup set.</span></span>

#### <a name="toodelete-a-backup-set"></a><span data-ttu-id="347d2-156">toodelete en säkerhetskopia</span><span class="sxs-lookup"><span data-stu-id="347d2-156">toodelete a backup set</span></span>
1. <span data-ttu-id="347d2-157">På hello StorSimple Manager-tjänsten klickar du på hello **säkerhetskopieringskatalog fliken**.</span><span class="sxs-lookup"><span data-stu-id="347d2-157">On hello StorSimple Manager service page, click hello **Backup Catalog tab**.</span></span>
2. <span data-ttu-id="347d2-158">Filtrera hello val på följande sätt:</span><span class="sxs-lookup"><span data-stu-id="347d2-158">Filter hello selections as follows:</span></span>
   
   1. <span data-ttu-id="347d2-159">Välj lämplig hello-enhet.</span><span class="sxs-lookup"><span data-stu-id="347d2-159">Select hello appropriate device.</span></span>
   2. <span data-ttu-id="347d2-160">Välj hello volym eller säkerhetskopiering princip för säkerhetskopiering hello tooselect gärna i hello nedrullningsbara listan.</span><span class="sxs-lookup"><span data-stu-id="347d2-160">In hello drop-down list, choose hello volume or backup policy for hello backup that you wish tooselect.</span></span>
   3. <span data-ttu-id="347d2-161">Ange hello tidsintervall.</span><span class="sxs-lookup"><span data-stu-id="347d2-161">Specify hello time range.</span></span>
   4. <span data-ttu-id="347d2-162">Klicka på kryssikonen hello</span><span class="sxs-lookup"><span data-stu-id="347d2-162">Click hello check icon</span></span> ![Kryssikon](./media/storsimple-manage-backup-catalog/HCS_CheckIcon.png) <span data-ttu-id="347d2-164">tooexecute den här frågan.</span><span class="sxs-lookup"><span data-stu-id="347d2-164">tooexecute this query.</span></span>
      
      <span data-ttu-id="347d2-165">hello ska säkerhetskopior som är associerade med principen för hello markerad volym eller säkerhetskopiering visas i hello lista över säkerhetskopior.</span><span class="sxs-lookup"><span data-stu-id="347d2-165">hello backups associated with hello selected volume or backup policy should appear in hello list of backup sets.</span></span>
3. <span data-ttu-id="347d2-166">Markera och utöka en säkerhetskopia.</span><span class="sxs-lookup"><span data-stu-id="347d2-166">Select and expand a backup set.</span></span> <span data-ttu-id="347d2-167">Hej **återställa** och **ta bort** alternativ visas på hello hello sidans nederkant.</span><span class="sxs-lookup"><span data-stu-id="347d2-167">hello **Restore** and **Delete** options are displayed at hello bottom of hello page.</span></span> <span data-ttu-id="347d2-168">Klicka på **Ta bort**.</span><span class="sxs-lookup"><span data-stu-id="347d2-168">Click **Delete**.</span></span>
4. <span data-ttu-id="347d2-169">Du meddelas när hello borttagning pågår och när den har slutförts.</span><span class="sxs-lookup"><span data-stu-id="347d2-169">You will be notified when hello deletion is in progress and when it has successfully finished.</span></span> <span data-ttu-id="347d2-170">Uppdatera hello frågan på den här sidan när hello borttagningen är klar.</span><span class="sxs-lookup"><span data-stu-id="347d2-170">After hello deletion is done, refresh hello query on this page.</span></span> <span data-ttu-id="347d2-171">hello bort säkerhetskopia visas inte längre i hello lista över säkerhetskopior.</span><span class="sxs-lookup"><span data-stu-id="347d2-171">hello deleted backup set will no longer appear in hello list of backup sets.</span></span>

## <a name="next-steps"></a><span data-ttu-id="347d2-172">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="347d2-172">Next steps</span></span>
* <span data-ttu-id="347d2-173">Lär dig hur för[Använd hello säkerhetskopieringskatalogen toorestore enheten från en säkerhetskopia](storsimple-restore-from-backup-set.md).</span><span class="sxs-lookup"><span data-stu-id="347d2-173">Learn how too[use hello backup catalog toorestore your device from a backup set](storsimple-restore-from-backup-set.md).</span></span>
* <span data-ttu-id="347d2-174">Lär dig hur för[Använd hello StorSimple Manager service tooadminister StorSimple-enheten](storsimple-manager-service-administration.md).</span><span class="sxs-lookup"><span data-stu-id="347d2-174">Learn how too[use hello StorSimple Manager service tooadminister your StorSimple device](storsimple-manager-service-administration.md).</span></span>

