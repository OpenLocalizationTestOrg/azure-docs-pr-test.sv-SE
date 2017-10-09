---
title: "aaaConfiguring principerna för tillgångsleverans med hjälp av Media Services REST API | Microsoft Docs"
description: "Det här avsnittet visar hur tooconfigure olika principerna för tillgångsleverans med hjälp av Media Services REST API."
services: media-services
documentationcenter: 
author: Juliako
manager: cfowler
editor: 
ms.assetid: 5cb9d32a-e68b-4585-aa82-58dded0691d0
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/10/2017
ms.author: juliako
ms.openlocfilehash: 8203230d570935e17382c598820dbfe42f83f8d8
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="configuring-asset-delivery-policies"></a>Konfigurera principerna för tillgångsleverans
[!INCLUDE [media-services-selector-asset-delivery-policy](../../includes/media-services-selector-asset-delivery-policy.md)]

Om du planerar toodeliver dynamiskt krypterad tillgångar, steg ett av hello i hello Media Services content delivery arbetsflödet konfigurera leveransprinciperna för tillgångar. Hej tillgångsleveransprincip talar om för Media Services hur du vill använda för din tillgång toobe levereras: i vilket strömningsprotokoll bör din tillgång dynamiskt paketeras (till exempel MPEG DASH, HLS, Smooth Streaming eller alla), oavsett om du vill toodynamically kryptera din tillgång och hur (envelope eller vanliga kryptering).

Det här avsnittet beskrivs hur och varför toocreate och konfigurera leveransprinciperna för tillgången.

>[!NOTE]
>När AMS-kontot skapas en **standard** strömningsslutpunkt har lagts till tooyour konto i hello **stoppad** tillstånd. toostart strömning ditt innehåll och dra nytta av dynamisk paketering och dynamisk kryptering hello strömningsslutpunkt som du vill toostream innehåll har toobe i hello **kör** tillstånd. 
>
>Dessutom toobe kan toouse dynamisk paketering och dynamisk kryptering din tillgång måste innehålla en uppsättning MP4s med anpassningsbar bithastighet eller Smooth Streaming-filer.

Du kan använda olika principer toohello samma tillgång. Du kan till exempel använda PlayReady-kryptering tooSmooth Streaming och AES Envelope kryptering tooMPEG DASH och HLS. Alla protokoll som inte har definierats i en leveransprincip (exempelvis du lägga till en enda princip som endast anger HLS som hello protokoll) kommer att blockeras från strömning. hello undantag toothis är om du har någon tillgångsleveransprincip alls definierats. Sedan tillåts alla protokoll i hello Rensa.

Om du vill toodeliver en krypterad tillgång för lagring, måste du konfigurera hello tillgångsleveransprincip. Innan din tillgång kan strömmas angetts hello streaming server tar bort hello lagringskryptering och dataströmmar innehåll med hello principen för tillgångsleverans. Till exempel toodeliver din tillgång krypteras med Advanced Encryption Standard (AES) kuvert krypteringsnyckeln genom att ange hello principtypen för**DynamicEnvelopeEncryption**. tooremove lagringskryptering och dataströmmen hello tillgångar i hello rensa, ange hello princip för**NoDynamicEncryption**. Exempel som visar hur tooconfigure dessa principtyper följer.

Beroende på hur du konfigurerar hello tillgångsleveransprincip du skulle kunna toodynamically paketet, dynamiskt kryptera och strömma hello följande strömningsprotokoll: Smooth Streaming, HLS, MPEG DASH-dataströmmar.

följande lista visar hello hello formaterar du använder toostream Smooth, HLS, DASH.

Smooth Streaming:

{namn på slutpunkt för direktuppspelning-namn på mediaservicekonto}.streaming.mediaservices.windows.net/lokalisator-ID}/{filnamn}.ism/Manifest

HLS:

{strömmande slutpunkten namn media services-konto name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest(format=m3u8-aapl)

MPEG DASH

{strömmande slutpunkten namn media services-konto name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest(format=mpd-time-csf)


Anvisningar för hur toopublish en tillgång och skapa en strömnings-URL, se [skapa en strömnings-URL](media-services-deliver-streaming-content.md).

## <a name="considerations"></a>Överväganden
* Du kan inte ta bort en AssetDeliveryPolicy som är associerade med en tillgång, medan det finns en (streaming) positionerare för tillgången. hello rekommendation är tooremove hello principen från hello tillgångsinformation innan du tar bort hello princip.
* En strömningslokaliserare kan inte skapas på en krypterad tillgång lagring när någon tillgångsleveransprincip anges.  Om inte hello tillgången är lagringskrypterad, kan hello system du skapa en positionerare och dataströmmen hello tillgång i hello Rensa utan en tillgångsleveransprincip.
* Du kan ha flera principerna för tillgångsleverans som är associerade med en enda resurs, men du kan bara ange ett sätt toohandle angivna AssetDeliveryProtocol.  Vilket innebär att om du toolink två leveransprinciperna som anger hello AssetDeliveryProtocol.SmoothStreaming protokoll som resulterar i ett fel eftersom hello system inte vet vilken vill du tooapply när en klient gör en Smooth Streaming-begäran.
* Om du har en tillgång med en befintlig strömningslokaliserare kan inte länka en ny princip toohello tillgång, Avlänka en befintlig princip från hello tillgång eller uppdatera en leveransprincip som är associerade med hello tillgången.  Du först har tooremove hello strömningslokaliserare, justera hello principer och återskapa hello strömning lokaliserare.  Du kan använda samma locatorId när du skapar nytt hello strömning lokaliserare, men du bör kontrollera hello som inte orsakar problem för klienter eftersom innehållet kan cachelagras av hello ursprung eller en underordnad CDN.

>[!NOTE]

>Vid åtkomst till entiteter i Media Services måste du ange specifika namn på huvudfält och värden i HTTP-begäranden. Mer information finns i [installationsprogrammet för Media Services REST API-utveckling](media-services-rest-how-to-use.md).

## <a name="connect-toomedia-services"></a>Ansluta tooMedia tjänster

Mer information om hur tooconnect toohello AMS API, se [åtkomst hello Azure Media Services API med Azure AD authentication](media-services-use-aad-auth-to-access-ams-api.md). 

>[!NOTE]
>När du har anslutit toohttps://media.windows.net, får du en 301 omdirigering att ange en annan Media Services-URI. Du måste göra följande anrop toohello ny URI.

## <a name="clear-asset-delivery-policy"></a>Rensa tillgångsleveransprincip
### <a id="create_asset_delivery_policy"></a>Skapa tillgångsleveransprincip
hello följande HTTP-begäran skapar en tillgångsleveransprincip som anger toonot använda dynamisk kryptering och toodeliver hello dataström i någon av hello följande protokoll: MPEG DASH, HLS och Smooth Streaming-protokoll. 

Information om vilka värden som du anger när du skapar en AssetDeliveryPolicy finns hello [typer som används när du definierar AssetDeliveryPolicy](#types) avsnitt.   

Begäran:

    POST https://media.windows.net/api/AssetDeliveryPolicies HTTP/1.1
    Content-Type: application/json
    DataServiceVersion: 1.0;NetFx
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json
    Accept-Charset: UTF-8
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=amsaccount1&urn%3aSubscriptionId=zbbef702-e769-2233-9f16-bc4d3aa97387&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1423397827&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=Szo6lbJAvL3dyecAeVmyAnzv3mGzfUNClR5shk9Ivbk%3d
    x-ms-version: 2.11
    x-ms-client-request-id: 4651882c-d7ad-4d5e-86ab-f07f47dcb41e
    Host: media.windows.net

    {"Name":"Clear Policy",
    "AssetDeliveryProtocol":7,
    "AssetDeliveryPolicyType":2,
    "AssetDeliveryConfiguration":null}

Svar:

    HTTP/1.1 201 Created
    Cache-Control: no-cache
    Content-Length: 363
    Content-Type: application/json;odata=minimalmetadata;streaming=true;charset=utf-8
    Location: https://media.windows.net/api/AssetDeliveryPolicies('nb%3Aadpid%3AUUID%3A92b0f6ba-3c9f-49b6-a5fa-2a8703b04ecd')
    Server: Microsoft-IIS/8.5
    x-ms-client-request-id: 4651882c-d7ad-4d5e-86ab-f07f47dcb41e
    request-id: 6aedbf93-4bc2-4586-8845-fd45590136af
    x-ms-request-id: 6aedbf93-4bc2-4586-8845-fd45590136af
    X-Content-Type-Options: nosniff
    DataServiceVersion: 3.0;
    X-Powered-By: ASP.NET
    Strict-Transport-Security: max-age=31536000; includeSubDomains
    Date: Sun, 08 Feb 2015 06:21:27 GMT

    {"odata.metadata":"https://media.windows.net/api/$metadata#AssetDeliveryPolicies/@Element",
    "Id":"nb:adpid:UUID:92b0f6ba-3c9f-49b6-a5fa-2a8703b04ecd",
    "Name":"Clear Policy",
    "AssetDeliveryProtocol":7,
    "AssetDeliveryPolicyType":2,
    "AssetDeliveryConfiguration":null,
    "Created":"2015-02-08T06:21:27.6908329Z",
    "LastModified":"2015-02-08T06:21:27.6908329Z"}

### <a id="link_asset_with_asset_delivery_policy"></a>Länken tillgång med tillgångsleveransprincip
hello efter HTTP-begäran länkar hello angetts toohello tillgången tillgångsleveransprincip till.

Begäran:

    POST https://media.windows.net/api/Assets('nb%3Acid%3AUUID%3A86933344-9539-4d0c-be7d-f842458693e0')/$links/DeliveryPolicies HTTP/1.1
    DataServiceVersion: 1.0;NetFx
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json
    Accept-Charset: UTF-8
    Content-Type: application/json
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=amsaccount1&urn%3aSubscriptionId=zbbef702-e769-3344-9f16-bc4d3aa97387&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1423397827&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=Szo6lbJAvL3dyecAeVmyAnzv3mGzfUNClR5shk9Ivbk%3d
    x-ms-version: 2.11
    x-ms-client-request-id: 56d2763f-6e72-419d-ba3c-685f6db97e81
    Host: media.windows.net

    {"uri":"https://media.windows.net/api/AssetDeliveryPolicies('nb%3Aadpid%3AUUID%3A92b0f6ba-3c9f-49b6-a5fa-2a8703b04ecd')"}

Svar:

    HTTP/1.1 204 No Content


## <a name="dynamicenvelopeencryption-asset-delivery-policy"></a>DynamicEnvelopeEncryption tillgångsleveransprincip
### <a name="create-content-key-of-hello-envelopeencryption-type-and-link-it-toohello-asset"></a>Skapa innehållsnyckeln av hello EnvelopeEncryption typ och länka det toohello tillgångsinformation
När du anger DynamicEnvelopeEncryption leveransprincip måste toomake att toolink din tillgång tooa innehållsnyckeln av hello EnvelopeEncryption typen. Mer information finns: [att skapa en innehållsnyckel](media-services-rest-create-contentkey.md)).

### <a id="get_delivery_url"></a>Hämta URL för leverans
Get hello leverans URL för hello angetts leveransmetod av hello innehållsnyckeln som skapats i hello föregående steg. En klient använder hello returnerade URL toorequest AES-nyckel eller en PlayReady-licens i ordning tooplayback hello skyddat innehåll.

Ange hello hello URL tooget i hello brödtext hello HTTP-begäran. Om du skyddar ditt innehåll med PlayReady Media Services PlayReady licens förvärv URL-begäran med 1 för hello keyDeliveryType: {”keyDeliveryType”: 1}. Om du skyddar ditt innehåll med hello kuvert kryptering URL-begäran en nyckel förvärv genom att ange 2 för keyDeliveryType: {”keyDeliveryType”: 2}.

Begäran:

    POST https://media.windows.net/api/ContentKeys('nb:kid:UUID:dc88f996-2859-4cf7-a279-c52a9d6b2f04')/GetKeyDeliveryUrl HTTP/1.1
    Content-Type: application/json
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json
    Accept-Charset: UTF-8
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=amsaccount1&urn%3aSubscriptionId=zbbef702-2233-477b-9f16-bc4d3aa97387&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1423452029&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=IEXV06e3drSIN5naFRBdhJZCbfEqQbFZsGSIGmawhEo%3d
    x-ms-version: 2.11
    x-ms-client-request-id: 569d4b7c-a446-4edc-b77c-9fb686083dd8
    Host: media.windows.net
    Content-Length: 21

    {"keyDeliveryType":2}

Svar:

    HTTP/1.1 200 OK
    Cache-Control: no-cache
    Content-Length: 198
    Content-Type: application/json;odata=minimalmetadata;streaming=true;charset=utf-8
    Server: Microsoft-IIS/8.5
    x-ms-client-request-id: 569d4b7c-a446-4edc-b77c-9fb686083dd8
    request-id: d26f65d2-fe65-4136-8fcf-31545be68377
    x-ms-request-id: d26f65d2-fe65-4136-8fcf-31545be68377
    X-Content-Type-Options: nosniff
    DataServiceVersion: 3.0;
    Strict-Transport-Security: max-age=31536000; includeSubDomains
    Date: Sun, 08 Feb 2015 21:42:30 GMT

    {"odata.metadata":"media.windows.net/api/$metadata#Edm.String","value":"https://amsaccount1.keydelivery.mediaservices.windows.net/?KID=dc88f996-2859-4cf7-a279-c52a9d6b2f04"}


### <a name="create-asset-delivery-policy"></a>Skapa tillgångsleveransprincip
hello följande HTTP-begäran skapar hello **AssetDeliveryPolicy** som är konfigurerade tooapply dynamiska kryptering (**DynamicEnvelopeEncryption**) toohello **HLS**protocol (i det här exemplet andra protokoll kommer att blockeras från strömning). 

Information om vilka värden som du anger när du skapar en AssetDeliveryPolicy finns hello [typer som används när du definierar AssetDeliveryPolicy](#types) avsnitt.   

Begäran:

    POST https://media.windows.net/api/AssetDeliveryPolicies HTTP/1.1
    Content-Type: application/json
    DataServiceVersion: 1.0;NetFx
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json
    Accept-Charset: UTF-8
    User-Agent: Microsoft ADO.NET Data Services
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=amsaccount1&urn%3aSubscriptionId=zbbef702-2233-477b-9f16-bc4d3aa97387&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1423480651&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=T2FG3tIV0e2ETzxQ6RDWxWAsAzuy3ez2ruXPhrBe62Y%3d
    x-ms-version: 2.11
    x-ms-client-request-id: fff319f6-71dd-4f6c-af27-b675c0066fa7
    Host: media.windows.net

    {"Name":"AssetDeliveryPolicy","AssetDeliveryProtocol":4,"AssetDeliveryPolicyType":3,"AssetDeliveryConfiguration":"[{\"Key\":2,\"Value\":\"https:\\/\\/amsaccount1.keydelivery.mediaservices.windows.net\\/\"}]"}


Svar:

    HTTP/1.1 201 Created
    Cache-Control: no-cache
    Content-Length: 460
    Content-Type: application/json;odata=minimalmetadata;streaming=true;charset=utf-8
    Location: media.windows.net/api/AssetDeliveryPolicies('nb%3Aadpid%3AUUID%3Aec9b994e-672c-4a5b-8490-a464eeb7964b')
    Server: Microsoft-IIS/8.5
    x-ms-client-request-id: fff319f6-71dd-4f6c-af27-b675c0066fa7
    request-id: c2a1ac0e-9644-474f-b38f-b9541c3a7c5f
    x-ms-request-id: c2a1ac0e-9644-474f-b38f-b9541c3a7c5f
    X-Content-Type-Options: nosniff
    DataServiceVersion: 3.0;
    Strict-Transport-Security: max-age=31536000; includeSubDomains
    Date: Mon, 09 Feb 2015 05:24:38 GMT

    {"odata.metadata":"media.windows.net/api/$metadata#AssetDeliveryPolicies/@Element","Id":"nb:adpid:UUID:ec9b994e-672c-4a5b-8490-a464eeb7964b","Name":"AssetDeliveryPolicy","AssetDeliveryProtocol":4,"AssetDeliveryPolicyType":3,"AssetDeliveryConfiguration":"[{\"Key\":2,\"Value\":\"https:\\/\\/amsaccount1.keydelivery.mediaservices.windows.net\\/\"}]","Created":"2015-02-09T05:24:38.9167436Z","LastModified":"2015-02-09T05:24:38.9167436Z"}


### <a name="link-asset-with-asset-delivery-policy"></a>Länken tillgång med tillgångsleveransprincip
Se [länk tillgång med tillgångsleveransprincip](#link_asset_with_asset_delivery_policy)

## <a name="dynamiccommonencryption-asset-delivery-policy"></a>DynamicCommonEncryption tillgångsleveransprincip
### <a name="create-content-key-of-hello-commonencryption-type-and-link-it-toohello-asset"></a>Skapa innehållsnyckeln av hello CommonEncryption typ och länka det toohello tillgångsinformation
När du anger DynamicCommonEncryption leveransprincip måste toomake att toolink din tillgång tooa innehållsnyckeln av hello CommonEncryption typen. Mer information finns: [att skapa en innehållsnyckel](media-services-rest-create-contentkey.md)).

### <a name="get-delivery-url"></a>Hämta URL för leverans
Hämta hello leverans URL för hello PlayReady leveransmetod av hello innehållsnyckeln som skapats i hello föregående steg. En klient använder hello returnerade URL toorequest en PlayReady-licens i ordning tooplayback hello skyddat innehåll. Mer information finns i [Hämta leverans URL](#get_delivery_url).

### <a name="create-asset-delivery-policy"></a>Skapa tillgångsleveransprincip
hello följande HTTP-begäran skapar hello **AssetDeliveryPolicy** som är konfigurerade tooapply dynamisk vanliga kryptering (**DynamicCommonEncryption**) toohello **Smooth Streaming**  protocol (i det här exemplet andra protokoll kommer att blockeras från strömning). 

Information om vilka värden som du anger när du skapar en AssetDeliveryPolicy finns hello [typer som används när du definierar AssetDeliveryPolicy](#types) avsnitt.   

Begäran:

    POST https://media.windows.net/api/AssetDeliveryPolicies HTTP/1.1
    Content-Type: application/json
    DataServiceVersion: 1.0;NetFx
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json
    Accept-Charset: UTF-8
    User-Agent: Microsoft ADO.NET Data Services
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=amsaccount1&urn%3aSubscriptionId=zbbef702-2233-477b-9f16-bc4d3aa97387&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1423480651&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=T2FG3tIV0e2ETzxQ6RDWxWAsAzuy3ez2ruXPhrBe62Y%3d
    x-ms-version: 2.11
    x-ms-client-request-id: fff319f6-71dd-4f6c-af27-b675c0066fa7
    Host: media.windows.net

    {"Name":"AssetDeliveryPolicy","AssetDeliveryProtocol":1,"AssetDeliveryPolicyType":4,"AssetDeliveryConfiguration":"[{\"Key\":2,\"Value\":\"https:\\/\\/amsaccount1.keydelivery.mediaservices.windows.net\/PlayReady\/"}]"}


Om du vill tooprotect innehåll med Widevine DRM, uppdatera hello AssetDeliveryConfiguration värden toouse WidevineLicenseAcquisitionUrl (som har hello värdet 7) och ange hello-URL för en licensleveranstjänst. Du kan använda följande AMS-partner toohelp du leverera Widevine-licenser hello: [Axinom](http://www.axinom.com/press/ibc-axinom-drm-6/), [EZDRM](http://ezdrm.com/), [castLabs](http://castlabs.com/company/partners/azure/).

Exempel: 

    {"Name":"AssetDeliveryPolicy","AssetDeliveryProtocol":2,"AssetDeliveryPolicyType":4,"AssetDeliveryConfiguration":"[{\"Key\":7,\"Value\":\"https:\\/\\/example.net\/WidevineLicenseAcquisition\/"}]"}

> [!NOTE]
> Vid kryptering med Widevine, skulle du bara att kunna toodeliver med bindestreck. Se till att toospecify STRECK (2) i hello protokollet för tillgångsleverans.
> 
> 

### <a name="link-asset-with-asset-delivery-policy"></a>Länken tillgång med tillgångsleveransprincip
Se [länk tillgång med tillgångsleveransprincip](#link_asset_with_asset_delivery_policy)

## <a id="types"></a>Typer som används när du definierar AssetDeliveryPolicy

### <a name="assetdeliveryprotocol"></a>AssetDeliveryProtocol

hello beskrivs följande enum värden som du kan ange för hello protokollet för tillgångsleverans.

    [Flags]
    public enum AssetDeliveryProtocol
    {
        /// <summary>
        /// No protocols.
        /// </summary>
        None = 0x0,

        /// <summary>
        /// Smooth streaming protocol.
        /// </summary>
        SmoothStreaming = 0x1,

        /// <summary>
        /// MPEG Dynamic Adaptive Streaming over HTTP (DASH)
        /// </summary>
        Dash = 0x2,

        /// <summary>
        /// Apple HTTP Live Streaming protocol.
        /// </summary>
        HLS = 0x4,

        ProgressiveDownload = 0x10, 
 
        /// <summary>
        /// Include all protocols.
        /// </summary>
        All = 0xFFFF
    }

### <a name="assetdeliverypolicytype"></a>AssetDeliveryPolicyType

hello beskriver följande enum värden som du kan ange för hello tillgångstyp leverans princip.  

    public enum AssetDeliveryPolicyType
    {
        /// <summary>
        /// Delivery Policy Type not set.  An invalid value.
        /// </summary>
        None,

        /// <summary>
        /// hello Asset should not be delivered via this AssetDeliveryProtocol. 
        /// </summary>
        Blocked, 

        /// <summary>
        /// Do not apply dynamic encryption toohello asset.
        /// </summary>
        /// 
        NoDynamicEncryption,  

        /// <summary>
        /// Apply Dynamic Envelope encryption.
        /// </summary>
        DynamicEnvelopeEncryption,

        /// <summary>
        /// Apply Dynamic Common encryption.
        /// </summary>
        DynamicCommonEncryption
        }

### <a name="contentkeydeliverytype"></a>ContentKeyDeliveryType

hello beskriver följande enum värden som du kan använda tooconfigure hello leveransmetod av hello innehåll viktiga toohello-klienten.
    
    public enum ContentKeyDeliveryType
    {
        /// <summary>
        /// None.
        ///
        </summary>
        None = 0,

        /// <summary>
        /// Use PlayReady License acquistion protocol
        ///
        </summary>
        PlayReadyLicense = 1,

        /// <summary>
        /// Use MPEG Baseline HTTP key protocol.
        ///
        </summary>
        BaselineHttp = 2,

        /// <summary>
        /// Use Widevine License acquistion protocol
        ///
        </summary>
        Widevine = 3

    }


### <a name="assetdeliverypolicyconfigurationkey"></a>AssetDeliveryPolicyConfigurationKey

hello följande enum beskrivs värden som du kan ange tooconfigure nycklar som används tooget specifik konfiguration för en tillgångsleveransprincip.

    public enum AssetDeliveryPolicyConfigurationKey
    {
        /// <summary>
        /// No policies.
        /// </summary>
        None,

        /// <summary>
        /// Exact Envelope key URL.
        /// </summary>
        EnvelopeKeyAcquisitionUrl,

        /// <summary>
        /// Base key url that will have KID=<Guid> appended for Envelope.
        /// </summary>
        EnvelopeBaseKeyAcquisitionUrl,

        /// <summary>
        /// hello initialization vector toouse for envelope encryption in Base64 format.
        /// </summary>
        EnvelopeEncryptionIVAsBase64,

        /// <summary>
        /// hello PlayReady License Acquisition Url toouse for common encryption.
        /// </summary>
        PlayReadyLicenseAcquisitionUrl,

        /// <summary>
        /// hello PlayReady Custom Attributes tooadd toohello PlayReady Content Header
        /// </summary>
        PlayReadyCustomAttributes,

        /// <summary>
        /// hello initialization vector toouse for envelope encryption.
        /// </summary>
        EnvelopeEncryptionIV,

        /// <summary>
        /// Widevine DRM acquisition url
        /// </summary>
        WidevineLicenseAcquisitionUrl
    }

## <a name="media-services-learning-paths"></a>Sökvägar för Media Services-utbildning
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Ge feedback
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

