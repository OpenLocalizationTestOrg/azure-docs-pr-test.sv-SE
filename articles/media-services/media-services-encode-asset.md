---
title: "aaaOverview och jämförelse av Azure på kräver mediet kodare | Microsoft Docs"
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
ms.openlocfilehash: 24a3e0a16162b1bebfcde290b6baf2dd8dbfff17
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="overview-and-comparison-of-azure-on-demand-media-encoders"></a><span data-ttu-id="d1d33-103">Översikt över och jämförelse av Azure på begäran mediet kodare</span><span class="sxs-lookup"><span data-stu-id="d1d33-103">Overview and comparison of Azure on demand media encoders</span></span>
## <a name="encoding-overview"></a><span data-ttu-id="d1d33-104">Kodning: översikt</span><span class="sxs-lookup"><span data-stu-id="d1d33-104">Encoding overview</span></span>
<span data-ttu-id="d1d33-105">Azure Media Services erbjuder flera alternativ för hello-kodning av media i hello molnet.</span><span class="sxs-lookup"><span data-stu-id="d1d33-105">Azure Media Services provides multiple options for hello encoding of media in hello cloud.</span></span>

<span data-ttu-id="d1d33-106">När börjat med Media Services är det viktigt toounderstand hello skillnaden mellan codec och filformat.</span><span class="sxs-lookup"><span data-stu-id="d1d33-106">When starting out with Media Services, it is important toounderstand hello difference between codecs and file formats.</span></span>
<span data-ttu-id="d1d33-107">Codec-rutiner är hello-programvara som implementerar hello komprimering/dekomprimering algoritmer filformat är behållare som innehåller hello komprimerade video.</span><span class="sxs-lookup"><span data-stu-id="d1d33-107">Codecs are hello software that implements hello compression/decompression algorithms whereas file formats are containers that hold hello compressed video.</span></span>

<span data-ttu-id="d1d33-108">Media Services tillhandahåller en dynamisk paketering som gör att du toodeliver din anpassningsbar bithastighet MP4 eller Smooth Streaming-kodade innehåll i strömningsformat som stöds av Media Services (MPEG DASH, HLS, Smooth Streaming) utan att behöva toore-paket till dessa strömningsformat.</span><span class="sxs-lookup"><span data-stu-id="d1d33-108">Media Services provides dynamic packaging which allows you toodeliver your adaptive bitrate MP4 or Smooth Streaming encoded content in streaming formats supported by Media Services (MPEG DASH, HLS, Smooth Streaming) without you having toore-package into these streaming formats.</span></span>

>[!NOTE]
><span data-ttu-id="d1d33-109">När AMS-kontot skapas en **standard** strömningsslutpunkt har lagts till tooyour konto i hello **stoppad** tillstånd.</span><span class="sxs-lookup"><span data-stu-id="d1d33-109">When your AMS account is created a **default** streaming endpoint is added tooyour account in hello **Stopped** state.</span></span> <span data-ttu-id="d1d33-110">toostart strömning ditt innehåll och dra nytta av dynamisk paketering och dynamisk kryptering hello strömningsslutpunkt som du vill toostream innehåll har toobe i hello **kör** tillstånd.</span><span class="sxs-lookup"><span data-stu-id="d1d33-110">toostart streaming your content and take advantage of dynamic packaging and dynamic encryption, hello streaming endpoint from which you want toostream content has toobe in hello **Running** state.</span></span> <span data-ttu-id="d1d33-111">tootake nytta av [dynamisk paketering](media-services-dynamic-packaging-overview.md), behöver du toodo hello följande:</span><span class="sxs-lookup"><span data-stu-id="d1d33-111">tootake advantage of [dynamic packaging](media-services-dynamic-packaging-overview.md), you need toodo hello following:</span></span>
>
><span data-ttu-id="d1d33-112">Dessutom koda källfilen till en uppsättning med anpassningsbar bithastighet MP4-filer eller Smooth Streaming-filer (hello kodningsstegen senare i den här självstudiekursen).</span><span class="sxs-lookup"><span data-stu-id="d1d33-112">Also, encode your source file into a set of adaptive bitrate MP4 files or adaptive bitrate Smooth Streaming files (hello encoding steps are demonstrated later in this tutorial).</span></span>

<span data-ttu-id="d1d33-113">Media Services stöder följande hello på begäran kodare som beskrivs i den här artikeln:</span><span class="sxs-lookup"><span data-stu-id="d1d33-113">Media Services supports hello following on demand encoders that are described in this article:</span></span>

* [<span data-ttu-id="d1d33-114">Media Encoder Standard</span><span class="sxs-lookup"><span data-stu-id="d1d33-114">Media Encoder Standard</span></span>](media-services-encode-asset.md#media-encoder-standard)
* [<span data-ttu-id="d1d33-115">Arbetsflöde för Media Encoder Premium</span><span class="sxs-lookup"><span data-stu-id="d1d33-115">Media Encoder Premium Workflow</span></span>](media-services-encode-asset.md#media-encoder-premium-workflow)

<span data-ttu-id="d1d33-116">Den här artikeln ger en kort översikt över på begäran mediet kodare och ger länkar tooarticles som ger mer detaljerad information.</span><span class="sxs-lookup"><span data-stu-id="d1d33-116">This article gives a brief overview of on demand media encoders and provides links tooarticles that give more detailed information.</span></span> <span data-ttu-id="d1d33-117">hello avsnittet innehåller också jämförelse av hello kodare.</span><span class="sxs-lookup"><span data-stu-id="d1d33-117">hello topic also provides comparison of hello encoders.</span></span>

>[!NOTE]
><span data-ttu-id="d1d33-118">Som standard har varje Media Services-konto en aktiv kodning aktivitet i taget.</span><span class="sxs-lookup"><span data-stu-id="d1d33-118">By default each Media Services account can have one active encoding task at a time.</span></span> <span data-ttu-id="d1d33-119">Du kan reservera kodning enheter som tillåter toohave flera kodning aktiviteter som körs samtidigt, ett för varje kodning reserverad enhet som du köper.</span><span class="sxs-lookup"><span data-stu-id="d1d33-119">You can reserve encoding units that allow you toohave multiple encoding tasks running concurrently, one for each encoding reserved unit you purchase.</span></span> <span data-ttu-id="d1d33-120">Mer information finns i [skalning kodning enheter](media-services-scale-media-processing-overview.md).</span><span class="sxs-lookup"><span data-stu-id="d1d33-120">For information, see [Scaling encoding units](media-services-scale-media-processing-overview.md).</span></span>

## <a name="media-encoder-standard"></a><span data-ttu-id="d1d33-121">Media Encoder Standard</span><span class="sxs-lookup"><span data-stu-id="d1d33-121">Media Encoder Standard</span></span>
### <a name="how-toouse"></a><span data-ttu-id="d1d33-122">Hur toouse</span><span class="sxs-lookup"><span data-stu-id="d1d33-122">How toouse</span></span>
[<span data-ttu-id="d1d33-123">Hur tooencode med Media Encoder Standard</span><span class="sxs-lookup"><span data-stu-id="d1d33-123">How tooencode with Media Encoder Standard</span></span>](media-services-dotnet-encode-with-media-encoder-standard.md)

### <a name="formats"></a><span data-ttu-id="d1d33-124">Format</span><span class="sxs-lookup"><span data-stu-id="d1d33-124">Formats</span></span>
[<span data-ttu-id="d1d33-125">Format och codec</span><span class="sxs-lookup"><span data-stu-id="d1d33-125">Formats and codecs</span></span>](media-services-media-encoder-standard-formats.md)

### <a name="presets"></a><span data-ttu-id="d1d33-126">Förinställningar</span><span class="sxs-lookup"><span data-stu-id="d1d33-126">Presets</span></span>
<span data-ttu-id="d1d33-127">Media Encoder Standard konfigureras med hjälp av en hello kodare förinställningar beskrivs [här](http://go.microsoft.com/fwlink/?linkid=618336&clcid=0x409).</span><span class="sxs-lookup"><span data-stu-id="d1d33-127">Media Encoder Standard is configured using one of hello encoder presets described [here](http://go.microsoft.com/fwlink/?linkid=618336&clcid=0x409).</span></span>

### <a name="input-and-output-metadata"></a><span data-ttu-id="d1d33-128">Indata och utdata metadata</span><span class="sxs-lookup"><span data-stu-id="d1d33-128">Input and output metadata</span></span>
<span data-ttu-id="d1d33-129">hello kodare inkommande metadata beskrivs [här](media-services-input-metadata-schema.md).</span><span class="sxs-lookup"><span data-stu-id="d1d33-129">hello encoders input metadata is described [here](media-services-input-metadata-schema.md).</span></span>

<span data-ttu-id="d1d33-130">hello kodare utdata metadata beskrivs [här](media-services-output-metadata-schema.md).</span><span class="sxs-lookup"><span data-stu-id="d1d33-130">hello encoders output metadata is described [here](media-services-output-metadata-schema.md).</span></span>

### <a name="generate-thumbnails"></a><span data-ttu-id="d1d33-131">Generera miniatyrbilder</span><span class="sxs-lookup"><span data-stu-id="d1d33-131">Generate thumbnails</span></span>
<span data-ttu-id="d1d33-132">Mer information finns i [hur toogenerate miniatyrer med Media Encoder Standard](media-services-advanced-encoding-with-mes.md#thumbnails).</span><span class="sxs-lookup"><span data-stu-id="d1d33-132">For information, see [How toogenerate thumbnails using Media Encoder Standard](media-services-advanced-encoding-with-mes.md#thumbnails).</span></span>

### <a name="trim-videos-clipping"></a><span data-ttu-id="d1d33-133">Trim videor (urklippet)</span><span class="sxs-lookup"><span data-stu-id="d1d33-133">Trim videos (clipping)</span></span>
<span data-ttu-id="d1d33-134">Mer information finns i [hur tootrim videor med Media Encoder Standard](media-services-advanced-encoding-with-mes.md#trim_video).</span><span class="sxs-lookup"><span data-stu-id="d1d33-134">For information, see [How tootrim videos using Media Encoder Standard](media-services-advanced-encoding-with-mes.md#trim_video).</span></span>

### <a name="create-overlays"></a><span data-ttu-id="d1d33-135">Skapa överlägg</span><span class="sxs-lookup"><span data-stu-id="d1d33-135">Create overlays</span></span>
<span data-ttu-id="d1d33-136">Mer information finns i [hur toocreate överlägg med Media Encoder Standard](media-services-advanced-encoding-with-mes.md#overlay).</span><span class="sxs-lookup"><span data-stu-id="d1d33-136">For information, see [How toocreate overlays using Media Encoder Standard](media-services-advanced-encoding-with-mes.md#overlay).</span></span>

### <a name="see-also"></a><span data-ttu-id="d1d33-137">Se även</span><span class="sxs-lookup"><span data-stu-id="d1d33-137">See also</span></span>
[<span data-ttu-id="d1d33-138">hello Media Services-bloggen</span><span class="sxs-lookup"><span data-stu-id="d1d33-138">hello Media Services blog</span></span>](https://azure.microsoft.com/blog/2015/07/16/announcing-the-general-availability-of-media-encoder-standard/)

## <a name="media-encoder-premium-workflow"></a><span data-ttu-id="d1d33-139">Arbetsflöde för Media Encoder Premium</span><span class="sxs-lookup"><span data-stu-id="d1d33-139">Media Encoder Premium Workflow</span></span>
### <a name="overview"></a><span data-ttu-id="d1d33-140">Översikt</span><span class="sxs-lookup"><span data-stu-id="d1d33-140">Overview</span></span>
[<span data-ttu-id="d1d33-141">Introduktion till Premium-kodning i Azure Media Services</span><span class="sxs-lookup"><span data-stu-id="d1d33-141">Introducing Premium Encoding in Azure Media Services</span></span>](https://azure.microsoft.com/blog/2015/03/05/introducing-premium-encoding-in-azure-media-services/)

### <a name="how-toouse"></a><span data-ttu-id="d1d33-142">Hur toouse</span><span class="sxs-lookup"><span data-stu-id="d1d33-142">How toouse</span></span>
<span data-ttu-id="d1d33-143">Arbetsflöde för Media Encoder Premium konfigureras med hjälp av komplexa arbetsflöden.</span><span class="sxs-lookup"><span data-stu-id="d1d33-143">Media Encoder Premium Workflow is configured using complex workflows.</span></span> <span data-ttu-id="d1d33-144">Arbetsflödesfiler kunde skapas och uppdateras med hello [Arbetsflödesdesignern](media-services-workflow-designer.md) verktyget.</span><span class="sxs-lookup"><span data-stu-id="d1d33-144">Workflow files could be created and updated using hello [Workflow Designer](media-services-workflow-designer.md) tool.</span></span>

[<span data-ttu-id="d1d33-145">Hur tooUse Premium kodning i Azure Media Services</span><span class="sxs-lookup"><span data-stu-id="d1d33-145">How tooUse Premium Encoding in Azure Media Services</span></span>](https://azure.microsoft.com/blog/2015/03/06/how-to-use-premium-encoding-in-azure-media-services/)

### <a name="known-issues"></a><span data-ttu-id="d1d33-146">Kända problem</span><span class="sxs-lookup"><span data-stu-id="d1d33-146">Known issues</span></span>
<span data-ttu-id="d1d33-147">Om din indatavideo inte innehåller textning, utdata hello tillgången fortfarande innehåller en tom TTML-fil.</span><span class="sxs-lookup"><span data-stu-id="d1d33-147">If your input video does not contain closed captioning, hello output Asset will still contain an empty TTML file.</span></span>


## <a name="media-services-learning-paths"></a><span data-ttu-id="d1d33-148">Sökvägar för Media Services-utbildning</span><span class="sxs-lookup"><span data-stu-id="d1d33-148">Media Services learning paths</span></span>
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a><span data-ttu-id="d1d33-149">Ge feedback</span><span class="sxs-lookup"><span data-stu-id="d1d33-149">Provide feedback</span></span>
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

## <a name="related-articles"></a><span data-ttu-id="d1d33-150">Relaterade artiklar</span><span class="sxs-lookup"><span data-stu-id="d1d33-150">Related articles</span></span>
* [<span data-ttu-id="d1d33-151">Utföra avancerade kodning uppgifter genom att anpassa Media Encoder Standard förinställningar</span><span class="sxs-lookup"><span data-stu-id="d1d33-151">Perform advanced encoding tasks by customizing Media Encoder Standard presets</span></span>](media-services-custom-mes-presets-with-dotnet.md)
* [<span data-ttu-id="d1d33-152">Kvoter och begränsningar</span><span class="sxs-lookup"><span data-stu-id="d1d33-152">Quotas and Limitations</span></span>](media-services-quotas-and-limitations.md)

<!--Reference links in article-->
[1]: http://azure.microsoft.com/pricing/details/media-services/
