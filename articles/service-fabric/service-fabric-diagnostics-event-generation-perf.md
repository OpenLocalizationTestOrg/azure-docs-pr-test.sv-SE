---
title: "aaaAzure Service Fabric-prestandaövervakning | Microsoft Docs"
description: "Mer information om prestandaräknare för övervakning och diagnostik av Azure Service Fabric-kluster."
services: service-fabric
documentationcenter: .net
author: dkkapur
manager: timlt
editor: 
ms.assetid: 
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 06/30/2017
ms.author: dekapur
ms.openlocfilehash: 54d4c62b7250a1f70b0898ba07ae5a37716f4cf4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="performance-metrics"></a><span data-ttu-id="7778b-103">Prestandamått</span><span class="sxs-lookup"><span data-stu-id="7778b-103">Performance metrics</span></span>

<span data-ttu-id="7778b-104">Mått måste vara insamlade toounderstand hello prestanda på klustret samt hello-program som körs i den.</span><span class="sxs-lookup"><span data-stu-id="7778b-104">Metrics should be collected toounderstand hello performance of your cluster as well as hello applications running in it.</span></span> <span data-ttu-id="7778b-105">För Service Fabric-kluster rekommenderar vi att samla in hello följande prestandaräknare.</span><span class="sxs-lookup"><span data-stu-id="7778b-105">For Service Fabric clusters, we recommend collecting hello following performance counters.</span></span>

## <a name="nodes"></a><span data-ttu-id="7778b-106">Noder</span><span class="sxs-lookup"><span data-stu-id="7778b-106">Nodes</span></span>

<span data-ttu-id="7778b-107">Överväg att samla in följande prestandaräknare toobetter förstå hello belastningen på varje dator och att rätt kluster skalning beslut hello hello i klustret, datorer.</span><span class="sxs-lookup"><span data-stu-id="7778b-107">For hello machines in your cluster, consider collecting hello following performance counters toobetter understand hello load on each machine and make appropriate cluster scaling decisions.</span></span>

| <span data-ttu-id="7778b-108">Räknaren kategori</span><span class="sxs-lookup"><span data-stu-id="7778b-108">Counter Category</span></span> | <span data-ttu-id="7778b-109">Räknarens namn</span><span class="sxs-lookup"><span data-stu-id="7778b-109">Counter Name</span></span> |
| --- | --- |
| <span data-ttu-id="7778b-110">Fysisk disk (per Disk)</span><span class="sxs-lookup"><span data-stu-id="7778b-110">PhysicalDisk(per Disk)</span></span> | <span data-ttu-id="7778b-111">Genomsn. Läs diskkölängd</span><span class="sxs-lookup"><span data-stu-id="7778b-111">Avg. Disk Read Queue Length</span></span> |
| <span data-ttu-id="7778b-112">Fysisk disk (per Disk)</span><span class="sxs-lookup"><span data-stu-id="7778b-112">PhysicalDisk(per Disk)</span></span> | <span data-ttu-id="7778b-113">Genomsn. Diskkölängd för skrivning</span><span class="sxs-lookup"><span data-stu-id="7778b-113">Avg. Disk Write Queue Length</span></span> |
| <span data-ttu-id="7778b-114">Fysisk disk (per Disk)</span><span class="sxs-lookup"><span data-stu-id="7778b-114">PhysicalDisk(per Disk)</span></span> | <span data-ttu-id="7778b-115">Genomsn. Disk sek/läsning</span><span class="sxs-lookup"><span data-stu-id="7778b-115">Avg. Disk sec/Read</span></span> |
| <span data-ttu-id="7778b-116">Fysisk disk (per Disk)</span><span class="sxs-lookup"><span data-stu-id="7778b-116">PhysicalDisk(per Disk)</span></span> | <span data-ttu-id="7778b-117">Genomsn. Disk sek/skrivning</span><span class="sxs-lookup"><span data-stu-id="7778b-117">Avg. Disk sec/Write</span></span> |
| <span data-ttu-id="7778b-118">Fysisk disk (per Disk)</span><span class="sxs-lookup"><span data-stu-id="7778b-118">PhysicalDisk(per Disk)</span></span> | <span data-ttu-id="7778b-119">Diskläsningar/sek</span><span class="sxs-lookup"><span data-stu-id="7778b-119">Disk Reads/sec</span></span> |
| <span data-ttu-id="7778b-120">Fysisk disk (per Disk)</span><span class="sxs-lookup"><span data-stu-id="7778b-120">PhysicalDisk(per Disk)</span></span> | <span data-ttu-id="7778b-121">Disk-lästa byte/sek</span><span class="sxs-lookup"><span data-stu-id="7778b-121">Disk Read Bytes/sec</span></span> |
| <span data-ttu-id="7778b-122">Fysisk disk (per Disk)</span><span class="sxs-lookup"><span data-stu-id="7778b-122">PhysicalDisk(per Disk)</span></span> | <span data-ttu-id="7778b-123">Diskskrivningar/sek</span><span class="sxs-lookup"><span data-stu-id="7778b-123">Disk Writes/sec</span></span> |
| <span data-ttu-id="7778b-124">Fysisk disk (per Disk)</span><span class="sxs-lookup"><span data-stu-id="7778b-124">PhysicalDisk(per Disk)</span></span> | <span data-ttu-id="7778b-125">Disk-skrivna byte/s</span><span class="sxs-lookup"><span data-stu-id="7778b-125">Disk Write Bytes/sec</span></span> |
| <span data-ttu-id="7778b-126">Minne</span><span class="sxs-lookup"><span data-stu-id="7778b-126">Memory</span></span> | <span data-ttu-id="7778b-127">Tillgängliga megabyte</span><span class="sxs-lookup"><span data-stu-id="7778b-127">Available MBytes</span></span> |
| <span data-ttu-id="7778b-128">Växling fil</span><span class="sxs-lookup"><span data-stu-id="7778b-128">PagingFile</span></span> | <span data-ttu-id="7778b-129">% Användning</span><span class="sxs-lookup"><span data-stu-id="7778b-129">% Usage</span></span> |
| <span data-ttu-id="7778b-130">Process(total)</span><span class="sxs-lookup"><span data-stu-id="7778b-130">Process(Total)</span></span> | <span data-ttu-id="7778b-131">% Processortid</span><span class="sxs-lookup"><span data-stu-id="7778b-131">% Processor Time</span></span> |
| <span data-ttu-id="7778b-132">Processen (per service)</span><span class="sxs-lookup"><span data-stu-id="7778b-132">Process (per service)</span></span> | <span data-ttu-id="7778b-133">% Processortid</span><span class="sxs-lookup"><span data-stu-id="7778b-133">% Processor Time</span></span> |
| <span data-ttu-id="7778b-134">Processen (per service)</span><span class="sxs-lookup"><span data-stu-id="7778b-134">Process (per service)</span></span> | <span data-ttu-id="7778b-135">Process-ID</span><span class="sxs-lookup"><span data-stu-id="7778b-135">ID Process</span></span> |
| <span data-ttu-id="7778b-136">Processen (per service)</span><span class="sxs-lookup"><span data-stu-id="7778b-136">Process (per service)</span></span> | <span data-ttu-id="7778b-137">Privata byte</span><span class="sxs-lookup"><span data-stu-id="7778b-137">Private Bytes</span></span> |
| <span data-ttu-id="7778b-138">Processen (per service)</span><span class="sxs-lookup"><span data-stu-id="7778b-138">Process (per service)</span></span> | <span data-ttu-id="7778b-139">Antal tråd</span><span class="sxs-lookup"><span data-stu-id="7778b-139">Thread Count</span></span> |
| <span data-ttu-id="7778b-140">Processen (per service)</span><span class="sxs-lookup"><span data-stu-id="7778b-140">Process (per service)</span></span> | <span data-ttu-id="7778b-141">Virtuell storlek-byte</span><span class="sxs-lookup"><span data-stu-id="7778b-141">Virtual Bytes</span></span> |
| <span data-ttu-id="7778b-142">Processen (per service)</span><span class="sxs-lookup"><span data-stu-id="7778b-142">Process (per service)</span></span> | <span data-ttu-id="7778b-143">Sidmängd</span><span class="sxs-lookup"><span data-stu-id="7778b-143">Working Set</span></span> |
| <span data-ttu-id="7778b-144">Processen (per service)</span><span class="sxs-lookup"><span data-stu-id="7778b-144">Process (per service)</span></span> | <span data-ttu-id="7778b-145">Privat arbetsminne</span><span class="sxs-lookup"><span data-stu-id="7778b-145">Working Set - Private</span></span> |

## <a name="net-applications-and-services"></a><span data-ttu-id="7778b-146">.NET-program och tjänster</span><span class="sxs-lookup"><span data-stu-id="7778b-146">.NET applications and services</span></span>

<span data-ttu-id="7778b-147">Samla in hello följande räknare om du distribuerar .NET services tooyour klustret.</span><span class="sxs-lookup"><span data-stu-id="7778b-147">Collect hello following counters if you are deploying .NET services tooyour cluster.</span></span> 

| <span data-ttu-id="7778b-148">Räknaren kategori</span><span class="sxs-lookup"><span data-stu-id="7778b-148">Counter Category</span></span> | <span data-ttu-id="7778b-149">Räknarens namn</span><span class="sxs-lookup"><span data-stu-id="7778b-149">Counter Name</span></span> |
| --- | --- |
| <span data-ttu-id="7778b-150">.NET CLR-minne (per service)</span><span class="sxs-lookup"><span data-stu-id="7778b-150">.NET CLR Memory (per service)</span></span> | <span data-ttu-id="7778b-151">Process-ID</span><span class="sxs-lookup"><span data-stu-id="7778b-151">Process ID</span></span> |
| <span data-ttu-id="7778b-152">.NET CLR-minne (per service)</span><span class="sxs-lookup"><span data-stu-id="7778b-152">.NET CLR Memory (per service)</span></span> | <span data-ttu-id="7778b-153"># Totalt antal allokerade byte</span><span class="sxs-lookup"><span data-stu-id="7778b-153"># Total committed Bytes</span></span> |
| <span data-ttu-id="7778b-154">.NET CLR-minne (per service)</span><span class="sxs-lookup"><span data-stu-id="7778b-154">.NET CLR Memory (per service)</span></span> | <span data-ttu-id="7778b-155"># Totalt antal reserverade byte</span><span class="sxs-lookup"><span data-stu-id="7778b-155"># Total reserved Bytes</span></span> |
| <span data-ttu-id="7778b-156">.NET CLR-minne (per service)</span><span class="sxs-lookup"><span data-stu-id="7778b-156">.NET CLR Memory (per service)</span></span> | <span data-ttu-id="7778b-157"># Byte i alla Heaps</span><span class="sxs-lookup"><span data-stu-id="7778b-157"># Bytes in all Heaps</span></span> |
| <span data-ttu-id="7778b-158">.NET CLR-minne (per service)</span><span class="sxs-lookup"><span data-stu-id="7778b-158">.NET CLR Memory (per service)</span></span> | <span data-ttu-id="7778b-159">Antal generation 0-insamlingar</span><span class="sxs-lookup"><span data-stu-id="7778b-159"># Gen 0 Collections</span></span> |
| <span data-ttu-id="7778b-160">.NET CLR-minne (per service)</span><span class="sxs-lookup"><span data-stu-id="7778b-160">.NET CLR Memory (per service)</span></span> | <span data-ttu-id="7778b-161">Antal generation 1-samlingar</span><span class="sxs-lookup"><span data-stu-id="7778b-161"># Gen 1 Collections</span></span> |
| <span data-ttu-id="7778b-162">.NET CLR-minne (per service)</span><span class="sxs-lookup"><span data-stu-id="7778b-162">.NET CLR Memory (per service)</span></span> | <span data-ttu-id="7778b-163"># Generation 2-insamlingar</span><span class="sxs-lookup"><span data-stu-id="7778b-163"># Gen 2 Collections</span></span> |
| <span data-ttu-id="7778b-164">.NET CLR-minne (per service)</span><span class="sxs-lookup"><span data-stu-id="7778b-164">.NET CLR Memory (per service)</span></span> | <span data-ttu-id="7778b-165">Tid i GC i procent</span><span class="sxs-lookup"><span data-stu-id="7778b-165">% Time in GC</span></span> |

### <a name="service-fabrics-custom-performance-counters"></a><span data-ttu-id="7778b-166">Service Fabric anpassade prestandaräknare</span><span class="sxs-lookup"><span data-stu-id="7778b-166">Service Fabric's custom performance counters</span></span>

<span data-ttu-id="7778b-167">Service Fabric genererar en stor mängd anpassade prestandaräknare.</span><span class="sxs-lookup"><span data-stu-id="7778b-167">Service Fabric generates a substantial amount of custom performance counters.</span></span> <span data-ttu-id="7778b-168">Om du har hello SDK är installerat, du kan se hello omfattande lista på din Windows-dator i tillämpningsprogrammet Prestandaövervakaren (Start > Prestandaövervakaren).</span><span class="sxs-lookup"><span data-stu-id="7778b-168">If you have hello SDK installed, you can see hello comprehensive list on your Windows machine in your Performance Monitor application (Start > Performance Monitor).</span></span> 

<span data-ttu-id="7778b-169">Du distribuerar tooyour klustret i hello program, om du använder Reliable Actors, lägger du till countes från `Service Fabric Actor` och `Service Fabric Actor Method` kategorier (se [Service Fabric tillförlitliga aktörer diagnostik](service-fabric-reliable-actors-diagnostics.md)).</span><span class="sxs-lookup"><span data-stu-id="7778b-169">In hello applications you are deploying tooyour cluster, if you are using Reliable Actors, add countes from `Service Fabric Actor` and `Service Fabric Actor Method` categories (see [Service Fabric Reliable Actors Diagnostics](service-fabric-reliable-actors-diagnostics.md)).</span></span>

<span data-ttu-id="7778b-170">Om du använder Reliable Services på samma sätt har vi `Service Fabric Service` och `Service Fabric Service Method` räknaren kategorier som du bör samla in prestandaräknare från.</span><span class="sxs-lookup"><span data-stu-id="7778b-170">If you use Reliable Services, we similarly have `Service Fabric Service` and `Service Fabric Service Method` counter categories that you should collect counters from.</span></span> 

<span data-ttu-id="7778b-171">Om du använder tillförlitliga samlingar, rekommenderar vi att lägga till hello `Avg. Transaction ms/Commit` från hello `Service Fabric Transactional Replicator` toocollect hello genomsnittlig commit latens per transaktion mått.</span><span class="sxs-lookup"><span data-stu-id="7778b-171">If you use Reliable Collections, we recommend adding hello `Avg. Transaction ms/Commit` from hello `Service Fabric Transactional Replicator` toocollect hello average commit latency per transaction metric.</span></span>


## <a name="next-steps"></a><span data-ttu-id="7778b-172">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="7778b-172">Next steps</span></span>

* <span data-ttu-id="7778b-173">Lär dig mer om [händelse genereras på hello plattformsnivå](service-fabric-diagnostics-event-generation-infra.md) i Service Fabric</span><span class="sxs-lookup"><span data-stu-id="7778b-173">Learn more about [event generation at hello platform level](service-fabric-diagnostics-event-generation-infra.md) in Service Fabric</span></span>
* <span data-ttu-id="7778b-174">Samla in prestandavärden via [Azure-diagnostik](service-fabric-diagnostics-event-aggregation-wad.md)</span><span class="sxs-lookup"><span data-stu-id="7778b-174">Collect performance metrics through [Azure Diagnostics](service-fabric-diagnostics-event-aggregation-wad.md)</span></span>
