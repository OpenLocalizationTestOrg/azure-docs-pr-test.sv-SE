---
title: "aaaUpload filer till ett Media Services-konto med hjälp av REST | Microsoft Docs"
description: "Lär dig hur tooget media innehåll i Media Services genom att skapa och ladda upp tillgångar."
services: media-services
documentationcenter: 
author: Juliako
manager: cfowler
editor: 
ms.assetid: 41df7cbe-b8e2-48c1-a86c-361ec4e5251f
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/10/2017
ms.author: juliako
ms.openlocfilehash: 2a92cecdc32d747d7a478946f069c15931eb32b9
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="upload-files-into-a-media-services-account-using-rest"></a><span data-ttu-id="c471f-103">Ladda upp filer till ett Media Services-konto med hjälp av REST</span><span class="sxs-lookup"><span data-stu-id="c471f-103">Upload files into a Media Services account using REST</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="c471f-104">.NET</span><span class="sxs-lookup"><span data-stu-id="c471f-104">.NET</span></span>](media-services-dotnet-upload-files.md)
> * [<span data-ttu-id="c471f-105">REST</span><span class="sxs-lookup"><span data-stu-id="c471f-105">REST</span></span>](media-services-rest-upload-files.md)
> * [<span data-ttu-id="c471f-106">Portal</span><span class="sxs-lookup"><span data-stu-id="c471f-106">Portal</span></span>](media-services-portal-upload-files.md)
> 
> 

<span data-ttu-id="c471f-107">I Media Services överför du dina digitala filer till en tillgång.</span><span class="sxs-lookup"><span data-stu-id="c471f-107">In Media Services, you upload your digital files into an asset.</span></span> <span data-ttu-id="c471f-108">Hej [tillgången](https://docs.microsoft.com/rest/api/media/operations/asset) entitet kan innehålla video, ljud, bilder, miniatyrsamlingar, text spår och textning filer (och hello metadata om dessa filer.)  När hello filerna har överförts till hello tillgång lagras innehållet på ett säkert sätt i hello molnet för ytterligare bearbetning och strömning.</span><span class="sxs-lookup"><span data-stu-id="c471f-108">hello [Asset](https://docs.microsoft.com/rest/api/media/operations/asset) entity can contain video, audio, images, thumbnail collections, text tracks and closed caption files (and hello metadata about these files.)  Once hello files are uploaded into hello asset, your content is stored securely in hello cloud for further processing and streaming.</span></span> 

> [!NOTE]
> <span data-ttu-id="c471f-109">det gäller hello följande överväganden:</span><span class="sxs-lookup"><span data-stu-id="c471f-109">hello following considerations apply:</span></span>
> 
> * <span data-ttu-id="c471f-110">Media Services använder hello värdet hello IAssetFile.Name egenskapen när du skapar URL: er för hello direktuppspelning av innehåll (till exempel http://{AMSAccount}.origin.mediaservices.windows.net/{GUID}/{IAssetFile.Name}/streamingParameters.) Därför tillåts procent-encoding inte.</span><span class="sxs-lookup"><span data-stu-id="c471f-110">Media Services uses hello value of hello IAssetFile.Name property when building URLs for hello streaming content (for example, http://{AMSAccount}.origin.mediaservices.windows.net/{GUID}/{IAssetFile.Name}/streamingParameters.) For this reason, percent-encoding is not allowed.</span></span> <span data-ttu-id="c471f-111">Hej värdet för hello **namn** egenskapen får inte ha någon av följande hello [procent-encoding-reserverade tecken](http://en.wikipedia.org/wiki/Percent-encoding#Percent-encoding_reserved_characters):! *' ();: @& = + $, /? [] # % ”.</span><span class="sxs-lookup"><span data-stu-id="c471f-111">hello value of hello **Name** property cannot have any of hello following [percent-encoding-reserved characters](http://en.wikipedia.org/wiki/Percent-encoding#Percent-encoding_reserved_characters): !*'();:@&=+$,/?%#[]".</span></span> <span data-ttu-id="c471f-112">Dessutom det kan bara finnas ett '.' för hello filnamnstillägg.</span><span class="sxs-lookup"><span data-stu-id="c471f-112">Also, there can only be one '.' for hello file name extension.</span></span>
> * <span data-ttu-id="c471f-113">hello längd på hello namn får inte vara större än 260 tecken.</span><span class="sxs-lookup"><span data-stu-id="c471f-113">hello length of hello name should not be greater than 260 characters.</span></span>
> * <span data-ttu-id="c471f-114">Det finns en gräns toohello maximal filstorlek som stöds för bearbetning i Media Services.</span><span class="sxs-lookup"><span data-stu-id="c471f-114">There is a limit toohello maximum file size supported for processing in Media Services.</span></span> <span data-ttu-id="c471f-115">Se [detta](media-services-quotas-and-limitations.md) avsnittet för information om hello filstorleksbegränsningar.</span><span class="sxs-lookup"><span data-stu-id="c471f-115">Please see [this](media-services-quotas-and-limitations.md) topic for details about hello file size limitation.</span></span>
> 

<span data-ttu-id="c471f-116">hello grundläggande arbetsflöde för uppladdning av tillgångar är indelad i följande avsnitt hello:</span><span class="sxs-lookup"><span data-stu-id="c471f-116">hello basic workflow for uploading Assets is divided into hello following sections:</span></span>

* <span data-ttu-id="c471f-117">Skapa en tillgång</span><span class="sxs-lookup"><span data-stu-id="c471f-117">Create an Asset</span></span>
* <span data-ttu-id="c471f-118">Kryptera en tillgång (valfritt)</span><span class="sxs-lookup"><span data-stu-id="c471f-118">Encrypt an Asset (Optional)</span></span>
* <span data-ttu-id="c471f-119">Överför en fil tooblob lagring</span><span class="sxs-lookup"><span data-stu-id="c471f-119">Upload a file tooblob storage</span></span>

<span data-ttu-id="c471f-120">AMS kan du också tooupload tillgångar gruppvis.</span><span class="sxs-lookup"><span data-stu-id="c471f-120">AMS also enables you tooupload assets in bulk.</span></span> <span data-ttu-id="c471f-121">Mer information finns i [det här](media-services-rest-upload-files.md#upload_in_bulk) avsnittet.</span><span class="sxs-lookup"><span data-stu-id="c471f-121">For more information, see [this](media-services-rest-upload-files.md#upload_in_bulk) section.</span></span>

> [!NOTE]
> <span data-ttu-id="c471f-122">Vid åtkomst till entiteter i Media Services måste du ange specifika namn på huvudfält och värden i HTTP-begäranden.</span><span class="sxs-lookup"><span data-stu-id="c471f-122">When accessing entities in Media Services, you must set specific header fields and values in your HTTP requests.</span></span> <span data-ttu-id="c471f-123">Mer information finns i [installationsprogrammet för Media Services REST API-utveckling](media-services-rest-how-to-use.md).</span><span class="sxs-lookup"><span data-stu-id="c471f-123">For more information, see [Setup for Media Services REST API Development](media-services-rest-how-to-use.md).</span></span>
> 

## <a name="connect-toomedia-services"></a><span data-ttu-id="c471f-124">Ansluta tooMedia tjänster</span><span class="sxs-lookup"><span data-stu-id="c471f-124">Connect tooMedia Services</span></span>

<span data-ttu-id="c471f-125">Mer information om hur tooconnect toohello AMS API, se [åtkomst hello Azure Media Services API med Azure AD authentication](media-services-use-aad-auth-to-access-ams-api.md).</span><span class="sxs-lookup"><span data-stu-id="c471f-125">For information on how tooconnect toohello AMS API, see [Access hello Azure Media Services API with Azure AD authentication](media-services-use-aad-auth-to-access-ams-api.md).</span></span> 

>[!NOTE]
><span data-ttu-id="c471f-126">När du har anslutit toohttps://media.windows.net, får du en 301 omdirigering att ange en annan Media Services-URI.</span><span class="sxs-lookup"><span data-stu-id="c471f-126">After successfully connecting toohttps://media.windows.net, you will receive a 301 redirect specifying another Media Services URI.</span></span> <span data-ttu-id="c471f-127">Du måste göra följande anrop toohello ny URI.</span><span class="sxs-lookup"><span data-stu-id="c471f-127">You must make subsequent calls toohello new URI.</span></span>

## <a name="upload-assets"></a><span data-ttu-id="c471f-128">Överför tillgångar</span><span class="sxs-lookup"><span data-stu-id="c471f-128">Upload assets</span></span>

### <a name="create-an-asset"></a><span data-ttu-id="c471f-129">Skapa en tillgång</span><span class="sxs-lookup"><span data-stu-id="c471f-129">Create an asset</span></span>

<span data-ttu-id="c471f-130">En tillgång är en behållare för flera typer eller uppsättningar med objekt i Media Services, inklusive video, ljud, bilder, miniatyrsamlingar, textspår och filer med dold textning.</span><span class="sxs-lookup"><span data-stu-id="c471f-130">An asset is a container for multiple types or sets of objects in Media Services, including video, audio, images, thumbnail collections, text tracks, and closed caption files.</span></span> <span data-ttu-id="c471f-131">I hello REST-API: skapa en tillgång måste du skicka POST begära tooMedia tjänster och placerar egenskapsinformation om din tillgång i hello frågans brödtext.</span><span class="sxs-lookup"><span data-stu-id="c471f-131">In hello REST API, creating an Asset requires sending POST request tooMedia Services and placing any property information about your asset in hello request body.</span></span>

<span data-ttu-id="c471f-132">En av hello egenskaper som du kan ange när du skapar en tillgång är **alternativ**.</span><span class="sxs-lookup"><span data-stu-id="c471f-132">One of hello properties that you can specify when creating an asset is **Options**.</span></span> <span data-ttu-id="c471f-133">**Alternativ för** är ett uppräkningsvärde som beskriver hello krypteringsalternativ som du kan skapa en tillgång med.</span><span class="sxs-lookup"><span data-stu-id="c471f-133">**Options** is an enumeration value that describes hello encryption options that an Asset can be created with.</span></span> <span data-ttu-id="c471f-134">Ett giltigt värde är ett av hello värden hello listan nedan, inte en kombination av värden.</span><span class="sxs-lookup"><span data-stu-id="c471f-134">A valid value is one of hello values from hello list below, not a combination of values.</span></span> 

* <span data-ttu-id="c471f-135">**Ingen** = **0**: Ingen kryptering används.</span><span class="sxs-lookup"><span data-stu-id="c471f-135">**None** = **0**: No encryption will be used.</span></span> <span data-ttu-id="c471f-136">Detta är standardvärdet för hello.</span><span class="sxs-lookup"><span data-stu-id="c471f-136">This is hello default value.</span></span> <span data-ttu-id="c471f-137">Observera att när du använder det här alternativet om ditt innehåll inte skyddas under överföringen eller i vila i lagring.</span><span class="sxs-lookup"><span data-stu-id="c471f-137">Note that when using this option your content is not protected in transit or at rest in storage.</span></span>
    <span data-ttu-id="c471f-138">Använd det här alternativet om du planerar toodeliver en MP4 med progressivt nedladdning.</span><span class="sxs-lookup"><span data-stu-id="c471f-138">If you plan toodeliver an MP4 using progressive download, use this option.</span></span> 
* <span data-ttu-id="c471f-139">**StorageEncrypted** = **1**: Ange om du vill använda för dina filer toobe som krypterats med AES 256-bitarskryptering för överföring och lagring.</span><span class="sxs-lookup"><span data-stu-id="c471f-139">**StorageEncrypted** = **1**: Specify if you want for your files toobe encrypted with AES-256 bit encryption for upload and storage.</span></span>
  
    <span data-ttu-id="c471f-140">Om tillgången är lagringskrypterad, måste du konfigurera principen för tillgångsleverans.</span><span class="sxs-lookup"><span data-stu-id="c471f-140">If your asset is storage encrypted, you must configure asset delivery policy.</span></span> <span data-ttu-id="c471f-141">Mer information finns i [konfigurera tillgångsleveransprincip](media-services-rest-configure-asset-delivery-policy.md).</span><span class="sxs-lookup"><span data-stu-id="c471f-141">For more information see [Configuring asset delivery policy](media-services-rest-configure-asset-delivery-policy.md).</span></span>
* <span data-ttu-id="c471f-142">**CommonEncryptionProtected** = **2**: Ange om du överför filer som skyddas med en gemensam krypteringsmetod (till exempel PlayReady).</span><span class="sxs-lookup"><span data-stu-id="c471f-142">**CommonEncryptionProtected** = **2**: Specify if you are uploading files protected with a common encryption method (such as PlayReady).</span></span> 
* <span data-ttu-id="c471f-143">**EnvelopeEncryptionProtected** = **4**: Ange om du överför HLS som krypterats med AES-filer.</span><span class="sxs-lookup"><span data-stu-id="c471f-143">**EnvelopeEncryptionProtected** = **4**: Specify if you are uploading HLS encrypted with AES files.</span></span> <span data-ttu-id="c471f-144">Observera att hello filer måste har kodats och krypterats av Transform Manager.</span><span class="sxs-lookup"><span data-stu-id="c471f-144">Note that hello files must have been encoded and encrypted by Transform Manager.</span></span>

> [!NOTE]
> <span data-ttu-id="c471f-145">Om din tillgång kommer att använda kryptering, måste du skapa en **ContentKey** och länka det tooyour tillgång enligt beskrivningen i följande ämne hello:[hur toocreate en ContentKey](media-services-rest-create-contentkey.md).</span><span class="sxs-lookup"><span data-stu-id="c471f-145">If your asset will use encryption, you must create a **ContentKey** and link it tooyour asset as described in hello following topic:[How toocreate a ContentKey](media-services-rest-create-contentkey.md).</span></span> <span data-ttu-id="c471f-146">Observera att när du har överfört hello filer till hello tillgång du egenskaper för tooupdate hello kryptering på hello **AssetFile** entitet med hello-värden som du har fått under hello **tillgången** kryptering.</span><span class="sxs-lookup"><span data-stu-id="c471f-146">Note that after you upload hello files into hello asset, you need tooupdate hello encryption properties on hello **AssetFile** entity with hello values you got during hello **Asset** encryption.</span></span> <span data-ttu-id="c471f-147">Göra det med hjälp av hello **sammanfoga** HTTP-begäran.</span><span class="sxs-lookup"><span data-stu-id="c471f-147">Do it by using hello **MERGE** HTTP request.</span></span> 
> 
> 

<span data-ttu-id="c471f-148">följande exempel visar hur hello toocreate en tillgång.</span><span class="sxs-lookup"><span data-stu-id="c471f-148">hello following example shows how toocreate an asset.</span></span>

<span data-ttu-id="c471f-149">**HTTP-begäran**</span><span class="sxs-lookup"><span data-stu-id="c471f-149">**HTTP Request**</span></span>

    POST https://media.windows.net/api/Assets HTTP/1.1
    Content-Type: application/json
    DataServiceVersion: 1.0;NetFx
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json
    Accept-Charset: UTF-8
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=amstestaccount001&urn%3aSubscriptionId=z7f09258-6753-2233-b1ae-193798e2c9d8&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1421640053&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=vlG%2fPYdFDMS1zKc36qcFVWnaNh07UCkhYj3B71%2fk1YA%3d
    x-ms-version: 2.11
    Host: media.windows.net

    {"Name":"BigBuckBunny.mp4"}

<span data-ttu-id="c471f-150">**HTTP-svar**</span><span class="sxs-lookup"><span data-stu-id="c471f-150">**HTTP Response**</span></span>

<span data-ttu-id="c471f-151">Om det lyckas, returneras hello följande:</span><span class="sxs-lookup"><span data-stu-id="c471f-151">If successful, hello following is returned:</span></span>

    HTP/1.1 201 Created
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
    Strict-Transport-Security: max-age=31536000; includeSubDomains
    Date: Sun, 18 Jan 2015 22:06:40 GMT
    {  
       "odata.metadata":"https://wamsbayclus001rest-hs.cloudapp.net/api/$metadata#Assets/@Element",
       "Id":"nb:cid:UUID:9bc8ff20-24fb-4fdb-9d7c-b04c7ee573a1",
       "State":0,
       "Created":"2015-01-18T22:06:40.6010903Z",
       "LastModified":"2015-01-18T22:06:40.6010903Z",
       "AlternateId":null,
       "Name":"BigBuckBunny.mp4",
       "Options":0,
       "Uri":"https://storagetestaccount001.blob.core.windows.net/asset-9bc8ff20-24fb-4fdb-9d7c-b04c7ee573a1",
       "StorageAccountName":"storagetestaccount001"
    }

### <a name="create-an-assetfile"></a><span data-ttu-id="c471f-152">Skapa en AssetFile</span><span class="sxs-lookup"><span data-stu-id="c471f-152">Create an AssetFile</span></span>
<span data-ttu-id="c471f-153">Hej [AssetFile](https://docs.microsoft.com/rest/api/media/operations/assetfile) entiteten representerar en video eller ljud-fil som lagras i en blob-behållare.</span><span class="sxs-lookup"><span data-stu-id="c471f-153">hello [AssetFile](https://docs.microsoft.com/rest/api/media/operations/assetfile) entity represents a video or audio file that is stored in a blob container.</span></span> <span data-ttu-id="c471f-154">En resursfil är alltid associerat med en tillgång och en tillgång kan innehålla en eller många tillgångsfiler.</span><span class="sxs-lookup"><span data-stu-id="c471f-154">An asset file is always associated with an asset, and an asset may contain one or many asset files.</span></span> <span data-ttu-id="c471f-155">hello misslyckas Media Services-kodaren om objekttypen tillgången filen inte är associerad med en digital fil i en blob-behållaren.</span><span class="sxs-lookup"><span data-stu-id="c471f-155">hello Media Services Encoder task fails if an asset file object is not associated with a digital file in a blob container.</span></span>

<span data-ttu-id="c471f-156">Observera att hello **AssetFile** instansen och hello faktiska mediefilen är två distinkta objekt.</span><span class="sxs-lookup"><span data-stu-id="c471f-156">Note that hello **AssetFile** instance and hello actual media file are two distinct objects.</span></span> <span data-ttu-id="c471f-157">Hej AssetFile instans innehåller metadata om hello mediefilen när hello mediefilen innehåller hello faktiskt medieinnehåll.</span><span class="sxs-lookup"><span data-stu-id="c471f-157">hello AssetFile instance contains metadata about hello media file, while hello media file contains hello actual media content.</span></span>

<span data-ttu-id="c471f-158">När du har överfört din digitala media-fil till en blobbbehållare använder hello **sammanfoga** HTTP-begäran tooupdate hello AssetFile med information om media-fil (som visas i hello kapitel).</span><span class="sxs-lookup"><span data-stu-id="c471f-158">After you upload your digital media file into a blob container, you will use hello **MERGE** HTTP request tooupdate hello AssetFile with information about your media file (as shown later in hello topic).</span></span> 

<span data-ttu-id="c471f-159">**HTTP-begäran**</span><span class="sxs-lookup"><span data-stu-id="c471f-159">**HTTP Request**</span></span>

    POST https://media.windows.net/api/Files HTTP/1.1
    Content-Type: application/json
    DataServiceVersion: 1.0;NetFx
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json
    Accept-Charset: UTF-8
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=amstestaccount001&urn%3aSubscriptionId=z7f09258-6753-4ca2-2233-193798e2c9d8&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1421640053&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=vlG%2fPYdFDMS1zKc36qcFVWnaNh07UCkhYj3B71%2fk1YA%3d
    x-ms-version: 2.11
    Host: media.windows.net
    Content-Length: 164

    {  
       "IsEncrypted":"false",
       "IsPrimary":"false",
       "MimeType":"video/mp4",
       "Name":"BigBuckBunny.mp4",
       "ParentAssetId":"nb:cid:UUID:9bc8ff20-24fb-4fdb-9d7c-b04c7ee573a1"
    }

<span data-ttu-id="c471f-160">**HTTP-svar**</span><span class="sxs-lookup"><span data-stu-id="c471f-160">**HTTP Response**</span></span>

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

### <a name="creating-hello-accesspolicy-with-write-permission"></a><span data-ttu-id="c471f-161">Skapar hello AccessPolicy med behörighet att skriva.</span><span class="sxs-lookup"><span data-stu-id="c471f-161">Creating hello AccessPolicy with write permission.</span></span>

>[!NOTE]
><span data-ttu-id="c471f-162">Det finns en gräns på 1 000 000 principer för olika AMS-principer (till exempel för positionerarprincipen eller ContentKeyAuthorizationPolicy).</span><span class="sxs-lookup"><span data-stu-id="c471f-162">There is a limit of 1,000,000 policies for different AMS policies (for example, for Locator policy or ContentKeyAuthorizationPolicy).</span></span> <span data-ttu-id="c471f-163">Du bör använda hello samma princip-ID om du alltid använder hello samma dagar / åtkomstbehörigheter, till exempel principer för lokaliserare som är avsedda tooremain på plats för lång tid (icke-överföringen principer).</span><span class="sxs-lookup"><span data-stu-id="c471f-163">You should use hello same policy ID if you are always using hello same days / access permissions, for example, policies for locators that are intended tooremain in place for a long time (non-upload policies).</span></span> <span data-ttu-id="c471f-164">Mer information finns i [detta](media-services-dotnet-manage-entities.md#limit-access-policies) avsnitt.</span><span class="sxs-lookup"><span data-stu-id="c471f-164">For more information, see [this](media-services-dotnet-manage-entities.md#limit-access-policies) topic.</span></span>

<span data-ttu-id="c471f-165">Innan du laddar upp filer i blob-lagring, ange hello princip rättigheter för att skriva tooan tillgången.</span><span class="sxs-lookup"><span data-stu-id="c471f-165">Before uploading any files into blob storage, set hello access policy rights for writing tooan asset.</span></span> <span data-ttu-id="c471f-166">Ange toodo som PUBLICERAR en HTTP-begäran toohello AccessPolicies entitet.</span><span class="sxs-lookup"><span data-stu-id="c471f-166">toodo that, POST an HTTP request toohello AccessPolicies entity set.</span></span> <span data-ttu-id="c471f-167">Ange ett värde för DurationInMinutes när de skapas eller du får felmeddelandet 500 intern Server tillbaka som svar.</span><span class="sxs-lookup"><span data-stu-id="c471f-167">Define a DurationInMinutes value upon creation or you will receive a 500 Internal Server error message back in response.</span></span> <span data-ttu-id="c471f-168">Mer information om AccessPolicies finns [AccessPolicy](https://docs.microsoft.com/rest/api/media/operations/accesspolicy).</span><span class="sxs-lookup"><span data-stu-id="c471f-168">For more information on AccessPolicies, see [AccessPolicy](https://docs.microsoft.com/rest/api/media/operations/accesspolicy).</span></span>

<span data-ttu-id="c471f-169">följande exempel visar hur hello toocreate en AccessPolicy:</span><span class="sxs-lookup"><span data-stu-id="c471f-169">hello following example shows how toocreate an AccessPolicy:</span></span>

<span data-ttu-id="c471f-170">**HTTP-begäran**</span><span class="sxs-lookup"><span data-stu-id="c471f-170">**HTTP Request**</span></span>

    POST https://media.windows.net/api/AccessPolicies HTTP/1.1
    Content-Type: application/json
    DataServiceVersion: 1.0;NetFx
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json
    Accept-Charset: UTF-8
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=amstestaccount001&urn%3aSubscriptionId=z7f09258-6753-2233-b1ae-193798e2c9d8&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1421640053&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=vlG%2fPYdFDMS1zKc36qcFVWnaNh07UCkhYj3B71%2fk1YA%3d
    x-ms-version: 2.11
    Host: media.windows.net

    {"Name":"NewUploadPolicy", "DurationInMinutes":"440", "Permissions":"2"} 

<span data-ttu-id="c471f-171">**HTTP-begäran**</span><span class="sxs-lookup"><span data-stu-id="c471f-171">**HTTP Request**</span></span>

    If successful, hello following response is returned:

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

### <a name="get-hello-upload-url"></a><span data-ttu-id="c471f-172">Hämta hello URL för överföring</span><span class="sxs-lookup"><span data-stu-id="c471f-172">Get hello Upload URL</span></span>
<span data-ttu-id="c471f-173">tooreceive hello faktiska överför URL, skapa en SAS-lokaliserare.</span><span class="sxs-lookup"><span data-stu-id="c471f-173">tooreceive hello actual upload URL, create a SAS Locator.</span></span> <span data-ttu-id="c471f-174">Lokaliserare definiera hello starttid och typ av anslutningens slutpunkt för klienter som vill tooaccess filer i en tillgång.</span><span class="sxs-lookup"><span data-stu-id="c471f-174">Locators define hello start time and type of connection endpoint for clients that want tooaccess Files in an Asset.</span></span> <span data-ttu-id="c471f-175">Du kan skapa flera lokaliserare entiteter för en viss AccessPolicy och tillgångshantering par toohandle olika klient begäranden och behov.</span><span class="sxs-lookup"><span data-stu-id="c471f-175">You can create multiple Locator entities for a given AccessPolicy and Asset pair toohandle different client requests and needs.</span></span> <span data-ttu-id="c471f-176">Var och en av dessa lokaliserare använda hello StartTime-värdet plus hello DurationInMinutes värdet av hello AccessPolicy toodetermine hello tid kan användas för en URL.</span><span class="sxs-lookup"><span data-stu-id="c471f-176">Each of these Locators use hello StartTime value plus hello DurationInMinutes value of hello AccessPolicy toodetermine hello length of time a URL can be used.</span></span> <span data-ttu-id="c471f-177">Mer information finns i [lokaliserare](https://docs.microsoft.com/rest/api/media/operations/locator).</span><span class="sxs-lookup"><span data-stu-id="c471f-177">For more information, see [Locator](https://docs.microsoft.com/rest/api/media/operations/locator).</span></span>

<span data-ttu-id="c471f-178">En SAS-URL har hello följande format:</span><span class="sxs-lookup"><span data-stu-id="c471f-178">A SAS URL has hello following format:</span></span>

    {https://myaccount.blob.core.windows.net}/{asset name}/{video file name}?{SAS signature}

<span data-ttu-id="c471f-179">Vissa förutsättningar gäller:</span><span class="sxs-lookup"><span data-stu-id="c471f-179">Some considerations apply:</span></span>

* <span data-ttu-id="c471f-180">Du kan inte ha fler än fem unika lokaliserare som är associerade med en viss resurs i taget.</span><span class="sxs-lookup"><span data-stu-id="c471f-180">You cannot have more than five unique Locators associated with a given Asset at one time.</span></span> <span data-ttu-id="c471f-181">Mer information finns i lokaliserare.</span><span class="sxs-lookup"><span data-stu-id="c471f-181">For more information, see Locator.</span></span>
* <span data-ttu-id="c471f-182">Om du behöver tooupload dina filer direkt, bör du ange StartTime värdet toofive minuter innan hello aktuell tid.</span><span class="sxs-lookup"><span data-stu-id="c471f-182">If you need tooupload your files immediately, you should set your StartTime value toofive minutes before hello current time.</span></span> <span data-ttu-id="c471f-183">Det beror på att det kan finnas klockan skeva mellan klientdatorn och Media Services.</span><span class="sxs-lookup"><span data-stu-id="c471f-183">This is because there may be clock skew between your client machine and Media Services.</span></span> <span data-ttu-id="c471f-184">StartTime-värdet måste också vara i hello följande DateTime-format: ÅÅÅÅ-MM-ddTHH (till exempel ”2014-05-23T17:53:50Z”).</span><span class="sxs-lookup"><span data-stu-id="c471f-184">Also, your StartTime value must be in hello following DateTime format: YYYY-MM-DDTHH:mm:ssZ (for example, "2014-05-23T17:53:50Z").</span></span>    
* <span data-ttu-id="c471f-185">Det kan finnas en 30-40 andra fördröjning efter en positionerare skapas toowhen som den är tillgänglig för användning.</span><span class="sxs-lookup"><span data-stu-id="c471f-185">There may be a 30-40 second delay after a Locator is created toowhen it is available for use.</span></span> <span data-ttu-id="c471f-186">Det här problemet gäller tooboth SAS-URL och ursprung lokaliserare.</span><span class="sxs-lookup"><span data-stu-id="c471f-186">This issue applies tooboth SAS URL and Origin Locators.</span></span>

<span data-ttu-id="c471f-187">hello som följande exempel visar hur toocreate en SAS-URL-lokaliserare, som definieras av hello typegenskapen i begärandetexten hello (”1” för en SAS-lokaliserare) och ”2” för en positionerare för ursprung på begäran.</span><span class="sxs-lookup"><span data-stu-id="c471f-187">hello following example shows how toocreate a SAS URL Locator, as defined by hello Type property in hello request body ("1" for a SAS locator and "2" for an On-Demand origin locator).</span></span> <span data-ttu-id="c471f-188">Hej **sökväg** egenskap som returneras innehåller hello-URL att du måste använda tooupload din fil.</span><span class="sxs-lookup"><span data-stu-id="c471f-188">hello **Path** property returned contains hello URL that you must use tooupload your file.</span></span>

<span data-ttu-id="c471f-189">**HTTP-begäran**</span><span class="sxs-lookup"><span data-stu-id="c471f-189">**HTTP Request**</span></span>

    POST https://media.windows.net/api/Locators HTTP/1.1
    Content-Type: application/json
    DataServiceVersion: 1.0;NetFx
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json
    Accept-Charset: UTF-8
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=amstestaccount001&urn%3aSubscriptionId=z7f09258-6753-4ca2-2233-193798e2c9d8&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1421640053&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=vlG%2fPYdFDMS1zKc36qcFVWnaNh07UCkhYj3B71%2fk1YA%3d
    x-ms-version: 2.11
    Host: media.windows.net
    {  
       "AccessPolicyId":"nb:pid:UUID:be0ac48d-af7d-4877-9d60-1805d68bffae",
       "AssetId":"nb:cid:UUID:9bc8ff20-24fb-4fdb-9d7c-b04c7ee573a1",
       "StartTime":"2015-02-18T16:45:53",
       "Type":1
    }

<span data-ttu-id="c471f-190">**HTTP-svar**</span><span class="sxs-lookup"><span data-stu-id="c471f-190">**HTTP Response**</span></span>

<span data-ttu-id="c471f-191">Om det lyckas, returneras hello efter svar:</span><span class="sxs-lookup"><span data-stu-id="c471f-191">If successful, hello following response is returned:</span></span>

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

### <a name="upload-a-file-into-a-blob-storage-container"></a><span data-ttu-id="c471f-192">Ladda upp en fil till en behållare för blob storage</span><span class="sxs-lookup"><span data-stu-id="c471f-192">Upload a file into a blob storage container</span></span>
<span data-ttu-id="c471f-193">När du har hello AccessPolicy och lokaliserare set är hello själva filen överförda tooan Azure Blob Storage-behållare med hello Azure Storage REST API: er.</span><span class="sxs-lookup"><span data-stu-id="c471f-193">Once you have hello AccessPolicy and Locator set, hello actual file is uploaded tooan Azure Blob Storage container using hello Azure Storage REST APIs.</span></span> <span data-ttu-id="c471f-194">Du måste överföra hello filer som blockblobar.</span><span class="sxs-lookup"><span data-stu-id="c471f-194">You must upload hello files as block blobs.</span></span> <span data-ttu-id="c471f-195">Azure Media Services stöder inte sidblobar.</span><span class="sxs-lookup"><span data-stu-id="c471f-195">Page blobs are not supported by Azure Media Services.</span></span>  

> [!NOTE]
> <span data-ttu-id="c471f-196">Du måste lägga till hello filnamn hello filen du vill ha tooupload toohello lokaliserare **sökväg** värde togs emot i hello föregående avsnitt.</span><span class="sxs-lookup"><span data-stu-id="c471f-196">You must add hello file name for hello file you want tooupload toohello Locator **Path** value received in hello previous section.</span></span> <span data-ttu-id="c471f-197">Till exempel https://storagetestaccount001.blob.core.windows.net/asset-e7b02da4-5a69-40e7-a8db-e8f4f697aac0/BigBuckBunny.mp4?</span><span class="sxs-lookup"><span data-stu-id="c471f-197">For example, https://storagetestaccount001.blob.core.windows.net/asset-e7b02da4-5a69-40e7-a8db-e8f4f697aac0/BigBuckBunny.mp4?</span></span> <span data-ttu-id="c471f-198">.</span><span class="sxs-lookup"><span data-stu-id="c471f-198">.</span></span> <span data-ttu-id="c471f-199">.</span><span class="sxs-lookup"><span data-stu-id="c471f-199">.</span></span> <span data-ttu-id="c471f-200">.</span><span class="sxs-lookup"><span data-stu-id="c471f-200">.</span></span> 
> 
> 

<span data-ttu-id="c471f-201">Mer information om hur du arbetar med Azure storage blobs finns [REST-API för Blob-tjänsten](https://docs.microsoft.com/rest/api/storageservices/Blob-Service-REST-API).</span><span class="sxs-lookup"><span data-stu-id="c471f-201">For more information on working with Azure storage blobs, see [Blob Service REST API](https://docs.microsoft.com/rest/api/storageservices/Blob-Service-REST-API).</span></span>

### <a name="update-hello-assetfile"></a><span data-ttu-id="c471f-202">Uppdatera hello AssetFile</span><span class="sxs-lookup"><span data-stu-id="c471f-202">Update hello AssetFile</span></span>
<span data-ttu-id="c471f-203">Nu när du har överfört din fil uppdateringsinformation hello FileAsset storlek (och andra).</span><span class="sxs-lookup"><span data-stu-id="c471f-203">Now that you've uploaded your file, update hello FileAsset size (and other) information.</span></span> <span data-ttu-id="c471f-204">Exempel:</span><span class="sxs-lookup"><span data-stu-id="c471f-204">For example:</span></span>

    MERGE https://media.windows.net/api/Files('nb%3Acid%3AUUID%3Af13a0137-0a62-9d4c-b3b9-ca944b5142c5') HTTP/1.1
    Content-Type: application/json
    DataServiceVersion: 1.0;NetFx
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json
    Accept-Charset: UTF-8
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=amstestaccount001&urn%3aSubscriptionId=z7f09258-6753-4ca2-2233-193798e2c9d8&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1421662918&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=utmoXXbm9Q7j4tW1yJuMVA3egRiQy5FPygwadkmPeaY%3d
    x-ms-version: 2.11
    Host: media.windows.net

    {  
       "ContentFileSize":"1186540",
       "Id":"nb:cid:UUID:f13a0137-0a62-9d4c-b3b9-ca944b5142c5",
       "MimeType":"video/mp4",
       "Name":"BigBuckBunny.mp4",
       "ParentAssetId":"nb:cid:UUID:9bc8ff20-24fb-4fdb-9d7c-b04c7ee573a1"
    }


<span data-ttu-id="c471f-205">**HTTP-svar**</span><span class="sxs-lookup"><span data-stu-id="c471f-205">**HTTP Response**</span></span>

<span data-ttu-id="c471f-206">Om det lyckas hello följande returneras: HTTP/1.1 204 Nej innehåll</span><span class="sxs-lookup"><span data-stu-id="c471f-206">If successful, hello following is returned: HTTP/1.1 204 No Content</span></span>

### <a name="delete-hello-locator-and-accesspolicy"></a><span data-ttu-id="c471f-207">Ta bort hello lokaliserare och AccessPolicy</span><span class="sxs-lookup"><span data-stu-id="c471f-207">Delete hello Locator and AccessPolicy</span></span>
<span data-ttu-id="c471f-208">**HTTP-begäran**</span><span class="sxs-lookup"><span data-stu-id="c471f-208">**HTTP Request**</span></span>

    DELETE https://media.windows.net/api/Locators('nb%3Alid%3AUUID%3Aaf57bdd8-6751-4e84-b403-f3c140444b54') HTTP/1.1
    DataServiceVersion: 1.0;NetFx
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json
    Accept-Charset: UTF-8
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=amstestaccount001&urn%3aSubscriptionId=z7f09258-6753-2233-b1ae-193798e2c9d8&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1421662918&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=utmoXXbm9Q7j4tW1yJuMVA3egRiQy5FPygwadkmPeaY%3d
    x-ms-version: 2.11
    Host: media.windows.net

<span data-ttu-id="c471f-209">**HTTP-svar**</span><span class="sxs-lookup"><span data-stu-id="c471f-209">**HTTP Response**</span></span>

<span data-ttu-id="c471f-210">Om det lyckas, returneras hello följande:</span><span class="sxs-lookup"><span data-stu-id="c471f-210">If successful, hello following is returned:</span></span>

    HTTP/1.1 204 No Content 
    ...

<span data-ttu-id="c471f-211">**HTTP-begäran**</span><span class="sxs-lookup"><span data-stu-id="c471f-211">**HTTP Request**</span></span>

    DELETE https://media.windows.net/api/AccessPolicies('nb%3Apid%3AUUID%3Abe0ac48d-af7d-4877-9d60-1805d68bffae') HTTP/1.1
    DataServiceVersion: 1.0;NetFx
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json
    Accept-Charset: UTF-8
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=amstestaccount001&urn%3aSubscriptionId=z7f09258-6753-2233-b1ae-193798e2c9d8&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1421662918&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=utmoXXbm9Q7j4tW1yJuMVA3egRiQy5FPygwadkmPeaY%3d
    x-ms-version: 2.11
    Host: media.windows.net

<span data-ttu-id="c471f-212">**HTTP-svar**</span><span class="sxs-lookup"><span data-stu-id="c471f-212">**HTTP Response**</span></span>

<span data-ttu-id="c471f-213">Om det lyckas, returneras hello följande:</span><span class="sxs-lookup"><span data-stu-id="c471f-213">If successful, hello following is returned:</span></span>

    HTTP/1.1 204 No Content 
    ...

## <span data-ttu-id="c471f-214"><a id="upload_in_bulk"></a>Överför tillgångar i grupp</span><span class="sxs-lookup"><span data-stu-id="c471f-214"><a id="upload_in_bulk"></a>Upload assets in bulk</span></span>
### <a name="create-hello-ingestmanifest"></a><span data-ttu-id="c471f-215">Skapa hello IngestManifest</span><span class="sxs-lookup"><span data-stu-id="c471f-215">Create hello IngestManifest</span></span>
<span data-ttu-id="c471f-216">Hej IngestManifest är en behållare för en uppsättning tillgångar, tillgångsfiler och statistik information som kan vara används toodetermine hello fortskrider samtidigt vill föra in för hello.</span><span class="sxs-lookup"><span data-stu-id="c471f-216">hello IngestManifest is a container for a set of assets, asset files, and statistic information that can be used toodetermine hello progress of bulk ingesting for hello set.</span></span>

<span data-ttu-id="c471f-217">**HTTP-begäran**</span><span class="sxs-lookup"><span data-stu-id="c471f-217">**HTTP Request**</span></span>

    POST https:// media.windows.net/API/IngestManifests HTTP/1.1
    Content-Type: application/json;odata=verbose
    Accept: application/json;odata=verbose
    DataServiceVersion: 3.0
    MaxDataServiceVersion: 3.0
    x-ms-version: 2.11
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=070500D0-F35C-4A5A-9249-485BBF4EC70B&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1334275521&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=GxdBb%2fmEyN7iHdNxbawawHRftLhPFFqxX1JZckuv3hY%3d
    Host: media.windows.net
    Content-Length: 36
    Expect: 100-continue

    { "Name" : "ExampleManifestREST" }

### <a name="create-assets"></a><span data-ttu-id="c471f-218">Skapa tillgångar</span><span class="sxs-lookup"><span data-stu-id="c471f-218">Create assets</span></span>
<span data-ttu-id="c471f-219">Innan du skapar hello IngestManifestAsset måste toocreate hello tillgång som ska utföras samtidigt vill föra in.</span><span class="sxs-lookup"><span data-stu-id="c471f-219">Before creating hello IngestManifestAsset, you need toocreate hello Asset that will be completed using bulk ingesting.</span></span> <span data-ttu-id="c471f-220">En tillgång är en behållare för flera typer eller uppsättningar med objekt i Media Services, inklusive video, ljud, bilder, miniatyrsamlingar, textspår och filer med dold textning.</span><span class="sxs-lookup"><span data-stu-id="c471f-220">An asset is a container for multiple types or sets of objects in Media Services, including video, audio, images, thumbnail collections, text tracks, and closed caption files.</span></span> <span data-ttu-id="c471f-221">I hello REST-API kräver skapar en tillgång skickar en HTTP POST-begäran tooMicrosoft Azure Media Services och placerar egenskapsinformation om din tillgång i hello frågans brödtext. I det här exemplet skapas hello tillgångsinformation med hjälp av hello StorageEncrption(1) alternativet medföljer hello frågans brödtext.</span><span class="sxs-lookup"><span data-stu-id="c471f-221">In hello REST API, creating an Asset requires sending a HTTP POST request tooMicrosoft Azure Media Services and placing any property information about your asset in hello request body.In this example, hello Asset is created using hello StorageEncrption(1) option included with hello request body.</span></span>

<span data-ttu-id="c471f-222">**HTTP-svar**</span><span class="sxs-lookup"><span data-stu-id="c471f-222">**HTTP Response**</span></span>

    POST https://media.windows.net/API/Assets HTTP/1.1
    Content-Type: application/json;odata=verbose
    Accept: application/json;odata=verbose
    DataServiceVersion: 3.0
    MaxDataServiceVersion: 3.0
    x-ms-version: 2.11
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=070500D0-F35C-4A5A-9249-485BBF4EC70B&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1334275521&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=GxdBb%2fmEyN7iHdNxbawawHRftLhPFFqxX1JZckuv3hY%3d
    Host: media.windows.net
    Content-Length: 55
    Expect: 100-continue

    { "Name" : "ExampleManifestREST_Asset", "Options" : 1 }

### <a name="create-hello-ingestmanifestassets"></a><span data-ttu-id="c471f-223">Skapa hello IngestManifestAssets</span><span class="sxs-lookup"><span data-stu-id="c471f-223">Create hello IngestManifestAssets</span></span>
<span data-ttu-id="c471f-224">IngestManifestAssets representerar tillgångar i en IngestManifest som används med samtidigt vill föra in.</span><span class="sxs-lookup"><span data-stu-id="c471f-224">IngestManifestAssets represent Assets within an IngestManifest that are used with bulk ingesting.</span></span> <span data-ttu-id="c471f-225">hello länkar i princip hello tillgången toohello manifest.</span><span class="sxs-lookup"><span data-stu-id="c471f-225">hello basically link hello asset toohello manifest.</span></span> <span data-ttu-id="c471f-226">Azure Media Services söker internt efter hello filöverföringen baserat på IngestManifestFiles samlingen som är kopplad toohello IngestManifestAsset.</span><span class="sxs-lookup"><span data-stu-id="c471f-226">Azure Media Services watches internally for hello file upload based on IngestManifestFiles collection associated toohello IngestManifestAsset.</span></span> <span data-ttu-id="c471f-227">När filerna har överförts är hello tillgången klar.</span><span class="sxs-lookup"><span data-stu-id="c471f-227">Once these files are uploaded, hello asset is completed.</span></span> <span data-ttu-id="c471f-228">Du kan skapa en ny IngestManifestAsset med en HTTP POST-begäran.</span><span class="sxs-lookup"><span data-stu-id="c471f-228">You can create a new IngestManifestAsset with a HTTP POST request.</span></span> <span data-ttu-id="c471f-229">I hello begärantext inkludera hello IngestManifest Id och hello tillgångsnummer som hello IngestManifestAsset bör länka samman för att föra in samtidigt.</span><span class="sxs-lookup"><span data-stu-id="c471f-229">In hello request body, include hello IngestManifest Id and hello Asset Id that hello IngestManifestAsset should link together for bulk ingesting.</span></span>

<span data-ttu-id="c471f-230">**HTTP-svar**</span><span class="sxs-lookup"><span data-stu-id="c471f-230">**HTTP Response**</span></span>

    POST https://media.windows.net/API/IngestManifestAssets HTTP/1.1
    Content-Type: application/json;odata=verbose
    Accept: application/json;odata=verbose
    DataServiceVersion: 3.0
    MaxDataServiceVersion: 3.0
    x-ms-version: 2.11
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=070500D0-F35C-4A5A-9249-485BBF4EC70B&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1334275521&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=GxdBb%2fmEyN7iHdNxbawawHRftLhPFFqxX1JZckuv3hY%3d
    Host: media.windows.net
    Content-Length: 152
    Expect: 100-continue
    { "ParentIngestManifestId" : "nb:mid:UUID:5c77f186-414f-8b48-8231-17f9264e2048", "Asset" : { "Id" : "nb:cid:UUID:b757929a-5a57-430b-b33e-c05c6cbef02e"}}


### <a name="create-hello-ingestmanifestfiles-for-each-asset"></a><span data-ttu-id="c471f-231">Skapa hello IngestManifestFiles för varje tillgång</span><span class="sxs-lookup"><span data-stu-id="c471f-231">Create hello IngestManifestFiles for each Asset</span></span>
<span data-ttu-id="c471f-232">En IngestManifestFile representerar ett faktiska video eller ljud blob-objekt som överförs som en del av bulk vill föra in för en tillgång.</span><span class="sxs-lookup"><span data-stu-id="c471f-232">An IngestManifestFile represents an actual video or audio blob object that will be uploaded as part of bulk ingesting for an asset.</span></span> <span data-ttu-id="c471f-233">Kryptering relaterade egenskaper krävs inte om inte hello tillgången använder ett krypteringsalternativ.</span><span class="sxs-lookup"><span data-stu-id="c471f-233">Encryption related properties are not required unless hello asset is using an encryption option.</span></span> <span data-ttu-id="c471f-234">hello-exempel som används i det här avsnittet visar skapar en IngestManifestFile som använder StorageEncryption för hello tillgången skapade tidigare.</span><span class="sxs-lookup"><span data-stu-id="c471f-234">hello example used in this section demonstrates creating an IngestManifestFile that uses StorageEncryption for hello Asset previously created.</span></span>

<span data-ttu-id="c471f-235">**HTTP-svar**</span><span class="sxs-lookup"><span data-stu-id="c471f-235">**HTTP Response**</span></span>

    POST https://media.windows.net/API/IngestManifestFiles HTTP/1.1
    Content-Type: application/json;odata=verbose
    Accept: application/json;odata=verbose
    DataServiceVersion: 3.0
    MaxDataServiceVersion: 3.0
    x-ms-version: 2.11
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=070500D0-F35C-4A5A-9249-485BBF4EC70B&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1334275521&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=GxdBb%2fmEyN7iHdNxbawawHRftLhPFFqxX1JZckuv3hY%3d
    Host: media.windows.net
    Content-Length: 367
    Expect: 100-continue

    { "Name" : "REST_Example_File.wmv", "ParentIngestManifestId" : "nb:mid:UUID:5c77f186-414f-8b48-8231-17f9264e2048", "ParentIngestManifestAssetId" : "nb:maid:UUID:beed8531-9a03-9043-b1d8-6a6d1044cdda", "IsEncrypted" : "true", "EncryptionScheme" : "StorageEncryption", "EncryptionVersion" : "1.0", "EncryptionKeyId" : "nb:kid:UUID:32e6efaf-5fba-4538-b115-9d1cefe43510" }

### <a name="upload-hello-files-tooblob-storage"></a><span data-ttu-id="c471f-236">Överför hello filer tooBlob lagring</span><span class="sxs-lookup"><span data-stu-id="c471f-236">Upload hello Files tooBlob Storage</span></span>
<span data-ttu-id="c471f-237">Du kan använda en hög hastighet klientprogrammet kan överföra hello tillgången filer toohello blob storage-behållare tillhandahålls av hello BlobStorageUriForUpload-egenskapen för hello IngestManifest Uri.</span><span class="sxs-lookup"><span data-stu-id="c471f-237">You can use any high speed client application capable of uploading hello asset files toohello blob storage container Uri provided by hello BlobStorageUriForUpload property of hello IngestManifest.</span></span> <span data-ttu-id="c471f-238">En tjänst för överföringen av viktiga hög hastighet är [Aspera på begäran för Azure-programmet](http://go.microsoft.com/fwlink/?LinkId=272001).</span><span class="sxs-lookup"><span data-stu-id="c471f-238">One notable high speed upload service is [Aspera On Demand for Azure Application](http://go.microsoft.com/fwlink/?LinkId=272001).</span></span>

### <a name="monitor-bulk-ingest-progress"></a><span data-ttu-id="c471f-239">Övervakaren Bulk mata in pågår</span><span class="sxs-lookup"><span data-stu-id="c471f-239">Monitor Bulk Ingest Progress</span></span>
<span data-ttu-id="c471f-240">Du kan övervaka hello samtidigt vill föra in åtgärder för en IngestManifest genom att avsöka hello statistik-egenskapen för hello IngestManifest.</span><span class="sxs-lookup"><span data-stu-id="c471f-240">You can monitor hello progress of bulk ingesting operations for an IngestManifest by polling hello Statistics property of hello IngestManifest.</span></span> <span data-ttu-id="c471f-241">Att egenskapen är en komplex typ [IngestManifestStatistics](https://docs.microsoft.com/rest/api/media/operations/ingestmanifeststatistics).</span><span class="sxs-lookup"><span data-stu-id="c471f-241">That property is a complex type, [IngestManifestStatistics](https://docs.microsoft.com/rest/api/media/operations/ingestmanifeststatistics).</span></span> <span data-ttu-id="c471f-242">toopoll hello statistik egenskapen skicka en HTTP GET-begäran skickas hello IngestManifest-Id.</span><span class="sxs-lookup"><span data-stu-id="c471f-242">toopoll hello Statistics property, submit a HTTP GET request passing hello IngestManifest Id.</span></span>

## <a name="create-contentkeys-used-for-encryption"></a><span data-ttu-id="c471f-243">Skapa ContentKeys som används för kryptering</span><span class="sxs-lookup"><span data-stu-id="c471f-243">Create ContentKeys used for encryption</span></span>
<span data-ttu-id="c471f-244">Om din tillgång kommer att använda kryptering, måste du skapa hello ContentKey toobe används för kryptering innan du skapar hello tillgångsfiler.</span><span class="sxs-lookup"><span data-stu-id="c471f-244">If your asset will use encryption, you must create hello ContentKey toobe used for encryption before creating hello asset files.</span></span> <span data-ttu-id="c471f-245">För lagringskryptering, ska hello följande egenskaper tas med i hello frågans brödtext.</span><span class="sxs-lookup"><span data-stu-id="c471f-245">For storage encryption, hello following properties should be included in hello request body.</span></span>

| <span data-ttu-id="c471f-246">Egenskapen för brödtext i begäran</span><span class="sxs-lookup"><span data-stu-id="c471f-246">Request body property</span></span> | <span data-ttu-id="c471f-247">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="c471f-247">Description</span></span> |
| --- | --- |
| <span data-ttu-id="c471f-248">Id</span><span class="sxs-lookup"><span data-stu-id="c471f-248">Id</span></span> |<span data-ttu-id="c471f-249">hello ContentKey-Id som vi generera oss själva med hjälp av hello följande format ”nb:kid:UUID:<NEW GUID>”.</span><span class="sxs-lookup"><span data-stu-id="c471f-249">hello ContentKey Id which we generate ourselves using hello following format, “nb:kid:UUID:<NEW GUID>”.</span></span> |
| <span data-ttu-id="c471f-250">ContentKeyType</span><span class="sxs-lookup"><span data-stu-id="c471f-250">ContentKeyType</span></span> |<span data-ttu-id="c471f-251">Detta är hello viktiga innehållstyp som ett heltal för den här nyckeln.</span><span class="sxs-lookup"><span data-stu-id="c471f-251">This is hello content key type as an integer for this content key.</span></span> <span data-ttu-id="c471f-252">Vi skickar hello värdet 1 för kryptering.</span><span class="sxs-lookup"><span data-stu-id="c471f-252">We pass hello value 1 for storage encryption.</span></span> |
| <span data-ttu-id="c471f-253">EncryptedContentKey</span><span class="sxs-lookup"><span data-stu-id="c471f-253">EncryptedContentKey</span></span> |<span data-ttu-id="c471f-254">Vi skapa ett nytt innehåll nyckelvärde som är en 256-bitars (32 byte)-värde.</span><span class="sxs-lookup"><span data-stu-id="c471f-254">We create a new content key value which is a 256-bit (32 byte) value.</span></span> <span data-ttu-id="c471f-255">hello nyckeln är krypterad med hello storage kryptering X.509-certifikat som vi hämta från Microsoft Azure Media Services genom att köra en HTTP GET-begäran för hello GetProtectionKeyId och GetProtectionKey metoder.</span><span class="sxs-lookup"><span data-stu-id="c471f-255">hello key is encrypted using hello storage encryption X.509 certificate which we retrieve from Microsoft Azure Media Services by executing a HTTP GET request for hello GetProtectionKeyId and GetProtectionKey Methods.</span></span> |
| <span data-ttu-id="c471f-256">ProtectionKeyId</span><span class="sxs-lookup"><span data-stu-id="c471f-256">ProtectionKeyId</span></span> |<span data-ttu-id="c471f-257">Detta är hello skydd nyckel-id för hello storage kryptering X.509-certifikat som har använt tooencrypt våra innehållsnyckeln.</span><span class="sxs-lookup"><span data-stu-id="c471f-257">This is hello protection key id for hello storage encryption X.509 certificate that was used tooencrypt our content key.</span></span> |
| <span data-ttu-id="c471f-258">ProtectionKeyType</span><span class="sxs-lookup"><span data-stu-id="c471f-258">ProtectionKeyType</span></span> |<span data-ttu-id="c471f-259">Detta är hello krypteringstyp för hello Skyddsnyckel som har använt tooencrypt hello innehållsnyckeln.</span><span class="sxs-lookup"><span data-stu-id="c471f-259">This is hello encryption type for hello protection key that was used tooencrypt hello content key.</span></span> <span data-ttu-id="c471f-260">Det här värdet är StorageEncryption(1) i vårt exempel.</span><span class="sxs-lookup"><span data-stu-id="c471f-260">This value is StorageEncryption(1) for our example.</span></span> |
| <span data-ttu-id="c471f-261">Kontrollsumma</span><span class="sxs-lookup"><span data-stu-id="c471f-261">Checksum</span></span> |<span data-ttu-id="c471f-262">hello MD5 beräknade kontrollsumman för hello innehållsnyckeln.</span><span class="sxs-lookup"><span data-stu-id="c471f-262">hello MD5 calculated checksum for hello content key.</span></span> <span data-ttu-id="c471f-263">Det beräknas genom att kryptera hello innehåll Id med hello innehållsnyckeln.</span><span class="sxs-lookup"><span data-stu-id="c471f-263">It is computed by encrypting hello content Id with hello content key.</span></span> <span data-ttu-id="c471f-264">hello exempelkod visar hur toocalculate hello kontrollsumma.</span><span class="sxs-lookup"><span data-stu-id="c471f-264">hello example code demonstrates how toocalculate hello checksum.</span></span> |

<span data-ttu-id="c471f-265">**HTTP-svar**</span><span class="sxs-lookup"><span data-stu-id="c471f-265">**HTTP Response**</span></span>

    POST https://media.windows.net/api/ContentKeys HTTP/1.1
    Content-Type: application/json;odata=verbose
    Accept: application/json;odata=verbose
    DataServiceVersion: 3.0
    MaxDataServiceVersion: 3.0
    x-ms-version: 2.11
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=070500D0-F35C-4A5A-9249-485BBF4EC70B&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1334275521&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=GxdBb%2fmEyN7iHdNxbawawHRftLhPFFqxX1JZckuv3hY%3d
    Host: media.windows.net
    Content-Length: 572
    Expect: 100-continue

    {"Id" : "nb:kid:UUID:316d14d4-b603-4d90-b8db-0fede8aa48f8", "ContentKeyType" : 1, "EncryptedContentKey" : "Y4NPej7heOFa2vsd8ZEOcjjpu/qOq3RJ6GRfxa8CCwtAM83d6J2mKOeQFUmMyVXUSsBCCOdufmieTKi+hOUtNAbyNM4lY4AXI537b9GaY8oSeje0NGU8+QCOuf7jGdRac5B9uIk7WwD76RAJnqyep6U/OdvQV4RLvvZ9w7nO4bY8RHaUaLxC2u4aIRRaZtLu5rm8GKBPy87OzQVXNgnLM01I8s3Z4wJ3i7jXqkknDy4VkIyLBSQvIvUzxYHeNdMVWDmS+jPN9ScVmolUwGzH1A23td8UWFHOjTjXHLjNm5Yq+7MIOoaxeMlKPYXRFKofRY8Qh5o5tqvycSAJ9KUqfg==", "ProtectionKeyId" : "7D9BB04D9D0A4A24800CADBFEF232689E048F69C", "ProtectionKeyType" : 1, "Checksum" : "TfXtjCIlq1Y=" }

### <a name="link-hello-contentkey-toohello-asset"></a><span data-ttu-id="c471f-266">Länka hello ContentKey toohello tillgångsinformation</span><span class="sxs-lookup"><span data-stu-id="c471f-266">Link hello ContentKey toohello Asset</span></span>
<span data-ttu-id="c471f-267">Hej ContentKey är associerade tooone eller flera resurser genom att skicka en HTTP POST-begäran.</span><span class="sxs-lookup"><span data-stu-id="c471f-267">hello ContentKey is associated tooone or more assets by sending a HTTP POST request.</span></span> <span data-ttu-id="c471f-268">hello är följande begäran ett exempel toolink hello exempel ContentKey toohello exempel tillgång efter-Id.</span><span class="sxs-lookup"><span data-stu-id="c471f-268">hello following request is an example toolink hello example ContentKey toohello example asset by Id.</span></span>

<span data-ttu-id="c471f-269">**HTTP-svar**</span><span class="sxs-lookup"><span data-stu-id="c471f-269">**HTTP Response**</span></span>

    POST https://media.windows.net/API/Assets('nb:cid:UUID:b3023475-09b4-4647-9d6d-6fc242822e68')/$links/ContentKeys HTTP/1.1
    Content-Type: application/json;odata=verbose
    Accept: application/json;odata=verbose
    DataServiceVersion: 3.0
    MaxDataServiceVersion: 3.0
    x-ms-version: 2.11
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=070500D0-F35C-4A5A-9249-485BBF4EC70B&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1334275521&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=GxdBb%2fmEyN7iHdNxbawawHRftLhPFFqxX1JZckuv3hY%3d
    Host: media.windows.net
    Content-Length: 113
    Expect: 100-continue

    { "uri": "https://media.windows.net/api/ContentKeys('nb%3Akid%3AUUID%3A32e6efaf-5fba-4538-b115-9d1cefe43510')"}

<span data-ttu-id="c471f-270">**HTTP-svar**</span><span class="sxs-lookup"><span data-stu-id="c471f-270">**HTTP Response**</span></span>

    GET https://media.windows.net/API/IngestManifests('nb:mid:UUID:5c77f186-414f-8b48-8231-17f9264e2048') HTTP/1.1
    Content-Type: application/json;odata=verbose
    Accept: application/json;odata=verbose
    DataServiceVersion: 3.0
    MaxDataServiceVersion: 3.0
    x-ms-version: 2.11
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=070500D0-F35C-4A5A-9249-485BBF4EC70B&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1334275521&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=GxdBb%2fmEyN7iHdNxbawawHRftLhPFFqxX1JZckuv3hY%3d
    Host: media.windows.net

## <a name="next-steps"></a><span data-ttu-id="c471f-271">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="c471f-271">Next steps</span></span>

<span data-ttu-id="c471f-272">Du kan nu koda överförda tillgångar.</span><span class="sxs-lookup"><span data-stu-id="c471f-272">You can now encode your uploaded assets.</span></span> <span data-ttu-id="c471f-273">Mer information finns i [Koda tillgångar](media-services-portal-encode.md).</span><span class="sxs-lookup"><span data-stu-id="c471f-273">For more information, see [Encode assets](media-services-portal-encode.md).</span></span>

<span data-ttu-id="c471f-274">Du kan också använda Azure Functions tootrigger ett kodningsjobb baserat på en fil som inkommer på hello konfigurerats behållare.</span><span class="sxs-lookup"><span data-stu-id="c471f-274">You can also use Azure Functions tootrigger an encoding job based on a file arriving in hello configured container.</span></span> <span data-ttu-id="c471f-275">Mer information finns i [det här exemplet](https://azure.microsoft.com/resources/samples/media-services-dotnet-functions-integration/ ).</span><span class="sxs-lookup"><span data-stu-id="c471f-275">For more information, see [this sample](https://azure.microsoft.com/resources/samples/media-services-dotnet-functions-integration/ ).</span></span>

## <a name="media-services-learning-paths"></a><span data-ttu-id="c471f-276">Sökvägar för Media Services-utbildning</span><span class="sxs-lookup"><span data-stu-id="c471f-276">Media Services learning paths</span></span>
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a><span data-ttu-id="c471f-277">Ge feedback</span><span class="sxs-lookup"><span data-stu-id="c471f-277">Provide feedback</span></span>
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

[How tooGet a Media Processor]: media-services-get-media-processor.md

