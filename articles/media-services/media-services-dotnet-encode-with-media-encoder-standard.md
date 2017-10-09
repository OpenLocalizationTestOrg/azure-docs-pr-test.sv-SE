---
title: "aaaEncode en tillgång med Media Encoder Standard med hjälp av .NET | Microsoft Docs"
description: "Det här avsnittet visar hur toouse .NET tooencode en tillgång med Media Encoder Strandard."
services: media-services
documentationcenter: 
author: juliako
manager: cfowler
editor: 
ms.assetid: 03431b64-5518-478a-a1c2-1de345999274
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/31/2017
ms.author: juliako;anilmur
ms.openlocfilehash: 25e274c3b67168f4afc8b8ab04af2d654c9dd6e4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="encode-an-asset-with-media-encoder-standard-using-net"></a><span data-ttu-id="202dc-103">Koda en tillgång med Media Encoder Standard med hjälp av .NET</span><span class="sxs-lookup"><span data-stu-id="202dc-103">Encode an asset with Media Encoder Standard using .NET</span></span>
<span data-ttu-id="202dc-104">Kodning jobb är en hello vanligaste bearbetningen i Media Services.</span><span class="sxs-lookup"><span data-stu-id="202dc-104">Encoding jobs are one of hello most common processing operations in Media Services.</span></span> <span data-ttu-id="202dc-105">Du skapar kodning jobb tooconvert media-filer från en kodning tooanother.</span><span class="sxs-lookup"><span data-stu-id="202dc-105">You create encoding jobs tooconvert media files from one encoding tooanother.</span></span> <span data-ttu-id="202dc-106">När du kodar kan du använda hello Media Services inbyggda Media Encoder.</span><span class="sxs-lookup"><span data-stu-id="202dc-106">When you encode, you can use hello Media Services built-in Media Encoder.</span></span> <span data-ttu-id="202dc-107">Du kan också använda en kodare som tillhandahålls av Media Services partner; kodare från tredje part är tillgängliga via hello Azure Marketplace.</span><span class="sxs-lookup"><span data-stu-id="202dc-107">You can also use an encoder provided by a Media Services partner; third party encoders are available through hello Azure Marketplace.</span></span> 

<span data-ttu-id="202dc-108">Det här avsnittet visar hur toouse .NET tooencode dina tillgångar med Media Encoder Standard (MES).</span><span class="sxs-lookup"><span data-stu-id="202dc-108">This topic shows how toouse .NET tooencode your assets with Media Encoder Standard (MES).</span></span> <span data-ttu-id="202dc-109">Media Encoder Standard konfigureras med hjälp av en hello kodare förinställningar beskrivs [här](http://go.microsoft.com/fwlink/?linkid=618336&clcid=0x409).</span><span class="sxs-lookup"><span data-stu-id="202dc-109">Media Encoder Standard is configured using one of hello encoder presets described [here](http://go.microsoft.com/fwlink/?linkid=618336&clcid=0x409).</span></span>

<span data-ttu-id="202dc-110">Rekommenderas tooalways koda källfilerna till en MP4-uppsättningen med anpassad bithastighet och sedan konvertera hello set toohello önskade format med hjälp av hello [dynamisk paketering](media-services-dynamic-packaging-overview.md).</span><span class="sxs-lookup"><span data-stu-id="202dc-110">It is recommended tooalways encode your source files into an adaptive bitrate MP4 set and then convert hello set toohello desired format using hello [Dynamic Packaging](media-services-dynamic-packaging-overview.md).</span></span> 

<span data-ttu-id="202dc-111">Om din utdatatillgången är lagringskrypterad, måste du konfigurera principen för tillgångsleverans.</span><span class="sxs-lookup"><span data-stu-id="202dc-111">If your output asset is storage encrypted, you must configure asset delivery policy.</span></span> <span data-ttu-id="202dc-112">Mer information finns i [konfigurera tillgångsleveransprincip](media-services-dotnet-configure-asset-delivery-policy.md).</span><span class="sxs-lookup"><span data-stu-id="202dc-112">For more information see [Configuring asset delivery policy](media-services-dotnet-configure-asset-delivery-policy.md).</span></span>

> [!NOTE]
> <span data-ttu-id="202dc-113">MES ger en utdatafil med ett namn som innehåller hello först 32 tecken för hello indatafilens namn.</span><span class="sxs-lookup"><span data-stu-id="202dc-113">MES produces an output file with a name that contains hello first 32 characters of hello input file name.</span></span> <span data-ttu-id="202dc-114">hello namn baserat på vad som anges i hello förvalda.</span><span class="sxs-lookup"><span data-stu-id="202dc-114">hello name is based on what is specified in hello preset file.</span></span> <span data-ttu-id="202dc-115">Till exempel ”FileName”: ”{Basename} _ {Index} {tillägg}”.</span><span class="sxs-lookup"><span data-stu-id="202dc-115">For example, "FileName": "{Basename}_{Index}{Extension}".</span></span> <span data-ttu-id="202dc-116">{Basename} har ersatts av hello först 32 tecken hello indatafilen namn.</span><span class="sxs-lookup"><span data-stu-id="202dc-116">{Basename} is replaced by hello first 32 characters of hello input file name.</span></span>
> 
> 

### <a name="mes-formats"></a><span data-ttu-id="202dc-117">MES format</span><span class="sxs-lookup"><span data-stu-id="202dc-117">MES Formats</span></span>
[<span data-ttu-id="202dc-118">Format och codec</span><span class="sxs-lookup"><span data-stu-id="202dc-118">Formats and codecs</span></span>](media-services-media-encoder-standard-formats.md)

### <a name="mes-presets"></a><span data-ttu-id="202dc-119">MES-förinställningar</span><span class="sxs-lookup"><span data-stu-id="202dc-119">MES Presets</span></span>
<span data-ttu-id="202dc-120">Media Encoder Standard konfigureras med hjälp av en hello kodare förinställningar beskrivs [här](http://go.microsoft.com/fwlink/?linkid=618336&clcid=0x409).</span><span class="sxs-lookup"><span data-stu-id="202dc-120">Media Encoder Standard is configured using one of hello encoder presets described [here](http://go.microsoft.com/fwlink/?linkid=618336&clcid=0x409).</span></span>

### <a name="input-and-output-metadata"></a><span data-ttu-id="202dc-121">Indata och utdata metadata</span><span class="sxs-lookup"><span data-stu-id="202dc-121">Input and output metadata</span></span>
<span data-ttu-id="202dc-122">När du kodar en inkommande tillgång (eller tillgångar) med hjälp av MES du får en utdatatillgången på hello koda slutförd som aktiviteten.</span><span class="sxs-lookup"><span data-stu-id="202dc-122">When you encode an input asset (or assets) using MES, you get an output asset at hello successful completion of that encode task.</span></span> <span data-ttu-id="202dc-123">Hej utdatatillgången innehåller video, ljud, miniatyrer, manifestet, etc. baserat på hello kodning förinställning som du använder.</span><span class="sxs-lookup"><span data-stu-id="202dc-123">hello output asset contains video, audio, thumbnails, manifest, etc. based on hello encoding preset you use.</span></span>

<span data-ttu-id="202dc-124">Hej utdatatillgången innehåller också en fil med metadata om hello inkommande tillgången.</span><span class="sxs-lookup"><span data-stu-id="202dc-124">hello output asset also contains a file with metadata about hello input asset.</span></span> <span data-ttu-id="202dc-125">hello namnet på XML-filen för hello metadata har hello följande format: < asset_id > _metadata.xml (till exempel 41114ad3-eb5e - 4c 57 8d 92-5354e2b7d4a4_metadata.xml), där < asset_id > är hello AssetId värde av hello inkommande tillgång.</span><span class="sxs-lookup"><span data-stu-id="202dc-125">hello name of hello metadata XML file has hello following format: <asset_id>_metadata.xml (for example, 41114ad3-eb5e-4c57-8d92-5354e2b7d4a4_metadata.xml), where <asset_id> is hello AssetId value of hello input asset.</span></span> <span data-ttu-id="202dc-126">hello schemat för den här inkommande XML-metadata beskrivs [här](media-services-input-metadata-schema.md).</span><span class="sxs-lookup"><span data-stu-id="202dc-126">hello schema of this input metadata XML is described [here](media-services-input-metadata-schema.md).</span></span>

<span data-ttu-id="202dc-127">Hej utdatatillgången innehåller också en fil med metadata om hello utdatatillgången.</span><span class="sxs-lookup"><span data-stu-id="202dc-127">hello output asset also contains a file with metadata about hello output asset.</span></span> <span data-ttu-id="202dc-128">hello namnet på XML-filen för hello metadata har hello följande format: < source_file_name > _manifest.xml (till exempel BigBuckBunny_manifest.xml).</span><span class="sxs-lookup"><span data-stu-id="202dc-128">hello name of hello metadata XML file has hello following format: <source_file_name>_manifest.xml (for example, BigBuckBunny_manifest.xml).</span></span> <span data-ttu-id="202dc-129">hello schemat för dessa utdata metadata XML beskrivs [här](media-services-output-metadata-schema.md).</span><span class="sxs-lookup"><span data-stu-id="202dc-129">hello schema of this output metadata XML is described [here](media-services-output-metadata-schema.md).</span></span>

<span data-ttu-id="202dc-130">Om du vill tooexamine antingen hello två metadatafiler, kan du skapa en SAS-positionerare och hämta hello filen tooyour lokal dator.</span><span class="sxs-lookup"><span data-stu-id="202dc-130">If you want tooexamine either of hello two metadata files, you can create a SAS locator and download hello file tooyour local computer.</span></span> <span data-ttu-id="202dc-131">Du hittar ett exempel på hur toocreate en SAS-positionerare och hämta en fil med hjälp av hello Media Services .NET SDK-tillägg.</span><span class="sxs-lookup"><span data-stu-id="202dc-131">You can find an example on how toocreate a SAS locator and download a file Using hello Media Services .NET SDK Extensions.</span></span>

## <a name="download-sample"></a><span data-ttu-id="202dc-132">Hämta exempel</span><span class="sxs-lookup"><span data-stu-id="202dc-132">Download sample</span></span>
<span data-ttu-id="202dc-133">Du kan hämta och kör ett exempel som visar hur tooencode med MES från [här](https://azure.microsoft.com/documentation/samples/media-services-dotnet-on-demand-encoding-with-media-encoder-standard/).</span><span class="sxs-lookup"><span data-stu-id="202dc-133">You can get and run a sample that shows how tooencode with MES from [here](https://azure.microsoft.com/documentation/samples/media-services-dotnet-on-demand-encoding-with-media-encoder-standard/).</span></span>

## <a name="net-sample-code"></a><span data-ttu-id="202dc-134">Exempelkod för .NET</span><span class="sxs-lookup"><span data-stu-id="202dc-134">.NET sample code</span></span>

<span data-ttu-id="202dc-135">Följande kodexempel hello använder Media Services .NET SDK tooperform hello följande uppgifter:</span><span class="sxs-lookup"><span data-stu-id="202dc-135">hello following code example uses Media Services .NET SDK tooperform hello following tasks:</span></span>

* <span data-ttu-id="202dc-136">Skapa ett kodningsjobb.</span><span class="sxs-lookup"><span data-stu-id="202dc-136">Create an encoding job.</span></span>
* <span data-ttu-id="202dc-137">Hämta en referens toohello Media Encoder Standard-kodare.</span><span class="sxs-lookup"><span data-stu-id="202dc-137">Get a reference toohello Media Encoder Standard encoder.</span></span>
* <span data-ttu-id="202dc-138">Ange toouse hello [anpassningsbar strömning](media-services-autogen-bitrate-ladder-with-mes.md) förinställda.</span><span class="sxs-lookup"><span data-stu-id="202dc-138">Specify toouse hello [Adaptive Streaming](media-services-autogen-bitrate-ladder-with-mes.md) preset.</span></span> 
* <span data-ttu-id="202dc-139">Lägg till en enskild uppgift toohello kodningsjobbet.</span><span class="sxs-lookup"><span data-stu-id="202dc-139">Add a single encoding task toohello job.</span></span> 
* <span data-ttu-id="202dc-140">Ange hello indata tillgången toobe kodad.</span><span class="sxs-lookup"><span data-stu-id="202dc-140">Specify hello input asset toobe encoded.</span></span>
* <span data-ttu-id="202dc-141">Skapa en utdata tillgång som innehåller hello kodade tillgången.</span><span class="sxs-lookup"><span data-stu-id="202dc-141">Create an output asset that will contain hello encoded asset.</span></span>
* <span data-ttu-id="202dc-142">Lägg till en händelse hanteraren toocheck hello jobb pågår.</span><span class="sxs-lookup"><span data-stu-id="202dc-142">Add an event handler toocheck hello job progress.</span></span>
* <span data-ttu-id="202dc-143">Skicka hello-jobbet.</span><span class="sxs-lookup"><span data-stu-id="202dc-143">Submit hello job.</span></span>

#### <a name="create-and-configure-a-visual-studio-project"></a><span data-ttu-id="202dc-144">Skapa och konfigurera ett Visual Studio-projekt</span><span class="sxs-lookup"><span data-stu-id="202dc-144">Create and configure a Visual Studio project</span></span>

<span data-ttu-id="202dc-145">Konfigurera utvecklingsmiljön och fylla hello app.config-fil med anslutningsinformation, enligt beskrivningen i [Media Services-utveckling med .NET](media-services-dotnet-how-to-use.md).</span><span class="sxs-lookup"><span data-stu-id="202dc-145">Set up your development environment and populate hello app.config file with connection information, as described in [Media Services development with .NET](media-services-dotnet-how-to-use.md).</span></span> 

#### <a name="example"></a><span data-ttu-id="202dc-146">Exempel</span><span class="sxs-lookup"><span data-stu-id="202dc-146">Example</span></span> 

        using System;
        using System.Linq;
        using System.Configuration;
        using System.IO;
        using System.Threading;
        using Microsoft.WindowsAzure.MediaServices.Client;

        namespace MediaEncoderStandardSample
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

                    // Create a task with hello encoding details, using a string preset.
                    // In this case "Adaptive Streaming" preset is used.
                    ITask task = job.Tasks.AddNew("My encoding task",
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

## <a name="media-services-learning-paths"></a><span data-ttu-id="202dc-147">Sökvägar för Media Services-utbildning</span><span class="sxs-lookup"><span data-stu-id="202dc-147">Media Services learning paths</span></span>
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a><span data-ttu-id="202dc-148">Ge feedback</span><span class="sxs-lookup"><span data-stu-id="202dc-148">Provide feedback</span></span>
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

## <a name="next-steps"></a><span data-ttu-id="202dc-149">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="202dc-149">Next steps</span></span>
<span data-ttu-id="202dc-150">[Hur toogenerate miniatyr med Media Encoder Standard med .NET](media-services-dotnet-generate-thumbnail-with-mes.md)
[Media Services Encoding: översikt](media-services-encode-asset.md)</span><span class="sxs-lookup"><span data-stu-id="202dc-150">[How toogenerate thumbnail using Media Encoder Standard with .NET](media-services-dotnet-generate-thumbnail-with-mes.md)
[Media Services Encoding Overview](media-services-encode-asset.md)</span></span>

