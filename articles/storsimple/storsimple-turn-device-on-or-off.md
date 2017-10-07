---
title: aaaTurn enheten StorSimple 8000-serien eller inaktivera | Microsoft Docs
description: "Förklarar hur tooturn på en ny StorSimple-enhet, aktivera en enhet som har stängts av eller förlorat power och stänga av en enhet som körs."
services: storsimple
documentationcenter: 
author: alkohli
manager: timlt
editor: 
ms.assetid: 8e9c6e6c-965c-4a81-81bd-e1c523a14c82
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 06/29/2017
ms.author: alkohli
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 85434bde9377e330cd6ba4797fd5fd68bcee944d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="turn-on-or-turn-off-your-storsimple-8000-series-device"></a>Aktivera eller inaktivera enheten StorSimple 8000-serien
## <a name="overview"></a>Översikt
Det krävs inte att stänga av en Microsoft Azure StorSimple-enhet som en del av normal drift. Du kan dock måste tooturn på en ny enhet eller en enhet som hade toobe stänga av. I allmänhet krävs en avstängning i fall där du måste ersätta misslyckade maskinvara, fysiskt flytta en enhet eller ta en enhet ur funktion. Den här självstudiekursen beskriver hello krävs proceduren för att aktivera och stänga av din StorSimple-enhet i olika scenarier.

## <a name="turn-on-a-new-device"></a>Aktivera en ny enhet
hello varierar steg för att aktivera en StorSimple-enhet för hello första gången beroende på om hello enheten är en 8100 eller en 8600-modell. hello 8100 har en enda primär enhet hello 8600 är en dubbla hölje-enhet med en primär enhet och en EBOD bilaga. hello beskrivs detaljerade anvisningar för båda modellerna i följande avsnitt hello.

* [Ny enhet med primär enhet](#new-device-with-primary-enclosure-only)
* [Ny enhet med EBOD hölje](#new-device-with-ebod-enclosure)

### <a name="new-device-with-primary-enclosure-only"></a>Ny enhet med primär enhet
Hej StorSimple 8100 modellen är en enda enhet-enhet. Enheten innehåller redundant ström och kylning moduler (PCMs). Både PCMs måste vara installerad och anslutna toodifferent power källor tooensure hög tillgänglighet.

Utför följande steg toocable hello enheten för power.

[!INCLUDE [storsimple-cable-8100-for-power](../../includes/storsimple-cable-8100-for-power.md)]

> [!NOTE]
> Slutför enhetskonfiguration och kablar instruktioner finns för[installerar StorSimple 8100-enhet](storsimple-8100-hardware-installation.md). Kontrollera att du följer anvisningarna hello exakt.
> 
> 

### <a name="new-device-with-ebod-enclosure"></a>Ny enhet med EBOD hölje
Hej StorSimple 8600-modellen har både en primär enhet och en EBOD bilaga. Detta kräver hello enheter toobe kabelanslutna tillsammans för seriellt ansluten SCSI (SAS)-anslutning och ström.

När du konfigurerar enheten för hello första gången, gör hello SAS-kablar först och sedan utför hello anvisningar för kablar.

[!INCLUDE [storsimple-sas-cable-8600](../../includes/storsimple-sas-cable-8600.md)]

[!INCLUDE [storsimple-cable-8600-for-power](../../includes/storsimple-cable-8600-for-power.md)]

> [!NOTE]
> Slutför enhetskonfiguration och kablar instruktioner finns för[installera din StorSimple-8600-enhet](storsimple-8600-hardware-installation.md). Kontrollera att du följer anvisningarna hello exakt.

## <a name="turn-on-a-device-after-shutdown"></a>Aktivera en enhet efter avstängning
hello stegen för att aktivera en StorSimple-enhet när den har avslutats är olika beroende på om hello enheten är en 8100 eller en 8600-modell. hello 8100 har en enda primär enhet hello 8600 är en dubbla hölje-enhet med en primär enhet och en EBOD bilaga.

* [Enhet med primär enhet](#device-with-primary-enclosure-only)
* [Enhet med EBOD hölje](#device-with-ebod-enclosure)

### <a name="device-with-primary-enclosure-only"></a>Enhet med primär enhet
Använd hello följa proceduren tooturn på en virtuell StorSimple-enhet med en primär enhet och inga EBOD hölje efter en avstängning.

#### <a name="tooturn-on-a-device-with-a-primary-enclosure-only"></a>tooturn på en enhet med en primär enhet
1. Kontrollera att hello power växlar på båda ström och kylning moduler (PCMs) är i hello OFF-läge. Om hello växlar inte hello OFF-läge, sedan Vänd dem toohello OFF-läge och vänta tills hello ljus toogo ut.
2. Aktivera hello enhet genom att vända hello strömbrytare på båda PCMs toohello ON position. hello enheten slås på.
3. Kontrollera hello följande tooverify som hello enheten är helt på:
   
   1. hello OK indikatorer på båda PCM-modulerna är gröna.
   2. hello status indikatorer på båda domänkontrollanter är fast grönt.
   3. hello blå Indikator på en av domänkontrollanterna hello blinkar, vilket indikerar att hello domänkontrollant är aktiv.
      
      Enheten är inte hälsosam om något av dessa villkor inte uppfylls. Kontrollera [kontaktar Microsoft Support](storsimple-8000-contact-microsoft-support.md).

### <a name="device-with-ebod-enclosure"></a>Enhet med EBOD hölje
Använd hello följa proceduren tooturn på en virtuell StorSimple-enhet med en primär enhet och en EBOD bilaga efter en avstängning. Utför exakt så som det beskrivs varje steg i sekvensen. Fel toodo kan så resultera i förlust av data.

#### <a name="tooturn-on-a-device-with-a-primary-and-an-ebod-enclosure"></a>tooturn på en enhet med en primär och en EBOD bilaga
1. Se till att hello EBOD hölje är anslutna toohello primära höljet. Mer information finns i [installera din StorSimple-8600-enhet](storsimple-8600-hardware-installation.md).
2. Kontrollera att hello ström och kylning moduler (PCMs) på både hello EBOD och primära höljen finns i hello OFF-läge. Om hello växlar inte hello OFF-läge, sedan Vänd dem toohello OFF-läge och vänta tills hello ljus toogo ut.
3. Aktivera hello EBOD hölje första genom att vända hello strömbrytare på båda PCMs toohello ON position. hello PCM indikatorer vara grön. En grön EBOD-styrenhet Indikator på denna enhet anger att hello EBOD hölje är på.
4. Aktivera hello primära höljet genom att vända hello strömbrytare på båda PCMs toohello ON position. Nu bör hello hela systemet på.
5. Kontrollera att hello SAS indikatorer är grön, vilket säkerställer att hello anslutningen mellan hello EBOD höljet och hello primära hölje är bra.

## <a name="turn-on-a-device-after-a-power-loss"></a>Aktivera en enhet efter strömavbrott
Ett strömavbrott eller avbrott kan stänga av en StorSimple-enhet. hello strömavbrott kan bero på något av hello strömkällor eller båda strömkällor. steg för återställning av hello är olika beroende på om hello enheten är en 8100 eller en 8600-modell. hello 8100 har en enda primär enhet hello 8600 är en dubbla hölje-enhet med en primär enhet och en EBOD bilaga. Det här avsnittet beskrivs hello recovery proceduren för varje scenario.

* [Enhet med primär enhet](#8100)
* [Enhet med EBOD hölje](#8600)

### <a name="device-with-primary-enclosure-only-a-name8100"></a>Enhet med primär enhet<a name="8100">
hello system kan fortsätta dess normal drift om power förlust tooone av sin strömförsörjning. Dock tooensure hög tillgänglighet för hello enheten, återställa power toohello strömförsörjning så snart som möjligt.

Om det finns ett strömavbrott eller strömavbrott på båda strömkällor, stängs hello system på ett korrekt och kontrollerat sätt. När hello strömmen, ska hello system aktivera automatiskt.

### <a name="device-with-ebod-enclosure-a-name8600"></a>Enhet med EBOD hölje<a name="8600">
#### <a name="power-loss-on-one-power-supply"></a>Ange ett strömavbrott på en ström
hello system kan fortsätta dess normal drift om power förlust tooone av sin strömförsörjning på hello primära hölje eller hello EBOD hölje. Dock tooensure hög tillgänglighet för hello enhet, Återställ toohello power-strömförsörjning så snart som möjligt.

#### <a name="power-loss-on-both-power-supplies-on-primary-and-ebod-enclosures"></a>Strömavbrott på båda strömkällor på primära och EBOD bilagor
Om det finns ett avbrott eller power strömavbrott på båda strömkällor, hello EBOD hölje stängs omedelbart och hello primära hölje stängs på ett korrekt och kontrollerat sätt. När strömmen, startar hello installation automatiskt.

Om hello power är avstängd manuellt tar du hello följande steg toorestore power toohello system.

1. Aktivera hello EBOD hölje.
2. När hello EBOD hölje finns på, aktivera hello primära enhet.

### <a name="power-loss-on-both-power-supplies-on-ebod-enclosure"></a>Strömavbrott på båda strömkällor på EBOD hölje
När du ställer in kablarna måste du se till att hello EBOD är aldrig anslutna enbart tooa separata PDU. Om misslyckas vid hello hello EBOD och primära höljet samtidigt hello system återställs.

Om endast hello EBOD hölje misslyckas på båda strömkällor, hello system kommer inte automatiskt att återställa. Ta hello följa steg tooturn på hello system och återställer den tooa felfritt tillstånd:

1. Om hello primära höljet är aktiverat inaktivera både ström och kylning moduler (PCMs).
2. Vänta några minuter för hello system tooshut ned.
3. Aktivera hello EBOD hölje.
4. När hello EBOD hölje finns på, aktivera hello primära enhet.

## <a name="turn-on-a-device-after-hello-primary-and-ebod-enclosure-connection-is-lost"></a>Aktivera en enhet efter hello primära och EBOD hölje anslutningen bryts
Om hello anslutningen bryts mellan hello vänteläge domänkontrollant och hello motsvarande EBOD styrenhet, fortsätter hello enheten toowork. Om hello mellan hello system aktiva styrenheten och hello motsvarande EBOD styrenhet förloras, växling vid fel ska inträffa och hello enheten ska fortsätta toowork som vanligt.

När båda seriellt ansluten SCSI (SAS)-kablar tas bort eller hello anslutningen mellan hello EBOD höljet och hello primära höljet fjärrdisken slutar hello enheten den att fungera. Nu utföra hello följande steg.

### <a name="tooturn-on-hello-device-after-connection-is-lost"></a>tooturn på hello enhet när anslutningen bryts
1. Åtkomst hello baksidan hello enhet.
2. Om hello SAS kabelanslutning mellan hello EBOD höljet och hello primära höljet har brutits, kommer alla SAS lane indikatorer på hello EBOD hölje vara inaktiverat.
3. Stäng av både ström och kylning moduler (PCMs) på hello EBOD höljet och hello primära enhet.
4. Vänta tills alla hello indikeringar på hello baksidan båda hello-höljen inaktivera.
5. Infoga hello SAS-kablar och kontrollera att det finns en bra anslutning mellan hello EBOD höljet och hello primära enhet.
6. Aktivera hello EBOD hölje första genom att vända båda PCM växlar toohello ON position.
7. Se till att hello EBOD höljet på genom att kontrollera att hello grön Indikator är ON.
8. Aktivera hello primära enhet.
9. Se till att hello primära höljet på genom att kontrollera att hello controller grön Indikator är på.
10. Kontrollera att hello EBOD hölje anslutning med hello primära höljet är bra genom att kontrollera att hello SAS lane indikatorer (fyra per EBOD styrenhet) är alla på.

> [!IMPORTANT]
> Om hello SAS-kablar är felaktiga eller hello anslutningen mellan hello EBOD höljet och hello primära höljet är inte bra när du aktiverar hello system, övergår den i återställningsläge. Kontrollera [kontaktar Microsoft Support](storsimple-8000-contact-microsoft-support.md) om detta händer.


## <a name="turn-off-a-running-device"></a>Inaktivera en enhet som körs
En aktiv virtuell StorSimple-enhet kan behöva toobe stängas av om den flyttas, tas bort från tjänsten, eller har en felaktig komponent som behöver toobe ersättas. hello stegen är olika beroende på om hello StorSimple-enhet är en 8100 eller en 8600-modell. hello 8100 har en enda primär enhet hello 8600 är en dubbla hölje-enhet med en primär enhet och en EBOD bilaga. Det här avsnittet beskrivs hello steg tooshut ned en enhet som körs.

* [Enhet med primär enhet](#8100a)
* [Enhet med EBOD hölje](#8600a)

### <a name="device-with-primary-enclosure-a-name8100a"></a>Enhet med primär enhet<a name="8100a">
tooshut av hello enheten på ett korrekt och kontrollerat sätt du kan göra det via hello klassiska Azure-portalen och hello Windows PowerShell för StorSimple. 

> [!IMPORTANT]
> Inte stängs av en enhet som körs med hjälp av hello strömknappen på hello baksidan hello enhet.
> 
> Kontrollera att alla hello komponenter är felfria innan du stänger hello enhet. I Hej klassiska Azure-portalen, navigera för**enheter** > **Underhåll** > **maskinvarustatus**, och kontrollera status för alla hello komponenter är grön. Detta gäller endast för ett felfritt system. Om hello vara avstängd tooreplace en felaktig komponent kan du ser en misslyckad (röd) eller försämrad (gul) status för respektive hello-komponent i hello **maskinvarustatus**.
> 
> 

När du har åtkomst till hello Windows PowerShell för StorSimple eller hello klassiska Azure-portalen så hello i [stänga av en StorSimple-enhet](storsimple-manage-device-controller.md#shut-down-a-storsimple-device). 

### <a name="device-with-ebod-enclosure-a-name8600a"></a>Enhet med EBOD hölje<a name="8600a">
> [!IMPORTANT]
> Se till att alla hello komponenter är felfria innan du stänger hello primära höljet och hello EBOD hölje. I hello Azure portal, navigerar för**enheter** > **övervakaren** > **maskinvara hälsa**, och kontrollera att alla hello-komponenter är felfria.


#### <a name="tooshut-down-a-running-device-with-ebod-enclosure"></a>tooshut ned en enhet som körs med EBOD hölje
1. Gör alla hello som anges i [stänga av en StorSimple-enhet](storsimple-8000-manage-device-controller.md#shut-down-a-storsimple-device) för hello primära enhet.
2. Efter hello är primära höljet avstängd hello EBOD att stänga av vända av både ström och kylning modul (PCM) växlar.
3. tooverify som hello EBOD har avslutats, kontrollera att alla ljus på hello baksidan hello EBOD hölje är inaktiverade.

> [!NOTE]
> hello SAS-kablar som används tooconnect hello EBOD hölje toohello primära höljet ska inte tas bort förrän efter hello datorn stängs av.

## <a name="next-steps"></a>Nästa steg
[Kontakta Microsoft Support](storsimple-8000-contact-microsoft-support.md) om du får problem när aktivera eller stänga av en StorSimple-enhet.

