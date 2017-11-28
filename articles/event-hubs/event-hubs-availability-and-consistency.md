---
title: "Tillgänglighet och konsekvens i Händelsehubbar i Azure | Microsoft Docs"
description: "Så här ger maximal mängd tillgänglighet och konsekvens med Händelsehubbar i Azure med hjälp av partitioner."
services: event-hubs
documentationcenter: na
author: sethmanheim
manager: timlt
editor: 
ms.assetid: 8f3637a1-bbd7-481e-be49-b3adf9510ba1
ms.service: event-hubs
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/15/2017
ms.author: sethm
ms.openlocfilehash: 681a9d1636d547492f6f827461c6b2494b918778
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/18/2017
---
# <a name="availability-and-consistency-in-event-hubs"></a><span data-ttu-id="bb8c1-103">Tillgänglighet och konsekvens i Händelsehubbar</span><span class="sxs-lookup"><span data-stu-id="bb8c1-103">Availability and consistency in Event Hubs</span></span>

## <a name="overview"></a><span data-ttu-id="bb8c1-104">Översikt</span><span class="sxs-lookup"><span data-stu-id="bb8c1-104">Overview</span></span>
<span data-ttu-id="bb8c1-105">Händelsehubbar i Azure använder en [partitionering modellen](event-hubs-features.md#partitions) för bättre tillgänglighet och parallellisering inom en enskild händelsehubb.</span><span class="sxs-lookup"><span data-stu-id="bb8c1-105">Azure Event Hubs uses a [partitioning model](event-hubs-features.md#partitions) to improve availability and parallelization within a single event hub.</span></span> <span data-ttu-id="bb8c1-106">Till exempel om en händelsehubb har fyra partitioner och en av dessa partitioner flyttas från en server till en annan i en åtgärd för belastningsutjämning, du fortfarande skicka och ta emot från tre partitioner.</span><span class="sxs-lookup"><span data-stu-id="bb8c1-106">For example, if an event hub has four partitions, and one of those partitions is moved from one server to another in a load balancing operation, you can still send and receive from three other partitions.</span></span> <span data-ttu-id="bb8c1-107">Dessutom kan med fler partitioner du behöva flera samtidiga läsare bearbetning av dina data, förbättra din sammanställda genomflöde.</span><span class="sxs-lookup"><span data-stu-id="bb8c1-107">Additionally, having more partitions enables you to have more concurrent readers processing your data, improving your aggregate throughput.</span></span> <span data-ttu-id="bb8c1-108">Förstå konsekvenserna av att partitionering och ordning i ett distribuerat system är en kritisk del av lösningen.</span><span class="sxs-lookup"><span data-stu-id="bb8c1-108">Understanding the implications of partitioning and ordering in a distributed system is a critical aspect of solution design.</span></span>

<span data-ttu-id="bb8c1-109">För att visa en kompromiss mellan ordning och tillgänglighet, finns det [CAP-sats](https://en.wikipedia.org/wiki/CAP_theorem), även kallad Brewers-sats.</span><span class="sxs-lookup"><span data-stu-id="bb8c1-109">To help explain the trade-off between ordering and availability, see the [CAP theorem](https://en.wikipedia.org/wiki/CAP_theorem), also known as Brewer's theorem.</span></span> <span data-ttu-id="bb8c1-110">Den här sats beskrivs valet mellan konsekvens, tillgänglighet och tolerans för partitionen.</span><span class="sxs-lookup"><span data-stu-id="bb8c1-110">This theorem discusses the choice between consistency, availability, and partition tolerance.</span></span>

<span data-ttu-id="bb8c1-111">Brewers sats definierar konsekvens och tillgänglighet på följande sätt:</span><span class="sxs-lookup"><span data-stu-id="bb8c1-111">Brewer's theorem defines consistency and availability as follows:</span></span>
* <span data-ttu-id="bb8c1-112">Partitionera tolerans: möjlighet för databearbetning systemet fortsätta databearbetning, även om en partition fel inträffar.</span><span class="sxs-lookup"><span data-stu-id="bb8c1-112">Partition tolerance: the ability of a data processing system to continue processing data even if a partition failure occurs.</span></span>
* <span data-ttu-id="bb8c1-113">Tillgänglighet: en icke-misslyckas nod returnerar ett rimligt svar inom en rimlig tidsperiod (med några fel eller timeout).</span><span class="sxs-lookup"><span data-stu-id="bb8c1-113">Availability: a non-failing node returns a reasonable response within a reasonable amount of time (with no errors or timeouts).</span></span>
* <span data-ttu-id="bb8c1-114">Konsekvenskontroll: Läs garanteras att returnera den senaste skrivningen för en viss klient.</span><span class="sxs-lookup"><span data-stu-id="bb8c1-114">Consistency: a read is guaranteed to return the most recent write for a given client.</span></span>

## <a name="partition-tolerance"></a><span data-ttu-id="bb8c1-115">Tolerans för partition</span><span class="sxs-lookup"><span data-stu-id="bb8c1-115">Partition tolerance</span></span>
<span data-ttu-id="bb8c1-116">Händelsehubbar är byggt på en partitionerad datamodell.</span><span class="sxs-lookup"><span data-stu-id="bb8c1-116">Event Hubs is built on top of a partitioned data model.</span></span> <span data-ttu-id="bb8c1-117">Du kan konfigurera antalet partitioner i din event hub under installationen, men du kan inte ändra det här värdet senare.</span><span class="sxs-lookup"><span data-stu-id="bb8c1-117">You can configure the number of partitions in your event hub during setup, but you cannot change this value later.</span></span> <span data-ttu-id="bb8c1-118">Eftersom du måste använda partitioner med Händelsehubbar, måste du fatta ett beslut om tillgänglighet och konsekvens för ditt program.</span><span class="sxs-lookup"><span data-stu-id="bb8c1-118">Since you must use partitions with Event Hubs, you have to make a decision about availability and consistency for your application.</span></span>

## <a name="availability"></a><span data-ttu-id="bb8c1-119">Tillgänglighet</span><span class="sxs-lookup"><span data-stu-id="bb8c1-119">Availability</span></span>
<span data-ttu-id="bb8c1-120">Det enklaste sättet att komma igång med Händelsehubbar är att använda standardinställningen.</span><span class="sxs-lookup"><span data-stu-id="bb8c1-120">The simplest way to get started with Event Hubs is to use the default behavior.</span></span> <span data-ttu-id="bb8c1-121">Om du skapar en ny `EventHubClient` objekt och använda den `Send` metoden händelserna automatiskt distribueras mellan partitioner i din event hub.</span><span class="sxs-lookup"><span data-stu-id="bb8c1-121">If you create a new `EventHubClient` object and use the `Send` method, your events are automatically distributed between partitions in your event hub.</span></span> <span data-ttu-id="bb8c1-122">Detta gör att störst upptid.</span><span class="sxs-lookup"><span data-stu-id="bb8c1-122">This behavior allows for the greatest amount of up time.</span></span>

<span data-ttu-id="bb8c1-123">Den här modellen är prioriterade för användningsområden som kräver högsta tid.</span><span class="sxs-lookup"><span data-stu-id="bb8c1-123">For use cases that require the maximum up time, this model is preferred.</span></span>

## <a name="consistency"></a><span data-ttu-id="bb8c1-124">Konsekvens</span><span class="sxs-lookup"><span data-stu-id="bb8c1-124">Consistency</span></span>
<span data-ttu-id="bb8c1-125">I vissa situationer kan det vara viktigt att sorteringen av händelser.</span><span class="sxs-lookup"><span data-stu-id="bb8c1-125">In some scenarios, the ordering of events can be important.</span></span> <span data-ttu-id="bb8c1-126">Du kan exempelvis vilja backend-systemet att bearbeta ett uppdateringskommando innan ett borttagningskommando.</span><span class="sxs-lookup"><span data-stu-id="bb8c1-126">For example, you may want your back-end system to process an update command before a delete command.</span></span> <span data-ttu-id="bb8c1-127">I den här instansen, du kan ange Partitionsnyckeln på en händelse eller Använd en `PartitionSender` objekt att endast skicka händelser till en viss partition.</span><span class="sxs-lookup"><span data-stu-id="bb8c1-127">In this instance, you can either set the partition key on an event, or use a `PartitionSender` object to only send events to a certain partition.</span></span> <span data-ttu-id="bb8c1-128">På så sätt att när dessa händelser läses från partitionen är de läsas i ordning.</span><span class="sxs-lookup"><span data-stu-id="bb8c1-128">Doing so ensures that when these events are read from the partition, they are read in order.</span></span>

<span data-ttu-id="bb8c1-129">Kom ihåg att om viss partitionen som du skickar är tillgänglig, visas ett felsvar med den här konfigurationen.</span><span class="sxs-lookup"><span data-stu-id="bb8c1-129">With this configuration, keep in mind that if the particular partition to which you are sending is unavailable, you will receive an error response.</span></span> <span data-ttu-id="bb8c1-130">Om du inte har en mappning till en enskild partition som en jämförelsepunkt skickar din händelse till nästa tillgängliga partition händelsehubbtjänsten.</span><span class="sxs-lookup"><span data-stu-id="bb8c1-130">As a point of comparison, if you do not have an affinity to a single partition, the Event Hubs service sends your event to the next available partition.</span></span>

<span data-ttu-id="bb8c1-131">En möjlig lösning så ordning när också maximera upptid, att aggregera händelser som en del av programmet för händelsebearbetning.</span><span class="sxs-lookup"><span data-stu-id="bb8c1-131">One possible solution to ensure ordering, while also maximizing up time, would be to aggregate events as part of your event processing application.</span></span> <span data-ttu-id="bb8c1-132">Det enklaste sättet att göra detta är att klona en händelse med en anpassad sequence number-egenskapen.</span><span class="sxs-lookup"><span data-stu-id="bb8c1-132">The easiest way to accomplish this is to stamp your event with a custom sequence number property.</span></span> <span data-ttu-id="bb8c1-133">Följande kod visar ett exempel:</span><span class="sxs-lookup"><span data-stu-id="bb8c1-133">The following code shows an example:</span></span>

```csharp
// Get the latest sequence number from your application
var sequenceNumber = GetNextSequenceNumber();
// Create a new EventData object by encoding a string as a byte array
var data = new EventData(Encoding.UTF8.GetBytes("This is my message..."));
// Set a custom sequence number property
data.Properties.Add("SequenceNumber", sequenceNumber);
// Send single message async
await eventHubClient.SendAsync(data);
```

<span data-ttu-id="bb8c1-134">Det här exemplet skickar din händelse till en av de tillgängliga partitionerna i din event hub och anger motsvarande sekvensnumret från ditt program.</span><span class="sxs-lookup"><span data-stu-id="bb8c1-134">This example sends your event to one of the available partitions in your event hub, and sets the corresponding sequence number from your application.</span></span> <span data-ttu-id="bb8c1-135">Denna lösning kräver tillstånd hållas av ditt program för bearbetning, men ger en slutpunkt som är mer troligt att vara tillgänglig för din avsändare.</span><span class="sxs-lookup"><span data-stu-id="bb8c1-135">This solution requires state to be kept by your processing application, but gives your senders an endpoint that is more likely to be available.</span></span>

## <a name="next-steps"></a><span data-ttu-id="bb8c1-136">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="bb8c1-136">Next steps</span></span>
<span data-ttu-id="bb8c1-137">Du kan lära dig mer om Event Hubs genom att gå till följande länkar:</span><span class="sxs-lookup"><span data-stu-id="bb8c1-137">You can learn more about Event Hubs by visiting the following links:</span></span>

* [<span data-ttu-id="bb8c1-138">Översikt över Event Hubs service</span><span class="sxs-lookup"><span data-stu-id="bb8c1-138">Event Hubs service overview</span></span>](event-hubs-what-is-event-hubs.md)
* [<span data-ttu-id="bb8c1-139">Skapa en Event Hub</span><span class="sxs-lookup"><span data-stu-id="bb8c1-139">Create an event hub</span></span>](event-hubs-create.md)
