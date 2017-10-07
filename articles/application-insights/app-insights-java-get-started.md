---
title: aaaJava web app analytics med Azure Application Insights | Microsoft Docs
description: "Övervakning av programprestanda för Java-webbappar med Application Insights. "
services: application-insights
documentationcenter: java
author: harelbr
manager: carmonm
ms.assetid: 051d4285-f38a-45d8-ad8a-45c3be828d91
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: get-started-article
ms.date: 03/14/2017
ms.author: bwren
ms.openlocfilehash: 6555ee53a44f937350e4fa296080f7dce4f45226
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-application-insights-in-a-java-web-project"></a>Komma igång med Application Insights i ett Java-webbprojekt


[Application Insights](https://azure.microsoft.com/services/application-insights/) är en utökningsbar analytics-tjänsten för webbutvecklare som hjälper dig att förstå hello prestanda och användning av tillämpningsprogrammet live. Använd den för[identifiera och diagnostisera prestandaproblem och undantag](app-insights-detect-triage-diagnose.md), och [skriva kod] [ api] tootrack vad användare göra med din app.

![exempeldata](./media/app-insights-java-get-started/5-results.png)

Application Insights har stöd för Java-appar som körs på Linux, Unix eller Windows.

Du behöver:

* Oracle JRE 1.6 eller senare eller Zulu JRE 1.6 eller senare
* En prenumeration för[Microsoft Azure](https://azure.microsoft.com/).

*Om du har ett webbprogram som redan är live, du kan följa hello alternativa proceduren för[lägga till hello SDK vid körning i hello webbserver](app-insights-java-live.md). Det alternativt undviker att återskapa hello koden, men du får inte hello alternativet toowrite kod tootrack användaraktivitet.*

## <a name="1-get-an-application-insights-instrumentation-key"></a>1. Hämta en Application Insights-instrumenteringsnyckel
1. Logga in toohello [Microsoft Azure-portalen](https://portal.azure.com).
2. Skapa en Application Insights-resurs. Ange hello programmet typen tooJava webbprogram.

    ![Fyll i ett namn, välj Java-webbapp och klicka på Skapa](./media/app-insights-java-get-started/02-create.png)
3. Hitta hello instrumentation nyckeln för hello ny resurs. Du behöver toopaste nyckeln i projektet koden inom kort.

    ![Klicka på egenskaper i hello nya Resursöversikt och kopiera hello Instrumentation nyckel](./media/app-insights-java-get-started/03-key.png)

## <a name="2-add-hello-application-insights-sdk-for-java-tooyour-project"></a>2. Lägg till hello Application Insights SDK för Java tooyour projekt
*Välj hello lämpligt sätt för projektet.*

#### <a name="if-youre-using-eclipse-toocreate-a-maven-or-dynamic-web-project-"></a>Om du använder-solförmörkelse toocreate ett Maven eller dynamisk projekt...
Använd hello [Application Insights SDK för Java-plugin-programmet][eclipse].

#### <a name="if-youre-using-maven"></a>Om du använder Maven …
Om projektet har redan ställts in toouse Maven för version, kod merge hello följande tooyour pom.xml fil.

Sedan uppdatera hello projektet beroenden tooget hello binärfiler hämtas.

```XML

    <repositories>
       <repository>
          <id>central</id>
          <name>Central</name>
          <url>http://repo1.maven.org/maven2</url>
       </repository>
    </repositories>

    <dependencies>
      <dependency>
        <groupId>com.microsoft.azure</groupId>
        <artifactId>applicationinsights-web</artifactId>
        <!-- or applicationinsights-core for bare API -->
        <version>[1.0,)</version>
      </dependency>
    </dependencies>
```

* *Stöter du på utvecklingsfel eller fel relaterade till verifieringen av kontrollsummor?* Prova att använda en specifik version, t.ex.: `<version>1.0.n</version>`. Du hittar hello senaste versionen i hello [SDK viktig information](https://github.com/Microsoft/ApplicationInsights-Java#release-notes) eller i vår [Maven artefakter](http://search.maven.org/#search%7Cga%7C1%7Capplicationinsights).
* *Behöver tooupdate tooa nya SDK?* Uppdatera ditt projekts beroenden.

#### <a name="if-youre-using-gradle"></a>Om du använder Gradle …
Om projektet har redan ställts in toouse Gradle för version, kod merge hello följande tooyour build.gradle fil.

Sedan uppdatera hello projektet beroenden tooget hello binärfiler laddas ned.

```JSON

    repositories {
      mavenCentral()
    }

    dependencies {
      compile group: 'com.microsoft.azure', name: 'applicationinsights-web', version: '1.+'
      // or applicationinsights-core for bare API
    }
```

* *Stöter du på utvecklingsfel eller fel relaterade till verifieringen av kontrollsummor? Prova att använda en specifik version, t.ex.:* `version:'1.0.n'`. *Du hittar hello senaste versionen i hello [SDK viktig information](https://github.com/Microsoft/ApplicationInsights-Java#release-notes).*
* *tooupdate tooa nya SDK*
  * Uppdatera ditt projekts beroenden.

#### <a name="otherwise-"></a>Eller …
Lägg manuellt till hello SDK:

1. Hämta hello [Application Insights SDK för Java](https://aka.ms/aijavasdk).
2. Extrahera hello binärfiler från hello zip-filen och Lägg till dem tooyour projekt.

### <a name="questions"></a>Frågor …
* *Vad är hello förhållandet mellan hello `-core` och `-web` komponenter i hello zip?*

  * `applicationinsights-core`ger du hello utan API. Du behöver alltid ha den här komponenten.
  * `applicationinsights-web` ger dig mått som spårar antalet HTTP-förfrågningar och svarstider. Du kan utelämna den här komponenten om du inte vill att den här telemetrin ska samlas in automatiskt. Till exempel om du vill toowrite egna.
* *tooupdate hello SDK när vi publicerar ändringar*

  * Hämta senaste hello [Application Insights SDK för Java](https://aka.ms/qqkaq6) och Ersätt hello gamla.
  * Beskrivs i hello [SDK viktig information](https://github.com/Microsoft/ApplicationInsights-Java#release-notes).

## <a name="3-add-an-application-insights-xml-file"></a>3. Lägga till en XML-fil för Application Insights
Lägg till mapp som ApplicationInsights.xml toohello resurser i projektet eller kontrollera läggs tooyour projekt klassökvägen för distribution. Kopiera hello följande XML till den.

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
* Händelser korrelationen är en komponent som tillägg toohello HTTP-begäran. Den tilldelas en identifierare tooeach begäran tas emot av hello-servern och lägger till den här identifieraren som ett objekt med egenskapen tooevery av telemetri som hello egenskapen 'Operation.Id'. Det gör att du toocorrelate hello telemetri som är associerade med varje begäran genom att ange ett filter i [diagnostiska Sök][diagnostic].
* hello Application Insights nyckel kan skickas dynamiskt från hello Azure-portalen som en systemegenskap (-DAPPLICATION_INSIGHTS_IKEY = your_ikey). Om det finns inte någon definierad egenskap sker sökning efter miljövariabeln (APPLICATION_INSIGHTS_IKEY) i Azure App-inställningarna. Om båda hello är odefinierad, används hello standard InstrumentationKey från ApplicationInsights.xml. Den här sekvensen hjälper dig att toomanage olika InstrumentationKeys för olika miljöer dynamiskt.

### <a name="alternative-ways-tooset-hello-instrumentation-key"></a>Alternativa sätt tooset hello instrumentation nyckel
Application Insights SDK söker efter hello nyckel i följande ordning:

1. Systemegenskap: -DAPPLICATION_INSIGHTS_IKEY=your_ikey
2. Miljövariabel: APPLICATION_INSIGHTS_IKEY
3. Konfigurationsfil: ApplicationInsights.xml

Du kan också [ange den i koden](app-insights-api-custom-events-metrics.md#ikey):

```Java

    telemetryClient.InstrumentationKey = "...";
```

## <a name="4-add-an-http-filter"></a>4. Lägga till ett HTTP-filter
hello sista konfigurationssteget kan hello HTTP-begäran komponenten toolog varje webbegäran. (Krävs inte om du bara behöver hello utan API.)

Leta upp och öppna hello web.xml filen i projektet och merge hello följande kod under hello webbprogrammet nod där programmet filter har konfigurerats.

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

#### <a name="if-youre-using-spring-web-mvc-31-or-later"></a>Om du använder Spring Web MVC 3.1 or later
Redigera de här elementen i *-servlet.xml tooinclude hello Application Insights paketet:

```XML

    <context:component-scan base-package=" com.springapp.mvc, com.microsoft.applicationinsights.web.spring"/>

    <mvc:interceptors>
        <mvc:interceptor>
            <mvc:mapping path="/**"/>
            <bean class="com.microsoft.applicationinsights.web.spring.RequestNameHandlerInterceptorAdapter" />
        </mvc:interceptor>
    </mvc:interceptors>
```

#### <a name="if-youre-using-struts-2"></a>Om du använder Struts 2
Lägg till den här artikeln toohello andra ojämnheter konfigurationsfilen (vanligtvis namngivna struts.xml eller andra ojämnheter default.xml):

```XML

     <interceptors>
       <interceptor name="ApplicationInsightsRequestNameInterceptor" class="com.microsoft.applicationinsights.web.struts.RequestNameInterceptor" />
     </interceptors>
     <default-interceptor-ref name="ApplicationInsightsRequestNameInterceptor" />
```

(Om du har interceptorerna som definierats i en standard-stack hello spärren kan bara läggas toothat stack)

## <a name="5-run-your-application"></a>5. Köra ditt program
Kör det i felsökningsläge på utvecklingsdatorn eller publicera tooyour server.

## <a name="6-view-your-telemetry-in-application-insights"></a>6. Visa telemetrin i Application Insights
Returnera tooyour Application Insights-resurs i [Microsoft Azure-portalen](https://portal.azure.com).

HTTP-begäranden data visas på hello översikt bladet. (Om informationen inte visas väntar du några sekunder och klickar på Uppdatera.)

![exempeldata](./media/app-insights-java-get-started/5-results.png)

[Lär dig mer om mätvärden.][metrics]

Klicka dig igenom några diagram toosee mer detaljerad samman mått.

![](./media/app-insights-java-get-started/6-barchart.png)

> Application Insights förutsätter hello format för HTTP-begäranden för MVC-program: `VERB controller/action`. `GET Home/Product/f9anuh81`, `GET Home/Product/2dffwrf5` och `GET Home/Product/sdf96vws` grupperas t.ex. i `GET Home/Product`. Denna gruppering gör det möjligt att skapa meningsfulla förfrågningsaggregeringar, t.ex. antal förfrågningar och genomsnittlig körningstid för förfrågningarna.
>
>

### <a name="instance-data"></a>Instansdata
Klicka dig igenom en specifik begäran Skriv toosee enskilda instanser.

Två typer av data visas i Application Insights: aggregerade data, som lagras och visas som medelvärden, antal och belopp; och instansdata, som är enskilda rapporter över HTTP-begäranden, undantag, sidvisningar eller anpassade händelser.

När du visar hello egenskaperna för en begäran kan se du hello telemetriska händelser som är kopplade till den, till exempel begäranden och undantag.

![](./media/app-insights-java-get-started/7-instance.png)

### <a name="analytics-powerful-query-language"></a>Analytics: Kraftfullt frågespråk
När du samlar in mer data, kan du köra frågor båda tooaggregate data och toofind enskilda instanser.  [Analytics](app-insights-analytics.md) är ett kraftfullt verktyg både för att bättre förstå prestanda och användning, och för diagnostikändamål.

![Exempel med Analytics](./media/app-insights-java-get-started/025.png)

## <a name="7-install-your-app-on-hello-server"></a>7. Installera appen på hello-server
Nu publicera din app toohello server, låta personer använder den, och titta på hello telemetri som visas på hello-portalen.

* Kontrollera att brandväggen tillåter programmet-toosend telemetri toothese portar:

  * dc.services.visualstudio.com:443
  * f5.services.visualstudio.com:443

* Om utgående trafik måste dirigeras via en brandvägg definierar du systemegenskaper `http.proxyHost` och `http.proxyPort`.

* Installera följande på Windows-servrar:

  * [Microsoft Visual C++ Redistributable](http://www.microsoft.com/download/details.aspx?id=40784)

    (Den här komponenten gör det möjligt att använda prestandaräknare.)


## <a name="exceptions-and-request-failures"></a>Fel relaterade till begäranden och undantag
Ohanterade undantag samlas in automatiskt:

![Öppna Inställningar, Fel](./media/app-insights-java-get-started/21-exceptions.png)

toocollect data på andra undantag, har du två alternativ:

* [Infoga anropar tootrackException() i koden][apiexceptions].
* [Installera hello Java-agenten på servern](app-insights-java-agent.md). Du kan ange hello-metoder som du vill toowatch.

## <a name="monitor-method-calls-and-external-dependencies"></a>Övervaka metodanrop och externa beroenden
[Installera hello agenten Java](app-insights-java-agent.md) toolog angivna interna metoder och anrop som görs via JDBC, med tidsinställning data.

## <a name="performance-counters"></a>Prestandaräknare
Öppna **inställningar**, **servrar**, toosee ett antal prestandaräknare.

![](./media/app-insights-java-get-started/11-perf-counters.png)

### <a name="customize-performance-counter-collection"></a>Anpassa samlingen med prestandaräknare
toodisable samling hello standarduppsättning prestandaräknare, lägger du till följande kod under hello rotnoden hello ApplicationInsights.xml filen hello:

```XML
    <PerformanceCounters>
       <UseBuiltIn>False</UseBuiltIn>
    </PerformanceCounters>
```

### <a name="collect-additional-performance-counters"></a>Samla in data från fler prestandaräknare
Du kan ange ytterligare prestandaräknare toobe samlas in.

#### <a name="jmx-counters-exposed-by-hello-java-virtual-machine"></a>Räknare för JMX (som exponeras av hello Java Virtual Machine)

```XML
    <PerformanceCounters>
      <Jmx>
        <Add objectName="java.lang:type=ClassLoading" attribute="TotalLoadedClassCount" displayName="Loaded Class Count"/>
        <Add objectName="java.lang:type=Memory" attribute="HeapMemoryUsage.used" displayName="Heap Memory Usage-used" type="composite"/>
      </Jmx>
    </PerformanceCounters>
```

* `displayName`– hello-namnet som visas i hello Application Insights-portalen.
* `objectName`– hello JMX-objektnamn.
* `attribute`– hello-attributet för hello JMX objektet namn toofetch
* `type`(valfritt) – hello JMX objektets attributtyp:
  * Standardvärde: en enkel typ, till exempel int eller long.
  * `composite`: hello prestandaräknardata har formatet hello av 'Attribute.Data'
  * `tabular`: hello prestandaräknardata har hello format för en tabellrad

#### <a name="windows-performance-counters"></a>Windows-prestandaräknare
Varje [Windows-prestandaräknare](https://msdn.microsoft.com/library/windows/desktop/aa373083.aspx) är medlem i en kategori (i hello samma sätt som att ett fält är medlem av en klass). Kategorier kan antingen vara globala eller ha numrerade eller namngivna instanser.

```XML
    <PerformanceCounters>
      <Windows>
        <Add displayName="Process User Time" categoryName="Process" counterName="%User Time" instanceName="__SELF__" />
        <Add displayName="Bytes Printed per Second" categoryName="Print Queue" counterName="Bytes Printed/sec" instanceName="Fax" />
      </Windows>
    </PerformanceCounters>
```

* displayName – hello namnet visas i hello Application Insights-portalen.
* Kategorinamn – hello prestandaräknarkategorin (prestandaobjekt) som är kopplad till den här prestandaräknaren.
* counterName – hello namn för hello prestandaräknare.
* Instansnamn – hello kategori prestandaräknarinstans hello namn eller en tom sträng (””), om hello kategori innehåller en enda instans. Om hello categoryName är processen och hello prestandaräknaren som toocollect är från hello aktuella JVM-processen på som din app körs, ange `"__SELF__"`.

Dina prestandaräknare visas som anpassade mått i [Metrics Explorer][metrics].

![](./media/app-insights-java-get-started/12-custom-perfs.png)

### <a name="unix-performance-counters"></a>Unix-prestandaräknare
* [Installera collectd med Application Insights-plugin-programmet hello](app-insights-java-collectd.md) tooget en mängd olika system- och data.

## <a name="get-user-and-session-data"></a>Samla in användar- och sesionsdata
Du skickar telemetri från webbservern. Nu hello tooget 360 graders vy av programmet, du kan lägga till fler övervakning:

* [Lägga till webbsidor för telemetri tooyour] [ usage] toomonitor sidan vyer och användaren mått.
* [Ställ in webbtester] [ availability] toomake säker tillämpningsprogrammet är live och svarstid.

## <a name="capture-log-traces"></a>Samla in loggspårningar
Du kan använda Application Insights tooslice och undersök loggar från Log4J eller Logback andra ramverk för loggning. Du kan jämföra hello loggar med HTTP-begäranden och andra telemetri. [Lär dig mer][javalogs].

## <a name="send-your-own-telemetry"></a>Skicka din egen telemetri
Nu när du har installerat hello SDK kan använda du hello API toosend egna telemetri.

* [Spåra anpassade händelser och mått] [ api] toolearn vad användarna gör med ditt program.
* [Söka efter händelser och loggar] [ diagnostic] toohelp diagnostisera problem.

## <a name="availability-web-tests"></a>Webbtester för tillgänglighet
Application Insights kan testa din webbplats på regelbundet toocheck som är det upp och svarar också. [tooset in][availability], klicka på webbtester.

![Klicka först på Webbtester och sedan på Lägg till webbtest](./media/app-insights-java-get-started/31-config-web-test.png)

Du kan visa diagram över svarstider, samt få e-postaviseringar om platsen kraschar.

![Exempel på webbtest](./media/app-insights-java-get-started/appinsights-10webtestresult.png)

[Läs mer om webbtester för tillgänglighet.][availability]

## <a name="questions-problems"></a>Frågor? Har du problem?
[Felsöka Java](app-insights-java-troubleshoot.md)

## <a name="video"></a>Video

> [!VIDEO https://channel9.msdn.com/events/Connect/2016/100/player]

## <a name="next-steps"></a>Nästa steg
* [Övervaka beroende anrop](app-insights-java-agent.md)
* [Övervaka Unix-prestandaräknare](app-insights-java-collectd.md)
* Lägg till [övervakning tooyour webbsidor](app-insights-javascript.md) toomonitor sidinläsning gånger AJAX-anrop, webbläsarundantag.
* Skriva [telemetri om anpassade](app-insights-api-custom-events-metrics.md) tootrack användning i hello webbläsare eller hello-servern.
* Skapa [instrumentpaneler](app-insights-dashboards.md) toobring tillsammans hello viktiga diagram för övervakning av systemet.
* Använd [Analytics](app-insights-analytics.md) för kraftfulla frågor via telemetri från din app
* Mer information finns i [Azure för Java-utvecklare](/java/azure).

<!--Link references-->

[api]: app-insights-api-custom-events-metrics.md
[apiexceptions]: app-insights-api-custom-events-metrics.md#trackexception
[availability]: app-insights-monitor-web-app-availability.md
[diagnostic]: app-insights-diagnostic-search.md
[eclipse]: app-insights-java-eclipse.md
[javalogs]: app-insights-java-trace-logs.md
[metrics]: app-insights-metrics-explorer.md
[usage]: app-insights-javascript.md
