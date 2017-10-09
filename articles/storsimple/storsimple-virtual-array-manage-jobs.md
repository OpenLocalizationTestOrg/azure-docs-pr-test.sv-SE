---
title: aaaView och hantera virtuella StorSimple-matris jobb | Microsoft Docs
description: "Beskriver hello StorSimple Enhetshanteraren jobb sida och hur toouse den tootrack senaste och aktuella jobb för hello virtuella StorSimple-matris."
services: storsimple
documentationcenter: NA
author: alkohli
manager: carmonm
editor: 
ms.assetid: 31879821-b599-4609-a7f4-d4b0f658a933
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: na
ms.date: 11/11/2016
ms.author: alkohli
ms.openlocfilehash: cf3f3e7bcdfff0ff2328b7354db2482286800e93
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="use-hello-storsimple-device-manager-service-tooview-jobs-for-hello-storsimple-virtual-array"></a><span data-ttu-id="b8a5f-103">Använda hello StorSimple Enhetshanteraren service tooview jobb för hello virtuella StorSimple-matris</span><span class="sxs-lookup"><span data-stu-id="b8a5f-103">Use hello StorSimple Device Manager service tooview jobs for hello StorSimple Virtual Array</span></span>
## <a name="overview"></a><span data-ttu-id="b8a5f-104">Översikt</span><span class="sxs-lookup"><span data-stu-id="b8a5f-104">Overview</span></span>
<span data-ttu-id="b8a5f-105">Hej **jobb** bladet innehåller en enda central portal för att visa och hantera jobb som har startats på virtuella lagringsmatriser som är anslutna tooyour StorSimple enheten Manager-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="b8a5f-105">hello **Jobs** blade provides a single central portal for viewing and managing jobs that are started on virtual arrays that are connected tooyour StorSimple Device Manager service.</span></span> <span data-ttu-id="b8a5f-106">Du kan visa körs slutförda och misslyckade jobb för flera virtuella enheter.</span><span class="sxs-lookup"><span data-stu-id="b8a5f-106">You can view running, completed, and failed jobs for multiple virtual devices.</span></span> <span data-ttu-id="b8a5f-107">Resultat visas i tabellformat.</span><span class="sxs-lookup"><span data-stu-id="b8a5f-107">Results are presented in a tabular format.</span></span>

![Jobb-bladet](./media/storsimple-virtual-array-manage-jobs/ova-jobs-blade.png)

<span data-ttu-id="b8a5f-109">Du kan snabbt hitta hello jobb som du är intresserad av genom att filtrera på fält som:</span><span class="sxs-lookup"><span data-stu-id="b8a5f-109">You can quickly find hello jobs you are interested in by filtering on fields such as:</span></span>

* <span data-ttu-id="b8a5f-110">**Tidsintervallet** – jobb kan filtreras baserat på hello datum och ett tidsintervall.</span><span class="sxs-lookup"><span data-stu-id="b8a5f-110">**Time range** – Jobs can be filtered based on hello date and time range.</span></span>
* <span data-ttu-id="b8a5f-111">**Enheter** – jobb startas på en specifik enhet ansluten tooyour tjänst.</span><span class="sxs-lookup"><span data-stu-id="b8a5f-111">**Devices** – Jobs are initiated on a specific device connected tooyour service.</span></span> <span data-ttu-id="b8a5f-112">hello filtrerade jobben visas sedan som en tabell baserad på hello följande attribut:</span><span class="sxs-lookup"><span data-stu-id="b8a5f-112">hello filtered jobs are then tabulated based on hello following attributes:</span></span>
  
  * <span data-ttu-id="b8a5f-113">**Namnet** – hello Jobbnamnet kan vara **alla**, **säkerhetskopiering**, **klona**, **växla över**, **hämta uppdateringar** , eller **installera uppdateringar**.</span><span class="sxs-lookup"><span data-stu-id="b8a5f-113">**Name** – hello job name can be **All**, **Backup**, **Clone**, **Fail over**, **Download updates**, or **Install updates**.</span></span>
  * <span data-ttu-id="b8a5f-114">**Status för** – jobb kan vara **alla**, **pågår**, **lyckades**, eller **misslyckades**, eller **avbruten**.</span><span class="sxs-lookup"><span data-stu-id="b8a5f-114">**Status** – Jobs can be **All**, **In progress**, **Succeeded**, or **Failed**, or **Canceled**.</span></span>
  * <span data-ttu-id="b8a5f-115">**Entiteten** – hello jobb kan vara kopplad till en volym, resurs eller enhet.</span><span class="sxs-lookup"><span data-stu-id="b8a5f-115">**Entity** – hello jobs can be associated with a volume, share, or device.</span></span>
  * <span data-ttu-id="b8a5f-116">**Enheten** – hello namn på hello enhet på vilken hello-jobbet startades.</span><span class="sxs-lookup"><span data-stu-id="b8a5f-116">**Device** – hello name of hello device on which hello job was started.</span></span>
  * <span data-ttu-id="b8a5f-117">**Startats på** – hello tid då hello-jobbet startades.</span><span class="sxs-lookup"><span data-stu-id="b8a5f-117">**Started on** – hello time when hello job was started.</span></span>
  * <span data-ttu-id="b8a5f-118">**Varaktighet** – hello varaktighet för i vilken hello jobbet kördes.</span><span class="sxs-lookup"><span data-stu-id="b8a5f-118">**Duration** – hello duration for on which hello job was run.</span></span>
* <span data-ttu-id="b8a5f-119">**Status för** – du kan söka efter alla, körs slutförda och misslyckade jobb.</span><span class="sxs-lookup"><span data-stu-id="b8a5f-119">**Status** – You can search for all, running, completed, or failed jobs.</span></span>
* <span data-ttu-id="b8a5f-120">**Jobbtyp** – hello jobbtypen kan all, säkerhetskopiering, återställning, redundans, ladda ned uppdateringar, eller installera uppdateringar.</span><span class="sxs-lookup"><span data-stu-id="b8a5f-120">**Job type** – hello job type can be all, backup, restore, failover, download updates, or install updates.</span></span>

<span data-ttu-id="b8a5f-121">hello lista över jobb uppdateras var 30: e sekund.</span><span class="sxs-lookup"><span data-stu-id="b8a5f-121">hello list of jobs is refreshed every 30 seconds.</span></span>

## <a name="view-job-details"></a><span data-ttu-id="b8a5f-122">Visa jobbinformation</span><span class="sxs-lookup"><span data-stu-id="b8a5f-122">View job details</span></span>
<span data-ttu-id="b8a5f-123">Utföra hello följande steg tooview hello information för alla jobb.</span><span class="sxs-lookup"><span data-stu-id="b8a5f-123">Perform hello following steps tooview hello details of any job.</span></span>

#### <a name="tooview-job-details"></a><span data-ttu-id="b8a5f-124">tooview jobbinformation</span><span class="sxs-lookup"><span data-stu-id="b8a5f-124">tooview job details</span></span>
1. <span data-ttu-id="b8a5f-125">På hello **jobb** bladet, visa hello jobb som du är intresserad av genom att köra en fråga med lämpliga filter.</span><span class="sxs-lookup"><span data-stu-id="b8a5f-125">On hello **Jobs** blade, display hello job(s) you are interested in by running a query with appropriate filters.</span></span> <span data-ttu-id="b8a5f-126">Du kan söka efter slutförda eller inte körs jobb.</span><span class="sxs-lookup"><span data-stu-id="b8a5f-126">You can search for completed or running jobs.</span></span>
2. <span data-ttu-id="b8a5f-127">Välj ett jobb hello tabular listan över jobb.</span><span class="sxs-lookup"><span data-stu-id="b8a5f-127">Select a job from hello tabular list of jobs.</span></span>
   
    ![Jobb-bladet](./media/storsimple-virtual-array-manage-jobs/ova-jobs-blade.png)
3. <span data-ttu-id="b8a5f-129">Hello längst hello-sidan, klickar du på **information**.</span><span class="sxs-lookup"><span data-stu-id="b8a5f-129">At hello bottom of hello page, click **Details**.</span></span>
4. <span data-ttu-id="b8a5f-130">I hello **information** dialogrutan kan du visa status, detaljer och statistik.</span><span class="sxs-lookup"><span data-stu-id="b8a5f-130">In hello **Details** dialog box, you can view status, details, and time statistics.</span></span> <span data-ttu-id="b8a5f-131">hello följande bild visar ett exempel på hello **säkerhetskopiering jobbinformation** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="b8a5f-131">hello following illustration shows an example of hello **Backup Job Details** dialog box.</span></span>
   
    ![Jobbinformation](./media/storsimple-virtual-array-manage-jobs/ova-jobs-details.png)

#### <a name="job-failures-when-hello-virtual-machine-is-paused-in-hello-hypervisor"></a><span data-ttu-id="b8a5f-133">Jobbfel när hello virtuella datorn är pausad i hello hypervisor-program</span><span class="sxs-lookup"><span data-stu-id="b8a5f-133">Job failures when hello virtual machine is paused in hello hypervisor</span></span>
<span data-ttu-id="b8a5f-134">När ett jobb är i statusen för din virtuella StorSimple-matris och hello enhet (virtuell dator etablerats i hypervisor-program) har pausats av fler än 15 minuter, hello jobb misslyckas.</span><span class="sxs-lookup"><span data-stu-id="b8a5f-134">When a job is in progress on your StorSimple Virtual Array and hello device (virtual machine provisioned in hypervisor) is paused for greater than 15 minutes, hello job fails.</span></span> <span data-ttu-id="b8a5f-135">Detta på grund av tooyour virtuella StorSimple-matris tid inte är synkroniserad med hello Microsoft Azure.</span><span class="sxs-lookup"><span data-stu-id="b8a5f-135">This is due tooyour StorSimple Virtual Array time being out of sync with hello Microsoft Azure time.</span></span> 

<span data-ttu-id="b8a5f-136">Du ser hello följande fel: ”din enhet tid är inte synkroniserat med Microsoft Azure hello med mer än 15 minuter.</span><span class="sxs-lookup"><span data-stu-id="b8a5f-136">You will see hello following error: "Your device time is out of sync with hello Microsoft Azure time by more than 15 minutes.</span></span> <span data-ttu-id="b8a5f-137">Se till att hello hypervisor och hello enheten gånger synkroniseras med en NTP-servern.</span><span class="sxs-lookup"><span data-stu-id="b8a5f-137">Ensure that hello hypervisor and hello device times are synchronized with an NTP servier.</span></span> <span data-ttu-id="b8a5f-138">Kontrollera att det inte finns några anslutningsproblem.</span><span class="sxs-lookup"><span data-stu-id="b8a5f-138">Verify that there are no connectivity issues.</span></span> <span data-ttu-id="b8a5f-139">tootroubleshoot anslutningsproblem kör diagnostiktest från hello lokala webbgränssnittet för din virtuella enhet ”.</span><span class="sxs-lookup"><span data-stu-id="b8a5f-139">tootroubleshoot connectivity issues, run diagnostic tests from hello local web UI of your virtual device."</span></span>

<span data-ttu-id="b8a5f-140">Dessa fel gäller toobackup, återställning, uppdatering och växling vid fel jobb.</span><span class="sxs-lookup"><span data-stu-id="b8a5f-140">These failures apply toobackup, restore, update, and failover jobs.</span></span> <span data-ttu-id="b8a5f-141">Om den virtuella datorn har etablerats i Hyper-V, synkroniserar hello datorn slutligen tid med din hypervisor.</span><span class="sxs-lookup"><span data-stu-id="b8a5f-141">If your virtual machine is provisioned in Hyper-V, hello machine eventually synchronizes time with your hypervisor.</span></span> <span data-ttu-id="b8a5f-142">När det händer kan du starta om jobbet.</span><span class="sxs-lookup"><span data-stu-id="b8a5f-142">Once that happens, you can restart your job.</span></span>

## <a name="next-steps"></a><span data-ttu-id="b8a5f-143">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="b8a5f-143">Next steps</span></span>
<span data-ttu-id="b8a5f-144">[Lär dig hur hello toouse lokala web UI tooadminister din virtuella StorSimple-matris](storsimple-ova-web-ui-admin.md).</span><span class="sxs-lookup"><span data-stu-id="b8a5f-144">[Learn how toouse hello local web UI tooadminister your StorSimple Virtual Array](storsimple-ova-web-ui-admin.md).</span></span>

