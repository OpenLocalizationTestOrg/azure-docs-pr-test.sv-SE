---
title: En rundtur via analyser i Azure Application Insights | Microsoft Docs
description: "Kort prover av alla huvudsakliga frågor i Analytics, kraftfullt sökverktyg av Application Insights."
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
ms.openlocfilehash: f5650d212eb2f8c460f062b3c11ae14c1e026ba6
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/18/2017
---
# <a name="a-tour-of-analytics-in-application-insights"></a><span data-ttu-id="e4b11-103">En genomgång av Analytics i Application Insights</span><span class="sxs-lookup"><span data-stu-id="e4b11-103">A tour of Analytics in Application Insights</span></span>
<span data-ttu-id="e4b11-104">[Analytics](app-insights-analytics.md) är kraftfull sökfunktionen i [Programinsikter](app-insights-overview.md).</span><span class="sxs-lookup"><span data-stu-id="e4b11-104">[Analytics](app-insights-analytics.md) is the powerful search feature of [Application Insights](app-insights-overview.md).</span></span> <span data-ttu-id="e4b11-105">Dessa sidor beskrivs Log Analytics-frågespråket.</span><span class="sxs-lookup"><span data-stu-id="e4b11-105">These pages describe the Log Analytics query language.</span></span>

* <span data-ttu-id="e4b11-106">**[Titta på inledande videon](https://applicationanalytics-media.azureedge.net/home_page_video.mp4)**.</span><span class="sxs-lookup"><span data-stu-id="e4b11-106">**[Watch the introductory video](https://applicationanalytics-media.azureedge.net/home_page_video.mp4)**.</span></span>
* <span data-ttu-id="e4b11-107">**[Testkör Analytics på våra simulerade data](https://analytics.applicationinsights.io/demo)**  om din app inte skickar data till Application Insights ännu.</span><span class="sxs-lookup"><span data-stu-id="e4b11-107">**[Test drive Analytics on our simulated data](https://analytics.applicationinsights.io/demo)** if your app isn't sending data to Application Insights yet.</span></span>
* <span data-ttu-id="e4b11-108">**[SQL-användarnas cheat blad](https://aka.ms/sql-analytics)**  översätter vanligaste idioms.</span><span class="sxs-lookup"><span data-stu-id="e4b11-108">**[SQL-users' cheat sheet](https://aka.ms/sql-analytics)** translates the most common idioms.</span></span>

<span data-ttu-id="e4b11-109">Låt oss ta ett gå igenom några enkla frågor för att komma igång.</span><span class="sxs-lookup"><span data-stu-id="e4b11-109">Let's take a walk through some basic queries to get you started.</span></span>

## <a name="connect-to-your-application-insights-data"></a><span data-ttu-id="e4b11-110">Anslut till Application Insights-data</span><span class="sxs-lookup"><span data-stu-id="e4b11-110">Connect to your Application Insights data</span></span>
<span data-ttu-id="e4b11-111">Öppna Analytics från din app [översikt bladet](app-insights-dashboards.md) i Application Insights:</span><span class="sxs-lookup"><span data-stu-id="e4b11-111">Open Analytics from your app's [overview blade](app-insights-dashboards.md) in Application Insights:</span></span>

![Öppna portal.azure.com, öppna Application Insights-resursen och klicka på Analytics.](./media/app-insights-analytics-tour/001.png)

## <a name="takehttpsdocsloganalyticsioquerylanguagequerylanguagetakeoperatorhtml-show-me-n-rows"></a><span data-ttu-id="e4b11-113">[Ta](https://docs.loganalytics.io/queryLanguage/query_language_takeoperator.html): Visa n rader</span><span class="sxs-lookup"><span data-stu-id="e4b11-113">[Take](https://docs.loganalytics.io/queryLanguage/query_language_takeoperator.html): show me n rows</span></span>
<span data-ttu-id="e4b11-114">Datapunkter som loggar användaren operations (vanligtvis HTTP-begäranden tas emot av ditt webbprogram) lagras i en tabell som kallas `requests`.</span><span class="sxs-lookup"><span data-stu-id="e4b11-114">Data points that log user operations (typically HTTP requests received by your web app) are stored in a table called `requests`.</span></span> <span data-ttu-id="e4b11-115">Varje rad är en telemetri data togs emot från Application Insights SDK i din app.</span><span class="sxs-lookup"><span data-stu-id="e4b11-115">Each row is a telemetry data point received from the Application Insights SDK in your app.</span></span>

<span data-ttu-id="e4b11-116">Låt oss börja med att undersöka några exempel raderna i tabellen:</span><span class="sxs-lookup"><span data-stu-id="e4b11-116">Let's start by examining a few sample rows of the table:</span></span>

![resultat](./media/app-insights-analytics-tour/010.png)

> [!NOTE]
> <span data-ttu-id="e4b11-118">Placera markören någonstans i instruktionen innan du klickar på OK.</span><span class="sxs-lookup"><span data-stu-id="e4b11-118">Put the cursor somewhere in the statement before you click Go.</span></span> <span data-ttu-id="e4b11-119">Du kan dela en instruktion över flera rader, men Placera inte tomma rader i en instruktion.</span><span class="sxs-lookup"><span data-stu-id="e4b11-119">You can split a statement over more than one line, but don't put blank lines in a statement.</span></span> <span data-ttu-id="e4b11-120">Tomma rader är ett praktiskt sätt att hålla flera separata frågor i fönstret.</span><span class="sxs-lookup"><span data-stu-id="e4b11-120">Blank lines are a convenient way to keep several separate queries in the window.</span></span>
>
>

<span data-ttu-id="e4b11-121">Välj kolumner, drar du dem, gruppera efter kolumner, och filtrera:</span><span class="sxs-lookup"><span data-stu-id="e4b11-121">Choose columns, drag them, group by columns, and filter:</span></span>

![Klicka på kolumnmarkering i övre högra hörnet av resultat](./media/app-insights-analytics-tour/030.png)

<span data-ttu-id="e4b11-123">Expandera alla objekt om du vill se detaljerad information:</span><span class="sxs-lookup"><span data-stu-id="e4b11-123">Expand any item to see the detail:</span></span>

![Välj tabellen och konfigurera kolumner](./media/app-insights-analytics-tour/040.png)

> [!NOTE]
> <span data-ttu-id="e4b11-125">Klicka på huvudet för en kolumn för att ordna om resultat som är tillgängliga i webbläsaren.</span><span class="sxs-lookup"><span data-stu-id="e4b11-125">Click the head of a column to re-order the results available in the web browser.</span></span> <span data-ttu-id="e4b11-126">Men tänk på att antalet rader som hämtas till webbläsaren för en stor resultatmängd är begränsad.</span><span class="sxs-lookup"><span data-stu-id="e4b11-126">But be aware that for a large result set, the number of rows downloaded to the browser is limited.</span></span> <span data-ttu-id="e4b11-127">Sortering därmed visar inte alltid de faktiska högsta eller lägsta artiklarna.</span><span class="sxs-lookup"><span data-stu-id="e4b11-127">Sorting this way doesn't always show you the actual highest or lowest items.</span></span> <span data-ttu-id="e4b11-128">Om du vill sortera objekt på ett tillförlitligt sätt att använda den `top` eller `sort` operator.</span><span class="sxs-lookup"><span data-stu-id="e4b11-128">To sort items reliably, use the `top` or `sort` operator.</span></span>
>
>

## <a name="tophttpsdocsloganalyticsioquerylanguagequerylanguagetopoperatorhtml-and-sorthttpsdocsloganalyticsioquerylanguagequerylanguagesortoperatorhtml"></a><span data-ttu-id="e4b11-129">[TOP](https://docs.loganalytics.io/queryLanguage/query_language_topoperator.html) och [sortera](https://docs.loganalytics.io/queryLanguage/query_language_sortoperator.html)</span><span class="sxs-lookup"><span data-stu-id="e4b11-129">[Top](https://docs.loganalytics.io/queryLanguage/query_language_topoperator.html) and [sort](https://docs.loganalytics.io/queryLanguage/query_language_sortoperator.html)</span></span>
<span data-ttu-id="e4b11-130">`take`är användbar för att få en snabb exempel på ett resultat, men den visar rader från tabellen i bokstavsordning.</span><span class="sxs-lookup"><span data-stu-id="e4b11-130">`take` is useful to get a quick sample of a result, but it shows rows from the table in no particular order.</span></span> <span data-ttu-id="e4b11-131">För att få en ordnad vy kan du använda `top` (till exempel) eller `sort` (över hela tabellen).</span><span class="sxs-lookup"><span data-stu-id="e4b11-131">To get an ordered view, use `top` (for a sample) or `sort` (over the whole table).</span></span>

<span data-ttu-id="e4b11-132">Visa de första n raderna, sorterade efter en viss kolumn:</span><span class="sxs-lookup"><span data-stu-id="e4b11-132">Show me the first n rows, ordered by a particular column:</span></span>

```AIQL

    requests | top 10 by timestamp desc
```

* <span data-ttu-id="e4b11-133">*Syntax:* de flesta operatörer har nyckelordet parametrar som `by`.</span><span class="sxs-lookup"><span data-stu-id="e4b11-133">*Syntax:* Most operators have keyword parameters such as `by`.</span></span>
* <span data-ttu-id="e4b11-134">`desc`= fallande ordning, `asc` = stigande.</span><span class="sxs-lookup"><span data-stu-id="e4b11-134">`desc` = descending order, `asc` = ascending.</span></span>

![](./media/app-insights-analytics-tour/260.png)

<span data-ttu-id="e4b11-135">`top...`är ett mer performant sätt att säga `sort ... | take...`.</span><span class="sxs-lookup"><span data-stu-id="e4b11-135">`top...` is a more performant way of saying `sort ... | take...`.</span></span> <span data-ttu-id="e4b11-136">Vi kan skriva:</span><span class="sxs-lookup"><span data-stu-id="e4b11-136">We could have written:</span></span>

```AIQL

    requests | sort by timestamp desc | take 10
```

<span data-ttu-id="e4b11-137">Resultatet blir detsamma, men körs det lite långsammare.</span><span class="sxs-lookup"><span data-stu-id="e4b11-137">The result would be the same, but it would run a bit more slowly.</span></span> <span data-ttu-id="e4b11-138">(Du kan också skriva `order`, vilket är ett alias för `sort`.)</span><span class="sxs-lookup"><span data-stu-id="e4b11-138">(You could also write `order`, which is an alias of `sort`.)</span></span>

<span data-ttu-id="e4b11-139">Kolumnrubrikerna i tabellvyn kan också användas för att sortera resultaten på skärmen.</span><span class="sxs-lookup"><span data-stu-id="e4b11-139">The column headers in the table view can also be used to sort the results on the screen.</span></span> <span data-ttu-id="e4b11-140">Men av kursen, om du har använt `take` eller `top` för att hämta bara en del av en tabell, du får endast ordna poster som du har hämtat.</span><span class="sxs-lookup"><span data-stu-id="e4b11-140">But of course, if you've used `take` or `top` to retrieve just part of a table, you'll only re-order the records you've retrieved.</span></span>

## <a name="wherehttpsdocsloganalyticsioquerylanguagequerylanguagewhereoperatorhtml-filtering-on-a-condition"></a><span data-ttu-id="e4b11-141">[Där](https://docs.loganalytics.io/queryLanguage/query_language_whereoperator.html): filtrering på ett villkor</span><span class="sxs-lookup"><span data-stu-id="e4b11-141">[Where](https://docs.loganalytics.io/queryLanguage/query_language_whereoperator.html): filtering on a condition</span></span>

<span data-ttu-id="e4b11-142">Låt oss se bara begäranden som returnerade en viss Resultatkod:</span><span class="sxs-lookup"><span data-stu-id="e4b11-142">Let's see just requests that returned a particular result code:</span></span>

```AIQL

    requests
    | where resultCode  == "404"
    | take 10
```

![](./media/app-insights-analytics-tour/250.png)

<span data-ttu-id="e4b11-143">Den `where` operator tar ett booleskt uttryck.</span><span class="sxs-lookup"><span data-stu-id="e4b11-143">The `where` operator takes a Boolean expression.</span></span> <span data-ttu-id="e4b11-144">Här följer några huvudpunkter om dem:</span><span class="sxs-lookup"><span data-stu-id="e4b11-144">Here are some key points about them:</span></span>

* <span data-ttu-id="e4b11-145">`and`, `or`: Booleska operatorer</span><span class="sxs-lookup"><span data-stu-id="e4b11-145">`and`, `or`: Boolean operators</span></span>
* <span data-ttu-id="e4b11-146">`==`, `<>`, `!=` : lika med och inte lika med</span><span class="sxs-lookup"><span data-stu-id="e4b11-146">`==`, `<>`, `!=` : equal and not equal</span></span>
* <span data-ttu-id="e4b11-147">`=~`, `!~` : skiftlägeskänsliga sträng lika och inte lika.</span><span class="sxs-lookup"><span data-stu-id="e4b11-147">`=~`, `!~` : case-insensitive string equal and not equal.</span></span> <span data-ttu-id="e4b11-148">Det finns många fler sträng jämförelseoperatorer.</span><span class="sxs-lookup"><span data-stu-id="e4b11-148">There are lots more string comparison operators.</span></span>

<!---Read all about [scalar expressions]().--->

### <a name="getting-the-right-type"></a><span data-ttu-id="e4b11-149">Hämtning av rätt typ</span><span class="sxs-lookup"><span data-stu-id="e4b11-149">Getting the right type</span></span>
<span data-ttu-id="e4b11-150">Sök efter misslyckade begäranden:</span><span class="sxs-lookup"><span data-stu-id="e4b11-150">Find unsuccessful requests:</span></span>

```AIQL

    requests
    | where isnotempty(resultCode) and toint(resultCode) >= 400
```
<!---
`resultCode` has type string, so we must cast it app-insights-analytics-reference.md#casts for a numeric comparison.
--->

## <a name="time"></a><span data-ttu-id="e4b11-151">Tid</span><span class="sxs-lookup"><span data-stu-id="e4b11-151">Time</span></span>

<span data-ttu-id="e4b11-152">Som standard är dina frågor begränsade till den senaste 24 timmarna.</span><span class="sxs-lookup"><span data-stu-id="e4b11-152">By default, your queries are restricted to the last 24 hours.</span></span> <span data-ttu-id="e4b11-153">Men du kan ändra detta intervall:</span><span class="sxs-lookup"><span data-stu-id="e4b11-153">But you can change this range:</span></span>

![](./media/app-insights-analytics-tour/change-time-range.png)

<span data-ttu-id="e4b11-154">Åsidosätt tidsintervallet genom att skriva en fråga som nämns `timestamp` i en where-sats.</span><span class="sxs-lookup"><span data-stu-id="e4b11-154">Override the time range by writing any query that mentions `timestamp` in a where-clause.</span></span> <span data-ttu-id="e4b11-155">Exempel:</span><span class="sxs-lookup"><span data-stu-id="e4b11-155">For example:</span></span>

```AIQL

    // What were the slowest requests over the past 3 days?
    requests
    | where timestamp > ago(3d)  // Override the time range
    | top 5 by duration
```

<span data-ttu-id="e4b11-156">Funktionen tid intervallet motsvarar en 'where'-sats läggas till efter varje uppgift om något av källtabellerna.</span><span class="sxs-lookup"><span data-stu-id="e4b11-156">The time range feature is equivalent to a 'where' clause inserted after each mention of one of the source tables.</span></span>

<span data-ttu-id="e4b11-157">`ago(3d)`innebär 'tre dagar sedan'.</span><span class="sxs-lookup"><span data-stu-id="e4b11-157">`ago(3d)` means 'three days ago'.</span></span> <span data-ttu-id="e4b11-158">Andra tidsenheter inkludera timmar (`2h`, `2.5h`), minuter (`25m`), och sekunder (`10s`).</span><span class="sxs-lookup"><span data-stu-id="e4b11-158">Other units of time include hours (`2h`, `2.5h`), minutes (`25m`), and seconds (`10s`).</span></span>

<span data-ttu-id="e4b11-159">Andra exempel:</span><span class="sxs-lookup"><span data-stu-id="e4b11-159">Other examples:</span></span>

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

<span data-ttu-id="e4b11-160">[Datum och tider referens](https://docs.loganalytics.io/concepts/concepts_datatypes_datetime.html).</span><span class="sxs-lookup"><span data-stu-id="e4b11-160">[Dates and times reference](https://docs.loganalytics.io/concepts/concepts_datatypes_datetime.html).</span></span>


## <a name="projecthttpsdocsloganalyticsioquerylanguagequerylanguageprojectoperatorhtml-select-rename-and-compute-columns"></a><span data-ttu-id="e4b11-161">[Projektet](https://docs.loganalytics.io/queryLanguage/query_language_projectoperator.html): Välj, byta namn på och bearbetning kolumner</span><span class="sxs-lookup"><span data-stu-id="e4b11-161">[Project](https://docs.loganalytics.io/queryLanguage/query_language_projectoperator.html): select, rename, and compute columns</span></span>
<span data-ttu-id="e4b11-162">Använd [ `project` ](https://docs.loganalytics.io/queryLanguage/query_language_projectoperator.html) att välja ut bara de kolumner som du vill:</span><span class="sxs-lookup"><span data-stu-id="e4b11-162">Use [`project`](https://docs.loganalytics.io/queryLanguage/query_language_projectoperator.html) to pick out just the columns you want:</span></span>

```AIQL

    requests | top 10 by timestamp desc
             | project timestamp, name, resultCode
```

![](./media/app-insights-analytics-tour/240.png)

<span data-ttu-id="e4b11-163">Du kan också byta namn på kolumner och definiera nya:</span><span class="sxs-lookup"><span data-stu-id="e4b11-163">You can also rename columns and define new ones:</span></span>

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

* <span data-ttu-id="e4b11-165">Kolumnnamn kan innehålla blanksteg eller symboler om de omges så här: `['...']` eller`["..."]`</span><span class="sxs-lookup"><span data-stu-id="e4b11-165">Column names can include spaces or symbols if they are bracketed like this: `['...']` or `["..."]`</span></span>
* <span data-ttu-id="e4b11-166">`%`är den vanliga modulo-operatorn.</span><span class="sxs-lookup"><span data-stu-id="e4b11-166">`%` is the usual modulo operator.</span></span>
* <span data-ttu-id="e4b11-167">`1d`(som är en siffra, en hade ”) är en literal timespan vilket innebär en dag.</span><span class="sxs-lookup"><span data-stu-id="e4b11-167">`1d` (that's a digit one, then a 'd') is a timespan literal meaning one day.</span></span> <span data-ttu-id="e4b11-168">Här följer några fler timespan-litteraler: `12h`, `30m`, `10s`, `0.01s`.</span><span class="sxs-lookup"><span data-stu-id="e4b11-168">Here are some more timespan literals: `12h`, `30m`, `10s`, `0.01s`.</span></span>
* <span data-ttu-id="e4b11-169">`floor`(alias `bin`) Avrundar värdet nedåt till närmaste signifikanta grundläggande värdet du anger.</span><span class="sxs-lookup"><span data-stu-id="e4b11-169">`floor` (alias `bin`) rounds a value down to the nearest multiple of the base value you provide.</span></span> <span data-ttu-id="e4b11-170">Så `floor(aTime, 1s)` avrundas nedåt till närmaste andra gången.</span><span class="sxs-lookup"><span data-stu-id="e4b11-170">So `floor(aTime, 1s)` rounds a time down to the nearest second.</span></span>

<span data-ttu-id="e4b11-171">Uttryck kan innehålla alla vanliga operatorer (`+`, `-`,...), och det finns en uppsättning praktiska funktioner.</span><span class="sxs-lookup"><span data-stu-id="e4b11-171">Expressions can include all the usual operators (`+`, `-`, ...), and there's a range of useful functions.</span></span>

## <a name="extend"></a><span data-ttu-id="e4b11-172">Utöka</span><span class="sxs-lookup"><span data-stu-id="e4b11-172">Extend</span></span>
<span data-ttu-id="e4b11-173">Om du vill lägga till kolumner i befintliga använda [ `extend` ](https://docs.loganalytics.io/queryLanguage/query_language_extendoperator.html):</span><span class="sxs-lookup"><span data-stu-id="e4b11-173">If you just want to add columns to the existing ones, use [`extend`](https://docs.loganalytics.io/queryLanguage/query_language_extendoperator.html):</span></span>

```AIQL

    requests
    | top 10 by timestamp desc
    | extend timeOfDay = floor(timestamp % 1d, 1s)
```

<span data-ttu-id="e4b11-174">Med hjälp av [ `extend` ](https://docs.loganalytics.io/queryLanguage/query_language_extendoperator.html) är mindre utförlig än [ `project` ](https://docs.loganalytics.io/queryLanguage/query_language_projectoperator.html) om du vill behålla de befintliga kolumnerna.</span><span class="sxs-lookup"><span data-stu-id="e4b11-174">Using [`extend`](https://docs.loganalytics.io/queryLanguage/query_language_extendoperator.html) is less verbose than [`project`](https://docs.loganalytics.io/queryLanguage/query_language_projectoperator.html) if you want to keep all the existing columns.</span></span>

### <a name="convert-to-local-time"></a><span data-ttu-id="e4b11-175">Omvandla till lokal tid</span><span class="sxs-lookup"><span data-stu-id="e4b11-175">Convert to local time</span></span>

<span data-ttu-id="e4b11-176">Tidsstämplar är alltid i UTC.</span><span class="sxs-lookup"><span data-stu-id="e4b11-176">Timestamps are always in UTC.</span></span> <span data-ttu-id="e4b11-177">Så om du är på oss Pacific kusten och det är vinter, kan du så här:</span><span class="sxs-lookup"><span data-stu-id="e4b11-177">So if you're on the US Pacific coast and it's winter, you might like this:</span></span>

```AIQL

    requests
    | top 10 by timestamp desc
    | extend localTime = timestamp - 8h
```


## <a name="summarizehttpsdocsloganalyticsioquerylanguagequerylanguagesummarizeoperatorhtml-aggregate-groups-of-rows"></a><span data-ttu-id="e4b11-178">[Sammanfatta](https://docs.loganalytics.io/queryLanguage/query_language_summarizeoperator.html): aggregera grupper av rader</span><span class="sxs-lookup"><span data-stu-id="e4b11-178">[Summarize](https://docs.loganalytics.io/queryLanguage/query_language_summarizeoperator.html): aggregate groups of rows</span></span>
<span data-ttu-id="e4b11-179">`Summarize`tillämpar en angiven *aggregeringsfunktionen* över grupper av rader.</span><span class="sxs-lookup"><span data-stu-id="e4b11-179">`Summarize` applies a specified *aggregation function* over groups of rows.</span></span>

<span data-ttu-id="e4b11-180">Till exempel överföringstiden för ditt webbprogram svara på en begäran rapporteras i fältet `duration`.</span><span class="sxs-lookup"><span data-stu-id="e4b11-180">For example, the time your web app takes to respond to a request is reported in the field `duration`.</span></span> <span data-ttu-id="e4b11-181">Låt oss se genomsnittlig svarstid för alla begäranden:</span><span class="sxs-lookup"><span data-stu-id="e4b11-181">Let's see the average response time to all requests:</span></span>

![](./media/app-insights-analytics-tour/410.png)

<span data-ttu-id="e4b11-182">Eller kan vi dela resultatet i förfrågningar av olika namn:</span><span class="sxs-lookup"><span data-stu-id="e4b11-182">Or we could separate the result into requests of different names:</span></span>

![](./media/app-insights-analytics-tour/420.png)

<span data-ttu-id="e4b11-183">`Summarize`samlar in datapunkter i dataströmmen i grupper som den `by` satsen utvärderar lika.</span><span class="sxs-lookup"><span data-stu-id="e4b11-183">`Summarize` collects the data points in the stream into groups for which the `by` clause evaluates equally.</span></span> <span data-ttu-id="e4b11-184">Varje värde i den `by` uttryck - varje åtgärdsnamn i exemplet ovan - resulterar i en rad i resultattabellen.</span><span class="sxs-lookup"><span data-stu-id="e4b11-184">Each value in the `by` expression - each operation name in the above example - results in a row in the result table.</span></span>

<span data-ttu-id="e4b11-185">Eller vi gick grupperar resultaten efter tid på dagen:</span><span class="sxs-lookup"><span data-stu-id="e4b11-185">Or we could group results by time of day:</span></span>

![](./media/app-insights-analytics-tour/430.png)

<span data-ttu-id="e4b11-186">Observera hur vi använder den `bin` funktionen (även kallat `floor`).</span><span class="sxs-lookup"><span data-stu-id="e4b11-186">Notice how we're using the `bin` function (aka `floor`).</span></span> <span data-ttu-id="e4b11-187">Om vi använde `by timestamp`, varje inkommande rad skulle hamna i en egen liten grupp.</span><span class="sxs-lookup"><span data-stu-id="e4b11-187">If we just used `by timestamp`, every input row would end up in its own little group.</span></span> <span data-ttu-id="e4b11-188">För en kontinuerlig skalära som tidsvärden eller siffror som vi behöver Bryt kontinuerligt område i en hanterbar antal diskreta värden.</span><span class="sxs-lookup"><span data-stu-id="e4b11-188">For any continuous scalar like times or numbers, we have to break the continuous range into a manageable number of discrete values.</span></span> <span data-ttu-id="e4b11-189">`bin`-som är bara det gamla avrundas nedåt `floor` fungerar - är det enklaste sättet att göra detta.</span><span class="sxs-lookup"><span data-stu-id="e4b11-189">`bin` - which is just the familiar rounding-down `floor` function - is the easiest way to do that.</span></span>

<span data-ttu-id="e4b11-190">Vi kan använda samma metod för att minska intervall i strängar:</span><span class="sxs-lookup"><span data-stu-id="e4b11-190">We can use the same technique to reduce ranges of strings:</span></span>

![](./media/app-insights-analytics-tour/440.png)

<span data-ttu-id="e4b11-191">Observera att du kan använda `name=` att ange namnet på en resultatkolumn, antingen i mängduttryck eller av-satsen.</span><span class="sxs-lookup"><span data-stu-id="e4b11-191">Notice that you can use `name=` to set the name of a result column, either in the aggregation expressions or the by-clause.</span></span>

## <a name="counting-sampled-data"></a><span data-ttu-id="e4b11-192">Cyklisk exempeldata</span><span class="sxs-lookup"><span data-stu-id="e4b11-192">Counting sampled data</span></span>
<span data-ttu-id="e4b11-193">`sum(itemCount)`är aggregeringen rekommenderade att räkna händelser.</span><span class="sxs-lookup"><span data-stu-id="e4b11-193">`sum(itemCount)` is the recommended aggregation to count events.</span></span> <span data-ttu-id="e4b11-194">I många fall itemCount == 1, så funktionen bara räknar antalet rader i gruppen.</span><span class="sxs-lookup"><span data-stu-id="e4b11-194">In many cases, itemCount==1, so the function simply counts up the number of rows in the group.</span></span> <span data-ttu-id="e4b11-195">Men när [provtagning](app-insights-sampling.md) är i drift, bara en del av de ursprungliga händelserna behålls som datapunkter i Application Insights, så att det finns för varje datapunkt som du ser, `itemCount` händelser.</span><span class="sxs-lookup"><span data-stu-id="e4b11-195">But when [sampling](app-insights-sampling.md) is in operation, only a fraction of the original events are retained as data points in Application Insights, so that for each data point you see, there are `itemCount` events.</span></span>

<span data-ttu-id="e4b11-196">Till exempel om sampling ignorerar 75% av den ursprungliga händelser och sedan itemCount == 4 i kvarhållna poster -, för varje kvarhållna post det var fyra ursprungliga poster.</span><span class="sxs-lookup"><span data-stu-id="e4b11-196">For example, if sampling discards 75% of the original events, then itemCount==4 in the retained records - that is, for every retained record, there were four original records.</span></span>

<span data-ttu-id="e4b11-197">Anpassningsbar provtagning gör itemCount vara högre under perioder när ditt program som ofta används.</span><span class="sxs-lookup"><span data-stu-id="e4b11-197">Adaptive sampling causes itemCount to be higher during periods when your application is being heavily used.</span></span>

<span data-ttu-id="e4b11-198">Sammanfattningsvis itemCount därför ger en bra uppskattning av det ursprungliga antalet händelser.</span><span class="sxs-lookup"><span data-stu-id="e4b11-198">Summing up itemCount therefore gives a good estimate of the original number of events.</span></span>

![](./media/app-insights-analytics-tour/510.png)

<span data-ttu-id="e4b11-199">Det finns också en `count()` aggregering (och ett antal åtgärden) i de fall där du verkligen vill räkna antalet rader i en grupp.</span><span class="sxs-lookup"><span data-stu-id="e4b11-199">There's also a `count()` aggregation (and a count operation), for cases where you really do want to count the number of rows in a group.</span></span>

<span data-ttu-id="e4b11-200">Det finns en mängd [aggregeringsfunktioner](https://docs.loganalytics.io/learn/tutorials/aggregations.html).</span><span class="sxs-lookup"><span data-stu-id="e4b11-200">There's a range of [aggregation functions](https://docs.loganalytics.io/learn/tutorials/aggregations.html).</span></span>

## <a name="charting-the-results"></a><span data-ttu-id="e4b11-201">Diagram resultaten</span><span class="sxs-lookup"><span data-stu-id="e4b11-201">Charting the results</span></span>
```AIQL

    exceptions
       | summarize count=sum(itemCount)  
         by bin(timestamp, 1h)
```

<span data-ttu-id="e4b11-202">Resultatet visas som en tabell som standard:</span><span class="sxs-lookup"><span data-stu-id="e4b11-202">By default, results display as a table:</span></span>

![](./media/app-insights-analytics-tour/225.png)

<span data-ttu-id="e4b11-203">Vi kan göra bättre än tabellvyn.</span><span class="sxs-lookup"><span data-stu-id="e4b11-203">We can do better than the table view.</span></span> <span data-ttu-id="e4b11-204">Nu ska vi titta på resultaten i schemat med lodrätt staplar alternativet:</span><span class="sxs-lookup"><span data-stu-id="e4b11-204">Let's look at the results in the chart view with the vertical bar option:</span></span>

![Klicka på diagrammet och sedan välja stående stapeldiagram och tilldela x och y axlar](./media/app-insights-analytics-tour/230.png)

<span data-ttu-id="e4b11-206">Observera att även om vi inte sortera resultaten av tid (som du ser i tabellen visas), diagram alltid visas datum och tid i rätt ordning.</span><span class="sxs-lookup"><span data-stu-id="e4b11-206">Notice that although we didn't sort the results by time (as you can see in the table display), the chart display always shows datetimes in correct order.</span></span>


## <a name="timecharts"></a><span data-ttu-id="e4b11-207">Timecharts</span><span class="sxs-lookup"><span data-stu-id="e4b11-207">Timecharts</span></span>
<span data-ttu-id="e4b11-208">Visa det hur många händelser är varje timma:</span><span class="sxs-lookup"><span data-stu-id="e4b11-208">Show how many events there are each hour:</span></span>

```AIQL

    requests
      | summarize event_count=sum(itemCount)
        by bin(timestamp, 1h)
```

<span data-ttu-id="e4b11-209">Välj Visningsalternativ för diagram:</span><span class="sxs-lookup"><span data-stu-id="e4b11-209">Select the Chart display option:</span></span>

![timechart](./media/app-insights-analytics-tour/080.png)

## <a name="multiple-series"></a><span data-ttu-id="e4b11-211">Flera serier</span><span class="sxs-lookup"><span data-stu-id="e4b11-211">Multiple series</span></span>
<span data-ttu-id="e4b11-212">Flera uttryck i den `summarize` satsen skapar flera kolumner.</span><span class="sxs-lookup"><span data-stu-id="e4b11-212">Multiple expressions in the `summarize` clause creates multiple columns.</span></span>

<span data-ttu-id="e4b11-213">Flera uttryck i den `by` satsen skapar flera rader, ett för varje kombination av värden.</span><span class="sxs-lookup"><span data-stu-id="e4b11-213">Multiple expressions in the `by` clause creates multiple rows, one for each combination of values.</span></span>

```AIQL

    requests
    | summarize count_=sum(itemCount), avg(duration)
      by bin(timestamp, 1h), client_StateOrProvince, client_City
    | order by timestamp asc, client_StateOrProvince, client_City
```

![Tabell för förfrågningar efter timme och plats](./media/app-insights-analytics-tour/090.png)

### <a name="segment-a-chart-by-dimensions"></a><span data-ttu-id="e4b11-215">Segmentera ett diagram av dimensioner</span><span class="sxs-lookup"><span data-stu-id="e4b11-215">Segment a chart by dimensions</span></span>
<span data-ttu-id="e4b11-216">Om du skapa diagram över en tabell som har en strängkolumn och en numerisk kolumn användas strängen för att dela upp de numeriska data i separata antal punkter.</span><span class="sxs-lookup"><span data-stu-id="e4b11-216">If you chart a table that has a string column and a numeric column, the string can be used to split the numeric data into separate series of points.</span></span> <span data-ttu-id="e4b11-217">Om det finns fler än en strängkolumn, kan du välja vilken kolumn som ska användas som diskriminatorn.</span><span class="sxs-lookup"><span data-stu-id="e4b11-217">If there's more than one string column, you can choose which column to use as the discriminator.</span></span>

![Segmentera ett analytics-diagram](./media/app-insights-analytics-tour/100.png)

#### <a name="bounce-rate"></a><span data-ttu-id="e4b11-219">Studs hastighet</span><span class="sxs-lookup"><span data-stu-id="e4b11-219">Bounce rate</span></span>

<span data-ttu-id="e4b11-220">Konvertera ett booleskt värde till en sträng som ska användas som en diskriminator:</span><span class="sxs-lookup"><span data-stu-id="e4b11-220">Convert a boolean to a string to use it as a discriminator:</span></span>

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

### <a name="display-multiple-metrics"></a><span data-ttu-id="e4b11-221">Visa flera mått</span><span class="sxs-lookup"><span data-stu-id="e4b11-221">Display multiple metrics</span></span>
<span data-ttu-id="e4b11-222">Om diagram av en tabell som har mer än en numerisk kolumn, förutom tidsstämpeln, kan du visa en kombination av dessa.</span><span class="sxs-lookup"><span data-stu-id="e4b11-222">If you chart a table that has more than one numeric column, in addition to the timestamp, you can display any combination of them.</span></span>

![Segmentera ett analytics-diagram](./media/app-insights-analytics-tour/110.png)

<span data-ttu-id="e4b11-224">Du måste välja **inte dela** innan du kan välja flera numeriska kolumner.</span><span class="sxs-lookup"><span data-stu-id="e4b11-224">You must select **Don't Split** before you can select multiple numeric columns.</span></span> <span data-ttu-id="e4b11-225">Du kan inte dela genom en strängkolumn samtidigt som visar mer än en numerisk kolumn.</span><span class="sxs-lookup"><span data-stu-id="e4b11-225">You can't split by a string column at the same time as displaying more than one numeric column.</span></span>

## <a name="daily-average-cycle"></a><span data-ttu-id="e4b11-226">Dagliga genomsnittliga cykel</span><span class="sxs-lookup"><span data-stu-id="e4b11-226">Daily average cycle</span></span>
<span data-ttu-id="e4b11-227">Hur varierar användning under den genomsnittliga dagen?</span><span class="sxs-lookup"><span data-stu-id="e4b11-227">How does usage vary over the average day?</span></span>

<span data-ttu-id="e4b11-228">Antal begäranden när modulo en dag binned i timmar:</span><span class="sxs-lookup"><span data-stu-id="e4b11-228">Count requests by the time modulo one day, binned into hours:</span></span>

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
> <span data-ttu-id="e4b11-230">Observera att för närvarande har vi att konvertera tidsvaraktigheter till datum och tid för att visa på ett linjediagram.</span><span class="sxs-lookup"><span data-stu-id="e4b11-230">Notice that we currently have to convert time durations to datetimes in order to display on a line chart.</span></span>
>
>

## <a name="compare-multiple-daily-series"></a><span data-ttu-id="e4b11-231">Jämför flera dagliga serier</span><span class="sxs-lookup"><span data-stu-id="e4b11-231">Compare multiple daily series</span></span>
<span data-ttu-id="e4b11-232">Hur användning varierar över tid på dagen i olika länder?</span><span class="sxs-lookup"><span data-stu-id="e4b11-232">How does usage vary over the time of day in different countries?</span></span>

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

## <a name="plot-a-distribution"></a><span data-ttu-id="e4b11-234">Rita en distributionsplats</span><span class="sxs-lookup"><span data-stu-id="e4b11-234">Plot a distribution</span></span>
<span data-ttu-id="e4b11-235">Hur många sessioner är det av olika längd?</span><span class="sxs-lookup"><span data-stu-id="e4b11-235">How many sessions are there of different lengths?</span></span>

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

<span data-ttu-id="e4b11-236">Den sista raden krävs för att konvertera till datetime.</span><span class="sxs-lookup"><span data-stu-id="e4b11-236">The last line is required to convert to datetime.</span></span> <span data-ttu-id="e4b11-237">För närvarande visas x-axeln i ett diagram som en skalär endast om det är ett datetime-värde.</span><span class="sxs-lookup"><span data-stu-id="e4b11-237">Currently the x axis of a chart is displayed as a scalar only if it is a datetime.</span></span>

<span data-ttu-id="e4b11-238">Den `where` satsen utesluter one-shot sessioner (sessionDuration == 0) och anger längden på x-axeln.</span><span class="sxs-lookup"><span data-stu-id="e4b11-238">The `where` clause excludes one-shot sessions (sessionDuration==0) and sets the length of the x-axis.</span></span>

![](./media/app-insights-analytics-tour/290.png)

## <a name="percentileshttpsdocsloganalyticsioquerylanguagequerylanguagepercentilesaggfunctionhtml"></a>[<span data-ttu-id="e4b11-239">Percentiler</span><span class="sxs-lookup"><span data-stu-id="e4b11-239">Percentiles</span></span>](https://docs.loganalytics.io/queryLanguage/query_language_percentiles_aggfunction.html)
<span data-ttu-id="e4b11-240">Vilka områden för varaktigheter täcka olika delar av sessioner?</span><span class="sxs-lookup"><span data-stu-id="e4b11-240">What ranges of durations cover different percentages of sessions?</span></span>

<span data-ttu-id="e4b11-241">Använda frågan ovan, men ersätt den sista raden:</span><span class="sxs-lookup"><span data-stu-id="e4b11-241">Use the above query, but replace the last line:</span></span>

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

<span data-ttu-id="e4b11-242">Vi också bort den övre gränsen i where-satsen för att få rätt uppgifter som inkluderar alla sessioner med mer än en förfrågan:</span><span class="sxs-lookup"><span data-stu-id="e4b11-242">We also removed the upper limit in the where clause, in order to get correct figures including all sessions with more than one request:</span></span>

![Resultat](./media/app-insights-analytics-tour/180.png)

<span data-ttu-id="e4b11-244">Där hittar som:</span><span class="sxs-lookup"><span data-stu-id="e4b11-244">From which we can see that:</span></span>

* <span data-ttu-id="e4b11-245">5% av sessioner har en varaktighet på mindre än 3 minuter 34s;</span><span class="sxs-lookup"><span data-stu-id="e4b11-245">5% of sessions have a duration of less than 3 minutes 34s;</span></span>
* <span data-ttu-id="e4b11-246">50% av sessioner senaste 36 minuter.</span><span class="sxs-lookup"><span data-stu-id="e4b11-246">50% of sessions last less than 36 minutes;</span></span>
* <span data-ttu-id="e4b11-247">5% av sessioner senaste mer än 7 dagar</span><span class="sxs-lookup"><span data-stu-id="e4b11-247">5% of sessions last more than 7 days</span></span>

<span data-ttu-id="e4b11-248">Om du vill ha en separat uppdelning för varje land, vi bara ha för att sammanfatta kolumnen client_CountryOrRegion separat via båda operatorer:</span><span class="sxs-lookup"><span data-stu-id="e4b11-248">To get a separate breakdown for each country, we just have to bring the client_CountryOrRegion column separately through both summarize operators:</span></span>

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

## <a name="join"></a><span data-ttu-id="e4b11-249">Slå ihop</span><span class="sxs-lookup"><span data-stu-id="e4b11-249">Join</span></span>
<span data-ttu-id="e4b11-250">Vi har åtkomst till flera tabeller, inklusive begäranden och undantag.</span><span class="sxs-lookup"><span data-stu-id="e4b11-250">We have access to several tables, including requests and exceptions.</span></span>

<span data-ttu-id="e4b11-251">Om du vill hitta undantag som rör en begäran som returnerade ett fel svar vi koppla tabellerna på `session_Id`:</span><span class="sxs-lookup"><span data-stu-id="e4b11-251">To find the exceptions related to a request that returned a failure response, we can join the tables on `session_Id`:</span></span>

```AIQL

    requests
    | where toint(resultCode) >= 500
    | join (exceptions) on operation_Id
    | take 30
```


<span data-ttu-id="e4b11-252">Det är bra att använda `project` att välja bara de kolumner som vi behöver innan du utför kopplingen.</span><span class="sxs-lookup"><span data-stu-id="e4b11-252">It's good practice to use `project` to select just the columns we need before performing the join.</span></span>
<span data-ttu-id="e4b11-253">I samma-satser vi Byt namn på kolumn för tidsstämpel.</span><span class="sxs-lookup"><span data-stu-id="e4b11-253">In the same clauses, we rename the timestamp column.</span></span>

## <a name="lethttpsdocsloganalyticsioquerylanguagequerylanguageletstatementhtml-assign-a-result-to-a-variable"></a><span data-ttu-id="e4b11-254">[Låt](https://docs.loganalytics.io/queryLanguage/query_language_letstatement.html): tilldela en variabel ett resultat</span><span class="sxs-lookup"><span data-stu-id="e4b11-254">[Let](https://docs.loganalytics.io/queryLanguage/query_language_letstatement.html): Assign a result to a variable</span></span>

<span data-ttu-id="e4b11-255">Använd `let` att skilja ut delarna av uttrycket ovan.</span><span class="sxs-lookup"><span data-stu-id="e4b11-255">Use `let` to separate out the parts of the previous expression.</span></span> <span data-ttu-id="e4b11-256">Resultatet är oförändrad:</span><span class="sxs-lookup"><span data-stu-id="e4b11-256">The results are unchanged:</span></span>

```AIQL

    let bad_requests =
      requests
        | where  toint(resultCode) >= 500  ;
    bad_requests
    | join (exceptions) on session_Id
    | take 30
```

> [!Tip] 
> <span data-ttu-id="e4b11-257">Analytics-klienten Placera inte tomma rader mellan delar av frågan.</span><span class="sxs-lookup"><span data-stu-id="e4b11-257">In the Analytics client, don't put blank lines between the parts of the query.</span></span> <span data-ttu-id="e4b11-258">Se till att köra alla.</span><span class="sxs-lookup"><span data-stu-id="e4b11-258">Make sure to execute all of it.</span></span>
>

<span data-ttu-id="e4b11-259">Använd `toscalar` att konvertera en enskild cell till ett värde:</span><span class="sxs-lookup"><span data-stu-id="e4b11-259">Use `toscalar` to convert a single table cell to a value:</span></span>

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


### <a name="functions"></a><span data-ttu-id="e4b11-260">Funktioner</span><span class="sxs-lookup"><span data-stu-id="e4b11-260">Functions</span></span>

<span data-ttu-id="e4b11-261">Använd *kan* att definiera en funktion:</span><span class="sxs-lookup"><span data-stu-id="e4b11-261">Use *Let* to define a function:</span></span>

```AIQL

    let usdate = (t:datetime)
    {
      strcat(getmonth(t), "/", dayofmonth(t),"/", getyear(t), " ",
      bin((t-1h)%12h+1h,1s), iff(t%24h<12h, "AM", "PM"))
    };
    requests  
    | extend PST = usdate(timestamp-8h)
```

## <a name="accessing-nested-objects"></a><span data-ttu-id="e4b11-262">Åtkomst till kapslade objekt</span><span class="sxs-lookup"><span data-stu-id="e4b11-262">Accessing nested objects</span></span>
<span data-ttu-id="e4b11-263">Kapslade objekt kan nås enkelt.</span><span class="sxs-lookup"><span data-stu-id="e4b11-263">Nested objects can be accessed easily.</span></span> <span data-ttu-id="e4b11-264">Du kan till exempel se strukturerade objekt så här i dataströmmen undantag:</span><span class="sxs-lookup"><span data-stu-id="e4b11-264">For example, in the exceptions stream you can see structured objects like this:</span></span>

![Resultat](./media/app-insights-analytics-tour/520.png)

<span data-ttu-id="e4b11-266">Du kan förenkla den genom att välja de egenskaper som du är intresserad av:</span><span class="sxs-lookup"><span data-stu-id="e4b11-266">You can flatten it by choosing the properties you're interested in:</span></span>

```AIQL

    exceptions | take 10
    | extend method1 = tostring(details[0].parsedStack[1].method)
```

<span data-ttu-id="e4b11-267">Observera att du måste konvertera resultatet till lämplig.</span><span class="sxs-lookup"><span data-stu-id="e4b11-267">Note that you need to cast the result to the appropriate type.</span></span>


## <a name="custom-properties-and-measurements"></a><span data-ttu-id="e4b11-268">Anpassade egenskaper och mått</span><span class="sxs-lookup"><span data-stu-id="e4b11-268">Custom properties and measurements</span></span>
<span data-ttu-id="e4b11-269">Om programmet ansluts [anpassade dimensioner (Egenskaper) och anpassade mått](app-insights-api-custom-events-metrics.md#properties) till händelser, så visas dem i den `customDimensions` och `customMeasurements` objekt.</span><span class="sxs-lookup"><span data-stu-id="e4b11-269">If your application attaches [custom dimensions (properties) and custom measurements](app-insights-api-custom-events-metrics.md#properties) to events, then you will see them in the `customDimensions` and `customMeasurements` objects.</span></span>

<span data-ttu-id="e4b11-270">Till exempel om appen innehåller:</span><span class="sxs-lookup"><span data-stu-id="e4b11-270">For example, if your app includes:</span></span>

```C#

    var dimensions = new Dictionary<string, string>
                     {{"p1", "v1"},{"p2", "v2"}};
    var measurements = new Dictionary<string, double>
                     {{"m1", 42.0}, {"m2", 43.2}};
    telemetryClient.TrackEvent("myEvent", dimensions, measurements);
```

<span data-ttu-id="e4b11-271">Så här extraherar värdena i Analytics:</span><span class="sxs-lookup"><span data-stu-id="e4b11-271">To extract these values in Analytics:</span></span>

```AIQL

    customEvents
    | extend p1 = customDimensions.p1,
      m1 = todouble(customMeasurements.m1) // cast to expected type

```

<span data-ttu-id="e4b11-272">Verifiera om en anpassad dimensionen är av en viss typ:</span><span class="sxs-lookup"><span data-stu-id="e4b11-272">To verify whether a custom dimension is of a particular type:</span></span>

```AIQL

    customEvents
    | extend p1 = customDimensions.p1,
      iff(notnull(todouble(customMeasurements.m1)), ...
```

## <a name="dashboards"></a><span data-ttu-id="e4b11-273">Instrumentpaneler</span><span class="sxs-lookup"><span data-stu-id="e4b11-273">Dashboards</span></span>
<span data-ttu-id="e4b11-274">Du kan fästa resultaten till en instrumentpanel för att sätta samman alla dina viktigaste diagram och tabeller.</span><span class="sxs-lookup"><span data-stu-id="e4b11-274">You can pin your results to a dashboard in order to bring together all your most important charts and tables.</span></span>

* <span data-ttu-id="e4b11-275">[Azure delade instrumentpanelen](app-insights-dashboards.md#share-dashboards): Klicka på ikonen PIN-kod.</span><span class="sxs-lookup"><span data-stu-id="e4b11-275">[Azure shared dashboard](app-insights-dashboards.md#share-dashboards): Click the pin icon.</span></span> <span data-ttu-id="e4b11-276">Innan du gör det måste du ha en delad instrumentpanel.</span><span class="sxs-lookup"><span data-stu-id="e4b11-276">Before you do this, you must have a shared dashboard.</span></span> <span data-ttu-id="e4b11-277">Öppna Azure-portalen eller skapa en instrumentpanel och klicka på resursen.</span><span class="sxs-lookup"><span data-stu-id="e4b11-277">In the Azure portal, open or create a dashboard and click Share.</span></span>
* <span data-ttu-id="e4b11-278">[Power BI-instrumentpanel](app-insights-export-power-bi.md): Klicka på Exportera, Power BI-fråga.</span><span class="sxs-lookup"><span data-stu-id="e4b11-278">[Power BI dashboard](app-insights-export-power-bi.md): Click Export, Power BI Query.</span></span> <span data-ttu-id="e4b11-279">En fördel med detta alternativ är att du kan visa din fråga tillsammans med andra resultat från en mängd olika källor.</span><span class="sxs-lookup"><span data-stu-id="e4b11-279">An advantage of this alternative is that you can display your query alongside other results from a wide range of sources.</span></span>

## <a name="combine-with-imported-data"></a><span data-ttu-id="e4b11-280">Kombinera med importerade data</span><span class="sxs-lookup"><span data-stu-id="e4b11-280">Combine with imported data</span></span>

<span data-ttu-id="e4b11-281">Analytics-rapporter se bra ut på instrumentpanelen, men ibland vill du att konvertera data till ett datamängder formulär.</span><span class="sxs-lookup"><span data-stu-id="e4b11-281">Analytics reports look great on the dashboard, but sometimes you want to translate the data to a more digestible form.</span></span> <span data-ttu-id="e4b11-282">Anta exempelvis att din autentiserade användare identifieras i telemetrin av ett alias.</span><span class="sxs-lookup"><span data-stu-id="e4b11-282">For example, suppose your authenticated users are identified in the telemetry by an alias.</span></span> <span data-ttu-id="e4b11-283">Du vill visa namnen verkliga i resultaten.</span><span class="sxs-lookup"><span data-stu-id="e4b11-283">You'd like to show their real names in your results.</span></span> <span data-ttu-id="e4b11-284">Om du vill göra detta måste en CSV-fil som mappar från alias till de verkliga namn.</span><span class="sxs-lookup"><span data-stu-id="e4b11-284">To do this, you need a CSV file that maps from the aliases to the real names.</span></span>

<span data-ttu-id="e4b11-285">Du kan importera en fil och använda den precis som alla standardtabeller (begäranden, undantag och så vidare).</span><span class="sxs-lookup"><span data-stu-id="e4b11-285">You can import a data file and use it just like any of the standard tables (requests, exceptions, and so on).</span></span> <span data-ttu-id="e4b11-286">Antingen fråga på sin egen eller Anslut den med andra tabeller.</span><span class="sxs-lookup"><span data-stu-id="e4b11-286">Either query it on its own, or join it with other tables.</span></span> <span data-ttu-id="e4b11-287">Om du har en tabell med namnet usermap och kolumner har till exempel `realName` och `userId`, och sedan använda den för att översätta det `user_AuthenticatedId` i begärandetelemetri:</span><span class="sxs-lookup"><span data-stu-id="e4b11-287">For example, if you have a table named usermap, and it has columns `realName` and `userId`, then you can use it to translate the `user_AuthenticatedId` field in the request telemetry:</span></span>

```AIQL

    requests
    | where notempty(user_AuthenticatedId)
    | project userId = user_AuthenticatedId
      // get the realName field from the usermap table:
    | join kind=leftouter ( usermap ) on userId
      // count transactions by name:
    | summarize count() by realName
```

<span data-ttu-id="e4b11-288">Så här importerar du en tabell, i bladet schemat under **andra datakällor**, följ instruktionerna för att lägga till en ny datakälla genom att överföra ett urval av dina data.</span><span class="sxs-lookup"><span data-stu-id="e4b11-288">To import a table, in the Schema blade, under **Other Data Sources**, follow the instructions to add a new data source, by uploading a sample of your data.</span></span> <span data-ttu-id="e4b11-289">Du kan sedan använda den här definitionen för att överföra tabeller.</span><span class="sxs-lookup"><span data-stu-id="e4b11-289">Then you can use this definition to upload tables.</span></span>

<span data-ttu-id="e4b11-290">Import-funktionen är för närvarande under förhandsgranskning, så visas först en ”Kontakta oss” länk under ”andra datakällor”.</span><span class="sxs-lookup"><span data-stu-id="e4b11-290">The import feature is currently in preview, so you will initially see a "Contact us" link under "Other data sources."</span></span> <span data-ttu-id="e4b11-291">Används för att registrera dig för förhandsgranskningsprogrammet och länken kommer sedan att ersättas av knappen ”Lägg till ny datakälla”.</span><span class="sxs-lookup"><span data-stu-id="e4b11-291">Use this to sign up to the preview program, and the link will then be replaced by an "Add new data source" button.</span></span>


## <a name="tables"></a><span data-ttu-id="e4b11-292">Tabeller</span><span class="sxs-lookup"><span data-stu-id="e4b11-292">Tables</span></span>
<span data-ttu-id="e4b11-293">Dataströmmen telemetri från din app är tillgängligt via flera tabeller.</span><span class="sxs-lookup"><span data-stu-id="e4b11-293">The stream of telemetry received from your app is accessible through several tables.</span></span> <span data-ttu-id="e4b11-294">Schemat för egenskaper som är tillgängliga för varje tabell visas till vänster i fönstret.</span><span class="sxs-lookup"><span data-stu-id="e4b11-294">The schema of properties available for each table is visible at the left of the window.</span></span>

### <a name="requests-table"></a><span data-ttu-id="e4b11-295">Begäranden tabell</span><span class="sxs-lookup"><span data-stu-id="e4b11-295">Requests table</span></span>
<span data-ttu-id="e4b11-296">Antal HTTP-begäranden till ditt webbprogram och segment per sidnamn:</span><span class="sxs-lookup"><span data-stu-id="e4b11-296">Count HTTP requests to your web app and segment by page name:</span></span>

![Antal begäranden segmenterat efter namn](./media/app-insights-analytics-tour/analytics-count-requests.png)

<span data-ttu-id="e4b11-298">Sök efter begäranden som inte uppfyller de flesta:</span><span class="sxs-lookup"><span data-stu-id="e4b11-298">Find the requests that fail most:</span></span>

![Antal begäranden segmenterat efter namn](./media/app-insights-analytics-tour/analytics-failed-requests.png)

### <a name="custom-events-table"></a><span data-ttu-id="e4b11-300">Anpassade händelser tabell</span><span class="sxs-lookup"><span data-stu-id="e4b11-300">Custom events table</span></span>
<span data-ttu-id="e4b11-301">Om du använder [trackevent ()](app-insights-api-custom-events-metrics.md#trackevent) för att skicka dina egna händelser, kan du läsa dem från den här tabellen.</span><span class="sxs-lookup"><span data-stu-id="e4b11-301">If you use [TrackEvent()](app-insights-api-custom-events-metrics.md#trackevent) to send your own events, you can read them from this table.</span></span>

<span data-ttu-id="e4b11-302">Låt oss ta ett exempel där app-koden innehåller dessa rader:</span><span class="sxs-lookup"><span data-stu-id="e4b11-302">Let's take an example where your app code contains these lines:</span></span>

```C#

    telemetry.TrackEvent("Query",
       new Dictionary<string,string> {{"query", sqlCmd}},
       new Dictionary<string,double> {
           {"retry", retryCount},
           {"querytime", totalTime}})
```

<span data-ttu-id="e4b11-303">Visa hur ofta dessa händelser:</span><span class="sxs-lookup"><span data-stu-id="e4b11-303">Display the frequency of these events:</span></span>

![Visa antal anpassade händelser](./media/app-insights-analytics-tour/analytics-custom-events-rate.png)

<span data-ttu-id="e4b11-305">Extrahera mått och dimensioner från händelser:</span><span class="sxs-lookup"><span data-stu-id="e4b11-305">Extract measurements and dimensions from the events:</span></span>

![Visa antal anpassade händelser](./media/app-insights-analytics-tour/analytics-custom-events-dimensions.png)

### <a name="custom-metrics-table"></a><span data-ttu-id="e4b11-307">Anpassade mått tabell</span><span class="sxs-lookup"><span data-stu-id="e4b11-307">Custom metrics table</span></span>
<span data-ttu-id="e4b11-308">Om du använder [TrackMetric()](app-insights-api-custom-events-metrics.md#trackmetric) om du vill skicka dina egna måttvärden hittar du resultaten i den **customMetrics** dataströmmen.</span><span class="sxs-lookup"><span data-stu-id="e4b11-308">If you are using [TrackMetric()](app-insights-api-custom-events-metrics.md#trackmetric) to send your own metric values, you’ll find its results in the **customMetrics** stream.</span></span> <span data-ttu-id="e4b11-309">Exempel:</span><span class="sxs-lookup"><span data-stu-id="e4b11-309">For example:</span></span>  

![Anpassade mått i Application Insights analytics](./media/app-insights-analytics-tour/analytics-custom-metrics.png)

> [!NOTE]
> <span data-ttu-id="e4b11-311">I [Metrics Explorer](app-insights-metrics-explorer.md), alla anpassade mått som är kopplad till någon typ av telemetri visas tillsammans i bladet mått tillsammans med mått som skickas med `TrackMetric()`.</span><span class="sxs-lookup"><span data-stu-id="e4b11-311">In [Metrics Explorer](app-insights-metrics-explorer.md), all custom measurements attached to any type of telemetry appear together in the metrics blade along with metrics sent using `TrackMetric()`.</span></span> <span data-ttu-id="e4b11-312">Men i Analytics, anpassade mått fortfarande är kopplade till oavsett vilken typ av telemetri som de har gjort på - händelser eller begäranden och så vidare - medan mått som skickats av TrackMetric visas i sina egna dataströmmen.</span><span class="sxs-lookup"><span data-stu-id="e4b11-312">But in Analytics, custom measurements are still attached to whichever type of telemetry they were carried on - events or requests, and so on - while metrics sent by TrackMetric appear in their own stream.</span></span>
>
>

### <a name="performance-counters-table"></a><span data-ttu-id="e4b11-313">Prestandaräknare tabell</span><span class="sxs-lookup"><span data-stu-id="e4b11-313">Performance counters table</span></span>
<span data-ttu-id="e4b11-314">[Prestandaräknare](app-insights-performance-counters.md) visa grundläggande system mätvärden för din app, t.ex CPU eller minne och nätverksanvändning.</span><span class="sxs-lookup"><span data-stu-id="e4b11-314">[Performance counters](app-insights-performance-counters.md) show you basic system metrics for your app, such as CPU, memory, and network utilization.</span></span> <span data-ttu-id="e4b11-315">Du kan konfigurera SDK för att skicka ytterligare räknare, inklusive din egen anpassade räknare.</span><span class="sxs-lookup"><span data-stu-id="e4b11-315">You can configure the SDK to send additional counters, including your own custom counters.</span></span>

<span data-ttu-id="e4b11-316">Den **performanceCounters** schemat visar den `category`, `counter` namn, och `instance` namn för varje prestandaräknare.</span><span class="sxs-lookup"><span data-stu-id="e4b11-316">The **performanceCounters** schema exposes the `category`, `counter` name, and `instance` name of each performance counter.</span></span> <span data-ttu-id="e4b11-317">Namn på prestandaräknarinstanser gäller endast vissa prestandaräknare och anger namnet på processen som antalet avser vanligtvis.</span><span class="sxs-lookup"><span data-stu-id="e4b11-317">Counter instance names are only applicable to some performance counters, and typically indicate the name of the process to which the count relates.</span></span> <span data-ttu-id="e4b11-318">Räknarna för programmet visas i telemetri för varje program.</span><span class="sxs-lookup"><span data-stu-id="e4b11-318">In the telemetry for each application, you’ll see only the counters for that application.</span></span> <span data-ttu-id="e4b11-319">Till exempel om du vill se vilka räknare är tillgängliga:</span><span class="sxs-lookup"><span data-stu-id="e4b11-319">For example, to see what counters are available:</span></span>

![Prestandaräknare i Application Insights analytics](./media/app-insights-analytics-tour/analytics-performance-counters.png)

<span data-ttu-id="e4b11-321">Hämta ett diagram av minne över den valda perioden:</span><span class="sxs-lookup"><span data-stu-id="e4b11-321">To get a chart of available memory over the selected period:</span></span>

![Minne timechart i Application Insights analytics](./media/app-insights-analytics-tour/analytics-available-memory.png)

<span data-ttu-id="e4b11-323">Som andra telemetri **performanceCounters** också har en kolumn `cloud_RoleInstance` som anger identiteten för värddatorn som appen körs.</span><span class="sxs-lookup"><span data-stu-id="e4b11-323">Like other telemetry, **performanceCounters** also has a column `cloud_RoleInstance` that indicates the identity of the host machine on which your app is running.</span></span> <span data-ttu-id="e4b11-324">Till exempel för att jämföra prestandan för din app på olika datorer:</span><span class="sxs-lookup"><span data-stu-id="e4b11-324">For example, to compare the performance of your app on the different machines:</span></span>

![Prestanda åtskilda med rollinstans i Application Insights analytics](./media/app-insights-analytics-tour/analytics-metrics-role-instance.png)

### <a name="exceptions-table"></a><span data-ttu-id="e4b11-326">Feltabellen</span><span class="sxs-lookup"><span data-stu-id="e4b11-326">Exceptions table</span></span>
<span data-ttu-id="e4b11-327">[Undantag som rapporterats av din app](app-insights-asp-net-exceptions.md) är tillgängliga i den här tabellen.</span><span class="sxs-lookup"><span data-stu-id="e4b11-327">[Exceptions reported by your app](app-insights-asp-net-exceptions.md) are available in this table.</span></span>

<span data-ttu-id="e4b11-328">Delta i operation_Id för att hitta HTTP-begäran som din app hantering när undantagsfel inträffade:</span><span class="sxs-lookup"><span data-stu-id="e4b11-328">To find the HTTP request that your app was handling when the exception was raised, join on operation_Id:</span></span>

![Ansluta till undantag med begäranden på operation_Id](./media/app-insights-analytics-tour/analytics-exception-request.png)

### <a name="browser-timings-table"></a><span data-ttu-id="e4b11-330">Webbläsaren tidsinställningar för tabellen</span><span class="sxs-lookup"><span data-stu-id="e4b11-330">Browser timings table</span></span>
<span data-ttu-id="e4b11-331">`browserTimings`Visar sidan Läs in data som samlas in i användarnas webbläsare.</span><span class="sxs-lookup"><span data-stu-id="e4b11-331">`browserTimings` shows page load data collected in your users' browsers.</span></span>

<span data-ttu-id="e4b11-332">[Konfigurera din app för klientsidan telemetri](app-insights-javascript.md) för att kunna se dessa mått.</span><span class="sxs-lookup"><span data-stu-id="e4b11-332">[Set up your app for client-side telemetry](app-insights-javascript.md) in order to see these metrics.</span></span>

<span data-ttu-id="e4b11-333">Schemat innehåller [mått som anger längden på olika faser i processen för inläsning sidan](app-insights-javascript.md#page-load-performance).</span><span class="sxs-lookup"><span data-stu-id="e4b11-333">The schema includes [metrics indicating the lengths of different stages of the page loading process](app-insights-javascript.md#page-load-performance).</span></span> <span data-ttu-id="e4b11-334">(De inte anger hur lång tid som användarna läsa en sida.)</span><span class="sxs-lookup"><span data-stu-id="e4b11-334">(They don’t indicate the length of time your users read a page.)</span></span>  

<span data-ttu-id="e4b11-335">Visa popularities på olika sidor och läsa in tider för varje sida:</span><span class="sxs-lookup"><span data-stu-id="e4b11-335">Show the popularities of different pages, and load times for each page:</span></span>

![Sidinläsningstider i Analytics](./media/app-insights-analytics-tour/analytics-page-load.png)

### <a name="availability-results-table"></a><span data-ttu-id="e4b11-337">Tillgänglighet resultattabellen</span><span class="sxs-lookup"><span data-stu-id="e4b11-337">Availability results table</span></span>
<span data-ttu-id="e4b11-338">`availabilityResults`Visar resultatet av dina [webbtester](app-insights-monitor-web-app-availability.md).</span><span class="sxs-lookup"><span data-stu-id="e4b11-338">`availabilityResults` shows the results of your [web tests](app-insights-monitor-web-app-availability.md).</span></span> <span data-ttu-id="e4b11-339">Varje körning av dina tester från varje test plats rapporteras separat.</span><span class="sxs-lookup"><span data-stu-id="e4b11-339">Each run of your tests from each test location is reported separately.</span></span>

![Sidinläsningstider i Analytics](./media/app-insights-analytics-tour/analytics-availability.png)

### <a name="dependencies-table"></a><span data-ttu-id="e4b11-341">Beroenden tabell</span><span class="sxs-lookup"><span data-stu-id="e4b11-341">Dependencies table</span></span>
<span data-ttu-id="e4b11-342">Innehåller resultatet av anrop att din app gör att databaser och REST API: er och andra anrop till TrackDependency().</span><span class="sxs-lookup"><span data-stu-id="e4b11-342">Contains results of calls that your app makes to databases and REST APIs, and other calls to TrackDependency().</span></span> <span data-ttu-id="e4b11-343">Även AJAX-anrop från webbläsaren.</span><span class="sxs-lookup"><span data-stu-id="e4b11-343">Also includes AJAX calls made from the browser.</span></span>

<span data-ttu-id="e4b11-344">AJAX-anrop från webbläsaren:</span><span class="sxs-lookup"><span data-stu-id="e4b11-344">AJAX calls from the browser:</span></span>

```AIQL

    dependencies | where client_Type == "Browser"
    | take 10
```

<span data-ttu-id="e4b11-345">Beroendeanrop från servern:</span><span class="sxs-lookup"><span data-stu-id="e4b11-345">Dependency calls from the server:</span></span>

```AIQL

    dependencies | where client_Type == "PC"
    | take 10
```

<span data-ttu-id="e4b11-346">Beroende på serversidan resultatet visar alltid `success==False` om Application Insights-agenten inte är installerad.</span><span class="sxs-lookup"><span data-stu-id="e4b11-346">Server-side dependency results always show `success==False` if the Application Insights Agent is not installed.</span></span> <span data-ttu-id="e4b11-347">Dock är andra data korrekta.</span><span class="sxs-lookup"><span data-stu-id="e4b11-347">However, the other data are correct.</span></span>

### <a name="traces-table"></a><span data-ttu-id="e4b11-348">Spår tabell</span><span class="sxs-lookup"><span data-stu-id="e4b11-348">Traces table</span></span>
<span data-ttu-id="e4b11-349">Innehåller telemetri som skickats av din app att använda TrackTrace(), eller [andra loggning ramverk](app-insights-asp-net-trace-logs.md).</span><span class="sxs-lookup"><span data-stu-id="e4b11-349">Contains the telemetry sent by your app using TrackTrace(), or [other logging frameworks](app-insights-asp-net-trace-logs.md).</span></span>

## <a name="video"></a><span data-ttu-id="e4b11-350">Video</span><span class="sxs-lookup"><span data-stu-id="e4b11-350">Video</span></span> 

> [!VIDEO https://channel9.msdn.com/events/Connect/2016/123/player] 

<span data-ttu-id="e4b11-351">Avancerade frågor:</span><span class="sxs-lookup"><span data-stu-id="e4b11-351">Advanced queries:</span></span>

> [!VIDEO https://channel9.msdn.com/Events/Build/2016/P591/player]


## <a name="next-steps"></a><span data-ttu-id="e4b11-352">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="e4b11-352">Next steps</span></span>
* [<span data-ttu-id="e4b11-353">Språkreferens för Analytics</span><span class="sxs-lookup"><span data-stu-id="e4b11-353">Analytics language reference</span></span>](app-insights-analytics-reference.md)
* <span data-ttu-id="e4b11-354">[SQL-användarnas cheat blad](https://aka.ms/sql-analytics) översätter vanligaste idioms.</span><span class="sxs-lookup"><span data-stu-id="e4b11-354">[SQL-users' cheat sheet](https://aka.ms/sql-analytics) translates the most common idioms.</span></span>

[!INCLUDE [app-insights-analytics-footer](../../includes/app-insights-analytics-footer.md)]
