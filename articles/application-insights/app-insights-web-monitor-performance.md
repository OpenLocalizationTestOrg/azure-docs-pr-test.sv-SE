---
title: "aaaMonitor appens hälso- och användningsinformation med Application Insights"
description: "Kom igång med Application Insights. Analysera användning, tillgänglighet och prestanda för din lokala eller Microsoft Azure-program."
services: application-insights
documentationcenter: 
author: CFreemanwa
manager: carmonm
ms.assetid: 40650472-e860-4c1b-a589-9956245df307
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 11/25/2015
ms.author: bwren
ms.openlocfilehash: 9230a6e65e5afb00c36122ff1d1de01ba19cd7f8
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="monitor-performance-in-web-applications"></a>Övervaka prestanda i webbprogram


Kontrollera att programmet fungerar bra och lär dig snabbt eventuella fel. [Application Insights] [ start] information om eventuella prestandaproblem och undantag, och hjälper dig att hitta och diagnostisera hello rot-orsaker.

Application Insights kan övervaka Java- och ASP.NET-webbprogram och tjänster, WCF-tjänster. De kan vara lokalt, på virtuella datorer eller som Microsoft Azure-webbplatser. 

Hello klientsidan vidta Application Insights telemetri från webbsidor och en mängd olika enheter, inklusive iOS, Android och Windows Store-appar.

>[!Note]
> Vi har gjort en ny miljö för att hitta långsamt utför sidor i ditt webbprogram. Om du inte har åtkomst till tooit kan du aktivera det genom att konfigurera alternativ för förhandsgranskning med hello [Preview bladet](app-insights-previews.md). Läs mer om den nya upplevelsen i [hitta och åtgärda flaskhalsar med hello interaktiva prestanda undersökningen](#Find-and-fix-performance-bottlenecks-with-an-interactive-Performance-investigation).

## <a name="setup"></a>Konfigurera övervakning av programprestanda
Om du ännu inte har lagt till Application Insights tooyour projektet (om det inte har ApplicationInsights.config), väljer du något av följande sätt tooget igång:

* [ASP.NET-webbprogram](app-insights-asp-net.md)
  * [Lägg till undantagsövervakning](app-insights-asp-net-exceptions.md)
  * [Lägg till beroende övervakning](app-insights-monitor-performance-live-website-now.md)
* [J2EE-webbprogram](app-insights-java-get-started.md)
  * [Lägg till beroende övervakning](app-insights-java-agent.md)

## <a name="view"></a>Utforska prestandamått
I [hello Azure-portalen](https://portal.azure.com), bläddra toohello Application Insights-resurs som du har skapat för ditt program. hello översikt bladet visar grundläggande prestandadata:

Klicka på alla diagram toosee detalj och toosee resultat för en längre period. Exempel på hello begäranden panelen och välj sedan ett tidsintervall:

![Klicka dig igenom toomore data och välj ett tidsintervall](./media/app-insights-web-monitor-performance/appinsights-48metrics.png)

Klicka på ett diagram toochoose vilka mått som visas, eller lägga till ett nytt diagram och välja dess mått:

![Klicka på ett diagram toochoose mått](./media/app-insights-web-monitor-performance/appinsights-61perfchoices.png)

> [!NOTE]
> **Avmarkera alla hello mått** toosee hello fullständig val som är tillgänglig. hello mått är indelade i grupper. När alla medlemmar i en grupp väljs visas hello andra medlemmar i gruppen endast.

## <a name="metrics"></a>Vad betyder det alla? Prestanda paneler och rapporter
Det finns olika prestandamått som du kan hämta. Vi börjar med de som visas som standard på hello programmet bladet.

### <a name="requests"></a>Begäranden
hello antal HTTP-begäranden tas emot i en angiven period. Jämför detta med hello resultat på andra rapporter toosee hur din app fungerar som hello belastningen varierar.

HTTP-begäranden som innehåller alla begäranden om sidor, data och avbildningar som GET eller POST.

Klicka på hello panelen tooget antal för specifika URL: er.

### <a name="average-response-time"></a>Genomsnittlig svarstid
Mått hello tiden mellan en webbegäran att ange ditt program och hello svar returneras.

hello punkter visar det genomsnittliga glidande dem. Om det finns en mängd begäranden, kan det finnas några som avviker från hello medelvärdet utan en uppenbara belastning eller sjunker i hello graph.

Leta efter ovanliga toppar. I allmänhet förväntat svar tid toorise med en ökning av begäranden. Om hello upphov oproportionerligt att din app träffa en resursgräns t.ex CPU eller hello kapacitet för en tjänst som används.

Klicka på hello panelen tooget gånger för specifika URL: er.

![](./media/app-insights-web-monitor-performance/appinsights-42reqs.png)

### <a name="slowest-requests"></a>Långsammaste begäranden
![](./media/app-insights-web-monitor-performance/appinsights-44slowest.png)

Visar vilka begäranden kan behöva prestandajustering.

### <a name="failed-requests"></a>Misslyckade begäranden
![](./media/app-insights-web-monitor-performance/appinsights-46failed.png)

Antalet begäranden som utlöste undantagsfel utan felhantering.

Klicka på hello panelen toosee hello information om specifika problem och välja en enskild begäran toosee dess information. 

Ett representativt urval fel sparas för enskilda kontroll.

### <a name="other-metrics"></a>Andra mått
toosee vilka andra mått som du kan visa, klicka på ett diagram och avmarkera alla hello mått toosee hello hela tillgängliga uppsättningen. Klicka på (i) toosee varje mått definition.

![Avmarkera alla mått toosee hello hela uppsättningen](./media/app-insights-web-monitor-performance/appinsights-62allchoices.png)

Att välja alla mått inaktiverar hello hello andra som kan visas på samma schema.

## <a name="set-alerts"></a>Ange aviseringar
toobe meddelas via e-post med ovanliga värden för alla mått, lägga till en avisering. Du kan välja toosend hello e-toohello kontoadministratörer eller toospecific e-postadresser.

![](./media/app-insights-web-monitor-performance/appinsights-413setMetricAlert.png)

Ange hello resursen innan hello andra egenskaper. Välj inte hello webtest resurser om du vill tooset aviseringar på prestanda eller användningsstatistik.

Vara försiktig toonote hello enheter där du uppmanas tooenter hello tröskelvärdet.

*Hello Lägg till avisering om visas inte.* -Är detta en grupp konto toowhich som du har skrivskyddad åtkomst? Kontrollera med hello kontoadministratör.

## <a name="diagnosis"></a>Diagnostisera problem
Här följer några tips för att hitta och diagnostisera prestandaproblem:

* Ställ in [webbtester] [ availability] toobe aviserad om webbplatsen kraschar eller svarar långsamt eller felaktigt. 
* Jämförelseantalet hello begäran med andra mått toosee om misslyckade eller långsamma svar är relaterade tooload.
* [Infoga och Sök trace-satser] [ diagnostic] i din kod toohelp identifiera problem.
* Övervaka ditt webbprogram i åtgärden med [direktsänd dataström med mått][livestream].
* Avbilda hello tillstånd på ditt .net-program med [ögonblicksbild felsökare][snapshot].

## <a name="find-and-fix-performance-bottlenecks-with-an-interactive-performance-investigation"></a>Hitta och åtgärda flaskhalsar med en interaktiv prestanda undersökning

Du kan använda hello ny Application Insights interaktiva prestanda undersökningen toolocate områden av ditt webbprogram som saktar ned prestandan. Du kan snabbt hitta vissa sidor som saktar ned och använder hello [Profiling verktyget](app-insights-profiler.md) toosee om det finns en korrelation mellan dessa sidor.

### <a name="create-a-list-of-slow-performing-pages"></a>Skapa en lista över långsamma prestanda sidor 

hello första steget för att söka efter prestandaproblem är tooget en lista över hello långsam svarar sidor. hello skärmbilden nedan visas hur du använder hello prestanda bladet tooget en lista över möjliga sidor tooinvestigate ytterligare. Du kan snabbt se från den här sidan att det var en nedgång i hello svarstid hello-appen på cirka 6:00 PM och igen klockan cirka 10. Du kan också se att hello GET/kundinformation åtgärden hade vissa långvariga åtgärder med en median svarstid 507.05 millisekunder. 

![Application Insights interaktiva prestanda](./media/app-insights-web-monitor-performance/performance1.png)

### <a name="drill-down-on-specific-pages"></a>Detaljnivån på specifika sidor

När du har en ögonblicksbild av appens prestanda kan få du mer information på specifika åtgärder för långsamma prestanda. Klicka på någon åtgärd i hello toosee hello information som visas nedan. Du kan se om hello prestanda var baserat på ett beroende från hello diagram. Du kan också se hur många användare erfarna hello olika svarstider. 

![Application Insights operations bladet](./media/app-insights-web-monitor-performance/performance5.png)

### <a name="drill-down-on-a-specific-time-period"></a>Detaljnivån för en viss tidsperiod

När du har identifierat en punkt i tiden tooinvestigate kan öka detaljnivån ytterligare toolook på hello specifika åtgärder som kan ha orsakat hello prestanda sidåtergivningen. När du klickar på en viss punkt i tiden hämta hello information om hello sida som visas nedan. I hello ser exemplet nedan du hello åtgärderna för en viss tidsperiod tillsammans med hello server svarskoder och hello åtgärden varaktighet. Du kan också ha hello URL: en för att öppna en TFS-arbetsobjekt om du behöver toosend denna information tooyour Utvecklingsteamet.

![Tidsintervallet för Application Insights](./media/app-insights-web-monitor-performance/performance2.png)

### <a name="drill-down-on-a-specific-operation"></a>Detaljnivån med en specifik åtgärd

När du har identifierat en punkt i tiden tooinvestigate kan öka detaljnivån ytterligare toolook på hello specifika åtgärder som kan ha orsakat hello prestanda sidåtergivningen. Klicka på en åtgärd från hello listan toosee hello närmare hello enligt nedan. I det här exemplet som du kan se att hello misslyckades och Application Insights har angett hello information om hello utlöste undantaget hello program. Igen, kan du enkelt skapa en TFS-arbetsobjekt från det här bladet.

![Application Insights åtgärden bladet](./media/app-insights-web-monitor-performance/performance3.png)


## <a name="next"></a>Nästa steg
[Webbtester] [ availability] -webbegäranden skickat tooyour program med jämna mellanrum från hello världen.

[Fånga och söka diagnostikspår] [ diagnostic] – Infoga spårning av anrop och gå igenom hello resultat toopinpoint problem.

[Spårning av användning] [ usage] -ta reda på hur personer använder ditt program.

[Felsökning av] [ qna] - och frågor och svar



<!--Link references-->

[availability]: app-insights-monitor-web-app-availability.md
[diagnostic]: app-insights-diagnostic-search.md
[greenbrown]: app-insights-asp-net.md
[qna]: app-insights-troubleshoot-faq.md
[redfield]: app-insights-monitor-performance-live-website-now.md
[start]: app-insights-overview.md
[usage]: app-insights-web-track-usage.md
[livestream]: app-insights-live-stream.md
[snapshot]: app-insights-snapshot-debugger.md



