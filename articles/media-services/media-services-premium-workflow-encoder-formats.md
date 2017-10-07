---
title: "aaaMedia kodare Premium arbetsflödet format och codec | Microsoft Docs"
description: "Det här avsnittet ger en översikt över arbetsflödet medieformat kodare Premium format och codec"
services: media-services
documentationcenter: 
author: juliako
manager: erik43
editor: 
ms.assetid: b197fce8-3b9b-4189-8d08-486810c0426f
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/09/2017
ms.author: juliako;anilmur
ms.openlocfilehash: e781384ca8f08926f00c83b6710fd413ce2a3e1c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="media-encoder-premium-workflow-formats-and-codecs"></a>Arbetsflöde för Media Encoder Premium format och codec
> [!NOTE]
> E-mepd på Microsoft.com för premium-kodare frågor.
> 
> Media Encoder Premium arbetsflöde medieprocessor som beskrivs i det här avsnittet är inte tillgänglig i Kina. 
> 
> 

Det här dokumentet innehåller en lista över inkommande och utgående filformat och codec-rutiner som stöds av hello offentliga förhandsversionen av hello **Media Encoder Premium arbetsflöde** kodare.

[Media Encoder Premium Worflow indata format och codec](#input_formats)

[Utdataformat för Media Encoder Premium Worflow och codec](#output_formats)

**Arbetsflöde för Media Encoder Premium** stöder textning beskrivs i [detta](#closed_captioning) avsnitt. 

## <a id="input_formats"></a>Arbetsflöde för Media Encoder Premium indata format och codec
hello efter avsnittet visar hello codec och filformat som har stöd för den här medieprocessor som indata.

### <a name="input-containerfile-formats"></a>Ange behållare/filformat
* Adobe® Flash® F4V
* MXF/SMPTE 377M
* GXF
* MPEG-2-transportströmmar
* MPEG-2-strömmar
* MP4-MPEG-4
* Windows Media/ASF
* AVI (okomprimerad 8-bitars/10 bitar)

### <a name="input-video-codecs"></a>Inkommande Video-codec
* AVC 8-/ 10-bitars, in too4:2:2, inklusive AVCIntra
* Avid DNxHD (i MXF)
* DVCPro/DVCProHD (i MXF)
* JPEG2000
* MPEG-2 (too422 profil och högnivå, inklusive varianter, till exempel XDCAM, XDCAM HD, XDCAM IMX, CableLabs® och D10)
* MPEG-1
* Windows Media Video-VC-1

### <a name="input-audio-codecs"></a>Inkommande ljud-codec
* AES (SMPTE 331 M och 302 M, AES3 2003)
* Dolby® E
* Dolby® Digital (AC3)
* AAC (AAC LC, HE AAC och AAC-HEv2 in too5.1)
* MPEG nivå 2
* MP3 (MPEG-1 ljud nivå 3)
* Windows Media Audio
* WAV PCM /

## <a id="output_format"></a>Media Encoder Premium arbetsflödet utdataformat och codec
hello innehåller följande avsnitt hello codec och filformat som stöds som utdata från den här medieprocessor.

### <a name="output-containerfile-formats"></a>Utdata behållare/filformat
* Adobe® Flash® F4V
* MXF (OP1a, XDCAM och AS02)
* DPP (inklusive AS11)
* GXF
* MP4-MPEG-4
* Windows Media/ASF
* AVI (okomprimerad 8-bitars/10 bitar)
* Filformatet för Smooth Streaming (PIFF 1.3)
* MPEG-TS 

### <a name="output-video-codecs"></a>Utdata Video-codec
* AVC (H.264; 8-bitars upp tooHigh profil, nivå 5.2; 4K extra HD; AVC Intra)
* Avid DNxHD (i MXF)
* DVCPro/DVCProHD (i MXF)
* MPEG-2 (too422 profil och högnivå, inklusive varianter, till exempel XDCAM, XDCAM HD, XDCAM IMX, CableLabs® och D10)
* MPEG-1
* Windows Media Video-VC-1
* JPEG-miniatyrer skapas

### <a name="output-audio-codecs"></a>Ljud-codec utdata
* AES (SMPTE 331 M och 302 M, AES3 2003)
* Dolby® Digital (AC3)
* Dolby® Digital Plus (E-AC3) in too7.1
* AAC (AAC LC, HE AAC och AAC-HEv2 in too5.1)
* MPEG nivå 2
* MP3 (MPEG-1 ljud nivå 3)
* Windows Media Audio

>[!NOTE]
>Om du kodar tooDolby® Digital (AC3) hello utdata kan bara skrivas till en ISO MP4-fil.

## <a id="closed_captioning"></a>Stöd för dold textning
På infognings **Media Encoder Premium arbetsflöde** stöder:

1. SCC filer
2. SMPTE TT filer
3. CEA-608/CEA-708 – som användardata (H.264 elementära strömmar, ATSC/53, SCTE20 SFI meddelanden) eller som extra data i MXF/GXF filer
4. STL underrubrik filer

Hello följande alternativ är tillgängliga på utdata:

1. CEA 608 tooCEA 708 översättning
2. CEA-608/CEA-708 passera (inbäddade i SFI meddelanden för H.264 elementära strömmar eller förs som extra data i MXF-filer)
3. SCC
4. SMPTE TT (från källan CEA 608 per SMPTE RP2052, även DFXP skapas)
5. SRT underrubrik fil
6. DVB underrubrik strömmar

Obs: stöds inte alla hello ovan utdataformat för leverans via strömning i Azure Media Services.

## <a name="known-issues"></a>Kända problem
Om din indatavideo inte innehåller textning, utdata hello tillgången fortfarande innehåller en tom TTML-fil. 

## <a name="media-services-learning-paths"></a>Sökvägar för Media Services-utbildning
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Ge feedback
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

