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
# <a name="a-tour-of-analytics-in-application-insights"></a><span data-ttu-id="d601a-103">En genomgång av Analytics i Application Insights</span><span class="sxs-lookup"><span data-stu-id="d601a-103">A tour of Analytics in Application Insights</span></span>
<span data-ttu-id="d601a-104">[Analytics](app-insights-analytics.md) är hello kraftfulla sökfunktionen i [Programinsikter](app-insights-overview.md).</span><span class="sxs-lookup"><span data-stu-id="d601a-104">[Analytics](app-insights-analytics.md) is hello powerful search feature of [Application Insights](app-insights-overview.md).</span></span> <span data-ttu-id="d601a-105">Dessa sidor beskrivs Log Analytics-frågespråket.</span><span class="sxs-lookup"><span data-stu-id="d601a-105">These pages describe the Log Analytics query language.</span></span>

* <span data-ttu-id="d601a-106">**[Se hello inledande video](https://applicationanalytics-media.azureedge.net/home_page_video.mp4)**.</span><span class="sxs-lookup"><span data-stu-id="d601a-106">**[Watch hello introductory video](https://applicationanalytics-media.azureedge.net/home_page_video.mp4)**.</span></span>
* <span data-ttu-id="d601a-107">**[Testkör Analytics på våra simulerade data](https://analytics.applicationinsights.io/demo)**  om din app inte skickar data tooApplication insikter ännu.</span><span class="sxs-lookup"><span data-stu-id="d601a-107">**[Test drive Analytics on our simulated data](https://analytics.applicationinsights.io/demo)** if your app isn't sending data tooApplication Insights yet.</span></span>
* <span data-ttu-id="d601a-108">**[SQL-användarnas cheat blad](https://aka.ms/sql-analytics)**  översätter hello vanligaste idioms.</span><span class="sxs-lookup"><span data-stu-id="d601a-108">**[SQL-users' cheat sheet](https://aka.ms/sql-analytics)** translates hello most common idioms.</span></span>

<span data-ttu-id="d601a-109">Låt oss ta ett gå igenom några grundläggande frågor tooget som du startade.</span><span class="sxs-lookup"><span data-stu-id="d601a-109">Let's take a walk through some basic queries tooget you started.</span></span>

## <a name="connect-tooyour-application-insights-data"></a><span data-ttu-id="d601a-110">Ansluta tooyour Application Insights-data</span><span class="sxs-lookup"><span data-stu-id="d601a-110">Connect tooyour Application Insights data</span></span>
<span data-ttu-id="d601a-111">Öppna Analytics från din app [översikt bladet](app-insights-dashboards.md) i Application Insights:</span><span class="sxs-lookup"><span data-stu-id="d601a-111">Open Analytics from your app's [overview blade](app-insights-dashboards.md) in Application Insights:</span></span>

![Öppna portal.azure.com, öppna Application Insights-resursen och klicka på Analytics.](./media/app-insights-analytics-tour/001.png)

## <a name="takehttpsdocsloganalyticsioquerylanguagequerylanguagetakeoperatorhtml-show-me-n-rows"></a><span data-ttu-id="d601a-113">[Ta](https://docs.loganalytics.io/queryLanguage/query_language_takeoperator.html): Visa n rader</span><span class="sxs-lookup"><span data-stu-id="d601a-113">[Take](https://docs.loganalytics.io/queryLanguage/query_language_takeoperator.html): show me n rows</span></span>
<span data-ttu-id="d601a-114">Datapunkter som loggar användaren operations (vanligtvis HTTP-begäranden tas emot av ditt webbprogram) lagras i en tabell som kallas `requests`.</span><span class="sxs-lookup"><span data-stu-id="d601a-114">Data points that log user operations (typically HTTP requests received by your web app) are stored in a table called `requests`.</span></span> <span data-ttu-id="d601a-115">Varje rad är en telemetri data togs emot från hello Application Insights SDK i din app.</span><span class="sxs-lookup"><span data-stu-id="d601a-115">Each row is a telemetry data point received from hello Application Insights SDK in your app.</span></span>

<span data-ttu-id="d601a-116">Låt oss börja med att undersöka några exempel rader i tabellen hello:</span><span class="sxs-lookup"><span data-stu-id="d601a-116">Let's start by examining a few sample rows of hello table:</span></span>

![resultat](./media/app-insights-analytics-tour/010.png)

> [!NOTE]
> <span data-ttu-id="d601a-118">Placera markören hello någonstans i hello instruktionen innan du klickar på OK.</span><span class="sxs-lookup"><span data-stu-id="d601a-118">Put hello cursor somewhere in hello statement before you click Go.</span></span> <span data-ttu-id="d601a-119">Du kan dela en instruktion över flera rader, men Placera inte tomma rader i en instruktion.</span><span class="sxs-lookup"><span data-stu-id="d601a-119">You can split a statement over more than one line, but don't put blank lines in a statement.</span></span> <span data-ttu-id="d601a-120">Tomma rader är ett bekvämt sätt tookeep flera separata frågor i hello-fönstret.</span><span class="sxs-lookup"><span data-stu-id="d601a-120">Blank lines are a convenient way tookeep several separate queries in hello window.</span></span>
>
>

<span data-ttu-id="d601a-121">Välj kolumner, drar du dem, gruppera efter kolumner, och filtrera:</span><span class="sxs-lookup"><span data-stu-id="d601a-121">Choose columns, drag them, group by columns, and filter:</span></span>

![Klicka på kolumnmarkering i övre högra hörnet av resultat](./media/app-insights-analytics-tour/030.png)

<span data-ttu-id="d601a-123">Expandera alla toosee hello information om:</span><span class="sxs-lookup"><span data-stu-id="d601a-123">Expand any item toosee hello detail:</span></span>

![Välj tabellen och konfigurera kolumner](./media/app-insights-analytics-tour/040.png)

> [!NOTE]
> <span data-ttu-id="d601a-125">Klicka på hello chefen för en kolumn toore ordning hello resultat är tillgängliga i hello webbläsare.</span><span class="sxs-lookup"><span data-stu-id="d601a-125">Click hello head of a column toore-order hello results available in hello web browser.</span></span> <span data-ttu-id="d601a-126">Men tänk på att hello antal rader hämtade toohello webbläsare för en stor resultatmängd är begränsad.</span><span class="sxs-lookup"><span data-stu-id="d601a-126">But be aware that for a large result set, hello number of rows downloaded toohello browser is limited.</span></span> <span data-ttu-id="d601a-127">Sortering därmed visar inte alltid du hello faktiska högsta eller lägsta objekt.</span><span class="sxs-lookup"><span data-stu-id="d601a-127">Sorting this way doesn't always show you hello actual highest or lowest items.</span></span> <span data-ttu-id="d601a-128">toosort-objekt på ett tillförlitligt sätt, använder du hello `top` eller `sort` operator.</span><span class="sxs-lookup"><span data-stu-id="d601a-128">toosort items reliably, use hello `top` or `sort` operator.</span></span>
>
>

## <a name="tophttpsdocsloganalyticsioquerylanguagequerylanguagetopoperatorhtml-and-sorthttpsdocsloganalyticsioquerylanguagequerylanguagesortoperatorhtml"></a><span data-ttu-id="d601a-129">[TOP](https://docs.loganalytics.io/queryLanguage/query_language_topoperator.html) och [sortera](https://docs.loganalytics.io/queryLanguage/query_language_sortoperator.html)</span><span class="sxs-lookup"><span data-stu-id="d601a-129">[Top](https://docs.loganalytics.io/queryLanguage/query_language_topoperator.html) and [sort](https://docs.loganalytics.io/queryLanguage/query_language_sortoperator.html)</span></span>
<span data-ttu-id="d601a-130">`take`är användbara tooget en snabb exempel på ett resultat, men den visar rader från tabellen hello i bokstavsordning.</span><span class="sxs-lookup"><span data-stu-id="d601a-130">`take` is useful tooget a quick sample of a result, but it shows rows from hello table in no particular order.</span></span> <span data-ttu-id="d601a-131">Använd tooget en ordnad vy `top` (till exempel) eller `sort` (via hello hela tabellen).</span><span class="sxs-lookup"><span data-stu-id="d601a-131">tooget an ordered view, use `top` (for a sample) or `sort` (over hello whole table).</span></span>

<span data-ttu-id="d601a-132">Visa hello första n rader, sorterade efter en viss kolumn:</span><span class="sxs-lookup"><span data-stu-id="d601a-132">Show me hello first n rows, ordered by a particular column:</span></span>

```AIQL

    requests | top 10 by timestamp desc
```

* <span data-ttu-id="d601a-133">*Syntax:* de flesta operatörer har nyckelordet parametrar som `by`.</span><span class="sxs-lookup"><span data-stu-id="d601a-133">*Syntax:* Most operators have keyword parameters such as `by`.</span></span>
* <span data-ttu-id="d601a-134">`desc`= fallande ordning, `asc` = stigande.</span><span class="sxs-lookup"><span data-stu-id="d601a-134">`desc` = descending order, `asc` = ascending.</span></span>

![](./media/app-insights-analytics-tour/260.png)

<span data-ttu-id="d601a-135">`top...`är ett mer performant sätt att säga `sort ... | take...`.</span><span class="sxs-lookup"><span data-stu-id="d601a-135">`top...` is a more performant way of saying `sort ... | take...`.</span></span> <span data-ttu-id="d601a-136">Vi kan skriva:</span><span class="sxs-lookup"><span data-stu-id="d601a-136">We could have written:</span></span>

```AIQL

    requests | sort by timestamp desc | take 10
```

<span data-ttu-id="d601a-137">hello resultatet blir hello detsamma, men det skulle köras lite långsammare.</span><span class="sxs-lookup"><span data-stu-id="d601a-137">hello result would be hello same, but it would run a bit more slowly.</span></span> <span data-ttu-id="d601a-138">(Du kan också skriva `order`, vilket är ett alias för `sort`.)</span><span class="sxs-lookup"><span data-stu-id="d601a-138">(You could also write `order`, which is an alias of `sort`.)</span></span>

<span data-ttu-id="d601a-139">hello kolumnrubrikerna i hello tabellvy kan också vara används toosort hello resultat på hello-skärmen.</span><span class="sxs-lookup"><span data-stu-id="d601a-139">hello column headers in hello table view can also be used toosort hello results on hello screen.</span></span> <span data-ttu-id="d601a-140">Men av kursen, om du har använt `take` eller `top` tooretrieve bara en del av en tabell, ska du bara ordna hello poster som du har hämtats.</span><span class="sxs-lookup"><span data-stu-id="d601a-140">But of course, if you've used `take` or `top` tooretrieve just part of a table, you'll only re-order hello records you've retrieved.</span></span>

## <a name="wherehttpsdocsloganalyticsioquerylanguagequerylanguagewhereoperatorhtml-filtering-on-a-condition"></a><span data-ttu-id="d601a-141">[Där](https://docs.loganalytics.io/queryLanguage/query_language_whereoperator.html): filtrering på ett villkor</span><span class="sxs-lookup"><span data-stu-id="d601a-141">[Where](https://docs.loganalytics.io/queryLanguage/query_language_whereoperator.html): filtering on a condition</span></span>

<span data-ttu-id="d601a-142">Låt oss se bara begäranden som returnerade en viss Resultatkod:</span><span class="sxs-lookup"><span data-stu-id="d601a-142">Let's see just requests that returned a particular result code:</span></span>

```AIQL

    requests
    | where resultCode  == "404"
    | take 10
```

![](./media/app-insights-analytics-tour/250.png)

<span data-ttu-id="d601a-143">Hej `where` operator tar ett booleskt uttryck.</span><span class="sxs-lookup"><span data-stu-id="d601a-143">hello `where` operator takes a Boolean expression.</span></span> <span data-ttu-id="d601a-144">Här följer några huvudpunkter om dem:</span><span class="sxs-lookup"><span data-stu-id="d601a-144">Here are some key points about them:</span></span>

* <span data-ttu-id="d601a-145">`and`, `or`: Booleska operatorer</span><span class="sxs-lookup"><span data-stu-id="d601a-145">`and`, `or`: Boolean operators</span></span>
* <span data-ttu-id="d601a-146">`==`, `<>`, `!=` : lika med och inte lika med</span><span class="sxs-lookup"><span data-stu-id="d601a-146">`==`, `<>`, `!=` : equal and not equal</span></span>
* <span data-ttu-id="d601a-147">`=~`, `!~` : skiftlägeskänsliga sträng lika och inte lika.</span><span class="sxs-lookup"><span data-stu-id="d601a-147">`=~`, `!~` : case-insensitive string equal and not equal.</span></span> <span data-ttu-id="d601a-148">Det finns många fler sträng jämförelseoperatorer.</span><span class="sxs-lookup"><span data-stu-id="d601a-148">There are lots more string comparison operators.</span></span>

<!---Read all about [scalar expressions]().--->

### <a name="getting-hello-right-type"></a><span data-ttu-id="d601a-149">Hämta hello rätt typ</span><span class="sxs-lookup"><span data-stu-id="d601a-149">Getting hello right type</span></span>
<span data-ttu-id="d601a-150">Sök efter misslyckade begäranden:</span><span class="sxs-lookup"><span data-stu-id="d601a-150">Find unsuccessful requests:</span></span>

```AIQL

    requests
    | where isnotempty(resultCode) and toint(resultCode) >= 400
```
<!---
`resultCode` has type string, so we must cast it app-insights-analytics-reference.md#casts for a numeric comparison.
--->

## <a name="time"></a><span data-ttu-id="d601a-151">Tid</span><span class="sxs-lookup"><span data-stu-id="d601a-151">Time</span></span>

<span data-ttu-id="d601a-152">Dina frågor är som standard begränsad toohello senaste 24 timmarna.</span><span class="sxs-lookup"><span data-stu-id="d601a-152">By default, your queries are restricted toohello last 24 hours.</span></span> <span data-ttu-id="d601a-153">Men du kan ändra detta intervall:</span><span class="sxs-lookup"><span data-stu-id="d601a-153">But you can change this range:</span></span>

![](./media/app-insights-analytics-tour/change-time-range.png)

<span data-ttu-id="d601a-154">Åsidosätt hello tidsintervall genom att skriva en fråga som nämns `timestamp` i en where-sats.</span><span class="sxs-lookup"><span data-stu-id="d601a-154">Override hello time range by writing any query that mentions `timestamp` in a where-clause.</span></span> <span data-ttu-id="d601a-155">Exempel:</span><span class="sxs-lookup"><span data-stu-id="d601a-155">For example:</span></span>

```AIQL

    // What were hello slowest requests over hello past 3 days?
    requests
    | where timestamp > ago(3d)  // Override hello time range
    | top 5 by duration
```

<span data-ttu-id="d601a-156">hello tid intervallet funktionen är likvärdiga tooa 'where' satsen läggas till efter varje uppgift om något av hello källtabellerna.</span><span class="sxs-lookup"><span data-stu-id="d601a-156">hello time range feature is equivalent tooa 'where' clause inserted after each mention of one of hello source tables.</span></span>

<span data-ttu-id="d601a-157">`ago(3d)`innebär 'tre dagar sedan'.</span><span class="sxs-lookup"><span data-stu-id="d601a-157">`ago(3d)` means 'three days ago'.</span></span> <span data-ttu-id="d601a-158">Andra tidsenheter inkludera timmar (`2h`, `2.5h`), minuter (`25m`), och sekunder (`10s`).</span><span class="sxs-lookup"><span data-stu-id="d601a-158">Other units of time include hours (`2h`, `2.5h`), minutes (`25m`), and seconds (`10s`).</span></span>

<span data-ttu-id="d601a-159">Andra exempel:</span><span class="sxs-lookup"><span data-stu-id="d601a-159">Other examples:</span></span>

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

<span data-ttu-id="d601a-160">[Datum och tider referens](https://docs.loganalytics.io/concepts/concepts_datatypes_datetime.html).</span><span class="sxs-lookup"><span data-stu-id="d601a-160">[Dates and times reference](https://docs.loganalytics.io/concepts/concepts_datatypes_datetime.html).</span></span>


## <a name="projecthttpsdocsloganalyticsioquerylanguagequerylanguageprojectoperatorhtml-select-rename-and-compute-columns"></a><span data-ttu-id="d601a-161">[Projektet](https://docs.loganalytics.io/queryLanguage/query_language_projectoperator.html): Välj, byta namn på och bearbetning kolumner</span><span class="sxs-lookup"><span data-stu-id="d601a-161">[Project](https://docs.loganalytics.io/queryLanguage/query_language_projectoperator.html): select, rename, and compute columns</span></span>
<span data-ttu-id="d601a-162">Använd [ `project` ](https://docs.loganalytics.io/queryLanguage/query_language_projectoperator.html) toopick ut precis hello kolumner som du vill:</span><span class="sxs-lookup"><span data-stu-id="d601a-162">Use [`project`](https://docs.loganalytics.io/queryLanguage/query_language_projectoperator.html) toopick out just hello columns you want:</span></span>

```AIQL

    requests | top 10 by timestamp desc
             | project timestamp, name, resultCode
```

![](./media/app-insights-analytics-tour/240.png)

<span data-ttu-id="d601a-163">Du kan också byta namn på kolumner och definiera nya:</span><span class="sxs-lookup"><span data-stu-id="d601a-163">You can also rename columns and define new ones:</span></span>

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

* <span data-ttu-id="d601a-165">Kolumnnamn kan innehålla blanksteg eller symboler om de omges så här: `['...']` eller`["..."]`</span><span class="sxs-lookup"><span data-stu-id="d601a-165">Column names can include spaces or symbols if they are bracketed like this: `['...']` or `["..."]`</span></span>
* <span data-ttu-id="d601a-166">`%`är hello vanliga modulo-operatorn.</span><span class="sxs-lookup"><span data-stu-id="d601a-166">`%` is hello usual modulo operator.</span></span>
* <span data-ttu-id="d601a-167">`1d`(som är en siffra, en hade ”) är en literal timespan vilket innebär en dag.</span><span class="sxs-lookup"><span data-stu-id="d601a-167">`1d` (that's a digit one, then a 'd') is a timespan literal meaning one day.</span></span> <span data-ttu-id="d601a-168">Här följer några fler timespan-litteraler: `12h`, `30m`, `10s`, `0.01s`.</span><span class="sxs-lookup"><span data-stu-id="d601a-168">Here are some more timespan literals: `12h`, `30m`, `10s`, `0.01s`.</span></span>
* <span data-ttu-id="d601a-169">`floor`(alias `bin`) Avrundar värdet ned toohello närmaste multipel av hello Basvärde som du anger.</span><span class="sxs-lookup"><span data-stu-id="d601a-169">`floor` (alias `bin`) rounds a value down toohello nearest multiple of hello base value you provide.</span></span> <span data-ttu-id="d601a-170">Så `floor(aTime, 1s)` Avrundar taget ned toohello närmsta sekund.</span><span class="sxs-lookup"><span data-stu-id="d601a-170">So `floor(aTime, 1s)` rounds a time down toohello nearest second.</span></span>

<span data-ttu-id="d601a-171">Uttryck kan innehålla alla hello vanliga operatorer (`+`, `-`,...), och det finns en uppsättning praktiska funktioner.</span><span class="sxs-lookup"><span data-stu-id="d601a-171">Expressions can include all hello usual operators (`+`, `-`, ...), and there's a range of useful functions.</span></span>

## <a name="extend"></a><span data-ttu-id="d601a-172">Utöka</span><span class="sxs-lookup"><span data-stu-id="d601a-172">Extend</span></span>
<span data-ttu-id="d601a-173">Om du bara vill tooadd kolumner toohello befintliga använder [ `extend` ](https://docs.loganalytics.io/queryLanguage/query_language_extendoperator.html):</span><span class="sxs-lookup"><span data-stu-id="d601a-173">If you just want tooadd columns toohello existing ones, use [`extend`](https://docs.loganalytics.io/queryLanguage/query_language_extendoperator.html):</span></span>

```AIQL

    requests
    | top 10 by timestamp desc
    | extend timeOfDay = floor(timestamp % 1d, 1s)
```

<span data-ttu-id="d601a-174">Med hjälp av [ `extend` ](https://docs.loganalytics.io/queryLanguage/query_language_extendoperator.html) är mindre utförlig än [ `project` ](https://docs.loganalytics.io/queryLanguage/query_language_projectoperator.html) om du vill tookeep alla hello befintliga kolumner.</span><span class="sxs-lookup"><span data-stu-id="d601a-174">Using [`extend`](https://docs.loganalytics.io/queryLanguage/query_language_extendoperator.html) is less verbose than [`project`](https://docs.loganalytics.io/queryLanguage/query_language_projectoperator.html) if you want tookeep all hello existing columns.</span></span>

### <a name="convert-toolocal-time"></a><span data-ttu-id="d601a-175">Konvertera toolocal tid</span><span class="sxs-lookup"><span data-stu-id="d601a-175">Convert toolocal time</span></span>

<span data-ttu-id="d601a-176">Tidsstämplar är alltid i UTC.</span><span class="sxs-lookup"><span data-stu-id="d601a-176">Timestamps are always in UTC.</span></span> <span data-ttu-id="d601a-177">Om du är på hello oss Pacific kusten och det är vinter, kan du därför som detta:</span><span class="sxs-lookup"><span data-stu-id="d601a-177">So if you're on hello US Pacific coast and it's winter, you might like this:</span></span>

```AIQL

    requests
    | top 10 by timestamp desc
    | extend localTime = timestamp - 8h
```


## <a name="summarizehttpsdocsloganalyticsioquerylanguagequerylanguagesummarizeoperatorhtml-aggregate-groups-of-rows"></a><span data-ttu-id="d601a-178">[Sammanfatta](https://docs.loganalytics.io/queryLanguage/query_language_summarizeoperator.html): aggregera grupper av rader</span><span class="sxs-lookup"><span data-stu-id="d601a-178">[Summarize](https://docs.loganalytics.io/queryLanguage/query_language_summarizeoperator.html): aggregate groups of rows</span></span>
<span data-ttu-id="d601a-179">`Summarize`tillämpar en angiven *aggregeringsfunktionen* över grupper av rader.</span><span class="sxs-lookup"><span data-stu-id="d601a-179">`Summarize` applies a specified *aggregation function* over groups of rows.</span></span>

<span data-ttu-id="d601a-180">Till exempel hello överföringstiden för ditt webbprogram toorespond tooa begäran rapporteras i hello fältet `duration`.</span><span class="sxs-lookup"><span data-stu-id="d601a-180">For example, hello time your web app takes toorespond tooa request is reported in hello field `duration`.</span></span> <span data-ttu-id="d601a-181">Låt oss se genomsnittlig svarstid för hello tid tooall begäranden:</span><span class="sxs-lookup"><span data-stu-id="d601a-181">Let's see hello average response time tooall requests:</span></span>

![](./media/app-insights-analytics-tour/410.png)

<span data-ttu-id="d601a-182">Eller kan vi dela hello resultatet i förfrågningar av olika namn:</span><span class="sxs-lookup"><span data-stu-id="d601a-182">Or we could separate hello result into requests of different names:</span></span>

![](./media/app-insights-analytics-tour/420.png)

<span data-ttu-id="d601a-183">`Summarize`samlar in hello datapunkter i hello dataström i grupper för vilka hello `by` satsen utvärderar lika.</span><span class="sxs-lookup"><span data-stu-id="d601a-183">`Summarize` collects hello data points in hello stream into groups for which hello `by` clause evaluates equally.</span></span> <span data-ttu-id="d601a-184">Varje värde i hello `by` uttryck - varje åtgärdsnamn i hello ovan exempel - resulterar i en rad i hello resultattabellen.</span><span class="sxs-lookup"><span data-stu-id="d601a-184">Each value in hello `by` expression - each operation name in hello above example - results in a row in hello result table.</span></span>

<span data-ttu-id="d601a-185">Eller vi gick grupperar resultaten efter tid på dagen:</span><span class="sxs-lookup"><span data-stu-id="d601a-185">Or we could group results by time of day:</span></span>

![](./media/app-insights-analytics-tour/430.png)

<span data-ttu-id="d601a-186">Observera hur vi använder hello `bin` funktionen (även kallat `floor`).</span><span class="sxs-lookup"><span data-stu-id="d601a-186">Notice how we're using hello `bin` function (aka `floor`).</span></span> <span data-ttu-id="d601a-187">Om vi använde `by timestamp`, varje inkommande rad skulle hamna i en egen liten grupp.</span><span class="sxs-lookup"><span data-stu-id="d601a-187">If we just used `by timestamp`, every input row would end up in its own little group.</span></span> <span data-ttu-id="d601a-188">För en kontinuerlig skalära like gånger eller nummer har vi toobreak hello kontinuerligt område i hanterbara flera diskreta värden.</span><span class="sxs-lookup"><span data-stu-id="d601a-188">For any continuous scalar like times or numbers, we have toobreak hello continuous range into a manageable number of discrete values.</span></span> <span data-ttu-id="d601a-189">`bin`-som är precis hello bekant avrundas nedåt `floor` fungerar - är hello enklaste sättet toodo som.</span><span class="sxs-lookup"><span data-stu-id="d601a-189">`bin` - which is just hello familiar rounding-down `floor` function - is hello easiest way toodo that.</span></span>

<span data-ttu-id="d601a-190">Vi kan använda hello samma teknik tooreduce intervall av strängar:</span><span class="sxs-lookup"><span data-stu-id="d601a-190">We can use hello same technique tooreduce ranges of strings:</span></span>

![](./media/app-insights-analytics-tour/440.png)

<span data-ttu-id="d601a-191">Observera att du kan använda `name=` tooset hello namnet på en resultatkolumn i hello mängduttryck eller hello av-satsen.</span><span class="sxs-lookup"><span data-stu-id="d601a-191">Notice that you can use `name=` tooset hello name of a result column, either in hello aggregation expressions or hello by-clause.</span></span>

## <a name="counting-sampled-data"></a><span data-ttu-id="d601a-192">Cyklisk exempeldata</span><span class="sxs-lookup"><span data-stu-id="d601a-192">Counting sampled data</span></span>
<span data-ttu-id="d601a-193">`sum(itemCount)`är hello rekommenderade aggregering toocount händelser.</span><span class="sxs-lookup"><span data-stu-id="d601a-193">`sum(itemCount)` is hello recommended aggregation toocount events.</span></span> <span data-ttu-id="d601a-194">I många fall itemCount == 1, så hello funktionen bara räknar upp hello antalet rader i hello grupp.</span><span class="sxs-lookup"><span data-stu-id="d601a-194">In many cases, itemCount==1, so hello function simply counts up hello number of rows in hello group.</span></span> <span data-ttu-id="d601a-195">Men när [provtagning](app-insights-sampling.md) är i drift, en bråkdel av hello ursprungliga händelser behålls som datapunkter i Application Insights, så att det finns för varje datapunkt som du ser, `itemCount` händelser.</span><span class="sxs-lookup"><span data-stu-id="d601a-195">But when [sampling](app-insights-sampling.md) is in operation, only a fraction of hello original events are retained as data points in Application Insights, so that for each data point you see, there are `itemCount` events.</span></span>

<span data-ttu-id="d601a-196">Till exempel om sampling ignorerar 75% av hello ursprungliga händelser och sedan itemCount == 4 i hello behålls poster – det vill säga för varje kvarhållna post fanns fyra ursprungliga poster.</span><span class="sxs-lookup"><span data-stu-id="d601a-196">For example, if sampling discards 75% of hello original events, then itemCount==4 in hello retained records - that is, for every retained record, there were four original records.</span></span>

<span data-ttu-id="d601a-197">Anpassningsbar provtagning gör itemCount toobe högre under perioder när ditt program som ofta används.</span><span class="sxs-lookup"><span data-stu-id="d601a-197">Adaptive sampling causes itemCount toobe higher during periods when your application is being heavily used.</span></span>

<span data-ttu-id="d601a-198">Sammanfattningsvis itemCount därför ger en bra uppskattning hello ursprungliga antalet händelser.</span><span class="sxs-lookup"><span data-stu-id="d601a-198">Summing up itemCount therefore gives a good estimate of hello original number of events.</span></span>

![](./media/app-insights-analytics-tour/510.png)

<span data-ttu-id="d601a-199">Det finns också en `count()` aggregering (och ett antal åtgärden) i de fall där du verkligen vill toocount hello antalet rader i en grupp.</span><span class="sxs-lookup"><span data-stu-id="d601a-199">There's also a `count()` aggregation (and a count operation), for cases where you really do want toocount hello number of rows in a group.</span></span>

<span data-ttu-id="d601a-200">Det finns en mängd [aggregeringsfunktioner](https://docs.loganalytics.io/learn/tutorials/aggregations.html).</span><span class="sxs-lookup"><span data-stu-id="d601a-200">There's a range of [aggregation functions](https://docs.loganalytics.io/learn/tutorials/aggregations.html).</span></span>

## <a name="charting-hello-results"></a><span data-ttu-id="d601a-201">Diagram hello resultat</span><span class="sxs-lookup"><span data-stu-id="d601a-201">Charting hello results</span></span>
```AIQL

    exceptions
       | summarize count=sum(itemCount)  
         by bin(timestamp, 1h)
```

<span data-ttu-id="d601a-202">Resultatet visas som en tabell som standard:</span><span class="sxs-lookup"><span data-stu-id="d601a-202">By default, results display as a table:</span></span>

![](./media/app-insights-analytics-tour/225.png)

<span data-ttu-id="d601a-203">Vi kan göra bättre än hello tabellvy.</span><span class="sxs-lookup"><span data-stu-id="d601a-203">We can do better than hello table view.</span></span> <span data-ttu-id="d601a-204">Vi titta på hello resulterar i hello diagramvy med hello vertikalstreck alternativet:</span><span class="sxs-lookup"><span data-stu-id="d601a-204">Let's look at hello results in hello chart view with hello vertical bar option:</span></span>

![Klicka på diagrammet och sedan välja stående stapeldiagram och tilldela x och y axlar](./media/app-insights-analytics-tour/230.png)

<span data-ttu-id="d601a-206">Observera att även om vi inte sortera hello resultaten av tid (som du ser i hello tabell visa), hello diagramvyn alltid visar datum och tid i rätt ordning.</span><span class="sxs-lookup"><span data-stu-id="d601a-206">Notice that although we didn't sort hello results by time (as you can see in hello table display), hello chart display always shows datetimes in correct order.</span></span>


## <a name="timecharts"></a><span data-ttu-id="d601a-207">Timecharts</span><span class="sxs-lookup"><span data-stu-id="d601a-207">Timecharts</span></span>
<span data-ttu-id="d601a-208">Visa det hur många händelser är varje timma:</span><span class="sxs-lookup"><span data-stu-id="d601a-208">Show how many events there are each hour:</span></span>

```AIQL

    requests
      | summarize event_count=sum(itemCount)
        by bin(timestamp, 1h)
```

<span data-ttu-id="d601a-209">Välj Visningsalternativ för hello diagram:</span><span class="sxs-lookup"><span data-stu-id="d601a-209">Select hello Chart display option:</span></span>

![timechart](./media/app-insights-analytics-tour/080.png)

## <a name="multiple-series"></a><span data-ttu-id="d601a-211">Flera serier</span><span class="sxs-lookup"><span data-stu-id="d601a-211">Multiple series</span></span>
<span data-ttu-id="d601a-212">Flera uttryck i hello `summarize` satsen skapar flera kolumner.</span><span class="sxs-lookup"><span data-stu-id="d601a-212">Multiple expressions in hello `summarize` clause creates multiple columns.</span></span>

<span data-ttu-id="d601a-213">Flera uttryck i hello `by` satsen skapar flera rader, ett för varje kombination av värden.</span><span class="sxs-lookup"><span data-stu-id="d601a-213">Multiple expressions in hello `by` clause creates multiple rows, one for each combination of values.</span></span>

```AIQL

    requests
    | summarize count_=sum(itemCount), avg(duration)
      by bin(timestamp, 1h), client_StateOrProvince, client_City
    | order by timestamp asc, client_StateOrProvince, client_City
```

![Tabell för förfrågningar efter timme och plats](./media/app-insights-analytics-tour/090.png)

### <a name="segment-a-chart-by-dimensions"></a><span data-ttu-id="d601a-215">Segmentera ett diagram av dimensioner</span><span class="sxs-lookup"><span data-stu-id="d601a-215">Segment a chart by dimensions</span></span>
<span data-ttu-id="d601a-216">Om diagrammet en tabell som har en strängkolumn och en numerisk kolumn hello sträng kan använda toosplit hello numeriska data till olika antal datapunkter.</span><span class="sxs-lookup"><span data-stu-id="d601a-216">If you chart a table that has a string column and a numeric column, hello string can be used toosplit hello numeric data into separate series of points.</span></span> <span data-ttu-id="d601a-217">Om det finns fler än en strängkolumn, kan du välja vilka kolumnen toouse som hello diskriminator.</span><span class="sxs-lookup"><span data-stu-id="d601a-217">If there's more than one string column, you can choose which column toouse as hello discriminator.</span></span>

![Segmentera ett analytics-diagram](./media/app-insights-analytics-tour/100.png)

#### <a name="bounce-rate"></a><span data-ttu-id="d601a-219">Studs hastighet</span><span class="sxs-lookup"><span data-stu-id="d601a-219">Bounce rate</span></span>

<span data-ttu-id="d601a-220">Konvertera en boolesk tooa sträng toouse den som en diskriminator:</span><span class="sxs-lookup"><span data-stu-id="d601a-220">Convert a boolean tooa string toouse it as a discriminator:</span></span>

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

### <a name="display-multiple-metrics"></a><span data-ttu-id="d601a-221">Visa flera mått</span><span class="sxs-lookup"><span data-stu-id="d601a-221">Display multiple metrics</span></span>
<span data-ttu-id="d601a-222">Om du diagram av en tabell som har mer än en numerisk kolumn i tillägg toohello tidsstämpel och kan du visa en kombination av dessa.</span><span class="sxs-lookup"><span data-stu-id="d601a-222">If you chart a table that has more than one numeric column, in addition toohello timestamp, you can display any combination of them.</span></span>

![Segmentera ett analytics-diagram](./media/app-insights-analytics-tour/110.png)

<span data-ttu-id="d601a-224">Du måste välja **inte dela** innan du kan välja flera numeriska kolumner.</span><span class="sxs-lookup"><span data-stu-id="d601a-224">You must select **Don't Split** before you can select multiple numeric columns.</span></span> <span data-ttu-id="d601a-225">Du kan inte dela genom en strängkolumn på hello samma tid som visas mer än en numerisk kolumn.</span><span class="sxs-lookup"><span data-stu-id="d601a-225">You can't split by a string column at hello same time as displaying more than one numeric column.</span></span>

## <a name="daily-average-cycle"></a><span data-ttu-id="d601a-226">Dagliga genomsnittliga cykel</span><span class="sxs-lookup"><span data-stu-id="d601a-226">Daily average cycle</span></span>
<span data-ttu-id="d601a-227">Hur varierar användning över hello genomsnittlig dag?</span><span class="sxs-lookup"><span data-stu-id="d601a-227">How does usage vary over hello average day?</span></span>

<span data-ttu-id="d601a-228">Antal begäranden med hello tid modulo en dag binned i timmar:</span><span class="sxs-lookup"><span data-stu-id="d601a-228">Count requests by hello time modulo one day, binned into hours:</span></span>

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
> <span data-ttu-id="d601a-230">Observera att för närvarande har vi tooconvert tid varaktigheter toodatetimes i ordning toodisplay på ett linjediagram.</span><span class="sxs-lookup"><span data-stu-id="d601a-230">Notice that we currently have tooconvert time durations toodatetimes in order toodisplay on a line chart.</span></span>
>
>

## <a name="compare-multiple-daily-series"></a><span data-ttu-id="d601a-231">Jämför flera dagliga serier</span><span class="sxs-lookup"><span data-stu-id="d601a-231">Compare multiple daily series</span></span>
<span data-ttu-id="d601a-232">Hur användning varierar över hello tid på dagen i olika länder?</span><span class="sxs-lookup"><span data-stu-id="d601a-232">How does usage vary over hello time of day in different countries?</span></span>

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

## <a name="plot-a-distribution"></a><span data-ttu-id="d601a-234">Rita en distributionsplats</span><span class="sxs-lookup"><span data-stu-id="d601a-234">Plot a distribution</span></span>
<span data-ttu-id="d601a-235">Hur många sessioner är det av olika längd?</span><span class="sxs-lookup"><span data-stu-id="d601a-235">How many sessions are there of different lengths?</span></span>

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

<span data-ttu-id="d601a-236">hello sista raden är nödvändiga tooconvert toodatetime.</span><span class="sxs-lookup"><span data-stu-id="d601a-236">hello last line is required tooconvert toodatetime.</span></span> <span data-ttu-id="d601a-237">För närvarande visas hello x-axeln i ett diagram som en skalär endast om det är ett datetime-värde.</span><span class="sxs-lookup"><span data-stu-id="d601a-237">Currently hello x axis of a chart is displayed as a scalar only if it is a datetime.</span></span>

<span data-ttu-id="d601a-238">Hej `where` satsen utesluter one-shot sessioner (sessionDuration == 0) och anger hello längd hello x-axeln.</span><span class="sxs-lookup"><span data-stu-id="d601a-238">hello `where` clause excludes one-shot sessions (sessionDuration==0) and sets hello length of hello x-axis.</span></span>

![](./media/app-insights-analytics-tour/290.png)

## <a name="percentileshttpsdocsloganalyticsioquerylanguagequerylanguagepercentilesaggfunctionhtml"></a>[<span data-ttu-id="d601a-239">Percentiler</span><span class="sxs-lookup"><span data-stu-id="d601a-239">Percentiles</span></span>](https://docs.loganalytics.io/queryLanguage/query_language_percentiles_aggfunction.html)
<span data-ttu-id="d601a-240">Vilka områden för varaktigheter täcka olika delar av sessioner?</span><span class="sxs-lookup"><span data-stu-id="d601a-240">What ranges of durations cover different percentages of sessions?</span></span>

<span data-ttu-id="d601a-241">Använda hello ovan frågan, men ersätt hello sista raden:</span><span class="sxs-lookup"><span data-stu-id="d601a-241">Use hello above query, but replace hello last line:</span></span>

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

<span data-ttu-id="d601a-242">Vi också bort hello övre gräns på hello där sats i ordning tooget korrigera uppgifter som inkluderar alla sessioner med mer än en förfrågan:</span><span class="sxs-lookup"><span data-stu-id="d601a-242">We also removed hello upper limit in hello where clause, in order tooget correct figures including all sessions with more than one request:</span></span>

![Resultat](./media/app-insights-analytics-tour/180.png)

<span data-ttu-id="d601a-244">Där hittar som:</span><span class="sxs-lookup"><span data-stu-id="d601a-244">From which we can see that:</span></span>

* <span data-ttu-id="d601a-245">5% av sessioner har en varaktighet på mindre än 3 minuter 34s;</span><span class="sxs-lookup"><span data-stu-id="d601a-245">5% of sessions have a duration of less than 3 minutes 34s;</span></span>
* <span data-ttu-id="d601a-246">50% av sessioner senaste 36 minuter.</span><span class="sxs-lookup"><span data-stu-id="d601a-246">50% of sessions last less than 36 minutes;</span></span>
* <span data-ttu-id="d601a-247">5% av sessioner senaste mer än 7 dagar</span><span class="sxs-lookup"><span data-stu-id="d601a-247">5% of sessions last more than 7 days</span></span>

<span data-ttu-id="d601a-248">tooget en separat uppdelning för varje land, vi har precis toobring hello client_CountryOrRegion kolumn separat via båda sammanfatta operatorer:</span><span class="sxs-lookup"><span data-stu-id="d601a-248">tooget a separate breakdown for each country, we just have toobring hello client_CountryOrRegion column separately through both summarize operators:</span></span>

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

## <a name="join"></a><span data-ttu-id="d601a-249">Slå ihop</span><span class="sxs-lookup"><span data-stu-id="d601a-249">Join</span></span>
<span data-ttu-id="d601a-250">Vi har åtkomst tooseveral tabeller, inklusive begäranden och undantag.</span><span class="sxs-lookup"><span data-stu-id="d601a-250">We have access tooseveral tables, including requests and exceptions.</span></span>

<span data-ttu-id="d601a-251">toofind hello undantag relaterade tooa begäran som returnerade ett fel svar, vi kan koppla hello tabeller på `session_Id`:</span><span class="sxs-lookup"><span data-stu-id="d601a-251">toofind hello exceptions related tooa request that returned a failure response, we can join hello tables on `session_Id`:</span></span>

```AIQL

    requests
    | where toint(resultCode) >= 500
    | join (exceptions) on operation_Id
    | take 30
```


<span data-ttu-id="d601a-252">Det är god sed toouse `project` tooselect bara hello kolumner som vi behöver innan du utför hello koppling.</span><span class="sxs-lookup"><span data-stu-id="d601a-252">It's good practice toouse `project` tooselect just hello columns we need before performing hello join.</span></span>
<span data-ttu-id="d601a-253">I hello samma satser vi byta namn på hello kolumn för tidsstämpel.</span><span class="sxs-lookup"><span data-stu-id="d601a-253">In hello same clauses, we rename hello timestamp column.</span></span>

## <a name="lethttpsdocsloganalyticsioquerylanguagequerylanguageletstatementhtml-assign-a-result-tooa-variable"></a><span data-ttu-id="d601a-254">[Låt](https://docs.loganalytics.io/queryLanguage/query_language_letstatement.html): tilldela en variabel för resultatet tooa</span><span class="sxs-lookup"><span data-stu-id="d601a-254">[Let](https://docs.loganalytics.io/queryLanguage/query_language_letstatement.html): Assign a result tooa variable</span></span>

<span data-ttu-id="d601a-255">Använd `let` tooseparate hello delar av hello föregående uttryck.</span><span class="sxs-lookup"><span data-stu-id="d601a-255">Use `let` tooseparate out hello parts of hello previous expression.</span></span> <span data-ttu-id="d601a-256">hello resultatet är oförändrad:</span><span class="sxs-lookup"><span data-stu-id="d601a-256">hello results are unchanged:</span></span>

```AIQL

    let bad_requests =
      requests
        | where  toint(resultCode) >= 500  ;
    bad_requests
    | join (exceptions) on session_Id
    | take 30
```

> [!Tip] 
> <span data-ttu-id="d601a-257">Hello Analytics-klienten Placera inte tomma rader mellan hello delar av hello fråga.</span><span class="sxs-lookup"><span data-stu-id="d601a-257">In hello Analytics client, don't put blank lines between hello parts of hello query.</span></span> <span data-ttu-id="d601a-258">Se till att tooexecute alla.</span><span class="sxs-lookup"><span data-stu-id="d601a-258">Make sure tooexecute all of it.</span></span>
>

<span data-ttu-id="d601a-259">Använd `toscalar` tooconvert en enskild tabell tooa Cellvärde:</span><span class="sxs-lookup"><span data-stu-id="d601a-259">Use `toscalar` tooconvert a single table cell tooa value:</span></span>

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


### <a name="functions"></a><span data-ttu-id="d601a-260">Funktioner</span><span class="sxs-lookup"><span data-stu-id="d601a-260">Functions</span></span>

<span data-ttu-id="d601a-261">Använd *kan* toodefine en funktion:</span><span class="sxs-lookup"><span data-stu-id="d601a-261">Use *Let* toodefine a function:</span></span>

```AIQL

    let usdate = (t:datetime)
    {
      strcat(getmonth(t), "/", dayofmonth(t),"/", getyear(t), " ",
      bin((t-1h)%12h+1h,1s), iff(t%24h<12h, "AM", "PM"))
    };
    requests  
    | extend PST = usdate(timestamp-8h)
```

## <a name="accessing-nested-objects"></a><span data-ttu-id="d601a-262">Åtkomst till kapslade objekt</span><span class="sxs-lookup"><span data-stu-id="d601a-262">Accessing nested objects</span></span>
<span data-ttu-id="d601a-263">Kapslade objekt kan nås enkelt.</span><span class="sxs-lookup"><span data-stu-id="d601a-263">Nested objects can be accessed easily.</span></span> <span data-ttu-id="d601a-264">Till exempel i hello undantag dataströmmen ser du strukturerade objekt så här:</span><span class="sxs-lookup"><span data-stu-id="d601a-264">For example, in hello exceptions stream you can see structured objects like this:</span></span>

![Resultat](./media/app-insights-analytics-tour/520.png)

<span data-ttu-id="d601a-266">Du kan förenkla den genom att välja hello egenskaper som du är intresserad av:</span><span class="sxs-lookup"><span data-stu-id="d601a-266">You can flatten it by choosing hello properties you're interested in:</span></span>

```AIQL

    exceptions | take 10
    | extend method1 = tostring(details[0].parsedStack[1].method)
```

<span data-ttu-id="d601a-267">Observera att du måste toocast hello toohello lämplig resultattyp.</span><span class="sxs-lookup"><span data-stu-id="d601a-267">Note that you need toocast hello result toohello appropriate type.</span></span>


## <a name="custom-properties-and-measurements"></a><span data-ttu-id="d601a-268">Anpassade egenskaper och mått</span><span class="sxs-lookup"><span data-stu-id="d601a-268">Custom properties and measurements</span></span>
<span data-ttu-id="d601a-269">Om programmet ansluts [anpassade dimensioner (Egenskaper) och anpassade mått](app-insights-api-custom-events-metrics.md#properties) tooevents och du kommer att se dem i hello `customDimensions` och `customMeasurements` objekt.</span><span class="sxs-lookup"><span data-stu-id="d601a-269">If your application attaches [custom dimensions (properties) and custom measurements](app-insights-api-custom-events-metrics.md#properties) tooevents, then you will see them in hello `customDimensions` and `customMeasurements` objects.</span></span>

<span data-ttu-id="d601a-270">Till exempel om appen innehåller:</span><span class="sxs-lookup"><span data-stu-id="d601a-270">For example, if your app includes:</span></span>

```C#

    var dimensions = new Dictionary<string, string>
                     {{"p1", "v1"},{"p2", "v2"}};
    var measurements = new Dictionary<string, double>
                     {{"m1", 42.0}, {"m2", 43.2}};
    telemetryClient.TrackEvent("myEvent", dimensions, measurements);
```

<span data-ttu-id="d601a-271">tooextract värdena i Analytics:</span><span class="sxs-lookup"><span data-stu-id="d601a-271">tooextract these values in Analytics:</span></span>

```AIQL

    customEvents
    | extend p1 = customDimensions.p1,
      m1 = todouble(customMeasurements.m1) // cast tooexpected type

```

<span data-ttu-id="d601a-272">tooverify om en anpassad dimensionen är av en viss typ:</span><span class="sxs-lookup"><span data-stu-id="d601a-272">tooverify whether a custom dimension is of a particular type:</span></span>

```AIQL

    customEvents
    | extend p1 = customDimensions.p1,
      iff(notnull(todouble(customMeasurements.m1)), ...
```

## <a name="dashboards"></a><span data-ttu-id="d601a-273">Instrumentpaneler</span><span class="sxs-lookup"><span data-stu-id="d601a-273">Dashboards</span></span>
<span data-ttu-id="d601a-274">Du kan fästa resultat tooa instrumentpanelen i ordning toobring ihop alla dina viktigaste diagram och tabeller.</span><span class="sxs-lookup"><span data-stu-id="d601a-274">You can pin your results tooa dashboard in order toobring together all your most important charts and tables.</span></span>

* <span data-ttu-id="d601a-275">[Azure delade instrumentpanelen](app-insights-dashboards.md#share-dashboards): Klicka på ikonen för hello PIN-kod.</span><span class="sxs-lookup"><span data-stu-id="d601a-275">[Azure shared dashboard](app-insights-dashboards.md#share-dashboards): Click hello pin icon.</span></span> <span data-ttu-id="d601a-276">Innan du gör det måste du ha en delad instrumentpanel.</span><span class="sxs-lookup"><span data-stu-id="d601a-276">Before you do this, you must have a shared dashboard.</span></span> <span data-ttu-id="d601a-277">Öppna hello Azure-portalen, eller skapa en instrumentpanel och klicka på resursen.</span><span class="sxs-lookup"><span data-stu-id="d601a-277">In hello Azure portal, open or create a dashboard and click Share.</span></span>
* <span data-ttu-id="d601a-278">[Power BI-instrumentpanel](app-insights-export-power-bi.md): Klicka på Exportera, Power BI-fråga.</span><span class="sxs-lookup"><span data-stu-id="d601a-278">[Power BI dashboard](app-insights-export-power-bi.md): Click Export, Power BI Query.</span></span> <span data-ttu-id="d601a-279">En fördel med detta alternativ är att du kan visa din fråga tillsammans med andra resultat från en mängd olika källor.</span><span class="sxs-lookup"><span data-stu-id="d601a-279">An advantage of this alternative is that you can display your query alongside other results from a wide range of sources.</span></span>

## <a name="combine-with-imported-data"></a><span data-ttu-id="d601a-280">Kombinera med importerade data</span><span class="sxs-lookup"><span data-stu-id="d601a-280">Combine with imported data</span></span>

<span data-ttu-id="d601a-281">Analytics-rapporter se bra ut på hello instrumentpanelen, men ibland vill du tootranslate hello data tooa flera datamängder formuläret.</span><span class="sxs-lookup"><span data-stu-id="d601a-281">Analytics reports look great on hello dashboard, but sometimes you want tootranslate hello data tooa more digestible form.</span></span> <span data-ttu-id="d601a-282">Anta exempelvis att din autentiserade användare identifieras i hello telemetri av ett alias.</span><span class="sxs-lookup"><span data-stu-id="d601a-282">For example, suppose your authenticated users are identified in hello telemetry by an alias.</span></span> <span data-ttu-id="d601a-283">Du vill att tooshow sitt verkliga namn i resultaten.</span><span class="sxs-lookup"><span data-stu-id="d601a-283">You'd like tooshow their real names in your results.</span></span> <span data-ttu-id="d601a-284">toodo, du behöver en CSV-fil som kopplar från hello-alias toohello verkliga namn.</span><span class="sxs-lookup"><span data-stu-id="d601a-284">toodo this, you need a CSV file that maps from hello aliases toohello real names.</span></span>

<span data-ttu-id="d601a-285">Du kan importera en fil och använda den precis som alla hello standardtabeller (begäranden, undantag och så vidare).</span><span class="sxs-lookup"><span data-stu-id="d601a-285">You can import a data file and use it just like any of hello standard tables (requests, exceptions, and so on).</span></span> <span data-ttu-id="d601a-286">Antingen fråga på sin egen eller Anslut den med andra tabeller.</span><span class="sxs-lookup"><span data-stu-id="d601a-286">Either query it on its own, or join it with other tables.</span></span> <span data-ttu-id="d601a-287">Om du har en tabell med namnet usermap och kolumner har till exempel `realName` och `userId`, kan du använda den tootranslate hello `user_AuthenticatedId` i hello begärandetelemetri:</span><span class="sxs-lookup"><span data-stu-id="d601a-287">For example, if you have a table named usermap, and it has columns `realName` and `userId`, then you can use it tootranslate hello `user_AuthenticatedId` field in hello request telemetry:</span></span>

```AIQL

    requests
    | where notempty(user_AuthenticatedId)
    | project userId = user_AuthenticatedId
      // get hello realName field from hello usermap table:
    | join kind=leftouter ( usermap ) on userId
      // count transactions by name:
    | summarize count() by realName
```

<span data-ttu-id="d601a-288">tooimport en tabell i hello schemat bladet under **andra datakällor**, följ hello instruktioner tooadd en ny datakälla genom att överföra ett urval av dina data.</span><span class="sxs-lookup"><span data-stu-id="d601a-288">tooimport a table, in hello Schema blade, under **Other Data Sources**, follow hello instructions tooadd a new data source, by uploading a sample of your data.</span></span> <span data-ttu-id="d601a-289">Du kan sedan använda den här definitionen tooupload tabeller.</span><span class="sxs-lookup"><span data-stu-id="d601a-289">Then you can use this definition tooupload tables.</span></span>

<span data-ttu-id="d601a-290">hello importfunktionen genomgår förhandsgranskning för närvarande, så visas först en ”Kontakta oss” länk under ”andra datakällor”.</span><span class="sxs-lookup"><span data-stu-id="d601a-290">hello import feature is currently in preview, so you will initially see a "Contact us" link under "Other data sources."</span></span> <span data-ttu-id="d601a-291">Använd den här toosign in toohello förhandsgranskningsprogrammet och hello länken kommer sedan att ersättas av knappen ”Lägg till ny datakälla”.</span><span class="sxs-lookup"><span data-stu-id="d601a-291">Use this toosign up toohello preview program, and hello link will then be replaced by an "Add new data source" button.</span></span>


## <a name="tables"></a><span data-ttu-id="d601a-292">Tabeller</span><span class="sxs-lookup"><span data-stu-id="d601a-292">Tables</span></span>
<span data-ttu-id="d601a-293">hello dataström telemetri från din app är tillgängligt via flera tabeller.</span><span class="sxs-lookup"><span data-stu-id="d601a-293">hello stream of telemetry received from your app is accessible through several tables.</span></span> <span data-ttu-id="d601a-294">hello schemat för egenskaper som är tillgängliga för varje tabell är synlig i hello till vänster i fönstret hello.</span><span class="sxs-lookup"><span data-stu-id="d601a-294">hello schema of properties available for each table is visible at hello left of hello window.</span></span>

### <a name="requests-table"></a><span data-ttu-id="d601a-295">Begäranden tabell</span><span class="sxs-lookup"><span data-stu-id="d601a-295">Requests table</span></span>
<span data-ttu-id="d601a-296">Antal HTTP-begäranden tooyour webbapp och segment per sidnamn:</span><span class="sxs-lookup"><span data-stu-id="d601a-296">Count HTTP requests tooyour web app and segment by page name:</span></span>

![Antal begäranden segmenterat efter namn](./media/app-insights-analytics-tour/analytics-count-requests.png)

<span data-ttu-id="d601a-298">Sök efter hello-begäranden som inte uppfyller de flesta:</span><span class="sxs-lookup"><span data-stu-id="d601a-298">Find hello requests that fail most:</span></span>

![Antal begäranden segmenterat efter namn](./media/app-insights-analytics-tour/analytics-failed-requests.png)

### <a name="custom-events-table"></a><span data-ttu-id="d601a-300">Anpassade händelser tabell</span><span class="sxs-lookup"><span data-stu-id="d601a-300">Custom events table</span></span>
<span data-ttu-id="d601a-301">Om du använder [trackevent ()](app-insights-api-custom-events-metrics.md#trackevent) toosend egna händelser, du kan läsa dem i den här tabellen.</span><span class="sxs-lookup"><span data-stu-id="d601a-301">If you use [TrackEvent()](app-insights-api-custom-events-metrics.md#trackevent) toosend your own events, you can read them from this table.</span></span>

<span data-ttu-id="d601a-302">Låt oss ta ett exempel där app-koden innehåller dessa rader:</span><span class="sxs-lookup"><span data-stu-id="d601a-302">Let's take an example where your app code contains these lines:</span></span>

```C#

    telemetry.TrackEvent("Query",
       new Dictionary<string,string> {{"query", sqlCmd}},
       new Dictionary<string,double> {
           {"retry", retryCount},
           {"querytime", totalTime}})
```

<span data-ttu-id="d601a-303">Visar hello frekvensen för dessa händelser:</span><span class="sxs-lookup"><span data-stu-id="d601a-303">Display hello frequency of these events:</span></span>

![Visa antal anpassade händelser](./media/app-insights-analytics-tour/analytics-custom-events-rate.png)

<span data-ttu-id="d601a-305">Extrahera mått och dimensioner från hello händelser:</span><span class="sxs-lookup"><span data-stu-id="d601a-305">Extract measurements and dimensions from hello events:</span></span>

![Visa antal anpassade händelser](./media/app-insights-analytics-tour/analytics-custom-events-dimensions.png)

### <a name="custom-metrics-table"></a><span data-ttu-id="d601a-307">Anpassade mått tabell</span><span class="sxs-lookup"><span data-stu-id="d601a-307">Custom metrics table</span></span>
<span data-ttu-id="d601a-308">Om du använder [TrackMetric()](app-insights-api-custom-events-metrics.md#trackmetric) toosend egna måttvärden hittar du resultaten i hello **customMetrics** dataströmmen.</span><span class="sxs-lookup"><span data-stu-id="d601a-308">If you are using [TrackMetric()](app-insights-api-custom-events-metrics.md#trackmetric) toosend your own metric values, you’ll find its results in hello **customMetrics** stream.</span></span> <span data-ttu-id="d601a-309">Exempel:</span><span class="sxs-lookup"><span data-stu-id="d601a-309">For example:</span></span>  

![Anpassade mått i Application Insights analytics](./media/app-insights-analytics-tour/analytics-custom-metrics.png)

> [!NOTE]
> <span data-ttu-id="d601a-311">I [Metrics Explorer](app-insights-metrics-explorer.md), alla anpassade mått bifogade tooany telemetri visas tillsammans i hello mått bladet tillsammans med mått som skickas med `TrackMetric()`.</span><span class="sxs-lookup"><span data-stu-id="d601a-311">In [Metrics Explorer](app-insights-metrics-explorer.md), all custom measurements attached tooany type of telemetry appear together in hello metrics blade along with metrics sent using `TrackMetric()`.</span></span> <span data-ttu-id="d601a-312">Men i Analytics, anpassade mått är fortfarande kopplad toowhichever typ av telemetri som de har gjort på - händelser eller begäranden och så vidare - medan mått som skickats av TrackMetric visas i sina egna dataströmmen.</span><span class="sxs-lookup"><span data-stu-id="d601a-312">But in Analytics, custom measurements are still attached toowhichever type of telemetry they were carried on - events or requests, and so on - while metrics sent by TrackMetric appear in their own stream.</span></span>
>
>

### <a name="performance-counters-table"></a><span data-ttu-id="d601a-313">Prestandaräknare tabell</span><span class="sxs-lookup"><span data-stu-id="d601a-313">Performance counters table</span></span>
<span data-ttu-id="d601a-314">[Prestandaräknare](app-insights-performance-counters.md) visa grundläggande system mätvärden för din app, t.ex CPU eller minne och nätverksanvändning.</span><span class="sxs-lookup"><span data-stu-id="d601a-314">[Performance counters](app-insights-performance-counters.md) show you basic system metrics for your app, such as CPU, memory, and network utilization.</span></span> <span data-ttu-id="d601a-315">Du kan konfigurera hello SDK toosend ytterligare räknare, inklusive din egen anpassade räknare.</span><span class="sxs-lookup"><span data-stu-id="d601a-315">You can configure hello SDK toosend additional counters, including your own custom counters.</span></span>

<span data-ttu-id="d601a-316">Hej **performanceCounters** schemat visar hello `category`, `counter` namn, och `instance` namn för varje prestandaräknare.</span><span class="sxs-lookup"><span data-stu-id="d601a-316">hello **performanceCounters** schema exposes hello `category`, `counter` name, and `instance` name of each performance counter.</span></span> <span data-ttu-id="d601a-317">Namn på prestandaräknarinstanser är endast tillämplig toosome prestandaräknare och anger vanligtvis hello namnet på hello process toowhich hello antal avser.</span><span class="sxs-lookup"><span data-stu-id="d601a-317">Counter instance names are only applicable toosome performance counters, and typically indicate hello name of hello process toowhich hello count relates.</span></span> <span data-ttu-id="d601a-318">I hello telemetri för varje program ser du bara hello räknare för programmet.</span><span class="sxs-lookup"><span data-stu-id="d601a-318">In hello telemetry for each application, you’ll see only hello counters for that application.</span></span> <span data-ttu-id="d601a-319">Till exempel toosee vilka räknare är tillgängliga:</span><span class="sxs-lookup"><span data-stu-id="d601a-319">For example, toosee what counters are available:</span></span>

![Prestandaräknare i Application Insights analytics](./media/app-insights-analytics-tour/analytics-performance-counters.png)

<span data-ttu-id="d601a-321">tooget ett diagram av minne över hello vald period:</span><span class="sxs-lookup"><span data-stu-id="d601a-321">tooget a chart of available memory over hello selected period:</span></span>

![Minne timechart i Application Insights analytics](./media/app-insights-analytics-tour/analytics-available-memory.png)

<span data-ttu-id="d601a-323">Som andra telemetri **performanceCounters** också har en kolumn `cloud_RoleInstance` som visar hello identitet hello värddatorn som appen körs.</span><span class="sxs-lookup"><span data-stu-id="d601a-323">Like other telemetry, **performanceCounters** also has a column `cloud_RoleInstance` that indicates hello identity of hello host machine on which your app is running.</span></span> <span data-ttu-id="d601a-324">Till exempel toocompare hello prestandan för din app på hello olika datorer:</span><span class="sxs-lookup"><span data-stu-id="d601a-324">For example, toocompare hello performance of your app on hello different machines:</span></span>

![Prestanda åtskilda med rollinstans i Application Insights analytics](./media/app-insights-analytics-tour/analytics-metrics-role-instance.png)

### <a name="exceptions-table"></a><span data-ttu-id="d601a-326">Feltabellen</span><span class="sxs-lookup"><span data-stu-id="d601a-326">Exceptions table</span></span>
<span data-ttu-id="d601a-327">[Undantag som rapporterats av din app](app-insights-asp-net-exceptions.md) är tillgängliga i den här tabellen.</span><span class="sxs-lookup"><span data-stu-id="d601a-327">[Exceptions reported by your app](app-insights-asp-net-exceptions.md) are available in this table.</span></span>

<span data-ttu-id="d601a-328">toofind Hej HTTP-begäran som din app hantering när hello undantagsfel inträffade, delta i operation_Id:</span><span class="sxs-lookup"><span data-stu-id="d601a-328">toofind hello HTTP request that your app was handling when hello exception was raised, join on operation_Id:</span></span>

![Ansluta till undantag med begäranden på operation_Id](./media/app-insights-analytics-tour/analytics-exception-request.png)

### <a name="browser-timings-table"></a><span data-ttu-id="d601a-330">Webbläsaren tidsinställningar för tabellen</span><span class="sxs-lookup"><span data-stu-id="d601a-330">Browser timings table</span></span>
<span data-ttu-id="d601a-331">`browserTimings`Visar sidan Läs in data som samlas in i användarnas webbläsare.</span><span class="sxs-lookup"><span data-stu-id="d601a-331">`browserTimings` shows page load data collected in your users' browsers.</span></span>

<span data-ttu-id="d601a-332">[Konfigurera din app för klientsidan telemetri](app-insights-javascript.md) ordning toosee dessa mått.</span><span class="sxs-lookup"><span data-stu-id="d601a-332">[Set up your app for client-side telemetry](app-insights-javascript.md) in order toosee these metrics.</span></span>

<span data-ttu-id="d601a-333">hello-schemat innehåller [mått som anger hello längder i olika faser i hello sidan processen för inläsning](app-insights-javascript.md#page-load-performance).</span><span class="sxs-lookup"><span data-stu-id="d601a-333">hello schema includes [metrics indicating hello lengths of different stages of hello page loading process](app-insights-javascript.md#page-load-performance).</span></span> <span data-ttu-id="d601a-334">(De inte visar hello tidslängd användarna läsa en sida.)</span><span class="sxs-lookup"><span data-stu-id="d601a-334">(They don’t indicate hello length of time your users read a page.)</span></span>  

<span data-ttu-id="d601a-335">Visa hello popularities på olika sidor och läsa in tider för varje sida:</span><span class="sxs-lookup"><span data-stu-id="d601a-335">Show hello popularities of different pages, and load times for each page:</span></span>

![Sidinläsningstider i Analytics](./media/app-insights-analytics-tour/analytics-page-load.png)

### <a name="availability-results-table"></a><span data-ttu-id="d601a-337">Tillgänglighet resultattabellen</span><span class="sxs-lookup"><span data-stu-id="d601a-337">Availability results table</span></span>
<span data-ttu-id="d601a-338">`availabilityResults`Visar hello resultatet av dina [webbtester](app-insights-monitor-web-app-availability.md).</span><span class="sxs-lookup"><span data-stu-id="d601a-338">`availabilityResults` shows hello results of your [web tests](app-insights-monitor-web-app-availability.md).</span></span> <span data-ttu-id="d601a-339">Varje körning av dina tester från varje test plats rapporteras separat.</span><span class="sxs-lookup"><span data-stu-id="d601a-339">Each run of your tests from each test location is reported separately.</span></span>

![Sidinläsningstider i Analytics](./media/app-insights-analytics-tour/analytics-availability.png)

### <a name="dependencies-table"></a><span data-ttu-id="d601a-341">Beroenden tabell</span><span class="sxs-lookup"><span data-stu-id="d601a-341">Dependencies table</span></span>
<span data-ttu-id="d601a-342">Innehåller resultatet av anrop som din app gör toodatabases och REST API: er och andra anropar tooTrackDependency().</span><span class="sxs-lookup"><span data-stu-id="d601a-342">Contains results of calls that your app makes toodatabases and REST APIs, and other calls tooTrackDependency().</span></span> <span data-ttu-id="d601a-343">Även AJAX-anrop från hello webbläsare.</span><span class="sxs-lookup"><span data-stu-id="d601a-343">Also includes AJAX calls made from hello browser.</span></span>

<span data-ttu-id="d601a-344">AJAX-anrop från hello webbläsare:</span><span class="sxs-lookup"><span data-stu-id="d601a-344">AJAX calls from hello browser:</span></span>

```AIQL

    dependencies | where client_Type == "Browser"
    | take 10
```

<span data-ttu-id="d601a-345">Beroendeanrop från hello-servern:</span><span class="sxs-lookup"><span data-stu-id="d601a-345">Dependency calls from hello server:</span></span>

```AIQL

    dependencies | where client_Type == "PC"
    | take 10
```

<span data-ttu-id="d601a-346">Beroende på serversidan resultatet visar alltid `success==False` om hello Application Insights Agent är inte installerat.</span><span class="sxs-lookup"><span data-stu-id="d601a-346">Server-side dependency results always show `success==False` if hello Application Insights Agent is not installed.</span></span> <span data-ttu-id="d601a-347">Dock är hello andra data korrekta.</span><span class="sxs-lookup"><span data-stu-id="d601a-347">However, hello other data are correct.</span></span>

### <a name="traces-table"></a><span data-ttu-id="d601a-348">Spår tabell</span><span class="sxs-lookup"><span data-stu-id="d601a-348">Traces table</span></span>
<span data-ttu-id="d601a-349">Innehåller hello telemetri som skickats av din app att använda TrackTrace(), eller [andra loggning ramverk](app-insights-asp-net-trace-logs.md).</span><span class="sxs-lookup"><span data-stu-id="d601a-349">Contains hello telemetry sent by your app using TrackTrace(), or [other logging frameworks](app-insights-asp-net-trace-logs.md).</span></span>

## <a name="video"></a><span data-ttu-id="d601a-350">Video</span><span class="sxs-lookup"><span data-stu-id="d601a-350">Video</span></span> 

> [!VIDEO https://channel9.msdn.com/events/Connect/2016/123/player] 

<span data-ttu-id="d601a-351">Avancerade frågor:</span><span class="sxs-lookup"><span data-stu-id="d601a-351">Advanced queries:</span></span>

> [!VIDEO https://channel9.msdn.com/Events/Build/2016/P591/player]


## <a name="next-steps"></a><span data-ttu-id="d601a-352">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="d601a-352">Next steps</span></span>
* [<span data-ttu-id="d601a-353">Språkreferens för Analytics</span><span class="sxs-lookup"><span data-stu-id="d601a-353">Analytics language reference</span></span>](app-insights-analytics-reference.md)
* <span data-ttu-id="d601a-354">[SQL-användarnas cheat blad](https://aka.ms/sql-analytics) översätter hello vanligaste idioms.</span><span class="sxs-lookup"><span data-stu-id="d601a-354">[SQL-users' cheat sheet](https://aka.ms/sql-analytics) translates hello most common idioms.</span></span>

[!INCLUDE [app-insights-analytics-footer](../../includes/app-insights-analytics-footer.md)]
