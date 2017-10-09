---
title: "aaaPublish Azure Media Services innehåll med hjälp av .NET | Microsoft Docs"
description: "Lär dig hur toocreate en positionerare som används toobuild en strömnings-URL. Kodexemplen är skrivna i C# och använder hello Media Services SDK för .NET."
author: juliako
manager: cfowler
editor: 
services: media-services
documentationcenter: 
ms.assetid: c53b1f83-4cb1-4b09-840f-9c145b7d6f8d
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/09/2017
ms.author: juliako
ms.openlocfilehash: c941cd93c252a96e66546cce2793bb426afac059
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="publish-azure-media-services-content-using-net"></a><span data-ttu-id="115da-104">Publicera Azure Media Services-innehåll med hjälp av .NET</span><span class="sxs-lookup"><span data-stu-id="115da-104">Publish Azure Media Services content using .NET</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="115da-105">REST</span><span class="sxs-lookup"><span data-stu-id="115da-105">REST</span></span>](media-services-rest-deliver-streaming-content.md)
> * [<span data-ttu-id="115da-106">.NET</span><span class="sxs-lookup"><span data-stu-id="115da-106">.NET</span></span>](media-services-deliver-streaming-content.md)
> * [<span data-ttu-id="115da-107">Portal</span><span class="sxs-lookup"><span data-stu-id="115da-107">Portal</span></span>](media-services-portal-publish.md)
> 
> 

## <a name="overview"></a><span data-ttu-id="115da-108">Översikt</span><span class="sxs-lookup"><span data-stu-id="115da-108">Overview</span></span>
<span data-ttu-id="115da-109">Du kan strömma en anpassningsbar bithastighet MP4-uppsättningen genom att skapa en positionerare för strömning och skapa en strömnings-URL.</span><span class="sxs-lookup"><span data-stu-id="115da-109">You can stream an adaptive bitrate MP4 set by creating an OnDemand streaming locator and building a streaming URL.</span></span> <span data-ttu-id="115da-110">Hej [koda en tillgång](media-services-encode-asset.md) avsnittet visas hur tooencode i en MP4 med anpassningsbar bithastighet.</span><span class="sxs-lookup"><span data-stu-id="115da-110">hello [encoding an asset](media-services-encode-asset.md) topic shows how tooencode into an adaptive bitrate MP4 set.</span></span> 

> [!NOTE]
> <span data-ttu-id="115da-111">Om ditt innehåll är krypterad, konfigurera principen för tillgångsleverans (enligt beskrivningen i [detta](media-services-dotnet-configure-asset-delivery-policy.md) avsnittet) innan du skapar en positionerare.</span><span class="sxs-lookup"><span data-stu-id="115da-111">If your content is encrypted, configure asset delivery policy (as described in [this](media-services-dotnet-configure-asset-delivery-policy.md) topic) before creating a locator.</span></span> 
> 
> 

<span data-ttu-id="115da-112">Du kan också använda en OnDemand-strömning lokaliserare toobuild-URL: er som punkt tooMP4-filer som kan hämtas progressivt.</span><span class="sxs-lookup"><span data-stu-id="115da-112">You can also use an OnDemand streaming locator toobuild URLs that point tooMP4 files that can be progressively downloaded.</span></span>  

<span data-ttu-id="115da-113">Det här avsnittet visar hur toocreate en OnDemand-strömning lokaliserare toopublish din tillgång och skapa en mjuk MPEG DASH och HLS-strömnings-URL: er.</span><span class="sxs-lookup"><span data-stu-id="115da-113">This topic shows how toocreate an OnDemand streaming locator toopublish your asset and build a Smooth, MPEG DASH, and HLS streaming URLs.</span></span> <span data-ttu-id="115da-114">Den visar även varm toobuild URL: er för progressiv nedladdning.</span><span class="sxs-lookup"><span data-stu-id="115da-114">It also shows hot toobuild progressive download URLs.</span></span> 

## <a name="create-an-ondemand-streaming-locator"></a><span data-ttu-id="115da-115">Skapa en OnDemand-strömning lokaliserare</span><span class="sxs-lookup"><span data-stu-id="115da-115">Create an OnDemand streaming locator</span></span>
<span data-ttu-id="115da-116">toocreate hello OnDemand-strömning positionerare och hämta URL: er, måste du toodo hello följande saker:</span><span class="sxs-lookup"><span data-stu-id="115da-116">toocreate hello OnDemand streaming locator and get URLs, you need toodo hello following things:</span></span>

1. <span data-ttu-id="115da-117">Definiera en åtkomstprincip om hello innehåll är krypterad.</span><span class="sxs-lookup"><span data-stu-id="115da-117">If hello content is encrypted, define an access policy.</span></span>
2. <span data-ttu-id="115da-118">Skapa en OnDemand-strömning lokaliserare.</span><span class="sxs-lookup"><span data-stu-id="115da-118">Create an OnDemand streaming locator.</span></span>
3. <span data-ttu-id="115da-119">Om du planerar toostream hämta hello strömning manifestfilen (.ism) i hello tillgången.</span><span class="sxs-lookup"><span data-stu-id="115da-119">If you plan toostream, get hello streaming manifest file (.ism) in hello asset.</span></span> 
   
   <span data-ttu-id="115da-120">Om du planerar tooprogressively download hämta hello namnen på MP4-filer i hello tillgången.</span><span class="sxs-lookup"><span data-stu-id="115da-120">If you plan tooprogressively download, get hello names of MP4 files in hello asset.</span></span>  
4. <span data-ttu-id="115da-121">Skapa URL: er toohello manifestfilen eller MP4-filer.</span><span class="sxs-lookup"><span data-stu-id="115da-121">Build URLs toohello manifest file or MP4 files.</span></span> 


>[!NOTE]
><span data-ttu-id="115da-122">Det finns en gräns på 1 000 000 principer för olika AMS-principer (till exempel för positionerarprincipen eller ContentKeyAuthorizationPolicy).</span><span class="sxs-lookup"><span data-stu-id="115da-122">There is a limit of 1,000,000 policies for different AMS policies (for example, for Locator policy or ContentKeyAuthorizationPolicy).</span></span> <span data-ttu-id="115da-123">Använd hello samma princip-ID om du alltid använder hello samma dagar / åtkomstbehörigheter.</span><span class="sxs-lookup"><span data-stu-id="115da-123">Use hello same policy ID if you are always using hello same days / access permissions.</span></span> <span data-ttu-id="115da-124">Principer för lokaliserare som är avsedd exempelvis tooremain på plats för lång tid (icke-överföringen principer).</span><span class="sxs-lookup"><span data-stu-id="115da-124">For example, policies for locators that are intended tooremain in place for a long time (non-upload policies).</span></span> <span data-ttu-id="115da-125">Mer information finns i [detta](media-services-dotnet-manage-entities.md#limit-access-policies) avsnitt.</span><span class="sxs-lookup"><span data-stu-id="115da-125">For more information, see [this](media-services-dotnet-manage-entities.md#limit-access-policies) topic.</span></span>

### <a name="use-media-services-net-sdk"></a><span data-ttu-id="115da-126">Använda Media Services .NET SDK</span><span class="sxs-lookup"><span data-stu-id="115da-126">Use Media Services .NET SDK</span></span>
<span data-ttu-id="115da-127">Skapa strömmande URL: er</span><span class="sxs-lookup"><span data-stu-id="115da-127">Build Streaming URLs</span></span> 

    private static void BuildStreamingURLs(IAsset asset)
    {

        // Create a 30-day readonly access policy. 
          // You cannot create a streaming locator using an AccessPolicy that includes write or delete permissions.
        IAccessPolicy policy = _context.AccessPolicies.Create("Streaming policy",
            TimeSpan.FromDays(30),
            AccessPermissions.Read);

        // Create a locator toohello streaming content on an origin. 
        ILocator originLocator = _context.Locators.CreateLocator(LocatorType.OnDemandOrigin, asset,
            policy,
            DateTime.UtcNow.AddMinutes(-5));

        // Display some useful values based on hello locator.
        Console.WriteLine("Streaming asset base path on origin: ");
        Console.WriteLine(originLocator.Path);
        Console.WriteLine();

        // Get a reference toohello streaming manifest file from hello  
        // collection of files in hello asset. 
        var manifestFile = asset.AssetFiles.Where(f => f.Name.ToLower().
                                    EndsWith(".ism")).
                                    FirstOrDefault();

        // Create a full URL toohello manifest file. Use this for playback
        // in streaming media clients. 
        string urlForClientStreaming = originLocator.Path + manifestFile.Name + "/manifest";
        Console.WriteLine("URL toomanifest for client streaming using Smooth Streaming protocol: ");
        Console.WriteLine(urlForClientStreaming);
        Console.WriteLine("URL toomanifest for client streaming using HLS protocol: ");
        Console.WriteLine(urlForClientStreaming + "(format=m3u8-aapl)");
        Console.WriteLine("URL toomanifest for client streaming using MPEG DASH protocol: ");
        Console.WriteLine(urlForClientStreaming + "(format=mpd-time-csf)"); 
        Console.WriteLine();
    }

<span data-ttu-id="115da-128">hello utdata:</span><span class="sxs-lookup"><span data-stu-id="115da-128">hello outputs:</span></span>

    URL toomanifest for client streaming using Smooth Streaming protocol:
    http://amstest1.streaming.mediaservices.windows.net/3c5fe676-199c-4620-9b03-ba014900f214/BigBuckBunny.ism/manifest
    URL toomanifest for client streaming using HLS protocol:
    http://amstest1.streaming.mediaservices.windows.net/3c5fe676-199c-4620-9b03-ba014900f214/BigBuckBunny.ism/manifest(format=m3u8-aapl)
    URL toomanifest for client streaming using MPEG DASH protocol:
    http://amstest1.streaming.mediaservices.windows.net/3c5fe676-199c-4620-9b03-ba014900f214/BigBuckBunny.ism/manifest(format=mpd-time-csf)


> [!NOTE]
> <span data-ttu-id="115da-129">Du kan också strömma ditt innehåll via en SSL-anslutning.</span><span class="sxs-lookup"><span data-stu-id="115da-129">You can also stream your content over an SSL connection.</span></span> <span data-ttu-id="115da-130">toodo hanterar detta, kontrollera din strömmande URL: er starta med HTTPS.</span><span class="sxs-lookup"><span data-stu-id="115da-130">toodo this approach, make sure your streaming URLs start with HTTPS.</span></span> <span data-ttu-id="115da-131">För närvarande stöder AMS inte SSL med anpassade domäner.</span><span class="sxs-lookup"><span data-stu-id="115da-131">Currently, AMS doesn’t support SSL with custom domains.</span></span>
> 
> 

<span data-ttu-id="115da-132">Skapa URL: er för progressiv hämtning</span><span class="sxs-lookup"><span data-stu-id="115da-132">Build progressive download URLs</span></span> 

    private static void BuildProgressiveDownloadURLs(IAsset asset)
    {
        // Create a 30-day readonly access policy. 
        IAccessPolicy policy = _context.AccessPolicies.Create("Streaming policy",
            TimeSpan.FromDays(30),
            AccessPermissions.Read);

        // Create an OnDemandOrigin locator toohello asset. 
        ILocator originLocator = _context.Locators.CreateLocator(LocatorType.OnDemandOrigin, asset,
            policy,
            DateTime.UtcNow.AddMinutes(-5));

        // Display some useful values based on hello locator.
        Console.WriteLine("Streaming asset base path on origin: ");
        Console.WriteLine(originLocator.Path);
        Console.WriteLine();

        // Get MP4 files.
        IEnumerable<IAssetFile> mp4AssetFiles = asset
            .AssetFiles
            .ToList()
            .Where(af => af.Name.EndsWith(".mp4", StringComparison.OrdinalIgnoreCase));

        // Create a full URL toohello MP4 files. Use this tooprogressively download files.
        foreach (var pd in mp4AssetFiles)
            Console.WriteLine(originLocator.Path + pd.Name);
    }

<span data-ttu-id="115da-133">hello utdata:</span><span class="sxs-lookup"><span data-stu-id="115da-133">hello outputs:</span></span>

    http://amstest1.streaming.mediaservices.windows.net/3c5fe676-199c-4620-9b03-ba014900f214/BigBuckBunny_H264_650kbps_AAC_und_ch2_96kbps.mp4
    http://amstest1.streaming.mediaservices.windows.net/3c5fe676-199c-4620-9b03-ba014900f214/BigBuckBunny_H264_400kbps_AAC_und_ch2_96kbps.mp4
    http://amstest1.streaming.mediaservices.windows.net/3c5fe676-199c-4620-9b03-ba014900f214/BigBuckBunny_H264_3400kbps_AAC_und_ch2_96kbps.mp4
    http://amstest1.streaming.mediaservices.windows.net/3c5fe676-199c-4620-9b03-ba014900f214/BigBuckBunny_H264_2250kbps_AAC_und_ch2_96kbps.mp4

    . . . 

### <a name="use-media-services-net-sdk-extensions"></a><span data-ttu-id="115da-134">Använd Media Services .NET SDK-tillägg</span><span class="sxs-lookup"><span data-stu-id="115da-134">Use Media Services .NET SDK Extensions</span></span>
<span data-ttu-id="115da-135">hello anropar följande kod .NET SDK-tillägg-metoder som skapar en positionerare och generera hello Smooth Streaming, HLS och MPEG-DASH-URL: er för anpassningsbar strömning.</span><span class="sxs-lookup"><span data-stu-id="115da-135">hello following code calls .NET SDK extensions methods that create a locator and generate hello Smooth Streaming, HLS, and MPEG-DASH URLs for adaptive streaming.</span></span>

    // Create a loctor.
    _context.Locators.Create(
        LocatorType.OnDemandOrigin,
        inputAsset,
        AccessPermissions.Read,
        TimeSpan.FromDays(30));

    // Get hello streaming URLs.
    Uri smoothStreamingUri = inputAsset.GetSmoothStreamingUri();
    Uri hlsUri = inputAsset.GetHlsUri();
    Uri mpegDashUri = inputAsset.GetMpegDashUri();

    Console.WriteLine(smoothStreamingUri);
    Console.WriteLine(hlsUri);
    Console.WriteLine(mpegDashUri);


## <a name="media-services-learning-paths"></a><span data-ttu-id="115da-136">Sökvägar för Media Services-utbildning</span><span class="sxs-lookup"><span data-stu-id="115da-136">Media Services learning paths</span></span>
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a><span data-ttu-id="115da-137">Ge feedback</span><span class="sxs-lookup"><span data-stu-id="115da-137">Provide feedback</span></span>
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

## <a name="next-steps"></a><span data-ttu-id="115da-138">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="115da-138">Next steps</span></span>
* [<span data-ttu-id="115da-139">Hämta tillgångar</span><span class="sxs-lookup"><span data-stu-id="115da-139">Download assets</span></span>](media-services-deliver-asset-download.md)
* [<span data-ttu-id="115da-140">Konfigurera tillgångsleveransprincip</span><span class="sxs-lookup"><span data-stu-id="115da-140">Configure asset delivery policy</span></span>](media-services-dotnet-configure-asset-delivery-policy.md)

