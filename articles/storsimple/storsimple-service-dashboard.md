---
title: aaaStorSimple Manager service instrumentpanelen | Microsoft Docs
description: "Beskriver hello StorSimple Manager-tjänsten instrumentpanelen och förklarar hur toouse den toomonitor hello hälsotillståndet för din StorSimple-lösning."
services: storsimple
documentationcenter: 
author: SharS
manager: carmonm
editor: 
ms.assetid: fb0f131d-d60b-45d7-ace2-56d0502e6627
ms.service: storsimple
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 05/24/2016
ms.author: v-sharos
ms.openlocfilehash: dc1197eb5deac337215b260845631a4f04be1011
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="use-hello-storsimple-manager-service-dashboard"></a>Använd instrumentpanelen för hello StorSimple Manager-tjänsten
## <a name="overview"></a>Översikt
Hej StorSimple Manager-tjänsten instrumentpanelssidan innehåller en sammanfattning av alla hello-enheter som är anslutna toohello StorSimple Manager-tjänsten, markera dem som behöver åtgärdas av en systemadministratör. Den här kursen introducerar hello instrumentpanelssida, förklarar hello instrumentpanelinnehåll och funktionen och beskriver hello uppgifter som du kan utföra den här sidan.

![Instrumentpanel](./media/storsimple-service-dashboard/HCS_ServiceDashboard.png)

Hej StorSimple Manager-tjänsten instrumentpanelen visar hello följande information:

* I hello **diagram** området som du kan se hello relevanta mått diagram för dina enheter. Du kan visa hello primära lagring (lokalt Fäst och nivåer) används över alla hello enheter, samt hello molnlagring som används av enheter under en viss tidsperiod. Använd hello kontroller i hello övre högra hörnet av hello diagram toospecify 1 vecka, 1-månads, 3-månaders eller 1 års skalan.
* Hej **användningsöversikten** visar hello primära lagring som etablerats och används av alla enheter relativa toohello totala lagringsutrymmet tillgängligt på alla enheter. **Etablerad** refererar toohello mängden lagringsutrymme som är förberedd och tilldelat för användning, medan **används** refererar toousage volymer som visas av hello initierare som är anslutna toohello enheter.
* Hej **aviseringar** området innehåller en ögonblicksbild av alla hello aktiva aviseringar över alla hello-enheter, grupperat efter allvarlighetsgrad. Om du klickar på hello allvarlighetsgrad öppnas hello **aviseringar** sida, begränsat tooshow dessa aviseringar. På hello **aviseringar** kan du klicka på en enskild varning tooview ytterligare information om den här aviseringen, inklusive alla rekommenderade åtgärder. Du kan även radera hello avisering om hello problemet har lösts.
* Hej **jobb** området innehåller en ögonblicksbild över senaste jobb på alla enheter som är anslutna tooyour service. Det finns länkar som du kan använda toolook jobb som pågår, de som misslyckades i hello senaste 24 timmarna, eller som är schemalagda toorun i hello nästkommande 24 timmar.
* Hej **snabböversikten** området innehåller användbar information, till exempel status för tjänsten, antal enheter anslutna toohello service, platsen för hello-tjänsten och information om hello-prenumeration som är associerad med hello-tjänst. Det finns också en länk toohello operations logg. Klicka på hello länken toosee en lista över alla slutförda StorSimple Manager-tjänståtgärder.

Du kan använda hello StorSimple Manager-tjänsten instrumentpanelen sidan tooinitiate hello följande uppgifter:

* Visa eller återskapa hello nyckel för tjänstregistrering.
* Ändra hello krypteringsnyckel för tjänstdata.
* Visa hello åtgärdsloggar.

## <a name="view-or-regenerate-hello-service-registration-key"></a>Visa eller återskapa hello nyckel för tjänstregistrering
hello nyckel för tjänstregistrering är används tooregister en Microsoft Azure StorSimple-enhet med hello StorSimple Manager-tjänsten så att hello enheten visas i hello Azure klassiska portal för ytterligare hanteringsåtgärder. hello nyckeln skapas på hello första enhet och delas med hello resten av dina enheter.

Klicka på **registreringsnyckel** (på hello längst ned på sidan för hello) öppnar du hello **nyckel för tjänstregistrering** i dialogrutan där du kan antingen kopiera hello aktuella service registrering viktiga toohello Urklipp eller Återskapa hello nyckel för tjänstregistrering.

Återskapar hello nyckel påverkar inte tidigare registrerade enheter: det påverkar endast hello-enheter som registreras med hello-tjänsten när hello nyckeln genereras.

Mer information om visning och generera hello service registrering går för[hämta hello nyckel för tjänstregistrering](storsimple-manage-service.md#get-the-service-registration-key).

## <a name="change-hello-service-data-encryption-key"></a>Ändra hello krypteringsnyckel för tjänstdata
Tjänsten datakrypteringsnycklar är används tooencrypt konfidentiell kundinformation, till exempel lagringskontouppgifter som skickas från din StorSimple Manager service toohello StorSimple-enhet. Du behöver toochange nycklarna regelbundet om organisationen har en princip för viktiga rotation på hello lagringsenheter. hello process viktiga förändringar kan skilja sig något beroende på om det finns en enstaka enhet eller flera enheter som hanteras av hello StorSimple Manager-tjänsten.

Krypteringsnyckel för tjänstdata ändra hello är en process i steg 3:

1. Använder hello klassiska Azure-portalen, auktorisera en enhet toochange hello krypteringsnyckel för tjänstdata.
2. Använder Windows PowerShell för StorSimple, initiera förändring för nyckel hello-service-kryptering.
3. Om du har mer än en StorSimple-enhet kan du uppdatera hello krypteringsnyckel för tjänstdata på hello andra enheter.

hello beskriver följande steg hello förnyelse process för hello krypteringsnyckel för tjänstdata.

[!INCLUDE [storsimple-change-data-encryption-key](../../includes/storsimple-change-data-encryption-key.md)]

## <a name="view-hello-operations-logs"></a>Visa hello operations loggfiler
Du kan visa hello åtgärdsloggar genom att klicka på hello åtgärden loggar länken i hello **snabböversikten** rutan hello instrumentpanelen. Detta tar du toohello management services-sida där du kan filtrera och se hello loggar specifika tooyour StorSimple Manager-tjänsten.

## <a name="next-steps"></a>Nästa steg
* Lär dig hur för[felsöka en StorSimple-enhet](storsimple-troubleshoot-operational-device.md).
* Läs mer om hur för[Använd hello StorSimple Manager service tooadminister StorSimple-enheten](storsimple-manager-service-administration.md).

