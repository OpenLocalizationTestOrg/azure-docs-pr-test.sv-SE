---
title: "Koda en tillgång med Media Encoder Standard med hjälp av .NET | Microsoft Docs"
description: "Det här avsnittet visar hur du använder .NET för att koda en tillgång med Media Encoder Strandard."
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
ms.openlocfilehash: 929592368501c54277748bf46b2160c9058db3fb
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/29/2017
---
# <a name="encode-an-asset-with-media-encoder-standard-using-net"></a><span data-ttu-id="e6ce7-103">Koda en tillgång med Media Encoder Standard med hjälp av .NET</span><span class="sxs-lookup"><span data-stu-id="e6ce7-103">Encode an asset with Media Encoder Standard using .NET</span></span>
<span data-ttu-id="e6ce7-104">Kodningsjobb är en av de vanligaste bearbetningsuppgifterna i Media Services.</span><span class="sxs-lookup"><span data-stu-id="e6ce7-104">Encoding jobs are one of the most common processing operations in Media Services.</span></span> <span data-ttu-id="e6ce7-105">Du skapar kodningsjobb för att konvertera mediefiler från en kodning till en annan.</span><span class="sxs-lookup"><span data-stu-id="e6ce7-105">You create encoding jobs to convert media files from one encoding to another.</span></span> <span data-ttu-id="e6ce7-106">När du kodar kan du använda Media Services-inbyggda Media-kodaren.</span><span class="sxs-lookup"><span data-stu-id="e6ce7-106">When you encode, you can use the Media Services built-in Media Encoder.</span></span> <span data-ttu-id="e6ce7-107">Du kan också använda en kodare som tillhandahålls av Media Services partner; kodare från tredje part är tillgängliga via Azure Marketplace.</span><span class="sxs-lookup"><span data-stu-id="e6ce7-107">You can also use an encoder provided by a Media Services partner; third party encoders are available through the Azure Marketplace.</span></span> 

<span data-ttu-id="e6ce7-108">Det här avsnittet visar hur du använder .NET för att koda dina tillgångar med Media Encoder Standard (MES).</span><span class="sxs-lookup"><span data-stu-id="e6ce7-108">This topic shows how to use .NET to encode your assets with Media Encoder Standard (MES).</span></span> <span data-ttu-id="e6ce7-109">Media Encoder Standard konfigureras med hjälp av en kodare förinställningar beskrivs [här](http://go.microsoft.com/fwlink/?linkid=618336&clcid=0x409).</span><span class="sxs-lookup"><span data-stu-id="e6ce7-109">Media Encoder Standard is configured using one of the encoder presets described [here](http://go.microsoft.com/fwlink/?linkid=618336&clcid=0x409).</span></span>

<span data-ttu-id="e6ce7-110">Det rekommenderas att du alltid koda källfilerna till en MP4-uppsättningen med anpassad bithastighet och sedan konvertera uppsättningen till önskade format med hjälp av den [dynamisk paketering](media-services-dynamic-packaging-overview.md).</span><span class="sxs-lookup"><span data-stu-id="e6ce7-110">It is recommended to always encode your source files into an adaptive bitrate MP4 set and then convert the set to the desired format using the [Dynamic Packaging](media-services-dynamic-packaging-overview.md).</span></span> 

<span data-ttu-id="e6ce7-111">Om din utdatatillgången är lagringskrypterad, måste du konfigurera principen för tillgångsleverans.</span><span class="sxs-lookup"><span data-stu-id="e6ce7-111">If your output asset is storage encrypted, you must configure asset delivery policy.</span></span> <span data-ttu-id="e6ce7-112">Mer information finns i [konfigurera tillgångsleveransprincip](media-services-dotnet-configure-asset-delivery-policy.md).</span><span class="sxs-lookup"><span data-stu-id="e6ce7-112">For more information see [Configuring asset delivery policy](media-services-dotnet-configure-asset-delivery-policy.md).</span></span>

> [!NOTE]
> <span data-ttu-id="e6ce7-113">MES producerar en utdatafil med ett namn som innehåller den första 32 tecknen i namnet indatafilen.</span><span class="sxs-lookup"><span data-stu-id="e6ce7-113">MES produces an output file with a name that contains the first 32 characters of the input file name.</span></span> <span data-ttu-id="e6ce7-114">Namnet är baserat på vad som anges i filen förinställda.</span><span class="sxs-lookup"><span data-stu-id="e6ce7-114">The name is based on what is specified in the preset file.</span></span> <span data-ttu-id="e6ce7-115">Till exempel ”FileName”: ”{Basename} _ {Index} {tillägg}”.</span><span class="sxs-lookup"><span data-stu-id="e6ce7-115">For example, "FileName": "{Basename}_{Index}{Extension}".</span></span> <span data-ttu-id="e6ce7-116">{Basename} ersätts av de första 32 tecknen i namnet på filen med indata.</span><span class="sxs-lookup"><span data-stu-id="e6ce7-116">{Basename} is replaced by the first 32 characters of the input file name.</span></span>
> 
> 

### <a name="mes-formats"></a><span data-ttu-id="e6ce7-117">MES format</span><span class="sxs-lookup"><span data-stu-id="e6ce7-117">MES Formats</span></span>
[<span data-ttu-id="e6ce7-118">Format och codec</span><span class="sxs-lookup"><span data-stu-id="e6ce7-118">Formats and codecs</span></span>](media-services-media-encoder-standard-formats.md)

### <a name="mes-presets"></a><span data-ttu-id="e6ce7-119">MES-förinställningar</span><span class="sxs-lookup"><span data-stu-id="e6ce7-119">MES Presets</span></span>
<span data-ttu-id="e6ce7-120">Media Encoder Standard konfigureras med hjälp av en kodare förinställningar beskrivs [här](http://go.microsoft.com/fwlink/?linkid=618336&clcid=0x409).</span><span class="sxs-lookup"><span data-stu-id="e6ce7-120">Media Encoder Standard is configured using one of the encoder presets described [here](http://go.microsoft.com/fwlink/?linkid=618336&clcid=0x409).</span></span>

### <a name="input-and-output-metadata"></a><span data-ttu-id="e6ce7-121">Indata och utdata metadata</span><span class="sxs-lookup"><span data-stu-id="e6ce7-121">Input and output metadata</span></span>
<span data-ttu-id="e6ce7-122">När du kodar en inkommande tillgång (eller tillgångar) med hjälp av MES du hämta ett utdatatillgången vid den lyckas slutförandet av denna koda aktivitet.</span><span class="sxs-lookup"><span data-stu-id="e6ce7-122">When you encode an input asset (or assets) using MES, you get an output asset at the successful completion of that encode task.</span></span> <span data-ttu-id="e6ce7-123">Utdatatillgången innehåller video, ljud, miniatyrer, manifestet, etc. som du använder för kodning.</span><span class="sxs-lookup"><span data-stu-id="e6ce7-123">The output asset contains video, audio, thumbnails, manifest, etc. based on the encoding preset you use.</span></span>

<span data-ttu-id="e6ce7-124">Utdatatillgången innehåller också en fil med metadata om inkommande tillgången.</span><span class="sxs-lookup"><span data-stu-id="e6ce7-124">The output asset also contains a file with metadata about the input asset.</span></span> <span data-ttu-id="e6ce7-125">Namnet på XML-metadatafilen har följande format: < asset_id > _metadata.xml (till exempel 41114ad3-eb5e - 4c 57 8d 92-5354e2b7d4a4_metadata.xml), där < asset_id > är AssetId värdet av inkommande tillgången.</span><span class="sxs-lookup"><span data-stu-id="e6ce7-125">The name of the metadata XML file has the following format: <asset_id>_metadata.xml (for example, 41114ad3-eb5e-4c57-8d92-5354e2b7d4a4_metadata.xml), where <asset_id> is the AssetId value of the input asset.</span></span> <span data-ttu-id="e6ce7-126">Schemat för den här inkommande XML-metadata beskrivs [här](media-services-input-metadata-schema.md).</span><span class="sxs-lookup"><span data-stu-id="e6ce7-126">The schema of this input metadata XML is described [here](media-services-input-metadata-schema.md).</span></span>

<span data-ttu-id="e6ce7-127">Utdatatillgången innehåller också en fil med metadata om utdatatillgången.</span><span class="sxs-lookup"><span data-stu-id="e6ce7-127">The output asset also contains a file with metadata about the output asset.</span></span> <span data-ttu-id="e6ce7-128">Namnet på XML-metadatafilen har följande format: < source_file_name > _manifest.xml (till exempel BigBuckBunny_manifest.xml).</span><span class="sxs-lookup"><span data-stu-id="e6ce7-128">The name of the metadata XML file has the following format: <source_file_name>_manifest.xml (for example, BigBuckBunny_manifest.xml).</span></span> <span data-ttu-id="e6ce7-129">Schemat för dessa utdata metadata XML beskrivs [här](media-services-output-metadata-schema.md).</span><span class="sxs-lookup"><span data-stu-id="e6ce7-129">The schema of this output metadata XML is described [here](media-services-output-metadata-schema.md).</span></span>

<span data-ttu-id="e6ce7-130">Om du vill undersöka antingen två metadatafiler du skapar en SAS-positionerare och hämta filen till den lokala datorn.</span><span class="sxs-lookup"><span data-stu-id="e6ce7-130">If you want to examine either of the two metadata files, you can create a SAS locator and download the file to your local computer.</span></span> <span data-ttu-id="e6ce7-131">Du hittar ett exempel på hur du skapar en SAS-positionerare och hämta en fil med Media Services .NET SDK-tilläggen.</span><span class="sxs-lookup"><span data-stu-id="e6ce7-131">You can find an example on how to create a SAS locator and download a file Using the Media Services .NET SDK Extensions.</span></span>

## <a name="download-sample"></a><span data-ttu-id="e6ce7-132">Hämta exempel</span><span class="sxs-lookup"><span data-stu-id="e6ce7-132">Download sample</span></span>
<span data-ttu-id="e6ce7-133">Du kan hämta och kör ett exempel som visar hur du kodar med MES från [här](https://azure.microsoft.com/documentation/samples/media-services-dotnet-on-demand-encoding-with-media-encoder-standard/).</span><span class="sxs-lookup"><span data-stu-id="e6ce7-133">You can get and run a sample that shows how to encode with MES from [here](https://azure.microsoft.com/documentation/samples/media-services-dotnet-on-demand-encoding-with-media-encoder-standard/).</span></span>

## <a name="net-sample-code"></a><span data-ttu-id="e6ce7-134">Exempelkod för .NET</span><span class="sxs-lookup"><span data-stu-id="e6ce7-134">.NET sample code</span></span>

<span data-ttu-id="e6ce7-135">Följande kodexempel använder Media Services .NET SDK för att utföra följande uppgifter:</span><span class="sxs-lookup"><span data-stu-id="e6ce7-135">The following code example uses Media Services .NET SDK to perform the following tasks:</span></span>

* <span data-ttu-id="e6ce7-136">Skapa ett kodningsjobb.</span><span class="sxs-lookup"><span data-stu-id="e6ce7-136">Create an encoding job.</span></span>
* <span data-ttu-id="e6ce7-137">Hämta en referens till Media Encoder Standard-kodare.</span><span class="sxs-lookup"><span data-stu-id="e6ce7-137">Get a reference to the Media Encoder Standard encoder.</span></span>
* <span data-ttu-id="e6ce7-138">Ange om du vill använda den [anpassningsbar strömning](media-services-autogen-bitrate-ladder-with-mes.md) förinställda.</span><span class="sxs-lookup"><span data-stu-id="e6ce7-138">Specify to use the [Adaptive Streaming](media-services-autogen-bitrate-ladder-with-mes.md) preset.</span></span> 
* <span data-ttu-id="e6ce7-139">Lägga till en enskild kodning uppgift i jobbet.</span><span class="sxs-lookup"><span data-stu-id="e6ce7-139">Add a single encoding task to the job.</span></span> 
* <span data-ttu-id="e6ce7-140">Ange indata tillgången ska kodas.</span><span class="sxs-lookup"><span data-stu-id="e6ce7-140">Specify the input asset to be encoded.</span></span>
* <span data-ttu-id="e6ce7-141">Skapa en utdata tillgång som innehåller den kodade tillgången.</span><span class="sxs-lookup"><span data-stu-id="e6ce7-141">Create an output asset that will contain the encoded asset.</span></span>
* <span data-ttu-id="e6ce7-142">Lägga till en händelsehanterare för att kontrollera jobbförloppet.</span><span class="sxs-lookup"><span data-stu-id="e6ce7-142">Add an event handler to check the job progress.</span></span>
* <span data-ttu-id="e6ce7-143">Skicka jobbet.</span><span class="sxs-lookup"><span data-stu-id="e6ce7-143">Submit the job.</span></span>

#### <a name="create-and-configure-a-visual-studio-project"></a><span data-ttu-id="e6ce7-144">Skapa och konfigurera ett Visual Studio-projekt</span><span class="sxs-lookup"><span data-stu-id="e6ce7-144">Create and configure a Visual Studio project</span></span>

<span data-ttu-id="e6ce7-145">Konfigurera utvecklingsmiljön och fyll i filen app.config med anslutningsinformation, enligt beskrivningen i [Media Services-utveckling med .NET](media-services-dotnet-how-to-use.md).</span><span class="sxs-lookup"><span data-stu-id="e6ce7-145">Set up your development environment and populate the app.config file with connection information, as described in [Media Services development with .NET](media-services-dotnet-how-to-use.md).</span></span> 

#### <a name="example"></a><span data-ttu-id="e6ce7-146">Exempel</span><span class="sxs-lookup"><span data-stu-id="e6ce7-146">Example</span></span> 

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

                    // Encode and generate the output using the "Adaptive Streaming" preset.
                    EncodeToAdaptiveBitrateMP4Set(asset);

                    Console.ReadLine();
                }

                static public IAsset EncodeToAdaptiveBitrateMP4Set(IAsset asset)
                {
                    // Declare a new job.
                    IJob job = _context.Jobs.Create("Media Encoder Standard Job");
                    // Get a media processor reference, and pass to it the name of the 
                    // processor to use for the specific task.
                    IMediaProcessor processor = GetLatestMediaProcessorByName("Media Encoder Standard");

                    // Create a task with the encoding details, using a string preset.
                    // In this case "Adaptive Streaming" preset is used.
                    ITask task = job.Tasks.AddNew("My encoding task",
                        processor,
                        "Adaptive Streaming",
                        TaskOptions.None);

                    // Specify the input asset to be encoded.
                    task.InputAssets.Add(asset);
                    // Add an output asset to contain the results of the job. 
                    // This output is specified as AssetCreationOptions.None, which 
                    // means the output asset is not encrypted. 
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

## <a name="media-services-learning-paths"></a><span data-ttu-id="e6ce7-147">Sökvägar för Media Services-utbildning</span><span class="sxs-lookup"><span data-stu-id="e6ce7-147">Media Services learning paths</span></span>
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a><span data-ttu-id="e6ce7-148">Ge feedback</span><span class="sxs-lookup"><span data-stu-id="e6ce7-148">Provide feedback</span></span>
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

## <a name="next-steps"></a><span data-ttu-id="e6ce7-149">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="e6ce7-149">Next steps</span></span>
<span data-ttu-id="e6ce7-150">[Så här genererar du miniatyrbild med Media Encoder Standard med .NET](media-services-dotnet-generate-thumbnail-with-mes.md)
[Media Services Encoding: översikt](media-services-encode-asset.md)</span><span class="sxs-lookup"><span data-stu-id="e6ce7-150">[How to generate thumbnail using Media Encoder Standard with .NET](media-services-dotnet-generate-thumbnail-with-mes.md)
[Media Services Encoding Overview](media-services-encode-asset.md)</span></span>

