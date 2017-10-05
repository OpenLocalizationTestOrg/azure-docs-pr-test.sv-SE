---
title: Visa och hantera StorSimple jobb | Microsoft Docs
description: "Beskriver sidan StorSimple Manager-jobb och hur du använder den för att spåra senaste aktuella och schemalagda säkerhetskopieringsjobb."
services: storsimple
documentationcenter: NA
author: alkohli
manager: carmonm
editor: 
ms.assetid: 55922cd0-d490-48eb-938a-012a67c1c09e
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 08/17/2016
ms.author: alkohli
ms.openlocfilehash: 7d9377bb8f3cb8c587823c2d71d61dfb9b50f52f
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="use-the-storsimple-manager-service-to-view-and-manage-storsimple-jobs"></a><span data-ttu-id="dc2b5-103">Använda StorSimple Manager-tjänsten för att visa och hantera jobb för StorSimple</span><span class="sxs-lookup"><span data-stu-id="dc2b5-103">Use the StorSimple Manager service to view and manage StorSimple jobs</span></span>
[!INCLUDE [storsimple-version-selector-manage-jobs](../../includes/storsimple-version-selector-manage-jobs.md)]

## <a name="overview"></a><span data-ttu-id="dc2b5-104">Översikt</span><span class="sxs-lookup"><span data-stu-id="dc2b5-104">Overview</span></span>
<span data-ttu-id="dc2b5-105">Den **jobb** innehåller en enda central portal för visning och hantera jobb som har startats på enheter anslutna till din StorSimple Manager-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="dc2b5-105">The **Jobs** page provides a single central portal for viewing and managing jobs that were started on devices connected to your StorSimple Manager service.</span></span> <span data-ttu-id="dc2b5-106">Du kan visa schemalagda, körs, slutförda och misslyckade jobb för flera enheter.</span><span class="sxs-lookup"><span data-stu-id="dc2b5-106">You can view scheduled, running, completed, and failed jobs for multiple devices.</span></span> <span data-ttu-id="dc2b5-107">Resultat visas i tabellformat.</span><span class="sxs-lookup"><span data-stu-id="dc2b5-107">Results are presented in a tabular format.</span></span> 

![Sidan jobb](./media/storsimple-manage-jobs/HCS_JobsPage.png)

<span data-ttu-id="dc2b5-109">Du kan snabbt hitta de jobb som du är intresserad av genom att filtrera på fält som:</span><span class="sxs-lookup"><span data-stu-id="dc2b5-109">You can quickly find the jobs you are interested in by filtering on fields such as:</span></span>

* <span data-ttu-id="dc2b5-110">**Status för** – jobb kan köras schemalagda misslyckades, slutförda, stoppar eller avbröts.</span><span class="sxs-lookup"><span data-stu-id="dc2b5-110">**Status** – Jobs can be running, scheduled, failed, completed, canceling, or canceled.</span></span>
* <span data-ttu-id="dc2b5-111">**Typen** – jobb kan skapas på grund av en schemalagd eller en säkerhetskopiering på begäran (**ta säkerhetskopiering**), kloning, en återställning av enheten, eller en uppdatering.</span><span class="sxs-lookup"><span data-stu-id="dc2b5-111">**Type** – Jobs can be created as a result of a scheduled or an on-demand backup (**Take Backup**), cloning, a device restore, or an update operation.</span></span>
* <span data-ttu-id="dc2b5-112">**Enheter** – jobb startas på en viss enhet ansluten till din tjänst.</span><span class="sxs-lookup"><span data-stu-id="dc2b5-112">**Devices** – Jobs are initiated on a certain device connected to your service.</span></span>
* <span data-ttu-id="dc2b5-113">**Från och till** – jobb kan filtreras baserat på det datum och tid intervallet.</span><span class="sxs-lookup"><span data-stu-id="dc2b5-113">**From and To** – Jobs can be filtered based on the date and time range.</span></span>

<span data-ttu-id="dc2b5-114">Filtrerade jobben visas sedan som en tabell på grundval av följande attribut:</span><span class="sxs-lookup"><span data-stu-id="dc2b5-114">The filtered jobs are then tabulated on the basis of the following attributes:</span></span>

* <span data-ttu-id="dc2b5-115">**Typen** – säkerhetskopiering, kloning, återställning, redundans eller update.</span><span class="sxs-lookup"><span data-stu-id="dc2b5-115">**Type** – Backup, clone, restore, failover, or update.</span></span>
* <span data-ttu-id="dc2b5-116">**Status för** – kör schemalagda, misslyckades, slutfört, avbryter eller avbrutits.</span><span class="sxs-lookup"><span data-stu-id="dc2b5-116">**Status** – Running, scheduled, failed, completed, canceling, or canceled.</span></span>
* <span data-ttu-id="dc2b5-117">**Entiteten** – jobb kan vara kopplad till en volym, en princip för säkerhetskopiering eller en enhet.</span><span class="sxs-lookup"><span data-stu-id="dc2b5-117">**Entity** – The jobs can be associated with a volume, a backup policy, or a device.</span></span> <span data-ttu-id="dc2b5-118">Ett jobb för klon är kopplat till en volym, en schemalagd säkerhetskopiering är associerad med en princip för säkerhetskopiering.</span><span class="sxs-lookup"><span data-stu-id="dc2b5-118">A clone job is associated with a volume, whereas a scheduled backup job is associated with a backup policy.</span></span> <span data-ttu-id="dc2b5-119">En enhet jobb skapas på grund av en katastrofåterställning (DR) eller en återställning.</span><span class="sxs-lookup"><span data-stu-id="dc2b5-119">A device job is created as a result of a disaster recovery (DR) or a restore operation.</span></span>
* <span data-ttu-id="dc2b5-120">**Enheten** – namnet på den enhet som då jobbet startades.</span><span class="sxs-lookup"><span data-stu-id="dc2b5-120">**Device** – The name of the device on which the job was started.</span></span>
* <span data-ttu-id="dc2b5-121">**Igång på** – den tid då jobbet startades.</span><span class="sxs-lookup"><span data-stu-id="dc2b5-121">**Started On** – The time when the job was started.</span></span>
* <span data-ttu-id="dc2b5-122">**Förlopp** – procentandel slutförandet av ett jobb som körs.</span><span class="sxs-lookup"><span data-stu-id="dc2b5-122">**Progress** – The percentage completion of a running job.</span></span> <span data-ttu-id="dc2b5-123">Detta bör alltid vara 100% för slutförda jobb.</span><span class="sxs-lookup"><span data-stu-id="dc2b5-123">For a completed job, this should always be 100%.</span></span>

<span data-ttu-id="dc2b5-124">Lista över jobb uppdateras var 30: e sekund.</span><span class="sxs-lookup"><span data-stu-id="dc2b5-124">The list of jobs is refreshed every 30 seconds.</span></span>

<span data-ttu-id="dc2b5-125">Du kan utföra följande åtgärder för jobbrelaterade på den här sidan:</span><span class="sxs-lookup"><span data-stu-id="dc2b5-125">You can perform the following job-related actions on this page:</span></span>

* <span data-ttu-id="dc2b5-126">Visa jobbinformation</span><span class="sxs-lookup"><span data-stu-id="dc2b5-126">View job details</span></span>
* <span data-ttu-id="dc2b5-127">Avbryta ett jobb</span><span class="sxs-lookup"><span data-stu-id="dc2b5-127">Cancel a job</span></span>

## <a name="view-job-details"></a><span data-ttu-id="dc2b5-128">Visa jobbinformation</span><span class="sxs-lookup"><span data-stu-id="dc2b5-128">View job details</span></span>
<span data-ttu-id="dc2b5-129">Utför följande steg om du vill visa information om jobb.</span><span class="sxs-lookup"><span data-stu-id="dc2b5-129">Perform the following steps to view the details of any job.</span></span>

#### <a name="to-view-job-details"></a><span data-ttu-id="dc2b5-130">Visa jobbinformation</span><span class="sxs-lookup"><span data-stu-id="dc2b5-130">To view job details</span></span>
1. <span data-ttu-id="dc2b5-131">På den **jobb** kan du visa de jobb som du är intresserad av genom att köra en fråga med lämpliga filter.</span><span class="sxs-lookup"><span data-stu-id="dc2b5-131">On the **Jobs** page, display the job(s) you are interested in by running a query with appropriate filters.</span></span> <span data-ttu-id="dc2b5-132">Du kan söka för slutförd, kör eller avbröts jobb.</span><span class="sxs-lookup"><span data-stu-id="dc2b5-132">You can search for completed, running, or canceled jobs.</span></span>
2. <span data-ttu-id="dc2b5-133">Välj ett jobb.</span><span class="sxs-lookup"><span data-stu-id="dc2b5-133">Select a job.</span></span>
3. <span data-ttu-id="dc2b5-134">Längst ned på sidan klickar du på **information**.</span><span class="sxs-lookup"><span data-stu-id="dc2b5-134">At the bottom of the page, click **Details**.</span></span>
4. <span data-ttu-id="dc2b5-135">I den **säkerhetskopiering jobbinformation** dialogrutan kan du visa status, information, statistik och data statistik.</span><span class="sxs-lookup"><span data-stu-id="dc2b5-135">In the **Backup Job Details** dialog box, you can view the status, details, time statistics, and data statistics.</span></span>

## <a name="cancel-a-job"></a><span data-ttu-id="dc2b5-136">Avbryta ett jobb</span><span class="sxs-lookup"><span data-stu-id="dc2b5-136">Cancel a job</span></span>
<span data-ttu-id="dc2b5-137">Utför följande steg om du vill avbryta ett jobb som körs.</span><span class="sxs-lookup"><span data-stu-id="dc2b5-137">Perform the following steps to cancel a running job.</span></span>

### <a name="to-cancel-a-job"></a><span data-ttu-id="dc2b5-138">Avbryta ett jobb</span><span class="sxs-lookup"><span data-stu-id="dc2b5-138">To cancel a job</span></span>
1. <span data-ttu-id="dc2b5-139">På den **jobb** sidan, visa pågående jobb som du vill avbryta genom att köra en fråga med lämpliga filter.</span><span class="sxs-lookup"><span data-stu-id="dc2b5-139">On the **Jobs** page, display the running job(s) that you want to cancel by running a query with appropriate filters.</span></span>
2. <span data-ttu-id="dc2b5-140">Markera jobbet.</span><span class="sxs-lookup"><span data-stu-id="dc2b5-140">Select the job.</span></span>
3. <span data-ttu-id="dc2b5-141">Längst ned på sidan klickar du på **Avbryt**.</span><span class="sxs-lookup"><span data-stu-id="dc2b5-141">At the bottom of the page, click **Cancel**.</span></span>
4. <span data-ttu-id="dc2b5-142">Klicka på **Ja** när du uppmanas att bekräfta åtgärden.</span><span class="sxs-lookup"><span data-stu-id="dc2b5-142">When prompted for confirmation, click **Yes**.</span></span>

<span data-ttu-id="dc2b5-143">Det här jobbet avbryts nu.</span><span class="sxs-lookup"><span data-stu-id="dc2b5-143">This job is now canceled.</span></span>

## <a name="next-steps"></a><span data-ttu-id="dc2b5-144">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="dc2b5-144">Next steps</span></span>
* <span data-ttu-id="dc2b5-145">Lär dig hur du [hantera din StorSimple-säkerhetskopieringsprinciper](storsimple-manage-backup-policies.md).</span><span class="sxs-lookup"><span data-stu-id="dc2b5-145">Learn how to [manage your StorSimple backup policies](storsimple-manage-backup-policies.md).</span></span>
* <span data-ttu-id="dc2b5-146">Lär dig hur du [använda StorSimple Manager-tjänsten för att administrera din StorSimple-enhet](storsimple-manager-service-administration.md).</span><span class="sxs-lookup"><span data-stu-id="dc2b5-146">Learn how to [use the StorSimple Manager service to administer your StorSimple device](storsimple-manager-service-administration.md).</span></span>

