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
# <a name="creating-filters-with-azure-media-services-net-sdk"></a>Skapa filter med Azure Media Services .NET SDK
> [!div class="op_single_selector"]
> * [.NET](media-services-dotnet-dynamic-manifest.md)
> * [REST](media-services-rest-dynamic-manifest.md)
> 
> 

Från och med 2.11 Media Services kan du toodefine filter för dina tillgångar. Dessa filter är server side regler som tillåter toochoose toodo sådant som dina kunder: uppspelning endast en del av en video (i stället för att spela upp hello hela video), eller ange bara en delmängd av ljud och video återgivningar att kundens enhet kan hantera () i stället för alla hello återgivningar som har associerats med hello tillgången). Den här filtreringen dina tillgångar uppnås genom **dynamiska Manifest**som skapas vid kundens begäran toostream en video baserat på angivna filter.

Mer detaljerad information relaterad toofilters och dynamiska Manifest finns [dynamiska visar en översikt över](media-services-dynamic-manifest-overview.md).

Det här avsnittet visar hur toouse Media Services .NET SDK toocreate, uppdatera och ta bort filter. 

Observera att om du uppdaterar ett filter kan det ta upp too2 minuter för strömmande slutpunkt toorefresh hello regler. Om hello innehåll behandlades med filtret (och cachelagras i proxyservrar och CDN cacheminnen), kan uppdatera det här filtret resultera i player-fel. Rekommenderas tooclear hello cachen när du har uppdaterat hello filter. Om det här alternativet inte är möjligt bör du använda ett annat filter. 

## <a name="types-used-toocreate-filters"></a>Använda typer toocreate filter
hello som följande typer används när du skapar filter: 

* **IStreamingFilter**.  Den här typen är baserad på hello följande REST API [Filter](https://docs.microsoft.com/rest/api/media/operations/filter)
* **IStreamingAssetFilter**. Den här typen är baserad på hello följande REST API [AssetFilter](https://docs.microsoft.com/rest/api/media/operations/assetfilter)
* **PresentationTimeRange**. Den här typen är baserad på hello följande REST API [PresentationTimeRange](https://docs.microsoft.com/rest/api/media/operations/presentationtimerange)
* **FilterTrackSelectStatement** och **IFilterTrackPropertyCondition**. Dessa typer som är baserade på hello följande REST API: er [FilterTrackSelect och FilterTrackPropertyCondition](https://docs.microsoft.com/rest/api/media/operations/filtertrackselect)

## <a name="createupdatereaddelete-global-filters"></a>Skapa/uppdatera och läsa/ta bort globala filter
hello följande kod visar hur toouse .NET toocreate, uppdatera, läsa och ta bort tillgången filter.

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


## <a name="createupdatereaddelete-asset-filters"></a>Skapa/uppdatera och läsa/ta bort tillgången filter
hello följande kod visar hur toouse .NET toocreate, uppdatera, läsa och ta bort tillgången filter.

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




## <a name="build-streaming-urls-that-use-filters"></a>Skapa strömning URL: er som använder filter
Mer information om hur toopublish och leverera dina tillgångar, se [leverera innehåll tooCustomers översikt](media-services-deliver-content-overview.md).

hello följande exempel visar hur tooadd filtrerar tooyour URL: er för strömning.

**MPEG DASH** 

    http://testendpoint-testaccount.streaming.mediaservices.windows.net/fecebb23-46f6-490d-8b70-203e86b0df58/BigBuckBunny.ism/Manifest(format=mpd-time-csf, filter=MyFilter)

**Apple HTTP Live Streaming (HLS) V4**

    http://testendpoint-testaccount.streaming.mediaservices.windows.net/fecebb23-46f6-490d-8b70-203e86b0df58/BigBuckBunny.ism/Manifest(format=m3u8-aapl, filter=MyFilter)

**Apple HTTP Live Streaming (HLS) V3**

    http://testendpoint-testaccount.streaming.mediaservices.windows.net/fecebb23-46f6-490d-8b70-203e86b0df58/BigBuckBunny.ism/Manifest(format=m3u8-aapl-v3, filter=MyFilter)

**Smooth Streaming**

    http://testendpoint-testaccount.streaming.mediaservices.windows.net/fecebb23-46f6-490d-8b70-203e86b0df58/BigBuckBunny.ism/Manifest(filter=MyFilter)


## <a name="media-services-learning-paths"></a>Sökvägar för Media Services-utbildning
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Ge feedback
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

## <a name="see-also"></a>Se även
[Översikt över dynamisk manifest](media-services-dynamic-manifest-overview.md)

