---
title: "aaaHow tooBuild komplexa scheman och avancerade upprepning med Azure Schemaläggaren"
description: "Hur tooBuild komplex schemalägger och avancerade upprepning med Azure Schemaläggaren"
services: scheduler
documentationcenter: .NET
author: derek1ee
manager: kevinlam1
editor: 
ms.assetid: 5c124986-9f29-4cbc-ad5a-c667b37fbe5a
ms.service: scheduler
ms.workload: infrastructure-services
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 08/18/2016
ms.author: deli
ms.openlocfilehash: 02172791978b12be0ccb3078125d057b2efe8523
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="how-toobuild-complex-schedules-and-advanced-recurrence-with-azure-scheduler"></a>Hur tooBuild komplex schemalägger och avancerade upprepning med Azure Schemaläggaren
## <a name="overview"></a>Översikt
På pulsslag hello av en Azure-schemaläggare jobbet är hello *schema*. hello schema bestämmer när och hur hello Schemaläggaren körs hello jobb.

Azure Schemaläggaren kan du toospecify olika enstaka och återkommande scheman för ett jobb. *Enstaka* scheman eller en gång vid en angiven tid – effektivt, de är *återkommande* scheman som kör en gång. Återkommande scheman eller på en förutbestämd frekvens.

Med den här flexibiliteten kan Azure Schemaläggaren du stöder ett stort antal affärsscenarier:

* Rensning av periodiska data – t.ex. varje dag, ta bort alla tweets som är äldre än tre månader
* Arkiveringen – t.ex. varje månad, push historik toobackup tjänsten
* Begäranden för externa data – t.ex. Hämta var 15: e minut, nya ski Väderrapport från NOAA
* Bild bearbetning – t.ex. varje veckodag under lågtrafik använder molnet databehandling toocompress bilder överförs den dagen

I den här artikeln vi att gå igenom exempel jobb som du kan skapa med Azure Schemaläggaren. Vi ger hello JSON-data som beskriver varje schema. Om du använder hello [Scheduler REST API](https://msdn.microsoft.com/library/mt629143.aspx), kan du använda samma JSON för [skapar ett jobb i Azure Schemaläggaren](https://msdn.microsoft.com/library/mt629145.aspx).

## <a name="supported-scenarios"></a>Scenarier som stöds
hello många exemplen i det här avsnittet visar hello breda scenarier som har stöd för Azure-Schemaläggaren. Allmänna ordalag visar de här exemplen hur toocreate scheman för många beteende användningsmönster, inklusive hello viktiga nedan:

* Kör en gång på ett visst datum och tid
* Kör och upprepas flera gånger explicit
* Kör omedelbart och upprepas
* Kör och upprepas var  *n*  minuter, timmar, dagar, veckor eller månader med början vid en viss tidpunkt
* Kör och upprepas för varje vecka eller månad frekvens men endast på specifika dagar, särskilda dagar i veckan eller särskilda dagar i månaden
* Kör och återkomma med flera gånger under en period – t.ex. sista fredagen och måndag varje månad, eller vid 5:15 am och 17:15:00 varje dag

## <a name="dates-and-datetimes"></a>Datum och datum och tid
Datum i Azure-schemaläggare följer hello [ISO 8601-specifikationen](http://en.wikipedia.org/wiki/ISO_8601) och omfattar endast hello datum.

Datum och tid referenser i Azure-schemaläggare följer hello [ISO 8601-specifikationen](http://en.wikipedia.org/wiki/ISO_8601) och inkluderar både datum och tid delar. En tid som inte anger en UTC-förskjutningen antas toobe UTC.  

## <a name="how-to-use-json-and-rest-api-for-creating-schedules"></a>Så här: Använda JSON- och REST-API för att skapa scheman
ett enkelt schema med toocreate hello [Azure Scheduler REST API](https://msdn.microsoft.com/library/mt629143), första [registrera prenumerationen med en resursleverantör](https://msdn.microsoft.com/library/azure/dn790548.aspx) (hello providernamn för Schemaläggaren är  *Microsoft.Scheduler*), sedan [skapa en jobbsamling](https://msdn.microsoft.com/library/mt629159.aspx), och slutligen [skapar ett jobb](https://msdn.microsoft.com/library/mt629145.aspx). När du skapar ett jobb kan ange du schemaläggning och upprepning med JSON som hello ett utdrag nedan:

    {
        "startTime": "2012-08-04T00:00Z", // optional
         …
        "recurrence":                     // optional
        {
            "frequency": "week",     // can be "year" "month" "day" "week" "hour" "minute"
            "interval": 1,                // optional, how often toofire (default too1)
            "schedule":                   // optional (advanced scheduling specifics)
            {
                "weekDays": ["monday", "wednesday", "friday"],
                "hours": [10, 22]                      
            },
            "count": 10,                  // optional (default toorecur infinitely)
            "endTime": "2012-11-04",      // optional (default toorecur infinitely)
        },
        …
    }

## <a name="overview-job-schema-basics"></a>Översikt: Jobbet schemat grunderna
hello följande tabell ger en översikt över hello viktiga element relaterade toorecurrence och schemaläggning i ett jobb:

| **JSON-namn** | **Beskrivning** |
|:--- |:--- |
| ***startTime*** |*startTime* är datum och tid. För enkel scheman *startTime* är hello första förekomsten och för komplexa scheman hello jobbet ska börja tidigast *startTime*. |
| ***upprepning*** |Hej *återkommande* objektet anger regler för upprepning för hello jobbet och hello upprepning hello jobbet körs med. hello återkommande objekt stöder hello element *frekvens, intervall, endTime, count,* och *schema*. Om *återkommande* definieras *frekvens* krävs, hello andra *återkommande* är valfria. |
| ***frekvens*** |Hej *frekvens* sträng som representerar hello frekvens enhet på vilken hello jobbets återkomster. Värden som stöds är *”minuter”, ”timmar”, ”dag”, ”vecka”* eller *”månad”.* |
| ***intervall*** |Hej *intervall* är ett positivt heltal och anger hello-intervall för hello *frekvens* som avgör hur ofta hello jobbet ska köras. Till exempel om *intervall* är 3 och *frekvens* ”vecka” hello jobbets återkomster var 3 veckor. Azure Scheduler stöder maximalt *intervall* 18 månader för månatliga frekvens 78 veckor för frekvensen Varje vecka eller 548 dagar för daglig frekvens. Hello stöds intervall är 1 för timme och minut frekvens < = *intervall* < = 1000. |
| ***Sluttid*** |Hej *endTime* sträng Anger hello tid efter vilken hello jobbet inte ska köras. Det är inte giltigt toohave en *endTime* i hello tidigare. Om inget *endTime* eller antal har angetts, hello jobbet körs oändligt. Båda *endTime* och *antal* kan inte tas med för hello samma jobb. |
| ***Antal*** |<p>Hej *antal* är heltalet (större än noll) som anger hello antal gånger som det här jobbet ska köras innan du Slutför.</p><p>Hej *antal* representerar hello antal gånger hello jobb körs innan bestäms som slutförd. Till exempel för ett jobb som körs varje dag med *antal* 5 och startdatum för måndag, hello jobbet slutförs efter körning fredag. Om hello startar datumet infaller under föregående hello, hello första körning beräknas från hello skapelsetid.</p><p>Om inget *endTime* eller *antal* anges hello jobbet körs oändligt. Båda *endTime* och *antal* kan inte tas med för hello samma jobb.</p> |
| ***schema*** |Ett jobb med angivna intervall ändrar dess upprepning baserat på ett återkommande schema. En *schema* innehåller ändringar som baseras på minuter, timmar, veckodagar, dagar i månaden och veckonumret. |

## <a name="overview-job-schema-defaults-limits-and-examples"></a>Översikt: Jobbet schemat standarder, begränsningar och exempel
Efter den här översikten vi beskrivs de här elementen i detalj.

| **JSON-namn** | **Värdetyp** | **Krävs?** | **Standardvärde** | **Giltiga värden** | **Exempel** |
|:--- |:--- |:--- |:--- |:--- |:--- |
| ***startTime*** |Sträng |Nej |Ingen |ISO 8601-datum/tid |<code>"startTime" : "2013-01-09T09:30:00-08:00"</code> |
| ***upprepning*** |Objekt |Nej |Ingen |Upprepningsobjekt |<code>"recurrence" : { "frequency" : "monthly", "interval" : 1 }</code> |
| ***frekvens*** |Sträng |Ja |Ingen |”minuter”, ”timmar”, ”dag”, ”vecka”, ”månad” |<code>"frequency" : "hour"</code> |
| ***intervall*** |Tal |Nej |1 |1 too1000. |<code>"interval":10</code> |
| ***Sluttid*** |Sträng |Nej |Ingen |Datetime-värde som representerar en tidpunkt i hello framtida |<code>"endTime" : "2013-02-09T09:30:00-08:00"</code> |
| ***Antal*** |Tal |Nej |Ingen |>= 1 |<code>"count": 5</code> |
| ***schema*** |Objekt |Nej |Ingen |Schemaobjekt |<code>"schedule" : { "minute" : [30], "hour" : [8,17] }</code> |

## <a name="deep-dive-starttime"></a>Ingående: *startTime*
hello den följande tabellen insamlingar hur *startTime* styr hur ett jobb körs.

| **startTime-värdet** | **Ingen upprepning** | **Upprepning. Inget schema** | **Återkommande schema** |
|:--- |:--- |:--- |:--- |
| **Ingen starttid** |Kör en gång direkt |Kör en gång direkt. Köra efterföljande körningar baserat på Beräkna från tid för senaste körning |<p>Kör en gång direkt</p><p>Kör efterföljande körningar baserat på upprepningsschemat</p> |
| **Starttid i förfluten tid** |Kör en gång direkt |<p>Beräkna första framtida körningstid efter starttiden och köras vid den tiden</p><p>Köra efterföljande körningar baserat oncalculating från tid för senaste körning</p><p>Se exempel efter den här tabellen för en ytterligare förklaring</p> |<p>Jobbet startar *inte sooner än* hello starttid som anges. hello första förekomsten är baserad på hello schema beräknade från hello starttid</p><p>Kör efterföljande körningar baserat på upprepningsschemat</p> |
| **Starttid i framtiden eller för närvarande** |Kör en gång vid angivna start |<p>Kör en gång vid angivna start</p><p>Köra efterföljande körningar baserat på Beräkna från tid för senaste körning</p> |<p>Jobbet startar *inte sooner än* hello starttid som anges. hello första förekomsten är baserad på hello schema beräknade från hello starttid</p><p>Kör efterföljande körningar baserat på upprepningsschemat</p> |

Låt oss se ett exempel på vad som händer där *startTime* i hello tidigare, med *återkommande* men ingen *schema*.  Anta att hello aktuell tid är 2015-04-08 13:00 *startTime* är 2015-04-07 14:00 och *återkommande* är 2 dagar (definieras med *frekvens*: dag och *intervall*: 2.) Observera att hello *startTime* i hello senaste och inträffar före hello aktuell tid

Under dessa förhållanden hello *första körningen* kommer att vara 2015-04-09 14:00\. motor för hello Schemaläggaren beräknar körning förekomster från hello starttid.  Alla instanser i hello senaste ignoreras. hello använder hello nästa förekomst som uppstår i hello framtida.  Så i detta fall *startTime* är 2015-04-07 på 2:00:00 så hello nästa förekomst är 2 dagar från den tid som är 2015-04-09 på 2:00 pm.

Observera att hello första körningen skulle vara hello samma även om hello startTime 2015-04-05 14:00 eller 2015-04-01 14:00\. Efter hello första körning och efterföljande körningar beräknas med hjälp av hello schemalagda – så att de är utsatta för 2015-04-11 på 2:00:00 sedan 2015-04-13 på 2:00:00 sedan 2015-04-15 på 2:00:00 osv.

Slutligen, när ett jobb har ett schema om timmar eller minuter har ställts in i hello schemat de standard toohello timmar eller minuter hello första körningen, respektive.

## <a name="deep-dive-schedule"></a>Ingående: *schema*
Dels måste en *schema* kan *gränsen* hello antal jobb körningar.  Till exempel, om ett jobb med en frekvens som ”månad” har en *schema* som körs på endast dag 31, hello jobbet körs i de månader som har en 31<sup>st</sup> dag.

Hej å andra sidan kan en *schema* kan också *Expandera* hello antal jobb körningar. Till exempel, om ett jobb med en frekvens som ”månad” har en *schema* som körs på 1 och 2 dagar i månaden hello jobbet körs på hello 1<sup>st</sup> och 2<sup>nd</sup> dagar i månaden hello i stället för en gång en månad.

Om flera schemaelement har angetts är hello ordningen för utvärderingen från hello största toosmallest – vecka, månad, dag, veckodag, timmar och minut.

hello följande tabell beskrivs *schema* element i detalj.

| **JSON-namn** | **Beskrivning** | **Giltiga värden** |
|:--- |:--- |:--- |
| **minuter** |Minuter av hello timme på vilka hello jobbet körs |<ul><li>Heltal, eller</li><li>Heltalsmatris</li></ul> |
| **timmar** |Timmars hello dag på vilka hello jobbet körs |<ul><li>Heltal, eller</li><li>Heltalsmatris</li></ul> |
| **Veckodagar** |Dagar för hello vecka hello jobbet körs. Kan bara anges med veckofrekvens. |<ul><li>”Måndag”, ”tisdag”, ”onsdag”, ”torsdag”, ”fredag”, ”lördag” eller ”söndag”</li><li>Matris med någon av hello ovan värden (max matrisstorlek 7)</li></ul>*Inte* skiftlägeskänsligt |
| **monthlyOccurrences** |Fastställer vilka dagar hello månad hello jobbet ska köras. Kan bara anges med månadsfrekvens. |<ul><li>Matris med monthlyOccurrence objekt:</li></ul> <pre>{ "day": *day*,<br />  "occurrence": *occurrence*<br />}</pre><p> *dag* är hello veckodag hello vecka hello jobbet körs, t.ex. {söndag} är varje söndag hello månad. Krävs.</p><p>Förekomst är *förekomsten* hello dag under hello månaden, t.ex. {söndag, -1} är hello sista söndagen hello månad. Valfri.</p> |
| **monthDays** |Dag i hello månad hello jobbet körs. Kan bara anges med månadsfrekvens. |<ul><li>Inget värde < = -1 och > =-31.</li><li>Ett värde > = 1 och < = 31.</li><li>En matris med ovanstående värden</li></ul> |

## <a name="examples-recurrence-schedules"></a>Exempel: Återkommande scheman
hello följande är olika exempel på återkommande scheman – fokus på hello objekt och dess underordnade element.

hello scheman under alla förutsätter att hello *intervall* anges too1\. En måste dessutom förutsätter hello rätt frekvens i enlighet toowhat är i hello *schema* – t.ex. en kan inte använda frekvens ”dag” och har en ”monthDays” ändring i hello schema. Dessa begränsningar som beskrivs ovan.

| **Exempel** | **Beskrivning** |
|:--- |:--- |
| <code>{"hours":[5]}</code> |Kör kl 5 varje dag. Azure Scheduler matchar varje värde i ”timmar” där varje värde i ”minuter”, en i taget, kör du en lista över alla hello tidpunkter på vilka hello jobbet toobe toocreate. |
| <code>{"minutes":[15], "hours":[5]}</code> |Kör kl 5:15 varje dag |
| <code>{"minutes":[15], "hours":[5,17]}</code> |Kör på 5:15 AM och 17:15:00 varje dag |
| <code>{"minutes":[15,45], "hours":[5,17]}</code> |Kör kl 5:15, 5:45 AM, 17:15:00 och 17:45:00 varje dag |
| <code>{"minutes":[0,15,30,45]}</code> |Kör var 15: e minut |
| <code>{hours":[0, 1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 13, 14, 15, 16, 17, 18, 19, 20, 21, 22, 23]}</code> |Kör varje timme. Det här jobbet körs varje timme. hello minut styrs av hello *startTime*, om någon sådan har angetts eller om inget anges av hello skapelsetid. Till exempel om hello start tid eller skapelsetid (beroende på vilket) är 12:25 PM, hello jobbet ska köras vid 00:25, 01:25 02:25,..., 23:25. hello schema är likvärdiga toohaving ett jobb med *frekvens* ”timme” en *intervall* 1 och Nej *schema*. hello skillnaden är att det här schemat kan användas med olika *frekvens* och *intervall* toocreate andra jobb för. Till exempel, om hello *frekvens* har ”månad” hello schema körs bara en gång i månaden i stället för varje dag om *frekvens* har ”dag” |
| <code>{minutes:[0]}</code> |Kör varje timme på hello timme. Det här jobbet körs även varje timme, men på hello timme (t.ex. 12: 00, 1 kl 02: 00, etc.) Detta är likvärdiga tooa jobb med frekvensen för ”timme”, startTime med noll minuter och inget schema om hello frekvens ”dag”, men om hello frekvens ”vecka” eller ”månad”, hello schema skulle köra endast en dag, en vecka eller en dag i månaden, respektive. |
| <code>{"minutes":[15]}</code> |Kör i 15 minuter efter timme varje timme. Körs varje timme, med start 00:15, 01:15, 02:15 osv och slutar 22:15 och 23:15. |
| <code>{"hours":[17], "weekDays":["saturday"]}</code> |Kör kl på lördagar varje vecka |
| <code>{hours":[17], "weekDays":["monday", "wednesday", "friday"]}</code> |Kör kl måndag, onsdag och fredag varje vecka |
| <code>{"minutes":[15,45], "hours":[17], "weekDays":["monday", "wednesday", "friday"]}</code> |Kör på 17:15:00 och 17:45:00 på måndag, onsdag och fredag varje vecka |
| <code>{"hours":[5,17], "weekDays":["monday", "wednesday", "friday"]}</code> |Kör på 5 AM och 17: 00 på måndag, onsdag och fredag varje vecka |
| <code>{"minutes":[15,45], "hours":[5,17], "weekDays":["monday", "wednesday", "friday"]}</code> |Kör kl 5:15, 5:45 AM, 17:15:00 och 17:45:00 på måndag, onsdag och fredag varje vecka |
| <code>{"minutes":[0,15,30,45], "weekDays":["monday", "tuesday", "wednesday", "thursday", "friday"]}</code> |Körs var 15: e minut på vardagar |
| <code>{"minutes":[0,15,30,45], "hours": [9, 10, 11, 12, 13, 14, 15, 16] "weekDays":["monday", "tuesday", "wednesday", "thursday", "friday"]}</code> |Kör var 15: e minut på vardagar mellan 9: 00 och 4:45 PM |
| <code>{"weekDays":["sunday"]}</code> |Kör på söndagar vid Start |
| <code>{"weekDays":["tuesday", "thursday"]}</code> |Kör varje tisdag och torsdag vid Start |
| <code>{"minutes":[0], "hours":[6], "monthDays":[28]}</code> |Kör kl 6 på hello 28 dag av varje månad (förutsatt att frekvensen för månad) |
| <code>{"minutes":[0], "hours":[6], "monthDays":[-1]}</code> |Kör kl 6 på hello sista dagen i hello månad. Om du vill att toorun ett jobb på hello sista dagen i månaden, Använd -1 i stället för dag 28, 29, 30 och 31. |
| <code>{"minutes":[0], "hours":[6], "monthDays":[1,-1]}</code> |Kör kl 6 på hello första och sista dag i varje månad |
| <code>{monthDays":[1,-1]}</code> |Kör på hello första och sista dag i varje månad vid Start |
| <code>{monthDays":[1,14]}</code> |Kör på hello första och Fourteenth dag av varje månad vid Start |
| <code>{monthDays":[2]}</code> |Kör på hello andra dagen i hello månad starttid |
| <code>{"minutes":[0], "hours":[5], "monthlyOccurrences":[{"day":"friday", "occurrence":1}]}</code> |Kör på första fredagen i varje månad kl 5 |
| <code>{"monthlyOccurrences":[{"day":"friday", "occurrence":1}]}</code> |: Köra första fredagen i varje månad vid Start |
| <code>{"monthlyOccurrences":[{"day":"friday", "occurrence":-3}]}</code> |Kör tredje fredag från slutet av månad, varje månad, vid Start |
| <code>{"minutes":[15], "hours":[5], "monthlyOccurrences":[{"day":"friday", "occurrence":1},{"day":"friday", "occurrence":-1}]}</code> |Kör på första och sista fredagen i varje månad kl 5:15 |
| <code>{"monthlyOccurrences":[{"day":"friday", "occurrence":1},{"day":"friday", "occurrence":-1}]}</code> |Kör på första och sista fredagen i varje månad vid Start |
| <code>{"monthlyOccurrences":[{"day":"friday", "occurrence":5}]}</code> |Kör på femte fredagen i varje månad vid Start. Om det finns inga femte fredag i en månad denna finns inte köras eftersom det är schemalagda toorun på endast femte fredagar. Du kan försöka med -1 i stället för 5 för hello förekomst om du vill toorun hello jobbet på hello senaste inträffar fredag hello månad. |
| <code>{"minutes":[0,15,30,45], "monthlyOccurrences":[{"day":"friday", "occurrence":-1}]}</code> |Körs var 15: e minut sista fredagen i hello månad |
| <code>{"minutes":[15,45], "hours":[5,17], "monthlyOccurrences":[{"day":"wednesday", "occurrence":3}]}</code> |Kör kl 5:15, 5:45 AM, 17:15:00 och 17:45:00 på hello 3 onsdag av varje månad |

## <a name="see-also"></a>Se även
 [Vad är Scheduler?](scheduler-intro.md)

 [Begrepp, terminologi och entitetshierarki relaterade till Azure Scheduler](scheduler-concepts-terms.md)

 [Komma igång med Schemaläggaren i hello Azure-portalen](scheduler-get-started-portal.md)

 [Prenumerationer och fakturering i Azure Scheduler](scheduler-plans-billing.md)

 [Referens för REST-API:et för Azure Scheduler](https://msdn.microsoft.com/library/mt629143)

 [Referens för PowerShell-cmdlets för Azure Scheduler](scheduler-powershell-reference.md)

 [Hög tillgänglighet och tillförlitlighet i Azure Scheduler](scheduler-high-availability-reliability.md)

 [Gränser, standardinställningar och felkoder i Azure Scheduler](scheduler-limits-defaults-errors.md)

 [Utgående autentisering i Azure Scheduler](scheduler-outbound-authentication.md)

