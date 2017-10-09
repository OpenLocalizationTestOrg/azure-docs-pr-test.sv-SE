---
title: "aaaMedia Analytics på hello Media Services plattform | Microsoft Docs"
description: "Översikt över förhandsversion av Media Analytics, en samling tal- och vision tjänster på företagsnivå, kompatibilitet, säkerhet och globalt omfattande"
services: media-services
documentationcenter: 
author: juliako
manager: cfowler
editor: 
ms.assetid: c56e3781-8510-4f7f-b5ff-a218c1bb6f4c
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 06/29/2017
ms.author: milanga;juliako;johndeu
ms.openlocfilehash: 7545f0532d7618164ebe65e2f4232c5f63453cfd
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="media-analytics-on-hello-media-services-platform"></a><span data-ttu-id="6502d-103">Media Analytics på hello Media Services-plattformen</span><span class="sxs-lookup"><span data-stu-id="6502d-103">Media Analytics on hello Media Services platform</span></span>
## <a name="overview"></a><span data-ttu-id="6502d-104">Översikt</span><span class="sxs-lookup"><span data-stu-id="6502d-104">Overview</span></span>
<span data-ttu-id="6502d-105">Flera organisationer använder video som hello prioriterade medelhög tootrain sina anställda, engagera sina kunder och IT-funktioner på dokumentet.</span><span class="sxs-lookup"><span data-stu-id="6502d-105">More organizations are using video as hello preferred medium tootrain their employees, engage their customers, and document business functions.</span></span> <span data-ttu-id="6502d-106">Molnet innehåller en sätt toostore strömma och komma åt dessa stora mediefiler.</span><span class="sxs-lookup"><span data-stu-id="6502d-106">Cloud computing provides a way toostore, stream, and access these large media files.</span></span> <span data-ttu-id="6502d-107">Men eftersom ett bibliotek med videoinnehåll växer måste ett lika effektivt sätt att extrahera insikter från hello innehåll.</span><span class="sxs-lookup"><span data-stu-id="6502d-107">But as a company's library of video content grows, it needs an equally effective means of extracting insights from hello content.</span></span> 

<span data-ttu-id="6502d-108">tooaddress detta växande behov Azure Media Services erbjuder Azure Media Analytics.</span><span class="sxs-lookup"><span data-stu-id="6502d-108">tooaddress this growing need, Azure Media Services offers Azure Media Analytics.</span></span> <span data-ttu-id="6502d-109">Media Analytics är en samling tal-och visionskomponenter som gör det enklare för organisationer och företag tooderive tillämplig insikter från sina videofiler.</span><span class="sxs-lookup"><span data-stu-id="6502d-109">Media Analytics is a collection of speech and vision components that makes it easier for organizations and enterprises tooderive actionable insights from their video files.</span></span> <span data-ttu-id="6502d-110">Inbyggda med hjälp av hello kärnkomponenter Media Services-plattformen kan Media Analytics hantera media bearbetning i stor skala på dagen.</span><span class="sxs-lookup"><span data-stu-id="6502d-110">Built by using hello core Media Services platform components, Media Analytics can handle media processing at scale on day one.</span></span>

<span data-ttu-id="6502d-111">Med Media Analytics sätta utvecklare snabbt avancerade video funktionerna i program.</span><span class="sxs-lookup"><span data-stu-id="6502d-111">With Media Analytics, developers can quickly bring advanced video functionality into applications.</span></span> <span data-ttu-id="6502d-112">Det ger företagsmiljöer hello full skala, kompatibilitet, säkerhet och globalt omfattande krävs för stora organisationer.</span><span class="sxs-lookup"><span data-stu-id="6502d-112">It provides enterprise environments with hello full scale, compliance, security, and global reach required by large organizations.</span></span>

<span data-ttu-id="6502d-113">hello följande diagram visar Media Analytics och andra viktiga delar av hello Media Services-plattformen.</span><span class="sxs-lookup"><span data-stu-id="6502d-113">hello following diagram shows Media Analytics and other major parts of hello Media Services platform.</span></span> 

![VoD-arbetsflöde](./media/media-services-analytics-overview/media-services-analytics-overview01.png)

<span data-ttu-id="6502d-115">Media Analytics-medieprocessorer producerar MP4-filer eller JSON-filer.</span><span class="sxs-lookup"><span data-stu-id="6502d-115">Media Analytics media processors produce MP4 files or JSON files.</span></span> <span data-ttu-id="6502d-116">Om en medieprocessor producerar MP4-fil, kan du progressivt hämta hello-fil.</span><span class="sxs-lookup"><span data-stu-id="6502d-116">If a media processor produces an MP4 file, you can progressively download hello file.</span></span> <span data-ttu-id="6502d-117">Om en medieprocessor producerar en JSON-fil, kan du ladda ned hello filen från Azure Blob storage.</span><span class="sxs-lookup"><span data-stu-id="6502d-117">If a media processor produces a JSON file, you can download hello file from Azure Blob storage.</span></span> 

## <a name="media-analytics-services"></a><span data-ttu-id="6502d-118">Media Analytics-tjänster</span><span class="sxs-lookup"><span data-stu-id="6502d-118">Media Analytics services</span></span>

### <a name="indexer"></a><span data-ttu-id="6502d-119">Indexerare</span><span class="sxs-lookup"><span data-stu-id="6502d-119">Indexer</span></span>
<span data-ttu-id="6502d-120">Med Azure Media Indexer ska göra du innehåll sökbara och generera-textning spår.</span><span class="sxs-lookup"><span data-stu-id="6502d-120">With Azure Media Indexer, you can make content searchable and generate closed-captioning tracks.</span></span> <span data-ttu-id="6502d-121">Toohello jämfört med tidigare version, Azure Media Indexer 2 Preview har stöd för snabbare indexering och bredare språk.</span><span class="sxs-lookup"><span data-stu-id="6502d-121">Compared toohello previous version, Azure Media Indexer 2 Preview has faster indexing and broader language support.</span></span> <span data-ttu-id="6502d-122">Språk som stöds är engelska, spanska, franska, tyska, italienska, kinesiska, portugisiska och arabiska.</span><span class="sxs-lookup"><span data-stu-id="6502d-122">Supported languages include English, Spanish, French, German, Italian, Chinese, Portuguese, and Arabic.</span></span> <span data-ttu-id="6502d-123">Mer detaljerad information och exempel finns [bearbeta videor med Azure Media Indexer 2](media-services-process-content-with-indexer2.md).</span><span class="sxs-lookup"><span data-stu-id="6502d-123">For detailed information and examples, see [Process videos with Azure Media Indexer 2](media-services-process-content-with-indexer2.md).</span></span>
### <a name="hyperlapse"></a><span data-ttu-id="6502d-124">Videostabilisering</span><span class="sxs-lookup"><span data-stu-id="6502d-124">Hyperlapse</span></span>
<span data-ttu-id="6502d-125">Microsoft Hyperlapse kombinerar videostabiliserings och tidsfördröjning kapaciteten toocreate snabbt och använda videor från långa-innehåll.</span><span class="sxs-lookup"><span data-stu-id="6502d-125">Microsoft Hyperlapse combines video stabilization and time-lapse capability toocreate quick, consumable videos from your long-form content.</span></span> <span data-ttu-id="6502d-126">Förutom att skapa tidsfördröjning video kan använda du Videostabilisera toocreate stabil videor från shaky videor som hämtats via mobiltelefoner och kameror.</span><span class="sxs-lookup"><span data-stu-id="6502d-126">Besides creating time-lapse video, you can use Hyperlapse toocreate stable videos from shaky videos captured via cell phones and camcorders.</span></span> <span data-ttu-id="6502d-127">Mer detaljerad information och exempel finns [Videostabilisera mediefiler med Azure Media Hyperlapse](media-services-hyperlapse-content.md).</span><span class="sxs-lookup"><span data-stu-id="6502d-127">For detailed information and examples, see [Hyperlapse media files with Azure Media Hyperlapse](media-services-hyperlapse-content.md).</span></span>
### <a name="motion-detector"></a><span data-ttu-id="6502d-128">Rörelsedetektor</span><span class="sxs-lookup"><span data-stu-id="6502d-128">Motion Detector</span></span>
<span data-ttu-id="6502d-129">Du kan använda rörelsedetektor toodetect rörelse i en video med stilla bakgrund.</span><span class="sxs-lookup"><span data-stu-id="6502d-129">You can use Motion Detector toodetect motion in a video with stationary backgrounds.</span></span> <span data-ttu-id="6502d-130">Detta gör det möjligt toocheck för falska positiva identifieringar på rörelse händelser som identifieras av övervakningskameror.</span><span class="sxs-lookup"><span data-stu-id="6502d-130">This makes it possible toocheck for false positives on motion events detected by surveillance cameras.</span></span> <span data-ttu-id="6502d-131">Mer detaljerad information och exempel finns [Rörelseidentifiering för Azure Media Analytics](media-services-motion-detection.md).</span><span class="sxs-lookup"><span data-stu-id="6502d-131">For detailed information and examples, see [Motion detection for Azure Media Analytics](media-services-motion-detection.md).</span></span>
### <a name="face-detector"></a><span data-ttu-id="6502d-132">Ansiktsigenkänning</span><span class="sxs-lookup"><span data-stu-id="6502d-132">Face Detector</span></span>
<span data-ttu-id="6502d-133">Med hjälp av framsidan detektor, kan du identifiera personer ytor och deras emotikoner, inklusive lycka, sadness och oväntat.</span><span class="sxs-lookup"><span data-stu-id="6502d-133">By using Face Detector, you can detect people’s faces and their emotions, including happiness, sadness, and surprise.</span></span> <span data-ttu-id="6502d-134">Detta har flera användbara branschspecifika program, beskrivs senare, inklusive aggregering och analys av reaktion på personer som deltar i en händelse.</span><span class="sxs-lookup"><span data-stu-id="6502d-134">This has several useful industry applications, described later, including aggregating and analyzing reactions of people attending an event.</span></span> <span data-ttu-id="6502d-135">Mer detaljerad information och exempel finns [Ansikts- och känslo identifiering för Azure Media Analytics](media-services-face-and-emotion-detection.md).</span><span class="sxs-lookup"><span data-stu-id="6502d-135">For detailed information and examples, see [Face and emotion detection for Azure Media Analytics](media-services-face-and-emotion-detection.md).</span></span>
### <a name="video-summarization"></a><span data-ttu-id="6502d-136">Sammanfattning av video</span><span class="sxs-lookup"><span data-stu-id="6502d-136">Video summarization</span></span>
<span data-ttu-id="6502d-137">Sammanfattning av video kan hjälpa dig skapa sammanfattningar av långa videor automatiskt genom att intressanta kodavsnitt från hello källa video.</span><span class="sxs-lookup"><span data-stu-id="6502d-137">Video summarization can help you create summaries of long videos by automatically selecting interesting snippets from hello source video.</span></span> <span data-ttu-id="6502d-138">Den här möjligheten är användbart när du vill tooprovide en snabb överblick över vilka tooexpect i en lång video.</span><span class="sxs-lookup"><span data-stu-id="6502d-138">This ability is useful when you want tooprovide a quick overview of what tooexpect in a long video.</span></span> <span data-ttu-id="6502d-139">Mer detaljerad information och exempel finns [sammanfattning av video Använd Azure Media Video-miniatyrer toocreate](media-services-video-summarization.md).</span><span class="sxs-lookup"><span data-stu-id="6502d-139">For detailed information and examples, see [Use Azure Media Video Thumbnails toocreate video summarization](media-services-video-summarization.md).</span></span>
### <a name="optical-character-recognition"></a><span data-ttu-id="6502d-140">Optisk teckenigenkänning</span><span class="sxs-lookup"><span data-stu-id="6502d-140">Optical character recognition</span></span>
<span data-ttu-id="6502d-141">Du kan konvertera textinnehåll i videofiler till redigerbar, sökbara digitala text med Azure Media OCR (OCR).</span><span class="sxs-lookup"><span data-stu-id="6502d-141">With Azure Media OCR (optical character recognition), you can convert text content in video files into editable, searchable digital text.</span></span> <span data-ttu-id="6502d-142">Sedan kan du automatisera hello extrahering av beskrivande metadata från hello video signal medieinnehåll.</span><span class="sxs-lookup"><span data-stu-id="6502d-142">You can then automate hello extraction of meaningful metadata from hello video signal of your media.</span></span>
### <a name="scalable-face-redaction"></a><span data-ttu-id="6502d-143">Skalbar ansikte bortredigering</span><span class="sxs-lookup"><span data-stu-id="6502d-143">Scalable face redaction</span></span>
<span data-ttu-id="6502d-144">Azure Media Redactor är en Media Analytics medieprocessor som erbjuder skalbara ansikte bortredigering i hello molnet.</span><span class="sxs-lookup"><span data-stu-id="6502d-144">Azure Media Redactor is a Media Analytics media processor that offers scalable face redaction in hello cloud.</span></span> <span data-ttu-id="6502d-145">Du kan ändra din video tooblur ytor valda personer med hjälp av framsidan bortredigering.</span><span class="sxs-lookup"><span data-stu-id="6502d-145">By using face redaction, you can modify your video tooblur faces of selected individuals.</span></span> <span data-ttu-id="6502d-146">Du kanske vill toouse hello ansikte bortredigering tjänst i Nyheter media eller allmän säkerhet ingår.</span><span class="sxs-lookup"><span data-stu-id="6502d-146">You might want toouse hello face redaction service in news media or when public safety is involved.</span></span> <span data-ttu-id="6502d-147">Några minuter med material som innehåller flera ytor kan ta timmar tooredact manuellt, men med den här tjänsten ansikte bortredigering tar bara några få enkla steg.</span><span class="sxs-lookup"><span data-stu-id="6502d-147">A few minutes of footage that contains multiple faces can take hours tooredact manually, but with this service, face redaction takes just a few simple steps.</span></span> <span data-ttu-id="6502d-148">Mer information finns i hello [redigera bort personerna bakom Azure Media Analytics](media-services-face-redaction.md) artikel.</span><span class="sxs-lookup"><span data-stu-id="6502d-148">For more information, see hello [Redact faces with Azure Media Analytics](media-services-face-redaction.md) article.</span></span>

## <a name="common-scenarios"></a><span data-ttu-id="6502d-149">Vanliga scenarier</span><span class="sxs-lookup"><span data-stu-id="6502d-149">Common scenarios</span></span>
<span data-ttu-id="6502d-150">Media Analytics hjälper organisationer och företag glean nya insikter från video och mer hantera effektivt stora volymer av videoinnehåll.</span><span class="sxs-lookup"><span data-stu-id="6502d-150">Media Analytics can help organizations and enterprises glean new insights from video and more effectively manage large volumes of video content.</span></span> <span data-ttu-id="6502d-151">Här följer några scenarier:</span><span class="sxs-lookup"><span data-stu-id="6502d-151">Here are several scenarios:</span></span>

* <span data-ttu-id="6502d-152">**Anropa Datacenter**.</span><span class="sxs-lookup"><span data-stu-id="6502d-152">**Call centers**.</span></span> <span data-ttu-id="6502d-153">Även om hello ankomsten av sociala medier underlätta kunden call Center fortfarande en stor del av kundservice transaktioner.</span><span class="sxs-lookup"><span data-stu-id="6502d-153">Even with hello advent of social media, customer call centers still facilitate a large percentage of customer-service transactions.</span></span> <span data-ttu-id="6502d-154">Kodning i den här ljuddata är en stor mängd kundinformation som kan vara analyseras tooachieve högre nöjda.</span><span class="sxs-lookup"><span data-stu-id="6502d-154">Encoded in this audio data is a large amount of customer information that can be analyzed tooachieve higher customer satisfaction.</span></span> <span data-ttu-id="6502d-155">Med hjälp av Media indexeraren organisationer extrahera text och skapa sökindex och instrumentpaneler.</span><span class="sxs-lookup"><span data-stu-id="6502d-155">By using Media Indexer, organizations can extract text and build search indexes and dashboards.</span></span> <span data-ttu-id="6502d-156">De kan sedan extrahera intelligence runt vanligt klagomål, källor klagomål och andra relevanta data.</span><span class="sxs-lookup"><span data-stu-id="6502d-156">Then they can extract intelligence around common complaints, sources of complaints, and other relevant data.</span></span>
* <span data-ttu-id="6502d-157">**Användargenererade innehåll avbrottsmoderering**.</span><span class="sxs-lookup"><span data-stu-id="6502d-157">**User-generated content moderation**.</span></span> <span data-ttu-id="6502d-158">Många organisationer har offentliga portaler som accepterar användargenererade media, till exempel videoklipp och bilder från nyheter media uttag toopolice avdelningar.</span><span class="sxs-lookup"><span data-stu-id="6502d-158">From news media outlets toopolice departments, many organizations have public-facing portals that accept user-generated media such as videos and images.</span></span> <span data-ttu-id="6502d-159">hello mängd innehåll kan ökar på grund av toounexpected händelser.</span><span class="sxs-lookup"><span data-stu-id="6502d-159">hello volume of content can spike due toounexpected events.</span></span> <span data-ttu-id="6502d-160">I dessa scenarier är det svårt tooconduct effektiva manuell granskning av innehållet för lämplighet.</span><span class="sxs-lookup"><span data-stu-id="6502d-160">In these scenarios, it is difficult tooconduct effective manual reviews of content for appropriateness.</span></span> <span data-ttu-id="6502d-161">Kunder kan förlita dig på hello innehåll avbrottsmoderering service toofocus på innehåll som är lämpligt.</span><span class="sxs-lookup"><span data-stu-id="6502d-161">Customers can rely on hello content-moderation service toofocus on content that is appropriate.</span></span>
* <span data-ttu-id="6502d-162">**Övervakning**.</span><span class="sxs-lookup"><span data-stu-id="6502d-162">**Surveillance**.</span></span> <span data-ttu-id="6502d-163">Med hello kommer tillväxt används av IP-kameror en växande inventering av övervakning video.</span><span class="sxs-lookup"><span data-stu-id="6502d-163">With hello growth in use of IP cameras comes a growing inventory of surveillance video.</span></span> <span data-ttu-id="6502d-164">Manuell granskning övervakning video är intensiv och felbenägna toohuman fel.</span><span class="sxs-lookup"><span data-stu-id="6502d-164">Manually reviewing surveillance video is time intensive and prone toohuman error.</span></span> <span data-ttu-id="6502d-165">Media Analytics tillhandahåller tjänster som rörelseidentifiering, står inför identifiering och Videostabilisera toomake hello processen för granskning, hantera och skapa derivat enklare.</span><span class="sxs-lookup"><span data-stu-id="6502d-165">Media Analytics provides services such as motion detection, face detection, and Hyperlapse toomake hello process of reviewing, managing, and creating derivatives easier.</span></span>

## <a name="media-analytics-media-processors"></a><span data-ttu-id="6502d-166">Media Analytics-medieprocessorer</span><span class="sxs-lookup"><span data-stu-id="6502d-166">Media Analytics media processors</span></span>
<span data-ttu-id="6502d-167">Detta avsnitt visar hello Media Analytics-medieprocessorer och visar hur toouse .NET eller REST tooget objektet en media-processor (HP).</span><span class="sxs-lookup"><span data-stu-id="6502d-167">This section lists hello Media Analytics media processors and shows how toouse .NET or REST tooget a media processor (MP) object.</span></span>

### <a name="mp-names"></a><span data-ttu-id="6502d-168">HP-namn</span><span class="sxs-lookup"><span data-stu-id="6502d-168">MP names</span></span>
* <span data-ttu-id="6502d-169">Azure Media Indexer 2 Preview</span><span class="sxs-lookup"><span data-stu-id="6502d-169">Azure Media Indexer 2 Preview</span></span>
* <span data-ttu-id="6502d-170">Azure Media Indexer</span><span class="sxs-lookup"><span data-stu-id="6502d-170">Azure Media Indexer</span></span>
* <span data-ttu-id="6502d-171">Azure Media Hyperlapse</span><span class="sxs-lookup"><span data-stu-id="6502d-171">Azure Media Hyperlapse</span></span>
* <span data-ttu-id="6502d-172">Azure Media Face Detector</span><span class="sxs-lookup"><span data-stu-id="6502d-172">Azure Media Face Detector</span></span>
* <span data-ttu-id="6502d-173">Azure Media Motion Detector</span><span class="sxs-lookup"><span data-stu-id="6502d-173">Azure Media Motion Detector</span></span>
* <span data-ttu-id="6502d-174">Azure Media Video Thumbnails</span><span class="sxs-lookup"><span data-stu-id="6502d-174">Azure Media Video Thumbnails</span></span>
* <span data-ttu-id="6502d-175">Azure Media OCR</span><span class="sxs-lookup"><span data-stu-id="6502d-175">Azure Media OCR</span></span>

### <a name="net"></a><span data-ttu-id="6502d-176">.NET</span><span class="sxs-lookup"><span data-stu-id="6502d-176">.NET</span></span>
<span data-ttu-id="6502d-177">hello följande tar en av hello angetts MP namn och returnerar ett HP-objekt.</span><span class="sxs-lookup"><span data-stu-id="6502d-177">hello following function takes one of hello specified MP names and returns an MP object.</span></span>

    static IMediaProcessor GetLatestMediaProcessorByName(string mediaProcessorName)
    {
        var processor = _context.MediaProcessors
            .Where(p => p.Name == mediaProcessorName)
            .ToList()
            .OrderBy(p => new Version(p.Version))
            .LastOrDefault();

        if (processor == null)
            throw new ArgumentException(string.Format("Unknown media processor",
                                                       mediaProcessorName));

        return processor;
    }


### <a name="rest"></a><span data-ttu-id="6502d-178">REST</span><span class="sxs-lookup"><span data-stu-id="6502d-178">REST</span></span>
<span data-ttu-id="6502d-179">Begäran:</span><span class="sxs-lookup"><span data-stu-id="6502d-179">Request:</span></span>

    GET https://media.windows.net/api/MediaProcessors()?$filter=Name%20eq%20'Azure%20Media%20OCR' HTTP/1.1
    DataServiceVersion: 1.0;NetFx
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json
    Accept-Charset: UTF-8
    User-Agent: Microsoft ADO.NET Data Services
    Authorization: Bearer <token>
    x-ms-version: 2.12
    Host: media.windows.net

<span data-ttu-id="6502d-180">Svar:</span><span class="sxs-lookup"><span data-stu-id="6502d-180">Response:</span></span>

    . . .

    {  
       "odata.metadata":"https://media.windows.net/api/$metadata#MediaProcessors",
       "value":[  
          {  
             "Id":"nb:mpid:UUID:074c3899-d9fb-448f-9ae1-4ebcbe633056",
             "Description":"Azure Media OCR",
             "Name":"Azure Media OCR",
             "Sku":"",
             "Vendor":"Microsoft",
             "Version":"1.1"
          }
       ]
    }

## <a name="demos"></a><span data-ttu-id="6502d-181">Demonstrationer</span><span class="sxs-lookup"><span data-stu-id="6502d-181">Demos</span></span>
<span data-ttu-id="6502d-182">Se [Azure Media Analytics demonstrationer](http://azuremedialabs.azurewebsites.net/demos/Analytics.html).</span><span class="sxs-lookup"><span data-stu-id="6502d-182">See [Azure Media Analytics demos](http://azuremedialabs.azurewebsites.net/demos/Analytics.html).</span></span>

## <a name="next-steps"></a><span data-ttu-id="6502d-183">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="6502d-183">Next steps</span></span>
<span data-ttu-id="6502d-184">Granska sökvägarna för Media Services-utbildning.</span><span class="sxs-lookup"><span data-stu-id="6502d-184">Review Media Services learning paths.</span></span>

[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a><span data-ttu-id="6502d-185">Ge feedback</span><span class="sxs-lookup"><span data-stu-id="6502d-185">Provide feedback</span></span>
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

## <a name="related-articles"></a><span data-ttu-id="6502d-186">Relaterade artiklar</span><span class="sxs-lookup"><span data-stu-id="6502d-186">Related articles</span></span>
<span data-ttu-id="6502d-187">Se [Media Services Analytics meddelande](https://azure.microsoft.com/blog/introducing-azure-media-analytics/).</span><span class="sxs-lookup"><span data-stu-id="6502d-187">See [Media Services Analytics announcement](https://azure.microsoft.com/blog/introducing-azure-media-analytics/).</span></span>

<!-- Images -->

[overview]: ./media/media-services-video-on-demand-workflow/media-services-video-on-demand.png
