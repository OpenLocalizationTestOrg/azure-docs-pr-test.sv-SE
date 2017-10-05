---
title: "Använd StorSimple 8000-serien sammanfattningen | Microsoft Docs"
description: "Beskriver sammanfattning StorSimple Enhetshanteraren service enheten och hur du använder den för att visa storage-mätvärden och anslutna initierare och hitta serienummer och IQN."
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
ms.openlocfilehash: 784d3ce9d8f926b00ac1c6fbf48a05c0b04f900a
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="use-the-device-summary-in-storsimple-device-manager-service"></a><span data-ttu-id="4f43a-103">Använda sammanfattningen i Enhetshanteraren för StorSimple-tjänsten</span><span class="sxs-lookup"><span data-stu-id="4f43a-103">Use the device summary in StorSimple Device Manager service</span></span>

## <a name="overview"></a><span data-ttu-id="4f43a-104">Översikt</span><span class="sxs-lookup"><span data-stu-id="4f43a-104">Overview</span></span>
<span data-ttu-id="4f43a-105">Bladet StorSimple enheten sammanfattning ger dig en översikt över information för en specifik StorSimple-enhet, till skillnad från tjänsten sammanfattning bladet som ger information om de enheter som ingår i Microsoft Azure StorSimple-lösningen.</span><span class="sxs-lookup"><span data-stu-id="4f43a-105">The StorSimple device summary blade gives you an overview of information for a specific StorSimple device, in contrast to the service summary blade, which gives you information about all the devices included in your Microsoft Azure StorSimple solution.</span></span>

<span data-ttu-id="4f43a-106">Enheten sammanfattning bladet innehåller en översikt över av en StorSimple 8000-serieenhet som har registrerats med en viss StorSimple Enhetshanteraren, syntaxmarkering dessa problem med enheter som behöver åtgärdas av en systemadministratör.</span><span class="sxs-lookup"><span data-stu-id="4f43a-106">The device summary blade provides a summary view of a StorSimple 8000 series device that is registered with a given StorSimple Device Manager, highlighting those device issues that need a system administrator's attention.</span></span> <span data-ttu-id="4f43a-107">Den här kursen introducerar bladet enheten sammanfattning, förklarar innehåll och funktionen och beskrivs vilka aktiviteter som du kan utföra från det här bladet.</span><span class="sxs-lookup"><span data-stu-id="4f43a-107">This tutorial introduces the device summary blade, explains the content and function, and describes the tasks that you can perform from this blade.</span></span>

<span data-ttu-id="4f43a-108">Sammanfattning bladet enheten visar följande information:</span><span class="sxs-lookup"><span data-stu-id="4f43a-108">The device summary blade displays the following information:</span></span>

![Översikt över bladet för enhet](./media/storsimple-8000-device-dashboard/device-summary1.png)

## <a name="management-command-bar"></a><span data-ttu-id="4f43a-110">Hantering av kommandofältet</span><span class="sxs-lookup"><span data-stu-id="4f43a-110">Management command bar</span></span>

<span data-ttu-id="4f43a-111">I bladet StorSimple-enhet visas alternativ för att hantera din StorSimple-enhet.</span><span class="sxs-lookup"><span data-stu-id="4f43a-111">In the StorSimple device blade, you see the options for managing your StorSimple device.</span></span> <span data-ttu-id="4f43a-112">Du kan se kommandona management överst på bladet och till vänster.</span><span class="sxs-lookup"><span data-stu-id="4f43a-112">You see the management commands across the top of the blade and on the left side.</span></span> <span data-ttu-id="4f43a-113">Använd dessa alternativ för att lägga till resurser eller volymer, eller uppdatera eller växla över din enhet.</span><span class="sxs-lookup"><span data-stu-id="4f43a-113">Use these options to add shares or volumes, or update or fail over your device.</span></span>

![Hantering av kommandofältet](./media/storsimple-8000-device-dashboard/device-summary2.png)

## <a name="essentials"></a><span data-ttu-id="4f43a-115">Essentials</span><span class="sxs-lookup"><span data-stu-id="4f43a-115">Essentials</span></span>

<span data-ttu-id="4f43a-116">Området essentials avbildar vissa viktiga egenskaper som, status, modell, mål IQN och programvaruversionen.</span><span class="sxs-lookup"><span data-stu-id="4f43a-116">The essentials area captures some of the important properties such as, the status, model, target IQN, and the software version.</span></span> 

![Enheten essentials](./media/storsimple-8000-device-dashboard/device-summary3.png)

## <a name="monitoring"></a><span data-ttu-id="4f43a-118">Övervakning</span><span class="sxs-lookup"><span data-stu-id="4f43a-118">Monitoring</span></span>

* <span data-ttu-id="4f43a-119">Den **aviseringar** panelen ger en ögonblicksbild av alla aktiva aviseringar för enheten, grupperat efter allvarlighetsgrad.</span><span class="sxs-lookup"><span data-stu-id="4f43a-119">The **Alerts** tile provides a snapshot of all the active alerts for your device, grouped by alert severity.</span></span>

    ![Aviseringen sida vid sida](./media/storsimple-8000-device-dashboard/device-summary4.png)

    <span data-ttu-id="4f43a-121">Klicka på rutan öppnas den **aviseringar** bladet och klicka sedan på en enskild varning för att visa ytterligare information om den här aviseringen, inklusive de rekommenderade åtgärder.</span><span class="sxs-lookup"><span data-stu-id="4f43a-121">Click the tile to open the **Alerts** blade and then click an individual alert to view additional details about that alert, including any recommended actions.</span></span> <span data-ttu-id="4f43a-122">Du kan också ta bort aviseringen om problemet har lösts.</span><span class="sxs-lookup"><span data-stu-id="4f43a-122">You can also clear the alert if the issue has been resolved.</span></span>

    ![Klicka på aviseringen sida vid sida](./media/storsimple-8000-device-dashboard/device-summary10.png)

* <span data-ttu-id="4f43a-124">Den **statusen och hälsan** panelen ger insikter om maskinvara komponentens hälsostatus för en enhet, inklusive enhetens status.</span><span class="sxs-lookup"><span data-stu-id="4f43a-124">The **Status and health** tile provides insights into the hardware component health for a device including the device status.</span></span> <span data-ttu-id="4f43a-125">Enhetens status kan vara offline, online, inaktiverat eller redo att konfigurera.</span><span class="sxs-lookup"><span data-stu-id="4f43a-125">The device status may be offline, online, deactivated, or ready to set up.</span></span>

    ![Statusen och hälsan panel](./media/storsimple-8000-device-dashboard/device-summary5.png)

* <span data-ttu-id="4f43a-127">Den **volymer** panelen innehåller en sammanfattning av antalet volymer i enheten grupperade efter status.</span><span class="sxs-lookup"><span data-stu-id="4f43a-127">The **Volumes** tile provides a summary of the number of volumes in your device grouped by status.</span></span>

    ![Volymbrickan](./media/storsimple-8000-device-dashboard/device-summary6.png)

    <span data-ttu-id="4f43a-129">Klicka på rutan öppnas den **volymer** listan bladet och klicka på en enskild volym för att visa eller ändra dess egenskaper.</span><span class="sxs-lookup"><span data-stu-id="4f43a-129">Click the tile to open the **Volumes** list blade, and then click on an individual volume to view or modify its properties.</span></span>
    
    ![Klicka på panelen volymer](./media/storsimple-8000-device-dashboard/device-summary9.png)
    
    <span data-ttu-id="4f43a-131">Mer information finns i så här [hantera volymer](storsimple-8000-manage-volumes-u2.md).</span><span class="sxs-lookup"><span data-stu-id="4f43a-131">For more information, see how to [manage volumes](storsimple-8000-manage-volumes-u2.md).</span></span>

* <span data-ttu-id="4f43a-132">I den **användning** diagram, kan du visa den primära lagringsutrymme som används på enheten och molnlagring som används under de senaste 7 dagarna standard tidsperiod.</span><span class="sxs-lookup"><span data-stu-id="4f43a-132">In the **Usage** chart, you can view the primary storage used across your device, and the cloud storage consumed over the past 7 days, the default time period.</span></span>

     ![Ikonen användning](./media/storsimple-8000-device-dashboard/device-summary7.png)
    
     <span data-ttu-id="4f43a-134">Om du vill välja en annan tidsskala, Använd den **redigera** alternativ i det övre högra hörnet i diagrammet.</span><span class="sxs-lookup"><span data-stu-id="4f43a-134">To choose a different time scale, use the **Edit** option in the top-right corner of the chart.</span></span>

     ![Redigera Användningsdiagram](./media/storsimple-8000-device-dashboard/device-summary12.png)

     <span data-ttu-id="4f43a-136">Du kan visa egenskaperna för totalt antal primära lagring (mängden data som skrivs av värdar på enheten) och totala molnlagring som används av din enhet under en viss tidsperiod i det här diagrammet.</span><span class="sxs-lookup"><span data-stu-id="4f43a-136">In this chart, you can view metrics for the total primary storage (the amount of data written by hosts to your device) and the total cloud storage consumed by your device over a period of time.</span></span>
  
     <span data-ttu-id="4f43a-137">I den här kontexten *primära lagringsplatsen* refererar till den totala mängden data som skrivs av värden, och kan delas upp på volymtyp: *primära nivåer lagring* innehåller både lokalt lagrade data och data nivåer till molnet.</span><span class="sxs-lookup"><span data-stu-id="4f43a-137">In this context, *primary storage* refers to the total amount of data written by the host, and can be broken down by volume type: *primary tiered storage* includes both locally stored data and data tiered to the cloud.</span></span> <span data-ttu-id="4f43a-138">*Primär lokalt Fäst lagring* innehåller bara data som lagras lokalt.</span><span class="sxs-lookup"><span data-stu-id="4f43a-138">*Primary locally pinned storage* includes just data stored locally.</span></span> <span data-ttu-id="4f43a-139">*Molnlagring*, å andra sidan är ett mått på den totala mängden data som lagras i molnet.</span><span class="sxs-lookup"><span data-stu-id="4f43a-139">*Cloud storage*, on the other hand, is a measurement of the total amount of data stored in the cloud.</span></span> <span data-ttu-id="4f43a-140">Den här innehåller nivåindelade data och säkerhetskopieringar.</span><span class="sxs-lookup"><span data-stu-id="4f43a-140">This storage includes tiered data and backups.</span></span> <span data-ttu-id="4f43a-141">Data som lagras i molnet är deduplicerad och komprimerade, medan primära lagringsplatsen anger hur mycket lagringsutrymme som används innan data är deduplicerad och komprimeras.</span><span class="sxs-lookup"><span data-stu-id="4f43a-141">The data stored in the cloud is deduplicated and compressed, whereas primary storage indicates the amount of storage used before the data is deduplicated and compressed.</span></span> <span data-ttu-id="4f43a-142">(Du kan jämföra dessa två tal för att få en uppfattning om hastigheten med komprimering.) För både primär och lagringsutrymmet i molnet, de belopp som visas baseras på frekvensen för spårning av som du konfigurerar.</span><span class="sxs-lookup"><span data-stu-id="4f43a-142">(You can compare these two numbers to get an idea of the compression rate.) For both primary and cloud storage, the amounts shown are based on the tracking frequency you configure.</span></span> <span data-ttu-id="4f43a-143">Till exempel om du väljer en frekvens som en vecka visar sedan diagrammet data för varje dag i föregående vecka.</span><span class="sxs-lookup"><span data-stu-id="4f43a-143">For example, if you choose a one week frequency, then the chart shows data for each day in the previous week.</span></span>

     <span data-ttu-id="4f43a-144">Om du vill visa mängden molnlagring som används över tid, Välj den **MOLN lagring används** alternativet.</span><span class="sxs-lookup"><span data-stu-id="4f43a-144">To see the amount of cloud storage consumed over time, select the **CLOUD STORAGE USED** option.</span></span> <span data-ttu-id="4f43a-145">Om du vill se det totala lagringsutrymmet som har skrivits av värden, Välj den **primära nivåer lagring används** och **primära LOKALT FÄST lagring används** alternativ.</span><span class="sxs-lookup"><span data-stu-id="4f43a-145">To see the total storage that has been written by the host, select the **PRIMARY TIERED STORAGE USED** and **PRIMARY LOCALLY PINNED STORAGE USED** options.</span></span> 
     <span data-ttu-id="4f43a-146">Mer information finns i [använda Enhetshanteraren för StorSimple-tjänsten för att övervaka din StorSimple-enhet](storsimple-monitor-device.md).</span><span class="sxs-lookup"><span data-stu-id="4f43a-146">For more information, see [Use the StorSimple Device Manager service to monitor your StorSimple device](storsimple-monitor-device.md).</span></span>


* <span data-ttu-id="4f43a-147">Den **kapacitet** panelen visar den primära lagring som är allokerade och återstående i enheten i förhållande till den totala lagringsstorleken för samma.</span><span class="sxs-lookup"><span data-stu-id="4f43a-147">The **Capacity** tile displays the primary storage that is provisioned and remaining across the device relative to the total storage available for the same.</span></span> <span data-ttu-id="4f43a-148">**Etablerad** avser mängden lagringsutrymme som är förberedd och tilldelat för användning, **återstående** refererar till den återstående kapacitet som kan etableras mellan den här enheten.</span><span class="sxs-lookup"><span data-stu-id="4f43a-148">**Provisioned** refers to the amount of storage that is prepared and allocated for use, **Remaining** refers to the remaining capacity that can be provisioned across this device.</span></span> 

    ![Ikonen användning](./media/storsimple-8000-device-dashboard/device-summary8.png)

    <span data-ttu-id="4f43a-150">Klicka på den här panelen om du vill visa hur kapaciteten etableras mellan nivåindelade och lokalt fästa volymer.</span><span class="sxs-lookup"><span data-stu-id="4f43a-150">Click this tile to view how the capacity is provisioned across tiered and locally pinned volumes.</span></span> <span data-ttu-id="4f43a-151">Den **återstående nivåer** kapacitet är den tillgängliga kapaciteten som kan etableras inklusive molnet, medan den **återstående lokala** är kapacitet kvar på diskar som är kopplade till den här enheten.</span><span class="sxs-lookup"><span data-stu-id="4f43a-151">The **Remaining Tiered** capacity is the available capacity that can be provisioned including cloud, while the **Remaining Local** is the capacity remaining on the disks attached to this device.</span></span>

    ![Klicka på Användningsdiagram](./media/storsimple-8000-device-dashboard/device-summary13.png)


## <a name="next-steps"></a><span data-ttu-id="4f43a-153">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="4f43a-153">Next steps</span></span>
* <span data-ttu-id="4f43a-154">Lär dig mer om den [StorSimple-tjänsten sammanfattning bladet](storsimple-8000-service-dashboard.md).</span><span class="sxs-lookup"><span data-stu-id="4f43a-154">Learn more about the [StorSimple service summary blade](storsimple-8000-service-dashboard.md).</span></span>
* <span data-ttu-id="4f43a-155">Lär dig mer om [använder Enhetshanteraren för StorSimple-tjänsten för att administrera din StorSimple-enhet](storsimple-8000-manager-service-administration.md).</span><span class="sxs-lookup"><span data-stu-id="4f43a-155">Learn more about [using the StorSimple Device Manager service to administer your StorSimple device](storsimple-8000-manager-service-administration.md).</span></span>

