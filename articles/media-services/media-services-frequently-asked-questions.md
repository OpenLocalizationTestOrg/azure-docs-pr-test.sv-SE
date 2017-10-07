---
title: "aaaAzure Media Services vanliga frågor och svar | Microsoft Docs"
description: "Vanliga frågor och svar (FAQ)"
services: media-services
documentationcenter: 
author: Juliako
manager: cfowler
editor: 
ms.assetid: 5374f7f4-c189-43ef-8b7f-f2f4141e2748
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/31/2017
ms.author: juliako
ms.openlocfilehash: 6d48a5c1291f3c2559d8445921d571718d0a0a6d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="frequently-asked-questions"></a>Vanliga frågor och svar

Den här artikeln tar vanliga frågor och svar som skapats av hello Azure Media Services (AMS)-användargruppen.

## <a name="general-ams-faqs"></a>Allmänna AMS vanliga frågor och svar
F: hur du skala indexering?

S: hello reserverade enheter är hello samma för kodning och indexering uppgifter. Följ instruktionerna på [hur tooScale Encoding-reserverade enheter](media-services-scale-media-processing-overview.md). **Obs** indexeraren prestanda inte påverkas av reserverade enhetstyp.

F: Jag har överförts, kodat och publicerade en video. Vad skulle vara hello orsak hello video agerar inte när jag försöker toostream den?

S: ett av vanligaste orsakerna är att du inte har hello strömningsslutpunkt från vilken du försöker tooplayback i hello hello **kör** tillstånd.  

F: kan jag göra sammansättning på en direktsänd dataström?

S: sammansättning på live dataströmmar för närvarande inte finns i Azure Media Services måste du toopre-utgöra på datorn.

F: kan jag använda Azure CDN med Liveströmning?

S: Media Services stöder integration med Azure CDN (Mer information finns i [hur tooManage Strömningsslutpunkter i ett Media Services-konto](media-services-portal-manage-streaming-endpoints.md)).  Du kan använda direktsänd strömning med CDN. Azure Media Services tillhandahåller Smooth Streaming, HLS och MPEG-DASH-utdata. Alla dessa format använder HTTP för att överföra data och få fördelarna med HTTP-cachelagring. Direktsänd strömning faktiska ljud och data är indelat toofragments och den här enskilda fragment hämta cachelagras i CDN. Endast data måste toobe uppdateras är hello manifestet data. CDN uppdaterar regelbundet manifestet data.

F: kan Azure Media services stöder lagra avbildningar?

S: Om du bara behöver toostore JPEG eller PNG-bilder, bör du behålla dem i Azure Blob Storage. Det finns ingen fördel tooputting dem i Media Services-konto om du inte vill tookeep dem som är associerade med din Video eller ljud tillgångar. Eller om du kan ha en behovet toouse hello bilder som överlägg i hello-videokodare. Media Encoder Standard stöder överlägg bilder ovanpå videor och som är vad listas JPEG och PNG som stöds indata format. Mer information finns i [skapar överlägg](media-services-advanced-encoding-with-mes.md#overlay).

F: hur kan kopiera resurser från en tooanother för Media Services-konto.

S: toocopy tillgångar från en tooanother för Media Services-konto med hjälp av .NET, Använd [IAsset.Copy](https://github.com/Azure/azure-sdk-for-media-services-extensions/blob/dev/MediaServices.Client.Extensions/IAssetExtensions.cs#L354) tilläggsmetoden som är tillgängliga i hello [Azure Media Services .NET SDK-tilläggen](https://github.com/Azure/azure-sdk-for-media-services-extensions/) databasen. Mer information finns i [detta](https://social.msdn.microsoft.com/Forums/azure/28912d5d-6733-41c1-b27d-5d5dff2695ca/migrate-media-services-across-subscription?forum=MediaServices) forum tråd.

F: vad hello stöds tecken för namngivning av filer när du arbetar med AMS?

S: Media Services använder hello värdet hello IAssetFile.Name egenskapen när du skapar URL: er för hello direktuppspelning av innehåll (till exempel http://{AMSAccount}.origin.mediaservices.windows.net/{GUID}/{IAssetFile.Name}/streamingParameters.) Därför tillåts procent-encoding inte. Hej värdet för hello **namn** egenskapen får inte ha någon av följande hello [procent-encoding-reserverade tecken](http://en.wikipedia.org/wiki/Percent-encoding#Percent-encoding_reserved_characters):! *' ();: @& = + $, /? [] # % ”. Dessutom det kan bara finnas ett '.' för hello filnamnstillägg.

F: hur tooconnect med hjälp av REST?

S: för information om hur tooconnect toohello AMS API, se [åtkomst hello Azure Media Services API med Azure AD authentication](media-services-use-aad-auth-to-access-ams-api.md). När du har anslutit toohttps://media.windows.net, får du en 301 omdirigering att ange en annan Media Services-URI. Du måste göra följande anrop toohello ny URI. 

F: hur kan rotera en video under hello kodning processen.

S: hello [Media Encoder Standard](media-services-dotnet-encode-with-media-encoder-standard.md) stöder rotation av vinklar 270-90/180. hello standardbeteendet är ”automatisk”, där den försöker toodetect hello rotation metadata i hello inkommande MP4/MOV-fil och kompensera för den. Inkludera hello följande **källor** elementet tooone hello json förinställningar som definierats [här](media-services-mes-presets-overview.md):

    "Version": 1.0,
    "Sources": [
    {
      "Streams": [],
      "Filters": {
        "Rotation": "90"
      }
    }
    ],
    "Codecs": [

    ...


## <a name="media-services-learning-paths"></a>Sökvägar för Media Services-utbildning
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Ge feedback
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]
