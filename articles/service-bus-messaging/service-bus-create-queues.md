---
title: "aaaWrite program som använder Azure Service Bus-köer | Microsoft Docs"
description: "Hur toowrite en enkel kö-baserade program som använder Azure Service Bus."
services: service-bus-messaging
documentationcenter: na
author: sethmanheim
manager: timlt
editor: 
ms.assetid: 754d91b3-1426-405e-84b4-fd36d65b114a
ms.service: service-bus-messaging
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/07/2017
ms.author: sethm
ms.openlocfilehash: 93b75902a06becd6e33e05298e09f5669d0e2aef
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-applications-that-use-service-bus-queues"></a>Skapa appar som använder Service Bus-köer
Det här avsnittet beskriver Service Bus-köer och visar hur toowrite en enkel kö-baserade program som använder Service Bus.

Anta från hello world varor där försäljningsdata från enskilda Point-of-Sale (POS) terminaler måste vara dirigeras tooan lagersystem som använder hello data toodetermine när börs har toobe fylls i. Den här lösningen använder Service Bus-meddelanden för hello kommunikation mellan hello terminaler och hello lagersystem, enligt beskrivningen i följande bild hello:

![Bild 1-Service Bus-köer](./media/service-bus-create-queues/IC657161.gif)

Varje POS terminal rapporterar försäljning data genom att skicka meddelanden toohello **DataCollectionQueue**. Dessa meddelanden ligger kvar i kön tills de har hämtats av hello lagersystem. Det här mönstret kallas ofta *asynkrona meddelanden*eftersom hello POS terminal saknar toowait på ett svar från hello inventering management system toocontinue bearbetning.

## <a name="why-queuing"></a>Varför queuing?
Överväg att hello fördelarna med att använda en kö i det här scenariot i stället för att hello POS terminaler prata direkt (synkront) toohello inventera hanteringssystemet innan vi tittar på hello-kod som är nödvändiga tooset in det här programmet.

### <a name="temporal-decoupling"></a>Tidsbestämd Frikoppling
Med hello mönstret för asynkrona meddelanden, producenter och konsumenter inte har toobe online på hello samtidigt. hello meddelandeinfrastrukturen lagrar på ett tillförlitligt sätt meddelanden tills hello konsumerande parten är redo tooreceive dem. Det innebär att hello komponenter i hello distribuerade program kan frikopplas, antingen frivilligt; till exempel för underhåll, eller på grund av tooa komponentkrasch, utan att påverka hello hela systemet. Dessutom hello förbrukning av programmet kan inte ha toobe online vid vissa tidpunkter på dagen hello. Till exempel i detta scenario retail hello lagersystem kan inte ha toocome online efter hello slut hello arbetsdag.

### <a name="load-leveling"></a>Belastningsutjämning
I många program varierar systembelastningen över tiden, bör hello bearbetningstid som krävs för varje arbetsenhet vanligtvis är konstant. Medlingen mellan meddelandet producenter och konsumenter med en kö innebär som hello användningsprogram (hello worker) har bara toobe etableras tooservice medelvärdet läsa in i stället för en belastning. hello kön hello djup växer och kontrakt som hello inkommande belastningen varierar. Detta sparar direkt pengar med beaktande toohello mängd infrastruktur krävs tooservice hello programinläsning.

![Service Bus-köer bild 2](./media/service-bus-create-queues/IC657162.gif)

### <a name="load-balancing"></a>Belastningsutjämning
Som hello belastningen ökar kan fler arbetsprocesser kan tillagda tooread från hello worker kö. Varje meddelande bearbetas av bara en av hello arbetsprocesser. Dessutom kan den här pull-baserade belastningsutjämning, för optimal användning av hello worker datorer även om hello worker datorer skiljer sig med beaktande tooprocessing power som de hämtar meddelanden med sina egna högsta hastighet. Det här mönstret kallas ofta hello konkurrerande konsument-mönster.

![Service Bus-köer bild 3](./media/service-bus-create-queues/IC657163.gif)

### <a name="loose-coupling"></a>Lösa kopplingar
Med hjälp av message queuing toointermediate mellan meddelandeproducenter och konsumenter innehåller en inbyggd lösa kopplingar mellan hello komponenter. Eftersom producenter och konsumenter inte är medvetna om varandra, kan en konsument uppgraderas utan någon inverkan på hello producenten. Dessutom kan hello messaging topologi utvecklas utan att påverka hello befintliga slutpunkter. Kommer vi att diskutera detta mer när vi pratar om Publicera/prenumerera.

## <a name="show-me-hello-code"></a>Visa hello kod
Hej följande avsnitt visar hur toouse Service Bus toobuild det här programmet.

### <a name="sign-up-for-an-azure-account"></a>Registrera dig för ett Azure-konto
Du behöver ett Azure-konto i ordning toostart arbeta med Service Bus. Om du inte redan har en, du kan registrera dig för ett kostnadsfritt konto [här](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A85619ABF).

### <a name="create-a-namespace"></a>Skapa ett namnområde
När du har en prenumeration kan du [skapa ett namnområde för tjänsten](service-bus-create-namespace-portal.md). Varje namnområde som fungerar som en omfattningsbehållare för en uppsättning Service Bus-entiteter. Ge din nya namnområdet ett unikt namn för alla Service Bus-konton. 

### <a name="install-hello-nuget-package"></a>Installera hello NuGet-paketet
toouse hello Service Bus-namnrymd, ett program måste referera till hello Service Bus-sammansättningen, särskilt Microsoft.ServiceBus.dll. Du hittar den här sammansättningen som en del av hello Microsoft Azure SDK och hello hämtningen är tillgänglig på hello [hämtningssidan för Azure SDK](https://azure.microsoft.com/downloads/). Dock hello [Service Bus NuGet-paketet](https://www.nuget.org/packages/WindowsAzure.ServiceBus) är hello enklaste sättet tooget hello Service Bus-API och tooconfigure ditt program med alla hello Service Bus-beroenden.

### <a name="create-hello-queue"></a>Skapa hello kö
Hanteringsåtgärder för Service Bus meddelandeentiteter (köer och publicera/prenumerera avsnitt) utförs via hello [NamespaceManager](/dotnet/api/microsoft.servicebus.namespacemanager) klass. Service Bus använder en [delade signatur åtkomst (SAS)](service-bus-sas.md) baserat säkerhetsmodell. Hej [TokenProvider](/dotnet/api/microsoft.servicebus.tokenprovider) klassen representerar en leverantör av säkerhetstoken med inbyggda fabriksmetoder som returnerar vissa välkända tokenleverantörer. Vi använder en [CreateSharedAccessSignatureTokenProvider](/dotnet/api/microsoft.servicebus.tokenprovider#Microsoft_ServiceBus_TokenProvider_CreateSharedAccessSignatureTokenProvider_System_String_) metoden toohold hello SAS-autentiseringsuppgifter. Hej [NamespaceManager](/dotnet/api/microsoft.servicebus.namespacemanager) instans sedan konstruerade med hello basadressen för hello Service Bus-namnrymd och hello tokenleverantör.

Hej [NamespaceManager](/dotnet/api/microsoft.servicebus.namespacemanager) klassen innehåller metoder toocreate, räkna upp och ta bort meddelandeentiteter. Hej koden som visas här visas hur hello [NamespaceManager](/dotnet/api/microsoft.servicebus.namespacemanager) -instansen är skapas och används toocreate hello **DataCollectionQueue** kön.

```csharp
Uri uri = ServiceBusEnvironment.CreateServiceUri("sb", 
                "test-blog", string.Empty);
string name = "RootManageSharedAccessKey";
string key = "abcdefghijklmopqrstuvwxyz";

TokenProvider tokenProvider = 
    TokenProvider.CreateSharedAccessSignatureTokenProvider(name, key);
NamespaceManager namespaceManager = 
    new NamespaceManager(uri, tokenProvider);
namespaceManager.CreateQueue("DataCollectionQueue");
```

Observera att det finns överlagringar av hello [CreateQueue](/dotnet/api/microsoft.servicebus.namespacemanager#Microsoft_ServiceBus_NamespaceManager_CreateQueue_System_String_) metoden som gör att egenskaper för hello kön toobe justerade. Du kan till exempel ange hello time to live (TTL) Standardvärdet för meddelanden som skickas toohello kön.

### <a name="send-messages-toohello-queue"></a>Skicka meddelanden toohello kön
För körning åtgärder på Service Bus-enheter. till exempel skicka och ta emot meddelanden, ett program måste först skapa en [MessagingFactory](/dotnet/api/microsoft.servicebus.messaging.messagingfactory) objekt. Liknande toohello [NamespaceManager](/dotnet/api/microsoft.servicebus.namespacemanager) class, hello [MessagingFactory](/dotnet/api/microsoft.servicebus.messaging.messagingfactory) instans skapas från hello basadress av hello tjänstens namnområde och hello tokenleverantör.

```csharp
 BrokeredMessage bm = new BrokeredMessage(salesData);
 bm.Label = "SalesReport";
 bm.Properties["StoreName"] = "Redmond";
 bm.Properties["MachineID"] = "POS_1";
```

Meddelanden som skickas till och tas emot från Service Bus-köer är instanser av hello [BrokeredMessage](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage) klass. Den här klassen består av en uppsättning standardegenskaper (t.ex [etikett](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage#Microsoft_ServiceBus_Messaging_BrokeredMessage_Label) och [TimeToLive](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage#Microsoft_ServiceBus_Messaging_BrokeredMessage_TimeToLive)), en ordlista som används toohold programegenskaper och en brödtext med godtyckliga programdata. Ett program kan konfigurera hello brödtext genom att passera i någon typ av serialiserbara objekt (hello följande exempel skickas i en **SalesData** objekt som representerar hello försäljningsdata från hello POS terminal), som använder hello [ DataContractSerializer](https://msdn.microsoft.com/library/system.runtime.serialization.datacontractserializer.aspx) tooserialize hello-objekt. Du kan också en [dataströmmen](https://msdn.microsoft.com/library/system.io.stream.aspx) objekt kan anges.

Hej toosend meddelanden av enklaste sättet-tooa som ges kö, i vårt fall hello **DataCollectionQueue**, är toouse [CreateMessageSender](/dotnet/api/microsoft.servicebus.messaging.messagingfactory#Microsoft_ServiceBus_Messaging_MessagingFactory_CreateMessageSender_System_String_) toocreate en [MessageSender](/dotnet/api/microsoft.servicebus.messaging.messagesender) objekt direkt från hello [MessagingFactory](/dotnet/api/microsoft.servicebus.messaging.messagingfactory) instans.

```csharp
MessageSender sender = factory.CreateMessageSender("DataCollectionQueue");
sender.Send(bm);
```

### <a name="receiving-messages-from-hello-queue"></a>Ta emot meddelanden från kön hello
tooreceive meddelanden från hello kön, som du kan använda en [MessageReceiver](/dotnet/api/microsoft.servicebus.messaging.messagereceiver) objekt som du skapar direkt från hello [MessagingFactory](/dotnet/api/microsoft.servicebus.messaging.messagingfactory) med [CreateMessageReceiver](/dotnet/api/microsoft.servicebus.messaging.messagingfactory#Microsoft_ServiceBus_Messaging_MessagingFactory_CreateMessageReceiver_System_String_). Meddelandet mottagare kan arbeta i två olika lägen: **ReceiveAndDelete** och **PeekLock**. Hej [ReceiveMode](/dotnet/api/microsoft.servicebus.messaging.receivemode) anges när hello meddelandet mottagaren har skapats som en parameter toohello [CreateMessageReceiver](/dotnet/api/microsoft.servicebus.messaging.messagingfactory?redirectedfrom=MSDN#Microsoft_ServiceBus_Messaging_MessagingFactory_CreateMessageReceiver_System_String_Microsoft_ServiceBus_Messaging_ReceiveMode_) anropa.

När du använder hello **ReceiveAndDelete** läge hello får är en engångsåtgärd, det vill säga när Service Bus tar emot hello begäran, den markerar hello meddelandet som Förbrukat och returnerar den toohello program. **ReceiveAndDelete** läge är hello enklaste modellen och fungerar bäst för scenarier där hello program kan tolerera icke-bearbetning av ett meddelande om ett fel toooccur. toounderstand, Föreställ i vilka hello konsumenten problem hello mottagning av begäran och sedan kraschar innan bearbetningen. Eftersom Service Bus markerad hello meddelandet som Förbrukat, när hello programmet startas om och börjar förbruka meddelanden igen, att ha missat hello meddelandet som förbrukades innan kraschen hello.

I **PeekLock** läge hello får blir en åtgärd i två steg, vilket gör det möjligt toosupport program som inte tolererar att ett meddelande saknas. När Service Bus tar emot hello begäran hittar hello nästa meddelande toobe förbrukas, låser det tooprevent andra användare tar emot det och returnerar sedan toohello program. När hello programmet har avslutat bearbetningen hello-meddelande (eller lagrar det på ett tillförlitligt sätt för framtida bearbetning), Slutför hello andra steget i hello får processen genom att anropa [Slutför](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage#Microsoft_ServiceBus_Messaging_BrokeredMessage_Complete) på emot hello-meddelande. När Service Bus ser hello [Slutför](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage#Microsoft_ServiceBus_Messaging_BrokeredMessage_Complete) anrop, markeras hello-meddelande som används.

Två resultat är möjliga. Först om programmet hello inte tooprocess Hej meddelande av någon anledning, kan det anropa [Avbryt](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage#Microsoft_ServiceBus_Messaging_BrokeredMessage_Abandon) för hello tog emot meddelande (i stället för [Slutför](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage#Microsoft_ServiceBus_Messaging_BrokeredMessage_Complete)). Detta gör att Service Bus toounlock hello-meddelande och göra den tillgänglig toobe tas emot igen, antingen genom att hello samma konsumenten eller av en annan Slutför konsument. Andra: det finns en tidsgräns som är associerade med hello Lås och om hello programmet inte kan bearbeta hello-meddelande innan hello tidsgränsen för låsning går ut (till exempel om hello programmet kraschar), kommer Service Bus att låsa upp hello meddelandet och gör den tillgänglig toobe tas emot igen (i praktiken utför en [Avbryt](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage#Microsoft_ServiceBus_Messaging_BrokeredMessage_Abandon) åtgärden som standard).

Observera att om hello programmet kraschar efter att den bearbetar hello-meddelande, men före hello [Slutför](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage#Microsoft_ServiceBus_Messaging_BrokeredMessage_Complete) begäran gjordes blir hello-meddelande levereras på nytt toohello program när den startas om. Detta kallas ofta * minst en gång * bearbetning. Detta innebär att varje meddelande bearbetas minst en gång men i vissa situationer hello samma meddelande levereras. Om hello scenariot inte tolererar duplicerad bearbetning, krävs ytterligare logik i hello programmet toodetect dubbletter. Detta kan uppnås baserat på hello [MessageId](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage#Microsoft_ServiceBus_Messaging_BrokeredMessage_MessageId) -egenskapen för hello-meddelande. hello värdet på egenskapen förblir konstant under alla leveransförsök. Detta kallas *exakt en gång* bearbetning.

hello koden som visas här tar emot och bearbetar ett meddelande med hello **PeekLock** läge, som är hello standard om inget [ReceiveMode](/dotnet/api/microsoft.servicebus.messaging.receivemode) värde anges explicit.

```csharp
MessageReceiver receiver = factory.CreateMessageReceiver("DataCollectionQueue");
BrokeredMessage receivedMessage = receiver.Receive();
try
{
    ProcessMessage(receivedMessage);
    receivedMessage.Complete();
}
catch (Exception e)
{
    receivedMessage.Abandon();
}
```

### <a name="use-hello-queue-client"></a>Använda hello kö-klient
Hej exempel tidigare i det här avsnittet skapas [MessageSender](/dotnet/api/microsoft.servicebus.messaging.messagesender) och [MessageReceiver](/dotnet/api/microsoft.servicebus.messaging.messagereceiver) objekt direkt från hello [MessagingFactory](/dotnet/api/microsoft.servicebus.messaging.messagingfactory) toosend och ta emot meddelanden från hello kö respektive. En annan metod är toouse en [QueueClient](/dotnet/api/microsoft.servicebus.messaging.queueclient) objekt, som stöder både skicka och ta emot åtgärder i tillägg toomore avancerade funktioner, till exempel sessioner.

```csharp
QueueClient queueClient = factory.CreateQueueClient("DataCollectionQueue");
queueClient.Send(bm);

BrokeredMessage message = queueClient.Receive();

try
{
    ProcessMessage(message);
    message.Complete();
}
catch (Exception e)
{
    message.Abandon();
} 
```

## <a name="next-steps"></a>Nästa steg
Nu när du har lärt dig grunderna hello köer finns [skapa program som använder Service Bus-ämnen och prenumerationer](service-bus-create-topics-subscriptions.md) toocontinue diskussionen med hello Publicera/prenumerera funktionerna i Service Bus-ämnen och prenumerationer.

