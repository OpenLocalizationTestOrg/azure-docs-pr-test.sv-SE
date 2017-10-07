---
title: "aaaAzure Händelsehubbar messaging undantag | Microsoft Docs"
description: "Lista med Händelsehubbar i Azure messaging-undantag och föreslagna åtgärder."
services: event-hubs
documentationcenter: na
author: sethmanheim
manager: timlt
editor: 
ms.assetid: 2c6273de-0106-47e5-b45d-59040e51f2c5
ms.service: event-hubs
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/15/2017
ms.author: sethm
ms.openlocfilehash: 9c164e76612c26607219f08407f689aaab4050a5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="event-hubs-messaging-exceptions"></a>Event Hubs – undantag för meddelanden
Den här artikeln innehåller några av hello-undantag som genereras av hello Azure Service Bus messaging API: er, bland annat Händelsehubbar. Den här referensen är ämne toochange, så kolla igen efter uppdateringar.

## <a name="exception-categories"></a>Undantag kategorier
hello Event Hubs API: er skapa undantag som kan delas in i följande kategorier hello, tillsammans med hello associerade åtgärder du kan vidta tootry toofix dem.

1. Användaren kodningsfel: [System.ArgumentException](https://msdn.microsoft.com/library/system.argumentexception.aspx), [System.InvalidOperationException](https://msdn.microsoft.com/library/system.invalidoperationexception.aspx), [System.OperationCanceledException](https://msdn.microsoft.com/library/system.operationcanceledexception.aspx), [System.Runtime.Serialization.SerializationException](https://msdn.microsoft.com/library/system.runtime.serialization.serializationexception.aspx). Allmän åtgärd: försök toofix hello kod innan du fortsätter.
2. Installationen/konfigurationsfel: [Microsoft.ServiceBus.Messaging.MessagingEntityNotFoundException](/dotnet/api/microsoft.servicebus.messaging.messagingentitynotfoundexception), [Microsoft.Azure.EventHubs.MessagingEntityNotFoundException](/dotnet/api/microsoft.azure.eventhubs.messagingentitynotfoundexception), [System.UnauthorizedAccessException](https://msdn.microsoft.com/library/system.unauthorizedaccessexception.aspx). Allmän åtgärd: Granska konfigurationen och ändra vid behov.
3. Tillfälligt undantag: [Microsoft.ServiceBus.Messaging.MessagingException](/dotnet/api/microsoft.servicebus.messaging.messagingexception), [Microsoft.ServiceBus.Messaging.ServerBusyException](#serverbusyexception), [Microsoft.Azure.EventHubs.ServerBusyException](#serverbusyexception), [Microsoft.ServiceBus.Messaging.MessagingCommunicationException](/dotnet/api/microsoft.servicebus.messaging.messagingcommunicationexception). Allmän åtgärd: gör om åtgärden hello eller meddela användare.
4. Andra undantag: [System.Transactions.TransactionException](https://msdn.microsoft.com/library/system.transactions.transactionexception.aspx), [System.TimeoutException](#timeoutexception), [Microsoft.ServiceBus.Messaging.MessageLockLostException](/dotnet/api/microsoft.servicebus.messaging.messagelocklostexception), [Microsoft.ServiceBus.Messaging.SessionLockLostException](/dotnet/api/microsoft.servicebus.messaging.sessionlocklostexception). Allmän åtgärd: specifika toohello undantagstyp; Se tabellen toohello i hello efter avsnittet. 

## <a name="exception-types"></a>Typer av undantag
hello följande tabell visas meddelanden undantag typer och orsaker och anteckningar föreslagna åtgärder du kan vidta.

| **Undantagstyp** | **Exempel-beskrivning/orsak** | **Föreslagen åtgärd** | **Tänk på automatisk omedelbara försök** |
| --- | --- | --- | --- |
| [TimeoutException](https://msdn.microsoft.com/library/system.timeoutexception.aspx) |hello servern svarade inte toohello begärde åtgärden inom hello anges tiden som styrs av [OperationTimeout](/dotnet/api/microsoft.servicebus.messaging.messagingfactorysettings#Microsoft_ServiceBus_Messaging_MessagingFactorySettings_OperationTimeout). hello-server kan ha avslutat hello den begärda åtgärden. Detta kan inträffa på grund av toonetwork eller andra infrastruktur fördröjningar. |Hello systemtillstånd för konsekvens och försök igen om det behövs.<br /> Se [TimeoutException](#timeoutexception). |Försök igen kan i vissa fall. Lägg till nytt försök logik toocode. |
| [InvalidOperationException](https://msdn.microsoft.com/library/system.invalidoperationexception.aspx) |hello begärt användaren åtgärden inte tillåts i hello server eller tjänst. Se hello Undantagsmeddelande mer information. Till exempel [Slutför](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage#Microsoft_ServiceBus_Messaging_BrokeredMessage_Complete) genererar det här undantaget om hello-meddelande togs emot i [ReceiveAndDelete](/dotnet/api/microsoft.servicebus.messaging.receivemode) läge. |Kontrollera hello koden och hello-dokumentationen. Kontrollera att hello begärda åtgärden är ogiltig. |Det hjälper inte försök igen. |
| [OperationCanceledException](https://msdn.microsoft.com/library/system.operationcanceledexception.aspx) |Ett försök har gjorts tooinvoke en åtgärd på ett objekt som redan har stängts, avbröts eller tagits bort. I sällsynta fall hello omgivande transaktion har redan tagits bort. |Kontrollera hello kod och kontrollera att den inte anropa åtgärder på ett borttaget objekt. |Det hjälper inte försök igen. |
| [UnauthorizedAccessException](https://msdn.microsoft.com/library/system.unauthorizedaccessexception.aspx) |Hej [TokenProvider](/dotnet/api/microsoft.servicebus.tokenprovider) objektet kunde inte hämta en token, hello token är ogiltig eller hello token innehåller inte hello anspråk krävs tooperform hello igen. |Kontrollera att hello tokenleverantör skapas med hello rätt värden. Kontrollera hello konfigurationen för hello Access Control service. |Försök igen kan i vissa fall. Lägg till nytt försök logik toocode. |
| [ArgumentException](https://msdn.microsoft.com/library/system.argumentexception.aspx)<br /> [ArgumentNullException](https://msdn.microsoft.com/library/system.argumentnullexception.aspx)<br />[ArgumentOutOfRangeException](https://msdn.microsoft.com/library/system.argumentoutofrangeexception.aspx) |En eller flera argument angavs toohello metoden är ogiltiga. hello URI som levererats för[NamespaceManager](/dotnet/api/microsoft.servicebus.namespacemanager) eller [skapa](/dotnet/api/microsoft.servicebus.messaging.messagingfactory#Microsoft_ServiceBus_Messaging_MessagingFactory_Create_System_Collections_Generic_IEnumerable_System_Uri__) innehåller sökvägen segment. hello URI-schema som angetts för[NamespaceManager](/dotnet/api/microsoft.servicebus.namespacemanager) eller [skapa](/dotnet/api/microsoft.servicebus.messaging.messagingfactory#Microsoft_ServiceBus_Messaging_MessagingFactory_Create_System_Collections_Generic_IEnumerable_System_Uri__) är ogiltig. hello egenskapens värde är större än 32KB. |Kontrollera hello anropande koden och kontrollera hello argumenten är korrekta. |Det hjälper inte försök igen. |
| [Microsoft.ServiceBus.Messaging.MessagingEntityNotFoundException](/dotnet/api/microsoft.servicebus.messaging.messagingentitynotfoundexception) <br /> [Microsoft.Azure.EventHubs.MessagingEntityNotFoundException](/dotnet/api/microsoft.azure.eventhubs.messagingentitynotfoundexception) |Entitet som är associerad med hello åtgärden finns inte eller har tagits bort. |Kontrollera att hello entiteten finns. |Det hjälper inte försök igen. |
| [MessageNotFoundException](/dotnet/api/microsoft.servicebus.messaging.messagenotfoundexception) |Försök tooreceive ett meddelande med ett visst sekvensnummer. Det här meddelandet hittades inte. |Kontrollera att hello-meddelande har tagits emot redan. Kontrollera hello systemkön kön toosee om hello-meddelande har deadlettered. |Det hjälper inte försök igen. |
| [MessagingCommunicationException](/dotnet/api/microsoft.servicebus.messaging.messagingcommunicationexception) |Klienten är inte kan tooestablish en anslutning tooEvent hubb. |Kontrollera att hello angivna värdnamnet är rätt och hello värden kan nås. |Försök igen kan hjälpa om återkommande anslutningsproblem. |
| [Microsoft.ServiceBus.Messaging.ServerBusyException](/dotnet/api/microsoft.servicebus.messaging.serverbusyexception) <br /> [Microsoft.Azure.EventHubs.ServerBusyException](/dotnet/api/microsoft.azure.eventhubs.serverbusyexception) |Tjänsten är inte kan tooprocess hello begäran just nu. |Klienten kan vänta på en viss tidsperiod och försök hello igen. <br /> Se [ServerBusyException](#serverbusyexception). |Klienten kan försöka igen efter vissa intervall. Om ett nytt försök resulterar i en annan undantag, kontrollera försök beteendet för undantaget. |
| [MessageLockLostException](/dotnet/api/microsoft.servicebus.messaging.messagelocklostexception) |Lås token som är associerade med hello-meddelande har upphört att gälla eller hello Lås token finns inte. |Ta bort hello-meddelande. |Det hjälper inte försök igen. |
| [SessionLockLostException](/dotnet/api/microsoft.servicebus.messaging.sessionlocklostexception) |Lås som är associerade med den här sessionen går förlorad. | Avbryt hello [MessageSession](/dotnet/api/microsoft.servicebus.messaging.messagesession) objekt. |Det hjälper inte försök igen. |
| [MessagingException](/dotnet/api/microsoft.servicebus.messaging.messagingexception) |Generisk messaging undantag som utlöses i följande fall hello: ett försök görs toocreate en [QueueClient](/dotnet/api/microsoft.servicebus.messaging.queueclient) med ett namn eller en sökväg som tillhör tooa olika entitetstypen (till exempel ett ämne). Ett försök har gjorts toosend meddelandet större än 256KB. hello-server eller tjänsten påträffade ett fel under bearbetningen av hello begäran. Hello Undantagsmeddelande mer information finns i. Detta är vanligtvis ett tillfälligt undantag. |Kontrollera hello kod och se till att endast serialiserbara objekt används för meddelandeinnehållet hello (eller använda en anpassad serialisering). Dokumentationen hello hello stöds värdetyper hello egenskaper och endast använda stöds typer. Kontrollera hello [IsTransient](/dotnet/api/microsoft.servicebus.messaging.messagingexception#Microsoft_ServiceBus_Messaging_MessagingException_IsTransient) egenskapen. Om det är **SANT**, du kan försöka hello igen. |Försök igen är odefinierad och inte kan hjälpa. |
| [MessagingEntityAlreadyExistsException](/dotnet/api/microsoft.servicebus.messaging.messagingentityalreadyexistsexception) |Försök toocreate en entitet med ett namn som redan används av en annan entitet i det namnområdet för tjänsten. |Ta bort befintliga hello-entitet eller välj ett annat namn för hello entiteten toobe skapas. |Det hjälper inte försök igen. |
| [QuotaExceededException](/dotnet/api/microsoft.servicebus.messaging.quotaexceededexception) |hello messaging entiteten har uppnått den maximala storleken. Detta kan inträffa om hello maximalt antal mottagare (som är 5) har redan öppnats i en per konsumenten gruppnivå. |Frigör utrymme i hello entiteten genom att ta emot meddelanden från hello entiteten eller dess underordnade köer. <br /> Se [QuotaExceededException](#quotaexceededexception) |Försök igen kan hjälpa om meddelanden har tagits bort i hello tiden. |
|  | | | |
| [SessionCannotBeLockedException](/dotnet/api/microsoft.servicebus.messaging.sessioncannotbelockedexception) |Försök tooaccept en session med en specifik sessions-ID, men hello sessionen är låst av en annan klient. |Kontrollera att hello sessionen låses upp av andra klienter. |Försök igen kan hjälpa om hello session har frigjorts i hello tillfälligt. |
| [TransactionSizeExceededException](/dotnet/api/microsoft.servicebus.messaging.transactionsizeexceededexception) |För många åtgärder är en del av hello transaktion. |Minska hello antal åtgärder som ingår i den här transaktionen. |Det hjälper inte försök igen. |
| [MessagingEntityDisabledException](/dotnet/api/microsoft.servicebus.messaging.messagingentitydisabledexception) |Begäran om en körningsåtgärd på en inaktiverad entitet. |Aktivera hello entitet. |Försök igen kan hjälpa om hello entiteten har aktiverats i hello tillfälligt. |
| [Microsoft.ServiceBus.Messaging.MessageSizeExceededException](/dotnet/api/microsoft.servicebus.messaging.messagesizeexceededexception) <br /> [Microsoft.Azure.EventHubs.MessageSizeExceededException](/dotnet/api/microsoft.azure.eventhubs.messagesizeexceededexception) | Nyttolast för ett meddelande överskrider hello 256 kB-gränsen. Observera att hello 256 kB-gränsen är hello totala meddelandestorlek, vilket kan innefatta Systemegenskaper och eventuella kostnader för .NET. |Minska storleken på meddelandets innehåll hello hello storlek och försök hello igen. |Det hjälper inte försök igen. |
| [TransactionException](https://msdn.microsoft.com/library/system.transactions.transactionexception.aspx) |Hej omgivande transaktion (*Transaction.Current*) är ogiltig. Det har slutförts eller avbrutits. I det interna undantaget kan innehålla ytterligare information. | |Det hjälper inte försök igen. |
| [TransactionInDoubtException](https://msdn.microsoft.com/library/system.transactions.transactionindoubtexception.aspx) |En åtgärd utförs på en transaktion som är tveksam, eller ett försök görs toocommit hello transaktion och hello transaktion blir osäkra. |Programmet måste hantera detta undantag (som ett specialfall) som hello transaktion kanske redan har utförts. |- |

## <a name="quotaexceededexception"></a>QuotaExceededException
[QuotaExceededException](/dotnet/api/microsoft.servicebus.messaging.quotaexceededexception) anger en kvot för en viss enhet har överskridits.

Detta kan inträffa om hello maximalt antal mottagare (5) har redan öppnats i en per konsumenten gruppnivå.

### <a name="event-hubs"></a>Händelsehubbar
Händelsehubbar är begränsad till 20 konsumentgrupper per Event Hub. När du försöker toocreate mer får du en [QuotaExceededException](/dotnet/api/microsoft.servicebus.messaging.quotaexceededexception). 

## <a name="timeoutexception"></a>TimeoutException
En [TimeoutException](https://msdn.microsoft.com/library/system.timeoutexception.aspx) anger att en användarinitierad åtgärden tar längre tid än hello igen. 

För Event Hubs hello timeout anges antingen som en del av hello anslutningssträngen eller via [ServiceBusConnectionStringBuilder](/dotnet/api/microsoft.servicebus.servicebusconnectionstringbuilder). hello felmeddelande själva kan variera, men innehåller alltid hello timeout-värde som angetts för hello den aktuella åtgärden. 

### <a name="common-causes"></a>Vanliga orsaker
Det finns två vanliga orsaker till detta fel: felaktig konfiguration eller ett tillfälligt tjänstfel.

1. **Felaktig konfiguration** hello åtgärden timeout kanske för liten för hello driftsvillkor. hello standardvärdet för hello åtgärden timeout i hello klient-SDK är 60 sekunder. Kontrollera toosee om koden har hello värde angetts toosomething är för liten. Observera hello villkoret för hello nätverket och CPU-användning kan påverka hello tid det tar för en viss åtgärd toocomplete så hello åtgärden tidsgränsen inte ska ställas in tooa mycket lågt värde.
2. **Tillfälligt tjänstfel** ibland hello händelsehubbtjänsten kan uppstå fördröjningar i bearbetning av begäranden, till exempel under perioder med hög trafik. I sådana fall kan du försöka utföra åtgärden efter en stund tills hello åtgärden lyckas. Om hello samma åtgärden fortfarande misslyckas efter flera försök, går du till hello [plats i Azure service status](https://azure.microsoft.com/status/) toosee om det inte finns några kända serviceavbrott.

## <a name="serverbusyexception"></a>ServerBusyException

En [Microsoft.ServiceBus.Messaging.ServerBusyException](/dotnet/api/microsoft.servicebus.messaging.serverbusyexception) eller [Microsoft.Azure.EventHubs.ServerBusyException](/dotnet/api/microsoft.azure.eventhubs.serverbusyexception) anger att en server är överbelastad. Det finns två relevanta felkoder för det här undantaget.

### <a name="error-code-50002"></a>Felkoden 50002

Det här felet kan bero på något av två skäl:

1. hello belastningen är inte jämnt fördelad över alla partitioner på hello Event Hub och en partition träffar hello lokala genomströmning enhet begränsning.
    
    Lösning: Ändra hello partition distributionsstrategi eller försöka [EventHubClient.Send(eventDataWithOutPartitionKey)](/dotnet/api/microsoft.servicebus.messaging.eventhubclient#Microsoft_ServiceBus_Messaging_EventHubClient_Send_Microsoft_ServiceBus_Messaging_EventData_) kan hjälpa.

2. Hej Händelsehubbar namnområdet har inte tillräcklig genomflödesenheter (du kan kontrollera hello **mått** blad i Händelsehubbar namnområde bladet i hello [Azure-portalen](https://portal.azure.com) tooconfirm). Observera att hello portal visar aggregerade (1 minut) information, men vi mäter hello genomflöde i realtid – så att det är endast en uppskattning.

    Lösning: Öka hello genomströmning enheter på hello namnområde kan hjälpa. Du kan göra detta på hello-portalen i hello **skala** bladet för hello Händelsehubbar namnområde bladet.

### <a name="error-code-50001"></a>Felkoden 50001

Det här felet ska sällan ske. Det händer när hello behållaren köra kod för namnområdet är låg på CPU – inte fler än några få sekunder innan hello Händelsehubbar läsa in belastningsutjämning börjar.


## <a name="next-steps"></a>Nästa steg
Mer information om Händelsehubbar genom att besöka hello följande länkar:

* [Event Hubs-översikt](event-hubs-what-is-event-hubs.md)
* [Skapa en händelsehubb](event-hubs-create.md)
* [Vanliga frågor och svar om Event Hubs](event-hubs-faq.md)
