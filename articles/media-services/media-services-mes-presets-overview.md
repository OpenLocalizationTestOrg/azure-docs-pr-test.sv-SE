---
title: "aaaTask förinställningar för MES (Media Encoder Standard) | Microsoft Docs"
description: "hello avsnittet ger och översikt över aktivitet förinställningar för MES (Media Encoder Standard)."
author: Juliako
manager: cfowler
editor: 
services: media-services
documentationcenter: 
ms.assetid: f243ed1c-ac9c-4300-a5f7-f092cf9853b9
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/19/2017
ms.author: juliako
ms.openlocfilehash: 56e098d6d8c8f84031421ec59f087f20370ba111
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="task-presets-for-mes-media-encoder-standard"></a>Uppgiften förinställningar för MES (Media Encoder Standard)

**Media Encoder Standard** definierar en uppsättning kodning förinställningar som du kan använda när du skapar kodning jobb. Det rekommenderas toouse hello ”anpassningsbar strömning” förinställningen om du vill tooencode en video för strömning med Media Services. När du anger detta förinställda, Media Encoder Standard tar [Autogenerera en bithastighet stege](media-services-autogen-bitrate-ladder-with-mes.md). 

Om du behöver toocustomize en kodning förinställning, bör du dock göra något av hello kodning förinställningar som definierats i det här avsnittet som en mall för en anpassad konfiguration. Förklaringar av vad varje element i dessa förinställda innebär och hello giltiga värden för varje element finns hello [Media Encoder Standard schemat](media-services-mes-schema.md) avsnittet.  
  
> [!NOTE]
>  När du använder en förinställning för 4k kodar, ska du hämta hello `S3` reserverade enhetstyp. Mer information finns i [hur tooScale kodning](https://azure.microsoft.com/en-us/documentation/articles/media-services-portal-encoding-units).  
  
När du arbetar med Media Encoder Standard är rotation aktiverad som standard. Om videon har registrerats på en smartphone eller annan mobil enhet i stående läge och sedan dessa förinställningar kommer som standard rotera dem tooLandscape läge tidigare tooencoding (till skillnad från, när du arbetar med Azure Media Encoder, där video rotation är en manuell åtgärd, enligt beskrivningen i [detta](http://azure.microsoft.com/blog/2014/08/21/advanced-encoding-features-in-azure-media-encoder/) blogg under ”Video Rotation”).  
  
Tillgängliga förinställningar:  
  
 [H264 Multibithastighet 1080p ljud 5.1](media-services-mes-preset-H264-Multiple-Bitrate-1080p-Audio-5.1.md) producerar en uppsättning 8 GOP-justerad MP4-filer från 6000 kbit/s too400 kbit/s och AAC 5.1 ljud.  
  
 [H264 Multibithastighet 1080p](media-services-mes-preset-H264-Multiple-Bitrate-1080p.md) producerar en uppsättning 8 GOP-justerad MP4-filer från 6000 kbit/s too400 kbit/s och AAC stereoljud.  
  
 [H264 Multibithastighet 16 x 9 för iOS](media-services-mes-preset-H264-Multiple-Bitrate-16x9-for-iOS.md) producerar en uppsättning 8 GOP-justerad MP4-filer från 8500 kbit/s too200 kbit/s och AAC stereoljud.  
  
 [H264 Multibithastighet 16 x 9 SD ljud 5.1](media-services-mes-preset-H264-Multiple-Bitrate-16x9-SD-Audio-5.1.md) producerar en uppsättning 5 GOP-justerad MP4-filer från 1900 kbit/s too400 kbit/s och AAC 5.1 ljud.  
  
 [H264 Multibithastighet 16 x 9 SD](media-services-mes-preset-H264-Multiple-Bitrate-16x9-SD.md) producerar en uppsättning 5 GOP-justerad MP4-filer från 1900 kbit/s too400 kbit/s och AAC stereoljud.  
  
 [H264 Multibithastighet 4K ljud 5.1](media-services-mes-preset-H264-Multiple-Bitrate-4K-Audio-5.1.md) producerar en uppsättning 12 GOP-justerad MP4-filer från 20000 kbit/s too1000 kbit/s och AAC 5.1 ljud.  
  
 [H264 Multibithastighet 4K](media-services-mes-preset-H264-Multiple-Bitrate-4K.md) producerar en uppsättning 12 GOP-justerad MP4-filer från 20000 kbit/s too1000 kbit/s och AAC stereoljud.  
  
 [H264 Multibithastighet 4 x 3 för iOS](media-services-mes-preset-H264-Multiple-Bitrate-4x3-for-iOS.md) producerar en uppsättning 8 GOP-justerad MP4-filer från 8500 kbit/s too200 kbit/s och AAC stereoljud.  
  
 [H264 Multibithastighet 4 x 3 SD ljud 5.1](media-services-mes-preset-H264-Multiple-Bitrate-4x3-SD-Audio-5.1.md) producerar en uppsättning 5 GOP-justerad MP4-filer från 1600 kbit/s too400 kbit/s och AAC 5.1 ljud.  
  
 [H264 Multibithastighet 4 x 3 SD](media-services-mes-preset-H264-Multiple-Bitrate-4x3-SD.md) producerar en uppsättning 5 GOP-justerad MP4-filer från 1600 kbit/s too400 kbit/s och AAC stereoljud.  
  
 [H264 Multibithastighet 720p ljud 5.1](media-services-mes-preset-H264-Multiple-Bitrate-720p-Audio-5.1.md) producerar en uppsättning 6 GOP-justerad MP4-filer från 3400 kbit/s too400 kbit/s och AAC 5.1 ljud.  
  
 [H264 Multibithastighet 720p](media-services-mes-preset-H264-Multiple-Bitrate-720p.md) producerar en uppsättning 6 GOP-justerad MP4-filer från 3400 kbit/s too400 kbit/s och AAC stereoljud.  
  
 [H264 enkel bithastighet 1080p ljud 5.1](media-services-mes-preset-H264-Single-Bitrate-1080p-Audio-5.1.md) producerar en enda MP4-fil med en bithastighet 6750 kbit/s och AAC 5.1 ljud.  
  
 [H264 enkel bithastighet 1080p](media-services-mes-preset-H264-Single-Bitrate-1080p.md) producerar en enda MP4-fil med en bithastighet 6750 kbit/s och AAC stereoljud.  
  
 [H264 enkel bithastighet 4K ljud 5.1](media-services-mes-preset-H264-Single-Bitrate-4K-Audio-5.1.md) producerar en enda MP4-fil med en bithastighet 18000 kbit/s och AAC 5.1 ljud.  
  
 [H264 enkel bithastighet 4K](media-services-mes-preset-H264-Single-Bitrate-4K.md) producerar en enda MP4-fil med en bithastighet 18000 kbit/s och AAC stereoljud.  
  
 [H264 enkel bithastighet 4 x 3 SD ljud 5.1](media-services-mes-preset-H264-Single-Bitrate-4x3-SD-Audio-5.1.md) producerar en enda MP4-fil med en bithastighet 1800 kbit/s och AAC 5.1 ljud.  
  
 [H264 enkel bithastighet 4 x 3 SD](media-services-mes-preset-H264-Single-Bitrate-4x3-SD.md) producerar en enda MP4-fil med en bithastighet 1800 kbit/s och AAC stereoljud.  
  
 [H264 enkel bithastighet 16 x 9 SD ljud 5.1](media-services-mes-preset-H264-Single-Bitrate-16x9-SD-Audio-5.1.md) producerar en enda MP4-fil med en bithastighet 2200 kbit/s och AAC 5.1 ljud.  
  
 [H264 enkel bithastighet 16 x 9 SD](media-services-mes-preset-H264-Single-Bitrate-16x9-SD.md) producerar en enda MP4-fil med en bithastighet 2200 kbit/s och AAC stereoljud.  
  
 [H264 enkel bithastighet 720p ljud 5.1](media-services-mes-preset-H264-Single-Bitrate-720p-Audio-5.1.md) producerar en enda MP4-fil med en bithastighet 4500 kbit/s och AAC 5.1 ljud.  
  
 [H264 enkel bithastighet 720p för Android](media-services-mes-preset-H264-Single-Bitrate-720p-for-Android.md) förinställda producerar en enda MP4-fil med en bithastighet 2000 kbit/s och stereo AAC.  
  
 [H264 enkel bithastighet 720p](media-services-mes-preset-H264-Single-Bitrate-720p.md) producerar en enda MP4-fil med en bithastighet 4500 kbit/s och AAC stereoljud.  
  
 [H264 enkel bithastighet hög kvalitet SD för Android](media-services-mes-preset-H264-Single-Bitrate-High-Quality-SD-for-Android.md) producerar en enda MP4-fil med en bithastighet 500 kbit/s och AAC stereoljud...  
  
 [H264 enkel bithastighet låg kvalitet SD för Android](media-services-mes-preset-H264-Single-Bitrate-Low-Quality-SD-for-Android.md) producerar en enda MP4-fil med en bithastighet 56 kbit/s och AAC stereoljud.  
  
 Mer information finns i Närliggande tooMedia Services kodare [kodning på begäran med Azure Media Services](https://azure.microsoft.com/en-us/documentation/articles/media-services-encode-asset/).
