---
title: aaaAzure Service Bus-meddelanden undantag | Microsoft Docs
description: "Lista över Service Bus-meddelanden undantag och föreslagna åtgärder."
services: service-bus-messaging
documentationcenter: na
author: sethmanheim
manager: timlt
editor: 
ms.assetid: 3d8526fe-6e47-4119-9f3e-c56d916a98f9
ms.service: service-bus-messaging
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/06/2017
ms.author: sethm
ms.openlocfilehash: 0a206b7bbc808c1190044c1dfd6ffd47b9d454fb
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="service-bus-messaging-exceptions"></a>Service Bus – undantag för meddelanden
Den här artikeln innehåller vissa undantag som genereras av hello Microsoft Azure Service Bus-meddelanden API: er. Den här referensen är ämne toochange, så kolla igen efter uppdateringar.

## <a name="exception-categories"></a>Undantag kategorier
hello messaging API: er skapa undantag som kan delas in i följande kategorier hello, tillsammans med hello associerade åtgärder du kan vidta tootry toofix dem. Observera att hello innebörd och orsakerna till ett undantag kan variera beroende på hello typ av meddelandeentitet (köer/ämnen eller Händelsehubbar):

1. Användaren kodningsfel ([System.ArgumentException](https://msdn.microsoft.com/library/system.argumentexception.aspx), [System.InvalidOperationException](https://msdn.microsoft.com/library/system.invalidoperationexception.aspx), [System.OperationCanceledException](https://msdn.microsoft.com/library/system.operationcanceledexception.aspx), [ System.Runtime.Serialization.SerializationException](https://msdn.microsoft.com/library/system.runtime.serialization.serializationexception.aspx)). Allmän åtgärd: försök toofix hello kod innan du fortsätter.
2. Installationen/konfigurationsfel ([Microsoft.ServiceBus.Messaging.MessagingEntityNotFoundException](/dotnet/api/microsoft.servicebus.messaging.messagingentitynotfoundexception), [System.UnauthorizedAccessException](https://msdn.microsoft.com/library/system.unauthorizedaccessexception.aspx). Allmän åtgärd: Granska konfigurationen och ändra vid behov.
3. Tillfälligt undantag ([Microsoft.ServiceBus.Messaging.MessagingException](/dotnet/api/microsoft.servicebus.messaging.messagingexception), [Microsoft.ServiceBus.Messaging.ServerBusyException](/dotnet/api/microsoft.servicebus.messaging.serverbusyexception), [ Microsoft.ServiceBus.Messaging.MessagingCommunicationException](/dotnet/api/microsoft.servicebus.messaging.messagingcommunicationexception)). Allmän åtgärd: gör om åtgärden hello eller meddela användare.
4. Andra undantag ([System.Transactions.TransactionException](https://msdn.microsoft.com/library/system.transactions.transactionexception.aspx), [System.TimeoutException](https://msdn.microsoft.com/library/system.timeoutexception.aspx), [Microsoft.ServiceBus.Messaging.MessageLockLostException](/dotnet/api/microsoft.servicebus.messaging.messagelocklostexception), [Microsoft.ServiceBus.Messaging.SessionLockLostException](/dotnet/api/microsoft.servicebus.messaging.sessionlocklostexception)). Allmän åtgärd: specifika toohello undantagstyp; Se tabellen toohello i hello efter avsnittet. 

## <a name="exception-types"></a>Typer av undantag
hello följande tabell visas meddelanden undantag typer och orsaker och anteckningar föreslagna åtgärder du kan vidta.

| **Undantagstyp** | **Exempel-beskrivning/orsak** | **Föreslagen åtgärd** | **Tänk på automatisk omedelbara försök** |
| --- | --- | --- | --- |
| [TimeoutException](https://msdn.microsoft.com/library/system.timeoutexception.aspx) |hello servern svarade inte toohello begärda åtgärd inom hello angetts som styrs av [OperationTimeout](/dotnet/api/microsoft.servicebus.messaging.messagingfactorysettings#Microsoft_ServiceBus_Messaging_MessagingFactorySettings_OperationTimeout). hello-server kan ha avslutat hello den begärda åtgärden. Detta kan inträffa på grund av toonetwork eller andra infrastruktur fördröjningar. |Hello systemtillstånd för konsekvens och försök igen om det behövs. Se [tidsgränsfel](#timeoutexception). |Försök igen kan i vissa fall. Lägg till nytt försök logik toocode. |
| [InvalidOperationException](https://msdn.microsoft.com/library/system.invalidoperationexception.aspx) |hello begärt användaren åtgärden inte tillåts i hello server eller tjänst. Se hello Undantagsmeddelande mer information. Till exempel [Slutför](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage#Microsoft_ServiceBus_Messaging_BrokeredMessage_Complete) genererar det här undantaget om hello-meddelande togs emot i [ReceiveAndDelete](/dotnet/api/microsoft.servicebus.messaging.receivemode) läge. |Kontrollera hello koden och hello-dokumentationen. Kontrollera att hello begärda åtgärden är ogiltig. |Det hjälper inte försök igen. |
| [OperationCanceledException](https://msdn.microsoft.com/library/system.operationcanceledexception.aspx) |Ett försök har gjorts tooinvoke en åtgärd på ett objekt som redan har stängts, avbröts eller tagits bort. I sällsynta fall hello omgivande transaktion har redan tagits bort. |Kontrollera hello kod och kontrollera att den inte anropa åtgärder på ett borttaget objekt. |Det hjälper inte försök igen. |
| [UnauthorizedAccessException](https://msdn.microsoft.com/library/system.unauthorizedaccessexception.aspx) |Hej [TokenProvider](/dotnet/api/microsoft.servicebus.tokenprovider) objektet kunde inte hämta en token, hello token är ogiltig eller hello token innehåller inte hello anspråk krävs tooperform hello igen. |Kontrollera att hello tokenleverantör skapas med hello rätt värden. Kontrollera hello konfigurationen för hello Access Control service. |Försök igen kan i vissa fall. Lägg till nytt försök logik toocode. |
| [ArgumentException](https://msdn.microsoft.com/library/system.argumentexception.aspx)<br /> [ArgumentNullException](https://msdn.microsoft.com/library/system.argumentnullexception.aspx)<br />[ArgumentOutOfRangeException](https://msdn.microsoft.com/library/system.argumentoutofrangeexception.aspx) |En eller flera argument angavs toohello metoden är ogiltiga.<br /> hello URI som levererats för[NamespaceManager](/dotnet/api/microsoft.servicebus.namespacemanager) eller [skapa](/dotnet/api/microsoft.servicebus.messaging.messagingfactory#Microsoft_ServiceBus_Messaging_MessagingFactory_Create_System_Collections_Generic_IEnumerable_System_Uri__) innehåller sökvägen segment.<br /> hello URI-schema som angetts för[NamespaceManager](/dotnet/api/microsoft.servicebus.namespacemanager) eller [skapa](/dotnet/api/microsoft.servicebus.messaging.messagingfactory#Microsoft_ServiceBus_Messaging_MessagingFactory_Create_System_Collections_Generic_IEnumerable_System_Uri__) är ogiltig. <br />hello egenskapens värde är större än 32KB. |Kontrollera hello anropande koden och kontrollera hello argumenten är korrekta. |Det hjälper inte försök igen. |
| [MessagingEntityNotFoundException](/dotnet/api/microsoft.servicebus.messaging.messagingentitynotfoundexception) |Entitet som är associerad med hello åtgärden finns inte eller har tagits bort. |Kontrollera att hello entiteten finns. |Det hjälper inte försök igen. |
| [MessageNotFoundException](/dotnet/api/microsoft.servicebus.messaging.messagenotfoundexception) |Försök tooreceive ett meddelande med ett visst sekvensnummer. Det här meddelandet hittades inte. |Kontrollera att hello-meddelande har tagits emot redan. Kontrollera hello systemkön kön toosee om hello-meddelande har deadlettered. |Det hjälper inte försök igen. |
| [MessagingCommunicationException](/dotnet/api/microsoft.servicebus.messaging.messagingcommunicationexception) |Klienten är inte kan tooestablish en anslutning tooService Bus. |Kontrollera att hello angivna värdnamnet är rätt och hello värden kan nås. |Försök igen kan hjälpa om återkommande anslutningsproblem. |
| [ServerBusyException](/dotnet/api/microsoft.servicebus.messaging.serverbusyexception) |Tjänsten är inte kan tooprocess hello begäran just nu. |Klienten kan vänta på en viss tidsperiod och försök hello igen. |Klienten kan försöka igen efter vissa intervall. Om ett nytt försök resulterar i en annan undantag, kontrollera försök beteendet för undantaget. |
| [MessageLockLostException](/dotnet/api/microsoft.servicebus.messaging.messagelocklostexception) |Lås token som är associerade med hello-meddelande har upphört att gälla eller hello Lås token finns inte. |Ta bort hello-meddelande. |Det hjälper inte försök igen. |
| [SessionLockLostException](/dotnet/api/microsoft.servicebus.messaging.sessionlocklostexception) |Lås som är associerade med den här sessionen går förlorad. |Avbryt hello [MessageSession](/dotnet/api/microsoft.servicebus.messaging.messagesession) objekt. |Det hjälper inte försök igen. |
| [MessagingException](/dotnet/api/microsoft.servicebus.messaging.messagingexception) |Generisk messaging undantag som utlöses i hello följande fall:<br /> Ett försök görs toocreate en [QueueClient](/dotnet/api/microsoft.servicebus.messaging.queueclient) med ett namn eller en sökväg som tillhör tooa olika entitetstypen (till exempel ett ämne).<br />  Ett försök har gjorts toosend meddelandet större än 256KB. hello-server eller tjänsten påträffade ett fel under bearbetningen av hello begäran. Hello Undantagsmeddelande mer information finns i. Detta är vanligtvis ett tillfälligt undantag. |Kontrollera hello kod och se till att endast serialiserbara objekt används för meddelandeinnehållet hello (eller använda en anpassad serialisering). Dokumentationen hello hello stöds värdetyper hello egenskaper och endast använda stöds typer. Kontrollera hello [IsTransient](/dotnet/api/microsoft.servicebus.messaging.messagingexception#Microsoft_ServiceBus_Messaging_MessagingException_IsTransient) egenskapen. Om det är **SANT**, du kan försöka hello igen. |Försök igen är odefinierad och inte kan hjälpa. |
| [MessagingEntityAlreadyExistsException](/dotnet/api/microsoft.servicebus.messaging.messagingentityalreadyexistsexception) |Försök toocreate en entitet med ett namn som redan används av en annan entitet i det namnområdet för tjänsten. |Ta bort befintliga hello-entitet eller välj ett annat namn för hello entiteten toobe skapas. |Det hjälper inte försök igen. |
| [QuotaExceededException](/dotnet/api/microsoft.servicebus.messaging.quotaexceededexception) |hello messaging entiteten har nått maximal tillåten storlek eller hello maximalt antal anslutningar tooa namnområde har överskridits. |Frigör utrymme i hello entiteten genom att ta emot meddelanden från hello entiteten eller dess underordnade köer. Se [QuotaExceededException](#quotaexceededexception). |Försök igen kan hjälpa om meddelanden har tagits bort i hello tiden. |
| [RuleActionException](/dotnet/api/microsoft.servicebus.messaging.ruleactionexception) |Service Bus returnerar det här undantaget om du försöker toocreate en ogiltig regel-åtgärd. Service Bus bifogar detta undantag tooa deadlettered meddelande om ett fel uppstår under bearbetning av hello Regelåtgärd för meddelandet. |Kontrollera hello Regelåtgärden är korrekt. |Det hjälper inte försök igen. |
| [FilterException](/dotnet/api/microsoft.servicebus.messaging.filterexception) |Service Bus returnerar det här undantaget om du försöker toocreate ett ogiltigt filter. Service Bus bifogar detta undantag tooa deadlettered meddelande om ett fel uppstod under bearbetning av hello filter för meddelandet. |Kontrollera hello filter är korrekt. |Det hjälper inte försök igen. |
| [SessionCannotBeLockedException](/dotnet/api/microsoft.servicebus.messaging.sessioncannotbelockedexception) |Försök tooaccept en session med en specifik sessions-ID, men hello sessionen är låst av en annan klient. |Kontrollera att hello sessionen låses upp av andra klienter. |Försök igen kan hjälpa om hello session har frigjorts i hello tillfälligt. |
| [TransactionSizeExceededException](/dotnet/api/microsoft.servicebus.messaging.transactionsizeexceededexception) |För många åtgärder är en del av hello transaktion. |Minska hello antal åtgärder som ingår i den här transaktionen. |Det hjälper inte försök igen. |
| [MessagingEntityDisabledException](/dotnet/api/microsoft.servicebus.messaging.messagingentitydisabledexception) |Begäran om en körningsåtgärd på en inaktiverad entitet. |Aktivera hello entitet. |Försök igen kan hjälpa om hello entiteten har aktiverats i hello tillfälligt. |
| [NoMatchingSubscriptionException](/dotnet/api/microsoft.servicebus.messaging.nomatchingsubscriptionexception) |Service Bus returnerar det här undantaget om du skickar ett meddelande tooa ämne som har före filtrering aktiverad och inget hello filter matchar. |Kontrollera att minst ett filter matchar. |Det hjälper inte försök igen. |
| [MessageSizeExceededException](/dotnet/api/microsoft.servicebus.messaging.messagesizeexceededexception) |Nyttolast för ett meddelande överskrider hello 256 KB-gränsen. Observera att hello 256 KB-gränsen är hello totala meddelandestorlek, vilket kan innefatta Systemegenskaper och eventuella kostnader för .NET. |Minska storleken på meddelandets innehåll hello hello storlek och försök hello igen. |Det hjälper inte försök igen. |
| [TransactionException](https://msdn.microsoft.com/library/system.transactions.transactionexception.aspx) |Hej omgivande transaktion (*Transaction.Current*) är ogiltig. Det har slutförts eller avbrutits. I det interna undantaget kan innehålla ytterligare information. | |Det hjälper inte försök igen. |
| [TransactionInDoubtException](https://msdn.microsoft.com/library/system.transactions.transactionindoubtexception.aspx) |En åtgärd utförs på en transaktion som är tveksam, eller ett försök görs toocommit hello transaktion och hello transaktion blir osäkra. |Programmet måste hantera detta undantag (som ett specialfall) som hello transaktion kanske redan har utförts. |- |

## <a name="quotaexceededexception"></a>QuotaExceededException
[QuotaExceededException](/dotnet/api/microsoft.servicebus.messaging.quotaexceededexception) anger en kvot för en viss enhet har överskridits.

### <a name="queues-and-topics"></a>Köer och ämnen
För köer och ämnen är det ofta hello storleken på hello kön. hello fel meddelandeegenskap innehåller ytterligare information, som i följande exempel hello:

```
Microsoft.ServiceBus.Messaging.QuotaExceededException
Message: hello maximum entity size has been reached or exceeded for Topic: ‘xxx-xxx-xxx’. 
    Size of entity in bytes:1073742326, Max entity size in bytes:
1073741824..TrackingId:xxxxxxxxxxxxxxxxxxxxxxxxxx, TimeStamp:3/15/2013 7:50:18 AM
```

hello-meddelande om avsnittet hello överskrider storleksgränsen i det här fallet 1 GB (hello Standardstorleksgränsen). 

### <a name="namespaces"></a>namnområden

För namnområden, [QuotaExceededException](/dotnet/api/microsoft.servicebus.messaging.quotaexceededexception) kan det tyda på att ett program har överskridit hello maximalt antal anslutningar tooa namnområde. Exempel:

```
Microsoft.ServiceBus.Messaging.QuotaExceededException: ConnectionsQuotaExceeded for namespace xxx.
<tracking-id-guid>_G12 ---> 
System.ServiceModel.FaultException`1[System.ServiceModel.ExceptionDetail]: 
ConnectionsQuotaExceeded for namespace xxx.
```

#### <a name="common-causes"></a>Vanliga orsaker
Det finns två vanliga orsaker till detta fel: hello kö med olevererade brev och icke-fungerande meddelandet mottagare.

1. **Kö med olevererade brev** en läsare misslyckas toocomplete meddelanden och hälsningsmeddelande returneras toohello kö/avsnittet hello Lås upphör att gälla. Detta kan inträffa om hello reader påträffar ett undantag som förhindrar anropar [BrokeredMessage.Complete](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.complete.aspx). När ett meddelande har lästs 10 gånger flyttas toohello obeställbara meddelanden som standard. Det här beteendet styrs av hello [QueueDescription.MaxDeliveryCount](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.queuedescription.maxdeliverycount.aspx) egenskap och har standardvärdet 10. När meddelanden växer i hello kö med olevererade brev, ta plats.
   
    tooresolve hello problemet skrivskyddade och fullständig hälsningsmeddelande från hello obeställbara meddelanden som du skulle från andra kö. Hej [QueueClient](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.queueclient.aspx) klassen innehåller även en [FormatDeadLetterPath](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.queueclient.formatdeadletterpath.aspx) metoden toohelp format hello kö med olevererade brev sökväg.
2. **Mottagaren stoppats** en mottagare har slutat att ta emot meddelanden från en kö eller prenumeration. Hej sätt tooidentify är toolook på hello [QueueDescription.MessageCountDetails](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.queuedescription.messagecountdetails.aspx) -egenskap som visar hello fullständig uppdelning av hälsningsmeddelande. Om hello [ActiveMessageCount](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.messagecountdetails.activemessagecount.aspx) egenskapen är hög eller växande sedan hälsningsmeddelande läses inte lika snabbt som de är skrivs.

### <a name="event-hubs"></a>Händelsehubbar
Händelsehubbar är begränsad till 20 konsumentgrupper per Event Hub. När du försöker toocreate mer får du en [QuotaExceededException](/dotnet/api/microsoft.servicebus.messaging.quotaexceededexception). 

## <a name="timeoutexception"></a>TimeoutException
En [TimeoutException](https://msdn.microsoft.com/library/system.timeoutexception.aspx) anger att en användarinitierad åtgärden tar längre tid än hello igen. 

Du bör kontrollera hello värdet för hello [ServicePointManager.DefaultConnectionLimit](https://msdn.microsoft.com/library/system.net.servicepointmanager.defaultconnectionlimit) egenskap som träffa den här gränsen kan också orsakas av en [TimeoutException](https://msdn.microsoft.com/library/system.timeoutexception.aspx).

### <a name="queues-and-topics"></a>Köer och ämnen
Hello timeout anges för köer och ämnen, antingen i hello [MessagingFactorySettings.OperationTimeout](/dotnet/api/microsoft.servicebus.messaging.messagingfactorysettings#Microsoft_ServiceBus_Messaging_MessagingFactorySettings_OperationTimeout) egenskapen som en del av hello anslutningssträngen eller via [ServiceBusConnectionStringBuilder](/dotnet/api/microsoft.servicebus.servicebusconnectionstringbuilder). hello felmeddelande själva kan variera, men innehåller alltid hello timeout-värde som angetts för hello den aktuella åtgärden. 

### <a name="event-hubs"></a>Händelsehubbar
För Event Hubs hello timeout anges antingen som en del av hello anslutningssträngen eller via [ServiceBusConnectionStringBuilder](/dotnet/api/microsoft.servicebus.servicebusconnectionstringbuilder). hello felmeddelande själva kan variera, men innehåller alltid hello timeout-värde som angetts för hello den aktuella åtgärden. 

### <a name="common-causes"></a>Vanliga orsaker
Det finns två vanliga orsaker till detta fel: felaktig konfiguration eller ett tillfälligt tjänstfel.

1. **Felaktig konfiguration** hello åtgärden timeout kanske för liten för hello driftsvillkor. hello standardvärdet för hello åtgärden timeout i hello klient-SDK är 60 sekunder. Kontrollera toosee om koden har hello värde angetts toosomething är för liten. Observera hello villkoret för hello nätverket och CPU-användning kan påverka hello tid det tar för en viss åtgärd toocomplete så hello åtgärden tidsgränsen inte ska ställas in tooa mycket lågt värde.
2. **Tillfälligt tjänstfel** ibland hello Service Bus-tjänst kan uppstå fördröjningar i bearbetning av begäranden, till exempel under perioder med hög trafik. I sådana fall kan du försöka utföra åtgärden efter en stund tills hello åtgärden lyckas. Om hello samma åtgärden fortfarande misslyckas efter flera försök, går du till hello [plats i Azure service status](https://azure.microsoft.com/status/) toosee om det inte finns några kända serviceavbrott.

## <a name="next-steps"></a>Nästa steg

Hello fullständiga Service Bus .NET-API-referens finns hello [Azure .NET API-referens för](/dotnet/api/overview/azure/servicebus).

Mer information om toolearn [Service Bus](https://azure.microsoft.com/services/service-bus/), se följande avsnitt hello.

* [Översikt över Service Bus-meddelandetjänster](service-bus-messaging-overview.md)
* [Service Bus-grunder](service-bus-fundamentals-hybrid-solutions.md)
* [Service Bus-arkitektur](service-bus-architecture.md)

