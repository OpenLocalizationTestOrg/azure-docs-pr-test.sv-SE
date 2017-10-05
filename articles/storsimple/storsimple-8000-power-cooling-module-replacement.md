---
title: "Ersätta en PCM på enheten StorSimple 8000-serien | Microsoft Docs"
description: "Beskriver hur du tar bort och ersätter ström och kylning modul (PCM) på din StorSimple-enhet"
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
ms.date: 06/02/2017
ms.author: alkohli
ms.openlocfilehash: 7d181e6e434c998573dbea4b541cfacf7a28ee66
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="replace-a-power-and-cooling-module-on-your-storsimple-device"></a><span data-ttu-id="0aee8-103">Ersätta en ström och kylning modulen på din StorSimple-enhet</span><span class="sxs-lookup"><span data-stu-id="0aee8-103">Replace a Power and Cooling Module on your StorSimple device</span></span>
## <a name="overview"></a><span data-ttu-id="0aee8-104">Översikt</span><span class="sxs-lookup"><span data-stu-id="0aee8-104">Overview</span></span>
<span data-ttu-id="0aee8-105">Ström och kylning modul (PCM) i Microsoft Azure StorSimple-enheten består av en strömförsörjning och kylfläktar som styrs från den primära servern och EBOD höljen.</span><span class="sxs-lookup"><span data-stu-id="0aee8-105">The Power and Cooling Module (PCM) in your Microsoft Azure StorSimple device consists of a power supply and cooling fans that are controlled through the primary and EBOD enclosures.</span></span> <span data-ttu-id="0aee8-106">Det finns endast en modell för PCM som är certifierad för varje enhet.</span><span class="sxs-lookup"><span data-stu-id="0aee8-106">There is only one model of PCM that is certified for each enclosure.</span></span> <span data-ttu-id="0aee8-107">Primär höljet är certifierad för en 764 W PCM och EBOD höljet är certifierad för en 580 W PCM.</span><span class="sxs-lookup"><span data-stu-id="0aee8-107">The primary enclosure is certified for a 764 W PCM and the EBOD enclosure is certified for a 580 W PCM.</span></span> <span data-ttu-id="0aee8-108">Även om PCMs för primära höljet och EBOD höljet är olika, är ersättning proceduren identiska.</span><span class="sxs-lookup"><span data-stu-id="0aee8-108">Although the PCMs for the primary enclosure and the EBOD enclosure are different, the replacement procedure is identical.</span></span>

<span data-ttu-id="0aee8-109">Den här självstudiekursen beskrivs hur du:</span><span class="sxs-lookup"><span data-stu-id="0aee8-109">This tutorial explains how to:</span></span>

* <span data-ttu-id="0aee8-110">Ta bort en PCM</span><span class="sxs-lookup"><span data-stu-id="0aee8-110">Remove a PCM</span></span>
* <span data-ttu-id="0aee8-111">Installera en ersättning PCM</span><span class="sxs-lookup"><span data-stu-id="0aee8-111">Install a replacement PCM</span></span>

> [!IMPORTANT]
> <span data-ttu-id="0aee8-112">Innan du tar bort och ersätter en PCM granska säkerhetsinformation i [ersättning av StorSimple maskinvara komponenten](storsimple-8000-hardware-component-replacement.md).</span><span class="sxs-lookup"><span data-stu-id="0aee8-112">Before removing and replacing a PCM, review the safety information in [StorSimple hardware component replacement](storsimple-8000-hardware-component-replacement.md).</span></span>


## <a name="before-you-replace-a-pcm"></a><span data-ttu-id="0aee8-113">Innan du ersätter en PCM</span><span class="sxs-lookup"><span data-stu-id="0aee8-113">Before you replace a PCM</span></span>
<span data-ttu-id="0aee8-114">Tänk på följande viktiga problem innan du ersätter din PCM:</span><span class="sxs-lookup"><span data-stu-id="0aee8-114">Be aware of the following important issues before you replace your PCM:</span></span>

* <span data-ttu-id="0aee8-115">Om PCM-strömförsörjning misslyckas, lämna felaktiga installerat modulen, men ta bort strömsladd.</span><span class="sxs-lookup"><span data-stu-id="0aee8-115">If the power supply of the PCM fails, leave the faulty module installed, but remove the power cord.</span></span> <span data-ttu-id="0aee8-116">Fläkten fortsätter att ta emot ström från höljet och fortsätta att tillhandahålla rätt kylning.</span><span class="sxs-lookup"><span data-stu-id="0aee8-116">The fan will continue to receive power from the enclosure and continue to provide proper cooling.</span></span> <span data-ttu-id="0aee8-117">Om fläkten misslyckas, måste PCM bytas omedelbart.</span><span class="sxs-lookup"><span data-stu-id="0aee8-117">If the fan fails, the PCM needs to be replaced immediately.</span></span>
* <span data-ttu-id="0aee8-118">Innan du tar bort PCM koppla kraften från PCM genom att inaktivera den huvudsakliga växeln (om sådan finns) eller genom att ta bort strömsladd fysiskt.</span><span class="sxs-lookup"><span data-stu-id="0aee8-118">Before removing the PCM, disconnect the power from the PCM by turning off the main switch (where present) or by physically removing the power cord.</span></span> <span data-ttu-id="0aee8-119">Detta ger en varning om att datorn att stängas power är nära förestående.</span><span class="sxs-lookup"><span data-stu-id="0aee8-119">This provides a warning to your system that a power shutdown is imminent.</span></span>
* <span data-ttu-id="0aee8-120">Kontrollera att den andra PCM fungerar för fortsatt systemfunktioner innan du ersätter felaktiga PCM.</span><span class="sxs-lookup"><span data-stu-id="0aee8-120">Make sure that the other PCM is functional for continued system operation before replacing the faulty PCM.</span></span> <span data-ttu-id="0aee8-121">En felaktig PCM måste ersättas med en fullt fungerande PCM så snart som möjligt.</span><span class="sxs-lookup"><span data-stu-id="0aee8-121">A faulty PCM must be replaced by a fully operational PCM as soon as possible.</span></span>
* <span data-ttu-id="0aee8-122">PCM modulen ersättning tar bara några minuter att slutföra, men den måste slutföras inom 10 minuter på att ta bort misslyckade PCM för att förhindra överhettning.</span><span class="sxs-lookup"><span data-stu-id="0aee8-122">PCM module replacement takes only few minutes to complete, but it must be completed within 10 minutes of removing the failed PCM to prevent overheating.</span></span>
* <span data-ttu-id="0aee8-123">Observera att ersättning 764 W PCM moduler skickas från fabriken inte innehåller batterimodulen säkerhetskopiering.</span><span class="sxs-lookup"><span data-stu-id="0aee8-123">Note that the replacement 764 W PCM modules shipped from the factory do not contain the backup battery module.</span></span> <span data-ttu-id="0aee8-124">Du måste ta bort batteriet från din felaktiga PCM och infoga det i modulen innan du utför ersättningen ersättning.</span><span class="sxs-lookup"><span data-stu-id="0aee8-124">You will need to remove the battery from your faulty PCM and then insert it into the replacement module prior to performing the replacement.</span></span> <span data-ttu-id="0aee8-125">Mer information finns i så här [ta bort och infoga en reserv batteri modul](storsimple-8000-battery-replacement.md).</span><span class="sxs-lookup"><span data-stu-id="0aee8-125">For more information, see how to [remove and insert a backup battery module](storsimple-8000-battery-replacement.md).</span></span>

## <a name="remove-a-pcm"></a><span data-ttu-id="0aee8-126">Ta bort en PCM</span><span class="sxs-lookup"><span data-stu-id="0aee8-126">Remove a PCM</span></span>
<span data-ttu-id="0aee8-127">Följ dessa instruktioner när du är redo att ta bort en ström och kylning modul (PCM) från Microsoft Azure StorSimple-enhet.</span><span class="sxs-lookup"><span data-stu-id="0aee8-127">Follow these instructions when you are ready to remove a Power and Cooling Module (PCM) from your Microsoft Azure StorSimple device.</span></span>

> [!NOTE]
> <span data-ttu-id="0aee8-128">Innan du tar bort din PCM kontrollerar du att rätt ersätter (764 W för primära höljet) eller 580 W för EBOD höljet.</span><span class="sxs-lookup"><span data-stu-id="0aee8-128">Before you remove your PCM, verify that you have a correct replacement (764 W for the primary enclosure or 580 W for the EBOD enclosure).</span></span>

#### <a name="to-remove-a-pcm"></a><span data-ttu-id="0aee8-129">Ta bort en PCM</span><span class="sxs-lookup"><span data-stu-id="0aee8-129">To remove a PCM</span></span>
1. <span data-ttu-id="0aee8-130">I den klassiska Azure-portalen klickar du på **Inställningar > övervaka > maskinvara hälsa**.</span><span class="sxs-lookup"><span data-stu-id="0aee8-130">In the Azure classic portal, click **Settings > Monitor > Hardware health**.</span></span> <span data-ttu-id="0aee8-131">Kontrollera statusen för komponenterna PCM under **delade komponenter** att identifiera vilket PCM misslyckades:</span><span class="sxs-lookup"><span data-stu-id="0aee8-131">Check the status of the PCM components under **Shared components** to identify which PCM has failed:</span></span>
   
   * <span data-ttu-id="0aee8-132">Om en strömförsörjning i PCM 0 har misslyckats, status för **strömförsörjning i PCM 0** blir röd.</span><span class="sxs-lookup"><span data-stu-id="0aee8-132">If a power supply in PCM 0 has failed, the status of **Power Supply in PCM 0** will be red.</span></span>
   * <span data-ttu-id="0aee8-133">Om en strömförsörjning i PCM 1 har misslyckats, status för **strömförsörjning i PCM 1** blir röd.</span><span class="sxs-lookup"><span data-stu-id="0aee8-133">If a power supply in PCM 1 has failed, the status of **Power Supply in PCM 1** will be red.</span></span>
   * <span data-ttu-id="0aee8-134">Om fläkten i PCM 1 har inte statusen för antingen **kylning 0 för PCM 0** eller **kylning 1 för PCM 0** blir röd.</span><span class="sxs-lookup"><span data-stu-id="0aee8-134">If the fan in PCM 1 has failed, the status of either **Cooling 0 for PCM 0** or **Cooling 1 for PCM 0** will be red.</span></span>
2. <span data-ttu-id="0aee8-135">Leta upp den misslyckade PCM på baksidan av primära höljet.</span><span class="sxs-lookup"><span data-stu-id="0aee8-135">Locate the failed PCM on the back of the primary enclosure.</span></span> <span data-ttu-id="0aee8-136">Om du kör en 8600-modell, identifiera primära höljet genom att titta på System enhet ID-numret visas på Kontrollpanelen LED-skärmen.</span><span class="sxs-lookup"><span data-stu-id="0aee8-136">If you are running an 8600 model, identify the primary enclosure by looking at the System Unit Identification Number shown on the front panel LED display.</span></span> <span data-ttu-id="0aee8-137">Standard enhets-ID som visas på det primära höljet är **00**, medan standardvärdet enhets-ID som visas på höljet EBOD är **01**.</span><span class="sxs-lookup"><span data-stu-id="0aee8-137">The default Unit ID displayed on the primary enclosure is **00**, whereas the default Unit ID displayed on the EBOD enclosure is **01**.</span></span> <span data-ttu-id="0aee8-138">I följande diagram och tabell förklarar frontpanel LED-skärmen.</span><span class="sxs-lookup"><span data-stu-id="0aee8-138">The following diagram and table explain the front panel of the LED display.</span></span>
   
    ![System-ID på OPS frontpanel](./media/storsimple-power-cooling-module-replacement/IC740991.png)
   
     <span data-ttu-id="0aee8-140">**Bild 1** framför panelen på enheten</span><span class="sxs-lookup"><span data-stu-id="0aee8-140">**Figure 1** Front panel of the device</span></span>  
   
   | <span data-ttu-id="0aee8-141">Etikett</span><span class="sxs-lookup"><span data-stu-id="0aee8-141">Label</span></span> | <span data-ttu-id="0aee8-142">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="0aee8-142">Description</span></span> |
   |:--- |:--- |
   | <span data-ttu-id="0aee8-143">1</span><span class="sxs-lookup"><span data-stu-id="0aee8-143">1</span></span> |<span data-ttu-id="0aee8-144">Ljud av</span><span class="sxs-lookup"><span data-stu-id="0aee8-144">Mute button</span></span> |
   | <span data-ttu-id="0aee8-145">2</span><span class="sxs-lookup"><span data-stu-id="0aee8-145">2</span></span> |<span data-ttu-id="0aee8-146">System power</span><span class="sxs-lookup"><span data-stu-id="0aee8-146">System power</span></span> |
   | <span data-ttu-id="0aee8-147">3</span><span class="sxs-lookup"><span data-stu-id="0aee8-147">3</span></span> |<span data-ttu-id="0aee8-148">Modul-fel</span><span class="sxs-lookup"><span data-stu-id="0aee8-148">Module fault</span></span> |
   | <span data-ttu-id="0aee8-149">4</span><span class="sxs-lookup"><span data-stu-id="0aee8-149">4</span></span> |<span data-ttu-id="0aee8-150">Logiska fel</span><span class="sxs-lookup"><span data-stu-id="0aee8-150">Logical fault</span></span> |
   | <span data-ttu-id="0aee8-151">5</span><span class="sxs-lookup"><span data-stu-id="0aee8-151">5</span></span> |<span data-ttu-id="0aee8-152">Enhet ID visas</span><span class="sxs-lookup"><span data-stu-id="0aee8-152">Unit ID display</span></span> |
3. <span data-ttu-id="0aee8-153">Övervakning indikatorn indikatorer på baksidan primära höljet kan också användas för att identifiera den felande PCM.</span><span class="sxs-lookup"><span data-stu-id="0aee8-153">The monitoring indicator LEDs in the back of the primary enclosure can also be used to identify the faulty PCM.</span></span> <span data-ttu-id="0aee8-154">Finns i följande diagram och tabell för att förstå hur du använder indikatorer för att hitta den felande PCM.</span><span class="sxs-lookup"><span data-stu-id="0aee8-154">See the following diagram and table to understand how to use the LEDs to locate the faulty PCM.</span></span> <span data-ttu-id="0aee8-155">Till exempel om det Indikator som motsvarar den **fläkt misslyckas** är upplysta, fläkten misslyckades.</span><span class="sxs-lookup"><span data-stu-id="0aee8-155">For example, if the LED corresponding to the **Fan Fail** is lit, the fan has failed.</span></span> <span data-ttu-id="0aee8-156">Likaså om den Indikator motsvarar **AC misslyckas** är upplysta, strömförsörjningen misslyckades.</span><span class="sxs-lookup"><span data-stu-id="0aee8-156">Likewise, if the LED corresponding to **AC Fail** is lit, the power supply has failed.</span></span> 
   
    ![Bakplan för enheten PCM övervakning indikatorn indikatorer](./media/storsimple-power-cooling-module-replacement/IC740992.png)
   
     <span data-ttu-id="0aee8-158">**Bild 2** tillbaka of PCM med indikator indikatorer</span><span class="sxs-lookup"><span data-stu-id="0aee8-158">**Figure 2** Back of PCM with indicator LEDs</span></span>
   
   | <span data-ttu-id="0aee8-159">Etikett</span><span class="sxs-lookup"><span data-stu-id="0aee8-159">Label</span></span> | <span data-ttu-id="0aee8-160">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="0aee8-160">Description</span></span> |
   |:--- |:--- |
   | <span data-ttu-id="0aee8-161">1</span><span class="sxs-lookup"><span data-stu-id="0aee8-161">1</span></span> |<span data-ttu-id="0aee8-162">AC strömavbrott</span><span class="sxs-lookup"><span data-stu-id="0aee8-162">AC power failure</span></span> |
   | <span data-ttu-id="0aee8-163">2</span><span class="sxs-lookup"><span data-stu-id="0aee8-163">2</span></span> |<span data-ttu-id="0aee8-164">Fläkt fel</span><span class="sxs-lookup"><span data-stu-id="0aee8-164">Fan failure</span></span> |
   | <span data-ttu-id="0aee8-165">3</span><span class="sxs-lookup"><span data-stu-id="0aee8-165">3</span></span> |<span data-ttu-id="0aee8-166">Batteri fel</span><span class="sxs-lookup"><span data-stu-id="0aee8-166">Battery fault</span></span> |
   | <span data-ttu-id="0aee8-167">4</span><span class="sxs-lookup"><span data-stu-id="0aee8-167">4</span></span> |<span data-ttu-id="0aee8-168">PCM OK</span><span class="sxs-lookup"><span data-stu-id="0aee8-168">PCM OK</span></span> |
   | <span data-ttu-id="0aee8-169">5</span><span class="sxs-lookup"><span data-stu-id="0aee8-169">5</span></span> |<span data-ttu-id="0aee8-170">DC strömavbrott</span><span class="sxs-lookup"><span data-stu-id="0aee8-170">DC power failure</span></span> |
   | <span data-ttu-id="0aee8-171">6</span><span class="sxs-lookup"><span data-stu-id="0aee8-171">6</span></span> |<span data-ttu-id="0aee8-172">Batteri felfri</span><span class="sxs-lookup"><span data-stu-id="0aee8-172">Battery healthy</span></span> |
4. <span data-ttu-id="0aee8-173">Referera till följande diagram av StorSimple-enhet för att hitta modulen misslyckade PCM.</span><span class="sxs-lookup"><span data-stu-id="0aee8-173">Refer to the following diagram of the back of the StorSimple device to locate the failed PCM module.</span></span> <span data-ttu-id="0aee8-174">PCM 0 är till vänster och PCM 1 till höger.</span><span class="sxs-lookup"><span data-stu-id="0aee8-174">PCM 0 is on the left and PCM 1 is on the right.</span></span> <span data-ttu-id="0aee8-175">Tabellen nedan beskrivs modulerna.</span><span class="sxs-lookup"><span data-stu-id="0aee8-175">The table that follows explains the modules.</span></span>
   
     ![Bakplan av primära Höljesmoduler](./media/storsimple-power-cooling-module-replacement/IC740994.png)
   
     <span data-ttu-id="0aee8-177">**Bild 3** baksidan av enheten med plugin-program</span><span class="sxs-lookup"><span data-stu-id="0aee8-177">**Figure 3** Back of device with plug-in modules</span></span> 
   
   | <span data-ttu-id="0aee8-178">Etikett</span><span class="sxs-lookup"><span data-stu-id="0aee8-178">Label</span></span> | <span data-ttu-id="0aee8-179">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="0aee8-179">Description</span></span> |
   |:--- |:--- |
   | <span data-ttu-id="0aee8-180">1</span><span class="sxs-lookup"><span data-stu-id="0aee8-180">1</span></span> |<span data-ttu-id="0aee8-181">PCM 0</span><span class="sxs-lookup"><span data-stu-id="0aee8-181">PCM 0</span></span> |
   | <span data-ttu-id="0aee8-182">2</span><span class="sxs-lookup"><span data-stu-id="0aee8-182">2</span></span> |<span data-ttu-id="0aee8-183">PCM 1</span><span class="sxs-lookup"><span data-stu-id="0aee8-183">PCM 1</span></span> |
   | <span data-ttu-id="0aee8-184">3</span><span class="sxs-lookup"><span data-stu-id="0aee8-184">3</span></span> |<span data-ttu-id="0aee8-185">Styrenhet 0</span><span class="sxs-lookup"><span data-stu-id="0aee8-185">Controller 0</span></span> |
   | <span data-ttu-id="0aee8-186">4</span><span class="sxs-lookup"><span data-stu-id="0aee8-186">4</span></span> |<span data-ttu-id="0aee8-187">Kontrollant 1</span><span class="sxs-lookup"><span data-stu-id="0aee8-187">Controller 1</span></span> |
5. <span data-ttu-id="0aee8-188">Stäng av den felaktiga PCM och koppla bort Ange strömsladd.</span><span class="sxs-lookup"><span data-stu-id="0aee8-188">Turn off the faulty PCM and disconnect the power supply cord.</span></span> <span data-ttu-id="0aee8-189">Du kan nu ta bort PCM.</span><span class="sxs-lookup"><span data-stu-id="0aee8-189">You can now remove the PCM.</span></span>
6. <span data-ttu-id="0aee8-190">Förstå själva låset och sidan av PCM referensen mellan USB och pekfingret och klämma dem tillsammans för att öppna referensen.</span><span class="sxs-lookup"><span data-stu-id="0aee8-190">Grasp the latch and the side of the PCM handle between your thumb and forefinger, and squeeze them together to open the handle.</span></span>
   
    ![Öppna PCM referensen](./media/storsimple-power-cooling-module-replacement/IC740995.png)
   
    <span data-ttu-id="0aee8-192">**Bild 4** öppna PCM-referens</span><span class="sxs-lookup"><span data-stu-id="0aee8-192">**Figure 4** Opening the PCM handle</span></span>
7. <span data-ttu-id="0aee8-193">Krama referensen och ta bort PCM.</span><span class="sxs-lookup"><span data-stu-id="0aee8-193">Grip the handle and remove the PCM.</span></span>
   
    ![Ta bort PCM-enhet](./media/storsimple-power-cooling-module-replacement/IC740996.png)
   
    <span data-ttu-id="0aee8-195">**Bild 5** tar bort PCM</span><span class="sxs-lookup"><span data-stu-id="0aee8-195">**Figure 5** Removing the PCM</span></span>

## <a name="install-a-replacement-pcm"></a><span data-ttu-id="0aee8-196">Installera en ersättning PCM</span><span class="sxs-lookup"><span data-stu-id="0aee8-196">Install a replacement PCM</span></span>
<span data-ttu-id="0aee8-197">Följ instruktionerna för att installera en PCM i din StorSimple-enhet.</span><span class="sxs-lookup"><span data-stu-id="0aee8-197">Follow these instructions to install a PCM in your StorSimple device.</span></span> <span data-ttu-id="0aee8-198">Se till att du har infogat säkerhetskopiering batterimodulen innan du installerar ersättning PCM (gäller endast 764 W PCMs).</span><span class="sxs-lookup"><span data-stu-id="0aee8-198">Ensure that you have inserted the backup battery module prior to installing the replacement PCM (applies to 764 W PCMs only).</span></span> <span data-ttu-id="0aee8-199">Mer information finns i så här [ta bort och infoga en reserv batteri modul](storsimple-8000-battery-replacement.md).</span><span class="sxs-lookup"><span data-stu-id="0aee8-199">For more information, see how to [remove and insert a backup battery module](storsimple-8000-battery-replacement.md).</span></span>

#### <a name="to-install-a-pcm"></a><span data-ttu-id="0aee8-200">Så här installerar du en PCM</span><span class="sxs-lookup"><span data-stu-id="0aee8-200">To install a PCM</span></span>
1. <span data-ttu-id="0aee8-201">Kontrollera att du har rätt ersättning PCM för denna enhet.</span><span class="sxs-lookup"><span data-stu-id="0aee8-201">Verify that you have the correct replacement PCM for this enclosure.</span></span> <span data-ttu-id="0aee8-202">Primär höljet måste en 764 W PCM och EBOD höljet måste en 580 W PCM.</span><span class="sxs-lookup"><span data-stu-id="0aee8-202">The primary enclosure needs a 764 W PCM and the EBOD enclosure needs a 580 W PCM.</span></span> <span data-ttu-id="0aee8-203">Du bör inte försöka använda den 580 W PCM i primära höljet eller 764 W-PCM i EBOD hölje.</span><span class="sxs-lookup"><span data-stu-id="0aee8-203">You should not attempt to use the 580 W PCM in the Primary enclosure, or the 764 W PCM in the EBOD enclosure.</span></span> <span data-ttu-id="0aee8-204">Följande bild visar var du vill identifiera den här informationen på den etikett som fästs på PCM.</span><span class="sxs-lookup"><span data-stu-id="0aee8-204">The following image shows where to identify this information on the label that is affixed to the PCM.</span></span>
   
    ![Enheten PCM etikett](./media/storsimple-power-cooling-module-replacement/IC740973.png)
   
    <span data-ttu-id="0aee8-206">**Bild 6** PCM etikett</span><span class="sxs-lookup"><span data-stu-id="0aee8-206">**Figure 6** PCM label</span></span>
2. <span data-ttu-id="0aee8-207">Sök efter skada på höljet var särskilt uppmärksam på anslutningarna.</span><span class="sxs-lookup"><span data-stu-id="0aee8-207">Check for damage to the enclosure, paying particular attention to the connectors.</span></span> 
   
   > [!NOTE]
   > <span data-ttu-id="0aee8-208">**Installera inte modulen om någon koppling stift är böjda.**</span><span class="sxs-lookup"><span data-stu-id="0aee8-208">**Do not install the module if any connector pins are bent.**</span></span>
   > 
   > 
3. <span data-ttu-id="0aee8-209">Dra modulen till höljet med PCM-referensen i öppet läge.</span><span class="sxs-lookup"><span data-stu-id="0aee8-209">With the PCM handle in the open position, slide the module into the enclosure.</span></span>
   
    ![Installerar PCM-enhet](./media/storsimple-power-cooling-module-replacement/IC740975.png)
   
    <span data-ttu-id="0aee8-211">**Bild 7** installerar PCM</span><span class="sxs-lookup"><span data-stu-id="0aee8-211">**Figure 7** Installing the PCM</span></span>
4. <span data-ttu-id="0aee8-212">Stäng manuellt PCM-referensen.</span><span class="sxs-lookup"><span data-stu-id="0aee8-212">Manually close the PCM handle.</span></span> <span data-ttu-id="0aee8-213">Du bör höra en klickning som snabbt tillkallar handtaget låstypen.</span><span class="sxs-lookup"><span data-stu-id="0aee8-213">You should hear a click as the handle latch engages.</span></span>
   
   > [!NOTE]
   > <span data-ttu-id="0aee8-214">För att säkerställa att utöva connector PIN-koder tug du försiktigt på referensen utan att släppa låset.</span><span class="sxs-lookup"><span data-stu-id="0aee8-214">To ensure that the connector pins have engaged, you can gently tug on the handle without releasing the latch.</span></span> <span data-ttu-id="0aee8-215">Om PCM bilder ut, innebär det att låset stängdes innan kopplingarna ägnar åt.</span><span class="sxs-lookup"><span data-stu-id="0aee8-215">If the PCM slides out, it implies that the latch was closed before the connectors engaged.</span></span>
   
5. <span data-ttu-id="0aee8-216">Anslut strömkablarna strömkällan och PCM.</span><span class="sxs-lookup"><span data-stu-id="0aee8-216">Connect the power cables to the power source and to the PCM.</span></span>
6. <span data-ttu-id="0aee8-217">Skydda belastningen befrielse balar.</span><span class="sxs-lookup"><span data-stu-id="0aee8-217">Secure the strain relief bales.</span></span>
7. <span data-ttu-id="0aee8-218">Aktivera PCM.</span><span class="sxs-lookup"><span data-stu-id="0aee8-218">Turn on the PCM.</span></span>
8. <span data-ttu-id="0aee8-219">Kontrollera att ersättningen lyckades: navigera till din enhet i Enhetshanteraren för StorSimple-tjänsten Azure-portalen och sedan till **Inställningar > övervaka > maskinvara hälsa**.</span><span class="sxs-lookup"><span data-stu-id="0aee8-219">Verify that the replacement was successful: in the Azure portal of your StorSimple Device Manager service, navigate to your device and then to **Settings > Monitor > Hardware health**.</span></span> <span data-ttu-id="0aee8-220">Under den **delade komponenter**, status för PCM vara grön.</span><span class="sxs-lookup"><span data-stu-id="0aee8-220">Under the **Shared components**, the status of the PCM should be green.</span></span>
   
   > [!NOTE]
   > <span data-ttu-id="0aee8-221">Det kan ta några minuter för ersättning PCM helt initieras.</span><span class="sxs-lookup"><span data-stu-id="0aee8-221">It may take a few minutes for the replacement PCM to completely initialize.</span></span>

## <a name="next-steps"></a><span data-ttu-id="0aee8-222">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="0aee8-222">Next steps</span></span>
<span data-ttu-id="0aee8-223">Lär dig mer om [ersättning av StorSimple maskinvara komponenten](storsimple-8000-hardware-component-replacement.md).</span><span class="sxs-lookup"><span data-stu-id="0aee8-223">Learn more about [StorSimple hardware component replacement](storsimple-8000-hardware-component-replacement.md).</span></span>

