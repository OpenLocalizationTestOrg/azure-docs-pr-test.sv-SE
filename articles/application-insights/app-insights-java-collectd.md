---
title: "Övervaka Java web app prestanda på Linux - Azure | Microsoft Docs"
description: "Utökad övervakning av programprestanda för din Java-webbplats med CollectD plugin-programmet för Application Insights."
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
ms.openlocfilehash: 4ea917b068e0242bfb88d7357eca032607a43a3f
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/18/2017
---
# <a name="collectd-linux-performance-metrics-in-application-insights"></a><span data-ttu-id="ac4a7-103">collectd: Linux prestandamått i Application Insights</span><span class="sxs-lookup"><span data-stu-id="ac4a7-103">collectd: Linux performance metrics in Application Insights</span></span>


<span data-ttu-id="ac4a7-104">Utforska prestandastatistik för Linux-system i [Programinsikter](app-insights-overview.md), installera [collectd](http://collectd.org/), tillsammans med dess plugin Application Insights.</span><span class="sxs-lookup"><span data-stu-id="ac4a7-104">To explore Linux system performance metrics in [Application Insights](app-insights-overview.md), install [collectd](http://collectd.org/), together with its Application Insights plug-in.</span></span> <span data-ttu-id="ac4a7-105">Den här lösningen med öppen källkod samlar in olika system- och statistik.</span><span class="sxs-lookup"><span data-stu-id="ac4a7-105">This open-source solution gathers various system and network statistics.</span></span>

<span data-ttu-id="ac4a7-106">Normalt ska du använda collectd om du redan har [instrumenterats Java webbtjänsten med Application Insights][java].</span><span class="sxs-lookup"><span data-stu-id="ac4a7-106">Typically you'll use collectd if you have already [instrumented your Java web service with Application Insights][java].</span></span> <span data-ttu-id="ac4a7-107">Den ger dig fler data som kan hjälpa dig att förbättra prestanda för din app eller diagnostisera problem.</span><span class="sxs-lookup"><span data-stu-id="ac4a7-107">It gives you more data to help you to enhance your app's performance or diagnose problems.</span></span> 

![exempel diagram](./media/app-insights-java-collectd/sample.png)

## <a name="get-your-instrumentation-key"></a><span data-ttu-id="ac4a7-109">Hämta nyckel för instrumentation</span><span class="sxs-lookup"><span data-stu-id="ac4a7-109">Get your instrumentation key</span></span>
<span data-ttu-id="ac4a7-110">I den [Microsoft Azure-portalen](https://portal.azure.com)öppnar den [Programinsikter](app-insights-overview.md) resurs där du vill att data ska visas.</span><span class="sxs-lookup"><span data-stu-id="ac4a7-110">In the [Microsoft Azure portal](https://portal.azure.com), open the [Application Insights](app-insights-overview.md) resource where you want the data to appear.</span></span> <span data-ttu-id="ac4a7-111">(Eller [skapar en ny resurs](app-insights-create-new-resource.md).)</span><span class="sxs-lookup"><span data-stu-id="ac4a7-111">(Or [create a new resource](app-insights-create-new-resource.md).)</span></span>

<span data-ttu-id="ac4a7-112">Ta en kopia av nyckeln instrumentation som identifierar resursen.</span><span class="sxs-lookup"><span data-stu-id="ac4a7-112">Take a copy of the instrumentation key, which identifies the resource.</span></span>

![Bläddra igenom alla, öppna din resurs i Essentials nedrullningsbara Markera och kopiera nyckeln Instrumentation](./media/app-insights-java-collectd/02-props.png)

## <a name="install-collectd-and-the-plug-in"></a><span data-ttu-id="ac4a7-114">Installera collectd och plugin-programmet</span><span class="sxs-lookup"><span data-stu-id="ac4a7-114">Install collectd and the plug-in</span></span>
<span data-ttu-id="ac4a7-115">På ditt Linux-servrar:</span><span class="sxs-lookup"><span data-stu-id="ac4a7-115">On your Linux server machines:</span></span>

1. <span data-ttu-id="ac4a7-116">Installera [collectd](http://collectd.org/) version 5.4.0 eller senare.</span><span class="sxs-lookup"><span data-stu-id="ac4a7-116">Install [collectd](http://collectd.org/) version 5.4.0 or later.</span></span>
2. <span data-ttu-id="ac4a7-117">Hämta den [Programinsikter collectd writer plugin](https://aka.ms/aijavasdk).</span><span class="sxs-lookup"><span data-stu-id="ac4a7-117">Download the [Application Insights collectd writer plugin](https://aka.ms/aijavasdk).</span></span> <span data-ttu-id="ac4a7-118">Notera versionsnumret.</span><span class="sxs-lookup"><span data-stu-id="ac4a7-118">Note the version number.</span></span>
3. <span data-ttu-id="ac4a7-119">Kopiera JAR-plugin-programmet i `/usr/share/collectd/java`.</span><span class="sxs-lookup"><span data-stu-id="ac4a7-119">Copy the plugin JAR into `/usr/share/collectd/java`.</span></span>
4. <span data-ttu-id="ac4a7-120">Redigera `/etc/collectd/collectd.conf`:</span><span class="sxs-lookup"><span data-stu-id="ac4a7-120">Edit `/etc/collectd/collectd.conf`:</span></span>
   * <span data-ttu-id="ac4a7-121">Se till att [pluginprogrammet Java](https://collectd.org/wiki/index.php/Plugin:Java) är aktiverad.</span><span class="sxs-lookup"><span data-stu-id="ac4a7-121">Ensure that [the Java plugin](https://collectd.org/wiki/index.php/Plugin:Java) is enabled.</span></span>
   * <span data-ttu-id="ac4a7-122">Uppdatera JVMArg för java.class.path för att inkludera följande JAR.</span><span class="sxs-lookup"><span data-stu-id="ac4a7-122">Update the JVMArg for the java.class.path to include the following JAR.</span></span> <span data-ttu-id="ac4a7-123">Uppdatera versionsnumret för att matcha det du hämtade:</span><span class="sxs-lookup"><span data-stu-id="ac4a7-123">Update the version number to match the one you downloaded:</span></span>
   * `/usr/share/collectd/java/applicationinsights-collectd-1.0.5.jar`
   * <span data-ttu-id="ac4a7-124">Lägg till den här fragment med Instrumentation nyckeln från din resurs:</span><span class="sxs-lookup"><span data-stu-id="ac4a7-124">Add this snippet, using the Instrumentation Key from your resource:</span></span>

```XML

     LoadPlugin "com.microsoft.applicationinsights.collectd.ApplicationInsightsWriter"
     <Plugin ApplicationInsightsWriter>
        InstrumentationKey "Your key"
     </Plugin>
```

<span data-ttu-id="ac4a7-125">Här är en del av en exempelkonfigurationsfilen:</span><span class="sxs-lookup"><span data-stu-id="ac4a7-125">Here's part of a sample configuration file:</span></span>

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

<span data-ttu-id="ac4a7-126">Konfigurera andra [collectd plugin-program](https://collectd.org/wiki/index.php/Table_of_Plugins), som kan samla in olika data från olika källor.</span><span class="sxs-lookup"><span data-stu-id="ac4a7-126">Configure other [collectd plugins](https://collectd.org/wiki/index.php/Table_of_Plugins), which can collect various data from different sources.</span></span>

<span data-ttu-id="ac4a7-127">Starta om collectd enligt dess [manuell](https://collectd.org/wiki/index.php/First_steps).</span><span class="sxs-lookup"><span data-stu-id="ac4a7-127">Restart collectd according to its [manual](https://collectd.org/wiki/index.php/First_steps).</span></span>

## <a name="view-the-data-in-application-insights"></a><span data-ttu-id="ac4a7-128">Visa data i Application Insights</span><span class="sxs-lookup"><span data-stu-id="ac4a7-128">View the data in Application Insights</span></span>
<span data-ttu-id="ac4a7-129">Öppna i Application Insights-resurs [Metrics Explorer och Lägg till diagram][metrics], välja mått som du vill se från anpassad kategori.</span><span class="sxs-lookup"><span data-stu-id="ac4a7-129">In your Application Insights resource, open [Metrics Explorer and add charts][metrics], selecting the metrics you want to see from the Custom category.</span></span>

![](./media/app-insights-java-collectd/result.png)

<span data-ttu-id="ac4a7-130">Som standard samman mätvärdena över alla värddatorerna som mätvärdena som har samlats in.</span><span class="sxs-lookup"><span data-stu-id="ac4a7-130">By default, the metrics are aggregated across all host machines from which the metrics were collected.</span></span> <span data-ttu-id="ac4a7-131">Om du vill visa mått per värd i diagrammet informationsbladet aktivera gruppering och sedan välja att gruppera efter CollectD-värden.</span><span class="sxs-lookup"><span data-stu-id="ac4a7-131">To view the metrics per host, in the Chart details blade, turn on Grouping and then choose to group by CollectD-Host.</span></span>

## <a name="to-exclude-upload-of-specific-statistics"></a><span data-ttu-id="ac4a7-132">För att utesluta överföringen av specifika statistik</span><span class="sxs-lookup"><span data-stu-id="ac4a7-132">To exclude upload of specific statistics</span></span>
<span data-ttu-id="ac4a7-133">Som standard skickar Application Insights plugin-programmet alla data som samlas in av alla aktiverade collectd läsa plugin-program.</span><span class="sxs-lookup"><span data-stu-id="ac4a7-133">By default, the Application Insights plugin sends all the data collected by all the enabled collectd 'read' plugins.</span></span> 

<span data-ttu-id="ac4a7-134">För att utesluta data från specifika plugin-program eller datakällor:</span><span class="sxs-lookup"><span data-stu-id="ac4a7-134">To exclude data from specific plugins or data sources:</span></span>

* <span data-ttu-id="ac4a7-135">Redigera konfigurationsfilen.</span><span class="sxs-lookup"><span data-stu-id="ac4a7-135">Edit the configuration file.</span></span> 
* <span data-ttu-id="ac4a7-136">I `<Plugin ApplicationInsightsWriter>`, Lägg till direktivet rader så här:</span><span class="sxs-lookup"><span data-stu-id="ac4a7-136">In `<Plugin ApplicationInsightsWriter>`, add directive lines like this:</span></span>

| <span data-ttu-id="ac4a7-137">Direktivet</span><span class="sxs-lookup"><span data-stu-id="ac4a7-137">Directive</span></span> | <span data-ttu-id="ac4a7-138">Verkan</span><span class="sxs-lookup"><span data-stu-id="ac4a7-138">Effect</span></span> |
| --- | --- |
| `Exclude disk` |<span data-ttu-id="ac4a7-139">Undanta alla data som samlas in av den `disk` plugin-program</span><span class="sxs-lookup"><span data-stu-id="ac4a7-139">Exclude all data collected by the `disk` plugin</span></span> |
| `Exclude disk:read,write` |<span data-ttu-id="ac4a7-140">Exkludera datakällor med namnet `read` och `write` från den `disk` plugin-programmet.</span><span class="sxs-lookup"><span data-stu-id="ac4a7-140">Exclude the sources named `read` and `write` from the `disk` plugin.</span></span> |

<span data-ttu-id="ac4a7-141">Separata direktiv med en ny rad.</span><span class="sxs-lookup"><span data-stu-id="ac4a7-141">Separate directives with a newline.</span></span>

## <a name="problems"></a><span data-ttu-id="ac4a7-142">Har du problem?</span><span class="sxs-lookup"><span data-stu-id="ac4a7-142">Problems?</span></span>
<span data-ttu-id="ac4a7-143">*Data i portalen visas inte*</span><span class="sxs-lookup"><span data-stu-id="ac4a7-143">*I don't see data in the portal*</span></span>

* <span data-ttu-id="ac4a7-144">Öppna [Sök] [ diagnostic] att se om raw händelser har tagits emot.</span><span class="sxs-lookup"><span data-stu-id="ac4a7-144">Open [Search][diagnostic] to see if the raw events have arrived.</span></span> <span data-ttu-id="ac4a7-145">Ibland kan de ta längre tid att visas i metrics explorer.</span><span class="sxs-lookup"><span data-stu-id="ac4a7-145">Sometimes they take longer to appear in metrics explorer.</span></span>
* <span data-ttu-id="ac4a7-146">Du kan behöva [ange brandväggsundantag för utgående data](app-insights-ip-addresses.md)</span><span class="sxs-lookup"><span data-stu-id="ac4a7-146">You might need to [set firewall exceptions for outgoing data](app-insights-ip-addresses.md)</span></span>
* <span data-ttu-id="ac4a7-147">Aktivera spårning i Application Insights plugin-programmet.</span><span class="sxs-lookup"><span data-stu-id="ac4a7-147">Enable tracing in the Application Insights plugin.</span></span> <span data-ttu-id="ac4a7-148">Lägg till följande rad i `<Plugin ApplicationInsightsWriter>`:</span><span class="sxs-lookup"><span data-stu-id="ac4a7-148">Add this line within `<Plugin ApplicationInsightsWriter>`:</span></span>
  * `SDKLogger true`
* <span data-ttu-id="ac4a7-149">Öppna en terminal och starta collectd i utförligt läge, se eventuella problem som den rapporterar:</span><span class="sxs-lookup"><span data-stu-id="ac4a7-149">Open a terminal and start collectd in verbose mode, to see any issues it is reporting:</span></span>
  * `sudo collectd -f`

## <a name="known-issue"></a><span data-ttu-id="ac4a7-150">Kända problem</span><span class="sxs-lookup"><span data-stu-id="ac4a7-150">Known issue</span></span>

<span data-ttu-id="ac4a7-151">Application Insights skriva plugin-programmet är inte kompatibelt med vissa Läs plugin-program.</span><span class="sxs-lookup"><span data-stu-id="ac4a7-151">The Application Insights Write plugin is incompatible with certain Read plugins.</span></span> <span data-ttu-id="ac4a7-152">Vissa plugin-program att ibland skicka ”NaN” där plugin-programmet Application Insights förväntar sig ett flyttal.</span><span class="sxs-lookup"><span data-stu-id="ac4a7-152">Some plugins sometimes send "NaN" where the Application Insights plugin expects a floating-point number.</span></span>

<span data-ttu-id="ac4a7-153">Symptom: Collectd loggen visar fel som innehåller ”AI:... SyntaxError: Oväntad token N ”.</span><span class="sxs-lookup"><span data-stu-id="ac4a7-153">Symptom: The collectd log shows errors that include "AI: ... SyntaxError: Unexpected token N".</span></span>

<span data-ttu-id="ac4a7-154">Lösning: Utesluta data som samlas in av plugin-program för problemet skrivåtgärder.</span><span class="sxs-lookup"><span data-stu-id="ac4a7-154">Workaround: Exclude data collected by the problem Write plugins.</span></span> 

<!--Link references-->

[api]: app-insights-api-custom-events-metrics.md
[apiexceptions]: app-insights-api-custom-events-metrics.md#track-exception
[availability]: app-insights-monitor-web-app-availability.md
[diagnostic]: app-insights-diagnostic-search.md
[eclipse]: app-insights-java-eclipse.md
[java]: app-insights-java-get-started.md
[javalogs]: app-insights-java-trace-logs.md
[metrics]: app-insights-metrics-explorer.md


