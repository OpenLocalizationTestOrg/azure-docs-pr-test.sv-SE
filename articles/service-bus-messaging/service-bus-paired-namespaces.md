---
title: "aaaAzure Service Bus länkas namnområden | Microsoft Docs"
description: "Implementeringsdetaljer parad namnområde och kostnad"
services: service-bus-messaging
documentationcenter: na
author: sethmanheim
manager: timlt
editor: 
ms.assetid: 2440c8d3-ed2e-47e0-93cf-ab7fbb855d2e
ms.service: service-bus-messaging
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 05/25/2017
ms.author: sethm
ms.openlocfilehash: 4c44b2b95d2228e1ad8075b52634d88a1593d3b1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="paired-namespace-implementation-details-and-cost-implications"></a>Länkas namnområde implementeringsdetaljer och cost effekter
Hej [PairNamespaceAsync] [ PairNamespaceAsync] metod med en [SendAvailabilityPairedNamespaceOptions] [ SendAvailabilityPairedNamespaceOptions] instansen, utför synliga uppgifter å dina vägnar. Eftersom det är kostnad överväganden när du använder hello funktion, är användbar toounderstand de uppgifter så att du förväntar dig hello beteendet när det sker. hello API aktiverar hello följande automatiska funktioner för din räkning:

* Skapa eftersläpning köer.
* Skapa en [MessageSender] [ MessageSender] objekt som nämns tooqueues eller avsnitt.
* När en meddelandeentitet blir otillgänglig, skickar pingmeddelanden toohello entiteten i ett försök toodetect när enheten blir tillgänglig igen.
* Du kan även skapa en uppsättning ”meddelande pumpar” att flytta meddelanden från hello eftersläpning köer toohello primära köer.
* Samordnar avslutande/felaktig av hello primära och sekundära [MessagingFactory] [ MessagingFactory] instanser.

På en hög nivå hello-funktionen fungerar på följande sätt: när hello primära entiteten är felfri, ingen funktionsändringar sker. När hello [FailoverInterval] [ FailoverInterval] tid förflutit och hello primära entiteten ser inga lyckade skickar efter ett tillfälligt [MessagingException] [ MessagingException] eller en [TimeoutException][TimeoutException], hello följande beteende inträffar:

1. Skicka operations toohello primära entiteten är inaktiverade och hello system pingar hello primära entiteten tills pingar har levereras.
2. En slumpmässig eftersläpning kö har valts.
3. [BrokeredMessage] [ BrokeredMessage] objekt är routade toohello valt eftersläpning kön.
4. Om en skicka åtgärden toohello valt eftersläpning kön misslyckas kön hämtas från hello rotation och en ny kö har valts. Alla avsändare på hello [MessagingFactory] [ MessagingFactory] instans Läs hello-fel.

hello följande figur visas hello sekvens. Först skickar hello avsändare meddelanden.

![Parad namnområden][0]

Vid fel toosend toohello primära kön börjar hello avsändaren skicka meddelanden tooa slumpmässigt eftersläpning kön. Samtidigt, startar en ping-aktivitet.

![Parad namnområden][1]

Nu finns kvar i hello sekundär kö hälsningsmeddelande och inte har levererats toohello primära kön. När hello primära kön är felfri igen, bör minst en process körs hello syphon. Hej syphon ger dig en hälsningsmeddelande från alla hello olika eftersläpning köer toohello rätt mål entiteter (köer och ämnen).

![Parad namnområden][2]

hello resten av det här avsnittet beskrivs hello specifik information om hur dessa delar fungerar.

## <a name="creation-of-backlog-queues"></a>Skapandet av eftersläpning köer
Hej [SendAvailabilityPairedNamespaceOptions] [ SendAvailabilityPairedNamespaceOptions] objekt som överförts toohello [PairNamespaceAsync] [ PairNamespaceAsync] metoden anger hello Antal eftersläpning köer du vill toouse. Varje kö eftersläpning skapas med hello följande egenskaper uttryckligen ange (alla andra värden anges toohello [QueueDescription] [ QueueDescription] standardvärden):

| Sökväg | [primära namnutrymme] / x-servicebus-överföring / [index] där [index] är ett värde i [0, BacklogQueueCount) |
| --- | --- |
| MaxSizeInMegabytes |5120 |
| MaxDeliveryCount |heltal. MaxValue |
| DefaultMessageTimeToLive |TimeSpan.MaxValue |
| AutoDeleteOnIdle |TimeSpan.MaxValue |
| Varaktighet |1 minut |
| EnableDeadLetteringOnMessageExpiration |SANT |
| EnableBatchedOperations |SANT |

Till exempel skapa hello första eftersläpning kön för namnområdet **contoso** heter `contoso/x-servicebus-transfer/0`.

När du skapar hello köer hello kod kontrollerar först toosee om det finns en sådan kö. Om hello kön inte finns, skapas hello kön. hello koden inte rensa ”extra” eftersläpning köer. Specifikt, om hello program med hello primära namnområde **contoso** begär fem eftersläpning köer, men en eftersläpning kö med sökvägen hello `contoso/x-servicebus-transfer/7` finns extra eftersläpning kön är kvar men används inte. hello system tillåter uttryckligen extra eftersläpning köer tooexist som inte används. Du är ansvarig för att rensa alla oanvända/oönskad eftersläpning köer som hello namnområde ägare. hello orsaken till detta beslut är att Service Bus inte kan vet vilket syfte som finns för alla hello köer i namnområdet. Dessutom, om en kö finns med namnet hello men inte uppfyller hello antas [QueueDescription][QueueDescription], din orsaker är din egen för ändra hello standardbeteendet. Inga garantier görs för ändringar toohello eftersläpning köer av din kod. Ändra till tootest noggrant.

## <a name="custom-messagesender"></a>Anpassade MessageSender
När du skickar, alla meddelanden gå igenom en intern [MessageSender] [ MessageSender] objekt som fungerar sedan normalt när allt fungerar och omdirigerar toohello eftersläpning köer när saker ”delar”. Vid mottagning av ett fel uppstod, startar en timer. När en [TimeSpan] [ TimeSpan] period som består av hello [FailoverInterval] [ FailoverInterval] egenskapsvärde under vilka inga lyckade meddelanden skickas , hello växling vid fel används. Nu händer hello följande för varje entitet:

* Ping-uppgiften körs varje [PingPrimaryInterval] [ PingPrimaryInterval] toocheck om hello enheten är tillgänglig. När den här aktiviteten lyckas startar alla klientkod som använder hello entitet omedelbart skicka nya meddelanden toohello primära namnområdet.
* Kommande begäranden toosend toothat samma entitet från andra sändare leder hello [BrokeredMessage] [ BrokeredMessage] skickas toobe ändrade toosit i hello eftersläpning kö. hello ändring tar bort vissa egenskaper från hello [BrokeredMessage] [ BrokeredMessage] objekt och lagrar dem på en annan plats. hello är följande egenskaper avmarkerad och läggas till under ett nytt alias, vilket gör att Service Bus och hello SDK tooprocess meddelanden enhetligt:

| Gamla egenskapsnamn | Nya egenskapsnamn |
| --- | --- |
| Sessions-ID |x-ms-sessions-ID |
| TimeToLive |x-ms-timetolive |
| ScheduledEnqueueTimeUtc |x-ms-sökväg |

hello ursprungliga målsökväg lagras också i hello-meddelande som en egenskap med namnet x-ms-sökväg. Den här designen kan meddelanden för många enheter toocoexist i en enda eftersläpning kö. hello egenskaper översätts igen genom hello syphon.

hello anpassade [MessageSender] [ MessageSender] objekt problem kan uppstå när meddelanden hanterar hello 256 KB-gränsen och växling vid fel används. hello anpassade [MessageSender] [ MessageSender] -objektet lagrar meddelanden för alla köer och ämnen tillsammans i hello eftersläpning köer. Det här objektet blandas meddelanden från många primärfärgerna tillsammans i hello eftersläpning köer. toohandle belastningsutjämningen bland många klienter som inte känner till alla andra hello SDK slumpmässigt hämtar en eftersläpning kö för varje [QueueClient] [ QueueClient] eller [TopicClient] [ TopicClient] du skapar i koden.

## <a name="pings"></a>Pingar
Ett ping-meddelande är ett tomt [BrokeredMessage] [ BrokeredMessage] med dess [ContentType] [ ContentType] egenskapsuppsättning tooapplication/vnd.ms-servicebus-ping och en [TimeToLive] [ TimeToLive] värdet 1 sekund. Den här ping har en särskild egenskap i Service Bus: hello aldrig skickas ett ping när alla anroparen begär en [BrokeredMessage][BrokeredMessage]. Du aldrig har alltså toolearn hur tooreceive och ignorera dessa meddelanden. Varje entitet (unikt kö eller ett ämne) per [MessagingFactory] [ MessagingFactory] instans per klient ska pingas när de anses vara toobe inte tillgänglig. Som standard är detta sker en gång per minut. Ping-meddelanden betraktas toobe vanliga Service Bus-meddelanden och kan resultera i kostnader för bandbredd och meddelanden. Så snart hello-klienter identifiera att hello systemet är tillgängligt, stoppa hälsningsmeddelande.

## <a name="hello-syphon"></a>Hej syphon
Minst ett körbart program i hello program bör aktivt köra hello syphon. Hej syphon utför lång avsökning får som pågår i 15 minuter. När du har 10 eftersläpning köer och alla entiteter som är tillgängliga hello program som är värd för hello syphon anrop hello mottagningsåtgärd 40 gånger per timme, 960 gånger per dag och 28800 gånger i 30 dagar. När hello syphon aktivt flyttar meddelanden från hello eftersläpning toohello primära kö, inträffar varje meddelande hello följande avgifter (standard kostnader för meddelandestorlek och bandbredd som används i alla led):

1. Skicka toohello eftersläpning.
2. Ta emot från hello eftersläpning.
3. Skicka toohello primära.
4. Ta emot från hello primära.

## <a name="closefault-behavior"></a>Stäng/fel beteende
I ett program som är värd för hello syphon, en gång hello primär eller sekundär [MessagingFactory] [ MessagingFactory] hos eller stängs utan dess partner samtidigt fel/stängs och hello syphon identifierar det här tillståndet , hello syphon fungerar. Om hello andra [MessagingFactory] [ MessagingFactory] inte är stängd inom 5 sekunder kommer hello syphon fault hello fortfarande öppen [MessagingFactory] [ MessagingFactory] .

## <a name="next-steps"></a>Nästa steg
Se [asynkrona meddelanden mönster och hög tillgänglighet] [ Asynchronous messaging patterns and high availability] för en detaljerad beskrivning av asynkrona Service Bus-meddelanden. 

[PairNamespaceAsync]: /dotnet/api/microsoft.servicebus.messaging.messagingfactory#Microsoft_ServiceBus_Messaging_MessagingFactory_PairNamespaceAsync_Microsoft_ServiceBus_Messaging_PairedNamespaceOptions_
[SendAvailabilityPairedNamespaceOptions]: /dotnet/api/microsoft.servicebus.messaging.sendavailabilitypairednamespaceoptions
[MessageSender]: /dotnet/api/microsoft.servicebus.messaging.messagesender
[MessagingFactory]: /dotnet/api/microsoft.servicebus.messaging.messagingfactory
[FailoverInterval]: /dotnet/api/microsoft.servicebus.messaging.pairednamespaceoptions#Microsoft_ServiceBus_Messaging_PairedNamespaceOptions_FailoverInterval
[MessagingException]: /dotnet/api/microsoft.servicebus.messaging.messagingexception
[TimeoutException]: https://msdn.microsoft.com/library/azure/system.timeoutexception.aspx
[BrokeredMessage]: /dotnet/api/microsoft.servicebus.messaging.brokeredmessage
[QueueDescription]: /dotnet/api/microsoft.servicebus.messaging.queuedescription
[TimeSpan]: https://msdn.microsoft.com/library/azure/system.timespan.aspx
[PingPrimaryInterval]: /dotnet/api/microsoft.servicebus.messaging.sendavailabilitypairednamespaceoptions#Microsoft_ServiceBus_Messaging_SendAvailabilityPairedNamespaceOptions_PingPrimaryInterval
[QueueClient]: /dotnet/api/microsoft.servicebus.messaging.queueclient
[TopicClient]: /dotnet/api/microsoft.servicebus.messaging.topicclient
[ContentType]: /dotnet/api/microsoft.servicebus.messaging.brokeredmessage#Microsoft_ServiceBus_Messaging_BrokeredMessage_ContentType
[TimeToLive]: /dotnet/api/microsoft.servicebus.messaging.brokeredmessage#Microsoft_ServiceBus_Messaging_BrokeredMessage_TimeToLive
[Asynchronous messaging patterns and high availability]: service-bus-async-messaging.md
[0]: ./media/service-bus-paired-namespaces/IC673405.png
[1]: ./media/service-bus-paired-namespaces/IC673406.png
[2]: ./media/service-bus-paired-namespaces/IC673407.png
