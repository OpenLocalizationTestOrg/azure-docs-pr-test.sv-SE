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
# <a name="monitor-your-nodejs-services-and-apps-with-application-insights"></a>Övervaka dina Node-js-tjänster och -appar med Application Insights

[Azure Application Insights](app-insights-overview.md) övervakar dina backend-tjänster och komponenter när du distribuerar toohelp du [identifiera och diagnostisera snabbt prestanda och andra problem](app-insights-detect-triage-diagnose.md). Använd det för Node.js-tjänster som finns var som helst: ditt datacenter, dina virtuella Azure-datorer och -webbappar och till och med andra offentliga moln.

tooreceive, lagra, och utforska dina data och övervakning, följ hello följande instruktioner tooinclude en agent i koden och ställa in en motsvarande Application Insights-resurs i Azure. hello agenten skickar data toothat resurs för ytterligare analys och undersökning.

hello Node.js agenten övervakar automatiskt inkommande och utgående HTTP-begäranden, flera system mätvärden och undantag. Från och med v0.20 kan den också övervaka vissa vanliga tredjepartspaket som `mongodb`, `mysql` och `redis`. Alla händelser relaterade till tooan inkommande HTTP-begäran korrelerade för snabbare felsökning.

Du kan övervaka flera aspekter av din app och systemet genom att manuellt instrumentering dem med hjälp av hello agent-API som beskrivs senare.

![Exempel på prestandaövervakningsdiagram](./media/app-insights-nodejs/10-perf.png)

## <a name="getting-started"></a>Komma igång

Vi går igenom hur du konfigurerar övervakning för en app eller tjänst.

### <a name="resource"></a> Konfigurera en App Insights-resurs

**Innan du börjar** ska du se till att ha en Azure-prenumeration eller [så skaffar du en kostnadsfritt][azure-free-offer]. Om din organisation redan har en Azure-prenumeration kan en administratör kan följa [instruktionerna] [ add-aad-user] tooadd du tooit.

[azure-free-offer]: https://azure.microsoft.com/en-us/free/
[add-aad-user]: https://docs.microsoft.com/en-us/azure/active-directory/active-directory-users-create-azure-portal

Nu logga in toohello [Azure-portalen] [ portal] och skapa en Application Insights-resurs som visas i följande hello - klickar du på ”nytt” > ”Developer tools” > ”Application Insights”. hello resurs inkluderar en slutpunkt för att ta emot telemetridata lagring för dessa data, sparade rapporter och instrumentpaneler, regeln och varningskonfigurationen och mycket mer.

![Skapa en App Insights-resurs](./media/app-insights-nodejs/03-new_appinsights_resource.png)

Välj ”Node.js-program” hello programmet listrutan hello resursen skapas på sidan. typ av app hello bestämmer hello standard instrumentpaneler och rapporter som skapas automatiskt. Oroa dig inte – alla App Insights-resurser kan i själva verket kan samla in data från alla språk och plattformar.

![Nytt App Insights-resursformulär](./media/app-insights-nodejs/04-create_appinsights_resource.png)

### <a name="agent"></a>Ställ in hello Node.js-agent

Nu är det tid tooinclude hello agent i din app så att den kan samla in data.
Starta genom att kopiera Instrumentation Resursnyckeln (kallas nedan tooas din `ikey`) från hello portal enligt nedan. hello App Insights systemet använder den här nyckeln toomap data tooyour Azure-resurs så du måste toospecify den i en miljövariabel eller din kod för hello agent toouse.  

![Kopiera instrumenteringsnyckel](./media/app-insights-nodejs/05-appinsights_ikey_portal.png)

Lägg till hello Node.js agent biblioteket tooyour appens beroenden via package.json. Kör från hello rotmappen för din app:

```bash
npm install applicationinsights --save
```

Nu måste tooexplicitly belastningen hello biblioteket i koden. Eftersom hello agent lägger in instrumentation till många bibliotek, bör du läsa den så snart som möjligt, även innan andra `require` instruktioner. tooget igång genom att lägga till hello överst i din första .js-fil:

```javascript
const appInsights = require("applicationinsights");
appInsights.setup("<instrumentation_key>");
appInsights.start();
```

Hej `setup` metod konfigurerar hello instrumentation nyckeln (och därmed Azure-resurs) toobe som används som standard för alla spårade objekt. Anropa `start` när konfigurationen är klar toobegin samlar in och skicka telemetridata.

Du kan också tillhandahålla en ikey via hello miljövariabeln APPINSIGHTS\_INSTRUMENTATIONKEY i stället för att manuellt för `setup()` eller `getClient()`. Den här metoden kan du behålla ikeys utanför allokerat källkoden och toospecify olika ikeys för olika miljöer.

Ytterligare konfigurationsalternativ dokumenteras nedan.

Du kan försöka hello agent utan att skicka telemetri genom att ange hello instrumentation viktiga tooany icke-tom sträng.

### <a name="monitor"></a> Övervaka din app

hello agent samlas automatiskt telemetri om hello Node.js runtime och vissa vanliga moduler från tredje part. Använd ditt program nu toogenerate vissa av dessa data.

Sedan hello [Azure-portalen] [ portal] Bläddra toohello Application Insights-resurs som du skapade tidigare och leta efter först några datapunkter hello översikt tidslinjen som hello följande bild. Klicka dig igenom hello diagram för mer information.

![Första datapunkter](./media/app-insights-nodejs/12-first-perf.png)

Klicka på identifierade för din app, som i följande bild hello hello programmet kartan knappen tooview hello topologi. Klicka här för komponenter i hello mappning för mer information.

![Mappning av enskild app](./media/app-insights-nodejs/06-appinsights_appmap.png)

Mer information om din app och felsöka problem med att använda hello andra vyer som är tillgängliga under hello ”Undersök” avsnittet.

![Avsnittet Undersök](./media/app-insights-nodejs/07-appinsights_investigate_blades.png)

#### <a name="no-data"></a>Ser du inga data?

Eftersom hello agent batchar data att skicka kan det finnas en fördröjning innan objekt visas i hello-portalen. Om du inte ser prova några av hello följande korrigeringar av data i din resurs:

* Använda hello programmet ännu mer; vidta fler åtgärder toogenerate mer telemetri.
* Klicka på **uppdatera** i hello portal resursvyer. Diagram Uppdatera automatiskt själva regelbundet men uppdatera tvingar denna toohappen omedelbart.
* Verifiera att [nödvändiga utgående portar](app-insights-ip-addresses.md) är öppna.
* Öppna hello [Sök](app-insights-diagnostic-search.md) panelen och leta efter enskilda händelser.
* Kontrollera hello [vanliga frågor och svar][].


## <a name="agent-configuration"></a>Agentkonfiguration

Följande är hello agentens konfigurationsmetoder och standardvärdena.

toofully korrelera händelser i en tjänst vara säker på att tooset `.setAutoDependencyCorrelation(true)`. Detta gör att hello agent tootrack kontexten över asynkrona återanrop i Node.js.

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

## <a name="agent-api"></a>Agent-API

<!-- TODO: Fully document agent API. -->

hello .NET agent API beskrivs utförligt [här](app-insights-api-custom-events-metrics.md).

Du kan spåra alla begäran, händelse, mått eller undantag hello Application Insights Node.js-klienten. hello exemplet nedan visar några av hello tillgängliga API: er.

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

### <a name="track-your-dependencies"></a>Spåra dina beroenden

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

### <a name="add-a-custom-property-tooall-events"></a>Lägga till en anpassad egenskap tooall händelser

```javascript
appInsights.client.commonProperties = {
    environment: process.env.SOME_ENV_VARIABLE
};
```

### <a name="track-http-get-requests"></a>Spåra HTTP GET-begäranden

```javascript
var server = http.createServer((req, res) => {
    if ( req.method === "GET" ) {
            appInsights.client.trackRequest(req, res);
    }
    // other work here....
    res.end();
});
```

### <a name="track-server-startup-time"></a>Spåra starttiden för server

```javascript
let start = Date.now();
server.on("listening", () => {
    let duration = Date.now() - start;
    appInsights.client.trackMetric("server startup time", duration);
});
```

## <a name="more-resources"></a>Fler resurser

* [Övervaka dina telemetri hello-portalen](app-insights-dashboards.md)
* [Skriv Analytics-frågor via din telemetri](app-insights-analytics-tour.md)

<!--references-->

[portal]: https://portal.azure.com/
[vanliga frågor och svar]: app-insights-troubleshoot-faq.md
