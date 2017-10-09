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
# <a name="encode-an-asset-with-media-encoder-standard-using-net"></a>Koda en tillgång med Media Encoder Standard med hjälp av .NET
Kodning jobb är en hello vanligaste bearbetningen i Media Services. Du skapar kodning jobb tooconvert media-filer från en kodning tooanother. När du kodar kan du använda hello Media Services inbyggda Media Encoder. Du kan också använda en kodare som tillhandahålls av Media Services partner; kodare från tredje part är tillgängliga via hello Azure Marketplace. 

Det här avsnittet visar hur toouse .NET tooencode dina tillgångar med Media Encoder Standard (MES). Media Encoder Standard konfigureras med hjälp av en hello kodare förinställningar beskrivs [här](http://go.microsoft.com/fwlink/?linkid=618336&clcid=0x409).

Rekommenderas tooalways koda källfilerna till en MP4-uppsättningen med anpassad bithastighet och sedan konvertera hello set toohello önskade format med hjälp av hello [dynamisk paketering](media-services-dynamic-packaging-overview.md). 

Om din utdatatillgången är lagringskrypterad, måste du konfigurera principen för tillgångsleverans. Mer information finns i [konfigurera tillgångsleveransprincip](media-services-dotnet-configure-asset-delivery-policy.md).

> [!NOTE]
> MES ger en utdatafil med ett namn som innehåller hello först 32 tecken för hello indatafilens namn. hello namn baserat på vad som anges i hello förvalda. Till exempel ”FileName”: ”{Basename} _ {Index} {tillägg}”. {Basename} har ersatts av hello först 32 tecken hello indatafilen namn.
> 
> 

### <a name="mes-formats"></a>MES format
[Format och codec](media-services-media-encoder-standard-formats.md)

### <a name="mes-presets"></a>MES-förinställningar
Media Encoder Standard konfigureras med hjälp av en hello kodare förinställningar beskrivs [här](http://go.microsoft.com/fwlink/?linkid=618336&clcid=0x409).

### <a name="input-and-output-metadata"></a>Indata och utdata metadata
När du kodar en inkommande tillgång (eller tillgångar) med hjälp av MES du får en utdatatillgången på hello koda slutförd som aktiviteten. Hej utdatatillgången innehåller video, ljud, miniatyrer, manifestet, etc. baserat på hello kodning förinställning som du använder.

Hej utdatatillgången innehåller också en fil med metadata om hello inkommande tillgången. hello namnet på XML-filen för hello metadata har hello följande format: < asset_id > _metadata.xml (till exempel 41114ad3-eb5e - 4c 57 8d 92-5354e2b7d4a4_metadata.xml), där < asset_id > är hello AssetId värde av hello inkommande tillgång. hello schemat för den här inkommande XML-metadata beskrivs [här](media-services-input-metadata-schema.md).

Hej utdatatillgången innehåller också en fil med metadata om hello utdatatillgången. hello namnet på XML-filen för hello metadata har hello följande format: < source_file_name > _manifest.xml (till exempel BigBuckBunny_manifest.xml). hello schemat för dessa utdata metadata XML beskrivs [här](media-services-output-metadata-schema.md).

Om du vill tooexamine antingen hello två metadatafiler, kan du skapa en SAS-positionerare och hämta hello filen tooyour lokal dator. Du hittar ett exempel på hur toocreate en SAS-positionerare och hämta en fil med hjälp av hello Media Services .NET SDK-tillägg.

## <a name="download-sample"></a>Hämta exempel
Du kan hämta och kör ett exempel som visar hur tooencode med MES från [här](https://azure.microsoft.com/documentation/samples/media-services-dotnet-on-demand-encoding-with-media-encoder-standard/).

## <a name="net-sample-code"></a>Exempelkod för .NET

Följande kodexempel hello använder Media Services .NET SDK tooperform hello följande uppgifter:

* Skapa ett kodningsjobb.
* Hämta en referens toohello Media Encoder Standard-kodare.
* Ange toouse hello [anpassningsbar strömning](media-services-autogen-bitrate-ladder-with-mes.md) förinställda. 
* Lägg till en enskild uppgift toohello kodningsjobbet. 
* Ange hello indata tillgången toobe kodad.
* Skapa en utdata tillgång som innehåller hello kodade tillgången.
* Lägg till en händelse hanteraren toocheck hello jobb pågår.
* Skicka hello-jobbet.

#### <a name="create-and-configure-a-visual-studio-project"></a>Skapa och konfigurera ett Visual Studio-projekt

Konfigurera utvecklingsmiljön och fylla hello app.config-fil med anslutningsinformation, enligt beskrivningen i [Media Services-utveckling med .NET](media-services-dotnet-how-to-use.md). 

#### <a name="example"></a>Exempel 

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

## <a name="media-services-learning-paths"></a>Sökvägar för Media Services-utbildning
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Ge feedback
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

## <a name="next-steps"></a>Nästa steg
[Hur toogenerate miniatyr med Media Encoder Standard med .NET](media-services-dotnet-generate-thumbnail-with-mes.md)
[Media Services Encoding: översikt](media-services-encode-asset.md)

