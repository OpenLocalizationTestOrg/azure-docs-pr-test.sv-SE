---
title: "aaaAnalytics - hello kraftfullt sökverktyg av Azure Application Insights | Microsoft Docs"
description: "Översikt över Analytics, hello kraftfullt diagnostiska sökverktyg av Application Insights. "
services: application-insights
documentationcenter: 
author: CFreemanwa
manager: carmonm
ms.assetid: 0a2f6011-5bcf-47b7-8450-40f284274b24
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 03/14/2017
ms.author: bwren
ms.openlocfilehash: d2b41e2fff7cc786e11fa3dfe94fc46f1b86e9eb
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="analytics-in-application-insights"></a><span data-ttu-id="dcbde-103">Analyser i Application Insights</span><span class="sxs-lookup"><span data-stu-id="dcbde-103">Analytics in Application Insights</span></span>
<span data-ttu-id="dcbde-104">[Analytics](app-insights-analytics.md) är hello kraftfulla sökfunktionen i [Programinsikter](app-insights-overview.md).</span><span class="sxs-lookup"><span data-stu-id="dcbde-104">[Analytics](app-insights-analytics.md) is hello powerful search feature of [Application Insights](app-insights-overview.md).</span></span> <span data-ttu-id="dcbde-105">Dessa sidor beskrivs Log Analytics-frågespråket.</span><span class="sxs-lookup"><span data-stu-id="dcbde-105">These pages describe the Log Analytics query language.</span></span> 

* <span data-ttu-id="dcbde-106">**[Se hello inledande video](https://applicationanalytics-media.azureedge.net/home_page_video.mp4)**.</span><span class="sxs-lookup"><span data-stu-id="dcbde-106">**[Watch hello introductory video](https://applicationanalytics-media.azureedge.net/home_page_video.mp4)**.</span></span>
* <span data-ttu-id="dcbde-107">**[Testkör Analytics på våra simulerade data](https://analytics.applicationinsights.io/demo)**  om din app inte skickar data tooApplication insikter ännu.</span><span class="sxs-lookup"><span data-stu-id="dcbde-107">**[Test drive Analytics on our simulated data](https://analytics.applicationinsights.io/demo)** if your app isn't sending data tooApplication Insights yet.</span></span>
* <span data-ttu-id="dcbde-108">**[SQL-användarnas cheat blad](https://aka.ms/sql-analytics)**  översätter hello vanligaste idioms.</span><span class="sxs-lookup"><span data-stu-id="dcbde-108">**[SQL-users' cheat sheet](https://aka.ms/sql-analytics)** translates hello most common idioms.</span></span>
* <span data-ttu-id="dcbde-109">**[Språkreferens för](app-insights-analytics-reference.md)**  Lär dig hur toouse alla hello kraftfulla funktioner för hello Log Analytics-frågespråket.</span><span class="sxs-lookup"><span data-stu-id="dcbde-109">**[Language Reference](app-insights-analytics-reference.md)** Learn how toouse all hello powerful features of hello Log Analytics query language.</span></span>


## <a name="queries-in-analytics"></a><span data-ttu-id="dcbde-110">Frågorna i Analytics</span><span class="sxs-lookup"><span data-stu-id="dcbde-110">Queries in Analytics</span></span>
<span data-ttu-id="dcbde-111">En typisk frågan är en *källa* tabell följt av en serie *operatörer* avgränsade med `|`.</span><span class="sxs-lookup"><span data-stu-id="dcbde-111">A typical query is a *source* table followed by a series of *operators* separated by `|`.</span></span> 

<span data-ttu-id="dcbde-112">Till exempel ska vi ta reda på vilken tid på dagen hello unionsmedborgare Hyderabad försök webbappen.</span><span class="sxs-lookup"><span data-stu-id="dcbde-112">For example, let's find out what time of day hello citizens of Hyderabad try our web app.</span></span> <span data-ttu-id="dcbde-113">Och när vi kan vi se vilka resultatkoder returneras tootheir HTTP-begäranden.</span><span class="sxs-lookup"><span data-stu-id="dcbde-113">And while we're there, let's see what result codes are returned tootheir HTTP requests.</span></span> 

```AIQL
requests
| where timestamp > ago(30d)
| summarize ClientCount = dcount(client_IP) by bin(timestamp, 1h), resultCode
| extend LocalTime = timestamp - 4h
| order by LocalTime desc
| render barchart
```

<span data-ttu-id="dcbde-114">Vi räkna distinkta klienternas IP-adresser, gruppera dem med hello timme på dagen hello över hello senaste 7 dagarna.</span><span class="sxs-lookup"><span data-stu-id="dcbde-114">We count distinct client IP addresses, grouping them by hello hour of hello day over hello past 7 days.</span></span> 

> [!NOTE]
> <span data-ttu-id="dcbde-115">tooget resultat utanför hello föregående 24 timmar, ta inkludera ”tidsstämpel' explicit i frågan eller Använd hello tid intervallet nedrullningsbara menyn.</span><span class="sxs-lookup"><span data-stu-id="dcbde-115">tooget results outside hello previous 24h, either include 'timestamp' explicitly in your query, or use hello time range drop-down menu.</span></span>
>

<span data-ttu-id="dcbde-116">Vi visar hello resultat med hello liggande diagram presentation, välja toostack hello resultat från olika svarskoder:</span><span class="sxs-lookup"><span data-stu-id="dcbde-116">Let's display hello results with hello bar chart presentation, choosing toostack hello results from different response codes:</span></span>

![Välj liggande diagram, x och y-axlarna sedan segmentering](./media/app-insights-analytics/020.png)

<span data-ttu-id="dcbde-118">Verkar vår app är mest populär på lunch och sängen-tid i Hyderabad.</span><span class="sxs-lookup"><span data-stu-id="dcbde-118">Looks like our app is most popular at lunchtime and bed-time in Hyderabad.</span></span> <span data-ttu-id="dcbde-119">(Och vi ska undersöka de 500 koderna.)</span><span class="sxs-lookup"><span data-stu-id="dcbde-119">(And we should investigate those 500 codes.)</span></span>

<span data-ttu-id="dcbde-120">Det finns också kraftfulla statistiska operationer:</span><span class="sxs-lookup"><span data-stu-id="dcbde-120">There are also powerful statistical operations:</span></span>

![Resultatet av statistiska fråga](./media/app-insights-analytics/025.png)

<span data-ttu-id="dcbde-122">hello språk har många bra funktioner:</span><span class="sxs-lookup"><span data-stu-id="dcbde-122">hello language has many attractive features:</span></span>


* <span data-ttu-id="dcbde-123">[Filter](https://docs.loganalytics.io/queryLanguage/query_language_whereoperator.html) dina rådata app telemetri av alla fält, inklusive din anpassade egenskaper och mått.</span><span class="sxs-lookup"><span data-stu-id="dcbde-123">[Filter](https://docs.loganalytics.io/queryLanguage/query_language_whereoperator.html) your raw app telemetry by any fields, including your custom properties and metrics.</span></span>
* <span data-ttu-id="dcbde-124">[Anslut](https://docs.loganalytics.io/queryLanguage/query_language_joinoperator.html) flera tabeller – korrelera begäranden med sidvisningar, beroendeanrop, undantag och loggspårningar.</span><span class="sxs-lookup"><span data-stu-id="dcbde-124">[Join](https://docs.loganalytics.io/queryLanguage/query_language_joinoperator.html) multiple tables – correlate requests with page views, dependency calls, exceptions and log traces.</span></span>
* <span data-ttu-id="dcbde-125">Kraftfulla statistiska [aggregeringar](https://docs.loganalytics.io/learn/tutorials/aggregations.html).</span><span class="sxs-lookup"><span data-stu-id="dcbde-125">Powerful statistical [aggregations](https://docs.loganalytics.io/learn/tutorials/aggregations.html).</span></span>
* <span data-ttu-id="dcbde-126">Precis lika kraftfulla som SQL, men mycket enklare för komplexa frågor: i stället för kapslade uttryck du skicka hello data från en grundläggande åtgärden toohello nästa.</span><span class="sxs-lookup"><span data-stu-id="dcbde-126">Just as powerful as SQL, but much easier for complex queries: instead of nesting statements, you pipe hello data from one elementary operation toohello next.</span></span>
* <span data-ttu-id="dcbde-127">Omedelbar och kraftfulla visualiseringar.</span><span class="sxs-lookup"><span data-stu-id="dcbde-127">Immediate and powerful visualizations.</span></span>
* <span data-ttu-id="dcbde-128">[PIN-kod diagram tooAzure instrumentpaneler](app-insights-analytics-using.md#pin-to-dashboard).</span><span class="sxs-lookup"><span data-stu-id="dcbde-128">[Pin charts tooAzure dashboards](app-insights-analytics-using.md#pin-to-dashboard).</span></span>
* <span data-ttu-id="dcbde-129">[Exportera frågor tooPower BI](app-insights-analytics-using.md#export-to-power-bi).</span><span class="sxs-lookup"><span data-stu-id="dcbde-129">[Export queries tooPower BI](app-insights-analytics-using.md#export-to-power-bi).</span></span>
* <span data-ttu-id="dcbde-130">Det finns en [REST API](https://dev.applicationinsights.io/) som du kan använda toorun frågor programmässigt, till exempel från Powershell.</span><span class="sxs-lookup"><span data-stu-id="dcbde-130">There's a [REST API](https://dev.applicationinsights.io/) that you can use toorun queries programmatically, for example from Powershell.</span></span>


## <a name="connect-tooyour-application-insights-data"></a><span data-ttu-id="dcbde-131">Ansluta tooyour Application Insights-data</span><span class="sxs-lookup"><span data-stu-id="dcbde-131">Connect tooyour Application Insights data</span></span>
<span data-ttu-id="dcbde-132">Öppna Analytics från din app [översikt bladet](app-insights-dashboards.md) i Application Insights:</span><span class="sxs-lookup"><span data-stu-id="dcbde-132">Open Analytics from your app's [overview blade](app-insights-dashboards.md) in Application Insights:</span></span> 

![Öppna portal.azure.com, öppna Application Insights-resursen och klicka på Analytics.](./media/app-insights-analytics/001.png)


## <a name="video"></a><span data-ttu-id="dcbde-134">Video</span><span class="sxs-lookup"><span data-stu-id="dcbde-134">Video</span></span>

> [!VIDEO https://channel9.msdn.com/events/Connect/2016/123/player] 


[!INCLUDE [app-insights-analytics-footer](../../includes/app-insights-analytics-footer.md)]



## <a name="query-examples"></a><span data-ttu-id="dcbde-135">frågan exempel</span><span class="sxs-lookup"><span data-stu-id="dcbde-135">Query examples</span></span>

<span data-ttu-id="dcbde-136">Prova dessa genomgång tooillustrate hello kraften i med hjälp av Analytics:</span><span class="sxs-lookup"><span data-stu-id="dcbde-136">Try these walkthroughs tooillustrate hello power of using Analytics:</span></span>

 *  [<span data-ttu-id="dcbde-137">Automatisk diagnostik av toppar och steg leder i begäranden varaktighet</span><span class="sxs-lookup"><span data-stu-id="dcbde-137">Automatic diagnostics of spikes and step jumps in requests durations</span></span>](https://analytics.applicationinsights.io/demo#/discover/query/results/chart?title=Automatic%20diagnostics%20of%20sudden%20spikes%20or%20step%20jumps%20in%20requests%20duration&shared=true)
 *  [<span data-ttu-id="dcbde-138">Analysera prestandaförsämringar med analys av tidsserier</span><span class="sxs-lookup"><span data-stu-id="dcbde-138">Analyzing performance degradations with time series analysis</span></span>](https://analytics.applicationinsights.io/demo#/discover/query/main?title=Analyzing%20performance%20degradations%20with%20time%20series%20analysis&shared=true)
 *  [<span data-ttu-id="dcbde-139">Analysera programfel med autocluster och diffpatterns</span><span class="sxs-lookup"><span data-stu-id="dcbde-139">Analyzing application failures with autocluster and diffpatterns</span></span>](https://analytics.applicationinsights.io/demo#/discover/query/main?title=Analyzing%20application%20failures%20with%20autocluster%20and%20diffpatterns&shared=true)
 *  [<span data-ttu-id="dcbde-140">Avancerade form identifieringar med analys av tidsserier</span><span class="sxs-lookup"><span data-stu-id="dcbde-140">Advanced shape detections with time series analysis</span></span>](https://analytics.applicationinsights.io/demo#/discover/query/main?title=Advanced%20shape%20detection%20with%20time%20series%20analysis&shared=true)
 *  [<span data-ttu-id="dcbde-141">Med hjälp av glidande fönstret operations tooanalyze programanvändning (rullande MAU/DAU osv)</span><span class="sxs-lookup"><span data-stu-id="dcbde-141">Using sliding window operations tooanalyze application usage (rolling MAU/DAU etc)</span></span>](https://analytics.applicationinsights.io/demo#/discover/query/main?title=Using%20sliding%20window%20calculations%20to%20analyze%20usage%20metrics:%20rolling%20MAU~2FDAU%20and%20cohorts&shared=true)
 *  <span data-ttu-id="dcbde-142">[Identifiering av avbrott i tjänsten baserat på analys av felsökningsloggar](https://analytics.applicationinsights.io/demo#/discover/query/main?title=Detection%20of%20service%20disruptions%20based%20on%20regression%20analysis%20of%20trace%20logs&shared=true) och ett matchande blogginlägg [här](https://maximshklar.wordpress.com/2017/02/16/finding-trends-in-traces-with-smart-data-analytics).</span><span class="sxs-lookup"><span data-stu-id="dcbde-142">[Detection of service disruptions based on analysis of debug logs](https://analytics.applicationinsights.io/demo#/discover/query/main?title=Detection%20of%20service%20disruptions%20based%20on%20regression%20analysis%20of%20trace%20logs&shared=true) and a matching blog post [here](https://maximshklar.wordpress.com/2017/02/16/finding-trends-in-traces-with-smart-data-analytics).</span></span>
 *  <span data-ttu-id="dcbde-143">[Profilering programmens prestanda med hjälp av enkla felsökningsloggar](https://analytics.applicationinsights.io/demo#/discover/query/main?title=Profiling%20applications'%20performance%20with%20simple%20debug%20logs&shared=true) och ett matchande blogginlägg [här](https://yossiattasblog.wordpress.com/2017/03/13/first-blog-post/)</span><span class="sxs-lookup"><span data-stu-id="dcbde-143">[Profiling applications’ performance using simple debug logs](https://analytics.applicationinsights.io/demo#/discover/query/main?title=Profiling%20applications'%20performance%20with%20simple%20debug%20logs&shared=true) and a matching blog post [here](https://yossiattasblog.wordpress.com/2017/03/13/first-blog-post/)</span></span>
 *  <span data-ttu-id="dcbde-144">[Mäter hello varaktighet för varje steg i din kod flöde med hjälp av enkla felsökningsloggar](https://analytics.applicationinsights.io/demo#/discover/query/main?title=Measuring%20the%20duration%20of%20each%20step%20in%20your%20code%20flow%20using%20simple%20debug%20logs&shared=true) och ett matchande blogginlägg [här](https://yossiattasblog.wordpress.com/2017/03/14/measuring-the-duration-of-each-step-in-your-code-flow-using-simple-debug-logs/)</span><span class="sxs-lookup"><span data-stu-id="dcbde-144">[Measuring hello duration for each step in your code flow using simple debug logs](https://analytics.applicationinsights.io/demo#/discover/query/main?title=Measuring%20the%20duration%20of%20each%20step%20in%20your%20code%20flow%20using%20simple%20debug%20logs&shared=true) and a matching blog post [here](https://yossiattasblog.wordpress.com/2017/03/14/measuring-the-duration-of-each-step-in-your-code-flow-using-simple-debug-logs/)</span></span>
 *  <span data-ttu-id="dcbde-145">[Analysera samtidighet med enkla felsökningsloggar](https://analytics.applicationinsights.io/demo#/discover/query/results/chart?title=Analyzing%20concurrency%20with%20simple%20debug%20logs&shared=true) och ett matchande blogginlägg [här](https://yossiattasblog.wordpress.com/2017/03/23/analyzing-concurrency-using-simple-debug-logs/)</span><span class="sxs-lookup"><span data-stu-id="dcbde-145">[Analyzing concurrency using simple debug logs](https://analytics.applicationinsights.io/demo#/discover/query/results/chart?title=Analyzing%20concurrency%20with%20simple%20debug%20logs&shared=true) and a matching blog post [here](https://yossiattasblog.wordpress.com/2017/03/23/analyzing-concurrency-using-simple-debug-logs/)</span></span>



## <a name="next-steps"></a><span data-ttu-id="dcbde-146">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="dcbde-146">Next steps</span></span>
* <span data-ttu-id="dcbde-147">Vi rekommenderar att du börjar med hello [språk rundtur](app-insights-analytics-tour.md).</span><span class="sxs-lookup"><span data-stu-id="dcbde-147">We recommend you start with hello [language tour](app-insights-analytics-tour.md).</span></span> 
* <span data-ttu-id="dcbde-148">Mer om [med hjälp av Analytics](app-insights-analytics-using.md).</span><span class="sxs-lookup"><span data-stu-id="dcbde-148">More about [using Analytics](app-insights-analytics-using.md).</span></span> 
* <span data-ttu-id="dcbde-149">[Språkreferens för](app-insights-analytics-reference.md).</span><span class="sxs-lookup"><span data-stu-id="dcbde-149">[Language reference](app-insights-analytics-reference.md).</span></span> 
