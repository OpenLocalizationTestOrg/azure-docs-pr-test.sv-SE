---
title: "aaaCustomizing Media Encoder Standard förinställningar | Microsoft Docs"
description: "Det här avsnittet visar hur tooperform avancerade kodning genom att anpassa Media Encoder Standard uppgiften förinställningar. hello avsnittet visas hur toouse Media Services .NET SDK toocreate kodning uppgift och jobbet. Den visar även hur toosupply anpassade förinställningar toohello kodningsjobbet."
services: media-services
documentationcenter: 
author: juliako
manager: cfowler
editor: 
ms.assetid: ec95392f-d34a-4c22-a6df-5274eaac445b
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/17/2017
ms.author: juliako
ms.openlocfilehash: fa8c3bef63b0c1ecc88a6b8874ecbff3a8028a57
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="customizing-media-encoder-standard-presets"></a><span data-ttu-id="efa03-105">Anpassa Media Encoder Standard förinställningar</span><span class="sxs-lookup"><span data-stu-id="efa03-105">Customizing Media Encoder Standard presets</span></span>

## <a name="overview"></a><span data-ttu-id="efa03-106">Översikt</span><span class="sxs-lookup"><span data-stu-id="efa03-106">Overview</span></span>

<span data-ttu-id="efa03-107">Det här avsnittet visar hur tooperform avancerade encoding med Media Encoder Standard (MES) med hjälp av en anpassad förinställning.</span><span class="sxs-lookup"><span data-stu-id="efa03-107">This topic shows how tooperform advanced encoding with Media Encoder Standard (MES) using a custom preset.</span></span> <span data-ttu-id="efa03-108">hello avsnittet använder .NET toocreate en kodning uppgift och ett jobb som utför den här uppgiften.</span><span class="sxs-lookup"><span data-stu-id="efa03-108">hello topic uses .NET toocreate an encoding task and a job that executes this task.</span></span>  

<span data-ttu-id="efa03-109">I det här avsnittet visas hur toocustomize en förinställning genom att göra hello [H264 Multibithastighet 720p](media-services-mes-preset-H264-Multiple-Bitrate-720p.md) hello förinställda och minska antalet lager.</span><span class="sxs-lookup"><span data-stu-id="efa03-109">In this topic you will see how toocustomize a preset by taking hello [H264 Multiple Bitrate 720p](media-services-mes-preset-H264-Multiple-Bitrate-720p.md) preset and reducing hello number of layers.</span></span> <span data-ttu-id="efa03-110">Hej [Anpassa Media Encoder Standard förinställningar](media-services-advanced-encoding-with-mes.md) avsnittet visar anpassade förinställningar som kan använda tooperform avancerade kodning uppgifter.</span><span class="sxs-lookup"><span data-stu-id="efa03-110">hello [Customizing Media Encoder Standard presets](media-services-advanced-encoding-with-mes.md) topic demonstrates custom presets that can be used tooperform advanced encoding tasks.</span></span>

## <span data-ttu-id="efa03-111"><a id="customizing_presets"></a>Anpassa en MES förinställning</span><span class="sxs-lookup"><span data-stu-id="efa03-111"><a id="customizing_presets"></a> Customizing a MES preset</span></span>

### <a name="original-preset"></a><span data-ttu-id="efa03-112">Ursprungliga förinställda</span><span class="sxs-lookup"><span data-stu-id="efa03-112">Original preset</span></span>

<span data-ttu-id="efa03-113">Spara hello JSON som definierats i hello [H264 Multibithastighet 720p](media-services-mes-preset-H264-Multiple-Bitrate-720p.md) -avsnittet i en fil med JSON.</span><span class="sxs-lookup"><span data-stu-id="efa03-113">Save hello JSON defined in hello [H264 Multiple Bitrate 720p](media-services-mes-preset-H264-Multiple-Bitrate-720p.md) topic in some file with .json extension.</span></span> <span data-ttu-id="efa03-114">Till exempel **CustomPreset_JSON.json**.</span><span class="sxs-lookup"><span data-stu-id="efa03-114">For example, **CustomPreset_JSON.json**.</span></span>

### <a name="customized-preset"></a><span data-ttu-id="efa03-115">Anpassade förinställda</span><span class="sxs-lookup"><span data-stu-id="efa03-115">Customized preset</span></span>

<span data-ttu-id="efa03-116">Öppna hello **CustomPreset_JSON.json** filen och ta bort först tre lager från **H264Layers** så filen ser ut så här.</span><span class="sxs-lookup"><span data-stu-id="efa03-116">Open hello **CustomPreset_JSON.json** file and remove first three layers from **H264Layers** so your file looks like this.</span></span>

    
    {  
      "Version": 1.0,  
      "Codecs": [  
        {  
          "KeyFrameInterval": "00:00:02",  
          "H264Layers": [  
            {  
              "Profile": "Auto",  
              "Level": "auto",  
              "Bitrate": 1000,  
              "MaxBitrate": 1000,  
              "BufferWindow": "00:00:05",  
              "Width": 640,  
              "Height": 360,  
              "BFrames": 3,  
              "ReferenceFrames": 3,  
              "AdaptiveBFrame": true,  
              "Type": "H264Layer",  
              "FrameRate": "0/1"  
            },  
            {  
              "Profile": "Auto",  
              "Level": "auto",  
              "Bitrate": 650,  
              "MaxBitrate": 650,  
              "BufferWindow": "00:00:05",  
              "Width": 640,  
              "Height": 360,  
              "BFrames": 3,  
              "ReferenceFrames": 3,  
              "AdaptiveBFrame": true,  
              "Type": "H264Layer",  
              "FrameRate": "0/1"  
            },  
            {  
              "Profile": "Auto",  
              "Level": "auto",  
              "Bitrate": 400,  
              "MaxBitrate": 400,  
              "BufferWindow": "00:00:05",  
              "Width": 320,  
              "Height": 180,  
              "BFrames": 3,  
              "ReferenceFrames": 3,  
              "AdaptiveBFrame": true,  
              "Type": "H264Layer",  
              "FrameRate": "0/1"  
            }  
          ],  
          "Type": "H264Video"  
        },  
        {  
          "Profile": "AACLC",  
          "Channels": 2,  
          "SamplingRate": 48000,  
          "Bitrate": 128,  
          "Type": "AACAudio"  
        }  
      ],  
      "Outputs": [  
        {  
          "FileName": "{Basename}_{Width}x{Height}_{VideoBitrate}.mp4",  
          "Format": {  
            "Type": "MP4Format"  
          }  
        }  
      ]  
    }  
    

## <span data-ttu-id="efa03-117"><a id="encoding_with_dotnet"></a>Encoding med Media Services .NET SDK</span><span class="sxs-lookup"><span data-stu-id="efa03-117"><a id="encoding_with_dotnet"></a>Encoding with Media Services .NET SDK</span></span>

<span data-ttu-id="efa03-118">Följande kodexempel hello använder Media Services .NET SDK tooperform hello följande uppgifter:</span><span class="sxs-lookup"><span data-stu-id="efa03-118">hello following code example uses Media Services .NET SDK tooperform hello following tasks:</span></span>

- <span data-ttu-id="efa03-119">Skapa ett kodningsjobb.</span><span class="sxs-lookup"><span data-stu-id="efa03-119">Create an encoding job.</span></span>
- <span data-ttu-id="efa03-120">Hämta en referens toohello Media Encoder Standard-kodare.</span><span class="sxs-lookup"><span data-stu-id="efa03-120">Get a reference toohello Media Encoder Standard encoder.</span></span>
- <span data-ttu-id="efa03-121">Läsa in hello anpassade JSON förinställningen som du skapade i föregående avsnitt i hello.</span><span class="sxs-lookup"><span data-stu-id="efa03-121">Load hello custom JSON preset that you created in hello previous section.</span></span> 
  
        // Load hello JSON from hello local file.
        string configuration = File.ReadAllText(fileName);  

- <span data-ttu-id="efa03-122">Lägg till ett kodningsjobb i aktiviteten toohello.</span><span class="sxs-lookup"><span data-stu-id="efa03-122">Add an encoding task toohello job.</span></span> 
- <span data-ttu-id="efa03-123">Ange hello indata tillgången toobe kodad.</span><span class="sxs-lookup"><span data-stu-id="efa03-123">Specify hello input asset toobe encoded.</span></span>
- <span data-ttu-id="efa03-124">Skapa en utdata tillgång som innehåller hello kodade tillgången.</span><span class="sxs-lookup"><span data-stu-id="efa03-124">Create an output asset that will contain hello encoded asset.</span></span>
- <span data-ttu-id="efa03-125">Lägg till en händelse hanteraren toocheck hello jobb pågår.</span><span class="sxs-lookup"><span data-stu-id="efa03-125">Add an event handler toocheck hello job progress.</span></span>
- <span data-ttu-id="efa03-126">Skicka hello-jobbet.</span><span class="sxs-lookup"><span data-stu-id="efa03-126">Submit hello job.</span></span>
   
#### <a name="create-and-configure-a-visual-studio-project"></a><span data-ttu-id="efa03-127">Skapa och konfigurera ett Visual Studio-projekt</span><span class="sxs-lookup"><span data-stu-id="efa03-127">Create and configure a Visual Studio project</span></span>

<span data-ttu-id="efa03-128">Konfigurera utvecklingsmiljön och fylla hello app.config-fil med anslutningsinformation, enligt beskrivningen i [Media Services-utveckling med .NET](media-services-dotnet-how-to-use.md).</span><span class="sxs-lookup"><span data-stu-id="efa03-128">Set up your development environment and populate hello app.config file with connection information, as described in [Media Services development with .NET](media-services-dotnet-how-to-use.md).</span></span> 

#### <a name="example"></a><span data-ttu-id="efa03-129">Exempel</span><span class="sxs-lookup"><span data-stu-id="efa03-129">Example</span></span>   

    using System;
    using System.Configuration;
    using System.IO;
    using System.Linq;
    using Microsoft.WindowsAzure.MediaServices.Client;
    using System.Threading;

    namespace CustomizeMESPresests
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

        private static readonly string _mediaFiles =
            Path.GetFullPath(@"../..\Media");

        private static readonly string _singleMP4File =
            Path.Combine(_mediaFiles, @"BigBuckBunny.mp4");

        static void Main(string[] args)
        {
            var tokenCredentials = new AzureAdTokenCredentials(_AADTenantDomain, AzureEnvironments.AzureCloudEnvironment);
            var tokenProvider = new AzureAdTokenProvider(tokenCredentials);

            _context = new CloudMediaContext(new Uri(_RESTAPIEndpoint), tokenProvider);

            // Get an uploaded asset.
            var asset = _context.Assets.FirstOrDefault();

            // Encode and generate hello output using custom presets.
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

            // Load hello XML (or JSON) from hello local file.
            string configuration = File.ReadAllText("CustomPreset_JSON.json");

            // Create a task
            ITask task = job.Tasks.AddNew("Media Encoder Standard encoding task",
            processor,
            configuration,
            TaskOptions.None);

            // Specify hello input asset toobe encoded.
            task.InputAssets.Add(asset);
            // Add an output asset toocontain hello results of hello job. 
            // This output is specified as AssetCreationOptions.None, which 
            // means hello output asset is not encrypted. 
            task.OutputAssets.AddNew("Output asset",
            AssetCreationOptions.None);

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

## <a name="media-services-learning-paths"></a><span data-ttu-id="efa03-130">Sökvägar för Media Services-utbildning</span><span class="sxs-lookup"><span data-stu-id="efa03-130">Media Services learning paths</span></span>
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a><span data-ttu-id="efa03-131">Ge feedback</span><span class="sxs-lookup"><span data-stu-id="efa03-131">Provide feedback</span></span>
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

## <a name="see-also"></a><span data-ttu-id="efa03-132">Se även</span><span class="sxs-lookup"><span data-stu-id="efa03-132">See Also</span></span>
[<span data-ttu-id="efa03-133">Media Services Encoding översikt</span><span class="sxs-lookup"><span data-stu-id="efa03-133">Media Services Encoding Overview</span></span>](media-services-encode-asset.md)

