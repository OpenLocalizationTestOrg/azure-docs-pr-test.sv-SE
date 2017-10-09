---
title: "aaaReplace chassi på StorSimple 8000-serieenhet | Microsoft Docs"
description: "Beskriver hur tooremove och Ersätt hello chassi för din StorSimple primära hölje eller EBOD hölje."
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
ms.openlocfilehash: 94bbd3d354a9b8866ece036238927e67ec5ce2a9
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="replace-hello-chassis-on-your-storsimple-device"></a><span data-ttu-id="73ce3-103">Ersätt hello chassi på din StorSimple-enhet</span><span class="sxs-lookup"><span data-stu-id="73ce3-103">Replace hello chassis on your StorSimple device</span></span>
## <a name="overview"></a><span data-ttu-id="73ce3-104">Översikt</span><span class="sxs-lookup"><span data-stu-id="73ce3-104">Overview</span></span>
<span data-ttu-id="73ce3-105">Den här självstudiekursen beskrivs hur tooremove och ersätta ett chassi i en enhet med StorSimple 8000-serien.</span><span class="sxs-lookup"><span data-stu-id="73ce3-105">This tutorial explains how tooremove and replace a chassis in a StorSimple 8000 series device.</span></span> <span data-ttu-id="73ce3-106">Hej StorSimple 8100 modellen är en enda enhet enhet (samma chassi) hello 8600 är en dubbel höljet enhet (två chassi).</span><span class="sxs-lookup"><span data-stu-id="73ce3-106">hello StorSimple 8100 model is a single enclosure device (one chassis), whereas hello 8600 is a dual enclosure device (two chassis).</span></span> <span data-ttu-id="73ce3-107">För en modell av 8600 är potentiellt två chassi som kan misslyckas på hello enhet: hello chassi för hello primära hölje eller hello chassi för hello EBOD hölje.</span><span class="sxs-lookup"><span data-stu-id="73ce3-107">For an 8600 model, there are potentially two chassis that could fail in hello device: hello chassis for hello primary enclosure or hello chassis for hello EBOD enclosure.</span></span>

<span data-ttu-id="73ce3-108">I båda fallen är hello ersättning chassi som levereras av Microsoft tom.</span><span class="sxs-lookup"><span data-stu-id="73ce3-108">In either case, hello replacement chassis that is shipped by Microsoft is empty.</span></span> <span data-ttu-id="73ce3-109">Ingen ström och kylning moduler (PCMs) domänkontrollant moduler solid state disk-hårddiskar (SSD), hårddiskar (HDD) eller EBOD moduler tas med.</span><span class="sxs-lookup"><span data-stu-id="73ce3-109">No Power and Cooling Modules (PCMs), controller modules, solid state disk drives (SSDs), hard disk drives (HDDs), or EBOD modules will be included.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="73ce3-110">Innan du tar bort och ersätter chassi hello, granska hello säkerhetsinformation i [ersättning av StorSimple maskinvara komponenten](storsimple-8000-hardware-component-replacement.md).</span><span class="sxs-lookup"><span data-stu-id="73ce3-110">Before removing and replacing hello chassis, review hello safety information in [StorSimple hardware component replacement](storsimple-8000-hardware-component-replacement.md).</span></span>


## <a name="remove-hello-chassis"></a><span data-ttu-id="73ce3-111">Ta bort hello chassi</span><span class="sxs-lookup"><span data-stu-id="73ce3-111">Remove hello chassis</span></span>
<span data-ttu-id="73ce3-112">Utföra hello följa steg tooremove hello chassi på din StorSimple-enhet.</span><span class="sxs-lookup"><span data-stu-id="73ce3-112">Perform hello following steps tooremove hello chassis on your StorSimple device.</span></span>

#### <a name="tooremove-a-chassis"></a><span data-ttu-id="73ce3-113">tooremove ett chassi</span><span class="sxs-lookup"><span data-stu-id="73ce3-113">tooremove a chassis</span></span>
1. <span data-ttu-id="73ce3-114">Kontrollera att hello StorSimple-enheten stängs av och kopplas bort från alla hello strömkällor.</span><span class="sxs-lookup"><span data-stu-id="73ce3-114">Make sure that hello StorSimple device is shut down and disconnected from all hello power sources.</span></span>
2. <span data-ttu-id="73ce3-115">Ta bort alla hello nätverks- och SAS-kablar, om tillämpligt.</span><span class="sxs-lookup"><span data-stu-id="73ce3-115">Remove all hello network and SAS cables, if applicable.</span></span>
3. <span data-ttu-id="73ce3-116">Ta bort hello enheten från hello rack.</span><span class="sxs-lookup"><span data-stu-id="73ce3-116">Remove hello unit from hello rack.</span></span>
4. <span data-ttu-id="73ce3-117">Ta bort alla hello enheter och Observera hello fack de tas bort.</span><span class="sxs-lookup"><span data-stu-id="73ce3-117">Remove each of hello drives and note hello slots from which they are removed.</span></span> <span data-ttu-id="73ce3-118">Mer information finns i [ta bort hello diskenhet](storsimple-8000-disk-drive-replacement.md#remove-the-disk-drive).</span><span class="sxs-lookup"><span data-stu-id="73ce3-118">For more information, see [Remove hello disk drive](storsimple-8000-disk-drive-replacement.md#remove-the-disk-drive).</span></span>
5. <span data-ttu-id="73ce3-119">Ta bort hello EBOD domänkontrollant moduler på hello EBOD hölje (om detta är hello chassi som misslyckades).</span><span class="sxs-lookup"><span data-stu-id="73ce3-119">On hello EBOD enclosure (if this is hello chassis that failed), remove hello EBOD controller modules.</span></span> <span data-ttu-id="73ce3-120">Mer information finns i [ta bort en domänkontrollant för en EBOD](storsimple-8000-ebod-controller-replacement.md#remove-an-ebod-controller).</span><span class="sxs-lookup"><span data-stu-id="73ce3-120">For more information, see [Remove an EBOD controller](storsimple-8000-ebod-controller-replacement.md#remove-an-ebod-controller).</span></span>
   
    <span data-ttu-id="73ce3-121">På hello primära hölje (om detta är hello chassi som misslyckades) ta bort hello-styrenheter och notera hello fack de tas bort.</span><span class="sxs-lookup"><span data-stu-id="73ce3-121">On hello primary enclosure (if this is hello chassis that failed), remove hello controllers and note hello slots from which they are removed.</span></span> <span data-ttu-id="73ce3-122">Mer information finns i [ta bort en domänkontrollant](storsimple-8000-controller-replacement.md#remove-a-controller).</span><span class="sxs-lookup"><span data-stu-id="73ce3-122">For more information, see [Remove a controller](storsimple-8000-controller-replacement.md#remove-a-controller).</span></span>

## <a name="install-hello-chassis"></a><span data-ttu-id="73ce3-123">Installera hello chassi</span><span class="sxs-lookup"><span data-stu-id="73ce3-123">Install hello chassis</span></span>
<span data-ttu-id="73ce3-124">Utföra hello följa steg tooinstall hello chassi på din StorSimple-enhet.</span><span class="sxs-lookup"><span data-stu-id="73ce3-124">Perform hello following steps tooinstall hello chassis on your StorSimple device.</span></span>

#### <a name="tooinstall-a-chassis"></a><span data-ttu-id="73ce3-125">tooinstall ett chassi</span><span class="sxs-lookup"><span data-stu-id="73ce3-125">tooinstall a chassis</span></span>
1. <span data-ttu-id="73ce3-126">Montera hello chassi i hello rack.</span><span class="sxs-lookup"><span data-stu-id="73ce3-126">Mount hello chassis in hello rack.</span></span> <span data-ttu-id="73ce3-127">Mer information finns i [rackmontera StorSimple-8100-enhet](storsimple-8100-hardware-installation.md#rack-mount-your-storsimple-8100-device) eller [rackmonterade StorSimple-8600-enhet](storsimple-8600-hardware-installation.md#rack-mount-your-storsimple-8600-device).</span><span class="sxs-lookup"><span data-stu-id="73ce3-127">For more information, see [Rack-mount your StorSimple 8100 device](storsimple-8100-hardware-installation.md#rack-mount-your-storsimple-8100-device) or [Rack-mount your StorSimple 8600 device](storsimple-8600-hardware-installation.md#rack-mount-your-storsimple-8600-device).</span></span>
2. <span data-ttu-id="73ce3-128">När hello chassi har monterats i hello rack, installerar du hello controller moduler i hello samma placerar att de tidigare har installerats i.</span><span class="sxs-lookup"><span data-stu-id="73ce3-128">After hello chassis is mounted in hello rack, install hello controller modules in hello same positions that they were previously installed in.</span></span>
3. <span data-ttu-id="73ce3-129">Installera hello enheter i hello samma placerar och fack som de installerades tidigare i.</span><span class="sxs-lookup"><span data-stu-id="73ce3-129">Install hello drives in hello same positions and slots that they were previously installed in.</span></span>
   
   > [!NOTE]
   > <span data-ttu-id="73ce3-130">Vi rekommenderar att du först installera hello SSD i hello platserna och sedan installera hello hårddiskar.</span><span class="sxs-lookup"><span data-stu-id="73ce3-130">We recommend that you install hello SSDs in hello slots first, and then install hello HDDs.</span></span>
  
4. <span data-ttu-id="73ce3-131">Med hello enheten monteras i hello rack och hello-komponenter, ansluta din enhet toohello lämplig strömkällor och aktivera hello enheten.</span><span class="sxs-lookup"><span data-stu-id="73ce3-131">With hello device mounted in hello rack and hello components installed, connect your device toohello appropriate power sources, and turn on hello device.</span></span> <span data-ttu-id="73ce3-132">Mer information finns i [Kabelanslut din 8100 StorSimple-enhet](storsimple-8100-hardware-installation.md#cable-your-storsimple-8100-device) eller [Kabelanslut din 8600 StorSimple-enhet](storsimple-8600-hardware-installation.md#cable-your-storsimple-8600-device).</span><span class="sxs-lookup"><span data-stu-id="73ce3-132">For details, see [Cable your StorSimple 8100 device](storsimple-8100-hardware-installation.md#cable-your-storsimple-8100-device) or [Cable your StorSimple 8600 device](storsimple-8600-hardware-installation.md#cable-your-storsimple-8600-device).</span></span>

## <a name="next-steps"></a><span data-ttu-id="73ce3-133">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="73ce3-133">Next steps</span></span>
<span data-ttu-id="73ce3-134">Lär dig mer om [ersättning av StorSimple maskinvara komponenten](storsimple-8000-hardware-component-replacement.md).</span><span class="sxs-lookup"><span data-stu-id="73ce3-134">Learn more about [StorSimple hardware component replacement](storsimple-8000-hardware-component-replacement.md).</span></span>

