---
title: "aaa hur tooget en Medieprocessor instans med hjälp av REST | Microsoft Docs"
description: "Lär dig hur toocreate media processor komponenten tooencode konvertera format, kryptera eller dekryptera medieinnehåll för Azure Media Services."
services: media-services
documentationcenter: 
author: Juliako
manager: cfowler
editor: 
ms.assetid: f9ff1997-0da6-4528-aaed-792837e5be41
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/10/2017
ms.author: juliako
ms.openlocfilehash: 9f423648ab73c90405c64895ce0f5b6a457862e4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="how-tooget-a-media-processor-instance"></a>Hur tooget en Medieprocessor-instans
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

>[!NOTE]
>Vid åtkomst till entiteter i Media Services måste du ange specifika namn på huvudfält och värden i HTTP-begäranden. Mer information finns i [installationsprogrammet för Media Services REST API-utveckling](media-services-rest-how-to-use.md).

## <a name="connect-toomedia-services"></a>Ansluta tooMedia tjänster

Mer information om hur tooconnect toohello AMS API, se [åtkomst hello Azure Media Services API med Azure AD authentication](media-services-use-aad-auth-to-access-ams-api.md). 

>[!NOTE]
>När du har anslutit toohttps://media.windows.net, får du en 301 omdirigering att ange en annan Media Services-URI. Du måste göra följande anrop toohello ny URI.

## <a name="get-a-media-processor"></a>Hämta en medieprocessor

hello följande REST-anrop som visar hur tooget en medieprocessor instansen efter namn (i det här fallet **Media Encoder Standard**). 

Begäran:

    GET https://media.windows.net/api/MediaProcessors()?$filter=Name%20eq%20'Media%20Encoder%20Standard' HTTP/1.1
    DataServiceVersion: 1.0;NetFx
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json
    Accept-Charset: UTF-8
    User-Agent: Microsoft ADO.NET Data Services
    Authorization: Bearer <token>
    x-ms-version: 2.11
    Host: media.windows.net

Svar:

    . . .

    {  
       "odata.metadata":"https://media.windows.net/api/$metadata#MediaProcessors",
       "value":[  
          {  
             "Id":"nb:mpid:UUID:ff4df607-d419-42f0-bc17-a481b1331e56",
             "Description":"Media Encoder Standard",
             "Name":"Media Encoder Standard",
             "Sku":"",
             "Vendor":"Microsoft",
             "Version":"1.1"
          }
       ]
    }


## <a name="media-services-learning-paths"></a>Sökvägar för Media Services-utbildning
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Ge feedback
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

## <a name="next-steps"></a>Nästa steg
Nu när du vet hur tooget en media processor gå toohello [hur tooEncode en tillgång](media-services-rest-get-started.md) avsnittet som visar hur hello toouse Media Encoder Standard tooencode en tillgång.

