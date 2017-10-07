---
title: "aaaCreate program som använder Azure Service Bus-ämnen och prenumerationer | Microsoft Docs"
description: "Introduktion toohello publicera och prenumerera funktionerna som erbjuds av Service Bus-ämnen och prenumerationer."
services: service-bus-messaging
documentationcenter: na
author: sethmanheim
manager: timlt
editor: 
ms.assetid: a48fc9b0-b7b0-464e-8187-a517bf8b4eb4
ms.service: service-bus-messaging
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 04/07/2017
ms.author: sethm
ms.openlocfilehash: f6d7de46ace7bd5b49de612db213ced789308d91
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-applications-that-use-service-bus-topics-and-subscriptions"></a>Skapa program som använder Service Bus-ämnen och prenumerationer
Azure Service Bus stöder en uppsättning molnbaserade, meddelande indatavärdena mellanprogram teknologier, inklusive tillförlitliga message Queuing- och varaktig Publicera/prenumerera på meddelanden. Den här artikeln bygger på informationen i hello [skapa program som använder Service Bus-köer](service-bus-create-queues.md) och ger en introduktion toohello Publicera/prenumerera funktionerna i Service Bus-ämnen.

## <a name="evolving-retail-scenario"></a>Utvecklingen av retail-scenario
Den här artikeln fortsätter hello retail-scenariot som används i [skapa program som använder Service Bus-köer](service-bus-create-queues.md). Kom ihåg att försäljningsdata från enskilda punkt för försäljning () Kassaterminaler måste vara routade tooan lagersystem som använder den data toodetermine när börs har toobe fylls i. Varje POS terminal rapporterar försäljning data genom att skicka meddelanden toohello **DataCollectionQueue** kö, där de finns kvar tills de tas emot av hello lagersystem som visas här:

![Service Bus 1](./media/service-bus-create-topics-subscriptions/IC657161.gif)

tooevolve det här scenariot, ett nytt krav har lagts till toohello system: hello store ägare vill toobe kan toomonitor hur hello store utförs i realtid.

tooaddress det här kravet hello system måste ”tryck på” av hello försäljning dataström. Vi vill varje meddelande som skickas av hello POS terminaler toobe skickas toohello lagersystem som innan fortfarande, men vi vill att en annan kopia av varje meddelande som vi kan använda toopresent hello instrumentpanelens visa toohello store ägare.

Du kan använda Service Bus i en situation som den här, där du behöver varje meddelande toobe som används av flera parter *avsnitt*. Innehåller ett Publicera/prenumerera-mönster där varje publicerat meddelande görs tillgängligt tooone eller flera prenumerationer har registrerats med hello-avsnittet. Köer är däremot varje meddelande tas emot av en enskild konsument.

Meddelanden skickas tooa avsnittet i hello samma sätt som de skickas tooa kön. Dock tas meddelanden inte emot hello artikeln direkt; de tas emot från prenumerationer. Du kan se i prenumerationen tooa avsnittet som en virtuell kö som tar emot kopior av hello-meddelanden som skickas toothat avsnittet. Meddelanden tas emot från en prenumeration hello samma sätt som de tas emot från en kö.

Gå tillbaka toohello retail scenario, hello kön ersätts av ett ämne och en prenumeration har lagts till vilken hello inventering management systemkomponent kan använda. hello system visas nu på följande sätt:

![Service Bus 2](./media/service-bus-create-topics-subscriptions/IC657165.gif)

hello konfigurationen här utför identiskt toohello tidigare kö-baserade designen. Det vill säga meddelanden som skickas toohello avsnittet är routade toohello **inventering** prenumeration från vilka hello **lagersystem** förbrukar dem.

I ordning toosupport hello management dashboard skapar vi en andra prenumeration på hello avsnittet som visas här:

![Service Bus 3](./media/service-bus-create-topics-subscriptions/IC657166.gif)

Med den här konfigurationen varje meddelande från hello POS terminaler görs tillgängliga tooboth hello **instrumentpanelen** och **inventering** prenumerationer.

## <a name="show-me-hello-code"></a>Visa hello kod
hello artikel [skapa program som använder Service Bus-köer](service-bus-create-queues.md) beskriver hur toosign dig för ett Azure-konto och skapa ett namnområde för tjänsten. hello enklaste sättet tooreference Service Bus-beroenden är tooinstall hello Service Bus [Nuget-paketet](https://www.nuget.org/packages/WindowsAzure.ServiceBus/). Du kan också hitta hello Service Bus-bibliotek som en del av hello Azure SDK. hello hämtningen är tillgänglig på hello [hämtningssidan för Azure SDK](https://azure.microsoft.com/downloads/).

### <a name="create-hello-topic-and-subscriptions"></a>Skapa hello avsnittet och prenumerationer
Hanteringsåtgärder för Service Bus meddelandeentiteter (köer och publicera/prenumerera avsnitt) utförs via hello [NamespaceManager](/dotnet/api/microsoft.servicebus.namespacemanager#microsoft_servicebus_namespacemanager) klass. Rätt autentiseringsuppgifter som krävs i ordning toocreate en **NamespaceManager** -instans för ett visst namnområde. Service Bus använder en [delade signatur åtkomst (SAS)](service-bus-sas.md) baserat säkerhetsmodell. Hej [TokenProvider](/dotnet/api/microsoft.servicebus.tokenprovider#microsoft_servicebus_tokenprovider) klassen representerar en leverantör av säkerhetstoken med inbyggda fabriksmetoder som returnerar vissa välkända tokenleverantörer. Vi använder en [CreateSharedAccessSignatureTokenProvider](/dotnet/api/microsoft.servicebus.tokenprovider#Microsoft_ServiceBus_TokenProvider_CreateSharedAccessSignatureTokenProvider_System_String_) metoden toohold hello SAS-autentiseringsuppgifter. Hej [NamespaceManager](/dotnet/api/microsoft.servicebus.namespacemanager#microsoft_servicebus_namespacemanager) instans sedan konstruerade med hello basadressen för hello Service Bus-namnrymd och hello tokenleverantör.

Hej [NamespaceManager](/dotnet/api/microsoft.servicebus.namespacemanager#microsoft_servicebus_namespacemanager) klassen innehåller metoder toocreate, räkna upp och ta bort meddelandeentiteter. Hej koden som visas här visas hur hello **NamespaceManager** -instansen är skapas och används toocreate hello **DataCollectionTopic** avsnittet.

```csharp
Uri uri = ServiceBusEnvironment.CreateServiceUri("sb", "test-blog", string.Empty);
string name = "RootManageSharedAccessKey";
string key = "abcdefghijklmopqrstuvwxyz";

TokenProvider tokenProvider = TokenProvider.CreateSharedAccessSignatureTokenProvider(name, key);
NamespaceManager namespaceManager = new NamespaceManager(uri, tokenProvider);

namespaceManager.CreateTopic("DataCollectionTopic");
```

Observera att det finns överlagringar av hello [CreateTopic](/dotnet/api/microsoft.servicebus.namespacemanager#Microsoft_ServiceBus_NamespaceManager_CreateTopic_System_String_) metoden som gör att du tooset egenskaper för hello-avsnittet. Du kan till exempel ange hello time to live (TTL) Standardvärdet för meddelanden som skickas toohello avsnittet. Lägg till hello **inventering** och **instrumentpanelen** prenumerationer.

```csharp
namespaceManager.CreateSubscription("DataCollectionTopic", "Inventory");
namespaceManager.CreateSubscription("DataCollectionTopic", "Dashboard");
```

### <a name="send-messages-toohello-topic"></a>Skicka meddelanden toohello avsnittet
För körning åtgärder på Service Bus-enheter. till exempel skicka och ta emot meddelanden, ett program måste först skapa en [MessagingFactory](/dotnet/api/microsoft.servicebus.messaging.messagingfactory#microsoft_servicebus_messaging_messagingfactory) objekt. Liknande toohello [NamespaceManager](/dotnet/api/microsoft.servicebus.namespacemanager#microsoft_servicebus_namespacemanager) class, hello **MessagingFactory** instans skapas från hello basadress av hello tjänstens namnområde och hello tokenleverantör.

```
MessagingFactory factory = MessagingFactory.Create(uri, tokenProvider);
```

Meddelanden som skickas tooand togs emot från Service Bus-ämnen är instanser av hello [BrokeredMessage](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage) klass. Den här klassen består av en uppsättning standardegenskaper (t.ex [etikett](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage.label?view=azureservicebus-4.0.0#Microsoft_ServiceBus_Messaging_BrokeredMessage_Label) och [TimeToLive](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage.timetolive?view=azureservicebus-4.0.0#Microsoft_ServiceBus_Messaging_BrokeredMessage_TimeToLive)), en ordlista som används toohold programegenskaper och en brödtext med godtyckliga programdata. Ett program kan konfigurera hello brödtext genom att passera i någon typ av serialiserbara objekt (hello följande exempel skickas i en **SalesData** objekt som representerar hello försäljningsdata från hello POS terminal), som använder hello [ DataContractSerializer](https://msdn.microsoft.com/library/system.runtime.serialization.datacontractserializer.aspx) tooserialize hello-objekt. Du kan också en [dataströmmen](https://msdn.microsoft.com/library/system.io.stream.aspx) objekt kan anges.

```csharp
BrokeredMessage bm = new BrokeredMessage(salesData);
bm.Label = "SalesReport";
bm.Properties["StoreName"] = "Redmond";
bm.Properties["MachineID"] = "POS_1";
```

hello enklaste sättet toosend meddelanden toohello avsnittet är toouse [CreateMessageSender](/dotnet/api/microsoft.servicebus.messaging.messagingfactory#Microsoft_ServiceBus_Messaging_MessagingFactory_CreateMessageSender_System_String_) toocreate en [MessageSender](/dotnet/api/microsoft.servicebus.messaging.messagesender) objekt direkt från hello [MessagingFactory](/dotnet/api/microsoft.servicebus.messaging.messagingfactory) instans:

```csharp
MessageSender sender = factory.CreateMessageSender("DataCollectionTopic");
sender.Send(bm);
```

### <a name="receive-messages-from-a-subscription"></a>Ta emot meddelanden från en prenumeration
Liknande toousing köer, tooreceive meddelanden från en prenumeration som du kan använda en [MessageReceiver](/dotnet/api/microsoft.servicebus.messaging.messagereceiver) objekt som du skapar direkt från hello [MessagingFactory](/dotnet/api/microsoft.servicebus.messaging.messagingfactory) med [ CreateMessageReceiver](/dotnet/api/microsoft.servicebus.messaging.messagingfactory#Microsoft_ServiceBus_Messaging_MessagingFactory_CreateMessageReceiver_System_String_). Du kan använda någon av hello visas två olika lägen (**ReceiveAndDelete** och **PeekLock**), enligt beskrivningen i [skapa program som använder Service Bus-köer](service-bus-create-queues.md).

Observera att när du skapar en **MessageReceiver** prenumerationer, hello *entityPath* parametern är av hello formuläret `topicPath/subscriptions/subscriptionName`. Därför toocreate en **MessageReceiver** för hello **inventering** hello-prenumeration **DataCollectionTopic** avsnittet *entityPath*måste anges för`DataCollectionTopic/subscriptions/Inventory`. hello kod visas på följande sätt:

```csharp
MessageReceiver receiver = factory.CreateMessageReceiver("DataCollectionTopic/subscriptions/Inventory");
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

## <a name="subscription-filters"></a>Prenumerationsfilter
Hittills i det här scenariot görs alla meddelanden som skickas toohello avsnittet tillgängliga tooall registrerade prenumerationer. hello här nyckel frasen görs ”tillgänglig”. När Service Bus prenumerationer visas alla meddelanden som skickas toohello avsnittet kan du kopiera en delmängd av dessa meddelanden toohello virtuella prenumerationskön. Detta görs med hjälp av prenumeration *filter*. När du skapar en prenumeration kan du ange ett filteruttryck i hello form av ett SQL92 style-predikat som fungerar via hello egenskaper för hello-meddelande, både hello Systemegenskaper (till exempel [etikett](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage#Microsoft_ServiceBus_Messaging_BrokeredMessage_Label)) och hello program egenskaper som **StoreName** i hello föregående exempel.

Under utveckling hello scenariot tooillustrate detta är en andra store toobe tillagda tooour retail scenario. Försäljningsdata från alla hello POS terminaler från båda butiker fortfarande ha toobe dirigeras toohello centraliserad lagersystem, men en arkivhanteraren hello instrumentpanelen verktyget bara är intresserad av hello prestanda för detta Arkiv. Du kan använda filtrering tooachieve detta prenumerationen. Observera att de anger hello när hello POS terminaler publicera meddelanden, **StoreName** programegenskapen hello-meddelande. Angivna två appbutiker, till exempel **Redmond** och **Seattle**, hello POS terminaler i hello Redmond lagra stämpel sina försäljningsdata meddelanden med en **StoreName** lika med för **Redmond**, medan hello Seattle lagra POS terminaler används en **StoreName** lika med för**Seattle**. Hej arkivhanteraren av hello Redmond lagra bara vill toosee data från dess POS-anslutningar. hello system visas på följande sätt:

![Tjänsten Bus4](./media/service-bus-create-topics-subscriptions/IC657167.gif)

tooset routning för den här du skapar hello **instrumentpanelen** prenumeration på följande sätt:

```csharp
SqlFilter dashboardFilter = new SqlFilter("StoreName = 'Redmond'");
namespaceManager.CreateSubscription("DataCollectionTopic", "Dashboard", dashboardFilter);
```

Med den här [prenumerationsfiltret](/dotnet/api/microsoft.servicebus.messaging.sqlfilter), endast meddelanden som har hello **StoreName** egenskapsuppsättning för**Redmond** blir kopierade toohello virtuella kö för hello **Instrumentpanelen** prenumeration. Det finns mycket mer toosubscription filtrering, men. Program kan ha flera filterregler per prenumeration i tillägg toohello möjlighet toomodify hello egenskaperna för ett meddelande när det överförs tooa prenumerationens virtuella kö.

## <a name="summary"></a>Sammanfattning
Alla hello orsaker toouse queuing beskrivs i [skapa program som använder Service Bus-köer](service-bus-create-queues.md) gäller även tootopics, speciellt:

* Tidsbestämd Frikoppling – meddelande producenter och konsumenter inte har toobe online på hello samtidigt.
* Belastningsutjämning – toppar i belastningen utjämnas av hello avsnittet Aktivera konsumerande program toobe försetts genomsnittlig belastning i stället för belastning.
* Belastningsutjämning – liknande tooa kön kan ha flera konkurrerande konsumenter som lyssnar på en enda prenumeration med varje meddelande som levererat tooonly en hello användare, vilket belastningsutjämning.
* Lösa kopplingar – utveckla hello messaging nätverk utan att påverka befintliga slutpunkter; till exempel lägger till prenumerationer eller ändra filter tooa avsnittet tooallow för nya konsumenter.

## <a name="next-steps"></a>Nästa steg

Se [skapa program som använder Service Bus-köer](service-bus-create-queues.md) information om hur toouse köer i hello POS retail scenario.

