---
title: "aaaBest metoder för att förbättra prestanda med hjälp av Azure Service Bus | Microsoft Docs"
description: Beskriver hur toouse Service Bus toooptimize prestanda vid utbyte av asynkrona meddelanden.
services: service-bus-messaging
documentationcenter: na
author: sethmanheim
manager: timlt
editor: 
ms.assetid: e756c15d-31fc-45c0-8df4-0bca0da10bb2
ms.service: service-bus-messaging
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 05/10/2017
ms.author: sethm
ms.openlocfilehash: 52764d227757cbb11246675878933f21685817f1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="best-practices-for-performance-improvements-using-service-bus-messaging"></a>Metodtips för bättre prestanda med hjälp av Service Bus-meddelanden

Den här artikeln beskriver hur toouse [Azure Service Bus-meddelanden](https://azure.microsoft.com/services/service-bus/) toooptimize prestanda vid utbyte av asynkrona meddelanden. hello första delen av det här avsnittet beskriver hello olika metoder som erbjuds toohelp ökade prestanda. hello andra delen innehåller råd om hur toouse Service Bus på ett sätt som kan erbjuda hello bästa prestanda i ett visst scenario.

I det här avsnittet refererar hello termen ”klient” tooany entitet som har åtkomst till Service Bus. En klient kan ta hello roll för en avsändare eller en mottagare. hello termen ”avsändare” används för en Service Bus kö eller ett ämne-klient som skickar meddelanden tooa Service Bus-kö eller ett ämne. hello termen ”mottagaren” refererar tooa Service Bus kö eller prenumeration klient som tar emot meddelanden från en Service Bus-kö eller prenumeration.

Dessa avsnitt introducerar några koncept som Service Bus använder toohelp ökar prestanda.

## <a name="protocols"></a>Protokoll
Service Bus gör att klienter toosend och ta emot meddelanden via ett av tre protokoll:

1. Avancerade Message Queuing-protokollet (AMQP)
2. Service Bus-meddelanden Protocol (SBMP)
3. HTTP

AMQP och SBMP är effektivare, eftersom de bevarar hello anslutning tooService Bus länge hello meddelandefabrik finns. Den implementerar batchbearbetning och förhämtar. Om du inte uttryckligen anges förutsätter allt innehåll i det här avsnittet hello användning av AMQP eller SBMP.

## <a name="reusing-factories-and-clients"></a>Återanvända fabriker och klienter
Service Bus klienten objekt, till exempel [QueueClient] [ QueueClient] eller [MessageSender][MessageSender], skapas via en [ MessagingFactory] [ MessagingFactory] -objektet, vilket ger också intern hantering av anslutningar. Du bör inte stänga meddelanden fabriker eller kön, ämnet och prenumerationen klienter när du skickar ett meddelande och sedan återskapa dem när du skickar nästa hello-meddelande. Stänga en meddelandefabrik tar bort hello anslutning toohello Service Bus-tjänst och en ny anslutning upprättas när återskapa hello fabriken. Upprätta en anslutning är en kostsam åtgärd som att du slipper med nytt hello samma fabriken och klienten objekt flera åtgärder. Du kan på ett säkert sätt använder hello [QueueClient] [ QueueClient] objekt för att skicka meddelanden från samtidiga asynkrona åtgärder och flera trådar. 

## <a name="concurrent-operations"></a>Samtidiga åtgärder
Utföra en åtgärd (skicka, ta emot, ta bort, etc.) tar en stund. Nu innehåller hello bearbetningen av hello åtgärden av hello Service Bus-tjänst i tillägg toohello svarstiden för hello och hello svarsmeddelanden. tooincrease hello antalet åtgärder per tid, åtgärder måste köras samtidigt. Du kan göra detta på flera olika sätt:

* **Asynkrona åtgärder**: hello klienten schemalägger åtgärder genom att utföra asynkrona åtgärder. hello nästa begäran har startats innan hello förra begäran har slutförts. hello följande är ett exempel på en asynkron sändningsåtgärd:
  
 ```csharp
  BrokeredMessage m1 = new BrokeredMessage(body);
  BrokeredMessage m2 = new BrokeredMessage(body);
  
  Task send1 = queueClient.SendAsync(m1).ContinueWith((t) => 
    {
      Console.WriteLine("Sent message #1");
    });
  Task send2 = queueClient.SendAsync(m2).ContinueWith((t) => 
    {
      Console.WriteLine("Sent message #2");
    });
  Task.WaitAll(send1, send2);
  Console.WriteLine("All messages sent");
  ```
  
  Detta är ett exempel på en asynkron mottagning igen:
  
  ```csharp
  Task receive1 = queueClient.ReceiveAsync().ContinueWith(ProcessReceivedMessage);
  Task receive2 = queueClient.ReceiveAsync().ContinueWith(ProcessReceivedMessage);
  
  Task.WaitAll(receive1, receive2);
  Console.WriteLine("All messages received");
  
  async void ProcessReceivedMessage(Task<BrokeredMessage> t)
  {
    BrokeredMessage m = t.Result;
    Console.WriteLine("{0} received", m.Label);
    await m.CompleteAsync();
    Console.WriteLine("{0} complete", m.Label);
  }
  ```
* **Flera fabriker**: alla klienter (avsändare i tillägget tooreceivers) som skapas av hello samma factory dela en TCP-anslutning. hello maximala genomströmningen begränsas av hello antal åtgärder som kan gå igenom den här TCP-anslutningen. hello dataflödet som kan hämtas med en enda fabrik varierar kraftigt med TCP fram och åter gånger och meddelandestorlek. tooobtain högre hastigheter, bör du använda flera meddelanden fabriker.

## <a name="receive-mode"></a>Mottagningsläge
När du skapar en kö eller prenumeration klient kan du ange en receive-läge: *titt Lås* eller *ta emot och ta bort*. hello standard mottagningsläge är [PeekLock][PeekLock]. När det här läget kan hello klient skickar en begäran tooreceive ett meddelande från Service Bus. När hello-klienten har tagit emot hello-meddelande, skickar en begäran toocomplete hello-meddelande.

När inställningen hello mottagningsläge för[ReceiveAndDelete][ReceiveAndDelete], båda stegen kombineras i en enskild begäran. Detta minskar hello övergripande antal åtgärder och kan förbättra hello totala genomströmningen. Den här prestandafördelar levereras hello risken för att förlora meddelanden.

Service Bus stöder inte transaktioner för ta emot och delete-åtgärder. Dessutom titt Lås semantik krävs för alla scenarier i vilka hello klient vill toodefer eller [förlorade](service-bus-dead-letter-queues.md) ett meddelande.

## <a name="client-side-batching"></a>Klientsidans batchbearbetning
Klientsidans batchbearbetning kan en kö eller avsnittet klienten toodelay hello skickas ett meddelande för en viss tidsperiod. Om hello klienten skickar ytterligare meddelanden under den här tiden, överför hälsningsmeddelande i en enskild batch. Klientsidans batchbearbetning gör att en kö eller prenumeration klienten toobatch flera **Slutför** begäranden till en enskild begäran. Batchbearbetning är endast tillgängligt för asynkron **skicka** och **Slutför** åtgärder. Synkrona åtgärder skickas omedelbart toohello Service Bus-tjänst. Batchbearbetning inte ske för titt eller ta emot operations eller batchbearbetning sker över klienter.

Som standard använder en klient en batch-intervallet för 20ms. Du kan ändra hello batch intervall genom att ange hello [BatchFlushInterval] [ BatchFlushInterval] egenskapen innan du skapar hello meddelandefabrik. Den här inställningen påverkar alla klienter som har skapats med den här fabriken.

toodisable batchbearbetning, ange hello [BatchFlushInterval] [ BatchFlushInterval] egenskapen för**TimeSpan.Zero**. Exempel:

```csharp
MessagingFactorySettings mfs = new MessagingFactorySettings();
mfs.TokenProvider = tokenProvider;
mfs.NetMessagingTransportSettings.BatchFlushInterval = TimeSpan.FromSeconds(0.05);
MessagingFactory messagingFactory = MessagingFactory.Create(namespaceUri, mfs);
```

Batchbearbetning påverkar inte hello antal fakturerbar asynkrona åtgärder och är bara tillgängligt för hello klientprotokoll för Service Bus. hello HTTP-protokollet har inte stöd för batchbearbetning.

## <a name="batching-store-access"></a>Batchbearbetning store-åtkomst
tooincrease hello genomflödet i kön, ämnet och prenumerationen, Service Bus batchar flera meddelanden vid skrivning tooits interna store. Om aktiverad på en kö eller ett ämne, kommer det att batchar att skriva meddelanden till hello-arkivet. Om aktiverad på en kö eller en prenumeration, kommer det att batchar att ta bort meddelanden från hello store. Om gruppbaserad store-åtkomst är aktiverat för en entitet, fördröjningar Service Bus store skrivning om enheten genom att konfigurera too20ms. Ytterligare store-åtgärder som inträffar under det här intervallet läggs toohello batch. Batchar store åtkomst endast påverkar **skicka** och **Slutför** verksamhet, ta emot påverkas inte. Batch store-åtkomst är en egenskap för en entitet. Batchbearbetning sker över alla enheter som möjliggör gruppbaserad store-åtkomst.

När du skapar en ny kö, ämne eller en prenumeration kan är batch store-åtkomst aktiverat som standard. toodisable batchar store-åtkomst, ange hello [EnableBatchedOperations] [ EnableBatchedOperations] egenskapen för**FALSKT** innan du skapar hello entitet. Exempel:

```csharp
QueueDescription qd = new QueueDescription();
qd.EnableBatchedOperations = false;
Queue q = namespaceManager.CreateQueue(qd);
```

Batch store-åtkomst påverkar inte hello antal fakturerbar asynkrona åtgärder och är en egenskap för kön, ämnet och prenumerationen. Det är oberoende av hello får läge och hello protokoll som används mellan en klient och hello Service Bus-tjänst.

## <a name="prefetching"></a>Förhämtar
Förhämtar kan hello kö eller prenumeration klienten tooload ytterligare meddelanden från hello-tjänsten när en receive-åtgärd utförs. hello klienten lagrar dessa meddelanden i ett lokalt cacheminne. hello hello cachens storlek bestäms av hello [QueueClient.PrefetchCount] [ QueueClient.PrefetchCount] eller [SubscriptionClient.PrefetchCount] [ SubscriptionClient.PrefetchCount] Egenskaper. Varje klient som möjliggör förhämtar upprätthåller sin egen cache. En cache delas inte mellan klienter. Om hello klienten startar en receive-åtgärd och dess cachen är tom, överför hello-tjänsten en grupp med meddelanden. hello storleken på hello batch är lika med hello storleken på cacheminnet hello eller 256 KB, beroende på vilket som är mindre. Om hello klienten startar en receive-åtgärd och hello cachen innehåller ett meddelande tas hello-meddelande från hello cache.

När ett meddelande är prefetched service Lås hello prefetched hälsningsmeddelande. På så sätt kan inte prefetched hello-meddelande tas emot av en annan mottagare. Om hello mottagaren inte kan slutföra hello-meddelande innan hello Lås upphör att gälla, blir tillgängliga tooother mottagare hello-meddelande. Hej prefetched kopia av hello-meddelande är kvar i hello cache. hello mottagare som förbrukar hello upphört att gälla cachelagrad kopia får ett undantag när den försöker toocomplete meddelandet. Som standard hello meddelandet Lås upphör att gälla efter 60 sekunder. Det här värdet kan vara utökade too5 minuter. tooprevent hello förbrukning av meddelanden som har upphört att gälla, hello cachestorleken bör alltid att vara mindre än hello antal meddelanden som kan användas av en klient i hello lock timeout-intervall.

När du använder hello standard Låsets upphörande 60 sekunder, en bra värde för [SubscriptionClient.PrefetchCount] [ SubscriptionClient.PrefetchCount] är 20 gånger hello maximala bearbetning av alla mottagare av hello fabriken. Till exempel en fabrik skapar 3 mottagare, och varje mottagare kan bearbeta upp too10 meddelanden per sekund. hello prefetch antal får inte överstiga 20 X 3 X 10 = 600. Som standard [QueueClient.PrefetchCount] [ QueueClient.PrefetchCount] är uppsättningen too0, vilket innebär att inga fler meddelanden hämtas från hello-tjänsten.

Förhämtar meddelanden ökar hello totala genomflödet för en kö eller en prenumeration eftersom det reducerar hello övergripande antal meddelandeåtgärder eller sändningar. Hämtar första hello-meddelande, men tar längre tid (på grund av ökade toohello meddelandestorlek). Ta emot prefetched meddelanden är snabbare eftersom dessa meddelanden redan har hämtats av hello-klienten.

hello time to live (TTL)-egenskapen för ett meddelande kontrolleras av hello-servern för närvarande hello hello servern skickar hello message toohello klient. hello klienten kontrollerar inte hello-meddelande-TTL egenskapen när hello-meddelande tas emot. Hello-meddelande kan i stället tas emot även om hello-meddelande-TTL har passerat medan hello-meddelande cachelagrades av hello-klienten.

Förhämtar påverkar inte hello antal fakturerbar asynkrona åtgärder och är bara tillgängligt för hello klientprotokoll för Service Bus. hello HTTP-protokollet stöder inte förhämtar. Förhämtar är tillgänglig för både synkrona och asynkrona får åtgärder.

## <a name="express-queues-and-topics"></a>Express köer och ämnen

Expressenheter aktivera högt genomflöde och svarstid för minskade scenarier och stöds endast i hello meddelanden standardnivån. Entiteter som skapats i [Premium-namnområden](service-bus-premium-messaging.md) inte stöder hello express alternativ. Med expressenheter, om ett meddelande skickas tooa kö eller ett ämne, lagras hello-meddelande omedelbart inte i hello meddelandearkiv. I stället cachelagras i minnet. Om ett meddelande finns kvar i hello kön i mer än ett par sekunder, skrivs automatiskt toostable lagring, vilket skyddar mot dataförlust på grund av tooan avbrott. Skriva hello-meddelande till en minnescache ökar dataflödet och minskar latens eftersom det inte finns några åtkomst toostable skickas lagringsutrymme på hello tid hello-meddelande. Meddelanden som används inom några sekunder skrivs inte toohello messaging store. hello följande exempel skapar en uttrycklig avsnittet.

```csharp
TopicDescription td = new TopicDescription(TopicName);
td.EnableExpress = true;
namespaceManager.CreateTopic(td);
```

Om ett meddelande som innehåller viktig information som inte får vara förlorade skickas tooan express entiteten, hello avsändaren kan tvinga Service Bus tooimmediately upprätthålla hello meddelandet toostable lagring genom att ange hello [ForcePersistence] [ ForcePersistence] egenskapen för**SANT**.

> [!NOTE]
> Expressenheter stöder inte transaktioner.

## <a name="use-of-partitioned-queues-or-topics"></a>Användning av partitionerade köer och ämnen
Internt, Service Bus använder hello samma nod och meddelanden store tooprocess och lagra alla meddelanden för en meddelandeentitet (kö eller ett ämne). En partitionerad kö eller ett ämne, på hello däremot distribueras över flera noder och meddelandearkiv. Partitionerade köer och ämnen inte bara att ge en bättre genomströmning än vanliga köer och ämnen, de också uppvisar en högre tillgänglighet. toocreate en partitionerad entitet set hello [EnablePartitioning] [ EnablePartitioning] egenskapen för**SANT**som visas i följande exempel hello. Mer information om partitionerade enheter finns [partitionerade meddelandeentiteter][Partitioned messaging entities].

```csharp
// Create partitioned queue.
QueueDescription qd = new QueueDescription(QueueName);
qd.EnablePartitioning = true;
namespaceManager.CreateQueue(qd);
```

## <a name="use-of-multiple-queues"></a>Användning av flera köer

Om det inte är möjligt toouse en partitionerad kö eller avsnittet eller hello förväntad belastning inte kan hanteras av en enskild partitionerade kö eller ett ämne, måste du använda flera meddelandeentiteter. När du använder flera enheter, skapa en dedikerad klient för varje entitet, istället för att använda hello samma klienten för alla entiteter.

## <a name="development-and-testing-features"></a>Utveckling och testning funktioner

Service Bus har en funktion som används för utveckling som **ska aldrig användas i produktion konfigurationer**: [TopicDescription.EnableFilteringMessagesBeforePublishing][].

När nya regler eller filter läggs toohello avsnittet, kan du använda [TopicDescription.EnableFilteringMessagesBeforePublishing][] tooverify som hello nya filteruttrycket fungerar som förväntat.

## <a name="scenarios"></a>Scenarier
hello följande avsnitt beskrivs vanliga meddelandehantering och beskriver hello önskad Service Bus-inställningar. Hastigheter klassificeras så liten (mindre än 1 meddelande per sekund), Måttlig (1-meddelande per sekund eller större men mindre än 100 meddelanden per sekund) och hög (100 meddelanden/andra eller senare). hello antalet klienter som är klassificerade som liten (5 eller färre), Måttlig (mer än 5 mindre än eller lika med too20), och stora (mer än 20).

### <a name="high-throughput-queue"></a>Hög genomströmning kön
Mål: Maximera hello genomströmningen i en enskild kö. hello antal avsändare och mottagare är liten.

* Använda en partitionerad kö för bättre prestanda och tillgänglighet.
* tooincrease hello övergripande överföringskö i hello kö kan använda flera fabriker toocreate avsändare. Använd asynkrona åtgärder eller flera trådar för varje avsändare.
* tooincrease hello övergripande mottagningshastighet från hello kön kan använda flera meddelandet fabriker toocreate mottagare.
* Använd asynkrona åtgärder tootake nytta av klientsidan batchbearbetning.
* Ange hello batchbearbetning intervall too50ms tooreduce hello antalet överföringar för Service Bus-klient-protokollet. Om du använder flera avsändare, öka hello batchbearbetning too100ms intervall.
* Lämna gruppbaserad store-åtkomst aktiverat. Detta ökar hello övergripande hastighet som meddelanden kan skrivas i hello kö.
* Ange hello prefetch antal too20 tider hello maximala hastighetsprestanda av alla mottagare av ett fabriken. Detta minskar hello antalet överföringar för Service Bus-klient-protokollet.

### <a name="multiple-high-throughput-queues"></a>Flera hög genomströmning köer
Mål: Maximera totala genomflödet i flera köer. hello genomströmningen i en enskild kö är måttlig eller hög.

tooobtain maximalt dataflöde i flera köer, Använd hello inställningar beskrivs toomaximize hello genomströmningen i en enskild kö. Dessutom kan använda olika fabriker toocreate klienter som skicka eller ta emot från olika köer.

### <a name="low-latency-queue"></a>Låg latens kön
Mål: Minimera svarstiden för slutpunkt till slutpunkt för en kö eller ett ämne. hello antal avsändare och mottagare är liten. hello genomflödet i hello kön är liten eller Måttlig.

* Använda en partitionerad kö för förbättrad tillgänglighet.
* Inaktivera klientens batchbearbetning. hello klient skickar ett meddelande direkt.
* Inaktivera batch store-åtkomst. hello service skriver omedelbart hello toohello meddelandearkiv.
* Om du använder en enskild klient ange hello prefetch antal too20 gånger hello behandlingstakten för hello mottagare. Om flera meddelanden anländer till hello kö på hello samma tid, hello Service Bus klientprotokoll överför dem på hello samtidigt. När hello klienten tar emot nästa hello-meddelande, finns meddelandet redan i hello lokalt cacheminne. hello cache ska vara små.
* Om du använder flera klienter, ange hello prefetch antal too0. Genom att göra får andra hello-klient andra hello-meddelande medan hello första klienten fortfarande bearbetar första hello-meddelande.

### <a name="queue-with-a-large-number-of-senders"></a>Kö med ett stort antal avsändare
Mål: Maximera genomströmningen i en kö eller ett ämne med ett stort antal avsändare. Varje avsändare skickar meddelanden med en måttlig hastighet. hello antal mottagare är liten.

Service Bus kan in too1000 samtidiga anslutningar tooa messaging entitet (eller 5000 använder AMQP). Den här gränsen tillämpas på hello namnområde nivå och köer-avsnitt-prenumerationer är begränsad av hello antalet samtidiga anslutningar per namnområde. För köer delas antalet mellan avsändare och mottagare. Om alla 1000 anslutningar krävs för avsändare, bör du ersätta hello kö med ett ämne och en enda prenumeration. Ett ämne accepterar too1000 samtidiga anslutningar från avsändare, medan hello prenumerationen accepterar en ytterligare 1000 samtidiga anslutningar från mottagare. Om fler än 1000 samtidiga avsändare krävs skicka hello avsändare meddelanden toohello Service Bus-protokollet via HTTP.

toomaximize genomströmning hello följande:

* Använda en partitionerad kö för bättre prestanda och tillgänglighet.
* Om varje avsändare finns i en annan process, kan du använda en enda fabrik processer.
* Använd asynkrona åtgärder tootake nytta av klientsidan batchbearbetning.
* Använd hello standard batchbearbetning intervall 20ms tooreduce hello antalet överföringar för Service Bus-klient-protokollet.
* Lämna gruppbaserad store-åtkomst aktiverat. Detta ökar hello övergripande hastighet med vilken meddelanden kan skrivas till hello kö eller ett ämne.
* Ange hello prefetch antal too20 tider hello maximala hastighetsprestanda av alla mottagare av ett fabriken. Detta minskar hello antalet överföringar för Service Bus-klient-protokollet.

### <a name="queue-with-a-large-number-of-receivers"></a>Kö med ett stort antal mottagare
Mål: Maximera hello mottagningshastighet för en kö eller en prenumeration med ett stort antal mottagare. Varje mottagare tar emot meddelanden med en måttlig hastighet. hello antal avsändare är liten.

Service Bus gör dig too1000 samtidiga anslutningar tooan entitet. Om en kö kräver mer än 1000 mottagare, bör du ersätta hello kö med ett ämne och flera prenumerationer. Varje prenumeration stöder too1000 samtidiga anslutningar. Mottagare kan också komma åt hello kön via hello HTTP-protokollet.

toomaximize genomströmning hello följande:

* Använda en partitionerad kö för bättre prestanda och tillgänglighet.
* Om varje mottagare finns i en annan process, kan du använda en enda fabrik processer.
* Mottagare kan använda synkron eller asynkron åtgärder. Angivna hello måttlig mottagningshastighet för en enskild mottagare, klientsidan massbearbetning av en fullständig begäran påverkar inte mottagaren genomflöde.
* Lämna gruppbaserad store-åtkomst aktiverat. Detta minskar hello allmänna belastningen på hello entitet. Det minskar också hello övergripande hastighet med vilken meddelanden kan skrivas till hello kö eller ett ämne.
* Ange hello prefetch antal tooa lågt värde (till exempel PrefetchCount = 10). Detta förhindrar att mottagarna inaktivitet medan andra mottagare har stora mängder meddelanden som cachelagras.

### <a name="topic-with-a-small-number-of-subscriptions"></a>Avsnittet med ett litet antal prenumerationer
Mål: Maximera hello genomflödet i ett ämne med ett litet antal prenumerationer. Ett meddelande tas emot av många prenumerationer, vilket innebär att hello kombineras får frekvens över alla prenumerationer som är större än hello överföringshastighet. hello antal avsändare är liten. hello antal mottagare per prenumeration är liten.

toomaximize genomströmning hello följande:

* Använd en partitionerad avsnittet för bättre prestanda och tillgänglighet.
* tooincrease hello övergripande överföringskö till hello ämne kan använda flera fabriker toocreate avsändare. Använd asynkrona åtgärder eller flera trådar för varje avsändare.
* tooincrease hello övergripande mottagningshastighet från en prenumeration kan använda flera meddelandet fabriker toocreate mottagare. Använda asynkrona åtgärder eller flera trådar för varje mottagare.
* Använd asynkrona åtgärder tootake nytta av klientsidan batchbearbetning.
* Använd hello standard batchbearbetning intervall 20ms tooreduce hello antalet överföringar för Service Bus-klient-protokollet.
* Lämna gruppbaserad store-åtkomst aktiverat. Detta ökar hello övergripande hastighet som meddelanden kan skrivas till hello ämne.
* Ange hello prefetch antal too20 tider hello maximala hastighetsprestanda av alla mottagare av ett fabriken. Detta minskar hello antalet överföringar för Service Bus-klient-protokollet.

### <a name="topic-with-a-large-number-of-subscriptions"></a>Avsnittet med ett stort antal prenumerationer
Mål: Maximera hello genomflödet i ett ämne med ett stort antal prenumerationer. Ett meddelande tas emot av många prenumerationer, vilket innebär att hello kombineras får frekvens över alla prenumerationer är mycket större än hello överföringshastighet. hello antal avsändare är liten. hello antal mottagare per prenumeration är liten.

Avsnitt med ett stort antal prenumerationer exponera en låg totala genomflödet vanligtvis om alla meddelanden dirigeras tooall prenumerationer. Detta beror på hello faktum att varje meddelande tas emot många gånger, och alla meddelanden som finns i ett ämne och alla dess prenumerationer lagras i hello lagra samma. Det förutsätts att hello antalet avsändare och mottagare per prenumeration antal är litet. Service Bus stöder upp too2 000 prenumerationer per avsnitt.

toomaximize genomströmning hello följande:

* Använd en partitionerad avsnittet för bättre prestanda och tillgänglighet.
* Använd asynkrona åtgärder tootake nytta av klientsidan batchbearbetning.
* Använd hello standard batchbearbetning intervall 20ms tooreduce hello antalet överföringar för Service Bus-klient-protokollet.
* Lämna gruppbaserad store-åtkomst aktiverat. Detta ökar hello övergripande hastighet som meddelanden kan skrivas till hello ämne.
* Ange hello prefetch antal too20 tider hello förväntas ta emot frekvens i sekunder. Detta minskar hello antalet överföringar för Service Bus-klient-protokollet.

## <a name="next-steps"></a>Nästa steg
toolearn mer information om hur du optimerar prestanda för Service Bus finns [partitionerade meddelandeentiteter][Partitioned messaging entities].

[QueueClient]: /dotnet/api/microsoft.servicebus.messaging.queueclient
[MessageSender]: /dotnet/api/microsoft.servicebus.messaging.messagesender
[MessagingFactory]: /dotnet/api/microsoft.servicebus.messaging.messagingfactory
[PeekLock]: /dotnet/api/microsoft.servicebus.messaging.receivemode
[ReceiveAndDelete]: /dotnet/api/microsoft.servicebus.messaging.receivemode
[BatchFlushInterval]: /dotnet/api/microsoft.servicebus.messaging.netmessagingtransportsettings.batchflushinterval#Microsoft_ServiceBus_Messaging_NetMessagingTransportSettings_BatchFlushInterval
[EnableBatchedOperations]: /dotnet/api/microsoft.servicebus.messaging.queuedescription.enablebatchedoperations#Microsoft_ServiceBus_Messaging_QueueDescription_EnableBatchedOperations
[QueueClient.PrefetchCount]: /dotnet/api/microsoft.servicebus.messaging.queueclient.prefetchcount#Microsoft_ServiceBus_Messaging_QueueClient_PrefetchCount
[SubscriptionClient.PrefetchCount]: /dotnet/api/microsoft.servicebus.messaging.subscriptionclient.prefetchcount#Microsoft_ServiceBus_Messaging_SubscriptionClient_PrefetchCount
[ForcePersistence]: /dotnet/api/microsoft.servicebus.messaging.brokeredmessage.forcepersistence#Microsoft_ServiceBus_Messaging_BrokeredMessage_ForcePersistence
[EnablePartitioning]: /dotnet/api/microsoft.servicebus.messaging.queuedescription.enablepartitioning#Microsoft_ServiceBus_Messaging_QueueDescription_EnablePartitioning
[Partitioned messaging entities]: service-bus-partitioning.md
[TopicDescription.EnableFilteringMessagesBeforePublishing]: /dotnet/api/microsoft.servicebus.messaging.topicdescription.enablefilteringmessagesbeforepublishing#Microsoft_ServiceBus_Messaging_TopicDescription_EnableFilteringMessagesBeforePublishing
