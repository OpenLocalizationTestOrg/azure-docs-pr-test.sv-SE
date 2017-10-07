---
title: "aaaAzure händelse API: er översikt | Microsoft Docs"
description: "Översikt över tillgängliga Azure Event Hubs API: er"
services: event-hubs
documentationcenter: na
author: sethmanheim
manager: timlt
editor: 
ms.assetid: 3f221a0c-182d-4e39-9f3d-3a3c16c5c6ed
ms.service: event-hubs
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/15/2017
ms.author: sethm
ms.openlocfilehash: 46dfcc544ff92642cfd7a967f9ec38a0d8e2bd5d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="available-event-hubs-apis"></a>Tillgängliga Event Hubs API: er

## <a name="runtime-apis"></a>Runtime-API: er

hello följer en beskrivning av alla tillgängliga Azure Event Hubs runtime-klienter. Några av dessa bibliotek är också begränsad hanteringsfunktioner, men det finns också [vissa bibliotek](#management-apis) dedikerade toomanagement åtgärder. hello core fokus för dessa bibliotek är toosend och ta emot meddelanden från en händelsehubb.

Se [ytterligare information](#additional-information) för mer information om hello aktuell status för varje runtime-bibliotek.

| Språk-plattformen | Klientpaketet | Paketet EventProcessorHost | Databasen |
| --- | --- | --- | --- |
| Standard för .NET | [NuGet](https://www.nuget.org/packages/Microsoft.Azure.EventHubs/) | [NuGet](https://www.nuget.org/packages/Microsoft.Azure.EventHubs.Processor/) | [GitHub](https://github.com/azure/azure-event-hubs-dotnet) |
| .NET framework | [NuGet](https://www.nuget.org/packages/WindowsAzure.ServiceBus/) | [NuGet](https://www.nuget.org/packages/Microsoft.Azure.ServiceBus.EventProcessorHost/) | Saknas |
| Java | [Maven 3.](https://search.maven.org/#search%7Cga%7C1%7Ca%3A%22azure-eventhubs%22) | [Maven 3.](https://search.maven.org/#search%7Cga%7C1%7Ca%3A%22azure-eventhubs-eph%22) | [GitHub](https://github.com/Azure/azure-event-hubs-java) |
| Node | [NPM](https://www.npmjs.com/package/azure-event-hubs) | Saknas | [GitHub](https://github.com/Azure/azure-event-hubs-node) |
| C | Saknas | Saknas | [GitHub](https://github.com/Azure/azure-event-hubs-c) |

### <a name="additional-information"></a>Ytterligare information

#### <a name="net"></a>.NET
hello .NET ekosystemet har flera körningar, så det finns flera .NET-bibliotek för Händelsehubbar. hello .NET standardbibliotek kan köras med .NET Core eller hello .NET Framework, medan hello-biblioteket för .NET Framework kan endast köras i en miljö med .NET Framework. Läs mer på .NET Frameworks [framework-versioner](https://docs.microsoft.com/dotnet/articles/standard/frameworks#framework-versions).

#### <a name="node"></a>Node

Hej Node.js-bibliotek är för närvarande under förhandsgranskning och hanteras som ett sida-projekt av Microsofts anställda och externa deltagare. Alla bidrag inklusive källkoden Välkommen och kommer att granskas.

## <a name="management-apis"></a>Management-API: er

hello nedan följer en lista över alla tillgängliga management specifika bibliotek. Ingen av dessa bibliotek innehåller runtime åtgärder och gäller hello uteslutande för hantering av Händelsehubbar entiteter.

| Språk-plattformen | Hanteringspaket | Databasen |
| --- | --- | --- | --- |
| Standard för .NET | [NuGet](https://www.nuget.org/packages/Microsoft.Azure.Management.EventHub) | [GitHub](https://github.com/Azure/azure-sdk-for-net/tree/AutoRest/src/ResourceManagement/EventHub) |

## <a name="next-steps"></a>Nästa steg
Mer information om Händelsehubbar genom att besöka hello följande länkar:

* [Event Hubs-översikt](event-hubs-what-is-event-hubs.md)
* [Skapa en Event Hub](event-hubs-create.md)
* [Vanliga frågor och svar om Event Hubs](event-hubs-faq.md)