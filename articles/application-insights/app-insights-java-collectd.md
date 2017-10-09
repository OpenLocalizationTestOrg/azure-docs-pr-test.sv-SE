---
title: "aaaMonitor Java web app prestanda på Linux - Azure | Microsoft Docs"
description: "Utökad övervakning av programprestanda för din Java-webbplats med hello CollectD plugin-programmet för Application Insights."
services: application-insights
documentationcenter: java
author: harelbr
manager: carmonm
ms.assetid: 40c68f45-197a-4624-bf89-541eb7323002
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 08/24/2016
ms.author: bwren
ms.openlocfilehash: f783e8607a83b2b43f67d3a2fc20f100aa2f75ec
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="collectd-linux-performance-metrics-in-application-insights"></a><span data-ttu-id="5dc64-103">collectd: Linux prestandamått i Application Insights</span><span class="sxs-lookup"><span data-stu-id="5dc64-103">collectd: Linux performance metrics in Application Insights</span></span>


<span data-ttu-id="5dc64-104">Linux tooexplore system prestandamått i [Programinsikter](app-insights-overview.md), installera [collectd](http://collectd.org/), tillsammans med dess plugin Application Insights.</span><span class="sxs-lookup"><span data-stu-id="5dc64-104">tooexplore Linux system performance metrics in [Application Insights](app-insights-overview.md), install [collectd](http://collectd.org/), together with its Application Insights plug-in.</span></span> <span data-ttu-id="5dc64-105">Den här lösningen med öppen källkod samlar in olika system- och statistik.</span><span class="sxs-lookup"><span data-stu-id="5dc64-105">This open-source solution gathers various system and network statistics.</span></span>

<span data-ttu-id="5dc64-106">Normalt ska du använda collectd om du redan har [instrumenterats Java webbtjänsten med Application Insights][java].</span><span class="sxs-lookup"><span data-stu-id="5dc64-106">Typically you'll use collectd if you have already [instrumented your Java web service with Application Insights][java].</span></span> <span data-ttu-id="5dc64-107">Den ger dig mer data toohelp du tooenhance appens prestanda eller diagnostisera problem.</span><span class="sxs-lookup"><span data-stu-id="5dc64-107">It gives you more data toohelp you tooenhance your app's performance or diagnose problems.</span></span> 

![exempel diagram](./media/app-insights-java-collectd/sample.png)

## <a name="get-your-instrumentation-key"></a><span data-ttu-id="5dc64-109">Hämta nyckel för instrumentation</span><span class="sxs-lookup"><span data-stu-id="5dc64-109">Get your instrumentation key</span></span>
<span data-ttu-id="5dc64-110">I hello [Microsoft Azure-portalen](https://portal.azure.com)öppnar hello [Programinsikter](app-insights-overview.md) resurs där du vill att hello data tooappear.</span><span class="sxs-lookup"><span data-stu-id="5dc64-110">In hello [Microsoft Azure portal](https://portal.azure.com), open hello [Application Insights](app-insights-overview.md) resource where you want hello data tooappear.</span></span> <span data-ttu-id="5dc64-111">(Eller [skapar en ny resurs](app-insights-create-new-resource.md).)</span><span class="sxs-lookup"><span data-stu-id="5dc64-111">(Or [create a new resource](app-insights-create-new-resource.md).)</span></span>

<span data-ttu-id="5dc64-112">Ta en kopia av hello instrumentation nyckel som identifierar hello resurs.</span><span class="sxs-lookup"><span data-stu-id="5dc64-112">Take a copy of hello instrumentation key, which identifies hello resource.</span></span>

![Bläddra igenom alla öppna resursen och sedan i hello Essentials listrutan, markera och kopiera hello Instrumentation nyckel](./media/app-insights-java-collectd/02-props.png)

## <a name="install-collectd-and-hello-plug-in"></a><span data-ttu-id="5dc64-114">Installera collectd och hello plugin-program</span><span class="sxs-lookup"><span data-stu-id="5dc64-114">Install collectd and hello plug-in</span></span>
<span data-ttu-id="5dc64-115">På ditt Linux-servrar:</span><span class="sxs-lookup"><span data-stu-id="5dc64-115">On your Linux server machines:</span></span>

1. <span data-ttu-id="5dc64-116">Installera [collectd](http://collectd.org/) version 5.4.0 eller senare.</span><span class="sxs-lookup"><span data-stu-id="5dc64-116">Install [collectd](http://collectd.org/) version 5.4.0 or later.</span></span>
2. <span data-ttu-id="5dc64-117">Hämta hello [Programinsikter collectd writer plugin](https://aka.ms/aijavasdk).</span><span class="sxs-lookup"><span data-stu-id="5dc64-117">Download hello [Application Insights collectd writer plugin](https://aka.ms/aijavasdk).</span></span> <span data-ttu-id="5dc64-118">Observera hello versionsnumret.</span><span class="sxs-lookup"><span data-stu-id="5dc64-118">Note hello version number.</span></span>
3. <span data-ttu-id="5dc64-119">Kopiera hello plugin JAR i `/usr/share/collectd/java`.</span><span class="sxs-lookup"><span data-stu-id="5dc64-119">Copy hello plugin JAR into `/usr/share/collectd/java`.</span></span>
4. <span data-ttu-id="5dc64-120">Redigera `/etc/collectd/collectd.conf`:</span><span class="sxs-lookup"><span data-stu-id="5dc64-120">Edit `/etc/collectd/collectd.conf`:</span></span>
   * <span data-ttu-id="5dc64-121">Se till att [hello Java-plugin-programmet](https://collectd.org/wiki/index.php/Plugin:Java) är aktiverad.</span><span class="sxs-lookup"><span data-stu-id="5dc64-121">Ensure that [hello Java plugin](https://collectd.org/wiki/index.php/Plugin:Java) is enabled.</span></span>
   * <span data-ttu-id="5dc64-122">Uppdatera hello JVMArg för hello java.class.path tooinclude hello följande JAR.</span><span class="sxs-lookup"><span data-stu-id="5dc64-122">Update hello JVMArg for hello java.class.path tooinclude hello following JAR.</span></span> <span data-ttu-id="5dc64-123">Uppdatera hello version nummer toomatch hello som du hämtade:</span><span class="sxs-lookup"><span data-stu-id="5dc64-123">Update hello version number toomatch hello one you downloaded:</span></span>
   * `/usr/share/collectd/java/applicationinsights-collectd-1.0.5.jar`
   * <span data-ttu-id="5dc64-124">Lägg till den här fragment med hello Instrumentation nyckeln från din resurs:</span><span class="sxs-lookup"><span data-stu-id="5dc64-124">Add this snippet, using hello Instrumentation Key from your resource:</span></span>

```XML

     LoadPlugin "com.microsoft.applicationinsights.collectd.ApplicationInsightsWriter"
     <Plugin ApplicationInsightsWriter>
        InstrumentationKey "Your key"
     </Plugin>
```

<span data-ttu-id="5dc64-125">Här är en del av en exempelkonfigurationsfilen:</span><span class="sxs-lookup"><span data-stu-id="5dc64-125">Here's part of a sample configuration file:</span></span>

```XML

    ...
    # collectd plugins
    LoadPlugin cpu
    LoadPlugin disk
    LoadPlugin load
    ...

    # Enable Java Plugin
    LoadPlugin "java"

    # Configure Java Plugin
    <Plugin "java">
      JVMArg "-verbose:jni"
      JVMArg "-Djava.class.path=/usr/share/collectd/java/applicationinsights-collectd-1.0.5.jar:/usr/share/collectd/java/collectd-api.jar"

      # Enabling Application Insights plugin
      LoadPlugin "com.microsoft.applicationinsights.collectd.ApplicationInsightsWriter"

      # Configuring Application Insights plugin
      <Plugin ApplicationInsightsWriter>
        InstrumentationKey "12345678-1234-1234-1234-123456781234"
      </Plugin>

      # Other plugin configurations ...
      ...
    </Plugin>
    ...
```

<span data-ttu-id="5dc64-126">Konfigurera andra [collectd plugin-program](https://collectd.org/wiki/index.php/Table_of_Plugins), som kan samla in olika data från olika källor.</span><span class="sxs-lookup"><span data-stu-id="5dc64-126">Configure other [collectd plugins](https://collectd.org/wiki/index.php/Table_of_Plugins), which can collect various data from different sources.</span></span>

<span data-ttu-id="5dc64-127">Starta om collectd enligt tooits [manuell](https://collectd.org/wiki/index.php/First_steps).</span><span class="sxs-lookup"><span data-stu-id="5dc64-127">Restart collectd according tooits [manual](https://collectd.org/wiki/index.php/First_steps).</span></span>

## <a name="view-hello-data-in-application-insights"></a><span data-ttu-id="5dc64-128">Visa hello data i Application Insights</span><span class="sxs-lookup"><span data-stu-id="5dc64-128">View hello data in Application Insights</span></span>
<span data-ttu-id="5dc64-129">Öppna i Application Insights-resurs [Metrics Explorer och Lägg till diagram][metrics], välja hello mått du vill använda toosee från hello anpassad kategori.</span><span class="sxs-lookup"><span data-stu-id="5dc64-129">In your Application Insights resource, open [Metrics Explorer and add charts][metrics], selecting hello metrics you want toosee from hello Custom category.</span></span>

![](./media/app-insights-java-collectd/result.png)

<span data-ttu-id="5dc64-130">Som standard samman hello mått över alla värddatorerna som hello mått har samlats in.</span><span class="sxs-lookup"><span data-stu-id="5dc64-130">By default, hello metrics are aggregated across all host machines from which hello metrics were collected.</span></span> <span data-ttu-id="5dc64-131">tooview hello mått per värd i hello diagram informationsbladet, aktivera gruppering och välj sedan toogroup CollectD-värden.</span><span class="sxs-lookup"><span data-stu-id="5dc64-131">tooview hello metrics per host, in hello Chart details blade, turn on Grouping and then choose toogroup by CollectD-Host.</span></span>

## <a name="tooexclude-upload-of-specific-statistics"></a><span data-ttu-id="5dc64-132">tooexclude överföringen av specifika statistik</span><span class="sxs-lookup"><span data-stu-id="5dc64-132">tooexclude upload of specific statistics</span></span>
<span data-ttu-id="5dc64-133">Som standard skickar Application Insights-plugin-programmet hello alla hello data som samlas in av alla hello aktiverat collectd ”läsa” plugin-program.</span><span class="sxs-lookup"><span data-stu-id="5dc64-133">By default, hello Application Insights plugin sends all hello data collected by all hello enabled collectd 'read' plugins.</span></span> 

<span data-ttu-id="5dc64-134">tooexclude data från specifika plugin-program eller datakällor:</span><span class="sxs-lookup"><span data-stu-id="5dc64-134">tooexclude data from specific plugins or data sources:</span></span>

* <span data-ttu-id="5dc64-135">Redigera hello konfigurationsfilen.</span><span class="sxs-lookup"><span data-stu-id="5dc64-135">Edit hello configuration file.</span></span> 
* <span data-ttu-id="5dc64-136">I `<Plugin ApplicationInsightsWriter>`, Lägg till direktivet rader så här:</span><span class="sxs-lookup"><span data-stu-id="5dc64-136">In `<Plugin ApplicationInsightsWriter>`, add directive lines like this:</span></span>

| <span data-ttu-id="5dc64-137">Direktivet</span><span class="sxs-lookup"><span data-stu-id="5dc64-137">Directive</span></span> | <span data-ttu-id="5dc64-138">Verkan</span><span class="sxs-lookup"><span data-stu-id="5dc64-138">Effect</span></span> |
| --- | --- |
| `Exclude disk` |<span data-ttu-id="5dc64-139">Undanta alla data som samlas in av hello `disk` plugin-program</span><span class="sxs-lookup"><span data-stu-id="5dc64-139">Exclude all data collected by hello `disk` plugin</span></span> |
| `Exclude disk:read,write` |<span data-ttu-id="5dc64-140">Exkludera hello källor med namnet `read` och `write` från hello `disk` plugin-programmet.</span><span class="sxs-lookup"><span data-stu-id="5dc64-140">Exclude hello sources named `read` and `write` from hello `disk` plugin.</span></span> |

<span data-ttu-id="5dc64-141">Separata direktiv med en ny rad.</span><span class="sxs-lookup"><span data-stu-id="5dc64-141">Separate directives with a newline.</span></span>

## <a name="problems"></a><span data-ttu-id="5dc64-142">Har du problem?</span><span class="sxs-lookup"><span data-stu-id="5dc64-142">Problems?</span></span>
<span data-ttu-id="5dc64-143">*Data i hello portal visas inte*</span><span class="sxs-lookup"><span data-stu-id="5dc64-143">*I don't see data in hello portal*</span></span>

* <span data-ttu-id="5dc64-144">Öppna [Sök] [ diagnostic] toosee om hello rådata händelser har tagits emot.</span><span class="sxs-lookup"><span data-stu-id="5dc64-144">Open [Search][diagnostic] toosee if hello raw events have arrived.</span></span> <span data-ttu-id="5dc64-145">Ibland kan de ta längre tooappear i metrics explorer.</span><span class="sxs-lookup"><span data-stu-id="5dc64-145">Sometimes they take longer tooappear in metrics explorer.</span></span>
* <span data-ttu-id="5dc64-146">Du kan behöva för[ange brandväggsundantag för utgående data](app-insights-ip-addresses.md)</span><span class="sxs-lookup"><span data-stu-id="5dc64-146">You might need too[set firewall exceptions for outgoing data](app-insights-ip-addresses.md)</span></span>
* <span data-ttu-id="5dc64-147">Aktivera spårning i hello Application Insights plugin-programmet.</span><span class="sxs-lookup"><span data-stu-id="5dc64-147">Enable tracing in hello Application Insights plugin.</span></span> <span data-ttu-id="5dc64-148">Lägg till följande rad i `<Plugin ApplicationInsightsWriter>`:</span><span class="sxs-lookup"><span data-stu-id="5dc64-148">Add this line within `<Plugin ApplicationInsightsWriter>`:</span></span>
  * `SDKLogger true`
* <span data-ttu-id="5dc64-149">Öppna en terminal och starta collectd i utförligt läge, toosee eventuella problem som den rapporterar:</span><span class="sxs-lookup"><span data-stu-id="5dc64-149">Open a terminal and start collectd in verbose mode, toosee any issues it is reporting:</span></span>
  * `sudo collectd -f`

## <a name="known-issue"></a><span data-ttu-id="5dc64-150">Kända problem</span><span class="sxs-lookup"><span data-stu-id="5dc64-150">Known issue</span></span>

<span data-ttu-id="5dc64-151">hello Application Insights skriva plugin-programmet är inte kompatibel med vissa Läs plugin-program.</span><span class="sxs-lookup"><span data-stu-id="5dc64-151">hello Application Insights Write plugin is incompatible with certain Read plugins.</span></span> <span data-ttu-id="5dc64-152">Vissa plugin-program att ibland skicka ”NaN” där Application Insights-plugin-programmet hello förväntar sig ett flyttal.</span><span class="sxs-lookup"><span data-stu-id="5dc64-152">Some plugins sometimes send "NaN" where hello Application Insights plugin expects a floating-point number.</span></span>

<span data-ttu-id="5dc64-153">Symptom: hello collectd loggen visar fel som innehåller ”AI:... SyntaxError: Oväntad token N ”.</span><span class="sxs-lookup"><span data-stu-id="5dc64-153">Symptom: hello collectd log shows errors that include "AI: ... SyntaxError: Unexpected token N".</span></span>

<span data-ttu-id="5dc64-154">Lösning: Utesluta data som samlas in av hello problemet skrivåtgärder plugin-program.</span><span class="sxs-lookup"><span data-stu-id="5dc64-154">Workaround: Exclude data collected by hello problem Write plugins.</span></span> 

<!--Link references-->

[api]: app-insights-api-custom-events-metrics.md
[apiexceptions]: app-insights-api-custom-events-metrics.md#track-exception
[availability]: app-insights-monitor-web-app-availability.md
[diagnostic]: app-insights-diagnostic-search.md
[eclipse]: app-insights-java-eclipse.md
[java]: app-insights-java-get-started.md
[javalogs]: app-insights-java-trace-logs.md
[metrics]: app-insights-metrics-explorer.md


