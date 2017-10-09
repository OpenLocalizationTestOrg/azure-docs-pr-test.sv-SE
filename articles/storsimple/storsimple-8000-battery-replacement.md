---
title: "aaaReplace batteri på Microsoft Azure StorSimple 8000-serieenhet | Microsoft Docs"
description: "Beskriver hur tooremove, Ersätt och underhålla hello säkerhetskopiering batteri modul på din StorSimple-enhet."
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
ms.date: 06/05/2017
ms.author: alkohli
ms.openlocfilehash: 5ac767807e6c3fd817d8d522629db2aceaac9bdf
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="replace-hello-backup-battery-module-on-your-storsimple-device"></a><span data-ttu-id="a234d-103">Ersätt hello säkerhetskopiering batteri modul på din StorSimple-enhet</span><span class="sxs-lookup"><span data-stu-id="a234d-103">Replace hello backup battery module on your StorSimple device</span></span>

## <a name="overview"></a><span data-ttu-id="a234d-104">Översikt</span><span class="sxs-lookup"><span data-stu-id="a234d-104">Overview</span></span>
<span data-ttu-id="a234d-105">hello primära höljet ström och kylning modul (PCM) på din Microsoft Azure StorSimple-enhet har en extra batteri pack.</span><span class="sxs-lookup"><span data-stu-id="a234d-105">hello primary enclosure Power and Cooling Module (PCM) on your Microsoft Azure StorSimple device has an additional battery pack.</span></span> <span data-ttu-id="a234d-106">Paketet innehåller power så att hello StorSimple-enhet kan spara data om förlust av AC power toohello primära enhet.</span><span class="sxs-lookup"><span data-stu-id="a234d-106">This pack provides power so that hello StorSimple device can save data if there is loss of AC power toohello primary enclosure.</span></span> <span data-ttu-id="a234d-107">Den här batteri pack är refererad tooas hello *säkerhetskopiering batterimodulen*.</span><span class="sxs-lookup"><span data-stu-id="a234d-107">This battery pack is referred tooas hello *backup battery module*.</span></span> <span data-ttu-id="a234d-108">hello säkerhetskopiering batteri modul finns bara för hello primära höljet i din StorSimple-enhet (hello EBOD hölje innehåller inte en säkerhetskopiering batteri modul).</span><span class="sxs-lookup"><span data-stu-id="a234d-108">hello backup battery module exists only for hello primary enclosure in your StorSimple device (hello EBOD enclosure does not contain a backup battery module).</span></span>

<span data-ttu-id="a234d-109">Den här självstudiekursen beskrivs hur du:</span><span class="sxs-lookup"><span data-stu-id="a234d-109">This tutorial explains how to:</span></span>

* <span data-ttu-id="a234d-110">Ta bort hello säkerhetskopiering batteri modul</span><span class="sxs-lookup"><span data-stu-id="a234d-110">Remove hello backup battery module</span></span>
* <span data-ttu-id="a234d-111">Installera en ny modul som reserv batteri</span><span class="sxs-lookup"><span data-stu-id="a234d-111">Install a new backup battery module</span></span>
* <span data-ttu-id="a234d-112">Underhåll hello säkerhetskopiering batteri modul</span><span class="sxs-lookup"><span data-stu-id="a234d-112">Maintain hello backup battery module</span></span>

> [!IMPORTANT]
> <span data-ttu-id="a234d-113">Innan du tar bort och ersätter en reserv batteri modul, granska hello säkerhetsinformation i hello [ersättning av introduktion tooStorSimple maskinvara komponenten](storsimple-8000-hardware-component-replacement.md).</span><span class="sxs-lookup"><span data-stu-id="a234d-113">Before removing and replacing a backup battery module, review hello safety information in hello [Introduction tooStorSimple hardware component replacement](storsimple-8000-hardware-component-replacement.md).</span></span>


## <a name="remove-hello-backup-battery-module"></a><span data-ttu-id="a234d-114">Ta bort hello säkerhetskopiering batteri modul</span><span class="sxs-lookup"><span data-stu-id="a234d-114">Remove hello backup battery module</span></span>
<span data-ttu-id="a234d-115">hello säkerhetskopiering batteri modul för din StorSimple-enhet är en-fältutbytbar enhet.</span><span class="sxs-lookup"><span data-stu-id="a234d-115">hello backup battery module for your StorSimple device is a field-replaceable unit.</span></span> <span data-ttu-id="a234d-116">Innan det installeras i hello PCM ska hello batterimodulen lagras i dess ursprungliga förpackning.</span><span class="sxs-lookup"><span data-stu-id="a234d-116">Before it is installed in hello PCM, hello battery module should be stored in its original packaging.</span></span> <span data-ttu-id="a234d-117">Utför följande steg tooremove hello säkerhetskopiering batteri hello.</span><span class="sxs-lookup"><span data-stu-id="a234d-117">Perform hello following steps tooremove hello backup battery.</span></span>

#### <a name="tooremove-hello-backup-battery-module"></a><span data-ttu-id="a234d-118">tooremove hello säkerhetskopiering batteri modul</span><span class="sxs-lookup"><span data-stu-id="a234d-118">tooremove hello backup battery module</span></span>
1. <span data-ttu-id="a234d-119">Gå tooyour StorSimple Enhetshanteraren service bladet i hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="a234d-119">In hello Azure portal, go tooyour StorSimple Device Manager service blade.</span></span> <span data-ttu-id="a234d-120">Gå för**enheter** och väljer sedan enheten hello lista över enheter.</span><span class="sxs-lookup"><span data-stu-id="a234d-120">Go too**Devices** and then select your device from hello list of devices.</span></span> <span data-ttu-id="a234d-121">Navigera för**övervakaren** > **maskinvara hälsa**.</span><span class="sxs-lookup"><span data-stu-id="a234d-121">Navigate too**Monitor** > **Hardware health**.</span></span> <span data-ttu-id="a234d-122">Under **delade komponenter**, se hello status för hello batteri.</span><span class="sxs-lookup"><span data-stu-id="a234d-122">Under **Shared Components**, look at hello status of hello battery.</span></span>
2. <span data-ttu-id="a234d-123">Identifiera hello PCM i vilka hello batteri misslyckades.</span><span class="sxs-lookup"><span data-stu-id="a234d-123">Identify hello PCM in which hello battery has failed.</span></span> <span data-ttu-id="a234d-124">Bild 1 visar hello baksidan hello StorSimple-enhet.</span><span class="sxs-lookup"><span data-stu-id="a234d-124">Figure 1 shows hello back of hello StorSimple device.</span></span>
   
    ![Bakplan av primära Höljesmoduler](./media/storsimple-battery-replacement/IC740994.png)
   
    <span data-ttu-id="a234d-126">**Bild 1** baksidan primära enhet visar PCM och controller moduler</span><span class="sxs-lookup"><span data-stu-id="a234d-126">**Figure 1** Back of primary device showing PCM and controller modules</span></span>
   
   | <span data-ttu-id="a234d-127">Etikett</span><span class="sxs-lookup"><span data-stu-id="a234d-127">Label</span></span> | <span data-ttu-id="a234d-128">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="a234d-128">Description</span></span> |
   |:--- |:--- |
   | <span data-ttu-id="a234d-129">1</span><span class="sxs-lookup"><span data-stu-id="a234d-129">1</span></span> |<span data-ttu-id="a234d-130">PCM 0</span><span class="sxs-lookup"><span data-stu-id="a234d-130">PCM 0</span></span> |
   | <span data-ttu-id="a234d-131">2</span><span class="sxs-lookup"><span data-stu-id="a234d-131">2</span></span> |<span data-ttu-id="a234d-132">PCM 1</span><span class="sxs-lookup"><span data-stu-id="a234d-132">PCM 1</span></span> |
   | <span data-ttu-id="a234d-133">3</span><span class="sxs-lookup"><span data-stu-id="a234d-133">3</span></span> |<span data-ttu-id="a234d-134">Styrenhet 0</span><span class="sxs-lookup"><span data-stu-id="a234d-134">Controller 0</span></span> |
   | <span data-ttu-id="a234d-135">4</span><span class="sxs-lookup"><span data-stu-id="a234d-135">4</span></span> |<span data-ttu-id="a234d-136">Kontrollant 1</span><span class="sxs-lookup"><span data-stu-id="a234d-136">Controller 1</span></span> |
   
    <span data-ttu-id="a234d-137">Enligt av talet 3 i hello bild 2 hello övervakning indikatorns LEDDE på PCM 0 som motsvarar för**batteri fel** bör upplysta.</span><span class="sxs-lookup"><span data-stu-id="a234d-137">As shown by number 3 in hello Figure 2, hello monitoring indicator LED on PCM 0 that corresponds too**Battery Fault** should be lit.</span></span>
   
    ![Bakplan av enheten PCM övervakning indikator indikatorer](./media/storsimple-battery-replacement/IC740992.png)
   
    <span data-ttu-id="a234d-139">**Bild 2** tillbaka of PCM visar hello övervakning indikatorer</span><span class="sxs-lookup"><span data-stu-id="a234d-139">**Figure 2** Back of PCM showing hello monitoring indicator LEDs</span></span>
   
   | <span data-ttu-id="a234d-140">Etikett</span><span class="sxs-lookup"><span data-stu-id="a234d-140">Label</span></span> | <span data-ttu-id="a234d-141">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="a234d-141">Description</span></span> |
   |:--- |:--- |
   | <span data-ttu-id="a234d-142">1</span><span class="sxs-lookup"><span data-stu-id="a234d-142">1</span></span> |<span data-ttu-id="a234d-143">AC strömavbrott</span><span class="sxs-lookup"><span data-stu-id="a234d-143">AC power failure</span></span> |
   | <span data-ttu-id="a234d-144">2</span><span class="sxs-lookup"><span data-stu-id="a234d-144">2</span></span> |<span data-ttu-id="a234d-145">Fläkt fel</span><span class="sxs-lookup"><span data-stu-id="a234d-145">Fan failure</span></span> |
   | <span data-ttu-id="a234d-146">3</span><span class="sxs-lookup"><span data-stu-id="a234d-146">3</span></span> |<span data-ttu-id="a234d-147">Batteri fel</span><span class="sxs-lookup"><span data-stu-id="a234d-147">Battery fault</span></span> |
   | <span data-ttu-id="a234d-148">4</span><span class="sxs-lookup"><span data-stu-id="a234d-148">4</span></span> |<span data-ttu-id="a234d-149">PCM OK</span><span class="sxs-lookup"><span data-stu-id="a234d-149">PCM OK</span></span> |
   | <span data-ttu-id="a234d-150">5</span><span class="sxs-lookup"><span data-stu-id="a234d-150">5</span></span> |<span data-ttu-id="a234d-151">DC strömavbrott</span><span class="sxs-lookup"><span data-stu-id="a234d-151">DC power failure</span></span> |
   | <span data-ttu-id="a234d-152">6</span><span class="sxs-lookup"><span data-stu-id="a234d-152">6</span></span> |<span data-ttu-id="a234d-153">Batteri felfri</span><span class="sxs-lookup"><span data-stu-id="a234d-153">Battery healthy</span></span> |
3. <span data-ttu-id="a234d-154">tooremove hello PCM med en misslyckad batteri åtgärderna hello i [ta bort en PCM](storsimple-power-cooling-module-replacement.md#remove-a-pcm).</span><span class="sxs-lookup"><span data-stu-id="a234d-154">tooremove hello PCM with a failed battery, follow hello steps in [Remove a PCM](storsimple-power-cooling-module-replacement.md#remove-a-pcm).</span></span>
4. <span data-ttu-id="a234d-155">Med hello PCM tas bort, lift och rotera hello batteri modulen hanterar uppåt som anges i följande bild hello och dra in tooremove hello batteri.</span><span class="sxs-lookup"><span data-stu-id="a234d-155">With hello PCM removed, lift and rotate hello battery module handle upward as indicated in hello following figure, and pull it up tooremove hello battery.</span></span>
   
    ![Ta bort batteri från PCM](./media/storsimple-battery-replacement/IC741019.png)
   
    <span data-ttu-id="a234d-157">**Bild 3** tar bort hello batteri från hello PCM</span><span class="sxs-lookup"><span data-stu-id="a234d-157">**Figure 3** Removing hello battery from hello PCM</span></span>
5. <span data-ttu-id="a234d-158">Placera hello modul i hello-fältutbytbar enhet paketera.</span><span class="sxs-lookup"><span data-stu-id="a234d-158">Place hello module in hello field-replaceable unit packaging.</span></span>
6. <span data-ttu-id="a234d-159">Returnera hello felaktiga enheten tooMicrosoft för rätt underhåll och hantering.</span><span class="sxs-lookup"><span data-stu-id="a234d-159">Return hello defective unit tooMicrosoft for proper servicing and handling.</span></span>

## <a name="install-a-new-backup-battery-module"></a><span data-ttu-id="a234d-160">Installera en ny modul som reserv batteri</span><span class="sxs-lookup"><span data-stu-id="a234d-160">Install a new backup battery module</span></span>
<span data-ttu-id="a234d-161">Utföra hello följande steg tooinstall hello ersättning batteri modul i hello PCM i hello primära hölje av StorSimple-enheten.</span><span class="sxs-lookup"><span data-stu-id="a234d-161">Perform hello following steps tooinstall hello replacement battery module in hello PCM in hello primary enclosure of your StorSimple device.</span></span>

#### <a name="tooinstall-hello-battery-module"></a><span data-ttu-id="a234d-162">tooinstall hello batteri modul</span><span class="sxs-lookup"><span data-stu-id="a234d-162">tooinstall hello battery module</span></span>
1. <span data-ttu-id="a234d-163">Placera hello säkerhetskopiering batteri modul i hello rätt orientering i hello PCM.</span><span class="sxs-lookup"><span data-stu-id="a234d-163">Place hello backup battery module in hello proper orientation in hello PCM.</span></span>
2. <span data-ttu-id="a234d-164">Tryck ned hello batterimodulen hantera alla hello sätt tooseat hello connector.</span><span class="sxs-lookup"><span data-stu-id="a234d-164">Press down hello battery module handle all hello way tooseat hello connector.</span></span>
3. <span data-ttu-id="a234d-165">Ersätt hello PCM i hello primära hölje genom att följa hello riktlinjerna i [ersätta en ström och kylning modulen på StorSimple-enheten](storsimple-power-cooling-module-replacement.md).</span><span class="sxs-lookup"><span data-stu-id="a234d-165">Replace hello PCM in hello primary enclosure by following hello guidelines in [Replace a Power and Cooling Module on your StorSimple device](storsimple-power-cooling-module-replacement.md).</span></span>
4. <span data-ttu-id="a234d-166">När hello ersättning är klar, går tooyour enhet och gå sedan för**övervakaren** > **maskinvara hälsa** i hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="a234d-166">After hello replacement is complete, go tooyour device and then go too**Monitor** > **Hardware health** in hello Azure portal.</span></span> <span data-ttu-id="a234d-167">Kontrollera hello status för hello batteri toomake till att hello-installationen har lyckats.</span><span class="sxs-lookup"><span data-stu-id="a234d-167">Verify hello status of hello battery toomake sure that hello installation was successful.</span></span> <span data-ttu-id="a234d-168">Grön status anger att hello batteri är felfri.</span><span class="sxs-lookup"><span data-stu-id="a234d-168">A green status indicates that hello battery is healthy.</span></span>

## <a name="maintain-hello-backup-battery-module"></a><span data-ttu-id="a234d-169">Underhåll hello säkerhetskopiering batteri modul</span><span class="sxs-lookup"><span data-stu-id="a234d-169">Maintain hello backup battery module</span></span>
<span data-ttu-id="a234d-170">I din StorSimple-enhet ger hello säkerhetskopiering batterimodulen power toohello styrenhet under ett Strömfel dataförlust.</span><span class="sxs-lookup"><span data-stu-id="a234d-170">In your StorSimple device, hello backup battery module provides power toohello controller during a power loss event.</span></span> <span data-ttu-id="a234d-171">Hej StorSimple enheten toosave kritiska data tidigare tooshutting ned kan på ett kontrollerat sätt.</span><span class="sxs-lookup"><span data-stu-id="a234d-171">It allows hello StorSimple device toosave critical data prior tooshutting down in a controlled manner.</span></span> <span data-ttu-id="a234d-172">Med två fullständigt debiteras batterierna i hello PCMs kan hello system hantera två på varandra följande förlorade händelser.</span><span class="sxs-lookup"><span data-stu-id="a234d-172">With two fully charged batteries in hello PCMs, hello system can handle two consecutive loss events.</span></span>

<span data-ttu-id="a234d-173">I hello Azure-portalen, hello **maskinvara hälsa** under hello **övervakaren** bladet anger om hello batteri är fel eller närmar sig hello slutet av livslängd.</span><span class="sxs-lookup"><span data-stu-id="a234d-173">In hello Azure portal, hello **Hardware health** under hello **Monitor** blade indicates whether hello battery is malfunctioning or hello end-of-life is approaching.</span></span> <span data-ttu-id="a234d-174">hello batteristatus anges med **batteri i PCM 0** eller **batteri i PCM 1** under **delade komponenter**.</span><span class="sxs-lookup"><span data-stu-id="a234d-174">hello battery status is indicated by **Battery in PCM 0** or **Battery in PCM 1** under **Shared Components**.</span></span> <span data-ttu-id="a234d-175">Det här bladet visar en **DEGRADERAD** tillstånd för det närmar sig slutet av livslängd, och **misslyckades** för nått end-för-livslängd.</span><span class="sxs-lookup"><span data-stu-id="a234d-175">This blade will show a **DEGRADED** state for end-of-life approaching, and **FAILED** for end-of-life reached.</span></span>

> [!NOTE]
> <span data-ttu-id="a234d-176">hello batteri kan rapportera **misslyckades** när den behöver bara toobe debiteras.</span><span class="sxs-lookup"><span data-stu-id="a234d-176">hello battery can report **FAILED** when it simply needs toobe charged.</span></span>


<span data-ttu-id="a234d-177">Om hello **DEGRADERAD** status visas, rekommenderar vi hello följande åtgärd:</span><span class="sxs-lookup"><span data-stu-id="a234d-177">If hello **DEGRADED** state appears, we recommend hello following course of action:</span></span>

* <span data-ttu-id="a234d-178">hello system kan ha uppstått nyligen strömavbrott eller hello batterierna kan väldigt regelbundet underhåll.</span><span class="sxs-lookup"><span data-stu-id="a234d-178">hello system may have experienced a recent power loss or hello batteries may be undergoing periodic maintenance.</span></span> <span data-ttu-id="a234d-179">Se hello system för 12 timmar innan du fortsätter.</span><span class="sxs-lookup"><span data-stu-id="a234d-179">Observe hello system for 12 hours before proceeding.</span></span>
  
  * <span data-ttu-id="a234d-180">Om hello tillstånd är fortfarande **DEGRADERAD** efter 12 timmar kontinuerlig anslutning tooAC ström med hello domänkontrollanter och PCMs körs sedan hello batteri måste toobe ersättas.</span><span class="sxs-lookup"><span data-stu-id="a234d-180">If hello state is still **DEGRADED** after 12 hours of continuous connection tooAC power with hello controllers and PCMs running, then hello battery needs toobe replaced.</span></span> <span data-ttu-id="a234d-181">Kontrollera [kontaktar Microsoft Support](storsimple-8000-contact-microsoft-support.md) för en ersättning säkerhetskopiering batteri modul.</span><span class="sxs-lookup"><span data-stu-id="a234d-181">Please [contact Microsoft Support](storsimple-8000-contact-microsoft-support.md) for a replacement backup battery module.</span></span>
  * <span data-ttu-id="a234d-182">Om hello tillstånd blir OK efter 12 timmar, hello batteri fungerar och behövs endast en avgift för underhåll.</span><span class="sxs-lookup"><span data-stu-id="a234d-182">If hello state becomes OK after 12 hours, hello battery is operational, and it only needed a maintenance charge.</span></span>
* <span data-ttu-id="a234d-183">Om det inte har en associerad förlust av AC-ström och hello PCM är påslagen och ansluten tooAC power, måste hello batteri toobe ersättas.</span><span class="sxs-lookup"><span data-stu-id="a234d-183">If there has not been an associated loss of AC power and hello PCM is turned on and connected tooAC power, hello battery needs toobe replaced.</span></span> <span data-ttu-id="a234d-184">[Kontakta Microsoft Support](storsimple-8000-contact-microsoft-support.md) tooorder en ersättning säkerhetskopiering batteri modul.</span><span class="sxs-lookup"><span data-stu-id="a234d-184">[Contact Microsoft Support](storsimple-8000-contact-microsoft-support.md) tooorder a replacement backup battery module.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="a234d-185">Ta bort hello misslyckades batteri enligt toonational och nationella föreskrifter.</span><span class="sxs-lookup"><span data-stu-id="a234d-185">Dispose of hello failed battery according toonational and regional regulations.</span></span>

## <a name="next-steps"></a><span data-ttu-id="a234d-186">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="a234d-186">Next steps</span></span>
<span data-ttu-id="a234d-187">Lär dig mer om [ersättning av StorSimple maskinvara komponenten](storsimple-8000-hardware-component-replacement.md).</span><span class="sxs-lookup"><span data-stu-id="a234d-187">Learn more about [StorSimple hardware component replacement](storsimple-8000-hardware-component-replacement.md).</span></span>

