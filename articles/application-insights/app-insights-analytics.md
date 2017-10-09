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
# <a name="analytics-in-application-insights"></a>Analyser i Application Insights
[Analytics](app-insights-analytics.md) är hello kraftfulla sökfunktionen i [Programinsikter](app-insights-overview.md). Dessa sidor beskrivs Log Analytics-frågespråket. 

* **[Se hello inledande video](https://applicationanalytics-media.azureedge.net/home_page_video.mp4)**.
* **[Testkör Analytics på våra simulerade data](https://analytics.applicationinsights.io/demo)**  om din app inte skickar data tooApplication insikter ännu.
* **[SQL-användarnas cheat blad](https://aka.ms/sql-analytics)**  översätter hello vanligaste idioms.
* **[Språkreferens för](app-insights-analytics-reference.md)**  Lär dig hur toouse alla hello kraftfulla funktioner för hello Log Analytics-frågespråket.


## <a name="queries-in-analytics"></a>Frågorna i Analytics
En typisk frågan är en *källa* tabell följt av en serie *operatörer* avgränsade med `|`. 

Till exempel ska vi ta reda på vilken tid på dagen hello unionsmedborgare Hyderabad försök webbappen. Och när vi kan vi se vilka resultatkoder returneras tootheir HTTP-begäranden. 

```AIQL
requests
| where timestamp > ago(30d)
| summarize ClientCount = dcount(client_IP) by bin(timestamp, 1h), resultCode
| extend LocalTime = timestamp - 4h
| order by LocalTime desc
| render barchart
```

Vi räkna distinkta klienternas IP-adresser, gruppera dem med hello timme på dagen hello över hello senaste 7 dagarna. 

> [!NOTE]
> tooget resultat utanför hello föregående 24 timmar, ta inkludera ”tidsstämpel' explicit i frågan eller Använd hello tid intervallet nedrullningsbara menyn.
>

Vi visar hello resultat med hello liggande diagram presentation, välja toostack hello resultat från olika svarskoder:

![Välj liggande diagram, x och y-axlarna sedan segmentering](./media/app-insights-analytics/020.png)

Verkar vår app är mest populär på lunch och sängen-tid i Hyderabad. (Och vi ska undersöka de 500 koderna.)

Det finns också kraftfulla statistiska operationer:

![Resultatet av statistiska fråga](./media/app-insights-analytics/025.png)

hello språk har många bra funktioner:


* [Filter](https://docs.loganalytics.io/queryLanguage/query_language_whereoperator.html) dina rådata app telemetri av alla fält, inklusive din anpassade egenskaper och mått.
* [Anslut](https://docs.loganalytics.io/queryLanguage/query_language_joinoperator.html) flera tabeller – korrelera begäranden med sidvisningar, beroendeanrop, undantag och loggspårningar.
* Kraftfulla statistiska [aggregeringar](https://docs.loganalytics.io/learn/tutorials/aggregations.html).
* Precis lika kraftfulla som SQL, men mycket enklare för komplexa frågor: i stället för kapslade uttryck du skicka hello data från en grundläggande åtgärden toohello nästa.
* Omedelbar och kraftfulla visualiseringar.
* [PIN-kod diagram tooAzure instrumentpaneler](app-insights-analytics-using.md#pin-to-dashboard).
* [Exportera frågor tooPower BI](app-insights-analytics-using.md#export-to-power-bi).
* Det finns en [REST API](https://dev.applicationinsights.io/) som du kan använda toorun frågor programmässigt, till exempel från Powershell.


## <a name="connect-tooyour-application-insights-data"></a>Ansluta tooyour Application Insights-data
Öppna Analytics från din app [översikt bladet](app-insights-dashboards.md) i Application Insights: 

![Öppna portal.azure.com, öppna Application Insights-resursen och klicka på Analytics.](./media/app-insights-analytics/001.png)


## <a name="video"></a>Video

> [!VIDEO https://channel9.msdn.com/events/Connect/2016/123/player] 


[!INCLUDE [app-insights-analytics-footer](../../includes/app-insights-analytics-footer.md)]



## <a name="query-examples"></a>frågan exempel

Prova dessa genomgång tooillustrate hello kraften i med hjälp av Analytics:

 *  [Automatisk diagnostik av toppar och steg leder i begäranden varaktighet](https://analytics.applicationinsights.io/demo#/discover/query/results/chart?title=Automatic%20diagnostics%20of%20sudden%20spikes%20or%20step%20jumps%20in%20requests%20duration&shared=true)
 *  [Analysera prestandaförsämringar med analys av tidsserier](https://analytics.applicationinsights.io/demo#/discover/query/main?title=Analyzing%20performance%20degradations%20with%20time%20series%20analysis&shared=true)
 *  [Analysera programfel med autocluster och diffpatterns](https://analytics.applicationinsights.io/demo#/discover/query/main?title=Analyzing%20application%20failures%20with%20autocluster%20and%20diffpatterns&shared=true)
 *  [Avancerade form identifieringar med analys av tidsserier](https://analytics.applicationinsights.io/demo#/discover/query/main?title=Advanced%20shape%20detection%20with%20time%20series%20analysis&shared=true)
 *  [Med hjälp av glidande fönstret operations tooanalyze programanvändning (rullande MAU/DAU osv)](https://analytics.applicationinsights.io/demo#/discover/query/main?title=Using%20sliding%20window%20calculations%20to%20analyze%20usage%20metrics:%20rolling%20MAU~2FDAU%20and%20cohorts&shared=true)
 *  [Identifiering av avbrott i tjänsten baserat på analys av felsökningsloggar](https://analytics.applicationinsights.io/demo#/discover/query/main?title=Detection%20of%20service%20disruptions%20based%20on%20regression%20analysis%20of%20trace%20logs&shared=true) och ett matchande blogginlägg [här](https://maximshklar.wordpress.com/2017/02/16/finding-trends-in-traces-with-smart-data-analytics).
 *  [Profilering programmens prestanda med hjälp av enkla felsökningsloggar](https://analytics.applicationinsights.io/demo#/discover/query/main?title=Profiling%20applications'%20performance%20with%20simple%20debug%20logs&shared=true) och ett matchande blogginlägg [här](https://yossiattasblog.wordpress.com/2017/03/13/first-blog-post/)
 *  [Mäter hello varaktighet för varje steg i din kod flöde med hjälp av enkla felsökningsloggar](https://analytics.applicationinsights.io/demo#/discover/query/main?title=Measuring%20the%20duration%20of%20each%20step%20in%20your%20code%20flow%20using%20simple%20debug%20logs&shared=true) och ett matchande blogginlägg [här](https://yossiattasblog.wordpress.com/2017/03/14/measuring-the-duration-of-each-step-in-your-code-flow-using-simple-debug-logs/)
 *  [Analysera samtidighet med enkla felsökningsloggar](https://analytics.applicationinsights.io/demo#/discover/query/results/chart?title=Analyzing%20concurrency%20with%20simple%20debug%20logs&shared=true) och ett matchande blogginlägg [här](https://yossiattasblog.wordpress.com/2017/03/23/analyzing-concurrency-using-simple-debug-logs/)



## <a name="next-steps"></a>Nästa steg
* Vi rekommenderar att du börjar med hello [språk rundtur](app-insights-analytics-tour.md). 
* Mer om [med hjälp av Analytics](app-insights-analytics-using.md). 
* [Språkreferens för](app-insights-analytics-reference.md). 
