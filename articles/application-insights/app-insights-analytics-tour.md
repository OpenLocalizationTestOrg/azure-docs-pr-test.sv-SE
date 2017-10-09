---
title: aaaA visning via analyser i Azure Application Insights | Microsoft Docs
description: "Kort prover av alla hello huvudsakliga frågor i Analytics hello kraftfullt sökverktyg av Application Insights."
services: application-insights
documentationcenter: 
author: CFreemanwa
manager: carmonm
ms.assetid: bddf4a6d-ea8d-4607-8531-1fe197cc57ad
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 05/06/2017
ms.author: bwren
ms.openlocfilehash: c268e26c6bf93ac2ee2a9d5e83613150dcf90b04
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="a-tour-of-analytics-in-application-insights"></a>En genomgång av Analytics i Application Insights
[Analytics](app-insights-analytics.md) är hello kraftfulla sökfunktionen i [Programinsikter](app-insights-overview.md). Dessa sidor beskrivs Log Analytics-frågespråket.

* **[Se hello inledande video](https://applicationanalytics-media.azureedge.net/home_page_video.mp4)**.
* **[Testkör Analytics på våra simulerade data](https://analytics.applicationinsights.io/demo)**  om din app inte skickar data tooApplication insikter ännu.
* **[SQL-användarnas cheat blad](https://aka.ms/sql-analytics)**  översätter hello vanligaste idioms.

Låt oss ta ett gå igenom några grundläggande frågor tooget som du startade.

## <a name="connect-tooyour-application-insights-data"></a>Ansluta tooyour Application Insights-data
Öppna Analytics från din app [översikt bladet](app-insights-dashboards.md) i Application Insights:

![Öppna portal.azure.com, öppna Application Insights-resursen och klicka på Analytics.](./media/app-insights-analytics-tour/001.png)

## <a name="takehttpsdocsloganalyticsioquerylanguagequerylanguagetakeoperatorhtml-show-me-n-rows"></a>[Ta](https://docs.loganalytics.io/queryLanguage/query_language_takeoperator.html): Visa n rader
Datapunkter som loggar användaren operations (vanligtvis HTTP-begäranden tas emot av ditt webbprogram) lagras i en tabell som kallas `requests`. Varje rad är en telemetri data togs emot från hello Application Insights SDK i din app.

Låt oss börja med att undersöka några exempel rader i tabellen hello:

![resultat](./media/app-insights-analytics-tour/010.png)

> [!NOTE]
> Placera markören hello någonstans i hello instruktionen innan du klickar på OK. Du kan dela en instruktion över flera rader, men Placera inte tomma rader i en instruktion. Tomma rader är ett bekvämt sätt tookeep flera separata frågor i hello-fönstret.
>
>

Välj kolumner, drar du dem, gruppera efter kolumner, och filtrera:

![Klicka på kolumnmarkering i övre högra hörnet av resultat](./media/app-insights-analytics-tour/030.png)

Expandera alla toosee hello information om:

![Välj tabellen och konfigurera kolumner](./media/app-insights-analytics-tour/040.png)

> [!NOTE]
> Klicka på hello chefen för en kolumn toore ordning hello resultat är tillgängliga i hello webbläsare. Men tänk på att hello antal rader hämtade toohello webbläsare för en stor resultatmängd är begränsad. Sortering därmed visar inte alltid du hello faktiska högsta eller lägsta objekt. toosort-objekt på ett tillförlitligt sätt, använder du hello `top` eller `sort` operator.
>
>

## <a name="tophttpsdocsloganalyticsioquerylanguagequerylanguagetopoperatorhtml-and-sorthttpsdocsloganalyticsioquerylanguagequerylanguagesortoperatorhtml"></a>[TOP](https://docs.loganalytics.io/queryLanguage/query_language_topoperator.html) och [sortera](https://docs.loganalytics.io/queryLanguage/query_language_sortoperator.html)
`take`är användbara tooget en snabb exempel på ett resultat, men den visar rader från tabellen hello i bokstavsordning. Använd tooget en ordnad vy `top` (till exempel) eller `sort` (via hello hela tabellen).

Visa hello första n rader, sorterade efter en viss kolumn:

```AIQL

    requests | top 10 by timestamp desc
```

* *Syntax:* de flesta operatörer har nyckelordet parametrar som `by`.
* `desc`= fallande ordning, `asc` = stigande.

![](./media/app-insights-analytics-tour/260.png)

`top...`är ett mer performant sätt att säga `sort ... | take...`. Vi kan skriva:

```AIQL

    requests | sort by timestamp desc | take 10
```

hello resultatet blir hello detsamma, men det skulle köras lite långsammare. (Du kan också skriva `order`, vilket är ett alias för `sort`.)

hello kolumnrubrikerna i hello tabellvy kan också vara används toosort hello resultat på hello-skärmen. Men av kursen, om du har använt `take` eller `top` tooretrieve bara en del av en tabell, ska du bara ordna hello poster som du har hämtats.

## <a name="wherehttpsdocsloganalyticsioquerylanguagequerylanguagewhereoperatorhtml-filtering-on-a-condition"></a>[Där](https://docs.loganalytics.io/queryLanguage/query_language_whereoperator.html): filtrering på ett villkor

Låt oss se bara begäranden som returnerade en viss Resultatkod:

```AIQL

    requests
    | where resultCode  == "404"
    | take 10
```

![](./media/app-insights-analytics-tour/250.png)

Hej `where` operator tar ett booleskt uttryck. Här följer några huvudpunkter om dem:

* `and`, `or`: Booleska operatorer
* `==`, `<>`, `!=` : lika med och inte lika med
* `=~`, `!~` : skiftlägeskänsliga sträng lika och inte lika. Det finns många fler sträng jämförelseoperatorer.

<!---Read all about [scalar expressions]().--->

### <a name="getting-hello-right-type"></a>Hämta hello rätt typ
Sök efter misslyckade begäranden:

```AIQL

    requests
    | where isnotempty(resultCode) and toint(resultCode) >= 400
```
<!---
`resultCode` has type string, so we must cast it app-insights-analytics-reference.md#casts for a numeric comparison.
--->

## <a name="time"></a>Tid

Dina frågor är som standard begränsad toohello senaste 24 timmarna. Men du kan ändra detta intervall:

![](./media/app-insights-analytics-tour/change-time-range.png)

Åsidosätt hello tidsintervall genom att skriva en fråga som nämns `timestamp` i en where-sats. Exempel:

```AIQL

    // What were hello slowest requests over hello past 3 days?
    requests
    | where timestamp > ago(3d)  // Override hello time range
    | top 5 by duration
```

hello tid intervallet funktionen är likvärdiga tooa 'where' satsen läggas till efter varje uppgift om något av hello källtabellerna.

`ago(3d)`innebär 'tre dagar sedan'. Andra tidsenheter inkludera timmar (`2h`, `2.5h`), minuter (`25m`), och sekunder (`10s`).

Andra exempel:

```AIQL

    // Last calendar week:
    requests
    | where timestamp > startofweek(now()-7d)
        and timestamp < startofweek(now())
    | top 5 by duration

    // First hour of every day in past seven days:
    requests
    | where timestamp > ago(7d) and timestamp % 1d < 1h
    | top 5 by duration

    // Specific dates:
    requests
    | where timestamp > datetime(2016-11-19) and timestamp < datetime(2016-11-21)
    | top 5 by duration

```

[Datum och tider referens](https://docs.loganalytics.io/concepts/concepts_datatypes_datetime.html).


## <a name="projecthttpsdocsloganalyticsioquerylanguagequerylanguageprojectoperatorhtml-select-rename-and-compute-columns"></a>[Projektet](https://docs.loganalytics.io/queryLanguage/query_language_projectoperator.html): Välj, byta namn på och bearbetning kolumner
Använd [ `project` ](https://docs.loganalytics.io/queryLanguage/query_language_projectoperator.html) toopick ut precis hello kolumner som du vill:

```AIQL

    requests | top 10 by timestamp desc
             | project timestamp, name, resultCode
```

![](./media/app-insights-analytics-tour/240.png)

Du kan också byta namn på kolumner och definiera nya:

```AIQL

    requests
    | top 10 by timestamp desc
    | project  
            name,
            response = resultCode,
            timestamp,
            ['time of day'] = floor(timestamp % 1d, 1s)
```

![Resultat](./media/app-insights-analytics-tour/270.png)

* Kolumnnamn kan innehålla blanksteg eller symboler om de omges så här: `['...']` eller`["..."]`
* `%`är hello vanliga modulo-operatorn.
* `1d`(som är en siffra, en hade ”) är en literal timespan vilket innebär en dag. Här följer några fler timespan-litteraler: `12h`, `30m`, `10s`, `0.01s`.
* `floor`(alias `bin`) Avrundar värdet ned toohello närmaste multipel av hello Basvärde som du anger. Så `floor(aTime, 1s)` Avrundar taget ned toohello närmsta sekund.

Uttryck kan innehålla alla hello vanliga operatorer (`+`, `-`,...), och det finns en uppsättning praktiska funktioner.

## <a name="extend"></a>Utöka
Om du bara vill tooadd kolumner toohello befintliga använder [ `extend` ](https://docs.loganalytics.io/queryLanguage/query_language_extendoperator.html):

```AIQL

    requests
    | top 10 by timestamp desc
    | extend timeOfDay = floor(timestamp % 1d, 1s)
```

Med hjälp av [ `extend` ](https://docs.loganalytics.io/queryLanguage/query_language_extendoperator.html) är mindre utförlig än [ `project` ](https://docs.loganalytics.io/queryLanguage/query_language_projectoperator.html) om du vill tookeep alla hello befintliga kolumner.

### <a name="convert-toolocal-time"></a>Konvertera toolocal tid

Tidsstämplar är alltid i UTC. Om du är på hello oss Pacific kusten och det är vinter, kan du därför som detta:

```AIQL

    requests
    | top 10 by timestamp desc
    | extend localTime = timestamp - 8h
```


## <a name="summarizehttpsdocsloganalyticsioquerylanguagequerylanguagesummarizeoperatorhtml-aggregate-groups-of-rows"></a>[Sammanfatta](https://docs.loganalytics.io/queryLanguage/query_language_summarizeoperator.html): aggregera grupper av rader
`Summarize`tillämpar en angiven *aggregeringsfunktionen* över grupper av rader.

Till exempel hello överföringstiden för ditt webbprogram toorespond tooa begäran rapporteras i hello fältet `duration`. Låt oss se genomsnittlig svarstid för hello tid tooall begäranden:

![](./media/app-insights-analytics-tour/410.png)

Eller kan vi dela hello resultatet i förfrågningar av olika namn:

![](./media/app-insights-analytics-tour/420.png)

`Summarize`samlar in hello datapunkter i hello dataström i grupper för vilka hello `by` satsen utvärderar lika. Varje värde i hello `by` uttryck - varje åtgärdsnamn i hello ovan exempel - resulterar i en rad i hello resultattabellen.

Eller vi gick grupperar resultaten efter tid på dagen:

![](./media/app-insights-analytics-tour/430.png)

Observera hur vi använder hello `bin` funktionen (även kallat `floor`). Om vi använde `by timestamp`, varje inkommande rad skulle hamna i en egen liten grupp. För en kontinuerlig skalära like gånger eller nummer har vi toobreak hello kontinuerligt område i hanterbara flera diskreta värden. `bin`-som är precis hello bekant avrundas nedåt `floor` fungerar - är hello enklaste sättet toodo som.

Vi kan använda hello samma teknik tooreduce intervall av strängar:

![](./media/app-insights-analytics-tour/440.png)

Observera att du kan använda `name=` tooset hello namnet på en resultatkolumn i hello mängduttryck eller hello av-satsen.

## <a name="counting-sampled-data"></a>Cyklisk exempeldata
`sum(itemCount)`är hello rekommenderade aggregering toocount händelser. I många fall itemCount == 1, så hello funktionen bara räknar upp hello antalet rader i hello grupp. Men när [provtagning](app-insights-sampling.md) är i drift, en bråkdel av hello ursprungliga händelser behålls som datapunkter i Application Insights, så att det finns för varje datapunkt som du ser, `itemCount` händelser.

Till exempel om sampling ignorerar 75% av hello ursprungliga händelser och sedan itemCount == 4 i hello behålls poster – det vill säga för varje kvarhållna post fanns fyra ursprungliga poster.

Anpassningsbar provtagning gör itemCount toobe högre under perioder när ditt program som ofta används.

Sammanfattningsvis itemCount därför ger en bra uppskattning hello ursprungliga antalet händelser.

![](./media/app-insights-analytics-tour/510.png)

Det finns också en `count()` aggregering (och ett antal åtgärden) i de fall där du verkligen vill toocount hello antalet rader i en grupp.

Det finns en mängd [aggregeringsfunktioner](https://docs.loganalytics.io/learn/tutorials/aggregations.html).

## <a name="charting-hello-results"></a>Diagram hello resultat
```AIQL

    exceptions
       | summarize count=sum(itemCount)  
         by bin(timestamp, 1h)
```

Resultatet visas som en tabell som standard:

![](./media/app-insights-analytics-tour/225.png)

Vi kan göra bättre än hello tabellvy. Vi titta på hello resulterar i hello diagramvy med hello vertikalstreck alternativet:

![Klicka på diagrammet och sedan välja stående stapeldiagram och tilldela x och y axlar](./media/app-insights-analytics-tour/230.png)

Observera att även om vi inte sortera hello resultaten av tid (som du ser i hello tabell visa), hello diagramvyn alltid visar datum och tid i rätt ordning.


## <a name="timecharts"></a>Timecharts
Visa det hur många händelser är varje timma:

```AIQL

    requests
      | summarize event_count=sum(itemCount)
        by bin(timestamp, 1h)
```

Välj Visningsalternativ för hello diagram:

![timechart](./media/app-insights-analytics-tour/080.png)

## <a name="multiple-series"></a>Flera serier
Flera uttryck i hello `summarize` satsen skapar flera kolumner.

Flera uttryck i hello `by` satsen skapar flera rader, ett för varje kombination av värden.

```AIQL

    requests
    | summarize count_=sum(itemCount), avg(duration)
      by bin(timestamp, 1h), client_StateOrProvince, client_City
    | order by timestamp asc, client_StateOrProvince, client_City
```

![Tabell för förfrågningar efter timme och plats](./media/app-insights-analytics-tour/090.png)

### <a name="segment-a-chart-by-dimensions"></a>Segmentera ett diagram av dimensioner
Om diagrammet en tabell som har en strängkolumn och en numerisk kolumn hello sträng kan använda toosplit hello numeriska data till olika antal datapunkter. Om det finns fler än en strängkolumn, kan du välja vilka kolumnen toouse som hello diskriminator.

![Segmentera ett analytics-diagram](./media/app-insights-analytics-tour/100.png)

#### <a name="bounce-rate"></a>Studs hastighet

Konvertera en boolesk tooa sträng toouse den som en diskriminator:

```AIQL

    // Bounce rate: sessions with only one page view
    requests
    | where notempty(session_Id)
    | where tostring(operation_SyntheticSource) == "" // real users
    | summarize pagesInSession=sum(itemCount), sessionEnd=max(timestamp)
               by session_Id
    | extend isbounce= pagesInSession == 1
    | summarize count()
               by tostring(isbounce), bin (sessionEnd, 1h)
    | render timechart
```

### <a name="display-multiple-metrics"></a>Visa flera mått
Om du diagram av en tabell som har mer än en numerisk kolumn i tillägg toohello tidsstämpel och kan du visa en kombination av dessa.

![Segmentera ett analytics-diagram](./media/app-insights-analytics-tour/110.png)

Du måste välja **inte dela** innan du kan välja flera numeriska kolumner. Du kan inte dela genom en strängkolumn på hello samma tid som visas mer än en numerisk kolumn.

## <a name="daily-average-cycle"></a>Dagliga genomsnittliga cykel
Hur varierar användning över hello genomsnittlig dag?

Antal begäranden med hello tid modulo en dag binned i timmar:

```AIQL

    requests
    | where timestamp > ago(30d)  // Override "Last 24h"
    | where tostring(operation_SyntheticSource) == "" // real users
    | extend hour = bin(timestamp % 1d , 1h)
          + datetime("2016-01-01") // Allow render on line chart
    | summarize event_count=sum(itemCount) by hour
```

![Linjediagram timmar i en genomsnittlig dag](./media/app-insights-analytics-tour/120.png)

> [!NOTE]
> Observera att för närvarande har vi tooconvert tid varaktigheter toodatetimes i ordning toodisplay på ett linjediagram.
>
>

## <a name="compare-multiple-daily-series"></a>Jämför flera dagliga serier
Hur användning varierar över hello tid på dagen i olika länder?

```AIQL

     requests  
     | where timestamp > ago(30d)  // Override "Last 24h"
     | where tostring(operation_SyntheticSource) == "" // real users
     | extend hour= floor( timestamp % 1d , 1h)
           + datetime("2001-01-01")
     | summarize event_count=sum(itemCount)
       by hour, client_CountryOrRegion
     | render timechart
```

![Delning av client_CountryOrRegion](./media/app-insights-analytics-tour/130.png)

## <a name="plot-a-distribution"></a>Rita en distributionsplats
Hur många sessioner är det av olika längd?

```AIQL

    requests
    | where timestamp > ago(30d) // override "Last 24h"
    | where isnotnull(session_Id) and isnotempty(session_Id)
    | summarize min(timestamp), max(timestamp)
      by session_Id
    | extend sessionDuration = max_timestamp - min_timestamp
    | where sessionDuration > 1s and sessionDuration < 3m
    | summarize count() by floor(sessionDuration, 3s)
    | project d = sessionDuration + datetime("2016-01-01"), count_
```

hello sista raden är nödvändiga tooconvert toodatetime. För närvarande visas hello x-axeln i ett diagram som en skalär endast om det är ett datetime-värde.

Hej `where` satsen utesluter one-shot sessioner (sessionDuration == 0) och anger hello längd hello x-axeln.

![](./media/app-insights-analytics-tour/290.png)

## <a name="percentileshttpsdocsloganalyticsioquerylanguagequerylanguagepercentilesaggfunctionhtml"></a>[Percentiler](https://docs.loganalytics.io/queryLanguage/query_language_percentiles_aggfunction.html)
Vilka områden för varaktigheter täcka olika delar av sessioner?

Använda hello ovan frågan, men ersätt hello sista raden:

```AIQL

    requests
    | where isnotnull(session_Id) and isnotempty(session_Id)
    | summarize min(timestamp), max(timestamp)
      by session_Id
    | extend sesh = max_timestamp - min_timestamp
    | where sesh > 1s
    | summarize count() by floor(sesh, 3s)
    | summarize percentiles(sesh, 5, 20, 50, 80, 95)
```

Vi också bort hello övre gräns på hello där sats i ordning tooget korrigera uppgifter som inkluderar alla sessioner med mer än en förfrågan:

![Resultat](./media/app-insights-analytics-tour/180.png)

Där hittar som:

* 5% av sessioner har en varaktighet på mindre än 3 minuter 34s;
* 50% av sessioner senaste 36 minuter.
* 5% av sessioner senaste mer än 7 dagar

tooget en separat uppdelning för varje land, vi har precis toobring hello client_CountryOrRegion kolumn separat via båda sammanfatta operatorer:

```AIQL

    requests
    | where isnotnull(session_Id) and isnotempty(session_Id)
    | summarize min(timestamp), max(timestamp)
      by session_Id, client_CountryOrRegion
    | extend sesh = max_timestamp - min_timestamp
    | where sesh > 1s
    | summarize count() by floor(sesh, 3s), client_CountryOrRegion
    | summarize percentiles(sesh, 5, 20, 50, 80, 95)
      by client_CountryOrRegion
```

![](./media/app-insights-analytics-tour/190.png)

## <a name="join"></a>Slå ihop
Vi har åtkomst tooseveral tabeller, inklusive begäranden och undantag.

toofind hello undantag relaterade tooa begäran som returnerade ett fel svar, vi kan koppla hello tabeller på `session_Id`:

```AIQL

    requests
    | where toint(resultCode) >= 500
    | join (exceptions) on operation_Id
    | take 30
```


Det är god sed toouse `project` tooselect bara hello kolumner som vi behöver innan du utför hello koppling.
I hello samma satser vi byta namn på hello kolumn för tidsstämpel.

## <a name="lethttpsdocsloganalyticsioquerylanguagequerylanguageletstatementhtml-assign-a-result-tooa-variable"></a>[Låt](https://docs.loganalytics.io/queryLanguage/query_language_letstatement.html): tilldela en variabel för resultatet tooa

Använd `let` tooseparate hello delar av hello föregående uttryck. hello resultatet är oförändrad:

```AIQL

    let bad_requests =
      requests
        | where  toint(resultCode) >= 500  ;
    bad_requests
    | join (exceptions) on session_Id
    | take 30
```

> [!Tip] 
> Hello Analytics-klienten Placera inte tomma rader mellan hello delar av hello fråga. Se till att tooexecute alla.
>

Använd `toscalar` tooconvert en enskild tabell tooa Cellvärde:

```AIQL
let topCities =  toscalar (
   requests
   | summarize count() by client_City 
   | top n by count_ 
   | summarize makeset(client_City));
requests
| where client_City in (topCities(3)) 
| summarize count() by client_City;
```


### <a name="functions"></a>Funktioner

Använd *kan* toodefine en funktion:

```AIQL

    let usdate = (t:datetime)
    {
      strcat(getmonth(t), "/", dayofmonth(t),"/", getyear(t), " ",
      bin((t-1h)%12h+1h,1s), iff(t%24h<12h, "AM", "PM"))
    };
    requests  
    | extend PST = usdate(timestamp-8h)
```

## <a name="accessing-nested-objects"></a>Åtkomst till kapslade objekt
Kapslade objekt kan nås enkelt. Till exempel i hello undantag dataströmmen ser du strukturerade objekt så här:

![Resultat](./media/app-insights-analytics-tour/520.png)

Du kan förenkla den genom att välja hello egenskaper som du är intresserad av:

```AIQL

    exceptions | take 10
    | extend method1 = tostring(details[0].parsedStack[1].method)
```

Observera att du måste toocast hello toohello lämplig resultattyp.


## <a name="custom-properties-and-measurements"></a>Anpassade egenskaper och mått
Om programmet ansluts [anpassade dimensioner (Egenskaper) och anpassade mått](app-insights-api-custom-events-metrics.md#properties) tooevents och du kommer att se dem i hello `customDimensions` och `customMeasurements` objekt.

Till exempel om appen innehåller:

```C#

    var dimensions = new Dictionary<string, string>
                     {{"p1", "v1"},{"p2", "v2"}};
    var measurements = new Dictionary<string, double>
                     {{"m1", 42.0}, {"m2", 43.2}};
    telemetryClient.TrackEvent("myEvent", dimensions, measurements);
```

tooextract värdena i Analytics:

```AIQL

    customEvents
    | extend p1 = customDimensions.p1,
      m1 = todouble(customMeasurements.m1) // cast tooexpected type

```

tooverify om en anpassad dimensionen är av en viss typ:

```AIQL

    customEvents
    | extend p1 = customDimensions.p1,
      iff(notnull(todouble(customMeasurements.m1)), ...
```

## <a name="dashboards"></a>Instrumentpaneler
Du kan fästa resultat tooa instrumentpanelen i ordning toobring ihop alla dina viktigaste diagram och tabeller.

* [Azure delade instrumentpanelen](app-insights-dashboards.md#share-dashboards): Klicka på ikonen för hello PIN-kod. Innan du gör det måste du ha en delad instrumentpanel. Öppna hello Azure-portalen, eller skapa en instrumentpanel och klicka på resursen.
* [Power BI-instrumentpanel](app-insights-export-power-bi.md): Klicka på Exportera, Power BI-fråga. En fördel med detta alternativ är att du kan visa din fråga tillsammans med andra resultat från en mängd olika källor.

## <a name="combine-with-imported-data"></a>Kombinera med importerade data

Analytics-rapporter se bra ut på hello instrumentpanelen, men ibland vill du tootranslate hello data tooa flera datamängder formuläret. Anta exempelvis att din autentiserade användare identifieras i hello telemetri av ett alias. Du vill att tooshow sitt verkliga namn i resultaten. toodo, du behöver en CSV-fil som kopplar från hello-alias toohello verkliga namn.

Du kan importera en fil och använda den precis som alla hello standardtabeller (begäranden, undantag och så vidare). Antingen fråga på sin egen eller Anslut den med andra tabeller. Om du har en tabell med namnet usermap och kolumner har till exempel `realName` och `userId`, kan du använda den tootranslate hello `user_AuthenticatedId` i hello begärandetelemetri:

```AIQL

    requests
    | where notempty(user_AuthenticatedId)
    | project userId = user_AuthenticatedId
      // get hello realName field from hello usermap table:
    | join kind=leftouter ( usermap ) on userId
      // count transactions by name:
    | summarize count() by realName
```

tooimport en tabell i hello schemat bladet under **andra datakällor**, följ hello instruktioner tooadd en ny datakälla genom att överföra ett urval av dina data. Du kan sedan använda den här definitionen tooupload tabeller.

hello importfunktionen genomgår förhandsgranskning för närvarande, så visas först en ”Kontakta oss” länk under ”andra datakällor”. Använd den här toosign in toohello förhandsgranskningsprogrammet och hello länken kommer sedan att ersättas av knappen ”Lägg till ny datakälla”.


## <a name="tables"></a>Tabeller
hello dataström telemetri från din app är tillgängligt via flera tabeller. hello schemat för egenskaper som är tillgängliga för varje tabell är synlig i hello till vänster i fönstret hello.

### <a name="requests-table"></a>Begäranden tabell
Antal HTTP-begäranden tooyour webbapp och segment per sidnamn:

![Antal begäranden segmenterat efter namn](./media/app-insights-analytics-tour/analytics-count-requests.png)

Sök efter hello-begäranden som inte uppfyller de flesta:

![Antal begäranden segmenterat efter namn](./media/app-insights-analytics-tour/analytics-failed-requests.png)

### <a name="custom-events-table"></a>Anpassade händelser tabell
Om du använder [trackevent ()](app-insights-api-custom-events-metrics.md#trackevent) toosend egna händelser, du kan läsa dem i den här tabellen.

Låt oss ta ett exempel där app-koden innehåller dessa rader:

```C#

    telemetry.TrackEvent("Query",
       new Dictionary<string,string> {{"query", sqlCmd}},
       new Dictionary<string,double> {
           {"retry", retryCount},
           {"querytime", totalTime}})
```

Visar hello frekvensen för dessa händelser:

![Visa antal anpassade händelser](./media/app-insights-analytics-tour/analytics-custom-events-rate.png)

Extrahera mått och dimensioner från hello händelser:

![Visa antal anpassade händelser](./media/app-insights-analytics-tour/analytics-custom-events-dimensions.png)

### <a name="custom-metrics-table"></a>Anpassade mått tabell
Om du använder [TrackMetric()](app-insights-api-custom-events-metrics.md#trackmetric) toosend egna måttvärden hittar du resultaten i hello **customMetrics** dataströmmen. Exempel:  

![Anpassade mått i Application Insights analytics](./media/app-insights-analytics-tour/analytics-custom-metrics.png)

> [!NOTE]
> I [Metrics Explorer](app-insights-metrics-explorer.md), alla anpassade mått bifogade tooany telemetri visas tillsammans i hello mått bladet tillsammans med mått som skickas med `TrackMetric()`. Men i Analytics, anpassade mått är fortfarande kopplad toowhichever typ av telemetri som de har gjort på - händelser eller begäranden och så vidare - medan mått som skickats av TrackMetric visas i sina egna dataströmmen.
>
>

### <a name="performance-counters-table"></a>Prestandaräknare tabell
[Prestandaräknare](app-insights-performance-counters.md) visa grundläggande system mätvärden för din app, t.ex CPU eller minne och nätverksanvändning. Du kan konfigurera hello SDK toosend ytterligare räknare, inklusive din egen anpassade räknare.

Hej **performanceCounters** schemat visar hello `category`, `counter` namn, och `instance` namn för varje prestandaräknare. Namn på prestandaräknarinstanser är endast tillämplig toosome prestandaräknare och anger vanligtvis hello namnet på hello process toowhich hello antal avser. I hello telemetri för varje program ser du bara hello räknare för programmet. Till exempel toosee vilka räknare är tillgängliga:

![Prestandaräknare i Application Insights analytics](./media/app-insights-analytics-tour/analytics-performance-counters.png)

tooget ett diagram av minne över hello vald period:

![Minne timechart i Application Insights analytics](./media/app-insights-analytics-tour/analytics-available-memory.png)

Som andra telemetri **performanceCounters** också har en kolumn `cloud_RoleInstance` som visar hello identitet hello värddatorn som appen körs. Till exempel toocompare hello prestandan för din app på hello olika datorer:

![Prestanda åtskilda med rollinstans i Application Insights analytics](./media/app-insights-analytics-tour/analytics-metrics-role-instance.png)

### <a name="exceptions-table"></a>Feltabellen
[Undantag som rapporterats av din app](app-insights-asp-net-exceptions.md) är tillgängliga i den här tabellen.

toofind Hej HTTP-begäran som din app hantering när hello undantagsfel inträffade, delta i operation_Id:

![Ansluta till undantag med begäranden på operation_Id](./media/app-insights-analytics-tour/analytics-exception-request.png)

### <a name="browser-timings-table"></a>Webbläsaren tidsinställningar för tabellen
`browserTimings`Visar sidan Läs in data som samlas in i användarnas webbläsare.

[Konfigurera din app för klientsidan telemetri](app-insights-javascript.md) ordning toosee dessa mått.

hello-schemat innehåller [mått som anger hello längder i olika faser i hello sidan processen för inläsning](app-insights-javascript.md#page-load-performance). (De inte visar hello tidslängd användarna läsa en sida.)  

Visa hello popularities på olika sidor och läsa in tider för varje sida:

![Sidinläsningstider i Analytics](./media/app-insights-analytics-tour/analytics-page-load.png)

### <a name="availability-results-table"></a>Tillgänglighet resultattabellen
`availabilityResults`Visar hello resultatet av dina [webbtester](app-insights-monitor-web-app-availability.md). Varje körning av dina tester från varje test plats rapporteras separat.

![Sidinläsningstider i Analytics](./media/app-insights-analytics-tour/analytics-availability.png)

### <a name="dependencies-table"></a>Beroenden tabell
Innehåller resultatet av anrop som din app gör toodatabases och REST API: er och andra anropar tooTrackDependency(). Även AJAX-anrop från hello webbläsare.

AJAX-anrop från hello webbläsare:

```AIQL

    dependencies | where client_Type == "Browser"
    | take 10
```

Beroendeanrop från hello-servern:

```AIQL

    dependencies | where client_Type == "PC"
    | take 10
```

Beroende på serversidan resultatet visar alltid `success==False` om hello Application Insights Agent är inte installerat. Dock är hello andra data korrekta.

### <a name="traces-table"></a>Spår tabell
Innehåller hello telemetri som skickats av din app att använda TrackTrace(), eller [andra loggning ramverk](app-insights-asp-net-trace-logs.md).

## <a name="video"></a>Video 

> [!VIDEO https://channel9.msdn.com/events/Connect/2016/123/player] 

Avancerade frågor:

> [!VIDEO https://channel9.msdn.com/Events/Build/2016/P591/player]


## <a name="next-steps"></a>Nästa steg
* [Språkreferens för Analytics](app-insights-analytics-reference.md)
* [SQL-användarnas cheat blad](https://aka.ms/sql-analytics) översätter hello vanligaste idioms.

[!INCLUDE [app-insights-analytics-footer](../../includes/app-insights-analytics-footer.md)]
