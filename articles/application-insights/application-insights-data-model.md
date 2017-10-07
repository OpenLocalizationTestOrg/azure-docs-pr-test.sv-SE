---
title: aaaAzure Application Insights telemetri datamodellen | Microsoft Docs
description: Insikter modellen programdata
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
ms.openlocfilehash: 5610519c3db8ec68d6cf787639204fb79724f511
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="application-insights-telemetry-data-model"></a>Application Insights telemetri datamodellen

[Azure Application Insights](app-insights-overview.md) skickar telemetri från dina web application toohello Azure-portalen så att du kan analysera hello prestanda och användning av programmet. hello telemetri modellen är standardiserade så att det är möjligt toocreate plattform och språkoberoende övervakning. 

Data som samlas in av Application Insights modeller detta mönster för körning av vanliga program:

![Application Insights programmodell](./media/application-insights-data-model/application-insights-data-model.png)

hello är följande typer av telemetri används toomonitor hello körningen av din app. hello följande tre typer vanligtvis automatiskt samlas in av hello Application Insights SDK från hello web application framework:

* [**Begäran** ](application-insights-data-model-request-telemetry.md) -genererade toolog en begäran tas emot av din app. Till exempel hello Application Insights web SDK genererar automatiskt en begärandeobjekt telemetri för varje HTTP-begäran som tar emot ditt webbprogram. 

    En **åtgärden** är hello trådar av körningen som bearbetar en begäran. Du kan också [skriva kod](app-insights-api-custom-events-metrics.md#trackrequest) toomonitor andra typer av åtgärden, som en ”väcka” på en webbplats jobbet eller fungera som regelbundet bearbetar data.  Varje åtgärd har ett ID. Detta ID som kan använda too[group]((application-insights-correlation.md) all telemetri uppstår när din app bearbetar hello-begäran. Varje åtgärd antingen lyckas eller misslyckas och har en tidsperiod.
* [**Undantag** ](application-insights-data-model-exception-telemetry.md) -representerar vanligen ett undantag som orsakar en toofail för åtgärden.
* [**Beroende** ](application-insights-data-model-dependency-telemetry.md) -representerar ett anrop från appen tooan externa tjänsten eller lagring som en REST API eller SQL. ASP.NET beroende anrop tooSQL definieras av `System.Data`. Anrop tooHTTP slutpunkter definieras av `System.Net`. 

Application Insights innehåller tre ytterligare datatyper för anpassad telemetri:

* [Spåra](application-insights-data-model-trace-telemetry.md) – användas antingen direkt eller via en kort tooimplement diagnostikloggning med hjälp av ett ramverk för instrumentation som är bekant tooyou som `Log4Net` eller `System.Diagnostics`.
* [Händelsen](application-insights-data-model-event-telemetry.md) -används vanligtvis toocapture användarinteraktion med din tjänst tooanalyze användningsmönster.
* [Måttet](application-insights-data-model-metric-telemetry.md) -används tooreport periodiska skalära mätningar.

Varje telemetri-objekt kan definiera hello [omständighetsinformation](application-insights-data-model-context.md) som programmets version eller användaren sessions-id. Kontexten är en uppsättning strikt typkontroll fält som avblockeras vissa scenarier. När programversion har initierats korrekt kan Application Insights identifiera nya mönster i programmets beteende som samverkar med omdistributionen. Sessions-id kan vara används toocalculate hello avbrott eller en problemet inverkan på användarna. Räknar antalet distinkta av session-ID-värden för vissa misslyckades beroende, fel spårningen eller kritiskt undantag ger en god förståelse av konsekvenser.

Application Insights telemetri modellen definierar ett sätt för[korrelera](application-insights-correlation.md) telemetri toohello åtgärden som den är en del. En begäran kan ringa en SQL-databas och registreras diagnostik information. Du kan ange hello korrelation kontext för telemetri objekt som binder den bakre toohello begärandetelemetri.

## <a name="schema-improvements"></a>Förbättringar av schemat

Application Insights-datamodell är ett enkelt och grundläggande men kraftfullt sätt toomodel programmets telemetri. Vi strävar efter tookeep hello modellen enkel och smidig toosupport viktiga scenarier och Tillåt tooextend hello schemat för avancerad användning.

använda GitHub tooreport modell eller schemat problem och förslag [ApplicationInsights hem](https://github.com/Microsoft/ApplicationInsights-Home/labels/schema) databasen.

## <a name="next-steps"></a>Nästa steg

- [Skriv anpassad telemetri](app-insights-api-custom-events-metrics.md)
- Lär dig hur för[utöka och filtrera telemetri](app-insights-api-filtering-sampling.md).
- Använd [provtagning](app-insights-sampling.md) toominimize mängden telemetri baseras på datamodellen.
- Checka ut [plattformar](app-insights-platforms.md) stöds av Application Insights.
