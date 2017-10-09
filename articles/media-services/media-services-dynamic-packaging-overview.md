---
title: "aaaAzure Media Services dynamisk paketering översikt | Microsoft Docs"
description: "Översikt över dynamisk paketering och hello avsnittet ger."
author: Juliako
manager: cfowler
editor: 
services: media-services
documentationcenter: 
ms.assetid: 0d9e4f54-5daa-45c1-bfaa-cf09ca89b812
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/29/2017
ms.author: juliako
ms.openlocfilehash: 970e24eba800e098774172c87f56629430b227a9
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="dynamic-packaging"></a>Dynamisk paketering
## <a name="overview"></a>Översikt
Microsoft Azure Media Services kan vara används toodeliver många media käll-filformat, strömmande medieformat och innehållsskydd format tooa mängd olika tekniker för klient (till exempel iOS, XBOX, Silverlight, Windows 8). Dessa klienter förstå olika protokoll, till exempel iOS kräver en HTTP Live Streaming (HLS) V4-format och Silverlight och Xbox kräver Smooth Streaming. Om du har en uppsättning med anpassningsbar bithastighet (multibithastighet) MP4 (ISO Base 14496 12) mediefiler eller en uppsättning med anpassningsbar bithastighet Smooth Streaming-filer som du vill tooserve tooclients som förstå MPEG DASH, HLS eller Smooth Streaming, bör du dra nytta av Media Services dynamisk paketering.

Med dynamisk paketering behöver du är toocreate en tillgång som innehåller en uppsättning med anpassningsbar bithastighet MP4-filer eller Smooth Streaming-filer. Sedan, baserat på hello format som anges i manifestet hello eller Fragmentera begäran, hello strömning på begäran kommer servern att säkerställa att du har fått hello dataström i hello-protokollet som du har valt. Därför behöver du bara toostore och betala för hello filer i ett enda lagringsformat och Media Services-tjänsten skapar och ger hello lämplig respons baserat på begäranden från en klient.

hello följande diagram visar hello traditionella kodning och statiska paketering arbetsflöde.

![Statisk kodning](./media/media-services-dynamic-packaging-overview/media-services-static-packaging.png)

hello visar följande diagram hello dynamisk paketering arbetsflöde.

![Dynamisk kodning](./media/media-services-dynamic-packaging-overview/media-services-dynamic-packaging.png)


## <a name="common-scenario"></a>Vanligt scenario
1. Överför en indatafil (kallas en mezzaninfil). Till exempel H.264 MP4 eller WMV (hello lista över de format som stöds finns [format som stöds av Media Encoder Standard hello](media-services-media-encoder-standard-formats.md).
2. Koda uppsättningarna mezzanine filen tooH.264 MP4 med anpassningsbar bithastighet.
3. Publicera hello tillgång som innehåller hello MP4-uppsättningen genom att skapa hello positionerare på begäran med anpassningsbar bithastighet.
4. Skapa hello tooaccess URL: er för strömning och strömma ditt innehåll.

## <a name="preparing-assets-for-dynamic-streaming"></a>Förbereda tillgångar för dynamiska strömning
tooprepare din tillgång för dynamiska direktuppspelning av du har två alternativ:

1. [Överföra en fil med master](media-services-dotnet-upload-files.md).
2. [Använd hello Media Encoder Standard-kodare tooproduce H.264 MP4 med anpassad bithastighet](media-services-dotnet-encode-with-media-encoder-standard.md).
3. [Strömma ditt innehåll](media-services-deliver-content-overview.md).

## <a id="unsupported_formats"></a>Format som inte stöds av dynamisk paketering
hello stöds följande källa format inte av dynamisk paketering.

* Dolby digital mp4-filer.
* Dolby digitala smooth filer.

## <a name="media-services-learning-paths"></a>Sökvägar för Media Services-utbildning
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Ge feedback
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

