---
title: "aaaStorSimple övervakning indikatorer | Microsoft Docs"
description: "Beskriver hello lysdioder (led) och akustiska signaler används toomonitor hello status för hello StorSimple-enhet."
services: storsimple
documentationcenter: NA
author: alkohli
manager: timlt
editor: 
ms.assetid: 59dee7b9-ca6d-4fd9-96e6-a0071e8d248e
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 06/05/2017
ms.author: alkohli
ms.openlocfilehash: e690b8f4727272f5fbb8886a594a046f794a1380
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="use-storsimple-monitoring-indicators-toomanage-your-device"></a>Använda StorSimple övervakning indikatorer toomanage din enhet
## <a name="overview"></a>Översikt
StorSimple-enheten inkluderar lysdioder (led) och larm som du kan använda toomonitor hello-moduler och övergripande status för hello StorSimple-enhet. hello indikatorer för övervakningen kan hittas på hello maskinvarukomponenter hello enhetens primära höljet och hello EBOD hölje. hello indikatorer för övervakningen kan vara indikatorer eller hörbart larm.

Det finns tre Indikator tillstånd används tooindicate hello status för en modul: grön blinkande grön toored-gul eller röd gul.  

* Grönt indikatorer representerar en operativ hälsostatus.  
* Blinkande grön toored gul representerar indikatorer hello förekomst av icke-kritiska villkor som kan kräva åtgärder från användaren.  
* Röd gul indikatorer betyda att det finns ett kritiskt fel finns i hello modul.  

hello resten av den här artikeln beskriver hello olika övervakning indikator led, deras platser på hello StorSimple-enhet, hello Enhetsstatus baserat på hello Indikator tillstånd och associerade hörbart larm.

## <a name="front-panel-indicator-leds"></a>Frontpanel indikator indikatorer
hello frontpanel, även kallat hello *operations panelen* eller *ops panelen*, visar hello sammanlagd status för alla hello-moduler i hello system. hello frontpanel är identiska på hello StorSimple primära och hello EBOD höljet och visas nedan.  

   ![Enheten frontpanel][1]

hello frontpanel innehåller hello följande symboler:  

1. Ljud av
2. Power indikator Indikator (grönt/Röd-gul)
3. Modulen fel indikatorns LEDDE (på OFF-Röd-gul)
4. Logiska fel indikatorns LEDDE (på OFF-Röd-gul
5. Enhet ID visas  

hello största skillnaden mellan hello frontpanel indikatorer för hello enheten och de för hello EBOD hölje är hello **System enhetsnummer identifiering** visas hello Indikator visas. hello standardenheten-ID som visas på enheten hello är **00**, medan hello standard enhet-ID som visas på hello EBOD hölje är **01**. Detta gör att du tooquickly skilja hello enheten och hello EBOD hölje när hello enheten är påslagen. Om enheten är inaktiverad, använder hello informationen i [aktiverar en ny enhet](storsimple-turn-device-on-or-off.md#turn-on-a-new-device) toodifferentiate hello enhet från hello EBOD hölje.  

## <a name="front-panel-led-status"></a>Frontpanel Indikator status
Använd följande tabell tooidentify hello status visas med hello indikatorer på hello frontpanel för hello enhet eller hello EBOD hölje hello.  

| System power | Modul-fel | Logiska fel | Larm | Status |
| --- | --- | --- | --- | --- |
| Röd gul |INAKTIVERA |INAKTIVERA |Saknas |AC power tappas bort arbetar på säkerhetskopiering power eller AC power och hello controller moduler har tagits bort. |
| Grön |ON |ON |Saknas |Testa tillstånd för OPS panelen slå på strömmen (5-tal) |
| Grön |INAKTIVERA |INAKTIVERA |Saknas |Slå på alla funktioner som är bra |
| Grön |ON |Saknas |PCM fel led, fläkt fel indikatorer |PCM begåtts, fläkt fel, över eller under temperatur |
| Grön |ON |Saknas |I/o-modulen indikatorer |Modulen begåtts domänkontrollant |
| Grön |ON |Saknas |Saknas |Höljet logik-fel |
| Grön |Flash |Saknas |Modulstatus Indikator för domänkontrollant modul. PCM fel led, fläkt fel indikatorer |Okänd controller modultypen installerat I2C bus fel, konfigurationsfel för domänkontrollant modulen vital product data (VPD) |

## <a name="power-cooling-module-pcm-indicator-leds"></a>Power kylning modul (PCM) indikator indikatorer
Power kylning modul (PCM) indikator indikatorer kan hittas på hello baksidan hello primära hölje eller EBOD höljet på varje PCM-modul. Det här avsnittet beskrivs hur toouse hello efter indikatorer toomonitor hello status för din StorSimple-enhet.  

* PCM indikatorer för hello primära enhet
* PCM indikatorer för hello EBOD hölje

## <a name="pcm-leds-for-hello-primary-enclosure"></a>PCM indikatorer för hello primära enhet
Hej StorSimple-enhet har en 764W PCM modul med en extra batteri. hello visar följande bild hello Indikator panelen för hello enhet.  

   ![PCM indikatorer på hello primära enhet][2]

Indikator för förklaringen:

1. AC strömavbrott
2. Fläkt fel
3. Batteri fel
4. PCM OK
5. DC-fel
6. Bra batteri  

hello status för hello PCM anges på hello VÄGLEDS panelen. hello enheten PCM Indikator panel har sex indikatorer. Fyra av dessa indikatorer visar hello status för hello strömförsörjning och hello-fläkt. hello återstående två indikatorer visar hello status av hello säkerhetskopiering batteri modul i hello PCM. Du kan använda följande tabeller toodetermine hello status för hello PCM hello.  

### <a name="pcm-indicator-leds-for-power-supply-and-fan"></a>PCM indikator indikatorer för strömförsörjning och -fläkt
| Status | PCM bra (grönt) | AC misslyckas (gult) | Fläkt misslyckas (gult) | Domänkontrollanten misslyckas (gult) |
| --- | --- | --- | --- | --- |
| Ingen AC-ström (tooenclosure) |INAKTIVERA |INAKTIVERA |INAKTIVERA |INAKTIVERA |
| Ingen AC-ström (denna PCM endast) |INAKTIVERA |ON |INAKTIVERA |ON |
| AC finns på PCM - OK |ON |INAKTIVERA |INAKTIVERA |INAKTIVERA |
| PCM misslyckas (fläkt misslyckas) |INAKTIVERA |INAKTIVERA |ON |Saknas |
| PCM-fel (via amp över spänning över aktuella) |INAKTIVERA |ON |ON |ON |
| PCM (fläkt utanför toleransen) |ON |INAKTIVERA |INAKTIVERA |ON |
| Standby-läge |Blinkande |INAKTIVERA |INAKTIVERA |INAKTIVERA |
| PCM hämtning av inbyggd programvara |INAKTIVERA |Blinkande |Blinkande |Blinkande |

### <a name="pcm-indicator-leds-for-hello-backup-battery"></a>PCM indikator indikatorer för hello säkerhetskopiering batteri
| Status | Batteri bra (grönt) | Batteri fel (gul) |
| --- | --- | --- |
| Batteri finns inte |INAKTIVERA |INAKTIVERA |
| Batteri finns och debiteras |ON |INAKTIVERA |
| Batteri laddar eller underhåll avslutning |Blinkande |INAKTIVERA |
| Batteri ”soft” fel (återställa) |INAKTIVERA |Blinkande |
| Batteri ”hårt” fel (oåterkalleligt) |INAKTIVERA |ON |
| Batteri disarmed |Blinkande |INAKTIVERA |

## <a name="pcm-leds-for-hello-ebod-enclosure"></a>PCM indikatorer för hello EBOD hölje
Hej EBOD hölje har en 580 w vid PCM och inget ytterligare batteri. hello PCM panelen för hello EBOD hölje har indikator indikatorer endast för hello strömkällor och hello-fläkt. hello följande bild visar dessa indikatorer.

   ![PCM indikatorer på hello EBOD hölje][3] 

Du kan använda följande tabell toodetermine hello status för hello PCM hello.  

| Status | PCM bra (grönt) | AC misslyckas (gult) | Fläkt misslyckas (gult) | Domänkontrollanten misslyckas (gult) |
| --- | --- | --- | --- | --- |
| Ingen AC-ström (tooenclosure) |INAKTIVERA |INAKTIVERA |INAKTIVERA |INAKTIVERA |
| Ingen AC-ström (denna PCM endast) |INAKTIVERA |ON |INAKTIVERA |ON |
| AC presentera PCM ON – OK |ON |INAKTIVERA |INAKTIVERA |INAKTIVERA |
| PCM misslyckas (fläkt misslyckas) |INAKTIVERA |INAKTIVERA |ON |X |
| PCM-fel (via amp över spänning över aktuella |INAKTIVERA |ON |ON |ON |
| PCM (fläkt utanför toleransen) |ON |INAKTIVERA |INAKTIVERA |ON |
| Vänteläge modellen |Blinkande |INAKTIVERA |INAKTIVERA |INAKTIVERA |
| PCM hämtning av inbyggd programvara |INAKTIVERA |Blinkande |Blinkande |Blinkande |

## <a name="controller-module-indicator-leds"></a>Domänkontrollanten modulen indikator indikatorer
Hej StorSimple-enhet innehåller indikatorer för hello primär domänkontrollant och hello EBOD domänkontrollant moduler.   

### <a name="monitoring-leds-for-hello-primary-controller"></a>Övervakning indikatorer för hello primär domänkontrollant
hello kan följande illustration du identifiera hello indikatorer på hello primär domänkontrollant. (Alla hello-komponenter är listade tooaid orientering.)  

   ![Övervaka led - primär domänkontrollant][4]

Använd hello följande tabell toodetermine om hello controller modulen fungerar korrekt.  

### <a name="controller-indicator-leds"></a>Domänkontrollanten indikator indikatorer
| INDIKATOR | Beskrivning |
| --- | --- |
| Indikator för ID (blå) |Anger att hello modulen identifieras. Om hello blå Indikator blinkar på en domänkontrollant som körs, sedan hello controller hello aktiva styrenheten och hello andra är hello vänteläge domänkontrollant. Mer information finns i [identifiera hello aktiva styrenheten på enheten](storsimple-8000-controller-replacement.md#identify-the-active-controller-on-your-device). |
| Fel Indikator (gul) |Anger om ett fel i hello controller. |
| OK Indikator (grön) |Konstant grönt anger hello styrenheten är OK. Blinkande grönt anger en domänkontrollant VPD konfigurationsfel. |
| SAS-aktivitet indikatorer (grön) |Konstant grönt anger en anslutning utan aktuell aktivitet. Blinkande grönt anger hello anslutningen har en pågående aktivitet. |
| Status för Ethernet-indikatorer |Höger anger länken/nätverksaktivitet: (konstant grön) länken active (blinkande grönt) nätverksaktivitet. Vänster anger nätverkshastigheten: (gul) 1000 Mb/s och (grön) 100 Mb/s (OFF) 10 Mb/s. Beroende på hello komponenten modellen, kan den här ljus blinkar även om hello nätverksgränssnittet inte är aktiverad. |
| EFTER indikatorer |Anger hello Start pågår när hello domänkontrollant är aktiverat. Om hello StorSimple-enhet misslyckas tooboot hjälper denna Indikator Microsoft-supporten identifiera hello i hello startprocessen hello ett fel inträffade vid. |

> [!IMPORTANT]
> Om hello fel Indikator upplysta, finns ett problem med hello controller modulen som kan lösas genom att starta om hello-styrenhet. Kontakta Microsoft Support om att starta om hello-domänkontrollant inte löser problemet.  
> 
> 

### <a name="monitoring-leds-for-hello-ebod-ebod-enclosure"></a>Övervakning indikatorer för hello EBOD (EBOD hölje)
Varje hello 6 Gb/s SAS EBOD domänkontrollanter har indikatorer som anger dess status enligt följande illustration hello.  

  ![Övervaka led - EBOD hölje][5]

Använd hello följande tabell toodetermine om hello EBOD domänkontrollant modulen fungerar normalt.  

### <a name="ebod-controller-module-indicator-leds"></a>EBOD domänkontrollant modulen indikator indikatorer
| Status | I/o-modulen bra (grönt) | Fel i/o-modul (gul) | Värdaktivitet port (grön) |
| --- | --- | --- | --- |
| Domänkontrollanten modulen OK |ON |INAKTIVERA |- |
| Domänkontrollanten modulen fel |INAKTIVERA |ON |- |
| Ingen extern värd portanslutning |- |- |INAKTIVERA |
| Port för externa värdanslutning – ingen aktivitet |- |- |ON |
| Extern värd anslutningsport - aktivitet |- |- |Blinkande |
| Metadata för domänkontrollanten modulfel |Blinkande |- |- |

## <a name="disk-drive-indicator-leds-for-hello-primary-enclosure-and-ebod-enclosure"></a>Diskenhet indikator indikatorer för hello primära höljet och EBOD hölje
Hej StorSimple-enhet har diskenheter som finns i både primära hello-höljet och hello EBOD hölje. Varje diskenhet innehåller övervakning indikator led, enligt beskrivningen i det här avsnittet. 

För hello diskenheter hello Enhetsstatus anges med en grön Indikator och en röd gul Indikator monteras på hello framför varje enhet operatör modul. hello följande bild visar dessa indikatorer.

  ![Diskenhet indikatorer][6]

Använd hello efter tabellen toodetermine hello tillståndet för varje diskenhet, vilket i sin tur påverkar hello övergripande frontpanel Indikator status.  

### <a name="disk-drive-indicator-leds-for-hello-ebod-enclosure"></a>Diskenhet indikator indikatorer för hello EBOD hölje
| Status | Aktiviteten OK Indikator (grön) | Fel Indikator (röd gul) | Associerade ops panelen Indikator |
| --- | --- | --- | --- |
| Ingen installerad enhet |INAKTIVERA |INAKTIVERA |Ingen |
| Enheten installerat och fungerar |Blinkar på/av aktivitet |X |Ingen |
| SCSI Enclosure Services (se) enhetsuppsättning identitet |ON |Blinkande 1 sekund på-1 sekund av |Ingen |
| Se enhetsuppsättning fel bitar |ON |ON |Logiska fel (röd) |
| Strömavbrott kontrollen krets |INAKTIVERA |ON |Modul-fel (röd) |

## <a name="audible-alarms"></a>Akustiska signaler
En StorSimple-enhet innehåller akustiska signaler som är associerade med både primära hello-höljet och hello EBOD hölje. Ett akustiskt larm finns på hello frontpanel (även kallat hello ops panelen) både höljen. hello akustiskt larm anger när ett fel finns. hello larm aktiveras för hello följande villkor:  

* Fläkt fel eller fel
* Spänning utanför intervallet
* Under eller över temperatur villkor
* Termisk överskridande
* Fel
* Logiska fel
* Ange Strömfel
* Borttagning av en exponent kylning modul (PCM)  

hello följande tabell beskrivs hello olika larm tillstånd.  

### <a name="alarm-states"></a>Larm tillstånd
| Larm tillstånd | Åtgärd | Åtgärden med ljud av knappen trycks ned |
| --- | --- | --- |
| S0 |Normalt läge: tyst |Signal två gånger |
| S1 |Fel-läge: 1 sekund på-1 sekund av |Övergång tooS2 eller S3 (se kommentarer) |
| S2 |Påminn läge: återkommande signal |Ingen |
| S3 |Ljudet är avstängt läge: tyst |Ingen |
| S4 |Kritiska fel läge: kontinuerlig larm |Inte tillgänglig: tyst läge inte aktiv |

> [!NOTE]
> * Tillståndet larm S1, om du inte trycker tyst inom 2 minuter övergår hello tillstånd automatiskt tooS2 eller S3.  
> * Larm tillstånd S1 tooS4 tillbaka tooS0 när hello fel är avmarkerad.  
> * Kritiska fel tillstånd S4 kan anges från andra tillstånd.  


Du kan stänga av hello akustiskt larm genom att trycka på hello ljud av knappen på hello ops panelen. Stänga av automatisk ljudet utförs efter två minuter om hello ljud av växeln inte drivs manuellt. När hello larm är avstängt, kommer att fortsätta toosound med kort återkommande PIP tooindicate som en problemet fortfarande kvarstår. hello larm blir tyst när alla hello problem är avmarkerad.

hello följande tabell beskrivs hello olika larm villkor.

### <a name="alarm-conditions"></a>Larm villkor
| Status | Allvarsgrad | Larm | OPS panelen Indikator |
| --- | --- | --- | --- |
| PCM aviseringen – strömavbrott DC från en enda PCM |Fel – utan förlust av redundans |S1 |Modul-fel |
| PCM aviseringen – strömavbrott DC från en enda PCM |Fel – förlust av redundans |S1 |Modul-fel |
| PCM-fläkt misslyckas |Fel – förlust av redundans |S1 |Modul-fel |
| Flesta SBB-modulen upptäckte PCM-fel |Fel |S1 |Modul-fel |
| PCM tas bort |Konfigurationsfel |Ingen |Modul-fel |
| Höljet konfigurationsfel |Fel – kritisk |S1 |Modul-fel |
| Låg temperatur-varning |Varning |S1 |Modul-fel |
| Hög temperatur-varning |Varning |S1 |Modul-fel |
| Över temperatur larm |Fel – kritisk |S1 |Modul-fel |
| I2C bus fel |Fel – förlust av redundans |S1 |Modul-fel |
| OPS panelen kommunikationsfel (I2C) |Fel – kritisk |S1 |Modul-fel |
| Domänkontrollanten fel |Fel – kritisk |S1 |Modul-fel |
| Flesta SBB gränssnittet modulen fel |Fel – kritisk |S1 |Modul-fel |
| Flesta SBB gränssnittet modulen fel – ingen fungerande moduler återstående |Fel – kritisk |S4 |Modul-fel |
| Flesta SBB gränssnittet modulen tas bort |Varning |Ingen |Modul-fel |
| Enheten Strömfel kontroll |Varning – ingen strömavbrott enhet |S1 |Modul-fel |
| Enheten Strömfel kontroll |Fel – kritiska; strömavbrott enhet |S1 |Modul-fel |
| Enheten tas bort |Varning |Ingen |Modul-fel |
| Otillräckligt med ström tillgänglig |Varning |Ingen |Modul-fel |

## <a name="next-steps"></a>Nästa steg
Lär dig mer om [StorSimple status och maskinvarukomponenter](storsimple-8000-monitor-hardware-status.md).

[1]: ./media/storsimple-monitoring-indicators/storsimple-monitoring-indicators-IMAGE01.png
[2]: ./media/storsimple-monitoring-indicators/storsimple-monitoring-indicators-IMAGE02.png
[3]: ./media/storsimple-monitoring-indicators/storsimple-monitoring-indicators-IMAGE03.png
[4]: ./media/storsimple-monitoring-indicators/storsimple-monitoring-indicators-IMAGE04.png
[5]: ./media/storsimple-monitoring-indicators/storsimple-monitoring-indicators-IMAGE05.png
[6]: ./media/storsimple-monitoring-indicators/storsimple-monitoring-indicators-IMAGE06.png


