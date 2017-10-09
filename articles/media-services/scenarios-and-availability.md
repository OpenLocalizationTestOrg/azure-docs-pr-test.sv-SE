---
title: "aaaMicrosoft Azure Media Services scenarier och tillgängligheten för funktioner i Datacenter | Microsoft Docs"
description: "Det här avsnittet ger en översikt över Microsoft Azure Media Services-scenarier och tillgängligheten för funktioner och tjänster i datacenter."
services: media-services
documentationcenter: 
author: Juliako
manager: cfowler
editor: 
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: hero-article
ms.date: 07/10/2017
ms.author: juliako;anilmur
ms.openlocfilehash: 3dbab6998ed5da738baf8f1e2fb096dfba336e19
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="scenarios-and-availability-of-media-services-features-across-datacenters"></a>Scenarier och tillgängligheten för Media Services-funktioner i datacenter

Microsoft Azure Media Services (AMS) kan du ladda upp toosecurely, lagra, koda och paketera video-eller ljudinnehåll för både på begäran och live strömmande leverans toovarious klienter (till exempel TV, datorer och mobila enheter).

AMS körs i flera Datacenter hello världen. Dessa Datacenter grupperas i toogeographic regioner, vilket ger dig möjlighet att välja var toobuild dina program. Du kan granska hello [lista över regioner och deras platser](https://azure.microsoft.com/regions/). 

Det här avsnittet beskriver vanliga scenarier för att leverera innehåll [live](#live_scenarios) eller [på begäran](#vod_scenarios). hello avsnittet ger också information om tillgängligheten för mediafunktioner och tjänster över datacenter.

## <a name="overview"></a>Översikt

### <a name="prerequisites"></a>Krav

toostart med Azure Media Services bör du ha hello följande:

* Ett Azure-konto. Om du inte har något konto kan skapa du ett kostnadsfritt utvärderingskonto på bara några minuter. Mer information finns i [kostnadsfri utvärderingsversion av Azure](https://azure.microsoft.com).
* Ett Azure Media Services-konto. Mer information finns i [Skapa konto](media-services-portal-create-account.md).
* Hej strömningsslutpunkt från vilken du vill att toostream innehåll har toobe i hello **kör** tillstånd.

    När AMS-kontot skapas en **standard** strömningsslutpunkt har lagts till tooyour konto i hello **stoppad** tillstånd. toostart strömning ditt innehåll och dra nytta av dynamisk paketering och dynamisk kryptering, hello strömmande slutpunkten har toobe i hello **kör** tillstånd.

### <a name="commonly-used-objects-when-developing-against-hello-ams-odata-model"></a>Vanliga objekt när du utvecklar mot hello AMS OData-modellen

hello följande bild visar några av de vanligaste hello objekt när du utvecklar mot hello Media Services OData-modellen.

Klicka på hello avbildningen tooview den full storlek.  

<a href="./media/media-services-overview/media-services-overview-object-model.png" target="_blank"><img src="./media/media-services-overview/media-services-overview-object-model-small.png"></a> 

Du kan visa hela modellen hello [här](https://media.windows.net/API/$metadata?api-version=2.15).  

## <a name="protect-content-in-storage-and-deliver-streaming-media-in-hello-clear-non-encrypted"></a>Skydda lagrat innehåll och leverera strömmande medier i hello avmarkera (okrypterat)

![VoD-arbetsflöde](./media/scenarios-and-availability/scenarios-and-availability01.png)

1. Överför en mediefil av hög kvalitet till en tillgång.

    Det rekommenderas tooapply storage kryptering alternativet tooyour tillgångar i ordning tooprotect ditt innehåll under överföra och medan i vila i lagringsutrymmet.
2. Koda tooa uppsättning MP4-filer med anpassningsbar bithastighet.

    Det rekommenderas tooapply storage kryptering alternativet toohello utdata tillgångar i ordning tooprotect innehållet i vila.
3. Konfigurera principen för tillgångsleverans (används av dynamisk paketering).

    Om tillgången är lagringskrypterad **måste** du konfigurera principen för tillgångsleverans.
4. Publicera hello tillgång genom att skapa en OnDemand-positionerare.
5. Strömma publicerat innehåll.

Information om tillgänglighet i datacenter finns hello [tillgänglighet](#availability) avsnitt.

## <a name="protect-content-in-storage-deliver-dynamically-encrypted-streaming-media"></a>Skydda lagrat innehåll, leverera dynamiskt krypterade strömmande media

![Skydda med PlayReady](./media/media-services-content-protection-overview/media-services-content-protection-with-multi-drm.png)

1. Överför en mediefil av hög kvalitet till en tillgång. Tillämpa storage kryptering alternativet toohello tillgången.
2. Koda tooa uppsättning MP4-filer med anpassningsbar bithastighet. Tillämpa storage kryptering alternativet toohello utdatatillgången.
3. Skapa en innehållskrypteringsnyckel för hello tillgång som du vill kryptera dynamiskt under uppspelning toobe.
4. Konfigurera principen för auktorisering av innehållsnyckel.
5. Konfigurera en princip för tillgångsleveranser (används av dynamisk paketering och dynamisk kryptering).
6. Publicera hello tillgång genom att skapa en OnDemand-positionerare.
7. Strömma publicerat innehåll.

Information om tillgänglighet i datacenter finns hello [tillgänglighet](#availability) avsnitt.

## <a name="use-media-analytics-tooderive-actionable-insights-from-your-videos"></a>Använd Media Analytics tooderive tillämplig insikter från dina videor

Media Analytics är en samling tal- och visionskomponenter komponenter som gör det lättare för organisationer och företag tooderive tillämplig insikter från sina videofiler. Mer information finns i [Översikt över Azure Media Services Analytics](media-services-analytics-overview.md).

1. Överför en mediefil av hög kvalitet till en tillgång.
2. Bearbeta videor med något av hello Media Analytics-tjänster som beskrivs i hello [översikt över Media Analytics](media-services-analytics-overview.md) avsnitt.
3. Media Analytics-medieprocessorer producerar MP4-filer eller JSON-filer. Om en medieprocessor har producerat en MP4-fil kan hämta du progressivt hello-filen. Om en medieprocessor har producerat en JSON-fil kan du ladda ned hello filen från hello Azure blob storage.

Information om tillgänglighet i datacenter finns hello [tillgänglighet](#availability) avsnitt.

## <a name="deliver-progressive-download"></a>Leverera progressiv nedladdning

1. Överför en mediefil av hög kvalitet till en tillgång.
2. Koda tooa enda MP4-fil.
3. Publicera hello tillgång genom att skapa en OnDemand- eller SAS-positionerare.

    Om du använder SAS-positionerare laddas hello innehållet ned från hello Azure blob storage. I det här fallet behöver inte toohave strömningsslutpunkter i startat tillstånd.
4. Progressivt hämtat innehåll

## <a id="live_scenarios"></a>Leverera liveuppspelningshändelser 

1. Infoga innehåll via olika protokoll för liveuppspelning (till exempel RTMP eller Smooth Streaming).
2. (Valfritt) Koda strömmen till ström med anpassningsbar bithastighet.
3. Förhandsgranska din liveuppspelning.
4. leverera hello innehållet via vanliga strömningsprotokoll (till exempel MPEG DASH, Smooth, HLS) direkt tooyour kunder eller tooa innehåll innehållsleveransnätverk (CDN) för vidare distribution.

    ELLER

    Post och store hello inhämtas innehåll i ordning toobe strömma senare (Video-on-Demand).

Du kan välja en av följande hello dirigerar när du gör direktsänd strömning:

### <a name="working-with-channels-that-receive-multi-bitrate-live-stream-from-on-premises-encoders-pass-through"></a>Arbeta med kanaler som tar emot liveström med flera bithastigheter från lokala kodare (genomströmning)

hello följande diagram visar hello huvudsakliga delarna i hello AMS-plattformen som ingår i hello **direkt** arbetsflöde.

![Live-arbetsflöde](./media/scenarios-and-availability/media-services-live-streaming-current.png)

Mer information finns i [Arbeta med kanaler som tar emot liveström med flera bithastigheter från lokala kodare](media-services-live-streaming-with-onprem-encoders.md).

### <a name="working-with-channels-that-are-enabled-tooperform-live-encoding-with-azure-media-services"></a>Arbeta med kanaler som är aktiverad tooperform live encoding med Azure Media Services

hello följande diagram visar hello större delar av hello AMS-plattformen som ingår i arbetsflödet för Liveströmning där en kanal är aktiverad tooperform live encoding med Media Services.

![Live-arbetsflöde](./media/scenarios-and-availability/media-services-live-streaming-new.png)

Mer information finns i [arbeta med kanaler som är aktiverad tooPerform Live Encoding med Azure Media Services](media-services-manage-live-encoder-enabled-channels.md).

Information om tillgänglighet i datacenter finns hello [tillgänglighet](#availability) avsnitt.

## <a name="consuming-content"></a>Konsumera innehåll

Azure Media Services innehåller hello verktyg du behöver toocreate omfattande dynamiska klientprogram för uppspelare för de flesta plattformar, bland annat: iOS-enheter, Android-enheter, Windows, Windows Phone, Xbox och fristående rutorna. följande ämne hello innehåller länkar tooSDKs och Spelarramverk som du kan använda toodevelop egna klientprogram som kan använda strömmande media från Media Services. Mer information finns i [Developing video player applications](media-services-develop-video-players.md) (Utveckla videospelarprogram)

## <a name="enabling-azure-cdn"></a>Aktivera Azure CDN

Media Services stöder integration med Azure CDN. Mer information om hur tooenable Azure CDN finns [hur tooManage Strömningsslutpunkter i ett Media Services-konto](media-services-portal-manage-streaming-endpoints.md).

## <a id="scaling"></a>Skala ett Media Services-konto

AMS-kunder kan skala slutpunkter för direktuppspelning, mediebearbetning och lagring i sina AMS-konton.

* Media Services-kunder kan antingen välja en **Standard**-slutpunkt för direktuppspelning eller en **Premium**-slutpunkt för direktuppspelning. En **Standard**-slutpunkt för direktuppspelning passar de flesta arbetsbelastningar för direktuppspelning. Hello samma funktioner som innehåller en **Premium** strömmande slutpunkter och skalar utgående bandbredd automatiskt. 

    **Premium**-slutpunkter för direktuppspelning passar för avancerade arbetsbelastningar och tillhandahåller dedikerad och skalbar bandbreddskapacitet. Kunder som har en **Premium**-slutpunkt för direktuppspelning får som standard en direktuppspelande enhet (SU). hello strömmande slutpunkten kan skalas genom att lägga till SUs. Varje SU ger ytterligare bandbredd kapacitet toohello program. Mer information om att skala **Premium** strömningsslutpunkter, se hello [skalning strömningsslutpunkter](media-services-portal-scale-streaming-endpoints.md) avsnittet.

* Ett Media Services-konto är kopplat till ett reserverat enhetstyp som bestämmer hello hastighet som media bearbeta uppgifter bearbetas. Du kan välja mellan följande hello reserverade enhetstyper: **S1**, **S2**, eller **S3**. Till exempel hello samma kodningsjobbet körs snabbare när du använder hello **S2** reserverade enhetstyp jämföra toohello **S1** typen.

    Dessutom toospecifying hello reserverade enhetstyp, kan du ange tooprovision till ditt konto med **reserverade enheter** (RUs). hello antalet etablerade RUs bestämmer hello antal media uppgifter som kan bearbetas samtidigt i en viss konto.

    >[!NOTE]
    >RU:er fungerar för parallellisera all bearbetning av media, inklusive indexeringsjobb med hjälp av Azure Media Indexer. Men till skillnad från kodning bearbetas inte indexeringsjobb snabbare med snabbare reserverade enheter.

    Mer information finns i [Skala mediebearbetning](media-services-portal-scale-media-processing.md).
* Du kan även skala ditt Media Services-konto genom att lägga till storage-konton tooit. Varje lagringskonto är begränsat too500 TB. tooexpand din lagring utöver standardbegränsningarna hello du tooattach flera storage-konton tooa enda Media Services-konto. Mer information finns i [Hantera lagringskonton](meda-services-managing-multiple-storage-accounts.md).

##<a id="availability"></a> Tillgänglighet för Media Services-funktioner i datacenter

Det här avsnittet innehåller information om tillgängligheten för Media Services-funktioner i datacenter.

### <a name="ams-accounts"></a>AMS-konton

#### <a name="availability"></a>Tillgänglighet

Du kan skapa Media Services-konton i hello följande områden: Nordeuropa, Västeuropa, västra USA, östra USA, Sydostasien, Östasien, västra Japan, östra, södra, västra Indien, södra Indien och centrala Indien. 

### <a name="streaming-endpoints"></a>Slutpunkter för direktuppspelning 

Media Services-kunder kan antingen välja en **Standard**-slutpunkt för direktuppspelning eller en **Premium**-slutpunkt för direktuppspelning. Mer information finns i hello [skalning](#scaling) avsnitt.

#### <a name="availability"></a>Tillgänglighet

|Namn|Status|Datacenter
|---|---|---|
|Standard|Allmän tillgänglighet (GA)|Alla|
|Premium|Allmän tillgänglighet (GA)|Alla|

### <a name="live-encoding"></a>Live Encoding

#### <a name="availability"></a>Tillgänglighet

Tillgänglig i alla datacenter förutom: Tyskland, södra Brasilien, västra Indien, södra Indien och centrala Indien. 

### <a name="encoding-media-processors"></a>Mediebearbetare för kodning

AMS erbjuder två kodare på begäran: **Media Encoder Standard** och **Media Encoder Premium Workflow**. Mer information finns i [Overview and comparison of Azure on-demand media encoders](media-services-encode-asset.md) (Översikt och jämförelse av Azures mediekodare på begäran). 

#### <a name="availability"></a>Tillgänglighet

|Namn på mediebearbetare|Status|Datacenter
|---|---|---|
|Media Encoder Standard|Allmän tillgänglighet (GA)|Alla|
|Arbetsflöde för Media Encoder Premium|Allmän tillgänglighet (GA)|Alla utom Kina|

### <a name="analytics-media-processors"></a>Mediebearbetare för analys

Media Analytics är en samling tal-och visionskomponenter som gör det enklare för organisationer och företag tooderive tillämplig insikter från sina videofiler. Mer information finns i [Översikt över Azure Media Services Analytics](media-services-analytics-overview.md).

#### <a name="availability"></a>Tillgänglighet

|Namn på mediebearbetare|Status|Datacenter
|---|---|---|
|Azure Media Face Detector|Förhandsversion|Alla|
|Azure Media Hyperlapse|Förhandsversion|Alla|
|Azure Media Indexer|Allmän tillgänglighet (GA)|Alla|
|Azure Media Motion Detector|Förhandsversion|Alla|
|Azure Media OCR|Förhandsversion|Alla|
|Azure Media Redactor|Förhandsversion|Alla|
|Azure Media Stabilizer|Förhandsversion|Alla|
|Azure Media Video Thumbnails|Förhandsversion|Alla|
|Azure Media Indexer 2|Förhandsversion|Allt utom Kina och federala myndigheter|

### <a name="protection"></a>Skydd

Microsoft Azure Media Services kan du toosecure media från hello tid den lämnar din dator via lagring, bearbetning och leverans. Mer information finns i avsnittet om att [skydda AMS-innehåll](media-services-content-protection-overview.md).

#### <a name="availability"></a>Tillgänglighet

|Kryptering|Status|Datacenter|
|---|---|---| 
|Lagring|Allmän tillgänglighet (GA)|Alla|
|128-bitars AES-nycklar|Allmän tillgänglighet (GA)|Alla|
|Fairplay|Allmän tillgänglighet (GA)|Alla|
|PlayReady|Allmän tillgänglighet (GA)|Alla|
|Widevine|Allmän tillgänglighet (GA)|Alla utom Tyskland, federala myndigheter och Kina.

### <a name="reserved-units-rus"></a>Reserverade enheter (RU:er)

hello antalet etablerade reserverade enheter avgör hello antal media uppgifter som kan bearbetas samtidigt i en viss konto. 

Mer information finns i hello [skalning](#scaling) avsnitt.

#### <a name="availability"></a>Tillgänglighet

Tillgängligt i alla datacenter.

### <a name="reserved-unit-ru-type"></a>Typ av reserverad enhet (RU)

Ett Media Services-konto är kopplat till en enhetstyp, som avgör hello hastighet som media bearbeta uppgifter bearbetas. Du kan välja mellan följande hello reserverade enhetstyper: S1, S2 och S3.

Mer information finns i hello [skalning](#scaling) avsnitt.

#### <a name="availability"></a>Tillgänglighet

|Namn på RU-typ|Status|Datacenter
|---|---|---|
|S1|Allmän tillgänglighet (GA)|Alla|
|S2|Allmän tillgänglighet (GA)|Alla utom södra Brasilien och västra Indien|
|S3|Allmän tillgänglighet (GA)|Allt utom västra Indien|

## <a name="next-steps"></a>Nästa steg

Granska sökvägarna för Media Services-utbildning.

[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Ge feedback
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

