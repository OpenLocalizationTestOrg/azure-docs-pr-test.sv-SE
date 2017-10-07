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
# <a name="event-hubs-net-standard-api-overview"></a><span data-ttu-id="66bd0-103">Översikt över Event Hubs .NET Standard-API</span><span class="sxs-lookup"><span data-stu-id="66bd0-103">Event Hubs .NET Standard API overview</span></span>
<span data-ttu-id="66bd0-104">Den här artikeln sammanfattas några av hello nyckeln Event Hubs .NET Standard klientens API: er.</span><span class="sxs-lookup"><span data-stu-id="66bd0-104">This article summarizes some of hello key Event Hubs .NET Standard client APIs.</span></span> <span data-ttu-id="66bd0-105">Det finns två klientbibliotek för .NET Standard:</span><span class="sxs-lookup"><span data-stu-id="66bd0-105">There are currently two .NET Standard client libraries:</span></span>
* [<span data-ttu-id="66bd0-106">Microsoft.Azure.EventHubs</span><span class="sxs-lookup"><span data-stu-id="66bd0-106">Microsoft.Azure.EventHubs</span></span>](/dotnet/api/microsoft.azure.eventhubs)
  *  <span data-ttu-id="66bd0-107">Det här biblioteket innehåller alla basic runtime-åtgärder.</span><span class="sxs-lookup"><span data-stu-id="66bd0-107">This library provides all basic runtime operations.</span></span>
* [<span data-ttu-id="66bd0-108">Microsoft.Azure.EventHubs.Processor</span><span class="sxs-lookup"><span data-stu-id="66bd0-108">Microsoft.Azure.EventHubs.Processor</span></span>](/dotnet/api/microsoft.azure.eventhubs.processor)
  * <span data-ttu-id="66bd0-109">Det här biblioteket lägger till ytterligare funktioner som gör det möjligt att hålla reda på bearbetade händelser och är hello enklaste sättet tooread från en händelsehubb.</span><span class="sxs-lookup"><span data-stu-id="66bd0-109">This library adds additional functionality that allows for keeping track of processed events, and is hello easiest way tooread from an event hub.</span></span>

## <a name="event-hubs-client"></a><span data-ttu-id="66bd0-110">Event Hubs klienten</span><span class="sxs-lookup"><span data-stu-id="66bd0-110">Event Hubs client</span></span>
<span data-ttu-id="66bd0-111">[EventHubClient](/dotnet/api/microsoft.azure.eventhubs.eventhubclient) är hello primära objekt som du använder toosend händelser, skapa mottagare och tooget körningsinformation.</span><span class="sxs-lookup"><span data-stu-id="66bd0-111">[EventHubClient](/dotnet/api/microsoft.azure.eventhubs.eventhubclient) is hello primary object you use toosend events, create receivers, and tooget run-time information.</span></span> <span data-ttu-id="66bd0-112">Den här klienten är länkade tooa viss händelsehubb, och skapar en ny anslutning toohello Händelsehubbar slutpunkt.</span><span class="sxs-lookup"><span data-stu-id="66bd0-112">This client is linked tooa particular event hub, and creates a new connection toohello Event Hubs endpoint.</span></span>

### <a name="create-an-event-hubs-client"></a><span data-ttu-id="66bd0-113">Skapa en händelsehubbklient</span><span class="sxs-lookup"><span data-stu-id="66bd0-113">Create an Event Hubs client</span></span>
<span data-ttu-id="66bd0-114">En [EventHubClient](/dotnet/api/microsoft.azure.eventhubs.eventhubclient) objektet har skapats från en anslutningssträng.</span><span class="sxs-lookup"><span data-stu-id="66bd0-114">An [EventHubClient](/dotnet/api/microsoft.azure.eventhubs.eventhubclient) object is created from a connection string.</span></span> <span data-ttu-id="66bd0-115">hello enklaste sättet tooinstantiate en ny klient visas i följande exempel hello:</span><span class="sxs-lookup"><span data-stu-id="66bd0-115">hello simplest way tooinstantiate a new client is shown in hello following example:</span></span>

```csharp
var eventHubClient = EventHubClient.CreateFromConnectionString("{Event Hubs connection string}");
```

<span data-ttu-id="66bd0-116">tooprogrammatically redigera hello anslutningssträngen kan du använda hello [EventHubsConnectionStringBuilder](/dotnet/api/microsoft.azure.eventhubs.eventhubsconnectionstringbuilder) klassen och skicka hello anslutningssträngen som en parameter för[EventHubClient.CreateFromConnectionString ](/dotnet/api/microsoft.azure.eventhubs.eventhubclient#Microsoft_Azure_EventHubs_EventHubClient_CreateFromConnectionString_System_String_).</span><span class="sxs-lookup"><span data-stu-id="66bd0-116">tooprogrammatically edit hello connection string, you can use hello [EventHubsConnectionStringBuilder](/dotnet/api/microsoft.azure.eventhubs.eventhubsconnectionstringbuilder) class, and pass hello connection string as a parameter too[EventHubClient.CreateFromConnectionString](/dotnet/api/microsoft.azure.eventhubs.eventhubclient#Microsoft_Azure_EventHubs_EventHubClient_CreateFromConnectionString_System_String_).</span></span>

```csharp
var connectionStringBuilder = new EventHubsConnectionStringBuilder("{Event Hubs connection string}")
{
    EntityPath = EhEntityPath
};

var eventHubClient = EventHubClient.CreateFromConnectionString(connectionStringBuilder.ToString());
```

### <a name="send-events"></a><span data-ttu-id="66bd0-117">Skicka händelser</span><span class="sxs-lookup"><span data-stu-id="66bd0-117">Send events</span></span>
<span data-ttu-id="66bd0-118">händelsehubb för toosend händelser tooan, Använd hello [EventData](/dotnet/api/microsoft.azure.eventhubs.eventdata) klass.</span><span class="sxs-lookup"><span data-stu-id="66bd0-118">toosend events tooan event hub, use hello [EventData](/dotnet/api/microsoft.azure.eventhubs.eventdata) class.</span></span> <span data-ttu-id="66bd0-119">hello innehållet måste vara en `byte` matris, eller en `byte` matris segment.</span><span class="sxs-lookup"><span data-stu-id="66bd0-119">hello body must be a `byte` array, or a `byte` array segment.</span></span>

```csharp
// Create a new EventData object by encoding a string as a byte array
var data = new EventData(Encoding.UTF8.GetBytes("This is my message..."));
// Set user properties if needed
data.Properties.Add("Type", "Informational");
// Send single message async
await eventHubClient.SendAsync(data);
```

### <a name="receive-events"></a><span data-ttu-id="66bd0-120">Ta emot händelser</span><span class="sxs-lookup"><span data-stu-id="66bd0-120">Receive events</span></span>
<span data-ttu-id="66bd0-121">hello rekommenderat sätt tooreceive händelser från Event Hubs använder hello [värd för händelsebearbetning](#event-processor-host-apis), som ger funktioner tooautomatically hålla reda på förskjutningen och partitions-information.</span><span class="sxs-lookup"><span data-stu-id="66bd0-121">hello recommended way tooreceive events from Event Hubs is using hello [Event Processor Host](#event-processor-host-apis), which provides functionality tooautomatically keep track of offset, and partition information.</span></span> <span data-ttu-id="66bd0-122">Det finns dock vissa situationer där du kanske vill toouse hello flexibilitet hello core Händelsehubbar tooreceive händelser.</span><span class="sxs-lookup"><span data-stu-id="66bd0-122">However, there are certain situations in which you may want toouse hello flexibility of hello core Event Hubs library tooreceive events.</span></span>

#### <a name="create-a-receiver"></a><span data-ttu-id="66bd0-123">Skapa en mottagare</span><span class="sxs-lookup"><span data-stu-id="66bd0-123">Create a receiver</span></span>
<span data-ttu-id="66bd0-124">Mottagare är bundet toospecific partitioner, så i ordning tooreceive alla händelser i en händelsehubb, behöver du toocreate flera instanser.</span><span class="sxs-lookup"><span data-stu-id="66bd0-124">Receivers are tied toospecific partitions, so in order tooreceive all events in an event hub, you will need toocreate multiple instances.</span></span> <span data-ttu-id="66bd0-125">Generellt sett är det en bra idé tooget hello partitionsinformation genom att programmera, i stället för att hårdkoda hello partitions-ID: n.</span><span class="sxs-lookup"><span data-stu-id="66bd0-125">Generally speaking, it is a good practice tooget hello partition information programatically, rather than hard-coding hello partition ids.</span></span> <span data-ttu-id="66bd0-126">I ordning toodo så kan du använda hello [GetRuntimeInformationAsync](/dotnet/api/microsoft.azure.eventhubs.eventhubclient#Microsoft_Azure_EventHubs_EventHubClient_GetRuntimeInformationAsync) metod.</span><span class="sxs-lookup"><span data-stu-id="66bd0-126">In order toodo so, you can use hello [GetRuntimeInformationAsync](/dotnet/api/microsoft.azure.eventhubs.eventhubclient#Microsoft_Azure_EventHubs_EventHubClient_GetRuntimeInformationAsync) method.</span></span>

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

<span data-ttu-id="66bd0-127">Eftersom händelser tas aldrig bort från en händelsehubb, och bara gälla, måste toospecify hello rätt startpunkt.</span><span class="sxs-lookup"><span data-stu-id="66bd0-127">Since events are never removed from an event hub (and only expire), you need toospecify hello proper starting point.</span></span> <span data-ttu-id="66bd0-128">hello visar följande exempel möjliga kombinationer.</span><span class="sxs-lookup"><span data-stu-id="66bd0-128">hello following example shows possible combinations.</span></span>

```csharp
// partitionId is assumed toocome from GetRuntimeInformationAsync()

// Using hello constant PartitionReceiver.EndOfStream only receives all messages from this point forward.
var receiver = eventHubClient.CreateReceiver(PartitionReceiver.DefaultConsumerGroupName, partitionId, PartitionReceiver.EndOfStream);

// All messages available
var receiver = eventHubClient.CreateReceiver(PartitionReceiver.DefaultConsumerGroupName, partitionId, "-1");

// From one day ago
var receiver = eventHubClient.CreateReceiver(PartitionReceiver.DefaultConsumerGroupName, partitionId, DateTime.Now.AddDays(-1));
```

#### <a name="consume-an-event"></a><span data-ttu-id="66bd0-129">Använda en händelse</span><span class="sxs-lookup"><span data-stu-id="66bd0-129">Consume an event</span></span>
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

## <a name="event-processor-host-apis"></a><span data-ttu-id="66bd0-130">Händelsen Processor värden API: er</span><span class="sxs-lookup"><span data-stu-id="66bd0-130">Event Processor Host APIs</span></span>
<span data-ttu-id="66bd0-131">Dessa API: er ger återhämtning tooworker processer som kan bli otillgängliga, genom att distribuera partitioner över tillgängliga arbetare.</span><span class="sxs-lookup"><span data-stu-id="66bd0-131">These APIs provide resiliency tooworker processes that may become unavailable, by distributing partitions across available workers.</span></span>

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

<span data-ttu-id="66bd0-132">hello följande är ett exempel på implementering av hello [IEventProcessor](/dotnet/api/microsoft.azure.eventhubs.processor.ieventprocessor).</span><span class="sxs-lookup"><span data-stu-id="66bd0-132">hello following is a sample implementation of hello [IEventProcessor](/dotnet/api/microsoft.azure.eventhubs.processor.ieventprocessor).</span></span>

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

## <a name="next-steps"></a><span data-ttu-id="66bd0-133">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="66bd0-133">Next steps</span></span>
<span data-ttu-id="66bd0-134">toolearn mer information om scenarier i Händelsehubbar finns i följande länkar:</span><span class="sxs-lookup"><span data-stu-id="66bd0-134">toolearn more about Event Hubs scenarios, visit these links:</span></span>

* [<span data-ttu-id="66bd0-135">Vad är Händelsehubbar i Azure?</span><span class="sxs-lookup"><span data-stu-id="66bd0-135">What is Azure Event Hubs?</span></span>](event-hubs-what-is-event-hubs.md)
* [<span data-ttu-id="66bd0-136">Tillgängliga Event Hubs API: er</span><span class="sxs-lookup"><span data-stu-id="66bd0-136">Available Event Hubs apis</span></span>](event-hubs-api-overview.md)

<span data-ttu-id="66bd0-137">hello .NET API-referenserna är här:</span><span class="sxs-lookup"><span data-stu-id="66bd0-137">hello .NET API references are here:</span></span>

* [<span data-ttu-id="66bd0-138">Microsoft.Azure.EventHubs</span><span class="sxs-lookup"><span data-stu-id="66bd0-138">Microsoft.Azure.EventHubs</span></span>](/dotnet/api/microsoft.azure.eventhubs)
* [<span data-ttu-id="66bd0-139">Microsoft.Azure.EventHubs.Processor</span><span class="sxs-lookup"><span data-stu-id="66bd0-139">Microsoft.Azure.EventHubs.Processor</span></span>](/dotnet/api/microsoft.azure.eventhubs.processor)