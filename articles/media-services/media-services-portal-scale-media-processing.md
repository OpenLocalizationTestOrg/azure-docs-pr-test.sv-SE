---
title: "Skala media bearbetning med hjälp av Azure portal | Microsoft Docs"
description: "Den här självstudiekursen vägleder dig genom stegen för skalning media bearbetning med Azure-portalen."
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
ms.openlocfilehash: d2312803a4471e207d3696ca8350a86e3c4761e6
ms.sourcegitcommit: b5c6197f997aa6858f420302d375896360dd7ceb
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 12/21/2017
---
# <a name="change-the-reserved-unit-type"></a>Ändra den reserverade enhetstypen
> [!div class="op_single_selector"]
> * [.NET](media-services-dotnet-encoding-units.md)
> * [Portal](media-services-portal-scale-media-processing.md)
> * [REST](https://docs.microsoft.com/rest/api/media/operations/encodingreservedunittype)
> * [Java](https://github.com/southworkscom/azure-sdk-for-media-services-java-samples)
> * [PHP](https://github.com/Azure/azure-sdk-for-php/tree/master/examples/MediaServices)
> 
> 

> [!NOTE]
> Om du vill hämta den senaste versionen av Java SDK och börja utveckla med Java Se [komma igång med Java-klient-SDK för Media Services](https://docs.microsoft.com/azure/media-services/media-services-java-how-to-use). <br/>
> Om du vill hämta den senaste PHP SDK för Media Services, leta efter version 0.5.7 av Microsoft/WindowAzure paketet i den [Packagist databasen](https://packagist.org/packages/microsoft/windowsazure#v0.5.7).  

## <a name="overview"></a>Översikt

Ett Media Services-konto är kopplat till en typ av reserverad enhet som bestämmer hur snabbt mediebearbetningsuppgifter ska bearbetas. Du kan välja mellan följande typer av reserverade enheter: **S1**, **S2** och **S3**. Samma kodningsjobb körs till exempel snabbare om du använder typen **S2** än om du använder typen **S1**.

Förutom att ange typ av reserverad enhet kan du etablera **reserverade enheter** (RU:er) för ditt konto. Antalet etablerade RU:er anger antalet medieuppgifter som kan bearbetas samtidigt i en viss konto.

>[!NOTE]
>RU:er fungerar för parallellisera all bearbetning av media, inklusive indexeringsjobb med hjälp av Azure Media Indexer. Men till skillnad från kodning bearbetas inte indexeringsjobb snabbare med snabbare reserverade enheter.

> [!IMPORTANT]
> Se till att granska den [översikt](media-services-scale-media-processing-overview.md) avsnittet om du vill ha mer information om skalning media bearbetning av avsnittet.
> 
> 

## <a name="scale-media-processing"></a>Skala mediebearbetning
Om du vill ändra vilken enhet och antalet reserverade enheter måste du göra följande:

1. Välj ditt Azure Media Services-konto i [Azure-portalen](https://portal.azure.com/).
2. I den **inställningar** väljer **mediereserverade enheter**.
   
    Du kan ändra antalet reserverade enheter för den valda reserverade enhetstypen den **Media hanteras enheter** skjutreglaget.
   
    Så här ändrar du den **RESERVERADE ENHETSTYP**, tryck på S1, S2 och S3.
   
    ![Sidan processorer](./media/media-services-portal-scale-media-processing/media-services-scale-media-processing.png)
3. Tryck på knappen SPARA för att spara ändringarna.
   
    Nya reserverade enheter tilldelas när du trycker på Spara.

## <a name="next-steps"></a>Nästa steg
Granska sökvägarna för Media Services-utbildning.

[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Ge feedback
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

