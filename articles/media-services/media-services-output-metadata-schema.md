---
title: aaaAzure Media Services metadata utdataschema | Microsoft Docs
description: "hello avsnittet ger en översikt över Azure Media Services utdataschema metadata."
author: Juliako
manager: cfowler
editor: 
services: media-services
documentationcenter: 
ms.assetid: 1ed84c88-eea5-4a24-9c4f-f2428157d08a
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/19/2017
ms.author: juliako
ms.openlocfilehash: 7f07d6accbe0b171d0408b15d5e1e6b5afd6c367
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="output-metadata"></a><span data-ttu-id="74e52-103">Metadata för utdata</span><span class="sxs-lookup"><span data-stu-id="74e52-103">Output Metadata</span></span>
## <a name="overview"></a><span data-ttu-id="74e52-104">Översikt</span><span class="sxs-lookup"><span data-stu-id="74e52-104">Overview</span></span>
<span data-ttu-id="74e52-105">Ett kodningsjobb är associerad med en inkommande tillgång (eller tillgångar) som du vill tooperform vissa kodning uppgifter.</span><span class="sxs-lookup"><span data-stu-id="74e52-105">An encoding job is associated with an input asset (or assets) on which you want tooperform some encoding tasks.</span></span> <span data-ttu-id="74e52-106">Koda till exempel en MP4-fil tooH.264 MP4 med anpassad bithastighet anger; Skapa en miniatyr; Skapa överlägg.</span><span class="sxs-lookup"><span data-stu-id="74e52-106">For example, encode an MP4 file tooH.264 MP4 adaptive bitrate sets; create a thumbnail; create overlays.</span></span> <span data-ttu-id="74e52-107">En utdatatillgången skapas vid slutförandet av uppgiften.</span><span class="sxs-lookup"><span data-stu-id="74e52-107">Upon completion of a task, an output asset is produced.</span></span>  <span data-ttu-id="74e52-108">Hej utdatatillgången innehåller video, ljud, miniatyrer, etc. hello utdatatillgången innehåller också en fil med metadata om hello utdatatillgången.</span><span class="sxs-lookup"><span data-stu-id="74e52-108">hello output asset contains video, audio, thumbnails, etc. hello output asset also contains a file with metadata about hello output asset.</span></span> <span data-ttu-id="74e52-109">hello namnet på XML-filen för hello metadata har hello följande format: &lt;source_file_name&gt;_manifest.xml (till exempel BigBuckBunny_manifest.xml).</span><span class="sxs-lookup"><span data-stu-id="74e52-109">hello name of hello metadata XML file has hello following format: &lt;source_file_name&gt;_manifest.xml (for example, BigBuckBunny_manifest.xml).</span></span>  

<span data-ttu-id="74e52-110">Om du vill tooexamine hello metadatafil, kan du skapa en **SAS** positionerare och hämta hello filen tooyour lokal dator.</span><span class="sxs-lookup"><span data-stu-id="74e52-110">If you want tooexamine hello metadata file, you can create a **SAS** locator and download hello file tooyour local computer.</span></span>  

<span data-ttu-id="74e52-111">Det här avsnittet beskrivs hello element och typer av hello XML-schemat på vilka hello utdata metada (&lt;source_file_name&gt;_manifest.xml) är baserad.</span><span class="sxs-lookup"><span data-stu-id="74e52-111">This topic discusses hello elements and types of hello XML schema on which hello output metada (&lt;source_file_name&gt;_manifest.xml) is based.</span></span> <span data-ttu-id="74e52-112">Information om hello-fil som innehåller metadata om hello inkommande tillgångsinformation finns [inkommande Metadata](media-services-input-metadata-schema.md).</span><span class="sxs-lookup"><span data-stu-id="74e52-112">For information about hello file that contains metadata about hello input asset, see [Input Metadata](media-services-input-metadata-schema.md).</span></span>  

> [!NOTE]
> <span data-ttu-id="74e52-113">Du kan hitta hello fullständigt schema kod och XML-exempel hello slutet av det här avsnittet.</span><span class="sxs-lookup"><span data-stu-id="74e52-113">You can find hello complete schema code and XML example at hello end of this topic.</span></span>  
>
>

## <span data-ttu-id="74e52-114"><a name="AssetFiles "></a>Rotelementet för AssetFiles</span><span class="sxs-lookup"><span data-stu-id="74e52-114"><a name="AssetFiles "></a> AssetFiles root element</span></span>
<span data-ttu-id="74e52-115">AssetFile poster för hello kodningsjobbet samling.</span><span class="sxs-lookup"><span data-stu-id="74e52-115">Collection of AssetFile entries for hello encoding job.</span></span>  

### <a name="child-elements"></a><span data-ttu-id="74e52-116">Underordnade element</span><span class="sxs-lookup"><span data-stu-id="74e52-116">Child elements</span></span>
| <span data-ttu-id="74e52-117">Namn</span><span class="sxs-lookup"><span data-stu-id="74e52-117">Name</span></span> | <span data-ttu-id="74e52-118">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="74e52-118">Description</span></span> |
| --- | --- |
| <span data-ttu-id="74e52-119">**AssetFile**</span><span class="sxs-lookup"><span data-stu-id="74e52-119">**AssetFile**</span></span><br/><br/> <span data-ttu-id="74e52-120">minOccurs = ”0” maxOccurs = ”1”</span><span class="sxs-lookup"><span data-stu-id="74e52-120">minOccurs="0" maxOccurs="1"</span></span> |<span data-ttu-id="74e52-121">En [AssetFile elementet](media-services-output-metadata-schema.md) som ingår i hello AssetFiles samling.</span><span class="sxs-lookup"><span data-stu-id="74e52-121">An [AssetFile element](media-services-output-metadata-schema.md) that is part of hello AssetFiles collection.</span></span> |

## <span data-ttu-id="74e52-122"><a name="AssetFile "></a>AssetFile element</span><span class="sxs-lookup"><span data-stu-id="74e52-122"><a name="AssetFile "></a> AssetFile element</span></span>
<span data-ttu-id="74e52-123">Du hittar ett XML-exempel [XML-exempel](media-services-output-metadata-schema.md#xml).</span><span class="sxs-lookup"><span data-stu-id="74e52-123">You can find an XML example [XML example](media-services-output-metadata-schema.md#xml).</span></span>  

### <a name="attributes"></a><span data-ttu-id="74e52-124">Attribut</span><span class="sxs-lookup"><span data-stu-id="74e52-124">Attributes</span></span>
| <span data-ttu-id="74e52-125">Namn</span><span class="sxs-lookup"><span data-stu-id="74e52-125">Name</span></span> | <span data-ttu-id="74e52-126">Typ</span><span class="sxs-lookup"><span data-stu-id="74e52-126">Type</span></span> | <span data-ttu-id="74e52-127">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="74e52-127">Description</span></span> |
| --- | --- | --- |
| <span data-ttu-id="74e52-128">**Namn**</span><span class="sxs-lookup"><span data-stu-id="74e52-128">**Name**</span></span><br/><br/> <span data-ttu-id="74e52-129">Krävs</span><span class="sxs-lookup"><span data-stu-id="74e52-129">Required</span></span> |<span data-ttu-id="74e52-130">**Xs:String**</span><span class="sxs-lookup"><span data-stu-id="74e52-130">**xs:string**</span></span> |<span data-ttu-id="74e52-131">filnamnet för hello media tillgången.</span><span class="sxs-lookup"><span data-stu-id="74e52-131">hello media asset file name.</span></span> |
| <span data-ttu-id="74e52-132">**Storlek**</span><span class="sxs-lookup"><span data-stu-id="74e52-132">**Size**</span></span><br/><br/> <span data-ttu-id="74e52-133">minInclusive = ”0”</span><span class="sxs-lookup"><span data-stu-id="74e52-133">minInclusive ="0"</span></span><br/><br/> <span data-ttu-id="74e52-134">Krävs</span><span class="sxs-lookup"><span data-stu-id="74e52-134">Required</span></span> |<span data-ttu-id="74e52-135">**Xs:Long**</span><span class="sxs-lookup"><span data-stu-id="74e52-135">**xs:long**</span></span> |<span data-ttu-id="74e52-136">Storleken på hello tillgångsfil i byte.</span><span class="sxs-lookup"><span data-stu-id="74e52-136">Size of hello asset file in bytes.</span></span> |
| <span data-ttu-id="74e52-137">**Varaktighet**</span><span class="sxs-lookup"><span data-stu-id="74e52-137">**Duration**</span></span><br/><br/> <span data-ttu-id="74e52-138">Krävs</span><span class="sxs-lookup"><span data-stu-id="74e52-138">Required</span></span> |<span data-ttu-id="74e52-139">**Duration**</span><span class="sxs-lookup"><span data-stu-id="74e52-139">**xs:duration**</span></span> |<span data-ttu-id="74e52-140">Innehåll play tillbaka varaktighet.</span><span class="sxs-lookup"><span data-stu-id="74e52-140">Content play back duration.</span></span> |

### <a name="child-elements"></a><span data-ttu-id="74e52-141">Underordnade element</span><span class="sxs-lookup"><span data-stu-id="74e52-141">Child elements</span></span>
| <span data-ttu-id="74e52-142">Namn</span><span class="sxs-lookup"><span data-stu-id="74e52-142">Name</span></span> | <span data-ttu-id="74e52-143">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="74e52-143">Description</span></span> |
| --- | --- |
| <span data-ttu-id="74e52-144">**Datakällor**</span><span class="sxs-lookup"><span data-stu-id="74e52-144">**Sources**</span></span> |<span data-ttu-id="74e52-145">Samling av indatakälla/mediefiler som bearbetades i ordning tooproduce denna AssetFile.</span><span class="sxs-lookup"><span data-stu-id="74e52-145">Collection of input/source media files, that was processed in order tooproduce this AssetFile.</span></span> <span data-ttu-id="74e52-146">Mer information finns i [källelementet](media-services-output-metadata-schema.md).</span><span class="sxs-lookup"><span data-stu-id="74e52-146">For more information, see [Source element](media-services-output-metadata-schema.md).</span></span> |
| <span data-ttu-id="74e52-147">**VideoTracks**</span><span class="sxs-lookup"><span data-stu-id="74e52-147">**VideoTracks**</span></span><br/><br/> <span data-ttu-id="74e52-148">minOccurs = ”0” maxOccurs = ”1”</span><span class="sxs-lookup"><span data-stu-id="74e52-148">minOccurs="0" maxOccurs="1"</span></span> |<span data-ttu-id="74e52-149">Varje fysisk AssetFile kan innehålla noll eller flera video spårar överlagrat till en lämplig behållare format i den.</span><span class="sxs-lookup"><span data-stu-id="74e52-149">Each physical AssetFile can contain in it zero or more video tracks interleaved into an appropriate container format.</span></span> <span data-ttu-id="74e52-150">Detta är de video spårar hello samling.</span><span class="sxs-lookup"><span data-stu-id="74e52-150">This is hello collection of all those video tracks.</span></span> <span data-ttu-id="74e52-151">Mer information finns i [VideoTracks elementet](media-services-output-metadata-schema.md).</span><span class="sxs-lookup"><span data-stu-id="74e52-151">For more information, see [VideoTracks element](media-services-output-metadata-schema.md).</span></span> |
| <span data-ttu-id="74e52-152">**AudioTracks**</span><span class="sxs-lookup"><span data-stu-id="74e52-152">**AudioTracks**</span></span><br/><br/> <span data-ttu-id="74e52-153">minOccurs = ”0” maxOccurs = ”1”</span><span class="sxs-lookup"><span data-stu-id="74e52-153">minOccurs="0" maxOccurs="1"</span></span> |<span data-ttu-id="74e52-154">Varje fysisk AssetFile kan innehålla noll eller flera ljud spårar överlagrat till en lämplig behållare format i den.</span><span class="sxs-lookup"><span data-stu-id="74e52-154">Each physical AssetFile can contain in it zero or more audio tracks interleaved into an appropriate container format.</span></span> <span data-ttu-id="74e52-155">Detta är de ljud spårar hello samling.</span><span class="sxs-lookup"><span data-stu-id="74e52-155">This is hello collection of all those audio tracks.</span></span> <span data-ttu-id="74e52-156">Mer information finns i [AudioTracks elementet](media-services-output-metadata-schema.md).</span><span class="sxs-lookup"><span data-stu-id="74e52-156">For more information, see [AudioTracks element](media-services-output-metadata-schema.md).</span></span> |

## <span data-ttu-id="74e52-157"><a name="Sources "></a>Källor element</span><span class="sxs-lookup"><span data-stu-id="74e52-157"><a name="Sources "></a> Sources element</span></span>
<span data-ttu-id="74e52-158">Samling av indatakälla/mediefiler som bearbetades i ordning tooproduce denna AssetFile.</span><span class="sxs-lookup"><span data-stu-id="74e52-158">Collection of input/source media files, that was processed in order tooproduce this AssetFile.</span></span>  

<span data-ttu-id="74e52-159">Du hittar ett XML-exempel [XML-exempel](media-services-output-metadata-schema.md#xml).</span><span class="sxs-lookup"><span data-stu-id="74e52-159">You can find an XML example [XML example](media-services-output-metadata-schema.md#xml).</span></span>  

### <a name="child-elements"></a><span data-ttu-id="74e52-160">Underordnade element</span><span class="sxs-lookup"><span data-stu-id="74e52-160">Child elements</span></span>
| <span data-ttu-id="74e52-161">Namn</span><span class="sxs-lookup"><span data-stu-id="74e52-161">Name</span></span> | <span data-ttu-id="74e52-162">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="74e52-162">Description</span></span> |
| --- | --- |
| <span data-ttu-id="74e52-163">**Källa**</span><span class="sxs-lookup"><span data-stu-id="74e52-163">**Source**</span></span><br/><br/> <span data-ttu-id="74e52-164">minOccurs = ”1” maxOccurs = ”unbounded”</span><span class="sxs-lookup"><span data-stu-id="74e52-164">minOccurs="1" maxOccurs="unbounded"</span></span> |<span data-ttu-id="74e52-165">En indata/källfilerna som används vid generering av tillgången.</span><span class="sxs-lookup"><span data-stu-id="74e52-165">An input/source file used when generating this asset.</span></span> <span data-ttu-id="74e52-166">Mer information finns i [källelementet](media-services-output-metadata-schema.md).</span><span class="sxs-lookup"><span data-stu-id="74e52-166">For more information see [Source element](media-services-output-metadata-schema.md).</span></span> |

## <span data-ttu-id="74e52-167"><a name="Source "></a>Källelementet</span><span class="sxs-lookup"><span data-stu-id="74e52-167"><a name="Source "></a> Source element</span></span>
<span data-ttu-id="74e52-168">En indata/källfilerna som används vid generering av tillgången.</span><span class="sxs-lookup"><span data-stu-id="74e52-168">An input/source file used when generating this asset.</span></span>  

<span data-ttu-id="74e52-169">Du hittar ett XML-exempel [XML-exempel](media-services-output-metadata-schema.md#xml).</span><span class="sxs-lookup"><span data-stu-id="74e52-169">You can find an XML example [XML example](media-services-output-metadata-schema.md#xml).</span></span>  

### <a name="attributes"></a><span data-ttu-id="74e52-170">Attribut</span><span class="sxs-lookup"><span data-stu-id="74e52-170">Attributes</span></span>
| <span data-ttu-id="74e52-171">Namn</span><span class="sxs-lookup"><span data-stu-id="74e52-171">Name</span></span> | <span data-ttu-id="74e52-172">Typ</span><span class="sxs-lookup"><span data-stu-id="74e52-172">Type</span></span> | <span data-ttu-id="74e52-173">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="74e52-173">Description</span></span> |
| --- | --- | --- |
| <span data-ttu-id="74e52-174">**Namn**</span><span class="sxs-lookup"><span data-stu-id="74e52-174">**Name**</span></span><br/><br/> <span data-ttu-id="74e52-175">Krävs</span><span class="sxs-lookup"><span data-stu-id="74e52-175">Required</span></span> |<span data-ttu-id="74e52-176">**Xs:String**</span><span class="sxs-lookup"><span data-stu-id="74e52-176">**xs:string**</span></span> |<span data-ttu-id="74e52-177">Indatakällan filnamn.</span><span class="sxs-lookup"><span data-stu-id="74e52-177">Input source file name.</span></span> |

## <span data-ttu-id="74e52-178"><a name="VideoTracks "></a>VideoTracks element</span><span class="sxs-lookup"><span data-stu-id="74e52-178"><a name="VideoTracks "></a> VideoTracks element</span></span>
<span data-ttu-id="74e52-179">Varje fysisk AssetFile kan innehålla noll eller flera video spårar överlagrat till en lämplig behållare format i den.</span><span class="sxs-lookup"><span data-stu-id="74e52-179">Each physical AssetFile can contain in it zero or more video tracks interleaved into an appropriate container format.</span></span> <span data-ttu-id="74e52-180">Detta är de video spårar hello samling.</span><span class="sxs-lookup"><span data-stu-id="74e52-180">This is hello collection of all those video tracks.</span></span>  

<span data-ttu-id="74e52-181">Du hittar ett XML-exempel [XML-exempel](media-services-output-metadata-schema.md#xml).</span><span class="sxs-lookup"><span data-stu-id="74e52-181">You can find an XML example [XML example](media-services-output-metadata-schema.md#xml).</span></span>  

### <a name="child-elements"></a><span data-ttu-id="74e52-182">Underordnade element</span><span class="sxs-lookup"><span data-stu-id="74e52-182">Child elements</span></span>
| <span data-ttu-id="74e52-183">Namn</span><span class="sxs-lookup"><span data-stu-id="74e52-183">Name</span></span> | <span data-ttu-id="74e52-184">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="74e52-184">Description</span></span> |
| --- | --- |
| <span data-ttu-id="74e52-185">**VideoTrack**</span><span class="sxs-lookup"><span data-stu-id="74e52-185">**VideoTrack**</span></span><br/><br/> <span data-ttu-id="74e52-186">minOccurs = ”1” maxOccurs = ”unbounded”</span><span class="sxs-lookup"><span data-stu-id="74e52-186">minOccurs="1" maxOccurs="unbounded"</span></span> |<span data-ttu-id="74e52-187">En specifik video spåra hello överordnade AssetFile.</span><span class="sxs-lookup"><span data-stu-id="74e52-187">A specific video track in hello parent AssetFile.</span></span> <span data-ttu-id="74e52-188">Mer information finns i [VideoTrack elementet](media-services-output-metadata-schema.md#VideoTrack).</span><span class="sxs-lookup"><span data-stu-id="74e52-188">For more information, see [VideoTrack element](media-services-output-metadata-schema.md#VideoTrack).</span></span> |

## <span data-ttu-id="74e52-189"><a name="VideoTrack"></a>VideoTrack element</span><span class="sxs-lookup"><span data-stu-id="74e52-189"><a name="VideoTrack"></a> VideoTrack element</span></span>
<span data-ttu-id="74e52-190">En specifik video spåra hello överordnade AssetFile.</span><span class="sxs-lookup"><span data-stu-id="74e52-190">A specific video track in hello parent AssetFile.</span></span>  

<span data-ttu-id="74e52-191">Du hittar ett XML-exempel [XML-exempel](media-services-output-metadata-schema.md#xml).</span><span class="sxs-lookup"><span data-stu-id="74e52-191">You can find an XML example [XML example](media-services-output-metadata-schema.md#xml).</span></span>  

### <a name="attributes"></a><span data-ttu-id="74e52-192">Attribut</span><span class="sxs-lookup"><span data-stu-id="74e52-192">Attributes</span></span>
| <span data-ttu-id="74e52-193">Namn</span><span class="sxs-lookup"><span data-stu-id="74e52-193">Name</span></span> | <span data-ttu-id="74e52-194">Typ</span><span class="sxs-lookup"><span data-stu-id="74e52-194">Type</span></span> | <span data-ttu-id="74e52-195">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="74e52-195">Description</span></span> |
| --- | --- | --- |
| <span data-ttu-id="74e52-196">**ID**</span><span class="sxs-lookup"><span data-stu-id="74e52-196">**Id**</span></span><br/><br/> <span data-ttu-id="74e52-197">minInclusive = ”0”</span><span class="sxs-lookup"><span data-stu-id="74e52-197">minInclusive ="0"</span></span><br/><br/> <span data-ttu-id="74e52-198">Krävs</span><span class="sxs-lookup"><span data-stu-id="74e52-198">Required</span></span> |<span data-ttu-id="74e52-199">**Xs:int**</span><span class="sxs-lookup"><span data-stu-id="74e52-199">**xs:int**</span></span> |<span data-ttu-id="74e52-200">Nollbaserade indexet för den här videon spåra. **Obs:** detta är inte nödvändigtvis hello TrackID som används i en MP4-fil.</span><span class="sxs-lookup"><span data-stu-id="74e52-200">Zero-based index of this video track. **Note:**  This is not necessarily hello TrackID as used in an MP4 file.</span></span> |
| <span data-ttu-id="74e52-201">**FourCC**</span><span class="sxs-lookup"><span data-stu-id="74e52-201">**FourCC**</span></span><br/><br/> <span data-ttu-id="74e52-202">Krävs</span><span class="sxs-lookup"><span data-stu-id="74e52-202">Required</span></span> |<span data-ttu-id="74e52-203">**Xs:String**</span><span class="sxs-lookup"><span data-stu-id="74e52-203">**xs:string**</span></span> |<span data-ttu-id="74e52-204">Video-codec FourCC-kod.</span><span class="sxs-lookup"><span data-stu-id="74e52-204">Video codec FourCC code.</span></span> |
| <span data-ttu-id="74e52-205">**Profil**</span><span class="sxs-lookup"><span data-stu-id="74e52-205">**Profile**</span></span> |<span data-ttu-id="74e52-206">**Xs:String**</span><span class="sxs-lookup"><span data-stu-id="74e52-206">**xs:string**</span></span> |<span data-ttu-id="74e52-207">H264 profilen (endast tillämpliga tooH264 codec).</span><span class="sxs-lookup"><span data-stu-id="74e52-207">H264 profile (only applicable tooH264 codec).</span></span> |
| <span data-ttu-id="74e52-208">**Nivå**</span><span class="sxs-lookup"><span data-stu-id="74e52-208">**Level**</span></span> |<span data-ttu-id="74e52-209">**Xs:String**</span><span class="sxs-lookup"><span data-stu-id="74e52-209">**xs:string**</span></span> |<span data-ttu-id="74e52-210">H264 nivå (endast tillämpliga tooH264 codec).</span><span class="sxs-lookup"><span data-stu-id="74e52-210">H264 level (only applicable tooH264 codec).</span></span> |
| <span data-ttu-id="74e52-211">**Bredd**</span><span class="sxs-lookup"><span data-stu-id="74e52-211">**Width**</span></span><br/><br/> <span data-ttu-id="74e52-212">minInclusive = ”0”</span><span class="sxs-lookup"><span data-stu-id="74e52-212">minInclusive ="0"</span></span><br/><br/> <span data-ttu-id="74e52-213">Krävs</span><span class="sxs-lookup"><span data-stu-id="74e52-213">Required</span></span> |<span data-ttu-id="74e52-214">**Xs:int**</span><span class="sxs-lookup"><span data-stu-id="74e52-214">**xs:int**</span></span> |<span data-ttu-id="74e52-215">Kodad video bredd i bildpunkter.</span><span class="sxs-lookup"><span data-stu-id="74e52-215">Encoded video width in pixels.</span></span> |
| <span data-ttu-id="74e52-216">**Höjd**</span><span class="sxs-lookup"><span data-stu-id="74e52-216">**Height**</span></span><br/><br/> <span data-ttu-id="74e52-217">minInclusive = ”0”</span><span class="sxs-lookup"><span data-stu-id="74e52-217">minInclusive ="0"</span></span><br/><br/> <span data-ttu-id="74e52-218">Krävs</span><span class="sxs-lookup"><span data-stu-id="74e52-218">Required</span></span> |<span data-ttu-id="74e52-219">**Xs:int**</span><span class="sxs-lookup"><span data-stu-id="74e52-219">**xs:int**</span></span> |<span data-ttu-id="74e52-220">Kodad video höjd i bildpunkter.</span><span class="sxs-lookup"><span data-stu-id="74e52-220">Encoded video height in pixels.</span></span> |
| <span data-ttu-id="74e52-221">**DisplayAspectRatioNumerator**</span><span class="sxs-lookup"><span data-stu-id="74e52-221">**DisplayAspectRatioNumerator**</span></span><br/><br/> <span data-ttu-id="74e52-222">minInclusive = ”0”</span><span class="sxs-lookup"><span data-stu-id="74e52-222">minInclusive ="0"</span></span><br/><br/> <span data-ttu-id="74e52-223">Krävs</span><span class="sxs-lookup"><span data-stu-id="74e52-223">Required</span></span> |<span data-ttu-id="74e52-224">**Xs:Double**</span><span class="sxs-lookup"><span data-stu-id="74e52-224">**xs:double**</span></span> |<span data-ttu-id="74e52-225">Videoenhet proportionerna täljaren.</span><span class="sxs-lookup"><span data-stu-id="74e52-225">Video display aspect ratio numerator.</span></span> |
| <span data-ttu-id="74e52-226">**DisplayAspectRatioDenominator**</span><span class="sxs-lookup"><span data-stu-id="74e52-226">**DisplayAspectRatioDenominator**</span></span><br/><br/> <span data-ttu-id="74e52-227">minInclusive = ”0”</span><span class="sxs-lookup"><span data-stu-id="74e52-227">minInclusive ="0"</span></span><br/><br/> <span data-ttu-id="74e52-228">Krävs</span><span class="sxs-lookup"><span data-stu-id="74e52-228">Required</span></span> |<span data-ttu-id="74e52-229">**Xs:Double**</span><span class="sxs-lookup"><span data-stu-id="74e52-229">**xs:double**</span></span> |<span data-ttu-id="74e52-230">Videoenhet proportionerna nämnare.</span><span class="sxs-lookup"><span data-stu-id="74e52-230">Video display aspect ratio denominator.</span></span> |
| <span data-ttu-id="74e52-231">**Ramhastighet**</span><span class="sxs-lookup"><span data-stu-id="74e52-231">**Framerate**</span></span><br/><br/> <span data-ttu-id="74e52-232">minInclusive = ”0”</span><span class="sxs-lookup"><span data-stu-id="74e52-232">minInclusive ="0"</span></span><br/><br/> <span data-ttu-id="74e52-233">Krävs</span><span class="sxs-lookup"><span data-stu-id="74e52-233">Required</span></span> |<span data-ttu-id="74e52-234">**Xs:decimal**</span><span class="sxs-lookup"><span data-stu-id="74e52-234">**xs:decimal**</span></span> |<span data-ttu-id="74e52-235">Mätt video bildfrekvens .3f format.</span><span class="sxs-lookup"><span data-stu-id="74e52-235">Measured video frame rate in .3f format.</span></span> |
| <span data-ttu-id="74e52-236">**TargetFramerate**</span><span class="sxs-lookup"><span data-stu-id="74e52-236">**TargetFramerate**</span></span><br/><br/> <span data-ttu-id="74e52-237">minInclusive = ”0”</span><span class="sxs-lookup"><span data-stu-id="74e52-237">minInclusive ="0"</span></span><br/><br/> <span data-ttu-id="74e52-238">Krävs</span><span class="sxs-lookup"><span data-stu-id="74e52-238">Required</span></span> |<span data-ttu-id="74e52-239">**Xs:decimal**</span><span class="sxs-lookup"><span data-stu-id="74e52-239">**xs:decimal**</span></span> |<span data-ttu-id="74e52-240">Förinställda mål video bildfrekvens .3f format.</span><span class="sxs-lookup"><span data-stu-id="74e52-240">Preset target video frame rate in .3f format.</span></span> |
| <span data-ttu-id="74e52-241">**Bithastighet**</span><span class="sxs-lookup"><span data-stu-id="74e52-241">**Bitrate**</span></span><br/><br/> <span data-ttu-id="74e52-242">minInclusive = ”0”</span><span class="sxs-lookup"><span data-stu-id="74e52-242">minInclusive ="0"</span></span><br/><br/> <span data-ttu-id="74e52-243">Krävs</span><span class="sxs-lookup"><span data-stu-id="74e52-243">Required</span></span> |<span data-ttu-id="74e52-244">**Xs:int**</span><span class="sxs-lookup"><span data-stu-id="74e52-244">**xs:int**</span></span> |<span data-ttu-id="74e52-245">Genomsnittlig video bithastighet i kilobit per sekund som beräknas från hello AssetFile.</span><span class="sxs-lookup"><span data-stu-id="74e52-245">Average video bit rate in kilobits per second, as calculated from hello AssetFile.</span></span> <span data-ttu-id="74e52-246">Räknar endast hello elementära dataströmmen nyttolasten och inkluderar inte hello paketering kostnader.</span><span class="sxs-lookup"><span data-stu-id="74e52-246">Counts only hello elementary stream payload, and does not include hello packaging overhead.</span></span> |
| <span data-ttu-id="74e52-247">**TargetBitrate**</span><span class="sxs-lookup"><span data-stu-id="74e52-247">**TargetBitrate**</span></span><br/><br/> <span data-ttu-id="74e52-248">minInclusive = ”0”</span><span class="sxs-lookup"><span data-stu-id="74e52-248">minInclusive ="0"</span></span><br/><br/> <span data-ttu-id="74e52-249">Krävs</span><span class="sxs-lookup"><span data-stu-id="74e52-249">Required</span></span> |<span data-ttu-id="74e52-250">**Xs:int**</span><span class="sxs-lookup"><span data-stu-id="74e52-250">**xs:int**</span></span> |<span data-ttu-id="74e52-251">Rikta genomsnittlig bithastighet för den här videon spår som begärdes via hello kodning förinställda i kilobit per sekund.</span><span class="sxs-lookup"><span data-stu-id="74e52-251">Target average bitrate for this video track, as requested via hello encoding preset, in kilobits per second.</span></span> |
| <span data-ttu-id="74e52-252">**MaxGOPBitrate**</span><span class="sxs-lookup"><span data-stu-id="74e52-252">**MaxGOPBitrate**</span></span><br/><br/> <span data-ttu-id="74e52-253">minInclusive = ”0”</span><span class="sxs-lookup"><span data-stu-id="74e52-253">minInclusive ="0"</span></span> |<span data-ttu-id="74e52-254">**Xs:int**</span><span class="sxs-lookup"><span data-stu-id="74e52-254">**xs:int**</span></span> |<span data-ttu-id="74e52-255">Max GOP genomsnittlig bithastighet för den här videon spår i kilobit per sekund.</span><span class="sxs-lookup"><span data-stu-id="74e52-255">Max GOP average bitrate for this video track, in kilobits per second.</span></span> |

## <span data-ttu-id="74e52-256"><a name="AudioTracks "></a>AudioTracks element</span><span class="sxs-lookup"><span data-stu-id="74e52-256"><a name="AudioTracks "></a> AudioTracks element</span></span>
<span data-ttu-id="74e52-257">Varje fysisk AssetFile kan innehålla noll eller flera ljud spårar överlagrat till en lämplig behållare format i den.</span><span class="sxs-lookup"><span data-stu-id="74e52-257">Each physical AssetFile can contain in it zero or more audio tracks interleaved into an appropriate container format.</span></span> <span data-ttu-id="74e52-258">Detta är de ljud spårar hello samling.</span><span class="sxs-lookup"><span data-stu-id="74e52-258">This is hello collection of all those audio tracks.</span></span>  

<span data-ttu-id="74e52-259">Du hittar ett XML-exempel [XML-exempel](media-services-output-metadata-schema.md#xml).</span><span class="sxs-lookup"><span data-stu-id="74e52-259">You can find an XML example [XML example](media-services-output-metadata-schema.md#xml).</span></span>  

### <a name="child-elements"></a><span data-ttu-id="74e52-260">Underordnade element</span><span class="sxs-lookup"><span data-stu-id="74e52-260">Child elements</span></span>
| <span data-ttu-id="74e52-261">Namn</span><span class="sxs-lookup"><span data-stu-id="74e52-261">Name</span></span> | <span data-ttu-id="74e52-262">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="74e52-262">Description</span></span> |
| --- | --- |
| <span data-ttu-id="74e52-263">**AudioTrack**</span><span class="sxs-lookup"><span data-stu-id="74e52-263">**AudioTrack**</span></span><br/><br/> <span data-ttu-id="74e52-264">minOccurs = ”1” maxOccurs = ”unbounded”</span><span class="sxs-lookup"><span data-stu-id="74e52-264">minOccurs="1" maxOccurs="unbounded"</span></span> |<span data-ttu-id="74e52-265">En specifik ljud spåra hello överordnade AssetFile.</span><span class="sxs-lookup"><span data-stu-id="74e52-265">A specific audio track in hello parent AssetFile.</span></span> <span data-ttu-id="74e52-266">Mer information finns i [AudioTrack elementet](media-services-output-metadata-schema.md).</span><span class="sxs-lookup"><span data-stu-id="74e52-266">For more information, see [AudioTrack element](media-services-output-metadata-schema.md).</span></span> |

## <span data-ttu-id="74e52-267"><a name="AudioTrack "></a>AudioTrack element</span><span class="sxs-lookup"><span data-stu-id="74e52-267"><a name="AudioTrack "></a> AudioTrack element</span></span>
<span data-ttu-id="74e52-268">En specifik ljud spåra hello överordnade AssetFile.</span><span class="sxs-lookup"><span data-stu-id="74e52-268">A specific audio track in hello parent AssetFile.</span></span>  

<span data-ttu-id="74e52-269">Du hittar ett XML-exempel [XML-exempel](media-services-output-metadata-schema.md#xml).</span><span class="sxs-lookup"><span data-stu-id="74e52-269">You can find an XML example [XML example](media-services-output-metadata-schema.md#xml).</span></span>  

### <a name="attributes"></a><span data-ttu-id="74e52-270">Attribut</span><span class="sxs-lookup"><span data-stu-id="74e52-270">Attributes</span></span>
| <span data-ttu-id="74e52-271">Namn</span><span class="sxs-lookup"><span data-stu-id="74e52-271">Name</span></span> | <span data-ttu-id="74e52-272">Typ</span><span class="sxs-lookup"><span data-stu-id="74e52-272">Type</span></span> | <span data-ttu-id="74e52-273">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="74e52-273">Description</span></span> |
| --- | --- | --- |
| <span data-ttu-id="74e52-274">**ID**</span><span class="sxs-lookup"><span data-stu-id="74e52-274">**Id**</span></span><br/><br/> <span data-ttu-id="74e52-275">minInclusive = ”0”</span><span class="sxs-lookup"><span data-stu-id="74e52-275">minInclusive ="0"</span></span><br/><br/> <span data-ttu-id="74e52-276">Krävs</span><span class="sxs-lookup"><span data-stu-id="74e52-276">Required</span></span> |<span data-ttu-id="74e52-277">**Xs:int**</span><span class="sxs-lookup"><span data-stu-id="74e52-277">**xs:int**</span></span> |<span data-ttu-id="74e52-278">Nollbaserade indexet för den här ljud spåra. **Obs:** detta är inte nödvändigtvis hello TrackID som används i en MP4-fil.</span><span class="sxs-lookup"><span data-stu-id="74e52-278">Zero-based index of this audio track. **Note:**  This is not necessarily hello TrackID as used in an MP4 file.</span></span> |
| <span data-ttu-id="74e52-279">**Codec**</span><span class="sxs-lookup"><span data-stu-id="74e52-279">**Codec**</span></span> |<span data-ttu-id="74e52-280">**Xs:String**</span><span class="sxs-lookup"><span data-stu-id="74e52-280">**xs:string**</span></span> |<span data-ttu-id="74e52-281">Ljud spåra codec sträng.</span><span class="sxs-lookup"><span data-stu-id="74e52-281">Audio track codec string.</span></span> |
| <span data-ttu-id="74e52-282">**EncoderVersion**</span><span class="sxs-lookup"><span data-stu-id="74e52-282">**EncoderVersion**</span></span> |<span data-ttu-id="74e52-283">**Xs:String**</span><span class="sxs-lookup"><span data-stu-id="74e52-283">**xs:string**</span></span> |<span data-ttu-id="74e52-284">Versionssträng valfria kodare som krävs för EAC3.</span><span class="sxs-lookup"><span data-stu-id="74e52-284">Optional encoder version string, required for EAC3.</span></span> |
| <span data-ttu-id="74e52-285">**Kanaler**</span><span class="sxs-lookup"><span data-stu-id="74e52-285">**Channels**</span></span><br/><br/> <span data-ttu-id="74e52-286">minInclusive = ”0”</span><span class="sxs-lookup"><span data-stu-id="74e52-286">minInclusive ="0"</span></span><br/><br/> <span data-ttu-id="74e52-287">Krävs</span><span class="sxs-lookup"><span data-stu-id="74e52-287">Required</span></span> |<span data-ttu-id="74e52-288">**Xs:int**</span><span class="sxs-lookup"><span data-stu-id="74e52-288">**xs:int**</span></span> |<span data-ttu-id="74e52-289">Antal ljud kanaler.</span><span class="sxs-lookup"><span data-stu-id="74e52-289">Number of audio channels.</span></span> |
| <span data-ttu-id="74e52-290">**SamplingRate**</span><span class="sxs-lookup"><span data-stu-id="74e52-290">**SamplingRate**</span></span><br/><br/> <span data-ttu-id="74e52-291">minInclusive = ”0”</span><span class="sxs-lookup"><span data-stu-id="74e52-291">minInclusive ="0"</span></span><br/><br/> <span data-ttu-id="74e52-292">Krävs</span><span class="sxs-lookup"><span data-stu-id="74e52-292">Required</span></span> |<span data-ttu-id="74e52-293">**Xs:int**</span><span class="sxs-lookup"><span data-stu-id="74e52-293">**xs:int**</span></span> |<span data-ttu-id="74e52-294">I prover per sekund eller Hz ljud samplingsfrekvensen.</span><span class="sxs-lookup"><span data-stu-id="74e52-294">Audio sampling rate in samples/sec or Hz.</span></span> |
| <span data-ttu-id="74e52-295">**Bithastighet**</span><span class="sxs-lookup"><span data-stu-id="74e52-295">**Bitrate**</span></span><br/><br/> <span data-ttu-id="74e52-296">minInclusive = ”0”</span><span class="sxs-lookup"><span data-stu-id="74e52-296">minInclusive ="0"</span></span><br/><br/> <span data-ttu-id="74e52-297">Krävs</span><span class="sxs-lookup"><span data-stu-id="74e52-297">Required</span></span> |<span data-ttu-id="74e52-298">**Xs:int**</span><span class="sxs-lookup"><span data-stu-id="74e52-298">**xs:int**</span></span> |<span data-ttu-id="74e52-299">Genomsnittlig ljud bithastighet i bitar per sekund som beräknas från hello AssetFile.</span><span class="sxs-lookup"><span data-stu-id="74e52-299">Average audio bit rate in bits per second, as calculated from hello AssetFile.</span></span> <span data-ttu-id="74e52-300">Räknar endast hello elementära dataströmmen nyttolasten och inkluderar inte hello paketering kostnader.</span><span class="sxs-lookup"><span data-stu-id="74e52-300">Counts only hello elementary stream payload, and does not include hello packaging overhead.</span></span> |
| <span data-ttu-id="74e52-301">**BitsPerSample**</span><span class="sxs-lookup"><span data-stu-id="74e52-301">**BitsPerSample**</span></span><br/><br/> <span data-ttu-id="74e52-302">minInclusive = ”0”</span><span class="sxs-lookup"><span data-stu-id="74e52-302">minInclusive ="0"</span></span><br/><br/> <span data-ttu-id="74e52-303">Krävs</span><span class="sxs-lookup"><span data-stu-id="74e52-303">Required</span></span> |<span data-ttu-id="74e52-304">**Xs:int**</span><span class="sxs-lookup"><span data-stu-id="74e52-304">**xs:int**</span></span> |<span data-ttu-id="74e52-305">Bitar per prov för hello wFormatTag formatet typ.</span><span class="sxs-lookup"><span data-stu-id="74e52-305">Bits per sample for hello wFormatTag format type.</span></span> |

### <a name="child-elements"></a><span data-ttu-id="74e52-306">Underordnade element</span><span class="sxs-lookup"><span data-stu-id="74e52-306">Child elements</span></span>
| <span data-ttu-id="74e52-307">Namn</span><span class="sxs-lookup"><span data-stu-id="74e52-307">Name</span></span> | <span data-ttu-id="74e52-308">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="74e52-308">Description</span></span> |
| --- | --- |
| <span data-ttu-id="74e52-309">**LoudnessMeteringResultParameters**</span><span class="sxs-lookup"><span data-stu-id="74e52-309">**LoudnessMeteringResultParameters**</span></span><br/><br/> <span data-ttu-id="74e52-310">minOccurs = ”0” maxOccurs = ”1”</span><span class="sxs-lookup"><span data-stu-id="74e52-310">minOccurs="0" maxOccurs="1"</span></span> |<span data-ttu-id="74e52-311">Loudness avläsning resultatet parametrar.</span><span class="sxs-lookup"><span data-stu-id="74e52-311">Loudness metering result parameters.</span></span> <span data-ttu-id="74e52-312">Mer information finns i [LoudnessMeteringResultParameters elementet](media-services-output-metadata-schema.md).</span><span class="sxs-lookup"><span data-stu-id="74e52-312">For more information, see [LoudnessMeteringResultParameters element](media-services-output-metadata-schema.md).</span></span> |

## <span data-ttu-id="74e52-313"><a name="LoudnessMeteringResultParameters "></a>LoudnessMeteringResultParameters element</span><span class="sxs-lookup"><span data-stu-id="74e52-313"><a name="LoudnessMeteringResultParameters "></a> LoudnessMeteringResultParameters element</span></span>
<span data-ttu-id="74e52-314">Loudness avläsning resultatet parametrar.</span><span class="sxs-lookup"><span data-stu-id="74e52-314">Loudness metering result parameters.</span></span>  

<span data-ttu-id="74e52-315">Du hittar ett XML-exempel [XML-exempel](media-services-output-metadata-schema.md#xml).</span><span class="sxs-lookup"><span data-stu-id="74e52-315">You can find an XML example [XML example](media-services-output-metadata-schema.md#xml).</span></span>  

### <a name="attributes"></a><span data-ttu-id="74e52-316">Attribut</span><span class="sxs-lookup"><span data-stu-id="74e52-316">Attributes</span></span>
| <span data-ttu-id="74e52-317">Namn</span><span class="sxs-lookup"><span data-stu-id="74e52-317">Name</span></span> | <span data-ttu-id="74e52-318">Typ</span><span class="sxs-lookup"><span data-stu-id="74e52-318">Type</span></span> | <span data-ttu-id="74e52-319">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="74e52-319">Description</span></span> |
| --- | --- | --- |
| <span data-ttu-id="74e52-320">**DPLMVersionInformation**</span><span class="sxs-lookup"><span data-stu-id="74e52-320">**DPLMVersionInformation**</span></span> |<span data-ttu-id="74e52-321">**Xs:String**</span><span class="sxs-lookup"><span data-stu-id="74e52-321">**xs:string**</span></span> |<span data-ttu-id="74e52-322">**Dolby** professional loudness avläsning development kit version.</span><span class="sxs-lookup"><span data-stu-id="74e52-322">**Dolby** professional loudness metering development kit version.</span></span> |
| <span data-ttu-id="74e52-323">**DialogNormalization**</span><span class="sxs-lookup"><span data-stu-id="74e52-323">**DialogNormalization**</span></span><br/><br/> <span data-ttu-id="74e52-324">minInclusive = ”-31” maxInclusive = ”-1”</span><span class="sxs-lookup"><span data-stu-id="74e52-324">minInclusive="-31" maxInclusive="-1"</span></span><br/><br/> <span data-ttu-id="74e52-325">Krävs</span><span class="sxs-lookup"><span data-stu-id="74e52-325">Required</span></span> |<span data-ttu-id="74e52-326">**Xs:int**</span><span class="sxs-lookup"><span data-stu-id="74e52-326">**xs:int**</span></span> |<span data-ttu-id="74e52-327">DialogNormalization genereras genom DPLM, krävs när LoudnessMetering har angetts</span><span class="sxs-lookup"><span data-stu-id="74e52-327">DialogNormalization generated through DPLM, required when LoudnessMetering is set</span></span> |
| <span data-ttu-id="74e52-328">**IntegratedLoudness**</span><span class="sxs-lookup"><span data-stu-id="74e52-328">**IntegratedLoudness**</span></span><br/><br/> <span data-ttu-id="74e52-329">minInclusive = ”-70” maxInclusive = ”10”</span><span class="sxs-lookup"><span data-stu-id="74e52-329">minInclusive="-70" maxInclusive="10"</span></span><br/><br/> <span data-ttu-id="74e52-330">Krävs</span><span class="sxs-lookup"><span data-stu-id="74e52-330">Required</span></span> |<span data-ttu-id="74e52-331">**Xs:float**</span><span class="sxs-lookup"><span data-stu-id="74e52-331">**xs:float**</span></span> |<span data-ttu-id="74e52-332">Integrerad loudness</span><span class="sxs-lookup"><span data-stu-id="74e52-332">Integrated loudness</span></span> |
| <span data-ttu-id="74e52-333">**IntegratedLoudnessUnit**</span><span class="sxs-lookup"><span data-stu-id="74e52-333">**IntegratedLoudnessUnit**</span></span><br/><br/> <span data-ttu-id="74e52-334">Krävs</span><span class="sxs-lookup"><span data-stu-id="74e52-334">Required</span></span> |<span data-ttu-id="74e52-335">**Xs:String**</span><span class="sxs-lookup"><span data-stu-id="74e52-335">**xs:string**</span></span> |<span data-ttu-id="74e52-336">Integrerad loudness enhet.</span><span class="sxs-lookup"><span data-stu-id="74e52-336">Integrated loudness unit.</span></span> |
| <span data-ttu-id="74e52-337">**IntegratedLoudnessGatingMethod**</span><span class="sxs-lookup"><span data-stu-id="74e52-337">**IntegratedLoudnessGatingMethod**</span></span><br/><br/> <span data-ttu-id="74e52-338">Krävs</span><span class="sxs-lookup"><span data-stu-id="74e52-338">Required</span></span> |<span data-ttu-id="74e52-339">**Xs:String**</span><span class="sxs-lookup"><span data-stu-id="74e52-339">**xs:string**</span></span> |<span data-ttu-id="74e52-340">Gating identifierare</span><span class="sxs-lookup"><span data-stu-id="74e52-340">Gating identifier</span></span> |
| <span data-ttu-id="74e52-341">**IntegratedLoudnessSpeechPercentage**</span><span class="sxs-lookup"><span data-stu-id="74e52-341">**IntegratedLoudnessSpeechPercentage**</span></span><br/><br/> <span data-ttu-id="74e52-342">minInclusive = ”0” maxInclusive = ”100”</span><span class="sxs-lookup"><span data-stu-id="74e52-342">minInclusive ="0" maxInclusive="100"</span></span> |<span data-ttu-id="74e52-343">**Xs:float**</span><span class="sxs-lookup"><span data-stu-id="74e52-343">**xs:float**</span></span> |<span data-ttu-id="74e52-344">Taligenkänning innehåll över hello program i procent.</span><span class="sxs-lookup"><span data-stu-id="74e52-344">Speech content over hello program, as a percentage.</span></span> |
| <span data-ttu-id="74e52-345">**SamplePeak**</span><span class="sxs-lookup"><span data-stu-id="74e52-345">**SamplePeak**</span></span><br/><br/> <span data-ttu-id="74e52-346">Krävs</span><span class="sxs-lookup"><span data-stu-id="74e52-346">Required</span></span> |<span data-ttu-id="74e52-347">**Xs:float**</span><span class="sxs-lookup"><span data-stu-id="74e52-347">**xs:float**</span></span> |<span data-ttu-id="74e52-348">Belastning absolut exempelvärde avmarkerad sedan återställning eller sedan den startades senast per kanal.</span><span class="sxs-lookup"><span data-stu-id="74e52-348">Peak absolute sample value, since reset or since it was last cleared, per channel.</span></span>  <span data-ttu-id="74e52-349">Enheterna är dBFS.</span><span class="sxs-lookup"><span data-stu-id="74e52-349">Units are dBFS.</span></span> |
| <span data-ttu-id="74e52-350">**SamplePeakUnit**</span><span class="sxs-lookup"><span data-stu-id="74e52-350">**SamplePeakUnit**</span></span><br/><br/> <span data-ttu-id="74e52-351">fast = ”dBFS”</span><span class="sxs-lookup"><span data-stu-id="74e52-351">fixed="dBFS"</span></span><br/><br/> <span data-ttu-id="74e52-352">Krävs</span><span class="sxs-lookup"><span data-stu-id="74e52-352">Required</span></span> |<span data-ttu-id="74e52-353">**Xs:anySimpleType**</span><span class="sxs-lookup"><span data-stu-id="74e52-353">**xs:anySimpleType**</span></span> |<span data-ttu-id="74e52-354">Exempel resursbehov.</span><span class="sxs-lookup"><span data-stu-id="74e52-354">Sample peak unit.</span></span> |
| <span data-ttu-id="74e52-355">**TruePeak**</span><span class="sxs-lookup"><span data-stu-id="74e52-355">**TruePeak**</span></span><br/><br/> <span data-ttu-id="74e52-356">Krävs</span><span class="sxs-lookup"><span data-stu-id="74e52-356">Required</span></span> |<span data-ttu-id="74e52-357">**Xs:float**</span><span class="sxs-lookup"><span data-stu-id="74e52-357">**xs:float**</span></span> |<span data-ttu-id="74e52-358">Maximala true högsta värde, enligt ITU-R BS.1770-2, sedan återställning eller sedan den startades senast avmarkerad per kanal.</span><span class="sxs-lookup"><span data-stu-id="74e52-358">Maximum true peak value, as per ITU-R BS.1770-2, since reset or since it was last cleared, per channel.</span></span> <span data-ttu-id="74e52-359">Enheterna är dBTP.</span><span class="sxs-lookup"><span data-stu-id="74e52-359">Units are dBTP.</span></span> |
| <span data-ttu-id="74e52-360">**TruePeakUnit**</span><span class="sxs-lookup"><span data-stu-id="74e52-360">**TruePeakUnit**</span></span><br/><br/> <span data-ttu-id="74e52-361">fast = ”dBTP”</span><span class="sxs-lookup"><span data-stu-id="74e52-361">fixed="dBTP"</span></span><br/><br/> <span data-ttu-id="74e52-362">Krävs</span><span class="sxs-lookup"><span data-stu-id="74e52-362">Required</span></span> |<span data-ttu-id="74e52-363">**Xs:anySimpleType**</span><span class="sxs-lookup"><span data-stu-id="74e52-363">**xs:anySimpleType**</span></span> |<span data-ttu-id="74e52-364">True resursbehov.</span><span class="sxs-lookup"><span data-stu-id="74e52-364">True peak unit.</span></span> |

## <a name="schema-code"></a><span data-ttu-id="74e52-365">Schemat kod</span><span class="sxs-lookup"><span data-stu-id="74e52-365">Schema Code</span></span>
    <?xml version="1.0" encoding="utf-8"?>  
    <xs:schema xmlns:xs="http://www.w3.org/2001/XMLSchema" xmlns:msdata="urn:schemas-microsoft-com:xml-msdata" version="1.2"  
               xmlns="http://schemas.microsoft.com/windowsazure/mediaservices/2013/05/mediaencoder/metadata"  
               targetNamespace="http://schemas.microsoft.com/windowsazure/mediaservices/2013/05/mediaencoder/metadata"  
               elementFormDefault="qualified">  
      <xs:element name="AssetFiles">  
        <xs:annotation>  
          <xs:documentation>Collection of AssetFile entries for hello encoding job</xs:documentation>  
        </xs:annotation>  
        <xs:complexType>  
          <xs:sequence>  
            <xs:element name="AssetFile" minOccurs="1" maxOccurs="unbounded">  
              <xs:annotation>  
                <xs:documentation>asset file</xs:documentation>  
              </xs:annotation>  
              <xs:complexType>  
                <xs:sequence>  
                  <xs:element name="Sources">  
                    <xs:annotation>  
                      <xs:documentation>Collection of input/source media files, that was processed in order tooproduce this AssetFile</xs:documentation>  
                    </xs:annotation>  
                    <xs:complexType>  
                      <xs:sequence>  
                        <xs:element name="Source" minOccurs="1" maxOccurs="unbounded">  
                          <xs:annotation>  
                            <xs:documentation>An input/source file used when generating this asset</xs:documentation>  
                          </xs:annotation>  
                          <xs:complexType>  
                            <xs:attribute name="Name" type="xs:string" use="required">  
                              <xs:annotation>  
                                <xs:documentation>input source file name</xs:documentation>  
                              </xs:annotation>  
                            </xs:attribute>  
                          </xs:complexType>  
                        </xs:element>  
                      </xs:sequence>  
                    </xs:complexType>  
                  </xs:element>  
                  <xs:element name="VideoTracks" minOccurs="0">  
                    <xs:annotation>  
                      <xs:documentation>Each physical AssetFile can contain in it zero or more video tracks interleaved into an appropriate container format. This is hello collection of all those video tracks</xs:documentation>  
                    </xs:annotation>  
                    <xs:complexType>  
                      <xs:sequence>  
                        <xs:element name="VideoTrack" maxOccurs="unbounded">  
                          <xs:annotation>  
                            <xs:documentation>A specific video track in hello parent AssetFile</xs:documentation>  
                          </xs:annotation>  
                          <xs:complexType>  
                            <xs:attribute name="Id" use="required">  
                              <xs:annotation>  
                                <xs:documentation>zero-based index of this video track. Note: this is not necessarily hello TrackID as used in an MP4 file</xs:documentation>  
                              </xs:annotation>  
                              <xs:simpleType>  
                                <xs:restriction base="xs:int">  
                                  <xs:minInclusive value="0"/>  
                                </xs:restriction>  
                              </xs:simpleType>  
                            </xs:attribute>  
                            <xs:attribute name="FourCC" type="xs:string" use="required">  
                              <xs:annotation>  
                                <xs:documentation>video codec FourCC code</xs:documentation>  
                              </xs:annotation>  
                            </xs:attribute>  
                            <xs:attribute name="Profile" type="xs:string">  
                              <xs:annotation>  
                                <xs:documentation>H264 profile (only appliable for H264 codec)</xs:documentation>  
                              </xs:annotation>  
                            </xs:attribute>  
                            <xs:attribute name="Level" type="xs:string">  
                              <xs:annotation>  
                                <xs:documentation>H264 level (only appliable for H264 codec)</xs:documentation>  
                              </xs:annotation>  
                            </xs:attribute>  
                            <xs:attribute name="Width" use="required">  
                              <xs:annotation>  
                                <xs:documentation>encoded video width in pixels</xs:documentation>  
                              </xs:annotation>  
                              <xs:simpleType>  
                                <xs:restriction base="xs:int">  
                                  <xs:minInclusive value="0"/>  
                                </xs:restriction>  
                              </xs:simpleType>  
                            </xs:attribute>  
                            <xs:attribute name="Height" use="required">  
                              <xs:annotation>  
                                <xs:documentation>encoded video height in pixels</xs:documentation>  
                              </xs:annotation>  
                              <xs:simpleType>  
                                <xs:restriction base="xs:int">  
                                  <xs:minInclusive value="0"/>  
                                </xs:restriction>  
                              </xs:simpleType>  
                            </xs:attribute>  
                            <xs:attribute name="DisplayAspectRatioNumerator" use="required">  
                              <xs:annotation>  
                                <xs:documentation>video display aspect ratio numerator</xs:documentation>  
                              </xs:annotation>  
                              <xs:simpleType>  
                                <xs:restriction base="xs:double">  
                                  <xs:minInclusive value="0"/>  
                                </xs:restriction>  
                              </xs:simpleType>  
                            </xs:attribute>  
                            <xs:attribute name="DisplayAspectRatioDenominator" use="required">  
                              <xs:annotation>  
                                <xs:documentation>video display aspect ratio denominator</xs:documentation>  
                              </xs:annotation>  
                              <xs:simpleType>  
                                <xs:restriction base="xs:double">  
                                  <xs:minInclusive value="0"/>  
                                </xs:restriction>  
                              </xs:simpleType>  
                            </xs:attribute>  
                            <xs:attribute name="Framerate" use="required">  
                              <xs:annotation>  
                                <xs:documentation>measured video frame rate in .3f format</xs:documentation>  
                              </xs:annotation>  
                              <xs:simpleType>  
                                <xs:restriction base="xs:decimal">  
                                  <xs:minInclusive value="0"/>  
                                  <xs:fractionDigits value="3"/>  
                                </xs:restriction>  
                              </xs:simpleType>  
                            </xs:attribute>  
                            <xs:attribute name="TargetFramerate" use="required">  
                              <xs:annotation>  
                                <xs:documentation>preset target video frame rate in .3f format</xs:documentation>  
                              </xs:annotation>  
                              <xs:simpleType>  
                                <xs:restriction base="xs:decimal">  
                                  <xs:minInclusive value="0"/>  
                                  <xs:fractionDigits value="3"/>  
                                </xs:restriction>  
                              </xs:simpleType>  
                            </xs:attribute>  
                            <xs:attribute name="Bitrate" use="required">  
                              <xs:annotation>  
                                <xs:documentation>average video bit rate in kilobits per second, as calculated from hello AssetFile. Counts only hello elementary stream payload, and does not include hello packaging overhead</xs:documentation>  
                              </xs:annotation>  
                              <xs:simpleType>  
                                <xs:restriction base="xs:int">  
                                  <xs:minInclusive value="0"/>  
                                </xs:restriction>  
                              </xs:simpleType>  
                            </xs:attribute>  
                            <xs:attribute name="TargetBitrate" use="required">  
                              <xs:annotation>  
                                <xs:documentation>target average bitrate for this video track, as requested via hello encoding preset, in kilobits per second</xs:documentation>  
                              </xs:annotation>  
                              <xs:simpleType>  
                                <xs:restriction base="xs:int">  
                                  <xs:minInclusive value="0"/>  
                                </xs:restriction>  
                              </xs:simpleType>  
                            </xs:attribute>  
                            <xs:attribute name="MaxGOPBitrate">  
                              <xs:annotation>  
                                <xs:documentation>Max GOP average bitrate for this video track, in kilobits per second</xs:documentation>  
                              </xs:annotation>  
                              <xs:simpleType>  
                                <xs:restriction base="xs:int">  
                                  <xs:minInclusive value="0"/>  
                                </xs:restriction>  
                              </xs:simpleType>  
                            </xs:attribute>  
                          </xs:complexType>  
                        </xs:element>  
                      </xs:sequence>  
                    </xs:complexType>  
                  </xs:element>  
                  <xs:element name="AudioTracks" minOccurs="0">  
                    <xs:annotation>  
                      <xs:documentation>each physical AssetFile can contain in it zero or more audio tracks interleaved into an appropriate container format. This is hello collection of all those audio tracks</xs:documentation>  
                    </xs:annotation>  
                    <xs:complexType>  
                      <xs:sequence>  
                        <xs:element name="AudioTrack" maxOccurs="unbounded">  
                          <xs:annotation>  
                            <xs:documentation>a specific audio track in hello parent AssetFile</xs:documentation>  
                          </xs:annotation>  
                          <xs:complexType>  
                            <xs:sequence>  
                              <xs:element name="LoudnessMeteringResultParameters" minOccurs="0" maxOccurs="1">  
                                <xs:annotation>  
                                  <xs:documentation>Loudness Metering Result Parameters</xs:documentation>  
                                </xs:annotation>  
                                <xs:complexType>  
                                  <xs:attribute name="DPLMVersionInformation" type="xs:string">  
                                    <xs:annotation>  
                                      <xs:documentation>Dolby Professional Loudness Metering Development Kit Version</xs:documentation>  
                                    </xs:annotation>  
                                  </xs:attribute>  
                                  <xs:attribute name="DialogNormalization" use="required">  
                                    <xs:annotation>  
                                      <xs:documentation> DialogNormalization generated through DPLM, required when LoudnessMetering is set</xs:documentation>  
                                    </xs:annotation>  
                                    <xs:simpleType>  
                                      <xs:restriction base="xs:int">  
                                        <xs:minInclusive value="-31"/>  
                                        <xs:maxInclusive value="-1"/>  
                                      </xs:restriction>  
                                    </xs:simpleType>  
                                  </xs:attribute>  
                                  <xs:attribute name="IntegratedLoudness" use="required">  
                                    <xs:annotation>  
                                      <xs:documentation>Integrated loudness</xs:documentation>  
                                    </xs:annotation>  
                                    <xs:simpleType>  
                                      <xs:restriction base="xs:float">  
                                        <xs:minInclusive value="-70"/>  
                                        <xs:maxInclusive value="10"/>  
                                      </xs:restriction>  
                                    </xs:simpleType>  
                                  </xs:attribute>  
                                  <xs:attribute name="IntegratedLoudnessUnit" use="required" type="xs:string">  
                                  </xs:attribute>  
                                  <xs:attribute name="IntegratedLoudnessGatingMethod" use="required" type="xs:string">  
                                    <xs:annotation>  
                                      <xs:documentation>Gating identifier</xs:documentation>  
                                    </xs:annotation>  
                                  </xs:attribute>  
                                  <xs:attribute name="IntegratedLoudnessSpeechPercentage">  
                                    <xs:annotation>  
                                      <xs:documentation>Speech content over hello program, as a percentage.</xs:documentation>  
                                    </xs:annotation>  
                                    <xs:simpleType>  
                                      <xs:restriction base="xs:float">  
                                        <xs:minInclusive value="0"/>  
                                        <xs:maxInclusive value="100"/>  
                                      </xs:restriction>  
                                    </xs:simpleType>  
                                  </xs:attribute>  
                                  <xs:attribute name="SamplePeak" use="required" type="xs:float">  
                                    <xs:annotation>  
                                      <xs:documentation>Peak absolute sample value, since reset or since it was last cleared, per channel.  Units are dBFS.</xs:documentation>  
                                    </xs:annotation>  
                                  </xs:attribute>  
                                  <xs:attribute name="SamplePeakUnit" use="required" fixed="dBFS">  
                                  </xs:attribute>  
                                  <xs:attribute name="TruePeak" use="required" type="xs:float">  
                                    <xs:annotation>  
                                      <xs:documentation>Maximum True Peak value, as per ITU-R BS.1770-2, since reset or since it was last cleared, per channel.  Units are dBTP.</xs:documentation>  
                                    </xs:annotation>  
                                  </xs:attribute>  
                                  <xs:attribute name="TruePeakUnit" use="required" fixed="dBTP">  
                                  </xs:attribute>  
                                </xs:complexType>  
                              </xs:element>  
                            </xs:sequence>  
                            <xs:attribute name="Id" use="required">  
                              <xs:annotation>  
                                <xs:documentation>zero-based index of this audio track. Note: this is not necessarily hello TrackID as used in an MP4 file</xs:documentation>  
                              </xs:annotation>  
                              <xs:simpleType>  
                                <xs:restriction base="xs:int">  
                                  <xs:minInclusive value="0"/>  
                                </xs:restriction>  
                              </xs:simpleType>  
                            </xs:attribute>  
                            <xs:attribute name="Codec" type="xs:string">  
                              <xs:annotation>  
                                <xs:documentation>audio track codec string</xs:documentation>  
                              </xs:annotation>  
                            </xs:attribute>  
                            <xs:attribute name="EncoderVersion" type="xs:string">  
                              <xs:annotation>  
                                <xs:documentation>optional encoder version string, required for EAC3</xs:documentation>  
                              </xs:annotation>  
                            </xs:attribute>  
                            <xs:attribute name="Channels" use="required">  
                              <xs:annotation>  
                                <xs:documentation>number of audio channels</xs:documentation>  
                              </xs:annotation>  
                              <xs:simpleType>  
                                <xs:restriction base="xs:int">  
                                  <xs:minInclusive value="0"/>  
                                </xs:restriction>  
                              </xs:simpleType>  
                            </xs:attribute>  
                            <xs:attribute name="SamplingRate" use="required">  
                              <xs:annotation>  
                                <xs:documentation>audio sampling rate in samples/sec or Hz</xs:documentation>  
                              </xs:annotation>  
                              <xs:simpleType>  
                                <xs:restriction base="xs:int">  
                                  <xs:minInclusive value="0"/>  
                                </xs:restriction>  
                              </xs:simpleType>  
                            </xs:attribute>  
                            <xs:attribute name="Bitrate" use="required">  
                              <xs:annotation>  
                                <xs:documentation>average audio bit rate in bits per second, as calculated from hello AssetFile. Counts only hello elementary stream payload, and does not include hello packaging overhead</xs:documentation>  
                              </xs:annotation>  
                              <xs:simpleType>  
                                <xs:restriction base="xs:int">  
                                  <xs:minInclusive value="0"/>  
                                </xs:restriction>  
                              </xs:simpleType>  
                            </xs:attribute>  
                            <xs:attribute name="BitsPerSample" use="required">  
                              <xs:annotation>  
                                <xs:documentation>Bits per sample for hello wFormatTag format type</xs:documentation>  
                              </xs:annotation>  
                              <xs:simpleType>  
                                <xs:restriction base="xs:int">  
                                  <xs:minInclusive value="0"/>  
                                </xs:restriction>  
                              </xs:simpleType>  
                            </xs:attribute>  
                          </xs:complexType>  
                        </xs:element>  
                      </xs:sequence>  
                    </xs:complexType>  
                  </xs:element>  
                </xs:sequence>  
                <xs:attribute name="Name" type="xs:string" use="required">  
                  <xs:annotation>  
                    <xs:documentation>hello media asset file name</xs:documentation>  
                  </xs:annotation>  
                </xs:attribute>  
                <xs:attribute name="Size" use="required">  
                  <xs:annotation>  
                    <xs:documentation>size of file in bytes</xs:documentation>  
                  </xs:annotation>  
                  <xs:simpleType>  
                    <xs:restriction base="xs:long">  
                      <xs:minInclusive value="0"/>  
                    </xs:restriction>  
                  </xs:simpleType>  
                </xs:attribute>  
                <xs:attribute name="Duration" use="required">  
                  <xs:annotation>  
                    <xs:documentation>content play back duration</xs:documentation>  
                  </xs:annotation>  
                  <xs:simpleType>  
                    <xs:restriction base="xs:duration"/>  
                  </xs:simpleType>  
                </xs:attribute>  
              </xs:complexType>  
            </xs:element>  
          </xs:sequence>  
        </xs:complexType>  
      </xs:element>  
    </xs:schema>  



## <span data-ttu-id="74e52-366"><a name="xml"></a>XML-exempel</span><span class="sxs-lookup"><span data-stu-id="74e52-366"><a name="xml"></a> XML example</span></span>
 <span data-ttu-id="74e52-367">hello följande är ett exempel på hello utdatafilen metadata.</span><span class="sxs-lookup"><span data-stu-id="74e52-367">hello following is an example of hello Output metadata file.</span></span>  

    <AssetFiles xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:xsd="http://www.w3.org/2001/XMLSchema"   
                xmlns="http://schemas.microsoft.com/windowsazure/mediaservices/2013/05/mediaencoder/metadata">  
      <AssetFile Name="BigBuckBunny_H264_3400kbps_AAC_und_ch2_96kbps.mp4" Size="4646283" Duration="PT8.4288444S">  
        <Sources>  
          <Source Name="BigBuckBunny.mp4"/>  
        </Sources>  
        <VideoTracks>  
          <VideoTrack Id="0" FourCC="AVC1" Profile="Main" Level="3.2" Width="1280" Height="720" DisplayAspectRatioNumerator="16" DisplayAspectRatioDenominator="9" Framerate="23.974" TargetFramerate="23.974" Bitrate="4250" TargetBitrate="3400" MaxGOPBitrate="5514"/>  
        </VideoTracks>  
        <AudioTracks>  
          <AudioTrack Id="0" Codec="AacLc" Channels="2" SamplingRate="44100" Bitrate="93" BitsPerSample="16"/>  
        </AudioTracks>  
      </AssetFile>  
      <AssetFile Name="BigBuckBunny_H264_2250kbps_AAC_und_ch2_96kbps.mp4" Size="3166728" Duration="PT8.4288444S">  
        <Sources>  
          <Source Name="BigBuckBunny.mp4"/>  
        </Sources>  
        <VideoTracks>  
          <VideoTrack Id="0" FourCC="AVC1" Profile="Main" Level="3.1" Width="960" Height="540" DisplayAspectRatioNumerator="16" DisplayAspectRatioDenominator="9" Framerate="23.974" TargetFramerate="23.974" Bitrate="2846" TargetBitrate="2250" MaxGOPBitrate="3630"/>  
        </VideoTracks>  
        <AudioTracks>  
          <AudioTrack Id="0" Codec="AacLc" Channels="2" SamplingRate="44100" Bitrate="93" BitsPerSample="16"/>  
        </AudioTracks>  
      </AssetFile>  
      <AssetFile Name="BigBuckBunny_H264_1500kbps_AAC_und_ch2_96kbps.mp4" Size="2205095" Duration="PT8.4288444S">  
        <Sources>  
          <Source Name="BigBuckBunny.mp4"/>  
        </Sources>  
        <VideoTracks>  
          <VideoTrack Id="0" FourCC="AVC1" Profile="Main" Level="3.1" Width="960" Height="540" DisplayAspectRatioNumerator="16" DisplayAspectRatioDenominator="9" Framerate="23.974" TargetFramerate="23.974" Bitrate="1932" TargetBitrate="1500" MaxGOPBitrate="2513"/>  
        </VideoTracks>  
        <AudioTracks>  
          <AudioTrack Id="0" Codec="AacLc" Channels="2" SamplingRate="44100" Bitrate="93" BitsPerSample="16"/>  
        </AudioTracks>  
      </AssetFile>  
      <AssetFile Name="BigBuckBunny_H264_1000kbps_AAC_und_ch2_96kbps.mp4" Size="1508567" Duration="PT8.4288444S">  
        <Sources>  
          <Source Name="BigBuckBunny.mp4"/>  
        </Sources>  
        <VideoTracks>  
          <VideoTrack Id="0" FourCC="AVC1" Profile="Main" Level="3.0" Width="640" Height="360" DisplayAspectRatioNumerator="16" DisplayAspectRatioDenominator="9" Framerate="23.974" TargetFramerate="23.974" Bitrate="1271" TargetBitrate="1000" MaxGOPBitrate="1527"/>  
        </VideoTracks>  
        <AudioTracks>  
          <AudioTrack Id="0" Codec="AacLc" Channels="2" SamplingRate="44100" Bitrate="93" BitsPerSample="16"/>  
        </AudioTracks>  
      </AssetFile>  
      <AssetFile Name="BigBuckBunny_H264_650kbps_AAC_und_ch2_96kbps.mp4" Size="1057155" Duration="PT8.4288444S">  
        <Sources>  
          <Source Name="BigBuckBunny.mp4"/>  
        </Sources>  
        <VideoTracks>  
          <VideoTrack Id="0" FourCC="AVC1" Profile="Main" Level="3.0" Width="640" Height="360" DisplayAspectRatioNumerator="16" DisplayAspectRatioDenominator="9" Framerate="23.974" TargetFramerate="23.974" Bitrate="843" TargetBitrate="650" MaxGOPBitrate="1086"/>  
        </VideoTracks>  
        <AudioTracks>  
          <AudioTrack Id="0" Codec="AacLc" Channels="2" SamplingRate="44100" Bitrate="93" BitsPerSample="16"/>  
        </AudioTracks>  
      </AssetFile>  
      <AssetFile Name="BigBuckBunny_H264_400kbps_AAC_und_ch2_96kbps.mp4" Size="699262" Duration="PT8.4288444S">  
        <Sources>  
          <Source Name="BigBuckBunny.mp4"/>  
        </Sources>  
        <VideoTracks>  
          <VideoTrack Id="0" FourCC="AVC1" Profile="Main" Level="1.3" Width="320" Height="180" DisplayAspectRatioNumerator="16" DisplayAspectRatioDenominator="9" Framerate="23.974" TargetFramerate="23.974" Bitrate="503" TargetBitrate="400" MaxGOPBitrate="661"/>  
        </VideoTracks>  
        <AudioTracks>  
          <AudioTrack Id="0" Codec="AacLc" Channels="2" SamplingRate="44100" Bitrate="93" BitsPerSample="16"/>  
        </AudioTracks>  
      </AssetFile>  
      <AssetFile Name="BigBuckBunny_AAC_und_ch2_96kbps.mp4" Size="166780" Duration="PT8.4288444S">  
        <Sources>  
          <Source Name="BigBuckBunny.mp4"/>  
        </Sources>  
        <AudioTracks>  
          <AudioTrack Id="0" Codec="AacLc" Channels="2" SamplingRate="44100" Bitrate="93" BitsPerSample="16"/>  
        </AudioTracks>  
      </AssetFile>  
      <AssetFile Name="BigBuckBunny_AAC_und_ch2_56kbps.mp4" Size="124576" Duration="PT8.4288444S">  
        <Sources>  
          <Source Name="BigBuckBunny.mp4"/>  
        </Sources>  
        <AudioTracks>  
          <AudioTrack Id="0" Codec="AacLc" Channels="2" SamplingRate="44100" Bitrate="53" BitsPerSample="16"/>  
        </AudioTracks>  
      </AssetFile>  
    </AssetFiles>  

## <a name="next-steps"></a><span data-ttu-id="74e52-368">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="74e52-368">Next steps</span></span>
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a><span data-ttu-id="74e52-369">Ge feedback</span><span class="sxs-lookup"><span data-stu-id="74e52-369">Provide feedback</span></span>
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]
