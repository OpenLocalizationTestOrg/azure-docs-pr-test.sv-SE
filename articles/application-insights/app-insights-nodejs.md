---
title: "aaaMonitor Node.js tjänster med Azure Application Insights | Microsoft Docs"
description: "Övervaka prestanda- och diagnostiseringsproblem i Node.js-tjänster med Application Insights."
services: application-insights
documentationcenter: nodejs
author: joshgav
manager: carmonm
ms.assetid: 2ec7f809-5e1a-41cf-9fcd-d0ed4bebd08c
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: get-started-article
ms.date: 05/01/2017
ms.author: bwren
ms.openlocfilehash: 0a7e66990cd4d3a2fcaf3fa779adb336c861f8ce
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="monitor-your-nodejs-services-and-apps-with-application-insights"></a><span data-ttu-id="b8b60-103">Övervaka dina Node-js-tjänster och -appar med Application Insights</span><span class="sxs-lookup"><span data-stu-id="b8b60-103">Monitor your Node.js services and apps with Application Insights</span></span>

<span data-ttu-id="b8b60-104">[Azure Application Insights](app-insights-overview.md) övervakar dina backend-tjänster och komponenter när du distribuerar toohelp du [identifiera och diagnostisera snabbt prestanda och andra problem](app-insights-detect-triage-diagnose.md).</span><span class="sxs-lookup"><span data-stu-id="b8b60-104">[Azure Application Insights](app-insights-overview.md) monitors your backend services and components after you deploy them toohelp you [discover and rapidly diagnose performance and other issues](app-insights-detect-triage-diagnose.md).</span></span> <span data-ttu-id="b8b60-105">Använd det för Node.js-tjänster som finns var som helst: ditt datacenter, dina virtuella Azure-datorer och -webbappar och till och med andra offentliga moln.</span><span class="sxs-lookup"><span data-stu-id="b8b60-105">Use it for Node.js services hosted anywhere: your datacenter, Azure VMs and Web Apps, and even other public clouds.</span></span>

<span data-ttu-id="b8b60-106">tooreceive, lagra, och utforska dina data och övervakning, följ hello följande instruktioner tooinclude en agent i koden och ställa in en motsvarande Application Insights-resurs i Azure.</span><span class="sxs-lookup"><span data-stu-id="b8b60-106">tooreceive, store, and explore your monitoring data, follow hello following instructions tooinclude an agent in your code and set up a corresponding Application Insights resource in Azure.</span></span> <span data-ttu-id="b8b60-107">hello agenten skickar data toothat resurs för ytterligare analys och undersökning.</span><span class="sxs-lookup"><span data-stu-id="b8b60-107">hello agent sends data toothat resource for further analysis and exploration.</span></span>

<span data-ttu-id="b8b60-108">hello Node.js agenten övervakar automatiskt inkommande och utgående HTTP-begäranden, flera system mätvärden och undantag.</span><span class="sxs-lookup"><span data-stu-id="b8b60-108">hello Node.js agent can automatically monitor incoming and outgoing HTTP requests, several system metrics, and exceptions.</span></span> <span data-ttu-id="b8b60-109">Från och med v0.20 kan den också övervaka vissa vanliga tredjepartspaket som `mongodb`, `mysql` och `redis`.</span><span class="sxs-lookup"><span data-stu-id="b8b60-109">Beginning in v0.20, it can also monitor some common third-party packages such as `mongodb`, `mysql`, and `redis`.</span></span> <span data-ttu-id="b8b60-110">Alla händelser relaterade till tooan inkommande HTTP-begäran korrelerade för snabbare felsökning.</span><span class="sxs-lookup"><span data-stu-id="b8b60-110">All events related tooan incoming HTTP request are correlated for faster troubleshooting.</span></span>

<span data-ttu-id="b8b60-111">Du kan övervaka flera aspekter av din app och systemet genom att manuellt instrumentering dem med hjälp av hello agent-API som beskrivs senare.</span><span class="sxs-lookup"><span data-stu-id="b8b60-111">You can monitor more aspects of your app and system by manually instrumenting them using hello agent API described later.</span></span>

![Exempel på prestandaövervakningsdiagram](./media/app-insights-nodejs/10-perf.png)

## <a name="getting-started"></a><span data-ttu-id="b8b60-113">Komma igång</span><span class="sxs-lookup"><span data-stu-id="b8b60-113">Getting Started</span></span>

<span data-ttu-id="b8b60-114">Vi går igenom hur du konfigurerar övervakning för en app eller tjänst.</span><span class="sxs-lookup"><span data-stu-id="b8b60-114">Let's step through setting up monitoring for an app or service.</span></span>

### <span data-ttu-id="b8b60-115"><a name="resource"></a> Konfigurera en App Insights-resurs</span><span class="sxs-lookup"><span data-stu-id="b8b60-115"><a name="resource"></a> Set up an App Insights resource</span></span>

<span data-ttu-id="b8b60-116">**Innan du börjar** ska du se till att ha en Azure-prenumeration eller [så skaffar du en kostnadsfritt][azure-free-offer].</span><span class="sxs-lookup"><span data-stu-id="b8b60-116">**Before you start**, make sure you have an Azure subscription or [get a new one for free][azure-free-offer].</span></span> <span data-ttu-id="b8b60-117">Om din organisation redan har en Azure-prenumeration kan en administratör kan följa [instruktionerna] [ add-aad-user] tooadd du tooit.</span><span class="sxs-lookup"><span data-stu-id="b8b60-117">If your organization already has an Azure subscription, an administrator can follow [these instructions][add-aad-user] tooadd you tooit.</span></span>

[azure-free-offer]: https://azure.microsoft.com/en-us/free/
[add-aad-user]: https://docs.microsoft.com/en-us/azure/active-directory/active-directory-users-create-azure-portal

<span data-ttu-id="b8b60-118">Nu logga in toohello [Azure-portalen] [ portal] och skapa en Application Insights-resurs som visas i följande hello - klickar du på ”nytt” > ”Developer tools” > ”Application Insights”.</span><span class="sxs-lookup"><span data-stu-id="b8b60-118">Now log in toohello [Azure portal][portal] and create an Application Insights resource as illustrated in hello following - click "New" > "Developer tools" > "Application Insights".</span></span> <span data-ttu-id="b8b60-119">hello resurs inkluderar en slutpunkt för att ta emot telemetridata lagring för dessa data, sparade rapporter och instrumentpaneler, regeln och varningskonfigurationen och mycket mer.</span><span class="sxs-lookup"><span data-stu-id="b8b60-119">hello resource includes an endpoint for receiving telemetry data, storage for this data, saved reports and dashboards, rule and alert configuration, and more.</span></span>

![Skapa en App Insights-resurs](./media/app-insights-nodejs/03-new_appinsights_resource.png)

<span data-ttu-id="b8b60-121">Välj ”Node.js-program” hello programmet listrutan hello resursen skapas på sidan.</span><span class="sxs-lookup"><span data-stu-id="b8b60-121">On hello resource creation page, choose "Node.js Application" from hello application type drop-down.</span></span> <span data-ttu-id="b8b60-122">typ av app hello bestämmer hello standard instrumentpaneler och rapporter som skapas automatiskt.</span><span class="sxs-lookup"><span data-stu-id="b8b60-122">hello app type determines hello default set of dashboards and reports created for you.</span></span> <span data-ttu-id="b8b60-123">Oroa dig inte – alla App Insights-resurser kan i själva verket kan samla in data från alla språk och plattformar.</span><span class="sxs-lookup"><span data-stu-id="b8b60-123">Don't worry though, any App Insights resource can in fact collect data from any language and platform.</span></span>

![Nytt App Insights-resursformulär](./media/app-insights-nodejs/04-create_appinsights_resource.png)

### <span data-ttu-id="b8b60-125"><a name="agent"></a>Ställ in hello Node.js-agent</span><span class="sxs-lookup"><span data-stu-id="b8b60-125"><a name="agent"></a> Set up hello Node.js agent</span></span>

<span data-ttu-id="b8b60-126">Nu är det tid tooinclude hello agent i din app så att den kan samla in data.</span><span class="sxs-lookup"><span data-stu-id="b8b60-126">Now it's time tooinclude hello agent in your app so it can gather data.</span></span>
<span data-ttu-id="b8b60-127">Starta genom att kopiera Instrumentation Resursnyckeln (kallas nedan tooas din `ikey`) från hello portal enligt nedan.</span><span class="sxs-lookup"><span data-stu-id="b8b60-127">Start by copying your resource's Instrumentation Key (hereinafter referred tooas your `ikey`) from hello portal as shown below.</span></span> <span data-ttu-id="b8b60-128">hello App Insights systemet använder den här nyckeln toomap data tooyour Azure-resurs så du måste toospecify den i en miljövariabel eller din kod för hello agent toouse.</span><span class="sxs-lookup"><span data-stu-id="b8b60-128">hello App Insights system uses this key toomap data tooyour Azure resource so you need toospecify it in an environment variable or your code for hello agent toouse.</span></span>  

![Kopiera instrumenteringsnyckel](./media/app-insights-nodejs/05-appinsights_ikey_portal.png)

<span data-ttu-id="b8b60-130">Lägg till hello Node.js agent biblioteket tooyour appens beroenden via package.json.</span><span class="sxs-lookup"><span data-stu-id="b8b60-130">Next, add hello Node.js agent library tooyour app's dependencies via package.json.</span></span> <span data-ttu-id="b8b60-131">Kör från hello rotmappen för din app:</span><span class="sxs-lookup"><span data-stu-id="b8b60-131">From hello root folder of your app, run:</span></span>

```bash
npm install applicationinsights --save
```

<span data-ttu-id="b8b60-132">Nu måste tooexplicitly belastningen hello biblioteket i koden.</span><span class="sxs-lookup"><span data-stu-id="b8b60-132">Now you need tooexplicitly load hello library in your code.</span></span> <span data-ttu-id="b8b60-133">Eftersom hello agent lägger in instrumentation till många bibliotek, bör du läsa den så snart som möjligt, även innan andra `require` instruktioner.</span><span class="sxs-lookup"><span data-stu-id="b8b60-133">Because hello agent injects instrumentation into many other libraries, you should load it as early as possible, even before other `require` statements.</span></span> <span data-ttu-id="b8b60-134">tooget igång genom att lägga till hello överst i din första .js-fil:</span><span class="sxs-lookup"><span data-stu-id="b8b60-134">tooget started, at hello top of your first .js file add:</span></span>

```javascript
const appInsights = require("applicationinsights");
appInsights.setup("<instrumentation_key>");
appInsights.start();
```

<span data-ttu-id="b8b60-135">Hej `setup` metod konfigurerar hello instrumentation nyckeln (och därmed Azure-resurs) toobe som används som standard för alla spårade objekt.</span><span class="sxs-lookup"><span data-stu-id="b8b60-135">hello `setup` method configures hello instrumentation key (and thus Azure resource) toobe used by default for all tracked items.</span></span> <span data-ttu-id="b8b60-136">Anropa `start` när konfigurationen är klar toobegin samlar in och skicka telemetridata.</span><span class="sxs-lookup"><span data-stu-id="b8b60-136">Call `start` after configuration is finished toobegin gathering and sending telemetry data.</span></span>

<span data-ttu-id="b8b60-137">Du kan också tillhandahålla en ikey via hello miljövariabeln APPINSIGHTS\_INSTRUMENTATIONKEY i stället för att manuellt för `setup()` eller `getClient()`.</span><span class="sxs-lookup"><span data-stu-id="b8b60-137">You can also provide an ikey via hello environment variable APPINSIGHTS\_INSTRUMENTATIONKEY instead of passing it manually too `setup()` or `getClient()`.</span></span> <span data-ttu-id="b8b60-138">Den här metoden kan du behålla ikeys utanför allokerat källkoden och toospecify olika ikeys för olika miljöer.</span><span class="sxs-lookup"><span data-stu-id="b8b60-138">This practice lets you keep ikeys out of committed source code and toospecify different ikeys for different environments.</span></span>

<span data-ttu-id="b8b60-139">Ytterligare konfigurationsalternativ dokumenteras nedan.</span><span class="sxs-lookup"><span data-stu-id="b8b60-139">Additional configuration options are documented below.</span></span>

<span data-ttu-id="b8b60-140">Du kan försöka hello agent utan att skicka telemetri genom att ange hello instrumentation viktiga tooany icke-tom sträng.</span><span class="sxs-lookup"><span data-stu-id="b8b60-140">You can try hello agent without sending telemetry by setting hello instrumentation key tooany non-empty string.</span></span>

### <span data-ttu-id="b8b60-141"><a name="monitor"></a> Övervaka din app</span><span class="sxs-lookup"><span data-stu-id="b8b60-141"><a name="monitor"></a> Monitor your app</span></span>

<span data-ttu-id="b8b60-142">hello agent samlas automatiskt telemetri om hello Node.js runtime och vissa vanliga moduler från tredje part.</span><span class="sxs-lookup"><span data-stu-id="b8b60-142">hello agent automatically gathers telemetry about hello Node.js runtime and some common third-party modules.</span></span> <span data-ttu-id="b8b60-143">Använd ditt program nu toogenerate vissa av dessa data.</span><span class="sxs-lookup"><span data-stu-id="b8b60-143">Use your application now toogenerate some of this data.</span></span>

<span data-ttu-id="b8b60-144">Sedan hello [Azure-portalen] [ portal] Bläddra toohello Application Insights-resurs som du skapade tidigare och leta efter först några datapunkter hello översikt tidslinjen som hello följande bild.</span><span class="sxs-lookup"><span data-stu-id="b8b60-144">Then, in hello [Azure portal][portal] browse toohello Application Insights resource you created earlier and look for your first few data points in hello Overview timeline, as in hello following image.</span></span> <span data-ttu-id="b8b60-145">Klicka dig igenom hello diagram för mer information.</span><span class="sxs-lookup"><span data-stu-id="b8b60-145">Click through hello charts for more details.</span></span>

![Första datapunkter](./media/app-insights-nodejs/12-first-perf.png)

<span data-ttu-id="b8b60-147">Klicka på identifierade för din app, som i följande bild hello hello programmet kartan knappen tooview hello topologi.</span><span class="sxs-lookup"><span data-stu-id="b8b60-147">Click hello Application map button tooview hello topology discovered for your app, as in hello following image.</span></span> <span data-ttu-id="b8b60-148">Klicka här för komponenter i hello mappning för mer information.</span><span class="sxs-lookup"><span data-stu-id="b8b60-148">Click through components in hello map for more details.</span></span>

![Mappning av enskild app](./media/app-insights-nodejs/06-appinsights_appmap.png)

<span data-ttu-id="b8b60-150">Mer information om din app och felsöka problem med att använda hello andra vyer som är tillgängliga under hello ”Undersök” avsnittet.</span><span class="sxs-lookup"><span data-stu-id="b8b60-150">Learn more about your app and troubleshoot problems using hello other views available under hello "Investigate" section.</span></span>

![Avsnittet Undersök](./media/app-insights-nodejs/07-appinsights_investigate_blades.png)

#### <a name="no-data"></a><span data-ttu-id="b8b60-152">Ser du inga data?</span><span class="sxs-lookup"><span data-stu-id="b8b60-152">No data?</span></span>

<span data-ttu-id="b8b60-153">Eftersom hello agent batchar data att skicka kan det finnas en fördröjning innan objekt visas i hello-portalen.</span><span class="sxs-lookup"><span data-stu-id="b8b60-153">Because hello agent batches data for submission there may be a delay before items are displayed in hello portal.</span></span> <span data-ttu-id="b8b60-154">Om du inte ser prova några av hello följande korrigeringar av data i din resurs:</span><span class="sxs-lookup"><span data-stu-id="b8b60-154">If you don't see data in your resource try some of hello following fixes:</span></span>

* <span data-ttu-id="b8b60-155">Använda hello programmet ännu mer; vidta fler åtgärder toogenerate mer telemetri.</span><span class="sxs-lookup"><span data-stu-id="b8b60-155">Use hello application some more; take more actions toogenerate more telemetry.</span></span>
* <span data-ttu-id="b8b60-156">Klicka på **uppdatera** i hello portal resursvyer.</span><span class="sxs-lookup"><span data-stu-id="b8b60-156">Click **Refresh** in hello portal resource view.</span></span> <span data-ttu-id="b8b60-157">Diagram Uppdatera automatiskt själva regelbundet men uppdatera tvingar denna toohappen omedelbart.</span><span class="sxs-lookup"><span data-stu-id="b8b60-157">Charts automatically refresh themselves periodically but refreshing forces this toohappen immediately.</span></span>
* <span data-ttu-id="b8b60-158">Verifiera att [nödvändiga utgående portar](app-insights-ip-addresses.md) är öppna.</span><span class="sxs-lookup"><span data-stu-id="b8b60-158">Verify that [needed outgoing ports](app-insights-ip-addresses.md) are open.</span></span>
* <span data-ttu-id="b8b60-159">Öppna hello [Sök](app-insights-diagnostic-search.md) panelen och leta efter enskilda händelser.</span><span class="sxs-lookup"><span data-stu-id="b8b60-159">Open hello [Search](app-insights-diagnostic-search.md) tile and look for individual events.</span></span>
* <span data-ttu-id="b8b60-160">Kontrollera hello [vanliga frågor och svar][].</span><span class="sxs-lookup"><span data-stu-id="b8b60-160">Check hello [FAQ][].</span></span>


## <a name="agent-configuration"></a><span data-ttu-id="b8b60-161">Agentkonfiguration</span><span class="sxs-lookup"><span data-stu-id="b8b60-161">Agent Configuration</span></span>

<span data-ttu-id="b8b60-162">Följande är hello agentens konfigurationsmetoder och standardvärdena.</span><span class="sxs-lookup"><span data-stu-id="b8b60-162">Following are hello agent's configuration methods and their default values.</span></span>

<span data-ttu-id="b8b60-163">toofully korrelera händelser i en tjänst vara säker på att tooset `.setAutoDependencyCorrelation(true)`.</span><span class="sxs-lookup"><span data-stu-id="b8b60-163">toofully correlate events in a service, be sure tooset `.setAutoDependencyCorrelation(true)`.</span></span> <span data-ttu-id="b8b60-164">Detta gör att hello agent tootrack kontexten över asynkrona återanrop i Node.js.</span><span class="sxs-lookup"><span data-stu-id="b8b60-164">This allows hello agent tootrack context across asynchronous callbacks in Node.js.</span></span>

```javascript
const appInsights = require("applicationinsights");
appInsights.setup("<instrumentation_key>")
    .setAutoDependencyCorrelation(false)
    .setAutoCollectRequests(true)
    .setAutoCollectPerformance(true)
    .setAutoCollectExceptions(true)
    .setAutoCollectDependencies(true)
    .start();
```

## <a name="agent-api"></a><span data-ttu-id="b8b60-165">Agent-API</span><span class="sxs-lookup"><span data-stu-id="b8b60-165">Agent API</span></span>

<!-- TODO: Fully document agent API. -->

<span data-ttu-id="b8b60-166">hello .NET agent API beskrivs utförligt [här](app-insights-api-custom-events-metrics.md).</span><span class="sxs-lookup"><span data-stu-id="b8b60-166">hello .NET agent API is fully described [here](app-insights-api-custom-events-metrics.md).</span></span>

<span data-ttu-id="b8b60-167">Du kan spåra alla begäran, händelse, mått eller undantag hello Application Insights Node.js-klienten.</span><span class="sxs-lookup"><span data-stu-id="b8b60-167">You can track any request, event, metric, or exception using hello Application Insights Node.js client.</span></span> <span data-ttu-id="b8b60-168">hello exemplet nedan visar några av hello tillgängliga API: er.</span><span class="sxs-lookup"><span data-stu-id="b8b60-168">hello following example demonstrates some of hello available APIs.</span></span>

```javascript
let appInsights = require("applicationinsights");
appInsights.setup().start(); // assuming ikey in env var
let client = appInsights.getClient();

client.trackEvent("my custom event", {customProperty: "custom property value"});
client.trackException(new Error("handled exceptions can be logged with this method"));
client.trackMetric("custom metric", 3);
client.trackTrace("trace message");

let http = require("http");
http.createServer( (req, res) => {
  client.trackRequest(req, res); // Place at hello beginning of your request handler
});
```

### <a name="track-your-dependencies"></a><span data-ttu-id="b8b60-169">Spåra dina beroenden</span><span class="sxs-lookup"><span data-stu-id="b8b60-169">Track your dependencies</span></span>

```javascript
let appInsights = require("applicationinsights");
let client = appInsights.getClient();

var success = false;
let startTime = Date.now();
// execute dependency call here....
let duration = Date.now() - startTime;
success = true;

client.trackDependency("dependency name", "command name", duration, success);
```

### <a name="add-a-custom-property-tooall-events"></a><span data-ttu-id="b8b60-170">Lägga till en anpassad egenskap tooall händelser</span><span class="sxs-lookup"><span data-stu-id="b8b60-170">Add a custom property tooall events</span></span>

```javascript
appInsights.client.commonProperties = {
    environment: process.env.SOME_ENV_VARIABLE
};
```

### <a name="track-http-get-requests"></a><span data-ttu-id="b8b60-171">Spåra HTTP GET-begäranden</span><span class="sxs-lookup"><span data-stu-id="b8b60-171">Track HTTP GET requests</span></span>

```javascript
var server = http.createServer((req, res) => {
    if ( req.method === "GET" ) {
            appInsights.client.trackRequest(req, res);
    }
    // other work here....
    res.end();
});
```

### <a name="track-server-startup-time"></a><span data-ttu-id="b8b60-172">Spåra starttiden för server</span><span class="sxs-lookup"><span data-stu-id="b8b60-172">Track server startup time</span></span>

```javascript
let start = Date.now();
server.on("listening", () => {
    let duration = Date.now() - start;
    appInsights.client.trackMetric("server startup time", duration);
});
```

## <a name="more-resources"></a><span data-ttu-id="b8b60-173">Fler resurser</span><span class="sxs-lookup"><span data-stu-id="b8b60-173">More resources</span></span>

* [<span data-ttu-id="b8b60-174">Övervaka dina telemetri hello-portalen</span><span class="sxs-lookup"><span data-stu-id="b8b60-174">Monitor your telemetry in hello portal</span></span>](app-insights-dashboards.md)
* [<span data-ttu-id="b8b60-175">Skriv Analytics-frågor via din telemetri</span><span class="sxs-lookup"><span data-stu-id="b8b60-175">Write Analytics queries over your telemetry</span></span>](app-insights-analytics-tour.md)

<!--references-->

[portal]: https://portal.azure.com/
[vanliga frågor och svar]: app-insights-troubleshoot-faq.md
