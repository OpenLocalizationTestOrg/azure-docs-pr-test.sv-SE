---
title: "Skapa program som använder Azure Service Bus-ämnen och prenumerationer | Microsoft Docs"
description: "Introduktion till publicerings-prenumerera funktionerna som erbjuds av Service Bus-ämnen och prenumerationer."
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
ms.openlocfilehash: eb01120ce9578f716e5381c107faa93f0b36e358
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/18/2017
---
# <a name="create-applications-that-use-service-bus-topics-and-subscriptions"></a>Skapa program som använder Service Bus-ämnen och prenumerationer
Azure Service Bus stöder en uppsättning molnbaserade, meddelande indatavärdena mellanprogram teknologier, inklusive tillförlitliga message Queuing- och varaktig Publicera/prenumerera på meddelanden. Den här artikeln bygger på informationen i [skapa program som använder Service Bus-köer](service-bus-create-queues.md) och ger en introduktion till Publicera/prenumerera-funktionerna som erbjuds av Service Bus-ämnen.

## <a name="evolving-retail-scenario"></a>Utvecklingen av retail-scenario
Den här artikeln fortsätter retail-scenariot som används i [skapa program som använder Service Bus-köer](service-bus-create-queues.md). Kom ihåg att försäljningsdata från enskilda punkt för försäljning (POS) terminaler ska vidarebefordras till ett lagersystem som använder informationen för att avgöra när börs måste fyllas. Varje POS terminal rapporterar försäljning data genom att skicka meddelanden till den **DataCollectionQueue** kö, där de finns kvar tills de tas emot av lagersystem, som visas här:

![Service Bus 1](./media/service-bus-create-topics-subscriptions/IC657161.gif)

För att utveckla det här scenariot, ett nytt krav har lagts till i systemet: store ägaren vill kunna övervaka hur arkivet utförs i realtid.

Om du vill åtgärda det här kravet systemet måste ”tryck på” av försäljning dataströmmen. Vi vill varje meddelande som skickas av kassaterminalerna skickas till lagersystem som innan fortfarande, men vi vill att en annan kopia av varje meddelande som vi kan använda för att visa en instrumentpanelsvy till store-ägare.

Du kan använda Service Bus i en situation som den här, där du behöver varje meddelande som ska konsumeras av flera parter *avsnitt*. Innehåller ett mönster för Publicera/prenumerera där varje publicerat meddelande görs tillgänglig för en eller flera prenumerationer som registrerats med ämnet. Köer är däremot varje meddelande tas emot av en enskild konsument.

Meddelanden skickas till ett ämne på samma sätt som de skickades till en kö. Dock tas meddelanden inte emot i artikeln direkt; de tas emot från prenumerationer. Du kan se en prenumeration på ett ämne som en virtuell kö som tar emot kopior av meddelanden som skickas till detta ämne. Meddelanden tas emot från en prenumeration på samma sätt som när de tas emot från en kö.

Gå tillbaka till scenariot retail kön ersätts av ett ämne och en prenumeration har lagts till, som kan använda för inventering hantering av system component. Systemet visas nu på följande sätt:

![Service Bus 2](./media/service-bus-create-topics-subscriptions/IC657165.gif)

Den här konfigurationen utför identiskt till föregående kö-baserade designen. Det vill säga meddelanden som skickas till ämnet dirigeras till den **inventering** prenumeration som den **lagersystem** förbrukar dem.

För att stödja hantering av instrumentpanelen skapa vi en andra prenumeration på avsnittet som visas här:

![Service Bus 3](./media/service-bus-create-topics-subscriptions/IC657166.gif)

Med den här konfigurationen varje meddelande från kassaterminalerna görs tillgänglig för både den **instrumentpanelen** och **inventering** prenumerationer.

## <a name="show-me-the-code"></a>Visa koden
Artikeln [skapa program som använder Service Bus-köer](service-bus-create-queues.md) beskriver hur du registrerar dig för ett Azure-konto och skapa ett namnområde för tjänsten. Det enklaste sättet att referera till Service Bus-beroenden är att installera Service Bus [Nuget-paketet](https://www.nuget.org/packages/WindowsAzure.ServiceBus/). Du kan också hitta Service Bus-bibliotek som en del av Azure SDK. Hämtningen är tillgänglig på den [hämtningssidan för Azure SDK](https://azure.microsoft.com/downloads/).

### <a name="create-the-topic-and-subscriptions"></a>Skapa avsnittet och prenumerationer
Hanteringsåtgärder för Service Bus meddelandeentiteter (köer och publicera/prenumerera avsnitt) utförs via den [NamespaceManager](/dotnet/api/microsoft.servicebus.namespacemanager#microsoft_servicebus_namespacemanager) klass. Rätt autentiseringsuppgifter krävs för att skapa en **NamespaceManager** -instans för ett visst namnområde. Service Bus använder en [delade signatur åtkomst (SAS)](service-bus-sas.md) baserat säkerhetsmodell. Den [TokenProvider](/dotnet/api/microsoft.servicebus.tokenprovider#microsoft_servicebus_tokenprovider) klassen representerar en leverantör av säkerhetstoken med inbyggda fabriksmetoder som returnerar vissa välkända tokenleverantörer. Vi använder en [CreateSharedAccessSignatureTokenProvider](/dotnet/api/microsoft.servicebus.tokenprovider#Microsoft_ServiceBus_TokenProvider_CreateSharedAccessSignatureTokenProvider_System_String_) metod för att rymma SAS-autentiseringsuppgifter. Den [NamespaceManager](/dotnet/api/microsoft.servicebus.namespacemanager#microsoft_servicebus_namespacemanager) instans sedan konstruerade med basadressen för namnområdet för Service Bus och tokenleverantör.

Den [NamespaceManager](/dotnet/api/microsoft.servicebus.namespacemanager#microsoft_servicebus_namespacemanager) klassen innehåller metoder för att skapa, räkna upp och ta bort meddelandeentiteter. Koden som visas här visas hur **NamespaceManager** instans skapas och används för att skapa den **DataCollectionTopic** avsnittet.

```csharp
Uri uri = ServiceBusEnvironment.CreateServiceUri("sb", "test-blog", string.Empty);
string name = "RootManageSharedAccessKey";
string key = "abcdefghijklmopqrstuvwxyz";

TokenProvider tokenProvider = TokenProvider.CreateSharedAccessSignatureTokenProvider(name, key);
NamespaceManager namespaceManager = new NamespaceManager(uri, tokenProvider);

namespaceManager.CreateTopic("DataCollectionTopic");
```

Observera att det finns overloads av den [CreateTopic](/dotnet/api/microsoft.servicebus.namespacemanager#Microsoft_ServiceBus_NamespaceManager_CreateTopic_System_String_) metod att ange egenskaper för avsnittet. Du kan till exempel ange time to live (TTL) Standardvärdet för meddelanden som skickas till ämnet. Lägg sedan till den **inventering** och **instrumentpanelen** prenumerationer.

```csharp
namespaceManager.CreateSubscription("DataCollectionTopic", "Inventory");
namespaceManager.CreateSubscription("DataCollectionTopic", "Dashboard");
```

### <a name="send-messages-to-the-topic"></a>Skicka meddelanden till ämnet
För körning åtgärder på Service Bus-enheter. till exempel skicka och ta emot meddelanden, ett program måste först skapa en [MessagingFactory](/dotnet/api/microsoft.servicebus.messaging.messagingfactory#microsoft_servicebus_messaging_messagingfactory) objekt. Liknar den [NamespaceManager](/dotnet/api/microsoft.servicebus.namespacemanager#microsoft_servicebus_namespacemanager) klassen, de **MessagingFactory** instans skapas från basadressen för namnområdet för tjänsten och tokenleverantör.

```
MessagingFactory factory = MessagingFactory.Create(uri, tokenProvider);
```

Meddelanden som skickas till och tagits emot från Service Bus-ämnen är instanser av den [BrokeredMessage](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage) klass. Den här klassen består av en uppsättning standardegenskaper (t.ex [etikett](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage.label?view=azureservicebus-4.0.0#Microsoft_ServiceBus_Messaging_BrokeredMessage_Label) och [TimeToLive](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage.timetolive?view=azureservicebus-4.0.0#Microsoft_ServiceBus_Messaging_BrokeredMessage_TimeToLive)), en ordlista som används för att lagra egenskaper för programmet och en brödtext med godtyckliga programdata. Ett program kan konfigurera meddelandets brödtext genom att passera i någon typ av serialiserbara objekt (i följande exempel skickas i en **SalesData** objekt som representerar försäljningsdata från terminalen POS), som använder den [DataContractSerializer](https://msdn.microsoft.com/library/system.runtime.serialization.datacontractserializer.aspx) att serialisera objektet. Du kan också en [dataströmmen](https://msdn.microsoft.com/library/system.io.stream.aspx) objekt kan anges.

```csharp
BrokeredMessage bm = new BrokeredMessage(salesData);
bm.Label = "SalesReport";
bm.Properties["StoreName"] = "Redmond";
bm.Properties["MachineID"] = "POS_1";
```

Det enklaste sättet att skicka meddelanden till ämnet är att använda [CreateMessageSender](/dotnet/api/microsoft.servicebus.messaging.messagingfactory#Microsoft_ServiceBus_Messaging_MessagingFactory_CreateMessageSender_System_String_) att skapa en [MessageSender](/dotnet/api/microsoft.servicebus.messaging.messagesender) objekt direkt från den [MessagingFactory](/dotnet/api/microsoft.servicebus.messaging.messagingfactory) instans:

```csharp
MessageSender sender = factory.CreateMessageSender("DataCollectionTopic");
sender.Send(bm);
```

### <a name="receive-messages-from-a-subscription"></a>Ta emot meddelanden från en prenumeration
Ungefär som att använda köer kan ta emot meddelanden från en prenumeration som du kan använda en [MessageReceiver](/dotnet/api/microsoft.servicebus.messaging.messagereceiver) objekt som du skapar direkt från den [MessagingFactory](/dotnet/api/microsoft.servicebus.messaging.messagingfactory) med [CreateMessageReceiver](/dotnet/api/microsoft.servicebus.messaging.messagingfactory#Microsoft_ServiceBus_Messaging_MessagingFactory_CreateMessageReceiver_System_String_). Du kan använda ett av två olika lägen får (**ReceiveAndDelete** och **PeekLock**), enligt beskrivningen i [skapa program som använder Service Bus-köer](service-bus-create-queues.md).

Observera att när du skapar en **MessageReceiver** för prenumerationer, den *entityPath* parametern har formatet `topicPath/subscriptions/subscriptionName`. Därför att skapa en **MessageReceiver** för den **inventering** prenumeration av den **DataCollectionTopic** avsnittet *entityPath* måste anges till `DataCollectionTopic/subscriptions/Inventory`. Koden som visas på följande sätt:

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
Hittills i det här scenariot görs alla meddelanden som skickas till ämnet tillgängliga för alla registrerade prenumerationer. Den viktiga frasen görs ”tillgänglig”. När Service Bus prenumerationer visa alla meddelanden som skickas till ämnet, kan du kopiera en delmängd av dessa meddelanden till virtuella prenumerationskön. Detta görs med hjälp av prenumeration *filter*. När du skapar en prenumeration kan du ange ett filteruttryck i form av ett SQL92 style-predikat som fungerar över egenskaperna för meddelandet, både systemegenskaperna (till exempel [etikett](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage#Microsoft_ServiceBus_Messaging_BrokeredMessage_Label)) och egenskaper för program som **StoreName** i föregående exempel.

Utvecklingen av scenariot för att illustrera detta är andra store att läggas till i vårt retail-scenario. Försäljningsdata från alla Kassaterminaler från båda butiker fortfarande ska vidarebefordras till centraliserad lagersystem, men en arkivhanteraren med hjälp av verktyget instrumentpanelen bara är intresserad av prestanda för detta Arkiv. Du kan använda filtrering för att uppnå detta prenumerationen. Observera att när kassaterminalerna publicera meddelanden måste de ange den **StoreName** program-egenskap. Angivna två appbutiker, till exempel **Redmond** och **Seattle**, Kassaterminaler i Redmond store stämpel sina försäljningsdata meddelanden med en **StoreName** lika med **Redmond**, medan Seattle lagra POS terminaler används en **StoreName** lika med **Seattle**. Arkivhanteraren Redmond arkivet vill endast visa data från dess POS-anslutningar. Systemet är följande:

![Tjänsten Bus4](./media/service-bus-create-topics-subscriptions/IC657167.gif)

Om du vill konfigurera den här routning du skapar den **instrumentpanelen** prenumeration på följande sätt:

```csharp
SqlFilter dashboardFilter = new SqlFilter("StoreName = 'Redmond'");
namespaceManager.CreateSubscription("DataCollectionTopic", "Dashboard", dashboardFilter);
```

Med den här [prenumerationsfiltret](/dotnet/api/microsoft.servicebus.messaging.sqlfilter), endast meddelanden som har den **StoreName** egenskapen **Redmond** kopieras till en virtuell kö för den **instrumentpanelen** prenumeration. Det finns mycket mer att prenumerationen filtrering, men. Program kan ha flera filterregler per prenumeration utöver möjligheten att ändra egenskaperna för ett meddelande som överförs till en prenumerationens virtuella kö.

## <a name="summary"></a>Sammanfattning
Alla skäl till att använda queuing beskrivs i [skapa program som använder Service Bus-köer](service-bus-create-queues.md) gäller även för ämnen, speciellt:

* Tidsbestämd Frikoppling – meddelandeproducenter och konsumenter behöver inte vara online samtidigt.
* Belastningsutjämning – toppar i belastningen utjämnas i avsnittet Aktivera användningsprogram att etableras för genomsnittlig belastning i stället för belastning.
* Belastningsutjämning – liknar en kö, du kan ha flera konkurrerande konsumenter som lyssnar på en enda prenumeration med varje meddelande som levererat till bara en användare, vilket belastningsutjämning.
* Lösa kopplingar – utveckla meddelanden nätverket utan att påverka befintliga slutpunkter; till exempel lägger till prenumerationer eller ändra filter till ett ämne så att nya användare.

## <a name="next-steps"></a>Nästa steg

Se [skapa program som använder Service Bus-köer](service-bus-create-queues.md) information om hur du använder köer i POS retail-scenariot.

