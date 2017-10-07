---
title: "aaaAzure Application Insights telemetri datamodellen - händelse telemetri | Microsoft Docs"
description: "Application Insights datamodellen för händelsen telemetri"
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
ms.openlocfilehash: cd7dc3c5f4f3df22b7a52ee79fcad566a27a9f4e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="event-telemetry-application-insights-data-model"></a>Händelsen telemetri: Application Insights-datamodell

Du kan skapa händelse telemetri objekt (i [Programinsikter](app-insights-overview.md)) toorepresent en händelse som påträffats i programmet. Vanligtvis är en användarinteraktion, såsom knappen Klicka eller ordna utcheckningen. Det kan också vara livscykel händelse som initiering eller konfiguration uppdatera. 

Händelser kan semantiskt, eller kanske inte korrelerade toorequests. Men om korrekt, är händelse telemetri viktigare än förfrågningar eller spår. Händelser som representerar företag telemetri och ska vara ett ämne tooseparate mindre aggressiv [provtagning](app-insights-api-filtering-sampling.md).

## <a name="name"></a>Namn

Händelsenamn. tooallow rätt gruppering och användbara mätvärden, begränsa ditt program så att den genererar ett litet antal separat händelsenamn. Till exempel inte använda ett separat namn för varje genererade instans av en händelse.

Maxlängd: 512 tecken

## <a name="custom-properties"></a>Anpassade egenskaper

[!INCLUDE [application-insights-data-model-properties](../../includes/application-insights-data-model-properties.md)]

## <a name="custom-measurements"></a>Anpassade mått

[!INCLUDE [application-insights-data-model-measurements](../../includes/application-insights-data-model-measurements.md)]

## <a name="next-steps"></a>Nästa steg

- Se [datamodellen](application-insights-data-model.md) för Application Insights typer och modell.
- [Skriva anpassade händelsen telemetri](app-insights-api-custom-events-metrics.md#trackevent)
- Checka ut [plattformar](app-insights-platforms.md) stöds av Application Insights.
