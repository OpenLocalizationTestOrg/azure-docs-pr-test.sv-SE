---
title: aaaService bussen asynkrona meddelanden | Microsoft Docs
description: Beskrivning av Azure Service Bus asynkrona meddelanden.
services: service-bus-messaging
documentationcenter: na
author: sethmanheim
manager: timlt
editor: 
ms.assetid: f1435549-e1f2-40cb-a280-64ea07b39fc7
ms.service: service-bus-messaging
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 04/19/2017
ms.author: sethm
ms.openlocfilehash: 5ab6ddf052155a9dd975b413cfaf393119c1999d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="asynchronous-messaging-patterns-and-high-availability"></a>Asynkrona meddelandemönster och hög tillgänglighet

Asynkrona meddelanden kan implementeras i en mängd olika sätt. Köer, ämnen och prenumerationer stöder Azure Service Bus asynchronism via en store och vidarebefordra mekanism. Under normal drift av (synkron), skicka meddelanden tooqueues och ämnen och ta emot meddelanden från köer och -prenumerationer. Program som du skriver är beroende av dessa enheter alltid är tillgängliga. När hello entitetshälsa ändras på grund av tooa olika förhållanden, behöver du ett sätt tooprovide en minskad kapacitet entitet som kan uppfylla de flesta behov.

Program använder vanligtvis asynkrona meddelanden mönster tooenable ett antal kommunikationsscenarier. Du kan skapa program som klienter kan skicka meddelanden tooservices, även om hello-tjänsten inte körs. En kö kan hjälpa nivån hello läsa in genom att tillhandahålla en plats toobuffer kommunikation för program som får belastning av kommunikation. Slutligen kan du få en tydlig och effektiv belastning belastningsutjämnaren toodistribute meddelanden på flera datorer.

Överväg ett antal olika sätt som dessa enheter kan visas inte tillgängligt för en beständig meddelandesystem i ordning toomaintain tillgänglighet av någon av dessa enheter. Generellt sett finns vi hello entitet blir otillgänglig tooapplications vi skriva i hello efter olika sätt:

* Det går inte toosend meddelanden.
* Det går inte tooreceive meddelanden.
* Det går inte toomanage entiteter (skapa, hämta, uppdatera eller ta bort enheter).
* Det går inte toocontact hello-tjänsten.

För var och en av dessa fel finns olika feltillstånd som gör att ett program toocontinue tooperform arbete på vissa nivå för minskade kapaciteten. Ett system som kan skicka meddelanden, men inte tagit emot dem fortfarande kan ta emot order från kunder men kan inte bearbeta dessa order. Det här avsnittet beskrivs problem som kan uppstå och hur dessa problem begränsas. Service Bus har införs ett antal åtgärder som du måste välja i och det här avsnittet beskriver också hello reglerna för hello användning av dessa Anmäl åtgärder.

## <a name="reliability-in-service-bus"></a>Tillförlitlighet i Service Bus
Det finns flera sätt toohandle meddelandet och entiteten problem och det finns riktlinjer för hello rätt användning av dessa åtgärder. toounderstand hello riktlinjer måste du först förstå vad kan misslyckas i Service Bus. På grund av toohello utformning av Azure system till dessa problem brukar toobe tillfällig. På en hög nivå visas hello olika orsaker till att på följande sätt:

* Begränsning från ett externt system som Service Bus är beroende av. Begränsning inträffar på samverkan med resurser för lagring och beräkning.
* Problemet för ett system som Service Bus beroende. En viss del av lagring kan exempelvis stöter på problem.
* Fel i Service Bus på enskild undersystemet. I så fall kan en beräkningsnod kan få till ett inkonsekvent tillstånd och måste startas om, gör att alla enheter fungerar tooload saldo tooother noder. I sin tur kan orsaka en kort period av långsam meddelandebehandling.
* Fel i Service Bus i ett Azure-datacenter. Detta är ett ”oåterkalleligt fel” som då hello system kan inte nås för många minuter eller några timmar.

> [!NOTE]
> hello termen **lagring** kan omfatta både Azure-lagring och SQL Azure.
> 
> 

Service Bus innehåller ett antal åtgärder för dessa problem. hello följande avsnitt beskrivs varje problem och deras respektive ändringar.

### <a name="throttling"></a>Begränsning
Med Service Bus begränsning gör det möjligt samverkande meddelandet hastighet. Varje enskild nod för Service Bus innehåller många entiteter. Var och en av dessa enheter ansluter krav på hello system vad gäller CPU, minne, lagring och andra facets. När någon av dessa aspekter identifierar användning som överskrider definierade tröskelvärden, Service Bus kan neka en viss begäran. hello anroparen tar emot en [ServerBusyException] [ ServerBusyException] och försöker efter 10 sekunder.

Som en minskning hello kod läsfel hello och stoppa alla försök av hello-meddelande för minst 10 sekunder. Eftersom hello fel kan inträffa mellan delar av hello kunden program förväntas att varje oberoende körs hello logik. hello kod kan minska hello sannolikheten för begränsas genom att aktivera partitionering på en kö eller ett ämne.

### <a name="issue-for-an-azure-dependency"></a>Problem för ett Azure beroende
Andra komponenter i Azure kan ibland ha problem med tjänsten. Till exempel när ett system som använder Service Bus uppgraderas uppleva systemet tillfälligt minskade funktioner. toowork runt dessa typer av problem, Service Bus regelbundet undersöker och implementerar åtgärder. Sidoeffekter av dessa åtgärder visas. Till exempel toohandle övergående problem med storage, Service Bus implementerar ett system som gör att meddelandet skicka operations toowork konsekvent. På grund av toohello uppbyggnad hello minskning, ett meddelande skickat tar upp too15 minuter tooappear i hello påverkas kö eller prenumeration och klar för en receive-åtgärd. De flesta enheter får generellt sett inte det här problemet. Dock krävs hello antal entiteter i Service Bus i Azure får den här säkerhetsfunktionen ibland för en liten del av Service Bus-kunder.

### <a name="service-bus-failure-on-a-single-subsystem"></a>Service Bus-fel på en enda undersystemet
Omständigheter kan orsaka en intern komponent i Service Bus toobecome inkonsekvent med alla program. När Service Bus identifierar detta, samlar in data från hello programmet tooaid diagnostisera vad hände. När hello data samlas hello programmet har startats om i ett försök tooreturn den tooa konsekvent tillstånd. Den här processen sker ganska snabbt och resulterar i en entitet visas toobe är inte tillgänglig för dig tooa några minuter om vanliga stopptider är mycket kortare.

I dessa fall hello klientprogrammet genererar en [System.TimeoutException] [ System.TimeoutException] eller [MessagingException] [ MessagingException] undantag. Service Bus innehåller en lösning på problemet i hello form av automatiserade klienten logik. När hello försök perioden är slut och hello-meddelande levereras inte kan du utforska med andra funktioner, till exempel [länkas namnområden][paired namespaces]. Parad namnområden ha andra upplysningar som beskrivs i artikeln.

### <a name="failure-of-service-bus-within-an-azure-datacenter"></a>Fel i Service Bus i ett Azure-datacenter
hello trolig orsak till ett fel i ett Azure-datacenter är en misslyckad uppgradering distribution av Service Bus eller ett beroende system. Eftersom hello-plattformen har mognadslagrats har hello sannolikheten för den här typen av fel minskat. Ett datacenter-fel kan också inträffa skäl som innehåller hello följande:

* Elektriskt avbrott (strömförsörjning och genererar power försvinner).
* Anslutningen (internet bryta mellan klienter och Azure).

I båda fallen orsakade en fysisk eller syntetiska katastrof hello problemet. toowork runt problemet och se till att du kan fortfarande skicka meddelanden, kan du använda [länkas namnområden] [ paired namespaces] tooenable meddelanden skickas toobe tooa andra plats medan hello primär plats görs felfri igen. Mer information finns i [bästa praxis för isolering program mot Service Bus-avbrott och katastrofer][Best practices for insulating applications against Service Bus outages and disasters].

## <a name="paired-namespaces"></a>Kopplade namnområden
Hej [länkas namnområden] [ paired namespaces] funktionen stöder scenarier där en Service Bus-entitet eller distribution inom ett datacenter blir otillgänglig. När den här händelsen inträffar sällan, måste distribuerade system fortfarande förberedas toohandle värsta fall. Den här händelsen inträffar normalt eftersom vissa element som Service Bus beroende har uppstått ett kortsiktiga problem. toomaintain tillgänglighet under ett avbrott, Service Bus-användare kan använda två separata namnområden, helst i olika datacenter, toohost sina meddelandeentiteter. hello resten av det här avsnittet använder hello följande terminologi:

* Primär namnområde: hello namnområde som interagerar ditt program för skicka och ta emot åtgärder.
* Sekundär namnområde: hello namnområde som fungerar som en primär säkerhetskopiering toohello-namnrymd. Programlogik interagerar inte med det här namnområdet.
* Intervall för redundans: hello mängden tid tooaccept normal fel innan programmet hello växlar från hello primära namnområde toohello sekundära namnområde.

Länkas namnområden stöd *skicka tillgänglighet*. Skicka tillgänglighet bevarar hälsningsmeddelande möjlighet toosend. toouse skicka tillgänglighet ditt program måste uppfylla hello följande krav:

1. Meddelanden tas bara emot från hello primära namnområde.
2. Meddelanden som skickas tooa angivna kö eller ett ämne kan tas emot i rätt ordning.
3. Meddelanden i en session kan tas emot i rätt ordning. Det här är ett avbrott från normala funktionerna för sessioner. Det innebär att programmet använder sessioner toologically gruppera meddelanden.
4. Sessionstillstånd bevaras endast på primära hello-namnområdet.
5. hello primära kön kan anslutas och börja ta emot meddelanden innan hello sekundär kö levererar alla meddelanden i kö för hello primära.

hello följande avsnitt beskrivs hello API: er, hur hello API: er implementeras och visar exempelkod där hello-funktionen. Observera att fakturering effekter som är associerade med den här funktionen.

### <a name="hello-messagingfactorypairnamespaceasync-api"></a>Hej MessagingFactory.PairNamespaceAsync API
hello parad namnområden funktionen innehåller hello [PairNamespaceAsync] [ PairNamespaceAsync] metod på hello [Microsoft.ServiceBus.Messaging.MessagingFactory] [ Microsoft.ServiceBus.Messaging.MessagingFactory] klass:

```csharp
public Task PairNamespaceAsync(PairedNamespaceOptions options);
```

När hello uppgiften har slutförts länkning av hello namnområdet är också fullständigt och klart tooact på för alla [MessageReceiver][MessageReceiver], [QueueClient] [ QueueClient], eller [TopicClient] [ TopicClient] skapats med hello [MessagingFactory] [ MessagingFactory] instans. [Microsoft.ServiceBus.Messaging.PairedNamespaceOptions] [ Microsoft.ServiceBus.Messaging.PairedNamespaceOptions] är hello basklass för hello olika typer av länkar som är tillgängliga med en [MessagingFactory] [ MessagingFactory] objekt. För närvarande hello härledda klass är endast ett med namnet [SendAvailabilityPairedNamespaceOptions][SendAvailabilityPairedNamespaceOptions], som implementerar hello skicka tillgänglighet. [SendAvailabilityPairedNamespaceOptions] [ SendAvailabilityPairedNamespaceOptions] har en uppsättning konstruktorer som bygger på varandra. Tittar på hello konstruktor med hello de flesta parametrar, förstår du hello funktionssätt hello andra konstruktorer.

```csharp
public SendAvailabilityPairedNamespaceOptions(
    NamespaceManager secondaryNamespaceManager,
    MessagingFactory messagingFactory,
    int backlogQueueCount,
    TimeSpan failoverInterval,
    bool enableSyphon)
```

Dessa parametrar har hello följande innebörd:

* *secondaryNamespaceManager*: en initierad [NamespaceManager] [ NamespaceManager] instansen för hello sekundära namnområde som hello [PairNamespaceAsync] [ PairNamespaceAsync] metod kan använda tooset in hello sekundära namnområde. hello namespace manager är används tooobtain hello lista över köer i hello namnområde och toomake till att hello krävs eftersläpning köer finns. Om de köerna som inte finns, skapas de. [NamespaceManager] [ NamespaceManager] kräver hello möjlighet toocreate en token med hello **hantera** anspråk.
* *messagingFactory*: hello [MessagingFactory] [ MessagingFactory] instanser för sekundära hello-namnområdet. Hej [MessagingFactory] [ MessagingFactory] objektet är används toosend och, om hello [EnableSyphon] [ EnableSyphon] egenskapen för**SANT** , ta emot meddelanden från hello eftersläpning köer.
* *backlogQueueCount*: hello antalet eftersläpning köer toocreate. Det här värdet måste vara minst 1. När du skickar meddelanden toohello eftersläpning kan väljs något av dessa köer slumpmässigt. Om du ställer in hello värdet too1 kan endast en kö skulle användas. När detta sker och hello en eftersläpning kön genererar fel, hello-klienten inte kan tootry en annan eftersläpning kö och misslyckas toosend meddelandet. Vi rekommenderar att ställa in det här värdet toosome större värde och standard hello värdet too10. Du kan ändra den här tooa som är högre eller lägre värde beroende på hur mycket data som programmet skickar per dag. Varje eftersläpning kö kan innehålla upp too5 GB meddelanden.
* *failoverInterval*: hello tidsperiod under vilken du kan acceptera fel på hello primära namnområde innan du byter en enda entitet över toohello sekundära namnområde. Redundans inträffar för en entitet av entiteten. Entiteter i en enda namnrymd live ofta på olika noder i Service Bus. Ett fel i en entitet innebär inte ett fel i en annan. Du kan ange ett värde för[System.TimeSpan.Zero] [ System.TimeSpan.Zero] toofailover toohello sekundära omedelbart efter din första, icke-tillfälligt fel. Fel som utlöser hello redundans timer finns några [MessagingException] [ MessagingException] i vilka hello [IsTransient] [ IsTransient] egenskapen är false, eller en [System.TimeoutException][System.TimeoutException]. Andra undantag som [UnauthorizedAccessException] [ UnauthorizedAccessException] inte orsakar växling vid fel, eftersom de visar hello klienten är felaktigt konfigurerad. En [ServerBusyException] [ ServerBusyException] inte orsak redundans eftersom hello rätt mönstret är toowait 10 sekunder sedan skickar hello-meddelande igen.
* *enableSyphon*: Anger att den här viss länkning bör också syphon meddelanden från hello sekundära namnområde tillbaka toohello primära namnområde. I allmänhet program som skickar meddelanden ska ange ett värde för**FALSKT**; program som tar emot meddelanden måste ange ett värde för**SANT**. hello anledningen är att ofta, det finns färre meddelandet mottagare än avsändare. Du kan välja ett enda program instans referensen hello syphon uppgifter toohave beroende på hello antal mottagare. Många mottagare har fakturering konsekvenser för varje eftersläpning kö.

toouse Hej koden, skapa en primär [MessagingFactory] [ MessagingFactory] instans, en sekundär [MessagingFactory] [ MessagingFactory] instans, en sekundär [NamespaceManager] [ NamespaceManager] instans, och en [SendAvailabilityPairedNamespaceOptions] [ SendAvailabilityPairedNamespaceOptions] instans. hello-anropet kan vara så enkelt som hello följande:

```csharp
SendAvailabilityPairedNamespaceOptions sendAvailabilityOptions = new SendAvailabilityPairedNamespaceOptions(secondaryNamespaceManager, secondary);
primary.PairNamespaceAsync(sendAvailabilityOptions).Wait();
```

När hello uppgiften som returneras av hello [PairNamespaceAsync] [ PairNamespaceAsync] metoden har slutförts, allt är korrekt och toouse är klara. Innan hello uppgiften returneras kan kanske du inte har slutförts alla hello bakgrundsjobbet krävs för hello länkning toowork höger. Du bör därför inte börja skicka meddelanden tills hello uppgiften returnerar. Eventuella fel, till exempel felaktiga autentiseringsuppgifter eller fel toocreate hello eftersläpning köer utlöstes dessa undantag när hello uppgiften har slutförts. När hello uppgiften returnerar, kontrollera att hello köer hittades eller skapas genom att undersöka hello [BacklogQueueCount] [ BacklogQueueCount] -egenskapen i din [SendAvailabilityPairedNamespaceOptions] [ SendAvailabilityPairedNamespaceOptions] instans. För hello föregående kod som åtgärden visas på följande sätt:

```csharp
if (sendAvailabilityOptions.BacklogQueueCount < 1)
{
    // Handle case where no queues were created.
}
```

## <a name="next-steps"></a>Nästa steg
Nu när du har lärt dig hello grunderna för asynkrona meddelanden i Service Bus kan du läsa mer information om [länkas namnområden][paired namespaces].

[ServerBusyException]: /dotnet/api/microsoft.servicebus.messaging.serverbusyexception
[System.TimeoutException]: https://msdn.microsoft.com/library/system.timeoutexception.aspx
[MessagingException]: /dotnet/api/microsoft.servicebus.messaging.messagingexception
[Best practices for insulating applications against Service Bus outages and disasters]: service-bus-outages-disasters.md
[Microsoft.ServiceBus.Messaging.MessagingFactory]: /dotnet/api/microsoft.servicebus.messaging.messagingfactory
[MessageReceiver]: /dotnet/api/microsoft.servicebus.messaging.messagereceiver
[QueueClient]: /dotnet/api/microsoft.servicebus.messaging.queueclient
[TopicClient]: /dotnet/api/microsoft.servicebus.messaging.topicclient
[Microsoft.ServiceBus.Messaging.PairedNamespaceOptions]: /dotnet/api/microsoft.servicebus.messaging.pairednamespaceoptions
[MessagingFactory]: /dotnet/api/microsoft.servicebus.messaging.messagingfactory
[SendAvailabilityPairedNamespaceOptions]: /dotnet/api/microsoft.servicebus.messaging.sendavailabilitypairednamespaceoptions
[NamespaceManager]: /dotnet/api/microsoft.servicebus.namespacemanager
[PairNamespaceAsync]: /dotnet/api/microsoft.servicebus.messaging.messagingfactory#Microsoft_ServiceBus_Messaging_MessagingFactory_PairNamespaceAsync_Microsoft_ServiceBus_Messaging_PairedNamespaceOptions_
[EnableSyphon]: /dotnet/api/microsoft.servicebus.messaging.sendavailabilitypairednamespaceoptions#Microsoft_ServiceBus_Messaging_SendAvailabilityPairedNamespaceOptions_EnableSyphon
[System.TimeSpan.Zero]: https://msdn.microsoft.com/library/system.timespan.zero.aspx
[IsTransient]: /dotnet/api/microsoft.servicebus.messaging.messagingexception#Microsoft_ServiceBus_Messaging_MessagingException_IsTransient
[UnauthorizedAccessException]: https://msdn.microsoft.com/library/system.unauthorizedaccessexception.aspx
[BacklogQueueCount]: /dotnet/api/microsoft.servicebus.messaging.sendavailabilitypairednamespaceoptions?redirectedfrom=MSDN#Microsoft_ServiceBus_Messaging_SendAvailabilityPairedNamespaceOptions_BacklogQueueCount
[paired namespaces]: service-bus-paired-namespaces.md
