---
title: "aaaManage StorSimple säkerhetskopiering katalogen | Microsoft Docs"
description: "Förklarar hur toouse hello StorSimple Enhetshanteraren service säkerhetskopieringskatalogen sidan toolist markerar och tar bort säkerhetskopior."
services: storsimple
documentationcenter: NA
author: alkohli
manager: timlt
editor: 
ms.assetid: 
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 06/29/2017
ms.author: alkohli
ms.openlocfilehash: e464609e74409a06a198790719abd82ed03c03d9
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="use-hello-storsimple-device-manager-service-toomanage-your-backup-catalog"></a><span data-ttu-id="f6933-103">Använd hello StorSimple Enhetshanteraren service toomanage katalogen för säkerhetskopieringar</span><span class="sxs-lookup"><span data-stu-id="f6933-103">Use hello StorSimple Device Manager service toomanage your backup catalog</span></span>
## <a name="overview"></a><span data-ttu-id="f6933-104">Översikt</span><span class="sxs-lookup"><span data-stu-id="f6933-104">Overview</span></span>
<span data-ttu-id="f6933-105">Hej StorSimple Enhetshanteraren service **säkerhetskopieringskatalogen** bladet visar alla hello säkerhetskopior som skapas vid manuell eller schemalagda säkerhetskopieringar.</span><span class="sxs-lookup"><span data-stu-id="f6933-105">hello StorSimple Device Manager service **Backup Catalog** blade displays all hello backup sets that are created when manual or scheduled backups are taken.</span></span> <span data-ttu-id="f6933-106">Du kan använda den här sidan toolist alla hello säkerhetskopior för en princip för säkerhetskopiering eller en volym, Välj eller ta bort eller använda en säkerhetskopiering toorestore eller klona en volym.</span><span class="sxs-lookup"><span data-stu-id="f6933-106">You can use this page toolist all hello backups for a backup policy or a volume, select or delete backups, or use a backup toorestore or clone a volume.</span></span>

<span data-ttu-id="f6933-107">Den här självstudiekursen beskrivs hur toolist markerar och ta bort en säkerhetskopia.</span><span class="sxs-lookup"><span data-stu-id="f6933-107">This tutorial explains how toolist, select, and delete a backup set.</span></span> <span data-ttu-id="f6933-108">toolearn hur toorestore enheten från en säkerhetskopia, gå för[återställa enheten från en säkerhetskopia](storsimple-8000-restore-from-backup-set-u2.md).</span><span class="sxs-lookup"><span data-stu-id="f6933-108">toolearn how toorestore your device from backup, go too[Restore your device from a backup set](storsimple-8000-restore-from-backup-set-u2.md).</span></span> <span data-ttu-id="f6933-109">hur tooclone en volym, gå för toolearn[klona en StorSimple-volym](storsimple-8000-clone-volume-u2.md).</span><span class="sxs-lookup"><span data-stu-id="f6933-109">toolearn how tooclone a volume, go too[Clone a StorSimple volume](storsimple-8000-clone-volume-u2.md).</span></span>

![Säkerhetskopieringskatalogen](./media/storsimple-8000-manage-backup-catalog/bucatalog.png) 

<span data-ttu-id="f6933-111">Hej **säkerhetskopieringskatalogen** bladet innehåller en fråga toonarrow valet av säkerhetskopian.</span><span class="sxs-lookup"><span data-stu-id="f6933-111">hello **Backup Catalog** blade provides a query toonarrow your backup set selection.</span></span> <span data-ttu-id="f6933-112">Du kan filtrera hello säkerhetskopior som hämtas, baserat på hello följande parametrar:</span><span class="sxs-lookup"><span data-stu-id="f6933-112">You can filter hello backup sets that are retrieved, based on hello following parameters:</span></span>

* <span data-ttu-id="f6933-113">**Enheten** – hello enhet på vilken hello säkerhetskopian skapades.</span><span class="sxs-lookup"><span data-stu-id="f6933-113">**Device** – hello device on which hello backup set was created.</span></span>
* <span data-ttu-id="f6933-114">**Princip för säkerhetskopiering eller volym** – hello säkerhetskopieringsprincip eller volym som är associerade med den här säkerhetskopian.</span><span class="sxs-lookup"><span data-stu-id="f6933-114">**Backup policy or Volume** – hello backup policy or volume associated with this backup set.</span></span>
* <span data-ttu-id="f6933-115">**Från och till** – hello intervallet för datum och tid när hello säkerhetskopian skapades.</span><span class="sxs-lookup"><span data-stu-id="f6933-115">**From and To** – hello date and time range when hello backup set was created.</span></span>

<span data-ttu-id="f6933-116">hello filtrerade säkerhetskopior visas sedan som en tabell baserad på hello följande attribut:</span><span class="sxs-lookup"><span data-stu-id="f6933-116">hello filtered backup sets are then tabulated based on hello following attributes:</span></span>

* <span data-ttu-id="f6933-117">**Namnet** – hello namn på hello säkerhetskopieringsprincip eller volym som är associerade med hello säkerhetskopia.</span><span class="sxs-lookup"><span data-stu-id="f6933-117">**Name** – hello name of hello backup policy or volume associated with hello backup set.</span></span>
* <span data-ttu-id="f6933-118">**Storlek** – hello verkliga storleken hos hello säkerhetskopia.</span><span class="sxs-lookup"><span data-stu-id="f6933-118">**Size** – hello actual size of hello backup set.</span></span>
* <span data-ttu-id="f6933-119">**Skapad den** – hello datum och tid då hello säkerhetskopior skapades.</span><span class="sxs-lookup"><span data-stu-id="f6933-119">**Created On** – hello date and time when hello backups were created.</span></span> 
* <span data-ttu-id="f6933-120">**Typen** – säkerhetskopior kan lokala ögonblicksbilder eller molnbaserade ögonblicksbilder.</span><span class="sxs-lookup"><span data-stu-id="f6933-120">**Type** – Backup sets can be local snapshots or cloud snapshots.</span></span> <span data-ttu-id="f6933-121">En lokal ögonblicksbild är en säkerhetskopia av din volymdata som lagras lokalt på hello enhet, medan en ögonblicksbild i molnet refererar toohello säkerhetskopiering av volymdata som finns i molnet hello.</span><span class="sxs-lookup"><span data-stu-id="f6933-121">A local snapshot is a backup of all your volume data stored locally on hello device, whereas a cloud snapshot refers toohello backup of volume data residing in hello cloud.</span></span> <span data-ttu-id="f6933-122">Lokala ögonblicksbilder ger snabbare åtkomst medan molnögonblicksbilder väljs för dataåterhämtning.</span><span class="sxs-lookup"><span data-stu-id="f6933-122">Local snapshots provide faster access, whereas cloud snapshots are chosen for data resiliency.</span></span>
* <span data-ttu-id="f6933-123">**Initierades av** – hello säkerhetskopieringar kan initieras automatiskt av ett schema eller manuellt av en användare.</span><span class="sxs-lookup"><span data-stu-id="f6933-123">**Initiated By** – hello backups can be initiated automatically by a schedule or manually by a user.</span></span> <span data-ttu-id="f6933-124">Du kan använda en princip för säkerhetskopiering tooschedule säkerhetskopior.</span><span class="sxs-lookup"><span data-stu-id="f6933-124">You can use a backup policy tooschedule backups.</span></span> <span data-ttu-id="f6933-125">Du kan också använda hello **ta säkerhetskopia** alternativet tootake en manuell säkerhetskopiering.</span><span class="sxs-lookup"><span data-stu-id="f6933-125">Alternatively, you can use hello **Take backup** option tootake a manual backup.</span></span>

## <a name="list-backup-sets-for-a-backup-policy"></a><span data-ttu-id="f6933-126">Lista över säkerhetskopior för en princip för säkerhetskopiering</span><span class="sxs-lookup"><span data-stu-id="f6933-126">List backup sets for a backup policy</span></span>
<span data-ttu-id="f6933-127">Slutför följande steg toolist hello alla hello säkerhetskopieringar för en princip för säkerhetskopiering.</span><span class="sxs-lookup"><span data-stu-id="f6933-127">Complete hello following steps toolist all hello backups for a backup policy.</span></span>

#### <a name="toolist-backup-sets"></a><span data-ttu-id="f6933-128">toolist säkerhetskopior</span><span class="sxs-lookup"><span data-stu-id="f6933-128">toolist backup sets</span></span>
1. <span data-ttu-id="f6933-129">Gå tooyour StorSimple enheten Manager-tjänsten och klicka på **säkerhetskopieringskatalog**.</span><span class="sxs-lookup"><span data-stu-id="f6933-129">Go tooyour StorSimple Device Manager service and click **Backup catalog**.</span></span>

2. <span data-ttu-id="f6933-130">Filtrera hello val på följande sätt:</span><span class="sxs-lookup"><span data-stu-id="f6933-130">Filter hello selections as follows:</span></span>
   
   1. <span data-ttu-id="f6933-131">Ange hello tidsintervall.</span><span class="sxs-lookup"><span data-stu-id="f6933-131">Specify hello time range.</span></span>
   2. <span data-ttu-id="f6933-132">Välj lämplig hello-enhet.</span><span class="sxs-lookup"><span data-stu-id="f6933-132">Select hello appropriate device.</span></span>
   3. <span data-ttu-id="f6933-133">Filtrera efter **säkerhetskopiera princip** tooview hello motsvarande hello säkerhetskopieringar.</span><span class="sxs-lookup"><span data-stu-id="f6933-133">Filter by **Backup policy** tooview hello corresponding hello backups.</span></span>
   3. <span data-ttu-id="f6933-134">Hello säkerhetskopieringsprincip listrutan, Välj **alla** tooview alla hello säkerhetskopieringar på hello valda enheten.</span><span class="sxs-lookup"><span data-stu-id="f6933-134">From hello backup policy dropdown list, choose **All** tooview all hello backups on hello selected device.</span></span>
   4. <span data-ttu-id="f6933-135">Klicka på **tillämpa** tooexecute den här frågan.</span><span class="sxs-lookup"><span data-stu-id="f6933-135">Click **Apply** tooexecute this query.</span></span>
      
      <span data-ttu-id="f6933-136">hello säkerhetskopior som är associerade med hello valt säkerhetskopieringsprincip ska visas i hello lista över säkerhetskopior.</span><span class="sxs-lookup"><span data-stu-id="f6933-136">hello backups associated with hello selected backup policy should appear in hello list of backup sets.</span></span>

      ![Gå toobackup katalog](./media/storsimple-8000-manage-backup-catalog/bucatalog1.png)

## <a name="select-a-backup-set"></a><span data-ttu-id="f6933-138">Välj en säkerhetskopia</span><span class="sxs-lookup"><span data-stu-id="f6933-138">Select a backup set</span></span>
<span data-ttu-id="f6933-139">Slutför hello följande steg tooselect en säkerhetskopia av en princip för volym eller säkerhetskopiering.</span><span class="sxs-lookup"><span data-stu-id="f6933-139">Complete hello following steps tooselect a backup set for a volume or backup policy.</span></span>

#### <a name="tooselect-a-backup-set"></a><span data-ttu-id="f6933-140">tooselect en säkerhetskopia</span><span class="sxs-lookup"><span data-stu-id="f6933-140">tooselect a backup set</span></span>
1. <span data-ttu-id="f6933-141">Gå tooyour StorSimple enheten Manager-tjänsten och klicka på **säkerhetskopieringskatalog**.</span><span class="sxs-lookup"><span data-stu-id="f6933-141">Go tooyour StorSimple Device Manager service and click **Backup catalog**.</span></span>
2. <span data-ttu-id="f6933-142">Filtrera hello val på följande sätt:</span><span class="sxs-lookup"><span data-stu-id="f6933-142">Filter hello selections as follows:</span></span>
   
   1. <span data-ttu-id="f6933-143">Ange hello tidsintervall.</span><span class="sxs-lookup"><span data-stu-id="f6933-143">Specify hello time range.</span></span> 
   2. <span data-ttu-id="f6933-144">Välj lämplig hello-enhet.</span><span class="sxs-lookup"><span data-stu-id="f6933-144">Select hello appropriate device.</span></span> 
   3. <span data-ttu-id="f6933-145">Filtrera efter volym eller säkerhetskopiering princip för hello säkerhetskopiering tooselect gärna.</span><span class="sxs-lookup"><span data-stu-id="f6933-145">Filter by volume or backup policy for hello backup that you wish tooselect.</span></span>
   4. <span data-ttu-id="f6933-146">Klicka på **tillämpa** tooexecute den här frågan.</span><span class="sxs-lookup"><span data-stu-id="f6933-146">Click **Apply** tooexecute this query.</span></span>
      
      <span data-ttu-id="f6933-147">hello ska säkerhetskopior som är associerade med principen för hello markerad volym eller säkerhetskopiering visas i hello lista över säkerhetskopior.</span><span class="sxs-lookup"><span data-stu-id="f6933-147">hello backups associated with hello selected volume or backup policy should appear in hello list of backup sets.</span></span>

      ![Gå toobackup katalog](./media/storsimple-8000-manage-backup-catalog/bucatalog1.png)

3. <span data-ttu-id="f6933-149">Markera och utöka en säkerhetskopia.</span><span class="sxs-lookup"><span data-stu-id="f6933-149">Select and expand a backup set.</span></span> <span data-ttu-id="f6933-150">Du kan nu se hello säkerhetskopior uppdelat efter hello volymer som den innehåller.</span><span class="sxs-lookup"><span data-stu-id="f6933-150">You can now see hello backup sets broken down by hello volumes that it contains.</span></span> <span data-ttu-id="f6933-151">Hej **återställa** och **ta bort** alternativ är tillgängliga via hello snabbmeny (högerklicka) för hello säkerhetskopian.</span><span class="sxs-lookup"><span data-stu-id="f6933-151">hello **Restore** and **Delete** options are available via hello context menu (right-click) for hello backup set.</span></span> <span data-ttu-id="f6933-152">Du kan utföra någon av dessa åtgärder på hello säkerhetskopia som du har valt.</span><span class="sxs-lookup"><span data-stu-id="f6933-152">You can perform either of these actions on hello backup set that you selected.</span></span>

    ![Gå toobackup katalog](./media/storsimple-8000-manage-backup-catalog/bucatalog2.png)

## <a name="delete-a-backup-set"></a><span data-ttu-id="f6933-154">Ta bort en säkerhetskopia</span><span class="sxs-lookup"><span data-stu-id="f6933-154">Delete a backup set</span></span>
<span data-ttu-id="f6933-155">Ta bort en säkerhetskopiering när du inte längre vill tooretain hello data som är associerade med den.</span><span class="sxs-lookup"><span data-stu-id="f6933-155">Delete a backup when you no longer wish tooretain hello data associated with it.</span></span> <span data-ttu-id="f6933-156">Utföra hello följande steg toodelete en säkerhetskopia.</span><span class="sxs-lookup"><span data-stu-id="f6933-156">Perform hello following steps toodelete a backup set.</span></span>

#### <a name="toodelete-a-backup-set"></a><span data-ttu-id="f6933-157">toodelete en säkerhetskopia</span><span class="sxs-lookup"><span data-stu-id="f6933-157">toodelete a backup set</span></span>
 <span data-ttu-id="f6933-158">Gå tooyour StorSimple enheten Manager-tjänsten och klicka på **säkerhetskopieringskatalog**.</span><span class="sxs-lookup"><span data-stu-id="f6933-158">Go tooyour StorSimple Device Manager service and click **Backup catalog**.</span></span>
2. <span data-ttu-id="f6933-159">Filtrera hello val på följande sätt:</span><span class="sxs-lookup"><span data-stu-id="f6933-159">Filter hello selections as follows:</span></span>
   
   1. <span data-ttu-id="f6933-160">Ange hello tidsintervall.</span><span class="sxs-lookup"><span data-stu-id="f6933-160">Specify hello time range.</span></span> 
   2. <span data-ttu-id="f6933-161">Välj lämplig hello-enhet.</span><span class="sxs-lookup"><span data-stu-id="f6933-161">Select hello appropriate device.</span></span> 
   3. <span data-ttu-id="f6933-162">Filtrera efter volym eller säkerhetskopiering princip för hello säkerhetskopiering tooselect gärna.</span><span class="sxs-lookup"><span data-stu-id="f6933-162">Filter by volume or backup policy for hello backup that you wish tooselect.</span></span>
   4. <span data-ttu-id="f6933-163">Klicka på **tillämpa** tooexecute den här frågan.</span><span class="sxs-lookup"><span data-stu-id="f6933-163">Click **Apply** tooexecute this query.</span></span>
      
      <span data-ttu-id="f6933-164">hello ska säkerhetskopior som är associerade med principen för hello markerad volym eller säkerhetskopiering visas i hello lista över säkerhetskopior.</span><span class="sxs-lookup"><span data-stu-id="f6933-164">hello backups associated with hello selected volume or backup policy should appear in hello list of backup sets.</span></span>

      ![Gå toobackup katalog](./media/storsimple-8000-manage-backup-catalog/bucatalog1.png)

3. <span data-ttu-id="f6933-166">Markera och utöka en säkerhetskopia.</span><span class="sxs-lookup"><span data-stu-id="f6933-166">Select and expand a backup set.</span></span> <span data-ttu-id="f6933-167">Du kan nu se hello säkerhetskopior uppdelat efter hello volymer som den innehåller.</span><span class="sxs-lookup"><span data-stu-id="f6933-167">You can now see hello backup sets broken down by hello volumes that it contains.</span></span> <span data-ttu-id="f6933-168">Hej **återställa** och **ta bort** alternativ är tillgängliga via hello snabbmeny (högerklicka) för hello säkerhetskopian.</span><span class="sxs-lookup"><span data-stu-id="f6933-168">hello **Restore** and **Delete** options are available via hello context menu (right-click) for hello backup set.</span></span> <span data-ttu-id="f6933-169">Högerklicka på hello valda säkerhetskopian och hello snabbmenyn, Välj **ta bort**.</span><span class="sxs-lookup"><span data-stu-id="f6933-169">Right-click hello selected backup set and from hello context menu, select **Delete**.</span></span>

    ![Gå toobackup katalog](./media/storsimple-8000-manage-backup-catalog/bucatalog3.png)

4. <span data-ttu-id="f6933-171">När du uppmanas att bekräfta granskar hello visas information och klickar på **ta bort**.</span><span class="sxs-lookup"><span data-stu-id="f6933-171">When prompted for confirmation, review hello displayed information and click **Delete**.</span></span> <span data-ttu-id="f6933-172">hello valda säkerhetskopian tas bort permanent.</span><span class="sxs-lookup"><span data-stu-id="f6933-172">hello selected backup is deleted permanently.</span></span>

    ![Gå toobackup katalog](./media/storsimple-8000-manage-backup-catalog/bucatalog4.png)  

5. <span data-ttu-id="f6933-174">Du meddelas när hello borttagning pågår och när den har slutförts.</span><span class="sxs-lookup"><span data-stu-id="f6933-174">You will be notified when hello deletion is in progress and when it has successfully finished.</span></span> <span data-ttu-id="f6933-175">Uppdatera hello frågan på den här sidan när hello borttagningen är klar.</span><span class="sxs-lookup"><span data-stu-id="f6933-175">After hello deletion is done, refresh hello query on this page.</span></span> <span data-ttu-id="f6933-176">hello bort säkerhetskopia visas inte längre i hello lista över säkerhetskopior.</span><span class="sxs-lookup"><span data-stu-id="f6933-176">hello deleted backup set will no longer appear in hello list of backup sets.</span></span>

    ![Gå toobackup katalog](./media/storsimple-8000-manage-backup-catalog/bucatalog7.png)

## <a name="next-steps"></a><span data-ttu-id="f6933-178">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="f6933-178">Next steps</span></span>
* <span data-ttu-id="f6933-179">Lär dig hur för[Använd hello säkerhetskopieringskatalogen toorestore enheten från en säkerhetskopia](storsimple-8000-restore-from-backup-set-u2.md).</span><span class="sxs-lookup"><span data-stu-id="f6933-179">Learn how too[use hello backup catalog toorestore your device from a backup set](storsimple-8000-restore-from-backup-set-u2.md).</span></span>
* <span data-ttu-id="f6933-180">Lär dig hur för[Använd hello StorSimple Enhetshanteraren service tooadminister StorSimple-enheten](storsimple-8000-manager-service-administration.md).</span><span class="sxs-lookup"><span data-stu-id="f6933-180">Learn how too[use hello StorSimple Device Manager service tooadminister your StorSimple device](storsimple-8000-manager-service-administration.md).</span></span>

