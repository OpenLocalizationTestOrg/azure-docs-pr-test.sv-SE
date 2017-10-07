---
title: aaaApplicationInsights.config - referens i Azure | Microsoft Docs
description: "Aktivera eller inaktivera data collection moduler och lägga till prestandaräknare och andra parametrar."
services: application-insights
documentationcenter: 
author: OlegAnaniev-MSFT
editor: alancameronwills
manager: carmonm
ms.assetid: 6e397752-c086-46e9-8648-a1196e8078c2
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 05/3/2017
ms.author: bwren
ms.openlocfilehash: 76cb11349d87dfc508ec8b1c454259a0b079c48a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="configuring-hello-application-insights-sdk-with-applicationinsightsconfig-or-xml"></a><span data-ttu-id="df084-103">Konfigurera hello Application Insights SDK med ApplicationInsights.config eller .xml</span><span class="sxs-lookup"><span data-stu-id="df084-103">Configuring hello Application Insights SDK with ApplicationInsights.config or .xml</span></span>
<span data-ttu-id="df084-104">hello Application Insights .NET SDK består av ett antal NuGet-paket.</span><span class="sxs-lookup"><span data-stu-id="df084-104">hello Application Insights .NET SDK consists of a number of NuGet packages.</span></span> <span data-ttu-id="df084-105">Den [core-paketet](http://www.nuget.org/packages/Microsoft.ApplicationInsights) innehåller hello API för att skicka telemetri till hello Application Insights.</span><span class="sxs-lookup"><span data-stu-id="df084-105">The [core package](http://www.nuget.org/packages/Microsoft.ApplicationInsights) provides hello API for sending telemetry to hello Application Insights.</span></span> <span data-ttu-id="df084-106">[Ytterligare paket](http://www.nuget.org/packages?q=Microsoft.ApplicationInsights) ange telemetri *moduler* och *initierare* för automatiskt spåra telemetri från ditt program och dess kontext.</span><span class="sxs-lookup"><span data-stu-id="df084-106">[Additional packages](http://www.nuget.org/packages?q=Microsoft.ApplicationInsights) provide telemetry *modules* and *initializers* for automatically tracking telemetry from your application and its context.</span></span> <span data-ttu-id="df084-107">Genom att justera hello-konfigurationsfil kan du aktivera eller inaktivera telemetri moduler och initierare och ange parametrar för några av dem.</span><span class="sxs-lookup"><span data-stu-id="df084-107">By adjusting hello configuration file, you can enable or disable telemetry modules and initializers, and set parameters for some of them.</span></span>

<span data-ttu-id="df084-108">hello konfigurationsfilen heter `ApplicationInsights.config` eller `ApplicationInsights.xml`, beroende på hello typ av ditt program.</span><span class="sxs-lookup"><span data-stu-id="df084-108">hello configuration file is named `ApplicationInsights.config` or `ApplicationInsights.xml`, depending on hello type of your application.</span></span> <span data-ttu-id="df084-109">Läggs den automatiskt tooyour projektet när du [installera de flesta versioner av hello SDK][start].</span><span class="sxs-lookup"><span data-stu-id="df084-109">It is automatically added tooyour project when you [install most versions of hello SDK][start].</span></span> <span data-ttu-id="df084-110">Läggs också tooa webbprogram genom [Status Monitor på en IIS-server][redfield], eller när du väljer hello Appplication Insights [tillägg för Azure-webbplats eller VM](app-insights-azure-web-apps.md).</span><span class="sxs-lookup"><span data-stu-id="df084-110">It is also added tooa web app by [Status Monitor on an IIS server][redfield], or when you select hello Appplication Insights [extension for an Azure website or VM](app-insights-azure-web-apps.md).</span></span>

<span data-ttu-id="df084-111">Det finns inte en motsvarande filen toocontrol hello [SDK på en webbsida][client].</span><span class="sxs-lookup"><span data-stu-id="df084-111">There isn't an equivalent file toocontrol hello [SDK in a web page][client].</span></span>

<span data-ttu-id="df084-112">Det här dokumentet beskriver hello avsnitt som du ser i hello configuration-fil, hur de styra hello komponenter i hello SDK, och vilka NuGet-paket att läsa in dessa komponenter.</span><span class="sxs-lookup"><span data-stu-id="df084-112">This document describes hello sections you see in hello configuration file, how they control hello components of hello SDK, and which NuGet packages load those components.</span></span>

## <a name="telemetry-modules-aspnet"></a><span data-ttu-id="df084-113">Telemetri moduler (ASP.NET)</span><span class="sxs-lookup"><span data-stu-id="df084-113">Telemetry Modules (ASP.NET)</span></span>
<span data-ttu-id="df084-114">Varje telemetri modul samlar in en viss typ av data och använder hello core API toosend hello data.</span><span class="sxs-lookup"><span data-stu-id="df084-114">Each telemetry module collects a specific type of data and uses hello core API toosend hello data.</span></span> <span data-ttu-id="df084-115">hello-moduler installeras av olika NuGet-paket, som också lägga till hello rader som behövs toohello .config-fil.</span><span class="sxs-lookup"><span data-stu-id="df084-115">hello modules are installed by different NuGet packages, which also add hello required lines toohello .config file.</span></span>

<span data-ttu-id="df084-116">Det är en nod i hello konfigurationsfilen för varje modul.</span><span class="sxs-lookup"><span data-stu-id="df084-116">There's a node in hello configuration file for each module.</span></span> <span data-ttu-id="df084-117">toodisable en modul, ta bort hello nod eller kommentera ut.</span><span class="sxs-lookup"><span data-stu-id="df084-117">toodisable a module, delete hello node or comment it out.</span></span>

### <a name="dependency-tracking"></a><span data-ttu-id="df084-118">Beroende spårning</span><span class="sxs-lookup"><span data-stu-id="df084-118">Dependency Tracking</span></span>
<span data-ttu-id="df084-119">[Beroende spårning](app-insights-asp-net-dependencies.md) samlar in telemetri om anrop som gör att din app toodatabases och externa tjänster och databaser.</span><span class="sxs-lookup"><span data-stu-id="df084-119">[Dependency tracking](app-insights-asp-net-dependencies.md) collects telemetry about calls your app makes toodatabases and external services and databases.</span></span> <span data-ttu-id="df084-120">tooallow den här modulen toowork i en IIS-server du behöver för[installera statusövervakaren][redfield].</span><span class="sxs-lookup"><span data-stu-id="df084-120">tooallow this module toowork in an IIS server, you need too[install Status Monitor][redfield].</span></span> <span data-ttu-id="df084-121">toouse den i Azure-webbappar eller virtuella datorer, [markerar hello Application Insights-tillägget](app-insights-azure-web-apps.md).</span><span class="sxs-lookup"><span data-stu-id="df084-121">toouse it in Azure web apps or VMs, [select hello Application Insights extension](app-insights-azure-web-apps.md).</span></span>

<span data-ttu-id="df084-122">Du kan också skriva egna beroende spårning kod med hello [TrackDependency API](app-insights-api-custom-events-metrics.md#trackdependency).</span><span class="sxs-lookup"><span data-stu-id="df084-122">You can also write your own dependency tracking code using hello [TrackDependency API](app-insights-api-custom-events-metrics.md#trackdependency).</span></span>

* `Microsoft.ApplicationInsights.DependencyCollector.DependencyTrackingTelemetryModule`
* <span data-ttu-id="df084-123">[Microsoft.ApplicationInsights.DependencyCollector](http://www.nuget.org/packages/Microsoft.ApplicationInsights.DependencyCollector) NuGet-paketet.</span><span class="sxs-lookup"><span data-stu-id="df084-123">[Microsoft.ApplicationInsights.DependencyCollector](http://www.nuget.org/packages/Microsoft.ApplicationInsights.DependencyCollector) NuGet package.</span></span>

### <a name="performance-collector"></a><span data-ttu-id="df084-124">Insamlare för prestanda</span><span class="sxs-lookup"><span data-stu-id="df084-124">Performance collector</span></span>
<span data-ttu-id="df084-125">[Samlar in prestandaräknare för system](app-insights-performance-counters.md) t.ex CPU, minne och nätverk läses in från IIS-installationer.</span><span class="sxs-lookup"><span data-stu-id="df084-125">[Collects system performance counters](app-insights-performance-counters.md) such as CPU, memory and network load from IIS installations.</span></span> <span data-ttu-id="df084-126">Du kan ange vilka räknare toocollect, inklusive prestandaräknare som du har skapat själv.</span><span class="sxs-lookup"><span data-stu-id="df084-126">You can specify which counters toocollect, including performance counters you have set up yourself.</span></span>

* `Microsoft.ApplicationInsights.Extensibility.PerfCounterCollector.PerformanceCollectorModule`
* <span data-ttu-id="df084-127">[Microsoft.ApplicationInsights.PerfCounterCollector](http://www.nuget.org/packages/Microsoft.ApplicationInsights.PerfCounterCollector) NuGet-paketet.</span><span class="sxs-lookup"><span data-stu-id="df084-127">[Microsoft.ApplicationInsights.PerfCounterCollector](http://www.nuget.org/packages/Microsoft.ApplicationInsights.PerfCounterCollector) NuGet package.</span></span>

### <a name="application-insights-diagnostics-telemetry"></a><span data-ttu-id="df084-128">Application Insights-Diagnostiktelemetri</span><span class="sxs-lookup"><span data-stu-id="df084-128">Application Insights Diagnostics Telemetry</span></span>
<span data-ttu-id="df084-129">Hej `DiagnosticsTelemetryModule` rapporterar fel i hello Application Insights instrumentation själva koden.</span><span class="sxs-lookup"><span data-stu-id="df084-129">hello `DiagnosticsTelemetryModule` reports errors in hello Application Insights instrumentation code itself.</span></span> <span data-ttu-id="df084-130">Till exempel om hello kod inte kan komma åt prestandaräknare eller om en `ITelemetryInitializer` genererar ett undantag.</span><span class="sxs-lookup"><span data-stu-id="df084-130">For example, if hello code cannot access performance counters or if an `ITelemetryInitializer` throws an exception.</span></span> <span data-ttu-id="df084-131">Spårningstelemetri spåras av den här modulen visas i hello [diagnostiska Sök][diagnostic].</span><span class="sxs-lookup"><span data-stu-id="df084-131">Trace telemetry tracked by this module appears in hello [Diagnostic Search][diagnostic].</span></span> <span data-ttu-id="df084-132">Skickar diagnostikdata toodc.services.vsallin.net.</span><span class="sxs-lookup"><span data-stu-id="df084-132">Sends diagnostic data toodc.services.vsallin.net.</span></span>

* `Microsoft.ApplicationInsights.Extensibility.Implementation.Tracing.DiagnosticsTelemetryModule`
* <span data-ttu-id="df084-133">[Microsoft.ApplicationInsights](http://www.nuget.org/packages/Microsoft.ApplicationInsights) NuGet-paketet.</span><span class="sxs-lookup"><span data-stu-id="df084-133">[Microsoft.ApplicationInsights](http://www.nuget.org/packages/Microsoft.ApplicationInsights) NuGet package.</span></span> <span data-ttu-id="df084-134">Om du endast installera det här paketet skapas inte hello ApplicationInsights.config-filen automatiskt.</span><span class="sxs-lookup"><span data-stu-id="df084-134">If you only install this package, hello ApplicationInsights.config file is not automatically created.</span></span>

### <a name="developer-mode"></a><span data-ttu-id="df084-135">Utvecklarläge</span><span class="sxs-lookup"><span data-stu-id="df084-135">Developer Mode</span></span>
<span data-ttu-id="df084-136">`DeveloperModeWithDebuggerAttachedTelemetryModule`Framtvingar hello Application Insights `TelemetryChannel` toosend data omedelbart, en telemetri objekt samtidigt, när en felsökare är kopplad toohello programprocessen.</span><span class="sxs-lookup"><span data-stu-id="df084-136">`DeveloperModeWithDebuggerAttachedTelemetryModule` forces hello Application Insights `TelemetryChannel` toosend data immediately, one telemetry item at a time, when a debugger is attached toohello application process.</span></span> <span data-ttu-id="df084-137">Detta minskar hello tidslängd mellan hello nu när ditt program spårar telemetri och när den visas på hello Application Insights-portalen.</span><span class="sxs-lookup"><span data-stu-id="df084-137">This reduces hello amount of time between hello moment when your application tracks telemetry and when it appears on hello Application Insights portal.</span></span> <span data-ttu-id="df084-138">Det medför betydande kostnader i CPU: N och bandbredd.</span><span class="sxs-lookup"><span data-stu-id="df084-138">It causes significant overhead in CPU and network bandwidth.</span></span>

* `Microsoft.ApplicationInsights.WindowsServer.DeveloperModeWithDebuggerAttachedTelemetryModule`
* <span data-ttu-id="df084-139">[Application Insights Windows Server](http://www.nuget.org/packages/Microsoft.ApplicationInsights.WindowsServer/) NuGet-paketet</span><span class="sxs-lookup"><span data-stu-id="df084-139">[Application Insights Windows Server](http://www.nuget.org/packages/Microsoft.ApplicationInsights.WindowsServer/) NuGet package</span></span>

### <a name="web-request-tracking"></a><span data-ttu-id="df084-140">Webbegäran spårning</span><span class="sxs-lookup"><span data-stu-id="df084-140">Web Request Tracking</span></span>
<span data-ttu-id="df084-141">Rapporter hello [tid och resultatet svarskoden](app-insights-asp-net.md) av HTTP-begäranden.</span><span class="sxs-lookup"><span data-stu-id="df084-141">Reports hello [response time and result code](app-insights-asp-net.md) of HTTP requests.</span></span>

* `Microsoft.ApplicationInsights.Web.RequestTrackingTelemetryModule`
* <span data-ttu-id="df084-142">[Microsoft.ApplicationInsights.Web](http://www.nuget.org/packages/Microsoft.ApplicationInsights.Web) NuGet-paketet</span><span class="sxs-lookup"><span data-stu-id="df084-142">[Microsoft.ApplicationInsights.Web](http://www.nuget.org/packages/Microsoft.ApplicationInsights.Web) NuGet package</span></span>

### <a name="exception-tracking"></a><span data-ttu-id="df084-143">Undantagsspårning</span><span class="sxs-lookup"><span data-stu-id="df084-143">Exception tracking</span></span>
<span data-ttu-id="df084-144">`ExceptionTrackingTelemetryModule`spårar ohanterade undantag i ditt webbprogram.</span><span class="sxs-lookup"><span data-stu-id="df084-144">`ExceptionTrackingTelemetryModule` tracks unhandled exceptions in your web app.</span></span> <span data-ttu-id="df084-145">Se [fel och undantag][exceptions].</span><span class="sxs-lookup"><span data-stu-id="df084-145">See [Failures and exceptions][exceptions].</span></span>

* `Microsoft.ApplicationInsights.Web.ExceptionTrackingTelemetryModule`
* <span data-ttu-id="df084-146">[Microsoft.ApplicationInsights.Web](http://www.nuget.org/packages/Microsoft.ApplicationInsights.Web) NuGet-paketet</span><span class="sxs-lookup"><span data-stu-id="df084-146">[Microsoft.ApplicationInsights.Web](http://www.nuget.org/packages/Microsoft.ApplicationInsights.Web) NuGet package</span></span>
* <span data-ttu-id="df084-147">`Microsoft.ApplicationInsights.WindowsServer.UnobservedExceptionTelemetryModule`-spårar [symptom uppgiften undantag](http://blogs.msdn.com/b/pfxteam/archive/2011/09/28/task-exception-handling-in-net-4-5.aspx).</span><span class="sxs-lookup"><span data-stu-id="df084-147">`Microsoft.ApplicationInsights.WindowsServer.UnobservedExceptionTelemetryModule` - tracks [unobserved task exceptions](http://blogs.msdn.com/b/pfxteam/archive/2011/09/28/task-exception-handling-in-net-4-5.aspx).</span></span>
* <span data-ttu-id="df084-148">`Microsoft.ApplicationInsights.WindowsServer.UnhandledExceptionTelemetryModule`-spårar ohanterade undantag för arbetsroller, windows-tjänster och program.</span><span class="sxs-lookup"><span data-stu-id="df084-148">`Microsoft.ApplicationInsights.WindowsServer.UnhandledExceptionTelemetryModule` - tracks unhandled exceptions for worker roles, windows services, and console applications.</span></span>
* <span data-ttu-id="df084-149">[Application Insights Windows Server](http://www.nuget.org/packages/Microsoft.ApplicationInsights.WindowsServer/) NuGet-paketet.</span><span class="sxs-lookup"><span data-stu-id="df084-149">[Application Insights Windows Server](http://www.nuget.org/packages/Microsoft.ApplicationInsights.WindowsServer/) NuGet package.</span></span>

### <a name="eventsource-tracking"></a><span data-ttu-id="df084-150">EventSource spårning</span><span class="sxs-lookup"><span data-stu-id="df084-150">EventSource Tracking</span></span>
<span data-ttu-id="df084-151">`EventSourceTelemetryModule`låter dig tooconfigure EventSource händelser toobe skickas tooApplication insikter som spårningar.</span><span class="sxs-lookup"><span data-stu-id="df084-151">`EventSourceTelemetryModule` allows you tooconfigure EventSource events toobe sent tooApplication Insights as traces.</span></span> <span data-ttu-id="df084-152">Information om hur du spårar EventSource händelser finns [med EventSource händelser](app-insights-asp-net-trace-logs.md#using-eventsource-events).</span><span class="sxs-lookup"><span data-stu-id="df084-152">For information on tracking EventSource events, see [Using EventSource Events](app-insights-asp-net-trace-logs.md#using-eventsource-events).</span></span>

* `Microsoft.ApplicationInsights.EventSourceListener.EventSourceTelemetryModule`
* [<span data-ttu-id="df084-153">Microsoft.ApplicationInsights.EventSourceListener</span><span class="sxs-lookup"><span data-stu-id="df084-153">Microsoft.ApplicationInsights.EventSourceListener</span></span>](http://www.nuget.org/packages/Microsoft.ApplicationInsights.EventSourceListener) 

### <a name="etw-event-tracking"></a><span data-ttu-id="df084-154">Spårning av ETW-händelse</span><span class="sxs-lookup"><span data-stu-id="df084-154">ETW Event Tracking</span></span>
<span data-ttu-id="df084-155">`EtwCollectorTelemetryModule`låter dig tooconfigure händelser från ETW-providers toobe skickas tooApplication insikter som spårningar.</span><span class="sxs-lookup"><span data-stu-id="df084-155">`EtwCollectorTelemetryModule` allows you tooconfigure events from ETW providers toobe sent tooApplication Insights as traces.</span></span> <span data-ttu-id="df084-156">Information om hur du spårar ETW-händelser finns i [med hjälp av ETW-händelser](app-insights-asp-net-trace-logs.md#using-etw-events).</span><span class="sxs-lookup"><span data-stu-id="df084-156">For information on tracking ETW events, see [Using ETW Events](app-insights-asp-net-trace-logs.md#using-etw-events).</span></span>

* `Microsoft.ApplicationInsights.EtwCollector.EtwCollectorTelemetryModule`
* [<span data-ttu-id="df084-157">Microsoft.ApplicationInsights.EtwCollector</span><span class="sxs-lookup"><span data-stu-id="df084-157">Microsoft.ApplicationInsights.EtwCollector</span></span>](http://www.nuget.org/packages/Microsoft.ApplicationInsights.EtwCollector) 

### <a name="microsoftapplicationinsights"></a><span data-ttu-id="df084-158">Microsoft.ApplicationInsights</span><span class="sxs-lookup"><span data-stu-id="df084-158">Microsoft.ApplicationInsights</span></span>
<span data-ttu-id="df084-159">Hej Microsoft.ApplicationInsights erbjuder hello [core API](https://msdn.microsoft.com/library/mt420197.aspx) av hello SDK.</span><span class="sxs-lookup"><span data-stu-id="df084-159">hello Microsoft.ApplicationInsights package provides hello [core API](https://msdn.microsoft.com/library/mt420197.aspx) of hello SDK.</span></span> <span data-ttu-id="df084-160">hello andra telemetri moduler använda detta, och du kan också [använder den toodefine egna telemetri](app-insights-api-custom-events-metrics.md).</span><span class="sxs-lookup"><span data-stu-id="df084-160">hello other telemetry modules use this, and you can also [use it toodefine your own telemetry](app-insights-api-custom-events-metrics.md).</span></span>

* <span data-ttu-id="df084-161">Ingen post i ApplicationInsights.config.</span><span class="sxs-lookup"><span data-stu-id="df084-161">No entry in ApplicationInsights.config.</span></span>
* <span data-ttu-id="df084-162">[Microsoft.ApplicationInsights](http://www.nuget.org/packages/Microsoft.ApplicationInsights) NuGet-paketet.</span><span class="sxs-lookup"><span data-stu-id="df084-162">[Microsoft.ApplicationInsights](http://www.nuget.org/packages/Microsoft.ApplicationInsights) NuGet package.</span></span> <span data-ttu-id="df084-163">Om du bara installera den här NuGet skapas ingen .config-fil.</span><span class="sxs-lookup"><span data-stu-id="df084-163">If you just install this NuGet, no .config file is generated.</span></span>

## <a name="telemetry-channel"></a><span data-ttu-id="df084-164">Telemetri kanal</span><span class="sxs-lookup"><span data-stu-id="df084-164">Telemetry Channel</span></span>
<span data-ttu-id="df084-165">hello telemetri kanaler hanterar buffring och överföring av telemetri toohello Application Insights-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="df084-165">hello telemetry channel manages buffering and transmission of telemetry toohello Application Insights service.</span></span>

* <span data-ttu-id="df084-166">`Microsoft.ApplicationInsights.WindowsServer.TelemetryChannel.ServerTelemetryChannel`är hello standardkanalen för tjänster.</span><span class="sxs-lookup"><span data-stu-id="df084-166">`Microsoft.ApplicationInsights.WindowsServer.TelemetryChannel.ServerTelemetryChannel` is hello default channel for services.</span></span> <span data-ttu-id="df084-167">Den buffrar data i minnet.</span><span class="sxs-lookup"><span data-stu-id="df084-167">It buffers data in memory.</span></span>
* <span data-ttu-id="df084-168">`Microsoft.ApplicationInsights.PersistenceChannel`är ett alternativ för konsolprogram.</span><span class="sxs-lookup"><span data-stu-id="df084-168">`Microsoft.ApplicationInsights.PersistenceChannel` is an alternative for console applications.</span></span> <span data-ttu-id="df084-169">Det kan spara alla unflushed toopersistent datalagring när din app stängs och skicka den när hello appen startas igen.</span><span class="sxs-lookup"><span data-stu-id="df084-169">It can save any unflushed data toopersistent storage when your app closes down, and will send it when hello app starts again.</span></span>

## <a name="telemetry-initializers-aspnet"></a><span data-ttu-id="df084-170">Telemetri initierare (ASP.NET)</span><span class="sxs-lookup"><span data-stu-id="df084-170">Telemetry Initializers (ASP.NET)</span></span>
<span data-ttu-id="df084-171">Telemetri initierare ange kontextegenskaper för som skickas tillsammans med alla element på telemetri.</span><span class="sxs-lookup"><span data-stu-id="df084-171">Telemetry initializers set context properties that are sent along with every item of telemetry.</span></span>

<span data-ttu-id="df084-172">Du kan [skriva egna initierare](app-insights-api-filtering-sampling.md#add-properties) tooset kontextegenskaper.</span><span class="sxs-lookup"><span data-stu-id="df084-172">You can [write your own initializers](app-insights-api-filtering-sampling.md#add-properties) tooset context properties.</span></span>

<span data-ttu-id="df084-173">hello standard initierare har värdet av hello webb- eller WindowsServer NuGet-paket:</span><span class="sxs-lookup"><span data-stu-id="df084-173">hello standard initializers are all set either by hello Web or WindowsServer NuGet packages:</span></span>

* <span data-ttu-id="df084-174">`AccountIdTelemetryInitializer`Egenskapen hello AccountId.</span><span class="sxs-lookup"><span data-stu-id="df084-174">`AccountIdTelemetryInitializer` sets hello AccountId property.</span></span>
* <span data-ttu-id="df084-175">`AuthenticatedUserIdTelemetryInitializer`Egenskapen hello AuthenticatedUserId som angetts av hello JavaScript SDK.</span><span class="sxs-lookup"><span data-stu-id="df084-175">`AuthenticatedUserIdTelemetryInitializer` sets hello AuthenticatedUserId property as set by hello JavaScript SDK.</span></span>
* <span data-ttu-id="df084-176">`AzureRoleEnvironmentTelemetryInitializer`uppdateringar hello `RoleName` och `RoleInstance` egenskaper för hello `Device` kontext för alla telemetri artiklar med information som hämtas från hello Azure körningsmiljön.</span><span class="sxs-lookup"><span data-stu-id="df084-176">`AzureRoleEnvironmentTelemetryInitializer` updates hello `RoleName` and `RoleInstance` properties of hello `Device` context for all telemetry items with information extracted from hello Azure runtime environment.</span></span>
* <span data-ttu-id="df084-177">`BuildInfoConfigComponentVersionTelemetryInitializer`uppdateringar hello `Version` -egenskapen för hello `Component` kontext för alla telemetri artiklar med hello-värde som hämtats från hello `BuildInfo.config` fil produceras av MS-Build.</span><span class="sxs-lookup"><span data-stu-id="df084-177">`BuildInfoConfigComponentVersionTelemetryInitializer` updates hello `Version` property of hello `Component` context for all telemetry items with hello value extracted from hello `BuildInfo.config` file produced by MS Build.</span></span>
* <span data-ttu-id="df084-178">`ClientIpHeaderTelemetryInitializer`uppdateringar `Ip` -egenskapen för hello `Location` kontexten för alla telemetri objekt baserat på hello `X-Forwarded-For` HTTP-huvud för hello-begäran.</span><span class="sxs-lookup"><span data-stu-id="df084-178">`ClientIpHeaderTelemetryInitializer` updates `Ip` property of hello `Location` context of all telemetry items based on hello `X-Forwarded-For` HTTP header of hello request.</span></span>
* <span data-ttu-id="df084-179">`DeviceTelemetryInitializer`uppdateringar hello följande egenskaper för hello `Device` kontext för alla telemetri objekt.</span><span class="sxs-lookup"><span data-stu-id="df084-179">`DeviceTelemetryInitializer` updates hello following properties of hello `Device` context for all telemetry items.</span></span>
  * <span data-ttu-id="df084-180">`Type`anges för ”dator”</span><span class="sxs-lookup"><span data-stu-id="df084-180">`Type` is set too"PC"</span></span>
  * <span data-ttu-id="df084-181">`Id`anges toohello domännamnet för hello dator där hello webbtillämpning körs.</span><span class="sxs-lookup"><span data-stu-id="df084-181">`Id` is set toohello domain name of hello computer where hello web application is running.</span></span>
  * <span data-ttu-id="df084-182">`OemName`är värdet toohello extraheras från hello `Win32_ComputerSystem.Manufacturer` med hjälp av WMI.</span><span class="sxs-lookup"><span data-stu-id="df084-182">`OemName` is set toohello value extracted from hello `Win32_ComputerSystem.Manufacturer` field using WMI.</span></span>
  * <span data-ttu-id="df084-183">`Model`är värdet toohello extraheras från hello `Win32_ComputerSystem.Model` med hjälp av WMI.</span><span class="sxs-lookup"><span data-stu-id="df084-183">`Model` is set toohello value extracted from hello `Win32_ComputerSystem.Model` field using WMI.</span></span>
  * <span data-ttu-id="df084-184">`NetworkType`är värdet toohello extraheras från hello `NetworkInterface`.</span><span class="sxs-lookup"><span data-stu-id="df084-184">`NetworkType` is set toohello value extracted from hello `NetworkInterface`.</span></span>
  * <span data-ttu-id="df084-185">`Language`anges toohello namnet på hello `CurrentCulture`.</span><span class="sxs-lookup"><span data-stu-id="df084-185">`Language` is set toohello name of hello `CurrentCulture`.</span></span>
* <span data-ttu-id="df084-186">`DomainNameRoleInstanceTelemetryInitializer`uppdateringar hello `RoleInstance` -egenskapen för hello `Device` kontext för alla telemetri artiklar med hello domännamn för hello dator där hello webbprogram körs.</span><span class="sxs-lookup"><span data-stu-id="df084-186">`DomainNameRoleInstanceTelemetryInitializer` updates hello `RoleInstance` property of hello `Device` context for all telemetry items with hello domain name of hello computer where hello web application is running.</span></span>
* <span data-ttu-id="df084-187">`OperationNameTelemetryInitializer`uppdateringar hello `Name` -egenskapen för hello `RequestTelemetry` och hello `Name` -egenskapen för hello `Operation` kontexten för alla telemetri objekt baserat på hello HTTP-metoden, samt namnen på ASP.NET MVC-kontrollanten och åtgärden anropade tooprocess hello begäran.</span><span class="sxs-lookup"><span data-stu-id="df084-187">`OperationNameTelemetryInitializer` updates hello `Name` property of hello `RequestTelemetry` and hello `Name` property of hello `Operation` context of all telemetry items based on hello HTTP method, as well as names of ASP.NET MVC controller and action invoked tooprocess hello request.</span></span>
* <span data-ttu-id="df084-188">`OperationIdTelemetryInitializer`eller `OperationCorrelationTelemetryInitializer` uppdateringar hello `Operation.Id` kontextegenskap telemetri postobjekt spåras när en begäran med hello genereras automatiskt `RequestTelemetry.Id`.</span><span class="sxs-lookup"><span data-stu-id="df084-188">`OperationIdTelemetryInitializer` or `OperationCorrelationTelemetryInitializer` updates hello `Operation.Id` context property of all telemetry items tracked while handling a request with hello automatically generated `RequestTelemetry.Id`.</span></span>
* <span data-ttu-id="df084-189">`SessionTelemetryInitializer`uppdateringar hello `Id` -egenskapen för hello `Session` kontext för alla telemetri objekt med värdet som har extraherats från hello `ai_session` cookie som genererats av hello ApplicationInsights JavaScript instrumentation kod som körs i hello användarens webbläsare.</span><span class="sxs-lookup"><span data-stu-id="df084-189">`SessionTelemetryInitializer` updates hello `Id` property of hello `Session` context for all telemetry items with value extracted from hello `ai_session` cookie generated by hello ApplicationInsights JavaScript instrumentation code running in hello user's browser.</span></span>
* <span data-ttu-id="df084-190">`SyntheticTelemetryInitializer`eller `SyntheticUserAgentTelemetryInitializer` uppdateringar hello `User`, `Session` och `Operation` kontexter egenskaper för alla objekt i telemetri spåras vid hantering av en begäran från en syntetisk källa, t.ex. en tillgänglighet testa eller Sök motorn bot.</span><span class="sxs-lookup"><span data-stu-id="df084-190">`SyntheticTelemetryInitializer` or `SyntheticUserAgentTelemetryInitializer` updates hello `User`, `Session` and `Operation` contexts properties of all telemetry items tracked when handling a request from a synthetic source, such as an availability test or search engine bot.</span></span> <span data-ttu-id="df084-191">Som standard [Metrics Explorer](app-insights-metrics-explorer.md) visar inte syntetiska telemetri.</span><span class="sxs-lookup"><span data-stu-id="df084-191">By default, [Metrics Explorer](app-insights-metrics-explorer.md) does not display synthetic telemetry.</span></span>

    <span data-ttu-id="df084-192">Hej `<Filters>` ange identifierar egenskaperna för hello begäranden.</span><span class="sxs-lookup"><span data-stu-id="df084-192">hello `<Filters>` set identifying properties of hello requests.</span></span>
* <span data-ttu-id="df084-193">`UserAgentTelemetryInitializer`uppdateringar hello `UserAgent` -egenskapen för hello `User` kontexten för alla telemetri objekt baserat på hello `User-Agent` HTTP-huvud för hello-begäran.</span><span class="sxs-lookup"><span data-stu-id="df084-193">`UserAgentTelemetryInitializer` updates hello `UserAgent` property of hello `User` context of all telemetry items based on hello `User-Agent` HTTP header of hello request.</span></span>
* <span data-ttu-id="df084-194">`UserTelemetryInitializer`uppdateringar hello `Id` och `AcquisitionDate` egenskaper för `User` kontext för alla telemetri objekt med värden som extraheras från hello `ai_user` cookie som genererats av hello Application Insights JavaScript instrumentation kod som körs i hello användarens webbläsare.</span><span class="sxs-lookup"><span data-stu-id="df084-194">`UserTelemetryInitializer` updates hello `Id` and `AcquisitionDate` properties of `User` context for all telemetry items with values extracted from hello `ai_user` cookie generated by hello Application Insights JavaScript instrumentation code running in hello user's browser.</span></span>
* <span data-ttu-id="df084-195">`WebTestTelemetryInitializer`Anger hello användar-id, sessions-id och syntetiska datakällans egenskaper för HTTP-begäranden som kommer från [tillgänglighetstester](app-insights-monitor-web-app-availability.md).</span><span class="sxs-lookup"><span data-stu-id="df084-195">`WebTestTelemetryInitializer` sets hello user id, session id and synthetic source properties for HTTP requests that come from [availability tests](app-insights-monitor-web-app-availability.md).</span></span>
  <span data-ttu-id="df084-196">Hej `<Filters>` ange identifierar egenskaperna för hello begäranden.</span><span class="sxs-lookup"><span data-stu-id="df084-196">hello `<Filters>` set identifying properties of hello requests.</span></span>

<span data-ttu-id="df084-197">För .NET-program som körs i Service Fabric kan du inkludera hello `Microsoft.ApplicationInsights.ServiceFabric` NuGet-paketet.</span><span class="sxs-lookup"><span data-stu-id="df084-197">For .NET applications running in Service Fabric, you can include hello `Microsoft.ApplicationInsights.ServiceFabric` NuGet package.</span></span> <span data-ttu-id="df084-198">Det här paketet innehåller en `FabricTelemetryInitializer`, vilket ger Service Fabric egenskaper tootelemetry objekt.</span><span class="sxs-lookup"><span data-stu-id="df084-198">This package includes a `FabricTelemetryInitializer`, which adds Service Fabric properties tootelemetry items.</span></span> <span data-ttu-id="df084-199">Mer information finns i hello [GitHub-sidan](https://go.microsoft.com/fwlink/?linkid=848457) om hello egenskaper lagts till av den här NuGet-paketet.</span><span class="sxs-lookup"><span data-stu-id="df084-199">For more information, see hello [GitHub page](https://go.microsoft.com/fwlink/?linkid=848457) about hello properties added by this NuGet package.</span></span>

## <a name="telemetry-processors-aspnet"></a><span data-ttu-id="df084-200">Telemetri processorer (ASP.NET)</span><span class="sxs-lookup"><span data-stu-id="df084-200">Telemetry Processors (ASP.NET)</span></span>
<span data-ttu-id="df084-201">Telemetri processorer kan filtrera och ändra varje telemetri objekt precis innan den skickas från hello SDK toohello portalen.</span><span class="sxs-lookup"><span data-stu-id="df084-201">Telemetry processors can filter and modify each telemetry item just before it is sent from hello SDK toohello portal.</span></span>

<span data-ttu-id="df084-202">Du kan [skriva telemetri processorerna](app-insights-api-filtering-sampling.md#filtering).</span><span class="sxs-lookup"><span data-stu-id="df084-202">You can [write your own telemetry processors](app-insights-api-filtering-sampling.md#filtering).</span></span>

#### <a name="adaptive-sampling-telemetry-processor-from-200-beta3"></a><span data-ttu-id="df084-203">Anpassningsbar provtagning telemetri processor (från 2.0.0-beta3)</span><span class="sxs-lookup"><span data-stu-id="df084-203">Adaptive sampling telemetry processor (from 2.0.0-beta3)</span></span>
<span data-ttu-id="df084-204">Den här funktionen är aktiverad som standard.</span><span class="sxs-lookup"><span data-stu-id="df084-204">This is enabled by default.</span></span> <span data-ttu-id="df084-205">Om appen skickar en mängd telemetri, denna processor tar bort vissa av den.</span><span class="sxs-lookup"><span data-stu-id="df084-205">If your app sends a lot of telemetry, this processor removes some of it.</span></span>

```xml

    <TelemetryProcessors>
      <Add Type="Microsoft.ApplicationInsights.WindowsServer.TelemetryChannel.AdaptiveSamplingTelemetryProcessor, Microsoft.AI.ServerTelemetryChannel">
        <MaxTelemetryItemsPerSecond>5</MaxTelemetryItemsPerSecond>
      </Add>
    </TelemetryProcessors>

```

<span data-ttu-id="df084-206">hello parametern innehåller hello mål som hello algoritmen försöker tooachieve.</span><span class="sxs-lookup"><span data-stu-id="df084-206">hello parameter provides hello target that hello algorithm tries tooachieve.</span></span> <span data-ttu-id="df084-207">Varje instans av hello SDK fungerar oberoende av varandra, så om din server är ett kluster på flera datorer, hello faktiska telemetrivolym ska multipliceras därefter.</span><span class="sxs-lookup"><span data-stu-id="df084-207">Each instance of hello SDK works independently, so if your server is a cluster of several machines, hello actual volume of telemetry will be multiplied accordingly.</span></span>

<span data-ttu-id="df084-208">[Lär dig mer om sampling](app-insights-sampling.md).</span><span class="sxs-lookup"><span data-stu-id="df084-208">[Learn more about sampling](app-insights-sampling.md).</span></span>

#### <a name="fixed-rate-sampling-telemetry-processor-from-200-beta1"></a><span data-ttu-id="df084-209">Fast räntesats provtagning telemetri processor (från 2.0.0-beta1)</span><span class="sxs-lookup"><span data-stu-id="df084-209">Fixed-rate sampling telemetry processor (from 2.0.0-beta1)</span></span>
<span data-ttu-id="df084-210">Det finns också en standard [provtagning telemetri processor](app-insights-api-filtering-sampling.md) (från 2.0.1):</span><span class="sxs-lookup"><span data-stu-id="df084-210">There is also a standard [sampling telemetry processor](app-insights-api-filtering-sampling.md) (from 2.0.1):</span></span>

```XML

    <TelemetryProcessors>
     <Add Type="Microsoft.ApplicationInsights.WindowsServer.TelemetryChannel.SamplingTelemetryProcessor, Microsoft.AI.ServerTelemetryChannel">

     <!-- Set a percentage close too100/N where N is an integer. -->
     <!-- E.g. 50 (=100/2), 33.33 (=100/3), 25 (=100/4), 20, 1 (=100/100), 0.1 (=100/1000) -->
     <SamplingPercentage>10</SamplingPercentage>
     </Add>
   </TelemetryProcessors>

```



## <a name="channel-parameters-java"></a><span data-ttu-id="df084-211">Kanalparametrar (Java)</span><span class="sxs-lookup"><span data-stu-id="df084-211">Channel parameters (Java)</span></span>
<span data-ttu-id="df084-212">Dessa parametrar påverkar hur hello Java SDK ska lagra och rensa hello telemetridata den samlar in.</span><span class="sxs-lookup"><span data-stu-id="df084-212">These parameters affect how hello Java SDK should store and flush hello telemetry data that it collects.</span></span>

#### <a name="maxtelemetrybuffercapacity"></a><span data-ttu-id="df084-213">MaxTelemetryBufferCapacity</span><span class="sxs-lookup"><span data-stu-id="df084-213">MaxTelemetryBufferCapacity</span></span>
<span data-ttu-id="df084-214">hello antal telemetri objekt som kan lagras i hello SDK InMemory-lagringen.</span><span class="sxs-lookup"><span data-stu-id="df084-214">hello number of telemetry items that can be stored in hello SDK's in-memory storage.</span></span> <span data-ttu-id="df084-215">När antalet har uppnåtts hello telemetri bufferten töms - som är hello telemetri objekt skickas toohello Application Insights-server.</span><span class="sxs-lookup"><span data-stu-id="df084-215">When this number is reached, hello telemetry buffer is flushed - that is, hello telemetry items are sent toohello Application Insights server.</span></span>

* <span data-ttu-id="df084-216">Min: 1</span><span class="sxs-lookup"><span data-stu-id="df084-216">Min: 1</span></span>
* <span data-ttu-id="df084-217">Max: 1000</span><span class="sxs-lookup"><span data-stu-id="df084-217">Max: 1000</span></span>
* <span data-ttu-id="df084-218">Standard: 500</span><span class="sxs-lookup"><span data-stu-id="df084-218">Default: 500</span></span>

```

  <ApplicationInsights>
      ...
      <Channel>
       <MaxTelemetryBufferCapacity>100</MaxTelemetryBufferCapacity>
      </Channel>
      ...
  </ApplicationInsights>
```

#### <a name="flushintervalinseconds"></a><span data-ttu-id="df084-219">FlushIntervalInSeconds</span><span class="sxs-lookup"><span data-stu-id="df084-219">FlushIntervalInSeconds</span></span>
<span data-ttu-id="df084-220">Anger hur ofta hello data som lagras i minnet i hello ska vara en (skickade tooApplication insikter).</span><span class="sxs-lookup"><span data-stu-id="df084-220">Determines how often hello data that is stored in hello in-memory storage should be flushed (sent tooApplication Insights).</span></span>

* <span data-ttu-id="df084-221">Min: 1</span><span class="sxs-lookup"><span data-stu-id="df084-221">Min: 1</span></span>
* <span data-ttu-id="df084-222">Max: 300</span><span class="sxs-lookup"><span data-stu-id="df084-222">Max: 300</span></span>
* <span data-ttu-id="df084-223">Standard: 5</span><span class="sxs-lookup"><span data-stu-id="df084-223">Default: 5</span></span>

```

    <ApplicationInsights>
      ...
      <Channel>
        <FlushIntervalInSeconds>100</FlushIntervalInSeconds>
      </Channel>
      ...
    </ApplicationInsights>
```

#### <a name="maxtransmissionstoragecapacityinmb"></a><span data-ttu-id="df084-224">MaxTransmissionStorageCapacityInMB</span><span class="sxs-lookup"><span data-stu-id="df084-224">MaxTransmissionStorageCapacityInMB</span></span>
<span data-ttu-id="df084-225">Anger hello maximal storlek i MB som har tilldelats toohello beständig lagring på hello lokal disk.</span><span class="sxs-lookup"><span data-stu-id="df084-225">Determines hello maximum size in MB that is allotted toohello persistent storage on hello local disk.</span></span> <span data-ttu-id="df084-226">Den här används för bestående telemetri objekt som inte kunde toobe överförs toohello Application Insights slutpunkt.</span><span class="sxs-lookup"><span data-stu-id="df084-226">This storage is used for persisting telemetry items that failed toobe transmitted toohello Application Insights endpoint.</span></span> <span data-ttu-id="df084-227">När hello lagringsstorlek har uppfyllts, ignoreras nya telemetri objekt.</span><span class="sxs-lookup"><span data-stu-id="df084-227">When hello storage size has been met, new telemetry items will be discarded.</span></span>

* <span data-ttu-id="df084-228">Min: 1</span><span class="sxs-lookup"><span data-stu-id="df084-228">Min: 1</span></span>
* <span data-ttu-id="df084-229">Max: 100</span><span class="sxs-lookup"><span data-stu-id="df084-229">Max: 100</span></span>
* <span data-ttu-id="df084-230">Standard: 10</span><span class="sxs-lookup"><span data-stu-id="df084-230">Default: 10</span></span>

```

   <ApplicationInsights>
      ...
      <Channel>
        <MaxTransmissionStorageCapacityInMB>50</MaxTransmissionStorageCapacityInMB>
      </Channel>
      ...
   </ApplicationInsights>
```



## <a name="instrumentationkey"></a><span data-ttu-id="df084-231">InstrumentationKey</span><span class="sxs-lookup"><span data-stu-id="df084-231">InstrumentationKey</span></span>
<span data-ttu-id="df084-232">Detta avgör hello Application Insights-resurs där dina data visas.</span><span class="sxs-lookup"><span data-stu-id="df084-232">This determines hello Application Insights resource in which your data appears.</span></span> <span data-ttu-id="df084-233">Normalt skapar du en separat resurs med en separat nyckel för var och en av dina program.</span><span class="sxs-lookup"><span data-stu-id="df084-233">Typically you create a separate resource, with a separate key, for each of your applications.</span></span>

<span data-ttu-id="df084-234">Om du vill tooset hello nyckeln dynamiskt – till exempel om du vill att toosend resultatet från din programresurser toodifferent - du utelämnar hello nyckeln från hello konfigurationsfilen och ange i koden i stället.</span><span class="sxs-lookup"><span data-stu-id="df084-234">If you want tooset hello key dynamically - for example if you want toosend results from your application toodifferent resources - you can omit hello key from hello configuration file, and set it in code instead.</span></span>

<span data-ttu-id="df084-235">tooset hello nyckel för alla instanser av TelemetryClient, inklusive standard telemetri moduler, ange hello nyckeln i TelemetryConfiguration.Active.</span><span class="sxs-lookup"><span data-stu-id="df084-235">tooset hello key for all instances of TelemetryClient, including standard telemetry modules, set hello key in TelemetryConfiguration.Active.</span></span> <span data-ttu-id="df084-236">Gör så här i en initieringsmetod, till exempel global.aspx.cs i en ASP.NET-tjänst:</span><span class="sxs-lookup"><span data-stu-id="df084-236">Do this in an initialization method, such as global.aspx.cs in an ASP.NET service:</span></span>

```C#

    protected void Application_Start()
    {
      Microsoft.ApplicationInsights.Extensibility.
        TelemetryConfiguration.Active.InstrumentationKey =
          // - for example -
          WebConfigurationManager.Settings["ikey"];
      //...
```

<span data-ttu-id="df084-237">Om du bara vill toosend en specifik uppsättning händelser tooa annan resurs kan du ange hello nyckel för en specifik TelemetryClient:</span><span class="sxs-lookup"><span data-stu-id="df084-237">If you just want toosend a specific set of events tooa different resource, you can set hello key for a specific TelemetryClient:</span></span>

```C#

    var tc = new TelemetryClient();
    tc.Context.InstrumentationKey = "----- my key ----";
    tc.TrackEvent("myEvent");
    // ...

```

<span data-ttu-id="df084-238">tooget en ny nyckel [skapar en ny resurs i hello Application Insights-portalen][new].</span><span class="sxs-lookup"><span data-stu-id="df084-238">tooget a new key, [create a new resource in hello Application Insights portal][new].</span></span>

## <a name="next-steps"></a><span data-ttu-id="df084-239">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="df084-239">Next steps</span></span>
<span data-ttu-id="df084-240">[Mer information om hello API][api].</span><span class="sxs-lookup"><span data-stu-id="df084-240">[Learn more about hello API][api].</span></span>

<!--Link references-->

[api]: app-insights-api-custom-events-metrics.md
[client]: app-insights-javascript.md
[diagnostic]: app-insights-diagnostic-search.md
[exceptions]: app-insights-asp-net-exceptions.md
[netlogs]: app-insights-asp-net-trace-logs.md
[new]: app-insights-create-new-resource.md
[redfield]: app-insights-monitor-performance-live-website-now.md
[start]: app-insights-overview.md
