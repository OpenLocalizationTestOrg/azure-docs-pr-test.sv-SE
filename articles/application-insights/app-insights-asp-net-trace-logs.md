---
title: "aaaExplore spårningsloggar .NET i Application Insights"
description: "Söka i loggar som genereras med spårning, NLog och Log4Net."
services: application-insights
documentationcenter: .net
author: CFreemanwa
manager: carmonm
ms.assetid: 0c2a084f-6e71-467b-a6aa-4ab222f17153
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 05/3/2017
ms.author: bwren
ms.openlocfilehash: 6bfcd9e5751c3656236d7eb2fc09321740171a70
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="explore-net-trace-logs-in-application-insights"></a>Utforska .NET spårningsloggar i Application Insights
Om du använder NLog, log4Net eller System.Diagnostics.Trace för diagnostikspårning i ASP.NET-program du har dina loggar som skickas för[Azure Application Insights][start], där du kan utforska och söka dem. Loggarna sammanfogas med hello andra telemetri som kommer från ditt program så att du kan identifiera hello-spårningar associerade med varje användarbegäran servicing och korrelera dem med andra händelser och undantag rapporter.

> [!NOTE]
> Behöver du hello modulen programlogg för avbildning? Det är ett nätverkskort som är användbar för 3 parts loggare, men om du inte redan använder NLog, log4Net eller System.Diagnostics.Trace, bör du bara anropa [Application Insights TrackTrace()](app-insights-api-custom-events-metrics.md#tracktrace) direkt.
>
>

## <a name="install-logging-on-your-app"></a>Installera loggning på din app
Installera din valda loggningsramverk i projektet. Det bör resultera i en post i app.config eller web.config.

Om du använder System.Diagnostics.Trace, behöver du tooadd en post tooweb.config:

```XML

    <configuration>
     <system.diagnostics>
       <trace autoflush="false" indentsize="4">
         <listeners>
           <add name="myListener"
             type="System.Diagnostics.TextWriterTraceListener"
             initializeData="TextWriterOutput.log" />
           <remove name="Default" />
         </listeners>
       </trace>
     </system.diagnostics>
   </configuration>
```
## <a name="configure-application-insights-toocollect-logs"></a>Konfigurera Application Insights toocollect loggar
**[Lägg till Application Insights tooyour projektet](app-insights-asp-net.md)**  om du inte har gjort det ännu. Logginsamlaren ett alternativet tooinclude hello visas.

Eller **konfigurera Application Insights** genom att högerklicka på projektet i Solution Explorer. Välj alternativet för hello för**konfigurera insamling**.

*Inget Application Insights-menyn eller loggfil insamlaren alternativ?* Försök [felsökning](#troubleshooting).

## <a name="manual-installation"></a>Manuell installation
Använd den här metoden om projekttypen inte stöds av hello Application Insights installer (till exempel en Windows desktop projekt).

1. Om du planerar toouse log4Net eller NLog måste du installera den i projektet.
2. Högerklicka på projektet i Solution Explorer och välj **hantera NuGet-paket**.
3. Sök efter ”Application Insights”
4. Välj lämplig hello-paket - en av:

   * Microsoft.ApplicationInsights.TraceListener (toocapture System.Diagnostics.Trace anrop)
   * Microsoft.ApplicationInsights.EventSourceListener (toocapture EventSource händelser)
   * Microsoft.ApplicationInsights.EtwListener (toocapture ETW-händelser)
   * Microsoft.ApplicationInsights.NLogTarget
   * Microsoft.ApplicationInsights.Log4NetAppender

Hej NuGet-paketet installerar hello nödvändiga sammansättningar och ändrar också web.config eller app.config.

## <a name="insert-diagnostic-log-calls"></a>Infoga diagnostiska loggen anrop
Om du använder System.Diagnostics.Trace, är en typisk anropet:

    System.Diagnostics.Trace.TraceWarning("Slow response - database01");

Om du föredrar log4net eller NLog:

    logger.Warn("Slow response - database01");

## <a name="using-eventsource-events"></a>Med hjälp av EventSource händelser
Du kan konfigurera [System.Diagnostics.Tracing.EventSource](https://msdn.microsoft.com/library/system.diagnostics.tracing.eventsource.aspx) händelser skickas toobe tooApplication insikter som spårningar. Installera först hello `Microsoft.ApplicationInsights.EventSourceListener` NuGet-paketet. Redigera `TelemetryModules` avsnitt i hello [ApplicationInsights.config](app-insights-configuration-with-applicationinsights-config.md) fil.

```xml
    <Add Type="Microsoft.ApplicationInsights.EventSourceListener.EventSourceTelemetryModule, Microsoft.ApplicationInsights.EventSourceListener">
      <Sources>
        <Add Name="MyCompany" Level="Verbose" />
      </Sources>
    </Add>
```

Du kan ange hello följande parametrar för varje källa:
 * `Name`Anger hello EventSource toocollect hello namn.
 * `Level`Anger hello loggning nivå toocollect. Kan vara något av `Critical`, `Error`, `Informational`, `LogAlways`, `Verbose`, `Warning`.
 * `Keywords`(Valfritt) anger nyckelord kombinationer toouse hello heltalsvärde.

## <a name="using-diagnosticsource-events"></a>Med hjälp av DiagnosticSource händelser
Du kan konfigurera [System.Diagnostics.DiagnosticSource](https://github.com/dotnet/corefx/blob/master/src/System.Diagnostics.DiagnosticSource/src/DiagnosticSourceUsersGuide.md) händelser skickas toobe tooApplication insikter som spårningar. Installera först hello [ `Microsoft.ApplicationInsights.DiagnosticSourceListener` ](https://www.nuget.org/packages/Microsoft.ApplicationInsights.DiagnosticSourceListener) NuGet-paketet. Redigera hello `TelemetryModules` avsnitt i hello [ApplicationInsights.config](app-insights-configuration-with-applicationinsights-config.md) fil.

```xml
    <Add Type="Microsoft.ApplicationInsights.DiagnsoticSourceListener.DiagnosticSourceTelemetryModule, Microsoft.ApplicationInsights.DiagnosticSourceListener">
      <Sources>
        <Add Name="MyDiagnosticSourceName" />
      </Sources>
    </Add>
```

För varje DiagnosticSource önskade tootrace, lägga till en post med hello `Name` -attributet inställt toohello namnet på din DiagnosticSource.

## <a name="using-etw-events"></a>Med hjälp av ETW-händelser
Du kan konfigurera ETW-händelser toobe skickas tooApplication insikter som spårningar. Installera först hello `Microsoft.ApplicationInsights.EtwCollector` NuGet-paketet. Redigera `TelemetryModules` avsnitt i hello [ApplicationInsights.config](app-insights-configuration-with-applicationinsights-config.md) fil.

> [!NOTE] 
> ETW-händelser kan endast samlas om hello processen värd hello SDK körs under en identitet som är medlem i ”användare” eller administratörer.

```xml
    <Add Type="Microsoft.ApplicationInsights.EtwCollector.EtwCollectorTelemetryModule, Microsoft.ApplicationInsights.EtwCollector">
      <Sources>
        <Add ProviderName="MyCompanyEventSourceName" Level="Verbose" />
      </Sources>
    </Add>
```

Du kan ange hello följande parametrar för varje källa:
 * `ProviderName`är hello ETW-provider toocollect hello namn.
 * `ProviderGuid`Anger hello GUID för hello ETW-provider toocollect kan användas i stället för `ProviderName`.
 * `Level`Anger hello loggning nivå toocollect. Kan vara något av `Critical`, `Error`, `Informational`, `LogAlways`, `Verbose`, `Warning`.
 * `Keywords`(Valfritt) anger hello nyckelordet kombinationer toouse heltalsvärde.

## <a name="using-hello-trace-api-directly"></a>Med hjälp av hello Trace API direkt
Du kan anropa hello Application Insights trace API direkt. hello loggning nätverkskort använder detta API.

Exempel:

    var telemetry = new Microsoft.ApplicationInsights.TelemetryClient();
    telemetry.TrackTrace("Slow response - database01");

En fördel med TrackTrace är att du kan publicera relativt lång data i hello-meddelande. Du kan till exempel kodar det postdata.

Dessutom kan du lägga till ett allvarlighetsgrad nivå tooyour meddelande. Och precis som andra telemetri, du kan lägga till värden för egenskaper som du kan använda toohelp filter eller söka efter olika uppsättningar av spår. Exempel:

    var telemetry = new Microsoft.ApplicationInsights.TelemetryClient();
    telemetry.TrackTrace("Slow database response",
                   SeverityLevel.Warning,
                   new Dictionary<string,string> { {"database", db.ID} });

Detta gör att du, i [Sök][diagnostic], tooeasily filtrera bort alla hälsningsmeddelande för en viss allvarlighetsgrad som rör tooa viss databas.

## <a name="explore-your-logs"></a>Utforska dina loggar
Kör appen i felsökningsläge eller distribuera den live.

I bladet för din app översikt i [hello Application Insights-portalen][portal], Välj [Sök][diagnostic].

![Välj sökning i Application Insights](./media/app-insights-asp-net-trace-logs/020-diagnostic-search.png)

![Söka](./media/app-insights-asp-net-trace-logs/10-diagnostics.png)

Du kan till exempel:

* Filtrera efter loggspårningar eller efter artiklar med specifika egenskaper
* Kontrollera om ett specifikt objekt i detalj.
* Hitta andra telemetri om toohello samma användarbegäran (det vill säga med hello samma åtgärds-ID)
* Spara hello konfigurationen av den här sidan som en favorit

> [!NOTE]
> **Sampling.** Om ditt program skickar stora mängder data och du använder hello Application Insights SDK för ASP.NET version 2.0.0-beta3 eller senare, hello anpassningsbar provtagning funktionen fungerar och skicka endast en del av din telemetri. [Läs mer om sampling.](app-insights-sampling.md)
>
>

## <a name="next-steps"></a>Nästa steg
[Diagnostisera fel och undantag i ASP.NET][exceptions]

[Mer information om sökningen][diagnostic].

## <a name="troubleshooting"></a>Felsökning
### <a name="how-do-i-do-this-for-java"></a>Hur gör jag för Java?
Använd hello [Java loggen kort](app-insights-java-trace-logs.md).

### <a name="theres-no-application-insights-option-on-hello-project-context-menu"></a>Det finns inget Application Insights alternativ på snabbmenyn för hello-projekt
* Kontrollera att Application Insights tools är installerat på den här datorn för utveckling. Leta efter Application Insights Tools i Verktyg för Visual Studio-menyn, tillägg och uppdateringar. Om det inte finns i hello installerad fliken Öppna hello Online flik och installera den.
* Det kan vara en typ av projekt som inte stöds av Application Insights verktyg. Använd [manuell installation](#manual-installation).

### <a name="no-log-adapter-option-in-hello-configuration-tool"></a>Inget loggen nätverkskort alternativ i hello configuration tool
* Du måste först tooinstall hello loggningsramverk.
* Om du använder System.Diagnostics.Trace, se till att du [konfigurerats i `web.config` ](https://msdn.microsoft.com/library/system.diagnostics.eventlogtracelistener.aspx).
* Har du fått hello senaste versionen av Application Insights? I Visual Studio **verktyg** -menyn, Välj **tillägg och uppdateringar**, och öppna hello **uppdateringar** fliken. Om utvecklare Analytics verktyg det klickar du på tooupdate den.

### <a name="emptykey"></a>Ett felmeddelande ”Instrumentation nyckeln kan inte vara tomt”
Ser ut som om du har installerat hello loggning kortet Nuget-paket utan att installera Application Insights.

I Solution Explorer högerklickar du på `ApplicationInsights.config` och välj **uppdatering Application Insights**. Du får en dialogruta där du toosign i tooAzure och skapa en Application Insights-resurs eller återanvända en befintlig. Som ska åtgärdas.

### <a name="i-can-see-traces-in-diagnostic-search-but-not-hello-other-events"></a>Jag kan finns i spåren i diagnostiska sökning, men inte hello andra händelser
Det kan ta en stund för alla hello-händelser och begäranden tooget via hello pipeline.

### <a name="limits"></a>Hur mycket data finns kvar?
Flera faktorer påverka hello mängden data som behålls. Se hello [gränser](app-insights-api-custom-events-metrics.md#limits) på hello kunden händelse mått sidan för mer information. 

### <a name="im-not-seeing-some-of-hello-log-entries-that-i-expect"></a>Jag ser inte några hello loggposter som förväntat
Om ditt program skickar stora mängder data och du använder hello Application Insights SDK för ASP.NET version 2.0.0-beta3 eller senare, hello anpassningsbar provtagning funktionen fungerar och skicka endast en del av din telemetri. [Läs mer om sampling.](app-insights-sampling.md)

## <a name="add"></a>Nästa steg
* [Ställ in tillgänglighet och svarstider tester][availability]
* [Felsökning][qna]

<!--Link references-->

[availability]: app-insights-monitor-web-app-availability.md
[diagnostic]: app-insights-diagnostic-search.md
[exceptions]: app-insights-asp-net-exceptions.md
[portal]: https://portal.azure.com/
[qna]: app-insights-troubleshoot-faq.md
[start]: app-insights-overview.md
