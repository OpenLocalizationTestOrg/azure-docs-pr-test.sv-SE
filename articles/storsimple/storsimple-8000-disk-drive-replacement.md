---
title: "aaaReplace en enhet på en enhet för StorSimple 8000-serien | Microsoft Docs"
description: "Förklarar hur tooreplace en disk enhet på en primär StorSimple-enhet eller en EBOD bilaga."
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
ms.openlocfilehash: 09c1bf5e97adf54a609a868e921351bc63e00a83
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="replace-a-disk-drive-on-your-storsimple-8000-series-device"></a><span data-ttu-id="5093c-103">Ersätta en diskenhet på enheten StorSimple 8000-serien</span><span class="sxs-lookup"><span data-stu-id="5093c-103">Replace a disk drive on your StorSimple 8000 series device</span></span>

## <a name="overview"></a><span data-ttu-id="5093c-104">Översikt</span><span class="sxs-lookup"><span data-stu-id="5093c-104">Overview</span></span>
<span data-ttu-id="5093c-105">Den här självstudiekursen beskrivs hur du kan ta bort och ersätter en felaktig eller misslyckade hårddisk på en Microsoft Azure StorSimple-enhet.</span><span class="sxs-lookup"><span data-stu-id="5093c-105">This tutorial explains how you can remove and replace a malfunctioning or failed hard disk drive on a Microsoft Azure StorSimple device.</span></span> <span data-ttu-id="5093c-106">tooreplace en enhet, måste du:</span><span class="sxs-lookup"><span data-stu-id="5093c-106">tooreplace a disk drive, you need to:</span></span>

* <span data-ttu-id="5093c-107">Koppla ur hello antitamper Lås</span><span class="sxs-lookup"><span data-stu-id="5093c-107">Disengage hello antitamper lock</span></span>
* <span data-ttu-id="5093c-108">Ta bort hello diskenhet</span><span class="sxs-lookup"><span data-stu-id="5093c-108">Remove hello disk drive</span></span>
* <span data-ttu-id="5093c-109">Installera hello ersättning diskenhet</span><span class="sxs-lookup"><span data-stu-id="5093c-109">Install hello replacement disk drive</span></span>

> [!IMPORTANT]
> <span data-ttu-id="5093c-110">Innan du tar bort och ersätter en diskenhet granska hello säkerhetsinformation i [ersättning av StorSimple maskinvara komponenten](storsimple-8000-hardware-component-replacement.md).</span><span class="sxs-lookup"><span data-stu-id="5093c-110">Before removing and replacing a disk drive, review hello safety information in [StorSimple hardware component replacement](storsimple-8000-hardware-component-replacement.md).</span></span>
 

## <a name="disengage-hello-antitamper-lock"></a><span data-ttu-id="5093c-111">Koppla ur hello antitamper Lås</span><span class="sxs-lookup"><span data-stu-id="5093c-111">Disengage hello antitamper lock</span></span>
<span data-ttu-id="5093c-112">Här beskrivs hur hello antitamper lås på din StorSimple-enhet kan vara aktiverad eller inaktiverad när du ersätter hello diskenheter.</span><span class="sxs-lookup"><span data-stu-id="5093c-112">This procedure explains how hello antitamper locks on your StorSimple device can be engaged or disengaged when you replace hello disk drives.</span></span> <span data-ttu-id="5093c-113">Hej antitamper Lås monteras i hello enhet operatör hanterar och de kan nås via en liten öppning under hello spärren hello referensen.</span><span class="sxs-lookup"><span data-stu-id="5093c-113">hello antitamper locks are fitted in hello drive carrier handles, and they are accessed through a small aperture in hello latch section of hello handle.</span></span> <span data-ttu-id="5093c-114">Enheter levereras med hello Lås set toohello låst position.</span><span class="sxs-lookup"><span data-stu-id="5093c-114">Drives are supplied with hello locks set toohello locked position.</span></span>

#### <a name="toounlock-hello-antitamper-lock"></a><span data-ttu-id="5093c-115">toounlock hello antitamper Lås</span><span class="sxs-lookup"><span data-stu-id="5093c-115">toounlock hello antitamper lock</span></span>
1. <span data-ttu-id="5093c-116">Infoga noggrant hello lock-tangenten (en ”tamperproof” T10 för som tillhandahålls av Microsoft) till hello öppning i hello referensen och till en socket.</span><span class="sxs-lookup"><span data-stu-id="5093c-116">Carefully insert hello lock key (a "tamperproof" T10 screwdriver that Microsoft provided) into hello aperture in hello handle and into its socket.</span></span> 
   
   <span data-ttu-id="5093c-117">Hello Röd indikator är synlig i hello öppning om hello antitamper lock är aktiverat.</span><span class="sxs-lookup"><span data-stu-id="5093c-117">If hello antitamper lock is activated, hello red indicator is visible in hello aperture.</span></span>
  
    ![Låst enhet](./media/storsimple-disk-drive-replacement/IC741056.png)
   
    <span data-ttu-id="5093c-119">**Bild 1** anti manipulationsindikerande Lås används</span><span class="sxs-lookup"><span data-stu-id="5093c-119">**Figure 1** Anti-tamper lock engaged</span></span>
   
   | <span data-ttu-id="5093c-120">Etikett</span><span class="sxs-lookup"><span data-stu-id="5093c-120">Label</span></span> | <span data-ttu-id="5093c-121">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="5093c-121">Description</span></span> |
   |:--- |:--- |
   | <span data-ttu-id="5093c-122">1</span><span class="sxs-lookup"><span data-stu-id="5093c-122">1</span></span> |<span data-ttu-id="5093c-123">Öppning av indikator</span><span class="sxs-lookup"><span data-stu-id="5093c-123">Indicator aperture</span></span> |
   | <span data-ttu-id="5093c-124">2</span><span class="sxs-lookup"><span data-stu-id="5093c-124">2</span></span> |<span data-ttu-id="5093c-125">Antitamper Lås</span><span class="sxs-lookup"><span data-stu-id="5093c-125">Antitamper lock</span></span> |
2. <span data-ttu-id="5093c-126">Rotera hello nyckel i en moturs riktning tills hello red indikatorn inte visas i hello öppning ovan hello nyckel.</span><span class="sxs-lookup"><span data-stu-id="5093c-126">Rotate hello key in an anticlockwise direction until hello red indicator is not visible in hello aperture above hello key.</span></span>
3. <span data-ttu-id="5093c-127">Ta bort hello nyckel.</span><span class="sxs-lookup"><span data-stu-id="5093c-127">Remove hello key.</span></span>
   
    ![Olåsta diskenhet](./media/storsimple-disk-drive-replacement/IC741057.png)
   
    <span data-ttu-id="5093c-129">**Bild 2** olåst diskenhet</span><span class="sxs-lookup"><span data-stu-id="5093c-129">**Figure 2** Unlocked disk drive</span></span>
4. <span data-ttu-id="5093c-130">hello diskenhet kan nu tas bort.</span><span class="sxs-lookup"><span data-stu-id="5093c-130">hello disk drive can now be removed.</span></span>

<span data-ttu-id="5093c-131">Hello åtgärderna i omvänd tooengage hello Lås.</span><span class="sxs-lookup"><span data-stu-id="5093c-131">Follow hello steps in reverse tooengage hello lock.</span></span>

## <a name="remove-hello-disk-drive"></a><span data-ttu-id="5093c-132">Ta bort hello diskenhet</span><span class="sxs-lookup"><span data-stu-id="5093c-132">Remove hello disk drive</span></span>
<span data-ttu-id="5093c-133">Din StorSimple-enhet stöder en RAID 10-liknande blanksteg lagringskonfiguration.</span><span class="sxs-lookup"><span data-stu-id="5093c-133">Your StorSimple device supports a RAID 10-like storage spaces configuration.</span></span> <span data-ttu-id="5093c-134">Detta innebär att den fungerar normalt med en skadad disk SSD-enhet (SSD) eller hårddisk enhet (HDD).</span><span class="sxs-lookup"><span data-stu-id="5093c-134">This implies that it can operate normally with one failed disk, solid-state drive (SSD), or hard disk drive (HDD).</span></span>

> [!IMPORTANT]
> * <span data-ttu-id="5093c-135">Om systemet har mer än en felande disken, ta inte bort mer än en SSD och HDD från hello system när som helst i tid.</span><span class="sxs-lookup"><span data-stu-id="5093c-135">If your system has more than one failed disk, do not remove more than one SSD or HDD from hello system at any point in time.</span></span> <span data-ttu-id="5093c-136">Detta kan resultera i förlust av data.</span><span class="sxs-lookup"><span data-stu-id="5093c-136">Doing so could result in loss of data.</span></span>
> * <span data-ttu-id="5093c-137">Se till att du placerar en ersättning SSD i en plats som tidigare har innehållit en SSD.</span><span class="sxs-lookup"><span data-stu-id="5093c-137">Make sure that you place a replacement SSD in a slot that previously contained an SSD.</span></span> <span data-ttu-id="5093c-138">Placera en annan Hårddisk på samma sätt i en plats som tidigare har innehållit en Hårddisk.</span><span class="sxs-lookup"><span data-stu-id="5093c-138">Similarly, place a replacement HDD in a slot that previously contained an HDD.</span></span>
> * <span data-ttu-id="5093c-139">I hello Azure-portalen, fack numreras från 0 – 11.</span><span class="sxs-lookup"><span data-stu-id="5093c-139">In hello Azure portal, slots are numbered from 0 – 11.</span></span> <span data-ttu-id="5093c-140">Därför om hello portal visar att en disk i uttag 2 misslyckades på hello enheten, leta efter hello disken hello tredje plats från hello övre vänstra.</span><span class="sxs-lookup"><span data-stu-id="5093c-140">Therefore, if hello portal shows that a disk in slot 2 has failed, on hello device, look for hello failed disk in hello third slot from hello top left.</span></span>
> 
> 

<span data-ttu-id="5093c-141">Enheter kan tas bort och ersätts medan hello system är igång.</span><span class="sxs-lookup"><span data-stu-id="5093c-141">Drives can be removed and replaced while hello system is operating.</span></span>

#### <a name="tooremove-a-drive"></a><span data-ttu-id="5093c-142">tooremove en enhet</span><span class="sxs-lookup"><span data-stu-id="5093c-142">tooremove a drive</span></span>
1. <span data-ttu-id="5093c-143">tooidentify hello misslyckades disk i hello Azure-portalen gå tooyour enhet **Inställningar > maskinvara hälsa**.</span><span class="sxs-lookup"><span data-stu-id="5093c-143">tooidentify hello failed disk, in hello Azure portal, go tooyour device **Settings > Hardware health**.</span></span> <span data-ttu-id="5093c-144">Eftersom en disk kan misslyckas i hello primära hölje och/eller i en EBOD hölje (om du använder en 8600-modellen), se hello status för hello diskar under **delade komponenter** och under **EBOD delade komponenter** .</span><span class="sxs-lookup"><span data-stu-id="5093c-144">Because a disk can fail in hello primary enclosure and/or in an EBOD enclosure (if you are using a 8600 model), look at hello status of hello disks under **Shared components** and under **EBOD shared components**.</span></span> <span data-ttu-id="5093c-145">En disk som misslyckats i antingen hölje visas med röd status.</span><span class="sxs-lookup"><span data-stu-id="5093c-145">A failed disk in either enclosure will be shown with a red status.</span></span>
2. <span data-ttu-id="5093c-146">Leta upp hello enheter i hello framför hello primära hölje eller hello EBOD hölje.</span><span class="sxs-lookup"><span data-stu-id="5093c-146">Locate hello drives in hello front of hello primary enclosure or hello EBOD enclosure.</span></span> 
3. <span data-ttu-id="5093c-147">Om det är olåst hello disk kan fortsätta toohello nästa steg.</span><span class="sxs-lookup"><span data-stu-id="5093c-147">If hello disk is unlocked, proceed toohello next step.</span></span> <span data-ttu-id="5093c-148">Om hello disk är låst kan låsa upp genom att följa proceduren hello i [kopplas ur hello antitamper Lås](#disengage-the-antitamper-lock).</span><span class="sxs-lookup"><span data-stu-id="5093c-148">If hello disk is locked, unlock it by following hello procedure in [Disengage hello antitamper lock](#disengage-the-antitamper-lock).</span></span>
4. <span data-ttu-id="5093c-149">Tryck på hello svart lås på hello enhet operatör modulen och dra hello enhet operatör handtaget ut och direkt från hello framför hello chassi.</span><span class="sxs-lookup"><span data-stu-id="5093c-149">Press hello black latch on hello drive carrier module and pull hello drive carrier handle out and away from hello front of hello chassis.</span></span>
   
    ![Frigöra referensen diskenhet](./media/storsimple-disk-drive-replacement/IC741051.png)
   
    <span data-ttu-id="5093c-151">**Bild 3** frisläppning hello enhet referensen</span><span class="sxs-lookup"><span data-stu-id="5093c-151">**Figure 3** Releasing hello drive handle</span></span>
5. <span data-ttu-id="5093c-152">När hello enhet operatör referensen är helt utsträckt bild hello enhet operatör utanför hello chassi.</span><span class="sxs-lookup"><span data-stu-id="5093c-152">When hello drive carrier handle is fully extended, slide hello drive carrier out of hello chassis.</span></span> 
   
    ![Glidande disk diskenhet](./media/storsimple-disk-drive-replacement/IC741052.png)
   
    <span data-ttu-id="5093c-154">**Bild 4** glidande hello diskenhet utanför hello operatör</span><span class="sxs-lookup"><span data-stu-id="5093c-154">**Figure 4** Sliding hello disk drive out of hello carrier</span></span>

## <a name="install-hello-replacement-disk-drive"></a><span data-ttu-id="5093c-155">Installera hello ersättning diskenhet</span><span class="sxs-lookup"><span data-stu-id="5093c-155">Install hello replacement disk drive</span></span>
<span data-ttu-id="5093c-156">När en enhet har misslyckats i din StorSimple-enhet och har tagits bort, så den här proceduren tooreplace den med en ny enhet.</span><span class="sxs-lookup"><span data-stu-id="5093c-156">After a drive has failed in your StorSimple device and you have removed it, follow this procedure tooreplace it with a new drive.</span></span>

#### <a name="tooinsert-a-drive"></a><span data-ttu-id="5093c-157">tooinsert en enhet</span><span class="sxs-lookup"><span data-stu-id="5093c-157">tooinsert a drive</span></span>
1. <span data-ttu-id="5093c-158">Se till att hello enhet operatör referensen utökas helt, enligt följande bild hello.</span><span class="sxs-lookup"><span data-stu-id="5093c-158">Ensure hello drive carrier handle is fully extended, as shown in hello following image.</span></span>
   
    ![Enhet med referensen utökad](./media/storsimple-disk-drive-replacement/IC741044.png)
   
    <span data-ttu-id="5093c-160">**Bild 5** enheten med referensen utökad</span><span class="sxs-lookup"><span data-stu-id="5093c-160">**Figure 5** Drive with handle extended</span></span>
2. <span data-ttu-id="5093c-161">Skjut hello enhet operatör alla hello sätt i hello chassi.</span><span class="sxs-lookup"><span data-stu-id="5093c-161">Slide hello drive carrier all hello way into hello chassis.</span></span>
   
    ![Glidande disk i enhet operatör](./media/storsimple-disk-drive-replacement/IC741045.png)
   
    <span data-ttu-id="5093c-163">**Bild 6** glidande hello enhet operatör i hello chassi</span><span class="sxs-lookup"><span data-stu-id="5093c-163">**Figure 6**  Sliding hello drive carrier into hello chassis</span></span>
3. <span data-ttu-id="5093c-164">Med hello enhet operatör infogade, Stäng hello enhet operatör referensen när fortsätter toopush hello enhet operatör i hello chassi tills hello enhet operatör referensen fästs i låst läge.</span><span class="sxs-lookup"><span data-stu-id="5093c-164">With hello drive carrier inserted, close hello drive carrier handle while continuing toopush hello drive carrier into hello chassis, until hello drive carrier handle snaps into a locked position.</span></span>
4. <span data-ttu-id="5093c-165">Använd hello Lås nyckel som tillhandahölls av Microsoft (tamperproof Torx för) toosecure hello operatör referensen till rätt plats genom att aktivera hello Lås skruv Stäng för ett kvartal medurs.</span><span class="sxs-lookup"><span data-stu-id="5093c-165">Use hello lock key that was provided by Microsoft (tamperproof Torx screwdriver) toosecure hello carrier handle into place by turning hello lock screw a quarter turn clockwise.</span></span>
5. <span data-ttu-id="5093c-166">Kontrollera att hello ersättning lyckades och hello enheten är i drift.</span><span class="sxs-lookup"><span data-stu-id="5093c-166">Verify that hello replacement was successful and hello drive is operational.</span></span> <span data-ttu-id="5093c-167">Komma åt hello Azure-portalen och navigera för**inställningar** > **maskinvara hälsa**.</span><span class="sxs-lookup"><span data-stu-id="5093c-167">Access hello Azure portal and navigate too**Settings** > **Hardware health**.</span></span> <span data-ttu-id="5093c-168">Under **delade komponenter** eller **EBOD delade komponenter**, status för hello enheten ska vara grön, som anger att den är felfri.</span><span class="sxs-lookup"><span data-stu-id="5093c-168">Under **Shared components** or **EBOD shared components**, hello drive status should be green, indicating that it is healthy.</span></span>
<!---Loc Comment: It seems it should say "Device settings > Hardware health" instead of "Settings > Hardware health"---->
   
   > [!NOTE]
   > <span data-ttu-id="5093c-169">Det kan ta flera timmar hello disk status tooturn grön efter hello ersättning.</span><span class="sxs-lookup"><span data-stu-id="5093c-169">It may take several hours for hello disk status tooturn green after hello replacement.</span></span>
  
## <a name="next-steps"></a><span data-ttu-id="5093c-170">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="5093c-170">Next steps</span></span>
<span data-ttu-id="5093c-171">Lär dig mer om [ersättning av StorSimple maskinvara komponenten](storsimple-8000-hardware-component-replacement.md).</span><span class="sxs-lookup"><span data-stu-id="5093c-171">Learn more about [StorSimple hardware component replacement](storsimple-8000-hardware-component-replacement.md).</span></span>

