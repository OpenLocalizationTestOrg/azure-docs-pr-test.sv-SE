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
# <a name="publish-azure-media-services-content-using-net"></a>Publicera Azure Media Services-innehåll med hjälp av .NET
> [!div class="op_single_selector"]
> * [REST](media-services-rest-deliver-streaming-content.md)
> * [.NET](media-services-deliver-streaming-content.md)
> * [Portal](media-services-portal-publish.md)
> 
> 

## <a name="overview"></a>Översikt
Du kan strömma en anpassningsbar bithastighet MP4-uppsättningen genom att skapa en positionerare för strömning och skapa en strömnings-URL. Hej [koda en tillgång](media-services-encode-asset.md) avsnittet visas hur tooencode i en MP4 med anpassningsbar bithastighet. 

> [!NOTE]
> Om ditt innehåll är krypterad, konfigurera principen för tillgångsleverans (enligt beskrivningen i [detta](media-services-dotnet-configure-asset-delivery-policy.md) avsnittet) innan du skapar en positionerare. 
> 
> 

Du kan också använda en OnDemand-strömning lokaliserare toobuild-URL: er som punkt tooMP4-filer som kan hämtas progressivt.  

Det här avsnittet visar hur toocreate en OnDemand-strömning lokaliserare toopublish din tillgång och skapa en mjuk MPEG DASH och HLS-strömnings-URL: er. Den visar även varm toobuild URL: er för progressiv nedladdning. 

## <a name="create-an-ondemand-streaming-locator"></a>Skapa en OnDemand-strömning lokaliserare
toocreate hello OnDemand-strömning positionerare och hämta URL: er, måste du toodo hello följande saker:

1. Definiera en åtkomstprincip om hello innehåll är krypterad.
2. Skapa en OnDemand-strömning lokaliserare.
3. Om du planerar toostream hämta hello strömning manifestfilen (.ism) i hello tillgången. 
   
   Om du planerar tooprogressively download hämta hello namnen på MP4-filer i hello tillgången.  
4. Skapa URL: er toohello manifestfilen eller MP4-filer. 


>[!NOTE]
>Det finns en gräns på 1 000 000 principer för olika AMS-principer (till exempel för positionerarprincipen eller ContentKeyAuthorizationPolicy). Använd hello samma princip-ID om du alltid använder hello samma dagar / åtkomstbehörigheter. Principer för lokaliserare som är avsedd exempelvis tooremain på plats för lång tid (icke-överföringen principer). Mer information finns i [detta](media-services-dotnet-manage-entities.md#limit-access-policies) avsnitt.

### <a name="use-media-services-net-sdk"></a>Använda Media Services .NET SDK
Skapa strömmande URL: er 

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

hello utdata:

    URL toomanifest for client streaming using Smooth Streaming protocol:
    http://amstest1.streaming.mediaservices.windows.net/3c5fe676-199c-4620-9b03-ba014900f214/BigBuckBunny.ism/manifest
    URL toomanifest for client streaming using HLS protocol:
    http://amstest1.streaming.mediaservices.windows.net/3c5fe676-199c-4620-9b03-ba014900f214/BigBuckBunny.ism/manifest(format=m3u8-aapl)
    URL toomanifest for client streaming using MPEG DASH protocol:
    http://amstest1.streaming.mediaservices.windows.net/3c5fe676-199c-4620-9b03-ba014900f214/BigBuckBunny.ism/manifest(format=mpd-time-csf)


> [!NOTE]
> Du kan också strömma ditt innehåll via en SSL-anslutning. toodo hanterar detta, kontrollera din strömmande URL: er starta med HTTPS. För närvarande stöder AMS inte SSL med anpassade domäner.
> 
> 

Skapa URL: er för progressiv hämtning 

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

hello utdata:

    http://amstest1.streaming.mediaservices.windows.net/3c5fe676-199c-4620-9b03-ba014900f214/BigBuckBunny_H264_650kbps_AAC_und_ch2_96kbps.mp4
    http://amstest1.streaming.mediaservices.windows.net/3c5fe676-199c-4620-9b03-ba014900f214/BigBuckBunny_H264_400kbps_AAC_und_ch2_96kbps.mp4
    http://amstest1.streaming.mediaservices.windows.net/3c5fe676-199c-4620-9b03-ba014900f214/BigBuckBunny_H264_3400kbps_AAC_und_ch2_96kbps.mp4
    http://amstest1.streaming.mediaservices.windows.net/3c5fe676-199c-4620-9b03-ba014900f214/BigBuckBunny_H264_2250kbps_AAC_und_ch2_96kbps.mp4

    . . . 

### <a name="use-media-services-net-sdk-extensions"></a>Använd Media Services .NET SDK-tillägg
hello anropar följande kod .NET SDK-tillägg-metoder som skapar en positionerare och generera hello Smooth Streaming, HLS och MPEG-DASH-URL: er för anpassningsbar strömning.

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


## <a name="media-services-learning-paths"></a>Sökvägar för Media Services-utbildning
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Ge feedback
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

## <a name="next-steps"></a>Nästa steg
* [Hämta tillgångar](media-services-deliver-asset-download.md)
* [Konfigurera tillgångsleveransprincip](media-services-dotnet-configure-asset-delivery-policy.md)

