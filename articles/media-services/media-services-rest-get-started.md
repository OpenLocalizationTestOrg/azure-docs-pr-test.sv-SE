---
title: "Komma igång med att leverera innehåll på begäran med hjälp av REST | Microsoft Docs"
description: "Den här självstudiekursen vägleder dig genom stegen för att implementera ett program för leverans av på begäran med Azure Media Services med hjälp av REST API."
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
ms.openlocfilehash: f304f7671465862123f64c8b0f9af95a7c828cc2
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/29/2017
---
# <a name="get-started-with-delivering-content-on-demand-using-rest"></a><span data-ttu-id="6d633-103">Komma igång med att leverera innehåll på begäran med hjälp av REST</span><span class="sxs-lookup"><span data-stu-id="6d633-103">Get started with delivering content on demand using REST</span></span>
[!INCLUDE [media-services-selector-get-started](../../includes/media-services-selector-get-started.md)]

<span data-ttu-id="6d633-104">Denna Snabbstart vägleder dig genom stegen för att implementera ett program för leverans av Video-on-Demand (VoD) med Azure Media Services (AMS) REST API: er.</span><span class="sxs-lookup"><span data-stu-id="6d633-104">This quickstart walks you through the steps of implementing a Video-on-Demand (VoD) content delivery application using Azure Media Services (AMS) REST APIs.</span></span>

<span data-ttu-id="6d633-105">Självstudierna innehåller det grundläggande Media Services-arbetsflödet och de vanligaste programmeringsobjekt och -uppgifter som krävs för utveckling av Media Services.</span><span class="sxs-lookup"><span data-stu-id="6d633-105">The tutorial introduces the basic Media Services workflow and the most common programming objects and tasks required for Media Services development.</span></span> <span data-ttu-id="6d633-106">I slutet av självstudierna kommer du att kunna strömma eller progressivt hämta en exempelmediefil som du har överfört, kodat och hämtat.</span><span class="sxs-lookup"><span data-stu-id="6d633-106">At the completion of the tutorial, you will be able to stream or progressively download a sample media file that you uploaded, encoded, and downloaded.</span></span>

<span data-ttu-id="6d633-107">Följande bild visar några av de vanligast använda objekten när du utvecklar VoD-program mot Media Services OData-modellen.</span><span class="sxs-lookup"><span data-stu-id="6d633-107">The following image shows some of the most commonly used objects when developing VoD applications against the Media Services OData model.</span></span>

<span data-ttu-id="6d633-108">Klicka på bilden för att visa den i full storlek.</span><span class="sxs-lookup"><span data-stu-id="6d633-108">Click the image to view it full size.</span></span>  

<a href="./media/media-services-rest-get-started/media-services-overview-object-model.png" target="_blank"><img src="./media/media-services-rest-get-started/media-services-overview-object-model-small.png"></a> 

## <a name="prerequisites"></a><span data-ttu-id="6d633-109">Krav</span><span class="sxs-lookup"><span data-stu-id="6d633-109">Prerequisites</span></span>
<span data-ttu-id="6d633-110">Följande förutsättningar krävs för att börja utveckla med Media Services med REST API: er.</span><span class="sxs-lookup"><span data-stu-id="6d633-110">The following prerequisites are required to start developing with Media Services with REST APIs.</span></span>

* <span data-ttu-id="6d633-111">Ett Azure-konto.</span><span class="sxs-lookup"><span data-stu-id="6d633-111">An Azure account.</span></span> <span data-ttu-id="6d633-112">Mer information om den [kostnadsfria utvärderingsversionen av Azure](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="6d633-112">For details, see [Azure Free Trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>
* <span data-ttu-id="6d633-113">Ett Media Services-konto.</span><span class="sxs-lookup"><span data-stu-id="6d633-113">A Media Services account.</span></span> <span data-ttu-id="6d633-114">Information om hur du skapar ett Media Services-konto finns i [Så här skapar du ett Media Services-konto](media-services-portal-create-account.md).</span><span class="sxs-lookup"><span data-stu-id="6d633-114">To create a Media Services account, see [How to Create a Media Services Account](media-services-portal-create-account.md).</span></span>
* <span data-ttu-id="6d633-115">Förståelse för hur du utvecklar med Media Services REST API.</span><span class="sxs-lookup"><span data-stu-id="6d633-115">Understanding of how to develop with Media Services REST API.</span></span> <span data-ttu-id="6d633-116">Mer information finns i [Media Services REST API-översikt](media-services-rest-how-to-use.md).</span><span class="sxs-lookup"><span data-stu-id="6d633-116">For more information, see [Media Services REST API overview](media-services-rest-how-to-use.md).</span></span>
* <span data-ttu-id="6d633-117">Ett program som kan skicka HTTP-begäranden och -svar.</span><span class="sxs-lookup"><span data-stu-id="6d633-117">An application of your choice that can send HTTP requests and responses.</span></span> <span data-ttu-id="6d633-118">Den här kursen använder [Fiddler](http://www.telerik.com/download/fiddler).</span><span class="sxs-lookup"><span data-stu-id="6d633-118">This tutorial uses [Fiddler](http://www.telerik.com/download/fiddler).</span></span>

<span data-ttu-id="6d633-119">Följande aktiviteter visas i den här snabbstarten.</span><span class="sxs-lookup"><span data-stu-id="6d633-119">The following tasks are shown in this quickstart.</span></span>

1. <span data-ttu-id="6d633-120">Starta slutpunkt för direktuppspelning (med hjälp av Azure Portal).</span><span class="sxs-lookup"><span data-stu-id="6d633-120">Start streaming endpoint (using the Azure portal).</span></span>
2. <span data-ttu-id="6d633-121">Ansluta till Media Services-konto med REST API.</span><span class="sxs-lookup"><span data-stu-id="6d633-121">Connect to the Media Services account with REST API.</span></span>
3. <span data-ttu-id="6d633-122">Skapa en ny tillgång och överföra en videofil med REST API.</span><span class="sxs-lookup"><span data-stu-id="6d633-122">Create a new asset and upload a video file with REST API.</span></span>
4. <span data-ttu-id="6d633-123">Koda källfilen till en uppsättning filer med anpassningsbar bithastighet MP4 med REST API.</span><span class="sxs-lookup"><span data-stu-id="6d633-123">Encode the source file into a set of adaptive bitrate MP4 files with REST API.</span></span>
5. <span data-ttu-id="6d633-124">Publicera tillgången och hämta strömning och progressiv överföring URL: er med REST API.</span><span class="sxs-lookup"><span data-stu-id="6d633-124">Publish the asset and get streaming and progressive download URLs with REST API.</span></span>
6. <span data-ttu-id="6d633-125">Spela upp ditt innehåll.</span><span class="sxs-lookup"><span data-stu-id="6d633-125">Play your content.</span></span>

>[!NOTE]
><span data-ttu-id="6d633-126">Det finns en gräns på 1 000 000 principer för olika AMS-principer (till exempel för positionerarprincipen eller ContentKeyAuthorizationPolicy).</span><span class="sxs-lookup"><span data-stu-id="6d633-126">There is a limit of 1,000,000 policies for different AMS policies (for example, for Locator policy or ContentKeyAuthorizationPolicy).</span></span> <span data-ttu-id="6d633-127">Du bör använda samma princip-ID om du alltid använder samma dagar/åtkomstbehörigheter, till exempel principer för positionerare som är avsedda att vara på plats under en längre tid (icke-överföringsprinciper).</span><span class="sxs-lookup"><span data-stu-id="6d633-127">You should use the same policy ID if you are always using the same days / access permissions, for example, policies for locators that are intended to remain in place for a long time (non-upload policies).</span></span> <span data-ttu-id="6d633-128">Mer information finns i [detta](media-services-dotnet-manage-entities.md#limit-access-policies) avsnitt.</span><span class="sxs-lookup"><span data-stu-id="6d633-128">For more information, see [this](media-services-dotnet-manage-entities.md#limit-access-policies) topic.</span></span>

<span data-ttu-id="6d633-129">Mer information om AMS REST-entiteter som används i det här avsnittet finns [Azure Media Services REST API-referens](/rest/api/media/services/azure-media-services-rest-api-reference).</span><span class="sxs-lookup"><span data-stu-id="6d633-129">For details about AMS REST entities used in this topic, see [Azure Media Services REST API Reference](/rest/api/media/services/azure-media-services-rest-api-reference).</span></span> <span data-ttu-id="6d633-130">Se även [Azure Media Services-koncepten](media-services-concepts.md).</span><span class="sxs-lookup"><span data-stu-id="6d633-130">Also, see [Azure Media Services concepts](media-services-concepts.md).</span></span>

>[!NOTE]
><span data-ttu-id="6d633-131">Vid åtkomst till entiteter i Media Services måste du ange specifika namn på huvudfält och värden i HTTP-begäranden.</span><span class="sxs-lookup"><span data-stu-id="6d633-131">When accessing entities in Media Services, you must set specific header fields and values in your HTTP requests.</span></span> <span data-ttu-id="6d633-132">Mer information finns i [installationsprogrammet för Media Services REST API-utveckling](media-services-rest-how-to-use.md).</span><span class="sxs-lookup"><span data-stu-id="6d633-132">For more information, see [Setup for Media Services REST API Development](media-services-rest-how-to-use.md).</span></span>

## <a name="start-streaming-endpoints-using-the-azure-portal"></a><span data-ttu-id="6d633-133">Starta slutpunkter för direktuppspelning med Azure Portal</span><span class="sxs-lookup"><span data-stu-id="6d633-133">Start streaming endpoints using the Azure portal</span></span>

<span data-ttu-id="6d633-134">När du arbetar med Azure Media Services är ett av de vanligaste scenarierna att leverera video via direktuppspelning med anpassningsbar bithastighet.</span><span class="sxs-lookup"><span data-stu-id="6d633-134">When working with Azure Media Services one of the most common scenarios is delivering video via adaptive bitrate streaming.</span></span> <span data-ttu-id="6d633-135">Media Services tillhandahåller en dynamisk paketering som gör att du kan leverera ditt MP4-kodade innehåll med anpassningsbar bithastighet i direktuppspelningsformat som stöds av Media Services (MPEG DASH, HLS, Smooth Streaming) direkt när du så önskar, utan att du behöver lagra på förhand paketerade versioner av vart och ett av dessa direktuppspelningsformat.</span><span class="sxs-lookup"><span data-stu-id="6d633-135">Media Services provides dynamic packaging, which allows you to deliver your adaptive bitrate MP4 encoded content in streaming formats supported by Media Services (MPEG DASH, HLS, Smooth Streaming) just-in-time, without you having to store pre-packaged versions of each of these streaming formats.</span></span>

>[!NOTE]
><span data-ttu-id="6d633-136">När ditt AMS-konto skapas läggs en **standard**-slutpunkt för direktuppspelning till på ditt konto med tillståndet **Stoppad**.</span><span class="sxs-lookup"><span data-stu-id="6d633-136">When your AMS account is created a **default** streaming endpoint is added to your account in the **Stopped** state.</span></span> <span data-ttu-id="6d633-137">Om du vill starta direktuppspelning av innehåll och dra nytta av dynamisk paketering och dynamisk kryptering måste slutpunkten för direktuppspelning som du vill spela upp innehåll från ha tillståndet **Körs**.</span><span class="sxs-lookup"><span data-stu-id="6d633-137">To start streaming your content and take advantage of dynamic packaging and dynamic encryption, the streaming endpoint from which you want to stream content has to be in the **Running** state.</span></span>

<span data-ttu-id="6d633-138">Starta slutpunkten för direktuppspelning genom att göra följande:</span><span class="sxs-lookup"><span data-stu-id="6d633-138">To start the streaming endpoint, do the following:</span></span>

1. <span data-ttu-id="6d633-139">Logga in på [Azure-portalen](https://portal.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="6d633-139">Log in at the [Azure portal](https://portal.azure.com/).</span></span>
2. <span data-ttu-id="6d633-140">I fönstret Inställningar klickar du på Slutpunkter för direktuppspelning.</span><span class="sxs-lookup"><span data-stu-id="6d633-140">In the Settings window, click Streaming endpoints.</span></span>
3. <span data-ttu-id="6d633-141">Klicka på den slutpunkt för direktuppspelning som är standard.</span><span class="sxs-lookup"><span data-stu-id="6d633-141">Click the default streaming endpoint.</span></span>

    <span data-ttu-id="6d633-142">Fönstret INFORMATION OM DEN SLUTPUNKT FÖR DIREKTUPPSPELNING SOM ÄR STANDARD visas.</span><span class="sxs-lookup"><span data-stu-id="6d633-142">The DEFAULT STREAMING ENDPOINT DETAILS window appears.</span></span>

4. <span data-ttu-id="6d633-143">Klicka på ikonen Start.</span><span class="sxs-lookup"><span data-stu-id="6d633-143">Click the Start icon.</span></span>
5. <span data-ttu-id="6d633-144">Klicka på knappen Spara för att spara ändringarna.</span><span class="sxs-lookup"><span data-stu-id="6d633-144">Click the Save button to save your changes.</span></span>

## <span data-ttu-id="6d633-145"><a id="connect"></a>Ansluta till Media Services-konto med REST API</span><span class="sxs-lookup"><span data-stu-id="6d633-145"><a id="connect"></a>Connect to the Media Services account with REST API</span></span>

<span data-ttu-id="6d633-146">Information om hur du ansluter till AMS API: et finns [åtkomst till Azure Media Services-API med Azure AD authentication](media-services-use-aad-auth-to-access-ams-api.md).</span><span class="sxs-lookup"><span data-stu-id="6d633-146">For information on how to connect to the AMS API, see [Access the Azure Media Services API with Azure AD authentication](media-services-use-aad-auth-to-access-ams-api.md).</span></span> 

>[!NOTE]
><span data-ttu-id="6d633-147">När du har anslutit till https://media.windows.net, får du en 301 omdirigering att ange en annan Media Services-URI.</span><span class="sxs-lookup"><span data-stu-id="6d633-147">After successfully connecting to https://media.windows.net, you will receive a 301 redirect specifying another Media Services URI.</span></span> <span data-ttu-id="6d633-148">Du måste göra följande anrop till en ny URI.</span><span class="sxs-lookup"><span data-stu-id="6d633-148">You must make subsequent calls to the new URI.</span></span>

<span data-ttu-id="6d633-149">Till exempel om när du försöker ansluta har du följande:</span><span class="sxs-lookup"><span data-stu-id="6d633-149">For example, if after trying to connect, you got the following:</span></span>

    HTTP/1.1 301 Moved Permanently
    Location: https://wamsbayclus001rest-hs.cloudapp.net/api/

<span data-ttu-id="6d633-150">Du bör publicera dina efterföljande API-anrop till https://wamsbayclus001rest-hs.cloudapp.net/api/.</span><span class="sxs-lookup"><span data-stu-id="6d633-150">You should post your subsequent API calls to https://wamsbayclus001rest-hs.cloudapp.net/api/.</span></span>

## <span data-ttu-id="6d633-151"><a id="upload"></a>Skapa en ny tillgång och överföra en videofil med REST API</span><span class="sxs-lookup"><span data-stu-id="6d633-151"><a id="upload"></a>Create a new asset and upload a video file with REST API</span></span>

<span data-ttu-id="6d633-152">I Media Services överför du dina digitala filer till en tillgång.</span><span class="sxs-lookup"><span data-stu-id="6d633-152">In Media Services, you upload your digital files into an asset.</span></span> <span data-ttu-id="6d633-153">Den **tillgången** entitet kan innehålla video, ljud, bilder, miniatyrsamlingar, text spår och textning filer (och metadata om dessa filer.)  När filerna har överförts till tillgången, lagras innehållet på ett säkert sätt i molnet för ytterligare bearbetning och strömning.</span><span class="sxs-lookup"><span data-stu-id="6d633-153">The **Asset** entity can contain video, audio, images, thumbnail collections, text tracks and closed caption files (and the metadata about these files.)  Once the files are uploaded into the asset, your content is stored securely in the cloud for further processing and streaming.</span></span>

<span data-ttu-id="6d633-154">En av de värden som du måste ange när du skapar en tillgång är alternativ för att skapa tillgången.</span><span class="sxs-lookup"><span data-stu-id="6d633-154">One of the values that you have to provide when creating an asset is asset creation options.</span></span> <span data-ttu-id="6d633-155">Den **alternativ** egenskapen är ett uppräkningsvärde som beskriver krypteringsalternativen som du kan skapa en tillgång med.</span><span class="sxs-lookup"><span data-stu-id="6d633-155">The **Options** property is an enumeration value that describes the encryption options that an Asset can be created with.</span></span> <span data-ttu-id="6d633-156">Ett giltigt värde är ett av värdena i listan nedan, inte en kombination av värden från listan:</span><span class="sxs-lookup"><span data-stu-id="6d633-156">A valid value is one of the values from the list below, not a combination of values from this list:</span></span>

* <span data-ttu-id="6d633-157">**Ingen** = **0** – Ingen kryptering används.</span><span class="sxs-lookup"><span data-stu-id="6d633-157">**None** = **0** - No encryption is used.</span></span> <span data-ttu-id="6d633-158">När du använder det här alternativet skyddas inte innehållet under överföring eller i vila i lagringsutrymmet.</span><span class="sxs-lookup"><span data-stu-id="6d633-158">When using this option your content is not protected in transit or at rest in storage.</span></span>
    <span data-ttu-id="6d633-159">Om du planerar att leverera en MP4 med progressivt nedladdning ska du använda det här alternativet.</span><span class="sxs-lookup"><span data-stu-id="6d633-159">If you plan to deliver an MP4 using progressive download, use this option.</span></span>
* <span data-ttu-id="6d633-160">**StorageEncrypted** = **1** – krypterar innehållet lokalt med hjälp av AES 256 bitarskryptering och överför den till Azure Storage där den lagras krypterat i vila.</span><span class="sxs-lookup"><span data-stu-id="6d633-160">**StorageEncrypted** = **1** - Encrypts your clear content locally using AES-256 bit encryption and then uploads it to Azure Storage where it is stored encrypted at rest.</span></span> <span data-ttu-id="6d633-161">Tillgångar som skyddas med Lagringskryptering avkrypteras automatiskt och placeras i ett krypterat filsystem före kodning och kan krypteras igen innan de överförs tillbaka som en ny utdatatillgång.</span><span class="sxs-lookup"><span data-stu-id="6d633-161">Assets protected with Storage Encryption are automatically unencrypted and placed in an encrypted file system prior to encoding, and optionally re-encrypted prior to uploading back as a new output asset.</span></span> <span data-ttu-id="6d633-162">Lagringskryptering används i första hand när du vill skydda indatamediefiler av hög kvalitet med stark kryptering i vila på disk.</span><span class="sxs-lookup"><span data-stu-id="6d633-162">The primary use case for Storage Encryption is when you want to secure your high-quality input media files with strong encryption at rest on disk.</span></span>
* <span data-ttu-id="6d633-163">**CommonEncryptionProtected** = **2** – Använd det här alternativet om du överför innehåll som redan har krypterats och skyddats med vanlig kryptering eller PlayReady DRM (till exempel Smooth Streaming som skyddas med PlayReady DRM).</span><span class="sxs-lookup"><span data-stu-id="6d633-163">**CommonEncryptionProtected** = **2** - Use this option if you are uploading content that has already been encrypted and protected with Common Encryption or PlayReady DRM (for example, Smooth Streaming protected with PlayReady DRM).</span></span>
* <span data-ttu-id="6d633-164">**EnvelopeEncryptionProtected** = **4** – Använd det här alternativet om du överför HLS som krypterats med AES.</span><span class="sxs-lookup"><span data-stu-id="6d633-164">**EnvelopeEncryptionProtected** = **4** – Use this option if you are uploading HLS encrypted with AES.</span></span> <span data-ttu-id="6d633-165">Filerna måste ha kodats och krypterats av Transform Manager.</span><span class="sxs-lookup"><span data-stu-id="6d633-165">The files must have been encoded and encrypted by Transform Manager.</span></span>

### <a name="create-an-asset"></a><span data-ttu-id="6d633-166">Skapa en tillgång</span><span class="sxs-lookup"><span data-stu-id="6d633-166">Create an asset</span></span>
<span data-ttu-id="6d633-167">En tillgång är en behållare för flera typer eller uppsättningar med objekt i Media Services, inklusive video, ljud, bilder, miniatyrsamlingar, textspår och filer med dold textning.</span><span class="sxs-lookup"><span data-stu-id="6d633-167">An asset is a container for multiple types or sets of objects in Media Services, including video, audio, images, thumbnail collections, text tracks, and closed caption files.</span></span> <span data-ttu-id="6d633-168">I REST-API kräver skapa en tillgång POST-begäran skickades till Media Services och placerar egenskapsinformation om din tillgång i begärandetexten.</span><span class="sxs-lookup"><span data-stu-id="6d633-168">In the REST API, creating an Asset requires sending POST request to Media Services and placing any property information about your asset in the request body.</span></span>

<span data-ttu-id="6d633-169">I följande exempel visas hur du skapar en tillgång.</span><span class="sxs-lookup"><span data-stu-id="6d633-169">The following example shows how to create an asset.</span></span>

<span data-ttu-id="6d633-170">**HTTP-begäran**</span><span class="sxs-lookup"><span data-stu-id="6d633-170">**HTTP Request**</span></span>

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


<span data-ttu-id="6d633-171">**HTTP-svar**</span><span class="sxs-lookup"><span data-stu-id="6d633-171">**HTTP Response**</span></span>

<span data-ttu-id="6d633-172">Om det lyckas, returneras följande:</span><span class="sxs-lookup"><span data-stu-id="6d633-172">If successful, the following is returned:</span></span>

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

### <a name="create-an-assetfile"></a><span data-ttu-id="6d633-173">Skapa en AssetFile</span><span class="sxs-lookup"><span data-stu-id="6d633-173">Create an AssetFile</span></span>
<span data-ttu-id="6d633-174">Den [AssetFile](https://docs.microsoft.com/rest/api/media/operations/assetfile) entiteten representerar en video eller ljud-fil som lagras i en blob-behållare.</span><span class="sxs-lookup"><span data-stu-id="6d633-174">The [AssetFile](https://docs.microsoft.com/rest/api/media/operations/assetfile) entity represents a video or audio file that is stored in a blob container.</span></span> <span data-ttu-id="6d633-175">En resursfil är alltid associerad med en tillgång och en tillgång kan innehålla en eller flera AssetFiles.</span><span class="sxs-lookup"><span data-stu-id="6d633-175">An asset file is always associated with an asset, and an asset may contain one or many AssetFiles.</span></span> <span data-ttu-id="6d633-176">Media Services Encoder uppgiften misslyckas om objekttypen tillgången filen inte är associerad med en digital fil i en blob-behållaren.</span><span class="sxs-lookup"><span data-stu-id="6d633-176">The Media Services Encoder task fails if an asset file object is not associated with a digital file in a blob container.</span></span>

<span data-ttu-id="6d633-177">När du har överfört din digitala media-fil till en blobbbehållare som du använder den **sammanfoga** HTTP-begäran om uppdatering av AssetFile med information om media-fil (som visas senare i avsnittet).</span><span class="sxs-lookup"><span data-stu-id="6d633-177">After you upload your digital media file into a blob container, you use the **MERGE** HTTP request to update the AssetFile with information about your media file (as shown later in the topic).</span></span>

<span data-ttu-id="6d633-178">**HTTP-begäran**</span><span class="sxs-lookup"><span data-stu-id="6d633-178">**HTTP Request**</span></span>

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


<span data-ttu-id="6d633-179">**HTTP-svar**</span><span class="sxs-lookup"><span data-stu-id="6d633-179">**HTTP Response**</span></span>

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


### <a name="creating-the-accesspolicy-with-write-permission"></a><span data-ttu-id="6d633-180">Skapa AccessPolicy med behörighet att skriva</span><span class="sxs-lookup"><span data-stu-id="6d633-180">Creating the AccessPolicy with write permission</span></span>
<span data-ttu-id="6d633-181">Innan du laddar upp filer i blob-lagring, ange principen rättigheter för att skriva till en tillgång.</span><span class="sxs-lookup"><span data-stu-id="6d633-181">Before uploading any files into blob storage, set the access policy rights for writing to an asset.</span></span> <span data-ttu-id="6d633-182">För att göra det efter en HTTP-begäran till AccessPolicies entitetsuppsättning.</span><span class="sxs-lookup"><span data-stu-id="6d633-182">To do that, POST an HTTP request to the AccessPolicies entity set.</span></span> <span data-ttu-id="6d633-183">Definiera ett DurationInMinutes värde när de skapas eller felmeddelande en 500 intern Server tillbaka som svar.</span><span class="sxs-lookup"><span data-stu-id="6d633-183">Define a DurationInMinutes value upon creation or you receive a 500 Internal Server error message back in response.</span></span> <span data-ttu-id="6d633-184">Mer information om AccessPolicies finns [AccessPolicy](https://docs.microsoft.com/rest/api/media/operations/accesspolicy).</span><span class="sxs-lookup"><span data-stu-id="6d633-184">For more information on AccessPolicies, see [AccessPolicy](https://docs.microsoft.com/rest/api/media/operations/accesspolicy).</span></span>

<span data-ttu-id="6d633-185">I följande exempel visas hur du skapar en AccessPolicy:</span><span class="sxs-lookup"><span data-stu-id="6d633-185">The following example shows how to create an AccessPolicy:</span></span>

<span data-ttu-id="6d633-186">**HTTP-begäran**</span><span class="sxs-lookup"><span data-stu-id="6d633-186">**HTTP Request**</span></span>

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

<span data-ttu-id="6d633-187">**HTTP-svar**</span><span class="sxs-lookup"><span data-stu-id="6d633-187">**HTTP Response**</span></span>

<span data-ttu-id="6d633-188">Om det lyckas, returneras följande svar:</span><span class="sxs-lookup"><span data-stu-id="6d633-188">If successful, the following response is returned:</span></span>

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

### <a name="get-the-upload-url"></a><span data-ttu-id="6d633-189">Hämta URL</span><span class="sxs-lookup"><span data-stu-id="6d633-189">Get the Upload URL</span></span>

<span data-ttu-id="6d633-190">Skapa en SAS-lokaliserare för att ta emot den faktiska URL.</span><span class="sxs-lookup"><span data-stu-id="6d633-190">To receive the actual upload URL, create a SAS Locator.</span></span> <span data-ttu-id="6d633-191">Lokaliserare definiera start- och typ av anslutningens slutpunkt för klienter som vill komma åt filer i en tillgång.</span><span class="sxs-lookup"><span data-stu-id="6d633-191">Locators define the start time and type of connection endpoint for clients that want to access Files in an Asset.</span></span> <span data-ttu-id="6d633-192">Du kan skapa flera lokaliserare entiteter för ett angivet AccessPolicy och tillgångshantering par att hantera olika klientbegäranden och behov.</span><span class="sxs-lookup"><span data-stu-id="6d633-192">You can create multiple Locator entities for a given AccessPolicy and Asset pair to handle different client requests and needs.</span></span> <span data-ttu-id="6d633-193">Var och en av dessa lokaliserare använder StartTime-värdet plus DurationInMinutes värdet för AccessPolicy för att avgöra hur lång tid som en URL som kan användas.</span><span class="sxs-lookup"><span data-stu-id="6d633-193">Each of these Locators uses the StartTime value plus the DurationInMinutes value of the AccessPolicy to determine the length of time a URL can be used.</span></span> <span data-ttu-id="6d633-194">Mer information finns i [lokaliserare](https://docs.microsoft.com/rest/api/media/operations/locator).</span><span class="sxs-lookup"><span data-stu-id="6d633-194">For more information, see [Locator](https://docs.microsoft.com/rest/api/media/operations/locator).</span></span>

<span data-ttu-id="6d633-195">En SAS-URL har följande format:</span><span class="sxs-lookup"><span data-stu-id="6d633-195">A SAS URL has the following format:</span></span>

    {https://myaccount.blob.core.windows.net}/{asset name}/{video file name}?{SAS signature}

<span data-ttu-id="6d633-196">Vissa förutsättningar gäller:</span><span class="sxs-lookup"><span data-stu-id="6d633-196">Some considerations apply:</span></span>

* <span data-ttu-id="6d633-197">Du kan inte ha fler än fem unika lokaliserare som är associerade med en viss resurs i taget.</span><span class="sxs-lookup"><span data-stu-id="6d633-197">You cannot have more than five unique Locators associated with a given Asset at one time.</span></span> <span data-ttu-id="6d633-198">Mer information finns i lokaliserare.</span><span class="sxs-lookup"><span data-stu-id="6d633-198">For more information, see Locator.</span></span>
* <span data-ttu-id="6d633-199">Om du behöver överföra filer omedelbart ska du ange StartTime-värdet till fem minuter före aktuell tid.</span><span class="sxs-lookup"><span data-stu-id="6d633-199">If you need to upload your files immediately, you should set your StartTime value to five minutes before the current time.</span></span> <span data-ttu-id="6d633-200">Det beror på att det kan finnas klockan skeva mellan klientdatorn och Media Services.</span><span class="sxs-lookup"><span data-stu-id="6d633-200">This is because there may be clock skew between your client machine and Media Services.</span></span> <span data-ttu-id="6d633-201">StartTime-värdet måste också vara i följande DateTime-format: ÅÅÅÅ-MM-ddTHH (till exempel ”2014-05-23T17:53:50Z”).</span><span class="sxs-lookup"><span data-stu-id="6d633-201">Also, your StartTime value must be in the following DateTime format: YYYY-MM-DDTHH:mm:ssZ (for example, "2014-05-23T17:53:50Z").</span></span>    
* <span data-ttu-id="6d633-202">Det kan finnas en 30-40 andra fördröjning efter en positionerare skapas när den är tillgänglig för användning.</span><span class="sxs-lookup"><span data-stu-id="6d633-202">There may be a 30-40 second delay after a Locator is created to when it is available for use.</span></span> <span data-ttu-id="6d633-203">Det här problemet gäller både SAS-URL och ursprung lokaliserare.</span><span class="sxs-lookup"><span data-stu-id="6d633-203">This issue applies to both SAS URL and Origin Locators.</span></span>

<span data-ttu-id="6d633-204">Mer information om SAS finns lokaliserare [detta](http://southworks.com/blog/2015/05/27/reusing-azure-media-services-locators-to-avoid-facing-the-5-shared-access-policy-limitation/) blogg.</span><span class="sxs-lookup"><span data-stu-id="6d633-204">For more information about SAS locators see [this](http://southworks.com/blog/2015/05/27/reusing-azure-media-services-locators-to-avoid-facing-the-5-shared-access-policy-limitation/) blog.</span></span>

<span data-ttu-id="6d633-205">I följande exempel visas hur du skapar en URL SAS-lokaliserare som definieras av egenskapen Type i begärandetexten (”1” för en SAS-lokaliserare) och ”2” för en positionerare för ursprung på begäran.</span><span class="sxs-lookup"><span data-stu-id="6d633-205">The following example shows how to create a SAS URL Locator, as defined by the Type property in the request body ("1" for a SAS locator and "2" for an On-Demand origin locator).</span></span> <span data-ttu-id="6d633-206">Den **sökväg** returnerade-egenskapen innehåller den URL som du måste använda för att överföra din fil.</span><span class="sxs-lookup"><span data-stu-id="6d633-206">The **Path** property returned contains the URL that you must use to upload your file.</span></span>

<span data-ttu-id="6d633-207">**HTTP-begäran**</span><span class="sxs-lookup"><span data-stu-id="6d633-207">**HTTP Request**</span></span>

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


<span data-ttu-id="6d633-208">**HTTP-svar**</span><span class="sxs-lookup"><span data-stu-id="6d633-208">**HTTP Response**</span></span>

<span data-ttu-id="6d633-209">Om det lyckas, returneras följande svar:</span><span class="sxs-lookup"><span data-stu-id="6d633-209">If successful, the following response is returned:</span></span>

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

### <a name="upload-a-file-into-a-blob-storage-container"></a><span data-ttu-id="6d633-210">Ladda upp en fil till en behållare för blob storage</span><span class="sxs-lookup"><span data-stu-id="6d633-210">Upload a file into a blob storage container</span></span>
<span data-ttu-id="6d633-211">När du har AccessPolicy och lokaliserare ange har den faktiska filen överförts till ett Azure blob storage-behållare med hjälp av Azure Storage REST-API: er.</span><span class="sxs-lookup"><span data-stu-id="6d633-211">Once you have the AccessPolicy and Locator set, the actual file is uploaded to an Azure blob storage container using the Azure Storage REST APIs.</span></span> <span data-ttu-id="6d633-212">Du måste överföra filer som blockblobar.</span><span class="sxs-lookup"><span data-stu-id="6d633-212">You must upload the files as block blobs.</span></span> <span data-ttu-id="6d633-213">Azure Media Services stöder inte sidblobar.</span><span class="sxs-lookup"><span data-stu-id="6d633-213">Page blobs are not supported by Azure Media Services.</span></span>  

> [!NOTE]
> <span data-ttu-id="6d633-214">Du måste lägga till filnamnet för den fil som du vill överföra till positioneraren **sökväg** värde som erhölls i föregående avsnitt.</span><span class="sxs-lookup"><span data-stu-id="6d633-214">You must add the file name for the file you want to upload to the Locator **Path** value received in the previous section.</span></span> <span data-ttu-id="6d633-215">Till exempel https://storagetestaccount001.blob.core.windows.net/asset-e7b02da4-5a69-40e7-a8db-e8f4f697aac0/BigBuckBunny.mp4?</span><span class="sxs-lookup"><span data-stu-id="6d633-215">For example, https://storagetestaccount001.blob.core.windows.net/asset-e7b02da4-5a69-40e7-a8db-e8f4f697aac0/BigBuckBunny.mp4?</span></span> <span data-ttu-id="6d633-216">.</span><span class="sxs-lookup"><span data-stu-id="6d633-216">.</span></span> <span data-ttu-id="6d633-217">.</span><span class="sxs-lookup"><span data-stu-id="6d633-217">.</span></span> <span data-ttu-id="6d633-218">.</span><span class="sxs-lookup"><span data-stu-id="6d633-218">.</span></span>
>
>

<span data-ttu-id="6d633-219">Mer information om hur du arbetar med Azure storage blobs finns [REST-API för Blob-tjänsten](https://docs.microsoft.com/rest/api/storageservices/Blob-Service-REST-API).</span><span class="sxs-lookup"><span data-stu-id="6d633-219">For more information on working with Azure storage blobs, see [Blob Service REST API](https://docs.microsoft.com/rest/api/storageservices/Blob-Service-REST-API).</span></span>

### <a name="update-the-assetfile"></a><span data-ttu-id="6d633-220">Uppdatera AssetFile</span><span class="sxs-lookup"><span data-stu-id="6d633-220">Update the AssetFile</span></span>
<span data-ttu-id="6d633-221">Nu när du har överfört din fil, uppdatera informationen om FileAsset storlek (och andra).</span><span class="sxs-lookup"><span data-stu-id="6d633-221">Now that you've uploaded your file, update the FileAsset size (and other) information.</span></span> <span data-ttu-id="6d633-222">Exempel:</span><span class="sxs-lookup"><span data-stu-id="6d633-222">For example:</span></span>

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


<span data-ttu-id="6d633-223">**HTTP-svar**</span><span class="sxs-lookup"><span data-stu-id="6d633-223">**HTTP Response**</span></span>

<span data-ttu-id="6d633-224">Om det lyckas, returneras följande:</span><span class="sxs-lookup"><span data-stu-id="6d633-224">If successful, the following is returned:</span></span>

    HTTP/1.1 204 No Content
    ...

## <a name="delete-the-locator-and-accesspolicy"></a><span data-ttu-id="6d633-225">Ta bort lokaliserare och AccessPolicy</span><span class="sxs-lookup"><span data-stu-id="6d633-225">Delete the Locator and AccessPolicy</span></span>
<span data-ttu-id="6d633-226">**HTTP-begäran**</span><span class="sxs-lookup"><span data-stu-id="6d633-226">**HTTP Request**</span></span>

    DELETE https://wamsbayclus001rest-hs.cloudapp.net/api/Locators('nb%3Alid%3AUUID%3Aaf57bdd8-6751-4e84-b403-f3c140444b54') HTTP/1.1
    DataServiceVersion: 1.0;NetFx
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json
    Accept-Charset: UTF-8
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=amstestaccount001&urn%3aSubscriptionId=z7f09258-2233-4ca2-b1ae-193798e2c9d8&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1421662918&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=utmoXXbm9Q7j4tW1yJuMVA3egRiQy5FPygwadkmPeaY%3d
    x-ms-version: 2.11
    Host: wamsbayclus001rest-hs.cloudapp.net


<span data-ttu-id="6d633-227">**HTTP-svar**</span><span class="sxs-lookup"><span data-stu-id="6d633-227">**HTTP Response**</span></span>

<span data-ttu-id="6d633-228">Om det lyckas, returneras följande:</span><span class="sxs-lookup"><span data-stu-id="6d633-228">If successful, the following is returned:</span></span>

    HTTP/1.1 204 No Content
    ...

<span data-ttu-id="6d633-229">**HTTP-begäran**</span><span class="sxs-lookup"><span data-stu-id="6d633-229">**HTTP Request**</span></span>

    DELETE https://wamsbayclus001rest-hs.cloudapp.net/api/AccessPolicies('nb%3Apid%3AUUID%3Abe0ac48d-af7d-4877-9d60-1805d68bffae') HTTP/1.1
    DataServiceVersion: 1.0;NetFx
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json
    Accept-Charset: UTF-8
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=amstestaccount001&urn%3aSubscriptionId=z7f09258-2233-4ca2-b1ae-193798e2c9d8&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1421662918&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=utmoXXbm9Q7j4tW1yJuMVA3egRiQy5FPygwadkmPeaY%3d
    x-ms-version: 2.11
    Host: wamsbayclus001rest-hs.cloudapp.net

<span data-ttu-id="6d633-230">**HTTP-svar**</span><span class="sxs-lookup"><span data-stu-id="6d633-230">**HTTP Response**</span></span>

<span data-ttu-id="6d633-231">Om det lyckas, returneras följande:</span><span class="sxs-lookup"><span data-stu-id="6d633-231">If successful, the following is returned:</span></span>

    HTTP/1.1 204 No Content
    ...

## <span data-ttu-id="6d633-232"><a id="encode"></a>Koda källfilen till en uppsättning MP4-filer med anpassningsbar bithastighet</span><span class="sxs-lookup"><span data-stu-id="6d633-232"><a id="encode"></a>Encode the source file into a set of adaptive bitrate MP4 files</span></span>

<span data-ttu-id="6d633-233">När du vill föra in tillgångar i Media Services kan media kan vara kodad, transmuxed, gräns och så vidare innan de skickas till klienter.</span><span class="sxs-lookup"><span data-stu-id="6d633-233">After ingesting Assets into Media Services, media can be encoded, transmuxed, watermarked, and so on, before it is delivered to clients.</span></span> <span data-ttu-id="6d633-234">Dessa aktiviteter schemaläggs och körs mot flera bakgrundsrollinstanser för höga prestanda och tillgänglighet.</span><span class="sxs-lookup"><span data-stu-id="6d633-234">These activities are scheduled and run against multiple background role instances to ensure high performance and availability.</span></span> <span data-ttu-id="6d633-235">Aktiviteterna kallas jobb och varje jobb består av atomiska uppgifter som gör det faktiska arbetet i tillgångsfilen (Mer information finns i [jobbet](/rest/api/media/services/job), [aktivitet](/rest/api/media/services/task) beskrivningar).</span><span class="sxs-lookup"><span data-stu-id="6d633-235">These activities are called Jobs and each Job is composed of atomic Tasks that do the actual work on the Asset file (for more information, see [Job](/rest/api/media/services/job), [Task](/rest/api/media/services/task) descriptions).</span></span>

<span data-ttu-id="6d633-236">Som tidigare nämnts, när du arbetar med Azure Media Services som en av de vanligaste scenarierna levererar strömning med anpassningsbar bithastighet till dina klienter.</span><span class="sxs-lookup"><span data-stu-id="6d633-236">As was mentioned earlier, when working with Azure Media Services one of the most common scenarios is delivering adaptive bitrate streaming to your clients.</span></span> <span data-ttu-id="6d633-237">Media Services kan dynamiskt Paketera en uppsättning filer med anpassningsbar bithastighet MP4 till något av följande format: HTTP Live Streaming (HLS), Smooth Streaming, MPEG DASH.</span><span class="sxs-lookup"><span data-stu-id="6d633-237">Media Services can dynamically package a set of adaptive bitrate MP4 files into one of the following formats: HTTP Live Streaming (HLS), Smooth Streaming, MPEG DASH.</span></span>

<span data-ttu-id="6d633-238">I följande avsnitt beskrivs hur du skapar ett jobb som innehåller en kodning uppgift.</span><span class="sxs-lookup"><span data-stu-id="6d633-238">The following section shows how to create a job that contains one encoding task.</span></span> <span data-ttu-id="6d633-239">Uppgiften anger att omkodning av mezzaninfilen till en uppsättning med anpassningsbar bithastighet MP4s **Media Encoder Standard**.</span><span class="sxs-lookup"><span data-stu-id="6d633-239">The task specifies to transcode the mezzanine file into a set of adaptive bitrate MP4s using **Media Encoder Standard**.</span></span> <span data-ttu-id="6d633-240">Avsnittet visar även hur du övervakar jobbet bearbetning pågår.</span><span class="sxs-lookup"><span data-stu-id="6d633-240">The section also shows how to monitor the job processing progress.</span></span> <span data-ttu-id="6d633-241">När jobbet är klart, skulle du kunna skapa positionerare som behövs för att få åtkomst till dina tillgångar.</span><span class="sxs-lookup"><span data-stu-id="6d633-241">When the job is complete, you would be able to create locators that are needed to get access to your assets.</span></span>

### <a name="get-a-media-processor"></a><span data-ttu-id="6d633-242">Hämta en medieprocessor</span><span class="sxs-lookup"><span data-stu-id="6d633-242">Get a media processor</span></span>
<span data-ttu-id="6d633-243">I Media Services är en medieprocessor en komponent som hanterar en specifik bearbetning aktivitet, till exempel kodning, kryptering eller dekryptering medieinnehåll-Formatkonvertering.</span><span class="sxs-lookup"><span data-stu-id="6d633-243">In Media Services, a media processor is a component that handles a specific processing task, such as encoding, format conversion, encrypting, or decrypting media content.</span></span> <span data-ttu-id="6d633-244">Kodning aktiviteten visas i den här självstudiekursen ska vi använda Media Encoder Standard.</span><span class="sxs-lookup"><span data-stu-id="6d633-244">For the encoding task shown in this tutorial, we are going to use the Media Encoder Standard.</span></span>

<span data-ttu-id="6d633-245">Följande kod i kodaren id-begäranden.</span><span class="sxs-lookup"><span data-stu-id="6d633-245">The following code requests the encoder's id.</span></span>

<span data-ttu-id="6d633-246">**HTTP-begäran**</span><span class="sxs-lookup"><span data-stu-id="6d633-246">**HTTP Request**</span></span>

    GET https://wamsbayclus001rest-hs.cloudapp.net/api/MediaProcessors()?$filter=Name%20eq%20'Media%20Encoder%20Standard' HTTP/1.1
    DataServiceVersion: 1.0;NetFx
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json
    Accept-Charset: UTF-8
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=amstestaccount001&urn%3aSubscriptionId=f7f09258-6753-4ca2-b1ae-193798e2c9d8&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1421675491&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=9hUudHYnATpi5hN3cvTfgw%2bL4N3tL0fdsRnQnm6ZYIU%3d
    x-ms-version: 2.11
    Host: wamsbayclus001rest-hs.cloudapp.net


<span data-ttu-id="6d633-247">**HTTP-svar**</span><span class="sxs-lookup"><span data-stu-id="6d633-247">**HTTP Response**</span></span>

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

### <a name="create-a-job"></a><span data-ttu-id="6d633-248">Skapa ett jobb</span><span class="sxs-lookup"><span data-stu-id="6d633-248">Create a job</span></span>
<span data-ttu-id="6d633-249">Varje jobb kan ha en eller flera aktiviteter beroende på vilken typ av bearbetning som du vill utföra.</span><span class="sxs-lookup"><span data-stu-id="6d633-249">Each Job can have one or more Tasks depending on the type of processing that you want to accomplish.</span></span> <span data-ttu-id="6d633-250">Via REST-API kan du skapa jobb och deras relaterade uppgifter i ett av två sätt: aktiviteter kan vara definierade infogad via navigeringsegenskap uppgifter på jobbet entiteter eller OData-batch-bearbetning.</span><span class="sxs-lookup"><span data-stu-id="6d633-250">Through the REST API, you can create Jobs and their related Tasks in one of two ways: Tasks can be defined inline through the Tasks navigation property on Job entities, or through OData batch processing.</span></span> <span data-ttu-id="6d633-251">Media Services SDK använder batch-bearbetning.</span><span class="sxs-lookup"><span data-stu-id="6d633-251">The Media Services SDK uses batch processing.</span></span> <span data-ttu-id="6d633-252">För läsbarhet kodexempel i det här avsnittet finns dock uppgifter definierats internt.</span><span class="sxs-lookup"><span data-stu-id="6d633-252">However, for the readability of the code examples in this topic, tasks are defined inline.</span></span> <span data-ttu-id="6d633-253">Mer information om batchbearbetning finns [Open Data Protocol (OData) gruppbearbetning](http://www.odata.org/documentation/odata-version-3-0/batch-processing/).</span><span class="sxs-lookup"><span data-stu-id="6d633-253">For information on batch processing, see [Open Data Protocol (OData) Batch Processing](http://www.odata.org/documentation/odata-version-3-0/batch-processing/).</span></span>

<span data-ttu-id="6d633-254">I följande exempel visas hur du skapar och efter ett jobb med en aktivitet som anger för att koda ett videoklipp vid en viss upplösning och kvalitet.</span><span class="sxs-lookup"><span data-stu-id="6d633-254">The following example shows you how to create and post a Job with one Task set to encode a video at a specific resolution and quality.</span></span> <span data-ttu-id="6d633-255">Avsnittet följande dokumentation innehåller en lista över de [uppgift förinställningar](http://msdn.microsoft.com/library/mt269960) stöds av Media Encoder Standard processor.</span><span class="sxs-lookup"><span data-stu-id="6d633-255">The following documentation section contains the list of all the [task presets](http://msdn.microsoft.com/library/mt269960) supported by the Media Encoder Standard processor.</span></span>  

<span data-ttu-id="6d633-256">**HTTP-begäran**</span><span class="sxs-lookup"><span data-stu-id="6d633-256">**HTTP Request**</span></span>

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

<span data-ttu-id="6d633-257">**HTTP-svar**</span><span class="sxs-lookup"><span data-stu-id="6d633-257">**HTTP Response**</span></span>

<span data-ttu-id="6d633-258">Om det lyckas, returneras följande svar:</span><span class="sxs-lookup"><span data-stu-id="6d633-258">If successful, the following response is returned:</span></span>

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


<span data-ttu-id="6d633-259">Det finns några viktiga saker att tänka varje jobb begäran:</span><span class="sxs-lookup"><span data-stu-id="6d633-259">There are a few important things to note in any Job request:</span></span>

* <span data-ttu-id="6d633-260">TaskBody egenskaper måste använda literal XML för att definiera antalet indata eller utdata tillgångar som används av aktiviteten.</span><span class="sxs-lookup"><span data-stu-id="6d633-260">TaskBody properties MUST use literal XML to define the number of input, or output assets that are used by the Task.</span></span> <span data-ttu-id="6d633-261">Aktiviteten innehåller XML-schemadefinitionen för XML-filen.</span><span class="sxs-lookup"><span data-stu-id="6d633-261">The Task topic contains the XML Schema Definition for the XML.</span></span>
* <span data-ttu-id="6d633-262">I definitionen TaskBody varje inre värde för <inputAsset> och <outputAsset> måste anges som JobInputAsset(value) eller JobOutputAsset(value).</span><span class="sxs-lookup"><span data-stu-id="6d633-262">In the TaskBody definition, each inner value for <inputAsset> and <outputAsset> must be set as JobInputAsset(value) or JobOutputAsset(value).</span></span>
* <span data-ttu-id="6d633-263">En aktivitet kan innehålla flera utdata tillgångar.</span><span class="sxs-lookup"><span data-stu-id="6d633-263">A task can have multiple output assets.</span></span> <span data-ttu-id="6d633-264">En JobOutputAsset(x) kan bara användas en gång som utdata för en aktivitet i ett jobb.</span><span class="sxs-lookup"><span data-stu-id="6d633-264">One JobOutputAsset(x) can only be used once as an output of a task in a job.</span></span>
* <span data-ttu-id="6d633-265">Du kan ange JobInputAsset eller JobOutputAsset som inkommande tillgång för en aktivitet.</span><span class="sxs-lookup"><span data-stu-id="6d633-265">You can specify JobInputAsset or JobOutputAsset as an input asset of a task.</span></span>
* <span data-ttu-id="6d633-266">Aktiviteter måste inte utgör en cykel.</span><span class="sxs-lookup"><span data-stu-id="6d633-266">Tasks must not form a cycle.</span></span>
* <span data-ttu-id="6d633-267">Värdeparametern som du skickar till JobInputAsset eller JobOutputAsset representerar indexet för en tillgång.</span><span class="sxs-lookup"><span data-stu-id="6d633-267">The value parameter that you pass to JobInputAsset or JobOutputAsset represents the index value for an Asset.</span></span> <span data-ttu-id="6d633-268">De faktiska tillgångarna har definierats i navigeringsegenskaper InputMediaAssets och OutputMediaAssets på entiteten jobbdefinitionen.</span><span class="sxs-lookup"><span data-stu-id="6d633-268">The actual Assets are defined in the InputMediaAssets and OutputMediaAssets navigation properties on the Job entity definition.</span></span>

> [!NOTE]
> <span data-ttu-id="6d633-269">Eftersom Media Services bygger på OData v3 enskilda tillgångar i InputMediaAssets och OutputMediaAssets navigering egenskapssamlingar refereras via en ”__metadata: uri” namn / värde-par.</span><span class="sxs-lookup"><span data-stu-id="6d633-269">Because Media Services is built on OData v3, the individual assets in InputMediaAssets and OutputMediaAssets navigation property collections are referenced through a "__metadata : uri" name-value pair.</span></span>
>
>

* <span data-ttu-id="6d633-270">InputMediaAssets mappas till en eller flera resurser som du har skapat i Media Services.</span><span class="sxs-lookup"><span data-stu-id="6d633-270">InputMediaAssets maps to one or more Assets you have created in Media Services.</span></span> <span data-ttu-id="6d633-271">OutputMediaAssets skapas av systemet.</span><span class="sxs-lookup"><span data-stu-id="6d633-271">OutputMediaAssets are created by the system.</span></span> <span data-ttu-id="6d633-272">De hänvisar inte till en befintlig tillgång.</span><span class="sxs-lookup"><span data-stu-id="6d633-272">They do not reference an existing asset.</span></span>
* <span data-ttu-id="6d633-273">OutputMediaAssets kan namnges med attributet assetName.</span><span class="sxs-lookup"><span data-stu-id="6d633-273">OutputMediaAssets can be named using the assetName attribute.</span></span> <span data-ttu-id="6d633-274">Om det här attributet är inte tillgängligt och sedan namnet på OutputMediaAsset är det inre textvärdet för den <outputAsset> elementet är med suffixet namnvärdet för jobb eller jobb-Id-värde (i de fall där egenskapen namn har inte definierats).</span><span class="sxs-lookup"><span data-stu-id="6d633-274">If this attribute is not present, then the name of the OutputMediaAsset is whatever the inner text value of the <outputAsset> element is with a suffix of either the Job Name value, or the Job Id value (in the case where the Name property isn't defined).</span></span> <span data-ttu-id="6d633-275">Till exempel om du anger ett värde för assetName till ”Sample”, anges sedan OutputMediaAsset Name-egenskapen till ”Sample”.</span><span class="sxs-lookup"><span data-stu-id="6d633-275">For example, if you set a value for assetName to "Sample", then the OutputMediaAsset Name property would be set to "Sample".</span></span> <span data-ttu-id="6d633-276">Men om du inte har angett ett värde för assetName men ställts in på jobbnamnet till ”NewJob”, OutputMediaAsset namnet skulle vara ”_NewJob JobOutputAsset (värde)”.</span><span class="sxs-lookup"><span data-stu-id="6d633-276">However, if you did not set a value for assetName, but did set the job name to "NewJob", then the OutputMediaAsset Name would be "JobOutputAsset(value)_NewJob".</span></span>

    <span data-ttu-id="6d633-277">I följande exempel visas hur du ställer in attributet assetName:</span><span class="sxs-lookup"><span data-stu-id="6d633-277">The following example shows how to set the assetName attribute:</span></span>

        "<?xml version=\"1.0\" encoding=\"utf-8\"?><taskBody><inputAsset>JobInputAsset(0)</inputAsset><outputAsset assetName=\"CustomOutputAssetName\">JobOutputAsset(0)</outputAsset></taskBody>"
* <span data-ttu-id="6d633-278">Så här aktiverar du länkning av aktivitet:</span><span class="sxs-lookup"><span data-stu-id="6d633-278">To enable task chaining:</span></span>

  * <span data-ttu-id="6d633-279">Ett jobb måste ha minst två aktiviteter</span><span class="sxs-lookup"><span data-stu-id="6d633-279">A job must have at least two tasks</span></span>
  * <span data-ttu-id="6d633-280">Det måste finnas minst en aktivitet vars indata är resultatet av en annan aktivitet i jobbet.</span><span class="sxs-lookup"><span data-stu-id="6d633-280">There must be at least one task whose input is output of another task in the job.</span></span>

<span data-ttu-id="6d633-281">Mer information finns i [skapar ett jobb Encoding med Media Services REST API](media-services-rest-encode-asset.md).</span><span class="sxs-lookup"><span data-stu-id="6d633-281">For more information see, [Creating an Encoding Job with the Media Services REST API](media-services-rest-encode-asset.md).</span></span>

### <a name="monitor-processing-progress"></a><span data-ttu-id="6d633-282">Övervakaren bearbetning pågår</span><span class="sxs-lookup"><span data-stu-id="6d633-282">Monitor Processing Progress</span></span>
<span data-ttu-id="6d633-283">Du kan hämta jobbstatus med hjälp av egenskapen tillstånd som visas i följande exempel.</span><span class="sxs-lookup"><span data-stu-id="6d633-283">You can retrieve the Job status by using the State property, as shown in the following example.</span></span>

<span data-ttu-id="6d633-284">**HTTP-begäran**</span><span class="sxs-lookup"><span data-stu-id="6d633-284">**HTTP Request**</span></span>

    GET https://wamsbayclus001rest-hs.net/api/Jobs('nb%3Ajid%3AUUID%3A71d2dd33-efdf-ec43-8ea1-136a110bd42c')/State HTTP/1.1
    Content-Type: application/json;odata=verbose
    Accept: application/json;odata=verbose
    DataServiceVersion: 3.0
    MaxDataServiceVersion: 3.0
    x-ms-version: 2.11
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=youraccountname&urn%3aSubscriptionId=zf84471d-2233-4e75-aa09-010f0fc0cf5b&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1336908022&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=RYXOraO6Z%2f7l9whWZQN%2bypeijgHwIk8XyikA01Kx1%2bk%3d
    Host: wamsbayclus001rest-hs.net
    Content-Length: 0


<span data-ttu-id="6d633-285">**HTTP-svar**</span><span class="sxs-lookup"><span data-stu-id="6d633-285">**HTTP Response**</span></span>

<span data-ttu-id="6d633-286">Om det lyckas, returneras följande svar:</span><span class="sxs-lookup"><span data-stu-id="6d633-286">If successful, the following response is returned:</span></span>

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


### <a name="cancel-a-job"></a><span data-ttu-id="6d633-287">Avbryta ett jobb</span><span class="sxs-lookup"><span data-stu-id="6d633-287">Cancel a job</span></span>
<span data-ttu-id="6d633-288">Media Services kan du avbryta jobb som körs med funktionen CancelJob.</span><span class="sxs-lookup"><span data-stu-id="6d633-288">Media Services allows you to cancel running jobs through the CancelJob function.</span></span> <span data-ttu-id="6d633-289">Det här anropet returnerar en 400 felkoden om du försöker avbryta ett jobb när dess tillstånd har avbrutits, avbryta, fel eller slutförts.</span><span class="sxs-lookup"><span data-stu-id="6d633-289">This call returns a 400 error code if you try to cancel a Job when its state is canceled, canceling, error, or finished.</span></span>

<span data-ttu-id="6d633-290">I följande exempel visas hur du anropar CancelJob.</span><span class="sxs-lookup"><span data-stu-id="6d633-290">The following example shows how to call CancelJob.</span></span>

<span data-ttu-id="6d633-291">**HTTP-begäran**</span><span class="sxs-lookup"><span data-stu-id="6d633-291">**HTTP Request**</span></span>

    GET https://wamsbayclus001rest-hs.net/API/CancelJob?jobid='nb%3ajid%3aUUID%3a71d2dd33-efdf-ec43-8ea1-136a110bd42c' HTTP/1.1
    Content-Type: application/json;odata=verbose
    Accept: application/json;odata=verbose
    DataServiceVersion: 3.0
    MaxDataServiceVersion: 3.0
    x-ms-version: 2.2
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=youraccountname&urn%3aSubscriptionId=2f84471d-b1ae-4e75-aa09-010f0fc0cf5b&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1336908753&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=kolAgnFfbQIeRv4lWxKSFa4USyjWXRm15Kd%2bNd5g8eA%3d
    Host: wamsbayclus001rest-hs.net


<span data-ttu-id="6d633-292">Om det lyckas, returneras en 204-svarskod med ingen brödtext.</span><span class="sxs-lookup"><span data-stu-id="6d633-292">If successful, a 204 response code is returned with no message body.</span></span>

> [!NOTE]
> <span data-ttu-id="6d633-293">Du måste URL-koda jobb-id (normalt nb:jid:UUID: somevalue) när skickas den i som en parameter till CancelJob.</span><span class="sxs-lookup"><span data-stu-id="6d633-293">You must URL-encode the job id (normally nb:jid:UUID: somevalue) when passing it in as a parameter to CancelJob.</span></span>
>
>

### <a name="get-the-output-asset"></a><span data-ttu-id="6d633-294">Hämta utdatatillgången</span><span class="sxs-lookup"><span data-stu-id="6d633-294">Get the output asset</span></span>
<span data-ttu-id="6d633-295">Följande kod visar hur du begär utdatatillgången Id.</span><span class="sxs-lookup"><span data-stu-id="6d633-295">The following code shows how to request the output asset Id.</span></span>

<span data-ttu-id="6d633-296">**HTTP-begäran**</span><span class="sxs-lookup"><span data-stu-id="6d633-296">**HTTP Request**</span></span>

    GET https://wamsbayclus001rest-hs.cloudapp.net/api/Jobs('nb%3Ajid%3AUUID%3A71d2dd33-efdf-ec43-8ea1-136a110bd42c')/OutputMediaAssets() HTTP/1.1
    DataServiceVersion: 1.0;NetFx
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json
    Accept-Charset: UTF-8
    User-Agent: Microsoft ADO.NET Data Services
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=amstestaccount001&urn%3aSubscriptionId=z7f09258-2233-4ca2-b1ae-193798e2c9d8&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1421675491&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=9hUudHYnATpi5hN3cvTfgw%2bL4N3tL0fdsRnQnm6ZYIU%3d
    x-ms-version: 2.11
    Host: wamsbayclus001rest-hs.cloudapp.net


<span data-ttu-id="6d633-297">**HTTP-svar**</span><span class="sxs-lookup"><span data-stu-id="6d633-297">**HTTP Response**</span></span>

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



## <span data-ttu-id="6d633-298"><a id="publish_get_urls"></a>Publicera tillgången och hämta strömning och progressiv överföring URL: er med REST API</span><span class="sxs-lookup"><span data-stu-id="6d633-298"><a id="publish_get_urls"></a>Publish the asset and get streaming and progressive download URLs with REST API</span></span>

<span data-ttu-id="6d633-299">Om du vill strömma eller hämta en tillgång behöver du först ”publicera” den genom att skapa en positionerare.</span><span class="sxs-lookup"><span data-stu-id="6d633-299">To stream or download an asset, you first need to "publish" it by creating a locator.</span></span> <span data-ttu-id="6d633-300">Positionerare ger åtkomst till filer som finns i tillgången.</span><span class="sxs-lookup"><span data-stu-id="6d633-300">Locators provide access to files contained in the asset.</span></span> <span data-ttu-id="6d633-301">Media Services stöder två typer av positionerare: OnDemandOrigin-positionerare som används för att strömma media (till exempel MPEG DASH, HLS eller Smooth Streaming) och Access Signature (SAS)-positionerare som används för att hämta mediefiler.</span><span class="sxs-lookup"><span data-stu-id="6d633-301">Media Services supports two types of locators: OnDemandOrigin locators, used to stream media (for example, MPEG DASH, HLS, or Smooth Streaming) and Access Signature (SAS) locators, used to download media files.</span></span> <span data-ttu-id="6d633-302">Mer information om SAS finns lokaliserare [detta](http://southworks.com/blog/2015/05/27/reusing-azure-media-services-locators-to-avoid-facing-the-5-shared-access-policy-limitation/) blogg.</span><span class="sxs-lookup"><span data-stu-id="6d633-302">For more information about SAS locators see [this](http://southworks.com/blog/2015/05/27/reusing-azure-media-services-locators-to-avoid-facing-the-5-shared-access-policy-limitation/) blog.</span></span>

<span data-ttu-id="6d633-303">När du har skapat positionerarna kan du skapa URL: er som används för att strömma eller hämta dina filer.</span><span class="sxs-lookup"><span data-stu-id="6d633-303">Once you create the locators, you can build the URLs that are used to stream or download your files.</span></span>

>[!NOTE]
><span data-ttu-id="6d633-304">När ditt AMS-konto skapas läggs en **standard**-slutpunkt för direktuppspelning till på ditt konto med tillståndet **Stoppad**.</span><span class="sxs-lookup"><span data-stu-id="6d633-304">When your AMS account is created a **default** streaming endpoint is added to your account in the **Stopped** state.</span></span> <span data-ttu-id="6d633-305">Om du vill starta direktuppspelning av innehåll och dra nytta av dynamisk paketering och dynamisk kryptering måste slutpunkten för direktuppspelning som du vill spela upp innehåll från ha tillståndet **Körs**.</span><span class="sxs-lookup"><span data-stu-id="6d633-305">To start streaming your content and take advantage of dynamic packaging and dynamic encryption, the streaming endpoint from which you want to stream content has to be in the **Running** state.</span></span>

<span data-ttu-id="6d633-306">En strömmande URL för Smooth Streaming har följande format:</span><span class="sxs-lookup"><span data-stu-id="6d633-306">A streaming URL for Smooth Streaming has the following format:</span></span>

    {streaming endpoint name-media services account name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest

<span data-ttu-id="6d633-307">En strömmande URL för HLS har följande format:</span><span class="sxs-lookup"><span data-stu-id="6d633-307">A streaming URL for HLS has the following format:</span></span>

    {streaming endpoint name-media services account name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest(format=m3u8-aapl)

<span data-ttu-id="6d633-308">En strömmande URL för MPEG DASH har följande format:</span><span class="sxs-lookup"><span data-stu-id="6d633-308">A streaming URL for MPEG DASH has the following format:</span></span>

    {streaming endpoint name-media services account name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest(format=mpd-time-csf)


<span data-ttu-id="6d633-309">En SAS-URL som används för att hämta filer har följande format:</span><span class="sxs-lookup"><span data-stu-id="6d633-309">A SAS URL used to download files has the following format:</span></span>

    {blob container name}/{asset name}/{file name}/{SAS signature}

<span data-ttu-id="6d633-310">Det här avsnittet visar hur du utföra följande uppgifter krävs för att ”publicera” dina tillgångar.</span><span class="sxs-lookup"><span data-stu-id="6d633-310">This section shows how to perform the following tasks necessary to "publish" your assets.</span></span>  

* <span data-ttu-id="6d633-311">Skapa AccessPolicy med läsbehörighet</span><span class="sxs-lookup"><span data-stu-id="6d633-311">Creating the AccessPolicy with read permission</span></span>
* <span data-ttu-id="6d633-312">Skapa en SAS-URL för hämtning av innehåll</span><span class="sxs-lookup"><span data-stu-id="6d633-312">Creating a SAS URL for downloading content</span></span>
* <span data-ttu-id="6d633-313">Skapa ett ursprung URL för direktuppspelning av innehåll</span><span class="sxs-lookup"><span data-stu-id="6d633-313">Creating an origin URL for streaming content</span></span>

### <a name="creating-the-accesspolicy-with-read-permission"></a><span data-ttu-id="6d633-314">Skapa AccessPolicy med läsbehörighet</span><span class="sxs-lookup"><span data-stu-id="6d633-314">Creating the AccessPolicy with read permission</span></span>
<span data-ttu-id="6d633-315">Innan strömmande mediainnehåll, definiera en AccessPolicy med läsbehörighet och skapa lämpliga lokaliserare enheten som anger vilken typ av leveransmekanismen som du vill aktivera för dina klienter.</span><span class="sxs-lookup"><span data-stu-id="6d633-315">Before downloading or streaming any media content, first define an AccessPolicy with read permissions and create the appropriate Locator entity that specifies the type of delivery mechanism you want to enable for your clients.</span></span> <span data-ttu-id="6d633-316">Mer information om egenskaper som är tillgängliga finns [AccessPolicy Entitetsegenskaper](https://docs.microsoft.com/rest/api/media/operations/accesspolicy#accesspolicy_properties).</span><span class="sxs-lookup"><span data-stu-id="6d633-316">For more information on the properties available, see [AccessPolicy Entity Properties](https://docs.microsoft.com/rest/api/media/operations/accesspolicy#accesspolicy_properties).</span></span>

<span data-ttu-id="6d633-317">I följande exempel visas hur du anger AccessPolicy för läsbehörighet för den angivna resursen.</span><span class="sxs-lookup"><span data-stu-id="6d633-317">The following example shows how to specify the AccessPolicy for read permissions for a given Asset.</span></span>

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

<span data-ttu-id="6d633-318">Om det lyckas, returneras 201 koden som beskriver AccessPolicy för entiteten som du skapade.</span><span class="sxs-lookup"><span data-stu-id="6d633-318">If successful, a 201 success code is returned describing the AccessPolicy entity that you created.</span></span> <span data-ttu-id="6d633-319">Du kan sedan använda AccessPolicy Id tillsammans med tillgångsinformation-Id för den tillgång som innehåller den fil som du vill leverera (till exempel en utdatatillgången) för att skapa lokaliserare entiteten.</span><span class="sxs-lookup"><span data-stu-id="6d633-319">You then use the AccessPolicy Id along with the Asset Id of the asset that contains the file you want to deliver(such as an output asset) to create the Locator entity.</span></span>

> [!NOTE]
> <span data-ttu-id="6d633-320">Grundläggande arbetsflödet är samma som överför en fil när du vill föra in en tillgång (som beskrevs tidigare i det här avsnittet).</span><span class="sxs-lookup"><span data-stu-id="6d633-320">This basic workflow is the same as uploading a file when ingesting an Asset (as was discussed earlier in this topic).</span></span> <span data-ttu-id="6d633-321">Som överför filer, om du (eller klienterna) behöver komma åt dina filer direkt, ange dessutom StartTime-värdet till fem minuter före aktuell tid.</span><span class="sxs-lookup"><span data-stu-id="6d633-321">Also, like uploading files, if you (or your clients) need to access your files immediately, set your StartTime value to five minutes before the current time.</span></span> <span data-ttu-id="6d633-322">Den här åtgärden är nödvändigt eftersom det kan finnas klockan skeva mellan klienten och Media Services.</span><span class="sxs-lookup"><span data-stu-id="6d633-322">This action is necessary because there may be clock skew between the client and Media Services.</span></span> <span data-ttu-id="6d633-323">StartTime-värdet måste vara i följande DateTime-format: ÅÅÅÅ-MM-ddTHH (till exempel ”2014-05-23T17:53:50Z”).</span><span class="sxs-lookup"><span data-stu-id="6d633-323">The StartTime value must be in the following DateTime format: YYYY-MM-DDTHH:mm:ssZ (for example, "2014-05-23T17:53:50Z").</span></span>
>
>

### <a name="creating-a-sas-url-for-downloading-content"></a><span data-ttu-id="6d633-324">Skapa en SAS-URL för hämtning av innehåll</span><span class="sxs-lookup"><span data-stu-id="6d633-324">Creating a SAS URL for downloading content</span></span>
<span data-ttu-id="6d633-325">Följande kod visar hur du kan få en URL som kan användas för att hämta en mediefil skapas och överföra tidigare.</span><span class="sxs-lookup"><span data-stu-id="6d633-325">The following code shows how to get a URL that can be used to download a media file created and uploaded previously.</span></span> <span data-ttu-id="6d633-326">AccessPolicy har skrivskyddad behörighet som anges och lokaliserare sökvägen refererar till en SAS-URL för hämtning.</span><span class="sxs-lookup"><span data-stu-id="6d633-326">The AccessPolicy has read permissions set and the Locator path refers to a SAS download URL.</span></span>

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

<span data-ttu-id="6d633-327">Om det lyckas, returneras följande svar:</span><span class="sxs-lookup"><span data-stu-id="6d633-327">If successful, the following response is returned:</span></span>

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


<span data-ttu-id="6d633-328">Den returnerade **sökväg** egenskap innehåller SAS-URL.</span><span class="sxs-lookup"><span data-stu-id="6d633-328">The returned **Path** property contains the SAS URL.</span></span>

> [!NOTE]
> <span data-ttu-id="6d633-329">Om du hämtar lagringskrypterad innehåll måste du manuellt dekryptera den innan du gör den eller använda lagring dekryptering MediaProcessor i en Bearbetningsuppgift bearbetade utdatafiler i klartext till en OutputAsset och sedan hämta från tillgången.</span><span class="sxs-lookup"><span data-stu-id="6d633-329">If you download storage encrypted content, you must manually decrypt it before rendering it, or use the Storage Decryption MediaProcessor in a processing task to output processed files in the clear to an OutputAsset and then download from that Asset.</span></span> <span data-ttu-id="6d633-330">Mer information om bearbetningen finns i skapar ett jobb Encoding med Media Services REST API.</span><span class="sxs-lookup"><span data-stu-id="6d633-330">For more information on processing, see Creating an Encoding Job with the Media Services REST API.</span></span> <span data-ttu-id="6d633-331">Lokaliserare för SAS-URL: en kan inte uppdateras när de har skapats.</span><span class="sxs-lookup"><span data-stu-id="6d633-331">Also, SAS URL Locators cannot be updated after they have been created.</span></span> <span data-ttu-id="6d633-332">Du kan exempelvis återanvända samma lokaliserare med ett uppdaterat StartTime-värde.</span><span class="sxs-lookup"><span data-stu-id="6d633-332">For example, you cannot reuse the same Locator with an updated StartTime value.</span></span> <span data-ttu-id="6d633-333">Detta beror på hur SAS-URL: er skapas.</span><span class="sxs-lookup"><span data-stu-id="6d633-333">This is because of the way SAS URLs are created.</span></span> <span data-ttu-id="6d633-334">Om du vill komma åt en tillgång för att ladda ned efter en positionerare har upphört att gälla måste du skapa en ny med en ny StartTime.</span><span class="sxs-lookup"><span data-stu-id="6d633-334">If you want to access an asset for download after a Locator has expired, then you must create a new one with a new StartTime.</span></span>
>
>

### <a name="download-files"></a><span data-ttu-id="6d633-335">Hämta filer</span><span class="sxs-lookup"><span data-stu-id="6d633-335">Download files</span></span>
<span data-ttu-id="6d633-336">När du har AccessPolicy och lokaliserare ange kan hämta du filer med hjälp av Azure Storage REST-API: er.</span><span class="sxs-lookup"><span data-stu-id="6d633-336">Once you have the AccessPolicy and Locator set, you can download files using the Azure Storage REST APIs.</span></span>  

> [!NOTE]
> <span data-ttu-id="6d633-337">Du måste lägga till filnamnet för den fil som du vill ladda ned till positioneraren **sökväg** värde som erhölls i föregående avsnitt.</span><span class="sxs-lookup"><span data-stu-id="6d633-337">You must add the file name for the file you want to download to the Locator **Path** value received in the previous section.</span></span> <span data-ttu-id="6d633-338">Till exempel https://storagetestaccount001.blob.core.windows.net/asset-e7b02da4-5a69-40e7-a8db-e8f4f697aac0/BigBuckBunny.mp4?</span><span class="sxs-lookup"><span data-stu-id="6d633-338">For example, https://storagetestaccount001.blob.core.windows.net/asset-e7b02da4-5a69-40e7-a8db-e8f4f697aac0/BigBuckBunny.mp4?</span></span> <span data-ttu-id="6d633-339">.</span><span class="sxs-lookup"><span data-stu-id="6d633-339">.</span></span> <span data-ttu-id="6d633-340">.</span><span class="sxs-lookup"><span data-stu-id="6d633-340">.</span></span> <span data-ttu-id="6d633-341">.</span><span class="sxs-lookup"><span data-stu-id="6d633-341">.</span></span>
>
>

<span data-ttu-id="6d633-342">Mer information om hur du arbetar med Azure storage blobs finns [REST-API för Blob-tjänsten](https://docs.microsoft.com/rest/api/storageservices/Blob-Service-REST-API).</span><span class="sxs-lookup"><span data-stu-id="6d633-342">For more information on working with Azure storage blobs, see [Blob Service REST API](https://docs.microsoft.com/rest/api/storageservices/Blob-Service-REST-API).</span></span>

<span data-ttu-id="6d633-343">På grund av kodningsjobbet som du utförde tidigare (kodning till anpassningsbar MP4-uppsättningen) har du flera MP4-filer som du kan hämta progressivt.</span><span class="sxs-lookup"><span data-stu-id="6d633-343">As a result of the encoding job that you performed earlier (encoding into Adaptive MP4 set), you have multiple MP4 files that you can progressively download.</span></span> <span data-ttu-id="6d633-344">Exempel:</span><span class="sxs-lookup"><span data-stu-id="6d633-344">For example:</span></span>    

    https://storagetestaccount001.blob.core.windows.net/asset-38058602-a4b8-4b33-b9f0-6880dc1490ea/BigBuckBunny_H264_650kbps_AAC_und_ch2_96kbps.mp4?sv=2012-02-12&sr=c&si=166d5154-b801-410b-a226-ee2f8eac1929&sig=P2iNZJAvAWpp%2Bj9yV6TQjoz5DIIaj7ve8ARynmEM6Xk%3D&se=2015-02-14T01:13:05Z

    https://storagetestaccount001.blob.core.windows.net/asset-38058602-a4b8-4b33-b9f0-6880dc1490ea/BigBuckBunny_H264_400kbps_AAC_und_ch2_96kbps.mp4?sv=2012-02-12&sr=c&si=166d5154-b801-410b-a226-ee2f8eac1929&sig=P2iNZJAvAWpp%2Bj9yV6TQjoz5DIIaj7ve8ARynmEM6Xk%3D&se=2015-02-14T01:13:05Z

    https://storagetestaccount001.blob.core.windows.net/asset-38058602-a4b8-4b33-b9f0-6880dc1490ea/BigBuckBunny_H264_3400kbps_AAC_und_ch2_96kbps.mp4?sv=2012-02-12&sr=c&si=166d5154-b801-410b-a226-ee2f8eac1929&sig=P2iNZJAvAWpp%2Bj9yV6TQjoz5DIIaj7ve8ARynmEM6Xk%3D&se=2015-02-14T01:13:05Z

    https://storagetestaccount001.blob.core.windows.net/asset-38058602-a4b8-4b33-b9f0-6880dc1490ea/BigBuckBunny_H264_2250kbps_AAC_und_ch2_96kbps.mp4?sv=2012-02-12&sr=c&si=166d5154-b801-410b-a226-ee2f8eac1929&sig=P2iNZJAvAWpp%2Bj9yV6TQjoz5DIIaj7ve8ARynmEM6Xk%3D&se=2015-02-14T01:13:05Z

    https://storagetestaccount001.blob.core.windows.net/asset-38058602-a4b8-4b33-b9f0-6880dc1490ea/BigBuckBunny_H264_1500kbps_AAC_und_ch2_96kbps.mp4?sv=2012-02-12&sr=c&si=166d5154-b801-410b-a226-ee2f8eac1929&sig=P2iNZJAvAWpp%2Bj9yV6TQjoz5DIIaj7ve8ARynmEM6Xk%3D&se=2015-02-14T01:13:05Z

    https://storagetestaccount001.blob.core.windows.net/asset-38058602-a4b8-4b33-b9f0-6880dc1490ea/BigBuckBunny_H264_1000kbps_AAC_und_ch2_96kbps.mp4?sv=2012-02-12&sr=c&si=166d5154-b801-410b-a226-ee2f8eac1929&sig=P2iNZJAvAWpp%2Bj9yV6TQjoz5DIIaj7ve8ARynmEM6Xk%3D&se=2015-02-14T01:13:05Z

    https://storagetestaccount001.blob.core.windows.net/asset-38058602-a4b8-4b33-b9f0-6880dc1490ea/BigBuckBunny_AAC_und_ch2_96kbps.mp4?sv=2012-02-12&sr=c&si=166d5154-b801-410b-a226-ee2f8eac1929&sig=P2iNZJAvAWpp%2Bj9yV6TQjoz5DIIaj7ve8ARynmEM6Xk%3D&se=2015-02-14T01:13:05Z

    https://storagetestaccount001.blob.core.windows.net/asset-38058602-a4b8-4b33-b9f0-6880dc1490ea/BigBuckBunny_AAC_und_ch2_56kbps.mp4?sv=2012-02-12&sr=c&si=166d5154-b801-410b-a226-ee2f8eac1929&sig=P2iNZJAvAWpp%2Bj9yV6TQjoz5DIIaj7ve8ARynmEM6Xk%3D&se=2015-02-14T01:13:05Z


### <a name="creating-a-streaming-url-for-streaming-content"></a><span data-ttu-id="6d633-345">Skapa en strömnings-URL för direktuppspelning av innehåll</span><span class="sxs-lookup"><span data-stu-id="6d633-345">Creating a streaming URL for streaming content</span></span>
<span data-ttu-id="6d633-346">Följande kod visar hur du skapar en Strömningslokaliserare URL:</span><span class="sxs-lookup"><span data-stu-id="6d633-346">The following code shows how to create a streaming URL Locator:</span></span>

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

<span data-ttu-id="6d633-347">Om det lyckas, returneras följande svar:</span><span class="sxs-lookup"><span data-stu-id="6d633-347">If successful, the following response is returned:</span></span>

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

<span data-ttu-id="6d633-348">Om du vill strömma en Smooth Streaming ursprung URL i en strömmande media player måste du lägga till sökvägen egenskap med namnet på Smooth Streaming manifestfilen, följt av ”/ manifest”.</span><span class="sxs-lookup"><span data-stu-id="6d633-348">To stream a Smooth Streaming origin URL in a streaming media player, you must append the Path property with the name of the Smooth Streaming manifest file, followed by "/manifest".</span></span>

    http://amstestaccount001.streaming.mediaservices.windows.net/ebf733c4-3e2e-4a68-b67b-cc5159d1d7f2/BigBuckBunny.ism/manifest

<span data-ttu-id="6d633-349">Om du vill strömma HLS, Lägg till (format = m3u8 aapl) när den ”/ manifest”.</span><span class="sxs-lookup"><span data-stu-id="6d633-349">To stream HLS, append (format=m3u8-aapl) after the "/manifest".</span></span>

    http://amstestaccount001.streaming.mediaservices.windows.net/ebf733c4-3e2e-4a68-b67b-cc5159d1d7f2/BigBuckBunny.ism/manifest(format=m3u8-aapl)

<span data-ttu-id="6d633-350">Om du vill strömma MPEG DASH, Lägg till (format = mpd-tid-csf) när den ”/ manifest”.</span><span class="sxs-lookup"><span data-stu-id="6d633-350">To stream MPEG DASH, append (format=mpd-time-csf) after the "/manifest".</span></span>

    http://amstestaccount001.streaming.mediaservices.windows.net/ebf733c4-3e2e-4a68-b67b-cc5159d1d7f2/BigBuckBunny.ism/manifest(format=mpd-time-csf)


## <span data-ttu-id="6d633-351"><a id="play"></a>Spela upp ditt innehåll</span><span class="sxs-lookup"><span data-stu-id="6d633-351"><a id="play"></a>Play your content</span></span>
<span data-ttu-id="6d633-352">Strömma videon med hjälp av [Azure Media Services Player](http://amsplayer.azurewebsites.net/azuremediaplayer.html).</span><span class="sxs-lookup"><span data-stu-id="6d633-352">To stream you video, use [Azure Media Services Player](http://amsplayer.azurewebsites.net/azuremediaplayer.html).</span></span>

<span data-ttu-id="6d633-353">Om du vill testa den progressiva nedladdningen, att klistra in en URL i en webbläsare (till exempel Internet Explorer, Chrome, Safari).</span><span class="sxs-lookup"><span data-stu-id="6d633-353">To test progressive download, paste a URL into a browser (for example, IE, Chrome, Safari).</span></span>

## <a name="next-steps-media-services-learning-paths"></a><span data-ttu-id="6d633-354">Nästa steg: sökvägar för Media Services-utbildning</span><span class="sxs-lookup"><span data-stu-id="6d633-354">Next Steps: Media Services learning paths</span></span>
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a><span data-ttu-id="6d633-355">Ge feedback</span><span class="sxs-lookup"><span data-stu-id="6d633-355">Provide feedback</span></span>
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]
