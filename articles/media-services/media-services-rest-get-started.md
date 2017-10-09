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
# <a name="get-started-with-delivering-content-on-demand-using-rest"></a><span data-ttu-id="04e33-103">Komma igång med att leverera innehåll på begäran med hjälp av REST</span><span class="sxs-lookup"><span data-stu-id="04e33-103">Get started with delivering content on demand using REST</span></span>
[!INCLUDE [media-services-selector-get-started](../../includes/media-services-selector-get-started.md)]

<span data-ttu-id="04e33-104">Denna Snabbstart vägleder dig genom hello stegen för implementera ett program för leverans av Video-on-Demand (VoD) med Azure Media Services (AMS) REST API: er.</span><span class="sxs-lookup"><span data-stu-id="04e33-104">This quickstart walks you through hello steps of implementing a Video-on-Demand (VoD) content delivery application using Azure Media Services (AMS) REST APIs.</span></span>

<span data-ttu-id="04e33-105">hello självstudierna innehåller hello grundläggande Media Services-arbetsflödet och hello vanligaste programmeringsobjekt och uppgifter som krävs för Media Services-utveckling.</span><span class="sxs-lookup"><span data-stu-id="04e33-105">hello tutorial introduces hello basic Media Services workflow and hello most common programming objects and tasks required for Media Services development.</span></span> <span data-ttu-id="04e33-106">På hello kursen hello är klar kan ska du kunna toostream eller progressivt hämta en exempelmediefil som överförs, kodat och hämtat.</span><span class="sxs-lookup"><span data-stu-id="04e33-106">At hello completion of hello tutorial, you will be able toostream or progressively download a sample media file that you uploaded, encoded, and downloaded.</span></span>

<span data-ttu-id="04e33-107">hello följande bild visar några av de vanligaste hello objekt när du utvecklar program VoD mot hello Media Services OData-modellen.</span><span class="sxs-lookup"><span data-stu-id="04e33-107">hello following image shows some of hello most commonly used objects when developing VoD applications against hello Media Services OData model.</span></span>

<span data-ttu-id="04e33-108">Klicka på hello avbildningen tooview den full storlek.</span><span class="sxs-lookup"><span data-stu-id="04e33-108">Click hello image tooview it full size.</span></span>  

<a href="./media/media-services-rest-get-started/media-services-overview-object-model.png" target="_blank"><img src="./media/media-services-rest-get-started/media-services-overview-object-model-small.png"></a> 

## <a name="prerequisites"></a><span data-ttu-id="04e33-109">Krav</span><span class="sxs-lookup"><span data-stu-id="04e33-109">Prerequisites</span></span>
<span data-ttu-id="04e33-110">hello är följande krav nödvändiga toostart utveckling med Media Services med REST API: er.</span><span class="sxs-lookup"><span data-stu-id="04e33-110">hello following prerequisites are required toostart developing with Media Services with REST APIs.</span></span>

* <span data-ttu-id="04e33-111">Ett Azure-konto.</span><span class="sxs-lookup"><span data-stu-id="04e33-111">An Azure account.</span></span> <span data-ttu-id="04e33-112">Mer information om den [kostnadsfria utvärderingsversionen av Azure](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="04e33-112">For details, see [Azure Free Trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>
* <span data-ttu-id="04e33-113">Ett Media Services-konto.</span><span class="sxs-lookup"><span data-stu-id="04e33-113">A Media Services account.</span></span> <span data-ttu-id="04e33-114">toocreate Media Services-konto finns [hur tooCreate Media Services-konto](media-services-portal-create-account.md).</span><span class="sxs-lookup"><span data-stu-id="04e33-114">toocreate a Media Services account, see [How tooCreate a Media Services Account](media-services-portal-create-account.md).</span></span>
* <span data-ttu-id="04e33-115">Förståelse av hur toodevelop med Media Services REST API.</span><span class="sxs-lookup"><span data-stu-id="04e33-115">Understanding of how toodevelop with Media Services REST API.</span></span> <span data-ttu-id="04e33-116">Mer information finns i [Media Services REST API-översikt](media-services-rest-how-to-use.md).</span><span class="sxs-lookup"><span data-stu-id="04e33-116">For more information, see [Media Services REST API overview](media-services-rest-how-to-use.md).</span></span>
* <span data-ttu-id="04e33-117">Ett program som kan skicka HTTP-begäranden och -svar.</span><span class="sxs-lookup"><span data-stu-id="04e33-117">An application of your choice that can send HTTP requests and responses.</span></span> <span data-ttu-id="04e33-118">Den här kursen använder [Fiddler](http://www.telerik.com/download/fiddler).</span><span class="sxs-lookup"><span data-stu-id="04e33-118">This tutorial uses [Fiddler](http://www.telerik.com/download/fiddler).</span></span>

<span data-ttu-id="04e33-119">hello efter aktiviteter som visas i den här snabbstarten.</span><span class="sxs-lookup"><span data-stu-id="04e33-119">hello following tasks are shown in this quickstart.</span></span>

1. <span data-ttu-id="04e33-120">Starta strömmande slutpunkten (med hello Azure-portalen).</span><span class="sxs-lookup"><span data-stu-id="04e33-120">Start streaming endpoint (using hello Azure portal).</span></span>
2. <span data-ttu-id="04e33-121">Ansluta toohello Media Services-konto med REST API.</span><span class="sxs-lookup"><span data-stu-id="04e33-121">Connect toohello Media Services account with REST API.</span></span>
3. <span data-ttu-id="04e33-122">Skapa en ny tillgång och överföra en videofil med REST API.</span><span class="sxs-lookup"><span data-stu-id="04e33-122">Create a new asset and upload a video file with REST API.</span></span>
4. <span data-ttu-id="04e33-123">Koda hello källfilen till en uppsättning filer med anpassningsbar bithastighet MP4 med REST API.</span><span class="sxs-lookup"><span data-stu-id="04e33-123">Encode hello source file into a set of adaptive bitrate MP4 files with REST API.</span></span>
5. <span data-ttu-id="04e33-124">Publicera hello tillgången och get strömning och progressiv nedladdning URL: er med REST API.</span><span class="sxs-lookup"><span data-stu-id="04e33-124">Publish hello asset and get streaming and progressive download URLs with REST API.</span></span>
6. <span data-ttu-id="04e33-125">Spela upp ditt innehåll.</span><span class="sxs-lookup"><span data-stu-id="04e33-125">Play your content.</span></span>

>[!NOTE]
><span data-ttu-id="04e33-126">Det finns en gräns på 1 000 000 principer för olika AMS-principer (till exempel för positionerarprincipen eller ContentKeyAuthorizationPolicy).</span><span class="sxs-lookup"><span data-stu-id="04e33-126">There is a limit of 1,000,000 policies for different AMS policies (for example, for Locator policy or ContentKeyAuthorizationPolicy).</span></span> <span data-ttu-id="04e33-127">Du bör använda hello samma princip-ID om du alltid använder hello samma dagar / åtkomstbehörigheter, till exempel principer för lokaliserare som är avsedda tooremain på plats för lång tid (icke-överföringen principer).</span><span class="sxs-lookup"><span data-stu-id="04e33-127">You should use hello same policy ID if you are always using hello same days / access permissions, for example, policies for locators that are intended tooremain in place for a long time (non-upload policies).</span></span> <span data-ttu-id="04e33-128">Mer information finns i [detta](media-services-dotnet-manage-entities.md#limit-access-policies) avsnitt.</span><span class="sxs-lookup"><span data-stu-id="04e33-128">For more information, see [this](media-services-dotnet-manage-entities.md#limit-access-policies) topic.</span></span>

<span data-ttu-id="04e33-129">Mer information om AMS REST-entiteter som används i det här avsnittet finns [Azure Media Services REST API-referens](/rest/api/media/services/azure-media-services-rest-api-reference).</span><span class="sxs-lookup"><span data-stu-id="04e33-129">For details about AMS REST entities used in this topic, see [Azure Media Services REST API Reference](/rest/api/media/services/azure-media-services-rest-api-reference).</span></span> <span data-ttu-id="04e33-130">Se även [Azure Media Services-koncepten](media-services-concepts.md).</span><span class="sxs-lookup"><span data-stu-id="04e33-130">Also, see [Azure Media Services concepts](media-services-concepts.md).</span></span>

>[!NOTE]
><span data-ttu-id="04e33-131">Vid åtkomst till entiteter i Media Services måste du ange specifika namn på huvudfält och värden i HTTP-begäranden.</span><span class="sxs-lookup"><span data-stu-id="04e33-131">When accessing entities in Media Services, you must set specific header fields and values in your HTTP requests.</span></span> <span data-ttu-id="04e33-132">Mer information finns i [installationsprogrammet för Media Services REST API-utveckling](media-services-rest-how-to-use.md).</span><span class="sxs-lookup"><span data-stu-id="04e33-132">For more information, see [Setup for Media Services REST API Development](media-services-rest-how-to-use.md).</span></span>

## <a name="start-streaming-endpoints-using-hello-azure-portal"></a><span data-ttu-id="04e33-133">Starta strömningsslutpunkter med hello Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="04e33-133">Start streaming endpoints using hello Azure portal</span></span>

<span data-ttu-id="04e33-134">När du arbetar med Azure Media Services är en av hello vanligaste scenarierna att leverera video via strömning med anpassningsbar bithastighet.</span><span class="sxs-lookup"><span data-stu-id="04e33-134">When working with Azure Media Services one of hello most common scenarios is delivering video via adaptive bitrate streaming.</span></span> <span data-ttu-id="04e33-135">Media Services tillhandahåller en dynamisk paketering som gör att du toodeliver dina MP4-kodade innehåll i strömningsformat som stöds av Media Services (MPEG DASH, HLS, Smooth Streaming) just-in-time, utan att behöva toostore tillsammans med anpassad bithastighet versioner av var och en av dessa strömningsformat.</span><span class="sxs-lookup"><span data-stu-id="04e33-135">Media Services provides dynamic packaging, which allows you toodeliver your adaptive bitrate MP4 encoded content in streaming formats supported by Media Services (MPEG DASH, HLS, Smooth Streaming) just-in-time, without you having toostore pre-packaged versions of each of these streaming formats.</span></span>

>[!NOTE]
><span data-ttu-id="04e33-136">När AMS-kontot skapas en **standard** strömningsslutpunkt har lagts till tooyour konto i hello **stoppad** tillstånd.</span><span class="sxs-lookup"><span data-stu-id="04e33-136">When your AMS account is created a **default** streaming endpoint is added tooyour account in hello **Stopped** state.</span></span> <span data-ttu-id="04e33-137">toostart strömning ditt innehåll och dra nytta av dynamisk paketering och dynamisk kryptering hello strömningsslutpunkt som du vill toostream innehåll har toobe i hello **kör** tillstånd.</span><span class="sxs-lookup"><span data-stu-id="04e33-137">toostart streaming your content and take advantage of dynamic packaging and dynamic encryption, hello streaming endpoint from which you want toostream content has toobe in hello **Running** state.</span></span>

<span data-ttu-id="04e33-138">toostart Hej strömmande slutpunkten, hello följande:</span><span class="sxs-lookup"><span data-stu-id="04e33-138">toostart hello streaming endpoint, do hello following:</span></span>

1. <span data-ttu-id="04e33-139">Logga in på hello [Azure-portalen](https://portal.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="04e33-139">Log in at hello [Azure portal](https://portal.azure.com/).</span></span>
2. <span data-ttu-id="04e33-140">Streaming slutpunkter på hello inställningar i fönstret.</span><span class="sxs-lookup"><span data-stu-id="04e33-140">In hello Settings window, click Streaming endpoints.</span></span>
3. <span data-ttu-id="04e33-141">Klicka på hello standard strömmande slutpunkten.</span><span class="sxs-lookup"><span data-stu-id="04e33-141">Click hello default streaming endpoint.</span></span>

    <span data-ttu-id="04e33-142">hello visas standard information om den STRÖMNINGSSLUTPUNKT.</span><span class="sxs-lookup"><span data-stu-id="04e33-142">hello DEFAULT STREAMING ENDPOINT DETAILS window appears.</span></span>

4. <span data-ttu-id="04e33-143">Klicka på hello Start-ikonen.</span><span class="sxs-lookup"><span data-stu-id="04e33-143">Click hello Start icon.</span></span>
5. <span data-ttu-id="04e33-144">Klicka på hello spara knappen toosave ändringarna.</span><span class="sxs-lookup"><span data-stu-id="04e33-144">Click hello Save button toosave your changes.</span></span>

## <span data-ttu-id="04e33-145"><a id="connect"></a>Anslut toohello Media Services-konto med REST API</span><span class="sxs-lookup"><span data-stu-id="04e33-145"><a id="connect"></a>Connect toohello Media Services account with REST API</span></span>

<span data-ttu-id="04e33-146">Mer information om hur tooconnect toohello AMS API, se [åtkomst hello Azure Media Services API med Azure AD authentication](media-services-use-aad-auth-to-access-ams-api.md).</span><span class="sxs-lookup"><span data-stu-id="04e33-146">For information on how tooconnect toohello AMS API, see [Access hello Azure Media Services API with Azure AD authentication](media-services-use-aad-auth-to-access-ams-api.md).</span></span> 

>[!NOTE]
><span data-ttu-id="04e33-147">När du har anslutit toohttps://media.windows.net, får du en 301 omdirigering att ange en annan Media Services-URI.</span><span class="sxs-lookup"><span data-stu-id="04e33-147">After successfully connecting toohttps://media.windows.net, you will receive a 301 redirect specifying another Media Services URI.</span></span> <span data-ttu-id="04e33-148">Du måste göra följande anrop toohello ny URI.</span><span class="sxs-lookup"><span data-stu-id="04e33-148">You must make subsequent calls toohello new URI.</span></span>

<span data-ttu-id="04e33-149">Till exempel om när du har försökt tooconnect, du har fått hello följande:</span><span class="sxs-lookup"><span data-stu-id="04e33-149">For example, if after trying tooconnect, you got hello following:</span></span>

    HTTP/1.1 301 Moved Permanently
    Location: https://wamsbayclus001rest-hs.cloudapp.net/api/

<span data-ttu-id="04e33-150">Du bör publicera dina efterföljande API-anrop toohttps://wamsbayclus001rest-hs.cloudapp.net/api/.</span><span class="sxs-lookup"><span data-stu-id="04e33-150">You should post your subsequent API calls toohttps://wamsbayclus001rest-hs.cloudapp.net/api/.</span></span>

## <span data-ttu-id="04e33-151"><a id="upload"></a>Skapa en ny tillgång och överföra en videofil med REST API</span><span class="sxs-lookup"><span data-stu-id="04e33-151"><a id="upload"></a>Create a new asset and upload a video file with REST API</span></span>

<span data-ttu-id="04e33-152">I Media Services överför du dina digitala filer till en tillgång.</span><span class="sxs-lookup"><span data-stu-id="04e33-152">In Media Services, you upload your digital files into an asset.</span></span> <span data-ttu-id="04e33-153">Hej **tillgången** entitet kan innehålla video, ljud, bilder, miniatyrsamlingar, text spår och textning filer (och hello metadata om dessa filer.)  När hello filerna har överförts till hello tillgång lagras innehållet på ett säkert sätt i hello molnet för ytterligare bearbetning och strömning.</span><span class="sxs-lookup"><span data-stu-id="04e33-153">hello **Asset** entity can contain video, audio, images, thumbnail collections, text tracks and closed caption files (and hello metadata about these files.)  Once hello files are uploaded into hello asset, your content is stored securely in hello cloud for further processing and streaming.</span></span>

<span data-ttu-id="04e33-154">En av hello värden att du har tooprovide när du skapar en tillgång är alternativ för att skapa tillgången.</span><span class="sxs-lookup"><span data-stu-id="04e33-154">One of hello values that you have tooprovide when creating an asset is asset creation options.</span></span> <span data-ttu-id="04e33-155">Hej **alternativ** egenskapen är ett uppräkningsvärde som beskriver hello krypteringsalternativ som du kan skapa en tillgång med.</span><span class="sxs-lookup"><span data-stu-id="04e33-155">hello **Options** property is an enumeration value that describes hello encryption options that an Asset can be created with.</span></span> <span data-ttu-id="04e33-156">Ett giltigt värde är ett av hello värden hello listan nedan, inte en kombination av värden från listan:</span><span class="sxs-lookup"><span data-stu-id="04e33-156">A valid value is one of hello values from hello list below, not a combination of values from this list:</span></span>

* <span data-ttu-id="04e33-157">**Ingen** = **0** – Ingen kryptering används.</span><span class="sxs-lookup"><span data-stu-id="04e33-157">**None** = **0** - No encryption is used.</span></span> <span data-ttu-id="04e33-158">När du använder det här alternativet skyddas inte innehållet under överföring eller i vila i lagringsutrymmet.</span><span class="sxs-lookup"><span data-stu-id="04e33-158">When using this option your content is not protected in transit or at rest in storage.</span></span>
    <span data-ttu-id="04e33-159">Använd det här alternativet om du planerar toodeliver en MP4 med progressivt nedladdning.</span><span class="sxs-lookup"><span data-stu-id="04e33-159">If you plan toodeliver an MP4 using progressive download, use this option.</span></span>
* <span data-ttu-id="04e33-160">**StorageEncrypted** = **1** – krypterar innehållet lokalt med hjälp av AES 256 bitarskryptering och sedan överför tooAzure lagring där den lagras krypterat i vila.</span><span class="sxs-lookup"><span data-stu-id="04e33-160">**StorageEncrypted** = **1** - Encrypts your clear content locally using AES-256 bit encryption and then uploads it tooAzure Storage where it is stored encrypted at rest.</span></span> <span data-ttu-id="04e33-161">Tillgångar som skyddas med Lagringskryptering okrypterad och placeras i en krypterad fil system tidigare tooencoding och eventuellt omkrypterade tidigare toouploading tillbaka som en ny utdatatillgång automatiskt.</span><span class="sxs-lookup"><span data-stu-id="04e33-161">Assets protected with Storage Encryption are automatically unencrypted and placed in an encrypted file system prior tooencoding, and optionally re-encrypted prior toouploading back as a new output asset.</span></span> <span data-ttu-id="04e33-162">hello primära användningsfall för Lagringskryptering är om du vill toosecure hög kvalitet inkommande mediefilerna med stark kryptering i vila på disk.</span><span class="sxs-lookup"><span data-stu-id="04e33-162">hello primary use case for Storage Encryption is when you want toosecure your high-quality input media files with strong encryption at rest on disk.</span></span>
* <span data-ttu-id="04e33-163">**CommonEncryptionProtected** = **2** – Använd det här alternativet om du överför innehåll som redan har krypterats och skyddats med vanlig kryptering eller PlayReady DRM (till exempel Smooth Streaming som skyddas med PlayReady DRM).</span><span class="sxs-lookup"><span data-stu-id="04e33-163">**CommonEncryptionProtected** = **2** - Use this option if you are uploading content that has already been encrypted and protected with Common Encryption or PlayReady DRM (for example, Smooth Streaming protected with PlayReady DRM).</span></span>
* <span data-ttu-id="04e33-164">**EnvelopeEncryptionProtected** = **4** – Använd det här alternativet om du överför HLS som krypterats med AES.</span><span class="sxs-lookup"><span data-stu-id="04e33-164">**EnvelopeEncryptionProtected** = **4** – Use this option if you are uploading HLS encrypted with AES.</span></span> <span data-ttu-id="04e33-165">hello-filer måste ha kodats och krypterats av Transform Manager.</span><span class="sxs-lookup"><span data-stu-id="04e33-165">hello files must have been encoded and encrypted by Transform Manager.</span></span>

### <a name="create-an-asset"></a><span data-ttu-id="04e33-166">Skapa en tillgång</span><span class="sxs-lookup"><span data-stu-id="04e33-166">Create an asset</span></span>
<span data-ttu-id="04e33-167">En tillgång är en behållare för flera typer eller uppsättningar med objekt i Media Services, inklusive video, ljud, bilder, miniatyrsamlingar, textspår och filer med dold textning.</span><span class="sxs-lookup"><span data-stu-id="04e33-167">An asset is a container for multiple types or sets of objects in Media Services, including video, audio, images, thumbnail collections, text tracks, and closed caption files.</span></span> <span data-ttu-id="04e33-168">I hello REST-API: skapa en tillgång måste du skicka POST begära tooMedia tjänster och placerar egenskapsinformation om din tillgång i hello frågans brödtext.</span><span class="sxs-lookup"><span data-stu-id="04e33-168">In hello REST API, creating an Asset requires sending POST request tooMedia Services and placing any property information about your asset in hello request body.</span></span>

<span data-ttu-id="04e33-169">följande exempel visar hur hello toocreate en tillgång.</span><span class="sxs-lookup"><span data-stu-id="04e33-169">hello following example shows how toocreate an asset.</span></span>

<span data-ttu-id="04e33-170">**HTTP-begäran**</span><span class="sxs-lookup"><span data-stu-id="04e33-170">**HTTP Request**</span></span>

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


<span data-ttu-id="04e33-171">**HTTP-svar**</span><span class="sxs-lookup"><span data-stu-id="04e33-171">**HTTP Response**</span></span>

<span data-ttu-id="04e33-172">Om det lyckas, returneras hello följande:</span><span class="sxs-lookup"><span data-stu-id="04e33-172">If successful, hello following is returned:</span></span>

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

### <a name="create-an-assetfile"></a><span data-ttu-id="04e33-173">Skapa en AssetFile</span><span class="sxs-lookup"><span data-stu-id="04e33-173">Create an AssetFile</span></span>
<span data-ttu-id="04e33-174">Hej [AssetFile](https://docs.microsoft.com/rest/api/media/operations/assetfile) entiteten representerar en video eller ljud-fil som lagras i en blob-behållare.</span><span class="sxs-lookup"><span data-stu-id="04e33-174">hello [AssetFile](https://docs.microsoft.com/rest/api/media/operations/assetfile) entity represents a video or audio file that is stored in a blob container.</span></span> <span data-ttu-id="04e33-175">En resursfil är alltid associerad med en tillgång och en tillgång kan innehålla en eller flera AssetFiles.</span><span class="sxs-lookup"><span data-stu-id="04e33-175">An asset file is always associated with an asset, and an asset may contain one or many AssetFiles.</span></span> <span data-ttu-id="04e33-176">hello misslyckas Media Services-kodaren om objekttypen tillgången filen inte är associerad med en digital fil i en blob-behållaren.</span><span class="sxs-lookup"><span data-stu-id="04e33-176">hello Media Services Encoder task fails if an asset file object is not associated with a digital file in a blob container.</span></span>

<span data-ttu-id="04e33-177">När du har överfört din digitala media-fil till en blobbbehållare du använder hello **sammanfoga** HTTP-begäran tooupdate hello AssetFile med information om media-fil (som visas i hello kapitel).</span><span class="sxs-lookup"><span data-stu-id="04e33-177">After you upload your digital media file into a blob container, you use hello **MERGE** HTTP request tooupdate hello AssetFile with information about your media file (as shown later in hello topic).</span></span>

<span data-ttu-id="04e33-178">**HTTP-begäran**</span><span class="sxs-lookup"><span data-stu-id="04e33-178">**HTTP Request**</span></span>

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


<span data-ttu-id="04e33-179">**HTTP-svar**</span><span class="sxs-lookup"><span data-stu-id="04e33-179">**HTTP Response**</span></span>

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


### <a name="creating-hello-accesspolicy-with-write-permission"></a><span data-ttu-id="04e33-180">Skapa hello AccessPolicy med behörighet att skriva</span><span class="sxs-lookup"><span data-stu-id="04e33-180">Creating hello AccessPolicy with write permission</span></span>
<span data-ttu-id="04e33-181">Innan du laddar upp filer i blob-lagring, ange hello princip rättigheter för att skriva tooan tillgången.</span><span class="sxs-lookup"><span data-stu-id="04e33-181">Before uploading any files into blob storage, set hello access policy rights for writing tooan asset.</span></span> <span data-ttu-id="04e33-182">Ange toodo som PUBLICERAR en HTTP-begäran toohello AccessPolicies entitet.</span><span class="sxs-lookup"><span data-stu-id="04e33-182">toodo that, POST an HTTP request toohello AccessPolicies entity set.</span></span> <span data-ttu-id="04e33-183">Definiera ett DurationInMinutes värde när de skapas eller felmeddelande en 500 intern Server tillbaka som svar.</span><span class="sxs-lookup"><span data-stu-id="04e33-183">Define a DurationInMinutes value upon creation or you receive a 500 Internal Server error message back in response.</span></span> <span data-ttu-id="04e33-184">Mer information om AccessPolicies finns [AccessPolicy](https://docs.microsoft.com/rest/api/media/operations/accesspolicy).</span><span class="sxs-lookup"><span data-stu-id="04e33-184">For more information on AccessPolicies, see [AccessPolicy](https://docs.microsoft.com/rest/api/media/operations/accesspolicy).</span></span>

<span data-ttu-id="04e33-185">följande exempel visar hur hello toocreate en AccessPolicy:</span><span class="sxs-lookup"><span data-stu-id="04e33-185">hello following example shows how toocreate an AccessPolicy:</span></span>

<span data-ttu-id="04e33-186">**HTTP-begäran**</span><span class="sxs-lookup"><span data-stu-id="04e33-186">**HTTP Request**</span></span>

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

<span data-ttu-id="04e33-187">**HTTP-svar**</span><span class="sxs-lookup"><span data-stu-id="04e33-187">**HTTP Response**</span></span>

<span data-ttu-id="04e33-188">Om det lyckas, returneras hello efter svar:</span><span class="sxs-lookup"><span data-stu-id="04e33-188">If successful, hello following response is returned:</span></span>

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

### <a name="get-hello-upload-url"></a><span data-ttu-id="04e33-189">Hämta hello URL för överföring</span><span class="sxs-lookup"><span data-stu-id="04e33-189">Get hello Upload URL</span></span>

<span data-ttu-id="04e33-190">tooreceive hello faktiska överför URL, skapa en SAS-lokaliserare.</span><span class="sxs-lookup"><span data-stu-id="04e33-190">tooreceive hello actual upload URL, create a SAS Locator.</span></span> <span data-ttu-id="04e33-191">Lokaliserare definiera hello starttid och typ av anslutningens slutpunkt för klienter som vill tooaccess filer i en tillgång.</span><span class="sxs-lookup"><span data-stu-id="04e33-191">Locators define hello start time and type of connection endpoint for clients that want tooaccess Files in an Asset.</span></span> <span data-ttu-id="04e33-192">Du kan skapa flera lokaliserare entiteter för en viss AccessPolicy och tillgångshantering par toohandle olika klient begäranden och behov.</span><span class="sxs-lookup"><span data-stu-id="04e33-192">You can create multiple Locator entities for a given AccessPolicy and Asset pair toohandle different client requests and needs.</span></span> <span data-ttu-id="04e33-193">Var och en av dessa lokaliserare använder hello StartTime-värdet plus hello DurationInMinutes värdet av hello AccessPolicy toodetermine hello tid kan användas för en URL.</span><span class="sxs-lookup"><span data-stu-id="04e33-193">Each of these Locators uses hello StartTime value plus hello DurationInMinutes value of hello AccessPolicy toodetermine hello length of time a URL can be used.</span></span> <span data-ttu-id="04e33-194">Mer information finns i [lokaliserare](https://docs.microsoft.com/rest/api/media/operations/locator).</span><span class="sxs-lookup"><span data-stu-id="04e33-194">For more information, see [Locator](https://docs.microsoft.com/rest/api/media/operations/locator).</span></span>

<span data-ttu-id="04e33-195">En SAS-URL har hello följande format:</span><span class="sxs-lookup"><span data-stu-id="04e33-195">A SAS URL has hello following format:</span></span>

    {https://myaccount.blob.core.windows.net}/{asset name}/{video file name}?{SAS signature}

<span data-ttu-id="04e33-196">Vissa förutsättningar gäller:</span><span class="sxs-lookup"><span data-stu-id="04e33-196">Some considerations apply:</span></span>

* <span data-ttu-id="04e33-197">Du kan inte ha fler än fem unika lokaliserare som är associerade med en viss resurs i taget.</span><span class="sxs-lookup"><span data-stu-id="04e33-197">You cannot have more than five unique Locators associated with a given Asset at one time.</span></span> <span data-ttu-id="04e33-198">Mer information finns i lokaliserare.</span><span class="sxs-lookup"><span data-stu-id="04e33-198">For more information, see Locator.</span></span>
* <span data-ttu-id="04e33-199">Om du behöver tooupload dina filer direkt, bör du ange StartTime värdet toofive minuter innan hello aktuell tid.</span><span class="sxs-lookup"><span data-stu-id="04e33-199">If you need tooupload your files immediately, you should set your StartTime value toofive minutes before hello current time.</span></span> <span data-ttu-id="04e33-200">Det beror på att det kan finnas klockan skeva mellan klientdatorn och Media Services.</span><span class="sxs-lookup"><span data-stu-id="04e33-200">This is because there may be clock skew between your client machine and Media Services.</span></span> <span data-ttu-id="04e33-201">StartTime-värdet måste också vara i hello följande DateTime-format: ÅÅÅÅ-MM-ddTHH (till exempel ”2014-05-23T17:53:50Z”).</span><span class="sxs-lookup"><span data-stu-id="04e33-201">Also, your StartTime value must be in hello following DateTime format: YYYY-MM-DDTHH:mm:ssZ (for example, "2014-05-23T17:53:50Z").</span></span>    
* <span data-ttu-id="04e33-202">Det kan finnas en 30-40 andra fördröjning efter en positionerare skapas toowhen som den är tillgänglig för användning.</span><span class="sxs-lookup"><span data-stu-id="04e33-202">There may be a 30-40 second delay after a Locator is created toowhen it is available for use.</span></span> <span data-ttu-id="04e33-203">Det här problemet gäller tooboth SAS-URL och ursprung lokaliserare.</span><span class="sxs-lookup"><span data-stu-id="04e33-203">This issue applies tooboth SAS URL and Origin Locators.</span></span>

<span data-ttu-id="04e33-204">Mer information om SAS finns lokaliserare [detta](http://southworks.com/blog/2015/05/27/reusing-azure-media-services-locators-to-avoid-facing-the-5-shared-access-policy-limitation/) blogg.</span><span class="sxs-lookup"><span data-stu-id="04e33-204">For more information about SAS locators see [this](http://southworks.com/blog/2015/05/27/reusing-azure-media-services-locators-to-avoid-facing-the-5-shared-access-policy-limitation/) blog.</span></span>

<span data-ttu-id="04e33-205">hello som följande exempel visar hur toocreate en SAS-URL-lokaliserare, som definieras av hello typegenskapen i begärandetexten hello (”1” för en SAS-lokaliserare) och ”2” för en positionerare för ursprung på begäran.</span><span class="sxs-lookup"><span data-stu-id="04e33-205">hello following example shows how toocreate a SAS URL Locator, as defined by hello Type property in hello request body ("1" for a SAS locator and "2" for an On-Demand origin locator).</span></span> <span data-ttu-id="04e33-206">Hej **sökväg** egenskap som returneras innehåller hello-URL att du måste använda tooupload din fil.</span><span class="sxs-lookup"><span data-stu-id="04e33-206">hello **Path** property returned contains hello URL that you must use tooupload your file.</span></span>

<span data-ttu-id="04e33-207">**HTTP-begäran**</span><span class="sxs-lookup"><span data-stu-id="04e33-207">**HTTP Request**</span></span>

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


<span data-ttu-id="04e33-208">**HTTP-svar**</span><span class="sxs-lookup"><span data-stu-id="04e33-208">**HTTP Response**</span></span>

<span data-ttu-id="04e33-209">Om det lyckas, returneras hello efter svar:</span><span class="sxs-lookup"><span data-stu-id="04e33-209">If successful, hello following response is returned:</span></span>

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

### <a name="upload-a-file-into-a-blob-storage-container"></a><span data-ttu-id="04e33-210">Ladda upp en fil till en behållare för blob storage</span><span class="sxs-lookup"><span data-stu-id="04e33-210">Upload a file into a blob storage container</span></span>
<span data-ttu-id="04e33-211">När du har hello AccessPolicy och lokaliserare set är hello faktiska filen överförda tooan Azure blob storage-behållare med hello Azure Storage REST API: er.</span><span class="sxs-lookup"><span data-stu-id="04e33-211">Once you have hello AccessPolicy and Locator set, hello actual file is uploaded tooan Azure blob storage container using hello Azure Storage REST APIs.</span></span> <span data-ttu-id="04e33-212">Du måste överföra hello filer som blockblobar.</span><span class="sxs-lookup"><span data-stu-id="04e33-212">You must upload hello files as block blobs.</span></span> <span data-ttu-id="04e33-213">Azure Media Services stöder inte sidblobar.</span><span class="sxs-lookup"><span data-stu-id="04e33-213">Page blobs are not supported by Azure Media Services.</span></span>  

> [!NOTE]
> <span data-ttu-id="04e33-214">Du måste lägga till hello filnamn hello filen du vill ha tooupload toohello lokaliserare **sökväg** värde togs emot i hello föregående avsnitt.</span><span class="sxs-lookup"><span data-stu-id="04e33-214">You must add hello file name for hello file you want tooupload toohello Locator **Path** value received in hello previous section.</span></span> <span data-ttu-id="04e33-215">Till exempel https://storagetestaccount001.blob.core.windows.net/asset-e7b02da4-5a69-40e7-a8db-e8f4f697aac0/BigBuckBunny.mp4?</span><span class="sxs-lookup"><span data-stu-id="04e33-215">For example, https://storagetestaccount001.blob.core.windows.net/asset-e7b02da4-5a69-40e7-a8db-e8f4f697aac0/BigBuckBunny.mp4?</span></span> <span data-ttu-id="04e33-216">.</span><span class="sxs-lookup"><span data-stu-id="04e33-216">.</span></span> <span data-ttu-id="04e33-217">.</span><span class="sxs-lookup"><span data-stu-id="04e33-217">.</span></span> <span data-ttu-id="04e33-218">.</span><span class="sxs-lookup"><span data-stu-id="04e33-218">.</span></span>
>
>

<span data-ttu-id="04e33-219">Mer information om hur du arbetar med Azure storage blobs finns [REST-API för Blob-tjänsten](https://docs.microsoft.com/rest/api/storageservices/Blob-Service-REST-API).</span><span class="sxs-lookup"><span data-stu-id="04e33-219">For more information on working with Azure storage blobs, see [Blob Service REST API](https://docs.microsoft.com/rest/api/storageservices/Blob-Service-REST-API).</span></span>

### <a name="update-hello-assetfile"></a><span data-ttu-id="04e33-220">Uppdatera hello AssetFile</span><span class="sxs-lookup"><span data-stu-id="04e33-220">Update hello AssetFile</span></span>
<span data-ttu-id="04e33-221">Nu när du har överfört din fil uppdateringsinformation hello FileAsset storlek (och andra).</span><span class="sxs-lookup"><span data-stu-id="04e33-221">Now that you've uploaded your file, update hello FileAsset size (and other) information.</span></span> <span data-ttu-id="04e33-222">Exempel:</span><span class="sxs-lookup"><span data-stu-id="04e33-222">For example:</span></span>

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


<span data-ttu-id="04e33-223">**HTTP-svar**</span><span class="sxs-lookup"><span data-stu-id="04e33-223">**HTTP Response**</span></span>

<span data-ttu-id="04e33-224">Om det lyckas, returneras hello följande:</span><span class="sxs-lookup"><span data-stu-id="04e33-224">If successful, hello following is returned:</span></span>

    HTTP/1.1 204 No Content
    ...

## <a name="delete-hello-locator-and-accesspolicy"></a><span data-ttu-id="04e33-225">Ta bort hello lokaliserare och AccessPolicy</span><span class="sxs-lookup"><span data-stu-id="04e33-225">Delete hello Locator and AccessPolicy</span></span>
<span data-ttu-id="04e33-226">**HTTP-begäran**</span><span class="sxs-lookup"><span data-stu-id="04e33-226">**HTTP Request**</span></span>

    DELETE https://wamsbayclus001rest-hs.cloudapp.net/api/Locators('nb%3Alid%3AUUID%3Aaf57bdd8-6751-4e84-b403-f3c140444b54') HTTP/1.1
    DataServiceVersion: 1.0;NetFx
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json
    Accept-Charset: UTF-8
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=amstestaccount001&urn%3aSubscriptionId=z7f09258-2233-4ca2-b1ae-193798e2c9d8&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1421662918&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=utmoXXbm9Q7j4tW1yJuMVA3egRiQy5FPygwadkmPeaY%3d
    x-ms-version: 2.11
    Host: wamsbayclus001rest-hs.cloudapp.net


<span data-ttu-id="04e33-227">**HTTP-svar**</span><span class="sxs-lookup"><span data-stu-id="04e33-227">**HTTP Response**</span></span>

<span data-ttu-id="04e33-228">Om det lyckas, returneras hello följande:</span><span class="sxs-lookup"><span data-stu-id="04e33-228">If successful, hello following is returned:</span></span>

    HTTP/1.1 204 No Content
    ...

<span data-ttu-id="04e33-229">**HTTP-begäran**</span><span class="sxs-lookup"><span data-stu-id="04e33-229">**HTTP Request**</span></span>

    DELETE https://wamsbayclus001rest-hs.cloudapp.net/api/AccessPolicies('nb%3Apid%3AUUID%3Abe0ac48d-af7d-4877-9d60-1805d68bffae') HTTP/1.1
    DataServiceVersion: 1.0;NetFx
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json
    Accept-Charset: UTF-8
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=amstestaccount001&urn%3aSubscriptionId=z7f09258-2233-4ca2-b1ae-193798e2c9d8&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1421662918&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=utmoXXbm9Q7j4tW1yJuMVA3egRiQy5FPygwadkmPeaY%3d
    x-ms-version: 2.11
    Host: wamsbayclus001rest-hs.cloudapp.net

<span data-ttu-id="04e33-230">**HTTP-svar**</span><span class="sxs-lookup"><span data-stu-id="04e33-230">**HTTP Response**</span></span>

<span data-ttu-id="04e33-231">Om det lyckas, returneras hello följande:</span><span class="sxs-lookup"><span data-stu-id="04e33-231">If successful, hello following is returned:</span></span>

    HTTP/1.1 204 No Content
    ...

## <span data-ttu-id="04e33-232"><a id="encode"></a>Koda hello källfilen till en uppsättning MP4-filer med anpassningsbar bithastighet</span><span class="sxs-lookup"><span data-stu-id="04e33-232"><a id="encode"></a>Encode hello source file into a set of adaptive bitrate MP4 files</span></span>

<span data-ttu-id="04e33-233">När du vill föra in tillgångar i Media Services kan media kan vara kodad, transmuxed, gräns och så vidare innan de skickas tooclients.</span><span class="sxs-lookup"><span data-stu-id="04e33-233">After ingesting Assets into Media Services, media can be encoded, transmuxed, watermarked, and so on, before it is delivered tooclients.</span></span> <span data-ttu-id="04e33-234">Dessa aktiviteter schemaläggs och körs mot flera bakgrund rollen instanser tooensure hög prestanda och tillgänglighet.</span><span class="sxs-lookup"><span data-stu-id="04e33-234">These activities are scheduled and run against multiple background role instances tooensure high performance and availability.</span></span> <span data-ttu-id="04e33-235">Aktiviteterna kallas jobb och varje jobb består av atomiska uppgifter som hello faktiska arbetet i tillgångsfilen hello (Mer information finns i [jobbet](/rest/api/media/services/job), [aktivitet](/rest/api/media/services/task) beskrivningar).</span><span class="sxs-lookup"><span data-stu-id="04e33-235">These activities are called Jobs and each Job is composed of atomic Tasks that do hello actual work on hello Asset file (for more information, see [Job](/rest/api/media/services/job), [Task](/rest/api/media/services/task) descriptions).</span></span>

<span data-ttu-id="04e33-236">Som tidigare nämnts, när arbeta med Azure Media Services en av de vanligaste scenarierna för hello levererar strömning tooyour klienter med anpassningsbar bithastighet.</span><span class="sxs-lookup"><span data-stu-id="04e33-236">As was mentioned earlier, when working with Azure Media Services one of hello most common scenarios is delivering adaptive bitrate streaming tooyour clients.</span></span> <span data-ttu-id="04e33-237">Media Services kan dynamiskt Paketera en uppsättning MP4-filer med anpassningsbar bithastighet till en hello följande format: HTTP Live Streaming (HLS), Smooth Streaming, MPEG DASH.</span><span class="sxs-lookup"><span data-stu-id="04e33-237">Media Services can dynamically package a set of adaptive bitrate MP4 files into one of hello following formats: HTTP Live Streaming (HLS), Smooth Streaming, MPEG DASH.</span></span>

<span data-ttu-id="04e33-238">hello efter avsnittet visar hur toocreate ett jobb som innehåller en kodning uppgift.</span><span class="sxs-lookup"><span data-stu-id="04e33-238">hello following section shows how toocreate a job that contains one encoding task.</span></span> <span data-ttu-id="04e33-239">hello aktivitet anger tootranscode hello mezzaninfil till en uppsättning med anpassningsbar bithastighet MP4s **Media Encoder Standard**.</span><span class="sxs-lookup"><span data-stu-id="04e33-239">hello task specifies tootranscode hello mezzanine file into a set of adaptive bitrate MP4s using **Media Encoder Standard**.</span></span> <span data-ttu-id="04e33-240">hello avsnitt visar även hur toomonitor hello jobbförloppet för bearbetning.</span><span class="sxs-lookup"><span data-stu-id="04e33-240">hello section also shows how toomonitor hello job processing progress.</span></span> <span data-ttu-id="04e33-241">Du kommer att kan toocreate lokaliserare som är nödvändiga tooget åtkomst tooyour tillgångar när hello jobbet är klart.</span><span class="sxs-lookup"><span data-stu-id="04e33-241">When hello job is complete, you would be able toocreate locators that are needed tooget access tooyour assets.</span></span>

### <a name="get-a-media-processor"></a><span data-ttu-id="04e33-242">Hämta en medieprocessor</span><span class="sxs-lookup"><span data-stu-id="04e33-242">Get a media processor</span></span>
<span data-ttu-id="04e33-243">I Media Services är en medieprocessor en komponent som hanterar en specifik bearbetning aktivitet, till exempel kodning, kryptering eller dekryptering medieinnehåll-Formatkonvertering.</span><span class="sxs-lookup"><span data-stu-id="04e33-243">In Media Services, a media processor is a component that handles a specific processing task, such as encoding, format conversion, encrypting, or decrypting media content.</span></span> <span data-ttu-id="04e33-244">För hello kodning aktivitet som visas i den här självstudiekursen, vi toouse hello Media Encoder Standard.</span><span class="sxs-lookup"><span data-stu-id="04e33-244">For hello encoding task shown in this tutorial, we are going toouse hello Media Encoder Standard.</span></span>

<span data-ttu-id="04e33-245">hello följande kod begäranden hello kodare id.</span><span class="sxs-lookup"><span data-stu-id="04e33-245">hello following code requests hello encoder's id.</span></span>

<span data-ttu-id="04e33-246">**HTTP-begäran**</span><span class="sxs-lookup"><span data-stu-id="04e33-246">**HTTP Request**</span></span>

    GET https://wamsbayclus001rest-hs.cloudapp.net/api/MediaProcessors()?$filter=Name%20eq%20'Media%20Encoder%20Standard' HTTP/1.1
    DataServiceVersion: 1.0;NetFx
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json
    Accept-Charset: UTF-8
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=amstestaccount001&urn%3aSubscriptionId=f7f09258-6753-4ca2-b1ae-193798e2c9d8&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1421675491&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=9hUudHYnATpi5hN3cvTfgw%2bL4N3tL0fdsRnQnm6ZYIU%3d
    x-ms-version: 2.11
    Host: wamsbayclus001rest-hs.cloudapp.net


<span data-ttu-id="04e33-247">**HTTP-svar**</span><span class="sxs-lookup"><span data-stu-id="04e33-247">**HTTP Response**</span></span>

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

### <a name="create-a-job"></a><span data-ttu-id="04e33-248">Skapa ett jobb</span><span class="sxs-lookup"><span data-stu-id="04e33-248">Create a job</span></span>
<span data-ttu-id="04e33-249">Varje jobb kan ha en eller flera aktiviteter beroende på hello typ av bearbetning som du vill tooaccomplish.</span><span class="sxs-lookup"><span data-stu-id="04e33-249">Each Job can have one or more Tasks depending on hello type of processing that you want tooaccomplish.</span></span> <span data-ttu-id="04e33-250">Via hello REST-API, du kan skapa jobb och deras relaterade uppgifter i ett av två sätt: aktiviteter kan vara definierade infogad via hello uppgifter navigeringsegenskap på jobbet entiteter eller OData-batch-bearbetning.</span><span class="sxs-lookup"><span data-stu-id="04e33-250">Through hello REST API, you can create Jobs and their related Tasks in one of two ways: Tasks can be defined inline through hello Tasks navigation property on Job entities, or through OData batch processing.</span></span> <span data-ttu-id="04e33-251">hello Media Services SDK använder batch-bearbetning.</span><span class="sxs-lookup"><span data-stu-id="04e33-251">hello Media Services SDK uses batch processing.</span></span> <span data-ttu-id="04e33-252">Men för hello läsbarhet hello kodexempel i det här avsnittet, uppgifter som är definierade infogad.</span><span class="sxs-lookup"><span data-stu-id="04e33-252">However, for hello readability of hello code examples in this topic, tasks are defined inline.</span></span> <span data-ttu-id="04e33-253">Mer information om batchbearbetning finns [Open Data Protocol (OData) gruppbearbetning](http://www.odata.org/documentation/odata-version-3-0/batch-processing/).</span><span class="sxs-lookup"><span data-stu-id="04e33-253">For information on batch processing, see [Open Data Protocol (OData) Batch Processing](http://www.odata.org/documentation/odata-version-3-0/batch-processing/).</span></span>

<span data-ttu-id="04e33-254">hello som följande exempel visar hur toocreate och efter ett jobb med en uppgift ange tooencode en video på en viss upplösning och kvalitet.</span><span class="sxs-lookup"><span data-stu-id="04e33-254">hello following example shows you how toocreate and post a Job with one Task set tooencode a video at a specific resolution and quality.</span></span> <span data-ttu-id="04e33-255">hello följande dokumentationsavsnitt innehåller hello lista över alla hello [uppgift förinställningar](http://msdn.microsoft.com/library/mt269960) stöds av Media Encoder Standard hello-processor.</span><span class="sxs-lookup"><span data-stu-id="04e33-255">hello following documentation section contains hello list of all hello [task presets](http://msdn.microsoft.com/library/mt269960) supported by hello Media Encoder Standard processor.</span></span>  

<span data-ttu-id="04e33-256">**HTTP-begäran**</span><span class="sxs-lookup"><span data-stu-id="04e33-256">**HTTP Request**</span></span>

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

<span data-ttu-id="04e33-257">**HTTP-svar**</span><span class="sxs-lookup"><span data-stu-id="04e33-257">**HTTP Response**</span></span>

<span data-ttu-id="04e33-258">Om det lyckas, returneras hello efter svar:</span><span class="sxs-lookup"><span data-stu-id="04e33-258">If successful, hello following response is returned:</span></span>

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


<span data-ttu-id="04e33-259">Det finns några viktiga saker toonote varje jobb begäran:</span><span class="sxs-lookup"><span data-stu-id="04e33-259">There are a few important things toonote in any Job request:</span></span>

* <span data-ttu-id="04e33-260">TaskBody egenskaper måste använda literal XML toodefine hello antal indata eller utdata tillgångar som används av hello aktivitet.</span><span class="sxs-lookup"><span data-stu-id="04e33-260">TaskBody properties MUST use literal XML toodefine hello number of input, or output assets that are used by hello Task.</span></span> <span data-ttu-id="04e33-261">hello avsnittet innehåller hello XML Schema Definition för hello XML.</span><span class="sxs-lookup"><span data-stu-id="04e33-261">hello Task topic contains hello XML Schema Definition for hello XML.</span></span>
* <span data-ttu-id="04e33-262">I hello TaskBody definition, varje inre värde för <inputAsset> och <outputAsset> måste anges som JobInputAsset(value) eller JobOutputAsset(value).</span><span class="sxs-lookup"><span data-stu-id="04e33-262">In hello TaskBody definition, each inner value for <inputAsset> and <outputAsset> must be set as JobInputAsset(value) or JobOutputAsset(value).</span></span>
* <span data-ttu-id="04e33-263">En aktivitet kan innehålla flera utdata tillgångar.</span><span class="sxs-lookup"><span data-stu-id="04e33-263">A task can have multiple output assets.</span></span> <span data-ttu-id="04e33-264">En JobOutputAsset(x) kan bara användas en gång som utdata för en aktivitet i ett jobb.</span><span class="sxs-lookup"><span data-stu-id="04e33-264">One JobOutputAsset(x) can only be used once as an output of a task in a job.</span></span>
* <span data-ttu-id="04e33-265">Du kan ange JobInputAsset eller JobOutputAsset som inkommande tillgång för en aktivitet.</span><span class="sxs-lookup"><span data-stu-id="04e33-265">You can specify JobInputAsset or JobOutputAsset as an input asset of a task.</span></span>
* <span data-ttu-id="04e33-266">Aktiviteter måste inte utgör en cykel.</span><span class="sxs-lookup"><span data-stu-id="04e33-266">Tasks must not form a cycle.</span></span>
* <span data-ttu-id="04e33-267">Parametern för hello-värde som du skickar tooJobInputAsset eller JobOutputAsset representerar hello indexvärdet för en tillgång.</span><span class="sxs-lookup"><span data-stu-id="04e33-267">hello value parameter that you pass tooJobInputAsset or JobOutputAsset represents hello index value for an Asset.</span></span> <span data-ttu-id="04e33-268">hello har faktiska tillgångar definierats i hello InputMediaAssets och OutputMediaAssets navigeringsegenskaper på hello jobbdefinitionen för entiteten.</span><span class="sxs-lookup"><span data-stu-id="04e33-268">hello actual Assets are defined in hello InputMediaAssets and OutputMediaAssets navigation properties on hello Job entity definition.</span></span>

> [!NOTE]
> <span data-ttu-id="04e33-269">Eftersom Media Services bygger på OData v3 hello enskilda tillgångar i InputMediaAssets och OutputMediaAssets navigering egenskapsuppsättningar refereras via en ”__metadata: uri” namn / värde-par.</span><span class="sxs-lookup"><span data-stu-id="04e33-269">Because Media Services is built on OData v3, hello individual assets in InputMediaAssets and OutputMediaAssets navigation property collections are referenced through a "__metadata : uri" name-value pair.</span></span>
>
>

* <span data-ttu-id="04e33-270">InputMediaAssets mappar tooone eller flera resurser som du har skapat i Media Services.</span><span class="sxs-lookup"><span data-stu-id="04e33-270">InputMediaAssets maps tooone or more Assets you have created in Media Services.</span></span> <span data-ttu-id="04e33-271">OutputMediaAssets skapas med hello system.</span><span class="sxs-lookup"><span data-stu-id="04e33-271">OutputMediaAssets are created by hello system.</span></span> <span data-ttu-id="04e33-272">De hänvisar inte till en befintlig tillgång.</span><span class="sxs-lookup"><span data-stu-id="04e33-272">They do not reference an existing asset.</span></span>
* <span data-ttu-id="04e33-273">OutputMediaAssets kan namnges med hello assetName attribut.</span><span class="sxs-lookup"><span data-stu-id="04e33-273">OutputMediaAssets can be named using hello assetName attribute.</span></span> <span data-ttu-id="04e33-274">Om det här attributet är inte tillgängligt och sedan hello hello OutputMediaAsset heter det inre textvärdet hello hello <outputAsset> elementet är med suffixet hello jobbnamn värde eller hello jobb-Id-värde (i hello fall där hello namnegenskapen har inte definierats).</span><span class="sxs-lookup"><span data-stu-id="04e33-274">If this attribute is not present, then hello name of hello OutputMediaAsset is whatever hello inner text value of hello <outputAsset> element is with a suffix of either hello Job Name value, or hello Job Id value (in hello case where hello Name property isn't defined).</span></span> <span data-ttu-id="04e33-275">Till exempel om du anger ett värde för assetName för ”Sample”, och sedan hello OutputMediaAsset namnegenskapen skulle anges för ”Sample”.</span><span class="sxs-lookup"><span data-stu-id="04e33-275">For example, if you set a value for assetName too"Sample", then hello OutputMediaAsset Name property would be set too"Sample".</span></span> <span data-ttu-id="04e33-276">Men om du inte har angett ett värde för assetName men ställts in hello Jobbnamn är för ”NewJob” och sedan hello OutputMediaAsset namn ”_NewJob JobOutputAsset (värde)”.</span><span class="sxs-lookup"><span data-stu-id="04e33-276">However, if you did not set a value for assetName, but did set hello job name too"NewJob", then hello OutputMediaAsset Name would be "JobOutputAsset(value)_NewJob".</span></span>

    <span data-ttu-id="04e33-277">hello som följande exempel visar hur tooset hello assetName attribut:</span><span class="sxs-lookup"><span data-stu-id="04e33-277">hello following example shows how tooset hello assetName attribute:</span></span>

        "<?xml version=\"1.0\" encoding=\"utf-8\"?><taskBody><inputAsset>JobInputAsset(0)</inputAsset><outputAsset assetName=\"CustomOutputAssetName\">JobOutputAsset(0)</outputAsset></taskBody>"
* <span data-ttu-id="04e33-278">tooenable länkning för uppgiften:</span><span class="sxs-lookup"><span data-stu-id="04e33-278">tooenable task chaining:</span></span>

  * <span data-ttu-id="04e33-279">Ett jobb måste ha minst två aktiviteter</span><span class="sxs-lookup"><span data-stu-id="04e33-279">A job must have at least two tasks</span></span>
  * <span data-ttu-id="04e33-280">Det måste finnas minst en aktivitet vars indata är resultatet av en annan aktivitet i hello-jobbet.</span><span class="sxs-lookup"><span data-stu-id="04e33-280">There must be at least one task whose input is output of another task in hello job.</span></span>

<span data-ttu-id="04e33-281">Mer information finns i [skapar en kodning jobb med hello Media Services REST API](media-services-rest-encode-asset.md).</span><span class="sxs-lookup"><span data-stu-id="04e33-281">For more information see, [Creating an Encoding Job with hello Media Services REST API](media-services-rest-encode-asset.md).</span></span>

### <a name="monitor-processing-progress"></a><span data-ttu-id="04e33-282">Övervakaren bearbetning pågår</span><span class="sxs-lookup"><span data-stu-id="04e33-282">Monitor Processing Progress</span></span>
<span data-ttu-id="04e33-283">Du kan hämta hello jobbstatus med hello tillstånd egenskapen som visas i följande exempel hello.</span><span class="sxs-lookup"><span data-stu-id="04e33-283">You can retrieve hello Job status by using hello State property, as shown in hello following example.</span></span>

<span data-ttu-id="04e33-284">**HTTP-begäran**</span><span class="sxs-lookup"><span data-stu-id="04e33-284">**HTTP Request**</span></span>

    GET https://wamsbayclus001rest-hs.net/api/Jobs('nb%3Ajid%3AUUID%3A71d2dd33-efdf-ec43-8ea1-136a110bd42c')/State HTTP/1.1
    Content-Type: application/json;odata=verbose
    Accept: application/json;odata=verbose
    DataServiceVersion: 3.0
    MaxDataServiceVersion: 3.0
    x-ms-version: 2.11
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=youraccountname&urn%3aSubscriptionId=zf84471d-2233-4e75-aa09-010f0fc0cf5b&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1336908022&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=RYXOraO6Z%2f7l9whWZQN%2bypeijgHwIk8XyikA01Kx1%2bk%3d
    Host: wamsbayclus001rest-hs.net
    Content-Length: 0


<span data-ttu-id="04e33-285">**HTTP-svar**</span><span class="sxs-lookup"><span data-stu-id="04e33-285">**HTTP Response**</span></span>

<span data-ttu-id="04e33-286">Om det lyckas, returneras hello efter svar:</span><span class="sxs-lookup"><span data-stu-id="04e33-286">If successful, hello following response is returned:</span></span>

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


### <a name="cancel-a-job"></a><span data-ttu-id="04e33-287">Avbryta ett jobb</span><span class="sxs-lookup"><span data-stu-id="04e33-287">Cancel a job</span></span>
<span data-ttu-id="04e33-288">Media Services kan du toocancel jobb som körs via hello CancelJob funktion.</span><span class="sxs-lookup"><span data-stu-id="04e33-288">Media Services allows you toocancel running jobs through hello CancelJob function.</span></span> <span data-ttu-id="04e33-289">Det här anropet returnerar 400 felkoden om du försöker toocancel ett jobb när dess tillstånd har avbrutits, avbryta, fel eller slutförts.</span><span class="sxs-lookup"><span data-stu-id="04e33-289">This call returns a 400 error code if you try toocancel a Job when its state is canceled, canceling, error, or finished.</span></span>

<span data-ttu-id="04e33-290">följande exempel visar hur hello toocall CancelJob.</span><span class="sxs-lookup"><span data-stu-id="04e33-290">hello following example shows how toocall CancelJob.</span></span>

<span data-ttu-id="04e33-291">**HTTP-begäran**</span><span class="sxs-lookup"><span data-stu-id="04e33-291">**HTTP Request**</span></span>

    GET https://wamsbayclus001rest-hs.net/API/CancelJob?jobid='nb%3ajid%3aUUID%3a71d2dd33-efdf-ec43-8ea1-136a110bd42c' HTTP/1.1
    Content-Type: application/json;odata=verbose
    Accept: application/json;odata=verbose
    DataServiceVersion: 3.0
    MaxDataServiceVersion: 3.0
    x-ms-version: 2.2
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=youraccountname&urn%3aSubscriptionId=2f84471d-b1ae-4e75-aa09-010f0fc0cf5b&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1336908753&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=kolAgnFfbQIeRv4lWxKSFa4USyjWXRm15Kd%2bNd5g8eA%3d
    Host: wamsbayclus001rest-hs.net


<span data-ttu-id="04e33-292">Om det lyckas, returneras en 204-svarskod med ingen brödtext.</span><span class="sxs-lookup"><span data-stu-id="04e33-292">If successful, a 204 response code is returned with no message body.</span></span>

> [!NOTE]
> <span data-ttu-id="04e33-293">Du måste URL-koda hello jobb-id (normalt nb:jid:UUID: somevalue) när du skickar det som en parameter tooCancelJob.</span><span class="sxs-lookup"><span data-stu-id="04e33-293">You must URL-encode hello job id (normally nb:jid:UUID: somevalue) when passing it in as a parameter tooCancelJob.</span></span>
>
>

### <a name="get-hello-output-asset"></a><span data-ttu-id="04e33-294">Hämta hello utdatatillgången</span><span class="sxs-lookup"><span data-stu-id="04e33-294">Get hello output asset</span></span>
<span data-ttu-id="04e33-295">hello följande kod visar hur toorequest hello utdata tillgången Id.</span><span class="sxs-lookup"><span data-stu-id="04e33-295">hello following code shows how toorequest hello output asset Id.</span></span>

<span data-ttu-id="04e33-296">**HTTP-begäran**</span><span class="sxs-lookup"><span data-stu-id="04e33-296">**HTTP Request**</span></span>

    GET https://wamsbayclus001rest-hs.cloudapp.net/api/Jobs('nb%3Ajid%3AUUID%3A71d2dd33-efdf-ec43-8ea1-136a110bd42c')/OutputMediaAssets() HTTP/1.1
    DataServiceVersion: 1.0;NetFx
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json
    Accept-Charset: UTF-8
    User-Agent: Microsoft ADO.NET Data Services
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=amstestaccount001&urn%3aSubscriptionId=z7f09258-2233-4ca2-b1ae-193798e2c9d8&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1421675491&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=9hUudHYnATpi5hN3cvTfgw%2bL4N3tL0fdsRnQnm6ZYIU%3d
    x-ms-version: 2.11
    Host: wamsbayclus001rest-hs.cloudapp.net


<span data-ttu-id="04e33-297">**HTTP-svar**</span><span class="sxs-lookup"><span data-stu-id="04e33-297">**HTTP Response**</span></span>

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



## <span data-ttu-id="04e33-298"><a id="publish_get_urls"></a>Publicera hello tillgången och get strömning och progressiv nedladdning URL: er med REST API</span><span class="sxs-lookup"><span data-stu-id="04e33-298"><a id="publish_get_urls"></a>Publish hello asset and get streaming and progressive download URLs with REST API</span></span>

<span data-ttu-id="04e33-299">toostream eller hämta en tillgång, du behöver för ”publicera” den genom att skapa en positionerare.</span><span class="sxs-lookup"><span data-stu-id="04e33-299">toostream or download an asset, you first need too"publish" it by creating a locator.</span></span> <span data-ttu-id="04e33-300">Positionerare ger åtkomst toofiles i hello tillgången.</span><span class="sxs-lookup"><span data-stu-id="04e33-300">Locators provide access toofiles contained in hello asset.</span></span> <span data-ttu-id="04e33-301">Media Services stöder två typer av positionerare: OnDemandOrigin-positionerare som används toostream media (till exempel MPEG DASH, HLS eller Smooth Streaming) och signatur åtkomst (SAS)-positionerare används toodownload mediefiler.</span><span class="sxs-lookup"><span data-stu-id="04e33-301">Media Services supports two types of locators: OnDemandOrigin locators, used toostream media (for example, MPEG DASH, HLS, or Smooth Streaming) and Access Signature (SAS) locators, used toodownload media files.</span></span> <span data-ttu-id="04e33-302">Mer information om SAS finns lokaliserare [detta](http://southworks.com/blog/2015/05/27/reusing-azure-media-services-locators-to-avoid-facing-the-5-shared-access-policy-limitation/) blogg.</span><span class="sxs-lookup"><span data-stu-id="04e33-302">For more information about SAS locators see [this](http://southworks.com/blog/2015/05/27/reusing-azure-media-services-locators-to-avoid-facing-the-5-shared-access-policy-limitation/) blog.</span></span>

<span data-ttu-id="04e33-303">När du skapar hello positionerare kan skapa du hello URL: er som används toostream eller hämta dina filer.</span><span class="sxs-lookup"><span data-stu-id="04e33-303">Once you create hello locators, you can build hello URLs that are used toostream or download your files.</span></span>

>[!NOTE]
><span data-ttu-id="04e33-304">När AMS-kontot skapas en **standard** strömningsslutpunkt har lagts till tooyour konto i hello **stoppad** tillstånd.</span><span class="sxs-lookup"><span data-stu-id="04e33-304">When your AMS account is created a **default** streaming endpoint is added tooyour account in hello **Stopped** state.</span></span> <span data-ttu-id="04e33-305">toostart strömning ditt innehåll och dra nytta av dynamisk paketering och dynamisk kryptering hello strömningsslutpunkt som du vill toostream innehåll har toobe i hello **kör** tillstånd.</span><span class="sxs-lookup"><span data-stu-id="04e33-305">toostart streaming your content and take advantage of dynamic packaging and dynamic encryption, hello streaming endpoint from which you want toostream content has toobe in hello **Running** state.</span></span>

<span data-ttu-id="04e33-306">En strömmande URL för Smooth Streaming har hello följande format:</span><span class="sxs-lookup"><span data-stu-id="04e33-306">A streaming URL for Smooth Streaming has hello following format:</span></span>

    {streaming endpoint name-media services account name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest

<span data-ttu-id="04e33-307">En strömmande URL för HLS har hello följande format:</span><span class="sxs-lookup"><span data-stu-id="04e33-307">A streaming URL for HLS has hello following format:</span></span>

    {streaming endpoint name-media services account name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest(format=m3u8-aapl)

<span data-ttu-id="04e33-308">En strömmande URL för MPEG DASH har hello följande format:</span><span class="sxs-lookup"><span data-stu-id="04e33-308">A streaming URL for MPEG DASH has hello following format:</span></span>

    {streaming endpoint name-media services account name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest(format=mpd-time-csf)


<span data-ttu-id="04e33-309">En SAS-URL som används för toodownload filer har hello följande format:</span><span class="sxs-lookup"><span data-stu-id="04e33-309">A SAS URL used toodownload files has hello following format:</span></span>

    {blob container name}/{asset name}/{file name}/{SAS signature}

<span data-ttu-id="04e33-310">Det här avsnittet visar hur tooperform hello följande uppgifter krävs för ”publicera” dina tillgångar.</span><span class="sxs-lookup"><span data-stu-id="04e33-310">This section shows how tooperform hello following tasks necessary too"publish" your assets.</span></span>  

* <span data-ttu-id="04e33-311">Skapa hello AccessPolicy med läsbehörighet</span><span class="sxs-lookup"><span data-stu-id="04e33-311">Creating hello AccessPolicy with read permission</span></span>
* <span data-ttu-id="04e33-312">Skapa en SAS-URL för hämtning av innehåll</span><span class="sxs-lookup"><span data-stu-id="04e33-312">Creating a SAS URL for downloading content</span></span>
* <span data-ttu-id="04e33-313">Skapa ett ursprung URL för direktuppspelning av innehåll</span><span class="sxs-lookup"><span data-stu-id="04e33-313">Creating an origin URL for streaming content</span></span>

### <a name="creating-hello-accesspolicy-with-read-permission"></a><span data-ttu-id="04e33-314">Skapa hello AccessPolicy med läsbehörighet</span><span class="sxs-lookup"><span data-stu-id="04e33-314">Creating hello AccessPolicy with read permission</span></span>
<span data-ttu-id="04e33-315">Innan du laddar ned eller strömmande mediainnehåll först definiera ett AccessPolicy med läsbehörighet och skapa hello lämpliga lokaliserare entitet som anger hello leveransmekanismen som du vill tooenable för klienterna.</span><span class="sxs-lookup"><span data-stu-id="04e33-315">Before downloading or streaming any media content, first define an AccessPolicy with read permissions and create hello appropriate Locator entity that specifies hello type of delivery mechanism you want tooenable for your clients.</span></span> <span data-ttu-id="04e33-316">Mer information om hello-egenskaper som är tillgängliga finns [AccessPolicy Entitetsegenskaper](https://docs.microsoft.com/rest/api/media/operations/accesspolicy#accesspolicy_properties).</span><span class="sxs-lookup"><span data-stu-id="04e33-316">For more information on hello properties available, see [AccessPolicy Entity Properties](https://docs.microsoft.com/rest/api/media/operations/accesspolicy#accesspolicy_properties).</span></span>

<span data-ttu-id="04e33-317">hello som följande exempel visar hur toospecify hello AccessPolicy för läsbehörighet för den angivna resursen.</span><span class="sxs-lookup"><span data-stu-id="04e33-317">hello following example shows how toospecify hello AccessPolicy for read permissions for a given Asset.</span></span>

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

<span data-ttu-id="04e33-318">Om det lyckas, returneras 201 koden som beskriver hello AccessPolicy entitet som du skapade.</span><span class="sxs-lookup"><span data-stu-id="04e33-318">If successful, a 201 success code is returned describing hello AccessPolicy entity that you created.</span></span> <span data-ttu-id="04e33-319">Du kan sedan använda hello AccessPolicy Id tillsammans med hello tillgångsnummer hello tillgång som innehåller hello fil toodeliver (till exempel en utdatatillgången) toocreate hello lokaliserare entitet.</span><span class="sxs-lookup"><span data-stu-id="04e33-319">You then use hello AccessPolicy Id along with hello Asset Id of hello asset that contains hello file you want toodeliver(such as an output asset) toocreate hello Locator entity.</span></span>

> [!NOTE]
> <span data-ttu-id="04e33-320">Det här grundläggande arbetsflödet är hello samma som överför en fil när du vill föra in en tillgång (som beskrevs tidigare i det här avsnittet).</span><span class="sxs-lookup"><span data-stu-id="04e33-320">This basic workflow is hello same as uploading a file when ingesting an Asset (as was discussed earlier in this topic).</span></span> <span data-ttu-id="04e33-321">Ange dessutom StartTime värdet toofive minuter innan hello klockslaget som ladda upp filer, om du (eller klienterna) behöver tooaccess filerna direkt.</span><span class="sxs-lookup"><span data-stu-id="04e33-321">Also, like uploading files, if you (or your clients) need tooaccess your files immediately, set your StartTime value toofive minutes before hello current time.</span></span> <span data-ttu-id="04e33-322">Den här åtgärden är nödvändigt eftersom det kan finnas klockan skeva mellan hello-klienten och Media Services.</span><span class="sxs-lookup"><span data-stu-id="04e33-322">This action is necessary because there may be clock skew between hello client and Media Services.</span></span> <span data-ttu-id="04e33-323">hello StartTime-värdet måste vara i hello följande DateTime-format: ÅÅÅÅ-MM-ddTHH (till exempel ”2014-05-23T17:53:50Z”).</span><span class="sxs-lookup"><span data-stu-id="04e33-323">hello StartTime value must be in hello following DateTime format: YYYY-MM-DDTHH:mm:ssZ (for example, "2014-05-23T17:53:50Z").</span></span>
>
>

### <a name="creating-a-sas-url-for-downloading-content"></a><span data-ttu-id="04e33-324">Skapa en SAS-URL för hämtning av innehåll</span><span class="sxs-lookup"><span data-stu-id="04e33-324">Creating a SAS URL for downloading content</span></span>
<span data-ttu-id="04e33-325">hello följande kod visar hur tooget en URL som kan använda toodownload en mediefil skapas och överföra tidigare.</span><span class="sxs-lookup"><span data-stu-id="04e33-325">hello following code shows how tooget a URL that can be used toodownload a media file created and uploaded previously.</span></span> <span data-ttu-id="04e33-326">Hej AccessPolicy har skrivskyddad behörighet som anges och hello lokaliserare sökvägen refererar tooa SAS nedladdnings-URL.</span><span class="sxs-lookup"><span data-stu-id="04e33-326">hello AccessPolicy has read permissions set and hello Locator path refers tooa SAS download URL.</span></span>

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

<span data-ttu-id="04e33-327">Om det lyckas, returneras hello efter svar:</span><span class="sxs-lookup"><span data-stu-id="04e33-327">If successful, hello following response is returned:</span></span>

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


<span data-ttu-id="04e33-328">hello returnerade **sökväg** egenskap innehåller hello SAS-URL.</span><span class="sxs-lookup"><span data-stu-id="04e33-328">hello returned **Path** property contains hello SAS URL.</span></span>

> [!NOTE]
> <span data-ttu-id="04e33-329">Om du hämtar lagringskrypterad innehåll, måste du manuellt dekryptera den innan du gör den eller använda hello lagring dekryptering MediaProcessor i en bearbetning uppgiften toooutput bearbetas filer i hello Rensa tooan OutputAsset och sedan ladda ned från tillgången.</span><span class="sxs-lookup"><span data-stu-id="04e33-329">If you download storage encrypted content, you must manually decrypt it before rendering it, or use hello Storage Decryption MediaProcessor in a processing task toooutput processed files in hello clear tooan OutputAsset and then download from that Asset.</span></span> <span data-ttu-id="04e33-330">Mer information om bearbetningen finns i skapar en kodning jobb med hello Media Services REST API.</span><span class="sxs-lookup"><span data-stu-id="04e33-330">For more information on processing, see Creating an Encoding Job with hello Media Services REST API.</span></span> <span data-ttu-id="04e33-331">Lokaliserare för SAS-URL: en kan inte uppdateras när de har skapats.</span><span class="sxs-lookup"><span data-stu-id="04e33-331">Also, SAS URL Locators cannot be updated after they have been created.</span></span> <span data-ttu-id="04e33-332">Du kan exempelvis återanvända hello samma lokaliserare med ett uppdaterat StartTime-värde.</span><span class="sxs-lookup"><span data-stu-id="04e33-332">For example, you cannot reuse hello same Locator with an updated StartTime value.</span></span> <span data-ttu-id="04e33-333">Detta är på grund av hello sätt SAS-URL: er skapas.</span><span class="sxs-lookup"><span data-stu-id="04e33-333">This is because of hello way SAS URLs are created.</span></span> <span data-ttu-id="04e33-334">Om du vill tooaccess en tillgång för att ladda ned efter en positionerare har upphört att gälla, måste du skapa en ny med en ny StartTime.</span><span class="sxs-lookup"><span data-stu-id="04e33-334">If you want tooaccess an asset for download after a Locator has expired, then you must create a new one with a new StartTime.</span></span>
>
>

### <a name="download-files"></a><span data-ttu-id="04e33-335">Hämta filer</span><span class="sxs-lookup"><span data-stu-id="04e33-335">Download files</span></span>
<span data-ttu-id="04e33-336">Du kan hämta filer med hjälp av hello Azure Storage REST API: er när du har hello AccessPolicy och lokaliserare uppsättningen.</span><span class="sxs-lookup"><span data-stu-id="04e33-336">Once you have hello AccessPolicy and Locator set, you can download files using hello Azure Storage REST APIs.</span></span>  

> [!NOTE]
> <span data-ttu-id="04e33-337">Du måste lägga till hello filnamn hello filen du vill ha toodownload toohello lokaliserare **sökväg** värde togs emot i hello föregående avsnitt.</span><span class="sxs-lookup"><span data-stu-id="04e33-337">You must add hello file name for hello file you want toodownload toohello Locator **Path** value received in hello previous section.</span></span> <span data-ttu-id="04e33-338">Till exempel https://storagetestaccount001.blob.core.windows.net/asset-e7b02da4-5a69-40e7-a8db-e8f4f697aac0/BigBuckBunny.mp4?</span><span class="sxs-lookup"><span data-stu-id="04e33-338">For example, https://storagetestaccount001.blob.core.windows.net/asset-e7b02da4-5a69-40e7-a8db-e8f4f697aac0/BigBuckBunny.mp4?</span></span> <span data-ttu-id="04e33-339">.</span><span class="sxs-lookup"><span data-stu-id="04e33-339">.</span></span> <span data-ttu-id="04e33-340">.</span><span class="sxs-lookup"><span data-stu-id="04e33-340">.</span></span> <span data-ttu-id="04e33-341">.</span><span class="sxs-lookup"><span data-stu-id="04e33-341">.</span></span>
>
>

<span data-ttu-id="04e33-342">Mer information om hur du arbetar med Azure storage blobs finns [REST-API för Blob-tjänsten](https://docs.microsoft.com/rest/api/storageservices/Blob-Service-REST-API).</span><span class="sxs-lookup"><span data-stu-id="04e33-342">For more information on working with Azure storage blobs, see [Blob Service REST API](https://docs.microsoft.com/rest/api/storageservices/Blob-Service-REST-API).</span></span>

<span data-ttu-id="04e33-343">På grund av hello kodning jobb som du utförde tidigare (kodning till anpassningsbar MP4-uppsättningen), har du flera MP4-filer som du kan hämta progressivt.</span><span class="sxs-lookup"><span data-stu-id="04e33-343">As a result of hello encoding job that you performed earlier (encoding into Adaptive MP4 set), you have multiple MP4 files that you can progressively download.</span></span> <span data-ttu-id="04e33-344">Exempel:</span><span class="sxs-lookup"><span data-stu-id="04e33-344">For example:</span></span>    

    https://storagetestaccount001.blob.core.windows.net/asset-38058602-a4b8-4b33-b9f0-6880dc1490ea/BigBuckBunny_H264_650kbps_AAC_und_ch2_96kbps.mp4?sv=2012-02-12&sr=c&si=166d5154-b801-410b-a226-ee2f8eac1929&sig=P2iNZJAvAWpp%2Bj9yV6TQjoz5DIIaj7ve8ARynmEM6Xk%3D&se=2015-02-14T01:13:05Z

    https://storagetestaccount001.blob.core.windows.net/asset-38058602-a4b8-4b33-b9f0-6880dc1490ea/BigBuckBunny_H264_400kbps_AAC_und_ch2_96kbps.mp4?sv=2012-02-12&sr=c&si=166d5154-b801-410b-a226-ee2f8eac1929&sig=P2iNZJAvAWpp%2Bj9yV6TQjoz5DIIaj7ve8ARynmEM6Xk%3D&se=2015-02-14T01:13:05Z

    https://storagetestaccount001.blob.core.windows.net/asset-38058602-a4b8-4b33-b9f0-6880dc1490ea/BigBuckBunny_H264_3400kbps_AAC_und_ch2_96kbps.mp4?sv=2012-02-12&sr=c&si=166d5154-b801-410b-a226-ee2f8eac1929&sig=P2iNZJAvAWpp%2Bj9yV6TQjoz5DIIaj7ve8ARynmEM6Xk%3D&se=2015-02-14T01:13:05Z

    https://storagetestaccount001.blob.core.windows.net/asset-38058602-a4b8-4b33-b9f0-6880dc1490ea/BigBuckBunny_H264_2250kbps_AAC_und_ch2_96kbps.mp4?sv=2012-02-12&sr=c&si=166d5154-b801-410b-a226-ee2f8eac1929&sig=P2iNZJAvAWpp%2Bj9yV6TQjoz5DIIaj7ve8ARynmEM6Xk%3D&se=2015-02-14T01:13:05Z

    https://storagetestaccount001.blob.core.windows.net/asset-38058602-a4b8-4b33-b9f0-6880dc1490ea/BigBuckBunny_H264_1500kbps_AAC_und_ch2_96kbps.mp4?sv=2012-02-12&sr=c&si=166d5154-b801-410b-a226-ee2f8eac1929&sig=P2iNZJAvAWpp%2Bj9yV6TQjoz5DIIaj7ve8ARynmEM6Xk%3D&se=2015-02-14T01:13:05Z

    https://storagetestaccount001.blob.core.windows.net/asset-38058602-a4b8-4b33-b9f0-6880dc1490ea/BigBuckBunny_H264_1000kbps_AAC_und_ch2_96kbps.mp4?sv=2012-02-12&sr=c&si=166d5154-b801-410b-a226-ee2f8eac1929&sig=P2iNZJAvAWpp%2Bj9yV6TQjoz5DIIaj7ve8ARynmEM6Xk%3D&se=2015-02-14T01:13:05Z

    https://storagetestaccount001.blob.core.windows.net/asset-38058602-a4b8-4b33-b9f0-6880dc1490ea/BigBuckBunny_AAC_und_ch2_96kbps.mp4?sv=2012-02-12&sr=c&si=166d5154-b801-410b-a226-ee2f8eac1929&sig=P2iNZJAvAWpp%2Bj9yV6TQjoz5DIIaj7ve8ARynmEM6Xk%3D&se=2015-02-14T01:13:05Z

    https://storagetestaccount001.blob.core.windows.net/asset-38058602-a4b8-4b33-b9f0-6880dc1490ea/BigBuckBunny_AAC_und_ch2_56kbps.mp4?sv=2012-02-12&sr=c&si=166d5154-b801-410b-a226-ee2f8eac1929&sig=P2iNZJAvAWpp%2Bj9yV6TQjoz5DIIaj7ve8ARynmEM6Xk%3D&se=2015-02-14T01:13:05Z


### <a name="creating-a-streaming-url-for-streaming-content"></a><span data-ttu-id="04e33-345">Skapa en strömnings-URL för direktuppspelning av innehåll</span><span class="sxs-lookup"><span data-stu-id="04e33-345">Creating a streaming URL for streaming content</span></span>
<span data-ttu-id="04e33-346">Hej följande kod visar hur toocreate en Strömningslokaliserare URL:</span><span class="sxs-lookup"><span data-stu-id="04e33-346">hello following code shows how toocreate a streaming URL Locator:</span></span>

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

<span data-ttu-id="04e33-347">Om det lyckas, returneras hello efter svar:</span><span class="sxs-lookup"><span data-stu-id="04e33-347">If successful, hello following response is returned:</span></span>

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

<span data-ttu-id="04e33-348">toostream en Smooth Streaming ursprung URL i en strömmande media player, måste du lägga till hello sökväg egenskap med namnet hello hello Smooth Streaming manifestfilen, följt av ”/ manifest”.</span><span class="sxs-lookup"><span data-stu-id="04e33-348">toostream a Smooth Streaming origin URL in a streaming media player, you must append hello Path property with hello name of hello Smooth Streaming manifest file, followed by "/manifest".</span></span>

    http://amstestaccount001.streaming.mediaservices.windows.net/ebf733c4-3e2e-4a68-b67b-cc5159d1d7f2/BigBuckBunny.ism/manifest

<span data-ttu-id="04e33-349">toostream HLS, Lägg till (format = m3u8 aapl) efter hello ”/ manifest”.</span><span class="sxs-lookup"><span data-stu-id="04e33-349">toostream HLS, append (format=m3u8-aapl) after hello "/manifest".</span></span>

    http://amstestaccount001.streaming.mediaservices.windows.net/ebf733c4-3e2e-4a68-b67b-cc5159d1d7f2/BigBuckBunny.ism/manifest(format=m3u8-aapl)

<span data-ttu-id="04e33-350">toostream MPEG DASH, Lägg till (format = mpd-tid-csf) efter hello ”/ manifest”.</span><span class="sxs-lookup"><span data-stu-id="04e33-350">toostream MPEG DASH, append (format=mpd-time-csf) after hello "/manifest".</span></span>

    http://amstestaccount001.streaming.mediaservices.windows.net/ebf733c4-3e2e-4a68-b67b-cc5159d1d7f2/BigBuckBunny.ism/manifest(format=mpd-time-csf)


## <span data-ttu-id="04e33-351"><a id="play"></a>Spela upp ditt innehåll</span><span class="sxs-lookup"><span data-stu-id="04e33-351"><a id="play"></a>Play your content</span></span>
<span data-ttu-id="04e33-352">toostream du video-använder [Azure Media Services Player](http://amsplayer.azurewebsites.net/azuremediaplayer.html).</span><span class="sxs-lookup"><span data-stu-id="04e33-352">toostream you video, use [Azure Media Services Player](http://amsplayer.azurewebsites.net/azuremediaplayer.html).</span></span>

<span data-ttu-id="04e33-353">tootest progressiv hämtning, klistra in en URL i en webbläsare (till exempel Internet Explorer, Chrome, Safari).</span><span class="sxs-lookup"><span data-stu-id="04e33-353">tootest progressive download, paste a URL into a browser (for example, IE, Chrome, Safari).</span></span>

## <a name="next-steps-media-services-learning-paths"></a><span data-ttu-id="04e33-354">Nästa steg: sökvägar för Media Services-utbildning</span><span class="sxs-lookup"><span data-stu-id="04e33-354">Next Steps: Media Services learning paths</span></span>
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a><span data-ttu-id="04e33-355">Ge feedback</span><span class="sxs-lookup"><span data-stu-id="04e33-355">Provide feedback</span></span>
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]
