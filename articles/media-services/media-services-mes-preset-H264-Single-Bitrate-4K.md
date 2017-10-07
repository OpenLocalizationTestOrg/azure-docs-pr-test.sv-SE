---
title: "aaaH264 enkel bithastighet 4K Media Encoder Standard förinställda - Azure | Microsoft Docs"
description: "hello avsnittet ger en översikt över hello ** H264 enkel bithastighet 4 K ** uppgiften förinställda."
author: Juliako
manager: cfowler
editor: 
services: media-services
documentationcenter: 
ms.assetid: 8e437aea-8193-49a0-9ff2-4fd391c80972
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/20/2017
ms.author: juliako
ms.openlocfilehash: 3a88207ba89baaefddfea631aa5d4c1c74194c68
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="h264-single-bitrate-4k"></a>H264 Med enkel bithastighet 4K
`Media Encoder Standard`definierar en uppsättning kodning förinställningar som du kan använda när du skapar kodning jobb. Du kan använda en `preset name` toospecify i vilket format du vill att tooencode media-fil. Du kan också skapa egna JSON eller XML-baserade förinställningar (med hjälp av UTF-8- eller UTF-16-kodning. Du skulle sedan överföra hello anpassade förinställda toohello kodare. Hello lista över alla hello förinställningen namn som stöds av det här `Media Encoder Standard` kodare, se [aktivitet förinställningar för Media Encoder Standard](media-services-mes-presets-overview.md).  
  
 Det här avsnittet visar hello `H264 Single Bitrate 4K` förinställda XML och JSON-format.  
  
 Den här förinställda ger en enda MP4-fil med en bithastighet 18000 kbit/s och AAC stereoljud. Detaljerad information om profilen bithastighet, provtagning hastighet, etc. Om detta förinställda ska undersöka hello XML- eller JSON som anges nedan. Förklaringar av vad varje element i dessa förinställda innebär och hello giltiga värden för varje element finns hello [Media Encoder Standard schemat](media-services-mes-schema.md) avsnittet.  
  
> [!NOTE]
>  Du bör få hello Premium reserverade enhetstyp med 4K kodar. Mer information finns i [hur tooScale kodning](https://azure.microsoft.com/en-us/documentation/articles/media-services-portal-encoding-units).  
  
 XML  
  
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
```  
  
 JSON  
  
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
```
