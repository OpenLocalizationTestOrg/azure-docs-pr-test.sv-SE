---
title: ReliableConcurrentQueue i Azure Service Fabric
description: "ReliableConcurrentQueue är en hög genomströmning kö som tillåter parallella enqueues och dequeues."
services: service-fabric
documentationcenter: .net
author: sangarg
manager: timlt
editor: raja,tyadam,masnider,vturecek
ms.assetid: 62857523-604b-434e-bd1c-2141ea4b00d1
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: required
ms.date: 5/1/2017
ms.author: sangarg
ms.openlocfilehash: 122cb48149477f295a65b8ee623c647b6db10a86
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="introduction-to-reliableconcurrentqueue-in-azure-service-fabric"></a><span data-ttu-id="0df4f-103">Introduktion till ReliableConcurrentQueue i Azure Service Fabric</span><span class="sxs-lookup"><span data-stu-id="0df4f-103">Introduction to ReliableConcurrentQueue in Azure Service Fabric</span></span>
<span data-ttu-id="0df4f-104">Tillförlitliga samtidiga kön är en asynkron transaktionell och replikerade kö vilka funktioner hög samtidighet för sätta och åtgärder som har status Created.</span><span class="sxs-lookup"><span data-stu-id="0df4f-104">Reliable Concurrent Queue is an asynchronous, transactional, and replicated queue which features high concurrency for enqueue and dequeue operations.</span></span> <span data-ttu-id="0df4f-105">Den är utformad för att leverera högt genomflöde och låg fördröjning av lugnt strikt FIFO ordningen som tillhandahålls av [tillförlitliga kön](https://msdn.microsoft.com/library/azure/dn971527.aspx) och i stället tillhandahåller en bästa sortering.</span><span class="sxs-lookup"><span data-stu-id="0df4f-105">It is designed to deliver high throughput and low latency by relaxing the strict FIFO ordering provided by [Reliable Queue](https://msdn.microsoft.com/library/azure/dn971527.aspx) and instead provides a best-effort ordering.</span></span>

## <a name="apis"></a><span data-ttu-id="0df4f-106">API:er</span><span class="sxs-lookup"><span data-stu-id="0df4f-106">APIs</span></span>

|<span data-ttu-id="0df4f-107">Samtidiga kön</span><span class="sxs-lookup"><span data-stu-id="0df4f-107">Concurrent Queue</span></span>                |<span data-ttu-id="0df4f-108">Tillförlitliga samtidiga kön</span><span class="sxs-lookup"><span data-stu-id="0df4f-108">Reliable Concurrent Queue</span></span>                                         |
|--------------------------------|------------------------------------------------------------------|
| <span data-ttu-id="0df4f-109">void Enqueue(T item)</span><span class="sxs-lookup"><span data-stu-id="0df4f-109">void Enqueue(T item)</span></span>           | <span data-ttu-id="0df4f-110">Uppgiften EnqueueAsync (ITransaction tx T objekt)</span><span class="sxs-lookup"><span data-stu-id="0df4f-110">Task EnqueueAsync(ITransaction tx, T item)</span></span>                       |
| <span data-ttu-id="0df4f-111">bool TryDequeue (ut T resultat)</span><span class="sxs-lookup"><span data-stu-id="0df4f-111">bool TryDequeue(out T result)</span></span>  | <span data-ttu-id="0df4f-112">Uppgiften < ConditionalValue < T >> TryDequeueAsync (tx ITransaction)</span><span class="sxs-lookup"><span data-stu-id="0df4f-112">Task< ConditionalValue < T > > TryDequeueAsync(ITransaction tx)</span></span>  |
| <span data-ttu-id="0df4f-113">int Count()</span><span class="sxs-lookup"><span data-stu-id="0df4f-113">int Count()</span></span>                    | <span data-ttu-id="0df4f-114">lång Count()</span><span class="sxs-lookup"><span data-stu-id="0df4f-114">long Count()</span></span>                                                     |

## <a name="comparison-with-reliable-queuehttpsmsdnmicrosoftcomlibraryazuredn971527aspx"></a><span data-ttu-id="0df4f-115">Jämförelse med [tillförlitliga kön](https://msdn.microsoft.com/library/azure/dn971527.aspx)</span><span class="sxs-lookup"><span data-stu-id="0df4f-115">Comparison with [Reliable Queue](https://msdn.microsoft.com/library/azure/dn971527.aspx)</span></span>

<span data-ttu-id="0df4f-116">Tillförlitliga samtidiga kön erbjuds som ett alternativ till [tillförlitliga kön](https://msdn.microsoft.com/library/azure/dn971527.aspx).</span><span class="sxs-lookup"><span data-stu-id="0df4f-116">Reliable Concurrent Queue is offered as an alternative to [Reliable Queue](https://msdn.microsoft.com/library/azure/dn971527.aspx).</span></span> <span data-ttu-id="0df4f-117">Det ska användas i fall där strikt FIFO ordning inte krävs, som garanterar FIFO kräver en kompromiss med samtidighet.</span><span class="sxs-lookup"><span data-stu-id="0df4f-117">It should be used in cases where strict FIFO ordering is not required, as guaranteeing FIFO requires a tradeoff with concurrency.</span></span>  <span data-ttu-id="0df4f-118">[Tillförlitliga kön](https://msdn.microsoft.com/library/azure/dn971527.aspx) använder lås för att genomdriva FIFO ordning med maximalt en transaktion tillåts att köa och maximalt en transaktion får status Created i taget.</span><span class="sxs-lookup"><span data-stu-id="0df4f-118">[Reliable Queue](https://msdn.microsoft.com/library/azure/dn971527.aspx) uses locks to enforce FIFO ordering, with at most one transaction allowed to enqueue and at most one transaction allowed to dequeue at a time.</span></span> <span data-ttu-id="0df4f-119">Jämförelse tillförlitliga samtidiga kön sänker onlinebeställning begränsningen och gör att alla antalet samtidiga transaktioner interleave sina sätta och åtgärder som har status Created.</span><span class="sxs-lookup"><span data-stu-id="0df4f-119">In comparison, Reliable Concurrent Queue relaxes the ordering constraint and allows any number concurrent transactions to interleave their enqueue and dequeue operations.</span></span> <span data-ttu-id="0df4f-120">Bästa ordning har angetts men den relativa sorteringen av två värden i en tillförlitlig samtidiga kö kan inte garanteras.</span><span class="sxs-lookup"><span data-stu-id="0df4f-120">Best-effort ordering is provided, however the relative ordering of two values in a Reliable Concurrent Queue can never be guaranteed.</span></span>

<span data-ttu-id="0df4f-121">Tillförlitliga samtidiga kön ger högre genomflöde och kortare svarstid än [tillförlitliga kö](https://msdn.microsoft.com/library/azure/dn971527.aspx) när det finns flera samtidiga transaktioner som utför enqueues och/eller dequeues.</span><span class="sxs-lookup"><span data-stu-id="0df4f-121">Reliable Concurrent Queue provides higher throughput and lower latency than [Reliable Queue](https://msdn.microsoft.com/library/azure/dn971527.aspx) whenever there are multiple concurrent transactions performing enqueues and/or dequeues.</span></span>

<span data-ttu-id="0df4f-122">Ett exempel på en användningsfall för ReliableConcurrentQueue är den [meddelandekö](https://en.wikipedia.org/wiki/Message_queue) scenario.</span><span class="sxs-lookup"><span data-stu-id="0df4f-122">A sample use case for the ReliableConcurrentQueue is the [Message Queue](https://en.wikipedia.org/wiki/Message_queue) scenario.</span></span> <span data-ttu-id="0df4f-123">I det här scenariot en eller flera meddelandeproducenter skapa och Lägg till objekt i kön och en eller flera meddelandet konsumenter pull-meddelanden från kön och bearbeta dem..</span><span class="sxs-lookup"><span data-stu-id="0df4f-123">In this scenario, one or more message producers create and add items to the queue, and one or more message consumers pull messages from the queue and process them.</span></span> <span data-ttu-id="0df4f-124">Flera producenter och konsumenter fungerar oberoende av varandra, med samtidiga transaktioner ska kunna bearbeta kön.</span><span class="sxs-lookup"><span data-stu-id="0df4f-124">Multiple producers and consumers can work independently, using concurrent transactions in order to process the queue.</span></span>

## <a name="usage-guidelines"></a><span data-ttu-id="0df4f-125">Riktlinjer för användning</span><span class="sxs-lookup"><span data-stu-id="0df4f-125">Usage Guidelines</span></span>
* <span data-ttu-id="0df4f-126">Kön förväntar sig att objekten i kön har en låg Bevarandeperiod.</span><span class="sxs-lookup"><span data-stu-id="0df4f-126">The queue expects that the items in the queue have a low retention period.</span></span> <span data-ttu-id="0df4f-127">Det vill säga skulle objekt inte kvar i kön för lång tid.</span><span class="sxs-lookup"><span data-stu-id="0df4f-127">That is, the items would not stay in the queue for a long time.</span></span>
* <span data-ttu-id="0df4f-128">Kön garanterar inte strikt FIFO ordning.</span><span class="sxs-lookup"><span data-stu-id="0df4f-128">The queue does not guarantee strict FIFO ordering.</span></span>
* <span data-ttu-id="0df4f-129">Kön läser inte sin egen skrivningar.</span><span class="sxs-lookup"><span data-stu-id="0df4f-129">The queue does not read its own writes.</span></span> <span data-ttu-id="0df4f-130">Om ett objekt i kö i en transaktion, kommer den inte vara synliga för en dequeuer inom samma transaktion.</span><span class="sxs-lookup"><span data-stu-id="0df4f-130">If an item is enqueued within a transaction, it will not be visible to a dequeuer within the same transaction.</span></span>
* <span data-ttu-id="0df4f-131">Dequeues inte är isolerade från varandra.</span><span class="sxs-lookup"><span data-stu-id="0df4f-131">Dequeues are not isolated from each other.</span></span> <span data-ttu-id="0df4f-132">Om objektet *A* har tagits bort i transaktion *txnA*, även om *txnA* är därmed inte verkställas objektet *A* skulle inte till en samtidig transaktion *txnB*.</span><span class="sxs-lookup"><span data-stu-id="0df4f-132">If item *A* is dequeued in transaction *txnA*, even though *txnA* is not committed, item *A* would not be visible to a concurrent transaction *txnB*.</span></span>  <span data-ttu-id="0df4f-133">Om *txnA* avbryts, *A* ska vara synlig för *txnB* omedelbart.</span><span class="sxs-lookup"><span data-stu-id="0df4f-133">If *txnA* aborts, *A* will become visible to *txnB* immediately.</span></span>
* <span data-ttu-id="0df4f-134">*TryPeekAsync* beteende kan implementeras med hjälp av en *TryDequeueAsync* och avbryter transaktionen.</span><span class="sxs-lookup"><span data-stu-id="0df4f-134">*TryPeekAsync* behavior can be implemented by using a *TryDequeueAsync* and then aborting the transaction.</span></span> <span data-ttu-id="0df4f-135">Ett exempel på detta finns i avsnittet Programming mönster.</span><span class="sxs-lookup"><span data-stu-id="0df4f-135">An example of this can be found in the Programming Patterns section.</span></span>
* <span data-ttu-id="0df4f-136">Antalet är icke-transaktionell.</span><span class="sxs-lookup"><span data-stu-id="0df4f-136">Count is non-transactional.</span></span> <span data-ttu-id="0df4f-137">Den kan användas för att få en uppfattning om antalet element i kön, men representerar point-in-time och kan inte förlita sig på.</span><span class="sxs-lookup"><span data-stu-id="0df4f-137">It can be used to get an idea of the number of elements in the queue, but represents a point-in-time and cannot be relied upon.</span></span>
* <span data-ttu-id="0df4f-138">Billigare bearbetning på dequeued objekt ska inte utföras medan transaktionen är aktiv för att undvika långvariga transaktioner som kan påverka prestanda på datorn.</span><span class="sxs-lookup"><span data-stu-id="0df4f-138">Expensive processing on the dequeued items should not be performed while the transaction is active, to avoid long-running transactions which may have a performance impact on the system.</span></span>

## <a name="code-snippets"></a><span data-ttu-id="0df4f-139">Kodstycken</span><span class="sxs-lookup"><span data-stu-id="0df4f-139">Code Snippets</span></span>
<span data-ttu-id="0df4f-140">Låt oss titta på några kodstycken och deras förväntade produktion.</span><span class="sxs-lookup"><span data-stu-id="0df4f-140">Let us look at a few code snippets and their expected outputs.</span></span> <span data-ttu-id="0df4f-141">Undantagshantering ignoreras i det här avsnittet.</span><span class="sxs-lookup"><span data-stu-id="0df4f-141">Exception handling is ignored in this section.</span></span>

### <a name="enqueueasync"></a><span data-ttu-id="0df4f-142">EnqueueAsync</span><span class="sxs-lookup"><span data-stu-id="0df4f-142">EnqueueAsync</span></span>
<span data-ttu-id="0df4f-143">Här följer några kodstycken för att använda EnqueueAsync följt av de förväntade utdata.</span><span class="sxs-lookup"><span data-stu-id="0df4f-143">Here are a few code snippets for using EnqueueAsync followed by their expected outputs.</span></span>

- <span data-ttu-id="0df4f-144">*Fall 1: Enkel köa aktiviteten*</span><span class="sxs-lookup"><span data-stu-id="0df4f-144">*Case 1: Single Enqueue Task*</span></span>

```
using (var txn = this.StateManager.CreateTransaction())
{
    await this.Queue.EnqueueAsync(txn, 10, cancellationToken);
    await this.Queue.EnqueueAsync(txn, 20, cancellationToken);

    await txn.CommitAsync();
}
```

<span data-ttu-id="0df4f-145">Anta att aktiviteten har slutförts och att det finns inga samtidiga transaktioner ändra kön.</span><span class="sxs-lookup"><span data-stu-id="0df4f-145">Assume that the task completed successfully, and that there were no concurrent transactions modifying the queue.</span></span> <span data-ttu-id="0df4f-146">Användaren kan förvänta sig kön att innehålla objekt i något av följande ordning:</span><span class="sxs-lookup"><span data-stu-id="0df4f-146">The user can expect the queue to contain the items in any of the following orders:</span></span>

> <span data-ttu-id="0df4f-147">10, 20</span><span class="sxs-lookup"><span data-stu-id="0df4f-147">10, 20</span></span>

> <span data-ttu-id="0df4f-148">20, 10</span><span class="sxs-lookup"><span data-stu-id="0df4f-148">20, 10</span></span>


- <span data-ttu-id="0df4f-149">*Fall 2: Parallell köa aktiviteten*</span><span class="sxs-lookup"><span data-stu-id="0df4f-149">*Case 2: Parallel Enqueue Task*</span></span>

```
// Parallel Task 1
using (var txn = this.StateManager.CreateTransaction())
{
    await this.Queue.EnqueueAsync(txn, 10, cancellationToken);
    await this.Queue.EnqueueAsync(txn, 20, cancellationToken);

    await txn.CommitAsync();
}

// Parallel Task 2
using (var txn = this.StateManager.CreateTransaction())
{
    await this.Queue.EnqueueAsync(txn, 30, cancellationToken);
    await this.Queue.EnqueueAsync(txn, 40, cancellationToken);

    await txn.CommitAsync();
}
```

<span data-ttu-id="0df4f-150">Anta att aktiviteterna har slutförts, att aktiviteterna har körts parallellt och att det inte finns några andra samtidiga transaktioner ändra kön.</span><span class="sxs-lookup"><span data-stu-id="0df4f-150">Assume that the tasks completed successfully, that the tasks ran in parallel, and that there were no other concurrent transactions modifying the queue.</span></span> <span data-ttu-id="0df4f-151">Inga härledning kan göras om ordningen på objekten i kön.</span><span class="sxs-lookup"><span data-stu-id="0df4f-151">No inference can be made about the order of items in the queue.</span></span> <span data-ttu-id="0df4f-152">Objekten kan visas i någon av 4 för det här kodstycket!</span><span class="sxs-lookup"><span data-stu-id="0df4f-152">For this code snippet, the items may appear in any of the 4!</span></span> <span data-ttu-id="0df4f-153">möjliga ordningar.</span><span class="sxs-lookup"><span data-stu-id="0df4f-153">possible orderings.</span></span>  <span data-ttu-id="0df4f-154">Kön försöker placera objekten i ordningen de ursprungliga (köas), men kan tvingas att ändra ordning på dem på grund av samtidiga åtgärder eller fel.</span><span class="sxs-lookup"><span data-stu-id="0df4f-154">The queue will attempt to keep the items in the original (enqueued) order, but may be forced to reorder them due to concurrent operations or faults.</span></span>


### <a name="dequeueasync"></a><span data-ttu-id="0df4f-155">DequeueAsync</span><span class="sxs-lookup"><span data-stu-id="0df4f-155">DequeueAsync</span></span>
<span data-ttu-id="0df4f-156">Här följer några kodstycken för att använda TryDequeueAsync följt av de förväntade utdata.</span><span class="sxs-lookup"><span data-stu-id="0df4f-156">Here are a few code snippets for using TryDequeueAsync followed by the expected outputs.</span></span> <span data-ttu-id="0df4f-157">Anta att kön redan fylls med följande objekt i kö:</span><span class="sxs-lookup"><span data-stu-id="0df4f-157">Assume that the queue is already populated with the following items in the queue:</span></span>
> <span data-ttu-id="0df4f-158">10, 20, 30, 40, 50, 60</span><span class="sxs-lookup"><span data-stu-id="0df4f-158">10, 20, 30, 40, 50, 60</span></span>

- <span data-ttu-id="0df4f-159">*Fall 1: En har status Created aktivitet*</span><span class="sxs-lookup"><span data-stu-id="0df4f-159">*Case 1: Single Dequeue Task*</span></span>

```
using (var txn = this.StateManager.CreateTransaction())
{
    await this.Queue.TryDequeueAsync(txn, cancellationToken);
    await this.Queue.TryDequeueAsync(txn, cancellationToken);
    await this.Queue.TryDequeueAsync(txn, cancellationToken);

    await txn.CommitAsync();
}
```

<span data-ttu-id="0df4f-160">Anta att aktiviteten har slutförts och att det finns inga samtidiga transaktioner ändra kön.</span><span class="sxs-lookup"><span data-stu-id="0df4f-160">Assume that the task completed successfully, and that there were no concurrent transactions modifying the queue.</span></span> <span data-ttu-id="0df4f-161">Eftersom ingen härledning kan göras om i vilken ordning av objekten i kön måste alla tre objekt kan vara togs bort från kön, i valfri ordning.</span><span class="sxs-lookup"><span data-stu-id="0df4f-161">Since no inference can be made about the order of the items in the queue, any three of the items may be dequeued, in any order.</span></span> <span data-ttu-id="0df4f-162">Kön försöker placera objekten i ordningen de ursprungliga (köas), men kan tvingas att ändra ordning på dem på grund av samtidiga åtgärder eller fel.</span><span class="sxs-lookup"><span data-stu-id="0df4f-162">The queue will attempt to keep the items in the original (enqueued) order, but may be forced to reorder them due to concurrent operations or faults.</span></span>  

- <span data-ttu-id="0df4f-163">*Fall 2: Parallell status Created aktivitet*</span><span class="sxs-lookup"><span data-stu-id="0df4f-163">*Case 2: Parallel Dequeue Task*</span></span>

```
// Parallel Task 1
List<int> dequeue1;
using (var txn = this.StateManager.CreateTransaction())
{
    dequeue1.Add(await this.Queue.TryDequeueAsync(txn, cancellationToken)).val;
    dequeue1.Add(await this.Queue.TryDequeueAsync(txn, cancellationToken)).val;

    await txn.CommitAsync();
}

// Parallel Task 2
List<int> dequeue2;
using (var txn = this.StateManager.CreateTransaction())
{
    dequeue2.Add(await this.Queue.TryDequeueAsync(txn, cancellationToken)).val;
    dequeue2.Add(await this.Queue.TryDequeueAsync(txn, cancellationToken)).val;

    await txn.CommitAsync();
}
```

<span data-ttu-id="0df4f-164">Anta att aktiviteterna har slutförts, att aktiviteterna har körts parallellt och att det inte finns några andra samtidiga transaktioner ändra kön.</span><span class="sxs-lookup"><span data-stu-id="0df4f-164">Assume that the tasks completed successfully, that the tasks ran in parallel, and that there were no other concurrent transactions modifying the queue.</span></span> <span data-ttu-id="0df4f-165">Eftersom ingen härledning kan göras om i vilken ordning av objekten i kön, listorna *dequeue1* och *dequeue2* varje innehåller två objekt, i vilken ordning som helst.</span><span class="sxs-lookup"><span data-stu-id="0df4f-165">Since no inference can be made about the order of the items in the queue, the lists *dequeue1* and *dequeue2* will each contain any two items, in any order.</span></span>

<span data-ttu-id="0df4f-166">Samma objektet kommer *inte* visas i båda listorna.</span><span class="sxs-lookup"><span data-stu-id="0df4f-166">The same item will *not* appear in both lists.</span></span> <span data-ttu-id="0df4f-167">Därför om dequeue1 har *10*, *30*, dequeue2 skulle ha *20*, *40*.</span><span class="sxs-lookup"><span data-stu-id="0df4f-167">Hence, if dequeue1 has *10*, *30*, then dequeue2 would have *20*, *40*.</span></span>

- <span data-ttu-id="0df4f-168">*Fall 3: Status Created ordning med Transaktionsavbrott*</span><span class="sxs-lookup"><span data-stu-id="0df4f-168">*Case 3: Dequeue Ordering With Transaction Abort*</span></span>

<span data-ttu-id="0df4f-169">Avbryter en transaktion med pågående dequeues placeringar objekten tillbaka på chefen för kön.</span><span class="sxs-lookup"><span data-stu-id="0df4f-169">Aborting a transaction with in-flight dequeues puts the items back on the head of the queue.</span></span> <span data-ttu-id="0df4f-170">Den ordning som objekten är lägga tillbaka i toppen av kön är inte säkert.</span><span class="sxs-lookup"><span data-stu-id="0df4f-170">The order in which the items are put back on the head of the queue is not guaranteed.</span></span> <span data-ttu-id="0df4f-171">Låt oss titta på följande kod:</span><span class="sxs-lookup"><span data-stu-id="0df4f-171">Let us look at the following code:</span></span>

```
using (var txn = this.StateManager.CreateTransaction())
{
    await this.Queue.TryDequeueAsync(txn, cancellationToken);
    await this.Queue.TryDequeueAsync(txn, cancellationToken);

    // Abort the transaction
    await txn.AbortAsync();
}
```
<span data-ttu-id="0df4f-172">Anta att objekten har tagits bort i följande ordning:</span><span class="sxs-lookup"><span data-stu-id="0df4f-172">Assume that the items were dequeued in the following order:</span></span>
> <span data-ttu-id="0df4f-173">10, 20</span><span class="sxs-lookup"><span data-stu-id="0df4f-173">10, 20</span></span>

<span data-ttu-id="0df4f-174">När vi avbryter transaktionen skulle objekten läggas till chefen för kön i något av följande ordning:</span><span class="sxs-lookup"><span data-stu-id="0df4f-174">When we abort the transaction, the items would be added back to the head of the queue in any of the following orders:</span></span>
> <span data-ttu-id="0df4f-175">10, 20</span><span class="sxs-lookup"><span data-stu-id="0df4f-175">10, 20</span></span>

> <span data-ttu-id="0df4f-176">20, 10</span><span class="sxs-lookup"><span data-stu-id="0df4f-176">20, 10</span></span>

<span data-ttu-id="0df4f-177">Detsamma gäller för alla fall där transaktionen inte har *genomförd*.</span><span class="sxs-lookup"><span data-stu-id="0df4f-177">The same is true for all cases where the transaction was not successfully *Committed*.</span></span>

## <a name="programming-patterns"></a><span data-ttu-id="0df4f-178">Mönster för programmering</span><span class="sxs-lookup"><span data-stu-id="0df4f-178">Programming Patterns</span></span>
<span data-ttu-id="0df4f-179">I det här avsnittet tittar vi på några programmering mönster som kan vara användbart i med hjälp av ReliableConcurrentQueue.</span><span class="sxs-lookup"><span data-stu-id="0df4f-179">In this section, let us look at a few programming patterns that might be helpful in using ReliableConcurrentQueue.</span></span>

### <a name="batch-dequeues"></a><span data-ttu-id="0df4f-180">Batch Dequeues</span><span class="sxs-lookup"><span data-stu-id="0df4f-180">Batch Dequeues</span></span>
<span data-ttu-id="0df4f-181">A rekommenderas programming mönstret är för konsumenten uppgiften att batchen dess dequeues i stället för att utföra en i taget har status Created.</span><span class="sxs-lookup"><span data-stu-id="0df4f-181">A recommended programming pattern is for the consumer task to batch its dequeues instead of performing one dequeue at a time.</span></span> <span data-ttu-id="0df4f-182">Användaren kan välja att begränsa fördröjningar mellan varje batch eller batchstorleken.</span><span class="sxs-lookup"><span data-stu-id="0df4f-182">The user can choose to throttle delays between every batch or the batch size.</span></span> <span data-ttu-id="0df4f-183">Följande kodavsnitt visar den här programmeringsmodell.</span><span class="sxs-lookup"><span data-stu-id="0df4f-183">The following code snippet shows this programming model.</span></span>  <span data-ttu-id="0df4f-184">Observera att i det här exemplet bearbetning görs när transaktionen är genomförd, så om ett fel uppstår under bearbetningen av, de obehandlade artiklarna försvinner utan har bearbetats.</span><span class="sxs-lookup"><span data-stu-id="0df4f-184">Note that in this example, the processing is done after the transaction is committed, so if a fault were to occur while processing, the unprocessed items will be lost without having been processed.</span></span>  <span data-ttu-id="0df4f-185">Bearbetningen kan också ske inom transaktionsomfånget, men detta kan ha en negativ inverkan på prestanda och kräver hantering av de objekt som redan har bearbetats.</span><span class="sxs-lookup"><span data-stu-id="0df4f-185">Alternatively, the processing can be done within the transaction's scope, however this may have a negative impact on performance and requires handling of the items already processed.</span></span>

```
int batchSize = 5;
long delayMs = 100;

while(!cancellationToken.IsCancellationRequested)
{
    // Buffer for dequeued items
    List<int> processItems = new List<int>();

    using (var txn = this.StateManager.CreateTransaction())
    {
        ConditionalValue<int> ret;

        for(int i = 0; i < batchSize; ++i)
        {
            ret = await this.Queue.TryDequeueAsync(txn, cancellationToken);

            if (ret.HasValue)
            {
                // If an item was dequeued, add to the buffer for processing
                processItems.Add(ret.Value);
            }
            else
            {
                // else break the for loop
                break;
            }
        }

        await txn.CommitAsync();
    }

    // Process the dequeues
    for (int i = 0; i < processItems.Count; ++i)
    {
        Console.WriteLine("Value : " + processItems[i]);
    }

    int delayFactor = batchSize - processItems.Count;
    await Task.Delay(TimeSpan.FromMilliseconds(delayMs * delayFactor), cancellationToken);
}
```

### <a name="best-effort-notification-based-processing"></a><span data-ttu-id="0df4f-186">Bästa meddelandebaserad bearbetning</span><span class="sxs-lookup"><span data-stu-id="0df4f-186">Best-Effort Notification-Based Processing</span></span>
<span data-ttu-id="0df4f-187">En annan intressant programming mönster använder Count-API.</span><span class="sxs-lookup"><span data-stu-id="0df4f-187">Another interesting programming pattern uses the Count API.</span></span> <span data-ttu-id="0df4f-188">Här kan vi implementera bästa meddelandebaserad bearbetning för kön.</span><span class="sxs-lookup"><span data-stu-id="0df4f-188">Here, we can implement best-effort notification-based processing for the queue.</span></span> <span data-ttu-id="0df4f-189">Kön antal kan användas för att begränsa en sätta eller en dequeue aktivitet.</span><span class="sxs-lookup"><span data-stu-id="0df4f-189">The queue Count can be used to throttle an enqueue or a dequeue task.</span></span>  <span data-ttu-id="0df4f-190">Observera att som i föregående exempel, eftersom sker bearbetningen utanför transaktionen, obearbetat objekt kan gå förlorade om ett fel uppstår under bearbetningen.</span><span class="sxs-lookup"><span data-stu-id="0df4f-190">Note that as in the previous example, since the processing occurs outside the transaction, unprocessed items may be lost if a fault occurs during processing.</span></span>

```
int threshold = 5;
long delayMs = 1000;

while(!cancellationToken.IsCancellationRequested)
{
    while (this.Queue.Count < threshold)
    {
        cancellationToken.ThrowIfCancellationRequested();

        // If the queue does not have the threshold number of items, delay the task and check again
        await Task.Delay(TimeSpan.FromMilliseconds(delayMs), cancellationToken);
    }

    // If there are approximately threshold number of items, try and process the queue

    // Buffer for dequeued items
    List<int> processItems = new List<int>();

    using (var txn = this.StateManager.CreateTransaction())
    {
        ConditionalValue<int> ret;

        do
        {
            ret = await this.Queue.TryDequeueAsync(txn, cancellationToken);

            if (ret.HasValue)
            {
                // If an item was dequeued, add to the buffer for processing
                processItems.Add(ret.Value);
            }
        } while (processItems.Count < threshold && ret.HasValue);

        await txn.CommitAsync();
    }

    // Process the dequeues
    for (int i = 0; i < processItems.Count; ++i)
    {
        Console.WriteLine("Value : " + processItems[i]);
    }
}
```

### <a name="best-effort-drain"></a><span data-ttu-id="0df4f-191">Bästa tömning</span><span class="sxs-lookup"><span data-stu-id="0df4f-191">Best-Effort Drain</span></span>
<span data-ttu-id="0df4f-192">Tömning av kön kan inte garanteras på grund av datastrukturen samtidiga.</span><span class="sxs-lookup"><span data-stu-id="0df4f-192">A drain of the queue cannot be guaranteed due to the concurrent nature of the data structure.</span></span>  <span data-ttu-id="0df4f-193">Det är möjligt att även om inga användare åtgärder på kön finns relä, ett visst anrop till TryDequeueAsync inte kan returnera ett objekt som tidigare var köas och bekräftats.</span><span class="sxs-lookup"><span data-stu-id="0df4f-193">It is possible that, even if no user operations on the queue are in-flight, a particular call to TryDequeueAsync may not return an item which was previously enqueued and committed.</span></span>  <span data-ttu-id="0df4f-194">Objektet köas garanterat *slutligen* bli synlig för status Created, men utan en mekanism för out-of-band-kommunikation, ett oberoende konsumenten kan inte vet att kön har uppnått ett stabilt tillstånd även om alla tillverkare har stoppats och inga nya sätta tillåts.</span><span class="sxs-lookup"><span data-stu-id="0df4f-194">The enqueued item is guaranteed to *eventually* become visible to dequeue, however without an out-of-band communication mechanism, an independent consumer cannot know that the queue has reached a steady-state even if all producers have been stopped and no new enqueue operations are allowed.</span></span> <span data-ttu-id="0df4f-195">Därför är åtgärden tömning bästa som implementeras nedan.</span><span class="sxs-lookup"><span data-stu-id="0df4f-195">Thus, the drain operation is best-effort as implemented below.</span></span>

<span data-ttu-id="0df4f-196">Användaren måste stoppa alla ytterligare producenten och konsumentuppgifter och vänta tills alla pågående transaktioner att avbrytas eller genomföras innan du försöker att tömma kön.</span><span class="sxs-lookup"><span data-stu-id="0df4f-196">The user should stop all further producer and consumer tasks, and wait for any in-flight transactions to commit or abort, before attempting to drain the queue.</span></span>  <span data-ttu-id="0df4f-197">Om användaren känner det förväntade antalet objekt i kön, kan de ställa in ett meddelande som signalerar till att alla objekt har har tagits bort.</span><span class="sxs-lookup"><span data-stu-id="0df4f-197">If the user knows the expected number of items in the queue, they can set up a notification which signals that all items have been dequeued.</span></span>

```
int numItemsDequeued;
int batchSize = 5;

ConditionalValue ret;

do
{
    List<int> processItems = new List<int>();

    using (var txn = this.StateManager.CreateTransaction())
    {
        do
        {
            ret = await this.Queue.TryDequeueAsync(txn, cancellationToken);

            if(ret.HasValue)
            {
                // Buffer the dequeues
                processItems.Add(ret.Value);
            }
        } while (ret.HasValue && processItems.Count < batchSize);

        await txn.CommitAsync();
    }

    // Process the dequeues
    for (int i = 0; i < processItems.Count; ++i)
    {
        Console.WriteLine("Value : " + processItems[i]);
    }
} while (ret.HasValue);
```

### <a name="peek"></a><span data-ttu-id="0df4f-198">Granska</span><span class="sxs-lookup"><span data-stu-id="0df4f-198">Peek</span></span>
<span data-ttu-id="0df4f-199">ReliableConcurrentQueue innehåller inte den *TryPeekAsync* api.</span><span class="sxs-lookup"><span data-stu-id="0df4f-199">ReliableConcurrentQueue does not provide the *TryPeekAsync* api.</span></span> <span data-ttu-id="0df4f-200">Användare kan hämta titt semantiska med en *TryDequeueAsync* och avbryter transaktionen.</span><span class="sxs-lookup"><span data-stu-id="0df4f-200">Users can get the peek semantic by using a *TryDequeueAsync* and then aborting the transaction.</span></span> <span data-ttu-id="0df4f-201">I det här exemplet dequeues bearbetas bara om objektets värde är större än *10*.</span><span class="sxs-lookup"><span data-stu-id="0df4f-201">In this example, dequeues are processed only if the item's value is greater than *10*.</span></span>

```
using (var txn = this.StateManager.CreateTransaction())
{
    ConditionalValue ret = await this.Queue.TryDequeueAsync(txn, cancellationToken);
    bool valueProcessed = false;

    if (ret.HasValue)
    {
        if (ret.Value > 10)
        {
            // Process the item
            Console.WriteLine("Value : " + ret.Value);
            valueProcessed = true;
        }
    }

    if (valueProcessed)
    {
        await txn.CommitAsync();    
    }
    else
    {
        await txn.AbortAsync();
    }
}
```

## <a name="must-read"></a><span data-ttu-id="0df4f-202">Måste läsa</span><span class="sxs-lookup"><span data-stu-id="0df4f-202">Must Read</span></span>
* [<span data-ttu-id="0df4f-203">Snabbstart för Reliable Services</span><span class="sxs-lookup"><span data-stu-id="0df4f-203">Reliable Services Quick Start</span></span>](service-fabric-reliable-services-quick-start.md)
* [<span data-ttu-id="0df4f-204">Arbeta med Reliable Collections</span><span class="sxs-lookup"><span data-stu-id="0df4f-204">Working with Reliable Collections</span></span>](service-fabric-work-with-reliable-collections.md)
* [<span data-ttu-id="0df4f-205">Reliable Services-meddelanden</span><span class="sxs-lookup"><span data-stu-id="0df4f-205">Reliable Services notifications</span></span>](service-fabric-reliable-services-notifications.md)
* [<span data-ttu-id="0df4f-206">Reliable Services säkerhetskopiering och återställning (Disaster Recovery)</span><span class="sxs-lookup"><span data-stu-id="0df4f-206">Reliable Services Backup and Restore (Disaster Recovery)</span></span>](service-fabric-reliable-services-backup-restore.md)
* [<span data-ttu-id="0df4f-207">Konfiguration av tillförlitliga tillstånd Manager</span><span class="sxs-lookup"><span data-stu-id="0df4f-207">Reliable State Manager Configuration</span></span>](service-fabric-reliable-services-configuration.md)
* [<span data-ttu-id="0df4f-208">Komma igång med Service Fabric Web API-tjänster</span><span class="sxs-lookup"><span data-stu-id="0df4f-208">Getting Started with Service Fabric Web API Services</span></span>](service-fabric-reliable-services-communication-webapi.md)
* [<span data-ttu-id="0df4f-209">Avancerad användning av tjänsterna tillförlitliga programmeringsmodellen</span><span class="sxs-lookup"><span data-stu-id="0df4f-209">Advanced Usage of the Reliable Services Programming Model</span></span>](service-fabric-reliable-services-advanced-usage.md)
* [<span data-ttu-id="0df4f-210">För utvecklare för tillförlitlig samlingar</span><span class="sxs-lookup"><span data-stu-id="0df4f-210">Developer Reference for Reliable Collections</span></span>](https://msdn.microsoft.com/library/azure/microsoft.servicefabric.data.collections.aspx)
