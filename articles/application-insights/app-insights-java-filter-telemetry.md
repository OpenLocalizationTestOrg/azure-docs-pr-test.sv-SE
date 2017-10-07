---
title: aaaFilter Azure Application Insights telemetri i Java-webbappen | Microsoft Docs
description: "Minska telemetri trafiken genom att filtrera bort händelser hello behöver du inte toomonitor."
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
ms.openlocfilehash: 95713e11d5f86472777c67e4e7f3177fbf2cd0b4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="filter-telemetry-in-your-java-web-app"></a><span data-ttu-id="96fd0-103">Filtrera telemetri i Java-webbappen</span><span class="sxs-lookup"><span data-stu-id="96fd0-103">Filter telemetry in your Java web app</span></span>

<span data-ttu-id="96fd0-104">Filter innehåller en sätt tooselect hello telemetri som din [Java-webbapp skickar tooApplication insikter](app-insights-java-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="96fd0-104">Filters provide a way tooselect hello telemetry that your [Java web app sends tooApplication Insights](app-insights-java-get-started.md).</span></span> <span data-ttu-id="96fd0-105">Det finns vissa out box-filter som du kan använda och du kan också skriva egna anpassade filter.</span><span class="sxs-lookup"><span data-stu-id="96fd0-105">There are some out-of-the-box filters that you can use, and you can also write your own custom filters.</span></span>

<span data-ttu-id="96fd0-106">hello out box filtren är:</span><span class="sxs-lookup"><span data-stu-id="96fd0-106">hello out-of-the-box filters include:</span></span>

* <span data-ttu-id="96fd0-107">Spåra allvarlighetsgrad</span><span class="sxs-lookup"><span data-stu-id="96fd0-107">Trace severity level</span></span>
* <span data-ttu-id="96fd0-108">Specifika URL: er, nyckelord eller svarskoder</span><span class="sxs-lookup"><span data-stu-id="96fd0-108">Specific URLs, keywords or response codes</span></span>
* <span data-ttu-id="96fd0-109">Snabba svar – det vill säga begäran toowhich appen svarade tooquickly</span><span class="sxs-lookup"><span data-stu-id="96fd0-109">Fast responses - that is, requests toowhich your app responded tooquickly</span></span>
* <span data-ttu-id="96fd0-110">Namn på specifika händelser</span><span class="sxs-lookup"><span data-stu-id="96fd0-110">Specific event names</span></span>

> [!NOTE]
> <span data-ttu-id="96fd0-111">Filter skeva hello mätvärden för din app.</span><span class="sxs-lookup"><span data-stu-id="96fd0-111">Filters skew hello metrics of your app.</span></span> <span data-ttu-id="96fd0-112">Du kan till exempel bestämma att, i ordning toodiagnose långsamt svar, som ett filter toodiscard snabba svarstider.</span><span class="sxs-lookup"><span data-stu-id="96fd0-112">For example, you might decide that, in order toodiagnose slow responses, you will set a filter toodiscard fast response times.</span></span> <span data-ttu-id="96fd0-113">Men du måste vara medveten om att hello genomsnittliga responstider rapporteras av Application Insights blir långsammare än hello true hastighet och hello antal begäranden ska vara mindre än hello verkliga antal.</span><span class="sxs-lookup"><span data-stu-id="96fd0-113">But you must be aware that hello average response times reported by Application Insights will then be slower than hello true speed, and hello count of requests will be smaller than hello real count.</span></span>
> <span data-ttu-id="96fd0-114">Om detta är ett problem kan använda [provtagning](app-insights-sampling.md) i stället.</span><span class="sxs-lookup"><span data-stu-id="96fd0-114">If this is a concern, use [Sampling](app-insights-sampling.md) instead.</span></span>

## <a name="setting-filters"></a><span data-ttu-id="96fd0-115">Ange filter</span><span class="sxs-lookup"><span data-stu-id="96fd0-115">Setting filters</span></span>

<span data-ttu-id="96fd0-116">ApplicationInsights.xml, lägga till en `TelemetryProcessors` avsnitt som det här exemplet:</span><span class="sxs-lookup"><span data-stu-id="96fd0-116">In ApplicationInsights.xml, add a `TelemetryProcessors` section like this example:</span></span>


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
                  <!-- Names of events we don't want toosee -->
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




<span data-ttu-id="96fd0-117">[Inspektera hello fullständig uppsättning inbyggda processorer](https://github.com/Microsoft/ApplicationInsights-Java/tree/master/core/src/main/java/com/microsoft/applicationinsights/internal/processor).</span><span class="sxs-lookup"><span data-stu-id="96fd0-117">[Inspect hello full set of built-in processors](https://github.com/Microsoft/ApplicationInsights-Java/tree/master/core/src/main/java/com/microsoft/applicationinsights/internal/processor).</span></span>

## <a name="built-in-filters"></a><span data-ttu-id="96fd0-118">Inbyggda filter</span><span class="sxs-lookup"><span data-stu-id="96fd0-118">Built-in filters</span></span>

### <a name="metric-telemetry-filter"></a><span data-ttu-id="96fd0-119">Mått telemetri filter</span><span class="sxs-lookup"><span data-stu-id="96fd0-119">Metric Telemetry filter</span></span>

```XML

           <Processor type="MetricTelemetryFilter">
                  <Add name="NotNeeded" value="metric1,metric2"/>
           </Processor>
```

* <span data-ttu-id="96fd0-120">`NotNeeded`– Kommaavgränsad lista över anpassade mått namn.</span><span class="sxs-lookup"><span data-stu-id="96fd0-120">`NotNeeded` - Comma-separated list of custom metric names.</span></span>


### <a name="page-view-telemetry-filter"></a><span data-ttu-id="96fd0-121">Sidan Visa telemetri filter</span><span class="sxs-lookup"><span data-stu-id="96fd0-121">Page View Telemetry filter</span></span>

```XML

           <Processor type="PageViewTelemetryFilter">
                  <Add name="DurationThresholdInMS" value="500"/>
                  <Add name="NotNeededNames" value="page1,page2"/>
                  <Add name="NotNeededUrls" value="url1,url2"/>
           </Processor>
```

* <span data-ttu-id="96fd0-122">`DurationThresholdInMS`-Varaktighet refererar toohello tidsåtgång tooload hello sidan.</span><span class="sxs-lookup"><span data-stu-id="96fd0-122">`DurationThresholdInMS` - Duration refers toohello time taken tooload hello page.</span></span> <span data-ttu-id="96fd0-123">Om detta anges rapporteras inte sidor som lästs in snabbare än den angivna tiden.</span><span class="sxs-lookup"><span data-stu-id="96fd0-123">If this is set, pages that loaded faster than this time are not reported.</span></span>
* <span data-ttu-id="96fd0-124">`NotNeededNames`– Kommaavgränsad lista över namnen.</span><span class="sxs-lookup"><span data-stu-id="96fd0-124">`NotNeededNames` - Comma-separated list of page names.</span></span>
* <span data-ttu-id="96fd0-125">`NotNeededUrls`– Kommaavgränsad lista över URL-fragment.</span><span class="sxs-lookup"><span data-stu-id="96fd0-125">`NotNeededUrls` - Comma-separated list of URL fragments.</span></span> <span data-ttu-id="96fd0-126">Till exempel `"home"` filtrerar ut alla sidor som har ”hem” hello-URL.</span><span class="sxs-lookup"><span data-stu-id="96fd0-126">For example, `"home"` filters out all pages that have "home" in hello URL.</span></span>


### <a name="request-telemetry-filter"></a><span data-ttu-id="96fd0-127">Begära telemetri Filter</span><span class="sxs-lookup"><span data-stu-id="96fd0-127">Request Telemetry Filter</span></span>


```XML

           <Processor type="RequestTelemetryFilter">
                  <Add name="MinimumDurationInMS" value="500"/>
                  <Add name="NotNeededResponseCodes" value="page1,page2"/>
                  <Add name="NotNeededUrls" value="url1,url2"/>
           </Processor>
```



### <a name="synthetic-source-filter"></a><span data-ttu-id="96fd0-128">Syntetiska källfiltret</span><span class="sxs-lookup"><span data-stu-id="96fd0-128">Synthetic Source filter</span></span>

<span data-ttu-id="96fd0-129">Filtrerar ut all telemetri som innehåller värden i hello SyntheticSource egenskapen.</span><span class="sxs-lookup"><span data-stu-id="96fd0-129">Filters out all telemetry that have values in hello SyntheticSource property.</span></span> <span data-ttu-id="96fd0-130">Dessa inkluderar begäranden från robotar, spindlar och tillgänglighetstester.</span><span class="sxs-lookup"><span data-stu-id="96fd0-130">These include requests from bots, spiders and availability tests.</span></span>

<span data-ttu-id="96fd0-131">Filtrera bort telemetri för syntetiska begäranden:</span><span class="sxs-lookup"><span data-stu-id="96fd0-131">Filter out telemetry for all synthetic requests:</span></span>


```XML

           <Processor type="SyntheticSourceFilter" />
```

<span data-ttu-id="96fd0-132">Filtrera bort telemetri för särskilda syntetiska källor:</span><span class="sxs-lookup"><span data-stu-id="96fd0-132">Filter out telemetry for specific synthetic sources:</span></span>


```XML

           <Processor type="SyntheticSourceFilter" >
                  <Add name="NotNeeded" value="source1,source2"/>
           </Processor>
```

* <span data-ttu-id="96fd0-133">`NotNeeded`– Kommaavgränsad lista över syntetisk datakällor.</span><span class="sxs-lookup"><span data-stu-id="96fd0-133">`NotNeeded` - Comma-separated list of synthetic source names.</span></span>

### <a name="telemetry-event-filter"></a><span data-ttu-id="96fd0-134">Händelsefilter för telemetri</span><span class="sxs-lookup"><span data-stu-id="96fd0-134">Telemetry Event filter</span></span>

<span data-ttu-id="96fd0-135">Anpassade händelser (loggat med [trackevent ()](app-insights-api-custom-events-metrics.md#trackevent)).</span><span class="sxs-lookup"><span data-stu-id="96fd0-135">Filters custom events (logged using [TrackEvent()](app-insights-api-custom-events-metrics.md#trackevent)).</span></span>


```XML

           <Processor type="TelemetryEventFilter" >
                  <Add name="NotNeededNames" value="event1, event2"/>
           </Processor>
```


* <span data-ttu-id="96fd0-136">`NotNeededNames`– Kommaavgränsad lista över händelsenamn.</span><span class="sxs-lookup"><span data-stu-id="96fd0-136">`NotNeededNames` - Comma-separated list of event names.</span></span>


### <a name="trace-telemetry-filter"></a><span data-ttu-id="96fd0-137">Spåra telemetri filter</span><span class="sxs-lookup"><span data-stu-id="96fd0-137">Trace Telemetry filter</span></span>

<span data-ttu-id="96fd0-138">Filter logga spår (loggat med [TrackTrace()](app-insights-api-custom-events-metrics.md#tracktrace) eller en [loggning framework insamlaren](app-insights-java-trace-logs.md)).</span><span class="sxs-lookup"><span data-stu-id="96fd0-138">Filters log traces (logged using [TrackTrace()](app-insights-api-custom-events-metrics.md#tracktrace) or a [logging framework collector](app-insights-java-trace-logs.md)).</span></span>

```XML

           <Processor type="TraceTelemetryFilter">
                  <Add name="FromSeverityLevel" value="ERROR"/>
           </Processor>
```

* <span data-ttu-id="96fd0-139">`FromSeverityLevel`Giltiga värden är:</span><span class="sxs-lookup"><span data-stu-id="96fd0-139">`FromSeverityLevel` valid values are:</span></span>
 *  <span data-ttu-id="96fd0-140">INAKTIVERA - filtrera bort alla spårningar</span><span class="sxs-lookup"><span data-stu-id="96fd0-140">OFF             - Filter out ALL traces</span></span>
 *  <span data-ttu-id="96fd0-141">TRACE - ingen filtrering.</span><span class="sxs-lookup"><span data-stu-id="96fd0-141">TRACE           - No filtering.</span></span> <span data-ttu-id="96fd0-142">är lika med tooTrace nivå</span><span class="sxs-lookup"><span data-stu-id="96fd0-142">equals tooTrace level</span></span>
 *  <span data-ttu-id="96fd0-143">INFO - filtrera bort spårningsnivå</span><span class="sxs-lookup"><span data-stu-id="96fd0-143">INFO            - Filter out TRACE level</span></span>
 *  <span data-ttu-id="96fd0-144">Varna - Filter i SPÅRNINGEN och information</span><span class="sxs-lookup"><span data-stu-id="96fd0-144">WARN            - Filter out TRACE and INFO</span></span>
 *  <span data-ttu-id="96fd0-145">FEL - filtrera bort Varna, INFO, SPÅRNING</span><span class="sxs-lookup"><span data-stu-id="96fd0-145">ERROR           - Filter out WARN, INFO, TRACE</span></span>
 *  <span data-ttu-id="96fd0-146">KRITISK - filtrera bort alla utom kritiska</span><span class="sxs-lookup"><span data-stu-id="96fd0-146">CRITICAL        - filter out all but CRITICAL</span></span>


## <a name="custom-filters"></a><span data-ttu-id="96fd0-147">Anpassade filter</span><span class="sxs-lookup"><span data-stu-id="96fd0-147">Custom filters</span></span>

### <a name="1-code-your-filter"></a><span data-ttu-id="96fd0-148">1. Code filtret</span><span class="sxs-lookup"><span data-stu-id="96fd0-148">1. Code your filter</span></span>

<span data-ttu-id="96fd0-149">I koden, skapar du en klass som implementerar `TelemetryProcessor`:</span><span class="sxs-lookup"><span data-stu-id="96fd0-149">In your code, create a class that implements `TelemetryProcessor`:</span></span>

```Java

    package com.fabrikam.MyFilter;
    import com.microsoft.applicationinsights.extensibility.TelemetryProcessor;
    import com.microsoft.applicationinsights.telemetry.Telemetry;

    public class SuccessFilter implements TelemetryProcessor {

       /* Any parameters that are required toosupport hello filter.*/
       private final String successful;

       /* Initializers for hello parameters, named "setParameterName" */
       public void setNotNeeded(String successful)
       {
          this.successful = successful;
       }

       /* This method is called for each item of telemetry toobe sent.
          Return false toodiscard it.
          Return true tooallow other processors tooinspect it. */
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


### <a name="2-invoke-your-filter-in-hello-configuration-file"></a><span data-ttu-id="96fd0-150">2. Anropa filtret i hello-konfigurationsfil</span><span class="sxs-lookup"><span data-stu-id="96fd0-150">2. Invoke your filter in hello configuration file</span></span>

<span data-ttu-id="96fd0-151">I ApplicationInsights.xml:</span><span class="sxs-lookup"><span data-stu-id="96fd0-151">In ApplicationInsights.xml:</span></span>

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

## <a name="troubleshooting"></a><span data-ttu-id="96fd0-152">Felsökning</span><span class="sxs-lookup"><span data-stu-id="96fd0-152">Troubleshooting</span></span>

<span data-ttu-id="96fd0-153">*Mina filter fungerar inte.*</span><span class="sxs-lookup"><span data-stu-id="96fd0-153">*My filter isn't working.*</span></span>

* <span data-ttu-id="96fd0-154">Kontrollera att du har angett giltiga parametervärden.</span><span class="sxs-lookup"><span data-stu-id="96fd0-154">Check that you have provided valid parameter values.</span></span> <span data-ttu-id="96fd0-155">Till exempel måste varaktighet vara heltal.</span><span class="sxs-lookup"><span data-stu-id="96fd0-155">For example, durations should be integers.</span></span> <span data-ttu-id="96fd0-156">Ogiltiga värden kommer hello filter toobe ignoreras.</span><span class="sxs-lookup"><span data-stu-id="96fd0-156">Invalid values will cause hello filter toobe ignored.</span></span> <span data-ttu-id="96fd0-157">Om ditt filter genererar ett undantag från en konstruktor eller set-metod, kommer att ignoreras.</span><span class="sxs-lookup"><span data-stu-id="96fd0-157">If your custom filter throws an exception from a constructor or set method, it will be ignored.</span></span>

## <a name="next-steps"></a><span data-ttu-id="96fd0-158">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="96fd0-158">Next steps</span></span>

* <span data-ttu-id="96fd0-159">[Provtagning](app-insights-sampling.md) -Överväg provtagning som ett alternativ som inte skeva din mått.</span><span class="sxs-lookup"><span data-stu-id="96fd0-159">[Sampling](app-insights-sampling.md) - Consider sampling as an alternative that does not skew your metrics.</span></span>
