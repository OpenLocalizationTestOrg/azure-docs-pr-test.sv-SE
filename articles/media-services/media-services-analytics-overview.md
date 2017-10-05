---
title: "Media Analytics på plattformen Media Services | Microsoft Docs"
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
ms.openlocfilehash: c0bbe6f80370515fa783b12757434897fe2221b6
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/29/2017
---
# <a name="media-analytics-on-the-media-services-platform"></a><span data-ttu-id="001c5-103">Media Analytics på plattformen Media Services</span><span class="sxs-lookup"><span data-stu-id="001c5-103">Media Analytics on the Media Services platform</span></span>
## <a name="overview"></a><span data-ttu-id="001c5-104">Översikt</span><span class="sxs-lookup"><span data-stu-id="001c5-104">Overview</span></span>
<span data-ttu-id="001c5-105">Flera organisationer använder video som önskade normal träna sina anställda, engagera kunderna och dokumentera funktionerna.</span><span class="sxs-lookup"><span data-stu-id="001c5-105">More organizations are using video as the preferred medium to train their employees, engage their customers, and document business functions.</span></span> <span data-ttu-id="001c5-106">Molnet är ett sätt att lagra strömma och komma åt dessa stora mediefiler.</span><span class="sxs-lookup"><span data-stu-id="001c5-106">Cloud computing provides a way to store, stream, and access these large media files.</span></span> <span data-ttu-id="001c5-107">Men eftersom ett bibliotek med videoinnehåll växer måste ett lika effektivt sätt att extrahera insikter från innehållet.</span><span class="sxs-lookup"><span data-stu-id="001c5-107">But as a company's library of video content grows, it needs an equally effective means of extracting insights from the content.</span></span> 

<span data-ttu-id="001c5-108">Azure Media Services erbjuder Azure Media Analytics för att tillgodose det växande behovet.</span><span class="sxs-lookup"><span data-stu-id="001c5-108">To address this growing need, Azure Media Services offers Azure Media Analytics.</span></span> <span data-ttu-id="001c5-109">Media Analytics är en samling tal- och visionskomponenter som gör det enklare för organisationer och företag att härleda insikter som det går att direkt agera utifrån från sina videofiler.</span><span class="sxs-lookup"><span data-stu-id="001c5-109">Media Analytics is a collection of speech and vision components that makes it easier for organizations and enterprises to derive actionable insights from their video files.</span></span> <span data-ttu-id="001c5-110">Inbyggda med hjälp av Media Services-plattformen kärnkomponenter kan Media Analytics hantera media bearbetning i stor skala på dagen.</span><span class="sxs-lookup"><span data-stu-id="001c5-110">Built by using the core Media Services platform components, Media Analytics can handle media processing at scale on day one.</span></span>

<span data-ttu-id="001c5-111">Med Media Analytics sätta utvecklare snabbt avancerade video funktionerna i program.</span><span class="sxs-lookup"><span data-stu-id="001c5-111">With Media Analytics, developers can quickly bring advanced video functionality into applications.</span></span> <span data-ttu-id="001c5-112">Det ger företagsmiljöer full skala, kompatibilitet, säkerhet och globalt omfattande krävs för stora organisationer.</span><span class="sxs-lookup"><span data-stu-id="001c5-112">It provides enterprise environments with the full scale, compliance, security, and global reach required by large organizations.</span></span>

<span data-ttu-id="001c5-113">Följande diagram visar Media Analytics och andra viktiga delar i plattformen Media Services.</span><span class="sxs-lookup"><span data-stu-id="001c5-113">The following diagram shows Media Analytics and other major parts of the Media Services platform.</span></span> 

![VoD-arbetsflöde](./media/media-services-analytics-overview/media-services-analytics-overview01.png)

<span data-ttu-id="001c5-115">Media Analytics-medieprocessorer producerar MP4-filer eller JSON-filer.</span><span class="sxs-lookup"><span data-stu-id="001c5-115">Media Analytics media processors produce MP4 files or JSON files.</span></span> <span data-ttu-id="001c5-116">Om en medieprocessor producerar MP4-fil, kan du hämta filen progressivt.</span><span class="sxs-lookup"><span data-stu-id="001c5-116">If a media processor produces an MP4 file, you can progressively download the file.</span></span> <span data-ttu-id="001c5-117">Om en medieprocessor producerar en JSON-fil, kan du hämta filen från Azure Blob storage.</span><span class="sxs-lookup"><span data-stu-id="001c5-117">If a media processor produces a JSON file, you can download the file from Azure Blob storage.</span></span> 

## <a name="media-analytics-services"></a><span data-ttu-id="001c5-118">Media Analytics-tjänster</span><span class="sxs-lookup"><span data-stu-id="001c5-118">Media Analytics services</span></span>

### <a name="indexer"></a><span data-ttu-id="001c5-119">Indexerare</span><span class="sxs-lookup"><span data-stu-id="001c5-119">Indexer</span></span>
<span data-ttu-id="001c5-120">Med Azure Media Indexer ska göra du innehåll sökbara och generera-textning spår.</span><span class="sxs-lookup"><span data-stu-id="001c5-120">With Azure Media Indexer, you can make content searchable and generate closed-captioning tracks.</span></span> <span data-ttu-id="001c5-121">Azure Media Indexer 2 Preview har jämfört med den tidigare versionen stöd för snabbare indexering och bredare språk.</span><span class="sxs-lookup"><span data-stu-id="001c5-121">Compared to the previous version, Azure Media Indexer 2 Preview has faster indexing and broader language support.</span></span> <span data-ttu-id="001c5-122">Språk som stöds är engelska, spanska, franska, tyska, italienska, kinesiska, portugisiska och arabiska.</span><span class="sxs-lookup"><span data-stu-id="001c5-122">Supported languages include English, Spanish, French, German, Italian, Chinese, Portuguese, and Arabic.</span></span> <span data-ttu-id="001c5-123">Mer detaljerad information och exempel finns [bearbeta videor med Azure Media Indexer 2](media-services-process-content-with-indexer2.md).</span><span class="sxs-lookup"><span data-stu-id="001c5-123">For detailed information and examples, see [Process videos with Azure Media Indexer 2](media-services-process-content-with-indexer2.md).</span></span>
### <a name="hyperlapse"></a><span data-ttu-id="001c5-124">Videostabilisering</span><span class="sxs-lookup"><span data-stu-id="001c5-124">Hyperlapse</span></span>
<span data-ttu-id="001c5-125">Microsoft Hyperlapse kombinerar videostabiliserings och tidsfördröjning möjlighet att snabbt skapa konsumeras videor från långa-innehåll.</span><span class="sxs-lookup"><span data-stu-id="001c5-125">Microsoft Hyperlapse combines video stabilization and time-lapse capability to create quick, consumable videos from your long-form content.</span></span> <span data-ttu-id="001c5-126">Förutom att skapa tidsfördröjning video kan använda du Videostabilisera för att skapa stabila videor från shaky videor som hämtats via mobiltelefoner och kameror.</span><span class="sxs-lookup"><span data-stu-id="001c5-126">Besides creating time-lapse video, you can use Hyperlapse to create stable videos from shaky videos captured via cell phones and camcorders.</span></span> <span data-ttu-id="001c5-127">Mer detaljerad information och exempel finns [Videostabilisera mediefiler med Azure Media Hyperlapse](media-services-hyperlapse-content.md).</span><span class="sxs-lookup"><span data-stu-id="001c5-127">For detailed information and examples, see [Hyperlapse media files with Azure Media Hyperlapse](media-services-hyperlapse-content.md).</span></span>
### <a name="motion-detector"></a><span data-ttu-id="001c5-128">Rörelsedetektor</span><span class="sxs-lookup"><span data-stu-id="001c5-128">Motion Detector</span></span>
<span data-ttu-id="001c5-129">Du kan använda rörelsedetektor för att identifiera rörelse i en video med stilla bakgrund.</span><span class="sxs-lookup"><span data-stu-id="001c5-129">You can use Motion Detector to detect motion in a video with stationary backgrounds.</span></span> <span data-ttu-id="001c5-130">Detta gör det möjligt att söka efter falska positiva identifieringar på rörelse händelser som identifieras av övervakningskameror.</span><span class="sxs-lookup"><span data-stu-id="001c5-130">This makes it possible to check for false positives on motion events detected by surveillance cameras.</span></span> <span data-ttu-id="001c5-131">Mer detaljerad information och exempel finns [Rörelseidentifiering för Azure Media Analytics](media-services-motion-detection.md).</span><span class="sxs-lookup"><span data-stu-id="001c5-131">For detailed information and examples, see [Motion detection for Azure Media Analytics](media-services-motion-detection.md).</span></span>
### <a name="face-detector"></a><span data-ttu-id="001c5-132">Ansiktsigenkänning</span><span class="sxs-lookup"><span data-stu-id="001c5-132">Face Detector</span></span>
<span data-ttu-id="001c5-133">Med hjälp av framsidan detektor, kan du identifiera personer ytor och deras emotikoner, inklusive lycka, sadness och oväntat.</span><span class="sxs-lookup"><span data-stu-id="001c5-133">By using Face Detector, you can detect people’s faces and their emotions, including happiness, sadness, and surprise.</span></span> <span data-ttu-id="001c5-134">Detta har flera användbara branschspecifika program, beskrivs senare, inklusive aggregering och analys av reaktion på personer som deltar i en händelse.</span><span class="sxs-lookup"><span data-stu-id="001c5-134">This has several useful industry applications, described later, including aggregating and analyzing reactions of people attending an event.</span></span> <span data-ttu-id="001c5-135">Mer detaljerad information och exempel finns [Ansikts- och känslo identifiering för Azure Media Analytics](media-services-face-and-emotion-detection.md).</span><span class="sxs-lookup"><span data-stu-id="001c5-135">For detailed information and examples, see [Face and emotion detection for Azure Media Analytics](media-services-face-and-emotion-detection.md).</span></span>
### <a name="video-summarization"></a><span data-ttu-id="001c5-136">Sammanfattning av video</span><span class="sxs-lookup"><span data-stu-id="001c5-136">Video summarization</span></span>
<span data-ttu-id="001c5-137">Sammanfattning av video kan hjälpa dig skapa sammanfattningar av långa videor automatiskt genom att intressanta kodavsnitt från källvideo.</span><span class="sxs-lookup"><span data-stu-id="001c5-137">Video summarization can help you create summaries of long videos by automatically selecting interesting snippets from the source video.</span></span> <span data-ttu-id="001c5-138">Den här möjligheten är användbart när du vill ge en snabb överblick över vad som händer i en lång video.</span><span class="sxs-lookup"><span data-stu-id="001c5-138">This ability is useful when you want to provide a quick overview of what to expect in a long video.</span></span> <span data-ttu-id="001c5-139">Mer detaljerad information och exempel finns [Använd Azure Media Video miniatyrer för att skapa sammanfattning av video](media-services-video-summarization.md).</span><span class="sxs-lookup"><span data-stu-id="001c5-139">For detailed information and examples, see [Use Azure Media Video Thumbnails to create video summarization](media-services-video-summarization.md).</span></span>
### <a name="optical-character-recognition"></a><span data-ttu-id="001c5-140">Optisk teckenigenkänning</span><span class="sxs-lookup"><span data-stu-id="001c5-140">Optical character recognition</span></span>
<span data-ttu-id="001c5-141">Du kan konvertera textinnehåll i videofiler till redigerbar, sökbara digitala text med Azure Media OCR (OCR).</span><span class="sxs-lookup"><span data-stu-id="001c5-141">With Azure Media OCR (optical character recognition), you can convert text content in video files into editable, searchable digital text.</span></span> <span data-ttu-id="001c5-142">Sedan kan du automatisera extrahering av beskrivande metadata från media video signalen.</span><span class="sxs-lookup"><span data-stu-id="001c5-142">You can then automate the extraction of meaningful metadata from the video signal of your media.</span></span>
### <a name="scalable-face-redaction"></a><span data-ttu-id="001c5-143">Skalbar ansikte bortredigering</span><span class="sxs-lookup"><span data-stu-id="001c5-143">Scalable face redaction</span></span>
<span data-ttu-id="001c5-144">Azure Media Redactor är en Media Analytics medieprocessor som erbjuder skalbara ansikte bortredigering i molnet.</span><span class="sxs-lookup"><span data-stu-id="001c5-144">Azure Media Redactor is a Media Analytics media processor that offers scalable face redaction in the cloud.</span></span> <span data-ttu-id="001c5-145">Du kan ändra videon om du vill minska ytor valda personer med hjälp av framsidan bortredigering.</span><span class="sxs-lookup"><span data-stu-id="001c5-145">By using face redaction, you can modify your video to blur faces of selected individuals.</span></span> <span data-ttu-id="001c5-146">Du kanske vill använda tjänsten ansikte bortredigering i Nyheter media eller allmän säkerhet ingår.</span><span class="sxs-lookup"><span data-stu-id="001c5-146">You might want to use the face redaction service in news media or when public safety is involved.</span></span> <span data-ttu-id="001c5-147">Några minuter med material som innehåller flera ytor kan ta timmar att redigera bort manuellt, men med den här tjänsten ansikte bortredigering tar bara några få enkla steg.</span><span class="sxs-lookup"><span data-stu-id="001c5-147">A few minutes of footage that contains multiple faces can take hours to redact manually, but with this service, face redaction takes just a few simple steps.</span></span> <span data-ttu-id="001c5-148">Mer information finns i [redigera bort personerna bakom Azure Media Analytics](media-services-face-redaction.md) artikel.</span><span class="sxs-lookup"><span data-stu-id="001c5-148">For more information, see the [Redact faces with Azure Media Analytics](media-services-face-redaction.md) article.</span></span>

## <a name="common-scenarios"></a><span data-ttu-id="001c5-149">Vanliga scenarier</span><span class="sxs-lookup"><span data-stu-id="001c5-149">Common scenarios</span></span>
<span data-ttu-id="001c5-150">Media Analytics hjälper organisationer och företag glean nya insikter från video och mer hantera effektivt stora volymer av videoinnehåll.</span><span class="sxs-lookup"><span data-stu-id="001c5-150">Media Analytics can help organizations and enterprises glean new insights from video and more effectively manage large volumes of video content.</span></span> <span data-ttu-id="001c5-151">Här följer några scenarier:</span><span class="sxs-lookup"><span data-stu-id="001c5-151">Here are several scenarios:</span></span>

* <span data-ttu-id="001c5-152">**Anropa Datacenter**.</span><span class="sxs-lookup"><span data-stu-id="001c5-152">**Call centers**.</span></span> <span data-ttu-id="001c5-153">Även med ankomsten av sociala medier underlätta kunden call Center fortfarande en stor del av kundservice transaktioner.</span><span class="sxs-lookup"><span data-stu-id="001c5-153">Even with the advent of social media, customer call centers still facilitate a large percentage of customer-service transactions.</span></span> <span data-ttu-id="001c5-154">Kodning i den här ljuddata är en stor mängd kundinformation som kan analyseras för att uppnå högre nöjda.</span><span class="sxs-lookup"><span data-stu-id="001c5-154">Encoded in this audio data is a large amount of customer information that can be analyzed to achieve higher customer satisfaction.</span></span> <span data-ttu-id="001c5-155">Med hjälp av Media indexeraren organisationer extrahera text och skapa sökindex och instrumentpaneler.</span><span class="sxs-lookup"><span data-stu-id="001c5-155">By using Media Indexer, organizations can extract text and build search indexes and dashboards.</span></span> <span data-ttu-id="001c5-156">De kan sedan extrahera intelligence runt vanligt klagomål, källor klagomål och andra relevanta data.</span><span class="sxs-lookup"><span data-stu-id="001c5-156">Then they can extract intelligence around common complaints, sources of complaints, and other relevant data.</span></span>
* <span data-ttu-id="001c5-157">**Användargenererade innehåll avbrottsmoderering**.</span><span class="sxs-lookup"><span data-stu-id="001c5-157">**User-generated content moderation**.</span></span> <span data-ttu-id="001c5-158">Många organisationer har från nyheter media uttag till polis avdelningar, offentliga portaler som accepterar användargenererade media, till exempel videoklipp och bilder.</span><span class="sxs-lookup"><span data-stu-id="001c5-158">From news media outlets to police departments, many organizations have public-facing portals that accept user-generated media such as videos and images.</span></span> <span data-ttu-id="001c5-159">Mängden innehåll kan ökar på grund av oväntade händelser.</span><span class="sxs-lookup"><span data-stu-id="001c5-159">The volume of content can spike due to unexpected events.</span></span> <span data-ttu-id="001c5-160">I dessa fall är det svårt att genomföra effektiva manuell granskning av innehållet för lämplighet.</span><span class="sxs-lookup"><span data-stu-id="001c5-160">In these scenarios, it is difficult to conduct effective manual reviews of content for appropriateness.</span></span> <span data-ttu-id="001c5-161">Kunder kan förlitar sig på tjänsten innehåll avbrottsmoderering att fokusera på innehåll som är lämpligt.</span><span class="sxs-lookup"><span data-stu-id="001c5-161">Customers can rely on the content-moderation service to focus on content that is appropriate.</span></span>
* <span data-ttu-id="001c5-162">**Övervakning**.</span><span class="sxs-lookup"><span data-stu-id="001c5-162">**Surveillance**.</span></span> <span data-ttu-id="001c5-163">Kommer en växande inventering av övervakning video med IP-kameror tillväxt används.</span><span class="sxs-lookup"><span data-stu-id="001c5-163">With the growth in use of IP cameras comes a growing inventory of surveillance video.</span></span> <span data-ttu-id="001c5-164">Manuell granskning övervakning video är tid intensiv och risken för mänskliga fel är stor.</span><span class="sxs-lookup"><span data-stu-id="001c5-164">Manually reviewing surveillance video is time intensive and prone to human error.</span></span> <span data-ttu-id="001c5-165">Media Analytics tillhandahåller tjänster som rörelseidentifiering, står inför identifiering och Videostabilisera att processen för granskning, hantera och skapa derivat enklare.</span><span class="sxs-lookup"><span data-stu-id="001c5-165">Media Analytics provides services such as motion detection, face detection, and Hyperlapse to make the process of reviewing, managing, and creating derivatives easier.</span></span>

## <a name="media-analytics-media-processors"></a><span data-ttu-id="001c5-166">Media Analytics-medieprocessorer</span><span class="sxs-lookup"><span data-stu-id="001c5-166">Media Analytics media processors</span></span>
<span data-ttu-id="001c5-167">Det här avsnittet listar Media Analytics-medieprocessorer och visar hur du använder .NET eller REST för att hämta objektet en media-processor (HP).</span><span class="sxs-lookup"><span data-stu-id="001c5-167">This section lists the Media Analytics media processors and shows how to use .NET or REST to get a media processor (MP) object.</span></span>

### <a name="mp-names"></a><span data-ttu-id="001c5-168">HP-namn</span><span class="sxs-lookup"><span data-stu-id="001c5-168">MP names</span></span>
* <span data-ttu-id="001c5-169">Azure Media Indexer 2 Preview</span><span class="sxs-lookup"><span data-stu-id="001c5-169">Azure Media Indexer 2 Preview</span></span>
* <span data-ttu-id="001c5-170">Azure Media Indexer</span><span class="sxs-lookup"><span data-stu-id="001c5-170">Azure Media Indexer</span></span>
* <span data-ttu-id="001c5-171">Azure Media Hyperlapse</span><span class="sxs-lookup"><span data-stu-id="001c5-171">Azure Media Hyperlapse</span></span>
* <span data-ttu-id="001c5-172">Azure Media Face Detector</span><span class="sxs-lookup"><span data-stu-id="001c5-172">Azure Media Face Detector</span></span>
* <span data-ttu-id="001c5-173">Azure Media Motion Detector</span><span class="sxs-lookup"><span data-stu-id="001c5-173">Azure Media Motion Detector</span></span>
* <span data-ttu-id="001c5-174">Azure Media Video Thumbnails</span><span class="sxs-lookup"><span data-stu-id="001c5-174">Azure Media Video Thumbnails</span></span>
* <span data-ttu-id="001c5-175">Azure Media OCR</span><span class="sxs-lookup"><span data-stu-id="001c5-175">Azure Media OCR</span></span>

### <a name="net"></a><span data-ttu-id="001c5-176">.NET</span><span class="sxs-lookup"><span data-stu-id="001c5-176">.NET</span></span>
<span data-ttu-id="001c5-177">Följande funktion använder ett av de angivna MP-namn och returnerar ett HP-objekt.</span><span class="sxs-lookup"><span data-stu-id="001c5-177">The following function takes one of the specified MP names and returns an MP object.</span></span>

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


### <a name="rest"></a><span data-ttu-id="001c5-178">REST</span><span class="sxs-lookup"><span data-stu-id="001c5-178">REST</span></span>
<span data-ttu-id="001c5-179">Begäran:</span><span class="sxs-lookup"><span data-stu-id="001c5-179">Request:</span></span>

    GET https://media.windows.net/api/MediaProcessors()?$filter=Name%20eq%20'Azure%20Media%20OCR' HTTP/1.1
    DataServiceVersion: 1.0;NetFx
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json
    Accept-Charset: UTF-8
    User-Agent: Microsoft ADO.NET Data Services
    Authorization: Bearer <token>
    x-ms-version: 2.12
    Host: media.windows.net

<span data-ttu-id="001c5-180">Svar:</span><span class="sxs-lookup"><span data-stu-id="001c5-180">Response:</span></span>

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

## <a name="demos"></a><span data-ttu-id="001c5-181">Demonstrationer</span><span class="sxs-lookup"><span data-stu-id="001c5-181">Demos</span></span>
<span data-ttu-id="001c5-182">Se [Azure Media Analytics demonstrationer](http://azuremedialabs.azurewebsites.net/demos/Analytics.html).</span><span class="sxs-lookup"><span data-stu-id="001c5-182">See [Azure Media Analytics demos](http://azuremedialabs.azurewebsites.net/demos/Analytics.html).</span></span>

## <a name="next-steps"></a><span data-ttu-id="001c5-183">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="001c5-183">Next steps</span></span>
<span data-ttu-id="001c5-184">Granska sökvägarna för Media Services-utbildning.</span><span class="sxs-lookup"><span data-stu-id="001c5-184">Review Media Services learning paths.</span></span>

[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a><span data-ttu-id="001c5-185">Ge feedback</span><span class="sxs-lookup"><span data-stu-id="001c5-185">Provide feedback</span></span>
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

## <a name="related-articles"></a><span data-ttu-id="001c5-186">Relaterade artiklar</span><span class="sxs-lookup"><span data-stu-id="001c5-186">Related articles</span></span>
<span data-ttu-id="001c5-187">Se [Media Services Analytics meddelande](https://azure.microsoft.com/blog/introducing-azure-media-analytics/).</span><span class="sxs-lookup"><span data-stu-id="001c5-187">See [Media Services Analytics announcement](https://azure.microsoft.com/blog/introducing-azure-media-analytics/).</span></span>

<!-- Images -->

[overview]: ./media/media-services-video-on-demand-workflow/media-services-video-on-demand.png
