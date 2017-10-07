---
title: aaaExploring HockeyApp data i Azure Application Insights | Microsoft Docs
description: "Analysera användnings- och prestanda för din Azure-app med Application Insights."
services: application-insights
documentationcenter: windows
author: CFreemanwa
manager: carmonm
ms.assetid: 97783cc6-67d6-465f-9926-cb9821f4176e
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 03/30/2017
ms.author: bwren
ms.openlocfilehash: ed7cf99b48f5ec78d6b401bb954cfcd014b9d1f4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="exploring-hockeyapp-data-in-application-insights"></a>Utforska HockeyApp data i Application Insights
[HockeyApp](https://azure.microsoft.com/services/hockeyapp/) rekommenderas hello plattform för övervakning av live stationära och mobila appar. Från HockeyApp, kan du skicka anpassade och spåra telemetri toomonitor användning och hjälpa diagnos (i tillägg toogetting kraschdata). Dataströmmen telemetri kan efterfrågas med hello kraftfulla [Analytics](app-insights-analytics.md) funktion i [Azure Application Insights](app-insights-overview.md). Dessutom kan du [exportera hello anpassade och spårningstelemetri](app-insights-export-telemetry.md). tooenable dessa funktioner, som du ställa in en brygga som vidarebefordrar HockeyApp anpassade data tooApplication insikter.

## <a name="hello-hockeyapp-bridge-app"></a>Hej HockeyApp Bridge app
Hej HockeyApp Bridge App är hello core funktion som gör att du tooaccess dina anpassade HockeyApp och spåra telemetri i Application Insights via hello Analytics och löpande Export funktioner. Anpassad och spåra händelser som samlas in av HockeyApp efter hello generering av hello HockeyApp Bridge App ska vara tillgänglig från dessa funktioner. Nu ska vi se hur tooset en av apparna Bridge.

Öppna kontoinställningar, i HockeyApp, [API token](https://rink.hockeyapp.net/manage/auth_tokens). Skapa en ny token eller återanvända en befintlig. hello lägsta behörighet som krävs är ”skrivskyddade”. Ta en kopia av hello API-token.

![Hämta ett HockeyApp API-token](./media/app-insights-hockeyapp-bridge-app/01.png)

Öppna hello Microsoft Azure-portalen och [skapa Application Insights-resurs](app-insights-create-new-resource.md). Ange program för ”HockeyApp bridge program”:

![Ny Application Insights-resurs](./media/app-insights-hockeyapp-bridge-app/02.png)

Du behöver inte tooset ett namn - detta in automatiskt från hello HockeyApp namn.

Hej HockeyApp bridge fält visas. 

![Ange bridge fält](./media/app-insights-hockeyapp-bridge-app/03.png)

Ange hello HockeyApp token som du antecknade tidigare. Den här åtgärden fyller hello ”HockeyApp-program” nedrullningsbar meny med alla HockeyApp-program. Välj hello en du vill toouse och fullständig hello resten av hello fält. 

Öppna hello ny resurs. 

Observera att det tar en stund hello data flödar toostart.

![Application Insights-resurs som väntar på data](./media/app-insights-hockeyapp-bridge-app/04.png)

Klart! Anpassade och spåra data som samlas in i appen HockeyApp instrumenterats från och med nu är nu också tillgängliga tooyou i hello Analytics och löpande Export funktioner i Application Insights.

Nu ska vi gå igenom var och en av dessa funktioner finns nu tillgänglig tooyou.

## <a name="analytics"></a>Analys
Analytics är ett kraftfullt verktyg för ad hoc-frågor till dina data, så att du toodiagnose och analysera dina telemetri och identifiera snabbt rotorsaker och mönster.

![Analys](./media/app-insights-hockeyapp-bridge-app/05.png)

* [Mer information om Analytics](app-insights-analytics-tour.md)

## <a name="continuous-export"></a>Löpande export
Löpande Export kan du tooexport dina data till en Azure Blob Storage-behållare. Detta är användbart om du behöver tookeep informationen under längre tid än hello kvarhållningsperiod för närvarande erbjuds av Application Insights. Du kan hålla hello data i blob storage kan bearbeta till en SQL-databas eller din önskade datalagerlösning.

[Mer information om löpande Export](app-insights-export-telemetry.md)

## <a name="next-steps"></a>Nästa steg
* [Tillämpa tooyour analysdata](app-insights-analytics-tour.md)

