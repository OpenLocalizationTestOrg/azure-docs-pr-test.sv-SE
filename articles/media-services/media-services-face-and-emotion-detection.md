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
# <a name="detect-face-and-emotion-with-azure-media-analytics"></a>Identifiera Ansikts- och Känslo med Azure Media Analytics
## <a name="overview"></a>Översikt
Hej **Azure Media ansikte detektor** medieprocessor (HP) kan du toocount, spåra förflyttningar och även mätaren målgruppen deltagande och reaktion via ansikte uttryck. Den här tjänsten innehåller två funktioner: 

* **Ansikts-identifiering**
  
    Identifiering av framsidan hittar och spårar mänsklig ytor inom en video. Flera ytor kan identifieras och spåras senare när de flyttas runt, med hello tid och plats metadata som returneras i en JSON-fil. Under spårning försöker toogive en konsekvent ID toohello samma står inför när hello person flyttar på skärmen, även om de hindras eller lämna en kort hello ram.
  
  > [!NOTE]
  > Den här tjänsten inte att utföra ansiktsigenkänning. En person som lämnar hello ram eller blir hindras för länge får ett nytt-ID när de kommer tillbaka.
  > 
  > 
* **Känsloigenkänning**
  
    Känsloigenkänning är en valfri komponent i hello ansikte identifiering Medieprocessor som returnerar analysis på flera känslomässiga attribut från hello ytor upptäckt, inklusive lycka, sadness, fruktan, resulterande och mycket mer. 

Hej **Azure Media ansikte detektor** MP är för närvarande under förhandsgranskning.

Det här avsnittet innehåller information om **Azure Media ansikte detektor** och visar hur toouse med Media Services SDK för .NET.

## <a name="face-detector-input-files"></a>Står inför detektor inkommande filer
Videofiler. För närvarande hello följande format stöds: MP4, MOV och WMV.

## <a name="face-detector-output-files"></a>Står inför detektor utdatafilerna
hello ansikte identifiering och spårning API kan hög precision min plats identifieras och spårning som kan identifiera dig too64 mänsklig ytor i en video. Främre ytor ange hello bästa resultat vid sidoytor och små ytor (mindre än eller lika med too24x24 bildpunkter) kanske inte lika exakt.

hello identifierade och spårade ytor returneras med koordinaterna (vänster, överkant, bredd och höjd) som visar hello platsen för ytor hello bild i bildpunkter som en ansikte ID-nummer som visar hello spårning av att enskilda. Ansikts-ID-nummer är felbenägna tooreset under omständigheter när hello främre yta tappas bort eller överlappas i hello ram ledde till vissa individer komma tilldelade flera ID: N.

## <a id="output_elements"></a>Element i hello utdata-JSON-fil

[!INCLUDE [media-services-analytics-output-json](../../includes/media-services-analytics-output-json.md)]

Framsidan detektor använder tekniker för fragmentering (där hello metadata kan delas upp i tidsbaserade segment och du kan hämta vad du behöver bara) och segmentering (där hello händelser har delats om de blir för stor). Med hjälp av några enkla beräkningar kan du omvandla hello data. Om exempelvis en händelse startade 6300 (ticks) med en tidsrymd för 2997 (ticks per sekund) och bildhastighet sedan 29,97 (bildrutor per sekund):

* Start/tidsrymd = 2.1 sekunder
* Sekunder x Framerate = 63 ramar

## <a name="face-detection-input-and-output-example"></a>Står inför identifiering indata och utdata exempel
### <a name="input-video"></a>Indatavideo
[Indatavideo](http://ampdemo.azureedge.net/azuremediaplayer.html?url=https%3A%2F%2Freferencestream-samplestream.streaming.mediaservices.windows.net%2Fc8834d9f-0b49-4b38-bcaf-ece2746f1972%2FMicrosoft%20Convergence%202015%20%20Keynote%20Highlights.ism%2Fmanifest&amp;autoplay=false)

### <a name="task-configuration-preset"></a>Uppgiftskonfigurationen (förinställda)
När du skapar en uppgift med **Azure Media ansikte detektor**, måste du ange en konfiguration förinställning. följande konfiguration förinställda hello gäller bara för framsidan identifiering.

    {
      "version":"1.0",
      "options":{
          "TrackingMode": "Fast"
      }
    }

#### <a name="attribute-descriptions"></a>Attributbeskrivningar
| Attributets namn | Beskrivning |
| --- | --- |
| Läge |Snabb bearbetning fast - hastighet, men mindre exakt (standard).|

### <a name="json-output"></a>JSON-utdata
hello följande exempel på JSON-utdata har trunkerats.

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

## <a name="emotion-detection-input-and-output-example"></a>Känsloigenkänning indata och utdata exempel
### <a name="input-video"></a>Indatavideo
[Indatavideo](http://ampdemo.azureedge.net/azuremediaplayer.html?url=https%3A%2F%2Freferencestream-samplestream.streaming.mediaservices.windows.net%2Fc8834d9f-0b49-4b38-bcaf-ece2746f1972%2FMicrosoft%20Convergence%202015%20%20Keynote%20Highlights.ism%2Fmanifest&amp;autoplay=false)

### <a name="task-configuration-preset"></a>Uppgiftskonfigurationen (förinställda)
När du skapar en uppgift med **Azure Media ansikte detektor**, måste du ange en konfiguration förinställning. följande konfiguration förinställda hello anger toocreate JSON baserat på hello känsloigenkänning.

    {
      "version": "1.0",
      "options": {
        "aggregateEmotionWindowMs": "987",
        "mode": "aggregateEmotion",
        "aggregateEmotionIntervalMs": "342"
      }
    }


#### <a name="attribute-descriptions"></a>Attributbeskrivningar
| Attributets namn | Beskrivning |
| --- | --- |
| Läge |Ytor: Inför endast identifiering.<br/>PerFaceEmotion: Returnerar känslo separat för varje ansikte identifiering.<br/>AggregateEmotion: Returnerar genomsnittlig känslo värden för alla ytor i ram. |
| AggregateEmotionWindowMs |Använd om AggregateEmotion läge har valt. Anger hello lång video används tooproduce varje sammanlagda resultat, i millisekunder. |
| AggregateEmotionIntervalMs |Använd om AggregateEmotion läge har valt. Anger med vilken frekvens tooproduce mängd resultat. |

#### <a name="aggregate-defaults"></a>Sammanställd standardvärden
Nedan rekommenderas värden för hello aggregerad fönsterfunktion och inställningar. AggregateEmotionWindowMs bör vara längre än AggregateEmotionIntervalMs.

|| Standardvärden (s) | Min(s) | Max(s) |
|--- | --- | --- | --- |
| AggregateEmotionWindowMs |0.5 |2 |0.25|
| AggregateEmotionIntervalMs |0.5 |1 |0.25|

### <a name="json-output"></a>JSON-utdata
JSON utdata för sammanställd känslo (trunkerad):

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

## <a name="limitations"></a>Begränsningar
* hello stöds video-indataformat inkluderar MP4, MOV och WMV.
* hello är kan upptäckas ansikte storlek 24 x 24 too2048x2048 bildpunkter. kommer inte att identifiera hello ytor utanför det här intervallet.
* Varje video är hello maximalt antal ytor som returnerades 64.
* Vissa ytor kanske inte identifieras på grund av tootechnical utmaningar; t.ex. väldigt ansikte vinklar (head-attityd) och stora är spärrat. Främre och nästan direkt ytor har hello bästa resultat.

## <a name="net-sample-code"></a>Exempelkod för .NET

hello följande program visar hur du:

1. Skapa en tillgång och överför en mediefil till hello tillgång.
2. Skapa ett jobb med en aktivitet för identifiering av står inför baserat på en konfigurationsfil som innehåller följande json förinställda hello. 
   
        {
            "version": "1.0"
        }
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

## <a name="media-services-learning-paths"></a>Sökvägar för Media Services-utbildning
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Ge feedback
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

## <a name="related-links"></a>Relaterade länkar
[Azure Media Services Analytics-översikt](media-services-analytics-overview.md)

[Azure Media Analytics demonstrationer](http://amslabs.azurewebsites.net/demos/Analytics.html)

