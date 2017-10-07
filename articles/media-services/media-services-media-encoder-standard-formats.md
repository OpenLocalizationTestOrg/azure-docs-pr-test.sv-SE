---
title: aaaMedia Encoder Standard format och codec
description: "Det här avsnittet ger en översikt över Media Encoder Standard format och codec."
services: media-services
documentationcenter: 
author: juliako
manager: cfowler
editor: 
ms.assetid: f334b1ce-2f56-4968-a019-f0a2b0016d9f
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/20/2017
ms.author: juliako;anilmur
ms.openlocfilehash: 51a67f372dff579383ffcfa988e8f4d38ad44a72
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="media-encoder-standard-formats-and-codecs"></a><span data-ttu-id="6a506-103">Standardformat för Media Encoder och codec-rutiner</span><span class="sxs-lookup"><span data-stu-id="6a506-103">Media Encoder Standard Formats and Codecs</span></span>
<span data-ttu-id="6a506-104">Det här dokumentet innehåller en lista över hello vanligaste importera och exportera filformat som du kan använda med Media Encoder Standard.</span><span class="sxs-lookup"><span data-stu-id="6a506-104">This document contains a list of hello most common import and export file formats that you can use with Media Encoder Standard.</span></span>

## <a name="input-containerfile-formats"></a><span data-ttu-id="6a506-105">Ange behållare/filformat</span><span class="sxs-lookup"><span data-stu-id="6a506-105">Input Container/File Formats</span></span>
| <span data-ttu-id="6a506-106">Filformat (filnamnstillägg)</span><span class="sxs-lookup"><span data-stu-id="6a506-106">File formats (file extensions)</span></span> | <span data-ttu-id="6a506-107">Stöds</span><span class="sxs-lookup"><span data-stu-id="6a506-107">Supported</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="6a506-108">FLV (med H.264 och AAC codec) (.flv)</span><span class="sxs-lookup"><span data-stu-id="6a506-108">FLV (with H.264 and AAC codecs) (.flv)</span></span> |<span data-ttu-id="6a506-109">Ja</span><span class="sxs-lookup"><span data-stu-id="6a506-109">Yes</span></span> |
| <span data-ttu-id="6a506-110">MXF (.mxf)</span><span class="sxs-lookup"><span data-stu-id="6a506-110">MXF    (.mxf)</span></span> |<span data-ttu-id="6a506-111">Ja</span><span class="sxs-lookup"><span data-stu-id="6a506-111">Yes</span></span> |
| <span data-ttu-id="6a506-112">GXF (.gxf)</span><span class="sxs-lookup"><span data-stu-id="6a506-112">GXF    (.gxf)</span></span> |<span data-ttu-id="6a506-113">Ja</span><span class="sxs-lookup"><span data-stu-id="6a506-113">Yes</span></span> |
| <span data-ttu-id="6a506-114">MPEG2-PS, MPEG2 TS 3GP (.ts, PS, .3gp, .3gpp, MPG)</span><span class="sxs-lookup"><span data-stu-id="6a506-114">MPEG2-PS, MPEG2-TS, 3GP (.ts, .ps, .3gp, .3gpp, .mpg)</span></span> |<span data-ttu-id="6a506-115">Ja</span><span class="sxs-lookup"><span data-stu-id="6a506-115">Yes</span></span> |
| <span data-ttu-id="6a506-116">Windows Media Video (WMV) / ASF (.wmv, .asf)</span><span class="sxs-lookup"><span data-stu-id="6a506-116">Windows Media Video (WMV)/ASF (.wmv, .asf)</span></span> |<span data-ttu-id="6a506-117">Ja</span><span class="sxs-lookup"><span data-stu-id="6a506-117">Yes</span></span> |
| <span data-ttu-id="6a506-118">AVI (okomprimerad 8-bitars/10 bitar) (.avi)</span><span class="sxs-lookup"><span data-stu-id="6a506-118">AVI (Uncompressed 8bit/10bit) (.avi)</span></span> |<span data-ttu-id="6a506-119">Ja</span><span class="sxs-lookup"><span data-stu-id="6a506-119">Yes</span></span> |
| <span data-ttu-id="6a506-120">MP4 (.mp4, .m4a, .m4v) / ISMV (.isma, .ismv)</span><span class="sxs-lookup"><span data-stu-id="6a506-120">MP4 (.mp4, .m4a, .m4v)/ISMV (.isma, .ismv)</span></span> |<span data-ttu-id="6a506-121">Ja</span><span class="sxs-lookup"><span data-stu-id="6a506-121">Yes</span></span> |
| <span data-ttu-id="6a506-122">[Microsoft Digital Video Recording(DVR-MS)](https://msdn.microsoft.com/library/windows/desktop/dd692984) (.dvr-ms)</span><span class="sxs-lookup"><span data-stu-id="6a506-122">[Microsoft Digital Video Recording(DVR-MS)](https://msdn.microsoft.com/library/windows/desktop/dd692984) (.dvr-ms)</span></span> |<span data-ttu-id="6a506-123">Ja</span><span class="sxs-lookup"><span data-stu-id="6a506-123">Yes</span></span> |
| <span data-ttu-id="6a506-124">Matroska/WebM (.mkv)</span><span class="sxs-lookup"><span data-stu-id="6a506-124">Matroska/WebM (.mkv)</span></span> |<span data-ttu-id="6a506-125">Ja</span><span class="sxs-lookup"><span data-stu-id="6a506-125">Yes</span></span> |
| <span data-ttu-id="6a506-126">WAVE/WAV (.wav)</span><span class="sxs-lookup"><span data-stu-id="6a506-126">WAVE/WAV (.wav)</span></span> |<span data-ttu-id="6a506-127">Ja</span><span class="sxs-lookup"><span data-stu-id="6a506-127">Yes</span></span> |
| <span data-ttu-id="6a506-128">QuickTime (.mov)</span><span class="sxs-lookup"><span data-stu-id="6a506-128">QuickTime (.mov)</span></span> |<span data-ttu-id="6a506-129">Ja</span><span class="sxs-lookup"><span data-stu-id="6a506-129">Yes</span></span> |

> [!NOTE]
> <span data-ttu-id="6a506-130">Är en lista med filnamnstillägg för hello ofta påträffade ovan.</span><span class="sxs-lookup"><span data-stu-id="6a506-130">Above is a list of hello more commonly encountered file extensions.</span></span> <span data-ttu-id="6a506-131">Media Encoder Standard stöder många andra (till exempel: .m2ts, .mpeg2video, .qt).</span><span class="sxs-lookup"><span data-stu-id="6a506-131">Media Encoder Standard does support many others (for example: .m2ts, .mpeg2video, .qt).</span></span> <span data-ttu-id="6a506-132">Lämna feedback om du försöker tooencode en fil och du får ett felmeddelande om hello-format som inte stöds, [här](https://feedback.azure.com/forums/169396-media-services/category/144411-encoding-and-processing/).</span><span class="sxs-lookup"><span data-stu-id="6a506-132">If you try tooencode a file and you get an error message about hello format not being supported, please provide a feedback [here](https://feedback.azure.com/forums/169396-media-services/category/144411-encoding-and-processing/).</span></span>
> 
> 

### <a name="audio-formats-in-input-containers"></a><span data-ttu-id="6a506-133">Ljud-formaten i inkommande behållare</span><span class="sxs-lookup"><span data-stu-id="6a506-133">Audio formats in input containers</span></span>
<span data-ttu-id="6a506-134">Media Encoder Standard stöder medför hello följande ljud-formaten i inkommande behållare:</span><span class="sxs-lookup"><span data-stu-id="6a506-134">Media Encoder Standard supports carrying hello following audio formats in input containers:</span></span>

* <span data-ttu-id="6a506-135">MXF, GXF och QuickTime filer som har ljud spårar med överlagrad stereo eller 5.1-exempel</span><span class="sxs-lookup"><span data-stu-id="6a506-135">MXF, GXF and QuickTime files which have audio tracks with interleaved stereo or 5.1 samples</span></span>

<span data-ttu-id="6a506-136">eller</span><span class="sxs-lookup"><span data-stu-id="6a506-136">or</span></span>

* <span data-ttu-id="6a506-137">MXF, GXF och QuickTime filer där hello ljud utförs som separata PCM spår men hello kanalmappning (toostereo eller 5.1) härledas från hello filens metadata</span><span class="sxs-lookup"><span data-stu-id="6a506-137">MXF, GXF and QuickTime files where hello audio is carried as separate PCM tracks but hello channel mapping (toostereo or 5.1) can be deduced from hello file metadata</span></span>

<span data-ttu-id="6a506-138">Observera att stödet för explicit/användardefinierat kanalmappning ska anges i hello nära framtiden.</span><span class="sxs-lookup"><span data-stu-id="6a506-138">Note that support for explicit/user-supplied channel mapping will be provided in hello near future.</span></span>

## <a name="input-video-codecs"></a><span data-ttu-id="6a506-139">Inkommande Video-codec</span><span class="sxs-lookup"><span data-stu-id="6a506-139">Input Video Codecs</span></span>
| <span data-ttu-id="6a506-140">Inkommande Video-codec</span><span class="sxs-lookup"><span data-stu-id="6a506-140">Input Video Codecs</span></span> | <span data-ttu-id="6a506-141">Stöds</span><span class="sxs-lookup"><span data-stu-id="6a506-141">Supported</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="6a506-142">AVC 8-/ 10-bitars, in too4:2:2, inklusive AVCIntra</span><span class="sxs-lookup"><span data-stu-id="6a506-142">AVC 8-bit/10-bit, up too4:2:2, including AVCIntra</span></span> |<span data-ttu-id="6a506-143">8-bitars 4:2:0 och 4:2:2</span><span class="sxs-lookup"><span data-stu-id="6a506-143">8 bit 4:2:0 and 4:2:2</span></span> |
| <span data-ttu-id="6a506-144">Avid DNxHD (i MXF)</span><span class="sxs-lookup"><span data-stu-id="6a506-144">Avid DNxHD (in MXF)</span></span> |<span data-ttu-id="6a506-145">Ja</span><span class="sxs-lookup"><span data-stu-id="6a506-145">Yes</span></span> |
| <span data-ttu-id="6a506-146">DVCPro/DVCProHD (i MXF)</span><span class="sxs-lookup"><span data-stu-id="6a506-146">DVCPro/DVCProHD (in MXF)</span></span> |<span data-ttu-id="6a506-147">Ja</span><span class="sxs-lookup"><span data-stu-id="6a506-147">Yes</span></span> |
| <span data-ttu-id="6a506-148">Digital video (DV) (i AVI-filer)</span><span class="sxs-lookup"><span data-stu-id="6a506-148">Digital video (DV) (in AVI files)</span></span> |<span data-ttu-id="6a506-149">Ja</span><span class="sxs-lookup"><span data-stu-id="6a506-149">Yes</span></span> |
| <span data-ttu-id="6a506-150">JPEG 2000</span><span class="sxs-lookup"><span data-stu-id="6a506-150">JPEG 2000</span></span> |<span data-ttu-id="6a506-151">Ja</span><span class="sxs-lookup"><span data-stu-id="6a506-151">Yes</span></span> |
| <span data-ttu-id="6a506-152">MPEG-2 (too422 profil och högnivå, inklusive varianter, till exempel XDCAM, XDCAM HD, XDCAM IMX, CableLabs® och D10)</span><span class="sxs-lookup"><span data-stu-id="6a506-152">MPEG-2 (up too422 Profile and High Level; including variants such as XDCAM, XDCAM HD, XDCAM IMX, CableLabs® and D10)</span></span> |<span data-ttu-id="6a506-153">Too422 profil</span><span class="sxs-lookup"><span data-stu-id="6a506-153">Up too422 Profile</span></span> |
| <span data-ttu-id="6a506-154">MPEG-1</span><span class="sxs-lookup"><span data-stu-id="6a506-154">MPEG-1</span></span> |<span data-ttu-id="6a506-155">Ja</span><span class="sxs-lookup"><span data-stu-id="6a506-155">Yes</span></span> |
| <span data-ttu-id="6a506-156">WMV9-VC-1</span><span class="sxs-lookup"><span data-stu-id="6a506-156">VC-1/WMV9</span></span> |<span data-ttu-id="6a506-157">Ja</span><span class="sxs-lookup"><span data-stu-id="6a506-157">Yes</span></span> |
| <span data-ttu-id="6a506-158">Canopus HQ/HQX</span><span class="sxs-lookup"><span data-stu-id="6a506-158">Canopus HQ/HQX</span></span> |<span data-ttu-id="6a506-159">Nej</span><span class="sxs-lookup"><span data-stu-id="6a506-159">No</span></span> |
| <span data-ttu-id="6a506-160">MPEG-4 del 2</span><span class="sxs-lookup"><span data-stu-id="6a506-160">MPEG-4 Part 2</span></span> |<span data-ttu-id="6a506-161">Ja</span><span class="sxs-lookup"><span data-stu-id="6a506-161">Yes</span></span> |
| [<span data-ttu-id="6a506-162">Theora</span><span class="sxs-lookup"><span data-stu-id="6a506-162">Theora</span></span>](https://en.wikipedia.org/wiki/Theora) |<span data-ttu-id="6a506-163">Ja</span><span class="sxs-lookup"><span data-stu-id="6a506-163">Yes</span></span> |
| <span data-ttu-id="6a506-164">YUV420 okomprimerade eller mezzanine</span><span class="sxs-lookup"><span data-stu-id="6a506-164">YUV420 uncompressed, or mezzanine</span></span> |<span data-ttu-id="6a506-165">Ja</span><span class="sxs-lookup"><span data-stu-id="6a506-165">Yes</span></span> |
| <span data-ttu-id="6a506-166">Apple ProRes 422</span><span class="sxs-lookup"><span data-stu-id="6a506-166">Apple ProRes 422</span></span> |<span data-ttu-id="6a506-167">Ja</span><span class="sxs-lookup"><span data-stu-id="6a506-167">Yes</span></span> |
| <span data-ttu-id="6a506-168">Apple ProRes 422 LT</span><span class="sxs-lookup"><span data-stu-id="6a506-168">Apple ProRes 422 LT</span></span> |<span data-ttu-id="6a506-169">Ja</span><span class="sxs-lookup"><span data-stu-id="6a506-169">Yes</span></span> |
| <span data-ttu-id="6a506-170">Apple ProRes 422 HQ</span><span class="sxs-lookup"><span data-stu-id="6a506-170">Apple ProRes 422 HQ</span></span> |<span data-ttu-id="6a506-171">Ja</span><span class="sxs-lookup"><span data-stu-id="6a506-171">Yes</span></span> |
| <span data-ttu-id="6a506-172">Apple ProRes Proxy</span><span class="sxs-lookup"><span data-stu-id="6a506-172">Apple ProRes Proxy</span></span> |<span data-ttu-id="6a506-173">Ja</span><span class="sxs-lookup"><span data-stu-id="6a506-173">Yes</span></span> |
| <span data-ttu-id="6a506-174">Apple ProRes 4444</span><span class="sxs-lookup"><span data-stu-id="6a506-174">Apple ProRes 4444</span></span> |<span data-ttu-id="6a506-175">Ja</span><span class="sxs-lookup"><span data-stu-id="6a506-175">Yes</span></span> |
| <span data-ttu-id="6a506-176">Apple ProRes 4444 XQ</span><span class="sxs-lookup"><span data-stu-id="6a506-176">Apple ProRes 4444 XQ</span></span> |<span data-ttu-id="6a506-177">Ja</span><span class="sxs-lookup"><span data-stu-id="6a506-177">Yes</span></span> |

## <a name="input-audio-codecs"></a><span data-ttu-id="6a506-178">Inkommande ljud-codec</span><span class="sxs-lookup"><span data-stu-id="6a506-178">Input Audio Codecs</span></span>
| <span data-ttu-id="6a506-179">Inkommande ljud-codec</span><span class="sxs-lookup"><span data-stu-id="6a506-179">Input Audio Codecs</span></span> | <span data-ttu-id="6a506-180">Stöds</span><span class="sxs-lookup"><span data-stu-id="6a506-180">Supported</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="6a506-181">AAC (AAC LC, HE AAC och AAC-HEv2 in too5.1)</span><span class="sxs-lookup"><span data-stu-id="6a506-181">AAC (AAC-LC, AAC-HE, and AAC-HEv2; up too5.1)</span></span> |<span data-ttu-id="6a506-182">Ja</span><span class="sxs-lookup"><span data-stu-id="6a506-182">Yes</span></span> |
| <span data-ttu-id="6a506-183">MPEG nivå 2</span><span class="sxs-lookup"><span data-stu-id="6a506-183">MPEG Layer 2</span></span> |<span data-ttu-id="6a506-184">Ja</span><span class="sxs-lookup"><span data-stu-id="6a506-184">Yes</span></span> |
| <span data-ttu-id="6a506-185">MP3 (MPEG-1 ljud nivå 3)</span><span class="sxs-lookup"><span data-stu-id="6a506-185">MP3 (MPEG-1 Audio Layer 3)</span></span> |<span data-ttu-id="6a506-186">Ja</span><span class="sxs-lookup"><span data-stu-id="6a506-186">Yes</span></span> |
| <span data-ttu-id="6a506-187">Windows Media Audio</span><span class="sxs-lookup"><span data-stu-id="6a506-187">Windows Media Audio</span></span> |<span data-ttu-id="6a506-188">Ja</span><span class="sxs-lookup"><span data-stu-id="6a506-188">Yes</span></span> |
| <span data-ttu-id="6a506-189">WAV PCM /</span><span class="sxs-lookup"><span data-stu-id="6a506-189">WAV/PCM</span></span> |<span data-ttu-id="6a506-190">Ja</span><span class="sxs-lookup"><span data-stu-id="6a506-190">Yes</span></span> |
| <span data-ttu-id="6a506-191">[FLAC](https://en.wikipedia.org/wiki/FLAC)</a></span><span class="sxs-lookup"><span data-stu-id="6a506-191">[FLAC](https://en.wikipedia.org/wiki/FLAC)</a></span></span> |<span data-ttu-id="6a506-192">Ja</span><span class="sxs-lookup"><span data-stu-id="6a506-192">Yes</span></span> |
| [<span data-ttu-id="6a506-193">Opus</span><span class="sxs-lookup"><span data-stu-id="6a506-193">Opus</span></span>](http://go.microsoft.com/fwlink/?LinkId=822667) |<span data-ttu-id="6a506-194">Ja</span><span class="sxs-lookup"><span data-stu-id="6a506-194">Yes</span></span> |
| <span data-ttu-id="6a506-195">[Vorbis](https://en.wikipedia.org/wiki/Vorbis)</a></span><span class="sxs-lookup"><span data-stu-id="6a506-195">[Vorbis](https://en.wikipedia.org/wiki/Vorbis)</a></span></span> |<span data-ttu-id="6a506-196">Ja</span><span class="sxs-lookup"><span data-stu-id="6a506-196">Yes</span></span> |
| <span data-ttu-id="6a506-197">AMR (anpassningsbar flera rate)</span><span class="sxs-lookup"><span data-stu-id="6a506-197">AMR (adaptive multi-rate)</span></span> |<span data-ttu-id="6a506-198">Ja</span><span class="sxs-lookup"><span data-stu-id="6a506-198">Yes</span></span> |
| <span data-ttu-id="6a506-199">AES (SMPTE 331 M och 302 M, AES3 2003)</span><span class="sxs-lookup"><span data-stu-id="6a506-199">AES (SMPTE 331M and 302M, AES3-2003)</span></span> |<span data-ttu-id="6a506-200">Nej</span><span class="sxs-lookup"><span data-stu-id="6a506-200">No</span></span> |
| <span data-ttu-id="6a506-201">Dolby® E</span><span class="sxs-lookup"><span data-stu-id="6a506-201">Dolby® E</span></span> |<span data-ttu-id="6a506-202">Nej</span><span class="sxs-lookup"><span data-stu-id="6a506-202">No</span></span> |
| <span data-ttu-id="6a506-203">Dolby® Digital (AC3)</span><span class="sxs-lookup"><span data-stu-id="6a506-203">Dolby® Digital (AC3)</span></span> |<span data-ttu-id="6a506-204">Nej</span><span class="sxs-lookup"><span data-stu-id="6a506-204">No</span></span> |
| <span data-ttu-id="6a506-205">Dolby® Digital Plus (E-AC3)</span><span class="sxs-lookup"><span data-stu-id="6a506-205">Dolby® Digital Plus (E-AC3)</span></span> |<span data-ttu-id="6a506-206">Nej</span><span class="sxs-lookup"><span data-stu-id="6a506-206">No</span></span> |

## <a name="output-formats-and-codecs"></a><span data-ttu-id="6a506-207">Output-format och codec</span><span class="sxs-lookup"><span data-stu-id="6a506-207">Output Formats and codecs</span></span>
<span data-ttu-id="6a506-208">hello i den följande tabellen listas hello codec och filformat som stöds för export.</span><span class="sxs-lookup"><span data-stu-id="6a506-208">hello following table lists hello codecs and file formats that are supported for export.</span></span>

| <span data-ttu-id="6a506-209">Filformat</span><span class="sxs-lookup"><span data-stu-id="6a506-209">File Format</span></span> | <span data-ttu-id="6a506-210">Video-Codec</span><span class="sxs-lookup"><span data-stu-id="6a506-210">Video Codec</span></span> | <span data-ttu-id="6a506-211">Ljud-Codec</span><span class="sxs-lookup"><span data-stu-id="6a506-211">Audio Codec</span></span> |
| --- | --- | --- |
| <span data-ttu-id="6a506-212">MP4</span><span class="sxs-lookup"><span data-stu-id="6a506-212">MP4</span></span> <br/><br/><span data-ttu-id="6a506-213">(inklusive behållare i flera bithastigheter MP4)</span><span class="sxs-lookup"><span data-stu-id="6a506-213">(including multi-bitrate MP4 containers)</span></span> |<span data-ttu-id="6a506-214">H.264 (hög Main och baslinjen profiler)</span><span class="sxs-lookup"><span data-stu-id="6a506-214">H.264 (High, Main, and Baseline Profiles)</span></span> |<span data-ttu-id="6a506-215">AAC-LC, HE-AAC v1, HE-AAC v2</span><span class="sxs-lookup"><span data-stu-id="6a506-215">AAC-LC, HE-AAC v1, HE-AAC v2</span></span> |
| <span data-ttu-id="6a506-216">MPEG2 TS</span><span class="sxs-lookup"><span data-stu-id="6a506-216">MPEG2-TS</span></span> |<span data-ttu-id="6a506-217">H.264 (hög Main och baslinjen profiler)</span><span class="sxs-lookup"><span data-stu-id="6a506-217">H.264 (High, Main, and Baseline Profiles)</span></span> |<span data-ttu-id="6a506-218">AAC-LC, HE-AAC v1, HE-AAC v2</span><span class="sxs-lookup"><span data-stu-id="6a506-218">AAC-LC, HE-AAC v1, HE-AAC v2</span></span> |

## <a name="media-services-learning-paths"></a><span data-ttu-id="6a506-219">Sökvägar för Media Services-utbildning</span><span class="sxs-lookup"><span data-stu-id="6a506-219">Media Services learning paths</span></span>
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a><span data-ttu-id="6a506-220">Ge feedback</span><span class="sxs-lookup"><span data-stu-id="6a506-220">Provide feedback</span></span>
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

## <a name="see-also"></a><span data-ttu-id="6a506-221">Se även</span><span class="sxs-lookup"><span data-stu-id="6a506-221">See also</span></span>
[<span data-ttu-id="6a506-222">Koda innehåll på begäran med Azure Media Services</span><span class="sxs-lookup"><span data-stu-id="6a506-222">Encoding On-Demand Content with Azure Media Services</span></span>](media-services-encode-asset.md)

[<span data-ttu-id="6a506-223">Hur tooencode med Media Encoder Standard</span><span class="sxs-lookup"><span data-stu-id="6a506-223">How tooencode with Media Encoder Standard</span></span>](media-services-dotnet-encode-with-media-encoder-standard.md)

