---
title: Digitalisera text med Azure Media Analytics OCR | Microsoft Docs
description: "Azure Media Analytics OCR (OCR) kan du konvertera textinnehåll i videofiler till redigerbar, sökbara digitala text.  På så sätt kan du automatisera extrahering av beskrivande metadata från media video signalen."
services: media-services
documentationcenter: 
author: juliako
manager: cfowler
editor: 
ms.assetid: 307c196e-3a50-4f4b-b982-51585448ffc6
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 07/31/2017
ms.author: juliako
ms.openlocfilehash: 43f5b3a9bbec243e668c79702045094fcfedbdda
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/29/2017
---
# <a name="use-azure-media-analytics-to-convert-text-content-in-video-files-into-digital-text"></a><span data-ttu-id="69a65-104">Använd Azure Media Analytics konvertera textinnehåll i videofiler till digitala text</span><span class="sxs-lookup"><span data-stu-id="69a65-104">Use Azure Media Analytics to convert text content in video files into digital text</span></span>
## <a name="overview"></a><span data-ttu-id="69a65-105">Översikt</span><span class="sxs-lookup"><span data-stu-id="69a65-105">Overview</span></span>
<span data-ttu-id="69a65-106">Om du behöver extrahera textinnehåll från din videofiler och generera en redigerbar sökbara digitala text, använder du Azure Media Analytics OCR (OCR).</span><span class="sxs-lookup"><span data-stu-id="69a65-106">If you need to extract text content from your video files and generate an editable, searchable digital text, you should use Azure Media Analytics OCR (optical character recognition).</span></span> <span data-ttu-id="69a65-107">Denna Azure Media Processor identifierar textinnehåll i din videofiler och genererar textfiler för din användning.</span><span class="sxs-lookup"><span data-stu-id="69a65-107">This Azure Media Processor detects text content in your video files and generates text files for your use.</span></span> <span data-ttu-id="69a65-108">OCR kan du automatisera extrahering av beskrivande metadata från media video signalen.</span><span class="sxs-lookup"><span data-stu-id="69a65-108">OCR enables you to automate the extraction of meaningful metadata from the video signal of your media.</span></span>

<span data-ttu-id="69a65-109">När den används tillsammans med en sökmotor, kan du enkelt indexera media med text och förbättra identifieringsmöjligheten för ditt innehåll.</span><span class="sxs-lookup"><span data-stu-id="69a65-109">When used in conjunction with a search engine, you can easily index your media by text, and enhance the discoverability of your content.</span></span> <span data-ttu-id="69a65-110">Detta är mycket användbart i hög textrepresentation video, t.ex. en inspelning eller skärmdump av ett bildspel.</span><span class="sxs-lookup"><span data-stu-id="69a65-110">This is extremely useful in highly textual video, like a video recording or screen-capture of a slideshow presentation.</span></span> <span data-ttu-id="69a65-111">Azure OCR Media processorn är optimerad för digital text.</span><span class="sxs-lookup"><span data-stu-id="69a65-111">The Azure OCR Media Processor is optimized for digital text.</span></span>

<span data-ttu-id="69a65-112">Den **Azure Media OCR** medieprocessor är för närvarande under förhandsgranskning.</span><span class="sxs-lookup"><span data-stu-id="69a65-112">The **Azure Media OCR** media processor is currently in Preview.</span></span>

<span data-ttu-id="69a65-113">Det här avsnittet innehåller information om **Azure Media OCR** och visar hur du använder det med Media Services SDK för .NET.</span><span class="sxs-lookup"><span data-stu-id="69a65-113">This topic gives details about  **Azure Media OCR** and shows how to use it with Media Services SDK for .NET.</span></span> <span data-ttu-id="69a65-114">Ytterligare information och exempel finns [bloggen](https://azure.microsoft.com/blog/announcing-video-ocr-public-preview-new-config/).</span><span class="sxs-lookup"><span data-stu-id="69a65-114">For additional information and examples, see [this blog](https://azure.microsoft.com/blog/announcing-video-ocr-public-preview-new-config/).</span></span>

## <a name="ocr-input-files"></a><span data-ttu-id="69a65-115">OCR inkommande filer</span><span class="sxs-lookup"><span data-stu-id="69a65-115">OCR input files</span></span>
<span data-ttu-id="69a65-116">Videofiler.</span><span class="sxs-lookup"><span data-stu-id="69a65-116">Video files.</span></span> <span data-ttu-id="69a65-117">För närvarande stöds följande format: MP4, MOV och WMV.</span><span class="sxs-lookup"><span data-stu-id="69a65-117">Currently, the following formats are supported: MP4, MOV, and WMV.</span></span>

## <a name="task-configuration"></a><span data-ttu-id="69a65-118">Konfigurera aktivitet</span><span class="sxs-lookup"><span data-stu-id="69a65-118">Task configuration</span></span>
<span data-ttu-id="69a65-119">Uppgiftskonfigurationen (förinställda).</span><span class="sxs-lookup"><span data-stu-id="69a65-119">Task configuration (preset).</span></span> <span data-ttu-id="69a65-120">När du skapar en uppgift med **Azure Media OCR**, måste du ange en konfiguration som förinställningen med hjälp av JSON eller XML.</span><span class="sxs-lookup"><span data-stu-id="69a65-120">When creating a task with **Azure Media OCR**, you must specify a configuration preset using JSON  or XML.</span></span> 

>[!NOTE]
><span data-ttu-id="69a65-121">OCR-motorn tar bara en bild region med minsta 40 bildpunkter och maximala 32000 bildpunkter som en giltig inmatning i både höjd och bredd.</span><span class="sxs-lookup"><span data-stu-id="69a65-121">The OCR engine only takes an image region with minimum 40 pixels to maximum 32000 pixels as a valid input in both height/width.</span></span>
>

### <a name="attribute-descriptions"></a><span data-ttu-id="69a65-122">Attributbeskrivningar</span><span class="sxs-lookup"><span data-stu-id="69a65-122">Attribute descriptions</span></span>
| <span data-ttu-id="69a65-123">Attributets namn</span><span class="sxs-lookup"><span data-stu-id="69a65-123">Attribute name</span></span> | <span data-ttu-id="69a65-124">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="69a65-124">Description</span></span> |
| --- | --- |
|<span data-ttu-id="69a65-125">AdvancedOutput</span><span class="sxs-lookup"><span data-stu-id="69a65-125">AdvancedOutput</span></span>| <span data-ttu-id="69a65-126">Om du anger AdvancedOutput till true innehåller JSON-utdata positionsparametrarna data för varje ord (förutom fraser och regioner).</span><span class="sxs-lookup"><span data-stu-id="69a65-126">If you set AdvancedOutput to true, the JSON output will contain positional data for every single word (in addition to phrases and regions).</span></span> <span data-ttu-id="69a65-127">Om du inte vill se detaljerna flaggan till false.</span><span class="sxs-lookup"><span data-stu-id="69a65-127">If you do not want to see these details, set the flag to false.</span></span> <span data-ttu-id="69a65-128">Standardvärdet är false.</span><span class="sxs-lookup"><span data-stu-id="69a65-128">The default value is false.</span></span> <span data-ttu-id="69a65-129">Mer information finns i [bloggen](https://azure.microsoft.com/blog/azure-media-ocr-simplified-output/).</span><span class="sxs-lookup"><span data-stu-id="69a65-129">For more information, see [this blog](https://azure.microsoft.com/blog/azure-media-ocr-simplified-output/).</span></span>|
| <span data-ttu-id="69a65-130">Språk</span><span class="sxs-lookup"><span data-stu-id="69a65-130">Language</span></span> |<span data-ttu-id="69a65-131">(valfritt) beskriver språket i texten som ska eftersökas.</span><span class="sxs-lookup"><span data-stu-id="69a65-131">(optional) describes the language of text for which to look.</span></span> <span data-ttu-id="69a65-132">Något av följande: AutoDetect (standard), arabiska, ChineseSimplified, ChineseTraditional, tjeckiska, danska, nederländska, engelska, finska, franska, tyska, grekiska, ungerska, italienska, japanska, koreanska, norska, polska, portugisiska, rumänska, ryska, SerbianCyrillic, SerbianLatin, slovakiska, spanska, svenska, turkiska.</span><span class="sxs-lookup"><span data-stu-id="69a65-132">One of the following: AutoDetect (default), Arabic, ChineseSimplified, ChineseTraditional, Czech Danish, Dutch, English, Finnish, French, German,  Greek, Hungarian, Italian, Japanese, Korean, Norwegian, Polish, Portuguese, Romanian, Russian, SerbianCyrillic, SerbianLatin, Slovak, Spanish, Swedish, Turkish.</span></span> |
| <span data-ttu-id="69a65-133">TextOrientation</span><span class="sxs-lookup"><span data-stu-id="69a65-133">TextOrientation</span></span> |<span data-ttu-id="69a65-134">(valfritt) beskriver orienteringen på texten som ska eftersökas.</span><span class="sxs-lookup"><span data-stu-id="69a65-134">(optional) describes the orientation of text for which to look.</span></span>  <span data-ttu-id="69a65-135">”Left” innebär att upp i alla bokstäver är pekar mot vänster.</span><span class="sxs-lookup"><span data-stu-id="69a65-135">"Left" means that the top of all letters are pointed towards the left.</span></span>  <span data-ttu-id="69a65-136">Standardtexten (till exempel den som finns i en bok) kan anropas ”in” orienterad.</span><span class="sxs-lookup"><span data-stu-id="69a65-136">Default text (like that which can be found in a book) can be called "Up" oriented.</span></span>  <span data-ttu-id="69a65-137">Något av följande: AutoDetect (standard), höger, nedåt, vänster.</span><span class="sxs-lookup"><span data-stu-id="69a65-137">One of the following: AutoDetect (default), Up, Right, Down, Left.</span></span> |
| <span data-ttu-id="69a65-138">TimeInterval</span><span class="sxs-lookup"><span data-stu-id="69a65-138">TimeInterval</span></span> |<span data-ttu-id="69a65-139">(valfritt) beskriver samplingsfrekvensen.</span><span class="sxs-lookup"><span data-stu-id="69a65-139">(optional) describes the sampling rate.</span></span>  <span data-ttu-id="69a65-140">Standardvärdet är varje 1/2 sekund.</span><span class="sxs-lookup"><span data-stu-id="69a65-140">Default is every 1/2 second.</span></span><br/><span data-ttu-id="69a65-141">JSON-format –: mm: ss. SSS (standard 00:00:00.500)</span><span class="sxs-lookup"><span data-stu-id="69a65-141">JSON format – HH:mm:ss.SSS (default 00:00:00.500)</span></span><br/><span data-ttu-id="69a65-142">XML-format – W3C XSD varaktighet primitiv (standard PT0.5)</span><span class="sxs-lookup"><span data-stu-id="69a65-142">XML format – W3C XSD duration primitive (default PT0.5)</span></span> |
| <span data-ttu-id="69a65-143">DetectRegions</span><span class="sxs-lookup"><span data-stu-id="69a65-143">DetectRegions</span></span> |<span data-ttu-id="69a65-144">(valfritt) En matris med DetectRegion objekt att ange regioner inom ramen att identifiera text video.</span><span class="sxs-lookup"><span data-stu-id="69a65-144">(optional) An array of DetectRegion objects specifying regions within the video frame in which to detect text.</span></span><br/><span data-ttu-id="69a65-145">Ett DetectRegion objekt består av följande fyra heltalsvärden:</span><span class="sxs-lookup"><span data-stu-id="69a65-145">A DetectRegion object is made of the following four integer values:</span></span><br/><span data-ttu-id="69a65-146">Vänster – bildpunkter från den vänstra marginalen</span><span class="sxs-lookup"><span data-stu-id="69a65-146">Left – pixels from the left-margin</span></span><br/><span data-ttu-id="69a65-147">TOP – bildpunkter från den övre marginalen</span><span class="sxs-lookup"><span data-stu-id="69a65-147">Top – pixels from the top-margin</span></span><br/><span data-ttu-id="69a65-148">Bredd – bredden för regionen i bildpunkter</span><span class="sxs-lookup"><span data-stu-id="69a65-148">Width – width of the region in pixels</span></span><br/><span data-ttu-id="69a65-149">Höjd – höjd region i bildpunkter</span><span class="sxs-lookup"><span data-stu-id="69a65-149">Height – height of the region in pixels</span></span> |

#### <a name="json-preset-example"></a><span data-ttu-id="69a65-150">Förinställda JSON-exempel</span><span class="sxs-lookup"><span data-stu-id="69a65-150">JSON preset example</span></span>

    {
        "Version":1.0, 
        "Options": 
        {
            "AdvancedOutput":"true",
            "Language":"English", 
            "TimeInterval":"00:00:01.5",
            "TextOrientation":"Up",
            "DetectRegions": [
                    {
                       "Left": 10,
                       "Top": 10,
                       "Width": 100,
                       "Height": 50
                    }
             ]
        }
    }


#### <a name="xml-preset-example"></a><span data-ttu-id="69a65-151">Förinställda XML-exempel</span><span class="sxs-lookup"><span data-stu-id="69a65-151">XML preset example</span></span>
    <?xml version=""1.0"" encoding=""utf-16""?>
    <VideoOcrPreset xmlns:xsi=""http://www.w3.org/2001/XMLSchema-instance"" xmlns:xsd=""http://www.w3.org/2001/XMLSchema"" Version=""1.0"" xmlns=""http://www.windowsazure.com/media/encoding/Preset/2014/03"">
      <Options>
         <AdvancedOutput>true</AdvancedOutput>
         <Language>English</Language>
         <TimeInterval>PT1.5S</TimeInterval>
         <DetectRegions>
             <DetectRegion>
                   <Left>10</Left>
                   <Top>10</Top>
                   <Width>100</Width>
                   <Height>50</Height>
            </DetectRegion>
       </DetectRegions>
       <TextOrientation>Up</TextOrientation>
      </Options>
    </VideoOcrPreset>

## <a name="ocr-output-files"></a><span data-ttu-id="69a65-152">OCR utdatafilerna</span><span class="sxs-lookup"><span data-stu-id="69a65-152">OCR output files</span></span>
<span data-ttu-id="69a65-153">Utdata från medieprocessor OCR är en JSON-fil.</span><span class="sxs-lookup"><span data-stu-id="69a65-153">The output of the OCR media processor is a JSON file.</span></span>

### <a name="elements-of-the-output-json-file"></a><span data-ttu-id="69a65-154">Element i JSON-filen för utdata</span><span class="sxs-lookup"><span data-stu-id="69a65-154">Elements of the output JSON file</span></span>
<span data-ttu-id="69a65-155">Video OCR utdata innehåller tid-segmenterade data om tecken hittades i videon.</span><span class="sxs-lookup"><span data-stu-id="69a65-155">The Video OCR output provides time-segmented data on the characters found in your video.</span></span>  <span data-ttu-id="69a65-156">Du kan använda attribut, till exempel språk eller orientering hone in på exakt ord att du är intresserad av att analysera.</span><span class="sxs-lookup"><span data-stu-id="69a65-156">You can use attributes such as language or orientation to hone-in on exactly the words that you are interested in analyzing.</span></span> 

<span data-ttu-id="69a65-157">Utdata innehåller följande attribut:</span><span class="sxs-lookup"><span data-stu-id="69a65-157">The output contains the following attributes:</span></span>

| <span data-ttu-id="69a65-158">Element</span><span class="sxs-lookup"><span data-stu-id="69a65-158">Element</span></span> | <span data-ttu-id="69a65-159">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="69a65-159">Description</span></span> |
| --- | --- |
| <span data-ttu-id="69a65-160">tidsrymd</span><span class="sxs-lookup"><span data-stu-id="69a65-160">Timescale</span></span> |<span data-ttu-id="69a65-161">”tick” per sekund i videon</span><span class="sxs-lookup"><span data-stu-id="69a65-161">"ticks" per second of the video</span></span> |
| <span data-ttu-id="69a65-162">Offset</span><span class="sxs-lookup"><span data-stu-id="69a65-162">Offset</span></span> |<span data-ttu-id="69a65-163">tidsförskjutningen för tidsstämplar.</span><span class="sxs-lookup"><span data-stu-id="69a65-163">time offset for timestamps.</span></span> <span data-ttu-id="69a65-164">I version 1.0 av API: er för Video att detta alltid vara 0.</span><span class="sxs-lookup"><span data-stu-id="69a65-164">In version 1.0 of Video APIs, this will always be 0.</span></span> |
| <span data-ttu-id="69a65-165">ramhastighet</span><span class="sxs-lookup"><span data-stu-id="69a65-165">Framerate</span></span> |<span data-ttu-id="69a65-166">Bildrutor per sekund av videon</span><span class="sxs-lookup"><span data-stu-id="69a65-166">Frames per second of the video</span></span> |
| <span data-ttu-id="69a65-167">Bredd</span><span class="sxs-lookup"><span data-stu-id="69a65-167">width</span></span> |<span data-ttu-id="69a65-168">bredd i bildpunkter</span><span class="sxs-lookup"><span data-stu-id="69a65-168">width of the video in pixels</span></span> |
| <span data-ttu-id="69a65-169">Höjd</span><span class="sxs-lookup"><span data-stu-id="69a65-169">height</span></span> |<span data-ttu-id="69a65-170">höjden för videon i bildpunkter</span><span class="sxs-lookup"><span data-stu-id="69a65-170">height of the video in pixels</span></span> |
| <span data-ttu-id="69a65-171">fragment</span><span class="sxs-lookup"><span data-stu-id="69a65-171">Fragments</span></span> |<span data-ttu-id="69a65-172">matris med tidsbaserade segment som metadata chunked-video</span><span class="sxs-lookup"><span data-stu-id="69a65-172">array of time-based chunks of video into which the metadata is chunked</span></span> |
| <span data-ttu-id="69a65-173">start</span><span class="sxs-lookup"><span data-stu-id="69a65-173">start</span></span> |<span data-ttu-id="69a65-174">Starttid för ett fragment i ”tick”</span><span class="sxs-lookup"><span data-stu-id="69a65-174">start time of a fragment in "ticks"</span></span> |
| <span data-ttu-id="69a65-175">Varaktighet</span><span class="sxs-lookup"><span data-stu-id="69a65-175">duration</span></span> |<span data-ttu-id="69a65-176">längden på ett fragment i ”tick”</span><span class="sxs-lookup"><span data-stu-id="69a65-176">length of a fragment in "ticks"</span></span> |
| <span data-ttu-id="69a65-177">intervall</span><span class="sxs-lookup"><span data-stu-id="69a65-177">interval</span></span> |<span data-ttu-id="69a65-178">intervallet för varje händelse inom det angivna fragmentet</span><span class="sxs-lookup"><span data-stu-id="69a65-178">interval of each event within the given fragment</span></span> |
| <span data-ttu-id="69a65-179">evenemang</span><span class="sxs-lookup"><span data-stu-id="69a65-179">events</span></span> |<span data-ttu-id="69a65-180">matris som innehåller regioner</span><span class="sxs-lookup"><span data-stu-id="69a65-180">array containing regions</span></span> |
| <span data-ttu-id="69a65-181">Region</span><span class="sxs-lookup"><span data-stu-id="69a65-181">region</span></span> |<span data-ttu-id="69a65-182">objektet som representerar upptäckte ord eller fraser</span><span class="sxs-lookup"><span data-stu-id="69a65-182">object representing detected words or phrases</span></span> |
| <span data-ttu-id="69a65-183">språk</span><span class="sxs-lookup"><span data-stu-id="69a65-183">language</span></span> |<span data-ttu-id="69a65-184">språket i texten som upptäckts inom ett område</span><span class="sxs-lookup"><span data-stu-id="69a65-184">language of the text detected within a region</span></span> |
| <span data-ttu-id="69a65-185">orientering</span><span class="sxs-lookup"><span data-stu-id="69a65-185">orientation</span></span> |<span data-ttu-id="69a65-186">orienteringen på texten som upptäckts inom ett område</span><span class="sxs-lookup"><span data-stu-id="69a65-186">orientation of the text detected within a region</span></span> |
| <span data-ttu-id="69a65-187">rader</span><span class="sxs-lookup"><span data-stu-id="69a65-187">lines</span></span> |<span data-ttu-id="69a65-188">matris med rader med text som har identifierats inom ett område</span><span class="sxs-lookup"><span data-stu-id="69a65-188">array of lines of text detected within a region</span></span> |
| <span data-ttu-id="69a65-189">Text</span><span class="sxs-lookup"><span data-stu-id="69a65-189">text</span></span> |<span data-ttu-id="69a65-190">texten som</span><span class="sxs-lookup"><span data-stu-id="69a65-190">the actual text</span></span> |

### <a name="json-output-example"></a><span data-ttu-id="69a65-191">Exempel på utdata JSON</span><span class="sxs-lookup"><span data-stu-id="69a65-191">JSON output example</span></span>
<span data-ttu-id="69a65-192">I följande exempel på utdata innehåller allmän video information och flera video fragment.</span><span class="sxs-lookup"><span data-stu-id="69a65-192">The following output example contains the general video information and several video fragments.</span></span> <span data-ttu-id="69a65-193">I varje video fragmentet innehåller den varje region som identifieras av OCR HP med språket och dess textorientering.</span><span class="sxs-lookup"><span data-stu-id="69a65-193">In every video fragment, it contains every region which is detected by OCR MP with the language and its text orientation.</span></span> <span data-ttu-id="69a65-194">Området innehåller även alla word rader i den här regionen med radens text, radens position och varje ord information (ordet innehåll, position och förtroende) i den här raden.</span><span class="sxs-lookup"><span data-stu-id="69a65-194">The region also contains every word line in this region with the line’s text, the line’s position, and every word information (word content, position and confidence) in this line.</span></span> <span data-ttu-id="69a65-195">Följande är ett exempel och du placerar vissa infogade kommentarer.</span><span class="sxs-lookup"><span data-stu-id="69a65-195">The following is an example, and I put some comments inline.</span></span>

    {
        "version": 1, 
        "timescale": 90000, 
        "offset": 0, 
        "framerate": 30, 
        "width": 640, 
        "height": 480,  // general video information
        "fragments": [
            {
                "start": 0, 
                "duration": 180000, 
                "interval": 90000,  // the time information about this fragment
                "events": [
                    [
                       { 
                            "region": { // the detected region array in this fragment 
                                "language": "English",  // region language
                                "orientation": "Up",  // text orientation
                                "lines": [  // line information array in this region, including the text and the position
                                    {
                                        "text": "One Two", 
                                        "left": 10, 
                                        "top": 10, 
                                        "right": 210, 
                                        "bottom": 110, 
                                        "word": [  // word information array in this line
                                            {
                                                "text": "One", 
                                                "left": 10, 
                                                "top": 10, 
                                                "right": 110, 
                                                "bottom": 110, 
                                                "confidence": 900
                                            }, 
                                            {
                                                "text": "Two", 
                                                "left": 110, 
                                                "top": 10, 
                                                "right": 210, 
                                                "bottom": 110, 
                                                "confidence": 910
                                            }
                                        ]
                                    }
                                ]
                            }
                        }
                    ]
                ]
            }
        ]
    }

## <a name="net-sample-code"></a><span data-ttu-id="69a65-196">Exempelkod för .NET</span><span class="sxs-lookup"><span data-stu-id="69a65-196">.NET sample code</span></span>

<span data-ttu-id="69a65-197">Följande program visar hur du:</span><span class="sxs-lookup"><span data-stu-id="69a65-197">The following program shows how to:</span></span>

1. <span data-ttu-id="69a65-198">Skapa en tillgång och överför en mediefil till tillgången.</span><span class="sxs-lookup"><span data-stu-id="69a65-198">Create an asset and upload a media file into the asset.</span></span>
2. <span data-ttu-id="69a65-199">Skapa ett jobb med en konfiguration/förinställda OCR-fil.</span><span class="sxs-lookup"><span data-stu-id="69a65-199">Create a job with an OCR configuration/preset file.</span></span>
3. <span data-ttu-id="69a65-200">Hämta JSON utdatafilerna.</span><span class="sxs-lookup"><span data-stu-id="69a65-200">Download the output JSON files.</span></span> 
   
#### <a name="create-and-configure-a-visual-studio-project"></a><span data-ttu-id="69a65-201">Skapa och konfigurera ett Visual Studio-projekt</span><span class="sxs-lookup"><span data-stu-id="69a65-201">Create and configure a Visual Studio project</span></span>

<span data-ttu-id="69a65-202">Konfigurera utvecklingsmiljön och fyll i filen app.config med anslutningsinformation, enligt beskrivningen i [Media Services-utveckling med .NET](media-services-dotnet-how-to-use.md).</span><span class="sxs-lookup"><span data-stu-id="69a65-202">Set up your development environment and populate the app.config file with connection information, as described in [Media Services development with .NET](media-services-dotnet-how-to-use.md).</span></span> 

#### <a name="example"></a><span data-ttu-id="69a65-203">Exempel</span><span class="sxs-lookup"><span data-stu-id="69a65-203">Example</span></span>

    using System;
    using System.Configuration;
    using System.IO;
    using System.Linq;
    using Microsoft.WindowsAzure.MediaServices.Client;
    using System.Threading;
    using System.Threading.Tasks;

    namespace OCR
    {
        class Program
        {
            // Read values from the App.config file.
            private static readonly string _AADTenantDomain =
                ConfigurationManager.AppSettings["AADTenantDomain"];
            private static readonly string _RESTAPIEndpoint =
                ConfigurationManager.AppSettings["MediaServiceRESTAPIEndpoint"];

            // Field for service context.
            private static CloudMediaContext _context = null;

            static void Main(string[] args)
            {
                var tokenCredentials = new AzureAdTokenCredentials(_AADTenantDomain, AzureEnvironments.AzureCloudEnvironment);
                var tokenProvider = new AzureAdTokenProvider(tokenCredentials);

                _context = new CloudMediaContext(new Uri(_RESTAPIEndpoint), tokenProvider);

                // Run the OCR job.
                var asset = RunOCRJob(@"C:\supportFiles\OCR\presentation.mp4",
                                            @"C:\supportFiles\OCR\config.json");

                // Download the job output asset.
                DownloadAsset(asset, @"C:\supportFiles\OCR\Output");
            }

            static IAsset RunOCRJob(string inputMediaFilePath, string configurationFile)
            {
                // Create an asset and upload the input media file to storage.
                IAsset asset = CreateAssetAndUploadSingleFile(inputMediaFilePath,
                    "My OCR Input Asset",
                    AssetCreationOptions.None);

                // Declare a new job.
                IJob job = _context.Jobs.Create("My OCR Job");

                // Get a reference to Azure Media OCR.
                string MediaProcessorName = "Azure Media OCR";

                var processor = GetLatestMediaProcessorByName(MediaProcessorName);

                // Read configuration from the specified file.
                string configuration = File.ReadAllText(configurationFile);

                // Create a task with the encoding details, using a string preset.
                ITask task = job.Tasks.AddNew("My OCR Task",
                    processor,
                    configuration,
                    TaskOptions.None);

                // Specify the input asset.
                task.InputAssets.Add(asset);

                // Add an output asset to contain the results of the job.
                task.OutputAssets.AddNew("My OCR Output Asset", AssetCreationOptions.None);

                // Use the following event handler to check job progress.  
                job.StateChanged += new EventHandler<JobStateChangedEventArgs>(StateChanged);

                // Launch the job.
                job.Submit();

                // Check job execution and wait for job to finish.
                Task progressJobTask = job.GetExecutionProgressTask(CancellationToken.None);

                progressJobTask.Wait();

                // If job state is Error, the event handling
                // method for job progress should log errors.  Here we check
                // for error state and exit if needed.
                if (job.State == JobState.Error)
                {
                    ErrorDetail error = job.Tasks.First().ErrorDetails.First();
                    Console.WriteLine(string.Format("Error: {0}. {1}",
                                                    error.Code,
                                                    error.Message));
                    return null;
                }

                return job.OutputMediaAssets[0];
            }

            static IAsset CreateAssetAndUploadSingleFile(string filePath, string assetName, AssetCreationOptions options)
            {
                IAsset asset = _context.Assets.Create(assetName, options);

                var assetFile = asset.AssetFiles.Create(Path.GetFileName(filePath));
                assetFile.Upload(filePath);

                return asset;
            }

            static void DownloadAsset(IAsset asset, string outputDirectory)
            {
                foreach (IAssetFile file in asset.AssetFiles)
                {
                    file.Download(Path.Combine(outputDirectory, file.Name));
                }
            }

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

            static private void StateChanged(object sender, JobStateChangedEventArgs e)
            {
                Console.WriteLine("Job state changed event:");
                Console.WriteLine("  Previous state: " + e.PreviousState);
                Console.WriteLine("  Current state: " + e.CurrentState);

                switch (e.CurrentState)
                {
                    case JobState.Finished:
                        Console.WriteLine();
                        Console.WriteLine("Job is finished.");
                        Console.WriteLine();
                        break;
                    case JobState.Canceling:
                    case JobState.Queued:
                    case JobState.Scheduled:
                    case JobState.Processing:
                        Console.WriteLine("Please wait...\n");
                        break;
                    case JobState.Canceled:
                    case JobState.Error:
                        // Cast sender as a job.
                        IJob job = (IJob)sender;
                        // Display or log error details as needed.
                        // LogJobStop(job.Id);
                        break;
                    default:
                        break;
                }
            }

        }
    }

## <a name="media-services-learning-paths"></a><span data-ttu-id="69a65-204">Sökvägar för Media Services-utbildning</span><span class="sxs-lookup"><span data-stu-id="69a65-204">Media Services learning paths</span></span>
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a><span data-ttu-id="69a65-205">Ge feedback</span><span class="sxs-lookup"><span data-stu-id="69a65-205">Provide feedback</span></span>
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

## <a name="related-links"></a><span data-ttu-id="69a65-206">Relaterade länkar</span><span class="sxs-lookup"><span data-stu-id="69a65-206">Related links</span></span>
[<span data-ttu-id="69a65-207">Azure Media Services Analytics-översikt</span><span class="sxs-lookup"><span data-stu-id="69a65-207">Azure Media Services Analytics Overview</span></span>](media-services-analytics-overview.md)

