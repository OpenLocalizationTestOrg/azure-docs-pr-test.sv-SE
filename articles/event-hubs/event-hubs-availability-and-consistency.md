---
title: "aaaAvailability och konsekvens i Händelsehubbar i Azure | Microsoft Docs"
description: "Hur tooprovide hello maximal mängd tillgänglighet och konsekvens med Händelsehubbar i Azure med hjälp av partitioner."
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
ms.openlocfilehash: a8ededaae1589830da21cb8910ca694d2d628bd2
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="availability-and-consistency-in-event-hubs"></a><span data-ttu-id="7f126-103">Tillgänglighet och konsekvens i Händelsehubbar</span><span class="sxs-lookup"><span data-stu-id="7f126-103">Availability and consistency in Event Hubs</span></span>

## <a name="overview"></a><span data-ttu-id="7f126-104">Översikt</span><span class="sxs-lookup"><span data-stu-id="7f126-104">Overview</span></span>
<span data-ttu-id="7f126-105">Händelsehubbar i Azure använder en [partitionering modellen](event-hubs-features.md#partitions) tooimprove tillgänglighet och parallellisering inom en enskild händelsehubb.</span><span class="sxs-lookup"><span data-stu-id="7f126-105">Azure Event Hubs uses a [partitioning model](event-hubs-features.md#partitions) tooimprove availability and parallelization within a single event hub.</span></span> <span data-ttu-id="7f126-106">Till exempel om en händelsehubb har fyra partitioner och en av dessa partitioner flyttas från en server tooanother i en åtgärd för belastningsutjämning, du fortfarande skicka och ta emot från tre partitioner.</span><span class="sxs-lookup"><span data-stu-id="7f126-106">For example, if an event hub has four partitions, and one of those partitions is moved from one server tooanother in a load balancing operation, you can still send and receive from three other partitions.</span></span> <span data-ttu-id="7f126-107">Dessutom med fler partitioner kan du toohave flera samtidiga läsare bearbetning av dina data, förbättra din sammanställda genomflöde.</span><span class="sxs-lookup"><span data-stu-id="7f126-107">Additionally, having more partitions enables you toohave more concurrent readers processing your data, improving your aggregate throughput.</span></span> <span data-ttu-id="7f126-108">Förstå hello följderna av att partitionering och ordning i ett distribuerat system är en viktig aspekt för lösningen.</span><span class="sxs-lookup"><span data-stu-id="7f126-108">Understanding hello implications of partitioning and ordering in a distributed system is a critical aspect of solution design.</span></span>

<span data-ttu-id="7f126-109">toohelp förklarar hello säkerhetsaspekten ordning och tillgänglighet, se hello [CAP-sats](https://en.wikipedia.org/wiki/CAP_theorem), även kallad Brewers-sats.</span><span class="sxs-lookup"><span data-stu-id="7f126-109">toohelp explain hello trade-off between ordering and availability, see hello [CAP theorem](https://en.wikipedia.org/wiki/CAP_theorem), also known as Brewer's theorem.</span></span> <span data-ttu-id="7f126-110">Den här sats beskrivs hello val mellan konsekvens, tillgänglighet och tolerans för partitionen.</span><span class="sxs-lookup"><span data-stu-id="7f126-110">This theorem discusses hello choice between consistency, availability, and partition tolerance.</span></span>

<span data-ttu-id="7f126-111">Brewers sats definierar konsekvens och tillgänglighet på följande sätt:</span><span class="sxs-lookup"><span data-stu-id="7f126-111">Brewer's theorem defines consistency and availability as follows:</span></span>
* <span data-ttu-id="7f126-112">Partitionera tolerans: hello möjligheten för en databearbetning system toocontinue databearbetning, även om en partition fel inträffar.</span><span class="sxs-lookup"><span data-stu-id="7f126-112">Partition tolerance: hello ability of a data processing system toocontinue processing data even if a partition failure occurs.</span></span>
* <span data-ttu-id="7f126-113">Tillgänglighet: en icke-misslyckas nod returnerar ett rimligt svar inom en rimlig tidsperiod (med några fel eller timeout).</span><span class="sxs-lookup"><span data-stu-id="7f126-113">Availability: a non-failing node returns a reasonable response within a reasonable amount of time (with no errors or timeouts).</span></span>
* <span data-ttu-id="7f126-114">Konsekvenskontroll: Läs garanteras tooreturn hello senaste skrivåtgärder för en viss klient.</span><span class="sxs-lookup"><span data-stu-id="7f126-114">Consistency: a read is guaranteed tooreturn hello most recent write for a given client.</span></span>

## <a name="partition-tolerance"></a><span data-ttu-id="7f126-115">Tolerans för partition</span><span class="sxs-lookup"><span data-stu-id="7f126-115">Partition tolerance</span></span>
<span data-ttu-id="7f126-116">Händelsehubbar är byggt på en partitionerad datamodell.</span><span class="sxs-lookup"><span data-stu-id="7f126-116">Event Hubs is built on top of a partitioned data model.</span></span> <span data-ttu-id="7f126-117">Du kan konfigurera hello antalet partitioner i din event hub under installationen, men du kan inte ändra det här värdet senare.</span><span class="sxs-lookup"><span data-stu-id="7f126-117">You can configure hello number of partitions in your event hub during setup, but you cannot change this value later.</span></span> <span data-ttu-id="7f126-118">Eftersom du måste använda partitioner med Händelsehubbar kan ha du toomake beslut om tillgänglighet och konsekvens för ditt program.</span><span class="sxs-lookup"><span data-stu-id="7f126-118">Since you must use partitions with Event Hubs, you have toomake a decision about availability and consistency for your application.</span></span>

## <a name="availability"></a><span data-ttu-id="7f126-119">Tillgänglighet</span><span class="sxs-lookup"><span data-stu-id="7f126-119">Availability</span></span>
<span data-ttu-id="7f126-120">hello enklaste sättet tooget igång med Händelsehubbar är toouse hello standardbeteendet.</span><span class="sxs-lookup"><span data-stu-id="7f126-120">hello simplest way tooget started with Event Hubs is toouse hello default behavior.</span></span> <span data-ttu-id="7f126-121">Om du skapar en ny `EventHubClient` objekt och använda hello `Send` metoden händelserna automatiskt distribueras mellan partitioner i din event hub.</span><span class="sxs-lookup"><span data-stu-id="7f126-121">If you create a new `EventHubClient` object and use hello `Send` method, your events are automatically distributed between partitions in your event hub.</span></span> <span data-ttu-id="7f126-122">Detta gör att hello största mängden tid.</span><span class="sxs-lookup"><span data-stu-id="7f126-122">This behavior allows for hello greatest amount of up time.</span></span>

<span data-ttu-id="7f126-123">Den här modellen är prioriterade för användningsområden som kräver hello maximal tid.</span><span class="sxs-lookup"><span data-stu-id="7f126-123">For use cases that require hello maximum up time, this model is preferred.</span></span>

## <a name="consistency"></a><span data-ttu-id="7f126-124">Konsekvens</span><span class="sxs-lookup"><span data-stu-id="7f126-124">Consistency</span></span>
<span data-ttu-id="7f126-125">I vissa situationer kan det vara viktigt att hello sorteringen av händelser.</span><span class="sxs-lookup"><span data-stu-id="7f126-125">In some scenarios, hello ordering of events can be important.</span></span> <span data-ttu-id="7f126-126">Du kanske till exempel din serverdel system tooprocess ett uppdateringskommando innan ett borttagningskommando.</span><span class="sxs-lookup"><span data-stu-id="7f126-126">For example, you may want your back-end system tooprocess an update command before a delete command.</span></span> <span data-ttu-id="7f126-127">I den här instansen, du kan ange hello partitionsnyckel på en händelse eller Använd en `PartitionSender` objekt tooonly skicka händelser tooa viss partition.</span><span class="sxs-lookup"><span data-stu-id="7f126-127">In this instance, you can either set hello partition key on an event, or use a `PartitionSender` object tooonly send events tooa certain partition.</span></span> <span data-ttu-id="7f126-128">På så sätt att när dessa händelser läses från hello partition, är de läsas i ordning.</span><span class="sxs-lookup"><span data-stu-id="7f126-128">Doing so ensures that when these events are read from hello partition, they are read in order.</span></span>

<span data-ttu-id="7f126-129">Kom ihåg att om hello viss partition toowhich som du skickar är tillgänglig, visas ett felsvar med den här konfigurationen.</span><span class="sxs-lookup"><span data-stu-id="7f126-129">With this configuration, keep in mind that if hello particular partition toowhich you are sending is unavailable, you will receive an error response.</span></span> <span data-ttu-id="7f126-130">Som en jämförelsepunkt om du inte har en mappning mellan tooa enskild partition, skickar hello händelsehubbtjänsten din händelse toohello nästa tillgängliga partition.</span><span class="sxs-lookup"><span data-stu-id="7f126-130">As a point of comparison, if you do not have an affinity tooa single partition, hello Event Hubs service sends your event toohello next available partition.</span></span>

<span data-ttu-id="7f126-131">En möjlig lösning tooensure ordning när också maximera upptid, skulle vara tooaggregate händelser som en del av programmet för händelsebearbetning.</span><span class="sxs-lookup"><span data-stu-id="7f126-131">One possible solution tooensure ordering, while also maximizing up time, would be tooaggregate events as part of your event processing application.</span></span> <span data-ttu-id="7f126-132">Hej enklaste sättet tooaccomplish detta är toostamp din händelse med en anpassad sequence number-egenskapen.</span><span class="sxs-lookup"><span data-stu-id="7f126-132">hello easiest way tooaccomplish this is toostamp your event with a custom sequence number property.</span></span> <span data-ttu-id="7f126-133">hello följande kod visar ett exempel:</span><span class="sxs-lookup"><span data-stu-id="7f126-133">hello following code shows an example:</span></span>

```csharp
// Get hello latest sequence number from your application
var sequenceNumber = GetNextSequenceNumber();
// Create a new EventData object by encoding a string as a byte array
var data = new EventData(Encoding.UTF8.GetBytes("This is my message..."));
// Set a custom sequence number property
data.Properties.Add("SequenceNumber", sequenceNumber);
// Send single message async
await eventHubClient.SendAsync(data);
```

<span data-ttu-id="7f126-134">Det här exemplet skickar din händelse tooone över hello tillgängliga partitioner i din event hub och anger hello motsvarande sekvensnumret från ditt program.</span><span class="sxs-lookup"><span data-stu-id="7f126-134">This example sends your event tooone of hello available partitions in your event hub, and sets hello corresponding sequence number from your application.</span></span> <span data-ttu-id="7f126-135">Denna lösning kräver tillstånd toobe som tillämpningsprogrammet bearbetning, men ger en slutpunkt som är mer troligt toobe som är tillgängliga för din avsändare.</span><span class="sxs-lookup"><span data-stu-id="7f126-135">This solution requires state toobe kept by your processing application, but gives your senders an endpoint that is more likely toobe available.</span></span>

## <a name="next-steps"></a><span data-ttu-id="7f126-136">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="7f126-136">Next steps</span></span>
<span data-ttu-id="7f126-137">Mer information om Händelsehubbar genom att besöka hello följande länkar:</span><span class="sxs-lookup"><span data-stu-id="7f126-137">You can learn more about Event Hubs by visiting hello following links:</span></span>

* [<span data-ttu-id="7f126-138">Översikt över Event Hubs service</span><span class="sxs-lookup"><span data-stu-id="7f126-138">Event Hubs service overview</span></span>](event-hubs-what-is-event-hubs.md)
* [<span data-ttu-id="7f126-139">Skapa en Event Hub</span><span class="sxs-lookup"><span data-stu-id="7f126-139">Create an event hub</span></span>](event-hubs-create.md)
