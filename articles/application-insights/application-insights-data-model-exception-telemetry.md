---
title: aaaAzure Application Insights telemetri datamodellen - Undantagstelemetri | Microsoft Docs
description: "Application Insights datamodellen för undantagstelemetri"
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
ms.openlocfilehash: 4c2b7d1ac3816d5623db9a35819a48a68a13a9cd
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="exception-telemetry-application-insights-data-model"></a>Undantagstelemetri: Application Insights-datamodell

I [Programinsikter](app-insights-overview.md), en instans av undantag representerar en hanterade eller ohanterade undantag som uppstod under körning av programmet hello som övervakas.

## <a name="problem-id"></a>Problem-Id

Identifierare för där hello undantag uppstod i koden. Används för att gruppera undantag. Vanligtvis en kombination av undantagstyp och en funktion från hello anropsstacken.

Maxlängd: 1024 tecken

## <a name="severity-level"></a>Allvarlighetsgrad

Spåra allvarlighetsgrad. Värdet kan vara `Verbose`, `Information`, `Warning`, `Error`, `Critical`.

## <a name="exception-details"></a>Undantagsinformation

(toobe utökad)

## <a name="custom-properties"></a>Anpassade egenskaper

[!INCLUDE [application-insights-data-model-properties](../../includes/application-insights-data-model-properties.md)]

## <a name="custom-measurements"></a>Anpassade mått

[!INCLUDE [application-insights-data-model-measurements](../../includes/application-insights-data-model-measurements.md)]

## <a name="next-steps"></a>Nästa steg

- Se [datamodellen](application-insights-data-model.md) för Application Insights typer och modell.
- Lär dig hur för[diagnostisera undantag i ditt webbprogram med Application Insights](app-insights-asp-net-exceptions.md).
- Checka ut [plattformar](app-insights-platforms.md) stöds av Application Insights.
