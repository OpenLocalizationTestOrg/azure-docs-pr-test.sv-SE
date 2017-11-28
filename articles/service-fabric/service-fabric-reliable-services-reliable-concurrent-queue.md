---
title: aaaReliableConcurrentQueue i Azure Service Fabric
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
ms.openlocfilehash: 78a9905996b9ab265c1288d2b49753638d7bc445
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="introduction-tooreliableconcurrentqueue-in-azure-service-fabric"></a><span data-ttu-id="c69d2-103">Introduktion tooReliableConcurrentQueue i Azure Service Fabric</span><span class="sxs-lookup"><span data-stu-id="c69d2-103">Introduction tooReliableConcurrentQueue in Azure Service Fabric</span></span>
<span data-ttu-id="c69d2-104">Tillförlitliga samtidiga kön är en asynkron transaktionell och replikerade kö vilka funktioner hög samtidighet för sätta och åtgärder som har status Created.</span><span class="sxs-lookup"><span data-stu-id="c69d2-104">Reliable Concurrent Queue is an asynchronous, transactional, and replicated queue which features high concurrency for enqueue and dequeue operations.</span></span> <span data-ttu-id="c69d2-105">Den är utformad toodeliver högt genomflöde och låg fördröjning av slappna hello strikt FIFO ordning som tillhandahålls av [tillförlitliga kön](https://msdn.microsoft.com/library/azure/dn971527.aspx) och i stället tillhandahåller en bästa sortering.</span><span class="sxs-lookup"><span data-stu-id="c69d2-105">It is designed toodeliver high throughput and low latency by relaxing hello strict FIFO ordering provided by [Reliable Queue](https://msdn.microsoft.com/library/azure/dn971527.aspx) and instead provides a best-effort ordering.</span></span>

## <a name="apis"></a><span data-ttu-id="c69d2-106">API:er</span><span class="sxs-lookup"><span data-stu-id="c69d2-106">APIs</span></span>

|<span data-ttu-id="c69d2-107">Samtidiga kön</span><span class="sxs-lookup"><span data-stu-id="c69d2-107">Concurrent Queue</span></span>                |<span data-ttu-id="c69d2-108">Tillförlitliga samtidiga kön</span><span class="sxs-lookup"><span data-stu-id="c69d2-108">Reliable Concurrent Queue</span></span>                                         |
|--------------------------------|------------------------------------------------------------------|
| <span data-ttu-id="c69d2-109">void Enqueue(T item)</span><span class="sxs-lookup"><span data-stu-id="c69d2-109">void Enqueue(T item)</span></span>           | <span data-ttu-id="c69d2-110">Uppgiften EnqueueAsync (ITransaction tx T objekt)</span><span class="sxs-lookup"><span data-stu-id="c69d2-110">Task EnqueueAsync(ITransaction tx, T item)</span></span>                       |
| <span data-ttu-id="c69d2-111">bool TryDequeue (ut T resultat)</span><span class="sxs-lookup"><span data-stu-id="c69d2-111">bool TryDequeue(out T result)</span></span>  | <span data-ttu-id="c69d2-112">Uppgiften < ConditionalValue < T >> TryDequeueAsync (tx ITransaction)</span><span class="sxs-lookup"><span data-stu-id="c69d2-112">Task< ConditionalValue < T > > TryDequeueAsync(ITransaction tx)</span></span>  |
| <span data-ttu-id="c69d2-113">int Count()</span><span class="sxs-lookup"><span data-stu-id="c69d2-113">int Count()</span></span>                    | <span data-ttu-id="c69d2-114">lång Count()</span><span class="sxs-lookup"><span data-stu-id="c69d2-114">long Count()</span></span>                                                     |

## <a name="comparison-with-reliable-queuehttpsmsdnmicrosoftcomlibraryazuredn971527aspx"></a><span data-ttu-id="c69d2-115">Jämförelse med [tillförlitliga kön](https://msdn.microsoft.com/library/azure/dn971527.aspx)</span><span class="sxs-lookup"><span data-stu-id="c69d2-115">Comparison with [Reliable Queue](https://msdn.microsoft.com/library/azure/dn971527.aspx)</span></span>

<span data-ttu-id="c69d2-116">Tillförlitliga samtidiga kön erbjuds som ett alternativ för[tillförlitliga kön](https://msdn.microsoft.com/library/azure/dn971527.aspx).</span><span class="sxs-lookup"><span data-stu-id="c69d2-116">Reliable Concurrent Queue is offered as an alternative too[Reliable Queue](https://msdn.microsoft.com/library/azure/dn971527.aspx).</span></span> <span data-ttu-id="c69d2-117">Det ska användas i fall där strikt FIFO ordning inte krävs, som garanterar FIFO kräver en kompromiss med samtidighet.</span><span class="sxs-lookup"><span data-stu-id="c69d2-117">It should be used in cases where strict FIFO ordering is not required, as guaranteeing FIFO requires a tradeoff with concurrency.</span></span>  <span data-ttu-id="c69d2-118">[Tillförlitliga kön](https://msdn.microsoft.com/library/azure/dn971527.aspx) använder lås tooenforce FIFO ordning, med maximalt en transaktion tillåts tooenqueue och maximalt en transaktion tillåts toodequeue i taget.</span><span class="sxs-lookup"><span data-stu-id="c69d2-118">[Reliable Queue](https://msdn.microsoft.com/library/azure/dn971527.aspx) uses locks tooenforce FIFO ordering, with at most one transaction allowed tooenqueue and at most one transaction allowed toodequeue at a time.</span></span> <span data-ttu-id="c69d2-119">Jämförelse tillförlitliga samtidiga kön sänker hello ordning begränsningen tillåter alla antalet samtidiga transaktioner toointerleave sina sätta och åtgärder har status Created.</span><span class="sxs-lookup"><span data-stu-id="c69d2-119">In comparison, Reliable Concurrent Queue relaxes hello ordering constraint and allows any number concurrent transactions toointerleave their enqueue and dequeue operations.</span></span> <span data-ttu-id="c69d2-120">Bästa ordning har angetts men hello relativa ordning av två värden i en tillförlitlig samtidiga kö kan inte garanteras.</span><span class="sxs-lookup"><span data-stu-id="c69d2-120">Best-effort ordering is provided, however hello relative ordering of two values in a Reliable Concurrent Queue can never be guaranteed.</span></span>

<span data-ttu-id="c69d2-121">Tillförlitliga samtidiga kön ger högre genomflöde och kortare svarstid än [tillförlitliga kö](https://msdn.microsoft.com/library/azure/dn971527.aspx) när det finns flera samtidiga transaktioner som utför enqueues och/eller dequeues.</span><span class="sxs-lookup"><span data-stu-id="c69d2-121">Reliable Concurrent Queue provides higher throughput and lower latency than [Reliable Queue](https://msdn.microsoft.com/library/azure/dn971527.aspx) whenever there are multiple concurrent transactions performing enqueues and/or dequeues.</span></span>

<span data-ttu-id="c69d2-122">Ett exempel på en användningsfall för hello ReliableConcurrentQueue är hello [meddelandekö](https://en.wikipedia.org/wiki/Message_queue) scenario.</span><span class="sxs-lookup"><span data-stu-id="c69d2-122">A sample use case for hello ReliableConcurrentQueue is hello [Message Queue](https://en.wikipedia.org/wiki/Message_queue) scenario.</span></span> <span data-ttu-id="c69d2-123">I det här scenariot en eller flera meddelandeproducenter skapa och lägga till objekt toohello kö och en eller flera meddelandet konsumenter pull-meddelanden från kön hello och bearbeta dem..</span><span class="sxs-lookup"><span data-stu-id="c69d2-123">In this scenario, one or more message producers create and add items toohello queue, and one or more message consumers pull messages from hello queue and process them.</span></span> <span data-ttu-id="c69d2-124">Flera producenter och konsumenter fungerar oberoende av varandra, med hjälp av samtidiga transaktioner i ordning tooprocess hello kö.</span><span class="sxs-lookup"><span data-stu-id="c69d2-124">Multiple producers and consumers can work independently, using concurrent transactions in order tooprocess hello queue.</span></span>

## <a name="usage-guidelines"></a><span data-ttu-id="c69d2-125">Riktlinjer för användning</span><span class="sxs-lookup"><span data-stu-id="c69d2-125">Usage Guidelines</span></span>
* <span data-ttu-id="c69d2-126">hello kön förväntar sig att hello objekt i hello kön har en låg Bevarandeperiod.</span><span class="sxs-lookup"><span data-stu-id="c69d2-126">hello queue expects that hello items in hello queue have a low retention period.</span></span> <span data-ttu-id="c69d2-127">Som är hello objekt skulle inte kvar i hello kö för länge.</span><span class="sxs-lookup"><span data-stu-id="c69d2-127">That is, hello items would not stay in hello queue for a long time.</span></span>
* <span data-ttu-id="c69d2-128">hello kön garanterar inte strikt FIFO ordning.</span><span class="sxs-lookup"><span data-stu-id="c69d2-128">hello queue does not guarantee strict FIFO ordering.</span></span>
* <span data-ttu-id="c69d2-129">hello kön läser inte sin egen skrivningar.</span><span class="sxs-lookup"><span data-stu-id="c69d2-129">hello queue does not read its own writes.</span></span> <span data-ttu-id="c69d2-130">Om ett objekt i kö i en transaktion, kommer inte att visas tooa dequeuer inom hello samma transaktion.</span><span class="sxs-lookup"><span data-stu-id="c69d2-130">If an item is enqueued within a transaction, it will not be visible tooa dequeuer within hello same transaction.</span></span>
* <span data-ttu-id="c69d2-131">Dequeues inte är isolerade från varandra.</span><span class="sxs-lookup"><span data-stu-id="c69d2-131">Dequeues are not isolated from each other.</span></span> <span data-ttu-id="c69d2-132">Om objektet *A* har tagits bort i transaktion *txnA*, även om *txnA* är därmed inte verkställas objektet *A* inte är synliga tooa samtidiga transaktionen *txnB*.</span><span class="sxs-lookup"><span data-stu-id="c69d2-132">If item *A* is dequeued in transaction *txnA*, even though *txnA* is not committed, item *A* would not be visible tooa concurrent transaction *txnB*.</span></span>  <span data-ttu-id="c69d2-133">Om *txnA* avbryts, *A* ska vara synliga för*txnB* omedelbart.</span><span class="sxs-lookup"><span data-stu-id="c69d2-133">If *txnA* aborts, *A* will become visible too*txnB* immediately.</span></span>
* <span data-ttu-id="c69d2-134">*TryPeekAsync* beteende kan implementeras med hjälp av en *TryDequeueAsync* och sedan avbryter hello transaktionen.</span><span class="sxs-lookup"><span data-stu-id="c69d2-134">*TryPeekAsync* behavior can be implemented by using a *TryDequeueAsync* and then aborting hello transaction.</span></span> <span data-ttu-id="c69d2-135">Ett exempel på detta finns i hello Programming mönster avsnitt.</span><span class="sxs-lookup"><span data-stu-id="c69d2-135">An example of this can be found in hello Programming Patterns section.</span></span>
* <span data-ttu-id="c69d2-136">Antalet är icke-transaktionell.</span><span class="sxs-lookup"><span data-stu-id="c69d2-136">Count is non-transactional.</span></span> <span data-ttu-id="c69d2-137">Det kan vara används tooget en uppfattning om hello antalet element i hello kön, men representerar point-in-time och kan inte förlita sig på.</span><span class="sxs-lookup"><span data-stu-id="c69d2-137">It can be used tooget an idea of hello number of elements in hello queue, but represents a point-in-time and cannot be relied upon.</span></span>
* <span data-ttu-id="c69d2-138">Billigare bearbetning på hello togs bort från kön objekt bör inte utföras medan hello transaktion är aktiv, tooavoid långvariga transaktioner som kan påverka prestanda på hello system.</span><span class="sxs-lookup"><span data-stu-id="c69d2-138">Expensive processing on hello dequeued items should not be performed while hello transaction is active, tooavoid long-running transactions which may have a performance impact on hello system.</span></span>

## <a name="code-snippets"></a><span data-ttu-id="c69d2-139">Kodstycken</span><span class="sxs-lookup"><span data-stu-id="c69d2-139">Code Snippets</span></span>
<span data-ttu-id="c69d2-140">Låt oss titta på några kodstycken och deras förväntade produktion.</span><span class="sxs-lookup"><span data-stu-id="c69d2-140">Let us look at a few code snippets and their expected outputs.</span></span> <span data-ttu-id="c69d2-141">Undantagshantering ignoreras i det här avsnittet.</span><span class="sxs-lookup"><span data-stu-id="c69d2-141">Exception handling is ignored in this section.</span></span>

### <a name="enqueueasync"></a><span data-ttu-id="c69d2-142">EnqueueAsync</span><span class="sxs-lookup"><span data-stu-id="c69d2-142">EnqueueAsync</span></span>
<span data-ttu-id="c69d2-143">Här följer några kodstycken för att använda EnqueueAsync följt av de förväntade utdata.</span><span class="sxs-lookup"><span data-stu-id="c69d2-143">Here are a few code snippets for using EnqueueAsync followed by their expected outputs.</span></span>

- <span data-ttu-id="c69d2-144">*Fall 1: Enkel köa aktiviteten*</span><span class="sxs-lookup"><span data-stu-id="c69d2-144">*Case 1: Single Enqueue Task*</span></span>

```
using (var txn = this.StateManager.CreateTransaction())
{
    await this.Queue.EnqueueAsync(txn, 10, cancellationToken);
    await this.Queue.EnqueueAsync(txn, 20, cancellationToken);

    await txn.CommitAsync();
}
```

<span data-ttu-id="c69d2-145">Anta att hello uppgiften har slutförts och som det finns inga samtidiga transaktioner ändra hello kön.</span><span class="sxs-lookup"><span data-stu-id="c69d2-145">Assume that hello task completed successfully, and that there were no concurrent transactions modifying hello queue.</span></span> <span data-ttu-id="c69d2-146">hello användare kan förvänta sig hello köobjekt toocontain hello i något av följande order hello:</span><span class="sxs-lookup"><span data-stu-id="c69d2-146">hello user can expect hello queue toocontain hello items in any of hello following orders:</span></span>

> <span data-ttu-id="c69d2-147">10, 20</span><span class="sxs-lookup"><span data-stu-id="c69d2-147">10, 20</span></span>

> <span data-ttu-id="c69d2-148">20, 10</span><span class="sxs-lookup"><span data-stu-id="c69d2-148">20, 10</span></span>


- <span data-ttu-id="c69d2-149">*Fall 2: Parallell köa aktiviteten*</span><span class="sxs-lookup"><span data-stu-id="c69d2-149">*Case 2: Parallel Enqueue Task*</span></span>

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

<span data-ttu-id="c69d2-150">Anta att hello aktiviteter har slutförts, att hello uppgifter kördes parallellt och att det inte finns några andra samtidiga transaktioner ändra hello kön.</span><span class="sxs-lookup"><span data-stu-id="c69d2-150">Assume that hello tasks completed successfully, that hello tasks ran in parallel, and that there were no other concurrent transactions modifying hello queue.</span></span> <span data-ttu-id="c69d2-151">Inga härledning kan göras om hello ordningen på objekten i hello kön.</span><span class="sxs-lookup"><span data-stu-id="c69d2-151">No inference can be made about hello order of items in hello queue.</span></span> <span data-ttu-id="c69d2-152">För det här kodstycket kan hello objekt visas i någon av hello 4!</span><span class="sxs-lookup"><span data-stu-id="c69d2-152">For this code snippet, hello items may appear in any of hello 4!</span></span> <span data-ttu-id="c69d2-153">möjliga ordningar.</span><span class="sxs-lookup"><span data-stu-id="c69d2-153">possible orderings.</span></span>  <span data-ttu-id="c69d2-154">hello kön försöker tookeep hello poster i en hello ursprungliga (köas), men kan vara framtvingad tooreorder dem på grund av tooconcurrent åtgärder eller fel.</span><span class="sxs-lookup"><span data-stu-id="c69d2-154">hello queue will attempt tookeep hello items in hello original (enqueued) order, but may be forced tooreorder them due tooconcurrent operations or faults.</span></span>


### <a name="dequeueasync"></a><span data-ttu-id="c69d2-155">DequeueAsync</span><span class="sxs-lookup"><span data-stu-id="c69d2-155">DequeueAsync</span></span>
<span data-ttu-id="c69d2-156">Här följer några kodstycken för att använda TryDequeueAsync följt av hello förväntades utdata.</span><span class="sxs-lookup"><span data-stu-id="c69d2-156">Here are a few code snippets for using TryDequeueAsync followed by hello expected outputs.</span></span> <span data-ttu-id="c69d2-157">Anta hello kön redan är ifyllda hello följande objekt i kö för hello:</span><span class="sxs-lookup"><span data-stu-id="c69d2-157">Assume that hello queue is already populated with hello following items in hello queue:</span></span>
> <span data-ttu-id="c69d2-158">10, 20, 30, 40, 50, 60</span><span class="sxs-lookup"><span data-stu-id="c69d2-158">10, 20, 30, 40, 50, 60</span></span>

- <span data-ttu-id="c69d2-159">*Fall 1: En har status Created aktivitet*</span><span class="sxs-lookup"><span data-stu-id="c69d2-159">*Case 1: Single Dequeue Task*</span></span>

```
using (var txn = this.StateManager.CreateTransaction())
{
    await this.Queue.TryDequeueAsync(txn, cancellationToken);
    await this.Queue.TryDequeueAsync(txn, cancellationToken);
    await this.Queue.TryDequeueAsync(txn, cancellationToken);

    await txn.CommitAsync();
}
```

<span data-ttu-id="c69d2-160">Anta att hello uppgiften har slutförts och som det finns inga samtidiga transaktioner ändra hello kön.</span><span class="sxs-lookup"><span data-stu-id="c69d2-160">Assume that hello task completed successfully, and that there were no concurrent transactions modifying hello queue.</span></span> <span data-ttu-id="c69d2-161">Eftersom ingen härledning kan göras om hello ordning hello artiklar i hello kö, alla tre hello-objekt kan vara togs bort från kön, i vilken ordning som helst.</span><span class="sxs-lookup"><span data-stu-id="c69d2-161">Since no inference can be made about hello order of hello items in hello queue, any three of hello items may be dequeued, in any order.</span></span> <span data-ttu-id="c69d2-162">hello kön försöker tookeep hello poster i en hello ursprungliga (köas), men kan vara framtvingad tooreorder dem på grund av tooconcurrent åtgärder eller fel.</span><span class="sxs-lookup"><span data-stu-id="c69d2-162">hello queue will attempt tookeep hello items in hello original (enqueued) order, but may be forced tooreorder them due tooconcurrent operations or faults.</span></span>  

- <span data-ttu-id="c69d2-163">*Fall 2: Parallell status Created aktivitet*</span><span class="sxs-lookup"><span data-stu-id="c69d2-163">*Case 2: Parallel Dequeue Task*</span></span>

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

<span data-ttu-id="c69d2-164">Anta att hello aktiviteter har slutförts, att hello uppgifter kördes parallellt och att det inte finns några andra samtidiga transaktioner ändra hello kön.</span><span class="sxs-lookup"><span data-stu-id="c69d2-164">Assume that hello tasks completed successfully, that hello tasks ran in parallel, and that there were no other concurrent transactions modifying hello queue.</span></span> <span data-ttu-id="c69d2-165">Eftersom ingen härledning kan göras om hello ordning hello artiklar i hello kö, hello listor *dequeue1* och *dequeue2* varje innehåller två objekt, i vilken ordning som helst.</span><span class="sxs-lookup"><span data-stu-id="c69d2-165">Since no inference can be made about hello order of hello items in hello queue, hello lists *dequeue1* and *dequeue2* will each contain any two items, in any order.</span></span>

<span data-ttu-id="c69d2-166">hello samma artikel kommer *inte* visas i båda listorna.</span><span class="sxs-lookup"><span data-stu-id="c69d2-166">hello same item will *not* appear in both lists.</span></span> <span data-ttu-id="c69d2-167">Därför om dequeue1 har *10*, *30*, dequeue2 skulle ha *20*, *40*.</span><span class="sxs-lookup"><span data-stu-id="c69d2-167">Hence, if dequeue1 has *10*, *30*, then dequeue2 would have *20*, *40*.</span></span>

- <span data-ttu-id="c69d2-168">*Fall 3: Status Created ordning med Transaktionsavbrott*</span><span class="sxs-lookup"><span data-stu-id="c69d2-168">*Case 3: Dequeue Ordering With Transaction Abort*</span></span>

<span data-ttu-id="c69d2-169">Avbryter en transaktion med pågående dequeues placeringar hello objekt tillbaka på hello huvud hello kön.</span><span class="sxs-lookup"><span data-stu-id="c69d2-169">Aborting a transaction with in-flight dequeues puts hello items back on hello head of hello queue.</span></span> <span data-ttu-id="c69d2-170">hello ordning som hello objekt är lägga tillbaka i hello huvud hello kön är inte säkert.</span><span class="sxs-lookup"><span data-stu-id="c69d2-170">hello order in which hello items are put back on hello head of hello queue is not guaranteed.</span></span> <span data-ttu-id="c69d2-171">Låt oss titta på hello följande kod:</span><span class="sxs-lookup"><span data-stu-id="c69d2-171">Let us look at hello following code:</span></span>

```
using (var txn = this.StateManager.CreateTransaction())
{
    await this.Queue.TryDequeueAsync(txn, cancellationToken);
    await this.Queue.TryDequeueAsync(txn, cancellationToken);

    // Abort hello transaction
    await txn.AbortAsync();
}
```
<span data-ttu-id="c69d2-172">Anta att hello-objekt har tagits bort i hello följande ordning:</span><span class="sxs-lookup"><span data-stu-id="c69d2-172">Assume that hello items were dequeued in hello following order:</span></span>
> <span data-ttu-id="c69d2-173">10, 20</span><span class="sxs-lookup"><span data-stu-id="c69d2-173">10, 20</span></span>

<span data-ttu-id="c69d2-174">När vi avbryta hello transaktion skulle hello objekt läggas till bakre toohello head hello köns i något av följande order hello:</span><span class="sxs-lookup"><span data-stu-id="c69d2-174">When we abort hello transaction, hello items would be added back toohello head of hello queue in any of hello following orders:</span></span>
> <span data-ttu-id="c69d2-175">10, 20</span><span class="sxs-lookup"><span data-stu-id="c69d2-175">10, 20</span></span>

> <span data-ttu-id="c69d2-176">20, 10</span><span class="sxs-lookup"><span data-stu-id="c69d2-176">20, 10</span></span>

<span data-ttu-id="c69d2-177">hello samma sak gäller för alla fall där hello transaktionen inte har *genomförd*.</span><span class="sxs-lookup"><span data-stu-id="c69d2-177">hello same is true for all cases where hello transaction was not successfully *Committed*.</span></span>

## <a name="programming-patterns"></a><span data-ttu-id="c69d2-178">Mönster för programmering</span><span class="sxs-lookup"><span data-stu-id="c69d2-178">Programming Patterns</span></span>
<span data-ttu-id="c69d2-179">I det här avsnittet tittar vi på några programmering mönster som kan vara användbart i med hjälp av ReliableConcurrentQueue.</span><span class="sxs-lookup"><span data-stu-id="c69d2-179">In this section, let us look at a few programming patterns that might be helpful in using ReliableConcurrentQueue.</span></span>

### <a name="batch-dequeues"></a><span data-ttu-id="c69d2-180">Batch Dequeues</span><span class="sxs-lookup"><span data-stu-id="c69d2-180">Batch Dequeues</span></span>
<span data-ttu-id="c69d2-181">A rekommenderas programming mönstret är för hello konsumenten uppgiften toobatch dess dequeues i stället för att utföra en i taget har status Created.</span><span class="sxs-lookup"><span data-stu-id="c69d2-181">A recommended programming pattern is for hello consumer task toobatch its dequeues instead of performing one dequeue at a time.</span></span> <span data-ttu-id="c69d2-182">hello användaren kan välja toothrottle fördröjningar mellan varje batch eller hello batchstorlek.</span><span class="sxs-lookup"><span data-stu-id="c69d2-182">hello user can choose toothrottle delays between every batch or hello batch size.</span></span> <span data-ttu-id="c69d2-183">hello visar följande kodavsnitt den här programmeringsmodell.</span><span class="sxs-lookup"><span data-stu-id="c69d2-183">hello following code snippet shows this programming model.</span></span>  <span data-ttu-id="c69d2-184">Observera att i det här exemplet hello bearbetning görs efter hello transaktionen är genomförd, så om ett fel toooccur vid bearbetning, hello obearbetat objekt försvinner utan har bearbetats.</span><span class="sxs-lookup"><span data-stu-id="c69d2-184">Note that in this example, hello processing is done after hello transaction is committed, so if a fault were toooccur while processing, hello unprocessed items will be lost without having been processed.</span></span>  <span data-ttu-id="c69d2-185">Alternativt hello bearbetning kan göras inom hello transaktions-scope, men detta kan ha en negativ inverkan på prestanda och kräver hanteringen av hello artiklar redan bearbetats.</span><span class="sxs-lookup"><span data-stu-id="c69d2-185">Alternatively, hello processing can be done within hello transaction's scope, however this may have a negative impact on performance and requires handling of hello items already processed.</span></span>

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
                // If an item was dequeued, add toohello buffer for processing
                processItems.Add(ret.Value);
            }
            else
            {
                // else break hello for loop
                break;
            }
        }

        await txn.CommitAsync();
    }

    // Process hello dequeues
    for (int i = 0; i < processItems.Count; ++i)
    {
        Console.WriteLine("Value : " + processItems[i]);
    }

    int delayFactor = batchSize - processItems.Count;
    await Task.Delay(TimeSpan.FromMilliseconds(delayMs * delayFactor), cancellationToken);
}
```

### <a name="best-effort-notification-based-processing"></a><span data-ttu-id="c69d2-186">Bästa meddelandebaserad bearbetning</span><span class="sxs-lookup"><span data-stu-id="c69d2-186">Best-Effort Notification-Based Processing</span></span>
<span data-ttu-id="c69d2-187">En annan intressant programming mönster använder hello antal API.</span><span class="sxs-lookup"><span data-stu-id="c69d2-187">Another interesting programming pattern uses hello Count API.</span></span> <span data-ttu-id="c69d2-188">Här kan vi implementera bästa meddelandebaserad bearbetning för hello kö.</span><span class="sxs-lookup"><span data-stu-id="c69d2-188">Here, we can implement best-effort notification-based processing for hello queue.</span></span> <span data-ttu-id="c69d2-189">hello kön antalet kan vara används toothrottle en sätta eller en dequeue aktivitet.</span><span class="sxs-lookup"><span data-stu-id="c69d2-189">hello queue Count can be used toothrottle an enqueue or a dequeue task.</span></span>  <span data-ttu-id="c69d2-190">Observera att som i föregående exempel hello eftersom hello bearbetningen sker utanför hello transaktion obearbetat objekt kan gå förlorade om ett fel uppstår under bearbetningen.</span><span class="sxs-lookup"><span data-stu-id="c69d2-190">Note that as in hello previous example, since hello processing occurs outside hello transaction, unprocessed items may be lost if a fault occurs during processing.</span></span>

```
int threshold = 5;
long delayMs = 1000;

while(!cancellationToken.IsCancellationRequested)
{
    while (this.Queue.Count < threshold)
    {
        cancellationToken.ThrowIfCancellationRequested();

        // If hello queue does not have hello threshold number of items, delay hello task and check again
        await Task.Delay(TimeSpan.FromMilliseconds(delayMs), cancellationToken);
    }

    // If there are approximately threshold number of items, try and process hello queue

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
                // If an item was dequeued, add toohello buffer for processing
                processItems.Add(ret.Value);
            }
        } while (processItems.Count < threshold && ret.HasValue);

        await txn.CommitAsync();
    }

    // Process hello dequeues
    for (int i = 0; i < processItems.Count; ++i)
    {
        Console.WriteLine("Value : " + processItems[i]);
    }
}
```

### <a name="best-effort-drain"></a><span data-ttu-id="c69d2-191">Bästa tömning</span><span class="sxs-lookup"><span data-stu-id="c69d2-191">Best-Effort Drain</span></span>
<span data-ttu-id="c69d2-192">Kan inte garanteras tömning av hello kö på grund av toohello samtidiga uppbyggnad hello-datastrukturen.</span><span class="sxs-lookup"><span data-stu-id="c69d2-192">A drain of hello queue cannot be guaranteed due toohello concurrent nature of hello data structure.</span></span>  <span data-ttu-id="c69d2-193">Det är möjligt att även om inga användare åtgärder på hello kön finns relä, en viss anropet tooTryDequeueAsync inte kan returnera ett objekt som tidigare var köas och bekräftats.</span><span class="sxs-lookup"><span data-stu-id="c69d2-193">It is possible that, even if no user operations on hello queue are in-flight, a particular call tooTryDequeueAsync may not return an item which was previously enqueued and committed.</span></span>  <span data-ttu-id="c69d2-194">hello köas objektet garanteras för*slutligen* bli synliga toodequeue men utan en mekanism för out-of-band-kommunikation, ett oberoende konsumenten inte kan vet hello kön har uppnått ett stabilt tillstånd även om alla producenter har stoppats och inga nya sätta tillåts.</span><span class="sxs-lookup"><span data-stu-id="c69d2-194">hello enqueued item is guaranteed too*eventually* become visible toodequeue, however without an out-of-band communication mechanism, an independent consumer cannot know that hello queue has reached a steady-state even if all producers have been stopped and no new enqueue operations are allowed.</span></span> <span data-ttu-id="c69d2-195">Därför är hello tömning åtgärden bästa som implementeras nedan.</span><span class="sxs-lookup"><span data-stu-id="c69d2-195">Thus, hello drain operation is best-effort as implemented below.</span></span>

<span data-ttu-id="c69d2-196">hello användaren ska stoppa alla ytterligare producenten och konsumentuppgifter och vänta tills alla pågående transaktioner toocommit eller Avbryt innan du försöker toodrain hello kön.</span><span class="sxs-lookup"><span data-stu-id="c69d2-196">hello user should stop all further producer and consumer tasks, and wait for any in-flight transactions toocommit or abort, before attempting toodrain hello queue.</span></span>  <span data-ttu-id="c69d2-197">Om hello användaren känner hello förväntat antal objekt i kö hello kan skapa de ett meddelande som signalerar till att alla objekt har har tagits bort.</span><span class="sxs-lookup"><span data-stu-id="c69d2-197">If hello user knows hello expected number of items in hello queue, they can set up a notification which signals that all items have been dequeued.</span></span>

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
                // Buffer hello dequeues
                processItems.Add(ret.Value);
            }
        } while (ret.HasValue && processItems.Count < batchSize);

        await txn.CommitAsync();
    }

    // Process hello dequeues
    for (int i = 0; i < processItems.Count; ++i)
    {
        Console.WriteLine("Value : " + processItems[i]);
    }
} while (ret.HasValue);
```

### <a name="peek"></a><span data-ttu-id="c69d2-198">Granska</span><span class="sxs-lookup"><span data-stu-id="c69d2-198">Peek</span></span>
<span data-ttu-id="c69d2-199">ReliableConcurrentQueue ger inte hello *TryPeekAsync* api.</span><span class="sxs-lookup"><span data-stu-id="c69d2-199">ReliableConcurrentQueue does not provide hello *TryPeekAsync* api.</span></span> <span data-ttu-id="c69d2-200">Användare kan få hello titt semantiska med hjälp av en *TryDequeueAsync* och sedan avbryter hello transaktionen.</span><span class="sxs-lookup"><span data-stu-id="c69d2-200">Users can get hello peek semantic by using a *TryDequeueAsync* and then aborting hello transaction.</span></span> <span data-ttu-id="c69d2-201">I det här exemplet dequeues bearbetas bara om hello objektets värde är större än *10*.</span><span class="sxs-lookup"><span data-stu-id="c69d2-201">In this example, dequeues are processed only if hello item's value is greater than *10*.</span></span>

```
using (var txn = this.StateManager.CreateTransaction())
{
    ConditionalValue ret = await this.Queue.TryDequeueAsync(txn, cancellationToken);
    bool valueProcessed = false;

    if (ret.HasValue)
    {
        if (ret.Value > 10)
        {
            // Process hello item
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

## <a name="must-read"></a><span data-ttu-id="c69d2-202">Måste läsa</span><span class="sxs-lookup"><span data-stu-id="c69d2-202">Must Read</span></span>
* [<span data-ttu-id="c69d2-203">Snabbstart för Reliable Services</span><span class="sxs-lookup"><span data-stu-id="c69d2-203">Reliable Services Quick Start</span></span>](service-fabric-reliable-services-quick-start.md)
* [<span data-ttu-id="c69d2-204">Arbeta med Reliable Collections</span><span class="sxs-lookup"><span data-stu-id="c69d2-204">Working with Reliable Collections</span></span>](service-fabric-work-with-reliable-collections.md)
* [<span data-ttu-id="c69d2-205">Reliable Services-meddelanden</span><span class="sxs-lookup"><span data-stu-id="c69d2-205">Reliable Services notifications</span></span>](service-fabric-reliable-services-notifications.md)
* [<span data-ttu-id="c69d2-206">Reliable Services säkerhetskopiering och återställning (Disaster Recovery)</span><span class="sxs-lookup"><span data-stu-id="c69d2-206">Reliable Services Backup and Restore (Disaster Recovery)</span></span>](service-fabric-reliable-services-backup-restore.md)
* [<span data-ttu-id="c69d2-207">Konfiguration av tillförlitliga tillstånd Manager</span><span class="sxs-lookup"><span data-stu-id="c69d2-207">Reliable State Manager Configuration</span></span>](service-fabric-reliable-services-configuration.md)
* [<span data-ttu-id="c69d2-208">Komma igång med Service Fabric Web API-tjänster</span><span class="sxs-lookup"><span data-stu-id="c69d2-208">Getting Started with Service Fabric Web API Services</span></span>](service-fabric-reliable-services-communication-webapi.md)
* [<span data-ttu-id="c69d2-209">Avancerad användning av hello programmeringsmodellen i Reliable Services</span><span class="sxs-lookup"><span data-stu-id="c69d2-209">Advanced Usage of hello Reliable Services Programming Model</span></span>](service-fabric-reliable-services-advanced-usage.md)
* [<span data-ttu-id="c69d2-210">För utvecklare för tillförlitlig samlingar</span><span class="sxs-lookup"><span data-stu-id="c69d2-210">Developer Reference for Reliable Collections</span></span>](https://msdn.microsoft.com/library/azure/microsoft.servicefabric.data.collections.aspx)
