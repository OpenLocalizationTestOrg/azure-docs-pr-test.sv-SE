---
title: "Översikt över och jämförelse av Azure på begäran mediet kodare | Microsoft Docs"
description: "Det här avsnittet ger en översikt och jämförelse av Azure på begäran mediet kodare."
services: media-services
documentationcenter: 
author: juliako
manager: cfowler
editor: 
ms.assetid: e6bfc068-fa46-4d68-b1ce-9092c8f3a3c9
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/10/2017
ms.author: juliako
ms.openlocfilehash: 538a6ab60168735c2626a93cdeedd8d4999a6efc
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/29/2017
---
# <a name="overview-and-comparison-of-azure-on-demand-media-encoders"></a><span data-ttu-id="7c9c3-103">Översikt över och jämförelse av Azure på begäran mediet kodare</span><span class="sxs-lookup"><span data-stu-id="7c9c3-103">Overview and comparison of Azure on demand media encoders</span></span>
## <a name="encoding-overview"></a><span data-ttu-id="7c9c3-104">Kodning: översikt</span><span class="sxs-lookup"><span data-stu-id="7c9c3-104">Encoding overview</span></span>
<span data-ttu-id="7c9c3-105">Azure Media Services erbjuder flera alternativ för kodning av media i molnet.</span><span class="sxs-lookup"><span data-stu-id="7c9c3-105">Azure Media Services provides multiple options for the encoding of media in the cloud.</span></span>

<span data-ttu-id="7c9c3-106">När börjat med Media Services är det viktigt att förstå skillnaden mellan codec och filformat.</span><span class="sxs-lookup"><span data-stu-id="7c9c3-106">When starting out with Media Services, it is important to understand the difference between codecs and file formats.</span></span>
<span data-ttu-id="7c9c3-107">Codec-rutiner är programvara som implementerar komprimering/dekomprimering algoritmer filformat är behållare som innehåller den komprimerad videon.</span><span class="sxs-lookup"><span data-stu-id="7c9c3-107">Codecs are the software that implements the compression/decompression algorithms whereas file formats are containers that hold the compressed video.</span></span>

<span data-ttu-id="7c9c3-108">Media Services tillhandahåller en dynamisk paketering som gör att du kan leverera ditt innehåll MP4 eller Smooth Streaming-kodade med anpassad bithastighet i strömningsformat som stöds av Media Services (MPEG DASH, HLS, Smooth Streaming) utan att du behöver packa om till dessa strömningsformat.</span><span class="sxs-lookup"><span data-stu-id="7c9c3-108">Media Services provides dynamic packaging which allows you to deliver your adaptive bitrate MP4 or Smooth Streaming encoded content in streaming formats supported by Media Services (MPEG DASH, HLS, Smooth Streaming) without you having to re-package into these streaming formats.</span></span>

>[!NOTE]
><span data-ttu-id="7c9c3-109">När ditt AMS-konto skapas läggs en **standard**-slutpunkt för direktuppspelning till på ditt konto med tillståndet **Stoppad**.</span><span class="sxs-lookup"><span data-stu-id="7c9c3-109">When your AMS account is created a **default** streaming endpoint is added to your account in the **Stopped** state.</span></span> <span data-ttu-id="7c9c3-110">Om du vill starta direktuppspelning av innehåll och dra nytta av dynamisk paketering och dynamisk kryptering måste slutpunkten för direktuppspelning som du vill spela upp innehåll från ha tillståndet **Körs**.</span><span class="sxs-lookup"><span data-stu-id="7c9c3-110">To start streaming your content and take advantage of dynamic packaging and dynamic encryption, the streaming endpoint from which you want to stream content has to be in the **Running** state.</span></span> <span data-ttu-id="7c9c3-111">Dra nytta av [dynamisk paketering](media-services-dynamic-packaging-overview.md), måste du göra följande:</span><span class="sxs-lookup"><span data-stu-id="7c9c3-111">To take advantage of [dynamic packaging](media-services-dynamic-packaging-overview.md), you need to do the following:</span></span>
>
><span data-ttu-id="7c9c3-112">Dessutom koda källfilen till en uppsättning MP4-filer med anpassningsbar bithastighet eller Smooth Streaming-filer (de kodningsstegen senare i den här självstudiekursen).</span><span class="sxs-lookup"><span data-stu-id="7c9c3-112">Also, encode your source file into a set of adaptive bitrate MP4 files or adaptive bitrate Smooth Streaming files (the encoding steps are demonstrated later in this tutorial).</span></span>

<span data-ttu-id="7c9c3-113">Media Services stöder följande på begäran kodare som beskrivs i den här artikeln:</span><span class="sxs-lookup"><span data-stu-id="7c9c3-113">Media Services supports the following on demand encoders that are described in this article:</span></span>

* [<span data-ttu-id="7c9c3-114">Media Encoder Standard</span><span class="sxs-lookup"><span data-stu-id="7c9c3-114">Media Encoder Standard</span></span>](media-services-encode-asset.md#media-encoder-standard)
* [<span data-ttu-id="7c9c3-115">Arbetsflöde för Media Encoder Premium</span><span class="sxs-lookup"><span data-stu-id="7c9c3-115">Media Encoder Premium Workflow</span></span>](media-services-encode-asset.md#media-encoder-premium-workflow)

<span data-ttu-id="7c9c3-116">Den här artikeln ger en kort översikt över på begäran mediet kodare och innehåller länkar till artiklar som ger mer detaljerad information.</span><span class="sxs-lookup"><span data-stu-id="7c9c3-116">This article gives a brief overview of on demand media encoders and provides links to articles that give more detailed information.</span></span> <span data-ttu-id="7c9c3-117">Avsnittet innehåller också jämförelse av kodare.</span><span class="sxs-lookup"><span data-stu-id="7c9c3-117">The topic also provides comparison of the encoders.</span></span>

>[!NOTE]
><span data-ttu-id="7c9c3-118">Som standard har varje Media Services-konto en aktiv kodning aktivitet i taget.</span><span class="sxs-lookup"><span data-stu-id="7c9c3-118">By default each Media Services account can have one active encoding task at a time.</span></span> <span data-ttu-id="7c9c3-119">Du kan reservera kodning enheter som du kan ha flera kodning aktiviteter som körs samtidigt, ett för varje kodning reserverad enhet som du köper.</span><span class="sxs-lookup"><span data-stu-id="7c9c3-119">You can reserve encoding units that allow you to have multiple encoding tasks running concurrently, one for each encoding reserved unit you purchase.</span></span> <span data-ttu-id="7c9c3-120">Mer information finns i [skalning kodning enheter](media-services-scale-media-processing-overview.md).</span><span class="sxs-lookup"><span data-stu-id="7c9c3-120">For information, see [Scaling encoding units](media-services-scale-media-processing-overview.md).</span></span>

## <a name="media-encoder-standard"></a><span data-ttu-id="7c9c3-121">Media Encoder Standard</span><span class="sxs-lookup"><span data-stu-id="7c9c3-121">Media Encoder Standard</span></span>
### <a name="how-to-use"></a><span data-ttu-id="7c9c3-122">Hur du ska använda detta</span><span class="sxs-lookup"><span data-stu-id="7c9c3-122">How to use</span></span>
[<span data-ttu-id="7c9c3-123">Koda med Media Encoder Standard</span><span class="sxs-lookup"><span data-stu-id="7c9c3-123">How to encode with Media Encoder Standard</span></span>](media-services-dotnet-encode-with-media-encoder-standard.md)

### <a name="formats"></a><span data-ttu-id="7c9c3-124">Format</span><span class="sxs-lookup"><span data-stu-id="7c9c3-124">Formats</span></span>
[<span data-ttu-id="7c9c3-125">Format och codec</span><span class="sxs-lookup"><span data-stu-id="7c9c3-125">Formats and codecs</span></span>](media-services-media-encoder-standard-formats.md)

### <a name="presets"></a><span data-ttu-id="7c9c3-126">Förinställningar</span><span class="sxs-lookup"><span data-stu-id="7c9c3-126">Presets</span></span>
<span data-ttu-id="7c9c3-127">Media Encoder Standard konfigureras med hjälp av en kodare förinställningar beskrivs [här](http://go.microsoft.com/fwlink/?linkid=618336&clcid=0x409).</span><span class="sxs-lookup"><span data-stu-id="7c9c3-127">Media Encoder Standard is configured using one of the encoder presets described [here](http://go.microsoft.com/fwlink/?linkid=618336&clcid=0x409).</span></span>

### <a name="input-and-output-metadata"></a><span data-ttu-id="7c9c3-128">Indata och utdata metadata</span><span class="sxs-lookup"><span data-stu-id="7c9c3-128">Input and output metadata</span></span>
<span data-ttu-id="7c9c3-129">Inkommande metadata kodare beskrivs [här](media-services-input-metadata-schema.md).</span><span class="sxs-lookup"><span data-stu-id="7c9c3-129">The encoders input metadata is described [here](media-services-input-metadata-schema.md).</span></span>

<span data-ttu-id="7c9c3-130">Kodare utdata metadata beskrivs [här](media-services-output-metadata-schema.md).</span><span class="sxs-lookup"><span data-stu-id="7c9c3-130">The encoders output metadata is described [here](media-services-output-metadata-schema.md).</span></span>

### <a name="generate-thumbnails"></a><span data-ttu-id="7c9c3-131">Generera miniatyrbilder</span><span class="sxs-lookup"><span data-stu-id="7c9c3-131">Generate thumbnails</span></span>
<span data-ttu-id="7c9c3-132">Mer information finns i [så att generera miniatyrbilder med Media Encoder Standard](media-services-advanced-encoding-with-mes.md#thumbnails).</span><span class="sxs-lookup"><span data-stu-id="7c9c3-132">For information, see [How to generate thumbnails using Media Encoder Standard](media-services-advanced-encoding-with-mes.md#thumbnails).</span></span>

### <a name="trim-videos-clipping"></a><span data-ttu-id="7c9c3-133">Trim videor (urklippet)</span><span class="sxs-lookup"><span data-stu-id="7c9c3-133">Trim videos (clipping)</span></span>
<span data-ttu-id="7c9c3-134">Mer information finns i [så trim videor med Media Encoder Standard](media-services-advanced-encoding-with-mes.md#trim_video).</span><span class="sxs-lookup"><span data-stu-id="7c9c3-134">For information, see [How to trim videos using Media Encoder Standard](media-services-advanced-encoding-with-mes.md#trim_video).</span></span>

### <a name="create-overlays"></a><span data-ttu-id="7c9c3-135">Skapa överlägg</span><span class="sxs-lookup"><span data-stu-id="7c9c3-135">Create overlays</span></span>
<span data-ttu-id="7c9c3-136">Mer information finns i [hur du skapar överlägg med Media Encoder Standard](media-services-advanced-encoding-with-mes.md#overlay).</span><span class="sxs-lookup"><span data-stu-id="7c9c3-136">For information, see [How to create overlays using Media Encoder Standard](media-services-advanced-encoding-with-mes.md#overlay).</span></span>

### <a name="see-also"></a><span data-ttu-id="7c9c3-137">Se även</span><span class="sxs-lookup"><span data-stu-id="7c9c3-137">See also</span></span>
[<span data-ttu-id="7c9c3-138">Media Services-blogg</span><span class="sxs-lookup"><span data-stu-id="7c9c3-138">The Media Services blog</span></span>](https://azure.microsoft.com/blog/2015/07/16/announcing-the-general-availability-of-media-encoder-standard/)

## <a name="media-encoder-premium-workflow"></a><span data-ttu-id="7c9c3-139">Arbetsflöde för Media Encoder Premium</span><span class="sxs-lookup"><span data-stu-id="7c9c3-139">Media Encoder Premium Workflow</span></span>
### <a name="overview"></a><span data-ttu-id="7c9c3-140">Översikt</span><span class="sxs-lookup"><span data-stu-id="7c9c3-140">Overview</span></span>
[<span data-ttu-id="7c9c3-141">Introduktion till Premium-kodning i Azure Media Services</span><span class="sxs-lookup"><span data-stu-id="7c9c3-141">Introducing Premium Encoding in Azure Media Services</span></span>](https://azure.microsoft.com/blog/2015/03/05/introducing-premium-encoding-in-azure-media-services/)

### <a name="how-to-use"></a><span data-ttu-id="7c9c3-142">Hur du ska använda detta</span><span class="sxs-lookup"><span data-stu-id="7c9c3-142">How to use</span></span>
<span data-ttu-id="7c9c3-143">Arbetsflöde för Media Encoder Premium konfigureras med hjälp av komplexa arbetsflöden.</span><span class="sxs-lookup"><span data-stu-id="7c9c3-143">Media Encoder Premium Workflow is configured using complex workflows.</span></span> <span data-ttu-id="7c9c3-144">Arbetsflödesfiler kunde skapas och uppdateras med den [Arbetsflödesdesignern](media-services-workflow-designer.md) verktyget.</span><span class="sxs-lookup"><span data-stu-id="7c9c3-144">Workflow files could be created and updated using the [Workflow Designer](media-services-workflow-designer.md) tool.</span></span>

[<span data-ttu-id="7c9c3-145">Hur du använder Premium kodning i Azure Media Services</span><span class="sxs-lookup"><span data-stu-id="7c9c3-145">How to Use Premium Encoding in Azure Media Services</span></span>](https://azure.microsoft.com/blog/2015/03/06/how-to-use-premium-encoding-in-azure-media-services/)

### <a name="known-issues"></a><span data-ttu-id="7c9c3-146">Kända problem</span><span class="sxs-lookup"><span data-stu-id="7c9c3-146">Known issues</span></span>
<span data-ttu-id="7c9c3-147">Om din indatavideo inte innehåller innehåller dold textning utdata tillgången fortfarande en tom TTML-fil.</span><span class="sxs-lookup"><span data-stu-id="7c9c3-147">If your input video does not contain closed captioning, the output Asset will still contain an empty TTML file.</span></span>


## <a name="media-services-learning-paths"></a><span data-ttu-id="7c9c3-148">Sökvägar för Media Services-utbildning</span><span class="sxs-lookup"><span data-stu-id="7c9c3-148">Media Services learning paths</span></span>
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a><span data-ttu-id="7c9c3-149">Ge feedback</span><span class="sxs-lookup"><span data-stu-id="7c9c3-149">Provide feedback</span></span>
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

## <a name="related-articles"></a><span data-ttu-id="7c9c3-150">Relaterade artiklar</span><span class="sxs-lookup"><span data-stu-id="7c9c3-150">Related articles</span></span>
* [<span data-ttu-id="7c9c3-151">Utföra avancerade kodning uppgifter genom att anpassa Media Encoder Standard förinställningar</span><span class="sxs-lookup"><span data-stu-id="7c9c3-151">Perform advanced encoding tasks by customizing Media Encoder Standard presets</span></span>](media-services-custom-mes-presets-with-dotnet.md)
* [<span data-ttu-id="7c9c3-152">Kvoter och begränsningar</span><span class="sxs-lookup"><span data-stu-id="7c9c3-152">Quotas and Limitations</span></span>](media-services-quotas-and-limitations.md)

<!--Reference links in article-->
[1]: http://azure.microsoft.com/pricing/details/media-services/
