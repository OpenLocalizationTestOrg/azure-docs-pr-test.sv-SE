---
title: "aaaHow gör jag... i Azure Application Insights | Microsoft Docs"
description: "Vanliga frågor och svar i Application Insights."
services: application-insights
documentationcenter: 
author: CFreemanwa
manager: carmonm
ms.assetid: 48b2b644-92e4-44c3-bc14-068f1bbedd22
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 04/04/2017
ms.author: bwren
ms.openlocfilehash: 89294c3583b7c4e7998143be6d359f2deb3c8f49
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="how-do-i--in-application-insights"></a>Hur kan jag ... i Application Insights?
## <a name="get-an-email-when-"></a>Få ett e-postmeddelande när...
### <a name="email-if-my-site-goes-down"></a>E-post om min plats kraschar
Ange en [tillgänglighet webbtest](app-insights-monitor-web-app-availability.md).

### <a name="email-if-my-site-is-overloaded"></a>E-post om min plats är överbelastad.
Ange en [avisering](app-insights-alerts.md) på **serversvarstid**. Ett tröskelvärde mellan 1 och 2 sekunder ska fungera.

![](./media/app-insights-how-do-i/030-server.png)

Din app kan också visa loggar belastning genom att returnera felkoder. Ange en varning på **misslyckade begäranden**.

Om du vill tooset en avisering i **undantag**, du måste kanske toodo [vissa ytterligare inställningar](app-insights-asp-net-exceptions.md) i ordning toosee data.

### <a name="email-on-exceptions"></a>E-post på undantag
1. [Konfigurera undantagsövervakning](app-insights-asp-net-exceptions.md)
2. [Ställ in en varning](app-insights-alerts.md) räkna mått på hello undantag

### <a name="email-on-an-event-in-my-app"></a>E-post på en händelse i min app
Anta att du vill att tooget ett e-postmeddelande när en viss händelse inträffar. Application Insights inte ger den här funktionen direkt, men den kan [skicka en avisering när en måttet överskrider ett tröskelvärde](app-insights-alerts.md).

Aviseringar kan ställas in på [anpassade mått](app-insights-api-custom-events-metrics.md#trackmetric), men inte anpassade händelser. Skriva vissa kod tooincrease ett mått när hello händelse inträffar:

    telemetry.TrackMetric("Alarm", 10);

Eller:

    var measurements = new Dictionary<string,double>();
    measurements ["Alarm"] = 10;
    telemetry.TrackEvent("status", null, measurements);

Eftersom aviseringar har två tillstånd, har du toosend ett lågt värde när du funderar på hello avisering toohave avslutades:

    telemetry.TrackMetric("Alarm", 0.5);

Skapa ett diagram i [mått explorer](app-insights-metrics-explorer.md) toosee din larm:

![](./media/app-insights-how-do-i/010-alarm.png)

Ange en avisering toofire om hello mått går över ett mid värde under en kort period:

![](./media/app-insights-how-do-i/020-threshold.png)

Ange hello medelvärdet period toohello minimum.

Du får e-postmeddelanden när hello mått går över såväl under hello tröskelvärde.

Vissa punkter tooconsider:

* En avisering har två lägen (”varning” och ”felfri”). hello tillstånd utvärderas bara när ett mått tas emot.
* Ett e-postmeddelande skickas endast när hello tillstånd ändras. Det är därför du har toosend mått för hög och låg-värde.
* tooevaluate hello aviseringen hello medelvärde tas emot hello värden över hello föregående period. Detta sker varje gång ett mått tas emot, så e-postmeddelanden skickas oftare än hello angivna perioden.
* Eftersom e-postmeddelanden skickas både ”varning” och ”felfri”, kanske du vill tooconsider nytt tänker din one-shot händelse som ett villkor för två tillstånd. Till exempel i stället för en ”jobbet är slutfört”-händelsen har en ”jobb pågår” villkor, där du får e-post i hello början och slutet av ett jobb.

### <a name="set-up-alerts-automatically"></a>Ställa in aviseringar automatiskt
[Använd PowerShell toocreate nya aviseringar](app-insights-alerts.md#automation)

## <a name="use-powershell-toomanage-application-insights"></a>Använd PowerShell tooManage Application Insights
* [Skapa nya resurser](app-insights-powershell-script-create-resource.md)
* [Skapa nya aviseringar](app-insights-alerts.md#automation)

## <a name="separate-telemetry-from-different-versions"></a>Separata telemetri från olika versioner

* Flera roller i en app: använda en enda Application Insights-resurs och filtrera på cloud_Rolename. [Läs mer](app-insights-monitor-multi-role-apps.md)
* Avgränsa utveckling och test-versioner: olika Application Insights-resurser. Hämta hello instrumentation nycklar från web.config. [Läs mer](app-insights-separate-resources.md)
* Rapportering skapa versioner: lägga till en egenskap med ett telemetri initieraren. [Läs mer](app-insights-separate-resources.md)

## <a name="monitor-backend-servers-and-desktop-apps"></a>Övervaka backend-servrar och -program
[Använd hello Windows Server SDK modulen](app-insights-windows-desktop.md).

## <a name="visualize-data"></a>Visualisera data
#### <a name="dashboard-with-metrics-from-multiple-apps"></a>Instrumentpanel med mått för flera appar
* I [mått Explorer](app-insights-metrics-explorer.md), anpassa ditt diagram och spara den som en favorit. Fäst den toohello Azure instrumentpanelen.

#### <a name="dashboard-with-data-from-other-sources-and-application-insights"></a>Instrumentpanel med data från andra källor och Application Insights
* [Exportera telemetri tooPower BI](app-insights-export-power-bi.md).

Eller

* Använda SharePoint som instrumentpanelen visar data i SharePoint-webbdelar. [Använd den löpande exporten och Stream Analytics tooexport tooSQL](app-insights-code-sample-export-sql-stream-analytics.md).  Använd PowerView tooexamine hello databas och skapa en SharePoint-webbdel för PowerView.

<a name="search-specific-users"></a>

### <a name="filter-out-anonymous-or-authenticated-users"></a>Filtrera ut anonyma och autentiserade användare
Om dina användare loggar in, kan du ange hello [autentiserat användar-id](app-insights-api-custom-events-metrics.md#authenticated-users). (Det inte sker automatiskt.)

Sedan kan du:

* Söka på särskilda användar-ID

![](./media/app-insights-how-do-i/110-search.png)

* Filtrera mått tooeither anonyma och autentiserade användare

![](./media/app-insights-how-do-i/115-metrics.png)

## <a name="modify-property-names-or-values"></a>Ändra egenskapsnamn eller värden
Skapa en [filter](app-insights-api-filtering-sampling.md#filtering). På så sätt kan du ändra eller filtrera telemetri innan det skickas från din app tooApplication insikter.

## <a name="list-specific-users-and-their-usage"></a>Lista över specifika användare och deras användning
Om du bara vill för[Sök efter specifika användare](#search-specific-users), du kan ange hello [autentiserat användar-id](app-insights-api-custom-events-metrics.md#authenticated-users).

Om du vill ha en lista över användare med data, till exempel vilka sidor du de titta på eller hur ofta de loggar in, har du två alternativ:

* [Ange autentiserat användar-id](app-insights-api-custom-events-metrics.md#authenticated-users), [exportera tooa databasen](app-insights-code-sample-export-sql-stream-analytics.md) och Använd lämpliga verktyg tooanalyze dina användardata.
* Om du har mindre antal användare kan skicka anpassade händelser eller mått, med hello data av intresse som hello värde eller händelsenamnet och inställningen hello användar-id som en egenskap. tooanalyze sidvisningar Ersätt hello standard JavaScript trackPageView anrop. tooanalyze serversidan telemetri använder en telemetri initieraren tooadd hello användar-id tooall server telemetri. Därefter kan du filtrera och segment mått och sökningar på hello användar-id.

## <a name="reduce-traffic-from-my-app-tooapplication-insights"></a>Minska trafiken från min app tooApplication insikter
* I [ApplicationInsights.config](app-insights-configuration-with-applicationinsights-config.md), inaktivera alla moduler som du inte behöver dessa hello prestandaräknaren insamlaren.
* Använd [provtagning och filtrering](app-insights-api-filtering-sampling.md) på hello SDK.
* Begränsa hello antalet Ajax-anrop som rapporterats för alla sidor i dina webbsidor. Hello skript kodutdrag efter `instrumentationKey:...` , infoga: `,maxAjaxCallsPerView:3` (eller ett lämpligt antal).
* Om du använder [TrackMetric](app-insights-api-custom-events-metrics.md#trackmetric), compute hello aggregering av batchar av måttvärden innan du skickar hello resultat. Det finns en överlagring av TrackMetric() som tillhandahåller för som.

Lär dig mer om [priser och kvoter](app-insights-pricing.md).

## <a name="disable-telemetry"></a>Inaktivera telemetri
för**dynamiskt stoppa och starta** hello insamling och vidarebefordran av telemetri från hello-servern:

```

    using  Microsoft.ApplicationInsights.Extensibility;

    TelemetryConfiguration.Active.DisableTelemetry = true;
```



för**inaktivera valda standard insamlare** – till exempel prestandaräknare, HTTP-begäranden eller beroenden - ta bort eller kommentera ut hello relevanta rader i [ApplicationInsights.config](app-insights-api-custom-events-metrics.md). Du kan göra detta, till exempel om du vill toosend TrackRequest data.

## <a name="view-system-performance-counters"></a>Visa prestandaräknare för system
Bland hello är mått som du kan visa i metrics explorer en uppsättning system prestandaräknare. Det finns en fördefinierad bladet med titeln **servrar** som visar flera.

![Öppna Application Insights-resursen och klicka på servrar](./media/app-insights-how-do-i/121-servers.png)

### <a name="if-you-see-no-performance-counter-data"></a>Om du ser inga prestandaräknardata
* **IIS-servern** på din egen dator eller på en virtuell dator. [Installera statusövervakaren](app-insights-monitor-performance-live-website-now.md).
* **Webbplatsen för Azure** -vi stöder inte prestandaräknare ännu. Det finns flera mått som du kan få som en del av hello Azure-webbplats på Kontrollpanelen.
* **UNIX-server** - [installera collectd](app-insights-java-collectd.md)

### <a name="toodisplay-more-performance-counters"></a>toodisplay mer prestandaräknare
* Första, [lägga till ett nytt diagram](app-insights-metrics-explorer.md) och se om hello räknare i grundläggande hello anges vi erbjuder.
* Om inte, [lägga till hello toohello räknaruppsättningen samlas in av hello prestandaräknarmodulen](app-insights-performance-counters.md).
