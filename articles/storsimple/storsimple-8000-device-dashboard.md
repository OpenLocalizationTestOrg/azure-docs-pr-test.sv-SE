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
# <a name="use-hello-device-summary-in-storsimple-device-manager-service"></a>Använd hello sammanfattningen i Enhetshanteraren för StorSimple-tjänsten

## <a name="overview"></a>Översikt
Översikt över bladet för hello StorSimple-enhet ger en översikt över information för en specifik StorSimple-enhet, däremot toohello tjänsten sammanfattning bladet som ger information om alla hello-enheter som ingår i Microsoft Azure StorSimple-lösningen.

hello enheten sammanfattning bladet innehåller en översikt över av en StorSimple 8000-serieenhet som har registrerats med en viss StorSimple Enhetshanteraren, syntaxmarkering dessa problem med enheter som behöver åtgärdas av en systemadministratör. Den här kursen introducerar hello enheten sammanfattning-bladet, förklarar hello innehåll och funktionen och beskriver hello uppgifter som du kan utföra från det här bladet.

hello enheten sammanfattning bladet visar hello följande information:

![Översikt över bladet för enhet](./media/storsimple-8000-device-dashboard/device-summary1.png)

## <a name="management-command-bar"></a>Hantering av kommandofältet

I bladet för hello StorSimple-enhet ser du hello alternativ för att hantera din StorSimple-enhet. Visas hello management kommandon överst hello av hello-bladet och hello vänster. Använder dessa alternativ tooadd resurser eller volymer, uppdatera eller växla över din enhet.

![Hantering av kommandofältet](./media/storsimple-8000-device-dashboard/device-summary2.png)

## <a name="essentials"></a>Essentials

hello essentials området avbildar hello viktiga egenskaper som, hello status, modell, mål IQN och hello programvaruversionen. 

![Enheten essentials](./media/storsimple-8000-device-dashboard/device-summary3.png)

## <a name="monitoring"></a>Övervakning

* Hej **aviseringar** panelen ger en ögonblicksbild av alla hello aktiva aviseringar för enheten, grupperat efter allvarlighetsgrad.

    ![Aviseringen sida vid sida](./media/storsimple-8000-device-dashboard/device-summary4.png)

    Klicka på hello panelen tooopen hello **aviseringar** bladet och klicka sedan på en enskild varning tooview ytterligare information om den här aviseringen, inklusive de rekommenderade åtgärder. Du kan även radera hello avisering om hello problemet har lösts.

    ![Klicka på aviseringen sida vid sida](./media/storsimple-8000-device-dashboard/device-summary10.png)

* Hej **statusen och hälsan** panelen ger insikter om hello maskinvara komponentens hälsostatus för en enhet, inklusive hello Enhetsstatus. hello Enhetsstatus kan vara offline, online, inaktiverat eller redo tooset upp.

    ![Statusen och hälsan panel](./media/storsimple-8000-device-dashboard/device-summary5.png)

* Hej **volymer** panelen innehåller en översikt över hello antalet volymer i enheten grupperade efter status.

    ![Volymbrickan](./media/storsimple-8000-device-dashboard/device-summary6.png)

    Klicka på hello panelen tooopen hello **volymer** bladet och sedan klicka på en enskild volym tooview eller ändra dess egenskaper.
    
    ![Klicka på panelen volymer](./media/storsimple-8000-device-dashboard/device-summary9.png)
    
    Mer information finns i hur för[hantera volymer](storsimple-8000-manage-volumes-u2.md).

* I hello **användning** diagram, kan du visa hello primära lagringsutrymme som används i hela enheten och hello molnlagring som används över hello senaste 7 dagarna, hello standard tidsperiod.

     ![Ikonen användning](./media/storsimple-8000-device-dashboard/device-summary7.png)
    
     toochoose en annan tidsskala använda hello **redigera** alternativ i hello övre högra hörnet av hello diagram.

     ![Redigera Användningsdiagram](./media/storsimple-8000-device-dashboard/device-summary12.png)

     I det här diagrammet kan du visa måtten för hello totala primära lagringsutrymmet (hello mängden data som skrivs av värdar tooyour enhet) och hello totala molnlagring som används av din enhet under en viss tidsperiod.
  
     I den här kontexten *primära lagringsplatsen* refererar toohello totala mängden data som skrivs av hello-värden och kan delas upp på volymtyp: *primära nivåer lagring* innehåller både lokalt lagrade data och data nivåindelad toohello moln. *Primär lokalt Fäst lagring* innehåller bara data som lagras lokalt. *Molnlagring*, på hello andra sidan, är ett mått på hello totala mängden data som lagras i hello molnet. Den här innehåller nivåindelade data och säkerhetskopieringar. hello-data som lagras i molnet hello är deduplicerad och komprimerade, medan primära lagringsplatsen anger hello mängden lagringsutrymme som används innan hello data är deduplicerad och komprimeras. (Du kan jämföra dessa två tal tooget en uppfattning om hello komprimeringshastighet.) För både primär och molnlagring hello belopp som visas baseras på hello spårning frekvens som du konfigurerar. Till exempel om du väljer en frekvens som en vecka sedan hello diagrammet visar data för varje dag i hello föregående vecka.

     toosee hello mängden molnlagring som används över tid, väljer hello **MOLN lagring används** alternativet. toosee hello totalt lagringsutrymme som har skrivits av hello-värden, Välj hello **primära nivåer lagring används** och **primära LOKALT FÄST lagring används** alternativ. 
     Mer information finns i [Använd hello StorSimple Enhetshanteraren service toomonitor StorSimple-enheten](storsimple-monitor-device.md).


* Hej **kapacitet** panelen visar hello primära lagring som har etablerats och återstående över hello enheten relativa toohello totala lagringsstorleken för hello samma. **Etablerad** refererar toohello mängden lagringsutrymme som är förberedd och tilldelat för användning, **återstående** refererar toohello återstående kapacitet som kan tillhandahållas i den här enheten. 

    ![Ikonen användning](./media/storsimple-8000-device-dashboard/device-summary8.png)

    Klicka på den här panelen tooview hur hello kapacitet etableras mellan nivåindelade och lokalt fästa volymer. Hej **återstående nivåer** kapaciteten är hello tillgänglig kapacitet som kan etableras inklusive molnet, medan hello **återstående lokala** är hello kapacitet kvar på hello diskar ansluten toothis enhet.

    ![Klicka på Användningsdiagram](./media/storsimple-8000-device-dashboard/device-summary13.png)


## <a name="next-steps"></a>Nästa steg
* Mer information om hello [StorSimple-tjänsten sammanfattning bladet](storsimple-8000-service-dashboard.md).
* Lär dig mer om [med hello StorSimple Enhetshanteraren service tooadminister StorSimple-enheten](storsimple-8000-manager-service-administration.md).

