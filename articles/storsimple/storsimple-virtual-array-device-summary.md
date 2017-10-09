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
# <a name="use-hello-device-summary-blade-for-storsimple-device-manager-connected-toostorsimple-virtual-array"></a>Använd hello enheten sammanfattning bladet för StorSimple Enhetshanteraren anslutna tooStorSimple virtuella matris

## <a name="overview"></a>Översikt

Hej StorSimple Enhetshanteraren enheten bladet innehåller en översikt över av en virtuell StorSimple-matris som har registrerats med en viss StorSimple Enhetshanteraren, syntaxmarkering dessa problem med enheter som behöver åtgärdas av en systemadministratör. Den här kursen introducerar hello enheten sammanfattning-bladet, förklarar hello innehåll och funktionen och beskriver hello uppgifter som du kan utföra från det här bladet.

hello enheten sammanfattning bladet visar hello följande information:

![Instrumentpanel för enhet](./media/storsimple-virtual-array-device-summary/device-blade.png)



## <a name="management"></a>Hantering

I bladet för hello StorSimple-enhet ser du hello alternativ för att hantera din StorSimple-enhet. Visas hello management kommandon överst hello av hello-bladet och hello vänster. Använder dessa alternativ tooadd resurser eller volymer, uppdatera eller växla över dina virtuella matris.

hello essentials området samlar in några av hello viktiga egenskaper som, hello status, modell, programvaruversionen samt en länk toohello **Webbgränssnittet** hello matrisen. Om du är i ett internt nätverk, kan du direkt starta hello [lokala webbgränssnittet](storsimple-ova-web-ui-admin.md) tooadminister virtuella matrisen.

![Enheten essentials](./media/storsimple-virtual-array-device-summary/device-essentials.png)

## <a name="storsimple-device-summary"></a>Översikt över StorSimple-enhet

* Hej **aviseringar** panelen innehåller en ögonblicksbild av alla hello aktiva aviseringar för virtuella matrisen, grupperat efter allvarlighetsgrad. Klicka på hello panelen tooopen hello **aviseringar** bladet och klicka sedan på en enskild varning tooview ytterligare information om den här aviseringen, inklusive de rekommenderade åtgärder. Du kan även radera hello avisering om hello problemet har lösts.

* Hej **kapacitet** panelen visar hello primära lagring som har etablerats och återstående över hello virtuella enheten relativa toohello totala lagringsstorleken för hello samma. **Etablerad** refererar toohello mängden lagringsutrymme som är förberedd och tilldelat för användning, **återstående** refererar toohello återstående kapacitet som kan tillhandahållas i den här enheten. Hej **återstående nivåer** kapaciteten är hello tillgänglig kapacitet som kan etableras inklusive molnet, medan hello **återstående lokala** är hello kapacitet kvar på hello diskar ansluten virtuell toothis matris.

* I hello **användning** diagram, kan du visa hello primära lagringsutrymme som används över virtuella matris, samt hello molnlagring som används över hello senaste 7 dagarna, hello standard tidsperiod. Använd hello **redigera** alternativet i hello övre högra hörnet av hello diagram toochoose olika skalan.

* Hej **resurser** eller **volymer** panelen innehåller en översikt över hello antalet resurser eller volymer i enheten grupperade efter status. Klicka på hello panelen tooopen hello **resurser** eller **volymer** bladet och sedan klicka på en enskild resurs eller volym tooview eller ändra dess egenskaper. Mer information finns i hur för[hanterar resurser](storsimple-virtual-array-manage-shares.md) eller [hantera volymer](storsimple-virtual-array-manage-volumes.md).

## <a name="next-steps"></a>Nästa steg
Lär dig att:
- [Hantera resurser på en virtuell StorSimple-matris](storsimple-virtual-array-manage-shares.md)
    
- [Hantera volymer på en virtuell StorSimple-matris](storsimple-virtual-array-manage-volumes.md)

