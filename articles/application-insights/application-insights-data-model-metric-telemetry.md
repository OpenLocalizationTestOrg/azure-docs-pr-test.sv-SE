---
title: "aaaAzure Application Insights telemetri datamodellen - måttet telemetri | Microsoft Docs"
description: "Application Insights datamodellen för mått telemetri"
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
ms.openlocfilehash: 005e218a8451007458185f1e457a20cee93fa630
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="metric-telemetry-application-insights-data-model"></a>Mått telemetri: Application Insights-datamodell

Det finns två typer av mått telemetri som stöds av [Programinsikter](app-insights-overview.md): enkel mätning och före aggregerade mått. Enskild mätning är bara ett namn och värde. Före aggregerade mått anger lägsta och högsta värdet för hello mått i hello aggregeringsintervall och standardavvikelsen för den.

Före aggregerade mått telemetri förutsätter aggregering perioden har en minut.

Det finns flera välkända mått namn som stöds av Application Insights. 

Måttet som representerar system och processen räknare:

| **.NET-namn**             | **Plattform storleksoberoende namn** | **REST API-namn** | **Beskrivning**
| ------------------------- | -------------------------- | ----------------- | ---------------- 
| `\Processor(_Total)\% Processor Time` | Pågående arbete... | [processorCpuPercentage](https://dev.applicationinsights.io/apiexplorer/metrics?appId=DEMO_APP&apiKey=DEMO_KEY&metricId=performanceCounters%2FprocessorCpuPercentage) | totala antalet datorer CPU
| `\Memory\Available Bytes`                 | Pågående arbete... | [memoryAvailableBytes](https://dev.applicationinsights.io/apiexplorer/metrics?appId=DEMO_APP&apiKey=DEMO_KEY&metricId=performanceCounters%2FmemoryAvailableBytes) | minne tillgängligt på disk
| `\Process(??APP_WIN32_PROC??)\% Processor Time` | Pågående arbete... | [processCpuPercentage](https://dev.applicationinsights.io/apiexplorer/metrics?appId=DEMO_APP&apiKey=DEMO_KEY&metricId=performanceCounters%2FprocessCpuPercentage) | CPU processens hello värd hello program
| `\Process(??APP_WIN32_PROC??)\Private Bytes`      | Pågående arbete... | [processPrivateBytes](https://dev.applicationinsights.io/apiexplorer/metrics?appId=DEMO_APP&apiKey=DEMO_KEY&metricId=performanceCounters%2FprocessPrivateBytes) | minne som används av hello process som är värd för programmet hello
| `\Process(??APP_WIN32_PROC??)\IO Data Bytes/sec` | Pågående arbete... | [processIOBytesPerSecond](https://dev.applicationinsights.io/apiexplorer/metrics?appId=DEMO_APP&apiKey=DEMO_KEY&metricId=performanceCounters%2FprocessIOBytesPerSecond) | antalet i/o-åtgärder körs med processen som är värd hello program
| `\ASP.NET Applications(??APP_W3SVC_PROC??)\Requests/Sec`             | Pågående arbete... | [requestsPerSecond](https://dev.applicationinsights.io/apiexplorer/metrics?appId=DEMO_APP&apiKey=DEMO_KEY&metricId=performanceCounters%2FrequestsPerSecond) | antalet begäranden som bearbetas av programmet 
| `\.NET CLR Exceptions(??APP_CLR_PROC??)\# of Exceps Thrown / sec`    | Pågående arbete... | [exceptionsPerSecond](https://dev.applicationinsights.io/apiexplorer/metrics?appId=DEMO_APP&apiKey=DEMO_KEY&metricId=performanceCounters%2FexceptionsPerSecond) | antal undantag som utlöses av program
| `\ASP.NET Applications(??APP_W3SVC_PROC??)\Request Execution Time`   | Pågående arbete... | [requestExecutionTime](https://dev.applicationinsights.io/apiexplorer/metrics?appId=DEMO_APP&apiKey=DEMO_KEY&metricId=performanceCounters%2FrequestExecutionTime) | begäranden om Genomsnittlig körningstid
| `\ASP.NET Applications(??APP_W3SVC_PROC??)\Requests In Application Queue` | Pågående arbete... | [requestsInQueue](https://dev.applicationinsights.io/apiexplorer/metrics?appId=DEMO_APP&apiKey=DEMO_KEY&metricId=performanceCounters%2FrequestsInQueue) | antalet begäranden som väntar hello i en kö

## <a name="name"></a>Namn

Namn för hello mått du vill att toosee i Application Insights-portalen och Användargränssnittet. 

## <a name="value"></a>Värde

Enskilt värde för mått. Summan av enskilda mått för hello aggregering.

## <a name="count"></a>Antal

Tjänstmåttets vikt för hello samman mått. Ska inte anges för ett mått.

## <a name="min"></a>Min

Minimivärdet för hello samman mått. Ska inte anges för ett mått.

## <a name="max"></a>Max.

Maximivärdet för hello samman mått. Ska inte anges för ett mått.

## <a name="standard-deviation"></a>Standardavvikelse

Standardavvikelsen för hello samman mått. Ska inte anges för ett mått.

## <a name="custom-properties"></a>Anpassade egenskaper

[!INCLUDE [application-insights-data-model-properties](../../includes/application-insights-data-model-properties.md)]

## <a name="next-steps"></a>Nästa steg

- Lär dig hur toouse [Application Insights API för anpassade händelser och mått](app-insights-api-custom-events-metrics.md#trackmetric).
- Se [datamodellen](application-insights-data-model.md) för Application Insights typer och modell.
- Checka ut [plattformar](app-insights-platforms.md) stöds av Application Insights.
