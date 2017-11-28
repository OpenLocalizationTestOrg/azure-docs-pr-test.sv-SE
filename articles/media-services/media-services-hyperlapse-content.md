---
title: aaaHyperlapse mediefiler med Azure Media Hyperlapse | Microsoft Docs
description: "Azure Media Hyperlapse skapar smooth tid upphörde att gälla videor från första person eller åtgärd kamera innehåll. Det här avsnittet visar hur toouse Media indexeraren."
services: media-services
documentationcenter: 
author: asolanki
manager: johndeu
editor: 
ms.assetid: 37d54db6-9cf3-4ae9-b3c6-0d29c744e965
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 03/02/2017
ms.author: adsolank
ms.openlocfilehash: 85bb07206d0ca2f5b2fd0767e6ed4904195d3ab6
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="hyperlapse-media-files-with-azure-media-hyperlapse"></a><span data-ttu-id="99cc9-104">Videostabilisera mediefiler med Azure Media Hyperlapse</span><span class="sxs-lookup"><span data-stu-id="99cc9-104">Hyperlapse Media Files with Azure Media Hyperlapse</span></span>
<span data-ttu-id="99cc9-105">Azure Media Hyperlapse är ett Media Processor (HP) som skapar smooth tid upphörde att gälla videor från första person eller åtgärd kamera innehåll.</span><span class="sxs-lookup"><span data-stu-id="99cc9-105">Azure Media Hyperlapse is a Media Processor (MP) that creates smooth time-lapsed videos from first-person or action-camera content.</span></span>  <span data-ttu-id="99cc9-106">Hej molnbaserade på samma nivå för[Microsoft Research skrivbord Videostabilisera Pro och telefon Videostabilisera Mobile](http://aka.ms/hyperlapse), Microsoft Hyperlapse för Azure Media Services använder hello massiv skala hello Azure Media Services Media Bearbetning av plattform toohorizontally skala och parallelize bulk Videostabilisera bearbetning.</span><span class="sxs-lookup"><span data-stu-id="99cc9-106">hello cloud-based sibling too[Microsoft Research's desktop Hyperlapse Pro and phone-based Hyperlapse Mobile](http://aka.ms/hyperlapse), Microsoft Hyperlapse for Azure Media Services utilizes hello massive scale of hello Azure Media Services Media Processing platform toohorizontally scale and parallelize bulk Hyperlapse processing.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="99cc9-107">Microsoft Hyperlapse är utformad toowork som är bäst på första person innehåll med en glidande kamera.</span><span class="sxs-lookup"><span data-stu-id="99cc9-107">Microsoft Hyperlapse is designed toowork best on first-person content with a moving camera.</span></span>  <span data-ttu-id="99cc9-108">Även om fortfarande övervakningskameror kan fortfarande fungerar, kan inte om hello Azure Media Hyperlapse Medieprocessor hello prestanda och kvalitet garanteras för andra typer av innehåll.</span><span class="sxs-lookup"><span data-stu-id="99cc9-108">Although still-camera footage can still work, hello performance and quality of hello Azure Media Hyperlapse Media Processor cannot be guaranteed for other types of content.</span></span>  <span data-ttu-id="99cc9-109">Mer om Microsoft Hyperlapse för Azure Media Services toolearn och se några exempel videoklipp, kolla hello [inledande blogginlägget](http://aka.ms/azurehyperlapseblog) från hello förhandsversion.</span><span class="sxs-lookup"><span data-stu-id="99cc9-109">toolearn more about Microsoft Hyperlapse for Azure Media Services and see some example videos, check out hello [introductory blog post](http://aka.ms/azurehyperlapseblog) from hello public preview.</span></span>
> 
> 

<span data-ttu-id="99cc9-110">Ett Azure Media Hyperlapse jobbet tar som indata en MP4, MOV eller WMV resursfil tillsammans med en konfigurationsfil som anger vilka ramar av video ska vara tid upphörde att gälla och toowhat hastighet (t.ex. första 10 000 ramar på 2 x).</span><span class="sxs-lookup"><span data-stu-id="99cc9-110">An Azure Media Hyperlapse job takes as input an MP4, MOV, or WMV asset file along with a configuration file that specifies which frames of video should be time-lapsed and toowhat speed (e.g. first 10,000 frames at 2x).</span></span>  <span data-ttu-id="99cc9-111">hello-utdata är ett stabilt och tid slut återgivning av hello indatavideo.</span><span class="sxs-lookup"><span data-stu-id="99cc9-111">hello output is a stabilized and time-lapsed rendition of hello input video.</span></span>

<span data-ttu-id="99cc9-112">Hello Azure Media Hyperlapse uppdaterad finns [Media Services bloggar](https://azure.microsoft.com/blog/topics/media-services/).</span><span class="sxs-lookup"><span data-stu-id="99cc9-112">For hello latest Azure Media Hyperlapse updates, see [Media Services blogs](https://azure.microsoft.com/blog/topics/media-services/).</span></span>

## <a name="hyperlapse-an-asset"></a><span data-ttu-id="99cc9-113">Videostabilisera en tillgång</span><span class="sxs-lookup"><span data-stu-id="99cc9-113">Hyperlapse an asset</span></span>
<span data-ttu-id="99cc9-114">Först måste tooupload din önskade indatafilen tooAzure Media Services.</span><span class="sxs-lookup"><span data-stu-id="99cc9-114">First you will need tooupload your desired input file tooAzure Media Services.</span></span>  <span data-ttu-id="99cc9-115">Mer om toolearn hello begrepp som ingår i överföringen och hantera innehåll, läsa hello [innehållshantering artikel](media-services-portal-vod-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="99cc9-115">toolearn more about hello concepts involved with uploading and managing content, read hello [content management article](media-services-portal-vod-get-started.md).</span></span>

### <span data-ttu-id="99cc9-116"><a id="configuration"></a>Konfigurationen förinställda för Hyperlapse</span><span class="sxs-lookup"><span data-stu-id="99cc9-116"><a id="configuration"></a>Configuration Preset for Hyperlapse</span></span>
<span data-ttu-id="99cc9-117">När det är ditt innehåll i Media Services-konto, behöver du tooconstruct konfigurationen förinställningen.</span><span class="sxs-lookup"><span data-stu-id="99cc9-117">Once your content is in your Media Services account, you will need tooconstruct your configuration preset.</span></span>  <span data-ttu-id="99cc9-118">hello i den följande tabellen beskrivs hello användardefinierade fält:</span><span class="sxs-lookup"><span data-stu-id="99cc9-118">hello following table explains hello user-specified fields:</span></span>

| <span data-ttu-id="99cc9-119">Fält</span><span class="sxs-lookup"><span data-stu-id="99cc9-119">Field</span></span> | <span data-ttu-id="99cc9-120">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="99cc9-120">Description</span></span> |
| --- | --- |
| <span data-ttu-id="99cc9-121">StartFrame</span><span class="sxs-lookup"><span data-stu-id="99cc9-121">StartFrame</span></span> |<span data-ttu-id="99cc9-122">hello ram på vilka hello Microsoft Hyperlapse bearbetning ska börja.</span><span class="sxs-lookup"><span data-stu-id="99cc9-122">hello frame upon which hello Microsoft Hyperlapse processing should begin.</span></span> |
| <span data-ttu-id="99cc9-123">NumFrames</span><span class="sxs-lookup"><span data-stu-id="99cc9-123">NumFrames</span></span> |<span data-ttu-id="99cc9-124">hello antalet ramar tooprocess</span><span class="sxs-lookup"><span data-stu-id="99cc9-124">hello number of frames tooprocess</span></span> |
| <span data-ttu-id="99cc9-125">Hastighet</span><span class="sxs-lookup"><span data-stu-id="99cc9-125">Speed</span></span> |<span data-ttu-id="99cc9-126">hello faktor med vilken toospeed in hello indatavideo.</span><span class="sxs-lookup"><span data-stu-id="99cc9-126">hello factor with which toospeed up hello input video.</span></span> |

<span data-ttu-id="99cc9-127">hello följande är ett exempel på en fil i XML- och JSON ska följa aktuell standard:</span><span class="sxs-lookup"><span data-stu-id="99cc9-127">hello following is an example of a conformant configuration file in XML and JSON:</span></span>

<span data-ttu-id="99cc9-128">**XML-förinställda:**</span><span class="sxs-lookup"><span data-stu-id="99cc9-128">**XML preset:**</span></span>

    <?xml version="1.0" encoding="utf-16"?>
    <Preset xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:xsd="http://www.w3.org/2001/XMLSchema" Version="1.0" xmlns="http://www.windowsazure.com/media/encoding/Preset/2014/03">
        <Sources>
            <Source StartFrame="0" NumFrames="10000" />
        </Sources>
        <Options>
            <Speed>12</Speed>
        </Options>
    </Preset>

<span data-ttu-id="99cc9-129">**JSON förinställda:**</span><span class="sxs-lookup"><span data-stu-id="99cc9-129">**JSON preset:**</span></span>

    {
        "Version":1.0,
        "Sources": [
            {
                "StartFrame":0,
                "NumFrames":2147483647
            }
        ],
        "Options": {
            "Speed":1,
            "Stabilize":false
        }
    }

### <span data-ttu-id="99cc9-130"><a id="sample_code"></a>Microsoft Hyperlapse med hello AMS .NET SDK</span><span class="sxs-lookup"><span data-stu-id="99cc9-130"><a id="sample_code"></a> Microsoft Hyperlapse with hello AMS .NET SDK</span></span>
<span data-ttu-id="99cc9-131">hello följande metod överför en mediefil som en tillgång och skapar ett jobb med hello Azure Media Hyperlapse Medieprocessor.</span><span class="sxs-lookup"><span data-stu-id="99cc9-131">hello following method uploads a media file as an asset and creates a job with hello Azure Media Hyperlapse Media Processor.</span></span>

> [!NOTE]
> <span data-ttu-id="99cc9-132">Du bör redan har en CloudMediaContext i omfång med hello namnet ”kontext” för den här koden toowork.</span><span class="sxs-lookup"><span data-stu-id="99cc9-132">You should already have a CloudMediaContext in scope with hello name "context" for this code toowork.</span></span>  <span data-ttu-id="99cc9-133">Mer information om detta, Läs hello toolearn [innehållshantering artikel](media-services-dotnet-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="99cc9-133">toolearn more about this, read hello [content management article](media-services-dotnet-get-started.md).</span></span>
> 
> [!NOTE]
> <span data-ttu-id="99cc9-134">hello strängargument ”hyperConfig” är förväntade toobe en konfiguration ska följa aktuell standard förinställda i JSON- eller XML-enligt beskrivningen ovan.</span><span class="sxs-lookup"><span data-stu-id="99cc9-134">hello string argument "hyperConfig" is expected toobe a conformant configuration preset in either JSON or XML as described above.</span></span>
> 
> 

        static bool RunHyperlapseJob(string input, string output, string hyperConfig)
        {
            // create asset with input file
            IAsset asset = context
            .Assets
            .CreateAssetAndUploadSingleFile(input, "My Hyperlapse Input", AssetCreationOptions.None);

            // grab instances of Azure Media Hyperlapse MP
            IMediaProcessor mp = context
            .MediaProcessors
            .GetLatestMediaProcessorByName("Azure Media Hyperlapse");

            // create Job with Hyperlapse task
            IJob job = context
            .Jobs
            .Create(String.Format("Hyperlapse {0}", input));

            if (String.IsNullOrEmpty(hyperConfig))
            {
            // config cannot be empty
            return false;
            }

            hyperConfig = File.ReadAllText(hyperConfig);

            ITask hyperlapseTask = job.Tasks.AddNew("Hyperlapse task",
            mp,
            hyperConfig,
            TaskOptions.None);
            hyperlapseTask.InputAssets.Add(asset);
            hyperlapseTask.OutputAssets.AddNew("Hyperlapse output",
            AssetCreationOptions.None);

            job.Submit();

            // Create progress printing and querying tasks
            Task progressPrintTask = new Task(() =>
            {

            IJob jobQuery = null;
            do
            {
                var progressContext = context;
                jobQuery = progressContext.Jobs
                .Where(j => j.Id == job.Id)
                .First();
                Console.WriteLine(string.Format("{0}\t{1}\t{2}",
                DateTime.Now,
                jobQuery.State,
                jobQuery.Tasks[0].Progress));
                Thread.Sleep(10000);
            }
            while (jobQuery.State != JobState.Finished &&
                                   jobQuery.State != JobState.Error &&
                                   jobQuery.State != JobState.Canceled);
                });
                
            progressPrintTask.Start();

            Task progressJobTask = job.GetExecutionProgressTask(
                                                 CancellationToken.None);
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
                return false;                  
            }

        DownloadAsset(job.OutputMediaAssets.First(), output);
        return true;
    }

    static void DownloadAsset(IAsset asset, string outputDirectory)
    {
        foreach (IAssetFile file in asset.AssetFiles)
        {
            file.Download(Path.Combine(outputDirectory, file.Name));
        }
    }


    static IAsset CreateAssetAndUploadSingleFile(string filePath, string assetName, AssetCreationOptions options)
    {
        IAsset asset = context.Assets.Create(assetName, options);

        var assetFile = asset.AssetFiles.Create(Path.GetFileName(filePath));
        assetFile.Upload(filePath);

        return asset;
    }

    static IMediaProcessor GetLatestMediaProcessorByName(string mediaProcessorName)
    {
        var processor = context.MediaProcessors
        .Where(p => p.Name == mediaProcessorName)
        .ToList()
        .OrderBy(p => new Version(p.Version))
        .LastOrDefault();

        if (processor == null)
            throw new ArgumentException(string.Format("Unknown media processor",
                                                       mediaProcessorName));

        return processor;
    }

### <span data-ttu-id="99cc9-135"><a id="file_types"></a>Filtyper som stöds</span><span class="sxs-lookup"><span data-stu-id="99cc9-135"><a id="file_types"></a>Supported File types</span></span>
* <span data-ttu-id="99cc9-136">MP4</span><span class="sxs-lookup"><span data-stu-id="99cc9-136">MP4</span></span>
* <span data-ttu-id="99cc9-137">MOV</span><span class="sxs-lookup"><span data-stu-id="99cc9-137">MOV</span></span>
* <span data-ttu-id="99cc9-138">WMV</span><span class="sxs-lookup"><span data-stu-id="99cc9-138">WMV</span></span>

## <a name="media-services-learning-paths"></a><span data-ttu-id="99cc9-139">Sökvägar för Media Services-utbildning</span><span class="sxs-lookup"><span data-stu-id="99cc9-139">Media Services learning paths</span></span>
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a><span data-ttu-id="99cc9-140">Ge feedback</span><span class="sxs-lookup"><span data-stu-id="99cc9-140">Provide feedback</span></span>
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

## <a name="related-links"></a><span data-ttu-id="99cc9-141">Relaterade länkar</span><span class="sxs-lookup"><span data-stu-id="99cc9-141">Related links</span></span>
[<span data-ttu-id="99cc9-142">Azure Media Services Analytics-översikt</span><span class="sxs-lookup"><span data-stu-id="99cc9-142">Azure Media Services Analytics Overview</span></span>](media-services-analytics-overview.md)

[<span data-ttu-id="99cc9-143">Azure Media Analytics demonstrationer</span><span class="sxs-lookup"><span data-stu-id="99cc9-143">Azure Media Analytics demos</span></span>](http://azuremedialabs.azurewebsites.net/demos/Analytics.html)

