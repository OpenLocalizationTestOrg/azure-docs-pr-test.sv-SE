---
title: "Ersätt batteri på Microsoft Azure StorSimple 8000-serieenhet | Microsoft Docs"
description: "Beskriver hur du tar bort, ersätta och underhålla batterimodulen säkerhetskopiering på StorSimple-enheten."
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
ms.openlocfilehash: 174a3163082594ea6a49b7f5a78857848f8f0566
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="replace-the-backup-battery-module-on-your-storsimple-device"></a><span data-ttu-id="1778c-103">Ersätt batterimodulen säkerhetskopiering på din StorSimple-enhet</span><span class="sxs-lookup"><span data-stu-id="1778c-103">Replace the backup battery module on your StorSimple device</span></span>

## <a name="overview"></a><span data-ttu-id="1778c-104">Översikt</span><span class="sxs-lookup"><span data-stu-id="1778c-104">Overview</span></span>
<span data-ttu-id="1778c-105">Primära höljet ström och kylning modul (PCM) på Microsoft Azure StorSimple-enheten har ett extra batteri pack.</span><span class="sxs-lookup"><span data-stu-id="1778c-105">The primary enclosure Power and Cooling Module (PCM) on your Microsoft Azure StorSimple device has an additional battery pack.</span></span> <span data-ttu-id="1778c-106">Paketet innehåller power så att StorSimple-enhet kan spara data om strömavbrott AC till primära höljet.</span><span class="sxs-lookup"><span data-stu-id="1778c-106">This pack provides power so that the StorSimple device can save data if there is loss of AC power to the primary enclosure.</span></span> <span data-ttu-id="1778c-107">Detta batteri pack kallas den *säkerhetskopiering batterimodulen*.</span><span class="sxs-lookup"><span data-stu-id="1778c-107">This battery pack is referred to as the *backup battery module*.</span></span> <span data-ttu-id="1778c-108">Säkerhetskopiering batteri-modul finns bara för primära höljet i din StorSimple-enhet (EBOD höljet inte innehåller en säkerhetskopiering batteri modul).</span><span class="sxs-lookup"><span data-stu-id="1778c-108">The backup battery module exists only for the primary enclosure in your StorSimple device (the EBOD enclosure does not contain a backup battery module).</span></span>

<span data-ttu-id="1778c-109">Den här självstudiekursen beskrivs hur du:</span><span class="sxs-lookup"><span data-stu-id="1778c-109">This tutorial explains how to:</span></span>

* <span data-ttu-id="1778c-110">Ta bort batterimodulen säkerhetskopiering</span><span class="sxs-lookup"><span data-stu-id="1778c-110">Remove the backup battery module</span></span>
* <span data-ttu-id="1778c-111">Installera en ny modul som reserv batteri</span><span class="sxs-lookup"><span data-stu-id="1778c-111">Install a new backup battery module</span></span>
* <span data-ttu-id="1778c-112">Underhåll batterimodulen säkerhetskopiering</span><span class="sxs-lookup"><span data-stu-id="1778c-112">Maintain the backup battery module</span></span>

> [!IMPORTANT]
> <span data-ttu-id="1778c-113">Innan du tar bort och ersätter en reserv batteri modul, granska säkerhetsinformation i den [introduktion till ersättning av StorSimple maskinvara komponenten](storsimple-8000-hardware-component-replacement.md).</span><span class="sxs-lookup"><span data-stu-id="1778c-113">Before removing and replacing a backup battery module, review the safety information in the [Introduction to StorSimple hardware component replacement](storsimple-8000-hardware-component-replacement.md).</span></span>


## <a name="remove-the-backup-battery-module"></a><span data-ttu-id="1778c-114">Ta bort batterimodulen säkerhetskopiering</span><span class="sxs-lookup"><span data-stu-id="1778c-114">Remove the backup battery module</span></span>
<span data-ttu-id="1778c-115">Batterimodulen säkerhetskopiering för din StorSimple-enhet är en-fältutbytbar enhet.</span><span class="sxs-lookup"><span data-stu-id="1778c-115">The backup battery module for your StorSimple device is a field-replaceable unit.</span></span> <span data-ttu-id="1778c-116">Innan det installeras i PCM ska batterimodulen lagras i dess ursprungliga förpackning.</span><span class="sxs-lookup"><span data-stu-id="1778c-116">Before it is installed in the PCM, the battery module should be stored in its original packaging.</span></span> <span data-ttu-id="1778c-117">Utför följande steg för att ta bort säkerhetskopiering batteri.</span><span class="sxs-lookup"><span data-stu-id="1778c-117">Perform the following steps to remove the backup battery.</span></span>

#### <a name="to-remove-the-backup-battery-module"></a><span data-ttu-id="1778c-118">Ta bort batterimodulen säkerhetskopiering</span><span class="sxs-lookup"><span data-stu-id="1778c-118">To remove the backup battery module</span></span>
1. <span data-ttu-id="1778c-119">Gå till din StorSimple Enhetshanteraren service bladet i Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="1778c-119">In the Azure portal, go to your StorSimple Device Manager service blade.</span></span> <span data-ttu-id="1778c-120">Gå till **enheter** och väljer sedan enheten i listan över enheter.</span><span class="sxs-lookup"><span data-stu-id="1778c-120">Go to **Devices** and then select your device from the list of devices.</span></span> <span data-ttu-id="1778c-121">Gå till **övervakaren** > **maskinvara hälsa**.</span><span class="sxs-lookup"><span data-stu-id="1778c-121">Navigate to **Monitor** > **Hardware health**.</span></span> <span data-ttu-id="1778c-122">Under **delade komponenter**, se status för batteriet.</span><span class="sxs-lookup"><span data-stu-id="1778c-122">Under **Shared Components**, look at the status of the battery.</span></span>
2. <span data-ttu-id="1778c-123">Identifiera PCM där batteriet misslyckades.</span><span class="sxs-lookup"><span data-stu-id="1778c-123">Identify the PCM in which the battery has failed.</span></span> <span data-ttu-id="1778c-124">Bild 1 visar på baksidan av StorSimple-enhet.</span><span class="sxs-lookup"><span data-stu-id="1778c-124">Figure 1 shows the back of the StorSimple device.</span></span>
   
    ![Bakplan av primära Höljesmoduler](./media/storsimple-battery-replacement/IC740994.png)
   
    <span data-ttu-id="1778c-126">**Bild 1** baksidan primära enhet visar PCM och controller moduler</span><span class="sxs-lookup"><span data-stu-id="1778c-126">**Figure 1** Back of primary device showing PCM and controller modules</span></span>
   
   | <span data-ttu-id="1778c-127">Etikett</span><span class="sxs-lookup"><span data-stu-id="1778c-127">Label</span></span> | <span data-ttu-id="1778c-128">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="1778c-128">Description</span></span> |
   |:--- |:--- |
   | <span data-ttu-id="1778c-129">1</span><span class="sxs-lookup"><span data-stu-id="1778c-129">1</span></span> |<span data-ttu-id="1778c-130">PCM 0</span><span class="sxs-lookup"><span data-stu-id="1778c-130">PCM 0</span></span> |
   | <span data-ttu-id="1778c-131">2</span><span class="sxs-lookup"><span data-stu-id="1778c-131">2</span></span> |<span data-ttu-id="1778c-132">PCM 1</span><span class="sxs-lookup"><span data-stu-id="1778c-132">PCM 1</span></span> |
   | <span data-ttu-id="1778c-133">3</span><span class="sxs-lookup"><span data-stu-id="1778c-133">3</span></span> |<span data-ttu-id="1778c-134">Styrenhet 0</span><span class="sxs-lookup"><span data-stu-id="1778c-134">Controller 0</span></span> |
   | <span data-ttu-id="1778c-135">4</span><span class="sxs-lookup"><span data-stu-id="1778c-135">4</span></span> |<span data-ttu-id="1778c-136">Kontrollant 1</span><span class="sxs-lookup"><span data-stu-id="1778c-136">Controller 1</span></span> |
   
    <span data-ttu-id="1778c-137">Baserat på nummer 3 i bild 2 visas indikatorn övervakning LETT på PCM 0 som motsvarar **batteri fel** bör upplysta.</span><span class="sxs-lookup"><span data-stu-id="1778c-137">As shown by number 3 in the Figure 2, the monitoring indicator LED on PCM 0 that corresponds to **Battery Fault** should be lit.</span></span>
   
    ![Bakplan av enheten PCM övervakning indikator indikatorer](./media/storsimple-battery-replacement/IC740992.png)
   
    <span data-ttu-id="1778c-139">**Bild 2** tillbaka of PCM visar övervakning indikatorn indikatorer</span><span class="sxs-lookup"><span data-stu-id="1778c-139">**Figure 2** Back of PCM showing the monitoring indicator LEDs</span></span>
   
   | <span data-ttu-id="1778c-140">Etikett</span><span class="sxs-lookup"><span data-stu-id="1778c-140">Label</span></span> | <span data-ttu-id="1778c-141">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="1778c-141">Description</span></span> |
   |:--- |:--- |
   | <span data-ttu-id="1778c-142">1</span><span class="sxs-lookup"><span data-stu-id="1778c-142">1</span></span> |<span data-ttu-id="1778c-143">AC strömavbrott</span><span class="sxs-lookup"><span data-stu-id="1778c-143">AC power failure</span></span> |
   | <span data-ttu-id="1778c-144">2</span><span class="sxs-lookup"><span data-stu-id="1778c-144">2</span></span> |<span data-ttu-id="1778c-145">Fläkt fel</span><span class="sxs-lookup"><span data-stu-id="1778c-145">Fan failure</span></span> |
   | <span data-ttu-id="1778c-146">3</span><span class="sxs-lookup"><span data-stu-id="1778c-146">3</span></span> |<span data-ttu-id="1778c-147">Batteri fel</span><span class="sxs-lookup"><span data-stu-id="1778c-147">Battery fault</span></span> |
   | <span data-ttu-id="1778c-148">4</span><span class="sxs-lookup"><span data-stu-id="1778c-148">4</span></span> |<span data-ttu-id="1778c-149">PCM OK</span><span class="sxs-lookup"><span data-stu-id="1778c-149">PCM OK</span></span> |
   | <span data-ttu-id="1778c-150">5</span><span class="sxs-lookup"><span data-stu-id="1778c-150">5</span></span> |<span data-ttu-id="1778c-151">DC strömavbrott</span><span class="sxs-lookup"><span data-stu-id="1778c-151">DC power failure</span></span> |
   | <span data-ttu-id="1778c-152">6</span><span class="sxs-lookup"><span data-stu-id="1778c-152">6</span></span> |<span data-ttu-id="1778c-153">Batteri felfri</span><span class="sxs-lookup"><span data-stu-id="1778c-153">Battery healthy</span></span> |
3. <span data-ttu-id="1778c-154">Om du vill ta bort PCM med en misslyckad batteri, följer du stegen i [ta bort en PCM](storsimple-power-cooling-module-replacement.md#remove-a-pcm).</span><span class="sxs-lookup"><span data-stu-id="1778c-154">To remove the PCM with a failed battery, follow the steps in [Remove a PCM](storsimple-power-cooling-module-replacement.md#remove-a-pcm).</span></span>
4. <span data-ttu-id="1778c-155">Med PCM tas bort, lyfter rotera modulreferens batteri uppåt som anges i följande bild och hämtar upp till ta bort batteriet.</span><span class="sxs-lookup"><span data-stu-id="1778c-155">With the PCM removed, lift and rotate the battery module handle upward as indicated in the following figure, and pull it up to remove the battery.</span></span>
   
    ![Ta bort batteri från PCM](./media/storsimple-battery-replacement/IC741019.png)
   
    <span data-ttu-id="1778c-157">**Bild 3** tar bort batteriet från PCM</span><span class="sxs-lookup"><span data-stu-id="1778c-157">**Figure 3** Removing the battery from the PCM</span></span>
5. <span data-ttu-id="1778c-158">Placera modulen i-fältutbytbar enhet paketera.</span><span class="sxs-lookup"><span data-stu-id="1778c-158">Place the module in the field-replaceable unit packaging.</span></span>
6. <span data-ttu-id="1778c-159">Returnera den felaktiga enheten till Microsoft för rätt underhåll och hantering.</span><span class="sxs-lookup"><span data-stu-id="1778c-159">Return the defective unit to Microsoft for proper servicing and handling.</span></span>

## <a name="install-a-new-backup-battery-module"></a><span data-ttu-id="1778c-160">Installera en ny modul som reserv batteri</span><span class="sxs-lookup"><span data-stu-id="1778c-160">Install a new backup battery module</span></span>
<span data-ttu-id="1778c-161">Utför följande steg för att installera modulen ersättning batteri i PCM i primära höljet av StorSimple-enheten.</span><span class="sxs-lookup"><span data-stu-id="1778c-161">Perform the following steps to install the replacement battery module in the PCM in the primary enclosure of your StorSimple device.</span></span>

#### <a name="to-install-the-battery-module"></a><span data-ttu-id="1778c-162">Installera batterimodulen</span><span class="sxs-lookup"><span data-stu-id="1778c-162">To install the battery module</span></span>
1. <span data-ttu-id="1778c-163">Placera batterimodulen säkerhetskopiering med rätt orientering i PCM.</span><span class="sxs-lookup"><span data-stu-id="1778c-163">Place the backup battery module in the proper orientation in the PCM.</span></span>
2. <span data-ttu-id="1778c-164">Tryck ned modulreferens batteri ända till plats kopplingen.</span><span class="sxs-lookup"><span data-stu-id="1778c-164">Press down the battery module handle all the way to seat the connector.</span></span>
3. <span data-ttu-id="1778c-165">Ersätt PCM i primära höljet genom att följa riktlinjerna i [ersätta en ström och kylning modulen på StorSimple-enheten](storsimple-power-cooling-module-replacement.md).</span><span class="sxs-lookup"><span data-stu-id="1778c-165">Replace the PCM in the primary enclosure by following the guidelines in [Replace a Power and Cooling Module on your StorSimple device](storsimple-power-cooling-module-replacement.md).</span></span>
4. <span data-ttu-id="1778c-166">När ersättningen är klar, gå till din enhet och gå sedan till **övervakaren** > **maskinvara hälsa** i Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="1778c-166">After the replacement is complete, go to your device and then go to **Monitor** > **Hardware health** in the Azure portal.</span></span> <span data-ttu-id="1778c-167">Kontrollera statusen för batteri och kontrollera att installationen har lyckats.</span><span class="sxs-lookup"><span data-stu-id="1778c-167">Verify the status of the battery to make sure that the installation was successful.</span></span> <span data-ttu-id="1778c-168">Grön status anger att batteriet är felfri.</span><span class="sxs-lookup"><span data-stu-id="1778c-168">A green status indicates that the battery is healthy.</span></span>

## <a name="maintain-the-backup-battery-module"></a><span data-ttu-id="1778c-169">Underhåll batterimodulen säkerhetskopiering</span><span class="sxs-lookup"><span data-stu-id="1778c-169">Maintain the backup battery module</span></span>
<span data-ttu-id="1778c-170">I din StorSimple-enhet ger säkerhetskopiering batterimodulen power styrenheten för under ett Strömfel går förlorade.</span><span class="sxs-lookup"><span data-stu-id="1778c-170">In your StorSimple device, the backup battery module provides power to the controller during a power loss event.</span></span> <span data-ttu-id="1778c-171">Det gör att den virtuella StorSimple-enheten att spara kritiska data innan du stänger av på ett kontrollerat sätt.</span><span class="sxs-lookup"><span data-stu-id="1778c-171">It allows the StorSimple device to save critical data prior to shutting down in a controlled manner.</span></span> <span data-ttu-id="1778c-172">Systemet kan hantera två på varandra följande förlorade händelser med två fullständigt debiteras batterierna på PCMs.</span><span class="sxs-lookup"><span data-stu-id="1778c-172">With two fully charged batteries in the PCMs, the system can handle two consecutive loss events.</span></span>

<span data-ttu-id="1778c-173">I Azure-portalen på **maskinvara hälsa** under den **övervakaren** bladet anger om batteriet är fel eller närmar sig slutet av livslängd.</span><span class="sxs-lookup"><span data-stu-id="1778c-173">In the Azure portal, the **Hardware health** under the **Monitor** blade indicates whether the battery is malfunctioning or the end-of-life is approaching.</span></span> <span data-ttu-id="1778c-174">Batteristatus anges med **batteri i PCM 0** eller **batteri i PCM 1** under **delade komponenter**.</span><span class="sxs-lookup"><span data-stu-id="1778c-174">The battery status is indicated by **Battery in PCM 0** or **Battery in PCM 1** under **Shared Components**.</span></span> <span data-ttu-id="1778c-175">Det här bladet visar en **DEGRADERAD** tillstånd för det närmar sig slutet av livslängd, och **misslyckades** för nått end-för-livslängd.</span><span class="sxs-lookup"><span data-stu-id="1778c-175">This blade will show a **DEGRADED** state for end-of-life approaching, and **FAILED** for end-of-life reached.</span></span>

> [!NOTE]
> <span data-ttu-id="1778c-176">Batteriet kan rapportera **misslyckades** när enheten behöver bara att debiteras.</span><span class="sxs-lookup"><span data-stu-id="1778c-176">The battery can report **FAILED** when it simply needs to be charged.</span></span>


<span data-ttu-id="1778c-177">Om den **DEGRADERAD** status visas, rekommenderar vi följande vilka åtgärder:</span><span class="sxs-lookup"><span data-stu-id="1778c-177">If the **DEGRADED** state appears, we recommend the following course of action:</span></span>

* <span data-ttu-id="1778c-178">Systemet kan ha uppstått nyligen strömavbrott eller batterierna kan väldigt regelbundet underhåll.</span><span class="sxs-lookup"><span data-stu-id="1778c-178">The system may have experienced a recent power loss or the batteries may be undergoing periodic maintenance.</span></span> <span data-ttu-id="1778c-179">Se systemet efter 12 timmar innan du fortsätter.</span><span class="sxs-lookup"><span data-stu-id="1778c-179">Observe the system for 12 hours before proceeding.</span></span>
  
  * <span data-ttu-id="1778c-180">Om tillståndet är fortfarande **DEGRADERAD** efter 12 timmar kontinuerlig anslutning till AC ström med domänkontrollanter och PCMs körs, sedan batteriet måste ersättas.</span><span class="sxs-lookup"><span data-stu-id="1778c-180">If the state is still **DEGRADED** after 12 hours of continuous connection to AC power with the controllers and PCMs running, then the battery needs to be replaced.</span></span> <span data-ttu-id="1778c-181">Kontrollera [kontaktar Microsoft Support](storsimple-8000-contact-microsoft-support.md) för en ersättning säkerhetskopiering batteri modul.</span><span class="sxs-lookup"><span data-stu-id="1778c-181">Please [contact Microsoft Support](storsimple-8000-contact-microsoft-support.md) for a replacement backup battery module.</span></span>
  * <span data-ttu-id="1778c-182">Om tillståndet blir OK efter 12 timmar, batteriet fungerar och behövs endast en avgift för underhåll.</span><span class="sxs-lookup"><span data-stu-id="1778c-182">If the state becomes OK after 12 hours, the battery is operational, and it only needed a maintenance charge.</span></span>
* <span data-ttu-id="1778c-183">Om det inte har en associerad förlust av funktionalitet och PCM är påslagen och ansluten till ett eluttag, måste batteriet bytas.</span><span class="sxs-lookup"><span data-stu-id="1778c-183">If there has not been an associated loss of AC power and the PCM is turned on and connected to AC power, the battery needs to be replaced.</span></span> <span data-ttu-id="1778c-184">[Kontakta Microsoft Support](storsimple-8000-contact-microsoft-support.md) beställa en ersättning säkerhetskopiering batteri modul.</span><span class="sxs-lookup"><span data-stu-id="1778c-184">[Contact Microsoft Support](storsimple-8000-contact-microsoft-support.md) to order a replacement backup battery module.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="1778c-185">Avyttra misslyckade batteri enligt nationella och regionala regler.</span><span class="sxs-lookup"><span data-stu-id="1778c-185">Dispose of the failed battery according to national and regional regulations.</span></span>

## <a name="next-steps"></a><span data-ttu-id="1778c-186">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="1778c-186">Next steps</span></span>
<span data-ttu-id="1778c-187">Lär dig mer om [ersättning av StorSimple maskinvara komponenten](storsimple-8000-hardware-component-replacement.md).</span><span class="sxs-lookup"><span data-stu-id="1778c-187">Learn more about [StorSimple hardware component replacement](storsimple-8000-hardware-component-replacement.md).</span></span>

