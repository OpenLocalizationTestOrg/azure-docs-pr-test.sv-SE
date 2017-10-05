---
title: "Översikt över Azure Relay API | Microsoft Docs"
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
ms.openlocfilehash: 8d93a0344adc3b0b7617f3a7d85124142d7a7555
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="available-relay-apis"></a>Tillgängliga Relay-API: er

## <a name="runtime-apis"></a>Runtime-API: er

I följande tabell visas alla tillgängliga Relay runtime-klienter.

Den [ytterligare information](#additional-information) avsnitt innehåller mer information om statusen för varje runtime-bibliotek.

| Språk-plattformen | Tillgänglig funktion | Klientpaketet | Databasen |
| --- | --- | --- | --- |
| Standard för .NET | Hybridanslutningar | [Microsoft.Azure.Relay](https://www.nuget.org/packages/Microsoft.Azure.Relay/) | [GitHub](https://github.com/azure/azure-relay-dotnet) |
| .NET framework | WCF-relä | [WindowsAzure.ServiceBus](https://www.nuget.org/packages/WindowsAzure.ServiceBus/) | Saknas |
| Node | Hybridanslutningar | [`hyco-ws`](https://www.npmjs.com/package/hyco-ws)<br/>[`hyco-websocket`](https://www.npmjs.com/package/hyco-websocket) | [GitHub](https://github.com/Azure/azure-relay-node) |

### <a name="additional-information"></a>Ytterligare information

#### <a name="net"></a>.NET
.NET-ekosystemet har flera körningar, så det finns flera .NET-bibliotek för Händelsehubbar. .NET standardbibliotek kan köras med .NET Core eller .NET Framework, medan biblioteket för .NET Framework kan endast köras i en miljö med .NET Framework. Läs mer på .NET Frameworks [framework-versioner](/dotnet/articles/standard/frameworks#framework-versions).

## <a name="next-steps"></a>Nästa steg
Mer information om Azure Relay finns i följande länkar:
* [Vad är Azure Relay?](relay-what-is-it.md)
* [Vanliga frågor och svar om Relay](relay-faq.md)