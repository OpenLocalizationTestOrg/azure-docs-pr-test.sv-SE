---
title: aaaUse StorSimple 8000-serien sammanfattningen | Microsoft Docs
description: "Beskriver hello StorSimple-tjänsten sammanfattning bladet och förklarar hur toouse den toomonitor hello hälsotillståndet för din StorSimple-lösning."
services: storsimple
documentationcenter: 
author: alkohli
manager: timlt
editor: 
ms.assetid: 
ms.service: storsimple
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/27/2017
ms.author: alkohli
ms.openlocfilehash: ec1d971a8656df0be6eb1e3ec2603d97737ff070
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="use-hello-service-summary-blade-for-storsimple-8000-series-device"></a>Använd hello service sammanfattning bladet för StorSimple 8000-serieenhet

## <a name="overview"></a>Översikt

Hej StorSimple Enhetshanteraren service sammanfattning bladet innehåller en sammanfattning av alla hello-enheter som är anslutna toohello StorSimple Device Manager-tjänsten, markera de enheter som behöver åtgärdas av en systemadministratör. Den här kursen introducerar hello service sammanfattning-bladet, förklarar hello instrumentpanelinnehåll och funktionen och beskriver hello uppgifter som du kan utföra den här sidan.

![Sammanfattning av tjänst](./media/storsimple-8000-service-dashboard/service-summary1.png)


## <a name="management-commands"></a>Kommandon för hantering

Hello alternativ för att hantera dina StorSimple enheten Manager-tjänsten och hello StorSimple 8000-serien enheter registrerade toothis service visas hello StorSimple-tjänsten sammanfattning bladet som. Visas hello management kommandon överst hello av hello-bladet och hello vänster.

![Kommandofältet](./media/storsimple-8000-service-dashboard/service-summary2.png)

Använd dessa alternativ tooperform olika åtgärder som Lägg till resurser eller volymer eller Övervakare hello olika jobb som körs på hello StorSimple-enheter.


## <a name="essentials"></a>Essentials

hello essentials området samlar in några av hello viktiga egenskaper, till exempel hello resursgruppens namn, plats och prenumeration som din StorSimple-Enhetshanteraren skapades.

![Essentials](./media/storsimple-8000-service-dashboard/service-summary3.png)

## <a name="storsimple-device-manager-service-summary"></a>Sammanfattning av StorSimple Enhetshanteraren tjänst

* Hej **aviseringar** panelen ger en ögonblicksbild av alla hello aktiva aviseringar över alla enheter, grupperat efter allvarlighetsgrad.

    ![Ikonen aviseringar](./media/storsimple-8000-service-dashboard/service-summary4.png)

    Klicka på ikonen för hello öppnar hello **aviseringar** bladet där du kan klicka på en enskild varning tooview ytterligare information om den här aviseringen, inklusive de rekommenderade åtgärder. Du kan även radera hello avisering om hello problemet har lösts.

    ![Klicka på ikonen för aviseringar](./media/storsimple-8000-service-dashboard/service-summary8.png)

* Hej **kapacitet** panelen visar visar hello primära lagring som är etablerad och återstående över alla enheter relativa toohello totala lagringsutrymmet tillgängligt på alla enheter. **Etablerad** refererar toohello mängden lagringsutrymme som är förberedd och tilldelat för användning, **återstående** refererar toohello återstående kapacitet som kan etableras på alla enheter.

    ![Kapacitet sida vid sida](./media/storsimple-8000-service-dashboard/service-summary6.png)

    Hej **återstående nivåer** kapaciteten är hello tillgänglig kapacitet som kan etableras inklusive molnet, medan hello **återstående lokala** är hello kapacitet kvar på hello diskar ansluten toohello Enheter för StorSimple 8000-serien.


* I hello **användning** diagram, kan du visa hello relevanta mått för dina enheter. Du kan visa hello primära lagringsutrymme som används på alla enheter och hello molnlagring som används av enheter över hello senaste 7 dagarna, hello standard tidsperiod. 

    ![Ikonen användning](./media/storsimple-8000-service-dashboard/service-summary7.png) 

    toochoose en annan tidsskala använda hello **redigera** alternativ i hello övre högra hörnet av hello diagram.

     ![Klicka på ikonen användning](./media/storsimple-8000-service-dashboard/service-summary10.png)

     ![Exportera diagram](./media/storsimple-8000-service-dashboard/service-summary11.png)

* Hej **enheter** panelen innehåller en översikt över hello antalet StorSimple 8000-serien enheter i din StorSimple Enhetshanteraren grupperade efter status för enheter. 

    ![Panelen för enheter](./media/storsimple-8000-service-dashboard/service-summary5.png)

    Klicka på den här panelen tooopen hello **enheter** listan bladet och klicka på en enskild enhet toodrill i hello enheten sammanfattning specifika toohello enheten. Du kan också utföra specifika åtgärder från en viss enhet sammanfattning-bladet. Mer information om hello enheten sammanfattning bladet går för[enheten sammanfattning bladet](storsimple-8000-device-dashboard.md).

    ![Klicka på ikonen för enheter](./media/storsimple-8000-service-dashboard/service-summary9.png)

## <a name="view-hello-activity-logs"></a>Visa hello aktivitetsloggar

tooview hello olika åtgärder som utförs inom din StorSimple Enhetshanteraren, klicka på hello **aktivitetsloggar** länk på hello vänster sida av din StorSimple-tjänsten sammanfattning-bladet. Detta tar toohello **aktivitetsloggar** bladet, där du kan se en översikt över hello senaste åtgärder utförs.

![Aktivitetsloggar](./media/storsimple-8000-service-dashboard/activity-logs1.png)
## <a name="next-steps"></a>Nästa steg

* Läs mer om hur för[Använd hello StorSimple Enhetshanteraren service tooadminister StorSimple-enheten](storsimple-8000-manager-service-administration.md).

