---
title: "aaaAzure Händelsehubbar exempel | Microsoft Docs"
description: Azure Event Hubs-exempel
services: event-hubs
documentationcenter: na
author: sethmanheim
manager: timlt
editor: 
ms.assetid: 
ms.service: event-hubs
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/15/2017
ms.author: sethm
ms.openlocfilehash: f01f52e6c13f9e885999a6726143440bbc70446d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="event-hubs-samples"></a>Event Hubs-exempel 

hello Azure Event Hubs urval visar viktiga funktioner i [Azure Event Hubs](/azure/event-hubs/). Den här artikeln kategoriserar och beskriver hello-exempel som är tillgängliga med länkar tooeach.

När hello detta skrivs finns Händelsehubbar exempel i flera olika platser:

- [Kodexempel för MSDN-utvecklare](https://code.msdn.microsoft.com/site/search?query=event%20hubs&f%5B0%5D.Value=event%20hubs&f%5B0%5D.Type=SearchText&ac=5)
- [GitHub](https://github.com/Azure/azure-event-hubs/tree/master/samples)

Läs mer om olika versioner av hello .NET Framework [ramverk och mål](/dotnet/articles/standard/frameworks).

Flera exempel kommer att läggas till med tiden, så kom tillbaka ofta efter uppdateringar.

## <a name="net-standard"></a>Standard för .NET

hello följande exempel visar hur toosend och ta emot händelser med hjälp av hello [händelsehubbklient](https://github.com/Azure/azure-event-hubs-dotnet/blob/master/readme.md) för hello [.NET standardbibliotek](/dotnet/articles/standard/library).

### <a name="send-events"></a>Skicka händelser 

Hej [börjar skicka](https://github.com/Azure/azure-event-hubs/tree/master/samples/DotNet/Microsoft.Azure.EventHubs/SampleSender) exempel visas hur toowrite en .NET Core-konsolapp som skickar händelser tooan händelsehubb.

### <a name="receive-events"></a>Ta emot händelser 

Hej [börja ta emot med hello värd för händelsebearbetning](https://github.com/Azure/azure-event-hubs/tree/master/samples/DotNet/Microsoft.Azure.EventHubs/SampleEphReceiver) prov är ett .NET Core-konsolprogram som tar emot meddelanden från en händelsehubb med hjälp av hello värd för händelsebearbetning.

## <a name="net-framework"></a>.NET framework   

De här exemplen visar olika funktioner i Azure Event Hubs, riktad hello [biblioteket för .NET Framework](/dotnet/framework/index).
 
### <a name="notify-users-of-events-received"></a>Meddela användare om händelser som tagits emot

Hej [AppToNotifyUsers](https://github.com/Azure-Samples/event-hubs-dotnet-user-notifications) exempel meddelar användare av data från sensorer och andra system.

### <a name="get-started-with-event-hubs"></a>Kom igång med händelsehubbar 

Hej [Event Hubs komma igång](https://code.msdn.microsoft.com/Service-Bus-Event-Hub-286fd097) exemplet visar hello grundläggande funktionerna i Händelsehubbar, till exempel hur toocreate en händelsehubb, skicka händelser tooan händelsehubb och använda händelser med hjälp av hello [värd för händelsebearbetning](https://www.nuget.org/packages/Microsoft.Azure.ServiceBus.EventProcessorHost/).

### <a name="scale-out-event-processing"></a>Skala ut händelsebearbetning 

Hej [skala ut händelsebearbetning](https://code.msdn.microsoft.com/Service-Bus-Event-Hub-45f43fc3) exemplet visar hur toouse hello [värd för händelsebearbetning](https://www.nuget.org/packages/Microsoft.Azure.ServiceBus.EventProcessorHost/) toodistribute hello arbetsbelastning av Händelsehubbar dataströmmen förbrukning. Den visar hur tooimplement hello **EventProcessor** och **EventProcessorFactory** objekt toomanage hello händelseströmmen. 

###  <a name="pull-data-from-sql-into-an-event-hub"></a>Hämtar data från SQL till en händelsehubb

Hej [dra SQL-data](https://github.com/Azure-Samples/event-hubs-dotnet-import-from-sql) exempel visas hur toopull data från en SQL tabell och push-installera den tooan händelsehubb, toouse indata i underordnade analytiska program.

### <a name="pull-web-data-into-an-event-hub"></a>Hämta webbdata till en händelsehubb 

Hej [importera data från hello web](https://github.com/Azure-Samples/event-hubs-dotnet-importfromweb) exempel visas hur toopull data från offentliga feeds (till exempel hello avdelning av transports trafikinformation feed) och skicka tooan händelsehubb.

## <a name="next-steps"></a>Nästa steg

Läs mer om .NET Framework-versioner genom att besöka hello följande länkar:

- [Ramverk och mål](/dotnet/articles/standard/frameworks)
- [.NET framework 4.6 och 4.5](/dotnet/framework/index)

Mer information om Händelsehubbar finns i följande artiklar hello:

- [Event Hubs-översikt](event-hubs-what-is-event-hubs.md)
- [Skapa en Event Hub](event-hubs-create.md)
- [Vanliga frågor och svar om Event Hubs](event-hubs-faq.md)