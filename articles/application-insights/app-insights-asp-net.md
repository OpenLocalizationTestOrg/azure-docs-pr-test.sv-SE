---
title: "aaaSet in web app analytics för ASP.NET med Azure Application Insights | Microsoft Docs"
description: "Konfigurera prestanda-, tillgänglighets- och användningsanalyser för din lokala eller Azure-baserade ASP.NET-webbplats."
services: application-insights
documentationcenter: .net
author: CFreemanwa
manager: carmonm
ms.assetid: d0eee3c0-b328-448f-8123-f478052751db
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: get-started-article
ms.date: 05/15/2017
ms.author: bwren
ms.openlocfilehash: 61a3cdce68da48bfb9450b1d296acc1535f50a38
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="set-up-application-insights-for-your-aspnet-website"></a>Konfigurera Application Insights för din ASP.NET-webbplats

Den här proceduren konfigurerar din ASP.NET web app toosend telemetri toohello [Azure Application Insights](app-insights-overview.md) service. Den fungerar för ASP.NET-program som finns i din egen IIS-server eller i hello molnet. Du får diagram och en kraftfull frågespråk som hjälper dig förstå hello prestanda för din app och hur personer använder plus automatiska meddelanden på fel eller prestandaproblem. Många utvecklare hitta funktionerna stor som de är, men du kan också utöka och anpassa hello telemetri om du behöver.

Installationen kräver bara några klick i Visual Studio. Du har hello alternativet tooavoid kostnader genom att begränsa hello telemetrivolym. Detta ger dig tooexperiment och felsökning eller toomonitor en plats med inte många användare. När du bestämmer toogo vidare och om du vill övervaka din produktionsplatsen är enkelt tooraise hello gränsen senare.

## <a name="before-you-start"></a>Innan du börjar
Du behöver:

* Visual Studio 2013 Update 3 eller senare. Senare versioner är att föredra.
* En prenumeration för[Microsoft Azure](http://azure.com). Om din grupp eller organisation har en Azure-prenumeration, hello ägare kan lägga till dig tooit, med hjälp av din [Microsoft-konto](http://live.com).

Det finns alternativ avsnitt toolook på om du är intresserad av:

* [Instrumentering av en webbapp under körning](app-insights-monitor-performance-live-website-now.md)
* [Azure Cloud Services](app-insights-cloudservices.md)

## <a name="ide"></a>Steg 1: Lägg till hello Application Insights SDK

Högerklicka på ditt webbappsprojekt i Solution Explorer och välj **Lägg till** > **Application Insights Telemetry...** eller **Konfigurera Application Insights**.

![Skärmbild av Solution Explorer med Lägg till Application Insights Telemetry markerat](./media/app-insights-asp-net/appinsights-03-addExisting.png)

(I Visual Studio 2015 finns också alternativet-tooadd Application Insights i dialogrutan för hello nytt projekt.)

Fortsätt toohello konfigurationssidan för Application Insights:

![Skärmbild av sidan Register your app with Application Insights (Registrera din app med Application Insights)](./media/app-insights-asp-net/visual-studio-register-dialog.png)

**a.** Välj hello-konto och att du använder tooaccess Azure-prenumeration.

**b.** Välj hello resurs i Azure där du vill toosee hello data från din app. Vanligtvis:

* Använd en [enkel resurs för olika komponenter](app-insights-monitor-multi-role-apps.md) i ett enda program. 
* Skapa separata resurser för orelaterade program.
 
Om du vill tooset hello grupp eller hello resursplats där dina data lagras **konfigurera inställningar för**. Resursgrupper är används toocontrol åtkomst toodata. Till exempel om du har flera program som ingår i hello samma system, kan du placera sina Application Insights-data i hello samma resursgrupp.

**c.** Ange ett tak på hello lediga volymen begränsningen, tooavoid avgifter. Application Insights är Frigör tooa vissa telemetrivolym. När hello resursen har skapats kan du ändra ditt val i hello portal genom att öppna **funktioner + pris** > **volym datahantering** > **dagliga volymen cap**.

**d.** Klicka på **registrera** toogo vidare och konfigurera Application Insights för ditt webbprogram. Telemetri skickas toohello [Azure-portalen](https://portal.azure.com), både under felsökning och när du har publicerat din app.

**e.** Om du inte vill toosend telemetri toohello portal när du felsöker kan bara lägga till hello Application Insights SDK tooyour app men inte konfigurera en resurs i hello-portalen. När du felsöker kommer du att kunna toosee telemetri i Visual Studio. Senare, du kan gå tillbaka till sidan för konfiguration av toothis eller du kan vänta tills du har distribuerat din app och [växla telemetri vid körning](app-insights-monitor-performance-live-website-now.md).


## <a name="run"></a> Steg 2: Kör appen
Kör din app med F5. Öppna olika sidor toogenerate vissa telemetri.

I Visual Studio kan du se antalet hello-händelser som har loggats.

![Skärmbild av Visual Studio. hello Application Insights knappen visas under felsökning.](./media/app-insights-asp-net/54.png)

## <a name="step-3-see-your-telemetry"></a>Steg 3: Visa din telemetri
Du kan se din telemetri i Visual Studio eller i hello Application Insights web-portalen. Sök telemetri i Visual Studio toohelp du felsöka din app. Övervaka prestanda och användning i hello webbportalen när datorn är aktiv. 

### <a name="see-your-telemetry-in-visual-studio"></a>Visa din telemetri i Visual Studio

I Visual Studio öppnas hello Application Insights. Antingen på hello **Programinsikter** knappen eller högerklicka på projektet i Solution Explorer, Välj **Programinsikter**, och klicka sedan på **Sök Live telemetri**.

Hello Visual Studio Application Insights Sök i fönstret visas hello **Data från felsökningssessionen** vy för telemetri som har genererats i hello på serversidan för din app. Experimentera med hello filter och på varje händelse toosee detalj.

![Skärmbild av hello visa Data från felsökningssessionen i hello Application Insights-fönstret.](./media/app-insights-asp-net/55.png)

> [!NOTE]
> Om du inte ser några data, kontrollera hello tidsintervallet är korrekt och klicka på sökikonen hello.

[Lär dig mer om Application Insights-verktygen i Visual Studio](app-insights-visual-studio.md).

<a name="monitor"></a>
### <a name="see-telemetry-in-web-portal"></a>Visa telemetri i webbportalen

Du kan också se telemetri i hello Application Insights web-portalen (om du har angett tooinstall endast hello SDK). hello-portalen har flera diagram analytiska verktyg vyer, och mellan olika komponenter än Visual Studio. hello portal visar även varningar.

Öppna Application Insights-resursen. Antingen logga in toohello [Azure-portalen](https://portal.azure.com/) och hitta den där eller högerklicka på hello-projekt i Visual Studio och låta den får du det.

![Skärmbild av Visual Studio, visar hur tooopen hello Application Insights-portalen](./media/app-insights-asp-net/appinsights-04-openPortal.png)

> [!NOTE]
> Om du får ett fel: har du fler än en uppsättning autentiseringsuppgifter för Microsoft och du är inloggad med hello fel set? Logga ut och logga in igen i hello-portalen.

hello portalen öppnas på en vy över hello telemetri från din app.

![Skärmbild av Application Insights-översiktssidan](./media/app-insights-asp-net/66.png)

Klicka på någon sida vid sida eller diagram toosee detalj i hello-portalen.

[Lär dig mer om hur du använder Application Insights i hello Azure-portalen](app-insights-dashboards.md).

## <a name="step-4-publish-your-app"></a>Steg 4: Publicera appen
Publicera din app tooyour IIS-servern eller tooAzure. Titta på [direktsänd dataström med mått](app-insights-metrics-explorer.md#live-metrics-stream) toomake att allt fungerar.

Din telemetri bygger upp i hello Application Insights-portalen där du kan övervaka, söka din telemetri och ställa in [instrumentpaneler](app-insights-dashboards.md). Du kan också använda hello kraftfulla [Log Analytics-frågespråket](https://docs.loganalytics.io/) tooanalyze användnings- och prestanda- eller toofind specifika händelser.

Du kan också fortsätta tooanalyze din telemetri i [Visual Studio](app-insights-visual-studio.md), med verktyg som diagnostiska Sök och [trender](app-insights-visual-studio-trends.md).

> [!NOTE]
> Om din app skickar tillräckligt med telemetri tooapproach hello [throttling-begränsning](app-insights-pricing.md#limits-summary), automatisk [provtagning](app-insights-sampling.md) växlar. Samplingsfrekvensen minskar hello antalet telemetri som skickats från din app samtidigt som korrelerade för att ställa diagnoser.
>
>

## <a name="land"></a> Då var allt klart

Grattis! Du installerade hello Application Insights paketet i din app och konfigurerats toosend telemetri toohello Application Insights-tjänsten på Azure.

![Diagram över flödet av telemetri](./media/app-insights-asp-net/01-scheme.png)

hello Azure-resurs som tar emot telemetri för din app har identifierats av en *instrumentation nyckeln*. Den här nyckeln hittar du i hello ApplicationInsights.config-filen.


## <a name="upgrade-toofuture-sdk-versions"></a>Uppgradera toofuture SDK-versioner
tooupgrade tooa [nya versionen av hello SDK](https://github.com/Microsoft/ApplicationInsights-dotnet-server/releases)öppnar hello **NuGet-Pakethanteraren** igen, och filtret på installerade paket. Markera **Microsoft.ApplicationInsights.Web** och välj **Uppgradera**.

Om du har gjort alla anpassningar tooApplicationInsights.config spara en kopia av den innan du uppgraderar. Sedan sammanfoga ändringarna i hello ny version.

## <a name="video"></a>Video

> [!VIDEO https://channel9.msdn.com/events/Connect/2016/100/player]

## <a name="next-steps"></a>Nästa steg

### <a name="more-telemetry"></a>Mer telemetri

* **[Webbläsare och webbsidesinläsning](app-insights-javascript.md)**  – Infoga ett kodfragment på dina webbsidor.
* **[Få mer detaljerad beroende- och undantagsövervakning](app-insights-monitor-performance-live-website-now.md)**  – Installera Status Monitor på servern.
* **[Anpassade händelser Code](app-insights-api-custom-events-metrics.md)**  toocount, tid, eller ett mått användaråtgärder.
* **[Hämta loggdata](app-insights-asp-net-trace-logs.md)**  – Korrelera loggdata med din telemetri.

### <a name="analysis"></a>Analys

* **[Arbeta med Application Insights i Visual Studio](app-insights-visual-studio.md)**<br/>Innehåller information om felsökning med telemetri diagnostiska sökningen och detaljerad toocode.
* **[Arbeta med hello Application Insights-portalen](app-insights-dashboards.md)**<br/> Innehåller information om instrumentpaneler, kraftfulla verktyg för diagnostik och analys, aviseringar, realtidsmappning av beroenden för din app och telemetriexport.
* **[Analytics](app-insights-analytics-tour.md)**  -hello kraftfulla frågespråk.

### <a name="alerts"></a>Aviseringar

* [Tillgänglighetstester](app-insights-monitor-web-app-availability.md): skapa tester toomake till webbplatsen visas på hello-webbplatsen.
* [Diagnostik för smartkort](app-insights-proactive-diagnostics.md): dessa tester körs automatiskt, så du inte behöver toodo något annat tooset dem upp. De berättar om din app har ett ovanligt antal misslyckade begäranden.
* [Mått aviseringar](app-insights-alerts.md): ange dessa toowarn om måttet överskrider ett tröskelvärde. Du kan ställa in dem för anpassade mätningar som du kodar i din app.

### <a name="automation"></a>Automation

* [Automatisera skapandet av en Application Insights-resurs](app-insights-powershell.md)
