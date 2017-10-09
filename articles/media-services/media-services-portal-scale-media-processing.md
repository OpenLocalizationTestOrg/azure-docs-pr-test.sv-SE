---
title: aaaScale media bearbetning med hello Azure-portalen | Microsoft Docs
description: "Den här självstudiekursen vägleder dig genom stegen för hello skalning Media bearbetning med hello Azure-portalen."
services: media-services
documentationcenter: 
author: Juliako
manager: cfowler
editor: 
ms.assetid: e500f733-68aa-450c-b212-cf717c0d15da
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/04/2017
ms.author: juliako
ms.openlocfilehash: 89240c6f7579b8795e7b47f2b1c398b1d5477e20
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="change-hello-reserved-unit-type"></a>Ändra hello reserverade enhetstyp
> [!div class="op_single_selector"]
> * [.NET](media-services-dotnet-encoding-units.md)
> * [Portal](media-services-portal-scale-media-processing.md)
> * [REST](https://docs.microsoft.com/rest/api/media/operations/encodingreservedunittype)
> * [Java](https://github.com/southworkscom/azure-sdk-for-media-services-java-samples)
> * [PHP](https://github.com/Azure/azure-sdk-for-php/tree/master/examples/MediaServices)
> 
> 

## <a name="overview"></a>Översikt

Ett Media Services-konto är kopplat till ett reserverat enhetstyp som bestämmer hello hastighet som media bearbeta uppgifter bearbetas. Du kan välja mellan följande hello reserverade enhetstyper: **S1**, **S2**, eller **S3**. Till exempel hello samma kodningsjobbet körs snabbare när du använder hello **S2** reserverade enhetstyp jämföra toohello **S1** typen.

Dessutom toospecifying hello reserverade enhetstyp, kan du ange tooprovision till ditt konto med **reserverade enheter** (RUs). hello antalet etablerade RUs bestämmer hello antal media uppgifter som kan bearbetas samtidigt i en viss konto.

>[!NOTE]
>RU:er fungerar för parallellisera all bearbetning av media, inklusive indexeringsjobb med hjälp av Azure Media Indexer. Men till skillnad från kodning bearbetas inte indexeringsjobb snabbare med snabbare reserverade enheter.

> [!IMPORTANT]
> Se till att tooreview hello [översikt](media-services-scale-media-processing-overview.md) avsnittet tooget mer information om att skala media bearbetning av avsnittet.
> 
> 

## <a name="scale-media-processing"></a>Skala mediebearbetning
toochange hello reserverad enhet typ och hello antalet reserverade enheter hello följande:

1. I hello [Azure-portalen](https://portal.azure.com/), Välj Azure Media Services-konto.
2. I hello **inställningar** väljer **mediereserverade enheter**.
   
    toochange hello antalet reserverade enheter för hello valt typ av enhet använder hello **Media hanteras enheter** skjutreglaget.
   
    toochange hello **RESERVERADE ENHETSTYP**, tryck på S1, S2 och S3.
   
    ![Sidan processorer](./media/media-services-portal-scale-media-processing/media-services-scale-media-processing.png)
3. Tryck på hello spara knappen toosave ändringarna.
   
    hello nya reserverade enheter tilldelas när du trycker på Spara.

## <a name="next-steps"></a>Nästa steg
Granska sökvägarna för Media Services-utbildning.

[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Ge feedback
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

