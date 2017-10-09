---
title: "aaaPerform avancerade kodning genom att anpassa MES förinställningar | Microsoft Docs"
description: "Det här avsnittet visar hur tooperform avancerade kodning genom att anpassa Media Encoder Standard uppgiften förinställningar."
services: media-services
documentationcenter: 
author: juliako
manager: cfowler
editor: 
ms.assetid: 2a4ade25-e600-4bce-a66e-e29cf4a38369
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/01/2017
ms.author: juliako
ms.openlocfilehash: 9caa68fafacaf51f91f0554c5bafe491928d8c77
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="perform-advanced-encoding-by-customizing-mes-presets"></a><span data-ttu-id="26ce8-103">Utföra avancerade encoding genom att anpassa MES förinställningar</span><span class="sxs-lookup"><span data-stu-id="26ce8-103">Perform advanced encoding by customizing MES presets</span></span> 

## <a name="overview"></a><span data-ttu-id="26ce8-104">Översikt</span><span class="sxs-lookup"><span data-stu-id="26ce8-104">Overview</span></span>

<span data-ttu-id="26ce8-105">Det här avsnittet visar hur toocustomize Media Encoder Standard förinställningar.</span><span class="sxs-lookup"><span data-stu-id="26ce8-105">This topic shows how toocustomize Media Encoder Standard presets.</span></span> <span data-ttu-id="26ce8-106">Hej [Encoding med Media Encoder Standard med hjälp av anpassade förinställningar](media-services-custom-mes-presets-with-dotnet.md) avsnittet visas hur toouse .NET toocreate kodning uppgift och ett jobb som utför den här uppgiften.</span><span class="sxs-lookup"><span data-stu-id="26ce8-106">hello [Encoding with Media Encoder Standard using custom presets](media-services-custom-mes-presets-with-dotnet.md) topic shows how toouse .NET toocreate an encoding task and a job that executes this task.</span></span> <span data-ttu-id="26ce8-107">När du anpassar en förinställning, ange hello anpassade förinställningar toohello kodning aktivitet.</span><span class="sxs-lookup"><span data-stu-id="26ce8-107">Once you customize a preset, supply hello custom presets toohello encoding task.</span></span> 

>[!NOTE]
><span data-ttu-id="26ce8-108">Om du använder en XML-förinställning, göra att toopreserve hello elementordning, som visas i XML-exempel (till exempel KeyFrameInterval måste föregå SceneChangeDetection).</span><span class="sxs-lookup"><span data-stu-id="26ce8-108">If using an XML preset, make sure toopreserve hello order of elements, as shown in XML samples below (for example, KeyFrameInterval should precede SceneChangeDetection).</span></span>
>

<span data-ttu-id="26ce8-109">I det här avsnittet hello anpassade förinställningar som utför hello efter kodning uppgifter har visat.</span><span class="sxs-lookup"><span data-stu-id="26ce8-109">In this topic, hello custom presets that perform hello following encoding tasks are demonstrated.</span></span>

## <a name="support-for-relative-sizes"></a><span data-ttu-id="26ce8-110">Stöd för relativa storleken</span><span class="sxs-lookup"><span data-stu-id="26ce8-110">Support for relative sizes</span></span>

<span data-ttu-id="26ce8-111">När du genererar miniatyrer, behöver du inte ange tooalways utdata bredd och höjd i bildpunkter.</span><span class="sxs-lookup"><span data-stu-id="26ce8-111">When generating thumbnails, you do not need tooalways specify output width and height in pixels.</span></span> <span data-ttu-id="26ce8-112">Du kan ange dem i procent i hello intervallet [% 1,..., 100%].</span><span class="sxs-lookup"><span data-stu-id="26ce8-112">You can specify them in percentages, in hello range [1%, …, 100%].</span></span>

### <a name="json-preset"></a><span data-ttu-id="26ce8-113">JSON förinställda</span><span class="sxs-lookup"><span data-stu-id="26ce8-113">JSON preset</span></span>
    "Width": "100%",
    "Height": "100%"

### <a name="xml-preset"></a><span data-ttu-id="26ce8-114">XML-förinställda</span><span class="sxs-lookup"><span data-stu-id="26ce8-114">XML preset</span></span>
    <Width>100%</Width>
    <Height>100%</Height>

## <span data-ttu-id="26ce8-115"><a id="thumbnails"></a>Generera miniatyrbilder</span><span class="sxs-lookup"><span data-stu-id="26ce8-115"><a id="thumbnails"></a>Generate thumbnails</span></span>

<span data-ttu-id="26ce8-116">Det här avsnittet visas hur toocustomize en förinställning som genererar miniatyrer.</span><span class="sxs-lookup"><span data-stu-id="26ce8-116">This section shows how toocustomize a preset that generates thumbnails.</span></span> <span data-ttu-id="26ce8-117">hello innehåller förinställning som anges nedan information om hur du vill att tooencode filen samt information som behövs för toogenerate miniatyrerna.</span><span class="sxs-lookup"><span data-stu-id="26ce8-117">hello preset defined below contains information on how you want tooencode your file as well as information needed toogenerate thumbnails.</span></span> <span data-ttu-id="26ce8-118">Du kan vidta någon av hello MES förinställningar dokumenterade [detta](media-services-mes-presets-overview.md) och lägger till kod som genererar miniatyrer.</span><span class="sxs-lookup"><span data-stu-id="26ce8-118">You can take any of hello MES presets documented [this](media-services-mes-presets-overview.md) section and add code that generates thumbnails.</span></span>  

> [!NOTE]
> <span data-ttu-id="26ce8-119">Hej **SceneChangeDetection** inställningen i hello följande förinställda kan endast anges tootrue om du kodar tooa enkel bithastighet video.</span><span class="sxs-lookup"><span data-stu-id="26ce8-119">hello **SceneChangeDetection** setting in hello following preset can only be set tootrue if you are encoding tooa single  bitrate video.</span></span> <span data-ttu-id="26ce8-120">Om du kodar tooa multibithastighet video och ange **SceneChangeDetection** tootrue, hello kodare returnerar ett fel.</span><span class="sxs-lookup"><span data-stu-id="26ce8-120">If you are encoding tooa multi-bitrate video and set **SceneChangeDetection** tootrue, hello encoder returns an error.</span></span>  
>
>

<span data-ttu-id="26ce8-121">Mer information om schemat finns [detta](media-services-mes-schema.md) avsnittet.</span><span class="sxs-lookup"><span data-stu-id="26ce8-121">For information about schema, see [this](media-services-mes-schema.md) topic.</span></span>

<span data-ttu-id="26ce8-122">Se till att tooreview hello [överväganden](#considerations) avsnitt.</span><span class="sxs-lookup"><span data-stu-id="26ce8-122">Make sure tooreview hello [Considerations](#considerations) section.</span></span>

### <span data-ttu-id="26ce8-123"><a id="json"></a>JSON förinställda</span><span class="sxs-lookup"><span data-stu-id="26ce8-123"><a id="json"></a>JSON preset</span></span>
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
              "Width": 640,
              "Height": 360
            }
          ],
          "Start": "{Best}",
          "Type": "JpgImage"
        },
        {
          "PngLayers": [
            {
              "Type": "PngLayer",
              "Width": 640,
              "Height": 360,
            }
          ],
          "Start": "00:00:01",
          "Step": "00:00:10",
          "Range": "00:00:58",
          "Type": "PngImage"
        },
        {
          "BmpLayers": [
            {
              "Type": "BmpLayer",
              "Width": 640,
              "Height": 360
            }
          ],
          "Start": "10%",
          "Step": "10%",
          "Range": "90%",
          "Type": "BmpImage"
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
          "FileName": "{Basename}_{Index}{Extension}",
          "Format": {
            "Type": "PngFormat"
          }
        },
        {
          "FileName": "{Basename}_{Index}{Extension}",
          "Format": {
            "Type": "BmpFormat"
          }
        },
        {
          "FileName": "{Basename}_{Width}x{Height}_{VideoBitrate}.mp4",
          "Format": {
            "Type": "MP4Format"
          }
        }
      ]
    }


### <span data-ttu-id="26ce8-124"><a id="xml"></a>XML-förinställda</span><span class="sxs-lookup"><span data-stu-id="26ce8-124"><a id="xml"></a>XML preset</span></span>
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
              <Width>640</Width>
              <Height>360</Height>
              <Quality>90</Quality>
            </JpgLayer>
          </JpgLayers>
        </JpgImage>
        <BmpImage Start="10%" Step="10%" Range="90%">
          <BmpLayers>
            <BmpLayer>
              <Width>640</Width>
              <Height>360</Height>
            </BmpLayer>
          </BmpLayers>
        </BmpImage>
        <PngImage Start="00:00:01" Step="00:00:10" Range="00:00:58">
          <PngLayers>
            <PngLayer>
              <Width>640</Width>
              <Height>360</Height>
            </PngLayer>
          </PngLayers>
        </PngImage>
      </Encoding>
      <Outputs>
        <Output FileName="{Basename}_{Width}x{Height}_{VideoBitrate}.mp4">
          <MP4Format />
        </Output>
        <Output FileName="{Basename}_{Index}{Extension}">
          <JpgFormat />
        </Output>
        <Output FileName="{Basename}_{Index}{Extension}">
          <BmpFormat />
        </Output>
        <Output FileName="{Basename}_{Index}{Extension}">
          <PngFormat />
        </Output>
      </Outputs>
    </Preset>

### <a name="considerations"></a><span data-ttu-id="26ce8-125">Överväganden</span><span class="sxs-lookup"><span data-stu-id="26ce8-125">Considerations</span></span>

<span data-ttu-id="26ce8-126">det gäller hello följande överväganden:</span><span class="sxs-lookup"><span data-stu-id="26ce8-126">hello following considerations apply:</span></span>

* <span data-ttu-id="26ce8-127">hello användningen av explicita tidsstämplar för Startintervall-steg förutsätter att hello Indatakällan är minst 1 minut.</span><span class="sxs-lookup"><span data-stu-id="26ce8-127">hello use of explicit timestamps for Start/Step/Range assumes that hello input source is at least 1 minute long.</span></span>
* <span data-ttu-id="26ce8-128">BmpImage-jpg/Png-element har Start, steg, och intervallet strängattribut – dessa kan tolkas som:</span><span class="sxs-lookup"><span data-stu-id="26ce8-128">Jpg/Png/BmpImage elements have Start, Step, and Range string attributes – these can be interpreted as:</span></span>

  * <span data-ttu-id="26ce8-129">RAM-numret om de är icke-negativa heltal, till exempel ”Start”: ”120”</span><span class="sxs-lookup"><span data-stu-id="26ce8-129">Frame Number if they are non-negative integers, for example "Start": "120",</span></span>
  * <span data-ttu-id="26ce8-130">Relativa toosource varaktighet om uttryckt %-suffixet, till exempel ”Start”: ”15%”, eller</span><span class="sxs-lookup"><span data-stu-id="26ce8-130">Relative toosource duration if expressed as %-suffixed, for example "Start": "15%", OR</span></span>
  * <span data-ttu-id="26ce8-131">Tidsstämpel om uttryckt: mm: ss...</span><span class="sxs-lookup"><span data-stu-id="26ce8-131">Timestamp if expressed as HH:MM:SS…</span></span> <span data-ttu-id="26ce8-132">Formatera, till exempel ”Start” ”: 00: 01:00”</span><span class="sxs-lookup"><span data-stu-id="26ce8-132">format, for example "Start" : "00:01:00"</span></span>

    <span data-ttu-id="26ce8-133">Du kan blanda och matcha här ska du ange.</span><span class="sxs-lookup"><span data-stu-id="26ce8-133">You can mix and match notations as you please.</span></span>

    <span data-ttu-id="26ce8-134">Dessutom Start har också stöd för ett särskilt makro: {bästa}, som försöker toodetermine hello första ”intressant” bildrutan hello innehåll Obs: (steg och intervallet ignoreras när Start är inställd för {bästa})</span><span class="sxs-lookup"><span data-stu-id="26ce8-134">Additionally, Start also supports a special Macro:{Best}, which attempts toodetermine hello first “interesting” frame of hello content NOTE: (Step and Range are ignored when Start is set too{Best})</span></span>
  * <span data-ttu-id="26ce8-135">Standardvärden: Starta: {bästa}</span><span class="sxs-lookup"><span data-stu-id="26ce8-135">Defaults: Start:{Best}</span></span>
* <span data-ttu-id="26ce8-136">Utdataformat måste uttryckligen anges för varje bildformat toobe: BmpFormat-Jpg/Png.</span><span class="sxs-lookup"><span data-stu-id="26ce8-136">Output format needs toobe explicitly provided for each Image format: Jpg/Png/BmpFormat.</span></span> <span data-ttu-id="26ce8-137">När matchar MES JpgVideo tooJpgFormat och så vidare.</span><span class="sxs-lookup"><span data-stu-id="26ce8-137">When present, MES matches JpgVideo tooJpgFormat and so on.</span></span> <span data-ttu-id="26ce8-138">OutputFormat introducerar ett nytt specifika bilden codec makro: {Index}, som måste toobe finns (en gång och bara en gång) för bildformat för utdata.</span><span class="sxs-lookup"><span data-stu-id="26ce8-138">OutputFormat introduces a new image-codec specific Macro: {Index}, which needs toobe present (once and only once) for image output formats.</span></span>

## <span data-ttu-id="26ce8-139"><a id="trim_video"></a>Ta bort en video (urklippet)</span><span class="sxs-lookup"><span data-stu-id="26ce8-139"><a id="trim_video"></a>Trim a video (clipping)</span></span>
<span data-ttu-id="26ce8-140">Det här avsnittet pratar om hur du ändrar hello kodare förinställningar tooclip eller trim hello indatavideo där hello-indata är en s.k. mezzaninfil eller på begäran-fil.</span><span class="sxs-lookup"><span data-stu-id="26ce8-140">This section talks about modifying hello encoder presets tooclip or trim hello input video where hello input is a so-called mezzanine file or on-demand file.</span></span> <span data-ttu-id="26ce8-141">hello kodare kan också använda tooclip eller rensa en tillgång, som har hämtats eller arkiverade från en direktsänd dataström – hello information för detta finns i [bloggen](https://azure.microsoft.com/blog/sub-clipping-and-live-archive-extraction-with-media-encoder-standard/).</span><span class="sxs-lookup"><span data-stu-id="26ce8-141">hello encoder can also be used tooclip or trim an asset, which is captured or archived from a live stream – hello details for this are available in [this blog](https://azure.microsoft.com/blog/sub-clipping-and-live-archive-extraction-with-media-encoder-standard/).</span></span>

<span data-ttu-id="26ce8-142">tootrim filmer, du kan vidta någon av hello MES förinställningar dokumenterade [detta](media-services-mes-presets-overview.md) avsnittet och ändra hello **källor** element (som visas nedan).</span><span class="sxs-lookup"><span data-stu-id="26ce8-142">tootrim your videos, you can take any of hello MES presets documented [this](media-services-mes-presets-overview.md) section and modify hello **Sources** element (as shown below).</span></span> <span data-ttu-id="26ce8-143">hello-värdet för StartTime måste toomatch hello absolut tidsstämplar hello inkommande video.</span><span class="sxs-lookup"><span data-stu-id="26ce8-143">hello value of StartTime needs toomatch hello absolute timestamps of hello input video.</span></span> <span data-ttu-id="26ce8-144">Till exempel om hello första bildrutan i hello indatavideo har en tidsstämpel för 12:00:10.000 och StartTime bör vara minst 12:00:10.000 och större.</span><span class="sxs-lookup"><span data-stu-id="26ce8-144">For example, if hello first frame of hello input video has a timestamp of 12:00:10.000, then StartTime should be at least 12:00:10.000 and greater.</span></span> <span data-ttu-id="26ce8-145">I hello exemplet nedan förutsätter att hello videoinmatning har en första tidsstämpel noll.</span><span class="sxs-lookup"><span data-stu-id="26ce8-145">In hello example below, we assume that hello input video has a starting timestamp of zero.</span></span> <span data-ttu-id="26ce8-146">**Källor** ska placeras hello början av hello förinställda.</span><span class="sxs-lookup"><span data-stu-id="26ce8-146">**Sources** should be placed at hello beginning of hello preset.</span></span>

### <span data-ttu-id="26ce8-147"><a id="json"></a>JSON förinställda</span><span class="sxs-lookup"><span data-stu-id="26ce8-147"><a id="json"></a>JSON preset</span></span>
    {
      "Version": 1.0,
      "Sources": [
        {
          "StartTime": "00:00:04",
          "Duration": "00:00:16"
        }
      ],
      "Codecs": [
        {
          "KeyFrameInterval": "00:00:02",
          "StretchMode": "AutoSize",
          "H264Layers": [
            {
              "Profile": "Auto",
              "Level": "auto",
              "Bitrate": 3400,
              "MaxBitrate": 3400,
              "BufferWindow": "00:00:05",
              "Width": 1280,
              "Height": 720,
              "BFrames": 3,
              "ReferenceFrames": 3,
              "AdaptiveBFrame": true,
              "Type": "H264Layer",
              "FrameRate": "0/1"
            },
            {
              "Profile": "Auto",
              "Level": "auto",
              "Bitrate": 2250,
              "MaxBitrate": 2250,
              "BufferWindow": "00:00:05",
              "Width": 960,
              "Height": 540,
              "BFrames": 3,
              "ReferenceFrames": 3,
              "AdaptiveBFrame": true,
              "Type": "H264Layer",
              "FrameRate": "0/1"
            },
            {
              "Profile": "Auto",
              "Level": "auto",
              "Bitrate": 1500,
              "MaxBitrate": 1500,
              "BufferWindow": "00:00:05",
              "Width": 960,
              "Height": 540,
              "BFrames": 3,
              "ReferenceFrames": 3,
              "AdaptiveBFrame": true,
              "Type": "H264Layer",
              "FrameRate": "0/1"
            },
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

### <a name="xml-preset"></a><span data-ttu-id="26ce8-148">XML-förinställda</span><span class="sxs-lookup"><span data-stu-id="26ce8-148">XML preset</span></span>
<span data-ttu-id="26ce8-149">tootrim filmer, du kan vidta någon av hello MES förinställningar dokumenterade [här](media-services-mes-presets-overview.md) och ändra hello **källor** element (som visas nedan).</span><span class="sxs-lookup"><span data-stu-id="26ce8-149">tootrim your videos, you can take any of hello MES presets documented [here](media-services-mes-presets-overview.md) and modify hello **Sources** element (as shown below).</span></span>

    <?xml version="1.0" encoding="utf-16"?>
    <Preset xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" Version="1.0" xmlns="http://www.windowsazure.com/media/encoding/Preset/2014/03">
      <Sources>
        <Source StartTime="PT4S" Duration="PT14S"/>
      </Sources>
      <Encoding>
        <H264Video>
          <KeyFrameInterval>00:00:02</KeyFrameInterval>
          <H264Layers>
            <H264Layer>
              <Bitrate>3400</Bitrate>
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
              <MaxBitrate>3400</MaxBitrate>
            </H264Layer>
            <H264Layer>
              <Bitrate>2250</Bitrate>
              <Width>960</Width>
              <Height>540</Height>
              <FrameRate>0/1</FrameRate>
              <Profile>Auto</Profile>
              <Level>auto</Level>
              <BFrames>3</BFrames>
              <ReferenceFrames>3</ReferenceFrames>
              <Slices>0</Slices>
              <AdaptiveBFrame>true</AdaptiveBFrame>
              <EntropyMode>Cabac</EntropyMode>
              <BufferWindow>00:00:05</BufferWindow>
              <MaxBitrate>2250</MaxBitrate>
            </H264Layer>
            <H264Layer>
              <Bitrate>1500</Bitrate>
              <Width>960</Width>
              <Height>540</Height>
              <FrameRate>0/1</FrameRate>
              <Profile>Auto</Profile>
              <Level>auto</Level>
              <BFrames>3</BFrames>
              <ReferenceFrames>3</ReferenceFrames>
              <Slices>0</Slices>
              <AdaptiveBFrame>true</AdaptiveBFrame>
              <EntropyMode>Cabac</EntropyMode>
              <BufferWindow>00:00:05</BufferWindow>
              <MaxBitrate>1500</MaxBitrate>
            </H264Layer>
            <H264Layer>
              <Bitrate>1000</Bitrate>
              <Width>640</Width>
              <Height>360</Height>
              <FrameRate>0/1</FrameRate>
              <Profile>Auto</Profile>
              <Level>auto</Level>
              <BFrames>3</BFrames>
              <ReferenceFrames>3</ReferenceFrames>
              <Slices>0</Slices>
              <AdaptiveBFrame>true</AdaptiveBFrame>
              <EntropyMode>Cabac</EntropyMode>
              <BufferWindow>00:00:05</BufferWindow>
              <MaxBitrate>1000</MaxBitrate>
            </H264Layer>
            <H264Layer>
              <Bitrate>650</Bitrate>
              <Width>640</Width>
              <Height>360</Height>
              <FrameRate>0/1</FrameRate>
              <Profile>Auto</Profile>
              <Level>auto</Level>
              <BFrames>3</BFrames>
              <ReferenceFrames>3</ReferenceFrames>
              <Slices>0</Slices>
              <AdaptiveBFrame>true</AdaptiveBFrame>
              <EntropyMode>Cabac</EntropyMode>
              <BufferWindow>00:00:05</BufferWindow>
              <MaxBitrate>650</MaxBitrate>
            </H264Layer>
            <H264Layer>
              <Bitrate>400</Bitrate>
              <Width>320</Width>
              <Height>180</Height>
              <FrameRate>0/1</FrameRate>
              <Profile>Auto</Profile>
              <Level>auto</Level>
              <BFrames>3</BFrames>
              <ReferenceFrames>3</ReferenceFrames>
              <Slices>0</Slices>
              <AdaptiveBFrame>true</AdaptiveBFrame>
              <EntropyMode>Cabac</EntropyMode>
              <BufferWindow>00:00:05</BufferWindow>
              <MaxBitrate>400</MaxBitrate>
            </H264Layer>
          </H264Layers>
        </H264Video>
        <AACAudio>
          <Profile>AACLC</Profile>
          <Channels>2</Channels>
          <SamplingRate>48000</SamplingRate>
          <Bitrate>128</Bitrate>
        </AACAudio>
      </Encoding>
      <Outputs>
        <Output FileName="{Basename}_{Width}x{Height}_{VideoBitrate}.mp4">
          <MP4Format />
        </Output>
      </Outputs>
    </Preset>

## <span data-ttu-id="26ce8-150"><a id="overlay"></a>Skapa ett överlägg</span><span class="sxs-lookup"><span data-stu-id="26ce8-150"><a id="overlay"></a>Create an overlay</span></span>

<span data-ttu-id="26ce8-151">hello Media Encoder Standard kan du toooverlay en bild till en befintlig video.</span><span class="sxs-lookup"><span data-stu-id="26ce8-151">hello Media Encoder Standard allows you toooverlay an image onto an existing video.</span></span> <span data-ttu-id="26ce8-152">För närvarande hello följande format stöds: png, jpg, gif, bmp och.</span><span class="sxs-lookup"><span data-stu-id="26ce8-152">Currently, hello following formats are supported: png, jpg, gif, and bmp.</span></span> <span data-ttu-id="26ce8-153">hello är förinställning som anges nedan ett grundläggande exempel på en videoöverlägg.</span><span class="sxs-lookup"><span data-stu-id="26ce8-153">hello preset defined below is a basic example  of a video overlay.</span></span>

<span data-ttu-id="26ce8-154">Dessutom toodefining en förinställd fil du har också toolet Media Services vet vilken fil i hello tillgången är hello överlägget avbildningen och vilken fil är hello källa video där du vill toooverlay hello bilden.</span><span class="sxs-lookup"><span data-stu-id="26ce8-154">In addition toodefining a preset file, you also have toolet Media Services know which file in hello asset is hello overlay image and which file is hello source video onto which you want toooverlay hello image.</span></span> <span data-ttu-id="26ce8-155">hello videofil har toobe hello **primära** fil.</span><span class="sxs-lookup"><span data-stu-id="26ce8-155">hello video file has toobe hello **primary** file.</span></span>

<span data-ttu-id="26ce8-156">Om du använder .NET, lägger du till hello följande två funktioner toohello .NET-exempel som definierats i [detta](media-services-custom-mes-presets-with-dotnet.md#encoding_with_dotnet) avsnittet.</span><span class="sxs-lookup"><span data-stu-id="26ce8-156">If you are using .NET, add hello following two functions toohello .NET example defined in [this](media-services-custom-mes-presets-with-dotnet.md#encoding_with_dotnet) topic.</span></span> <span data-ttu-id="26ce8-157">Hej **UploadMediaFilesFromFolder** funktionen Överför filer från en mapp (till exempel BigBuckBunny.mp4 och Image001.png) och anger hello mp4 toobe hello primära fil i hello tillgången.</span><span class="sxs-lookup"><span data-stu-id="26ce8-157">hello **UploadMediaFilesFromFolder** function uploads files from a folder (for example, BigBuckBunny.mp4 and Image001.png) and sets hello mp4 file toobe hello primary file in hello asset.</span></span> <span data-ttu-id="26ce8-158">Hej **EncodeWithOverlay** använder funktionen hello anpassade förvalda som skickades tooit (till exempel hello förinställningen som följer) toocreate hello kodning aktivitet.</span><span class="sxs-lookup"><span data-stu-id="26ce8-158">hello **EncodeWithOverlay** function uses hello custom preset file that was passed tooit (for example, hello preset that follows) toocreate hello encoding task.</span></span>


    static public IAsset UploadMediaFilesFromFolder(string folderPath)
    {
        IAsset asset = _context.Assets.CreateFromFolder(folderPath, AssetCreationOptions.None);
    
        foreach (var af in asset.AssetFiles)
        {
            // hello following code assumes 
            // you have an input folder with one MP4 and one overlay image file.
            if (af.Name.Contains(".mp4"))
                af.IsPrimary = true;
            else
                af.IsPrimary = false;
    
            af.Update();
        }
    
        return asset;
    }

    static public IAsset EncodeWithOverlay(IAsset assetSource, string customPresetFileName)
    {
        // Declare a new job.
        IJob job = _context.Jobs.Create("Media Encoder Standard Job");
        // Get a media processor reference, and pass tooit hello name of hello 
        // processor toouse for hello specific task.
        IMediaProcessor processor = GetLatestMediaProcessorByName("Media Encoder Standard");

        // Load hello XML (or JSON) from hello local file.
        string configuration = File.ReadAllText(customPresetFileName);

        // Create a task
        ITask task = job.Tasks.AddNew("Media Encoder Standard encoding task",
            processor,
            configuration,
            TaskOptions.None);

        // Specify hello input assets toobe encoded.
        // This asset contains a source file and an overlay file.
        task.InputAssets.Add(assetSource);

        // Add an output asset toocontain hello results of hello job. 
        task.OutputAssets.AddNew("Output asset",
            AssetCreationOptions.None);

        job.StateChanged += new EventHandler<JobStateChangedEventArgs>(JobStateChanged);
        job.Submit();
        job.GetExecutionProgressTask(CancellationToken.None).Wait();

        return job.OutputMediaAssets[0];
    }


> [!NOTE]
> <span data-ttu-id="26ce8-159">Aktuella begränsningar:</span><span class="sxs-lookup"><span data-stu-id="26ce8-159">Current limitations:</span></span>
>
> <span data-ttu-id="26ce8-160">hello överlägget opacitetsinställningen stöds inte.</span><span class="sxs-lookup"><span data-stu-id="26ce8-160">hello overlay opacity setting is not supported.</span></span>
>
> <span data-ttu-id="26ce8-161">Din video källfilen och hello överlägget image-filen har toobe i hello samma tillgång och hello videofil behov toobe set som hello primära fil i tillgången.</span><span class="sxs-lookup"><span data-stu-id="26ce8-161">Your source video file and hello overlay image file have toobe in hello same asset, and hello video file needs toobe set as hello primary file in this asset.</span></span>
>
>

### <a name="json-preset"></a><span data-ttu-id="26ce8-162">JSON förinställda</span><span class="sxs-lookup"><span data-stu-id="26ce8-162">JSON preset</span></span>
    {
      "Version": 1.0,
      "Sources": [
        {
          "Streams": [],
          "Filters": {
            "VideoOverlay": {
              "Position": {
                "X": 100,
                "Y": 100,
                "Width": 100,
                "Height": 50
              },
              "AudioGainLevel": 0.0,
              "MediaParams": [
                {
                  "OverlayLoopCount": 1
                },
                {
                  "IsOverlay": true,
                  "OverlayLoopCount": 1,
                  "InputLoop": true
                }
              ],
              "Source": "Image001.png",
              "Clip": {
                "Duration": "00:00:05"
              },
              "FadeInDuration": {
                "Duration": "00:00:01"
              },
              "FadeOutDuration": {
                "StartTime": "00:00:03",
                "Duration": "00:00:04"
              }
            }
          },
          "Pad": true
        }
      ],
      "Codecs": [
        {
          "KeyFrameInterval": "00:00:02",
          "H264Layers": [
            {
              "Profile": "Auto",
              "Level": "auto",
              "Bitrate": 1045,
              "MaxBitrate": 1045,
              "BufferWindow": "00:00:05",
              "ReferenceFrames": 3,
              "EntropyMode": "Cavlc",
              "AdaptiveBFrame": true,
              "Type": "H264Layer",
              "Width": "640",
              "Height": "360",
              "FrameRate": "0/1"
            }
          ],
          "Type": "H264Video"
        },
        {
          "Type": "CopyAudio"
        }
      ],
      "Outputs": [
        {
          "FileName": "{Basename}{Extension}",
          "Format": {
            "Type": "MP4Format"
          }
        }
      ]
    }


### <a name="xml-preset"></a><span data-ttu-id="26ce8-163">XML-förinställda</span><span class="sxs-lookup"><span data-stu-id="26ce8-163">XML preset</span></span>
    <?xml version="1.0" encoding="utf-16"?>
    <Preset xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" Version="1.0" xmlns="http://www.windowsazure.com/media/encoding/Preset/2014/03">
      <Sources>
        <Source>
          <Streams />
          <Filters>
            <VideoOverlay>
              <Source>Image001.png</Source>
              <Clip Duration="PT5S" />
              <FadeInDuration Duration="PT1S" />
              <FadeOutDuration StartTime="PT3S" Duration="PT4S" />
              <Position X="100" Y="100" Width="100" Height="50" />
              <Opacity>0</Opacity>
              <AudioGainLevel>0</AudioGainLevel>
              <MediaParams>
                <MediaParam>
                  <IsOverlay>false</IsOverlay>
                  <OverlayLoopCount>1</OverlayLoopCount>
                  <InputLoop>false</InputLoop>
                </MediaParam>
                <MediaParam>
                  <IsOverlay>true</IsOverlay>
                  <OverlayLoopCount>1</OverlayLoopCount>
                  <InputLoop>true</InputLoop>
                </MediaParam>
              </MediaParams>
            </VideoOverlay>
          </Filters>
          <Pad>true</Pad>
        </Source>
      </Sources>
      <Encoding>
        <H264Video>
          <KeyFrameInterval>00:00:02</KeyFrameInterval>
          <H264Layers>
            <H264Layer>
              <Bitrate>1045</Bitrate>
              <Width>640</Width>
              <Height>360</Height>
              <FrameRate>0/1</FrameRate>
              <Profile>Auto</Profile>
              <Level>auto</Level>
              <BFrames>0</BFrames>
              <ReferenceFrames>3</ReferenceFrames>
              <Slices>0</Slices>
              <AdaptiveBFrame>true</AdaptiveBFrame>
              <EntropyMode>Cavlc</EntropyMode>
              <BufferWindow>00:00:05</BufferWindow>
              <MaxBitrate>1045</MaxBitrate>
            </H264Layer>
          </H264Layers>
        </H264Video>
        <CopyAudio />
      </Encoding>
      <Outputs>
        <Output FileName="{Basename}{Extension}">
          <MP4Format />
        </Output>
      </Outputs>
    </Preset>


## <span data-ttu-id="26ce8-164"><a id="silent_audio"></a>Infoga en tyst ljud spåra när indata har inget ljud</span><span class="sxs-lookup"><span data-stu-id="26ce8-164"><a id="silent_audio"></a>Insert a silent audio track when input has no audio</span></span>
<span data-ttu-id="26ce8-165">Som standard om du skickar en inkommande toohello kodare som bara innehåller video, och inget ljud innehåller hello utdatatillgången filer som innehåller endast video data.</span><span class="sxs-lookup"><span data-stu-id="26ce8-165">By default, if you send an input toohello encoder that contains only video, and no audio, then hello output asset contains files that contain only video data.</span></span> <span data-ttu-id="26ce8-166">Vissa spelare kanske inte toohandle sådana utdataströmmar.</span><span class="sxs-lookup"><span data-stu-id="26ce8-166">Some players may not be able toohandle such output streams.</span></span> <span data-ttu-id="26ce8-167">Du kan använda den här inställningen tooforce hello kodare tooadd en tyst ljud spåra toohello utdata i scenariot.</span><span class="sxs-lookup"><span data-stu-id="26ce8-167">You can use this setting tooforce hello encoder tooadd a silent audio track toohello output in that scenario.</span></span>

<span data-ttu-id="26ce8-168">tooforce hello kodare tooproduce en tillgång som innehåller en tyst ljud spåra när indata har inget ljud ange hello ”InsertSilenceIfNoAudio” värde.</span><span class="sxs-lookup"><span data-stu-id="26ce8-168">tooforce hello encoder tooproduce an asset that contains a silent audio track when input has no audio, specify hello "InsertSilenceIfNoAudio" value.</span></span>

<span data-ttu-id="26ce8-169">Du kan vidta någon av hello MES förinställningar dokumenterade i [detta](media-services-mes-presets-overview.md) avsnitt och att Hej ändringar:</span><span class="sxs-lookup"><span data-stu-id="26ce8-169">You can take any of hello MES presets documented in [this](media-services-mes-presets-overview.md) section, and make hello following modification:</span></span>

### <a name="json-preset"></a><span data-ttu-id="26ce8-170">JSON förinställda</span><span class="sxs-lookup"><span data-stu-id="26ce8-170">JSON preset</span></span>
    {
      "Channels": 2,
      "SamplingRate": 44100,
      "Bitrate": 96,
      "Type": "AACAudio",
      "Condition": "InsertSilenceIfNoAudio"
    }

### <a name="xml-preset"></a><span data-ttu-id="26ce8-171">XML-förinställda</span><span class="sxs-lookup"><span data-stu-id="26ce8-171">XML preset</span></span>
    <AACAudio Condition="InsertSilenceIfNoAudio">
      <Channels>2</Channels>
      <SamplingRate>44100</SamplingRate>
      <Bitrate>96</Bitrate>
    </AACAudio>

## <span data-ttu-id="26ce8-172"><a id="deinterlacing"></a>Inaktivera automatisk Frigör sammanflätning</span><span class="sxs-lookup"><span data-stu-id="26ce8-172"><a id="deinterlacing"></a>Disable auto de-interlacing</span></span>
<span data-ttu-id="26ce8-173">Kunder behöver inte toodo något om de liksom hello interlace innehållet toobe automatiskt ej sammanflätad.</span><span class="sxs-lookup"><span data-stu-id="26ce8-173">Customers don’t need toodo anything if they like hello interlace contents toobe automatically de-interlaced.</span></span> <span data-ttu-id="26ce8-174">När hello automatiskt Frigör sammanflätning är aktiverad (standard) hello MES hello automatisk identifiering av sammanflätade ramar och frigör interlaces endast ramar som markerats som sammanflätad.</span><span class="sxs-lookup"><span data-stu-id="26ce8-174">When hello auto de-interlacing is on (default) hello MES does hello auto detection of interlaced frames and only de-interlaces frames marked as interlaced.</span></span>

<span data-ttu-id="26ce8-175">Du kan inaktivera hello automatiskt Frigör sammanflätning.</span><span class="sxs-lookup"><span data-stu-id="26ce8-175">You can turn hello auto de-interlacing off.</span></span> <span data-ttu-id="26ce8-176">Det här alternativet rekommenderas inte.</span><span class="sxs-lookup"><span data-stu-id="26ce8-176">This option is not recommended.</span></span>

### <a name="json-preset"></a><span data-ttu-id="26ce8-177">JSON förinställda</span><span class="sxs-lookup"><span data-stu-id="26ce8-177">JSON preset</span></span>
    "Sources": [
    {
     "Filters": {
        "Deinterlace": {
          "Mode": "Off"
        }
      },
    }
    ]

### <a name="xml-preset"></a><span data-ttu-id="26ce8-178">XML-förinställda</span><span class="sxs-lookup"><span data-stu-id="26ce8-178">XML preset</span></span>
    <Sources>
    <Source>
      <Filters>
        <Deinterlace>
          <Mode>Off</Mode>
        </Deinterlace>
      </Filters>
    </Source>
    </Sources>


## <span data-ttu-id="26ce8-179"><a id="audio_only"></a>Endast ljud förinställningar</span><span class="sxs-lookup"><span data-stu-id="26ce8-179"><a id="audio_only"></a>Audio-only presets</span></span>
<span data-ttu-id="26ce8-180">Det här avsnittet beskrivs två ljuddata MES förinställningar: AAC ljud- och AAC bra kvalitet ljud.</span><span class="sxs-lookup"><span data-stu-id="26ce8-180">This section demonstrates two audio-only MES presets: AAC Audio and AAC Good Quality Audio.</span></span>

### <a name="aac-audio"></a><span data-ttu-id="26ce8-181">AAC ljud</span><span class="sxs-lookup"><span data-stu-id="26ce8-181">AAC Audio</span></span>
    {
      "Version": 1.0,
      "Codecs": [
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
          "FileName": "{Basename}_AAC_{AudioBitrate}.mp4",
          "Format": {
            "Type": "MP4Format"
          }
        }
      ]
    }

### <a name="aac-good-quality-audio"></a><span data-ttu-id="26ce8-182">AAC god kvalitet ljud</span><span class="sxs-lookup"><span data-stu-id="26ce8-182">AAC Good Quality Audio</span></span>
    {
      "Version": 1.0,
      "Codecs": [
        {
          "Profile": "AACLC",
          "Channels": 2,
          "SamplingRate": 48000,
          "Bitrate": 192,
          "Type": "AACAudio"
        }
      ],
      "Outputs": [
        {
          "FileName": "{Basename}_AAC_{AudioBitrate}.mp4",
          "Format": {
            "Type": "MP4Format"
          }
        }
      ]
    }

## <span data-ttu-id="26ce8-183"><a id="concatenate"></a>Sammanfoga två eller flera videofiler</span><span class="sxs-lookup"><span data-stu-id="26ce8-183"><a id="concatenate"></a>Concatenate two or more video files</span></span>

<span data-ttu-id="26ce8-184">hello följande exempel visar hur du kan skapa en förinställd tooconcatenate två eller fler videofiler.</span><span class="sxs-lookup"><span data-stu-id="26ce8-184">hello following example illustrates how you can generate a preset tooconcatenate two or more video files.</span></span> <span data-ttu-id="26ce8-185">hello vanligaste scenariot är när du vill tooadd ett sidhuvud eller en släpvagn toohello huvudsakliga video.</span><span class="sxs-lookup"><span data-stu-id="26ce8-185">hello most common scenario is when you want tooadd a header or a trailer toohello main video.</span></span> <span data-ttu-id="26ce8-186">hello avsedd användning är när hello videofiler redigeras tillsammans resursegenskaper (skärmupplösning, bildfrekvens, ljud spåra antalet osv.).</span><span class="sxs-lookup"><span data-stu-id="26ce8-186">hello intended use is when hello video files being edited together share  properties (video resolution, frame rate, audio track count, etc.).</span></span> <span data-ttu-id="26ce8-187">Noga inte toomix videor olika hastigheterna eller med olika antal ljud spår.</span><span class="sxs-lookup"><span data-stu-id="26ce8-187">You should take care not toomix videos of different frame rates, or with different number of audio tracks.</span></span>

>[!NOTE]
><span data-ttu-id="26ce8-188">hello aktuella designen av hello sammanfogning funktionen förväntar sig att hello indata videoklipp är konsekventa vad gäller upplösning bildfrekvens osv.</span><span class="sxs-lookup"><span data-stu-id="26ce8-188">hello current design of hello concatenation feature expects that hello input video clips are consistent in terms of resolution, frame rate etc.</span></span> 

### <a name="requirements-and-considerations"></a><span data-ttu-id="26ce8-189">Krav och överväganden</span><span class="sxs-lookup"><span data-stu-id="26ce8-189">Requirements and considerations</span></span>

* <span data-ttu-id="26ce8-190">Inkommande videor endast ha ett ljud spår.</span><span class="sxs-lookup"><span data-stu-id="26ce8-190">Input videos should only have one audio track.</span></span>
* <span data-ttu-id="26ce8-191">Ange videor ska ha hello samma bildfrekvens.</span><span class="sxs-lookup"><span data-stu-id="26ce8-191">Input videos should all have hello same frame rate.</span></span>
* <span data-ttu-id="26ce8-192">Du måste överföra videor till separata tillgångar och ange hello videor som hello primärfilen i varje tillgång.</span><span class="sxs-lookup"><span data-stu-id="26ce8-192">You must upload your videos into separate assets and set hello videos as hello primary file in each asset.</span></span>
* <span data-ttu-id="26ce8-193">Du måste tooknow hello varaktighet videor.</span><span class="sxs-lookup"><span data-stu-id="26ce8-193">You need tooknow hello duration of your videos.</span></span>
* <span data-ttu-id="26ce8-194">hello förinställda exemplen nedan förutsätter att alla hello inkommande videor som börjar med en tidsstämpel noll.</span><span class="sxs-lookup"><span data-stu-id="26ce8-194">hello preset examples below assumes that all hello input videos start with a timestamp of zero.</span></span> <span data-ttu-id="26ce8-195">Du behöver toomodify hello StartTime värden om hello videor har olika första tidsstämpel som är vanligtvis hello fallet med Direktmigrering Arkiv.</span><span class="sxs-lookup"><span data-stu-id="26ce8-195">You need toomodify hello StartTime values if hello videos have different starting timestamp, as is typically hello case with live archives.</span></span>
* <span data-ttu-id="26ce8-196">hello gör JSON förinställda explicita referenser toohello AssetID värdena för hello inkommande tillgångar.</span><span class="sxs-lookup"><span data-stu-id="26ce8-196">hello JSON preset makes explicit references toohello AssetID values of hello input assets.</span></span>
* <span data-ttu-id="26ce8-197">hello exempelkoden förutsätter att hello JSON förinställningen har sparats tooa lokal fil, till exempel ”C:\supportFiles\preset.json”.</span><span class="sxs-lookup"><span data-stu-id="26ce8-197">hello sample code assumes that hello JSON preset has been saved tooa local file, such as "C:\supportFiles\preset.json".</span></span> <span data-ttu-id="26ce8-198">Det förutsätts även att två tillgångar har skapats genom att ladda upp två videofiler och att du vet hello gällande AssetID värden.</span><span class="sxs-lookup"><span data-stu-id="26ce8-198">It also assumes that two assets have been created by uploading two video files, and that you know hello resultant AssetID values.</span></span>
* <span data-ttu-id="26ce8-199">Hej kodfragmentet och JSON förinställda visar ett exempel på Sammanfoga två videofiler.</span><span class="sxs-lookup"><span data-stu-id="26ce8-199">hello code snippet and JSON preset shows an example of concatenating two video files.</span></span> <span data-ttu-id="26ce8-200">Du kan utöka den toomore än två videor av:</span><span class="sxs-lookup"><span data-stu-id="26ce8-200">You can extend it toomore than two videos by:</span></span>

  1. <span data-ttu-id="26ce8-201">Anropar uppgift. InputAssets.Add() upprepade gånger tooadd fler videoklipp i ordning.</span><span class="sxs-lookup"><span data-stu-id="26ce8-201">Calling task.InputAssets.Add() repeatedly tooadd more videos, in order.</span></span>
  2. <span data-ttu-id="26ce8-202">Gör motsvarande redigerar toohello ”källor” element i hello JSON, genom att lägga till fler poster i hello samma ordning.</span><span class="sxs-lookup"><span data-stu-id="26ce8-202">Making corresponding edits toohello "Sources" element in hello JSON, by adding more entries, in hello same order.</span></span>

### <a name="net-code"></a><span data-ttu-id="26ce8-203">.NET-kod</span><span class="sxs-lookup"><span data-stu-id="26ce8-203">.NET code</span></span>

    IAsset asset1 = _context.Assets.Where(asset => asset.Id == "nb:cid:UUID:606db602-efd7-4436-97b4-c0b867ba195b").FirstOrDefault();
    IAsset asset2 = _context.Assets.Where(asset => asset.Id == "nb:cid:UUID:a7e2b90f-0565-4a94-87fe-0a9fa07b9c7e").FirstOrDefault();

    // Declare a new job.
    IJob job = _context.Jobs.Create("Media Encoder Standard Job for Concatenating Videos");
    // Get a media processor reference, and pass tooit hello name of the
    // processor toouse for hello specific task.
    IMediaProcessor processor = GetLatestMediaProcessorByName("Media Encoder Standard");

    // Load hello XML (or JSON) from hello local file.
    string configuration = File.ReadAllText(@"c:\supportFiles\preset.json");

    // Create a task
    ITask task = job.Tasks.AddNew("Media Encoder Standard encoding task",
        processor,
        configuration,
        TaskOptions.None);

    // Specify hello input videos toobe concatenated (in order).
    task.InputAssets.Add(asset1);
    task.InputAssets.Add(asset2);
    // Add an output asset toocontain hello results of hello job.
    // This output is specified as AssetCreationOptions.None, which
    // means hello output asset is not encrypted.
    task.OutputAssets.AddNew("Output asset",
        AssetCreationOptions.None);

    job.StateChanged += new EventHandler<JobStateChangedEventArgs>(JobStateChanged);
    job.Submit();
    job.GetExecutionProgressTask(CancellationToken.None).Wait();

### <a name="json-preset"></a><span data-ttu-id="26ce8-204">JSON förinställda</span><span class="sxs-lookup"><span data-stu-id="26ce8-204">JSON preset</span></span>

<span data-ttu-id="26ce8-205">Uppdatera dina anpassade förinställningen med ID: n för hello tillgångar som du vill tooconcatenate och hello lämplig tidpunkt segment för varje video.</span><span class="sxs-lookup"><span data-stu-id="26ce8-205">Update your custom preset with ids of hello assets that you want tooconcatenate, and with hello appropriate time segment for each video.</span></span>

    {
      "Version": 1.0,
      "Sources": [
        {
          "AssetID": "606db602-efd7-4436-97b4-c0b867ba195b",
          "StartTime": "00:00:01",
          "Duration": "00:00:15"
        },
        {
          "AssetID": "a7e2b90f-0565-4a94-87fe-0a9fa07b9c7e",
          "StartTime": "00:00:02",
          "Duration": "00:00:05"
        }
      ],
      "Codecs": [
        {
          "KeyFrameInterval": "00:00:02",
          "SceneChangeDetection": true,
          "H264Layers": [
            {
              "Level": "auto",
              "Bitrate": 1800,
              "MaxBitrate": 1800,
              "BufferWindow": "00:00:05",
              "BFrames": 3,
              "ReferenceFrames": 3,
              "AdaptiveBFrame": true,
              "Type": "H264Layer",
              "Width": "640",
              "Height": "360",
              "FrameRate": "0/1"
            }
          ],
          "Type": "H264Video"
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
          "FileName": "{Basename}_{Width}x{Height}_{VideoBitrate}.mp4",
          "Format": {
            "Type": "MP4Format"
          }
        }
      ]
    }

## <span data-ttu-id="26ce8-206"><a id="crop"></a>Beskär videor med Media Encoder Standard</span><span class="sxs-lookup"><span data-stu-id="26ce8-206"><a id="crop"></a>Crop videos with Media Encoder Standard</span></span>
<span data-ttu-id="26ce8-207">Se hello [Beskär videor med Media Encoder Standard](media-services-crop-video.md) avsnittet.</span><span class="sxs-lookup"><span data-stu-id="26ce8-207">See hello [Crop videos with Media Encoder Standard](media-services-crop-video.md) topic.</span></span>

## <span data-ttu-id="26ce8-208"><a id="no_video"></a>Infoga en video spåra när indata har ingen bild</span><span class="sxs-lookup"><span data-stu-id="26ce8-208"><a id="no_video"></a>Insert a video track when input has no video</span></span>

<span data-ttu-id="26ce8-209">Som standard om du skickar en inkommande toohello kodare som innehåller endast ljud och ingen bild innehåller hello utdatatillgången filer som innehåller endast ljuddata.</span><span class="sxs-lookup"><span data-stu-id="26ce8-209">By default, if you send an input toohello encoder that contains only audio, and no video, then hello output asset contains files that contain only audio data.</span></span> <span data-ttu-id="26ce8-210">Vissa spelare, inklusive Azure Media Player (se [detta](https://feedback.azure.com/forums/169396-azure-media-services/suggestions/8082468-audio-only-scenarios)) kanske inte kan toohandle dessa strömmar.</span><span class="sxs-lookup"><span data-stu-id="26ce8-210">Some players, including Azure Media Player (see [this](https://feedback.azure.com/forums/169396-azure-media-services/suggestions/8082468-audio-only-scenarios)) may not be able toohandle such streams.</span></span> <span data-ttu-id="26ce8-211">Du kan använda den här inställningen tooforce hello kodare tooadd en monokromt video spåra toohello utdata i scenariot.</span><span class="sxs-lookup"><span data-stu-id="26ce8-211">You can use this setting tooforce hello encoder tooadd a monochrome video track toohello output in that scenario.</span></span>

> [!NOTE]
> <span data-ttu-id="26ce8-212">Tvinga hello kodare tooinsert en utdata video spåra ökar hello storlek hello utdata tillgången och hello därmed kostnaden för hello kodning aktivitet.</span><span class="sxs-lookup"><span data-stu-id="26ce8-212">Forcing hello encoder tooinsert an output video track increases hello size of hello output Asset, and thereby hello cost incurred for hello encoding Task.</span></span> <span data-ttu-id="26ce8-213">Du bör köra testerna tooverify som Storleksökningen gällande har endast en liten inverkan på dina månatliga avgifter.</span><span class="sxs-lookup"><span data-stu-id="26ce8-213">You should run tests tooverify that this resultant increase has only a modest impact on your monthly charges.</span></span>
>

### <a name="inserting-video-at-only-hello-lowest-bitrate"></a><span data-ttu-id="26ce8-214">Infoga video på endast hello lägsta bithastighet</span><span class="sxs-lookup"><span data-stu-id="26ce8-214">Inserting video at only hello lowest bitrate</span></span>

<span data-ttu-id="26ce8-215">Anta att du använder en flera kodning av bithastighet förinställningen som [”H264 Multibithastighet 720p”](media-services-mes-preset-h264-multiple-bitrate-720p.md) tooencode hela din indata katalogen för strömning, som innehåller en blandning av video och ljud-only-filer.</span><span class="sxs-lookup"><span data-stu-id="26ce8-215">Suppose you are using a multiple bitrate encoding preset such as ["H264 Multiple Bitrate 720p"](media-services-mes-preset-h264-multiple-bitrate-720p.md) tooencode your entire input catalog for streaming, which contains a mix of video files and audio-only files.</span></span> <span data-ttu-id="26ce8-216">I detta scenario när hello indata har ingen bild kanske du vill tooforce hello kodare tooinsert en video monokromt spåra på bara hello lägsta bithastighet, som skillnad från tooinserting video vid varje utdata bithastighet.</span><span class="sxs-lookup"><span data-stu-id="26ce8-216">In this scenario, when hello input has no video, you may want tooforce hello encoder tooinsert a monochrome video track at just hello lowest bitrate, as opposed tooinserting video at every output bitrate.</span></span> <span data-ttu-id="26ce8-217">tooachieve detta, behöver du toouse hello **InsertBlackIfNoVideoBottomLayerOnly** flaggan.</span><span class="sxs-lookup"><span data-stu-id="26ce8-217">tooachieve this, you need toouse hello **InsertBlackIfNoVideoBottomLayerOnly** flag.</span></span>

<span data-ttu-id="26ce8-218">Du kan vidta någon av hello MES förinställningar dokumenterade i [detta](media-services-mes-presets-overview.md) avsnitt och att Hej ändringar:</span><span class="sxs-lookup"><span data-stu-id="26ce8-218">You can take any of hello MES presets documented in [this](media-services-mes-presets-overview.md) section, and make hello following modification:</span></span>

#### <a name="json-preset"></a><span data-ttu-id="26ce8-219">JSON förinställda</span><span class="sxs-lookup"><span data-stu-id="26ce8-219">JSON preset</span></span>
    {
          "KeyFrameInterval": "00:00:02",
          "StretchMode": "AutoSize",
          "Condition": "InsertBlackIfNoVideoBottomLayerOnly",
          "H264Layers": [
          …
          ]
    }

#### <a name="xml-preset"></a><span data-ttu-id="26ce8-220">XML-förinställda</span><span class="sxs-lookup"><span data-stu-id="26ce8-220">XML preset</span></span>

<span data-ttu-id="26ce8-221">Använd villkor när du använder XML = ”InsertBlackIfNoVideoBottomLayerOnly” som en attributet toohello **H264Video** element och villkor = ”InsertSilenceIfNoAudio” som ett attribut för**AACAudio**.</span><span class="sxs-lookup"><span data-stu-id="26ce8-221">When using XML, use Condition="InsertBlackIfNoVideoBottomLayerOnly" as an attribute toohello **H264Video** element and  Condition="InsertSilenceIfNoAudio" as an attribute too**AACAudio**.</span></span>

```
. . .
<Encoding>
  <H264Video Condition="InsertBlackIfNoVideoBottomLayerOnly">
    <KeyFrameInterval>00:00:02</KeyFrameInterval>
    <SceneChangeDetection>true</SceneChangeDetection>
    <StretchMode>AutoSize</StretchMode>
    <H264Layers>
      <H264Layer>
        . . .
      </H264Layer>
    </H264Layers>
    <Chapters />
  </H264Video>
  <AACAudio Condition="InsertSilenceIfNoAudio">
    <Profile>AACLC</Profile>
    <Channels>2</Channels>
    <SamplingRate>48000</SamplingRate>
    <Bitrate>128</Bitrate>
  </AACAudio>
</Encoding>
. . .
```

### <a name="inserting-video-at-all-output-bitrates"></a><span data-ttu-id="26ce8-222">Infoga video alls utdata bithastighet</span><span class="sxs-lookup"><span data-stu-id="26ce8-222">Inserting video at all output bitrates</span></span>
<span data-ttu-id="26ce8-223">Anta att du använder en flera kodning av bithastighet förinställningen som [”H264 Multibithastighet 720p](media-services-mes-preset-H264-Multiple-Bitrate-720p.md) tooencode hela din indata katalogen för strömning, som innehåller en blandning av video och ljud-only-filer.</span><span class="sxs-lookup"><span data-stu-id="26ce8-223">Suppose you are using a multiple bitrate encoding preset such as ["H264 Multiple Bitrate 720p](media-services-mes-preset-H264-Multiple-Bitrate-720p.md) tooencode your entire input catalog for streaming, which contains a mix of video files and audio-only files.</span></span> <span data-ttu-id="26ce8-224">I detta scenario när hello indata har ingen bild kanske du vill tooforce hello kodare tooinsert monokromt video reda på alla hello utdata bithastighet.</span><span class="sxs-lookup"><span data-stu-id="26ce8-224">In this scenario, when hello input has no video, you may want tooforce hello encoder tooinsert a monochrome video track at all hello output bitrates.</span></span> <span data-ttu-id="26ce8-225">Detta säkerställer att din utdata tillgångar är alla homogen med avseende toonumber spårar video och ljud spår.</span><span class="sxs-lookup"><span data-stu-id="26ce8-225">This ensures that your output Assets are all homogenous with respect toonumber of video tracks and audio tracks.</span></span> <span data-ttu-id="26ce8-226">tooachieve detta, behöver du toospecify hello ”InsertBlackIfNoVideo” flaggan.</span><span class="sxs-lookup"><span data-stu-id="26ce8-226">tooachieve this, you need toospecify hello "InsertBlackIfNoVideo" flag.</span></span>

<span data-ttu-id="26ce8-227">Du kan vidta någon av hello MES förinställningar dokumenterade i [detta](media-services-mes-presets-overview.md) avsnitt och att Hej ändringar:</span><span class="sxs-lookup"><span data-stu-id="26ce8-227">You can take any of hello MES presets documented in [this](media-services-mes-presets-overview.md) section, and make hello following modification:</span></span>

#### <a name="json-preset"></a><span data-ttu-id="26ce8-228">JSON förinställda</span><span class="sxs-lookup"><span data-stu-id="26ce8-228">JSON preset</span></span>
    {
          "KeyFrameInterval": "00:00:02",
          "StretchMode": "AutoSize",
          "Condition": "InsertBlackIfNoVideo",
          "H264Layers": [
          …
          ]
    }

#### <a name="xml-preset"></a><span data-ttu-id="26ce8-229">XML-förinställda</span><span class="sxs-lookup"><span data-stu-id="26ce8-229">XML preset</span></span>

<span data-ttu-id="26ce8-230">Använd villkor när du använder XML = ”InsertBlackIfNoVideo” som en attributet toohello **H264Video** element och villkor = ”InsertSilenceIfNoAudio” som ett attribut för**AACAudio**.</span><span class="sxs-lookup"><span data-stu-id="26ce8-230">When using XML, use Condition="InsertBlackIfNoVideo" as an attribute toohello **H264Video** element and  Condition="InsertSilenceIfNoAudio" as an attribute too**AACAudio**.</span></span>

```
. . .
<Encoding>
  <H264Video Condition="InsertBlackIfNoVideo">
    <KeyFrameInterval>00:00:02</KeyFrameInterval>
    <SceneChangeDetection>true</SceneChangeDetection>
    <StretchMode>AutoSize</StretchMode>
    <H264Layers>
      <H264Layer>
        . . .
      </H264Layer>
    </H264Layers>
    <Chapters />
  </H264Video>
  <AACAudio Condition="InsertSilenceIfNoAudio">
    <Profile>AACLC</Profile>
    <Channels>2</Channels>
    <SamplingRate>48000</SamplingRate>
    <Bitrate>128</Bitrate>
  </AACAudio>
</Encoding>
. . .  
```

## <span data-ttu-id="26ce8-231"><a id="rotate_video"></a>Rotera en video</span><span class="sxs-lookup"><span data-stu-id="26ce8-231"><a id="rotate_video"></a>Rotate a video</span></span>
<span data-ttu-id="26ce8-232">Hej [Media Encoder Standard](media-services-dotnet-encode-with-media-encoder-standard.md) stöder rotation av vinklar 0/90/180/270.</span><span class="sxs-lookup"><span data-stu-id="26ce8-232">hello [Media Encoder Standard](media-services-dotnet-encode-with-media-encoder-standard.md) supports rotation by angles of 0/90/180/270.</span></span> <span data-ttu-id="26ce8-233">hello standardbeteendet är ”automatisk”, där den försöker toodetect hello rotation metadata i hello inkommande videofil och kompensera för den.</span><span class="sxs-lookup"><span data-stu-id="26ce8-233">hello default behavior is "Auto", where it tries toodetect hello rotation metadata in hello incoming video file and compensate for it.</span></span> <span data-ttu-id="26ce8-234">Inkludera hello följande **källor** elementet tooone hello förinställningar som definierats i [detta](media-services-mes-presets-overview.md) avsnitt:</span><span class="sxs-lookup"><span data-stu-id="26ce8-234">Include hello following **Sources** element tooone of hello presets defined in [this](media-services-mes-presets-overview.md) section:</span></span>

### <a name="json-preset"></a><span data-ttu-id="26ce8-235">JSON förinställda</span><span class="sxs-lookup"><span data-stu-id="26ce8-235">JSON preset</span></span>
    "Sources": [
    {
      "Streams": [],
      "Filters": {
        "Rotation": "90"
      }
    }
    ],
    "Codecs": [

    ...
### <a name="xml-preset"></a><span data-ttu-id="26ce8-236">XML-förinställda</span><span class="sxs-lookup"><span data-stu-id="26ce8-236">XML preset</span></span>
    <Sources>
           <Source>
          <Streams />
          <Filters>
            <Rotation>90</Rotation>
          </Filters>
        </Source>
    </Sources>

<span data-ttu-id="26ce8-237">Se även [detta](media-services-mes-schema.md#PreserveResolutionAfterRotation) avsnittet för mer information om hur hello kodare tolkar hello bredd och höjd inställningar i hello förinställningen när rotation ersättning utlöses.</span><span class="sxs-lookup"><span data-stu-id="26ce8-237">Also, see [this](media-services-mes-schema.md#PreserveResolutionAfterRotation) topic for more information on how hello encoder interprets hello Width and Height settings in hello preset, when rotation compensation is triggered.</span></span>

<span data-ttu-id="26ce8-238">Du kan använda hello värdet ”0” tooindicate toohello kodare tooignore rotation metadata, om den finns i hello inkommande videon.</span><span class="sxs-lookup"><span data-stu-id="26ce8-238">You can use hello value "0" tooindicate toohello encoder tooignore rotation metadata, if present, in hello input video.</span></span>

## <a name="media-services-learning-paths"></a><span data-ttu-id="26ce8-239">Sökvägar för Media Services-utbildning</span><span class="sxs-lookup"><span data-stu-id="26ce8-239">Media Services learning paths</span></span>
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a><span data-ttu-id="26ce8-240">Ge feedback</span><span class="sxs-lookup"><span data-stu-id="26ce8-240">Provide feedback</span></span>
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

## <a name="see-also"></a><span data-ttu-id="26ce8-241">Se även</span><span class="sxs-lookup"><span data-stu-id="26ce8-241">See Also</span></span>
[<span data-ttu-id="26ce8-242">Media Services Encoding översikt</span><span class="sxs-lookup"><span data-stu-id="26ce8-242">Media Services Encoding Overview</span></span>](media-services-encode-asset.md)
