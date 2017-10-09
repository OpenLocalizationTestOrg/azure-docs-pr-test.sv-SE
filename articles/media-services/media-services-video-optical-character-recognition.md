---
title: aaaDigitize text med Azure Media Analytics OCR | Microsoft Docs
description: "Azure Media Analytics OCR (OCR) kan du tooconvert textinnehåll i videofiler till redigerbar, sökbara digitala text.  Detta möjliggör tooautomate hello extrahering av beskrivande metadata från hello video signal medieinnehåll."
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
ms.openlocfilehash: 0476c3ba3942b2c5182a34a429909adbf5c75ac9
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="use-azure-media-analytics-tooconvert-text-content-in-video-files-into-digital-text"></a><span data-ttu-id="f1aa9-104">Använd Azure Media Analytics tooconvert textinnehåll i videofiler till digitala text</span><span class="sxs-lookup"><span data-stu-id="f1aa9-104">Use Azure Media Analytics tooconvert text content in video files into digital text</span></span>
## <a name="overview"></a><span data-ttu-id="f1aa9-105">Översikt</span><span class="sxs-lookup"><span data-stu-id="f1aa9-105">Overview</span></span>
<span data-ttu-id="f1aa9-106">Om du behöver tooextract text innehåll från din videofiler och generera en redigerbar sökbara digitala text, använder du Azure Media Analytics OCR (OCR).</span><span class="sxs-lookup"><span data-stu-id="f1aa9-106">If you need tooextract text content from your video files and generate an editable, searchable digital text, you should use Azure Media Analytics OCR (optical character recognition).</span></span> <span data-ttu-id="f1aa9-107">Denna Azure Media Processor identifierar textinnehåll i din videofiler och genererar textfiler för din användning.</span><span class="sxs-lookup"><span data-stu-id="f1aa9-107">This Azure Media Processor detects text content in your video files and generates text files for your use.</span></span> <span data-ttu-id="f1aa9-108">OCR kan du tooautomate hello extrahering av beskrivande metadata från hello video signal medieinnehåll.</span><span class="sxs-lookup"><span data-stu-id="f1aa9-108">OCR enables you tooautomate hello extraction of meaningful metadata from hello video signal of your media.</span></span>

<span data-ttu-id="f1aa9-109">När den används tillsammans med en sökmotor, kan du enkelt indexera media med text och förbättra hello identifieringsmöjligheten för ditt innehåll.</span><span class="sxs-lookup"><span data-stu-id="f1aa9-109">When used in conjunction with a search engine, you can easily index your media by text, and enhance hello discoverability of your content.</span></span> <span data-ttu-id="f1aa9-110">Detta är mycket användbart i hög textrepresentation video, t.ex. en inspelning eller skärmdump av ett bildspel.</span><span class="sxs-lookup"><span data-stu-id="f1aa9-110">This is extremely useful in highly textual video, like a video recording or screen-capture of a slideshow presentation.</span></span> <span data-ttu-id="f1aa9-111">hello Azure OCR Medieprocessor är optimerad för digital text.</span><span class="sxs-lookup"><span data-stu-id="f1aa9-111">hello Azure OCR Media Processor is optimized for digital text.</span></span>

<span data-ttu-id="f1aa9-112">Hej **Azure Media OCR** medieprocessor är för närvarande under förhandsgranskning.</span><span class="sxs-lookup"><span data-stu-id="f1aa9-112">hello **Azure Media OCR** media processor is currently in Preview.</span></span>

<span data-ttu-id="f1aa9-113">Det här avsnittet innehåller information om **Azure Media OCR** och visar hur toouse med Media Services SDK för .NET.</span><span class="sxs-lookup"><span data-stu-id="f1aa9-113">This topic gives details about  **Azure Media OCR** and shows how toouse it with Media Services SDK for .NET.</span></span> <span data-ttu-id="f1aa9-114">Ytterligare information och exempel finns [bloggen](https://azure.microsoft.com/blog/announcing-video-ocr-public-preview-new-config/).</span><span class="sxs-lookup"><span data-stu-id="f1aa9-114">For additional information and examples, see [this blog](https://azure.microsoft.com/blog/announcing-video-ocr-public-preview-new-config/).</span></span>

## <a name="ocr-input-files"></a><span data-ttu-id="f1aa9-115">OCR inkommande filer</span><span class="sxs-lookup"><span data-stu-id="f1aa9-115">OCR input files</span></span>
<span data-ttu-id="f1aa9-116">Videofiler.</span><span class="sxs-lookup"><span data-stu-id="f1aa9-116">Video files.</span></span> <span data-ttu-id="f1aa9-117">För närvarande hello följande format stöds: MP4, MOV och WMV.</span><span class="sxs-lookup"><span data-stu-id="f1aa9-117">Currently, hello following formats are supported: MP4, MOV, and WMV.</span></span>

## <a name="task-configuration"></a><span data-ttu-id="f1aa9-118">Konfigurera aktivitet</span><span class="sxs-lookup"><span data-stu-id="f1aa9-118">Task configuration</span></span>
<span data-ttu-id="f1aa9-119">Uppgiftskonfigurationen (förinställda).</span><span class="sxs-lookup"><span data-stu-id="f1aa9-119">Task configuration (preset).</span></span> <span data-ttu-id="f1aa9-120">När du skapar en uppgift med **Azure Media OCR**, måste du ange en konfiguration som förinställningen med hjälp av JSON eller XML.</span><span class="sxs-lookup"><span data-stu-id="f1aa9-120">When creating a task with **Azure Media OCR**, you must specify a configuration preset using JSON  or XML.</span></span> 

>[!NOTE]
><span data-ttu-id="f1aa9-121">hello OCR-motorn tar bara en bild region med minsta 40 bildpunkter toomaximum 32000 bildpunkter som en giltig inmatning i både höjd och bredd.</span><span class="sxs-lookup"><span data-stu-id="f1aa9-121">hello OCR engine only takes an image region with minimum 40 pixels toomaximum 32000 pixels as a valid input in both height/width.</span></span>
>

### <a name="attribute-descriptions"></a><span data-ttu-id="f1aa9-122">Attributbeskrivningar</span><span class="sxs-lookup"><span data-stu-id="f1aa9-122">Attribute descriptions</span></span>
| <span data-ttu-id="f1aa9-123">Attributets namn</span><span class="sxs-lookup"><span data-stu-id="f1aa9-123">Attribute name</span></span> | <span data-ttu-id="f1aa9-124">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="f1aa9-124">Description</span></span> |
| --- | --- |
|<span data-ttu-id="f1aa9-125">AdvancedOutput</span><span class="sxs-lookup"><span data-stu-id="f1aa9-125">AdvancedOutput</span></span>| <span data-ttu-id="f1aa9-126">Om du ställer in AdvancedOutput tootrue innehåller hello JSON-utdata positionsparametrarna data för varje ord (i tillägget toophrases och regioner).</span><span class="sxs-lookup"><span data-stu-id="f1aa9-126">If you set AdvancedOutput tootrue, hello JSON output will contain positional data for every single word (in addition toophrases and regions).</span></span> <span data-ttu-id="f1aa9-127">Om du inte vill toosee detaljerna, ange hello flaggan toofalse.</span><span class="sxs-lookup"><span data-stu-id="f1aa9-127">If you do not want toosee these details, set hello flag toofalse.</span></span> <span data-ttu-id="f1aa9-128">hello standardvärdet är false.</span><span class="sxs-lookup"><span data-stu-id="f1aa9-128">hello default value is false.</span></span> <span data-ttu-id="f1aa9-129">Mer information finns i [bloggen](https://azure.microsoft.com/blog/azure-media-ocr-simplified-output/).</span><span class="sxs-lookup"><span data-stu-id="f1aa9-129">For more information, see [this blog](https://azure.microsoft.com/blog/azure-media-ocr-simplified-output/).</span></span>|
| <span data-ttu-id="f1aa9-130">Språk</span><span class="sxs-lookup"><span data-stu-id="f1aa9-130">Language</span></span> |<span data-ttu-id="f1aa9-131">(valfritt) beskriver hello språk för vilka toolook.</span><span class="sxs-lookup"><span data-stu-id="f1aa9-131">(optional) describes hello language of text for which toolook.</span></span> <span data-ttu-id="f1aa9-132">En av följande hello: AutoDetect (standard), arabiska, ChineseSimplified, ChineseTraditional, tjeckiska, danska, nederländska, engelska, finska, franska, tyska, grekiska, ungerska, italienska, japanska, koreanska, norska, polska, portugisiska, rumänska, ryska, SerbianCyrillic, SerbianLatin, slovakiska, spanska, svenska, turkiska.</span><span class="sxs-lookup"><span data-stu-id="f1aa9-132">One of hello following: AutoDetect (default), Arabic, ChineseSimplified, ChineseTraditional, Czech Danish, Dutch, English, Finnish, French, German,  Greek, Hungarian, Italian, Japanese, Korean, Norwegian, Polish, Portuguese, Romanian, Russian, SerbianCyrillic, SerbianLatin, Slovak, Spanish, Swedish, Turkish.</span></span> |
| <span data-ttu-id="f1aa9-133">TextOrientation</span><span class="sxs-lookup"><span data-stu-id="f1aa9-133">TextOrientation</span></span> |<span data-ttu-id="f1aa9-134">(valfritt) beskriver hello orientering för vilka toolook.</span><span class="sxs-lookup"><span data-stu-id="f1aa9-134">(optional) describes hello orientation of text for which toolook.</span></span>  <span data-ttu-id="f1aa9-135">”Vänstra” innebär att hello överkant versaler pekar mot hello vänster.</span><span class="sxs-lookup"><span data-stu-id="f1aa9-135">"Left" means that hello top of all letters are pointed towards hello left.</span></span>  <span data-ttu-id="f1aa9-136">Standardtexten (till exempel den som finns i en bok) kan anropas ”in” orienterad.</span><span class="sxs-lookup"><span data-stu-id="f1aa9-136">Default text (like that which can be found in a book) can be called "Up" oriented.</span></span>  <span data-ttu-id="f1aa9-137">En av följande hello: AutoDetect (standard), höger, nedåt, vänster.</span><span class="sxs-lookup"><span data-stu-id="f1aa9-137">One of hello following: AutoDetect (default), Up, Right, Down, Left.</span></span> |
| <span data-ttu-id="f1aa9-138">TimeInterval</span><span class="sxs-lookup"><span data-stu-id="f1aa9-138">TimeInterval</span></span> |<span data-ttu-id="f1aa9-139">(valfritt) beskriver hello samplingsfrekvensen.</span><span class="sxs-lookup"><span data-stu-id="f1aa9-139">(optional) describes hello sampling rate.</span></span>  <span data-ttu-id="f1aa9-140">Standardvärdet är varje 1/2 sekund.</span><span class="sxs-lookup"><span data-stu-id="f1aa9-140">Default is every 1/2 second.</span></span><br/><span data-ttu-id="f1aa9-141">JSON-format –: mm: ss. SSS (standard 00:00:00.500)</span><span class="sxs-lookup"><span data-stu-id="f1aa9-141">JSON format – HH:mm:ss.SSS (default 00:00:00.500)</span></span><br/><span data-ttu-id="f1aa9-142">XML-format – W3C XSD varaktighet primitiv (standard PT0.5)</span><span class="sxs-lookup"><span data-stu-id="f1aa9-142">XML format – W3C XSD duration primitive (default PT0.5)</span></span> |
| <span data-ttu-id="f1aa9-143">DetectRegions</span><span class="sxs-lookup"><span data-stu-id="f1aa9-143">DetectRegions</span></span> |<span data-ttu-id="f1aa9-144">(valfritt) En matris med DetectRegion objekt att ange regioner inom hello bildruta i vilken toodetect text.</span><span class="sxs-lookup"><span data-stu-id="f1aa9-144">(optional) An array of DetectRegion objects specifying regions within hello video frame in which toodetect text.</span></span><br/><span data-ttu-id="f1aa9-145">Ett DetectRegion objekt består av följande fyra heltalsvärden hello:</span><span class="sxs-lookup"><span data-stu-id="f1aa9-145">A DetectRegion object is made of hello following four integer values:</span></span><br/><span data-ttu-id="f1aa9-146">Vänster – bildpunkter från hello vänster marginal</span><span class="sxs-lookup"><span data-stu-id="f1aa9-146">Left – pixels from hello left-margin</span></span><br/><span data-ttu-id="f1aa9-147">TOP – bildpunkter från hello top-marginal</span><span class="sxs-lookup"><span data-stu-id="f1aa9-147">Top – pixels from hello top-margin</span></span><br/><span data-ttu-id="f1aa9-148">Bredd – bredden på hello region i bildpunkter</span><span class="sxs-lookup"><span data-stu-id="f1aa9-148">Width – width of hello region in pixels</span></span><br/><span data-ttu-id="f1aa9-149">Höjd – höjd hello region i bildpunkter</span><span class="sxs-lookup"><span data-stu-id="f1aa9-149">Height – height of hello region in pixels</span></span> |

#### <a name="json-preset-example"></a><span data-ttu-id="f1aa9-150">Förinställda JSON-exempel</span><span class="sxs-lookup"><span data-stu-id="f1aa9-150">JSON preset example</span></span>

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


#### <a name="xml-preset-example"></a><span data-ttu-id="f1aa9-151">Förinställda XML-exempel</span><span class="sxs-lookup"><span data-stu-id="f1aa9-151">XML preset example</span></span>
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

## <a name="ocr-output-files"></a><span data-ttu-id="f1aa9-152">OCR utdatafilerna</span><span class="sxs-lookup"><span data-stu-id="f1aa9-152">OCR output files</span></span>
<span data-ttu-id="f1aa9-153">hello utdata hello OCR medium processorkonfiguration är en JSON-fil.</span><span class="sxs-lookup"><span data-stu-id="f1aa9-153">hello output of hello OCR media processor is a JSON file.</span></span>

### <a name="elements-of-hello-output-json-file"></a><span data-ttu-id="f1aa9-154">Element i hello utdata-JSON-fil</span><span class="sxs-lookup"><span data-stu-id="f1aa9-154">Elements of hello output JSON file</span></span>
<span data-ttu-id="f1aa9-155">hello Video OCR utdata innehåller tid-segmenterade data om hello tecken hittades i videon.</span><span class="sxs-lookup"><span data-stu-id="f1aa9-155">hello Video OCR output provides time-segmented data on hello characters found in your video.</span></span>  <span data-ttu-id="f1aa9-156">Du kan använda attribut, till exempel språk eller orientering toohone i på exakt hello ord att du är intresserad av att analysera.</span><span class="sxs-lookup"><span data-stu-id="f1aa9-156">You can use attributes such as language or orientation toohone-in on exactly hello words that you are interested in analyzing.</span></span> 

<span data-ttu-id="f1aa9-157">hello utdata innehåller hello följande attribut:</span><span class="sxs-lookup"><span data-stu-id="f1aa9-157">hello output contains hello following attributes:</span></span>

| <span data-ttu-id="f1aa9-158">Element</span><span class="sxs-lookup"><span data-stu-id="f1aa9-158">Element</span></span> | <span data-ttu-id="f1aa9-159">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="f1aa9-159">Description</span></span> |
| --- | --- |
| <span data-ttu-id="f1aa9-160">tidsrymd</span><span class="sxs-lookup"><span data-stu-id="f1aa9-160">Timescale</span></span> |<span data-ttu-id="f1aa9-161">”tick” per sekund av hello video</span><span class="sxs-lookup"><span data-stu-id="f1aa9-161">"ticks" per second of hello video</span></span> |
| <span data-ttu-id="f1aa9-162">Offset</span><span class="sxs-lookup"><span data-stu-id="f1aa9-162">Offset</span></span> |<span data-ttu-id="f1aa9-163">tidsförskjutningen för tidsstämplar.</span><span class="sxs-lookup"><span data-stu-id="f1aa9-163">time offset for timestamps.</span></span> <span data-ttu-id="f1aa9-164">I version 1.0 av API: er för Video att detta alltid vara 0.</span><span class="sxs-lookup"><span data-stu-id="f1aa9-164">In version 1.0 of Video APIs, this will always be 0.</span></span> |
| <span data-ttu-id="f1aa9-165">ramhastighet</span><span class="sxs-lookup"><span data-stu-id="f1aa9-165">Framerate</span></span> |<span data-ttu-id="f1aa9-166">Bildrutor per sekund av hello video</span><span class="sxs-lookup"><span data-stu-id="f1aa9-166">Frames per second of hello video</span></span> |
| <span data-ttu-id="f1aa9-167">Bredd</span><span class="sxs-lookup"><span data-stu-id="f1aa9-167">width</span></span> |<span data-ttu-id="f1aa9-168">bredden på hello video i bildpunkter</span><span class="sxs-lookup"><span data-stu-id="f1aa9-168">width of hello video in pixels</span></span> |
| <span data-ttu-id="f1aa9-169">Höjd</span><span class="sxs-lookup"><span data-stu-id="f1aa9-169">height</span></span> |<span data-ttu-id="f1aa9-170">höjden på hello video i bildpunkter</span><span class="sxs-lookup"><span data-stu-id="f1aa9-170">height of hello video in pixels</span></span> |
| <span data-ttu-id="f1aa9-171">fragment</span><span class="sxs-lookup"><span data-stu-id="f1aa9-171">Fragments</span></span> |<span data-ttu-id="f1aa9-172">matris med tidsbaserade segment i vilka hello metadata chunked-video</span><span class="sxs-lookup"><span data-stu-id="f1aa9-172">array of time-based chunks of video into which hello metadata is chunked</span></span> |
| <span data-ttu-id="f1aa9-173">start</span><span class="sxs-lookup"><span data-stu-id="f1aa9-173">start</span></span> |<span data-ttu-id="f1aa9-174">Starttid för ett fragment i ”tick”</span><span class="sxs-lookup"><span data-stu-id="f1aa9-174">start time of a fragment in "ticks"</span></span> |
| <span data-ttu-id="f1aa9-175">Varaktighet</span><span class="sxs-lookup"><span data-stu-id="f1aa9-175">duration</span></span> |<span data-ttu-id="f1aa9-176">längden på ett fragment i ”tick”</span><span class="sxs-lookup"><span data-stu-id="f1aa9-176">length of a fragment in "ticks"</span></span> |
| <span data-ttu-id="f1aa9-177">interval</span><span class="sxs-lookup"><span data-stu-id="f1aa9-177">interval</span></span> |<span data-ttu-id="f1aa9-178">intervallet för varje händelse inom hello angivna fragment</span><span class="sxs-lookup"><span data-stu-id="f1aa9-178">interval of each event within hello given fragment</span></span> |
| <span data-ttu-id="f1aa9-179">evenemang</span><span class="sxs-lookup"><span data-stu-id="f1aa9-179">events</span></span> |<span data-ttu-id="f1aa9-180">matris som innehåller regioner</span><span class="sxs-lookup"><span data-stu-id="f1aa9-180">array containing regions</span></span> |
| <span data-ttu-id="f1aa9-181">Region</span><span class="sxs-lookup"><span data-stu-id="f1aa9-181">region</span></span> |<span data-ttu-id="f1aa9-182">objektet som representerar upptäckte ord eller fraser</span><span class="sxs-lookup"><span data-stu-id="f1aa9-182">object representing detected words or phrases</span></span> |
| <span data-ttu-id="f1aa9-183">språk</span><span class="sxs-lookup"><span data-stu-id="f1aa9-183">language</span></span> |<span data-ttu-id="f1aa9-184">språket hello texten identifieras inom en region</span><span class="sxs-lookup"><span data-stu-id="f1aa9-184">language of hello text detected within a region</span></span> |
| <span data-ttu-id="f1aa9-185">orientering</span><span class="sxs-lookup"><span data-stu-id="f1aa9-185">orientation</span></span> |<span data-ttu-id="f1aa9-186">textens hello identifieras inom en region orientering</span><span class="sxs-lookup"><span data-stu-id="f1aa9-186">orientation of hello text detected within a region</span></span> |
| <span data-ttu-id="f1aa9-187">rader</span><span class="sxs-lookup"><span data-stu-id="f1aa9-187">lines</span></span> |<span data-ttu-id="f1aa9-188">matris med rader med text som har identifierats inom ett område</span><span class="sxs-lookup"><span data-stu-id="f1aa9-188">array of lines of text detected within a region</span></span> |
| <span data-ttu-id="f1aa9-189">Text</span><span class="sxs-lookup"><span data-stu-id="f1aa9-189">text</span></span> |<span data-ttu-id="f1aa9-190">hello faktiska text</span><span class="sxs-lookup"><span data-stu-id="f1aa9-190">hello actual text</span></span> |

### <a name="json-output-example"></a><span data-ttu-id="f1aa9-191">Exempel på utdata JSON</span><span class="sxs-lookup"><span data-stu-id="f1aa9-191">JSON output example</span></span>
<span data-ttu-id="f1aa9-192">hello innehåller följande exempel på utdata hello allmän video information och flera video fragment.</span><span class="sxs-lookup"><span data-stu-id="f1aa9-192">hello following output example contains hello general video information and several video fragments.</span></span> <span data-ttu-id="f1aa9-193">I varje video fragmentet innehåller den varje region som identifieras av OCR MP med hello språk och dess textorientering.</span><span class="sxs-lookup"><span data-stu-id="f1aa9-193">In every video fragment, it contains every region which is detected by OCR MP with hello language and its text orientation.</span></span> <span data-ttu-id="f1aa9-194">hello region innehåller även alla word rader i den här regionen med hello rad text, hello radpositionen och varje ord information (ordet innehåll, position och förtroende) i den här raden.</span><span class="sxs-lookup"><span data-stu-id="f1aa9-194">hello region also contains every word line in this region with hello line’s text, hello line’s position, and every word information (word content, position and confidence) in this line.</span></span> <span data-ttu-id="f1aa9-195">hello följande är ett exempel och du placerar vissa infogade kommentarer.</span><span class="sxs-lookup"><span data-stu-id="f1aa9-195">hello following is an example, and I put some comments inline.</span></span>

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
                "interval": 90000,  // hello time information about this fragment
                "events": [
                    [
                       { 
                            "region": { // hello detected region array in this fragment 
                                "language": "English",  // region language
                                "orientation": "Up",  // text orientation
                                "lines": [  // line information array in this region, including hello text and hello position
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

## <a name="net-sample-code"></a><span data-ttu-id="f1aa9-196">Exempelkod för .NET</span><span class="sxs-lookup"><span data-stu-id="f1aa9-196">.NET sample code</span></span>

<span data-ttu-id="f1aa9-197">hello följande program visar hur du:</span><span class="sxs-lookup"><span data-stu-id="f1aa9-197">hello following program shows how to:</span></span>

1. <span data-ttu-id="f1aa9-198">Skapa en tillgång och överför en mediefil till hello tillgång.</span><span class="sxs-lookup"><span data-stu-id="f1aa9-198">Create an asset and upload a media file into hello asset.</span></span>
2. <span data-ttu-id="f1aa9-199">Skapa ett jobb med en konfiguration/förinställda OCR-fil.</span><span class="sxs-lookup"><span data-stu-id="f1aa9-199">Create a job with an OCR configuration/preset file.</span></span>
3. <span data-ttu-id="f1aa9-200">Hämta hello utdata JSON-filer.</span><span class="sxs-lookup"><span data-stu-id="f1aa9-200">Download hello output JSON files.</span></span> 
   
#### <a name="create-and-configure-a-visual-studio-project"></a><span data-ttu-id="f1aa9-201">Skapa och konfigurera ett Visual Studio-projekt</span><span class="sxs-lookup"><span data-stu-id="f1aa9-201">Create and configure a Visual Studio project</span></span>

<span data-ttu-id="f1aa9-202">Konfigurera utvecklingsmiljön och fylla hello app.config-fil med anslutningsinformation, enligt beskrivningen i [Media Services-utveckling med .NET](media-services-dotnet-how-to-use.md).</span><span class="sxs-lookup"><span data-stu-id="f1aa9-202">Set up your development environment and populate hello app.config file with connection information, as described in [Media Services development with .NET](media-services-dotnet-how-to-use.md).</span></span> 

#### <a name="example"></a><span data-ttu-id="f1aa9-203">Exempel</span><span class="sxs-lookup"><span data-stu-id="f1aa9-203">Example</span></span>

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
            // Read values from hello App.config file.
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

                // Run hello OCR job.
                var asset = RunOCRJob(@"C:\supportFiles\OCR\presentation.mp4",
                                            @"C:\supportFiles\OCR\config.json");

                // Download hello job output asset.
                DownloadAsset(asset, @"C:\supportFiles\OCR\Output");
            }

            static IAsset RunOCRJob(string inputMediaFilePath, string configurationFile)
            {
                // Create an asset and upload hello input media file toostorage.
                IAsset asset = CreateAssetAndUploadSingleFile(inputMediaFilePath,
                    "My OCR Input Asset",
                    AssetCreationOptions.None);

                // Declare a new job.
                IJob job = _context.Jobs.Create("My OCR Job");

                // Get a reference tooAzure Media OCR.
                string MediaProcessorName = "Azure Media OCR";

                var processor = GetLatestMediaProcessorByName(MediaProcessorName);

                // Read configuration from hello specified file.
                string configuration = File.ReadAllText(configurationFile);

                // Create a task with hello encoding details, using a string preset.
                ITask task = job.Tasks.AddNew("My OCR Task",
                    processor,
                    configuration,
                    TaskOptions.None);

                // Specify hello input asset.
                task.InputAssets.Add(asset);

                // Add an output asset toocontain hello results of hello job.
                task.OutputAssets.AddNew("My OCR Output Asset", AssetCreationOptions.None);

                // Use hello following event handler toocheck job progress.  
                job.StateChanged += new EventHandler<JobStateChangedEventArgs>(StateChanged);

                // Launch hello job.
                job.Submit();

                // Check job execution and wait for job toofinish.
                Task progressJobTask = job.GetExecutionProgressTask(CancellationToken.None);

                progressJobTask.Wait();

                // If job state is Error, hello event handling
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

## <a name="media-services-learning-paths"></a><span data-ttu-id="f1aa9-204">Sökvägar för Media Services-utbildning</span><span class="sxs-lookup"><span data-stu-id="f1aa9-204">Media Services learning paths</span></span>
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a><span data-ttu-id="f1aa9-205">Ge feedback</span><span class="sxs-lookup"><span data-stu-id="f1aa9-205">Provide feedback</span></span>
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

## <a name="related-links"></a><span data-ttu-id="f1aa9-206">Relaterade länkar</span><span class="sxs-lookup"><span data-stu-id="f1aa9-206">Related links</span></span>
[<span data-ttu-id="f1aa9-207">Azure Media Services Analytics-översikt</span><span class="sxs-lookup"><span data-stu-id="f1aa9-207">Azure Media Services Analytics Overview</span></span>](media-services-analytics-overview.md)

