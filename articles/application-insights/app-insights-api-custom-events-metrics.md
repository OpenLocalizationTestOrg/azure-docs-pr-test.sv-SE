---
title: "aaaApplication Insights API för anpassade händelser och mått | Microsoft Docs"
description: "Infoga ett fåtal rader med kod i din enhet eller skrivbordsapp, webbsida eller tjänst, tootrack användning och diagnostisera problem."
services: application-insights
documentationcenter: 
author: CFreemanwa
manager: carmonm
ms.assetid: 80400495-c67b-4468-a92e-abf49793a54d
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: multiple
ms.topic: article
ms.date: 05/17/2017
ms.author: bwren
ms.openlocfilehash: f3d207a47bb4825efda806a19dd0c26540db7bdd
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="application-insights-api-for-custom-events-and-metrics"></a><span data-ttu-id="a44f6-103">Application Insights API för anpassade händelser och mått</span><span class="sxs-lookup"><span data-stu-id="a44f6-103">Application Insights API for custom events and metrics</span></span>

<span data-ttu-id="a44f6-104">Infoga några rader med kod i dina program toofind reda på vad användarna gör med den eller toohelp diagnostisera problem.</span><span class="sxs-lookup"><span data-stu-id="a44f6-104">Insert a few lines of code in your application toofind out what users are doing with it, or toohelp diagnose issues.</span></span> <span data-ttu-id="a44f6-105">Du kan skicka telemetri från enheten och den stationära appar, webbklienter och webbservrar.</span><span class="sxs-lookup"><span data-stu-id="a44f6-105">You can send telemetry from device and desktop apps, web clients, and web servers.</span></span> <span data-ttu-id="a44f6-106">Använd hello [Azure Application Insights](app-insights-overview.md) telemetriska API: N toosend anpassade händelser och mått och egna versioner av standard telemetri.</span><span class="sxs-lookup"><span data-stu-id="a44f6-106">Use hello [Azure Application Insights](app-insights-overview.md) core telemetry API toosend custom events and metrics, and your own versions of standard telemetry.</span></span> <span data-ttu-id="a44f6-107">Detta API är hello samma API hello standarden Application Insights datainsamlare använder.</span><span class="sxs-lookup"><span data-stu-id="a44f6-107">This API is hello same API that hello standard Application Insights data collectors use.</span></span>

## <a name="api-summary"></a><span data-ttu-id="a44f6-108">API-sammanfattning</span><span class="sxs-lookup"><span data-stu-id="a44f6-108">API summary</span></span>
<span data-ttu-id="a44f6-109">hello API är enhetligt för alla plattformar förutom några mindre variationer.</span><span class="sxs-lookup"><span data-stu-id="a44f6-109">hello API is uniform across all platforms, apart from a few small variations.</span></span>

| <span data-ttu-id="a44f6-110">Metod</span><span class="sxs-lookup"><span data-stu-id="a44f6-110">Method</span></span> | <span data-ttu-id="a44f6-111">Används för</span><span class="sxs-lookup"><span data-stu-id="a44f6-111">Used for</span></span> |
| --- | --- |
| [`TrackPageView`](#page-views) |<span data-ttu-id="a44f6-112">Sidor, skärmar, blad eller formulär.</span><span class="sxs-lookup"><span data-stu-id="a44f6-112">Pages, screens, blades, or forms.</span></span> |
| [`TrackEvent`](#trackevent) |<span data-ttu-id="a44f6-113">Användaråtgärder och andra händelser.</span><span class="sxs-lookup"><span data-stu-id="a44f6-113">User actions and other events.</span></span> <span data-ttu-id="a44f6-114">Använda tootrack användaren beteende eller toomonitor prestanda.</span><span class="sxs-lookup"><span data-stu-id="a44f6-114">Used tootrack user behavior or toomonitor performance.</span></span> |
| [`TrackMetric`](#trackmetric) |<span data-ttu-id="a44f6-115">Prestandamått, till exempel kölängder inte relaterade toospecific händelser.</span><span class="sxs-lookup"><span data-stu-id="a44f6-115">Performance measurements such as queue lengths not related toospecific events.</span></span> |
| [`TrackException`](#trackexception) |<span data-ttu-id="a44f6-116">Loggning undantag för diagnos.</span><span class="sxs-lookup"><span data-stu-id="a44f6-116">Logging exceptions for diagnosis.</span></span> <span data-ttu-id="a44f6-117">Spåra där de förekommer i relationen tooother händelser och undersöka stackspår.</span><span class="sxs-lookup"><span data-stu-id="a44f6-117">Trace where they occur in relation tooother events and examine stack traces.</span></span> |
| [`TrackRequest`](#trackrequest) |<span data-ttu-id="a44f6-118">Loggning hello frekvens och varaktighet för serverbegäranden för prestandaanalys.</span><span class="sxs-lookup"><span data-stu-id="a44f6-118">Logging hello frequency and duration of server requests for performance analysis.</span></span> |
| [`TrackTrace`](#tracktrace) |<span data-ttu-id="a44f6-119">Diagnostiska loggmeddelanden.</span><span class="sxs-lookup"><span data-stu-id="a44f6-119">Diagnostic log messages.</span></span> <span data-ttu-id="a44f6-120">Du kan också samla in loggar från tredje part.</span><span class="sxs-lookup"><span data-stu-id="a44f6-120">You can also capture third-party logs.</span></span> |
| [`TrackDependency`](#trackdependency) |<span data-ttu-id="a44f6-121">Loggning hello varaktighet och frekvens för anrop tooexternal komponenter som din app är beroende av.</span><span class="sxs-lookup"><span data-stu-id="a44f6-121">Logging hello duration and frequency of calls tooexternal components that your app depends on.</span></span> |

<span data-ttu-id="a44f6-122">Du kan [bifoga egenskaper och mått](#properties) toomost för anropen telemetri.</span><span class="sxs-lookup"><span data-stu-id="a44f6-122">You can [attach properties and metrics](#properties) toomost of these telemetry calls.</span></span>

## <span data-ttu-id="a44f6-123"><a name="prep"></a>Innan du börjar</span><span class="sxs-lookup"><span data-stu-id="a44f6-123"><a name="prep"></a>Before you start</span></span>
<span data-ttu-id="a44f6-124">Om du inte har en referens i Application Insights SDK ännu:</span><span class="sxs-lookup"><span data-stu-id="a44f6-124">If you don't have a reference on Application Insights SDK yet:</span></span>

* <span data-ttu-id="a44f6-125">Lägga till hello Application Insights SDK tooyour projekt:</span><span class="sxs-lookup"><span data-stu-id="a44f6-125">Add hello Application Insights SDK tooyour project:</span></span>

  * [<span data-ttu-id="a44f6-126">ASP.NET-projekt</span><span class="sxs-lookup"><span data-stu-id="a44f6-126">ASP.NET project</span></span>](app-insights-asp-net.md)
  * [<span data-ttu-id="a44f6-127">Java-projekt</span><span class="sxs-lookup"><span data-stu-id="a44f6-127">Java project</span></span>](app-insights-java-get-started.md)
  * [<span data-ttu-id="a44f6-128">JavaScript i varje webbsida</span><span class="sxs-lookup"><span data-stu-id="a44f6-128">JavaScript in each webpage</span></span>](app-insights-javascript.md) 
* <span data-ttu-id="a44f6-129">Inkludera i din enhet eller web serverkod:</span><span class="sxs-lookup"><span data-stu-id="a44f6-129">In your device or web server code, include:</span></span>

    <span data-ttu-id="a44f6-130">*C#:*`using Microsoft.ApplicationInsights;`</span><span class="sxs-lookup"><span data-stu-id="a44f6-130">*C#:* `using Microsoft.ApplicationInsights;`</span></span>

    <span data-ttu-id="a44f6-131">*Visual Basic:*`Imports Microsoft.ApplicationInsights`</span><span class="sxs-lookup"><span data-stu-id="a44f6-131">*Visual Basic:* `Imports Microsoft.ApplicationInsights`</span></span>

    <span data-ttu-id="a44f6-132">*Java:*`import com.microsoft.applicationinsights.TelemetryClient;`</span><span class="sxs-lookup"><span data-stu-id="a44f6-132">*Java:* `import com.microsoft.applicationinsights.TelemetryClient;`</span></span>

## <a name="constructing-a-telemetryclient-instance"></a><span data-ttu-id="a44f6-133">Hur du skapar en TelemetryClient-instans</span><span class="sxs-lookup"><span data-stu-id="a44f6-133">Constructing a TelemetryClient instance</span></span>
<span data-ttu-id="a44f6-134">Skapa en instans av `TelemetryClient` (förutom i JavaScript i webbsidor):</span><span class="sxs-lookup"><span data-stu-id="a44f6-134">Construct an instance of `TelemetryClient` (except in JavaScript in webpages):</span></span>

<span data-ttu-id="a44f6-135">*C#*</span><span class="sxs-lookup"><span data-stu-id="a44f6-135">*C#*</span></span>

    private TelemetryClient telemetry = new TelemetryClient();

<span data-ttu-id="a44f6-136">*Visual Basic*</span><span class="sxs-lookup"><span data-stu-id="a44f6-136">*Visual Basic*</span></span>

    Private Dim telemetry As New TelemetryClient

<span data-ttu-id="a44f6-137">*Java*</span><span class="sxs-lookup"><span data-stu-id="a44f6-137">*Java*</span></span>

    private TelemetryClient telemetry = new TelemetryClient();

<span data-ttu-id="a44f6-138">TelemetryClient är trådsäkra.</span><span class="sxs-lookup"><span data-stu-id="a44f6-138">TelemetryClient is thread-safe.</span></span>

<span data-ttu-id="a44f6-139">Vi rekommenderar att du använder en instans av TelemetryClient för varje modul för din app.</span><span class="sxs-lookup"><span data-stu-id="a44f6-139">We recommend that you use an instance of TelemetryClient for each module of your app.</span></span> <span data-ttu-id="a44f6-140">Du kan exempelvis har en TelemetryClient-instans i din web tooreport inkommande HTTP-begäranden och en annan i en mellanprogram klassen tooreport logik affärshändelser.</span><span class="sxs-lookup"><span data-stu-id="a44f6-140">For instance, you may have one TelemetryClient instance in your web service tooreport incoming HTTP requests, and another in a middleware class tooreport business logic events.</span></span> <span data-ttu-id="a44f6-141">Du kan ange egenskaper som `TelemetryClient.Context.User.Id` tootrack användare och sessioner, eller `TelemetryClient.Context.Device.Id` tooidentify hello datorn.</span><span class="sxs-lookup"><span data-stu-id="a44f6-141">You can set properties such as `TelemetryClient.Context.User.Id` tootrack users and sessions, or `TelemetryClient.Context.Device.Id` tooidentify hello machine.</span></span> <span data-ttu-id="a44f6-142">Den här informationen är anslutna tooall händelser som hello instans skickar.</span><span class="sxs-lookup"><span data-stu-id="a44f6-142">This information is attached tooall events that hello instance sends.</span></span>

## <a name="trackevent"></a><span data-ttu-id="a44f6-143">TrackEvent</span><span class="sxs-lookup"><span data-stu-id="a44f6-143">TrackEvent</span></span>
<span data-ttu-id="a44f6-144">I Application Insights en *anpassade händelsen* är en datapunkt som du kan visa i [Metrics Explorer](app-insights-metrics-explorer.md) som en sammansatt antal och i [diagnostiska Sök](app-insights-diagnostic-search.md) som individuella förekomster.</span><span class="sxs-lookup"><span data-stu-id="a44f6-144">In Application Insights, a *custom event* is a data point that you can display in [Metrics Explorer](app-insights-metrics-explorer.md) as an aggregated count, and in [Diagnostic Search](app-insights-diagnostic-search.md) as individual occurrences.</span></span> <span data-ttu-id="a44f6-145">(Det är inte relaterade tooMVC eller andra framework ”händelser”.)</span><span class="sxs-lookup"><span data-stu-id="a44f6-145">(It isn't related tooMVC or other framework "events.")</span></span>

<span data-ttu-id="a44f6-146">Infoga `TrackEvent` anropar i din kod toocount olika händelser.</span><span class="sxs-lookup"><span data-stu-id="a44f6-146">Insert `TrackEvent` calls in your code toocount various events.</span></span> <span data-ttu-id="a44f6-147">Hur ofta användare välja en viss funktion, hur ofta de uppnå specifika mål eller kanske hur ofta de göra vissa typer av misstag.</span><span class="sxs-lookup"><span data-stu-id="a44f6-147">How often users choose a particular feature, how often they achieve particular goals, or maybe how often they make particular types of mistakes.</span></span>

<span data-ttu-id="a44f6-148">Till exempel skicka en händelse när en användare wins hello spelet i spelet app:</span><span class="sxs-lookup"><span data-stu-id="a44f6-148">For example, in a game app, send an event whenever a user wins hello game:</span></span>

<span data-ttu-id="a44f6-149">*JavaScript*</span><span class="sxs-lookup"><span data-stu-id="a44f6-149">*JavaScript*</span></span>

    appInsights.trackEvent("WinGame");

<span data-ttu-id="a44f6-150">*C#*</span><span class="sxs-lookup"><span data-stu-id="a44f6-150">*C#*</span></span>

    telemetry.TrackEvent("WinGame");

<span data-ttu-id="a44f6-151">*Visual Basic*</span><span class="sxs-lookup"><span data-stu-id="a44f6-151">*Visual Basic*</span></span>

    telemetry.TrackEvent("WinGame")

<span data-ttu-id="a44f6-152">*Java*</span><span class="sxs-lookup"><span data-stu-id="a44f6-152">*Java*</span></span>

    telemetry.trackEvent("WinGame");

### <a name="view-your-events-in-hello-microsoft-azure-portal"></a><span data-ttu-id="a44f6-153">Visa händelserna i hello Microsoft Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="a44f6-153">View your events in hello Microsoft Azure portal</span></span>
<span data-ttu-id="a44f6-154">toosee antalet händelserna, öppna en [Metrics Explorer](app-insights-metrics-explorer.md) bladet Lägg till ett nytt diagram och välj **händelser**.</span><span class="sxs-lookup"><span data-stu-id="a44f6-154">toosee a count of your events, open a [Metrics Explorer](app-insights-metrics-explorer.md) blade, add a new chart, and select **Events**.</span></span>  

![Finns ett antal anpassade händelser](./media/app-insights-api-custom-events-metrics/01-custom.png)

<span data-ttu-id="a44f6-156">toocompare hello antal olika händelser, ställa in hello diagramtyp också**rutnätet**, och som händelsenamn:</span><span class="sxs-lookup"><span data-stu-id="a44f6-156">toocompare hello counts of different events, set hello chart type too**Grid**, and group by event name:</span></span>

![Ange hello diagramtyp och gruppering](./media/app-insights-api-custom-events-metrics/07-grid.png)

<span data-ttu-id="a44f6-158">Klicka på hello rutnätet via en händelse namnet toosee enskilda förekomster av händelsen.</span><span class="sxs-lookup"><span data-stu-id="a44f6-158">On hello grid, click through an event name toosee individual occurrences of that event.</span></span> <span data-ttu-id="a44f6-159">toosee mer detaljerat - Klicka på någon av förekomsterna i hello-listan.</span><span class="sxs-lookup"><span data-stu-id="a44f6-159">toosee more detail - click any occurrence in hello list.</span></span>

![Detaljvisning hello-händelser](./media/app-insights-api-custom-events-metrics/03-instances.png)

<span data-ttu-id="a44f6-161">toofocus på specifika händelser i sökning eller Metrics Explorer set hello bladet toohello händelse Filternamn som du är intresserad av:</span><span class="sxs-lookup"><span data-stu-id="a44f6-161">toofocus on specific events in either Search or Metrics Explorer, set hello blade's filter toohello event names that you're interested in:</span></span>

![Öppna filter, expanderar händelsenamnet och välj ett eller flera värden](./media/app-insights-api-custom-events-metrics/06-filter.png)

### <a name="custom-events-in-analytics"></a><span data-ttu-id="a44f6-163">Anpassade händelser i Analytics</span><span class="sxs-lookup"><span data-stu-id="a44f6-163">Custom events in Analytics</span></span>

<span data-ttu-id="a44f6-164">hello telemetri är tillgängliga i hello `customEvents` tabell i [Application Insights Analytics](app-insights-analytics.md).</span><span class="sxs-lookup"><span data-stu-id="a44f6-164">hello telemetry is available in hello `customEvents` table in [Application Insights Analytics](app-insights-analytics.md).</span></span> <span data-ttu-id="a44f6-165">Varje rad representerar ett anrop för`trackEvent(..)` i din app.</span><span class="sxs-lookup"><span data-stu-id="a44f6-165">Each row represents a call too`trackEvent(..)` in your app.</span></span> 

<span data-ttu-id="a44f6-166">Om [provtagning](app-insights-sampling.md) är i drift, hello itemCount egenskapen visar ett värde större än 1.</span><span class="sxs-lookup"><span data-stu-id="a44f6-166">If [sampling](app-insights-sampling.md) is in operation, hello itemCount property shows a value greater than 1.</span></span> <span data-ttu-id="a44f6-167">För exempel itemCount == 10 innebär att av 10 anrop tootrackEvent() hello provtagning processen bara skickas en av dem.</span><span class="sxs-lookup"><span data-stu-id="a44f6-167">For example itemCount==10 means that of 10 calls tootrackEvent(), hello sampling process only transmitted one of them.</span></span> <span data-ttu-id="a44f6-168">tooget rätt antal anpassade händelser, bör du använda därför använda kod som `customEvent | summarize sum(itemCount)`.</span><span class="sxs-lookup"><span data-stu-id="a44f6-168">tooget a correct count of custom events, you should use therefore use code such as `customEvent | summarize sum(itemCount)`.</span></span>


## <a name="trackmetric"></a><span data-ttu-id="a44f6-169">TrackMetric</span><span class="sxs-lookup"><span data-stu-id="a44f6-169">TrackMetric</span></span>

<span data-ttu-id="a44f6-170">Application Insights kan diagram mått som inte är anslutna tooparticular händelser.</span><span class="sxs-lookup"><span data-stu-id="a44f6-170">Application Insights can chart metrics that are not attached tooparticular events.</span></span> <span data-ttu-id="a44f6-171">Exempelvis kan du övervaka en Kölängd med jämna mellanrum.</span><span class="sxs-lookup"><span data-stu-id="a44f6-171">For example, you could monitor a queue length at regular intervals.</span></span> <span data-ttu-id="a44f6-172">Hello individuella mätningar tar mindre än hello variationer och trender med statistik, och därför statistiska stapeldiagram är användbara.</span><span class="sxs-lookup"><span data-stu-id="a44f6-172">With metrics, hello individual measurements are of less interest than hello variations and trends, and so statistical charts are useful.</span></span>

<span data-ttu-id="a44f6-173">Ordna toosend mått tooApplication insikter, du kan använda hello `TrackMetric(..)` API.</span><span class="sxs-lookup"><span data-stu-id="a44f6-173">In order toosend metrics tooApplication Insights, you can use hello `TrackMetric(..)` API.</span></span> <span data-ttu-id="a44f6-174">Det finns två sätt toosend ett mått:</span><span class="sxs-lookup"><span data-stu-id="a44f6-174">There are two ways toosend a metric:</span></span> 

* <span data-ttu-id="a44f6-175">Enstaka värde.</span><span class="sxs-lookup"><span data-stu-id="a44f6-175">Single value.</span></span> <span data-ttu-id="a44f6-176">Varje gång du utför ett mått i ditt program kan skicka du hello motsvarande värde tooApplication insikter.</span><span class="sxs-lookup"><span data-stu-id="a44f6-176">Every time you perform a measurement in your application, you send hello corresponding value tooApplication Insights.</span></span> <span data-ttu-id="a44f6-177">Anta att du har ett mått som beskriver hello antalet objekt i en behållare.</span><span class="sxs-lookup"><span data-stu-id="a44f6-177">For example, assume that you have a metric describing hello number of items in a container.</span></span> <span data-ttu-id="a44f6-178">Du först lägga till tre objekt i behållaren hello och sedan ta bort två objekt under en viss tidsperiod.</span><span class="sxs-lookup"><span data-stu-id="a44f6-178">During a particular time period, you first put three items into hello container and then you remove two items.</span></span> <span data-ttu-id="a44f6-179">Därför bör du anropa `TrackMetric` två gånger: först skicka hello värdet `3` och hello värdet `-2`.</span><span class="sxs-lookup"><span data-stu-id="a44f6-179">Accordingly, you would call `TrackMetric` twice: first passing hello value `3` and then hello value `-2`.</span></span> <span data-ttu-id="a44f6-180">Application Insights lagrar båda värdena för din räkning.</span><span class="sxs-lookup"><span data-stu-id="a44f6-180">Application Insights stores both values on your behalf.</span></span> 

* <span data-ttu-id="a44f6-181">Aggregering.</span><span class="sxs-lookup"><span data-stu-id="a44f6-181">Aggregation.</span></span> <span data-ttu-id="a44f6-182">När du arbetar med mått är sällan varje enskild mätning av intresse.</span><span class="sxs-lookup"><span data-stu-id="a44f6-182">When working with metrics, every single measurement is rarely of interest.</span></span> <span data-ttu-id="a44f6-183">I stället är en sammanfattning av vad som hände under en viss tidsperiod viktigt.</span><span class="sxs-lookup"><span data-stu-id="a44f6-183">Instead a summary of what happened during a particular time period is important.</span></span> <span data-ttu-id="a44f6-184">Sådana sammanfattning kallas _aggregering_.</span><span class="sxs-lookup"><span data-stu-id="a44f6-184">Such a summary is called _aggregation_.</span></span> <span data-ttu-id="a44f6-185">Hello ovan exempel hello mått summan för den tidsperioden är `1` och hello antal hello måttvärden är `2`.</span><span class="sxs-lookup"><span data-stu-id="a44f6-185">In hello above example, hello aggregate metric sum for that time period is `1` and hello count of hello metric values is `2`.</span></span> <span data-ttu-id="a44f6-186">När du använder hello aggregering metod kan du bara anropa `TrackMetric` en gång per period och skicka hello sammanställda tidsvärden.</span><span class="sxs-lookup"><span data-stu-id="a44f6-186">When using hello aggregation approach, you only invoke `TrackMetric` once per time period and send hello aggregate values.</span></span> <span data-ttu-id="a44f6-187">Detta är hello rekommenderat tillvägagångssätt eftersom det kan avsevärt minska hello kostnad och prestanda försämras genom att skicka färre data pekar tooApplication insikter när fortfarande att samla in alla relevanta uppgifter.</span><span class="sxs-lookup"><span data-stu-id="a44f6-187">This is hello recommended approach since it can significantly reduce hello cost and performance overhead by sending fewer data points tooApplication Insights, while still collecting all relevant information.</span></span>

### <a name="examples"></a><span data-ttu-id="a44f6-188">Exempel:</span><span class="sxs-lookup"><span data-stu-id="a44f6-188">Examples:</span></span>

#### <a name="single-values"></a><span data-ttu-id="a44f6-189">Enstaka värden</span><span class="sxs-lookup"><span data-stu-id="a44f6-189">Single values</span></span>

<span data-ttu-id="a44f6-190">toosend ett enda värde:</span><span class="sxs-lookup"><span data-stu-id="a44f6-190">toosend a single metric value:</span></span>

<span data-ttu-id="a44f6-191">*JavaScript*</span><span class="sxs-lookup"><span data-stu-id="a44f6-191">*JavaScript*</span></span>

 ```Javascript
     appInsights.trackMetric("queueLength", 42.0);
 ```

<span data-ttu-id="a44f6-192">*C#, Java*</span><span class="sxs-lookup"><span data-stu-id="a44f6-192">*C#, Java*</span></span>

```C#
    var sample = new MetricTelemetry();
    sample.Name = "metric name";
    sample.Value = 42.3;
    telemetryClient.TrackMetric(sample);
```

#### <a name="aggregating-metrics"></a><span data-ttu-id="a44f6-193">Sammanställa mått</span><span class="sxs-lookup"><span data-stu-id="a44f6-193">Aggregating metrics</span></span>

<span data-ttu-id="a44f6-194">Det rekommenderas tooaggregate mått innan de skickas från din app, tooreduce bandbredd, kostnad och tooimprove prestanda.</span><span class="sxs-lookup"><span data-stu-id="a44f6-194">It is recommended tooaggregate metrics before sending them from your app, tooreduce bandwidth, cost and tooimprove performance.</span></span>
<span data-ttu-id="a44f6-195">Här är ett exempel på sammanställa kod:</span><span class="sxs-lookup"><span data-stu-id="a44f6-195">Here is an example of aggregating code:</span></span>

<span data-ttu-id="a44f6-196">*C#*</span><span class="sxs-lookup"><span data-stu-id="a44f6-196">*C#*</span></span>

```C#
using System;
using System.Threading;
using System.Threading.Tasks;

using Microsoft.ApplicationInsights;
using Microsoft.ApplicationInsights.DataContracts;

namespace MetricAggregationExample
{
    /// <summary>
    /// Aggregates metric values for a single time period.
    /// </summary>
    internal class MetricAggregator
    {
        private SpinLock _trackLock = new SpinLock();

        public DateTimeOffset StartTimestamp    { get; }
        public int Count                        { get; private set; }
        public double Sum                       { get; private set; }
        public double SumOfSquares              { get; private set; }
        public double Min                       { get; private set; }
        public double Max                       { get; private set; }
        public double Average                   { get { return (Count == 0) ? 0 : (Sum / Count); } }
        public double Variance                  { get { return (Count == 0) ? 0 : (SumOfSquares / Count)
                                                                                  - (Average * Average); } }
        public double StandardDeviation         { get { return Math.Sqrt(Variance); } }

        public MetricAggregator(DateTimeOffset startTimestamp)
        {
            this.StartTimestamp = startTimestamp;
        }

        public void TrackValue(double value)
        {
            bool lockAcquired = false;

            try
            {
                _trackLock.Enter(ref lockAcquired);

                if ((Count == 0) || (value < Min))  { Min = value; }
                if ((Count == 0) || (value > Max))  { Max = value; }
                Count++;
                Sum += value;
                SumOfSquares += value * value;
            }
            finally
            {
                if (lockAcquired)
                {
                    _trackLock.Exit();
                }
            }
        }
    }   // internal class MetricAggregator

    /// <summary>
    /// Accepts metric values and sends hello aggregated values at 1-minute intervals.
    /// </summary>
    public sealed class Metric : IDisposable
    {
        private static readonly TimeSpan AggregationPeriod = TimeSpan.FromSeconds(60);

        private bool _isDisposed = false;
        private MetricAggregator _aggregator = null;
        private readonly TelemetryClient _telemetryClient;

        public string Name { get; }

        public Metric(string name, TelemetryClient telemetryClient)
        {
            this.Name = name ?? "null";
            this._aggregator = new MetricAggregator(DateTimeOffset.UtcNow);
            this._telemetryClient = telemetryClient ?? throw new ArgumentNullException(nameof(telemetryClient));

            Task.Run(this.AggregatorLoopAsync);
        }

        public void TrackValue(double value)
        {
            MetricAggregator currAggregator = _aggregator;
            if (currAggregator != null)
            {
                currAggregator.TrackValue(value);
            }
        }

        private async Task AggregatorLoopAsync()
        {
            while (_isDisposed == false)
            {
                try
                {
                    // Wait for end end of hello aggregation period:
                    await Task.Delay(AggregationPeriod).ConfigureAwait(continueOnCapturedContext: false);

                    // Atomically snap hello current aggregation:
                    MetricAggregator nextAggregator = new MetricAggregator(DateTimeOffset.UtcNow);
                    MetricAggregator prevAggregator = Interlocked.Exchange(ref _aggregator, nextAggregator);

                    // Only send anything is at least one value was measured:
                    if (prevAggregator != null && prevAggregator.Count > 0)
                    {
                        // Compute hello actual aggregation period length:
                        TimeSpan aggPeriod = nextAggregator.StartTimestamp - prevAggregator.StartTimestamp;
                        if (aggPeriod.TotalMilliseconds < 1)
                        {
                            aggPeriod = TimeSpan.FromMilliseconds(1);
                        }

                        // Construct hello metric telemetry item and send:
                        var aggregatedMetricTelemetry = new MetricTelemetry(
                                Name,
                                prevAggregator.Count,
                                prevAggregator.Sum,
                                prevAggregator.Min,
                                prevAggregator.Max,
                                prevAggregator.StandardDeviation);
                        aggregatedMetricTelemetry.Properties["AggregationPeriod"] = aggPeriod.ToString("c");

                        _telemetryClient.Track(aggregatedMetricTelemetry);
                    }
                }
                catch(Exception ex)
                {
                    // log ex as appropriate for your application
                }
            }
        }

        void IDisposable.Dispose()
        {
            _isDisposed = true;
            _aggregator = null;
        }
    }   // public sealed class Metric
}
```

### <a name="custom-metrics-in-metrics-explorer"></a><span data-ttu-id="a44f6-197">Anpassade mått i Metrics Explorer</span><span class="sxs-lookup"><span data-stu-id="a44f6-197">Custom metrics in Metrics Explorer</span></span>

<span data-ttu-id="a44f6-198">toosee hello resultat, öppna Metrics Explorer och Lägg till ett nytt diagram.</span><span class="sxs-lookup"><span data-stu-id="a44f6-198">toosee hello results, open Metrics Explorer and add a new chart.</span></span> <span data-ttu-id="a44f6-199">Redigera hello diagram tooshow din mått.</span><span class="sxs-lookup"><span data-stu-id="a44f6-199">Edit hello chart tooshow your metric.</span></span>

> [!NOTE]
> <span data-ttu-id="a44f6-200">Din anpassade mått kan ta flera minuter tooappear i hello lista över tillgängliga mått.</span><span class="sxs-lookup"><span data-stu-id="a44f6-200">Your custom metric might take several minutes tooappear in hello list of available metrics.</span></span>
>

![Lägg till ett nytt schema eller välj ett diagram och under anpassad, väljer du din mått](./media/app-insights-api-custom-events-metrics/03-track-custom.png)

### <a name="custom-metrics-in-analytics"></a><span data-ttu-id="a44f6-202">Anpassade mått i Analytics</span><span class="sxs-lookup"><span data-stu-id="a44f6-202">Custom metrics in Analytics</span></span>

<span data-ttu-id="a44f6-203">hello telemetri är tillgängliga i hello `customMetrics` tabell i [Application Insights Analytics](app-insights-analytics.md).</span><span class="sxs-lookup"><span data-stu-id="a44f6-203">hello telemetry is available in hello `customMetrics` table in [Application Insights Analytics](app-insights-analytics.md).</span></span> <span data-ttu-id="a44f6-204">Varje rad representerar ett anrop för`trackMetric(..)` i din app.</span><span class="sxs-lookup"><span data-stu-id="a44f6-204">Each row represents a call too`trackMetric(..)` in your app.</span></span>
* <span data-ttu-id="a44f6-205">`valueSum`-Detta är hello summan av hello mått.</span><span class="sxs-lookup"><span data-stu-id="a44f6-205">`valueSum` - This is hello sum of hello measurements.</span></span> <span data-ttu-id="a44f6-206">tooget hello medelvärdet, division med `valueCount`.</span><span class="sxs-lookup"><span data-stu-id="a44f6-206">tooget hello mean value, divide by `valueCount`.</span></span>
* <span data-ttu-id="a44f6-207">`valueCount`-hello antalet mått som har aggregerats till detta `trackMetric(..)` anropa.</span><span class="sxs-lookup"><span data-stu-id="a44f6-207">`valueCount` - hello number of measurements that were aggregated into this `trackMetric(..)` call.</span></span>

## <a name="page-views"></a><span data-ttu-id="a44f6-208">Sidvisningar</span><span class="sxs-lookup"><span data-stu-id="a44f6-208">Page views</span></span>
<span data-ttu-id="a44f6-209">I en enhet eller en webbsida app skickas sidan Visa telemetri som standard när varje skärmen eller sidan har lästs in.</span><span class="sxs-lookup"><span data-stu-id="a44f6-209">In a device or webpage app, page view telemetry is sent by default when each screen or page is loaded.</span></span> <span data-ttu-id="a44f6-210">Men du kan ändra de tootrack sidvisningar vid ytterligare eller olika tidpunkter.</span><span class="sxs-lookup"><span data-stu-id="a44f6-210">But you can change that tootrack page views at additional or different times.</span></span> <span data-ttu-id="a44f6-211">Till exempel i en app som visar flikarna eller blad, kan du tootrack en sida när hello användare öppnar ett nytt blad.</span><span class="sxs-lookup"><span data-stu-id="a44f6-211">For example, in an app that displays tabs or blades, you might want tootrack a page whenever hello user opens a new blade.</span></span>

![Användning linsen på bladet översikt](./media/app-insights-api-custom-events-metrics/appinsights-47usage-2.png)

<span data-ttu-id="a44f6-213">Användar-och sessionen skickas som egenskaper tillsammans med sidvyer så hello användar- och sessionsobjekt diagram kommer alive när sidan Visa telemetri.</span><span class="sxs-lookup"><span data-stu-id="a44f6-213">User and session data is sent as properties along with page views, so hello user and session charts come alive when there is page view telemetry.</span></span>

### <a name="custom-page-views"></a><span data-ttu-id="a44f6-214">Anpassade sidvisningar</span><span class="sxs-lookup"><span data-stu-id="a44f6-214">Custom page views</span></span>
<span data-ttu-id="a44f6-215">*JavaScript*</span><span class="sxs-lookup"><span data-stu-id="a44f6-215">*JavaScript*</span></span>

    appInsights.trackPageView("tab1");

<span data-ttu-id="a44f6-216">*C#*</span><span class="sxs-lookup"><span data-stu-id="a44f6-216">*C#*</span></span>

    telemetry.TrackPageView("GameReviewPage");

<span data-ttu-id="a44f6-217">*Visual Basic*</span><span class="sxs-lookup"><span data-stu-id="a44f6-217">*Visual Basic*</span></span>

    telemetry.TrackPageView("GameReviewPage")


<span data-ttu-id="a44f6-218">Om du har flera flikar i olika HTML-sidor kan ange du hello URL för:</span><span class="sxs-lookup"><span data-stu-id="a44f6-218">If you have several tabs within different HTML pages, you can specify hello URL too:</span></span>

    appInsights.trackPageView("tab1", "http://fabrikam.com/page1.htm");

### <a name="timing-page-views"></a><span data-ttu-id="a44f6-219">Tidsinställning sidvisningar</span><span class="sxs-lookup"><span data-stu-id="a44f6-219">Timing page views</span></span>
<span data-ttu-id="a44f6-220">Som standard hello gånger rapporteras som **inläsningstid för sidvisning** mäts från när hello webbläsaren skickar hello begäran förrän hello webbläsarens sidan händelsen anropas.</span><span class="sxs-lookup"><span data-stu-id="a44f6-220">By default, hello times reported as **Page view load time** are measured from when hello browser sends hello request, until hello browser's page load event is called.</span></span>

<span data-ttu-id="a44f6-221">I stället kan du antingen:</span><span class="sxs-lookup"><span data-stu-id="a44f6-221">Instead, you can either:</span></span>

* <span data-ttu-id="a44f6-222">Ange en explicit varaktighet i hello [trackPageView](https://github.com/Microsoft/ApplicationInsights-JS/blob/master/API-reference.md#trackpageview) anropa: `appInsights.trackPageView("tab1", null, null, null, durationInMilliseconds);`.</span><span class="sxs-lookup"><span data-stu-id="a44f6-222">Set an explicit duration in hello [trackPageView](https://github.com/Microsoft/ApplicationInsights-JS/blob/master/API-reference.md#trackpageview) call: `appInsights.trackPageView("tab1", null, null, null, durationInMilliseconds);`.</span></span>
* <span data-ttu-id="a44f6-223">Använd hello sidan Visa tidsinställning samtal `startTrackPage` och `stopTrackPage`.</span><span class="sxs-lookup"><span data-stu-id="a44f6-223">Use hello page view timing calls `startTrackPage` and `stopTrackPage`.</span></span>

<span data-ttu-id="a44f6-224">*JavaScript*</span><span class="sxs-lookup"><span data-stu-id="a44f6-224">*JavaScript*</span></span>

    // toostart timing a page:
    appInsights.startTrackPage("Page1");

<span data-ttu-id="a44f6-225">...</span><span class="sxs-lookup"><span data-stu-id="a44f6-225">...</span></span>

    // toostop timing and log hello page:
    appInsights.stopTrackPage("Page1", url, properties, measurements);

<span data-ttu-id="a44f6-226">Hej namn som du använder som hello första parametern associerar hello starta och stoppa anrop.</span><span class="sxs-lookup"><span data-stu-id="a44f6-226">hello name that you use as hello first parameter associates hello start and stop calls.</span></span> <span data-ttu-id="a44f6-227">Toohello aktuella sidnamn standardvärdet.</span><span class="sxs-lookup"><span data-stu-id="a44f6-227">It defaults toohello current page name.</span></span>

<span data-ttu-id="a44f6-228">hello resulterande sidinläsning varaktighet som visas i Metrics Explorer härleds från hello intervallet mellan hello starta och stoppa anrop.</span><span class="sxs-lookup"><span data-stu-id="a44f6-228">hello resulting page load durations displayed in Metrics Explorer are derived from hello interval between hello start and stop calls.</span></span> <span data-ttu-id="a44f6-229">Är det upp tooyou vilka intervall som du faktiskt tid.</span><span class="sxs-lookup"><span data-stu-id="a44f6-229">It's up tooyou what interval you actually time.</span></span>

### <a name="page-telemetry-in-analytics"></a><span data-ttu-id="a44f6-230">Sidan telemetri i Analytics</span><span class="sxs-lookup"><span data-stu-id="a44f6-230">Page telemetry in Analytics</span></span>

<span data-ttu-id="a44f6-231">I [Analytics](app-insights-analytics.md) två tabeller visar data från funktioner i webbläsaren:</span><span class="sxs-lookup"><span data-stu-id="a44f6-231">In [Analytics](app-insights-analytics.md) two tables show data from browser operations:</span></span>

* <span data-ttu-id="a44f6-232">Hej `pageViews` tabellen innehåller information om hello URL och sidrubriken</span><span class="sxs-lookup"><span data-stu-id="a44f6-232">hello `pageViews` table contains data about hello URL and page title</span></span>
* <span data-ttu-id="a44f6-233">Hej `browserTimings` tabellen innehåller information om klientens prestanda som hello tidsåtgång tooprocess hello inkommande data</span><span class="sxs-lookup"><span data-stu-id="a44f6-233">hello `browserTimings` table contains data about client performance, such as hello time taken tooprocess hello incoming data</span></span>

<span data-ttu-id="a44f6-234">toofind hur lång tid tar hello webbläsare tooprocess olika sidor:</span><span class="sxs-lookup"><span data-stu-id="a44f6-234">toofind how long hello browser takes tooprocess different pages:</span></span>

```
browserTimings | summarize avg(networkDuration), avg(processingDuration), avg(totalDuration) by name 
```

<span data-ttu-id="a44f6-235">toodiscover hello popularities av olika webbläsare:</span><span class="sxs-lookup"><span data-stu-id="a44f6-235">toodiscover hello popularities of different browsers:</span></span>

```
pageViews | summarize count() by client_Browser
```

<span data-ttu-id="a44f6-236">tooassociate vyer tooAJAX sidanrop, ansluta med beroenden:</span><span class="sxs-lookup"><span data-stu-id="a44f6-236">tooassociate page views tooAJAX calls, join with dependencies:</span></span>

```
pageViews | join (dependencies) on operation_Id 
```

## <a name="trackrequest"></a><span data-ttu-id="a44f6-237">TrackRequest</span><span class="sxs-lookup"><span data-stu-id="a44f6-237">TrackRequest</span></span>
<span data-ttu-id="a44f6-238">hello server SDK använder TrackRequest toolog HTTP-begäranden.</span><span class="sxs-lookup"><span data-stu-id="a44f6-238">hello server SDK uses TrackRequest toolog HTTP requests.</span></span>

<span data-ttu-id="a44f6-239">Du kan också anropa den själv om du vill toosimulate begäranden i ett sammanhang där du inte har hello web service-modulen körs.</span><span class="sxs-lookup"><span data-stu-id="a44f6-239">You can also call it yourself if you want toosimulate requests in a context where you don't have hello web service module running.</span></span>

<span data-ttu-id="a44f6-240">Hello bör dock sätt toosend begärandetelemetri är där hello begäran fungerar som en <a href="#operation-context">åtgärdssammanhang</a>.</span><span class="sxs-lookup"><span data-stu-id="a44f6-240">However, hello recommended way toosend request telemetry is where hello request acts as an <a href="#operation-context">operation context</a>.</span></span>

## <a name="operation-context"></a><span data-ttu-id="a44f6-241">Åtgärden kontext</span><span class="sxs-lookup"><span data-stu-id="a44f6-241">Operation context</span></span>
<span data-ttu-id="a44f6-242">Koppla ihop telemetri objekt genom att koppla toothem en gemensam åtgärds-ID.</span><span class="sxs-lookup"><span data-stu-id="a44f6-242">You can associate telemetry items together by attaching toothem a common operation ID.</span></span> <span data-ttu-id="a44f6-243">hello standard spårning av förfrågningar modulen sker för undantag och andra händelser som sänds när en HTTP-begäran bearbetas.</span><span class="sxs-lookup"><span data-stu-id="a44f6-243">hello standard request-tracking module does this for exceptions and other events that are sent while an HTTP request is being processed.</span></span> <span data-ttu-id="a44f6-244">I [Sök](app-insights-diagnostic-search.md) och [Analytics](app-insights-analytics.md), du kan använda hello ID tooeasily Sök alla händelser som är associerade med hello-begäran.</span><span class="sxs-lookup"><span data-stu-id="a44f6-244">In [Search](app-insights-diagnostic-search.md) and [Analytics](app-insights-analytics.md), you can use hello ID tooeasily find any events associated with hello request.</span></span>

<span data-ttu-id="a44f6-245">hello enklaste sättet tooset hello-ID är tooset en kontext för åtgärden med hjälp av det här mönstret:</span><span class="sxs-lookup"><span data-stu-id="a44f6-245">hello easiest way tooset hello ID is tooset an operation context by using this pattern:</span></span>

<span data-ttu-id="a44f6-246">*C#*</span><span class="sxs-lookup"><span data-stu-id="a44f6-246">*C#*</span></span>

```C#
// Establish an operation context and associated telemetry item:
using (var operation = telemetry.StartOperation<RequestTelemetry>("operationName"))
{
    // Telemetry sent in here will use hello same operation ID.
    ...
    telemetry.TrackTrace(...); // or other Track* calls
    ...
    // Set properties of containing telemetry item--for example:
    operation.Telemetry.ResponseCode = "200";

    // Optional: explicitly send telemetry item:
    telemetry.StopOperation(operation);

} // When operation is disposed, telemetry item is sent.
```

<span data-ttu-id="a44f6-247">Tillsammans med inställningen en åtgärdssammanhang `StartOperation` skapar ett telemetri objekt hello-typ som du anger.</span><span class="sxs-lookup"><span data-stu-id="a44f6-247">Along with setting an operation context, `StartOperation` creates a telemetry item of hello type that you specify.</span></span> <span data-ttu-id="a44f6-248">Skickar den hello telemetri objektet automatiskt när hello igen, eller om du explicit anropa `StopOperation`.</span><span class="sxs-lookup"><span data-stu-id="a44f6-248">It sends hello telemetry item when you dispose hello operation, or if you explicitly call `StopOperation`.</span></span> <span data-ttu-id="a44f6-249">Om du använder `RequestTelemetry` som hello telemetri typ, varaktigheten har angetts toohello tidsgränsen intervallet mellan start och stopp.</span><span class="sxs-lookup"><span data-stu-id="a44f6-249">If you use `RequestTelemetry` as hello telemetry type, its duration is set toohello timed interval between start and stop.</span></span>

<span data-ttu-id="a44f6-250">Åtgärden kontexter kan inte kapslas.</span><span class="sxs-lookup"><span data-stu-id="a44f6-250">Operation contexts can't be nested.</span></span> <span data-ttu-id="a44f6-251">Om det finns redan en kontext för åtgärden, så att dess ID är associerad med alla hello finns artiklar, inklusive hello-objekt som skapats med `StartOperation`.</span><span class="sxs-lookup"><span data-stu-id="a44f6-251">If there is already an operation context, then its ID is associated with all hello contained items, including hello item created with `StartOperation`.</span></span>

<span data-ttu-id="a44f6-252">I sökning hello åtgärden kontext är används toocreate hello **relaterade objekt** lista:</span><span class="sxs-lookup"><span data-stu-id="a44f6-252">In Search, hello operation context is used toocreate hello **Related Items** list:</span></span>

![Relaterade objekt](./media/app-insights-api-custom-events-metrics/21.png)

<span data-ttu-id="a44f6-254">Mer information om anpassade åtgärder spårning finns i [program – insikter-anpassad-operationer-tracking.md].</span><span class="sxs-lookup"><span data-stu-id="a44f6-254">See [application-insights-custom-operations-tracking.md] for more information on custom operations tracking.</span></span>

### <a name="requests-in-analytics"></a><span data-ttu-id="a44f6-255">Begäranden i Analytics</span><span class="sxs-lookup"><span data-stu-id="a44f6-255">Requests in Analytics</span></span> 

<span data-ttu-id="a44f6-256">I [Application Insights Analytics](app-insights-analytics.md), begär visa upp i hello `requests` tabell.</span><span class="sxs-lookup"><span data-stu-id="a44f6-256">In [Application Insights Analytics](app-insights-analytics.md), requests show up in hello `requests` table.</span></span>

<span data-ttu-id="a44f6-257">Om [provtagning](app-insights-sampling.md) är i drift, hello itemCount egenskapen visar ett värde större än 1.</span><span class="sxs-lookup"><span data-stu-id="a44f6-257">If [sampling](app-insights-sampling.md) is in operation, hello itemCount property will show a value greater than 1.</span></span> <span data-ttu-id="a44f6-258">För exempel itemCount == 10 innebär att av 10 anrop tootrackRequest() hello provtagning processen bara skickas en av dem.</span><span class="sxs-lookup"><span data-stu-id="a44f6-258">For example itemCount==10 means that of 10 calls tootrackRequest(), hello sampling process only transmitted one of them.</span></span> <span data-ttu-id="a44f6-259">tooget segmenterat rätt antal begäranden och genomsnittlig varaktighet per begäran namn, använda koden som:</span><span class="sxs-lookup"><span data-stu-id="a44f6-259">tooget a correct count of requests and average duration segmented by request names, use code such as:</span></span>

```AIQL
requests | summarize count = sum(itemCount), avgduration = avg(duration) by name
```


## <a name="trackexception"></a><span data-ttu-id="a44f6-260">TrackException</span><span class="sxs-lookup"><span data-stu-id="a44f6-260">TrackException</span></span>
<span data-ttu-id="a44f6-261">Skicka undantag tooApplication insikter:</span><span class="sxs-lookup"><span data-stu-id="a44f6-261">Send exceptions tooApplication Insights:</span></span>

* <span data-ttu-id="a44f6-262">för[räkna](app-insights-metrics-explorer.md), som en indikation på hello ofta ett problem.</span><span class="sxs-lookup"><span data-stu-id="a44f6-262">too[count them](app-insights-metrics-explorer.md), as an indication of hello frequency of a problem.</span></span>
* <span data-ttu-id="a44f6-263">för[undersöka enskilda förekomster](app-insights-diagnostic-search.md).</span><span class="sxs-lookup"><span data-stu-id="a44f6-263">too[examine individual occurrences](app-insights-diagnostic-search.md).</span></span>

<span data-ttu-id="a44f6-264">hello-rapporterna innehåller hello stackspår.</span><span class="sxs-lookup"><span data-stu-id="a44f6-264">hello reports include hello stack traces.</span></span>

<span data-ttu-id="a44f6-265">*C#*</span><span class="sxs-lookup"><span data-stu-id="a44f6-265">*C#*</span></span>

    try
    {
        ...
    }
    catch (Exception ex)
    {
       telemetry.TrackException(ex);
    }

<span data-ttu-id="a44f6-266">*JavaScript*</span><span class="sxs-lookup"><span data-stu-id="a44f6-266">*JavaScript*</span></span>

    try
    {
       ...
    }
    catch (ex)
    {
       appInsights.trackException(ex);
    }

<span data-ttu-id="a44f6-267">hello SDK fånga undantag som många automatiskt, så att du inte alltid har toocall TrackException explicit.</span><span class="sxs-lookup"><span data-stu-id="a44f6-267">hello SDKs catch many exceptions automatically, so you don't always have toocall TrackException explicitly.</span></span>

* <span data-ttu-id="a44f6-268">ASP.NET: [skriva kod toocatch undantag](app-insights-asp-net-exceptions.md).</span><span class="sxs-lookup"><span data-stu-id="a44f6-268">ASP.NET: [Write code toocatch exceptions](app-insights-asp-net-exceptions.md).</span></span>
* <span data-ttu-id="a44f6-269">J2EE: [undantag noteras automatiskt](app-insights-java-get-started.md#exceptions-and-request-failures).</span><span class="sxs-lookup"><span data-stu-id="a44f6-269">J2EE: [Exceptions are caught automatically](app-insights-java-get-started.md#exceptions-and-request-failures).</span></span>
* <span data-ttu-id="a44f6-270">JavaScript: Undantag noteras automatiskt.</span><span class="sxs-lookup"><span data-stu-id="a44f6-270">JavaScript: Exceptions are caught automatically.</span></span> <span data-ttu-id="a44f6-271">Om du vill toodisable Automatisk insamling, lägger du till en rad toohello kodstycke som infogas i dina webbsidor:</span><span class="sxs-lookup"><span data-stu-id="a44f6-271">If you want toodisable automatic collection, add a line toohello code snippet that you insert in your webpages:</span></span>

    ```
    ({
      instrumentationKey: "your key"
      , disableExceptionTracking: true
    })
    ```

### <a name="exceptions-in-analytics"></a><span data-ttu-id="a44f6-272">Undantag i Analytics</span><span class="sxs-lookup"><span data-stu-id="a44f6-272">Exceptions in Analytics</span></span>

<span data-ttu-id="a44f6-273">I [Application Insights Analytics](app-insights-analytics.md), undantag visas i hello `exceptions` tabell.</span><span class="sxs-lookup"><span data-stu-id="a44f6-273">In [Application Insights Analytics](app-insights-analytics.md), exceptions show up in hello `exceptions` table.</span></span>

<span data-ttu-id="a44f6-274">Om [provtagning](app-insights-sampling.md) är i drift, hello `itemCount` egenskapen visar ett värde större än 1.</span><span class="sxs-lookup"><span data-stu-id="a44f6-274">If [sampling](app-insights-sampling.md) is in operation, hello `itemCount` property shows a value greater than 1.</span></span> <span data-ttu-id="a44f6-275">För exempel itemCount == 10 innebär att av 10 anrop tootrackException() hello provtagning processen bara skickas en av dem.</span><span class="sxs-lookup"><span data-stu-id="a44f6-275">For example itemCount==10 means that of 10 calls tootrackException(), hello sampling process only transmitted one of them.</span></span> <span data-ttu-id="a44f6-276">tooget rätt antal undantag segmenterade av typ av undantag, använda koden som:</span><span class="sxs-lookup"><span data-stu-id="a44f6-276">tooget a correct count of exceptions segmented by type of exception, use code such as:</span></span>

```
exceptions | summarize sum(itemCount) by type
```

<span data-ttu-id="a44f6-277">De flesta hello viktiga Stackinformation redan extraherade i olika variabler, men du kan dra ifrån hello `details` struktur tooget mer.</span><span class="sxs-lookup"><span data-stu-id="a44f6-277">Most of hello important stack information is already extracted into separate variables, but you can pull apart hello `details` structure tooget more.</span></span> <span data-ttu-id="a44f6-278">Eftersom den här strukturen är dynamiska, bör du omvandla hello toohello resultattyp du förväntar dig.</span><span class="sxs-lookup"><span data-stu-id="a44f6-278">Since this structure is dynamic, you should cast hello result toohello type you expect.</span></span> <span data-ttu-id="a44f6-279">Exempel:</span><span class="sxs-lookup"><span data-stu-id="a44f6-279">For example:</span></span>

```AIQL
exceptions
| extend method2 = tostring(details[0].parsedStack[1].method)
```

<span data-ttu-id="a44f6-280">tooassociate undantag med relaterade begäranden, Använd en koppling:</span><span class="sxs-lookup"><span data-stu-id="a44f6-280">tooassociate exceptions with their related requests, use a join:</span></span>

```
exceptions
| join (requests) on operation_Id 
```

## <a name="tracktrace"></a><span data-ttu-id="a44f6-281">TrackTrace</span><span class="sxs-lookup"><span data-stu-id="a44f6-281">TrackTrace</span></span>
<span data-ttu-id="a44f6-282">Använd TrackTrace toohelp diagnostisera problem genom att skicka en ”spåret spår” tooApplication insikter.</span><span class="sxs-lookup"><span data-stu-id="a44f6-282">Use TrackTrace toohelp diagnose problems by sending a "breadcrumb trail" tooApplication Insights.</span></span> <span data-ttu-id="a44f6-283">Du kan skicka mängder diagnostikdata och granska dem i [diagnostiska Sök](app-insights-diagnostic-search.md).</span><span class="sxs-lookup"><span data-stu-id="a44f6-283">You can send chunks of diagnostic data and inspect them in [Diagnostic Search](app-insights-diagnostic-search.md).</span></span>

<span data-ttu-id="a44f6-284">[Logga kort](app-insights-asp-net-trace-logs.md) använda API toosend från tredje part loggar toohello portalen.</span><span class="sxs-lookup"><span data-stu-id="a44f6-284">[Log adapters](app-insights-asp-net-trace-logs.md) use this API toosend third-party logs toohello portal.</span></span>

<span data-ttu-id="a44f6-285">*C#*</span><span class="sxs-lookup"><span data-stu-id="a44f6-285">*C#*</span></span>

    telemetry.TrackTrace(message, SeverityLevel.Warning, properties);


<span data-ttu-id="a44f6-286">Du kan söka i meddelandeinnehåll, men du kan inte filtrera (till skillnad från egenskapsvärden) på den.</span><span class="sxs-lookup"><span data-stu-id="a44f6-286">You can search on message content, but (unlike property values) you can't filter on it.</span></span>

<span data-ttu-id="a44f6-287">hello storleksgränsen på `message` är mycket högre än hello gränsen på Egenskaper.</span><span class="sxs-lookup"><span data-stu-id="a44f6-287">hello size limit on `message` is much higher than hello limit on properties.</span></span>
<span data-ttu-id="a44f6-288">En fördel med TrackTrace är att du kan publicera relativt lång data i hello-meddelande.</span><span class="sxs-lookup"><span data-stu-id="a44f6-288">An advantage of TrackTrace is that you can put relatively long data in hello message.</span></span> <span data-ttu-id="a44f6-289">Koda till exempel det postdata.</span><span class="sxs-lookup"><span data-stu-id="a44f6-289">For example, you can encode POST data there.</span></span>  

<span data-ttu-id="a44f6-290">Dessutom kan du lägga till ett allvarlighetsgrad nivå tooyour meddelande.</span><span class="sxs-lookup"><span data-stu-id="a44f6-290">In addition, you can add a severity level tooyour message.</span></span> <span data-ttu-id="a44f6-291">Och precis som andra telemetri, du kan lägga till egenskapen värden toohelp du filtrera eller söka efter olika uppsättningar av spår.</span><span class="sxs-lookup"><span data-stu-id="a44f6-291">And, like other telemetry, you can add property values toohelp you filter or search for different sets of traces.</span></span> <span data-ttu-id="a44f6-292">Exempel:</span><span class="sxs-lookup"><span data-stu-id="a44f6-292">For example:</span></span>

    var telemetry = new Microsoft.ApplicationInsights.TelemetryClient();
    telemetry.TrackTrace("Slow database response",
                   SeverityLevel.Warning,
                   new Dictionary<string,string> { {"database", db.ID} });

<span data-ttu-id="a44f6-293">I [Sök](app-insights-diagnostic-search.md), du kan sedan enkelt filtreras alla hello meddelanden om en viss allvarlighetsgrad som rör tooa viss databas.</span><span class="sxs-lookup"><span data-stu-id="a44f6-293">In [Search](app-insights-diagnostic-search.md), you can then easily filter out all hello messages of a particular severity level that relate tooa particular database.</span></span>


### <a name="traces-in-analytics"></a><span data-ttu-id="a44f6-294">Spåren i Analytics</span><span class="sxs-lookup"><span data-stu-id="a44f6-294">Traces in Analytics</span></span>

<span data-ttu-id="a44f6-295">I [Application Insights Analytics](app-insights-analytics.md), anropar tooTrackTrace visa i hello `traces` tabell.</span><span class="sxs-lookup"><span data-stu-id="a44f6-295">In [Application Insights Analytics](app-insights-analytics.md), calls tooTrackTrace show up in hello `traces` table.</span></span>

<span data-ttu-id="a44f6-296">Om [provtagning](app-insights-sampling.md) är i drift, hello itemCount egenskapen visar ett värde större än 1.</span><span class="sxs-lookup"><span data-stu-id="a44f6-296">If [sampling](app-insights-sampling.md) is in operation, hello itemCount property shows a value greater than 1.</span></span> <span data-ttu-id="a44f6-297">För exempel itemCount == 10 innebär att 10 anropar för`trackTrace()`, hello provtagning processen skickas endast ett av dem.</span><span class="sxs-lookup"><span data-stu-id="a44f6-297">For example itemCount==10 means that of 10 calls too`trackTrace()`, hello sampling process only transmitted one of them.</span></span> <span data-ttu-id="a44f6-298">tooget rätt antal spårning av anrop du bör använda därför kod som `traces | summarize sum(itemCount)`.</span><span class="sxs-lookup"><span data-stu-id="a44f6-298">tooget a correct count of trace calls, you should use therefore code such as `traces | summarize sum(itemCount)`.</span></span>

## <a name="trackdependency"></a><span data-ttu-id="a44f6-299">TrackDependency</span><span class="sxs-lookup"><span data-stu-id="a44f6-299">TrackDependency</span></span>
<span data-ttu-id="a44f6-300">Använd hello TrackDependency anropa tootrack hello svarstider och slutförandefrekvenser för anrop tooan externt kodavsnitt.</span><span class="sxs-lookup"><span data-stu-id="a44f6-300">Use hello TrackDependency call tootrack hello response times and success rates of calls tooan external piece of code.</span></span> <span data-ttu-id="a44f6-301">hello resultatet visas i hello beroendediagrammen i hello-portalen.</span><span class="sxs-lookup"><span data-stu-id="a44f6-301">hello results appear in hello dependency charts in hello portal.</span></span>

```C#
var success = false;
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

<span data-ttu-id="a44f6-302">Kom ihåg hello servern SDK innehåller en [beroende modulen](app-insights-asp-net-dependencies.md) som identifierar och spårar vissa beroendeanrop automatiskt – till exempel toodatabases och REST API: er.</span><span class="sxs-lookup"><span data-stu-id="a44f6-302">Remember that hello server SDKs include a [dependency module](app-insights-asp-net-dependencies.md) that discovers and tracks certain dependency calls automatically--for example, toodatabases and REST APIs.</span></span> <span data-ttu-id="a44f6-303">Du har tooinstall en agent på din server toomake hello modulen fungerar.</span><span class="sxs-lookup"><span data-stu-id="a44f6-303">You have tooinstall an agent on your server toomake hello module work.</span></span> <span data-ttu-id="a44f6-304">Du kan använda det här anropet om du vill tootrack anrop som hello automatisk spårning inte fånga eller om du inte vill tooinstall hello agent.</span><span class="sxs-lookup"><span data-stu-id="a44f6-304">You use this call if you want tootrack calls that hello automated tracking doesn't catch, or if you don't want tooinstall hello agent.</span></span>

<span data-ttu-id="a44f6-305">Redigera tooturn av hello standard beroende spårning modulen [ApplicationInsights.config](app-insights-configuration-with-applicationinsights-config.md) och ta bort hello-referens för`DependencyCollector.DependencyTrackingTelemetryModule`.</span><span class="sxs-lookup"><span data-stu-id="a44f6-305">tooturn off hello standard dependency-tracking module, edit [ApplicationInsights.config](app-insights-configuration-with-applicationinsights-config.md) and delete hello reference too`DependencyCollector.DependencyTrackingTelemetryModule`.</span></span>

### <a name="dependencies-in-analytics"></a><span data-ttu-id="a44f6-306">Beroenden i Analytics</span><span class="sxs-lookup"><span data-stu-id="a44f6-306">Dependencies in Analytics</span></span>

<span data-ttu-id="a44f6-307">I [Application Insights Analytics](app-insights-analytics.md), trackDependency anrop visas i hello `dependencies` tabell.</span><span class="sxs-lookup"><span data-stu-id="a44f6-307">In [Application Insights Analytics](app-insights-analytics.md), trackDependency calls show up in hello `dependencies` table.</span></span>

<span data-ttu-id="a44f6-308">Om [provtagning](app-insights-sampling.md) är i drift, hello itemCount egenskapen visar ett värde större än 1.</span><span class="sxs-lookup"><span data-stu-id="a44f6-308">If [sampling](app-insights-sampling.md) is in operation, hello itemCount property shows a value greater than 1.</span></span> <span data-ttu-id="a44f6-309">För exempel itemCount == 10 innebär att av 10 anrop tootrackDependency() hello provtagning processen bara skickas en av dem.</span><span class="sxs-lookup"><span data-stu-id="a44f6-309">For example itemCount==10 means that of 10 calls tootrackDependency(), hello sampling process only transmitted one of them.</span></span> <span data-ttu-id="a44f6-310">tooget rätt antal beroenden segmenterade av mål-komponent, använda koden som:</span><span class="sxs-lookup"><span data-stu-id="a44f6-310">tooget a correct count of dependencies segmented by target component, use code such as:</span></span>

```
dependencies | summarize sum(itemCount) by target
```

<span data-ttu-id="a44f6-311">tooassociate beroenden med relaterade begäranden, Använd en koppling:</span><span class="sxs-lookup"><span data-stu-id="a44f6-311">tooassociate dependencies with their related requests, use a join:</span></span>

```
dependencies
| join (requests) on operation_Id 
```

## <a name="flushing-data"></a><span data-ttu-id="a44f6-312">Tömmer data</span><span class="sxs-lookup"><span data-stu-id="a44f6-312">Flushing data</span></span>
<span data-ttu-id="a44f6-313">Normalt skickar hello SDK data alltid valt toominimize hello inverkan på hello användare.</span><span class="sxs-lookup"><span data-stu-id="a44f6-313">Normally, hello SDK sends data at times chosen toominimize hello impact on hello user.</span></span> <span data-ttu-id="a44f6-314">Dock i vissa fall kanske du vill tooflush hello buffert – till exempel om du använder hello SDK i ett program som stängs av.</span><span class="sxs-lookup"><span data-stu-id="a44f6-314">However, in some cases, you might want tooflush hello buffer--for example, if you are using hello SDK in an application that shuts down.</span></span>

<span data-ttu-id="a44f6-315">*C#*</span><span class="sxs-lookup"><span data-stu-id="a44f6-315">*C#*</span></span>

    telemetry.Flush();

    // Allow some time for flushing before shutdown.
    System.Threading.Thread.Sleep(1000);

<span data-ttu-id="a44f6-316">Observera att hello-funktionen är asynkron för hello [server telemetri kanal](https://www.nuget.org/packages/Microsoft.ApplicationInsights.WindowsServer.TelemetryChannel/).</span><span class="sxs-lookup"><span data-stu-id="a44f6-316">Note that hello function is asynchronous for hello [server telemetry channel](https://www.nuget.org/packages/Microsoft.ApplicationInsights.WindowsServer.TelemetryChannel/).</span></span>

## <a name="authenticated-users"></a><span data-ttu-id="a44f6-317">Autentiserade användare</span><span class="sxs-lookup"><span data-stu-id="a44f6-317">Authenticated users</span></span>
<span data-ttu-id="a44f6-318">I en webbapp identifieras användare (som standard) av cookies.</span><span class="sxs-lookup"><span data-stu-id="a44f6-318">In a web app, users are (by default) identified by cookies.</span></span> <span data-ttu-id="a44f6-319">En användare kan räknas mer än en gång om de har åtkomst till din app från en annan dator eller webbläsare, eller om de tar bort cookies.</span><span class="sxs-lookup"><span data-stu-id="a44f6-319">A user might be counted more than once if they access your app from a different machine or browser, or if they delete cookies.</span></span>

<span data-ttu-id="a44f6-320">Om användare loggar in tooyour app måste få du en mer korrekt antal genom att ange hello autentiserat användar-ID i hello webbläsare koden:</span><span class="sxs-lookup"><span data-stu-id="a44f6-320">If users sign in tooyour app, you can get a more accurate count by setting hello authenticated user ID in hello browser code:</span></span>

<span data-ttu-id="a44f6-321">*JavaScript*</span><span class="sxs-lookup"><span data-stu-id="a44f6-321">*JavaScript*</span></span>

```JS
// Called when my app has identified hello user.
function Authenticated(signInId) {
    var validatedId = signInId.replace(/[,;=| ]+/g, "_");
    appInsights.setAuthenticatedUserContext(validatedId);
    ...
}
```

<span data-ttu-id="a44f6-322">På en webbplats med ASP.NET MVC-program, till exempel:</span><span class="sxs-lookup"><span data-stu-id="a44f6-322">In an ASP.NET web MVC application, for example:</span></span>

<span data-ttu-id="a44f6-323">*Razor*</span><span class="sxs-lookup"><span data-stu-id="a44f6-323">*Razor*</span></span>

        @if (Request.IsAuthenticated)
        {
            <script>
                appInsights.setAuthenticatedUserContext("@User.Identity.Name
                   .Replace("\\", "\\\\")"
                   .replace(/[,;=| ]+/g, "_"));
            </script>
        }

<span data-ttu-id="a44f6-324">Det är inte nödvändigt toouse hello användares faktiska inloggningsnamn.</span><span class="sxs-lookup"><span data-stu-id="a44f6-324">It isn't necessary toouse hello user's actual sign-in name.</span></span> <span data-ttu-id="a44f6-325">Har bara toobe ett ID som är unikt toothat användare.</span><span class="sxs-lookup"><span data-stu-id="a44f6-325">It only has toobe an ID that is unique toothat user.</span></span> <span data-ttu-id="a44f6-326">Namnet får inte innehålla blanksteg eller något av tecknen hello `,;=|`.</span><span class="sxs-lookup"><span data-stu-id="a44f6-326">It must not include spaces or any of hello characters `,;=|`.</span></span>

<span data-ttu-id="a44f6-327">hello användar-ID är också i en sessionscookie och skickas toohello server.</span><span class="sxs-lookup"><span data-stu-id="a44f6-327">hello user ID is also set in a session cookie and sent toohello server.</span></span> <span data-ttu-id="a44f6-328">Om hello server SDK är installerat autentiserade hello användar-ID skickas som en del av hello kontextegenskaperna för både klient och server telemetri.</span><span class="sxs-lookup"><span data-stu-id="a44f6-328">If hello server SDK is installed, hello authenticated user ID is sent as part of hello context properties of both client and server telemetry.</span></span> <span data-ttu-id="a44f6-329">Därefter kan du filtrera och söka på den.</span><span class="sxs-lookup"><span data-stu-id="a44f6-329">You can then filter and search on it.</span></span>

<span data-ttu-id="a44f6-330">Om din app-grupper användare till konton, du kan också skicka en identifierare för hello-konto (med hello tecken samma begränsningar).</span><span class="sxs-lookup"><span data-stu-id="a44f6-330">If your app groups users into accounts, you can also pass an identifier for hello account (with hello same character restrictions).</span></span>

      appInsights.setAuthenticatedUserContext(validatedId, accountId);

<span data-ttu-id="a44f6-331">I [Metrics Explorer](app-insights-metrics-explorer.md), du kan skapa ett diagram som räknar **autentiserade användare,**, och **användarkonton**.</span><span class="sxs-lookup"><span data-stu-id="a44f6-331">In [Metrics Explorer](app-insights-metrics-explorer.md), you can create a chart that counts **Users, Authenticated**, and **User accounts**.</span></span>

<span data-ttu-id="a44f6-332">Du kan också [Sök](app-insights-diagnostic-search.md) för klienten datapunkter med specifika användarnamn och konton.</span><span class="sxs-lookup"><span data-stu-id="a44f6-332">You can also [search](app-insights-diagnostic-search.md) for client data points with specific user names and accounts.</span></span>

## <span data-ttu-id="a44f6-333"><a name="properties"></a>Filtrera, söka och segmentera dina data med hjälp av egenskaper</span><span class="sxs-lookup"><span data-stu-id="a44f6-333"><a name="properties"></a>Filtering, searching, and segmenting your data by using properties</span></span>
<span data-ttu-id="a44f6-334">Du kan bifoga egenskaper och mätningar tooyour händelser (och även toometrics, sidvisningar, undantag och andra telemetridata).</span><span class="sxs-lookup"><span data-stu-id="a44f6-334">You can attach properties and measurements tooyour events (and also toometrics, page views, exceptions, and other telemetry data).</span></span>

<span data-ttu-id="a44f6-335">*Egenskaper för* är strängvärden som du kan använda toofilter din telemetri i hello användningsrapporter.</span><span class="sxs-lookup"><span data-stu-id="a44f6-335">*Properties* are string values that you can use toofilter your telemetry in hello usage reports.</span></span> <span data-ttu-id="a44f6-336">Till exempel om din app erbjuder flera spel, kan du koppla hello namnet på hello spel tooeach händelsen så att du kan se vilka spel är mer populära.</span><span class="sxs-lookup"><span data-stu-id="a44f6-336">For example, if your app provides several games, you can attach hello name of hello game tooeach event so that you can see which games are more popular.</span></span>

<span data-ttu-id="a44f6-337">Det finns en gräns på 8192 på hello string-längden.</span><span class="sxs-lookup"><span data-stu-id="a44f6-337">There's a limit of 8192 on hello string length.</span></span> <span data-ttu-id="a44f6-338">(Om du vill toosend stora mängder data kan använda hello message-parameter för [TrackTrace](#track-trace).)</span><span class="sxs-lookup"><span data-stu-id="a44f6-338">(If you want toosend large chunks of data, use hello message parameter of [TrackTrace](#track-trace).)</span></span>

<span data-ttu-id="a44f6-339">*Mått* är numeriska värden som kan presenteras grafiskt.</span><span class="sxs-lookup"><span data-stu-id="a44f6-339">*Metrics* are numeric values that can be presented graphically.</span></span> <span data-ttu-id="a44f6-340">Du kanske exempelvis vill toosee om det finns en stegvis ökning hello resultat som din spelare uppnå.</span><span class="sxs-lookup"><span data-stu-id="a44f6-340">For example, you might want toosee if there's a gradual increase in hello scores that your gamers achieve.</span></span> <span data-ttu-id="a44f6-341">hello diagram kan vara åtskilda med hello egenskaper som skickas med hello händelse så att du kan få avgränsa eller staplade diagram för olika spel.</span><span class="sxs-lookup"><span data-stu-id="a44f6-341">hello graphs can be segmented by hello properties that are sent with hello event, so that you can get separate or stacked graphs for different games.</span></span>

<span data-ttu-id="a44f6-342">De ska vara större än eller lika med too0 för måttvärden toobe visas korrekt.</span><span class="sxs-lookup"><span data-stu-id="a44f6-342">For metric values toobe correctly displayed, they should be greater than or equal too0.</span></span>

<span data-ttu-id="a44f6-343">Det finns några [begränsning hello antalet egenskaper och egenskapsvärden mått](#limits) som du kan använda.</span><span class="sxs-lookup"><span data-stu-id="a44f6-343">There are some [limits on hello number of properties, property values, and metrics](#limits) that you can use.</span></span>

<span data-ttu-id="a44f6-344">*JavaScript*</span><span class="sxs-lookup"><span data-stu-id="a44f6-344">*JavaScript*</span></span>

    appInsights.trackEvent
      ("WinGame",
         // String properties:
         {Game: currentGame.name, Difficulty: currentGame.difficulty},
         // Numeric metrics:
         {Score: currentGame.score, Opponents: currentGame.opponentCount}
         );

    appInsights.trackPageView
        ("page name", "http://fabrikam.com/pageurl.html",
          // String properties:
         {Game: currentGame.name, Difficulty: currentGame.difficulty},
         // Numeric metrics:
         {Score: currentGame.score, Opponents: currentGame.opponentCount}
         );


<span data-ttu-id="a44f6-345">*C#*</span><span class="sxs-lookup"><span data-stu-id="a44f6-345">*C#*</span></span>

    // Set up some properties and metrics:
    var properties = new Dictionary <string, string>
       {{"game", currentGame.Name}, {"difficulty", currentGame.Difficulty}};
    var metrics = new Dictionary <string, double>
       {{"Score", currentGame.Score}, {"Opponents", currentGame.OpponentCount}};

    // Send hello event:
    telemetry.TrackEvent("WinGame", properties, metrics);


<span data-ttu-id="a44f6-346">*Visual Basic*</span><span class="sxs-lookup"><span data-stu-id="a44f6-346">*Visual Basic*</span></span>

    ' Set up some properties:
    Dim properties = New Dictionary (Of String, String)
    properties.Add("game", currentGame.Name)
    properties.Add("difficulty", currentGame.Difficulty)

    Dim metrics = New Dictionary (Of String, Double)
    metrics.Add("Score", currentGame.Score)
    metrics.Add("Opponents", currentGame.OpponentCount)

    ' Send hello event:
    telemetry.TrackEvent("WinGame", properties, metrics)


<span data-ttu-id="a44f6-347">*Java*</span><span class="sxs-lookup"><span data-stu-id="a44f6-347">*Java*</span></span>

    Map<String, String> properties = new HashMap<String, String>();
    properties.put("game", currentGame.getName());
    properties.put("difficulty", currentGame.getDifficulty());

    Map<String, Double> metrics = new HashMap<String, Double>();
    metrics.put("Score", currentGame.getScore());
    metrics.put("Opponents", currentGame.getOpponentCount());

    telemetry.trackEvent("WinGame", properties, metrics);


> [!NOTE]
> <span data-ttu-id="a44f6-348">Ta hand inte toolog personligt identifierbar information i egenskaperna.</span><span class="sxs-lookup"><span data-stu-id="a44f6-348">Take care not toolog personally identifiable information in properties.</span></span>
>
>

<span data-ttu-id="a44f6-349">*Om du använde mått*, öppna Metrics Explorer och välj hello mått hello **anpassad** grupp:</span><span class="sxs-lookup"><span data-stu-id="a44f6-349">*If you used metrics*, open Metrics Explorer and select hello metric from hello **Custom** group:</span></span>

![Öppna Metrics Explorer, väljer hello diagrammet och välj hello mått](./media/app-insights-api-custom-events-metrics/03-track-custom.png)

> [!NOTE]
> <span data-ttu-id="a44f6-351">Om din mått inte visas, eller om hello **anpassad** rubrik finns inte där, Stäng hello markeringen bladet och försök igen senare.</span><span class="sxs-lookup"><span data-stu-id="a44f6-351">If your metric doesn't appear, or if hello **Custom** heading isn't there, close hello selection blade and try again later.</span></span> <span data-ttu-id="a44f6-352">Mått kan ta en timme toobe samman via hello pipeline.</span><span class="sxs-lookup"><span data-stu-id="a44f6-352">Metrics can sometimes take an hour toobe aggregated through hello pipeline.</span></span>

<span data-ttu-id="a44f6-353">*Om du använde egenskaper och mått*, segmentera hello mått av hello-egenskapen:</span><span class="sxs-lookup"><span data-stu-id="a44f6-353">*If you used properties and metrics*, segment hello metric by hello property:</span></span>

![Ange gruppering och välj sedan hello egenskapen under Gruppera efter](./media/app-insights-api-custom-events-metrics/04-segment-metric-event.png)

<span data-ttu-id="a44f6-355">*I diagnostiska sökningen*, kan du visa hello egenskaper och mått för enskilda förekomster av en händelse.</span><span class="sxs-lookup"><span data-stu-id="a44f6-355">*In Diagnostic Search*, you can view hello properties and metrics of individual occurrences of an event.</span></span>

![Välj en instans och välj sedan ”...”](./media/app-insights-api-custom-events-metrics/appinsights-23-customevents-4.png)

<span data-ttu-id="a44f6-357">Använd hello **Sök** fältet toosee händelse förekomster som har ett visst egenskapsvärde.</span><span class="sxs-lookup"><span data-stu-id="a44f6-357">Use hello **Search** field toosee event occurrences that have a particular property value.</span></span>

![Skriv en term i sökrutan](./media/app-insights-api-custom-events-metrics/appinsights-23-customevents-5.png)

<span data-ttu-id="a44f6-359">[Mer information om sökuttryck](app-insights-diagnostic-search.md).</span><span class="sxs-lookup"><span data-stu-id="a44f6-359">[Learn more about search expressions](app-insights-diagnostic-search.md).</span></span>

### <a name="alternative-way-tooset-properties-and-metrics"></a><span data-ttu-id="a44f6-360">Alternativt sätt tooset egenskaper och mått</span><span class="sxs-lookup"><span data-stu-id="a44f6-360">Alternative way tooset properties and metrics</span></span>
<span data-ttu-id="a44f6-361">Om det är enklare, du kan samla in hello parametrar för en händelse i ett separat objekt:</span><span class="sxs-lookup"><span data-stu-id="a44f6-361">If it's more convenient, you can collect hello parameters of an event in a separate object:</span></span>

    var event = new EventTelemetry();

    event.Name = "WinGame";
    event.Metrics["processingTime"] = stopwatch.Elapsed.TotalMilliseconds;
    event.Properties["game"] = currentGame.Name;
    event.Properties["difficulty"] = currentGame.Difficulty;
    event.Metrics["Score"] = currentGame.Score;
    event.Metrics["Opponents"] = currentGame.Opponents.Length;

    telemetry.TrackEvent(event);

> [!WARNING]
> <span data-ttu-id="a44f6-362">Inte återanvända hello samma telemetri objektet instans (`event` i det här exemplet) toocall Track*() flera gånger.</span><span class="sxs-lookup"><span data-stu-id="a44f6-362">Don't reuse hello same telemetry item instance (`event` in this example) toocall Track*() multiple times.</span></span> <span data-ttu-id="a44f6-363">Detta kan orsaka telemetri toobe skickas med felaktig konfiguration.</span><span class="sxs-lookup"><span data-stu-id="a44f6-363">This may cause telemetry toobe sent with incorrect configuration.</span></span>
>
>

### <a name="custom-measurements-and-properties-in-analytics"></a><span data-ttu-id="a44f6-364">Anpassade mått och egenskaper i Analytics</span><span class="sxs-lookup"><span data-stu-id="a44f6-364">Custom measurements and properties in Analytics</span></span>

<span data-ttu-id="a44f6-365">I [Analytics](app-insights-analytics.md), anpassade mått och egenskaper visas i hello `customMeasurements` och `customDimensions` attribut för varje post telemetri.</span><span class="sxs-lookup"><span data-stu-id="a44f6-365">In [Analytics](app-insights-analytics.md), custom metrics and properties show in hello `customMeasurements` and `customDimensions` attributes of each telemetry record.</span></span>

<span data-ttu-id="a44f6-366">Till exempel om du har lagt till en egenskap med namnet ”spel” tooyour begärandetelemetri frågan räknar hello förekomster av olika värden för ”spel” och visa hello medelvärdet av hello anpassade mått ”resultat”:</span><span class="sxs-lookup"><span data-stu-id="a44f6-366">For example, if you have added a property named "game" tooyour request telemetry, this query counts hello occurrences of different values of "game", and show hello average of hello custom metric "score":</span></span>

```
requests
| summarize sum(itemCount), avg(todouble(customMeasurements.score)) by tostring(customDimensions.game) 
```

<span data-ttu-id="a44f6-367">Observera att:</span><span class="sxs-lookup"><span data-stu-id="a44f6-367">Notice that:</span></span>

* <span data-ttu-id="a44f6-368">När du extraherar ett värde från hello customDimensions eller customMeasurements JSON, den har dynamiska typ, och så måste du skicka den `tostring` eller `todouble`.</span><span class="sxs-lookup"><span data-stu-id="a44f6-368">When you extract a value from hello customDimensions or customMeasurements JSON, it has dynamic type, and so you must cast it `tostring` or `todouble`.</span></span>
* <span data-ttu-id="a44f6-369">tootake hänsyn hello möjligheten att [provtagning](app-insights-sampling.md), bör du använda `sum(itemCount)`, inte `count()`.</span><span class="sxs-lookup"><span data-stu-id="a44f6-369">tootake account of hello possibility of [sampling](app-insights-sampling.md), you should use `sum(itemCount)`, not `count()`.</span></span>



## <span data-ttu-id="a44f6-370"><a name="timed"></a>Tidsinställning händelser</span><span class="sxs-lookup"><span data-stu-id="a44f6-370"><a name="timed"></a> Timing events</span></span>
<span data-ttu-id="a44f6-371">Ibland vill du toochart hur lång tid det tar tooperform en åtgärd.</span><span class="sxs-lookup"><span data-stu-id="a44f6-371">Sometimes you want toochart how long it takes tooperform an action.</span></span> <span data-ttu-id="a44f6-372">Du kanske exempelvis vill tooknow hur länge användare ta tooconsider val i spel.</span><span class="sxs-lookup"><span data-stu-id="a44f6-372">For example, you might want tooknow how long users take tooconsider choices in a game.</span></span> <span data-ttu-id="a44f6-373">Du kan använda hello mätning parametern för denna.</span><span class="sxs-lookup"><span data-stu-id="a44f6-373">You can use hello measurement parameter for this.</span></span>

<span data-ttu-id="a44f6-374">*C#*</span><span class="sxs-lookup"><span data-stu-id="a44f6-374">*C#*</span></span>

    var stopwatch = System.Diagnostics.Stopwatch.StartNew();

    // ... perform hello timed action ...

    stopwatch.Stop();

    var metrics = new Dictionary <string, double>
       {{"processingTime", stopwatch.Elapsed.TotalMilliseconds}};

    // Set up some properties:
    var properties = new Dictionary <string, string>
       {{"signalSource", currentSignalSource.Name}};

    // Send hello event:
    telemetry.TrackEvent("SignalProcessed", properties, metrics);



## <span data-ttu-id="a44f6-375"><a name="defaults"></a>Standardegenskaperna för anpassad telemetri</span><span class="sxs-lookup"><span data-stu-id="a44f6-375"><a name="defaults"></a>Default properties for custom telemetry</span></span>
<span data-ttu-id="a44f6-376">Om du vill tooset egenskapen standardvärden för några av hello anpassade händelser som du skriver kan ange du dem i en TelemetryClient-instans.</span><span class="sxs-lookup"><span data-stu-id="a44f6-376">If you want tooset default property values for some of hello custom events that you write, you can set them in a TelemetryClient instance.</span></span> <span data-ttu-id="a44f6-377">De är anslutna tooevery telemetri objekt som skickas från klienten.</span><span class="sxs-lookup"><span data-stu-id="a44f6-377">They are attached tooevery telemetry item that's sent from that client.</span></span>

<span data-ttu-id="a44f6-378">*C#*</span><span class="sxs-lookup"><span data-stu-id="a44f6-378">*C#*</span></span>

    using Microsoft.ApplicationInsights.DataContracts;

    var gameTelemetry = new TelemetryClient();
    gameTelemetry.Context.Properties["Game"] = currentGame.Name;
    // Now all telemetry will automatically be sent with hello context property:
    gameTelemetry.TrackEvent("WinGame");

<span data-ttu-id="a44f6-379">*Visual Basic*</span><span class="sxs-lookup"><span data-stu-id="a44f6-379">*Visual Basic*</span></span>

    Dim gameTelemetry = New TelemetryClient()
    gameTelemetry.Context.Properties("Game") = currentGame.Name
    ' Now all telemetry will automatically be sent with hello context property:
    gameTelemetry.TrackEvent("WinGame")

<span data-ttu-id="a44f6-380">*Java*</span><span class="sxs-lookup"><span data-stu-id="a44f6-380">*Java*</span></span>

    import com.microsoft.applicationinsights.TelemetryClient;
    import com.microsoft.applicationinsights.TelemetryContext;
    ...


    TelemetryClient gameTelemetry = new TelemetryClient();
    TelemetryContext context = gameTelemetry.getContext();
    context.getProperties().put("Game", currentGame.Name);

    gameTelemetry.TrackEvent("WinGame");



<span data-ttu-id="a44f6-381">Enskilda telemetri anrop kan åsidosätta hello standardvärdena i sina egenskapen ordlistor.</span><span class="sxs-lookup"><span data-stu-id="a44f6-381">Individual telemetry calls can override hello default values in their property dictionaries.</span></span>

<span data-ttu-id="a44f6-382">*Web klienter för JavaScript*, [använder JavaScript telemetri initierare](#js-initializer).</span><span class="sxs-lookup"><span data-stu-id="a44f6-382">*For JavaScript web clients*, [use JavaScript telemetry initializers](#js-initializer).</span></span>

<span data-ttu-id="a44f6-383">*tooadd egenskaper tooall telemetri*, inklusive hello data från standard samling moduler [implementera `ITelemetryInitializer` ](app-insights-api-filtering-sampling.md#add-properties).</span><span class="sxs-lookup"><span data-stu-id="a44f6-383">*tooadd properties tooall telemetry*, including hello data from standard collection modules, [implement `ITelemetryInitializer`](app-insights-api-filtering-sampling.md#add-properties).</span></span>

## <a name="sampling-filtering-and-processing-telemetry"></a><span data-ttu-id="a44f6-384">Provtagning, filtrering och bearbetar telemetri</span><span class="sxs-lookup"><span data-stu-id="a44f6-384">Sampling, filtering, and processing telemetry</span></span>
<span data-ttu-id="a44f6-385">Du kan skriva kod tooprocess din telemetri innan den skickas från hello SDK.</span><span class="sxs-lookup"><span data-stu-id="a44f6-385">You can write code tooprocess your telemetry before it's sent from hello SDK.</span></span> <span data-ttu-id="a44f6-386">hello bearbetning innehåller data som skickas från hello standard telemetri moduler, till exempel HTTP request-samlingen och beroende samling.</span><span class="sxs-lookup"><span data-stu-id="a44f6-386">hello processing includes data that's sent from hello standard telemetry modules, such as HTTP request collection and dependency collection.</span></span>

<span data-ttu-id="a44f6-387">[Lägga till egenskaper](app-insights-api-filtering-sampling.md#add-properties) tootelemetry genom att implementera `ITelemetryInitializer`.</span><span class="sxs-lookup"><span data-stu-id="a44f6-387">[Add properties](app-insights-api-filtering-sampling.md#add-properties) tootelemetry by implementing `ITelemetryInitializer`.</span></span> <span data-ttu-id="a44f6-388">Du kan till exempel lägga till versionsnummer eller värden som beräknats från andra egenskaper.</span><span class="sxs-lookup"><span data-stu-id="a44f6-388">For example, you can add version numbers or values that are calculated from other properties.</span></span>

<span data-ttu-id="a44f6-389">[Filtrering](app-insights-api-filtering-sampling.md#filtering) kan ändra eller ta bort telemetri innan det skickas från hello SDK genom att implementera `ITelemetryProcesor`.</span><span class="sxs-lookup"><span data-stu-id="a44f6-389">[Filtering](app-insights-api-filtering-sampling.md#filtering) can modify or discard telemetry before it's sent from hello SDK by implementing `ITelemetryProcesor`.</span></span> <span data-ttu-id="a44f6-390">Du styr vilka skickas eller tas bort, men du har tooaccount för hello effekt på dina.</span><span class="sxs-lookup"><span data-stu-id="a44f6-390">You control what is sent or discarded, but you have tooaccount for hello effect on your metrics.</span></span> <span data-ttu-id="a44f6-391">Beroende på hur du ta bort objekt, kan du förlora hello möjlighet toonavigate mellan relaterade objekt.</span><span class="sxs-lookup"><span data-stu-id="a44f6-391">Depending on how you discard items, you might lose hello ability toonavigate between related items.</span></span>

<span data-ttu-id="a44f6-392">[Provtagning](app-insights-api-filtering-sampling.md) är en lösning för paketerade tooreduce hello mängden data som skickas från din app toohello portal.</span><span class="sxs-lookup"><span data-stu-id="a44f6-392">[Sampling](app-insights-api-filtering-sampling.md) is a packaged solution tooreduce hello volume of data that's sent from your app toohello portal.</span></span> <span data-ttu-id="a44f6-393">Detta sker utan att påverka hello visas mått.</span><span class="sxs-lookup"><span data-stu-id="a44f6-393">It does so without affecting hello displayed metrics.</span></span> <span data-ttu-id="a44f6-394">Och detta sker utan att påverka din möjlighet toodiagnose problem genom att navigera mellan relaterade objekt som undantag, förfrågningar och sidvyer.</span><span class="sxs-lookup"><span data-stu-id="a44f6-394">And it does so without affecting your ability toodiagnose problems by navigating between related items such as exceptions, requests, and page views.</span></span>

<span data-ttu-id="a44f6-395">[Läs mer](app-insights-api-filtering-sampling.md).</span><span class="sxs-lookup"><span data-stu-id="a44f6-395">[Learn more](app-insights-api-filtering-sampling.md).</span></span>

## <a name="disabling-telemetry"></a><span data-ttu-id="a44f6-396">Inaktivera telemetri</span><span class="sxs-lookup"><span data-stu-id="a44f6-396">Disabling telemetry</span></span>
<span data-ttu-id="a44f6-397">för*dynamiskt stoppa och starta* hello insamling och vidarebefordran av telemetri:</span><span class="sxs-lookup"><span data-stu-id="a44f6-397">too*dynamically stop and start* hello collection and transmission of telemetry:</span></span>

<span data-ttu-id="a44f6-398">*C#*</span><span class="sxs-lookup"><span data-stu-id="a44f6-398">*C#*</span></span>

```C#

    using  Microsoft.ApplicationInsights.Extensibility;

    TelemetryConfiguration.Active.DisableTelemetry = true;
```

<span data-ttu-id="a44f6-399">för*inaktivera valda standard insamlare*--exempelvis prestandaräknare, HTTP-begäranden eller beroenden – ta bort eller kommentera ut hello relevanta rader i [ApplicationInsights.config](app-insights-configuration-with-applicationinsights-config.md). Du kan göra detta, till exempel om du vill toosend TrackRequest data.</span><span class="sxs-lookup"><span data-stu-id="a44f6-399">too*disable selected standard collectors*--for example, performance counters, HTTP requests, or dependencies--delete or comment out hello relevant lines in [ApplicationInsights.config](app-insights-configuration-with-applicationinsights-config.md). You can do this, for example, if you want toosend your own TrackRequest data.</span></span>

## <span data-ttu-id="a44f6-400"><a name="debug"></a>Utvecklarläge</span><span class="sxs-lookup"><span data-stu-id="a44f6-400"><a name="debug"></a>Developer mode</span></span>
<span data-ttu-id="a44f6-401">Vid felsökning, är det användbart toohave din telemetri expedierade via hello pipeline så att du kan se resultatet direkt.</span><span class="sxs-lookup"><span data-stu-id="a44f6-401">During debugging, it's useful toohave your telemetry expedited through hello pipeline so that you can see results immediately.</span></span> <span data-ttu-id="a44f6-402">Du kan också hämta meddelandena som hjälper dig spåra eventuella problem med hello telemetri.</span><span class="sxs-lookup"><span data-stu-id="a44f6-402">You also get additional messages that help you trace any problems with hello telemetry.</span></span> <span data-ttu-id="a44f6-403">Stänga av den i produktion, eftersom det kan påverka din app.</span><span class="sxs-lookup"><span data-stu-id="a44f6-403">Switch it off in production, because it may slow down your app.</span></span>

<span data-ttu-id="a44f6-404">*C#*</span><span class="sxs-lookup"><span data-stu-id="a44f6-404">*C#*</span></span>

    TelemetryConfiguration.Active.TelemetryChannel.DeveloperMode = true;

<span data-ttu-id="a44f6-405">*Visual Basic*</span><span class="sxs-lookup"><span data-stu-id="a44f6-405">*Visual Basic*</span></span>

    TelemetryConfiguration.Active.TelemetryChannel.DeveloperMode = True


## <span data-ttu-id="a44f6-406"><a name="ikey"></a>Ange hello instrumentation nyckel för valda anpassad telemetri</span><span class="sxs-lookup"><span data-stu-id="a44f6-406"><a name="ikey"></a> Setting hello instrumentation key for selected custom telemetry</span></span>
<span data-ttu-id="a44f6-407">*C#*</span><span class="sxs-lookup"><span data-stu-id="a44f6-407">*C#*</span></span>

    var telemetry = new TelemetryClient();
    telemetry.InstrumentationKey = "---my key---";
    // ...


## <span data-ttu-id="a44f6-408"><a name="dynamic-ikey"></a>Dynamisk instrumentation nyckel</span><span class="sxs-lookup"><span data-stu-id="a44f6-408"><a name="dynamic-ikey"></a> Dynamic instrumentation key</span></span>
<span data-ttu-id="a44f6-409">tooavoid blandas in telemetri från utvecklings-, test- och produktionsmiljöer, kan du [skapa separata Application Insights-resurser](app-insights-create-new-resource.md) och ändra deras nycklar, beroende på hello-miljö.</span><span class="sxs-lookup"><span data-stu-id="a44f6-409">tooavoid mixing up telemetry from development, test, and production environments, you can [create separate Application Insights resources](app-insights-create-new-resource.md) and change their keys, depending on hello environment.</span></span>

<span data-ttu-id="a44f6-410">I stället för att hämta hello instrumentation nyckeln från hello konfigurationsfil kan du ange den i din kod.</span><span class="sxs-lookup"><span data-stu-id="a44f6-410">Instead of getting hello instrumentation key from hello configuration file, you can set it in your code.</span></span> <span data-ttu-id="a44f6-411">Ange hello nyckeln i en initieringsmetod, till exempel global.aspx.cs i en ASP.NET-tjänst:</span><span class="sxs-lookup"><span data-stu-id="a44f6-411">Set hello key in an initialization method, such as global.aspx.cs in an ASP.NET service:</span></span>

<span data-ttu-id="a44f6-412">*C#*</span><span class="sxs-lookup"><span data-stu-id="a44f6-412">*C#*</span></span>

    protected void Application_Start()
    {
      Microsoft.ApplicationInsights.Extensibility.
        TelemetryConfiguration.Active.InstrumentationKey =
          // - for example -
          WebConfigurationManager.Settings["ikey"];
      ...

<span data-ttu-id="a44f6-413">*JavaScript*</span><span class="sxs-lookup"><span data-stu-id="a44f6-413">*JavaScript*</span></span>

    appInsights.config.instrumentationKey = myKey;



<span data-ttu-id="a44f6-414">Du kanske vill tooset i webbsidor, den från hello webbserverns tillstånd i stället kodning bokstavligt hello skript.</span><span class="sxs-lookup"><span data-stu-id="a44f6-414">In webpages, you might want tooset it from hello web server's state, rather than coding it literally into hello script.</span></span> <span data-ttu-id="a44f6-415">Till exempel i en webbsida som skapats i en ASP.NET-app:</span><span class="sxs-lookup"><span data-stu-id="a44f6-415">For example, in a webpage generated in an ASP.NET app:</span></span>

<span data-ttu-id="a44f6-416">*JavaScript i Razor*</span><span class="sxs-lookup"><span data-stu-id="a44f6-416">*JavaScript in Razor*</span></span>

    <script type="text/javascript">
    // Standard Application Insights webpage script:
    var appInsights = window.appInsights || function(config){ ...
    // Modify this part:
    }({instrumentationKey:  
      // Generate from server property:
      @Microsoft.ApplicationInsights.Extensibility.
         TelemetryConfiguration.Active.InstrumentationKey"
    }) // ...


## <a name="telemetrycontext"></a><span data-ttu-id="a44f6-417">TelemetryContext</span><span class="sxs-lookup"><span data-stu-id="a44f6-417">TelemetryContext</span></span>
<span data-ttu-id="a44f6-418">TelemetryClient har en kontextegenskap som innehåller värden som skickas tillsammans med alla telemetridata.</span><span class="sxs-lookup"><span data-stu-id="a44f6-418">TelemetryClient has a Context property, which contains values that are sent along with all telemetry data.</span></span> <span data-ttu-id="a44f6-419">De normalt har angetts av hello standard telemetri moduler, men du kan också ange dem själv.</span><span class="sxs-lookup"><span data-stu-id="a44f6-419">They are normally set by hello standard telemetry modules, but you can also set them yourself.</span></span> <span data-ttu-id="a44f6-420">Exempel:</span><span class="sxs-lookup"><span data-stu-id="a44f6-420">For example:</span></span>

    telemetry.Context.Operation.Name = "MyOperationName";

<span data-ttu-id="a44f6-421">Om du anger dessa värden själv, ta bort hello relevanta raden från [ApplicationInsights.config](app-insights-configuration-with-applicationinsights-config.md), så att dina värden och standardvärden som hello inte bli förvirrad.</span><span class="sxs-lookup"><span data-stu-id="a44f6-421">If you set any of these values yourself, consider removing hello relevant line from [ApplicationInsights.config](app-insights-configuration-with-applicationinsights-config.md), so that your values and hello standard values don't get confused.</span></span>

* <span data-ttu-id="a44f6-422">**Komponenten**: hello appen och dess version.</span><span class="sxs-lookup"><span data-stu-id="a44f6-422">**Component**: hello app and its version.</span></span>
* <span data-ttu-id="a44f6-423">**Enheten**: Data om hello enheten där hello appen körs.</span><span class="sxs-lookup"><span data-stu-id="a44f6-423">**Device**: Data about hello device where hello app is running.</span></span> <span data-ttu-id="a44f6-424">(I web apps är detta hello server eller klientenheten som hello telemetri skickas från.)</span><span class="sxs-lookup"><span data-stu-id="a44f6-424">(In web apps, this is hello server or client device that hello telemetry is sent from.)</span></span>
* <span data-ttu-id="a44f6-425">**InstrumentationKey**: hello Application Insights-resurs i Azure var hello telemetri visas.</span><span class="sxs-lookup"><span data-stu-id="a44f6-425">**InstrumentationKey**: hello Application Insights resource in Azure where hello telemetry appear.</span></span> <span data-ttu-id="a44f6-426">Det hämtas vanligtvis från ApplicationInsights.config.</span><span class="sxs-lookup"><span data-stu-id="a44f6-426">It's usually picked up from ApplicationInsights.config.</span></span>
* <span data-ttu-id="a44f6-427">**Plats**: hello hello enhetens geografiska plats.</span><span class="sxs-lookup"><span data-stu-id="a44f6-427">**Location**: hello geographic location of hello device.</span></span>
* <span data-ttu-id="a44f6-428">**Åtgärden**: I web apps hello aktuella HTTP-begäran.</span><span class="sxs-lookup"><span data-stu-id="a44f6-428">**Operation**: In web apps, hello current HTTP request.</span></span> <span data-ttu-id="a44f6-429">I andra apptyper du vill kan du ange den här toogroup händelser tillsammans.</span><span class="sxs-lookup"><span data-stu-id="a44f6-429">In other app types, you can set this toogroup events together.</span></span>
  * <span data-ttu-id="a44f6-430">**ID**: ett genererat värde som korreleras olika händelser så att när du granska alla händelser i diagnostiska sökning, kan du hitta relaterade objekt.</span><span class="sxs-lookup"><span data-stu-id="a44f6-430">**Id**: A generated value that correlates different events, so that when you inspect any event in Diagnostic Search, you can find related items.</span></span>
  * <span data-ttu-id="a44f6-431">**Namnet**: ett ID, vanligtvis hello URL för hello HTTP-begäran.</span><span class="sxs-lookup"><span data-stu-id="a44f6-431">**Name**: An identifier, usually hello URL of hello HTTP request.</span></span>
  * <span data-ttu-id="a44f6-432">**SyntheticSource**: om inte null eller tomt, en sträng som anger att hello datakälla för hello-begäran har identifierats som ett program eller webbplats test.</span><span class="sxs-lookup"><span data-stu-id="a44f6-432">**SyntheticSource**: If not null or empty, a string that indicates that hello source of hello request has been identified as a robot or web test.</span></span> <span data-ttu-id="a44f6-433">Som standard är den inte beräkningar i Metrics Explorer.</span><span class="sxs-lookup"><span data-stu-id="a44f6-433">By default, it is excluded from calculations in Metrics Explorer.</span></span>
* <span data-ttu-id="a44f6-434">**Egenskaper för**: egenskaper som skickas med alla telemetridata.</span><span class="sxs-lookup"><span data-stu-id="a44f6-434">**Properties**: Properties that are sent with all telemetry data.</span></span> <span data-ttu-id="a44f6-435">Den kan åsidosättas i enskilda spåra * anrop.</span><span class="sxs-lookup"><span data-stu-id="a44f6-435">It can be overridden in individual Track* calls.</span></span>
* <span data-ttu-id="a44f6-436">**Sessionen**: hello användarens session.</span><span class="sxs-lookup"><span data-stu-id="a44f6-436">**Session**: hello user's session.</span></span> <span data-ttu-id="a44f6-437">hello-ID har angetts tooa genereras-värde som ändras när hello användaren inte har active för en stund.</span><span class="sxs-lookup"><span data-stu-id="a44f6-437">hello ID is set tooa generated value, which is changed when hello user has not been active for a while.</span></span>
* <span data-ttu-id="a44f6-438">**Användaren**: användarinformation.</span><span class="sxs-lookup"><span data-stu-id="a44f6-438">**User**: User information.</span></span>

## <a name="limits"></a><span data-ttu-id="a44f6-439">Begränsningar</span><span class="sxs-lookup"><span data-stu-id="a44f6-439">Limits</span></span>
[!INCLUDE [application-insights-limits](../../includes/application-insights-limits.md)]

<span data-ttu-id="a44f6-440">tooavoid träffa hello-gräns för överföringshastigheten av data, Använd [provtagning](app-insights-sampling.md).</span><span class="sxs-lookup"><span data-stu-id="a44f6-440">tooavoid hitting hello data rate limit, use [sampling](app-insights-sampling.md).</span></span>

<span data-ttu-id="a44f6-441">toodetermine hur länge data sparas finns [datalagring och sekretess](app-insights-data-retention-privacy.md).</span><span class="sxs-lookup"><span data-stu-id="a44f6-441">toodetermine how long data is kept, see [Data retention and privacy](app-insights-data-retention-privacy.md).</span></span>

## <a name="reference-docs"></a><span data-ttu-id="a44f6-442">Referensdokument</span><span class="sxs-lookup"><span data-stu-id="a44f6-442">Reference docs</span></span>
* [<span data-ttu-id="a44f6-443">ASP.NET-referens</span><span class="sxs-lookup"><span data-stu-id="a44f6-443">ASP.NET reference</span></span>](https://msdn.microsoft.com/library/dn817570.aspx)
* [<span data-ttu-id="a44f6-444">Referens för Java</span><span class="sxs-lookup"><span data-stu-id="a44f6-444">Java reference</span></span>](http://dl.windowsazure.com/applicationinsights/javadoc/)
* [<span data-ttu-id="a44f6-445">JavaScript-referens</span><span class="sxs-lookup"><span data-stu-id="a44f6-445">JavaScript reference</span></span>](https://github.com/Microsoft/ApplicationInsights-JS/blob/master/API-reference.md)
* [<span data-ttu-id="a44f6-446">Android SDK</span><span class="sxs-lookup"><span data-stu-id="a44f6-446">Android SDK</span></span>](https://github.com/Microsoft/ApplicationInsights-Android)
* [<span data-ttu-id="a44f6-447">iOS SDK</span><span class="sxs-lookup"><span data-stu-id="a44f6-447">iOS SDK</span></span>](https://github.com/Microsoft/ApplicationInsights-iOS)

## <a name="sdk-code"></a><span data-ttu-id="a44f6-448">SDK-kod</span><span class="sxs-lookup"><span data-stu-id="a44f6-448">SDK code</span></span>
* [<span data-ttu-id="a44f6-449">ASP.NET Core SDK</span><span class="sxs-lookup"><span data-stu-id="a44f6-449">ASP.NET Core SDK</span></span>](https://github.com/Microsoft/ApplicationInsights-dotnet)
* [<span data-ttu-id="a44f6-450">ASP.NET 5</span><span class="sxs-lookup"><span data-stu-id="a44f6-450">ASP.NET 5</span></span>](https://github.com/Microsoft/ApplicationInsights-aspnet5)
* [<span data-ttu-id="a44f6-451">Windows Server-paket</span><span class="sxs-lookup"><span data-stu-id="a44f6-451">Windows Server packages</span></span>](https://github.com/Microsoft/applicationInsights-dotnet-server)
* [<span data-ttu-id="a44f6-452">Java SDK</span><span class="sxs-lookup"><span data-stu-id="a44f6-452">Java SDK</span></span>](https://github.com/Microsoft/ApplicationInsights-Java)
* [<span data-ttu-id="a44f6-453">JavaScript SDK</span><span class="sxs-lookup"><span data-stu-id="a44f6-453">JavaScript SDK</span></span>](https://github.com/Microsoft/ApplicationInsights-JS)
* [<span data-ttu-id="a44f6-454">Alla plattformar</span><span class="sxs-lookup"><span data-stu-id="a44f6-454">All platforms</span></span>](https://github.com/Microsoft?utf8=%E2%9C%93&query=applicationInsights)

## <a name="questions"></a><span data-ttu-id="a44f6-455">Frågor</span><span class="sxs-lookup"><span data-stu-id="a44f6-455">Questions</span></span>
* <span data-ttu-id="a44f6-456">*Vilka undantag kan Track_() anrop utlösa?*</span><span class="sxs-lookup"><span data-stu-id="a44f6-456">*What exceptions might Track_() calls throw?*</span></span>

    <span data-ttu-id="a44f6-457">Ingen.</span><span class="sxs-lookup"><span data-stu-id="a44f6-457">None.</span></span> <span data-ttu-id="a44f6-458">Du behöver inte toowrap dem i försök catch-satser.</span><span class="sxs-lookup"><span data-stu-id="a44f6-458">You don't need toowrap them in try-catch clauses.</span></span> <span data-ttu-id="a44f6-459">Om hello SDK påträffar problem, loggas meddelanden i hello debug konsolens utdata och – om hello få meddelanden via--i diagnostiska sökningen.</span><span class="sxs-lookup"><span data-stu-id="a44f6-459">If hello SDK encounters problems, it will log messages in hello debug console output and--if hello messages get through--in Diagnostic Search.</span></span>
* <span data-ttu-id="a44f6-460">*Finns det en REST API tooget data från hello portal?*</span><span class="sxs-lookup"><span data-stu-id="a44f6-460">*Is there a REST API tooget data from hello portal?*</span></span>

    <span data-ttu-id="a44f6-461">Ja, hello [API för dataåtkomst](https://dev.applicationinsights.io/).</span><span class="sxs-lookup"><span data-stu-id="a44f6-461">Yes, hello [data access API](https://dev.applicationinsights.io/).</span></span> <span data-ttu-id="a44f6-462">Andra sätt tooextract data omfattar [exportera från Analytics tooPower BI](app-insights-export-power-bi.md) och [löpande export](app-insights-export-telemetry.md).</span><span class="sxs-lookup"><span data-stu-id="a44f6-462">Other ways tooextract data include [export from Analytics tooPower BI](app-insights-export-power-bi.md) and [continuous export](app-insights-export-telemetry.md).</span></span>

## <span data-ttu-id="a44f6-463"><a name="next"></a>Nästa steg</span><span class="sxs-lookup"><span data-stu-id="a44f6-463"><a name="next"></a>Next steps</span></span>
* [<span data-ttu-id="a44f6-464">Sök händelser och loggar</span><span class="sxs-lookup"><span data-stu-id="a44f6-464">Search events and logs</span></span>](app-insights-diagnostic-search.md)

* [<span data-ttu-id="a44f6-465">Felsökning</span><span class="sxs-lookup"><span data-stu-id="a44f6-465">Troubleshooting</span></span>](app-insights-troubleshoot-faq.md)


