---
title: "aaaProgramming guide för Händelsehubbar i Azure | Microsoft Docs"
description: "Skriva kod för Händelsehubbar i Azure med hjälp av hello Azure .NET SDK."
services: event-hubs
documentationcenter: na
author: sethmanheim
manager: timlt
editor: 
ms.assetid: 64cbfd3d-4a0e-4455-a90a-7f3d4f080323
ms.service: event-hubs
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: tbd
ms.date: 08/17/2017
ms.author: sethm
ms.openlocfilehash: 43bebd126c2311af9e3daeb52324132b66cf0884
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="event-hubs-programming-guide"></a>Programmeringsguide för händelsehubbar

Den här artikeln beskrivs några vanliga scenarier i att skriva kod med Azure Event Hubs och hello Azure .NET SDK. Den förutsätter att du har en grundläggande förståelse av händelsehubbar. En översikt av Händelsehubbar finns hello [översikt av Händelsehubbar](event-hubs-what-is-event-hubs.md).

## <a name="event-publishers"></a>Händelseutfärdare

Du skickar händelser tooan händelsehubb antingen med HTTP POST eller via en AMQP 1.0-anslutning. Hej valet av vilken toouse och när beror på hello specifika scenario du står inför. AMQP 1.0-anslutningar är avgiftsbelagda som asynkrona anslutningar i Service Bus och är mer lämpliga i scenarier med ofta högre meddelandevolymer och lägre svarstidskrav, eftersom de tillhandahåller en permanent meddelandekanal.

Du skapar och hanterar Händelsehubbar med hello [NamespaceManager][] klass. När du använder hello .NET hanterade API: er, hello primära konstruktioner för att publicera data tooEvent-hubbar är hello [EventHubClient](/dotnet/api/microsoft.servicebus.messaging.eventhubclient) och [EventData][] klasser. [EventHubClient][] innehåller kommunikationskanalen hello AMQP över vilka händelser skickas toohello händelsehubb. Hej [EventData][] klass representerar en händelse och är används toopublish meddelanden tooan händelsehubb. Den här klassen innehåller hello brödtexten, vissa metadata och rubrikinformation om händelsen hello. Andra egenskaper läggs toohello [EventData][] när det överförs via en händelsehubb.

## <a name="get-started"></a>Kom igång

hello .NET-klasser som har stöd för Händelsehubbar finns i hello Microsoft.ServiceBus.dll-sammansättningen. Hej enklaste sättet tooreference hello Service Bus-API och tooconfigure ditt program med alla hello Service Bus-beroenden är toodownload hello [Service Bus NuGet-paketet](https://www.nuget.org/packages/WindowsAzure.ServiceBus). Du kan också använda hello [Pakethanterarkonsolen](http://docs.nuget.org/docs/start-here/using-the-package-manager-console) i Visual Studio. toodo så utfärda hello följande kommando i hello [Pakethanterarkonsolen](http://docs.nuget.org/docs/start-here/using-the-package-manager-console) fönster:

```
Install-Package WindowsAzure.ServiceBus
```

## <a name="create-an-event-hub"></a>Skapa en händelsehubb
Du kan använda hello [NamespaceManager][] klassen toocreate Händelsehubbar. Exempel:

```csharp
var manager = new Microsoft.ServiceBus.NamespaceManager("mynamespace.servicebus.windows.net");
var description = manager.CreateEventHub("MyEventHub");
```

I de flesta fall bör du använda hello [CreateEventHubIfNotExists][] metoder tooavoid generera fel om hello-tjänsten startas om. Exempel:

```csharp
var description = manager.CreateEventHubIfNotExists("MyEventHub");
```

Alla Händelsehubbar åtgärder för att skapa, inklusive [CreateEventHubIfNotExists][], kräver **hantera** behörigheter på hello namnområde i fråga. Om du vill toolimit hello behörigheter för dina utfärdar- eller konsumentprogram program, kan du undvika dessa Skapa åtgärden anrop i produktionskod när du använder autentiseringsuppgifter med begränsad behörighet.

Hej [EventHubDescription](/dotnet/api/microsoft.servicebus.messaging.eventhubdescription) klassen innehåller information om en händelsehubb, inklusive hello auktoriseringsregler Kvarhållningsintervall för hello-meddelande, partitions-ID: N, status och sökväg. Du kan använda den här klassen tooupdate hello metadata i en händelsehubb.

## <a name="create-an-event-hubs-client"></a>Skapa en händelsehubbklient
hello primära klassen för att interagera med Händelsehubbar är [Microsoft.ServiceBus.Messaging.EventHubClient][EventHubClient]. Den här klassen innehåller både sändar- och mottagarfunktioner. Du kan skapa en instans av den här klassen med hello [skapa](/dotnet/api/microsoft.servicebus.messaging.eventhubclient.create) -metoden, som visas i följande exempel hello.

```csharp
var client = EventHubClient.Create(description.Path);
```

Den här metoden använder anslutningsinformationen för hello Service Bus i hello App.config-filen i hello `appSettings` avsnitt. Ett exempel på hello `appSettings` XML används toostore hello Service Bus-anslutningsinformation, hello i dokumentationen för hello [Microsoft.ServiceBus.Messaging.EventHubClient.Create(System.String)](/dotnet/api/microsoft.servicebus.messaging.eventhubclient#Microsoft_ServiceBus_Messaging_EventHubClient_Create_System_String_) metod.

Ett annat alternativ är toocreate hello klient från en anslutningssträng. Det här alternativet fungerar bra när du använder arbetsroller i Azure, eftersom du kan lagra hello sträng i hello konfigurationsegenskaperna för arbetaren hello. Exempel:

```csharp
EventHubClient.CreateFromConnectionString("your_connection_string");
```

hello anslutningssträngen blir i samma format som det visas i hello App.config-filen för föregående metoder för hello hello:

```
Endpoint=sb://[namespace].servicebus.windows.net/;SharedAccessKeyName=RootManageSharedAccessKey;SharedAccessKey=[key]
```

Slutligen är det också möjligt toocreate en [EventHubClient][] objekt från en [MessagingFactory](/dotnet/api/microsoft.servicebus.messaging.messagingfactory) instansen, som visas i följande exempel hello.

```csharp
var factory = MessagingFactory.CreateFromConnectionString("your_connection_string");
var client = factory.CreateEventHubClient("MyEventHub");
```

Det är viktigt att ytterligare toonote [EventHubClient][] objekt som skapas från en instans av en meddelandefabrik återanvänder hello samma underliggande TCP-anslutningen. Därför har de här objekten en gräns för genomflöde på klientsidan. Hej [skapa](/dotnet/api/microsoft.servicebus.messaging.eventhubclient#Microsoft_ServiceBus_Messaging_EventHubClient_Create_System_String_) metoden återanvänder en enda meddelandefabrik. Om du behöver mycket hög genomströmning från en enda avsändare kan du skapa flera meddelandefabriker och ett [EventHubClient][]-objekt från respektive meddelandefabrik.

## <a name="send-events-tooan-event-hub"></a>Skicka händelser tooan händelsehubb
Du skickar händelser tooan händelsehubb genom att skapa en [EventData][] instans och skicka den via hello [skicka](/dotnet/api/microsoft.servicebus.messaging.eventhubclient#Microsoft_ServiceBus_Messaging_EventHubClient_Send_Microsoft_ServiceBus_Messaging_EventData_) metod. Den här metoden tar en enda [EventData][] parameter i instansen och skickar den synkront tooan händelsehubb.

## <a name="event-serialization"></a>Händelseserialisering
Hej [EventData][] klassen har [fyra överbelastade konstruktorer](/dotnet/api/microsoft.servicebus.messaging.eventdata#constructors_) som tar olika parametrar, till exempel ett objekt och serialiserare, en bytematris eller en dataström. Det är också möjligt tooinstantiate hello [EventData][] klassen och ange hello brödtextströmmen efteråt. När du använder JSON med [EventData][], kan du använda **Encoding.UTF8.GetBytes()** tooretrieve hello Bytematrisen för en JSON-kodad sträng.

## <a name="partition-key"></a>Partitionsnyckeln
Hej [EventData][] klassen har en [PartitionKey][] egenskap som gör hello avsändaren toospecify ett värde som hashas tooproduce en tilldelning av partitionen. En partitionsnyckel garanterar att alla hello händelser med samma nyckel skickas hello toohello samma partition i händelsehubben hello. Vanliga partitionsnycklar är användarsession-ID:n och unika avsändar-ID:n. Hej [PartitionKey][] är valfri och kan anges när du använder hello [Microsoft.ServiceBus.Messaging.EventHubClient.Send(Microsoft.ServiceBus.Messaging.EventData)](/dotnet/api/microsoft.servicebus.messaging.eventhubclient#Microsoft_ServiceBus_Messaging_EventHubClient_Send_Microsoft_ServiceBus_Messaging_EventData_) eller [Microsoft.ServiceBus.Messaging.EventHubClient.SendAsync(Microsoft.ServiceBus.Messaging.EventData)](/dotnet/api/microsoft.servicebus.messaging.eventhubclient#Microsoft_ServiceBus_Messaging_EventHubClient_SendAsync_Microsoft_ServiceBus_Messaging_EventData_) metoder. Om du inte anger ett värde för [PartitionKey][], skickas händelser är distribuerade toopartitions med hjälp av en resursallokeringsmodell.

### <a name="availability-considerations"></a>Överväganden för tillgänglighet

Med hjälp av en partitionsnyckel är valfritt och bör du noggrant om huruvida toouse en. I många fall är med en partitionsnyckel ett bra val om händelsen ordning är viktig. När du använder en partitionsnyckel partitionerna kräver tillgänglighet på en enda nod och driftavbrott med tiden. till exempel när compute-noder omstart och korrigering. Om du ställer in ett partitions-ID och den aktuella partitionen är tillgänglig av någon anledning, misslyckas därför ett försök tooaccess hello data i den aktuella partitionen. Om hög tillgänglighet är de viktigaste inte ange en partitionsnyckel; i så fall skickas händelser toopartitions med hello resursallokeringsmodell som beskrivs ovan. I detta scenario upprättar du ett uttryckligt val mellan tillgänglighet (ingen partitions-ID) och konsekvens (fästning händelser tooa partitions-ID).

Ett annat övervägande hanterar fördröjningar i bearbetning av händelser. I vissa fall kan det vara bättre toodrop data och försök än tootry och håll dig uppdaterad med bearbetning, vilket kan medföra ytterligare nedströms bearbetning fördröjningar. Till exempel med ett lager ticker är det bättre toowait för fullständig uppdaterade data, men i en levande chatt eller VOIP-scenario i stället har du hello data snabbt, även om den inte är klar.

Den angivna dessa tillgänglighet i dessa scenarier kan du välja något av följande strategier för felhantering hello:

- Stoppa (stop läsning från Händelsehubbar förrän saker har åtgärdats)
- Släpp (meddelandena inte viktig, släppa dem)
- Försök (försök hello meddelanden som du ser passar)
- [Obeställbara](../service-bus-messaging/service-bus-dead-letter-queues.md) (Använd en kö eller en annan händelse hubb toodead brev endast hello meddelanden du kunde inte behandla)

Mer information och en beskrivning av hur hello avvägningarna mellan tillgänglighet och konsekvens finns [tillgänglighet och konsekvens i Händelsehubbar](event-hubs-availability-and-consistency.md). 

## <a name="batch-event-send-operations"></a>Åtgärder för att skicka batchhändelser
Att skicka händelser i batchar kan öka genomflödet dramatiskt. Hej [SendBatch](/dotnet/api/microsoft.servicebus.messaging.eventhubclient#Microsoft_ServiceBus_Messaging_EventHubClient_SendBatch_System_Collections_Generic_IEnumerable_Microsoft_ServiceBus_Messaging_EventData__) metoden tar en **IEnumerable** parameter av typen [EventData][] och skickar hello hela batchen som en atomisk åtgärd toohello händelsehubb.

```csharp
public void SendBatch(IEnumerable<EventData> eventDataList);
```

Observera att en enskild batch inte får överstiga hello 256 KB-gränsen för en händelse. Dessutom använder varje meddelande i hello batch hello samma utgivaridentitet. Det är hello ansvar hello avsändaren tooensure som hello batchen inte överskrider hello maximala händelsestorleken. Om den gör det genereras ett **Skicka**-felmeddelande för klienten.

## <a name="send-asynchronously-and-send-at-scale"></a>Skicka asynkront och skicka i skala
Du kan även skicka händelser tooan händelsehubb asynkront. Skicka asynkront kan öka hello hastighet som en klient kan toosend händelser. Båda hello [skicka](/dotnet/api/microsoft.servicebus.messaging.eventhubclient#Microsoft_ServiceBus_Messaging_EventHubClient_Send_Microsoft_ServiceBus_Messaging_EventData_) och [SendBatch](/dotnet/api/microsoft.servicebus.messaging.eventhubclient#Microsoft_ServiceBus_Messaging_EventHubClient_SendBatch_System_Collections_Generic_IEnumerable_Microsoft_ServiceBus_Messaging_EventData__) metoder är tillgängliga i asynkrona versioner som returnerar en [aktivitet](https://msdn.microsoft.com/library/system.threading.tasks.task.aspx) objekt. När den här tekniken kan öka genomflödet kan den också göra hello klienthändelser toocontinue toosend även när den begränsas av hello händelsehubbtjänsten och kan resultera i hello klienten råkar ut för fel eller förlorade meddelanden om inte implementeras ordentligt. Du kan dessutom använda hello [RetryPolicy](/dotnet/api/microsoft.servicebus.messaging.cliententity#Microsoft_ServiceBus_Messaging_ClientEntity_RetryPolicy) hello toocontrol klienten gör klientalternativ egenskap.

## <a name="create-a-partition-sender"></a>Skapa en partitionsavsändare
Det vanligaste toosend händelser tooan-händelsehubb utan någon partitionsnyckel, men i vissa fall kanske du toosend händelser direkt tooa angivna partitionen. Exempel:

```csharp
var partitionedSender = client.CreatePartitionedSender(description.PartitionIds[0]);
```

[CreatePartitionedSender](/dotnet/api/microsoft.servicebus.messaging.eventhubclient#Microsoft_ServiceBus_Messaging_EventHubClient_CreatePartitionedSender_System_String_) returnerar ett [EventHubSender](/dotnet/api/microsoft.servicebus.messaging.eventhubsender) objekt som du kan använda toopublish händelser tooa specifika händelser hubb partition.

## <a name="event-consumers"></a>Händelsekonsumenter
Händelsehubbar har två primära modeller för händelsekonsumtion: direkta mottagare och abstraktioner på högre nivåer som t.ex. [EventProcessorHost][]. Direkta mottagare är ansvariga för sin egen samordning av åtkomst toopartitions inom en konsumentgrupp.

### <a name="direct-consumer"></a>Direktkonsumenten
hello snabbaste sättet tooread från en partition inom en konsumentgrupp är toouse hello [EventHubReceiver](/dotnet/apie/microsoft.servicebus.messaging.eventhubreceiver) klass. toocreate en instans av den här klassen måste du använda en instans av hello [EventHubConsumerGroup](/dotnet/api/microsoft.servicebus.messaging.eventhubconsumergroup) klass. I följande exempel hello, måste hello partitions-ID anges när du skapar hello mottagaren för konsumentgruppen hello.

```csharp
EventHubConsumerGroup group = client.GetDefaultConsumerGroup();
var receiver = group.CreateReceiver(client.GetRuntimeInformation().PartitionIds[0]);
```

Hej [CreateReceiver](/dotnet/api/microsoft.servicebus.messaging.eventhubconsumergroup#methods_summary) metod har flera överlagringar som underlättar kontroll över hello läsare som skapas. De här metoderna omfattar att ange en offset som en sträng eller en tidsstämpel och hello möjlighet toospecify om tooinclude denna angivna offset i hello returnerade strömma eller starta efter den. När du har skapat hello mottagaren kan du ta emot händelser på hello returnerade objekt. Hej [Receive](/dotnet/api/microsoft.servicebus.messaging.eventhubreceiver#methods_summary) metod har fyra överlagringar som kontrollen hello får parametrarna för Mottagningsåtgärden, t.ex batchstorlek och väntetid. Du kan använda hello asynkrona versionerna av dessa metoder tooincrease hello genomflödet av en konsument. Exempel:

```csharp
bool receive = true;
string myOffset;
while(receive)
{
    var message = receiver.Receive();
    myOffset = message.Offset;
    string body = Encoding.UTF8.GetString(message.GetBytes());
    Console.WriteLine(String.Format("Received message offset: {0} \nbody: {1}", myOffset, body));
}
```

Hello meddelanden tas emot i hello ordning som de skickades toohello händelsehubb med avseende tooa specifik partition. hello offset är en sträng token används tooidentify ett meddelande i en partition.

Observera att en enda partition inom en konsumentgrupp inte samtidigt kan ha fler än fem läsare anslutna vid en viss tidpunkt. När läsare ansluts eller kopplas, kan sessionerna förbli aktiva i flera minuter innan hello tjänsten känner av att de har kopplats från. Under den här tiden kan återansluta tooa partition misslyckas. En komplett exempel för att skriva en direkt mottagare för Händelsehubbar finns hello [Event Hubs Direct Receivers](https://code.msdn.microsoft.com/Event-Hub-Direct-Receivers-13fa95c6) exempel.

### <a name="event-processor-host"></a>Värd för händelsebearbetning
Hej [EventProcessorHost][] klassen bearbetar data från Händelsehubbar. När du skapar händelseläsare på hello .NET-plattformen, bör du använda den här implementeringen. [EventProcessorHost][] ger en trådsäker, flerprocessig, säker körningsmiljö för implementeringar av händelseprocessorer som också ger hantering av kontrollpunkter och hantering av partitionsleasing.

toouse hello [EventProcessorHost][] klassen, som du kan implementera [IEventProcessor](/dotnet/api/microsoft.servicebus.messaging.ieventprocessor). Det här gränssnittet innehåller tre metoder:

* [OpenAsync](/dotnet/api/microsoft.servicebus.messaging.ieventprocessor#Microsoft_ServiceBus_Messaging_IEventProcessor_OpenAsync_Microsoft_ServiceBus_Messaging_PartitionContext_)
* [CloseAsync](/dotnet/api/microsoft.servicebus.messaging.ieventprocessor#Microsoft_ServiceBus_Messaging_IEventProcessor_CloseAsync_Microsoft_ServiceBus_Messaging_PartitionContext_Microsoft_ServiceBus_Messaging_CloseReason_)
* [ProcessEventsAsync](/dotnet/api/microsoft.servicebus.messaging.ieventprocessor#Microsoft_ServiceBus_Messaging_IEventProcessor_ProcessEventsAsync_Microsoft_ServiceBus_Messaging_PartitionContext_System_Collections_Generic_IEnumerable_Microsoft_ServiceBus_Messaging_EventData__)

Skapa en instans av toostart händelsebearbetning, [EventProcessorHost][], tillhandahåller hello lämpliga parametrar för händelsehubben. Anropa sedan [RegisterEventProcessorAsync](/dotnet/api/microsoft.servicebus.messaging.eventprocessorhost#Microsoft_ServiceBus_Messaging_EventProcessorHost_RegisterEventProcessorAsync__1) tooregister din [IEventProcessor](/dotnet/api/microsoft.servicebus.messaging.ieventprocessor) implementering med hello körning. Hello-värden försöker nu tooacquire en leasing för varje partition i hello händelsehubb med hjälp av en ”girig” algoritm. De här leasingarna varar en viss tid och måste sedan förnyas. Allteftersom nya noder worker-instanser i det här fallet är online, de placerar den ut leasingreservationer och med tiden skiftar hello belastningen mellan noder som varje försöker tooacquire mer lån.

![Värd för händelsebearbetning](./media/event-hubs-programming-guide/IC759863.png)

Med tiden etableras ett jämviktsläge. Den här dynamiska funktionen gör att processorbaserad autoskalning toobe tillämpas tooconsumers för både skala upp och skala ned. Eftersom Händelsehubbar inte har ett direkt begrepp om antalet meddelanden, Genomsnittlig CPU-användning är ofta hello bästa mekanism toomeasure tillbaka serverdelen eller konsumenterna skala. Om utgivare börjar toopublish fler händelser än konsumenterna kan bearbeta, kan hello CPU för konsumenterna vara används toocause Autoskala på worker-instanser.

Hej [EventProcessorHost][] klassen implementerar också en mekanism för Azure storage-baserade kontrollpunkter. Den här mekanismen lagrar hello förskjutning på en per partition-basis, så att varje konsument kan fastställa vilka hello senaste kontrollpunkten från hello föregående konsumenten kunde. Eftersom partitioner övergår mellan noder via leasingar är det här är hello synkroniseringsmekanism som gör det lättare att skifta belastningar.

## <a name="publisher-revocation"></a>Återkallande av utgivare
Dessutom toohello avancerade funktioner för körning av [EventProcessorHost][], Event Hubs gör att återkalla utgivare i ordning tooblock specifika utgivare från att skicka händelsen tooan händelsehubb. Dessa funktioner är särskilt användbart om en utgivartoken har komprometterats eller om en programuppdatering får dem toobehave felaktigt. I sådana situationer blockeras hello utgivarens identitet, vilket är en del av deras SAS-token, från att publicera händelser.

Mer information om att återkalla utgivare och hur toosend tooEvent hubbar som utgivare, se hello [Event Hubs Large Scale Secure Publishing](https://code.msdn.microsoft.com/Service-Bus-Event-Hub-99ce67ab) exempel.

## <a name="next-steps"></a>Nästa steg
toolearn mer information om scenarier i Händelsehubbar finns i följande länkar:

* [Event Hubs API-översikt](event-hubs-api-overview.md)
* [Vad är Händelsehubbar](event-hubs-what-is-event-hubs.md)
* [Tillgänglighet och konsekvens i Event Hubs](event-hubs-availability-and-consistency.md)
* [Händelsebearbetningsvärd API-referens](/dotnet/api/microsoft.servicebus.messaging.eventprocessorhost)

[NamespaceManager]: /dotnet/api/microsoft.servicebus.namespacemanager
[EventHubClient]: /dotnet/api/microsoft.servicebus.messaging.eventhubclient
[EventData]: /dotnet/api/microsoft.servicebus.messaging.eventdata
[CreateEventHubIfNotExists]: /dotnet/api/microsoft.servicebus.namespacemanager.createeventhubifnotexists
[PartitionKey]: /dotnet/api/microsoft.servicebus.messaging.eventdata#Microsoft_ServiceBus_Messaging_EventData_PartitionKey
[EventProcessorHost]: /dotnet/api/microsoft.servicebus.messaging.eventprocessorhost
