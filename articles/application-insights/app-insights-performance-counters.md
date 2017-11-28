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
# <a name="system-performance-counters-in-application-insights"></a><span data-ttu-id="dd122-103">Prestandaräknare för system i Application Insights</span><span class="sxs-lookup"><span data-stu-id="dd122-103">System performance counters in Application Insights</span></span>
<span data-ttu-id="dd122-104">Windows tillhandahåller flera olika [prestandaräknare](http://www.codeproject.com/Articles/8590/An-Introduction-To-Performance-Counters) , till exempel CPU användandet, minne, disk och användningen av nätverket.</span><span class="sxs-lookup"><span data-stu-id="dd122-104">Windows provides a wide variety of [performance counters](http://www.codeproject.com/Articles/8590/An-Introduction-To-Performance-Counters) such as CPU occupancy, memory, disk, and network usage.</span></span> <span data-ttu-id="dd122-105">Du kan också definiera egna.</span><span class="sxs-lookup"><span data-stu-id="dd122-105">You can also define your own.</span></span> <span data-ttu-id="dd122-106">[Application Insights](app-insights-overview.md) kan du visa de här prestandaräknarna om tillämpningsprogrammet körs under IIS på en lokal värd eller virtuell dator toowhich du har administratörsbehörighet.</span><span class="sxs-lookup"><span data-stu-id="dd122-106">[Application Insights](app-insights-overview.md) can show these performance counters if your application is running under IIS on an on-premises host or virtual machine toowhich you have administrative access.</span></span> <span data-ttu-id="dd122-107">hello diagram visar hello resurser tillgängliga tooyour live programmet och kan hjälpa tooidentify Obalanserat belastningen mellan server-instanser.</span><span class="sxs-lookup"><span data-stu-id="dd122-107">hello charts indicate hello resources available tooyour live application, and can help tooidentify unbalanced load between server instances.</span></span>

<span data-ttu-id="dd122-108">Prestandaräknare visas i hello servrar blad som innehåller en tabell som segment av server-instans.</span><span class="sxs-lookup"><span data-stu-id="dd122-108">Performance counters appear in hello Servers blade, which includes a table that segments by server instance.</span></span>

![Prestandaräknare som rapporteras i Application Insights](./media/app-insights-performance-counters/counters-by-server-instance.png)

<span data-ttu-id="dd122-110">(Prestandaräknare är inte tillgängliga för Azure Web Apps.</span><span class="sxs-lookup"><span data-stu-id="dd122-110">(Performance counters aren't available for Azure Web Apps.</span></span> <span data-ttu-id="dd122-111">Men du kan [skicka Azure Diagnostics tooApplication insikter](app-insights-azure-diagnostics.md).)</span><span class="sxs-lookup"><span data-stu-id="dd122-111">But you can [send Azure Diagnostics tooApplication Insights](app-insights-azure-diagnostics.md).)</span></span>

## <a name="view-counters"></a><span data-ttu-id="dd122-112">Visa räknare</span><span class="sxs-lookup"><span data-stu-id="dd122-112">View counters</span></span>
<span data-ttu-id="dd122-113">hello servrar bladet visar en standarduppsättning prestandaräknare.</span><span class="sxs-lookup"><span data-stu-id="dd122-113">hello Servers blade shows a default set of performance counters.</span></span> 

<span data-ttu-id="dd122-114">toosee andra räknare redigera hello diagram på bladet för hello-servrar, eller öppna ett nytt [Metrics Explorer](app-insights-metrics-explorer.md) bladet och lägga till nya diagram.</span><span class="sxs-lookup"><span data-stu-id="dd122-114">toosee other counters, either edit hello charts on hello Servers blade, or open a new [Metrics Explorer](app-insights-metrics-explorer.md) blade and add new charts.</span></span> 

<span data-ttu-id="dd122-115">hello tillgängliga räknare listas som mått när du redigerar ett diagram.</span><span class="sxs-lookup"><span data-stu-id="dd122-115">hello available counters are listed as metrics when you edit a chart.</span></span>

![Prestandaräknare som rapporteras i Application Insights](./media/app-insights-performance-counters/choose-performance-counters.png)

<span data-ttu-id="dd122-117">toosee alla mest användbara diagram på en plats, skapa en [instrumentpanelen](app-insights-dashboards.md) och fästa dem tooit.</span><span class="sxs-lookup"><span data-stu-id="dd122-117">toosee all your most useful charts in one place, create a [dashboard](app-insights-dashboards.md) and pin them tooit.</span></span>

## <a name="add-counters"></a><span data-ttu-id="dd122-118">Lägga till räknare</span><span class="sxs-lookup"><span data-stu-id="dd122-118">Add counters</span></span>
<span data-ttu-id="dd122-119">Om hello prestandaräknare som du vill använda inte visas i hello listan över mått som är eftersom hello Application Insights SDK inte samla in den på din webbserver.</span><span class="sxs-lookup"><span data-stu-id="dd122-119">If hello performance counter you want isn't shown in hello list of metrics, that's because hello Application Insights SDK isn't collecting it in your web server.</span></span> <span data-ttu-id="dd122-120">Du kan konfigurera den toodo så.</span><span class="sxs-lookup"><span data-stu-id="dd122-120">You can configure it toodo so.</span></span>

1. <span data-ttu-id="dd122-121">Ta reda på vilka räknare som är tillgängliga i servern med hjälp av PowerShell-kommando på hello-servern:</span><span class="sxs-lookup"><span data-stu-id="dd122-121">Find out what counters are available in your server by using this PowerShell command at hello server:</span></span>
   
    `Get-Counter -ListSet *`
   
    <span data-ttu-id="dd122-122">(Se [ `Get-Counter` ](https://technet.microsoft.com/library/hh849685.aspx).)</span><span class="sxs-lookup"><span data-stu-id="dd122-122">(See [`Get-Counter`](https://technet.microsoft.com/library/hh849685.aspx).)</span></span>
2. <span data-ttu-id="dd122-123">Öppna ApplicationInsights.config.</span><span class="sxs-lookup"><span data-stu-id="dd122-123">Open ApplicationInsights.config.</span></span>
   
   * <span data-ttu-id="dd122-124">Om du har lagt till Application Insights tooyour appen under utvecklingen redigera ApplicationInsights.config i projektet och sedan omdistribuera den tooyour servrar.</span><span class="sxs-lookup"><span data-stu-id="dd122-124">If you added Application Insights tooyour app during development, edit ApplicationInsights.config in your project, and then re-deploy it tooyour servers.</span></span>
   * <span data-ttu-id="dd122-125">Om du använde statusövervakaren tooinstrument en webbapp vid körning kan hitta ApplicationInsights.config i hello rotkatalog hello app i IIS.</span><span class="sxs-lookup"><span data-stu-id="dd122-125">If you used Status Monitor tooinstrument a web app at runtime, find ApplicationInsights.config in hello root directory of hello app in IIS.</span></span> <span data-ttu-id="dd122-126">Uppdatera den där på varje server-instans.</span><span class="sxs-lookup"><span data-stu-id="dd122-126">Update it there in each server instance.</span></span>
3. <span data-ttu-id="dd122-127">Redigera hello prestanda insamlaren direktiv:</span><span class="sxs-lookup"><span data-stu-id="dd122-127">Edit hello performance collector directive:</span></span>
   
```XML
   
    <Add Type="Microsoft.ApplicationInsights.Extensibility.PerfCounterCollector.PerformanceCollectorModule, Microsoft.AI.PerfCounterCollector">
      <Counters>
        <Add PerformanceCounter="\Objects\Processes"/>
        <Add PerformanceCounter="\Sales(photo)\# Items Sold" ReportAs="Photo sales"/>
      </Counters>
    </Add>

```

<span data-ttu-id="dd122-128">Du kan avbilda standard räknare och du har implementerat själv.</span><span class="sxs-lookup"><span data-stu-id="dd122-128">You can capture both standard counters and those you have implemented yourself.</span></span> <span data-ttu-id="dd122-129">`\Objects\Processes`är ett exempel på en standard räknare tillgänglig på alla Windows-datorer.</span><span class="sxs-lookup"><span data-stu-id="dd122-129">`\Objects\Processes` is an example of a standard counter, available on all Windows systems.</span></span> <span data-ttu-id="dd122-130">`\Sales(photo)\# Items Sold`är ett exempel på en anpassad räknare som kan implementeras i en webbtjänst.</span><span class="sxs-lookup"><span data-stu-id="dd122-130">`\Sales(photo)\# Items Sold` is an example of a custom counter that might be implemented in a web service.</span></span> 

<span data-ttu-id="dd122-131">hello-formatet är `\Category(instance)\Counter"`, eller kategorier som inte har instanser, bara `\Category\Counter`.</span><span class="sxs-lookup"><span data-stu-id="dd122-131">hello format is `\Category(instance)\Counter"`, or for categories that don't have instances, just `\Category\Counter`.</span></span>

<span data-ttu-id="dd122-132">`ReportAs`krävs för räknarnamn som inte matchar `[a-zA-Z()/-_ \.]+` – det vill säga de innehåller tecken som inte ingår i hello följande uppsättningar: bokstäver avrunda hakparenteser, snedstreck, bindestreck, understreck, blanksteg, punkt.</span><span class="sxs-lookup"><span data-stu-id="dd122-132">`ReportAs` is required for counter names that do not match `[a-zA-Z()/-_ \.]+` - that is, they contain characters that are not in hello following sets: letters, round brackets, forward slash, hyphen, underscore, space, dot.</span></span>

<span data-ttu-id="dd122-133">Om du anger en instans, samlas den som en dimension ”CounterInstanceName” av hello rapporteras mått.</span><span class="sxs-lookup"><span data-stu-id="dd122-133">If you specify an instance, it will be collected as a dimension "CounterInstanceName" of hello reported metric.</span></span>

### <a name="collecting-performance-counters-in-code"></a><span data-ttu-id="dd122-134">Insamling av prestandaräknare i koden</span><span class="sxs-lookup"><span data-stu-id="dd122-134">Collecting performance counters in code</span></span>
<span data-ttu-id="dd122-135">toocollect systemprestanda prestandaräknare och skicka dem tooApplication insikter, kan du anpassa hello kodfragmentet nedan:</span><span class="sxs-lookup"><span data-stu-id="dd122-135">toocollect system performance counters and send them tooApplication Insights, you can adapt hello snippet below:</span></span>


``` C#

    var perfCollectorModule = new PerformanceCollectorModule();
    perfCollectorModule.Counters.Add(new PerformanceCounterCollectionRequest(
      @"\.NET CLR Memory([replace-with-application-process-name])\# GC Handles", "GC Handles")));
    perfCollectorModule.Initialize(TelemetryConfiguration.Active);
```

<span data-ttu-id="dd122-136">Eller så kan du göra samma sak med anpassade mått som du skapade hello:</span><span class="sxs-lookup"><span data-stu-id="dd122-136">Or you can do hello same thing with custom metrics you created:</span></span>

``` C#
    var perfCollectorModule = new PerformanceCollectorModule();
    perfCollectorModule.Counters.Add(new PerformanceCounterCollectionRequest(
      @"\Sales(photo)\# Items Sold", "Photo sales"));
    perfCollectorModule.Initialize(TelemetryConfiguration.Active);
```

## <a name="performance-counters-in-analytics"></a><span data-ttu-id="dd122-137">Prestandaräknare i Analytics</span><span class="sxs-lookup"><span data-stu-id="dd122-137">Performance counters in Analytics</span></span>
<span data-ttu-id="dd122-138">Du kan söka efter och visa rapporter för räknaren av prestanda i [Analytics](app-insights-analytics.md).</span><span class="sxs-lookup"><span data-stu-id="dd122-138">You can search and display performance counter reports in [Analytics](app-insights-analytics.md).</span></span>

<span data-ttu-id="dd122-139">Hej **performanceCounters** schemat visar hello `category`, `counter` namn, och `instance` namn för varje prestandaräknare.</span><span class="sxs-lookup"><span data-stu-id="dd122-139">hello **performanceCounters** schema exposes hello `category`, `counter` name, and `instance` name of each performance counter.</span></span>  <span data-ttu-id="dd122-140">I hello telemetri för varje program ser du bara hello räknare för programmet.</span><span class="sxs-lookup"><span data-stu-id="dd122-140">In hello telemetry for each application, you’ll see only hello counters for that application.</span></span> <span data-ttu-id="dd122-141">Till exempel toosee vilka räknare är tillgängliga:</span><span class="sxs-lookup"><span data-stu-id="dd122-141">For example, toosee what counters are available:</span></span> 

![Prestandaräknare i Application Insights analytics](./media/app-insights-performance-counters/analytics-performance-counters.png)

<span data-ttu-id="dd122-143">(Här 'Instansen' refererar toohello prestandaräknarinstans, hello inte roll- eller serverinstansen för datorn.</span><span class="sxs-lookup"><span data-stu-id="dd122-143">('Instance' here refers toohello performance counter instance,  not hello role or server machine instance.</span></span> <span data-ttu-id="dd122-144">hello namn på prestandaräknare instans vanligtvis segment räknare, till exempel processortid av hello namn hello process eller ett program.)</span><span class="sxs-lookup"><span data-stu-id="dd122-144">hello performance counter instance name typically segments counters such as processor time by hello name of hello process or application.)</span></span>

<span data-ttu-id="dd122-145">tooget ett diagram av minne över hello senaste perioden:</span><span class="sxs-lookup"><span data-stu-id="dd122-145">tooget a chart of available memory over hello recent period:</span></span> 

![Minne timechart i Application Insights analytics](./media/app-insights-performance-counters/analytics-available-memory.png)

<span data-ttu-id="dd122-147">Som andra telemetri **performanceCounters** också har en kolumn `cloud_RoleInstance` som visar hello identitet hello värdserverinstansens som appen körs.</span><span class="sxs-lookup"><span data-stu-id="dd122-147">Like other telemetry, **performanceCounters** also has a column `cloud_RoleInstance` that indicates hello identity of hello host server instance on which your app is running.</span></span> <span data-ttu-id="dd122-148">Till exempel toocompare hello prestandan för din app på hello olika datorer:</span><span class="sxs-lookup"><span data-stu-id="dd122-148">For example, toocompare hello performance of your app on hello different machines:</span></span> 

![Prestanda åtskilda med rollinstans i Application Insights analytics](./media/app-insights-performance-counters/analytics-metrics-role-instance.png)

## <a name="aspnet-and-application-insights-counts"></a><span data-ttu-id="dd122-150">ASP.NET och Application Insights-antal</span><span class="sxs-lookup"><span data-stu-id="dd122-150">ASP.NET and Application Insights counts</span></span>
<span data-ttu-id="dd122-151">*Vad är hello skillnaden mellan hello undantag hastighet och undantag mått?*</span><span class="sxs-lookup"><span data-stu-id="dd122-151">*What's hello difference between hello Exception rate and Exceptions metrics?*</span></span>

* <span data-ttu-id="dd122-152">*Undantag hastighet* är en system-prestandaräknare.</span><span class="sxs-lookup"><span data-stu-id="dd122-152">*Exception rate* is a system performance counter.</span></span> <span data-ttu-id="dd122-153">hello CLR räknar alla hello hanterade och ohanterade undantag som utlöses och delar hello totala i ett exempelintervall av hello längden på hello intervall.</span><span class="sxs-lookup"><span data-stu-id="dd122-153">hello CLR counts all hello handled and unhandled exceptions that are thrown, and divides hello total in a sampling interval by hello length of hello interval.</span></span> <span data-ttu-id="dd122-154">hello Application Insights SDK samlar in det här resultatet och skickar den toohello portal.</span><span class="sxs-lookup"><span data-stu-id="dd122-154">hello Application Insights SDK collects this result and sends it toohello portal.</span></span>
* <span data-ttu-id="dd122-155">*Undantag* är antalet hello TrackException rapporter som tagits emot av hello-portal i hello insamlingsintervall av hello diagram.</span><span class="sxs-lookup"><span data-stu-id="dd122-155">*Exceptions* is a count of hello TrackException reports received by hello portal in hello sampling interval of hello chart.</span></span> <span data-ttu-id="dd122-156">Det innehåller endast hello hanteras undantag där du har skrivit TrackException anropar i koden, och inte innehåller alla [ohanterade undantag](app-insights-asp-net-exceptions.md).</span><span class="sxs-lookup"><span data-stu-id="dd122-156">It includes only hello handled exceptions where you have written TrackException calls in your code, and doesn't include all [unhandled exceptions](app-insights-asp-net-exceptions.md).</span></span> 

## <a name="alerts"></a><span data-ttu-id="dd122-157">Aviseringar</span><span class="sxs-lookup"><span data-stu-id="dd122-157">Alerts</span></span>
<span data-ttu-id="dd122-158">Precis som andra mått kan du [anger att en varning](app-insights-alerts.md) toowarn du om en prestandaräknare går utanför en gräns som du anger.</span><span class="sxs-lookup"><span data-stu-id="dd122-158">Like other metrics, you can [set an alert](app-insights-alerts.md) toowarn you if a performance counter goes outside a limit you specify.</span></span> <span data-ttu-id="dd122-159">Öppna bladet med säkerhetsaviseringar hello och klicka på Lägg till avisering.</span><span class="sxs-lookup"><span data-stu-id="dd122-159">Open hello Alerts blade and click Add Alert.</span></span>

## <span data-ttu-id="dd122-160"><a name="next"></a>Nästa steg</span><span class="sxs-lookup"><span data-stu-id="dd122-160"><a name="next"></a>Next steps</span></span>
* [<span data-ttu-id="dd122-161">Beroende spårning</span><span class="sxs-lookup"><span data-stu-id="dd122-161">Dependency tracking</span></span>](app-insights-asp-net-dependencies.md)
* [<span data-ttu-id="dd122-162">Undantagsspårning</span><span class="sxs-lookup"><span data-stu-id="dd122-162">Exception tracking</span></span>](app-insights-asp-net-exceptions.md)

