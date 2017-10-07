---
title: "aaaHow tooCreate ett media-processor med hello Azure Media Services SDK för .NET | Microsoft Docs"
description: "Lär dig hur toocreate media processor komponenten tooencode konvertera format, kryptera eller dekryptera medieinnehåll för Azure Media Services. Kodexemplen är skrivna i C# och använder hello Media Services SDK för .NET."
services: media-services
documentationcenter: 
author: juliako
manager: cfowler
editor: 
ms.assetid: dbf9496f-c6f0-42a7-aa36-70f89dcb8ea2
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/31/2017
ms.author: juliako
ms.openlocfilehash: f133565cc1321d366013f17302adc8bc7585b251
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="how-to-get-a-media-processor-instance"></a>Så här: hämta en Media-processor
> [!div class="op_single_selector"]
> * [.NET](media-services-get-media-processor.md)
> * [REST](media-services-rest-get-media-processor.md)
> 
> 

## <a name="overview"></a>Översikt
Formatera konvertering krypterar eller dekrypterar medieinnehåll i Media Services en medieprocessor är en komponent som hanterar en specifik bearbetning aktivitet, till exempel kodning. Du vanligtvis skapa en medieprocessor när du skapar en uppgift tooencode, kryptera eller konvertera hello-formatet för medieinnehåll.

## <a name="azure-media-processors"></a>Azure media-processorer 

följande ämne hello innehåller listor över media processorer:

* [Kodning media-processorer](scenarios-and-availability.md#encoding-media-processors)
* [Analytics-medieprocessorer](scenarios-and-availability.md#analytics-media-processors)

## <a name="get-media-processor"></a>Hämta Medieprocessor

Hej följa metoden visar hur tooget en media-processor. hello kodexemplet förutsätter vi hello användning av en modul-nivå variabel med namnet **_context** tooreference hello-serverkontext enligt beskrivningen i avsnittet hello [så här: ansluta programmässigt Services tooMedia](media-services-use-aad-auth-to-access-ams-api.md).

    private static IMediaProcessor GetLatestMediaProcessorByName(string mediaProcessorName)
    {
        var processor = _context.MediaProcessors.Where(p => p.Name == mediaProcessorName).
        ToList().OrderBy(p => new Version(p.Version)).LastOrDefault();

        if (processor == null)
        throw new ArgumentException(string.Format("Unknown media processor", mediaProcessorName));

        return processor;
    }


## <a name="media-services-learning-paths"></a>Sökvägar för Media Services-utbildning
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Ge feedback
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

## <a name="next-steps"></a>Nästa steg
Nu när du vet hur tooget en media processor gå toohello [hur tooEncode en tillgång](media-services-dotnet-encode-with-media-encoder-standard.md) avsnittet som visar hur hello toouse Media Encoder Standard tooencode en tillgång.

