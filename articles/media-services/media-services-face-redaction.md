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
# <a name="redact-faces-with-azure-media-analytics"></a>Redigera bort personerna bakom Azure Media Analytics
## <a name="overview"></a>Översikt
**Azure Media Redactor** är en [Azure Media Analytics](media-services-analytics-overview.md) medieprocessor (HP) som erbjuder skalbara ansikte bortredigering i hello molnet. Framsidan bortredigering aktiverar du toomodify videon i ordning tooblur ytor valda enskilda användare. Du kanske vill toouse hello ansikte bortredigering service i offentliga säkerhet och nyheter media scenarier. Några minuter med material som innehåller flera ytor kan ta timmar tooredact manuellt, men med den här tjänsten hello ansikte bortredigering processen tar bara några få enkla steg. Mer information finns i [detta](https://azure.microsoft.com/blog/azure-media-redactor/) blogg.

Det här avsnittet innehåller information om **Azure Media Redactor** och visar hur toouse med Media Services SDK för .NET.

Hej **Azure Media Redactor** MP är för närvarande under förhandsgranskning. Den är tillgänglig i alla offentliga Azure-regioner som tillhör amerikanska myndigheter och Kina datacenter. Den här förhandsgranskningen är för närvarande kostnadsfritt. 

## <a name="face-redaction-modes"></a>Framsidan bortredigering lägen
Ansikte bortredigering fungerar genom att identifiera ytor i varje ram av video och spåra hello ansikte objektet både framåt och bakåt i tiden, så att hello samma person kan oskarpa från andra vinklar samt. hello automatiserade bortredigering processen är mycket komplex och alltid skapar inte 100% av önskade utgående därför Media Analytics ger dig med ett par olika sätt toomodify hello slutgiltiga utdatan.

I tillägg tooa fullständigt automatiskt läge finns ett arbetsflöde för två gånger där hello val/de-selection av hittade ytor via en lista över ID: N. Dessutom använder toomake godtycklig per ram justeringar hello MP en metadatafil i JSON-format. Det här arbetsflödet är uppdelat i **analysera** och **Redact** lägen. Du kan kombinera hello två lägen i ett steg som kör både aktiviteter i ett jobb. Det här läget kallas **kombinerade**.

### <a name="combined-mode"></a>Kombinerade läge
Detta genererar automatiskt en omarbetade mp4 utan alla manuella indata.

| Fas | Filnamn | Anteckningar |
| --- | --- | --- |
| Inkommande tillgångsinformation |foo.bar |Video i WMV, MOV eller MP4-format |
| Inkommande config |Jobbet configuration förinställda |{”version”:'1.0 ', 'alternativ': {'mode': 'kombineras'}} |
| Utdatatillgången |foo_redacted.mp4 |Video med oskärpa tillämpas |

#### <a name="input-example"></a>Inkommande exempel:
[Visa den här videon](http://ampdemo.azureedge.net/?url=http%3A%2F%2Freferencestream-samplestream.streaming.mediaservices.windows.net%2Fed99001d-72ee-4f91-9fc0-cd530d0adbbc%2FDancing.mp4)

#### <a name="output-example"></a>Exempel på utdata:
[Visa den här videon](http://ampdemo.azureedge.net/?url=http%3A%2F%2Freferencestream-samplestream.streaming.mediaservices.windows.net%2Fc6608001-e5da-429b-9ec8-d69d8f3bfc79%2Fdance_redacted.mp4)

### <a name="analyze-mode"></a>Analysera läge
Hej **analysera** förbikoppling av hello två gånger arbetsflöde tar en video-indata och producerar en JSON-fil av platser som står inför och jpg-bilder för varje upptäckt står inför.

| Fas | Filnamn | Anteckningar |
| --- | --- | --- |
| Inkommande tillgångsinformation |foo.bar |Video i WMV, MPV eller MP4-format |
| Inkommande config |Jobbet configuration förinställda |{”version”:'1.0 ', 'alternativ': {'mode': 'Analysera'}} |
| Utdatatillgången |foo_annotations.JSON |Anteckningsdata för platser som står inför i JSON-format. Detta kan bara redigeras av hello användaren toomodify hello oskärpa avgränsar rutorna. Se exemplet nedan. |
| Utdatatillgången |foo_thumb%06d.jpg [foo_thumb000001.jpg, foo_thumb000002.jpg] |Beskuren jpg för varje upptäckt min, där hello siffran indikerar hello labelId av hello ansikte |

#### <a name="output-example"></a>Exempel på utdata:

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

### <a name="redact-mode"></a>Redigera bort läge
hello andra förbikoppling av hello arbetsflöde tar ett större antal indata måste kombineras i ett enskilt objekt.

Det finns en lista över ID: N tooblur hello ursprungliga video och hello anteckningar JSON. Det här läget använder hello anteckningar tooapply suddar ut på hello indatavideo.

hello inkluderar utdata från hello analysera pass inte hello ursprungliga video. hello video måste toobe överförs till hello inkommande tillgång för hello Redact läge aktivitet och valt som primärt hello-fil.

| Fas | Filnamn | Anteckningar |
| --- | --- | --- |
| Inkommande tillgångsinformation |foo.bar |Video i WMV, MPV eller MP4-format. Samma video som i steg 1. |
| Inkommande tillgångsinformation |foo_annotations.JSON |anteckningar metadatafil från den första fasen, med valfria ändringar. |
| Inkommande tillgångsinformation |foo_IDList.txt (valfritt) |Valfria ny rad avgränsade lista över ansikte tooredact ID: N. Om tomt skapar detta oskärpa alla ytor. |
| Inkommande config |Jobbet configuration förinställda |{”version”:'1.0 ', 'alternativ': {'mode': 'Redigera bort'}} |
| Utdatatillgången |foo_redacted.mp4 |Video med oskärpa tillämpas baserat på anteckningar |

#### <a name="example-output"></a>Exempel på utdata
Detta är hello utdata från ett IDList med ett ID som har valts.

[Visa den här videon](http://ampdemo.azureedge.net/?url=http%3A%2F%2Freferencestream-samplestream.streaming.mediaservices.windows.net%2Fad6e24a2-4f9c-46ee-9fa7-bf05e20d19ac%2Fdance_redacted1.mp4)

Exempel foo_IDList.txt
 
     1
     2
     3

## <a name="blur-types"></a>Oskärpa typer

I hello **kombinerade** eller **Redact** läge, 5 olika oskärpa lägen som du kan välja mellan via hello JSON-indata konfiguration: **låg**, **Med**, **Hög**, **felsöka**, och **svart**. Som standard **Med** används.

Du kan hitta exempel på hello oskärpa typer nedan.

### <a name="example-json"></a>Exempel JSON:

    {'version':'1.0', 'options': {'Mode': 'Combined', 'BlurType': 'High'}}

#### <a name="low"></a>Låg

![Låg](./media/media-services-face-redaction/blur1.png)
 
#### <a name="med"></a>Med

![Med](./media/media-services-face-redaction/blur2.png)

#### <a name="high"></a>Hög

![Hög](./media/media-services-face-redaction/blur3.png)

#### <a name="debug"></a>Felsökning

![Felsökning](./media/media-services-face-redaction/blur4.png)

#### <a name="black"></a>Svart

![Svart](./media/media-services-face-redaction/blur5.png)

## <a name="elements-of-hello-output-json-file"></a>Element i hello utdata-JSON-fil

hello bortredigering MP kan hög precision min plats identifieras och spårning som kan identifiera dig too64 mänsklig ytor i en video ram. Främre ytor ange hello bästa resultat vid sidoytor och små ytor (mindre än eller lika med too24x24 pixlar) är en utmaning.

[!INCLUDE [media-services-analytics-output-json](../../includes/media-services-analytics-output-json.md)]

## <a name="net-sample-code"></a>Exempelkod för .NET

hello följande program visar hur du:

1. Skapa en tillgång och överför en mediefil till hello tillgång.
2. Skapa ett jobb med framsidan bortredigering uppgiften baserat på en konfigurationsfil som innehåller hello följande json förinställda. 
   
        {'version':'1.0', 'options': {'mode':'combined'}}
3. Hämta hello utdata JSON-filer. 

#### <a name="create-and-configure-a-visual-studio-project"></a>Skapa och konfigurera ett Visual Studio-projekt

Konfigurera utvecklingsmiljön och fylla hello app.config-fil med anslutningsinformation, enligt beskrivningen i [Media Services-utveckling med .NET](media-services-dotnet-how-to-use.md). 

#### <a name="example"></a>Exempel

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

## <a name="next-steps"></a>Nästa steg

[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Ge feedback
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

## <a name="related-links"></a>Relaterade länkar
[Azure Media Services Analytics-översikt](media-services-analytics-overview.md)

[Azure Media Analytics demonstrationer](http://azuremedialabs.azurewebsites.net/demos/Analytics.html)

