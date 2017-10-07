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
# <a name="introduction-tooreliableconcurrentqueue-in-azure-service-fabric"></a>Introduktion tooReliableConcurrentQueue i Azure Service Fabric
Tillförlitliga samtidiga kön är en asynkron transaktionell och replikerade kö vilka funktioner hög samtidighet för sätta och åtgärder som har status Created. Den är utformad toodeliver högt genomflöde och låg fördröjning av slappna hello strikt FIFO ordning som tillhandahålls av [tillförlitliga kön](https://msdn.microsoft.com/library/azure/dn971527.aspx) och i stället tillhandahåller en bästa sortering.

## <a name="apis"></a>API:er

|Samtidiga kön                |Tillförlitliga samtidiga kön                                         |
|--------------------------------|------------------------------------------------------------------|
| void Enqueue(T item)           | Uppgiften EnqueueAsync (ITransaction tx T objekt)                       |
| bool TryDequeue (ut T resultat)  | Uppgiften < ConditionalValue < T >> TryDequeueAsync (tx ITransaction)  |
| int Count()                    | lång Count()                                                     |

## <a name="comparison-with-reliable-queuehttpsmsdnmicrosoftcomlibraryazuredn971527aspx"></a>Jämförelse med [tillförlitliga kön](https://msdn.microsoft.com/library/azure/dn971527.aspx)

Tillförlitliga samtidiga kön erbjuds som ett alternativ för[tillförlitliga kön](https://msdn.microsoft.com/library/azure/dn971527.aspx). Det ska användas i fall där strikt FIFO ordning inte krävs, som garanterar FIFO kräver en kompromiss med samtidighet.  [Tillförlitliga kön](https://msdn.microsoft.com/library/azure/dn971527.aspx) använder lås tooenforce FIFO ordning, med maximalt en transaktion tillåts tooenqueue och maximalt en transaktion tillåts toodequeue i taget. Jämförelse tillförlitliga samtidiga kön sänker hello ordning begränsningen tillåter alla antalet samtidiga transaktioner toointerleave sina sätta och åtgärder har status Created. Bästa ordning har angetts men hello relativa ordning av två värden i en tillförlitlig samtidiga kö kan inte garanteras.

Tillförlitliga samtidiga kön ger högre genomflöde och kortare svarstid än [tillförlitliga kö](https://msdn.microsoft.com/library/azure/dn971527.aspx) när det finns flera samtidiga transaktioner som utför enqueues och/eller dequeues.

Ett exempel på en användningsfall för hello ReliableConcurrentQueue är hello [meddelandekö](https://en.wikipedia.org/wiki/Message_queue) scenario. I det här scenariot en eller flera meddelandeproducenter skapa och lägga till objekt toohello kö och en eller flera meddelandet konsumenter pull-meddelanden från kön hello och bearbeta dem.. Flera producenter och konsumenter fungerar oberoende av varandra, med hjälp av samtidiga transaktioner i ordning tooprocess hello kö.

## <a name="usage-guidelines"></a>Riktlinjer för användning
* hello kön förväntar sig att hello objekt i hello kön har en låg Bevarandeperiod. Som är hello objekt skulle inte kvar i hello kö för länge.
* hello kön garanterar inte strikt FIFO ordning.
* hello kön läser inte sin egen skrivningar. Om ett objekt i kö i en transaktion, kommer inte att visas tooa dequeuer inom hello samma transaktion.
* Dequeues inte är isolerade från varandra. Om objektet *A* har tagits bort i transaktion *txnA*, även om *txnA* är därmed inte verkställas objektet *A* inte är synliga tooa samtidiga transaktionen *txnB*.  Om *txnA* avbryts, *A* ska vara synliga för*txnB* omedelbart.
* *TryPeekAsync* beteende kan implementeras med hjälp av en *TryDequeueAsync* och sedan avbryter hello transaktionen. Ett exempel på detta finns i hello Programming mönster avsnitt.
* Antalet är icke-transaktionell. Det kan vara används tooget en uppfattning om hello antalet element i hello kön, men representerar point-in-time och kan inte förlita sig på.
* Billigare bearbetning på hello togs bort från kön objekt bör inte utföras medan hello transaktion är aktiv, tooavoid långvariga transaktioner som kan påverka prestanda på hello system.

## <a name="code-snippets"></a>Kodstycken
Låt oss titta på några kodstycken och deras förväntade produktion. Undantagshantering ignoreras i det här avsnittet.

### <a name="enqueueasync"></a>EnqueueAsync
Här följer några kodstycken för att använda EnqueueAsync följt av de förväntade utdata.

- *Fall 1: Enkel köa aktiviteten*

```
using (var txn = this.StateManager.CreateTransaction())
{
    await this.Queue.EnqueueAsync(txn, 10, cancellationToken);
    await this.Queue.EnqueueAsync(txn, 20, cancellationToken);

    await txn.CommitAsync();
}
```

Anta att hello uppgiften har slutförts och som det finns inga samtidiga transaktioner ändra hello kön. hello användare kan förvänta sig hello köobjekt toocontain hello i något av följande order hello:

> 10, 20

> 20, 10


- *Fall 2: Parallell köa aktiviteten*

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

Anta att hello aktiviteter har slutförts, att hello uppgifter kördes parallellt och att det inte finns några andra samtidiga transaktioner ändra hello kön. Inga härledning kan göras om hello ordningen på objekten i hello kön. För det här kodstycket kan hello objekt visas i någon av hello 4! möjliga ordningar.  hello kön försöker tookeep hello poster i en hello ursprungliga (köas), men kan vara framtvingad tooreorder dem på grund av tooconcurrent åtgärder eller fel.


### <a name="dequeueasync"></a>DequeueAsync
Här följer några kodstycken för att använda TryDequeueAsync följt av hello förväntades utdata. Anta hello kön redan är ifyllda hello följande objekt i kö för hello:
> 10, 20, 30, 40, 50, 60

- *Fall 1: En har status Created aktivitet*

```
using (var txn = this.StateManager.CreateTransaction())
{
    await this.Queue.TryDequeueAsync(txn, cancellationToken);
    await this.Queue.TryDequeueAsync(txn, cancellationToken);
    await this.Queue.TryDequeueAsync(txn, cancellationToken);

    await txn.CommitAsync();
}
```

Anta att hello uppgiften har slutförts och som det finns inga samtidiga transaktioner ändra hello kön. Eftersom ingen härledning kan göras om hello ordning hello artiklar i hello kö, alla tre hello-objekt kan vara togs bort från kön, i vilken ordning som helst. hello kön försöker tookeep hello poster i en hello ursprungliga (köas), men kan vara framtvingad tooreorder dem på grund av tooconcurrent åtgärder eller fel.  

- *Fall 2: Parallell status Created aktivitet*

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

Anta att hello aktiviteter har slutförts, att hello uppgifter kördes parallellt och att det inte finns några andra samtidiga transaktioner ändra hello kön. Eftersom ingen härledning kan göras om hello ordning hello artiklar i hello kö, hello listor *dequeue1* och *dequeue2* varje innehåller två objekt, i vilken ordning som helst.

hello samma artikel kommer *inte* visas i båda listorna. Därför om dequeue1 har *10*, *30*, dequeue2 skulle ha *20*, *40*.

- *Fall 3: Status Created ordning med Transaktionsavbrott*

Avbryter en transaktion med pågående dequeues placeringar hello objekt tillbaka på hello huvud hello kön. hello ordning som hello objekt är lägga tillbaka i hello huvud hello kön är inte säkert. Låt oss titta på hello följande kod:

```
using (var txn = this.StateManager.CreateTransaction())
{
    await this.Queue.TryDequeueAsync(txn, cancellationToken);
    await this.Queue.TryDequeueAsync(txn, cancellationToken);

    // Abort hello transaction
    await txn.AbortAsync();
}
```
Anta att hello-objekt har tagits bort i hello följande ordning:
> 10, 20

När vi avbryta hello transaktion skulle hello objekt läggas till bakre toohello head hello köns i något av följande order hello:
> 10, 20

> 20, 10

hello samma sak gäller för alla fall där hello transaktionen inte har *genomförd*.

## <a name="programming-patterns"></a>Mönster för programmering
I det här avsnittet tittar vi på några programmering mönster som kan vara användbart i med hjälp av ReliableConcurrentQueue.

### <a name="batch-dequeues"></a>Batch Dequeues
A rekommenderas programming mönstret är för hello konsumenten uppgiften toobatch dess dequeues i stället för att utföra en i taget har status Created. hello användaren kan välja toothrottle fördröjningar mellan varje batch eller hello batchstorlek. hello visar följande kodavsnitt den här programmeringsmodell.  Observera att i det här exemplet hello bearbetning görs efter hello transaktionen är genomförd, så om ett fel toooccur vid bearbetning, hello obearbetat objekt försvinner utan har bearbetats.  Alternativt hello bearbetning kan göras inom hello transaktions-scope, men detta kan ha en negativ inverkan på prestanda och kräver hanteringen av hello artiklar redan bearbetats.

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

### <a name="best-effort-notification-based-processing"></a>Bästa meddelandebaserad bearbetning
En annan intressant programming mönster använder hello antal API. Här kan vi implementera bästa meddelandebaserad bearbetning för hello kö. hello kön antalet kan vara används toothrottle en sätta eller en dequeue aktivitet.  Observera att som i föregående exempel hello eftersom hello bearbetningen sker utanför hello transaktion obearbetat objekt kan gå förlorade om ett fel uppstår under bearbetningen.

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

### <a name="best-effort-drain"></a>Bästa tömning
Kan inte garanteras tömning av hello kö på grund av toohello samtidiga uppbyggnad hello-datastrukturen.  Det är möjligt att även om inga användare åtgärder på hello kön finns relä, en viss anropet tooTryDequeueAsync inte kan returnera ett objekt som tidigare var köas och bekräftats.  hello köas objektet garanteras för*slutligen* bli synliga toodequeue men utan en mekanism för out-of-band-kommunikation, ett oberoende konsumenten inte kan vet hello kön har uppnått ett stabilt tillstånd även om alla producenter har stoppats och inga nya sätta tillåts. Därför är hello tömning åtgärden bästa som implementeras nedan.

hello användaren ska stoppa alla ytterligare producenten och konsumentuppgifter och vänta tills alla pågående transaktioner toocommit eller Avbryt innan du försöker toodrain hello kön.  Om hello användaren känner hello förväntat antal objekt i kö hello kan skapa de ett meddelande som signalerar till att alla objekt har har tagits bort.

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

### <a name="peek"></a>Granska
ReliableConcurrentQueue ger inte hello *TryPeekAsync* api. Användare kan få hello titt semantiska med hjälp av en *TryDequeueAsync* och sedan avbryter hello transaktionen. I det här exemplet dequeues bearbetas bara om hello objektets värde är större än *10*.

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

## <a name="must-read"></a>Måste läsa
* [Snabbstart för Reliable Services](service-fabric-reliable-services-quick-start.md)
* [Arbeta med Reliable Collections](service-fabric-work-with-reliable-collections.md)
* [Reliable Services-meddelanden](service-fabric-reliable-services-notifications.md)
* [Reliable Services säkerhetskopiering och återställning (Disaster Recovery)](service-fabric-reliable-services-backup-restore.md)
* [Konfiguration av tillförlitliga tillstånd Manager](service-fabric-reliable-services-configuration.md)
* [Komma igång med Service Fabric Web API-tjänster](service-fabric-reliable-services-communication-webapi.md)
* [Avancerad användning av hello programmeringsmodellen i Reliable Services](service-fabric-reliable-services-advanced-usage.md)
* [För utvecklare för tillförlitlig samlingar](https://msdn.microsoft.com/library/azure/microsoft.servicefabric.data.collections.aspx)
