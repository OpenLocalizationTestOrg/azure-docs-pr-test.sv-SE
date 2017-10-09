---
title: "aaaDetect Ansikts- och Känslo med Azure Media Analytics | Microsoft Docs"
description: "Det här avsnittet visar hur toodetect står och emotikoner med Azure Media Analytics."
services: media-services
documentationcenter: 
author: juliako
manager: cfowler
editor: 
ms.assetid: 5ca4692c-23f1-451d-9d82-cbc8bf0fd707
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 07/18/2017
ms.author: milanga;juliako;
ms.openlocfilehash: f58d81d82dde08a694cdb4d92c6bab6a40a9c157
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="detect-face-and-emotion-with-azure-media-analytics"></a><span data-ttu-id="19842-103">Identifiera Ansikts- och Känslo med Azure Media Analytics</span><span class="sxs-lookup"><span data-stu-id="19842-103">Detect Face and Emotion with Azure Media Analytics</span></span>
## <a name="overview"></a><span data-ttu-id="19842-104">Översikt</span><span class="sxs-lookup"><span data-stu-id="19842-104">Overview</span></span>
<span data-ttu-id="19842-105">Hej **Azure Media ansikte detektor** medieprocessor (HP) kan du toocount, spåra förflyttningar och även mätaren målgruppen deltagande och reaktion via ansikte uttryck.</span><span class="sxs-lookup"><span data-stu-id="19842-105">hello **Azure Media Face Detector** media processor (MP) enables you toocount, track movements, and even gauge audience participation and reaction via facial expressions.</span></span> <span data-ttu-id="19842-106">Den här tjänsten innehåller två funktioner:</span><span class="sxs-lookup"><span data-stu-id="19842-106">This service contains two features:</span></span> 

* <span data-ttu-id="19842-107">**Ansikts-identifiering**</span><span class="sxs-lookup"><span data-stu-id="19842-107">**Face detection**</span></span>
  
    <span data-ttu-id="19842-108">Identifiering av framsidan hittar och spårar mänsklig ytor inom en video.</span><span class="sxs-lookup"><span data-stu-id="19842-108">Face detection finds and tracks human faces within a video.</span></span> <span data-ttu-id="19842-109">Flera ytor kan identifieras och spåras senare när de flyttas runt, med hello tid och plats metadata som returneras i en JSON-fil.</span><span class="sxs-lookup"><span data-stu-id="19842-109">Multiple faces can be detected and subsequently be tracked as they move around, with hello time and location metadata returned in a JSON file.</span></span> <span data-ttu-id="19842-110">Under spårning försöker toogive en konsekvent ID toohello samma står inför när hello person flyttar på skärmen, även om de hindras eller lämna en kort hello ram.</span><span class="sxs-lookup"><span data-stu-id="19842-110">During tracking, it will attempt toogive a consistent ID toohello same face while hello person is moving around on screen, even if they are obstructed or briefly leave hello frame.</span></span>
  
  > [!NOTE]
  > <span data-ttu-id="19842-111">Den här tjänsten inte att utföra ansiktsigenkänning.</span><span class="sxs-lookup"><span data-stu-id="19842-111">This service does not perform facial recognition.</span></span> <span data-ttu-id="19842-112">En person som lämnar hello ram eller blir hindras för länge får ett nytt-ID när de kommer tillbaka.</span><span class="sxs-lookup"><span data-stu-id="19842-112">An individual who leaves hello frame or becomes obstructed for too long will be given a new ID when they return.</span></span>
  > 
  > 
* <span data-ttu-id="19842-113">**Känsloigenkänning**</span><span class="sxs-lookup"><span data-stu-id="19842-113">**Emotion detection**</span></span>
  
    <span data-ttu-id="19842-114">Känsloigenkänning är en valfri komponent i hello ansikte identifiering Medieprocessor som returnerar analysis på flera känslomässiga attribut från hello ytor upptäckt, inklusive lycka, sadness, fruktan, resulterande och mycket mer.</span><span class="sxs-lookup"><span data-stu-id="19842-114">Emotion Detection is an optional component of hello Face Detection Media Processor that returns analysis on multiple emotional attributes from hello faces detected, including happiness, sadness, fear, anger, and more.</span></span> 

<span data-ttu-id="19842-115">Hej **Azure Media ansikte detektor** MP är för närvarande under förhandsgranskning.</span><span class="sxs-lookup"><span data-stu-id="19842-115">hello **Azure Media Face Detector** MP is currently in Preview.</span></span>

<span data-ttu-id="19842-116">Det här avsnittet innehåller information om **Azure Media ansikte detektor** och visar hur toouse med Media Services SDK för .NET.</span><span class="sxs-lookup"><span data-stu-id="19842-116">This topic gives details about  **Azure Media Face Detector** and shows how toouse it with Media Services SDK for .NET.</span></span>

## <a name="face-detector-input-files"></a><span data-ttu-id="19842-117">Står inför detektor inkommande filer</span><span class="sxs-lookup"><span data-stu-id="19842-117">Face Detector input files</span></span>
<span data-ttu-id="19842-118">Videofiler.</span><span class="sxs-lookup"><span data-stu-id="19842-118">Video files.</span></span> <span data-ttu-id="19842-119">För närvarande hello följande format stöds: MP4, MOV och WMV.</span><span class="sxs-lookup"><span data-stu-id="19842-119">Currently, hello following formats are supported: MP4, MOV, and WMV.</span></span>

## <a name="face-detector-output-files"></a><span data-ttu-id="19842-120">Står inför detektor utdatafilerna</span><span class="sxs-lookup"><span data-stu-id="19842-120">Face Detector output files</span></span>
<span data-ttu-id="19842-121">hello ansikte identifiering och spårning API kan hög precision min plats identifieras och spårning som kan identifiera dig too64 mänsklig ytor i en video.</span><span class="sxs-lookup"><span data-stu-id="19842-121">hello face detection and tracking API provides high precision face location detection and tracking that can detect up too64 human faces in a video.</span></span> <span data-ttu-id="19842-122">Främre ytor ange hello bästa resultat vid sidoytor och små ytor (mindre än eller lika med too24x24 bildpunkter) kanske inte lika exakt.</span><span class="sxs-lookup"><span data-stu-id="19842-122">Frontal faces provide hello best results, while side faces and small faces (less than or equal too24x24 pixels) might not be as accurate.</span></span>

<span data-ttu-id="19842-123">hello identifierade och spårade ytor returneras med koordinaterna (vänster, överkant, bredd och höjd) som visar hello platsen för ytor hello bild i bildpunkter som en ansikte ID-nummer som visar hello spårning av att enskilda.</span><span class="sxs-lookup"><span data-stu-id="19842-123">hello detected and tracked faces are returned with coordinates (left, top, width, and height) indicating hello location of faces in hello image in pixels, as well as a face ID number indicating hello tracking of that individual.</span></span> <span data-ttu-id="19842-124">Ansikts-ID-nummer är felbenägna tooreset under omständigheter när hello främre yta tappas bort eller överlappas i hello ram ledde till vissa individer komma tilldelade flera ID: N.</span><span class="sxs-lookup"><span data-stu-id="19842-124">Face ID numbers are prone tooreset under circumstances when hello frontal face is lost or overlapped in hello frame, resulting in some individuals getting assigned multiple IDs.</span></span>

## <span data-ttu-id="19842-125"><a id="output_elements"></a>Element i hello utdata-JSON-fil</span><span class="sxs-lookup"><span data-stu-id="19842-125"><a id="output_elements"></a>Elements of hello output JSON file</span></span>

[!INCLUDE [media-services-analytics-output-json](../../includes/media-services-analytics-output-json.md)]

<span data-ttu-id="19842-126">Framsidan detektor använder tekniker för fragmentering (där hello metadata kan delas upp i tidsbaserade segment och du kan hämta vad du behöver bara) och segmentering (där hello händelser har delats om de blir för stor).</span><span class="sxs-lookup"><span data-stu-id="19842-126">Face Detector uses techniques of fragmentation (where hello metadata can be broken up in time-based chunks and you can download only what you need), and segmentation (where hello events are broken up in case they get too large).</span></span> <span data-ttu-id="19842-127">Med hjälp av några enkla beräkningar kan du omvandla hello data.</span><span class="sxs-lookup"><span data-stu-id="19842-127">Some simple calculations can help you transform hello data.</span></span> <span data-ttu-id="19842-128">Om exempelvis en händelse startade 6300 (ticks) med en tidsrymd för 2997 (ticks per sekund) och bildhastighet sedan 29,97 (bildrutor per sekund):</span><span class="sxs-lookup"><span data-stu-id="19842-128">For example, if an event started at 6300 (ticks), with a timescale of 2997 (ticks/sec) and framerate of 29.97 (frames/sec), then:</span></span>

* <span data-ttu-id="19842-129">Start/tidsrymd = 2.1 sekunder</span><span class="sxs-lookup"><span data-stu-id="19842-129">Start/Timescale = 2.1 seconds</span></span>
* <span data-ttu-id="19842-130">Sekunder x Framerate = 63 ramar</span><span class="sxs-lookup"><span data-stu-id="19842-130">Seconds x Framerate = 63 frames</span></span>

## <a name="face-detection-input-and-output-example"></a><span data-ttu-id="19842-131">Står inför identifiering indata och utdata exempel</span><span class="sxs-lookup"><span data-stu-id="19842-131">Face detection input and output example</span></span>
### <a name="input-video"></a><span data-ttu-id="19842-132">Indatavideo</span><span class="sxs-lookup"><span data-stu-id="19842-132">Input video</span></span>
[<span data-ttu-id="19842-133">Indatavideo</span><span class="sxs-lookup"><span data-stu-id="19842-133">Input Video</span></span>](http://ampdemo.azureedge.net/azuremediaplayer.html?url=https%3A%2F%2Freferencestream-samplestream.streaming.mediaservices.windows.net%2Fc8834d9f-0b49-4b38-bcaf-ece2746f1972%2FMicrosoft%20Convergence%202015%20%20Keynote%20Highlights.ism%2Fmanifest&amp;autoplay=false)

### <a name="task-configuration-preset"></a><span data-ttu-id="19842-134">Uppgiftskonfigurationen (förinställda)</span><span class="sxs-lookup"><span data-stu-id="19842-134">Task configuration (preset)</span></span>
<span data-ttu-id="19842-135">När du skapar en uppgift med **Azure Media ansikte detektor**, måste du ange en konfiguration förinställning.</span><span class="sxs-lookup"><span data-stu-id="19842-135">When creating a task with **Azure Media Face Detector**, you must specify a configuration preset.</span></span> <span data-ttu-id="19842-136">följande konfiguration förinställda hello gäller bara för framsidan identifiering.</span><span class="sxs-lookup"><span data-stu-id="19842-136">hello following configuration preset is just for face detection.</span></span>

    {
      "version":"1.0",
      "options":{
          "TrackingMode": "Fast"
      }
    }

#### <a name="attribute-descriptions"></a><span data-ttu-id="19842-137">Attributbeskrivningar</span><span class="sxs-lookup"><span data-stu-id="19842-137">Attribute descriptions</span></span>
| <span data-ttu-id="19842-138">Attributets namn</span><span class="sxs-lookup"><span data-stu-id="19842-138">Attribute name</span></span> | <span data-ttu-id="19842-139">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="19842-139">Description</span></span> |
| --- | --- |
| <span data-ttu-id="19842-140">Läge</span><span class="sxs-lookup"><span data-stu-id="19842-140">Mode</span></span> |<span data-ttu-id="19842-141">Snabb bearbetning fast - hastighet, men mindre exakt (standard).</span><span class="sxs-lookup"><span data-stu-id="19842-141">Fast - fast processing speed, but less accurate (default).</span></span>|

### <a name="json-output"></a><span data-ttu-id="19842-142">JSON-utdata</span><span class="sxs-lookup"><span data-stu-id="19842-142">JSON output</span></span>
<span data-ttu-id="19842-143">hello följande exempel på JSON-utdata har trunkerats.</span><span class="sxs-lookup"><span data-stu-id="19842-143">hello following example of JSON output was truncated.</span></span>

    {
    "version": 1,
    "timescale": 30000,
    "offset": 0,
    "framerate": 29.97,
    "width": 1280,
    "height": 720,
    "fragments": [
        {
        "start": 0,
        "duration": 60060
        },
        {
        "start": 60060,
        "duration": 60060,
        "interval": 1001,
        "events": [
            [
            {
                "id": 0,
                "x": 0.519531,
                "y": 0.180556,
                "width": 0.0867188,
                "height": 0.154167
            }
            ],
            [
            {
                "id": 0,
                "x": 0.517969,
                "y": 0.181944,
                "width": 0.0867188,
                "height": 0.154167
            }
            ],
            [
            {
                "id": 0,
                "x": 0.517187,
                "y": 0.183333,
                "width": 0.0851562,
                "height": 0.151389
            }
            ],

        . . . 

## <a name="emotion-detection-input-and-output-example"></a><span data-ttu-id="19842-144">Känsloigenkänning indata och utdata exempel</span><span class="sxs-lookup"><span data-stu-id="19842-144">Emotion detection input and output example</span></span>
### <a name="input-video"></a><span data-ttu-id="19842-145">Indatavideo</span><span class="sxs-lookup"><span data-stu-id="19842-145">Input video</span></span>
[<span data-ttu-id="19842-146">Indatavideo</span><span class="sxs-lookup"><span data-stu-id="19842-146">Input Video</span></span>](http://ampdemo.azureedge.net/azuremediaplayer.html?url=https%3A%2F%2Freferencestream-samplestream.streaming.mediaservices.windows.net%2Fc8834d9f-0b49-4b38-bcaf-ece2746f1972%2FMicrosoft%20Convergence%202015%20%20Keynote%20Highlights.ism%2Fmanifest&amp;autoplay=false)

### <a name="task-configuration-preset"></a><span data-ttu-id="19842-147">Uppgiftskonfigurationen (förinställda)</span><span class="sxs-lookup"><span data-stu-id="19842-147">Task configuration (preset)</span></span>
<span data-ttu-id="19842-148">När du skapar en uppgift med **Azure Media ansikte detektor**, måste du ange en konfiguration förinställning.</span><span class="sxs-lookup"><span data-stu-id="19842-148">When creating a task with **Azure Media Face Detector**, you must specify a configuration preset.</span></span> <span data-ttu-id="19842-149">följande konfiguration förinställda hello anger toocreate JSON baserat på hello känsloigenkänning.</span><span class="sxs-lookup"><span data-stu-id="19842-149">hello following configuration preset specifies toocreate JSON based on hello emotion detection.</span></span>

    {
      "version": "1.0",
      "options": {
        "aggregateEmotionWindowMs": "987",
        "mode": "aggregateEmotion",
        "aggregateEmotionIntervalMs": "342"
      }
    }


#### <a name="attribute-descriptions"></a><span data-ttu-id="19842-150">Attributbeskrivningar</span><span class="sxs-lookup"><span data-stu-id="19842-150">Attribute descriptions</span></span>
| <span data-ttu-id="19842-151">Attributets namn</span><span class="sxs-lookup"><span data-stu-id="19842-151">Attribute name</span></span> | <span data-ttu-id="19842-152">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="19842-152">Description</span></span> |
| --- | --- |
| <span data-ttu-id="19842-153">Läge</span><span class="sxs-lookup"><span data-stu-id="19842-153">Mode</span></span> |<span data-ttu-id="19842-154">Ytor: Inför endast identifiering.</span><span class="sxs-lookup"><span data-stu-id="19842-154">Faces: Only face detection.</span></span><br/><span data-ttu-id="19842-155">PerFaceEmotion: Returnerar känslo separat för varje ansikte identifiering.</span><span class="sxs-lookup"><span data-stu-id="19842-155">PerFaceEmotion: Return emotion independently for each face detection.</span></span><br/><span data-ttu-id="19842-156">AggregateEmotion: Returnerar genomsnittlig känslo värden för alla ytor i ram.</span><span class="sxs-lookup"><span data-stu-id="19842-156">AggregateEmotion: Return average emotion values for all faces in frame.</span></span> |
| <span data-ttu-id="19842-157">AggregateEmotionWindowMs</span><span class="sxs-lookup"><span data-stu-id="19842-157">AggregateEmotionWindowMs</span></span> |<span data-ttu-id="19842-158">Använd om AggregateEmotion läge har valt.</span><span class="sxs-lookup"><span data-stu-id="19842-158">Use if AggregateEmotion mode selected.</span></span> <span data-ttu-id="19842-159">Anger hello lång video används tooproduce varje sammanlagda resultat, i millisekunder.</span><span class="sxs-lookup"><span data-stu-id="19842-159">Specifies hello length of video used tooproduce each aggregate result, in milliseconds.</span></span> |
| <span data-ttu-id="19842-160">AggregateEmotionIntervalMs</span><span class="sxs-lookup"><span data-stu-id="19842-160">AggregateEmotionIntervalMs</span></span> |<span data-ttu-id="19842-161">Använd om AggregateEmotion läge har valt.</span><span class="sxs-lookup"><span data-stu-id="19842-161">Use if AggregateEmotion mode selected.</span></span> <span data-ttu-id="19842-162">Anger med vilken frekvens tooproduce mängd resultat.</span><span class="sxs-lookup"><span data-stu-id="19842-162">Specifies with what frequency tooproduce aggregate results.</span></span> |

#### <a name="aggregate-defaults"></a><span data-ttu-id="19842-163">Sammanställd standardvärden</span><span class="sxs-lookup"><span data-stu-id="19842-163">Aggregate defaults</span></span>
<span data-ttu-id="19842-164">Nedan rekommenderas värden för hello aggregerad fönsterfunktion och inställningar.</span><span class="sxs-lookup"><span data-stu-id="19842-164">Below are recommended values for hello aggregate window and interval settings.</span></span> <span data-ttu-id="19842-165">AggregateEmotionWindowMs bör vara längre än AggregateEmotionIntervalMs.</span><span class="sxs-lookup"><span data-stu-id="19842-165">AggregateEmotionWindowMs should be longer than AggregateEmotionIntervalMs.</span></span>

|| <span data-ttu-id="19842-166">Standardvärden (s)</span><span class="sxs-lookup"><span data-stu-id="19842-166">Defaults(s)</span></span> | <span data-ttu-id="19842-167">Min(s)</span><span class="sxs-lookup"><span data-stu-id="19842-167">Min(s)</span></span> | <span data-ttu-id="19842-168">Max(s)</span><span class="sxs-lookup"><span data-stu-id="19842-168">Max(s)</span></span> |
|--- | --- | --- | --- |
| <span data-ttu-id="19842-169">AggregateEmotionWindowMs</span><span class="sxs-lookup"><span data-stu-id="19842-169">AggregateEmotionWindowMs</span></span> |<span data-ttu-id="19842-170">0.5</span><span class="sxs-lookup"><span data-stu-id="19842-170">0.5</span></span> |<span data-ttu-id="19842-171">2</span><span class="sxs-lookup"><span data-stu-id="19842-171">2</span></span> |<span data-ttu-id="19842-172">0.25</span><span class="sxs-lookup"><span data-stu-id="19842-172">0.25</span></span>|
| <span data-ttu-id="19842-173">AggregateEmotionIntervalMs</span><span class="sxs-lookup"><span data-stu-id="19842-173">AggregateEmotionIntervalMs</span></span> |<span data-ttu-id="19842-174">0.5</span><span class="sxs-lookup"><span data-stu-id="19842-174">0.5</span></span> |<span data-ttu-id="19842-175">1</span><span class="sxs-lookup"><span data-stu-id="19842-175">1</span></span> |<span data-ttu-id="19842-176">0.25</span><span class="sxs-lookup"><span data-stu-id="19842-176">0.25</span></span>|

### <a name="json-output"></a><span data-ttu-id="19842-177">JSON-utdata</span><span class="sxs-lookup"><span data-stu-id="19842-177">JSON output</span></span>
<span data-ttu-id="19842-178">JSON utdata för sammanställd känslo (trunkerad):</span><span class="sxs-lookup"><span data-stu-id="19842-178">JSON output for aggregate emotion (truncated):</span></span>

    {
     "version": 1,
     "timescale": 30000,
     "offset": 0,
     "framerate": 29.97,
     "width": 1280,
     "height": 720,
     "fragments": [
       {
         "start": 0,
         "duration": 60060,
         "interval": 15015,
         "events": [
           [
             {
               "windowFaceDistribution": {
                 "neutral": 0,
                 "happiness": 0,
                 "surprise": 0,
                 "sadness": 0,
                 "anger": 0,
                 "disgust": 0,
                 "fear": 0,
                 "contempt": 0
               },
               "windowMeanScores": {
                 "neutral": 0,
                 "happiness": 0,
                 "surprise": 0,
                 "sadness": 0,
                 "anger": 0,
                 "disgust": 0,
                 "fear": 0,
                 "contempt": 0
               }
             }
           ],
           [
             {
               "windowFaceDistribution": {
                 "neutral": 0,
                 "happiness": 0,
                 "surprise": 0,
                 "sadness": 0,
                 "anger": 0,
                 "disgust": 0,
                 "fear": 0,
                 "contempt": 0
               },
               "windowMeanScores": {
                 "neutral": 0,
                 "happiness": 0,
                 "surprise": 0,
                 "sadness": 0,
                 "anger": 0,
                 "disgust": 0,
                 "fear": 0,
                 "contempt": 0
               }
             }
           ],
           [
             {
               "windowFaceDistribution": {
                 "neutral": 0,
                 "happiness": 0,
                 "surprise": 0,
                 "sadness": 0,
                 "anger": 0,
                 "disgust": 0,
                 "fear": 0,
                 "contempt": 0
               },
               "windowMeanScores": {
                 "neutral": 0,
                 "happiness": 0,
                 "surprise": 0,
                 "sadness": 0,
                 "anger": 0,
                 "disgust": 0,
                 "fear": 0,
                 "contempt": 0
               }
             }
           ],
           [
             {
               "windowFaceDistribution": {
                 "neutral": 0,
                 "happiness": 0,
                 "surprise": 0,
                 "sadness": 0,
                 "anger": 0,
                 "disgust": 0,
                 "fear": 0,
                 "contempt": 0
               },
               "windowMeanScores": {
                 "neutral": 0,
                 "happiness": 0,
                 "surprise": 0,
                 "sadness": 0,
                 "anger": 0,
                 "disgust": 0,
                 "fear": 0,
                 "contempt": 0
               }
             }
           ]
         ]
       },
       {
         "start": 60060,
         "duration": 60060,
         "interval": 15015,
         "events": [
           [
             {
               "windowFaceDistribution": {
                 "neutral": 1,
                 "happiness": 0,
                 "surprise": 0,
                 "sadness": 0,
                 "anger": 0,
                 "disgust": 0,
                 "fear": 0,
                 "contempt": 0
               },
               "windowMeanScores": {
                 "neutral": 0.688541,
                 "happiness": 0.0586323,
                 "surprise": 0.227184,
                 "sadness": 0.00945675,
                 "anger": 0.00592107,
                 "disgust": 0.00154993,
                 "fear": 0.00450447,
                 "contempt": 0.0042109
               }
             }
           ],
           [
             {
               "windowFaceDistribution": {
                 "neutral": 1,
                 "happiness": 0,
                 "surprise": 0,
                 "sadness": 0,
                 "anger": 0,
                 "disgust": 0,
                 "fear": 0,

## <a name="limitations"></a><span data-ttu-id="19842-179">Begränsningar</span><span class="sxs-lookup"><span data-stu-id="19842-179">Limitations</span></span>
* <span data-ttu-id="19842-180">hello stöds video-indataformat inkluderar MP4, MOV och WMV.</span><span class="sxs-lookup"><span data-stu-id="19842-180">hello supported input video formats include MP4, MOV, and WMV.</span></span>
* <span data-ttu-id="19842-181">hello är kan upptäckas ansikte storlek 24 x 24 too2048x2048 bildpunkter.</span><span class="sxs-lookup"><span data-stu-id="19842-181">hello detectable face size range is 24x24 too2048x2048 pixels.</span></span> <span data-ttu-id="19842-182">kommer inte att identifiera hello ytor utanför det här intervallet.</span><span class="sxs-lookup"><span data-stu-id="19842-182">hello faces out of this range will not be detected.</span></span>
* <span data-ttu-id="19842-183">Varje video är hello maximalt antal ytor som returnerades 64.</span><span class="sxs-lookup"><span data-stu-id="19842-183">For each video, hello maximum number of faces returned is 64.</span></span>
* <span data-ttu-id="19842-184">Vissa ytor kanske inte identifieras på grund av tootechnical utmaningar; t.ex. väldigt ansikte vinklar (head-attityd) och stora är spärrat.</span><span class="sxs-lookup"><span data-stu-id="19842-184">Some faces may not be detected due tootechnical challenges; e.g. very large face angles (head-pose), and large occlusion.</span></span> <span data-ttu-id="19842-185">Främre och nästan direkt ytor har hello bästa resultat.</span><span class="sxs-lookup"><span data-stu-id="19842-185">Frontal and near-frontal faces have hello best results.</span></span>

## <a name="net-sample-code"></a><span data-ttu-id="19842-186">Exempelkod för .NET</span><span class="sxs-lookup"><span data-stu-id="19842-186">.NET sample code</span></span>

<span data-ttu-id="19842-187">hello följande program visar hur du:</span><span class="sxs-lookup"><span data-stu-id="19842-187">hello following program shows how to:</span></span>

1. <span data-ttu-id="19842-188">Skapa en tillgång och överför en mediefil till hello tillgång.</span><span class="sxs-lookup"><span data-stu-id="19842-188">Create an asset and upload a media file into hello asset.</span></span>
2. <span data-ttu-id="19842-189">Skapa ett jobb med en aktivitet för identifiering av står inför baserat på en konfigurationsfil som innehåller följande json förinställda hello.</span><span class="sxs-lookup"><span data-stu-id="19842-189">Create a job with a face detection task based on a configuration file that contains hello following json preset.</span></span> 
   
        {
            "version": "1.0"
        }
3. <span data-ttu-id="19842-190">Hämta hello utdata JSON-filer.</span><span class="sxs-lookup"><span data-stu-id="19842-190">Download hello output JSON files.</span></span> 

#### <a name="create-and-configure-a-visual-studio-project"></a><span data-ttu-id="19842-191">Skapa och konfigurera ett Visual Studio-projekt</span><span class="sxs-lookup"><span data-stu-id="19842-191">Create and configure a Visual Studio project</span></span>

<span data-ttu-id="19842-192">Konfigurera utvecklingsmiljön och fylla hello app.config-fil med anslutningsinformation, enligt beskrivningen i [Media Services-utveckling med .NET](media-services-dotnet-how-to-use.md).</span><span class="sxs-lookup"><span data-stu-id="19842-192">Set up your development environment and populate hello app.config file with connection information, as described in [Media Services development with .NET](media-services-dotnet-how-to-use.md).</span></span> 

#### <a name="example"></a><span data-ttu-id="19842-193">Exempel</span><span class="sxs-lookup"><span data-stu-id="19842-193">Example</span></span>

    using System;
    using System.Configuration;
    using System.IO;
    using System.Linq;
    using Microsoft.WindowsAzure.MediaServices.Client;
    using System.Threading;
    using System.Threading.Tasks;

    namespace FaceDetection
    {
        class Program
        {
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

                // Run hello FaceDetection job.
                var asset = RunFaceDetectionJob(@"C:\supportFiles\FaceDetection\BigBuckBunny.mp4",
                                            @"C:\supportFiles\FaceDetection\config.json");

                // Download hello job output asset.
                DownloadAsset(asset, @"C:\supportFiles\FaceDetection\Output");
            }

            static IAsset RunFaceDetectionJob(string inputMediaFilePath, string configurationFile)
            {
                // Create an asset and upload hello input media file toostorage.
                IAsset asset = CreateAssetAndUploadSingleFile(inputMediaFilePath,
                    "My Face Detection Input Asset",
                    AssetCreationOptions.None);

                // Declare a new job.
                IJob job = _context.Jobs.Create("My Face Detection Job");

                // Get a reference tooAzure Media Face Detector.
                string MediaProcessorName = "Azure Media Face Detector";

                var processor = GetLatestMediaProcessorByName(MediaProcessorName);

                // Read configuration from hello specified file.
                string configuration = File.ReadAllText(configurationFile);

                // Create a task with hello encoding details, using a string preset.
                ITask task = job.Tasks.AddNew("My Face Detection Task",
                    processor,
                    configuration,
                    TaskOptions.None);

                // Specify hello input asset.
                task.InputAssets.Add(asset);

                // Add an output asset toocontain hello results of hello job.
                task.OutputAssets.AddNew("My Face Detectoion Output Asset", AssetCreationOptions.None);

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

## <a name="media-services-learning-paths"></a><span data-ttu-id="19842-194">Sökvägar för Media Services-utbildning</span><span class="sxs-lookup"><span data-stu-id="19842-194">Media Services learning paths</span></span>
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a><span data-ttu-id="19842-195">Ge feedback</span><span class="sxs-lookup"><span data-stu-id="19842-195">Provide feedback</span></span>
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

## <a name="related-links"></a><span data-ttu-id="19842-196">Relaterade länkar</span><span class="sxs-lookup"><span data-stu-id="19842-196">Related links</span></span>
[<span data-ttu-id="19842-197">Azure Media Services Analytics-översikt</span><span class="sxs-lookup"><span data-stu-id="19842-197">Azure Media Services Analytics Overview</span></span>](media-services-analytics-overview.md)

[<span data-ttu-id="19842-198">Azure Media Analytics demonstrationer</span><span class="sxs-lookup"><span data-stu-id="19842-198">Azure Media Analytics demos</span></span>](http://amslabs.azurewebsites.net/demos/Analytics.html)

