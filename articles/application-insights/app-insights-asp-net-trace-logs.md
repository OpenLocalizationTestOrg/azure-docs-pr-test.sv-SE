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
# <a name="explore-net-trace-logs-in-application-insights"></a><span data-ttu-id="973de-103">Utforska .NET spårningsloggar i Application Insights</span><span class="sxs-lookup"><span data-stu-id="973de-103">Explore .NET trace logs in Application Insights</span></span>
<span data-ttu-id="973de-104">Om du använder NLog, log4Net eller System.Diagnostics.Trace för diagnostikspårning i ASP.NET-program du har dina loggar som skickas för[Azure Application Insights][start], där du kan utforska och söka dem.</span><span class="sxs-lookup"><span data-stu-id="973de-104">If you use NLog, log4Net or System.Diagnostics.Trace for diagnostic tracing in your ASP.NET application, you can have your logs sent too[Azure Application Insights][start], where you can explore and search them.</span></span> <span data-ttu-id="973de-105">Loggarna sammanfogas med hello andra telemetri som kommer från ditt program så att du kan identifiera hello-spårningar associerade med varje användarbegäran servicing och korrelera dem med andra händelser och undantag rapporter.</span><span class="sxs-lookup"><span data-stu-id="973de-105">Your logs will be merged with hello other telemetry coming from your application, so that you can identify hello traces associated with servicing each user request, and correlate them with other events and exception reports.</span></span>

> [!NOTE]
> <span data-ttu-id="973de-106">Behöver du hello modulen programlogg för avbildning?</span><span class="sxs-lookup"><span data-stu-id="973de-106">Do you need hello log capture module?</span></span> <span data-ttu-id="973de-107">Det är ett nätverkskort som är användbar för 3 parts loggare, men om du inte redan använder NLog, log4Net eller System.Diagnostics.Trace, bör du bara anropa [Application Insights TrackTrace()](app-insights-api-custom-events-metrics.md#tracktrace) direkt.</span><span class="sxs-lookup"><span data-stu-id="973de-107">It's a useful adapter for 3rd-party loggers, but if you aren't already using NLog, log4Net or System.Diagnostics.Trace, consider just calling [Application Insights TrackTrace()](app-insights-api-custom-events-metrics.md#tracktrace) directly.</span></span>
>
>

## <a name="install-logging-on-your-app"></a><span data-ttu-id="973de-108">Installera loggning på din app</span><span class="sxs-lookup"><span data-stu-id="973de-108">Install logging on your app</span></span>
<span data-ttu-id="973de-109">Installera din valda loggningsramverk i projektet.</span><span class="sxs-lookup"><span data-stu-id="973de-109">Install your chosen logging framework in your project.</span></span> <span data-ttu-id="973de-110">Det bör resultera i en post i app.config eller web.config.</span><span class="sxs-lookup"><span data-stu-id="973de-110">This should result in an entry in app.config or web.config.</span></span>

<span data-ttu-id="973de-111">Om du använder System.Diagnostics.Trace, behöver du tooadd en post tooweb.config:</span><span class="sxs-lookup"><span data-stu-id="973de-111">If you're using System.Diagnostics.Trace, you need tooadd an entry tooweb.config:</span></span>

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
## <a name="configure-application-insights-toocollect-logs"></a><span data-ttu-id="973de-112">Konfigurera Application Insights toocollect loggar</span><span class="sxs-lookup"><span data-stu-id="973de-112">Configure Application Insights toocollect logs</span></span>
<span data-ttu-id="973de-113">**[Lägg till Application Insights tooyour projektet](app-insights-asp-net.md)**  om du inte har gjort det ännu.</span><span class="sxs-lookup"><span data-stu-id="973de-113">**[Add Application Insights tooyour project](app-insights-asp-net.md)** if you haven't done that yet.</span></span> <span data-ttu-id="973de-114">Logginsamlaren ett alternativet tooinclude hello visas.</span><span class="sxs-lookup"><span data-stu-id="973de-114">You'll see an option tooinclude hello log collector.</span></span>

<span data-ttu-id="973de-115">Eller **konfigurera Application Insights** genom att högerklicka på projektet i Solution Explorer.</span><span class="sxs-lookup"><span data-stu-id="973de-115">Or **Configure Application Insights** by right-clicking your project in Solution Explorer.</span></span> <span data-ttu-id="973de-116">Välj alternativet för hello för**konfigurera insamling**.</span><span class="sxs-lookup"><span data-stu-id="973de-116">Select hello option too**Configure trace collection**.</span></span>

<span data-ttu-id="973de-117">*Inget Application Insights-menyn eller loggfil insamlaren alternativ?*</span><span class="sxs-lookup"><span data-stu-id="973de-117">*No Application Insights menu or log collector option?*</span></span> <span data-ttu-id="973de-118">Försök [felsökning](#troubleshooting).</span><span class="sxs-lookup"><span data-stu-id="973de-118">Try [Troubleshooting](#troubleshooting).</span></span>

## <a name="manual-installation"></a><span data-ttu-id="973de-119">Manuell installation</span><span class="sxs-lookup"><span data-stu-id="973de-119">Manual installation</span></span>
<span data-ttu-id="973de-120">Använd den här metoden om projekttypen inte stöds av hello Application Insights installer (till exempel en Windows desktop projekt).</span><span class="sxs-lookup"><span data-stu-id="973de-120">Use this method if your project type isn't supported by hello Application Insights installer (for example a Windows desktop project).</span></span>

1. <span data-ttu-id="973de-121">Om du planerar toouse log4Net eller NLog måste du installera den i projektet.</span><span class="sxs-lookup"><span data-stu-id="973de-121">If you plan toouse log4Net or NLog, install it in your project.</span></span>
2. <span data-ttu-id="973de-122">Högerklicka på projektet i Solution Explorer och välj **hantera NuGet-paket**.</span><span class="sxs-lookup"><span data-stu-id="973de-122">In Solution Explorer, right-click your project and choose **Manage NuGet Packages**.</span></span>
3. <span data-ttu-id="973de-123">Sök efter ”Application Insights”</span><span class="sxs-lookup"><span data-stu-id="973de-123">Search for "Application Insights"</span></span>
4. <span data-ttu-id="973de-124">Välj lämplig hello-paket - en av:</span><span class="sxs-lookup"><span data-stu-id="973de-124">Select hello appropriate package - one of:</span></span>

   * <span data-ttu-id="973de-125">Microsoft.ApplicationInsights.TraceListener (toocapture System.Diagnostics.Trace anrop)</span><span class="sxs-lookup"><span data-stu-id="973de-125">Microsoft.ApplicationInsights.TraceListener (toocapture System.Diagnostics.Trace calls)</span></span>
   * <span data-ttu-id="973de-126">Microsoft.ApplicationInsights.EventSourceListener (toocapture EventSource händelser)</span><span class="sxs-lookup"><span data-stu-id="973de-126">Microsoft.ApplicationInsights.EventSourceListener (toocapture EventSource events)</span></span>
   * <span data-ttu-id="973de-127">Microsoft.ApplicationInsights.EtwListener (toocapture ETW-händelser)</span><span class="sxs-lookup"><span data-stu-id="973de-127">Microsoft.ApplicationInsights.EtwListener (toocapture ETW events)</span></span>
   * <span data-ttu-id="973de-128">Microsoft.ApplicationInsights.NLogTarget</span><span class="sxs-lookup"><span data-stu-id="973de-128">Microsoft.ApplicationInsights.NLogTarget</span></span>
   * <span data-ttu-id="973de-129">Microsoft.ApplicationInsights.Log4NetAppender</span><span class="sxs-lookup"><span data-stu-id="973de-129">Microsoft.ApplicationInsights.Log4NetAppender</span></span>

<span data-ttu-id="973de-130">Hej NuGet-paketet installerar hello nödvändiga sammansättningar och ändrar också web.config eller app.config.</span><span class="sxs-lookup"><span data-stu-id="973de-130">hello NuGet package installs hello necessary assemblies, and also modifies web.config or app.config.</span></span>

## <a name="insert-diagnostic-log-calls"></a><span data-ttu-id="973de-131">Infoga diagnostiska loggen anrop</span><span class="sxs-lookup"><span data-stu-id="973de-131">Insert diagnostic log calls</span></span>
<span data-ttu-id="973de-132">Om du använder System.Diagnostics.Trace, är en typisk anropet:</span><span class="sxs-lookup"><span data-stu-id="973de-132">If you use System.Diagnostics.Trace, a typical call would be:</span></span>

    System.Diagnostics.Trace.TraceWarning("Slow response - database01");

<span data-ttu-id="973de-133">Om du föredrar log4net eller NLog:</span><span class="sxs-lookup"><span data-stu-id="973de-133">If you prefer log4net or NLog:</span></span>

    logger.Warn("Slow response - database01");

## <a name="using-eventsource-events"></a><span data-ttu-id="973de-134">Med hjälp av EventSource händelser</span><span class="sxs-lookup"><span data-stu-id="973de-134">Using EventSource events</span></span>
<span data-ttu-id="973de-135">Du kan konfigurera [System.Diagnostics.Tracing.EventSource](https://msdn.microsoft.com/library/system.diagnostics.tracing.eventsource.aspx) händelser skickas toobe tooApplication insikter som spårningar.</span><span class="sxs-lookup"><span data-stu-id="973de-135">You can configure [System.Diagnostics.Tracing.EventSource](https://msdn.microsoft.com/library/system.diagnostics.tracing.eventsource.aspx) events toobe sent tooApplication Insights as traces.</span></span> <span data-ttu-id="973de-136">Installera först hello `Microsoft.ApplicationInsights.EventSourceListener` NuGet-paketet.</span><span class="sxs-lookup"><span data-stu-id="973de-136">First, install hello `Microsoft.ApplicationInsights.EventSourceListener` NuGet package.</span></span> <span data-ttu-id="973de-137">Redigera `TelemetryModules` avsnitt i hello [ApplicationInsights.config](app-insights-configuration-with-applicationinsights-config.md) fil.</span><span class="sxs-lookup"><span data-stu-id="973de-137">Then edit `TelemetryModules` section of hello [ApplicationInsights.config](app-insights-configuration-with-applicationinsights-config.md) file.</span></span>

```xml
    <Add Type="Microsoft.ApplicationInsights.EventSourceListener.EventSourceTelemetryModule, Microsoft.ApplicationInsights.EventSourceListener">
      <Sources>
        <Add Name="MyCompany" Level="Verbose" />
      </Sources>
    </Add>
```

<span data-ttu-id="973de-138">Du kan ange hello följande parametrar för varje källa:</span><span class="sxs-lookup"><span data-stu-id="973de-138">For each source, you can set hello following parameters:</span></span>
 * <span data-ttu-id="973de-139">`Name`Anger hello EventSource toocollect hello namn.</span><span class="sxs-lookup"><span data-stu-id="973de-139">`Name` specifies hello name of hello EventSource toocollect.</span></span>
 * <span data-ttu-id="973de-140">`Level`Anger hello loggning nivå toocollect.</span><span class="sxs-lookup"><span data-stu-id="973de-140">`Level` specifies hello logging level toocollect.</span></span> <span data-ttu-id="973de-141">Kan vara något av `Critical`, `Error`, `Informational`, `LogAlways`, `Verbose`, `Warning`.</span><span class="sxs-lookup"><span data-stu-id="973de-141">Can be one of `Critical`, `Error`, `Informational`, `LogAlways`, `Verbose`, `Warning`.</span></span>
 * <span data-ttu-id="973de-142">`Keywords`(Valfritt) anger nyckelord kombinationer toouse hello heltalsvärde.</span><span class="sxs-lookup"><span data-stu-id="973de-142">`Keywords` (Optional) specifies hello integer value of keywords combinations toouse.</span></span>

## <a name="using-diagnosticsource-events"></a><span data-ttu-id="973de-143">Med hjälp av DiagnosticSource händelser</span><span class="sxs-lookup"><span data-stu-id="973de-143">Using DiagnosticSource events</span></span>
<span data-ttu-id="973de-144">Du kan konfigurera [System.Diagnostics.DiagnosticSource](https://github.com/dotnet/corefx/blob/master/src/System.Diagnostics.DiagnosticSource/src/DiagnosticSourceUsersGuide.md) händelser skickas toobe tooApplication insikter som spårningar.</span><span class="sxs-lookup"><span data-stu-id="973de-144">You can configure [System.Diagnostics.DiagnosticSource](https://github.com/dotnet/corefx/blob/master/src/System.Diagnostics.DiagnosticSource/src/DiagnosticSourceUsersGuide.md) events toobe sent tooApplication Insights as traces.</span></span> <span data-ttu-id="973de-145">Installera först hello [ `Microsoft.ApplicationInsights.DiagnosticSourceListener` ](https://www.nuget.org/packages/Microsoft.ApplicationInsights.DiagnosticSourceListener) NuGet-paketet.</span><span class="sxs-lookup"><span data-stu-id="973de-145">First, install hello [`Microsoft.ApplicationInsights.DiagnosticSourceListener`](https://www.nuget.org/packages/Microsoft.ApplicationInsights.DiagnosticSourceListener) NuGet package.</span></span> <span data-ttu-id="973de-146">Redigera hello `TelemetryModules` avsnitt i hello [ApplicationInsights.config](app-insights-configuration-with-applicationinsights-config.md) fil.</span><span class="sxs-lookup"><span data-stu-id="973de-146">Then edit hello `TelemetryModules` section of hello [ApplicationInsights.config](app-insights-configuration-with-applicationinsights-config.md) file.</span></span>

```xml
    <Add Type="Microsoft.ApplicationInsights.DiagnsoticSourceListener.DiagnosticSourceTelemetryModule, Microsoft.ApplicationInsights.DiagnosticSourceListener">
      <Sources>
        <Add Name="MyDiagnosticSourceName" />
      </Sources>
    </Add>
```

<span data-ttu-id="973de-147">För varje DiagnosticSource önskade tootrace, lägga till en post med hello `Name` -attributet inställt toohello namnet på din DiagnosticSource.</span><span class="sxs-lookup"><span data-stu-id="973de-147">For each DiagnosticSource you want tootrace, add an entry with hello `Name` attribute  set toohello name of your DiagnosticSource.</span></span>

## <a name="using-etw-events"></a><span data-ttu-id="973de-148">Med hjälp av ETW-händelser</span><span class="sxs-lookup"><span data-stu-id="973de-148">Using ETW events</span></span>
<span data-ttu-id="973de-149">Du kan konfigurera ETW-händelser toobe skickas tooApplication insikter som spårningar.</span><span class="sxs-lookup"><span data-stu-id="973de-149">You can configure ETW events toobe sent tooApplication Insights as traces.</span></span> <span data-ttu-id="973de-150">Installera först hello `Microsoft.ApplicationInsights.EtwCollector` NuGet-paketet.</span><span class="sxs-lookup"><span data-stu-id="973de-150">First, install hello `Microsoft.ApplicationInsights.EtwCollector` NuGet package.</span></span> <span data-ttu-id="973de-151">Redigera `TelemetryModules` avsnitt i hello [ApplicationInsights.config](app-insights-configuration-with-applicationinsights-config.md) fil.</span><span class="sxs-lookup"><span data-stu-id="973de-151">Then edit `TelemetryModules` section of hello [ApplicationInsights.config](app-insights-configuration-with-applicationinsights-config.md) file.</span></span>

> [!NOTE] 
> <span data-ttu-id="973de-152">ETW-händelser kan endast samlas om hello processen värd hello SDK körs under en identitet som är medlem i ”användare” eller administratörer.</span><span class="sxs-lookup"><span data-stu-id="973de-152">ETW events can only be collected if hello process hosting hello SDK is running under an identity that is a member of "Performance Log Users" or Administrators.</span></span>

```xml
    <Add Type="Microsoft.ApplicationInsights.EtwCollector.EtwCollectorTelemetryModule, Microsoft.ApplicationInsights.EtwCollector">
      <Sources>
        <Add ProviderName="MyCompanyEventSourceName" Level="Verbose" />
      </Sources>
    </Add>
```

<span data-ttu-id="973de-153">Du kan ange hello följande parametrar för varje källa:</span><span class="sxs-lookup"><span data-stu-id="973de-153">For each source, you can set hello following parameters:</span></span>
 * <span data-ttu-id="973de-154">`ProviderName`är hello ETW-provider toocollect hello namn.</span><span class="sxs-lookup"><span data-stu-id="973de-154">`ProviderName` is hello name of hello ETW provider toocollect.</span></span>
 * <span data-ttu-id="973de-155">`ProviderGuid`Anger hello GUID för hello ETW-provider toocollect kan användas i stället för `ProviderName`.</span><span class="sxs-lookup"><span data-stu-id="973de-155">`ProviderGuid` specifies hello GUID of hello ETW provider toocollect, can be used instead of `ProviderName`.</span></span>
 * <span data-ttu-id="973de-156">`Level`Anger hello loggning nivå toocollect.</span><span class="sxs-lookup"><span data-stu-id="973de-156">`Level` sets hello logging level toocollect.</span></span> <span data-ttu-id="973de-157">Kan vara något av `Critical`, `Error`, `Informational`, `LogAlways`, `Verbose`, `Warning`.</span><span class="sxs-lookup"><span data-stu-id="973de-157">Can be one of `Critical`, `Error`, `Informational`, `LogAlways`, `Verbose`, `Warning`.</span></span>
 * <span data-ttu-id="973de-158">`Keywords`(Valfritt) anger hello nyckelordet kombinationer toouse heltalsvärde.</span><span class="sxs-lookup"><span data-stu-id="973de-158">`Keywords` (Optional) sets hello integer value of keyword combinations toouse.</span></span>

## <a name="using-hello-trace-api-directly"></a><span data-ttu-id="973de-159">Med hjälp av hello Trace API direkt</span><span class="sxs-lookup"><span data-stu-id="973de-159">Using hello Trace API directly</span></span>
<span data-ttu-id="973de-160">Du kan anropa hello Application Insights trace API direkt.</span><span class="sxs-lookup"><span data-stu-id="973de-160">You can call hello Application Insights trace API directly.</span></span> <span data-ttu-id="973de-161">hello loggning nätverkskort använder detta API.</span><span class="sxs-lookup"><span data-stu-id="973de-161">hello logging adapters use this API.</span></span>

<span data-ttu-id="973de-162">Exempel:</span><span class="sxs-lookup"><span data-stu-id="973de-162">For example:</span></span>

    var telemetry = new Microsoft.ApplicationInsights.TelemetryClient();
    telemetry.TrackTrace("Slow response - database01");

<span data-ttu-id="973de-163">En fördel med TrackTrace är att du kan publicera relativt lång data i hello-meddelande.</span><span class="sxs-lookup"><span data-stu-id="973de-163">An advantage of TrackTrace is that you can put relatively long data in hello message.</span></span> <span data-ttu-id="973de-164">Du kan till exempel kodar det postdata.</span><span class="sxs-lookup"><span data-stu-id="973de-164">For example, you could encode POST data there.</span></span>

<span data-ttu-id="973de-165">Dessutom kan du lägga till ett allvarlighetsgrad nivå tooyour meddelande.</span><span class="sxs-lookup"><span data-stu-id="973de-165">In addition, you can add a severity level tooyour message.</span></span> <span data-ttu-id="973de-166">Och precis som andra telemetri, du kan lägga till värden för egenskaper som du kan använda toohelp filter eller söka efter olika uppsättningar av spår.</span><span class="sxs-lookup"><span data-stu-id="973de-166">And, like other telemetry, you can add property values that you can use toohelp filter or search for different sets of traces.</span></span> <span data-ttu-id="973de-167">Exempel:</span><span class="sxs-lookup"><span data-stu-id="973de-167">For example:</span></span>

    var telemetry = new Microsoft.ApplicationInsights.TelemetryClient();
    telemetry.TrackTrace("Slow database response",
                   SeverityLevel.Warning,
                   new Dictionary<string,string> { {"database", db.ID} });

<span data-ttu-id="973de-168">Detta gör att du, i [Sök][diagnostic], tooeasily filtrera bort alla hälsningsmeddelande för en viss allvarlighetsgrad som rör tooa viss databas.</span><span class="sxs-lookup"><span data-stu-id="973de-168">This would enable you, in [Search][diagnostic], tooeasily filter out all hello messages of a particular severity level relating tooa particular database.</span></span>

## <a name="explore-your-logs"></a><span data-ttu-id="973de-169">Utforska dina loggar</span><span class="sxs-lookup"><span data-stu-id="973de-169">Explore your logs</span></span>
<span data-ttu-id="973de-170">Kör appen i felsökningsläge eller distribuera den live.</span><span class="sxs-lookup"><span data-stu-id="973de-170">Run your app, either in debug mode or deploy it live.</span></span>

<span data-ttu-id="973de-171">I bladet för din app översikt i [hello Application Insights-portalen][portal], Välj [Sök][diagnostic].</span><span class="sxs-lookup"><span data-stu-id="973de-171">In your app's overview blade in [hello Application Insights portal][portal], choose [Search][diagnostic].</span></span>

![Välj sökning i Application Insights](./media/app-insights-asp-net-trace-logs/020-diagnostic-search.png)

![Söka](./media/app-insights-asp-net-trace-logs/10-diagnostics.png)

<span data-ttu-id="973de-174">Du kan till exempel:</span><span class="sxs-lookup"><span data-stu-id="973de-174">You can, for example:</span></span>

* <span data-ttu-id="973de-175">Filtrera efter loggspårningar eller efter artiklar med specifika egenskaper</span><span class="sxs-lookup"><span data-stu-id="973de-175">Filter on log traces, or on items with specific properties</span></span>
* <span data-ttu-id="973de-176">Kontrollera om ett specifikt objekt i detalj.</span><span class="sxs-lookup"><span data-stu-id="973de-176">Inspect a specific item in detail.</span></span>
* <span data-ttu-id="973de-177">Hitta andra telemetri om toohello samma användarbegäran (det vill säga med hello samma åtgärds-ID)</span><span class="sxs-lookup"><span data-stu-id="973de-177">Find other telemetry relating toohello same user request (that is, with hello same OperationId)</span></span>
* <span data-ttu-id="973de-178">Spara hello konfigurationen av den här sidan som en favorit</span><span class="sxs-lookup"><span data-stu-id="973de-178">Save hello configuration of this page as a Favorite</span></span>

> [!NOTE]
> <span data-ttu-id="973de-179">**Sampling.**</span><span class="sxs-lookup"><span data-stu-id="973de-179">**Sampling.**</span></span> <span data-ttu-id="973de-180">Om ditt program skickar stora mängder data och du använder hello Application Insights SDK för ASP.NET version 2.0.0-beta3 eller senare, hello anpassningsbar provtagning funktionen fungerar och skicka endast en del av din telemetri.</span><span class="sxs-lookup"><span data-stu-id="973de-180">If your application sends a lot of data and you are using hello Application Insights SDK for ASP.NET version 2.0.0-beta3 or later, hello adaptive sampling feature may operate and send only a percentage of your telemetry.</span></span> [<span data-ttu-id="973de-181">Läs mer om sampling.</span><span class="sxs-lookup"><span data-stu-id="973de-181">Learn more about sampling.</span></span>](app-insights-sampling.md)
>
>

## <a name="next-steps"></a><span data-ttu-id="973de-182">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="973de-182">Next steps</span></span>
<span data-ttu-id="973de-183">[Diagnostisera fel och undantag i ASP.NET][exceptions]</span><span class="sxs-lookup"><span data-stu-id="973de-183">[Diagnose failures and exceptions in ASP.NET][exceptions]</span></span>

<span data-ttu-id="973de-184">[Mer information om sökningen][diagnostic].</span><span class="sxs-lookup"><span data-stu-id="973de-184">[Learn more about Search][diagnostic].</span></span>

## <a name="troubleshooting"></a><span data-ttu-id="973de-185">Felsökning</span><span class="sxs-lookup"><span data-stu-id="973de-185">Troubleshooting</span></span>
### <a name="how-do-i-do-this-for-java"></a><span data-ttu-id="973de-186">Hur gör jag för Java?</span><span class="sxs-lookup"><span data-stu-id="973de-186">How do I do this for Java?</span></span>
<span data-ttu-id="973de-187">Använd hello [Java loggen kort](app-insights-java-trace-logs.md).</span><span class="sxs-lookup"><span data-stu-id="973de-187">Use hello [Java log adapters](app-insights-java-trace-logs.md).</span></span>

### <a name="theres-no-application-insights-option-on-hello-project-context-menu"></a><span data-ttu-id="973de-188">Det finns inget Application Insights alternativ på snabbmenyn för hello-projekt</span><span class="sxs-lookup"><span data-stu-id="973de-188">There's no Application Insights option on hello project context menu</span></span>
* <span data-ttu-id="973de-189">Kontrollera att Application Insights tools är installerat på den här datorn för utveckling.</span><span class="sxs-lookup"><span data-stu-id="973de-189">Check Application Insights tools is installed on this development machine.</span></span> <span data-ttu-id="973de-190">Leta efter Application Insights Tools i Verktyg för Visual Studio-menyn, tillägg och uppdateringar.</span><span class="sxs-lookup"><span data-stu-id="973de-190">In Visual Studio menu Tools, Extensions and Updates, look for Application Insights Tools.</span></span> <span data-ttu-id="973de-191">Om det inte finns i hello installerad fliken Öppna hello Online flik och installera den.</span><span class="sxs-lookup"><span data-stu-id="973de-191">If it isn't in hello Installed tab, open hello Online tab and install it.</span></span>
* <span data-ttu-id="973de-192">Det kan vara en typ av projekt som inte stöds av Application Insights verktyg.</span><span class="sxs-lookup"><span data-stu-id="973de-192">This might be a type of project not supported by Application Insights tools.</span></span> <span data-ttu-id="973de-193">Använd [manuell installation](#manual-installation).</span><span class="sxs-lookup"><span data-stu-id="973de-193">Use [manual installation](#manual-installation).</span></span>

### <a name="no-log-adapter-option-in-hello-configuration-tool"></a><span data-ttu-id="973de-194">Inget loggen nätverkskort alternativ i hello configuration tool</span><span class="sxs-lookup"><span data-stu-id="973de-194">No log adapter option in hello configuration tool</span></span>
* <span data-ttu-id="973de-195">Du måste först tooinstall hello loggningsramverk.</span><span class="sxs-lookup"><span data-stu-id="973de-195">You need tooinstall hello logging framework first.</span></span>
* <span data-ttu-id="973de-196">Om du använder System.Diagnostics.Trace, se till att du [konfigurerats i `web.config` ](https://msdn.microsoft.com/library/system.diagnostics.eventlogtracelistener.aspx).</span><span class="sxs-lookup"><span data-stu-id="973de-196">If you're using System.Diagnostics.Trace, make sure you [configured it in `web.config`](https://msdn.microsoft.com/library/system.diagnostics.eventlogtracelistener.aspx).</span></span>
* <span data-ttu-id="973de-197">Har du fått hello senaste versionen av Application Insights?</span><span class="sxs-lookup"><span data-stu-id="973de-197">Have you got hello latest version of Application Insights?</span></span> <span data-ttu-id="973de-198">I Visual Studio **verktyg** -menyn, Välj **tillägg och uppdateringar**, och öppna hello **uppdateringar** fliken. Om utvecklare Analytics verktyg det klickar du på tooupdate den.</span><span class="sxs-lookup"><span data-stu-id="973de-198">In Visual Studio **Tools** menu, choose **Extensions and Updates**, and open hello **Updates** tab. If Developer Analytics tools is there, click tooupdate it.</span></span>

### <span data-ttu-id="973de-199"><a name="emptykey"></a>Ett felmeddelande ”Instrumentation nyckeln kan inte vara tomt”</span><span class="sxs-lookup"><span data-stu-id="973de-199"><a name="emptykey"></a>I get an error "Instrumentation key cannot be empty"</span></span>
<span data-ttu-id="973de-200">Ser ut som om du har installerat hello loggning kortet Nuget-paket utan att installera Application Insights.</span><span class="sxs-lookup"><span data-stu-id="973de-200">Looks like you installed hello logging adapter Nuget package without installing Application Insights.</span></span>

<span data-ttu-id="973de-201">I Solution Explorer högerklickar du på `ApplicationInsights.config` och välj **uppdatering Application Insights**.</span><span class="sxs-lookup"><span data-stu-id="973de-201">In Solution Explorer, right-click `ApplicationInsights.config` and choose **Update Application Insights**.</span></span> <span data-ttu-id="973de-202">Du får en dialogruta där du toosign i tooAzure och skapa en Application Insights-resurs eller återanvända en befintlig.</span><span class="sxs-lookup"><span data-stu-id="973de-202">You'll get a dialog that invites you toosign in tooAzure and either create an Application Insights resource, or re-use an existing one.</span></span> <span data-ttu-id="973de-203">Som ska åtgärdas.</span><span class="sxs-lookup"><span data-stu-id="973de-203">That should fix it.</span></span>

### <a name="i-can-see-traces-in-diagnostic-search-but-not-hello-other-events"></a><span data-ttu-id="973de-204">Jag kan finns i spåren i diagnostiska sökning, men inte hello andra händelser</span><span class="sxs-lookup"><span data-stu-id="973de-204">I can see traces in diagnostic search, but not hello other events</span></span>
<span data-ttu-id="973de-205">Det kan ta en stund för alla hello-händelser och begäranden tooget via hello pipeline.</span><span class="sxs-lookup"><span data-stu-id="973de-205">It can sometimes take a while for all hello events and requests tooget through hello pipeline.</span></span>

### <span data-ttu-id="973de-206"><a name="limits"></a>Hur mycket data finns kvar?</span><span class="sxs-lookup"><span data-stu-id="973de-206"><a name="limits"></a>How much data is retained?</span></span>
<span data-ttu-id="973de-207">Flera faktorer påverka hello mängden data som behålls.</span><span class="sxs-lookup"><span data-stu-id="973de-207">Several factors impact hello amount of data retained.</span></span> <span data-ttu-id="973de-208">Se hello [gränser](app-insights-api-custom-events-metrics.md#limits) på hello kunden händelse mått sidan för mer information.</span><span class="sxs-lookup"><span data-stu-id="973de-208">See hello [limits](app-insights-api-custom-events-metrics.md#limits) section of hello customer event metrics page for more information.</span></span> 

### <a name="im-not-seeing-some-of-hello-log-entries-that-i-expect"></a><span data-ttu-id="973de-209">Jag ser inte några hello loggposter som förväntat</span><span class="sxs-lookup"><span data-stu-id="973de-209">I'm not seeing some of hello log entries that I expect</span></span>
<span data-ttu-id="973de-210">Om ditt program skickar stora mängder data och du använder hello Application Insights SDK för ASP.NET version 2.0.0-beta3 eller senare, hello anpassningsbar provtagning funktionen fungerar och skicka endast en del av din telemetri.</span><span class="sxs-lookup"><span data-stu-id="973de-210">If your application sends a lot of data and you are using hello Application Insights SDK for ASP.NET version 2.0.0-beta3 or later, hello adaptive sampling feature may operate and send only a percentage of your telemetry.</span></span> [<span data-ttu-id="973de-211">Läs mer om sampling.</span><span class="sxs-lookup"><span data-stu-id="973de-211">Learn more about sampling.</span></span>](app-insights-sampling.md)

## <span data-ttu-id="973de-212"><a name="add"></a>Nästa steg</span><span class="sxs-lookup"><span data-stu-id="973de-212"><a name="add"></a>Next steps</span></span>
* <span data-ttu-id="973de-213">[Ställ in tillgänglighet och svarstider tester][availability]</span><span class="sxs-lookup"><span data-stu-id="973de-213">[Set up availability and responsiveness tests][availability]</span></span>
* <span data-ttu-id="973de-214">[Felsökning][qna]</span><span class="sxs-lookup"><span data-stu-id="973de-214">[Troubleshooting][qna]</span></span>

<!--Link references-->

[availability]: app-insights-monitor-web-app-availability.md
[diagnostic]: app-insights-diagnostic-search.md
[exceptions]: app-insights-asp-net-exceptions.md
[portal]: https://portal.azure.com/
[qna]: app-insights-troubleshoot-faq.md
[start]: app-insights-overview.md
