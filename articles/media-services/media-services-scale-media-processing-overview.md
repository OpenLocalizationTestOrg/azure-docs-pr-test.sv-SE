---
title: "aaaScaling Media bearbetning översikt | Microsoft Docs"
description: "Det här avsnittet är en översikt över skalning Media bearbetning med Azure Media Services."
services: media-services
documentationcenter: 
author: juliako
manager: cfowler
editor: 
ms.assetid: 780ef5c2-3bd6-4261-8540-6dee77041387
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/04/2017
ms.author: juliako
ms.openlocfilehash: d17531f79d4c1e0d0fa544c4fb5c083684e706fe
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="scaling-media-processing-overview"></a>Skalning bearbetning av Media: översikt
Den här sidan ger en översikt över hur och varför tooscale media bearbetning. 

## <a name="overview"></a>Översikt
Ett Media Services-konto är kopplat till ett reserverat enhetstyp som bestämmer hello hastighet som media bearbeta uppgifter bearbetas. Du kan välja mellan följande hello reserverade enhetstyper: **S1**, **S2**, eller **S3**. Till exempel hello samma kodningsjobbet körs snabbare när du använder hello **S2** reserverade enhetstyp jämföra toohello **S1** typen. Mer information finns i hello [reserverade enhetstyper](https://azure.microsoft.com/blog/high-speed-encoding-with-azure-media-services/).

Dessutom toospecifying hello reserverade enhetstyp, kan du ange tooprovision ditt konto med reserverade enheter. hello antalet etablerade reserverade enheter avgör hello antal media uppgifter som kan bearbetas samtidigt i en viss konto. Till exempel om ditt konto har fem reserverade enheter fem media aktiviteter körs samtidigt så länge som det finns aktiviteter toobe bearbetas. hello återstående aktiviteter kommer att vänta i kön hello och ska hämta tas upp för bearbetning i tur och ordning när en aktivitet är klar. Om ett konto inte har några reserverade enheter som har etablerats sedan hanteras uppgifter sekventiellt. I det här fallet hello väntetid mellan en aktivitet slutförs och hello nästa startar beror på hello tillgängligheten för resurser i hello system.

## <a name="choosing-between-different-reserved-unit-types"></a>Att välja mellan olika reserverade enhetstyper
hello i den följande tabellen hjälper dig att fatta beslut om att välja mellan olika kodning hastigheter. Den innehåller några benchmark fall och innehåller SAS-URL: er som du kan använda toodownload videor som du kan utföra egna test:

| Scenarier | **S1** | **S2** | **S3** |
| --- | --- | --- | --- |
| Avsedda användningsfall |Enkel bithastighet kodning. <br/>Filer på SD eller under lösningar, tid inte känsliga, låg kostnad. |Enkel bithastighet och flera bithastighet kodning.<br/>Normal användning för både SD och HD kodning. |Enkel bithastighet och flera bithastighet kodning.<br/>Videor för fullständig HD och 4K lösning. Tid som känsliga och snabbare tidsåtgången kodning. |
| Prestandamått |[Indatafilen: 5 minuter långa 640x360p på 29,97 bildrutor per sekund](https://wamspartners.blob.core.windows.net/for-long-term-share/Whistler_5min_360p30.mp4?sr=c&si=AzureDotComReadOnly&sig=OY0TZ%2BP2jLK7vmcQsCTAWl33GIVCu67I02pgarkCTNw%3D).<br/><br/>Kodning tooa enkel bithastighet MP4-fil vid hello samma upplösning tar ungefär 11 minuter. |[Indatafilen: 5 minuter långa 1280x720p på 29,97 bildrutor per sekund](https://wamspartners.blob.core.windows.net/for-long-term-share/Whistler_5min_720p30.mp4?sr=c&si=AzureDotComReadOnly&sig=OY0TZ%2BP2jLK7vmcQsCTAWl33GIVCu67I02pgarkCTNw%3D)<br/><br/>Kodningen med ”H264 enkel bithastighet 720p” förinställda tar cirka 5 minuter.<br/><br/>Kodningen med ”H264 Multibithastighet 720p” förinställda tar ungefär 11,5 minuter. |[Indatafilen: 5 minuter långa 1920x1080p på 29,97 bildrutor per sekund](https://wamspartners.blob.core.windows.net/for-long-term-share/Whistler_5min_1080p30.mp4?sr=c&si=AzureDotComReadOnly&sig=OY0TZ%2BP2jLK7vmcQsCTAWl33GIVCu67I02pgarkCTNw%3D). <br/><br/>Kodningen med ”H264 enkel bithastighet 1080p” förinställda tar cirka 2.7 minuter.<br/><br/>Kodningen med ”H264 Multibithastighet 1080p” förinställda tar ungefär 5.7 minuter. |

## <a name="considerations"></a>Överväganden
> [!IMPORTANT]
> Gå igenom överväganden som beskrivs i det här avsnittet.  
> 
> 

* Reserverade enheter fungerar för parallelizing all bearbetning av media, inklusive indexering jobb med hjälp av Azure Media Indexer.  Men till skillnad från kodning bearbetas inte indexeringsjobb snabbare med snabbare reserverade enheter.
* Om du använder hello delade poolen, som är utan några reserverade enheter och sedan koda aktiviteterna har hello samma prestanda som S1 RUs. Dock övre gräns toohello tid ingen aktiviteterna kan spendera i köade tillstånd och samtidigt, högst endast en aktivitet körs.
* hello följande Datacenter erbjuder hello **S2** reserverade enhetstyp: södra och västra Indien.
* hello följande Datacenter erbjuder inte hello **S3** reserverade enhetstyp: Indien, västra.

## <a name="billing"></a>Fakturering

Du debiteras efter faktiska använda minuter av mediereserverade enheter. En detaljerad förklaring finns hello vanliga frågor och svar i hello [Media Services priser](https://azure.microsoft.com/pricing/details/media-services/) sidan.   

## <a name="quotas-and-limitations"></a>Kvoter och begränsningar
Information om kvoter och begränsningar och hur tooopen ett supportärende Se [kvoter och begränsningar](media-services-quotas-and-limitations.md).

## <a name="next-step"></a>Nästa steg
Uppnå hello skalning media Bearbetningsuppgift med något av dessa tekniker: 

> [!div class="op_single_selector"]
> * [.NET](media-services-dotnet-encoding-units.md)
> * [Portal](media-services-portal-scale-media-processing.md)
> * [REST](https://docs.microsoft.com/rest/api/media/operations/encodingreservedunittype)
> * [Java](https://github.com/southworkscom/azure-sdk-for-media-services-java-samples)
> * [PHP](https://github.com/Azure/azure-sdk-for-php/tree/master/examples/MediaServices)
> 
> 

## <a name="media-services-learning-paths"></a>Sökvägar för Media Services-utbildning
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Ge feedback
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

