---
title: "aaaStorSimple Snapshot Manager säkerhetskopieringsjobb | Microsoft Docs"
description: "Beskriver hur toouse hello StorSimple Snapshot Manager MMC snapin-modulen tooview och hantera schemalagda pågående och slutförda säkerhetskopieringsjobb."
services: storsimple
documentationcenter: NA
author: SharS
manager: timlt
editor: 
ms.assetid: bf4dcff6-c819-4766-b9d9-9922831cb200
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 06/05/2017
ms.author: v-sharos
ms.openlocfilehash: 3dba0a2aa527d17d67130f537bcdce5722b05a76
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="use-storsimple-snapshot-manager-tooview-and-manage-backup-jobs"></a><span data-ttu-id="dd2a7-103">Använda StorSimple Snapshot Manager tooview och hantera säkerhetskopieringsjobb</span><span class="sxs-lookup"><span data-stu-id="dd2a7-103">Use StorSimple Snapshot Manager tooview and manage backup jobs</span></span>

## <a name="overview"></a><span data-ttu-id="dd2a7-104">Översikt</span><span class="sxs-lookup"><span data-stu-id="dd2a7-104">Overview</span></span>
<span data-ttu-id="dd2a7-105">Hej **jobb** nod i hello **omfång** visar hello **schemalagda**, **senaste 24 timmarna**, och **kör**säkerhetskopiera uppgifter som du har initierat interaktivt eller av en konfigurerade principen.</span><span class="sxs-lookup"><span data-stu-id="dd2a7-105">hello **Jobs** node in hello **Scope** pane shows hello **Scheduled**, **Last 24 hours**, and **Running** backup tasks that you initiated interactively or by a configured policy.</span></span> 

<span data-ttu-id="dd2a7-106">Den här självstudiekursen beskrivs hur du kan använda hello **jobb** nod toodisplay information om schemalagda senaste och pågående säkerhetskopieringsjobb.</span><span class="sxs-lookup"><span data-stu-id="dd2a7-106">This tutorial explains how you can use hello **Jobs** node toodisplay information about scheduled, recent, and currently running backup jobs.</span></span> <span data-ttu-id="dd2a7-107">(hello lista över jobb och motsvarande information visas i hello **resultat** fönstret.) Du kan dessutom högerklickar du på ett listade jobb och se en snabbmeny som visar tillgängliga åtgärder.</span><span class="sxs-lookup"><span data-stu-id="dd2a7-107">(hello list of jobs and corresponding information appears in hello **Results** pane.) Additionally, you can right-click a listed job and see a context menu that lists available actions.</span></span>

## <a name="view-scheduled-jobs"></a><span data-ttu-id="dd2a7-108">Visa schemalagda jobb</span><span class="sxs-lookup"><span data-stu-id="dd2a7-108">View scheduled jobs</span></span>
<span data-ttu-id="dd2a7-109">Använd följande procedur tooview schemalagda säkerhetskopieringsjobb hello.</span><span class="sxs-lookup"><span data-stu-id="dd2a7-109">Use hello following procedure tooview scheduled backup jobs.</span></span>

#### <a name="tooview-scheduled-jobs"></a><span data-ttu-id="dd2a7-110">tooview schemalagda jobb</span><span class="sxs-lookup"><span data-stu-id="dd2a7-110">tooview scheduled jobs</span></span>
1. <span data-ttu-id="dd2a7-111">Klicka på hello skrivbordsikon toostart StorSimple Snapshot Manager.</span><span class="sxs-lookup"><span data-stu-id="dd2a7-111">Click hello desktop icon toostart StorSimple Snapshot Manager.</span></span> 
2. <span data-ttu-id="dd2a7-112">I hello **omfång** rutan Expandera hello **jobb** och klicka **schemalagda**.</span><span class="sxs-lookup"><span data-stu-id="dd2a7-112">In hello **Scope** pane, expand hello **Jobs** node, and click **Scheduled**.</span></span> <span data-ttu-id="dd2a7-113">hello visas följande information i hello **resultat** fönstret:</span><span class="sxs-lookup"><span data-stu-id="dd2a7-113">hello following information appears in hello **Results** pane:</span></span>
   
   * <span data-ttu-id="dd2a7-114">**Namnet** – hello namn på hello schemalagda ögonblicksbild</span><span class="sxs-lookup"><span data-stu-id="dd2a7-114">**Name** – hello name of hello scheduled snapshot</span></span>
   * <span data-ttu-id="dd2a7-115">**Kör nästa gång** – hello datum och tid för hello nästa schemalagda ögonblicksbild</span><span class="sxs-lookup"><span data-stu-id="dd2a7-115">**Next Run** – hello date and time of hello next scheduled snapshot</span></span>
   * <span data-ttu-id="dd2a7-116">**Senaste körning** – hello datum och tid för hello senaste schemalagda ögonblicksbild</span><span class="sxs-lookup"><span data-stu-id="dd2a7-116">**Last Run** – hello date and time of hello most recent scheduled snapshot</span></span>
     
     > [!NOTE]
     > <span data-ttu-id="dd2a7-117">En enda ögonblicksbilder hello **nästa** och **senaste kör** blir hello samma.</span><span class="sxs-lookup"><span data-stu-id="dd2a7-117">For one-time only snapshots, hello **Next Run** and **Last Run** will be hello same.</span></span>
     
     ![Schemalagda säkerhetskopieringsjobb](./media/storsimple-snapshot-manager-manage-backup-jobs/HCS_SSM_Jobs_scheduled.png) 
3. <span data-ttu-id="dd2a7-119">tooperform ytterligare åtgärder på ett specifikt jobb, högerklicka på hello Jobbnamnet i hello **resultat** fönstret och välj bland hello menyalternativen.</span><span class="sxs-lookup"><span data-stu-id="dd2a7-119">tooperform additional actions on a specific job, right-click hello job name in hello **Results** pane and select from hello menu options.</span></span>

## <a name="view-recent-jobs"></a><span data-ttu-id="dd2a7-120">Visa senaste jobb</span><span class="sxs-lookup"><span data-stu-id="dd2a7-120">View recent jobs</span></span>
<span data-ttu-id="dd2a7-121">Använd hello följa proceduren tooview säkerhetskopiering och återställning jobb som har slutförts i hello senaste 24 timmarna.</span><span class="sxs-lookup"><span data-stu-id="dd2a7-121">Use hello following procedure tooview backup and restore jobs that were completed in hello last 24 hours.</span></span>

#### <a name="tooview-recent-jobs"></a><span data-ttu-id="dd2a7-122">tooview senaste jobb</span><span class="sxs-lookup"><span data-stu-id="dd2a7-122">tooview recent jobs</span></span>
1. <span data-ttu-id="dd2a7-123">Klicka på hello skrivbordsikon toostart StorSimple Snapshot Manager.</span><span class="sxs-lookup"><span data-stu-id="dd2a7-123">Click hello desktop icon toostart StorSimple Snapshot Manager.</span></span>
2. <span data-ttu-id="dd2a7-124">I hello **omfång** rutan Expandera hello **jobb** och klicka **senaste 24 timmarna**.</span><span class="sxs-lookup"><span data-stu-id="dd2a7-124">In hello **Scope** pane, expand hello **Jobs** node, and click **Last 24 hours**.</span></span> <span data-ttu-id="dd2a7-125">Hej **resultat** visar säkerhetskopieringsjobb för hello senaste 24 timmarna (tooa högst 64 jobb).</span><span class="sxs-lookup"><span data-stu-id="dd2a7-125">hello **Results** pane shows backup jobs for hello last 24 hours (tooa maximum of 64 jobs).</span></span> <span data-ttu-id="dd2a7-126">hello visas följande information i hello **resultat** rutan, beroende på hello **visa** alternativ som du anger:</span><span class="sxs-lookup"><span data-stu-id="dd2a7-126">hello following information appears in hello **Results** pane, depending on hello **View** options you specify:</span></span>
   
   * <span data-ttu-id="dd2a7-127">**Namnet** – hello namn på hello schemalagda ögonblicksbild.</span><span class="sxs-lookup"><span data-stu-id="dd2a7-127">**Name** – hello name of hello scheduled snapshot.</span></span>
   * <span data-ttu-id="dd2a7-128">**Starta** – hello datum och tid då hello ögonblicksbild påbörjades.</span><span class="sxs-lookup"><span data-stu-id="dd2a7-128">**Started** – hello date and time when hello snapshot began.</span></span>
   * <span data-ttu-id="dd2a7-129">**Stoppats** – hello datum och tid när hello ögonblicksbild slutförts eller avslutats.</span><span class="sxs-lookup"><span data-stu-id="dd2a7-129">**Stopped** – hello date and time when hello snapshot finished or was terminated.</span></span>
   * <span data-ttu-id="dd2a7-130">**Förfluten tid** – hello tidslängd mellan hello **igång** och **stoppad** gånger.</span><span class="sxs-lookup"><span data-stu-id="dd2a7-130">**Elapsed** – hello amount of time between hello **Started** and **Stopped** times.</span></span>
   * <span data-ttu-id="dd2a7-131">**Status för** – hello tillståndet för hello nyligen slutföra jobbet.</span><span class="sxs-lookup"><span data-stu-id="dd2a7-131">**Status** – hello state of hello recently completed job.</span></span> <span data-ttu-id="dd2a7-132">**Lyckade** anger hello säkerhetskopian skapades.</span><span class="sxs-lookup"><span data-stu-id="dd2a7-132">**Success** indicates that hello backup was created successfully.</span></span> <span data-ttu-id="dd2a7-133">**Det gick inte** anger hello jobbet inte kördes.</span><span class="sxs-lookup"><span data-stu-id="dd2a7-133">**Failed** indicates that hello job did not run successfully.</span></span>
   * <span data-ttu-id="dd2a7-134">**Information** – hello hello felet.</span><span class="sxs-lookup"><span data-stu-id="dd2a7-134">**Information** – hello reason for hello failure.</span></span>
   * <span data-ttu-id="dd2a7-135">**Byte bearbetas (MB)** – hello mängd data från hello volym grupp som har bearbetats (i MB).</span><span class="sxs-lookup"><span data-stu-id="dd2a7-135">**Bytes processed (MB)** – hello amount of data from hello volume group that was processed (in MBs).</span></span> 
     
     ![Jobb som körts i hello senaste 24 timmarna](./media/storsimple-snapshot-manager-manage-backup-jobs/HCS_SSM_Jobs_Last_24_hours.png) 
3. <span data-ttu-id="dd2a7-137">tooperform ytterligare åtgärder på ett specifikt jobb, högerklicka på hello Jobbnamnet i hello **resultat** fönstret och välj bland hello menyalternativen.</span><span class="sxs-lookup"><span data-stu-id="dd2a7-137">tooperform additional actions on a specific job, right-click hello job name in hello **Results** pane and select from hello menu options.</span></span>
   
    ![Ta bort ett jobb](./media/storsimple-snapshot-manager-manage-backup-catalog/HCS_SSM_Delete_backup.png)

## <a name="view-currently-running-jobs"></a><span data-ttu-id="dd2a7-139">Visa pågående jobb</span><span class="sxs-lookup"><span data-stu-id="dd2a7-139">View currently running jobs</span></span>
<span data-ttu-id="dd2a7-140">Använd hello följa proceduren tooview jobb som körs för närvarande.</span><span class="sxs-lookup"><span data-stu-id="dd2a7-140">Use hello following procedure tooview jobs that are currently running.</span></span>

#### <a name="tooview-currently-running-jobs"></a><span data-ttu-id="dd2a7-141">tooview jobb som körs för närvarande</span><span class="sxs-lookup"><span data-stu-id="dd2a7-141">tooview currently running jobs</span></span>
1. <span data-ttu-id="dd2a7-142">Klicka på hello skrivbordsikon toostart StorSimple Snapshot Manager.</span><span class="sxs-lookup"><span data-stu-id="dd2a7-142">Click hello desktop icon toostart StorSimple Snapshot Manager.</span></span>
2. <span data-ttu-id="dd2a7-143">I hello **omfång** rutan Expandera hello **jobb** och klicka **kör**.</span><span class="sxs-lookup"><span data-stu-id="dd2a7-143">In hello **Scope** pane, expand hello **Jobs** node, and click **Running**.</span></span> <span data-ttu-id="dd2a7-144">Beroende på hello **visa** alternativ som du anger hello följande information visas i hello **resultat** fönstret:</span><span class="sxs-lookup"><span data-stu-id="dd2a7-144">Depending on hello **View** options you specify, hello following information appears in hello **Results** pane:</span></span>
   
   * <span data-ttu-id="dd2a7-145">**Namnet** – hello namn på hello schemalagda ögonblicksbild.</span><span class="sxs-lookup"><span data-stu-id="dd2a7-145">**Name** – hello name of hello scheduled snapshot.</span></span>
   * <span data-ttu-id="dd2a7-146">**Starta** – hello datum och tid då hello ögonblicksbild påbörjades.</span><span class="sxs-lookup"><span data-stu-id="dd2a7-146">**Started** – hello date and time when hello snapshot began.</span></span>
   * <span data-ttu-id="dd2a7-147">**Kontrollpunkt** – hello aktuell åtgärd av hello säkerhetskopiering.</span><span class="sxs-lookup"><span data-stu-id="dd2a7-147">**Checkpoint** – hello current action of hello backup.</span></span>
   * <span data-ttu-id="dd2a7-148">**Status för** – hello procent färdigt.</span><span class="sxs-lookup"><span data-stu-id="dd2a7-148">**Status** – hello percentage of completion.</span></span>
   * <span data-ttu-id="dd2a7-149">**Förfluten tid** – hello mängden tid som har gått sedan hello säkerhetskopiering påbörjades.</span><span class="sxs-lookup"><span data-stu-id="dd2a7-149">**Elapsed** – hello amount of time that has passed since hello backup began.</span></span> 
   * <span data-ttu-id="dd2a7-150">**Genomsnittlig genomströmning (MB)** – förhållandet mellan totalt antal bearbetade data toothat total tid för bearbetning av (MB) byte.</span><span class="sxs-lookup"><span data-stu-id="dd2a7-150">**Average throughput (MB)** – ratio of total bytes of data processed toothat of total time taken for processing (MBs).</span></span>
   * <span data-ttu-id="dd2a7-151">**Byte bearbetas (MB)** – Totalt antal byte av data som bearbetas (i MB).</span><span class="sxs-lookup"><span data-stu-id="dd2a7-151">**Bytes processed (MB)** – total bytes of data processed (in MBs).</span></span>
   * <span data-ttu-id="dd2a7-152">**Byte som skrivits (MB)** – Totalt antal byte skrevs (i MB).</span><span class="sxs-lookup"><span data-stu-id="dd2a7-152">**Bytes written (MB)** – total bytes of data written (in MBs).</span></span> <span data-ttu-id="dd2a7-153">Den inkluderar hello data samt hello metadata och därför är vanligtvis större än hello byte bearbetas.</span><span class="sxs-lookup"><span data-stu-id="dd2a7-153">It includes hello data as well as hello metadata and hence is typically greater than hello Bytes Processed.</span></span>
     
     ![Jobb som körs](./media/storsimple-snapshot-manager-manage-backup-jobs/HCS_SSM_Jobs_running.png)
3. <span data-ttu-id="dd2a7-155">tooperform ytterligare åtgärder på ett specifikt jobb, högerklicka på hello Jobbnamnet i hello **resultat** fönstret och välj bland hello menyalternativen.</span><span class="sxs-lookup"><span data-stu-id="dd2a7-155">tooperform additional actions on a specific job, right-click hello job name in hello **Results** pane and select from hello menu options.</span></span>

## <a name="next-steps"></a><span data-ttu-id="dd2a7-156">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="dd2a7-156">Next steps</span></span>
* <span data-ttu-id="dd2a7-157">Lär dig hur för[med StorSimple Snapshot Manager tooadminister din StorSimple-lösning](storsimple-snapshot-manager-admin.md).</span><span class="sxs-lookup"><span data-stu-id="dd2a7-157">Learn how too[use StorSimple Snapshot Manager tooadminister your StorSimple solution](storsimple-snapshot-manager-admin.md).</span></span>
* <span data-ttu-id="dd2a7-158">Lär dig hur för[använder StorSimple Snapshot Manager toomanage hello säkerhetskopieringskatalogen](storsimple-snapshot-manager-manage-backup-catalog.md).</span><span class="sxs-lookup"><span data-stu-id="dd2a7-158">Learn how too[use StorSimple Snapshot Manager toomanage hello backup catalog](storsimple-snapshot-manager-manage-backup-catalog.md).</span></span>

