---
title: "aaaOverview i Azure Service Bus-meddelanden köer, ämnen och prenumerationer | Microsoft Docs"
description: "Översikt över Service Bus meddelandeentiteter."
services: service-bus-messaging
documentationcenter: na
author: sethmanheim
manager: timlt
editor: 
ms.assetid: a306ced4-74e9-47c6-990a-d9c47efa31d5
ms.service: service-bus-messaging
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/28/2017
ms.author: sethm
ms.openlocfilehash: 73135d2658e341c14dbb114ab938faed91578ff1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="service-bus-queues-topics-and-subscriptions"></a>Service Bus-köer, -ämnen och -prenumerationer

Microsoft Azure Service Bus stöder en uppsättning molnbaserade, meddelande indatavärdena mellanprogram teknologier, inklusive tillförlitliga message Queuing- och varaktig Publicera/prenumerera på meddelanden. Dessa ”asynkrona” meddelandefunktioner kan betraktas som frikopplad messaging funktioner som stöd för att publicera och prenumerera, temporal Frikoppling och scenarier med hjälp av hello Service Bus-meddelanden fabric för belastningsutjämning. Frikopplad kommunikation har många fördelar: klienter och servrar kan till exempel ansluta efter behov och utföra åtgärder på ett asynkront sätt.

Hej meddelandeentiteter som bildar hello kärnan i hello messaging funktionerna i Service Bus är köer, ämnen och prenumerationer och regler/en åtgärd.

## <a name="queues"></a>Köer

Köer erbjuder *First In, först ut* (FIFO) meddelandet leverans tooone eller flera konkurrerande konsumenter. Det vill säga är meddelanden vanligtvis förväntade toobe emot och bearbetas av hello mottagarna i hello ordning som de har lagts till toohello kön, och varje meddelande tas emot och bearbetas av bara en meddelandekonsument. En stor fördel med köer är tooachieve ”temporal Frikoppling” av programkomponenter. Med andra ord hello producenter (avsändare) och konsumenter (mottagare) behöver inte toobe skicka och ta emot meddelanden på hello samma tid, eftersom meddelanden lagras varaktigt i hello kö. Dessutom hello producenten inte har toowait på ett svar från konsumenten hello i ordning toocontinue tooprocess och skicka meddelanden.

En relaterad fördel är ”belastning utjämning”, som gör att producenter och konsumenter toosend och ta emot meddelanden med olika hastigheter. I många program varierar systembelastningen hello med tiden. dock är hello bearbetningstid som krävs för varje arbetsenhet vanligtvis är konstant. Medlingen mellan meddelandeproducenter och konsumenter med hjälp av en kö innebär att hello förbrukar programmet bara toobe etablerade toobe kan toohandle genomsnittlig belastning i stället för belastning. hello kön hello djup växer och dras samman allt eftersom hello inkommande belastningen varierar. Detta sparar direkt pengar med beaktande toohello mängd infrastruktur krävs tooservice hello programinläsning. Som hello belastningen ökar kan fler arbetsprocesser kan tillagda tooread från hello kö. Varje meddelande bearbetas av bara en av hello arbetsprocesser. Dessutom kan den här pull-baserade belastningsutjämning, för optimal användning av hello worker datorer även om hello worker datorer skiljer sig med beaktande tooprocessing power som de hämtar meddelanden med sina egna högsta hastighet. Det här mönstret kallas ofta ”konkurrerande konsument” hello-mönster.

Med hjälp av köer toointermediate mellan meddelandeproducenter och konsumenter innehåller en inbyggd lösa kopplingar mellan hello komponenter. Eftersom producenter och konsumenter inte är medvetna om varandra, kan en konsument uppgraderas utan någon inverkan på hello producenten.

Skapa en kö är en process med flera steg. Du kan utföra hanteringsåtgärder för Service Bus meddelandeentiteter (köer och ämnen) via hello [Microsoft.ServiceBus.NamespaceManager](/dotnet/api/microsoft.servicebus.namespacemanager#microsoft_servicebus_namespacemanager) -klassen, som har skapats genom att ange hello basadressen för hello Service Bus namnområdet och hello autentiseringsuppgifter. [NamespaceManager](/dotnet/api/microsoft.servicebus.namespacemanager#microsoft_servicebus_namespacemanager) tillhandahåller metoder toocreate räkna upp och ta bort meddelandeentiteter. När du har skapat en [Microsoft.ServiceBus.TokenProvider](/dotnet/api/microsoft.servicebus.tokenprovider#microsoft_servicebus_tokenprovider) objekt från hello SAS-namn och nyckel och ett namnområde servicehantering objekt, kan du använda hello [Microsoft.ServiceBus.NamespaceManager.CreateQueue](/dotnet/api/microsoft.servicebus.namespacemanager#Microsoft_ServiceBus_NamespaceManager_CreateQueue_System_String_) metoden toocreate hello kön. Exempel:

```csharp
// Create management credentials
TokenProvider credentials = TokenProvider.CreateSharedAccessSignatureTokenProvider(sasKeyName,sasKeyValue);
// Create namespace client
NamespaceManager namespaceClient = new NamespaceManager(ServiceBusEnvironment.CreateServiceUri("sb", ServiceNamespace, string.Empty), credentials);
```

Du kan sedan skapa ett köobjekt och en meddelandefabrik med hello Service Bus-URI som argument. Exempel:

```csharp
QueueDescription myQueue;
myQueue = namespaceClient.CreateQueue("TestQueue");
MessagingFactory factory = MessagingFactory.Create(ServiceBusEnvironment.CreateServiceUri("sb", ServiceNamespace, string.Empty), credentials); 
QueueClient myQueueClient = factory.CreateQueueClient("TestQueue");
```

Du kan skicka meddelanden toohello kön. Om du har en lista över asynkrona meddelanden som kallas exempelvis `MessageList`, hello kod visas liknande toohello följande:

```csharp
for (int count = 0; count < 6; count++)
{
    var issue = MessageList[count];
    issue.Label = issue.Properties["IssueTitle"].ToString();
    myQueueClient.Send(issue);
}
```

Du sedan ta emot meddelanden från kön hello på följande sätt:

```csharp
while ((message = myQueueClient.Receive(new TimeSpan(hours: 0, minutes: 0, seconds: 5))) != null)
    {
        Console.WriteLine(string.Format("Message received: {0}, {1}, {2}", message.SequenceNumber, message.Label, message.MessageId));
        message.Complete();

        Console.WriteLine("Processing message (sleeping...)");
        Thread.Sleep(1000);
    }
```

I hello [ReceiveAndDelete](/dotnet/api/microsoft.servicebus.messaging.receivemode) läge hello får åtgärden är en; som är när Service Bus tar emot hello begäran, den markerar hello meddelandet som Förbrukat och skickar tillbaka det toohello program. **ReceiveAndDelete** läge är hello enklaste modellen och fungerar bäst för scenarier där hello program kan tolerera icke-bearbetning av ett meddelande i hello händelse av fel. toounderstand, Föreställ i vilka hello konsumenten problem hello mottagning av begäran och sedan kraschar innan bearbetningen. Eftersom Service Bus markerar hello meddelandet som Förbrukat när hello programmet startas om och börjar förbruka meddelanden igen, att ha missat hello-meddelande som har förbrukats tidigare toohello krascher.

I [PeekLock](/dotnet/api/microsoft.servicebus.messaging.receivemode) läge hello får åtgärden blir två steg, vilket gör det möjligt toosupport program som inte tolererar att ett meddelande saknas. När Service Bus tar emot hello begäran, den hittar hello nästa meddelande toobe förbrukas, låser det tooprevent andra användare tar emot den, och returnerar sedan toohello program. När hello programmet har avslutat bearbetningen hello-meddelande (eller lagrar det på ett tillförlitligt sätt för framtida bearbetning), Slutför hello andra steget i hello får processen genom att anropa [Slutför](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage#Microsoft_ServiceBus_Messaging_BrokeredMessage_Complete) på emot hello-meddelande. När Service Bus ser hello **Slutför** anrop, markeras hello-meddelande som används.

Om programmet hello inte tooprocess Hej meddelande av någon anledning, kan det anropa hello [Avbryt](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage#Microsoft_ServiceBus_Messaging_BrokeredMessage_Abandon) metod för hello tog emot meddelande (i stället för [Slutför](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage#Microsoft_ServiceBus_Messaging_BrokeredMessage_Complete)). Detta gör att Service Bus toounlock hello-meddelande och göra den tillgänglig toobe tas emot igen, antingen genom att hello samma konsumenten eller av en annan konkurrerande konsument. För det andra, det är en tidsgräns som är associerade med hello Lås och om hello programmet misslyckas tooprocess hello meddelande innan timeout för lås hello går ut (till exempel om hello programmet kraschar), kommer Service Bus låser upp hello-meddelande och gör den tillgänglig toobe tas emot igen (i praktiken utför en [Avbryt](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage#Microsoft_ServiceBus_Messaging_BrokeredMessage_Abandon) åtgärden som standard).

Observera att i hello händelse som hello programmet kraschar efter hello meddelandet har bearbetats men innan hello **Slutför** begäran har utfärdats, hello-meddelande är toohello levereras på nytt program när den startas om. Det här kallas ofta *minst när* bearbetning, det vill säga varje meddelande bearbetas minst en gång. Men i vissa situationer hello levereras samma meddelande. Om hello scenariot inte tolererar duplicerad bearbetning och sedan ytterligare logik som krävs i hello programmet toodetect dubbletter som kan uppnås baserat på hello **MessageId** -egenskapen för hello-meddelande som förblir konstant under alla leveransförsök. Detta kallas *exakt en gång* bearbetning.

## <a name="topics-and-subscriptions"></a>Ämnen och prenumerationer
I kontrast tooqueues, där varje meddelande bearbetas av en enskild konsument *avsnitt* och *prenumerationer* tillhandahålla en en-till-många-kommunikation i en *förPublicera/prenumerera* mönster. Användbar för att skala toovery stort antal mottagare, varje publicerat meddelande görs tillgängligt tooeach prenumeration som har registrerats med hello-avsnittet. Meddelanden skickas tooa avsnittet och levererat tooone eller flera associerade prenumerationer, beroende på filterregler som kan ställas in på grundval av per prenumeration. hello prenumerationer kan använda ytterligare filter toorestrict hälsningsmeddelande som de vill tooreceive. Meddelanden skickas tooa avsnittet hello samma sätt de skickas tooa kön, men meddelanden tas inte emot hello artikeln direkt. I stället tas de emot från prenumerationer. En prenumeration på artikeln liknar en virtuell kö som tar emot kopior av hello-meddelanden som skickas toohello avsnittet. Meddelanden tas emot från en prenumeration identiskt toohello sätt de tas emot från en kö.

Som jämförelse hello sändning av meddelande funktionerna i en kö mappar direkt tooa avsnittet och dess funktioner för message-mottagning mappar tooa prenumeration. Det innebär att prenumerationer stöder hello samma mönster som beskrivits tidigare i det här avsnittet med beaktande tooqueues bland annat: konkurrerande konsument, temporal Frikoppling, utjämna belastningen och belastningsutjämning.

Att skapa ett ämne är liknande toocreating en kö, enligt hello exempel hello föregående avsnitt. Skapa hello-tjänstens URI och sedan använda hello [NamespaceManager](/dotnet/api/microsoft.servicebus.namespacemanager) klassen toocreate hello namnområde klienten. Du kan sedan skapa ett avsnitt med hello [CreateTopic](/dotnet/api/microsoft.servicebus.namespacemanager#Microsoft_ServiceBus_NamespaceManager_CreateTopic_System_String_) metod. Exempel:

```csharp
TopicDescription dataCollectionTopic = namespaceClient.CreateTopic("DataCollectionTopic");
```

Lägg till prenumerationer som du vill:

```csharp
SubscriptionDescription myAgentSubscription = namespaceClient.CreateSubscription(myTopic.Path, "Inventory");
SubscriptionDescription myAuditSubscription = namespaceClient.CreateSubscription(myTopic.Path, "Dashboard");
```

Du kan sedan skapa en klient i avsnittet. Exempel:

```csharp
MessagingFactory factory = MessagingFactory.Create(serviceUri, tokenProvider);
TopicClient myTopicClient = factory.CreateTopicClient(myTopic.Path)
```

Med hello avsändaren kan du skicka och ta emot meddelanden tooand hello artikeln enligt hello föregående avsnitt. Exempel:

```csharp
foreach (BrokeredMessage message in messageList)
{
    myTopicClient.Send(message);
    Console.WriteLine(
    string.Format("Message sent: Id = {0}, Body = {1}", message.MessageId, message.GetBody<string>()));
}
```

Liknande tooqueues, meddelanden tas emot från en prenumeration med hjälp av en [SubscriptionClient](/dotnet/api/microsoft.servicebus.messaging.subscriptionclient) objekt i stället för en [QueueClient](/dotnet/api/microsoft.servicebus.messaging.queueclient) objekt. Skapa hello prenumeration klienten, skickar hello namnet på hello avsnittet hello namnet på hello prenumeration, och (valfritt) hello mottagningsläge som parametrar. Till exempel med hello **inventering** prenumerationen:

```csharp
// Create hello subscription client
MessagingFactory factory = MessagingFactory.Create(serviceUri, tokenProvider); 

SubscriptionClient agentSubscriptionClient = factory.CreateSubscriptionClient("IssueTrackingTopic", "Inventory", ReceiveMode.PeekLock);
SubscriptionClient auditSubscriptionClient = factory.CreateSubscriptionClient("IssueTrackingTopic", "Dashboard", ReceiveMode.ReceiveAndDelete); 

while ((message = agentSubscriptionClient.Receive(TimeSpan.FromSeconds(5))) != null)
{
    Console.WriteLine("\nReceiving message from Inventory...");
    Console.WriteLine(string.Format("Message received: Id = {0}, Body = {1}", message.MessageId, message.GetBody<string>()));
    message.Complete();
}          

// Create a receiver using ReceiveAndDelete mode
while ((message = auditSubscriptionClient.Receive(TimeSpan.FromSeconds(5))) != null)
{
    Console.WriteLine("\nReceiving message from Dashboard...");
    Console.WriteLine(string.Format("Message received: Id = {0}, Body = {1}", message.MessageId, message.GetBody<string>()));
}
```

### <a name="rules-and-actions"></a>Regler och åtgärder
I många fall är måste meddelanden som har specifika egenskaper bearbetas på olika sätt. tooenable, kan du konfigurera prenumerationer toofind meddelanden som har önskade egenskaper och utföra vissa ändringar toothose egenskaper. Du kan bara kopiera en delmängd av dessa meddelanden toohello virtuella prenumerationskön när Service Bus prenumerationer visas alla meddelanden som skickas toohello avsnittet. Detta åstadkoms med hjälp av prenumerationsfilter. Sådana ändringar kallas *filtrera åtgärder*. När en prenumeration har skapats kan du ange ett filteruttryck som körs på hello egenskaper för hello-meddelande, både hello Systemegenskaper (till exempel **etikett**) och anpassade egenskaper (till exempel **StoreName**.) hello SQL-uttryck är valfri i det här fallet; utan en SQL-filteruttrycket utförs något filter som definieras för en prenumeration på alla hälsningsmeddelande för den prenumerationen.

Om du använder hello föregående exempel toofilter meddelanden kommer endast från **Store1**, skulle du skapa hello instrumentpanelen prenumeration på följande sätt:

```csharp
namespaceManager.CreateSubscription("IssueTrackingTopic", "Dashboard", new SqlFilter("StoreName = 'Store1'"));
```

Med den här prenumerationsfiltret på plats, endast meddelanden som har hello `StoreName` egenskapsuppsättning för`Store1` är kopierade toohello virtuella kö för hello `Dashboard` prenumeration.

Mer information om möjliga filtervärden dokumentationen hello för hello [SqlFilter](/dotnet/api/microsoft.servicebus.messaging.sqlfilter) och [SqlRuleAction](/dotnet/api/microsoft.servicebus.messaging.sqlruleaction) klasser. Se även hello [asynkrona meddelanden: avancerade filter](http://code.msdn.microsoft.com/Brokered-Messaging-6b0d2749) och [avsnittet filter](https://github.com/Azure-Samples/azure-servicebus-messaging-samples/tree/master/TopicFilters) prover.

## <a name="next-steps"></a>Nästa steg
Se hello följande avancerade avsnitt för mer information och exempel på hur du använder Service Bus-meddelanden.

* [Översikt över Service Bus-meddelandetjänster](service-bus-messaging-overview.md)
* [Service Bus brokered messaging .NET tutorial](service-bus-brokered-tutorial-dotnet.md)
* [Service Bus självstudiekurs om asynkrona meddelanden REST](service-bus-brokered-tutorial-rest.md)
* [Ämnesfilter](https://github.com/Azure/azure-service-bus/tree/master/samples/DotNet/Microsoft.ServiceBus.Messaging/TopicFilters)
* [Asynkrona meddelanden: Avancerade filter-exempel](http://code.msdn.microsoft.com/Brokered-Messaging-6b0d2749)

