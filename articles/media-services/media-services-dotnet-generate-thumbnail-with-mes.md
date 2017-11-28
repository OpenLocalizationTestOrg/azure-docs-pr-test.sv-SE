---
title: aaaHow toogenerate miniatyrer med Media Encoder Standard med .NET
description: "Det här avsnittet visar hur toouse .NET tooencode en tillgång och generera miniatyrbilder på hello samtidigt med Media Encoder Standard."
services: media-services
documentationcenter: 
author: juliako
manager: cfowler
editor: 
ms.assetid: b8dab73a-1d91-4b6d-9741-a92ad39fc3f7
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/14/2017
ms.author: juliako
ms.openlocfilehash: 23d3e4d9bf64a688d45499c045f19d2792167990
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="how-toogenerate-thumbnails-using-media-encoder-standard-with-net"></a><span data-ttu-id="c7a1c-103">Hur toogenerate miniatyrer med Media Encoder Standard med .NET</span><span class="sxs-lookup"><span data-stu-id="c7a1c-103">How toogenerate thumbnails using Media Encoder Standard with .NET</span></span>

<span data-ttu-id="c7a1c-104">Du kan använda Media Encoder Standard toogenerate en eller flera miniatyrer från din videoinmatning i [JPEG](https://en.wikipedia.org/wiki/JPEG), [PNG](https://en.wikipedia.org/wiki/Portable_Network_Graphics), eller [BMP](https://en.wikipedia.org/wiki/BMP_file_format) filer.</span><span class="sxs-lookup"><span data-stu-id="c7a1c-104">You can use Media Encoder Standard toogenerate one or more thumbnails from your input video in [JPEG](https://en.wikipedia.org/wiki/JPEG), [PNG](https://en.wikipedia.org/wiki/Portable_Network_Graphics), or [BMP](https://en.wikipedia.org/wiki/BMP_file_format) image file formats.</span></span> <span data-ttu-id="c7a1c-105">Du kan skicka uppgifter som genererar endast bilder eller kan du kombinera miniatyr generation kodning.</span><span class="sxs-lookup"><span data-stu-id="c7a1c-105">You can submit Tasks that produce only images, or you can combine thumbnail generation with encoding.</span></span> <span data-ttu-id="c7a1c-106">Det här avsnittet innehåller några exempel XML och JSON miniatyr förinställningar för dessa scenarier.</span><span class="sxs-lookup"><span data-stu-id="c7a1c-106">This topic provides a few sample XML and JSON thumbnail presets for such scenarios.</span></span> <span data-ttu-id="c7a1c-107">Hello slutet av hello-avsnittet, finns en [exempelkoden](#code_sample) som visar hur toouse hello Media Services .NET SDK tooaccomplish hello kodning aktivitet.</span><span class="sxs-lookup"><span data-stu-id="c7a1c-107">At hello end of hello topic, there is a [sample code](#code_sample) that shows how toouse hello Media Services .NET SDK tooaccomplish hello encoding task.</span></span>

<span data-ttu-id="c7a1c-108">Mer information om hello-element som används i exemplet förinställningar bör du granska [Media Encoder Standard schemat](media-services-mes-schema.md).</span><span class="sxs-lookup"><span data-stu-id="c7a1c-108">For more details on hello elements that are used in sample presets, you should review [Media Encoder Standard schema](media-services-mes-schema.md).</span></span>

<span data-ttu-id="c7a1c-109">Se till att tooreview hello [överväganden](media-services-dotnet-generate-thumbnail-with-mes.md#considerations) avsnitt.</span><span class="sxs-lookup"><span data-stu-id="c7a1c-109">Make sure tooreview hello [Considerations](media-services-dotnet-generate-thumbnail-with-mes.md#considerations) section.</span></span>

## <a name="example--single-png-file"></a><span data-ttu-id="c7a1c-110">Exempel – PNG-fil</span><span class="sxs-lookup"><span data-stu-id="c7a1c-110">Example – single PNG file</span></span>

<span data-ttu-id="c7a1c-111">Hej följande JSON och XML-förinställda kan vara används tooproduce som ett enda utflöde PNG-fil utanför hello några sekunder hello inkommande video, där hello encoder finns det en bästa försöket att hitta en ”intressant” ram.</span><span class="sxs-lookup"><span data-stu-id="c7a1c-111">hello following JSON and XML preset can be used tooproduce a single output PNG file out of hello first few seconds of hello input video, where hello encoder makes a best-effort attempt at finding an “interesting” frame.</span></span> <span data-ttu-id="c7a1c-112">Observera att bildmåtten för hello utdata har ställts in too100%, vilket innebär att de matchar hello dimensioner för hello indatavideo.</span><span class="sxs-lookup"><span data-stu-id="c7a1c-112">Note that hello output image dimensions have been set too100%, meaning these will match hello dimensions of hello input video.</span></span> <span data-ttu-id="c7a1c-113">Observera också hur hello ”Format” i ”utdata” är obligatoriska toomatch hello användning av ”PngLayers” hello ”codec” under.</span><span class="sxs-lookup"><span data-stu-id="c7a1c-113">Note also how hello “Format” setting in “Outputs” is required toomatch hello use of “PngLayers” in hello “Codecs” section.</span></span> 

### <a name="json-preset"></a><span data-ttu-id="c7a1c-114">JSON förinställda</span><span class="sxs-lookup"><span data-stu-id="c7a1c-114">JSON preset</span></span>

    {
      "Version": 1.0,
      "Codecs": [
        {
          "PngLayers": [
            {
              "Type": "PngLayer",
              "Width": "100%",
              "Height": "100%"
            }
          ],
          "Start": "{Best}",
          "Type": "PngImage"
        }
      ],
      "Outputs": [
        {
          "FileName": "{Basename}_{Index}{Extension}",
          "Format": {
            "Type": "PngFormat"
          }
        }
      ]
    }
    
### <a name="xml-preset"></a><span data-ttu-id="c7a1c-115">XML-förinställda</span><span class="sxs-lookup"><span data-stu-id="c7a1c-115">XML preset</span></span>

    <?xml version="1.0" encoding="utf-16"?>
    <Preset xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" Version="1.0" xmlns="http://www.windowsazure.com/media/encoding/Preset/2014/03">
      <Encoding>
        <PngImage Start="{Best}">
          <PngLayers>
            <PngLayer>
              <Width>100%</Width>
              <Height>100%</Height>
            </PngLayer>
          </PngLayers>
        </PngImage>
      </Encoding>
      <Outputs>
        <Output FileName="{Basename}_{Index}{Extension}">
          <PngFormat />
        </Output>
      </Outputs>
    </Preset>

## <a name="example--a-series-of-jpeg-images"></a><span data-ttu-id="c7a1c-116">Exempel – en serie JPEG-bilder</span><span class="sxs-lookup"><span data-stu-id="c7a1c-116">Example – a series of JPEG images</span></span>

<span data-ttu-id="c7a1c-117">Hej följande JSON och XML-förinställda kan vara används tooproduce en uppsättning 10 bilder på tidsstämplar 5% 15%,..., 95% av hello inkommande tidslinje, där hello avbildningens storlek är angivna toobe ett kvartal som hello indata video.</span><span class="sxs-lookup"><span data-stu-id="c7a1c-117">hello following JSON and XML preset can be used tooproduce a set of 10 images at timestamps of 5%, 15%, …, 95% of hello input timeline, where hello image size is specified toobe one quarter that of hello input video.</span></span>

### <a name="json-preset"></a><span data-ttu-id="c7a1c-118">JSON förinställda</span><span class="sxs-lookup"><span data-stu-id="c7a1c-118">JSON preset</span></span>

    {
      "Version": 1.0,
      "Codecs": [
        {
          "JpgLayers": [
            {
              "Quality": 90,
              "Type": "JpgLayer",
              "Width": "25%",
              "Height": "25%"
            }
          ],
          "Start": "5%",
          "Step": "1",
          "Range": "1",
          "Type": "JpgImage"
        }
      ],
      "Outputs": [
        {
          "FileName": "{Basename}_{Index}{Extension}",
          "Format": {
            "Type": "JpgFormat"
          }
        }
      ]
    }

### <a name="xml-preset"></a><span data-ttu-id="c7a1c-119">XML-förinställda</span><span class="sxs-lookup"><span data-stu-id="c7a1c-119">XML preset</span></span>
    
    <?xml version="1.0" encoding="utf-16"?>
    <Preset xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" Version="1.0" xmlns="http://www.windowsazure.com/media/encoding/Preset/2014/03">
      <Encoding>
        <JpgImage Start="5%" Step="10%" Range="96%">
          <JpgLayers>
            <JpgLayer>
              <Width>25%</Width>
              <Height>25%</Height>
              <Quality>90</Quality>
            </JpgLayer>
          </JpgLayers>
        </JpgImage>
      </Encoding>
      <Outputs>
        <Output FileName="{Basename}_{Index}{Extension}">
          <JpgFormat />
        </Output>
      </Outputs>
    </Preset>

## <a name="example--one-image-at-a-specific-timestamp"></a><span data-ttu-id="c7a1c-120">Exempel – en avbildning på en specifik tidsstämpel</span><span class="sxs-lookup"><span data-stu-id="c7a1c-120">Example – one image at a specific timestamp</span></span>

<span data-ttu-id="c7a1c-121">hello följande JSON- och XML förinställda kan vara används tooproduce en JPEG-bild på hello 30 sekunder Markera hello inkommande video.</span><span class="sxs-lookup"><span data-stu-id="c7a1c-121">hello following JSON and XML preset can be used tooproduce a single JPEG image at hello 30 second mark of hello input video.</span></span> <span data-ttu-id="c7a1c-122">Den här förinställningen förväntar hello inkommande toobe mer än 30 sekunder varaktighet (annan hello-jobb misslyckas).</span><span class="sxs-lookup"><span data-stu-id="c7a1c-122">This preset expects hello input toobe more than 30 seconds in duration (else hello job will fail).</span></span>

### <a name="json-preset"></a><span data-ttu-id="c7a1c-123">JSON förinställda</span><span class="sxs-lookup"><span data-stu-id="c7a1c-123">JSON preset</span></span>

    {
      "Version": 1.0,
      "Codecs": [
        {
          "JpgLayers": [
            {
              "Quality": 90,
              "Type": "JpgLayer",
              "Width": "25%",
              "Height": "25%"
            }
          ],
          "Start": "00:00:30",
          "Step": "1",
          "Range": "1",
          "Type": "JpgImage"
        }
      ],
      "Outputs": [
        {
          "FileName": "{Basename}_{Index}{Extension}",
          "Format": {
            "Type": "JpgFormat"
          }
        }
      ]
    }
    
### <a name="xml-preset"></a><span data-ttu-id="c7a1c-124">XML-förinställda</span><span class="sxs-lookup"><span data-stu-id="c7a1c-124">XML preset</span></span>
    
    <?xml version="1.0" encoding="utf-16"?>
    <Preset xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" Version="1.0" xmlns="http://www.windowsazure.com/media/encoding/Preset/2014/03">
      <Encoding>
        <JpgImage Start="00:00:30" Step="00:00:02" Range="00:00:01">
          <JpgLayers>
            <JpgLayer>
              <Width>25%</Width>
              <Height>25%</Height>
              <Quality>90</Quality>
            </JpgLayer>
          </JpgLayers>
        </JpgImage>
      </Encoding>
      <Outputs>
        <Output FileName="{Basename}_{Index}{Extension}">
          <JpgFormat />
        </Output>
      </Outputs>
    </Preset>

## <span data-ttu-id="c7a1c-125"><a id="code_sample"></a>Exempel – koda video och generera miniatyr</span><span class="sxs-lookup"><span data-stu-id="c7a1c-125"><a id="code_sample"></a>Example – encode video and generate thumbnail</span></span>

<span data-ttu-id="c7a1c-126">Följande kodexempel hello använder Media Services .NET SDK tooperform hello följande uppgifter:</span><span class="sxs-lookup"><span data-stu-id="c7a1c-126">hello following code example uses Media Services .NET SDK tooperform hello following tasks:</span></span>

* <span data-ttu-id="c7a1c-127">Skapa ett kodningsjobb.</span><span class="sxs-lookup"><span data-stu-id="c7a1c-127">Create an encoding job.</span></span>
* <span data-ttu-id="c7a1c-128">Hämta en referens toohello Media Encoder Standard-kodare.</span><span class="sxs-lookup"><span data-stu-id="c7a1c-128">Get a reference toohello Media Encoder Standard encoder.</span></span>
* <span data-ttu-id="c7a1c-129">Läs in hello förinställningen [XML](media-services-dotnet-generate-thumbnail-with-mes.md#xml) eller [JSON](media-services-dotnet-generate-thumbnail-with-mes.md#json) som innehåller hello encoding förinställt samt information som behövs för toogenerate miniatyrerna.</span><span class="sxs-lookup"><span data-stu-id="c7a1c-129">Load hello preset [XML](media-services-dotnet-generate-thumbnail-with-mes.md#xml) or [JSON](media-services-dotnet-generate-thumbnail-with-mes.md#json) that contain hello encoding preset as well as information needed toogenerate thumbnails.</span></span> <span data-ttu-id="c7a1c-130">Du kan spara den [XML](media-services-dotnet-generate-thumbnail-with-mes.md#xml) eller [JSON](media-services-dotnet-generate-thumbnail-with-mes.md#json) i en fil och använda hello följande kod tooload hello.</span><span class="sxs-lookup"><span data-stu-id="c7a1c-130">You can save this  [XML](media-services-dotnet-generate-thumbnail-with-mes.md#xml) or [JSON](media-services-dotnet-generate-thumbnail-with-mes.md#json) in a file and use hello following code tooload hello file.</span></span>
  
        // Load hello XML (or JSON) from hello local file.
        string configuration = File.ReadAllText(fileName);  
* <span data-ttu-id="c7a1c-131">Lägg till en enskild uppgift toohello kodningsjobbet.</span><span class="sxs-lookup"><span data-stu-id="c7a1c-131">Add a single encoding task toohello job.</span></span> 
* <span data-ttu-id="c7a1c-132">Ange hello indata tillgången toobe kodad.</span><span class="sxs-lookup"><span data-stu-id="c7a1c-132">Specify hello input asset toobe encoded.</span></span>
* <span data-ttu-id="c7a1c-133">Skapa en utdata tillgång som innehåller hello kodade tillgången.</span><span class="sxs-lookup"><span data-stu-id="c7a1c-133">Create an output asset that will contain hello encoded asset.</span></span>
* <span data-ttu-id="c7a1c-134">Lägg till en händelse hanteraren toocheck hello jobb pågår.</span><span class="sxs-lookup"><span data-stu-id="c7a1c-134">Add an event handler toocheck hello job progress.</span></span>
* <span data-ttu-id="c7a1c-135">Skicka hello-jobbet.</span><span class="sxs-lookup"><span data-stu-id="c7a1c-135">Submit hello job.</span></span>

<span data-ttu-id="c7a1c-136">Se hello [Media Services-utveckling med .NET](media-services-dotnet-how-to-use.md) avsnittet för instruktioner om hur tooset in din utvecklingsmiljö.</span><span class="sxs-lookup"><span data-stu-id="c7a1c-136">See hello [Media Services development with .NET](media-services-dotnet-how-to-use.md) topic for directions on how tooset up your dev environment.</span></span>

        using System;
        using System.Configuration;
        using System.IO;
        using System.Linq;
        using Microsoft.WindowsAzure.MediaServices.Client;
        using System.Threading;

        namespace EncodeAndGenerateThumbnails
        {
        class Program
        {
            // Read values from hello App.config file.
            private static readonly string _AADTenantDomain =
            ConfigurationManager.AppSettings["AADTenantDomain"];
            private static readonly string _RESTAPIEndpoint =
            ConfigurationManager.AppSettings["MediaServiceRESTAPIEndpoint"];

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

            // Encode and generate hello thumbnails.
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
            string configuration = File.ReadAllText("ThumbnailPreset_JSON.json");

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

## <span data-ttu-id="c7a1c-137"><a id="json"></a>Miniatyrer JSON förinställda</span><span class="sxs-lookup"><span data-stu-id="c7a1c-137"><a id="json"></a>Thumbnail JSON preset</span></span>
<span data-ttu-id="c7a1c-138">Mer information om schemat finns [detta](https://msdn.microsoft.com/library/mt269962.aspx) avsnittet.</span><span class="sxs-lookup"><span data-stu-id="c7a1c-138">For information about schema, see [this](https://msdn.microsoft.com/library/mt269962.aspx) topic.</span></span>

    {
      "Version": 1.0,
      "Codecs": [
        {
          "KeyFrameInterval": "00:00:02",
          "SceneChangeDetection": "true",
          "H264Layers": [
            {
              "Profile": "Auto",
              "Level": "auto",
              "Bitrate": 4500,
              "MaxBitrate": 4500,
              "BufferWindow": "00:00:05",
              "Width": 1280,
              "Height": 720,
              "ReferenceFrames": 3,
              "EntropyMode": "Cabac",
              "AdaptiveBFrame": true,
              "Type": "H264Layer",
              "FrameRate": "0/1"
    
            }
          ],
          "Type": "H264Video"
        },
        {
          "JpgLayers": [
            {
              "Quality": 90,
              "Type": "JpgLayer",
              "Width": "100%",
              "Height": "100%"
            }
          ],
          "Start": "{Best}",
          "Type": "JpgImage"
        },
        {
          "Channels": 2,
          "SamplingRate": 48000,
          "Bitrate": 128,
          "Type": "AACAudio"
        }
      ],
      "Outputs": [
        {
          "FileName": "{Basename}_{Index}{Extension}",
          "Format": {
            "Type": "JpgFormat"
          }
        },
        {
          "FileName": "{Basename}_{Resolution}_{VideoBitrate}.mp4",
          "Format": {
            "Type": "MP4Format"
          }
        }
      ]
    }

## <span data-ttu-id="c7a1c-139"><a id="xml"></a>Miniatyrer XML förinställda</span><span class="sxs-lookup"><span data-stu-id="c7a1c-139"><a id="xml"></a>Thumbnail XML preset</span></span>
<span data-ttu-id="c7a1c-140">Mer information om schemat finns [detta](https://msdn.microsoft.com/library/mt269962.aspx) avsnittet.</span><span class="sxs-lookup"><span data-stu-id="c7a1c-140">For information about schema, see [this](https://msdn.microsoft.com/library/mt269962.aspx) topic.</span></span>
    
    <?xml version="1.0" encoding="utf-16"?>
    <Preset xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" Version="1.0" xmlns="http://www.windowsazure.com/media/encoding/Preset/2014/03">
      <Encoding>
        <H264Video>
          <KeyFrameInterval>00:00:02</KeyFrameInterval>
          <SceneChangeDetection>true</SceneChangeDetection>
          <H264Layers>
            <H264Layer>
              <Bitrate>4500</Bitrate>
              <Width>1280</Width>
              <Height>720</Height>
              <FrameRate>0/1</FrameRate>
              <Profile>Auto</Profile>
              <Level>auto</Level>
              <BFrames>3</BFrames>
              <ReferenceFrames>3</ReferenceFrames>
              <Slices>0</Slices>
              <AdaptiveBFrame>true</AdaptiveBFrame>
              <EntropyMode>Cabac</EntropyMode>
              <BufferWindow>00:00:05</BufferWindow>
              <MaxBitrate>4500</MaxBitrate>
            </H264Layer>
          </H264Layers>
        </H264Video>
        <AACAudio>
          <Profile>AACLC</Profile>
          <Channels>2</Channels>
          <SamplingRate>48000</SamplingRate>
          <Bitrate>128</Bitrate>
        </AACAudio>
        <JpgImage Start="{Best}">
          <JpgLayers>
            <JpgLayer>
              <Width>100%</Width>
              <Height>100%/Height>
              <Quality>90</Quality>
            </JpgLayer>
          </JpgLayers>
        </JpgImage>
      </Encoding>
      <Outputs>
        <Output FileName="{Basename}_{Resolution}_{VideoBitrate}.mp4">
          <MP4Format />
        </Output>
        <Output FileName="{Basename}_{Index}{Extension}">
          <JpgFormat />
        </Output>
      </Outputs>
    </Preset>

## <a name="considerations"></a><span data-ttu-id="c7a1c-141">Överväganden</span><span class="sxs-lookup"><span data-stu-id="c7a1c-141">Considerations</span></span>
<span data-ttu-id="c7a1c-142">det gäller hello följande överväganden:</span><span class="sxs-lookup"><span data-stu-id="c7a1c-142">hello following considerations apply:</span></span>

* <span data-ttu-id="c7a1c-143">hello användningen av explicita tidsstämplar för Startintervall-steg förutsätter att hello Indatakällan är minst 1 minut.</span><span class="sxs-lookup"><span data-stu-id="c7a1c-143">hello use of explicit timestamps for Start/Step/Range assumes that hello input source is at least 1 minute long.</span></span>
* <span data-ttu-id="c7a1c-144">BmpImage-jpg/Png-element har Start, steg och intervallet string attribut – dessa kan tolkas som:</span><span class="sxs-lookup"><span data-stu-id="c7a1c-144">Jpg/Png/BmpImage elements have Start, Step and Range string attributes – these can be interpreted as:</span></span>
  
  * <span data-ttu-id="c7a1c-145">Ramens nummer om de är icke-negativa heltal, t.ex.</span><span class="sxs-lookup"><span data-stu-id="c7a1c-145">Frame Number if they are non-negative integers, eg.</span></span> <span data-ttu-id="c7a1c-146">”Start”: ”120”</span><span class="sxs-lookup"><span data-stu-id="c7a1c-146">"Start": "120",</span></span>
  * <span data-ttu-id="c7a1c-147">Relativa toosource varaktighet om uttryckt %-suffixet, t.ex.</span><span class="sxs-lookup"><span data-stu-id="c7a1c-147">Relative toosource duration if expressed as %-suffixed, eg.</span></span> <span data-ttu-id="c7a1c-148">”Start”: ”15%”, eller</span><span class="sxs-lookup"><span data-stu-id="c7a1c-148">"Start": "15%", OR</span></span>
  * <span data-ttu-id="c7a1c-149">Tidsstämpel om uttryckt: mm: ss...</span><span class="sxs-lookup"><span data-stu-id="c7a1c-149">Timestamp if expressed as HH:MM:SS…</span></span> <span data-ttu-id="c7a1c-150">format.</span><span class="sxs-lookup"><span data-stu-id="c7a1c-150">format.</span></span> <span data-ttu-id="c7a1c-151">T.ex.</span><span class="sxs-lookup"><span data-stu-id="c7a1c-151">Eg.</span></span> <span data-ttu-id="c7a1c-152">”Start” ”: 00: 01:00”</span><span class="sxs-lookup"><span data-stu-id="c7a1c-152">"Start" : "00:01:00"</span></span>
    
    <span data-ttu-id="c7a1c-153">Du kan blanda och matcha här ska du ange.</span><span class="sxs-lookup"><span data-stu-id="c7a1c-153">You can mix and match notations as you please.</span></span>
    
    <span data-ttu-id="c7a1c-154">Dessutom Start har också stöd för ett särskilt makro: {bästa}, som försöker toodetermine hello första ”intressant” bildrutan hello innehåll Obs: (steg och intervallet ignoreras när Start är inställd för {bästa})</span><span class="sxs-lookup"><span data-stu-id="c7a1c-154">Additionally, Start also supports a special Macro:{Best}, which attempts toodetermine hello first “interesting” frame of hello content NOTE: (Step and Range are ignored when Start is set too{Best})</span></span>
  * <span data-ttu-id="c7a1c-155">Standardvärden: Starta: {bästa}</span><span class="sxs-lookup"><span data-stu-id="c7a1c-155">Defaults: Start:{Best}</span></span>
* <span data-ttu-id="c7a1c-156">Utdataformat måste uttryckligen anges för varje bildformat toobe: BmpFormat-Jpg/Png.</span><span class="sxs-lookup"><span data-stu-id="c7a1c-156">Output format needs toobe explicitly provided for each Image format: Jpg/Png/BmpFormat.</span></span> <span data-ttu-id="c7a1c-157">När MES matchar JpgVideo tooJpgFormat och så vidare.</span><span class="sxs-lookup"><span data-stu-id="c7a1c-157">When present, MES will match JpgVideo tooJpgFormat and so on.</span></span> <span data-ttu-id="c7a1c-158">OutputFormat introducerar ett nytt specifika bilden codec makro: {Index}, som måste toobe finns (en gång och bara en gång) för bildformat för utdata.</span><span class="sxs-lookup"><span data-stu-id="c7a1c-158">OutputFormat introduces a new image-codec specific Macro: {Index}, which needs toobe present (once and only once) for image output formats.</span></span>

## <a name="next-steps"></a><span data-ttu-id="c7a1c-159">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="c7a1c-159">Next steps</span></span>

<span data-ttu-id="c7a1c-160">Du kan kontrollera hello [jobb pågår](media-services-check-job-progress.md) när hello kodningsjobbet väntar.</span><span class="sxs-lookup"><span data-stu-id="c7a1c-160">You can check hello [job progress](media-services-check-job-progress.md) while hello encoding job is pending.</span></span>

## <a name="media-services-learning-paths"></a><span data-ttu-id="c7a1c-161">Sökvägar för Media Services-utbildning</span><span class="sxs-lookup"><span data-stu-id="c7a1c-161">Media Services learning paths</span></span>
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a><span data-ttu-id="c7a1c-162">Ge feedback</span><span class="sxs-lookup"><span data-stu-id="c7a1c-162">Provide feedback</span></span>
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

## <a name="see-also"></a><span data-ttu-id="c7a1c-163">Se även</span><span class="sxs-lookup"><span data-stu-id="c7a1c-163">See Also</span></span>
[<span data-ttu-id="c7a1c-164">Media Services Encoding översikt</span><span class="sxs-lookup"><span data-stu-id="c7a1c-164">Media Services Encoding Overview</span></span>](media-services-encode-asset.md)

