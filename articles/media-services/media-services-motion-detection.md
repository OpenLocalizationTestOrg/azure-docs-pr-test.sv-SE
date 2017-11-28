---
title: "aaaDetect rörelser med Azure Media Analytics | Microsoft Docs"
description: "hello Azure Media rörelsedetektor media-processor (HP) aktiverar du tooefficiently identifierar avsnitt av intresse i ett annat långa och primärdomänkontrollant video."
services: media-services
documentationcenter: 
author: juliako
manager: cfowler
editor: 
ms.assetid: d144f813-1a55-442f-a895-5c4cb6d0aeae
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 07/31/2017
ms.author: milanga;juliako;
ms.openlocfilehash: cb431375c92222053ed2239dd4e45767524dab68
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="detect-motions-with-azure-media-analytics"></a><span data-ttu-id="f6405-103">Identifiera rörelser med Azure Media Analytics</span><span class="sxs-lookup"><span data-stu-id="f6405-103">Detect Motions with Azure Media Analytics</span></span>
## <a name="overview"></a><span data-ttu-id="f6405-104">Översikt</span><span class="sxs-lookup"><span data-stu-id="f6405-104">Overview</span></span>
<span data-ttu-id="f6405-105">Hej **Azure Media rörelsedetektor** media-processor (HP) aktiverar du tooefficiently identifierar avsnitt av intresse i ett annat långa och primärdomänkontrollant video.</span><span class="sxs-lookup"><span data-stu-id="f6405-105">hello **Azure Media Motion Detector** media processor (MP) enables you tooefficiently identify sections of interest within an otherwise long and uneventful video.</span></span> <span data-ttu-id="f6405-106">Rörelseidentifiering kan användas på statisk kamera tagning tooidentify avsnitt av hello video där rörelse inträffar.</span><span class="sxs-lookup"><span data-stu-id="f6405-106">Motion detection can be used on static camera footage tooidentify sections of hello video where motion occurs.</span></span> <span data-ttu-id="f6405-107">Den genererar en JSON-fil som innehåller en metadata med tidsstämplar och hello avgränsar region där hello händelsen inträffade.</span><span class="sxs-lookup"><span data-stu-id="f6405-107">It generates a JSON file containing a metadata with timestamps and hello bounding region where hello event occurred.</span></span>

<span data-ttu-id="f6405-108">Den här tekniken är riktade mot säkerhet video feeds kan toocategorize rörelse in relevanta händelser och falska positiva identifieringar som skuggor och andra ändringar.</span><span class="sxs-lookup"><span data-stu-id="f6405-108">Targeted towards security video feeds, this technology is able toocategorize motion into relevant events and false positives such as shadows and lighting changes.</span></span> <span data-ttu-id="f6405-109">Detta ger dig toogenerate säkerhetsaviseringar från kameran feeds utan som skräppost med oändlig irrelevanta händelser inte kan tooextract stund intressanta från extremt långa övervakning videor.</span><span class="sxs-lookup"><span data-stu-id="f6405-109">This allows you toogenerate security alerts from camera feeds without being spammed with endless irrelevant events, while being able tooextract moments of interest from extremely long surveillance videos.</span></span>

<span data-ttu-id="f6405-110">Hej **Azure Media rörelsedetektor** MP är för närvarande under förhandsgranskning.</span><span class="sxs-lookup"><span data-stu-id="f6405-110">hello **Azure Media Motion Detector** MP is currently in Preview.</span></span>

<span data-ttu-id="f6405-111">Det här avsnittet innehåller information om **Azure Media rörelsedetektor** och visar hur toouse med Media Services SDK för .NET</span><span class="sxs-lookup"><span data-stu-id="f6405-111">This topic gives details about  **Azure Media Motion Detector** and shows how toouse it with Media Services SDK for .NET</span></span>

## <a name="motion-detector-input-files"></a><span data-ttu-id="f6405-112">Rörelse detektor inkommande filer</span><span class="sxs-lookup"><span data-stu-id="f6405-112">Motion Detector input files</span></span>
<span data-ttu-id="f6405-113">Videofiler.</span><span class="sxs-lookup"><span data-stu-id="f6405-113">Video files.</span></span> <span data-ttu-id="f6405-114">För närvarande hello följande format stöds: MP4, MOV och WMV.</span><span class="sxs-lookup"><span data-stu-id="f6405-114">Currently, hello following formats are supported: MP4, MOV, and WMV.</span></span>

## <a name="task-configuration-preset"></a><span data-ttu-id="f6405-115">Uppgiftskonfigurationen (förinställda)</span><span class="sxs-lookup"><span data-stu-id="f6405-115">Task configuration (preset)</span></span>
<span data-ttu-id="f6405-116">När du skapar en uppgift med **Azure Media rörelsedetektor**, måste du ange en konfiguration förinställning.</span><span class="sxs-lookup"><span data-stu-id="f6405-116">When creating a task with **Azure Media Motion Detector**, you must specify a configuration preset.</span></span> 

### <a name="parameters"></a><span data-ttu-id="f6405-117">Parametrar</span><span class="sxs-lookup"><span data-stu-id="f6405-117">Parameters</span></span>
<span data-ttu-id="f6405-118">Du kan använda hello följande parametrar:</span><span class="sxs-lookup"><span data-stu-id="f6405-118">You can use hello following parameters:</span></span>

| <span data-ttu-id="f6405-119">Namn</span><span class="sxs-lookup"><span data-stu-id="f6405-119">Name</span></span> | <span data-ttu-id="f6405-120">Alternativ</span><span class="sxs-lookup"><span data-stu-id="f6405-120">Options</span></span> | <span data-ttu-id="f6405-121">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="f6405-121">Description</span></span> | <span data-ttu-id="f6405-122">Standard</span><span class="sxs-lookup"><span data-stu-id="f6405-122">Default</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="f6405-123">sensitivityLevel</span><span class="sxs-lookup"><span data-stu-id="f6405-123">sensitivityLevel</span></span> |<span data-ttu-id="f6405-124">Sträng: låg, 'medium', 'hög'</span><span class="sxs-lookup"><span data-stu-id="f6405-124">String:'low', 'medium', 'high'</span></span> |<span data-ttu-id="f6405-125">Anger hello känslighet på vilka rörelser rapporteras.</span><span class="sxs-lookup"><span data-stu-id="f6405-125">Sets hello sensitivity level at which motions is reported.</span></span> <span data-ttu-id="f6405-126">Justera det här tooadjust antalet falska positiva identifieringar.</span><span class="sxs-lookup"><span data-stu-id="f6405-126">Adjust this tooadjust amount of false positives.</span></span> |<span data-ttu-id="f6405-127">-medium'</span><span class="sxs-lookup"><span data-stu-id="f6405-127">'medium'</span></span> |
| <span data-ttu-id="f6405-128">frameSamplingValue</span><span class="sxs-lookup"><span data-stu-id="f6405-128">frameSamplingValue</span></span> |<span data-ttu-id="f6405-129">Positivt heltal</span><span class="sxs-lookup"><span data-stu-id="f6405-129">Positive integer</span></span> |<span data-ttu-id="f6405-130">Anger hello frekvens vid vilken algoritm körs.</span><span class="sxs-lookup"><span data-stu-id="f6405-130">Sets hello frequency at which algorithm runs.</span></span> <span data-ttu-id="f6405-131">1 är lika med varje ram, 2: var 2 ram och så vidare.</span><span class="sxs-lookup"><span data-stu-id="f6405-131">1 equals every frame, 2 means every 2nd frame, and so on.</span></span> |<span data-ttu-id="f6405-132">1</span><span class="sxs-lookup"><span data-stu-id="f6405-132">1</span></span> |
| <span data-ttu-id="f6405-133">detectLightChange</span><span class="sxs-lookup"><span data-stu-id="f6405-133">detectLightChange</span></span> |<span data-ttu-id="f6405-134">Booleskt: 'true', 'false'</span><span class="sxs-lookup"><span data-stu-id="f6405-134">Boolean:'true', 'false'</span></span> |<span data-ttu-id="f6405-135">Anger om lätta ändringar rapporteras i hello resultat</span><span class="sxs-lookup"><span data-stu-id="f6405-135">Sets whether light changes are reported in hello results</span></span> |<span data-ttu-id="f6405-136">'False'</span><span class="sxs-lookup"><span data-stu-id="f6405-136">'False'</span></span> |
| <span data-ttu-id="f6405-137">mergeTimeThreshold</span><span class="sxs-lookup"><span data-stu-id="f6405-137">mergeTimeThreshold</span></span> |<span data-ttu-id="f6405-138">Tid för xs:: Mm: ss</span><span class="sxs-lookup"><span data-stu-id="f6405-138">Xs-time: Hh:mm:ss</span></span><br/><span data-ttu-id="f6405-139">Exempel: 00:00:03</span><span class="sxs-lookup"><span data-stu-id="f6405-139">Example: 00:00:03</span></span> |<span data-ttu-id="f6405-140">Anger hello tidsfönstret mellan rörelse händelser där 2 händelser kombineras och rapporteras som 1.</span><span class="sxs-lookup"><span data-stu-id="f6405-140">Specifies hello time window between motion events where 2 events will be combined and reported as 1.</span></span> |<span data-ttu-id="f6405-141">00:00:00</span><span class="sxs-lookup"><span data-stu-id="f6405-141">00:00:00</span></span> |
| <span data-ttu-id="f6405-142">detectionZones</span><span class="sxs-lookup"><span data-stu-id="f6405-142">detectionZones</span></span> |<span data-ttu-id="f6405-143">En matris med identifiering zoner:</span><span class="sxs-lookup"><span data-stu-id="f6405-143">An array of detection zones:</span></span><br/><span data-ttu-id="f6405-144">-Identifiering zonen är en matris med 3 eller fler punkter</span><span class="sxs-lookup"><span data-stu-id="f6405-144">- Detection Zone is an array of 3 or more points</span></span><br/><span data-ttu-id="f6405-145">-Platsen är en x och y koordinaten från 0 too1.</span><span class="sxs-lookup"><span data-stu-id="f6405-145">- Point is a x and y coordinate from 0 too1.</span></span> |<span data-ttu-id="f6405-146">Beskriver hello lista över identifiering av polygona zoner toobe används.</span><span class="sxs-lookup"><span data-stu-id="f6405-146">Describes hello list of polygonal detection zones toobe used.</span></span><br/><span data-ttu-id="f6405-147">Resultatet rapporteras med hello zoner som ett-ID och hello först en vara ”id”: 0</span><span class="sxs-lookup"><span data-stu-id="f6405-147">Results will be reported with hello zones as an ID, with hello first one being 'id':0</span></span> |<span data-ttu-id="f6405-148">Zon som omfattar hela hello-ramen.</span><span class="sxs-lookup"><span data-stu-id="f6405-148">Single zone which covers hello entire frame.</span></span> |

### <a name="json-example"></a><span data-ttu-id="f6405-149">JSON-exempel</span><span class="sxs-lookup"><span data-stu-id="f6405-149">JSON example</span></span>
    {
      "version": "1.0",
      "options": {
        "sensitivityLevel": "medium",
        "frameSamplingValue": 1,
        "detectLightChange": "False",
        "mergeTimeThreshold":
        "00:00:02",
        "detectionZones": [
          [
            {"x": 0, "y": 0},
            {"x": 0.5, "y": 0},
            {"x": 0, "y": 1}
           ],
          [
            {"x": 0.3, "y": 0.3},
            {"x": 0.55, "y": 0.3},
            {"x": 0.8, "y": 0.3},
            {"x": 0.8, "y": 0.55},
            {"x": 0.8, "y": 0.8},
            {"x": 0.55, "y": 0.8},
            {"x": 0.3, "y": 0.8},
            {"x": 0.3, "y": 0.55}
          ]
        ]
      }
    }


## <a name="motion-detector-output-files"></a><span data-ttu-id="f6405-150">Videofiler detektor utdata</span><span class="sxs-lookup"><span data-stu-id="f6405-150">Motion Detector output files</span></span>
<span data-ttu-id="f6405-151">Ett jobb för identifiering av rörelse returneras en JSON-fil i hello utdatatillgången som beskriver hello rörelse aviseringar och deras kategorier inom hello video.</span><span class="sxs-lookup"><span data-stu-id="f6405-151">A motion detection job will return a JSON file in hello output asset which describes hello motion alerts, and their categories, within hello video.</span></span> <span data-ttu-id="f6405-152">hello-filen innehåller information om hello och varaktighet för rörelse upptäcktes i hello video.</span><span class="sxs-lookup"><span data-stu-id="f6405-152">hello file will contain information about hello time and duration of motion detected in hello video.</span></span>

<span data-ttu-id="f6405-153">hello rörelse detektor API innehåller indikatorer när det finns objekt i rörelse i en fast bakgrund video (t.ex. övervakning av video).</span><span class="sxs-lookup"><span data-stu-id="f6405-153">hello Motion Detector API provides indicators once there are objects in motion in a fixed background video (e.g. a surveillance video).</span></span> <span data-ttu-id="f6405-154">hello rörelsedetektor är utbildade tooreduce falsklarm, till exempel ljus och shadow ändringar.</span><span class="sxs-lookup"><span data-stu-id="f6405-154">hello Motion Detector is trained tooreduce false alarms, such as lighting and shadow changes.</span></span> <span data-ttu-id="f6405-155">Aktuella begränsningar för hello algoritmer är natt vision videor, delvis transparenta objekt och små objekt.</span><span class="sxs-lookup"><span data-stu-id="f6405-155">Current limitations of hello algorithms include night vision videos, semi-transparent objects, and small objects.</span></span>

### <span data-ttu-id="f6405-156"><a id="output_elements"></a>Element i hello utdata-JSON-fil</span><span class="sxs-lookup"><span data-stu-id="f6405-156"><a id="output_elements"></a>Elements of hello output JSON file</span></span>
> [!NOTE]
> <span data-ttu-id="f6405-157">I hello senaste versionen, hello utdata-JSON-format har ändrats och kan utgöra en ny ändring för vissa kunder.</span><span class="sxs-lookup"><span data-stu-id="f6405-157">In hello latest release, hello Output JSON format has changed and may represent a breaking change for some customers.</span></span>
> 
> 

<span data-ttu-id="f6405-158">hello beskrivs följande tabell element i hello utdata-JSON-fil.</span><span class="sxs-lookup"><span data-stu-id="f6405-158">hello following table describes elements of hello output JSON file.</span></span>

| <span data-ttu-id="f6405-159">Element</span><span class="sxs-lookup"><span data-stu-id="f6405-159">Element</span></span> | <span data-ttu-id="f6405-160">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="f6405-160">Description</span></span> |
| --- | --- |
| <span data-ttu-id="f6405-161">Version</span><span class="sxs-lookup"><span data-stu-id="f6405-161">Version</span></span> |<span data-ttu-id="f6405-162">Detta refererar toohello hello Video API-version.</span><span class="sxs-lookup"><span data-stu-id="f6405-162">This refers toohello version of hello Video API.</span></span> <span data-ttu-id="f6405-163">hello aktuella versionen är 2.</span><span class="sxs-lookup"><span data-stu-id="f6405-163">hello current version is 2.</span></span> |
| <span data-ttu-id="f6405-164">tidsrymd</span><span class="sxs-lookup"><span data-stu-id="f6405-164">Timescale</span></span> |<span data-ttu-id="f6405-165">”Tick” per sekund av hello video.</span><span class="sxs-lookup"><span data-stu-id="f6405-165">"Ticks" per second of hello video.</span></span> |
| <span data-ttu-id="f6405-166">Offset</span><span class="sxs-lookup"><span data-stu-id="f6405-166">Offset</span></span> |<span data-ttu-id="f6405-167">hello tidsförskjutningen för tidsstämplar i ”tick”.</span><span class="sxs-lookup"><span data-stu-id="f6405-167">hello time offset for timestamps in "ticks".</span></span> <span data-ttu-id="f6405-168">I version 1.0 av API: er för Video att detta alltid vara 0.</span><span class="sxs-lookup"><span data-stu-id="f6405-168">In version 1.0 of Video APIs, this will always be 0.</span></span> <span data-ttu-id="f6405-169">I framtiden scenarier som vi stöder det här värdet kan ändras.</span><span class="sxs-lookup"><span data-stu-id="f6405-169">In future scenarios we support, this value may change.</span></span> |
| <span data-ttu-id="f6405-170">ramhastighet</span><span class="sxs-lookup"><span data-stu-id="f6405-170">Framerate</span></span> |<span data-ttu-id="f6405-171">Bildrutor per sekund av hello video.</span><span class="sxs-lookup"><span data-stu-id="f6405-171">Frames per second of hello video.</span></span> |
| <span data-ttu-id="f6405-172">Bredd, höjd</span><span class="sxs-lookup"><span data-stu-id="f6405-172">Width, Height</span></span> |<span data-ttu-id="f6405-173">Refererar toohello bredd och höjd hello video i bildpunkter.</span><span class="sxs-lookup"><span data-stu-id="f6405-173">Refers toohello width and height of hello video in pixels.</span></span> |
| <span data-ttu-id="f6405-174">Start</span><span class="sxs-lookup"><span data-stu-id="f6405-174">Start</span></span> |<span data-ttu-id="f6405-175">hello starta tidsstämpeln i ”tick”.</span><span class="sxs-lookup"><span data-stu-id="f6405-175">hello start timestamp in "ticks".</span></span> |
| <span data-ttu-id="f6405-176">Varaktighet</span><span class="sxs-lookup"><span data-stu-id="f6405-176">Duration</span></span> |<span data-ttu-id="f6405-177">hello längd hello händelse i ”tick”.</span><span class="sxs-lookup"><span data-stu-id="f6405-177">hello length of hello event, in "ticks".</span></span> |
| <span data-ttu-id="f6405-178">intervall</span><span class="sxs-lookup"><span data-stu-id="f6405-178">Interval</span></span> |<span data-ttu-id="f6405-179">hello-intervall för varje post i hello händelse i ”tick”.</span><span class="sxs-lookup"><span data-stu-id="f6405-179">hello interval of each entry in hello event, in "ticks".</span></span> |
| <span data-ttu-id="f6405-180">Händelser</span><span class="sxs-lookup"><span data-stu-id="f6405-180">Events</span></span> |<span data-ttu-id="f6405-181">Varje händelse fragment innehåller hello rörelse identifieras inom den varaktigheten.</span><span class="sxs-lookup"><span data-stu-id="f6405-181">Each event fragment contains hello motion detected within that time duration.</span></span> |
| <span data-ttu-id="f6405-182">Typ</span><span class="sxs-lookup"><span data-stu-id="f6405-182">Type</span></span> |<span data-ttu-id="f6405-183">I hello aktuell version är det alltid '2' för allmän rörelse.</span><span class="sxs-lookup"><span data-stu-id="f6405-183">In hello current version, this is always ‘2’ for generic motion.</span></span> <span data-ttu-id="f6405-184">Den här etiketten ger API: er för Video hello flexibilitet toocategorize rörelse i framtida versioner.</span><span class="sxs-lookup"><span data-stu-id="f6405-184">This label gives Video APIs hello flexibility toocategorize motion in future versions.</span></span> |
| <span data-ttu-id="f6405-185">RegionID</span><span class="sxs-lookup"><span data-stu-id="f6405-185">RegionID</span></span> |<span data-ttu-id="f6405-186">Detta kommer alltid att 0 i den här versionen som beskrivs ovan.</span><span class="sxs-lookup"><span data-stu-id="f6405-186">As explained above, this will always be 0 in this version.</span></span> <span data-ttu-id="f6405-187">Den här etiketten ger Video API hello flexibilitet toofind rörelse i olika områden i framtida versioner.</span><span class="sxs-lookup"><span data-stu-id="f6405-187">This label gives Video API hello flexibility toofind motion in various regions in future versions.</span></span> |
| <span data-ttu-id="f6405-188">Regioner</span><span class="sxs-lookup"><span data-stu-id="f6405-188">Regions</span></span> |<span data-ttu-id="f6405-189">Refererar toohello område i videon där du är intresserad rörelse.</span><span class="sxs-lookup"><span data-stu-id="f6405-189">Refers toohello area in your video where you care about motion.</span></span> <br/><br/><span data-ttu-id="f6405-190">-”id” representerar hello region område – i den här versionen finns bara en, ID 0.</span><span class="sxs-lookup"><span data-stu-id="f6405-190">-"id" represents hello region area – in this version there is only one, ID 0.</span></span> <br/><span data-ttu-id="f6405-191">-”typ” representerar hello form av hello region som intresserar dig för rörelse.</span><span class="sxs-lookup"><span data-stu-id="f6405-191">-"type" represents hello shape of hello region you care about for motion.</span></span> <span data-ttu-id="f6405-192">För närvarande stöds ”rektangel” och ”polygon”.</span><span class="sxs-lookup"><span data-stu-id="f6405-192">Currently, "rectangle" and "polygon" are supported.</span></span><br/> <span data-ttu-id="f6405-193">Om du har angett ”rektangel” hello region har mått i X, Y, bredd och höjd.</span><span class="sxs-lookup"><span data-stu-id="f6405-193">If you specified "rectangle", hello region has dimensions in X, Y, Width, and Height.</span></span> <span data-ttu-id="f6405-194">hello X och Y-koordinaterna representerar hello övre vänstra Xyserver koordinaterna för hello region i 0.0 too1.0 normaliserade skalan.</span><span class="sxs-lookup"><span data-stu-id="f6405-194">hello X and Y coordinates represent hello upper left hand XY coordinates of hello region in a normalized scale of 0.0 too1.0.</span></span> <span data-ttu-id="f6405-195">representerar hello storleken på hello region i 0.0 too1.0 normaliserade skalan hello bredd och höjd.</span><span class="sxs-lookup"><span data-stu-id="f6405-195">hello width and height represent hello size of hello region in a normalized scale of 0.0 too1.0.</span></span> <span data-ttu-id="f6405-196">I hello aktuella versionen kan korrigeras alltid X, Y, bredd och höjd på 0, 0 och 1, 1.</span><span class="sxs-lookup"><span data-stu-id="f6405-196">In hello current version, X, Y, Width, and Height are always fixed at 0, 0 and 1, 1.</span></span> <br/><span data-ttu-id="f6405-197">Om du har angett ”polygon” har hello region dimensioner i punkter.</span><span class="sxs-lookup"><span data-stu-id="f6405-197">If you specified "polygon", hello region has dimensions in points.</span></span> <br/> |
| <span data-ttu-id="f6405-198">fragment</span><span class="sxs-lookup"><span data-stu-id="f6405-198">Fragments</span></span> |<span data-ttu-id="f6405-199">hello metadata chunked upp till olika segment som kallas fragment.</span><span class="sxs-lookup"><span data-stu-id="f6405-199">hello metadata is chunked up into different segments called fragments.</span></span> <span data-ttu-id="f6405-200">Varje avsnitt innehåller en start, varaktighet, antalet och händelse(r).</span><span class="sxs-lookup"><span data-stu-id="f6405-200">Each fragment contains a start, duration, interval number, and event(s).</span></span> <span data-ttu-id="f6405-201">Ett fragment med inga händelser innebär att inga rörelse identifierades under den starttid och varaktighet.</span><span class="sxs-lookup"><span data-stu-id="f6405-201">A fragment with no events means that no motion was detected during that start time and duration.</span></span> |
| <span data-ttu-id="f6405-202">Hakparenteserna]</span><span class="sxs-lookup"><span data-stu-id="f6405-202">Brackets []</span></span> |<span data-ttu-id="f6405-203">Varje hakparentes representerar ett intervall i hello-händelse.</span><span class="sxs-lookup"><span data-stu-id="f6405-203">Each bracket represents one interval in hello event.</span></span> <span data-ttu-id="f6405-204">Tomma parenteser för intervallet innebär att inga rörelse upptäcktes.</span><span class="sxs-lookup"><span data-stu-id="f6405-204">Empty brackets for that interval means that no motion was detected.</span></span> |
| <span data-ttu-id="f6405-205">Platser</span><span class="sxs-lookup"><span data-stu-id="f6405-205">locations</span></span> |<span data-ttu-id="f6405-206">Den nya posten under händelser visar hello plats där hello rörelse inträffade.</span><span class="sxs-lookup"><span data-stu-id="f6405-206">This new entry under events lists hello location where hello motion occurred.</span></span> <span data-ttu-id="f6405-207">Det här är mer specifik än hello identifiering zoner.</span><span class="sxs-lookup"><span data-stu-id="f6405-207">This is more specific than hello detection zones.</span></span> |

<span data-ttu-id="f6405-208">hello följande är exempel på en JSON-utdata</span><span class="sxs-lookup"><span data-stu-id="f6405-208">hello following is a JSON output example</span></span>

    {
      "version": 2,
      "timescale": 23976,
      "offset": 0,
      "framerate": 24,
      "width": 1280,
      "height": 720,
      "regions": [
        {
          "id": 0,
          "type": "polygon",
          "points": [{'x': 0, 'y': 0},
            {'x': 0.5, 'y': 0},
            {'x': 0, 'y': 1}]
        }
      ],
      "fragments": [
        {
          "start": 0,
          "duration": 226765
        },
        {
          "start": 226765,
          "duration": 47952,
          "interval": 999,
          "events": [
            [
              {
                "type": 2,
                "typeName": "motion",
                "locations": [
                  {
                    "x": 0.004184,
                    "y": 0.007463,
                    "width": 0.991667,
                    "height": 0.985185
                  }
                ],
                "regionId": 0
              }
            ],

    …
## <a name="limitations"></a><span data-ttu-id="f6405-209">Begränsningar</span><span class="sxs-lookup"><span data-stu-id="f6405-209">Limitations</span></span>
* <span data-ttu-id="f6405-210">hello stöds video-indataformat inkluderar MP4, MOV och WMV.</span><span class="sxs-lookup"><span data-stu-id="f6405-210">hello supported input video formats include MP4, MOV, and WMV.</span></span>
* <span data-ttu-id="f6405-211">Rörelseidentifiering är optimerad för stilla bakgrund videor.</span><span class="sxs-lookup"><span data-stu-id="f6405-211">Motion Detection is optimized for stationary background videos.</span></span> <span data-ttu-id="f6405-212">hello algoritmen fokuserar på att minska falsklarm, till exempel ljusförändringar och skuggor.</span><span class="sxs-lookup"><span data-stu-id="f6405-212">hello algorithm focuses on reducing false alarms, such as lighting changes, and shadows.</span></span>
* <span data-ttu-id="f6405-213">Vissa rörelse kanske inte identifieras på grund av tootechnical utmaningar; t.ex. natt vision videor, delvis transparenta objekt och små objekt.</span><span class="sxs-lookup"><span data-stu-id="f6405-213">Some motion may not be detected due tootechnical challenges; e.g. night vision videos, semi-transparent objects, and small objects.</span></span>

## <a name="net-sample-code"></a><span data-ttu-id="f6405-214">Exempelkod för .NET</span><span class="sxs-lookup"><span data-stu-id="f6405-214">.NET sample code</span></span>

<span data-ttu-id="f6405-215">hello följande program visar hur du:</span><span class="sxs-lookup"><span data-stu-id="f6405-215">hello following program shows how to:</span></span>

1. <span data-ttu-id="f6405-216">Skapa en tillgång och överför en mediefil till hello tillgång.</span><span class="sxs-lookup"><span data-stu-id="f6405-216">Create an asset and upload a media file into hello asset.</span></span>
2. <span data-ttu-id="f6405-217">Skapa ett jobb med en aktivitet för identifiering av video rörelse baserat på en konfigurationsfil som innehåller hello följande json förinställda.</span><span class="sxs-lookup"><span data-stu-id="f6405-217">Create a job with a video motion detection task based on a configuration file that contains hello following json preset.</span></span> 
   
        {
          "Version": "1.0",
          "Options": {
            "SensitivityLevel": "medium",
            "FrameSamplingValue": 1,
            "DetectLightChange": "False",
            "MergeTimeThreshold":
            "00:00:02",
            "DetectionZones": [
              [
                {"x": 0, "y": 0},
                {"x": 0.5, "y": 0},
                {"x": 0, "y": 1}
               ],
              [
                {"x": 0.3, "y": 0.3},
                {"x": 0.55, "y": 0.3},
                {"x": 0.8, "y": 0.3},
                {"x": 0.8, "y": 0.55},
                {"x": 0.8, "y": 0.8},
                {"x": 0.55, "y": 0.8},
                {"x": 0.3, "y": 0.8},
                {"x": 0.3, "y": 0.55}
              ]
            ]
          }
        }
3. <span data-ttu-id="f6405-218">Hämta hello utdata JSON-filer.</span><span class="sxs-lookup"><span data-stu-id="f6405-218">Download hello output JSON files.</span></span> 

#### <a name="create-and-configure-a-visual-studio-project"></a><span data-ttu-id="f6405-219">Skapa och konfigurera ett Visual Studio-projekt</span><span class="sxs-lookup"><span data-stu-id="f6405-219">Create and configure a Visual Studio project</span></span>

<span data-ttu-id="f6405-220">Konfigurera utvecklingsmiljön och fylla hello app.config-fil med anslutningsinformation, enligt beskrivningen i [Media Services-utveckling med .NET](media-services-dotnet-how-to-use.md).</span><span class="sxs-lookup"><span data-stu-id="f6405-220">Set up your development environment and populate hello app.config file with connection information, as described in [Media Services development with .NET](media-services-dotnet-how-to-use.md).</span></span> 

#### <a name="example"></a><span data-ttu-id="f6405-221">Exempel</span><span class="sxs-lookup"><span data-stu-id="f6405-221">Example</span></span>


    using System;
    using System.Configuration;
    using System.IO;
    using System.Linq;
    using Microsoft.WindowsAzure.MediaServices.Client;
    using System.Threading;
    using System.Threading.Tasks;

    namespace VideoMotionDetection
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

                // Run hello VideoMotionDetection job.
                var asset = RunVideoMotionDetectionJob(@"C:\supportFiles\VideoMotionDetection\BigBuckBunny.mp4",
                                            @"C:\supportFiles\VideoMotionDetection\config.json");

                // Download hello job output asset.
                DownloadAsset(asset, @"C:\supportFiles\VideoMotionDetection\Output");
            }

            static IAsset RunVideoMotionDetectionJob(string inputMediaFilePath, string configurationFile)
            {
                // Create an asset and upload hello input media file toostorage.
                IAsset asset = CreateAssetAndUploadSingleFile(inputMediaFilePath,
                    "My Video Motion Detection Input Asset",
                    AssetCreationOptions.None);

                // Declare a new job.
                IJob job = _context.Jobs.Create("My Video Motion Detection Job");

                // Get a reference tooAzure Media Motion Detector.
                string MediaProcessorName = "Azure Media Motion Detector";

                var processor = GetLatestMediaProcessorByName(MediaProcessorName);

                // Read configuration from hello specified file.
                string configuration = File.ReadAllText(configurationFile);

                // Create a task with hello encoding details, using a string preset.
                ITask task = job.Tasks.AddNew("My Video Motion Detection Task",
                    processor,
                    configuration,
                    TaskOptions.None);

                // Specify hello input asset.
                task.InputAssets.Add(asset);

                // Add an output asset toocontain hello results of hello job.
                task.OutputAssets.AddNew("My Video Motion Detectoion Output Asset", AssetCreationOptions.None);

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

## <a name="media-services-learning-paths"></a><span data-ttu-id="f6405-222">Sökvägar för Media Services-utbildning</span><span class="sxs-lookup"><span data-stu-id="f6405-222">Media Services learning paths</span></span>
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a><span data-ttu-id="f6405-223">Ge feedback</span><span class="sxs-lookup"><span data-stu-id="f6405-223">Provide feedback</span></span>
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

## <a name="related-links"></a><span data-ttu-id="f6405-224">Relaterade länkar</span><span class="sxs-lookup"><span data-stu-id="f6405-224">Related links</span></span>
[<span data-ttu-id="f6405-225">Azure Media Services rörelsedetektor blogg</span><span class="sxs-lookup"><span data-stu-id="f6405-225">Azure Media Services Motion Detector blog</span></span>](https://azure.microsoft.com/blog/motion-detector-update/)

[<span data-ttu-id="f6405-226">Azure Media Services Analytics-översikt</span><span class="sxs-lookup"><span data-stu-id="f6405-226">Azure Media Services Analytics Overview</span></span>](media-services-analytics-overview.md)

[<span data-ttu-id="f6405-227">Azure Media Analytics demonstrationer</span><span class="sxs-lookup"><span data-stu-id="f6405-227">Azure Media Analytics demos</span></span>](http://azuremedialabs.azurewebsites.net/demos/Analytics.html)

