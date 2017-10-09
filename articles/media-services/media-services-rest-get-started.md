---
title: "aaaGet igång med att leverera innehåll på begäran med hjälp av REST | Microsoft Docs"
description: "Den här självstudiekursen vägleder dig genom hello stegen för att implementera ett program för leverans av på begäran med Azure Media Services med hjälp av REST API."
services: media-services
documentationcenter: 
author: Juliako
manager: cfowler
editor: 
ms.assetid: 88194b59-e479-43ac-b179-af4f295e3780
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/10/2017
ms.author: juliako
ms.openlocfilehash: f270ed59e9ae9745e8403ec6e19d5c3533fc82b7
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-delivering-content-on-demand-using-rest"></a>Komma igång med att leverera innehåll på begäran med hjälp av REST
[!INCLUDE [media-services-selector-get-started](../../includes/media-services-selector-get-started.md)]

Denna Snabbstart vägleder dig genom hello stegen för implementera ett program för leverans av Video-on-Demand (VoD) med Azure Media Services (AMS) REST API: er.

hello självstudierna innehåller hello grundläggande Media Services-arbetsflödet och hello vanligaste programmeringsobjekt och uppgifter som krävs för Media Services-utveckling. På hello kursen hello är klar kan ska du kunna toostream eller progressivt hämta en exempelmediefil som överförs, kodat och hämtat.

hello följande bild visar några av de vanligaste hello objekt när du utvecklar program VoD mot hello Media Services OData-modellen.

Klicka på hello avbildningen tooview den full storlek.  

<a href="./media/media-services-rest-get-started/media-services-overview-object-model.png" target="_blank"><img src="./media/media-services-rest-get-started/media-services-overview-object-model-small.png"></a> 

## <a name="prerequisites"></a>Krav
hello är följande krav nödvändiga toostart utveckling med Media Services med REST API: er.

* Ett Azure-konto. Mer information om den [kostnadsfria utvärderingsversionen av Azure](https://azure.microsoft.com/pricing/free-trial/).
* Ett Media Services-konto. toocreate Media Services-konto finns [hur tooCreate Media Services-konto](media-services-portal-create-account.md).
* Förståelse av hur toodevelop med Media Services REST API. Mer information finns i [Media Services REST API-översikt](media-services-rest-how-to-use.md).
* Ett program som kan skicka HTTP-begäranden och -svar. Den här kursen använder [Fiddler](http://www.telerik.com/download/fiddler).

hello efter aktiviteter som visas i den här snabbstarten.

1. Starta strömmande slutpunkten (med hello Azure-portalen).
2. Ansluta toohello Media Services-konto med REST API.
3. Skapa en ny tillgång och överföra en videofil med REST API.
4. Koda hello källfilen till en uppsättning filer med anpassningsbar bithastighet MP4 med REST API.
5. Publicera hello tillgången och get strömning och progressiv nedladdning URL: er med REST API.
6. Spela upp ditt innehåll.

>[!NOTE]
>Det finns en gräns på 1 000 000 principer för olika AMS-principer (till exempel för positionerarprincipen eller ContentKeyAuthorizationPolicy). Du bör använda hello samma princip-ID om du alltid använder hello samma dagar / åtkomstbehörigheter, till exempel principer för lokaliserare som är avsedda tooremain på plats för lång tid (icke-överföringen principer). Mer information finns i [detta](media-services-dotnet-manage-entities.md#limit-access-policies) avsnitt.

Mer information om AMS REST-entiteter som används i det här avsnittet finns [Azure Media Services REST API-referens](/rest/api/media/services/azure-media-services-rest-api-reference). Se även [Azure Media Services-koncepten](media-services-concepts.md).

>[!NOTE]
>Vid åtkomst till entiteter i Media Services måste du ange specifika namn på huvudfält och värden i HTTP-begäranden. Mer information finns i [installationsprogrammet för Media Services REST API-utveckling](media-services-rest-how-to-use.md).

## <a name="start-streaming-endpoints-using-hello-azure-portal"></a>Starta strömningsslutpunkter med hello Azure-portalen

När du arbetar med Azure Media Services är en av hello vanligaste scenarierna att leverera video via strömning med anpassningsbar bithastighet. Media Services tillhandahåller en dynamisk paketering som gör att du toodeliver dina MP4-kodade innehåll i strömningsformat som stöds av Media Services (MPEG DASH, HLS, Smooth Streaming) just-in-time, utan att behöva toostore tillsammans med anpassad bithastighet versioner av var och en av dessa strömningsformat.

>[!NOTE]
>När AMS-kontot skapas en **standard** strömningsslutpunkt har lagts till tooyour konto i hello **stoppad** tillstånd. toostart strömning ditt innehåll och dra nytta av dynamisk paketering och dynamisk kryptering hello strömningsslutpunkt som du vill toostream innehåll har toobe i hello **kör** tillstånd.

toostart Hej strömmande slutpunkten, hello följande:

1. Logga in på hello [Azure-portalen](https://portal.azure.com/).
2. Streaming slutpunkter på hello inställningar i fönstret.
3. Klicka på hello standard strömmande slutpunkten.

    hello visas standard information om den STRÖMNINGSSLUTPUNKT.

4. Klicka på hello Start-ikonen.
5. Klicka på hello spara knappen toosave ändringarna.

## <a id="connect"></a>Anslut toohello Media Services-konto med REST API

Mer information om hur tooconnect toohello AMS API, se [åtkomst hello Azure Media Services API med Azure AD authentication](media-services-use-aad-auth-to-access-ams-api.md). 

>[!NOTE]
>När du har anslutit toohttps://media.windows.net, får du en 301 omdirigering att ange en annan Media Services-URI. Du måste göra följande anrop toohello ny URI.

Till exempel om när du har försökt tooconnect, du har fått hello följande:

    HTTP/1.1 301 Moved Permanently
    Location: https://wamsbayclus001rest-hs.cloudapp.net/api/

Du bör publicera dina efterföljande API-anrop toohttps://wamsbayclus001rest-hs.cloudapp.net/api/.

## <a id="upload"></a>Skapa en ny tillgång och överföra en videofil med REST API

I Media Services överför du dina digitala filer till en tillgång. Hej **tillgången** entitet kan innehålla video, ljud, bilder, miniatyrsamlingar, text spår och textning filer (och hello metadata om dessa filer.)  När hello filerna har överförts till hello tillgång lagras innehållet på ett säkert sätt i hello molnet för ytterligare bearbetning och strömning.

En av hello värden att du har tooprovide när du skapar en tillgång är alternativ för att skapa tillgången. Hej **alternativ** egenskapen är ett uppräkningsvärde som beskriver hello krypteringsalternativ som du kan skapa en tillgång med. Ett giltigt värde är ett av hello värden hello listan nedan, inte en kombination av värden från listan:

* **Ingen** = **0** – Ingen kryptering används. När du använder det här alternativet skyddas inte innehållet under överföring eller i vila i lagringsutrymmet.
    Använd det här alternativet om du planerar toodeliver en MP4 med progressivt nedladdning.
* **StorageEncrypted** = **1** – krypterar innehållet lokalt med hjälp av AES 256 bitarskryptering och sedan överför tooAzure lagring där den lagras krypterat i vila. Tillgångar som skyddas med Lagringskryptering okrypterad och placeras i en krypterad fil system tidigare tooencoding och eventuellt omkrypterade tidigare toouploading tillbaka som en ny utdatatillgång automatiskt. hello primära användningsfall för Lagringskryptering är om du vill toosecure hög kvalitet inkommande mediefilerna med stark kryptering i vila på disk.
* **CommonEncryptionProtected** = **2** – Använd det här alternativet om du överför innehåll som redan har krypterats och skyddats med vanlig kryptering eller PlayReady DRM (till exempel Smooth Streaming som skyddas med PlayReady DRM).
* **EnvelopeEncryptionProtected** = **4** – Använd det här alternativet om du överför HLS som krypterats med AES. hello-filer måste ha kodats och krypterats av Transform Manager.

### <a name="create-an-asset"></a>Skapa en tillgång
En tillgång är en behållare för flera typer eller uppsättningar med objekt i Media Services, inklusive video, ljud, bilder, miniatyrsamlingar, textspår och filer med dold textning. I hello REST-API: skapa en tillgång måste du skicka POST begära tooMedia tjänster och placerar egenskapsinformation om din tillgång i hello frågans brödtext.

följande exempel visar hur hello toocreate en tillgång.

**HTTP-begäran**

    POST https://wamsbayclus001rest-hs.cloudapp.net/api/Assets HTTP/1.1
    Content-Type: application/json
    DataServiceVersion: 1.0;NetFx
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json
    Accept-Charset: UTF-8
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=amstestaccount001&urn%3aSubscriptionId=z7f09258-2233-4ca2-b1ae-193798e2c9d8&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1421640053&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=vlG%2fPYdFDMS1zKc36qcFVWnaNh07UCkhYj3B71%2fk1YA%3d
    x-ms-version: 2.11
    x-ms-client-request-id: c59de965-bc89-4295-9a57-75d897e5221e
    Host: wamsbayclus001rest-hs.cloudapp.net
    Content-Length: 45

    {"Name":"BigBuckBunny.mp4", "Options":"0"}


**HTTP-svar**

Om det lyckas, returneras hello följande:

    HTTP/1.1 201 Created
    Cache-Control: no-cache
    Content-Length: 452
    Content-Type: application/json;odata=minimalmetadata;streaming=true;charset=utf-8
    Location: https://wamsbayclus001rest-hs.cloudapp.net/api/Assets('nb%3Acid%3AUUID%3A9bc8ff20-24fb-4fdb-9d7c-b04c7ee573a1')
    Server: Microsoft-IIS/8.5
    x-ms-client-request-id: c59de965-bc89-4295-9a57-75d897e5221e
    request-id: e98be122-ae09-473a-8072-0ccd234a0657
    x-ms-request-id: e98be122-ae09-473a-8072-0ccd234a0657
    X-Content-Type-Options: nosniff
    DataServiceVersion: 3.0;
    X-Powered-By: ASP.NET
    Strict-Transport-Security: max-age=31536000; includeSubDomains
    Date: Sun, 18 Jan 2015 22:06:40 GMT

    {  
       "odata.metadata":"https://wamsbayclus001rest-hs.cloudapp.net/api/$metadata#Assets/@Element",
       "Id":"nb:cid:UUID:9bc8ff20-24fb-4fdb-9d7c-b04c7ee573a1",
       "State":0,
       "Created":"2015-01-18T22:06:40.6010903Z",
       "LastModified":"2015-01-18T22:06:40.6010903Z",
       "AlternateId":null,
       "Name":"BigBuckBunny2.mp4",
       "Options":0,
       "Uri":"https://storagetestaccount001.blob.core.windows.net/asset-9bc8ff20-24fb-4fdb-9d7c-b04c7ee573a1",
       "StorageAccountName":"storagetestaccount001"
    }

### <a name="create-an-assetfile"></a>Skapa en AssetFile
Hej [AssetFile](https://docs.microsoft.com/rest/api/media/operations/assetfile) entiteten representerar en video eller ljud-fil som lagras i en blob-behållare. En resursfil är alltid associerad med en tillgång och en tillgång kan innehålla en eller flera AssetFiles. hello misslyckas Media Services-kodaren om objekttypen tillgången filen inte är associerad med en digital fil i en blob-behållaren.

När du har överfört din digitala media-fil till en blobbbehållare du använder hello **sammanfoga** HTTP-begäran tooupdate hello AssetFile med information om media-fil (som visas i hello kapitel).

**HTTP-begäran**

    POST https://wamsbayclus001rest-hs.cloudapp.net/api/Files HTTP/1.1
    Content-Type: application/json
    DataServiceVersion: 1.0;NetFx
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json
    Accept-Charset: UTF-8
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=amstestaccount001&urn%3aSubscriptionId=z7f09258-2233-4ca2-b1ae-193798e2c9d8&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1421640053&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=vlG%2fPYdFDMS1zKc36qcFVWnaNh07UCkhYj3B71%2fk1YA%3d
    x-ms-version: 2.11
    Host: wamsbayclus001rest-hs.cloudapp.net
    Content-Length: 164

    {  
       "IsEncrypted":"false",
       "IsPrimary":"false",
       "MimeType":"video/mp4",
       "Name":"BigBuckBunny.mp4",
       "ParentAssetId":"nb:cid:UUID:9bc8ff20-24fb-4fdb-9d7c-b04c7ee573a1"
    }


**HTTP-svar**

    HTTP/1.1 201 Created
    Cache-Control: no-cache
    Content-Length: 535
    Content-Type: application/json;odata=minimalmetadata;streaming=true;charset=utf-8
    Location: https://wamsbayclus001rest-hs.cloudapp.net/api/Files('nb%3Acid%3AUUID%3Af13a0137-0a62-9d4c-b3b9-ca944b5142c5')
    Server: Microsoft-IIS/8.5
    request-id: 98a30e2d-f379-4495-988e-0b79edc9b80e
    x-ms-request-id: 98a30e2d-f379-4495-988e-0b79edc9b80e
    X-Content-Type-Options: nosniff
    DataServiceVersion: 3.0;
    X-Powered-By: ASP.NET
    Strict-Transport-Security: max-age=31536000; includeSubDomains
    Date: Mon, 19 Jan 2015 00:34:07 GMT

    {  
       "odata.metadata":"https://wamsbayclus001rest-hs.cloudapp.net/api/$metadata#Files/@Element",
       "Id":"nb:cid:UUID:f13a0137-0a62-9d4c-b3b9-ca944b5142c5",
       "Name":"BigBuckBunny.mp4",
       "ContentFileSize":"0",
       "ParentAssetId":"nb:cid:UUID:9bc8ff20-24fb-4fdb-9d7c-b04c7ee573a1",
       "EncryptionVersion":null,
       "EncryptionScheme":null,
       "IsEncrypted":false,
       "EncryptionKeyId":null,
       "InitializationVector":null,
       "IsPrimary":false,
       "LastModified":"2015-01-19T00:34:08.1934137Z",
       "Created":"2015-01-19T00:34:08.1934137Z",
       "MimeType":"video/mp4",
       "ContentChecksum":null
    }


### <a name="creating-hello-accesspolicy-with-write-permission"></a>Skapa hello AccessPolicy med behörighet att skriva
Innan du laddar upp filer i blob-lagring, ange hello princip rättigheter för att skriva tooan tillgången. Ange toodo som PUBLICERAR en HTTP-begäran toohello AccessPolicies entitet. Definiera ett DurationInMinutes värde när de skapas eller felmeddelande en 500 intern Server tillbaka som svar. Mer information om AccessPolicies finns [AccessPolicy](https://docs.microsoft.com/rest/api/media/operations/accesspolicy).

följande exempel visar hur hello toocreate en AccessPolicy:

**HTTP-begäran**

    POST https://wamsbayclus001rest-hs.cloudapp.net/api/AccessPolicies HTTP/1.1
    Content-Type: application/json
    DataServiceVersion: 1.0;NetFx
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json
    Accept-Charset: UTF-8
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=amstestaccount001&urn%3aSubscriptionId=z7f09258-2233-4ca2-b1ae-193798e2c9d8&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1421640053&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=vlG%2fPYdFDMS1zKc36qcFVWnaNh07UCkhYj3B71%2fk1YA%3d
    x-ms-version: 2.11
    Host: wamsbayclus001rest-hs.cloudapp.net
    Content-Length: 74

    {"Name":"NewUploadPolicy", "DurationInMinutes":"440", "Permissions":"2"}

**HTTP-svar**

Om det lyckas, returneras hello efter svar:

    HTTP/1.1 201 Created
    Cache-Control: no-cache
    Content-Length: 312
    Content-Type: application/json;odata=minimalmetadata;streaming=true;charset=utf-8
    Location: https://wamsbayclus001rest-hs.cloudapp.net/api/AccessPolicies('nb%3Apid%3AUUID%3Abe0ac48d-af7d-4877-9d60-1805d68bffae')
    Server: Microsoft-IIS/8.5
    request-id: 74c74545-7e0a-4cd6-a440-c1c48074a970
    x-ms-request-id: 74c74545-7e0a-4cd6-a440-c1c48074a970
    X-Content-Type-Options: nosniff
    DataServiceVersion: 3.0;
    X-Powered-By: ASP.NET
    Strict-Transport-Security: max-age=31536000; includeSubDomains
    Date: Sun, 18 Jan 2015 22:18:06 GMT

    {  
       "odata.metadata":"https://wamsbayclus001rest-hs.cloudapp.net/api/$metadata#AccessPolicies/@Element",
       "Id":"nb:pid:UUID:be0ac48d-af7d-4877-9d60-1805d68bffae",
       "Created":"2015-01-18T22:18:06.6370575Z",
       "LastModified":"2015-01-18T22:18:06.6370575Z",
       "Name":"NewUploadPolicy",
       "DurationInMinutes":440.0,
       "Permissions":2
    }

### <a name="get-hello-upload-url"></a>Hämta hello URL för överföring

tooreceive hello faktiska överför URL, skapa en SAS-lokaliserare. Lokaliserare definiera hello starttid och typ av anslutningens slutpunkt för klienter som vill tooaccess filer i en tillgång. Du kan skapa flera lokaliserare entiteter för en viss AccessPolicy och tillgångshantering par toohandle olika klient begäranden och behov. Var och en av dessa lokaliserare använder hello StartTime-värdet plus hello DurationInMinutes värdet av hello AccessPolicy toodetermine hello tid kan användas för en URL. Mer information finns i [lokaliserare](https://docs.microsoft.com/rest/api/media/operations/locator).

En SAS-URL har hello följande format:

    {https://myaccount.blob.core.windows.net}/{asset name}/{video file name}?{SAS signature}

Vissa förutsättningar gäller:

* Du kan inte ha fler än fem unika lokaliserare som är associerade med en viss resurs i taget. Mer information finns i lokaliserare.
* Om du behöver tooupload dina filer direkt, bör du ange StartTime värdet toofive minuter innan hello aktuell tid. Det beror på att det kan finnas klockan skeva mellan klientdatorn och Media Services. StartTime-värdet måste också vara i hello följande DateTime-format: ÅÅÅÅ-MM-ddTHH (till exempel ”2014-05-23T17:53:50Z”).    
* Det kan finnas en 30-40 andra fördröjning efter en positionerare skapas toowhen som den är tillgänglig för användning. Det här problemet gäller tooboth SAS-URL och ursprung lokaliserare.

Mer information om SAS finns lokaliserare [detta](http://southworks.com/blog/2015/05/27/reusing-azure-media-services-locators-to-avoid-facing-the-5-shared-access-policy-limitation/) blogg.

hello som följande exempel visar hur toocreate en SAS-URL-lokaliserare, som definieras av hello typegenskapen i begärandetexten hello (”1” för en SAS-lokaliserare) och ”2” för en positionerare för ursprung på begäran. Hej **sökväg** egenskap som returneras innehåller hello-URL att du måste använda tooupload din fil.

**HTTP-begäran**

    POST https://wamsbayclus001rest-hs.cloudapp.net/api/Locators HTTP/1.1
    Content-Type: application/json
    DataServiceVersion: 1.0;NetFx
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json
    Accept-Charset: UTF-8
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=amstestaccount001&urn%3aSubscriptionId=f7f09258-6753-4ca2-b1ae-193798e2c9d8&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1421640053&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=vlG%2fPYdFDMS1zKc36qcFVWnaNh07UCkhYj3B71%2fk1YA%3d
    x-ms-version: 2.11
    Host: wamsbayclus001rest-hs.cloudapp.net
    Content-Length: 178

    {  
       "AccessPolicyId":"nb:pid:UUID:be0ac48d-af7d-4877-9d60-1805d68bffae",
       "AssetId":"nb:cid:UUID:9bc8ff20-24fb-4fdb-9d7c-b04c7ee573a1",
       "StartTime":"2015-02-18T16:45:53",
       "Type":1
    }


**HTTP-svar**

Om det lyckas, returneras hello efter svar:

    HTTP/1.1 201 Created
    Cache-Control: no-cache
    Content-Length: 949
    Content-Type: application/json;odata=minimalmetadata;streaming=true;charset=utf-8
    Location: https://wamsbayclus001rest-hs.cloudapp.net/api/Locators('nb%3Alid%3AUUID%3Aaf57bdd8-6751-4e84-b403-f3c140444b54')
    Server: Microsoft-IIS/8.5
    request-id: 2adeb1f8-89c5-4cc8-aa4f-08cdfef33ae0
    x-ms-request-id: 2adeb1f8-89c5-4cc8-aa4f-08cdfef33ae0
    X-Content-Type-Options: nosniff
    DataServiceVersion: 3.0;
    X-Powered-By: ASP.NET
    Strict-Transport-Security: max-age=31536000; includeSubDomains
    Date: Mon, 19 Jan 2015 03:01:29 GMT

    {  
       "odata.metadata":"https://wamsbayclus001rest-hs.cloudapp.net/api/$metadata#Locators/@Element",
       "Id":"nb:lid:UUID:af57bdd8-6751-4e84-b403-f3c140444b54",
       "ExpirationDateTime":"2015-02-19T00:05:53",
       "Type":1,
       "Path":"https://storagetestaccount001.blob.core.windows.net/asset-f438649c-313c-46e2-8d68-7d2550288247?sv=2012-02-12&sr=c&si=af57bdd8-6751-4e84-b403-f3c140444b54&sig=fE4btwEfZtVQFeC0Wh3Kwks2OFPQfzl5qTMW5YytiuY%3D&st=2015-02-18T16%3A45%3A53Z&se=2015-02-19T00%3A05%3A53Z",
       "BaseUri":"https://storagetestaccount001.blob.core.windows.net/asset-f438649c-313c-46e2-8d68-7d2550288247",
       "ContentAccessComponent":"?sv=2012-02-12&sr=c&si=af57bdd8-6751-4e84-b403-f3c140444b54&sig=fE4btwEfZtVQFeC0Wh3Kwks2OFPQfzl5qTMW5YytiuY%3D&st=2015-02-18T16%3A45%3A53Z&se=2015-02-19T00%3A05%3A53Z",
       "AccessPolicyId":"nb:pid:UUID:be0ac48d-af7d-4877-9d60-1805d68bffae",
       "AssetId":"nb:cid:UUID:9bc8ff20-24fb-4fdb-9d7c-b04c7ee573a1",
       "StartTime":"2015-02-18T16:45:53",
       "Name":null
    }

### <a name="upload-a-file-into-a-blob-storage-container"></a>Ladda upp en fil till en behållare för blob storage
När du har hello AccessPolicy och lokaliserare set är hello faktiska filen överförda tooan Azure blob storage-behållare med hello Azure Storage REST API: er. Du måste överföra hello filer som blockblobar. Azure Media Services stöder inte sidblobar.  

> [!NOTE]
> Du måste lägga till hello filnamn hello filen du vill ha tooupload toohello lokaliserare **sökväg** värde togs emot i hello föregående avsnitt. Till exempel https://storagetestaccount001.blob.core.windows.net/asset-e7b02da4-5a69-40e7-a8db-e8f4f697aac0/BigBuckBunny.mp4? . . .
>
>

Mer information om hur du arbetar med Azure storage blobs finns [REST-API för Blob-tjänsten](https://docs.microsoft.com/rest/api/storageservices/Blob-Service-REST-API).

### <a name="update-hello-assetfile"></a>Uppdatera hello AssetFile
Nu när du har överfört din fil uppdateringsinformation hello FileAsset storlek (och andra). Exempel:

    MERGE https://wamsbayclus001rest-hs.cloudapp.net/api/Files('nb%3Acid%3AUUID%3Af13a0137-0a62-9d4c-b3b9-ca944b5142c5') HTTP/1.1
    Content-Type: application/json
    DataServiceVersion: 1.0;NetFx
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json
    Accept-Charset: UTF-8
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=amstestaccount001&urn%3aSubscriptionId=z7f09258-2233-4ca2-b1ae-193798e2c9d8&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1421662918&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=utmoXXbm9Q7j4tW1yJuMVA3egRiQy5FPygwadkmPeaY%3d
    x-ms-version: 2.11
    Host: wamsbayclus001rest-hs.cloudapp.net

    {  
       "ContentFileSize":"1186540",
       "Id":"nb:cid:UUID:f13a0137-0a62-9d4c-b3b9-ca944b5142c5",
       "MimeType":"video/mp4",
       "Name":"BigBuckBunny.mp4",
       "ParentAssetId":"nb:cid:UUID:9bc8ff20-24fb-4fdb-9d7c-b04c7ee573a1"
    }


**HTTP-svar**

Om det lyckas, returneras hello följande:

    HTTP/1.1 204 No Content
    ...

## <a name="delete-hello-locator-and-accesspolicy"></a>Ta bort hello lokaliserare och AccessPolicy
**HTTP-begäran**

    DELETE https://wamsbayclus001rest-hs.cloudapp.net/api/Locators('nb%3Alid%3AUUID%3Aaf57bdd8-6751-4e84-b403-f3c140444b54') HTTP/1.1
    DataServiceVersion: 1.0;NetFx
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json
    Accept-Charset: UTF-8
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=amstestaccount001&urn%3aSubscriptionId=z7f09258-2233-4ca2-b1ae-193798e2c9d8&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1421662918&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=utmoXXbm9Q7j4tW1yJuMVA3egRiQy5FPygwadkmPeaY%3d
    x-ms-version: 2.11
    Host: wamsbayclus001rest-hs.cloudapp.net


**HTTP-svar**

Om det lyckas, returneras hello följande:

    HTTP/1.1 204 No Content
    ...

**HTTP-begäran**

    DELETE https://wamsbayclus001rest-hs.cloudapp.net/api/AccessPolicies('nb%3Apid%3AUUID%3Abe0ac48d-af7d-4877-9d60-1805d68bffae') HTTP/1.1
    DataServiceVersion: 1.0;NetFx
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json
    Accept-Charset: UTF-8
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=amstestaccount001&urn%3aSubscriptionId=z7f09258-2233-4ca2-b1ae-193798e2c9d8&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1421662918&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=utmoXXbm9Q7j4tW1yJuMVA3egRiQy5FPygwadkmPeaY%3d
    x-ms-version: 2.11
    Host: wamsbayclus001rest-hs.cloudapp.net

**HTTP-svar**

Om det lyckas, returneras hello följande:

    HTTP/1.1 204 No Content
    ...

## <a id="encode"></a>Koda hello källfilen till en uppsättning MP4-filer med anpassningsbar bithastighet

När du vill föra in tillgångar i Media Services kan media kan vara kodad, transmuxed, gräns och så vidare innan de skickas tooclients. Dessa aktiviteter schemaläggs och körs mot flera bakgrund rollen instanser tooensure hög prestanda och tillgänglighet. Aktiviteterna kallas jobb och varje jobb består av atomiska uppgifter som hello faktiska arbetet i tillgångsfilen hello (Mer information finns i [jobbet](/rest/api/media/services/job), [aktivitet](/rest/api/media/services/task) beskrivningar).

Som tidigare nämnts, när arbeta med Azure Media Services en av de vanligaste scenarierna för hello levererar strömning tooyour klienter med anpassningsbar bithastighet. Media Services kan dynamiskt Paketera en uppsättning MP4-filer med anpassningsbar bithastighet till en hello följande format: HTTP Live Streaming (HLS), Smooth Streaming, MPEG DASH.

hello efter avsnittet visar hur toocreate ett jobb som innehåller en kodning uppgift. hello aktivitet anger tootranscode hello mezzaninfil till en uppsättning med anpassningsbar bithastighet MP4s **Media Encoder Standard**. hello avsnitt visar även hur toomonitor hello jobbförloppet för bearbetning. Du kommer att kan toocreate lokaliserare som är nödvändiga tooget åtkomst tooyour tillgångar när hello jobbet är klart.

### <a name="get-a-media-processor"></a>Hämta en medieprocessor
I Media Services är en medieprocessor en komponent som hanterar en specifik bearbetning aktivitet, till exempel kodning, kryptering eller dekryptering medieinnehåll-Formatkonvertering. För hello kodning aktivitet som visas i den här självstudiekursen, vi toouse hello Media Encoder Standard.

hello följande kod begäranden hello kodare id.

**HTTP-begäran**

    GET https://wamsbayclus001rest-hs.cloudapp.net/api/MediaProcessors()?$filter=Name%20eq%20'Media%20Encoder%20Standard' HTTP/1.1
    DataServiceVersion: 1.0;NetFx
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json
    Accept-Charset: UTF-8
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=amstestaccount001&urn%3aSubscriptionId=f7f09258-6753-4ca2-b1ae-193798e2c9d8&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1421675491&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=9hUudHYnATpi5hN3cvTfgw%2bL4N3tL0fdsRnQnm6ZYIU%3d
    x-ms-version: 2.11
    Host: wamsbayclus001rest-hs.cloudapp.net


**HTTP-svar**

    HTTP/1.1 200 OK
    Cache-Control: no-cache
    Content-Length: 273
    Content-Type: application/json;odata=minimalmetadata;streaming=true;charset=utf-8
    Server: Microsoft-IIS/8.5
    request-id: 6beb04b4-55a7-480d-8aa8-e5c5d59ffa1f
    x-ms-request-id: 6beb04b4-55a7-480d-8aa8-e5c5d59ffa1f
    X-Content-Type-Options: nosniff
    DataServiceVersion: 3.0;
    X-Powered-By: ASP.NET
    Strict-Transport-Security: max-age=31536000; includeSubDomains
    Date: Mon, 19 Jan 2015 07:54:09 GMT

    {  
       "odata.metadata":"https://wamsbayclus001rest-hs.cloudapp.net/api/$metadata#MediaProcessors",
       "value":[  
          {  
             "Id":"nb:mpid:UUID:ff4df607-d419-42f0-bc17-a481b1331e56",
             "Description":"Media Encoder Standard",
             "Name":"Media Encoder Standard",
             "Sku":"",
             "Vendor":"Microsoft",
             "Version":"1.1"
          }
       ]
    }

### <a name="create-a-job"></a>Skapa ett jobb
Varje jobb kan ha en eller flera aktiviteter beroende på hello typ av bearbetning som du vill tooaccomplish. Via hello REST-API, du kan skapa jobb och deras relaterade uppgifter i ett av två sätt: aktiviteter kan vara definierade infogad via hello uppgifter navigeringsegenskap på jobbet entiteter eller OData-batch-bearbetning. hello Media Services SDK använder batch-bearbetning. Men för hello läsbarhet hello kodexempel i det här avsnittet, uppgifter som är definierade infogad. Mer information om batchbearbetning finns [Open Data Protocol (OData) gruppbearbetning](http://www.odata.org/documentation/odata-version-3-0/batch-processing/).

hello som följande exempel visar hur toocreate och efter ett jobb med en uppgift ange tooencode en video på en viss upplösning och kvalitet. hello följande dokumentationsavsnitt innehåller hello lista över alla hello [uppgift förinställningar](http://msdn.microsoft.com/library/mt269960) stöds av Media Encoder Standard hello-processor.  

**HTTP-begäran**

    POST https://wamsbayclus001rest-hs.cloudapp.net/api/Jobs HTTP/1.1
    DataServiceVersion: 1.0;NetFx
    MaxDataServiceVersion: 3.0;NetFx
    Content-Type: application/json
    Accept: application/json;odata=verbose
    Accept-Charset: UTF-8
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=amstestaccount001&urn%3aSubscriptionId=z7f09258-2233-4ca2-b1ae-193798e2c9d8&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1421675491&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=9hUudHYnATpi5hN3cvTfgw%2bL4N3tL0fdsRnQnm6ZYIU%3d
    x-ms-version: 2.11
    Host: wamsbayclus001rest-hs.cloudapp.net
    Content-Length: 482

    {  
       "Name":"NewTestJob",
       "InputMediaAssets":[  
          {  
             "__metadata":{  
                "uri":"https://wamsbayclus001rest-hs.net/api/Assets('nb%3Acid%3AUUID%3A9bc8ff20-24fb-4fdb-9d7c-b04c7ee573a1')"
             }
          }
       ],
       "Tasks":[  
          {  
             "Configuration":"Adaptive Streaming",
             "MediaProcessorId":"nb:mpid:UUID:ff4df607-d419-42f0-bc17-a481b1331e56",
             "TaskBody":"<?xml version=\"1.0\" encoding=\"utf-8\"?><taskBody><inputAsset>JobInputAsset(0)</inputAsset>
                <outputAsset>JobOutputAsset(0)</outputAsset></taskBody>"
          }
       ]
    }

**HTTP-svar**

Om det lyckas, returneras hello efter svar:

    HTTP/1.1 201 Created
    Cache-Control: no-cache
    Content-Length: 1215
    Content-Type: application/json;odata=verbose;charset=utf-8
    Location: https://wamsbayclus001rest-hs.cloudapp.net/api/Jobs('nb%3Ajid%3AUUID%3A71d2dd33-efdf-ec43-8ea1-136a110bd42c')
    Server: Microsoft-IIS/8.5
    request-id: 532ac1ec-a475-4dce-b2d5-7c8ce94ac87c
    x-ms-request-id: 532ac1ec-a475-4dce-b2d5-7c8ce94ac87c
    X-Content-Type-Options: nosniff
    DataServiceVersion: 3.0;
    X-Powered-By: ASP.NET
    Strict-Transport-Security: max-age=31536000; includeSubDomains
    Date: Mon, 19 Jan 2015 08:04:35 GMT

    {  
       "d":{  
          "__metadata":{  
             "id":"https://wamsbayclus001rest-hs.cloudapp.net/api/Jobs('nb%3Ajid%3AUUID%3A71d2dd33-efdf-ec43-8ea1-136a110bd42c')",
             "uri":"https://wamsbayclus001rest-hs.cloudapp.net/api/Jobs('nb%3Ajid%3AUUID%3A71d2dd33-efdf-ec43-8ea1-136a110bd42c')",
             "type":"Microsoft.Cloud.Media.Vod.Rest.Data.Models.Job"
          },
          "Tasks":{  
             "__deferred":{  
                "uri":"https://wamsbayclus001rest-hs.cloudapp.net/api/Jobs('nb%3Ajid%3AUUID%3A71d2dd33-efdf-ec43-8ea1-136a110bd42c')/Tasks"
             }
          },
          "OutputMediaAssets":{  
             "__deferred":{  
                "uri":"https://wamsbayclus001rest-hs.cloudapp.net/api/Jobs('nb%3Ajid%3AUUID%3A71d2dd33-efdf-ec43-8ea1-136a110bd42c')/OutputMediaAssets"
             }
          },
          "InputMediaAssets":{  
             "__deferred":{  
                "uri":"https://wamsbayclus001rest-hs.cloudapp.net/api/Jobs('nb%3Ajid%3AUUID%3A9bc8ff20-24fb-4fdb-9d7c-b04c7ee573a1')/InputMediaAssets"
             }
          },
          "Id":"nb:jid:UUID:71d2dd33-efdf-ec43-8ea1-136a110bd42c",
          "Name":"NewTestJob",
          "Created":"2015-01-19T08:04:34.3287057Z",
          "LastModified":"2015-01-19T08:04:34.3287057Z",
          "EndTime":null,
          "Priority":0,
          "RunningDuration":0,
          "StartTime":null,
          "State":0,
          "TemplateId":null,
          "JobNotificationSubscriptions":{  
             "__metadata":{  
                "type":"Collection(Microsoft.Cloud.Media.Vod.Rest.Data.Models.JobNotificationSubscription)"
             },
             "results":[  

             ]
          }
       }
    }


Det finns några viktiga saker toonote varje jobb begäran:

* TaskBody egenskaper måste använda literal XML toodefine hello antal indata eller utdata tillgångar som används av hello aktivitet. hello avsnittet innehåller hello XML Schema Definition för hello XML.
* I hello TaskBody definition, varje inre värde för <inputAsset> och <outputAsset> måste anges som JobInputAsset(value) eller JobOutputAsset(value).
* En aktivitet kan innehålla flera utdata tillgångar. En JobOutputAsset(x) kan bara användas en gång som utdata för en aktivitet i ett jobb.
* Du kan ange JobInputAsset eller JobOutputAsset som inkommande tillgång för en aktivitet.
* Aktiviteter måste inte utgör en cykel.
* Parametern för hello-värde som du skickar tooJobInputAsset eller JobOutputAsset representerar hello indexvärdet för en tillgång. hello har faktiska tillgångar definierats i hello InputMediaAssets och OutputMediaAssets navigeringsegenskaper på hello jobbdefinitionen för entiteten.

> [!NOTE]
> Eftersom Media Services bygger på OData v3 hello enskilda tillgångar i InputMediaAssets och OutputMediaAssets navigering egenskapsuppsättningar refereras via en ”__metadata: uri” namn / värde-par.
>
>

* InputMediaAssets mappar tooone eller flera resurser som du har skapat i Media Services. OutputMediaAssets skapas med hello system. De hänvisar inte till en befintlig tillgång.
* OutputMediaAssets kan namnges med hello assetName attribut. Om det här attributet är inte tillgängligt och sedan hello hello OutputMediaAsset heter det inre textvärdet hello hello <outputAsset> elementet är med suffixet hello jobbnamn värde eller hello jobb-Id-värde (i hello fall där hello namnegenskapen har inte definierats). Till exempel om du anger ett värde för assetName för ”Sample”, och sedan hello OutputMediaAsset namnegenskapen skulle anges för ”Sample”. Men om du inte har angett ett värde för assetName men ställts in hello Jobbnamn är för ”NewJob” och sedan hello OutputMediaAsset namn ”_NewJob JobOutputAsset (värde)”.

    hello som följande exempel visar hur tooset hello assetName attribut:

        "<?xml version=\"1.0\" encoding=\"utf-8\"?><taskBody><inputAsset>JobInputAsset(0)</inputAsset><outputAsset assetName=\"CustomOutputAssetName\">JobOutputAsset(0)</outputAsset></taskBody>"
* tooenable länkning för uppgiften:

  * Ett jobb måste ha minst två aktiviteter
  * Det måste finnas minst en aktivitet vars indata är resultatet av en annan aktivitet i hello-jobbet.

Mer information finns i [skapar en kodning jobb med hello Media Services REST API](media-services-rest-encode-asset.md).

### <a name="monitor-processing-progress"></a>Övervakaren bearbetning pågår
Du kan hämta hello jobbstatus med hello tillstånd egenskapen som visas i följande exempel hello.

**HTTP-begäran**

    GET https://wamsbayclus001rest-hs.net/api/Jobs('nb%3Ajid%3AUUID%3A71d2dd33-efdf-ec43-8ea1-136a110bd42c')/State HTTP/1.1
    Content-Type: application/json;odata=verbose
    Accept: application/json;odata=verbose
    DataServiceVersion: 3.0
    MaxDataServiceVersion: 3.0
    x-ms-version: 2.11
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=youraccountname&urn%3aSubscriptionId=zf84471d-2233-4e75-aa09-010f0fc0cf5b&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1336908022&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=RYXOraO6Z%2f7l9whWZQN%2bypeijgHwIk8XyikA01Kx1%2bk%3d
    Host: wamsbayclus001rest-hs.net
    Content-Length: 0


**HTTP-svar**

Om det lyckas, returneras hello efter svar:

    HTTP/1.1 200 OK
    Cache-Control: no-cache
    Content-Length: 17
    Content-Type: application/json;odata=verbose;charset=utf-8
    Server: Microsoft-IIS/7.5
    x-ms-request-id: 01324d81-18e2-4493-a3e5-c6186209f0c8
    X-Content-Type-Options: nosniff
    DataServiceVersion: 1.0;
    X-AspNet-Version: 4.0.30319
    X-Powered-By: ASP.NET
    Date: Sun, 13 May 2012 05:16:53 GMT

    {"d":{"State":2}}


### <a name="cancel-a-job"></a>Avbryta ett jobb
Media Services kan du toocancel jobb som körs via hello CancelJob funktion. Det här anropet returnerar 400 felkoden om du försöker toocancel ett jobb när dess tillstånd har avbrutits, avbryta, fel eller slutförts.

följande exempel visar hur hello toocall CancelJob.

**HTTP-begäran**

    GET https://wamsbayclus001rest-hs.net/API/CancelJob?jobid='nb%3ajid%3aUUID%3a71d2dd33-efdf-ec43-8ea1-136a110bd42c' HTTP/1.1
    Content-Type: application/json;odata=verbose
    Accept: application/json;odata=verbose
    DataServiceVersion: 3.0
    MaxDataServiceVersion: 3.0
    x-ms-version: 2.2
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=youraccountname&urn%3aSubscriptionId=2f84471d-b1ae-4e75-aa09-010f0fc0cf5b&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1336908753&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=kolAgnFfbQIeRv4lWxKSFa4USyjWXRm15Kd%2bNd5g8eA%3d
    Host: wamsbayclus001rest-hs.net


Om det lyckas, returneras en 204-svarskod med ingen brödtext.

> [!NOTE]
> Du måste URL-koda hello jobb-id (normalt nb:jid:UUID: somevalue) när du skickar det som en parameter tooCancelJob.
>
>

### <a name="get-hello-output-asset"></a>Hämta hello utdatatillgången
hello följande kod visar hur toorequest hello utdata tillgången Id.

**HTTP-begäran**

    GET https://wamsbayclus001rest-hs.cloudapp.net/api/Jobs('nb%3Ajid%3AUUID%3A71d2dd33-efdf-ec43-8ea1-136a110bd42c')/OutputMediaAssets() HTTP/1.1
    DataServiceVersion: 1.0;NetFx
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json
    Accept-Charset: UTF-8
    User-Agent: Microsoft ADO.NET Data Services
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=amstestaccount001&urn%3aSubscriptionId=z7f09258-2233-4ca2-b1ae-193798e2c9d8&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1421675491&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=9hUudHYnATpi5hN3cvTfgw%2bL4N3tL0fdsRnQnm6ZYIU%3d
    x-ms-version: 2.11
    Host: wamsbayclus001rest-hs.cloudapp.net


**HTTP-svar**

    HTTP/1.1 200 OK
    Cache-Control: no-cache
    Content-Length: 445
    Content-Type: application/json;odata=minimalmetadata;streaming=true;charset=utf-8
    Server: Microsoft-IIS/8.5
    request-id: 73cd605d-066a-46f1-8358-f4bd25a9220a
    x-ms-request-id: 73cd605d-066a-46f1-8358-f4bd25a9220a
    X-Content-Type-Options: nosniff
    DataServiceVersion: 3.0;
    X-Powered-By: ASP.NET
    Strict-Transport-Security: max-age=31536000; includeSubDomains
    Date: Mon, 19 Jan 2015 08:28:13 GMT

    {  
       "odata.metadata":"https://wamsbayclus001rest-hs.cloudapp.net/api/$metadata#Assets",
       "value":[  
          {  
             "Id":"nb:cid:UUID:71d2dd33-efdf-ec43-8ea1-136a110bd42c",
             "State":0,
             "Created":"2015-01-19T07:52:15.603",
             "LastModified":"2015-01-19T07:52:15.603",
             "AlternateId":null,
             "Name":"Multibitrate MP4s",
             "Options":0,
             "Uri":"https://storagetestaccount001.blob.core.windows.net/asset-71d2dd33-efdf-ec43-8ea1-136a110bd42c",
             "StorageAccountName":"storagetestaccount001"
          }
       ]
    }



## <a id="publish_get_urls"></a>Publicera hello tillgången och get strömning och progressiv nedladdning URL: er med REST API

toostream eller hämta en tillgång, du behöver för ”publicera” den genom att skapa en positionerare. Positionerare ger åtkomst toofiles i hello tillgången. Media Services stöder två typer av positionerare: OnDemandOrigin-positionerare som används toostream media (till exempel MPEG DASH, HLS eller Smooth Streaming) och signatur åtkomst (SAS)-positionerare används toodownload mediefiler. Mer information om SAS finns lokaliserare [detta](http://southworks.com/blog/2015/05/27/reusing-azure-media-services-locators-to-avoid-facing-the-5-shared-access-policy-limitation/) blogg.

När du skapar hello positionerare kan skapa du hello URL: er som används toostream eller hämta dina filer.

>[!NOTE]
>När AMS-kontot skapas en **standard** strömningsslutpunkt har lagts till tooyour konto i hello **stoppad** tillstånd. toostart strömning ditt innehåll och dra nytta av dynamisk paketering och dynamisk kryptering hello strömningsslutpunkt som du vill toostream innehåll har toobe i hello **kör** tillstånd.

En strömmande URL för Smooth Streaming har hello följande format:

    {streaming endpoint name-media services account name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest

En strömmande URL för HLS har hello följande format:

    {streaming endpoint name-media services account name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest(format=m3u8-aapl)

En strömmande URL för MPEG DASH har hello följande format:

    {streaming endpoint name-media services account name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest(format=mpd-time-csf)


En SAS-URL som används för toodownload filer har hello följande format:

    {blob container name}/{asset name}/{file name}/{SAS signature}

Det här avsnittet visar hur tooperform hello följande uppgifter krävs för ”publicera” dina tillgångar.  

* Skapa hello AccessPolicy med läsbehörighet
* Skapa en SAS-URL för hämtning av innehåll
* Skapa ett ursprung URL för direktuppspelning av innehåll

### <a name="creating-hello-accesspolicy-with-read-permission"></a>Skapa hello AccessPolicy med läsbehörighet
Innan du laddar ned eller strömmande mediainnehåll först definiera ett AccessPolicy med läsbehörighet och skapa hello lämpliga lokaliserare entitet som anger hello leveransmekanismen som du vill tooenable för klienterna. Mer information om hello-egenskaper som är tillgängliga finns [AccessPolicy Entitetsegenskaper](https://docs.microsoft.com/rest/api/media/operations/accesspolicy#accesspolicy_properties).

hello som följande exempel visar hur toospecify hello AccessPolicy för läsbehörighet för den angivna resursen.

    POST https://wamsbayclus001rest-hs.net/API/AccessPolicies HTTP/1.1
    Content-Type: application/json
    Accept: application/json
    DataServiceVersion: 3.0
    MaxDataServiceVersion: 3.0
    x-ms-version: 2.11
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=youraccountname&urn%3aSubscriptionId=2f84471d-b1ae-4e75-aa09-010f0fc0cf5b&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1337067658&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=dithjGvlXR9HlyAf5DE99N5OCYkPAxsHIcsTSjm9%2fVE%3d
    Host: wamsbayclus001rest-hs.net
    Content-Length: 74
    Expect: 100-continue

    {"Name": "DownloadPolicy", "DurationInMinutes" : "300", "Permissions" : 1}

Om det lyckas, returneras 201 koden som beskriver hello AccessPolicy entitet som du skapade. Du kan sedan använda hello AccessPolicy Id tillsammans med hello tillgångsnummer hello tillgång som innehåller hello fil toodeliver (till exempel en utdatatillgången) toocreate hello lokaliserare entitet.

> [!NOTE]
> Det här grundläggande arbetsflödet är hello samma som överför en fil när du vill föra in en tillgång (som beskrevs tidigare i det här avsnittet). Ange dessutom StartTime värdet toofive minuter innan hello klockslaget som ladda upp filer, om du (eller klienterna) behöver tooaccess filerna direkt. Den här åtgärden är nödvändigt eftersom det kan finnas klockan skeva mellan hello-klienten och Media Services. hello StartTime-värdet måste vara i hello följande DateTime-format: ÅÅÅÅ-MM-ddTHH (till exempel ”2014-05-23T17:53:50Z”).
>
>

### <a name="creating-a-sas-url-for-downloading-content"></a>Skapa en SAS-URL för hämtning av innehåll
hello följande kod visar hur tooget en URL som kan använda toodownload en mediefil skapas och överföra tidigare. Hej AccessPolicy har skrivskyddad behörighet som anges och hello lokaliserare sökvägen refererar tooa SAS nedladdnings-URL.

    POST https://wamsbayclus001rest-hs.net/API/Locators HTTP/1.1
    Content-Type: application/json
    Accept: application/json
    DataServiceVersion: 3.0
    MaxDataServiceVersion: 3.0
    x-ms-version: 2.11
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=youraccountname&urn%3aSubscriptionId=zf84471d-b1ae-2233-aa09-010f0fc0cf5b&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1337067658&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=dithjGvlXR9HlyAf5DE99N5OCYkPAxsHIcsTSjm9%2fVE%3d
    Host: wamsbayclus001rest-hs.net
    Content-Length: 182
    Expect: 100-continue

    {"AccessPolicyId": "nb:pid:UUID:38c71dd0-44c5-4c5f-8418-08bb6fbf7bf8", "AssetId" : "nb:cid:UUID:71d2dd33-efdf-ec43-8ea1-136a110bd42c", "StartTime" : "2014-05-17T16:45:53", "Type":1}

Om det lyckas, returneras hello efter svar:

    HTTP/1.1 201 Created
    Cache-Control: no-cache
    Content-Length: 1150
    Content-Type: application/json;odata=verbose;charset=utf-8
    Location: https://wamsbayclus001rest-hs.net/api/Locators('nb%3Alid%3AUUID%3A8e5a821d-2194-4d00-8884-adf979856874')
    Server: Microsoft-IIS/7.5
    x-ms-request-id: 8cfd8556-3064-416a-97f2-67d9e35f0911
    X-Content-Type-Options: nosniff
    DataServiceVersion: 1.0;
    X-AspNet-Version: 4.0.30319
    X-Powered-By: ASP.NET
    Date: Mon, 14 May 2012 21:41:32 GMT

    {  
       "d":{  
          "__metadata":{  
             "id":"https://wamsbayclus001rest-hs.net/api/Locators('nb%3Alid%3AUUID%3A8e5a821d-2194-4d00-8884-adf979856874')",
             "uri":"https://wamsbayclus001rest-hs.net/api/Locators('nb%3Alid%3AUUID%3A8e5a821d-2194-4d00-8884-adf979856874')",
             "type":"Microsoft.Cloud.Media.Vod.Rest.Data.Models.Locator"
          },
          "AccessPolicy":{  
             "__deferred":{  
                "uri":"https://wamsbayclus001rest-hs.net/api/Locators('nb%3Alid%3AUUID%3A8e5a821d-2194-4d00-8884-adf979856874')/AccessPolicy"
             }
          },
          "Asset":{  
             "__deferred":{  
                "uri":"https://wamsbayclus001rest-hs.net/api/Locators('nb%3Alid%3AUUID%3A71d2dd33-efdf-ec43-8ea1-136a110bd42c')/Asset"
             }
          },
          "Id":"nb:lid:UUID:8e5a821d-2194-4d00-8884-adf979856874",
          "ExpirationDateTime":"\/Date(1337049393000)\/",
          "Type":1,
          "Path":"https://storagetestaccount001.blob.core.windows.net/asset-71d2dd33-efdf-ec43-8ea1-136a110bd42c?st=2012-05-14T21%3A36%3A33Z&se=2012-05-15T02%3A36%3A33Z&sr=c&si=8e5a821d-2194-4d00-8884-adf979856874&sig=y75dViDpC5V8WutrXM%2B%2FGpR3uOtqmlISiNlHU1YUBOg%3D",
          "AccessPolicyId":"nb:pid:UUID:38c71dd0-44c5-4c5f-8418-08bb6fbf7bf8",
          "AssetId":"nb:cid:UUID:71d2dd33-efdf-ec43-8ea1-136a110bd42c",
          "StartTime":"\/Date(1337031393000)\/"
       }
    }


hello returnerade **sökväg** egenskap innehåller hello SAS-URL.

> [!NOTE]
> Om du hämtar lagringskrypterad innehåll, måste du manuellt dekryptera den innan du gör den eller använda hello lagring dekryptering MediaProcessor i en bearbetning uppgiften toooutput bearbetas filer i hello Rensa tooan OutputAsset och sedan ladda ned från tillgången. Mer information om bearbetningen finns i skapar en kodning jobb med hello Media Services REST API. Lokaliserare för SAS-URL: en kan inte uppdateras när de har skapats. Du kan exempelvis återanvända hello samma lokaliserare med ett uppdaterat StartTime-värde. Detta är på grund av hello sätt SAS-URL: er skapas. Om du vill tooaccess en tillgång för att ladda ned efter en positionerare har upphört att gälla, måste du skapa en ny med en ny StartTime.
>
>

### <a name="download-files"></a>Hämta filer
Du kan hämta filer med hjälp av hello Azure Storage REST API: er när du har hello AccessPolicy och lokaliserare uppsättningen.  

> [!NOTE]
> Du måste lägga till hello filnamn hello filen du vill ha toodownload toohello lokaliserare **sökväg** värde togs emot i hello föregående avsnitt. Till exempel https://storagetestaccount001.blob.core.windows.net/asset-e7b02da4-5a69-40e7-a8db-e8f4f697aac0/BigBuckBunny.mp4? . . .
>
>

Mer information om hur du arbetar med Azure storage blobs finns [REST-API för Blob-tjänsten](https://docs.microsoft.com/rest/api/storageservices/Blob-Service-REST-API).

På grund av hello kodning jobb som du utförde tidigare (kodning till anpassningsbar MP4-uppsättningen), har du flera MP4-filer som du kan hämta progressivt. Exempel:    

    https://storagetestaccount001.blob.core.windows.net/asset-38058602-a4b8-4b33-b9f0-6880dc1490ea/BigBuckBunny_H264_650kbps_AAC_und_ch2_96kbps.mp4?sv=2012-02-12&sr=c&si=166d5154-b801-410b-a226-ee2f8eac1929&sig=P2iNZJAvAWpp%2Bj9yV6TQjoz5DIIaj7ve8ARynmEM6Xk%3D&se=2015-02-14T01:13:05Z

    https://storagetestaccount001.blob.core.windows.net/asset-38058602-a4b8-4b33-b9f0-6880dc1490ea/BigBuckBunny_H264_400kbps_AAC_und_ch2_96kbps.mp4?sv=2012-02-12&sr=c&si=166d5154-b801-410b-a226-ee2f8eac1929&sig=P2iNZJAvAWpp%2Bj9yV6TQjoz5DIIaj7ve8ARynmEM6Xk%3D&se=2015-02-14T01:13:05Z

    https://storagetestaccount001.blob.core.windows.net/asset-38058602-a4b8-4b33-b9f0-6880dc1490ea/BigBuckBunny_H264_3400kbps_AAC_und_ch2_96kbps.mp4?sv=2012-02-12&sr=c&si=166d5154-b801-410b-a226-ee2f8eac1929&sig=P2iNZJAvAWpp%2Bj9yV6TQjoz5DIIaj7ve8ARynmEM6Xk%3D&se=2015-02-14T01:13:05Z

    https://storagetestaccount001.blob.core.windows.net/asset-38058602-a4b8-4b33-b9f0-6880dc1490ea/BigBuckBunny_H264_2250kbps_AAC_und_ch2_96kbps.mp4?sv=2012-02-12&sr=c&si=166d5154-b801-410b-a226-ee2f8eac1929&sig=P2iNZJAvAWpp%2Bj9yV6TQjoz5DIIaj7ve8ARynmEM6Xk%3D&se=2015-02-14T01:13:05Z

    https://storagetestaccount001.blob.core.windows.net/asset-38058602-a4b8-4b33-b9f0-6880dc1490ea/BigBuckBunny_H264_1500kbps_AAC_und_ch2_96kbps.mp4?sv=2012-02-12&sr=c&si=166d5154-b801-410b-a226-ee2f8eac1929&sig=P2iNZJAvAWpp%2Bj9yV6TQjoz5DIIaj7ve8ARynmEM6Xk%3D&se=2015-02-14T01:13:05Z

    https://storagetestaccount001.blob.core.windows.net/asset-38058602-a4b8-4b33-b9f0-6880dc1490ea/BigBuckBunny_H264_1000kbps_AAC_und_ch2_96kbps.mp4?sv=2012-02-12&sr=c&si=166d5154-b801-410b-a226-ee2f8eac1929&sig=P2iNZJAvAWpp%2Bj9yV6TQjoz5DIIaj7ve8ARynmEM6Xk%3D&se=2015-02-14T01:13:05Z

    https://storagetestaccount001.blob.core.windows.net/asset-38058602-a4b8-4b33-b9f0-6880dc1490ea/BigBuckBunny_AAC_und_ch2_96kbps.mp4?sv=2012-02-12&sr=c&si=166d5154-b801-410b-a226-ee2f8eac1929&sig=P2iNZJAvAWpp%2Bj9yV6TQjoz5DIIaj7ve8ARynmEM6Xk%3D&se=2015-02-14T01:13:05Z

    https://storagetestaccount001.blob.core.windows.net/asset-38058602-a4b8-4b33-b9f0-6880dc1490ea/BigBuckBunny_AAC_und_ch2_56kbps.mp4?sv=2012-02-12&sr=c&si=166d5154-b801-410b-a226-ee2f8eac1929&sig=P2iNZJAvAWpp%2Bj9yV6TQjoz5DIIaj7ve8ARynmEM6Xk%3D&se=2015-02-14T01:13:05Z


### <a name="creating-a-streaming-url-for-streaming-content"></a>Skapa en strömnings-URL för direktuppspelning av innehåll
Hej följande kod visar hur toocreate en Strömningslokaliserare URL:

    POST https://wamsbayclus001rest-hs/API/Locators HTTP/1.1
    Content-Type: application/json
    Accept: application/json
    DataServiceVersion: 3.0
    MaxDataServiceVersion: 3.0
    x-ms-version: 2.11
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=youraccountname&urn%3aSubscriptionId=2f84471d-b1ae-4e75-aa09-010f0fc0cf5b&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1337067658&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=dithjGvlXR9HlyAf5DE99N5OCYkPAxsHIcsTSjm9%2fVE%3d
    Host: wamsbayclus001rest-hs
    Content-Length: 182
    Expect: 100-continue

    {"AccessPolicyId": "nb:pid:UUID:38c71dd0-44c5-4c5f-8418-08bb6fbf7bf8", "AssetId" : "nb:cid:UUID:eb5540a2-116e-4d36-b084-7e9958f7f3c3", "StartTime" : "2014-05-17T16:45:53",, "Type":2}

Om det lyckas, returneras hello efter svar:

    HTTP/1.1 201 Created
    Cache-Control: no-cache
    Content-Length: 981
    Content-Type: application/json;odata=verbose;charset=utf-8
    Location: https://wamsbayclus001rest-hs.net/api/Locators('nb%3Alid%3AUUID%3A52034bf6-dfae-4d83-aad3-3bd87dcb1a5d')
    Server: Microsoft-IIS/7.5
    x-ms-request-id: 2eac4158-fc78-4fa2-81ee-c9f582dc385f
    X-Content-Type-Options: nosniff
    DataServiceVersion: 1.0;
    X-AspNet-Version: 4.0.30319
    X-Powered-By: ASP.NET
    Date: Mon, 14 May 2012 21:41:39 GMT

    {  
       "d":{  
          "__metadata":{  
             "id":"https://wamsbayclus001rest-hs.net/api/Locators('nb%3Alid%3AUUID%3A52034bf6-dfae-4d83-aad3-3bd87dcb1a5d')",
             "uri":"https://wamsbayclus001rest-hs.net/api/Locators('nb%3Alid%3AUUID%3A52034bf6-dfae-4d83-aad3-3bd87dcb1a5d')",
             "type":"Microsoft.Cloud.Media.Vod.Rest.Data.Models.Locator"
          },
          "AccessPolicy":{  
             "__deferred":{  
                "uri":"https://wamsbayclus001rest-hs.net/api/Locators('nb%3Alid%3AUUID%3A52034bf6-dfae-4d83-aad3-3bd87dcb1a5d')/AccessPolicy"
             }
          },
          "Asset":{  
             "__deferred":{  
                "uri":"https://wamsbayclus001rest-hs.net/api/Locators('nb%3Alid%3AUUID%3A52034bf6-dfae-4d83-aad3-3bd87dcb1a5d')/Asset"
             }
          },
          "Id":"nb:lid:UUID:52034bf6-dfae-4d83-aad3-3bd87dcb1a5d",
          "ExpirationDateTime":"\/Date(1337049395000)\/",
          "Type":2,
          "Path":"http://wamsbayclus001rest-hs.net/52034bf6-dfae-4d83-aad3-3bd87dcb1a5d/",
          "AccessPolicyId":"nb:pid:UUID:38c71dd0-44c5-4c5f-8418-08bb6fbf7bf8",
          "AssetId":"nb:cid:UUID:eb5540a2-116e-4d36-b084-7e9958f7f3c3",
          "StartTime":"\/Date(1337031395000)\/"
       }
    }

toostream en Smooth Streaming ursprung URL i en strömmande media player, måste du lägga till hello sökväg egenskap med namnet hello hello Smooth Streaming manifestfilen, följt av ”/ manifest”.

    http://amstestaccount001.streaming.mediaservices.windows.net/ebf733c4-3e2e-4a68-b67b-cc5159d1d7f2/BigBuckBunny.ism/manifest

toostream HLS, Lägg till (format = m3u8 aapl) efter hello ”/ manifest”.

    http://amstestaccount001.streaming.mediaservices.windows.net/ebf733c4-3e2e-4a68-b67b-cc5159d1d7f2/BigBuckBunny.ism/manifest(format=m3u8-aapl)

toostream MPEG DASH, Lägg till (format = mpd-tid-csf) efter hello ”/ manifest”.

    http://amstestaccount001.streaming.mediaservices.windows.net/ebf733c4-3e2e-4a68-b67b-cc5159d1d7f2/BigBuckBunny.ism/manifest(format=mpd-time-csf)


## <a id="play"></a>Spela upp ditt innehåll
toostream du video-använder [Azure Media Services Player](http://amsplayer.azurewebsites.net/azuremediaplayer.html).

tootest progressiv hämtning, klistra in en URL i en webbläsare (till exempel Internet Explorer, Chrome, Safari).

## <a name="next-steps-media-services-learning-paths"></a>Nästa steg: sökvägar för Media Services-utbildning
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Ge feedback
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]
