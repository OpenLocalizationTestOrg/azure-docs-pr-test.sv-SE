---
title: "aaaReplace en PCM på enheten StorSimple 8000-serien | Microsoft Docs"
description: "Förklarar hur tooremove och Ersätt hello ström och kylning modul (PCM) på din StorSimple-enhet"
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
ms.openlocfilehash: 474fd09787c5361a81efda4de74356027ac60f47
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="replace-a-power-and-cooling-module-on-your-storsimple-device"></a><span data-ttu-id="73812-103">Ersätta en ström och kylning modulen på din StorSimple-enhet</span><span class="sxs-lookup"><span data-stu-id="73812-103">Replace a Power and Cooling Module on your StorSimple device</span></span>
## <a name="overview"></a><span data-ttu-id="73812-104">Översikt</span><span class="sxs-lookup"><span data-stu-id="73812-104">Overview</span></span>
<span data-ttu-id="73812-105">hello ström och kylning modul (PCM) i Microsoft Azure StorSimple-enheten består av en strömförsörjning och kylfläktar som styrs via hello primära och EBOD bilagor.</span><span class="sxs-lookup"><span data-stu-id="73812-105">hello Power and Cooling Module (PCM) in your Microsoft Azure StorSimple device consists of a power supply and cooling fans that are controlled through hello primary and EBOD enclosures.</span></span> <span data-ttu-id="73812-106">Det finns endast en modell för PCM som är certifierad för varje enhet.</span><span class="sxs-lookup"><span data-stu-id="73812-106">There is only one model of PCM that is certified for each enclosure.</span></span> <span data-ttu-id="73812-107">hello primära höljet är certifierad för en 764 W PCM och hello EBOD hölje är certifierad för en 580 W PCM.</span><span class="sxs-lookup"><span data-stu-id="73812-107">hello primary enclosure is certified for a 764 W PCM and hello EBOD enclosure is certified for a 580 W PCM.</span></span> <span data-ttu-id="73812-108">Även om hello PCMs för hello primära höljet och hello EBOD hölje mellan är hello ersättning proceduren identiska.</span><span class="sxs-lookup"><span data-stu-id="73812-108">Although hello PCMs for hello primary enclosure and hello EBOD enclosure are different, hello replacement procedure is identical.</span></span>

<span data-ttu-id="73812-109">Den här självstudiekursen beskrivs hur du:</span><span class="sxs-lookup"><span data-stu-id="73812-109">This tutorial explains how to:</span></span>

* <span data-ttu-id="73812-110">Ta bort en PCM</span><span class="sxs-lookup"><span data-stu-id="73812-110">Remove a PCM</span></span>
* <span data-ttu-id="73812-111">Installera en ersättning PCM</span><span class="sxs-lookup"><span data-stu-id="73812-111">Install a replacement PCM</span></span>

> [!IMPORTANT]
> <span data-ttu-id="73812-112">Innan du tar bort och ersätter en PCM granska hello säkerhetsinformation i [ersättning av StorSimple maskinvara komponenten](storsimple-8000-hardware-component-replacement.md).</span><span class="sxs-lookup"><span data-stu-id="73812-112">Before removing and replacing a PCM, review hello safety information in [StorSimple hardware component replacement](storsimple-8000-hardware-component-replacement.md).</span></span>


## <a name="before-you-replace-a-pcm"></a><span data-ttu-id="73812-113">Innan du ersätter en PCM</span><span class="sxs-lookup"><span data-stu-id="73812-113">Before you replace a PCM</span></span>
<span data-ttu-id="73812-114">Tänk på följande viktiga problem innan du ersätter din PCM hello:</span><span class="sxs-lookup"><span data-stu-id="73812-114">Be aware of hello following important issues before you replace your PCM:</span></span>

* <span data-ttu-id="73812-115">Om hello strömförsörjning av hello PCM misslyckas, lämna hello felaktiga module installerad ta bort hello strömsladd</span><span class="sxs-lookup"><span data-stu-id="73812-115">If hello power supply of hello PCM fails, leave hello faulty module installed, but remove hello power cord.</span></span> <span data-ttu-id="73812-116">hello-fläkt fortsätter tooreceive strömförsörjning från hello höljet och fortsätta tooprovide rätt kylning.</span><span class="sxs-lookup"><span data-stu-id="73812-116">hello fan will continue tooreceive power from hello enclosure and continue tooprovide proper cooling.</span></span> <span data-ttu-id="73812-117">Om hello-fläkt misslyckas måste hello PCM toobe ersättas omedelbart.</span><span class="sxs-lookup"><span data-stu-id="73812-117">If hello fan fails, hello PCM needs toobe replaced immediately.</span></span>
* <span data-ttu-id="73812-118">Innan du tar bort hello PCM koppla hello ur strömmen hello PCM genom att inaktivera hello huvudsakliga växel (om sådan finns) eller genom att ta bort hello strömsladd fysiskt.</span><span class="sxs-lookup"><span data-stu-id="73812-118">Before removing hello PCM, disconnect hello power from hello PCM by turning off hello main switch (where present) or by physically removing hello power cord.</span></span> <span data-ttu-id="73812-119">Detta ger tooyour varningssystem att en power avstängning är nära förestående.</span><span class="sxs-lookup"><span data-stu-id="73812-119">This provides a warning tooyour system that a power shutdown is imminent.</span></span>
* <span data-ttu-id="73812-120">Kontrollera att hello andra PCM fungerar för fortsatt systemåtgärd innan ersätter hello felaktiga PCM.</span><span class="sxs-lookup"><span data-stu-id="73812-120">Make sure that hello other PCM is functional for continued system operation before replacing hello faulty PCM.</span></span> <span data-ttu-id="73812-121">En felaktig PCM måste ersättas med en fullt fungerande PCM så snart som möjligt.</span><span class="sxs-lookup"><span data-stu-id="73812-121">A faulty PCM must be replaced by a fully operational PCM as soon as possible.</span></span>
* <span data-ttu-id="73812-122">PCM modulen ersättning tar bara några minuter toocomplete, men den måste slutföras inom 10 minuter på att ta bort hello misslyckades PCM tooprevent överhettning.</span><span class="sxs-lookup"><span data-stu-id="73812-122">PCM module replacement takes only few minutes toocomplete, but it must be completed within 10 minutes of removing hello failed PCM tooprevent overheating.</span></span>
* <span data-ttu-id="73812-123">Observera att hello ersättning 764 W PCM moduler från hello factory inte innehålla hello säkerhetskopiering batterimodulen.</span><span class="sxs-lookup"><span data-stu-id="73812-123">Note that hello replacement 764 W PCM modules shipped from hello factory do not contain hello backup battery module.</span></span> <span data-ttu-id="73812-124">Du behöver tooremove hello batteri från din felaktiga PCM och infoga det i hello ersättning modulen tidigare tooperforming hello ersättning.</span><span class="sxs-lookup"><span data-stu-id="73812-124">You will need tooremove hello battery from your faulty PCM and then insert it into hello replacement module prior tooperforming hello replacement.</span></span> <span data-ttu-id="73812-125">Mer information finns i hur för[ta bort och infoga en reserv batteri modul](storsimple-8000-battery-replacement.md).</span><span class="sxs-lookup"><span data-stu-id="73812-125">For more information, see how too[remove and insert a backup battery module](storsimple-8000-battery-replacement.md).</span></span>

## <a name="remove-a-pcm"></a><span data-ttu-id="73812-126">Ta bort en PCM</span><span class="sxs-lookup"><span data-stu-id="73812-126">Remove a PCM</span></span>
<span data-ttu-id="73812-127">Följ dessa instruktioner när du är klar tooremove en ström och kylning modul (PCM) från Microsoft Azure StorSimple-enhet.</span><span class="sxs-lookup"><span data-stu-id="73812-127">Follow these instructions when you are ready tooremove a Power and Cooling Module (PCM) from your Microsoft Azure StorSimple device.</span></span>

> [!NOTE]
> <span data-ttu-id="73812-128">Innan du tar bort din PCM kontrollerar du att rätt ersätter (764 W för hello primära höljet) eller 580 W för hello EBOD hölje.</span><span class="sxs-lookup"><span data-stu-id="73812-128">Before you remove your PCM, verify that you have a correct replacement (764 W for hello primary enclosure or 580 W for hello EBOD enclosure).</span></span>

#### <a name="tooremove-a-pcm"></a><span data-ttu-id="73812-129">tooremove en PCM</span><span class="sxs-lookup"><span data-stu-id="73812-129">tooremove a PCM</span></span>
1. <span data-ttu-id="73812-130">I hello klassiska Azure-portalen klickar du på **Inställningar > övervaka > maskinvara hälsa**.</span><span class="sxs-lookup"><span data-stu-id="73812-130">In hello Azure classic portal, click **Settings > Monitor > Hardware health**.</span></span> <span data-ttu-id="73812-131">Kontrollera status för hello hello PCM komponenter under **delade komponenter** tooidentify som PCM misslyckades:</span><span class="sxs-lookup"><span data-stu-id="73812-131">Check hello status of hello PCM components under **Shared components** tooidentify which PCM has failed:</span></span>
   
   * <span data-ttu-id="73812-132">Om en strömförsörjning i PCM 0 har misslyckats hello status för **strömförsörjning i PCM 0** blir röd.</span><span class="sxs-lookup"><span data-stu-id="73812-132">If a power supply in PCM 0 has failed, hello status of **Power Supply in PCM 0** will be red.</span></span>
   * <span data-ttu-id="73812-133">Om en strömförsörjning i PCM 1 har misslyckats hello status för **strömförsörjning i PCM 1** blir röd.</span><span class="sxs-lookup"><span data-stu-id="73812-133">If a power supply in PCM 1 has failed, hello status of **Power Supply in PCM 1** will be red.</span></span>
   * <span data-ttu-id="73812-134">Om hello-fläkt i PCM 1 har misslyckats hello statusen **kylning 0 för PCM 0** eller **kylning 1 för PCM 0** blir röd.</span><span class="sxs-lookup"><span data-stu-id="73812-134">If hello fan in PCM 1 has failed, hello status of either **Cooling 0 for PCM 0** or **Cooling 1 for PCM 0** will be red.</span></span>
2. <span data-ttu-id="73812-135">Leta upp hello misslyckade PCM på hello tillbaka av hello primära enhet.</span><span class="sxs-lookup"><span data-stu-id="73812-135">Locate hello failed PCM on hello back of hello primary enclosure.</span></span> <span data-ttu-id="73812-136">Om du kör en 8600-modell, identifiera hello primära höljet genom att titta på hello System identifiering enhetsnummer visas hello frontpanel Indikator visas.</span><span class="sxs-lookup"><span data-stu-id="73812-136">If you are running an 8600 model, identify hello primary enclosure by looking at hello System Unit Identification Number shown on hello front panel LED display.</span></span> <span data-ttu-id="73812-137">Hej standard enhets-ID som visas på hello primära höljet är **00**, medan hello standard enhets-ID som visas på hello EBOD hölje är **01**.</span><span class="sxs-lookup"><span data-stu-id="73812-137">hello default Unit ID displayed on hello primary enclosure is **00**, whereas hello default Unit ID displayed on hello EBOD enclosure is **01**.</span></span> <span data-ttu-id="73812-138">hello beskrivs följande diagram och tabellen hello frontpanel hello Indikator visas.</span><span class="sxs-lookup"><span data-stu-id="73812-138">hello following diagram and table explain hello front panel of hello LED display.</span></span>
   
    ![System-ID på OPS frontpanel](./media/storsimple-power-cooling-module-replacement/IC740991.png)
   
     <span data-ttu-id="73812-140">**Bild 1** framför panelen av hello-enhet</span><span class="sxs-lookup"><span data-stu-id="73812-140">**Figure 1** Front panel of hello device</span></span>  
   
   | <span data-ttu-id="73812-141">Etikett</span><span class="sxs-lookup"><span data-stu-id="73812-141">Label</span></span> | <span data-ttu-id="73812-142">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="73812-142">Description</span></span> |
   |:--- |:--- |
   | <span data-ttu-id="73812-143">1</span><span class="sxs-lookup"><span data-stu-id="73812-143">1</span></span> |<span data-ttu-id="73812-144">Ljud av</span><span class="sxs-lookup"><span data-stu-id="73812-144">Mute button</span></span> |
   | <span data-ttu-id="73812-145">2</span><span class="sxs-lookup"><span data-stu-id="73812-145">2</span></span> |<span data-ttu-id="73812-146">System power</span><span class="sxs-lookup"><span data-stu-id="73812-146">System power</span></span> |
   | <span data-ttu-id="73812-147">3</span><span class="sxs-lookup"><span data-stu-id="73812-147">3</span></span> |<span data-ttu-id="73812-148">Modul-fel</span><span class="sxs-lookup"><span data-stu-id="73812-148">Module fault</span></span> |
   | <span data-ttu-id="73812-149">4</span><span class="sxs-lookup"><span data-stu-id="73812-149">4</span></span> |<span data-ttu-id="73812-150">Logiska fel</span><span class="sxs-lookup"><span data-stu-id="73812-150">Logical fault</span></span> |
   | <span data-ttu-id="73812-151">5</span><span class="sxs-lookup"><span data-stu-id="73812-151">5</span></span> |<span data-ttu-id="73812-152">Enhet ID visas</span><span class="sxs-lookup"><span data-stu-id="73812-152">Unit ID display</span></span> |
3. <span data-ttu-id="73812-153">hello övervakning indikator led i hello baksidan hello primära höljet kan också användas tooidentify hello felaktiga PCM.</span><span class="sxs-lookup"><span data-stu-id="73812-153">hello monitoring indicator LEDs in hello back of hello primary enclosure can also be used tooidentify hello faulty PCM.</span></span> <span data-ttu-id="73812-154">Se hello följande diagram och tabell toounderstand hur toouse hello indikatorer toolocate hello felaktiga PCM.</span><span class="sxs-lookup"><span data-stu-id="73812-154">See hello following diagram and table toounderstand how toouse hello LEDs toolocate hello faulty PCM.</span></span> <span data-ttu-id="73812-155">Till exempel om hello LEDDE motsvarande toohello **fläkt misslyckas** är upplysta, hello-fläkt misslyckades.</span><span class="sxs-lookup"><span data-stu-id="73812-155">For example, if hello LED corresponding toohello **Fan Fail** is lit, hello fan has failed.</span></span> <span data-ttu-id="73812-156">Likaså om hello LEDDE för motsvarande**AC misslyckas** är upplysta, hello strömförsörjning misslyckades.</span><span class="sxs-lookup"><span data-stu-id="73812-156">Likewise, if hello LED corresponding too**AC Fail** is lit, hello power supply has failed.</span></span> 
   
    ![Bakplan för enheten PCM övervakning indikatorn indikatorer](./media/storsimple-power-cooling-module-replacement/IC740992.png)
   
     <span data-ttu-id="73812-158">**Bild 2** tillbaka of PCM med indikator indikatorer</span><span class="sxs-lookup"><span data-stu-id="73812-158">**Figure 2** Back of PCM with indicator LEDs</span></span>
   
   | <span data-ttu-id="73812-159">Etikett</span><span class="sxs-lookup"><span data-stu-id="73812-159">Label</span></span> | <span data-ttu-id="73812-160">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="73812-160">Description</span></span> |
   |:--- |:--- |
   | <span data-ttu-id="73812-161">1</span><span class="sxs-lookup"><span data-stu-id="73812-161">1</span></span> |<span data-ttu-id="73812-162">AC strömavbrott</span><span class="sxs-lookup"><span data-stu-id="73812-162">AC power failure</span></span> |
   | <span data-ttu-id="73812-163">2</span><span class="sxs-lookup"><span data-stu-id="73812-163">2</span></span> |<span data-ttu-id="73812-164">Fläkt fel</span><span class="sxs-lookup"><span data-stu-id="73812-164">Fan failure</span></span> |
   | <span data-ttu-id="73812-165">3</span><span class="sxs-lookup"><span data-stu-id="73812-165">3</span></span> |<span data-ttu-id="73812-166">Batteri fel</span><span class="sxs-lookup"><span data-stu-id="73812-166">Battery fault</span></span> |
   | <span data-ttu-id="73812-167">4</span><span class="sxs-lookup"><span data-stu-id="73812-167">4</span></span> |<span data-ttu-id="73812-168">PCM OK</span><span class="sxs-lookup"><span data-stu-id="73812-168">PCM OK</span></span> |
   | <span data-ttu-id="73812-169">5</span><span class="sxs-lookup"><span data-stu-id="73812-169">5</span></span> |<span data-ttu-id="73812-170">DC strömavbrott</span><span class="sxs-lookup"><span data-stu-id="73812-170">DC power failure</span></span> |
   | <span data-ttu-id="73812-171">6</span><span class="sxs-lookup"><span data-stu-id="73812-171">6</span></span> |<span data-ttu-id="73812-172">Batteri felfri</span><span class="sxs-lookup"><span data-stu-id="73812-172">Battery healthy</span></span> |
4. <span data-ttu-id="73812-173">Läs följande diagram över hello baksidan hello StorSimple enheten toolocate hello misslyckades PCM modulen toohello.</span><span class="sxs-lookup"><span data-stu-id="73812-173">Refer toohello following diagram of hello back of hello StorSimple device toolocate hello failed PCM module.</span></span> <span data-ttu-id="73812-174">PCM 0 är hello vänster och PCM 1 på hello rätt.</span><span class="sxs-lookup"><span data-stu-id="73812-174">PCM 0 is on hello left and PCM 1 is on hello right.</span></span> <span data-ttu-id="73812-175">hello i tabellen nedan beskrivs hello moduler.</span><span class="sxs-lookup"><span data-stu-id="73812-175">hello table that follows explains hello modules.</span></span>
   
     ![Bakplan av primära Höljesmoduler](./media/storsimple-power-cooling-module-replacement/IC740994.png)
   
     <span data-ttu-id="73812-177">**Bild 3** baksidan av enheten med plugin-program</span><span class="sxs-lookup"><span data-stu-id="73812-177">**Figure 3** Back of device with plug-in modules</span></span> 
   
   | <span data-ttu-id="73812-178">Etikett</span><span class="sxs-lookup"><span data-stu-id="73812-178">Label</span></span> | <span data-ttu-id="73812-179">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="73812-179">Description</span></span> |
   |:--- |:--- |
   | <span data-ttu-id="73812-180">1</span><span class="sxs-lookup"><span data-stu-id="73812-180">1</span></span> |<span data-ttu-id="73812-181">PCM 0</span><span class="sxs-lookup"><span data-stu-id="73812-181">PCM 0</span></span> |
   | <span data-ttu-id="73812-182">2</span><span class="sxs-lookup"><span data-stu-id="73812-182">2</span></span> |<span data-ttu-id="73812-183">PCM 1</span><span class="sxs-lookup"><span data-stu-id="73812-183">PCM 1</span></span> |
   | <span data-ttu-id="73812-184">3</span><span class="sxs-lookup"><span data-stu-id="73812-184">3</span></span> |<span data-ttu-id="73812-185">Styrenhet 0</span><span class="sxs-lookup"><span data-stu-id="73812-185">Controller 0</span></span> |
   | <span data-ttu-id="73812-186">4</span><span class="sxs-lookup"><span data-stu-id="73812-186">4</span></span> |<span data-ttu-id="73812-187">Kontrollant 1</span><span class="sxs-lookup"><span data-stu-id="73812-187">Controller 1</span></span> |
5. <span data-ttu-id="73812-188">Aktivera ut hello felaktiga PCM och koppla från hello strömsladd leverans.</span><span class="sxs-lookup"><span data-stu-id="73812-188">Turn off hello faulty PCM and disconnect hello power supply cord.</span></span> <span data-ttu-id="73812-189">Du kan nu ta bort hello PCM.</span><span class="sxs-lookup"><span data-stu-id="73812-189">You can now remove hello PCM.</span></span>
6. <span data-ttu-id="73812-190">Tag hello låset och hello sida av hello PCM hantera mellan USB och pekfingret och klämma dem tillsammans tooopen hello referensen.</span><span class="sxs-lookup"><span data-stu-id="73812-190">Grasp hello latch and hello side of hello PCM handle between your thumb and forefinger, and squeeze them together tooopen hello handle.</span></span>
   
    ![Öppna PCM referensen](./media/storsimple-power-cooling-module-replacement/IC740995.png)
   
    <span data-ttu-id="73812-192">**Bild 4** öppnas hello PCM hantera</span><span class="sxs-lookup"><span data-stu-id="73812-192">**Figure 4** Opening hello PCM handle</span></span>
7. <span data-ttu-id="73812-193">Greppet hello hantera och ta bort hello PCM.</span><span class="sxs-lookup"><span data-stu-id="73812-193">Grip hello handle and remove hello PCM.</span></span>
   
    ![Ta bort PCM-enhet](./media/storsimple-power-cooling-module-replacement/IC740996.png)
   
    <span data-ttu-id="73812-195">**Bild 5** tar bort hello PCM</span><span class="sxs-lookup"><span data-stu-id="73812-195">**Figure 5** Removing hello PCM</span></span>

## <a name="install-a-replacement-pcm"></a><span data-ttu-id="73812-196">Installera en ersättning PCM</span><span class="sxs-lookup"><span data-stu-id="73812-196">Install a replacement PCM</span></span>
<span data-ttu-id="73812-197">Följ dessa instruktioner tooinstall en PCM i din StorSimple-enhet.</span><span class="sxs-lookup"><span data-stu-id="73812-197">Follow these instructions tooinstall a PCM in your StorSimple device.</span></span> <span data-ttu-id="73812-198">Se till att du har infogat hello säkerhetskopiering batteri modulen tidigare tooinstalling hello ersättning PCM (gäller too764 W PCMs).</span><span class="sxs-lookup"><span data-stu-id="73812-198">Ensure that you have inserted hello backup battery module prior tooinstalling hello replacement PCM (applies too764 W PCMs only).</span></span> <span data-ttu-id="73812-199">Mer information finns i hur för[ta bort och infoga en reserv batteri modul](storsimple-8000-battery-replacement.md).</span><span class="sxs-lookup"><span data-stu-id="73812-199">For more information, see how too[remove and insert a backup battery module](storsimple-8000-battery-replacement.md).</span></span>

#### <a name="tooinstall-a-pcm"></a><span data-ttu-id="73812-200">tooinstall en PCM</span><span class="sxs-lookup"><span data-stu-id="73812-200">tooinstall a PCM</span></span>
1. <span data-ttu-id="73812-201">Kontrollera att du har hello rätt ersättning PCM för denna enhet.</span><span class="sxs-lookup"><span data-stu-id="73812-201">Verify that you have hello correct replacement PCM for this enclosure.</span></span> <span data-ttu-id="73812-202">hello primära höljet måste en 764 W PCM och hello EBOD hölje måste en 580 W PCM.</span><span class="sxs-lookup"><span data-stu-id="73812-202">hello primary enclosure needs a 764 W PCM and hello EBOD enclosure needs a 580 W PCM.</span></span> <span data-ttu-id="73812-203">Du bör inte försöka toouse hello 580 W PCM i hello primära hölje eller hello 764 W PCM i hello EBOD hölje.</span><span class="sxs-lookup"><span data-stu-id="73812-203">You should not attempt toouse hello 580 W PCM in hello Primary enclosure, or hello 764 W PCM in hello EBOD enclosure.</span></span> <span data-ttu-id="73812-204">Följande bild visar där tooidentify informationen på hello etiketten som fästs toohello PCM hello.</span><span class="sxs-lookup"><span data-stu-id="73812-204">hello following image shows where tooidentify this information on hello label that is affixed toohello PCM.</span></span>
   
    ![Enheten PCM etikett](./media/storsimple-power-cooling-module-replacement/IC740973.png)
   
    <span data-ttu-id="73812-206">**Bild 6** PCM etikett</span><span class="sxs-lookup"><span data-stu-id="73812-206">**Figure 6** PCM label</span></span>
2. <span data-ttu-id="73812-207">Sök efter skada toohello hölje, och var särskilt uppmärksam toohello kopplingar.</span><span class="sxs-lookup"><span data-stu-id="73812-207">Check for damage toohello enclosure, paying particular attention toohello connectors.</span></span> 
   
   > [!NOTE]
   > <span data-ttu-id="73812-208">**Installera inte hello modulen om någon koppling stift är böjda.**</span><span class="sxs-lookup"><span data-stu-id="73812-208">**Do not install hello module if any connector pins are bent.**</span></span>
   > 
   > 
3. <span data-ttu-id="73812-209">Öppna position, bild hello-modulen till hello höljet med hello PCM hantera i hello.</span><span class="sxs-lookup"><span data-stu-id="73812-209">With hello PCM handle in hello open position, slide hello module into hello enclosure.</span></span>
   
    ![Installerar PCM-enhet](./media/storsimple-power-cooling-module-replacement/IC740975.png)
   
    <span data-ttu-id="73812-211">**Bild 7** installerar hello PCM</span><span class="sxs-lookup"><span data-stu-id="73812-211">**Figure 7** Installing hello PCM</span></span>
4. <span data-ttu-id="73812-212">Stäng hello PCM referensen manuellt.</span><span class="sxs-lookup"><span data-stu-id="73812-212">Manually close hello PCM handle.</span></span> <span data-ttu-id="73812-213">Du bör höra en klickning som snabbt tillkallar hello handtaget låstypen.</span><span class="sxs-lookup"><span data-stu-id="73812-213">You should hear a click as hello handle latch engages.</span></span>
   
   > [!NOTE]
   > <span data-ttu-id="73812-214">tooensure som hello connector PIN-koder har blivit engagerade du försiktigt tug hello handtag utan att släppa hello spärren.</span><span class="sxs-lookup"><span data-stu-id="73812-214">tooensure that hello connector pins have engaged, you can gently tug on hello handle without releasing hello latch.</span></span> <span data-ttu-id="73812-215">Om hello PCM bilder ut, innebär det att hello spärren stängdes innan hello kopplingar ägnar åt.</span><span class="sxs-lookup"><span data-stu-id="73812-215">If hello PCM slides out, it implies that hello latch was closed before hello connectors engaged.</span></span>
   
5. <span data-ttu-id="73812-216">Ansluta hello kablar toohello power kraftkälla och toohello PCM.</span><span class="sxs-lookup"><span data-stu-id="73812-216">Connect hello power cables toohello power source and toohello PCM.</span></span>
6. <span data-ttu-id="73812-217">Skydda hello stam befrielse balar.</span><span class="sxs-lookup"><span data-stu-id="73812-217">Secure hello strain relief bales.</span></span>
7. <span data-ttu-id="73812-218">Aktivera hello PCM.</span><span class="sxs-lookup"><span data-stu-id="73812-218">Turn on hello PCM.</span></span>
8. <span data-ttu-id="73812-219">Kontrollera att hello ersättning lyckades: navigera tooyour enheten i hello Azure-portalen för StorSimple Device Manager-tjänsten, och sedan för**Inställningar > övervakaren > maskinvara hälsa**.</span><span class="sxs-lookup"><span data-stu-id="73812-219">Verify that hello replacement was successful: in hello Azure portal of your StorSimple Device Manager service, navigate tooyour device and then too**Settings > Monitor > Hardware health**.</span></span> <span data-ttu-id="73812-220">Under hello **delade komponenter**, hello status för hello PCM vara grön.</span><span class="sxs-lookup"><span data-stu-id="73812-220">Under hello **Shared components**, hello status of hello PCM should be green.</span></span>
   
   > [!NOTE]
   > <span data-ttu-id="73812-221">Det kan ta några minuter för hello ersättning PCM toocompletely initieras.</span><span class="sxs-lookup"><span data-stu-id="73812-221">It may take a few minutes for hello replacement PCM toocompletely initialize.</span></span>

## <a name="next-steps"></a><span data-ttu-id="73812-222">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="73812-222">Next steps</span></span>
<span data-ttu-id="73812-223">Lär dig mer om [ersättning av StorSimple maskinvara komponenten](storsimple-8000-hardware-component-replacement.md).</span><span class="sxs-lookup"><span data-stu-id="73812-223">Learn more about [StorSimple hardware component replacement](storsimple-8000-hardware-component-replacement.md).</span></span>

