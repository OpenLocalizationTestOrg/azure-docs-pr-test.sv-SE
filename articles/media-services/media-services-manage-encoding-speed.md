---
title: "aaa hantera hastighet och samtidighet på din encoding med Azure Media Services | Microsoft Docs"
description: "Den här artikeln ger en kort översikt över hur du kan hantera hastighet och samtidighet kodning jobb/aktiviteter med Azure Media Services."
services: media-services
documentationcenter: 
author: juliako
manager: cfowler
editor: 
ms.assetid: 676313f8-a158-4e3a-a99b-2c29a341ecc9
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/10/2017
ms.author: juliako
ms.openlocfilehash: da52a6278a3d3b084dbf5a594db37df8447bb944
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
#  <a name="manage-speed-and-concurrency-of-your-encoding"></a>Hantera hastighet och samtidighet på din kodning

Den här artikeln ger en kort översikt över hur du kan hantera hastighet och samtidighet kodning jobb/aktiviteter.

## <a name="overview"></a>Översikt

I Media Services en **reserverade enhetstyp** anger hello hastighet som media bearbeta uppgifter bearbetas. Du kan välja mellan följande hello reserverade enhetstyper: **S1**, **S2**, eller **S3**. Till exempel hello samma kodningsjobbet körs snabbare när du använder hello **S2** reserverade enhetstyp jämföra toohello **S1** typen. Hej [skalning kodning enheter](media-services-scale-media-processing-overview.md) avsnittet visas en tabell som hjälper dig att fatta beslut om att välja mellan olika kodning hastigheter.

Dessutom toospecifying hello reserverade enhetstyp, kan du ange tooprovision till ditt konto med **reserverade enheter**. hello antalet etablerade reserverade enheter avgör hello antal media uppgifter som kan bearbetas samtidigt i en viss konto. Till exempel om ditt konto har fem reserverade enheter fem media aktiviteter körs samtidigt så länge som det finns aktiviteter toobe bearbetas. hello återstående aktiviteter kommer att vänta i kön hello och ska hämta tas upp för bearbetning i tur och ordning när en aktivitet är klar. Om ett konto inte har några reserverade enheter som har etablerats sedan hanteras uppgifter sekventiellt. I det här fallet hello väntetid mellan en aktivitet slutförs och hello nästa startar beror på hello tillgängligheten för resurser i hello system.

Mer detaljerad information och exempel som visar hur tooscale kodning enheter, se [detta](media-services-scale-media-processing-overview.md) avsnittet.

## <a name="next-step"></a>Nästa steg

[Kodning skalningsenheter](media-services-scale-media-processing-overview.md)

## <a name="media-services-learning-paths"></a>Sökvägar för Media Services-utbildning
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Ge feedback
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

