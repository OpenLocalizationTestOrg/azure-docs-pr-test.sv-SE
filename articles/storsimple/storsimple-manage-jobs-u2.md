---
title: aaaView och hantera StorSimple jobb | Microsoft Docs
description: "Beskriver sidan för jobb hello StorSimple Manager-tjänsten och hur toouse den tootrack senaste aktuella och schemalagda säkerhetskopieringsjobb."
services: storsimple
documentationcenter: NA
author: alkohli
manager: carmonm
editor: 
ms.assetid: cdd3e9e8-7ccd-40a3-8c07-0ab1338c0e59
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 08/17/2016
ms.author: alkohli
ms.openlocfilehash: d6ecdcbc3d8a4757c2328303f268e51c8ce26b65
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="use-hello-storsimple-manager-service-tooview-and-manage-storsimple-jobs-update-2"></a><span data-ttu-id="677dd-103">Använd hello StorSimple Manager-tjänsten tooview och hantera StorSimple jobb (uppdatering 2)</span><span class="sxs-lookup"><span data-stu-id="677dd-103">Use hello StorSimple Manager service tooview and manage StorSimple jobs (Update 2)</span></span>
[!INCLUDE [storsimple-version-selector-manage-jobs](../../includes/storsimple-version-selector-manage-jobs.md)]

## <a name="overview"></a><span data-ttu-id="677dd-104">Översikt</span><span class="sxs-lookup"><span data-stu-id="677dd-104">Overview</span></span>
<span data-ttu-id="677dd-105">Hej **jobb** innehåller en enda central portal för visning och hantera jobb som har startats på enheter anslutna tooyour StorSimple Manager-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="677dd-105">hello **Jobs** page provides a single central portal for viewing and managing jobs that were started on devices connected tooyour StorSimple Manager service.</span></span> <span data-ttu-id="677dd-106">Du kan visa schemalagda, körs, slutfört, avbrutet och misslyckade jobb för flera enheter.</span><span class="sxs-lookup"><span data-stu-id="677dd-106">You can view scheduled, running, completed, canceled, and failed jobs for multiple devices.</span></span> <span data-ttu-id="677dd-107">Resultat visas i tabellformat.</span><span class="sxs-lookup"><span data-stu-id="677dd-107">Results are presented in a tabular format.</span></span> 

![Sidan jobb](./media/storsimple-manage-jobs-u2/jobs.png)

<span data-ttu-id="677dd-109">Du kan snabbt hitta hello jobb som du är intresserad av genom att filtrera på fält som:</span><span class="sxs-lookup"><span data-stu-id="677dd-109">You can quickly find hello jobs you are interested in by filtering on fields such as:</span></span>

* <span data-ttu-id="677dd-110">**Status för** – jobb kan köras, slutförda, avbrutna, misslyckades, stoppar eller slutfördes med fel.</span><span class="sxs-lookup"><span data-stu-id="677dd-110">**Status** – Jobs can be running, completed, canceled, failed, canceling, or completed with errors.</span></span>
* <span data-ttu-id="677dd-111">**Från och till** – jobb kan filtreras baserat på hello datum och ett tidsintervall.</span><span class="sxs-lookup"><span data-stu-id="677dd-111">**From and To** – Jobs can be filtered based on hello date and time range.</span></span>
* <span data-ttu-id="677dd-112">**Typen** – hello jobbtypen kan vara säkerhetskopiering, manuell säkerhetskopiering, återställning, kloning, enheten redundans, skapar lokalt Fäst volym, ändra volymen, uppdatera, stöd för paket eller etablering av virtuella enheten.</span><span class="sxs-lookup"><span data-stu-id="677dd-112">**Type** – hello job type can be backup, manual backup, restore, clone, device failover, create locally pinned volume, modify volume, update, support package, or virtual device provisioning.</span></span>
* <span data-ttu-id="677dd-113">**Enheter** – jobb startas på en viss enhet ansluten tooyour-tjänst.</span><span class="sxs-lookup"><span data-stu-id="677dd-113">**Devices** – Jobs are initiated on a certain device connected tooyour service.</span></span>
  <span data-ttu-id="677dd-114">hello filtrerade jobben visas sedan som en tabell hello baserat på hello följande attribut:</span><span class="sxs-lookup"><span data-stu-id="677dd-114">hello filtered jobs are then tabulated on hello basis of hello following attributes:</span></span>
  
  * <span data-ttu-id="677dd-115">**Typen** – säkerhetskopiering, manuell säkerhetskopiering, återställning, kloning, enheten redundans, skapar lokalt Fäst volym, ändra volymen, uppdatera, stöd för paket eller etablering av virtuella enheten.</span><span class="sxs-lookup"><span data-stu-id="677dd-115">**Type** – backup, manual backup, restore, clone, device failover, create locally pinned volume, modify volume, update, support package, or virtual device provisioning.</span></span>
  * <span data-ttu-id="677dd-116">**Status för** – körs, slutförda, avbrutna, misslyckades, stoppar eller slutfördes med fel.</span><span class="sxs-lookup"><span data-stu-id="677dd-116">**Status** – running, completed, canceled, failed, canceling, or completed with errors.</span></span>
  * <span data-ttu-id="677dd-117">**Entiteten** – hello jobb kan vara kopplad till en volym, en princip för säkerhetskopiering eller en enhet.</span><span class="sxs-lookup"><span data-stu-id="677dd-117">**Entity** – hello jobs can be associated with a volume, a backup policy, or a device.</span></span> <span data-ttu-id="677dd-118">Till exempel associeras ett jobb för klon med en volym, en schemalagd säkerhetskopiering är associerad med en princip för säkerhetskopiering.</span><span class="sxs-lookup"><span data-stu-id="677dd-118">For example, a clone job is associated with a volume, whereas a scheduled backup job is associated with a backup policy.</span></span> <span data-ttu-id="677dd-119">En enhet jobb skapas på grund av en katastrofåterställning (DR) eller en återställning.</span><span class="sxs-lookup"><span data-stu-id="677dd-119">A device job is created as a result of a disaster recovery (DR) or a restore operation.</span></span>
  * <span data-ttu-id="677dd-120">**Enheten** – hello namn på hello enhet på vilken hello-jobbet startades.</span><span class="sxs-lookup"><span data-stu-id="677dd-120">**Device** – hello name of hello device on which hello job was started.</span></span>
  * <span data-ttu-id="677dd-121">**Startats på** – hello tid då hello-jobbet startades.</span><span class="sxs-lookup"><span data-stu-id="677dd-121">**Started on** – hello time when hello job was started.</span></span>
  * <span data-ttu-id="677dd-122">**Förlopp** – hello procentandel slutförande av ett jobb som körs.</span><span class="sxs-lookup"><span data-stu-id="677dd-122">**Progress** – hello percentage completion of a running job.</span></span> <span data-ttu-id="677dd-123">Detta bör alltid vara 100% för slutförda jobb.</span><span class="sxs-lookup"><span data-stu-id="677dd-123">For a completed job, this should always be 100%.</span></span>

<span data-ttu-id="677dd-124">hello lista över jobb uppdateras var 30: e sekund.</span><span class="sxs-lookup"><span data-stu-id="677dd-124">hello list of jobs is refreshed every 30 seconds.</span></span>

<span data-ttu-id="677dd-125">Du kan utföra hello följande jobbrelaterade åtgärder på den här sidan:</span><span class="sxs-lookup"><span data-stu-id="677dd-125">You can perform hello following job-related actions on this page:</span></span>

* <span data-ttu-id="677dd-126">Visa jobbinformation</span><span class="sxs-lookup"><span data-stu-id="677dd-126">View job details</span></span>
* <span data-ttu-id="677dd-127">Avbryta ett jobb</span><span class="sxs-lookup"><span data-stu-id="677dd-127">Cancel a job</span></span>

## <a name="view-job-details"></a><span data-ttu-id="677dd-128">Visa jobbinformation</span><span class="sxs-lookup"><span data-stu-id="677dd-128">View job details</span></span>
<span data-ttu-id="677dd-129">Utföra hello följande steg tooview hello information för alla jobb.</span><span class="sxs-lookup"><span data-stu-id="677dd-129">Perform hello following steps tooview hello details of any job.</span></span>

#### <a name="tooview-job-details"></a><span data-ttu-id="677dd-130">tooview jobbinformation</span><span class="sxs-lookup"><span data-stu-id="677dd-130">tooview job details</span></span>
1. <span data-ttu-id="677dd-131">På hello **jobb** sidan, visa hello jobb som du är intresserad av genom att köra en fråga med lämpliga filter.</span><span class="sxs-lookup"><span data-stu-id="677dd-131">On hello **Jobs** page, display hello job(s) you are interested in by running a query with appropriate filters.</span></span> <span data-ttu-id="677dd-132">Du kan söka för slutförd, kör eller avbröts jobb.</span><span class="sxs-lookup"><span data-stu-id="677dd-132">You can search for completed, running, or canceled jobs.</span></span>
2. <span data-ttu-id="677dd-133">Välj ett jobb.</span><span class="sxs-lookup"><span data-stu-id="677dd-133">Select a job.</span></span>
3. <span data-ttu-id="677dd-134">Hello längst hello-sidan, klickar du på **information**.</span><span class="sxs-lookup"><span data-stu-id="677dd-134">At hello bottom of hello page, click **Details**.</span></span>
4. <span data-ttu-id="677dd-135">I hello **säkerhetskopiering jobbinformation** dialogrutan kan du visa hello status, information, statistik och data statistik.</span><span class="sxs-lookup"><span data-stu-id="677dd-135">In hello **Backup Job Details** dialog box, you can view hello status, details, time statistics, and data statistics.</span></span>
   
    ![Informationssidan för jobbet](./media/storsimple-manage-jobs-u2/JobDetails.png)

## <a name="cancel-a-job"></a><span data-ttu-id="677dd-137">Avbryta ett jobb</span><span class="sxs-lookup"><span data-stu-id="677dd-137">Cancel a job</span></span>
<span data-ttu-id="677dd-138">Utföra hello följande steg toocancel ett jobb som körs.</span><span class="sxs-lookup"><span data-stu-id="677dd-138">Perform hello following steps toocancel a running job.</span></span>

> [!NOTE]
> <span data-ttu-id="677dd-139">Kan inte annulleras vissa jobb, t.ex ändra en toochange hello volym volymtyp eller expandera en volym.</span><span class="sxs-lookup"><span data-stu-id="677dd-139">Some jobs, such as modifying a volume toochange hello volume type or expanding a volume, cannot be canceled.</span></span>
> 
> 

### <a name="toocancel-a-job"></a><span data-ttu-id="677dd-140">toocancel ett jobb</span><span class="sxs-lookup"><span data-stu-id="677dd-140">toocancel a job</span></span>
1. <span data-ttu-id="677dd-141">På hello **jobb** sidan, visa hello körs jobb som du vill toocancel genom att köra en fråga med lämpliga filter.</span><span class="sxs-lookup"><span data-stu-id="677dd-141">On hello **Jobs** page, display hello running job(s) that you want toocancel by running a query with appropriate filters.</span></span>
2. <span data-ttu-id="677dd-142">Välj hello jobb.</span><span class="sxs-lookup"><span data-stu-id="677dd-142">Select hello job.</span></span>
3. <span data-ttu-id="677dd-143">Hello längst hello-sidan, klickar du på **Avbryt**.</span><span class="sxs-lookup"><span data-stu-id="677dd-143">At hello bottom of hello page, click **Cancel**.</span></span>
4. <span data-ttu-id="677dd-144">Klicka på **Ja** när du uppmanas att bekräfta åtgärden.</span><span class="sxs-lookup"><span data-stu-id="677dd-144">When prompted for confirmation, click **Yes**.</span></span>

<span data-ttu-id="677dd-145">Det här jobbet avbryts nu.</span><span class="sxs-lookup"><span data-stu-id="677dd-145">This job is now canceled.</span></span>

## <a name="next-steps"></a><span data-ttu-id="677dd-146">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="677dd-146">Next steps</span></span>
* <span data-ttu-id="677dd-147">Lär dig hur för[hantera din StorSimple-säkerhetskopieringsprinciper](storsimple-manage-backup-policies.md).</span><span class="sxs-lookup"><span data-stu-id="677dd-147">Learn how too[manage your StorSimple backup policies](storsimple-manage-backup-policies.md).</span></span>
* <span data-ttu-id="677dd-148">Lär dig hur för[Använd hello StorSimple Manager service tooadminister StorSimple-enheten](storsimple-manager-service-administration.md).</span><span class="sxs-lookup"><span data-stu-id="677dd-148">Learn how too[use hello StorSimple Manager service tooadminister your StorSimple device](storsimple-manager-service-administration.md).</span></span>

