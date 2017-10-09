---
title: 'Azure Stream Analytics: Optimera ditt jobb toouse Streaming enheter effektivt | Microsoft Docs'
description: "Fråga bästa praxis för skalning och prestanda i Azure Stream Analytics."
keywords: "Streaming unit frågeprestanda"
services: stream-analytics
documentationcenter: 
author: jeffstokes72
manager: jhubbard
editor: cgronlun
ms.assetid: 
ms.service: stream-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: data-services
ms.date: 04/20/2017
ms.author: jeffstok
ms.openlocfilehash: 5ad98b34d625190a879260f54c9eff0294e230cb
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="optimize-your-job-toouse-streaming-units-efficiently"></a><span data-ttu-id="24fed-104">Optimera din jobbet toouse Streaming enheter effektivt</span><span class="sxs-lookup"><span data-stu-id="24fed-104">Optimize your job toouse Streaming Units efficiently</span></span>

<span data-ttu-id="24fed-105">Azure Stream Analytics aggregerar hello prestanda ”vikt” för ett jobb som körs i enheter för strömning (SUs).</span><span class="sxs-lookup"><span data-stu-id="24fed-105">Azure Stream Analytics aggregates hello performance "weight" of running a job into Streaming Units (SUs).</span></span> <span data-ttu-id="24fed-106">SUs representerar hello datorresurser som förbrukade tooexecute ett jobb.</span><span class="sxs-lookup"><span data-stu-id="24fed-106">SUs represent hello computing resources that are consumed tooexecute a job.</span></span> <span data-ttu-id="24fed-107">SUs ge ett sätt toodescribe hello relativa händelsebearbetning kapacitet baserat på ett blandat mått av CPU, minne, läser och skriver priser.</span><span class="sxs-lookup"><span data-stu-id="24fed-107">SUs provide a way toodescribe hello relative event processing capacity based on a blended measure of CPU, memory, and read and write rates.</span></span> <span data-ttu-id="24fed-108">Den här kapaciteten kan du fokusera på hello frågan logik och tar bort du inte behöver tooknow lagring nivå prestandaöverväganden, allokera minne för ditt jobb manuellt och ungefärlig hello CPU core-antal behövs toorun ditt jobb inom rimlig tid.</span><span class="sxs-lookup"><span data-stu-id="24fed-108">This capacity lets you focus on hello query logic and removes you from needing tooknow storage tier performance considerations, allocate memory for your job manually, and approximate hello CPU core-count needed toorun your job in a timely manner.</span></span>

## <a name="how-many-sus-are-required-for-a-job"></a><span data-ttu-id="24fed-109">Hur många SUs krävs för ett jobb?</span><span class="sxs-lookup"><span data-stu-id="24fed-109">How many SUs are required for a job?</span></span>

<span data-ttu-id="24fed-110">Väljer hello antalet nödvändiga SUs för ett visst jobb beror på hello partition konfiguration för hello indata och hello-fråga som har definierats inom hello jobb.</span><span class="sxs-lookup"><span data-stu-id="24fed-110">Choosing hello number of required SUs for a particular job depends on hello partition configuration for hello inputs and hello query that's defined within hello job.</span></span> <span data-ttu-id="24fed-111">Hej **skala** bladet kan du tooset hello rätt antal SUs.</span><span class="sxs-lookup"><span data-stu-id="24fed-111">hello **Scale** blade allows you tooset hello right number of SUs.</span></span> <span data-ttu-id="24fed-112">Det är en god rutin tooallocate flera SUs än vad som behövs.</span><span class="sxs-lookup"><span data-stu-id="24fed-112">It is a best practice tooallocate more SUs than needed.</span></span> <span data-ttu-id="24fed-113">hello Stream Analytics-bearbetningsmotorn optimeras för svarstid och genomströmning till hello kostnaden för att allokera ytterligare minne.</span><span class="sxs-lookup"><span data-stu-id="24fed-113">hello Stream Analytics processing engine optimizes for latency and throughput at hello cost of allocating additional memory.</span></span>

<span data-ttu-id="24fed-114">I allmänhet hello bästa praxis är toostart med 6 SUs för frågor som inte använder *PARTITION BY*.</span><span class="sxs-lookup"><span data-stu-id="24fed-114">In general, hello best practice is toostart with 6 SUs for queries that don't use *PARTITION BY*.</span></span> <span data-ttu-id="24fed-115">Sedan fastställa hello av platsen genom att använda en prövningsmetod med där du ändra hello antal SUs när du skickar representativt datamängder och undersöka hello SU % användning mått.</span><span class="sxs-lookup"><span data-stu-id="24fed-115">Then determine hello sweet spot by using a trial and error method in which you modify hello number of SUs after you pass representative amounts of data and examine hello SU %Utilization metric.</span></span>

<span data-ttu-id="24fed-116">Azure Stream Analytics behåller händelser i ett fönster som kallas hello ”beställning buffert” innan den börjar eventuell bearbetning.</span><span class="sxs-lookup"><span data-stu-id="24fed-116">Azure Stream Analytics keeps events in a window called hello “reorder buffer” before it starts any processing.</span></span> <span data-ttu-id="24fed-117">Händelser är sorterade i hello ordna om fönster med tid och efterföljande åtgärder utförs för närvarande sorteras hello-händelser.</span><span class="sxs-lookup"><span data-stu-id="24fed-117">Events are sorted within hello reorder window by time, and subsequent operations are performed on hello temporally sorted events.</span></span> <span data-ttu-id="24fed-118">Ordningen händelser vid tidpunkten säkerställer att hello-operator har insyn i alla hello händelser i hello anges tidsperioden.</span><span class="sxs-lookup"><span data-stu-id="24fed-118">Reordering events by time ensures that hello operator has visibility into all hello events in hello stipulated timeframe.</span></span> <span data-ttu-id="24fed-119">Du kan också hello operatorn utför hello nödvändiga bearbetning och ger utdata.</span><span class="sxs-lookup"><span data-stu-id="24fed-119">It also lets hello operator perform hello requisite processing and produce an output.</span></span> <span data-ttu-id="24fed-120">En sidoeffekt av denna metod är att bearbetningen försenas hello varaktighet hello ordna om fönster.</span><span class="sxs-lookup"><span data-stu-id="24fed-120">A side effect of this mechanism is that processing is delayed by hello duration of hello reorder window.</span></span> <span data-ttu-id="24fed-121">hello minneskrav för hello jobb (som påverkar SU förbrukning) är en funktion av hello storleken på den här ordna om fönster och hello antalet händelser som finns i den.</span><span class="sxs-lookup"><span data-stu-id="24fed-121">hello memory footprint of hello job (which affects SU consumption) is a function of hello size of this reorder window and hello number of events contained within it.</span></span>

> [!NOTE]
> <span data-ttu-id="24fed-122">När hello antalet läsare ändras under jobbet uppgraderingar, skrivs tillfälligt varningar tooaudit loggar.</span><span class="sxs-lookup"><span data-stu-id="24fed-122">When hello number of readers changes during job upgrades, transient warnings are written tooaudit logs.</span></span> <span data-ttu-id="24fed-123">Stream Analytics-jobb återskapa automatiskt från dessa tillfälliga problem.</span><span class="sxs-lookup"><span data-stu-id="24fed-123">Stream Analytics jobs automatically recover from these transient issues.</span></span>

## <a name="common-high-memory-causes-for-high-su-usage-for-running-jobs"></a><span data-ttu-id="24fed-124">Vanliga orsaker till hög minne för SU hårt för att köra jobb</span><span class="sxs-lookup"><span data-stu-id="24fed-124">Common high-memory causes for high SU usage for running jobs</span></span>

### <a name="high-cardinality-for-group-by"></a><span data-ttu-id="24fed-125">Hög kardinalitet för GROUP BY</span><span class="sxs-lookup"><span data-stu-id="24fed-125">High cardinality for GROUP BY</span></span>

<span data-ttu-id="24fed-126">Hej Kardinaliteten för inkommande händelser avgör minnesanvändning för hello jobb.</span><span class="sxs-lookup"><span data-stu-id="24fed-126">hello cardinality of incoming events dictates memory usage for hello job.</span></span>

<span data-ttu-id="24fed-127">Till exempel i `SELECT count(*) from input group by clustered, tumblingwindow (minutes, 5)`, hello nummer som är associerat med **klustrade** är hello kardinalitet för hello fråga.</span><span class="sxs-lookup"><span data-stu-id="24fed-127">For example, in `SELECT count(*) from input group by clustered, tumblingwindow (minutes, 5)`, hello number associated with **clustered** is hello cardinality of hello query.</span></span>

<span data-ttu-id="24fed-128">toomitigate problem som orsakas av hög kardinalitet skala ut hello frågan genom att öka partitioner som använder **PARTITION BY**.</span><span class="sxs-lookup"><span data-stu-id="24fed-128">toomitigate issues that are caused by high cardinality, scale out hello query by increasing partitions using **PARTITION BY**.</span></span>

```
Select count(*) from input
Partition By clusterid
GROUP BY clustered tumblingwindow (minutes, 5)
```

<span data-ttu-id="24fed-129">Hej antalet *klustrade* är hello kardinalitet för GROUP BY här.</span><span class="sxs-lookup"><span data-stu-id="24fed-129">hello number of *clustered* is hello cardinality of GROUP BY here.</span></span>

<span data-ttu-id="24fed-130">När hello frågan är partitionerad den som är utspridda över flera noder.</span><span class="sxs-lookup"><span data-stu-id="24fed-130">After hello query is partitioned, it is spread out over multiple nodes.</span></span> <span data-ttu-id="24fed-131">Därför minskas hello antalet händelser som kommer till varje nod, vilket minskar i sin tur hello storleken på hello beställning buffert.</span><span class="sxs-lookup"><span data-stu-id="24fed-131">As a result, hello number of events coming into each node is reduced, which in turn reduces hello size of hello reorder buffer.</span></span> <span data-ttu-id="24fed-132">Du bör också partitionera event hub partitioner av partitionid.</span><span class="sxs-lookup"><span data-stu-id="24fed-132">You should also partition event hub partitions by partitionid.</span></span>

### <a name="high-unmatched-event-count-for-join"></a><span data-ttu-id="24fed-133">Hög omatchat antal för koppling</span><span class="sxs-lookup"><span data-stu-id="24fed-133">High unmatched event count for JOIN</span></span>

<span data-ttu-id="24fed-134">hello antal omatchade händelser i en koppling påverkar hello minnesanvändning hello frågan.</span><span class="sxs-lookup"><span data-stu-id="24fed-134">hello number of unmatched events in a JOIN affects hello memory utilization of hello query.</span></span> <span data-ttu-id="24fed-135">Till exempel ta en fråga som söker efter toofind hello antal exponeringar som genererar klick:</span><span class="sxs-lookup"><span data-stu-id="24fed-135">For example, take a query that is looking toofind hello number of ad impressions that generates clicks:</span></span>

```
SELECT id from clicks INNER JOIN,
impressions on impressions.id = clicks.id AND DATEDIFF(hour, impressions, clicks) between 0 AND 10
```

<span data-ttu-id="24fed-136">Det är möjligt att många annonser visas och några klickningar skapas i det här scenariot.</span><span class="sxs-lookup"><span data-stu-id="24fed-136">In this scenario, it is possible that many ads are shown and few clicks are generated.</span></span> <span data-ttu-id="24fed-137">Om detta inträffar kräver hello jobbet tookeep alla hello händelser inom hello tidsperioden.</span><span class="sxs-lookup"><span data-stu-id="24fed-137">Such a result would require hello job tookeep all hello events within hello time window.</span></span> <span data-ttu-id="24fed-138">Hej mängden minne som används är proportionell toohello fönstret storlek och händelsen hastighet.</span><span class="sxs-lookup"><span data-stu-id="24fed-138">hello amount of memory consumed is proportional toohello window size and event rate.</span></span> 

<span data-ttu-id="24fed-139">toomitigate den här situationen kan skala ut hello frågan genom att öka partitioner genom att använda PARTITION.</span><span class="sxs-lookup"><span data-stu-id="24fed-139">toomitigate this situation, scale out hello query by increasing partitions by using PARTITION BY.</span></span> 

<span data-ttu-id="24fed-140">När hello frågan är partitionerad den som är utspridda över flera noder för bearbetning.</span><span class="sxs-lookup"><span data-stu-id="24fed-140">After hello query is partitioned, it is spread out over multiple processing nodes.</span></span> <span data-ttu-id="24fed-141">Därför minskas hello antalet händelser som kommer till varje nod, vilket minskar i sin tur hello storleken på hello beställning buffert.</span><span class="sxs-lookup"><span data-stu-id="24fed-141">As a result, hello number of events coming into each node is reduced, which in turn reduces hello size of hello reorder buffer.</span></span>

### <a name="large-number-of-out-of-order-events"></a><span data-ttu-id="24fed-142">Stort antal oordnade händelser</span><span class="sxs-lookup"><span data-stu-id="24fed-142">Large number of out of order events</span></span> 

<span data-ttu-id="24fed-143">Ett stort antal oordnade händelser inom ett stort tidsfönster gör hello hello ”ordna om bufferten” toobe större storlek.</span><span class="sxs-lookup"><span data-stu-id="24fed-143">A large number of out of order events within a large time window causes hello size of hello "reorder buffer" toobe larger.</span></span> <span data-ttu-id="24fed-144">toomitigate den här situationen kan skala hello frågan genom att öka partitioner genom att använda PARTITION.</span><span class="sxs-lookup"><span data-stu-id="24fed-144">toomitigate this situation, scale hello query by increasing partitions by using PARTITION BY.</span></span> <span data-ttu-id="24fed-145">När hello frågan är partitionerad den som är utspridda över flera noder.</span><span class="sxs-lookup"><span data-stu-id="24fed-145">After hello query is partitioned, it is spread out over multiple nodes.</span></span> <span data-ttu-id="24fed-146">Därför minskas hello antalet händelser som kommer till varje nod, vilket minskar i sin tur hello storleken på hello beställning buffert.</span><span class="sxs-lookup"><span data-stu-id="24fed-146">As a result, hello number of events coming into each node is reduced, which in turn reduces hello size of hello reorder buffer.</span></span> 


## <a name="get-help"></a><span data-ttu-id="24fed-147">Få hjälp</span><span class="sxs-lookup"><span data-stu-id="24fed-147">Get help</span></span>
<span data-ttu-id="24fed-148">För ytterligare hjälp försök vår [Azure Stream Analytics-forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics).</span><span class="sxs-lookup"><span data-stu-id="24fed-148">For further assistance, try our [Azure Stream Analytics forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics).</span></span>

## <a name="next-steps"></a><span data-ttu-id="24fed-149">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="24fed-149">Next steps</span></span>
* [<span data-ttu-id="24fed-150">Introduktion tooAzure Stream Analytics</span><span class="sxs-lookup"><span data-stu-id="24fed-150">Introduction tooAzure Stream Analytics</span></span>](stream-analytics-introduction.md)
* [<span data-ttu-id="24fed-151">Komma igång med Azure Stream Analytics</span><span class="sxs-lookup"><span data-stu-id="24fed-151">Get started using Azure Stream Analytics</span></span>](stream-analytics-real-time-fraud-detection.md)
* [<span data-ttu-id="24fed-152">Skala Azure Stream Analytics-jobb</span><span class="sxs-lookup"><span data-stu-id="24fed-152">Scale Azure Stream Analytics jobs</span></span>](stream-analytics-scale-jobs.md)
* [<span data-ttu-id="24fed-153">Azure Stream Analytics query language-referens</span><span class="sxs-lookup"><span data-stu-id="24fed-153">Azure Stream Analytics query language reference</span></span>](https://msdn.microsoft.com/library/azure/dn834998.aspx)
* [<span data-ttu-id="24fed-154">Azure Stream Analytics Management REST API-referens</span><span class="sxs-lookup"><span data-stu-id="24fed-154">Azure Stream Analytics Management REST API reference</span></span>](https://msdn.microsoft.com/library/azure/dn835031.aspx)
