---
title: aaaCreating filter med Azure Media Services REST API | Microsoft Docs
description: "Det här avsnittet beskrivs hur toocreate filtrerar så att klienten kan använda dem toostream vissa delar av en dataström. Media Services skapar dynamiska manifesten tooachieve denna selektiv strömning."
services: media-services
documentationcenter: 
author: Juliako
manager: cfowler
editor: 
ms.assetid: f7d23daf-7cd2-49c7-a195-ab902912ab3c
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: ne
ms.topic: article
ms.date: 08/10/2017
ms.author: juliako;cenkdin
ms.openlocfilehash: d0b5af3b193b35f22ac70887963c2f0a06b60bde
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="creating-filters-with-azure-media-services-rest-api"></a><span data-ttu-id="03816-104">Skapa filter med Azure Media Services REST API</span><span class="sxs-lookup"><span data-stu-id="03816-104">Creating Filters with Azure Media Services REST API</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="03816-105">.NET</span><span class="sxs-lookup"><span data-stu-id="03816-105">.NET</span></span>](media-services-dotnet-dynamic-manifest.md)
> * [<span data-ttu-id="03816-106">REST</span><span class="sxs-lookup"><span data-stu-id="03816-106">REST</span></span>](media-services-rest-dynamic-manifest.md)
> 
> 

<span data-ttu-id="03816-107">Från och med 2.11 Media Services kan du toodefine filter för dina tillgångar.</span><span class="sxs-lookup"><span data-stu-id="03816-107">Starting with 2.11 release, Media Services enables you toodefine filters for your assets.</span></span> <span data-ttu-id="03816-108">Dessa filter är server side regler som tillåter toochoose toodo sådant som dina kunder: uppspelning endast en del av en video (i stället för att spela upp hello hela video), eller ange bara en delmängd av ljud och video återgivningar att kundens enhet kan hantera () i stället för alla hello återgivningar som har associerats med hello tillgången).</span><span class="sxs-lookup"><span data-stu-id="03816-108">These filters are server side rules that will allow your customers toochoose toodo things like: playback only a section of a video (instead of playing hello whole video), or specify only a subset of audio and video renditions that your customer's device can handle (instead of all hello renditions that are associated with hello asset).</span></span> <span data-ttu-id="03816-109">Den här filtreringen dina tillgångar arkiveras via **dynamiska Manifest**som skapas vid kundens begäran toostream en video baserat på angivna filter.</span><span class="sxs-lookup"><span data-stu-id="03816-109">This filtering of your assets is archived through **Dynamic Manifest**s that are created upon your customer's request toostream a video based on specified filter(s).</span></span>

<span data-ttu-id="03816-110">Mer detaljerad information relaterad toofilters och dynamiska Manifest finns [dynamiska visar en översikt över](media-services-dynamic-manifest-overview.md).</span><span class="sxs-lookup"><span data-stu-id="03816-110">For more detailed information related toofilters and Dynamic Manifest, see [Dynamic manifests overview](media-services-dynamic-manifest-overview.md).</span></span>

<span data-ttu-id="03816-111">Det här avsnittet visar hur toouse REST API: er toocreate, uppdatera och ta bort filter.</span><span class="sxs-lookup"><span data-stu-id="03816-111">This topic shows how toouse REST APIs toocreate, update, and delete filters.</span></span> 

## <a name="types-used-toocreate-filters"></a><span data-ttu-id="03816-112">Använda typer toocreate filter</span><span class="sxs-lookup"><span data-stu-id="03816-112">Types used toocreate filters</span></span>
<span data-ttu-id="03816-113">hello som följande typer används när du skapar filter:</span><span class="sxs-lookup"><span data-stu-id="03816-113">hello following types are used when creating filters:</span></span>  

* [<span data-ttu-id="03816-114">Filter</span><span class="sxs-lookup"><span data-stu-id="03816-114">Filter</span></span>](https://docs.microsoft.com/rest/api/media/operations/filter)
* [<span data-ttu-id="03816-115">AssetFilter</span><span class="sxs-lookup"><span data-stu-id="03816-115">AssetFilter</span></span>](https://docs.microsoft.com/rest/api/media/operations/assetfilter)
* [<span data-ttu-id="03816-116">PresentationTimeRange</span><span class="sxs-lookup"><span data-stu-id="03816-116">PresentationTimeRange</span></span>](https://docs.microsoft.com/rest/api/media/operations/presentationtimerange)
* [<span data-ttu-id="03816-117">FilterTrackSelect och FilterTrackPropertyCondition</span><span class="sxs-lookup"><span data-stu-id="03816-117">FilterTrackSelect and FilterTrackPropertyCondition</span></span>](https://docs.microsoft.com/rest/api/media/operations/filtertrackselect)

>[!NOTE]

><span data-ttu-id="03816-118">Vid åtkomst till entiteter i Media Services måste du ange specifika namn på huvudfält och värden i HTTP-begäranden.</span><span class="sxs-lookup"><span data-stu-id="03816-118">When accessing entities in Media Services, you must set specific header fields and values in your HTTP requests.</span></span> <span data-ttu-id="03816-119">Mer information finns i [installationsprogrammet för Media Services REST API-utveckling](media-services-rest-how-to-use.md).</span><span class="sxs-lookup"><span data-stu-id="03816-119">For more information, see [Setup for Media Services REST API Development](media-services-rest-how-to-use.md).</span></span>

## <a name="connect-toomedia-services"></a><span data-ttu-id="03816-120">Ansluta tooMedia tjänster</span><span class="sxs-lookup"><span data-stu-id="03816-120">Connect tooMedia Services</span></span>

<span data-ttu-id="03816-121">Mer information om hur tooconnect toohello AMS API, se [åtkomst hello Azure Media Services API med Azure AD authentication](media-services-use-aad-auth-to-access-ams-api.md).</span><span class="sxs-lookup"><span data-stu-id="03816-121">For information on how tooconnect toohello AMS API, see [Access hello Azure Media Services API with Azure AD authentication](media-services-use-aad-auth-to-access-ams-api.md).</span></span> 

>[!NOTE]
><span data-ttu-id="03816-122">När du har anslutit toohttps://media.windows.net, får du en 301 omdirigering att ange en annan Media Services-URI.</span><span class="sxs-lookup"><span data-stu-id="03816-122">After successfully connecting toohttps://media.windows.net, you will receive a 301 redirect specifying another Media Services URI.</span></span> <span data-ttu-id="03816-123">Du måste göra följande anrop toohello ny URI.</span><span class="sxs-lookup"><span data-stu-id="03816-123">You must make subsequent calls toohello new URI.</span></span>

## <a name="create-filters"></a><span data-ttu-id="03816-124">Skapa filter</span><span class="sxs-lookup"><span data-stu-id="03816-124">Create filters</span></span>
### <a name="create-global-filters"></a><span data-ttu-id="03816-125">Skapa globala filter</span><span class="sxs-lookup"><span data-stu-id="03816-125">Create global Filters</span></span>
<span data-ttu-id="03816-126">toocreate ett globalt Filter med hello efter HTTP-begäranden:</span><span class="sxs-lookup"><span data-stu-id="03816-126">toocreate a global Filter, use hello following HTTP requests:</span></span>  

#### <a name="http-request"></a><span data-ttu-id="03816-127">HTTP-begäran</span><span class="sxs-lookup"><span data-stu-id="03816-127">HTTP Request</span></span>
<span data-ttu-id="03816-128">Huvuden för begäran</span><span class="sxs-lookup"><span data-stu-id="03816-128">Request Headers</span></span>

    POST https://media.windows.net/API/Filters HTTP/1.1 
    DataServiceVersion:3.0 
    MaxDataServiceVersion: 3.0 
    Content-Type: application/json 
    Accept: application/json 
    Accept-Charset: UTF-8 
    Authorization: Bearer <token value> 
    x-ms-version: 2.11 
    x-ms-client-request-id: 00000000-0000-0000-0000-000000000000 
    Host:media.windows.net 

<span data-ttu-id="03816-129">Begärandetexten</span><span class="sxs-lookup"><span data-stu-id="03816-129">Request body</span></span> 

    {  
       "Name":"GlobalFilter",
       "PresentationTimeRange":{  
          "StartTimestamp":"0",
          "EndTimestamp":"9223372036854775807",
          "PresentationWindowDuration":"12000000000",
          "LiveBackoffDuration":"0",
          "Timescale":"10000000"
       },
       "Tracks":[  
          {  
             "PropertyConditions":
                  [  
                {  
                   "Property":"Type",
                   "Value":"audio",
                   "Operator":"Equal"
                },
                {  
                   "Property":"Bitrate",
                   "Value":"0-2147483647",
                   "Operator":"Equal"
                }
             ]
          }
       ]
    }




#### <a name="http-response"></a><span data-ttu-id="03816-130">HTTP-svar</span><span class="sxs-lookup"><span data-stu-id="03816-130">HTTP Response</span></span>
    HTTP/1.1 201 Created 

### <a name="create-local-assetfilters"></a><span data-ttu-id="03816-131">Skapa lokala AssetFilters</span><span class="sxs-lookup"><span data-stu-id="03816-131">Create local AssetFilters</span></span>
<span data-ttu-id="03816-132">toocreate lokala AssetFilter, Använd hello efter HTTP-begäranden:</span><span class="sxs-lookup"><span data-stu-id="03816-132">toocreate a local AssetFilter, use hello following HTTP requests:</span></span>  

#### <a name="http-request"></a><span data-ttu-id="03816-133">HTTP-begäran</span><span class="sxs-lookup"><span data-stu-id="03816-133">HTTP Request</span></span>
<span data-ttu-id="03816-134">Huvuden för begäran</span><span class="sxs-lookup"><span data-stu-id="03816-134">Request Headers</span></span>

    POST https://media.windows.net/API/AssetFilters HTTP/1.1 
    DataServiceVersion: 3.0 
    MaxDataServiceVersion: 3.0 
    Content-Type: application/json 
    Accept: application/json 
    Accept-Charset: UTF-8 
    Authorization: Bearer <token value> 
    x-ms-version: 2.11 
    x-ms-client-request-id: 00000000-0000-0000-0000-000000000000 
    Host: media.windows.net  

<span data-ttu-id="03816-135">Begärandetexten</span><span class="sxs-lookup"><span data-stu-id="03816-135">Request body</span></span> 

    {   
       "Name":"AssetFilter", 
       "ParentAssetId":"nb:cid:UUID:536e555d-1500-80c3-92dc-f1e4fdc6c592", 
       "PresentationTimeRange":{   
          "StartTimestamp":"0", 
          "EndTimestamp":"9223372036854775807", 
          "PresentationWindowDuration":"12000000000", 
          "LiveBackoffDuration":"0", 
          "Timescale":"10000000" 
       }, 
       "Tracks":[   
          {   
             "PropertyConditions": 
                  [   
                {   
                   "Property":"Type", 
                   "Value":"audio", 
                   "Operator":"Equal" 
                }, 
                {   
                   "Property":"Bitrate", 
                   "Value":"0-2147483647", 
                   "Operator":"Equal" 
                } 
             ] 
          } 
       ] 
    } 

#### <a name="http-response"></a><span data-ttu-id="03816-136">HTTP-svar</span><span class="sxs-lookup"><span data-stu-id="03816-136">HTTP Response</span></span>
    HTTP/1.1 201 Created 
    . . . 

## <a name="list-filters"></a><span data-ttu-id="03816-137">Visa filter</span><span class="sxs-lookup"><span data-stu-id="03816-137">List filters</span></span>
### <a name="get-all-global-filters-in-hello-ams-account"></a><span data-ttu-id="03816-138">Hämta alla globala **Filter**s i hello AMS-konto</span><span class="sxs-lookup"><span data-stu-id="03816-138">Get all global **Filter**s in hello AMS account</span></span>
<span data-ttu-id="03816-139">toolist filter, Använd hello efter HTTP-begäranden:</span><span class="sxs-lookup"><span data-stu-id="03816-139">toolist filters, use hello following HTTP requests:</span></span> 

#### <a name="http-request"></a><span data-ttu-id="03816-140">HTTP-begäran</span><span class="sxs-lookup"><span data-stu-id="03816-140">HTTP Request</span></span>
    GET https://media.windows.net/API/Filters HTTP/1.1 
    DataServiceVersion:3.0 
    MaxDataServiceVersion: 3.0 
    Accept: application/json 
    Accept-Charset: UTF-8 
    Authorization: Bearer <token value> 
    x-ms-version: 2.11 
    Host: media.windows.net 

### <a name="get-assetfilters-associated-with-an-asset"></a><span data-ttu-id="03816-141">Hämta **AssetFilter**s som är kopplade till en tillgång</span><span class="sxs-lookup"><span data-stu-id="03816-141">Get **AssetFilter**s associated with an asset</span></span>
#### <a name="http-request"></a><span data-ttu-id="03816-142">HTTP-begäran</span><span class="sxs-lookup"><span data-stu-id="03816-142">HTTP Request</span></span>
    GET https://media.windows.net/API/Assets('nb%3Acid%3AUUID%3A536e555d-1500-80c3-92dc-f1e4fdc6c592')/AssetFilters HTTP/1.1 
    DataServiceVersion: 3.0 
    MaxDataServiceVersion: 3.0 
    Accept: application/json 
    Accept-Charset: UTF-8 
    Authorization: Bearer <token value> 
    x-ms-version: 2.11 
    x-ms-client-request-id: 00000000-0000-0000-0000-000000000000 
    Host: media.windows.net 

### <a name="get-an-assetfilter-based-on-its-id"></a><span data-ttu-id="03816-143">Hämta en **AssetFilter** baserat på dess Id</span><span class="sxs-lookup"><span data-stu-id="03816-143">Get an **AssetFilter** based on its Id</span></span>
#### <a name="http-request"></a><span data-ttu-id="03816-144">HTTP-begäran</span><span class="sxs-lookup"><span data-stu-id="03816-144">HTTP Request</span></span>
    GET https://media.windows.net/API/AssetFilters('nb%3Acid%3AUUID%3A536e555d-1500-80c3-92dc-f1e4fdc6c592__%23%23%23__TestFilter') HTTP/1.1 
    DataServiceVersion: 3.0 
    MaxDataServiceVersion: 3.0 
    Accept: application/json 
    Accept-Charset: UTF-8 
    Authorization: Bearer <token value> 
    x-ms-version: 2.11 
    x-ms-client-request-id: 00000000


## <a name="update-filters"></a><span data-ttu-id="03816-145">Uppdatera filter</span><span class="sxs-lookup"><span data-stu-id="03816-145">Update filters</span></span>
<span data-ttu-id="03816-146">Använd KORRIGERA PUT- eller MERGE tooupdate ett filter med nya värden.</span><span class="sxs-lookup"><span data-stu-id="03816-146">Use PATCH, PUT or MERGE tooupdate a filter with new property values.</span></span>  <span data-ttu-id="03816-147">Mer information om dessa åtgärder finns [korrigering, PUT, MERGE](http://msdn.microsoft.com/library/dd541276.aspx).</span><span class="sxs-lookup"><span data-stu-id="03816-147">For more information about these operations, see [PATCH, PUT, MERGE](http://msdn.microsoft.com/library/dd541276.aspx).</span></span>

<span data-ttu-id="03816-148">Om du uppdaterar ett filter kan ta det upp too2 minuter för strömmande slutpunkt toorefresh hello regler.</span><span class="sxs-lookup"><span data-stu-id="03816-148">If you update a filter, it can take up too2 minutes for streaming endpoint toorefresh hello rules.</span></span> <span data-ttu-id="03816-149">Om hello innehåll behandlades med filtret (och cachelagras i proxyservrar och CDN cacheminnen), kan uppdatera det här filtret resultera i player-fel.</span><span class="sxs-lookup"><span data-stu-id="03816-149">If hello content was served using this filter (and cached in proxies and CDN caches), updating this filter can result in player failures.</span></span> <span data-ttu-id="03816-150">Rekommenderas tooclear hello cachen när du har uppdaterat hello filter.</span><span class="sxs-lookup"><span data-stu-id="03816-150">It is recommend tooclear hello cache after updating hello filter.</span></span> <span data-ttu-id="03816-151">Om det här alternativet inte är möjligt bör du använda ett annat filter.</span><span class="sxs-lookup"><span data-stu-id="03816-151">If this option is not possible, consider using a different filter.</span></span>  

### <a name="update-global-filters"></a><span data-ttu-id="03816-152">Uppdatera globala filter</span><span class="sxs-lookup"><span data-stu-id="03816-152">Update global Filters</span></span>
<span data-ttu-id="03816-153">tooupdate ett globalt filter med hello efter HTTP-begäranden:</span><span class="sxs-lookup"><span data-stu-id="03816-153">tooupdate a global filter, use hello following HTTP requests:</span></span> 

#### <a name="http-request"></a><span data-ttu-id="03816-154">HTTP-begäran</span><span class="sxs-lookup"><span data-stu-id="03816-154">HTTP Request</span></span>
<span data-ttu-id="03816-155">Huvuden för begäran:</span><span class="sxs-lookup"><span data-stu-id="03816-155">Request headers:</span></span> 

    MERGE https://media.windows.net/API/Filters('filterName') HTTP/1.1 
    DataServiceVersion:3.0 
    MaxDataServiceVersion: 3.0 
    Content-Type: application/json 
    Accept: application/json 
    Accept-Charset: UTF-8 
    Authorization: Bearer <token value> 
    x-ms-version: 2.11 
    x-ms-client-request-id: 00000000-0000-0000-0000-000000000000 
    Host: media.windows.net 
    Content-Length: 384

<span data-ttu-id="03816-156">Begärandetexten:</span><span class="sxs-lookup"><span data-stu-id="03816-156">Request body:</span></span> 

    { 
       "Tracks":[   
          {   
             "PropertyConditions": 
             [   
                {   
                   "Property":"Type", 
                   "Value":"audio", 
                   "Operator":"Equal" 
                }, 
                {   
                   "Property":"Bitrate", 
                   "Value":"0-2147483647", 
                   "Operator":"Equal" 
                } 
             ] 
          } 
       ] 
    } 

### <a name="update-local-assetfilters"></a><span data-ttu-id="03816-157">Uppdatera lokala AssetFilters</span><span class="sxs-lookup"><span data-stu-id="03816-157">Update local AssetFilters</span></span>
<span data-ttu-id="03816-158">tooupdate lokala filter, Använd hello efter HTTP-begäranden:</span><span class="sxs-lookup"><span data-stu-id="03816-158">tooupdate a local filter, use hello following HTTP requests:</span></span> 

#### <a name="http-request"></a><span data-ttu-id="03816-159">HTTP-begäran</span><span class="sxs-lookup"><span data-stu-id="03816-159">HTTP Request</span></span>
<span data-ttu-id="03816-160">Huvuden för begäran:</span><span class="sxs-lookup"><span data-stu-id="03816-160">Request headers:</span></span> 

    MERGE https://media.windows.net/API/AssetFilters('nb%3Acid%3AUUID%3A536e555d-1500-80c3-92dc-f1e4fdc6c592__%23%23%23__TestFilter')  HTTP/1.1 
    DataServiceVersion: 3.0 
    MaxDataServiceVersion: 3.0 
    Content-Type: application/json 
    Accept: application/json 
    Accept-Charset: UTF-8 
    Authorization: Bearer <token value> 
    x-ms-version: 2.11 
    x-ms-client-request-id: 00000000-0000-0000-0000-000000000000 
    Host: media.windows.net 

<span data-ttu-id="03816-161">Begärandetexten:</span><span class="sxs-lookup"><span data-stu-id="03816-161">Request body:</span></span> 

    { 
       "Tracks":[   
          {   
             "PropertyConditions": 
             [   
                {   
                   "Property":"Type", 
                   "Value":"audio", 
                   "Operator":"Equal" 
                }, 
                {   
                   "Property":"Bitrate", 
                   "Value":"0-2147483647", 
                   "Operator":"Equal" 
                } 
             ] 
          } 
       ] 
    } 


## <a name="delete-filters"></a><span data-ttu-id="03816-162">Ta bort filter</span><span class="sxs-lookup"><span data-stu-id="03816-162">Delete filters</span></span>
### <a name="delete-global-filters"></a><span data-ttu-id="03816-163">Ta bort globala filter</span><span class="sxs-lookup"><span data-stu-id="03816-163">Delete global Filters</span></span>
<span data-ttu-id="03816-164">toodelete ett globalt Filter med hello efter HTTP-begäranden:</span><span class="sxs-lookup"><span data-stu-id="03816-164">toodelete a global Filter, use hello following HTTP requests:</span></span>

#### <a name="http-request"></a><span data-ttu-id="03816-165">HTTP-begäran</span><span class="sxs-lookup"><span data-stu-id="03816-165">HTTP Request</span></span>
    DELETE https://media.windows.net/api/Filters('GlobalFilter') HTTP/1.1 
    DataServiceVersion:3.0 
    MaxDataServiceVersion: 3.0 
    Accept: application/json 
    Accept-Charset: UTF-8 
    Authorization: Bearer <token value> 
    x-ms-version: 2.11 
    Host: media.windows.net 


### <a name="delete-local-assetfilters"></a><span data-ttu-id="03816-166">Ta bort lokala AssetFilters</span><span class="sxs-lookup"><span data-stu-id="03816-166">Delete local AssetFilters</span></span>
<span data-ttu-id="03816-167">toodelete lokala AssetFilter, Använd hello efter HTTP-begäranden:</span><span class="sxs-lookup"><span data-stu-id="03816-167">toodelete a local AssetFilter, use hello following HTTP requests:</span></span>

#### <a name="http-request"></a><span data-ttu-id="03816-168">HTTP-begäran</span><span class="sxs-lookup"><span data-stu-id="03816-168">HTTP Request</span></span>
    DELETE https://media.windows.net/API/AssetFilters('nb%3Acid%3AUUID%3A536e555d-1500-80c3-92dc-f1e4fdc6c592__%23%23%23__LocalFilter') HTTP/1.1 
    DataServiceVersion: 3.0 
    MaxDataServiceVersion: 3.0 
    Accept: application/json 
    Accept-Charset: UTF-8 
    Authorization: Bearer <token value> 
    x-ms-version: 2.11 
    Host: media.windows.net 

## <a name="build-streaming-urls-that-use-filters"></a><span data-ttu-id="03816-169">Skapa strömning URL: er som använder filter</span><span class="sxs-lookup"><span data-stu-id="03816-169">Build streaming URLs that use filters</span></span>
<span data-ttu-id="03816-170">Mer information om hur toopublish och leverera dina tillgångar, se [leverera innehåll tooCustomers översikt](media-services-deliver-content-overview.md).</span><span class="sxs-lookup"><span data-stu-id="03816-170">For information on how toopublish and deliver your assets, see [Delivering Content tooCustomers Overview](media-services-deliver-content-overview.md).</span></span>

<span data-ttu-id="03816-171">hello följande exempel visar hur tooadd filtrerar tooyour URL: er för strömning.</span><span class="sxs-lookup"><span data-stu-id="03816-171">hello following examples show how tooadd filters tooyour streaming URLs.</span></span>

<span data-ttu-id="03816-172">**MPEG DASH**</span><span class="sxs-lookup"><span data-stu-id="03816-172">**MPEG DASH**</span></span> 

    http://testendpoint-testaccount.streaming.mediaservices.windows.net/fecebb23-46f6-490d-8b70-203e86b0df58/BigBuckBunny.ism/Manifest(format=mpd-time-csf, filter=MyFilter)

<span data-ttu-id="03816-173">**Apple HTTP Live Streaming (HLS) V4**</span><span class="sxs-lookup"><span data-stu-id="03816-173">**Apple HTTP Live Streaming (HLS) V4**</span></span>

    http://testendpoint-testaccount.streaming.mediaservices.windows.net/fecebb23-46f6-490d-8b70-203e86b0df58/BigBuckBunny.ism/Manifest(format=m3u8-aapl, filter=MyFilter)

<span data-ttu-id="03816-174">**Apple HTTP Live Streaming (HLS) V3**</span><span class="sxs-lookup"><span data-stu-id="03816-174">**Apple HTTP Live Streaming (HLS) V3**</span></span>

    http://testendpoint-testaccount.streaming.mediaservices.windows.net/fecebb23-46f6-490d-8b70-203e86b0df58/BigBuckBunny.ism/Manifest(format=m3u8-aapl-v3, filter=MyFilter)

<span data-ttu-id="03816-175">**Smooth Streaming**</span><span class="sxs-lookup"><span data-stu-id="03816-175">**Smooth Streaming**</span></span>

    http://testendpoint-testaccount.streaming.mediaservices.windows.net/fecebb23-46f6-490d-8b70-203e86b0df58/BigBuckBunny.ism/Manifest(filter=MyFilter)

    
## <a name="media-services-learning-paths"></a><span data-ttu-id="03816-176">Sökvägar för Media Services-utbildning</span><span class="sxs-lookup"><span data-stu-id="03816-176">Media Services learning paths</span></span>
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a><span data-ttu-id="03816-177">Ge feedback</span><span class="sxs-lookup"><span data-stu-id="03816-177">Provide feedback</span></span>
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

## <a name="see-also"></a><span data-ttu-id="03816-178">Se även</span><span class="sxs-lookup"><span data-stu-id="03816-178">See Also</span></span>
[<span data-ttu-id="03816-179">Översikt över dynamisk manifest</span><span class="sxs-lookup"><span data-stu-id="03816-179">Dynamic manifests overview</span></span>](media-services-dynamic-manifest-overview.md)

