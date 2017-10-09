---
title: "aaaRetry logiken i hello Media Services SDK för .NET | Microsoft Docs"
description: "hello avsnittet ger en översikt över logik i hello Media Services SDK för .NET."
author: Juliako
manager: cfowler
editor: 
services: media-services
documentationcenter: 
ms.assetid: 527b61a6-c862-4bd8-bcbc-b9aea1ffdee3
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/21/2017
ms.author: juliako
ms.openlocfilehash: 18d0a9d68e55a48bc769fb6ae5711ddba78ed8e6
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="retry-logic-in-hello-media-services-sdk-for-net"></a>Försök logiken i hello Media Services SDK för .NET
När du arbetar med Microsoft Azure-tjänster, kan tillfälliga problem uppstå. Om ett tillfälligt fel uppstår i de flesta fall efter några återförsök slutförs hello åtgärden. hello Media Services SDK för .NET implementerar hello försök logik toohandle tillfälliga problem som är associerade med undantag och fel som orsakas av webbegäranden, köra frågor, spara ändringarna och lagringsåtgärder.  Som standard kör hello Media Services SDK för .NET fyra återförsök innan utlösande hello undantag tooyour programmet igen. hello kod i ditt program måste sedan hantera undantaget korrekt.  

 hello nedan följer en kort riktlinje webbegäran, lagring, fråga och SaveChanges principer:  

* hello Storage principen används för blob storage-åtgärder (överföring eller hämtning av tillgångsinformation filer).  
* hello webbegäran principen används för allmänna webbegäranden (till exempel för att få en autentisering token och lösa hello användare kluster endpoint).  
* hello fråga princip används för att fråga entiteter från VILOLÄGE (till exempel mediaContext.Assets.Where(...)).  
* hello SaveChanges princip används för att göra något som ändrar data i hello-tjänsten (till exempel skapa en entitet som uppdaterar en entitet som anropar en tjänst för en åtgärd).  
  
  Det här avsnittet beskriver typer av undantag och felkoder som hanteras av hello Media Services SDK för .NET försök logik.  

## <a name="exception-types"></a>Typer av undantag
hello följande tabell beskrivs undantag som hello Media Services SDK för .NET-referenser eller hanteras inte för vissa åtgärder som kan orsaka tillfälliga problem.  

| Undantag | Webbegäran | Lagring | Fråga | SaveChanges |
| --- | --- | --- | --- | --- |
| WebException<br/>Mer information finns i hello [WebException statuskoder](media-services-retry-logic-in-dotnet-sdk.md#WebExceptionStatus) avsnitt. |Ja |Ja |Ja |Ja |
| DataServiceClientException<br/> Mer information finns i [statuskoder för HTTP-fel](media-services-retry-logic-in-dotnet-sdk.md#HTTPStatusCode). |Nej |Ja |Ja |Ja |
| DataServiceQueryException<br/> Mer information finns i [statuskoder för HTTP-fel](media-services-retry-logic-in-dotnet-sdk.md#HTTPStatusCode). |Nej |Ja |Ja |Ja |
| DataServiceRequestException<br/> Mer information finns i [statuskoder för HTTP-fel](media-services-retry-logic-in-dotnet-sdk.md#HTTPStatusCode). |Nej |Ja |Ja |Ja |
| DataServiceTransportException |Nej |Nej |Ja |Ja |
| TimeoutException |Ja |Ja |Ja |Nej |
| SocketException |Ja |Ja |Ja |Ja |
| StorageException |Nej |Ja |Nej |Nej |
| IOException |Nej |Ja |Nej |Nej |

### <a name="WebExceptionStatus"></a>Statuskoder för WebException
hello följande tabell visar vilket fel som WebException koder hello logik tillämpas. Hej [WebExceptionStatus](http://msdn.microsoft.com/library/system.net.webexceptionstatus.aspx) uppräkning definierar hello-statuskoder.  

| Status | Webbegäran | Lagring | Fråga | SaveChanges |
| --- | --- | --- | --- | --- |
| ConnectFailure |Ja |Ja |Ja |Ja |
| NameResolutionFailure |Ja |Ja |Ja |Ja |
| ProxyNameResolutionFailure |Ja |Ja |Ja |Ja |
| SendFailure |Ja |Ja |Ja |Ja |
| PipelineFailure |Ja |Ja |Ja |Nej |
| ConnectionClosed |Ja |Ja |Ja |Nej |
| KeepAliveFailure |Ja |Ja |Ja |Nej |
| UnknownError |Ja |Ja |Ja |Nej |
| ReceiveFailure |Ja |Ja |Ja |Nej |
| RequestCanceled |Ja |Ja |Ja |Nej |
| Timeout |Ja |Ja |Ja |Nej |
| Protokollfel <br/>hello nytt försök protokollfel styrs av hello HTTP-status kod hantering. Mer information finns i [statuskoder för HTTP-fel](media-services-retry-logic-in-dotnet-sdk.md#HTTPStatusCode). |Ja |Ja |Ja |Ja |

### <a name="HTTPStatusCode"></a>Statuskoder för HTTP-fel
När frågan och SaveChanges kasta DataServiceClientException, DataServiceQueryException eller DataServiceQueryException, returneras hello HTTP-status felkod hello StatusCode egenskapen.  hello visas nedan vilka felkoder hello logik tillämpas.  

| Status | Webbegäran | Lagring | Fråga | SaveChanges |
| --- | --- | --- | --- | --- |
| 401 |Nej |Ja |Nej |Nej |
| 403 |Nej |Ja<br/>Hanterar återförsök med längre väntar. |Nej |Nej |
| 408 |Ja |Ja |Ja |Ja |
| 429 |Ja |Ja |Ja |Ja |
| 500 |Ja |Ja |Ja |Nej |
| 502 |Ja |Ja |Ja |Nej |
| 503 |Ja |Ja |Ja |Ja |
| 504 |Ja |Ja |Ja |Nej |

Om du vill tootake en titt på hello faktiska implementeringen av hello Media Services SDK för .NET logik finns [azure-sdk-för-media-services](https://github.com/Azure/azure-sdk-for-media-services/tree/dev/src/net/Client/TransientFaultHandling).

## <a name="next-steps"></a>Nästa steg
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Ge feedback
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

