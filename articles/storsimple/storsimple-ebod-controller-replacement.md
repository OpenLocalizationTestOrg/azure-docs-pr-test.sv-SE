---
title: aaaReplace en StorSimple EBOD styrenhet | Microsoft Docs
description: "Förklarar hur tooremove och Ersätt en eller båda EBOD domänkontrollanter på en StorSimple 8600-enhet."
services: storsimple
documentationcenter: 
author: alkohli
manager: carmonm
editor: 
ms.assetid: 8cbfa507-1a56-4e24-99dd-7db9abd3b850
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 08/17/2016
ms.author: alkohli
ms.openlocfilehash: 5d29de2ee30bfdd70910050eee5cfa1d293d444f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="replace-an-ebod-controller-on-your-storsimple-device"></a><span data-ttu-id="f1b42-103">Ersätta en domänkontrollant för en EBOD på din StorSimple-enhet</span><span class="sxs-lookup"><span data-stu-id="f1b42-103">Replace an EBOD controller on your StorSimple device</span></span>
## <a name="overview"></a><span data-ttu-id="f1b42-104">Översikt</span><span class="sxs-lookup"><span data-stu-id="f1b42-104">Overview</span></span>
<span data-ttu-id="f1b42-105">Den här självstudiekursen beskrivs hur tooreplace en felaktig EBOD domänkontrollant modul på Microsoft Azure StorSimple-enheten.</span><span class="sxs-lookup"><span data-stu-id="f1b42-105">This tutorial explains how tooreplace a faulty EBOD controller module on your Microsoft Azure StorSimple device.</span></span> <span data-ttu-id="f1b42-106">tooreplace en EBOD Styrenhetsmodul, måste du:</span><span class="sxs-lookup"><span data-stu-id="f1b42-106">tooreplace an EBOD controller module, you need to:</span></span>

* <span data-ttu-id="f1b42-107">Ta bort hello felaktiga EBOD domänkontrollant</span><span class="sxs-lookup"><span data-stu-id="f1b42-107">Remove hello faulty EBOD controller</span></span>
* <span data-ttu-id="f1b42-108">Installera en ny domänkontrollant EBOD</span><span class="sxs-lookup"><span data-stu-id="f1b42-108">Install a new EBOD controller</span></span>

<span data-ttu-id="f1b42-109">Överväg följande information innan du börjar hello:</span><span class="sxs-lookup"><span data-stu-id="f1b42-109">Consider hello following information before you begin:</span></span>

* <span data-ttu-id="f1b42-110">Tom EBOD moduler måste infogas i alla oanvända platser.</span><span class="sxs-lookup"><span data-stu-id="f1b42-110">Blank EBOD modules must be inserted into all unused slots.</span></span> <span data-ttu-id="f1b42-111">hello höljet ska inte kall korrekt om en plats är öppna.</span><span class="sxs-lookup"><span data-stu-id="f1b42-111">hello enclosure will not cool properly if a slot is left open.</span></span>
* <span data-ttu-id="f1b42-112">Hej EBOD domänkontrollanten är växlas och kan tas bort eller ersättas.</span><span class="sxs-lookup"><span data-stu-id="f1b42-112">hello EBOD controller is hot-swappable and can be removed or replaced.</span></span> <span data-ttu-id="f1b42-113">Ta inte bort en misslyckad modul tills du har en ersättning.</span><span class="sxs-lookup"><span data-stu-id="f1b42-113">Do not remove a failed module until you have a replacement.</span></span> <span data-ttu-id="f1b42-114">När du initierar hello ersättning process, måste du slutföra det inom 10 minuter.</span><span class="sxs-lookup"><span data-stu-id="f1b42-114">When you initiate hello replacement process, you must finish it within 10 minutes.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="f1b42-115">Innan du försöker tooremove eller ersätta en StorSimple-komponent, se till att du granskar hello [säkerhet ikonen konventioner](storsimple-safety.md#safety-icon-conventions) och andra [säkerhetsåtgärderna](storsimple-safety.md).</span><span class="sxs-lookup"><span data-stu-id="f1b42-115">Before attempting tooremove or replace any StorSimple component, make sure that you review hello [safety icon conventions](storsimple-safety.md#safety-icon-conventions) and other [safety precautions](storsimple-safety.md).</span></span>
> 
> 

## <a name="remove-an-ebod-controller"></a><span data-ttu-id="f1b42-116">Ta bort en domänkontrollant för en EBOD</span><span class="sxs-lookup"><span data-stu-id="f1b42-116">Remove an EBOD controller</span></span>
<span data-ttu-id="f1b42-117">Ersätta hello misslyckades EBOD domänkontrollant modul i din StorSimple-enhet, kontrollera att den hello andra EBOD domänkontrollant modulen är aktiv och körs.</span><span class="sxs-lookup"><span data-stu-id="f1b42-117">Before replacing hello failed EBOD controller module in your StorSimple device, make sure that hello other EBOD controller module is active and running.</span></span> <span data-ttu-id="f1b42-118">hello förklarar följande procedur och tabell hur tooremove hello EBOD domänkontrollant modulen.</span><span class="sxs-lookup"><span data-stu-id="f1b42-118">hello following procedure and table explain how tooremove hello EBOD controller module.</span></span>

#### <a name="tooremove-an-ebod-module"></a><span data-ttu-id="f1b42-119">tooremove en EBOD-modul</span><span class="sxs-lookup"><span data-stu-id="f1b42-119">tooremove an EBOD module</span></span>
1. <span data-ttu-id="f1b42-120">Öppna hello klassiska Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="f1b42-120">Open hello Azure classic portal.</span></span>
2. <span data-ttu-id="f1b42-121">Navigera för**enheter** > **Underhåll** > **maskinvarustatus**, och kontrollera att hello status för hello LETT för hello active EBOD domänkontrollanten modulen är grön och hello Indikator för hello misslyckades EBOD domänkontrollant modulen är röda.</span><span class="sxs-lookup"><span data-stu-id="f1b42-121">Navigate too**Devices** > **Maintenance** > **Hardware Status**, and verify that hello status of hello LED for hello active EBOD controller module is green and hello LED for hello failed EBOD controller module is red.</span></span>
3. <span data-ttu-id="f1b42-122">Leta upp hello misslyckades EBOD domänkontrollant modulen på hello baksidan hello enhet.</span><span class="sxs-lookup"><span data-stu-id="f1b42-122">Locate hello failed EBOD controller module at hello back of hello device.</span></span>
4. <span data-ttu-id="f1b42-123">Ta bort hello kablar som ansluter hello EBOD domänkontrollant modulen toohello domänkontrollant innan du tar hello EBOD modulen utanför hello system.</span><span class="sxs-lookup"><span data-stu-id="f1b42-123">Remove hello cables that connect hello EBOD controller module toohello controller before taking hello EBOD module out of hello system.</span></span>
5. <span data-ttu-id="f1b42-124">Anteckna hello exakt SAS-port för hello EBOD domänkontrollant modul som var anslutna toohello domänkontrollant.</span><span class="sxs-lookup"><span data-stu-id="f1b42-124">Make a note of hello exact SAS port of hello EBOD controller module that was connected toohello controller.</span></span> <span data-ttu-id="f1b42-125">När du ersätter hello EBOD modulen kommer du att nödvändiga toorestore hello toothis systemkonfigurationen.</span><span class="sxs-lookup"><span data-stu-id="f1b42-125">You will be required toorestore hello system toothis configuration after you replace hello EBOD module.</span></span> 
   
   > [!NOTE]
   > <span data-ttu-id="f1b42-126">Normalt Port A, som är märkta som blir **värd i** i hello följande diagram.</span><span class="sxs-lookup"><span data-stu-id="f1b42-126">Typically, this will be Port A, which is labeled as **Host in** in hello following diagram.</span></span>
   > 
   > 
   
    ![Bakplan EBOD domänkontrollant](./media/storsimple-ebod-controller-replacement/IC741049.png)
   
     <span data-ttu-id="f1b42-128">**Bild 1** tillbaka of EBOD modul</span><span class="sxs-lookup"><span data-stu-id="f1b42-128">**Figure 1** Back of EBOD module</span></span>
   
   | <span data-ttu-id="f1b42-129">Etikett</span><span class="sxs-lookup"><span data-stu-id="f1b42-129">Label</span></span> | <span data-ttu-id="f1b42-130">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="f1b42-130">Description</span></span> |
   |:--- |:--- |
   | <span data-ttu-id="f1b42-131">1</span><span class="sxs-lookup"><span data-stu-id="f1b42-131">1</span></span> |<span data-ttu-id="f1b42-132">Fel Indikator</span><span class="sxs-lookup"><span data-stu-id="f1b42-132">Fault LED</span></span> |
   | <span data-ttu-id="f1b42-133">2</span><span class="sxs-lookup"><span data-stu-id="f1b42-133">2</span></span> |<span data-ttu-id="f1b42-134">Indikator för ström</span><span class="sxs-lookup"><span data-stu-id="f1b42-134">Power LED</span></span> |
   | <span data-ttu-id="f1b42-135">3</span><span class="sxs-lookup"><span data-stu-id="f1b42-135">3</span></span> |<span data-ttu-id="f1b42-136">SAS-anslutningar</span><span class="sxs-lookup"><span data-stu-id="f1b42-136">SAS connectors</span></span> |
   | <span data-ttu-id="f1b42-137">4</span><span class="sxs-lookup"><span data-stu-id="f1b42-137">4</span></span> |<span data-ttu-id="f1b42-138">SAS-indikatorer</span><span class="sxs-lookup"><span data-stu-id="f1b42-138">SAS LEDs</span></span> |
   | <span data-ttu-id="f1b42-139">5</span><span class="sxs-lookup"><span data-stu-id="f1b42-139">5</span></span> |<span data-ttu-id="f1b42-140">Serieportar factory endast för användning</span><span class="sxs-lookup"><span data-stu-id="f1b42-140">Serial ports for factory use only</span></span> |
   | <span data-ttu-id="f1b42-141">6</span><span class="sxs-lookup"><span data-stu-id="f1b42-141">6</span></span> |<span data-ttu-id="f1b42-142">Port en (värd i)</span><span class="sxs-lookup"><span data-stu-id="f1b42-142">Port A (Host in)</span></span> |
   | <span data-ttu-id="f1b42-143">7</span><span class="sxs-lookup"><span data-stu-id="f1b42-143">7</span></span> |<span data-ttu-id="f1b42-144">Port B (Host ut)</span><span class="sxs-lookup"><span data-stu-id="f1b42-144">Port B (Host out)</span></span> |
   | <span data-ttu-id="f1b42-145">8</span><span class="sxs-lookup"><span data-stu-id="f1b42-145">8</span></span> |<span data-ttu-id="f1b42-146">Port C (endast Factory användning)</span><span class="sxs-lookup"><span data-stu-id="f1b42-146">Port C (Factory use only)</span></span> |

## <a name="install-a-new-ebod-controller"></a><span data-ttu-id="f1b42-147">Installera en ny domänkontrollant EBOD</span><span class="sxs-lookup"><span data-stu-id="f1b42-147">Install a new EBOD controller</span></span>
<span data-ttu-id="f1b42-148">hello följande procedur och tabell förklarar hur tooinstall en EBOD Styrenhetsmodul i din StorSimple-enhet.</span><span class="sxs-lookup"><span data-stu-id="f1b42-148">hello following procedure and table explain how tooinstall an EBOD controller module in your StorSimple device.</span></span>

#### <a name="tooinstall-an-ebod-controller"></a><span data-ttu-id="f1b42-149">tooinstall EBOD-styrenhet</span><span class="sxs-lookup"><span data-stu-id="f1b42-149">tooinstall an EBOD controller</span></span>
1. <span data-ttu-id="f1b42-150">Kontrollera hello EBOD enheten skador, särskilt toohello gränssnittet connector.</span><span class="sxs-lookup"><span data-stu-id="f1b42-150">Check hello EBOD device for damage, especially toohello interface connector.</span></span> <span data-ttu-id="f1b42-151">Installera inte hello nya EBOD styrenhet om någon PIN-koder är böjda.</span><span class="sxs-lookup"><span data-stu-id="f1b42-151">Do not install hello new EBOD controller if any pins are bent.</span></span>
2. <span data-ttu-id="f1b42-152">Öppna position, bild hello-modulen till hello höljet tills hello Lås interagera med hello Lås i hello.</span><span class="sxs-lookup"><span data-stu-id="f1b42-152">With hello latches in hello open position, slide hello module into hello enclosure until hello latches engage.</span></span>
   
    ![Installera EBOD domänkontrollant](./media/storsimple-ebod-controller-replacement/IC741050.png)
   
    <span data-ttu-id="f1b42-154">**Bild 2** installerar hello EBOD domänkontrollant modul</span><span class="sxs-lookup"><span data-stu-id="f1b42-154">**Figure 2**  Installing hello EBOD controller module</span></span>
3. <span data-ttu-id="f1b42-155">Stäng hello spärren.</span><span class="sxs-lookup"><span data-stu-id="f1b42-155">Close hello latch.</span></span> <span data-ttu-id="f1b42-156">Du bör höra en klickning som snabbt tillkallar hello spärren.</span><span class="sxs-lookup"><span data-stu-id="f1b42-156">You should hear a click as hello latch engages.</span></span>
   
    ![Släppa EBOD spärren](./media/storsimple-ebod-controller-replacement/IC741047.png)
   
    <span data-ttu-id="f1b42-158">**Bild 3** stänger hello EBOD modulen spärren</span><span class="sxs-lookup"><span data-stu-id="f1b42-158">**Figure 3**  Closing hello EBOD module latch</span></span>
4. <span data-ttu-id="f1b42-159">Återanslut hello-kablar.</span><span class="sxs-lookup"><span data-stu-id="f1b42-159">Reconnect hello cables.</span></span> <span data-ttu-id="f1b42-160">Använd hello exakta konfigurationen som fanns innan hello ersättning.</span><span class="sxs-lookup"><span data-stu-id="f1b42-160">Use hello exact configuration that was present before hello replacement.</span></span> <span data-ttu-id="f1b42-161">Se följande diagram hello och för information om hur tooconnect hello kablar.</span><span class="sxs-lookup"><span data-stu-id="f1b42-161">See hello following diagram and table for details about how tooconnect hello cables.</span></span>
   
    ![Kabelansluta den 4U för ström](./media/storsimple-ebod-controller-replacement/IC770723.png)
   
    <span data-ttu-id="f1b42-163">**Bild 4**.</span><span class="sxs-lookup"><span data-stu-id="f1b42-163">**Figure 4**.</span></span> <span data-ttu-id="f1b42-164">Ansluta kablar</span><span class="sxs-lookup"><span data-stu-id="f1b42-164">Reconnecting cables</span></span>
   
   | <span data-ttu-id="f1b42-165">Etikett</span><span class="sxs-lookup"><span data-stu-id="f1b42-165">Label</span></span> | <span data-ttu-id="f1b42-166">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="f1b42-166">Description</span></span> |
   |:--- |:--- |
   | <span data-ttu-id="f1b42-167">1</span><span class="sxs-lookup"><span data-stu-id="f1b42-167">1</span></span> |<span data-ttu-id="f1b42-168">Primär enhet</span><span class="sxs-lookup"><span data-stu-id="f1b42-168">Primary enclosure</span></span> |
   | <span data-ttu-id="f1b42-169">2</span><span class="sxs-lookup"><span data-stu-id="f1b42-169">2</span></span> |<span data-ttu-id="f1b42-170">PCM 0</span><span class="sxs-lookup"><span data-stu-id="f1b42-170">PCM 0</span></span> |
   | <span data-ttu-id="f1b42-171">3</span><span class="sxs-lookup"><span data-stu-id="f1b42-171">3</span></span> |<span data-ttu-id="f1b42-172">PCM 1</span><span class="sxs-lookup"><span data-stu-id="f1b42-172">PCM 1</span></span> |
   | <span data-ttu-id="f1b42-173">4</span><span class="sxs-lookup"><span data-stu-id="f1b42-173">4</span></span> |<span data-ttu-id="f1b42-174">Styrenhet 0</span><span class="sxs-lookup"><span data-stu-id="f1b42-174">Controller 0</span></span> |
   | <span data-ttu-id="f1b42-175">5</span><span class="sxs-lookup"><span data-stu-id="f1b42-175">5</span></span> |<span data-ttu-id="f1b42-176">Kontrollant 1</span><span class="sxs-lookup"><span data-stu-id="f1b42-176">Controller 1</span></span> |
   | <span data-ttu-id="f1b42-177">6</span><span class="sxs-lookup"><span data-stu-id="f1b42-177">6</span></span> |<span data-ttu-id="f1b42-178">EBOD styrenhet 0</span><span class="sxs-lookup"><span data-stu-id="f1b42-178">EBOD controller 0</span></span> |
   | <span data-ttu-id="f1b42-179">7</span><span class="sxs-lookup"><span data-stu-id="f1b42-179">7</span></span> |<span data-ttu-id="f1b42-180">EBOD kontrollant 1</span><span class="sxs-lookup"><span data-stu-id="f1b42-180">EBOD controller 1</span></span> |
   | <span data-ttu-id="f1b42-181">8</span><span class="sxs-lookup"><span data-stu-id="f1b42-181">8</span></span> |<span data-ttu-id="f1b42-182">EBOD hölje</span><span class="sxs-lookup"><span data-stu-id="f1b42-182">EBOD enclosure</span></span> |
   | <span data-ttu-id="f1b42-183">9</span><span class="sxs-lookup"><span data-stu-id="f1b42-183">9</span></span> |<span data-ttu-id="f1b42-184">Kraftfördelningsenheter</span><span class="sxs-lookup"><span data-stu-id="f1b42-184">Power Distribution Units</span></span> |

## <a name="next-steps"></a><span data-ttu-id="f1b42-185">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="f1b42-185">Next steps</span></span>
<span data-ttu-id="f1b42-186">Lär dig mer om [ersättning av StorSimple maskinvara komponenten](storsimple-hardware-component-replacement.md).</span><span class="sxs-lookup"><span data-stu-id="f1b42-186">Learn more about [StorSimple hardware component replacement](storsimple-hardware-component-replacement.md).</span></span>

