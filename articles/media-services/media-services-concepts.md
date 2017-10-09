---
title: aaaAzure Media Services-koncepten | Microsoft Docs
description: "Det här avsnittet ger en översikt över Azure Media Services-koncepten"
services: media-services
documentationcenter: 
author: Juliako
manager: cfowler
editor: 
ms.assetid: dcefc8bc-e2ea-4b38-a643-9010f4436fb5
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/07/2017
ms.author: juliako
ms.openlocfilehash: 0a45deff32336dfcf778552f720c1ea21927951b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-media-services-concepts"></a>Azure Media Services-koncepten
Det här avsnittet ger en översikt över hello viktigaste Media Services-koncepten.

## <a id="assets"></a>Tillgångar och lagring
### <a name="assets"></a>Tillgångar
En [tillgången](https://docs.microsoft.com/rest/api/media/operations/asset) innehåller digitala filer (inklusive video, ljud, bilder, miniatyrsamlingar, textspår och filer med dold textning) och hello metadata om dessa filer. När hello digitala filer överförs till en tillgång, kan de användas i hello Media Services encoding och direktöverföring av arbetsflöden.

En tillgång är mappade tooa blob-behållaren i hello Azure Storage-konto och hello i hello tillgången filer som blockblobar i behållaren. Azure Media Services stöder inte sidblobar.

När du bestämmer vilka media innehåll tooupload och lagra i en tillgång, tillämpa hello följande överväganden:

* En tillgång får innehålla endast en enda, unikt instans av medieinnehåll. Till exempel en enda redigering av TV-avsnitt, film eller annons.
* En tillgång får inte innehålla flera återgivningar eller ändringar i en fil som audiovisuella. Ett exempel på en felaktig användning av en tillgång skulle försöker toostore avsnitt för mer än en TV, annons eller flera kameravinklar från en enda produktion inuti en tillgång. Lagra flera återgivningar eller ändringar i en audiovisuella fil i en tillgång kan medföra problem skickar kodning jobben strömning och skydda hello leverans av hello tillgång senare i hello arbetsflöde.  

### <a name="asset-file"></a>Resursfil
En [AssetFile](https://docs.microsoft.com/rest/api/media/operations/assetfile) representerar en faktiska video eller ljud-fil som lagras i en blob-behållare. En resursfil är alltid associerad med en tillgång och en tillgång kan innehålla en eller många filer. hello misslyckas Media Services-kodaren om objekttypen tillgången filen inte är associerad med en digital fil i en blob-behållaren.

Hej **AssetFile** instansen och hello faktiska mediefilen är två distinkta objekt. Hej AssetFile instans innehåller metadata om hello mediefilen när hello mediefilen innehåller hello faktiskt medieinnehåll.

Du bör inte försöka toochange hello innehållet i blob-behållare som har genererats av Media Services utan att använda Media Service API: er.

### <a name="asset-encryption-options"></a>Alternativ för kryptering av tillgångsinformation
Beroende på hello typ av innehåll som du vill tooupload, lagra och leverera Media Services tillhandahåller olika krypteringsalternativ för som du kan välja bland.

>[!NOTE]
>Ingen kryptering används. Detta är standardvärdet för hello. När du använder det här alternativet skyddas inte innehållet under överföring eller i vila i lagringsutrymmet.

Om du planerar toodeliver en MP4 med progressivt nedladdning ska använda det här alternativet tooupload ditt innehåll.

**StorageEncrypted** – Använd det här alternativet tooencrypt innehållet lokalt med hjälp av AES 256-bitars kryptering och sedan ladda upp den tooAzure lagring där den lagras krypterat i vila. Tillgångar som skyddas med lagringskryptering okrypterad och placeras i en krypterad fil system tidigare tooencoding och eventuellt omkrypterade tidigare toouploading tillbaka som en ny utdatatillgång automatiskt. hello primära användningsfall för lagringskryptering är om du vill toosecure dina indata mediefiler hög kvalitet med stark kryptering i vila på disk. 

I ordning toodeliver en krypterad tillgång för lagring, måste du konfigurera hello tillgångsleveransprincip så Media Services vet du hur toodeliver ditt innehåll. Innan din tillgång kan strömmas angetts hello streaming server tar bort hello lagringskryptering och dataströmmar innehåll med hello principen för tillgångsleverans (till exempel AES, PlayReady eller Ingen kryptering). 

**CommonEncryptionProtected** – Använd det här alternativet om du vill tooencrypt (eller överför redan krypterat) innehåll med vanlig kryptering eller PlayReady DRM (till exempel Smooth Streaming som skyddas med PlayReady DRM).

**EnvelopeEncryptionProtected** – Använd det här alternativet om du vill tooprotect (eller överför redan skyddad) HTTP Live Streaming (HLS) krypteras med Advanced Encryption Standard (AES). Om du överför HLS som redan har krypterats med AES, måste den har krypterats av Transform Manager.

### <a name="access-policy"></a>Åtkomstprincipen
En [AccessPolicy](https://docs.microsoft.com/rest/api/media/operations/accesspolicy) definierar behörigheter (t.ex. Läs-, Skriv- och listan) och varaktighet för åtkomst tooan tillgången. Du skulle vanligtvis skickar en AccessPolicy för objektet tooa-positionerare som blir används tooaccess hello filerna i tillgången.

>[!NOTE]
>Det finns en gräns på 1 000 000 principer för olika AMS-principer (till exempel för positionerarprincipen eller ContentKeyAuthorizationPolicy). Du bör använda hello samma princip-ID om du alltid använder hello samma dagar / åtkomstbehörigheter, till exempel principer för lokaliserare som är avsedda tooremain på plats för lång tid (icke-överföringen principer). Mer information finns i [detta](media-services-dotnet-manage-entities.md#limit-access-policies) avsnitt.

### <a name="blob-container"></a>BLOB-behållare
En blob-behållare grupperar en uppsättning blobbar. BLOB-behållare används som gräns för åtkomstkontroll och delade signatur åtkomst (SAS)-positionerare på tillgångar i Media Services. Ett Azure Storage-konto kan innehålla ett obegränsat antal blob-behållare. En behållare kan lagra ett obegränsat antal blobbar.

>[!NOTE]
> Du bör inte försöka toochange hello innehållet i blob-behållare som har genererats av Media Services utan att använda Media Service API: er.
> 
> 

### <a id="locators"></a>Lokaliserare
[Lokaliserare](https://docs.microsoft.com/rest/api/media/operations/locator)s ger en post punkt tooaccess hello filer som finns i en tillgång. En åtkomstprincip är används toodefine hello behörigheter och varaktighet för att en klient har åtkomst tooa ges tillgång. Positionerare kan ha en många tooone-relation med en åtkomstprincip, så att olika positionerare kan ge olika starttiderna och -typer toodifferent klienter när alla med hello samma behörighet och varaktigheten; på grund av en delad åtkomst för begränsning av Azure storage-tjänster, kan inte du ha fler än fem unika lokaliserare som är associerade med en viss resurs i taget. 

Media Services stöder två typer av positionerare: OnDemandOrigin-positionerare som används för toostream media (till exempel MPEG DASH, HLS eller Smooth Streaming) eller progressivt hämta media och SAS-URL-positionerare som används tooupload eller hämta media filer to\from Azure storage. 

>[!NOTE]
>hello listan behörighet (AccessPermissions.List) bör inte användas när du skapar en OnDemandOrigin-positionerare. 

### <a name="storage-account"></a>Lagringskonto
Alla åtkomst tooAzure Storage görs genom ett lagringskonto. Ett Media Service-konto kan associera med en eller flera lagringskonton. Ett konto kan innehålla ett obegränsat antal behållare, så länge som deras sammanlagda storlek är under 500TB per lagringskonto.  Media Services tillhandahåller SDK nivå tooling tooallow du toomanage flera lagringskonton och Läs in balansera hello distribution av dina tillgångar under överföringen toothese konton baserat på mått eller en slumpmässig distributionsplats. Mer information finns i Arbeta med [Azure Storage](https://msdn.microsoft.com/library/azure/dn767951.aspx). 

## <a name="jobs-and-tasks"></a>Jobb och uppgifter
En [jobbet](https://https://docs.microsoft.com/rest/api/media/operations/job) är brukar användas tooprocess (till exempel index eller koda) en presentation av ljud och video. Om du bearbetar flera videor, skapa ett jobb för varje video toobe kodad.

Ett jobb innehåller metadata om hello bearbetning toobe utförs. Varje jobb innehåller en eller flera [aktivitet](https://docs.microsoft.com/rest/api/media/operations/task)s som anger en atomisk Bearbetningsuppgift tillgångarna indata, utdata tillgångar, en medieprocessor och dess associerade inställningarna. Uppgifter i ett jobb kan sammankopplas, där ges hello utdatatillgången av en aktivitet som hello indatatagg tillgången toohello nästa uppgift. På så sätt kan ett jobb innehåller alla hello bearbetning behövs för en media.

## <a id="encoding"></a>Kodning
Azure Media Services erbjuder flera alternativ för hello-kodning av media i hello molnet.

När börjat med Media Services är det viktigt toounderstand hello skillnaden mellan codec och filformat.
Codec-rutiner är hello-programvara som implementerar hello komprimering/dekomprimering algoritmer filformat är behållare som innehåller hello komprimerade video.

Media Services tillhandahåller en dynamisk paketering som gör att du toodeliver din anpassningsbar bithastighet MP4 eller Smooth Streaming-kodade innehåll i strömningsformat som stöds av Media Services (MPEG DASH, HLS, Smooth Streaming) utan att behöva toore-paket till dessa strömningsformat.

tootake nytta av [dynamisk paketering](media-services-dynamic-packaging-overview.md), du behöver tooencode din mezzaninfil (källa) till en uppsättning med anpassningsbar bithastighet MP4-filer eller Smooth Streaming-filer och har minst en standard- eller premium strömmande slutpunkt i startat tillstånd.

Media Services stöder hello följa på begäran kodare som beskrivs i den här artikeln:

* [Media Encoder Standard](media-services-encode-asset.md#media-encoder-standard)
* [Arbetsflöde för Media Encoder Premium](media-services-encode-asset.md#media-encoder-premium-workflow)

Information om stöds kodare finns [kodare](media-services-encode-asset.md).

## <a name="live-streaming"></a>Liveströmning
I Azure Media Services representerar en kanal en pipeline för bearbetning av liveströmmat innehåll. En kanal som tar emot live indata i ett av två sätt:

* En lokal livekodare skickar flera bithastigheter RTMP eller Smooth Streaming (fragmenterad MP4) toohello kanal. Du kan använda följande livekodare som skickar Smooth Streaming i flera bithastigheter hello: MediaExcel, Ateme, anta kommunikation, Envivio, Cisco och Elemental. hello följande livekodare skickar RTMP: Adobe Flash Live Encoder, Telestream Wirecast, Teradek, Haivision och Tricaster kodare. hello infogade strömmarna passerar genom kanaler utan ytterligare omkodning och kodning. På begäran levererar Media Services hello dataströmmen toocustomers.
* En dataström med enkel bithastighet (i ett hello följande format: RTP (MPEG-TS)), RTMP eller Smooth Streaming (fragmenterad MP4)) skickas toohello kanal som är aktiverad tooperform live encoding med Media Services. hello kanalen utför sedan live encoding av hello inkommande dataströmmen tooa flera bithastigheter (anpassningsbar) video dataström med enkel bithastighet. På begäran levererar Media Services hello dataströmmen toocustomers.

### <a name="channel"></a>Kanal
I Media Services [kanal](https://docs.microsoft.com/rest/api/media/operations/channel)s ansvarar för bearbetning av liveströmmat innehåll. En kanal som tillhandahåller en slutpunkt för indata (infognings-URL) som du sedan ange tooa live transcoder. hello kanal tar emot live indata från hello live transcoder och gör den tillgänglig för strömning via en eller flera Strömningsslutpunkter. Kanaler ger också en förhandsgranskningsslutpunkten (förhandsgranskning URL) om du använder toopreview och verifiera strömmen innan ytterligare bearbetning och leverans.

Du kan hämta hello infognings-URL och hello förhandsgransknings-URL när du skapar hello-kanal. tooget dessa URL: er hello kanal har inte toobe i hello igång tillstånd. När du är klar toostart push-överföring av data från en levande transcoder till hello kanal måste hello kanal vara igång. När hello live transcoder startar mata in data, kan du förhandsgranska dataströmmen.

Varje Media Services-konto kan innehålla flera kanaler, flera program och flera Strömningsslutpunkter. Beroende på hello bandbredds- och behov kan StreamingEndpoint tjänster vara dedikerade tooone eller flera kanaler. Alla StreamingEndpoint dra från varje kanal.

### <a name="program-event"></a>Program (händelse)
En [Program (händelse)](https://docs.microsoft.com/rest/api/media/operations/program) kan du toocontrol hello publicering och lagring av segment i en direktsänd dataström. Kanaler hanterar program (händelser). hello är relationen mellan kanal och Program liknande tootraditional media där en kanal har en konstant ström av innehåll och ett program är begränsade toosome tidsinställd händelse på kanalen.
Du kan ange hello antal timmar som du vill att tooretain hello registreras innehåll för programmet hello genom att ange hello **ArchiveWindowLength** egenskapen. Det här värdet kan anges från minst 5 minuter tooa högst 25 timmar.

ArchiveWindowLength avgör också hello hur lång tid som klienter kan söka bakåt i tiden från hello aktuella direktsända positionen. Program kan köras under hello angiven tidsperiod, men innehåll som ligger bakom hello längd ignoreras kontinuerligt. Det här värdet för den här egenskapen avgör också hur länge hello klient manifest kan växa.

Varje program (händelse) är associerat med en tillgång. toopublish hello program måste du skapa en positionerare för hello associerade tillgången. Med den här positioneraren kan toobuild en strömnings-URL som du kan tillhandahålla tooyour klienter.

En kanal har stöd för upp toothree program som körs samtidigt så du kan skapa flera Arkiv för hello samma inkommande dataström. Detta ger dig toopublish och arkivera olika delar av en händelse efter behov. Till exempel är dina affärsbehov tooarchive 6 timmar av ett program, men toobroadcast sista 10 minuter. tooaccomplish detta, behöver du toocreate två program som körs samtidigt. Ett program är tooarchive 6 timmar av händelsen hello men hello programmet publiceras inte. hello kan inte ange tooarchive i 10 minuter och det här programmet har publicerats.

Mer information finns i:

* [Arbeta med kanaler som är aktiverad tooPerform Live Encoding med Azure Media Services](media-services-manage-live-encoder-enabled-channels.md)
* [Arbeta med kanaler som tar emot liveuppspelningar med flera bithastigheter från lokala kodare](media-services-live-streaming-with-onprem-encoders.md)
* [Kvoter och begränsningar](media-services-quotas-and-limitations.md).

## <a name="protecting-content"></a>Skydda innehållet
### <a name="dynamic-encryption"></a>Dynamisk kryptering
Azure Media Services kan du toosecure media från hello tid den lämnar din dator via lagring, bearbetning och leverans. Media Services kan du toodeliver ditt innehåll dynamiskt krypterad med Standard AES (Advanced Encryption) (med 128-bitars krypteringsnycklar) och vanliga kryptering (CENC) med PlayReady och/eller Widevine DRM. Media Services tillhandahåller också en tjänst för att leverera AES-nycklar och PlayReady-licenser tooauthorized klienter.

För närvarande kan du kryptera hello följande strömningsformat: HLS MPEG DASH och Smooth Streaming. Det går inte att kryptera progressiv hämtning.

Om du vill använda för Media Services tooencrypt en tillgång, behöver tooassociate en krypteringsnyckel (CommonEncryption eller EnvelopeEncryption) med din tillgång och även konfigurera auktoriseringsprinciper för hello nyckeln.

Om du vill toostream en krypterad tillgång för lagring, måste du konfigurera hello tillgångsleveransprincip i ordning toospecify du hur toodeliver din tillgång.

När en dataströmmen har begärts av en spelare, använder Media Services hello angetts viktiga toodynamically Kryptera innehåll med en kuvert kryptering (med AES) eller vanliga kryptering (med PlayReady eller Widevine). toodecrypt hello dataströmmen hello player begär hello nyckeln från hello viktiga leverans av tjänsten. toodecide om huruvida användaren hello är godkänd tooget hello nyckel, hello tjänsten utvärderar hello auktoriseringsprinciper som du angav för hello nyckeln.

### <a name="token-restriction"></a>Tokenbegränsningar
Hej innehållsnyckelns auktoriseringsprincip kan ha en eller flera auktoriseringsbegränsningar: öppen och token begränsning eller en IP-begränsning. Hej tokenbegränsade principen måste åtföljas av en token som utfärdas av en säker säkerhetstokentjänst (STS). Media Services stöder token i hello Simple Web Tokens (SWT) format och JSON-Webbtoken (JWT)-format. Media Services tillhandahåller inte Secure Token tjänster. Du kan skapa en anpassad STS eller använda Microsoft Azure ACS tooissue token. hello STS måste vara konfigurerade toocreate en token som signerats med hello angetts nyckel och utfärda anspråk som du angav i hello tokenbegränsningar konfiguration. hello Media Services viktiga leveranser returnerar hello begärt (eller licensinformation) toohello klienten om hello token är giltig och hello anspråk i hello token matchar de som konfigurerats för hello (eller licensinformation).

Du måste ange hello primära Verifieringsnyckeln, utfärdaren och målgrupp parametrar när du konfigurerar hello tokenbegränsade principen. hello primära Verifieringsnyckeln innehåller hello nyckel att hello token som signerats med är utfärdaren hello säker tokentjänsten som utfärdar hello-token. hello målgruppen (kallas ibland för scope) beskrivs hello syftet med hello-token eller hello resurs hello token auktoriserar åtkomst till. hello Media Services viktiga delivery service verifierar att dessa värden i hello token matchar hello värden i hello mallen.

Mer information finns i följande artiklar hello:

[Skydda Innehållsöversikt](media-services-content-protection-overview.md)
[skydda med AES-128](media-services-protect-with-aes128.md)
[skydda med DRM](media-services-protect-with-drm.md)

## <a name="delivering"></a>Leverera
### <a id="dynamic_packaging"></a>Dynamisk paketering
När du arbetar med Media Services, rekommenderas tooencode mezzanine filerna till en anpassad bithastighet MP4 ange och sedan konvertera hello ställer du in toohello önskade med hello [dynamisk paketering](media-services-dynamic-packaging-overview.md).

### <a name="streaming-endpoint"></a>Strömmande slutpunkt
En StreamingEndpoint representerar en strömmande tjänst som kan leverera innehåll direkt tooa player klientprogrammet eller tooa innehåll innehållsleveransnätverk (CDN) för vidare distribution (Azure Media Services ger nu) hello Azure CDN-integrering. hello utgående ström från en strömmande slutpunkt-tjänst kan vara en direktsänd dataström eller en video på begäran tillgångar i Media Services-kontot. Media Services kunderna välja antingen en **Standard** strömmande slutpunkten eller en eller flera **Premium** strömningsslutpunkter enligt tootheir behov. Standard strömmande slutpunkten är lämplig för de flesta strömmande arbetsbelastningar. 

Standard-slutpunkt för direktuppspelning passar de flesta arbetsbelastningar för direktuppspelning. Standard Strömningsslutpunkter erbjuder hello flexibilitet toodeliver ditt innehåll toovirtually varje enhet via dynamisk paketering till HLS, MPEG DASH och Smooth Streaming samt dynamisk kryptering för Microsoft PlayReady, Google Widevines, Apple Fairplay , och AES128.  De även skala från mycket små toovery stora målgrupper med tusentals samtidiga användare via Azure CDN-integration. Om du har en avancerad arbetsbelastning eller dina strömmande kapacitetskrav inte passar toostandard strömmande slutpunkten genomströmning mål eller toocontrol hello kapacitet måste hello StreamingEndpoint service toohandle växande bandbredd, rekommenderas tooallocate skalenheter (även kallat premium enheter för strömning).

Det rekommenderas toouse dynamisk paketering och dynamisk kryptering.

>[!NOTE]
>När AMS-kontot skapas en **standard** strömningsslutpunkt har lagts till tooyour konto i hello **stoppad** tillstånd. toostart strömning ditt innehåll och dra nytta av dynamisk paketering och dynamisk kryptering hello strömningsslutpunkt som du vill toostream innehåll har toobe i hello **kör** tillstånd. 

Mer information finns i [detta](media-services-portal-manage-streaming-endpoints.md) avsnitt.

Som standard kan du ha in too2 strömningsslutpunkter i Media Services-kontot. toorequest en högre gräns finns [kvoter och begränsningar](media-services-quotas-and-limitations.md).

Du debiteras endast när din StreamingEndpoint är i körningstillstånd.

### <a name="asset-delivery-policy"></a>Principen för tillgångsleverans
Ett av hello steg i hello Media Services innehållsleverans arbetsflöde konfigurerar [leveransprinciperna för tillgångar ](https://docs.microsoft.com/rest/api/media/operations/assetdeliverypolicy)som du vill toobe strömmas. Hej tillgångsleveransprincip talar om för Media Services hur du vill använda för din tillgång toobe levereras: i vilket strömningsprotokoll bör din tillgång dynamiskt paketeras (till exempel MPEG DASH, HLS, Smooth Streaming eller alla), oavsett om du vill toodynamically kryptera din tillgång och hur (envelope eller vanliga kryptering).

Om du har en tillgång med lagring krypterade innan din tillgång kan strömmas angetts hello streaming server tar bort hello lagringskryptering och dataströmmar innehåll med hello principen för tillgångsleverans. Till exempel toodeliver din tillgång krypteras med krypteringsnyckeln Advanced Encryption Standard (AES), ange hello princip typen tooDynamicEnvelopeEncryption. Ange hello princip typen tooNoDynamicEncryption tooremove lagringskryptering och dataströmmen hello tillgångar i hello Rensa.

### <a name="progressive-download"></a>Progressiv hämtning
Progressiv nedladdning kan toostart uppspelning innan hello hela filen har hämtats. Du kan bara progressivt hämta MP4-fil.

>[!NOTE]
>Du måste dekryptera krypterade tillgångar om du vill att de toobe som är tillgängliga för progressiv nedladdning.

URL för tooprovide användare med progressiv hämtning, du måste först skapa en OnDemandOrigin-positionerare. Att skapa hello positionerare, ger du hello grundläggande sökvägen toohello tillgången. Du måste sedan tooappend hello namnet på MP4-fil. Exempel:

http://amstest1.Streaming.mediaservices.Windows.NET/3c5fe676-199c-4620-9b03-ba014900f214/BigBuckBunny_H264_650kbps_AAC_und_ch2_96kbps.mp4

### <a name="streaming-urls"></a>Strömmande URL: er
Direktuppspelning av ditt innehåll tooclients. tooprovide användare med strömnings-URL: er kan du först skapa en OnDemandOrigin-positionerare. Att skapa hello positionerare, ger du hello grundläggande sökvägen toohello tillgång som innehåller hello-innehåll som du vill toostream. Dock toobe kan toostream det här innehållet toomodify måste den här sökvägen ytterligare. en fullständig URL-toohello strömning manifestfilen måste du sammanfoga hello lokaliserare sökvägen tooconstruct värdet och hello Manifestfilens namn (filename.ism). Lägg sedan till /Manifest och en sökväg i korrekt format (vid behov) toohello lokaliserare.

Du kan också strömma ditt innehåll via en SSL-anslutning. toodo, kontrollera din strömmande URL: er starta med HTTPS. För närvarande stöder AMS inte SSL med anpassade domäner.  

>[!NOTE]
>Du kan bara strömma via SSL om hello strömningsslutpunkt från vilken du kan leverera ditt innehåll skapades efter 10 September 2014. Om din strömmande URL: er baseras på hello strömningsslutpunkter som skapats efter 10 September innehåller hello Webbadressen ”streaming.mediaservices.windows.net” (hello nya format). Strömmande URL: er som innehåller ”origin.mediaservices.windows.net” (hello gamla format) stöder inte SSL. Om URL: en har hello gamla format och du vill toobe kan toostream via SSL, kan du skapa en ny strömmande slutpunkt. Använd URL: er skapas baserat på hello strömning nya endpoint toostream ditt innehåll via SSL.

hello följande lista beskriver olika strömningsformat och ger exempel:

* Smooth Streaming

{namn på slutpunkt för direktuppspelning-namn på mediaservicekonto}.streaming.mediaservices.windows.net/lokalisator-ID}/{filnamn}.ism/Manifest

http://testendpoint-testaccount.Streaming.mediaservices.Windows.NET/fecebb23-46F6-490d-8B70-203e86b0df58/BigBuckBunny.ISM/manifest

* MPEG DASH

{strömmande slutpunkten namn media services-konto name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest(format=mpd-time-csf)

http://testendpoint-testaccount.Streaming.mediaservices.Windows.NET/fecebb23-46F6-490d-8B70-203e86b0df58/BigBuckBunny.ISM/manifest(format=mpd-Time-CSF)

* Apple HTTP Live Streaming (HLS) V4

{strömmande slutpunkten namn media services-konto name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest(format=m3u8-aapl)

http://testendpoint-testaccount.Streaming.mediaservices.Windows.NET/fecebb23-46F6-490d-8B70-203e86b0df58/BigBuckBunny.ISM/manifest(format=m3u8-aapl)

* Apple HTTP Live Streaming (HLS) V3

{strömmande slutpunkten namn media services-konto name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest(format=m3u8-aapl-v3)

http://testendpoint-testaccount.Streaming.mediaservices.Windows.NET/fecebb23-46F6-490d-8B70-203e86b0df58/BigBuckBunny.ISM/manifest(format=m3u8-aapl-v3)

## <a name="media-services-learning-paths"></a>Sökvägar för Media Services-utbildning
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Ge feedback
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

