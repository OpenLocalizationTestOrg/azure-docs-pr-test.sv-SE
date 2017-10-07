---
title: aaaClone StorSimple-volym | Microsoft Docs
description: "Beskriver hello klona av olika typer och när toouse dem, och förklarar hur du kan använda en säkerhetskopia tooclone en enskild volym."
services: storsimple
documentationcenter: NA
author: alkohli
manager: carmonm
editor: 
ms.assetid: b5d615f2-02a7-4222-9867-6c0385ce748c
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 08/17/2016
ms.author: alkohli
ms.openlocfilehash: e98d28db1abeb515ba78ab5860e7c5eba0dfcbb5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="use-hello-storsimple-manager-service-tooclone-a-volume"></a><span data-ttu-id="25fc5-103">Använd hello StorSimple Manager-tjänsten tooclone en volym</span><span class="sxs-lookup"><span data-stu-id="25fc5-103">Use hello StorSimple Manager service tooclone a volume</span></span>
[!INCLUDE [storsimple-version-selector-clone-volume](../../includes/storsimple-version-selector-clone-volume.md)]

## <a name="overview"></a><span data-ttu-id="25fc5-104">Översikt</span><span class="sxs-lookup"><span data-stu-id="25fc5-104">Overview</span></span>
<span data-ttu-id="25fc5-105">Hej StorSimple Manager-tjänsten **säkerhetskopieringskatalogen** sidan visas alla hello säkerhetskopior som skapas när manuell eller automatisk säkerhetskopiering utförs.</span><span class="sxs-lookup"><span data-stu-id="25fc5-105">hello StorSimple Manager service **Backup Catalog** page displays all hello backup sets that are created when manual or automated backups are taken.</span></span> <span data-ttu-id="25fc5-106">Du kan använda den här sidan toolist alla hello säkerhetskopior för en princip för säkerhetskopiering eller en volym, Välj eller ta bort eller använda en säkerhetskopiering toorestore eller klona en volym.</span><span class="sxs-lookup"><span data-stu-id="25fc5-106">You can use this page toolist all hello backups for a backup policy or a volume, select or delete backups, or use a backup toorestore or clone a volume.</span></span>

![Säkerhetskopieringskatalogen sida](./media/storsimple-clone-volume/HCS_BackupCatalog.png)  

<span data-ttu-id="25fc5-108">Den här självstudiekursen beskrivs hur du kan använda en säkerhetskopia tooclone en enskild volym.</span><span class="sxs-lookup"><span data-stu-id="25fc5-108">This tutorial describes how you can use a backup set tooclone an individual volume.</span></span> <span data-ttu-id="25fc5-109">Här förklaras också hello skillnaden mellan *tillfälligt* och *permanent* klonar.</span><span class="sxs-lookup"><span data-stu-id="25fc5-109">It also explains hello difference between *transient* and *permanent* clones.</span></span> 

## <a name="create-a-clone-of-a-volume"></a><span data-ttu-id="25fc5-110">Skapa en klon av en volym</span><span class="sxs-lookup"><span data-stu-id="25fc5-110">Create a clone of a volume</span></span>
<span data-ttu-id="25fc5-111">Du kan skapa en klon av hello samma enhet, en annan enhet eller även en virtuell dator med hjälp av en lokal eller en ögonblicksbild i molnet.</span><span class="sxs-lookup"><span data-stu-id="25fc5-111">You can create a clone on hello same device, another device, or even a virtual machine by using a local or a cloud snapshot.</span></span>

#### <a name="tooclone-a-volume"></a><span data-ttu-id="25fc5-112">tooclone en volym</span><span class="sxs-lookup"><span data-stu-id="25fc5-112">tooclone a volume</span></span>
1. <span data-ttu-id="25fc5-113">På hello StorSimple Manager-tjänsten klickar du på hello **säkerhetskopieringskatalog** fliken och markera en säkerhetskopia.</span><span class="sxs-lookup"><span data-stu-id="25fc5-113">On hello StorSimple Manager service page, click hello **Backup catalog** tab and select a backup set.</span></span>
2. <span data-ttu-id="25fc5-114">Expandera hello säkerhetskopia tooview hello associerade volymer.</span><span class="sxs-lookup"><span data-stu-id="25fc5-114">Expand hello backup set tooview hello associated volumes.</span></span> <span data-ttu-id="25fc5-115">Klicka och välj en volym från hello säkerhetskopia.</span><span class="sxs-lookup"><span data-stu-id="25fc5-115">Click and select a volume from hello backup set.</span></span>
   
     ![Klona en volym](./media/storsimple-clone-volume/HCS_Clone.png) 
3. <span data-ttu-id="25fc5-117">Klicka på **klona** toobegin kloning hello valda volymen.</span><span class="sxs-lookup"><span data-stu-id="25fc5-117">Click **Clone** toobegin cloning hello selected volume.</span></span>
4. <span data-ttu-id="25fc5-118">Hello klona volymen i guiden under **ange namn och plats**:</span><span class="sxs-lookup"><span data-stu-id="25fc5-118">In hello Clone Volume wizard, under **Specify name and location**:</span></span>
   
   1. <span data-ttu-id="25fc5-119">Identifiera en målenhet.</span><span class="sxs-lookup"><span data-stu-id="25fc5-119">Identify a target device.</span></span> <span data-ttu-id="25fc5-120">Detta är hello plats där hello klona kommer att skapas.</span><span class="sxs-lookup"><span data-stu-id="25fc5-120">This is hello location where hello clone will be created.</span></span> <span data-ttu-id="25fc5-121">Du kan välja hello samma enhet eller ange en annan enhet.</span><span class="sxs-lookup"><span data-stu-id="25fc5-121">You can choose hello same device or specify another device.</span></span> <span data-ttu-id="25fc5-122">Om du väljer en volym som är associerade med andra leverantörer av molntjänster (inte Azure) hello nedrullningsbara lista för hello målenhet visas endast fysiska enheter.</span><span class="sxs-lookup"><span data-stu-id="25fc5-122">If you choose a volume associated with other cloud service providers (not Azure), hello drop-down list for hello target device will only show physical devices.</span></span> <span data-ttu-id="25fc5-123">Du kan inte klona en volym som är associerade med andra molntjänstleverantörer på en virtuell enhet.</span><span class="sxs-lookup"><span data-stu-id="25fc5-123">You cannot clone a volume associated with other cloud service providers on a virtual device.</span></span>
      
      > [!NOTE]
      > <span data-ttu-id="25fc5-124">Kontrollera att hello kapacitet som krävs för hello klona är lägre än hello kapacitet på hello målenhet.</span><span class="sxs-lookup"><span data-stu-id="25fc5-124">Make sure that hello capacity required for hello clone is lower than hello capacity available on hello target device.</span></span>
      > 
      > 
   2. <span data-ttu-id="25fc5-125">Ange ett unikt volymnamn för din kloning.</span><span class="sxs-lookup"><span data-stu-id="25fc5-125">Specify a unique volume name for your clone.</span></span> <span data-ttu-id="25fc5-126">hello-namnet måste innehålla mellan 3 och 127 tecken.</span><span class="sxs-lookup"><span data-stu-id="25fc5-126">hello name must contain between 3 and 127 characters.</span></span>
   3. <span data-ttu-id="25fc5-127">Klicka på pilikonen hello</span><span class="sxs-lookup"><span data-stu-id="25fc5-127">Click hello arrow icon</span></span> ![pilikon](./media/storsimple-clone-volume/HCS_ArrowIcon.png) <span data-ttu-id="25fc5-129">tooproceed toohello nästa sida.</span><span class="sxs-lookup"><span data-stu-id="25fc5-129">tooproceed toohello next page.</span></span>
5. <span data-ttu-id="25fc5-130">Under **ange värdar som kan använda den här volymen**:</span><span class="sxs-lookup"><span data-stu-id="25fc5-130">Under **Specify hosts that can use this volume**:</span></span>
   
   1. <span data-ttu-id="25fc5-131">Ange en åtkomstkontrollpost (ACR) för hello kloning.</span><span class="sxs-lookup"><span data-stu-id="25fc5-131">Specify an access control record (ACR) for hello clone.</span></span> <span data-ttu-id="25fc5-132">Du kan lägga till en ny ACR eller välj hello befintlig lista.</span><span class="sxs-lookup"><span data-stu-id="25fc5-132">You can add a new ACR or choose from hello existing list.</span></span>
   2. <span data-ttu-id="25fc5-133">Klicka på kryssikonen hello</span><span class="sxs-lookup"><span data-stu-id="25fc5-133">Click hello check icon</span></span> ![kryssikon](./media/storsimple-clone-volume/HCS_CheckIcon.png)<span data-ttu-id="25fc5-135">toocomplete hello-åtgärden.</span><span class="sxs-lookup"><span data-stu-id="25fc5-135">toocomplete hello operation.</span></span>
6. <span data-ttu-id="25fc5-136">Ett jobb för klon initieras och du meddelas när hello klona har skapats.</span><span class="sxs-lookup"><span data-stu-id="25fc5-136">A clone job will be initiated and you will be notified when hello clone is successfully created.</span></span> <span data-ttu-id="25fc5-137">Klicka på **visa jobb** toomonitor hello klona jobb på hello **jobb** sidan.</span><span class="sxs-lookup"><span data-stu-id="25fc5-137">Click **View Job** toomonitor hello clone job on hello **Jobs** page.</span></span>
7. <span data-ttu-id="25fc5-138">Efter hello har klona jobbet slutförts:</span><span class="sxs-lookup"><span data-stu-id="25fc5-138">After hello clone job is completed:</span></span>
   
   1. <span data-ttu-id="25fc5-139">Gå toohello **enheter** sida och välj hello **Volymbehållare** fliken.</span><span class="sxs-lookup"><span data-stu-id="25fc5-139">Go toohello **Devices** page, and select hello **Volume Containers** tab.</span></span> 
   2. <span data-ttu-id="25fc5-140">Välj hello volymbehållare som är associerad med hello källvolymen som du har klonat.</span><span class="sxs-lookup"><span data-stu-id="25fc5-140">Select hello volume container that is associated with hello source volume that you cloned.</span></span> <span data-ttu-id="25fc5-141">I hello lista över volymer, bör du se hello klon som nyss skapades.</span><span class="sxs-lookup"><span data-stu-id="25fc5-141">In hello list of volumes, you should see hello clone that was just created.</span></span>

> [!NOTE]
> <span data-ttu-id="25fc5-142">Övervakning och standard säkerhetskopieringen inaktiveras automatiskt på en klonade volym.</span><span class="sxs-lookup"><span data-stu-id="25fc5-142">Monitoring and default backup are automatically disabled on a cloned volume.</span></span>
> 
> 

<span data-ttu-id="25fc5-143">En klon som har skapats på detta sätt är ett tillfälligt kloning.</span><span class="sxs-lookup"><span data-stu-id="25fc5-143">A clone that is created this way is a transient clone.</span></span> <span data-ttu-id="25fc5-144">Mer information om kloning typer finns [tillfälliga och permanenta kloner](#transient-vs-permanent-clones).</span><span class="sxs-lookup"><span data-stu-id="25fc5-144">For more information about clone types, see [Transient vs. permanent clones](#transient-vs-permanent-clones).</span></span>

<span data-ttu-id="25fc5-145">Den här klonen är nu en vanlig volym och åtgärder som är möjligt på en volym är tillgänglig för hello kloning.</span><span class="sxs-lookup"><span data-stu-id="25fc5-145">This clone is now a regular volume, and any operation that is possible on a volume will be available for hello clone.</span></span> <span data-ttu-id="25fc5-146">Du behöver tooconfigure volymen för säkerhetskopior.</span><span class="sxs-lookup"><span data-stu-id="25fc5-146">You will need tooconfigure this volume for any backups.</span></span>

## <a name="transient-vs-permanent-clones"></a><span data-ttu-id="25fc5-147">Tillfälligt kontra permanent kloner</span><span class="sxs-lookup"><span data-stu-id="25fc5-147">Transient vs. permanent clones</span></span>
<span data-ttu-id="25fc5-148">Tillfälliga och permanenta kloner skapas bara när du klona på tooa annan enhet.</span><span class="sxs-lookup"><span data-stu-id="25fc5-148">Transient and permanent clones are created only when you are cloning on tooa different device.</span></span> <span data-ttu-id="25fc5-149">Du kan också klona en viss volym från en säkerhetskopia tooa annan enhet.</span><span class="sxs-lookup"><span data-stu-id="25fc5-149">You can clone a specific volume from a backup set tooa different device.</span></span> <span data-ttu-id="25fc5-150">En klon som skapats i det här sättet är en *tillfälligt* kloning.</span><span class="sxs-lookup"><span data-stu-id="25fc5-150">A clone created in this way is a *transient* clone.</span></span> <span data-ttu-id="25fc5-151">hello tillfälligt klona har referenser toohello ursprungliga volymen och använder den volymen tooread vid skrivning till lokalt.</span><span class="sxs-lookup"><span data-stu-id="25fc5-151">hello transient clone will have references toohello original volume and will use that volume tooread while writing locally.</span></span> 

<span data-ttu-id="25fc5-152">När du har tagit en ögonblicksbild i molnet med tillfälliga klonen hello resulterande klona blir en *permanent* kloning.</span><span class="sxs-lookup"><span data-stu-id="25fc5-152">After you take a cloud snapshot of a transient clone, hello resulting clone will be a *permanent* clone.</span></span> <span data-ttu-id="25fc5-153">hello permanent klona är oberoende och har inte några referenser toohello ursprungliga volymen som den har klonats från.</span><span class="sxs-lookup"><span data-stu-id="25fc5-153">hello permanent clone is independent and doesn’t have any references toohello original volume that it was cloned from.</span></span>  

## <a name="scenarios-for-transient-and-permanent-clones"></a><span data-ttu-id="25fc5-154">Scenarier för tillfälliga och permanenta kloner</span><span class="sxs-lookup"><span data-stu-id="25fc5-154">Scenarios for transient and permanent clones</span></span>
<span data-ttu-id="25fc5-155">hello följande avsnitt beskrivs exempel situationer där tillfälliga och permanenta kloner kan användas.</span><span class="sxs-lookup"><span data-stu-id="25fc5-155">hello following sections describe example situations in which transient and permanent clones can be used.</span></span>

### <a name="item-level-recovery-with-a-transient-clone"></a><span data-ttu-id="25fc5-156">Återställning på objektnivå med en tillfälliga klon</span><span class="sxs-lookup"><span data-stu-id="25fc5-156">Item-level recovery with a transient clone</span></span>
<span data-ttu-id="25fc5-157">Du måste toorecover en en-år-gamla Microsoft PowerPoint-fil.</span><span class="sxs-lookup"><span data-stu-id="25fc5-157">You need toorecover a one-year-old Microsoft PowerPoint presentation file.</span></span> <span data-ttu-id="25fc5-158">IT-administratören identifierar hello säkerhetskopieringen från den aktuella tidsperioden och sedan filter hello volym.</span><span class="sxs-lookup"><span data-stu-id="25fc5-158">Your IT administrator identifies hello specific backup from that time frame, and then filters hello volume.</span></span> <span data-ttu-id="25fc5-159">Hej administratör sedan klonar volymen hello hittar hello-fil som du söker efter och ger den tooyou.</span><span class="sxs-lookup"><span data-stu-id="25fc5-159">hello administrator then clones hello volume, locates hello file that you are looking for, and provides it tooyou.</span></span> <span data-ttu-id="25fc5-160">I det här scenariot används ett tillfälligt kloning.</span><span class="sxs-lookup"><span data-stu-id="25fc5-160">In this scenario, a transient clone is used.</span></span> 

<span data-ttu-id="25fc5-161">![Video tillgänglig](./media/storsimple-clone-volume/Video_icon.png) **Video tillgänglig**</span><span class="sxs-lookup"><span data-stu-id="25fc5-161">![Video available](./media/storsimple-clone-volume/Video_icon.png) **Video available**</span></span>

<span data-ttu-id="25fc5-162">toowatch en video som visar hur du kan använda hello klona och återställa funktioner i StorSimple toorecover borttagna filer, klickar du på [här](https://azure.microsoft.com/documentation/videos/storsimple-recover-deleted-files-with-storsimple/).</span><span class="sxs-lookup"><span data-stu-id="25fc5-162">toowatch a video that demonstrates how you can use hello clone and restore features in StorSimple toorecover deleted files, click [here](https://azure.microsoft.com/documentation/videos/storsimple-recover-deleted-files-with-storsimple/).</span></span>

### <a name="testing-in-hello-production-environment-with-a-permanent-clone"></a><span data-ttu-id="25fc5-163">Testa i produktionsmiljö för hello med en permanent klon</span><span class="sxs-lookup"><span data-stu-id="25fc5-163">Testing in hello production environment with a permanent clone</span></span>
<span data-ttu-id="25fc5-164">Du måste tooverify ett tester programfel i hello-produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="25fc5-164">You need tooverify a testing bug in hello production environment.</span></span> <span data-ttu-id="25fc5-165">Du kan skapa en klon av hello volym i produktionsmiljö hello genom att ta en ögonblicksbild i molnet för den här klonen.</span><span class="sxs-lookup"><span data-stu-id="25fc5-165">You create a clone of hello volume in hello production environment by taking a cloud snapshot of this clone.</span></span> <span data-ttu-id="25fc5-166">hello är klonade volymen nu oberoende.</span><span class="sxs-lookup"><span data-stu-id="25fc5-166">hello cloned volume is now independent.</span></span> <span data-ttu-id="25fc5-167">I det här scenariot används en permanent kloning.</span><span class="sxs-lookup"><span data-stu-id="25fc5-167">In this scenario, a permanent clone is used.</span></span>

## <a name="next-steps"></a><span data-ttu-id="25fc5-168">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="25fc5-168">Next steps</span></span>
* <span data-ttu-id="25fc5-169">Lär dig hur för[återställer en StorSimple-volym från en säkerhetskopia](storsimple-restore-from-backup-set.md).</span><span class="sxs-lookup"><span data-stu-id="25fc5-169">Learn how too[restore a StorSimple volume from a backup set](storsimple-restore-from-backup-set.md).</span></span>
* <span data-ttu-id="25fc5-170">Lär dig hur för[Använd hello StorSimple Manager service tooadminister StorSimple-enheten](storsimple-manager-service-administration.md).</span><span class="sxs-lookup"><span data-stu-id="25fc5-170">Learn how too[use hello StorSimple Manager service tooadminister your StorSimple device](storsimple-manager-service-administration.md).</span></span>

