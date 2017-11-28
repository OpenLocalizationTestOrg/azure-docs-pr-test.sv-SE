---
title: aaaUse Azure Media Video-miniatyrer tooCreate en Videosammanfattning | Microsoft Docs
description: "Sammanfattning av video kan hjälpa dig skapa sammanfattningar av långa videor automatiskt genom att intressanta kodavsnitt från hello källa video. Detta är användbart när du vill tooprovide en snabb överblick över vilka tooexpect i en lång video."
services: media-services
documentationcenter: 
author: juliako
manager: cfowler
editor: 
ms.assetid: a245529f-3150-4afc-93ec-e40d8a6b761d
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 07/18/2017
ms.author: milanga;juliako;
ms.openlocfilehash: 0a8f0bba6c12a948b940114fe4937e675688a8c7
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="use-azure-media-video-thumbnails-toocreate-a-video-summarization"></a><span data-ttu-id="05310-104">Använd Azure Media Video-miniatyrer tooCreate en Videosammanfattning</span><span class="sxs-lookup"><span data-stu-id="05310-104">Use Azure Media Video Thumbnails tooCreate a Video Summarization</span></span>
## <a name="overview"></a><span data-ttu-id="05310-105">Översikt</span><span class="sxs-lookup"><span data-stu-id="05310-105">Overview</span></span>
<span data-ttu-id="05310-106">Hej **Azure Media Video-miniatyrer** medieprocessor (HP) kan du toocreate en sammanfattning av en video som är användbara toocustomers som bara vill toopreview en sammanfattning av en lång video.</span><span class="sxs-lookup"><span data-stu-id="05310-106">hello **Azure Media Video Thumbnails** media processor (MP) enables you toocreate a summary of a video that is useful toocustomers who just want toopreview a summary of a long video.</span></span> <span data-ttu-id="05310-107">Kunder kan exempelvis vilja toosee en kort ”sammanfattning video” när de hovra över en miniatyrbild.</span><span class="sxs-lookup"><span data-stu-id="05310-107">For example, customers might want toosee a short "summary video" when they hover over a thumbnail.</span></span> <span data-ttu-id="05310-108">Genom att modifiera hello parametrarna för **Azure Media Video-miniatyrer** via en förinställd konfiguration kan du använda hello MP kraftfulla som identifiering och sammanfogning teknik tooalgorithmically generera ett beskrivande underklipp.</span><span class="sxs-lookup"><span data-stu-id="05310-108">By tweaking hello parameters of **Azure Media Video Thumbnails** through a configuration preset, you can use hello MP's powerful shot detection and concatenation technology tooalgorithmically generate a descriptive subclip.</span></span>  

<span data-ttu-id="05310-109">Hej **Azure Media Video miniatyr** MP är för närvarande under förhandsgranskning.</span><span class="sxs-lookup"><span data-stu-id="05310-109">hello **Azure Media Video Thumbnail** MP is currently in Preview.</span></span>

<span data-ttu-id="05310-110">Det här avsnittet innehåller information om **Azure Media Video miniatyr** och visar hur toouse med Media Services SDK för .NET.</span><span class="sxs-lookup"><span data-stu-id="05310-110">This topic gives details about  **Azure Media Video Thumbnail** and shows how toouse it with Media Services SDK for .NET.</span></span>

## <a name="limitations"></a><span data-ttu-id="05310-111">Begränsningar</span><span class="sxs-lookup"><span data-stu-id="05310-111">Limitations</span></span>

<span data-ttu-id="05310-112">I vissa fall, om videon inte består av olika i bakgrunden ska hello utdata en enda som visar.</span><span class="sxs-lookup"><span data-stu-id="05310-112">In some cases, if your video is not comprised of different scenes, hello output will only be a single shot.</span></span>

## <a name="video-summary-example"></a><span data-ttu-id="05310-113">Video sammanfattning exempel</span><span class="sxs-lookup"><span data-stu-id="05310-113">Video summary example</span></span>
<span data-ttu-id="05310-114">Här följer några exempel på vilka hello Azure Media Video-miniatyrer medieprocessor kan göra:</span><span class="sxs-lookup"><span data-stu-id="05310-114">Here are some examples of what hello Azure Media Video Thumbnails media processor can do:</span></span>

### <a name="original-video"></a><span data-ttu-id="05310-115">Ursprungliga video</span><span class="sxs-lookup"><span data-stu-id="05310-115">Original video</span></span>
[<span data-ttu-id="05310-116">Ursprungliga video</span><span class="sxs-lookup"><span data-stu-id="05310-116">Original video</span></span>](http://ampdemo.azureedge.net/azuremediaplayer.html?url=https%3A%2F%2Fnimbuscdn-nimbuspm.streaming.mediaservices.windows.net%2Faed33834-ec2d-4788-88b5-a4505b3d032c%2FMicrosoft%27s%20HoloLens%20Live%20Demonstration.ism%2Fmanifest)

### <a name="video-thumbnail-result"></a><span data-ttu-id="05310-117">Video miniatyr resultat</span><span class="sxs-lookup"><span data-stu-id="05310-117">Video thumbnail result</span></span>
[<span data-ttu-id="05310-118">Video miniatyr resultat</span><span class="sxs-lookup"><span data-stu-id="05310-118">Video thumbnail result</span></span>](http://ampdemo.azureedge.net/azuremediaplayer.html?url=http%3A%2F%2Fnimbuscdn-nimbuspm.streaming.mediaservices.windows.net%2Ff5c91052-4232-41d4-b531-062e07b6a9ae%2FHololens%2520Demo_VideoThumbnails_MotionThumbnail.mp4)

## <a name="task-configuration-preset"></a><span data-ttu-id="05310-119">Uppgiftskonfigurationen (förinställda)</span><span class="sxs-lookup"><span data-stu-id="05310-119">Task configuration (preset)</span></span>
<span data-ttu-id="05310-120">När du skapar en video miniatyr uppgift med **Azure Media Video-miniatyrer**, måste du ange en konfiguration förinställning.</span><span class="sxs-lookup"><span data-stu-id="05310-120">When creating a video thumbnail task with **Azure Media Video Thumbnails**, you must specify a configuration preset.</span></span> <span data-ttu-id="05310-121">hello ovan miniatyr exemplet skapades med hello följande grundläggande JSON-konfiguration:</span><span class="sxs-lookup"><span data-stu-id="05310-121">hello above thumbnail sample was created with hello following basic JSON configuration:</span></span>

    {"version":"1.0"}

<span data-ttu-id="05310-122">För närvarande kan du ändra hello följande parametrar:</span><span class="sxs-lookup"><span data-stu-id="05310-122">Currently, you can change hello following parameters:</span></span>

| <span data-ttu-id="05310-123">Param</span><span class="sxs-lookup"><span data-stu-id="05310-123">Param</span></span> | <span data-ttu-id="05310-124">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="05310-124">Description</span></span> |
| --- | --- |
| <span data-ttu-id="05310-125">outputAudio</span><span class="sxs-lookup"><span data-stu-id="05310-125">outputAudio</span></span> |<span data-ttu-id="05310-126">Anger huruvida hello gällande video innehåller något ljud.</span><span class="sxs-lookup"><span data-stu-id="05310-126">Specifies whether or not hello resultant video contains any audio.</span></span> <br/><span data-ttu-id="05310-127">Tillåtna värden är: SANT eller FALSKT.</span><span class="sxs-lookup"><span data-stu-id="05310-127">Allowed values are: True or False.</span></span> <span data-ttu-id="05310-128">Standardvärdet är True.</span><span class="sxs-lookup"><span data-stu-id="05310-128">Default is True.</span></span> |
| <span data-ttu-id="05310-129">fadeInFadeOut</span><span class="sxs-lookup"><span data-stu-id="05310-129">fadeInFadeOut</span></span> |<span data-ttu-id="05310-130">Anger huruvida Tona övergångar används mellan hello separat rörelse miniatyrerna.</span><span class="sxs-lookup"><span data-stu-id="05310-130">Specifies whether or not fade transitions are used between hello separate motion thumbnails.</span></span>  <br/><span data-ttu-id="05310-131">Tillåtna värden är: SANT eller FALSKT.</span><span class="sxs-lookup"><span data-stu-id="05310-131">Allowed values are: True or False.</span></span>  <span data-ttu-id="05310-132">Standardvärdet är True.</span><span class="sxs-lookup"><span data-stu-id="05310-132">Default is True.</span></span> |
| <span data-ttu-id="05310-133">maxMotionThumbnailDurationInSecs</span><span class="sxs-lookup"><span data-stu-id="05310-133">maxMotionThumbnailDurationInSecs</span></span> |<span data-ttu-id="05310-134">Heltal som anger hur länge hello hela gällande videon vara.</span><span class="sxs-lookup"><span data-stu-id="05310-134">Integer that specifies how long hello entire resultant video shall be.</span></span>  <span data-ttu-id="05310-135">Standard är beroende av ursprungliga video varaktighet.</span><span class="sxs-lookup"><span data-stu-id="05310-135">Default depends on original video duration.</span></span> |

<span data-ttu-id="05310-136">hello följande tabell beskrivs hello standardlängden när **maxMotionThumbnailInSecs** används inte.</span><span class="sxs-lookup"><span data-stu-id="05310-136">hello following table describes hello default duration, when **maxMotionThumbnailInSecs** is not used.</span></span>

|  |  |  |
| --- | --- | --- | --- | --- |
| <span data-ttu-id="05310-137">Video varaktighet</span><span class="sxs-lookup"><span data-stu-id="05310-137">Video duration</span></span> |<span data-ttu-id="05310-138">d < 3 min</span><span class="sxs-lookup"><span data-stu-id="05310-138">d < 3 min</span></span> |<span data-ttu-id="05310-139">3 min < d < 15 min</span><span class="sxs-lookup"><span data-stu-id="05310-139">3 min < d < 15 min</span></span> |
| <span data-ttu-id="05310-140">Miniatyrer varaktighet</span><span class="sxs-lookup"><span data-stu-id="05310-140">Thumbnail duration</span></span> |<span data-ttu-id="05310-141">15 sek (2-3 i bakgrunden)</span><span class="sxs-lookup"><span data-stu-id="05310-141">15 sec (2-3 scenes)</span></span> |<span data-ttu-id="05310-142">30 sekunder (3-5 i bakgrunden)</span><span class="sxs-lookup"><span data-stu-id="05310-142">30 sec (3-5 scenes)</span></span> |

<span data-ttu-id="05310-143">hello anger följande JSON tillgängliga parametrar.</span><span class="sxs-lookup"><span data-stu-id="05310-143">hello following JSON sets available parameters.</span></span>

    {
        "version": "1.0",
        "options": {
            "outputAudio": "true",
            "maxMotionThumbnailDurationInSecs": "10",
            "fadeInFadeOut": "true"
        }
    }

## <a name="net-sample-code"></a><span data-ttu-id="05310-144">Exempelkod för .NET</span><span class="sxs-lookup"><span data-stu-id="05310-144">.NET sample code</span></span>

<span data-ttu-id="05310-145">hello följande program visar hur du:</span><span class="sxs-lookup"><span data-stu-id="05310-145">hello following program shows how to:</span></span>

1. <span data-ttu-id="05310-146">Skapa en tillgång och överför en mediefil till hello tillgång.</span><span class="sxs-lookup"><span data-stu-id="05310-146">Create an asset and upload a media file into hello asset.</span></span>
2. <span data-ttu-id="05310-147">Skapar ett jobb med en video miniatyr aktivitet baserat på en konfigurationsfil som innehåller hello följande json förinställda.</span><span class="sxs-lookup"><span data-stu-id="05310-147">Creates a job with a video thumbnail task based on a configuration file that contains hello following json preset.</span></span> 
   
        {                
            "version": "1.0",
            "options": {
                "outputAudio": "true",
                "maxMotionThumbnailDurationInSecs": "30",
                "fadeInFadeOut": "false"
            }
        }
3. <span data-ttu-id="05310-148">Hämtar hello utdatafilerna.</span><span class="sxs-lookup"><span data-stu-id="05310-148">Downloads hello output files.</span></span> 

#### <a name="create-and-configure-a-visual-studio-project"></a><span data-ttu-id="05310-149">Skapa och konfigurera ett Visual Studio-projekt</span><span class="sxs-lookup"><span data-stu-id="05310-149">Create and configure a Visual Studio project</span></span>

<span data-ttu-id="05310-150">Konfigurera utvecklingsmiljön och fylla hello app.config-fil med anslutningsinformation, enligt beskrivningen i [Media Services-utveckling med .NET](media-services-dotnet-how-to-use.md).</span><span class="sxs-lookup"><span data-stu-id="05310-150">Set up your development environment and populate hello app.config file with connection information, as described in [Media Services development with .NET](media-services-dotnet-how-to-use.md).</span></span> 

#### <a name="example"></a><span data-ttu-id="05310-151">Exempel</span><span class="sxs-lookup"><span data-stu-id="05310-151">Example</span></span>

    using System;
    using System.Configuration;
    using System.IO;
    using System.Linq;
    using Microsoft.WindowsAzure.MediaServices.Client;
    using System.Threading;
    using System.Threading.Tasks;

    namespace VideoSummarization
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


                // Run hello thumbnail job.
                var asset = RunVideoThumbnailJob(@"C:\supportFiles\VideoThumbnail\BigBuckBunny.mp4",
                                            @"C:\supportFiles\VideoThumbnail\config.json");

                // Download hello job output asset.
                DownloadAsset(asset, @"C:\supportFiles\VideoThumbnail\Output");
            }

            static IAsset RunVideoThumbnailJob(string inputMediaFilePath, string configurationFile)
            {
                // Create an asset and upload hello input media file toostorage.
                IAsset asset = CreateAssetAndUploadSingleFile(inputMediaFilePath,
                    "My Video Thumbnail Input Asset",
                    AssetCreationOptions.None);

                // Declare a new job.
                IJob job = _context.Jobs.Create("My Video Thumbnail Job");

                // Get a reference tooAzure Media Video Thumbnails.
                string MediaProcessorName = "Azure Media Video Thumbnails";

                var processor = GetLatestMediaProcessorByName(MediaProcessorName);

                // Read configuration from hello specified file.
                string configuration = File.ReadAllText(configurationFile);

                // Create a task with hello encoding details, using a string preset.
                ITask task = job.Tasks.AddNew("My Video Thumbnail Task",
                    processor,
                    configuration,
                    TaskOptions.None);

                // Specify hello input asset.
                task.InputAssets.Add(asset);

                // Add an output asset toocontain hello results of hello job.
                task.OutputAssets.AddNew("My Video Thumbnail Output Asset", AssetCreationOptions.None);

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

### <a name="video-thumbnail-output"></a><span data-ttu-id="05310-152">Miniatyrer videoutgång</span><span class="sxs-lookup"><span data-stu-id="05310-152">Video thumbnail output</span></span>
[<span data-ttu-id="05310-153">Miniatyrer videoutgång</span><span class="sxs-lookup"><span data-stu-id="05310-153">Video thumbnail output</span></span>](http://ampdemo.azureedge.net/azuremediaplayer.html?url=http%3A%2F%2Fnimbuscdn-nimbuspm.streaming.mediaservices.windows.net%2Fd06f24dc-bc81-488e-a8d0-348b7dc41b56%2FHololens%2520Demo_VideoThumbnails_MotionThumbnail.mp4)

## <a name="media-services-learning-paths"></a><span data-ttu-id="05310-154">Sökvägar för Media Services-utbildning</span><span class="sxs-lookup"><span data-stu-id="05310-154">Media Services learning paths</span></span>
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a><span data-ttu-id="05310-155">Ge feedback</span><span class="sxs-lookup"><span data-stu-id="05310-155">Provide feedback</span></span>
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

## <a name="related-links"></a><span data-ttu-id="05310-156">Relaterade länkar</span><span class="sxs-lookup"><span data-stu-id="05310-156">Related links</span></span>
[<span data-ttu-id="05310-157">Azure Media Services Analytics-översikt</span><span class="sxs-lookup"><span data-stu-id="05310-157">Azure Media Services Analytics Overview</span></span>](media-services-analytics-overview.md)

[<span data-ttu-id="05310-158">Azure Media Analytics demonstrationer</span><span class="sxs-lookup"><span data-stu-id="05310-158">Azure Media Analytics demos</span></span>](http://azuremedialabs.azurewebsites.net/demos/Analytics.html)

