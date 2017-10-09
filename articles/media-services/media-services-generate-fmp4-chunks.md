---
title: aaaCreate ett Azure Media Services encoding-aktivitet som genererar fMP4 segment | Microsoft Docs
description: "Det här avsnittet visar hur toocreate en kodning uppgift som genererar fMP4 blocken. När denna uppgift används med Media Encoder Standard hello eller arbetsflödet för Media Encoder Premium-kodare innehåller hello utdatatillgången fMP4 segment i stället för ISO MP4-filer."
services: media-services
documentationcenter: 
author: juliako
manager: cfowler
editor: 
ms.assetid: b7029ac5-eadd-4a2f-8111-1fc460828981
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/18/2017
ms.author: juliako
ms.openlocfilehash: 388f3ccb9865b5c4e159af86d5a9ee2f4e3f6120
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
#  <a name="create-an-encoding-task-that-generates-fmp4-chunks"></a><span data-ttu-id="97c71-104">Skapa en kodning uppgift som genererar fMP4 segment</span><span class="sxs-lookup"><span data-stu-id="97c71-104">Create an encoding task that generates fMP4 chunks</span></span>

## <a name="overview"></a><span data-ttu-id="97c71-105">Översikt</span><span class="sxs-lookup"><span data-stu-id="97c71-105">Overview</span></span>

<span data-ttu-id="97c71-106">Det här avsnittet visar hur toocreate en kodning uppgift som genererar fragmenterad MP4 (fMP4) segment i stället för ISO MP4-filer.</span><span class="sxs-lookup"><span data-stu-id="97c71-106">This topic shows how toocreate an encoding task that generates fragmented MP4 (fMP4) chunks instead of ISO MP4 files.</span></span> <span data-ttu-id="97c71-107">toogenerate fMP4 segment använder hello **Media Encoder Standard** eller **Media Encoder Premium arbetsflöde** kodare toocreate kodning uppgift och även ange  **AssetFormatOption.AdaptiveStreaming** alternativ, som visas i det här kodstycket:</span><span class="sxs-lookup"><span data-stu-id="97c71-107">toogenerate fMP4 chunks, use hello **Media Encoder Standard** or **Media Encoder Premium Workflow** encoder toocreate an encoding task and also specify **AssetFormatOption.AdaptiveStreaming** option, as shown in this code snippet:</span></span>  
    
    task.OutputAssets.AddNew(@"Output Asset containing fMP4 chunks", 
            options: AssetCreationOptions.None, 
            formatOption: AssetFormatOption.AdaptiveStreaming);


## <span data-ttu-id="97c71-108"><a id="encoding_with_dotnet"></a>Encoding med Media Services .NET SDK</span><span class="sxs-lookup"><span data-stu-id="97c71-108"><a id="encoding_with_dotnet"></a>Encoding with Media Services .NET SDK</span></span>

<span data-ttu-id="97c71-109">Följande kodexempel hello använder Media Services .NET SDK tooperform hello följande uppgifter:</span><span class="sxs-lookup"><span data-stu-id="97c71-109">hello following code example uses Media Services .NET SDK tooperform hello following tasks:</span></span>

- <span data-ttu-id="97c71-110">Skapa ett kodningsjobb.</span><span class="sxs-lookup"><span data-stu-id="97c71-110">Create an encoding job.</span></span>
- <span data-ttu-id="97c71-111">Hämta en referens toohello **Media Encoder Standard** kodare.</span><span class="sxs-lookup"><span data-stu-id="97c71-111">Get a reference toohello **Media Encoder Standard** encoder.</span></span>
- <span data-ttu-id="97c71-112">Lägg till en aktivitet toohello kodningsjobbet och ange toouse hello **anpassningsbar strömning** förinställda.</span><span class="sxs-lookup"><span data-stu-id="97c71-112">Add an encoding task toohello job and specify toouse hello **Adaptive Streaming** preset.</span></span> 
- <span data-ttu-id="97c71-113">Skapa en utdata tillgång som innehåller fMP4 segment och en .ism-fil.</span><span class="sxs-lookup"><span data-stu-id="97c71-113">Create an output asset that will contain fMP4 chunks and an .ism file.</span></span>
- <span data-ttu-id="97c71-114">Lägg till en händelse hanteraren toocheck hello jobb pågår.</span><span class="sxs-lookup"><span data-stu-id="97c71-114">Add an event handler toocheck hello job progress.</span></span>
- <span data-ttu-id="97c71-115">Skicka hello-jobbet.</span><span class="sxs-lookup"><span data-stu-id="97c71-115">Submit hello job.</span></span>

#### <a name="create-and-configure-a-visual-studio-project"></a><span data-ttu-id="97c71-116">Skapa och konfigurera ett Visual Studio-projekt</span><span class="sxs-lookup"><span data-stu-id="97c71-116">Create and configure a Visual Studio project</span></span>

<span data-ttu-id="97c71-117">Konfigurera utvecklingsmiljön och fylla hello app.config-fil med anslutningsinformation, enligt beskrivningen i [Media Services-utveckling med .NET](media-services-dotnet-how-to-use.md).</span><span class="sxs-lookup"><span data-stu-id="97c71-117">Set up your development environment and populate hello app.config file with connection information, as described in [Media Services development with .NET](media-services-dotnet-how-to-use.md).</span></span> 

#### <a name="example"></a><span data-ttu-id="97c71-118">Exempel</span><span class="sxs-lookup"><span data-stu-id="97c71-118">Example</span></span>

    using System;
    using System.Configuration;
    using System.Linq;
    using Microsoft.WindowsAzure.MediaServices.Client;
    using System.Threading;

    namespace AdaptiveStreaming
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

            // Get an uploaded asset.
            var asset = _context.Assets.FirstOrDefault();

            // Encode and generate hello output using hello "Adaptive Streaming" preset.
            EncodeToAdaptiveBitrateMP4Set(asset);

            Console.ReadLine();
        }
        static public IAsset EncodeToAdaptiveBitrateMP4Set(IAsset asset)
        {
            // Declare a new job.
            IJob job = _context.Jobs.Create("Media Encoder Standard Job");

            // Get a media processor reference, and pass tooit hello name of hello 
            // processor toouse for hello specific task.
            IMediaProcessor processor = GetLatestMediaProcessorByName("Media Encoder Standard");

            // Create a task
            ITask task = job.Tasks.AddNew("Media Encoder Standard encoding task",
            processor,
            "Adaptive Streaming",
            TaskOptions.None);

            // Specify hello input asset toobe encoded.
            task.InputAssets.Add(asset);

            // Add an output asset toocontain hello results of hello job. 

            // This output is specified as AssetCreationOptions.None, which 
            // means hello output asset is not encrypted. 
            // It is also specified toouse AssetFormatOption.AdaptiveStreaming, 
            // which means hello output asset will contain fMP4 chunks.

            task.OutputAssets.AddNew(@"Output Asset containing fMP4 chunks",
            options: AssetCreationOptions.None,
            formatOption: AssetFormatOption.AdaptiveStreaming);

            job.StateChanged += new EventHandler<JobStateChangedEventArgs>(JobStateChanged);
            job.Submit();
            job.GetExecutionProgressTask(CancellationToken.None).Wait();

            return job.OutputMediaAssets[0];
        }
        private static void JobStateChanged(object sender, JobStateChangedEventArgs e)
        {
            Console.WriteLine("Job state changed event:");
            Console.WriteLine("  Previous state: " + e.PreviousState);
            Console.WriteLine("  Current state: " + e.CurrentState);
            switch (e.CurrentState)
            {
            case JobState.Finished:
                Console.WriteLine();
                Console.WriteLine("Job is finished. Please wait while local tasks or downloads complete...");
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
                break;
            default:
                break;
            }
        }
        private static IMediaProcessor GetLatestMediaProcessorByName(string mediaProcessorName)
        {
            var processor = _context.MediaProcessors.Where(p => p.Name == mediaProcessorName).
            ToList().OrderBy(p => new Version(p.Version)).LastOrDefault();

            if (processor == null)
            throw new ArgumentException(string.Format("Unknown media processor", mediaProcessorName));

            return processor;
        }
        }
    }

## <a name="media-services-learning-paths"></a><span data-ttu-id="97c71-119">Sökvägar för Media Services-utbildning</span><span class="sxs-lookup"><span data-stu-id="97c71-119">Media Services learning paths</span></span>
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a><span data-ttu-id="97c71-120">Ge feedback</span><span class="sxs-lookup"><span data-stu-id="97c71-120">Provide feedback</span></span>
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

## <a name="see-also"></a><span data-ttu-id="97c71-121">Se även</span><span class="sxs-lookup"><span data-stu-id="97c71-121">See Also</span></span>
[<span data-ttu-id="97c71-122">Media Services Encoding översikt</span><span class="sxs-lookup"><span data-stu-id="97c71-122">Media Services Encoding Overview</span></span>](media-services-encode-asset.md)

