---
title: aaaScheduler begrepp och entiteter | Microsoft Docs
description: "Begrepp, terminologi och entitetshierarki, inklusive jobb och jobbsamlingar, relaterade till Azure Scheduler.  Innehåller ett omfattande exempel på ett schemalagt jobb."
services: scheduler
documentationcenter: .NET
author: derek1ee
manager: kevinlam1
editor: 
ms.assetid: 3ef16fab-d18a-48ba-8e56-3f3e0a1bcb92
ms.service: scheduler
ms.workload: infrastructure-services
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: get-started-article
ms.date: 08/18/2016
ms.author: deli
ms.openlocfilehash: 73e7de7bfd2937e401aeab05e0e10fa292cf37b9
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="scheduler-concepts-terminology--entity-hierarchy"></a>Begrepp, terminologi och entitetshierarki relaterade till Azure Scheduler
## <a name="scheduler-entity-hierarchy"></a>Entitetshierarki i Scheduler
hello beskrivs följande tabell hello huvudsakliga resurser visas eller används av hello Scheduler API:

| Resurs | Beskrivning |
| --- | --- |
| **Jobbsamling** |En jobbsamling innehåller ett antal jobb och underhåller inställningar, kvoter och begränsningar som delas av jobb i hello samling. En jobbsamling skapas av en prenumerationsägare och grupperar jobb baserat på användnings- eller programgränser. Det är begränsad tooone region. Det gör även hello verkställandet av kvoter tooconstrain hello användning för alla jobb i samlingen. hello kvoter inkluderar MaxJobs och maxrecurrence-värdet. |
| **Jobb** |Ett jobb definierar en enskild återkommande åtgärd med enkla eller komplexa strategier för körning. Exempel på åtgärder är HTTP, lagringskö, Service Bus-kö eller Service Bus-ämnesförfrågningar. |
| **Jobbhistorik** |En jobbhistorik representerar information om körningen av ett jobb. Den innehåller information om huruvida jobbet lyckades eller misslyckades, samt eventuell svarsinformation. |

## <a name="scheduler-entity-management"></a>Entitetshantering i Scheduler
Exponera hello följande åtgärder på hello resurser på en hög nivå hello Schemaläggaren och hello service management API:

| Funktion | Beskrivning och URI-adress |
| --- | --- |
| **Hantering av jobbsamlingar** |Hämta, PLACERA och ta bort stöd för att skapa och ändra jobbsamlingar och jobb för hello som finns där. En jobbsamling är en behållare för jobb och mappa tooquotas och delade inställningar. Exempel på kvoter, som beskrivs senare, är högsta antal jobb och minsta intervall. <p>PUT och DELETE: `https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Scheduler/jobCollections/{jobCollectionName}`</p><p>GET: `https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Scheduler/jobCollections/{jobCollectionName}`</p> |
| **Jobbhantering** |Stöd för GET, PUT, POST, PATCH och DELETE för att skapa och ändra jobb. Alla jobb måste tillhöra tooa jobbsamlingen som redan finns, så det finns ingen implicit skapande. <p>`https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Scheduler/jobCollections/{jobCollectionName}/jobs/{jobName}`</p> |
| **Hantering av jobbhistorik** |Stöd för GET för att hämta 60 dagars jobbkörningshistorik, t.ex. förfluten tid och körningsresultat. Lägger till parameterstöd för frågesträngar för filtrering baserat på tillstånd och status. <P>`https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Scheduler/jobCollections/{jobCollectionName}/jobs/{jobName}/history`</p> |

## <a name="job-types"></a>Jobbtyper
Det finns flera typer av jobb: HTTP-jobb (inklusive HTTPS-jobb som stöder SSL), jobb för lagringsköer, Service Bus-köjobb och Service Bus-ämnesjobb. HTTP-jobben är idealiska om du har en slutpunkt för en befintlig arbetsbelastning eller tjänst. Du kan använda lagring kön jobb toopost meddelanden toostorage köer, så att dessa jobb är idealisk för arbetsbelastningar som använder lagringsköer. På liknande sätt är Service Bus-jobb idealiska för arbetsbelastningar som använder Service Bus-köer och Service Bus-ämnen.

## <a name="hello-job-entity-in-detail"></a>Hej ”jobbet” entitet i detalj
På en grundläggande nivå har ett schemalagt jobb flera delar:

* hello åtgärd tooperform när hello jobbet timer utlöses  
* (Valfritt) hello toorun hello jobbet  
* (Valfritt) När och hur ofta toorepeat hello jobb  
* (Valfritt) En åtgärd toofire om hello primär åtgärd misslyckas  

Internt, innehåller också ett schemalagt jobb systemdefinierade data, till exempel hello nästa schemalagda körning tid.

hello följande kod innehåller en omfattande exempel på ett schemalagt jobb. Information finns i de efterföljande avsnitten.

    {
        "startTime": "2012-08-04T00:00Z",               // optional
        "action":
        {
            "type": "http",
            "retryPolicy": { "retryType":"none" },
            "request":
            {
                "uri": "http://contoso.com/foo",        // required
                "method": "PUT",                        // required
                "body": "Posting from a timer",         // optional
                "headers":                              // optional

                {
                    "Content-Type": "application/json"
                },
            },
           "errorAction":
           {
               "type": "http",
               "request":
               {
                   "uri": "http://contoso.com/notifyError",
                   "method": "POST",
               },
           },
        },
        "recurrence":                                   // optional
        {
            "frequency": "week",                        // can be "year" "month" "day" "week" "minute"
            "interval": 1,                              // optional, how often toofire (default too1)
            "schedule":                                 // optional (advanced scheduling specifics)
            {
                "weekDays": ["monday", "wednesday", "friday"],
                "hours": [10, 22]
            },
            "count": 10,                                 // optional (default toorecur infinitely)
            "endTime": "2012-11-04",                     // optional (default toorecur infinitely)
        },
        "state": "disabled",                           // enabled or disabled
        "status":                                       // controlled by Scheduler service
        {
            "lastExecutionTime": "2007-03-01T13:00:00Z",
            "nextExecutionTime": "2007-03-01T14:00:00Z ",
            "executionCount": 3,
                                                "failureCount": 0,
                                                "faultedCount": 0
        },
    }

Som det visas i hello exempel schemalagt jobb ovan, har en jobbdefinition flera delar:

* Starttid (”startTime”)  
* Åtgärd (”action”), som även omfattar en felåtgärd (”errorAction”)
* Upprepning (”recurrence”)  
* Tillstånd (”state”)  
* Status (”status”)  
* Återförsöksprincip (”retryPolicy”)  

Nu ska vi titta närmare på var och en av dessa delar:

## <a name="starttime"></a>startTime
Hej ”startTime” är hello starttid och gör att hello anroparen toospecify en tidszon förskjutning hello uppkopplat läge i [ISO 8601-format](http://en.wikipedia.org/wiki/ISO_8601).

## <a name="action-and-erroraction"></a>action och errorAction
hello ”åtgärden” hello-åtgärd som anropas för varje förekomst och beskriver en typ av tjänst-anrop. hello-åtgärden är vad kommer att utföras på hello tillhandahålls schema. Scheduler stöder HTTP, lagringskö, Service Bus-ämne och Service Bus-köåtgärder.

hello är i hello-exemplet ovan en HTTP-åtgärd. Nedan är ett exempel på en åtgärd för lagringskön:

    {
            "type": "storageQueue",
            "queueMessage":
            {
                "storageAccount": "myStorageAccount",  // required
                "queueName": "myqueue",                // required
                "sasToken": "TOKEN",                   // required
                "message":                             // required
                    "My message body",
            },
    }

Nedan är ett exempel på en Service Bus-ämnesåtgärd.

  "action": { "type": "serviceBusTopic", "serviceBusTopicMessage": { "topicPath": "t1",  
      "namespace": "mySBNamespace", "transportType": "netMessaging", // Kan vara antingen netMessaging eller AMQP "authentication": { "sasKeyName": "QPolicy", "type": "sharedAccessKey" }, "message": "Some message", "brokeredMessageProperties": {}, "customMessageProperties": { "appname": "FromScheduler" } }, }

Nedan är ett exempel på en Service Bus-köåtgärd:

  "action": { "serviceBusQueueMessage": { "queueName": "q1",  
      "namespace": "mySBNamespace", "transportType": "netMessaging", // Kan vara antingen netMessaging eller AMQP "authentication": {  
        "sasKeyName": "QPolicy", "type": "sharedAccessKey" }, "message": "Some message",  
      "brokeredMessageProperties": {}, "customMessageProperties": { "appname": "FromScheduler" } }, "type": "serviceBusQueue" }

Hej ”errorAction” är hello felhanterare, hello-åtgärd som anropas när hello primära åtgärden misslyckas. Du kan använda den här variabeln toocall en felhantering slutpunkt eller skicka ett meddelande till användare. Detta kan användas för att nå en sekundär slutpunkt i hello fall att hello primära är inte tillgänglig (t.ex. i hello fallet med en katastrof på hello endpoint platsen) eller kan användas för att meddela en felhantering slutpunkt. Precis som hello primär åtgärd vara hello felåtgärd enkla eller sammansatta logik baserat på andra åtgärder. hur toocreate SAS-token finns för toolearn[skapar och använder en signatur för delad åtkomst](https://msdn.microsoft.com/library/azure/jj721951.aspx).

## <a name="recurrence"></a>recurrence
Upprepningskomponenten har flera delar:

* Frekvens: Minut, timme, dag, vecka, månad eller år.  
* Intervall: Intervall på hello angivna frekvensen för hello upprepning  
* Föreskrivna schema: Ange minuter, timmar, veckodagar, månader och monthdays av hello-intervall  
* Antal: Antal förekomster.  
* Sluttid: inga jobb körs när hello angett sluttid  

Ett jobb är återkommande om ett återkommande objekt har angetts i jobbets JSON-definition. Om både antal och endTime har angetts måste hanteras hello slutförande regel som inträffar först.

## <a name="state"></a>state
hello hello jobb är en av fyra värden: aktiverat, inaktiverat, slutfört eller fel. Du kan publicera eller korrigering jobb så som tooupdate dem toohello aktiverat eller inaktiverat tillstånd. Om ett jobb har slutförts eller fel som är ett slutgiltigt tillstånd som inte kan uppdateras (även om hello jobbet kan fortfarande tas bort). Ett exempel på hello tillstånd egenskapen är följande:

        "state": "disabled", // enabled, disabled, completed, or faulted
Jobb med tillståndet completed eller faulted tas bort efter 60 dagar.

## <a name="status"></a>status
När ett jobb i Schemaläggaren har börjat returneras information om hello hello jobbets aktuella status. Det här objektet går inte att ange hello användare – anges som hello system. Men ingår den i hello jobbobjektet (snarare än en separat länkade resurs) så att ett enkelt kan hämta hello status för ett jobb.

Jobbstatus innehåller hello tiden för hello tidigare körning (eventuella) och hello hello nästa schemalagda körningstid (för pågående jobb) och hello körning antal hello jobb.

## <a name="retrypolicy"></a>retryPolicy
Om ett jobb i Schemaläggaren misslyckas är möjliga toospecify en försök princip toodetermine om och hur hello åtgärden igen. Detta bestäms av hello **retryType** objekt – anges för**ingen** om det inte finns några återförsöksprincip som ovan. Ställa in också**fast** om det finns en återförsöksprincip.

tooset en återförsöksprincip två ytterligare inställningar kan anges: ett Återförsöksintervall (**retryInterval**) och hello antal återförsök (**retryCount**).

hello återförsöksintervall anges med hello **retryInterval** objekt, är hello intervall mellan försök. Standardvärdet är 30 sekunder. Det minsta konfigurerbara värdet är 15 sekunder och det högsta värdet är 18 månader. Jobb i kostnadsfria jobbsamlingar har ett minsta konfigurerbart värde på 1 timme.  Det har definierats i hello ISO 8601-format. På liknande sätt hello värdet för hello antal försök har angetts med hello **retryCount** objekt; det är hello antalet gånger som ett nytt försök görs. Standardvärdet är 4 och maxvärdet är 20\. Båda **retryInterval** och **retryCount** är valfria. De ges sina standardvärden om **retryType** har angetts för**fast** och inga värden anges explicit.

## <a name="see-also"></a>Se även
 [Vad är Scheduler?](scheduler-intro.md)

 [Komma igång med Schemaläggaren i hello Azure-portalen](scheduler-get-started-portal.md)

 [Prenumerationer och fakturering i Azure Scheduler](scheduler-plans-billing.md)

 [Hur toobuild komplex schemaläggs och avancerade upprepning med Azure Schemaläggaren](scheduler-advanced-complexity.md)

 [Referens för REST-API:et för Azure Scheduler](https://msdn.microsoft.com/library/mt629143)

 [Referens för PowerShell-cmdlets för Azure Scheduler](scheduler-powershell-reference.md)

 [Hög tillgänglighet och tillförlitlighet i Azure Scheduler](scheduler-high-availability-reliability.md)

 [Gränser, standardinställningar och felkoder i Azure Scheduler](scheduler-limits-defaults-errors.md)

 [Utgående autentisering i Azure Scheduler](scheduler-outbound-authentication.md)

