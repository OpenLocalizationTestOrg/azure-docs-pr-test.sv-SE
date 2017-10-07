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
# <a name="detect-motions-with-azure-media-analytics"></a>Identifiera rörelser med Azure Media Analytics
## <a name="overview"></a>Översikt
Hej **Azure Media rörelsedetektor** media-processor (HP) aktiverar du tooefficiently identifierar avsnitt av intresse i ett annat långa och primärdomänkontrollant video. Rörelseidentifiering kan användas på statisk kamera tagning tooidentify avsnitt av hello video där rörelse inträffar. Den genererar en JSON-fil som innehåller en metadata med tidsstämplar och hello avgränsar region där hello händelsen inträffade.

Den här tekniken är riktade mot säkerhet video feeds kan toocategorize rörelse in relevanta händelser och falska positiva identifieringar som skuggor och andra ändringar. Detta ger dig toogenerate säkerhetsaviseringar från kameran feeds utan som skräppost med oändlig irrelevanta händelser inte kan tooextract stund intressanta från extremt långa övervakning videor.

Hej **Azure Media rörelsedetektor** MP är för närvarande under förhandsgranskning.

Det här avsnittet innehåller information om **Azure Media rörelsedetektor** och visar hur toouse med Media Services SDK för .NET

## <a name="motion-detector-input-files"></a>Rörelse detektor inkommande filer
Videofiler. För närvarande hello följande format stöds: MP4, MOV och WMV.

## <a name="task-configuration-preset"></a>Uppgiftskonfigurationen (förinställda)
När du skapar en uppgift med **Azure Media rörelsedetektor**, måste du ange en konfiguration förinställning. 

### <a name="parameters"></a>Parametrar
Du kan använda hello följande parametrar:

| Namn | Alternativ | Beskrivning | Standard |
| --- | --- | --- | --- |
| sensitivityLevel |Sträng: låg, 'medium', 'hög' |Anger hello känslighet på vilka rörelser rapporteras. Justera det här tooadjust antalet falska positiva identifieringar. |-medium' |
| frameSamplingValue |Positivt heltal |Anger hello frekvens vid vilken algoritm körs. 1 är lika med varje ram, 2: var 2 ram och så vidare. |1 |
| detectLightChange |Booleskt: 'true', 'false' |Anger om lätta ändringar rapporteras i hello resultat |'False' |
| mergeTimeThreshold |Tid för xs:: Mm: ss<br/>Exempel: 00:00:03 |Anger hello tidsfönstret mellan rörelse händelser där 2 händelser kombineras och rapporteras som 1. |00:00:00 |
| detectionZones |En matris med identifiering zoner:<br/>-Identifiering zonen är en matris med 3 eller fler punkter<br/>-Platsen är en x och y koordinaten från 0 too1. |Beskriver hello lista över identifiering av polygona zoner toobe används.<br/>Resultatet rapporteras med hello zoner som ett-ID och hello först en vara ”id”: 0 |Zon som omfattar hela hello-ramen. |

### <a name="json-example"></a>JSON-exempel
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


## <a name="motion-detector-output-files"></a>Videofiler detektor utdata
Ett jobb för identifiering av rörelse returneras en JSON-fil i hello utdatatillgången som beskriver hello rörelse aviseringar och deras kategorier inom hello video. hello-filen innehåller information om hello och varaktighet för rörelse upptäcktes i hello video.

hello rörelse detektor API innehåller indikatorer när det finns objekt i rörelse i en fast bakgrund video (t.ex. övervakning av video). hello rörelsedetektor är utbildade tooreduce falsklarm, till exempel ljus och shadow ändringar. Aktuella begränsningar för hello algoritmer är natt vision videor, delvis transparenta objekt och små objekt.

### <a id="output_elements"></a>Element i hello utdata-JSON-fil
> [!NOTE]
> I hello senaste versionen, hello utdata-JSON-format har ändrats och kan utgöra en ny ändring för vissa kunder.
> 
> 

hello beskrivs följande tabell element i hello utdata-JSON-fil.

| Element | Beskrivning |
| --- | --- |
| Version |Detta refererar toohello hello Video API-version. hello aktuella versionen är 2. |
| tidsrymd |”Tick” per sekund av hello video. |
| Offset |hello tidsförskjutningen för tidsstämplar i ”tick”. I version 1.0 av API: er för Video att detta alltid vara 0. I framtiden scenarier som vi stöder det här värdet kan ändras. |
| ramhastighet |Bildrutor per sekund av hello video. |
| Bredd, höjd |Refererar toohello bredd och höjd hello video i bildpunkter. |
| Start |hello starta tidsstämpeln i ”tick”. |
| Varaktighet |hello längd hello händelse i ”tick”. |
| intervall |hello-intervall för varje post i hello händelse i ”tick”. |
| Händelser |Varje händelse fragment innehåller hello rörelse identifieras inom den varaktigheten. |
| Typ |I hello aktuell version är det alltid '2' för allmän rörelse. Den här etiketten ger API: er för Video hello flexibilitet toocategorize rörelse i framtida versioner. |
| RegionID |Detta kommer alltid att 0 i den här versionen som beskrivs ovan. Den här etiketten ger Video API hello flexibilitet toofind rörelse i olika områden i framtida versioner. |
| Regioner |Refererar toohello område i videon där du är intresserad rörelse. <br/><br/>-”id” representerar hello region område – i den här versionen finns bara en, ID 0. <br/>-”typ” representerar hello form av hello region som intresserar dig för rörelse. För närvarande stöds ”rektangel” och ”polygon”.<br/> Om du har angett ”rektangel” hello region har mått i X, Y, bredd och höjd. hello X och Y-koordinaterna representerar hello övre vänstra Xyserver koordinaterna för hello region i 0.0 too1.0 normaliserade skalan. representerar hello storleken på hello region i 0.0 too1.0 normaliserade skalan hello bredd och höjd. I hello aktuella versionen kan korrigeras alltid X, Y, bredd och höjd på 0, 0 och 1, 1. <br/>Om du har angett ”polygon” har hello region dimensioner i punkter. <br/> |
| fragment |hello metadata chunked upp till olika segment som kallas fragment. Varje avsnitt innehåller en start, varaktighet, antalet och händelse(r). Ett fragment med inga händelser innebär att inga rörelse identifierades under den starttid och varaktighet. |
| Hakparenteserna] |Varje hakparentes representerar ett intervall i hello-händelse. Tomma parenteser för intervallet innebär att inga rörelse upptäcktes. |
| Platser |Den nya posten under händelser visar hello plats där hello rörelse inträffade. Det här är mer specifik än hello identifiering zoner. |

hello följande är exempel på en JSON-utdata

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
## <a name="limitations"></a>Begränsningar
* hello stöds video-indataformat inkluderar MP4, MOV och WMV.
* Rörelseidentifiering är optimerad för stilla bakgrund videor. hello algoritmen fokuserar på att minska falsklarm, till exempel ljusförändringar och skuggor.
* Vissa rörelse kanske inte identifieras på grund av tootechnical utmaningar; t.ex. natt vision videor, delvis transparenta objekt och små objekt.

## <a name="net-sample-code"></a>Exempelkod för .NET

hello följande program visar hur du:

1. Skapa en tillgång och överför en mediefil till hello tillgång.
2. Skapa ett jobb med en aktivitet för identifiering av video rörelse baserat på en konfigurationsfil som innehåller hello följande json förinställda. 
   
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

## <a name="media-services-learning-paths"></a>Sökvägar för Media Services-utbildning
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Ge feedback
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

## <a name="related-links"></a>Relaterade länkar
[Azure Media Services rörelsedetektor blogg](https://azure.microsoft.com/blog/motion-detector-update/)

[Azure Media Services Analytics-översikt](media-services-analytics-overview.md)

[Azure Media Analytics demonstrationer](http://azuremedialabs.azurewebsites.net/demos/Analytics.html)

