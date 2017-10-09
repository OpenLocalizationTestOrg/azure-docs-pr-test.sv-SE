---
title: aaaAzure Media Services inkommande metadataschema | Microsoft Docs
description: "hello avsnittet ger en översikt över Azure Media Services inkommande metadataschema."
author: Juliako
manager: cfowler
editor: 
services: media-services
documentationcenter: 
ms.assetid: d72848e2-4b65-4c84-94bc-e2a90a6e7f47
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/19/2017
ms.author: juliako
ms.openlocfilehash: 9b72c6ff317aa98451ea75548465dc6023b44a55
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="input-metadata"></a><span data-ttu-id="fe8cb-103">Ange Metadata</span><span class="sxs-lookup"><span data-stu-id="fe8cb-103">Input Metadata</span></span>
<span data-ttu-id="fe8cb-104">Ett kodningsjobb är associerad med en inkommande tillgång (eller tillgångar) som du vill tooperform vissa kodning uppgifter.</span><span class="sxs-lookup"><span data-stu-id="fe8cb-104">An encoding job is associated with an input asset (or assets) on which you want tooperform some encoding tasks.</span></span>  <span data-ttu-id="fe8cb-105">En utdatatillgången skapas vid slutförandet av uppgiften.</span><span class="sxs-lookup"><span data-stu-id="fe8cb-105">Upon completion of a task, an output asset is produced.</span></span>  <span data-ttu-id="fe8cb-106">Hej utdatatillgången innehåller video, ljud, miniatyrbilder manifestet, etc. hello utdatatillgången innehåller också en fil med metadata om hello inkommande tillgången.</span><span class="sxs-lookup"><span data-stu-id="fe8cb-106">hello output asset contains video, audio, thumbnails, manifest, etc. hello output asset also contains a file with metadata about hello input asset.</span></span> <span data-ttu-id="fe8cb-107">hello namnet på XML-filen för hello metadata har hello följande format: &lt;asset_id&gt;_metadata.xml (till exempel 41114ad3-eb5e - 4c 57 8d 92-5354e2b7d4a4_metadata.xml), där &lt;asset_id&gt; är hello AssetId värdet för hello inkommande tillgång.</span><span class="sxs-lookup"><span data-stu-id="fe8cb-107">hello name of hello metadata XML file has hello following format: &lt;asset_id&gt;_metadata.xml (for example, 41114ad3-eb5e-4c57-8d92-5354e2b7d4a4_metadata.xml), where &lt;asset_id&gt; is hello AssetId value of hello input asset.</span></span>  

<span data-ttu-id="fe8cb-108">Om du vill tooexamine hello metadatafil, kan du skapa en **SAS** positionerare och hämta hello filen tooyour lokal dator.</span><span class="sxs-lookup"><span data-stu-id="fe8cb-108">If you want tooexamine hello metadata file, you can create a **SAS** locator and download hello file tooyour local computer.</span></span> <span data-ttu-id="fe8cb-109">Du hittar ett exempel på hur toocreate en SAS-positionerare och hämta en fil [med hello Media Services .NET SDK-tilläggen](media-services-dotnet-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="fe8cb-109">You can find an example on how toocreate a SAS locator and download a file  [Using hello Media Services .NET SDK Extensions](media-services-dotnet-get-started.md).</span></span>  

<span data-ttu-id="fe8cb-110">Det här avsnittet beskrivs hello element och typer av hello XML-schemat på vilka hello inkommande metada (&lt;asset_id&gt;_metadata.xml) är baserad.</span><span class="sxs-lookup"><span data-stu-id="fe8cb-110">This topic discusses hello elements and types of hello XML schema on which hello input metada (&lt;asset_id&gt;_metadata.xml) is based.</span></span>  <span data-ttu-id="fe8cb-111">Information om hello-fil som innehåller metadata om hello utdatatillgången finns [utdata Metadata](media-services-output-metadata-schema.md).</span><span class="sxs-lookup"><span data-stu-id="fe8cb-111">For information about hello file that contains metadata about hello output asset, see [Output Metadata](media-services-output-metadata-schema.md).</span></span>  

> [!NOTE]
> <span data-ttu-id="fe8cb-112">Du kan hitta hello [schemat kod](media-services-input-metadata-schema.md#code) en [XML-exempel](media-services-input-metadata-schema.md#xml) hello slutet av det här avsnittet.</span><span class="sxs-lookup"><span data-stu-id="fe8cb-112">You can find hello [Schema Code](media-services-input-metadata-schema.md#code) an [XML example](media-services-input-metadata-schema.md#xml) at hello end of this topic.</span></span>  
> 
> 

## <span data-ttu-id="fe8cb-113"><a name="AssetFiles"></a>AssetFiles element (rotelementet)</span><span class="sxs-lookup"><span data-stu-id="fe8cb-113"><a name="AssetFiles"></a> AssetFiles element (root element)</span></span>
<span data-ttu-id="fe8cb-114">Innehåller en samling [AssetFile elementet](media-services-input-metadata-schema.md#AssetFile)s för hello kodningsjobbet.</span><span class="sxs-lookup"><span data-stu-id="fe8cb-114">Contains a collection of [AssetFile element](media-services-input-metadata-schema.md#AssetFile)s for hello encoding job.</span></span>  

<span data-ttu-id="fe8cb-115">Se ett XML-exempel hello slutet av det här avsnittet: [XML-exempel](media-services-input-metadata-schema.md#xml).</span><span class="sxs-lookup"><span data-stu-id="fe8cb-115">See an XML example at hello end of this topic: [XML example](media-services-input-metadata-schema.md#xml).</span></span>  

| <span data-ttu-id="fe8cb-116">Namn</span><span class="sxs-lookup"><span data-stu-id="fe8cb-116">Name</span></span> | <span data-ttu-id="fe8cb-117">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="fe8cb-117">Description</span></span> |
| --- | --- |
| <span data-ttu-id="fe8cb-118">**AssetFile**</span><span class="sxs-lookup"><span data-stu-id="fe8cb-118">**AssetFile**</span></span><br /><br /> <span data-ttu-id="fe8cb-119">minOccurs = ”1” maxOccurs = ”unbounded”</span><span class="sxs-lookup"><span data-stu-id="fe8cb-119">minOccurs="1" maxOccurs="unbounded"</span></span> |<span data-ttu-id="fe8cb-120">En enda underordnade element.</span><span class="sxs-lookup"><span data-stu-id="fe8cb-120">A single child element.</span></span> <span data-ttu-id="fe8cb-121">Mer information finns i [AssetFile elementet](media-services-input-metadata-schema.md#AssetFile).</span><span class="sxs-lookup"><span data-stu-id="fe8cb-121">For more information, see [AssetFile element](media-services-input-metadata-schema.md#AssetFile).</span></span> |

## <span data-ttu-id="fe8cb-122"><a name="AssetFile"></a>AssetFile element</span><span class="sxs-lookup"><span data-stu-id="fe8cb-122"><a name="AssetFile"></a> AssetFile element</span></span>
 <span data-ttu-id="fe8cb-123">Innehåller attribut och element som beskriver en resursfil.</span><span class="sxs-lookup"><span data-stu-id="fe8cb-123">Contains attributes and elements that describe an asset file.</span></span>  

 <span data-ttu-id="fe8cb-124">Se ett XML-exempel hello slutet av det här avsnittet: [XML-exempel](media-services-input-metadata-schema.md#xml).</span><span class="sxs-lookup"><span data-stu-id="fe8cb-124">See an XML example at hello end of this topic: [XML example](media-services-input-metadata-schema.md#xml).</span></span>  

### <a name="attributes"></a><span data-ttu-id="fe8cb-125">Attribut</span><span class="sxs-lookup"><span data-stu-id="fe8cb-125">Attributes</span></span>
| <span data-ttu-id="fe8cb-126">Namn</span><span class="sxs-lookup"><span data-stu-id="fe8cb-126">Name</span></span> | <span data-ttu-id="fe8cb-127">Typ</span><span class="sxs-lookup"><span data-stu-id="fe8cb-127">Type</span></span> | <span data-ttu-id="fe8cb-128">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="fe8cb-128">Description</span></span> |
| --- | --- | --- |
| <span data-ttu-id="fe8cb-129">**Namn**</span><span class="sxs-lookup"><span data-stu-id="fe8cb-129">**Name**</span></span><br /><br /> <span data-ttu-id="fe8cb-130">Krävs</span><span class="sxs-lookup"><span data-stu-id="fe8cb-130">Required</span></span> |<span data-ttu-id="fe8cb-131">**Xs:String**</span><span class="sxs-lookup"><span data-stu-id="fe8cb-131">**xs:string**</span></span> |<span data-ttu-id="fe8cb-132">Filnamnet för tillgången.</span><span class="sxs-lookup"><span data-stu-id="fe8cb-132">Asset file name.</span></span> |
| <span data-ttu-id="fe8cb-133">**Storlek**</span><span class="sxs-lookup"><span data-stu-id="fe8cb-133">**Size**</span></span><br /><br /> <span data-ttu-id="fe8cb-134">Krävs</span><span class="sxs-lookup"><span data-stu-id="fe8cb-134">Required</span></span> |<span data-ttu-id="fe8cb-135">**Xs:Long**</span><span class="sxs-lookup"><span data-stu-id="fe8cb-135">**xs:long**</span></span> |<span data-ttu-id="fe8cb-136">Storleken på hello tillgångsfil i byte.</span><span class="sxs-lookup"><span data-stu-id="fe8cb-136">Size of hello asset file in bytes.</span></span> |
| <span data-ttu-id="fe8cb-137">**Varaktighet**</span><span class="sxs-lookup"><span data-stu-id="fe8cb-137">**Duration**</span></span><br /><br /> <span data-ttu-id="fe8cb-138">Krävs</span><span class="sxs-lookup"><span data-stu-id="fe8cb-138">Required</span></span> |<span data-ttu-id="fe8cb-139">**Duration**</span><span class="sxs-lookup"><span data-stu-id="fe8cb-139">**xs:duration**</span></span> |<span data-ttu-id="fe8cb-140">Innehåll play tillbaka varaktighet.</span><span class="sxs-lookup"><span data-stu-id="fe8cb-140">Content play back duration.</span></span> <span data-ttu-id="fe8cb-141">Exempel: Duration = ”PT25M37.757S”.</span><span class="sxs-lookup"><span data-stu-id="fe8cb-141">Example: Duration="PT25M37.757S".</span></span> |
| <span data-ttu-id="fe8cb-142">**NumberOfStreams**</span><span class="sxs-lookup"><span data-stu-id="fe8cb-142">**NumberOfStreams**</span></span><br /><br /> <span data-ttu-id="fe8cb-143">Krävs</span><span class="sxs-lookup"><span data-stu-id="fe8cb-143">Required</span></span> |<span data-ttu-id="fe8cb-144">**Xs:int**</span><span class="sxs-lookup"><span data-stu-id="fe8cb-144">**xs:int**</span></span> |<span data-ttu-id="fe8cb-145">Antal dataströmmar i hello resursfil.</span><span class="sxs-lookup"><span data-stu-id="fe8cb-145">Number of streams in hello asset file.</span></span> |
| <span data-ttu-id="fe8cb-146">**FormatNames**</span><span class="sxs-lookup"><span data-stu-id="fe8cb-146">**FormatNames**</span></span><br /><br /> <span data-ttu-id="fe8cb-147">Krävs</span><span class="sxs-lookup"><span data-stu-id="fe8cb-147">Required</span></span> |<span data-ttu-id="fe8cb-148">**Xs:String**</span><span class="sxs-lookup"><span data-stu-id="fe8cb-148">**xs:string**</span></span> |<span data-ttu-id="fe8cb-149">Formatnamn.</span><span class="sxs-lookup"><span data-stu-id="fe8cb-149">Format names.</span></span> |
| <span data-ttu-id="fe8cb-150">**FormatVerboseNames**</span><span class="sxs-lookup"><span data-stu-id="fe8cb-150">**FormatVerboseNames**</span></span><br /><br /> <span data-ttu-id="fe8cb-151">Krävs</span><span class="sxs-lookup"><span data-stu-id="fe8cb-151">Required</span></span> |<span data-ttu-id="fe8cb-152">**Xs:String**</span><span class="sxs-lookup"><span data-stu-id="fe8cb-152">**xs:string**</span></span> |<span data-ttu-id="fe8cb-153">Utförlig formatnamn.</span><span class="sxs-lookup"><span data-stu-id="fe8cb-153">Format verbose names.</span></span> |
| <span data-ttu-id="fe8cb-154">**StartTime**</span><span class="sxs-lookup"><span data-stu-id="fe8cb-154">**StartTime**</span></span> |<span data-ttu-id="fe8cb-155">**Duration**</span><span class="sxs-lookup"><span data-stu-id="fe8cb-155">**xs:duration**</span></span> |<span data-ttu-id="fe8cb-156">Starttid för innehåll.</span><span class="sxs-lookup"><span data-stu-id="fe8cb-156">Content start time.</span></span> <span data-ttu-id="fe8cb-157">Exempel: StartTime = ”PT2.669S”.</span><span class="sxs-lookup"><span data-stu-id="fe8cb-157">Example: StartTime="PT2.669S".</span></span> |
| <span data-ttu-id="fe8cb-158">**OverallBitRate**</span><span class="sxs-lookup"><span data-stu-id="fe8cb-158">**OverallBitRate**</span></span> |<span data-ttu-id="fe8cb-159">**Xs:int**</span><span class="sxs-lookup"><span data-stu-id="fe8cb-159">**xs:int**</span></span> |<span data-ttu-id="fe8cb-160">Genomsnittlig bithastighet hello tillgången filen i kbit/s.</span><span class="sxs-lookup"><span data-stu-id="fe8cb-160">Average bitrate of hello asset file in kbps.</span></span> |

> [!NOTE]
> <span data-ttu-id="fe8cb-161">hello följande 4 underordnade element måste visas i en sekvens.</span><span class="sxs-lookup"><span data-stu-id="fe8cb-161">hello following 4 child elements must appear in a sequence.</span></span>  
> 
> 

### <a name="child-elements"></a><span data-ttu-id="fe8cb-162">Underordnade element</span><span class="sxs-lookup"><span data-stu-id="fe8cb-162">Child elements</span></span>
| <span data-ttu-id="fe8cb-163">Namn</span><span class="sxs-lookup"><span data-stu-id="fe8cb-163">Name</span></span> | <span data-ttu-id="fe8cb-164">Typ</span><span class="sxs-lookup"><span data-stu-id="fe8cb-164">Type</span></span> | <span data-ttu-id="fe8cb-165">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="fe8cb-165">Description</span></span> |
| --- | --- | --- |
| <span data-ttu-id="fe8cb-166">**Program**</span><span class="sxs-lookup"><span data-stu-id="fe8cb-166">**Programs**</span></span><br /><br /> <span data-ttu-id="fe8cb-167">minOccurs = ”0”</span><span class="sxs-lookup"><span data-stu-id="fe8cb-167">minOccurs="0"</span></span> | |<span data-ttu-id="fe8cb-168">Samling av alla [program elementet](media-services-input-metadata-schema.md#Programs) när hello resursfilen har MPEG-TS-format.</span><span class="sxs-lookup"><span data-stu-id="fe8cb-168">Collection of all [Programs element](media-services-input-metadata-schema.md#Programs) when hello asset file is in MPEG-TS format.</span></span> |
| <span data-ttu-id="fe8cb-169">**VideoTracks**</span><span class="sxs-lookup"><span data-stu-id="fe8cb-169">**VideoTracks**</span></span><br /><br /> <span data-ttu-id="fe8cb-170">minOccurs = ”0”</span><span class="sxs-lookup"><span data-stu-id="fe8cb-170">minOccurs="0"</span></span> | |<span data-ttu-id="fe8cb-171">Varje fysisk tillgång-fil kan innehålla noll eller flera video spårar överlagrat till en lämplig behållare format.</span><span class="sxs-lookup"><span data-stu-id="fe8cb-171">Each physical asset file can contain zero or more video tracks interleaved into an appropriate container format.</span></span> <span data-ttu-id="fe8cb-172">Det här elementet innehåller en samling med alla [VideoTracks elementet](media-services-input-metadata-schema.md#VideoTracks) som ingår i hello resursfil.</span><span class="sxs-lookup"><span data-stu-id="fe8cb-172">This element contains a collection of all [VideoTracks element](media-services-input-metadata-schema.md#VideoTracks) that are part of hello asset file.</span></span> |
| <span data-ttu-id="fe8cb-173">**AudioTracks**</span><span class="sxs-lookup"><span data-stu-id="fe8cb-173">**AudioTracks**</span></span><br /><br /> <span data-ttu-id="fe8cb-174">minOccurs = ”0”</span><span class="sxs-lookup"><span data-stu-id="fe8cb-174">minOccurs="0"</span></span> | |<span data-ttu-id="fe8cb-175">Varje fysisk tillgång-fil kan innehålla noll eller flera ljud spårar överlagrat till en lämplig behållare format.</span><span class="sxs-lookup"><span data-stu-id="fe8cb-175">Each physical asset file can contain zero or more audio tracks interleaved into an appropriate container format.</span></span> <span data-ttu-id="fe8cb-176">Det här elementet innehåller en samling med alla [AudioTracks elementet](media-services-input-metadata-schema.md#AudioTracks) som ingår i hello resursfil.</span><span class="sxs-lookup"><span data-stu-id="fe8cb-176">This element contains a collection of all [AudioTracks element](media-services-input-metadata-schema.md#AudioTracks) that are part of hello asset file.</span></span> |
| <span data-ttu-id="fe8cb-177">**Metadata**</span><span class="sxs-lookup"><span data-stu-id="fe8cb-177">**Metadata**</span></span><br /><br /> <span data-ttu-id="fe8cb-178">minOccurs = ”0” maxOccurs = ”unbounded”</span><span class="sxs-lookup"><span data-stu-id="fe8cb-178">minOccurs="0" maxOccurs="unbounded"</span></span> |[<span data-ttu-id="fe8cb-179">MetadataType</span><span class="sxs-lookup"><span data-stu-id="fe8cb-179">MetadataType</span></span>](media-services-input-metadata-schema.md#MetadataType) |<span data-ttu-id="fe8cb-180">Tillgångsinformation filens metadata representeras som key\value strängar.</span><span class="sxs-lookup"><span data-stu-id="fe8cb-180">Asset file’s metadata represented as key\value strings.</span></span> <span data-ttu-id="fe8cb-181">Exempel:</span><span class="sxs-lookup"><span data-stu-id="fe8cb-181">For example:</span></span><br /><br /> <span data-ttu-id="fe8cb-182">**&lt;Metadatanyckel = ”språk” value = ”eng” /&gt;**</span><span class="sxs-lookup"><span data-stu-id="fe8cb-182">**&lt;Metadata key="language" value="eng" /&gt;**</span></span> |

## <span data-ttu-id="fe8cb-183"><a name="TrackType"></a>TrackType</span><span class="sxs-lookup"><span data-stu-id="fe8cb-183"><a name="TrackType"></a> TrackType</span></span>
<span data-ttu-id="fe8cb-184">Se ett XML-exempel hello slutet av det här avsnittet: [XML-exempel](media-services-input-metadata-schema.md#xml).</span><span class="sxs-lookup"><span data-stu-id="fe8cb-184">See an XML example at hello end of this topic: [XML example](media-services-input-metadata-schema.md#xml).</span></span>  

### <a name="attributes"></a><span data-ttu-id="fe8cb-185">Attribut</span><span class="sxs-lookup"><span data-stu-id="fe8cb-185">Attributes</span></span>
| <span data-ttu-id="fe8cb-186">Namn</span><span class="sxs-lookup"><span data-stu-id="fe8cb-186">Name</span></span> | <span data-ttu-id="fe8cb-187">Typ</span><span class="sxs-lookup"><span data-stu-id="fe8cb-187">Type</span></span> | <span data-ttu-id="fe8cb-188">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="fe8cb-188">Description</span></span> |
| --- | --- | --- |
| <span data-ttu-id="fe8cb-189">**ID**</span><span class="sxs-lookup"><span data-stu-id="fe8cb-189">**Id**</span></span><br /><br /> <span data-ttu-id="fe8cb-190">Krävs</span><span class="sxs-lookup"><span data-stu-id="fe8cb-190">Required</span></span> |<span data-ttu-id="fe8cb-191">**Xs:int**</span><span class="sxs-lookup"><span data-stu-id="fe8cb-191">**xs:int**</span></span> |<span data-ttu-id="fe8cb-192">Nollbaserade indexet för den här ljud- och spåra.</span><span class="sxs-lookup"><span data-stu-id="fe8cb-192">Zero-based index of this audio or video track.</span></span><br /><br /> <span data-ttu-id="fe8cb-193">Detta är inte nödvändigtvis att hello TrackID som används i en MP4-fil.</span><span class="sxs-lookup"><span data-stu-id="fe8cb-193">This is not necessarily that hello TrackID as used in an MP4 file.</span></span> |
| <span data-ttu-id="fe8cb-194">**Codec**</span><span class="sxs-lookup"><span data-stu-id="fe8cb-194">**Codec**</span></span> |<span data-ttu-id="fe8cb-195">**Xs:String**</span><span class="sxs-lookup"><span data-stu-id="fe8cb-195">**xs:string**</span></span> |<span data-ttu-id="fe8cb-196">Video spåra codec sträng.</span><span class="sxs-lookup"><span data-stu-id="fe8cb-196">Video track codec string.</span></span> |
| <span data-ttu-id="fe8cb-197">**CodecLongName**</span><span class="sxs-lookup"><span data-stu-id="fe8cb-197">**CodecLongName**</span></span> |<span data-ttu-id="fe8cb-198">**Xs:String**</span><span class="sxs-lookup"><span data-stu-id="fe8cb-198">**xs:string**</span></span> |<span data-ttu-id="fe8cb-199">Ljud- och spåra codec långt namn.</span><span class="sxs-lookup"><span data-stu-id="fe8cb-199">Audio or video track codec long name.</span></span> |
| <span data-ttu-id="fe8cb-200">**Tidsvärde**</span><span class="sxs-lookup"><span data-stu-id="fe8cb-200">**TimeBase**</span></span><br /><br /> <span data-ttu-id="fe8cb-201">Krävs</span><span class="sxs-lookup"><span data-stu-id="fe8cb-201">Required</span></span> |<span data-ttu-id="fe8cb-202">**Xs:String**</span><span class="sxs-lookup"><span data-stu-id="fe8cb-202">**xs:string**</span></span> |<span data-ttu-id="fe8cb-203">Bas.</span><span class="sxs-lookup"><span data-stu-id="fe8cb-203">Time base.</span></span> <span data-ttu-id="fe8cb-204">Exempel: Tidsvärde = ”1/48000”</span><span class="sxs-lookup"><span data-stu-id="fe8cb-204">Example: TimeBase="1/48000"</span></span> |
| <span data-ttu-id="fe8cb-205">**NumberOfFrames**</span><span class="sxs-lookup"><span data-stu-id="fe8cb-205">**NumberOfFrames**</span></span> |<span data-ttu-id="fe8cb-206">**Xs:int**</span><span class="sxs-lookup"><span data-stu-id="fe8cb-206">**xs:int**</span></span> |<span data-ttu-id="fe8cb-207">Antal bildrutor (finns i video spår).</span><span class="sxs-lookup"><span data-stu-id="fe8cb-207">Number of frames (present for video tracks).</span></span> |
| <span data-ttu-id="fe8cb-208">**StartTime**</span><span class="sxs-lookup"><span data-stu-id="fe8cb-208">**StartTime**</span></span> |<span data-ttu-id="fe8cb-209">**Duration**</span><span class="sxs-lookup"><span data-stu-id="fe8cb-209">**xs:duration**</span></span> |<span data-ttu-id="fe8cb-210">Spåra starttid.</span><span class="sxs-lookup"><span data-stu-id="fe8cb-210">Track start time.</span></span> <span data-ttu-id="fe8cb-211">Exempel: StartTime = ”PT2.669S”</span><span class="sxs-lookup"><span data-stu-id="fe8cb-211">Example: StartTime="PT2.669S"</span></span> |
| <span data-ttu-id="fe8cb-212">**Varaktighet**</span><span class="sxs-lookup"><span data-stu-id="fe8cb-212">**Duration**</span></span> |<span data-ttu-id="fe8cb-213">**Duration**</span><span class="sxs-lookup"><span data-stu-id="fe8cb-213">**xs:duration**</span></span> |<span data-ttu-id="fe8cb-214">Spåra varaktighet.</span><span class="sxs-lookup"><span data-stu-id="fe8cb-214">Track duration.</span></span> <span data-ttu-id="fe8cb-215">Exempel: Duration = ”PTSampleFormat M37.757S”.</span><span class="sxs-lookup"><span data-stu-id="fe8cb-215">Example: Duration="PTSampleFormat M37.757S".</span></span> |

> [!NOTE]
> <span data-ttu-id="fe8cb-216">hello efter 2 underordnade element måste visas i en sekvens.</span><span class="sxs-lookup"><span data-stu-id="fe8cb-216">hello following 2 child elements must appear in a sequence.</span></span>  
> 
> 

### <a name="child-elements"></a><span data-ttu-id="fe8cb-217">Underordnade element</span><span class="sxs-lookup"><span data-stu-id="fe8cb-217">Child elements</span></span>
| <span data-ttu-id="fe8cb-218">Namn</span><span class="sxs-lookup"><span data-stu-id="fe8cb-218">Name</span></span> | <span data-ttu-id="fe8cb-219">Typ</span><span class="sxs-lookup"><span data-stu-id="fe8cb-219">Type</span></span> | <span data-ttu-id="fe8cb-220">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="fe8cb-220">Description</span></span> |
| --- | --- | --- |
| <span data-ttu-id="fe8cb-221">**Disposition**</span><span class="sxs-lookup"><span data-stu-id="fe8cb-221">**Disposition**</span></span><br /><br /> <span data-ttu-id="fe8cb-222">minOccurs = ”0” maxOccurs = ”1”</span><span class="sxs-lookup"><span data-stu-id="fe8cb-222">minOccurs="0" maxOccurs="1"</span></span> |[<span data-ttu-id="fe8cb-223">StreamDispositionType</span><span class="sxs-lookup"><span data-stu-id="fe8cb-223">StreamDispositionType</span></span>](media-services-input-metadata-schema.md#StreamDispositionType) |<span data-ttu-id="fe8cb-224">Innehåller information om presentation (till exempel om en viss ljud spåra är för personer med nedsatt).</span><span class="sxs-lookup"><span data-stu-id="fe8cb-224">Contains presentation information (for example, whether a particular audio track is for visually impaired viewers).</span></span> |
| <span data-ttu-id="fe8cb-225">**Metadata**</span><span class="sxs-lookup"><span data-stu-id="fe8cb-225">**Metadata**</span></span><br /><br /> <span data-ttu-id="fe8cb-226">minOccurs = ”0” maxOccurs = ”unbounded”</span><span class="sxs-lookup"><span data-stu-id="fe8cb-226">minOccurs="0" maxOccurs="unbounded"</span></span> |[<span data-ttu-id="fe8cb-227">MetadataType</span><span class="sxs-lookup"><span data-stu-id="fe8cb-227">MetadataType</span></span>](media-services-input-metadata-schema.md#MetadataType) |<span data-ttu-id="fe8cb-228">Allmän nyckel/värde-strängar som kan använda toohold en mängd information.</span><span class="sxs-lookup"><span data-stu-id="fe8cb-228">Generic key/value strings that can be used toohold a variety of information.</span></span> <span data-ttu-id="fe8cb-229">Till exempel nyckel = ”språk”, och värdet = ”eng”.</span><span class="sxs-lookup"><span data-stu-id="fe8cb-229">For example, key=”language”, and value=”eng”.</span></span> |

## <span data-ttu-id="fe8cb-230"><a name="AudioTrackType"></a>AudioTrackType (ärver från TrackType)</span><span class="sxs-lookup"><span data-stu-id="fe8cb-230"><a name="AudioTrackType"></a> AudioTrackType (inherits from TrackType)</span></span>
 <span data-ttu-id="fe8cb-231">**AudioTrackType** är en global komplex typ som ärver från [TrackType](media-services-input-metadata-schema.md#TrackType).</span><span class="sxs-lookup"><span data-stu-id="fe8cb-231">**AudioTrackType** is a global complex type that inherits from [TrackType](media-services-input-metadata-schema.md#TrackType).</span></span>  

 <span data-ttu-id="fe8cb-232">hello-typen motsvarar en viss ljud spår i hello resursfil.</span><span class="sxs-lookup"><span data-stu-id="fe8cb-232">hello type represents a specific audio track in hello asset file.</span></span>  

 <span data-ttu-id="fe8cb-233">Se ett XML-exempel hello slutet av det här avsnittet: [XML-exempel](media-services-input-metadata-schema.md#xml).</span><span class="sxs-lookup"><span data-stu-id="fe8cb-233">See an XML example at hello end of this topic: [XML example](media-services-input-metadata-schema.md#xml).</span></span>  

### <a name="attributes"></a><span data-ttu-id="fe8cb-234">Attribut</span><span class="sxs-lookup"><span data-stu-id="fe8cb-234">Attributes</span></span>
| <span data-ttu-id="fe8cb-235">Namn</span><span class="sxs-lookup"><span data-stu-id="fe8cb-235">Name</span></span> | <span data-ttu-id="fe8cb-236">Typ</span><span class="sxs-lookup"><span data-stu-id="fe8cb-236">Type</span></span> | <span data-ttu-id="fe8cb-237">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="fe8cb-237">Description</span></span> |
| --- | --- | --- |
| <span data-ttu-id="fe8cb-238">**SampleFormat**</span><span class="sxs-lookup"><span data-stu-id="fe8cb-238">**SampleFormat**</span></span> |<span data-ttu-id="fe8cb-239">**Xs:String**</span><span class="sxs-lookup"><span data-stu-id="fe8cb-239">**xs:string**</span></span> |<span data-ttu-id="fe8cb-240">Exempel-format.</span><span class="sxs-lookup"><span data-stu-id="fe8cb-240">Sample format.</span></span> |
| <span data-ttu-id="fe8cb-241">**ChannelLayout**</span><span class="sxs-lookup"><span data-stu-id="fe8cb-241">**ChannelLayout**</span></span> |<span data-ttu-id="fe8cb-242">**Xs:String**</span><span class="sxs-lookup"><span data-stu-id="fe8cb-242">**xs:string**</span></span> |<span data-ttu-id="fe8cb-243">Kanal layout.</span><span class="sxs-lookup"><span data-stu-id="fe8cb-243">Channel layout.</span></span> |
| <span data-ttu-id="fe8cb-244">**Kanaler**</span><span class="sxs-lookup"><span data-stu-id="fe8cb-244">**Channels**</span></span><br /><br /> <span data-ttu-id="fe8cb-245">Krävs</span><span class="sxs-lookup"><span data-stu-id="fe8cb-245">Required</span></span> |<span data-ttu-id="fe8cb-246">**Xs:int**</span><span class="sxs-lookup"><span data-stu-id="fe8cb-246">**xs:int**</span></span> |<span data-ttu-id="fe8cb-247">Antal (0 eller fler) ljud kanaler.</span><span class="sxs-lookup"><span data-stu-id="fe8cb-247">Number (0 or more) of audio channels.</span></span> |
| <span data-ttu-id="fe8cb-248">**SamplingRate**</span><span class="sxs-lookup"><span data-stu-id="fe8cb-248">**SamplingRate**</span></span><br /><br /> <span data-ttu-id="fe8cb-249">Krävs</span><span class="sxs-lookup"><span data-stu-id="fe8cb-249">Required</span></span> |<span data-ttu-id="fe8cb-250">**Xs:int**</span><span class="sxs-lookup"><span data-stu-id="fe8cb-250">**xs:int**</span></span> |<span data-ttu-id="fe8cb-251">I prover per sekund eller Hz ljud samplingsfrekvensen.</span><span class="sxs-lookup"><span data-stu-id="fe8cb-251">Audio sampling rate in samples/sec or Hz.</span></span> |
| <span data-ttu-id="fe8cb-252">**Bithastighet**</span><span class="sxs-lookup"><span data-stu-id="fe8cb-252">**Bitrate**</span></span> |<span data-ttu-id="fe8cb-253">**Xs:int**</span><span class="sxs-lookup"><span data-stu-id="fe8cb-253">**xs:int**</span></span> |<span data-ttu-id="fe8cb-254">Genomsnittlig ljud bithastighet i bitar per sekund som beräknas från hello resursfil.</span><span class="sxs-lookup"><span data-stu-id="fe8cb-254">Average audio bit rate in bits per second, as calculated from hello asset file.</span></span> <span data-ttu-id="fe8cb-255">Endast hello elementära dataströmmen nyttolast räknas och hello paketering kostnader ingår inte i det här antalet.</span><span class="sxs-lookup"><span data-stu-id="fe8cb-255">Only hello elementary stream payload is counted, and hello packaging overhead is not included in this count.</span></span> |
| <span data-ttu-id="fe8cb-256">**BitsPerSample**</span><span class="sxs-lookup"><span data-stu-id="fe8cb-256">**BitsPerSample**</span></span> |<span data-ttu-id="fe8cb-257">**Xs:int**</span><span class="sxs-lookup"><span data-stu-id="fe8cb-257">**xs:int**</span></span> |<span data-ttu-id="fe8cb-258">Bitar per prov för hello wFormatTag formatet typ.</span><span class="sxs-lookup"><span data-stu-id="fe8cb-258">Bits per sample for hello wFormatTag format type.</span></span> |

## <span data-ttu-id="fe8cb-259"><a name="VideoTrackType"></a>VideoTrackType (ärver från TrackType)</span><span class="sxs-lookup"><span data-stu-id="fe8cb-259"><a name="VideoTrackType"></a> VideoTrackType (inherits from TrackType)</span></span>
<span data-ttu-id="fe8cb-260">**VideoTrackType** är en global komplex typ som ärver från [TrackType](media-services-input-metadata-schema.md#TrackType).</span><span class="sxs-lookup"><span data-stu-id="fe8cb-260">**VideoTrackType** is a global complex type that inherits from [TrackType](media-services-input-metadata-schema.md#TrackType).</span></span>  

<span data-ttu-id="fe8cb-261">hello-typen motsvarar en viss video spår i hello resursfil.</span><span class="sxs-lookup"><span data-stu-id="fe8cb-261">hello type represents a specific video track in hello asset file.</span></span>  

<span data-ttu-id="fe8cb-262">Se ett XML-exempel hello slutet av det här avsnittet: [XML-exempel](media-services-input-metadata-schema.md#xml).</span><span class="sxs-lookup"><span data-stu-id="fe8cb-262">See an XML example at hello end of this topic: [XML example](media-services-input-metadata-schema.md#xml).</span></span>  

### <a name="attributes"></a><span data-ttu-id="fe8cb-263">Attribut</span><span class="sxs-lookup"><span data-stu-id="fe8cb-263">Attributes</span></span>
| <span data-ttu-id="fe8cb-264">Namn</span><span class="sxs-lookup"><span data-stu-id="fe8cb-264">Name</span></span> | <span data-ttu-id="fe8cb-265">Typ</span><span class="sxs-lookup"><span data-stu-id="fe8cb-265">Type</span></span> | <span data-ttu-id="fe8cb-266">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="fe8cb-266">Description</span></span> |
| --- | --- | --- |
| <span data-ttu-id="fe8cb-267">**FourCC**</span><span class="sxs-lookup"><span data-stu-id="fe8cb-267">**FourCC**</span></span><br /><br /> <span data-ttu-id="fe8cb-268">Krävs</span><span class="sxs-lookup"><span data-stu-id="fe8cb-268">Required</span></span> |<span data-ttu-id="fe8cb-269">**Xs:String**</span><span class="sxs-lookup"><span data-stu-id="fe8cb-269">**xs:string**</span></span> |<span data-ttu-id="fe8cb-270">Video-codec FourCC-kod.</span><span class="sxs-lookup"><span data-stu-id="fe8cb-270">Video codec FourCC code.</span></span> |
| <span data-ttu-id="fe8cb-271">**Profil**</span><span class="sxs-lookup"><span data-stu-id="fe8cb-271">**Profile**</span></span> |<span data-ttu-id="fe8cb-272">**Xs:String**</span><span class="sxs-lookup"><span data-stu-id="fe8cb-272">**xs:string**</span></span> |<span data-ttu-id="fe8cb-273">Video spåra profil.</span><span class="sxs-lookup"><span data-stu-id="fe8cb-273">Video track's profile.</span></span> |
| <span data-ttu-id="fe8cb-274">**Nivå**</span><span class="sxs-lookup"><span data-stu-id="fe8cb-274">**Level**</span></span> |<span data-ttu-id="fe8cb-275">**Xs:String**</span><span class="sxs-lookup"><span data-stu-id="fe8cb-275">**xs:string**</span></span> |<span data-ttu-id="fe8cb-276">Video spåra nivå.</span><span class="sxs-lookup"><span data-stu-id="fe8cb-276">Video track's level.</span></span> |
| <span data-ttu-id="fe8cb-277">**PixelFormat**</span><span class="sxs-lookup"><span data-stu-id="fe8cb-277">**PixelFormat**</span></span> |<span data-ttu-id="fe8cb-278">**Xs:String**</span><span class="sxs-lookup"><span data-stu-id="fe8cb-278">**xs:string**</span></span> |<span data-ttu-id="fe8cb-279">Video spåra bildpunktsformat.</span><span class="sxs-lookup"><span data-stu-id="fe8cb-279">Video track's pixel format.</span></span> |
| <span data-ttu-id="fe8cb-280">**Bredd**</span><span class="sxs-lookup"><span data-stu-id="fe8cb-280">**Width**</span></span><br /><br /> <span data-ttu-id="fe8cb-281">Krävs</span><span class="sxs-lookup"><span data-stu-id="fe8cb-281">Required</span></span> |<span data-ttu-id="fe8cb-282">**Xs:int**</span><span class="sxs-lookup"><span data-stu-id="fe8cb-282">**xs:int**</span></span> |<span data-ttu-id="fe8cb-283">Kodad video bredd i bildpunkter.</span><span class="sxs-lookup"><span data-stu-id="fe8cb-283">Encoded video width in pixels.</span></span> |
| <span data-ttu-id="fe8cb-284">**Höjd**</span><span class="sxs-lookup"><span data-stu-id="fe8cb-284">**Height**</span></span><br /><br /> <span data-ttu-id="fe8cb-285">Krävs</span><span class="sxs-lookup"><span data-stu-id="fe8cb-285">Required</span></span> |<span data-ttu-id="fe8cb-286">**Xs:int**</span><span class="sxs-lookup"><span data-stu-id="fe8cb-286">**xs:int**</span></span> |<span data-ttu-id="fe8cb-287">Kodad video höjd i bildpunkter.</span><span class="sxs-lookup"><span data-stu-id="fe8cb-287">Encoded video height in pixels.</span></span> |
| <span data-ttu-id="fe8cb-288">**DisplayAspectRatioNumerator**</span><span class="sxs-lookup"><span data-stu-id="fe8cb-288">**DisplayAspectRatioNumerator**</span></span><br /><br /> <span data-ttu-id="fe8cb-289">Krävs</span><span class="sxs-lookup"><span data-stu-id="fe8cb-289">Required</span></span> |<span data-ttu-id="fe8cb-290">**Xs:Double**</span><span class="sxs-lookup"><span data-stu-id="fe8cb-290">**xs:double**</span></span> |<span data-ttu-id="fe8cb-291">Videoenhet proportionerna täljaren.</span><span class="sxs-lookup"><span data-stu-id="fe8cb-291">Video display aspect ratio numerator.</span></span> |
| <span data-ttu-id="fe8cb-292">**DisplayAspectRatioDenominator**</span><span class="sxs-lookup"><span data-stu-id="fe8cb-292">**DisplayAspectRatioDenominator**</span></span><br /><br /> <span data-ttu-id="fe8cb-293">Krävs</span><span class="sxs-lookup"><span data-stu-id="fe8cb-293">Required</span></span> |<span data-ttu-id="fe8cb-294">**Xs:Double**</span><span class="sxs-lookup"><span data-stu-id="fe8cb-294">**xs:double**</span></span> |<span data-ttu-id="fe8cb-295">Videoenhet proportionerna nämnare.</span><span class="sxs-lookup"><span data-stu-id="fe8cb-295">Video display aspect ratio denominator.</span></span> |
| <span data-ttu-id="fe8cb-296">**DisplayAspectRatioDenominator**</span><span class="sxs-lookup"><span data-stu-id="fe8cb-296">**DisplayAspectRatioDenominator**</span></span><br /><br /> <span data-ttu-id="fe8cb-297">Krävs</span><span class="sxs-lookup"><span data-stu-id="fe8cb-297">Required</span></span> |<span data-ttu-id="fe8cb-298">**Xs:Double**</span><span class="sxs-lookup"><span data-stu-id="fe8cb-298">**xs:double**</span></span> |<span data-ttu-id="fe8cb-299">Video exempel proportionerna täljaren.</span><span class="sxs-lookup"><span data-stu-id="fe8cb-299">Video sample aspect ratio numerator.</span></span> |
| <span data-ttu-id="fe8cb-300">**SampleAspectRatioNumerator**</span><span class="sxs-lookup"><span data-stu-id="fe8cb-300">**SampleAspectRatioNumerator**</span></span> |<span data-ttu-id="fe8cb-301">**Xs:Double**</span><span class="sxs-lookup"><span data-stu-id="fe8cb-301">**xs:double**</span></span> |<span data-ttu-id="fe8cb-302">Video exempel proportionerna täljaren.</span><span class="sxs-lookup"><span data-stu-id="fe8cb-302">Video sample aspect ratio numerator.</span></span> |
| <span data-ttu-id="fe8cb-303">**SampleAspectRatioNumerator**</span><span class="sxs-lookup"><span data-stu-id="fe8cb-303">**SampleAspectRatioNumerator**</span></span> |<span data-ttu-id="fe8cb-304">**Xs:Double**</span><span class="sxs-lookup"><span data-stu-id="fe8cb-304">**xs:double**</span></span> |<span data-ttu-id="fe8cb-305">Video exempel proportionerna nämnare.</span><span class="sxs-lookup"><span data-stu-id="fe8cb-305">Video sample aspect ratio denominator.</span></span> |
| <span data-ttu-id="fe8cb-306">**Ramhastighet**</span><span class="sxs-lookup"><span data-stu-id="fe8cb-306">**FrameRate**</span></span><br /><br /> <span data-ttu-id="fe8cb-307">Krävs</span><span class="sxs-lookup"><span data-stu-id="fe8cb-307">Required</span></span> |<span data-ttu-id="fe8cb-308">**Xs:decimal**</span><span class="sxs-lookup"><span data-stu-id="fe8cb-308">**xs:decimal**</span></span> |<span data-ttu-id="fe8cb-309">Mätt video bildfrekvens .3f format.</span><span class="sxs-lookup"><span data-stu-id="fe8cb-309">Measured video frame rate in .3f format.</span></span> |
| <span data-ttu-id="fe8cb-310">**Bithastighet**</span><span class="sxs-lookup"><span data-stu-id="fe8cb-310">**Bitrate**</span></span> |<span data-ttu-id="fe8cb-311">**Xs:int**</span><span class="sxs-lookup"><span data-stu-id="fe8cb-311">**xs:int**</span></span> |<span data-ttu-id="fe8cb-312">Genomsnittlig video bithastighet i kilobit per sekund som beräknas från hello resursfil.</span><span class="sxs-lookup"><span data-stu-id="fe8cb-312">Average video bit rate in kilobits per second, as calculated from hello asset file.</span></span> <span data-ttu-id="fe8cb-313">Endast hello elementära dataströmmen nyttolast räknas och hello paketering kostnader ingår inte.</span><span class="sxs-lookup"><span data-stu-id="fe8cb-313">Only hello elementary stream payload is counted, and hello packaging overhead is not included.</span></span> |
| <span data-ttu-id="fe8cb-314">**MaxGOPBitrate**</span><span class="sxs-lookup"><span data-stu-id="fe8cb-314">**MaxGOPBitrate**</span></span> |<span data-ttu-id="fe8cb-315">**Xs:int**</span><span class="sxs-lookup"><span data-stu-id="fe8cb-315">**xs:int**</span></span> |<span data-ttu-id="fe8cb-316">Max GOP genomsnittlig bithastighet för den här videon spår i kilobit per sekund.</span><span class="sxs-lookup"><span data-stu-id="fe8cb-316">Max GOP average bitrate for this video track, in kilobits per second.</span></span> |
| <span data-ttu-id="fe8cb-317">**HasBFrames**</span><span class="sxs-lookup"><span data-stu-id="fe8cb-317">**HasBFrames**</span></span> |<span data-ttu-id="fe8cb-318">**Xs:int**</span><span class="sxs-lookup"><span data-stu-id="fe8cb-318">**xs:int**</span></span> |<span data-ttu-id="fe8cb-319">Video spåra antalet B ramar.</span><span class="sxs-lookup"><span data-stu-id="fe8cb-319">Video track number of B frames.</span></span> |

## <span data-ttu-id="fe8cb-320"><a name="MetadataType"></a>MetadataType</span><span class="sxs-lookup"><span data-stu-id="fe8cb-320"><a name="MetadataType"></a> MetadataType</span></span>
<span data-ttu-id="fe8cb-321">**MetadataType** är en global komplex typ som beskriver metadata för en resursfil som nyckel/värde-strängar.</span><span class="sxs-lookup"><span data-stu-id="fe8cb-321">**MetadataType** is a global complex type that describes metadata of an asset file as key/value strings.</span></span> <span data-ttu-id="fe8cb-322">Till exempel nyckel = ”språk”, och värdet = ”eng”.</span><span class="sxs-lookup"><span data-stu-id="fe8cb-322">For example, key=”language”, and value=”eng”.</span></span>  

<span data-ttu-id="fe8cb-323">Se ett XML-exempel hello slutet av det här avsnittet: [XML-exempel](media-services-input-metadata-schema.md#xml).</span><span class="sxs-lookup"><span data-stu-id="fe8cb-323">See an XML example at hello end of this topic: [XML example](media-services-input-metadata-schema.md#xml).</span></span>  

### <a name="attributes"></a><span data-ttu-id="fe8cb-324">Attribut</span><span class="sxs-lookup"><span data-stu-id="fe8cb-324">Attributes</span></span>
| <span data-ttu-id="fe8cb-325">Namn</span><span class="sxs-lookup"><span data-stu-id="fe8cb-325">Name</span></span> | <span data-ttu-id="fe8cb-326">Typ</span><span class="sxs-lookup"><span data-stu-id="fe8cb-326">Type</span></span> | <span data-ttu-id="fe8cb-327">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="fe8cb-327">Description</span></span> |
| --- | --- | --- |
| <span data-ttu-id="fe8cb-328">**nyckel**</span><span class="sxs-lookup"><span data-stu-id="fe8cb-328">**key**</span></span><br /><br /> <span data-ttu-id="fe8cb-329">Krävs</span><span class="sxs-lookup"><span data-stu-id="fe8cb-329">Required</span></span> |<span data-ttu-id="fe8cb-330">**Xs:String**</span><span class="sxs-lookup"><span data-stu-id="fe8cb-330">**xs:string**</span></span> |<span data-ttu-id="fe8cb-331">hello nyckeln i hello nyckel/värde-par.</span><span class="sxs-lookup"><span data-stu-id="fe8cb-331">hello key in hello key/value pair.</span></span> |
| <span data-ttu-id="fe8cb-332">**värdet**</span><span class="sxs-lookup"><span data-stu-id="fe8cb-332">**value**</span></span><br /><br /> <span data-ttu-id="fe8cb-333">Krävs</span><span class="sxs-lookup"><span data-stu-id="fe8cb-333">Required</span></span> |<span data-ttu-id="fe8cb-334">**Xs:String**</span><span class="sxs-lookup"><span data-stu-id="fe8cb-334">**xs:string**</span></span> |<span data-ttu-id="fe8cb-335">hello värdet i hello nyckel/värde-par.</span><span class="sxs-lookup"><span data-stu-id="fe8cb-335">hello value in hello key/value pair.</span></span> |

## <span data-ttu-id="fe8cb-336"><a name="ProgramType"></a>ProgramType</span><span class="sxs-lookup"><span data-stu-id="fe8cb-336"><a name="ProgramType"></a> ProgramType</span></span>
<span data-ttu-id="fe8cb-337">**ProgramType** är en global komplex typ som beskriver ett program.</span><span class="sxs-lookup"><span data-stu-id="fe8cb-337">**ProgramType** is a global complex type that describes a program.</span></span>  

### <a name="attributes"></a><span data-ttu-id="fe8cb-338">Attribut</span><span class="sxs-lookup"><span data-stu-id="fe8cb-338">Attributes</span></span>
| <span data-ttu-id="fe8cb-339">Namn</span><span class="sxs-lookup"><span data-stu-id="fe8cb-339">Name</span></span> | <span data-ttu-id="fe8cb-340">Typ</span><span class="sxs-lookup"><span data-stu-id="fe8cb-340">Type</span></span> | <span data-ttu-id="fe8cb-341">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="fe8cb-341">Description</span></span> |
| --- | --- | --- |
| <span data-ttu-id="fe8cb-342">**ProgramId**</span><span class="sxs-lookup"><span data-stu-id="fe8cb-342">**ProgramId**</span></span><br /><br /> <span data-ttu-id="fe8cb-343">Krävs</span><span class="sxs-lookup"><span data-stu-id="fe8cb-343">Required</span></span> |<span data-ttu-id="fe8cb-344">**Xs:int**</span><span class="sxs-lookup"><span data-stu-id="fe8cb-344">**xs:int**</span></span> |<span data-ttu-id="fe8cb-345">Program-Id</span><span class="sxs-lookup"><span data-stu-id="fe8cb-345">Program Id</span></span> |
| <span data-ttu-id="fe8cb-346">**NumberOfPrograms**</span><span class="sxs-lookup"><span data-stu-id="fe8cb-346">**NumberOfPrograms**</span></span><br /><br /> <span data-ttu-id="fe8cb-347">Krävs</span><span class="sxs-lookup"><span data-stu-id="fe8cb-347">Required</span></span> |<span data-ttu-id="fe8cb-348">**Xs:int**</span><span class="sxs-lookup"><span data-stu-id="fe8cb-348">**xs:int**</span></span> |<span data-ttu-id="fe8cb-349">Antal program.</span><span class="sxs-lookup"><span data-stu-id="fe8cb-349">Number of programs.</span></span> |
| <span data-ttu-id="fe8cb-350">**PmtPid**</span><span class="sxs-lookup"><span data-stu-id="fe8cb-350">**PmtPid**</span></span><br /><br /> <span data-ttu-id="fe8cb-351">Krävs</span><span class="sxs-lookup"><span data-stu-id="fe8cb-351">Required</span></span> |<span data-ttu-id="fe8cb-352">**Xs:int**</span><span class="sxs-lookup"><span data-stu-id="fe8cb-352">**xs:int**</span></span> |<span data-ttu-id="fe8cb-353">Programmet kartan tabeller (PMTs) innehåller information om program.</span><span class="sxs-lookup"><span data-stu-id="fe8cb-353">Program Map Tables (PMTs) contain information about programs.</span></span>  <span data-ttu-id="fe8cb-354">Mer information finns i [betalning](http://en.wikipedia.org/wiki/MPEG_transport_stream#PMT).</span><span class="sxs-lookup"><span data-stu-id="fe8cb-354">For more information, see [PMt](http://en.wikipedia.org/wiki/MPEG_transport_stream#PMT).</span></span> |
| <span data-ttu-id="fe8cb-355">**PcrPid**</span><span class="sxs-lookup"><span data-stu-id="fe8cb-355">**PcrPid**</span></span><br /><br /> <span data-ttu-id="fe8cb-356">Krävs</span><span class="sxs-lookup"><span data-stu-id="fe8cb-356">Required</span></span> |<span data-ttu-id="fe8cb-357">**Xs:int**</span><span class="sxs-lookup"><span data-stu-id="fe8cb-357">**xs:int**</span></span> |<span data-ttu-id="fe8cb-358">Används av avkodarens.</span><span class="sxs-lookup"><span data-stu-id="fe8cb-358">Used by decoder.</span></span> <span data-ttu-id="fe8cb-359">Mer information finns i [PCR](http://en.wikipedia.org/wiki/MPEG_transport_stream#PCR)</span><span class="sxs-lookup"><span data-stu-id="fe8cb-359">For more information, see [PCR](http://en.wikipedia.org/wiki/MPEG_transport_stream#PCR)</span></span> |
| <span data-ttu-id="fe8cb-360">**StartPTS**</span><span class="sxs-lookup"><span data-stu-id="fe8cb-360">**StartPTS**</span></span> |<span data-ttu-id="fe8cb-361">**Xs: lång**</span><span class="sxs-lookup"><span data-stu-id="fe8cb-361">**xs: long**</span></span> |<span data-ttu-id="fe8cb-362">Starta en presentation tidsstämpel.</span><span class="sxs-lookup"><span data-stu-id="fe8cb-362">Starting presentation time stamp.</span></span> |
| <span data-ttu-id="fe8cb-363">**EndPTS**</span><span class="sxs-lookup"><span data-stu-id="fe8cb-363">**EndPTS**</span></span> |<span data-ttu-id="fe8cb-364">**Xs: lång**</span><span class="sxs-lookup"><span data-stu-id="fe8cb-364">**xs: long**</span></span> |<span data-ttu-id="fe8cb-365">Avslutas presentation tidsstämpel.</span><span class="sxs-lookup"><span data-stu-id="fe8cb-365">Ending presentation time stamp.</span></span> |

## <span data-ttu-id="fe8cb-366"><a name="StreamDispositionType"></a>StreamDispositionType</span><span class="sxs-lookup"><span data-stu-id="fe8cb-366"><a name="StreamDispositionType"></a> StreamDispositionType</span></span>
<span data-ttu-id="fe8cb-367">**StreamDispositionType** är en global komplex typ som beskriver hello dataströmmen.</span><span class="sxs-lookup"><span data-stu-id="fe8cb-367">**StreamDispositionType** is a global complex type that describes hello stream.</span></span>  

<span data-ttu-id="fe8cb-368">Se ett XML-exempel hello slutet av det här avsnittet: [XML-exempel](media-services-input-metadata-schema.md#xml).</span><span class="sxs-lookup"><span data-stu-id="fe8cb-368">See an XML example at hello end of this topic: [XML example](media-services-input-metadata-schema.md#xml).</span></span>  

### <a name="attributes"></a><span data-ttu-id="fe8cb-369">Attribut</span><span class="sxs-lookup"><span data-stu-id="fe8cb-369">Attributes</span></span>
| <span data-ttu-id="fe8cb-370">Namn</span><span class="sxs-lookup"><span data-stu-id="fe8cb-370">Name</span></span> | <span data-ttu-id="fe8cb-371">Typ</span><span class="sxs-lookup"><span data-stu-id="fe8cb-371">Type</span></span> | <span data-ttu-id="fe8cb-372">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="fe8cb-372">Description</span></span> |
| --- | --- | --- |
| <span data-ttu-id="fe8cb-373">**Standard**</span><span class="sxs-lookup"><span data-stu-id="fe8cb-373">**Default**</span></span><br /><br /> <span data-ttu-id="fe8cb-374">Krävs</span><span class="sxs-lookup"><span data-stu-id="fe8cb-374">Required</span></span> |<span data-ttu-id="fe8cb-375">**Xs:int**</span><span class="sxs-lookup"><span data-stu-id="fe8cb-375">**xs:int**</span></span> |<span data-ttu-id="fe8cb-376">Ange det här attributet too1 tooindicate är hello standardpresentationen.</span><span class="sxs-lookup"><span data-stu-id="fe8cb-376">Set this attribute too1 tooindicate this is hello default presentation.</span></span> |
| <span data-ttu-id="fe8cb-377">**Dub**</span><span class="sxs-lookup"><span data-stu-id="fe8cb-377">**Dub**</span></span><br /><br /> <span data-ttu-id="fe8cb-378">Krävs</span><span class="sxs-lookup"><span data-stu-id="fe8cb-378">Required</span></span> |<span data-ttu-id="fe8cb-379">**Xs:int**</span><span class="sxs-lookup"><span data-stu-id="fe8cb-379">**xs:int**</span></span> |<span data-ttu-id="fe8cb-380">Ställ in det här attributet too1 tooindicate är hello dubbat presentation.</span><span class="sxs-lookup"><span data-stu-id="fe8cb-380">Set this attribute too1 tooindicate this is hello dubbed presentation.</span></span> |
| <span data-ttu-id="fe8cb-381">**Ursprungliga**</span><span class="sxs-lookup"><span data-stu-id="fe8cb-381">**Original**</span></span><br /><br /> <span data-ttu-id="fe8cb-382">Krävs</span><span class="sxs-lookup"><span data-stu-id="fe8cb-382">Required</span></span> |<span data-ttu-id="fe8cb-383">**Xs:int**</span><span class="sxs-lookup"><span data-stu-id="fe8cb-383">**xs:int**</span></span> |<span data-ttu-id="fe8cb-384">Ange det här attributet too1 tooindicate är hello ursprungliga presentationen.</span><span class="sxs-lookup"><span data-stu-id="fe8cb-384">Set this attribute too1 tooindicate this is hello original presentation.</span></span> |
| <span data-ttu-id="fe8cb-385">**Kommentar**</span><span class="sxs-lookup"><span data-stu-id="fe8cb-385">**Comment**</span></span><br /><br /> <span data-ttu-id="fe8cb-386">Krävs</span><span class="sxs-lookup"><span data-stu-id="fe8cb-386">Required</span></span> |<span data-ttu-id="fe8cb-387">**Xs:int**</span><span class="sxs-lookup"><span data-stu-id="fe8cb-387">**xs:int**</span></span> |<span data-ttu-id="fe8cb-388">Ställ in det här attributet too1 tooindicate spåra innehåller kommentar.</span><span class="sxs-lookup"><span data-stu-id="fe8cb-388">Set this attribute too1 tooindicate this track contains commentary.</span></span> |
| <span data-ttu-id="fe8cb-389">**Texter**</span><span class="sxs-lookup"><span data-stu-id="fe8cb-389">**Lyrics**</span></span><br /><br /> <span data-ttu-id="fe8cb-390">Krävs</span><span class="sxs-lookup"><span data-stu-id="fe8cb-390">Required</span></span> |<span data-ttu-id="fe8cb-391">**Xs:int**</span><span class="sxs-lookup"><span data-stu-id="fe8cb-391">**xs:int**</span></span> |<span data-ttu-id="fe8cb-392">Ställ in det här attributet too1 tooindicate spåra innehåller texter.</span><span class="sxs-lookup"><span data-stu-id="fe8cb-392">Set this attribute too1 tooindicate this track contains lyrics.</span></span> |
| <span data-ttu-id="fe8cb-393">**Karaoke**</span><span class="sxs-lookup"><span data-stu-id="fe8cb-393">**Karaoke**</span></span><br /><br /> <span data-ttu-id="fe8cb-394">Krävs</span><span class="sxs-lookup"><span data-stu-id="fe8cb-394">Required</span></span> |<span data-ttu-id="fe8cb-395">**Xs:int**</span><span class="sxs-lookup"><span data-stu-id="fe8cb-395">**xs:int**</span></span> |<span data-ttu-id="fe8cb-396">Ställ in det här attributet too1 tooindicate representerar hello karaoke spåra (bakgrundsmusik, ingen sångrösten).</span><span class="sxs-lookup"><span data-stu-id="fe8cb-396">Set this attribute too1 tooindicate this represents hello karaoke track (background music, no vocals).</span></span> |
| <span data-ttu-id="fe8cb-397">**Tvingad**</span><span class="sxs-lookup"><span data-stu-id="fe8cb-397">**Forced**</span></span><br /><br /> <span data-ttu-id="fe8cb-398">Krävs</span><span class="sxs-lookup"><span data-stu-id="fe8cb-398">Required</span></span> |<span data-ttu-id="fe8cb-399">**Xs:int**</span><span class="sxs-lookup"><span data-stu-id="fe8cb-399">**xs:int**</span></span> |<span data-ttu-id="fe8cb-400">Ange det här attributet too1 tooindicate är hello tvingas presentation.</span><span class="sxs-lookup"><span data-stu-id="fe8cb-400">Set this attribute too1 tooindicate this is hello forced presentation.</span></span> |
| <span data-ttu-id="fe8cb-401">**HearingImpaired**</span><span class="sxs-lookup"><span data-stu-id="fe8cb-401">**HearingImpaired**</span></span><br /><br /> <span data-ttu-id="fe8cb-402">Krävs</span><span class="sxs-lookup"><span data-stu-id="fe8cb-402">Required</span></span> |<span data-ttu-id="fe8cb-403">**Xs:int**</span><span class="sxs-lookup"><span data-stu-id="fe8cb-403">**xs:int**</span></span> |<span data-ttu-id="fe8cb-404">Ange det här attributet too1 tooindicate spåret avser hello hörsel försämras.</span><span class="sxs-lookup"><span data-stu-id="fe8cb-404">Set this attribute too1 tooindicate this track is for hello hearing impaired.</span></span> |
| <span data-ttu-id="fe8cb-405">**VisualImpaired**</span><span class="sxs-lookup"><span data-stu-id="fe8cb-405">**VisualImpaired**</span></span><br /><br /> <span data-ttu-id="fe8cb-406">Krävs</span><span class="sxs-lookup"><span data-stu-id="fe8cb-406">Required</span></span> |<span data-ttu-id="fe8cb-407">**Xs:int**</span><span class="sxs-lookup"><span data-stu-id="fe8cb-407">**xs:int**</span></span> |<span data-ttu-id="fe8cb-408">Ange det här attributet too1 tooindicate spåret är för personer med nedsatt syn hello.</span><span class="sxs-lookup"><span data-stu-id="fe8cb-408">Set this attribute too1 tooindicate this track is for hello visually impaired.</span></span> |
| <span data-ttu-id="fe8cb-409">**CleanEffects**</span><span class="sxs-lookup"><span data-stu-id="fe8cb-409">**CleanEffects**</span></span><br /><br /> <span data-ttu-id="fe8cb-410">Krävs</span><span class="sxs-lookup"><span data-stu-id="fe8cb-410">Required</span></span> |<span data-ttu-id="fe8cb-411">**Xs:int**</span><span class="sxs-lookup"><span data-stu-id="fe8cb-411">**xs:int**</span></span> |<span data-ttu-id="fe8cb-412">Ange det här attributet too1 tooindicate spåret har ren effekter.</span><span class="sxs-lookup"><span data-stu-id="fe8cb-412">Set this attribute too1 tooindicate this track has clean effects.</span></span> |
| <span data-ttu-id="fe8cb-413">**AttachedPic**</span><span class="sxs-lookup"><span data-stu-id="fe8cb-413">**AttachedPic**</span></span><br /><br /> <span data-ttu-id="fe8cb-414">Krävs</span><span class="sxs-lookup"><span data-stu-id="fe8cb-414">Required</span></span> |<span data-ttu-id="fe8cb-415">**Xs:int**</span><span class="sxs-lookup"><span data-stu-id="fe8cb-415">**xs:int**</span></span> |<span data-ttu-id="fe8cb-416">Ange det här attributet too1 tooindicate spåret har bilder.</span><span class="sxs-lookup"><span data-stu-id="fe8cb-416">Set this attribute too1 tooindicate this track has pictures.</span></span> |

## <span data-ttu-id="fe8cb-417"><a name="Programs"></a>Program-element</span><span class="sxs-lookup"><span data-stu-id="fe8cb-417"><a name="Programs"></a> Programs element</span></span>
<span data-ttu-id="fe8cb-418">-Elementet innehåller flera **programmet** element.</span><span class="sxs-lookup"><span data-stu-id="fe8cb-418">Wrapper element holding multiple **Program** elements.</span></span>  

### <a name="child-elements"></a><span data-ttu-id="fe8cb-419">Underordnade element</span><span class="sxs-lookup"><span data-stu-id="fe8cb-419">Child elements</span></span>
| <span data-ttu-id="fe8cb-420">Namn</span><span class="sxs-lookup"><span data-stu-id="fe8cb-420">Name</span></span> | <span data-ttu-id="fe8cb-421">Typ</span><span class="sxs-lookup"><span data-stu-id="fe8cb-421">Type</span></span> | <span data-ttu-id="fe8cb-422">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="fe8cb-422">Description</span></span> |
| --- | --- | --- |
| <span data-ttu-id="fe8cb-423">**Programmet**</span><span class="sxs-lookup"><span data-stu-id="fe8cb-423">**Program**</span></span><br /><br /> <span data-ttu-id="fe8cb-424">minOccurs = ”0” maxOccurs = ”unbounded”</span><span class="sxs-lookup"><span data-stu-id="fe8cb-424">minOccurs="0" maxOccurs="unbounded"</span></span> |[<span data-ttu-id="fe8cb-425">ProgramType</span><span class="sxs-lookup"><span data-stu-id="fe8cb-425">ProgramType</span></span>](media-services-input-metadata-schema.md#ProgramType) |<span data-ttu-id="fe8cb-426">Innehåller information om program i hello resursfilen för tillgångsfiler i MPEG-TS-format.</span><span class="sxs-lookup"><span data-stu-id="fe8cb-426">For asset files that are in MPEG-TS format, contains information about programs in hello asset file.</span></span> |

## <span data-ttu-id="fe8cb-427"><a name="VideoTracks"></a>VideoTracks element</span><span class="sxs-lookup"><span data-stu-id="fe8cb-427"><a name="VideoTracks"></a> VideoTracks element</span></span>
 <span data-ttu-id="fe8cb-428">-Elementet innehåller flera **VideoTrack** element.</span><span class="sxs-lookup"><span data-stu-id="fe8cb-428">Wrapper element holding multiple **VideoTrack** elements.</span></span>  

 <span data-ttu-id="fe8cb-429">Se ett XML-exempel hello slutet av det här avsnittet: [XML-exempel](media-services-input-metadata-schema.md#xml).</span><span class="sxs-lookup"><span data-stu-id="fe8cb-429">See an XML example at hello end of this topic: [XML example](media-services-input-metadata-schema.md#xml).</span></span>  

### <a name="child-elements"></a><span data-ttu-id="fe8cb-430">Underordnade element</span><span class="sxs-lookup"><span data-stu-id="fe8cb-430">Child elements</span></span>
| <span data-ttu-id="fe8cb-431">Namn</span><span class="sxs-lookup"><span data-stu-id="fe8cb-431">Name</span></span> | <span data-ttu-id="fe8cb-432">Typ</span><span class="sxs-lookup"><span data-stu-id="fe8cb-432">Type</span></span> | <span data-ttu-id="fe8cb-433">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="fe8cb-433">Description</span></span> |
| --- | --- | --- |
| <span data-ttu-id="fe8cb-434">**VideoTrack**</span><span class="sxs-lookup"><span data-stu-id="fe8cb-434">**VideoTrack**</span></span><br /><br /> <span data-ttu-id="fe8cb-435">minOccurs = ”0” maxOccurs = ”unbounded”</span><span class="sxs-lookup"><span data-stu-id="fe8cb-435">minOccurs="0" maxOccurs="unbounded"</span></span> |[<span data-ttu-id="fe8cb-436">VideoTrackType (ärver från TrackType)</span><span class="sxs-lookup"><span data-stu-id="fe8cb-436">VideoTrackType (inherits from TrackType)</span></span>](media-services-input-metadata-schema.md#VideoTrackType) |<span data-ttu-id="fe8cb-437">Innehåller information om video spår i hello resursfil.</span><span class="sxs-lookup"><span data-stu-id="fe8cb-437">Contains information about video tracks in hello asset file.</span></span> |

## <span data-ttu-id="fe8cb-438"><a name="AudioTracks"></a>AudioTracks element</span><span class="sxs-lookup"><span data-stu-id="fe8cb-438"><a name="AudioTracks"></a> AudioTracks element</span></span>
 <span data-ttu-id="fe8cb-439">-Elementet innehåller flera **AudioTrack** element.</span><span class="sxs-lookup"><span data-stu-id="fe8cb-439">Wrapper element holding multiple **AudioTrack** elements.</span></span>  

 <span data-ttu-id="fe8cb-440">Se ett XML-exempel hello slutet av det här avsnittet: [XML-exempel](media-services-input-metadata-schema.md#xml).</span><span class="sxs-lookup"><span data-stu-id="fe8cb-440">See an XML example at hello end of this topic: [XML example](media-services-input-metadata-schema.md#xml).</span></span>  

### <a name="elements"></a><span data-ttu-id="fe8cb-441">Element</span><span class="sxs-lookup"><span data-stu-id="fe8cb-441">elements</span></span>
| <span data-ttu-id="fe8cb-442">Namn</span><span class="sxs-lookup"><span data-stu-id="fe8cb-442">Name</span></span> | <span data-ttu-id="fe8cb-443">Typ</span><span class="sxs-lookup"><span data-stu-id="fe8cb-443">Type</span></span> | <span data-ttu-id="fe8cb-444">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="fe8cb-444">Description</span></span> |
| --- | --- | --- |
| <span data-ttu-id="fe8cb-445">**AudioTrack**</span><span class="sxs-lookup"><span data-stu-id="fe8cb-445">**AudioTrack**</span></span><br /><br /> <span data-ttu-id="fe8cb-446">minOccurs = ”0” maxOccurs = ”unbounded”</span><span class="sxs-lookup"><span data-stu-id="fe8cb-446">minOccurs="0" maxOccurs="unbounded"</span></span> |[<span data-ttu-id="fe8cb-447">AudioTrackType (ärver från TrackType)</span><span class="sxs-lookup"><span data-stu-id="fe8cb-447">AudioTrackType (inherits from TrackType)</span></span>](media-services-input-metadata-schema.md#AudioTrackType) |<span data-ttu-id="fe8cb-448">Innehåller information om ljud spår i hello resursfil.</span><span class="sxs-lookup"><span data-stu-id="fe8cb-448">Contains information about audio tracks in hello asset file.</span></span> |

## <span data-ttu-id="fe8cb-449"><a name="code"></a>Schemat kod</span><span class="sxs-lookup"><span data-stu-id="fe8cb-449"><a name="code"></a> Schema Code</span></span>
    <?xml version="1.0" encoding="utf-8"?>  
    <xs:schema xmlns:xs="http://www.w3.org/2001/XMLSchema" xmlns:msdata="urn:schemas-microsoft-com:xml-msdata" version="1.0"  
               xmlns="http://schemas.microsoft.com/windowsazure/mediaservices/2014/07/mediaencoder/inputmetadata"  
               targetNamespace="http://schemas.microsoft.com/windowsazure/mediaservices/2014/07/mediaencoder/inputmetadata"  
               elementFormDefault="qualified">  

      <xs:complexType name="MetadataType">  
        <xs:attribute name="key"   type="xs:string" use="required"/>  
        <xs:attribute name="value" type="xs:string" use="required"/>  
      </xs:complexType>  

      <xs:complexType name="ProgramType">  
        <xs:attribute name="ProgramId" type="xs:int" use="required">  
          <xs:annotation>  
            <xs:documentation>Program Id</xs:documentation>  
          </xs:annotation>  
        </xs:attribute>  
        <xs:attribute name="NumberOfPrograms" type="xs:int" use="required">  
          <xs:annotation>  
            <xs:documentation>Number of programs</xs:documentation>  
          </xs:annotation>  
        </xs:attribute>  
        <xs:attribute name="PmtPid" type="xs:int" use="required">  
          <xs:annotation>  
            <xs:documentation>pmt pid</xs:documentation>  
          </xs:annotation>  
        </xs:attribute>  
        <xs:attribute name="PcrPid" type="xs:int" use="required">  
          <xs:annotation>  
            <xs:documentation>pcr pid</xs:documentation>  
          </xs:annotation>  
        </xs:attribute>  
        <xs:attribute name="StartPTS" type="xs:long">  
          <xs:annotation>  
            <xs:documentation>start pts</xs:documentation>  
          </xs:annotation>  
        </xs:attribute>  
        <xs:attribute name="EndPTS" type="xs:long">  
          <xs:annotation>  
            <xs:documentation>end pts</xs:documentation>  
          </xs:annotation>  
        </xs:attribute>  
      </xs:complexType>  

      <xs:complexType name="StreamDispositionType">  
        <xs:attribute name="Default"          type="xs:int" use="required" />  
        <xs:attribute name="Dub"              type="xs:int" use="required" />  
        <xs:attribute name="Original"         type="xs:int" use="required" />  
        <xs:attribute name="Comment"          type="xs:int" use="required" />  
        <xs:attribute name="Lyrics"           type="xs:int" use="required" />  
        <xs:attribute name="Karaoke"          type="xs:int" use="required" />  
        <xs:attribute name="Forced"           type="xs:int" use="required" />  
        <xs:attribute name="HearingImpaired"  type="xs:int" use="required" />  
        <xs:attribute name="VisualImpaired"   type="xs:int" use="required" />  
        <xs:attribute name="CleanEffects"     type="xs:int" use="required" />  
        <xs:attribute name="AttachedPic"      type="xs:int" use="required" />  
      </xs:complexType>  

      <xs:complexType name="TrackType" abstract="true">  
        <xs:sequence>  
          <xs:element name="Disposition" type="StreamDispositionType" minOccurs="0" maxOccurs="1"/>  
          <xs:element name="Metadata" type="MetadataType" minOccurs="0" maxOccurs="unbounded"/>  
        </xs:sequence>  
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
        <xs:attribute name="Codec" type="xs:string">  
          <xs:annotation>  
            <xs:documentation>video track codec string</xs:documentation>  
          </xs:annotation>  
        </xs:attribute>  
        <xs:attribute name="CodecLongName" type="xs:string">  
          <xs:annotation>  
            <xs:documentation>video track codec long name</xs:documentation>  
          </xs:annotation>  
        </xs:attribute>  
        <xs:attribute name="TimeBase"  type="xs:string" use="required">  
          <xs:annotation>  
            <xs:documentation>Time base. Example: TimeBase="1/48000"</xs:documentation>  
          </xs:annotation>  
        </xs:attribute>  
        <xs:attribute name="NumberOfFrames">  
          <xs:annotation>  
            <xs:documentation>number of frames</xs:documentation>  
          </xs:annotation>  
          <xs:simpleType>  
            <xs:restriction base="xs:int">  
              <xs:minInclusive value="0"/>  
            </xs:restriction>  
          </xs:simpleType>  
        </xs:attribute>  
        <xs:attribute name="StartTime" type="xs:duration">  
          <xs:annotation>  
            <xs:documentation>Track start time. Example: StartTime="PT2.669S"</xs:documentation>  
          </xs:annotation>  
        </xs:attribute>  
        <xs:attribute name="Duration" type="xs:duration">  
          <xs:annotation>  
            <xs:documentation>Track duration. Example: Duration="PT25M37.757S"</xs:documentation>  
          </xs:annotation>  
        </xs:attribute>  
      </xs:complexType>  

      <xs:complexType name="VideoTrackType">  
        <xs:annotation>  
          <xs:documentation>A specific video track in hello parent AssetFile</xs:documentation>  
        </xs:annotation>  
        <xs:complexContent>  
          <xs:extension base="TrackType">  
            <xs:attribute name="FourCC" type="xs:string" use="required">  
              <xs:annotation>  
                <xs:documentation>video codec FourCC code</xs:documentation>  
              </xs:annotation>  
            </xs:attribute>  
            <xs:attribute name="Profile" type="xs:string">  
              <xs:annotation>  
                <xs:documentation>profile</xs:documentation>  
              </xs:annotation>  
            </xs:attribute>  
            <xs:attribute name="Level" type="xs:string">  
              <xs:annotation>  
                <xs:documentation>level</xs:documentation>  
              </xs:annotation>  
            </xs:attribute>  
            <xs:attribute name="PixelFormat" type="xs:string">  
              <xs:annotation>  
                <xs:documentation>Video track's pixel format</xs:documentation>  
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
            <xs:attribute name="SampleAspectRatioNumerator">  
              <xs:annotation>  
                <xs:documentation>video sample aspect ratio numerator</xs:documentation>  
              </xs:annotation>  
              <xs:simpleType>  
                <xs:restriction base="xs:double">  
                  <xs:minInclusive value="0"/>  
                </xs:restriction>  
              </xs:simpleType>  
            </xs:attribute>  
            <xs:attribute name="SampleAspectRatioDenominator">  
              <xs:annotation>  
                <xs:documentation>video sample aspect ratio denominator</xs:documentation>  
              </xs:annotation>  
              <xs:simpleType>  
                <xs:restriction base="xs:double">  
                  <xs:minInclusive value="0"/>  
                </xs:restriction>  
              </xs:simpleType>  
            </xs:attribute>  
            <xs:attribute name="FrameRate" use="required">  
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
            <xs:attribute name="Bitrate">  
              <xs:annotation>  
                <xs:documentation>average video bit rate in kilobits per second, as calculated from hello AssetFile. Counts only hello elementary stream payload, and does not include hello packaging overhead</xs:documentation>  
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
            <xs:attribute name="HasBFrames" type="xs:int">  
              <xs:annotation>  
                <xs:documentation>video track number of B frames</xs:documentation>  
              </xs:annotation>  
            </xs:attribute>  
          </xs:extension>  
        </xs:complexContent>  
      </xs:complexType>  

      <xs:complexType name="AudioTrackType">  
        <xs:annotation>  
          <xs:documentation>a specific audio track in hello parent AssetFile</xs:documentation>  
        </xs:annotation>  
        <xs:complexContent>  
          <xs:extension base="TrackType">  
            <xs:attribute name="SampleFormat"  type="xs:string">  
              <xs:annotation>  
                <xs:documentation>sample format</xs:documentation>  
              </xs:annotation>  
            </xs:attribute>  
            <xs:attribute name="ChannelLayout"  type="xs:string">  
              <xs:annotation>  
                <xs:documentation>channel layout</xs:documentation>  
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
            <xs:attribute name="Bitrate">  
              <xs:annotation>  
                <xs:documentation>average audio bit rate in bits per second, as calculated from hello AssetFile. Counts only hello elementary stream payload, and does not include hello packaging overhead</xs:documentation>  
              </xs:annotation>  
              <xs:simpleType>  
                <xs:restriction base="xs:int">  
                  <xs:minInclusive value="0"/>  
                </xs:restriction>  
              </xs:simpleType>  
            </xs:attribute>  
            <xs:attribute name="BitsPerSample">  
              <xs:annotation>  
                <xs:documentation>Bits per sample for hello wFormatTag format type</xs:documentation>  
              </xs:annotation>  
              <xs:simpleType>  
                <xs:restriction base="xs:int">  
                  <xs:minInclusive value="0"/>  
                </xs:restriction>  
              </xs:simpleType>  
            </xs:attribute>  
          </xs:extension>  
        </xs:complexContent>  
      </xs:complexType>  

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
                  <xs:element name="Programs" minOccurs="0">  
                    <xs:annotation>  
                      <xs:documentation>This is hello collection of all programs when file is MPEG-TS</xs:documentation>  
                    </xs:annotation>  
                    <xs:complexType>  
                      <xs:sequence>  
                        <xs:element name="Program" type="ProgramType" minOccurs="0" maxOccurs="unbounded" />  
                      </xs:sequence>  
                    </xs:complexType>  
                  </xs:element>  
                  <xs:element name="VideoTracks" minOccurs="0">  
                    <xs:annotation>  
                      <xs:documentation>Each physical AssetFile can contain in it zero or more video tracks interleaved into an appropriate container format. This is hello collection of all those video tracks</xs:documentation>  
                    </xs:annotation>  
                    <xs:complexType>  
                      <xs:sequence>  
                        <xs:element name="VideoTrack" type="VideoTrackType" minOccurs="0" maxOccurs="unbounded" />  
                      </xs:sequence>  
                    </xs:complexType>  
                  </xs:element>  
                  <xs:element name="AudioTracks" minOccurs="0">  
                    <xs:annotation>  
                      <xs:documentation>each physical AssetFile can contain in it zero or more audio tracks interleaved into an appropriate container format. This is hello collection of all those audio tracks</xs:documentation>  
                    </xs:annotation>  
                    <xs:complexType>  
                      <xs:sequence>  
                        <xs:element name="AudioTrack" type="AudioTrackType" minOccurs="0" maxOccurs="unbounded" />  
                      </xs:sequence>  
                    </xs:complexType>  
                  </xs:element>  
                  <xs:element name="Metadata" type="MetadataType" minOccurs="0" maxOccurs="unbounded" />  
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
                <xs:attribute name="Duration" type="xs:duration" use="required">  
                  <xs:annotation>  
                    <xs:documentation>content play back duration. Example: Duration="PT25M37.757S"</xs:documentation>  
                  </xs:annotation>  
                </xs:attribute>  
                <xs:attribute name="NumberOfStreams" type="xs:int" use="required">  
                  <xs:annotation>  
                    <xs:documentation>number of streams in asset file</xs:documentation>  
                  </xs:annotation>  
                </xs:attribute>  
                <xs:attribute name="FormatNames" type="xs:string" use="required">  
                  <xs:annotation>  
                    <xs:documentation>format names</xs:documentation>  
                  </xs:annotation>  
                </xs:attribute>  
                <xs:attribute name="FormatVerboseName" type="xs:string" use="required">  
                  <xs:annotation>  
                    <xs:documentation>format verbose names</xs:documentation>  
                  </xs:annotation>  
                </xs:attribute>  
                <xs:attribute name="StartTime" type="xs:duration">  
                  <xs:annotation>  
                    <xs:documentation>content start time. Example: StartTime="PT2.669S"</xs:documentation>  
                  </xs:annotation>  
                </xs:attribute>  
                <xs:attribute name="OverallBitRate">  
                  <xs:annotation>  
                    <xs:documentation>average bitrate of hello asset file in kbps</xs:documentation>  
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
    </xs:schema>  


## <span data-ttu-id="fe8cb-450"><a name="xml"></a>XML-exempel</span><span class="sxs-lookup"><span data-stu-id="fe8cb-450"><a name="xml"></a> XML example</span></span>
<span data-ttu-id="fe8cb-451">hello följande är ett exempel på hello indata metadatafil.</span><span class="sxs-lookup"><span data-stu-id="fe8cb-451">hello following is an example of hello Input metadata file.</span></span>  

    <?xml version="1.0" encoding="utf-8"?>  
    <AssetFiles xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns="http://schemas.microsoft.com/windowsazure/mediaservices/2014/07/mediaencoder/inputmetadata">  
      <AssetFile Name="bear.mp4" Size="1973733" Duration="PT12.678S" NumberOfStreams="2" FormatNames="mov,mp4,m4a,3gp,3g2,mj2" FormatVerboseName="QuickTime / MOV" StartTime="PT0S" OverallBitRate="1245">  
        <VideoTracks>  
          <VideoTrack Id="1" Codec="h264" CodecLongName="H.264 / AVC / MPEG-4 AVC / MPEG-4 part 10" TimeBase="1/29970" NumberOfFrames="375" StartTime="PT0.034S" Duration="PT12.645S" FourCC="avc1" Profile="High" Level="4.1" PixelFormat="yuv420p" Width="512" Height="384" DisplayAspectRatioNumerator="4" DisplayAspectRatioDenominator="3" SampleAspectRatioNumerator="1" SampleAspectRatioDenominator="1" FrameRate="29.656" Bitrate="1043" HasBFrames="1">  
            <Disposition Default="1" Dub="0" Original="0" Comment="0" Lyrics="0" Karaoke="0" Forced="0" HearingImpaired="0" VisualImpaired="0" CleanEffects="0" AttachedPic="0" />  
            <Metadata key="creation_time" value="2010-03-10 16:11:56" />  
            <Metadata key="language" value="eng" />  
            <Metadata key="handler_name" value="Mainconcept MP4 Video Media Handler" />  
          </VideoTrack>  
        </VideoTracks>  
        <AudioTracks>  
          <AudioTrack Id="0" Codec="aac" CodecLongName="AAC (Advanced Audio Coding)" TimeBase="1/44100" NumberOfFrames="546" StartTime="PT0S" Duration="PT12.678S" SampleFormat="fltp" ChannelLayout="stereo" Channels="2" SamplingRate="44100" Bitrate="156" BitsPerSample="0">  
            <Disposition Default="1" Dub="0" Original="0" Comment="0" Lyrics="0" Karaoke="0" Forced="0" HearingImpaired="0" VisualImpaired="0" CleanEffects="0" AttachedPic="0" />  
            <Metadata key="creation_time" value="2010-03-10 16:11:56" />  
            <Metadata key="language" value="eng" />  
            <Metadata key="handler_name" value="Mainconcept MP4 Sound Media Handler" />  
          </AudioTrack>  
        </AudioTracks>  
        <Metadata key="major_brand" value="mp42" />  
        <Metadata key="minor_version" value="0" />  
        <Metadata key="compatible_brands" value="mp42mp41" />  
        <Metadata key="creation_time" value="2010-03-10 16:11:53" />  
        <Metadata key="comment" value="Courtesy of National Geographic.  Used by Permission." />  
      </AssetFile>  
    </AssetFiles>  

## <a name="next-steps"></a><span data-ttu-id="fe8cb-452">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="fe8cb-452">Next steps</span></span>
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a><span data-ttu-id="fe8cb-453">Ge feedback</span><span class="sxs-lookup"><span data-stu-id="fe8cb-453">Provide feedback</span></span>
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

