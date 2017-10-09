---
title: "aaaAzure Relay API översikt | Microsoft Docs"
description: "Översikt över tillgängliga Azure Relay API: er"
services: event-hubs
documentationcenter: na
author: sethmanheim
manager: timlt
editor: 
ms.assetid: fdaa1d2b-bd80-4e75-abb9-0c3d0773af2d
ms.service: service-bus-relay
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/03/2017
ms.author: sethm
ms.openlocfilehash: 3c4d737d5fee9a8babce094fa6dfddb28910834b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="available-relay-apis"></a>Tillgängliga Relay-API: er

## <a name="runtime-apis"></a>Runtime-API: er

hello visas följande tabell alla tillgängliga Relay runtime-klienter.

Hej [ytterligare information](#additional-information) avsnitt innehåller mer information om hello status för varje runtime-bibliotek.

| Språk-plattformen | Tillgänglig funktion | Klientpaketet | Databasen |
| --- | --- | --- | --- |
| Standard för .NET | Hybridanslutningar | [Microsoft.Azure.Relay](https://www.nuget.org/packages/Microsoft.Azure.Relay/) | [GitHub](https://github.com/azure/azure-relay-dotnet) |
| .NET framework | WCF-relä | [WindowsAzure.ServiceBus](https://www.nuget.org/packages/WindowsAzure.ServiceBus/) | Saknas |
| Node | Hybridanslutningar | [`hyco-ws`](https://www.npmjs.com/package/hyco-ws)<br/>[`hyco-websocket`](https://www.npmjs.com/package/hyco-websocket) | [GitHub](https://github.com/Azure/azure-relay-node) |

### <a name="additional-information"></a>Ytterligare information

#### <a name="net"></a>.NET
hello .NET ekosystemet har flera körningar, så det finns flera .NET-bibliotek för Händelsehubbar. hello .NET standardbibliotek kan köras med .NET Core eller hello .NET Framework, medan hello-biblioteket för .NET Framework kan endast köras i en miljö med .NET Framework. Läs mer på .NET Frameworks [framework-versioner](/dotnet/articles/standard/frameworks#framework-versions).

## <a name="next-steps"></a>Nästa steg
toolearn mer om Azure Relay finns i följande länkar:
* [Vad är Azure Relay?](relay-what-is-it.md)
* [Vanliga frågor och svar om Relay](relay-faq.md)