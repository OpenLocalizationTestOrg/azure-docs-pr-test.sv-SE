---
title: Filtrera Azure Application Insights telemetri i Java-webbappen | Microsoft Docs
description: "Minska telemetri trafiken genom att filtrera bort händelser som du inte behöver övervaka."
services: application-insights
documentationcenter: 
author: CFreemanwa
manager: carmonm
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 11/23/2016
ms.author: bwren
ms.openlocfilehash: 5f6d6d4ad590b85810c42e9f9520850024c5446a
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/18/2017
---
# <a name="filter-telemetry-in-your-java-web-app"></a><span data-ttu-id="2b67f-103">Filtrera telemetri i Java-webbappen</span><span class="sxs-lookup"><span data-stu-id="2b67f-103">Filter telemetry in your Java web app</span></span>

<span data-ttu-id="2b67f-104">Filter ger dig ett sätt att välja telemetrin som din [Java-webbapp skickar till Application Insights](app-insights-java-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="2b67f-104">Filters provide a way to select the telemetry that your [Java web app sends to Application Insights](app-insights-java-get-started.md).</span></span> <span data-ttu-id="2b67f-105">Det finns vissa out box-filter som du kan använda och du kan också skriva egna anpassade filter.</span><span class="sxs-lookup"><span data-stu-id="2b67f-105">There are some out-of-the-box filters that you can use, and you can also write your own custom filters.</span></span>

<span data-ttu-id="2b67f-106">Out box-filtren är:</span><span class="sxs-lookup"><span data-stu-id="2b67f-106">The out-of-the-box filters include:</span></span>

* <span data-ttu-id="2b67f-107">Spåra allvarlighetsgrad</span><span class="sxs-lookup"><span data-stu-id="2b67f-107">Trace severity level</span></span>
* <span data-ttu-id="2b67f-108">Specifika URL: er, nyckelord eller svarskoder</span><span class="sxs-lookup"><span data-stu-id="2b67f-108">Specific URLs, keywords or response codes</span></span>
* <span data-ttu-id="2b67f-109">Snabba svar – det vill säga begäran som din app svarat på snabbt</span><span class="sxs-lookup"><span data-stu-id="2b67f-109">Fast responses - that is, requests to which your app responded to quickly</span></span>
* <span data-ttu-id="2b67f-110">Namn på specifika händelser</span><span class="sxs-lookup"><span data-stu-id="2b67f-110">Specific event names</span></span>

> [!NOTE]
> <span data-ttu-id="2b67f-111">Filter skeva mätvärden för din app.</span><span class="sxs-lookup"><span data-stu-id="2b67f-111">Filters skew the metrics of your app.</span></span> <span data-ttu-id="2b67f-112">Du kan till exempel bestämma att, för att kunna diagnostisera långsamt svar du anger ett filter för att ta bort snabba svarstider.</span><span class="sxs-lookup"><span data-stu-id="2b67f-112">For example, you might decide that, in order to diagnose slow responses, you will set a filter to discard fast response times.</span></span> <span data-ttu-id="2b67f-113">Men du måste vara medveten om att de genomsnittliga svarstider som rapporterats av Application Insights blir långsammare än true hastighet och antal begäranden som ska vara mindre än antalet verkliga.</span><span class="sxs-lookup"><span data-stu-id="2b67f-113">But you must be aware that the average response times reported by Application Insights will then be slower than the true speed, and the count of requests will be smaller than the real count.</span></span>
> <span data-ttu-id="2b67f-114">Om detta är ett problem kan använda [provtagning](app-insights-sampling.md) i stället.</span><span class="sxs-lookup"><span data-stu-id="2b67f-114">If this is a concern, use [Sampling](app-insights-sampling.md) instead.</span></span>

## <a name="setting-filters"></a><span data-ttu-id="2b67f-115">Ange filter</span><span class="sxs-lookup"><span data-stu-id="2b67f-115">Setting filters</span></span>

<span data-ttu-id="2b67f-116">ApplicationInsights.xml, lägga till en `TelemetryProcessors` avsnitt som det här exemplet:</span><span class="sxs-lookup"><span data-stu-id="2b67f-116">In ApplicationInsights.xml, add a `TelemetryProcessors` section like this example:</span></span>


```XML

    <ApplicationInsights>
      <TelemetryProcessors>

        <BuiltInProcessors>
           <Processor type="TraceTelemetryFilter">
                  <Add name="FromSeverityLevel" value="ERROR"/>
           </Processor>

           <Processor type="RequestTelemetryFilter">
                  <Add name="MinimumDurationInMS" value="100"/>
                  <Add name="NotNeededResponseCodes" value="200-400"/>
           </Processor>

           <Processor type="PageViewTelemetryFilter">
                  <Add name="DurationThresholdInMS" value="100"/>
                  <Add name="NotNeededNames" value="home,index"/>
                  <Add name="NotNeededUrls" value=".jpg,.css"/>
           </Processor>

           <Processor type="TelemetryEventFilter">
                  <!-- Names of events we don't want to see -->
                  <Add name="NotNeededNames" value="Start,Stop,Pause"/>
           </Processor>

           <!-- Exclude telemetry from availability tests and bots -->
           <Processor type="SyntheticSourceFilter">
                <!-- Optional: specify which synthetic sources,
                     comma-separated
                     - default is all synthetics -->
                <Add name="NotNeededSources" value="Application Insights Availability Monitoring,BingPreview"
           </Processor>

        </BuiltInProcessors>

        <CustomProcessors>
          <Processor type="com.fabrikam.MyFilter">
            <Add name="Successful" value="false"/>
          </Processor>
        </CustomProcessors>

      </TelemetryProcessors>
    </ApplicationInsights>

```




<span data-ttu-id="2b67f-117">[Kontrollera en fullständig uppsättning inbyggda processorer](https://github.com/Microsoft/ApplicationInsights-Java/tree/master/core/src/main/java/com/microsoft/applicationinsights/internal/processor).</span><span class="sxs-lookup"><span data-stu-id="2b67f-117">[Inspect the full set of built-in processors](https://github.com/Microsoft/ApplicationInsights-Java/tree/master/core/src/main/java/com/microsoft/applicationinsights/internal/processor).</span></span>

## <a name="built-in-filters"></a><span data-ttu-id="2b67f-118">Inbyggda filter</span><span class="sxs-lookup"><span data-stu-id="2b67f-118">Built-in filters</span></span>

### <a name="metric-telemetry-filter"></a><span data-ttu-id="2b67f-119">Mått telemetri filter</span><span class="sxs-lookup"><span data-stu-id="2b67f-119">Metric Telemetry filter</span></span>

```XML

           <Processor type="MetricTelemetryFilter">
                  <Add name="NotNeeded" value="metric1,metric2"/>
           </Processor>
```

* <span data-ttu-id="2b67f-120">`NotNeeded`– Kommaavgränsad lista över anpassade mått namn.</span><span class="sxs-lookup"><span data-stu-id="2b67f-120">`NotNeeded` - Comma-separated list of custom metric names.</span></span>


### <a name="page-view-telemetry-filter"></a><span data-ttu-id="2b67f-121">Sidan Visa telemetri filter</span><span class="sxs-lookup"><span data-stu-id="2b67f-121">Page View Telemetry filter</span></span>

```XML

           <Processor type="PageViewTelemetryFilter">
                  <Add name="DurationThresholdInMS" value="500"/>
                  <Add name="NotNeededNames" value="page1,page2"/>
                  <Add name="NotNeededUrls" value="url1,url2"/>
           </Processor>
```

* <span data-ttu-id="2b67f-122">`DurationThresholdInMS`-Varaktighet refererar till den tid det tar att läsa in sidan.</span><span class="sxs-lookup"><span data-stu-id="2b67f-122">`DurationThresholdInMS` - Duration refers to the time taken to load the page.</span></span> <span data-ttu-id="2b67f-123">Om detta anges rapporteras inte sidor som lästs in snabbare än den angivna tiden.</span><span class="sxs-lookup"><span data-stu-id="2b67f-123">If this is set, pages that loaded faster than this time are not reported.</span></span>
* <span data-ttu-id="2b67f-124">`NotNeededNames`– Kommaavgränsad lista över namnen.</span><span class="sxs-lookup"><span data-stu-id="2b67f-124">`NotNeededNames` - Comma-separated list of page names.</span></span>
* <span data-ttu-id="2b67f-125">`NotNeededUrls`– Kommaavgränsad lista över URL-fragment.</span><span class="sxs-lookup"><span data-stu-id="2b67f-125">`NotNeededUrls` - Comma-separated list of URL fragments.</span></span> <span data-ttu-id="2b67f-126">Till exempel `"home"` filtrerar ut alla sidor som har ”hem” i Webbadressen.</span><span class="sxs-lookup"><span data-stu-id="2b67f-126">For example, `"home"` filters out all pages that have "home" in the URL.</span></span>


### <a name="request-telemetry-filter"></a><span data-ttu-id="2b67f-127">Begära telemetri Filter</span><span class="sxs-lookup"><span data-stu-id="2b67f-127">Request Telemetry Filter</span></span>


```XML

           <Processor type="RequestTelemetryFilter">
                  <Add name="MinimumDurationInMS" value="500"/>
                  <Add name="NotNeededResponseCodes" value="page1,page2"/>
                  <Add name="NotNeededUrls" value="url1,url2"/>
           </Processor>
```



### <a name="synthetic-source-filter"></a><span data-ttu-id="2b67f-128">Syntetiska källfiltret</span><span class="sxs-lookup"><span data-stu-id="2b67f-128">Synthetic Source filter</span></span>

<span data-ttu-id="2b67f-129">Filtrerar ut all telemetri som värden i egenskapen SyntheticSource.</span><span class="sxs-lookup"><span data-stu-id="2b67f-129">Filters out all telemetry that have values in the SyntheticSource property.</span></span> <span data-ttu-id="2b67f-130">Dessa inkluderar begäranden från robotar, spindlar och tillgänglighetstester.</span><span class="sxs-lookup"><span data-stu-id="2b67f-130">These include requests from bots, spiders and availability tests.</span></span>

<span data-ttu-id="2b67f-131">Filtrera bort telemetri för syntetiska begäranden:</span><span class="sxs-lookup"><span data-stu-id="2b67f-131">Filter out telemetry for all synthetic requests:</span></span>


```XML

           <Processor type="SyntheticSourceFilter" />
```

<span data-ttu-id="2b67f-132">Filtrera bort telemetri för särskilda syntetiska källor:</span><span class="sxs-lookup"><span data-stu-id="2b67f-132">Filter out telemetry for specific synthetic sources:</span></span>


```XML

           <Processor type="SyntheticSourceFilter" >
                  <Add name="NotNeeded" value="source1,source2"/>
           </Processor>
```

* <span data-ttu-id="2b67f-133">`NotNeeded`– Kommaavgränsad lista över syntetisk datakällor.</span><span class="sxs-lookup"><span data-stu-id="2b67f-133">`NotNeeded` - Comma-separated list of synthetic source names.</span></span>

### <a name="telemetry-event-filter"></a><span data-ttu-id="2b67f-134">Händelsefilter för telemetri</span><span class="sxs-lookup"><span data-stu-id="2b67f-134">Telemetry Event filter</span></span>

<span data-ttu-id="2b67f-135">Anpassade händelser (loggat med [trackevent ()](app-insights-api-custom-events-metrics.md#trackevent)).</span><span class="sxs-lookup"><span data-stu-id="2b67f-135">Filters custom events (logged using [TrackEvent()](app-insights-api-custom-events-metrics.md#trackevent)).</span></span>


```XML

           <Processor type="TelemetryEventFilter" >
                  <Add name="NotNeededNames" value="event1, event2"/>
           </Processor>
```


* <span data-ttu-id="2b67f-136">`NotNeededNames`– Kommaavgränsad lista över händelsenamn.</span><span class="sxs-lookup"><span data-stu-id="2b67f-136">`NotNeededNames` - Comma-separated list of event names.</span></span>


### <a name="trace-telemetry-filter"></a><span data-ttu-id="2b67f-137">Spåra telemetri filter</span><span class="sxs-lookup"><span data-stu-id="2b67f-137">Trace Telemetry filter</span></span>

<span data-ttu-id="2b67f-138">Filter logga spår (loggat med [TrackTrace()](app-insights-api-custom-events-metrics.md#tracktrace) eller en [loggning framework insamlaren](app-insights-java-trace-logs.md)).</span><span class="sxs-lookup"><span data-stu-id="2b67f-138">Filters log traces (logged using [TrackTrace()](app-insights-api-custom-events-metrics.md#tracktrace) or a [logging framework collector](app-insights-java-trace-logs.md)).</span></span>

```XML

           <Processor type="TraceTelemetryFilter">
                  <Add name="FromSeverityLevel" value="ERROR"/>
           </Processor>
```

* <span data-ttu-id="2b67f-139">`FromSeverityLevel`Giltiga värden är:</span><span class="sxs-lookup"><span data-stu-id="2b67f-139">`FromSeverityLevel` valid values are:</span></span>
 *  <span data-ttu-id="2b67f-140">INAKTIVERA - filtrera bort alla spårningar</span><span class="sxs-lookup"><span data-stu-id="2b67f-140">OFF             - Filter out ALL traces</span></span>
 *  <span data-ttu-id="2b67f-141">TRACE - ingen filtrering.</span><span class="sxs-lookup"><span data-stu-id="2b67f-141">TRACE           - No filtering.</span></span> <span data-ttu-id="2b67f-142">Spårningsnivån som motsvarar</span><span class="sxs-lookup"><span data-stu-id="2b67f-142">equals to Trace level</span></span>
 *  <span data-ttu-id="2b67f-143">INFO - filtrera bort spårningsnivå</span><span class="sxs-lookup"><span data-stu-id="2b67f-143">INFO            - Filter out TRACE level</span></span>
 *  <span data-ttu-id="2b67f-144">Varna - Filter i SPÅRNINGEN och information</span><span class="sxs-lookup"><span data-stu-id="2b67f-144">WARN            - Filter out TRACE and INFO</span></span>
 *  <span data-ttu-id="2b67f-145">FEL - filtrera bort Varna, INFO, SPÅRNING</span><span class="sxs-lookup"><span data-stu-id="2b67f-145">ERROR           - Filter out WARN, INFO, TRACE</span></span>
 *  <span data-ttu-id="2b67f-146">KRITISK - filtrera bort alla utom kritiska</span><span class="sxs-lookup"><span data-stu-id="2b67f-146">CRITICAL        - filter out all but CRITICAL</span></span>


## <a name="custom-filters"></a><span data-ttu-id="2b67f-147">Anpassade filter</span><span class="sxs-lookup"><span data-stu-id="2b67f-147">Custom filters</span></span>

### <a name="1-code-your-filter"></a><span data-ttu-id="2b67f-148">1. Code filtret</span><span class="sxs-lookup"><span data-stu-id="2b67f-148">1. Code your filter</span></span>

<span data-ttu-id="2b67f-149">I koden, skapar du en klass som implementerar `TelemetryProcessor`:</span><span class="sxs-lookup"><span data-stu-id="2b67f-149">In your code, create a class that implements `TelemetryProcessor`:</span></span>

```Java

    package com.fabrikam.MyFilter;
    import com.microsoft.applicationinsights.extensibility.TelemetryProcessor;
    import com.microsoft.applicationinsights.telemetry.Telemetry;

    public class SuccessFilter implements TelemetryProcessor {

       /* Any parameters that are required to support the filter.*/
       private final String successful;

       /* Initializers for the parameters, named "setParameterName" */
       public void setNotNeeded(String successful)
       {
          this.successful = successful;
       }

       /* This method is called for each item of telemetry to be sent.
          Return false to discard it.
          Return true to allow other processors to inspect it. */
       @Override
       public boolean process(Telemetry telemetry) {
        if (telemetry == null) { return true; }
        if (telemetry instanceof RequestTelemetry)
        {
            RequestTelemetry requestTelemetry = (RequestTelemetry)telemetry;
            return request.getSuccess() == successful;
        }
        return true;
       }
    }

```


### <a name="2-invoke-your-filter-in-the-configuration-file"></a><span data-ttu-id="2b67f-150">2. Anropa filtret i konfigurationsfilen</span><span class="sxs-lookup"><span data-stu-id="2b67f-150">2. Invoke your filter in the configuration file</span></span>

<span data-ttu-id="2b67f-151">I ApplicationInsights.xml:</span><span class="sxs-lookup"><span data-stu-id="2b67f-151">In ApplicationInsights.xml:</span></span>

```XML


    <ApplicationInsights>
      <TelemetryProcessors>
        <CustomProcessors>
          <Processor type="com.fabrikam.SuccessFilter">
            <Add name="Successful" value="false"/>
          </Processor>
        </CustomProcessors>
      </TelemetryProcessors>
    </ApplicationInsights>

```

## <a name="troubleshooting"></a><span data-ttu-id="2b67f-152">Felsökning</span><span class="sxs-lookup"><span data-stu-id="2b67f-152">Troubleshooting</span></span>

<span data-ttu-id="2b67f-153">*Mina filter fungerar inte.*</span><span class="sxs-lookup"><span data-stu-id="2b67f-153">*My filter isn't working.*</span></span>

* <span data-ttu-id="2b67f-154">Kontrollera att du har angett giltiga parametervärden.</span><span class="sxs-lookup"><span data-stu-id="2b67f-154">Check that you have provided valid parameter values.</span></span> <span data-ttu-id="2b67f-155">Till exempel måste varaktighet vara heltal.</span><span class="sxs-lookup"><span data-stu-id="2b67f-155">For example, durations should be integers.</span></span> <span data-ttu-id="2b67f-156">Ogiltiga värden kommer att orsaka filtret som ska ignoreras.</span><span class="sxs-lookup"><span data-stu-id="2b67f-156">Invalid values will cause the filter to be ignored.</span></span> <span data-ttu-id="2b67f-157">Om ditt filter genererar ett undantag från en konstruktor eller set-metod, kommer att ignoreras.</span><span class="sxs-lookup"><span data-stu-id="2b67f-157">If your custom filter throws an exception from a constructor or set method, it will be ignored.</span></span>

## <a name="next-steps"></a><span data-ttu-id="2b67f-158">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="2b67f-158">Next steps</span></span>

* <span data-ttu-id="2b67f-159">[Provtagning](app-insights-sampling.md) -Överväg provtagning som ett alternativ som inte skeva din mått.</span><span class="sxs-lookup"><span data-stu-id="2b67f-159">[Sampling](app-insights-sampling.md) - Consider sampling as an alternative that does not skew your metrics.</span></span>
