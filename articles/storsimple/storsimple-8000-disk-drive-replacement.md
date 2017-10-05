---
title: "Ersätta en enhet på en enhet för StorSimple 8000-serien | Microsoft Docs"
description: "Förklarar hur du ersätter en enhet på en primär StorSimple-enhet eller en EBOD bilaga."
services: storsimple
documentationcenter: 
author: alkohli
manager: timlt
editor: 
ms.assetid: 
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 073/2017
ms.author: alkohli
ms.openlocfilehash: bb259b626ecd4dcbaa8f1c465f1ece4516aa8881
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/29/2017
---
# <a name="replace-a-disk-drive-on-your-storsimple-8000-series-device"></a><span data-ttu-id="69ca8-103">Ersätta en diskenhet på enheten StorSimple 8000-serien</span><span class="sxs-lookup"><span data-stu-id="69ca8-103">Replace a disk drive on your StorSimple 8000 series device</span></span>

## <a name="overview"></a><span data-ttu-id="69ca8-104">Översikt</span><span class="sxs-lookup"><span data-stu-id="69ca8-104">Overview</span></span>
<span data-ttu-id="69ca8-105">Den här självstudiekursen beskrivs hur du kan ta bort och ersätter en felaktig eller misslyckade hårddisk på en Microsoft Azure StorSimple-enhet.</span><span class="sxs-lookup"><span data-stu-id="69ca8-105">This tutorial explains how you can remove and replace a malfunctioning or failed hard disk drive on a Microsoft Azure StorSimple device.</span></span> <span data-ttu-id="69ca8-106">Om du vill ersätta en enhet, måste du:</span><span class="sxs-lookup"><span data-stu-id="69ca8-106">To replace a disk drive, you need to:</span></span>

* <span data-ttu-id="69ca8-107">Koppla ur antitamper låset</span><span class="sxs-lookup"><span data-stu-id="69ca8-107">Disengage the antitamper lock</span></span>
* <span data-ttu-id="69ca8-108">Ta bort enheten</span><span class="sxs-lookup"><span data-stu-id="69ca8-108">Remove the disk drive</span></span>
* <span data-ttu-id="69ca8-109">Installera diskenhet ersättning</span><span class="sxs-lookup"><span data-stu-id="69ca8-109">Install the replacement disk drive</span></span>

> [!IMPORTANT]
> <span data-ttu-id="69ca8-110">Innan du tar bort och ersätter en diskenhet granska säkerhetsinformation i [ersättning av StorSimple maskinvara komponenten](storsimple-8000-hardware-component-replacement.md).</span><span class="sxs-lookup"><span data-stu-id="69ca8-110">Before removing and replacing a disk drive, review the safety information in [StorSimple hardware component replacement](storsimple-8000-hardware-component-replacement.md).</span></span>
 

## <a name="disengage-the-antitamper-lock"></a><span data-ttu-id="69ca8-111">Koppla ur antitamper låset</span><span class="sxs-lookup"><span data-stu-id="69ca8-111">Disengage the antitamper lock</span></span>
<span data-ttu-id="69ca8-112">Här beskrivs hur antitamper lås på din StorSimple-enhet kan vara aktiverad eller inaktiverad när du ersätter diskenheter.</span><span class="sxs-lookup"><span data-stu-id="69ca8-112">This procedure explains how the antitamper locks on your StorSimple device can be engaged or disengaged when you replace the disk drives.</span></span> <span data-ttu-id="69ca8-113">Antitamper Lås monteras i enheten operatör hanterar och de kan nås via en liten öppning i avsnittet spärren i referensen.</span><span class="sxs-lookup"><span data-stu-id="69ca8-113">The antitamper locks are fitted in the drive carrier handles, and they are accessed through a small aperture in the latch section of the handle.</span></span> <span data-ttu-id="69ca8-114">Enheter anges med lås som den låsta position.</span><span class="sxs-lookup"><span data-stu-id="69ca8-114">Drives are supplied with the locks set to the locked position.</span></span>

#### <a name="to-unlock-the-antitamper-lock"></a><span data-ttu-id="69ca8-115">Att låsa upp antitamper låset</span><span class="sxs-lookup"><span data-stu-id="69ca8-115">To unlock the antitamper lock</span></span>
1. <span data-ttu-id="69ca8-116">Infoga noggrant på Lock (en ”tamperproof” T10 för som tillhandahålls av Microsoft) till öppning i referensen och till en socket.</span><span class="sxs-lookup"><span data-stu-id="69ca8-116">Carefully insert the lock key (a "tamperproof" T10 screwdriver that Microsoft provided) into the aperture in the handle and into its socket.</span></span> 
   
   <span data-ttu-id="69ca8-117">Röd indikator är synlig i öppning om antitamper lock är aktiverat.</span><span class="sxs-lookup"><span data-stu-id="69ca8-117">If the antitamper lock is activated, the red indicator is visible in the aperture.</span></span>
  
    ![Låst enhet](./media/storsimple-disk-drive-replacement/IC741056.png)
   
    <span data-ttu-id="69ca8-119">**Bild 1** anti manipulationsindikerande Lås används</span><span class="sxs-lookup"><span data-stu-id="69ca8-119">**Figure 1** Anti-tamper lock engaged</span></span>
   
   | <span data-ttu-id="69ca8-120">Etikett</span><span class="sxs-lookup"><span data-stu-id="69ca8-120">Label</span></span> | <span data-ttu-id="69ca8-121">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="69ca8-121">Description</span></span> |
   |:--- |:--- |
   | <span data-ttu-id="69ca8-122">1</span><span class="sxs-lookup"><span data-stu-id="69ca8-122">1</span></span> |<span data-ttu-id="69ca8-123">Öppning av indikator</span><span class="sxs-lookup"><span data-stu-id="69ca8-123">Indicator aperture</span></span> |
   | <span data-ttu-id="69ca8-124">2</span><span class="sxs-lookup"><span data-stu-id="69ca8-124">2</span></span> |<span data-ttu-id="69ca8-125">Antitamper Lås</span><span class="sxs-lookup"><span data-stu-id="69ca8-125">Antitamper lock</span></span> |
2. <span data-ttu-id="69ca8-126">Rotera nyckeln i en moturs riktning tills den röda indikatorn inte visas i öppning ovanför nyckeln.</span><span class="sxs-lookup"><span data-stu-id="69ca8-126">Rotate the key in an anticlockwise direction until the red indicator is not visible in the aperture above the key.</span></span>
3. <span data-ttu-id="69ca8-127">Ta bort nyckeln.</span><span class="sxs-lookup"><span data-stu-id="69ca8-127">Remove the key.</span></span>
   
    ![Olåsta diskenhet](./media/storsimple-disk-drive-replacement/IC741057.png)
   
    <span data-ttu-id="69ca8-129">**Bild 2** olåst diskenhet</span><span class="sxs-lookup"><span data-stu-id="69ca8-129">**Figure 2** Unlocked disk drive</span></span>
4. <span data-ttu-id="69ca8-130">Diskettenheten kan nu tas bort.</span><span class="sxs-lookup"><span data-stu-id="69ca8-130">The disk drive can now be removed.</span></span>

<span data-ttu-id="69ca8-131">Följ stegen i omvänd att låset.</span><span class="sxs-lookup"><span data-stu-id="69ca8-131">Follow the steps in reverse to engage the lock.</span></span>

## <a name="remove-the-disk-drive"></a><span data-ttu-id="69ca8-132">Ta bort enheten</span><span class="sxs-lookup"><span data-stu-id="69ca8-132">Remove the disk drive</span></span>
<span data-ttu-id="69ca8-133">Din StorSimple-enhet stöder en RAID 10-liknande blanksteg lagringskonfiguration.</span><span class="sxs-lookup"><span data-stu-id="69ca8-133">Your StorSimple device supports a RAID 10-like storage spaces configuration.</span></span> <span data-ttu-id="69ca8-134">Detta innebär att den fungerar normalt med en skadad disk SSD-enhet (SSD) eller hårddisk enhet (HDD).</span><span class="sxs-lookup"><span data-stu-id="69ca8-134">This implies that it can operate normally with one failed disk, solid-state drive (SSD), or hard disk drive (HDD).</span></span>

> [!IMPORTANT]
> * <span data-ttu-id="69ca8-135">Om systemet har mer än en felande disken, ta inte bort mer än en SSD och HDD från systemet när som helst i tid.</span><span class="sxs-lookup"><span data-stu-id="69ca8-135">If your system has more than one failed disk, do not remove more than one SSD or HDD from the system at any point in time.</span></span> <span data-ttu-id="69ca8-136">Detta kan resultera i förlust av data.</span><span class="sxs-lookup"><span data-stu-id="69ca8-136">Doing so could result in loss of data.</span></span>
> * <span data-ttu-id="69ca8-137">Se till att du placerar en ersättning SSD i en plats som tidigare har innehållit en SSD.</span><span class="sxs-lookup"><span data-stu-id="69ca8-137">Make sure that you place a replacement SSD in a slot that previously contained an SSD.</span></span> <span data-ttu-id="69ca8-138">Placera en annan Hårddisk på samma sätt i en plats som tidigare har innehållit en Hårddisk.</span><span class="sxs-lookup"><span data-stu-id="69ca8-138">Similarly, place a replacement HDD in a slot that previously contained an HDD.</span></span>
> * <span data-ttu-id="69ca8-139">I Azure-portalen fack numreras från 0 – 11.</span><span class="sxs-lookup"><span data-stu-id="69ca8-139">In the Azure portal, slots are numbered from 0 – 11.</span></span> <span data-ttu-id="69ca8-140">Därför om portalen visar att en disk i uttag 2 misslyckades på enheten, leta efter den skadade disken på tredje plats från den övre vänstra.</span><span class="sxs-lookup"><span data-stu-id="69ca8-140">Therefore, if the portal shows that a disk in slot 2 has failed, on the device, look for the failed disk in the third slot from the top left.</span></span>
> 
> 

<span data-ttu-id="69ca8-141">Enheter kan tas bort och ersätts när systemet körs.</span><span class="sxs-lookup"><span data-stu-id="69ca8-141">Drives can be removed and replaced while the system is operating.</span></span>

#### <a name="to-remove-a-drive"></a><span data-ttu-id="69ca8-142">Ta bort en enhet</span><span class="sxs-lookup"><span data-stu-id="69ca8-142">To remove a drive</span></span>
1. <span data-ttu-id="69ca8-143">Gå till din enhet för att identifiera den felande disken i Azure-portalen **Inställningar > maskinvara hälsa**.</span><span class="sxs-lookup"><span data-stu-id="69ca8-143">To identify the failed disk, in the Azure portal, go to your device **Settings > Hardware health**.</span></span> <span data-ttu-id="69ca8-144">Eftersom en disk kan misslyckas i primära höljet och/eller i en EBOD hölje (om du använder en 8600-modellen), se status för diskarna under **delade komponenter** och under **EBOD delade komponenter**.</span><span class="sxs-lookup"><span data-stu-id="69ca8-144">Because a disk can fail in the primary enclosure and/or in an EBOD enclosure (if you are using a 8600 model), look at the status of the disks under **Shared components** and under **EBOD shared components**.</span></span> <span data-ttu-id="69ca8-145">En disk som misslyckats i antingen hölje visas med röd status.</span><span class="sxs-lookup"><span data-stu-id="69ca8-145">A failed disk in either enclosure will be shown with a red status.</span></span>
2. <span data-ttu-id="69ca8-146">Hitta enheter först i primära höljet eller EBOD höljet.</span><span class="sxs-lookup"><span data-stu-id="69ca8-146">Locate the drives in the front of the primary enclosure or the EBOD enclosure.</span></span> 
3. <span data-ttu-id="69ca8-147">Om disken är olåst vidare till nästa steg.</span><span class="sxs-lookup"><span data-stu-id="69ca8-147">If the disk is unlocked, proceed to the next step.</span></span> <span data-ttu-id="69ca8-148">Om disken är låst kan låsa upp genom att följa proceduren i [kopplas ur antitamper låset](#disengage-the-antitamper-lock).</span><span class="sxs-lookup"><span data-stu-id="69ca8-148">If the disk is locked, unlock it by following the procedure in [Disengage the antitamper lock](#disengage-the-antitamper-lock).</span></span>
4. <span data-ttu-id="69ca8-149">Tryck på svart låset på enheten operatör modulen och dra enhet operatör referensen ut och framifrån chassit.</span><span class="sxs-lookup"><span data-stu-id="69ca8-149">Press the black latch on the drive carrier module and pull the drive carrier handle out and away from the front of the chassis.</span></span>
   
    ![Frigöra referensen diskenhet](./media/storsimple-disk-drive-replacement/IC741051.png)
   
    <span data-ttu-id="69ca8-151">**Bild 3** frigöra referensen för enhet</span><span class="sxs-lookup"><span data-stu-id="69ca8-151">**Figure 3** Releasing the drive handle</span></span>
5. <span data-ttu-id="69ca8-152">När enheten operatör referensen utökas fullständigt, dra en operatör enhet utanför chassit.</span><span class="sxs-lookup"><span data-stu-id="69ca8-152">When the drive carrier handle is fully extended, slide the drive carrier out of the chassis.</span></span> 
   
    ![Glidande disk diskenhet](./media/storsimple-disk-drive-replacement/IC741052.png)
   
    <span data-ttu-id="69ca8-154">**Bild 4** glidande diskenhet utanför en operatör</span><span class="sxs-lookup"><span data-stu-id="69ca8-154">**Figure 4** Sliding the disk drive out of the carrier</span></span>

## <a name="install-the-replacement-disk-drive"></a><span data-ttu-id="69ca8-155">Installera diskenhet ersättning</span><span class="sxs-lookup"><span data-stu-id="69ca8-155">Install the replacement disk drive</span></span>
<span data-ttu-id="69ca8-156">När en enhet har misslyckats i din StorSimple-enhet och har tagits bort, följer du proceduren för att ersätta den med en ny enhet.</span><span class="sxs-lookup"><span data-stu-id="69ca8-156">After a drive has failed in your StorSimple device and you have removed it, follow this procedure to replace it with a new drive.</span></span>

#### <a name="to-insert-a-drive"></a><span data-ttu-id="69ca8-157">Infoga en enhet</span><span class="sxs-lookup"><span data-stu-id="69ca8-157">To insert a drive</span></span>
1. <span data-ttu-id="69ca8-158">Se till att enheten operatör referensen utökas helt, enligt följande bild.</span><span class="sxs-lookup"><span data-stu-id="69ca8-158">Ensure the drive carrier handle is fully extended, as shown in the following image.</span></span>
   
    ![Enhet med referensen utökad](./media/storsimple-disk-drive-replacement/IC741044.png)
   
    <span data-ttu-id="69ca8-160">**Bild 5** enheten med referensen utökad</span><span class="sxs-lookup"><span data-stu-id="69ca8-160">**Figure 5** Drive with handle extended</span></span>
2. <span data-ttu-id="69ca8-161">Skjut enhet operatör ända i chassit.</span><span class="sxs-lookup"><span data-stu-id="69ca8-161">Slide the drive carrier all the way into the chassis.</span></span>
   
    ![Glidande disk i enhet operatör](./media/storsimple-disk-drive-replacement/IC741045.png)
   
    <span data-ttu-id="69ca8-163">**Bild 6** glidande enhet operatör i chassit</span><span class="sxs-lookup"><span data-stu-id="69ca8-163">**Figure 6**  Sliding the drive carrier into the chassis</span></span>
3. <span data-ttu-id="69ca8-164">Stäng enheten operatör referensen med en enhet operatör som infogas när fortsätter att skicka en enhet operatör till chassit, tills enheten operatör referensen fästs i låst läge.</span><span class="sxs-lookup"><span data-stu-id="69ca8-164">With the drive carrier inserted, close the drive carrier handle while continuing to push the drive carrier into the chassis, until the drive carrier handle snaps into a locked position.</span></span>
4. <span data-ttu-id="69ca8-165">Använd på LOCK som angavs av Microsoft (tamperproof Torx för) operatör referensen till rätt plats genom att aktivera Lås skruven Stäng för ett kvartal medurs.</span><span class="sxs-lookup"><span data-stu-id="69ca8-165">Use the lock key that was provided by Microsoft (tamperproof Torx screwdriver) to secure the carrier handle into place by turning the lock screw a quarter turn clockwise.</span></span>
5. <span data-ttu-id="69ca8-166">Kontrollera att ersättningen lyckades och att enheten är i drift.</span><span class="sxs-lookup"><span data-stu-id="69ca8-166">Verify that the replacement was successful and the drive is operational.</span></span> <span data-ttu-id="69ca8-167">Åtkomst till Azure-portalen och gå till **inställningar** > **maskinvara hälsa**.</span><span class="sxs-lookup"><span data-stu-id="69ca8-167">Access the Azure portal and navigate to **Settings** > **Hardware health**.</span></span> <span data-ttu-id="69ca8-168">Under **delade komponenter** eller **EBOD delade komponenter**, status för enheten ska vara grön, som anger att den är felfri.</span><span class="sxs-lookup"><span data-stu-id="69ca8-168">Under **Shared components** or **EBOD shared components**, the drive status should be green, indicating that it is healthy.</span></span>
<!---Loc Comment: It seems it should say "Device settings > Hardware health" instead of "Settings > Hardware health"---->
   
   > [!NOTE]
   > <span data-ttu-id="69ca8-169">Det kan ta flera timmar diskstatusen blir grönt efter ersättningen.</span><span class="sxs-lookup"><span data-stu-id="69ca8-169">It may take several hours for the disk status to turn green after the replacement.</span></span>
  
## <a name="next-steps"></a><span data-ttu-id="69ca8-170">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="69ca8-170">Next steps</span></span>
<span data-ttu-id="69ca8-171">Lär dig mer om [ersättning av StorSimple maskinvara komponenten](storsimple-8000-hardware-component-replacement.md).</span><span class="sxs-lookup"><span data-stu-id="69ca8-171">Learn more about [StorSimple hardware component replacement](storsimple-8000-hardware-component-replacement.md).</span></span>

