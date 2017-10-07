---
title: "aaaApplication insikter för Java webbappar som redan är live"
description: "Övervaka ett webbprogram som körs redan på servern"
services: application-insights
documentationcenter: java
author: harelbr
manager: carmonm
ms.assetid: 12f3dbb9-915f-4087-87c9-807286030b0b
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 11/10/2016
ms.author: bwren
ms.openlocfilehash: 2b01cd61657522ccf1d2d97b2a29cdeb08ec9a18
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="application-insights-for-java-web-apps-that-are-already-live"></a>Application Insights för Java-webbappar som redan är live


Om du har ett program som redan körs på J2EE-server kan du börja övervaka med [Programinsikter](app-insights-overview.md) utan hello behöver toomake code ändras eller kompileras projektet. Med det här alternativet får du information om HTTP-begäranden skickas tooyour server, ohanterade undantag och prestandaräknare.

Du behöver en prenumeration för[Microsoft Azure](https://azure.com).

> [!NOTE]
> hello procedur på den här sidan lägger till webbprogrammet för hello SDK tooyour vid körning. Den här runtime instrumentation är användbart om du inte vill att tooupdate eller återskapa din källkod. Men om möjligt rekommenderar vi att du [lägga till hello SDK toohello källkoden](app-insights-java-get-started.md) i stället. Som ger dig fler alternativ, till exempel skriva kod tootrack användaraktivitet.
> 
> 

## <a name="1-get-an-application-insights-instrumentation-key"></a>1. Hämta en Application Insights-instrumenteringsnyckel
1. Logga in toohello [Microsoft Azure-portalen](https://portal.azure.com)
2. Skapa en ny Application Insights-resurs och ange hello programmet typen tooJava webbprogram.
   
    ![Fyll i ett namn, välj Java-webbapp och klicka på Skapa](./media/app-insights-java-live/02-create.png)

    hello resursen skapas i några sekunder.

4. Öppna hello ny resurs och dess instrumentation nyckel. Du behöver toopaste nyckeln i projektet koden inom kort.
   
    ![Klicka på egenskaper i hello nya Resursöversikt och kopiera hello Instrumentation nyckel](./media/app-insights-java-live/03-key.png)

## <a name="2-download-hello-sdk"></a>2. Hämta hello SDK
1. Hämta hello [Application Insights SDK för Java](https://aka.ms/aijavasdk). 
2. Extrahera hello SDK innehållet toohello-katalogen som dina projekt binärfiler har lästs in på servern. Om du använder Tomcat kan katalogen vara under`webapps/<your_app_name>/WEB-INF/lib`

Observera att du behöver toorepeat detta på varje server-instans och för varje app.

## <a name="3-add-an-application-insights-xml-file"></a>3. Lägg till en XML-fil för Application Insights
Skapa ApplicationInsights.xml i hello mappen där du har lagt till hello SDK. Placera i den hello följande XML.

Ersätt hello instrumentation nyckel som du har fått från hello Azure-portalen.

```XML

    <?xml version="1.0" encoding="utf-8"?>
    <ApplicationInsights xmlns="http://schemas.microsoft.com/ApplicationInsights/2013/Settings" schemaVersion="2014-05-30">


      <!-- hello key from hello portal: -->

      <InstrumentationKey>** Your instrumentation key **</InstrumentationKey>


      <!-- HTTP request component (not required for bare API) -->

      <TelemetryModules>
        <Add type="com.microsoft.applicationinsights.web.extensibility.modules.WebRequestTrackingTelemetryModule"/>
        <Add type="com.microsoft.applicationinsights.web.extensibility.modules.WebSessionTrackingTelemetryModule"/>
        <Add type="com.microsoft.applicationinsights.web.extensibility.modules.WebUserTrackingTelemetryModule"/>
      </TelemetryModules>

      <!-- Events correlation (not required for bare API) -->
      <!-- These initializers add context data tooeach event -->

      <TelemetryInitializers>
        <Add   type="com.microsoft.applicationinsights.web.extensibility.initializers.WebOperationIdTelemetryInitializer"/>
        <Add type="com.microsoft.applicationinsights.web.extensibility.initializers.WebOperationNameTelemetryInitializer"/>
        <Add type="com.microsoft.applicationinsights.web.extensibility.initializers.WebSessionTelemetryInitializer"/>
        <Add type="com.microsoft.applicationinsights.web.extensibility.initializers.WebUserTelemetryInitializer"/>
        <Add type="com.microsoft.applicationinsights.web.extensibility.initializers.WebUserAgentTelemetryInitializer"/>

      </TelemetryInitializers>
    </ApplicationInsights>
```

* hello instrumentation nyckel skickas tillsammans med alla element på telemetri och talar om Application Insights toodisplay den i din resurs.
* hello HTTP-begäran-komponenten är valfritt. Telemetri om begäran och svar gånger toohello portal skickas automatiskt.
* Händelser korrelationen är en komponent som tillägg toohello HTTP-begäran. Den tilldelas en identifierare tooeach begäran tas emot av hello-servern och lägger till den här identifieraren som ett objekt med egenskapen tooevery av telemetri som hello egenskapen 'Operation.Id'. Det gör att du toocorrelate hello telemetri som är associerade med varje begäran genom att ange ett filter i [diagnostiska Sök](app-insights-diagnostic-search.md).

## <a name="4-add-an-http-filter"></a>4. Lägga till ett HTTP-filter
Leta upp och öppna hello web.xml filen i projektet och merge hello följande kodavsnittet under hello webbprogrammet nod där programmet filter har konfigurerats.

tooget hello mest korrekta resultat, hello filter ska mappas innan alla filter.

```XML

    <filter>
      <filter-name>ApplicationInsightsWebFilter</filter-name>
      <filter-class>
        com.microsoft.applicationinsights.web.internal.WebRequestTrackingFilter
      </filter-class>
    </filter>
    <filter-mapping>
       <filter-name>ApplicationInsightsWebFilter</filter-name>
       <url-pattern>/*</url-pattern>
    </filter-mapping>
```

## <a name="5-check-firewall-exceptions"></a>5. Kontrollera brandväggsundantag
Du kan behöva för[ange undantag toosend utgående data](app-insights-ip-addresses.md).

## <a name="6-restart-your-web-app"></a>6. Starta om ditt webbprogram
## <a name="7-view-your-telemetry-in-application-insights"></a>7. Visa telemetrin i Application Insights
Returnera tooyour Application Insights-resurs i [Microsoft Azure-portalen](https://portal.azure.com).

Telemetri om HTTP-begäranden som visas på hello översikt bladet. (Om informationen inte visas väntar du några sekunder och klickar på Uppdatera.)

![exempeldata](./media/app-insights-java-live/5-results.png)

Klicka på via alla diagram toosee mer detaljerad mått. 

![](./media/app-insights-java-live/6-barchart.png)

Och när du visar hello egenskaperna för en begäran, kan du visa hello telemetri händelser som är kopplade till den, till exempel begäranden och undantag.

![](./media/app-insights-java-live/7-instance.png)

[Lär dig mer om mätvärden.](app-insights-metrics-explorer.md)

## <a name="next-steps"></a>Nästa steg
* [Lägga till telemetri tooyour webbsidor](app-insights-javascript.md) toomonitor sidan vyer och användaren mått.
* [Ställ in webbtester](app-insights-monitor-web-app-availability.md) toomake säker tillämpningsprogrammet är live och svarstid.
* [Avbilda loggspårningar](app-insights-java-trace-logs.md)
* [Söka efter händelser och loggar](app-insights-diagnostic-search.md) toohelp diagnostisera problem.

