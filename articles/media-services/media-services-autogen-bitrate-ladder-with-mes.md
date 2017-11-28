---
title: aaaUse Azure Media Encoder Standard tooauto-Generera en bithastighet stege | Microsoft Docs
description: "Det här avsnittet visar hur toouse Media Encoder Standard (MES) tooauto-Generera en bithastighet stege baserat på hello inkommande upplösning och bithastighet. aldrig kommer att överskridas hello inkommande upplösning och bithastighet. Till exempel om hello-indata är 720p på 3 Mbit/s, utdata kommer förblir 720p i bästa och startar priser som är lägre än 3 Mbit/s."
services: media-services
documentationcenter: 
author: juliako
manager: cfowler
editor: 
ms.assetid: 63ed95da-1b82-44b0-b8ff-eebd535bc5c7
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/20/2017
ms.author: juliako
ms.openlocfilehash: 5437f54ac28c42ddd4f9d1986549d6da6261c5da
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
#  <a name="use-azure-media-encoder-standard-tooauto-generate-a-bitrate-ladder"></a><span data-ttu-id="72334-105">Använd Azure Media Encoder Standard tooauto-Generera en bithastighet stege</span><span class="sxs-lookup"><span data-stu-id="72334-105">Use Azure Media Encoder Standard tooauto-generate a bitrate ladder</span></span>

## <a name="overview"></a><span data-ttu-id="72334-106">Översikt</span><span class="sxs-lookup"><span data-stu-id="72334-106">Overview</span></span>

<span data-ttu-id="72334-107">Det här avsnittet visar hur toouse Media Encoder Standard (MES) tooauto-Generera en bithastighet stege (bithastighet upplösning par) baserat på hello inkommande upplösning och bithastighet.</span><span class="sxs-lookup"><span data-stu-id="72334-107">This topic shows how toouse Media Encoder Standard (MES) tooauto-generate a bitrate ladder (bitrate-resolution pairs) based on hello input resolution and bitrate.</span></span> <span data-ttu-id="72334-108">hello kan autogenererade förinställda inte överstiga hello indata upplösning och bithastighet.</span><span class="sxs-lookup"><span data-stu-id="72334-108">hello auto-generated preset will never exceed hello input resolution and bitrate.</span></span> <span data-ttu-id="72334-109">Till exempel om hello-indata är 720p på 3 Mbit/s, utdata kommer förblir 720p i bästa och startar priser som är lägre än 3 Mbit/s.</span><span class="sxs-lookup"><span data-stu-id="72334-109">For example, if hello input is 720p at 3Mbps, output will remain 720p at best, and will start at rates lower than 3Mbps.</span></span>

### <a name="encoding-for-streaming-only"></a><span data-ttu-id="72334-110">Kodning för strömning endast</span><span class="sxs-lookup"><span data-stu-id="72334-110">Encoding for Streaming Only</span></span>

<span data-ttu-id="72334-111">Om din avsikt är tooencode förinställda källfilerna video endast för strömning, bör du använda hello ”anpassningsbar strömning” när du skapar en kodning aktivitet.</span><span class="sxs-lookup"><span data-stu-id="72334-111">If your intent is tooencode your source video only for streaming, then you shoud use hello "Adaptive Streaming" preset when creating an encoding task.</span></span> <span data-ttu-id="72334-112">När du använder hello **anpassningsbar strömning** förinställts hello MES kodare kommer Intelligent cap stege en bithastighet.</span><span class="sxs-lookup"><span data-stu-id="72334-112">When using hello **Adaptive Streaming** preset, hello MES encoder will intelligently cap a bitrate ladder.</span></span> <span data-ttu-id="72334-113">Du kommer inte att kunna toocontrol hello kodning kostnader, eftersom hello bestämmer hur många lager toouse och med vilken upplösning.</span><span class="sxs-lookup"><span data-stu-id="72334-113">However, you will not be able toocontrol hello encoding costs, since hello service determines how many layers toouse and at what resolution.</span></span> <span data-ttu-id="72334-114">Du kan se ett exempel på utdata-lager som produceras av MES på grund av encoding med hello **anpassningsbar strömning** förinställningen hello slutet av det här avsnittet.</span><span class="sxs-lookup"><span data-stu-id="72334-114">You can see examples of output layers produced by MES as a result of encoding with hello **Adaptive Streaming** preset at hello end of this topic.</span></span> <span data-ttu-id="72334-115">hello utdata tillgång innehåller MP4-filer där ljud och video inte är överlagrat.</span><span class="sxs-lookup"><span data-stu-id="72334-115">hello output Asset will contain MP4 files where audio and video are not interleaved.</span></span>

### <a name="encoding-for-streaming-and-progressive-download"></a><span data-ttu-id="72334-116">Kodning för strömning och progressiv hämtning</span><span class="sxs-lookup"><span data-stu-id="72334-116">Encoding for Streaming and Progressive Download</span></span>

<span data-ttu-id="72334-117">Om din avsikt är tooencode förinställda videon källa för direktuppspelning samt tooproduce MP4-filer för progressiv nedladdning av du bör använda hello ”innehåll anpassningsbar flera bithastighet MP4” när du skapar en kodning aktivitet.</span><span class="sxs-lookup"><span data-stu-id="72334-117">If your intent is tooencode your source video for streaming as well as tooproduce MP4 files for progressive download, then you shoud use hello "Content Adaptive Multiple Bitrate MP4" preset when creating an encoding task.</span></span> <span data-ttu-id="72334-118">När du använder hello **innehåll anpassningsbar flera bithastighet MP4** förinställts hello MES kodare gäller hello samma kodning logik som ovan, men nu hello utdatatillgången innehåller MP4-filer där ljud och video är överlagrad.</span><span class="sxs-lookup"><span data-stu-id="72334-118">When using hello **Content Adaptive Multiple Bitrate MP4** preset, hello MES encoder will apply hello same encoding logic as above, but now hello output asset will contain MP4 files where audio and video are interleaved.</span></span> <span data-ttu-id="72334-119">Du kan använda någon av dessa MP4-filer (till exempel hello högsta bithastighet version) som en fil för progressiv nedladdning.</span><span class="sxs-lookup"><span data-stu-id="72334-119">You can use one of these MP4 files (for example, hello highest bitrate version) as a progressive download file.</span></span>

## <span data-ttu-id="72334-120"><a id="encoding_with_dotnet"></a>Encoding med Media Services .NET SDK</span><span class="sxs-lookup"><span data-stu-id="72334-120"><a id="encoding_with_dotnet"></a>Encoding with Media Services .NET SDK</span></span>

<span data-ttu-id="72334-121">Följande kodexempel hello använder Media Services .NET SDK tooperform hello följande uppgifter:</span><span class="sxs-lookup"><span data-stu-id="72334-121">hello following code example uses Media Services .NET SDK tooperform hello following tasks:</span></span>

- <span data-ttu-id="72334-122">Skapa ett kodningsjobb.</span><span class="sxs-lookup"><span data-stu-id="72334-122">Create an encoding job.</span></span>
- <span data-ttu-id="72334-123">Hämta en referens toohello Media Encoder Standard-kodare.</span><span class="sxs-lookup"><span data-stu-id="72334-123">Get a reference toohello Media Encoder Standard encoder.</span></span>
- <span data-ttu-id="72334-124">Lägg till en aktivitet toohello kodningsjobbet och ange toouse hello **anpassningsbar strömning** förinställda.</span><span class="sxs-lookup"><span data-stu-id="72334-124">Add an encoding task toohello job and specify toouse hello **Adaptive Streaming** preset.</span></span> 
- <span data-ttu-id="72334-125">Skapa en utdata tillgång som innehåller hello kodade tillgången.</span><span class="sxs-lookup"><span data-stu-id="72334-125">Create an output asset that will contain hello encoded asset.</span></span>
- <span data-ttu-id="72334-126">Lägg till en händelse hanteraren toocheck hello jobb pågår.</span><span class="sxs-lookup"><span data-stu-id="72334-126">Add an event handler toocheck hello job progress.</span></span>
- <span data-ttu-id="72334-127">Skicka hello-jobbet.</span><span class="sxs-lookup"><span data-stu-id="72334-127">Submit hello job.</span></span>

#### <a name="create-and-configure-a-visual-studio-project"></a><span data-ttu-id="72334-128">Skapa och konfigurera ett Visual Studio-projekt</span><span class="sxs-lookup"><span data-stu-id="72334-128">Create and configure a Visual Studio project</span></span>

<span data-ttu-id="72334-129">Konfigurera utvecklingsmiljön och fylla hello app.config-fil med anslutningsinformation, enligt beskrivningen i [Media Services-utveckling med .NET](media-services-dotnet-how-to-use.md).</span><span class="sxs-lookup"><span data-stu-id="72334-129">Set up your development environment and populate hello app.config file with connection information, as described in [Media Services development with .NET](media-services-dotnet-how-to-use.md).</span></span> 

#### <a name="example"></a><span data-ttu-id="72334-130">Exempel</span><span class="sxs-lookup"><span data-stu-id="72334-130">Example</span></span>

    using System;
    using System.Configuration;
    using System.Linq;
    using Microsoft.WindowsAzure.MediaServices.Client;
    using System.Threading;

    namespace AdaptiveStreamingMESPresest
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

## <span data-ttu-id="72334-131"><a id="output"></a>Utdata</span><span class="sxs-lookup"><span data-stu-id="72334-131"><a id="output"></a>Output</span></span>

<span data-ttu-id="72334-132">Det här avsnittet visar tre exempel på utdata-lager som produceras av MES på grund av encoding med hello **anpassningsbar strömning** förinställda.</span><span class="sxs-lookup"><span data-stu-id="72334-132">This section shows three examples of output layers produced by MES as a result of encoding with hello **Adaptive Streaming** preset.</span></span> 

### <a name="example-1"></a><span data-ttu-id="72334-133">Exempel 1</span><span class="sxs-lookup"><span data-stu-id="72334-133">Example 1</span></span>
<span data-ttu-id="72334-134">Källa med höjd ”1080” och ramhastighet ”29.970” ger 6 video lager:</span><span class="sxs-lookup"><span data-stu-id="72334-134">Source with height "1080" and framerate "29.970" produces 6 video layers:</span></span>

|<span data-ttu-id="72334-135">Lager</span><span class="sxs-lookup"><span data-stu-id="72334-135">Layer</span></span>|<span data-ttu-id="72334-136">Höjd</span><span class="sxs-lookup"><span data-stu-id="72334-136">Height</span></span>|<span data-ttu-id="72334-137">Bredd</span><span class="sxs-lookup"><span data-stu-id="72334-137">Width</span></span>|<span data-ttu-id="72334-138">Bitrate(kbps)</span><span class="sxs-lookup"><span data-stu-id="72334-138">Bitrate(kbps)</span></span>|
|---|---|---|---|
|<span data-ttu-id="72334-139">1</span><span class="sxs-lookup"><span data-stu-id="72334-139">1</span></span>|<span data-ttu-id="72334-140">1080</span><span class="sxs-lookup"><span data-stu-id="72334-140">1080</span></span>|<span data-ttu-id="72334-141">1920</span><span class="sxs-lookup"><span data-stu-id="72334-141">1920</span></span>|<span data-ttu-id="72334-142">6780</span><span class="sxs-lookup"><span data-stu-id="72334-142">6780</span></span>|
|<span data-ttu-id="72334-143">2</span><span class="sxs-lookup"><span data-stu-id="72334-143">2</span></span>|<span data-ttu-id="72334-144">720</span><span class="sxs-lookup"><span data-stu-id="72334-144">720</span></span>|<span data-ttu-id="72334-145">1280</span><span class="sxs-lookup"><span data-stu-id="72334-145">1280</span></span>|<span data-ttu-id="72334-146">3520</span><span class="sxs-lookup"><span data-stu-id="72334-146">3520</span></span>|
|<span data-ttu-id="72334-147">3</span><span class="sxs-lookup"><span data-stu-id="72334-147">3</span></span>|<span data-ttu-id="72334-148">540</span><span class="sxs-lookup"><span data-stu-id="72334-148">540</span></span>|<span data-ttu-id="72334-149">960</span><span class="sxs-lookup"><span data-stu-id="72334-149">960</span></span>|<span data-ttu-id="72334-150">2210</span><span class="sxs-lookup"><span data-stu-id="72334-150">2210</span></span>|
|<span data-ttu-id="72334-151">4</span><span class="sxs-lookup"><span data-stu-id="72334-151">4</span></span>|<span data-ttu-id="72334-152">360</span><span class="sxs-lookup"><span data-stu-id="72334-152">360</span></span>|<span data-ttu-id="72334-153">640</span><span class="sxs-lookup"><span data-stu-id="72334-153">640</span></span>|<span data-ttu-id="72334-154">1150</span><span class="sxs-lookup"><span data-stu-id="72334-154">1150</span></span>|
|<span data-ttu-id="72334-155">5</span><span class="sxs-lookup"><span data-stu-id="72334-155">5</span></span>|<span data-ttu-id="72334-156">270</span><span class="sxs-lookup"><span data-stu-id="72334-156">270</span></span>|<span data-ttu-id="72334-157">480</span><span class="sxs-lookup"><span data-stu-id="72334-157">480</span></span>|<span data-ttu-id="72334-158">720</span><span class="sxs-lookup"><span data-stu-id="72334-158">720</span></span>|
|<span data-ttu-id="72334-159">6</span><span class="sxs-lookup"><span data-stu-id="72334-159">6</span></span>|<span data-ttu-id="72334-160">180</span><span class="sxs-lookup"><span data-stu-id="72334-160">180</span></span>|<span data-ttu-id="72334-161">320</span><span class="sxs-lookup"><span data-stu-id="72334-161">320</span></span>|<span data-ttu-id="72334-162">380</span><span class="sxs-lookup"><span data-stu-id="72334-162">380</span></span>|

### <a name="example-2"></a><span data-ttu-id="72334-163">Exempel 2</span><span class="sxs-lookup"><span data-stu-id="72334-163">Example 2</span></span>
<span data-ttu-id="72334-164">Källa med höjd ”720” och ramhastighet ”23.970” ger 5 video lager:</span><span class="sxs-lookup"><span data-stu-id="72334-164">Source with height "720" and framerate "23.970" produces 5 video layers:</span></span>

|<span data-ttu-id="72334-165">Lager</span><span class="sxs-lookup"><span data-stu-id="72334-165">Layer</span></span>|<span data-ttu-id="72334-166">Höjd</span><span class="sxs-lookup"><span data-stu-id="72334-166">Height</span></span>|<span data-ttu-id="72334-167">Bredd</span><span class="sxs-lookup"><span data-stu-id="72334-167">Width</span></span>|<span data-ttu-id="72334-168">Bitrate(kbps)</span><span class="sxs-lookup"><span data-stu-id="72334-168">Bitrate(kbps)</span></span>|
|---|---|---|---|
|<span data-ttu-id="72334-169">1</span><span class="sxs-lookup"><span data-stu-id="72334-169">1</span></span>|<span data-ttu-id="72334-170">720</span><span class="sxs-lookup"><span data-stu-id="72334-170">720</span></span>|<span data-ttu-id="72334-171">1280</span><span class="sxs-lookup"><span data-stu-id="72334-171">1280</span></span>|<span data-ttu-id="72334-172">2940</span><span class="sxs-lookup"><span data-stu-id="72334-172">2940</span></span>|
|<span data-ttu-id="72334-173">2</span><span class="sxs-lookup"><span data-stu-id="72334-173">2</span></span>|<span data-ttu-id="72334-174">540</span><span class="sxs-lookup"><span data-stu-id="72334-174">540</span></span>|<span data-ttu-id="72334-175">960</span><span class="sxs-lookup"><span data-stu-id="72334-175">960</span></span>|<span data-ttu-id="72334-176">1850</span><span class="sxs-lookup"><span data-stu-id="72334-176">1850</span></span>|
|<span data-ttu-id="72334-177">3</span><span class="sxs-lookup"><span data-stu-id="72334-177">3</span></span>|<span data-ttu-id="72334-178">360</span><span class="sxs-lookup"><span data-stu-id="72334-178">360</span></span>|<span data-ttu-id="72334-179">640</span><span class="sxs-lookup"><span data-stu-id="72334-179">640</span></span>|<span data-ttu-id="72334-180">960</span><span class="sxs-lookup"><span data-stu-id="72334-180">960</span></span>|
|<span data-ttu-id="72334-181">4</span><span class="sxs-lookup"><span data-stu-id="72334-181">4</span></span>|<span data-ttu-id="72334-182">270</span><span class="sxs-lookup"><span data-stu-id="72334-182">270</span></span>|<span data-ttu-id="72334-183">480</span><span class="sxs-lookup"><span data-stu-id="72334-183">480</span></span>|<span data-ttu-id="72334-184">600</span><span class="sxs-lookup"><span data-stu-id="72334-184">600</span></span>|
|<span data-ttu-id="72334-185">5</span><span class="sxs-lookup"><span data-stu-id="72334-185">5</span></span>|<span data-ttu-id="72334-186">180</span><span class="sxs-lookup"><span data-stu-id="72334-186">180</span></span>|<span data-ttu-id="72334-187">320</span><span class="sxs-lookup"><span data-stu-id="72334-187">320</span></span>|<span data-ttu-id="72334-188">320</span><span class="sxs-lookup"><span data-stu-id="72334-188">320</span></span>|

### <a name="example-3"></a><span data-ttu-id="72334-189">Exempel 3</span><span class="sxs-lookup"><span data-stu-id="72334-189">Example 3</span></span>
<span data-ttu-id="72334-190">Källa med höjd ”360” och ramhastighet ”29.970” ger 3 video lager:</span><span class="sxs-lookup"><span data-stu-id="72334-190">Source with height "360" and framerate "29.970" produces 3 video layers:</span></span>

|<span data-ttu-id="72334-191">Lager</span><span class="sxs-lookup"><span data-stu-id="72334-191">Layer</span></span>|<span data-ttu-id="72334-192">Höjd</span><span class="sxs-lookup"><span data-stu-id="72334-192">Height</span></span>|<span data-ttu-id="72334-193">Bredd</span><span class="sxs-lookup"><span data-stu-id="72334-193">Width</span></span>|<span data-ttu-id="72334-194">Bitrate(kbps)</span><span class="sxs-lookup"><span data-stu-id="72334-194">Bitrate(kbps)</span></span>|
|---|---|---|---|
|<span data-ttu-id="72334-195">1</span><span class="sxs-lookup"><span data-stu-id="72334-195">1</span></span>|<span data-ttu-id="72334-196">360</span><span class="sxs-lookup"><span data-stu-id="72334-196">360</span></span>|<span data-ttu-id="72334-197">640</span><span class="sxs-lookup"><span data-stu-id="72334-197">640</span></span>|<span data-ttu-id="72334-198">700</span><span class="sxs-lookup"><span data-stu-id="72334-198">700</span></span>|
|<span data-ttu-id="72334-199">2</span><span class="sxs-lookup"><span data-stu-id="72334-199">2</span></span>|<span data-ttu-id="72334-200">270</span><span class="sxs-lookup"><span data-stu-id="72334-200">270</span></span>|<span data-ttu-id="72334-201">480</span><span class="sxs-lookup"><span data-stu-id="72334-201">480</span></span>|<span data-ttu-id="72334-202">440</span><span class="sxs-lookup"><span data-stu-id="72334-202">440</span></span>|
|<span data-ttu-id="72334-203">3</span><span class="sxs-lookup"><span data-stu-id="72334-203">3</span></span>|<span data-ttu-id="72334-204">180</span><span class="sxs-lookup"><span data-stu-id="72334-204">180</span></span>|<span data-ttu-id="72334-205">320</span><span class="sxs-lookup"><span data-stu-id="72334-205">320</span></span>|<span data-ttu-id="72334-206">230</span><span class="sxs-lookup"><span data-stu-id="72334-206">230</span></span>|
## <a name="media-services-learning-paths"></a><span data-ttu-id="72334-207">Sökvägar för Media Services-utbildning</span><span class="sxs-lookup"><span data-stu-id="72334-207">Media Services learning paths</span></span>
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a><span data-ttu-id="72334-208">Ge feedback</span><span class="sxs-lookup"><span data-stu-id="72334-208">Provide feedback</span></span>
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

## <a name="see-also"></a><span data-ttu-id="72334-209">Se även</span><span class="sxs-lookup"><span data-stu-id="72334-209">See Also</span></span>
[<span data-ttu-id="72334-210">Media Services Encoding översikt</span><span class="sxs-lookup"><span data-stu-id="72334-210">Media Services Encoding Overview</span></span>](media-services-encode-asset.md)

