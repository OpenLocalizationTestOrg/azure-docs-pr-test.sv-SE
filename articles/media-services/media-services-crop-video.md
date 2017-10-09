---
title: aaaHow toocrop videor med Media Encoder Standard - Azure | Microsoft Docs
description: "Den här artikeln visar hur toocrop videor med Media Encoder Standard."
services: media-services
documentationcenter: 
author: anilmur
manager: cfowler
editor: 
ms.assetid: 7628f674-2005-4531-8b61-d7a4f53e46ba
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 08/09/2017
ms.author: anilmur;juliako;
ms.openlocfilehash: 2b4ac3d96228b93c890a38c57c4913988de1e8bb
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="crop-videos-with-media-encoder-standard"></a>Beskär videoklipp med Media Encoder Standard
Du kan använda Media Encoder Standard (MES) toocrop din videoinmatning. Beskär är hello process för att välja ett rektangulärt fönster i hello bildruta och kodning bara hello bildpunkter i fönstret. följande diagram hello hjälper illustrera hello processen.

![Beskär en video](./media/media-services-crop-video/media-services-crop-video01.png)

Anta att du har som indata en video som har en upplösning på 1 920 x 1 080 bildpunkter (proportionerna 16:9), men har svarta fält (pelare rutor) på hello vänster och höger, så att endast en 4:3-fönstret eller 1440 x 1 080 bildpunkter innehåller active video. Du kan använda MES toocrop eller redigera ut hello svart staplar och koda hello 1440 x 1 080 region.

Beskär i MES är ett före bearbetning skede, så hello beskärningsverktyget parametrar i hello kodning förinställda gäller toohello ursprungliga indatavideo. Kodning är ett efterföljande steg och hello bredd och höjd inställningar gäller toohello *bearbetas före* video och inte toohello ursprungliga video. När du utformar din förinställda måste toodo hello följande: a väljer hello Beskär parametrar utifrån hello ursprungliga indatavideo och (b) din koda inställningar baserat på hello beskäras video. Om du inte matchar din koda inställningar toohello beskäras video, hello utdata blir inte som förväntat.

Hej [följande](media-services-custom-mes-presets-with-dotnet.md#encoding_with_dotnet) avsnittet visar hur toocreate ett kodningsjobb med MES och hur toospecify en anpassad förinställning för hello kodning aktivitet. 

## <a name="creating-a-custom-preset"></a>Skapa en anpassad förinställning
I hello hello diagram visas ett exempel:

1. Ursprungliga indata är 1 920 x 1 080
2. Den måste toobe beskäras tooan utdata från 1440 x 1 080, vilket är uppbyggd i hello inkommande ram
3. Detta innebär en X-förskjutning av (1 920 – 1 440) / 2 = 240 och en Y-förskjutning noll
4. hello bredd och höjd hello Beskär rektangeln är 1440 1080, respektive
5. I hello koda steget hello fråga är tooproduce tre lager, är lösningar 1440 x 1 080 960 x 720 och 480 x 360 respektive

### <a name="json-preset"></a>JSON förinställda
    {
      "Version": 1.0,
      "Sources": [
        {
          "Streams": [],
          "Filters": {
            "Crop": {
                "X": 240,
                "Y": 0,
                "Width": 1440,
                "Height": 1080
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
              "Bitrate": 3400,
              "MaxBitrate": 3400,
              "BufferWindow": "00:00:05",
              "Width": 1440,
              "Height": 1080,
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
              "Bitrate": 1250,
              "MaxBitrate": 1250,
              "BufferWindow": "00:00:05",
              "Width": 480,
              "Height": 360,
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


## <a name="restrictions-on-cropping"></a>Begränsningar för Beskär
hello Beskär funktionen är avsedd toobe manuell. Du skulle behöva tooload indata video till ett lämpligt redigeringsverktyg som låter dig välja ramar intressanta, placera hello markören toodetermine förskjutningar för hello Beskär rektangel, toodetermine hello kodning förinställning som är anpassad för att viss video, osv. Den här funktionen är inte avsedd tooenable t.ex: Automatisk identifiering och borttagning av svart letterbox/pillarbox-format kantlinjer i din videoinmatning.

Följande begränsningar gäller toohello Beskär funktionen. Om dessa inte är uppfyllda hello koda uppgiften misslyckas eller ett oväntat resultat.

1. hello har koordinater och storleken på hello Beskär rektangeln toofit inom hello indatavideo
2. Som nämnts ovan är hello bredd och höjd i hello koda inställningar har toocorrespond toohello beskäras video
3. Beskär gäller toovideos i liggande läge (d.v.s. inte tillämpligt toovideos registreras med en smartphone hålls lodrätt eller i stående läge)
4. Fungerar bäst med progressiv video till med kvadratisk bildpunkter

## <a name="provide-feedback"></a>Ge feedback
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

## <a name="next-step"></a>Nästa steg
Se Azure Media Services-utbildning sökvägar toohelp du lär dig mer om funktionerna som erbjuds av AMS.  

[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]
