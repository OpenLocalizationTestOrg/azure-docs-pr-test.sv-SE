---
title: "Vad är Azure Application Insights aaa? | Microsoft Docs"
description: "Application Performance Management och användningsspårning av ditt live-webbprogram.  Identifiera, hantera och diagnostisera problem och förstå hur din app används."
services: application-insights
documentationcenter: 
author: CFreemanwa
manager: carmonm
ms.assetid: 379721d1-0f82-445a-b416-45b94cb969ec
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: get-started-article
ms.date: 05/14/2017
ms.author: bwren
ms.openlocfilehash: d2596f53a36991fcd08551e6395ece68a5801e39
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="what-is-application-insights"></a>Vad är Application Insights?
Application Insights är en utökningsbar APM-tjänst (Application Performance Management) för webbutvecklare på flera plattformar. Använd den toomonitor live webbprogrammet. Den identifierar automatiskt prestandaavvikelser. Den innehåller kraftfulla analytics verktyg toohelp du diagnostisera problem och toounderstand vad användare utför med din app.  Den är utformad toohelp kontinuerligt förbättra prestanda och användbarhet. Det fungerar för appar på en mängd olika plattformar, inklusive .NET, Node.js och J2EE finns lokalt eller i hello molnet. Den kan integreras med devOps-processen och har anslutning punkter tooa olika utvecklingsverktyg.

![Skapa diagram med statistik över användaraktivitet eller visa detaljer om specifika händelser.](./media/app-insights-overview/00-sample.png)

[Ta en titt på hello introduktion animering](https://www.youtube.com/watch?v=fX2NtGrh-Y0).

## <a name="how-does-application-insights-work"></a>Hur fungerar Application Insights?
Du installerar ett litet instrumentation paket i ditt program och ställa in en Application Insights-resurs i hello Microsoft Azure-portalen. hello instrumentation övervakar din app och skickar telemetri data toohello portal. (hello programmet kan köras var som helst - det har inte toobe finns i Azure.)

Du kan instrumentera inte bara hello webbtjänstprogrammet, utan även alla komponenter som bakgrund och hello JavaScript i hello webbsidor sig själva. 

![Programinstrumentering insikter i din app skickar telemetri tooyour Application Insights-resurs.](./media/app-insights-overview/01-scheme.png)


Du kan dessutom dra i telemetri från hello värdmiljöer som prestandaräknare, Azure-diagnostik eller Docker-loggar. Du kan också ställa in webbtester som regelbundet skicka syntetiska begäranden tooyour webbtjänsten.

Alla dessa telemetri strömmar är integrerade i hello Azure-portalen, du kan tillämpa kraftfulla analytiska och Sök verktyg toohello rådata.


### <a name="whats-hello-overhead"></a>Vad är hello kostnader?
hello påverkan på prestandan för din app är mycket liten. Anropsspårning är icke-blockerande, och grupperas och skickas i en separat tråd.

## <a name="what-does-application-insights-monitor"></a>Vad övervakar Application Insights?

Application Insights syftar hello Utvecklingsteamet, toohelp du förstår hur din app fungerar och hur den används. Tjänsten övervakar:

* **Begärandefrekvens, svarstider och felfrekvens** – Ta reda på vilka sidor som är mest populära, vid vilka tidpunkter på dagen och var dina användare finns. Se vilka sidor som fungerar bäst. Om svarstiden och felfrekvensen är hög när det finns många begäranden kan det bero på ett resurstilldelningsproblem. 
* **Beroendefrekvens, svarstider och felfrekvens** – Ta reda på om externa tjänster gör systemet långsammare.
* **Undantag** – analysera hello samman statistik, eller välja specifika instanser och detaljerat hello stackspårning och relaterade begäranden. Både server- och webbläsarundantag rapporteras.
* **Sidvyer och inläsningsprestanda** – Rapporteras av användarnas webbläsare.
* **AJAX-anrop** från webbsidor – frekvens, svarstider och felfrekvens.
* **Antal användare och sessioner**.
* **Prestandaräknare** från dina Windows- eller Linux-serverdatorer, till exempel processor, minne och nätverksanvändning. 
* **Värddiagnostik** från Docker eller Azure. 
* **Diagnostikspårningsloggar** från din app – så att du kan jämföra spårningshändelser med begäranden.
* **Anpassade händelser och mått** att du kan skriva själv i hello klient eller server kod, tootrack affärshändelser som objekt säljs eller spel vann.

## <a name="where-do-i-see-my-telemetry"></a>Var ser jag min telemetri?

Det finns många sätt tooexplore dina data. Läs dessa artiklar:

|  |  |
| --- | --- |
| [**Smart identifiering och manuella aviseringar**](app-insights-proactive-diagnostics.md)<br/>Automatisk aviseringar anpassa tooyour app normal mönster av telemetri och utlösare när det finns något utanför hello vanliga mönstret. Du kan också [ställa in aviseringar](app-insights-alerts.md) på särskilda nivåer med anpassade måttvärden eller standardmått. |![Aviseringsexempel](./media/app-insights-overview/alerts-tn.png) |
| [**Programkarta**](app-insights-app-map.md)<br/>hello komponenter i appen med nyckelvärden och -varningar. |![Programkarta](./media/app-insights-overview/appmap-tn.png)  |
| [**Profilerare**](app-insights-profiler.md)<br/>Kontrollera hello körning av profiler provade begäranden. |![Profilerare](./media/app-insights-overview/profiler.png) |
| [**Användningsanalys**](app-insights-usage-overview.md)<br/>Analysera användarsegment och kvarhållning.|![Kvarhållningsverktyg](./media/app-insights-overview/retention.png) |
| [**Diagnostiksökning efter instansdata**](app-insights-diagnostic-search.md)<br/>Sök efter och filtrera händelser, till exempel begäranden, undantag, beroendeanrop, loggspårningar och sidvyer.  |![Telemetrisökning](./media/app-insights-overview/search-tn.png) |
| [**Metrics Explorer för aggregerade data**](app-insights-metrics-explorer.md)<br/>Utforska, filtrera och segmentera aggregerade data, till exempel begärande-, fel- och undantagsfrekvens, svarstider och sidinläsningstider. |![Mått](./media/app-insights-overview/metrics-tn.png) |
| [**Instrumentpaneler**](app-insights-dashboards.md#dashboards)<br/>Kombinera data från flera resurser och dela med andra. Bra för flera komponenten program och för kontinuerlig visning i hello team rum. |![Exempel på instrumentpaneler](./media/app-insights-overview/dashboard-tn.png) |
| [**Live-ström med mätvärden**](app-insights-live-stream.md)<br/>När du distribuerar en ny version kan du titta på dessa nära realtid prestanda indikatorer toomake att allt fungerar som förväntat. |![Exempel på live-mätvärden](./media/app-insights-overview/live-metrics-tn.png) |
| [**Analytics**](app-insights-analytics.md)<br/>Besvara svåra frågor om appens prestanda och användning med hjälp av det här kraftfulla frågespråket. |![Analytics-exempel](./media/app-insights-overview/analytics-tn.png) |
| [**Visual Studio**](app-insights-visual-studio.md)<br/>Se prestandadata i hello kod. Gå toocode från stackspår.|![Visual Studio](./media/app-insights-overview/visual-studio-tn.png) |
| [**Felsökning av ögonblicksbild**](app-insights-snapshot-debugger.md)<br/>Felsök ögonblicksbilder från program som körs med parametervärden.|![Visual Studio](./media/app-insights-overview/snapshot.png) |
| [**Power BI**](app-insights-export-power-bi.md)<br/>Integrera användningsmätvärden med annan Business Intelligence.| ![Power BI](./media/app-insights-overview/power-bi.png)|
| [**REST API**](https://dev.applicationinsights.io/)<br/>Skriva koden toorun frågor via din mått och rådata.| ![REST API](./media/app-insights-overview/rest-tn.png) |
| [**Löpande export**](app-insights-export-telemetry.md)<br/>Massredigera export av rådata toostorage när den tas emot. |![Exportera](./media/app-insights-overview/export-tn.png) |

## <a name="how-do-i-use-application-insights"></a>Hur använder jag Application Insights?

### <a name="monitor"></a>Övervaka
Installera Application Insights i din app, konfigurera [webbtester för tillgänglighet](app-insights-monitor-web-app-availability.md) och:

* Konfigurera en [instrumentpanelen](app-insights-dashboards.md) sidan belastningar och AJAX-anrop för ditt team rum tookeep ett öga på belastning, svarstider och hello prestanda hos dina beroenden.
* Identifiera som är hello långsammaste och de flesta misslyckade begäranden.
* Titta på [Liveströmning](app-insights-live-stream.md) när du distribuerar en ny version tooknow omedelbart om försämring.

### <a name="detect-diagnose"></a>Identifiera och diagnostisera
När du får en avisering eller identifierar ett problem:

* Utvärdera hur många användare som påverkas.
* Korrelera fel med undantag, beroendeanrop och spårningar.
* Undersök profilerare, ögonblicksbilder, stackdumpar och spårningsloggar.

### <a name="build-measure-learn"></a>Bygg, mät och lär
[Mäta hello effektivt](app-insights-usage-overview.md) av varje ny funktion som du distribuerar.

* Planera toomeasure hur kunder använder nya UX eller funktioner för företag.
* Skriv in anpassad telemetri i din kod.
* Grundläggande hello nästa utveckling Bläddra på hårddisken bevis från din telemetri.

## <a name="get-started"></a>Kom igång
Application Insights är en av hello många tjänster finns i Microsoft Azure och telemetri det skickas för analys och presentation. Innan du gör något annat måste en prenumeration för[Microsoft Azure](http://azure.com). Är det kostnadsfria toosign upp och om du väljer hello grundläggande [priser plan](https://azure.microsoft.com/pricing/details/application-insights/) av Application Insights är gratis tills programmet har växt toohave betydande användning. Om din organisation redan har en prenumeration, de kan lägga till ditt Microsoft-konto tooit.

Det finns flera sätt tooget igång. Börja på det sätt som passar dig bäst. Du kan lägga till hello andra senare.

* **AT körtid: instrumentera ditt webbprogram på hello-servern.** Undviker någon uppdatering toohello kod. Du måste administratören tooyour fjärråtkomstserver.
  * [**IIS lokalt eller på en virtuell dator**](app-insights-monitor-performance-live-website-now.md)
  * [**Azure-webbapp eller virtuell dator**](app-insights-monitor-performance-live-website-now.md)
  * [**J2EE**](app-insights-java-live.md)
* **Vid tid: Lägg till Application Insights tooyour kod.** Kan du toowrite anpassad telemetri och tooinstrument backend-appar och program.
  * [Visual Studio](app-insights-asp-net.md) 2013 uppdatering 2 eller senare.
  * Java i [Eclipse](app-insights-java-eclipse.md) eller [andra verktyg](app-insights-java-get-started.md)
  * [Node.js](app-insights-nodejs.md)
  * [Andra plattformar](app-insights-platforms.md)
* **[Instrumentera dina webbsidor](app-insights-javascript.md)** för sidvisning, AJAX och annan telemetri på klientsidan.
* **[Tillgänglighetstester](app-insights-monitor-web-app-availability.md)** –pinga din webbplats regelbundet från våra servrar.


## <a name="next-steps"></a>Nästa steg
Kom igång under körningsfasen med:

* [IIS-server](app-insights-monitor-performance-live-website-now.md)
* [J2EE-server](app-insights-java-live.md)

Kom igång under utvecklingsfasen med:

* [ASP.NET](app-insights-asp-net.md)
* [Java](app-insights-java-get-started.md)
* [Node.js](app-insights-nodejs.md)

## <a name="support-and-feedback"></a>Support och feedback
* Frågor och problem:
  * [Felsökning][qna]
  * [MSDN-forum](https://social.msdn.microsoft.com/Forums/vstudio/home?forum=ApplicationInsights)
  * [StackOverflow](http://stackoverflow.com/questions/tagged/ms-application-insights)
* Dina förslag:
  * [UserVoice](https://visualstudio.uservoice.com/forums/357324)
* Blogg:
  * [Application Insights-blogg](https://azure.microsoft.com/blog/tag/application-insights)

## <a name="videos"></a>Videoklipp

[![Animerad introduktion](./media/app-insights-overview/video-front-1.png)](https://www.youtube.com/watch?v=fX2NtGrh-Y0)

> [!VIDEO https://channel9.msdn.com/events/Connect/2016/100/player] 

<!--Link references-->

[android]: https://github.com/Microsoft/ApplicationInsights-Android
[azure]: ../insights-perf-analytics.md
[client]: app-insights-javascript.md
[desktop]: app-insights-windows-desktop.md
[detect]: app-insights-detect-triage-diagnose.md
[greenbrown]: app-insights-asp-net.md
[ios]: https://github.com/Microsoft/ApplicationInsights-iOS
[java]: app-insights-java-get-started.md
[knowUsers]: app-insights-web-track-usage.md
[platforms]: app-insights-platforms.md
[portal]: http://portal.azure.com/
[qna]: app-insights-troubleshoot-faq.md
[redfield]: app-insights-monitor-performance-live-website-now.md
