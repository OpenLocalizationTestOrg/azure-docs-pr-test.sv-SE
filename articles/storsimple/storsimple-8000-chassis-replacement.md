---
title: "Ersätt chassi på StorSimple 8000-serieenhet | Microsoft Docs"
description: "Beskriver hur du tar bort och ersätter chassi för din StorSimple primära hölje eller EBOD hölje."
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
ms.openlocfilehash: 073fcf0064f1d1482f4683d733f00cf918ff2f38
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="replace-the-chassis-on-your-storsimple-device"></a><span data-ttu-id="355c7-103">Ersätt chassi på din StorSimple-enhet</span><span class="sxs-lookup"><span data-stu-id="355c7-103">Replace the chassis on your StorSimple device</span></span>
## <a name="overview"></a><span data-ttu-id="355c7-104">Översikt</span><span class="sxs-lookup"><span data-stu-id="355c7-104">Overview</span></span>
<span data-ttu-id="355c7-105">Den här självstudiekursen beskrivs hur du tar bort och ersätter ett chassi i en enhet med StorSimple 8000-serien.</span><span class="sxs-lookup"><span data-stu-id="355c7-105">This tutorial explains how to remove and replace a chassis in a StorSimple 8000 series device.</span></span> <span data-ttu-id="355c7-106">StorSimple 8100 modellen är en enda enhet enhet (samma chassi), medan 8600 är en dubbel höljet enhet (två chassi).</span><span class="sxs-lookup"><span data-stu-id="355c7-106">The StorSimple 8100 model is a single enclosure device (one chassis), whereas the 8600 is a dual enclosure device (two chassis).</span></span> <span data-ttu-id="355c7-107">För en modell av 8600 är potentiellt två chassi som kan ha i enheten: chassi för primära höljet eller chassi för EBOD höljet.</span><span class="sxs-lookup"><span data-stu-id="355c7-107">For an 8600 model, there are potentially two chassis that could fail in the device: the chassis for the primary enclosure or the chassis for the EBOD enclosure.</span></span>

<span data-ttu-id="355c7-108">I båda fallen är ersättning chassi som levereras av Microsoft tom.</span><span class="sxs-lookup"><span data-stu-id="355c7-108">In either case, the replacement chassis that is shipped by Microsoft is empty.</span></span> <span data-ttu-id="355c7-109">Ingen ström och kylning moduler (PCMs) domänkontrollant moduler solid state disk-hårddiskar (SSD), hårddiskar (HDD) eller EBOD moduler tas med.</span><span class="sxs-lookup"><span data-stu-id="355c7-109">No Power and Cooling Modules (PCMs), controller modules, solid state disk drives (SSDs), hard disk drives (HDDs), or EBOD modules will be included.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="355c7-110">Innan du tar bort och ersätter chassit, granska säkerhetsinformation i [ersättning av StorSimple maskinvara komponenten](storsimple-8000-hardware-component-replacement.md).</span><span class="sxs-lookup"><span data-stu-id="355c7-110">Before removing and replacing the chassis, review the safety information in [StorSimple hardware component replacement](storsimple-8000-hardware-component-replacement.md).</span></span>


## <a name="remove-the-chassis"></a><span data-ttu-id="355c7-111">Ta bort chassit</span><span class="sxs-lookup"><span data-stu-id="355c7-111">Remove the chassis</span></span>
<span data-ttu-id="355c7-112">Utför följande steg för att ta bort chassi på StorSimple-enheten.</span><span class="sxs-lookup"><span data-stu-id="355c7-112">Perform the following steps to remove the chassis on your StorSimple device.</span></span>

#### <a name="to-remove-a-chassis"></a><span data-ttu-id="355c7-113">Ta bort ett chassi</span><span class="sxs-lookup"><span data-stu-id="355c7-113">To remove a chassis</span></span>
1. <span data-ttu-id="355c7-114">Kontrollera att StorSimple-enheten stängs av och kopplas bort från alla strömkällor.</span><span class="sxs-lookup"><span data-stu-id="355c7-114">Make sure that the StorSimple device is shut down and disconnected from all the power sources.</span></span>
2. <span data-ttu-id="355c7-115">Ta bort alla nätverks- och SAS-kablar, om tillämpligt.</span><span class="sxs-lookup"><span data-stu-id="355c7-115">Remove all the network and SAS cables, if applicable.</span></span>
3. <span data-ttu-id="355c7-116">Ta bort enheten från racket.</span><span class="sxs-lookup"><span data-stu-id="355c7-116">Remove the unit from the rack.</span></span>
4. <span data-ttu-id="355c7-117">Ta bort var och en av enheterna och Lägg märke till de platser där de tas bort.</span><span class="sxs-lookup"><span data-stu-id="355c7-117">Remove each of the drives and note the slots from which they are removed.</span></span> <span data-ttu-id="355c7-118">Mer information finns i [ta bort diskenheten](storsimple-8000-disk-drive-replacement.md#remove-the-disk-drive).</span><span class="sxs-lookup"><span data-stu-id="355c7-118">For more information, see [Remove the disk drive](storsimple-8000-disk-drive-replacement.md#remove-the-disk-drive).</span></span>
5. <span data-ttu-id="355c7-119">På EBOD höljet (om detta är chassi som misslyckades), ta bort EBOD domänkontrollant moduler.</span><span class="sxs-lookup"><span data-stu-id="355c7-119">On the EBOD enclosure (if this is the chassis that failed), remove the EBOD controller modules.</span></span> <span data-ttu-id="355c7-120">Mer information finns i [ta bort en domänkontrollant för en EBOD](storsimple-8000-ebod-controller-replacement.md#remove-an-ebod-controller).</span><span class="sxs-lookup"><span data-stu-id="355c7-120">For more information, see [Remove an EBOD controller](storsimple-8000-ebod-controller-replacement.md#remove-an-ebod-controller).</span></span>
   
    <span data-ttu-id="355c7-121">På primära höljet (om detta är chassi som misslyckades), ta bort domänkontrollanterna och Lägg märke till de platser där de tas bort.</span><span class="sxs-lookup"><span data-stu-id="355c7-121">On the primary enclosure (if this is the chassis that failed), remove the controllers and note the slots from which they are removed.</span></span> <span data-ttu-id="355c7-122">Mer information finns i [ta bort en domänkontrollant](storsimple-8000-controller-replacement.md#remove-a-controller).</span><span class="sxs-lookup"><span data-stu-id="355c7-122">For more information, see [Remove a controller](storsimple-8000-controller-replacement.md#remove-a-controller).</span></span>

## <a name="install-the-chassis"></a><span data-ttu-id="355c7-123">Installera chassit</span><span class="sxs-lookup"><span data-stu-id="355c7-123">Install the chassis</span></span>
<span data-ttu-id="355c7-124">Utför följande steg för att installera chassit på din StorSimple-enhet.</span><span class="sxs-lookup"><span data-stu-id="355c7-124">Perform the following steps to install the chassis on your StorSimple device.</span></span>

#### <a name="to-install-a-chassis"></a><span data-ttu-id="355c7-125">Så här installerar du ett chassi</span><span class="sxs-lookup"><span data-stu-id="355c7-125">To install a chassis</span></span>
1. <span data-ttu-id="355c7-126">Montera chassi i racket.</span><span class="sxs-lookup"><span data-stu-id="355c7-126">Mount the chassis in the rack.</span></span> <span data-ttu-id="355c7-127">Mer information finns i [rackmontera StorSimple-8100-enhet](storsimple-8100-hardware-installation.md#rack-mount-your-storsimple-8100-device) eller [rackmonterade StorSimple-8600-enhet](storsimple-8600-hardware-installation.md#rack-mount-your-storsimple-8600-device).</span><span class="sxs-lookup"><span data-stu-id="355c7-127">For more information, see [Rack-mount your StorSimple 8100 device](storsimple-8100-hardware-installation.md#rack-mount-your-storsimple-8100-device) or [Rack-mount your StorSimple 8600 device](storsimple-8600-hardware-installation.md#rack-mount-your-storsimple-8600-device).</span></span>
2. <span data-ttu-id="355c7-128">När chassit har monterats i racket, installerar du modulerna som domänkontrollant på samma platser som de tidigare har installerats i.</span><span class="sxs-lookup"><span data-stu-id="355c7-128">After the chassis is mounted in the rack, install the controller modules in the same positions that they were previously installed in.</span></span>
3. <span data-ttu-id="355c7-129">Installera enheterna i samma platser och distributionsplatser som de tidigare har installerats i.</span><span class="sxs-lookup"><span data-stu-id="355c7-129">Install the drives in the same positions and slots that they were previously installed in.</span></span>
   
   > [!NOTE]
   > <span data-ttu-id="355c7-130">Vi rekommenderar att du först installera SSD-enheterna i platserna och sedan installera hårddiskar.</span><span class="sxs-lookup"><span data-stu-id="355c7-130">We recommend that you install the SSDs in the slots first, and then install the HDDs.</span></span>
  
4. <span data-ttu-id="355c7-131">När enheten är monterat i racket och de komponenter som installeras kan ansluta enheten till lämplig strömkällor och slå på enheten.</span><span class="sxs-lookup"><span data-stu-id="355c7-131">With the device mounted in the rack and the components installed, connect your device to the appropriate power sources, and turn on the device.</span></span> <span data-ttu-id="355c7-132">Mer information finns i [Kabelanslut din 8100 StorSimple-enhet](storsimple-8100-hardware-installation.md#cable-your-storsimple-8100-device) eller [Kabelanslut din 8600 StorSimple-enhet](storsimple-8600-hardware-installation.md#cable-your-storsimple-8600-device).</span><span class="sxs-lookup"><span data-stu-id="355c7-132">For details, see [Cable your StorSimple 8100 device](storsimple-8100-hardware-installation.md#cable-your-storsimple-8100-device) or [Cable your StorSimple 8600 device](storsimple-8600-hardware-installation.md#cable-your-storsimple-8600-device).</span></span>

## <a name="next-steps"></a><span data-ttu-id="355c7-133">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="355c7-133">Next steps</span></span>
<span data-ttu-id="355c7-134">Lär dig mer om [ersättning av StorSimple maskinvara komponenten](storsimple-8000-hardware-component-replacement.md).</span><span class="sxs-lookup"><span data-stu-id="355c7-134">Learn more about [StorSimple hardware component replacement](storsimple-8000-hardware-component-replacement.md).</span></span>

