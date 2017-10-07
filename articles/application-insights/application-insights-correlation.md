---
title: aaaAzure Application Insights telemetri korrelation | Microsoft Docs
description: Application Insights telemetri korrelation
services: application-insights
documentationcenter: .net
author: SergeyKanzhelev
manager: carmonm
ms.service: application-insights
ms.workload: TBD
ms.tgt_pltfrm: ibiza
ms.devlang: multiple
ms.topic: article
ms.date: 04/25/2017
ms.author: bwren
ms.openlocfilehash: 3ed8c589d237cac5daceac939ca893b7d81a2967
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="telemetry-correlation-in-application-insights"></a>Korrelation telemetri i Application Insights

I hello world micro tjänster kräver varje logisk åtgärd arbete i olika komponenter i hello-tjänsten. Var och en av de här komponenterna separat kan övervakas av [Programinsikter](app-insights-overview.md). hello webbkomponenten appen kommunicerar med providern komponenten toovalidate användarens autentiseringsuppgifter och med hello API komponenten tooget data för visualisering. hello API-filkomponenten i sin tur kan fråga efter data från andra tjänster och använda cache-provider-komponenter och meddela hello fakturering komponenten med avseende på det här anropet. Application Insights stöder distribuerade telemetri korrelation. Det gör att du toodetect vilken komponent som ansvarar för fel eller prestandaförsämring.

Den här artikeln förklarar hello-datamodell som används av Application Insights toocorrelate telemetri som skickas av flera komponenter. Den omfattar hello kontexten spridningen metoder och protokoll. Den omfattar också hello implementering av hello korrelation begrepp på olika språk och plattformar.

## <a name="telemetry-correlation-data-model"></a>Telemetri Korrelations-datamodell

Application Insights definierar en [datamodellen](application-insights-data-model.md) för distribuerade telemetri korrelation. tooassociate telemetri med hello logisk åtgärd, varje telemetri objekt har en kontextfält med namnet `operation_Id`. Den här identifieraren delas av alla telemetri objekt i hello distribuerade spårning. Så även om förlust av telemetri från ett enda lager kan fortfarande du associera telemetri som rapporterats av andra komponenter.

Distribuerad logisk åtgärd består vanligtvis av en uppsättning mindre operations - begäranden som bearbetas av en av hello komponenter. Dessa åtgärder har definierats av [begära telemetri](application-insights-data-model-request-telemetry.md). Varje begärandetelemetri har sin egen `id` som globalt unikt identifierar. Och all telemetri - spårningar, undantag, etc. som associerats med denna begäran bör ange hello `operation_parentId` toohello värdet för hello begäran `id`.

Varje utgående åtgärd som representeras av HTTP-anropet tooanother komponenten [beroendetelemetri](application-insights-data-model-dependency-telemetry.md). Beroendetelemetri definierar också sin egen `id` som är globalt unikt. Begärandetelemetri initieras av det här beroendeanropet används som `operation_parentId`.

Du kan skapa hello vy över distribuerade logiska åtgärden med hjälp av `operation_Id`, `operation_parentId`, och `request.id` med `dependency.id`. De här fälten också definiera hello orsakssamband ordning telemetri.

Spår från komponenter kan gå toohello olika lagringsobjekt i micro services-miljön. Varje komponent kan ha sin egen instrumentation nyckel i Application Insights. tooget telemetri för hello logisk åtgärd, måste tooquery data från varje lagring. När antalet lagringsobjekt är stort, behöver du toohave en ledtråd om var toolook nästa.

Application Insights-datamodell definierar två fält toosolve problemet: `request.source` och `dependency.target`. hello första fältet identifierar hello-komponent som initierade hello beroende begäran och hello andra identifierar vilken komponent som returnerade hello svar för hello beroendeanropet.


## <a name="example"></a>Exempel

Låt oss ta ett exempel på ett program BÖRS priser som visar hello aktuella priset på en börs med hello externa API kallas LAGERLISTA API. hello BÖRS priser programmet har en sida `Stock page` öppnas med hello klienten web webbläsare `GET /Home/Stock`. hello programmet frågor hello BÖRS API med hjälp av en HTTP-anrop `GET /api/stock/value`.

Du kan analysera resulterande telemetri som kör en fråga:

```
(requests | union dependencies | union pageViews) 
| where operation_Id == "STYz"
| project timestamp, itemType, name, id, operation_ParentId, operation_Id
```

I hello resultatet visa Observera att alla objekt i telemetri dela hello rot `operation_Id`. När ajax-anrop görs från hello sida - nytt unikt id `qJSXU` är tilldelade toohello beroendetelemetri och Pageview's id används som `operation_ParentId`. I sin tur serverbegäran använder ajax's id som `operation_ParentId`osv.

| itemType   | namn                      | id           | operation_ParentId | operation_Id |
|------------|---------------------------|--------------|--------------------|--------------|
| pageView   | Lagrets sida                |              | STYz               | STYz         |
| beroende | GET/Home/Stock           | qJSXU        | STYz               | STYz         |
| Begäran    | GET-Home/Stock            | KqKwlrSt9PA = | qJSXU              | STYz         |
| beroende | Hämta /api/stock/value      | bBrf2L7mm2g = | KqKwlrSt9PA =       | STYz         |

Nu när hello anropet `GET /api/stock/value` gjorts tooan extern tjänst du vill tooknow hello identiteten för servern. Du kan ange `dependency.target` fältet på lämpligt sätt. När externa hello-tjänsten inte stöder övervakning - `target` anges toohello värdnamnet för hello-tjänst som `stock-prices-api.com`. Men om tjänsten identifierar sig genom att returnera en fördefinierad HTTP-huvudet - `target` innehåller hello tjänstidentiteten som tillåter Application Insights toobuild distribuerade spårning genom att fråga telemetri från tjänsten. 

## <a name="correlation-headers"></a>Huvuden för korrelation

Vi arbetar på RFC förslag för hello [korrelation HTTP-protokollet](https://github.com/lmolkova/correlation/blob/master/http_protocol_proposal_v1.md). Den här förslag definierar två huvuden:

- `Request-Id`utföra hello globalt unikt id för hello anrop
- `Correlation-Context`-utföra hello namn värde-par samling hello distribuerade trace egenskaper

hello som standard definierar också två scheman för `Request-Id` generation - platta och hierarkiska. Hello flat schema, det är en välkänd `Id` key som definierats för hello `Correlation-Context` samling.

Application Insights definierar hello [tillägget](https://github.com/lmolkova/correlation/blob/master/http_protocol_proposal_v2.md) för hello korrelation HTTP-protokollet. Den använder `Request-Context` namn värde-par toopropagate hello uppsättning egenskaper som används av hello omedelbar anropare eller mottagarmetoderna. Application Insights SDK använder den här rubriken tooset `dependency.target` och `request.source` fält.

## <a name="open-tracing-and-application-insights"></a>Öppna spårning och Application Insights

[Öppna spårning](http://opentracing.io/) och Application Insights data modeller ser ut 

- `request`, `pageView` mappar för**Span** med`span.kind = server`
- `dependency`mappar för**Span** med`span.kind = client`
- `id`för en `request` och `dependency` mappar för**Span.Id**
- `operation_Id`mappar för**TraceId**
- `operation_ParentId`mappar för**referens** av typen`ChileOf`

Se [datamodellen](application-insights-data-model.md) för Application Insights typer och modell.

Se [specifikationen](https://github.com/opentracing/specification/blob/master/specification.md) och [semantic_conventions](https://github.com/opentracing/specification/blob/master/semantic_conventions.md) definitioner av öppna spårning begrepp.


## <a name="telemetry-correlation-in-net"></a>Telemetri korrelation i .NET

Med tiden .NET definierats många sätt toocorrelate telemetri och diagnostik loggar. Det finns `System.Diagnostics.CorrelationManager` så att tootrack [LogicalOperationStack och ActivityId](https://msdn.microsoft.com/library/system.diagnostics.correlationmanager.aspx). `System.Diagnostics.Tracing.EventSource`och Windows ETW definiera hello metoden [SetCurrentThreadActivityId](https://msdn.microsoft.com/library/system.diagnostics.tracing.eventsource.setcurrentthreadactivityid.aspx). `ILogger`använder [loggen scope](https://docs.microsoft.com/aspnet/core/fundamentals/logging#log-scopes). WCF- och HTTP-överföring in ”aktuella” kontexten spridning.

De här metoderna inte men aktivera stöd för automatisk distribuerade spårning. `DiagnosticsSource`är ett sätt toosupport automatisk mellan datorn korrelation. .NET-biblioteken stöder diagnostik och Tillåt automatisk mellan dator spridning av hello korrelation kontexten via hello transport som http.

Hej [guide tooActivities](https://github.com/dotnet/corefx/blob/master/src/System.Diagnostics.DiagnosticSource/src/ActivityUserGuide.md) förklarar hello grunderna i Aktivitetsspårning i diagnostik källan. 

ASP.NET Core 2.0 stöder extrahering av Http-huvuden och starta hello ny aktivitet. 

`System.Net.HttpClient`startversion `<fill in>` stöder automatisk injektion av hello korrelation Http-huvuden och spåra hello http-anrop som en aktivitet.

Det finns en ny Http-modul [Microsoft.AspNet.TelemetryCorrelation](https://www.nuget.org/packages/Microsoft.AspNet.TelemetryCorrelation/) för hello ASP.NET klassisk. Den här modulen implementerar telemetri korrelation med DiagnosticsSource. Startar aktivitet baserat på inkommande begärandehuvuden. Den också korrelerar telemetri från hello olika steg för behandling av begäranden. Även för hello fall när varje steg i processen för IIS körs på en annan hantera trådar.

Application Insights SDK startversion `2.4.0-beta1` använder DiagnosticsSource och aktiviteten toocollect telemetri och koppla den till hello pågående aktivitet. 

## <a name="next-steps"></a>Nästa steg

- [Skriv anpassad telemetri](app-insights-api-custom-events-metrics.md)
- Publicera alla komponenter i micro tjänsten på Application Insights. Checka ut [plattformar som stöds av](app-insights-platforms.md).
- Se [datamodellen](application-insights-data-model.md) för Application Insights typer och modell.
- Lär dig hur för[utöka och filtrera telemetri](app-insights-api-filtering-sampling.md).
