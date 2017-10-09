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
# <a name="use-azure-media-analytics-tooconvert-text-content-in-video-files-into-digital-text"></a>Använd Azure Media Analytics tooconvert textinnehåll i videofiler till digitala text
## <a name="overview"></a>Översikt
Om du behöver tooextract text innehåll från din videofiler och generera en redigerbar sökbara digitala text, använder du Azure Media Analytics OCR (OCR). Denna Azure Media Processor identifierar textinnehåll i din videofiler och genererar textfiler för din användning. OCR kan du tooautomate hello extrahering av beskrivande metadata från hello video signal medieinnehåll.

När den används tillsammans med en sökmotor, kan du enkelt indexera media med text och förbättra hello identifieringsmöjligheten för ditt innehåll. Detta är mycket användbart i hög textrepresentation video, t.ex. en inspelning eller skärmdump av ett bildspel. hello Azure OCR Medieprocessor är optimerad för digital text.

Hej **Azure Media OCR** medieprocessor är för närvarande under förhandsgranskning.

Det här avsnittet innehåller information om **Azure Media OCR** och visar hur toouse med Media Services SDK för .NET. Ytterligare information och exempel finns [bloggen](https://azure.microsoft.com/blog/announcing-video-ocr-public-preview-new-config/).

## <a name="ocr-input-files"></a>OCR inkommande filer
Videofiler. För närvarande hello följande format stöds: MP4, MOV och WMV.

## <a name="task-configuration"></a>Konfigurera aktivitet
Uppgiftskonfigurationen (förinställda). När du skapar en uppgift med **Azure Media OCR**, måste du ange en konfiguration som förinställningen med hjälp av JSON eller XML. 

>[!NOTE]
>hello OCR-motorn tar bara en bild region med minsta 40 bildpunkter toomaximum 32000 bildpunkter som en giltig inmatning i både höjd och bredd.
>

### <a name="attribute-descriptions"></a>Attributbeskrivningar
| Attributets namn | Beskrivning |
| --- | --- |
|AdvancedOutput| Om du ställer in AdvancedOutput tootrue innehåller hello JSON-utdata positionsparametrarna data för varje ord (i tillägget toophrases och regioner). Om du inte vill toosee detaljerna, ange hello flaggan toofalse. hello standardvärdet är false. Mer information finns i [bloggen](https://azure.microsoft.com/blog/azure-media-ocr-simplified-output/).|
| Språk |(valfritt) beskriver hello språk för vilka toolook. En av följande hello: AutoDetect (standard), arabiska, ChineseSimplified, ChineseTraditional, tjeckiska, danska, nederländska, engelska, finska, franska, tyska, grekiska, ungerska, italienska, japanska, koreanska, norska, polska, portugisiska, rumänska, ryska, SerbianCyrillic, SerbianLatin, slovakiska, spanska, svenska, turkiska. |
| TextOrientation |(valfritt) beskriver hello orientering för vilka toolook.  ”Vänstra” innebär att hello överkant versaler pekar mot hello vänster.  Standardtexten (till exempel den som finns i en bok) kan anropas ”in” orienterad.  En av följande hello: AutoDetect (standard), höger, nedåt, vänster. |
| TimeInterval |(valfritt) beskriver hello samplingsfrekvensen.  Standardvärdet är varje 1/2 sekund.<br/>JSON-format –: mm: ss. SSS (standard 00:00:00.500)<br/>XML-format – W3C XSD varaktighet primitiv (standard PT0.5) |
| DetectRegions |(valfritt) En matris med DetectRegion objekt att ange regioner inom hello bildruta i vilken toodetect text.<br/>Ett DetectRegion objekt består av följande fyra heltalsvärden hello:<br/>Vänster – bildpunkter från hello vänster marginal<br/>TOP – bildpunkter från hello top-marginal<br/>Bredd – bredden på hello region i bildpunkter<br/>Höjd – höjd hello region i bildpunkter |

#### <a name="json-preset-example"></a>Förinställda JSON-exempel

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


#### <a name="xml-preset-example"></a>Förinställda XML-exempel
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

## <a name="ocr-output-files"></a>OCR utdatafilerna
hello utdata hello OCR medium processorkonfiguration är en JSON-fil.

### <a name="elements-of-hello-output-json-file"></a>Element i hello utdata-JSON-fil
hello Video OCR utdata innehåller tid-segmenterade data om hello tecken hittades i videon.  Du kan använda attribut, till exempel språk eller orientering toohone i på exakt hello ord att du är intresserad av att analysera. 

hello utdata innehåller hello följande attribut:

| Element | Beskrivning |
| --- | --- |
| tidsrymd |”tick” per sekund av hello video |
| Offset |tidsförskjutningen för tidsstämplar. I version 1.0 av API: er för Video att detta alltid vara 0. |
| ramhastighet |Bildrutor per sekund av hello video |
| Bredd |bredden på hello video i bildpunkter |
| Höjd |höjden på hello video i bildpunkter |
| fragment |matris med tidsbaserade segment i vilka hello metadata chunked-video |
| start |Starttid för ett fragment i ”tick” |
| Varaktighet |längden på ett fragment i ”tick” |
| interval |intervallet för varje händelse inom hello angivna fragment |
| evenemang |matris som innehåller regioner |
| Region |objektet som representerar upptäckte ord eller fraser |
| språk |språket hello texten identifieras inom en region |
| orientering |textens hello identifieras inom en region orientering |
| rader |matris med rader med text som har identifierats inom ett område |
| Text |hello faktiska text |

### <a name="json-output-example"></a>Exempel på utdata JSON
hello innehåller följande exempel på utdata hello allmän video information och flera video fragment. I varje video fragmentet innehåller den varje region som identifieras av OCR MP med hello språk och dess textorientering. hello region innehåller även alla word rader i den här regionen med hello rad text, hello radpositionen och varje ord information (ordet innehåll, position och förtroende) i den här raden. hello följande är ett exempel och du placerar vissa infogade kommentarer.

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

## <a name="net-sample-code"></a>Exempelkod för .NET

hello följande program visar hur du:

1. Skapa en tillgång och överför en mediefil till hello tillgång.
2. Skapa ett jobb med en konfiguration/förinställda OCR-fil.
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

## <a name="media-services-learning-paths"></a>Sökvägar för Media Services-utbildning
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Ge feedback
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

## <a name="related-links"></a>Relaterade länkar
[Azure Media Services Analytics-översikt](media-services-analytics-overview.md)

