---
title: "Azure Stream Analytics: Optimera ditt jobb för att använda enheter för strömning effektivt | Microsoft Docs"
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
ms.openlocfilehash: 1441a5df4fd92abf85763ca9a1512503b1a0da56
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/29/2017
---
# <a name="optimize-your-job-to-use-streaming-units-efficiently"></a><span data-ttu-id="48810-104">Optimera ditt jobb för att effektivt använda enheter för strömning</span><span class="sxs-lookup"><span data-stu-id="48810-104">Optimize your job to use Streaming Units efficiently</span></span>

<span data-ttu-id="48810-105">Azure Stream Analytics aggregerar prestanda ”vikt” för ett jobb som körs i enheter för strömning (SUs).</span><span class="sxs-lookup"><span data-stu-id="48810-105">Azure Stream Analytics aggregates the performance "weight" of running a job into Streaming Units (SUs).</span></span> <span data-ttu-id="48810-106">SUs representerar de resurser som används för att köra ett jobb.</span><span class="sxs-lookup"><span data-stu-id="48810-106">SUs represent the computing resources that are consumed to execute a job.</span></span> <span data-ttu-id="48810-107">SU:er är ett sätt att beskriva den relativa kapaciteten för händelsebearbetning utifrån en kombination av mått för processor, minne, och läs- och skrivhastigheter.</span><span class="sxs-lookup"><span data-stu-id="48810-107">SUs provide a way to describe the relative event processing capacity based on a blended measure of CPU, memory, and read and write rates.</span></span> <span data-ttu-id="48810-108">Den här kapaciteten kan du fokusera på att frågan logik och tar bort du behöver känna lagring tjänstnivån prestandaöverväganden allokera minne för ditt jobb manuellt och ungefärlig core-antalet processorer för att köra ditt jobb inom rimlig tid.</span><span class="sxs-lookup"><span data-stu-id="48810-108">This capacity lets you focus on the query logic and removes you from needing to know storage tier performance considerations, allocate memory for your job manually, and approximate the CPU core-count needed to run your job in a timely manner.</span></span>

## <a name="how-many-sus-are-required-for-a-job"></a><span data-ttu-id="48810-109">Hur många SUs krävs för ett jobb?</span><span class="sxs-lookup"><span data-stu-id="48810-109">How many SUs are required for a job?</span></span>

<span data-ttu-id="48810-110">Att välja antalet nödvändiga SUs för ett visst jobb beror på hur partition för indata och den fråga som definieras i jobbet.</span><span class="sxs-lookup"><span data-stu-id="48810-110">Choosing the number of required SUs for a particular job depends on the partition configuration for the inputs and the query that's defined within the job.</span></span> <span data-ttu-id="48810-111">Den **skala** bladet kan du ange antalet SUs.</span><span class="sxs-lookup"><span data-stu-id="48810-111">The **Scale** blade allows you to set the right number of SUs.</span></span> <span data-ttu-id="48810-112">Det är en bra idé att allokera mer SUs än vad som behövs.</span><span class="sxs-lookup"><span data-stu-id="48810-112">It is a best practice to allocate more SUs than needed.</span></span> <span data-ttu-id="48810-113">Stream Analytics-bearbetningsmotorn optimeras för svarstid och genomströmning på bekostnad av ytterligare minnesallokering.</span><span class="sxs-lookup"><span data-stu-id="48810-113">The Stream Analytics processing engine optimizes for latency and throughput at the cost of allocating additional memory.</span></span>

<span data-ttu-id="48810-114">I allmänhet är det bästa sättet är att starta med 6 SUs för frågor som inte använder *PARTITION BY*.</span><span class="sxs-lookup"><span data-stu-id="48810-114">In general, the best practice is to start with 6 SUs for queries that don't use *PARTITION BY*.</span></span> <span data-ttu-id="48810-115">Sedan fastställa av platsen genom att använda en prövningsmetod med vilket ändra antalet SUs när du skickar representativt datamängder och undersöka SU % användning mått.</span><span class="sxs-lookup"><span data-stu-id="48810-115">Then determine the sweet spot by using a trial and error method in which you modify the number of SUs after you pass representative amounts of data and examine the SU %Utilization metric.</span></span>

<span data-ttu-id="48810-116">Azure Stream Analytics behåller händelser i ett fönster som kallas ”beställning bufferten” innan den börjar eventuell bearbetning.</span><span class="sxs-lookup"><span data-stu-id="48810-116">Azure Stream Analytics keeps events in a window called the “reorder buffer” before it starts any processing.</span></span> <span data-ttu-id="48810-117">Händelser är sorterade i fönstret beställning av tid och efterföljande åtgärder utförs för närvarande sorterade-händelser.</span><span class="sxs-lookup"><span data-stu-id="48810-117">Events are sorted within the reorder window by time, and subsequent operations are performed on the temporally sorted events.</span></span> <span data-ttu-id="48810-118">Ordningen händelser vid tidpunkten säkerställer att operatorn har insyn i alla händelser i den angivna tidsperioden.</span><span class="sxs-lookup"><span data-stu-id="48810-118">Reordering events by time ensures that the operator has visibility into all the events in the stipulated timeframe.</span></span> <span data-ttu-id="48810-119">Du kan också operatorn utför nödvändiga bearbetning och ger utdata.</span><span class="sxs-lookup"><span data-stu-id="48810-119">It also lets the operator perform the requisite processing and produce an output.</span></span> <span data-ttu-id="48810-120">En sidoeffekt av denna mekanism är att bearbetningen försenas av tidsperioden för att ändra ordning.</span><span class="sxs-lookup"><span data-stu-id="48810-120">A side effect of this mechanism is that processing is delayed by the duration of the reorder window.</span></span> <span data-ttu-id="48810-121">Minneskrav för jobbet (vilket påverkar SU förbrukning) är en funktion av storleken på fönstret Ändra ordning och antalet händelser som finns i den.</span><span class="sxs-lookup"><span data-stu-id="48810-121">The memory footprint of the job (which affects SU consumption) is a function of the size of this reorder window and the number of events contained within it.</span></span>

> [!NOTE]
> <span data-ttu-id="48810-122">När antalet läsare ändras under jobbet uppgraderingar, skrivs tillfälligt varningar till granskningsloggar.</span><span class="sxs-lookup"><span data-stu-id="48810-122">When the number of readers changes during job upgrades, transient warnings are written to audit logs.</span></span> <span data-ttu-id="48810-123">Stream Analytics-jobb återskapa automatiskt från dessa tillfälliga problem.</span><span class="sxs-lookup"><span data-stu-id="48810-123">Stream Analytics jobs automatically recover from these transient issues.</span></span>

## <a name="common-high-memory-causes-for-high-su-usage-for-running-jobs"></a><span data-ttu-id="48810-124">Vanliga orsaker till hög minne för SU hårt för att köra jobb</span><span class="sxs-lookup"><span data-stu-id="48810-124">Common high-memory causes for high SU usage for running jobs</span></span>

### <a name="high-cardinality-for-group-by"></a><span data-ttu-id="48810-125">Hög kardinalitet för GROUP BY</span><span class="sxs-lookup"><span data-stu-id="48810-125">High cardinality for GROUP BY</span></span>

<span data-ttu-id="48810-126">Inkommande händelsers kardinalitet avgör minnesanvändning för jobbet.</span><span class="sxs-lookup"><span data-stu-id="48810-126">The cardinality of incoming events dictates memory usage for the job.</span></span>

<span data-ttu-id="48810-127">Till exempel i `SELECT count(*) from input group by clustered, tumblingwindow (minutes, 5)`, hur många som är associerade med **klustrade** är kardinalitet för frågan.</span><span class="sxs-lookup"><span data-stu-id="48810-127">For example, in `SELECT count(*) from input group by clustered, tumblingwindow (minutes, 5)`, the number associated with **clustered** is the cardinality of the query.</span></span>

<span data-ttu-id="48810-128">För att åtgärda problem som orsakas av hög kardinalitet, skala ut frågan genom att öka partitioner som använder **PARTITION BY**.</span><span class="sxs-lookup"><span data-stu-id="48810-128">To mitigate issues that are caused by high cardinality, scale out the query by increasing partitions using **PARTITION BY**.</span></span>

```
Select count(*) from input
Partition By clusterid
GROUP BY clustered tumblingwindow (minutes, 5)
```

<span data-ttu-id="48810-129">Antalet *klustrade* är här till Kardinaliteten för GROUP BY.</span><span class="sxs-lookup"><span data-stu-id="48810-129">The number of *clustered* is the cardinality of GROUP BY here.</span></span>

<span data-ttu-id="48810-130">Efter att frågan är partitionerad den som är utspridda över flera noder.</span><span class="sxs-lookup"><span data-stu-id="48810-130">After the query is partitioned, it is spread out over multiple nodes.</span></span> <span data-ttu-id="48810-131">Därför kan minskas antalet händelser som kommer till varje nod, vilket i sin tur minskar storleken på bufferten som beställning.</span><span class="sxs-lookup"><span data-stu-id="48810-131">As a result, the number of events coming into each node is reduced, which in turn reduces the size of the reorder buffer.</span></span> <span data-ttu-id="48810-132">Du bör också partitionera event hub partitioner av partitionid.</span><span class="sxs-lookup"><span data-stu-id="48810-132">You should also partition event hub partitions by partitionid.</span></span>

### <a name="high-unmatched-event-count-for-join"></a><span data-ttu-id="48810-133">Hög omatchat antal för koppling</span><span class="sxs-lookup"><span data-stu-id="48810-133">High unmatched event count for JOIN</span></span>

<span data-ttu-id="48810-134">Antalet omatchade händelser i en koppling påverkar frågan minnesanvändning.</span><span class="sxs-lookup"><span data-stu-id="48810-134">The number of unmatched events in a JOIN affects the memory utilization of the query.</span></span> <span data-ttu-id="48810-135">Till exempel ta en fråga som du vill ta reda på antalet Annonsexponeringar som genererar klick:</span><span class="sxs-lookup"><span data-stu-id="48810-135">For example, take a query that is looking to find the number of ad impressions that generates clicks:</span></span>

```
SELECT id from clicks INNER JOIN,
impressions on impressions.id = clicks.id AND DATEDIFF(hour, impressions, clicks) between 0 AND 10
```

<span data-ttu-id="48810-136">Det är möjligt att många annonser visas och några klickningar skapas i det här scenariot.</span><span class="sxs-lookup"><span data-stu-id="48810-136">In this scenario, it is possible that many ads are shown and few clicks are generated.</span></span> <span data-ttu-id="48810-137">Om detta inträffar skulle kräva jobbet att behålla alla händelser inom tidsperioden.</span><span class="sxs-lookup"><span data-stu-id="48810-137">Such a result would require the job to keep all the events within the time window.</span></span> <span data-ttu-id="48810-138">Mängden minne som används är proportionell mot fönstret storlek och händelsen hastighet.</span><span class="sxs-lookup"><span data-stu-id="48810-138">The amount of memory consumed is proportional to the window size and event rate.</span></span> 

<span data-ttu-id="48810-139">Skala ut frågan genom att öka partitioner genom att använda PARTITION för att minska den här situationen.</span><span class="sxs-lookup"><span data-stu-id="48810-139">To mitigate this situation, scale out the query by increasing partitions by using PARTITION BY.</span></span> 

<span data-ttu-id="48810-140">Efter att frågan är partitionerad den som är utspridda över flera noder för bearbetning.</span><span class="sxs-lookup"><span data-stu-id="48810-140">After the query is partitioned, it is spread out over multiple processing nodes.</span></span> <span data-ttu-id="48810-141">Därför kan minskas antalet händelser som kommer till varje nod, vilket i sin tur minskar storleken på bufferten som beställning.</span><span class="sxs-lookup"><span data-stu-id="48810-141">As a result, the number of events coming into each node is reduced, which in turn reduces the size of the reorder buffer.</span></span>

### <a name="large-number-of-out-of-order-events"></a><span data-ttu-id="48810-142">Stort antal oordnade händelser</span><span class="sxs-lookup"><span data-stu-id="48810-142">Large number of out of order events</span></span> 

<span data-ttu-id="48810-143">Ett stort antal oordnade händelser inom ett stort tidsfönster orsakar storlek buffertens ”ändra” ska vara större.</span><span class="sxs-lookup"><span data-stu-id="48810-143">A large number of out of order events within a large time window causes the size of the "reorder buffer" to be larger.</span></span> <span data-ttu-id="48810-144">Skala frågan för att minska den här situationen genom att öka partitioner genom att använda PARTITION.</span><span class="sxs-lookup"><span data-stu-id="48810-144">To mitigate this situation, scale the query by increasing partitions by using PARTITION BY.</span></span> <span data-ttu-id="48810-145">Efter att frågan är partitionerad den som är utspridda över flera noder.</span><span class="sxs-lookup"><span data-stu-id="48810-145">After the query is partitioned, it is spread out over multiple nodes.</span></span> <span data-ttu-id="48810-146">Därför kan minskas antalet händelser som kommer till varje nod, vilket i sin tur minskar storleken på bufferten som beställning.</span><span class="sxs-lookup"><span data-stu-id="48810-146">As a result, the number of events coming into each node is reduced, which in turn reduces the size of the reorder buffer.</span></span> 


## <a name="get-help"></a><span data-ttu-id="48810-147">Få hjälp</span><span class="sxs-lookup"><span data-stu-id="48810-147">Get help</span></span>
<span data-ttu-id="48810-148">För ytterligare hjälp försök vår [Azure Stream Analytics-forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics).</span><span class="sxs-lookup"><span data-stu-id="48810-148">For further assistance, try our [Azure Stream Analytics forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics).</span></span>

## <a name="next-steps"></a><span data-ttu-id="48810-149">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="48810-149">Next steps</span></span>
* [<span data-ttu-id="48810-150">Introduktion till Azure Stream Analytics</span><span class="sxs-lookup"><span data-stu-id="48810-150">Introduction to Azure Stream Analytics</span></span>](stream-analytics-introduction.md)
* [<span data-ttu-id="48810-151">Komma igång med Azure Stream Analytics</span><span class="sxs-lookup"><span data-stu-id="48810-151">Get started using Azure Stream Analytics</span></span>](stream-analytics-real-time-fraud-detection.md)
* [<span data-ttu-id="48810-152">Skala Azure Stream Analytics-jobb</span><span class="sxs-lookup"><span data-stu-id="48810-152">Scale Azure Stream Analytics jobs</span></span>](stream-analytics-scale-jobs.md)
* [<span data-ttu-id="48810-153">Azure Stream Analytics query language-referens</span><span class="sxs-lookup"><span data-stu-id="48810-153">Azure Stream Analytics query language reference</span></span>](https://msdn.microsoft.com/library/azure/dn834998.aspx)
* [<span data-ttu-id="48810-154">Azure Stream Analytics Management REST API-referens</span><span class="sxs-lookup"><span data-stu-id="48810-154">Azure Stream Analytics Management REST API reference</span></span>](https://msdn.microsoft.com/library/azure/dn835031.aspx)
