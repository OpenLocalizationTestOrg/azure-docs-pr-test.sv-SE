---
title: aaaOverview transaktion bearbetas i Azure Service Bus | Microsoft Docs
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
ms.openlocfilehash: 5ed4d1fd3a089b8ebcd69a568f4ac863e753aecd
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="overview-of-service-bus-transaction-processing"></a><span data-ttu-id="4d27b-103">Översikt över Service Bus transaktionsbearbetning</span><span class="sxs-lookup"><span data-stu-id="4d27b-103">Overview of Service Bus transaction processing</span></span>
<span data-ttu-id="4d27b-104">Den här artikeln beskrivs hello transaktion funktionerna i Azure Service Bus.</span><span class="sxs-lookup"><span data-stu-id="4d27b-104">This article discusses hello transaction capabilities of Azure Service Bus.</span></span> <span data-ttu-id="4d27b-105">Mycket av hello diskussion illustreras med hello [atomiska transaktioner med Service Bus-exempel](https://github.com/Azure/azure-service-bus/tree/master/samples/DotNet/Microsoft.ServiceBus.Messaging/AtomicTransactions).</span><span class="sxs-lookup"><span data-stu-id="4d27b-105">Much of hello discussion is illustrated by hello [Atomic Transactions with Service Bus sample](https://github.com/Azure/azure-service-bus/tree/master/samples/DotNet/Microsoft.ServiceBus.Messaging/AtomicTransactions).</span></span> <span data-ttu-id="4d27b-106">Den här artikeln är begränsad tooan översikt över transaktionsbearbetning och hello *skicka via* funktion i Service Bus, medan hello atomiska transaktioner exempel större och mer komplexa omfång.</span><span class="sxs-lookup"><span data-stu-id="4d27b-106">This article is limited tooan overview of transaction processing and hello *send via* feature in Service Bus, while hello Atomic Transactions sample is broader and more complex in scope.</span></span>

## <a name="transactions-in-service-bus"></a><span data-ttu-id="4d27b-107">Transaktioner i Service Bus</span><span class="sxs-lookup"><span data-stu-id="4d27b-107">Transactions in Service Bus</span></span>
<span data-ttu-id="4d27b-108">En [transaktion](https://github.com/Azure/azure-service-bus/tree/master/samples/DotNet/Microsoft.ServiceBus.Messaging/AtomicTransactions#what-are-transactions) för att gruppera två eller flera åtgärder i en *körningen scope*.</span><span class="sxs-lookup"><span data-stu-id="4d27b-108">A [transaction](https://github.com/Azure/azure-service-bus/tree/master/samples/DotNet/Microsoft.ServiceBus.Messaging/AtomicTransactions#what-are-transactions) groups two or more operations together into an *execution scope*.</span></span> <span data-ttu-id="4d27b-109">Enligt natur har sådana en transaktion måste se till att alla åtgärder som hör tooa angivna grupp operations lyckas eller misslyckas gemensamt.</span><span class="sxs-lookup"><span data-stu-id="4d27b-109">By nature, such a transaction must ensure that all operations belonging tooa given group of operations either succeed or fail jointly.</span></span> <span data-ttu-id="4d27b-110">I detta avseende transaktioner ska fungera som en enhet, vilket ofta är refererad tooas *odelbarhet*.</span><span class="sxs-lookup"><span data-stu-id="4d27b-110">In this respect transactions act as one unit, which is often referred tooas *atomicity*.</span></span> 

<span data-ttu-id="4d27b-111">Service Bus är en transaktionsmeddelande broker och säkerställer transaktionella integritet för alla interna åtgärder mot dess meddelanden sparas.</span><span class="sxs-lookup"><span data-stu-id="4d27b-111">Service Bus is a transactional message broker and ensures transactional integrity for all internal operations against its message stores.</span></span> <span data-ttu-id="4d27b-112">Alla överföringar av meddelanden i Service Bus, till exempel flytta meddelanden tooa [kö med olevererade brev](service-bus-dead-letter-queues.md) eller [automatisk vidarebefordran](service-bus-auto-forwarding.md) är transaktionella meddelanden mellan entiteter.</span><span class="sxs-lookup"><span data-stu-id="4d27b-112">All transfers of messages inside of Service Bus, such as moving messages tooa [dead-letter queue](service-bus-dead-letter-queues.md) or [automatic forwarding](service-bus-auto-forwarding.md) of messages between entities, are transactional.</span></span> <span data-ttu-id="4d27b-113">Därför om Service Bus tar emot ett meddelande, har det redan lagras och märkta med ett sekvensnummer.</span><span class="sxs-lookup"><span data-stu-id="4d27b-113">As such, if Service Bus accepts a message, it has already been stored and labeled with a sequence number.</span></span> <span data-ttu-id="4d27b-114">Därefter överföringar av alla meddelanden i Service Bus är samordnade åtgärder mellan enheter och dirigerar varken tooloss (källa lyckas och misslyckas mål) eller tooduplication (misslyckas källa och mål lyckas) av hello-meddelande.</span><span class="sxs-lookup"><span data-stu-id="4d27b-114">From then on, any message transfers within Service Bus are coordinated operations across entities, and will neither lead tooloss (source succeeds and target fails) or tooduplication (source fails and target succeeds) of hello message.</span></span>

<span data-ttu-id="4d27b-115">Service Bus stöder gruppering åtgärder mot en enda meddelandeentitet (kö, ämne, prenumeration) hello omfattas av en transaktion.</span><span class="sxs-lookup"><span data-stu-id="4d27b-115">Service Bus supports grouping operations against a single messaging entity (queue, topic, subscription) within hello scope of a transaction.</span></span> <span data-ttu-id="4d27b-116">Exempelvis kan du skicka några meddelanden tooone kö från inom ett transaktionsomfång och hälsningsmeddelande ska verkställas toohello kön loggen när hello transaktionen har slutförts.</span><span class="sxs-lookup"><span data-stu-id="4d27b-116">For example, you can send several messages tooone queue from within a transaction scope, and hello messages will only be committed toohello queue's log when hello transaction successfully completes.</span></span>

## <a name="operations-within-a-transaction-scope"></a><span data-ttu-id="4d27b-117">Åtgärder i ett transaktions-scope</span><span class="sxs-lookup"><span data-stu-id="4d27b-117">Operations within a transaction scope</span></span>
<span data-ttu-id="4d27b-118">hello-åtgärder som kan utföras inom ett transaktionsomfång är följande:</span><span class="sxs-lookup"><span data-stu-id="4d27b-118">hello operations that can be performed within a transaction scope are as follows:</span></span>

* <span data-ttu-id="4d27b-119">**[QueueClient](/dotnet/api/microsoft.servicebus.messaging.queueclient), [MessageSender](/dotnet/api/microsoft.servicebus.messaging.messagesender), [TopicClient](/dotnet/api/microsoft.servicebus.messaging.topicclient)**: skicka, SendAsync SendBatch, SendBatchAsync</span><span class="sxs-lookup"><span data-stu-id="4d27b-119">**[QueueClient](/dotnet/api/microsoft.servicebus.messaging.queueclient), [MessageSender](/dotnet/api/microsoft.servicebus.messaging.messagesender), [TopicClient](/dotnet/api/microsoft.servicebus.messaging.topicclient)**: Send, SendAsync, SendBatch, SendBatchAsync</span></span> 
* <span data-ttu-id="4d27b-120">**[BrokeredMessage](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage)**: klar CompleteAsync, avbryta, AbandonAsync, Systemkön, DeadletterAsync, skjuta upp, DeferAsync, RenewLock, RenewLockAsync</span><span class="sxs-lookup"><span data-stu-id="4d27b-120">**[BrokeredMessage](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage)**: Complete, CompleteAsync, Abandon, AbandonAsync, Deadletter, DeadletterAsync, Defer, DeferAsync, RenewLock, RenewLockAsync</span></span> 

<span data-ttu-id="4d27b-121">Ta emot operations ingår inte i listan, eftersom det antas att programmet hello får meddelanden med hello [ReceiveMode.PeekLock](/dotnet/api/microsoft.servicebus.messaging.receivemode) läge i några få loop eller med en [OnMessage](/dotnet/api/microsoft.servicebus.messaging.messagereceiver#Microsoft_ServiceBus_Messaging_MessageReceiver_OnMessage_System_Action_Microsoft_ServiceBus_Messaging_BrokeredMessage__Microsoft_ServiceBus_Messaging_OnMessageOptions_) återanrop och endast sedan öppnas en transaktion omfånget för bearbetning hello-meddelande.</span><span class="sxs-lookup"><span data-stu-id="4d27b-121">Receive operations are not included, because it is assumed that hello application acquires messages using hello [ReceiveMode.PeekLock](/dotnet/api/microsoft.servicebus.messaging.receivemode) mode, inside some receive loop or with an [OnMessage](/dotnet/api/microsoft.servicebus.messaging.messagereceiver#Microsoft_ServiceBus_Messaging_MessageReceiver_OnMessage_System_Action_Microsoft_ServiceBus_Messaging_BrokeredMessage__Microsoft_ServiceBus_Messaging_OnMessageOptions_) callback, and only then opens a transaction scope for processing hello message.</span></span>

<span data-ttu-id="4d27b-122">hello disposition av hello-meddelande (slutförd, avbryta, obeställbara, skjuta upp) sedan uppstår inom hello omfattning och beroende, hello övergripande resultatet av hello transaktion.</span><span class="sxs-lookup"><span data-stu-id="4d27b-122">hello disposition of hello message (complete, abandon, dead-letter, defer) then occurs within hello scope of, and dependent on, hello overall outcome of hello transaction.</span></span>

## <a name="transfers-and-send-via"></a><span data-ttu-id="4d27b-123">Överföringar och ”skicka”</span><span class="sxs-lookup"><span data-stu-id="4d27b-123">Transfers and "send via"</span></span>
<span data-ttu-id="4d27b-124">tooenable transaktionella övergång av data från en kö tooa processor och tooanother kön, Service Bus stöder *överföringar*.</span><span class="sxs-lookup"><span data-stu-id="4d27b-124">tooenable transactional handover of data from a queue tooa processor, and then tooanother queue, Service Bus supports *transfers*.</span></span> <span data-ttu-id="4d27b-125">I en överföringen en avsändare skickar först en message-tooa ”överföringskön” och hello överföringskön flyttar hello meddelandet toohello avsedd målkön med hello samma robust överföra implementering som förlitar sig hello automatiskt vidarebefordra meddelanden på.</span><span class="sxs-lookup"><span data-stu-id="4d27b-125">In a transfer operation, a sender first sends a message tooa "transfer queue" and hello transfer queue immediately moves hello message toohello intended destination queue using hello same robust transfer implementation that hello auto-forward capability relies on.</span></span> <span data-ttu-id="4d27b-126">hello-meddelande är aldrig allokerat toohello överföringskön logga in så att den blir synlig för hello överföringskön konsumenter.</span><span class="sxs-lookup"><span data-stu-id="4d27b-126">hello message is never committed toohello transfer queue's log in a way that it becomes visible for hello transfer queue's consumers.</span></span>

<span data-ttu-id="4d27b-127">hello kraften i funktionen transaktionella blir tydligt när hello överföringskön själva är hello hello avsändarens inkommande meddelanden.</span><span class="sxs-lookup"><span data-stu-id="4d27b-127">hello power of this transactional capability becomes apparent when hello transfer queue itself is hello source of hello sender's input messages.</span></span> <span data-ttu-id="4d27b-128">Service Bus kan med andra ord överföra hello meddelandekö toohello mål ”via” Hej överföringskön när du utför en fullständig (eller skjuta upp, eller förlorade) åtgärden på den inkommande hello-meddelande, allt i en atomisk åtgärd.</span><span class="sxs-lookup"><span data-stu-id="4d27b-128">In other words, Service Bus can transfer hello message toohello destination queue "via" hello transfer queue, while performing a complete (or defer, or dead-letter) operation on hello input message, all in one atomic operation.</span></span> 

### <a name="see-it-in-code"></a><span data-ttu-id="4d27b-129">Se den i koden</span><span class="sxs-lookup"><span data-stu-id="4d27b-129">See it in code</span></span>
<span data-ttu-id="4d27b-130">tooset in sådana överföringar skapa avsändarens som riktar sig till hello målkön via hello överföringskön.</span><span class="sxs-lookup"><span data-stu-id="4d27b-130">tooset up such transfers, you create a message sender that targets hello destination queue via hello transfer queue.</span></span> <span data-ttu-id="4d27b-131">Du måste även ha en mottagare som tar emot meddelanden från samma kön.</span><span class="sxs-lookup"><span data-stu-id="4d27b-131">You will also have a receiver that pulls messages from that same queue.</span></span> <span data-ttu-id="4d27b-132">Exempel:</span><span class="sxs-lookup"><span data-stu-id="4d27b-132">For example:</span></span>

```csharp
var sender = this.messagingFactory.CreateMessageSender(destinationQueue, myQueueName);
var receiver = this.messagingFactory.CreateMessageReceiver(myQueueName);
```

<span data-ttu-id="4d27b-133">En enkel transaktion sedan använder de här elementen som i följande exempel hello:</span><span class="sxs-lookup"><span data-stu-id="4d27b-133">A simple transaction then uses these elements, as in hello following example:</span></span>

```csharp
var msg = receiver.Receive();

using (scope = new TransactionScope())
{
    // Do whatever work is required 

    var newmsg = ... // package hello result 

    msg.Complete(); // mark hello message as done
    sender.Send(newmsg); // forward hello result

    scope.Complete(); // declare hello transaction done
} 
```

## <a name="next-steps"></a><span data-ttu-id="4d27b-134">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="4d27b-134">Next steps</span></span>

<span data-ttu-id="4d27b-135">Se följande artiklar för mer information om Service Bus-köer hello:</span><span class="sxs-lookup"><span data-stu-id="4d27b-135">See hello following articles for more information about Service Bus queues:</span></span>

* [<span data-ttu-id="4d27b-136">Länkning Service Bus-entiteter med automatisk vidarebefordring</span><span class="sxs-lookup"><span data-stu-id="4d27b-136">Chaining Service Bus entities with auto-forwarding</span></span>](service-bus-auto-forwarding.md)
* [<span data-ttu-id="4d27b-137">Automatisk vidarebefordring exempel</span><span class="sxs-lookup"><span data-stu-id="4d27b-137">Auto-forward sample</span></span>](https://github.com/Azure/azure-service-bus/tree/master/samples/DotNet/Microsoft.ServiceBus.Messaging/AutoForward)
* [<span data-ttu-id="4d27b-138">Atomiska transaktioner med Service Bus-exempel</span><span class="sxs-lookup"><span data-stu-id="4d27b-138">Atomic Transactions with Service Bus sample</span></span>](https://github.com/Azure/azure-service-bus/tree/master/samples/DotNet/Microsoft.ServiceBus.Messaging/AtomicTransactions)
* [<span data-ttu-id="4d27b-139">Azure-köer och Service Bus-köer jämfört med</span><span class="sxs-lookup"><span data-stu-id="4d27b-139">Azure Queues and Service Bus queues compared</span></span>](service-bus-azure-and-service-bus-queues-compared-contrasted.md)
* [<span data-ttu-id="4d27b-140">Hur toouse Service Bus-köer</span><span class="sxs-lookup"><span data-stu-id="4d27b-140">How toouse Service Bus queues</span></span>](service-bus-dotnet-get-started-with-queues.md)

