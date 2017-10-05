---
title: "Skriva program som använder Azure Service Bus-köer | Microsoft Docs"
description: "Hur du skriver en enkel kö-baserade program som använder Azure Service Bus."
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
ms.openlocfilehash: 419caff7e8ceeb419c89a2ef9a6614c1accf3e52
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/18/2017
---
# <a name="create-applications-that-use-service-bus-queues"></a>Skapa appar som använder Service Bus-köer
Det här avsnittet beskriver Service Bus-köer och visar hur du skriver en enkel kö-baserade program som använder Service Bus.

Anta från hela världen för retail försäljningsdata från enskilda Point-of-Sale () Kassaterminaler som måste vidarebefordrar till ett lagersystem som använder data för att avgöra när börs måste fyllas. Den här lösningen använder Service Bus-meddelanden för kommunikation mellan terminalerna och lagersystem, enligt beskrivningen i följande bild:

![Bild 1-Service Bus-köer](./media/service-bus-create-queues/IC657161.gif)

Varje POS terminal rapporterar försäljning data genom att skicka meddelanden till den **DataCollectionQueue**. Dessa meddelanden ligger kvar i kön tills de har hämtats av hanteringssystem för inventering. Det här mönstret kallas ofta *asynkrona meddelanden*eftersom POS-terminal inte behöver vänta på svar från lagersystem fortsätta bearbetningen.

## <a name="why-queuing"></a>Varför queuing?
Innan vi titta på koden som krävs för att ställa in det här programmet, Överväg fördelarna med att använda en kö i det här scenariot i stället för att kassaterminalerna prata direkt (synkront) till hanteringssystemet inventering.

### <a name="temporal-decoupling"></a>Tidsbestämd Frikoppling
Med mönstret för asynkrona meddelanden, behöver producenter och konsumenter inte vara online samtidigt. Meddelandeinfrastrukturen lagrar meddelanden på ett tillförlitligt sätt tills konsumentparten är redo att ta emot dem. Detta innebär att komponenterna i den distribuerade appen kan frikopplas, antingen frivilligt; till exempel för underhåll, eller på grund av en komponentkrasch, utan påverkar hela systemet. Den konsumerande appen kan bara innehålla vara online under vissa tider på dagen. Till exempel i detta scenario retail kanske hanteringssystemet inventering endast efter slutet av arbetsdagen.

### <a name="load-leveling"></a>Belastningsutjämning
I många program varierar systembelastningen över tiden, medan den bearbetningstid som krävs för varje arbetsenhet är vanligtvis är konstant. Medlingen mellan meddelandeproducenter och konsumenter med hjälp av en kö innebär att den konsumerande appen (arbetaren) endast måste etableras för att underhålla en genomsnittlig belastning i stället för en belastning. Köns djup växer och kontrakt som den inkommande belastningen varierar. Detta sparar direkt pengar avseende mängden infrastruktur som krävs för att underhålla.

![Service Bus-köer bild 2](./media/service-bus-create-queues/IC657162.gif)

### <a name="load-balancing"></a>Belastningsutjämning
När belastningen ökar kan fler arbetsprocesser läggas för att läsa från kön worker. Varje meddelande bearbetas bara av en av arbetsprocesserna. Dessutom kan den här pull-baserade belastningsutjämning, för optimal användning av worker-datorer även om worker datorer skiljer sig åt vad gäller processorkraft, som de hämtar meddelanden med sina egna högsta hastighet. Det här mönstret kallas ofta konkurrerande konsument mönstret.

![Service Bus-köer bild 3](./media/service-bus-create-queues/IC657163.gif)

### <a name="loose-coupling"></a>Lösa kopplingar
Använda message queuing till mellanliggande mellan meddelandeproducenter och konsumenter innehåller en inbyggd lösa kopplingar mellan komponenterna. Eftersom producenter och konsumenter inte är medvetna om varandra, kan en konsument uppgraderas utan någon inverkan på producenten. Dessutom kan meddelanden topologin utvecklas utan att påverka befintliga slutpunkter. Kommer vi att diskutera detta mer när vi pratar om Publicera/prenumerera.

## <a name="show-me-the-code"></a>Visa koden
I följande avsnitt beskrivs hur du använder Service Bus för att skapa det här programmet.

### <a name="sign-up-for-an-azure-account"></a>Registrera dig för ett Azure-konto
Du behöver ett Azure-konto för att börja arbeta med Service Bus. Om du inte redan har en, du kan registrera dig för ett kostnadsfritt konto [här](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A85619ABF).

### <a name="create-a-namespace"></a>Skapa ett namnområde
När du har en prenumeration kan du [skapa ett namnområde för tjänsten](service-bus-create-namespace-portal.md). Varje namnområde som fungerar som en omfattningsbehållare för en uppsättning Service Bus-entiteter. Ge din nya namnområdet ett unikt namn för alla Service Bus-konton. 

### <a name="install-the-nuget-package"></a>Installera NuGet-paketet
Om du vill använda Service Bus-namnrymd, måste ett program referera till Service Bus-sammansättningen specifikt Microsoft.ServiceBus.dll. Du hittar den här sammansättningen som en del av Microsoft Azure SDK och hämta är tillgänglig på den [hämtningssidan för Azure SDK](https://azure.microsoft.com/downloads/). Men den [Service Bus NuGet-paketet](https://www.nuget.org/packages/WindowsAzure.ServiceBus) är det enklaste sättet att hämta Service Bus-API och att konfigurera ditt program med alla Service Bus-beroenden.

### <a name="create-the-queue"></a>Skapa kön
Hanteringsåtgärder för Service Bus meddelandeentiteter (köer och publicera/prenumerera avsnitt) utförs via den [NamespaceManager](/dotnet/api/microsoft.servicebus.namespacemanager) klass. Service Bus använder en [delade signatur åtkomst (SAS)](service-bus-sas.md) baserat säkerhetsmodell. Den [TokenProvider](/dotnet/api/microsoft.servicebus.tokenprovider) klassen representerar en leverantör av säkerhetstoken med inbyggda fabriksmetoder som returnerar vissa välkända tokenleverantörer. Vi använder en [CreateSharedAccessSignatureTokenProvider](/dotnet/api/microsoft.servicebus.tokenprovider#Microsoft_ServiceBus_TokenProvider_CreateSharedAccessSignatureTokenProvider_System_String_) metod för att rymma SAS-autentiseringsuppgifter. Den [NamespaceManager](/dotnet/api/microsoft.servicebus.namespacemanager) instans sedan konstruerade med basadressen för namnområdet för Service Bus och tokenleverantör.

Den [NamespaceManager](/dotnet/api/microsoft.servicebus.namespacemanager) klassen innehåller metoder för att skapa, räkna upp och ta bort meddelandeentiteter. Koden som visas här visas hur [NamespaceManager](/dotnet/api/microsoft.servicebus.namespacemanager) instans skapas och används för att skapa den **DataCollectionQueue** kön.

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

Observera att det finns overloads av den [CreateQueue](/dotnet/api/microsoft.servicebus.namespacemanager#Microsoft_ServiceBus_NamespaceManager_CreateQueue_System_String_) metoden som gör att egenskaper för kön till finjusteras. Du kan till exempel ange time to live (TTL) Standardvärdet för meddelanden som skickas till kön.

### <a name="send-messages-to-the-queue"></a>Skicka meddelanden till kön
För körning åtgärder på Service Bus-enheter. till exempel skicka och ta emot meddelanden, ett program måste först skapa en [MessagingFactory](/dotnet/api/microsoft.servicebus.messaging.messagingfactory) objekt. Liknar den [NamespaceManager](/dotnet/api/microsoft.servicebus.namespacemanager) klassen, de [MessagingFactory](/dotnet/api/microsoft.servicebus.messaging.messagingfactory) instans skapas från basadressen för namnområdet för tjänsten och tokenleverantör.

```csharp
 BrokeredMessage bm = new BrokeredMessage(salesData);
 bm.Label = "SalesReport";
 bm.Properties["StoreName"] = "Redmond";
 bm.Properties["MachineID"] = "POS_1";
```

Meddelanden som skickas till och tas emot från Service Bus-köer är instanser av den [BrokeredMessage](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage) klass. Den här klassen består av en uppsättning standardegenskaper (t.ex [etikett](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage#Microsoft_ServiceBus_Messaging_BrokeredMessage_Label) och [TimeToLive](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage#Microsoft_ServiceBus_Messaging_BrokeredMessage_TimeToLive)), en ordlista som används för att lagra egenskaper för programmet och en brödtext med godtyckliga programdata. Ett program kan konfigurera meddelandets brödtext genom att passera i någon typ av serialiserbara objekt (i följande exempel skickas i en **SalesData** objekt som representerar försäljningsdata från terminalen POS), som använder den [DataContractSerializer](https://msdn.microsoft.com/library/system.runtime.serialization.datacontractserializer.aspx) att serialisera objektet. Du kan också en [dataströmmen](https://msdn.microsoft.com/library/system.io.stream.aspx) objekt kan anges.

Det enklaste sättet att skicka meddelanden till en viss kö, i vårt fall den **DataCollectionQueue**, är att använda [CreateMessageSender](/dotnet/api/microsoft.servicebus.messaging.messagingfactory#Microsoft_ServiceBus_Messaging_MessagingFactory_CreateMessageSender_System_String_) att skapa en [MessageSender](/dotnet/api/microsoft.servicebus.messaging.messagesender) objektet direkt från den [MessagingFactory](/dotnet/api/microsoft.servicebus.messaging.messagingfactory) instans.

```csharp
MessageSender sender = factory.CreateMessageSender("DataCollectionQueue");
sender.Send(bm);
```

### <a name="receiving-messages-from-the-queue"></a>Ta emot meddelanden från kön
Om du vill ta emot meddelanden från kön, kan du använda en [MessageReceiver](/dotnet/api/microsoft.servicebus.messaging.messagereceiver) objekt som du skapar direkt från den [MessagingFactory](/dotnet/api/microsoft.servicebus.messaging.messagingfactory) med [CreateMessageReceiver](/dotnet/api/microsoft.servicebus.messaging.messagingfactory#Microsoft_ServiceBus_Messaging_MessagingFactory_CreateMessageReceiver_System_String_). Meddelandet mottagare kan arbeta i två olika lägen: **ReceiveAndDelete** och **PeekLock**. Den [ReceiveMode](/dotnet/api/microsoft.servicebus.messaging.receivemode) anges när meddelandet mottagaren har skapats som en parameter för den [CreateMessageReceiver](/dotnet/api/microsoft.servicebus.messaging.messagingfactory?redirectedfrom=MSDN#Microsoft_ServiceBus_Messaging_MessagingFactory_CreateMessageReceiver_System_String_Microsoft_ServiceBus_Messaging_ReceiveMode_) anropa.

När du använder den **ReceiveAndDelete** läge Receive-aktiviteten är en engångsåtgärd, det vill säga när Service Bus tar emot begäran, den markerar meddelandet som Förbrukat och skickar tillbaka det till programmet. **ReceiveAndDelete** läge är den enklaste modellen och fungerar bäst för scenarier där programmet kan tolerera icke-bearbetning av ett meddelande om ett fel skulle inträffa. För att förstå detta kan du föreställa dig ett scenario där konsumenten utfärdar en receive-begäran och sedan kraschar innan den kan bearbeta denna begäran. Eftersom Service Bus markerat meddelandet som Förbrukat när programmet startas om och börjar förbruka meddelanden igen, att ha missat meddelandet som förbrukades innan kraschen.

I **PeekLock** läge inleveransen en åtgärd i två steg, vilket gör det möjligt att stödprogram som inte tolererar att ett meddelande saknas. När Service Bus tar emot begäran, den söker efter nästa meddelande som ska förbrukas, låser det för att förhindra andra användare tar emot det och skickar sedan tillbaka det till programmet. När programmet har avslutat bearbetningen av meddelandet (eller lagrar det på ett tillförlitligt sätt för framtida bearbetning), slutför programmet det andra steget i processen genom att anropa [Slutför](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage#Microsoft_ServiceBus_Messaging_BrokeredMessage_Complete) för det mottagna meddelandet. När Service Bus ser den [Slutför](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage#Microsoft_ServiceBus_Messaging_BrokeredMessage_Complete) anrop, markerar den meddelandet som Förbrukat.

Två resultat är möjliga. Först om programmet inte kan bearbeta meddelandet av någon anledning, kan det anropa [Avbryt](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage#Microsoft_ServiceBus_Messaging_BrokeredMessage_Abandon) för det mottagna meddelandet (i stället för [Slutför](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage#Microsoft_ServiceBus_Messaging_BrokeredMessage_Complete)). Detta gör att Service Bus att låsa upp meddelandet och gör det tillgängligt att tas emot igen, antingen genom samma konsumenten eller av en annan Slutför konsument. Det finns en tidsgräns som är kopplade till låset och om programmet går inte att bearbeta meddelandet innan tidsgränsen för låsning går ut (till exempel om programmet kraschar), kommer Service Bus att låsa upp meddelandet och gör det tillgängligt och kan tas emot igen (andra: i praktiken utför en [Avbryt](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage#Microsoft_ServiceBus_Messaging_BrokeredMessage_Abandon) åtgärden som standard).

Observera att om programmet kraschar efter den bearbetar meddelandet men innan det [Slutför](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage#Microsoft_ServiceBus_Messaging_BrokeredMessage_Complete) begäran gjordes, meddelandet att levereras till programmet när den startas om. Detta kallas ofta * minst en gång * bearbetning. Detta innebär att varje meddelande bearbetas minst en gång men i vissa situationer kan samma meddelande levereras. Om scenariot inte tolererar duplicerad bearbetning, krävs ytterligare logik i programmet för att identifiera dubbletter. Detta kan uppnås baserat på de [MessageId](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage#Microsoft_ServiceBus_Messaging_BrokeredMessage_MessageId) för meddelandet. Värdet för den här egenskapen förblir konstant under alla leveransförsök. Detta kallas *exakt en gång* bearbetning.

Koden som visas här tar emot och bearbetar ett meddelande med hjälp av den **PeekLock** läge, som är standard om inget [ReceiveMode](/dotnet/api/microsoft.servicebus.messaging.receivemode) värde anges explicit.

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

### <a name="use-the-queue-client"></a>Använd klienten för kön
Exemplen tidigare i det här avsnittet skapas [MessageSender](/dotnet/api/microsoft.servicebus.messaging.messagesender) och [MessageReceiver](/dotnet/api/microsoft.servicebus.messaging.messagereceiver) objekt direkt från den [MessagingFactory](/dotnet/api/microsoft.servicebus.messaging.messagingfactory) skicka och ta emot meddelanden från den kö, respektive. En annan metod är att använda en [QueueClient](/dotnet/api/microsoft.servicebus.messaging.queueclient) objektet, vilket stöder både skicka och ta emot operations förutom mer avancerade funktioner, till exempel sessioner.

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
Nu när du har lärt dig grunderna om köer finns [skapa program som använder Service Bus-ämnen och prenumerationer](service-bus-create-topics-subscriptions.md) fortsätta den här diskussionen med Publicera/prenumerera-funktionerna i Service Bus-ämnen och prenumerationer.

