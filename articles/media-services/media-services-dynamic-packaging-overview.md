---
title: "aaaAzure Media Services dynamisk paketering översikt | Microsoft Docs"
description: "Översikt över dynamisk paketering och hello avsnittet ger."
author: Juliako
manager: cfowler
editor: 
services: media-services
documentationcenter: 
ms.assetid: 0d9e4f54-5daa-45c1-bfaa-cf09ca89b812
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/29/2017
ms.author: juliako
ms.openlocfilehash: 970e24eba800e098774172c87f56629430b227a9
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="dynamic-packaging"></a><span data-ttu-id="d7703-103">Dynamisk paketering</span><span class="sxs-lookup"><span data-stu-id="d7703-103">Dynamic packaging</span></span>
## <a name="overview"></a><span data-ttu-id="d7703-104">Översikt</span><span class="sxs-lookup"><span data-stu-id="d7703-104">Overview</span></span>
<span data-ttu-id="d7703-105">Microsoft Azure Media Services kan vara används toodeliver många media käll-filformat, strömmande medieformat och innehållsskydd format tooa mängd olika tekniker för klient (till exempel iOS, XBOX, Silverlight, Windows 8).</span><span class="sxs-lookup"><span data-stu-id="d7703-105">Microsoft Azure Media Services can be used toodeliver many media source file formats, media streaming formats, and content protection formats tooa variety of client technologies (for example, iOS, XBOX, Silverlight, Windows 8).</span></span> <span data-ttu-id="d7703-106">Dessa klienter förstå olika protokoll, till exempel iOS kräver en HTTP Live Streaming (HLS) V4-format och Silverlight och Xbox kräver Smooth Streaming.</span><span class="sxs-lookup"><span data-stu-id="d7703-106">These clients understand different protocols, for example iOS requires an HTTP Live Streaming (HLS) V4 format and Silverlight and Xbox require Smooth Streaming.</span></span> <span data-ttu-id="d7703-107">Om du har en uppsättning med anpassningsbar bithastighet (multibithastighet) MP4 (ISO Base 14496 12) mediefiler eller en uppsättning med anpassningsbar bithastighet Smooth Streaming-filer som du vill tooserve tooclients som förstå MPEG DASH, HLS eller Smooth Streaming, bör du dra nytta av Media Services dynamisk paketering.</span><span class="sxs-lookup"><span data-stu-id="d7703-107">If you have a set of adaptive bitrate (multi-bitrate) MP4 (ISO Base Media 14496-12) files or a set of adaptive bitrate Smooth Streaming files that you want tooserve tooclients that understand MPEG DASH, HLS or Smooth Streaming, you should take advantage of Media Services dynamic packaging.</span></span>

<span data-ttu-id="d7703-108">Med dynamisk paketering behöver du är toocreate en tillgång som innehåller en uppsättning med anpassningsbar bithastighet MP4-filer eller Smooth Streaming-filer.</span><span class="sxs-lookup"><span data-stu-id="d7703-108">With dynamic packaging all you need is toocreate an asset that contains a set of adaptive bitrate MP4 files or adaptive bitrate Smooth Streaming files.</span></span> <span data-ttu-id="d7703-109">Sedan, baserat på hello format som anges i manifestet hello eller Fragmentera begäran, hello strömning på begäran kommer servern att säkerställa att du har fått hello dataström i hello-protokollet som du har valt.</span><span class="sxs-lookup"><span data-stu-id="d7703-109">Then, based on hello specified format in hello manifest or fragment request, hello On-Demand Streaming server will ensure that you receive hello stream in hello protocol you have chosen.</span></span> <span data-ttu-id="d7703-110">Därför behöver du bara toostore och betala för hello filer i ett enda lagringsformat och Media Services-tjänsten skapar och ger hello lämplig respons baserat på begäranden från en klient.</span><span class="sxs-lookup"><span data-stu-id="d7703-110">As a result, you only need toostore and pay for hello files in single storage format and Media Services service will build and serve hello appropriate response based on requests from a client.</span></span>

<span data-ttu-id="d7703-111">hello följande diagram visar hello traditionella kodning och statiska paketering arbetsflöde.</span><span class="sxs-lookup"><span data-stu-id="d7703-111">hello following diagram shows hello traditional encoding and static packaging workflow.</span></span>

![Statisk kodning](./media/media-services-dynamic-packaging-overview/media-services-static-packaging.png)

<span data-ttu-id="d7703-113">hello visar följande diagram hello dynamisk paketering arbetsflöde.</span><span class="sxs-lookup"><span data-stu-id="d7703-113">hello following diagram shows hello dynamic packaging workflow.</span></span>

![Dynamisk kodning](./media/media-services-dynamic-packaging-overview/media-services-dynamic-packaging.png)


## <a name="common-scenario"></a><span data-ttu-id="d7703-115">Vanligt scenario</span><span class="sxs-lookup"><span data-stu-id="d7703-115">Common scenario</span></span>
1. <span data-ttu-id="d7703-116">Överför en indatafil (kallas en mezzaninfil).</span><span class="sxs-lookup"><span data-stu-id="d7703-116">Upload an input file (called a mezzanine file).</span></span> <span data-ttu-id="d7703-117">Till exempel H.264 MP4 eller WMV (hello lista över de format som stöds finns [format som stöds av Media Encoder Standard hello](media-services-media-encoder-standard-formats.md).</span><span class="sxs-lookup"><span data-stu-id="d7703-117">For example, H.264, MP4, or WMV (for hello list of supported formats see [Formats Supported by hello Media Encoder Standard](media-services-media-encoder-standard-formats.md).</span></span>
2. <span data-ttu-id="d7703-118">Koda uppsättningarna mezzanine filen tooH.264 MP4 med anpassningsbar bithastighet.</span><span class="sxs-lookup"><span data-stu-id="d7703-118">Encode your mezzanine file tooH.264 MP4 adaptive bitrate sets.</span></span>
3. <span data-ttu-id="d7703-119">Publicera hello tillgång som innehåller hello MP4-uppsättningen genom att skapa hello positionerare på begäran med anpassningsbar bithastighet.</span><span class="sxs-lookup"><span data-stu-id="d7703-119">Publish hello asset that contains hello adaptive bitrate MP4 set by creating hello On-Demand Locator.</span></span>
4. <span data-ttu-id="d7703-120">Skapa hello tooaccess URL: er för strömning och strömma ditt innehåll.</span><span class="sxs-lookup"><span data-stu-id="d7703-120">Build hello streaming URLs tooaccess and stream your content.</span></span>

## <a name="preparing-assets-for-dynamic-streaming"></a><span data-ttu-id="d7703-121">Förbereda tillgångar för dynamiska strömning</span><span class="sxs-lookup"><span data-stu-id="d7703-121">Preparing assets for dynamic streaming</span></span>
<span data-ttu-id="d7703-122">tooprepare din tillgång för dynamiska direktuppspelning av du har två alternativ:</span><span class="sxs-lookup"><span data-stu-id="d7703-122">tooprepare your asset for dynamic streaming you have two options:</span></span>

1. <span data-ttu-id="d7703-123">[Överföra en fil med master](media-services-dotnet-upload-files.md).</span><span class="sxs-lookup"><span data-stu-id="d7703-123">[Upload a master file](media-services-dotnet-upload-files.md).</span></span>
2. <span data-ttu-id="d7703-124">[Använd hello Media Encoder Standard-kodare tooproduce H.264 MP4 med anpassad bithastighet](media-services-dotnet-encode-with-media-encoder-standard.md).</span><span class="sxs-lookup"><span data-stu-id="d7703-124">[Use hello Media Encoder Standard encoder tooproduce H.264 MP4 adaptive bitrate sets](media-services-dotnet-encode-with-media-encoder-standard.md).</span></span>
3. <span data-ttu-id="d7703-125">[Strömma ditt innehåll](media-services-deliver-content-overview.md).</span><span class="sxs-lookup"><span data-stu-id="d7703-125">[Stream your content](media-services-deliver-content-overview.md).</span></span>

## <span data-ttu-id="d7703-126"><a id="unsupported_formats"></a>Format som inte stöds av dynamisk paketering</span><span class="sxs-lookup"><span data-stu-id="d7703-126"><a id="unsupported_formats"></a>Formats that are not supported by dynamic packaging</span></span>
<span data-ttu-id="d7703-127">hello stöds följande källa format inte av dynamisk paketering.</span><span class="sxs-lookup"><span data-stu-id="d7703-127">hello following source file formats are not supported by dynamic packaging.</span></span>

* <span data-ttu-id="d7703-128">Dolby digital mp4-filer.</span><span class="sxs-lookup"><span data-stu-id="d7703-128">Dolby digital mp4 files.</span></span>
* <span data-ttu-id="d7703-129">Dolby digitala smooth filer.</span><span class="sxs-lookup"><span data-stu-id="d7703-129">Dolby digital smooth files.</span></span>

## <a name="media-services-learning-paths"></a><span data-ttu-id="d7703-130">Sökvägar för Media Services-utbildning</span><span class="sxs-lookup"><span data-stu-id="d7703-130">Media Services learning paths</span></span>
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a><span data-ttu-id="d7703-131">Ge feedback</span><span class="sxs-lookup"><span data-stu-id="d7703-131">Provide feedback</span></span>
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

