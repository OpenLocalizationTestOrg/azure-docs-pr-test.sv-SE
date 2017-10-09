---
title: aaaStorSimple virtuella matris enheten sammanfattning bladet | Microsoft Docs
description: "Beskriver hello enheten sammanfattning bladet för StorSimple Enhetshanteraren och förklarar hur toouse den toomonitor hello hälsotillståndet för din virtuella StorSimple-matris."
services: storsimple
documentationcenter: 
author: manuaery
manager: syadav
editor: 
ms.assetid: a13c1ea7-6428-4234-84a6-0ebf51670a85
ms.service: storsimple
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/29/2016
ms.author: manuaery
ms.openlocfilehash: 3649eaac8a924a772f310a809ddf9706e912157a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="use-hello-device-summary-blade-for-storsimple-device-manager-connected-toostorsimple-virtual-array"></a><span data-ttu-id="a90ac-103">Använd hello enheten sammanfattning bladet för StorSimple Enhetshanteraren anslutna tooStorSimple virtuella matris</span><span class="sxs-lookup"><span data-stu-id="a90ac-103">Use hello device summary blade for StorSimple Device Manager connected tooStorSimple Virtual Array</span></span>

## <a name="overview"></a><span data-ttu-id="a90ac-104">Översikt</span><span class="sxs-lookup"><span data-stu-id="a90ac-104">Overview</span></span>

<span data-ttu-id="a90ac-105">Hej StorSimple Enhetshanteraren enheten bladet innehåller en översikt över av en virtuell StorSimple-matris som har registrerats med en viss StorSimple Enhetshanteraren, syntaxmarkering dessa problem med enheter som behöver åtgärdas av en systemadministratör.</span><span class="sxs-lookup"><span data-stu-id="a90ac-105">hello StorSimple Device Manager device blade provides a summary view of a StorSimple Virtual Array that is registered with a given StorSimple Device Manager, highlighting those device issues that need a system administrator's attention.</span></span> <span data-ttu-id="a90ac-106">Den här kursen introducerar hello enheten sammanfattning-bladet, förklarar hello innehåll och funktionen och beskriver hello uppgifter som du kan utföra från det här bladet.</span><span class="sxs-lookup"><span data-stu-id="a90ac-106">This tutorial introduces hello device summary blade, explains hello content and function, and describes hello tasks that you can perform from this blade.</span></span>

<span data-ttu-id="a90ac-107">hello enheten sammanfattning bladet visar hello följande information:</span><span class="sxs-lookup"><span data-stu-id="a90ac-107">hello device summary blade displays hello following information:</span></span>

![Instrumentpanel för enhet](./media/storsimple-virtual-array-device-summary/device-blade.png)



## <a name="management"></a><span data-ttu-id="a90ac-109">Hantering</span><span class="sxs-lookup"><span data-stu-id="a90ac-109">Management</span></span>

<span data-ttu-id="a90ac-110">I bladet för hello StorSimple-enhet ser du hello alternativ för att hantera din StorSimple-enhet.</span><span class="sxs-lookup"><span data-stu-id="a90ac-110">In hello StorSimple device blade, you see hello options for managing your StorSimple device.</span></span> <span data-ttu-id="a90ac-111">Visas hello management kommandon överst hello av hello-bladet och hello vänster.</span><span class="sxs-lookup"><span data-stu-id="a90ac-111">You see hello management commands across hello top of hello blade and on hello left side.</span></span> <span data-ttu-id="a90ac-112">Använder dessa alternativ tooadd resurser eller volymer, uppdatera eller växla över dina virtuella matris.</span><span class="sxs-lookup"><span data-stu-id="a90ac-112">Use these options tooadd shares or volumes, or update or fail over your virtual array.</span></span>

<span data-ttu-id="a90ac-113">hello essentials området samlar in några av hello viktiga egenskaper som, hello status, modell, programvaruversionen samt en länk toohello **Webbgränssnittet** hello matrisen.</span><span class="sxs-lookup"><span data-stu-id="a90ac-113">hello essentials area captures some of hello important properties such as, hello status, model, software version as well as a link toohello **Web UI** of hello array.</span></span> <span data-ttu-id="a90ac-114">Om du är i ett internt nätverk, kan du direkt starta hello [lokala webbgränssnittet](storsimple-ova-web-ui-admin.md) tooadminister virtuella matrisen.</span><span class="sxs-lookup"><span data-stu-id="a90ac-114">If you are on an internal network, you can directly launch hello [local web UI](storsimple-ova-web-ui-admin.md) tooadminister your virtual array.</span></span>

![Enheten essentials](./media/storsimple-virtual-array-device-summary/device-essentials.png)

## <a name="storsimple-device-summary"></a><span data-ttu-id="a90ac-116">Översikt över StorSimple-enhet</span><span class="sxs-lookup"><span data-stu-id="a90ac-116">StorSimple device summary</span></span>

* <span data-ttu-id="a90ac-117">Hej **aviseringar** panelen innehåller en ögonblicksbild av alla hello aktiva aviseringar för virtuella matrisen, grupperat efter allvarlighetsgrad.</span><span class="sxs-lookup"><span data-stu-id="a90ac-117">hello **Alerts** tile provides a snapshot of all hello active alerts for your virtual array, grouped by alert severity.</span></span> <span data-ttu-id="a90ac-118">Klicka på hello panelen tooopen hello **aviseringar** bladet och klicka sedan på en enskild varning tooview ytterligare information om den här aviseringen, inklusive de rekommenderade åtgärder.</span><span class="sxs-lookup"><span data-stu-id="a90ac-118">Click hello tile tooopen hello **Alerts** blade and then click an individual alert tooview additional details about that alert, including any recommended actions.</span></span> <span data-ttu-id="a90ac-119">Du kan även radera hello avisering om hello problemet har lösts.</span><span class="sxs-lookup"><span data-stu-id="a90ac-119">You can also clear hello alert if hello issue has been resolved.</span></span>

* <span data-ttu-id="a90ac-120">Hej **kapacitet** panelen visar hello primära lagring som har etablerats och återstående över hello virtuella enheten relativa toohello totala lagringsstorleken för hello samma.</span><span class="sxs-lookup"><span data-stu-id="a90ac-120">hello **Capacity** tile displays hello primary storage that is provisioned and remaining across hello virtual device relative toohello total storage available for hello same.</span></span> <span data-ttu-id="a90ac-121">**Etablerad** refererar toohello mängden lagringsutrymme som är förberedd och tilldelat för användning, **återstående** refererar toohello återstående kapacitet som kan tillhandahållas i den här enheten.</span><span class="sxs-lookup"><span data-stu-id="a90ac-121">**Provisioned** refers toohello amount of storage that is prepared and allocated for use, **Remaining** refers toohello remaining capacity that can be provisioned across this device.</span></span> <span data-ttu-id="a90ac-122">Hej **återstående nivåer** kapaciteten är hello tillgänglig kapacitet som kan etableras inklusive molnet, medan hello **återstående lokala** är hello kapacitet kvar på hello diskar ansluten virtuell toothis matris.</span><span class="sxs-lookup"><span data-stu-id="a90ac-122">hello **Remaining Tiered** capacity is hello available capacity that can be provisioned including cloud, while hello **Remaining Local** is hello capacity remaining on hello disks attached toothis virtual array.</span></span>

* <span data-ttu-id="a90ac-123">I hello **användning** diagram, kan du visa hello primära lagringsutrymme som används över virtuella matris, samt hello molnlagring som används över hello senaste 7 dagarna, hello standard tidsperiod.</span><span class="sxs-lookup"><span data-stu-id="a90ac-123">In hello **Usage** chart, you can view hello primary storage used across your virtual array, as well as hello cloud storage consumed  over hello past 7 days, hello default time period.</span></span> <span data-ttu-id="a90ac-124">Använd hello **redigera** alternativet i hello övre högra hörnet av hello diagram toochoose olika skalan.</span><span class="sxs-lookup"><span data-stu-id="a90ac-124">Use hello **Edit** option in hello top-right corner of hello chart toochoose a different time scale.</span></span>

* <span data-ttu-id="a90ac-125">Hej **resurser** eller **volymer** panelen innehåller en översikt över hello antalet resurser eller volymer i enheten grupperade efter status.</span><span class="sxs-lookup"><span data-stu-id="a90ac-125">hello **Shares** or **Volumes** tile provides a summary of hello number of shares or volumes in your device grouped by status.</span></span> <span data-ttu-id="a90ac-126">Klicka på hello panelen tooopen hello **resurser** eller **volymer** bladet och sedan klicka på en enskild resurs eller volym tooview eller ändra dess egenskaper.</span><span class="sxs-lookup"><span data-stu-id="a90ac-126">Click hello tile tooopen hello **Shares**  or **Volumes** list blade, and then click on an individual share or volume tooview or modify its properties.</span></span> <span data-ttu-id="a90ac-127">Mer information finns i hur för[hanterar resurser](storsimple-virtual-array-manage-shares.md) eller [hantera volymer](storsimple-virtual-array-manage-volumes.md).</span><span class="sxs-lookup"><span data-stu-id="a90ac-127">For more information, see how too[manage shares](storsimple-virtual-array-manage-shares.md) or [manage volumes](storsimple-virtual-array-manage-volumes.md).</span></span>

## <a name="next-steps"></a><span data-ttu-id="a90ac-128">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="a90ac-128">Next steps</span></span>
<span data-ttu-id="a90ac-129">Lär dig att:</span><span class="sxs-lookup"><span data-stu-id="a90ac-129">Learn how to:</span></span>
- [<span data-ttu-id="a90ac-130">Hantera resurser på en virtuell StorSimple-matris</span><span class="sxs-lookup"><span data-stu-id="a90ac-130">Manage shares on a StorSimple Virtual Array</span></span>](storsimple-virtual-array-manage-shares.md)
    
- [<span data-ttu-id="a90ac-131">Hantera volymer på en virtuell StorSimple-matris</span><span class="sxs-lookup"><span data-stu-id="a90ac-131">Manage volumes on a StorSimple Virtual Array</span></span>](storsimple-virtual-array-manage-volumes.md)

