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
# <a name="hyperlapse-media-files-with-azure-media-hyperlapse"></a>Videostabilisera mediefiler med Azure Media Hyperlapse
Azure Media Hyperlapse är ett Media Processor (HP) som skapar smooth tid upphörde att gälla videor från första person eller åtgärd kamera innehåll.  Hej molnbaserade på samma nivå för[Microsoft Research skrivbord Videostabilisera Pro och telefon Videostabilisera Mobile](http://aka.ms/hyperlapse), Microsoft Hyperlapse för Azure Media Services använder hello massiv skala hello Azure Media Services Media Bearbetning av plattform toohorizontally skala och parallelize bulk Videostabilisera bearbetning.

> [!IMPORTANT]
> Microsoft Hyperlapse är utformad toowork som är bäst på första person innehåll med en glidande kamera.  Även om fortfarande övervakningskameror kan fortfarande fungerar, kan inte om hello Azure Media Hyperlapse Medieprocessor hello prestanda och kvalitet garanteras för andra typer av innehåll.  Mer om Microsoft Hyperlapse för Azure Media Services toolearn och se några exempel videoklipp, kolla hello [inledande blogginlägget](http://aka.ms/azurehyperlapseblog) från hello förhandsversion.
> 
> 

Ett Azure Media Hyperlapse jobbet tar som indata en MP4, MOV eller WMV resursfil tillsammans med en konfigurationsfil som anger vilka ramar av video ska vara tid upphörde att gälla och toowhat hastighet (t.ex. första 10 000 ramar på 2 x).  hello-utdata är ett stabilt och tid slut återgivning av hello indatavideo.

Hello Azure Media Hyperlapse uppdaterad finns [Media Services bloggar](https://azure.microsoft.com/blog/topics/media-services/).

## <a name="hyperlapse-an-asset"></a>Videostabilisera en tillgång
Först måste tooupload din önskade indatafilen tooAzure Media Services.  Mer om toolearn hello begrepp som ingår i överföringen och hantera innehåll, läsa hello [innehållshantering artikel](media-services-portal-vod-get-started.md).

### <a id="configuration"></a>Konfigurationen förinställda för Hyperlapse
När det är ditt innehåll i Media Services-konto, behöver du tooconstruct konfigurationen förinställningen.  hello i den följande tabellen beskrivs hello användardefinierade fält:

| Fält | Beskrivning |
| --- | --- |
| StartFrame |hello ram på vilka hello Microsoft Hyperlapse bearbetning ska börja. |
| NumFrames |hello antalet ramar tooprocess |
| Hastighet |hello faktor med vilken toospeed in hello indatavideo. |

hello följande är ett exempel på en fil i XML- och JSON ska följa aktuell standard:

**XML-förinställda:**

    <?xml version="1.0" encoding="utf-16"?>
    <Preset xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:xsd="http://www.w3.org/2001/XMLSchema" Version="1.0" xmlns="http://www.windowsazure.com/media/encoding/Preset/2014/03">
        <Sources>
            <Source StartFrame="0" NumFrames="10000" />
        </Sources>
        <Options>
            <Speed>12</Speed>
        </Options>
    </Preset>

**JSON förinställda:**

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

### <a id="sample_code"></a>Microsoft Hyperlapse med hello AMS .NET SDK
hello följande metod överför en mediefil som en tillgång och skapar ett jobb med hello Azure Media Hyperlapse Medieprocessor.

> [!NOTE]
> Du bör redan har en CloudMediaContext i omfång med hello namnet ”kontext” för den här koden toowork.  Mer information om detta, Läs hello toolearn [innehållshantering artikel](media-services-dotnet-get-started.md).
> 
> [!NOTE]
> hello strängargument ”hyperConfig” är förväntade toobe en konfiguration ska följa aktuell standard förinställda i JSON- eller XML-enligt beskrivningen ovan.
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

### <a id="file_types"></a>Filtyper som stöds
* MP4
* MOV
* WMV

## <a name="media-services-learning-paths"></a>Sökvägar för Media Services-utbildning
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Ge feedback
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

## <a name="related-links"></a>Relaterade länkar
[Azure Media Services Analytics-översikt](media-services-analytics-overview.md)

[Azure Media Analytics demonstrationer](http://azuremedialabs.azurewebsites.net/demos/Analytics.html)

