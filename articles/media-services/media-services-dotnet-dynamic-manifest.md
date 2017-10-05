---
title: Skapa filter med Azure Media Services .NET SDK
description: "Det här avsnittet beskriver hur du skapar filter så att klienten kan använda dem till dataströmmen vissa delar av en dataström. Media Services skapar dynamiska manifest för att uppnå det här selektiv strömning."
services: media-services
documentationcenter: 
author: Juliako
manager: cfowler
editor: 
ms.assetid: 2f6894ca-fb43-43c0-9151-ddbb2833cafd
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: ne
ms.topic: article
ms.date: 07/21/2017
ms.author: juliako;cenkdin
ms.openlocfilehash: 6c43473b86c14679ace558de478bd95f41d476da
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/29/2017
---
# <a name="creating-filters-with-azure-media-services-net-sdk"></a><span data-ttu-id="d9776-104">Skapa filter med Azure Media Services .NET SDK</span><span class="sxs-lookup"><span data-stu-id="d9776-104">Creating Filters with Azure Media Services .NET SDK</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="d9776-105">.NET</span><span class="sxs-lookup"><span data-stu-id="d9776-105">.NET</span></span>](media-services-dotnet-dynamic-manifest.md)
> * [<span data-ttu-id="d9776-106">REST</span><span class="sxs-lookup"><span data-stu-id="d9776-106">REST</span></span>](media-services-rest-dynamic-manifest.md)
> 
> 

<span data-ttu-id="d9776-107">Från och med 2.11 kan Media Services du definiera filter för dina tillgångar.</span><span class="sxs-lookup"><span data-stu-id="d9776-107">Starting with 2.11 release, Media Services enables you to define filters for your assets.</span></span> <span data-ttu-id="d9776-108">Dessa filter är server sida regler som gör att kunderna kan välja att till exempel: uppspelning endast en del av en video (i stället för hela video), eller ange bara en del av ljud och video återgivningar som kundens enhet kan hantera (i stället alla återgivningar som är kopplade till tillgången).</span><span class="sxs-lookup"><span data-stu-id="d9776-108">These filters are server side rules that will allow your customers to choose to do things like: playback only a section of a video (instead of playing the whole video), or specify only a subset of audio and video renditions that your customer's device can handle (instead of all the renditions that are associated with the asset).</span></span> <span data-ttu-id="d9776-109">Den här filtreringen dina tillgångar uppnås genom **dynamiska Manifest**s som skapas på kundens begäran för direktuppspelning av video baserat på angivna filter.</span><span class="sxs-lookup"><span data-stu-id="d9776-109">This filtering of your assets is achieved through **Dynamic Manifest**s that are created upon your customer's request to stream a video based on specified filter(s).</span></span>

<span data-ttu-id="d9776-110">Mer detaljerad information som rör filter och dynamiska Manifest finns [dynamiska visar en översikt över](media-services-dynamic-manifest-overview.md).</span><span class="sxs-lookup"><span data-stu-id="d9776-110">For more detailed information related to filters and Dynamic Manifest, see [Dynamic manifests overview](media-services-dynamic-manifest-overview.md).</span></span>

<span data-ttu-id="d9776-111">Det här avsnittet visar hur du använder Media Services .NET SDK för att skapa, uppdatera och ta bort filter.</span><span class="sxs-lookup"><span data-stu-id="d9776-111">This topic shows how to use Media Services .NET SDK to create, update, and delete filters.</span></span> 

<span data-ttu-id="d9776-112">Observera att om du uppdaterar ett filter kan det ta upp till 2 minuter för strömmande slutpunkten för att uppdatera reglerna.</span><span class="sxs-lookup"><span data-stu-id="d9776-112">Note if you update a filter, it can take up to 2 minutes for streaming endpoint to refresh the rules.</span></span> <span data-ttu-id="d9776-113">Om innehållet behandlades med filtret (och cachelagras i proxyservrar och CDN cacheminnen), kan uppdatera det här filtret resultera i player-fel.</span><span class="sxs-lookup"><span data-stu-id="d9776-113">If the content was served using this filter (and cached in proxies and CDN caches), updating this filter can result in player failures.</span></span> <span data-ttu-id="d9776-114">Det rekommenderas att rensa cachen när du har uppdaterat filtret.</span><span class="sxs-lookup"><span data-stu-id="d9776-114">It is recommend to clear the cache after updating the filter.</span></span> <span data-ttu-id="d9776-115">Om det här alternativet inte är möjligt bör du använda ett annat filter.</span><span class="sxs-lookup"><span data-stu-id="d9776-115">If this option is not possible, consider using a different filter.</span></span> 

## <a name="types-used-to-create-filters"></a><span data-ttu-id="d9776-116">Typer som används för att skapa filter</span><span class="sxs-lookup"><span data-stu-id="d9776-116">Types used to create filters</span></span>
<span data-ttu-id="d9776-117">Följande typer används när du skapar filter:</span><span class="sxs-lookup"><span data-stu-id="d9776-117">The following types are used when creating filters:</span></span> 

* <span data-ttu-id="d9776-118">**IStreamingFilter**.</span><span class="sxs-lookup"><span data-stu-id="d9776-118">**IStreamingFilter**.</span></span>  <span data-ttu-id="d9776-119">Den här typen baserat på följande REST API [Filter](https://docs.microsoft.com/rest/api/media/operations/filter)</span><span class="sxs-lookup"><span data-stu-id="d9776-119">This type is based on the following REST API [Filter](https://docs.microsoft.com/rest/api/media/operations/filter)</span></span>
* <span data-ttu-id="d9776-120">**IStreamingAssetFilter**.</span><span class="sxs-lookup"><span data-stu-id="d9776-120">**IStreamingAssetFilter**.</span></span> <span data-ttu-id="d9776-121">Den här typen baserat på följande REST API [AssetFilter](https://docs.microsoft.com/rest/api/media/operations/assetfilter)</span><span class="sxs-lookup"><span data-stu-id="d9776-121">This type is based on the following REST API [AssetFilter](https://docs.microsoft.com/rest/api/media/operations/assetfilter)</span></span>
* <span data-ttu-id="d9776-122">**PresentationTimeRange**.</span><span class="sxs-lookup"><span data-stu-id="d9776-122">**PresentationTimeRange**.</span></span> <span data-ttu-id="d9776-123">Den här typen baserat på följande REST API [PresentationTimeRange](https://docs.microsoft.com/rest/api/media/operations/presentationtimerange)</span><span class="sxs-lookup"><span data-stu-id="d9776-123">This type is based on the following REST API [PresentationTimeRange](https://docs.microsoft.com/rest/api/media/operations/presentationtimerange)</span></span>
* <span data-ttu-id="d9776-124">**FilterTrackSelectStatement** och **IFilterTrackPropertyCondition**.</span><span class="sxs-lookup"><span data-stu-id="d9776-124">**FilterTrackSelectStatement** and **IFilterTrackPropertyCondition**.</span></span> <span data-ttu-id="d9776-125">Dessa typer baseras på följande REST API: er [FilterTrackSelect och FilterTrackPropertyCondition](https://docs.microsoft.com/rest/api/media/operations/filtertrackselect)</span><span class="sxs-lookup"><span data-stu-id="d9776-125">These types are based on the following REST APIs [FilterTrackSelect and FilterTrackPropertyCondition](https://docs.microsoft.com/rest/api/media/operations/filtertrackselect)</span></span>

## <a name="createupdatereaddelete-global-filters"></a><span data-ttu-id="d9776-126">Skapa/uppdatera och läsa/ta bort globala filter</span><span class="sxs-lookup"><span data-stu-id="d9776-126">Create/Update/Read/Delete global filters</span></span>
<span data-ttu-id="d9776-127">Följande kod visar hur du använder .NET för att skapa, uppdatera och läsa och ta bort tillgången filter.</span><span class="sxs-lookup"><span data-stu-id="d9776-127">The following code shows how to use .NET to create, update,read, and delete asset filters.</span></span>

    string filterName = "GlobalFilter_" + Guid.NewGuid().ToString();

    List<FilterTrackSelectStatement> filterTrackSelectStatements = new List<FilterTrackSelectStatement>();

    FilterTrackSelectStatement filterTrackSelectStatement = new FilterTrackSelectStatement();
    filterTrackSelectStatement.PropertyConditions = new List<IFilterTrackPropertyCondition>();
    filterTrackSelectStatement.PropertyConditions.Add(new FilterTrackNameCondition("Track Name", FilterTrackCompareOperator.NotEqual));
    filterTrackSelectStatement.PropertyConditions.Add(new FilterTrackBitrateRangeCondition(new FilterTrackBitrateRange(0, 1), FilterTrackCompareOperator.NotEqual));
    filterTrackSelectStatement.PropertyConditions.Add(new FilterTrackTypeCondition(FilterTrackType.Audio, FilterTrackCompareOperator.NotEqual));
    filterTrackSelectStatements.Add(filterTrackSelectStatement);

    // Create
    IStreamingFilter filter = _context.Filters.Create(filterName, new PresentationTimeRange(), filterTrackSelectStatements);

    // Update
    filter.PresentationTimeRange = new PresentationTimeRange(timescale: 500);
    filter.Update();

    // Read
    var filterUpdated = _context.Filters.FirstOrDefault();
    Console.WriteLine(filterUpdated.Name);

    // Delete
    filter.Delete();


## <a name="createupdatereaddelete-asset-filters"></a><span data-ttu-id="d9776-128">Skapa/uppdatera och läsa/ta bort tillgången filter</span><span class="sxs-lookup"><span data-stu-id="d9776-128">Create/Update/Read/Delete asset filters</span></span>
<span data-ttu-id="d9776-129">Följande kod visar hur du använder .NET för att skapa, uppdatera och läsa och ta bort tillgången filter.</span><span class="sxs-lookup"><span data-stu-id="d9776-129">The following code shows how to use .NET to create, update,read, and delete asset filters.</span></span>

    string assetName = "AssetFilter_" + Guid.NewGuid().ToString();
    var asset = _context.Assets.Create(assetName, AssetCreationOptions.None);

    string filterName = "AssetFilter_" + Guid.NewGuid().ToString();


    // Create
    IStreamingAssetFilter filter = asset.AssetFilters.Create(filterName,
                                        new PresentationTimeRange(), 
                                        new List<FilterTrackSelectStatement>());

    // Update
    filter.PresentationTimeRange = 
            new PresentationTimeRange(start: 6000000000, end: 72000000000);

    filter.Update();

    // Read
    asset = _context.Assets.Where(c => c.Id == asset.Id).FirstOrDefault();
    var filterUpdated = asset.AssetFilters.FirstOrDefault();
    Console.WriteLine(filterUpdated.Name);

    // Delete
    filterUpdated.Delete();




## <a name="build-streaming-urls-that-use-filters"></a><span data-ttu-id="d9776-130">Skapa strömning URL: er som använder filter</span><span class="sxs-lookup"><span data-stu-id="d9776-130">Build streaming URLs that use filters</span></span>
<span data-ttu-id="d9776-131">Mer information om hur man publicerar och leverera dina tillgångar finns [leverera innehåll till kunder översikt](media-services-deliver-content-overview.md).</span><span class="sxs-lookup"><span data-stu-id="d9776-131">For information on how to publish and deliver your assets, see [Delivering Content to Customers Overview](media-services-deliver-content-overview.md).</span></span>

<span data-ttu-id="d9776-132">Följande exempel visar hur du lägger till filter till din strömmande URL: er.</span><span class="sxs-lookup"><span data-stu-id="d9776-132">The following examples show how to add filters to your streaming URLs.</span></span>

<span data-ttu-id="d9776-133">**MPEG DASH**</span><span class="sxs-lookup"><span data-stu-id="d9776-133">**MPEG DASH**</span></span> 

    http://testendpoint-testaccount.streaming.mediaservices.windows.net/fecebb23-46f6-490d-8b70-203e86b0df58/BigBuckBunny.ism/Manifest(format=mpd-time-csf, filter=MyFilter)

<span data-ttu-id="d9776-134">**Apple HTTP Live Streaming (HLS) V4**</span><span class="sxs-lookup"><span data-stu-id="d9776-134">**Apple HTTP Live Streaming (HLS) V4**</span></span>

    http://testendpoint-testaccount.streaming.mediaservices.windows.net/fecebb23-46f6-490d-8b70-203e86b0df58/BigBuckBunny.ism/Manifest(format=m3u8-aapl, filter=MyFilter)

<span data-ttu-id="d9776-135">**Apple HTTP Live Streaming (HLS) V3**</span><span class="sxs-lookup"><span data-stu-id="d9776-135">**Apple HTTP Live Streaming (HLS) V3**</span></span>

    http://testendpoint-testaccount.streaming.mediaservices.windows.net/fecebb23-46f6-490d-8b70-203e86b0df58/BigBuckBunny.ism/Manifest(format=m3u8-aapl-v3, filter=MyFilter)

<span data-ttu-id="d9776-136">**Smooth Streaming**</span><span class="sxs-lookup"><span data-stu-id="d9776-136">**Smooth Streaming**</span></span>

    http://testendpoint-testaccount.streaming.mediaservices.windows.net/fecebb23-46f6-490d-8b70-203e86b0df58/BigBuckBunny.ism/Manifest(filter=MyFilter)


## <a name="media-services-learning-paths"></a><span data-ttu-id="d9776-137">Sökvägar för Media Services-utbildning</span><span class="sxs-lookup"><span data-stu-id="d9776-137">Media Services learning paths</span></span>
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a><span data-ttu-id="d9776-138">Ge feedback</span><span class="sxs-lookup"><span data-stu-id="d9776-138">Provide feedback</span></span>
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

## <a name="see-also"></a><span data-ttu-id="d9776-139">Se även</span><span class="sxs-lookup"><span data-stu-id="d9776-139">See Also</span></span>
[<span data-ttu-id="d9776-140">Översikt över dynamisk manifest</span><span class="sxs-lookup"><span data-stu-id="d9776-140">Dynamic manifests overview</span></span>](media-services-dynamic-manifest-overview.md)

