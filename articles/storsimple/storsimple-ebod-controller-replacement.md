---
title: "Ersätta en StorSimple EBOD styrenhet | Microsoft Docs"
description: "Beskriver hur du tar bort och ersätter en eller båda EBOD domänkontrollanter på en StorSimple 8600-enhet."
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
ms.openlocfilehash: 23d819ddc3bbcbaf2847cdcc9191407ead0ff43d
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="replace-an-ebod-controller-on-your-storsimple-device"></a><span data-ttu-id="1c906-103">Ersätta en domänkontrollant för en EBOD på din StorSimple-enhet</span><span class="sxs-lookup"><span data-stu-id="1c906-103">Replace an EBOD controller on your StorSimple device</span></span>
## <a name="overview"></a><span data-ttu-id="1c906-104">Översikt</span><span class="sxs-lookup"><span data-stu-id="1c906-104">Overview</span></span>
<span data-ttu-id="1c906-105">Den här självstudiekursen beskrivs hur du ersätta en felande EBOD domänkontrollant modul på Microsoft Azure StorSimple-enheten.</span><span class="sxs-lookup"><span data-stu-id="1c906-105">This tutorial explains how to replace a faulty EBOD controller module on your Microsoft Azure StorSimple device.</span></span> <span data-ttu-id="1c906-106">Om du vill ersätta en EBOD Styrenhetsmodul, måste du:</span><span class="sxs-lookup"><span data-stu-id="1c906-106">To replace an EBOD controller module, you need to:</span></span>

* <span data-ttu-id="1c906-107">Ta bort felaktiga EBOD-styrenhet</span><span class="sxs-lookup"><span data-stu-id="1c906-107">Remove the faulty EBOD controller</span></span>
* <span data-ttu-id="1c906-108">Installera en ny domänkontrollant EBOD</span><span class="sxs-lookup"><span data-stu-id="1c906-108">Install a new EBOD controller</span></span>

<span data-ttu-id="1c906-109">Tänk på följande innan du börjar:</span><span class="sxs-lookup"><span data-stu-id="1c906-109">Consider the following information before you begin:</span></span>

* <span data-ttu-id="1c906-110">Tom EBOD moduler måste infogas i alla oanvända platser.</span><span class="sxs-lookup"><span data-stu-id="1c906-110">Blank EBOD modules must be inserted into all unused slots.</span></span> <span data-ttu-id="1c906-111">Höljet ska inte kall korrekt om en plats är öppna.</span><span class="sxs-lookup"><span data-stu-id="1c906-111">The enclosure will not cool properly if a slot is left open.</span></span>
* <span data-ttu-id="1c906-112">EBOD domänkontrollant är växlas och kan tas bort eller ersättas.</span><span class="sxs-lookup"><span data-stu-id="1c906-112">The EBOD controller is hot-swappable and can be removed or replaced.</span></span> <span data-ttu-id="1c906-113">Ta inte bort en misslyckad modul tills du har en ersättning.</span><span class="sxs-lookup"><span data-stu-id="1c906-113">Do not remove a failed module until you have a replacement.</span></span> <span data-ttu-id="1c906-114">När du startar ersättningsprocessen, måste du slutföra det inom 10 minuter.</span><span class="sxs-lookup"><span data-stu-id="1c906-114">When you initiate the replacement process, you must finish it within 10 minutes.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="1c906-115">Innan du försöker ta bort eller ersätta en StorSimple-komponent, se till att du läser den [säkerhet ikonen konventioner](storsimple-safety.md#safety-icon-conventions) och andra [säkerhetsåtgärderna](storsimple-safety.md).</span><span class="sxs-lookup"><span data-stu-id="1c906-115">Before attempting to remove or replace any StorSimple component, make sure that you review the [safety icon conventions](storsimple-safety.md#safety-icon-conventions) and other [safety precautions](storsimple-safety.md).</span></span>
> 
> 

## <a name="remove-an-ebod-controller"></a><span data-ttu-id="1c906-116">Ta bort en domänkontrollant för en EBOD</span><span class="sxs-lookup"><span data-stu-id="1c906-116">Remove an EBOD controller</span></span>
<span data-ttu-id="1c906-117">Kontrollera att andra EBOD domänkontrollant modulen är aktiv och körs innan du ersätter den misslyckade EBOD domänkontrollant modulen i din StorSimple-enhet.</span><span class="sxs-lookup"><span data-stu-id="1c906-117">Before replacing the failed EBOD controller module in your StorSimple device, make sure that the other EBOD controller module is active and running.</span></span> <span data-ttu-id="1c906-118">Följande procedur och tabell förklarar hur du tar bort EBOD domänkontrollant modul.</span><span class="sxs-lookup"><span data-stu-id="1c906-118">The following procedure and table explain how to remove the EBOD controller module.</span></span>

#### <a name="to-remove-an-ebod-module"></a><span data-ttu-id="1c906-119">Ta bort en EBOD-modul</span><span class="sxs-lookup"><span data-stu-id="1c906-119">To remove an EBOD module</span></span>
1. <span data-ttu-id="1c906-120">Öppna den klassiska Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="1c906-120">Open the Azure classic portal.</span></span>
2. <span data-ttu-id="1c906-121">Gå till **enheter** > **Underhåll** > **maskinvarustatus**, och kontrollera att status för Indikator för modulen controller active EBOD är grön Indikator för modulen controller misslyckade EBOD är röda.</span><span class="sxs-lookup"><span data-stu-id="1c906-121">Navigate to **Devices** > **Maintenance** > **Hardware Status**, and verify that the status of the LED for the active EBOD controller module is green and the LED for the failed EBOD controller module is red.</span></span>
3. <span data-ttu-id="1c906-122">Leta upp modulen misslyckade EBOD domänkontrollant på baksidan av enheten.</span><span class="sxs-lookup"><span data-stu-id="1c906-122">Locate the failed EBOD controller module at the back of the device.</span></span>
4. <span data-ttu-id="1c906-123">Ta bort kablarna som ansluter modulen EBOD domänkontrollant till styrenhet innan du tar modulen EBOD från systemet.</span><span class="sxs-lookup"><span data-stu-id="1c906-123">Remove the cables that connect the EBOD controller module to the controller before taking the EBOD module out of the system.</span></span>
5. <span data-ttu-id="1c906-124">Anteckna exakt SAS-porten för modulen EBOD domänkontrollant som var ansluten till styrenhet.</span><span class="sxs-lookup"><span data-stu-id="1c906-124">Make a note of the exact SAS port of the EBOD controller module that was connected to the controller.</span></span> <span data-ttu-id="1c906-125">Du behöver återställa datorn till den här konfigurationen när du ersätter EBOD-modulen.</span><span class="sxs-lookup"><span data-stu-id="1c906-125">You will be required to restore the system to this configuration after you replace the EBOD module.</span></span> 
   
   > [!NOTE]
   > <span data-ttu-id="1c906-126">Normalt Port A, som är märkta som blir **värd i** i följande diagram.</span><span class="sxs-lookup"><span data-stu-id="1c906-126">Typically, this will be Port A, which is labeled as **Host in** in the following diagram.</span></span>
   > 
   > 
   
    ![Bakplan EBOD domänkontrollant](./media/storsimple-ebod-controller-replacement/IC741049.png)
   
     <span data-ttu-id="1c906-128">**Bild 1** tillbaka of EBOD modul</span><span class="sxs-lookup"><span data-stu-id="1c906-128">**Figure 1** Back of EBOD module</span></span>
   
   | <span data-ttu-id="1c906-129">Etikett</span><span class="sxs-lookup"><span data-stu-id="1c906-129">Label</span></span> | <span data-ttu-id="1c906-130">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="1c906-130">Description</span></span> |
   |:--- |:--- |
   | <span data-ttu-id="1c906-131">1</span><span class="sxs-lookup"><span data-stu-id="1c906-131">1</span></span> |<span data-ttu-id="1c906-132">Fel Indikator</span><span class="sxs-lookup"><span data-stu-id="1c906-132">Fault LED</span></span> |
   | <span data-ttu-id="1c906-133">2</span><span class="sxs-lookup"><span data-stu-id="1c906-133">2</span></span> |<span data-ttu-id="1c906-134">Indikator för ström</span><span class="sxs-lookup"><span data-stu-id="1c906-134">Power LED</span></span> |
   | <span data-ttu-id="1c906-135">3</span><span class="sxs-lookup"><span data-stu-id="1c906-135">3</span></span> |<span data-ttu-id="1c906-136">SAS-anslutningar</span><span class="sxs-lookup"><span data-stu-id="1c906-136">SAS connectors</span></span> |
   | <span data-ttu-id="1c906-137">4</span><span class="sxs-lookup"><span data-stu-id="1c906-137">4</span></span> |<span data-ttu-id="1c906-138">SAS-indikatorer</span><span class="sxs-lookup"><span data-stu-id="1c906-138">SAS LEDs</span></span> |
   | <span data-ttu-id="1c906-139">5</span><span class="sxs-lookup"><span data-stu-id="1c906-139">5</span></span> |<span data-ttu-id="1c906-140">Serieportar factory endast för användning</span><span class="sxs-lookup"><span data-stu-id="1c906-140">Serial ports for factory use only</span></span> |
   | <span data-ttu-id="1c906-141">6</span><span class="sxs-lookup"><span data-stu-id="1c906-141">6</span></span> |<span data-ttu-id="1c906-142">Port en (värd i)</span><span class="sxs-lookup"><span data-stu-id="1c906-142">Port A (Host in)</span></span> |
   | <span data-ttu-id="1c906-143">7</span><span class="sxs-lookup"><span data-stu-id="1c906-143">7</span></span> |<span data-ttu-id="1c906-144">Port B (Host ut)</span><span class="sxs-lookup"><span data-stu-id="1c906-144">Port B (Host out)</span></span> |
   | <span data-ttu-id="1c906-145">8</span><span class="sxs-lookup"><span data-stu-id="1c906-145">8</span></span> |<span data-ttu-id="1c906-146">Port C (endast Factory användning)</span><span class="sxs-lookup"><span data-stu-id="1c906-146">Port C (Factory use only)</span></span> |

## <a name="install-a-new-ebod-controller"></a><span data-ttu-id="1c906-147">Installera en ny domänkontrollant EBOD</span><span class="sxs-lookup"><span data-stu-id="1c906-147">Install a new EBOD controller</span></span>
<span data-ttu-id="1c906-148">Följande procedur och tabell förklarar hur du installerar en domänkontrollant EBOD-modul i din StorSimple-enhet.</span><span class="sxs-lookup"><span data-stu-id="1c906-148">The following procedure and table explain how to install an EBOD controller module in your StorSimple device.</span></span>

#### <a name="to-install-an-ebod-controller"></a><span data-ttu-id="1c906-149">Att installera en domänkontrollant för en EBOD</span><span class="sxs-lookup"><span data-stu-id="1c906-149">To install an EBOD controller</span></span>
1. <span data-ttu-id="1c906-150">Kontrollera enheten EBOD skador, särskilt för att gränssnittet kopplingen.</span><span class="sxs-lookup"><span data-stu-id="1c906-150">Check the EBOD device for damage, especially to the interface connector.</span></span> <span data-ttu-id="1c906-151">Installera inte nya EBOD styrenhet om någon PIN-koder är böjda.</span><span class="sxs-lookup"><span data-stu-id="1c906-151">Do not install the new EBOD controller if any pins are bent.</span></span>
2. <span data-ttu-id="1c906-152">Skjut modulen i höljet med lås i öppet läge tills Lås engagera.</span><span class="sxs-lookup"><span data-stu-id="1c906-152">With the latches in the open position, slide the module into the enclosure until the latches engage.</span></span>
   
    ![Installera EBOD domänkontrollant](./media/storsimple-ebod-controller-replacement/IC741050.png)
   
    <span data-ttu-id="1c906-154">**Bild 2** installera modulen EBOD domänkontrollant</span><span class="sxs-lookup"><span data-stu-id="1c906-154">**Figure 2**  Installing the EBOD controller module</span></span>
3. <span data-ttu-id="1c906-155">Stäng låset.</span><span class="sxs-lookup"><span data-stu-id="1c906-155">Close the latch.</span></span> <span data-ttu-id="1c906-156">Du bör höra en klickning som snabbt tillkallar låset.</span><span class="sxs-lookup"><span data-stu-id="1c906-156">You should hear a click as the latch engages.</span></span>
   
    ![Släppa EBOD spärren](./media/storsimple-ebod-controller-replacement/IC741047.png)
   
    <span data-ttu-id="1c906-158">**Bild 3** stänger EBOD modulen spärren</span><span class="sxs-lookup"><span data-stu-id="1c906-158">**Figure 3**  Closing the EBOD module latch</span></span>
4. <span data-ttu-id="1c906-159">Återanslut kablarna.</span><span class="sxs-lookup"><span data-stu-id="1c906-159">Reconnect the cables.</span></span> <span data-ttu-id="1c906-160">Använd den exakta konfigurationen som fanns innan du ersätter.</span><span class="sxs-lookup"><span data-stu-id="1c906-160">Use the exact configuration that was present before the replacement.</span></span> <span data-ttu-id="1c906-161">Finns i följande diagram och tabell för information om hur du ansluter kablarna.</span><span class="sxs-lookup"><span data-stu-id="1c906-161">See the following diagram and table for details about how to connect the cables.</span></span>
   
    ![Kabelansluta den 4U för ström](./media/storsimple-ebod-controller-replacement/IC770723.png)
   
    <span data-ttu-id="1c906-163">**Bild 4**.</span><span class="sxs-lookup"><span data-stu-id="1c906-163">**Figure 4**.</span></span> <span data-ttu-id="1c906-164">Ansluta kablar</span><span class="sxs-lookup"><span data-stu-id="1c906-164">Reconnecting cables</span></span>
   
   | <span data-ttu-id="1c906-165">Etikett</span><span class="sxs-lookup"><span data-stu-id="1c906-165">Label</span></span> | <span data-ttu-id="1c906-166">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="1c906-166">Description</span></span> |
   |:--- |:--- |
   | <span data-ttu-id="1c906-167">1</span><span class="sxs-lookup"><span data-stu-id="1c906-167">1</span></span> |<span data-ttu-id="1c906-168">Primär enhet</span><span class="sxs-lookup"><span data-stu-id="1c906-168">Primary enclosure</span></span> |
   | <span data-ttu-id="1c906-169">2</span><span class="sxs-lookup"><span data-stu-id="1c906-169">2</span></span> |<span data-ttu-id="1c906-170">PCM 0</span><span class="sxs-lookup"><span data-stu-id="1c906-170">PCM 0</span></span> |
   | <span data-ttu-id="1c906-171">3</span><span class="sxs-lookup"><span data-stu-id="1c906-171">3</span></span> |<span data-ttu-id="1c906-172">PCM 1</span><span class="sxs-lookup"><span data-stu-id="1c906-172">PCM 1</span></span> |
   | <span data-ttu-id="1c906-173">4</span><span class="sxs-lookup"><span data-stu-id="1c906-173">4</span></span> |<span data-ttu-id="1c906-174">Styrenhet 0</span><span class="sxs-lookup"><span data-stu-id="1c906-174">Controller 0</span></span> |
   | <span data-ttu-id="1c906-175">5</span><span class="sxs-lookup"><span data-stu-id="1c906-175">5</span></span> |<span data-ttu-id="1c906-176">Kontrollant 1</span><span class="sxs-lookup"><span data-stu-id="1c906-176">Controller 1</span></span> |
   | <span data-ttu-id="1c906-177">6</span><span class="sxs-lookup"><span data-stu-id="1c906-177">6</span></span> |<span data-ttu-id="1c906-178">EBOD styrenhet 0</span><span class="sxs-lookup"><span data-stu-id="1c906-178">EBOD controller 0</span></span> |
   | <span data-ttu-id="1c906-179">7</span><span class="sxs-lookup"><span data-stu-id="1c906-179">7</span></span> |<span data-ttu-id="1c906-180">EBOD kontrollant 1</span><span class="sxs-lookup"><span data-stu-id="1c906-180">EBOD controller 1</span></span> |
   | <span data-ttu-id="1c906-181">8</span><span class="sxs-lookup"><span data-stu-id="1c906-181">8</span></span> |<span data-ttu-id="1c906-182">EBOD hölje</span><span class="sxs-lookup"><span data-stu-id="1c906-182">EBOD enclosure</span></span> |
   | <span data-ttu-id="1c906-183">9</span><span class="sxs-lookup"><span data-stu-id="1c906-183">9</span></span> |<span data-ttu-id="1c906-184">Kraftfördelningsenheter</span><span class="sxs-lookup"><span data-stu-id="1c906-184">Power Distribution Units</span></span> |

## <a name="next-steps"></a><span data-ttu-id="1c906-185">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="1c906-185">Next steps</span></span>
<span data-ttu-id="1c906-186">Lär dig mer om [ersättning av StorSimple maskinvara komponenten](storsimple-hardware-component-replacement.md).</span><span class="sxs-lookup"><span data-stu-id="1c906-186">Learn more about [StorSimple hardware component replacement](storsimple-hardware-component-replacement.md).</span></span>

