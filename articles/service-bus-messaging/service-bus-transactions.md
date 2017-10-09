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
# <a name="overview-of-service-bus-transaction-processing"></a>Översikt över Service Bus transaktionsbearbetning
Den här artikeln beskrivs hello transaktion funktionerna i Azure Service Bus. Mycket av hello diskussion illustreras med hello [atomiska transaktioner med Service Bus-exempel](https://github.com/Azure/azure-service-bus/tree/master/samples/DotNet/Microsoft.ServiceBus.Messaging/AtomicTransactions). Den här artikeln är begränsad tooan översikt över transaktionsbearbetning och hello *skicka via* funktion i Service Bus, medan hello atomiska transaktioner exempel större och mer komplexa omfång.

## <a name="transactions-in-service-bus"></a>Transaktioner i Service Bus
En [transaktion](https://github.com/Azure/azure-service-bus/tree/master/samples/DotNet/Microsoft.ServiceBus.Messaging/AtomicTransactions#what-are-transactions) för att gruppera två eller flera åtgärder i en *körningen scope*. Enligt natur har sådana en transaktion måste se till att alla åtgärder som hör tooa angivna grupp operations lyckas eller misslyckas gemensamt. I detta avseende transaktioner ska fungera som en enhet, vilket ofta är refererad tooas *odelbarhet*. 

Service Bus är en transaktionsmeddelande broker och säkerställer transaktionella integritet för alla interna åtgärder mot dess meddelanden sparas. Alla överföringar av meddelanden i Service Bus, till exempel flytta meddelanden tooa [kö med olevererade brev](service-bus-dead-letter-queues.md) eller [automatisk vidarebefordran](service-bus-auto-forwarding.md) är transaktionella meddelanden mellan entiteter. Därför om Service Bus tar emot ett meddelande, har det redan lagras och märkta med ett sekvensnummer. Därefter överföringar av alla meddelanden i Service Bus är samordnade åtgärder mellan enheter och dirigerar varken tooloss (källa lyckas och misslyckas mål) eller tooduplication (misslyckas källa och mål lyckas) av hello-meddelande.

Service Bus stöder gruppering åtgärder mot en enda meddelandeentitet (kö, ämne, prenumeration) hello omfattas av en transaktion. Exempelvis kan du skicka några meddelanden tooone kö från inom ett transaktionsomfång och hälsningsmeddelande ska verkställas toohello kön loggen när hello transaktionen har slutförts.

## <a name="operations-within-a-transaction-scope"></a>Åtgärder i ett transaktions-scope
hello-åtgärder som kan utföras inom ett transaktionsomfång är följande:

* **[QueueClient](/dotnet/api/microsoft.servicebus.messaging.queueclient), [MessageSender](/dotnet/api/microsoft.servicebus.messaging.messagesender), [TopicClient](/dotnet/api/microsoft.servicebus.messaging.topicclient)**: skicka, SendAsync SendBatch, SendBatchAsync 
* **[BrokeredMessage](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage)**: klar CompleteAsync, avbryta, AbandonAsync, Systemkön, DeadletterAsync, skjuta upp, DeferAsync, RenewLock, RenewLockAsync 

Ta emot operations ingår inte i listan, eftersom det antas att programmet hello får meddelanden med hello [ReceiveMode.PeekLock](/dotnet/api/microsoft.servicebus.messaging.receivemode) läge i några få loop eller med en [OnMessage](/dotnet/api/microsoft.servicebus.messaging.messagereceiver#Microsoft_ServiceBus_Messaging_MessageReceiver_OnMessage_System_Action_Microsoft_ServiceBus_Messaging_BrokeredMessage__Microsoft_ServiceBus_Messaging_OnMessageOptions_) återanrop och endast sedan öppnas en transaktion omfånget för bearbetning hello-meddelande.

hello disposition av hello-meddelande (slutförd, avbryta, obeställbara, skjuta upp) sedan uppstår inom hello omfattning och beroende, hello övergripande resultatet av hello transaktion.

## <a name="transfers-and-send-via"></a>Överföringar och ”skicka”
tooenable transaktionella övergång av data från en kö tooa processor och tooanother kön, Service Bus stöder *överföringar*. I en överföringen en avsändare skickar först en message-tooa ”överföringskön” och hello överföringskön flyttar hello meddelandet toohello avsedd målkön med hello samma robust överföra implementering som förlitar sig hello automatiskt vidarebefordra meddelanden på. hello-meddelande är aldrig allokerat toohello överföringskön logga in så att den blir synlig för hello överföringskön konsumenter.

hello kraften i funktionen transaktionella blir tydligt när hello överföringskön själva är hello hello avsändarens inkommande meddelanden. Service Bus kan med andra ord överföra hello meddelandekö toohello mål ”via” Hej överföringskön när du utför en fullständig (eller skjuta upp, eller förlorade) åtgärden på den inkommande hello-meddelande, allt i en atomisk åtgärd. 

### <a name="see-it-in-code"></a>Se den i koden
tooset in sådana överföringar skapa avsändarens som riktar sig till hello målkön via hello överföringskön. Du måste även ha en mottagare som tar emot meddelanden från samma kön. Exempel:

```csharp
var sender = this.messagingFactory.CreateMessageSender(destinationQueue, myQueueName);
var receiver = this.messagingFactory.CreateMessageReceiver(myQueueName);
```

En enkel transaktion sedan använder de här elementen som i följande exempel hello:

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

## <a name="next-steps"></a>Nästa steg

Se följande artiklar för mer information om Service Bus-köer hello:

* [Länkning Service Bus-entiteter med automatisk vidarebefordring](service-bus-auto-forwarding.md)
* [Automatisk vidarebefordring exempel](https://github.com/Azure/azure-service-bus/tree/master/samples/DotNet/Microsoft.ServiceBus.Messaging/AutoForward)
* [Atomiska transaktioner med Service Bus-exempel](https://github.com/Azure/azure-service-bus/tree/master/samples/DotNet/Microsoft.ServiceBus.Messaging/AtomicTransactions)
* [Azure-köer och Service Bus-köer jämfört med](service-bus-azure-and-service-bus-queues-compared-contrasted.md)
* [Hur toouse Service Bus-köer](service-bus-dotnet-get-started-with-queues.md)

