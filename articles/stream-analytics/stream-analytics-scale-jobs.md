---
title: "aaaScale Stream Analytics-jobb tooincrease genomströmning | Microsoft Docs"
description: "Lär dig hur tooscale Stream Analytics-jobb genom att konfigurera inkommande partitioner, justera hello frågedefinitionen och ange jobbet enheter för strömning."
keywords: "data som strömmas, finjustera strömning databehandling analytics"
services: stream-analytics
documentationcenter: 
author: jeffstokes72
manager: jhubbard
editor: cgronlun
ms.assetid: 7e857ddb-71dd-4537-b7ab-4524335d7b35
ms.service: stream-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: data-services
ms.date: 06/22/2017
ms.author: jeffstok
ms.openlocfilehash: 4ba8f6b2f8bfebd52cfa07696b501b42cda21f75
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="scale-azure-stream-analytics-jobs-tooincrease-stream-data-processing-throughput"></a><span data-ttu-id="a094a-104">Skala Azure Stream Analytics-jobb tooincrease dataströmmen databearbetning genomflöde</span><span class="sxs-lookup"><span data-stu-id="a094a-104">Scale Azure Stream Analytics jobs tooincrease stream data processing throughput</span></span>
<span data-ttu-id="a094a-105">Den här artikeln visar hur tootune en Stream Analytics fråga tooincrease genomströmning för Streaming Analytics-jobb.</span><span class="sxs-lookup"><span data-stu-id="a094a-105">This article shows you how tootune a Stream Analytics query tooincrease throughput for Streaming Analytics jobs.</span></span> <span data-ttu-id="a094a-106">Du lär dig hur tooscale Stream Analytics-jobb genom att konfigurera inkommande partitioner, prestandajustering hello analytics fråga definition och beräkna och ange jobbet *strömningsenheter* (SUs).</span><span class="sxs-lookup"><span data-stu-id="a094a-106">You learn how tooscale Stream Analytics jobs by configuring input partitions, tuning hello analytics query definition, and calculating and setting job *streaming units* (SUs).</span></span> 

## <a name="what-are-hello-parts-of-a-stream-analytics-job"></a><span data-ttu-id="a094a-107">Vad är hello delar av ett Stream Analytics-jobb?</span><span class="sxs-lookup"><span data-stu-id="a094a-107">What are hello parts of a Stream Analytics job?</span></span>
<span data-ttu-id="a094a-108">En Stream Analytics-jobbdefinition innehåller indata, en fråge- och utdata.</span><span class="sxs-lookup"><span data-stu-id="a094a-108">A Stream Analytics job definition includes inputs, a query, and output.</span></span> <span data-ttu-id="a094a-109">Indata är där hello jobbet läser hello dataström från.</span><span class="sxs-lookup"><span data-stu-id="a094a-109">Inputs are where hello job reads hello data stream from.</span></span> <span data-ttu-id="a094a-110">hello frågan är hello används tootransform data Indataströmmen och hello utdata är där hello jobbet skickar hello jobbet resultatet till.</span><span class="sxs-lookup"><span data-stu-id="a094a-110">hello query is used tootransform hello data input stream, and hello output is where hello job sends hello job results to.</span></span>  

<span data-ttu-id="a094a-111">Ett jobb kräver minst en Indatakällan för strömmande data.</span><span class="sxs-lookup"><span data-stu-id="a094a-111">A job requires at least one input source for data streaming.</span></span> <span data-ttu-id="a094a-112">hello kan-dataströmmen inkommande datakälla lagras i en Azure händelsehubb eller i Azure blob storage.</span><span class="sxs-lookup"><span data-stu-id="a094a-112">hello data stream input source can be stored in an Azure event hub or in Azure blob storage.</span></span> <span data-ttu-id="a094a-113">Mer information finns i [introduktion tooAzure Stream Analytics](stream-analytics-introduction.md) och [komma igång med Azure Stream Analytics](stream-analytics-real-time-fraud-detection.md).</span><span class="sxs-lookup"><span data-stu-id="a094a-113">For more information, see [Introduction tooAzure Stream Analytics](stream-analytics-introduction.md) and [Get started using Azure Stream Analytics](stream-analytics-real-time-fraud-detection.md).</span></span>

## <a name="partitions-in-event-hubs-and-azure-storage"></a><span data-ttu-id="a094a-114">Partitioner i händelsehubbar och Azure-lagring</span><span class="sxs-lookup"><span data-stu-id="a094a-114">Partitions in event hubs and Azure storage</span></span>
<span data-ttu-id="a094a-115">Skalning Stream Analytics-jobbet utnyttjar partitioner i hello indata eller utdata.</span><span class="sxs-lookup"><span data-stu-id="a094a-115">Scaling a Stream Analytics job takes advantage of partitions in hello input or output.</span></span> <span data-ttu-id="a094a-116">Partitionering kan du dela upp data i delmängder baserat på en partitionsnyckel.</span><span class="sxs-lookup"><span data-stu-id="a094a-116">Partitioning lets you divide data into subsets based on a partition key.</span></span> <span data-ttu-id="a094a-117">En process som förbrukar hello data (till exempel ett Streaming Analytics-jobb) kan använda och skriva olika partitioner parallellt, vilket ökar genomströmningen.</span><span class="sxs-lookup"><span data-stu-id="a094a-117">A process that consumes hello data (such as a Streaming Analytics job) can consume and write different partitions in parallel, which increases throughput.</span></span> <span data-ttu-id="a094a-118">När du arbetar med Streaming Analytics kan dra du nytta av partitionering i händelsehubbar och i Blob storage.</span><span class="sxs-lookup"><span data-stu-id="a094a-118">When you work with Streaming Analytics, you can take advantage of partitioning in event hubs and in Blob storage.</span></span> 

<span data-ttu-id="a094a-119">Mer information om partitioner finns hello följande artiklar:</span><span class="sxs-lookup"><span data-stu-id="a094a-119">For more information about partitions, see hello following articles:</span></span>

* [<span data-ttu-id="a094a-120">Översikt över Event Hubs funktioner</span><span class="sxs-lookup"><span data-stu-id="a094a-120">Event Hubs features overview</span></span>](../event-hubs/event-hubs-features.md#partitions)
* [<span data-ttu-id="a094a-121">Data partitionering</span><span class="sxs-lookup"><span data-stu-id="a094a-121">Data partitioning</span></span>](https://docs.microsoft.com/azure/architecture/best-practices/data-partitioning#partitioning-azure-blob-storage)


## <a name="streaming-units-sus"></a><span data-ttu-id="a094a-122">Enheter för strömning (SUs)</span><span class="sxs-lookup"><span data-stu-id="a094a-122">Streaming units (SUs)</span></span>
<span data-ttu-id="a094a-123">Strömmande enheter (SUs) representerar hello resurser och datorkraft som krävs i ordning tooexecute Azure Stream Analytics-jobbet.</span><span class="sxs-lookup"><span data-stu-id="a094a-123">Streaming units (SUs) represent hello resources and computing power that are required in order tooexecute an Azure Stream Analytics job.</span></span> <span data-ttu-id="a094a-124">SUs ge ett sätt toodescribe hello relativa händelsebearbetning kapacitet baserat på ett blandat mått av CPU, minne, läser och skriver priser.</span><span class="sxs-lookup"><span data-stu-id="a094a-124">SUs provide a way toodescribe hello relative event processing capacity based on a blended measure of CPU, memory, and read and write rates.</span></span> <span data-ttu-id="a094a-125">Varje SU motsvarar tooroughly 1 MB per sekund för genomströmning.</span><span class="sxs-lookup"><span data-stu-id="a094a-125">Each SU corresponds tooroughly 1 MB/second of throughput.</span></span> 

<span data-ttu-id="a094a-126">Om du väljer hur många SUs krävs för ett specifikt jobb beror på hello partition konfiguration för hello indata och hello frågan för hello jobb.</span><span class="sxs-lookup"><span data-stu-id="a094a-126">Choosing how many SUs are required for a particular job depends on hello partition configuration for hello inputs and on hello query defined for hello job.</span></span> <span data-ttu-id="a094a-127">Du kan välja upp tooyour kvoten i SUs för ett jobb.</span><span class="sxs-lookup"><span data-stu-id="a094a-127">You can select up tooyour quota in SUs for a job.</span></span> <span data-ttu-id="a094a-128">Som standard har varje Azure-prenumeration en kvot på upp too50 SUs för alla hello analytics-jobb i en viss region.</span><span class="sxs-lookup"><span data-stu-id="a094a-128">By default, each Azure subscription has a quota of up too50 SUs for all hello analytics jobs in a specific region.</span></span> <span data-ttu-id="a094a-129">tooincrease SUs för dina prenumerationer utöver den här kvoten Kontakta [Microsoft-supporten](http://support.microsoft.com).</span><span class="sxs-lookup"><span data-stu-id="a094a-129">tooincrease SUs for your subscriptions beyond this quota, contact [Microsoft Support](http://support.microsoft.com).</span></span> <span data-ttu-id="a094a-130">Giltiga värden för SUs per jobb är 1, 3, 6, och upp i steg 6.</span><span class="sxs-lookup"><span data-stu-id="a094a-130">Valid values for SUs per job are 1, 3, 6, and up in increments of 6.</span></span>

## <a name="embarrassingly-parallel-jobs"></a><span data-ttu-id="a094a-131">Embarrassingly parallella jobb</span><span class="sxs-lookup"><span data-stu-id="a094a-131">Embarrassingly parallel jobs</span></span>
<span data-ttu-id="a094a-132">En *embarrassingly parallella* jobbet är hello mest skalbara scenario som vi har i Azure Stream Analytics.</span><span class="sxs-lookup"><span data-stu-id="a094a-132">An *embarrassingly parallel* job is hello most scalable scenario we have in Azure Stream Analytics.</span></span> <span data-ttu-id="a094a-133">Ansluter en partition av hello inkommande tooone instans av hello frågan tooone partition hello utdata.</span><span class="sxs-lookup"><span data-stu-id="a094a-133">It connects one partition of hello input tooone instance of hello query tooone partition of hello output.</span></span> <span data-ttu-id="a094a-134">Den här parallellitet har hello följande krav:</span><span class="sxs-lookup"><span data-stu-id="a094a-134">This parallelism has hello following requirements:</span></span>

1. <span data-ttu-id="a094a-135">Om din fråga logik som är beroende av hello samma nyckel som bearbetas av hello samma fråga instans, måste du se till att hello händelser gå toohello samma partition som indata.</span><span class="sxs-lookup"><span data-stu-id="a094a-135">If your query logic depends on hello same key being processed by hello same query instance, you must make sure that hello events go toohello same partition of your input.</span></span> <span data-ttu-id="a094a-136">För händelsehubbar, innebär detta att hello händelsedata måste ha hello **PartitionKey** värdet uppsättningen.</span><span class="sxs-lookup"><span data-stu-id="a094a-136">For event hubs, this means that hello event data must have hello **PartitionKey** value set.</span></span> <span data-ttu-id="a094a-137">Du kan också använda partitionerade avsändare.</span><span class="sxs-lookup"><span data-stu-id="a094a-137">Alternatively, you can use partitioned senders.</span></span> <span data-ttu-id="a094a-138">För blob-lagring innebär detta att hello händelser skickas toohello samma partition mapp.</span><span class="sxs-lookup"><span data-stu-id="a094a-138">For blob storage, this means that hello events are sent toohello same partition folder.</span></span> <span data-ttu-id="a094a-139">Om din fråga logik inte kräver hello samma nyckel toobe bearbetas av hello samma fråga instans kan du ignorera det här kravet.</span><span class="sxs-lookup"><span data-stu-id="a094a-139">If your query logic does not require hello same key toobe processed by hello same query instance, you can ignore this requirement.</span></span> <span data-ttu-id="a094a-140">Ett exempel på den här logiken skulle vara en enkel fråga väljer project filter.</span><span class="sxs-lookup"><span data-stu-id="a094a-140">An example of this logic would be a simple select-project-filter query.</span></span>  

2. <span data-ttu-id="a094a-141">När hello data är placerade på hello inkommande sida, måste du se till att frågan är partitionerad.</span><span class="sxs-lookup"><span data-stu-id="a094a-141">Once hello data is laid out on hello input side, you must make sure that your query is partitioned.</span></span> <span data-ttu-id="a094a-142">Detta kräver toouse **Partition By** i alla hello steg.</span><span class="sxs-lookup"><span data-stu-id="a094a-142">This requires you toouse **Partition By** in all hello steps.</span></span> <span data-ttu-id="a094a-143">Flera steg tillåts, men de måste vara partitionerad hello samma nyckel.</span><span class="sxs-lookup"><span data-stu-id="a094a-143">Multiple steps are allowed, but they all must be partitioned by hello same key.</span></span> <span data-ttu-id="a094a-144">För närvarande hello partitionering nyckeln måste anges för**PartitionId** för hello jobbet toobe fullständigt parallellt.</span><span class="sxs-lookup"><span data-stu-id="a094a-144">Currently, hello partitioning key must be set too**PartitionId** in order for hello job toobe fully parallel.</span></span>  

3. <span data-ttu-id="a094a-145">För närvarande stöder endast händelsehubbar och blob-lagring partitionerade utdata.</span><span class="sxs-lookup"><span data-stu-id="a094a-145">Currently only event hubs and blob storage support partitioned output.</span></span> <span data-ttu-id="a094a-146">Event hub utdata, måste du konfigurera hello partition viktiga toobe **PartitionId**.</span><span class="sxs-lookup"><span data-stu-id="a094a-146">For event hub output, you must configure hello partition key toobe **PartitionId**.</span></span> <span data-ttu-id="a094a-147">För blob storage-utdata har du inte toodo något.</span><span class="sxs-lookup"><span data-stu-id="a094a-147">For blob storage output, you don't have toodo anything.</span></span>  

4. <span data-ttu-id="a094a-148">hello antalet inkommande partitioner måste vara lika med hello antalet partitioner för utdata.</span><span class="sxs-lookup"><span data-stu-id="a094a-148">hello number of input partitions must equal hello number of output partitions.</span></span> <span data-ttu-id="a094a-149">BLOB storage-utdata stöds inte för närvarande partitioner.</span><span class="sxs-lookup"><span data-stu-id="a094a-149">Blob storage output doesn't currently support partitions.</span></span> <span data-ttu-id="a094a-150">Men det är OK, eftersom den ärver hello partitioneringsschema av hello överordnad fråga.</span><span class="sxs-lookup"><span data-stu-id="a094a-150">But that's okay, because it inherits hello partitioning scheme of hello upstream query.</span></span> <span data-ttu-id="a094a-151">Här följer exempel på partitionen värden som tillåter ett fullständigt parallella jobb:</span><span class="sxs-lookup"><span data-stu-id="a094a-151">Here are examples of partition values that allow a fully parallel job:</span></span>  

   * <span data-ttu-id="a094a-152">8 event hub inkommande partitioner och 8 händelsehubb utdata partitioner</span><span class="sxs-lookup"><span data-stu-id="a094a-152">8 event hub input partitions and 8 event hub output partitions</span></span>
   * <span data-ttu-id="a094a-153">8 event hub inkommande partitioner och blob storage-utdata</span><span class="sxs-lookup"><span data-stu-id="a094a-153">8 event hub input partitions and blob storage output</span></span>  
   * <span data-ttu-id="a094a-154">8 blob storage inkommande partitioner och blob storage-utdata</span><span class="sxs-lookup"><span data-stu-id="a094a-154">8 blob storage input partitions and blob storage output</span></span>  
   * <span data-ttu-id="a094a-155">8 blob storage inkommande partitioner och 8 event hub utdata partitioner</span><span class="sxs-lookup"><span data-stu-id="a094a-155">8 blob storage input partitions and 8 event hub output partitions</span></span>  

<span data-ttu-id="a094a-156">hello beskrivs följande avsnitt några exempelscenarier som embarrassingly parallella.</span><span class="sxs-lookup"><span data-stu-id="a094a-156">hello following sections discuss some example scenarios that are embarrassingly parallel.</span></span>

### <a name="simple-query"></a><span data-ttu-id="a094a-157">Enkel fråga</span><span class="sxs-lookup"><span data-stu-id="a094a-157">Simple query</span></span>

* <span data-ttu-id="a094a-158">Indata: Event hub med 8 partitioner</span><span class="sxs-lookup"><span data-stu-id="a094a-158">Input: Event hub with 8 partitions</span></span>
* <span data-ttu-id="a094a-159">Utdata: Event hub med 8 partitioner</span><span class="sxs-lookup"><span data-stu-id="a094a-159">Output: Event hub with 8 partitions</span></span>

<span data-ttu-id="a094a-160">Fråga:</span><span class="sxs-lookup"><span data-stu-id="a094a-160">Query:</span></span>

    SELECT TollBoothId
    FROM Input1 Partition By PartitionId
    WHERE TollBoothId > 100

<span data-ttu-id="a094a-161">Den här frågan är ett enkelt filter.</span><span class="sxs-lookup"><span data-stu-id="a094a-161">This query is a simple filter.</span></span> <span data-ttu-id="a094a-162">Vi behöver därför inte tooworry om partitionering hello indata som skickas toohello händelsehubb.</span><span class="sxs-lookup"><span data-stu-id="a094a-162">Therefore, we don't need tooworry about partitioning hello input that is being sent toohello event hub.</span></span> <span data-ttu-id="a094a-163">Observera att hello-frågan inkluderar **Partition av PartitionId**, så att den uppfyller krav #2 från tidigare.</span><span class="sxs-lookup"><span data-stu-id="a094a-163">Notice that hello query includes **Partition By PartitionId**, so it fulfills requirement #2 from earlier.</span></span> <span data-ttu-id="a094a-164">Hello utdata vi måste tooconfigure hello event hub utdata i hello jobbet toohave hello partitionera nyckeluppsättning för**PartitionId**.</span><span class="sxs-lookup"><span data-stu-id="a094a-164">For hello output, we need tooconfigure hello event hub output in hello job toohave hello parition key set too**PartitionId**.</span></span> <span data-ttu-id="a094a-165">En senaste kontrollen är toomake att hello antalet inkommande partitioner är lika toohello antalet partitioner för utdata.</span><span class="sxs-lookup"><span data-stu-id="a094a-165">One last check is toomake sure that hello number of input partitions is equal toohello number of output partitions.</span></span>

### <a name="query-with-a-grouping-key"></a><span data-ttu-id="a094a-166">Fråga med en grupperingsnyckel för</span><span class="sxs-lookup"><span data-stu-id="a094a-166">Query with a grouping key</span></span>

* <span data-ttu-id="a094a-167">Indata: Event hub med 8 partitioner</span><span class="sxs-lookup"><span data-stu-id="a094a-167">Input: Event hub with 8 partitions</span></span>
* <span data-ttu-id="a094a-168">Utdata: Blob storage</span><span class="sxs-lookup"><span data-stu-id="a094a-168">Output: Blob storage</span></span>

<span data-ttu-id="a094a-169">Fråga:</span><span class="sxs-lookup"><span data-stu-id="a094a-169">Query:</span></span>

    SELECT COUNT(*) AS Count, TollBoothId
    FROM Input1 Partition By PartitionId
    GROUP BY TumblingWindow(minute, 3), TollBoothId, PartitionId

<span data-ttu-id="a094a-170">Den här frågan har en gruppering.</span><span class="sxs-lookup"><span data-stu-id="a094a-170">This query has a grouping key.</span></span> <span data-ttu-id="a094a-171">Därför hello toobe för samma nyckel måste bearbetas av hello samma fråga instans, vilket innebär att händelser måste skickas som toohello händelsehubb på ett sätt som är partitionerad.</span><span class="sxs-lookup"><span data-stu-id="a094a-171">Therefore, hello same key needs toobe processed by hello same query instance, which means that events must be sent toohello event hub in a partitioned manner.</span></span> <span data-ttu-id="a094a-172">Men vilken nyckel ska användas?</span><span class="sxs-lookup"><span data-stu-id="a094a-172">But which key should be used?</span></span> <span data-ttu-id="a094a-173">**PartitionId** är ett begrepp som jobbet logik.</span><span class="sxs-lookup"><span data-stu-id="a094a-173">**PartitionId** is a job-logic concept.</span></span> <span data-ttu-id="a094a-174">hello nyckeln faktiskt värnar om är **TollBoothId**, så hello **PartitionKey** värdet av hello händelsedata ska **TollBoothId**.</span><span class="sxs-lookup"><span data-stu-id="a094a-174">hello key we actually care about is **TollBoothId**, so hello **PartitionKey** value of hello event data should be **TollBoothId**.</span></span> <span data-ttu-id="a094a-175">Vi göra detta i hello frågan genom att ange **Partition By** för**PartitionId**.</span><span class="sxs-lookup"><span data-stu-id="a094a-175">We do this in hello query by setting **Partition By** too**PartitionId**.</span></span> <span data-ttu-id="a094a-176">Eftersom hello utdata är blob storage, behöver vi inte tooworry om hur du konfigurerar en partitionsnyckelvärde, enligt kravet #4.</span><span class="sxs-lookup"><span data-stu-id="a094a-176">Since hello output is blob storage, we don't need tooworry about configuring a partition key value, as per requirement #4.</span></span>

### <a name="multi-step-query-with-a-grouping-key"></a><span data-ttu-id="a094a-177">Flera steg frågan med en grupperingsnyckel för</span><span class="sxs-lookup"><span data-stu-id="a094a-177">Multi-step query with a grouping key</span></span>
* <span data-ttu-id="a094a-178">Indata: Event hub med 8 partitioner</span><span class="sxs-lookup"><span data-stu-id="a094a-178">Input: Event hub with 8 partitions</span></span>
* <span data-ttu-id="a094a-179">Utdata: Händelsen NAV-instans med 8 partitioner</span><span class="sxs-lookup"><span data-stu-id="a094a-179">Output: Event hub instance with 8 partitions</span></span>

<span data-ttu-id="a094a-180">Fråga:</span><span class="sxs-lookup"><span data-stu-id="a094a-180">Query:</span></span>

    WITH Step1 AS (
    SELECT COUNT(*) AS Count, TollBoothId, PartitionId
    FROM Input1 Partition By PartitionId
    GROUP BY TumblingWindow(minute, 3), TollBoothId, PartitionId
    )

    SELECT SUM(Count) AS Count, TollBoothId
    FROM Step1 Partition By PartitionId
    GROUP BY TumblingWindow(minute, 3), TollBoothId, PartitionId

<span data-ttu-id="a094a-181">Den här frågan har en grupperingsnyckel, så hello toobe för samma nyckel måste bearbetas av hello samma fråga-instans.</span><span class="sxs-lookup"><span data-stu-id="a094a-181">This query has a grouping key, so hello same key needs toobe processed by hello same query instance.</span></span> <span data-ttu-id="a094a-182">Vi kan använda hello samma strategi som hello föregående exempel.</span><span class="sxs-lookup"><span data-stu-id="a094a-182">We can use hello same strategy as in hello previous example.</span></span> <span data-ttu-id="a094a-183">I det här fallet har hello-frågan flera steg.</span><span class="sxs-lookup"><span data-stu-id="a094a-183">In this case, hello query has multiple steps.</span></span> <span data-ttu-id="a094a-184">Har varje steg **Partition av PartitionId**?</span><span class="sxs-lookup"><span data-stu-id="a094a-184">Does each step have **Partition By PartitionId**?</span></span> <span data-ttu-id="a094a-185">Ja, så hello fråga uppfyller kravet #3.</span><span class="sxs-lookup"><span data-stu-id="a094a-185">Yes, so hello query fulfills requirement #3.</span></span> <span data-ttu-id="a094a-186">Hello utdata vi måste tooset hello partitionsnyckel för**PartitionId**, enligt tidigare diskussion.</span><span class="sxs-lookup"><span data-stu-id="a094a-186">For hello output, we need tooset hello partition key too**PartitionId**, as discussed earlier.</span></span> <span data-ttu-id="a094a-187">Vi kan också se att den har hello samma antal partitioner som hello indata.</span><span class="sxs-lookup"><span data-stu-id="a094a-187">We can also see that it has hello same number of partitions as hello input.</span></span>

## <a name="example-scenarios-that-are-not-embarrassingly-parallel"></a><span data-ttu-id="a094a-188">Exempelscenarier som *inte* embarrassingly parallellt</span><span class="sxs-lookup"><span data-stu-id="a094a-188">Example scenarios that are *not* embarrassingly parallel</span></span>

<span data-ttu-id="a094a-189">I föregående avsnitt hello visade vi några embarrassingly parallella scenarier.</span><span class="sxs-lookup"><span data-stu-id="a094a-189">In hello previous section, we showed some embarrassingly parallel scenarios.</span></span> <span data-ttu-id="a094a-190">I det här avsnittet diskuterar vi scenarier som inte uppfyller alla hello krav toobe embarrassingly parallellt.</span><span class="sxs-lookup"><span data-stu-id="a094a-190">In this section, we discuss scenarios that don't meet all hello requirements toobe embarrassingly parallel.</span></span> 

### <a name="mismatched-partition-count"></a><span data-ttu-id="a094a-191">Antal matchningar partitioner</span><span class="sxs-lookup"><span data-stu-id="a094a-191">Mismatched partition count</span></span>
* <span data-ttu-id="a094a-192">Indata: Event hub med 8 partitioner</span><span class="sxs-lookup"><span data-stu-id="a094a-192">Input: Event hub with 8 partitions</span></span>
* <span data-ttu-id="a094a-193">Utdata: Event hub med 32 partitioner</span><span class="sxs-lookup"><span data-stu-id="a094a-193">Output: Event hub with 32 partitions</span></span>

<span data-ttu-id="a094a-194">I det här fallet det spelar ingen roll vilken hello-frågan är.</span><span class="sxs-lookup"><span data-stu-id="a094a-194">In this case, it doesn't matter what hello query is.</span></span> <span data-ttu-id="a094a-195">Om hello inkommande partitionsantal inte matchar antalet för hello utdata partitioner, inte hello topologi embarrassingly parallellt.</span><span class="sxs-lookup"><span data-stu-id="a094a-195">If hello input partition count doesn't match hello output partition count, hello topology isn't embarrassingly parallel.</span></span>

### <a name="not-using-event-hubs-or-blob-storage-as-output"></a><span data-ttu-id="a094a-196">Inte använder händelsehubbar eller blob-lagring som utdata</span><span class="sxs-lookup"><span data-stu-id="a094a-196">Not using event hubs or blob storage as output</span></span>
* <span data-ttu-id="a094a-197">Indata: Event hub med 8 partitioner</span><span class="sxs-lookup"><span data-stu-id="a094a-197">Input: Event hub with 8 partitions</span></span>
* <span data-ttu-id="a094a-198">Utdata: PowerBI</span><span class="sxs-lookup"><span data-stu-id="a094a-198">Output: PowerBI</span></span>

<span data-ttu-id="a094a-199">PowerBI utdata stöder för närvarande inte partitionering.</span><span class="sxs-lookup"><span data-stu-id="a094a-199">PowerBI output doesn't currently support partitioning.</span></span> <span data-ttu-id="a094a-200">Det här scenariot är därför inte embarrassingly parallellt.</span><span class="sxs-lookup"><span data-stu-id="a094a-200">Therefore, this scenario is not embarrassingly parallel.</span></span>

### <a name="multi-step-query-with-different-partition-by-values"></a><span data-ttu-id="a094a-201">Flera steg fråga med olika Partition By-värden</span><span class="sxs-lookup"><span data-stu-id="a094a-201">Multi-step query with different Partition By values</span></span>
* <span data-ttu-id="a094a-202">Indata: Event hub med 8 partitioner</span><span class="sxs-lookup"><span data-stu-id="a094a-202">Input: Event hub with 8 partitions</span></span>
* <span data-ttu-id="a094a-203">Utdata: Event hub med 8 partitioner</span><span class="sxs-lookup"><span data-stu-id="a094a-203">Output: Event hub with 8 partitions</span></span>

<span data-ttu-id="a094a-204">Fråga:</span><span class="sxs-lookup"><span data-stu-id="a094a-204">Query:</span></span>

    WITH Step1 AS (
    SELECT COUNT(*) AS Count, TollBoothId, PartitionId
    FROM Input1 Partition By PartitionId
    GROUP BY TumblingWindow(minute, 3), TollBoothId, PartitionId
    )

    SELECT SUM(Count) AS Count, TollBoothId
    FROM Step1 Partition By TollBoothId
    GROUP BY TumblingWindow(minute, 3), TollBoothId

<span data-ttu-id="a094a-205">Som du ser hello andra steget använder **TollBoothId** som hello partitionering nyckel.</span><span class="sxs-lookup"><span data-stu-id="a094a-205">As you can see, hello second step uses **TollBoothId** as hello partitioning key.</span></span> <span data-ttu-id="a094a-206">Det här steget är inte hello samma hello första steg, och därför kräver oss toodo en blandad.</span><span class="sxs-lookup"><span data-stu-id="a094a-206">This step is not hello same as hello first step, and it therefore requires us toodo a shuffle.</span></span> 

<span data-ttu-id="a094a-207">hello Visar föregående exempel några Stream Analytics-jobb som överensstämmer för (inte eller) en embarrassingly parallella topologi.</span><span class="sxs-lookup"><span data-stu-id="a094a-207">hello preceding examples show some Stream Analytics jobs that conform too(or don't) an embarrassingly parallel topology.</span></span> <span data-ttu-id="a094a-208">Om de uppfyller har hello risken för maximal skala.</span><span class="sxs-lookup"><span data-stu-id="a094a-208">If they do conform, they have hello potential for maximum scale.</span></span> <span data-ttu-id="a094a-209">För jobb som inte passar något av dessa profiler vägledning skalning kommer att vara tillgänglig i framtida uppdateringar.</span><span class="sxs-lookup"><span data-stu-id="a094a-209">For jobs that don't fit one of these profiles, scaling guidance will be available in future updates.</span></span> <span data-ttu-id="a094a-210">Använd hello allmänna riktlinjer för tillfället i hello följande avsnitt.</span><span class="sxs-lookup"><span data-stu-id="a094a-210">For now, use hello general guidance in hello following sections.</span></span>

## <a name="calculate-hello-maximum-streaming-units-of-a-job"></a><span data-ttu-id="a094a-211">Beräkna Hej max strömmande enheter för ett jobb</span><span class="sxs-lookup"><span data-stu-id="a094a-211">Calculate hello maximum streaming units of a job</span></span>
<span data-ttu-id="a094a-212">hello totala antalet strömmande enheter som kan användas av ett Stream Analytics-jobb är beroende av hello antalet steg i hello frågan för hello jobbet och hello antalet partitioner för varje steg.</span><span class="sxs-lookup"><span data-stu-id="a094a-212">hello total number of streaming units that can be used by a Stream Analytics job depends on hello number of steps in hello query defined for hello job and hello number of partitions for each step.</span></span>

### <a name="steps-in-a-query"></a><span data-ttu-id="a094a-213">Stegen i en fråga</span><span class="sxs-lookup"><span data-stu-id="a094a-213">Steps in a query</span></span>
<span data-ttu-id="a094a-214">En fråga kan ha en eller flera steg.</span><span class="sxs-lookup"><span data-stu-id="a094a-214">A query can have one or many steps.</span></span> <span data-ttu-id="a094a-215">Varje steg finns en underfråga som definieras av hello **WITH** nyckelord.</span><span class="sxs-lookup"><span data-stu-id="a094a-215">Each step is a subquery defined by hello **WITH** keyword.</span></span> <span data-ttu-id="a094a-216">hello-fråga som är utanför hello **WITH** nyckelordet (enbart en fråga) också räknas som ett steg, till exempel hello **Välj** instruktionen i hello följande fråga:</span><span class="sxs-lookup"><span data-stu-id="a094a-216">hello query that is outside hello **WITH** keyword (one query only) is also counted as a step, such as hello **SELECT** statement in hello following query:</span></span>

    WITH Step1 AS (
        SELECT COUNT(*) AS Count, TollBoothId
        FROM Input1 Partition By PartitionId
        GROUP BY TumblingWindow(minute, 3), TollBoothId, PartitionId
    )

    SELECT SUM(Count) AS Count, TollBoothId
    FROM Step1
    GROUP BY TumblingWindow(minute,3), TollBoothId

<span data-ttu-id="a094a-217">Den här frågan har två steg.</span><span class="sxs-lookup"><span data-stu-id="a094a-217">This query has two steps.</span></span>

> [!NOTE]
> <span data-ttu-id="a094a-218">Den här frågan diskuteras i detalj senare i hello artikeln.</span><span class="sxs-lookup"><span data-stu-id="a094a-218">This query is discussed in more detail later in hello article.</span></span>
>  

### <a name="partition-a-step"></a><span data-ttu-id="a094a-219">Partitionera ett steg</span><span class="sxs-lookup"><span data-stu-id="a094a-219">Partition a step</span></span>
<span data-ttu-id="a094a-220">Partitionering ett steg kräver hello följande villkor:</span><span class="sxs-lookup"><span data-stu-id="a094a-220">Partitioning a step requires hello following conditions:</span></span>

* <span data-ttu-id="a094a-221">hello indatakälla partitioneras.</span><span class="sxs-lookup"><span data-stu-id="a094a-221">hello input source must be partitioned.</span></span> 
* <span data-ttu-id="a094a-222">Hej **Välj** hello frågas uttryck måste läsa från en partitionerad Indatakällan.</span><span class="sxs-lookup"><span data-stu-id="a094a-222">hello **SELECT** statement of hello query must read from a partitioned input source.</span></span>
* <span data-ttu-id="a094a-223">hello query hello steg måste ha hello **Partition By** nyckelord.</span><span class="sxs-lookup"><span data-stu-id="a094a-223">hello query within hello step must have hello **Partition By** keyword.</span></span>

<span data-ttu-id="a094a-224">När en fråga är partitionerad hello inkommande händelser är bearbetade och aggregerade i separat partitionsgrupper och utdata händelser genereras för varje hello grupper.</span><span class="sxs-lookup"><span data-stu-id="a094a-224">When a query is partitioned, hello input events are processed and aggregated in separate partition groups, and outputs events are generated for each of hello groups.</span></span> <span data-ttu-id="a094a-225">Om du vill att en kombinerad mängd måste du skapa en andra partitionerade steg tooaggregate.</span><span class="sxs-lookup"><span data-stu-id="a094a-225">If you want a combined aggregate, you must create a second non-partitioned step tooaggregate.</span></span>

### <a name="calculate-hello-max-streaming-units-for-a-job"></a><span data-ttu-id="a094a-226">Beräkna Hej max strömningsenheter för ett jobb</span><span class="sxs-lookup"><span data-stu-id="a094a-226">Calculate hello max streaming units for a job</span></span>
<span data-ttu-id="a094a-227">Alla åtgärder för partitionerade kan tillsammans skala upp toosix strömningsenheter (SUs) för ett Stream Analytics-jobb.</span><span class="sxs-lookup"><span data-stu-id="a094a-227">All non-partitioned steps together can scale up toosix streaming units (SUs) for a Stream Analytics job.</span></span> <span data-ttu-id="a094a-228">vara måste partitionerade tooadd SUs, ett steg.</span><span class="sxs-lookup"><span data-stu-id="a094a-228">tooadd SUs, a step must be partitioned.</span></span> <span data-ttu-id="a094a-229">Varje partition får ha sex SUs.</span><span class="sxs-lookup"><span data-stu-id="a094a-229">Each partition can have six SUs.</span></span>

<table border="1">
<tr><th><span data-ttu-id="a094a-230">Fråga</span><span class="sxs-lookup"><span data-stu-id="a094a-230">Query</span></span></th><th><span data-ttu-id="a094a-231">Max SUs för hello jobbet</span><span class="sxs-lookup"><span data-stu-id="a094a-231">Max SUs for hello job</span></span></th></td>

<tr><td>
<ul>
<li><span data-ttu-id="a094a-232">hello frågan innehåller ett steg.</span><span class="sxs-lookup"><span data-stu-id="a094a-232">hello query contains one step.</span></span></li>
<li><span data-ttu-id="a094a-233">hello steg inte har partitionerats.</span><span class="sxs-lookup"><span data-stu-id="a094a-233">hello step is not partitioned.</span></span></li>
</ul>
</td>
<td><span data-ttu-id="a094a-234">6</span><span class="sxs-lookup"><span data-stu-id="a094a-234">6</span></span></td></tr>

<tr><td>
<ul>
<li><span data-ttu-id="a094a-235">hello inkommande dataströmmen är partitionerad med 3.</span><span class="sxs-lookup"><span data-stu-id="a094a-235">hello input data stream is partitioned by 3.</span></span></li>
<li><span data-ttu-id="a094a-236">hello frågan innehåller ett steg.</span><span class="sxs-lookup"><span data-stu-id="a094a-236">hello query contains one step.</span></span></li>
<li><span data-ttu-id="a094a-237">hello steg är partitionerad.</span><span class="sxs-lookup"><span data-stu-id="a094a-237">hello step is partitioned.</span></span></li>
</ul>
</td>
<td><span data-ttu-id="a094a-238">18</span><span class="sxs-lookup"><span data-stu-id="a094a-238">18</span></span></td></tr>

<tr><td>
<ul>
<li><span data-ttu-id="a094a-239">hello frågan innehåller två steg.</span><span class="sxs-lookup"><span data-stu-id="a094a-239">hello query contains two steps.</span></span></li>
<li><span data-ttu-id="a094a-240">Ingen av hello stegen är partitionerad.</span><span class="sxs-lookup"><span data-stu-id="a094a-240">Neither of hello steps is partitioned.</span></span></li>
</ul>
</td>
<td><span data-ttu-id="a094a-241">6</span><span class="sxs-lookup"><span data-stu-id="a094a-241">6</span></span></td></tr>

<tr><td>
<ul>
<li><span data-ttu-id="a094a-242">hello inkommande dataströmmen är partitionerad med 3.</span><span class="sxs-lookup"><span data-stu-id="a094a-242">hello input data stream is partitioned by 3.</span></span></li>
<li><span data-ttu-id="a094a-243">hello frågan innehåller två steg.</span><span class="sxs-lookup"><span data-stu-id="a094a-243">hello query contains two steps.</span></span> <span data-ttu-id="a094a-244">hello inkommande steg är partitionerad och hello andra steget är inte.</span><span class="sxs-lookup"><span data-stu-id="a094a-244">hello input step is partitioned and hello second step is not.</span></span></li>
<li><span data-ttu-id="a094a-245">Hej <strong>Välj</strong> instruktionen läser från hello partitionerad indata.</span><span class="sxs-lookup"><span data-stu-id="a094a-245">hello <strong>SELECT</strong> statement reads from hello partitioned input.</span></span></li>
</ul>
</td>
<td><span data-ttu-id="a094a-246">24 (18 för partitionerade steg) + 6 partitionerade anvisningar</span><span class="sxs-lookup"><span data-stu-id="a094a-246">24 (18 for partitioned steps + 6 for non-partitioned steps)</span></span></td></tr>
</table>

### <a name="examples-of-scaling"></a><span data-ttu-id="a094a-247">Exempel på skalning</span><span class="sxs-lookup"><span data-stu-id="a094a-247">Examples of scaling</span></span>

<span data-ttu-id="a094a-248">hello beräknar följande fråga hello många bilar inom en minut tre period gå via en avgift station som har tre tollbooths.</span><span class="sxs-lookup"><span data-stu-id="a094a-248">hello following query calculates hello number of cars within a three-minute window going through a toll station that has three tollbooths.</span></span> <span data-ttu-id="a094a-249">Den här frågan kan skalas upp toosix SUs.</span><span class="sxs-lookup"><span data-stu-id="a094a-249">This query can be scaled up toosix SUs.</span></span>

    SELECT COUNT(*) AS Count, TollBoothId
    FROM Input1
    GROUP BY TumblingWindow(minute, 3), TollBoothId, PartitionId

<span data-ttu-id="a094a-250">toouse flera SUs för hello frågan, både hello inkommande dataström och hello frågan vara partitionerade.</span><span class="sxs-lookup"><span data-stu-id="a094a-250">toouse more SUs for hello query, both hello input data stream and hello query must be partitioned.</span></span> <span data-ttu-id="a094a-251">Eftersom hello data dataströmmen partition anges too3 kan hello följande ändrade frågan skalas upp too18 SUs:</span><span class="sxs-lookup"><span data-stu-id="a094a-251">Since hello data stream partition is set too3, hello following modified query can be scaled up too18 SUs:</span></span>

    SELECT COUNT(*) AS Count, TollBoothId
    FROM Input1 Partition By PartitionId
    GROUP BY TumblingWindow(minute, 3), TollBoothId, PartitionId

<span data-ttu-id="a094a-252">När en fråga är partitionerad hello inkommande händelser bearbetas och aggregeras i en separat partitionsgrupper.</span><span class="sxs-lookup"><span data-stu-id="a094a-252">When a query is partitioned, hello input events are processed and aggregated in separate partition groups.</span></span> <span data-ttu-id="a094a-253">Utdatahändelserna genereras även för hello grupper.</span><span class="sxs-lookup"><span data-stu-id="a094a-253">Output events are also generated for each of hello groups.</span></span> <span data-ttu-id="a094a-254">Partitionering kan orsaka vissa oväntade resultat när hello **GROUP BY** fältet är inte hello partitionsnyckel i hello inkommande dataströmmen.</span><span class="sxs-lookup"><span data-stu-id="a094a-254">Partitioning can cause some unexpected results when hello **GROUP BY** field is not hello partition key in hello input data stream.</span></span> <span data-ttu-id="a094a-255">Till exempel hello **TollBoothId** fält i hello föregående frågan är inte hello Partitionsnyckeln för **Input1**.</span><span class="sxs-lookup"><span data-stu-id="a094a-255">For example, hello **TollBoothId** field in hello previous query is not hello partition key of **Input1**.</span></span> <span data-ttu-id="a094a-256">hello resultatet är att hello data från vaktkur #1 kan spridas i flera partitioner.</span><span class="sxs-lookup"><span data-stu-id="a094a-256">hello result is that hello data from TollBooth #1 can be spread in multiple partitions.</span></span>

<span data-ttu-id="a094a-257">Var och en av hello **Input1** partitionerna bearbetas separat av Stream Analytics.</span><span class="sxs-lookup"><span data-stu-id="a094a-257">Each of hello **Input1** partitions will be processed separately by Stream Analytics.</span></span> <span data-ttu-id="a094a-258">Flera poster antalet hello bil för hello därför samma vaktkur i hello samma rullande fönster kommer att skapas.</span><span class="sxs-lookup"><span data-stu-id="a094a-258">As a result, multiple records of hello car count for hello same tollbooth in hello same Tumbling window will be created.</span></span> <span data-ttu-id="a094a-259">Det här problemet kan åtgärdas genom att lägga till ett icke-partition steg, som i följande exempel hello om hello inkommande partitionsnyckel inte kan ändras:</span><span class="sxs-lookup"><span data-stu-id="a094a-259">If hello input partition key can't be changed, this problem can be fixed by adding a non-partition step, as in hello following example:</span></span>

    WITH Step1 AS (
        SELECT COUNT(*) AS Count, TollBoothId
        FROM Input1 Partition By PartitionId
        GROUP BY TumblingWindow(minute, 3), TollBoothId, PartitionId
    )

    SELECT SUM(Count) AS Count, TollBoothId
    FROM Step1
    GROUP BY TumblingWindow(minute, 3), TollBoothId

<span data-ttu-id="a094a-260">Den här frågan kan vara skalade too24 SUs.</span><span class="sxs-lookup"><span data-stu-id="a094a-260">This query can be scaled too24 SUs.</span></span>

> [!NOTE]
> <span data-ttu-id="a094a-261">Om du ansluter till två dataströmmar, kontrollerar du att hello dataströmmar partitioneras av hello partitionsnyckel för hello kolumnen att du använder toocreate hello kopplingar.</span><span class="sxs-lookup"><span data-stu-id="a094a-261">If you are joining two streams, make sure that hello streams are partitioned by hello partition key of hello column that you use toocreate hello joins.</span></span> <span data-ttu-id="a094a-262">Kontrollera också att du har hello samma antal partitioner i båda dataströmmar.</span><span class="sxs-lookup"><span data-stu-id="a094a-262">Also make sure that you have hello same number of partitions in both streams.</span></span>
> 
> 

## <a name="configure-stream-analytics-streaming-units"></a><span data-ttu-id="a094a-263">Konfigurera Stream Analytics enheter för strömning</span><span class="sxs-lookup"><span data-stu-id="a094a-263">Configure Stream Analytics streaming units</span></span>

1. <span data-ttu-id="a094a-264">Logga in toohello [Azure-portalen](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="a094a-264">Sign in toohello [Azure portal](https://portal.azure.com).</span></span>
2. <span data-ttu-id="a094a-265">Hello lista över resurser, hitta hello Stream Analytics-jobbet att tooscale och sedan öppna den.</span><span class="sxs-lookup"><span data-stu-id="a094a-265">In hello list of resources, find hello Stream Analytics job that you want tooscale and then open it.</span></span>
3. <span data-ttu-id="a094a-266">I hello jobb-bladet under **konfigurera**, klickar du på **skala**.</span><span class="sxs-lookup"><span data-stu-id="a094a-266">In hello job blade, under **Configure**, click **Scale**.</span></span>

    ![Azure Stream Analytics-jobbet Portalkonfiguration][img.stream.analytics.preview.portal.settings.scale]

4. <span data-ttu-id="a094a-268">Använd hello skjutreglaget tooset hello SUs för hello jobbet.</span><span class="sxs-lookup"><span data-stu-id="a094a-268">Use hello slider tooset hello SUs for hello job.</span></span> <span data-ttu-id="a094a-269">Observera att du är begränsad toospecific SU inställningar.</span><span class="sxs-lookup"><span data-stu-id="a094a-269">Notice that you are limited toospecific SU settings.</span></span>


## <a name="monitor-job-performance"></a><span data-ttu-id="a094a-270">Övervaka jobb prestanda</span><span class="sxs-lookup"><span data-stu-id="a094a-270">Monitor job performance</span></span>
<span data-ttu-id="a094a-271">Med hello Azure-portalen kan spåra du hello genomflödet i ett jobb:</span><span class="sxs-lookup"><span data-stu-id="a094a-271">Using hello Azure portal, you can track hello throughput of a job:</span></span>

![Azure Stream Analytics övervaka jobb][img.stream.analytics.monitor.job]

<span data-ttu-id="a094a-273">Beräkna hello förväntades genomflödet för hello arbetsbelastning.</span><span class="sxs-lookup"><span data-stu-id="a094a-273">Calculate hello expected throughput of hello workload.</span></span> <span data-ttu-id="a094a-274">Om hello genomströmning är mindre än förväntat, finjustera hello inkommande partition, finjustera hello fråga och lägga till SUs tooyour jobb.</span><span class="sxs-lookup"><span data-stu-id="a094a-274">If hello throughput is less than expected, tune hello input partition, tune hello query, and add SUs tooyour job.</span></span>


## <a name="visualize-stream-analytics-throughput-at-scale-hello-raspberry-pi-scenario"></a><span data-ttu-id="a094a-275">Visualisera Stream Analytics genomflöde i skala: hello hallon Pi scenario</span><span class="sxs-lookup"><span data-stu-id="a094a-275">Visualize Stream Analytics throughput at scale: hello Raspberry Pi scenario</span></span>
<span data-ttu-id="a094a-276">toohelp du förstår hur Stream Analytics-jobb skala, vi utföra ett experiment baserat på indata från en hallon Pi-enhet.</span><span class="sxs-lookup"><span data-stu-id="a094a-276">toohelp you understand how Stream Analytics jobs scale, we performed an experiment based on input from a Raspberry Pi device.</span></span> <span data-ttu-id="a094a-277">Experimentet Låt oss se hello påverkar genomflöde för flera strömmande enheter och partitioner.</span><span class="sxs-lookup"><span data-stu-id="a094a-277">This experiment let us see hello effect on throughput of multiple streaming units and partitions.</span></span>

<span data-ttu-id="a094a-278">I det här scenariot skickar hello enheten sensor data (klienter) tooan händelsehubb.</span><span class="sxs-lookup"><span data-stu-id="a094a-278">In this scenario, hello device sends sensor data (clients) tooan event hub.</span></span> <span data-ttu-id="a094a-279">Streaming Analytics bearbetar hello data och skickar en avisering eller statistik som en utdata tooanother händelsehubb.</span><span class="sxs-lookup"><span data-stu-id="a094a-279">Streaming Analytics processes hello data and sends an alert or statistics as an output tooanother event hub.</span></span> 

<span data-ttu-id="a094a-280">hello klienten skickar sensordata i JSON-format.</span><span class="sxs-lookup"><span data-stu-id="a094a-280">hello client sends sensor data in JSON format.</span></span> <span data-ttu-id="a094a-281">hello utdata är också i JSON-format.</span><span class="sxs-lookup"><span data-stu-id="a094a-281">hello data output is also in JSON format.</span></span> <span data-ttu-id="a094a-282">hello data ser ut så här:</span><span class="sxs-lookup"><span data-stu-id="a094a-282">hello data looks like this:</span></span>

    {"devicetime":"2014-12-11T02:24:56.8850110Z","hmdt":42.7,"temp":72.6,"prss":98187.75,"lght":0.38,"dspl":"R-PI Olivier's Office"}

<span data-ttu-id="a094a-283">hello följande fråga är används toosend en avisering när en enstaka stängs av:</span><span class="sxs-lookup"><span data-stu-id="a094a-283">hello following query is used toosend an alert when a light is switched off:</span></span>

    SELECT AVG(lght),
     "LightOff" as AlertText
    FROM input TIMESTAMP
    BY devicetime
     WHERE
        lght< 0.05 GROUP BY TumblingWindow(second, 1)

### <a name="measure-throughput"></a><span data-ttu-id="a094a-284">Mäta dataflöde</span><span class="sxs-lookup"><span data-stu-id="a094a-284">Measure throughput</span></span>

<span data-ttu-id="a094a-285">I den här kontexten är genomströmning hello inkommande data som bearbetas av Stream Analytics i en fast mängd tid.</span><span class="sxs-lookup"><span data-stu-id="a094a-285">In this context, throughput is hello amount of input data processed by Stream Analytics in a fixed amount of time.</span></span> <span data-ttu-id="a094a-286">(Vi mätt i 10 minuter.) tooachieve hello bästa bearbetning genomströmning för hello mata in data, både hello dataströmmen indata och hello frågan har partitionerats.</span><span class="sxs-lookup"><span data-stu-id="a094a-286">(We measured for 10 minutes.) tooachieve hello best processing throughput for hello input data, both hello data stream input and hello query were  partitioned.</span></span> <span data-ttu-id="a094a-287">Vi inkluderat **COUNT()** i hello frågan toomeasure hur många inkommande händelser har bearbetats.</span><span class="sxs-lookup"><span data-stu-id="a094a-287">We included **COUNT()** in hello query toomeasure how many input events were processed.</span></span> <span data-ttu-id="a094a-288">toomake att hello jobbet väntar inte på bara inkommande händelser toocome, varje partition för hello inkommande händelsehubb har förinstallerats på 300 MB som indata.</span><span class="sxs-lookup"><span data-stu-id="a094a-288">toomake sure hello job was not simply waiting for input events toocome, each partition of hello input event hub was preloaded with about 300 MB of input data.</span></span>

<span data-ttu-id="a094a-289">hello visar följande tabell hello resultat som vi såg när vi har ökat hello antalet strömningsenheter och hello motsvarande partition räknar i händelsehubbar.</span><span class="sxs-lookup"><span data-stu-id="a094a-289">hello following table shows hello results we saw when we increased hello number of streaming units and hello corresponding partition counts in event hubs.</span></span>  

<table border="1">
<tr><th><span data-ttu-id="a094a-290">Inkommande partitioner</span><span class="sxs-lookup"><span data-stu-id="a094a-290">Input Partitions</span></span></th><th><span data-ttu-id="a094a-291">Utdata partitioner</span><span class="sxs-lookup"><span data-stu-id="a094a-291">Output Partitions</span></span></th><th><span data-ttu-id="a094a-292">Enheter för strömning</span><span class="sxs-lookup"><span data-stu-id="a094a-292">Streaming Units</span></span></th><th><span data-ttu-id="a094a-293">Varaktigt dataflöde</span><span class="sxs-lookup"><span data-stu-id="a094a-293">Sustained Throughput</span></span>
</th></td>

<tr><td><span data-ttu-id="a094a-294">12</span><span class="sxs-lookup"><span data-stu-id="a094a-294">12</span></span></td>
<td><span data-ttu-id="a094a-295">12</span><span class="sxs-lookup"><span data-stu-id="a094a-295">12</span></span></td>
<td><span data-ttu-id="a094a-296">6</span><span class="sxs-lookup"><span data-stu-id="a094a-296">6</span></span></td>
<td><span data-ttu-id="a094a-297">4.06 MB/s</span><span class="sxs-lookup"><span data-stu-id="a094a-297">4.06 MB/s</span></span></td>
</tr>

<tr><td><span data-ttu-id="a094a-298">12</span><span class="sxs-lookup"><span data-stu-id="a094a-298">12</span></span></td>
<td><span data-ttu-id="a094a-299">12</span><span class="sxs-lookup"><span data-stu-id="a094a-299">12</span></span></td>
<td><span data-ttu-id="a094a-300">12</span><span class="sxs-lookup"><span data-stu-id="a094a-300">12</span></span></td>
<td><span data-ttu-id="a094a-301">8.06 MB/s</span><span class="sxs-lookup"><span data-stu-id="a094a-301">8.06 MB/s</span></span></td>
</tr>

<tr><td><span data-ttu-id="a094a-302">48</span><span class="sxs-lookup"><span data-stu-id="a094a-302">48</span></span></td>
<td><span data-ttu-id="a094a-303">48</span><span class="sxs-lookup"><span data-stu-id="a094a-303">48</span></span></td>
<td><span data-ttu-id="a094a-304">48</span><span class="sxs-lookup"><span data-stu-id="a094a-304">48</span></span></td>
<td><span data-ttu-id="a094a-305">38.32 MB/s</span><span class="sxs-lookup"><span data-stu-id="a094a-305">38.32 MB/s</span></span></td>
</tr>

<tr><td><span data-ttu-id="a094a-306">192</span><span class="sxs-lookup"><span data-stu-id="a094a-306">192</span></span></td>
<td><span data-ttu-id="a094a-307">192</span><span class="sxs-lookup"><span data-stu-id="a094a-307">192</span></span></td>
<td><span data-ttu-id="a094a-308">192</span><span class="sxs-lookup"><span data-stu-id="a094a-308">192</span></span></td>
<td><span data-ttu-id="a094a-309">172.67 MB/s</span><span class="sxs-lookup"><span data-stu-id="a094a-309">172.67 MB/s</span></span></td>
</tr>

<tr><td><span data-ttu-id="a094a-310">480</span><span class="sxs-lookup"><span data-stu-id="a094a-310">480</span></span></td>
<td><span data-ttu-id="a094a-311">480</span><span class="sxs-lookup"><span data-stu-id="a094a-311">480</span></span></td>
<td><span data-ttu-id="a094a-312">480</span><span class="sxs-lookup"><span data-stu-id="a094a-312">480</span></span></td>
<td><span data-ttu-id="a094a-313">454.27 MB/s</span><span class="sxs-lookup"><span data-stu-id="a094a-313">454.27 MB/s</span></span></td>
</tr>

<tr><td><span data-ttu-id="a094a-314">720</span><span class="sxs-lookup"><span data-stu-id="a094a-314">720</span></span></td>
<td><span data-ttu-id="a094a-315">720</span><span class="sxs-lookup"><span data-stu-id="a094a-315">720</span></span></td>
<td><span data-ttu-id="a094a-316">720</span><span class="sxs-lookup"><span data-stu-id="a094a-316">720</span></span></td>
<td><span data-ttu-id="a094a-317">609.69 MB/s</span><span class="sxs-lookup"><span data-stu-id="a094a-317">609.69 MB/s</span></span></td>
</tr>
</table>

<span data-ttu-id="a094a-318">Och hello följande diagram visar en visualisering av hello relation mellan SUs och genomflöde.</span><span class="sxs-lookup"><span data-stu-id="a094a-318">And hello following graph shows a visualization of hello relationship between SUs and throughput.</span></span>

![img.stream.Analytics.perfgraph][img.stream.analytics.perfgraph]

## <a name="get-help"></a><span data-ttu-id="a094a-320">Få hjälp</span><span class="sxs-lookup"><span data-stu-id="a094a-320">Get help</span></span>
<span data-ttu-id="a094a-321">För ytterligare hjälp försök vår [Azure Stream Analytics-forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics).</span><span class="sxs-lookup"><span data-stu-id="a094a-321">For further assistance, try our [Azure Stream Analytics forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics).</span></span>

## <a name="next-steps"></a><span data-ttu-id="a094a-322">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="a094a-322">Next steps</span></span>
* [<span data-ttu-id="a094a-323">Introduktion tooAzure Stream Analytics</span><span class="sxs-lookup"><span data-stu-id="a094a-323">Introduction tooAzure Stream Analytics</span></span>](stream-analytics-introduction.md)
* [<span data-ttu-id="a094a-324">Komma igång med Azure Stream Analytics</span><span class="sxs-lookup"><span data-stu-id="a094a-324">Get started using Azure Stream Analytics</span></span>](stream-analytics-real-time-fraud-detection.md)
* [<span data-ttu-id="a094a-325">Referens för Azure Stream Analytics-frågespråket</span><span class="sxs-lookup"><span data-stu-id="a094a-325">Azure Stream Analytics Query Language Reference</span></span>](https://msdn.microsoft.com/library/azure/dn834998.aspx)
* [<span data-ttu-id="a094a-326">Referens för Azure Stream Analytics Management REST API</span><span class="sxs-lookup"><span data-stu-id="a094a-326">Azure Stream Analytics Management REST API Reference</span></span>](https://msdn.microsoft.com/library/azure/dn835031.aspx)

<!--Image references-->

[img.stream.analytics.monitor.job]: ./media/stream-analytics-scale-jobs/StreamAnalytics.job.monitor-NewPortal.png
[img.stream.analytics.configure.scale]: ./media/stream-analytics-scale-jobs/StreamAnalytics.configure.scale.png
[img.stream.analytics.perfgraph]: ./media/stream-analytics-scale-jobs/perf.png
[img.stream.analytics.streaming.units.scale]: ./media/stream-analytics-scale-jobs/StreamAnalyticsStreamingUnitsExample.jpg
[img.stream.analytics.preview.portal.settings.scale]: ./media/stream-analytics-scale-jobs/StreamAnalyticsPreviewPortalJobSettings-NewPortal.png   

<!--Link references-->

[microsoft.support]: http://support.microsoft.com
[azure.management.portal]: http://manage.windowsazure.com
[azure.event.hubs.developer.guide]: http://msdn.microsoft.com/library/azure/dn789972.aspx

[stream.analytics.introduction]: stream-analytics-introduction.md
[stream.analytics.get.started]: stream-analytics-real-time-fraud-detection.md
[stream.analytics.query.language.reference]: http://go.microsoft.com/fwlink/?LinkID=513299
[stream.analytics.rest.api.reference]: http://go.microsoft.com/fwlink/?LinkId=517301

