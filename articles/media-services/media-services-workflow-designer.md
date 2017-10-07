---
title: "aaaCreate avancerade kodning arbetsflöden med Arbetsflödesdesignern | Microsoft Docs"
description: "Läs mer om hur toocreate avancerade kodning arbetsflöden med Arbetsflödesdesignern."
services: media-services
documentationcenter: 
author: anilmur
manager: cfowler
editor: 
ms.assetid: 004815f2-0761-4706-87a1-675ba36e0322
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/19/2017
ms.author: juliako;johndeu;anilmur
ms.openlocfilehash: 3744cde54c78bec7c7b586962ec1a8fe9529c1d2
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-advanced-encoding-workflows-with-workflow-designer"></a>Skapa avancerade arbetsflöden för kodning med Workflow Designer
## <a name="overview"></a>Översikt
Hej **Arbetsflödesdesignern** är ett stationära Windows-verktyg som är används toodesign och skapa anpassade arbetsflöden för kodning med **Media Encoder Premium arbetsflöde**.
Genom att använda hello power hello workflow designer-verktyget kan du utforma och skapa komplexa arbetsflöden som körs i **Media Encoder Premium**.  

Arbetsflöden kan omfatta kunden beslut logik och förgrening utifrån hello indatakälla filens egenskaper. Du kan skapa arbetsflöden med åsidosättningsbar egenskaper och dynamiska värden toomake även hello mest komplexa kodning uppgifter enkelt toorepeat och anpassa i hello molnet.

Exempel arbetsflöden som du kan skapa inkluderar:

* Beslutet arbetsflöden inspektera hello källinnehållet för matchning och koda endast hello önskad utdata spår.  Detta är helfpul genom att eliminera hello gått förlorat spår som genereras av upscaling hello källa innehåll har råkat.
* Flera indatafiler kan vara används toosupport titlar, överlägg och fästa tillsammans innehåll. 

Det här verktyget kan också vara används toomodify någon av våra [publicerade arbetsflöden](media-services-workflow-designer.md#existing_workflows). 

> [!NOTE]
> Kontakta i tooget ditt exemplar av hello Workflow Designer verktyget mepd@microsoft.com.
> 
> 

När en arbetsflödesfil har skapats kan överföras som en tillgång och sedan kan användas för kodning mediefiler. Mer information om hur tooencode med **Media Encoder Premium arbetsflöde** med **.NET**, se [avancerade encoding med Media Encoder Premium arbetsflöde](media-services-encode-with-premium-workflow.md).

## <a id="existing_workflows"></a>Ändra befintliga arbetsflöden
Hej standard [publicerade arbetsflöden](media-services-workflow-designer.md#existing_workflows) kan ändras med hello designer verktyg. Du kan hämta hello standard Arbetsflödesfiler [här](https://github.com/Azure/azure-media-services-samples/tree/master/Encoding%20Presets/VoD/MediaEncoderPremiumWorkfows). hello mappen innehåller också hello beskrivning av dessa filer.

hello följande videoklipp visar hur toouse hello designer.

### <a name="day-1--getting-started"></a>Dag 1 – komma igång
Dag 1 video omfattar:

* Designer översikt
* Grundläggande arbetsflöden – ”Hello World”
* Skapa flera utdata MP4-filer för användning med Azure Media Services-direktuppspelning

> [!VIDEO https://channel9.msdn.com/Blogs/Azure/Azure-Premium-Encoder-Workflow-Designer-Training-Videos-Day-1/player]
> 
> 

### <a name="day-2"></a>Dag 2
Dag 2 video omfattar:

* Olika scenarier för filhantering i källan – hantering av ljud
* Arbetsflöden med avancerad logik
* Diagrammet faser

> [!VIDEO https://channel9.msdn.com/Blogs/Azure/Azure-Premium-Encoder-Workflow-Designer-Training-Videos-Day-2/player]
> 
> 

### <a name="day-3"></a>Dag 3
Dag 3 video omfattar:

* Skript inuti arbetsflöden/ritningarna
* Begränsningar med hello aktuella kodaren
* FRÅGOR OCH SVAR

> [!VIDEO https://channel9.msdn.com/Blogs/Azure/Azure-Premium-Encoder-Workflow-Designer-Training-Videos-Day-3/player]
> 
> 

## <a name="next-step"></a>Nästa steg
Granska sökvägarna för Media Services-utbildning.

[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Ge feedback
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

Om du behöver stöd för eller har frågor om hur du skapar anpassade arbetsflöden i hello Workflow designer verktyget kan du skicka e-post toomepd@microsoft.com.

## <a name="see-also"></a>Se även
[Azure Premium-kodare arbetsflödet Designer utbildning videor](http://johndeutscher.com/2015/07/06/azure-premium-encoder-workflow-designer-training-videos/)

