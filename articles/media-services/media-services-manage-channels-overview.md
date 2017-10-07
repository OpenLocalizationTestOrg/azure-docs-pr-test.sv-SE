---
title: "aaaOverview för Liveströmning med Azure Media Services | Microsoft Docs"
description: "Det här avsnittet ger en översikt över direktsänd strömning med Azure Media Services."
services: media-services
documentationcenter: 
author: Juliako
manager: cfowler
editor: 
ms.assetid: fb63502e-914d-4c1f-853c-4a7831bb08e8
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: ne
ms.topic: article
ms.date: 06/29/2017
ms.author: juliako
ms.openlocfilehash: edc49069db6b491902bdcbb808b1974858cc92f1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="overview-of-live-streaming-using-azure-media-services"></a>Översikt över Liveströmning med Azure Media Services
## <a name="overview"></a>Översikt
När du levererar live ingår streaming händelser med Azure Media Services hello följande komponenter oftast:

* En kamera som används toobroadcast en händelse.
* En live-videokodare som konverterar signaler från hello kamera toostreams som skickas tooa live streaming service.

    Du kan också välja flera kodare som synkroniserar tiden live. För vissa viktiga live-händelser som kräver mycket hög tillgänglighet och kvalitet, bör tooemploy aktiv / aktiv-kodare med tiden synkronisering tooachieve sömlös redundans utan att förlora data.
* En liveströmningstjänst som gör att du toodo hello följande:

  * infoga innehållet via olika protokoll för liveuppspelning (till exempel RTMP eller Smooth Streaming)
  * (valfritt) koda strömmen till ström med anpassningsbar bithastighet
  * förhandsgranska din liveuppspelning
  * post och store hello inhämtas innehåll i ordning toobe strömma senare (Video-on-Demand)
  * leverera hello innehållet via vanliga strömningsprotokoll (till exempel MPEG DASH, Smooth, HLS) direkt tooyour kunder eller tooa innehåll innehållsleveransnätverk (CDN) för vidare distribution.

**Microsoft Azure Media Services** (AMS) ger hello möjlighet tooingest, koda, förhandsgranska, lagra och leverera liveströmmat innehåll.

När du levererar ditt innehåll toocustomers målet är toodeliver en hög kvalitet video toovarious enheter under olika nätverksförhållanden. tooachieve, Använd direktsänd kodare tooencode din video dataström för dataströmmen tooa flera bithastigheter (anpassningsbar bithastighet).  tootake vård strömning på olika enheter, använder Media Services [dynamisk paketering](media-services-dynamic-packaging-overview.md) toodynamically Ompaketera din ström toodifferent protokoll. Media Services stöder leverans av hello följande strömningstekniker med anpassningsbar bithastighet: HTTP Live Streaming (HLS), Smooth Streaming, MPEG DASH.

I Azure Media Services **kanaler**, **program**, och **Strömningsslutpunkter** referensen alla hello direktsänd strömning funktioner, inklusive infogande, formatering, DVR, säkerhet, skalbarhet och redundans.

En **kanal** representerar en pipeline för bearbetning av liveuppspelningsinnehåll. En kanal kan ta emot en live indataström i hello följande sätt:

* En lokal livekodare skickar multibithastighet **RTMP** eller **Smooth Streaming** (fragmenterad MP4) toohello kanal som är konfigurerad för **direkt** leverans. Hej **direkt** leveransen är när hello inhämtas strömmarna passerar genom **kanal**s utan vidare bearbetning. Du kan använda följande livekodare som skickar Smooth Streaming i flera bithastigheter hello: MediaExcel, Ateme, anta kommunikation, Envivio, Cisco och Elemental. hello följande livekodare skickar RTMP: Adobe Flash Media Live-kodare (FMLE), Telestream Wirecast, Haivision, Teradek och Tricaster-omkodare.  En livekodare kan även skicka en enkel bithastighet dataströmmen tooa kanal som inte har aktiverats för live encoding, men det rekommenderas inte. På begäran levererar Media Services hello dataströmmen toocustomers.

  > [!NOTE]
  > Genomströmningsmetoden är hello mest ekonomiska sättet toodo live strömmande när du utför flera händelser under en längre tid och du redan har investerat i lokala kodare. Se [prisuppgifter](https://azure.microsoft.com/pricing/details/media-services/).
  > 
  > 
* En lokal livekodare skickar en dataström med enkel bithastighet toohello kanal som är aktiverade tooperform live encoding med Media Services i något av följande format hello: RTMP eller Smooth Streaming (fragmenterad MP4). RTP (MPEG-TS) stöds också, förutsatt att du har en dedicerad anslutning toohello Azure-datacenter. följande livekodare med RTMP hello utdata är kända toowork med kanaler av den här typen: Telestream Wirecast, FMLE. hello kanalen utför sedan live encoding av hello inkommande dataströmmen tooa flera bithastigheter (anpassningsbar) video dataström med enkel bithastighet. På begäran levererar Media Services hello dataströmmen toocustomers.

Från och med hello Media Services 2.10 versionen när du skapar en kanal kan ange du hur du vill använda för Indataströmmen din kanal tooreceive hello och om huruvida du vill använda för hello kanal tooperform live encoding av strömmen. Du kan välja mellan två alternativ:

* **Ingen** (genomströmning) – Ange det här värdet om du planerar toouse en lokal livekodare som kommer att skrivas ut flera bithastigheter dataström (en direkt stream). I det här fallet hello inkommande dataström passerat toohello utdata utan kodning. Detta görs hello av en kanal tidigare too2.10 utgåva.  
* **Standard** – Välj det här värdet om du planerar toouse Media Services tooencode toomulti bithastighet strömmen direktsänd dataström med enkel bithastighet. Den här metoden är mer ekonomiskt för snabbt skala upp för sällan händelser. Tänk på att det finns en fakturering påverkan för live encoding och ska du komma ihåg att och en live encoding kanal i hello ”körs” tillstånd kommer avgifter faktureringsinformation.  Vi rekommenderar att du stoppar du omedelbart dina kanaler som körs efter din direktsänd strömning händelse är klar tooavoid timvis extra kostnader.

## <a name="comparison-of-channel-types"></a>Jämförelse av kanaltyper
Följande tabell innehåller en guide toocomparing hello två kanaltyper stöds i Media Services

| Funktion | Direkt-kanal | Standard-kanal |
| --- | --- | --- |
| Enkel bithastighet indata kodas till flera olika bithastigheter i hello moln |Nej |Ja |
| Maximal upplösning, antalet lager |1080p, 8 lager 60 + fps |720p, 6 lager 30 fps |
| Inkommande protokoll |RTMP, Smooth Streaming |RTMP, Smooth Streaming och RTP |
| Pris |Se hello [sida med priser](https://azure.microsoft.com/pricing/details/media-services/) och klicka på fliken ”Live Video” |Se hello [sida med priser](https://azure.microsoft.com/pricing/details/media-services/) |
| Maximal körtid |24 x 7 |8 timmar |
| Stöd för att infoga pekdatorer |Nej |Ja |
| Stöd för ad-signalering |Nej |Ja |
| Direkt CEA 608/708 etiketter |Ja |Ja |
| Möjlighet toorecover från kort bås i bidrag feed |Ja |Nej (kanalen kommer att börja slating efter 6 + sekunder utan indata) |
| Stöd för icke-enhetlig inkommande GOPs |Ja |Nej – indata måste åtgärdas 2 sek GOPs |
| Stöd för variabeln ram hastighet indata |Ja |Nej – indata som måste åtgärdas bildfrekvens.<br/>Mindre variationer användas, till exempel under hög rörelse i bakgrunden. Men kodaren går inte att släppa too10 bildrutor per sekund. |
| Auto-avslutning av kanaler när indata-feeden går förlorad |Nej |Efter 12 timmar, om det finns inga Program som körs |

## <a name="working-with-channels-that-receive-multi-bitrate-live-stream-from-on-premises-encoders-pass-through"></a>Arbeta med kanaler som tar emot liveström med flera bithastigheter från lokala kodare (genomströmning)
hello följande diagram visar hello huvudsakliga delarna i hello AMS-plattformen som ingår i hello **direkt** arbetsflöde.

![Live-arbetsflöde](./media/media-services-live-streaming-workflow/media-services-live-streaming-current.png)

Mer information finns i [Arbeta med kanaler som tar emot liveström med flera bithastigheter från lokala kodare](media-services-live-streaming-with-onprem-encoders.md).

## <a name="working-with-channels-that-are-enabled-tooperform-live-encoding-with-azure-media-services"></a>Arbeta med kanaler som är aktiverad tooperform live encoding med Azure Media Services
hello följande diagram visar hello större delar av hello AMS-plattformen som ingår i arbetsflödet för Liveströmning där en kanal är aktiverad tooperform live encoding med Media Services.

![Live-arbetsflöde](./media/media-services-live-streaming-workflow/media-services-live-streaming-new.png)

Mer information finns i [arbeta med kanaler som är aktiverad tooPerform Live Encoding med Azure Media Services](media-services-manage-live-encoder-enabled-channels.md).

## <a name="description-of-a-channel-and-its-related-components"></a>Beskrivning av en kanal och dess relaterade komponenter
### <a name="channel"></a>Kanal
I Media Services [kanal](https://docs.microsoft.com/rest/api/media/operations/channel)s ansvarar för bearbetning av liveströmmat innehåll. En kanal som tillhandahåller en slutpunkt för indata (infognings-URL) som du sedan ange tooa live transcoder. hello kanal tar emot live indata från hello live transcoder och gör den tillgänglig för strömning via en eller flera Strömningsslutpunkter. Kanaler ger också en förhandsgranskningsslutpunkten (förhandsgranskning URL) om du använder toopreview och verifiera strömmen innan ytterligare bearbetning och leverans.

Du kan hämta hello infognings-URL och hello förhandsgransknings-URL när du skapar hello-kanal. tooget dessa URL: er hello kanal har inte toobe i hello igång tillstånd. När du är klar toostart push-överföring av data från en levande transcoder till hello kanal måste hello kanal vara igång. När hello live transcoder startar mata in data, kan du förhandsgranska dataströmmen.

Varje Media Services-konto kan innehålla flera kanaler, flera program och flera Strömningsslutpunkter. Beroende på hello bandbredds- och behov kan StreamingEndpoint tjänster vara dedikerade tooone eller flera kanaler. Alla StreamingEndpoint dra från varje kanal.

### <a name="program"></a>Program
En [programmet](https://docs.microsoft.com/rest/api/media/operations/program) kan du toocontrol hello publicering och lagring av segment i en direktsänd dataström. Kanaler hanterar program. hello är relationen mellan kanal och Program mycket lik tootraditional media där en kanal har en konstant ström av innehåll och ett program är begränsade toosome tidsinställd händelse på kanalen.
Du kan ange hello antal timmar som du vill att tooretain hello registreras innehåll för programmet hello genom att ange hello **ArchiveWindowLength** egenskapen. Det här värdet kan anges från minst 5 minuter tooa högst 25 timmar.

ArchiveWindowLength avgör också hello hur lång tid som klienter kan söka bakåt i tiden från hello aktuella direktsända positionen. Program kan köras under hello angiven tidsperiod, men innehåll som ligger bakom hello längd ignoreras kontinuerligt. Det här värdet för den här egenskapen avgör också hur länge hello klient manifest kan växa.

Varje program är associerat med en tillgång. toopublish hello program måste du skapa en positionerare för hello associerade tillgången. Med den här positioneraren kan toobuild en strömnings-URL som du kan tillhandahålla tooyour klienter.

En kanal har stöd för upp toothree program som körs samtidigt så du kan skapa flera Arkiv för hello samma inkommande dataström. Detta ger dig toopublish och arkivera olika delar av en händelse efter behov. Till exempel är dina affärsbehov tooarchive 6 timmar av ett program, men toobroadcast sista 10 minuter. tooaccomplish detta, behöver du toocreate två program som körs samtidigt. Ett program är tooarchive 6 timmar av händelsen hello men hello programmet publiceras inte. hello kan inte ange tooarchive i 10 minuter och det här programmet har publicerats.

## <a name="billing-implications"></a>Konsekvenser för fakturering
En kanal börjar fakturering som sitt tillstånd övergångar för ”körs” via hello API.  

hello följande tabell visar hur kanal tillstånd mappa toobilling tillstånd i hello API och Azure-portalen. Observera att hello tillstånd är något annorlunda mellan hello API och portalen UX. När en kanal har tillståndet hello ”körs” via hello API eller i hello ”klar” eller ”Streaming” tillstånd i hello Azure-portalen, fakturering kommer att vara aktiv.

toostop hello kanal från fakturering du ytterligare, har du tooStop hello kanal via hello API eller hello Azure-portalen.
Du ansvarar för att stoppa dina kanaler när du är klar med hello-kanal. Fel toostop hello kanal leder fortsatt fakturering.

> [!NOTE]
> När du arbetar med vanliga kanaler automatiskt AMS avslutning någon kanal som är fortfarande tillståndet ”körs” 12 timmar efter hello inkommande feeden går förlorade och inga program som körs. Men debiteras du fortfarande för hello tid hello kanal har tillståndet ”körs”.
>
>

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

## <a name="media-services-learning-paths"></a>Sökvägar för Media Services-utbildning
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Ge feedback
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

## <a name="related-topics"></a>Relaterade ämnen
[Azure Media Services fragmenterad MP4 Live infognings-specifikationen](media-services-fmp4-live-ingest-overview.md)

[Arbeta med kanaler som är aktiverad tooPerform Live Encoding med Azure Media Services](media-services-manage-live-encoder-enabled-channels.md)

[Arbeta med kanaler som tar emot liveuppspelningar med flera bithastigheter från lokala kodare](media-services-live-streaming-with-onprem-encoders.md)

[Kvoter och begränsningar](media-services-quotas-and-limitations.md).  

[Media Services-begrepp](media-services-concepts.md)
