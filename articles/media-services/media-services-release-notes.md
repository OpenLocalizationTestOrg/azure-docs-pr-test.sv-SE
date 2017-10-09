---
title: aaaMedia Services viktig information | Microsoft Docs
description: Media Services viktig information
services: media-services
documentationcenter: 
author: Juliako
manager: cfowler
editor: 
ms.assetid: 3ca2d7af-1cf0-45fa-9585-3b73f3ee057d
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: media
ms.devlang: dotnet
ms.topic: article
ms.date: 07/20/2017
ms.author: juliako
ms.openlocfilehash: c365b1133987267173ec858298c4c6de62744946
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-media-services-release-notes"></a>Azure Media Services viktig information
Dessa versionsanmärkningar sammanfattar ändringar från tidigare versioner och kända problem.

> [!NOTE]
> Vi vill toohear från våra kunder och fokusera på att åtgärda problem som påverkar dig. tooreport ett problem eller ställa frågor kan du efter i hello [Azure Media Services MSDN-Forum].
> 
> 

## <a id="issues"></a>Kända problem
### <a id="general_issues"></a>Allmänna problem med Media Services
| Problem | Beskrivning |
| --- | --- |
| Några vanliga HTTP-huvuden finns inte i hello REST API. |Om du utvecklar Media Services-program med hjälp av hello REST-API kan du upptäcka att vissa vanliga http-huvudfält (inklusive CLIENT-REQUEST-ID, ID för FÖRFRÅGAN och RETUR-CLIENT-REQUEST-ID) stöds inte. hello huvuden läggs till i en kommande uppdatering. |
| Kodning i procent är inte tillåtet. |Media Services använder hello värdet hello IAssetFile.Name egenskapen när du skapar URL: er för hello direktuppspelning av innehåll (till exempel http://{AMSAccount}.origin.mediaservices.windows.net/{GUID}/{IAssetFile.Name}/streamingParameters.) Därför tillåts procent-encoding inte. Hej värdet för hello **namn** egenskapen får inte ha någon av följande hello [procent-encoding-reserverade tecken](http://en.wikipedia.org/wiki/Percent-encoding#Percent-encoding_reserved_characters):! *' ();: @& = + $, /? [] # % ”. Dessutom det kan bara finnas ett '.' för hello filnamnstillägg. |
| Hej ListBlobs metod som är en del av hello Azure Storage SDK version 3.x misslyckas. |Media Services genererar SAS-URL: er baserat på hello [2012-02-12](https://docs.microsoft.com/rest/api/storageservices/Version-2012-02-12) version. Om du vill toouse Azure Storage SDK: N toolist blobbar i en blobbbehållare använder hello [CloudBlobContainer.ListBlobs](http://msdn.microsoft.com/library/microsoft.windowsazure.storage.blob.cloudblobcontainer.listblobs.aspx) metod som är en del av Azure Storage SDK version 2.x. Hej ListBlobs metod som är en del av hello Azure Storage SDK version 3.x misslyckas. |
| Media Services begränsning mekanism begränsar hello resursanvändningen för program som gör överdriven begäran toohello service. hello-tjänsten kan returnera hello tjänsten är inte tillgänglig (503) HTTP-statuskod. |Mer information finns i hello beskrivning av hello 503 HTTP-statuskod i hello [felkoder för Azure Media Services](media-services-encoding-error-codes.md) avsnittet. |
| När du frågar entiteter finns en gräns på 1000 entiteter som returneras i taget eftersom offentlig REST-v2 begränsar frågans resultat too1000 resultat. |Du behöver toouse **hoppa över** och **ta** (.NET) / **upp** (REST) enligt beskrivningen i [exemplet .NET](media-services-dotnet-manage-entities.md#enumerating-through-large-collections-of-entities) och [REST-API exempel](media-services-rest-manage-entities.md#enumerating-through-large-collections-of-entities). |
| Vissa klienter kan stöta på ett problem Upprepa tagg i manifestet för hello Smooth Streaming. |Mer information finns i [det här](media-services-deliver-content-overview.md#known-issues) avsnittet. |
| Azure Media Services .NET SDK-objekt kan inte serialiseras och därför fungerar inte med Azure Caching. |Om du försöker tooserialize hello SDK AssetCollection objektet tooadd den tooAzure cachelagring, ett undantag genererats. |
| Kodning jobb misslyckas med en sträng med meddelandet ”steget: DownloadFile. Kod: System.NullReferenceException ”. |hello vanliga kodning arbetsflöde är tooupload inkommande video fil(er) tooan indata tillgången och skicka en eller flera kodning jobb för att ange tillgången, utan att ytterligare ändra som indata tillgången. Om du ändrar hello indata tillgången (till exempel genom att lägga till/ta bort/byta namn på filer i hello tillgången) och sedan efterföljande jobb kan misslyckas med ett DownloadFile-fel. hello-lösning är toodelete hello indata tillgångsinformation och överföra indata fil(er) tooa ny tillgång. |

## <a id="rest_version_history"></a>Versionshistorik för REST API
Information om hello versionshistorik för Media Services REST API finns [Azure Media Services REST API-referens].

## <a name="june-2017-release"></a>Versionen för juni 2017

Nu har stöd för Media Services [Azure Active Directory (Azure AD)-baserad autentisering](media-services-use-aad-auth-to-access-ams-api.md).

> [!IMPORTANT]
> Media Services stöder för närvarande hello Azure Access Control service-autentisering. Access Control tillstånd att bli inaktuell på den 1 juni 2018. Vi rekommenderar att du migrerar toohello Azure AD-autentisering så snart som möjligt.

## <a name="march-2017-release"></a>Mars 2017 version

Nu kan du använda Azure Media Standard för[Autogenerera en bithastighet stege](media-services-autogen-bitrate-ladder-with-mes.md) genom att ange hello ”anpassningsbar strömning” förinställningen strängen när du skapar en kodning aktivitet. ”Anpassningsbar Streaming” är hello rekommenderade förinställning om du vill tooencode en video för strömning med Media Services. Om du behöver en encoding förinställt toocustomize för ditt specifika scenario börjar du med [dessa](media-services-mes-presets-overview.md) förinställningar.

Nu kan du använda Azure Media Standard eller Media Encoder Premium arbetsflöde för[skapa en kodning uppgift som genererar fMP4 segment](media-services-generate-fmp4-chunks.md). 


## <a name="febuary-2017-release"></a>Febuary 2017 version

Startar 1 April 2017 raderas alla jobb poster i ditt konto som är äldre än 90 dagar automatiskt, tillsammans med dess associerade aktiviteten poster, även om hello Totalt antal poster som är lägre än hello maximala kvoten. Om du behöver tooarchive hello projektaktivitet/information du kan använda hello kod beskrivs [här](media-services-dotnet-manage-entities.md).

## <a name="january-2017-release"></a>Januari 2017 version

I Microsoft Azure Media Services (AMS), en **Strömningsslutpunkt** representerar en strömmande tjänst som kan leverera innehåll direkt tooa player klientprogrammet eller tooa innehåll innehållsleveransnätverk (CDN) för vidare distribution. Media Services tillhandahåller också sömlös integration av Azure CDN. hello utgående ström från en StreamingEndpoint-tjänst kan vara en direktsänd dataström, en video på begäran eller progressiv hämtning av dina tillgångar i Media Services-kontot. Varje Azure Media Services-konto innehåller standard StreamingEndpoint. Du kan skapa ytterligare Strömningsslutpunkter hello kontot. Det finns två versioner av Strömningsslutpunkter 1.0 och 2.0. Från och med januari 10 2017 alla konton som nyligen skapade AMS innehåller version 2.0 **standard** StreamingEndpoint. Ytterligare strömningsslutpunkter som du lägger till toothis kontot också vara version 2.0. Den här ändringen påverkar inte befintliga konton för hello; befintliga Strömningsslutpunkter blir version 1.0 och kan vara uppgraderade tooversion 2.0. Med den här ändringen kommer det ske beteende, fakturering och funktionen förändringar (Mer information finns i [detta](media-services-streaming-endpoints-overview.md) avsnittet).

Dessutom från och med version hello 2.15, Azure Media Services läggs hello följande egenskaper toohello Strömningsslutpunkt entitet: **CdnProvider**, **CdnProfile**,  **FreeTrialEndTime**, **StreamingEndpointVersion**. Detaljerad översikt över de här egenskaperna finns [detta](https://docs.microsoft.com/rest/api/media/operations/streamingendpoint). 

## <a name="december-2016-release"></a>December 2016-versionen

Azure Media Services kan du nu tooaccess telemetri/mätvärdesdata för dess tjänster. hello nuvarande version av AMS kan du samla in telemetridata för live-kanal, StreamingEndpoint, och live Arkiv entiteter. Mer information finns i [detta](media-services-telemetry-overview.md) avsnitt.

## <a id="july_changes16"></a>Versionen för juli 2016
### <a name="updates-toomanifest-file-ism-generated-by-encoding-tasks"></a>Uppdateringar toomanifest fil (*. ISM) som genereras av kodningsuppgifter
När en kodning uppgift är skickade tooMedia Encoder Standard eller Azure Media Encoder, hello kodning aktivitet genererar en [strömmande manifestfilen](media-services-deliver-content-overview.md) (* .ism)-fil i hello utdata tillgången. Med hello senaste versionen av tjänsten, har hello syntaxen för den här strömmande manifestfilen uppdaterats.

> [!NOTE]
> hello syntaxen för hello strömning manifestet (.ism)-fil är reserverad för internt bruk och är ämne toochange i framtida versioner. Du inte ändra eller manipulera hello innehållet i den här filen.
> 
> 

### <a name="a-new-client-manifest-ismc-file-is-generated-in-hello-output-asset-when-an-encoding-task-outputs-one-or-more-mp4-files"></a>En ny klient manifestet (*. ISMC)-filen har genererats i hello utdata tillgång när en kodning uppgift matar ut en eller flera MP4-filer
Från och med hello senaste versionen av tjänsten, efter hello att en kodning uppgiften som genererar en mer MP4-filer utdata hello tillgång innehåller också en strömmande klienten manifestet (*.ismc) fil. Hej .ismc filen förbättrar hello prestanda för dynamiska strömning. 

> [!NOTE]
> hello syntaxen för hello klientfil manifestet (.ismc) är reserverad för internt bruk och är ämne toochange i framtida versioner. Du inte ändra eller manipulera hello innehållet i den här filen.
> 
> 

Mer information finns i [detta](https://blogs.msdn.microsoft.com/randomnumber/2016/07/08/encoder-changes-within-azure-media-services-now-create-ismc-file/) blogg.

### <a name="known-issues"></a>Kända problem
Vissa klienter kan stöta på ett problem Upprepa tagg i manifestet för hello Smooth Streaming. Mer information finns i [det här](media-services-deliver-content-overview.md#known-issues) avsnittet.

## <a id="apr_changes16"></a>April 2016-versionen
### <a name="azure-media-analytics"></a>Azure Media Analytics
Azure Media Servces introducerade Azure Media Analytics för kraftfulla video intelligence. Detaljerad information finns i [översikt över Azure Media Services Analytics](media-services-analytics-overview.md).

### <a name="apple-fairplay-preview"></a>Apple FairPlay (förhandsgranskning)
Azure Media Services nu aktiverar du toodynamically kryptera din HTTP Live Streaming (HLS) innehåll med Apple FairPlay. Du kan också använda AMS licens leverans av tjänsten toodeliver FairPlay-licenser tooclients. Mer information finns i [Använd Azure Media Services tooStream HLS-innehåll som skyddas med Apple FairPlay ](media-services-protect-hls-with-fairplay.md).

## <a id="feb_changes16"></a>Februari 2016-versionen
hello senaste versionen av Azure Media Services SDK för .NET (3.5.3) innehåller en relaterad felkorrigering Widevine. hello problemet var: AssetDeliveryPolicy gick inte att återanvändas för flera resurser som är krypterat med Widevine. Som en del av den här buggfix hello efter egenskapen har lagts till toohello SDK: **WidevineBaseLicenseAcquisitionUrl**.

    Dictionary<AssetDeliveryPolicyConfigurationKey, string> assetDeliveryPolicyConfiguration =
        new Dictionary<AssetDeliveryPolicyConfigurationKey, string>
    {
        {AssetDeliveryPolicyConfigurationKey.WidevineBaseLicenseAcquisitionUrl,"http://testurl"},

    };

## <a id="jan_changes_16"></a>Januari 2016-versionen
Encoding-reserverade enheter byta namn på tooreduce sammanblandning med kodaren namn.

hello Basic, Standard och Premium encoding-reserverade enheter har bytt namn till tooS1, S2, och S3 reserverade enheter, respektive.  Kunder som använder grundläggande kodning RUs idag ser S1 hello etikett i Azure Portal (och i hello faktura) samtidigt som Standard och Premium ser hello etiketter S2 och S3 respektive. 

## <a id="dec_changes_15"></a>December 2015-versionen

### <a name="azure-media-encoder-deprecation-announcement"></a>Azure Media Encoder utfasningen meddelande

Azure Media Encoder att bli inaktuell från och med cirka 12 månader från hello-versionen av Media Encoder Standard.

### <a name="azure-sdk-for-php"></a>Azure SDK för PHP
hello Azure SDK-teamet publicerat en ny version av hello [Azure SDK för PHP](http://github.com/Azure/azure-sdk-for-php) paket som innehåller uppdateringar och nya funktioner för Microsoft Azure Media Services. I synnerhet hello Azure Media Services SDK för PHP nu stöder hello senaste [innehåll skydd](media-services-content-protection-overview.md) funktioner: dynamisk kryptering med AES och DRM (PlayReady och Widevine) med och utan begränsning för Token. Det stöder också skalning [Encoding enheter](media-services-dotnet-encoding-units.md).

Mer information finns i:

* Hej [Microsoft Azure Media Services SDK för PHP](http://southworks.com/blog/2015/12/09/new-microsoft-azure-media-services-sdk-for-php-release-available-with-new-features-and-samples/) blogg.
* hello följande [kodexempel](http://github.com/Azure/azure-sdk-for-php/tree/master/examples/MediaServices) toohelp komma igång snabbt:
  * **vodworkflow_aes.php**: Detta är en PHP-fil som visar hur toouse AES-128 dynamisk kryptering och leverans av tjänsten. Den är baserad på hello .NET-exempel som beskrivs i [detta](media-services-protect-with-aes128.md) artikel.
  * **vodworkflow_aes.php**: Detta är en PHP-fil som visar hur toouse PlayReady dynamisk kryptering och Licensleveranstjänst. Den är baserad på hello .NET-exempel som beskrivs i [detta](media-services-protect-with-drm.md) artikel.
  * **scale_encoding_units.php**: Detta är en PHP-fil som visar hur tooscale kodning reserverad enhet.

## <a id="nov_changes_15"></a>November 2015-versionen
Azure Media Services erbjuder Google Widevines licensleveranstjänst i hello molnet. Mer information finns för[meddelande bloggen](https://azure.microsoft.com/blog/announcing-google-widevine-license-delivery-services-public-preview-in-azure-media-services/). Se även [självstudierna](media-services-protect-with-drm.md) och [GitHub-lagringsplatsen](http://github.com/Azure-Samples/media-services-dotnet-dynamic-encryption-with-drm). 

Observera att Widevines licensleveranstjänster i Azure Media Services från Microsoft i förhandsgranskningen. Mer information finns i [bloggen](https://azure.microsoft.com/blog/announcing-google-widevine-license-delivery-services-public-preview-in-azure-media-services/).

## <a id="oct_changes_15"></a>Oktober 2015-versionen
Azure Media Services (AMS) har publicerats i hello följande datacenter: södra, västra Indien, södra Indien och centrala Indien. Du kan nu använda hello Azure-portalen för[skapa Media Service-konton](media-services-portal-create-account.md) och utföra olika uppgifter som beskrivs [här](https://azure.microsoft.com/documentation/services/media-services/). Dock är Live Encoding inte aktiverat i dessa datacenter. Dessutom finns inte alla typer av Encoding-reserverade enheter i dessa datacenter.

* Södra Brasilien:                                          Endast Encoding-reserverade enheter av grundläggande och standardtyp är tillgängliga
* Västra Indien, södra Indien och centrala Indien:              Endast Encoding-reserverade enheter av grundläggande typ är tillgängliga

## <a id="september_changes_15"></a>Utgåvan från september 2015
* AMS nu erbjudanden hello möjlighet tooprotect både Video-On-Demand (VOD) och Live dataströmmar med Widevine modulära DRM-teknik. Du kan använda hello efter leverans services partner toohelp du leverera Widevine-licenser: [Axinom](http://www.axinom.com/press/ibc-axinom-drm-6/), [EZDRM](http://ezdrm.com/), [castLabs](http://castlabs.com/company/partners/azure/). Mer information finns i [bloggen](https://azure.microsoft.com/blog/azure-media-services-adds-google-widevine-packaging-for-delivering-multi-drm-stream/).
  
    Du kan använda [AMS .NET SDK](https://www.nuget.org/packages/windowsazure.mediaservices/) (startar med hello version 3.5.1) eller REST API tooconfigure din AssetDeliveryConfiguration toouse Widevine.  
* AMS tillagt stöd för Apple ProRes videor. Du kan överföra QuickTime videor källfilerna som använder Apple ProRes eller andra codec. Mer information finns i [bloggen](https://azure.microsoft.com/blog/announcing-support-for-apple-prores-videos-in-azure-media-services/).
* Du kan nu använda Media Encoder Standard toodo underordnad avklippta och live Arkiv extrahering. Mer information finns i [bloggen](https://azure.microsoft.com/blog/sub-clipping-and-live-archive-extraction-with-media-encoder-standard/).
* hello efter filtrering uppdateringar har gjorts: 
  
  * Nu kan du använda Apple HTTP Live Streaming (HLS)-format med ljuddata filter. Den här uppdateringen kan du tooremove ljuddata spåra genom att ange (ljuddata = false) i hello-URL.
  * När du definierar filter för dina tillgångar, nu har du möjlighet toocombine flera (dig too3) filtrerar i en enskild URL.
    
    Mer information finns i [detta](https://azure.microsoft.com/blog/azure-media-services-release-dynamic-manifest-composition-remove-hls-audio-only-track-and-hls-i-frame-track-support/) blogg.
* AMS stöder nu jag ramar i HLS v4. Stöd för jag-ramar optimerar spola framåt och bakåt. Som standard är alla HLS v4-utdata I ramar spelningslista (EXT-X-I-FRAME-STREAM-INF).
  
    Mer information finns i [detta](https://azure.microsoft.com/blog/azure-media-services-release-dynamic-manifest-composition-remove-hls-audio-only-track-and-hls-i-frame-track-support/) blogg.

## <a id="august_changes_15"></a>Augusti 2015-versionen
* Azure Media Services SDK för Java V0.8.0 versionen och nya prov är nu tillgängliga. Mer information finns i:
  
  * [Blogginlägget](http://southworks.com/blog/2015/08/25/microsoft-azure-media-services-sdk-for-java-v0-8-0-released-and-new-samples-available/)
  * [Databasen för Java-exempel](https://github.com/southworkscom/azure-sdk-for-media-services-java-samples)
* Azure Media Player-uppdateringen med stöd för flera ljudström. Mer information finns i:
  * [Blogginlägget](https://azure.microsoft.com/blog/2015/08/13/azure-media-player-update-with-multi-audio-stream-support/)

## <a id="july_changes_15"></a>Versionen för juli 2015
* Om hello allmän tillgänglighet för Media Encoder Standard. Mer information finns i [i det här blogginlägget](https://azure.microsoft.com/blog/2015/07/16/announcing-the-general-availability-of-media-encoder-standard/).
  
    Media Encoder Standard använder förinställningar som beskrivs i [detta](http://go.microsoft.com/fwlink/?LinkId=618336) avsnitt. Observera att när du använder en förinställning för 4k kodar, ska du hämta hello **Premium** reserverade enhetstyp. Mer information finns i [hur tooScale kodning](media-services-scale-media-processing-overview.md).
* Live realtid etiketter med Azure Media Services och spelare. Mer information finns i [i det här blogginlägget](https://azure.microsoft.com/blog/2015/07/08/live-real-time-captions-with-azure-media-services-and-player/)

### <a name="media-services-net-sdk-updates"></a>Media Services .NET SDK-uppdateringar
Azure Media Services .NET SDK är nu version 3.4.0.0. hello följande funktioner har lagts till i den här versionen:  

* Implementerade stöd för live Arkiv. Observera att du inte kan hämta en tillgång som innehåller ett live-Arkiv.
* Implementerade stöd för dynamiska filter.
* Implementerade funktioner som gör att användare tookeep lagringsbehållaren när du tar bort tillgången.
* Felkorrigeringar relaterade tooretry principer i kanaler.
* Aktiverad **Media Encoder Premium arbetsflöde**.

## <a id="june_changes_15"></a>Versionen för juni 2015
### <a name="media-services-net-sdk-updates"></a>Media Services .NET SDK-uppdateringar
Azure Media Services .NET SDK är nu version 3.3.0.0. hello följande funktioner har lagts till i den här versionen:  

* stöd för identifiering av OpenId Connect-specifikationen
* stöd för hantering av nycklar förnyelse på identitet providern sida. 

Om du använder en identitetsleverantör som exponerar OpenID Connect discovery-dokumentet (som hello gör du följande providers: Azure Active Directory, Google, Salesforce), du kan instruera Azure Media Services tooobtain Signeringsnycklar för verifiering av JWT-token från OpenID connect discovery-specifikationen. 

Mer information finns i [med hjälp av Json Web nycklar från OpenID Connect identifiering spec toowork med JWT-token autentisering i Azure Media Services](http://gtrifonov.com/2015/06/07/using-json-web-keys-from-openid-connect-discovery-spec-to-work-with-jwt-token-authentication-in-azure-media-services/).

## <a id="may_changes_15"></a>Maj 2015-versionen
Om hello följande nya funktioner:

* [En förhandsgranskning av Live Encoding med Media Services](media-services-manage-live-encoder-enabled-channels.md)
* [Dynamisk manifest](media-services-dynamic-manifest-overview.md)
* [En förhandsgranskning av Azure Media Hyperlapse media-processor](https://azure.microsoft.com/blog/?p=286281&preview=1&_ppp=61e1a0b3db)

## <a id="april_changes_15"></a>April 2015-versionen
### <a name="general-media-services-updates"></a>Allmänna Media Services-uppdateringar
* [Om Azure Media Player](https://azure.microsoft.com/blog/2015/04/15/announcing-azure-media-player/).
* Från och med Media Services REST 2.10 kanaler som är konfigurerade tooingest en RTMP-protokollet skapas med primära och sekundära infognings-URL: er. Mer information finns i [kanal infognings-konfigurationer](media-services-live-streaming-with-onprem-encoders.md#channel_input)
* Azure Media Indexer uppdateringar
* Stöd för spanska språk
* Ny konfiguration av xml-format

Mer information finns i [bloggen](https://azure.microsoft.com/blog/2015/04/13/azure-media-indexer-spanish-v1-2/).

### <a name="media-services-net-sdk-updates"></a>Media Services .NET SDK-uppdateringar
Azure Media Services .NET SDK är nu version 3.2.0.0.

hello följande är några av hello kundinriktade uppdateringar:

* **Att ändra**: ändra **TokenRestrictionTemplate.Issuer** och **TokenRestrictionTemplate.Audience** toobe av typen string.
* Uppdateringar relaterade toocreating anpassade försök principer.
* Felkorrigeringar relaterade toouploading/hämta filer.
* Hej **MediaServicesCredentials** klass accepterar nu primära och sekundära access control endpoint tooauthenticate mot.

## <a id="march_changes_15"></a>Mars 2015-versionen
### <a name="general-media-services-updates"></a>Allmänna Media Services-uppdateringar
* Media Services tillhandahåller nu Azure CDN-integration. toosupport hello-integration hello **CdnEnabled** egenskapen lades till för**StreamingEndpoint**.  **CdnEnabled** kan användas med REST API: er från och med version 2.9 (Mer information finns i [StreamingEndpoint](https://docs.microsoft.com/rest/api/media/operations/streamingendpoint)).  **CdnEnabled** kan användas med .NET SDK från och med version 3.1.0.2 (Mer information finns i [StreamingEndpoint](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.mediaservices.client.istreamingendpoint\(v=azure.10\).aspx)).
* Meddelande om **Media Encoder Premium arbetsflöde**. Mer information finns i [introduktion till Premium kodning i Azure Media Services](https://azure.microsoft.com/blog/2015/03/05/introducing-premium-encoding-in-azure-media-services/).

## <a id="february_changes_15"></a>Februari 2015-versionen
### <a name="general-media-services-updates"></a>Allmänna Media Services-uppdateringar
Media Services REST API är nu version 2.9. Från och med den här versionen kan aktivera du hello Azure CDN-integrering med strömningsslutpunkter. Mer information finns i [StreamingEndpoint](https://msdn.microsoft.com/library/dn783468.aspx).

## <a id="january_changes_15"></a>Januari 2015-versionen
### <a name="general-media-services-updates"></a>Allmänna Media Services-uppdateringar
Meddelande för allmän tillgänglighet (GA) för innehållsskydd med dynamisk kryptering. Mer information finns i [Azure Media Services förbättrar strömmande säkerhet med teknik för allmän tillgänglighet DRM](https://azure.microsoft.com/blog/2015/01/29/azure-media-services-enhances-streaming-security-with-general-availability-of-drm-technology/).

### <a name="media-services-net-sdk-updates"></a>Media Services .NET SDK-uppdateringar
Azure Media Services .NET SDK är nu version 3.1.0.1.

Den här versionen markerade hello Microsoft.WindowsAzure.MediaServices.Client.ContentKeyAuthorization.TokenRestrictionTemplate standardkonstruktor som föråldrad. nya hello-konstruktorn har TokenType som ett argument.

    TokenRestrictionTemplate template = new TokenRestrictionTemplate(TokenType.SWT);


## <a id="december_changes_14"></a>December 2014-versionen
### <a name="general-media-services-updates"></a>Allmänna Media Services-uppdateringar
* Vissa uppdateringar och nya funktioner har lagts till toohello Azure indexeraren medieprocessor. Mer information finns i [Azure Media Indexer Version 1.1.6.7 viktig information](https://azure.microsoft.com/blog/2014/12/03/azure-media-indexer-version-1-1-6-7-release-notes/).
* Lägga till ett nytt REST-API som möjliggör tooupdate kodningsreserverade enheter: [EncodingReservedUnitType med övriga](https://docs.microsoft.com/rest/api/media/operations/encodingreservedunittype).
* Stöd för tillagda CORS för viktiga leverans av tjänsten.
* Prestandaförbättringar frågor till alternativ för auktorisering av principer som gjordes.
* I Kina Datacenter hello [nyckel leverans URL](https://docs.microsoft.com/rest/api/media/operations/contentkey#get_delivery_service_url) är nu per kund (precis som i andra datacenter).
* Tillagda HLS automatiskt mål varaktighet. När du gör direktsänd strömning måste paketeras HLS alltid dynamiskt. Som standard beräknas automatiskt Media Services HLS segment paketering förhållandet (FragmentsPerSegment) baserat på hello keyframe intervall (KeyFrameInterval), kallas även tooas gruppera bilder – GOP som tas emot från hello livekodaren. Mer information finns i [arbetar med Azure Media Services Liveströmning].

### <a name="media-services-net-sdk-updates"></a>Media Services .NET SDK-uppdateringar
* [Azure Media Services .NET SDK](http://www.nuget.org/packages/windowsazure.mediaservices/) är nu version 3.1.0.0.
* Uppgradera hello .net SDK beroende too.NET 4.5 Framework.
* Lägga till ett nytt API som möjliggör tooupdate kodningsreserverade enheter. Mer information finns i [uppdatering reserverade enhetstyp och öka kodning RUs med hjälp av .NET](media-services-dotnet-encoding-units.md).
* Tillagda JWT (JSON Web Token) stöd för token-autentisering. Mer information finns i [JWT-token autentisering i Azure Media Services och dynamisk kryptering](http://www.gtrifonov.com/2015/01/03/jwt-token-authentication-in-azure-media-services-and-dynamic-encryption/).
* Tillagda relativa förskjutningar för BeginDate och ExpirationDate i mallen för hello PlayReady-licens.

## <a id="november_changes_14"></a>November 2014-versionen
* Media Services kan du nu tooingest en levande Smooth Streaming (FMP4) innehåll via en SSL-anslutning. tooingest via SSL, se till att tooupdate hello infognings-URL: en tooHTTPS.  Observera att för närvarande AMS inte stöder SSL med anpassade domäner.  Läs mer om direktsänd strömning [arbetar med Azure Media Services Liveströmning].
* Du kan för närvarande mata in en direktsänd dataström med RTMP via en SSL-anslutning.
* Du kan bara strömma via SSL om hello strömningsslutpunkt från vilken du kan leverera ditt innehåll skapades efter 10 September 2014. Om din strömmande URL: er baseras på hello strömningsslutpunkter som skapats efter 10 September innehåller hello Webbadressen ”streaming.mediaservices.windows.net” (hello nya format). Strömmande URL: er som innehåller ”origin.mediaservices.windows.net” (hello gamla format) stöder inte SSL. Om URL: en är gammalt hello-format och du vill ha toobe kan toostream över SSL, [skapa en ny strömmande slutpunkt](media-services-portal-manage-streaming-endpoints.md). Använd URL: er skapas baserat på hello strömning nya endpoint toostream ditt innehåll via SSL.

## <a id="october_changes_14"></a>Oktober 2014-versionen
### <a id="new_encoder_release"></a>Media Services-kodaren versionen
Om hello nya versionen av Media Services Azure Media Encoder. Med hello senaste Azure Media Encoder endast debiteras du för utgående GB, men annars hello nya kodare är funktionen som är kompatibel med tidigare hello-kodaren. Mer information [prisinformation för Media Services]).

### <a id="oct_sdk"></a>Media Services .NET SDK
Media Services SDK för .NET-tilläggen är nu version 2.0.0.3.

Media Services SDK för .NET är nu version 3.0.0.8.

hello följande ändringar har gjorts:

* Eftersom i klasser som princip försök igen.
* Att lägga till användaren agent sträng toohttp huvuden för begäran.
* Lägga till nuget återställning build steg.
* Åtgärda scenariot testerna toouse x509 certifikat från databasen.
* Verifiera inställningarna vid uppdatering av kanal och strömmande slutpunkt.

### <a name="new-github-repository-toohost-media-services-samples"></a>Nya GitHub-lagringsplatsen toohost Media Services-exempel
Exempel finns i [GitHub-lagringsplatsen för Azure Media Services-exempel](https://github.com/Azure/Azure-Media-Services-Samples).

## <a id="september_changes_14"></a>September 2014-versionen
Media Services REST-metadata är nu version 2.7. Mer information om hello senaste REST uppdateringar finns [Azure Media Services REST API-referens].

Media Services SDK för .NET är nu version 3.0.0.7

### <a id="sept_14_breaking_changes"></a>Gör ändringar
* **Ursprung** bytte för[StreamingEndpoint].
* En ändring i hello standardbeteendet när du använder hello **Azure-portalen** tooencode och sedan publicera MP4-filer.

Tidigare, när du använder hello Azure klassiska Portal toopublish en enda fil MP4 videotillgång en SAS-URL skulle skapas (SAS-URL: er kan toodownload hello video från ett blob storage). När du använder hello Azure klassiska Portal tooencode och publicera en videotillgång av MP4-fil genererats för närvarande hello URL: en pekar tooan Azure Media Services strömmande slutpunkten.  Den här ändringen påverkar inte MP4-videor som är direkt överförda tooMedia tjänster och publiceras utan som kodats med Azure Media Services.

Du har för närvarande hello följande två alternativ toosolve hello problem.

* Aktivera enheter för strömning och använda dynamisk paketering toostream hello .mp4 tillgång som smooth streaming-presentation.
* Skapa en SAS-url-toodownload (eller uppspelning progressivt) hello .mp4. Mer information om hur toocreate en SAS-lokaliserare finns [leverera innehåll].

### <a id="sept_14_GA_changes"></a>Nya funktioner/scenarier som ingår i GA-version
* **Indexeraren Medieprocessor**. Mer information finns i [indexering mediefiler med Azure Media Indexer].
* Hej [StreamingEndpoint] entitet kan du nu tooadd anpassad domän (värd) namn.
  
    För en anpassad domän namnet toobe används som hello Media Services strömning namnet på slutpunkten, behöver du tooadd anpassad värd namn tooyour strömmande slutpunkten. Använda hello Media Services REST API: er eller .NET SDK tooadd anpassade värdnamn.
  
    det gäller hello följande överväganden:
  
  * Du måste ha hello ägarskap för hello domännamn.
  * hello ägarskap för hello domännamn måste verifieras av Azure Media Services. toovalidate hello domän, skapa en CNAME-post som mappar <MediaServicesAccountId>.<parent domain> tooverifydns. < mediaservices dns-zon >. 
  * Du måste skapa en annan CNAME-post som mappar hello anpassad värd namn (till exempel sports.contoso.com) tooyour Media Services Streamingendponts värdnamn (exempelvis amstest.streaming.mediaservices.windows.net).

    Mer information finns i hello **CustomHostNames** egenskap i hello [StreamingEndpoint] avsnittet.

### <a id="sept_14_preview_changes"></a>Nya funktioner/scenarier som ingår i hello offentliga förhandsversionen
* Dynamisk strömmande förhandsgranskning. Mer information finns i [arbetar med Azure Media Services Liveströmning].
* Viktiga Delivery Service. Mer information finns i [med hjälp av dynamisk AES-128-kryptering och leverans av tjänsten].
* Dynamisk AES-kryptering. Mer information finns i [med hjälp av dynamisk AES-128-kryptering och leverans av tjänsten].
* PlayReady Licensleveranstjänst. Mer information finns i [med PlayReady dynamisk kryptering och Licensleveranstjänst].
* PlayReady-dynamisk kryptering. Mer information finns i [med PlayReady dynamisk kryptering och Licensleveranstjänst].
* Media Services PlayReady License-mall. Mer information finns i [Media Services PlayReady licens mall översikt].
* Strömning lagring krypteras tillgångar. Mer information finns i [strömning lagring krypterat innehåll].

## <a id="august_changes_14"></a>Augusti 2014-versionen
När du koda en tillgång, skapas en utdatatillgången vid slutförandet av hello kodningsjobbet. Azure Media Services Encoder gav metadata om utdata tillgångar förrän den här versionen. Från och med den här versionen hello kodaren ger också metadata om inkommande tillgångar. Mer information finns i hello [indata Metadata] och [utdata Metadata] avsnitt.

## <a id="july_changes_14"></a>Versionen som juli 2014
hello följande felkorrigeringar har gjorts för hello Azure Media Services Paketeraren och Krypterare:

* Bara ljud spelar tillbaka när transmuxing en levande Arkiv tillgången tooHTTP direktsänd strömning – problemet har åtgärdats och nu både ljud och video spelas upp.
* När Paketera en tillgång tooHTTP Liveströmning och AES 128-bitars kryptering för kuvert hello paketeras dataströmmar spelas inte upp på Android-enheter – det här felet är åtgärdat och hello paketerade dataströmmen spelas upp på Android-enheter som stöder HTTP Live Streaming.

## <a id="may_changes_14"></a>Kan 2014-versionen
### <a id="may_14_changes"></a>Allmänna Media Services-uppdateringar
Du kan nu använda [dynamisk paketering] toostream HTTP Live Streaming (HLS) v3. toostream HLS v3, lägga till hello följande format toohello lokaliserare sökväg: * .ism/manifest(format=m3u8-aapl-v3). Mer information finns i [Nick Drouin blogg].

Dynamisk paketering nu också stöder leverera HLS (v3- och v4) som krypterats med PlayReady utifrån Smooth Streaming statiskt krypteras med PlayReady. Mer information om hur tooencrypt Smooth Streaming med PlayReady finns [Protecting Smooth Stream med PlayReady].

### <a name="may_14_donnet_changes"></a>Media Services .NET SDK-uppdateringar
Följande förbättringar hello ingår i hello Media Services .NET SDK 3.0.0.5 versionen:

* Bättre hastighet och återhämtning för att ladda upp/hämta media tillgångar.
* Förbättringar i försök logik och tillfälligt undantagshantering: 
  
  * Tillfälligt fel identifiering och försök igen logik har förbättrats för undantag som orsakas av frågor, spara ändringar, överföra och hämta filer. 
  * När du hämtar Web undantag (till exempel under en ACS-tokenbegäran), ser du att allvarliga fel misslyckas snabbare nu.

Mer information finns i [försök logiken i hello Media Services SDK för .NET].

## <a id="april_changes_14"></a>Versionen för april 2014 kodaren
### <a name="april_14_enocer_changes"></a>Media Services-kodaren uppdateringar
* Stöd har lagts till för att föra in AVI filer som har skapats med hjälp av hello bete DAL EDIUS linjära redigerare, där hello video är lätt komprimerade med bete DAL HQ/HQX codec. Mer information finns i [hello bete DAL annonserar EDIUS 7 strömning via molnet].
* Stöd har lagts till för att ange hello namngivningskonvention hello-filer som skapas av hello Media Encoder. Mer information finns i [styra Media Service-kodaren utdatafilnamn].
* Stöd för video eller ljud överlägg har lagts till. Mer information finns i [skapar överlägg].
* Stöd har lagts till för att gå till samman flera video segment. Mer information finns i [segment för att gå till Video].
* Fast ett programfel relaterade tootranscoding av MP4s där hello ljud har kodats med ljud MPEG-1 layer 3 (aka MP3).

## <a id="jan_feb_changes_14"></a>Januari/februari 2014 versioner
### <a name="jan_fab_14_donnet_changes"></a>Azure Media Services .NET SDK 3.0.0.1, 3.0.0.2 och 3.0.0.3
hello ändringar i 3.0.0.1 och 3.0.0.2 är:

* Fast problem med toousage LINQ-frågor med OrderBy-uttryck.
* Dela testlösningar i [GitHub] i enhetsbaserad testerna och scenariobaserade tester.

Mer information om ändringar av hello finns: [Azure Media Services .NET SDK 3.0.0.1 och 3.0.0.2 släpper].

hello följande ändringar har gjorts i 3.0.0.3:

* Uppgradera Azure storage beroenden toouse version 3.0.3.0. 
* Bakåtkompatibilitet problem för 3.0 har åtgärdats. *.* versioner. 

## <a id="december_changes_13"></a>December 2013 Release
### <a name="dec_13_donnet_changes"></a>Azure Media Services .NET SDK 3.0.0.0
> [!NOTE]
> 3.0.x.x versioner är inte bakåtkompatibla med 2.4.x.x versioner.
> 
> 

hello senaste versionen av hello Media Services SDK är nu 3.0.0.0. Du kan hämta senaste hello-paket från Nuget eller hämta hello bits från [GitHub].

Från och med hello Media Services SDK version 3.0.0.0 kan du återanvända hello [Azure Active Directory Access Control Service (ACS)] token. Mer information finns i hello ”återanvända Access Control Service Tokens” avsnittet i hello [anslutningstjänsterna tooMedia med hello Media Services SDK för .NET] avsnittet.

### <a name="dec_13_donnet_ext_changes"></a>Azure Media Services .NET SDK-tilläggen 2.0.0.0
hello Azure Media Services .NET SDK-tilläggen är en uppsättning tilläggsmetoder och hjälpfunktioner som förenklar koden och gör det enklare toodevelop med Azure Media Services. Du kan hämta hello senaste bits från [Azure Media Services .NET SDK-tilläggen].

## <a id="november_changes_13"></a>Versionen för november 2013
### <a name="nov_13_donnet_changes"></a>Azure Media Services .NET SDK ändringar
Från och med den här versionen, hanterar hello Media Services SDK för .NET tillfälligt fel fel som kan uppstå när du gör anrop toohello Media Services REST API-nivå.

## <a id="august_changes_13"></a>Augusti 2013 Release
### <a name="aug_13_powershell_changes"></a>Media Services PowerShell cmdletar som ingår i Azure Sdk-verktyg
hello följande Media Services PowerShell-cmdletar ingår nu i [azure-sdk-verktyg].

* Get-AzureMediaServices 
  
    Till exempel `Get-AzureMediaServicesAccount`.
* New-AzureMediaServicesAccount 
  
    Till exempel `New-AzureMediaServicesAccount -Name “MediaAccountName” -Location “Region” -StorageAccountName “StorageAccountName”`.
* New-AzureMediaServicesKey 
  
    Till exempel `New-AzureMediaServicesKey -Name “MediaAccountName” -KeyType Secondary -Force`.
* Remove-AzureMediaServicesAccount 
  
    Till exempel `Remove-AzureMediaServicesAccount -Name “MediaAccountName” -Force`.

## <a id="june_changes_13"></a>Versionen för juni 2013
### <a name="june_13_general_changes"></a>Ändringar av Azure Media Services
hello ändringar som nämns i det här avsnittet är uppdateringar som ingår i hello juni 2013 Media Services-versioner.

* Möjlighet toolink flera lagringskonton tooa Media Service-konto. 
  
    StorageAccount
  
    Asset.StorageAccountName och Asset.StorageAccount
* Möjlighet tooupdate Job.Priority. 
* Meddelanden relaterade entiteter och egenskaper: 
  
    JobNotificationSubscription
  
    NotificationEndPoint
  
    Jobb
* Asset.Uri 
* Locator.Name 

### <a name="june_13_dotnet_changes"></a>Ändringar av Azure Media Services .NET SDK
hello följande ändringar ingår i juni 2013 versioner av Media Services SDK. hello finns senaste Media Services SDK på GitHub.

* Från och med version hello 2.3.0.0 hello Media Services SDK har stöd för länkning av flera storage-konton tooa Media Services-konto. hello stöder följande API: er den här funktionen:
  
    Hej IStorageAccount typen.
  
    Hej Microsoft.WindowsAzure.MediaServices.Client.CloudMediaContext.StorageAccounts egenskapen.
  
    Hej StorageAccount egenskapen.
  
    Hej StorageAccountName egenskapen.
  
    Mer information finns i [hantera Media Services tillgångar över flera Lagringskonton].
* Meddelanden relaterade API: er. Från och med version hello 2.2.0.0 som du har hello möjlighet toolisten tooAzure Queue storage meddelanden. Mer information finns i [hantering av Media Services jobbet meddelanden].
  
    Hej Microsoft.WindowsAzure.MediaServices.Client.IJob.JobNotificationSubscriptions egenskapen.
  
    Hej Microsoft.WindowsAzure.MediaServices.Client.INotificationEndPoint typen.
  
    Hej Microsoft.WindowsAzure.MediaServices.Client.IJobNotificationSubscription typen.
  
    Hej Microsoft.WindowsAzure.MediaServices.Client.NotificationEndPointCollection typen.
  
    Hej Microsoft.WindowsAzure.MediaServices.Client.NotificationEndPointType typen.
  
    Hej Microsoft.WindowsAzure.MediaServices.Client.NotificationJobState typen.
* Beroende på hello Azure Storage Client SDK 2.0 (Microsoft.WindowsAzure.StorageClient.dll).
* Beroendet av OData 5.5 (Microsoft.Data.OData.dll).

## <a id="december_changes_12"></a>December 2012-versionen
### <a name="dec_12_dotnet_changes"></a>Ändringar av Azure Media Services .NET SDK
* IntelliSense: Lägga till saknas Intellisense dokumentationen för många typer.
* Microsoft.Practices.TransientFaultHandling.Core: Ett problem har åtgärdats där hello SDK fortfarande hade en beroende tooan gammal version av den här sammansättningen. hello SDK nu referenser version 5.1.1209.1 av den här sammansättningen.

Korrigeringar för problem som hittas i hello November 2012 SDK:

* IAsset.Locators.Count: Det här antalet nu korrekt rapporteras på nya IAsset gränssnitt när alla lokaliserare har tagits bort.
* IAssetFile.ContentFileSize: Det här värdet nu korrekt anges efter en överföring av IAssetFile.Upload(filepath).
* IAssetFile.ContentFileSize: Den här egenskapen kan nu anges när du skapar en resursfil. Det var tidigare skrivskyddad.
* IAssetFile.Upload(filepath): Ett problem har åtgärdats där den här metoden Överför synkron har utlösande hello följande fel vid överföring av flera filer toohello tillgången. hello felet var ”servern kunde inte tooauthenticate hello begäran. Kontrollera att hello värdet för Authorization-huvud är formaterad korrekt inklusive hello signatur ”.
* IAssetFile.UploadAsync: Ett problem har åtgärdats där fler än 5 filer kan överföras samtidigt.
* IAssetFile.UploadProgressChanged: Den här händelsen tillhandahålls nu av hello SDK.
* IAssetFile.DownloadAsync (sträng, BlobTransferClient ILocator, CancellationToken): den här metodöverlagringen tillhandahålls nu.
* IAssetFile.DownloadAsync: Ett problem har åtgärdats där fler än 5 filer kan hämtas samtidigt.
* IAssetFile.Delete(): Ett problem har åtgärdats där anropa delete kan utlösa ett undantag om ingen fil har överförts för hello IAssetFile.
* Jobb: Ett problem har åtgärdats där länkning ”MP4 tooSmooth dataströmmar aktiviteten” med en ”PlayReady skydd Task” med en jobbmall kan inte skapa några aktiviteter alls.
* EncryptionUtils.GetCertificateFromStore(): Den här metoden genererar inte längre ett null-referensundantag på grund av tooa fel hitta hello certifikat baserat på problem med certifikat.

## <a id="november_changes_12"></a>November 2012-versionen
hello nämns i det här avsnittet har uppdateringar som ingår i hello November 2012 (version 2.0.0.0) SDK. De här ändringarna kan kräva någon kod som skrivs för hello juni 2012 preview SDK version toobe ändras eller skrivas om.

* Tillgångar
  
    IAsset.Create(assetName) är hello bara tillgång skapa funktion. IAsset.Create överför inte längre filerna som en del av hello metodanrop. Använd IAssetFile för överföring.
  
    Hej IAsset.Publish metoden och hello AssetState.Publish uppräkningsvärdet har tagits bort från hello Services SDK. All kod som förlitar sig på detta värde måste vara skrivna igen.
* FileInfo
  
    Den här klassen har tagits bort och ersatts av IAssetFile.
  
    IAssetFiles
  
    IAssetFile ersätter FileInfo och fungerar annorlunda. toouse, initiera hello IAssetFiles objekt, följt av en filöverföring antingen med hjälp av hello Media Services SDK eller hello Azure Storage SDK: N. Du kan använda följande IAssetFile.Upload överlagringar hello:
  
  * IAssetFile.Upload(filePath): En synkron metod som blockerar hello tråd och rekommenderas endast när du överför en fil.
  * IAssetFile.UploadAsync (filePath, blobTransferClient, lokaliserare, cancellationToken): en asynkron metod. Detta är hello önskad överföringsmekanism. 
    
    Känt fel: med hello cancellationToken avbryts verkligen hello överför; hello annullering tillstånd hello uppgifter kan dock vara någon av ett antal tillstånd. Du måste korrekt fånga och hantera undantag.
* Lokaliserare
  
    hello ursprung-specifika versioner har tagits bort. hello SAS-specifika kontext. Locators.CreateSasLocator (tillgången, accessPolicy) markeras som föråldrad eller tagits bort av GA. Se hello lokaliserare avsnittet under nya funktioner för uppdaterade beteendet.

## <a id="june_changes_12"></a>Förhandsversionen för juni 2012
hello följande funktioner som är nytt i hello November versionen av hello SDK.

* Ta bort enheter
  
    IAsset, IAssetFile, ILocator, IAccessPolicy, IContentKey objekt har nu tagits bort på hello objektnivå, d.v.s. IObject.Delete() i stället för att en delete i hello samling som är cloudMediaContext.ObjCollection.Delete(objInstance).
* Lokaliserare
  
    Lokaliserare nu måste skapas med hello CreateLocator metoden och använda hello LocatorType.SAS eller LocatorType.OnDemandOrigin enum-värden som ett argument för hello viss typ av lokaliserare önskade toocreate.
  
    Nya egenskaper lagts tooLocators toomake den enklare tooobtain användbara URI: er för ditt innehåll. Förändringen av positionerare var avsett tooprovide mer flexibilitet för framtida från tredje part utökningsbarhet och öka enkel användning för media klientprogram.
* Stöd för asynkron metod
  
    Asynkron stöd har lagts till tooall metoder.

## <a name="media-services-learning-paths"></a>Sökvägar för Media Services-utbildning
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Ge feedback
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

<!-- Anchors. -->

<!-- Images. -->

<!--- URLs. --->
[Azure Media Services MSDN-Forum]: http://social.msdn.microsoft.com/forums/azure/home?forum=MediaServices
[Azure Media Services REST API-referens]: https://docs.microsoft.com/rest/api/media/operations/azure-media-services-rest-api-reference
[prisinformation för Media Services]: http://azure.microsoft.com/pricing/details/media-services/
[indata Metadata]: http://msdn.microsoft.com/library/azure/dn783120.aspx
[utdata Metadata]: http://msdn.microsoft.com/library/azure/dn783217.aspx
[leverera innehåll]: http://msdn.microsoft.com/library/azure/hh973618.aspx
[indexering mediefiler med Azure Media Indexer]: http://msdn.microsoft.com/library/azure/dn783455.aspx
[StreamingEndpoint]: http://msdn.microsoft.com/library/azure/dn783468.aspx
[arbetar med Azure Media Services Liveströmning]: http://msdn.microsoft.com/library/azure/dn783466.aspx
[med hjälp av dynamisk AES-128-kryptering och leverans av tjänsten]: http://msdn.microsoft.com/library/azure/dn783457.aspx
[med PlayReady dynamisk kryptering och Licensleveranstjänst]: http://msdn.microsoft.com/library/azure/dn783467.aspx
[Preview features]: http://azure.microsoft.com/services/preview/
[Media Services PlayReady licens mall översikt]: http://msdn.microsoft.com/library/azure/dn783459.aspx
[strömning lagring krypterat innehåll]: http://msdn.microsoft.com/library/azure/dn783451.aspx
[Azure portal]: https://manage.windowsazure.com
[dynamisk paketering]: http://msdn.microsoft.com/library/azure/jj889436.aspx
[Nick Drouin blogg]: http://blog-ndrouin.azurewebsites.net/hls-v3-new-old-thing/
[Protecting Smooth Stream med PlayReady]: http://msdn.microsoft.com/library/azure/dn189154.aspx
[försök logiken i hello Media Services SDK för .NET]: http://msdn.microsoft.com/library/azure/dn745650.aspx
[hello bete DAL annonserar EDIUS 7 strömning via molnet]: http://www.streamingmedia.com/Producer/Articles/ReadArticle.aspx?ArticleID=96351&utm_source=dlvr.it&utm_medium=twitter
[styra Media Service-kodaren utdatafilnamn]: http://msdn.microsoft.com/library/azure/dn303341.aspx
[skapar överlägg]: http://msdn.microsoft.com/library/azure/dn640496.aspx
[segment för att gå till Video]: http://msdn.microsoft.com/library/azure/dn640504.aspx
[Azure Media Services .NET SDK 3.0.0.1 och 3.0.0.2 släpper]: http://www.gtrifonov.com/2014/02/07/windows-azure-media-services-.net-sdk-3.0.0.2-release/
[Azure Active Directory Access Control Service (ACS)]: http://msdn.microsoft.com/library/hh147631.aspx
[anslutningstjänsterna tooMedia med hello Media Services SDK för .NET]: http://msdn.microsoft.com/library/azure/jj129571.aspx
[Azure Media Services .NET SDK-tilläggen]: https://github.com/Azure/azure-sdk-for-media-services-extensions/tree/dev
[azure-sdk-verktyg]: https://github.com/Azure/azure-sdk-tools
[GitHub]: https://github.com/Azure/azure-sdk-for-media-services
[hantera Media Services tillgångar över flera Lagringskonton]: http://msdn.microsoft.com/library/azure/dn271889.aspx
[hantering av Media Services jobbet meddelanden]: http://msdn.microsoft.com/library/azure/dn261241.aspx

