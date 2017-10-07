---
title: "aaaPerformance räknare i Application Insights | Microsoft Docs"
description: "Övervaka system och anpassade .NET-prestandaräknare i Application Insights."
services: application-insights
documentationcenter: 
author: CFreemanwa
manager: carmonm
ms.assetid: 5b816f4c-a77a-4674-ae36-802ee3a2f56d
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 10/11/2016
ms.author: bwren
ms.openlocfilehash: 0a51c225f1d1124c9e7fe89f34e747cb26a3589e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="system-performance-counters-in-application-insights"></a>Prestandaräknare för system i Application Insights
Windows tillhandahåller flera olika [prestandaräknare](http://www.codeproject.com/Articles/8590/An-Introduction-To-Performance-Counters) , till exempel CPU användandet, minne, disk och användningen av nätverket. Du kan också definiera egna. [Application Insights](app-insights-overview.md) kan du visa de här prestandaräknarna om tillämpningsprogrammet körs under IIS på en lokal värd eller virtuell dator toowhich du har administratörsbehörighet. hello diagram visar hello resurser tillgängliga tooyour live programmet och kan hjälpa tooidentify Obalanserat belastningen mellan server-instanser.

Prestandaräknare visas i hello servrar blad som innehåller en tabell som segment av server-instans.

![Prestandaräknare som rapporteras i Application Insights](./media/app-insights-performance-counters/counters-by-server-instance.png)

(Prestandaräknare är inte tillgängliga för Azure Web Apps. Men du kan [skicka Azure Diagnostics tooApplication insikter](app-insights-azure-diagnostics.md).)

## <a name="view-counters"></a>Visa räknare
hello servrar bladet visar en standarduppsättning prestandaräknare. 

toosee andra räknare redigera hello diagram på bladet för hello-servrar, eller öppna ett nytt [Metrics Explorer](app-insights-metrics-explorer.md) bladet och lägga till nya diagram. 

hello tillgängliga räknare listas som mått när du redigerar ett diagram.

![Prestandaräknare som rapporteras i Application Insights](./media/app-insights-performance-counters/choose-performance-counters.png)

toosee alla mest användbara diagram på en plats, skapa en [instrumentpanelen](app-insights-dashboards.md) och fästa dem tooit.

## <a name="add-counters"></a>Lägga till räknare
Om hello prestandaräknare som du vill använda inte visas i hello listan över mått som är eftersom hello Application Insights SDK inte samla in den på din webbserver. Du kan konfigurera den toodo så.

1. Ta reda på vilka räknare som är tillgängliga i servern med hjälp av PowerShell-kommando på hello-servern:
   
    `Get-Counter -ListSet *`
   
    (Se [ `Get-Counter` ](https://technet.microsoft.com/library/hh849685.aspx).)
2. Öppna ApplicationInsights.config.
   
   * Om du har lagt till Application Insights tooyour appen under utvecklingen redigera ApplicationInsights.config i projektet och sedan omdistribuera den tooyour servrar.
   * Om du använde statusövervakaren tooinstrument en webbapp vid körning kan hitta ApplicationInsights.config i hello rotkatalog hello app i IIS. Uppdatera den där på varje server-instans.
3. Redigera hello prestanda insamlaren direktiv:
   
```XML
   
    <Add Type="Microsoft.ApplicationInsights.Extensibility.PerfCounterCollector.PerformanceCollectorModule, Microsoft.AI.PerfCounterCollector">
      <Counters>
        <Add PerformanceCounter="\Objects\Processes"/>
        <Add PerformanceCounter="\Sales(photo)\# Items Sold" ReportAs="Photo sales"/>
      </Counters>
    </Add>

```

Du kan avbilda standard räknare och du har implementerat själv. `\Objects\Processes`är ett exempel på en standard räknare tillgänglig på alla Windows-datorer. `\Sales(photo)\# Items Sold`är ett exempel på en anpassad räknare som kan implementeras i en webbtjänst. 

hello-formatet är `\Category(instance)\Counter"`, eller kategorier som inte har instanser, bara `\Category\Counter`.

`ReportAs`krävs för räknarnamn som inte matchar `[a-zA-Z()/-_ \.]+` – det vill säga de innehåller tecken som inte ingår i hello följande uppsättningar: bokstäver avrunda hakparenteser, snedstreck, bindestreck, understreck, blanksteg, punkt.

Om du anger en instans, samlas den som en dimension ”CounterInstanceName” av hello rapporteras mått.

### <a name="collecting-performance-counters-in-code"></a>Insamling av prestandaräknare i koden
toocollect systemprestanda prestandaräknare och skicka dem tooApplication insikter, kan du anpassa hello kodfragmentet nedan:


``` C#

    var perfCollectorModule = new PerformanceCollectorModule();
    perfCollectorModule.Counters.Add(new PerformanceCounterCollectionRequest(
      @"\.NET CLR Memory([replace-with-application-process-name])\# GC Handles", "GC Handles")));
    perfCollectorModule.Initialize(TelemetryConfiguration.Active);
```

Eller så kan du göra samma sak med anpassade mått som du skapade hello:

``` C#
    var perfCollectorModule = new PerformanceCollectorModule();
    perfCollectorModule.Counters.Add(new PerformanceCounterCollectionRequest(
      @"\Sales(photo)\# Items Sold", "Photo sales"));
    perfCollectorModule.Initialize(TelemetryConfiguration.Active);
```

## <a name="performance-counters-in-analytics"></a>Prestandaräknare i Analytics
Du kan söka efter och visa rapporter för räknaren av prestanda i [Analytics](app-insights-analytics.md).

Hej **performanceCounters** schemat visar hello `category`, `counter` namn, och `instance` namn för varje prestandaräknare.  I hello telemetri för varje program ser du bara hello räknare för programmet. Till exempel toosee vilka räknare är tillgängliga: 

![Prestandaräknare i Application Insights analytics](./media/app-insights-performance-counters/analytics-performance-counters.png)

(Här 'Instansen' refererar toohello prestandaräknarinstans, hello inte roll- eller serverinstansen för datorn. hello namn på prestandaräknare instans vanligtvis segment räknare, till exempel processortid av hello namn hello process eller ett program.)

tooget ett diagram av minne över hello senaste perioden: 

![Minne timechart i Application Insights analytics](./media/app-insights-performance-counters/analytics-available-memory.png)

Som andra telemetri **performanceCounters** också har en kolumn `cloud_RoleInstance` som visar hello identitet hello värdserverinstansens som appen körs. Till exempel toocompare hello prestandan för din app på hello olika datorer: 

![Prestanda åtskilda med rollinstans i Application Insights analytics](./media/app-insights-performance-counters/analytics-metrics-role-instance.png)

## <a name="aspnet-and-application-insights-counts"></a>ASP.NET och Application Insights-antal
*Vad är hello skillnaden mellan hello undantag hastighet och undantag mått?*

* *Undantag hastighet* är en system-prestandaräknare. hello CLR räknar alla hello hanterade och ohanterade undantag som utlöses och delar hello totala i ett exempelintervall av hello längden på hello intervall. hello Application Insights SDK samlar in det här resultatet och skickar den toohello portal.
* *Undantag* är antalet hello TrackException rapporter som tagits emot av hello-portal i hello insamlingsintervall av hello diagram. Det innehåller endast hello hanteras undantag där du har skrivit TrackException anropar i koden, och inte innehåller alla [ohanterade undantag](app-insights-asp-net-exceptions.md). 

## <a name="alerts"></a>Aviseringar
Precis som andra mått kan du [anger att en varning](app-insights-alerts.md) toowarn du om en prestandaräknare går utanför en gräns som du anger. Öppna bladet med säkerhetsaviseringar hello och klicka på Lägg till avisering.

## <a name="next"></a>Nästa steg
* [Beroende spårning](app-insights-asp-net-dependencies.md)
* [Undantagsspårning](app-insights-asp-net-exceptions.md)

