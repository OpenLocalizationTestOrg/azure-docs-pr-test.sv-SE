---
title: "aaaOverview och jämförelse av Azure på kräver mediet kodare | Microsoft Docs"
description: "Det här avsnittet ger en översikt och jämförelse av Azure på begäran mediet kodare."
services: media-services
documentationcenter: 
author: juliako
manager: cfowler
editor: 
ms.assetid: e6bfc068-fa46-4d68-b1ce-9092c8f3a3c9
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/10/2017
ms.author: juliako
ms.openlocfilehash: 24a3e0a16162b1bebfcde290b6baf2dd8dbfff17
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="overview-and-comparison-of-azure-on-demand-media-encoders"></a>Översikt över och jämförelse av Azure på begäran mediet kodare
## <a name="encoding-overview"></a>Kodning: översikt
Azure Media Services erbjuder flera alternativ för hello-kodning av media i hello molnet.

När börjat med Media Services är det viktigt toounderstand hello skillnaden mellan codec och filformat.
Codec-rutiner är hello-programvara som implementerar hello komprimering/dekomprimering algoritmer filformat är behållare som innehåller hello komprimerade video.

Media Services tillhandahåller en dynamisk paketering som gör att du toodeliver din anpassningsbar bithastighet MP4 eller Smooth Streaming-kodade innehåll i strömningsformat som stöds av Media Services (MPEG DASH, HLS, Smooth Streaming) utan att behöva toore-paket till dessa strömningsformat.

>[!NOTE]
>När AMS-kontot skapas en **standard** strömningsslutpunkt har lagts till tooyour konto i hello **stoppad** tillstånd. toostart strömning ditt innehåll och dra nytta av dynamisk paketering och dynamisk kryptering hello strömningsslutpunkt som du vill toostream innehåll har toobe i hello **kör** tillstånd. tootake nytta av [dynamisk paketering](media-services-dynamic-packaging-overview.md), behöver du toodo hello följande:
>
>Dessutom koda källfilen till en uppsättning med anpassningsbar bithastighet MP4-filer eller Smooth Streaming-filer (hello kodningsstegen senare i den här självstudiekursen).

Media Services stöder följande hello på begäran kodare som beskrivs i den här artikeln:

* [Media Encoder Standard](media-services-encode-asset.md#media-encoder-standard)
* [Arbetsflöde för Media Encoder Premium](media-services-encode-asset.md#media-encoder-premium-workflow)

Den här artikeln ger en kort översikt över på begäran mediet kodare och ger länkar tooarticles som ger mer detaljerad information. hello avsnittet innehåller också jämförelse av hello kodare.

>[!NOTE]
>Som standard har varje Media Services-konto en aktiv kodning aktivitet i taget. Du kan reservera kodning enheter som tillåter toohave flera kodning aktiviteter som körs samtidigt, ett för varje kodning reserverad enhet som du köper. Mer information finns i [skalning kodning enheter](media-services-scale-media-processing-overview.md).

## <a name="media-encoder-standard"></a>Media Encoder Standard
### <a name="how-toouse"></a>Hur toouse
[Hur tooencode med Media Encoder Standard](media-services-dotnet-encode-with-media-encoder-standard.md)

### <a name="formats"></a>Format
[Format och codec](media-services-media-encoder-standard-formats.md)

### <a name="presets"></a>Förinställningar
Media Encoder Standard konfigureras med hjälp av en hello kodare förinställningar beskrivs [här](http://go.microsoft.com/fwlink/?linkid=618336&clcid=0x409).

### <a name="input-and-output-metadata"></a>Indata och utdata metadata
hello kodare inkommande metadata beskrivs [här](media-services-input-metadata-schema.md).

hello kodare utdata metadata beskrivs [här](media-services-output-metadata-schema.md).

### <a name="generate-thumbnails"></a>Generera miniatyrbilder
Mer information finns i [hur toogenerate miniatyrer med Media Encoder Standard](media-services-advanced-encoding-with-mes.md#thumbnails).

### <a name="trim-videos-clipping"></a>Trim videor (urklippet)
Mer information finns i [hur tootrim videor med Media Encoder Standard](media-services-advanced-encoding-with-mes.md#trim_video).

### <a name="create-overlays"></a>Skapa överlägg
Mer information finns i [hur toocreate överlägg med Media Encoder Standard](media-services-advanced-encoding-with-mes.md#overlay).

### <a name="see-also"></a>Se även
[hello Media Services-bloggen](https://azure.microsoft.com/blog/2015/07/16/announcing-the-general-availability-of-media-encoder-standard/)

## <a name="media-encoder-premium-workflow"></a>Arbetsflöde för Media Encoder Premium
### <a name="overview"></a>Översikt
[Introduktion till Premium-kodning i Azure Media Services](https://azure.microsoft.com/blog/2015/03/05/introducing-premium-encoding-in-azure-media-services/)

### <a name="how-toouse"></a>Hur toouse
Arbetsflöde för Media Encoder Premium konfigureras med hjälp av komplexa arbetsflöden. Arbetsflödesfiler kunde skapas och uppdateras med hello [Arbetsflödesdesignern](media-services-workflow-designer.md) verktyget.

[Hur tooUse Premium kodning i Azure Media Services](https://azure.microsoft.com/blog/2015/03/06/how-to-use-premium-encoding-in-azure-media-services/)

### <a name="known-issues"></a>Kända problem
Om din indatavideo inte innehåller textning, utdata hello tillgången fortfarande innehåller en tom TTML-fil.


## <a name="media-services-learning-paths"></a>Sökvägar för Media Services-utbildning
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Ge feedback
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

## <a name="related-articles"></a>Relaterade artiklar
* [Utföra avancerade kodning uppgifter genom att anpassa Media Encoder Standard förinställningar](media-services-custom-mes-presets-with-dotnet.md)
* [Kvoter och begränsningar](media-services-quotas-and-limitations.md)

<!--Reference links in article-->
[1]: http://azure.microsoft.com/pricing/details/media-services/
