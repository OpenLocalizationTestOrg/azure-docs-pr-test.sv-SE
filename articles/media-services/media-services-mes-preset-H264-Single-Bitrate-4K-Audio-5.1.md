---
title: H264 Enkel bithastighet 4K ljud 5.1 | Microsoft Docs
description: "Avsnittet ger en översikt över de ** H264 enkel bithastighet 4K ljud 5.1* * aktiviteten förinställda."
author: Juliako
manager: cfowler
editor: 
services: media-services
documentationcenter: 
ms.assetid: 72cb95ac-2cd6-4ef4-9938-8f22cea04920
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/20/2017
ms.author: juliako
ms.openlocfilehash: d34fb5b640dbdbfc4d3d6b716fdd46176826a318
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/29/2017
---
# <a name="h264-single-bitrate-4k-audio-51"></a><span data-ttu-id="ab45b-103">H264 Enkel bithastighet 4K ljud 5.1</span><span class="sxs-lookup"><span data-stu-id="ab45b-103">H264 Single Bitrate 4K Audio 5.1</span></span>
<span data-ttu-id="ab45b-104">`Media Encoder Standard`definierar en uppsättning kodning förinställningar som du kan använda när du skapar kodning jobb.</span><span class="sxs-lookup"><span data-stu-id="ab45b-104">`Media Encoder Standard` defines a set of encoding presets you can use when creating encoding jobs.</span></span> <span data-ttu-id="ab45b-105">Du kan använda en `preset name` att ange i vilket format du vill koda media-fil.</span><span class="sxs-lookup"><span data-stu-id="ab45b-105">You can either use a `preset name` to specify into which format you would like to encode your media file.</span></span> <span data-ttu-id="ab45b-106">Du kan också skapa egna JSON eller XML-baserade förinställningar (med hjälp av UTF-8- eller UTF-16-kodning.</span><span class="sxs-lookup"><span data-stu-id="ab45b-106">Or, you can create your own JSON or XML-based presets (using UTF-8 or UTF-16 encoding.</span></span> <span data-ttu-id="ab45b-107">Du skulle sedan överföra anpassad förinställning på kodaren.</span><span class="sxs-lookup"><span data-stu-id="ab45b-107">You would then pass the custom preset to the encoder.</span></span> <span data-ttu-id="ab45b-108">Lista över alla förvalda namn som stöds av det här `Media Encoder Standard` kodare, se [aktivitet förinställningar för Media Encoder Standard](media-services-mes-presets-overview.md).</span><span class="sxs-lookup"><span data-stu-id="ab45b-108">For the list of all the preset names supported by this `Media Encoder Standard` encoder, see [Task Presets for Media Encoder Standard](media-services-mes-presets-overview.md).</span></span>  
  
 <span data-ttu-id="ab45b-109">Det här avsnittet beskrivs de `H264 Single Bitrate 4K Audio 5.1` förinställningen (i XML- och JSON-format).</span><span class="sxs-lookup"><span data-stu-id="ab45b-109">This topic shows the `H264 Single Bitrate 4K Audio 5.1` preset (in XML and JSON format).</span></span>  
  
 <span data-ttu-id="ab45b-110">Den här förinställningen producerar en enda MP4-fil med en bithastighet 18000 kbit/s och AAC 5.1 ljud.</span><span class="sxs-lookup"><span data-stu-id="ab45b-110">This preset produces a single MP4 file with a bitrate of 18000 kbps, and AAC 5.1 audio.</span></span> <span data-ttu-id="ab45b-111">Detaljerad information om profilen bithastighet, provtagning hastighet, etc. Om detta förinställda ska undersöka XML- eller JSON som anges nedan.</span><span class="sxs-lookup"><span data-stu-id="ab45b-111">For detailed information about profile, bitrate, sampling rate, etc. of this preset, examine the XML or JSON defined below.</span></span> <span data-ttu-id="ab45b-112">Förklaringar av vad varje element betyder och giltiga värden för varje element finns i [Media Encoder Standard schemat](media-services-mes-schema.md).</span><span class="sxs-lookup"><span data-stu-id="ab45b-112">For explanations of what each element means, and the valid values for each element, see the [Media Encoder Standard schema](media-services-mes-schema.md).</span></span>  
  
> [!NOTE]
>  <span data-ttu-id="ab45b-113">Du bör få Premium reserverad enhet typ med 4K kodar.</span><span class="sxs-lookup"><span data-stu-id="ab45b-113">You should get the Premium reserved unit type with 4K encodes.</span></span> <span data-ttu-id="ab45b-114">Mer information finns i [så skala kodning](https://azure.microsoft.com/en-us/documentation/articles/media-services-portal-encoding-units).</span><span class="sxs-lookup"><span data-stu-id="ab45b-114">For more information, see [How to Scale Encoding](https://azure.microsoft.com/en-us/documentation/articles/media-services-portal-encoding-units).</span></span>  
  
 <span data-ttu-id="ab45b-115">XML</span><span class="sxs-lookup"><span data-stu-id="ab45b-115">XML</span></span>  
  
```  
<?xml version="1.0" encoding="utf-16"?>  
<Preset xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" Version="1.0" xmlns="http://www.windowsazure.com/media/encoding/Preset/2014/03">  
  <Encoding>  
    <H264Video>  
      <KeyFrameInterval>00:00:02</KeyFrameInterval>  
      <SceneChangeDetection>true</SceneChangeDetection>  
      <H264Layers>  
        <H264Layer>  
          <Bitrate>18000</Bitrate>  
          <Width>3840</Width>  
          <Height>2160</Height>  
          <FrameRate>0/1</FrameRate>  
          <Profile>Auto</Profile>  
          <Level>auto</Level>  
          <BFrames>3</BFrames>  
          <ReferenceFrames>3</ReferenceFrames>  
          <Slices>0</Slices>  
          <AdaptiveBFrame>true</AdaptiveBFrame>  
          <EntropyMode>Cabac</EntropyMode>  
          <BufferWindow>00:00:05</BufferWindow>  
          <MaxBitrate>18000</MaxBitrate>  
        </H264Layer>  
      </H264Layers>  
      <Chapters />  
    </H264Video>  
    <AACAudio>  
      <Profile>AACLC</Profile>  
      <Channels>6</Channels>  
      <SamplingRate>48000</SamplingRate>  
      <Bitrate>384</Bitrate>  
    </AACAudio>  
  </Encoding>  
  <Outputs>  
    <Output FileName="{Basename}_{Width}x{Height}_{VideoBitrate}.mp4">  
      <MP4Format />  
    </Output>  
  </Outputs>  
</Preset>  
```  
  
 <span data-ttu-id="ab45b-116">JSON</span><span class="sxs-lookup"><span data-stu-id="ab45b-116">JSON</span></span>  
  
```  
{  
  "Version": 1.0,  
  "Codecs": [  
    {  
      "KeyFrameInterval": "00:00:02",  
      "SceneChangeDetection": true,  
      "H264Layers": [  
        {  
          "Profile": "Auto",  
          "Level": "auto",  
          "Bitrate": 18000,  
          "MaxBitrate": 18000,  
          "BufferWindow": "00:00:05",  
          "Width": 3840,  
          "Height": 2160,  
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
      "Channels": 6,  
      "SamplingRate": 48000,  
      "Bitrate": 384,  
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
```
