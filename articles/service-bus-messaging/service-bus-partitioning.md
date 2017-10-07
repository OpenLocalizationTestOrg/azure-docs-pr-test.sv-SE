---
title: "aaaCreate partitionerad Azure Service Bus-köer och ämnen | Microsoft Docs"
description: "Beskriver hur toopartition Service Bus-köer och ämnen med hjälp av flera meddelandet förmedlar."
services: service-bus-messaging
documentationcenter: na
author: sethmanheim
manager: timlt
editor: 
ms.assetid: a0c7d5a2-4876-42cb-8344-a1fc988746e7
ms.service: service-bus-messaging
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/07/2017
ms.author: sethm;hillaryc
ms.openlocfilehash: 6d42556a0714d6a012dc319f662521c8b0bb958b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="partitioned-queues-and-topics"></a>Partitionerade köer och ämnen
Azure Service Bus använder flera mäklare tooprocess meddelanden och meddelanden till flera lagrar toostore meddelanden. En vanlig kö eller ett ämne hanteras av en enda meddelande broker och lagras i ett meddelandearkiv. Service Bus *partitioner* aktivera köer och ämnen, eller *meddelandeentiteter*, toobe partitionerad över flera meddelandet mäklare och meddelandearkiv. Detta innebär att hello totala genomflödet av en partitionerad entitet begränsas inte längre av hello prestanda för ett enda meddelande broker eller meddelandearkiv. Dessutom kan återges ett tillfälligt avbrott i ett meddelandearkiv inte en partitionerad kö eller ett ämne inte tillgänglig. Partitionerade köer och ämnen kan innehålla alla avancerade Service Bus-funktioner, till exempel stöd för transaktioner och sessioner.

Information om Service Bus internals finns hello [Service Bus-arkitektur] [ Service Bus architecture] artikel.

Partitionering är aktiverad som standard när entiteten skapas på alla köer och ämnen i både Standard- och Premium-meddelanden. Du kan skapa Standard meddelandeentiteter nivå utan partitionering, men köer och ämnen i ett Premium-namnområde partitioneras alltid; Det här alternativet kan inte inaktiveras. 

Det är inte möjligt toochange hello alternativet på en befintlig kö eller i nivåerna Standard eller Premium-partitionering, kan du bara ange hello alternativ när du skapar hello entitet.

## <a name="how-it-works"></a>Hur det fungerar

Varje partitionerade kö eller ett ämne består av flera fragment. Varje fragment lagras i en annan meddelandearkiv och hanteras av ett annat meddelande broker. När ett meddelande skickas tooa partitionerade kö eller ett ämne, tilldelar Service Bus hello-meddelande tooone av hello-fragment. hello val görs slumpmässigt Service Bus eller med hjälp av en partitionsnyckel som hello avsändaren kan ange.

När en klient vill tooreceive ett meddelande från en partitionerad kö eller en prenumeration tooa partitionerade artikeln, Service Bus frågar alla fragment för meddelanden, returnerar du första hello-meddelande som hämtas från någon av hello meddelanden lagrar toohello mottagare. Service Bus cacheminnen hello andra meddelanden och returnerar dem när den tar emot ytterligare ta emot begäranden. En mottagande klienten är inte medveten om hello partitionering; Hej klientriktade beteendet för en partitionerad kö eller ett ämne (till exempel läsa, slutföra, skjuta upp systemkön, förhämtar) är identiska toohello beteendet för en vanlig entitet.

Det finns inga extra kostnad när de skickar ett meddelande till eller ta emot ett meddelande från en partitionerad kö eller ett ämne.

## <a name="enable-partitioning"></a>Aktivera partitionering

toouse partitionerad köer och ämnen med Azure Service Bus använder hello Azure SDK version 2.2 eller senare, eller ange `api-version=2013-10` i HTTP-begäranden.

### <a name="standard"></a>Standard

I hello meddelanden standardnivån, kan du skapa Service Bus-köer och ämnen i 1, 2, 3, 4 och 5 GB storlekar (hello standardvärdet är 1 GB). Med partitionering aktiverats, skapar Service Bus 16 kopior (16 partitioner) för hello entitet för varje GB som du anger. Så om du skapar en kö är 5 GB i storlek med 16 partitioner hello största köstorlek blir (5 \* 16) = 80 GB. Du kan se hello maximal storlek för din partitionerade kö eller ett ämne genom att titta på posten på hello [Azure-portalen][Azure portal], i hello **översikt** bladet för den enheten.

### <a name="premium"></a>Premium

I ett namnområde för Premium-nivån, kan du skapa Service Bus-köer och ämnen i 1, 2, 3, 4, 5, 10, 20, 40 eller 80 GB storlekar (hello standardvärdet är 1 GB). Med partitionering aktiverad som standard skapar Service Bus två partitioner per enhet. Du kan se hello maximal storlek för din partitionerade kö eller ett ämne genom att titta på posten på hello [Azure-portalen][Azure portal], i hello **översikt** bladet för den enheten.

Mer information om partitionering i hello Premium messaging-nivån finns [Service Bus Premium- och Standard-meddelandenivåer](service-bus-premium-messaging.md). 

### <a name="create-a-partitioned-entity"></a>Skapa en partitionerad entitet

Det finns flera sätt toocreate en partitionerad kö eller ett ämne. När du skapar hello kö eller ett ämne från ditt program måste du aktivera partitionering för hello kö eller ett ämne med respektive inställningen hello [QueueDescription.EnablePartitioning] [ QueueDescription.EnablePartitioning] eller [TopicDescription.EnablePartitioning] [ TopicDescription.EnablePartitioning] egenskapen för**SANT**. Dessa egenskaper måste anges vid hello tid hello kön eller avsnittet skapas. Som nämnts tidigare anger är det inte möjligt toochange dessa egenskaper för en befintlig kö eller ett ämne. Exempel:

```csharp
// Create partitioned topic
NamespaceManager ns = NamespaceManager.CreateFromConnectionString(myConnectionString);
TopicDescription td = new TopicDescription(TopicName);
td.EnablePartitioning = true;
ns.CreateTopic(td);
```

Du kan också skapa en partitionerad kö eller ett ämne i hello [Azure-portalen] [ Azure portal] eller i Visual Studio. När du skapar en kö eller ett ämne i hello portal hello **aktivera partitionering** alternativ i hello kö eller ett ämne **skapa** bladet är markerad som standard. Du kan bara inaktivera det här alternativet i en standardnivån enheten. i hello är Premium nivå partitionering alltid aktiverat. I Visual Studio klickar du på hello **aktivera partitionering** kryssrutan i hello **ny kö** eller **ny artikel** dialogrutan.

## <a name="use-of-partition-keys"></a>Användning av partitionsnycklar
När ett meddelande är i kö i en partitionerad kö eller ett ämne, kontrollerar Service Bus hello förekomsten av en partitionsnyckel. Om den hittar en väljs hello fragment baserat på nyckeln. Om det inte hittar en partitionsnyckel, väljs hello fragment baserat på en intern algoritm.

### <a name="using-a-partition-key"></a>Med hjälp av en partitionsnyckel
Vissa scenarier, till exempel sessioner eller transaktioner, kräver meddelanden toobe lagras i en specifik fragment. Dessa scenarier kräver hello använder en partitionsnyckel. Alla meddelanden som används hello tilldelas samma partitionsnyckel toohello samma fragment. Om hello fragment är tillfälligt otillgänglig, returneras ett fel i Service Bus.

Beroende på hello scenario används olika meddelandeegenskaper som en partitionsnyckel:

**Sessions-ID**: om ett meddelande har hello [BrokeredMessage.SessionId] [ BrokeredMessage.SessionId] egenskapsuppsättning och Service Bus använder den här egenskapen som hello partitionsnyckel. Det här sättet, alla meddelanden som tillhör toohello samma session hanteras av hello samma meddelande broker. Detta gör att Service Bus tooguarantee meddelande ordning samt hello konsekvenskontroll av sessionstillstånd.

**PartitionKey**: om ett meddelande har hello [BrokeredMessage.PartitionKey] [ BrokeredMessage.PartitionKey] egenskapen men inte hello [BrokeredMessage.SessionId] [ BrokeredMessage.SessionId] egenskapsuppsättning och Service Bus använder hello [PartitionKey] [ PartitionKey] egenskap som hello partitionsnyckel. Om hello-meddelande har båda hello [SessionId] [ SessionId] och hello [PartitionKey] [ PartitionKey] egenskaper, båda egenskaperna måste vara identiska. Om hello [PartitionKey] [ PartitionKey] tooa annat värde än hello anges i egenskapen [SessionId] [ SessionId] Service Bus-egenskapen returnerar en ogiltig åtgärden undantag. Hej [PartitionKey] [ PartitionKey] egenskapen ska användas om en avsändare skickar-session medveten transaktionella meddelanden. hello partitionsnyckel garanterar att alla meddelanden som skickas i en transaktion som hanteras av hello samma asynkrona broker.

**MessageId**: om hello kö eller ett ämne har hello [QueueDescription.RequiresDuplicateDetection] [ QueueDescription.RequiresDuplicateDetection] egenskapsuppsättning för**SANT** och hello [BrokeredMessage.SessionId] [ BrokeredMessage.SessionId] eller [BrokeredMessage.PartitionKey] [ BrokeredMessage.PartitionKey] egenskaper har inte angetts, och sedan hello [BrokeredMessage.MessageId] [ BrokeredMessage.MessageId] egenskapen fungerar som hello partitionsnyckel. (Observera att hello bibliotek för Microsoft .NET och AMQP automatiskt tilldela en meddelande-ID om hello avsändarprogrammet inte.) I det här fallet hello alla kopior av hello samma meddelande hanteras av samma meddelande broker. Detta gör att Service Bus toodetect och undvika dubbla meddelanden. Om hello [QueueDescription.RequiresDuplicateDetection] [ QueueDescription.RequiresDuplicateDetection] egenskapen har inte angetts för**SANT**, Service Bus anses inte hello [MessageId] [ MessageId] egenskapen som en partitionsnyckel.

### <a name="not-using-a-partition-key"></a>Inte använder en partitionsnyckel
Hello saknas en partitionsnyckel distribuerar Service Bus meddelanden i en resursallokering sätt tooall hello fragment av hello partitionerade kö eller ett ämne. Om hello valt fragment inte är tillgänglig, tilldelar Service Bus hello meddelandet tooa olika fragment. Det här sättet hello send-åtgärden lyckas trots hello temporär otillgänglighet ett meddelandearkiv. Du kommer dock inte får hello garanteras ordning att tillhandahåller en partitionsnyckel.

En mer detaljerad beskrivning av hello förhållandet mellan tillgänglighet (ingen partitionsnyckel) och konsekvenskontroll (med en partitionsnyckel) finns [i den här artikeln](../event-hubs/event-hubs-availability-and-consistency.md). Den här informationen gäller även toopartitioned Service Bus-entiteter och Händelsehubbar partitioner.

toogive Service Bus tillräckligt med tid tooenqueue hello-meddelande till en annan fragment hello [MessagingFactorySettings.OperationTimeout] [ MessagingFactorySettings.OperationTimeout] värdet som anges av hello-klient som skickar hello-meddelande måste vara större än 15 sekunder. Vi rekommenderar att du ställer in hello [OperationTimeout] [ OperationTimeout] egenskapen toohello standardvärdet 60 sekunder.

Observera att en partitionsnyckel ”PIN” ett meddelande tooa fragment. Om hello meddelandearkiv som innehåller det här fragmentet är tillgänglig, returneras ett fel i Service Bus. Hello saknas en partitionsnyckel, Service Bus kan välja ett annat fragment och hello åtgärden lyckas. Vi rekommenderar därför att du inte anger en partitionsnyckel om det inte krävs.

## <a name="advanced-topics-use-transactions-with-partitioned-entities"></a>Avancerade alternativ: använda transaktioner med partitionerade enheter
Meddelanden som skickas som en del av en transaktion måste ange en partitionsnyckel. Detta kan vara något av följande egenskaper hello: [BrokeredMessage.SessionId][BrokeredMessage.SessionId], [BrokeredMessage.PartitionKey][BrokeredMessage.PartitionKey], eller [ BrokeredMessage.MessageId][BrokeredMessage.MessageId]. Alla meddelanden som skickas som en del av samma transaktion måste ange hello hello samma partitionsnyckel. Om du försöker toosend ett meddelande utan någon partitionsnyckel inom en transaktion, returnerar Service Bus en ogiltig åtgärd undantag. Om du försöker toosend flera meddelanden i hello samma transaktion som har olika partitionsnycklar, Service Bus returnerar en ogiltig åtgärd undantag. Exempel:

```csharp
CommittableTransaction committableTransaction = new CommittableTransaction();
using (TransactionScope ts = new TransactionScope(committableTransaction))
{
    BrokeredMessage msg = new BrokeredMessage("This is a message");
    msg.PartitionKey = "myPartitionKey";
    messageSender.Send(msg); 
    ts.Complete();
}
committableTransaction.Commit();
```

Om någon av hello egenskaper som fungerar som en partitionsnyckel konfigureras hello Service Bus PIN-koder meddelandet tooa specifika fragment. Detta inträffar oavsett används för en transaktion. Vi rekommenderar att du inte anger en partitionsnyckel om det inte är nödvändigt.

## <a name="using-sessions-with-partitioned-entities"></a>Sessioner med partitionerade enheter
toosend ett transaktionsmeddelande tooa session-medvetna ämne eller kö hello-meddelande måste ha hello [BrokeredMessage.SessionId] [ BrokeredMessage.SessionId] egenskapsuppsättning. Om hello [BrokeredMessage.PartitionKey] [ BrokeredMessage.PartitionKey] egenskapen anges också måste den vara identiska toohello [SessionId] [ SessionId] egenskapen. Om de skiljer sig returnerar en ogiltig åtgärd undantag i Service Bus.

Till skillnad från vanliga (partitionerade) köer och ämnen är det inte möjligt toouse en enda transaktion toosend flera meddelanden toodifferent sessioner. Om försöktes, returnerar Service Bus en ogiltig åtgärd undantag. Exempel:

```csharp
CommittableTransaction committableTransaction = new CommittableTransaction();
using (TransactionScope ts = new TransactionScope(committableTransaction))
{
    BrokeredMessage msg = new BrokeredMessage("This is a message");
    msg.SessionId = "mySession";
    messageSender.Send(msg); 
    ts.Complete();
}
committableTransaction.Commit();
```

## <a name="automatic-message-forwarding-with-partitioned-entities"></a>Automatisk vidarebefordring med partitionerade enheter
Service Bus stöder automatisk meddelandet vidarebefordran från, eller för mellan partitionerade enheter. tooenable meddelanden vidarebefordras måste ange hello [QueueDescription.ForwardTo] [ QueueDescription.ForwardTo] egenskapen hello källkön eller prenumeration. Om hello-meddelande som anger en partitionsnyckel ([SessionId][SessionId], [PartitionKey][PartitionKey], eller [MessageId] [ MessageId]), som partitionsnyckel används för hello målentitet.

## <a name="considerations-and-guidelines"></a>Information och riktlinjer
* **Hög konsekvenskontroll funktioner**: om en enhet använder funktioner som sessioner, identifiering av dubbletter eller explicit kontroll över partitionering nyckeln och sedan hello asynkrona åtgärder är alltid dirigeras toospecific fragment. Om någon av hello fragment drabbas av hög trafik eller hello underliggande arkivet är i feltillstånd, dessa åtgärder misslyckas och tillgänglighet minskar. Generellt sett är hello konsekvenskontroll fortfarande mycket högre än partitionerade enheter. endast en del av trafiken upplever problem, som skillnad från tooall hello trafik. Mer information finns i det här [beskrivning av tillgänglighet och konsekvenskontroll](../event-hubs/event-hubs-availability-and-consistency.md).
* **Hantering av**: åtgärder, till exempel skapa, uppdatera och ta bort måste utföras på alla hello fragment av hello entitet. Om några fragment är ohälsosamt kan det resultera i fel för dessa åtgärder. För hämta-åtgärden hello måste information såsom meddelandet räknar aggregeras från alla fragment. Om några fragment är felfri har hello Tillgänglighetsstatus för entiteten rapporterats som begränsad.
* **Volymen meddelandet scenarier med låg**: för sådana scenarier, särskilt när du använder hello HTTP-protokollet, kanske tooperform flera får operations i ordning tooobtain alla hälsningsmeddelande. Ta emot förfrågningar hello klientdelen utför en receive på alla hello fragment och cachelagrar alla hello-svar som tagits emot. En efterföljande receive-begäran på hello samma anslutning kan dra nytta av den här cachelagring och ta emot svarstiderna blir lägre. Men om du har flera anslutningar eller använda HTTP upprättar som en ny anslutning för varje begäran. Det finns därför ingen garanti för att den skulle hamna på hello samma nod. Om alla befintliga meddelanden är låst och cachelagras i en annan klientdelen hello får åtgärden returnerar **null**. Meddelanden till slut upphör och du kan ta emot dem igen. HTTP keep-alive rekommenderas.
* **Bläddra/titt meddelanden**: [PeekBatch](/dotnet/api/microsoft.servicebus.messaging.queueclient#Microsoft_ServiceBus_Messaging_QueueClient_PeekBatch_System_Int32_) inte alltid returnerar hello antal meddelanden som anges i hello [MessageCount](/dotnet/api/microsoft.servicebus.messaging.queuedescription#Microsoft_ServiceBus_Messaging_QueueDescription_MessageCount) egenskapen. Det finns två vanliga orsaker till detta. En orsak är att hello hello samling meddelanden aggregerade storlek överskrider hello maximal storlek på 256KB. En annan orsak är att om hello kö eller ett ämne har hello [EnablePartitioning egenskapen](/dotnet/api/microsoft.servicebus.messaging.queuedescription#Microsoft_ServiceBus_Messaging_QueueDescription_EnablePartitioning) ställa in också**SANT**, en partition kan inte ha tillräckligt många meddelanden toocomplete hello begärt antal meddelanden. I allmänhet tooreceive ett visst antal meddelanden om ett program, den ska anropa [PeekBatch](/dotnet/api/microsoft.servicebus.messaging.queueclient#Microsoft_ServiceBus_Messaging_QueueClient_PeekBatch_System_Int32_) upprepade gånger tills den hämtar det antalet meddelanden eller så finns det inga fler meddelanden toopeek. Mer information, inklusive kodexempel, se [QueueClient.PeekBatch](/dotnet/api/microsoft.servicebus.messaging.queueclient#Microsoft_ServiceBus_Messaging_QueueClient_PeekBatch_System_Int32_) eller [SubscriptionClient.PeekBatch](/dotnet/api/microsoft.servicebus.messaging.subscriptionclient#Microsoft_ServiceBus_Messaging_SubscriptionClient_PeekBatch_System_Int32_).

## <a name="latest-added-features"></a>Senaste extrafunktioner
* Lägg till eller ta bort regeln stöds nu med partitionerade enheter. Skiljer sig från partitionerade enheter, dessa åtgärder stöds inte under transaktioner. 
* AMQP stöds nu för att skicka och ta emot meddelanden tooand från en partitionerad entitet.
* AMQP har nu stöd för hello följande åtgärder: [Batch skicka](/dotnet/api/microsoft.servicebus.messaging.queueclient#Microsoft_ServiceBus_Messaging_QueueClient_SendBatch_System_Collections_Generic_IEnumerable_Microsoft_ServiceBus_Messaging_BrokeredMessage__), [Batch får](/dotnet/api/microsoft.servicebus.messaging.queueclient#Microsoft_ServiceBus_Messaging_QueueClient_ReceiveBatch_System_Int32_), [ta emot med sekvensnumret](/dotnet/api/microsoft.servicebus.messaging.queueclient#Microsoft_ServiceBus_Messaging_QueueClient_Receive_System_Int64_), [granska](/dotnet/api/microsoft.servicebus.messaging.queueclient#Microsoft_ServiceBus_Messaging_QueueClient_Peek), [ Förnya Lås](/dotnet/api/microsoft.servicebus.messaging.queueclient#Microsoft_ServiceBus_Messaging_QueueClient_RenewMessageLock_System_Guid_), [schemalägga meddelandet](/dotnet/api/microsoft.servicebus.messaging.queueclient#Microsoft_ServiceBus_Messaging_QueueClient_ScheduleMessageAsync_Microsoft_ServiceBus_Messaging_BrokeredMessage_System_DateTimeOffset_), [Avbryt schemalagda meddelande](/dotnet/api/microsoft.servicebus.messaging.queueclient#Microsoft_ServiceBus_Messaging_QueueClient_CancelScheduledMessageAsync_System_Int64_), [Lägg till regel](/dotnet/api/microsoft.servicebus.messaging.ruledescription), [ta bort regeln](/dotnet/api/microsoft.servicebus.messaging.ruledescription), [Session förnya Lås](/dotnet/api/microsoft.servicebus.messaging.messagesession#Microsoft_ServiceBus_Messaging_MessageSession_RenewLock), [Set sessionstillstånd](/dotnet/api/microsoft.servicebus.messaging.messagesession#Microsoft_ServiceBus_Messaging_MessageSession_SetState_System_IO_Stream_), [Get sessionstillstånd](/dotnet/api/microsoft.servicebus.messaging.messagesession#Microsoft_ServiceBus_Messaging_MessageSession_GetState), och [räkna upp sessioner](/dotnet/api/microsoft.servicebus.messaging.queueclient#Microsoft_ServiceBus_Messaging_QueueClient_GetMessageSessionsAsync).

## <a name="partitioned-entities-limitations"></a>Begränsningar för partitionerade enheter
Service Bus medför för närvarande hello följande begränsningar på partitionerade köer och ämnen:

* Partitionerade köer och ämnen stöder inte skicka meddelanden som tillhör toodifferent sessioner i en enda transaktion.
* Service Bus kan för närvarande in too100 partitionerad köer och ämnen per namnområde. Varje partitionerade kö eller ett ämne räknar mot hello kvoten på 10 000 enheter per namnområde (inte gäller tooPremium nivån).

## <a name="next-steps"></a>Nästa steg
Visa hello diskussion av [AMQP 1.0-stöd för Service Bus partitionerad köer och ämnen] [ AMQP 1.0 support for Service Bus partitioned queues and topics] toolearn mer om partitionering meddelandeentiteter. 

[Service Bus architecture]: service-bus-architecture.md
[Azure portal]: https://portal.azure.com
[QueueDescription.EnablePartitioning]: /dotnet/api/microsoft.servicebus.messaging.queuedescription#Microsoft_ServiceBus_Messaging_QueueDescription_EnablePartitioning
[TopicDescription.EnablePartitioning]: /dotnet/api/microsoft.servicebus.messaging.topicdescription#Microsoft_ServiceBus_Messaging_TopicDescription_EnablePartitioning
[BrokeredMessage.SessionId]: /dotnet/api/microsoft.servicebus.messaging.brokeredmessage#Microsoft_ServiceBus_Messaging_BrokeredMessage_SessionId
[BrokeredMessage.PartitionKey]: /dotnet/api/microsoft.servicebus.messaging.brokeredmessage#Microsoft_ServiceBus_Messaging_BrokeredMessage_PartitionKey
[SessionId]: /dotnet/api/microsoft.servicebus.messaging.brokeredmessage#Microsoft_ServiceBus_Messaging_BrokeredMessage_SessionId
[PartitionKey]: /dotnet/api/microsoft.servicebus.messaging.brokeredmessage#Microsoft_ServiceBus_Messaging_BrokeredMessage_PartitionKey
[QueueDescription.RequiresDuplicateDetection]: /dotnet/api/microsoft.servicebus.messaging.queuedescription#Microsoft_ServiceBus_Messaging_QueueDescription_RequiresDuplicateDetection
[BrokeredMessage.MessageId]: /dotnet/api/microsoft.servicebus.messaging.brokeredmessage#Microsoft_ServiceBus_Messaging_BrokeredMessage_MessageId
[MessageId]: /dotnet/api/microsoft.servicebus.messaging.brokeredmessage#Microsoft_ServiceBus_Messaging_BrokeredMessage_MessageId
[MessagingFactorySettings.OperationTimeout]: /dotnet/api/microsoft.servicebus.messaging.messagingfactorysettings#Microsoft_ServiceBus_Messaging_MessagingFactorySettings_OperationTimeout
[OperationTimeout]: /dotnet/api/microsoft.servicebus.messaging.messagingfactorysettings#Microsoft_ServiceBus_Messaging_MessagingFactorySettings_OperationTimeout
[QueueDescription.ForwardTo]: /dotnet/api/microsoft.servicebus.messaging.queuedescription#Microsoft_ServiceBus_Messaging_QueueDescription_ForwardTo
[AMQP 1.0 support for Service Bus partitioned queues and topics]: service-bus-partitioned-queues-and-topics-amqp-overview.md
