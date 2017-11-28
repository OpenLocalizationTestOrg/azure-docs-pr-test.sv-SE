---
title: "Översikt över transaktionsbearbetning i Azure Service Bus | Microsoft Docs"
description: "Översikt över Azure Service Bus atomiska transaktioner och skicka via"
services: service-bus-messaging
documentationcenter: .net
author: sethmanheim
manager: timlt
editor: 
ms.assetid: 64449247-1026-44ba-b15a-9610f9385ed8
ms.service: service-bus-messaging
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 05/17/2017
ms.author: clemensv;sethm
ms.openlocfilehash: a88f2d81ab43e38c9363a67aaefc178b47bfb259
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="overview-of-service-bus-transaction-processing"></a><span data-ttu-id="d0308-103">Översikt över Service Bus transaktionsbearbetning</span><span class="sxs-lookup"><span data-stu-id="d0308-103">Overview of Service Bus transaction processing</span></span>
<span data-ttu-id="d0308-104">Den här artikeln beskriver funktionerna i Azure Service Bus transaktion.</span><span class="sxs-lookup"><span data-stu-id="d0308-104">This article discusses the transaction capabilities of Azure Service Bus.</span></span> <span data-ttu-id="d0308-105">Mycket av diskussionen visas den [atomiska transaktioner med Service Bus-exempel](https://github.com/Azure/azure-service-bus/tree/master/samples/DotNet/Microsoft.ServiceBus.Messaging/AtomicTransactions).</span><span class="sxs-lookup"><span data-stu-id="d0308-105">Much of the discussion is illustrated by the [Atomic Transactions with Service Bus sample](https://github.com/Azure/azure-service-bus/tree/master/samples/DotNet/Microsoft.ServiceBus.Messaging/AtomicTransactions).</span></span> <span data-ttu-id="d0308-106">Den här artikeln är begränsad till en översikt över transaktionsbearbetning och *skicka via* funktion i Service Bus, medan atomiska transaktioner exempel större och mer komplexa omfång.</span><span class="sxs-lookup"><span data-stu-id="d0308-106">This article is limited to an overview of transaction processing and the *send via* feature in Service Bus, while the Atomic Transactions sample is broader and more complex in scope.</span></span>

## <a name="transactions-in-service-bus"></a><span data-ttu-id="d0308-107">Transaktioner i Service Bus</span><span class="sxs-lookup"><span data-stu-id="d0308-107">Transactions in Service Bus</span></span>
<span data-ttu-id="d0308-108">En [transaktion](https://github.com/Azure/azure-service-bus/tree/master/samples/DotNet/Microsoft.ServiceBus.Messaging/AtomicTransactions#what-are-transactions) för att gruppera två eller flera åtgärder i en *körningen scope*.</span><span class="sxs-lookup"><span data-stu-id="d0308-108">A [transaction](https://github.com/Azure/azure-service-bus/tree/master/samples/DotNet/Microsoft.ServiceBus.Messaging/AtomicTransactions#what-are-transactions) groups two or more operations together into an *execution scope*.</span></span> <span data-ttu-id="d0308-109">Enligt natur har sådana en transaktion måste se till att alla åtgärder som tillhör en viss grupp av åtgärder lyckas eller misslyckas gemensamt.</span><span class="sxs-lookup"><span data-stu-id="d0308-109">By nature, such a transaction must ensure that all operations belonging to a given group of operations either succeed or fail jointly.</span></span> <span data-ttu-id="d0308-110">I detta avseende transaktioner ska fungera som en enhet, som ofta kallas *odelbarhet*.</span><span class="sxs-lookup"><span data-stu-id="d0308-110">In this respect transactions act as one unit, which is often referred to as *atomicity*.</span></span> 

<span data-ttu-id="d0308-111">Service Bus är en transaktionsmeddelande broker och säkerställer transaktionella integritet för alla interna åtgärder mot dess meddelanden sparas.</span><span class="sxs-lookup"><span data-stu-id="d0308-111">Service Bus is a transactional message broker and ensures transactional integrity for all internal operations against its message stores.</span></span> <span data-ttu-id="d0308-112">Alla överföringar av meddelanden i Service Bus, till exempel flytta meddelanden till en [kö med olevererade brev](service-bus-dead-letter-queues.md) eller [automatisk vidarebefordran](service-bus-auto-forwarding.md) är transaktionella meddelanden mellan entiteter.</span><span class="sxs-lookup"><span data-stu-id="d0308-112">All transfers of messages inside of Service Bus, such as moving messages to a [dead-letter queue](service-bus-dead-letter-queues.md) or [automatic forwarding](service-bus-auto-forwarding.md) of messages between entities, are transactional.</span></span> <span data-ttu-id="d0308-113">Därför om Service Bus tar emot ett meddelande, har det redan lagras och märkta med ett sekvensnummer.</span><span class="sxs-lookup"><span data-stu-id="d0308-113">As such, if Service Bus accepts a message, it has already been stored and labeled with a sequence number.</span></span> <span data-ttu-id="d0308-114">Därefter överföringar av alla meddelanden i Service Bus är samordnade åtgärder mellan enheter och kommer varken leda till förlust av (lyckas källa och mål misslyckas) eller duplicering (misslyckas källa och mål lyckas) för meddelandet.</span><span class="sxs-lookup"><span data-stu-id="d0308-114">From then on, any message transfers within Service Bus are coordinated operations across entities, and will neither lead to loss (source succeeds and target fails) or to duplication (source fails and target succeeds) of the message.</span></span>

<span data-ttu-id="d0308-115">Service Bus stöder gruppering åtgärder mot en enskild meddelandeentitet (kö, ämne, prenumeration) omfattas av en transaktion.</span><span class="sxs-lookup"><span data-stu-id="d0308-115">Service Bus supports grouping operations against a single messaging entity (queue, topic, subscription) within the scope of a transaction.</span></span> <span data-ttu-id="d0308-116">Exempelvis kan du skicka flera meddelanden till en kö från inom ett transaktionsomfång och meddelandena som ska allokeras till köns loggen när transaktionen har slutförts.</span><span class="sxs-lookup"><span data-stu-id="d0308-116">For example, you can send several messages to one queue from within a transaction scope, and the messages will only be committed to the queue's log when the transaction successfully completes.</span></span>

## <a name="operations-within-a-transaction-scope"></a><span data-ttu-id="d0308-117">Åtgärder i ett transaktions-scope</span><span class="sxs-lookup"><span data-stu-id="d0308-117">Operations within a transaction scope</span></span>
<span data-ttu-id="d0308-118">De åtgärder som kan utföras inom ett transaktionsomfång är följande:</span><span class="sxs-lookup"><span data-stu-id="d0308-118">The operations that can be performed within a transaction scope are as follows:</span></span>

* <span data-ttu-id="d0308-119">**[QueueClient](/dotnet/api/microsoft.servicebus.messaging.queueclient), [MessageSender](/dotnet/api/microsoft.servicebus.messaging.messagesender), [TopicClient](/dotnet/api/microsoft.servicebus.messaging.topicclient)**: skicka, SendAsync SendBatch, SendBatchAsync</span><span class="sxs-lookup"><span data-stu-id="d0308-119">**[QueueClient](/dotnet/api/microsoft.servicebus.messaging.queueclient), [MessageSender](/dotnet/api/microsoft.servicebus.messaging.messagesender), [TopicClient](/dotnet/api/microsoft.servicebus.messaging.topicclient)**: Send, SendAsync, SendBatch, SendBatchAsync</span></span> 
* <span data-ttu-id="d0308-120">**[BrokeredMessage](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage)**: klar CompleteAsync, avbryta, AbandonAsync, Systemkön, DeadletterAsync, skjuta upp, DeferAsync, RenewLock, RenewLockAsync</span><span class="sxs-lookup"><span data-stu-id="d0308-120">**[BrokeredMessage](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage)**: Complete, CompleteAsync, Abandon, AbandonAsync, Deadletter, DeadletterAsync, Defer, DeferAsync, RenewLock, RenewLockAsync</span></span> 

<span data-ttu-id="d0308-121">Ta emot operations ingår inte i listan, eftersom det antas att programmet hämtar meddelanden med hjälp av den [ReceiveMode.PeekLock](/dotnet/api/microsoft.servicebus.messaging.receivemode) läge i några få loop eller med en [OnMessage](/dotnet/api/microsoft.servicebus.messaging.messagereceiver#Microsoft_ServiceBus_Messaging_MessageReceiver_OnMessage_System_Action_Microsoft_ServiceBus_Messaging_BrokeredMessage__Microsoft_ServiceBus_Messaging_OnMessageOptions_) motringning och endast sedan öppnar ett transaktions-scope för bearbetning av meddelandet.</span><span class="sxs-lookup"><span data-stu-id="d0308-121">Receive operations are not included, because it is assumed that the application acquires messages using the [ReceiveMode.PeekLock](/dotnet/api/microsoft.servicebus.messaging.receivemode) mode, inside some receive loop or with an [OnMessage](/dotnet/api/microsoft.servicebus.messaging.messagereceiver#Microsoft_ServiceBus_Messaging_MessageReceiver_OnMessage_System_Action_Microsoft_ServiceBus_Messaging_BrokeredMessage__Microsoft_ServiceBus_Messaging_OnMessageOptions_) callback, and only then opens a transaction scope for processing the message.</span></span>

<span data-ttu-id="d0308-122">Disposition av meddelandet (slutförd, avbryta, obeställbara, skjuta upp) sker sedan inom omfånget för, och beroende på övergripande resultatet av transaktionen.</span><span class="sxs-lookup"><span data-stu-id="d0308-122">The disposition of the message (complete, abandon, dead-letter, defer) then occurs within the scope of, and dependent on, the overall outcome of the transaction.</span></span>

## <a name="transfers-and-send-via"></a><span data-ttu-id="d0308-123">Överföringar och ”skicka”</span><span class="sxs-lookup"><span data-stu-id="d0308-123">Transfers and "send via"</span></span>
<span data-ttu-id="d0308-124">Om du vill aktivera transaktionella övergång av data från en kö till en processor och sedan till en annan kö, Service Bus stöder *överföringar*.</span><span class="sxs-lookup"><span data-stu-id="d0308-124">To enable transactional handover of data from a queue to a processor, and then to another queue, Service Bus supports *transfers*.</span></span> <span data-ttu-id="d0308-125">En avsändare skickar först ett meddelande till en ”överföringskön” i överföringen, och överföringskön flyttar meddelandet till kön avsedda mål med samma robust överföring implementering som automatiskt vidarebefordra meddelanden bygger på.</span><span class="sxs-lookup"><span data-stu-id="d0308-125">In a transfer operation, a sender first sends a message to a "transfer queue" and the transfer queue immediately moves the message to the intended destination queue using the same robust transfer implementation that the auto-forward capability relies on.</span></span> <span data-ttu-id="d0308-126">Meddelandet har aldrig allokeras till överföringskön logga in så att den blir synlig för överföringskön konsumenter.</span><span class="sxs-lookup"><span data-stu-id="d0308-126">The message is never committed to the transfer queue's log in a way that it becomes visible for the transfer queue's consumers.</span></span>

<span data-ttu-id="d0308-127">Kraften i funktionen transaktionella blir tydligt när köns överföringen är källan till avsändarens inkommande meddelanden.</span><span class="sxs-lookup"><span data-stu-id="d0308-127">The power of this transactional capability becomes apparent when the transfer queue itself is the source of the sender's input messages.</span></span> <span data-ttu-id="d0308-128">Med andra ord Service Bus kan överföra meddelandet till målkön ”via” överföringskön, när du utför en fullständig (eller skjuta upp, eller förlorade) åtgärden på Indatameddelandet i en atomisk åtgärd.</span><span class="sxs-lookup"><span data-stu-id="d0308-128">In other words, Service Bus can transfer the message to the destination queue "via" the transfer queue, while performing a complete (or defer, or dead-letter) operation on the input message, all in one atomic operation.</span></span> 

### <a name="see-it-in-code"></a><span data-ttu-id="d0308-129">Se den i koden</span><span class="sxs-lookup"><span data-stu-id="d0308-129">See it in code</span></span>
<span data-ttu-id="d0308-130">Om du vill konfigurera överföringarna skapa avsändarens som riktar sig till målkön via överföringskön.</span><span class="sxs-lookup"><span data-stu-id="d0308-130">To set up such transfers, you create a message sender that targets the destination queue via the transfer queue.</span></span> <span data-ttu-id="d0308-131">Du måste även ha en mottagare som tar emot meddelanden från samma kön.</span><span class="sxs-lookup"><span data-stu-id="d0308-131">You will also have a receiver that pulls messages from that same queue.</span></span> <span data-ttu-id="d0308-132">Exempel:</span><span class="sxs-lookup"><span data-stu-id="d0308-132">For example:</span></span>

```csharp
var sender = this.messagingFactory.CreateMessageSender(destinationQueue, myQueueName);
var receiver = this.messagingFactory.CreateMessageReceiver(myQueueName);
```

<span data-ttu-id="d0308-133">En enkel transaktion sedan använder de här elementen som i följande exempel:</span><span class="sxs-lookup"><span data-stu-id="d0308-133">A simple transaction then uses these elements, as in the following example:</span></span>

```csharp
var msg = receiver.Receive();

using (scope = new TransactionScope())
{
    // Do whatever work is required 

    var newmsg = ... // package the result 

    msg.Complete(); // mark the message as done
    sender.Send(newmsg); // forward the result

    scope.Complete(); // declare the transaction done
} 
```

## <a name="next-steps"></a><span data-ttu-id="d0308-134">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="d0308-134">Next steps</span></span>

<span data-ttu-id="d0308-135">Se följande artiklar för mer information om Service Bus-köer:</span><span class="sxs-lookup"><span data-stu-id="d0308-135">See the following articles for more information about Service Bus queues:</span></span>

* [<span data-ttu-id="d0308-136">Länkning Service Bus-entiteter med automatisk vidarebefordring</span><span class="sxs-lookup"><span data-stu-id="d0308-136">Chaining Service Bus entities with auto-forwarding</span></span>](service-bus-auto-forwarding.md)
* [<span data-ttu-id="d0308-137">Automatisk vidarebefordring exempel</span><span class="sxs-lookup"><span data-stu-id="d0308-137">Auto-forward sample</span></span>](https://github.com/Azure/azure-service-bus/tree/master/samples/DotNet/Microsoft.ServiceBus.Messaging/AutoForward)
* [<span data-ttu-id="d0308-138">Atomiska transaktioner med Service Bus-exempel</span><span class="sxs-lookup"><span data-stu-id="d0308-138">Atomic Transactions with Service Bus sample</span></span>](https://github.com/Azure/azure-service-bus/tree/master/samples/DotNet/Microsoft.ServiceBus.Messaging/AtomicTransactions)
* [<span data-ttu-id="d0308-139">Azure-köer och Service Bus-köer jämfört med</span><span class="sxs-lookup"><span data-stu-id="d0308-139">Azure Queues and Service Bus queues compared</span></span>](service-bus-azure-and-service-bus-queues-compared-contrasted.md)
* [<span data-ttu-id="d0308-140">Använd Service Bus-köer</span><span class="sxs-lookup"><span data-stu-id="d0308-140">How to use Service Bus queues</span></span>](service-bus-dotnet-get-started-with-queues.md)

