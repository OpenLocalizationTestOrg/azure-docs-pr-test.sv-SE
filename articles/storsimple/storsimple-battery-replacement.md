---
title: "aaaReplace batteri på Microsoft Azure StorSimple-enhet | Microsoft Docs"
description: "Beskriver hur tooremove, Ersätt och underhålla hello säkerhetskopiering batteri modul på din StorSimple-enhet."
services: storsimple
documentationcenter: 
author: alkohli
manager: carmonm
editor: 
ms.assetid: 3c8a6654-4826-4883-aad8-75f332347c53
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 08/17/2016
ms.author: alkohli
ms.openlocfilehash: 542774a5f451ec7ad2bd442f88598df318d8b285
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="replace-hello-backup-battery-module-on-your-storsimple-device"></a><span data-ttu-id="6f433-103">Ersätt hello säkerhetskopiering batteri modul på din StorSimple-enhet</span><span class="sxs-lookup"><span data-stu-id="6f433-103">Replace hello backup battery module on your StorSimple device</span></span>
## <a name="overview"></a><span data-ttu-id="6f433-104">Översikt</span><span class="sxs-lookup"><span data-stu-id="6f433-104">Overview</span></span>
<span data-ttu-id="6f433-105">hello primära höljet ström och kylning modul (PCM) på din Microsoft Azure StorSimple-enhet har en extra batteri pack.</span><span class="sxs-lookup"><span data-stu-id="6f433-105">hello primary enclosure Power and Cooling Module (PCM) on your Microsoft Azure StorSimple device has an additional battery pack.</span></span> <span data-ttu-id="6f433-106">Paketet innehåller power så att hello StorSimple-enhet kan spara data om förlust av AC power toohello primära enhet.</span><span class="sxs-lookup"><span data-stu-id="6f433-106">This pack provides power so that hello StorSimple device can save data if there is loss of AC power toohello primary enclosure.</span></span> <span data-ttu-id="6f433-107">Den här batteri pack är refererad tooas hello *säkerhetskopiering batterimodulen*.</span><span class="sxs-lookup"><span data-stu-id="6f433-107">This battery pack is referred tooas hello *backup battery module*.</span></span> <span data-ttu-id="6f433-108">hello säkerhetskopiering batteri modul finns bara för hello primära höljet i din StorSimple-enhet (hello EBOD hölje innehåller inte en säkerhetskopiering batteri modul).</span><span class="sxs-lookup"><span data-stu-id="6f433-108">hello backup battery module exists only for hello primary enclosure in your StorSimple device (hello EBOD enclosure does not contain a backup battery module).</span></span> 

<span data-ttu-id="6f433-109">Den här självstudiekursen beskrivs hur du:</span><span class="sxs-lookup"><span data-stu-id="6f433-109">This tutorial explains how to:</span></span>

* <span data-ttu-id="6f433-110">Ta bort hello säkerhetskopiering batteri modul</span><span class="sxs-lookup"><span data-stu-id="6f433-110">Remove hello backup battery module</span></span> 
* <span data-ttu-id="6f433-111">Installera en ny modul som reserv batteri</span><span class="sxs-lookup"><span data-stu-id="6f433-111">Install a new backup battery module</span></span>
* <span data-ttu-id="6f433-112">Underhåll hello säkerhetskopiering batteri modul</span><span class="sxs-lookup"><span data-stu-id="6f433-112">Maintain hello backup battery module</span></span>

> [!IMPORTANT]
> <span data-ttu-id="6f433-113">Innan du tar bort och ersätter en reserv batteri modul, granska hello säkerhetsinformation i hello [ersättning av introduktion tooStorSimple maskinvara komponenten](storsimple-hardware-component-replacement.md).</span><span class="sxs-lookup"><span data-stu-id="6f433-113">Before removing and replacing a backup battery module, review hello safety information in hello [Introduction tooStorSimple hardware component replacement](storsimple-hardware-component-replacement.md).</span></span>
> 
> 

## <a name="remove-hello-backup-battery-module"></a><span data-ttu-id="6f433-114">Ta bort hello säkerhetskopiering batteri modul</span><span class="sxs-lookup"><span data-stu-id="6f433-114">Remove hello backup battery module</span></span>
<span data-ttu-id="6f433-115">hello säkerhetskopiering batteri modul för din StorSimple-enhet är en-fältutbytbar enhet.</span><span class="sxs-lookup"><span data-stu-id="6f433-115">hello backup battery module for your StorSimple device is a field-replaceable unit.</span></span> <span data-ttu-id="6f433-116">Innan det installeras i hello PCM ska hello batterimodulen lagras i dess ursprungliga förpackning.</span><span class="sxs-lookup"><span data-stu-id="6f433-116">Before it is installed in hello PCM, hello battery module should be stored in its original packaging.</span></span> <span data-ttu-id="6f433-117">Utför följande steg tooremove hello säkerhetskopiering batteri hello.</span><span class="sxs-lookup"><span data-stu-id="6f433-117">Perform hello following steps tooremove hello backup battery.</span></span>

#### <a name="tooremove-hello-backup-battery-module"></a><span data-ttu-id="6f433-118">tooremove hello säkerhetskopiering batteri modul</span><span class="sxs-lookup"><span data-stu-id="6f433-118">tooremove hello backup battery module</span></span>
1. <span data-ttu-id="6f433-119">I Hej klassiska Azure-portalen, gå för**enheter** > **Underhåll** > **maskinvarustatus**.</span><span class="sxs-lookup"><span data-stu-id="6f433-119">In hello Azure classic portal, go too**Devices** > **Maintenance** > **Hardware Status**.</span></span> <span data-ttu-id="6f433-120">Under **delade komponenter**, se hello status för hello batteri.</span><span class="sxs-lookup"><span data-stu-id="6f433-120">Under **Shared Components**, look at hello status of hello battery.</span></span>
2. <span data-ttu-id="6f433-121">Identifiera hello PCM i vilka hello batteri misslyckades.</span><span class="sxs-lookup"><span data-stu-id="6f433-121">Identify hello PCM in which hello battery has failed.</span></span> <span data-ttu-id="6f433-122">Bild 1 visar hello baksidan hello StorSimple-enhet.</span><span class="sxs-lookup"><span data-stu-id="6f433-122">Figure 1 shows hello back of hello StorSimple device.</span></span>
   
    ![Bakplan av primära Höljesmoduler](./media/storsimple-battery-replacement/IC740994.png)
   
    <span data-ttu-id="6f433-124">**Bild 1** baksidan primära enhet visar PCM och controller moduler</span><span class="sxs-lookup"><span data-stu-id="6f433-124">**Figure 1** Back of primary device showing PCM and controller modules</span></span>
   
   | <span data-ttu-id="6f433-125">Etikett</span><span class="sxs-lookup"><span data-stu-id="6f433-125">Label</span></span> | <span data-ttu-id="6f433-126">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="6f433-126">Description</span></span> |
   |:--- |:--- |
   | <span data-ttu-id="6f433-127">1</span><span class="sxs-lookup"><span data-stu-id="6f433-127">1</span></span> |<span data-ttu-id="6f433-128">PCM 0</span><span class="sxs-lookup"><span data-stu-id="6f433-128">PCM 0</span></span> |
   | <span data-ttu-id="6f433-129">2</span><span class="sxs-lookup"><span data-stu-id="6f433-129">2</span></span> |<span data-ttu-id="6f433-130">PCM 1</span><span class="sxs-lookup"><span data-stu-id="6f433-130">PCM 1</span></span> |
   | <span data-ttu-id="6f433-131">3</span><span class="sxs-lookup"><span data-stu-id="6f433-131">3</span></span> |<span data-ttu-id="6f433-132">Styrenhet 0</span><span class="sxs-lookup"><span data-stu-id="6f433-132">Controller 0</span></span> |
   | <span data-ttu-id="6f433-133">4</span><span class="sxs-lookup"><span data-stu-id="6f433-133">4</span></span> |<span data-ttu-id="6f433-134">Kontrollant 1</span><span class="sxs-lookup"><span data-stu-id="6f433-134">Controller 1</span></span> |
   
    <span data-ttu-id="6f433-135">Enligt av talet 3 i hello bild 2 hello övervakning indikatorns LEDDE på PCM 0 som motsvarar för**batteri fel** bör upplysta.</span><span class="sxs-lookup"><span data-stu-id="6f433-135">As shown by number 3 in hello Figure 2, hello monitoring indicator LED on PCM 0 that corresponds too**Battery Fault** should be lit.</span></span>
   
    ![Bakplan av enheten PCM övervakning indikator indikatorer](./media/storsimple-battery-replacement/IC740992.png)
   
    <span data-ttu-id="6f433-137">**Bild 2** tillbaka of PCM visar hello övervakning indikatorer</span><span class="sxs-lookup"><span data-stu-id="6f433-137">**Figure 2** Back of PCM showing hello monitoring indicator LEDs</span></span>
   
   | <span data-ttu-id="6f433-138">Etikett</span><span class="sxs-lookup"><span data-stu-id="6f433-138">Label</span></span> | <span data-ttu-id="6f433-139">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="6f433-139">Description</span></span> |
   |:--- |:--- |
   | <span data-ttu-id="6f433-140">1</span><span class="sxs-lookup"><span data-stu-id="6f433-140">1</span></span> |<span data-ttu-id="6f433-141">AC strömavbrott</span><span class="sxs-lookup"><span data-stu-id="6f433-141">AC power failure</span></span> |
   | <span data-ttu-id="6f433-142">2</span><span class="sxs-lookup"><span data-stu-id="6f433-142">2</span></span> |<span data-ttu-id="6f433-143">Fläkt fel</span><span class="sxs-lookup"><span data-stu-id="6f433-143">Fan failure</span></span> |
   | <span data-ttu-id="6f433-144">3</span><span class="sxs-lookup"><span data-stu-id="6f433-144">3</span></span> |<span data-ttu-id="6f433-145">Batteri fel</span><span class="sxs-lookup"><span data-stu-id="6f433-145">Battery fault</span></span> |
   | <span data-ttu-id="6f433-146">4</span><span class="sxs-lookup"><span data-stu-id="6f433-146">4</span></span> |<span data-ttu-id="6f433-147">PCM OK</span><span class="sxs-lookup"><span data-stu-id="6f433-147">PCM OK</span></span> |
   | <span data-ttu-id="6f433-148">5</span><span class="sxs-lookup"><span data-stu-id="6f433-148">5</span></span> |<span data-ttu-id="6f433-149">DC strömavbrott</span><span class="sxs-lookup"><span data-stu-id="6f433-149">DC power failure</span></span> |
   | <span data-ttu-id="6f433-150">6</span><span class="sxs-lookup"><span data-stu-id="6f433-150">6</span></span> |<span data-ttu-id="6f433-151">Batteri felfri</span><span class="sxs-lookup"><span data-stu-id="6f433-151">Battery healthy</span></span> |
3. <span data-ttu-id="6f433-152">tooremove hello PCM med en misslyckad batteri åtgärderna hello i [ta bort en PCM](storsimple-power-cooling-module-replacement.md#remove-a-pcm).</span><span class="sxs-lookup"><span data-stu-id="6f433-152">tooremove hello PCM with a failed battery, follow hello steps in [Remove a PCM](storsimple-power-cooling-module-replacement.md#remove-a-pcm).</span></span>
4. <span data-ttu-id="6f433-153">Med hello PCM tas bort, lift och rotera hello batteri modulen hanterar uppåt som anges i följande bild hello och dra in tooremove hello batteri.</span><span class="sxs-lookup"><span data-stu-id="6f433-153">With hello PCM removed, lift and rotate hello battery module handle upward as indicated in hello following figure, and pull it up tooremove hello battery.</span></span>
   
    ![Ta bort batteri från PCM](./media/storsimple-battery-replacement/IC741019.png)
   
    <span data-ttu-id="6f433-155">**Bild 3** tar bort hello batteri från hello PCM</span><span class="sxs-lookup"><span data-stu-id="6f433-155">**Figure 3** Removing hello battery from hello PCM</span></span>
5. <span data-ttu-id="6f433-156">Placera hello modul i hello-fältutbytbar enhet paketera.</span><span class="sxs-lookup"><span data-stu-id="6f433-156">Place hello module in hello field-replaceable unit packaging.</span></span>
6. <span data-ttu-id="6f433-157">Returnera hello felaktiga enheten tooMicrosoft för rätt underhåll och hantering.</span><span class="sxs-lookup"><span data-stu-id="6f433-157">Return hello defective unit tooMicrosoft for proper servicing and handling.</span></span>

## <a name="install-a-new-backup-battery-module"></a><span data-ttu-id="6f433-158">Installera en ny modul som reserv batteri</span><span class="sxs-lookup"><span data-stu-id="6f433-158">Install a new backup battery module</span></span>
<span data-ttu-id="6f433-159">Utföra hello följande steg tooinstall hello ersättning batteri modul i hello PCM i hello primära hölje av StorSimple-enheten.</span><span class="sxs-lookup"><span data-stu-id="6f433-159">Perform hello following steps tooinstall hello replacement battery module in hello PCM in hello primary enclosure of your StorSimple device.</span></span>

#### <a name="tooinstall-hello-battery-module"></a><span data-ttu-id="6f433-160">tooinstall hello batteri modul</span><span class="sxs-lookup"><span data-stu-id="6f433-160">tooinstall hello battery module</span></span>
1. <span data-ttu-id="6f433-161">Placera hello säkerhetskopiering batteri modul i hello rätt orientering i hello PCM.</span><span class="sxs-lookup"><span data-stu-id="6f433-161">Place hello backup battery module in hello proper orientation in hello PCM.</span></span>
2. <span data-ttu-id="6f433-162">Tryck ned hello batterimodulen hantera alla hello sätt tooseat hello connector.</span><span class="sxs-lookup"><span data-stu-id="6f433-162">Press down hello battery module handle all hello way tooseat hello connector.</span></span>
3. <span data-ttu-id="6f433-163">Ersätt hello PCM i hello primära hölje genom att följa hello riktlinjerna i [ersätta en ström och kylning modulen på StorSimple-enheten](storsimple-power-cooling-module-replacement.md).</span><span class="sxs-lookup"><span data-stu-id="6f433-163">Replace hello PCM in hello primary enclosure by following hello guidelines in [Replace a Power and Cooling Module on your StorSimple device](storsimple-power-cooling-module-replacement.md).</span></span>
4. <span data-ttu-id="6f433-164">När hello ersättning är klar, går för**enheter** > **Underhåll** > **maskinvarustatus** i hello klassiska Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="6f433-164">After hello replacement is complete, go too**Devices** > **Maintenance** > **Hardware Status** in hello Azure classic portal.</span></span> <span data-ttu-id="6f433-165">Kontrollera hello status för hello batteri toomake till att hello-installationen har lyckats.</span><span class="sxs-lookup"><span data-stu-id="6f433-165">Verify hello status of hello battery toomake sure that hello installation was successful.</span></span> <span data-ttu-id="6f433-166">Grön status anger att hello batteri är felfri.</span><span class="sxs-lookup"><span data-stu-id="6f433-166">A green status indicates that hello battery is healthy.</span></span>

## <a name="maintain-hello-backup-battery-module"></a><span data-ttu-id="6f433-167">Underhåll hello säkerhetskopiering batteri modul</span><span class="sxs-lookup"><span data-stu-id="6f433-167">Maintain hello backup battery module</span></span>
<span data-ttu-id="6f433-168">I din StorSimple-enhet ger hello säkerhetskopiering batterimodulen power toohello styrenhet under ett Strömfel dataförlust.</span><span class="sxs-lookup"><span data-stu-id="6f433-168">In your StorSimple device, hello backup battery module provides power toohello controller during a power loss event.</span></span> <span data-ttu-id="6f433-169">Hej StorSimple enheten toosave kritiska data tidigare tooshutting ned kan på ett kontrollerat sätt.</span><span class="sxs-lookup"><span data-stu-id="6f433-169">It allows hello StorSimple device toosave critical data prior tooshutting down in a controlled manner.</span></span> <span data-ttu-id="6f433-170">Med två fullständigt debiteras batterierna i hello PCMs kan hello system hantera två på varandra följande förlorade händelser.</span><span class="sxs-lookup"><span data-stu-id="6f433-170">With two fully charged batteries in hello PCMs, hello system can handle two consecutive loss events.</span></span>

<span data-ttu-id="6f433-171">I hello klassiska Azure-portalen, hello **maskinvarustatus** på hello **Underhåll** sidan anger om hello batteri är fel eller närmar sig hello slutet av livslängd.</span><span class="sxs-lookup"><span data-stu-id="6f433-171">In hello Azure classic portal, hello **Hardware Status** on hello **Maintenance** page indicates whether hello battery is malfunctioning or hello end-of-life is approaching.</span></span> <span data-ttu-id="6f433-172">hello batteristatus anges med **batteri i PCM 0** eller **batteri i PCM 1** under **delade komponenter**.</span><span class="sxs-lookup"><span data-stu-id="6f433-172">hello battery status is indicated by **Battery in PCM 0** or **Battery in PCM 1** under **Shared Components**.</span></span> <span data-ttu-id="6f433-173">Den här sidan visas en **DEGRADERAD** tillstånd för det närmar sig slutet av livslängd, och **misslyckades** för nått end-för-livslängd.</span><span class="sxs-lookup"><span data-stu-id="6f433-173">This page will show a **DEGRADED** state for end-of-life approaching, and **FAILED** for end-of-life reached.</span></span> 

> [!NOTE]
> <span data-ttu-id="6f433-174">hello batteri kan rapportera **misslyckades** när den behöver bara toobe debiteras.</span><span class="sxs-lookup"><span data-stu-id="6f433-174">hello battery can report **FAILED** when it simply needs toobe charged.</span></span>
> 
> 

<span data-ttu-id="6f433-175">Om hello **DEGRADERAD** status visas, rekommenderar vi hello följande åtgärd:</span><span class="sxs-lookup"><span data-stu-id="6f433-175">If hello **DEGRADED** state appears, we recommend hello following course of action:</span></span>

* <span data-ttu-id="6f433-176">hello system kan ha uppstått nyligen strömavbrott eller hello batterierna kan väldigt regelbundet underhåll.</span><span class="sxs-lookup"><span data-stu-id="6f433-176">hello system may have experienced a recent power loss or hello batteries may be undergoing periodic maintenance.</span></span> <span data-ttu-id="6f433-177">Se hello system för 12 timmar innan du fortsätter.</span><span class="sxs-lookup"><span data-stu-id="6f433-177">Observe hello system for 12 hours before proceeding.</span></span>
  
  * <span data-ttu-id="6f433-178">Om hello tillstånd är fortfarande **DEGRADERAD** efter 12 timmar kontinuerlig anslutning tooAC ström med hello domänkontrollanter och PCMs körs sedan hello batteri måste toobe ersättas.</span><span class="sxs-lookup"><span data-stu-id="6f433-178">If hello state is still **DEGRADED** after 12 hours of continuous connection tooAC power with hello controllers and PCMs running, then hello battery needs toobe replaced.</span></span> <span data-ttu-id="6f433-179">Kontrollera [kontaktar Microsoft Support](storsimple-contact-microsoft-support.md) för en ersättning säkerhetskopiering batteri modul.</span><span class="sxs-lookup"><span data-stu-id="6f433-179">Please [contact Microsoft Support](storsimple-contact-microsoft-support.md) for a replacement backup battery module.</span></span>
  * <span data-ttu-id="6f433-180">Om hello tillstånd blir OK efter 12 timmar, hello batteri fungerar och behövs endast en avgift för underhåll.</span><span class="sxs-lookup"><span data-stu-id="6f433-180">If hello state becomes OK after 12 hours, hello battery is operational, and it only needed a maintenance charge.</span></span>
* <span data-ttu-id="6f433-181">Om det inte har en associerad förlust av AC-ström och hello PCM är påslagen och ansluten tooAC power, måste hello batteri toobe ersättas.</span><span class="sxs-lookup"><span data-stu-id="6f433-181">If there has not been an associated loss of AC power and hello PCM is turned on and connected tooAC power, hello battery needs toobe replaced.</span></span> <span data-ttu-id="6f433-182">[Kontakta Microsoft Support](storsimple-contact-microsoft-support.md) tooorder en ersättning säkerhetskopiering batteri modul.</span><span class="sxs-lookup"><span data-stu-id="6f433-182">[Contact Microsoft Support](storsimple-contact-microsoft-support.md) tooorder a replacement backup battery module.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="6f433-183">Ta bort hello misslyckades batteri enligt toonational och nationella föreskrifter.</span><span class="sxs-lookup"><span data-stu-id="6f433-183">Dispose of hello failed battery according toonational and regional regulations.</span></span> 
> 
> 

## <a name="next-steps"></a><span data-ttu-id="6f433-184">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="6f433-184">Next steps</span></span>
<span data-ttu-id="6f433-185">Lär dig mer om [ersättning av StorSimple maskinvara komponenten](storsimple-hardware-component-replacement.md).</span><span class="sxs-lookup"><span data-stu-id="6f433-185">Learn more about [StorSimple hardware component replacement](storsimple-hardware-component-replacement.md).</span></span>

