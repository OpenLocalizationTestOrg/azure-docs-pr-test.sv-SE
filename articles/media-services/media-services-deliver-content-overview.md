---
title: "aaaDelivering innehåll toocustomers | Microsoft Docs"
description: "Det här avsnittet ger en översikt över vad som är involverad i att leverera ditt innehåll med Azure Media Services."
services: media-services
documentationcenter: 
author: Juliako
manager: cfowler
editor: 
ms.assetid: 89ede54a-6a9c-4814-9858-dcfbb5f4fed5
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/29/2017
ms.author: juliako
ms.openlocfilehash: 0570bd62d9d42633df0132f9449b357e2abb4086
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="deliver-content-toocustomers"></a>Leverera innehåll toocustomers
När du levererar strömning eller video-on-demand innehåll toocustomers vi är målet toodeliver högkvalitativ video toovarious enheter under olika nätverksförhållanden.

tooachieve det här målet, kan du:

* Koda strömmen tooa flera bithastigheter (anpassningsbar bithastighet) video strömmen. Detta hand tar om kvalitet och nätverk.
* Använda Microsoft Azure Media Services [dynamisk paketering](media-services-dynamic-packaging-overview.md) toodynamically Ompaketera din ström till olika protokoll. Detta hand tar om strömning på olika enheter. Media Services stöder leverans av hello följande strömningstekniker med anpassningsbar bithastighet: HTTP Live Streaming (HLS), Smooth Streaming och MPEG-DASH.

>[!NOTE]
>När AMS-kontot skapas en **standard** strömningsslutpunkt har lagts till tooyour konto i hello **stoppad** tillstånd. toostart strömning ditt innehåll och dra nytta av dynamisk paketering och dynamisk kryptering hello strömningsslutpunkt som du vill toostream innehåll har toobe i hello **kör** tillstånd. 

Den här artikeln ger en översikt över viktiga innehållsleverans begrepp.

toocheck kända problem, se [kända problem](media-services-deliver-content-overview.md#known-issues).

## <a name="dynamic-packaging"></a>Dynamisk paketering
Med hello dynamisk paketering som Media Services tillhandahåller, du kan leverera ditt innehåll MP4 eller Smooth Streaming-kodade med anpassad bithastighet i strömningsformat som stöds av Media Services (MPEG DASH, HLS, Smooth Streaming) utan att behöva toore paketet till Dessa strömningsformat. Vi rekommenderar att leverera ditt innehåll med dynamisk paketering.

tootake nytta av dynamisk paketering behöver du behöver tooencode din mezzaninfil (källa) till en uppsättning MP4-filer med anpassningsbar bithastighet eller Smooth Streaming-filer.

Med dynamisk paketering, lagra och betala för hello filer i ett enda lagringsformat. Media Services skapar och ger hello lämplig respons baserat på dina önskemål.

Dynamisk paketering är tillgänglig för standard- och premium strömningsslutpunkter. 

Mer information finns i [dynamisk paketering](media-services-dynamic-packaging-overview.md).

## <a name="filters-and-dynamic-manifests"></a>Filter och dynamiska manifest
Du kan definiera filter för dina tillgångar med Media Services. Dessa filter är serversidan regler som ger kunderna exempelvis spela upp ett visst avsnitt av en video eller ange en delmängd av ljud och video återgivningar som kundens enhet kan hantera (i stället för alla hello återgivningar som är associerade med hello tillgången ). Den här filtreringen uppnås genom *dynamiska manifesten* som skapas när kunden begär toostream en video baserat på en eller flera specifika filter.

Mer information finns i [filter och dynamiska manifesten](media-services-dynamic-manifest-overview.md).

## <a name="locators"></a>Lokaliserare
tooprovide din användare en URL som kan använda toostream eller hämta ditt innehåll, måste du först toopublish din tillgång genom att skapa en positionerare. En positionerare ger en post punkt tooaccess hello filer som finns i en tillgång. Media Services stöder två typer av lokaliserare:

* OnDemandOrigin-positionerare. Dessa används toostream Media (till exempel MPEG DASH, HLS eller Smooth Streaming) eller hämta progressivt filer.
* Lokaliserare för delad åtkomst (SAS)-signaturen URL. Dessa är används toodownload media filer tooyour lokal dator.

En *princip* är används toodefine hello behörigheterna (t.ex. Läs-, Skriv- och listan) och varaktighet som en klient har åtkomst för en viss resurs. Observera hello listan behörighet (AccessPermissions.List) inte ska användas i att skapa en positionerare OrDemandOrigin.

Lokaliserare har förfallodatum. hello Azure-portalen anger ett utgångsdatum 100 år i hello framtida för lokaliserare.

> [!NOTE]
> Om du har använt hello Azure portal toocreate lokaliserare före mars 2015 dessa lokaliserare har ställts in tooexpire efter två år.
> 
> 

tooupdate ett förfallodatum för en lokaliserare, Använd [REST](https://docs.microsoft.com/rest/api/media/operations/locator#update_a_locator) eller [.NET](http://go.microsoft.com/fwlink/?LinkID=533259) API: er. Observera att när du uppdaterar en SAS-lokaliserare hello utgångsdatum ändras hello-URL.

Lokaliserare har inte utformats för åtkomstkontroll för toomanage per användare. Du kan ge olika åtkomst rättigheter tooindividual användare med hjälp av Digital Rights Management (DRM) lösningar. Mer information finns i [skydda Media](http://msdn.microsoft.com/library/azure/dn282272.aspx).

När du skapar en positionerare kan finnas det en fördröjning på 30 sekunder på grund av toorequired lagring och spridningen processer i Azure Storage.

## <a name="adaptive-streaming"></a>Anpassad strömning
Anpassningsbar bithastighet tekniker Tillåt videospelarprogram toodetermine nätverksförhållanden och Välj flera bithastighet. När nätverkskommunikation försämrar kan hello klient välja en lägre bithastighet så uppspelning kan fortsätta med lägre bildkvaliteten. Eftersom nätverksförhållanden förbättra växla hello klienten tooa högre bithastighet med förbättrad bildkvaliteten. Azure Media Services stöder följande tekniker för anpassningsbar bithastighet hello: HTTP Live Streaming (HLS), Smooth Streaming och MPEG-DASH.

tooprovide användare med strömnings-URL: er kan du först skapa en OnDemandOrigin-positionerare. Skapa hello lokaliserare ger du hello bassökväg toohello tillgång som innehåller hello innehåll vill du toostream. Dock toobe kan toostream det här innehållet toomodify måste den här sökvägen ytterligare. en fullständig URL-toohello strömning manifestfilen måste du sammanfoga hello lokaliserare sökvägen tooconstruct värdet och hello Manifestfilens namn (filename.ism). Lägg sedan till **/Manifest** och en sökväg i korrekt format (vid behov) toohello lokaliserare.

> [!NOTE]
> Du kan också strömma ditt innehåll via en SSL-anslutning. toodo, kontrollera din strömmande URL: er starta med HTTPS. Observera att för närvarande AMS inte stöder SSL med anpassade domäner.  
> 


Du kan bara strömma via SSL om hello strömningsslutpunkt från vilken du kan leverera ditt innehåll skapades efter 10 September 2014. Om din strömmande URL: er som är baserade på hello strömningsslutpunkter som skapats efter 10 September 2014 innehåller hello URL ”streaming.mediaservices.windows.net”. Strömmande URL: er som innehåller ”origin.mediaservices.windows.net” (hello gamla format) stöder inte SSL. Om URL: en har hello gamla format och du vill toobe kan toostream via SSL, kan du skapa en ny strömmande slutpunkt. Använd webbadresserna baserat på hello strömning nya endpoint toostream ditt innehåll via SSL.

## <a name="streaming-url-formats"></a>Strömmande URL-format
### <a name="mpeg-dash-format"></a>MPEG-DASH-format
{strömmande slutpunkten namn media services-konto name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest(format=mpd-time-csf)

http://testendpoint-testaccount.Streaming.mediaservices.Windows.NET/fecebb23-46F6-490d-8B70-203e86b0df58/BigBuckBunny.ISM/manifest(format=mpd-Time-CSF)

### <a name="apple-http-live-streaming-hls-v4-format"></a>Apple HTTP Live Streaming (HLS) V4-format
{strömmande slutpunkten namn media services-konto name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest(format=m3u8-aapl)

http://testendpoint-testaccount.Streaming.mediaservices.Windows.NET/fecebb23-46F6-490d-8B70-203e86b0df58/BigBuckBunny.ISM/manifest(format=m3u8-aapl)

### <a name="apple-http-live-streaming-hls-v3-format"></a>Apple HTTP Live Streaming (HLS) V3-format
{strömmande slutpunkten namn media services-konto name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest(format=m3u8-aapl-v3)

http://testendpoint-testaccount.Streaming.mediaservices.Windows.NET/fecebb23-46F6-490d-8B70-203e86b0df58/BigBuckBunny.ISM/manifest(format=m3u8-aapl-v3)

### <a name="apple-http-live-streaming-hls-format-with-audio-only-filter"></a>Apple HTTP Live Streaming (HLS)-format med ljuddata filter
Som standard ingår endast ljud spårar i hello HLS manifest. Detta krävs för Apple Store certifikatutfärdare för mobila nätverk. Om en klient inte har tillräckligt med bandbredd eller är ansluten via en anslutning 2G, växlar i det här fallet uppspelning endast tooaudio. Detta gör tookeep innehåll strömning utan buffert, men bild ingen. I vissa fall kanske player buffring prioriterade över ljuddata. Om du vill tooremove hello ljuddata spåra lägga till **ljuddata = false** toohello URL.

http://testendpoint-testaccount.Streaming.mediaservices.Windows.NET/fecebb23-46F6-490d-8B70-203e86b0df58/BigBuckBunny.ISM/manifest(format=m3u8-aapl-v3,Audio-only=false)

Mer information finns i [dynamisk sammansättning i Manifest support och HLS utdata ytterligare funktioner](https://azure.microsoft.com/blog/azure-media-services-release-dynamic-manifest-composition-remove-hls-audio-only-track-and-hls-i-frame-track-support/).

### <a name="smooth-streaming-format"></a>Smooth Streaming-format
{namn på slutpunkt för direktuppspelning-namn på mediaservicekonto}.streaming.mediaservices.windows.net/lokalisator-ID}/{filnamn}.ism/Manifest

Exempel:

http://testendpoint-testaccount.Streaming.mediaservices.Windows.NET/fecebb23-46F6-490d-8B70-203e86b0df58/BigBuckBunny.ISM/manifest

### <a id="fmp4_v20"></a>Smooth Streaming 2.0 manifestet (äldre manifestet)
Som standard innehåller Smooth Streaming manifestet format hello Upprepa tagg (r-tagg). Dock stöder inte vissa spelare hello r-tagg. Klienter med dessa spelare kan använda ett format som inaktiverar hello r-taggen:

{strömmande slutpunkten namn media services-konto name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest(format=fmp4-v20)

    http://testendpoint-testaccount.streaming.mediaservices.windows.net/fecebb23-46f6-490d-8b70-203e86b0df58/BigBuckBunny.ism/Manifest(format=fmp4-v20)

## <a name="progressive-download"></a>Progressiv hämtning
Du kan starta uppspelning innan hello hela filen har hämtats med progressiv nedladdning. Du kan inte progressivt hämta .ism * (ismv isma, ismt eller ismc) filer.

tooprogressively laddar ned innehåll använder hello OnDemandOrigin typ av lokaliserare. hello visar följande exempel hello-URL som baseras på hello OnDemandOrigin typ av lokaliserare:

    http://amstest1.streaming.mediaservices.windows.net/3c5fe676-199c-4620-9b03-ba014900f214/BigBuckBunny_H264_650kbps_AAC_und_ch2_96kbps.mp4

Du måste dekryptera alla lagring krypteras tillgångar som du vill toostream från hello ursprung tjänsten för progressiv nedladdning.

## <a name="download"></a>Ladda ned
toodownload enheten innehåll tooa klienten måste du skapa en SAS-lokaliserare. hello SAS-positionerare ger åtkomst toohello Azure Storage-behållare där filen finns. toobuild hello nedladdnings-URL du har tooembed hello filnamn mellan hello värd- och SAS-signatur.

hello visar följande exempel hello-URL som baseras på hello SAS-positionerare:

    https://test001.blob.core.windows.net/asset-ca7a4c3f-9eb5-4fd8-a898-459cb17761bd/BigBuckBunny.mp4?sv=2012-02-12&se=2014-05-03T01%3A23%3A50Z&sr=c&si=7c093e7c-7dab-45b4-beb4-2bfdff764bb5&sig=msEHP90c6JHXEOtTyIWqD7xio91GtVg0UIzjdpFscHk%3D

det gäller hello följande överväganden:

* Du måste dekryptera alla lagring krypteras tillgångar som du vill toostream från hello ursprung tjänsten för progressiv nedladdning.
* En fil som inte har slutförts inom 12 timmar misslyckas.

## <a name="streaming-endpoints"></a>Slutpunkter för direktuppspelning

En strömningsslutpunkt som representerar en strömmande tjänst som kan leverera innehåll direkt tooa klienten player program eller tooa innehållsleveransnätverk (CDN) för vidare distribution. hello utgående ström från en strömmande slutpunkt-tjänst kan vara en direktsänd dataström eller en video-on-demand tillgångar i Media Services-kontot. Det finns två typer av strömningsslutpunkter, **standard** och **premium**. Mer information finns i [Streaming slutpunkter översikt](media-services-streaming-endpoints-overview.md).

>[!NOTE]
>När AMS-kontot skapas en **standard** strömningsslutpunkt har lagts till tooyour konto i hello **stoppad** tillstånd. toostart strömning ditt innehåll och dra nytta av dynamisk paketering och dynamisk kryptering hello strömningsslutpunkt som du vill toostream innehåll har toobe i hello **kör** tillstånd. 

## <a name="known-issues"></a>Kända problem
### <a name="changes-toosmooth-streaming-manifest-version"></a>Ändringar tooSmooth Streaming manifestVersion
Innan hello versionen av juli 2016 tjänsten--när tillgångar som produceras av Media Encoder Standard, Media Encoder Premium arbetsflöde eller hello tidigare Azure Media Encoder har strömmats med hjälp av dynamisk paketering--hello Smooth Streaming manifest returnerade skulle överensstämmer tooversion 2.0. I version 2.0 Använd inte hello fragment varaktigheter hello s.k. Upprepa (r) taggar. Exempel:

<?xml version="1.0" encoding="UTF-8"?>
    <SmoothStreamingMedia MajorVersion="2" MinorVersion="0" Duration="8000" TimeScale="1000">
        <StreamIndex Chunks="4" Type="video" Url="QualityLevels({bitrate})/Fragments(video={start time})" QualityLevels="3" Subtype="" Name="video" TimeScale="1000">
            <QualityLevel Index="0" Bitrate="1000000" FourCC="AVC1" MaxWidth="640" MaxHeight="360" CodecPrivateData="00000001674D4029965201405FF2E02A100000030010000003032E0A000F42400040167F18E3050007A12000200B3F8C70ED0B16890000000168EB7352" />
            <c t="0" d="2000" n="0" />
            <c d="2000" />
            <c d="2000" />
            <c d="2000" />
        </StreamIndex>
    </SmoothStreamingMedia>

I hello juli 2016 versionen av tjänsten uppfyller hello genereras Smooth Streaming manifestet tooversion 2.2, fragment varaktigheter med Upprepa taggar. Exempel:

    <?xml version="1.0" encoding="UTF-8"?>
    <SmoothStreamingMedia MajorVersion="2" MinorVersion="2" Duration="8000" TimeScale="1000">
        <StreamIndex Chunks="4" Type="video" Url="QualityLevels({bitrate})/Fragments(video={start time})" QualityLevels="3" Subtype="" Name="video" TimeScale="1000">
            <QualityLevel Index="0" Bitrate="1000000" FourCC="AVC1" MaxWidth="640" MaxHeight="360" CodecPrivateData="00000001674D4029965201405FF2E02A100000030010000003032E0A000F42400040167F18E3050007A12000200B3F8C70ED0B16890000000168EB7352" />
            <c t="0" d="2000" r="4" />
        </StreamIndex>
    </SmoothStreamingMedia>

Vissa av hello äldre Smooth Streaming-klienter kanske inte stöder hello Upprepa taggar och misslyckas tooload hello manifest. toomitigate problemet kan du kan använda hello äldre manifestet formatparametern **(format = fmp4 v20)** eller uppdatera din toohello senaste klientversionen, vilket stöder Upprepa taggar. Mer information finns i [Smooth Streaming 2.0](media-services-deliver-content-overview.md#fmp4_v20).

## <a name="media-services-learning-paths"></a>Sökvägar för Media Services-utbildning
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Ge feedback
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

## <a name="related-topics"></a>Relaterade ämnen
[Uppdatera Media Services lokaliserare efter rullande lagringsnycklar](media-services-roll-storage-access-keys.md)

