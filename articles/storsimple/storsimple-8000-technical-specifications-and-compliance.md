---
title: aaaStorSimple tekniska specifikationer | Microsoft Docs
description: "Beskriver hello tekniska specifikationer och standarder kompatibilitetsinformation för hello StorSimple maskinvarukomponenter."
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
ms.date: 06/02/2017
ms.author: alkohli
ms.openlocfilehash: 98fa3307e2a929551c74e8b3179bb0fb61c0ab53
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="technical-specifications-and-compliance-for-hello-storsimple-device"></a>Tekniska specifikationer och kompatibilitet för hello StorSimple-enhet

## <a name="overview"></a>Översikt

hello maskinvarukomponenter av Microsoft Azure StorSimple-enheten följa toohello tekniska specifikationer och standarder beskrivs i den här artikeln. hello tekniska specifikationer beskrivs hello ström och kylning moduler (PCMs), diskenheter, lagringskapacitet och bilagor. hello kompatibilitetsinformation omfattar bland annat internationella standarder, säkerhet och utsläpp och kablar.

## <a name="power-and-cooling-module-specifications"></a>Ström och kylning modulen specifikationer

Hej StorSimple-enhet har två 100 – 240 V dubbla fläktar, flesta SBB-kompatibel Power kylning moduler (PCMs). Detta ger en redundant strömförsörjning konfiguration. Om en PCM misslyckas fortsätter hello enheten toooperate normalt på hello andra PCM tills hello misslyckades modulen ersätts.

Hej EBOD hölje använder en 580 W PCM och primär enhet använder en 764 W PCM. hello de följande tabellerna listan hello tekniska specifikationer som är associerade med hello PCMs.

| Specifikationen | 580 W PCM (EBOD) | 764 W PCM (primär) |
| --- | --- | --- |
| Maximal utgående power |580 W |764 |
| frekvens |50/60 Hz |50/60 Hz |
| Intervallet spänning |Automatisk allt: 90 – 264 V AC, 47/63 Hz |Automatisk allt: 90-264 V AC, 47/63 Hz |
| Maximal översvämning aktuella |20 A |20 A |
| Power faktor korrigering |> 95% nominell ingångsspänning |> 95% nominell ingångsspänning |
| Övertoner |Uppfyller EN61000-3-2 |Uppfyller EN61000-3-2 |
| Resultat |5v vänteläge spänning @ 2.0 A |5v vänteläge spänning @ 2.7 A |
| + 5V @ 42 A |5V @ 40 + A | |
| + 12 V @ 38 A |+ 12 V @ 38 A | |
| Hot pluggable |Ja |Ja |
| Växlar och indikatorer |AC från växeln och fyra statusindikator indikatorer |AC från växeln och sex statusindikator indikatorer |
| Höljet kylning |Axial kylfläktar med variabel hastighet fläktkontroll |Axial kylfläktar med variabel hastighet fläktkontroll |

## <a name="power-consumption-statistics"></a>Statistik för Power förbrukning

hello följande tabell listar hello vanliga strömförbrukningsdata (faktiska värdena kan variera från hello publicerade) för hello olika modeller av StorSimple-enhet.

| Villkor | 240 V AC | 240 V AC | 240 V AC | 110 V AC | 110 V AC | 110 V AC |
| --- | --- | --- | --- | --- | --- | --- |
|  Fläktar långsam, enheter som är inaktiv |1,45 A |0.31 kW |1057.76 BTU/timme |3.19 A |0.34 kW |1160.13 BTU/timme |
|  Fläktar långsamma enheter åtkomst till |1.54 A |0.33 kW |1126.01 BTU/timme |3.27 A |0.36 kW |1228.37 BTU/timme |
|  Fläktar snabbt enheter inaktiv två PSUs påslagen |PUNKT 2.14 A |0.49 kW |1671.95 BTU/timme |4.99 A |0.54 kW |1842.56 BTU/timme |
|  Snabb, fläktar enheter inaktiv finns en PSU tillhandahålls en inaktiv |2.05 A |0.48 kW |1637.83 BTU/timme |4.58 A |0,50 kW |1706.07 BTU/timme |
|  Fläktar snabbt enheter åtkomst till två PSUs påslagen |2,26 A |0,51 kW |1740.19 BTU/timme |4.95 A |0.54 kW |1842.56 BTU/timme |
|  Snabb fläktar, enheter som ansluter till, en PSU tillhandahålls en inaktiv |PUNKT 2.14 A |0.49 kW |1671.95 BTU/timme |4.81 A |0.53 kW |1808.44 BTU/timme |

## <a name="disk-drive-specifications"></a>Specifikationer för enhet

Din StorSimple-enhet stöder upp too12 3,5-tums formuläret faktor seriellt ansluten SCSI (SAS)-enheter. hello faktiska enheter kan vara en blandning av SSD-enheter (SSD) eller hårddiskar (HDD) beroende på hello produktkonfigurationen. hello 12 diskenhet platser finns i en konfiguration 3 av 4 framför hello höljet. Hej EBOD hölje kan ytterligare lagringsutrymme för en annan 12 diskenheter. Dessa är alltid hårddiskar.

## <a name="storage-specifications"></a>Specifikationer för lagring

Hej StorSimple-enheter har en blandning av hårddiskar och SSD-enheter för både hello 8100 och 8600. hello totala användbara kapaciteten för hello 8100 och 8600 är ungefär 15 TB och 38 TB. i den följande tabellen hello dokument hello information om SSD och HDD kapacitet hello gäller hello StorSimple-lösningen kapacitet.

| Enhetsmodell / kapacitet | 8100 | 8600 |
| --- | --- | --- |
| Antalet hårddiskar (HDD) |8 |19 |
| Antal SSD-enheter |4 |5 |
| Enskild hårddiskkapacitet |4 TB |4 TB |
| Enda SSD-kapacitet |400 GB |800 GB |
| Ledig kapacitet |4 TB |4 TB |
| Användbar hårddiskkapacitet |14 TB |36 TB |
| Använda SSD-kapacitet |800 GB |2 TB |
| Totalt antal användbara kapacitet * |~ 15 TB |~ 38 TB |
| Lösning för maximal kapacitet (inklusive moln) |200 TB |500 TB |

<sup>* </sup>- *hello totala användbara kapacitet innehåller hello kapacitet för data, metadata och buffertar.*

## <a name="enclosure-dimensions-and-weight-specifications"></a>Höljet dimensioner och vikt specifikationer

följande tabeller visas hello hello olika höljet specifikationer för dimensioner och vikt.

### <a name="enclosure-dimensions"></a>Höljet dimensioner

hello visar följande tabell hello dimensioner för hello höljet i millimeter och tum.

| Höljet | Millimeter | Tum |
| --- | --- | --- |
| Höjd |87.9 |3.46 |
| Bredden över montering flänsad |483 |19.02 |
| Bredden i brödtexten för höljet |443 |17.44 |
| Djup från främre montering flänsad tooextremity av höljet text |577 |22.72 |
| Djup från operations panelen toofurthest delen av hölje |630.5 |24.82 |
| Djup från montera flänsad toofurthest delen av hölje |603 |23.74 |

### <a name="enclosure-weight"></a>Höljet vikt

Beroende på hello konfiguration ett lästs in helt primära hölje kan väga från 21 too33 kg och kräver två personer toohandle den.

| Höljet | Vikt |
| --- | --- |
| Högsta vikt (beroende på hello configuration) |30 kg – 33 kg |
| Tom (inga enheter monterade) |21 – 23 kg |

## <a name="enclosure-environment-specifications"></a>Specifikationer för höljet miljö

Det här avsnittet innehåller hello specifikationer relaterade toohello hölje miljö. hello temperatur, fuktighet, höjd, undanröjs, vibrationer, orientering, säkerhet och elektromagnetisk kompatibilitet (EMC) ingår i den här kategorin.

### <a name="temperature-and-humidity"></a>Temperatur- och fuktighetskonsekvens

| Höljet | Temperaturintervall | Den omgivande relativa fuktighet | Maximal våt termometern |
| --- | --- | --- | --- |
| Använd |5 OC - 35° C (41° F - 95° F) |20% till 80% icke-kondenserande- |28° C (82° F) |
| Kan inte användas |-40° C - 70 OC 40° F - 158° F |5-100% icke-kondenserande |29 OC (84° F) |

### <a name="airflow-altitude-shock-vibration-orientation-safety-and-emc"></a>Luftflödet, höjd, undanröjs, vibrationer, orientering, säkerhet och EMC

| Höljet | Använd specifikationer |
| --- | --- |
| Luftflöde |System luftflödet är främre toorear. Systemet måste köras med en low-pressure, bakre avgaser installation. Skapats av rack dörrar och hinder som ligger högst 5 Pascal (0,5 mätare). |
| Höjd operativa |-30 mätare too3045 mätare (-100 fot too10 000 fot) med högsta temperatur för drift Frigör klassificerats av 5 ° C ovan 7000 fot. |
| Höjd ur drift |-305 mätare too12, 192 mätare (– 1 000 fot too40 000 fot) |
| Undanröjs, operativa |5g 10 ms ½ sinus |
| Undanröjs, ur drift |30g 10 ms ½ sinus |
| Vibration vid drift |0.21g RMS 5 500 Hz slumpmässiga |
| Vibration, ur drift |1.04g RMS 2 200 Hz slumpmässiga |
| Vibration dataflytt |3g 2 200 Hz sinus |
| Orientering och montering |19-tums rackmonterad (2 EIA enheter) |
| Rack spår |toofit minsta 700 mm (31.50 tum) djup rack följer IEC 297 |
| Säkerhet och godkännanden |CE och UL SV 61000-3, IEC 61000-3, UL 61000-3 |
| EMC |EN55022 (CISPR - A), FCC A |

## <a name="international-standards-compliance"></a>Internationella standarder kompatibilitet

Microsoft Azure StorSimple-enheten uppfyller hello följer internationella standarder:  

* CE - EN 60950-1
* CB rapporten tooIEC 60950-1
* UL och cUL tooUL 60950-1

## <a name="safety-compliance"></a>Säkerhet

Microsoft Azure StorSimple-enheten uppfyller hello följande säkerhet klassificeringar:

* System produkten typen godkännande: UL, cUL CE
* Säkerhet: UL 60950 IEC 60950, SV 60950

## <a name="emc-compliance"></a>EMC-kompatibilitet

Microsoft Azure StorSimple-enheten uppfyller hello efter EMC klassificeringar.

### <a name="emissions"></a>Utsläpp

hello-enheten är EMC-kompatibel för utförts och UTSTRÅLAD nivåer.

* Utförts utsläpp begränsa nivåer: CFR 47 Part 15B klass A EN55022 klass A CISPR klass A
* UTSTRÅLAD utsläpp begränsa nivåer: CFR 47 Part 15B klass A EN55022 klass A CISPR klass A

### <a name="harmonics-and-flicker"></a>Övertoner och flimmer

hello enheten överensstämmer med EN61000-3-2/3.

### <a name="immunity-limit-levels"></a>Gränsen för immunitetsnivåerna

hello enheten överensstämmer med EN55024.

## <a name="ac-power-cord-compliance"></a>AC power kabel kompatibilitet

hello plug och hello fullständig power kabel sammansättningen måste uppfylla hello standarder för hello land i vilka hello används och de måste ha säkerheten godkännanden som accepteras i det landet. hello följande tabeller listan standarder för hello USA och Europa.

### <a name="ac-power-cords---usa-must-be-nrtl-listed"></a>Strömkablar AC - USA (måste vara NRTL anges)

| Komponent | Specifikationen |
| --- | --- |
| Kabel typ |SA eller SVT 18 AWG minimum, 3 ledare 2.0 mätare maximal längd |
| Plug |NEMA 5-15P erfarenhet av att arbeta typen bilaga plug klassificerade 120 V, 10 A; eller IEC 320 C14 250 V, 10 A |
| Socket |IEC 320 C-13, 250 V, 10 A |

### <a name="ac-power-cords---europe"></a>Strömkablar AC - Europa

| Komponent | Specifikationen |
| --- | --- |
| Kabel typ |Harmoniserade H05-VVF-3G1.0 |
| Socket |IEC 320 C-13, 250 V, 10 A |

## <a name="supported-network-cables"></a>Stöds nätverkskablarna

Hello 10 GbE-nätverkskort, DATA 2 och DATA 3 finns toohello [lista över stöds nätverkskablarna och moduler](storsimple-supported-hardware-for-10-gbe-network-interfaces.md).

## <a name="next-steps"></a>Nästa steg

Du är nu redo toodeploy en StorSimple-enhet i ditt datacenter. Mer information finns i [distribuera din lokala enhet](storsimple-8000-deployment-walkthrough-u2.md).

