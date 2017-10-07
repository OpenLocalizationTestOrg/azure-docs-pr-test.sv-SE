---
title: "aaaEncode en tillgång med Media Encoder Standard med hello Azure-portalen | Microsoft Docs"
description: "Den här självstudiekursen vägleder dig genom hello stegen för att koda en tillgång med Media Encoder Standard med hello Azure-portalen."
services: media-services
documentationcenter: 
author: Juliako
manager: cfowler
editor: 
ms.assetid: 107d9e9a-71e9-43e5-b17c-6e00983aceab
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/07/2017
ms.author: juliako
ms.openlocfilehash: 0d118bbbe1fa9f4ba0bfa3ea3b10fb541d1d6379
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="encode-an-asset-using-media-encoder-standard-with-hello-azure-portal"></a>Koda en tillgång med Media Encoder Standard med hello Azure-portalen
> [!NOTE]
> toocomplete den här självstudiekursen kommer du behöver ett Azure-konto. Mer information om den [kostnadsfria utvärderingsversionen av Azure](https://azure.microsoft.com/pricing/free-trial/). 
> 
> 

När du arbetar med Azure Media Services är ett av hello vanligaste scenarierna att leverera med anpassningsbar bithastighet strömmande tooyour klienter. Media Services stöder följande strömningstekniker med anpassningsbar bithastighet hello: HTTP Live Streaming (HLS), Smooth Streaming, MPEG DASH. tooprepare videor för anpassningsbar bithastighet strömning måste tooencode källfilerna video i flera bithastigheter filer. Du bör använda hello **Media Encoder Standard** kodare tooencode videor.  

Media Services tillhandahåller också dynamisk paketering som gör toodeliver din multibithastighet MP4s i hello följande strömningsformat: MPEG DASH, HLS, Smooth Streaming, utan att behöva toore-paket till dessa strömningsformat. Med dynamisk paketering behöver du bara toostore och betala för hello filer i ett enda lagringsformat och Media Services skapar och ger hello lämplig respons baserat på begäranden från en klient.

tootake nytta av dynamisk paketering behöver du behöver tooencode källfilen till en uppsättning med flera bithastigheter MP4-filer (hello kodningsstegen senare i det här avsnittet).

tooscale media bearbetning, se [detta](media-services-portal-scale-media-processing.md) avsnittet.

## <a name="encode-with-hello-azure-portal"></a>Koda med hello Azure-portalen
Det här avsnittet beskrivs hello åtgärder du kan vidta tooencode ditt innehåll med Media Encoder Standard.

1. I hello [Azure-portalen](https://portal.azure.com/), Välj Azure Media Services-konto.
2. I hello **inställningar** väljer **tillgångar**.  
3. I hello **tillgångar** fönster, Välj hello tillgång som du vill att tooencode.
4. Tryck på hello **koda** knappen.
5. I hello **koda en tillgång** fönster, Välj hello ”Media Encoder Standard” processor och en förinställning. Information om förinställningar finns i [auto-generate a bitrate ladder](media-services-autogen-bitrate-ladder-with-mes.md) (autogenerera en bithastighetsstege) och [Task Presets for MES](media-services-mes-presets-overview.md) (Uppgiftsförinställningar för MES). Om du planerar toocontrol vilka kodning förinställda används, Kom ihåg: det är viktigt tooselect hello förinställning som är mest lämpliga för din indatavideo. Till exempel om du vet att din indatavideo har en upplösning på 1 920 x 1 080 bildpunkter, du kan använda hello ”H264 Multibithastighet 1080p” förinställda. Om du har en video med låg upplösning (640 x 360) bör du inte använda förinställningen ”H264 multibithastighet 1080p”.
   
   Har en möjlighet att redigera hello av hello utdatatillgången och hello namn för hello jobbet för enklare hantering.
   
   ![Koda tillgångar](./media/media-services-portal-vod-get-started/media-services-encode1.png)
6. Tryck på **Skapa**.

## <a name="next-step"></a>Nästa steg
Du kan övervaka kodningsjobb med hello Azure-portalen, enligt beskrivningen i [detta](media-services-portal-check-job-progress.md) artikel.  

## <a name="media-services-learning-paths"></a>Sökvägar för Media Services-utbildning
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Ge feedback
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

