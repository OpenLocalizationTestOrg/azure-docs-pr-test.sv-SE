---
title: "aaaUsing Analytics - hello kraftfullt sökverktyg av Azure Application Insights | Microsoft Docs"
description: "Med hjälp av hello Analytics, hello kraftfullt diagnostiska sökverktyg av Application Insights. "
services: application-insights
documentationcenter: 
author: danhadari
manager: carmonm
ms.assetid: c3b34430-f592-4c32-b900-e9f50ca096b3
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 03/14/2017
ms.author: bwren
ms.openlocfilehash: 6e0246848457db368c57d08c47b5bf73f4e5e3a3
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="using-analytics-in-application-insights"></a><span data-ttu-id="a61fd-103">Med hjälp av Analytics i Application Insights</span><span class="sxs-lookup"><span data-stu-id="a61fd-103">Using Analytics in Application Insights</span></span>
<span data-ttu-id="a61fd-104">[Analytics](app-insights-analytics.md) är hello kraftfulla sökfunktionen i [Programinsikter](app-insights-overview.md).</span><span class="sxs-lookup"><span data-stu-id="a61fd-104">[Analytics](app-insights-analytics.md) is hello powerful search feature of [Application Insights](app-insights-overview.md).</span></span> <span data-ttu-id="a61fd-105">Dessa sidor beskrivs Log Analytics-frågespråket.</span><span class="sxs-lookup"><span data-stu-id="a61fd-105">These pages describe the Log Analytics query language.</span></span>

* <span data-ttu-id="a61fd-106">**[Se hello inledande video](https://applicationanalytics-media.azureedge.net/home_page_video.mp4)**.</span><span class="sxs-lookup"><span data-stu-id="a61fd-106">**[Watch hello introductory video](https://applicationanalytics-media.azureedge.net/home_page_video.mp4)**.</span></span>
* <span data-ttu-id="a61fd-107">**[Testkör Analytics på våra simulerade data](https://analytics.applicationinsights.io/demo)**  om din app inte skickar data tooApplication insikter ännu.</span><span class="sxs-lookup"><span data-stu-id="a61fd-107">**[Test drive Analytics on our simulated data](https://analytics.applicationinsights.io/demo)** if your app isn't sending data tooApplication Insights yet.</span></span>

## <a name="open-analytics"></a><span data-ttu-id="a61fd-108">Öppna Analytics</span><span class="sxs-lookup"><span data-stu-id="a61fd-108">Open Analytics</span></span>
<span data-ttu-id="a61fd-109">Klicka på Analytics från din app hem resurs i Application Insights.</span><span class="sxs-lookup"><span data-stu-id="a61fd-109">From your app's home resource in Application Insights, click Analytics.</span></span>

![Öppna portal.azure.com, öppna Application Insights-resursen och klicka på Analytics.](./media/app-insights-analytics-using/001.png)

<span data-ttu-id="a61fd-111">hello infogade kursen får du några förslag på vad du kan göra.</span><span class="sxs-lookup"><span data-stu-id="a61fd-111">hello inline tutorial gives you some ideas about what you can do.</span></span>

<span data-ttu-id="a61fd-112">Det finns en [mer omfattande rundtur här](app-insights-analytics-tour.md).</span><span class="sxs-lookup"><span data-stu-id="a61fd-112">There's a [more extensive tour here](app-insights-analytics-tour.md).</span></span>

## <a name="query-your-telemetry"></a><span data-ttu-id="a61fd-113">Fråga din telemetri</span><span class="sxs-lookup"><span data-stu-id="a61fd-113">Query your telemetry</span></span>
### <a name="write-a-query"></a><span data-ttu-id="a61fd-114">Skriva en fråga</span><span class="sxs-lookup"><span data-stu-id="a61fd-114">Write a query</span></span>
![Visa schema](./media/app-insights-analytics-using/150.png)

<span data-ttu-id="a61fd-116">Börja med hello namnen på alla hello registren hello vänster (eller hello [intervallet](https://docs.loganalytics.io/queryLanguage/query_language_rangeoperator.html) eller [union](https://docs.loganalytics.io/queryLanguage/query_language_unionoperator.html) operatörer).</span><span class="sxs-lookup"><span data-stu-id="a61fd-116">Begin with hello names of any of hello tables listed on hello left (or hello [range](https://docs.loganalytics.io/queryLanguage/query_language_rangeoperator.html) or [union](https://docs.loganalytics.io/queryLanguage/query_language_unionoperator.html) operators).</span></span> <span data-ttu-id="a61fd-117">Använd `|` toocreate en pipeline för [operatörer](https://docs.loganalytics.io/learn/cheatsheets/useful_operators.html).</span><span class="sxs-lookup"><span data-stu-id="a61fd-117">Use `|` toocreate a pipeline of [operators](https://docs.loganalytics.io/learn/cheatsheets/useful_operators.html).</span></span> 

<span data-ttu-id="a61fd-118">IntelliSense får du hello operatorer och hello Uttryckselement som du kan använda.</span><span class="sxs-lookup"><span data-stu-id="a61fd-118">IntelliSense prompts you with hello operators and hello expression elements that you can use.</span></span> <span data-ttu-id="a61fd-119">Klicka på informationsikonen hello (eller trycker på CTRL + blanksteg) tooget en utförligare beskrivning och exempel på hur toouse varje element.</span><span class="sxs-lookup"><span data-stu-id="a61fd-119">Click hello information icon (or press CTRL+Space) tooget a longer description and examples of how toouse each element.</span></span>

<span data-ttu-id="a61fd-120">Se hello [Analytics språk rundtur](app-insights-analytics-tour.md) och [Språkreferens](app-insights-analytics-reference.md).</span><span class="sxs-lookup"><span data-stu-id="a61fd-120">See hello [Analytics language tour](app-insights-analytics-tour.md) and [language reference](app-insights-analytics-reference.md).</span></span>

### <a name="run-a-query"></a><span data-ttu-id="a61fd-121">Köra en fråga</span><span class="sxs-lookup"><span data-stu-id="a61fd-121">Run a query</span></span>
![Kör en fråga](./media/app-insights-analytics-using/130.png)

1. <span data-ttu-id="a61fd-123">Du kan använda enkel radbrytningar i en fråga.</span><span class="sxs-lookup"><span data-stu-id="a61fd-123">You can use single line breaks in a query.</span></span>
2. <span data-ttu-id="a61fd-124">Placera hello markören inuti eller hello slutet av hello-fråga som du vill använda toorun.</span><span class="sxs-lookup"><span data-stu-id="a61fd-124">Put hello cursor inside or at hello end of hello query you want toorun.</span></span>
3. <span data-ttu-id="a61fd-125">Kontrollera hello tidsintervallet på din fråga.</span><span class="sxs-lookup"><span data-stu-id="a61fd-125">Check hello time range of your query.</span></span> <span data-ttu-id="a61fd-126">(Du kan ändra det eller åsidosätta den genom att lägga till egna [ `where...timestamp...` ](https://docs.loganalytics.io/concepts/concepts_datatypes_timespan.html) -sats i frågan.)</span><span class="sxs-lookup"><span data-stu-id="a61fd-126">(You can change it, or override it by including your own [`where...timestamp...`](https://docs.loganalytics.io/concepts/concepts_datatypes_timespan.html) clause in your query.)</span></span>
3. <span data-ttu-id="a61fd-127">Klicka på Gå toorun hello fråga.</span><span class="sxs-lookup"><span data-stu-id="a61fd-127">Click Go toorun hello query.</span></span>
4. <span data-ttu-id="a61fd-128">Placera inte tomma rader i frågan.</span><span class="sxs-lookup"><span data-stu-id="a61fd-128">Don't put blank lines in your query.</span></span> <span data-ttu-id="a61fd-129">Du kan behålla flera separata frågor i en fråga flik genom att avgränsa dem med tomma rader.</span><span class="sxs-lookup"><span data-stu-id="a61fd-129">You can keep several separated queries in one query tab by separating them with blank lines.</span></span> <span data-ttu-id="a61fd-130">Endast hello-fråga som har hello markören körs.</span><span class="sxs-lookup"><span data-stu-id="a61fd-130">Only hello query that has hello cursor runs.</span></span>

### <a name="save-a-query"></a><span data-ttu-id="a61fd-131">Spara en fråga</span><span class="sxs-lookup"><span data-stu-id="a61fd-131">Save a query</span></span>
![Spara en fråga](./media/app-insights-analytics-using/140.png)

1. <span data-ttu-id="a61fd-133">Spara hello aktuella frågefilen.</span><span class="sxs-lookup"><span data-stu-id="a61fd-133">Save hello current query file.</span></span>
2. <span data-ttu-id="a61fd-134">Öppna en sparad fråga-fil.</span><span class="sxs-lookup"><span data-stu-id="a61fd-134">Open a saved query file.</span></span>
3. <span data-ttu-id="a61fd-135">Skapa en ny fråga-fil.</span><span class="sxs-lookup"><span data-stu-id="a61fd-135">Create a new query file.</span></span>

## <a name="see-hello-details"></a><span data-ttu-id="a61fd-136">Se hello information</span><span class="sxs-lookup"><span data-stu-id="a61fd-136">See hello details</span></span>
<span data-ttu-id="a61fd-137">Expandera alla rader i hello resultat toosee den fullständiga listan över egenskaper.</span><span class="sxs-lookup"><span data-stu-id="a61fd-137">Expand any row in hello results toosee its complete list of properties.</span></span> <span data-ttu-id="a61fd-138">Ytterligare kan du expandera en egenskap som är strukturerade värde: till exempel, anpassade dimensioner eller hello stack lista i ett undantag.</span><span class="sxs-lookup"><span data-stu-id="a61fd-138">You can further expand any property that is a structured value - for example, custom dimensions, or hello stack listing in an exception.</span></span>

![Expandera en rad](./media/app-insights-analytics-using/070.png)

## <a name="arrange-hello-results"></a><span data-ttu-id="a61fd-140">Ordna hello resultat</span><span class="sxs-lookup"><span data-stu-id="a61fd-140">Arrange hello results</span></span>
<span data-ttu-id="a61fd-141">Du kan sortera, filtrera, sidbryta och gruppera hello resultaten som returnerades från frågan.</span><span class="sxs-lookup"><span data-stu-id="a61fd-141">You can sort, filter, paginate, and group hello results returned from your query.</span></span>

> [!NOTE]
> <span data-ttu-id="a61fd-142">Sortering, gruppering och filtrering i hello webbläsare kör inte frågan igen.</span><span class="sxs-lookup"><span data-stu-id="a61fd-142">Sorting, grouping, and filtering in hello browser don't re-run your query.</span></span> <span data-ttu-id="a61fd-143">De endast ordna hello resultaten som returnerades av din senaste fråga.</span><span class="sxs-lookup"><span data-stu-id="a61fd-143">They only rearrange hello results that were returned by your last query.</span></span> 
> 
> <span data-ttu-id="a61fd-144">tooperform dessa uppgifter i hello server innan hello resultaten returneras, skriva en fråga med hello [sortera](https://docs.loganalytics.io/queryLanguage/query_language_sortoperator.html), [sammanfatta](https://docs.loganalytics.io/queryLanguage/query_language_summarizeoperator.html) och [där](https://docs.loganalytics.io/queryLanguage/query_language_whereoperator.html) operatörer.</span><span class="sxs-lookup"><span data-stu-id="a61fd-144">tooperform these tasks in hello server before hello results are returned, write your query with hello [sort](https://docs.loganalytics.io/queryLanguage/query_language_sortoperator.html), [summarize](https://docs.loganalytics.io/queryLanguage/query_language_summarizeoperator.html) and [where](https://docs.loganalytics.io/queryLanguage/query_language_whereoperator.html) operators.</span></span>
> 
> 

<span data-ttu-id="a61fd-145">Välj hello kolumner som du skulle t.ex. toosee, dra kolumnen huvuden toorearrange dem och ändra storlek på kolumner genom att dra gränserna.</span><span class="sxs-lookup"><span data-stu-id="a61fd-145">Pick hello columns you'd like toosee, drag column headers toorearrange them, and resize columns by dragging their borders.</span></span>

![Ordna kolumner](./media/app-insights-analytics-using/030.png)

### <a name="sort-and-filter-items"></a><span data-ttu-id="a61fd-147">Sortera och filtrera objekt</span><span class="sxs-lookup"><span data-stu-id="a61fd-147">Sort and filter items</span></span>
<span data-ttu-id="a61fd-148">Sortera resultaten genom att klicka på hello head för en kolumn.</span><span class="sxs-lookup"><span data-stu-id="a61fd-148">Sort your results by clicking hello head of a column.</span></span> <span data-ttu-id="a61fd-149">Klicka igen toosort hello annat sätt och klicka på en tredje gång toorevert toohello ursprungliga ordning returneras av frågan.</span><span class="sxs-lookup"><span data-stu-id="a61fd-149">Click again toosort hello other way, and click a third time toorevert toohello original ordering returned by your query.</span></span>

<span data-ttu-id="a61fd-150">Använd hello filtrera ikonen toonarrow sökningen.</span><span class="sxs-lookup"><span data-stu-id="a61fd-150">Use hello filter icon toonarrow your search.</span></span>

![Sortera och filtrera kolumner](./media/app-insights-analytics-using/040.png)

### <a name="group-items"></a><span data-ttu-id="a61fd-152">Gruppera objekt</span><span class="sxs-lookup"><span data-stu-id="a61fd-152">Group items</span></span>
<span data-ttu-id="a61fd-153">toosort använda gruppering av mer än en kolumn.</span><span class="sxs-lookup"><span data-stu-id="a61fd-153">toosort by more than one column, use grouping.</span></span> <span data-ttu-id="a61fd-154">Först aktivera den och dra kolumnrubrikerna till hello utrymme ovanför hello tabell.</span><span class="sxs-lookup"><span data-stu-id="a61fd-154">First enable it, and then drag column headers into hello space above hello table.</span></span>

![Grupp](./media/app-insights-analytics-using/060.png)

### <a name="missing-some-results"></a><span data-ttu-id="a61fd-156">Saknar vissa resultat?</span><span class="sxs-lookup"><span data-stu-id="a61fd-156">Missing some results?</span></span>

<span data-ttu-id="a61fd-157">Om du tror att det inte uppstår alla hello resultat som du förväntade dig, finns det några möjliga orsaker.</span><span class="sxs-lookup"><span data-stu-id="a61fd-157">If you think you're not seeing all hello results you expected, there are a couple of possible reasons.</span></span>

* <span data-ttu-id="a61fd-158">**Intervallet tidsfiltret**.</span><span class="sxs-lookup"><span data-stu-id="a61fd-158">**Time range filter**.</span></span> <span data-ttu-id="a61fd-159">Som standard endast visas resultaten från hello senaste 24 timmarna.</span><span class="sxs-lookup"><span data-stu-id="a61fd-159">By default, you will only see results from hello last 24 hours.</span></span> <span data-ttu-id="a61fd-160">Det finns en automatisk filter som begränsar hello olika resultat som hämtas från hello källtabellerna.</span><span class="sxs-lookup"><span data-stu-id="a61fd-160">There is an automatic filter that limits hello range of results that are retrieved from hello source tables.</span></span> 

    <span data-ttu-id="a61fd-161">Du kan dock ändra hello tidsintervall filtret med hjälp av hello nedrullningsbara menyn.</span><span class="sxs-lookup"><span data-stu-id="a61fd-161">However, you can change hello time range filter by using hello drop-down menu.</span></span>

    <span data-ttu-id="a61fd-162">Eller du kan åsidosätta automatisk hello-intervall genom att inkludera egna [ `where  ... timestamp ...` satsen](https://docs.loganalytics.io/queryLanguage/query_language_whereoperator.html) i frågan.</span><span class="sxs-lookup"><span data-stu-id="a61fd-162">Or you can override hello automatic range by including your own [`where  ... timestamp ...` clause](https://docs.loganalytics.io/queryLanguage/query_language_whereoperator.html) into your query.</span></span> <span data-ttu-id="a61fd-163">Exempel:</span><span class="sxs-lookup"><span data-stu-id="a61fd-163">For example:</span></span>

    `requests | where timestamp > ago('2d')`

* <span data-ttu-id="a61fd-164">**Resultaten gränsen**.</span><span class="sxs-lookup"><span data-stu-id="a61fd-164">**Results limit**.</span></span> <span data-ttu-id="a61fd-165">Det finns en gräns på cirka 10 k rader på hello resultaten som returnerades från hello-portalen.</span><span class="sxs-lookup"><span data-stu-id="a61fd-165">There's a limit of about 10k rows on hello results returned from hello portal.</span></span> <span data-ttu-id="a61fd-166">En varning visas om du går över hello-gränsen.</span><span class="sxs-lookup"><span data-stu-id="a61fd-166">A warning shows if you go over hello limit.</span></span> <span data-ttu-id="a61fd-167">Om detta händer visar sortering av resultaten i hello tabellen inte alltid alla hello faktiska första eller sista resultat.</span><span class="sxs-lookup"><span data-stu-id="a61fd-167">If that happens, sorting your results in hello table won't always show you all hello actual first or last results.</span></span> 

    <span data-ttu-id="a61fd-168">Det är god sed tooavoid hitting hello gränsen.</span><span class="sxs-lookup"><span data-stu-id="a61fd-168">It's good practice tooavoid hitting hello limit.</span></span> <span data-ttu-id="a61fd-169">Använda hello tidsfiltret intervallet, eller använda operatorer som:</span><span class="sxs-lookup"><span data-stu-id="a61fd-169">Use hello time range filter, or use operators such as:</span></span>

  * [<span data-ttu-id="a61fd-170">de 100 främsta av tidsstämpel</span><span class="sxs-lookup"><span data-stu-id="a61fd-170">top 100 by timestamp</span></span>](https://docs.loganalytics.io/queryLanguage/query_language_topoperator.html) 
  * [<span data-ttu-id="a61fd-171">ta 100</span><span class="sxs-lookup"><span data-stu-id="a61fd-171">take 100</span></span>](https://docs.loganalytics.io/queryLanguage/query_language_takeoperator.html)
  * [<span data-ttu-id="a61fd-172">Sammanfatta</span><span class="sxs-lookup"><span data-stu-id="a61fd-172">summarize </span></span>](https://docs.loganalytics.io/queryLanguage/query_language_summarizeoperator.html) 
  * [<span data-ttu-id="a61fd-173">där tidsstämpel > ago(3d)</span><span class="sxs-lookup"><span data-stu-id="a61fd-173">where timestamp > ago(3d)</span></span>](https://docs.loganalytics.io/queryLanguage/query_language_whereoperator.html)

<span data-ttu-id="a61fd-174">(Vill ha mer än 10 k raderna?</span><span class="sxs-lookup"><span data-stu-id="a61fd-174">(Want more than 10k rows?</span></span> <span data-ttu-id="a61fd-175">Överväg att använda [löpande Export](app-insights-export-telemetry.md) i stället.</span><span class="sxs-lookup"><span data-stu-id="a61fd-175">Consider using [Continuous Export](app-insights-export-telemetry.md) instead.</span></span> <span data-ttu-id="a61fd-176">Analytics är utformad för analys, i stället för hämtning rådata.)</span><span class="sxs-lookup"><span data-stu-id="a61fd-176">Analytics is designed for analysis, rather than retrieving raw data.)</span></span>

## <a name="diagrams"></a><span data-ttu-id="a61fd-177">Diagram</span><span class="sxs-lookup"><span data-stu-id="a61fd-177">Diagrams</span></span>
<span data-ttu-id="a61fd-178">Välj hello typ av diagram som du vill:</span><span class="sxs-lookup"><span data-stu-id="a61fd-178">Select hello type of diagram you'd like:</span></span>

![Välj en diagramtyp av](./media/app-insights-analytics-using/230.png)

<span data-ttu-id="a61fd-180">Om du har flera kolumner med rätt hello-typer kan välja du hello x och y-axlarna och en kolumn med dimensioner toosplit hello resultaten efter.</span><span class="sxs-lookup"><span data-stu-id="a61fd-180">If you have several columns of hello right types, you can choose hello x and y axes, and a column of dimensions toosplit hello results by.</span></span>

<span data-ttu-id="a61fd-181">Som standard visas först som en tabell och du väljer hello diagram manuellt.</span><span class="sxs-lookup"><span data-stu-id="a61fd-181">By default, results are initially displayed as a table, and you select hello diagram manually.</span></span> <span data-ttu-id="a61fd-182">Men du kan använda hello [återge direktiv](https://docs.loganalytics.io/queryLanguage/query_language_renderoperator.html) hello slutet av en fråga tooselect ett diagram.</span><span class="sxs-lookup"><span data-stu-id="a61fd-182">But you can use hello [render directive](https://docs.loganalytics.io/queryLanguage/query_language_renderoperator.html) at hello end of a query tooselect a diagram.</span></span>

### <a name="analytics-diagnostics"></a><span data-ttu-id="a61fd-183">Analytics-diagnostik</span><span class="sxs-lookup"><span data-stu-id="a61fd-183">Analytics diagnostics</span></span>


<span data-ttu-id="a61fd-184">Om det finns en plötslig insamling eller steg i dina data på en timechart kan du se en markerad punkt hello rad.</span><span class="sxs-lookup"><span data-stu-id="a61fd-184">On a timechart, if there is a sudden spike or step in your data, you may see a highlighted point on hello line.</span></span> <span data-ttu-id="a61fd-185">Detta anger att Analytics-diagnostik har identifierats av en kombination av egenskaper som filtrerar ut hello plötslig förändring.</span><span class="sxs-lookup"><span data-stu-id="a61fd-185">This indicates that Analytics Diagnostics has identified a combination of properties that filter out hello sudden change.</span></span> <span data-ttu-id="a61fd-186">Klicka på hello punkt tooget mer information om hello filter och toosee hello filtrerad version.</span><span class="sxs-lookup"><span data-stu-id="a61fd-186">Click hello point tooget more detail on hello filter, and toosee hello filtered version.</span></span> <span data-ttu-id="a61fd-187">Detta kan hjälpa dig att identifiera vilken orsakade hello ändring.</span><span class="sxs-lookup"><span data-stu-id="a61fd-187">This may help you identify what caused hello change.</span></span> 

[<span data-ttu-id="a61fd-188">Mer information om Analytics diagnostik</span><span class="sxs-lookup"><span data-stu-id="a61fd-188">Learn more about Analytics Diagnostics</span></span>](app-insights-analytics-diagnostics.md)


![Analytics-diagnostik](./media/app-insights-analytics-using/analytics-diagnostics.png)

## <a name="pin-toodashboard"></a><span data-ttu-id="a61fd-190">PIN-kod toodashboard</span><span class="sxs-lookup"><span data-stu-id="a61fd-190">Pin toodashboard</span></span>
<span data-ttu-id="a61fd-191">Du kan fästa en diagrammet eller tooone av din [delade instrumentpaneler](app-insights-dashboards.md) -Klicka bara på hello PIN-kod.</span><span class="sxs-lookup"><span data-stu-id="a61fd-191">You can pin a diagram or table tooone of your [shared dashboards](app-insights-dashboards.md) - just click hello pin.</span></span> <span data-ttu-id="a61fd-192">(Du kan behöva för[uppgradera din app prissättning](app-insights-pricing.md) tooturn på den här funktionen.)</span><span class="sxs-lookup"><span data-stu-id="a61fd-192">(You might need too[upgrade your app's pricing package](app-insights-pricing.md) tooturn on this feature.)</span></span> 

![Klicka på hello PIN-kod](./media/app-insights-analytics-using/pin-01.png)

<span data-ttu-id="a61fd-194">Det innebär att när du sätter ihop en instrumentpanel toohelp du övervaka hello prestanda och användning av web services, kan du inkludera ganska komplex analys tillsammans med hello andra mått.</span><span class="sxs-lookup"><span data-stu-id="a61fd-194">This means that, when you put together a dashboard toohelp you monitor hello performance or usage of your web services, you can include quite complex analysis alongside hello other metrics.</span></span> 

<span data-ttu-id="a61fd-195">Du kan fästa en tabell toohello instrumentpanel, om den har fyra eller färre kolumner.</span><span class="sxs-lookup"><span data-stu-id="a61fd-195">You can pin a table toohello dashboard, if it has four or fewer columns.</span></span> <span data-ttu-id="a61fd-196">Endast hello översta sju rader visas.</span><span class="sxs-lookup"><span data-stu-id="a61fd-196">Only hello top seven rows are displayed.</span></span>

### <a name="dashboard-refresh"></a><span data-ttu-id="a61fd-197">Uppdatera instrumentpanelen</span><span class="sxs-lookup"><span data-stu-id="a61fd-197">Dashboard refresh</span></span>
<span data-ttu-id="a61fd-198">hello diagram Fäst toohello instrumentpanelen uppdateras automatiskt genom att köra frågan hello cirka varje timme.</span><span class="sxs-lookup"><span data-stu-id="a61fd-198">hello chart pinned toohello dashboard is refreshed automatically by re-running hello query approximately every hours.</span></span> <span data-ttu-id="a61fd-199">Du kan också klicka på uppdateringsknappen hello.</span><span class="sxs-lookup"><span data-stu-id="a61fd-199">You can also click hello Refresh button.</span></span>

### <a name="automatic-simplifications"></a><span data-ttu-id="a61fd-200">Automatisk förenklingar</span><span class="sxs-lookup"><span data-stu-id="a61fd-200">Automatic simplifications</span></span>

<span data-ttu-id="a61fd-201">Vissa förenklingar är tillämpade tooa diagrammet när du fäster tooa instrumentpanelen.</span><span class="sxs-lookup"><span data-stu-id="a61fd-201">Certain simplifications are applied tooa chart when you pin it tooa dashboard.</span></span>

<span data-ttu-id="a61fd-202">**Tid begränsning:** frågorna är automatiskt begränsad toohello senaste 14 dagarna.</span><span class="sxs-lookup"><span data-stu-id="a61fd-202">**Time restriction:** Queries are automatically limited toohello past 14 days.</span></span> <span data-ttu-id="a61fd-203">hello effekt är hello samma som om frågan innehåller `where timestamp > ago(14d)`.</span><span class="sxs-lookup"><span data-stu-id="a61fd-203">hello effect is hello same as if your query includes `where timestamp > ago(14d)`.</span></span>

<span data-ttu-id="a61fd-204">**Bin antal begränsning:** om du visar ett diagram med mycket diskreta lagerplatser (vanligtvis ett stapeldiagram) hello mindre ifyllda lagerplatser grupperas automatiskt i en enda ”andra” bin.</span><span class="sxs-lookup"><span data-stu-id="a61fd-204">**Bin count restriction:** If you display a chart that has a lot of discrete bins (typically a bar chart), hello less populated bins are automatically grouped into a single "others" bin.</span></span> <span data-ttu-id="a61fd-205">Till exempel den här frågan:</span><span class="sxs-lookup"><span data-stu-id="a61fd-205">For example, this query:</span></span>

    requests | summarize count_search = count() by client_CountryOrRegion

<span data-ttu-id="a61fd-206">ser ut så här i Analytics:</span><span class="sxs-lookup"><span data-stu-id="a61fd-206">looks like this in Analytics:</span></span>

![Diagram med lång pilslut](./media/app-insights-analytics-using/pin-07.png)

<span data-ttu-id="a61fd-208">men när du fäster den tooa instrumentpanelen, det ser ut så här:</span><span class="sxs-lookup"><span data-stu-id="a61fd-208">but when you pin it tooa dashboard, it looks like this:</span></span>

![Diagram med begränsad lagerplatser](./media/app-insights-analytics-using/pin-08.png)

## <a name="export-tooexcel"></a><span data-ttu-id="a61fd-210">Exportera tooExcel</span><span class="sxs-lookup"><span data-stu-id="a61fd-210">Export tooExcel</span></span>
<span data-ttu-id="a61fd-211">När du har kört en fråga, kan du hämta en CSV-fil.</span><span class="sxs-lookup"><span data-stu-id="a61fd-211">After you've run a query, you can download a .csv file.</span></span> <span data-ttu-id="a61fd-212">Klicka på **Exportera Excel**.</span><span class="sxs-lookup"><span data-stu-id="a61fd-212">Click **Export,  Excel**.</span></span>

## <a name="export-toopower-bi"></a><span data-ttu-id="a61fd-213">Exportera tooPower BI</span><span class="sxs-lookup"><span data-stu-id="a61fd-213">Export tooPower BI</span></span>
<span data-ttu-id="a61fd-214">Placera markören hello i en fråga och välj **exportera Power BI**.</span><span class="sxs-lookup"><span data-stu-id="a61fd-214">Put hello cursor in a query and choose **Export, Power BI**.</span></span>

![Exportera från Analytics tooPower BI](./media/app-insights-analytics-using/240.png)

<span data-ttu-id="a61fd-216">Du kan köra hello frågan i Power BI.</span><span class="sxs-lookup"><span data-stu-id="a61fd-216">You run hello query in Power BI.</span></span> <span data-ttu-id="a61fd-217">Du kan ange den toorefresh enligt ett schema.</span><span class="sxs-lookup"><span data-stu-id="a61fd-217">You can set it toorefresh on a schedule.</span></span>

<span data-ttu-id="a61fd-218">Du kan skapa instrumentpaneler som samordnar data från olika källor med Power BI.</span><span class="sxs-lookup"><span data-stu-id="a61fd-218">With Power BI, you can create dashboards that bring together data from a wide variety of sources.</span></span>

[<span data-ttu-id="a61fd-219">Mer information om export tooPower BI</span><span class="sxs-lookup"><span data-stu-id="a61fd-219">Learn more about export tooPower BI</span></span>](app-insights-export-power-bi.md)

## <a name="deep-link"></a><span data-ttu-id="a61fd-220">Djuplänk</span><span class="sxs-lookup"><span data-stu-id="a61fd-220">Deep link</span></span>

<span data-ttu-id="a61fd-221">Hämta en länk under **Export resursen länken** som du kan skicka tooanother användaren.</span><span class="sxs-lookup"><span data-stu-id="a61fd-221">Get a link under **Export, Share link** that you can send tooanother user.</span></span> <span data-ttu-id="a61fd-222">Från hello användaren har [åtkomst tooyour resursgruppen](app-insights-resources-roles-access-control.md), hello frågan öppnas i hello Analytics Användargränssnittet.</span><span class="sxs-lookup"><span data-stu-id="a61fd-222">Provided hello user has [access tooyour resource group](app-insights-resources-roles-access-control.md), hello query will open in hello Analytics UI.</span></span>

<span data-ttu-id="a61fd-223">(I hello länk hello Frågetexten visas efter ”? q =”, gzip komprimeras och Base64-kodad.</span><span class="sxs-lookup"><span data-stu-id="a61fd-223">(In hello link, hello query text appears after "?q=", gzip compressed and base-64 encoded.</span></span> <span data-ttu-id="a61fd-224">Du kan skriva kod toogenerate djuplänkar du ange toousers.</span><span class="sxs-lookup"><span data-stu-id="a61fd-224">You could write code toogenerate deep links that you provide toousers.</span></span> <span data-ttu-id="a61fd-225">Hello bör dock sätt toorun Analytics från kod är med hjälp av hello [REST API](https://dev.applicationinsights.io/).)</span><span class="sxs-lookup"><span data-stu-id="a61fd-225">However, hello recommended way toorun Analytics from code is by using hello [REST API](https://dev.applicationinsights.io/).)</span></span>


## <a name="automation"></a><span data-ttu-id="a61fd-226">Automation</span><span class="sxs-lookup"><span data-stu-id="a61fd-226">Automation</span></span>

<span data-ttu-id="a61fd-227">Använd hello [Data Access REST API: et](https://dev.applicationinsights.io/) toorun Analytics-frågor.</span><span class="sxs-lookup"><span data-stu-id="a61fd-227">Use hello  [Data Access REST API](https://dev.applicationinsights.io/) toorun Analytics queries.</span></span> <span data-ttu-id="a61fd-228">[Till exempel](https://dev.applicationinsights.io/apiexplorer/query?appId=DEMO_APP&apiKey=DEMO_KEY&query=requests%0A%7C%20where%20timestamp%20%3E%3D%20ago%2824h%29%0A%7C%20count) (med PowerShell):</span><span class="sxs-lookup"><span data-stu-id="a61fd-228">[For example](https://dev.applicationinsights.io/apiexplorer/query?appId=DEMO_APP&apiKey=DEMO_KEY&query=requests%0A%7C%20where%20timestamp%20%3E%3D%20ago%2824h%29%0A%7C%20count) (using PowerShell):</span></span>

```PS
curl "https://api.applicationinsights.io/beta/apps/DEMO_APP/query?query=requests%7C%20where%20timestamp%20%3E%3D%20ago(24h)%7C%20count" -H "x-api-key: DEMO_KEY"
```

<span data-ttu-id="a61fd-229">Till skillnad från hello Analytics UI, hello REST-API lägger inte automatiskt till tidsstämpel begränsning tooyour frågor.</span><span class="sxs-lookup"><span data-stu-id="a61fd-229">Unlike hello Analytics UI, hello REST API does not automatically add any timestamp limitation tooyour queries.</span></span> <span data-ttu-id="a61fd-230">Kom ihåg tooadd egna where-satsen, tooavoid få stora svar.</span><span class="sxs-lookup"><span data-stu-id="a61fd-230">Remember tooadd your own where-clause, tooavoid getting huge responses.</span></span>



## <a name="import-data"></a><span data-ttu-id="a61fd-231">Importera data</span><span class="sxs-lookup"><span data-stu-id="a61fd-231">Import data</span></span>

<span data-ttu-id="a61fd-232">Du kan importera data från en CSV-fil.</span><span class="sxs-lookup"><span data-stu-id="a61fd-232">You can import data from a CSV file.</span></span> <span data-ttu-id="a61fd-233">En typisk användning är tooimport statiska data som du kan ansluta med tabeller från din telemetri.</span><span class="sxs-lookup"><span data-stu-id="a61fd-233">A typical usage is tooimport static data that you can join with tables from your telemetry.</span></span> 

<span data-ttu-id="a61fd-234">Om autentiserade användare identifieras i din telemetri av ett alias eller dolda id, kan du importera en tabell som mappar alias tooreal namn.</span><span class="sxs-lookup"><span data-stu-id="a61fd-234">For example, if authenticated users are identified in your telemetry by an alias or obfuscated id, you could import a table that maps aliases tooreal names.</span></span> <span data-ttu-id="a61fd-235">Genom att utföra en koppling på hello begärandetelemetri kan identifiera du användare efter verkliga namn i hello Analytics-rapporter.</span><span class="sxs-lookup"><span data-stu-id="a61fd-235">By performing a join on hello request telemetry, you can identify users by their real names in hello Analytics reports.</span></span>

### <a name="define-your-data-schema"></a><span data-ttu-id="a61fd-236">Definiera dataschemat</span><span class="sxs-lookup"><span data-stu-id="a61fd-236">Define your data schema</span></span>

1. <span data-ttu-id="a61fd-237">Klicka på **inställningar** (längst upp till vänster) och sedan **datakällor**.</span><span class="sxs-lookup"><span data-stu-id="a61fd-237">Click **Settings** (at top left) and then **Data Sources**.</span></span> 
2. <span data-ttu-id="a61fd-238">Lägg till en datakälla hello instruktionerna.</span><span class="sxs-lookup"><span data-stu-id="a61fd-238">Add a data source, following hello instructions.</span></span> <span data-ttu-id="a61fd-239">Du tillfrågas toosupply ett exempel på hello data som ska innehålla minst tio rader.</span><span class="sxs-lookup"><span data-stu-id="a61fd-239">You are asked toosupply a sample of hello data, which should include at least ten rows.</span></span> <span data-ttu-id="a61fd-240">Du kan sedan Korrigera hello schemat.</span><span class="sxs-lookup"><span data-stu-id="a61fd-240">You then correct hello schema.</span></span>

<span data-ttu-id="a61fd-241">Detta definierar en datakälla som du kan sedan använda tooimport enskilda tabeller.</span><span class="sxs-lookup"><span data-stu-id="a61fd-241">This defines a data source, which you can then use tooimport individual tables.</span></span>

### <a name="import-a-table"></a><span data-ttu-id="a61fd-242">Importera en tabell</span><span class="sxs-lookup"><span data-stu-id="a61fd-242">Import a table</span></span>

1. <span data-ttu-id="a61fd-243">Öppna din definitionen av datakällan hello-listan.</span><span class="sxs-lookup"><span data-stu-id="a61fd-243">Open your data source definition from hello list.</span></span>
2. <span data-ttu-id="a61fd-244">Klicka på ”ladda upp” och följ hello instruktioner tooupload hello tabell.</span><span class="sxs-lookup"><span data-stu-id="a61fd-244">Click "Upload" and follow hello instructions tooupload hello table.</span></span> <span data-ttu-id="a61fd-245">Detta innebär en anropet tooa REST-API och det är därför enkelt tooautomate.</span><span class="sxs-lookup"><span data-stu-id="a61fd-245">This involves a call tooa REST API, and so it is easy tooautomate.</span></span> 

<span data-ttu-id="a61fd-246">Din tabell är nu tillgänglig för användning i Analytics-frågor.</span><span class="sxs-lookup"><span data-stu-id="a61fd-246">Your table is now available for use in Analytics queries.</span></span> <span data-ttu-id="a61fd-247">Den visas i Analytics</span><span class="sxs-lookup"><span data-stu-id="a61fd-247">It will appear in Analytics</span></span> 

### <a name="use-hello-table"></a><span data-ttu-id="a61fd-248">Använd hello tabell</span><span class="sxs-lookup"><span data-stu-id="a61fd-248">Use hello table</span></span>

<span data-ttu-id="a61fd-249">Anta att dina definitionen av datakällan anropas `usermap`, och att det finns två fält `realName` och `user_AuthenticatedId`.</span><span class="sxs-lookup"><span data-stu-id="a61fd-249">Let's suppose your data source definition is called `usermap`, and that it has two fields, `realName` and `user_AuthenticatedId`.</span></span> <span data-ttu-id="a61fd-250">Hej `requests` tabellen har också ett fält med namnet `user_AuthenticatedId`, så det är enkelt toojoin dem:</span><span class="sxs-lookup"><span data-stu-id="a61fd-250">hello `requests` table also has a field named `user_AuthenticatedId`, so it's easy toojoin them:</span></span>

```AIQL

    requests
    | where notempty(user_AuthenticatedId) | take 10
    | join kind=leftouter ( usermap ) on user_AuthenticatedId 
```
<span data-ttu-id="a61fd-251">hello resulterande tabellen för förfrågningar har en ytterligare kolumn `realName`.</span><span class="sxs-lookup"><span data-stu-id="a61fd-251">hello resulting table of requests has an additional column, `realName`.</span></span>

### <a name="import-from-logstash"></a><span data-ttu-id="a61fd-252">Importera från LogStash</span><span class="sxs-lookup"><span data-stu-id="a61fd-252">Import from LogStash</span></span>

<span data-ttu-id="a61fd-253">Om du använder [LogStash](https://www.elastic.co/guide/en/logstash/current/getting-started-with-logstash.html), du kan använda Analytics tooquery loggarna.</span><span class="sxs-lookup"><span data-stu-id="a61fd-253">If you use [LogStash](https://www.elastic.co/guide/en/logstash/current/getting-started-with-logstash.html), you can use Analytics tooquery your logs.</span></span> <span data-ttu-id="a61fd-254">Använd hello [plugin-program som kommer data i Analytics](https://github.com/Microsoft/logstash-output-application-insights).</span><span class="sxs-lookup"><span data-stu-id="a61fd-254">Use hello [plugin that pipes data into Analytics](https://github.com/Microsoft/logstash-output-application-insights).</span></span> 

## <a name="video"></a><span data-ttu-id="a61fd-255">Video</span><span class="sxs-lookup"><span data-stu-id="a61fd-255">Video</span></span>

> [!VIDEO https://channel9.msdn.com/events/Connect/2016/123/player] 

[!INCLUDE [app-insights-analytics-footer](../../includes/app-insights-analytics-footer.md)]

