---
title: 'aaaOverview av hello Azure Event Hubs .NET Standard API: erna | Microsoft Docs'
description: "Översikt över standard .NET-API"
services: event-hubs
documentationcenter: na
author: sethmanheim
manager: timlt
editor: 
ms.assetid: a173f8e4-556c-42b8-b856-838189f7e636
ms.service: event-hubs
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/15/2017
ms.author: sethm
ms.openlocfilehash: c97acecb35b69039e06ded7203c75fca41ce98f2
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="event-hubs-net-standard-api-overview"></a>Översikt över Event Hubs .NET Standard-API
Den här artikeln sammanfattas några av hello nyckeln Event Hubs .NET Standard klientens API: er. Det finns två klientbibliotek för .NET Standard:
* [Microsoft.Azure.EventHubs](/dotnet/api/microsoft.azure.eventhubs)
  *  Det här biblioteket innehåller alla basic runtime-åtgärder.
* [Microsoft.Azure.EventHubs.Processor](/dotnet/api/microsoft.azure.eventhubs.processor)
  * Det här biblioteket lägger till ytterligare funktioner som gör det möjligt att hålla reda på bearbetade händelser och är hello enklaste sättet tooread från en händelsehubb.

## <a name="event-hubs-client"></a>Event Hubs klienten
[EventHubClient](/dotnet/api/microsoft.azure.eventhubs.eventhubclient) är hello primära objekt som du använder toosend händelser, skapa mottagare och tooget körningsinformation. Den här klienten är länkade tooa viss händelsehubb, och skapar en ny anslutning toohello Händelsehubbar slutpunkt.

### <a name="create-an-event-hubs-client"></a>Skapa en händelsehubbklient
En [EventHubClient](/dotnet/api/microsoft.azure.eventhubs.eventhubclient) objektet har skapats från en anslutningssträng. hello enklaste sättet tooinstantiate en ny klient visas i följande exempel hello:

```csharp
var eventHubClient = EventHubClient.CreateFromConnectionString("{Event Hubs connection string}");
```

tooprogrammatically redigera hello anslutningssträngen kan du använda hello [EventHubsConnectionStringBuilder](/dotnet/api/microsoft.azure.eventhubs.eventhubsconnectionstringbuilder) klassen och skicka hello anslutningssträngen som en parameter för[EventHubClient.CreateFromConnectionString ](/dotnet/api/microsoft.azure.eventhubs.eventhubclient#Microsoft_Azure_EventHubs_EventHubClient_CreateFromConnectionString_System_String_).

```csharp
var connectionStringBuilder = new EventHubsConnectionStringBuilder("{Event Hubs connection string}")
{
    EntityPath = EhEntityPath
};

var eventHubClient = EventHubClient.CreateFromConnectionString(connectionStringBuilder.ToString());
```

### <a name="send-events"></a>Skicka händelser
händelsehubb för toosend händelser tooan, Använd hello [EventData](/dotnet/api/microsoft.azure.eventhubs.eventdata) klass. hello innehållet måste vara en `byte` matris, eller en `byte` matris segment.

```csharp
// Create a new EventData object by encoding a string as a byte array
var data = new EventData(Encoding.UTF8.GetBytes("This is my message..."));
// Set user properties if needed
data.Properties.Add("Type", "Informational");
// Send single message async
await eventHubClient.SendAsync(data);
```

### <a name="receive-events"></a>Ta emot händelser
hello rekommenderat sätt tooreceive händelser från Event Hubs använder hello [värd för händelsebearbetning](#event-processor-host-apis), som ger funktioner tooautomatically hålla reda på förskjutningen och partitions-information. Det finns dock vissa situationer där du kanske vill toouse hello flexibilitet hello core Händelsehubbar tooreceive händelser.

#### <a name="create-a-receiver"></a>Skapa en mottagare
Mottagare är bundet toospecific partitioner, så i ordning tooreceive alla händelser i en händelsehubb, behöver du toocreate flera instanser. Generellt sett är det en bra idé tooget hello partitionsinformation genom att programmera, i stället för att hårdkoda hello partitions-ID: n. I ordning toodo så kan du använda hello [GetRuntimeInformationAsync](/dotnet/api/microsoft.azure.eventhubs.eventhubclient#Microsoft_Azure_EventHubs_EventHubClient_GetRuntimeInformationAsync) metod.

```csharp
// Create a list tookeep track of hello receivers
var receivers = new List<PartitionReceiver>();
// Use hello eventHubClient created above tooget hello runtime information
var runTimeInformation = await eventHubClient.GetRuntimeInformationAsync();
// Loop over hello resulting partition ids
foreach (var partitionId in runTimeInformation.PartitionIds)
{
    // Create hello receiver
    var receiver = eventHubClient.CreateReceiver(PartitionReceiver.DefaultConsumerGroupName, partitionId, PartitionReceiver.EndOfStream);
    // Add hello receiver toohello list
    receivers.Add(receiver);
}
```

Eftersom händelser tas aldrig bort från en händelsehubb, och bara gälla, måste toospecify hello rätt startpunkt. hello visar följande exempel möjliga kombinationer.

```csharp
// partitionId is assumed toocome from GetRuntimeInformationAsync()

// Using hello constant PartitionReceiver.EndOfStream only receives all messages from this point forward.
var receiver = eventHubClient.CreateReceiver(PartitionReceiver.DefaultConsumerGroupName, partitionId, PartitionReceiver.EndOfStream);

// All messages available
var receiver = eventHubClient.CreateReceiver(PartitionReceiver.DefaultConsumerGroupName, partitionId, "-1");

// From one day ago
var receiver = eventHubClient.CreateReceiver(PartitionReceiver.DefaultConsumerGroupName, partitionId, DateTime.Now.AddDays(-1));
```

#### <a name="consume-an-event"></a>Använda en händelse
```csharp
// Receive a maximum of 100 messages in this call tooReceiveAsync
var ehEvents = await receiver.ReceiveAsync(100);
// ReceiveAsync can return null if there are no messages
if (ehEvents != null)
{
    // Since ReceiveAsync can return more than a single event you will need a loop tooprocess
    foreach (var ehEvent in ehEvents)
    {
        // Decode hello byte array segment
        var message = UnicodeEncoding.UTF8.GetString(ehEvent.Body.Array);
        // Load hello custom property that we set in hello send example
        var customType = ehEvent.Properties["Type"];
        // Implement processing logic here
    }
}       
```

## <a name="event-processor-host-apis"></a>Händelsen Processor värden API: er
Dessa API: er ger återhämtning tooworker processer som kan bli otillgängliga, genom att distribuera partitioner över tillgängliga arbetare.

```csharp
// Checkpointing is done within hello SimpleEventProcessor and on a per-consumerGroup per-partition basis, workers resume from where they last left off.

// Read these connection strings from a secure location
var ehConnectionString = "{Event Hubs connection string}";
var ehEntityPath = "{event hub path/name}";
var storageConnectionString = "{Storage connection string}";
var storageContainerName = "{Storage account container name}";

var eventProcessorHost = new EventProcessorHost(
    ehEntityPath,
    PartitionReceiver.DefaultConsumerGroupName,
    ehConnectionString,
    storageConnectionString,
    storageContainerName);

// Start/register an EventProcessorHost
await eventProcessorHost.RegisterEventProcessorAsync<SimpleEventProcessor>();

// Disposes of hello Event Processor Host
await eventProcessorHost.UnregisterEventProcessorAsync();
```

hello följande är ett exempel på implementering av hello [IEventProcessor](/dotnet/api/microsoft.azure.eventhubs.processor.ieventprocessor).

```csharp
public class SimpleEventProcessor : IEventProcessor
{
    public Task CloseAsync(PartitionContext context, CloseReason reason)
    {
        Console.WriteLine($"Processor Shutting Down. Partition '{context.PartitionId}', Reason: '{reason}'.");
        return Task.CompletedTask;
    }

    public Task OpenAsync(PartitionContext context)
    {
        Console.WriteLine($"SimpleEventProcessor initialized. Partition: '{context.PartitionId}'");
        return Task.CompletedTask;
    }

    public Task ProcessErrorAsync(PartitionContext context, Exception error)
    {
        Console.WriteLine($"Error on Partition: {context.PartitionId}, Error: {error.Message}");
        return Task.CompletedTask;
    }

    public Task ProcessEventsAsync(PartitionContext context, IEnumerable<EventData> messages)
    {
        foreach (var eventData in messages)
        {
            var data = Encoding.UTF8.GetString(eventData.Body.Array, eventData.Body.Offset, eventData.Body.Count);
            Console.WriteLine($"Message received. Partition: '{context.PartitionId}', Data: '{data}'");
        }

        return context.CheckpointAsync();
    }
}
```

## <a name="next-steps"></a>Nästa steg
toolearn mer information om scenarier i Händelsehubbar finns i följande länkar:

* [Vad är Händelsehubbar i Azure?](event-hubs-what-is-event-hubs.md)
* [Tillgängliga Event Hubs API: er](event-hubs-api-overview.md)

hello .NET API-referenserna är här:

* [Microsoft.Azure.EventHubs](/dotnet/api/microsoft.azure.eventhubs)
* [Microsoft.Azure.EventHubs.Processor](/dotnet/api/microsoft.azure.eventhubs.processor)