---
title: "felkoder för aaaAzure Media Services | Microsoft Docs"
description: "hello avsnittet ger en översikt över Azure Media Services-felkoder."
author: Juliako
manager: cfowler
editor: 
services: media-services
documentationcenter: 
ms.assetid: d3a62a64-7608-4b17-8667-479b26ba0d6c
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/31/2017
ms.author: juliako
ms.openlocfilehash: de1ffd6dee8901a3051eb5032536c3669482d6b3
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-media-services-error-codes"></a>Azure Media Services-felkoder
När du använder Microsoft Azure Media Services kan du få HTTP felkoder från hello tjänsten beroende på problem som till exempel autentiseringstoken ut tooactions som inte stöds i Media Services. hello följer en lista över **HTTP felkoder** som kan returneras av Media Services och hello möjliga orsakerna till dem.  

## <a name="400-bad-request"></a>400 Felaktig förfrågan
hello-begäran innehåller ogiltig information och nekas på grund av tooone av hello följande orsaker:

* En API-version som inte stöds har angetts. Senaste version hello finns [installationsprogrammet för Media Services REST API-utveckling](media-services-rest-how-to-use.md).
* hello API-version av Media Services har inte angetts. Mer information om hur toospecify hello API-versionen finns [Media Services Operations REST API-referens](https://docs.microsoft.com/rest/api/media/operations/azure-media-services-rest-api-reference).
  
  > [!NOTE]
  > Om du använder hello .NET eller Java SDK tooconnect tooMedia tjänster, hello API-version har angetts för dig när du försöker och utföra vissa åtgärder mot Media Services.
  > 
  > 
* En odefinierad egenskap har angetts. hello egenskapsnamnet är i hello felmeddelande. Endast de egenskaper som är medlemmar i en given entitet kan anges. Se [Azure Media Services REST API-referens](https://docs.microsoft.com/rest/api/media/operations/azure-media-services-rest-api-reference) för en lista över enheter och deras egenskaper.
* Ett ogiltigt egenskapsvärde har angetts. hello egenskapsnamnet är i hello felmeddelande. Se hello föregående länk för giltig egenskapstyper och deras värden.
* Ett värde för egenskapen saknas och måste anges.
* En del av hello angivna URL: en innehåller ett felaktigt värde.
* Ett försök har gjorts tooupdate en WriteOnce-egenskap.
* Ett försök har gjorts toocreate ett jobb som har en inkommande tillgång med en primär AssetFile som angavs inte eller kunde inte fastställas.
* Ett försök har gjorts tooupdate en SAS-lokaliserare. SAS-positionerare kan endast skapas eller tas bort. Strömning positionerare kan uppdateras. Mer information finns i [lokaliserare](https://docs.microsoft.com/rest/api/media/operations/locator).
* En åtgärd som inte stöds eller en fråga skickades.

## <a name="401-unauthorized"></a>401 obehörig
hello begäran kunde inte autentiseras (innan den kan godkännas) på grund av tooone av hello följande orsaker:

* Autentiseringshuvud saknas.
* Felaktig autentisering huvudvärde.
  * hello-token har upphört att gälla. 
  * hello token innehåller en ogiltig signatur.

## <a name="403-forbidden"></a>403 Nekad
hello-begäran är inte tillåtet på grund av tooone av hello följande orsaker:

* hello Media Services-konto kan inte hittas eller har tagits bort.
* hello Media Services-konto har inaktiverats och hello Begärandetypen är inte HTTP GET. Tjänståtgärder returneras ett 403 svar.
* hello autentiseringstoken innehåller inte information om hello användarens autentiseringsuppgifter: AccountName och/eller prenumerations-ID. Du hittar den här informationen i hello Media Services UI-tillägg för ditt Media Services-konto i hello Azure-hanteringsportalen.
* hello resursen kan inte nås.
  
  * Ett försök har gjorts toouse en MediaProcessor som inte är tillgängligt för Media Services-kontot.
  * Ett försök gjordes tooupdate en jobbmall definieras av Media Services.
  * Ett försök har gjorts toooverwrite vissa andra Media Services kontots lokaliserare.
  * Ett försök har gjorts toooverwrite vissa andra Media Services kontots ContentKey.
* hello resurs kunde inte skapas på grund av tooa service kvot som har uppnåtts för hello Media Services-konto. Mer information om hello service kvoter finns [kvoter och begränsningar](media-services-quotas-and-limitations.md).

## <a name="404-not-found"></a>404 Hittades inte
hello-begäran är inte tillåten på en resurs på grund av tooone av hello följande orsaker:

* Ett försök har gjorts tooupdate en entitet som inte finns.
* Ett försök har gjorts toodelete en entitet som inte finns.
* Ett försök har gjorts toocreate en entitet som länkar tooan entitet som inte finns.
* Ett försök har gjorts tooGET en entitet som inte finns.
* Ett försök har gjorts toospecify ett lagringskonto som inte är associerad med hello Media Services-konto.  

## <a name="409-conflict"></a>409 konflikt
hello-begäran är inte tillåtet på grund av tooone av hello följande orsaker:

* Mer än en AssetFile har hello angivet namn inom hello tillgången.
* Ett försök gjordes toocreate en andra primära AssetFile inom hello tillgången.
* Ett försök gjordes toocreate en ContentKey med hello angivna ID: T används redan.
* Ett försök gjordes toocreate en lokaliserare med hello angivna ID: T används redan.
* Mer än en IngestManifestFile har hello angivet namn inom hello IngestManifest.
* Ett försök har gjorts toolink en andra storage kryptering ContentKey toohello lagring krypteras tillgången.
* Ett försök gjordes toolink hello samma ContentKey toohello tillgången.
* Ett försök gjordes toocreate en positionerare tooan tillgången vars lagringsbehållaren saknas eller är inte längre som är associerade med hello tillgången.
* Ett försök har gjorts toocreate en positionerare tooan tillgång som redan har 5 lokaliserare används. (Azure Storage tillämpar hello gräns på fem principer för delad åtkomst på en lagringsbehållare.)
* Länka lagringskontot för en tillgång tooan IngestManifestAsset är inte hello samma som hello storage-konto används av hello överordnade IngestManifest.  

## <a name="500-internal-server-error"></a>500 Internt serverfel
Under hello bearbetningen av hello begäran påträffar Media Services ett fel som förhindrar hello bearbetningen fortsätter. Detta kan bero på grund av tooone av hello följande orsaker:

* Skapa en tillgång eller jobb misslyckas eftersom hello Media Services konto service-kvotinformation är otillgänglig.
* Skapa en tillgång eller IngestManifest blobblagringsbehållare misslyckas eftersom hello konto information om lagringskontot är inte tillgänglig för tillfället.
* Ett oväntat fel.

## <a name="503-service-unavailable"></a>503 inte tillgänglig
hello-servern är för närvarande inte tooreceive begäranden. Det här felet kan orsakas av hög begäranden toohello service. Media Services begränsning mekanism begränsar hello resursanvändningen för program som gör överdriven begäran toohello service.

> [!NOTE]
> Kontrollera hello fel meddelande och fel kod sträng tooget mer detaljerad information om hello orsak som du fick hello 503-fel. Det här felet innebär alltid inte begränsning.
> 
> 

Möjliga status är:

* ”Servern är upptagen. Tidigare körs av denna typ av begäran tog mer än {0} sekunder ”.
* ”Servern är upptagen. Mer än {0}-begäranden per sekund kan vara begränsas ”.
* ”Servern är upptagen. Fler än {0}-förfrågningar inom {1} sekunder kan begränsas ”.

toohandle felet, bör du använda exponentiell inte logik. Som innebär att progressivt längre väntar mellan två på varandra följande felsvar försök.  Mer information finns i [tillfälliga fel hanterar programmet Block](https://msdn.microsoft.com/library/hh680905.aspx).

> [!NOTE]
> Om du använder [Azure Media Services SDK för .net](https://github.com/Azure/azure-sdk-for-media-services/tree/master), hello logik för hello 503-fel har implementerats av hello SDK.  
> 
> 

## <a name="see-also"></a>Se även
[Media Services Management felkoder](http://msdn.microsoft.com/library/windowsazure/dn167016.aspx)

## <a name="next-steps"></a>Nästa steg
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Ge feedback
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

