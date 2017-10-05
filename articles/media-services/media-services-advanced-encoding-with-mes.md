---
title: "Utföra avancerade encoding genom att anpassa MES förinställningar | Microsoft Docs"
description: "Det här avsnittet beskrivs hur du utför avancerad kodning genom att anpassa Media Encoder Standard uppgiften förinställningar."
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
ms.openlocfilehash: 8de3bdd45261c84a0e1bb90f1c58863ad740dd5a
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/29/2017
---
# <a name="perform-advanced-encoding-by-customizing-mes-presets"></a><span data-ttu-id="97605-103">Utföra avancerade encoding genom att anpassa MES förinställningar</span><span class="sxs-lookup"><span data-stu-id="97605-103">Perform advanced encoding by customizing MES presets</span></span> 

## <a name="overview"></a><span data-ttu-id="97605-104">Översikt</span><span class="sxs-lookup"><span data-stu-id="97605-104">Overview</span></span>

<span data-ttu-id="97605-105">Det här avsnittet beskrivs hur du anpassar Media Encoder Standard förinställningar.</span><span class="sxs-lookup"><span data-stu-id="97605-105">This topic shows how to customize Media Encoder Standard presets.</span></span> <span data-ttu-id="97605-106">Den [Encoding med Media Encoder Standard med hjälp av anpassade förinställningar](media-services-custom-mes-presets-with-dotnet.md) avsnittet visas hur du skapar en uppgift som kodning och ett jobb som utför den här uppgiften med hjälp av .NET.</span><span class="sxs-lookup"><span data-stu-id="97605-106">The [Encoding with Media Encoder Standard using custom presets](media-services-custom-mes-presets-with-dotnet.md) topic shows how to use .NET to create an encoding task and a job that executes this task.</span></span> <span data-ttu-id="97605-107">När du har ändrat en förinställning kan du ange anpassade förinställningar kodning aktiviteten.</span><span class="sxs-lookup"><span data-stu-id="97605-107">Once you customize a preset, supply the custom presets to the encoding task.</span></span> 

>[!NOTE]
><span data-ttu-id="97605-108">Om du använder en XML-förinställning, se till att bevara elementordning, som visas i XML-exempel (till exempel KeyFrameInterval måste föregå SceneChangeDetection).</span><span class="sxs-lookup"><span data-stu-id="97605-108">If using an XML preset, make sure to preserve the order of elements, as shown in XML samples below (for example, KeyFrameInterval should precede SceneChangeDetection).</span></span>
>

<span data-ttu-id="97605-109">I det här avsnittet visas anpassade förinställningar som utför följande uppgifter för kodning.</span><span class="sxs-lookup"><span data-stu-id="97605-109">In this topic, the custom presets that perform the following encoding tasks are demonstrated.</span></span>

## <a name="support-for-relative-sizes"></a><span data-ttu-id="97605-110">Stöd för relativa storleken</span><span class="sxs-lookup"><span data-stu-id="97605-110">Support for relative sizes</span></span>

<span data-ttu-id="97605-111">När du genererar miniatyrer, behöver du inte alltid ange utdata bredd och höjd i bildpunkter.</span><span class="sxs-lookup"><span data-stu-id="97605-111">When generating thumbnails, you do not need to always specify output width and height in pixels.</span></span> <span data-ttu-id="97605-112">Du kan ange dem i procent i intervallet [% 1,..., 100%].</span><span class="sxs-lookup"><span data-stu-id="97605-112">You can specify them in percentages, in the range [1%, …, 100%].</span></span>

### <a name="json-preset"></a><span data-ttu-id="97605-113">JSON förinställda</span><span class="sxs-lookup"><span data-stu-id="97605-113">JSON preset</span></span>
    "Width": "100%",
    "Height": "100%"

### <a name="xml-preset"></a><span data-ttu-id="97605-114">XML-förinställda</span><span class="sxs-lookup"><span data-stu-id="97605-114">XML preset</span></span>
    <Width>100%</Width>
    <Height>100%</Height>

## <span data-ttu-id="97605-115"><a id="thumbnails"></a>Generera miniatyrbilder</span><span class="sxs-lookup"><span data-stu-id="97605-115"><a id="thumbnails"></a>Generate thumbnails</span></span>

<span data-ttu-id="97605-116">Det här avsnittet visar hur du anpassar en förinställning som genererar miniatyrer.</span><span class="sxs-lookup"><span data-stu-id="97605-116">This section shows how to customize a preset that generates thumbnails.</span></span> <span data-ttu-id="97605-117">Den förinställning som anges nedan innehåller information om hur du vill koda din fil samt information som behövs för att generera miniatyrbilder.</span><span class="sxs-lookup"><span data-stu-id="97605-117">The preset defined below contains information on how you want to encode your file as well as information needed to generate thumbnails.</span></span> <span data-ttu-id="97605-118">Du kan vidta någon av MES förinställningar dokumenterade [detta](media-services-mes-presets-overview.md) och lägger till kod som genererar miniatyrer.</span><span class="sxs-lookup"><span data-stu-id="97605-118">You can take any of the MES presets documented [this](media-services-mes-presets-overview.md) section and add code that generates thumbnails.</span></span>  

> [!NOTE]
> <span data-ttu-id="97605-119">Den **SceneChangeDetection** inställningen i följande förinställda kan endast anges till true om du kodar för en enda bithastighet video.</span><span class="sxs-lookup"><span data-stu-id="97605-119">The **SceneChangeDetection** setting in the following preset can only be set to true if you are encoding to a single  bitrate video.</span></span> <span data-ttu-id="97605-120">Om du kodar till en video med flera bithastigheter och ange **SceneChangeDetection** till true, kodaren returnerar ett fel.</span><span class="sxs-lookup"><span data-stu-id="97605-120">If you are encoding to a multi-bitrate video and set **SceneChangeDetection** to true, the encoder returns an error.</span></span>  
>
>

<span data-ttu-id="97605-121">Mer information om schemat finns [detta](media-services-mes-schema.md) avsnittet.</span><span class="sxs-lookup"><span data-stu-id="97605-121">For information about schema, see [this](media-services-mes-schema.md) topic.</span></span>

<span data-ttu-id="97605-122">Se till att granska den [överväganden](#considerations) avsnitt.</span><span class="sxs-lookup"><span data-stu-id="97605-122">Make sure to review the [Considerations](#considerations) section.</span></span>

### <span data-ttu-id="97605-123"><a id="json"></a>JSON förinställda</span><span class="sxs-lookup"><span data-stu-id="97605-123"><a id="json"></a>JSON preset</span></span>
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


### <span data-ttu-id="97605-124"><a id="xml"></a>XML-förinställda</span><span class="sxs-lookup"><span data-stu-id="97605-124"><a id="xml"></a>XML preset</span></span>
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

### <a name="considerations"></a><span data-ttu-id="97605-125">Överväganden</span><span class="sxs-lookup"><span data-stu-id="97605-125">Considerations</span></span>

<span data-ttu-id="97605-126">Följande gäller:</span><span class="sxs-lookup"><span data-stu-id="97605-126">The following considerations apply:</span></span>

* <span data-ttu-id="97605-127">Användningen av explicita tidsstämplar för Startintervall-steg förutsätter att Indatakällan är minst 1 minut.</span><span class="sxs-lookup"><span data-stu-id="97605-127">The use of explicit timestamps for Start/Step/Range assumes that the input source is at least 1 minute long.</span></span>
* <span data-ttu-id="97605-128">BmpImage-jpg/Png-element har Start, steg, och intervallet strängattribut – dessa kan tolkas som:</span><span class="sxs-lookup"><span data-stu-id="97605-128">Jpg/Png/BmpImage elements have Start, Step, and Range string attributes – these can be interpreted as:</span></span>

  * <span data-ttu-id="97605-129">RAM-numret om de är icke-negativa heltal, till exempel ”Start”: ”120”</span><span class="sxs-lookup"><span data-stu-id="97605-129">Frame Number if they are non-negative integers, for example "Start": "120",</span></span>
  * <span data-ttu-id="97605-130">Relativt källa varaktighet om uttryckt som %-suffixet, till exempel ”Start”: ”15%”, eller</span><span class="sxs-lookup"><span data-stu-id="97605-130">Relative to source duration if expressed as %-suffixed, for example "Start": "15%", OR</span></span>
  * <span data-ttu-id="97605-131">Tidsstämpel om uttryckt: mm: ss...</span><span class="sxs-lookup"><span data-stu-id="97605-131">Timestamp if expressed as HH:MM:SS…</span></span> <span data-ttu-id="97605-132">Formatera, till exempel ”Start” ”: 00: 01:00”</span><span class="sxs-lookup"><span data-stu-id="97605-132">format, for example "Start" : "00:01:00"</span></span>

    <span data-ttu-id="97605-133">Du kan blanda och matcha här ska du ange.</span><span class="sxs-lookup"><span data-stu-id="97605-133">You can mix and match notations as you please.</span></span>

    <span data-ttu-id="97605-134">Dessutom Start har också stöd för ett särskilt makro: {bästa}, som försöker fastställa den första ”intressanta” bildrutan anteckningens innehåll: (steg och intervallet ignoreras när Start är inställd på {bäst})</span><span class="sxs-lookup"><span data-stu-id="97605-134">Additionally, Start also supports a special Macro:{Best}, which attempts to determine the first “interesting” frame of the content NOTE: (Step and Range are ignored when Start is set to {Best})</span></span>
  * <span data-ttu-id="97605-135">Standardvärden: Starta: {bästa}</span><span class="sxs-lookup"><span data-stu-id="97605-135">Defaults: Start:{Best}</span></span>
* <span data-ttu-id="97605-136">Utdataformat måste tillhandahållas explicit för varje bildformat: Jpg/Png/BmpFormat.</span><span class="sxs-lookup"><span data-stu-id="97605-136">Output format needs to be explicitly provided for each Image format: Jpg/Png/BmpFormat.</span></span> <span data-ttu-id="97605-137">När matchar MES JpgVideo till JpgFormat och så vidare.</span><span class="sxs-lookup"><span data-stu-id="97605-137">When present, MES matches JpgVideo to JpgFormat and so on.</span></span> <span data-ttu-id="97605-138">OutputFormat introducerar ett nytt specifika bilden codec makro: {Index}, vilket måste vara finns (en gång och bara en gång) för bildformat för utdata.</span><span class="sxs-lookup"><span data-stu-id="97605-138">OutputFormat introduces a new image-codec specific Macro: {Index}, which needs to be present (once and only once) for image output formats.</span></span>

## <span data-ttu-id="97605-139"><a id="trim_video"></a>Ta bort en video (urklippet)</span><span class="sxs-lookup"><span data-stu-id="97605-139"><a id="trim_video"></a>Trim a video (clipping)</span></span>
<span data-ttu-id="97605-140">Det här avsnittet handlar om att ändra kodare förinställningar för att klippa ut eller rensa indatavideo där indata är en s.k. mezzaninfil eller på begäran-fil.</span><span class="sxs-lookup"><span data-stu-id="97605-140">This section talks about modifying the encoder presets to clip or trim the input video where the input is a so-called mezzanine file or on-demand file.</span></span> <span data-ttu-id="97605-141">Kodaren kan också användas för att klippa ut eller ta bort en tillgång, som har hämtats eller arkiverade från en direktsänd dataström – information om detta finns i [bloggen](https://azure.microsoft.com/blog/sub-clipping-and-live-archive-extraction-with-media-encoder-standard/).</span><span class="sxs-lookup"><span data-stu-id="97605-141">The encoder can also be used to clip or trim an asset, which is captured or archived from a live stream – the details for this are available in [this blog](https://azure.microsoft.com/blog/sub-clipping-and-live-archive-extraction-with-media-encoder-standard/).</span></span>

<span data-ttu-id="97605-142">Beskär videor du vidta någon av MES förinställningar dokumenterade [detta](media-services-mes-presets-overview.md) avsnittet och ändra den **källor** element (som visas nedan).</span><span class="sxs-lookup"><span data-stu-id="97605-142">To trim your videos, you can take any of the MES presets documented [this](media-services-mes-presets-overview.md) section and modify the **Sources** element (as shown below).</span></span> <span data-ttu-id="97605-143">Värdet för StartTime måste matcha inkommande videon absolut tidsstämplar.</span><span class="sxs-lookup"><span data-stu-id="97605-143">The value of StartTime needs to match the absolute timestamps of the input video.</span></span> <span data-ttu-id="97605-144">Till exempel om den första bildrutan indata har en tidsstämpel för 12:00:10.000, sedan StartTime bör vara minst 12:00:10.000 och större.</span><span class="sxs-lookup"><span data-stu-id="97605-144">For example, if the first frame of the input video has a timestamp of 12:00:10.000, then StartTime should be at least 12:00:10.000 and greater.</span></span> <span data-ttu-id="97605-145">I exemplet nedan förutsätter att indatavideo har en första tidsstämpel noll.</span><span class="sxs-lookup"><span data-stu-id="97605-145">In the example below, we assume that the input video has a starting timestamp of zero.</span></span> <span data-ttu-id="97605-146">**Källor** ska placeras i början av förinställningen.</span><span class="sxs-lookup"><span data-stu-id="97605-146">**Sources** should be placed at the beginning of the preset.</span></span>

### <span data-ttu-id="97605-147"><a id="json"></a>JSON förinställda</span><span class="sxs-lookup"><span data-stu-id="97605-147"><a id="json"></a>JSON preset</span></span>
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

### <a name="xml-preset"></a><span data-ttu-id="97605-148">XML-förinställda</span><span class="sxs-lookup"><span data-stu-id="97605-148">XML preset</span></span>
<span data-ttu-id="97605-149">Beskär videor du vidta någon av MES förinställningar dokumenterade [här](media-services-mes-presets-overview.md) och ändra den **källor** element (som visas nedan).</span><span class="sxs-lookup"><span data-stu-id="97605-149">To trim your videos, you can take any of the MES presets documented [here](media-services-mes-presets-overview.md) and modify the **Sources** element (as shown below).</span></span>

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

## <span data-ttu-id="97605-150"><a id="overlay"></a>Skapa ett överlägg</span><span class="sxs-lookup"><span data-stu-id="97605-150"><a id="overlay"></a>Create an overlay</span></span>

<span data-ttu-id="97605-151">Media Encoder Standard kan du överlagra en bild till en befintlig video.</span><span class="sxs-lookup"><span data-stu-id="97605-151">The Media Encoder Standard allows you to overlay an image onto an existing video.</span></span> <span data-ttu-id="97605-152">För närvarande följande format stöds: png, jpg, gif, bmp och.</span><span class="sxs-lookup"><span data-stu-id="97605-152">Currently, the following formats are supported: png, jpg, gif, and bmp.</span></span> <span data-ttu-id="97605-153">Den förinställning som anges nedan är ett grundläggande exempel på en videoöverlägg.</span><span class="sxs-lookup"><span data-stu-id="97605-153">The preset defined below is a basic example  of a video overlay.</span></span>

<span data-ttu-id="97605-154">Utöver definierar en förvalda har du också kan Media Services vet vilken fil i tillgången överlägget avbildningen och vilken fil som är videon som du vill överlagra avbildningen.</span><span class="sxs-lookup"><span data-stu-id="97605-154">In addition to defining a preset file, you also have to let Media Services know which file in the asset is the overlay image and which file is the source video onto which you want to overlay the image.</span></span> <span data-ttu-id="97605-155">Video filen måste vara den **primära** fil.</span><span class="sxs-lookup"><span data-stu-id="97605-155">The video file has to be the **primary** file.</span></span>

<span data-ttu-id="97605-156">Om du använder .NET, lägger du till följande två funktioner i .NET-exempel som definierats i [detta](media-services-custom-mes-presets-with-dotnet.md#encoding_with_dotnet) avsnittet.</span><span class="sxs-lookup"><span data-stu-id="97605-156">If you are using .NET, add the following two functions to the .NET example defined in [this](media-services-custom-mes-presets-with-dotnet.md#encoding_with_dotnet) topic.</span></span> <span data-ttu-id="97605-157">Den **UploadMediaFilesFromFolder** funktionen Överför filer från en mapp (till exempel BigBuckBunny.mp4 och Image001.png) och anger mp4-fil ska vara den primära filen i tillgången.</span><span class="sxs-lookup"><span data-stu-id="97605-157">The **UploadMediaFilesFromFolder** function uploads files from a folder (for example, BigBuckBunny.mp4 and Image001.png) and sets the mp4 file to be the primary file in the asset.</span></span> <span data-ttu-id="97605-158">Den **EncodeWithOverlay** funktionen använder den anpassade förinställda filen som skickades till den (till exempel den förinställning som visas nedan) att skapa aktiviteten kodning.</span><span class="sxs-lookup"><span data-stu-id="97605-158">The **EncodeWithOverlay** function uses the custom preset file that was passed to it (for example, the preset that follows) to create the encoding task.</span></span>


    static public IAsset UploadMediaFilesFromFolder(string folderPath)
    {
        IAsset asset = _context.Assets.CreateFromFolder(folderPath, AssetCreationOptions.None);
    
        foreach (var af in asset.AssetFiles)
        {
            // The following code assumes 
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
        // Get a media processor reference, and pass to it the name of the 
        // processor to use for the specific task.
        IMediaProcessor processor = GetLatestMediaProcessorByName("Media Encoder Standard");

        // Load the XML (or JSON) from the local file.
        string configuration = File.ReadAllText(customPresetFileName);

        // Create a task
        ITask task = job.Tasks.AddNew("Media Encoder Standard encoding task",
            processor,
            configuration,
            TaskOptions.None);

        // Specify the input assets to be encoded.
        // This asset contains a source file and an overlay file.
        task.InputAssets.Add(assetSource);

        // Add an output asset to contain the results of the job. 
        task.OutputAssets.AddNew("Output asset",
            AssetCreationOptions.None);

        job.StateChanged += new EventHandler<JobStateChangedEventArgs>(JobStateChanged);
        job.Submit();
        job.GetExecutionProgressTask(CancellationToken.None).Wait();

        return job.OutputMediaAssets[0];
    }


> [!NOTE]
> <span data-ttu-id="97605-159">Aktuella begränsningar:</span><span class="sxs-lookup"><span data-stu-id="97605-159">Current limitations:</span></span>
>
> <span data-ttu-id="97605-160">Inställningen för opacitet överlägget stöds inte.</span><span class="sxs-lookup"><span data-stu-id="97605-160">The overlay opacity setting is not supported.</span></span>
>
> <span data-ttu-id="97605-161">Källfilen video och överlägget image-filen måste vara i samma tillgång och videofilen måste anges som den primära filen i tillgången.</span><span class="sxs-lookup"><span data-stu-id="97605-161">Your source video file and the overlay image file have to be in the same asset, and the video file needs to be set as the primary file in this asset.</span></span>
>
>

### <a name="json-preset"></a><span data-ttu-id="97605-162">JSON förinställda</span><span class="sxs-lookup"><span data-stu-id="97605-162">JSON preset</span></span>
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


### <a name="xml-preset"></a><span data-ttu-id="97605-163">XML-förinställda</span><span class="sxs-lookup"><span data-stu-id="97605-163">XML preset</span></span>
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


## <span data-ttu-id="97605-164"><a id="silent_audio"></a>Infoga en tyst ljud spåra när indata har inget ljud</span><span class="sxs-lookup"><span data-stu-id="97605-164"><a id="silent_audio"></a>Insert a silent audio track when input has no audio</span></span>
<span data-ttu-id="97605-165">Som standard om du skickar indata till den kodare som bara innehåller video, och inget ljud innehåller utdatatillgången filer som innehåller endast video data.</span><span class="sxs-lookup"><span data-stu-id="97605-165">By default, if you send an input to the encoder that contains only video, and no audio, then the output asset contains files that contain only video data.</span></span> <span data-ttu-id="97605-166">Vissa spelare kanske inte kan hantera dessa utdataströmmar.</span><span class="sxs-lookup"><span data-stu-id="97605-166">Some players may not be able to handle such output streams.</span></span> <span data-ttu-id="97605-167">Du kan använda den här inställningen för att tvinga kodaren att lägga till en tyst ljud Spåra utdata i scenariot.</span><span class="sxs-lookup"><span data-stu-id="97605-167">You can use this setting to force the encoder to add a silent audio track to the output in that scenario.</span></span>

<span data-ttu-id="97605-168">Ange värdet ”InsertSilenceIfNoAudio” om du vill tvinga kodaren att skapa en tillgång som innehåller en tyst ljud spåra när indata har inget ljud.</span><span class="sxs-lookup"><span data-stu-id="97605-168">To force the encoder to produce an asset that contains a silent audio track when input has no audio, specify the "InsertSilenceIfNoAudio" value.</span></span>

<span data-ttu-id="97605-169">Du kan vidta någon av MES förinställningar dokumenterade i [detta](media-services-mes-presets-overview.md) avsnittet och gör följande ändringar:</span><span class="sxs-lookup"><span data-stu-id="97605-169">You can take any of the MES presets documented in [this](media-services-mes-presets-overview.md) section, and make the following modification:</span></span>

### <a name="json-preset"></a><span data-ttu-id="97605-170">JSON förinställda</span><span class="sxs-lookup"><span data-stu-id="97605-170">JSON preset</span></span>
    {
      "Channels": 2,
      "SamplingRate": 44100,
      "Bitrate": 96,
      "Type": "AACAudio",
      "Condition": "InsertSilenceIfNoAudio"
    }

### <a name="xml-preset"></a><span data-ttu-id="97605-171">XML-förinställda</span><span class="sxs-lookup"><span data-stu-id="97605-171">XML preset</span></span>
    <AACAudio Condition="InsertSilenceIfNoAudio">
      <Channels>2</Channels>
      <SamplingRate>44100</SamplingRate>
      <Bitrate>96</Bitrate>
    </AACAudio>

## <span data-ttu-id="97605-172"><a id="deinterlacing"></a>Inaktivera automatisk Frigör sammanflätning</span><span class="sxs-lookup"><span data-stu-id="97605-172"><a id="deinterlacing"></a>Disable auto de-interlacing</span></span>
<span data-ttu-id="97605-173">Kunder behöver inte göra något om de vill interlace innehållet ska automatiskt Frigör sammanflätade.</span><span class="sxs-lookup"><span data-stu-id="97605-173">Customers don’t need to do anything if they like the interlace contents to be automatically de-interlaced.</span></span> <span data-ttu-id="97605-174">När den automatiska Frigör sammanflätning är aktiverad (standard) stöder automatisk identifiering av sammanflätade ramar systemet och frigör interlaces endast ramar som markerats som sammanflätad.</span><span class="sxs-lookup"><span data-stu-id="97605-174">When the auto de-interlacing is on (default) the MES does the auto detection of interlaced frames and only de-interlaces frames marked as interlaced.</span></span>

<span data-ttu-id="97605-175">Du kan inaktivera den automatiska Frigör sammanflätning.</span><span class="sxs-lookup"><span data-stu-id="97605-175">You can turn the auto de-interlacing off.</span></span> <span data-ttu-id="97605-176">Det här alternativet rekommenderas inte.</span><span class="sxs-lookup"><span data-stu-id="97605-176">This option is not recommended.</span></span>

### <a name="json-preset"></a><span data-ttu-id="97605-177">JSON förinställda</span><span class="sxs-lookup"><span data-stu-id="97605-177">JSON preset</span></span>
    "Sources": [
    {
     "Filters": {
        "Deinterlace": {
          "Mode": "Off"
        }
      },
    }
    ]

### <a name="xml-preset"></a><span data-ttu-id="97605-178">XML-förinställda</span><span class="sxs-lookup"><span data-stu-id="97605-178">XML preset</span></span>
    <Sources>
    <Source>
      <Filters>
        <Deinterlace>
          <Mode>Off</Mode>
        </Deinterlace>
      </Filters>
    </Source>
    </Sources>


## <span data-ttu-id="97605-179"><a id="audio_only"></a>Endast ljud förinställningar</span><span class="sxs-lookup"><span data-stu-id="97605-179"><a id="audio_only"></a>Audio-only presets</span></span>
<span data-ttu-id="97605-180">Det här avsnittet beskrivs två ljuddata MES förinställningar: AAC ljud- och AAC bra kvalitet ljud.</span><span class="sxs-lookup"><span data-stu-id="97605-180">This section demonstrates two audio-only MES presets: AAC Audio and AAC Good Quality Audio.</span></span>

### <a name="aac-audio"></a><span data-ttu-id="97605-181">AAC ljud</span><span class="sxs-lookup"><span data-stu-id="97605-181">AAC Audio</span></span>
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

### <a name="aac-good-quality-audio"></a><span data-ttu-id="97605-182">AAC god kvalitet ljud</span><span class="sxs-lookup"><span data-stu-id="97605-182">AAC Good Quality Audio</span></span>
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

## <span data-ttu-id="97605-183"><a id="concatenate"></a>Sammanfoga två eller flera videofiler</span><span class="sxs-lookup"><span data-stu-id="97605-183"><a id="concatenate"></a>Concatenate two or more video files</span></span>

<span data-ttu-id="97605-184">I följande exempel visas hur du kan skapa en förinställning för att sammanfoga två eller flera videofiler.</span><span class="sxs-lookup"><span data-stu-id="97605-184">The following example illustrates how you can generate a preset to concatenate two or more video files.</span></span> <span data-ttu-id="97605-185">Det vanligaste scenariot är om du vill lägga till ett sidhuvud eller en släpvagn huvudsakliga videon.</span><span class="sxs-lookup"><span data-stu-id="97605-185">The most common scenario is when you want to add a header or a trailer to the main video.</span></span> <span data-ttu-id="97605-186">Det är avsedd att användas när videofiler redigeras tillsammans resursegenskaper (skärmupplösning, bildfrekvens, ljud spåra antalet osv.).</span><span class="sxs-lookup"><span data-stu-id="97605-186">The intended use is when the video files being edited together share  properties (video resolution, frame rate, audio track count, etc.).</span></span> <span data-ttu-id="97605-187">Noga inte för att blanda videor olika hastigheterna eller med olika antal ljud spår.</span><span class="sxs-lookup"><span data-stu-id="97605-187">You should take care not to mix videos of different frame rates, or with different number of audio tracks.</span></span>

>[!NOTE]
><span data-ttu-id="97605-188">Den aktuella designen av funktionen sammanfogning förväntar sig att ingående videoklipp är konsekvent vad gäller upplösning bildfrekvens osv.</span><span class="sxs-lookup"><span data-stu-id="97605-188">The current design of the concatenation feature expects that the input video clips are consistent in terms of resolution, frame rate etc.</span></span> 

### <a name="requirements-and-considerations"></a><span data-ttu-id="97605-189">Krav och överväganden</span><span class="sxs-lookup"><span data-stu-id="97605-189">Requirements and considerations</span></span>

* <span data-ttu-id="97605-190">Inkommande videor endast ha ett ljud spår.</span><span class="sxs-lookup"><span data-stu-id="97605-190">Input videos should only have one audio track.</span></span>
* <span data-ttu-id="97605-191">Inkommande videor ska ha samma bildfrekvens.</span><span class="sxs-lookup"><span data-stu-id="97605-191">Input videos should all have the same frame rate.</span></span>
* <span data-ttu-id="97605-192">Du måste överföra videor till separata tillgångar och ange videor som den primära filen i varje tillgång.</span><span class="sxs-lookup"><span data-stu-id="97605-192">You must upload your videos into separate assets and set the videos as the primary file in each asset.</span></span>
* <span data-ttu-id="97605-193">Du behöver veta varaktigheten för dina videor.</span><span class="sxs-lookup"><span data-stu-id="97605-193">You need to know the duration of your videos.</span></span>
* <span data-ttu-id="97605-194">Förinställda exemplen nedan förutsätter att alla inkommande videor som börjar med en tidsstämpel noll.</span><span class="sxs-lookup"><span data-stu-id="97605-194">The preset examples below assumes that all the input videos start with a timestamp of zero.</span></span> <span data-ttu-id="97605-195">Du behöver ändra värdena StartTime om videor har olika första tidstämpeln som normalt är fallet med Direktmigrering Arkiv.</span><span class="sxs-lookup"><span data-stu-id="97605-195">You need to modify the StartTime values if the videos have different starting timestamp, as is typically the case with live archives.</span></span>
* <span data-ttu-id="97605-196">JSON-förinställda gör explicita referenser till AssetID värdena för inkommande tillgångar.</span><span class="sxs-lookup"><span data-stu-id="97605-196">The JSON preset makes explicit references to the AssetID values of the input assets.</span></span>
* <span data-ttu-id="97605-197">Exempelkoden förutsätter att JSON-förinställda har sparats till en lokal fil, till exempel ”C:\supportFiles\preset.json”.</span><span class="sxs-lookup"><span data-stu-id="97605-197">The sample code assumes that the JSON preset has been saved to a local file, such as "C:\supportFiles\preset.json".</span></span> <span data-ttu-id="97605-198">Det förutsätts även att två tillgångar har skapats genom att ladda upp två videofiler och att du vet värdena AssetID.</span><span class="sxs-lookup"><span data-stu-id="97605-198">It also assumes that two assets have been created by uploading two video files, and that you know the resultant AssetID values.</span></span>
* <span data-ttu-id="97605-199">Kodstycke och JSON förinställda visar ett exempel på Sammanfoga två videofiler.</span><span class="sxs-lookup"><span data-stu-id="97605-199">The code snippet and JSON preset shows an example of concatenating two video files.</span></span> <span data-ttu-id="97605-200">Du kan utvidga den till fler än två videor av:</span><span class="sxs-lookup"><span data-stu-id="97605-200">You can extend it to more than two videos by:</span></span>

  1. <span data-ttu-id="97605-201">Anropar uppgift. InputAssets.Add() flera gånger för att lägga till fler videoklipp i ordning.</span><span class="sxs-lookup"><span data-stu-id="97605-201">Calling task.InputAssets.Add() repeatedly to add more videos, in order.</span></span>
  2. <span data-ttu-id="97605-202">Gör motsvarande redigerar i elementet ”källor” i JSON, genom att lägga till fler poster i samma ordning.</span><span class="sxs-lookup"><span data-stu-id="97605-202">Making corresponding edits to the "Sources" element in the JSON, by adding more entries, in the same order.</span></span>

### <a name="net-code"></a><span data-ttu-id="97605-203">.NET-kod</span><span class="sxs-lookup"><span data-stu-id="97605-203">.NET code</span></span>

    IAsset asset1 = _context.Assets.Where(asset => asset.Id == "nb:cid:UUID:606db602-efd7-4436-97b4-c0b867ba195b").FirstOrDefault();
    IAsset asset2 = _context.Assets.Where(asset => asset.Id == "nb:cid:UUID:a7e2b90f-0565-4a94-87fe-0a9fa07b9c7e").FirstOrDefault();

    // Declare a new job.
    IJob job = _context.Jobs.Create("Media Encoder Standard Job for Concatenating Videos");
    // Get a media processor reference, and pass to it the name of the
    // processor to use for the specific task.
    IMediaProcessor processor = GetLatestMediaProcessorByName("Media Encoder Standard");

    // Load the XML (or JSON) from the local file.
    string configuration = File.ReadAllText(@"c:\supportFiles\preset.json");

    // Create a task
    ITask task = job.Tasks.AddNew("Media Encoder Standard encoding task",
        processor,
        configuration,
        TaskOptions.None);

    // Specify the input videos to be concatenated (in order).
    task.InputAssets.Add(asset1);
    task.InputAssets.Add(asset2);
    // Add an output asset to contain the results of the job.
    // This output is specified as AssetCreationOptions.None, which
    // means the output asset is not encrypted.
    task.OutputAssets.AddNew("Output asset",
        AssetCreationOptions.None);

    job.StateChanged += new EventHandler<JobStateChangedEventArgs>(JobStateChanged);
    job.Submit();
    job.GetExecutionProgressTask(CancellationToken.None).Wait();

### <a name="json-preset"></a><span data-ttu-id="97605-204">JSON förinställda</span><span class="sxs-lookup"><span data-stu-id="97605-204">JSON preset</span></span>

<span data-ttu-id="97605-205">Uppdatera dina anpassade förinställningen med ID-numren för de resurser som du vill att sammanfoga och med lämplig tidpunkt segment för varje video.</span><span class="sxs-lookup"><span data-stu-id="97605-205">Update your custom preset with ids of the assets that you want to concatenate, and with the appropriate time segment for each video.</span></span>

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

## <span data-ttu-id="97605-206"><a id="crop"></a>Beskär videor med Media Encoder Standard</span><span class="sxs-lookup"><span data-stu-id="97605-206"><a id="crop"></a>Crop videos with Media Encoder Standard</span></span>
<span data-ttu-id="97605-207">Finns det [Beskär videor med Media Encoder Standard](media-services-crop-video.md) avsnittet.</span><span class="sxs-lookup"><span data-stu-id="97605-207">See the [Crop videos with Media Encoder Standard](media-services-crop-video.md) topic.</span></span>

## <span data-ttu-id="97605-208"><a id="no_video"></a>Infoga en video spåra när indata har ingen bild</span><span class="sxs-lookup"><span data-stu-id="97605-208"><a id="no_video"></a>Insert a video track when input has no video</span></span>

<span data-ttu-id="97605-209">Som standard om du skickar indata till den kodare som innehåller endast ljud och ingen bild innehåller utdatatillgången filer som innehåller endast ljuddata.</span><span class="sxs-lookup"><span data-stu-id="97605-209">By default, if you send an input to the encoder that contains only audio, and no video, then the output asset contains files that contain only audio data.</span></span> <span data-ttu-id="97605-210">Vissa spelare, inklusive Azure Media Player (se [detta](https://feedback.azure.com/forums/169396-azure-media-services/suggestions/8082468-audio-only-scenarios)) kanske inte kan hantera dessa strömmar.</span><span class="sxs-lookup"><span data-stu-id="97605-210">Some players, including Azure Media Player (see [this](https://feedback.azure.com/forums/169396-azure-media-services/suggestions/8082468-audio-only-scenarios)) may not be able to handle such streams.</span></span> <span data-ttu-id="97605-211">Du kan använda den här inställningen för att tvinga kodaren att lägga till en monokromt video Spåra utdata i scenariot.</span><span class="sxs-lookup"><span data-stu-id="97605-211">You can use this setting to force the encoder to add a monochrome video track to the output in that scenario.</span></span>

> [!NOTE]
> <span data-ttu-id="97605-212">Tvinga kodaren att infoga en video Spåra utdata ökar storleken på utdata tillgången, och därigenom kostnader för kodning uppgiften.</span><span class="sxs-lookup"><span data-stu-id="97605-212">Forcing the encoder to insert an output video track increases the size of the output Asset, and thereby the cost incurred for the encoding Task.</span></span> <span data-ttu-id="97605-213">Du bör köra tester för att kontrollera att denna gällande ökning har endast en liten inverkan på dina månatliga avgifter.</span><span class="sxs-lookup"><span data-stu-id="97605-213">You should run tests to verify that this resultant increase has only a modest impact on your monthly charges.</span></span>
>

### <a name="inserting-video-at-only-the-lowest-bitrate"></a><span data-ttu-id="97605-214">Infoga video på endast de lägsta bithastigheten</span><span class="sxs-lookup"><span data-stu-id="97605-214">Inserting video at only the lowest bitrate</span></span>

<span data-ttu-id="97605-215">Anta att du använder en flera kodning av bithastighet förinställningen som [”H264 Multibithastighet 720p”](media-services-mes-preset-h264-multiple-bitrate-720p.md) att koda hela inkommande katalogen för strömning, som innehåller en blandning av video och ljud-only-filer.</span><span class="sxs-lookup"><span data-stu-id="97605-215">Suppose you are using a multiple bitrate encoding preset such as ["H264 Multiple Bitrate 720p"](media-services-mes-preset-h264-multiple-bitrate-720p.md) to encode your entire input catalog for streaming, which contains a mix of video files and audio-only files.</span></span> <span data-ttu-id="97605-216">I detta scenario när indata har ingen bild kanske du vill tvinga kodaren Infoga monokromt video reda på just den lägsta bithastigheten i stället för att infoga video i varje utdata bithastighet.</span><span class="sxs-lookup"><span data-stu-id="97605-216">In this scenario, when the input has no video, you may want to force the encoder to insert a monochrome video track at just the lowest bitrate, as opposed to inserting video at every output bitrate.</span></span> <span data-ttu-id="97605-217">För att åstadkomma detta måste du använda den **InsertBlackIfNoVideoBottomLayerOnly** flaggan.</span><span class="sxs-lookup"><span data-stu-id="97605-217">To achieve this, you need to use the **InsertBlackIfNoVideoBottomLayerOnly** flag.</span></span>

<span data-ttu-id="97605-218">Du kan vidta någon av MES förinställningar dokumenterade i [detta](media-services-mes-presets-overview.md) avsnittet och gör följande ändringar:</span><span class="sxs-lookup"><span data-stu-id="97605-218">You can take any of the MES presets documented in [this](media-services-mes-presets-overview.md) section, and make the following modification:</span></span>

#### <a name="json-preset"></a><span data-ttu-id="97605-219">JSON förinställda</span><span class="sxs-lookup"><span data-stu-id="97605-219">JSON preset</span></span>
    {
          "KeyFrameInterval": "00:00:02",
          "StretchMode": "AutoSize",
          "Condition": "InsertBlackIfNoVideoBottomLayerOnly",
          "H264Layers": [
          …
          ]
    }

#### <a name="xml-preset"></a><span data-ttu-id="97605-220">XML-förinställda</span><span class="sxs-lookup"><span data-stu-id="97605-220">XML preset</span></span>

<span data-ttu-id="97605-221">Använd villkor när du använder XML = ”InsertBlackIfNoVideoBottomLayerOnly” som ett attribut till den **H264Video** element och villkor = ”InsertSilenceIfNoAudio” som ett attribut till **AACAudio**.</span><span class="sxs-lookup"><span data-stu-id="97605-221">When using XML, use Condition="InsertBlackIfNoVideoBottomLayerOnly" as an attribute to the **H264Video** element and  Condition="InsertSilenceIfNoAudio" as an attribute to **AACAudio**.</span></span>

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

### <a name="inserting-video-at-all-output-bitrates"></a><span data-ttu-id="97605-222">Infoga video alls utdata bithastighet</span><span class="sxs-lookup"><span data-stu-id="97605-222">Inserting video at all output bitrates</span></span>
<span data-ttu-id="97605-223">Anta att du använder en flera kodning av bithastighet förinställningen som [”H264 Multibithastighet 720p](media-services-mes-preset-H264-Multiple-Bitrate-720p.md) att koda hela inkommande katalogen för strömning, som innehåller en blandning av video och ljud-only-filer.</span><span class="sxs-lookup"><span data-stu-id="97605-223">Suppose you are using a multiple bitrate encoding preset such as ["H264 Multiple Bitrate 720p](media-services-mes-preset-H264-Multiple-Bitrate-720p.md) to encode your entire input catalog for streaming, which contains a mix of video files and audio-only files.</span></span> <span data-ttu-id="97605-224">I detta scenario när indata har ingen bild kanske du vill tvinga kodaren Infoga monokromt video reda på alla utdata bithastighet.</span><span class="sxs-lookup"><span data-stu-id="97605-224">In this scenario, when the input has no video, you may want to force the encoder to insert a monochrome video track at all the output bitrates.</span></span> <span data-ttu-id="97605-225">Detta säkerställer att din utdata tillgångar är alla homogen förhållande till antalet spårar video och ljud spår.</span><span class="sxs-lookup"><span data-stu-id="97605-225">This ensures that your output Assets are all homogenous with respect to number of video tracks and audio tracks.</span></span> <span data-ttu-id="97605-226">Du måste ange flaggan ”InsertBlackIfNoVideo” för att uppnå.</span><span class="sxs-lookup"><span data-stu-id="97605-226">To achieve this, you need to specify the "InsertBlackIfNoVideo" flag.</span></span>

<span data-ttu-id="97605-227">Du kan vidta någon av MES förinställningar dokumenterade i [detta](media-services-mes-presets-overview.md) avsnittet och gör följande ändringar:</span><span class="sxs-lookup"><span data-stu-id="97605-227">You can take any of the MES presets documented in [this](media-services-mes-presets-overview.md) section, and make the following modification:</span></span>

#### <a name="json-preset"></a><span data-ttu-id="97605-228">JSON förinställda</span><span class="sxs-lookup"><span data-stu-id="97605-228">JSON preset</span></span>
    {
          "KeyFrameInterval": "00:00:02",
          "StretchMode": "AutoSize",
          "Condition": "InsertBlackIfNoVideo",
          "H264Layers": [
          …
          ]
    }

#### <a name="xml-preset"></a><span data-ttu-id="97605-229">XML-förinställda</span><span class="sxs-lookup"><span data-stu-id="97605-229">XML preset</span></span>

<span data-ttu-id="97605-230">Använd villkor när du använder XML = ”InsertBlackIfNoVideo” som ett attribut till den **H264Video** element och villkor = ”InsertSilenceIfNoAudio” som ett attribut till **AACAudio**.</span><span class="sxs-lookup"><span data-stu-id="97605-230">When using XML, use Condition="InsertBlackIfNoVideo" as an attribute to the **H264Video** element and  Condition="InsertSilenceIfNoAudio" as an attribute to **AACAudio**.</span></span>

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

## <span data-ttu-id="97605-231"><a id="rotate_video"></a>Rotera en video</span><span class="sxs-lookup"><span data-stu-id="97605-231"><a id="rotate_video"></a>Rotate a video</span></span>
<span data-ttu-id="97605-232">Den [Media Encoder Standard](media-services-dotnet-encode-with-media-encoder-standard.md) stöder rotation av vinklar 0/90/180/270.</span><span class="sxs-lookup"><span data-stu-id="97605-232">The [Media Encoder Standard](media-services-dotnet-encode-with-media-encoder-standard.md) supports rotation by angles of 0/90/180/270.</span></span> <span data-ttu-id="97605-233">Standardinställningen är ”automatisk” där försöker identifiera rotation metadata i den inkommande videofilen och kompensera för den.</span><span class="sxs-lookup"><span data-stu-id="97605-233">The default behavior is "Auto", where it tries to detect the rotation metadata in the incoming video file and compensate for it.</span></span> <span data-ttu-id="97605-234">Inkludera följande **källor** element till ett av de förinställda som definierats i [detta](media-services-mes-presets-overview.md) avsnitt:</span><span class="sxs-lookup"><span data-stu-id="97605-234">Include the following **Sources** element to one of the presets defined in [this](media-services-mes-presets-overview.md) section:</span></span>

### <a name="json-preset"></a><span data-ttu-id="97605-235">JSON förinställda</span><span class="sxs-lookup"><span data-stu-id="97605-235">JSON preset</span></span>
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
### <a name="xml-preset"></a><span data-ttu-id="97605-236">XML-förinställda</span><span class="sxs-lookup"><span data-stu-id="97605-236">XML preset</span></span>
    <Sources>
           <Source>
          <Streams />
          <Filters>
            <Rotation>90</Rotation>
          </Filters>
        </Source>
    </Sources>

<span data-ttu-id="97605-237">Se även [detta](media-services-mes-schema.md#PreserveResolutionAfterRotation) avsnittet för mer information om hur kodaren tolkar bredd och höjd inställningarna i förinställningen när rotation ersättning utlöses.</span><span class="sxs-lookup"><span data-stu-id="97605-237">Also, see [this](media-services-mes-schema.md#PreserveResolutionAfterRotation) topic for more information on how the encoder interprets the Width and Height settings in the preset, when rotation compensation is triggered.</span></span>

<span data-ttu-id="97605-238">Du kan använda värdet ”0” för att visa kodaren att ignorera rotation metadata, om den finns i inkommande video.</span><span class="sxs-lookup"><span data-stu-id="97605-238">You can use the value "0" to indicate to the encoder to ignore rotation metadata, if present, in the input video.</span></span>

## <a name="media-services-learning-paths"></a><span data-ttu-id="97605-239">Sökvägar för Media Services-utbildning</span><span class="sxs-lookup"><span data-stu-id="97605-239">Media Services learning paths</span></span>
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a><span data-ttu-id="97605-240">Ge feedback</span><span class="sxs-lookup"><span data-stu-id="97605-240">Provide feedback</span></span>
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

## <a name="see-also"></a><span data-ttu-id="97605-241">Se även</span><span class="sxs-lookup"><span data-stu-id="97605-241">See Also</span></span>
[<span data-ttu-id="97605-242">Media Services Encoding översikt</span><span class="sxs-lookup"><span data-stu-id="97605-242">Media Services Encoding Overview</span></span>](media-services-encode-asset.md)
