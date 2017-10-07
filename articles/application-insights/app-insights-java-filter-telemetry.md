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
# <a name="filter-telemetry-in-your-java-web-app"></a>Filtrera telemetri i Java-webbappen

Filter innehåller en sätt tooselect hello telemetri som din [Java-webbapp skickar tooApplication insikter](app-insights-java-get-started.md). Det finns vissa out box-filter som du kan använda och du kan också skriva egna anpassade filter.

hello out box filtren är:

* Spåra allvarlighetsgrad
* Specifika URL: er, nyckelord eller svarskoder
* Snabba svar – det vill säga begäran toowhich appen svarade tooquickly
* Namn på specifika händelser

> [!NOTE]
> Filter skeva hello mätvärden för din app. Du kan till exempel bestämma att, i ordning toodiagnose långsamt svar, som ett filter toodiscard snabba svarstider. Men du måste vara medveten om att hello genomsnittliga responstider rapporteras av Application Insights blir långsammare än hello true hastighet och hello antal begäranden ska vara mindre än hello verkliga antal.
> Om detta är ett problem kan använda [provtagning](app-insights-sampling.md) i stället.

## <a name="setting-filters"></a>Ange filter

ApplicationInsights.xml, lägga till en `TelemetryProcessors` avsnitt som det här exemplet:


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




[Inspektera hello fullständig uppsättning inbyggda processorer](https://github.com/Microsoft/ApplicationInsights-Java/tree/master/core/src/main/java/com/microsoft/applicationinsights/internal/processor).

## <a name="built-in-filters"></a>Inbyggda filter

### <a name="metric-telemetry-filter"></a>Mått telemetri filter

```XML

           <Processor type="MetricTelemetryFilter">
                  <Add name="NotNeeded" value="metric1,metric2"/>
           </Processor>
```

* `NotNeeded`– Kommaavgränsad lista över anpassade mått namn.


### <a name="page-view-telemetry-filter"></a>Sidan Visa telemetri filter

```XML

           <Processor type="PageViewTelemetryFilter">
                  <Add name="DurationThresholdInMS" value="500"/>
                  <Add name="NotNeededNames" value="page1,page2"/>
                  <Add name="NotNeededUrls" value="url1,url2"/>
           </Processor>
```

* `DurationThresholdInMS`-Varaktighet refererar toohello tidsåtgång tooload hello sidan. Om detta anges rapporteras inte sidor som lästs in snabbare än den angivna tiden.
* `NotNeededNames`– Kommaavgränsad lista över namnen.
* `NotNeededUrls`– Kommaavgränsad lista över URL-fragment. Till exempel `"home"` filtrerar ut alla sidor som har ”hem” hello-URL.


### <a name="request-telemetry-filter"></a>Begära telemetri Filter


```XML

           <Processor type="RequestTelemetryFilter">
                  <Add name="MinimumDurationInMS" value="500"/>
                  <Add name="NotNeededResponseCodes" value="page1,page2"/>
                  <Add name="NotNeededUrls" value="url1,url2"/>
           </Processor>
```



### <a name="synthetic-source-filter"></a>Syntetiska källfiltret

Filtrerar ut all telemetri som innehåller värden i hello SyntheticSource egenskapen. Dessa inkluderar begäranden från robotar, spindlar och tillgänglighetstester.

Filtrera bort telemetri för syntetiska begäranden:


```XML

           <Processor type="SyntheticSourceFilter" />
```

Filtrera bort telemetri för särskilda syntetiska källor:


```XML

           <Processor type="SyntheticSourceFilter" >
                  <Add name="NotNeeded" value="source1,source2"/>
           </Processor>
```

* `NotNeeded`– Kommaavgränsad lista över syntetisk datakällor.

### <a name="telemetry-event-filter"></a>Händelsefilter för telemetri

Anpassade händelser (loggat med [trackevent ()](app-insights-api-custom-events-metrics.md#trackevent)).


```XML

           <Processor type="TelemetryEventFilter" >
                  <Add name="NotNeededNames" value="event1, event2"/>
           </Processor>
```


* `NotNeededNames`– Kommaavgränsad lista över händelsenamn.


### <a name="trace-telemetry-filter"></a>Spåra telemetri filter

Filter logga spår (loggat med [TrackTrace()](app-insights-api-custom-events-metrics.md#tracktrace) eller en [loggning framework insamlaren](app-insights-java-trace-logs.md)).

```XML

           <Processor type="TraceTelemetryFilter">
                  <Add name="FromSeverityLevel" value="ERROR"/>
           </Processor>
```

* `FromSeverityLevel`Giltiga värden är:
 *  INAKTIVERA - filtrera bort alla spårningar
 *  TRACE - ingen filtrering. är lika med tooTrace nivå
 *  INFO - filtrera bort spårningsnivå
 *  Varna - Filter i SPÅRNINGEN och information
 *  FEL - filtrera bort Varna, INFO, SPÅRNING
 *  KRITISK - filtrera bort alla utom kritiska


## <a name="custom-filters"></a>Anpassade filter

### <a name="1-code-your-filter"></a>1. Code filtret

I koden, skapar du en klass som implementerar `TelemetryProcessor`:

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


### <a name="2-invoke-your-filter-in-hello-configuration-file"></a>2. Anropa filtret i hello-konfigurationsfil

I ApplicationInsights.xml:

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

## <a name="troubleshooting"></a>Felsökning

*Mina filter fungerar inte.*

* Kontrollera att du har angett giltiga parametervärden. Till exempel måste varaktighet vara heltal. Ogiltiga värden kommer hello filter toobe ignoreras. Om ditt filter genererar ett undantag från en konstruktor eller set-metod, kommer att ignoreras.

## <a name="next-steps"></a>Nästa steg

* [Provtagning](app-insights-sampling.md) -Överväg provtagning som ett alternativ som inte skeva din mått.
