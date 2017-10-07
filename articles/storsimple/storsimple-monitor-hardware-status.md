---
title: aaaStorSimple status och maskinvarukomponenter | Microsoft Docs
description: "Lär dig hur toomonitor hello maskinvarukomponenter av StorSimple-enheten via hello StorSimple Manager-tjänsten."
services: storsimple
documentationcenter: 
author: alkohli
manager: carmonm
editor: 
ms.assetid: 0d56a2ba-daf0-45ad-9610-8b8712dd5750
ms.service: storsimple
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/18/2016
ms.author: alkohli
ms.openlocfilehash: 12a62c0ffc4a5b5e72a25e9e1d976009be03c598
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="use-hello-storsimple-manager-service-toomonitor-hardware-components-and-status"></a>Använd hello StorSimple Manager-tjänsten toomonitor maskinvarukomponenter och status
## <a name="overview"></a>Översikt
Den här artikeln beskriver hello olika fysiska och logiska komponenter i din lokala StorSimple-enhet. Här förklaras också hur toomonitor hello Komponentstatus för enheten med hjälp av hello **Underhåll** sida i hello StorSimple Manager-tjänsten. 

Hej **Underhåll** sidan visar hello maskinvarustatus för alla komponenter i hello StorSimple-enhet. 

Det finns tre avsnitt som beskriver hello listan med komponenter för 8100:

* **Delade komponenter** – dessa inte är del av hello domänkontrollanter som diskenheter, höljet, PCM komponenter och PCM temperatur rad spänning och rad aktuella sensorer.
* **Komponenter för styrenhet 0** – hello-komponenter som finns på en domänkontrollant 0, till exempel domänkontrollanter, SAS expander och koppling, domänkontrollant temperatursensorer, och hello olika nätverksgränssnitt.
* **Kontrollant 1 komponenter** – hello komponenter som utgör Controller 1, liknande toothose detaljerad för styrenhet 0.

En 8600-enhet har ytterligare komponenter som motsvarar toohello hölje utökad Bunch av diskar (EBOD). Det finns fem avsnitt under hello listan med komponenter. Det finns tre avsnitt som innehåller hello komponenter i hello primära hölje och är identiska toohello som beskrivs för 8100 av dessa. Det finns två ytterligare avsnitt för hello EBOD hölje som beskriver:

* **EBOD hölje delade komponenter** – hello komponenter finns i hello EBOD höljet och PCM som inte är en del av hello EBOD domänkontrollant.
* **EBOD styrenhet 0 komponenter** – hello komponenter som finns på EBOD hölje 0, till exempel hello EBOD styrenhet, SAS-expander och koppling och domänkontrollant temperatursensorer.
* **EBOD domänkontrollant 1 komponenter** – hello komponenter som utgör EBOD hölje 1, liknande toothose detaljerad för EBOD hölje 0.

> [!NOTE]
> **hello maskinvara i avsnittet status finns inte i hello Underhåll sida för en virtuell StorSimple-enhet (1100).**
> 
> 

## <a name="monitor-hello-hardware-status"></a>Övervaka hello maskinvarustatus
Utför hello följande steg tooview hello maskinvarustatus för en enhetskomponent:

1. Navigera för**enheter**, Välj en specifik StorSimple-enhet. Toogo hello enhetsnivå menyn och klicka sedan på **Underhåll**. 
2. Leta upp hello **maskinvarustatus** avsnittet och välja bland tillgängliga hello-komponenter (som beskrivs ovan). Klicka bara på en pil som föregår hello etikett tooexpand hello listan och visa hello Komponentstatus av hello olika komponenter. Se hello [detaljerad komponentlista för hello primära höljet](#component-list-for-primary-enclosure-of-storsimple-device) och hello [detaljerad komponentlista för hello EBOD hölje](#component-list-for-ebod-enclosure-of-storsimple-device).
3. Använd hello efter färg kodning schemat toointerpret hello Komponentstatus:
   
   * **Grön Kontrollera** – Denotes en **felfri** eller **OK** komponent.
   * **Gult** – anger en komponent i **varning** tillstånd.
   * **Rött utropstecken** – Denotes en komponent som har en **fel** eller **behöver kontrolleras** status.
   * **Vit med svart text** – anger en komponent som inte finns.
4. Om du stöter på en komponent som inte är i en **felfri** tillstånd, kontakta Microsoft Support. Om aviseringar ska aktiveras på enheten, får du en e-postavisering. Om du behöver tooreplace misslyckade maskinvarukomponent finns [ersättning av StorSimple maskinvara komponenten](storsimple-hardware-component-replacement.md).

## <a name="component-list-for-primary-enclosure-of-storsimple-device"></a>Lista över komponenten för primära höljet av StorSimple-enhet
hello hello följande tabell beskrivs fysiska och logiska komponenter som ingår i hello primära inneslutning av din lokala StorSimple-enhet.

| Komponent | Modul | Typ | Plats | Fältet utbytbara enhet (FRU)? | Beskrivning |
| --- | --- | --- | --- | --- | --- |
| Enheten på platsen [0-11] |Diskenheter |Fysiska |Delad |Ja |Visas en rad för varje hello SSD eller hello HDD-enheter i hello primära hölje. |
| Omgivande-temperatursensor |Höljet |Fysiska |Delad |Nej |Mått hello temperatur inom hello chassi. |
| Halva plan-temperatursensor |Höljet |Fysiska |Delad |Nej |Mått hello temperatur hello halva plan. |
| Akustiskt larm |Höljet |Fysiska |Delad |Nej |Anger om hello akustiskt larm undersystemet i hello chassi fungerar. |
| Höljet |Höljet |Fysiska |Delad |Ja |Anger hello förekomsten av ett chassi. |
| Inställningar för höljet |Höljet |Fysiska |Delad |Nej |Refererar toohello frontpanel hello chassi. |
| Raden spänningssensorer |PCM |Fysiska |Delad |Nej |Flera rad spänningssensorer har det tillståndet som visas som anger om hello mäts spänning ligger inom Toleransvärdena. |
| Raden aktuella sensorer |PCM |Fysiska |Delad |Nej |Ett stort antal rad aktuella sensorer ha deras status visas som anger om hello uppmätta aktuella ligger inom Toleransvärdena. |
| Temperatursensorer i PCM |PCM |Fysiska |Delad |Nej |Ett stort antal temperatursensorer som in- och Hotspot sensorer har det tillståndet som visas som anger om hello mäts temperatur ligger inom Toleransvärdena. |
| Strömförsörjning [0-1] |PCM |Fysiska |Delad |Ja |En rad visas för varje hello strömkällor i hello två PCMs finns i hello baksidan hello enhet. |
| Kylning [0-1] |PCM |Fysiska |Delad |Ja |En rad visas för varje hello fyra kylfläktar som finns i hello två PCMs. |
| Batteri [0-1] |PCM |Fysiska |Delad |Ja |En rad visas för varje hello säkerhetskopiering batteri moduler som har satts i hello PCM. |
| Metis |Saknas |Logiska |Delad |Saknas |Visar hello tillståndet för hello batterierna: om de behöver debitering och närmar sig slutet av livslängd. |
| Kluster |Saknas |Logiska |Delad |Saknas |Visar hello tillståndet för hello kluster som skapas mellan hello två integrerad styrenhet moduler. |
| Klusternod |Saknas |Logiska |Delad |Saknas |Visar hello hello domänkontrollant som en del av hello klustret. |
| Klusterkvorum |Saknas |Logiska | |Saknas |Anger hello förekomst av hello majoritet disk medlemskap i hello HDD-lagringspoolen. |
| Hårddisk datautrymme |Saknas |Logiska |Delad |Saknas |hello lagringsutrymme som används för data i lagringspoolen för hello hårddisken (HDD). |
| Hårddisk hantering av utrymme |Saknas |Logiska |Delad |Saknas |hello-utrymme är reserverad i hello HDD lagringspoolen för hanteringsuppgifter. |
| Hårddisk kvorum utrymme |Saknas |Logiska |Delad |Saknas |hello-utrymme är reserverad i hello HDD lagringspoolen för klustrets kvorum. |
| Hårddisk ersättning utrymme |Saknas |Logiska |Delad |Saknas |hello utrymme är reserverad i hello HDD lagringspoolen för ersättning av domänkontrollanten. |
| SSD datautrymme |Saknas |Logiska |Delad |Saknas |hello lagringsutrymme som används för data i lagringspoolen för hello Solid-State-hårddisk (SSD). |
| SSD NVRAM utrymme |Saknas |Logiska |Delad |Saknas |hello lagringsutrymme i hello SSD lagringspoolen som är dedikerad för NVRAM logik. |
| HDD-lagringspoolen |Saknas |Logiska |Delad |Saknas |Visar hello tillståndet för hello logiska lagringspoolen som skapas från enhet hårddiskar. |
| SSD-lagringspoolen |Saknas |Logiska |Delad |Saknas |Visar hello tillståndet för hello logiska lagringspoolen som skapas från SSD-enhet. |
| Domänkontrollant [0-1] [tillstånd] |I/O |Fysiska |Domänkontrollant |Ja |Visar hello tillståndet för hello styrenhet, och om den är aktiv eller i vänteläge läge i hello-chassi. |
| Temperatursensorer i controller |I/O |Fysiska |Domänkontrollant |Nej |Ett stort antal temperatursensorer som i/o-modul, CPU-temperatur, DIMM och PCIe sensorer har det tillståndet som visas som anger huruvida hello temperatur påträffade ligger inom Toleransvärdena. |
| SAS expander |I/O |Fysiska |Domänkontrollant |Nej |Visar hello hello seriellt anslutna SCSI (SAS) expander, som används tooconnect hello inbyggd toohello lagringsstyrenhet. |
| SAS-koppling [0-1] |I/O |Fysiska |Domänkontrollant |Nej |Visar hello varje SAS-koppling som används tooconnect integrerad lagring toohello SAS expander. |
| Flesta SBB halva plan sammankoppling |I/O |Fysiska |Domänkontrollant |Nej |Visar hello hello halva plan connector, som har använt tooconnect varje domänkontrollant toohello halva plan. |
| Processorkärna |I/O |Fysiska |Domänkontrollant |Nej |Visar hello hello processorkärnor inom varje styrenhet. |
| Höljet electronics power |I/O |Fysiska |Domänkontrollant |Nej |Visar hello hello power system som används av hello höljet. |
| Höljet electronics diagnostik |I/O |Fysiska |Domänkontrollant |Nej |Visar hello hello diagnostik delsystem som tillhandahålls av hello-styrenhet. |
| Hanteringsstyrenhet för baskort (BMC) |I/O |Fysiska |Domänkontrollant |Nej |Visar hello hello hanteringsstyrenhet för baskort (BMC), vilket är en specialiserad tjänst processor som övervakar hello maskinvaruenhet via sensorer och kommunicerar med hello systemadministratören via en oberoende anslutning. |
| Ethernet |I/O |Fysiska |Domänkontrollant |Nej |Visar hello varje hello nätverksgränssnitt, det vill säga hello hanterings- och dataportar på hello-styrenhet. |
| NVRAM |I/O |Fysiska |Domänkontrollant |Nej |Visar hello NVRAM, ett beständigt minne säkerhetskopierats av hello batteri som fungerar tooretain program-kritisk information i hello-händelse strömavbrott. |

## <a name="component-list-for-ebod-enclosure-of-storsimple-device"></a>Lista över komponenten för EBOD hölje av StorSimple-enhet
hello hello följande tabell beskrivs fysiska och logiska komponenter som ingår i hello EBOD inneslutning av din lokala StorSimple-enhet.

| Komponent | Modul | Typ | Plats | FRU? | Beskrivning |
| --- | --- | --- | --- | --- | --- |
| Enheten på platsen [0-11] |Diskenheter |Fysiska |Delad |Ja |En rad visas för varje hello HDD-enheter i hello framför hello EBOD hölje. |
| Omgivande-temperatursensor |Höljet |Fysiska |Delad |Nej |Mått hello temperatur inom hello chassi. |
| Halva plan-temperatursensor |Höljet |Fysiska |Delad |Nej |Mått hello temperatur hello halva plan. |
| Akustiskt larm |Höljet |Fysiska |Delad |Nej |Anger om hello akustiskt larm undersystemet i hello chassi fungerar. |
| Höljet |Höljet |Fysiska |Delad |Ja |Anger hello förekomsten av ett chassi. |
| Inställningar för höljet |Höljet |Fysiska |Delad |Nej |Refererar toohello OPS eller hello frontpanel hello chassi. |
| Raden spänningssensorer |PCM |Fysiska |Delad |Nej |Flera rad spänningssensorer har det tillståndet som visas som anger om hello mäts spänning ligger inom Toleransvärdena. |
| Raden aktuella sensorer |PCM |Fysiska |Delad |Nej |Ett stort antal rad aktuella sensorer ha deras status visas som anger om hello uppmätta aktuella ligger inom Toleransvärdena. |
| Temperatursensorer i PCM |PCM |Fysiska |Delad |Nej |Ett stort antal temperatursensorer som in- och Hotspot sensorer har det tillståndet som visas som anger om hello mäts temperatur ligger inom Toleransvärdena. |
| Strömförsörjning [0-1] |PCM |Fysiska |Delad |Ja |En rad visas för varje hello strömkällor i hello två PCMs finns i hello baksidan hello enhet. |
| Kylning [0-1] |PCM |Fysiska |Delad |Ja |En rad visas för varje hello fyra kylfläktar som finns i hello två PCMs. |
| Lokal lagring [HDD] |Saknas |Logiska |Delad |Saknas |Visar hello tillståndet för hello logiska lagringspoolen som skapas från enhet hårddiskar. |
| Domänkontrollant [0-1] [tillstånd] |I/O |Fysiska |Domänkontrollant |Ja |Visar hello tillståndet för hello domänkontrollanter i hello EBOD modul. |
| Temperatursensorer i EBOD |I/O |Fysiska |Domänkontrollant |Nej |Ett stort antal temperatursensorer från varje domänkontrollant har det tillståndet som visas som anger om hello temperatur påträffade ligger inom Toleransvärdena. |
| SAS expander |I/O |Fysiska |Domänkontrollant |Nej |Visar hello hello SAS expander som används tooconnect hello inbyggd toohello lagringsstyrenhet. |
| SAS-koppling [0-2] |I/O |Fysiska |Domänkontrollant |Nej |Visar hello varje SAS-koppling som används tooconnect integrerad lagring toohello SAS expander. |
| Flesta SBB halva plan sammankoppling |I/O |Fysiska |Domänkontrollant |Nej |Visar hello hello halva plan connector, som har använt tooconnect varje domänkontrollant toohello halva plan. |
| Höljet electronics power |I/O |Fysiska |Domänkontrollant |Nej |Visar hello hello power system som används av hello höljet. |
| Höljet electronics diagnostik |I/O |Fysiska |Domänkontrollant |Nej |Visar hello hello diagnostik delsystem som tillhandahålls av hello-styrenhet. |
| Anslutningen toodevice domänkontrollant |I/O |Fysiska |Domänkontrollant |Nej |Visar hello hello anslutningen mellan hello EBOD i/o-modulen och hello enhetskontroll. |

## <a name="next-steps"></a>Nästa steg
* toouse hello StorSimple Manager service tooadminister enheten gå för[Använd hello StorSimple Manager service tooadminister StorSimple-enheten](storsimple-manager-service-administration.md).
* Om du behöver tootroubleshoot en enhetskomponent som har status degraderad eller inte fungerar, se för [StorSimple övervakning indikatorer](storsimple-monitoring-indicators.md). 
* tooreplace misslyckade maskinvarukomponent finns [ersättning av StorSimple maskinvara komponenten](storsimple-hardware-component-replacement.md).
* Om du fortsätter tooexperience enhetsproblem [kontaktar Microsoft Support](storsimple-contact-microsoft-support.md).

