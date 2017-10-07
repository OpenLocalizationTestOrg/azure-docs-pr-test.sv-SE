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
#  <a name="use-azure-media-encoder-standard-tooauto-generate-a-bitrate-ladder"></a>Använd Azure Media Encoder Standard tooauto-Generera en bithastighet stege

## <a name="overview"></a>Översikt

Det här avsnittet visar hur toouse Media Encoder Standard (MES) tooauto-Generera en bithastighet stege (bithastighet upplösning par) baserat på hello inkommande upplösning och bithastighet. hello kan autogenererade förinställda inte överstiga hello indata upplösning och bithastighet. Till exempel om hello-indata är 720p på 3 Mbit/s, utdata kommer förblir 720p i bästa och startar priser som är lägre än 3 Mbit/s.

### <a name="encoding-for-streaming-only"></a>Kodning för strömning endast

Om din avsikt är tooencode förinställda källfilerna video endast för strömning, bör du använda hello ”anpassningsbar strömning” när du skapar en kodning aktivitet. När du använder hello **anpassningsbar strömning** förinställts hello MES kodare kommer Intelligent cap stege en bithastighet. Du kommer inte att kunna toocontrol hello kodning kostnader, eftersom hello bestämmer hur många lager toouse och med vilken upplösning. Du kan se ett exempel på utdata-lager som produceras av MES på grund av encoding med hello **anpassningsbar strömning** förinställningen hello slutet av det här avsnittet. hello utdata tillgång innehåller MP4-filer där ljud och video inte är överlagrat.

### <a name="encoding-for-streaming-and-progressive-download"></a>Kodning för strömning och progressiv hämtning

Om din avsikt är tooencode förinställda videon källa för direktuppspelning samt tooproduce MP4-filer för progressiv nedladdning av du bör använda hello ”innehåll anpassningsbar flera bithastighet MP4” när du skapar en kodning aktivitet. När du använder hello **innehåll anpassningsbar flera bithastighet MP4** förinställts hello MES kodare gäller hello samma kodning logik som ovan, men nu hello utdatatillgången innehåller MP4-filer där ljud och video är överlagrad. Du kan använda någon av dessa MP4-filer (till exempel hello högsta bithastighet version) som en fil för progressiv nedladdning.

## <a id="encoding_with_dotnet"></a>Encoding med Media Services .NET SDK

Följande kodexempel hello använder Media Services .NET SDK tooperform hello följande uppgifter:

- Skapa ett kodningsjobb.
- Hämta en referens toohello Media Encoder Standard-kodare.
- Lägg till en aktivitet toohello kodningsjobbet och ange toouse hello **anpassningsbar strömning** förinställda. 
- Skapa en utdata tillgång som innehåller hello kodade tillgången.
- Lägg till en händelse hanteraren toocheck hello jobb pågår.
- Skicka hello-jobbet.

#### <a name="create-and-configure-a-visual-studio-project"></a>Skapa och konfigurera ett Visual Studio-projekt

Konfigurera utvecklingsmiljön och fylla hello app.config-fil med anslutningsinformation, enligt beskrivningen i [Media Services-utveckling med .NET](media-services-dotnet-how-to-use.md). 

#### <a name="example"></a>Exempel

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

## <a id="output"></a>Utdata

Det här avsnittet visar tre exempel på utdata-lager som produceras av MES på grund av encoding med hello **anpassningsbar strömning** förinställda. 

### <a name="example-1"></a>Exempel 1
Källa med höjd ”1080” och ramhastighet ”29.970” ger 6 video lager:

|Lager|Höjd|Bredd|Bitrate(kbps)|
|---|---|---|---|
|1|1080|1920|6780|
|2|720|1280|3520|
|3|540|960|2210|
|4|360|640|1150|
|5|270|480|720|
|6|180|320|380|

### <a name="example-2"></a>Exempel 2
Källa med höjd ”720” och ramhastighet ”23.970” ger 5 video lager:

|Lager|Höjd|Bredd|Bitrate(kbps)|
|---|---|---|---|
|1|720|1280|2940|
|2|540|960|1850|
|3|360|640|960|
|4|270|480|600|
|5|180|320|320|

### <a name="example-3"></a>Exempel 3
Källa med höjd ”360” och ramhastighet ”29.970” ger 3 video lager:

|Lager|Höjd|Bredd|Bitrate(kbps)|
|---|---|---|---|
|1|360|640|700|
|2|270|480|440|
|3|180|320|230|
## <a name="media-services-learning-paths"></a>Sökvägar för Media Services-utbildning
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Ge feedback
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

## <a name="see-also"></a>Se även
[Media Services Encoding översikt](media-services-encode-asset.md)

