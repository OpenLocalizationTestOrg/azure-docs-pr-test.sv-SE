---
title: "aaaView och hantera jobb för StorSimple 8000-serien | Microsoft Docs"
description: "Beskriver hello StorSimple Enhetshanteraren service jobb-bladet och hur toouse den tootrack senaste aktuella och schemalagda säkerhetskopieringsjobb."
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
ms.openlocfilehash: a76acd67e2568478dd43d0fb16c1ae2dfb72a23d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="use-hello-storsimple-device-manager-service-tooview-and-manage-jobs-update-3-and-later"></a><span data-ttu-id="5c895-103">Använd hello StorSimple Enhetshanteraren service tooview och hantera jobb (uppdatering 3 och senare)</span><span class="sxs-lookup"><span data-stu-id="5c895-103">Use hello StorSimple Device Manager service tooview and manage jobs (Update 3 and later)</span></span>

## <a name="overview"></a><span data-ttu-id="5c895-104">Översikt</span><span class="sxs-lookup"><span data-stu-id="5c895-104">Overview</span></span>
<span data-ttu-id="5c895-105">Hej **jobb** bladet innehåller en enda central portal för visning och hantera jobb som har startats på enheter anslutna tooyour StorSimple enheten Manager-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="5c895-105">hello **Jobs** blade provides a single central portal for viewing and managing jobs that were started on devices connected tooyour StorSimple Device Manager service.</span></span> <span data-ttu-id="5c895-106">Du kan visa schemalagda, körs, slutfört, avbrutet och misslyckade jobb för flera enheter.</span><span class="sxs-lookup"><span data-stu-id="5c895-106">You can view scheduled, running, completed, canceled, and failed jobs for multiple devices.</span></span> <span data-ttu-id="5c895-107">Resultat visas i tabellformat.</span><span class="sxs-lookup"><span data-stu-id="5c895-107">Results are presented in a tabular format.</span></span>

![Jobb-bladet](./media/storsimple-8000-manage-jobs-u2/jobs1.png)

<span data-ttu-id="5c895-109">Du kan snabbt hitta hello jobb som du är intresserad av genom att filtrera på fält som:</span><span class="sxs-lookup"><span data-stu-id="5c895-109">You can quickly find hello jobs you are interested in by filtering on fields such as:</span></span>

* <span data-ttu-id="5c895-110">**Status för** – jobb kan vara pågår, lyckades, avbröts, misslyckades, stoppar eller har slutförts med fel.</span><span class="sxs-lookup"><span data-stu-id="5c895-110">**Status** – Jobs can be in progress, succeeded, canceled, failed, canceling, or succeeded with errors.</span></span>
* <span data-ttu-id="5c895-111">**Tidsintervallet** – jobb kan filtreras baserat på hello datum och ett tidsintervall.</span><span class="sxs-lookup"><span data-stu-id="5c895-111">**Time range** – Jobs can be filtered based on hello date and time range.</span></span> <span data-ttu-id="5c895-112">hello intervall har passerat 1 timme, senaste 24 timmarna, de senaste 7 dagarna, de senaste 30 dagarna, senaste året eller anpassade datum.</span><span class="sxs-lookup"><span data-stu-id="5c895-112">hello ranges are past 1 hour, past 24 hours, past 7 days, past 30 days, past year, or custom date.</span></span>
* <span data-ttu-id="5c895-113">**Typen** – hello jobbtypen kan vara schemalagd säkerhetskopiering, manuell säkerhetskopiering, återställning, säkerhetskopiering, klona volymen, växla över volymbehållare, skapa lokalt Fäst volym, ändra volymen, installera uppdateringar, samla in loggar för support och skapa moln-enhet.</span><span class="sxs-lookup"><span data-stu-id="5c895-113">**Type** – hello job type can be scheduled backup, manual backup, restore backup, clone volume, fail over volume containers, create locally-pinned volume, modify volume, install updates, collect support logs and create cloud appliance.</span></span>
* <span data-ttu-id="5c895-114">**Enheter** – jobb startas på en viss enhet ansluten tooyour-tjänst.</span><span class="sxs-lookup"><span data-stu-id="5c895-114">**Devices** – Jobs are initiated on a certain device connected tooyour service.</span></span>
  
<span data-ttu-id="5c895-115">hello filtrerade jobben visas sedan som en tabell hello baserat på hello följande attribut:</span><span class="sxs-lookup"><span data-stu-id="5c895-115">hello filtered jobs are then tabulated on hello basis of hello following attributes:</span></span>
  
* <span data-ttu-id="5c895-116">**Namnet** – schemalagd säkerhetskopiering, manuell säkerhetskopiering, återställning, säkerhetskopiering, klona volym växla över volymbehållare, skapar lokalt Fäst volym, ändra volymen, installera uppdateringar, samla in loggar för stöd eller skapa moln-enhet.</span><span class="sxs-lookup"><span data-stu-id="5c895-116">**Name** – scheduled backup, manual backup, restore backup, clone volume, fail over volume containers, create locally pinned volume, modify volume, install updates, collect support logs, or create cloud appliance.</span></span>
* <span data-ttu-id="5c895-117">**Status för** – körs, slutförda, avbrutna, misslyckades, stoppar eller slutfördes med fel.</span><span class="sxs-lookup"><span data-stu-id="5c895-117">**Status** – running, completed, canceled, failed, canceling, or completed with errors.</span></span>
* <span data-ttu-id="5c895-118">**Entiteten** – hello jobb kan vara kopplad till en volym, en princip för säkerhetskopiering eller en enhet.</span><span class="sxs-lookup"><span data-stu-id="5c895-118">**Entity** – hello jobs can be associated with a volume, a backup policy, or a device.</span></span> <span data-ttu-id="5c895-119">Till exempel associeras ett jobb för klon med en volym, en schemalagd säkerhetskopiering är associerad med en princip för säkerhetskopiering.</span><span class="sxs-lookup"><span data-stu-id="5c895-119">For example, a clone job is associated with a volume, whereas a scheduled backup job is associated with a backup policy.</span></span> <span data-ttu-id="5c895-120">En enhet jobb skapas på grund av en katastrofåterställning (DR) eller en återställning.</span><span class="sxs-lookup"><span data-stu-id="5c895-120">A device job is created as a result of a disaster recovery (DR) or a restore operation.</span></span>
* <span data-ttu-id="5c895-121">**Enheten** – hello namn på hello enhet på vilken hello-jobbet startades.</span><span class="sxs-lookup"><span data-stu-id="5c895-121">**Device** – hello name of hello device on which hello job was started.</span></span>
* <span data-ttu-id="5c895-122">**Startats på** – hello tid då hello-jobbet startades.</span><span class="sxs-lookup"><span data-stu-id="5c895-122">**Started on** – hello time when hello job was started.</span></span>
* <span data-ttu-id="5c895-123">**Varaktighet** – hello krävs toocomplete hello jobbet.</span><span class="sxs-lookup"><span data-stu-id="5c895-123">**Duration** – hello time required toocomplete hello job.</span></span>

<span data-ttu-id="5c895-124">hello lista över jobb uppdateras var 30: e sekund.</span><span class="sxs-lookup"><span data-stu-id="5c895-124">hello list of jobs is refreshed every 30 seconds.</span></span>

<span data-ttu-id="5c895-125">Du kan utföra hello följande jobbrelaterade åtgärder på den här sidan:</span><span class="sxs-lookup"><span data-stu-id="5c895-125">You can perform hello following job-related actions on this page:</span></span>

* <span data-ttu-id="5c895-126">Visa jobbinformation</span><span class="sxs-lookup"><span data-stu-id="5c895-126">View job details</span></span>
* <span data-ttu-id="5c895-127">Avbryta ett jobb</span><span class="sxs-lookup"><span data-stu-id="5c895-127">Cancel a job</span></span>

## <a name="view-job-details"></a><span data-ttu-id="5c895-128">Visa jobbinformation</span><span class="sxs-lookup"><span data-stu-id="5c895-128">View job details</span></span>
<span data-ttu-id="5c895-129">Utföra hello följande steg tooview hello information för alla jobb.</span><span class="sxs-lookup"><span data-stu-id="5c895-129">Perform hello following steps tooview hello details of any job.</span></span>

#### <a name="tooview-job-details"></a><span data-ttu-id="5c895-130">tooview jobbinformation</span><span class="sxs-lookup"><span data-stu-id="5c895-130">tooview job details</span></span>
1. <span data-ttu-id="5c895-131">Gå tooyour StorSimple enheten Manager-tjänsten och klicka sedan på **jobb**.</span><span class="sxs-lookup"><span data-stu-id="5c895-131">Go tooyour StorSimple Device Manager service and then click **Jobs**.</span></span>

2. <span data-ttu-id="5c895-132">I hello **jobb** bladet, visa hello jobb som du är intresserad av genom att köra en fråga med lämpliga filter.</span><span class="sxs-lookup"><span data-stu-id="5c895-132">In hello **Jobs** blade, display hello job(s) you are interested in by running a query with appropriate filters.</span></span> <span data-ttu-id="5c895-133">Du kan söka för slutförd, kör eller avbröts jobb.</span><span class="sxs-lookup"><span data-stu-id="5c895-133">You can search for completed, running, or canceled jobs.</span></span>

    ![Jobb-bladet](./media/storsimple-8000-manage-jobs-u2/jobs1.png)

2. <span data-ttu-id="5c895-135">Välj och klicka på ett jobb.</span><span class="sxs-lookup"><span data-stu-id="5c895-135">Select and click a job.</span></span>

    ![Jobb-bladet](./media/storsimple-8000-manage-jobs-u2/jobs3.png)

3. <span data-ttu-id="5c895-137">I hello jobbet informationsbladet ser du hello status, information, statistik och data statistik.</span><span class="sxs-lookup"><span data-stu-id="5c895-137">In hello job details blade, you can view hello status, details, time statistics, and data statistics.</span></span>
   
    ![Jobbinformation](./media/storsimple-8000-manage-jobs-u2/jobs4.png)

## <a name="cancel-a-job"></a><span data-ttu-id="5c895-139">Avbryta ett jobb</span><span class="sxs-lookup"><span data-stu-id="5c895-139">Cancel a job</span></span>
<span data-ttu-id="5c895-140">Utföra hello följande steg toocancel ett jobb som körs.</span><span class="sxs-lookup"><span data-stu-id="5c895-140">Perform hello following steps toocancel a running job.</span></span>

> [!NOTE]
> <span data-ttu-id="5c895-141">Kan inte annulleras vissa jobb, t.ex ändra en toochange hello volym volymtyp eller expandera en volym.</span><span class="sxs-lookup"><span data-stu-id="5c895-141">Some jobs, such as modifying a volume toochange hello volume type or expanding a volume, cannot be canceled.</span></span>


### <a name="toocancel-a-job"></a><span data-ttu-id="5c895-142">toocancel ett jobb</span><span class="sxs-lookup"><span data-stu-id="5c895-142">toocancel a job</span></span>
1. <span data-ttu-id="5c895-143">På hello **jobb** sidan, visa hello körs jobb som du vill toocancel genom att köra en fråga med lämpliga filter.</span><span class="sxs-lookup"><span data-stu-id="5c895-143">On hello **Jobs** page, display hello running job(s) that you want toocancel by running a query with appropriate filters.</span></span> <span data-ttu-id="5c895-144">Välj hello jobb.</span><span class="sxs-lookup"><span data-stu-id="5c895-144">Select hello job.</span></span>

2. <span data-ttu-id="5c895-145">Högerklicka på snabbmenyn för hello valda jobbet tooinvoke hello och på **Avbryt**.</span><span class="sxs-lookup"><span data-stu-id="5c895-145">Right-click on hello selected job tooinvoke hello context menu and click **Cancel**.</span></span>

    ![Jobbinformation](./media/storsimple-8000-manage-jobs-u2/jobs2.png)

3. <span data-ttu-id="5c895-147">Klicka på **Ja** när du uppmanas att bekräfta åtgärden.</span><span class="sxs-lookup"><span data-stu-id="5c895-147">When prompted for confirmation, click **Yes**.</span></span> <span data-ttu-id="5c895-148">Det här jobbet avbryts nu.</span><span class="sxs-lookup"><span data-stu-id="5c895-148">This job is now canceled.</span></span>

## <a name="next-steps"></a><span data-ttu-id="5c895-149">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="5c895-149">Next steps</span></span>
* <span data-ttu-id="5c895-150">Lär dig hur för[hantera din StorSimple-säkerhetskopieringsprinciper](storsimple-8000-manage-backup-policies-u2.md).</span><span class="sxs-lookup"><span data-stu-id="5c895-150">Learn how too[manage your StorSimple backup policies](storsimple-8000-manage-backup-policies-u2.md).</span></span>
* <span data-ttu-id="5c895-151">Lär dig hur för[Använd hello StorSimple Enhetshanteraren service tooadminister StorSimple-enheten](storsimple-8000-manager-service-administration.md).</span><span class="sxs-lookup"><span data-stu-id="5c895-151">Learn how too[use hello StorSimple Device Manager service tooadminister your StorSimple device](storsimple-8000-manager-service-administration.md).</span></span>

