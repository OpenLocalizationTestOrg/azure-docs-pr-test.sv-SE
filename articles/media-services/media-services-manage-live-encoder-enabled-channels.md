---
title: "aaaLive strömning med Azure Media Services toocreate multibithastighet dataströmmar | Microsoft Docs"
description: "Det här avsnittet beskrivs hur tooset in en kanal som tar emot en enkel bithastighet direktsänd dataström från en lokala kodare och utför sedan live encoding tooadaptive bithastighetsströmmen med Media Services. hello dataströmmen kan sedan levereras tooclient uppspelning program via en eller flera Strömningsslutpunkter, med någon av följande anpassningsbar strömning protokoll hello: HLS, Smooth Stream, MPEG DASH."
services: media-services
documentationcenter: 
author: anilmur
manager: cfowler
editor: 
ms.assetid: 30ce6556-b0ff-46d8-a15d-5f10e4c360e2
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/09/2017
ms.author: juliako;anilmur
ms.openlocfilehash: a8bbdd1570cc9a11bfc2de7bb4ceb9006cc25534
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="live-streaming-using-azure-media-services-toocreate-multi-bitrate-streams"></a>Direktsänd strömning med Azure Media Services toocreate multibithastighet strömmar
## <a name="overview"></a>Översikt
I Azure Media Services (AMS), en **kanal** representerar en pipeline för bearbetning av liveströmmat innehåll. En **kanal** tar emot live indata i ett av två sätt:

* En lokal livekodare skickar en dataström med enkel bithastighet toohello kanal som är aktiverade tooperform live encoding med Media Services i något av hello följande format: RTP (MPEG-TS) RTMP eller Smooth Streaming (fragmenterad MP4). hello kanalen utför sedan live encoding av hello inkommande dataströmmen tooa flera bithastigheter (anpassningsbar) video dataström med enkel bithastighet. På begäran levererar Media Services hello dataströmmen toocustomers.
* En lokal livekodare skickar flera bithastigheter **RTMP** eller **Smooth Streaming** (fragmenterad MP4) toohello kanal som inte är aktiverat tooperform live encoding med AMS. hello inhämtas strömmarna passerar genom **kanal**s utan vidare bearbetning. Den här metoden anropas **direkt**. Du kan använda följande livekodare som skickar Smooth Streaming i flera bithastigheter hello: MediaExcel, Ateme, anta kommunikation, Envivio, Cisco och Elemental. hello följande livekodare skickar RTMP: Adobe Flash Media Live-kodare (FMLE), Telestream Wirecast, Haivision, Teradek och Tricaster kodare.  En livekodare kan även skicka en enkel bithastighet dataströmmen tooa kanal som inte har aktiverats för live encoding, men det rekommenderas inte. På begäran levererar Media Services hello dataströmmen toocustomers.
  
  > [!NOTE]
  > Genomströmningsmetoden är hello mest ekonomiska sättet toodo live strömning.
  > 
  > 

Från och med hello Media Services 2.10 versionen när du skapar en kanal kan ange du hur du vill använda för Indataströmmen din kanal tooreceive hello och om huruvida du vill använda för hello kanal tooperform live encoding av strömmen. Du kan välja mellan två alternativ:

* **Ingen** – ange det här värdet om du planerar toouse en lokal livekodare som kommer att skrivas ut flera bithastigheter dataström (en direkt stream). I det här fallet hello inkommande dataström passerat toohello utdata utan kodning. Detta görs hello av en kanal tidigare too2.10 utgåva.  Mer detaljerad information om att arbeta med kanaler av den här typen finns [direktsänd strömning med lokala kodare som skapar dataströmmar i multibithastighet](media-services-live-streaming-with-onprem-encoders.md).
* **Standard** – Välj det här värdet om du planerar toouse Media Services tooencode toomulti bithastighet strömmen direktsänd dataström med enkel bithastighet. Tänk på att det finns en fakturering påverkan för live encoding och ska du komma ihåg att och en live encoding kanal i hello ”körs” tillstånd kommer avgifter faktureringsinformation.  Vi rekommenderar att du stoppar du omedelbart dina kanaler som körs efter din direktsänd strömning händelse är klar tooavoid timvis extra kostnader.

> [!NOTE]
> Det här avsnittet beskrivs attributen för kanaler som är aktiverade tooperform live encoding (**Standard** kodningstypen). För information om att arbeta med kanaler som inte är aktiverat tooperform live encoding finns [direktsänd strömning med lokala kodare som skapar dataströmmar i multibithastighet](media-services-live-streaming-with-onprem-encoders.md).
> 
> Se till att tooreview hello [överväganden](media-services-manage-live-encoder-enabled-channels.md#Considerations) avsnitt.
> 
> 

## <a name="billing-implications"></a>Konsekvenser för fakturering
En kanal för live encoding börjar fakturering som sitt tillstånd övergångar för ”körs” via hello API.   Du kan också visa hello tillstånd i hello Azure-portalen eller hello Azure Media Services Explorer tool (http://aka.ms/amse).

hello följande tabell visar hur kanal tillstånd mappa toobilling tillstånd i hello API och Azure-portalen. Observera att hello tillstånd är något annorlunda mellan hello API och portalen UX. När en kanal har tillståndet hello ”körs” via hello API eller i hello ”klar” eller ”Streaming” tillstånd i hello Azure-portalen, fakturering kommer att vara aktiv.
toostop hello kanal från fakturering du ytterligare, har du tooStop hello kanal via hello API eller hello Azure-portalen.
Du ansvarar för att stoppa dina kanaler när du är klar med hello live encoding kanaler.  Fel toostop en kodning kanal leder fortsatt fakturering.

### <a id="states"></a>Kanal tillstånd och hur de mappas toohello fakturering läge
hello aktuell status för en kanal. Möjliga värden omfattar:

* **Stoppats**. Hello inledningsvis i hello kanal är efter skapades (om inte autostart har valts i hello-portalen.) Ingen fakturering inträffar i det här tillståndet. I det här tillståndet hello kanal egenskaper kan uppdateras men strömning tillåts inte.
* **Starta**. hello kanal startas. Ingen fakturering inträffar i det här tillståndet. Inga uppdateringar eller strömning tillåts i det här tillståndet. Om ett fel inträffar, returnerar hello kanal toohello stoppat tillstånd.
* **Kör**. hello kanal kan bearbetning live dataströmmar. Det nu fakturering användning. Du måste stoppa hello kanal tooprevent ytterligare fakturering. 
* **Stoppa**. hello kanalen har stoppats. Ingen fakturering inträffar denna status. Inga uppdateringar eller strömning tillåts i det här tillståndet.
* **Om du tar bort**. hello kanal tas bort. Ingen fakturering inträffar denna status. Inga uppdateringar eller strömning tillåts i det här tillståndet.

hello följande tabell visar hur kanal tillstånd kartan toohello fakturerings-läge. 

| Kanaltillstånd | Portalgränssnittsindikatorer | Är det fakturering? |
| --- | --- | --- |
| Startar |Startar |Nej (övergångsläge) |
| Körs |Klart (inga program körs)<br/>eller<br/>Strömmar (minst ett program körs) |JA |
| Stoppas |Stoppas |Nej (övergångsläge) |
| Stoppad |Stoppad |Nej |

### <a name="automatic-shut-off-for-unused-channels"></a>Automatisk avstängning av för oanvända kanaler
Från och med 25 januari 2016 Media Services distribuerat en uppdatering som automatiskt stoppar en kanal (med live encoding aktiverat) när den har körts i ett tillstånd som inte används under lång tid. Detta gäller tooChannels som inte har något active och som inte har fått ett inkommande bidrag flödet för en längre tid.

hello tröskelvärdet för en oanvända tid är praktiskt taget 12 timmar, men är ämne toochange.

## <a name="live-encoding-workflow"></a>Live Encoding-arbetsflöde
hello följande diagram representerar en live-streaming arbetsflöde där en kanal som tar emot en dataström med enkel bithastighet i något av följande protokoll hello: RTMP, Smooth Streaming eller RTP (MPEG-TS); den sedan kodar hello dataströmmen tooa multibithastighet dataströmmen. 

![Live-arbetsflöde][live-overview]

## <a id="scenario"></a>Vanligt Scenario för direktsänd strömning
hello följande är allmänna steg som ingår i att skapa vanliga program för direktsänd strömning.

> [!NOTE]
> Hej max är för närvarande rekommenderas varaktighet för en direktsänd händelse 8 timmar. Kontakta amslived på Microsoft.com om du behöver toorun en kanal under längre perioder. Tänk på att det finns en fakturering påverkan för live encoding och ska du komma ihåg att och en live encoding kanal i hello ”körs” tillstånd kommer avgifter per timme faktureringsinformation.  Vi rekommenderar att du stoppar du omedelbart dina kanaler som körs efter din direktsänd strömning händelse är klar tooavoid timvis extra kostnader. 
> 
> 

1. Anslut en videokamera tooa dator. Starta och konfigurera en lokal livekodare som kan mata ut en **enda** bithastighet i något av hello följande protokoll: RTMP, Smooth Streaming eller RTP (MPEG-TS). 
   
    Det här steget kan också utföras när du har skapat din kanal.
2. Skapa och starta en kanal. 
3. Hämta hello kanal infognings-URL. 
   
    hello infognings-URL som används av hello livekodaren toosend hello dataströmmen toohello kanal.
4. Hämta hello kanalens förhandsgransknings-URL. 
   
    Använd den här URL: en tooverify att din kanal är tar emot hello direktsänd dataström.
5. Skapa ett program. 
   
    Det skapas även en tillgång till när med hello Azure-portalen, för att skapa ett program. 
   
    När du använder .NET SDK eller REST behöver toocreate en tillgång och ange toouse tillgången när du skapar ett Program. 
6. Publicera hello tillgången som är associerad med hello-programmet.   
   
    >[!NOTE]
    >När AMS-kontot skapas en **standard** strömningsslutpunkt har lagts till tooyour konto i hello **stoppad** tillstånd. Hej strömningsslutpunkt från vilken du vill att toostream innehåll har toobe i hello **kör** tillstånd. 
    
7. Starta hello programmet när du är klar toostart strömning och arkivering.
8. Du kan också kan hello livekodare vara signalerat toostart en annons. hello annonsen infogas i utdataströmmen hello.
9. Stoppa programmet hello när du vill toostop strömningen och arkiveringen hello händelsen.
10. Ta bort hello programmet (och ta eventuellt bort tillgången hello).   

> [!NOTE]
> Det är mycket viktigt inte tooforget tooStop en kanal för Live-kodning. Tänk på att det finns en varje timme fakturering påverkan för live encoding och ska du komma ihåg att och en live encoding kanal i hello ”körs” tillstånd kommer avgifter faktureringsinformation.  Vi rekommenderar att du stoppar du omedelbart dina kanaler som körs efter din direktsänd strömning händelse är klar tooavoid timvis extra kostnader. 
> 
> 

## <a id="channel"></a>Kanal input (infognings) konfigurationer
### <a id="Ingest_Protocols"></a>Strömmande infogningsprotokollet
Om hello **kodare typen** har angetts för**Standard**, giltiga alternativ är:

* **RTP** (MPEG-TS): MPEG-2-transportström över RTP.  
* Enkel bithastighet **RTMP**
* Enkel bithastighet **fragmenterad MP4** (Smooth Streaming)

#### <a name="rtp-mpeg-ts---mpeg-2-transport-stream-over-rtp"></a>RTP (MPEG-TS) - MPEG-2-transportström över RTP.
Vanligt användningsfall: 

Professional sändningsföretag normalt fungerar med avancerade lokal livekodare från leverantörer som grundämne tekniker, Ericsson, Ateme, Imagine eller Envivio toosend en dataström. Ofta används tillsammans med en IT-avdelning och i privata nätverk.

Att tänka på:

* Det rekommenderas starkt hello användning av ett enda program-transportström (SPTS) som indata. 
* Du kan ange upp too8 ljudströmmar med MPEG-2 TS över RTP. 
* hello video-ström ska ha en genomsnittlig bithastighet under 15 Mbit/s
* hello sammanställd genomsnittlig bithastighet hello ljudströmmar ska vara under 1 Mbit/s
* Följande är hello koder som stöds:
  
  * MPEG-2 / H.262 Video 
    
    * Centrala profil (4:2:0)
    * Hög profil (4:2:0, 4:2:2)
    * 422 profil (4:2:0, 4:2:2)
  * MPEG-4 AVC / H.264 Video  
    
    * Baslinje, Main, hög profil (8-bitars 4:2:0)
    * Hög 10 profil (10 bitar 4:2:0)
    * Hög 422 profil (10 bitar 4:2:2)
  * MPEG-2 AAC-LC ljud 
    
    * Mono, Stereo Surround (5.1, 7.1)
    * MPEG-2-style ADTS paketering
  * Dolby Digital (AC-3) ljud 
    
    * Mono, Stereo Surround (5.1, 7.1)
  * MPEG Audio (Layer II och III) 
    
    * Mono, Stereo
* Rekommenderade broadcast kodare inkluderar:
  
  * Anta att kommunikation Selenio ENC 1
  * Anta att kommunikation Selenio ENC 2
  * Elemental Live

#### <a id="single_bitrate_RTMP"></a>RTMP med enkel bithastighet
Att tänka på:

* hello inkommande dataströmmen får inte innehålla flera bithastigheter video
* hello video-ström ska ha en genomsnittlig bithastighet under 15 Mbit/s
* hello ljudström ska ha en genomsnittlig bithastighet nedan 1 Mbit/s
* Följande är hello koder som stöds:
* MPEG-4 AVC / H.264 Video
* Baslinje, Main, hög profil (8-bitars 4:2:0)
* Hög 10 profil (10 bitar 4:2:0)
* Hög 422 profil (10 bitar 4:2:2)
* MPEG-2 AAC-LC ljud
* Mono, Stereo Surround (5.1, 7.1)
* 44,1 samplingsfrekvensen
* MPEG-2-style ADTS paketering
* Rekommenderade kodare inkluderar:
* Telestream Wirecast
* Flash Media Livekodare

#### <a name="single-bitrate-fragmented-mp4-smooth-streaming"></a>Fragmenterad MP4 med enkel bithastighet (Smooth Streaming)
Vanligt användningsfall:

Använd en lokal livekodare från leverantörer som grundämne tekniker, Ericsson, Ateme, Envivio toosend hello Indataströmmen över hello öppna internet tooa Närliggande Azure-datacenter.

Att tänka på:

Samma som för [RTMP med enkel bithastighet](media-services-manage-live-encoder-enabled-channels.md#single_bitrate_RTMP).

#### <a name="other-considerations"></a>Andra överväganden
* Du kan inte ändra hello indataprotokollet när hello kanalen eller dess associerade program körs. Om du behöver olika protokoll får du skapa separata kanaler för varje indataprotokoll.
* Maximal upplösning för hello inkommande video-ström är 1 920 x 1 080 och högst 60 fält per sekund om sammanflätad eller 30 bildrutor per sekund om progressiv.

### <a name="ingest-urls-endpoints"></a>Infognings-URL: er (slutpunkter)
En kanal som tillhandahåller en slutpunkt för indata (infognings-URL) som du anger i hello livekodaren, så kan push-hello kodare strömmar tooyour kanaler.

Du kan hämta hello infognings-URL: er när du skapar en kanal. tooget dessa URL: er hello kanal har inte toobe i hello **kör** tillstånd. När du är klar toostart skicka data till hello kanal, måste det vara i hello **kör** tillstånd. När hello kanal startar mata in data, kan du förhandsgranska dataströmmen via hello förhandsgransknings-URL.

Du har ett alternativ för vill föra in fragmenterad MP4 (Smooth Streaming) direktsänd dataström via en SSL-anslutning. tooingest via SSL, se till att tooupdate hello infognings-URL: en tooHTTPS. Observera att för närvarande AMS inte stöder SSL med anpassade domäner.  

### <a name="allowed-ip-addresses"></a>Tillåtna IP-adresser
Du kan definiera hello IP-adresser som är tillåtna toopublish video toothis kanal. Tillåtna IP-adresser kan anges som antingen en enskild IP-adress (t.ex. ”10.0.0.1”), ett IP-intervall med en IP-adress och en CIDR-undernätsmask (t.ex. ”10.0.0.1/22”) eller ett IP-intervall med en IP-adress och en CIDR-undernätsmask med punktavgränsad decimalform (t.ex. '10.0.0.1(255.255.252.0)').

Om inga IP-adresser har angetts och det saknas regeldefinitioner kommer ingen IP-adress att tillåtas. tooallow IP-adresser, skapa en regel och ange 0.0.0.0/0.

## <a name="channel-preview"></a>Kanal preview
### <a name="preview-urls"></a>URL: er för förhandsgranskning
Kanaler ger en förhandsgranskningsslutpunkten (förhandsgranskning URL) om du använder toopreview och verifiera strömmen innan ytterligare bearbetning och leverans.

Du kan hämta hello förhandsgransknings-URL när du skapar hello-kanal. tooget hello URL hello kanal har inte toobe i hello **kör** tillstånd.

När hello kanal startar mata in data, kan du förhandsgranska dataströmmen.

> [!NOTE]
> Hello Förhandsgranska dataströmmen kan för närvarande bara levereras i fragmenterad MP4 (Smooth Streaming) oavsett hello angivet Indatatyp. Du kan använda hello [http://smf.cloudapp.net/healthmonitor](http://smf.cloudapp.net/healthmonitor) player tootest hello Smooth Stream. Du kan också använda en spelare finns i hello Azure portal tooview strömmen.
> 
> 

### <a name="allowed-ip-addresses"></a>Tillåtna IP-adresser
Du kan definiera hello IP-adresser som är tillåtna tooconnect toohello förhandsgranskningsslutpunkten. Om inga IP-adresser har angetts någon IP-adress ska tillåtas. Tillåtna IP-adresser kan anges som antingen en enskild IP-adress (t.ex. ”10.0.0.1”), ett IP-intervall med en IP-adress och en CIDR-undernätsmask (t.ex. ”10.0.0.1/22”) eller ett IP-intervall med en IP-adress och en CIDR-undernätsmask med punktavgränsad decimalform (t.ex. ”10.0.0.1(255.255.252.0)”).

## <a name="live-encoding-settings"></a>Live kodningsinställningar
Det här avsnittet beskrivs hur hello inställningar för hello livekodaren i hello kanal kan justeras när hello **kodning typen** av en kanal har angetts för**Standard**.

> [!NOTE]
> När mata in flera språkspår och utföra live encoding med Azure, endast RTP stöds för flera språk indata. Du kan definiera upp too8 ljudströmmar med MPEG-2 TS över RTP. Vill föra in flera ljud spår med RTMP eller Smooth streaming stöds inte för närvarande. När du gör live encoding med [lokalt live kodar](media-services-live-streaming-with-onprem-encoders.md), det finns ingen sådan begränsning eftersom det skickas tooAMS passerar genom en kanal utan vidare bearbetning.
> 
> 

### <a name="ad-marker-source"></a>Markör AD-källa
Du kan ange hello källan för annonsmarkörssignaler. Standardvärdet är **Api**, vilket anger att hello livekodaren i hello kanalen ska lyssna tooan asynkron **Annonsmarkörs-API**.

hello annat giltiga alternativ är **Scte35** (tillåts endast om hello mata in strömning protokoll anges tooRTP (MPEG-TS). När Scte35 har angetts, kommer att parsa SCTE 35 signaler från hello RTP (MPEG-TS) Indataströmmen hello livekodaren.

### <a name="cea-708-closed-captions"></a>CEA 708 textning
Ett valfritt flagga som visar hello livekodaren tooignore CEA 708 bildtexter data som är inbäddad i hello inkommande video. När hello flaggan anges toofalse (standard), identifierar hello kodare och sätt tillbaka CEA 708 data i hello video utdataströmmar.

### <a name="video-stream"></a>Video-ström
Valfri. Beskriver hello video Indataströmmen. Om det här fältet inte anges används standardvärdet för hello. Den här inställningen är bara tillåtet om hello indata strömningsprotokoll anges tooRTP (MPEG-TS).

#### <a name="index"></a>Index
Ett Nollbaserat index som anger vilka video Indataströmmen ska bearbetas av hello livekodaren i hello kanalen. Den här inställningen gäller endast om infognings-strömning protokollet är RTP (MPEG-TS).

Standardvärdet är noll. Det rekommenderas toosend i ett enda program transportström (SPTS). Om hello Indataströmmen innehåller flera program, hello livekodaren Parsar hello programmet kartan tabellen (betalning) i hello indata, identifierar hello indata som har ett stream-typnamn för MPEG-2-Video eller H.264 och ordnar dem i hello ordning som anges i hello bet. hello nollbaserade index används sedan toopick in hello nte post i den ordningen.

### <a name="audio-stream"></a>Ljudström
Valfri. Beskriver hello inkommande ljudströmmar. Om det här fältet inte har angetts gäller hello standardvärdena. Den här inställningen är bara tillåtet om hello indata strömningsprotokoll anges tooRTP (MPEG-TS).

#### <a name="index"></a>Index
Det rekommenderas toosend i ett enda program transportström (SPTS). Om hello Indataströmmen innehåller flera program, hello livekodaren i hello kanal Parsar hello programmet kartan tabellen (betalning) i hello indata, identifierar hello indata som har ett stream-typnamn för MPEG-2 AAC ADTS eller AC-3 System-A eller AC-3 System-B eller privata MPEG-2 SÄMSTA eller ljud MPEG-1 eller MPEG-2 ljud och ordnar dem i hello ordning som anges i hello bet. hello nollbaserade index används sedan toopick in hello nte post i den ordningen.

#### <a name="language"></a>Språk
Hej språk-ID för hello ljudström, som uppfyller tooISO 639-2, till exempel ENG. Om den inte finns är hello standard och (Odefinierad).

Det kan finnas upp too8 ljudström mängder anges om hello indata toohello kanal är MPEG-2 TS över RTP. Det kan dock inga två poster med hello samma värde för indexet.

### <a id="preset"></a>System förinställda
Anger hello förinställda toobe som används av hello livekodaren i den här kanalen. Hej endast tillåtna värde är för närvarande **Default720p** (standard).

Observera att om du behöver anpassade förinställningar, bör du kontakta amslived på Microsoft.com.

**Default720p** kommer koda hello video till hello efter 7 lager.

#### <a name="output-video-stream"></a>Video utdataströmmen
| Bithastighet | Bredd | Höjd | MaxFPS | Profil | Namn på utdata Stream |
| --- | --- | --- | --- | --- | --- |
| 3500 |1280 |720 |30 |Hög |Video_1280x720_3500kbps |
| 2200 |960 |540 |30 |Main |Video_960x540_2200kbps |
| 1350 |704 |396 |30 |Main |Video_704x396_1350kbps |
| 850 |512 |288 |30 |Main |Video_512x288_850kbps |
| 550 |384 |216 |30 |Main |Video_384x216_550kbps |
| 350 |340 |192 |30 |Baslinje |Video_340x192_350kbps |
| 200 |340 |192 |30 |Baslinje |Video_340x192_200kbps |

#### <a name="output-audio-stream"></a>Utdataströmmen för ljud
Ljud är kodade toostereo AAC LC 64 kbit/s samplingsfrekvens 44,1 kHz.

## <a name="signaling-advertisements"></a>Annonser-signalering
När kanalen har Live Encoding aktiverat, du har en komponent i pipeline som är bearbetningen video och kan ändra den. Du kan skicka en signal för hello kanal tooinsert pekdatorer och/eller annonser i hello utgående ström med anpassningsbar bithastighet. Pekdatorer är fortfarande bilder som du kan använda toocover in hello inkommande live feeden i vissa fall (till exempel under en kommersiell paus). Annonserar signaler är tiden har synkroniserats signalerar du bäddar in hello utgående dataström tootell hello videospelare tootake specialåtgärd – till exempel tooswitch tooan annons vid hello lämplig tidpunkt. Se den här [blogg](https://codesequoia.wordpress.com/2014/02/24/understanding-scte-35/) en översikt över hello SCTE 35 signaling mekanismen som används för detta ändamål. Nedan visas ett vanligt scenario som du kan implementera i din direktsända händelse.

1. Har din visningsprogram hämta en avbildning av före HÄNDELSEN innan hello händelsen startar.
2. Har din visningsprogram hämta en avbildning efter HÄNDELSEN när hello händelsen slutar.
3. Har din visningsprogram får en FELHÄNDELSE avbildning om det finns ett problem när hello händelser (till exempel strömavbrott i hello stadium).
4. Skicka en AD-BREAK avbildningen toohide hello direktsänd händelse under en kommersiell break-feed.

hello följande är hello egenskaper som du kan ange när signalering annonser. 

### <a name="duration"></a>Varaktighet
hello varaktighet i sekunder, för hello kommersiella break. Detta har toobe ett positivt värde inte är noll i ordning toostart hello kommersiella break. När en kommersiell break är pågående och hello varaktighet set toozero med hello CueId matchar hello pågående kommersiella break, och sedan att break har avbrutits.

### <a name="cueid"></a>CueId
Ett unikt ID för hello kommersiella break toobe som används av nedströms program tootake lämpliga åtgärder. Måste toobe ett positivt heltal. Du kan ange det här värdet tooany slumpmässiga positivt heltal eller använda en överordnad system tootrack hello stack-ID: N. Se vissa toonormalize alla ID: n toopositive heltal innan du skickar via hello API.

### <a name="show-slate"></a>Visa mallen
Valfri. Signalerar hello livekodaren tooswitch toohello [standard mallen](media-services-manage-live-encoder-enabled-channels.md#default_slate) avbildningen under kommersiella pausen och dölja hello inkommande Videoupptagning. Ljudet är också avstängt under mallen. Standardvärdet är **FALSKT**. 

hello bild används blir hello som har angetts via hello standard en tillgång Id-egenskapen för närvarande hello hello kanal skapas. hello mallen kommer att sträckas ut toofit hello visningsstorlek avbildningen. 

## <a name="insert-slate--images"></a>Infoga mallen bilder
Hej livekodaren i hello kanal kan vara signalerat tooswitch tooa en bild. Det kan också vara signalerat tooend en pågående mallen. 

Hej livekodare kan vara konfigurerade tooswitch tooa en bild och dölja hello inkommande video signal i vissa situationer – till exempel under ett ad-break. Om en sådan mallen inte har konfigurerats maskeras inte indatavideo under den ad-break.

### <a name="duration"></a>Varaktighet
hello varaktighet hello mallen i sekunder. Detta har toobe ett positivt värde inte är noll i ordning toostart hello mallen. Om det finns en pågående mallen och en varaktighet på noll har angetts, avslutas den pågående mallen.

### <a name="insert-slate-on-ad-marker"></a>Infoga bakgrundsbild vid reklammarkör
När set tootrue, den här inställningen konfigurerar hello livekodaren tooinsert en bild under en ad-break. hello standardvärdet är true. 

### <a id="default_slate"></a>Id som standard en tillgång

Valfri. Anger hello tillgångsnummer av hello Media Services tillgång som innehåller hello en bild. Standardvärdet är null. 


>[!NOTE] 
>Innan du skapar hello kanal ska hello en bild med följande begränsningar hello skickas som en dedikerad tillgång (inga andra filer ska vara i tillgången). Den här avbildningen används endast när hello livekodaren infogar en mallen på grund av tooan ad break eller uttryckligen har signalerat tooinsert en mallen. Hej livekodare kan även gå i ett läge som en under vissa felvillkor – till exempel om hello insignal går förlorad. Det finns för närvarande inga alternativet toouse en anpassad avbildning när hello livekodaren anger sådana insignal förlorade tillståndet. För den här funktionen kan du rösta [här](https://feedback.azure.com/forums/169396-azure-media-services/suggestions/10190457-define-custom-slate-image-on-a-live-encoder-channel).


* Högst 1 920 x 1 080 i resolution.
* Högst 3 MB i storlek.
* hello filnamn måste ha filnamnstillägget *.jpg.
* hello avbildningen måste laddas upp till en tillgång som hello endast AssetFile i tillgången och den här AssetFile markeras som hello primära filen. hello tillgång kan inte vara lagringskrypterad.

Om hello **standard reservera tillgångsnummer** har inte angetts, och **Infoga mallen på ad markör** har angetts för**SANT**, en standardbild Azure Media Services kommer att använda toohide hello indata video-ström. Ljudet är också avstängt under mallen. 

## <a name="channels-programs"></a>Kanalens program
En kanal är associerad med program som gör att du toocontrol hello publicering och lagring av segment i en direktsänd dataström. Kanaler hanterar program. hello är relationen mellan kanal och Program mycket lik tootraditional media där en kanal har en konstant ström av innehåll och ett program är begränsade toosome tidsinställd händelse på kanalen.

Du kan ange hello antal timmar som du vill att tooretain hello registreras innehåll för programmet hello genom att ange hello **Arkivfönster** längd. Det här värdet kan anges från minst 5 minuter tooa högst 25 timmar. Längd avgör också hello hur lång tid som klienter kan söka bakåt i tiden från hello aktuella direktsända positionen. Program kan köras under hello angiven tidsperiod, men innehåll som ligger bakom hello längd ignoreras kontinuerligt. Det här värdet för den här egenskapen avgör också hur länge hello klient manifest kan växa.

Varje program som är associerad med en tillgång som lagrar hello strömmas innehåll. En tillgång är mappade tooa block blob-behållaren i hello Azure Storage-konto och hello i hello tillgången filer som blobbar i behållaren. toopublish hello program så att kunderna kan visa hello dataströmmen måste du skapa en positionerare för hello associerade tillgången. Med den här positioneraren kan toobuild en strömnings-URL som du kan tillhandahålla tooyour klienter.

En kanal har stöd för upp toothree program som körs samtidigt så du kan skapa flera Arkiv för hello samma inkommande dataström. Detta ger dig toopublish och arkivera olika delar av en händelse efter behov. Till exempel är dina affärsbehov tooarchive 6 timmar av ett program, men toobroadcast sista 10 minuter. tooaccomplish detta, behöver du toocreate två program som körs samtidigt. Ett program är tooarchive 6 timmar av händelsen hello men hello programmet publiceras inte. hello kan inte ange tooarchive i 10 minuter och det här programmet har publicerats.

Du bör inte återanvända befintliga program för nya händelser. Skapa och starta ett nytt program för varje händelse enligt beskrivningen i avsnittet för hello Programming Live Streaming program istället.

Starta hello programmet när du är klar toostart strömning och arkivering. Stoppa programmet hello när du vill toostop strömningen och arkiveringen hello händelsen. 

toodelete arkiverat innehåll, stoppa och ta bort programmet hello och ta sedan bort hello associerade tillgången. En tillgång kan inte tas bort om den används av ett program. hello programmet måste tas bort först. 

När du stoppar och ta bort programmet hello hello användare skulle vara kan toostream ditt arkiverade innehåll som en video på begäran för så länge du inte ta bort hello tillgången.

Om du vill tooretain hello arkiverat innehåll, men inte har den tillgänglig för strömning, tar du bort hello strömning lokaliserare.

## <a name="getting-a-thumbnail-preview-of-a-live-feed"></a>Hämta en miniatyr av en live-feed
När Live Encoding aktiveras kan nu du få en förhandsgranskning av hello live feed när den når hello-kanal. Detta kan vara ett ovärderligt verktyg toocheck om din live feed faktiskt når hello-kanal. 

## <a id="states"></a>Kanal tillstånd och hur tillstånd mappas toohello fakturering läge
hello aktuell status för en kanal. Möjliga värden omfattar:

* **Stoppats**. Detta är hello inledningsvis i hello kanal när skapandet. I det här tillståndet hello kanal egenskaper kan uppdateras men strömning tillåts inte.
* **Starta**. hello kanal startas. Inga uppdateringar eller strömning tillåts i det här tillståndet. Om ett fel inträffar, returnerar hello kanal toohello stoppat tillstånd.
* **Kör**. hello kanal kan bearbetning live dataströmmar.
* **Stoppa**. hello kanalen har stoppats. Inga uppdateringar eller strömning tillåts i det här tillståndet.
* **Om du tar bort**. hello kanal tas bort. Inga uppdateringar eller strömning tillåts i det här tillståndet.

hello följande tabell visar hur kanal tillstånd kartan toohello fakturerings-läge. 

| Kanaltillstånd | Portalgränssnittsindikatorer | Fakturerad? |
| --- | --- | --- |
| Startar |Startar |Nej (övergångsläge) |
| Körs |Klart (inga program körs)<br/>eller<br/>Strömmar (minst ett program körs) |Ja |
| Stoppas |Stoppas |Nej (övergångsläge) |
| Stoppad |Stoppad |Nej |

> [!NOTE]
> För närvarande hello kanal start medelvärde är ungefär 2 minuter, men ibland kan ta upp too20 + minuter. Återställer kanal kan ta upp too5 minuter.
> 
> 

## <a id="Considerations"></a>Överväganden
* När en kanal för **Standard** kodningstypen påträffar en förlust av inkommande källa/bidrag feed, den kompenserar den genom att ersätta hello källa ljud och video med ett fel mallen och tystnad. hello kanal fortsätter tooemit en mallen tills hello indata/bidrag feed återupptar. Vi rekommenderar att en levande kanal inte lämnas i sådant tillstånd under längre tid än 2 timmar. Hello funktionssätt hello kanal på inkommande återanslutning kan inte garanteras förbi den punkten, är varken sitt beteende i svaret tooa Återställ kommando. Du har toostop hello kanal, ta bort den och skapa en ny.
* Du kan inte ändra hello indataprotokollet när hello kanalen eller dess associerade program körs. Om du behöver olika protokoll får du skapa separata kanaler för varje indataprotokoll.
* Varje gång du konfigurera om hello livekodaren anropa hello **återställa** metod på hello-kanal. Innan du återställer hello kanal har toostop hello program. Starta om programmet hello när du har återställt hello-kanal.
* En kanal kan stoppas endast när den är i hello Körstatus och alla program på hello kanalen har stoppats.
* Som standard kan du bara lägga till 5 kanaler tooyour Media Services-konto. Det här är en icke-begränsande kvot för alla nya konton. Mer information finns i [kvoter och begränsningar](media-services-quotas-and-limitations.md).
* Du kan inte ändra hello indataprotokollet när hello kanalen eller dess associerade program körs. Om du behöver olika protokoll får du skapa separata kanaler för varje indataprotokoll.
* Du debiteras endast när din kanal är i hello **kör** tillstånd. Mer information finns för[detta](media-services-manage-live-encoder-enabled-channels.md#states) avsnitt.
* Hej max är för närvarande rekommenderas varaktighet för en direktsänd händelse 8 timmar. Kontakta amslived på Microsoft.com om du behöver toorun en kanal under längre perioder.
* Kontrollera att toohave hello strömningsslutpunkt som du vill toostream innehållet i hello **kör** tillstånd.
* När mata in flera språkspår och utföra live encoding med Azure, endast RTP stöds för flera språk indata. Du kan definiera upp too8 ljudströmmar med MPEG-2 TS över RTP. Vill föra in flera ljud spår med RTMP eller Smooth streaming stöds inte för närvarande. När du gör live encoding med [lokalt live kodar](media-services-live-streaming-with-onprem-encoders.md), det finns ingen sådan begränsning eftersom det skickas tooAMS passerar genom en kanal utan vidare bearbetning.
* hello encoding förinställt använder hello begreppet ”max bildfrekvens” på 30 fps. Så om hello indata 60fps / 59.97i, hello inkommande ramar är bort/de-interlaced too30/29,97 fps. Om hello-indata är 50fps/50i, är hello inkommande ramar bort/de-interlaced too25 fps. Om hello indata är 25 bildrutor förblir utdata 25 bildrutor per sekund.
* Glöm inte tooSTOP din kanaler när du är klar. Om du inte gör fortsätter fakturering.

## <a name="known-issues"></a>Kända problem
* Tid att starta kanalen har förbättrats tooan medelvärde 2 minuter, men ibland med ökad efterfrågan fortfarande kan ta upp too20 + minuter.
* Stöd för RTP är behov tillgodoses ordentligt mot professional sändningsföretag. Granska hello not RTP i [detta](https://azure.microsoft.com/blog/2015/04/13/an-introduction-to-live-encoding-with-azure-media-services/) blogg.
* En bilder bör följa toorestrictions beskrivs [här](media-services-manage-live-encoder-enabled-channels.md#default_slate). Om du försöker skapa en kanal med en en som är större än 1 920 x 1 080 hello begäran kommer så småningom standardfelet ut.
* Återigen... Glöm inte tooSTOP din kanaler när du är klar strömning. Om du inte gör fortsätter fakturering.

## <a name="next-step"></a>Nästa steg
Granska sökvägarna för Media Services-utbildning.

[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Ge feedback
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

## <a name="related-topics"></a>Relaterade ämnen
[Leverera händelser via strömning Live med Azure Media Services](media-services-overview.md)

[Skapa kanaler som utför live encoding från en enskilt bithastighet tooadaptive bithastighet ström med portalen](media-services-portal-creating-live-encoder-enabled-channel.md)

[Skapa kanaler som utför live encoding från en enskilt bithastighet tooadaptive bithastighet ström med .NET SDK](media-services-dotnet-creating-live-encoder-enabled-channel.md)

[Hantera kanaler med REST API](https://docs.microsoft.com/rest/api/media/operations/channel)
 
[Media Services-begrepp](media-services-concepts.md)

[Azure Media Services fragmenterad MP4 Live infognings-specifikationen](media-services-fmp4-live-ingest-overview.md)

[live-overview]: ./media/media-services-manage-live-encoder-enabled-channels/media-services-live-streaming-new.png

