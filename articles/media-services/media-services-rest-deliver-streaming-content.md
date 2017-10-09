---
title: "aaaPublish Azure Media Services innehåll med hjälp av REST"
description: "Lär dig hur toocreate en positionerare som används toobuild en strömnings-URL. hello koden använder REST API."
author: Juliako
manager: cfowler
editor: 
services: media-services
documentationcenter: 
ms.assetid: ff332c30-30c6-4ed1-99d0-5fffd25d4f23
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/09/2017
ms.author: juliako
ms.openlocfilehash: f849e21b3103b9b33bc652e886b2016ea495b19a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="publish-azure-media-services-content-using-rest"></a><span data-ttu-id="10d80-104">Publicera Azure Media Services-innehåll med hjälp av REST</span><span class="sxs-lookup"><span data-stu-id="10d80-104">Publish Azure Media Services content using REST</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="10d80-105">.NET</span><span class="sxs-lookup"><span data-stu-id="10d80-105">.NET</span></span>](media-services-deliver-streaming-content.md)
> * [<span data-ttu-id="10d80-106">REST</span><span class="sxs-lookup"><span data-stu-id="10d80-106">REST</span></span>](media-services-rest-deliver-streaming-content.md)
> * [<span data-ttu-id="10d80-107">Portal</span><span class="sxs-lookup"><span data-stu-id="10d80-107">Portal</span></span>](media-services-portal-publish.md)
> 
> 

## <a name="overview"></a><span data-ttu-id="10d80-108">Översikt</span><span class="sxs-lookup"><span data-stu-id="10d80-108">Overview</span></span>
<span data-ttu-id="10d80-109">Du kan strömma en anpassningsbar bithastighet MP4-uppsättningen genom att skapa en positionerare för strömning och skapa en strömnings-URL.</span><span class="sxs-lookup"><span data-stu-id="10d80-109">You can stream an adaptive bitrate MP4 set by creating an OnDemand streaming locator and building a streaming URL.</span></span> <span data-ttu-id="10d80-110">Hej [koda en tillgång](media-services-rest-encode-asset.md) avsnittet visas hur tooencode i en MP4 med anpassningsbar bithastighet.</span><span class="sxs-lookup"><span data-stu-id="10d80-110">hello [encoding an asset](media-services-rest-encode-asset.md) topic shows how tooencode into an adaptive bitrate MP4 set.</span></span> <span data-ttu-id="10d80-111">Om ditt innehåll är krypterad, konfigurera principen för tillgångsleverans (enligt beskrivningen i [detta](media-services-rest-configure-asset-delivery-policy.md) avsnittet) innan du skapar en positionerare.</span><span class="sxs-lookup"><span data-stu-id="10d80-111">If your content is encrypted, configure asset delivery policy (as described in [this](media-services-rest-configure-asset-delivery-policy.md) topic) before creating a locator.</span></span> 

<span data-ttu-id="10d80-112">Du kan också använda en OnDemand-strömning lokaliserare toobuild-URL: er som punkt tooMP4-filer som kan hämtas progressivt.</span><span class="sxs-lookup"><span data-stu-id="10d80-112">You can also use an OnDemand streaming locator toobuild URLs that point tooMP4 files that can be progressively downloaded.</span></span>  

<span data-ttu-id="10d80-113">Det här avsnittet visar hur toocreate en strömmande positionerare i ordning toopublish din tillgång och skapa en Smooth, MPEG DASH och HLS-strömnings-URL: er.</span><span class="sxs-lookup"><span data-stu-id="10d80-113">This topic shows how toocreate an OnDemand streaming locator in order toopublish your asset and build a Smooth, MPEG DASH, and HLS streaming URLs.</span></span> <span data-ttu-id="10d80-114">Den visar även varm toobuild URL: er för progressiv nedladdning.</span><span class="sxs-lookup"><span data-stu-id="10d80-114">It also shows hot toobuild progressive download URLs.</span></span>

<span data-ttu-id="10d80-115">Hej [följande](#types) avsnittet visar hello enum-typer vars värden används i hello REST-anrop.</span><span class="sxs-lookup"><span data-stu-id="10d80-115">hello [following](#types) section shows hello enum types whose values are used in hello REST calls.</span></span>   

> [!NOTE]
> <span data-ttu-id="10d80-116">Vid åtkomst till entiteter i Media Services måste du ange specifika namn på huvudfält och värden i HTTP-begäranden.</span><span class="sxs-lookup"><span data-stu-id="10d80-116">When accessing entities in Media Services, you must set specific header fields and values in your HTTP requests.</span></span> <span data-ttu-id="10d80-117">Mer information finns i [installationsprogrammet för Media Services REST API-utveckling](media-services-rest-how-to-use.md).</span><span class="sxs-lookup"><span data-stu-id="10d80-117">For more information, see [Setup for Media Services REST API Development](media-services-rest-how-to-use.md).</span></span>
> 

## <a name="connect-toomedia-services"></a><span data-ttu-id="10d80-118">Ansluta tooMedia tjänster</span><span class="sxs-lookup"><span data-stu-id="10d80-118">Connect tooMedia Services</span></span>

<span data-ttu-id="10d80-119">Mer information om hur tooconnect toohello AMS API, se [åtkomst hello Azure Media Services API med Azure AD authentication](media-services-use-aad-auth-to-access-ams-api.md).</span><span class="sxs-lookup"><span data-stu-id="10d80-119">For information on how tooconnect toohello AMS API, see [Access hello Azure Media Services API with Azure AD authentication](media-services-use-aad-auth-to-access-ams-api.md).</span></span> 

>[!NOTE]
><span data-ttu-id="10d80-120">När du har anslutit toohttps://media.windows.net, får du en 301 omdirigering att ange en annan Media Services-URI.</span><span class="sxs-lookup"><span data-stu-id="10d80-120">After successfully connecting toohttps://media.windows.net, you will receive a 301 redirect specifying another Media Services URI.</span></span> <span data-ttu-id="10d80-121">Du måste göra följande anrop toohello ny URI.</span><span class="sxs-lookup"><span data-stu-id="10d80-121">You must make subsequent calls toohello new URI.</span></span>

## <a name="create-an-ondemand-streaming-locator"></a><span data-ttu-id="10d80-122">Skapa en OnDemand-strömning lokaliserare</span><span class="sxs-lookup"><span data-stu-id="10d80-122">Create an OnDemand streaming locator</span></span>
<span data-ttu-id="10d80-123">toocreate hello OnDemand-strömning positionerare och hämta URL: er som du behöver toodo hello följande:</span><span class="sxs-lookup"><span data-stu-id="10d80-123">toocreate hello OnDemand streaming locator and get URLs you need toodo hello following:</span></span>

1. <span data-ttu-id="10d80-124">Definiera en åtkomstprincip om hello innehåll är krypterad.</span><span class="sxs-lookup"><span data-stu-id="10d80-124">If hello content is encrypted, define an access policy.</span></span>
2. <span data-ttu-id="10d80-125">Skapa en OnDemand-strömning lokaliserare.</span><span class="sxs-lookup"><span data-stu-id="10d80-125">Create an OnDemand streaming locator.</span></span>
3. <span data-ttu-id="10d80-126">Om du planerar toostream hämta hello strömning manifestfilen (.ism) i hello tillgången.</span><span class="sxs-lookup"><span data-stu-id="10d80-126">If you plan toostream, get hello streaming manifest file (.ism) in hello asset.</span></span> 
   
   <span data-ttu-id="10d80-127">Om du planerar tooprogressively download hämta hello namnen på MP4-filer i hello tillgången.</span><span class="sxs-lookup"><span data-stu-id="10d80-127">If you plan tooprogressively download, get hello names of MP4 files in hello asset.</span></span> 
4. <span data-ttu-id="10d80-128">Skapa URL: er toohello manifestfilen eller MP4-filer.</span><span class="sxs-lookup"><span data-stu-id="10d80-128">Build URLs toohello manifest file or MP4 files.</span></span> 
5. <span data-ttu-id="10d80-129">Observera att du inte kan skapa en strömningslokaliserare med hjälp av en AccessPolicy som innehåller skriva eller ta bort behörigheter.</span><span class="sxs-lookup"><span data-stu-id="10d80-129">Note that you cannot create a streaming locator using an AccessPolicy that includes write or delete permissions.</span></span>

### <a name="create-an-access-policy"></a><span data-ttu-id="10d80-130">Skapa en åtkomstprincip</span><span class="sxs-lookup"><span data-stu-id="10d80-130">Create an access policy</span></span>

>[!NOTE]
><span data-ttu-id="10d80-131">Det finns en gräns på 1 000 000 principer för olika AMS-principer (till exempel för positionerarprincipen eller ContentKeyAuthorizationPolicy).</span><span class="sxs-lookup"><span data-stu-id="10d80-131">There is a limit of 1,000,000 policies for different AMS policies (for example, for Locator policy or ContentKeyAuthorizationPolicy).</span></span> <span data-ttu-id="10d80-132">Du bör använda hello samma princip-ID om du alltid använder hello samma dagar / åtkomstbehörigheter, till exempel principer för lokaliserare som är avsedda tooremain på plats för lång tid (icke-överföringen principer).</span><span class="sxs-lookup"><span data-stu-id="10d80-132">You should use hello same policy ID if you are always using hello same days / access permissions, for example, policies for locators that are intended tooremain in place for a long time (non-upload policies).</span></span> <span data-ttu-id="10d80-133">Mer information finns i [detta](media-services-dotnet-manage-entities.md#limit-access-policies) avsnitt.</span><span class="sxs-lookup"><span data-stu-id="10d80-133">For more information, see [this](media-services-dotnet-manage-entities.md#limit-access-policies) topic.</span></span>

<span data-ttu-id="10d80-134">Begäran:</span><span class="sxs-lookup"><span data-stu-id="10d80-134">Request:</span></span>

    POST https://media.windows.net/api/AccessPolicies HTTP/1.1
    Content-Type: application/json
    DataServiceVersion: 1.0;NetFx
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json
    Accept-Charset: UTF-8
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=amstest1&urn%3aSubscriptionId=zbbef702-e769-2233-9f16-bc4d3aa97387&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1424263184&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=NWE%2f986Hr5lZTzVGKtC%2ftzHm9n6U%2fxpTFULItxKUGC4%3d
    x-ms-version: 2.11
    x-ms-client-request-id: 6bcfd511-a561-448d-a022-a319a89ecffa
    Host: media.windows.net
    Content-Length: 68

    {"Name":"access policy","DurationInMinutes":43200.0,"Permissions":1}

<span data-ttu-id="10d80-135">Svar:</span><span class="sxs-lookup"><span data-stu-id="10d80-135">Response:</span></span>

    HTTP/1.1 201 Created
    Cache-Control: no-cache
    Content-Length: 311
    Content-Type: application/json;odata=minimalmetadata;streaming=true;charset=utf-8
    Location: https:/media.windows.net/api/AccessPolicies('nb%3Apid%3AUUID%3A69c80d98-7830-407f-a9af-e25f4b0d3e5f')
    Server: Microsoft-IIS/8.5
    request-id: a877528a-bdb4-4414-9862-273f8e64f882
    x-ms-request-id: a877528a-bdb4-4414-9862-273f8e64f882
    x-ms-client-request-id: 6bcfd511-a561-448d-a022-a319a89ecffa
    X-Content-Type-Options: nosniff
    DataServiceVersion: 3.0;
    X-Powered-By: ASP.NET
    Strict-Transport-Security: max-age=31536000; includeSubDomains
    Date: Wed, 18 Feb 2015 06:52:09 GMT

    {"odata.metadata":"https://media.windows.net/api/$metadata#AccessPolicies/@Element","Id":"nb:pid:UUID:69c80d98-7830-407f-a9af-e25f4b0d3e5f","Created":"2015-02-18T06:52:09.8862191Z","LastModified":"2015-02-18T06:52:09.8862191Z","Name":"access policy","DurationInMinutes":43200.0,"Permissions":1}

### <a name="create-an-ondemand-streaming-locator"></a><span data-ttu-id="10d80-136">Skapa en OnDemand-strömning lokaliserare</span><span class="sxs-lookup"><span data-stu-id="10d80-136">Create an OnDemand streaming locator</span></span>
<span data-ttu-id="10d80-137">Skapa hello positionerare för hello angivna tillgången och tillgångsinformation principen.</span><span class="sxs-lookup"><span data-stu-id="10d80-137">Create hello locator for hello specified asset and asset policy.</span></span>

<span data-ttu-id="10d80-138">Begäran:</span><span class="sxs-lookup"><span data-stu-id="10d80-138">Request:</span></span>

    POST https://media.windows.net/api/Locators HTTP/1.1
    Content-Type: application/json
    DataServiceVersion: 1.0;NetFx
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json
    Accept-Charset: UTF-8
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=amstest1&urn%3aSubscriptionId=zbbef702-e769-2233-9f16-bc4d3aa97387&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1424263184&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=NWE%2f986Hr5lZTzVGKtC%2ftzHm9n6U%2fxpTFULItxKUGC4%3d
    x-ms-version: 2.11
    x-ms-client-request-id: ac159492-9a0c-40c3-aacc-551b1b4c5f62
    Host: media.windows.net
    Content-Length: 181

    {"AccessPolicyId":"nb:pid:UUID:1480030d-c481-430a-9687-535c6a5cb272","AssetId":"nb:cid:UUID:cc1e445d-1500-80bd-538e-f1e4b71b465e","StartTime":"2015-02-18T06:34:47.267872Z","Type":2}

<span data-ttu-id="10d80-139">Svar:</span><span class="sxs-lookup"><span data-stu-id="10d80-139">Response:</span></span>

    HTTP/1.1 201 Created
    Cache-Control: no-cache
    Content-Length: 637
    Content-Type: application/json;odata=minimalmetadata;streaming=true;charset=utf-8
    Location: https://media.windows.net/api/Locators('nb%3Alid%3AUUID%3Abe245661-2bbd-4fc6-b14f-9cf9a1492e5e')
    Server: Microsoft-IIS/8.5
    request-id: 5bd5864a-0afd-44c0-a67a-4044a2c9043b
    x-ms-request-id: 5bd5864a-0afd-44c0-a67a-4044a2c9043b
    x-ms-client-request-id: ac159492-9a0c-40c3-aacc-551b1b4c5f62
    X-Content-Type-Options: nosniff
    DataServiceVersion: 3.0;
    X-Powered-By: ASP.NET
    Strict-Transport-Security: max-age=31536000; includeSubDomains
    Date: Wed, 18 Feb 2015 06:58:37 GMT

    {"odata.metadata":"https://media.windows.net/api/$metadata#Locators/@Element","Id":"nb:lid:UUID:be245661-2bbd-4fc6-b14f-9cf9a1492e5e","ExpirationDateTime":"2015-03-20T06:34:47.267872+00:00","Type":2,"Path":"http://amstest1.streaming.mediaservices.windows.net/be245661-2bbd-4fc6-b14f-9cf9a1492e5e/","BaseUri":"http://amstest1.streaming.mediaservices.windows.net","ContentAccessComponent":"be245661-2bbd-4fc6-b14f-9cf9a1492e5e","AccessPolicyId":"nb:pid:UUID:1480030d-c481-430a-9687-535c6a5cb272","AssetId":"nb:cid:UUID:cc1e445d-1500-80bd-538e-f1e4b71b465e","StartTime":"2015-02-18T06:34:47.267872+00:00","Name":null}

### <a name="build-streaming-urls"></a><span data-ttu-id="10d80-140">Skapa URL: er för strömning</span><span class="sxs-lookup"><span data-stu-id="10d80-140">Build streaming URLs</span></span>
<span data-ttu-id="10d80-141">Använd hello **sökväg** värdet som returneras efter hello generering av hello lokaliserare toobuild hello Smooth, HLS och MPEG DASH URL: er.</span><span class="sxs-lookup"><span data-stu-id="10d80-141">Use hello **Path** value returned after hello creation of hello locator toobuild hello Smooth, HLS, and MPEG DASH URLs.</span></span> 

<span data-ttu-id="10d80-142">Smooth Streaming: **sökväg** + Manifestfilens filnamn + ”/ manifest”</span><span class="sxs-lookup"><span data-stu-id="10d80-142">Smooth Streaming: **Path** + manifest file name + "/manifest"</span></span>

<span data-ttu-id="10d80-143">Exempel:</span><span class="sxs-lookup"><span data-stu-id="10d80-143">example:</span></span>

    http://amstest1.streaming.mediaservices.windows.net/3c5fe676-199c-4620-9b03-ba014900f214/BigBuckBunny.ism/manifest

<span data-ttu-id="10d80-144">HLS: **sökväg** + Manifestfilens filnamn + ”/ manifest(format=m3u8-aapl)”</span><span class="sxs-lookup"><span data-stu-id="10d80-144">HLS: **Path** + manifest file name + "/manifest(format=m3u8-aapl)"</span></span>

<span data-ttu-id="10d80-145">Exempel:</span><span class="sxs-lookup"><span data-stu-id="10d80-145">example:</span></span>

    http://amstest1.streaming.mediaservices.windows.net/3c5fe676-199c-4620-9b03-ba014900f214/BigBuckBunny.ism/manifest(format=m3u8-aapl)


<span data-ttu-id="10d80-146">STRECK: **sökväg** + Manifestfilens filnamn + ”/ manifest(format=mpd-time-csf)”</span><span class="sxs-lookup"><span data-stu-id="10d80-146">DASH: **Path** + manifest file name + "/manifest(format=mpd-time-csf)"</span></span>

<span data-ttu-id="10d80-147">Exempel:</span><span class="sxs-lookup"><span data-stu-id="10d80-147">example:</span></span>

    http://amstest1.streaming.mediaservices.windows.net/3c5fe676-199c-4620-9b03-ba014900f214/BigBuckBunny.ism/manifest(format=mpd-time-csf)


### <a name="build-progressive-download-urls"></a><span data-ttu-id="10d80-148">Skapa URL: er för progressiv hämtning</span><span class="sxs-lookup"><span data-stu-id="10d80-148">Build progressive download URLs</span></span>
<span data-ttu-id="10d80-149">Använd hello **sökväg** värdet som returneras efter hello generering av hello lokaliserare toobuild hello progressiv nedladdning URL.</span><span class="sxs-lookup"><span data-stu-id="10d80-149">Use hello **Path** value returned after hello creation of hello locator toobuild hello progressive download URL.</span></span>   

<span data-ttu-id="10d80-150">URL: **sökväg** + tillgången mp4 filnamn</span><span class="sxs-lookup"><span data-stu-id="10d80-150">URL: **Path** + asset file mp4 name</span></span>

<span data-ttu-id="10d80-151">Exempel:</span><span class="sxs-lookup"><span data-stu-id="10d80-151">example:</span></span>

    http://amstest1.streaming.mediaservices.windows.net/3c5fe676-199c-4620-9b03-ba014900f214/BigBuckBunny_H264_650kbps_AAC_und_ch2_96kbps.mp4

## <span data-ttu-id="10d80-152"><a id="types"></a>Enum-typer</span><span class="sxs-lookup"><span data-stu-id="10d80-152"><a id="types"></a>Enum types</span></span>
    [Flags]
    public enum AccessPermissions
    {
        None = 0,
        Read = 1,
        Write = 2,
        Delete = 4,
        List = 8,
    }

    public enum LocatorType
    {
        None = 0,
        Sas = 1,
        OnDemandOrigin = 2,
    }

## <a name="media-services-learning-paths"></a><span data-ttu-id="10d80-153">Sökvägar för Media Services-utbildning</span><span class="sxs-lookup"><span data-stu-id="10d80-153">Media Services learning paths</span></span>
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a><span data-ttu-id="10d80-154">Ge feedback</span><span class="sxs-lookup"><span data-stu-id="10d80-154">Provide feedback</span></span>
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

## <a name="see-also"></a><span data-ttu-id="10d80-155">Se även</span><span class="sxs-lookup"><span data-stu-id="10d80-155">See also</span></span>
[<span data-ttu-id="10d80-156">Media Services operations REST API-översikt</span><span class="sxs-lookup"><span data-stu-id="10d80-156">Media Services operations REST API overview</span></span>](media-services-rest-how-to-use.md)

[<span data-ttu-id="10d80-157">Konfigurera tillgångsleveransprincip</span><span class="sxs-lookup"><span data-stu-id="10d80-157">Configure asset delivery policy</span></span>](media-services-rest-configure-asset-delivery-policy.md)

