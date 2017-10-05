---
title: "Översikt över Azure Event Hubs .NET Standard API: er | Microsoft Docs"
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
ms.openlocfilehash: eea682c40cd415b383a8b2f0004a5f3648e2f01f
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/18/2017
---
# <a name="event-hubs-net-standard-api-overview"></a><span data-ttu-id="0f8e5-103">Översikt över Event Hubs .NET Standard-API</span><span class="sxs-lookup"><span data-stu-id="0f8e5-103">Event Hubs .NET Standard API overview</span></span>
<span data-ttu-id="0f8e5-104">Den här artikeln sammanfattas några av nyckeln Event Hubs .NET Standard klientens API: er.</span><span class="sxs-lookup"><span data-stu-id="0f8e5-104">This article summarizes some of the key Event Hubs .NET Standard client APIs.</span></span> <span data-ttu-id="0f8e5-105">Det finns två klientbibliotek för .NET Standard:</span><span class="sxs-lookup"><span data-stu-id="0f8e5-105">There are currently two .NET Standard client libraries:</span></span>
* [<span data-ttu-id="0f8e5-106">Microsoft.Azure.EventHubs</span><span class="sxs-lookup"><span data-stu-id="0f8e5-106">Microsoft.Azure.EventHubs</span></span>](/dotnet/api/microsoft.azure.eventhubs)
  *  <span data-ttu-id="0f8e5-107">Det här biblioteket innehåller alla basic runtime-åtgärder.</span><span class="sxs-lookup"><span data-stu-id="0f8e5-107">This library provides all basic runtime operations.</span></span>
* [<span data-ttu-id="0f8e5-108">Microsoft.Azure.EventHubs.Processor</span><span class="sxs-lookup"><span data-stu-id="0f8e5-108">Microsoft.Azure.EventHubs.Processor</span></span>](/dotnet/api/microsoft.azure.eventhubs.processor)
  * <span data-ttu-id="0f8e5-109">Det här biblioteket lägger till ytterligare funktioner som gör det möjligt att hålla reda på bearbetade händelser och är det enklaste sättet att läsa från en händelsehubb.</span><span class="sxs-lookup"><span data-stu-id="0f8e5-109">This library adds additional functionality that allows for keeping track of processed events, and is the easiest way to read from an event hub.</span></span>

## <a name="event-hubs-client"></a><span data-ttu-id="0f8e5-110">Event Hubs klienten</span><span class="sxs-lookup"><span data-stu-id="0f8e5-110">Event Hubs client</span></span>
<span data-ttu-id="0f8e5-111">[EventHubClient](/dotnet/api/microsoft.azure.eventhubs.eventhubclient) är det primära objekt som du använder för att skicka händelser, skapa mottagare och för att hämta information om körning.</span><span class="sxs-lookup"><span data-stu-id="0f8e5-111">[EventHubClient](/dotnet/api/microsoft.azure.eventhubs.eventhubclient) is the primary object you use to send events, create receivers, and to get run-time information.</span></span> <span data-ttu-id="0f8e5-112">Den här klienten är kopplad till en viss händelsehubb och skapar en ny anslutning till slutpunkten för Händelsehubbar.</span><span class="sxs-lookup"><span data-stu-id="0f8e5-112">This client is linked to a particular event hub, and creates a new connection to the Event Hubs endpoint.</span></span>

### <a name="create-an-event-hubs-client"></a><span data-ttu-id="0f8e5-113">Skapa en händelsehubbklient</span><span class="sxs-lookup"><span data-stu-id="0f8e5-113">Create an Event Hubs client</span></span>
<span data-ttu-id="0f8e5-114">En [EventHubClient](/dotnet/api/microsoft.azure.eventhubs.eventhubclient) objektet har skapats från en anslutningssträng.</span><span class="sxs-lookup"><span data-stu-id="0f8e5-114">An [EventHubClient](/dotnet/api/microsoft.azure.eventhubs.eventhubclient) object is created from a connection string.</span></span> <span data-ttu-id="0f8e5-115">Det enklaste sättet att skapa en instans av en ny klient visas i följande exempel:</span><span class="sxs-lookup"><span data-stu-id="0f8e5-115">The simplest way to instantiate a new client is shown in the following example:</span></span>

```csharp
var eventHubClient = EventHubClient.CreateFromConnectionString("{Event Hubs connection string}");
```

<span data-ttu-id="0f8e5-116">Om du vill redigera programmässigt anslutningssträngen, du kan använda den [EventHubsConnectionStringBuilder](/dotnet/api/microsoft.azure.eventhubs.eventhubsconnectionstringbuilder) klassen och ange anslutningssträngen som en parameter för [EventHubClient.CreateFromConnectionString](/dotnet/api/microsoft.azure.eventhubs.eventhubclient#Microsoft_Azure_EventHubs_EventHubClient_CreateFromConnectionString_System_String_).</span><span class="sxs-lookup"><span data-stu-id="0f8e5-116">To programmatically edit the connection string, you can use the [EventHubsConnectionStringBuilder](/dotnet/api/microsoft.azure.eventhubs.eventhubsconnectionstringbuilder) class, and pass the connection string as a parameter to [EventHubClient.CreateFromConnectionString](/dotnet/api/microsoft.azure.eventhubs.eventhubclient#Microsoft_Azure_EventHubs_EventHubClient_CreateFromConnectionString_System_String_).</span></span>

```csharp
var connectionStringBuilder = new EventHubsConnectionStringBuilder("{Event Hubs connection string}")
{
    EntityPath = EhEntityPath
};

var eventHubClient = EventHubClient.CreateFromConnectionString(connectionStringBuilder.ToString());
```

### <a name="send-events"></a><span data-ttu-id="0f8e5-117">Skicka händelser</span><span class="sxs-lookup"><span data-stu-id="0f8e5-117">Send events</span></span>
<span data-ttu-id="0f8e5-118">Om du vill skicka händelser till en händelsehubb, använder den [EventData](/dotnet/api/microsoft.azure.eventhubs.eventdata) klass.</span><span class="sxs-lookup"><span data-stu-id="0f8e5-118">To send events to an event hub, use the [EventData](/dotnet/api/microsoft.azure.eventhubs.eventdata) class.</span></span> <span data-ttu-id="0f8e5-119">Innehållet måste vara en `byte` matris, eller en `byte` matris segment.</span><span class="sxs-lookup"><span data-stu-id="0f8e5-119">The body must be a `byte` array, or a `byte` array segment.</span></span>

```csharp
// Create a new EventData object by encoding a string as a byte array
var data = new EventData(Encoding.UTF8.GetBytes("This is my message..."));
// Set user properties if needed
data.Properties.Add("Type", "Informational");
// Send single message async
await eventHubClient.SendAsync(data);
```

### <a name="receive-events"></a><span data-ttu-id="0f8e5-120">Ta emot händelser</span><span class="sxs-lookup"><span data-stu-id="0f8e5-120">Receive events</span></span>
<span data-ttu-id="0f8e5-121">Det rekommenderade sättet att ta emot händelser från Event Hubs använder den [värd för händelsebearbetning](#event-processor-host-apis), som ger funktioner för att automatiskt hålla reda på förskjutningen och partitionsinformation.</span><span class="sxs-lookup"><span data-stu-id="0f8e5-121">The recommended way to receive events from Event Hubs is using the [Event Processor Host](#event-processor-host-apis), which provides functionality to automatically keep track of offset, and partition information.</span></span> <span data-ttu-id="0f8e5-122">Det finns dock vissa situationer där kan du vill använda Händelsehubbar Kärnbibliotek flexibilitet för att ta emot händelser.</span><span class="sxs-lookup"><span data-stu-id="0f8e5-122">However, there are certain situations in which you may want to use the flexibility of the core Event Hubs library to receive events.</span></span>

#### <a name="create-a-receiver"></a><span data-ttu-id="0f8e5-123">Skapa en mottagare</span><span class="sxs-lookup"><span data-stu-id="0f8e5-123">Create a receiver</span></span>
<span data-ttu-id="0f8e5-124">Mottagare är knutna till specifika partitioner så för att kunna ta emot alla händelser i en händelsehubb, behöver du skapa flera instanser.</span><span class="sxs-lookup"><span data-stu-id="0f8e5-124">Receivers are tied to specific partitions, so in order to receive all events in an event hub, you will need to create multiple instances.</span></span> <span data-ttu-id="0f8e5-125">Generellt sett är det en bra idé att hämta partitionsinformation om genom att programmera, i stället för att hårdkoda partitions-ID: n.</span><span class="sxs-lookup"><span data-stu-id="0f8e5-125">Generally speaking, it is a good practice to get the partition information programatically, rather than hard-coding the partition ids.</span></span> <span data-ttu-id="0f8e5-126">Du kan använda för att göra det, den [GetRuntimeInformationAsync](/dotnet/api/microsoft.azure.eventhubs.eventhubclient#Microsoft_Azure_EventHubs_EventHubClient_GetRuntimeInformationAsync) metod.</span><span class="sxs-lookup"><span data-stu-id="0f8e5-126">In order to do so, you can use the [GetRuntimeInformationAsync](/dotnet/api/microsoft.azure.eventhubs.eventhubclient#Microsoft_Azure_EventHubs_EventHubClient_GetRuntimeInformationAsync) method.</span></span>

```csharp
// Create a list to keep track of the receivers
var receivers = new List<PartitionReceiver>();
// Use the eventHubClient created above to get the runtime information
var runTimeInformation = await eventHubClient.GetRuntimeInformationAsync();
// Loop over the resulting partition ids
foreach (var partitionId in runTimeInformation.PartitionIds)
{
    // Create the receiver
    var receiver = eventHubClient.CreateReceiver(PartitionReceiver.DefaultConsumerGroupName, partitionId, PartitionReceiver.EndOfStream);
    // Add the receiver to the list
    receivers.Add(receiver);
}
```

<span data-ttu-id="0f8e5-127">Du måste ange rätt startpunkten eftersom händelser tas aldrig bort från en händelsehubb (och bara gälla).</span><span class="sxs-lookup"><span data-stu-id="0f8e5-127">Since events are never removed from an event hub (and only expire), you need to specify the proper starting point.</span></span> <span data-ttu-id="0f8e5-128">I följande exempel visas möjliga kombinationer.</span><span class="sxs-lookup"><span data-stu-id="0f8e5-128">The following example shows possible combinations.</span></span>

```csharp
// partitionId is assumed to come from GetRuntimeInformationAsync()

// Using the constant PartitionReceiver.EndOfStream only receives all messages from this point forward.
var receiver = eventHubClient.CreateReceiver(PartitionReceiver.DefaultConsumerGroupName, partitionId, PartitionReceiver.EndOfStream);

// All messages available
var receiver = eventHubClient.CreateReceiver(PartitionReceiver.DefaultConsumerGroupName, partitionId, "-1");

// From one day ago
var receiver = eventHubClient.CreateReceiver(PartitionReceiver.DefaultConsumerGroupName, partitionId, DateTime.Now.AddDays(-1));
```

#### <a name="consume-an-event"></a><span data-ttu-id="0f8e5-129">Använda en händelse</span><span class="sxs-lookup"><span data-stu-id="0f8e5-129">Consume an event</span></span>
```csharp
// Receive a maximum of 100 messages in this call to ReceiveAsync
var ehEvents = await receiver.ReceiveAsync(100);
// ReceiveAsync can return null if there are no messages
if (ehEvents != null)
{
    // Since ReceiveAsync can return more than a single event you will need a loop to process
    foreach (var ehEvent in ehEvents)
    {
        // Decode the byte array segment
        var message = UnicodeEncoding.UTF8.GetString(ehEvent.Body.Array);
        // Load the custom property that we set in the send example
        var customType = ehEvent.Properties["Type"];
        // Implement processing logic here
    }
}       
```

## <a name="event-processor-host-apis"></a><span data-ttu-id="0f8e5-130">Händelsen Processor värden API: er</span><span class="sxs-lookup"><span data-stu-id="0f8e5-130">Event Processor Host APIs</span></span>
<span data-ttu-id="0f8e5-131">Dessa API: er ger återhämtning i arbetsprocesser som kan bli otillgängliga, genom att distribuera partitioner över tillgängliga arbetare.</span><span class="sxs-lookup"><span data-stu-id="0f8e5-131">These APIs provide resiliency to worker processes that may become unavailable, by distributing partitions across available workers.</span></span>

```csharp
// Checkpointing is done within the SimpleEventProcessor and on a per-consumerGroup per-partition basis, workers resume from where they last left off.

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

// Disposes of the Event Processor Host
await eventProcessorHost.UnregisterEventProcessorAsync();
```

<span data-ttu-id="0f8e5-132">Följande är ett exempel på implementering av den [IEventProcessor](/dotnet/api/microsoft.azure.eventhubs.processor.ieventprocessor).</span><span class="sxs-lookup"><span data-stu-id="0f8e5-132">The following is a sample implementation of the [IEventProcessor](/dotnet/api/microsoft.azure.eventhubs.processor.ieventprocessor).</span></span>

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

## <a name="next-steps"></a><span data-ttu-id="0f8e5-133">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="0f8e5-133">Next steps</span></span>
<span data-ttu-id="0f8e5-134">Mer information om scenarier i händelsehubbar finns i följande länkar:</span><span class="sxs-lookup"><span data-stu-id="0f8e5-134">To learn more about Event Hubs scenarios, visit these links:</span></span>

* [<span data-ttu-id="0f8e5-135">Vad är Händelsehubbar i Azure?</span><span class="sxs-lookup"><span data-stu-id="0f8e5-135">What is Azure Event Hubs?</span></span>](event-hubs-what-is-event-hubs.md)
* [<span data-ttu-id="0f8e5-136">Tillgängliga Event Hubs API: er</span><span class="sxs-lookup"><span data-stu-id="0f8e5-136">Available Event Hubs apis</span></span>](event-hubs-api-overview.md)

<span data-ttu-id="0f8e5-137">.NET API-referenserna är här:</span><span class="sxs-lookup"><span data-stu-id="0f8e5-137">The .NET API references are here:</span></span>

* [<span data-ttu-id="0f8e5-138">Microsoft.Azure.EventHubs</span><span class="sxs-lookup"><span data-stu-id="0f8e5-138">Microsoft.Azure.EventHubs</span></span>](/dotnet/api/microsoft.azure.eventhubs)
* [<span data-ttu-id="0f8e5-139">Microsoft.Azure.EventHubs.Processor</span><span class="sxs-lookup"><span data-stu-id="0f8e5-139">Microsoft.Azure.EventHubs.Processor</span></span>](/dotnet/api/microsoft.azure.eventhubs.processor)