---
title: "Koda en tillgång med Media Encoder Standard med Azure-portalen | Microsoft Docs"
description: "Den här självstudiekursen vägleder dig genom stegen för att koda en tillgång med Media Encoder Standard med Azure-portalen."
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
ms.openlocfilehash: a299245e285c4caa68988b184799cd6f4d13e080
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/29/2017
---
# <a name="encode-an-asset-using-media-encoder-standard-with-the-azure-portal"></a>Koda en tillgång med Media Encoder Standard med Azure-portalen
> [!NOTE]
> Du behöver ett Azure-konto för att slutföra den här självstudien. Mer information om den [kostnadsfria utvärderingsversionen av Azure](https://azure.microsoft.com/pricing/free-trial/). 
> 
> 

När du arbetar med Azure Media Services är ett av de vanligaste scenarierna att leverera strömning med anpassad bithastighet till dina klienter. Media Services har stöd för följande strömningstekniker med anpassningsbar bithastighet: HTTP-liveuppspelning (HLS), jämn direktuppspelning, MPEG DASH. För att förbereda dina videor för strömning med anpassad bithastighet måste du koda källvideon till filer i multibithastighet. Du bör använda kodaren **Media Encoder Standard** för att koda dina videor.  

Media Services tillhandahåller också dynamisk paketering som gör att du kan leverera dina MP4s multibithastighet i följande strömningsformat: MPEG DASH, HLS, Smooth Streaming, utan att du behöver packa om till dessa strömningsformat. Med dynamisk paketering behöver du bara lagra och betala för filerna i ett enda lagringsformat, och Media Services skapar och ger lämplig respons baserat på begäranden från en klient.

Om du vill dra nytta av dynamisk paketering måste du koda din källfil till en uppsättning MP4-filer med flera bithastigheter (kodningsstegen visas längre fram i det här avsnittet).

Om du vill skala media bearbetning, se [detta](media-services-portal-scale-media-processing.md) avsnittet.

## <a name="encode-with-the-azure-portal"></a>Koda med Azure-portalen
I det här avsnittet beskrivs de steg som du kan vidta för att koda ditt innehåll med Media Encoder Standard.

1. Välj ditt Azure Media Services-konto i [Azure-portalen](https://portal.azure.com/).
2. I fönstret **Inställningar** väljer du **Tillgångar**.  
3. I fönstret **Tillgångar** väljer du den tillgång som du vill koda.
4. Tryck på knappen **Koda**.
5. I fönstret **Koda en tillgång** väljer du processorn ”Media Encoder Standard” och en förinställning. Information om förinställningar finns i [auto-generate a bitrate ladder](media-services-autogen-bitrate-ladder-with-mes.md) (autogenerera en bithastighetsstege) och [Task Presets for MES](media-services-mes-presets-overview.md) (Uppgiftsförinställningar för MES). Om du planerar att styra vilka kodningsförinställningar, kom ihåg att det är viktigt att välja den förinställning som är mest lämplig för din indatavideo. Om du till exempel vet att din indatavideo har en upplösning på 1 920 x 1 080 bildpunkter, kan du använda förinställningen ”H264 multibithastighet 1080p”. Om du har en video med låg upplösning (640 x 360) bör du inte använda förinställningen ”H264 multibithastighet 1080p”.
   
   Du kan redigera namnet på utdatatillgången och namnet på jobbet för enklare hantering.
   
   ![Koda tillgångar](./media/media-services-portal-vod-get-started/media-services-encode1.png)
6. Tryck på **Skapa**.

## <a name="next-step"></a>Nästa steg
Du kan övervaka kodningsjobb med Azure-portalen, enligt beskrivningen i [detta](media-services-portal-check-job-progress.md) artikel.  

## <a name="media-services-learning-paths"></a>Sökvägar för Media Services-utbildning
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Ge feedback
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

