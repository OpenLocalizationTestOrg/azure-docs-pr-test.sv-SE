---
title: "Avancerade encoding med Media Encoder Premium arbetsflödet | Microsoft Docs"
description: "Lär dig mer om att koda med Media Encoder Premium arbetsflöde. Kodexemplen är skrivna i C# och använder Media Services SDK för .NET."
services: media-services
documentationcenter: 
author: juliako
manager: cfowler
editor: 
ms.assetid: 0f4c87ac-810a-4d42-8df8-923dff2016c6
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/18/2017
ms.author: juliako
ms.openlocfilehash: 2b03853bf07e05c07fd730d5e8a8563963887921
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/29/2017
---
# <a name="advanced-encoding-with-media-encoder-premium-workflow"></a><span data-ttu-id="55709-104">Avancerade encoding med Media Encoder Premium arbetsflöde</span><span class="sxs-lookup"><span data-stu-id="55709-104">Advanced encoding with Media Encoder Premium Workflow</span></span>
> [!NOTE]
> <span data-ttu-id="55709-105">Media Encoder Premium arbetsflöde medieprocessor som beskrivs i det här avsnittet är inte tillgänglig i Kina.</span><span class="sxs-lookup"><span data-stu-id="55709-105">Media Encoder Premium Workflow media processor discussed in this topic is not available in China.</span></span>
>
>

<span data-ttu-id="55709-106">E-mepd på Microsoft.com för premium-kodare frågor.</span><span class="sxs-lookup"><span data-stu-id="55709-106">For premium encoder questions, email mepd at Microsoft.com.</span></span>

## <a name="overview"></a><span data-ttu-id="55709-107">Översikt</span><span class="sxs-lookup"><span data-stu-id="55709-107">Overview</span></span>
<span data-ttu-id="55709-108">Microsoft Azure Media Services presenterar den **Media Encoder Premium arbetsflöde** medieprocessor.</span><span class="sxs-lookup"><span data-stu-id="55709-108">Microsoft Azure Media Services is introducing the **Media Encoder Premium Workflow** media processor.</span></span> <span data-ttu-id="55709-109">Detta processor erbjudanden förskott kodning funktioner för arbetsflöden din premium på begäran.</span><span class="sxs-lookup"><span data-stu-id="55709-109">This processor offers advance encoding capabilities for your premium on-demand workflows.</span></span>

<span data-ttu-id="55709-110">Följande avsnitt beskriver information som rör **Media Encoder Premium arbetsflöde**:</span><span class="sxs-lookup"><span data-stu-id="55709-110">The following topics outline details related to **Media Encoder Premium Workflow**:</span></span>

* <span data-ttu-id="55709-111">[Formaterar stöds av Media Encoder Premium arbetsflödet](media-services-premium-workflow-encoder-formats.md) – Discusses filen formaterar och codec-rutiner som stöds av **Media Encoder Premium arbetsflöde**.</span><span class="sxs-lookup"><span data-stu-id="55709-111">[Formats Supported by the Media Encoder Premium Workflow](media-services-premium-workflow-encoder-formats.md) – Discusses the file formats and codecs supported by **Media Encoder Premium Workflow**.</span></span>
* <span data-ttu-id="55709-112">[Översikt över och jämförelse av Azure på begäran mediet kodare](media-services-encode-asset.md) jämför kodning funktionerna i **Media Encoder Premium arbetsflöde** och **Media Encoder Standard**.</span><span class="sxs-lookup"><span data-stu-id="55709-112">[Overview and comparison of Azure on demand media encoders](media-services-encode-asset.md) compares the encoding capabilities of **Media Encoder Premium Workflow** and **Media Encoder Standard**.</span></span>

<span data-ttu-id="55709-113">Det här avsnittet visar hur du kodar med **Media Encoder Premium arbetsflöde** med hjälp av .NET.</span><span class="sxs-lookup"><span data-stu-id="55709-113">This topic demonstrates how to encode with **Media Encoder Premium Workflow** using .NET.</span></span>

<span data-ttu-id="55709-114">Kodning uppgifter för den **Media Encoder Premium arbetsflöde** kräver en separat konfigurationsfil, kallas för ett arbetsflödesfil.</span><span class="sxs-lookup"><span data-stu-id="55709-114">Encoding tasks for the **Media Encoder Premium Workflow** require a separate configuration file, called a Workflow file.</span></span> <span data-ttu-id="55709-115">Filerna har filnamnstillägget .workflow och skapas med hjälp av den [Arbetsflödesdesignern](media-services-workflow-designer.md) verktyget.</span><span class="sxs-lookup"><span data-stu-id="55709-115">These files have a .workflow extension and are created using the [Workflow Designer](media-services-workflow-designer.md) tool.</span></span>

<span data-ttu-id="55709-116">Du kan också få standard Arbetsflödesfiler [här](https://github.com/Azure/azure-media-services-samples/tree/master/Encoding%20Presets/VoD/MediaEncoderPremiumWorkfows).</span><span class="sxs-lookup"><span data-stu-id="55709-116">You can also get the default workflow files [here](https://github.com/Azure/azure-media-services-samples/tree/master/Encoding%20Presets/VoD/MediaEncoderPremiumWorkfows).</span></span> <span data-ttu-id="55709-117">Mappen innehåller också en beskrivning av dessa filer.</span><span class="sxs-lookup"><span data-stu-id="55709-117">The folder also contains the description of these files.</span></span>

<span data-ttu-id="55709-118">Arbetsflödesfiler måste överföras till Media Services-kontot som en tillgång och tillgången ska skickas till aktiviteten kodning.</span><span class="sxs-lookup"><span data-stu-id="55709-118">The workflow files need to be uploaded to your Media Services account as an Asset, and this Asset should be passed in to the encoding Task.</span></span>

## <a name="create-and-configure-a-visual-studio-project"></a><span data-ttu-id="55709-119">Skapa och konfigurera ett Visual Studio-projekt</span><span class="sxs-lookup"><span data-stu-id="55709-119">Create and configure a Visual Studio project</span></span>

<span data-ttu-id="55709-120">Konfigurera utvecklingsmiljön och fyll i filen app.config med anslutningsinformation, enligt beskrivningen i [Media Services-utveckling med .NET](media-services-dotnet-how-to-use.md).</span><span class="sxs-lookup"><span data-stu-id="55709-120">Set up your development environment and populate the app.config file with connection information, as described in [Media Services development with .NET](media-services-dotnet-how-to-use.md).</span></span> 

## <a name="encoding-example"></a><span data-ttu-id="55709-121">Kodning exempel</span><span class="sxs-lookup"><span data-stu-id="55709-121">Encoding example</span></span>

<span data-ttu-id="55709-122">I följande exempel visar hur du kodar med **Media Encoder Premium arbetsflöde**.</span><span class="sxs-lookup"><span data-stu-id="55709-122">The following example demonstrates how to encode with **Media Encoder Premium Workflow**.</span></span>

<span data-ttu-id="55709-123">Utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="55709-123">The following steps are performed:</span></span>

1. <span data-ttu-id="55709-124">Skapa en tillgång och överför en arbetsflödesfil.</span><span class="sxs-lookup"><span data-stu-id="55709-124">Create an asset and upload a workflow file.</span></span>
2. <span data-ttu-id="55709-125">Skapa en tillgång och överför en mediefil källa.</span><span class="sxs-lookup"><span data-stu-id="55709-125">Create an asset and upload a source media file.</span></span>
3. <span data-ttu-id="55709-126">Hämta media-processorn ”Media Encoder Premium arbetsflöde”.</span><span class="sxs-lookup"><span data-stu-id="55709-126">Get the "Media Encoder Premium Workflow" media processor.</span></span>
4. <span data-ttu-id="55709-127">Skapa ett jobb och en aktivitet.</span><span class="sxs-lookup"><span data-stu-id="55709-127">Create a job and a task.</span></span>

    <span data-ttu-id="55709-128">I de flesta fall konfigurationssträngen för aktiviteten är tom (som i följande exempel).</span><span class="sxs-lookup"><span data-stu-id="55709-128">In most cases, the configuration string for the task is empty (like in the following example).</span></span> <span data-ttu-id="55709-129">Vissa avancerade scenarier (som kräver att du att ange egenskaper för runtime dynamiskt) i så fall skulle du ange en XML-sträng kodning aktiviteten.</span><span class="sxs-lookup"><span data-stu-id="55709-129">There are some advanced scenarios (that require you to to set runtime properties dynamically) in which case you would provide an XML string to the encoding task.</span></span> <span data-ttu-id="55709-130">Exempel på sådana scenarier är: skapa en överlägget, parallella och sekventiella media att gå till, textning.</span><span class="sxs-lookup"><span data-stu-id="55709-130">Examples of such scenarios are: creating an overlay, parallel or sequential media stitching, subtitling.</span></span>
5. <span data-ttu-id="55709-131">Lägga till två inkommande tillgångar i aktiviteten.</span><span class="sxs-lookup"><span data-stu-id="55709-131">Add two input assets to the task.</span></span>

    1. <span data-ttu-id="55709-132">1 – arbetsflöde tillgången.</span><span class="sxs-lookup"><span data-stu-id="55709-132">1st – the workflow asset.</span></span>
    2. <span data-ttu-id="55709-133">2 – video tillgången.</span><span class="sxs-lookup"><span data-stu-id="55709-133">2nd – the video asset.</span></span>

    >[!NOTE]
    ><span data-ttu-id="55709-134">Arbetsflödet tillgången måste läggas till aktivitet innan media tillgången.</span><span class="sxs-lookup"><span data-stu-id="55709-134">The workflow asset must be added to the task before the media asset.</span></span>
   <span data-ttu-id="55709-135">Konfigurationssträngen för den här uppgiften ska vara tomt.</span><span class="sxs-lookup"><span data-stu-id="55709-135">The configuration string for this task should be empty.</span></span>
   
6. <span data-ttu-id="55709-136">Skicka kodningsjobbet.</span><span class="sxs-lookup"><span data-stu-id="55709-136">Submit the encoding job.</span></span>

        using System;
        using System.Linq;
        using System.Configuration;
        using System.IO;
        using System.Threading;
        using System.Threading.Tasks;
        using Microsoft.WindowsAzure.MediaServices.Client;

        namespace MediaEncoderPremiumWorkflowSample
        {
            class Program
            {
                private static readonly string _AADTenantDomain =
                    ConfigurationManager.AppSettings["AADTenantDomain"];
                private static readonly string _RESTAPIEndpoint =
                    ConfigurationManager.AppSettings["MediaServiceRESTAPIEndpoint"];

                // Field for service context.
                private static CloudMediaContext _context = null;

                private static readonly string _supportFiles =
                    Path.GetFullPath(@"../..\Media");

                private static readonly string _workflowFilePath =
                    Path.GetFullPath(_supportFiles + @"\H264 Progressive Download MP4.workflow");

                private static readonly string _singleMP4InputFilePath =
                    Path.GetFullPath(_supportFiles + @"\BigBuckBunny.mp4");

                static void Main(string[] args)
                {
                    var tokenCredentials = new AzureAdTokenCredentials(_AADTenantDomain, AzureEnvironments.AzureCloudEnvironment);
                    var tokenProvider = new AzureAdTokenProvider(tokenCredentials);

                    _context = new CloudMediaContext(new Uri(_RESTAPIEndpoint), tokenProvider);

                    var workflowAsset = CreateAssetAndUploadSingleFile(_workflowFilePath);
                    var videoAsset = CreateAssetAndUploadSingleFile(_singleMP4InputFilePath);
                    IAsset outputAsset = CreateEncodingJob(workflowAsset, videoAsset);

                }

                static public IAsset CreateAssetAndUploadSingleFile(string singleFilePath)
                {
                    var assetName = "UploadSingleFile_" + DateTime.UtcNow.ToString();
                    var asset = _context.Assets.Create(assetName, AssetCreationOptions.None);

                    var fileName = Path.GetFileName(singleFilePath);

                    var assetFile = asset.AssetFiles.Create(fileName);

                    Console.WriteLine("Created assetFile {0}", assetFile.Name);

                    Console.WriteLine("Upload {0}", assetFile.Name);

                    assetFile.Upload(singleFilePath);
                    Console.WriteLine("Done uploading {0}", assetFile.Name);

                    return asset;
                }

                static public IAsset CreateEncodingJob(IAsset workflow, IAsset video)
                {
                    // Declare a new job.
                    IJob job = _context.Jobs.Create("Premium Workflow encoding job");
                    // Get a media processor reference, and pass to it the name of the
                    // processor to use for the specific task.
                    IMediaProcessor processor = GetLatestMediaProcessorByName("Media Encoder Premium Workflow");

                    // Create a task with the encoding details, using a string preset.
                    ITask task = job.Tasks.AddNew("Premium Workflow encoding task",
                        processor,
                        "",
                        TaskOptions.None);

                    // Specify the input asset to be encoded.
                    task.InputAssets.Add(workflow);
                    task.InputAssets.Add(video); // we add one asset
                                                 // Add an output asset to contain the results of the job.
                                                 // This output is specified as AssetCreationOptions.None, which
                                                 // means the output asset is not encrypted.
                    task.OutputAssets.AddNew("Output asset",
                        AssetCreationOptions.None);

                    // Use the following event handler to check job progress.  
                    job.StateChanged += new
                            EventHandler<JobStateChangedEventArgs>(StateChanged);

                    // Launch the job.
                    job.Submit();

                    // Check job execution and wait for job to finish.
                    Task progressJobTask = job.GetExecutionProgressTask(CancellationToken.None);
                    progressJobTask.Wait();

                    // If job state is Error the event handling
                    // method for job progress should log errors.  Here we check
                    // for error state and exit if needed.
                    if (job.State == JobState.Error)
                    {
                        throw new Exception("\nExiting method due to job error.");
                    }

                    return job.OutputMediaAssets[0];
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

<span data-ttu-id="55709-137">E-mepd på Microsoft.com för premium-kodare frågor.</span><span class="sxs-lookup"><span data-stu-id="55709-137">For premium encoder questions, email mepd at Microsoft.com.</span></span>

## <a name="media-services-learning-paths"></a><span data-ttu-id="55709-138">Sökvägar för Media Services-utbildning</span><span class="sxs-lookup"><span data-stu-id="55709-138">Media Services learning paths</span></span>
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a><span data-ttu-id="55709-139">Ge feedback</span><span class="sxs-lookup"><span data-stu-id="55709-139">Provide feedback</span></span>
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]
