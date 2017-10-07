---
title: aaaRedact personerna bakom Azure Media Analytics | Microsoft Docs
description: "Det här avsnittet visar hur tooredact står med Azure media analytics."
services: media-services
documentationcenter: 
author: juliako
manager: cfowler
editor: 
ms.assetid: 5b6d8b8c-5f4d-4fef-b3d6-dc22c6b5a0f5
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 07/31/2017
ms.author: juliako;
ms.openlocfilehash: 1f5688a8c6374151c526a9c702b904d8c3e46164
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="redact-faces-with-azure-media-analytics"></a><span data-ttu-id="e9a86-103">Redigera bort personerna bakom Azure Media Analytics</span><span class="sxs-lookup"><span data-stu-id="e9a86-103">Redact faces with Azure Media Analytics</span></span>
## <a name="overview"></a><span data-ttu-id="e9a86-104">Översikt</span><span class="sxs-lookup"><span data-stu-id="e9a86-104">Overview</span></span>
<span data-ttu-id="e9a86-105">**Azure Media Redactor** är en [Azure Media Analytics](media-services-analytics-overview.md) medieprocessor (HP) som erbjuder skalbara ansikte bortredigering i hello molnet.</span><span class="sxs-lookup"><span data-stu-id="e9a86-105">**Azure Media Redactor** is an [Azure Media Analytics](media-services-analytics-overview.md) media processor (MP) that offers scalable face redaction in hello cloud.</span></span> <span data-ttu-id="e9a86-106">Framsidan bortredigering aktiverar du toomodify videon i ordning tooblur ytor valda enskilda användare.</span><span class="sxs-lookup"><span data-stu-id="e9a86-106">Face redaction enables you toomodify your video in order tooblur faces of selected individuals.</span></span> <span data-ttu-id="e9a86-107">Du kanske vill toouse hello ansikte bortredigering service i offentliga säkerhet och nyheter media scenarier.</span><span class="sxs-lookup"><span data-stu-id="e9a86-107">You may want toouse hello face redaction service in public safety and news media scenarios.</span></span> <span data-ttu-id="e9a86-108">Några minuter med material som innehåller flera ytor kan ta timmar tooredact manuellt, men med den här tjänsten hello ansikte bortredigering processen tar bara några få enkla steg.</span><span class="sxs-lookup"><span data-stu-id="e9a86-108">A few minutes of footage that contains multiple faces can take hours tooredact manually, but with this service hello face redaction process will require just a few simple steps.</span></span> <span data-ttu-id="e9a86-109">Mer information finns i [detta](https://azure.microsoft.com/blog/azure-media-redactor/) blogg.</span><span class="sxs-lookup"><span data-stu-id="e9a86-109">For  more information, see [this](https://azure.microsoft.com/blog/azure-media-redactor/) blog.</span></span>

<span data-ttu-id="e9a86-110">Det här avsnittet innehåller information om **Azure Media Redactor** och visar hur toouse med Media Services SDK för .NET.</span><span class="sxs-lookup"><span data-stu-id="e9a86-110">This topic gives details about **Azure Media Redactor** and shows how toouse it with Media Services SDK for .NET.</span></span>

<span data-ttu-id="e9a86-111">Hej **Azure Media Redactor** MP är för närvarande under förhandsgranskning.</span><span class="sxs-lookup"><span data-stu-id="e9a86-111">hello **Azure Media Redactor** MP is currently in Preview.</span></span> <span data-ttu-id="e9a86-112">Den är tillgänglig i alla offentliga Azure-regioner som tillhör amerikanska myndigheter och Kina datacenter.</span><span class="sxs-lookup"><span data-stu-id="e9a86-112">It is available in all public Azure regions as well as US Government and China data centers.</span></span> <span data-ttu-id="e9a86-113">Den här förhandsgranskningen är för närvarande kostnadsfritt.</span><span class="sxs-lookup"><span data-stu-id="e9a86-113">This preview is currently free of charge.</span></span> 

## <a name="face-redaction-modes"></a><span data-ttu-id="e9a86-114">Framsidan bortredigering lägen</span><span class="sxs-lookup"><span data-stu-id="e9a86-114">Face redaction modes</span></span>
<span data-ttu-id="e9a86-115">Ansikte bortredigering fungerar genom att identifiera ytor i varje ram av video och spåra hello ansikte objektet både framåt och bakåt i tiden, så att hello samma person kan oskarpa från andra vinklar samt.</span><span class="sxs-lookup"><span data-stu-id="e9a86-115">Facial redaction works by detecting faces in every frame of video and tracking hello face object both forwards and backwards in time, so that hello same individual can be blurred from other angles as well.</span></span> <span data-ttu-id="e9a86-116">hello automatiserade bortredigering processen är mycket komplex och alltid skapar inte 100% av önskade utgående därför Media Analytics ger dig med ett par olika sätt toomodify hello slutgiltiga utdatan.</span><span class="sxs-lookup"><span data-stu-id="e9a86-116">hello automated redaction process is very complex and does not always produce 100% of desired output, for this reason Media Analytics provides you with a couple of ways toomodify hello final output.</span></span>

<span data-ttu-id="e9a86-117">I tillägg tooa fullständigt automatiskt läge finns ett arbetsflöde för två gånger där hello val/de-selection av hittade ytor via en lista över ID: N.</span><span class="sxs-lookup"><span data-stu-id="e9a86-117">In addition tooa fully automatic mode, there is a two-pass workflow which allows hello selection/de-selection of found faces via a list of IDs.</span></span> <span data-ttu-id="e9a86-118">Dessutom använder toomake godtycklig per ram justeringar hello MP en metadatafil i JSON-format.</span><span class="sxs-lookup"><span data-stu-id="e9a86-118">Also, toomake arbitrary per frame adjustments hello MP uses a metadata file in JSON format.</span></span> <span data-ttu-id="e9a86-119">Det här arbetsflödet är uppdelat i **analysera** och **Redact** lägen.</span><span class="sxs-lookup"><span data-stu-id="e9a86-119">This workflow is split into **Analyze** and **Redact** modes.</span></span> <span data-ttu-id="e9a86-120">Du kan kombinera hello två lägen i ett steg som kör både aktiviteter i ett jobb. Det här läget kallas **kombinerade**.</span><span class="sxs-lookup"><span data-stu-id="e9a86-120">You can combine hello two modes in a single pass that runs both tasks in one job; this mode is called **Combined**.</span></span>

### <a name="combined-mode"></a><span data-ttu-id="e9a86-121">Kombinerade läge</span><span class="sxs-lookup"><span data-stu-id="e9a86-121">Combined mode</span></span>
<span data-ttu-id="e9a86-122">Detta genererar automatiskt en omarbetade mp4 utan alla manuella indata.</span><span class="sxs-lookup"><span data-stu-id="e9a86-122">This will produce a redacted mp4 automatically without any manual input.</span></span>

| <span data-ttu-id="e9a86-123">Fas</span><span class="sxs-lookup"><span data-stu-id="e9a86-123">Stage</span></span> | <span data-ttu-id="e9a86-124">Filnamn</span><span class="sxs-lookup"><span data-stu-id="e9a86-124">File Name</span></span> | <span data-ttu-id="e9a86-125">Anteckningar</span><span class="sxs-lookup"><span data-stu-id="e9a86-125">Notes</span></span> |
| --- | --- | --- |
| <span data-ttu-id="e9a86-126">Inkommande tillgångsinformation</span><span class="sxs-lookup"><span data-stu-id="e9a86-126">Input asset</span></span> |<span data-ttu-id="e9a86-127">foo.bar</span><span class="sxs-lookup"><span data-stu-id="e9a86-127">foo.bar</span></span> |<span data-ttu-id="e9a86-128">Video i WMV, MOV eller MP4-format</span><span class="sxs-lookup"><span data-stu-id="e9a86-128">Video in WMV, MOV, or MP4 format</span></span> |
| <span data-ttu-id="e9a86-129">Inkommande config</span><span class="sxs-lookup"><span data-stu-id="e9a86-129">Input config</span></span> |<span data-ttu-id="e9a86-130">Jobbet configuration förinställda</span><span class="sxs-lookup"><span data-stu-id="e9a86-130">Job configuration preset</span></span> |<span data-ttu-id="e9a86-131">{”version”:'1.0 ', 'alternativ': {'mode': 'kombineras'}}</span><span class="sxs-lookup"><span data-stu-id="e9a86-131">{'version':'1.0', 'options': {'mode':'combined'}}</span></span> |
| <span data-ttu-id="e9a86-132">Utdatatillgången</span><span class="sxs-lookup"><span data-stu-id="e9a86-132">Output asset</span></span> |<span data-ttu-id="e9a86-133">foo_redacted.mp4</span><span class="sxs-lookup"><span data-stu-id="e9a86-133">foo_redacted.mp4</span></span> |<span data-ttu-id="e9a86-134">Video med oskärpa tillämpas</span><span class="sxs-lookup"><span data-stu-id="e9a86-134">Video with blurring applied</span></span> |

#### <a name="input-example"></a><span data-ttu-id="e9a86-135">Inkommande exempel:</span><span class="sxs-lookup"><span data-stu-id="e9a86-135">Input example:</span></span>
[<span data-ttu-id="e9a86-136">Visa den här videon</span><span class="sxs-lookup"><span data-stu-id="e9a86-136">view this video</span></span>](http://ampdemo.azureedge.net/?url=http%3A%2F%2Freferencestream-samplestream.streaming.mediaservices.windows.net%2Fed99001d-72ee-4f91-9fc0-cd530d0adbbc%2FDancing.mp4)

#### <a name="output-example"></a><span data-ttu-id="e9a86-137">Exempel på utdata:</span><span class="sxs-lookup"><span data-stu-id="e9a86-137">Output example:</span></span>
[<span data-ttu-id="e9a86-138">Visa den här videon</span><span class="sxs-lookup"><span data-stu-id="e9a86-138">view this video</span></span>](http://ampdemo.azureedge.net/?url=http%3A%2F%2Freferencestream-samplestream.streaming.mediaservices.windows.net%2Fc6608001-e5da-429b-9ec8-d69d8f3bfc79%2Fdance_redacted.mp4)

### <a name="analyze-mode"></a><span data-ttu-id="e9a86-139">Analysera läge</span><span class="sxs-lookup"><span data-stu-id="e9a86-139">Analyze mode</span></span>
<span data-ttu-id="e9a86-140">Hej **analysera** förbikoppling av hello två gånger arbetsflöde tar en video-indata och producerar en JSON-fil av platser som står inför och jpg-bilder för varje upptäckt står inför.</span><span class="sxs-lookup"><span data-stu-id="e9a86-140">hello **analyze** pass of hello two-pass workflow takes a video input and produces a JSON file of face locations, and jpg images of each detected face.</span></span>

| <span data-ttu-id="e9a86-141">Fas</span><span class="sxs-lookup"><span data-stu-id="e9a86-141">Stage</span></span> | <span data-ttu-id="e9a86-142">Filnamn</span><span class="sxs-lookup"><span data-stu-id="e9a86-142">File Name</span></span> | <span data-ttu-id="e9a86-143">Anteckningar</span><span class="sxs-lookup"><span data-stu-id="e9a86-143">Notes</span></span> |
| --- | --- | --- |
| <span data-ttu-id="e9a86-144">Inkommande tillgångsinformation</span><span class="sxs-lookup"><span data-stu-id="e9a86-144">Input asset</span></span> |<span data-ttu-id="e9a86-145">foo.bar</span><span class="sxs-lookup"><span data-stu-id="e9a86-145">foo.bar</span></span> |<span data-ttu-id="e9a86-146">Video i WMV, MPV eller MP4-format</span><span class="sxs-lookup"><span data-stu-id="e9a86-146">Video in WMV, MPV, or MP4 format</span></span> |
| <span data-ttu-id="e9a86-147">Inkommande config</span><span class="sxs-lookup"><span data-stu-id="e9a86-147">Input config</span></span> |<span data-ttu-id="e9a86-148">Jobbet configuration förinställda</span><span class="sxs-lookup"><span data-stu-id="e9a86-148">Job configuration preset</span></span> |<span data-ttu-id="e9a86-149">{”version”:'1.0 ', 'alternativ': {'mode': 'Analysera'}}</span><span class="sxs-lookup"><span data-stu-id="e9a86-149">{'version':'1.0', 'options': {'mode':'analyze'}}</span></span> |
| <span data-ttu-id="e9a86-150">Utdatatillgången</span><span class="sxs-lookup"><span data-stu-id="e9a86-150">Output asset</span></span> |<span data-ttu-id="e9a86-151">foo_annotations.JSON</span><span class="sxs-lookup"><span data-stu-id="e9a86-151">foo_annotations.json</span></span> |<span data-ttu-id="e9a86-152">Anteckningsdata för platser som står inför i JSON-format.</span><span class="sxs-lookup"><span data-stu-id="e9a86-152">Annotation data of face locations in JSON format.</span></span> <span data-ttu-id="e9a86-153">Detta kan bara redigeras av hello användaren toomodify hello oskärpa avgränsar rutorna.</span><span class="sxs-lookup"><span data-stu-id="e9a86-153">This can be edited by hello user toomodify hello blurring bounding boxes.</span></span> <span data-ttu-id="e9a86-154">Se exemplet nedan.</span><span class="sxs-lookup"><span data-stu-id="e9a86-154">See sample below.</span></span> |
| <span data-ttu-id="e9a86-155">Utdatatillgången</span><span class="sxs-lookup"><span data-stu-id="e9a86-155">Output asset</span></span> |<span data-ttu-id="e9a86-156">foo_thumb%06d.jpg [foo_thumb000001.jpg, foo_thumb000002.jpg]</span><span class="sxs-lookup"><span data-stu-id="e9a86-156">foo_thumb%06d.jpg [foo_thumb000001.jpg, foo_thumb000002.jpg]</span></span> |<span data-ttu-id="e9a86-157">Beskuren jpg för varje upptäckt min, där hello siffran indikerar hello labelId av hello ansikte</span><span class="sxs-lookup"><span data-stu-id="e9a86-157">A cropped jpg of each detected face, where hello number indicates hello labelId of hello face</span></span> |

#### <a name="output-example"></a><span data-ttu-id="e9a86-158">Exempel på utdata:</span><span class="sxs-lookup"><span data-stu-id="e9a86-158">Output example:</span></span>

    {
      "version": 1,
      "timescale": 24000,
      "offset": 0,
      "framerate": 23.976,
      "width": 1280,
      "height": 720,
      "fragments": [
        {
          "start": 0,
          "duration": 48048,
          "interval": 1001,
          "events": [
            [],
            [],
            [],
            [],
            [],
            [],
            [],
            [],
            [],
            [],
            [],
            [],
            [],
            [
              {
                "index": 13,
                "id": 1138,
                "x": 0.29537,
                "y": -0.18987,
                "width": 0.36239,
                "height": 0.80335
              },
              {
                "index": 13,
                "id": 2028,
                "x": 0.60427,
                "y": 0.16098,
                "width": 0.26958,
                "height": 0.57943
              }
            ],

    … truncated

### <a name="redact-mode"></a><span data-ttu-id="e9a86-159">Redigera bort läge</span><span class="sxs-lookup"><span data-stu-id="e9a86-159">Redact mode</span></span>
<span data-ttu-id="e9a86-160">hello andra förbikoppling av hello arbetsflöde tar ett större antal indata måste kombineras i ett enskilt objekt.</span><span class="sxs-lookup"><span data-stu-id="e9a86-160">hello second pass of hello workflow takes a larger number of inputs that must be combined into a single asset.</span></span>

<span data-ttu-id="e9a86-161">Det finns en lista över ID: N tooblur hello ursprungliga video och hello anteckningar JSON.</span><span class="sxs-lookup"><span data-stu-id="e9a86-161">This includes a list of IDs tooblur, hello original video, and hello annotations JSON.</span></span> <span data-ttu-id="e9a86-162">Det här läget använder hello anteckningar tooapply suddar ut på hello indatavideo.</span><span class="sxs-lookup"><span data-stu-id="e9a86-162">This mode uses hello annotations tooapply blurring on hello input video.</span></span>

<span data-ttu-id="e9a86-163">hello inkluderar utdata från hello analysera pass inte hello ursprungliga video.</span><span class="sxs-lookup"><span data-stu-id="e9a86-163">hello output from hello Analyze pass does not include hello original video.</span></span> <span data-ttu-id="e9a86-164">hello video måste toobe överförs till hello inkommande tillgång för hello Redact läge aktivitet och valt som primärt hello-fil.</span><span class="sxs-lookup"><span data-stu-id="e9a86-164">hello video needs toobe uploaded into hello input asset for hello Redact mode task and selected as hello primary file.</span></span>

| <span data-ttu-id="e9a86-165">Fas</span><span class="sxs-lookup"><span data-stu-id="e9a86-165">Stage</span></span> | <span data-ttu-id="e9a86-166">Filnamn</span><span class="sxs-lookup"><span data-stu-id="e9a86-166">File Name</span></span> | <span data-ttu-id="e9a86-167">Anteckningar</span><span class="sxs-lookup"><span data-stu-id="e9a86-167">Notes</span></span> |
| --- | --- | --- |
| <span data-ttu-id="e9a86-168">Inkommande tillgångsinformation</span><span class="sxs-lookup"><span data-stu-id="e9a86-168">Input asset</span></span> |<span data-ttu-id="e9a86-169">foo.bar</span><span class="sxs-lookup"><span data-stu-id="e9a86-169">foo.bar</span></span> |<span data-ttu-id="e9a86-170">Video i WMV, MPV eller MP4-format.</span><span class="sxs-lookup"><span data-stu-id="e9a86-170">Video in WMV, MPV, or MP4 format.</span></span> <span data-ttu-id="e9a86-171">Samma video som i steg 1.</span><span class="sxs-lookup"><span data-stu-id="e9a86-171">Same video as in step 1.</span></span> |
| <span data-ttu-id="e9a86-172">Inkommande tillgångsinformation</span><span class="sxs-lookup"><span data-stu-id="e9a86-172">Input asset</span></span> |<span data-ttu-id="e9a86-173">foo_annotations.JSON</span><span class="sxs-lookup"><span data-stu-id="e9a86-173">foo_annotations.json</span></span> |<span data-ttu-id="e9a86-174">anteckningar metadatafil från den första fasen, med valfria ändringar.</span><span class="sxs-lookup"><span data-stu-id="e9a86-174">annotations metadata file from phase one, with optional modifications.</span></span> |
| <span data-ttu-id="e9a86-175">Inkommande tillgångsinformation</span><span class="sxs-lookup"><span data-stu-id="e9a86-175">Input asset</span></span> |<span data-ttu-id="e9a86-176">foo_IDList.txt (valfritt)</span><span class="sxs-lookup"><span data-stu-id="e9a86-176">foo_IDList.txt (Optional)</span></span> |<span data-ttu-id="e9a86-177">Valfria ny rad avgränsade lista över ansikte tooredact ID: N.</span><span class="sxs-lookup"><span data-stu-id="e9a86-177">Optional new line separated list of face IDs tooredact.</span></span> <span data-ttu-id="e9a86-178">Om tomt skapar detta oskärpa alla ytor.</span><span class="sxs-lookup"><span data-stu-id="e9a86-178">If left blank, this blurs all faces.</span></span> |
| <span data-ttu-id="e9a86-179">Inkommande config</span><span class="sxs-lookup"><span data-stu-id="e9a86-179">Input config</span></span> |<span data-ttu-id="e9a86-180">Jobbet configuration förinställda</span><span class="sxs-lookup"><span data-stu-id="e9a86-180">Job configuration preset</span></span> |<span data-ttu-id="e9a86-181">{”version”:'1.0 ', 'alternativ': {'mode': 'Redigera bort'}}</span><span class="sxs-lookup"><span data-stu-id="e9a86-181">{'version':'1.0', 'options': {'mode':'redact'}}</span></span> |
| <span data-ttu-id="e9a86-182">Utdatatillgången</span><span class="sxs-lookup"><span data-stu-id="e9a86-182">Output asset</span></span> |<span data-ttu-id="e9a86-183">foo_redacted.mp4</span><span class="sxs-lookup"><span data-stu-id="e9a86-183">foo_redacted.mp4</span></span> |<span data-ttu-id="e9a86-184">Video med oskärpa tillämpas baserat på anteckningar</span><span class="sxs-lookup"><span data-stu-id="e9a86-184">Video with blurring applied based on annotations</span></span> |

#### <a name="example-output"></a><span data-ttu-id="e9a86-185">Exempel på utdata</span><span class="sxs-lookup"><span data-stu-id="e9a86-185">Example output</span></span>
<span data-ttu-id="e9a86-186">Detta är hello utdata från ett IDList med ett ID som har valts.</span><span class="sxs-lookup"><span data-stu-id="e9a86-186">This is hello output from an IDList with one ID selected.</span></span>

[<span data-ttu-id="e9a86-187">Visa den här videon</span><span class="sxs-lookup"><span data-stu-id="e9a86-187">view this video</span></span>](http://ampdemo.azureedge.net/?url=http%3A%2F%2Freferencestream-samplestream.streaming.mediaservices.windows.net%2Fad6e24a2-4f9c-46ee-9fa7-bf05e20d19ac%2Fdance_redacted1.mp4)

<span data-ttu-id="e9a86-188">Exempel foo_IDList.txt</span><span class="sxs-lookup"><span data-stu-id="e9a86-188">Example foo_IDList.txt</span></span>
 
     1
     2
     3

## <a name="blur-types"></a><span data-ttu-id="e9a86-189">Oskärpa typer</span><span class="sxs-lookup"><span data-stu-id="e9a86-189">Blur types</span></span>

<span data-ttu-id="e9a86-190">I hello **kombinerade** eller **Redact** läge, 5 olika oskärpa lägen som du kan välja mellan via hello JSON-indata konfiguration: **låg**, **Med**, **Hög**, **felsöka**, och **svart**.</span><span class="sxs-lookup"><span data-stu-id="e9a86-190">In hello **Combined** or **Redact** mode, there are 5 different blur modes you can choose from via hello JSON input configuration: **Low**, **Med**, **High**, **Debug**, and **Black**.</span></span> <span data-ttu-id="e9a86-191">Som standard **Med** används.</span><span class="sxs-lookup"><span data-stu-id="e9a86-191">By default **Med** is used.</span></span>

<span data-ttu-id="e9a86-192">Du kan hitta exempel på hello oskärpa typer nedan.</span><span class="sxs-lookup"><span data-stu-id="e9a86-192">You can find samples of hello blur types below.</span></span>

### <a name="example-json"></a><span data-ttu-id="e9a86-193">Exempel JSON:</span><span class="sxs-lookup"><span data-stu-id="e9a86-193">Example JSON:</span></span>

    {'version':'1.0', 'options': {'Mode': 'Combined', 'BlurType': 'High'}}

#### <a name="low"></a><span data-ttu-id="e9a86-194">Låg</span><span class="sxs-lookup"><span data-stu-id="e9a86-194">Low</span></span>

![Låg](./media/media-services-face-redaction/blur1.png)
 
#### <a name="med"></a><span data-ttu-id="e9a86-196">Med</span><span class="sxs-lookup"><span data-stu-id="e9a86-196">Med</span></span>

![Med](./media/media-services-face-redaction/blur2.png)

#### <a name="high"></a><span data-ttu-id="e9a86-198">Hög</span><span class="sxs-lookup"><span data-stu-id="e9a86-198">High</span></span>

![Hög](./media/media-services-face-redaction/blur3.png)

#### <a name="debug"></a><span data-ttu-id="e9a86-200">Felsökning</span><span class="sxs-lookup"><span data-stu-id="e9a86-200">Debug</span></span>

![Felsökning](./media/media-services-face-redaction/blur4.png)

#### <a name="black"></a><span data-ttu-id="e9a86-202">Svart</span><span class="sxs-lookup"><span data-stu-id="e9a86-202">Black</span></span>

![Svart](./media/media-services-face-redaction/blur5.png)

## <a name="elements-of-hello-output-json-file"></a><span data-ttu-id="e9a86-204">Element i hello utdata-JSON-fil</span><span class="sxs-lookup"><span data-stu-id="e9a86-204">Elements of hello output JSON file</span></span>

<span data-ttu-id="e9a86-205">hello bortredigering MP kan hög precision min plats identifieras och spårning som kan identifiera dig too64 mänsklig ytor i en video ram.</span><span class="sxs-lookup"><span data-stu-id="e9a86-205">hello Redaction MP provides high precision face location detection and tracking that can detect up too64 human faces in a video frame.</span></span> <span data-ttu-id="e9a86-206">Främre ytor ange hello bästa resultat vid sidoytor och små ytor (mindre än eller lika med too24x24 pixlar) är en utmaning.</span><span class="sxs-lookup"><span data-stu-id="e9a86-206">Frontal faces provide hello best results, while side faces and small faces (less than or equal too24x24 pixels) are challenging.</span></span>

[!INCLUDE [media-services-analytics-output-json](../../includes/media-services-analytics-output-json.md)]

## <a name="net-sample-code"></a><span data-ttu-id="e9a86-207">Exempelkod för .NET</span><span class="sxs-lookup"><span data-stu-id="e9a86-207">.NET sample code</span></span>

<span data-ttu-id="e9a86-208">hello följande program visar hur du:</span><span class="sxs-lookup"><span data-stu-id="e9a86-208">hello following program shows how to:</span></span>

1. <span data-ttu-id="e9a86-209">Skapa en tillgång och överför en mediefil till hello tillgång.</span><span class="sxs-lookup"><span data-stu-id="e9a86-209">Create an asset and upload a media file into hello asset.</span></span>
2. <span data-ttu-id="e9a86-210">Skapa ett jobb med framsidan bortredigering uppgiften baserat på en konfigurationsfil som innehåller hello följande json förinställda.</span><span class="sxs-lookup"><span data-stu-id="e9a86-210">Create a job with a face redaction task based on a configuration file that contains hello following json preset.</span></span> 
   
        {'version':'1.0', 'options': {'mode':'combined'}}
3. <span data-ttu-id="e9a86-211">Hämta hello utdata JSON-filer.</span><span class="sxs-lookup"><span data-stu-id="e9a86-211">Download hello output JSON files.</span></span> 

#### <a name="create-and-configure-a-visual-studio-project"></a><span data-ttu-id="e9a86-212">Skapa och konfigurera ett Visual Studio-projekt</span><span class="sxs-lookup"><span data-stu-id="e9a86-212">Create and configure a Visual Studio project</span></span>

<span data-ttu-id="e9a86-213">Konfigurera utvecklingsmiljön och fylla hello app.config-fil med anslutningsinformation, enligt beskrivningen i [Media Services-utveckling med .NET](media-services-dotnet-how-to-use.md).</span><span class="sxs-lookup"><span data-stu-id="e9a86-213">Set up your development environment and populate hello app.config file with connection information, as described in [Media Services development with .NET](media-services-dotnet-how-to-use.md).</span></span> 

#### <a name="example"></a><span data-ttu-id="e9a86-214">Exempel</span><span class="sxs-lookup"><span data-stu-id="e9a86-214">Example</span></span>

    using System;
    using System.Configuration;
    using System.IO;
    using System.Linq;
    using Microsoft.WindowsAzure.MediaServices.Client;
    using System.Threading;
    using System.Threading.Tasks;

    namespace FaceRedaction
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

            // Run hello FaceRedaction job.
            var asset = RunFaceRedactionJob(@"C:\supportFiles\FaceRedaction\SomeFootage.mp4",
                        @"C:\supportFiles\FaceRedaction\config.json");

            // Download hello job output asset.
            DownloadAsset(asset, @"C:\supportFiles\FaceRedaction\Output");
        }

        static IAsset RunFaceRedactionJob(string inputMediaFilePath, string configurationFile)
        {
            // Create an asset and upload hello input media file toostorage.
            IAsset asset = CreateAssetAndUploadSingleFile(inputMediaFilePath,
            "My Face Redaction Input Asset",
            AssetCreationOptions.None);

            // Declare a new job.
            IJob job = _context.Jobs.Create("My Face Redaction Job");

            // Get a reference tooAzure Media Redactor.
            string MediaProcessorName = "Azure Media Redactor";

            var processor = GetLatestMediaProcessorByName(MediaProcessorName);

            // Read configuration from hello specified file.
            string configuration = File.ReadAllText(configurationFile);

            // Create a task with hello encoding details, using a string preset.
            ITask task = job.Tasks.AddNew("My Face Redaction Task",
            processor,
            configuration,
            TaskOptions.None);

            // Specify hello input asset.
            task.InputAssets.Add(asset);

            // Add an output asset toocontain hello results of hello job.
            task.OutputAssets.AddNew("My Face Redaction Output Asset", AssetCreationOptions.None);

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

## <a name="next-steps"></a><span data-ttu-id="e9a86-215">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="e9a86-215">Next steps</span></span>

[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a><span data-ttu-id="e9a86-216">Ge feedback</span><span class="sxs-lookup"><span data-stu-id="e9a86-216">Provide feedback</span></span>
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

## <a name="related-links"></a><span data-ttu-id="e9a86-217">Relaterade länkar</span><span class="sxs-lookup"><span data-stu-id="e9a86-217">Related links</span></span>
[<span data-ttu-id="e9a86-218">Azure Media Services Analytics-översikt</span><span class="sxs-lookup"><span data-stu-id="e9a86-218">Azure Media Services Analytics Overview</span></span>](media-services-analytics-overview.md)

[<span data-ttu-id="e9a86-219">Azure Media Analytics demonstrationer</span><span class="sxs-lookup"><span data-stu-id="e9a86-219">Azure Media Analytics demos</span></span>](http://azuremedialabs.azurewebsites.net/demos/Analytics.html)

