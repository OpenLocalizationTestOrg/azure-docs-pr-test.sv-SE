---
title: aaaAzure Relay undantag och hur tooresolve dem | Microsoft Docs
description: "Hämta en lista över Azure Relay undantag och föreslagna åtgärder du kan vidta toohelp lösas."
services: service-bus-relay
documentationcenter: na
author: sethmanheim
manager: timlt
editor: 
ms.assetid: 5f9dd02c-cce0-43b3-8eb8-744f0c27f38c
ms.service: service-bus-relay
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/23/2017
ms.author: sethm
ms.openlocfilehash: de417c8e9e43407ef355fd44f6170cf2cdc46d6d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-relay-exceptions"></a>Azure Relay-undantag

Den här artikeln innehåller vissa undantag som genereras av hello Azure Relay API: er. Den här referensen är ämne toochange, så kolla igen efter uppdateringar.

## <a name="exception-categories"></a>Undantag kategorier

hello Relay-API: er skapa undantag som kan delas in i följande kategorier hello. Listan finns även föreslagna åtgärder som att du kan vidta toohelp Lös hello undantag.

*   **Användaren kodningsfel**: [System.ArgumentException](https://msdn.microsoft.com/library/system.argumentexception.aspx), [System.InvalidOperationException](https://msdn.microsoft.com/library/system.invalidoperationexception.aspx), [System.OperationCanceledException](https://msdn.microsoft.com/library/system.operationcanceledexception.aspx), [ System.Runtime.Serialization.SerializationException](https://msdn.microsoft.com/library/system.runtime.serialization.serializationexception.aspx). 

    **Allmän åtgärd**: försök toofix hello kod innan du fortsätter.
*   **Installationen/konfigurationsfel**: [System.UnauthorizedAccessException](https://msdn.microsoft.com/library/system.unauthorizedaccessexception.aspx). 

    **Allmän åtgärd**: Granska din konfiguration. Om det behövs ändrar du hello konfiguration.
*   **Tillfälligt undantag**: [Microsoft.ServiceBus.Messaging.MessagingException](/dotnet/api/microsoft.servicebus.messaging.messagingexception), [Microsoft.ServiceBus.Messaging.ServerBusyException](/dotnet/api/microsoft.servicebus.messaging.serverbusyexception), [ Microsoft.ServiceBus.Messaging.MessagingCommunicationException](/dotnet/api/microsoft.servicebus.messaging.messagingcommunicationexception). 

    **Allmän åtgärd**: gör om åtgärden hello eller meddela användare.
*   **Andra undantag**: [System.Transactions.TransactionException](https://msdn.microsoft.com/library/system.transactions.transactionexception.aspx), [System.TimeoutException](https://msdn.microsoft.com/library/system.timeoutexception.aspx). 

    **Allmän åtgärd**: specifika toohello undantagstyp. Se tabellen hello i hello efter avsnittet. 

## <a name="exception-types"></a>Typer av undantag

hello följande tabell visas meddelanden undantag typer och deras orsaker. Den visas också föreslagna åtgärder du kan vidta toohelp Lös hello undantag.

| **Undantagstyp** | **Beskrivning** | **Föreslagen åtgärd** | **Tänk på automatisk eller omedelbara försök igen** |
| --- | --- | --- | --- |
| [Timeout](https://msdn.microsoft.com/library/system.timeoutexception.aspx) |hello servern svarade inte toohello begärde åtgärden inom hello anges tiden som styrs av [OperationTimeout](/dotnet/api/microsoft.servicebus.messaging.messagingfactorysettings.operationtimeout). Hej server kan ha slutfört hello den begärda åtgärden. Detta kan inträffa på grund av toonetwork eller andra infrastruktur fördröjningar. |Kontrollera hello systemtillstånd för konsekvens och sedan försöka igen om det behövs. Se [TimeoutException](#timeoutexception). |Försök igen kan i vissa fall. Lägg till nytt försök logik toocode. |
| [Ogiltig åtgärd](https://msdn.microsoft.com/library/system.invalidoperationexception.aspx) |hello begärt användaren åtgärden inte tillåts i hello server eller tjänst. Se hello Undantagsmeddelande mer information. |Kontrollera hello koden och hello-dokumentationen. Kontrollera att hello begärda åtgärden är ogiltig. |Det hjälper inte försök igen. |
| [Åtgärden avbröts](https://msdn.microsoft.com/library/system.operationcanceledexception.aspx) |Ett försök har gjorts tooinvoke en åtgärd på ett objekt som har redan stängts, avbröts eller tagits bort. I sällsynta fall hello omgivande transaktion har redan tagits bort. |Kontrollera hello kod och kontrollera att den inte anropa åtgärder på ett borttaget objekt. |Det hjälper inte försök igen. |
| [Obehörig åtkomst](https://msdn.microsoft.com/library/system.unauthorizedaccessexception.aspx) |Hej [TokenProvider](/dotnet/api/microsoft.servicebus.tokenprovider) objektet kunde inte hämta en token, hello token är ogiltig eller hello token innehåller inte hello anspråk krävs tooperform hello igen. |Se till att hello tokenleverantör har skapats med hello rätt värden. Kontrollera hello konfigurationen för hello Access Control service. |Försök igen kan i vissa fall. Lägg till nytt försök logik toocode. |
| [Argumentet undantag](https://msdn.microsoft.com/library/system.argumentexception.aspx),<br /> [Argumentet är Null](https://msdn.microsoft.com/library/system.argumentnullexception.aspx),<br />[Argumentet utanför intervallet](https://msdn.microsoft.com/library/system.argumentoutofrangeexception.aspx) |En eller flera av hello följande har inträffat:<br />En eller flera argument angavs toohello metoden är ogiltiga.<br /> hello URI som levererats för[NamespaceManager](/dotnet/api/microsoft.servicebus.namespacemanager) eller [skapa](/dotnet/api/microsoft.servicebus.messaging.messagingfactory.create) innehåller en eller flera sökvägssegment.<br />hello URI-schema som angetts för[NamespaceManager](/dotnet/api/microsoft.servicebus.namespacemanager) eller [skapa](/dotnet/api/microsoft.servicebus.messaging.messagingfactory.create) är ogiltig. <br />hello egenskapens värde är större än 32 KB. |Kontrollera hello anropande koden och kontrollera hello argumenten är korrekta. |Det hjälper inte försök igen. |
| [Servern är upptagen](/dotnet/api/microsoft.servicebus.messaging.serverbusyexception) |Tjänsten är inte kan tooprocess hello begäran just nu. |hello-klienten kan vänta på en viss tidsperiod och försök hello igen. |hello-klienten kan försöka igen efter en viss tid. Om en försök resulterar i en annan undantag Kontrollera hello försök beteendet för undantaget. |
| [Kvoten överskreds](/dotnet/api/microsoft.servicebus.messaging.quotaexceededexception) |hello messaging entiteten har uppnått den maximala storleken. |Frigör utrymme i hello entiteten genom att ta emot meddelanden från hello entiteten eller dess underköer. Se [QuotaExceededException](#quotaexceededexception). |Försök igen kan hjälpa om meddelanden har tagits bort i hello tiden. |
| [Meddelandestorlek överskrider](/dotnet/api/microsoft.servicebus.messaging.messagesizeexceededexception) |Nyttolast för ett meddelande överskrider hello 256 KB-gränsen. Observera att hello 256 KB-gränsen är hello totala meddelandestorlek. hello totala meddelandestorlek kan innehålla Systemegenskaper och eventuella kostnader för Microsoft .NET. |Minska storleken på meddelandets innehåll hello hello storlek och försök hello igen. |Det hjälper inte försök igen. |

## <a name="quotaexceededexception"></a>QuotaExceededException

[QuotaExceededException](/dotnet/api/microsoft.servicebus.messaging.quotaexceededexception) anger en kvot för en viss enhet har överskridits.

För relä, undantaget radbryts hello [System.ServiceModel.QuotaExceededException](https://msdn.microsoft.com/library/system.servicemodel.quotaexceededexception.aspx), vilket anger att hello maxantalet lyssnare har överskridits för den här slutpunkten. Detta visas i hello **MaximumListenersPerEndpoint** värdet för hello Undantagsmeddelande.

## <a name="timeoutexception"></a>TimeoutException
En [TimeoutException](https://msdn.microsoft.com/library/system.timeoutexception.aspx) anger att en användarinitierad åtgärden tar längre tid än hello igen. 

Kontrollera hello värdet av hello [ServicePointManager.DefaultConnectionLimit](https://msdn.microsoft.com/library/system.net.servicepointmanager.defaultconnectionlimit) egenskapen. Träffa den här gränsen kan även orsaka en [TimeoutException](https://msdn.microsoft.com/library/system.timeoutexception.aspx).

För Relay, kan du få tidsgränsfel första gången du öppnar en avsändare relay-anslutning. Det finns två vanliga orsaker till detta undantag:

*   Hej [OpenTimeout](https://msdn.microsoft.com/library/wcf.opentimeout.aspx) värde kanske för liten (om jämnt genom att en del av en sekund).
* En lokal relay lyssnare kanske inte svarar (eller brandväggen regler problem som förhindrar att lyssnare godtar nya klientanslutningar kan uppstå) och hello [OpenTimeout](https://msdn.microsoft.com/library/wcf.opentimeout.aspx) värdet är mindre än 20 sekunder.

Exempel:

```
'System.TimeoutException’: hello operation did not complete within hello allotted timeout of 00:00:10.
hello time allotted toothis operation may have been a portion of a longer timeout.
```

### <a name="common-causes"></a>Vanliga orsaker
Det finns två vanliga orsaker till det här felet:

*   **Felaktig konfiguration**
    
    Tidsgräns för hello åtgärd kanske för liten för hello driftsvillkor. hello standardvärdet för hello åtgärden timeout i hello klient-SDK är 60 sekunder. Kontrollera toosee om hello i koden värdet toosomething för liten. Observera att CPU användnings- och hello villkoret hello nätverket kan påverka hello tid det tar för en åtgärd toocomplete. Det är en bra idé inte tooset hello åtgärden tooa mycket små tidsgränsvärde.
*   **Tillfälligt tjänstfel**

    Hello vidarebefordrande tjänsten kan ibland uppstå fördröjningar i bearbetning av begäranden. Detta kan inträffa, till exempel under perioder med hög trafik. Om detta inträffar, försök igen efter en stund tills hello åtgärden lyckas. Om hello samma åtgärd kvarstår toofail efter flera försök Kontrollera hello [plats i Azure service status](https://azure.microsoft.com/status/) toosee om det finns kända tjänstavbrott.

## <a name="next-steps"></a>Nästa steg
* [Azure Relay vanliga frågor och svar](relay-faq.md)
* [Skapa en relay-namnområde](relay-create-namespace-portal.md)
* [Kom igång med Azure Relay och .NET](relay-hybrid-connections-dotnet-get-started.md)
* [Kom igång med Azure Relay och nod](relay-hybrid-connections-node-get-started.md)

