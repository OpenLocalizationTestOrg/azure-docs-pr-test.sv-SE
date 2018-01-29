---
title: "H264 enkel bithastighet 4 x 3 SD Media Encoder Standard förinställda - Azure | Microsoft Docs"
description: "Avsnittet ger en översikt över de ** H264 enkel bithastighet 4 x 3 SD ** uppgiften förinställda."
author: Juliako
manager: cfowler
editor: 
services: media-services
documentationcenter: 
ms.assetid: 171689fe-7c4f-4d5a-b48e-281136d8ac97
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/20/2017
ms.author: juliako
ms.openlocfilehash: 61fac597c6e9ee425cedd1df2d819acebb148280
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/11/2017
---
# <a name="h264-single-bitrate-4x3-sd"></a>H264 Enkel bithastighet 4 x 3 SD
`Media Encoder Standard`definierar en uppsättning kodning förinställningar som du kan använda när du skapar kodning jobb. Du kan använda en `preset name` att ange i vilket format du vill koda media-fil. Du kan också skapa egna JSON eller XML-baserade förinställningar (med hjälp av UTF-8- eller UTF-16-kodning. Du skulle sedan överföra anpassad förinställning på kodaren. Lista över alla förvalda namn som stöds av det här `Media Encoder Standard` kodare, se [aktivitet förinställningar för Media Encoder Standard](media-services-mes-presets-overview.md).  
  
 Det här avsnittet beskrivs de `H264 Single Bitrate 4x3 SD` förinställda XML och JSON-format.  
  
 Den här förinställda ger en enda MP4-fil med en bithastighet 1800 kbit/s och AAC stereoljud. Detaljerad information om profilen bithastighet, provtagning hastighet, etc. Om detta förinställda ska undersöka XML- eller JSON som anges nedan. Förklaringar av vad varje element i dessa förinställda innebär och giltiga värden för varje element finns i [Media Encoder Standard schemat](media-services-mes-schema.md) avsnittet.  
  
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
          <Bitrate>1800</Bitrate>  
          <Width>640</Width>  
          <Height>480</Height>  
          <FrameRate>0/1</FrameRate>  
          <Profile>Auto</Profile>  
          <Level>auto</Level>  
          <BFrames>3</BFrames>  
          <ReferenceFrames>3</ReferenceFrames>  
          <Slices>0</Slices>  
          <AdaptiveBFrame>true</AdaptiveBFrame>  
          <EntropyMode>Cabac</EntropyMode>  
          <BufferWindow>00:00:05</BufferWindow>  
          <MaxBitrate>1800</MaxBitrate>  
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
          "Bitrate": 1800,  
          "MaxBitrate": 1800,  
          "BufferWindow": "00:00:05",  
          "Width": 640,  
          "Height": 480,  
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
