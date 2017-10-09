---
title: aaaUse StorSimple 8000-serien sammanfattningen | Microsoft Docs
description: "Beskriver hello StorSimple Enhetshanteraren service sammanfattningen och hur toouse den tooview storage-mätvärden och anslutna initierare och hitta hello serienummer och IQN."
services: storsimple
documentationcenter: NA
author: alkohli
manager: timlt
editor: 
ms.assetid: 
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 07/03/2017
ms.author: alkohli
ms.openlocfilehash: b45ffc6ec52ebb6549c25a00c68c62460b208b7a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="use-hello-device-summary-in-storsimple-device-manager-service"></a><span data-ttu-id="6a7fd-103">Använd hello sammanfattningen i Enhetshanteraren för StorSimple-tjänsten</span><span class="sxs-lookup"><span data-stu-id="6a7fd-103">Use hello device summary in StorSimple Device Manager service</span></span>

## <a name="overview"></a><span data-ttu-id="6a7fd-104">Översikt</span><span class="sxs-lookup"><span data-stu-id="6a7fd-104">Overview</span></span>
<span data-ttu-id="6a7fd-105">Översikt över bladet för hello StorSimple-enhet ger en översikt över information för en specifik StorSimple-enhet, däremot toohello tjänsten sammanfattning bladet som ger information om alla hello-enheter som ingår i Microsoft Azure StorSimple-lösningen.</span><span class="sxs-lookup"><span data-stu-id="6a7fd-105">hello StorSimple device summary blade gives you an overview of information for a specific StorSimple device, in contrast toohello service summary blade, which gives you information about all hello devices included in your Microsoft Azure StorSimple solution.</span></span>

<span data-ttu-id="6a7fd-106">hello enheten sammanfattning bladet innehåller en översikt över av en StorSimple 8000-serieenhet som har registrerats med en viss StorSimple Enhetshanteraren, syntaxmarkering dessa problem med enheter som behöver åtgärdas av en systemadministratör.</span><span class="sxs-lookup"><span data-stu-id="6a7fd-106">hello device summary blade provides a summary view of a StorSimple 8000 series device that is registered with a given StorSimple Device Manager, highlighting those device issues that need a system administrator's attention.</span></span> <span data-ttu-id="6a7fd-107">Den här kursen introducerar hello enheten sammanfattning-bladet, förklarar hello innehåll och funktionen och beskriver hello uppgifter som du kan utföra från det här bladet.</span><span class="sxs-lookup"><span data-stu-id="6a7fd-107">This tutorial introduces hello device summary blade, explains hello content and function, and describes hello tasks that you can perform from this blade.</span></span>

<span data-ttu-id="6a7fd-108">hello enheten sammanfattning bladet visar hello följande information:</span><span class="sxs-lookup"><span data-stu-id="6a7fd-108">hello device summary blade displays hello following information:</span></span>

![Översikt över bladet för enhet](./media/storsimple-8000-device-dashboard/device-summary1.png)

## <a name="management-command-bar"></a><span data-ttu-id="6a7fd-110">Hantering av kommandofältet</span><span class="sxs-lookup"><span data-stu-id="6a7fd-110">Management command bar</span></span>

<span data-ttu-id="6a7fd-111">I bladet för hello StorSimple-enhet ser du hello alternativ för att hantera din StorSimple-enhet.</span><span class="sxs-lookup"><span data-stu-id="6a7fd-111">In hello StorSimple device blade, you see hello options for managing your StorSimple device.</span></span> <span data-ttu-id="6a7fd-112">Visas hello management kommandon överst hello av hello-bladet och hello vänster.</span><span class="sxs-lookup"><span data-stu-id="6a7fd-112">You see hello management commands across hello top of hello blade and on hello left side.</span></span> <span data-ttu-id="6a7fd-113">Använder dessa alternativ tooadd resurser eller volymer, uppdatera eller växla över din enhet.</span><span class="sxs-lookup"><span data-stu-id="6a7fd-113">Use these options tooadd shares or volumes, or update or fail over your device.</span></span>

![Hantering av kommandofältet](./media/storsimple-8000-device-dashboard/device-summary2.png)

## <a name="essentials"></a><span data-ttu-id="6a7fd-115">Essentials</span><span class="sxs-lookup"><span data-stu-id="6a7fd-115">Essentials</span></span>

<span data-ttu-id="6a7fd-116">hello essentials området avbildar hello viktiga egenskaper som, hello status, modell, mål IQN och hello programvaruversionen.</span><span class="sxs-lookup"><span data-stu-id="6a7fd-116">hello essentials area captures some of hello important properties such as, hello status, model, target IQN, and hello software version.</span></span> 

![Enheten essentials](./media/storsimple-8000-device-dashboard/device-summary3.png)

## <a name="monitoring"></a><span data-ttu-id="6a7fd-118">Övervakning</span><span class="sxs-lookup"><span data-stu-id="6a7fd-118">Monitoring</span></span>

* <span data-ttu-id="6a7fd-119">Hej **aviseringar** panelen ger en ögonblicksbild av alla hello aktiva aviseringar för enheten, grupperat efter allvarlighetsgrad.</span><span class="sxs-lookup"><span data-stu-id="6a7fd-119">hello **Alerts** tile provides a snapshot of all hello active alerts for your device, grouped by alert severity.</span></span>

    ![Aviseringen sida vid sida](./media/storsimple-8000-device-dashboard/device-summary4.png)

    <span data-ttu-id="6a7fd-121">Klicka på hello panelen tooopen hello **aviseringar** bladet och klicka sedan på en enskild varning tooview ytterligare information om den här aviseringen, inklusive de rekommenderade åtgärder.</span><span class="sxs-lookup"><span data-stu-id="6a7fd-121">Click hello tile tooopen hello **Alerts** blade and then click an individual alert tooview additional details about that alert, including any recommended actions.</span></span> <span data-ttu-id="6a7fd-122">Du kan även radera hello avisering om hello problemet har lösts.</span><span class="sxs-lookup"><span data-stu-id="6a7fd-122">You can also clear hello alert if hello issue has been resolved.</span></span>

    ![Klicka på aviseringen sida vid sida](./media/storsimple-8000-device-dashboard/device-summary10.png)

* <span data-ttu-id="6a7fd-124">Hej **statusen och hälsan** panelen ger insikter om hello maskinvara komponentens hälsostatus för en enhet, inklusive hello Enhetsstatus.</span><span class="sxs-lookup"><span data-stu-id="6a7fd-124">hello **Status and health** tile provides insights into hello hardware component health for a device including hello device status.</span></span> <span data-ttu-id="6a7fd-125">hello Enhetsstatus kan vara offline, online, inaktiverat eller redo tooset upp.</span><span class="sxs-lookup"><span data-stu-id="6a7fd-125">hello device status may be offline, online, deactivated, or ready tooset up.</span></span>

    ![Statusen och hälsan panel](./media/storsimple-8000-device-dashboard/device-summary5.png)

* <span data-ttu-id="6a7fd-127">Hej **volymer** panelen innehåller en översikt över hello antalet volymer i enheten grupperade efter status.</span><span class="sxs-lookup"><span data-stu-id="6a7fd-127">hello **Volumes** tile provides a summary of hello number of volumes in your device grouped by status.</span></span>

    ![Volymbrickan](./media/storsimple-8000-device-dashboard/device-summary6.png)

    <span data-ttu-id="6a7fd-129">Klicka på hello panelen tooopen hello **volymer** bladet och sedan klicka på en enskild volym tooview eller ändra dess egenskaper.</span><span class="sxs-lookup"><span data-stu-id="6a7fd-129">Click hello tile tooopen hello **Volumes** list blade, and then click on an individual volume tooview or modify its properties.</span></span>
    
    ![Klicka på panelen volymer](./media/storsimple-8000-device-dashboard/device-summary9.png)
    
    <span data-ttu-id="6a7fd-131">Mer information finns i hur för[hantera volymer](storsimple-8000-manage-volumes-u2.md).</span><span class="sxs-lookup"><span data-stu-id="6a7fd-131">For more information, see how too[manage volumes](storsimple-8000-manage-volumes-u2.md).</span></span>

* <span data-ttu-id="6a7fd-132">I hello **användning** diagram, kan du visa hello primära lagringsutrymme som används i hela enheten och hello molnlagring som används över hello senaste 7 dagarna, hello standard tidsperiod.</span><span class="sxs-lookup"><span data-stu-id="6a7fd-132">In hello **Usage** chart, you can view hello primary storage used across your device, and hello cloud storage consumed over hello past 7 days, hello default time period.</span></span>

     ![Ikonen användning](./media/storsimple-8000-device-dashboard/device-summary7.png)
    
     <span data-ttu-id="6a7fd-134">toochoose en annan tidsskala använda hello **redigera** alternativ i hello övre högra hörnet av hello diagram.</span><span class="sxs-lookup"><span data-stu-id="6a7fd-134">toochoose a different time scale, use hello **Edit** option in hello top-right corner of hello chart.</span></span>

     ![Redigera Användningsdiagram](./media/storsimple-8000-device-dashboard/device-summary12.png)

     <span data-ttu-id="6a7fd-136">I det här diagrammet kan du visa måtten för hello totala primära lagringsutrymmet (hello mängden data som skrivs av värdar tooyour enhet) och hello totala molnlagring som används av din enhet under en viss tidsperiod.</span><span class="sxs-lookup"><span data-stu-id="6a7fd-136">In this chart, you can view metrics for hello total primary storage (hello amount of data written by hosts tooyour device) and hello total cloud storage consumed by your device over a period of time.</span></span>
  
     <span data-ttu-id="6a7fd-137">I den här kontexten *primära lagringsplatsen* refererar toohello totala mängden data som skrivs av hello-värden och kan delas upp på volymtyp: *primära nivåer lagring* innehåller både lokalt lagrade data och data nivåindelad toohello moln.</span><span class="sxs-lookup"><span data-stu-id="6a7fd-137">In this context, *primary storage* refers toohello total amount of data written by hello host, and can be broken down by volume type: *primary tiered storage* includes both locally stored data and data tiered toohello cloud.</span></span> <span data-ttu-id="6a7fd-138">*Primär lokalt Fäst lagring* innehåller bara data som lagras lokalt.</span><span class="sxs-lookup"><span data-stu-id="6a7fd-138">*Primary locally pinned storage* includes just data stored locally.</span></span> <span data-ttu-id="6a7fd-139">*Molnlagring*, på hello andra sidan, är ett mått på hello totala mängden data som lagras i hello molnet.</span><span class="sxs-lookup"><span data-stu-id="6a7fd-139">*Cloud storage*, on hello other hand, is a measurement of hello total amount of data stored in hello cloud.</span></span> <span data-ttu-id="6a7fd-140">Den här innehåller nivåindelade data och säkerhetskopieringar.</span><span class="sxs-lookup"><span data-stu-id="6a7fd-140">This storage includes tiered data and backups.</span></span> <span data-ttu-id="6a7fd-141">hello-data som lagras i molnet hello är deduplicerad och komprimerade, medan primära lagringsplatsen anger hello mängden lagringsutrymme som används innan hello data är deduplicerad och komprimeras.</span><span class="sxs-lookup"><span data-stu-id="6a7fd-141">hello data stored in hello cloud is deduplicated and compressed, whereas primary storage indicates hello amount of storage used before hello data is deduplicated and compressed.</span></span> <span data-ttu-id="6a7fd-142">(Du kan jämföra dessa två tal tooget en uppfattning om hello komprimeringshastighet.) För både primär och molnlagring hello belopp som visas baseras på hello spårning frekvens som du konfigurerar.</span><span class="sxs-lookup"><span data-stu-id="6a7fd-142">(You can compare these two numbers tooget an idea of hello compression rate.) For both primary and cloud storage, hello amounts shown are based on hello tracking frequency you configure.</span></span> <span data-ttu-id="6a7fd-143">Till exempel om du väljer en frekvens som en vecka sedan hello diagrammet visar data för varje dag i hello föregående vecka.</span><span class="sxs-lookup"><span data-stu-id="6a7fd-143">For example, if you choose a one week frequency, then hello chart shows data for each day in hello previous week.</span></span>

     <span data-ttu-id="6a7fd-144">toosee hello mängden molnlagring som används över tid, väljer hello **MOLN lagring används** alternativet.</span><span class="sxs-lookup"><span data-stu-id="6a7fd-144">toosee hello amount of cloud storage consumed over time, select hello **CLOUD STORAGE USED** option.</span></span> <span data-ttu-id="6a7fd-145">toosee hello totalt lagringsutrymme som har skrivits av hello-värden, Välj hello **primära nivåer lagring används** och **primära LOKALT FÄST lagring används** alternativ.</span><span class="sxs-lookup"><span data-stu-id="6a7fd-145">toosee hello total storage that has been written by hello host, select hello **PRIMARY TIERED STORAGE USED** and **PRIMARY LOCALLY PINNED STORAGE USED** options.</span></span> 
     <span data-ttu-id="6a7fd-146">Mer information finns i [Använd hello StorSimple Enhetshanteraren service toomonitor StorSimple-enheten](storsimple-monitor-device.md).</span><span class="sxs-lookup"><span data-stu-id="6a7fd-146">For more information, see [Use hello StorSimple Device Manager service toomonitor your StorSimple device](storsimple-monitor-device.md).</span></span>


* <span data-ttu-id="6a7fd-147">Hej **kapacitet** panelen visar hello primära lagring som har etablerats och återstående över hello enheten relativa toohello totala lagringsstorleken för hello samma.</span><span class="sxs-lookup"><span data-stu-id="6a7fd-147">hello **Capacity** tile displays hello primary storage that is provisioned and remaining across hello device relative toohello total storage available for hello same.</span></span> <span data-ttu-id="6a7fd-148">**Etablerad** refererar toohello mängden lagringsutrymme som är förberedd och tilldelat för användning, **återstående** refererar toohello återstående kapacitet som kan tillhandahållas i den här enheten.</span><span class="sxs-lookup"><span data-stu-id="6a7fd-148">**Provisioned** refers toohello amount of storage that is prepared and allocated for use, **Remaining** refers toohello remaining capacity that can be provisioned across this device.</span></span> 

    ![Ikonen användning](./media/storsimple-8000-device-dashboard/device-summary8.png)

    <span data-ttu-id="6a7fd-150">Klicka på den här panelen tooview hur hello kapacitet etableras mellan nivåindelade och lokalt fästa volymer.</span><span class="sxs-lookup"><span data-stu-id="6a7fd-150">Click this tile tooview how hello capacity is provisioned across tiered and locally pinned volumes.</span></span> <span data-ttu-id="6a7fd-151">Hej **återstående nivåer** kapaciteten är hello tillgänglig kapacitet som kan etableras inklusive molnet, medan hello **återstående lokala** är hello kapacitet kvar på hello diskar ansluten toothis enhet.</span><span class="sxs-lookup"><span data-stu-id="6a7fd-151">hello **Remaining Tiered** capacity is hello available capacity that can be provisioned including cloud, while hello **Remaining Local** is hello capacity remaining on hello disks attached toothis device.</span></span>

    ![Klicka på Användningsdiagram](./media/storsimple-8000-device-dashboard/device-summary13.png)


## <a name="next-steps"></a><span data-ttu-id="6a7fd-153">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="6a7fd-153">Next steps</span></span>
* <span data-ttu-id="6a7fd-154">Mer information om hello [StorSimple-tjänsten sammanfattning bladet](storsimple-8000-service-dashboard.md).</span><span class="sxs-lookup"><span data-stu-id="6a7fd-154">Learn more about hello [StorSimple service summary blade](storsimple-8000-service-dashboard.md).</span></span>
* <span data-ttu-id="6a7fd-155">Lär dig mer om [med hello StorSimple Enhetshanteraren service tooadminister StorSimple-enheten](storsimple-8000-manager-service-administration.md).</span><span class="sxs-lookup"><span data-stu-id="6a7fd-155">Learn more about [using hello StorSimple Device Manager service tooadminister your StorSimple device](storsimple-8000-manager-service-administration.md).</span></span>

