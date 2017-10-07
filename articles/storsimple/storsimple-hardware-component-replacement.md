---
title: "ersättning av aaaStorSimple maskinvara komponenten | Microsoft Docs"
description: "Beskriver hur toosafely ersätta hello PCMs, batteri, domänkontrollant moduler, EBOD domänkontrollanter, diskenheter och chassi av StorSimple-enhet."
services: storsimple
documentationcenter: 
author: alkohli
manager: timlt
editor: 
ms.assetid: e8087ba7-0b66-4f59-8988-e53aad52ee21
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 02/27/2017
ms.author: alkohli
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 472d9dc1c31b61550fe079cc9b9419510487db3d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="replace-a-hardware-component-on-your-storsimple-8000-series-device"></a>Ersätta en maskinvarukomponent på enheten StorSimple 8000-serien

## <a name="overview"></a>Översikt
hello maskinvara komponenten ersättning självstudier beskriver hello maskinvarukomponenter för din Microsoft Azure StorSimple 8000-serien enheten och hello steg nödvändiga tooremove och ersätta dem. Den här artikeln beskriver hello säkerhet ikoner, pekare toohello detaljerade självstudier och visar hello komponenter som är utbytbara.

> [!IMPORTANT]
> Innan du försöker tooremove eller ersätta en StorSimple-komponent, se till att du granskar hello [säkerhet ikonen konventioner](#safety-icon-conventions) och andra [säkerhetsåtgärderna](storsimple-safety.md).
> 
> 

### <a name="safety-icon-conventions"></a>Ikonen konventioner för säkerhet
hello beskrivs följande tabell hello säkerhet ikoner som används i de här kurserna. Noggrann toothese säkerhet ikoner när du går igenom hello steg tooremove och Ersätt enhetskomponenter.

| Ikon | Text | Ytterligare information |
|:--- |:--- |:--- |
| ![Varningsikon](./media/storsimple-hardware-component-replacement/Warning.png) |**RISK!** |Visar en farliga situation som, om inte undvikas resulterar i död eller allvarliga skador. Signal ordet är begränsad toohello extrema fall. |
| ![Varningsikon](./media/storsimple-hardware-component-replacement/Warning.png) |**VARNING!** |Visar en farliga situation som, om inte undvikas, kan leda till död eller allvarliga skador. |
| ![Varningsikon](./media/storsimple-hardware-component-replacement/Caution.png) |**VARNING!** |Visar en farliga situation som, om inte undvikas, kan leda till lägre eller måttlig skada. |
| ![Meddelande-ikon](./media/storsimple-hardware-component-replacement/NoticeIcon.png) |**MEDDELANDE:** |Anger information som anses vara viktiga, men inte risker relaterade. |
| ![Elektrisk undanröjs ikon](./media/storsimple-hardware-component-replacement/Electric.png) |**Elektrisk undanröjs risk** |Anger hög spänning. |
| ![Ikon för tunga vikt](./media/storsimple-hardware-component-replacement/Weight.png) |**Tunga vikt** | |
| ![Ingen användare användbar delar ikon](./media/storsimple-hardware-component-replacement/NoUserServiceableParts.png) |**Inga användare användbar delar** |Inte kommer åt såvida inte korrekt utbildade. |
| ![Läs anvisningarna ikon](./media/storsimple-hardware-component-replacement/ReadInstructions.png) |**Läsa alla instruktioner först** | |
| ![Risken tipsikon](./media/storsimple-hardware-component-replacement/TipHazard.png) |**Tips risk** | |

### <a name="before-you-begin"></a>Innan du börjar
Bekanta dig med hello säkerhetsinformation om din enhets- och ikoner som används i den här kursen. Gå för[på ett säkert sätt installera och använda din StorSimple-enhet](storsimple-safety.md) fullständig information. Vara säker på att tooreview hello [säkerhetsåtgärderna](storsimple-safety.md#handling-precautions) innan du hanterar din StorSimple-enhet. 

Överväg att hello följande information innan du försöker tooreplace en komponent.

![Varningsikon](./media/storsimple-hardware-component-replacement/Warning.png) ![elektrisk undanröjs ikonen](./media/storsimple-hardware-component-replacement/Electric.png) **varning!** 

* Bakgrund själv korrekt genom att använda en elektrostatisk utsläpp eller antistatiska mat vid hantering av moduler och komponenter i din StorSimple-enhet.
* Touch inte någon kretsar. Använda guider och hello angivna hanteras när komponenter som kan ha exponerade kretsar.

![Varningsikon](./media/storsimple-hardware-component-replacement/Warning.png) ![notera ikonen](./media/storsimple-hardware-component-replacement/NoticeIcon.png) **meddelande:**

När du ersätter en modul **lämnar aldrig ett tomt fack hello baksidan hello höljet**. Hämta en ersättare eller ett tomt modulen innan du tar bort hello delar.

## <a name="hardware-component-replacement-procedures"></a>Procedurer för maskinvara komponenten ersättning
Enheten StorSimple 8000-serien består av flera plugin-program i hello primära och/eller EBOD höljen. hello 8100 har en enda primär enhet hello 8600 är en dubbel hölje-enhet med en primär enhet och en EBOD bilaga.

hello huvudsakliga maskinvarukomponenter i enheten sammanfattas i följande tabeller hello. Klicka på länken hello i hello **ersättning proceduren** kolumnen toogo toohello associerade kursen.

| Komponenter | # Finns | Plugin-modul? | Ersättning proceduren |
|:--- |:--- |:--- |:--- |
| Chassi |1 |Nej |[Ersätt hello chassi på din StorSimple-enhet](storsimple-chassis-replacement.md) |
| Primära domänkontrollanter |2 |Ja |[Ersätta en domänkontrollant modul på din StorSimple-enhet](storsimple-controller-replacement.md) |
| 764W ström och kylning moduler (PCMs) |2 |Ja |[Ersätta en ström och kylning modulen på din StorSimple-enhet](storsimple-power-cooling-module-replacement.md) |
| Säkerhetskopiering batteri |2 |Ja |[Ersätt hello säkerhetskopiering batteri modul på din StorSimple-enhet](storsimple-battery-replacement.md) |
| Diskenheter |12 |Ja |[Ersätta en diskenhet på din StorSimple-enhet](storsimple-disk-drive-replacement.md) |

**Tabell 1** maskinvarukomponenter i hello primära hölje

hello primära höljet och hello EBOD hölje skiljer sig åt i sina i/o-moduler. Dessutom har hello PCMs olika effekt. Hej PCMs i hello primära hölje 764 W, medan de i hello EBOD hölje är 580 W. hello PCMs i hello primära höljet också innehålla en reserv batteri-modul.

| Komponenter | # Finns | Plugin-modul? | Ersättning proceduren |
|:--- |:--- |:--- |:--- |
| Chassi |1 |Nej |[Ersätt hello chassi på din StorSimple-enhet](storsimple-chassis-replacement.md) |
| EBOD domänkontrollanter |2 |Ja |[Ersätta en domänkontrollant för en EBOD på din StorSimple-enhet](storsimple-ebod-controller-replacement.md) |
| 580 w vid ström och kylning moduler (PCMs) |2 |Ja |[Ersätta en ström och kylning modulen på din StorSimple-enhet](storsimple-power-cooling-module-replacement.md) |
| Diskenheter |12 |Ja |[Ersätta en diskenhet på din StorSimple-enhet](storsimple-disk-drive-replacement.md) |

**Tabell 2** maskinvarukomponenter i hello EBOD hölje

hello plugin-program på hello enhet markeras i hello följande främre och bakre diagram. Du kan använda dessa diagram toodetermine hello platsen för hello olika plugin-program om det krävs en ersättning. hello främre diagram visar hello diskenheter och hello bakre diagram över hello EBOD höljet och hello primära höljet visar hello plugin-program.

![Frontplane av enheten med diskenheter](./media/storsimple-hardware-component-replacement/IC741028.png)

**Bild 1** framför hello-enhet

| Etikett | Beskrivning |
|:--- |:--- |
| 0 - 11 |Diskenheter (totalt 12) |

Både primära hello-höljet och hello EBOD hölje har enheten operatör moduler. hello chassi inrymmer tolv 3,5-tums diskenheter ordnade i formatet 3 av 4.

![Bakplan av primära Höljesmoduler](./media/storsimple-hardware-component-replacement/IC740994.png)

**Bild 2** baksidan hello primära enhet

| Etikett | Beskrivning |
|:--- |:--- |
| 1 |PCM 0 |
| 2 |PCM 1 |
| 3 |Styrenhet 0 |
| 4 |Kontrollant 1 |

![Bakplan av EBOD hölje plugin-moduler](./media/storsimple-hardware-component-replacement/IC769599.png)

**Bild 3** baksidan hello EBOD hölje

| Etikett | Beskrivning |
|:--- |:--- |
| 1 |PCM 0 |
| 2 |PCM 1 |
| 3 |EBOD styrenhet 0 |
| 4 |EBOD kontrollant 1 |

## <a name="field-replaceable-units"></a>Fältet utbytbara enheter
hello efter fältet utbytbara enheter (FRU) är tillgängliga för din StorSimple-enhet:

* Chassi (inklusive hello integrerade åtgärder panelen)
* 764 W AC PCM
* 580 W AC PCM
* Hårddisk med enheten operatör modul
* Modul för domänkontrollant
* EBOD domänkontrollant modul
* Modul för säkerhetskopiering batteri
* Rackmonterade tåg kit

Kontrollera [kontaktar Microsoft Support](storsimple-contact-microsoft-support.md) tooorder någon av dessa enheter ersättning.

## <a name="next-steps"></a>Nästa steg
Granska alla [säkerhetsinformation](storsimple-safety.md) innan du försöker tooreplace en StorSimple-maskinvarukomponent.

