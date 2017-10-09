---
title: "aaaLive mått ström med anpassade mått och diagnostik i Azure Application Insights | Microsoft Docs"
description: "Övervaka ditt webbprogram i realtid med anpassade mått och diagnostisera problem med en levande feed fel, spårningar och händelser."
services: application-insights
documentationcenter: 
author: SoubhagyaDash
manager: carmonm
ms.assetid: 1f471176-38f3-40b3-bc6d-3f47d0cbaaa2
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 05/24/2017
ms.author: bwren
ms.openlocfilehash: 68ddfbf387379ea778c20280c4ec96baa06d3273
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="live-metrics-stream-monitor--diagnose-with-1-second-latency"></a><span data-ttu-id="c3238-103">Direktsänd dataström med mått: Övervaka och diagnostisera med 1 sekund svarstid</span><span class="sxs-lookup"><span data-stu-id="c3238-103">Live Metrics Stream: Monitor & Diagnose with 1-second latency</span></span> 

<span data-ttu-id="c3238-104">Avsökning hello pulsslagsstatus centralt i ditt live, i produktion webbprogram med hjälp av mätvärden för Liveströmning från [Programinsikter](app-insights-overview.md).</span><span class="sxs-lookup"><span data-stu-id="c3238-104">Probe hello beating heart of your live, in-production web application by using Live Metrics Stream from [Application Insights](app-insights-overview.md).</span></span> <span data-ttu-id="c3238-105">Välj och filtrera mått och prestanda räknare toowatch i realtid, utan någon störningar tooyour tjänst.</span><span class="sxs-lookup"><span data-stu-id="c3238-105">Select and filter metrics and performance counters toowatch in real time, without any disturbance tooyour service.</span></span> <span data-ttu-id="c3238-106">Kontrollera stackspår från exemplet misslyckades begäranden och undantag.</span><span class="sxs-lookup"><span data-stu-id="c3238-106">Inspect stack traces from sample failed requests and exceptions.</span></span> <span data-ttu-id="c3238-107">Tillsammans med [Profiler](app-insights-profiler.md), [ögonblicksbild felsökare](app-insights-snapshot-debugger.md), och [prestandatester](app-insights-monitor-web-app-availability.md#performance-tests), direktsänd dataström med mått ger ett kraftfullt och icke-inkräktande diagnostikverktyg för webbplatsen live.</span><span class="sxs-lookup"><span data-stu-id="c3238-107">Together with [Profiler](app-insights-profiler.md), [Snapshot debugger](app-insights-snapshot-debugger.md), and [performance testing](app-insights-monitor-web-app-availability.md#performance-tests),  Live Metrics Stream provides a powerful and non-invasive diagnostic tool for your live web site.</span></span>

<span data-ttu-id="c3238-108">Med direktsänd dataström med mått kan du:</span><span class="sxs-lookup"><span data-stu-id="c3238-108">With Live Metrics Stream, you can:</span></span>

* <span data-ttu-id="c3238-109">Verifiera en korrigering medan släpps, genom att titta på prestanda och fel antal.</span><span class="sxs-lookup"><span data-stu-id="c3238-109">Validate a fix while it is released, by watching performance and failure counts.</span></span>
* <span data-ttu-id="c3238-110">Titta på hello effekten av testa belastningar och diagnostisera problem live.</span><span class="sxs-lookup"><span data-stu-id="c3238-110">Watch hello effect of test loads, and diagnose issues live.</span></span> 
* <span data-ttu-id="c3238-111">Fokusera på specifika test sessioner eller filtrera bort kända problem genom att välja och filtrering hello mått som du vill toowatch.</span><span class="sxs-lookup"><span data-stu-id="c3238-111">Focus on particular test sessions or filter out known issues, by selecting and filtering hello metrics you want toowatch.</span></span>
* <span data-ttu-id="c3238-112">Hämta undantag spårningar när de görs.</span><span class="sxs-lookup"><span data-stu-id="c3238-112">Get exception traces as they happen.</span></span>
* <span data-ttu-id="c3238-113">Experiment med filter toofind hello relevant KPI: er.</span><span class="sxs-lookup"><span data-stu-id="c3238-113">Experiment with filters toofind hello most relevant KPIs.</span></span>
* <span data-ttu-id="c3238-114">Övervaka alla Windows prestanda räknaren live.</span><span class="sxs-lookup"><span data-stu-id="c3238-114">Monitor any Windows performance counter live.</span></span>
* <span data-ttu-id="c3238-115">Enkelt identifiera en server som har problem och filtrera alla hello KPI/live feed toojust servern.</span><span class="sxs-lookup"><span data-stu-id="c3238-115">Easily identify a server that is having issues, and filter all hello KPI/live feed toojust that server.</span></span>

<span data-ttu-id="c3238-116">[![Live mått direktuppspelning av video](./media/app-insights-live-stream/youtube.png)](https://www.youtube.com/watch?v=zqfHf1Oi5PY)</span><span class="sxs-lookup"><span data-stu-id="c3238-116">[![Live Metrics Stream video](./media/app-insights-live-stream/youtube.png)](https://www.youtube.com/watch?v=zqfHf1Oi5PY)</span></span>

<span data-ttu-id="c3238-117">Direktsänd dataström med mått är tillgänglig på ASP.NET-appar som körs lokalt eller i hello molnet.</span><span class="sxs-lookup"><span data-stu-id="c3238-117">Live Metrics Stream is currently available on ASP.NET apps running on-premises or in hello Cloud.</span></span> 

## <a name="get-started"></a><span data-ttu-id="c3238-118">Kom igång</span><span class="sxs-lookup"><span data-stu-id="c3238-118">Get started</span></span>

1. <span data-ttu-id="c3238-119">Om du inte gjort ännu [installerat Application Insights](app-insights-asp-net.md) i ditt webbprogram för ASP.NET eller [Windows server-app](app-insights-windows-services.md), göra det nu.</span><span class="sxs-lookup"><span data-stu-id="c3238-119">If you haven't yet [installed Application Insights](app-insights-asp-net.md) in your ASP.NET web app or [Windows server app](app-insights-windows-services.md), do that now.</span></span> 
2. <span data-ttu-id="c3238-120">**Senaste uppdateringsversionen för toohello** av hello Application Insights-paketet.</span><span class="sxs-lookup"><span data-stu-id="c3238-120">**Update toohello latest version** of hello Application Insights package.</span></span> <span data-ttu-id="c3238-121">Högerklicka på projektet i Visual Studio och välj **hantera Nuget-paket**.</span><span class="sxs-lookup"><span data-stu-id="c3238-121">In Visual Studio, right-click your project and choose **Manage Nuget packages**.</span></span> <span data-ttu-id="c3238-122">Öppna hello **uppdateringar** markerar **inkludera förhandsversion**, och Välj alla hello Microsoft.ApplicationInsights.* paket.</span><span class="sxs-lookup"><span data-stu-id="c3238-122">Open hello **Updates** tab, check **Include prerelease**, and select all hello Microsoft.ApplicationInsights.* packages.</span></span>

    <span data-ttu-id="c3238-123">Distribuera om din app.</span><span class="sxs-lookup"><span data-stu-id="c3238-123">Redeploy your app.</span></span>

3. <span data-ttu-id="c3238-124">I hello [Azure-portalen](https://portal.azure.com), öppna hello Application Insights-resurs för din app och öppna sedan direktsänd dataström.</span><span class="sxs-lookup"><span data-stu-id="c3238-124">In hello [Azure portal](https://portal.azure.com), open hello Application Insights resource for your app, and then open Live Stream.</span></span>

4. <span data-ttu-id="c3238-125">[Säker hello kontrollkanal](#secure-the-control-channel) om du kan använda känsliga data, till exempel Kundnamn i filter.</span><span class="sxs-lookup"><span data-stu-id="c3238-125">[Secure hello control channel](#secure-the-control-channel) if you might use sensitive data such as customer names in your filters.</span></span>


![Klicka på direktsänd dataström i hello översikt bladet](./media/app-insights-live-stream/live-stream-2.png)

### <a name="no-data-check-your-server-firewall"></a><span data-ttu-id="c3238-127">Ser du inga data?</span><span class="sxs-lookup"><span data-stu-id="c3238-127">No data?</span></span> <span data-ttu-id="c3238-128">Kontrollera server-brandvägg</span><span class="sxs-lookup"><span data-stu-id="c3238-128">Check your server firewall</span></span>

<span data-ttu-id="c3238-129">Kontrollera hello [utgående portar för Liveströmning mått](app-insights-ip-addresses.md#outgoing-ports) är öppna i hello brandväggen för dina servrar.</span><span class="sxs-lookup"><span data-stu-id="c3238-129">Check hello [outgoing ports for Live Metrics Stream](app-insights-ip-addresses.md#outgoing-ports) are open in hello firewall of your servers.</span></span> 


## <a name="how-does-live-metrics-stream-differ-from-metrics-explorer-and-analytics"></a><span data-ttu-id="c3238-130">Hur skiljer sig direktsänd dataström med mått från Metrics Explorer och Analytics?</span><span class="sxs-lookup"><span data-stu-id="c3238-130">How does Live Metrics Stream differ from Metrics Explorer and Analytics?</span></span>

| |<span data-ttu-id="c3238-131">Direktsänd ström</span><span class="sxs-lookup"><span data-stu-id="c3238-131">Live Stream</span></span> | <span data-ttu-id="c3238-132">Metrics Explorer och analys</span><span class="sxs-lookup"><span data-stu-id="c3238-132">Metrics Explorer and Analytics</span></span> |
|---|---|---|
|<span data-ttu-id="c3238-133">Svarstid</span><span class="sxs-lookup"><span data-stu-id="c3238-133">Latency</span></span>|<span data-ttu-id="c3238-134">Data som visas i en sekund</span><span class="sxs-lookup"><span data-stu-id="c3238-134">Data displayed within one second</span></span>|<span data-ttu-id="c3238-135">Visar det sammanlagda resultatet minuter</span><span class="sxs-lookup"><span data-stu-id="c3238-135">Aggregated over minutes</span></span>|
|<span data-ttu-id="c3238-136">Inget bevarande</span><span class="sxs-lookup"><span data-stu-id="c3238-136">No retention</span></span>|<span data-ttu-id="c3238-137">Data kvarstår medan den är i hello diagram och sedan tas bort</span><span class="sxs-lookup"><span data-stu-id="c3238-137">Data persists while it's on hello chart, and is then discarded</span></span>|[<span data-ttu-id="c3238-138">Data som lagras i 90 dagar</span><span class="sxs-lookup"><span data-stu-id="c3238-138">Data retained for 90 days</span></span>](app-insights-data-retention-privacy.md#how-long-is-the-data-kept)|
|<span data-ttu-id="c3238-139">På begäran</span><span class="sxs-lookup"><span data-stu-id="c3238-139">On demand</span></span>|<span data-ttu-id="c3238-140">Data strömmas När du öppnar Live mått</span><span class="sxs-lookup"><span data-stu-id="c3238-140">Data is streamed while you open Live Metrics</span></span>|<span data-ttu-id="c3238-141">Informationen skickas när hello SDK är installerat och aktiverat</span><span class="sxs-lookup"><span data-stu-id="c3238-141">Data is sent whenever hello SDK is installed and enabled</span></span>|
|<span data-ttu-id="c3238-142">Kostnadsfri</span><span class="sxs-lookup"><span data-stu-id="c3238-142">Free</span></span>|<span data-ttu-id="c3238-143">Det kostar inget direktsänd dataström med data</span><span class="sxs-lookup"><span data-stu-id="c3238-143">There is no charge for Live Stream data</span></span>|<span data-ttu-id="c3238-144">Ämnesnamn för[priser](app-insights-pricing.md)</span><span class="sxs-lookup"><span data-stu-id="c3238-144">Subject too[pricing](app-insights-pricing.md)</span></span>
|<span data-ttu-id="c3238-145">Samling</span><span class="sxs-lookup"><span data-stu-id="c3238-145">Sampling</span></span>|<span data-ttu-id="c3238-146">Alla valda mått och räknare överförs.</span><span class="sxs-lookup"><span data-stu-id="c3238-146">All selected metrics and counters are transmitted.</span></span> <span data-ttu-id="c3238-147">Fel och stackspår samplas.</span><span class="sxs-lookup"><span data-stu-id="c3238-147">Failures and stack traces are sampled.</span></span> <span data-ttu-id="c3238-148">TelemetryProcessors tillämpas inte.</span><span class="sxs-lookup"><span data-stu-id="c3238-148">TelemetryProcessors are not applied.</span></span>|<span data-ttu-id="c3238-149">Händelser kan vara [provtagning](app-insights-api-filtering-sampling.md)</span><span class="sxs-lookup"><span data-stu-id="c3238-149">Events may be [sampled](app-insights-api-filtering-sampling.md)</span></span>|
|<span data-ttu-id="c3238-150">Kontrollkanal</span><span class="sxs-lookup"><span data-stu-id="c3238-150">Control channel</span></span>|<span data-ttu-id="c3238-151">Filter kontrollen signaler skickas toohello SDK.</span><span class="sxs-lookup"><span data-stu-id="c3238-151">Filter control signals are sent toohello SDK.</span></span> <span data-ttu-id="c3238-152">Vi rekommenderar att du [skydda den här kanalen](#secure-channel).</span><span class="sxs-lookup"><span data-stu-id="c3238-152">We recommend you [secure this channel](#secure-channel).</span></span>|<span data-ttu-id="c3238-153">Kommunikationen är enkelriktat toohello portal</span><span class="sxs-lookup"><span data-stu-id="c3238-153">Communication is one-way, toohello portal</span></span>|


## <a name="select-and-filter-your-metrics"></a><span data-ttu-id="c3238-154">Välj och filtrera dina</span><span class="sxs-lookup"><span data-stu-id="c3238-154">Select and filter your metrics</span></span>

<span data-ttu-id="c3238-155">(Tillgänglig på klassisk ASP.NET appar med hello senaste SDK: N.)</span><span class="sxs-lookup"><span data-stu-id="c3238-155">(Available on classic ASP.NET apps with hello latest SDK.)</span></span>

<span data-ttu-id="c3238-156">Du kan övervaka egna live KPI genom att använda valfri filter några Application Insights telemetri från hello-portalen.</span><span class="sxs-lookup"><span data-stu-id="c3238-156">You can monitor custom KPI live by applying arbitrary filters on any Application Insights telemetry from hello portal.</span></span> <span data-ttu-id="c3238-157">Klicka på hello filterkontrollen som visas när du muspekaren över något av hello scheman.</span><span class="sxs-lookup"><span data-stu-id="c3238-157">Click hello filter control that shows when you mouse-over any of hello charts.</span></span> <span data-ttu-id="c3238-158">följande diagram hello rita upp en anpassad antalet begäranden KPI med filter på URL: en och varaktighet attribut.</span><span class="sxs-lookup"><span data-stu-id="c3238-158">hello following chart is plotting a custom Request count KPI with filters on URL and Duration attributes.</span></span> <span data-ttu-id="c3238-159">Verifiera filter med hello dataströmmen Preview avsnitt som visar en levande feed av telemetri som matchar hello kriterier som du har angett när som helst i tid.</span><span class="sxs-lookup"><span data-stu-id="c3238-159">Validate your filters with hello Stream Preview section that shows a live feed of telemetry that matches hello criteria you have specified at any point in time.</span></span> 

![Anpassade begäran KPI](./media/app-insights-live-stream/live-stream-filteredMetric.png)

<span data-ttu-id="c3238-161">Du kan övervaka ett annat värde än antalet.</span><span class="sxs-lookup"><span data-stu-id="c3238-161">You can monitor a value different from Count.</span></span> <span data-ttu-id="c3238-162">hello alternativen beror på hello typ av dataström som kan vara någon telemetri i Application Insights: begäranden, beroenden, undantag, spårningar, händelser och mått.</span><span class="sxs-lookup"><span data-stu-id="c3238-162">hello options depend on hello type of stream, which could be any Application Insights telemetry: requests, dependencies, exceptions, traces, events, or metrics.</span></span> <span data-ttu-id="c3238-163">Det kan vara dina egna [anpassade mått](app-insights-api-custom-events-metrics.md#properties):</span><span class="sxs-lookup"><span data-stu-id="c3238-163">It can be your own [custom measurement](app-insights-api-custom-events-metrics.md#properties):</span></span>

![Alternativ](./media/app-insights-live-stream/live-stream-valueoptions.png)

<span data-ttu-id="c3238-165">Dessutom tooApplication insikter telemetri, du kan också övervaka Windows-prestandaräknare genom att välja som hello dataströmmen alternativ och hello namn på hello prestandaräknaren.</span><span class="sxs-lookup"><span data-stu-id="c3238-165">In addition tooApplication Insights telemetry, you can also monitor any Windows performance counter by selecting that from hello stream options, and providing hello name of hello performance counter.</span></span>

<span data-ttu-id="c3238-166">Live mått aggregeras vid två punkter: lokalt på varje server och sedan på alla servrar.</span><span class="sxs-lookup"><span data-stu-id="c3238-166">Live metrics are aggregated at two points: locally on each server, and then across all servers.</span></span> <span data-ttu-id="c3238-167">Du kan ändra hello standard på antingen genom att välja andra alternativ i hello respektive listrutorna.</span><span class="sxs-lookup"><span data-stu-id="c3238-167">You can change hello default at either by selecting other options in hello respective drop-downs.</span></span>

## <a name="sample-telemetry-custom-live-diagnostic-events"></a><span data-ttu-id="c3238-168">Exempel telemetri: Egna Live diagnostiska händelser</span><span class="sxs-lookup"><span data-stu-id="c3238-168">Sample Telemetry: Custom Live Diagnostic Events</span></span>
<span data-ttu-id="c3238-169">Som standard visar hello live feed händelser prover av misslyckade begäranden och beroendeanrop, undantag, händelser och spår.</span><span class="sxs-lookup"><span data-stu-id="c3238-169">By default, hello live feed of events shows samples of failed requests and dependency calls, exceptions, events, and traces.</span></span> <span data-ttu-id="c3238-170">Klicka på hello ikonen toosee hello tillämpas filtervillkor när som helst i tid.</span><span class="sxs-lookup"><span data-stu-id="c3238-170">Click hello filter icon toosee hello applied criteria at any point in time.</span></span> 

![Standard live-feed](./media/app-insights-live-stream/live-stream-eventsdefault.png)

<span data-ttu-id="c3238-172">Med statistik, kan som du ange en godtycklig kriterier tooany hello Application Insights telemetri typer.</span><span class="sxs-lookup"><span data-stu-id="c3238-172">As with metrics, you can specify any arbitrary criteria tooany of hello Application Insights telemetry types.</span></span> <span data-ttu-id="c3238-173">I det här exemplet väljer vi misslyckade begäranden för specifika, spår och händelser.</span><span class="sxs-lookup"><span data-stu-id="c3238-173">In this example, we are selecting specific request failures, traces, and events.</span></span> <span data-ttu-id="c3238-174">Vi är att markera alla undantag och beroende fel.</span><span class="sxs-lookup"><span data-stu-id="c3238-174">We are also selecting all exceptions and dependency failures.</span></span>

![Egna live feed](./media/app-insights-live-stream/live-stream-events.png)

<span data-ttu-id="c3238-176">Obs: För närvarande för undantaget meddelandebaserad kriterier, använda hello yttersta Undantagsmeddelande.</span><span class="sxs-lookup"><span data-stu-id="c3238-176">Note: Currently, for Exception message-based criteria, use hello outermost exception message.</span></span> <span data-ttu-id="c3238-177">I föregående exempel hello toofilter ut hello ofarlig undantag med meddelandet i det interna undantaget (sätt Hej ”<--” avgränsare) ”hello klienten har kopplats från”.</span><span class="sxs-lookup"><span data-stu-id="c3238-177">In hello preceding example, toofilter out hello benign exception with inner exception message (follows hello "<--" delimiter) "hello client disconnected."</span></span> <span data-ttu-id="c3238-178">Använd ett meddelande innehåller inte-”fel vid läsning av innehållet i en begäran” villkor.</span><span class="sxs-lookup"><span data-stu-id="c3238-178">use a message not-contains "Error reading request content" criteria.</span></span>

<span data-ttu-id="c3238-179">Se hello information för ett objekt i hello live feed genom att klicka på den.</span><span class="sxs-lookup"><span data-stu-id="c3238-179">See hello details of an item in hello live feed by clicking it.</span></span> <span data-ttu-id="c3238-180">Du kan pausa hello feed antingen genom att klicka på **pausa** bara bläddring nedåt eller klicka på ett element.</span><span class="sxs-lookup"><span data-stu-id="c3238-180">You can pause hello feed either by clicking **Pause** or simply scrolling down, or clicking an item.</span></span> <span data-ttu-id="c3238-181">Live feeden återupptas när du bläddrar i den bakre toohello upp eller genom att klicka på hello räknare för objekt som samlas in när den har pausats.</span><span class="sxs-lookup"><span data-stu-id="c3238-181">Live feed will resume after you scroll back toohello top, or by clicking hello counter of items collected while it was paused.</span></span>

![Provade live fel](./media/app-insights-live-stream/live-metrics-eventdetail.png)

## <a name="filter-by-server-instance"></a><span data-ttu-id="c3238-183">Filtrera efter server-instans</span><span class="sxs-lookup"><span data-stu-id="c3238-183">Filter by server instance</span></span>

<span data-ttu-id="c3238-184">Om du vill toomonitor en viss instans av serverroll kan filtrera du efter server.</span><span class="sxs-lookup"><span data-stu-id="c3238-184">If you want toomonitor a particular server role instance, you can filter by server.</span></span>

![Provade live fel](./media/app-insights-live-stream/live-stream-filter.png)

## <a name="sdk-requirements"></a><span data-ttu-id="c3238-186">SDK-krav</span><span class="sxs-lookup"><span data-stu-id="c3238-186">SDK Requirements</span></span>
<span data-ttu-id="c3238-187">Egna Live mått är tillgängliga med version 2.4.0-beta2 eller senare av [Application Insights SDK för webbtjänst](https://www.nuget.org/packages/Microsoft.ApplicationInsights.Web/).</span><span class="sxs-lookup"><span data-stu-id="c3238-187">Custom Live Metrics Stream is available with version 2.4.0-beta2 or newer of [Application Insights SDK for web](https://www.nuget.org/packages/Microsoft.ApplicationInsights.Web/).</span></span> <span data-ttu-id="c3238-188">Kom ihåg tooselect ”inkludera förhandsversion” alternativet från NuGet-Pakethanteraren.</span><span class="sxs-lookup"><span data-stu-id="c3238-188">Remember tooselect "Include Prerelease" option from NuGet package manager.</span></span>

## <a name="secure-hello-control-channel"></a><span data-ttu-id="c3238-189">Säker hello kontrollkanal</span><span class="sxs-lookup"><span data-stu-id="c3238-189">Secure hello control channel</span></span>
<span data-ttu-id="c3238-190">hello anpassade filter kriterier som du anger skickas tillbaka toohello Live mått komponent i hello Application Insights SDK.</span><span class="sxs-lookup"><span data-stu-id="c3238-190">hello custom filters criteria you specify are sent back toohello Live Metrics component in hello Application Insights SDK.</span></span> <span data-ttu-id="c3238-191">hello filter kan innehålla känslig information, till exempel customerIDs.</span><span class="sxs-lookup"><span data-stu-id="c3238-191">hello filters could potentially contain sensitive information such as customerIDs.</span></span> <span data-ttu-id="c3238-192">Du kan göra hello kanal säkert med en hemlig nyckel API dessutom toohello instrumentation nyckel.</span><span class="sxs-lookup"><span data-stu-id="c3238-192">You can make hello channel secure with a secret API key in addition toohello instrumentation key.</span></span>
### <a name="create-an-api-key"></a><span data-ttu-id="c3238-193">Skapa en API-nyckel</span><span class="sxs-lookup"><span data-stu-id="c3238-193">Create an API Key</span></span>

![Skapa api-nyckel](./media/app-insights-live-stream/live-metrics-apikeycreate.png)

### <a name="add-api-key-tooconfiguration"></a><span data-ttu-id="c3238-195">Lägg till API-nyckel tooConfiguration</span><span class="sxs-lookup"><span data-stu-id="c3238-195">Add API key tooConfiguration</span></span>
<span data-ttu-id="c3238-196">Lägg till hello AuthenticationApiKey toohello QuickPulseTelemetryModule hello applicationinsights.config-filen:</span><span class="sxs-lookup"><span data-stu-id="c3238-196">In hello applicationinsights.config file, add hello AuthenticationApiKey toohello QuickPulseTelemetryModule:</span></span>
``` XML

<Add Type="Microsoft.ApplicationInsights.Extensibility.PerfCounterCollector.QuickPulse.QuickPulseTelemetryModule, Microsoft.AI.PerfCounterCollector">
      <AuthenticationApiKey>YOUR-API-KEY-HERE</AuthenticationApiKey>
</Add> 

```
<span data-ttu-id="c3238-197">Eller i koden, Ställ in den på hello QuickPulseTelemetryModule:</span><span class="sxs-lookup"><span data-stu-id="c3238-197">Or in code, set it on hello QuickPulseTelemetryModule:</span></span>

``` C#

    module.AuthenticationApiKey = "YOUR-API-KEY-HERE";

```

<span data-ttu-id="c3238-198">Om du känner igen och litar på alla hello anslutna servrar, kan du försöka hello anpassade filter utan hello autentiserad kanal.</span><span class="sxs-lookup"><span data-stu-id="c3238-198">However, if you recognize and trust all hello connected servers, you can try hello custom filters without hello authenticated channel.</span></span> <span data-ttu-id="c3238-199">Det här alternativet är tillgängligt i sex månader.</span><span class="sxs-lookup"><span data-stu-id="c3238-199">This option is available for six months.</span></span> <span data-ttu-id="c3238-200">Den här åsidosättningen krävs när varje ny session, eller när en ny server är online.</span><span class="sxs-lookup"><span data-stu-id="c3238-200">This override is required once every new session, or when a new server comes online.</span></span>

![Live mått Auth-alternativ](./media/app-insights-live-stream/live-stream-auth.png)

>[!NOTE]
><span data-ttu-id="c3238-202">Vi rekommenderar starkt att du ställer in hello autentiserad kanal innan du anger vara känslig information, till exempel CustomerID i hello filtervillkor.</span><span class="sxs-lookup"><span data-stu-id="c3238-202">We strongly recommend that you set up hello authenticated channel before entering potentially sensitive information like CustomerID in hello filter criteria.</span></span>
>

## <a name="generating-a-performance-test-load"></a><span data-ttu-id="c3238-203">Generera en prestandabelastningen test</span><span class="sxs-lookup"><span data-stu-id="c3238-203">Generating a performance test load</span></span>

<span data-ttu-id="c3238-204">Om du vill toowatch hello effekten av en belastningen ökar, Använd hello prestandatester bladet.</span><span class="sxs-lookup"><span data-stu-id="c3238-204">If you want toowatch hello effect of a load increase, use hello Performance Test blade.</span></span> <span data-ttu-id="c3238-205">Den simulerar begäranden från ett antal samtidiga användare.</span><span class="sxs-lookup"><span data-stu-id="c3238-205">It simulates requests from a number of simultaneous users.</span></span> <span data-ttu-id="c3238-206">Det kan köras antingen ”manuell” test (ping tester) på en enskild URL eller det kan köras en [flera steg prestanda webbtest](app-insights-monitor-web-app-availability.md#multi-step-web-tests) du överför (på samma sätt som en tillgänglighet testa hello).</span><span class="sxs-lookup"><span data-stu-id="c3238-206">It can run either "manual tests" (ping tests) of a single URL, or it can run a [multi-step web performance test](app-insights-monitor-web-app-availability.md#multi-step-web-tests) that you upload (in hello same way as an availability test).</span></span>

> [!TIP]
> <span data-ttu-id="c3238-207">När du har skapat hello prestandatester öppna hello test och hello direktsänd dataström bladet i separata fönster.</span><span class="sxs-lookup"><span data-stu-id="c3238-207">After you create hello performance test, open hello test and hello Live Stream blade in separate windows.</span></span> <span data-ttu-id="c3238-208">Du kan se när hello köade prestanda test startar och titta på direktsänd dataström i hello samtidigt.</span><span class="sxs-lookup"><span data-stu-id="c3238-208">You can see when hello queued performance test starts, and watch live stream at hello same time.</span></span>
>


## <a name="troubleshooting"></a><span data-ttu-id="c3238-209">Felsökning</span><span class="sxs-lookup"><span data-stu-id="c3238-209">Troubleshooting</span></span>

<span data-ttu-id="c3238-210">Ser du inga data?</span><span class="sxs-lookup"><span data-stu-id="c3238-210">No data?</span></span> <span data-ttu-id="c3238-211">Om programmet är i ett skyddat nätverk: Live mått dataströmmen använder en annan IP-adresser än andra Application Insights telemetry.</span><span class="sxs-lookup"><span data-stu-id="c3238-211">If your application is in a protected network: Live Metrics Stream uses a different IP addresses than other Application Insights telemetry.</span></span> <span data-ttu-id="c3238-212">Kontrollera att [IP-adresserna](app-insights-ip-addresses.md) är öppna i brandväggen.</span><span class="sxs-lookup"><span data-stu-id="c3238-212">Make sure [those IP addresses](app-insights-ip-addresses.md) are open in your firewall.</span></span>



## <a name="next-steps"></a><span data-ttu-id="c3238-213">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="c3238-213">Next steps</span></span>
* [<span data-ttu-id="c3238-214">Övervakning med Application Insights</span><span class="sxs-lookup"><span data-stu-id="c3238-214">Monitoring usage with Application Insights</span></span>](app-insights-web-track-usage.md)
* [<span data-ttu-id="c3238-215">Diagnostiska sökning</span><span class="sxs-lookup"><span data-stu-id="c3238-215">Using Diagnostic Search</span></span>](app-insights-diagnostic-search.md)
* [<span data-ttu-id="c3238-216">Profiler</span><span class="sxs-lookup"><span data-stu-id="c3238-216">Profiler</span></span>](app-insights-profiler.md)
* [<span data-ttu-id="c3238-217">Snapshot-felsökare</span><span class="sxs-lookup"><span data-stu-id="c3238-217">Snapshot debugger</span></span>](app-insights-snapshot-debugger.md)
