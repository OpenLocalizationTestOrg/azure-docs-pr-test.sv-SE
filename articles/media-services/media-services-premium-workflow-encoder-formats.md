---
title: "aaaMedia kodare Premium arbetsflödet format och codec | Microsoft Docs"
description: "Det här avsnittet ger en översikt över arbetsflödet medieformat kodare Premium format och codec"
services: media-services
documentationcenter: 
author: juliako
manager: erik43
editor: 
ms.assetid: b197fce8-3b9b-4189-8d08-486810c0426f
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/09/2017
ms.author: juliako;anilmur
ms.openlocfilehash: e781384ca8f08926f00c83b6710fd413ce2a3e1c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="media-encoder-premium-workflow-formats-and-codecs"></a><span data-ttu-id="6a909-103">Arbetsflöde för Media Encoder Premium format och codec</span><span class="sxs-lookup"><span data-stu-id="6a909-103">Media Encoder Premium Workflow formats and codecs</span></span>
> [!NOTE]
> <span data-ttu-id="6a909-104">E-mepd på Microsoft.com för premium-kodare frågor.</span><span class="sxs-lookup"><span data-stu-id="6a909-104">For premium encoder questions, email mepd at Microsoft.com.</span></span>
> 
> <span data-ttu-id="6a909-105">Media Encoder Premium arbetsflöde medieprocessor som beskrivs i det här avsnittet är inte tillgänglig i Kina.</span><span class="sxs-lookup"><span data-stu-id="6a909-105">Media Encoder Premium Workflow media processor discussed in this topic is not available in China.</span></span> 
> 
> 

<span data-ttu-id="6a909-106">Det här dokumentet innehåller en lista över inkommande och utgående filformat och codec-rutiner som stöds av hello offentliga förhandsversionen av hello **Media Encoder Premium arbetsflöde** kodare.</span><span class="sxs-lookup"><span data-stu-id="6a909-106">This document contains a list of input and output file formats and codecs that are supported by hello public preview version of hello **Media Encoder Premium Workflow** encoder.</span></span>

[<span data-ttu-id="6a909-107">Media Encoder Premium Worflow indata format och codec</span><span class="sxs-lookup"><span data-stu-id="6a909-107">Media Encoder Premium Worflow Input Formats and Codecs</span></span>](#input_formats)

[<span data-ttu-id="6a909-108">Utdataformat för Media Encoder Premium Worflow och codec</span><span class="sxs-lookup"><span data-stu-id="6a909-108">Media Encoder Premium Worflow Output Formats and Codecs</span></span>](#output_formats)

<span data-ttu-id="6a909-109">**Arbetsflöde för Media Encoder Premium** stöder textning beskrivs i [detta](#closed_captioning) avsnitt.</span><span class="sxs-lookup"><span data-stu-id="6a909-109">**Media Encoder Premium Workflow** supports closed captioning described in [this](#closed_captioning) section.</span></span> 

## <span data-ttu-id="6a909-110"><a id="input_formats"></a>Arbetsflöde för Media Encoder Premium indata format och codec</span><span class="sxs-lookup"><span data-stu-id="6a909-110"><a id="input_formats"></a>Media Encoder Premium Workflow Input Formats and Codecs</span></span>
<span data-ttu-id="6a909-111">hello efter avsnittet visar hello codec och filformat som har stöd för den här medieprocessor som indata.</span><span class="sxs-lookup"><span data-stu-id="6a909-111">hello following section lists hello codecs and file formats that this media processor supports as input.</span></span>

### <a name="input-containerfile-formats"></a><span data-ttu-id="6a909-112">Ange behållare/filformat</span><span class="sxs-lookup"><span data-stu-id="6a909-112">Input Container/File Formats</span></span>
* <span data-ttu-id="6a909-113">Adobe® Flash® F4V</span><span class="sxs-lookup"><span data-stu-id="6a909-113">Adobe® Flash® F4V</span></span>
* <span data-ttu-id="6a909-114">MXF/SMPTE 377M</span><span class="sxs-lookup"><span data-stu-id="6a909-114">MXF/SMPTE 377M</span></span>
* <span data-ttu-id="6a909-115">GXF</span><span class="sxs-lookup"><span data-stu-id="6a909-115">GXF</span></span>
* <span data-ttu-id="6a909-116">MPEG-2-transportströmmar</span><span class="sxs-lookup"><span data-stu-id="6a909-116">MPEG-2 Transport Streams</span></span>
* <span data-ttu-id="6a909-117">MPEG-2-strömmar</span><span class="sxs-lookup"><span data-stu-id="6a909-117">MPEG-2 Program Streams</span></span>
* <span data-ttu-id="6a909-118">MP4-MPEG-4</span><span class="sxs-lookup"><span data-stu-id="6a909-118">MPEG-4/MP4</span></span>
* <span data-ttu-id="6a909-119">Windows Media/ASF</span><span class="sxs-lookup"><span data-stu-id="6a909-119">Windows Media/ASF</span></span>
* <span data-ttu-id="6a909-120">AVI (okomprimerad 8-bitars/10 bitar)</span><span class="sxs-lookup"><span data-stu-id="6a909-120">AVI (Uncompressed 8bit/10bit)</span></span>

### <a name="input-video-codecs"></a><span data-ttu-id="6a909-121">Inkommande Video-codec</span><span class="sxs-lookup"><span data-stu-id="6a909-121">Input Video Codecs</span></span>
* <span data-ttu-id="6a909-122">AVC 8-/ 10-bitars, in too4:2:2, inklusive AVCIntra</span><span class="sxs-lookup"><span data-stu-id="6a909-122">AVC 8-bit/10-bit, up too4:2:2, including AVCIntra</span></span>
* <span data-ttu-id="6a909-123">Avid DNxHD (i MXF)</span><span class="sxs-lookup"><span data-stu-id="6a909-123">Avid DNxHD (in MXF)</span></span>
* <span data-ttu-id="6a909-124">DVCPro/DVCProHD (i MXF)</span><span class="sxs-lookup"><span data-stu-id="6a909-124">DVCPro/DVCProHD (in MXF)</span></span>
* <span data-ttu-id="6a909-125">JPEG2000</span><span class="sxs-lookup"><span data-stu-id="6a909-125">JPEG2000</span></span>
* <span data-ttu-id="6a909-126">MPEG-2 (too422 profil och högnivå, inklusive varianter, till exempel XDCAM, XDCAM HD, XDCAM IMX, CableLabs® och D10)</span><span class="sxs-lookup"><span data-stu-id="6a909-126">MPEG-2 (up too422 Profile and High Level; including variants such as XDCAM, XDCAM HD, XDCAM IMX, CableLabs® and D10)</span></span>
* <span data-ttu-id="6a909-127">MPEG-1</span><span class="sxs-lookup"><span data-stu-id="6a909-127">MPEG-1</span></span>
* <span data-ttu-id="6a909-128">Windows Media Video-VC-1</span><span class="sxs-lookup"><span data-stu-id="6a909-128">Windows Media Video/VC-1</span></span>

### <a name="input-audio-codecs"></a><span data-ttu-id="6a909-129">Inkommande ljud-codec</span><span class="sxs-lookup"><span data-stu-id="6a909-129">Input Audio Codecs</span></span>
* <span data-ttu-id="6a909-130">AES (SMPTE 331 M och 302 M, AES3 2003)</span><span class="sxs-lookup"><span data-stu-id="6a909-130">AES (SMPTE 331M and 302M, AES3-2003)</span></span>
* <span data-ttu-id="6a909-131">Dolby® E</span><span class="sxs-lookup"><span data-stu-id="6a909-131">Dolby® E</span></span>
* <span data-ttu-id="6a909-132">Dolby® Digital (AC3)</span><span class="sxs-lookup"><span data-stu-id="6a909-132">Dolby® Digital (AC3)</span></span>
* <span data-ttu-id="6a909-133">AAC (AAC LC, HE AAC och AAC-HEv2 in too5.1)</span><span class="sxs-lookup"><span data-stu-id="6a909-133">AAC (AAC-LC, AAC-HE, and AAC-HEv2; up too5.1)</span></span>
* <span data-ttu-id="6a909-134">MPEG nivå 2</span><span class="sxs-lookup"><span data-stu-id="6a909-134">MPEG Layer 2</span></span>
* <span data-ttu-id="6a909-135">MP3 (MPEG-1 ljud nivå 3)</span><span class="sxs-lookup"><span data-stu-id="6a909-135">MP3 (MPEG-1 Audio Layer 3)</span></span>
* <span data-ttu-id="6a909-136">Windows Media Audio</span><span class="sxs-lookup"><span data-stu-id="6a909-136">Windows Media Audio</span></span>
* <span data-ttu-id="6a909-137">WAV PCM /</span><span class="sxs-lookup"><span data-stu-id="6a909-137">WAV/PCM</span></span>

## <span data-ttu-id="6a909-138"><a id="output_format"></a>Media Encoder Premium arbetsflödet utdataformat och codec</span><span class="sxs-lookup"><span data-stu-id="6a909-138"><a id="output_format"></a>Media Encoder Premium Workflow Output Formats and Codecs</span></span>
<span data-ttu-id="6a909-139">hello innehåller följande avsnitt hello codec och filformat som stöds som utdata från den här medieprocessor.</span><span class="sxs-lookup"><span data-stu-id="6a909-139">hello following section lists hello codecs and file formats that are supported as output from this media processor.</span></span>

### <a name="output-containerfile-formats"></a><span data-ttu-id="6a909-140">Utdata behållare/filformat</span><span class="sxs-lookup"><span data-stu-id="6a909-140">Output Container/File Formats</span></span>
* <span data-ttu-id="6a909-141">Adobe® Flash® F4V</span><span class="sxs-lookup"><span data-stu-id="6a909-141">Adobe® Flash® F4V</span></span>
* <span data-ttu-id="6a909-142">MXF (OP1a, XDCAM och AS02)</span><span class="sxs-lookup"><span data-stu-id="6a909-142">MXF (OP1a, XDCAM and AS02)</span></span>
* <span data-ttu-id="6a909-143">DPP (inklusive AS11)</span><span class="sxs-lookup"><span data-stu-id="6a909-143">DPP (including AS11)</span></span>
* <span data-ttu-id="6a909-144">GXF</span><span class="sxs-lookup"><span data-stu-id="6a909-144">GXF</span></span>
* <span data-ttu-id="6a909-145">MP4-MPEG-4</span><span class="sxs-lookup"><span data-stu-id="6a909-145">MPEG-4/MP4</span></span>
* <span data-ttu-id="6a909-146">Windows Media/ASF</span><span class="sxs-lookup"><span data-stu-id="6a909-146">Windows Media/ASF</span></span>
* <span data-ttu-id="6a909-147">AVI (okomprimerad 8-bitars/10 bitar)</span><span class="sxs-lookup"><span data-stu-id="6a909-147">AVI (Uncompressed 8bit/10bit)</span></span>
* <span data-ttu-id="6a909-148">Filformatet för Smooth Streaming (PIFF 1.3)</span><span class="sxs-lookup"><span data-stu-id="6a909-148">Smooth Streaming File Format (PIFF 1.3)</span></span>
* <span data-ttu-id="6a909-149">MPEG-TS</span><span class="sxs-lookup"><span data-stu-id="6a909-149">MPEG-TS</span></span> 

### <a name="output-video-codecs"></a><span data-ttu-id="6a909-150">Utdata Video-codec</span><span class="sxs-lookup"><span data-stu-id="6a909-150">Output Video Codecs</span></span>
* <span data-ttu-id="6a909-151">AVC (H.264; 8-bitars upp tooHigh profil, nivå 5.2; 4K extra HD; AVC Intra)</span><span class="sxs-lookup"><span data-stu-id="6a909-151">AVC (H.264; 8-bit; up tooHigh Profile, Level 5.2; 4K Ultra HD; AVC Intra)</span></span>
* <span data-ttu-id="6a909-152">Avid DNxHD (i MXF)</span><span class="sxs-lookup"><span data-stu-id="6a909-152">Avid DNxHD (in MXF)</span></span>
* <span data-ttu-id="6a909-153">DVCPro/DVCProHD (i MXF)</span><span class="sxs-lookup"><span data-stu-id="6a909-153">DVCPro/DVCProHD (in MXF)</span></span>
* <span data-ttu-id="6a909-154">MPEG-2 (too422 profil och högnivå, inklusive varianter, till exempel XDCAM, XDCAM HD, XDCAM IMX, CableLabs® och D10)</span><span class="sxs-lookup"><span data-stu-id="6a909-154">MPEG-2 (up too422 Profile and High Level; including variants such as XDCAM, XDCAM HD, XDCAM IMX, CableLabs® and D10)</span></span>
* <span data-ttu-id="6a909-155">MPEG-1</span><span class="sxs-lookup"><span data-stu-id="6a909-155">MPEG-1</span></span>
* <span data-ttu-id="6a909-156">Windows Media Video-VC-1</span><span class="sxs-lookup"><span data-stu-id="6a909-156">Windows Media Video/VC-1</span></span>
* <span data-ttu-id="6a909-157">JPEG-miniatyrer skapas</span><span class="sxs-lookup"><span data-stu-id="6a909-157">JPEG thumbnail creation</span></span>

### <a name="output-audio-codecs"></a><span data-ttu-id="6a909-158">Ljud-codec utdata</span><span class="sxs-lookup"><span data-stu-id="6a909-158">Output Audio Codecs</span></span>
* <span data-ttu-id="6a909-159">AES (SMPTE 331 M och 302 M, AES3 2003)</span><span class="sxs-lookup"><span data-stu-id="6a909-159">AES (SMPTE 331M and 302M, AES3-2003)</span></span>
* <span data-ttu-id="6a909-160">Dolby® Digital (AC3)</span><span class="sxs-lookup"><span data-stu-id="6a909-160">Dolby® Digital (AC3)</span></span>
* <span data-ttu-id="6a909-161">Dolby® Digital Plus (E-AC3) in too7.1</span><span class="sxs-lookup"><span data-stu-id="6a909-161">Dolby® Digital Plus (E-AC3) up too7.1</span></span>
* <span data-ttu-id="6a909-162">AAC (AAC LC, HE AAC och AAC-HEv2 in too5.1)</span><span class="sxs-lookup"><span data-stu-id="6a909-162">AAC (AAC-LC, AAC-HE, and AAC-HEv2; up too5.1)</span></span>
* <span data-ttu-id="6a909-163">MPEG nivå 2</span><span class="sxs-lookup"><span data-stu-id="6a909-163">MPEG Layer 2</span></span>
* <span data-ttu-id="6a909-164">MP3 (MPEG-1 ljud nivå 3)</span><span class="sxs-lookup"><span data-stu-id="6a909-164">MP3 (MPEG-1 Audio Layer 3)</span></span>
* <span data-ttu-id="6a909-165">Windows Media Audio</span><span class="sxs-lookup"><span data-stu-id="6a909-165">Windows Media Audio</span></span>

>[!NOTE]
><span data-ttu-id="6a909-166">Om du kodar tooDolby® Digital (AC3) hello utdata kan bara skrivas till en ISO MP4-fil.</span><span class="sxs-lookup"><span data-stu-id="6a909-166">If you encode tooDolby® Digital (AC3), hello output can only be written into an ISO MP4 file.</span></span>

## <span data-ttu-id="6a909-167"><a id="closed_captioning"></a>Stöd för dold textning</span><span class="sxs-lookup"><span data-stu-id="6a909-167"><a id="closed_captioning"></a>Support for Closed Captioning</span></span>
<span data-ttu-id="6a909-168">På infognings **Media Encoder Premium arbetsflöde** stöder:</span><span class="sxs-lookup"><span data-stu-id="6a909-168">On ingest, **Media Encoder Premium Workflow** supports:</span></span>

1. <span data-ttu-id="6a909-169">SCC filer</span><span class="sxs-lookup"><span data-stu-id="6a909-169">SCC files</span></span>
2. <span data-ttu-id="6a909-170">SMPTE TT filer</span><span class="sxs-lookup"><span data-stu-id="6a909-170">SMPTE-TT files</span></span>
3. <span data-ttu-id="6a909-171">CEA-608/CEA-708 – som användardata (H.264 elementära strömmar, ATSC/53, SCTE20 SFI meddelanden) eller som extra data i MXF/GXF filer</span><span class="sxs-lookup"><span data-stu-id="6a909-171">CEA-608/CEA-708 – carried as user data (SEI messages of H.264 elementary streams, ATSC/53, SCTE20) or carried as ancillary data in MXF/GXF files</span></span>
4. <span data-ttu-id="6a909-172">STL underrubrik filer</span><span class="sxs-lookup"><span data-stu-id="6a909-172">STL subtitle files</span></span>

<span data-ttu-id="6a909-173">Hello följande alternativ är tillgängliga på utdata:</span><span class="sxs-lookup"><span data-stu-id="6a909-173">On output, hello following options are available:</span></span>

1. <span data-ttu-id="6a909-174">CEA 608 tooCEA 708 översättning</span><span class="sxs-lookup"><span data-stu-id="6a909-174">CEA-608 tooCEA-708 translation</span></span>
2. <span data-ttu-id="6a909-175">CEA-608/CEA-708 passera (inbäddade i SFI meddelanden för H.264 elementära strömmar eller förs som extra data i MXF-filer)</span><span class="sxs-lookup"><span data-stu-id="6a909-175">CEA-608/CEA-708 pass through (embedded in SEI messages of H.264 elementary streams, or carried as ancillary data in MXF files)</span></span>
3. <span data-ttu-id="6a909-176">SCC</span><span class="sxs-lookup"><span data-stu-id="6a909-176">SCC</span></span>
4. <span data-ttu-id="6a909-177">SMPTE TT (från källan CEA 608 per SMPTE RP2052, även DFXP skapas)</span><span class="sxs-lookup"><span data-stu-id="6a909-177">SMPTE Timed Text (from source CEA-608 per SMPTE RP2052; including DFXP file creation)</span></span>
5. <span data-ttu-id="6a909-178">SRT underrubrik fil</span><span class="sxs-lookup"><span data-stu-id="6a909-178">SRT Subtitle file</span></span>
6. <span data-ttu-id="6a909-179">DVB underrubrik strömmar</span><span class="sxs-lookup"><span data-stu-id="6a909-179">DVB subtitle streams</span></span>

<span data-ttu-id="6a909-180">Obs: stöds inte alla hello ovan utdataformat för leverans via strömning i Azure Media Services.</span><span class="sxs-lookup"><span data-stu-id="6a909-180">Note: not all of hello above output formats are supported for delivery via streaming in Azure Media Services.</span></span>

## <a name="known-issues"></a><span data-ttu-id="6a909-181">Kända problem</span><span class="sxs-lookup"><span data-stu-id="6a909-181">Known issues</span></span>
<span data-ttu-id="6a909-182">Om din indatavideo inte innehåller textning, utdata hello tillgången fortfarande innehåller en tom TTML-fil.</span><span class="sxs-lookup"><span data-stu-id="6a909-182">If your input video does not contain closed captioning, hello output Asset will still contain an empty TTML file.</span></span> 

## <a name="media-services-learning-paths"></a><span data-ttu-id="6a909-183">Sökvägar för Media Services-utbildning</span><span class="sxs-lookup"><span data-stu-id="6a909-183">Media Services learning paths</span></span>
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a><span data-ttu-id="6a909-184">Ge feedback</span><span class="sxs-lookup"><span data-stu-id="6a909-184">Provide feedback</span></span>
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

