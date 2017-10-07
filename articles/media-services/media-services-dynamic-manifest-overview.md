---
title: aaaFilters och dynamiska manifesten | Microsoft Docs
description: "Det här avsnittet beskrivs hur toocreate filtrerar så att klienten kan använda dem toostream vissa delar av en dataström. Media Services skapar dynamiska manifesten tooarchive denna selektiv strömning."
services: media-services
documentationcenter: 
author: cenkdin
manager: cfowler
editor: 
ms.assetid: ff102765-8cee-4c08-a6da-b603db9e2054
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: ne
ms.topic: article
ms.date: 06/29/2017
ms.author: cenkd;juliako
ms.openlocfilehash: 9527a011438c11da07a363d701ea736414412ecf
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="filters-and-dynamic-manifests"></a>Filter och dynamiska manifest
Från och med 2.11 Media Services kan du toodefine filter för dina tillgångar. Dessa filter är server side regler som tillåter toochoose toodo sådant som dina kunder: uppspelning endast en del av en video (i stället för att spela upp hello hela video), eller ange bara en delmängd av ljud och video återgivningar att kundens enhet kan hantera () i stället för alla hello återgivningar som har associerats med hello tillgången). Den här filtreringen dina tillgångar arkiveras via **dynamiska Manifest**som skapas vid kundens begäran toostream en video baserat på angivna filter.

Detta avsnitt beskriver vanliga scenarier som använder filter skulle vara mycket bra tooyour kunder och länkar tootopics som visar hur toocreate filtrerar programmässigt (för närvarande kan du skapa filter med REST API: er endast).

## <a name="overview"></a>Översikt
När du levererar ditt innehåll toocustomers (händelser via strömning live eller video-on-demand) målet är toodeliver en hög kvalitet video toovarious enheter under olika nätverksförhållanden. tooachieve det här målet hello följande:

* koda din dataström toomulti bithastighet ([anpassningsbar bithastighet](http://en.wikipedia.org/wiki/Adaptive_bitrate_streaming)) video-ström (Detta tar hand om kvalitet och nätverk) och 
* Använd Media Services [dynamisk paketering](media-services-dynamic-packaging-overview.md) toodynamically Ompaketera din ström till olika protokoll (Detta tar hand om strömning på olika enheter). Media Services stöder leverans av hello följande strömningstekniker med anpassningsbar bithastighet: HTTP Live Streaming (HLS), Smooth Streaming och MPEG DASH. 

### <a name="manifest-files"></a>Manifest-filer
När du koda en tillgång för anpassningsbar bithastighet strömning en **manifest** skapas (spelningslista)-fil (hello-filen är text- eller XML-baserade). Hej **manifest** filen innehåller strömning metadata som: spåra typ (ljud, video eller text), spåra namn, start-och sluttid, bithastighet (Egenskaper), spåra språk, presentationsfönstret (skjutfönster fast varaktighet), video codec (FourCC). Det gör även hello player tooretrieve hello nästa fragment ger information om hello nästa spelas upp video fragment tillgängliga och deras plats. Fragment (eller segment) är hello faktiska ”mängder” en videoinnehåll.

Här är ett exempel på en manifestfil: 

    <?xml version="1.0" encoding="UTF-8"?>    
    <SmoothStreamingMedia MajorVersion="2" MinorVersion="0" Duration="330187755" TimeScale="10000000">

    <StreamIndex Chunks="17" Type="video" Url="QualityLevels({bitrate})/Fragments(video={start time})" QualityLevels="8">
    <QualityLevel Index="0" Bitrate="5860941" FourCC="H264" MaxWidth="1920" MaxHeight="1080" CodecPrivateData="0000000167640028AC2CA501E0089F97015202020280000003008000001931300016E360000E4E1FF8C7076850A4580000000168E9093525" />
    <QualityLevel Index="1" Bitrate="4602724" FourCC="H264" MaxWidth="1920" MaxHeight="1080" CodecPrivateData="0000000167640028AC2CA501E0089F97015202020280000003008000001931100011EDC00002CD29FF8C7076850A45800000000168E9093525" />
    <QualityLevel Index="2" Bitrate="3319311" FourCC="H264" MaxWidth="1280" MaxHeight="720" CodecPrivateData="000000016764001FAC2CA5014016EC054808080A00000300020000030064C0800067C28000103667F8C7076850A4580000000168E9093525" />
    <QualityLevel Index="3" Bitrate="2195119" FourCC="H264" MaxWidth="960" MaxHeight="540" CodecPrivateData="000000016764001FAC2CA503C045FBC054808080A000000300200000064C1000044AA0000ABA9FE31C1DA14291600000000168E9093525" />
    <QualityLevel Index="4" Bitrate="1469881" FourCC="H264" MaxWidth="960" MaxHeight="540" CodecPrivateData="000000016764001FAC2CA503C045FBC054808080A000000300200000064C04000B71A0000E4E1FF8C7076850A4580000000168E9093525" />
    <QualityLevel Index="5" Bitrate="978815" FourCC="H264" MaxWidth="640" MaxHeight="360" CodecPrivateData="000000016764001EAC2CA50280BFE5C0548303032000000300200000064C08001E8480004C4B7F8C7076850A45800000000168E9093525" />
    <QualityLevel Index="6" Bitrate="638374" FourCC="H264" MaxWidth="640" MaxHeight="360" CodecPrivateData="000000016764001EAC2CA50280BFE5C0548303032000000300200000064C080013D60000C65DFE31C1DA1429160000000168E9093525" />
    <QualityLevel Index="7" Bitrate="388851" FourCC="H264" MaxWidth="320" MaxHeight="180" CodecPrivateData="000000016764000DAC2CA505067E7C054830303200000300020000030064C040030D40003D093F8C7076850A45800000000168E9093525" />

    <c t="0" d="20000000" /><c d="20000000" /><c d="20000000" /><c d="20000000" /><c d="20000000" /><c d="20000000" /><c d="20000000" /><c d="20000000" /><c d="20000000" /><c d="20000000" /><c d="20000000" /><c d="20000000" /><c d="20000000" /><c d="20000000" /><c d="20000000" /><c d="20000000" /><c d="9600000"/>
    </StreamIndex>


    <StreamIndex Chunks="17" Type="audio" Url="QualityLevels({bitrate})/Fragments(AAC_und_ch2_128kbps={start time})" QualityLevels="1" Name="AAC_und_ch2_128kbps">
    <QualityLevel AudioTag="255" Index="0" BitsPerSample="16" Bitrate="125658" FourCC="AACL" CodecPrivateData="1210" Channels="2" PacketSize="4" SamplingRate="44100" />

    <c t="0" d="20201360" /><c d="20201361" /><c d="20201360" /><c d="20201361" /><c d="20201360" /><c d="20201361" /><c d="20201360" /><c d="20201361" /><c d="20201360" /><c d="20201361" /><c d="20201360" /><c d="20201361" /><c d="20201361" /><c d="20201360" /><c d="20201361" /><c d="20201360" /><c d="6965987" /></StreamIndex>


    <StreamIndex Chunks="17" Type="audio" Url="QualityLevels({bitrate})/Fragments(AAC_und_ch2_56kbps={start time})" QualityLevels="1" Name="AAC_und_ch2_56kbps">
    <QualityLevel AudioTag="255" Index="0" BitsPerSample="16" Bitrate="53655" FourCC="AACL" CodecPrivateData="1210" Channels="2" PacketSize="4" SamplingRate="44100" />

    <c t="0" d="20201360" /><c d="20201361" /><c d="20201360" /><c d="20201361" /><c d="20201360" /><c d="20201361" /><c d="20201360" /><c d="20201361" /><c d="20201360" /><c d="20201361" /><c d="20201360" /><c d="20201361" /><c d="20201361" /><c d="20201360" /><c d="20201361" /><c d="20201360" /><c d="6965987" /></StreamIndex>

    </SmoothStreamingMedia>

### <a name="dynamic-manifests"></a>Dynamisk manifest
Det finns [scenarier](media-services-dynamic-manifest-overview.md#scenarios) när klienten behov bättre flexibilitet än vad som beskrivs i hello standard tillgången manifestfilen. Exempel:

* Enhetsspecifik: leverera endast hello angetts återgivningar och/eller angetts språkspår som stöds av hello-enhet som har använt tooplayback hello innehåll (”återgivning filtrering”). 
* Minska hello manifestet tooshow underordnade klipp för en direktsänd händelse (”underordnade clip filtrering”).
* Trim hello början på en video (”trimning en video”).
* Justera Presentation fönstret (DVR) i ordning tooprovide en begränsad tidslängd hello DVR fönster i hello player (”justera presentationsfönster”).

tooachieve denna flexibilitet, Media Services erbjuder **dynamiska visar** baserat på fördefinierade [filter](media-services-dynamic-manifest-overview.md#filters).  När du har definierat hello filter kan klienterna använda dem toostream en specifik återgivning eller underordnade klipp av videon. De vill ange filter i hello strömnings-URL. Filter kan tillämpas tooadaptive bithastighet strömning protokoll som stöds av [dynamisk paketering](media-services-dynamic-packaging-overview.md): HLS MPEG-DASH och Smooth Streaming. Exempel:

URL för MPEG DASH med filter

    http://testendpoint-testaccount.streaming.mediaservices.windows.net/fecebb23-46f6-490d-8b70-203e86b0df58/BigBuckBunny.ism/Manifest(format=mpd-time-csf,filter=MyLocalFilter)

Smooth Streaming URL med filtret

    http://testendpoint-testaccount.streaming.mediaservices.windows.net/fecebb23-46f6-490d-8b70-203e86b0df58/BigBuckBunny.ism/Manifest(filter=MyLocalFilter)


Mer information om hur toodeliver innehåll och bygga strömning URL: er, se [leverera Innehållsöversikt](media-services-deliver-content-overview.md).

> [!NOTE]
> Observera att dynamisk visar inte ändra hello tillgången och hello standardmanifestet för tillgången. Klienten kan välja toorequest en dataström med eller utan filter. 
> 
> 

### <a id="filters"></a>Filter
Det finns två typer av tillgångsinformation filter: 

* Globala filter (kan vara tillämpade tooany tillgångar i hello Azure Media Services-konto, har en livslängd på hello kontot) och 
* Lokala filter (kan bara vara tillämpade tooan tillgångsinformation med vilka hello filter kopplades när den har skapats, har en livslängd på hello tillgången). 

Globala och lokala filtyperna har exakt hello samma egenskaper. hello största skillnaden mellan hello två är vilken typ av arkiverar är lämpligast för vilka scenarier. Globala filter är normalt lämpliga för profiler för enheter (återgivning filtrering) där lokala filter kunde använda tootrim en specifik resurs.

## <a id="scenarios"></a>Vanliga scenarier
Som nämnts innan du levererar ditt innehåll toocustomers (händelser via strömning live eller video-on-demand) målet är toodeliver en video av hög kvalitet toovarious enheter under olika nätverket vid villkor. Dessutom din kan ha andra krav som inbegriper filtrering dina tillgångar och om hur du använder **dynamiska Manifest**s. hello följande avsnitt ger en kort översikt över olika scenarier för filtrering.

* Ange endast en delmängd av ljud och video återgivningar som vissa enheter kan hantera (i stället för alla hello återgivningar som är associerade med hello tillgången). 
* Spela upp endast en del av en video (i stället för hello hela video).
* Justera DVR presentationsfönster.

## <a name="rendition-filtering"></a>Återgivning filtrering
Du kan välja tooencode tillgången toomultiple kodning profiler (H.264 baslinje, H.264 hög, AACL, AACH, Dolby Digital Plus) och flera olika bithastigheter kvalitet. Dock stöder inte alla klientenheter din tillgångs profiler och bithastighet. Äldre Android-enheter stöder till exempel bara H.264 baslinjen + AACL. Skicka högre bithastighet tooa enhet som inte kan hämta hello fördelar slösar bandbredd och enheten beräkning. Dessa enheter måste avkoda alla hello angivna information endast tooscale den ned för att visas.

Med dynamisk Manifest kan du skapa profiler för enhet, till exempel mobile, konsolen HD/SD, etc. och inkluderar hello spårar och egenskaper som du vill toobe en del av varje profil.

![Filtrera återgivning-exempel][renditions2]

I följande exempel hello, var en kodare används tooencode en mezzanine tillgång till sju ISO MP4s video återgivningar (från 180p too1080p). hello kodade tillgången kan dynamiskt paketeras i något av följande protokoll för dataströmmar hello: MPEG DASH, HLS och Smooth.  Överst hello i hello diagram, hello HLS manifest för hello tillgång utan filter visas (den innehåller alla sju återgivningar).  Hello längst ned till vänster visas hello HLS manifest toowhich som ett filter med namnet ”ott” har tillämpats. Hej ”ott” filter anger tooremove alla bithastighet nedan 1 Mbit/s, vilket resulterade i hello längst ned två olika tas bort i hello-svar.  I hello nedre höger hello HLS visas manifestet toowhich ett filter med namnet ”mobil” tillämpades. Hej ”mobil” filter anger tooremove återgivningar där hello lösning är större än 720p, vilket resulterade i hello två 1080p återgivningar tas bort.

![Återgivning filtrering][renditions1]

## <a name="removing-language-tracks"></a>Spårar för att ta bort språk
Dina tillgångar kan innehålla flera ljud språk engelska, spanska, franska, t.ex. Vanligtvis hello Player SDK chefer standard ljud Spåra markering och tillgängliga ljud spårar per användarens val. Det är svårt toodevelop sådana Player-SDK: er, kräver olika implementeringar över enhetsspecifika spelarramverk. Dessutom begränsas Player-API: er på vissa plattformar och inkluderar inte ljud markeringen funktion där användare inte kan välja eller ändra hello standard ljud spåra. Du kan styra hello beteende genom att skapa filter som bara innehåller önskade ljud språk med tillgångsinformation filter.

![Språk spårar filtrering][language_filter]

## <a name="trimming-start-of-an-asset"></a>Trimning start för en tillgång
I de flesta händelser via strömning live kör ansvariga för några test innan hello faktiska händelsen. De kan till exempel inkludera en mallen så här innan hello hello händelse: ”programmet börjar tillfälligt”. Om arkivering av programmet hello hello test- och en data också arkiveras och kommer att ingå i hello presentation. Men ska den här informationen inte visas toohello klienter. Du kan skapa tidsfiltret en start och ta bort hello oönskad data från hello manifest med dynamiska Manifest.

![Trimning start][trim_filter]

## <a name="creating-sub-clips-views-from-a-live-archive"></a>Skapa underordnade klipp (vyer) från en live-Arkiv
Många direktsända händelser är tidskrävande och live Arkiv kan innehålla flera händelser. När hello direktsänd händelse ends programföretag kanske toobreak upp hello live arkivet till logiska programmet startas och stoppa sekvenser. Sedan kan publicera separat dessa virtuella program utan efter bearbetning hello live Arkiv och inte skapa separata resurser (som inte kommer att få förmånen av hello befintliga cachelagrade fragment i hello CDN-nät). Exempel på sådana virtuella program (underordnade klipp) är hello kvartal football eller basketmatch, hello innings i baseball eller enskilda händelser i en här olympiad program.

Du kan skapa filter med start-och sluttider och skapa virtuella vyer över hello överkant live arkivet med dynamiska Manifest. 

![Underklipp filter][subclip_filter]

Filtrerade tillgångsinformation:

![Skidåkning][skiing]

## <a name="adjusting-presentation-window-dvr"></a>Justera presentationsfönstret (DVR)
För närvarande erbjuder Azure Media Services cirkulär Arkiv där hello varaktighet kan konfigureras mellan 5 minuter – 25 timmar. Manifestet filtrering kan vara används toocreate ett rullande fönster DVR över hello överkant hello Arkiv, utan att ta bort mediet. Det finns många scenarier där programföretag vill tooprovide ett begränsat DVR fönster som följer med hello live kant och på hello samtidigt ha ett större arkivering fönster. En broadcast kanske toouse hello data som ligger utanför hello DVR fönstret toohighlight klipp he\she kan tooprovide olika DVR windows eller för olika enheter. De flesta hello mobila enheter hantera inte exempelvis stora DVR windows (du kan ha ett två minuter DVR fönster för mobila enheter och 1 timme för skrivbordsklienter).

![DVR fönster][dvr_filter]

## <a name="adjusting-livebackoff-live-position"></a>Justera LiveBackoff (direktsända positionen)
Manifestet filtrering kan vara används tooremove några sekunder från hello live kanten av en direktsänd program. Detta gör att programföretag toowatch hello presentation på hello preview publikationen peka och skapa annonsen infogas punkter innan hello visningsprogram får hello dataström (vanligtvis backas upp av med 30 sekunder). Programföretag kan sedan skicka dessa annonser tootheir klienten ramverk i tid för dem tooreceived och processen hello information innan hello annons möjlighet.

Dessutom toohello annons stöd, LiveBackoff kan användas för att justera klienten live download position så att när klienter driva och träffar hello live kant kan de fortfarande få fragment från servern i stället för 404 eller 412 HTTP-fel.

![livebackoff_filter][livebackoff_filter]

## <a name="combining-multiple-rules-in-a-single-filter"></a>Kombinera flera regler i ett enda filter
Du kan kombinera flera filtreringsregler i ett enda filter. Du kan definiera intervallet regeln tooremove mallen från en live-Arkiv och även filtrera tillgänglig bithastighet som exempel. Resultatet är hello sammansättning (skärningspunkten) av reglerna för flera filtrering regler hello slutet.

![flera regler][multiple-rules]

## <a name="create-filters-programmatically"></a>Skapa filter programmässigt
hello följande ämne beskriver Media Services-enheter som är relaterade toofilters. hello avsnittet visar även hur tooprogrammatically skapa filter.  

[Skapa filter med REST API: er](media-services-rest-dynamic-manifest.md).

## <a name="combining-multiple-filters-filter-composition"></a>Kombinera flera filter (filter sammansättning)
Du kan också kombinera flera filter i en enskild URL. 

hello visar följande scenario varför du kanske vill toocombine filter:

1. Behöver du toofilter din video egenskaper för mobila enheter, till exempel Android eller iPAD (i ordning toolimit video egenskaper). tooremove Hej oönskade egenskaper, skulle du skapa ett globalt filter som är lämplig för profiler för enheter. Som nämnts ovan är globala filter kan användas för alla tillgångar under hello samma media services-konto utan några ytterligare association. 
2. Du vill tootrim hello start och sluttiden för en tillgång. tooachieve, skulle du skapa ett lokalt filter och hello Starttid/Sluttid. 
3. Du vill att dessa filter toocombine båda (utan kombination behövs tooadd kvalitet filtrering toohello trimning filter som gör filter användning svårt).

toocombine filter måste tooset hello filter namn toohello manifest/spelningslista URL: en med semikolonavgränsad. Antar vi att du har ett filter med namnet *MyMobileDevice* som filtrerar egenskaper och du har en annan med namnet *MyStartTime* tooset en specifik starttid. Du kan kombinera dem så här:

    http://teststreaming.streaming.mediaservices.windows.net/3d56a4d-b71d-489b-854f-1d67c0596966/64ff1f89-b430-43f8-87dd-56c87b7bd9e2.ism/Manifest(filter=MyMobileDevice;MyStartTime)

Du kan kombinera in too3 filter. 

Mer information finns i [detta](https://azure.microsoft.com/blog/azure-media-services-release-dynamic-manifest-composition-remove-hls-audio-only-track-and-hls-i-frame-track-support/) blogg.

## <a name="know-issues-and-limitations"></a>Information om problem och begränsningar
* Dynamisk manifestet fungerar i GOP gränser (nyckeln ramar) därför trimning har GOP noggrannhet. 
* Du kan använda samma filnamn för lokala och globala filter. Observera den lokala filtret har högre prioritet och åsidosätter globala filter.
* Om du uppdaterar ett filter kan ta det upp too2 minuter för strömmande slutpunkt toorefresh hello regler. Om hello innehåll behandlades använda filter (och cachelagras i proxyservrar och CDN cacheminnen) kan uppdaterar dessa filter resultera i player-fel. Rekommenderas tooclear hello cachen när du har uppdaterat hello filter. Om det här alternativet inte är möjligt bör du använda ett annat filter.

## <a name="media-services-learning-paths"></a>Sökvägar för Media Services-utbildning
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Ge feedback
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

## <a name="see-also"></a>Se även
[Leverera innehåll tooCustomers översikt](media-services-deliver-content-overview.md)

[renditions1]: ./media/media-services-dynamic-manifest-overview/media-services-rendition-filter.png
[renditions2]: ./media/media-services-dynamic-manifest-overview/media-services-rendition-filter2.png

[rendered_subclip]: ./media/media-services-dynamic-manifests/media-services-rendered-subclip.png
[timeline_trim_event]: ./media/media-services-dynamic-manifests/media-services-timeline-trim-event.png
[timeline_trim_subclip]: ./media/media-services-dynamic-manifests/media-services-timeline-trim-subclip.png

[multiple-rules]:./media/media-services-dynamic-manifest-overview/media-services-multiple-rules-filters.png

[subclip_filter]: ./media/media-services-dynamic-manifest-overview/media-services-subclips-filter.png
[trim_event]: ./media/media-services-dynamic-manifests/media-services-timeline-trim-event.png
[trim_subclip]: ./media/media-services-dynamic-manifests/media-services-timeline-trim-subclip.png
[trim_filter]: ./media/media-services-dynamic-manifest-overview/media-services-trim-filter.png
[redered_subclip]: ./media/media-services-dynamic-manifests/media-services-rendered-subclip.png
[livebackoff_filter]: ./media/media-services-dynamic-manifest-overview/media-services-livebackoff-filter.png
[language_filter]: ./media/media-services-dynamic-manifest-overview/media-services-language-filter.png
[dvr_filter]: ./media/media-services-dynamic-manifest-overview/media-services-dvr-filter.png
[skiing]: ./media/media-services-dynamic-manifest-overview/media-services-skiing.png
