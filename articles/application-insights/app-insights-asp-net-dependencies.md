---
title: "aaaDependency spårning i Azure Application Insights | Microsoft Docs"
description: "Analysera användningen, tillgängligheten och prestanda i din lokala program eller Microsoft Azure-webbapp med Application Insights."
services: application-insights
documentationcenter: .net
author: CFreemanwa
manager: carmonm
ms.assetid: d15c4ca8-4c1a-47ab-a03d-c322b4bb2a9e
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 05/04/2017
ms.author: bwren
ms.openlocfilehash: e72f5465462ae8e64363cbbaa62911aff636c504
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="set-up-application-insights-dependency-tracking"></a>Konfigurera Application Insights: beroende-spårning
En *beroende* är en extern komponent som anropas av din app. Det är normalt en tjänst som anropas med HTTP, eller en databas eller ett filsystem. [Application Insights](app-insights-overview.md) mäter hur länge programmet väntar på beroenden och hur ofta en beroendeanropet misslyckas. Du kan undersöka specifika anrop och koppla dem toorequests och undantag.

![exempeldiagram](./media/app-insights-asp-net-dependencies/10-intro.png)

beroendeövervakare för hello out box rapporter för närvarande anrop toothese typer av beroenden:

* Server
  * SQL-databaser
  * ASP.NET-webbprogram och WCF-tjänster som använder HTTP-baserade bindningar
  * Lokal eller fjärransluten HTTP-anrop
  * Azure Cosmos DB, tabell, blob-lagring och kön
* Webbsidor
  * AJAX-anrop

Övervaka fungerar med hjälp av [byte kod instrumentation](https://msdn.microsoft.com/library/z9z62c29.aspx) runt valda metoder. Prestanda försämras är minimal.

Du kan också skriva egna SDK anrop toomonitor andra beroenden, både i hello klienten och servern kod med hjälp av hello [TrackDependency API](app-insights-api-custom-events-metrics.md#trackdependency).

## <a name="set-up-dependency-monitoring"></a>Konfigurera övervakning av beroende
Partiell beroendeinformation samlas in automatiskt med hello [Application Insights SDK](app-insights-asp-net.md). tooget fullständiga data, installera hello lämplig agent för hello-värdservern.

| Plattform | Installera |
| --- | --- |
| IIS-servern |Antingen [installera statusövervakaren på servern](app-insights-monitor-performance-live-website-now.md) eller [uppgradera ditt program too.NET framework 4.6 eller senare](http://go.microsoft.com/fwlink/?LinkId=528259) och installera hello [Application Insights SDK](app-insights-asp-net.md) i din app. |
| Azure-webbapp |I Kontrollpanelen web app, [öppna hello Application Insights-bladet i Kontrollpanelen web app](app-insights-azure-web-apps.md) och välj Installera om du uppmanas. |
| Azure-molntjänst |[Använd startaktivitet](app-insights-cloudservices.md) eller [installera .NET framework 4.6 +](../cloud-services/cloud-services-dotnet-install-dotnet.md) |

## <a name="where-toofind-dependency-data"></a>Där toofind beroendedata
* [Programavbildningen](#application-map) visualizes beroenden mellan appen och angränsande komponenter.
* [Prestanda, webbläsare och fel blad](#performance-and-blades) visa beroende serverdata.
* [Webbläsare bladet](#ajax-calls) visar AJAX-anrop från användarnas webbläsare.
* [Klicka dig igenom från långsamma eller misslyckade förfrågningar](#diagnose-slow-requests) toocheck sina beroendeanrop.
* [Analytics](#analytics) kan vara används tooquery beroendedata.

## <a name="application-map"></a>Programkarta
Programavbildningen fungerar som en visual stöd toodiscovering beroenden mellan hello komponenter i ditt program. Den genereras automatiskt från hello telemetri från din app. Det här exemplet visar AJAX-anrop från hello webbläsare skript och REST-anrop från hello server app tootwo externa tjänster.

![Programkarta](./media/app-insights-asp-net-dependencies/08.png)

* **Navigera från hello rutorna** toorelevant beroende och andra diagram.
* **PIN-kod hello kartan** toohello [instrumentpanelen](app-insights-dashboards.md), där den fungerar som den ska.

[Läs mer](app-insights-app-map.md).

## <a name="performance-and-failure-blades"></a>Prestanda och fel blad
hello prestanda bladet visar hello varaktighet för beroendeanrop gjorda av hello server-appen. Det finns en översikt över diagram och en tabell som är segmenterat av anrop.

![Prestandadiagram bladet beroende](./media/app-insights-asp-net-dependencies/dependencies-in-performance-blade.png)

Klicka dig igenom hello sammanfattning diagram eller hello tabellen objekt toosearch raw förekomster av dessa anrop.

![Beroende anropet instanser](./media/app-insights-asp-net-dependencies/dependency-call-instance.png)

**Fel antal** visas på hello **fel** bladet. Ett fel är alla returkoder som inte är i intervallet-hello 399 200, eller okänd.

> [!NOTE]
> **100% fel?** -Detta innebär förmodligen att du endast får partiella beroendedata. Du behöver för[ställa in beroende övervakning lämpliga tooyour plattform](#set-up-dependency-monitoring).
>
>

## <a name="ajax-calls"></a>AJAX-anrop
hello webbläsare bladet visar hello varaktighet och antalet misslyckade AJAX-anrop från [JavaScript i webbsidorna](app-insights-javascript.md). De visas som beroenden.

## <a name="diagnosis"></a>Diagnostisera långsam begäranden
Varje begäran händelse är associerad med hello beroendeanrop, undantag och andra händelser som spåras medan appen bearbetar hello begäran. Så om vissa begäranden felaktigt, kan du läsa mer om det är på grund av tooslow svar från ett beroende.

Låt oss gå igenom ett exempel på som.

### <a name="tracing-from-requests-toodependencies"></a>Spårning från begäranden toodependencies
Öppna hello prestanda bladet och titta på hello rutnätet begäranden:

![Lista med begäranden med medelvärden och antal](./media/app-insights-asp-net-dependencies/02-reqs.png)

hello upp en tar mycket lång tid. Låt oss se om vi ta reda på om hello tid det tar.

Klicka på de rad toosee enskilda begäran:

![Lista över begäran förekomster](./media/app-insights-asp-net-dependencies/03-instances.png)

Klicka på alla långvariga instans tooinspect den ytterligare och rulla ned toohello remote beroende anrop relaterade toothis begäran:

![Hitta anrop tooRemote beroenden, identifiera onormal varaktighet](./media/app-insights-asp-net-dependencies/04-dependencies.png)

Det ser ut som de flesta hello tid behandling denna begäran har använt i en anropet tooa lokal tjänst.

Välj den raden tooget mer information:

![Klicka dig igenom den fjärranslutna beroende tooidentify hello sällan](./media/app-insights-asp-net-dependencies/05-detail.png)

Ser ut som detta är där hello problem. Vi har precisa hello problemet, så nu vi bara behöver toofind reda på varför anropet tar så lång tid.

### <a name="request-timeline"></a>Tidslinje för begäran
I annat fall finns ingen beroendeanropet som är särskilt lång. Men genom att växla toohello tidslinjevyn kan vi se där hello fördröjning uppstod i vår intern bearbetning:

![Hitta anrop tooRemote beroenden, identifiera onormal varaktighet](./media/app-insights-asp-net-dependencies/04-1.png)

Det verkar toobe ett stort mellanrum efter hello första beroendeanropet så att vi ska titta på vår kod toosee varför som är.

### <a name="profile-your-live-site"></a>Webbplatsen live-profil

Ingen idé där hello tillfälle? Hej [Application Insights profiler](app-insights-profiler.md) spårningar HTTP anropar tooyour aktiva plats och visar vilka funktioner i koden tog hello längsta tid.

## <a name="failed-requests"></a>Misslyckade begäranden
Misslyckade begäranden kan även vara associerad med misslyckade anrop toodependencies. Vi kan igen och klicka på via tootrack ned hello problem.

![Klicka på hello diagram för misslyckade begäranden](./media/app-insights-asp-net-dependencies/06-fail.png)

Klicka dig igenom tooan förekomsten av en misslyckad begäran och titta på dess associerade händelserna.

![Klicka på typ av begäran, hello tooget tooa olika instansvyn för Hej samma instans, klickar du på den tooget undantagsinformation.](./media/app-insights-asp-net-dependencies/07-faildetail.png)

## <a name="analytics"></a>Analys
Du kan spåra beroenden i hello [Log Analytics-frågespråket](https://docs.loganalytics.io/). Här följer några exempel.

* Hitta alla misslyckade beroendeanrop:

```

    dependencies | where success != "True" | take 10
```

* Hitta AJAX-anrop:

```

    dependencies | where client_Type == "Browser" | take 10
```

* Hitta beroendeanrop som är associerade med begäranden:

```

    dependencies
    | where timestamp > ago(1d) and  client_Type != "Browser"
    | join (requests | where timestamp > ago(1d))
      on operation_Id  
```


* Hitta AJAX-anrop som är associerade med sidvisningar:

```

    dependencies
    | where timestamp > ago(1d) and  client_Type == "Browser"
    | join (browserTimings | where timestamp > ago(1d))
      on operation_Id
```



## <a name="custom-dependency-tracking"></a>Anpassade beroende spårning
hello standard beroende spårning modulen upptäcker automatiskt externa beroenden som databaser och REST API: er. Men du kanske vissa ytterligare komponenter toobe behandlas i hello samma sätt.

Du kan skriva kod som skickar beroendeinformation med hello samma [TrackDependency API](app-insights-api-custom-events-metrics.md#trackdependency) som används av hello standard moduler.

Du skapar din kod med en sammansättning som du inte skriva själv, du kan tiden för alla hello anrop tooit toofind vilka bidrag gör tooyour svar gånger. toohave dessa data visas i hello beroendediagrammen i Application Insights, skicka den via `TrackDependency`.

```C#

            var startTime = DateTime.UtcNow;
            var timer = System.Diagnostics.Stopwatch.StartNew();
            try
            {
                success = dependency.Call();
            }
            finally
            {
                timer.Stop();
                telemetry.TrackDependency("myDependency", "myCall", startTime, timer.Elapsed, success);
            }
```

Om du vill tooswitch av hello standard beroende spårning modul, tar du bort hello referens tooDependencyTrackingTelemetryModule i [ApplicationInsights.config](app-insights-configuration-with-applicationinsights-config.md).

## <a name="troubleshooting"></a>Felsökning
*Beroende lyckade flagga alltid visar som SANT eller FALSKT.*

*SQL-fråga som inte visas i sin helhet.*

* Uppgradera toohello senaste versionen av hello SDK. Om din version av .NET är mindre än 4.6:
  * IIS-värd: Installera [Application Insights Agent](app-insights-monitor-performance-live-website-now.md) på hello värdservrar.
  * Azure-webbapp: öppna Application Insights i Kontrollpanelen för hello web app och installera Application Insights.

## <a name="video"></a>Video

> [!VIDEO https://channel9.msdn.com/events/Connect/2016/112/player]

## <a name="next-steps"></a>Nästa steg
* [Undantag](app-insights-asp-net-exceptions.md)
* [Användar- och siddata](app-insights-javascript.md)
* [Tillgänglighet](app-insights-monitor-web-app-availability.md)
