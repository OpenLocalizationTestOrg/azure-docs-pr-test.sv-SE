---
title: "Ladda upp filer till ett Media Services-konto med hjälp av REST | Microsoft Docs"
description: "Lär dig mer om att få medieinnehåll i Media Services genom att skapa och ladda upp tillgångar."
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
ms.openlocfilehash: 955356ffe6fc524c1528364add7e2c2a336137b7
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/29/2017
---
# <a name="upload-files-into-a-media-services-account-using-rest"></a><span data-ttu-id="1c7e1-103">Ladda upp filer till ett Media Services-konto med hjälp av REST</span><span class="sxs-lookup"><span data-stu-id="1c7e1-103">Upload files into a Media Services account using REST</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="1c7e1-104">.NET</span><span class="sxs-lookup"><span data-stu-id="1c7e1-104">.NET</span></span>](media-services-dotnet-upload-files.md)
> * [<span data-ttu-id="1c7e1-105">REST</span><span class="sxs-lookup"><span data-stu-id="1c7e1-105">REST</span></span>](media-services-rest-upload-files.md)
> * [<span data-ttu-id="1c7e1-106">Portal</span><span class="sxs-lookup"><span data-stu-id="1c7e1-106">Portal</span></span>](media-services-portal-upload-files.md)
> 
> 

<span data-ttu-id="1c7e1-107">I Media Services överför du dina digitala filer till en tillgång.</span><span class="sxs-lookup"><span data-stu-id="1c7e1-107">In Media Services, you upload your digital files into an asset.</span></span> <span data-ttu-id="1c7e1-108">Den [tillgången](https://docs.microsoft.com/rest/api/media/operations/asset) entitet kan innehålla video, ljud, bilder, miniatyrsamlingar, text spår och textning filer (och metadata om dessa filer.)  När filerna har överförts till tillgången, lagras innehållet på ett säkert sätt i molnet för ytterligare bearbetning och strömning.</span><span class="sxs-lookup"><span data-stu-id="1c7e1-108">The [Asset](https://docs.microsoft.com/rest/api/media/operations/asset) entity can contain video, audio, images, thumbnail collections, text tracks and closed caption files (and the metadata about these files.)  Once the files are uploaded into the asset, your content is stored securely in the cloud for further processing and streaming.</span></span> 

> [!NOTE]
> <span data-ttu-id="1c7e1-109">Följande gäller:</span><span class="sxs-lookup"><span data-stu-id="1c7e1-109">The following considerations apply:</span></span>
> 
> * <span data-ttu-id="1c7e1-110">Media Services använder värdet för egenskapen IAssetFile.Name när du skapar URL: er för strömning innehållet (till exempel http://{AMSAccount}.origin.mediaservices.windows.net/{GUID}/{IAssetFile.Name}/streamingParameters.) Därför tillåts procent-encoding inte.</span><span class="sxs-lookup"><span data-stu-id="1c7e1-110">Media Services uses the value of the IAssetFile.Name property when building URLs for the streaming content (for example, http://{AMSAccount}.origin.mediaservices.windows.net/{GUID}/{IAssetFile.Name}/streamingParameters.) For this reason, percent-encoding is not allowed.</span></span> <span data-ttu-id="1c7e1-111">Värdet för den **namn** egenskapen får inte ha något av följande [procent-encoding-reserverade tecken](http://en.wikipedia.org/wiki/Percent-encoding#Percent-encoding_reserved_characters):! *' ();: @& = + $, /? [] # % ”.</span><span class="sxs-lookup"><span data-stu-id="1c7e1-111">The value of the **Name** property cannot have any of the following [percent-encoding-reserved characters](http://en.wikipedia.org/wiki/Percent-encoding#Percent-encoding_reserved_characters): !*'();:@&=+$,/?%#[]".</span></span> <span data-ttu-id="1c7e1-112">Dessutom det kan bara finnas ett '.' för filnamnstillägget.</span><span class="sxs-lookup"><span data-stu-id="1c7e1-112">Also, there can only be one '.' for the file name extension.</span></span>
> * <span data-ttu-id="1c7e1-113">Längden på namnet får inte vara större än 260 tecken.</span><span class="sxs-lookup"><span data-stu-id="1c7e1-113">The length of the name should not be greater than 260 characters.</span></span>
> * <span data-ttu-id="1c7e1-114">Det finns en gräns för maximal filstorlek för bearbetning i Media Services.</span><span class="sxs-lookup"><span data-stu-id="1c7e1-114">There is a limit to the maximum file size supported for processing in Media Services.</span></span> <span data-ttu-id="1c7e1-115">Information om filstorleksbegränsningen finns i [det här](media-services-quotas-and-limitations.md) avsnittet.</span><span class="sxs-lookup"><span data-stu-id="1c7e1-115">Please see [this](media-services-quotas-and-limitations.md) topic for details about the file size limitation.</span></span>
> 

<span data-ttu-id="1c7e1-116">Det grundläggande arbetsflödet för uppladdning av tillgångar är uppdelad i följande avsnitt:</span><span class="sxs-lookup"><span data-stu-id="1c7e1-116">The basic workflow for uploading Assets is divided into the following sections:</span></span>

* <span data-ttu-id="1c7e1-117">Skapa en tillgång</span><span class="sxs-lookup"><span data-stu-id="1c7e1-117">Create an Asset</span></span>
* <span data-ttu-id="1c7e1-118">Kryptera en tillgång (valfritt)</span><span class="sxs-lookup"><span data-stu-id="1c7e1-118">Encrypt an Asset (Optional)</span></span>
* <span data-ttu-id="1c7e1-119">Överför en fil till blob storage</span><span class="sxs-lookup"><span data-stu-id="1c7e1-119">Upload a file to blob storage</span></span>

<span data-ttu-id="1c7e1-120">AMS kan du överföra tillgångar gruppvis.</span><span class="sxs-lookup"><span data-stu-id="1c7e1-120">AMS also enables you to upload assets in bulk.</span></span> <span data-ttu-id="1c7e1-121">Mer information finns i [det här](media-services-rest-upload-files.md#upload_in_bulk) avsnittet.</span><span class="sxs-lookup"><span data-stu-id="1c7e1-121">For more information, see [this](media-services-rest-upload-files.md#upload_in_bulk) section.</span></span>

> [!NOTE]
> <span data-ttu-id="1c7e1-122">Vid åtkomst till entiteter i Media Services måste du ange specifika namn på huvudfält och värden i HTTP-begäranden.</span><span class="sxs-lookup"><span data-stu-id="1c7e1-122">When accessing entities in Media Services, you must set specific header fields and values in your HTTP requests.</span></span> <span data-ttu-id="1c7e1-123">Mer information finns i [installationsprogrammet för Media Services REST API-utveckling](media-services-rest-how-to-use.md).</span><span class="sxs-lookup"><span data-stu-id="1c7e1-123">For more information, see [Setup for Media Services REST API Development](media-services-rest-how-to-use.md).</span></span>
> 

## <a name="connect-to-media-services"></a><span data-ttu-id="1c7e1-124">Ansluta till Media Services</span><span class="sxs-lookup"><span data-stu-id="1c7e1-124">Connect to Media Services</span></span>

<span data-ttu-id="1c7e1-125">Information om hur du ansluter till AMS API: et finns [åtkomst till Azure Media Services-API med Azure AD authentication](media-services-use-aad-auth-to-access-ams-api.md).</span><span class="sxs-lookup"><span data-stu-id="1c7e1-125">For information on how to connect to the AMS API, see [Access the Azure Media Services API with Azure AD authentication](media-services-use-aad-auth-to-access-ams-api.md).</span></span> 

>[!NOTE]
><span data-ttu-id="1c7e1-126">När du har anslutit till https://media.windows.net, får du en 301 omdirigering att ange en annan Media Services-URI.</span><span class="sxs-lookup"><span data-stu-id="1c7e1-126">After successfully connecting to https://media.windows.net, you will receive a 301 redirect specifying another Media Services URI.</span></span> <span data-ttu-id="1c7e1-127">Du måste göra följande anrop till en ny URI.</span><span class="sxs-lookup"><span data-stu-id="1c7e1-127">You must make subsequent calls to the new URI.</span></span>

## <a name="upload-assets"></a><span data-ttu-id="1c7e1-128">Överför tillgångar</span><span class="sxs-lookup"><span data-stu-id="1c7e1-128">Upload assets</span></span>

### <a name="create-an-asset"></a><span data-ttu-id="1c7e1-129">Skapa en tillgång</span><span class="sxs-lookup"><span data-stu-id="1c7e1-129">Create an asset</span></span>

<span data-ttu-id="1c7e1-130">En tillgång är en behållare för flera typer eller uppsättningar med objekt i Media Services, inklusive video, ljud, bilder, miniatyrsamlingar, textspår och filer med dold textning.</span><span class="sxs-lookup"><span data-stu-id="1c7e1-130">An asset is a container for multiple types or sets of objects in Media Services, including video, audio, images, thumbnail collections, text tracks, and closed caption files.</span></span> <span data-ttu-id="1c7e1-131">I REST-API kräver skapa en tillgång POST-begäran skickades till Media Services och placerar egenskapsinformation om din tillgång i begärandetexten.</span><span class="sxs-lookup"><span data-stu-id="1c7e1-131">In the REST API, creating an Asset requires sending POST request to Media Services and placing any property information about your asset in the request body.</span></span>

<span data-ttu-id="1c7e1-132">En av de egenskaper som du kan ange när du skapar en tillgång är **alternativ**.</span><span class="sxs-lookup"><span data-stu-id="1c7e1-132">One of the properties that you can specify when creating an asset is **Options**.</span></span> <span data-ttu-id="1c7e1-133">**Alternativ för** är ett uppräkningsvärde som beskriver krypteringsalternativen som du kan skapa en tillgång med.</span><span class="sxs-lookup"><span data-stu-id="1c7e1-133">**Options** is an enumeration value that describes the encryption options that an Asset can be created with.</span></span> <span data-ttu-id="1c7e1-134">Ett giltigt värde är ett av värdena i listan nedan, inte en kombination av värden.</span><span class="sxs-lookup"><span data-stu-id="1c7e1-134">A valid value is one of the values from the list below, not a combination of values.</span></span> 

* <span data-ttu-id="1c7e1-135">**Ingen** = **0**: Ingen kryptering används.</span><span class="sxs-lookup"><span data-stu-id="1c7e1-135">**None** = **0**: No encryption will be used.</span></span> <span data-ttu-id="1c7e1-136">Detta är standardvärdet.</span><span class="sxs-lookup"><span data-stu-id="1c7e1-136">This is the default value.</span></span> <span data-ttu-id="1c7e1-137">Observera att när du använder det här alternativet om ditt innehåll inte skyddas under överföringen eller i vila i lagring.</span><span class="sxs-lookup"><span data-stu-id="1c7e1-137">Note that when using this option your content is not protected in transit or at rest in storage.</span></span>
    <span data-ttu-id="1c7e1-138">Om du planerar att leverera en MP4 med progressivt nedladdning ska du använda det här alternativet.</span><span class="sxs-lookup"><span data-stu-id="1c7e1-138">If you plan to deliver an MP4 using progressive download, use this option.</span></span> 
* <span data-ttu-id="1c7e1-139">**StorageEncrypted** = **1**: Ange om du vill använda för dina filer som ska krypteras med AES 256-bitarskryptering för överföring och lagring.</span><span class="sxs-lookup"><span data-stu-id="1c7e1-139">**StorageEncrypted** = **1**: Specify if you want for your files to be encrypted with AES-256 bit encryption for upload and storage.</span></span>
  
    <span data-ttu-id="1c7e1-140">Om tillgången är lagringskrypterad, måste du konfigurera principen för tillgångsleverans.</span><span class="sxs-lookup"><span data-stu-id="1c7e1-140">If your asset is storage encrypted, you must configure asset delivery policy.</span></span> <span data-ttu-id="1c7e1-141">Mer information finns i [konfigurera tillgångsleveransprincip](media-services-rest-configure-asset-delivery-policy.md).</span><span class="sxs-lookup"><span data-stu-id="1c7e1-141">For more information see [Configuring asset delivery policy](media-services-rest-configure-asset-delivery-policy.md).</span></span>
* <span data-ttu-id="1c7e1-142">**CommonEncryptionProtected** = **2**: Ange om du överför filer som skyddas med en gemensam krypteringsmetod (till exempel PlayReady).</span><span class="sxs-lookup"><span data-stu-id="1c7e1-142">**CommonEncryptionProtected** = **2**: Specify if you are uploading files protected with a common encryption method (such as PlayReady).</span></span> 
* <span data-ttu-id="1c7e1-143">**EnvelopeEncryptionProtected** = **4**: Ange om du överför HLS som krypterats med AES-filer.</span><span class="sxs-lookup"><span data-stu-id="1c7e1-143">**EnvelopeEncryptionProtected** = **4**: Specify if you are uploading HLS encrypted with AES files.</span></span> <span data-ttu-id="1c7e1-144">Observera att filerna måste ha kodats och krypterats av Transform Manager.</span><span class="sxs-lookup"><span data-stu-id="1c7e1-144">Note that the files must have been encoded and encrypted by Transform Manager.</span></span>

> [!NOTE]
> <span data-ttu-id="1c7e1-145">Om din tillgång kommer att använda kryptering, måste du skapa en **ContentKey** och länka det till din tillgång enligt beskrivningen i följande avsnitt:[hur du skapar en ContentKey](media-services-rest-create-contentkey.md).</span><span class="sxs-lookup"><span data-stu-id="1c7e1-145">If your asset will use encryption, you must create a **ContentKey** and link it to your asset as described in the following topic:[How to create a ContentKey](media-services-rest-create-contentkey.md).</span></span> <span data-ttu-id="1c7e1-146">Observera att när du har överfört filerna till tillgången du behöver uppdatera egenskaper för kryptering på den **AssetFile** entitet med de värden som du har fått under den **tillgången** kryptering.</span><span class="sxs-lookup"><span data-stu-id="1c7e1-146">Note that after you upload the files into the asset, you need to update the encryption properties on the **AssetFile** entity with the values you got during the **Asset** encryption.</span></span> <span data-ttu-id="1c7e1-147">Göra det med hjälp av den **sammanfoga** HTTP-begäran.</span><span class="sxs-lookup"><span data-stu-id="1c7e1-147">Do it by using the **MERGE** HTTP request.</span></span> 
> 
> 

<span data-ttu-id="1c7e1-148">I följande exempel visas hur du skapar en tillgång.</span><span class="sxs-lookup"><span data-stu-id="1c7e1-148">The following example shows how to create an asset.</span></span>

<span data-ttu-id="1c7e1-149">**HTTP-begäran**</span><span class="sxs-lookup"><span data-stu-id="1c7e1-149">**HTTP Request**</span></span>

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

<span data-ttu-id="1c7e1-150">**HTTP-svar**</span><span class="sxs-lookup"><span data-stu-id="1c7e1-150">**HTTP Response**</span></span>

<span data-ttu-id="1c7e1-151">Om det lyckas, returneras följande:</span><span class="sxs-lookup"><span data-stu-id="1c7e1-151">If successful, the following is returned:</span></span>

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

### <a name="create-an-assetfile"></a><span data-ttu-id="1c7e1-152">Skapa en AssetFile</span><span class="sxs-lookup"><span data-stu-id="1c7e1-152">Create an AssetFile</span></span>
<span data-ttu-id="1c7e1-153">Den [AssetFile](https://docs.microsoft.com/rest/api/media/operations/assetfile) entiteten representerar en video eller ljud-fil som lagras i en blob-behållare.</span><span class="sxs-lookup"><span data-stu-id="1c7e1-153">The [AssetFile](https://docs.microsoft.com/rest/api/media/operations/assetfile) entity represents a video or audio file that is stored in a blob container.</span></span> <span data-ttu-id="1c7e1-154">En resursfil är alltid associerat med en tillgång och en tillgång kan innehålla en eller många tillgångsfiler.</span><span class="sxs-lookup"><span data-stu-id="1c7e1-154">An asset file is always associated with an asset, and an asset may contain one or many asset files.</span></span> <span data-ttu-id="1c7e1-155">Media Services Encoder uppgiften misslyckas om objekttypen tillgången filen inte är associerad med en digital fil i en blob-behållaren.</span><span class="sxs-lookup"><span data-stu-id="1c7e1-155">The Media Services Encoder task fails if an asset file object is not associated with a digital file in a blob container.</span></span>

<span data-ttu-id="1c7e1-156">Observera att den **AssetFile** instansen och den faktiska mediefilen är två distinkta objekt.</span><span class="sxs-lookup"><span data-stu-id="1c7e1-156">Note that the **AssetFile** instance and the actual media file are two distinct objects.</span></span> <span data-ttu-id="1c7e1-157">AssetFile-instans innehåller metadata om filen media när mediefilen innehåller faktiskt medieinnehåll.</span><span class="sxs-lookup"><span data-stu-id="1c7e1-157">The AssetFile instance contains metadata about the media file, while the media file contains the actual media content.</span></span>

<span data-ttu-id="1c7e1-158">När du har överfört din digitala media-fil till en blobbbehållare, ska du använda den **sammanfoga** HTTP-begäran om uppdatering av AssetFile med information om media-fil (som visas senare i avsnittet).</span><span class="sxs-lookup"><span data-stu-id="1c7e1-158">After you upload your digital media file into a blob container, you will use the **MERGE** HTTP request to update the AssetFile with information about your media file (as shown later in the topic).</span></span> 

<span data-ttu-id="1c7e1-159">**HTTP-begäran**</span><span class="sxs-lookup"><span data-stu-id="1c7e1-159">**HTTP Request**</span></span>

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

<span data-ttu-id="1c7e1-160">**HTTP-svar**</span><span class="sxs-lookup"><span data-stu-id="1c7e1-160">**HTTP Response**</span></span>

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

### <a name="creating-the-accesspolicy-with-write-permission"></a><span data-ttu-id="1c7e1-161">Skapar AccessPolicy med behörighet att skriva.</span><span class="sxs-lookup"><span data-stu-id="1c7e1-161">Creating the AccessPolicy with write permission.</span></span>

>[!NOTE]
><span data-ttu-id="1c7e1-162">Det finns en gräns på 1 000 000 principer för olika AMS-principer (till exempel för positionerarprincipen eller ContentKeyAuthorizationPolicy).</span><span class="sxs-lookup"><span data-stu-id="1c7e1-162">There is a limit of 1,000,000 policies for different AMS policies (for example, for Locator policy or ContentKeyAuthorizationPolicy).</span></span> <span data-ttu-id="1c7e1-163">Du bör använda samma princip-ID om du alltid använder samma dagar/åtkomstbehörigheter, till exempel principer för positionerare som är avsedda att vara på plats under en längre tid (icke-överföringsprinciper).</span><span class="sxs-lookup"><span data-stu-id="1c7e1-163">You should use the same policy ID if you are always using the same days / access permissions, for example, policies for locators that are intended to remain in place for a long time (non-upload policies).</span></span> <span data-ttu-id="1c7e1-164">Mer information finns i [detta](media-services-dotnet-manage-entities.md#limit-access-policies) avsnitt.</span><span class="sxs-lookup"><span data-stu-id="1c7e1-164">For more information, see [this](media-services-dotnet-manage-entities.md#limit-access-policies) topic.</span></span>

<span data-ttu-id="1c7e1-165">Innan du laddar upp filer i blob-lagring, ange principen rättigheter för att skriva till en tillgång.</span><span class="sxs-lookup"><span data-stu-id="1c7e1-165">Before uploading any files into blob storage, set the access policy rights for writing to an asset.</span></span> <span data-ttu-id="1c7e1-166">För att göra det efter en HTTP-begäran till AccessPolicies entitetsuppsättning.</span><span class="sxs-lookup"><span data-stu-id="1c7e1-166">To do that, POST an HTTP request to the AccessPolicies entity set.</span></span> <span data-ttu-id="1c7e1-167">Ange ett värde för DurationInMinutes när de skapas eller du får felmeddelandet 500 intern Server tillbaka som svar.</span><span class="sxs-lookup"><span data-stu-id="1c7e1-167">Define a DurationInMinutes value upon creation or you will receive a 500 Internal Server error message back in response.</span></span> <span data-ttu-id="1c7e1-168">Mer information om AccessPolicies finns [AccessPolicy](https://docs.microsoft.com/rest/api/media/operations/accesspolicy).</span><span class="sxs-lookup"><span data-stu-id="1c7e1-168">For more information on AccessPolicies, see [AccessPolicy](https://docs.microsoft.com/rest/api/media/operations/accesspolicy).</span></span>

<span data-ttu-id="1c7e1-169">I följande exempel visas hur du skapar en AccessPolicy:</span><span class="sxs-lookup"><span data-stu-id="1c7e1-169">The following example shows how to create an AccessPolicy:</span></span>

<span data-ttu-id="1c7e1-170">**HTTP-begäran**</span><span class="sxs-lookup"><span data-stu-id="1c7e1-170">**HTTP Request**</span></span>

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

<span data-ttu-id="1c7e1-171">**HTTP-begäran**</span><span class="sxs-lookup"><span data-stu-id="1c7e1-171">**HTTP Request**</span></span>

    If successful, the following response is returned:

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

### <a name="get-the-upload-url"></a><span data-ttu-id="1c7e1-172">Hämta URL</span><span class="sxs-lookup"><span data-stu-id="1c7e1-172">Get the Upload URL</span></span>
<span data-ttu-id="1c7e1-173">Skapa en SAS-lokaliserare för att ta emot den faktiska URL.</span><span class="sxs-lookup"><span data-stu-id="1c7e1-173">To receive the actual upload URL, create a SAS Locator.</span></span> <span data-ttu-id="1c7e1-174">Lokaliserare definiera start- och typ av anslutningens slutpunkt för klienter som vill komma åt filer i en tillgång.</span><span class="sxs-lookup"><span data-stu-id="1c7e1-174">Locators define the start time and type of connection endpoint for clients that want to access Files in an Asset.</span></span> <span data-ttu-id="1c7e1-175">Du kan skapa flera lokaliserare entiteter för ett angivet AccessPolicy och tillgångshantering par att hantera olika klientbegäranden och behov.</span><span class="sxs-lookup"><span data-stu-id="1c7e1-175">You can create multiple Locator entities for a given AccessPolicy and Asset pair to handle different client requests and needs.</span></span> <span data-ttu-id="1c7e1-176">Var och en av dessa lokaliserare använda StartTime-värdet plus DurationInMinutes värdet för AccessPolicy för att avgöra hur lång tid som en URL som kan användas.</span><span class="sxs-lookup"><span data-stu-id="1c7e1-176">Each of these Locators use the StartTime value plus the DurationInMinutes value of the AccessPolicy to determine the length of time a URL can be used.</span></span> <span data-ttu-id="1c7e1-177">Mer information finns i [lokaliserare](https://docs.microsoft.com/rest/api/media/operations/locator).</span><span class="sxs-lookup"><span data-stu-id="1c7e1-177">For more information, see [Locator](https://docs.microsoft.com/rest/api/media/operations/locator).</span></span>

<span data-ttu-id="1c7e1-178">En SAS-URL har följande format:</span><span class="sxs-lookup"><span data-stu-id="1c7e1-178">A SAS URL has the following format:</span></span>

    {https://myaccount.blob.core.windows.net}/{asset name}/{video file name}?{SAS signature}

<span data-ttu-id="1c7e1-179">Vissa förutsättningar gäller:</span><span class="sxs-lookup"><span data-stu-id="1c7e1-179">Some considerations apply:</span></span>

* <span data-ttu-id="1c7e1-180">Du kan inte ha fler än fem unika lokaliserare som är associerade med en viss resurs i taget.</span><span class="sxs-lookup"><span data-stu-id="1c7e1-180">You cannot have more than five unique Locators associated with a given Asset at one time.</span></span> <span data-ttu-id="1c7e1-181">Mer information finns i lokaliserare.</span><span class="sxs-lookup"><span data-stu-id="1c7e1-181">For more information, see Locator.</span></span>
* <span data-ttu-id="1c7e1-182">Om du behöver överföra filer omedelbart ska du ange StartTime-värdet till fem minuter före aktuell tid.</span><span class="sxs-lookup"><span data-stu-id="1c7e1-182">If you need to upload your files immediately, you should set your StartTime value to five minutes before the current time.</span></span> <span data-ttu-id="1c7e1-183">Det beror på att det kan finnas klockan skeva mellan klientdatorn och Media Services.</span><span class="sxs-lookup"><span data-stu-id="1c7e1-183">This is because there may be clock skew between your client machine and Media Services.</span></span> <span data-ttu-id="1c7e1-184">StartTime-värdet måste också vara i följande DateTime-format: ÅÅÅÅ-MM-ddTHH (till exempel ”2014-05-23T17:53:50Z”).</span><span class="sxs-lookup"><span data-stu-id="1c7e1-184">Also, your StartTime value must be in the following DateTime format: YYYY-MM-DDTHH:mm:ssZ (for example, "2014-05-23T17:53:50Z").</span></span>    
* <span data-ttu-id="1c7e1-185">Det kan finnas en 30-40 andra fördröjning efter en positionerare skapas när den är tillgänglig för användning.</span><span class="sxs-lookup"><span data-stu-id="1c7e1-185">There may be a 30-40 second delay after a Locator is created to when it is available for use.</span></span> <span data-ttu-id="1c7e1-186">Det här problemet gäller både SAS-URL och ursprung lokaliserare.</span><span class="sxs-lookup"><span data-stu-id="1c7e1-186">This issue applies to both SAS URL and Origin Locators.</span></span>

<span data-ttu-id="1c7e1-187">I följande exempel visas hur du skapar en URL SAS-lokaliserare som definieras av egenskapen Type i begärandetexten (”1” för en SAS-lokaliserare) och ”2” för en positionerare för ursprung på begäran.</span><span class="sxs-lookup"><span data-stu-id="1c7e1-187">The following example shows how to create a SAS URL Locator, as defined by the Type property in the request body ("1" for a SAS locator and "2" for an On-Demand origin locator).</span></span> <span data-ttu-id="1c7e1-188">Den **sökväg** returnerade-egenskapen innehåller den URL som du måste använda för att överföra din fil.</span><span class="sxs-lookup"><span data-stu-id="1c7e1-188">The **Path** property returned contains the URL that you must use to upload your file.</span></span>

<span data-ttu-id="1c7e1-189">**HTTP-begäran**</span><span class="sxs-lookup"><span data-stu-id="1c7e1-189">**HTTP Request**</span></span>

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

<span data-ttu-id="1c7e1-190">**HTTP-svar**</span><span class="sxs-lookup"><span data-stu-id="1c7e1-190">**HTTP Response**</span></span>

<span data-ttu-id="1c7e1-191">Om det lyckas, returneras följande svar:</span><span class="sxs-lookup"><span data-stu-id="1c7e1-191">If successful, the following response is returned:</span></span>

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

### <a name="upload-a-file-into-a-blob-storage-container"></a><span data-ttu-id="1c7e1-192">Ladda upp en fil till en behållare för blob storage</span><span class="sxs-lookup"><span data-stu-id="1c7e1-192">Upload a file into a blob storage container</span></span>
<span data-ttu-id="1c7e1-193">När du har AccessPolicy och lokaliserare ange har den faktiska filen överförts till en Azure Blob Storage-behållare med hjälp av Azure Storage REST-API: er.</span><span class="sxs-lookup"><span data-stu-id="1c7e1-193">Once you have the AccessPolicy and Locator set, the actual file is uploaded to an Azure Blob Storage container using the Azure Storage REST APIs.</span></span> <span data-ttu-id="1c7e1-194">Du måste överföra filer som blockblobar.</span><span class="sxs-lookup"><span data-stu-id="1c7e1-194">You must upload the files as block blobs.</span></span> <span data-ttu-id="1c7e1-195">Azure Media Services stöder inte sidblobar.</span><span class="sxs-lookup"><span data-stu-id="1c7e1-195">Page blobs are not supported by Azure Media Services.</span></span>  

> [!NOTE]
> <span data-ttu-id="1c7e1-196">Du måste lägga till filnamnet för den fil som du vill överföra till positioneraren **sökväg** värde som erhölls i föregående avsnitt.</span><span class="sxs-lookup"><span data-stu-id="1c7e1-196">You must add the file name for the file you want to upload to the Locator **Path** value received in the previous section.</span></span> <span data-ttu-id="1c7e1-197">Till exempel https://storagetestaccount001.blob.core.windows.net/asset-e7b02da4-5a69-40e7-a8db-e8f4f697aac0/BigBuckBunny.mp4?</span><span class="sxs-lookup"><span data-stu-id="1c7e1-197">For example, https://storagetestaccount001.blob.core.windows.net/asset-e7b02da4-5a69-40e7-a8db-e8f4f697aac0/BigBuckBunny.mp4?</span></span> <span data-ttu-id="1c7e1-198">.</span><span class="sxs-lookup"><span data-stu-id="1c7e1-198">.</span></span> <span data-ttu-id="1c7e1-199">.</span><span class="sxs-lookup"><span data-stu-id="1c7e1-199">.</span></span> <span data-ttu-id="1c7e1-200">.</span><span class="sxs-lookup"><span data-stu-id="1c7e1-200">.</span></span> 
> 
> 

<span data-ttu-id="1c7e1-201">Mer information om hur du arbetar med Azure storage blobs finns [REST-API för Blob-tjänsten](https://docs.microsoft.com/rest/api/storageservices/Blob-Service-REST-API).</span><span class="sxs-lookup"><span data-stu-id="1c7e1-201">For more information on working with Azure storage blobs, see [Blob Service REST API](https://docs.microsoft.com/rest/api/storageservices/Blob-Service-REST-API).</span></span>

### <a name="update-the-assetfile"></a><span data-ttu-id="1c7e1-202">Uppdatera AssetFile</span><span class="sxs-lookup"><span data-stu-id="1c7e1-202">Update the AssetFile</span></span>
<span data-ttu-id="1c7e1-203">Nu när du har överfört din fil, uppdatera informationen om FileAsset storlek (och andra).</span><span class="sxs-lookup"><span data-stu-id="1c7e1-203">Now that you've uploaded your file, update the FileAsset size (and other) information.</span></span> <span data-ttu-id="1c7e1-204">Exempel:</span><span class="sxs-lookup"><span data-stu-id="1c7e1-204">For example:</span></span>

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


<span data-ttu-id="1c7e1-205">**HTTP-svar**</span><span class="sxs-lookup"><span data-stu-id="1c7e1-205">**HTTP Response**</span></span>

<span data-ttu-id="1c7e1-206">Om det lyckas följande returneras: HTTP/1.1 204 Nej innehåll</span><span class="sxs-lookup"><span data-stu-id="1c7e1-206">If successful, the following is returned: HTTP/1.1 204 No Content</span></span>

### <a name="delete-the-locator-and-accesspolicy"></a><span data-ttu-id="1c7e1-207">Ta bort lokaliserare och AccessPolicy</span><span class="sxs-lookup"><span data-stu-id="1c7e1-207">Delete the Locator and AccessPolicy</span></span>
<span data-ttu-id="1c7e1-208">**HTTP-begäran**</span><span class="sxs-lookup"><span data-stu-id="1c7e1-208">**HTTP Request**</span></span>

    DELETE https://media.windows.net/api/Locators('nb%3Alid%3AUUID%3Aaf57bdd8-6751-4e84-b403-f3c140444b54') HTTP/1.1
    DataServiceVersion: 1.0;NetFx
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json
    Accept-Charset: UTF-8
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=amstestaccount001&urn%3aSubscriptionId=z7f09258-6753-2233-b1ae-193798e2c9d8&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1421662918&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=utmoXXbm9Q7j4tW1yJuMVA3egRiQy5FPygwadkmPeaY%3d
    x-ms-version: 2.11
    Host: media.windows.net

<span data-ttu-id="1c7e1-209">**HTTP-svar**</span><span class="sxs-lookup"><span data-stu-id="1c7e1-209">**HTTP Response**</span></span>

<span data-ttu-id="1c7e1-210">Om det lyckas, returneras följande:</span><span class="sxs-lookup"><span data-stu-id="1c7e1-210">If successful, the following is returned:</span></span>

    HTTP/1.1 204 No Content 
    ...

<span data-ttu-id="1c7e1-211">**HTTP-begäran**</span><span class="sxs-lookup"><span data-stu-id="1c7e1-211">**HTTP Request**</span></span>

    DELETE https://media.windows.net/api/AccessPolicies('nb%3Apid%3AUUID%3Abe0ac48d-af7d-4877-9d60-1805d68bffae') HTTP/1.1
    DataServiceVersion: 1.0;NetFx
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json
    Accept-Charset: UTF-8
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=amstestaccount001&urn%3aSubscriptionId=z7f09258-6753-2233-b1ae-193798e2c9d8&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1421662918&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=utmoXXbm9Q7j4tW1yJuMVA3egRiQy5FPygwadkmPeaY%3d
    x-ms-version: 2.11
    Host: media.windows.net

<span data-ttu-id="1c7e1-212">**HTTP-svar**</span><span class="sxs-lookup"><span data-stu-id="1c7e1-212">**HTTP Response**</span></span>

<span data-ttu-id="1c7e1-213">Om det lyckas, returneras följande:</span><span class="sxs-lookup"><span data-stu-id="1c7e1-213">If successful, the following is returned:</span></span>

    HTTP/1.1 204 No Content 
    ...

## <span data-ttu-id="1c7e1-214"><a id="upload_in_bulk"></a>Överför tillgångar i grupp</span><span class="sxs-lookup"><span data-stu-id="1c7e1-214"><a id="upload_in_bulk"></a>Upload assets in bulk</span></span>
### <a name="create-the-ingestmanifest"></a><span data-ttu-id="1c7e1-215">Skapa IngestManifest</span><span class="sxs-lookup"><span data-stu-id="1c7e1-215">Create the IngestManifest</span></span>
<span data-ttu-id="1c7e1-216">IngestManifest är en behållare för en uppsättning tillgångar, tillgångsfiler och statistik information som kan användas för att fastställa förloppet för bulk vill föra in för.</span><span class="sxs-lookup"><span data-stu-id="1c7e1-216">The IngestManifest is a container for a set of assets, asset files, and statistic information that can be used to determine the progress of bulk ingesting for the set.</span></span>

<span data-ttu-id="1c7e1-217">**HTTP-begäran**</span><span class="sxs-lookup"><span data-stu-id="1c7e1-217">**HTTP Request**</span></span>

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

### <a name="create-assets"></a><span data-ttu-id="1c7e1-218">Skapa tillgångar</span><span class="sxs-lookup"><span data-stu-id="1c7e1-218">Create assets</span></span>
<span data-ttu-id="1c7e1-219">Innan du skapar IngestManifestAsset, måste du skapa den tillgång som ska utföras samtidigt vill föra in.</span><span class="sxs-lookup"><span data-stu-id="1c7e1-219">Before creating the IngestManifestAsset, you need to create the Asset that will be completed using bulk ingesting.</span></span> <span data-ttu-id="1c7e1-220">En tillgång är en behållare för flera typer eller uppsättningar med objekt i Media Services, inklusive video, ljud, bilder, miniatyrsamlingar, textspår och filer med dold textning.</span><span class="sxs-lookup"><span data-stu-id="1c7e1-220">An asset is a container for multiple types or sets of objects in Media Services, including video, audio, images, thumbnail collections, text tracks, and closed caption files.</span></span> <span data-ttu-id="1c7e1-221">I REST-API kräver skapa en tillgång en HTTP POST-begäran skickades till Microsoft Azure Media Services och placerar egenskapsinformation om din tillgång i begärandetexten. I det här exemplet skapas tillgången med alternativet StorageEncrption(1) ingår i begärandetexten.</span><span class="sxs-lookup"><span data-stu-id="1c7e1-221">In the REST API, creating an Asset requires sending a HTTP POST request to Microsoft Azure Media Services and placing any property information about your asset in the request body.In this example, the Asset is created using the StorageEncrption(1) option included with the request body.</span></span>

<span data-ttu-id="1c7e1-222">**HTTP-svar**</span><span class="sxs-lookup"><span data-stu-id="1c7e1-222">**HTTP Response**</span></span>

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

### <a name="create-the-ingestmanifestassets"></a><span data-ttu-id="1c7e1-223">Skapa IngestManifestAssets</span><span class="sxs-lookup"><span data-stu-id="1c7e1-223">Create the IngestManifestAssets</span></span>
<span data-ttu-id="1c7e1-224">IngestManifestAssets representerar tillgångar i en IngestManifest som används med samtidigt vill föra in.</span><span class="sxs-lookup"><span data-stu-id="1c7e1-224">IngestManifestAssets represent Assets within an IngestManifest that are used with bulk ingesting.</span></span> <span data-ttu-id="1c7e1-225">Den huvudsakligen länka tillgången till manifestet.</span><span class="sxs-lookup"><span data-stu-id="1c7e1-225">The basically link the asset to the manifest.</span></span> <span data-ttu-id="1c7e1-226">Azure Media Services bevakar internt för filöverföringen baserat på IngestManifestFiles samlingen som är kopplad till IngestManifestAsset.</span><span class="sxs-lookup"><span data-stu-id="1c7e1-226">Azure Media Services watches internally for the file upload based on IngestManifestFiles collection associated to the IngestManifestAsset.</span></span> <span data-ttu-id="1c7e1-227">När filerna har överförts är tillgången klar.</span><span class="sxs-lookup"><span data-stu-id="1c7e1-227">Once these files are uploaded, the asset is completed.</span></span> <span data-ttu-id="1c7e1-228">Du kan skapa en ny IngestManifestAsset med en HTTP POST-begäran.</span><span class="sxs-lookup"><span data-stu-id="1c7e1-228">You can create a new IngestManifestAsset with a HTTP POST request.</span></span> <span data-ttu-id="1c7e1-229">Inkludera IngestManifest-Id och tillgångsinformation Id som IngestManifestAsset bör länka samman för att föra in bulk i frågans brödtext.</span><span class="sxs-lookup"><span data-stu-id="1c7e1-229">In the request body, include the IngestManifest Id and the Asset Id that the IngestManifestAsset should link together for bulk ingesting.</span></span>

<span data-ttu-id="1c7e1-230">**HTTP-svar**</span><span class="sxs-lookup"><span data-stu-id="1c7e1-230">**HTTP Response**</span></span>

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


### <a name="create-the-ingestmanifestfiles-for-each-asset"></a><span data-ttu-id="1c7e1-231">Skapa IngestManifestFiles för varje tillgång</span><span class="sxs-lookup"><span data-stu-id="1c7e1-231">Create the IngestManifestFiles for each Asset</span></span>
<span data-ttu-id="1c7e1-232">En IngestManifestFile representerar ett faktiska video eller ljud blob-objekt som överförs som en del av bulk vill föra in för en tillgång.</span><span class="sxs-lookup"><span data-stu-id="1c7e1-232">An IngestManifestFile represents an actual video or audio blob object that will be uploaded as part of bulk ingesting for an asset.</span></span> <span data-ttu-id="1c7e1-233">Kryptering relaterade egenskaper krävs inte om inte tillgången använder ett krypteringsalternativ.</span><span class="sxs-lookup"><span data-stu-id="1c7e1-233">Encryption related properties are not required unless the asset is using an encryption option.</span></span> <span data-ttu-id="1c7e1-234">Exemplet i det här avsnittet visar skapar en IngestManifestFile som använder StorageEncryption för tillgången skapade tidigare.</span><span class="sxs-lookup"><span data-stu-id="1c7e1-234">The example used in this section demonstrates creating an IngestManifestFile that uses StorageEncryption for the Asset previously created.</span></span>

<span data-ttu-id="1c7e1-235">**HTTP-svar**</span><span class="sxs-lookup"><span data-stu-id="1c7e1-235">**HTTP Response**</span></span>

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

### <a name="upload-the-files-to-blob-storage"></a><span data-ttu-id="1c7e1-236">Överföra filer till Blob Storage</span><span class="sxs-lookup"><span data-stu-id="1c7e1-236">Upload the Files to Blob Storage</span></span>
<span data-ttu-id="1c7e1-237">Du kan använda en hög hastighet klientprogrammet kan överföra tillgångsfiler till blob storage-behållare Uri som anges av egenskapen BlobStorageUriForUpload för IngestManifest.</span><span class="sxs-lookup"><span data-stu-id="1c7e1-237">You can use any high speed client application capable of uploading the asset files to the blob storage container Uri provided by the BlobStorageUriForUpload property of the IngestManifest.</span></span> <span data-ttu-id="1c7e1-238">En tjänst för överföringen av viktiga hög hastighet är [Aspera på begäran för Azure-programmet](http://go.microsoft.com/fwlink/?LinkId=272001).</span><span class="sxs-lookup"><span data-stu-id="1c7e1-238">One notable high speed upload service is [Aspera On Demand for Azure Application](http://go.microsoft.com/fwlink/?LinkId=272001).</span></span>

### <a name="monitor-bulk-ingest-progress"></a><span data-ttu-id="1c7e1-239">Övervakaren Bulk mata in pågår</span><span class="sxs-lookup"><span data-stu-id="1c7e1-239">Monitor Bulk Ingest Progress</span></span>
<span data-ttu-id="1c7e1-240">Du kan övervaka förloppet för bulk vill föra in åtgärder för en IngestManifest genom att avsöka egenskapen statistik för IngestManifest.</span><span class="sxs-lookup"><span data-stu-id="1c7e1-240">You can monitor the progress of bulk ingesting operations for an IngestManifest by polling the Statistics property of the IngestManifest.</span></span> <span data-ttu-id="1c7e1-241">Att egenskapen är en komplex typ [IngestManifestStatistics](https://docs.microsoft.com/rest/api/media/operations/ingestmanifeststatistics).</span><span class="sxs-lookup"><span data-stu-id="1c7e1-241">That property is a complex type, [IngestManifestStatistics](https://docs.microsoft.com/rest/api/media/operations/ingestmanifeststatistics).</span></span> <span data-ttu-id="1c7e1-242">Om du vill söka egenskapen statistik skicka en HTTP GET-begäran skickas IngestManifest Id.</span><span class="sxs-lookup"><span data-stu-id="1c7e1-242">To poll the Statistics property, submit a HTTP GET request passing the IngestManifest Id.</span></span>

## <a name="create-contentkeys-used-for-encryption"></a><span data-ttu-id="1c7e1-243">Skapa ContentKeys som används för kryptering</span><span class="sxs-lookup"><span data-stu-id="1c7e1-243">Create ContentKeys used for encryption</span></span>
<span data-ttu-id="1c7e1-244">Om din tillgång kommer att använda kryptering, måste du skapa ContentKey som ska användas för kryptering innan du skapar tillgångsfiler.</span><span class="sxs-lookup"><span data-stu-id="1c7e1-244">If your asset will use encryption, you must create the ContentKey to be used for encryption before creating the asset files.</span></span> <span data-ttu-id="1c7e1-245">Följande egenskaper ska tas med i begärandetexten för storage kryptering.</span><span class="sxs-lookup"><span data-stu-id="1c7e1-245">For storage encryption, the following properties should be included in the request body.</span></span>

| <span data-ttu-id="1c7e1-246">Egenskapen för brödtext i begäran</span><span class="sxs-lookup"><span data-stu-id="1c7e1-246">Request body property</span></span> | <span data-ttu-id="1c7e1-247">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="1c7e1-247">Description</span></span> |
| --- | --- |
| <span data-ttu-id="1c7e1-248">Id</span><span class="sxs-lookup"><span data-stu-id="1c7e1-248">Id</span></span> |<span data-ttu-id="1c7e1-249">ContentKey-Id som vi generera oss själva i följande format ”nb:kid:UUID:<NEW GUID>”.</span><span class="sxs-lookup"><span data-stu-id="1c7e1-249">The ContentKey Id which we generate ourselves using the following format, “nb:kid:UUID:<NEW GUID>”.</span></span> |
| <span data-ttu-id="1c7e1-250">ContentKeyType</span><span class="sxs-lookup"><span data-stu-id="1c7e1-250">ContentKeyType</span></span> |<span data-ttu-id="1c7e1-251">Detta är viktiga innehållstypen som ett heltal för den här nyckeln.</span><span class="sxs-lookup"><span data-stu-id="1c7e1-251">This is the content key type as an integer for this content key.</span></span> <span data-ttu-id="1c7e1-252">Vi skickar värdet 1 för kryptering.</span><span class="sxs-lookup"><span data-stu-id="1c7e1-252">We pass the value 1 for storage encryption.</span></span> |
| <span data-ttu-id="1c7e1-253">EncryptedContentKey</span><span class="sxs-lookup"><span data-stu-id="1c7e1-253">EncryptedContentKey</span></span> |<span data-ttu-id="1c7e1-254">Vi skapa ett nytt innehåll nyckelvärde som är en 256-bitars (32 byte)-värde.</span><span class="sxs-lookup"><span data-stu-id="1c7e1-254">We create a new content key value which is a 256-bit (32 byte) value.</span></span> <span data-ttu-id="1c7e1-255">Nyckeln är krypterad med storage kryptering X.509-certifikat som vi hämta från Microsoft Azure Media Services genom att köra en HTTP GET-begäran för GetProtectionKeyId och GetProtectionKey metoder.</span><span class="sxs-lookup"><span data-stu-id="1c7e1-255">The key is encrypted using the storage encryption X.509 certificate which we retrieve from Microsoft Azure Media Services by executing a HTTP GET request for the GetProtectionKeyId and GetProtectionKey Methods.</span></span> |
| <span data-ttu-id="1c7e1-256">ProtectionKeyId</span><span class="sxs-lookup"><span data-stu-id="1c7e1-256">ProtectionKeyId</span></span> |<span data-ttu-id="1c7e1-257">Detta är skydd nyckel-id för storage kryptering X.509-certifikatet som användes för att kryptera våra innehållsnyckeln.</span><span class="sxs-lookup"><span data-stu-id="1c7e1-257">This is the protection key id for the storage encryption X.509 certificate that was used to encrypt our content key.</span></span> |
| <span data-ttu-id="1c7e1-258">ProtectionKeyType</span><span class="sxs-lookup"><span data-stu-id="1c7e1-258">ProtectionKeyType</span></span> |<span data-ttu-id="1c7e1-259">Detta är krypteringstyp för skydd nyckeln som används för att kryptera innehållsnyckeln.</span><span class="sxs-lookup"><span data-stu-id="1c7e1-259">This is the encryption type for the protection key that was used to encrypt the content key.</span></span> <span data-ttu-id="1c7e1-260">Det här värdet är StorageEncryption(1) i vårt exempel.</span><span class="sxs-lookup"><span data-stu-id="1c7e1-260">This value is StorageEncryption(1) for our example.</span></span> |
| <span data-ttu-id="1c7e1-261">Kontrollsumma</span><span class="sxs-lookup"><span data-stu-id="1c7e1-261">Checksum</span></span> |<span data-ttu-id="1c7e1-262">Den beräknade kontrollsumman MD5 för innehållsnyckeln.</span><span class="sxs-lookup"><span data-stu-id="1c7e1-262">The MD5 calculated checksum for the content key.</span></span> <span data-ttu-id="1c7e1-263">Det beräknas genom att kryptera innehållet Id med innehållsnyckeln.</span><span class="sxs-lookup"><span data-stu-id="1c7e1-263">It is computed by encrypting the content Id with the content key.</span></span> <span data-ttu-id="1c7e1-264">Koden visar hur du beräkna kontrollsumman.</span><span class="sxs-lookup"><span data-stu-id="1c7e1-264">The example code demonstrates how to calculate the checksum.</span></span> |

<span data-ttu-id="1c7e1-265">**HTTP-svar**</span><span class="sxs-lookup"><span data-stu-id="1c7e1-265">**HTTP Response**</span></span>

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

### <a name="link-the-contentkey-to-the-asset"></a><span data-ttu-id="1c7e1-266">Länka ContentKey till tillgången</span><span class="sxs-lookup"><span data-stu-id="1c7e1-266">Link the ContentKey to the Asset</span></span>
<span data-ttu-id="1c7e1-267">ContentKey är kopplad till en eller flera resurser genom att skicka en HTTP POST-begäran.</span><span class="sxs-lookup"><span data-stu-id="1c7e1-267">The ContentKey is associated to one or more assets by sending a HTTP POST request.</span></span> <span data-ttu-id="1c7e1-268">Följande begäran är ett exempel länka exempel ContentKey till exempel tillgången Id.</span><span class="sxs-lookup"><span data-stu-id="1c7e1-268">The following request is an example to link the example ContentKey to the example asset by Id.</span></span>

<span data-ttu-id="1c7e1-269">**HTTP-svar**</span><span class="sxs-lookup"><span data-stu-id="1c7e1-269">**HTTP Response**</span></span>

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

<span data-ttu-id="1c7e1-270">**HTTP-svar**</span><span class="sxs-lookup"><span data-stu-id="1c7e1-270">**HTTP Response**</span></span>

    GET https://media.windows.net/API/IngestManifests('nb:mid:UUID:5c77f186-414f-8b48-8231-17f9264e2048') HTTP/1.1
    Content-Type: application/json;odata=verbose
    Accept: application/json;odata=verbose
    DataServiceVersion: 3.0
    MaxDataServiceVersion: 3.0
    x-ms-version: 2.11
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=070500D0-F35C-4A5A-9249-485BBF4EC70B&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1334275521&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=GxdBb%2fmEyN7iHdNxbawawHRftLhPFFqxX1JZckuv3hY%3d
    Host: media.windows.net

## <a name="next-steps"></a><span data-ttu-id="1c7e1-271">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="1c7e1-271">Next steps</span></span>

<span data-ttu-id="1c7e1-272">Du kan nu koda överförda tillgångar.</span><span class="sxs-lookup"><span data-stu-id="1c7e1-272">You can now encode your uploaded assets.</span></span> <span data-ttu-id="1c7e1-273">Mer information finns i [Koda tillgångar](media-services-portal-encode.md).</span><span class="sxs-lookup"><span data-stu-id="1c7e1-273">For more information, see [Encode assets](media-services-portal-encode.md).</span></span>

<span data-ttu-id="1c7e1-274">Du kan också använda Azure Functions för att utlösa ett kodningsjobb baserat på en fil som skickas till den konfigurerade behållaren.</span><span class="sxs-lookup"><span data-stu-id="1c7e1-274">You can also use Azure Functions to trigger an encoding job based on a file arriving in the configured container.</span></span> <span data-ttu-id="1c7e1-275">Mer information finns i [det här exemplet](https://azure.microsoft.com/resources/samples/media-services-dotnet-functions-integration/ ).</span><span class="sxs-lookup"><span data-stu-id="1c7e1-275">For more information, see [this sample](https://azure.microsoft.com/resources/samples/media-services-dotnet-functions-integration/ ).</span></span>

## <a name="media-services-learning-paths"></a><span data-ttu-id="1c7e1-276">Sökvägar för Media Services-utbildning</span><span class="sxs-lookup"><span data-stu-id="1c7e1-276">Media Services learning paths</span></span>
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a><span data-ttu-id="1c7e1-277">Ge feedback</span><span class="sxs-lookup"><span data-stu-id="1c7e1-277">Provide feedback</span></span>
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

[How to Get a Media Processor]: media-services-get-media-processor.md

