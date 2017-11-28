---
title: aaaCreating filter med Azure Media Services .NET SDK
description: "Det här avsnittet beskrivs hur toocreate filtrerar så att klienten kan använda dem toostream vissa delar av en dataström. Media Services skapar dynamiska manifesten tooachieve denna selektiv strömning."
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
ms.openlocfilehash: 16d9497d48ab1d3f841dd97efb0f66016a2435c5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="creating-filters-with-azure-media-services-net-sdk"></a><span data-ttu-id="b1d1f-104">Skapa filter med Azure Media Services .NET SDK</span><span class="sxs-lookup"><span data-stu-id="b1d1f-104">Creating Filters with Azure Media Services .NET SDK</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="b1d1f-105">.NET</span><span class="sxs-lookup"><span data-stu-id="b1d1f-105">.NET</span></span>](media-services-dotnet-dynamic-manifest.md)
> * [<span data-ttu-id="b1d1f-106">REST</span><span class="sxs-lookup"><span data-stu-id="b1d1f-106">REST</span></span>](media-services-rest-dynamic-manifest.md)
> 
> 

<span data-ttu-id="b1d1f-107">Från och med 2.11 Media Services kan du toodefine filter för dina tillgångar.</span><span class="sxs-lookup"><span data-stu-id="b1d1f-107">Starting with 2.11 release, Media Services enables you toodefine filters for your assets.</span></span> <span data-ttu-id="b1d1f-108">Dessa filter är server side regler som tillåter toochoose toodo sådant som dina kunder: uppspelning endast en del av en video (i stället för att spela upp hello hela video), eller ange bara en delmängd av ljud och video återgivningar att kundens enhet kan hantera () i stället för alla hello återgivningar som har associerats med hello tillgången).</span><span class="sxs-lookup"><span data-stu-id="b1d1f-108">These filters are server side rules that will allow your customers toochoose toodo things like: playback only a section of a video (instead of playing hello whole video), or specify only a subset of audio and video renditions that your customer's device can handle (instead of all hello renditions that are associated with hello asset).</span></span> <span data-ttu-id="b1d1f-109">Den här filtreringen dina tillgångar uppnås genom **dynamiska Manifest**som skapas vid kundens begäran toostream en video baserat på angivna filter.</span><span class="sxs-lookup"><span data-stu-id="b1d1f-109">This filtering of your assets is achieved through **Dynamic Manifest**s that are created upon your customer's request toostream a video based on specified filter(s).</span></span>

<span data-ttu-id="b1d1f-110">Mer detaljerad information relaterad toofilters och dynamiska Manifest finns [dynamiska visar en översikt över](media-services-dynamic-manifest-overview.md).</span><span class="sxs-lookup"><span data-stu-id="b1d1f-110">For more detailed information related toofilters and Dynamic Manifest, see [Dynamic manifests overview](media-services-dynamic-manifest-overview.md).</span></span>

<span data-ttu-id="b1d1f-111">Det här avsnittet visar hur toouse Media Services .NET SDK toocreate, uppdatera och ta bort filter.</span><span class="sxs-lookup"><span data-stu-id="b1d1f-111">This topic shows how toouse Media Services .NET SDK toocreate, update, and delete filters.</span></span> 

<span data-ttu-id="b1d1f-112">Observera att om du uppdaterar ett filter kan det ta upp too2 minuter för strömmande slutpunkt toorefresh hello regler.</span><span class="sxs-lookup"><span data-stu-id="b1d1f-112">Note if you update a filter, it can take up too2 minutes for streaming endpoint toorefresh hello rules.</span></span> <span data-ttu-id="b1d1f-113">Om hello innehåll behandlades med filtret (och cachelagras i proxyservrar och CDN cacheminnen), kan uppdatera det här filtret resultera i player-fel.</span><span class="sxs-lookup"><span data-stu-id="b1d1f-113">If hello content was served using this filter (and cached in proxies and CDN caches), updating this filter can result in player failures.</span></span> <span data-ttu-id="b1d1f-114">Rekommenderas tooclear hello cachen när du har uppdaterat hello filter.</span><span class="sxs-lookup"><span data-stu-id="b1d1f-114">It is recommend tooclear hello cache after updating hello filter.</span></span> <span data-ttu-id="b1d1f-115">Om det här alternativet inte är möjligt bör du använda ett annat filter.</span><span class="sxs-lookup"><span data-stu-id="b1d1f-115">If this option is not possible, consider using a different filter.</span></span> 

## <a name="types-used-toocreate-filters"></a><span data-ttu-id="b1d1f-116">Använda typer toocreate filter</span><span class="sxs-lookup"><span data-stu-id="b1d1f-116">Types used toocreate filters</span></span>
<span data-ttu-id="b1d1f-117">hello som följande typer används när du skapar filter:</span><span class="sxs-lookup"><span data-stu-id="b1d1f-117">hello following types are used when creating filters:</span></span> 

* <span data-ttu-id="b1d1f-118">**IStreamingFilter**.</span><span class="sxs-lookup"><span data-stu-id="b1d1f-118">**IStreamingFilter**.</span></span>  <span data-ttu-id="b1d1f-119">Den här typen är baserad på hello följande REST API [Filter](https://docs.microsoft.com/rest/api/media/operations/filter)</span><span class="sxs-lookup"><span data-stu-id="b1d1f-119">This type is based on hello following REST API [Filter](https://docs.microsoft.com/rest/api/media/operations/filter)</span></span>
* <span data-ttu-id="b1d1f-120">**IStreamingAssetFilter**.</span><span class="sxs-lookup"><span data-stu-id="b1d1f-120">**IStreamingAssetFilter**.</span></span> <span data-ttu-id="b1d1f-121">Den här typen är baserad på hello följande REST API [AssetFilter](https://docs.microsoft.com/rest/api/media/operations/assetfilter)</span><span class="sxs-lookup"><span data-stu-id="b1d1f-121">This type is based on hello following REST API [AssetFilter](https://docs.microsoft.com/rest/api/media/operations/assetfilter)</span></span>
* <span data-ttu-id="b1d1f-122">**PresentationTimeRange**.</span><span class="sxs-lookup"><span data-stu-id="b1d1f-122">**PresentationTimeRange**.</span></span> <span data-ttu-id="b1d1f-123">Den här typen är baserad på hello följande REST API [PresentationTimeRange](https://docs.microsoft.com/rest/api/media/operations/presentationtimerange)</span><span class="sxs-lookup"><span data-stu-id="b1d1f-123">This type is based on hello following REST API [PresentationTimeRange](https://docs.microsoft.com/rest/api/media/operations/presentationtimerange)</span></span>
* <span data-ttu-id="b1d1f-124">**FilterTrackSelectStatement** och **IFilterTrackPropertyCondition**.</span><span class="sxs-lookup"><span data-stu-id="b1d1f-124">**FilterTrackSelectStatement** and **IFilterTrackPropertyCondition**.</span></span> <span data-ttu-id="b1d1f-125">Dessa typer som är baserade på hello följande REST API: er [FilterTrackSelect och FilterTrackPropertyCondition](https://docs.microsoft.com/rest/api/media/operations/filtertrackselect)</span><span class="sxs-lookup"><span data-stu-id="b1d1f-125">These types are based on hello following REST APIs [FilterTrackSelect and FilterTrackPropertyCondition](https://docs.microsoft.com/rest/api/media/operations/filtertrackselect)</span></span>

## <a name="createupdatereaddelete-global-filters"></a><span data-ttu-id="b1d1f-126">Skapa/uppdatera och läsa/ta bort globala filter</span><span class="sxs-lookup"><span data-stu-id="b1d1f-126">Create/Update/Read/Delete global filters</span></span>
<span data-ttu-id="b1d1f-127">hello följande kod visar hur toouse .NET toocreate, uppdatera, läsa och ta bort tillgången filter.</span><span class="sxs-lookup"><span data-stu-id="b1d1f-127">hello following code shows how toouse .NET toocreate, update,read, and delete asset filters.</span></span>

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


## <a name="createupdatereaddelete-asset-filters"></a><span data-ttu-id="b1d1f-128">Skapa/uppdatera och läsa/ta bort tillgången filter</span><span class="sxs-lookup"><span data-stu-id="b1d1f-128">Create/Update/Read/Delete asset filters</span></span>
<span data-ttu-id="b1d1f-129">hello följande kod visar hur toouse .NET toocreate, uppdatera, läsa och ta bort tillgången filter.</span><span class="sxs-lookup"><span data-stu-id="b1d1f-129">hello following code shows how toouse .NET toocreate, update,read, and delete asset filters.</span></span>

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




## <a name="build-streaming-urls-that-use-filters"></a><span data-ttu-id="b1d1f-130">Skapa strömning URL: er som använder filter</span><span class="sxs-lookup"><span data-stu-id="b1d1f-130">Build streaming URLs that use filters</span></span>
<span data-ttu-id="b1d1f-131">Mer information om hur toopublish och leverera dina tillgångar, se [leverera innehåll tooCustomers översikt](media-services-deliver-content-overview.md).</span><span class="sxs-lookup"><span data-stu-id="b1d1f-131">For information on how toopublish and deliver your assets, see [Delivering Content tooCustomers Overview](media-services-deliver-content-overview.md).</span></span>

<span data-ttu-id="b1d1f-132">hello följande exempel visar hur tooadd filtrerar tooyour URL: er för strömning.</span><span class="sxs-lookup"><span data-stu-id="b1d1f-132">hello following examples show how tooadd filters tooyour streaming URLs.</span></span>

<span data-ttu-id="b1d1f-133">**MPEG DASH**</span><span class="sxs-lookup"><span data-stu-id="b1d1f-133">**MPEG DASH**</span></span> 

    http://testendpoint-testaccount.streaming.mediaservices.windows.net/fecebb23-46f6-490d-8b70-203e86b0df58/BigBuckBunny.ism/Manifest(format=mpd-time-csf, filter=MyFilter)

<span data-ttu-id="b1d1f-134">**Apple HTTP Live Streaming (HLS) V4**</span><span class="sxs-lookup"><span data-stu-id="b1d1f-134">**Apple HTTP Live Streaming (HLS) V4**</span></span>

    http://testendpoint-testaccount.streaming.mediaservices.windows.net/fecebb23-46f6-490d-8b70-203e86b0df58/BigBuckBunny.ism/Manifest(format=m3u8-aapl, filter=MyFilter)

<span data-ttu-id="b1d1f-135">**Apple HTTP Live Streaming (HLS) V3**</span><span class="sxs-lookup"><span data-stu-id="b1d1f-135">**Apple HTTP Live Streaming (HLS) V3**</span></span>

    http://testendpoint-testaccount.streaming.mediaservices.windows.net/fecebb23-46f6-490d-8b70-203e86b0df58/BigBuckBunny.ism/Manifest(format=m3u8-aapl-v3, filter=MyFilter)

<span data-ttu-id="b1d1f-136">**Smooth Streaming**</span><span class="sxs-lookup"><span data-stu-id="b1d1f-136">**Smooth Streaming**</span></span>

    http://testendpoint-testaccount.streaming.mediaservices.windows.net/fecebb23-46f6-490d-8b70-203e86b0df58/BigBuckBunny.ism/Manifest(filter=MyFilter)


## <a name="media-services-learning-paths"></a><span data-ttu-id="b1d1f-137">Sökvägar för Media Services-utbildning</span><span class="sxs-lookup"><span data-stu-id="b1d1f-137">Media Services learning paths</span></span>
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a><span data-ttu-id="b1d1f-138">Ge feedback</span><span class="sxs-lookup"><span data-stu-id="b1d1f-138">Provide feedback</span></span>
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

## <a name="see-also"></a><span data-ttu-id="b1d1f-139">Se även</span><span class="sxs-lookup"><span data-stu-id="b1d1f-139">See Also</span></span>
[<span data-ttu-id="b1d1f-140">Översikt över dynamisk manifest</span><span class="sxs-lookup"><span data-stu-id="b1d1f-140">Dynamic manifests overview</span></span>](media-services-dynamic-manifest-overview.md)

