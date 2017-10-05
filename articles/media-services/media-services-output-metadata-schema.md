---
title: Azure Media Services metadata utdataschema | Microsoft Docs
description: "Avsnittet ger en översikt över Azure Media Services utdataschema metadata."
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
ms.openlocfilehash: c175d359f93e7cd8cd73aa498ad8b71c4ec497f2
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/29/2017
---
# <a name="output-metadata"></a><span data-ttu-id="84c78-103">Metadata för utdata</span><span class="sxs-lookup"><span data-stu-id="84c78-103">Output Metadata</span></span>
## <a name="overview"></a><span data-ttu-id="84c78-104">Översikt</span><span class="sxs-lookup"><span data-stu-id="84c78-104">Overview</span></span>
<span data-ttu-id="84c78-105">Ett kodningsjobb är associerad med en inkommande tillgång (eller tillgångar) om du vill utföra vissa uppgifter som kodning.</span><span class="sxs-lookup"><span data-stu-id="84c78-105">An encoding job is associated with an input asset (or assets) on which you want to perform some encoding tasks.</span></span> <span data-ttu-id="84c78-106">Koda till exempel en MP4-fil att H.264 MP4 med anpassad bithastighet anger; Skapa en miniatyr; Skapa överlägg.</span><span class="sxs-lookup"><span data-stu-id="84c78-106">For example, encode an MP4 file to H.264 MP4 adaptive bitrate sets; create a thumbnail; create overlays.</span></span> <span data-ttu-id="84c78-107">En utdatatillgången skapas vid slutförandet av uppgiften.</span><span class="sxs-lookup"><span data-stu-id="84c78-107">Upon completion of a task, an output asset is produced.</span></span>  <span data-ttu-id="84c78-108">Utdatatillgången innehåller video, ljud, miniatyrer, osv. Utdatatillgången innehåller också en fil med metadata om utdatatillgången.</span><span class="sxs-lookup"><span data-stu-id="84c78-108">The output asset contains video, audio, thumbnails, etc. The output asset also contains a file with metadata about the output asset.</span></span> <span data-ttu-id="84c78-109">Namnet på XML-metadatafilen har följande format: &lt;source_file_name&gt;_manifest.xml (till exempel BigBuckBunny_manifest.xml).</span><span class="sxs-lookup"><span data-stu-id="84c78-109">The name of the metadata XML file has the following format: &lt;source_file_name&gt;_manifest.xml (for example, BigBuckBunny_manifest.xml).</span></span>  

<span data-ttu-id="84c78-110">Om du vill undersöka metadatafilen kan du skapa en **SAS** positionerare och hämta filen till den lokala datorn.</span><span class="sxs-lookup"><span data-stu-id="84c78-110">If you want to examine the metadata file, you can create a **SAS** locator and download the file to your local computer.</span></span>  

<span data-ttu-id="84c78-111">Det här avsnittet beskrivs de element och typer av XML-schemat som utdata metada (&lt;source_file_name&gt;_manifest.xml) är baserad.</span><span class="sxs-lookup"><span data-stu-id="84c78-111">This topic discusses the elements and types of the XML schema on which the output metada (&lt;source_file_name&gt;_manifest.xml) is based.</span></span> <span data-ttu-id="84c78-112">Information om den fil som innehåller metadata om inkommande tillgången, finns [inkommande Metadata](media-services-input-metadata-schema.md).</span><span class="sxs-lookup"><span data-stu-id="84c78-112">For information about the file that contains metadata about the input asset, see [Input Metadata](media-services-input-metadata-schema.md).</span></span>  

> [!NOTE]
> <span data-ttu-id="84c78-113">Du kan hitta fullständigt schema koden och XML-exempel i slutet av det här avsnittet.</span><span class="sxs-lookup"><span data-stu-id="84c78-113">You can find the complete schema code and XML example at the end of this topic.</span></span>  
>
>

## <span data-ttu-id="84c78-114"><a name="AssetFiles "></a>Rotelementet för AssetFiles</span><span class="sxs-lookup"><span data-stu-id="84c78-114"><a name="AssetFiles "></a> AssetFiles root element</span></span>
<span data-ttu-id="84c78-115">Samling av AssetFile poster för kodningsjobbet.</span><span class="sxs-lookup"><span data-stu-id="84c78-115">Collection of AssetFile entries for the encoding job.</span></span>  

### <a name="child-elements"></a><span data-ttu-id="84c78-116">Underordnade element</span><span class="sxs-lookup"><span data-stu-id="84c78-116">Child elements</span></span>
| <span data-ttu-id="84c78-117">Namn</span><span class="sxs-lookup"><span data-stu-id="84c78-117">Name</span></span> | <span data-ttu-id="84c78-118">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="84c78-118">Description</span></span> |
| --- | --- |
| <span data-ttu-id="84c78-119">**AssetFile**</span><span class="sxs-lookup"><span data-stu-id="84c78-119">**AssetFile**</span></span><br/><br/> <span data-ttu-id="84c78-120">minOccurs = ”0” maxOccurs = ”1”</span><span class="sxs-lookup"><span data-stu-id="84c78-120">minOccurs="0" maxOccurs="1"</span></span> |<span data-ttu-id="84c78-121">En [AssetFile elementet](media-services-output-metadata-schema.md) som ingår i samlingen AssetFiles.</span><span class="sxs-lookup"><span data-stu-id="84c78-121">An [AssetFile element](media-services-output-metadata-schema.md) that is part of the AssetFiles collection.</span></span> |

## <span data-ttu-id="84c78-122"><a name="AssetFile "></a>AssetFile element</span><span class="sxs-lookup"><span data-stu-id="84c78-122"><a name="AssetFile "></a> AssetFile element</span></span>
<span data-ttu-id="84c78-123">Du hittar ett XML-exempel [XML-exempel](media-services-output-metadata-schema.md#xml).</span><span class="sxs-lookup"><span data-stu-id="84c78-123">You can find an XML example [XML example](media-services-output-metadata-schema.md#xml).</span></span>  

### <a name="attributes"></a><span data-ttu-id="84c78-124">Attribut</span><span class="sxs-lookup"><span data-stu-id="84c78-124">Attributes</span></span>
| <span data-ttu-id="84c78-125">Namn</span><span class="sxs-lookup"><span data-stu-id="84c78-125">Name</span></span> | <span data-ttu-id="84c78-126">Typ</span><span class="sxs-lookup"><span data-stu-id="84c78-126">Type</span></span> | <span data-ttu-id="84c78-127">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="84c78-127">Description</span></span> |
| --- | --- | --- |
| <span data-ttu-id="84c78-128">**Namn**</span><span class="sxs-lookup"><span data-stu-id="84c78-128">**Name**</span></span><br/><br/> <span data-ttu-id="84c78-129">Krävs</span><span class="sxs-lookup"><span data-stu-id="84c78-129">Required</span></span> |<span data-ttu-id="84c78-130">**Xs:String**</span><span class="sxs-lookup"><span data-stu-id="84c78-130">**xs:string**</span></span> |<span data-ttu-id="84c78-131">Filnamnet för media tillgången.</span><span class="sxs-lookup"><span data-stu-id="84c78-131">The media asset file name.</span></span> |
| <span data-ttu-id="84c78-132">**Storlek**</span><span class="sxs-lookup"><span data-stu-id="84c78-132">**Size**</span></span><br/><br/> <span data-ttu-id="84c78-133">minInclusive = ”0”</span><span class="sxs-lookup"><span data-stu-id="84c78-133">minInclusive ="0"</span></span><br/><br/> <span data-ttu-id="84c78-134">Krävs</span><span class="sxs-lookup"><span data-stu-id="84c78-134">Required</span></span> |<span data-ttu-id="84c78-135">**Xs:Long**</span><span class="sxs-lookup"><span data-stu-id="84c78-135">**xs:long**</span></span> |<span data-ttu-id="84c78-136">Storleken på resursfilen i byte.</span><span class="sxs-lookup"><span data-stu-id="84c78-136">Size of the asset file in bytes.</span></span> |
| <span data-ttu-id="84c78-137">**Varaktighet**</span><span class="sxs-lookup"><span data-stu-id="84c78-137">**Duration**</span></span><br/><br/> <span data-ttu-id="84c78-138">Krävs</span><span class="sxs-lookup"><span data-stu-id="84c78-138">Required</span></span> |<span data-ttu-id="84c78-139">**Duration**</span><span class="sxs-lookup"><span data-stu-id="84c78-139">**xs:duration**</span></span> |<span data-ttu-id="84c78-140">Innehåll play tillbaka varaktighet.</span><span class="sxs-lookup"><span data-stu-id="84c78-140">Content play back duration.</span></span> |

### <a name="child-elements"></a><span data-ttu-id="84c78-141">Underordnade element</span><span class="sxs-lookup"><span data-stu-id="84c78-141">Child elements</span></span>
| <span data-ttu-id="84c78-142">Namn</span><span class="sxs-lookup"><span data-stu-id="84c78-142">Name</span></span> | <span data-ttu-id="84c78-143">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="84c78-143">Description</span></span> |
| --- | --- |
| <span data-ttu-id="84c78-144">**Datakällor**</span><span class="sxs-lookup"><span data-stu-id="84c78-144">**Sources**</span></span> |<span data-ttu-id="84c78-145">Samling av indatakälla/mediefiler som bearbetades för att skapa den här AssetFile.</span><span class="sxs-lookup"><span data-stu-id="84c78-145">Collection of input/source media files, that was processed in order to produce this AssetFile.</span></span> <span data-ttu-id="84c78-146">Mer information finns i [källelementet](media-services-output-metadata-schema.md).</span><span class="sxs-lookup"><span data-stu-id="84c78-146">For more information, see [Source element](media-services-output-metadata-schema.md).</span></span> |
| <span data-ttu-id="84c78-147">**VideoTracks**</span><span class="sxs-lookup"><span data-stu-id="84c78-147">**VideoTracks**</span></span><br/><br/> <span data-ttu-id="84c78-148">minOccurs = ”0” maxOccurs = ”1”</span><span class="sxs-lookup"><span data-stu-id="84c78-148">minOccurs="0" maxOccurs="1"</span></span> |<span data-ttu-id="84c78-149">Varje fysisk AssetFile kan innehålla noll eller flera video spårar överlagrat till en lämplig behållare format i den.</span><span class="sxs-lookup"><span data-stu-id="84c78-149">Each physical AssetFile can contain in it zero or more video tracks interleaved into an appropriate container format.</span></span> <span data-ttu-id="84c78-150">Detta är en samling av alla video spår.</span><span class="sxs-lookup"><span data-stu-id="84c78-150">This is the collection of all those video tracks.</span></span> <span data-ttu-id="84c78-151">Mer information finns i [VideoTracks elementet](media-services-output-metadata-schema.md).</span><span class="sxs-lookup"><span data-stu-id="84c78-151">For more information, see [VideoTracks element](media-services-output-metadata-schema.md).</span></span> |
| <span data-ttu-id="84c78-152">**AudioTracks**</span><span class="sxs-lookup"><span data-stu-id="84c78-152">**AudioTracks**</span></span><br/><br/> <span data-ttu-id="84c78-153">minOccurs = ”0” maxOccurs = ”1”</span><span class="sxs-lookup"><span data-stu-id="84c78-153">minOccurs="0" maxOccurs="1"</span></span> |<span data-ttu-id="84c78-154">Varje fysisk AssetFile kan innehålla noll eller flera ljud spårar överlagrat till en lämplig behållare format i den.</span><span class="sxs-lookup"><span data-stu-id="84c78-154">Each physical AssetFile can contain in it zero or more audio tracks interleaved into an appropriate container format.</span></span> <span data-ttu-id="84c78-155">Detta är en samling av alla ljud spår.</span><span class="sxs-lookup"><span data-stu-id="84c78-155">This is the collection of all those audio tracks.</span></span> <span data-ttu-id="84c78-156">Mer information finns i [AudioTracks elementet](media-services-output-metadata-schema.md).</span><span class="sxs-lookup"><span data-stu-id="84c78-156">For more information, see [AudioTracks element](media-services-output-metadata-schema.md).</span></span> |

## <span data-ttu-id="84c78-157"><a name="Sources "></a>Källor element</span><span class="sxs-lookup"><span data-stu-id="84c78-157"><a name="Sources "></a> Sources element</span></span>
<span data-ttu-id="84c78-158">Samling av indatakälla/mediefiler som bearbetades för att skapa den här AssetFile.</span><span class="sxs-lookup"><span data-stu-id="84c78-158">Collection of input/source media files, that was processed in order to produce this AssetFile.</span></span>  

<span data-ttu-id="84c78-159">Du hittar ett XML-exempel [XML-exempel](media-services-output-metadata-schema.md#xml).</span><span class="sxs-lookup"><span data-stu-id="84c78-159">You can find an XML example [XML example](media-services-output-metadata-schema.md#xml).</span></span>  

### <a name="child-elements"></a><span data-ttu-id="84c78-160">Underordnade element</span><span class="sxs-lookup"><span data-stu-id="84c78-160">Child elements</span></span>
| <span data-ttu-id="84c78-161">Namn</span><span class="sxs-lookup"><span data-stu-id="84c78-161">Name</span></span> | <span data-ttu-id="84c78-162">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="84c78-162">Description</span></span> |
| --- | --- |
| <span data-ttu-id="84c78-163">**Källa**</span><span class="sxs-lookup"><span data-stu-id="84c78-163">**Source**</span></span><br/><br/> <span data-ttu-id="84c78-164">minOccurs = ”1” maxOccurs = ”unbounded”</span><span class="sxs-lookup"><span data-stu-id="84c78-164">minOccurs="1" maxOccurs="unbounded"</span></span> |<span data-ttu-id="84c78-165">En indata/källfilerna som används vid generering av tillgången.</span><span class="sxs-lookup"><span data-stu-id="84c78-165">An input/source file used when generating this asset.</span></span> <span data-ttu-id="84c78-166">Mer information finns i [källelementet](media-services-output-metadata-schema.md).</span><span class="sxs-lookup"><span data-stu-id="84c78-166">For more information see [Source element](media-services-output-metadata-schema.md).</span></span> |

## <span data-ttu-id="84c78-167"><a name="Source "></a>Källelementet</span><span class="sxs-lookup"><span data-stu-id="84c78-167"><a name="Source "></a> Source element</span></span>
<span data-ttu-id="84c78-168">En indata/källfilerna som används vid generering av tillgången.</span><span class="sxs-lookup"><span data-stu-id="84c78-168">An input/source file used when generating this asset.</span></span>  

<span data-ttu-id="84c78-169">Du hittar ett XML-exempel [XML-exempel](media-services-output-metadata-schema.md#xml).</span><span class="sxs-lookup"><span data-stu-id="84c78-169">You can find an XML example [XML example](media-services-output-metadata-schema.md#xml).</span></span>  

### <a name="attributes"></a><span data-ttu-id="84c78-170">Attribut</span><span class="sxs-lookup"><span data-stu-id="84c78-170">Attributes</span></span>
| <span data-ttu-id="84c78-171">Namn</span><span class="sxs-lookup"><span data-stu-id="84c78-171">Name</span></span> | <span data-ttu-id="84c78-172">Typ</span><span class="sxs-lookup"><span data-stu-id="84c78-172">Type</span></span> | <span data-ttu-id="84c78-173">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="84c78-173">Description</span></span> |
| --- | --- | --- |
| <span data-ttu-id="84c78-174">**Namn**</span><span class="sxs-lookup"><span data-stu-id="84c78-174">**Name**</span></span><br/><br/> <span data-ttu-id="84c78-175">Krävs</span><span class="sxs-lookup"><span data-stu-id="84c78-175">Required</span></span> |<span data-ttu-id="84c78-176">**Xs:String**</span><span class="sxs-lookup"><span data-stu-id="84c78-176">**xs:string**</span></span> |<span data-ttu-id="84c78-177">Indatakällan filnamn.</span><span class="sxs-lookup"><span data-stu-id="84c78-177">Input source file name.</span></span> |

## <span data-ttu-id="84c78-178"><a name="VideoTracks "></a>VideoTracks element</span><span class="sxs-lookup"><span data-stu-id="84c78-178"><a name="VideoTracks "></a> VideoTracks element</span></span>
<span data-ttu-id="84c78-179">Varje fysisk AssetFile kan innehålla noll eller flera video spårar överlagrat till en lämplig behållare format i den.</span><span class="sxs-lookup"><span data-stu-id="84c78-179">Each physical AssetFile can contain in it zero or more video tracks interleaved into an appropriate container format.</span></span> <span data-ttu-id="84c78-180">Detta är en samling av alla video spår.</span><span class="sxs-lookup"><span data-stu-id="84c78-180">This is the collection of all those video tracks.</span></span>  

<span data-ttu-id="84c78-181">Du hittar ett XML-exempel [XML-exempel](media-services-output-metadata-schema.md#xml).</span><span class="sxs-lookup"><span data-stu-id="84c78-181">You can find an XML example [XML example](media-services-output-metadata-schema.md#xml).</span></span>  

### <a name="child-elements"></a><span data-ttu-id="84c78-182">Underordnade element</span><span class="sxs-lookup"><span data-stu-id="84c78-182">Child elements</span></span>
| <span data-ttu-id="84c78-183">Namn</span><span class="sxs-lookup"><span data-stu-id="84c78-183">Name</span></span> | <span data-ttu-id="84c78-184">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="84c78-184">Description</span></span> |
| --- | --- |
| <span data-ttu-id="84c78-185">**VideoTrack**</span><span class="sxs-lookup"><span data-stu-id="84c78-185">**VideoTrack**</span></span><br/><br/> <span data-ttu-id="84c78-186">minOccurs = ”1” maxOccurs = ”unbounded”</span><span class="sxs-lookup"><span data-stu-id="84c78-186">minOccurs="1" maxOccurs="unbounded"</span></span> |<span data-ttu-id="84c78-187">En specifik video spåra i överordnat AssetFile.</span><span class="sxs-lookup"><span data-stu-id="84c78-187">A specific video track in the parent AssetFile.</span></span> <span data-ttu-id="84c78-188">Mer information finns i [VideoTrack elementet](media-services-output-metadata-schema.md#VideoTrack).</span><span class="sxs-lookup"><span data-stu-id="84c78-188">For more information, see [VideoTrack element](media-services-output-metadata-schema.md#VideoTrack).</span></span> |

## <span data-ttu-id="84c78-189"><a name="VideoTrack"></a>VideoTrack element</span><span class="sxs-lookup"><span data-stu-id="84c78-189"><a name="VideoTrack"></a> VideoTrack element</span></span>
<span data-ttu-id="84c78-190">En specifik video spåra i överordnat AssetFile.</span><span class="sxs-lookup"><span data-stu-id="84c78-190">A specific video track in the parent AssetFile.</span></span>  

<span data-ttu-id="84c78-191">Du hittar ett XML-exempel [XML-exempel](media-services-output-metadata-schema.md#xml).</span><span class="sxs-lookup"><span data-stu-id="84c78-191">You can find an XML example [XML example](media-services-output-metadata-schema.md#xml).</span></span>  

### <a name="attributes"></a><span data-ttu-id="84c78-192">Attribut</span><span class="sxs-lookup"><span data-stu-id="84c78-192">Attributes</span></span>
| <span data-ttu-id="84c78-193">Namn</span><span class="sxs-lookup"><span data-stu-id="84c78-193">Name</span></span> | <span data-ttu-id="84c78-194">Typ</span><span class="sxs-lookup"><span data-stu-id="84c78-194">Type</span></span> | <span data-ttu-id="84c78-195">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="84c78-195">Description</span></span> |
| --- | --- | --- |
| <span data-ttu-id="84c78-196">**ID**</span><span class="sxs-lookup"><span data-stu-id="84c78-196">**Id**</span></span><br/><br/> <span data-ttu-id="84c78-197">minInclusive = ”0”</span><span class="sxs-lookup"><span data-stu-id="84c78-197">minInclusive ="0"</span></span><br/><br/> <span data-ttu-id="84c78-198">Krävs</span><span class="sxs-lookup"><span data-stu-id="84c78-198">Required</span></span> |<span data-ttu-id="84c78-199">**Xs:int**</span><span class="sxs-lookup"><span data-stu-id="84c78-199">**xs:int**</span></span> |<span data-ttu-id="84c78-200">Nollbaserade indexet för den här videon spåra. **Obs:** detta är inte nödvändigtvis TrackID som används i en MP4-fil.</span><span class="sxs-lookup"><span data-stu-id="84c78-200">Zero-based index of this video track. **Note:**  This is not necessarily the TrackID as used in an MP4 file.</span></span> |
| <span data-ttu-id="84c78-201">**FourCC**</span><span class="sxs-lookup"><span data-stu-id="84c78-201">**FourCC**</span></span><br/><br/> <span data-ttu-id="84c78-202">Krävs</span><span class="sxs-lookup"><span data-stu-id="84c78-202">Required</span></span> |<span data-ttu-id="84c78-203">**Xs:String**</span><span class="sxs-lookup"><span data-stu-id="84c78-203">**xs:string**</span></span> |<span data-ttu-id="84c78-204">Video-codec FourCC-kod.</span><span class="sxs-lookup"><span data-stu-id="84c78-204">Video codec FourCC code.</span></span> |
| <span data-ttu-id="84c78-205">**Profil**</span><span class="sxs-lookup"><span data-stu-id="84c78-205">**Profile**</span></span> |<span data-ttu-id="84c78-206">**Xs:String**</span><span class="sxs-lookup"><span data-stu-id="84c78-206">**xs:string**</span></span> |<span data-ttu-id="84c78-207">H264-profil (gäller endast för H264 codec).</span><span class="sxs-lookup"><span data-stu-id="84c78-207">H264 profile (only applicable to H264 codec).</span></span> |
| <span data-ttu-id="84c78-208">**Nivå**</span><span class="sxs-lookup"><span data-stu-id="84c78-208">**Level**</span></span> |<span data-ttu-id="84c78-209">**Xs:String**</span><span class="sxs-lookup"><span data-stu-id="84c78-209">**xs:string**</span></span> |<span data-ttu-id="84c78-210">H264 nivå (gäller endast för H264 codec).</span><span class="sxs-lookup"><span data-stu-id="84c78-210">H264 level (only applicable to H264 codec).</span></span> |
| <span data-ttu-id="84c78-211">**Bredd**</span><span class="sxs-lookup"><span data-stu-id="84c78-211">**Width**</span></span><br/><br/> <span data-ttu-id="84c78-212">minInclusive = ”0”</span><span class="sxs-lookup"><span data-stu-id="84c78-212">minInclusive ="0"</span></span><br/><br/> <span data-ttu-id="84c78-213">Krävs</span><span class="sxs-lookup"><span data-stu-id="84c78-213">Required</span></span> |<span data-ttu-id="84c78-214">**Xs:int**</span><span class="sxs-lookup"><span data-stu-id="84c78-214">**xs:int**</span></span> |<span data-ttu-id="84c78-215">Kodad video bredd i bildpunkter.</span><span class="sxs-lookup"><span data-stu-id="84c78-215">Encoded video width in pixels.</span></span> |
| <span data-ttu-id="84c78-216">**Höjd**</span><span class="sxs-lookup"><span data-stu-id="84c78-216">**Height**</span></span><br/><br/> <span data-ttu-id="84c78-217">minInclusive = ”0”</span><span class="sxs-lookup"><span data-stu-id="84c78-217">minInclusive ="0"</span></span><br/><br/> <span data-ttu-id="84c78-218">Krävs</span><span class="sxs-lookup"><span data-stu-id="84c78-218">Required</span></span> |<span data-ttu-id="84c78-219">**Xs:int**</span><span class="sxs-lookup"><span data-stu-id="84c78-219">**xs:int**</span></span> |<span data-ttu-id="84c78-220">Kodad video höjd i bildpunkter.</span><span class="sxs-lookup"><span data-stu-id="84c78-220">Encoded video height in pixels.</span></span> |
| <span data-ttu-id="84c78-221">**DisplayAspectRatioNumerator**</span><span class="sxs-lookup"><span data-stu-id="84c78-221">**DisplayAspectRatioNumerator**</span></span><br/><br/> <span data-ttu-id="84c78-222">minInclusive = ”0”</span><span class="sxs-lookup"><span data-stu-id="84c78-222">minInclusive ="0"</span></span><br/><br/> <span data-ttu-id="84c78-223">Krävs</span><span class="sxs-lookup"><span data-stu-id="84c78-223">Required</span></span> |<span data-ttu-id="84c78-224">**Xs:Double**</span><span class="sxs-lookup"><span data-stu-id="84c78-224">**xs:double**</span></span> |<span data-ttu-id="84c78-225">Videoenhet proportionerna täljaren.</span><span class="sxs-lookup"><span data-stu-id="84c78-225">Video display aspect ratio numerator.</span></span> |
| <span data-ttu-id="84c78-226">**DisplayAspectRatioDenominator**</span><span class="sxs-lookup"><span data-stu-id="84c78-226">**DisplayAspectRatioDenominator**</span></span><br/><br/> <span data-ttu-id="84c78-227">minInclusive = ”0”</span><span class="sxs-lookup"><span data-stu-id="84c78-227">minInclusive ="0"</span></span><br/><br/> <span data-ttu-id="84c78-228">Krävs</span><span class="sxs-lookup"><span data-stu-id="84c78-228">Required</span></span> |<span data-ttu-id="84c78-229">**Xs:Double**</span><span class="sxs-lookup"><span data-stu-id="84c78-229">**xs:double**</span></span> |<span data-ttu-id="84c78-230">Videoenhet proportionerna nämnare.</span><span class="sxs-lookup"><span data-stu-id="84c78-230">Video display aspect ratio denominator.</span></span> |
| <span data-ttu-id="84c78-231">**Ramhastighet**</span><span class="sxs-lookup"><span data-stu-id="84c78-231">**Framerate**</span></span><br/><br/> <span data-ttu-id="84c78-232">minInclusive = ”0”</span><span class="sxs-lookup"><span data-stu-id="84c78-232">minInclusive ="0"</span></span><br/><br/> <span data-ttu-id="84c78-233">Krävs</span><span class="sxs-lookup"><span data-stu-id="84c78-233">Required</span></span> |<span data-ttu-id="84c78-234">**Xs:decimal**</span><span class="sxs-lookup"><span data-stu-id="84c78-234">**xs:decimal**</span></span> |<span data-ttu-id="84c78-235">Mätt video bildfrekvens .3f format.</span><span class="sxs-lookup"><span data-stu-id="84c78-235">Measured video frame rate in .3f format.</span></span> |
| <span data-ttu-id="84c78-236">**TargetFramerate**</span><span class="sxs-lookup"><span data-stu-id="84c78-236">**TargetFramerate**</span></span><br/><br/> <span data-ttu-id="84c78-237">minInclusive = ”0”</span><span class="sxs-lookup"><span data-stu-id="84c78-237">minInclusive ="0"</span></span><br/><br/> <span data-ttu-id="84c78-238">Krävs</span><span class="sxs-lookup"><span data-stu-id="84c78-238">Required</span></span> |<span data-ttu-id="84c78-239">**Xs:decimal**</span><span class="sxs-lookup"><span data-stu-id="84c78-239">**xs:decimal**</span></span> |<span data-ttu-id="84c78-240">Förinställda mål video bildfrekvens .3f format.</span><span class="sxs-lookup"><span data-stu-id="84c78-240">Preset target video frame rate in .3f format.</span></span> |
| <span data-ttu-id="84c78-241">**Bithastighet**</span><span class="sxs-lookup"><span data-stu-id="84c78-241">**Bitrate**</span></span><br/><br/> <span data-ttu-id="84c78-242">minInclusive = ”0”</span><span class="sxs-lookup"><span data-stu-id="84c78-242">minInclusive ="0"</span></span><br/><br/> <span data-ttu-id="84c78-243">Krävs</span><span class="sxs-lookup"><span data-stu-id="84c78-243">Required</span></span> |<span data-ttu-id="84c78-244">**Xs:int**</span><span class="sxs-lookup"><span data-stu-id="84c78-244">**xs:int**</span></span> |<span data-ttu-id="84c78-245">Genomsnittlig video bithastighet i kilobit per sekund som beräknas från AssetFile.</span><span class="sxs-lookup"><span data-stu-id="84c78-245">Average video bit rate in kilobits per second, as calculated from the AssetFile.</span></span> <span data-ttu-id="84c78-246">Räknar elementära dataströmmen nyttolasten och inkluderar inte paketering kostnader.</span><span class="sxs-lookup"><span data-stu-id="84c78-246">Counts only the elementary stream payload, and does not include the packaging overhead.</span></span> |
| <span data-ttu-id="84c78-247">**TargetBitrate**</span><span class="sxs-lookup"><span data-stu-id="84c78-247">**TargetBitrate**</span></span><br/><br/> <span data-ttu-id="84c78-248">minInclusive = ”0”</span><span class="sxs-lookup"><span data-stu-id="84c78-248">minInclusive ="0"</span></span><br/><br/> <span data-ttu-id="84c78-249">Krävs</span><span class="sxs-lookup"><span data-stu-id="84c78-249">Required</span></span> |<span data-ttu-id="84c78-250">**Xs:int**</span><span class="sxs-lookup"><span data-stu-id="84c78-250">**xs:int**</span></span> |<span data-ttu-id="84c78-251">Rikta genomsnittlig bithastighet för den här videon spår som begärdes via encoding förinställt i kilobit per sekund.</span><span class="sxs-lookup"><span data-stu-id="84c78-251">Target average bitrate for this video track, as requested via the encoding preset, in kilobits per second.</span></span> |
| <span data-ttu-id="84c78-252">**MaxGOPBitrate**</span><span class="sxs-lookup"><span data-stu-id="84c78-252">**MaxGOPBitrate**</span></span><br/><br/> <span data-ttu-id="84c78-253">minInclusive = ”0”</span><span class="sxs-lookup"><span data-stu-id="84c78-253">minInclusive ="0"</span></span> |<span data-ttu-id="84c78-254">**Xs:int**</span><span class="sxs-lookup"><span data-stu-id="84c78-254">**xs:int**</span></span> |<span data-ttu-id="84c78-255">Max GOP genomsnittlig bithastighet för den här videon spår i kilobit per sekund.</span><span class="sxs-lookup"><span data-stu-id="84c78-255">Max GOP average bitrate for this video track, in kilobits per second.</span></span> |

## <span data-ttu-id="84c78-256"><a name="AudioTracks "></a>AudioTracks element</span><span class="sxs-lookup"><span data-stu-id="84c78-256"><a name="AudioTracks "></a> AudioTracks element</span></span>
<span data-ttu-id="84c78-257">Varje fysisk AssetFile kan innehålla noll eller flera ljud spårar överlagrat till en lämplig behållare format i den.</span><span class="sxs-lookup"><span data-stu-id="84c78-257">Each physical AssetFile can contain in it zero or more audio tracks interleaved into an appropriate container format.</span></span> <span data-ttu-id="84c78-258">Detta är en samling av alla ljud spår.</span><span class="sxs-lookup"><span data-stu-id="84c78-258">This is the collection of all those audio tracks.</span></span>  

<span data-ttu-id="84c78-259">Du hittar ett XML-exempel [XML-exempel](media-services-output-metadata-schema.md#xml).</span><span class="sxs-lookup"><span data-stu-id="84c78-259">You can find an XML example [XML example](media-services-output-metadata-schema.md#xml).</span></span>  

### <a name="child-elements"></a><span data-ttu-id="84c78-260">Underordnade element</span><span class="sxs-lookup"><span data-stu-id="84c78-260">Child elements</span></span>
| <span data-ttu-id="84c78-261">Namn</span><span class="sxs-lookup"><span data-stu-id="84c78-261">Name</span></span> | <span data-ttu-id="84c78-262">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="84c78-262">Description</span></span> |
| --- | --- |
| <span data-ttu-id="84c78-263">**AudioTrack**</span><span class="sxs-lookup"><span data-stu-id="84c78-263">**AudioTrack**</span></span><br/><br/> <span data-ttu-id="84c78-264">minOccurs = ”1” maxOccurs = ”unbounded”</span><span class="sxs-lookup"><span data-stu-id="84c78-264">minOccurs="1" maxOccurs="unbounded"</span></span> |<span data-ttu-id="84c78-265">En specifik ljud spåra i överordnat AssetFile.</span><span class="sxs-lookup"><span data-stu-id="84c78-265">A specific audio track in the parent AssetFile.</span></span> <span data-ttu-id="84c78-266">Mer information finns i [AudioTrack elementet](media-services-output-metadata-schema.md).</span><span class="sxs-lookup"><span data-stu-id="84c78-266">For more information, see [AudioTrack element](media-services-output-metadata-schema.md).</span></span> |

## <span data-ttu-id="84c78-267"><a name="AudioTrack "></a>AudioTrack element</span><span class="sxs-lookup"><span data-stu-id="84c78-267"><a name="AudioTrack "></a> AudioTrack element</span></span>
<span data-ttu-id="84c78-268">En specifik ljud spåra i överordnat AssetFile.</span><span class="sxs-lookup"><span data-stu-id="84c78-268">A specific audio track in the parent AssetFile.</span></span>  

<span data-ttu-id="84c78-269">Du hittar ett XML-exempel [XML-exempel](media-services-output-metadata-schema.md#xml).</span><span class="sxs-lookup"><span data-stu-id="84c78-269">You can find an XML example [XML example](media-services-output-metadata-schema.md#xml).</span></span>  

### <a name="attributes"></a><span data-ttu-id="84c78-270">Attribut</span><span class="sxs-lookup"><span data-stu-id="84c78-270">Attributes</span></span>
| <span data-ttu-id="84c78-271">Namn</span><span class="sxs-lookup"><span data-stu-id="84c78-271">Name</span></span> | <span data-ttu-id="84c78-272">Typ</span><span class="sxs-lookup"><span data-stu-id="84c78-272">Type</span></span> | <span data-ttu-id="84c78-273">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="84c78-273">Description</span></span> |
| --- | --- | --- |
| <span data-ttu-id="84c78-274">**ID**</span><span class="sxs-lookup"><span data-stu-id="84c78-274">**Id**</span></span><br/><br/> <span data-ttu-id="84c78-275">minInclusive = ”0”</span><span class="sxs-lookup"><span data-stu-id="84c78-275">minInclusive ="0"</span></span><br/><br/> <span data-ttu-id="84c78-276">Krävs</span><span class="sxs-lookup"><span data-stu-id="84c78-276">Required</span></span> |<span data-ttu-id="84c78-277">**Xs:int**</span><span class="sxs-lookup"><span data-stu-id="84c78-277">**xs:int**</span></span> |<span data-ttu-id="84c78-278">Nollbaserade indexet för den här ljud spåra. **Obs:** detta är inte nödvändigtvis TrackID som används i en MP4-fil.</span><span class="sxs-lookup"><span data-stu-id="84c78-278">Zero-based index of this audio track. **Note:**  This is not necessarily the TrackID as used in an MP4 file.</span></span> |
| <span data-ttu-id="84c78-279">**Codec**</span><span class="sxs-lookup"><span data-stu-id="84c78-279">**Codec**</span></span> |<span data-ttu-id="84c78-280">**Xs:String**</span><span class="sxs-lookup"><span data-stu-id="84c78-280">**xs:string**</span></span> |<span data-ttu-id="84c78-281">Ljud spåra codec sträng.</span><span class="sxs-lookup"><span data-stu-id="84c78-281">Audio track codec string.</span></span> |
| <span data-ttu-id="84c78-282">**EncoderVersion**</span><span class="sxs-lookup"><span data-stu-id="84c78-282">**EncoderVersion**</span></span> |<span data-ttu-id="84c78-283">**Xs:String**</span><span class="sxs-lookup"><span data-stu-id="84c78-283">**xs:string**</span></span> |<span data-ttu-id="84c78-284">Versionssträng valfria kodare som krävs för EAC3.</span><span class="sxs-lookup"><span data-stu-id="84c78-284">Optional encoder version string, required for EAC3.</span></span> |
| <span data-ttu-id="84c78-285">**Kanaler**</span><span class="sxs-lookup"><span data-stu-id="84c78-285">**Channels**</span></span><br/><br/> <span data-ttu-id="84c78-286">minInclusive = ”0”</span><span class="sxs-lookup"><span data-stu-id="84c78-286">minInclusive ="0"</span></span><br/><br/> <span data-ttu-id="84c78-287">Krävs</span><span class="sxs-lookup"><span data-stu-id="84c78-287">Required</span></span> |<span data-ttu-id="84c78-288">**Xs:int**</span><span class="sxs-lookup"><span data-stu-id="84c78-288">**xs:int**</span></span> |<span data-ttu-id="84c78-289">Antal ljud kanaler.</span><span class="sxs-lookup"><span data-stu-id="84c78-289">Number of audio channels.</span></span> |
| <span data-ttu-id="84c78-290">**SamplingRate**</span><span class="sxs-lookup"><span data-stu-id="84c78-290">**SamplingRate**</span></span><br/><br/> <span data-ttu-id="84c78-291">minInclusive = ”0”</span><span class="sxs-lookup"><span data-stu-id="84c78-291">minInclusive ="0"</span></span><br/><br/> <span data-ttu-id="84c78-292">Krävs</span><span class="sxs-lookup"><span data-stu-id="84c78-292">Required</span></span> |<span data-ttu-id="84c78-293">**Xs:int**</span><span class="sxs-lookup"><span data-stu-id="84c78-293">**xs:int**</span></span> |<span data-ttu-id="84c78-294">I prover per sekund eller Hz ljud samplingsfrekvensen.</span><span class="sxs-lookup"><span data-stu-id="84c78-294">Audio sampling rate in samples/sec or Hz.</span></span> |
| <span data-ttu-id="84c78-295">**Bithastighet**</span><span class="sxs-lookup"><span data-stu-id="84c78-295">**Bitrate**</span></span><br/><br/> <span data-ttu-id="84c78-296">minInclusive = ”0”</span><span class="sxs-lookup"><span data-stu-id="84c78-296">minInclusive ="0"</span></span><br/><br/> <span data-ttu-id="84c78-297">Krävs</span><span class="sxs-lookup"><span data-stu-id="84c78-297">Required</span></span> |<span data-ttu-id="84c78-298">**Xs:int**</span><span class="sxs-lookup"><span data-stu-id="84c78-298">**xs:int**</span></span> |<span data-ttu-id="84c78-299">Genomsnittlig ljud bithastighet i bitar per sekund som beräknas från AssetFile.</span><span class="sxs-lookup"><span data-stu-id="84c78-299">Average audio bit rate in bits per second, as calculated from the AssetFile.</span></span> <span data-ttu-id="84c78-300">Räknar elementära dataströmmen nyttolasten och inkluderar inte paketering kostnader.</span><span class="sxs-lookup"><span data-stu-id="84c78-300">Counts only the elementary stream payload, and does not include the packaging overhead.</span></span> |
| <span data-ttu-id="84c78-301">**BitsPerSample**</span><span class="sxs-lookup"><span data-stu-id="84c78-301">**BitsPerSample**</span></span><br/><br/> <span data-ttu-id="84c78-302">minInclusive = ”0”</span><span class="sxs-lookup"><span data-stu-id="84c78-302">minInclusive ="0"</span></span><br/><br/> <span data-ttu-id="84c78-303">Krävs</span><span class="sxs-lookup"><span data-stu-id="84c78-303">Required</span></span> |<span data-ttu-id="84c78-304">**Xs:int**</span><span class="sxs-lookup"><span data-stu-id="84c78-304">**xs:int**</span></span> |<span data-ttu-id="84c78-305">Bitar per prov för formatet wFormatTag typ.</span><span class="sxs-lookup"><span data-stu-id="84c78-305">Bits per sample for the wFormatTag format type.</span></span> |

### <a name="child-elements"></a><span data-ttu-id="84c78-306">Underordnade element</span><span class="sxs-lookup"><span data-stu-id="84c78-306">Child elements</span></span>
| <span data-ttu-id="84c78-307">Namn</span><span class="sxs-lookup"><span data-stu-id="84c78-307">Name</span></span> | <span data-ttu-id="84c78-308">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="84c78-308">Description</span></span> |
| --- | --- |
| <span data-ttu-id="84c78-309">**LoudnessMeteringResultParameters**</span><span class="sxs-lookup"><span data-stu-id="84c78-309">**LoudnessMeteringResultParameters**</span></span><br/><br/> <span data-ttu-id="84c78-310">minOccurs = ”0” maxOccurs = ”1”</span><span class="sxs-lookup"><span data-stu-id="84c78-310">minOccurs="0" maxOccurs="1"</span></span> |<span data-ttu-id="84c78-311">Loudness avläsning resultatet parametrar.</span><span class="sxs-lookup"><span data-stu-id="84c78-311">Loudness metering result parameters.</span></span> <span data-ttu-id="84c78-312">Mer information finns i [LoudnessMeteringResultParameters elementet](media-services-output-metadata-schema.md).</span><span class="sxs-lookup"><span data-stu-id="84c78-312">For more information, see [LoudnessMeteringResultParameters element](media-services-output-metadata-schema.md).</span></span> |

## <span data-ttu-id="84c78-313"><a name="LoudnessMeteringResultParameters "></a>LoudnessMeteringResultParameters element</span><span class="sxs-lookup"><span data-stu-id="84c78-313"><a name="LoudnessMeteringResultParameters "></a> LoudnessMeteringResultParameters element</span></span>
<span data-ttu-id="84c78-314">Loudness avläsning resultatet parametrar.</span><span class="sxs-lookup"><span data-stu-id="84c78-314">Loudness metering result parameters.</span></span>  

<span data-ttu-id="84c78-315">Du hittar ett XML-exempel [XML-exempel](media-services-output-metadata-schema.md#xml).</span><span class="sxs-lookup"><span data-stu-id="84c78-315">You can find an XML example [XML example](media-services-output-metadata-schema.md#xml).</span></span>  

### <a name="attributes"></a><span data-ttu-id="84c78-316">Attribut</span><span class="sxs-lookup"><span data-stu-id="84c78-316">Attributes</span></span>
| <span data-ttu-id="84c78-317">Namn</span><span class="sxs-lookup"><span data-stu-id="84c78-317">Name</span></span> | <span data-ttu-id="84c78-318">Typ</span><span class="sxs-lookup"><span data-stu-id="84c78-318">Type</span></span> | <span data-ttu-id="84c78-319">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="84c78-319">Description</span></span> |
| --- | --- | --- |
| <span data-ttu-id="84c78-320">**DPLMVersionInformation**</span><span class="sxs-lookup"><span data-stu-id="84c78-320">**DPLMVersionInformation**</span></span> |<span data-ttu-id="84c78-321">**Xs:String**</span><span class="sxs-lookup"><span data-stu-id="84c78-321">**xs:string**</span></span> |<span data-ttu-id="84c78-322">**Dolby** professional loudness avläsning development kit version.</span><span class="sxs-lookup"><span data-stu-id="84c78-322">**Dolby** professional loudness metering development kit version.</span></span> |
| <span data-ttu-id="84c78-323">**DialogNormalization**</span><span class="sxs-lookup"><span data-stu-id="84c78-323">**DialogNormalization**</span></span><br/><br/> <span data-ttu-id="84c78-324">minInclusive = ”-31” maxInclusive = ”-1”</span><span class="sxs-lookup"><span data-stu-id="84c78-324">minInclusive="-31" maxInclusive="-1"</span></span><br/><br/> <span data-ttu-id="84c78-325">Krävs</span><span class="sxs-lookup"><span data-stu-id="84c78-325">Required</span></span> |<span data-ttu-id="84c78-326">**Xs:int**</span><span class="sxs-lookup"><span data-stu-id="84c78-326">**xs:int**</span></span> |<span data-ttu-id="84c78-327">DialogNormalization genereras genom DPLM, krävs när LoudnessMetering har angetts</span><span class="sxs-lookup"><span data-stu-id="84c78-327">DialogNormalization generated through DPLM, required when LoudnessMetering is set</span></span> |
| <span data-ttu-id="84c78-328">**IntegratedLoudness**</span><span class="sxs-lookup"><span data-stu-id="84c78-328">**IntegratedLoudness**</span></span><br/><br/> <span data-ttu-id="84c78-329">minInclusive = ”-70” maxInclusive = ”10”</span><span class="sxs-lookup"><span data-stu-id="84c78-329">minInclusive="-70" maxInclusive="10"</span></span><br/><br/> <span data-ttu-id="84c78-330">Krävs</span><span class="sxs-lookup"><span data-stu-id="84c78-330">Required</span></span> |<span data-ttu-id="84c78-331">**Xs:float**</span><span class="sxs-lookup"><span data-stu-id="84c78-331">**xs:float**</span></span> |<span data-ttu-id="84c78-332">Integrerad loudness</span><span class="sxs-lookup"><span data-stu-id="84c78-332">Integrated loudness</span></span> |
| <span data-ttu-id="84c78-333">**IntegratedLoudnessUnit**</span><span class="sxs-lookup"><span data-stu-id="84c78-333">**IntegratedLoudnessUnit**</span></span><br/><br/> <span data-ttu-id="84c78-334">Krävs</span><span class="sxs-lookup"><span data-stu-id="84c78-334">Required</span></span> |<span data-ttu-id="84c78-335">**Xs:String**</span><span class="sxs-lookup"><span data-stu-id="84c78-335">**xs:string**</span></span> |<span data-ttu-id="84c78-336">Integrerad loudness enhet.</span><span class="sxs-lookup"><span data-stu-id="84c78-336">Integrated loudness unit.</span></span> |
| <span data-ttu-id="84c78-337">**IntegratedLoudnessGatingMethod**</span><span class="sxs-lookup"><span data-stu-id="84c78-337">**IntegratedLoudnessGatingMethod**</span></span><br/><br/> <span data-ttu-id="84c78-338">Krävs</span><span class="sxs-lookup"><span data-stu-id="84c78-338">Required</span></span> |<span data-ttu-id="84c78-339">**Xs:String**</span><span class="sxs-lookup"><span data-stu-id="84c78-339">**xs:string**</span></span> |<span data-ttu-id="84c78-340">Gating identifierare</span><span class="sxs-lookup"><span data-stu-id="84c78-340">Gating identifier</span></span> |
| <span data-ttu-id="84c78-341">**IntegratedLoudnessSpeechPercentage**</span><span class="sxs-lookup"><span data-stu-id="84c78-341">**IntegratedLoudnessSpeechPercentage**</span></span><br/><br/> <span data-ttu-id="84c78-342">minInclusive = ”0” maxInclusive = ”100”</span><span class="sxs-lookup"><span data-stu-id="84c78-342">minInclusive ="0" maxInclusive="100"</span></span> |<span data-ttu-id="84c78-343">**Xs:float**</span><span class="sxs-lookup"><span data-stu-id="84c78-343">**xs:float**</span></span> |<span data-ttu-id="84c78-344">Taligenkänning innehåll via programmet, i procent.</span><span class="sxs-lookup"><span data-stu-id="84c78-344">Speech content over the program, as a percentage.</span></span> |
| <span data-ttu-id="84c78-345">**SamplePeak**</span><span class="sxs-lookup"><span data-stu-id="84c78-345">**SamplePeak**</span></span><br/><br/> <span data-ttu-id="84c78-346">Krävs</span><span class="sxs-lookup"><span data-stu-id="84c78-346">Required</span></span> |<span data-ttu-id="84c78-347">**Xs:float**</span><span class="sxs-lookup"><span data-stu-id="84c78-347">**xs:float**</span></span> |<span data-ttu-id="84c78-348">Belastning absolut exempelvärde avmarkerad sedan återställning eller sedan den startades senast per kanal.</span><span class="sxs-lookup"><span data-stu-id="84c78-348">Peak absolute sample value, since reset or since it was last cleared, per channel.</span></span>  <span data-ttu-id="84c78-349">Enheterna är dBFS.</span><span class="sxs-lookup"><span data-stu-id="84c78-349">Units are dBFS.</span></span> |
| <span data-ttu-id="84c78-350">**SamplePeakUnit**</span><span class="sxs-lookup"><span data-stu-id="84c78-350">**SamplePeakUnit**</span></span><br/><br/> <span data-ttu-id="84c78-351">fast = ”dBFS”</span><span class="sxs-lookup"><span data-stu-id="84c78-351">fixed="dBFS"</span></span><br/><br/> <span data-ttu-id="84c78-352">Krävs</span><span class="sxs-lookup"><span data-stu-id="84c78-352">Required</span></span> |<span data-ttu-id="84c78-353">**Xs:anySimpleType**</span><span class="sxs-lookup"><span data-stu-id="84c78-353">**xs:anySimpleType**</span></span> |<span data-ttu-id="84c78-354">Exempel resursbehov.</span><span class="sxs-lookup"><span data-stu-id="84c78-354">Sample peak unit.</span></span> |
| <span data-ttu-id="84c78-355">**TruePeak**</span><span class="sxs-lookup"><span data-stu-id="84c78-355">**TruePeak**</span></span><br/><br/> <span data-ttu-id="84c78-356">Krävs</span><span class="sxs-lookup"><span data-stu-id="84c78-356">Required</span></span> |<span data-ttu-id="84c78-357">**Xs:float**</span><span class="sxs-lookup"><span data-stu-id="84c78-357">**xs:float**</span></span> |<span data-ttu-id="84c78-358">Maximala true högsta värde, enligt ITU-R BS.1770-2, sedan återställning eller sedan den startades senast avmarkerad per kanal.</span><span class="sxs-lookup"><span data-stu-id="84c78-358">Maximum true peak value, as per ITU-R BS.1770-2, since reset or since it was last cleared, per channel.</span></span> <span data-ttu-id="84c78-359">Enheterna är dBTP.</span><span class="sxs-lookup"><span data-stu-id="84c78-359">Units are dBTP.</span></span> |
| <span data-ttu-id="84c78-360">**TruePeakUnit**</span><span class="sxs-lookup"><span data-stu-id="84c78-360">**TruePeakUnit**</span></span><br/><br/> <span data-ttu-id="84c78-361">fast = ”dBTP”</span><span class="sxs-lookup"><span data-stu-id="84c78-361">fixed="dBTP"</span></span><br/><br/> <span data-ttu-id="84c78-362">Krävs</span><span class="sxs-lookup"><span data-stu-id="84c78-362">Required</span></span> |<span data-ttu-id="84c78-363">**Xs:anySimpleType**</span><span class="sxs-lookup"><span data-stu-id="84c78-363">**xs:anySimpleType**</span></span> |<span data-ttu-id="84c78-364">True resursbehov.</span><span class="sxs-lookup"><span data-stu-id="84c78-364">True peak unit.</span></span> |

## <a name="schema-code"></a><span data-ttu-id="84c78-365">Schemat kod</span><span class="sxs-lookup"><span data-stu-id="84c78-365">Schema Code</span></span>
    <?xml version="1.0" encoding="utf-8"?>  
    <xs:schema xmlns:xs="http://www.w3.org/2001/XMLSchema" xmlns:msdata="urn:schemas-microsoft-com:xml-msdata" version="1.2"  
               xmlns="http://schemas.microsoft.com/windowsazure/mediaservices/2013/05/mediaencoder/metadata"  
               targetNamespace="http://schemas.microsoft.com/windowsazure/mediaservices/2013/05/mediaencoder/metadata"  
               elementFormDefault="qualified">  
      <xs:element name="AssetFiles">  
        <xs:annotation>  
          <xs:documentation>Collection of AssetFile entries for the encoding job</xs:documentation>  
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
                      <xs:documentation>Collection of input/source media files, that was processed in order to produce this AssetFile</xs:documentation>  
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
                      <xs:documentation>Each physical AssetFile can contain in it zero or more video tracks interleaved into an appropriate container format. This is the collection of all those video tracks</xs:documentation>  
                    </xs:annotation>  
                    <xs:complexType>  
                      <xs:sequence>  
                        <xs:element name="VideoTrack" maxOccurs="unbounded">  
                          <xs:annotation>  
                            <xs:documentation>A specific video track in the parent AssetFile</xs:documentation>  
                          </xs:annotation>  
                          <xs:complexType>  
                            <xs:attribute name="Id" use="required">  
                              <xs:annotation>  
                                <xs:documentation>zero-based index of this video track. Note: this is not necessarily the TrackID as used in an MP4 file</xs:documentation>  
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
                                <xs:documentation>average video bit rate in kilobits per second, as calculated from the AssetFile. Counts only the elementary stream payload, and does not include the packaging overhead</xs:documentation>  
                              </xs:annotation>  
                              <xs:simpleType>  
                                <xs:restriction base="xs:int">  
                                  <xs:minInclusive value="0"/>  
                                </xs:restriction>  
                              </xs:simpleType>  
                            </xs:attribute>  
                            <xs:attribute name="TargetBitrate" use="required">  
                              <xs:annotation>  
                                <xs:documentation>target average bitrate for this video track, as requested via the encoding preset, in kilobits per second</xs:documentation>  
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
                      <xs:documentation>each physical AssetFile can contain in it zero or more audio tracks interleaved into an appropriate container format. This is the collection of all those audio tracks</xs:documentation>  
                    </xs:annotation>  
                    <xs:complexType>  
                      <xs:sequence>  
                        <xs:element name="AudioTrack" maxOccurs="unbounded">  
                          <xs:annotation>  
                            <xs:documentation>a specific audio track in the parent AssetFile</xs:documentation>  
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
                                      <xs:documentation>Speech content over the program, as a percentage.</xs:documentation>  
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
                                <xs:documentation>zero-based index of this audio track. Note: this is not necessarily the TrackID as used in an MP4 file</xs:documentation>  
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
                                <xs:documentation>average audio bit rate in bits per second, as calculated from the AssetFile. Counts only the elementary stream payload, and does not include the packaging overhead</xs:documentation>  
                              </xs:annotation>  
                              <xs:simpleType>  
                                <xs:restriction base="xs:int">  
                                  <xs:minInclusive value="0"/>  
                                </xs:restriction>  
                              </xs:simpleType>  
                            </xs:attribute>  
                            <xs:attribute name="BitsPerSample" use="required">  
                              <xs:annotation>  
                                <xs:documentation>Bits per sample for the wFormatTag format type</xs:documentation>  
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
                    <xs:documentation>the media asset file name</xs:documentation>  
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



## <span data-ttu-id="84c78-366"><a name="xml"></a>XML-exempel</span><span class="sxs-lookup"><span data-stu-id="84c78-366"><a name="xml"></a> XML example</span></span>
 <span data-ttu-id="84c78-367">Följande är ett exempel på utdata-metadatafil.</span><span class="sxs-lookup"><span data-stu-id="84c78-367">The following is an example of the Output metadata file.</span></span>  

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

## <a name="next-steps"></a><span data-ttu-id="84c78-368">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="84c78-368">Next steps</span></span>
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a><span data-ttu-id="84c78-369">Ge feedback</span><span class="sxs-lookup"><span data-stu-id="84c78-369">Provide feedback</span></span>
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]
