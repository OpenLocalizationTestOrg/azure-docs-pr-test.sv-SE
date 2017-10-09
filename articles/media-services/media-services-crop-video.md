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
# <a name="crop-videos-with-media-encoder-standard"></a><span data-ttu-id="aeafa-103">Beskär videoklipp med Media Encoder Standard</span><span class="sxs-lookup"><span data-stu-id="aeafa-103">Crop videos with Media Encoder Standard</span></span>
<span data-ttu-id="aeafa-104">Du kan använda Media Encoder Standard (MES) toocrop din videoinmatning.</span><span class="sxs-lookup"><span data-stu-id="aeafa-104">You can use Media Encoder Standard (MES) toocrop your input video.</span></span> <span data-ttu-id="aeafa-105">Beskär är hello process för att välja ett rektangulärt fönster i hello bildruta och kodning bara hello bildpunkter i fönstret.</span><span class="sxs-lookup"><span data-stu-id="aeafa-105">Cropping is hello process of selecting a rectangular window within hello video frame, and encoding just hello pixels within that window.</span></span> <span data-ttu-id="aeafa-106">följande diagram hello hjälper illustrera hello processen.</span><span class="sxs-lookup"><span data-stu-id="aeafa-106">hello following diagram helps illustrate hello process.</span></span>

![Beskär en video](./media/media-services-crop-video/media-services-crop-video01.png)

<span data-ttu-id="aeafa-108">Anta att du har som indata en video som har en upplösning på 1 920 x 1 080 bildpunkter (proportionerna 16:9), men har svarta fält (pelare rutor) på hello vänster och höger, så att endast en 4:3-fönstret eller 1440 x 1 080 bildpunkter innehåller active video.</span><span class="sxs-lookup"><span data-stu-id="aeafa-108">Suppose you have as input a video that has a resolution of 1920x1080 pixels (16:9 aspect ratio), but has black bars (pillar boxes) at hello left and right, so that only a 4:3 window or 1440x1080 pixels contains active video.</span></span> <span data-ttu-id="aeafa-109">Du kan använda MES toocrop eller redigera ut hello svart staplar och koda hello 1440 x 1 080 region.</span><span class="sxs-lookup"><span data-stu-id="aeafa-109">You can use MES toocrop or edit out hello black bars, and encode hello 1440x1080 region.</span></span>

<span data-ttu-id="aeafa-110">Beskär i MES är ett före bearbetning skede, så hello beskärningsverktyget parametrar i hello kodning förinställda gäller toohello ursprungliga indatavideo.</span><span class="sxs-lookup"><span data-stu-id="aeafa-110">Cropping in MES is a pre-processing stage, so hello cropping parameters in hello encoding preset apply toohello original input video.</span></span> <span data-ttu-id="aeafa-111">Kodning är ett efterföljande steg och hello bredd och höjd inställningar gäller toohello *bearbetas före* video och inte toohello ursprungliga video.</span><span class="sxs-lookup"><span data-stu-id="aeafa-111">Encoding is a subsequent stage, and hello width/height settings apply toohello *pre-processed* video, and not toohello original video.</span></span> <span data-ttu-id="aeafa-112">När du utformar din förinställda måste toodo hello följande: a väljer hello Beskär parametrar utifrån hello ursprungliga indatavideo och (b) din koda inställningar baserat på hello beskäras video.</span><span class="sxs-lookup"><span data-stu-id="aeafa-112">When designing your preset you need toodo hello following: (a) select hello crop parameters based on hello original input video, and (b) select your encode settings based on hello cropped video.</span></span> <span data-ttu-id="aeafa-113">Om du inte matchar din koda inställningar toohello beskäras video, hello utdata blir inte som förväntat.</span><span class="sxs-lookup"><span data-stu-id="aeafa-113">If you do not match your encode settings toohello cropped video, hello output will not be as you expect.</span></span>

<span data-ttu-id="aeafa-114">Hej [följande](media-services-custom-mes-presets-with-dotnet.md#encoding_with_dotnet) avsnittet visar hur toocreate ett kodningsjobb med MES och hur toospecify en anpassad förinställning för hello kodning aktivitet.</span><span class="sxs-lookup"><span data-stu-id="aeafa-114">hello [following](media-services-custom-mes-presets-with-dotnet.md#encoding_with_dotnet) topic shows how toocreate an encoding job with MES and how toospecify a custom preset for hello encoding task.</span></span> 

## <a name="creating-a-custom-preset"></a><span data-ttu-id="aeafa-115">Skapa en anpassad förinställning</span><span class="sxs-lookup"><span data-stu-id="aeafa-115">Creating a custom preset</span></span>
<span data-ttu-id="aeafa-116">I hello hello diagram visas ett exempel:</span><span class="sxs-lookup"><span data-stu-id="aeafa-116">In hello example shown in hello diagram:</span></span>

1. <span data-ttu-id="aeafa-117">Ursprungliga indata är 1 920 x 1 080</span><span class="sxs-lookup"><span data-stu-id="aeafa-117">Original input is 1920x1080</span></span>
2. <span data-ttu-id="aeafa-118">Den måste toobe beskäras tooan utdata från 1440 x 1 080, vilket är uppbyggd i hello inkommande ram</span><span class="sxs-lookup"><span data-stu-id="aeafa-118">It needs toobe cropped tooan output of 1440x1080, which is centered in hello input frame</span></span>
3. <span data-ttu-id="aeafa-119">Detta innebär en X-förskjutning av (1 920 – 1 440) / 2 = 240 och en Y-förskjutning noll</span><span class="sxs-lookup"><span data-stu-id="aeafa-119">This means an X offset of (1920 – 1440)/2 = 240, and a Y offset of zero</span></span>
4. <span data-ttu-id="aeafa-120">hello bredd och höjd hello Beskär rektangeln är 1440 1080, respektive</span><span class="sxs-lookup"><span data-stu-id="aeafa-120">hello Width and Height of hello Crop rectangle are 1440 and 1080, respectively</span></span>
5. <span data-ttu-id="aeafa-121">I hello koda steget hello fråga är tooproduce tre lager, är lösningar 1440 x 1 080 960 x 720 och 480 x 360 respektive</span><span class="sxs-lookup"><span data-stu-id="aeafa-121">In hello encode stage, hello ask is tooproduce three layers, are resolutions 1440x1080, 960x720 and 480x360, respectively</span></span>

### <a name="json-preset"></a><span data-ttu-id="aeafa-122">JSON förinställda</span><span class="sxs-lookup"><span data-stu-id="aeafa-122">JSON preset</span></span>
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


## <a name="restrictions-on-cropping"></a><span data-ttu-id="aeafa-123">Begränsningar för Beskär</span><span class="sxs-lookup"><span data-stu-id="aeafa-123">Restrictions on cropping</span></span>
<span data-ttu-id="aeafa-124">hello Beskär funktionen är avsedd toobe manuell.</span><span class="sxs-lookup"><span data-stu-id="aeafa-124">hello cropping feature is meant toobe manual.</span></span> <span data-ttu-id="aeafa-125">Du skulle behöva tooload indata video till ett lämpligt redigeringsverktyg som låter dig välja ramar intressanta, placera hello markören toodetermine förskjutningar för hello Beskär rektangel, toodetermine hello kodning förinställning som är anpassad för att viss video, osv. Den här funktionen är inte avsedd tooenable t.ex: Automatisk identifiering och borttagning av svart letterbox/pillarbox-format kantlinjer i din videoinmatning.</span><span class="sxs-lookup"><span data-stu-id="aeafa-125">You would need tooload your input video into a suitable editing tool that lets you select frames of interest, position hello cursor toodetermine offsets for hello cropping rectangle, toodetermine hello encoding preset that is tuned for that particular video, etc. This feature is not meant tooenable things like: automatic detection and removal of black letterbox/pillarbox borders in your input video.</span></span>

<span data-ttu-id="aeafa-126">Följande begränsningar gäller toohello Beskär funktionen.</span><span class="sxs-lookup"><span data-stu-id="aeafa-126">Following constraints apply toohello cropping feature.</span></span> <span data-ttu-id="aeafa-127">Om dessa inte är uppfyllda hello koda uppgiften misslyckas eller ett oväntat resultat.</span><span class="sxs-lookup"><span data-stu-id="aeafa-127">If these are not met, hello encode Task can fail, or produce an unexpected output.</span></span>

1. <span data-ttu-id="aeafa-128">hello har koordinater och storleken på hello Beskär rektangeln toofit inom hello indatavideo</span><span class="sxs-lookup"><span data-stu-id="aeafa-128">hello co-ordinates and size of hello Crop rectangle have toofit within hello input video</span></span>
2. <span data-ttu-id="aeafa-129">Som nämnts ovan är hello bredd och höjd i hello koda inställningar har toocorrespond toohello beskäras video</span><span class="sxs-lookup"><span data-stu-id="aeafa-129">As mentioned above, hello Width & Height in hello encode settings have toocorrespond toohello cropped video</span></span>
3. <span data-ttu-id="aeafa-130">Beskär gäller toovideos i liggande läge (d.v.s. inte tillämpligt toovideos registreras med en smartphone hålls lodrätt eller i stående läge)</span><span class="sxs-lookup"><span data-stu-id="aeafa-130">Cropping applies toovideos captured in landscape mode (i.e. not applicable toovideos recorded with a smartphone held vertically or in portrait mode)</span></span>
4. <span data-ttu-id="aeafa-131">Fungerar bäst med progressiv video till med kvadratisk bildpunkter</span><span class="sxs-lookup"><span data-stu-id="aeafa-131">Works best with progressive video captured with square pixels</span></span>

## <a name="provide-feedback"></a><span data-ttu-id="aeafa-132">Ge feedback</span><span class="sxs-lookup"><span data-stu-id="aeafa-132">Provide feedback</span></span>
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

## <a name="next-step"></a><span data-ttu-id="aeafa-133">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="aeafa-133">Next step</span></span>
<span data-ttu-id="aeafa-134">Se Azure Media Services-utbildning sökvägar toohelp du lär dig mer om funktionerna som erbjuds av AMS.</span><span class="sxs-lookup"><span data-stu-id="aeafa-134">See Azure Media Services learning paths toohelp you learn about great features offered by AMS.</span></span>  

[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]
