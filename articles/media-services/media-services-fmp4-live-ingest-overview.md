---
title: aaaAzure Media Services fragmenterad MP4 live infognings-specifikationen | Microsoft Docs
description: "Den här specifikationen beskriver hello-protokollet och format för fragmenterad MP4-baserade direktsänd strömning införandet för Azure Media Services. Du kan använda Azure Media Services toostream direktsända händelser och broadcast innehåll i realtid med Azure som hello molnplattform. Det här dokumentet beskriver också bästa praxis för att skapa mycket överflödiga och robusta live infognings-metoder."
services: media-services
documentationcenter: 
author: cenkdin
manager: cfowler
editor: 
ms.assetid: 43fac263-a5ea-44af-8dd5-cc88e423b4de
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/29/2017
ms.author: cenkd;juliako
ms.openlocfilehash: 0c191f8d6c5a595621feaba0e571fb984b666f34
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-media-services-fragmented-mp4-live-ingest-specification"></a>Azure Media Services fragmenterad MP4 live infognings-specifikationen
Den här specifikationen beskriver hello-protokollet och format för fragmenterad MP4-baserade direktsänd strömning införandet för Azure Media Services. Media Services tillhandahåller en liveströmningstjänst som kunder använder toostream direktsända händelser och broadcast innehåll i realtid med Azure som hello molnplattform. Det här dokumentet beskriver också bästa praxis för att skapa mycket överflödiga och robusta live infognings-metoder.

## <a name="1-conformance-notation"></a>1. Överensstämmelse notation
hello nyckelord ”måste”, ”får inte”, ”krävs”, ”skall”, ”skall inte”, ”SHOULD”, ”bör inte”, är ”rekommenderade”, ”maj” och ”valfritt” i det här dokumentet toobe tolkas som beskrivs i RFC 2119.

## <a name="2-service-diagram"></a>2. Diagram för tjänsten
hello följande diagram visar hello övergripande arkitektur hello direktsänd strömning service i Media Services:

1. En livekodare skickar live feeds toochannels som skapas och etablerats via hello Azure Media Services SDK.
2. Kanaler, program och strömningsslutpunkter i Media Services-referensen alla hello direktsänd strömning funktioner, inklusive infogande, formatering, moln DVR, säkerhet, skalbarhet och redundans.
3. Kunder kan du kan också välja toodeploy ett Azure Content Delivery Network lager mellan hello strömmande slutpunkt och hello slutpunkter på klienter.
4. Klienten slutpunkter ström från hello strömmande slutpunkten med hjälp av anpassningsbar strömning av HTTP-protokoll. Exempel: Microsoft Smooth Streaming, dynamiska anpassningsbar strömning via HTTP (TANKSTRECK eller MPEG-DASH) och Apple HTTP Live Streaming (HLS).

![infognings-flöde][image1]

## <a name="3-bitstream-format--iso-14496-12-fragmented-mp4"></a>3. Formatet för Bitstream – ISO 14496 12 fragmenterad MP4
hello kabelformat för direktsänd strömning mata in beskrivs i det här dokumentet är baserad på [ISO-14496-12]. En detaljerad förklaring av fragmenterad MP4-format och tillägg både för video-on-demand-filer och direktsänd strömning införandet finns [[MS-SSTR]](http://msdn.microsoft.com/library/ff469518.aspx).

### <a name="live-ingest-format-definitions"></a>Live infognings-format definitioner
hello beskrivs följande lista specialformat definitioner som gäller toolive mata in i Azure Media Services:

1. Hej **ftyp**, **Live Manifest serverrutan**, och **moov** rutor måste skickas med varje begäran (HTTP POST). Dessa rutor måste skickas hello början av hello dataströmmen och infognings-helst hello kodare måste ansluta tooresume dataströmmen. Mer information finns i avsnittet 6 i [1].
2. [1] avsnitt 3.3.2 definierar en valfri ruta som kallas **StreamManifestBox** för live mata in. På grund av toohello routning logiken för hello Azure belastningsutjämnare, är med hjälp av den här rutan föråldrad. hello rutan får inte vara när du vill föra in till Media Services. Om den här rutan finns ignoreras Media Services tyst det.
3. Hej **TrackFragmentExtendedHeaderBox** rutan som definierats i 3.2.3.2 i [1] måste finnas för varje fragment.
4. Version 2 av hello **TrackFragmentExtendedHeaderBox** kryssrutan ska använda toogenerate media segment som har identiska URL: er i flera datacenter. hello fragment fältet är REQUIRED mellan datacenter växling vid fel i index-baserad strömningsformat, till exempel Apple HLS och indexbaserade MPEG-DASH. tooenable mellan datacenter redundans hello fragment index måste synkroniseras mellan flera kodare och ökas med 1 för varje efterföljande media fragment även över kodare omstarter eller fel.
5. [1] avsnitt 3.3.6 definierar en ruta som kallas **MovieFragmentRandomAccessBox** (**mfra**) som kan skickas hello slutet av live införandet tooindicate slutet på dataströmmen (EOS) toohello kanal. På grund av toohello infognings-logiken för Media Services med EOS är föråldrad och hello **mfra** rutan för live införandet inte ska skickas. Om skickas ignoreras Media Services tyst det. tooreset hello tillstånd hello infognings-punkt, rekommenderar vi att du använder [kanal återställa](https://docs.microsoft.com/rest/api/media/operations/channel#reset_channels). Vi rekommenderar också att du använder [programmet avslutas](https://msdn.microsoft.com/library/azure/dn783463.aspx#stop_programs) tooend en presentation och dataströmmen.
6. hello MP4 fragment varaktigheten ska vara konstant, tooreduce hello storleken på hello klientmanifesten. En konstant MP4 fragment varaktighet förbättrar också klienten hämta heuristik via hello Upprepa taggar. hello varaktighet kan variera toocompensate för hastigheterna heltal.
7. hello MP4 fragment varaktigheten ska vara mellan 2 och cirka 6 sekunder.
8. MP4 Fragmentera tidsstämplar och index (**TrackFragmentExtendedHeaderBox** `fragment_ absolute_ time` och `fragment_index`) ska komma in i stigande ordning. Även om Media Services är flexibel tooduplicate fragment, har den begränsad möjlighet tooreorder fragment enligt toohello media tidslinje.

## <a name="4-protocol-format--http"></a>4. Protokollet formatet – HTTP
ISO fragmenterad MP4-baserade live infognings för Media Services använder en standard tidskrävande HTTP POST-begäran tootransmit kodad media data som paketeras i fragmenterad MP4 format toohello service. Varje HTTP POST skickar en fullständig fragmenterad MP4 bitstream (”stream”), från hello som börjar med rubriken rutorna (**ftyp**, **Live Manifest serverrutan**, och **moov** rutor), och fortsätter med en sekvens av fragment (**moof** och **mdat** rutor). URL-syntax för hello HTTP POST-begäran, finns i avsnittet 9.2 i [1]. Ett exempel på hello POST URL är: 

    http://customer.channel.mediaservices.windows.net/ingest.isml/streams(720p)

### <a name="requirements"></a>Krav
Här följer hello detaljerade krav:

1. hello kodaren ska starta hello broadcast genom att skicka en HTTP POST-begäran med en tom ”text” (nollängd innehåll) med hjälp av hello samma införandet URL. Detta kan hjälpa hello kodare snabbt identifiera om hello live införandet slutpunkten är giltig och om det inte finns någon autentisering eller andra villkor som krävs. Per HTTP-protokoll hello-server kan inte skicka tillbaka ett HTTP-svar tills hello hela förfrågan, inklusive hello POST brödtext tas emot. Eftersom hello tidskrävande en direktsänd händelse, utan det här steget hello kodare kanske inte kan toodetect eventuella fel tills den är klar skickar alla hello-data.
2. hello kodare måste hantera fel eller autentiseringsfrågor på grund av (1). Om (1) lyckas med svaret 200 fortsätta.
3. hello kodare måste starta en ny HTTP POST-begäran med hello fragmenterad MP4-dataström. hello nyttolasten måste börja med hello sidhuvud rutorna följt av fragment. Observera att hello **ftyp**, **Live Manifest serverrutan**, och **moov** rutorna (i ordning) måste skickas med varje begäran, även om hello kodare måste ansluta eftersom hello föregående begäran avslutades tidigare toohello slutet hello dataströmmen. 
4. hello kodare måste använda segmentvis överföringskodning för uppladdning av, eftersom det är omöjligt toopredict hello hela innehållslängden för hello live händelse.
5. När hello händelsen över, när du har skickat hello senaste fragment, måste avslutas hello kodare avslutas hello segmentvis överföringskodning meddelandesekvens (de flesta HTTP klientstackar hantera automatiskt). hello kodare måste vänta tills hello service tooreturn hello slutliga svarskoden och avbryta hello-anslutningen. 
6. hello kodare får inte använda hello `Events()` substantiv enligt beskrivningen i 9.2 i [1] för live införandet till Media Services.
7. Om hello HTTP POST-begäran avslutas eller tider med en TCP fel tidigare toohello end hello stream, hello-kodare måste utfärda en ny POST-begäran med hjälp av en ny anslutning och följ hello föregående krav. Dessutom måste hello kodare och skicka hello föregående två MP4-fragment för varje spår i hello dataströmmen och återuppta utan introduktion till en avvikelse i hello media tidslinje. Skicka hello två sista MP4 fragment för varje spår säkerställer att det finns inga data går förlorade. Med andra ord om en dataström som innehåller både ljud och video spåra och hello aktuell POST-begäran misslyckas, hello kodare måste ansluta och skicka hello två sista fragment för hello ljud spår, som tidigare har skickats, och två sista hello fragment för hello video spåra, som tidigare har skickats, tooensure att det finns inga data går förlorade. hello kodare måste upprätthålla en ”vidarebefordra” buffert på media fragment som skickas igen när anslutningen upprättas.

## <a name="5-timescale"></a>5. tidsrymd
[[MS-SSTR] ](https://msdn.microsoft.com/library/ff469518.aspx) beskriver hello användning av tidsrymd för **SmoothStreamingMedia** (avsnittet 2.2.2.1) **StreamElement** (avsnittet 2.2.2.3) **StreamFragmentElement**(Punkt 2.2.2.6) och **LiveSMIL** (punkt 2.2.7.3.1). Om hello tidsrymd värdet inte finns, är 10 000 000 (10 MHz) hello standardvärdet används. Även om hello Smooth Streaming formatspecifikationen inte blockerar användningen av andra tidsrymd värden, de flesta implementeringar av encoder använda denna standardinställning värde (10 MHz) toogenerate Smooth Streaming mata in data. På grund av toohello [Azure Media dynamisk paketering](media-services-dynamic-packaging-overview.md) funktion, rekommenderar vi att du använder en 90-KHz tidsrymd för videoströmmar och 44,1 eller 48.1 KHz för ljudströmmar. Om du använder olika tidsrymd värden för olika dataströmmar, måste hello dataströmmen nivå tidsrymd skickas. Mer information finns i [[MS-SSTR]](https://msdn.microsoft.com/library/ff469518.aspx).     

## <a name="6-definition-of-stream"></a>6. Definitionen för ”dataströmmen”
Dataströmmen är hello grundläggande åtgärd i live införandet för att skapa direktsända presentationer hantering strömmande växling vid fel och redundans scenarier. Dataströmmen har definierats som en unik, fragmenterad MP4-bitstream som kan innehålla en enda spåra eller flera band. En fullständig presentation kan innehålla en eller flera strömmar, beroende på hello konfiguration av hello livekodare. hello som följande exempel visar olika alternativ för att använda dataströmmar toocompose en fullständig presentation.

**Exempel:** 

En kund vill toocreate en direktsänd strömning presentation som innehåller följande ljud/video bithastighet hello:

Video – 3000 kbit/s, 1500 kbit/s, 750 kbit/s

Ljud – 128 kbit/s

### <a name="option-1-all-tracks-in-one-stream"></a>Alternativ 1: Alla spår i en dataström
I det här alternativet, en enda kodare genererar alla ljud/video spår och paketerar dem till en fragmenterad MP4-bitstream. hello fragmenterad MP4 bitstream skickas via en HTTP POST-anslutning. I det här exemplet finns bara en dataström för live presentationen.

![Dataströmmar-ett spåra][image2]

### <a name="option-2-each-track-in-a-separate-stream"></a>Alternativ 2: Varje spår i en separat ström
I det här alternativet hello kodare placerar en spårning i varje fragment MP4-bitstream och publicerar alla hello dataströmmar över separat HTTP-anslutningar. Detta kan göras med en kodare eller flera kodare. hello live införandet ser live presentationen som består av fyra dataströmmar.

![Dataströmmar separat spår][image3]

### <a name="option-3-bundle-audio-track-with-hello-lowest-bitrate-video-track-into-one-stream"></a>Alternativ 3: Paketera ljud spåra med hello lägsta bithastighet video spår till en dataström
I det här alternativet väljer hello kunden toobundle hello ljud spåra med hello lägsta bithastighet video spår i en fragment MP4 bitstream och lämna hello andra två video spår som separata dataströmmar. 

![Spårar dataströmmar ljud och video][image4]

### <a name="summary"></a>Sammanfattning
Detta är inte en fullständig förteckning över alla möjliga införandet alternativ för det här exemplet. Som ett faktiskt stöds grupperingen spår i dataströmmar av live införandet. Kunder och kodare leverantörer kan välja sina egna implementeringar baserat på engineering komplexitet, kodare kapacitet och redundans och överväganden för växling vid fel. I de flesta fall är det dock bara en ljud spåra hello hela live presentationen. Så det är viktigt tooensure hello hälsa hello infognings-dataström som innehåller hello ljud spåra. Den här beräkningen ofta resulterar i att placera hello ljud spåra i sin egen dataström (som alternativ 2) eller paketering med hello lägsta bithastighet video spår (som alternativ 3). Dessutom för bättre redundans och feltolerans skickar hello samma ljud spår i två olika dataströmmar (alternativ 2 med redundant ljud spår) eller sammanföra hello ljud spåra med minst två av hello lägsta bithastighet video spår (alternativ 3 med ljud paketerade i på minst två videoströmmar) rekommenderas för Direktmigrering mata in i Media Services.

## <a name="7-service-failover"></a>7. -Redundans
Det angivna hello karaktär för direktsänd strömning är bra redundansstöd viktiga för att säkerställa hello tillgänglighet av hello-tjänsten. Media Services är utformad toohandle olika typer av fel, inklusive nätverksfel, fel och problem med lagring. När den används tillsammans med rätt redundanslogiken från hello livekodaren sida, kan kunder uppnå en hög tillförlitlig tjänst för liveströmning från hello molnet.

I det här avsnittet diskuterar vi tjänsten redundanssituationer. I det här fallet hello fel händer någonstans i hello-tjänsten och den visar sig som ett nätverksfel. Här följer några rekommendationer för hello kodare implementering för att hantera redundans för tjänsten:

1. Använd en 10 sekunder timeout för att fastställa hello TCP-anslutning. Om ett försök tooestablish hello anslutningen tar längre än 10 sekunder, avbryta hello åtgärden och försök igen. 
2. Använd en kort tidsgräns för att skicka hello HTTP-begäran meddelandet segment. Om hello mål MP4 fragment varaktighet N sekunder Använd en skicka-timeout mellan N och 2 N: e sekund. till exempel om hello MP4 fragment varaktighet 6 sekunder, använda en tidsgräns på 6 too12 sekunder. Om en timeout uppstår infognings Återställ hello anslutning, öppna en ny anslutning och återuppta ström på hello ny anslutning. 
3. Underhålla en rullande buffert som har fragment för hello två sista för varje spår som har och helt skickades toohello service.  Om hello HTTP POST-begäran för en dataström avslutas eller timeout tidigare toohello slutet på dataströmmen för hello, öppna en ny anslutning och börjar en annan HTTP POST-begäran, skicka hello dataströmmen huvuden, skicka hello två sista fragment för varje spår och återuppta hello dataströmmen utan Introduktion till en avvikelse i hello media tidslinje. Detta minskar hello risken för dataförluster.
4. Vi rekommenderar att hello-kodaren inte begränsa hello antal återförsök tooestablish en anslutning eller återuppta strömning efter ett TCP-fel har inträffat.
5. Efter ett TCP-fel:
  
    a. hello aktuella anslutningen måste stängas och en ny anslutning måste skapas för en ny HTTP POST-begäran.

    b. hello nya HTTP POST URL måste vara hello samma som hello inledande POST URL.
  
    c. hello nya HTTP POST måste innehålla dataströmmen huvuden (**ftyp**, **Live Manifest serverrutan**, och **moov** rutor) som är identiska toohello dataströmmen huvuden i hello inledande POST.
  
    d. hello två sista fragment skickas för varje spår skickas och strömning måste fortsätta utan att en avvikelse i hello media tidslinje. hello MP4 fragment tidsstämplar öka kontinuerligt, även över HTTP POST-förfrågningar.
6. hello kodaren ska avsluta hello HTTP POST-begäran om data inte skickas med en hastighet som motsvarar hello MP4 fragment varaktighet.  En HTTP POST-begäran som inte skickar data kan förhindra Media Services snabbt kopplar från hello kodare i hello-händelse en tjänstuppdatering. Därför hello HTTP POST för sparse (ad signal) spårar bör vara kortvarigt och avslutar så snart hello sparse fragment skickas.

## <a name="8-encoder-failover"></a>8. kodaren växling vid fel
Kodaren redundans är hello andra typen av failover-scenariot som behöver åtgärdas toobe för slutpunkt till slutpunkt live strömmande leverans. I det här scenariot inträffar hello fel på hello kodare sida. 

![kodaren växling vid fel][image5]

hello följande förväntningar gäller från hello live införandet slutpunkten när kodaren redundans inträffar:

1. En ny instans av kodaren ska skapas toocontinue direktuppspelning, enligt beskrivningen i hello diagram (dataströmmen för 3000k video med streckad linje).
2. Det gick inte att instansen hello nya kodare måste använda hello samma URL: en för HTTP POST-begäranden som hello.
3. hello nya kodare POST-begäran måste innehålla hello samma fragmenterad MP4-huvud rutor som hello instans som misslyckats.
4. hello nya kodare måste synkroniseras korrekt med alla andra körs kodare för hello samma live presentation toogenerate synkroniseras ljud/video prover med justerade fragment gränser.
5. hello ny ström måste vara samma sak med hello tidigare stream, och utbytbara på hello sidhuvud och fragment nivåer.
6. hello nya kodare försök toominimize data går förlorade. Hej `fragment_absolute_time` och `fragment_index` av media fragment bör öka från hello punkt där hello kodare senast avbröts. Hej `fragment_absolute_time` och `fragment_index` bör öka i ett kontinuerligt sätt, men det är tillåtna toointroduce en avvikelse om det behövs. Media Services ignorerar fragment som den redan har tagit emot och bearbetas, så det är bättre tooerr längs hello skicka fragment än toointroduce avbrott hello media tidslinjen. 

## <a name="9-encoder-redundancy"></a>9. kodaren redundans
För vissa viktiga live-händelser att begäran ännu högre tillgänglighet och kvalitet rekommenderar vi att du använder aktiv / aktiv-kodare tooachieve sömlös redundans med inga data går förlorade.

![kodaren redundans][image6]

Enligt beskrivningen i det här diagrammet push två grupper av kodare två kopior av varje dataström samtidigt i hello live-tjänsten. Den här inställningen stöds eftersom Media Services kan filtrera ut dubbla fragment baserat på dataström-ID och fragment tidsstämpel. Hej resulterande direktsänd dataström och arkivet är en enda kopia av alla hello-dataströmmar som är hello bästa möjliga aggregering från hello två källor. Till exempel i extrema fall hypotetiska är så länge det finns en kodare (det inte är toobe hello samma) körs vid en viss tidpunkt för varje dataström hello resulterande direktsänd dataström från hello-tjänsten kontinuerligt utan dataförlust. 

hello kraven för det här scenariot är hello nästan samma som hello krav i hello ”kodare redundans” fall, med undantag för hello hello andra uppsättning kodare med i hello samma tid som hello primära kodare.

## <a name="10-service-redundancy"></a>10. tjänsten redundans
För hög redundant global distributionsplatsen måste ibland du ha mellan region säkerhetskopiering toohandle regionala katastrofer. Expandera hello ”kodare redundans” topologi kan kunder välja toohave en redundant tjänstdistribution i en annan region som är kopplad till hello andra uppsättning kodare. Kunder som också kan arbeta med en innehållsleveransnätverk providern toodeploy en Global Traffic Manager framför hello två service distributioner tooseamlessly väg klienttrafik. hello kraven för hello kodare är hello samma som hello ”kodare redundans” fallet. hello endast undantaget är hello andra uppsättning kodare behov toobe pekar tooa olika live infognings-slutpunkten. hello visar följande diagram den här installationen:

![tjänsten redundans][image7]

## <a name="11-special-types-of-ingestion-formats"></a>11. Särskilda typer av införandet format
Det här avsnittet beskriver särskilda typer av live införandet format som är utformad toohandle specifika scenarier.

### <a name="sparse-track"></a>Sparse spåra
När en presentation direktsänd strömning med en rich-klient-upplevelse, ofta det är nödvändigt tootransmit tid-synkroniserade händelser eller signalerar band med hello huvudsakliga media data. Ett exempel på detta är dynamiska live ad infogning. Den här typen av händelse-signalering skiljer sig från vanlig ljud/video streaming sin sparse natur. Med andra ord hello-signalering data vanligtvis sker inte kontinuerligt och hello-intervall kan vara svårt toopredict. hello begreppet sparse spåra var utformad tooingest och utsända in-band-signaling data.

hello är följande en rekommenderad implementering för att föra in sparse spåra:

1. Skapa en separat fragmenterad MP4-bitstream som innehåller endast sparse spårar utan ljud/video spår.
2. I hello **Live Manifest serverrutan** enligt definitionen i avsnittet 6 i [1], använda hello *parentTrackName* toospecify hello på parameternamn hello överordnade spåra. Mer information finns i avsnittet 4.2.1.2.1.2 i [1].
3. I hello **Live Manifest serverrutan**, **manifestOutput** måste anges för**SANT**.
4. Eftersom hello sparse hello signalering händelse, rekommenderar vi hello följande:
   
    a. Hello början av hello direktsänd händelse skickar hello kodare hello första huvud rutorna toohello-tjänster hello service tooregister hello sparse spåra i hello klienten manifest.
   
    b. hello kodaren ska upphöra hello HTTP POST-begäran om data inte skickas. En tidskrävande HTTP POST som inte skickar data kan förhindra Media Services snabbt kopplar från hello kodare hello service update eller server startas om för händelsen. I dessa fall har hello mediaserver blockerats tillfälligt i en receive-åtgärd på hello socket.
   
    c. Under hello tid signalering data inte är tillgängliga, bör hello kodare stänga hello HTTP POST-begäran. När hello POST-begäran är aktiv ska hello kodaren skicka data.

    d. När du skickar sparse fragment kan hello kodare ange ett explicit content-length-huvud, om den är tillgänglig.

    e. När du skickar sparse fragment med en ny anslutning ska hello kodaren börja skicka från hello sidhuvud rutorna följt av nya hello-fragment. Detta är i de fall i vilka failover händer emellan och hello ny sparse-anslutning används etablerade tooa ny server som inte har upptäckt hello sparse spåra innan.

    f. hello sparse spåra fragment blir tillgängliga toohello klient när hello motsvarande överordnade spåra fragment som har ett lika mycket eller större tidsstämpelvärde görs tillgängliga toohello klienten. Till exempel om hello sparse fragment har en tidsstämpel för t = 1000, förväntas som när hello klienten ser ”video” (förutsatt att hello överordnade spåra namn är ”video”) Fragmentera tidsstämpel 1000 eller senare, du kan ladda ned hello sparse fragment t = 1000. Observera att hello faktiska signal kan användas för en annan plats i hello presentation tidslinjen för den avsedda syften. I det här exemplet är det möjligt att hello sparse fragment av t = 1000 har en nyttolast för XML, vilket är för att infoga en annons i ett läge som ett par sekunder senare.

    g. hello nyttolasten för sparse spåra fragment kan vara i olika format (till exempel XML, text eller binär), beroende på hello scenario.

### <a name="redundant-audio-track"></a>Redundant ljud spåra
I ett typiskt HTTP anpassningsbar strömning scenario (till exempel Smooth Streaming eller TANKSTRECK), ofta finns bara ett ljud spår i hello hela presentationen. Till skillnad från video spår som har flera olika för hello klienten toochoose från felvillkor kan hello ljud spåra vara en enskild felpunkt om hello införandet av hello-dataström som innehåller hello ljud spåra har brutits. 

toosolve problemet Media Services stöder live införandet av redundanta ljud-spår. hello idé är att hello samma ljud spåra kan skickas flera gånger i olika dataströmmar. Även om hello tjänsten registrerar endast hello ljud spåra en gång i manifestet för hello-klienten, kan den använda redundant ljud spår som reserver för att hämta ljud fragment om hello primära ljud spåra har problem. tooingest redundant ljud spår hello kodare måste:

1. Skapa hello samma ljud spåra i flera fragment MP4 bitstreams. hello redundant ljud spårar måste vara samma sak, med hello samma fragment tidsstämplar och vara utbytbara på fragment-nivå och hello-huvud.
2. Se till att ”ljud” hello-posten i hello Live Server Manifest (avsnittet 6 i [1]) är hello samma för alla redundant ljud spår.

hello efter genomförandet rekommenderas för redundant ljud spårar:

1. Skicka varje unik ljud spår i en dataström med sig själv. Du kan också skicka en redundant dataström för var och en av dessa ljud spåra strömmar, där hello andra dataströmmen skiljer sig från hello först endast genom hello identifierare i hello HTTP POST-URL: {protokollet} :// {serveradress} / {publicera punkt path}/Streams({identifier}).
2. Använd separata dataströmmar toosend hello två lägsta video bithastighet. Var och en av dessa strömmar måste även innehålla en kopia av varje unik ljud spår. När flera språk stöds ska dessa strömmar innehålla ljud spår för varje språk.
3. Använda en separat server (kodare) instanser tooencode och skicka hello redundant dataströmmar som nämns i (1) och (2). 

## <a name="media-services-learning-paths"></a>Sökvägar för Media Services-utbildning
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Ge feedback
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

[image1]: ./media/media-services-fmp4-live-ingest-overview/media-services-image1.png
[image2]: ./media/media-services-fmp4-live-ingest-overview/media-services-image2.png
[image3]: ./media/media-services-fmp4-live-ingest-overview/media-services-image3.png
[image4]: ./media/media-services-fmp4-live-ingest-overview/media-services-image4.png
[image5]: ./media/media-services-fmp4-live-ingest-overview/media-services-image5.png
[image6]: ./media/media-services-fmp4-live-ingest-overview/media-services-image6.png
[image7]: ./media/media-services-fmp4-live-ingest-overview/media-services-image7.png
