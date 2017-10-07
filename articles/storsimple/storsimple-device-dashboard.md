---
title: aaaUse hello StorSimple Manager enheten instrumentpanelen | Microsoft Docs
description: "Beskriver hello StorSimple Manager-tjänsten enhet instrumentpanelen och hur toouse den tooview storage-mätvärden och anslutna initierare och hitta hello serienummer och IQN."
services: storsimple
documentationcenter: NA
author: alkohli
manager: timlt
editor: 
ms.assetid: 6c213969-a385-461f-b698-78ef5b8a79cc
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 02/27/2017
ms.author: alkohli
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: e213fc0a081c21b9d6b408a3dd845cc93a31e250
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="use-hello-device-dashboard-in-storsimple-manager-service"></a>Använd hello enheten instrumentpanelen i StorSimple Manager-tjänsten  

## <a name="overview"></a>Översikt
hello StorSimple Manager enheten instrumentpanelen ger dig en översikt över information för en specifik StorSimple-enhet, däremot toohello tjänsten instrumentpanel som ger information om alla hello-enheter som ingår i Microsoft Azure StorSimple-lösningen.

![Instrumentpanelssidan för enhet](./media/storsimple-device-dashboard/StorSimple_DeviceDashbaord1M.png)

hello instrumentpanelen innehåller hello följande information:

* **Diagramområde** – du kan se hello relevanta storage-mätvärden i hello diagramområde hello överst i hello instrumentpanelen. I det här diagrammet kan du visa måtten för hello totala primära lagringsutrymmet (hello mängden data som skrivs av värdar tooyour enhet) och hello totala molnlagring som används av din enhet under en viss tidsperiod.
  
     I den här kontexten *primära lagringsplatsen* refererar toohello totala mängden data som skrivs av hello-värden och kan delas upp på volymtyp: *primära nivåer lagring* innehåller både lokalt lagrade data och data nivåindelad toohello moln; *primära lokalt Fäst lagring* innehåller bara data som lagras lokalt. *Molnlagring*, på hello andra sidan, är ett mått på hello totala mängden data som lagras i hello molnet. Detta inkluderar nivåindelade data och säkerhetskopieringar. Observera att data som lagras i molnet hello är deduplicerad och komprimerade, medan primära lagringsplatsen anger hello mängden lagringsutrymme som används innan hello data är deduplicerad och komprimeras. (Du kan jämföra dessa två tal tooget en uppfattning om hello komprimeringshastighet.) För både primär och molnlagring hello belopp som visas baseras på hello spårning frekvens som du konfigurerar. Till exempel om du väljer en frekvens som en vecka visas sedan hello diagrammet data för varje dag i hello föregående vecka.
  
     Du kan konfigurera hello diagrammet på följande sätt:
  
  * toosee hello mängden molnlagring som används över tid, väljer hello **MOLN lagring används** alternativet. toosee hello totalt lagringsutrymme som har skrivits av hello-värden, Välj hello **primära nivåer lagring används** och **primära LOKALT FÄST lagring används** alternativ. Båda alternativen är markerade i hello bild; därför visar hello diagram lagring både till molnet och primära lagringsplatsen. Observera att alla primära lagringsplatsen används tidigare tooinstalling uppdatering 2 representeras av hello **primära nivåer lagring används** rad.
  * Använd hello nedrullningsbara menyn i hello övre högra hörnet av hello diagram toospecify en 1 vecka, 1-månads, 3-månaders eller 1 års tidsperiod. Observera att hello översta diagrammet uppdateras bara en gång per dag och därför visar hello summor föregående dag.
    
    Mer information finns i [Använd hello StorSimple Manager service toomonitor StorSimple-enheten](storsimple-monitor-device.md).
* **Användningsöversikten** – i hello **användningsöversikten** område, du kan se hello mängden lagringsutrymme som primärt används hello mängden etablerade lagring och hello maximal lagringskapacitet för din enhet. Genom att jämföra dessa användning siffror toohello största mängden lagringsutrymme som är tillgänglig, visas en överblick över om du behöver ytterligare lagringsutrymme för tooobtain. Observera att den här översikten uppdateras var 15: e minut och på grund av hello skillnaden i uppdateringsfrekvens, kan visa olika nummer än de som visas i hello diagramområde ovan, som uppdateras dagligen. Mer information finns i [Använd hello StorSimple Manager service toomonitor StorSimple-enheten](storsimple-monitor-device.md).
* **Aviseringar** – hello **aviseringar** innehåller en översikt över hello aviseringar för din enhet. Aviseringar är grupperade efter allvarlighetsgrad och ett antal tillhandahålls av hello antal varningar på varje allvarlighetsgrad. Klicka på hello avisering allvarlighetsgrad öppnar en vy som omfattas av hello aviseringar fliken tooshow du bara hello aviseringar om att allvarlighetsgrad för den här enheten.
* **Jobb** – hello **jobb** området visar hello av resultatet av den senaste jobb aktiviteten. Detta kan garantera att hello system fungerar som förväntat eller det att du vet att du måste tootake korrigerande åtgärder. Klicka på toosee mer information om senast slutförda jobb **jobb lyckades i hello senaste 24 timmarna**.
* Hej **snabböversikten** området på hello höger i hello instrumentpanelen innehåller användbar information som enhetsmodell serienummer, status, beskrivning och antal volymer.

Du kan också konfigurera redundans och visa anslutna initierare från hello enheten instrumentpanelen.

hello vanliga aktiviteter som kan utföras på den här sidan är:

* Visa anslutna initierare
* Hitta hello enhetens serienummer
* Hitta hello enheten målet IQN

## <a name="view-connected-initiators"></a>Visa anslutna initierare
Du kan visa hello iSCSI-initierare som är anslutna tooyour enhet genom att klicka på hello **visa anslutna initierare** länken i hello **snabböversikten** område i instrumentpanelen i enheten. Den här sidan innehåller en tabell lista över hello initierare som har anslutit tooyour enhet. För varje initierare kan du se:

* Hej iSCSI kvalificerade namn (IQN) för hello anslutna initierare.
* hello namnet på hello åtkomstkontrollpost (ACR) som gör att anslutna initieraren.
* hello IP-adressen för hello anslutna initierare.
* hello nätverksgränssnitt som hello-initieraren är anslutna tooon lagringsenheten. De kan variera från DATA 0 tooDATA 5.
* Alla hello volymer som hello anslutna initierare tillåts tooaccess enligt toohello aktuella ACR-konfigurationen.

Om du ser oväntat initierare i listan eller inte ser hello förväntades viktiga granska din ACR-konfiguration. Högst 512 initierare kan ansluta tooyour enheten.

## <a name="find-hello-device-serial-number"></a>Hitta hello enhetens serienummer
Du kan behöva hello enhetens serienummer när du konfigurerar Microsoft Multipath I/O (MPIO) på hello enhet. Utför följande steg toofind hello enhetens serienummer hello.

#### <a name="toofind-hello-device-serial-number"></a>toofind hello enhetens serienummer
1. Navigera för**enheter** > **instrumentpanelen**.
2. Leta upp hello i hello högra fönstret hello instrumentpanelen **snabböversikten** område.
3. Bläddra nedåt och hitta hello serienummer.

## <a name="find-hello-device-target-iqn"></a>Hitta hello enheten målet IQN
Du kan behöva hello enheten mål IQN när du konfigurerar hello CHAP Challenge Handshake Authentication Protocol () på din StorSimple-enhet. Utföra hello följande steg toofind hello enheten mål IQN.

### <a name="toofind-hello-device-target-iqn"></a>toofind hello enheten mål IQN
1. Navigera för**enheter** > **instrumentpanelen**.
2. Leta upp hello i hello högra fönstret hello instrumentpanelen **snabböversikten** område.
3. Bläddra nedåt och hitta hello mål IQN.

## <a name="next-steps"></a>Nästa steg
* Mer information om hello [StorSimple Manager service instrumentpanelen](storsimple-service-dashboard.md).
* Lär dig mer om [med hello StorSimple Manager service tooadminister StorSimple-enheten](storsimple-manager-service-administration.md).

