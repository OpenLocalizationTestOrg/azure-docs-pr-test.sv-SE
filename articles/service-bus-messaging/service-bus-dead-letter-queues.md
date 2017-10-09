---
title: "aaaService Bus obeställbara köer | Microsoft Docs"
description: "Översikt över Azure Service Bus-köer för obeställbara"
services: service-bus-messaging
documentationcenter: .net
author: sethmanheim
manager: timlt
editor: 
ms.assetid: 68b2aa38-dba7-491a-9c26-0289bc15d397
ms.service: service-bus-messaging
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 05/17/2017
ms.author: clemensv;sethm
ms.openlocfilehash: 1638272085b8a3a59e8814f6f943caee35a2bfdc
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="overview-of-service-bus-dead-letter-queues"></a>Översikt över Service Bus-köer för obeställbara

Service Bus-köer och ämnesprenumerationer ger en sekundär plats kö kallas en *kö med olevererade brev* (DQL). hello obeställbara meddelanden behöver inte toobe explicit skapas och kan inte tagits bort eller på annat sätt hanterade oberoende av hello huvudsakliga entitet.

Den här artikeln beskrivs köer i Azure Service Bus. Mycket av hello diskussion illustreras med hello [köer exempel](https://github.com/Azure/azure-service-bus/tree/master/samples/DotNet/Microsoft.ServiceBus.Messaging/DeadletterQueue) på GitHub.
 
## <a name="hello-dead-letter-queue"></a>hello obeställbara meddelanden

hello syftar hello obeställbara meddelanden toohold meddelanden som inte kunde levereras tooany mottagaren eller meddelanden som inte kunde bearbetas. Meddelanden kan sedan tas bort från hello DQL och kontrolleras. Ett program kan, med hjälp av en operatör korrigera problem och skicka hello-meddelande, logga hello faktum att det uppstod ett fel och vidta åtgärder. 

Ur ett API och protokoll hello DQL är främst liknande tooany andra kö, förutom att meddelanden kan endast skickas via hello förlorade gest i hello överordnade entiteten. Dessutom time to live inte observeras, och det går inte att ett meddelande från en DQL för obeställbara. hello obeställbara meddelanden har fullständigt stöd för leverans av titt Lås och transaktionell åtgärder.

Observera att det finns ingen automatisk rensning av hello DQL. Meddelanden ligger kvar i hello DQL tills du uttryckligen hämta dem från hello DQL och anropet [Complete()](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage#Microsoft_ServiceBus_Messaging_BrokeredMessage_CompleteAsync) på förlorade hello-meddelande.

## <a name="moving-messages-toohello-dlq"></a>Flytta meddelanden toohello DQL

Det finns flera aktiviteter i Service Bus som orsakar meddelanden tooget pushas toohello DQL från inom hello messaging motorn sig själv. Ett program kan också explicit flytta meddelanden toohello DQL. 

Allteftersom hello-meddelande har flyttats av hello broker, två egenskaper läggs toohello meddelande som hello broker anropar sin interna version av hello [Systemkön](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage#Microsoft_ServiceBus_Messaging_BrokeredMessage_DeadLetter_System_String_System_String_) metod på hello-meddelande: `DeadLetterReason` och `DeadLetterErrorDescription`.

Program kan definiera egna koder för hello `DeadLetterReason` egenskapen men hello system anger hello följande värden.

| Villkor | DeadLetterReason | DeadLetterErrorDescription |
| --- | --- | --- |
| Alltid |HeaderSizeExceeded |Hej storlekskvoten för dataströmmen har överskridits. |
| ! TopicDescription.<br />EnableFilteringMessagesBeforePublishing och SubscriptionDescription.<br />EnableDeadLetteringOnFilterEvaluationExceptions |ett undantag. GetType(). Namn |ett undantag. Meddelande |
| EnableDeadLetteringOnMessageExpiration |TTLExpiredException |hello-meddelande gått ut och var döda lettered. |
| SubscriptionDescription.RequiresSession |Sessions-id är null. |Sessionen aktiverat entitet kan inte meddelandet vars sessions-ID är null. |
| ! kö med olevererade brev |MaxTransferHopCountExceeded |Null |
| Programmet explicit döda oljekategori |Anges av programmet |Anges av programmet |

## <a name="exceeding-maxdeliverycount"></a>Överstiger MaxDeliveryCount
Köer och -prenumerationer har en [QueueDescription.MaxDeliveryCount](/dotnet/api/microsoft.servicebus.messaging.queuedescription#Microsoft_ServiceBus_Messaging_QueueDescription_MaxDeliveryCount) och [SubscriptionDescription.MaxDeliveryCount](/dotnet/api/microsoft.servicebus.messaging.subscriptiondescription#Microsoft_ServiceBus_Messaging_SubscriptionDescription_MaxDeliveryCount) egenskapen respektive; hello standardvärdet är 10. När ett meddelande som har levererats under ett lås ([ReceiveMode.PeekLock](/dotnet/api/microsoft.servicebus.messaging.receivemode)), men har antingen explicit avbrutna eller hello låset har upphört att gälla, hello-meddelande [BrokeredMessage.DeliveryCount](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage#Microsoft_ServiceBus_Messaging_BrokeredMessage_DeliveryCount) är stegvis. När [DeliveryCount](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage#Microsoft_ServiceBus_Messaging_BrokeredMessage_DeliveryCount) överskrider [MaxDeliveryCount](/dotnet/api/microsoft.servicebus.messaging.queuedescription#Microsoft_ServiceBus_Messaging_QueueDescription_MaxDeliveryCount), hello-meddelande har flyttats toohello DQL, att ange hello `MaxDeliveryCountExceeded` orsakskod.

Det här problemet kan inte inaktiveras, men du kan ange [MaxDeliveryCount](/dotnet/api/microsoft.servicebus.messaging.queuedescription#Microsoft_ServiceBus_Messaging_QueueDescription_MaxDeliveryCount) tooa mycket stora tal.

## <a name="exceeding-timetolive"></a>Mer än TimeToLive
När hello [QueueDescription.EnableDeadLetteringOnMessageExpiration](/dotnet/api/microsoft.servicebus.messaging.queuedescription#Microsoft_ServiceBus_Messaging_QueueDescription_EnableDeadLetteringOnMessageExpiration) eller [SubscriptionDescription.EnableDeadLetteringOnMessageExpiration](/dotnet/api/microsoft.servicebus.messaging.subscriptiondescription#Microsoft_ServiceBus_Messaging_SubscriptionDescription_EnableDeadLetteringOnMessageExpiration) egenskapen för**true** (hello standardvärdet är **FALSKT**), alla utgående meddelanden är flyttade toohello DQL, att ange hello `TTLExpiredException` orsakskod.

Observera att inaktuella meddelanden rensas bara och därför flyttas toohello DQL när det finns minst en aktiv mottagaren dra på hello huvudkön eller prenumeration; Detta är avsiktligt.

## <a name="errors-while-processing-subscription-rules"></a>Fel vid bearbetning av prenumerationsregler
När hello [SubscriptionDescription.EnableDeadLetteringOnFilterEvaluationExceptions](/dotnet/api/microsoft.servicebus.messaging.subscriptiondescription#Microsoft_ServiceBus_Messaging_SubscriptionDescription_EnableDeadLetteringOnFilterEvaluationExceptions) egenskapen är aktiverad för en prenumeration, eventuella fel som inträffar när en prenumeration SQL filterregeln kör fångas i hello DQL tillsammans med hello felaktig meddelandet.

## <a name="application-level-dead-lettering"></a>Programnivå dead-lettering
Program kan använda DQL tooexplicitly avvisa oacceptabel hälsningsmeddelande i tillägg toohello systembaserade dead-lettering funktioner. Detta kan inkludera meddelanden som inte kan behandlas korrekt på grund av tooany slags systemproblem, meddelanden som har felaktiga nyttolaster eller meddelanden som avbryts autentiseringen när vissa meddelandenivå säkerhetsprogram används.

## <a name="dead-lettering-in-forwardto-or-sendvia-scenarios"></a>Dead-lettering i ForwardTo eller SendVia scenarier

Meddelanden kommer att skickas toohello överföring obeställbara meddelanden under hello följande villkor:

- Ett meddelande som passerar genom mer än 3 köer och ämnen som [sammankopplade](service-bus-auto-forwarding.md).
- hello målkön eller avsnittet är inaktiverad eller borttagen.
- hello målkön eller avsnittet överskrider hello entitet maximala storlek.

tooretrieve lettered förlorade meddelanden, kan du skapa en mottagare med hello [FormatTransferDeadletterPath](/dotnet/api/microsoft.servicebus.messaging.queueclient#Microsoft_ServiceBus_Messaging_QueueClient_FormatTransferDeadLetterPath_System_String_) verktyget metod.

## <a name="example"></a>Exempel
följande kodstycke hello skapar en mottagare för meddelandet. I hello meddelandemottagning för hello huvudkön, hello koden hämtar hello-meddelande med [Receive(TimeSpan.Zero)](/dotnet/api/microsoft.servicebus.messaging.messagereceiver#Microsoft_ServiceBus_Messaging_MessageReceiver_Receive_System_TimeSpan_), där du tillfrågas hello broker tooinstantly returnerade ett meddelande som är tillgänglig eller tooreturn med inget resultat. Om hello-koden får ett meddelande visas den omedelbart avbryts, vilket ökar hello `DeliveryCount`. När hello flyttas hello meddelandet toohello DQL, hello huvudkön är tom och hello loop utgångar som [ReceiveAsync](/dotnet/api/microsoft.servicebus.messaging.messagereceiver#Microsoft_ServiceBus_Messaging_MessageReceiver_ReceiveAsync_System_TimeSpan_) returnerar **null**.

```csharp
var receiver = await receiverFactory.CreateMessageReceiverAsync(queueName, ReceiveMode.PeekLock);
while(true)
{
    var msg = await receiver.ReceiveAsync(TimeSpan.Zero);
    if (msg != null)
    {
        Console.WriteLine("Picked up message; DeliveryCount {0}", msg.DeliveryCount);
        await msg.AbandonAsync();
    }
    else
    {
        break;
    }
}
```

## <a name="next-steps"></a>Nästa steg
Se följande artiklar för mer information om Service Bus-köer hello:

* [Komma igång med Service Bus-köer](service-bus-dotnet-get-started-with-queues.md)
* [Azure-köer och Service Bus-köer jämfört med](service-bus-azure-and-service-bus-queues-compared-contrasted.md)

