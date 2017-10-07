---
title: "aaaGet igång med Azure Application Insights med Java i Eclipse | Microsoft docs"
description: "Använd hello Eclipse plugin tooadd prestanda och övervakning tooyour Java-webbplats med Application Insights"
services: application-insights
documentationcenter: java
author: CFreemanwa
manager: carmonm
ms.assetid: e88c9f53-cd90-4abc-b097-1f170937908e
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 12/12/2016
ms.author: bwren
ms.openlocfilehash: 3142a26a9e2d14c2c433882e3d337f2a8c8f2247
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-application-insights-with-java-in-eclipse"></a>Kom igång med Application Insights med Java i Eclipse
hello Application Insights SDK skickar telemetri från ditt webbprogram för Java så att du kan analysera användnings- och prestandadata. hello Eclipse plugin-program för Application Insights installeras automatiskt hello SDK i projektet så att du får slut hello rutan telemetri plus en API som du kan använda toowrite anpassad telemetri.   

## <a name="prerequisites"></a>Krav
Plugin-programmet hello fungerar för närvarande för Maven-projekt och dynamiskt webbprojekt i Eclipse.
([Lägg till Application Insights tooother typer av Java-projekt][java].)

Du behöver:

* Oracle JRE 1.6 eller senare
* En prenumeration för[Microsoft Azure](https://azure.microsoft.com/).
* [Solförmörkelse IDE för Java EE-utvecklare](http://www.eclipse.org/downloads/), Indigo eller senare.
* Windows 7 eller senare och Windows Server 2008 eller senare

## <a name="install-hello-sdk-on-eclipse-one-time"></a>Installera hello SDK på Eclipse (en gång)
Du har bara toodo en gång per dator. Det här steget installerar en verktygslåda som sedan kan lägga till hello SDK tooeach dynamiskt webbprojekt.

1. Klicka på Hjälp, installera ny programvara i Eclipse.

    ![Hjälp, installera ny programvara](./media/app-insights-java-eclipse/0-plugin.png)
2. hello SDK finns i http://dl.microsoft.com/eclipse under Azure Toolkit.
3. Avmarkera **kontakta alla update platser...**

    ![För Application Insights SDK, rensar kontakta uppdatera alla platser](./media/app-insights-java-eclipse/1-plugin.png)

Följ hello återstående steg för varje Java-projekt.

## <a name="create-an-application-insights-resource-in-azure"></a>Skapa en Application Insights-resurs i Azure
1. Logga in toohello [Azure-portalen](https://portal.azure.com).
2. Skapa en ny Application Insights-resurs. Ange hello programmet typen tooJava webbprogram.  

    ![Klicka på + och välj Application Insights](./media/app-insights-java-eclipse/01-create.png)  

4. Hitta hello instrumentation nyckeln för hello ny resurs. Du behöver toopaste den i projektet koden inom kort.  

    ![Klicka på egenskaper i hello nya Resursöversikt och kopiera hello Instrumentation nyckel](./media/app-insights-java-eclipse/03-key.png)  

## <a name="add-application-insights-tooyour-project"></a>Lägga till Application Insights tooyour projekt
1. Lägg till Application Insights hello snabbmenyn för webbprojektet Java.

    ![Klicka på egenskaper i hello nya Resursöversikt och kopiera hello Instrumentation nyckel](./media/app-insights-java-eclipse/02-context-menu.png)
2. Klistra in hello instrumentation nyckel som du har fått från hello Azure-portalen.

    ![Klicka på egenskaper i hello nya Resursöversikt och kopiera hello Instrumentation nyckel](./media/app-insights-java-eclipse/03-ikey.png)

hello nyckel skickas tillsammans med alla element på telemetri och talar om Application Insights toodisplay den i din resurs.

## <a name="run-hello-application-and-see-metrics"></a>Kör programmet hello och se mått
Kör ditt program.

Returnera tooyour Application Insights-resurs i Microsoft Azure.

HTTP-begäranden data visas på hello översikt bladet. (Om informationen inte visas väntar du några sekunder och klickar på Uppdatera.)

![Svaret från servern och begäran antal fel ](./media/app-insights-java-eclipse/5-results.png)

Klicka på via alla diagram toosee mer detaljerad mått.

![Begär antal efter namn](./media/app-insights-java-eclipse/6-barchart.png)

[Lär dig mer om mätvärden.][metrics]

Och när du visar hello egenskaperna för en begäran, kan du visa hello telemetri händelser som är kopplade till den, till exempel begäranden och undantag.

![Alla spårningar för den här begäran](./media/app-insights-java-eclipse/7-instance.png)

## <a name="client-side-telemetry"></a>Telemetri för klientsidan
Hello Snabbstart-bladet, klicka på Hämta koden toomonitor webbsidor:

![Välj Snabbstart, hämta koden toomonitor webbsidor på bladet översikt över din app. Kopiera hello skript.](./media/app-insights-java-eclipse/02-monitor-web-page.png)

Infoga hello kodstycke i hello huvud HTML-filer.

#### <a name="view-client-side-data"></a>Visa data för klientsidan
Öppna din uppdaterade webbsidor och använder dem. Vänta en minut eller två och återgå sedan tooApplication insikter och öppna hello användning bladet. (Hello översikt bladet rulla nedåt och klicka på användning.)

Sidan Visa, användare och session mått visas på bladet för användning av hello:

![Sessioner, användare och sidvyer](./media/app-insights-java-eclipse/appinsights-47usage-2.png)

[Mer information om hur du konfigurerar telemetri för klientsidan.][usage]

## <a name="publish-your-application"></a>Publicera programmet
Nu publicera din app toohello server, låta personer använder den, och titta på hello telemetri som visas på hello-portalen.

* Kontrollera att brandväggen tillåter programmet-toosend telemetri toothese portar:

  * dc.services.visualstudio.com:443
  * dc.services.visualstudio.com:80
  * f5.services.visualstudio.com:443
  * f5.services.visualstudio.com:80
* Installera följande på Windows-servrar:

  * [Microsoft Visual C++ Redistributable](http://www.microsoft.com/download/details.aspx?id=40784)

    (Gör att du kan använda prestandaräknare.)

## <a name="exceptions-and-request-failures"></a>Fel relaterade till begäranden och undantag
Ohanterade undantag samlas in automatiskt:

![](./media/app-insights-java-eclipse/21-exceptions.png)

toocollect data på andra undantag, har du två alternativ:

* [Infoga anropar tooTrackException i koden](app-insights-api-custom-events-metrics.md#trackexception).
* [Installera hello Java-agenten på servern](app-insights-java-agent.md). Du kan ange hello-metoder som du vill toowatch.

## <a name="monitor-method-calls-and-external-dependencies"></a>Övervaka metodanrop och externa beroenden
[Installera hello agenten Java](app-insights-java-agent.md) toolog angivna interna metoder och anrop som görs via JDBC, med tidsinställning data.

## <a name="performance-counters"></a>Prestandaräknare
Rulla nedåt på Översikt-bladet och klicka på hello **servrar** panelen. Ser du ett antal prestandaräknare.

![Rulla nedåt tooclick hello servrar sida vid sida](./media/app-insights-java-eclipse/11-perf-counters.png)

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

![](./media/app-insights-java-eclipse/12-custom-perfs.png)

### <a name="unix-performance-counters"></a>Unix-prestandaräknare
* [Installera collectd med Application Insights-plugin-programmet hello](app-insights-java-collectd.md) tooget en mängd olika system- och data.

## <a name="availability-web-tests"></a>Webbtester för tillgänglighet
Application Insights kan testa din webbplats på regelbundet toocheck som är det upp och svarar också. [tooset in][availability], bläddra nedåt tooclick tillgänglighet.

![Rulla ned, klicka på Tillgänglighet och sedan på Lägg till webbtest.](./media/app-insights-java-eclipse/31-config-web-test.png)

Du kan visa diagram över svarstider, samt få e-postaviseringar om platsen kraschar.

![Exempel på webbtest](./media/app-insights-java-eclipse/appinsights-10webtestresult.png)

[Läs mer om webbtester för tillgänglighet.][availability]

## <a name="diagnostic-logs"></a>Diagnostikloggar
Om du använder Logback eller Log4J (version 1.2 eller v2.0) för spårning, kan du ha spårloggarna skickas automatiskt tooApplication insikter där du kan utforska och söka i dem.

[Mer information om diagnostikloggar][javalogs]

## <a name="custom-telemetry"></a>Anpassad telemetri
Infoga ett fåtal rader med kod i Java web application toofind reda på vad användarna gör med den eller toohelp diagnostisera problem.

Du kan infoga kod både webbsida JavaScript och hello serversidan Java.

[Lär dig mer om anpassad telemetri][track]

## <a name="next-steps"></a>Nästa steg
#### <a name="detect-and-diagnose-issues"></a>Identifiera och diagnostisera problem
* [Lägg till webbplats klienten telemetri] [ usage] tooget prestanda telemetri från hello webbklienten.
* [Ställ in webbtester] [ availability] toomake säker tillämpningsprogrammet är live och svarstid.
* [Söka efter händelser och loggar] [ diagnostic] toohelp diagnostisera problem.
* [Fånga in spårningar Log4J eller Logback][javalogs]

#### <a name="track-usage"></a>Spåra användning
* [Lägg till webbplats klienten telemetri] [ usage] toomonitor sidan vyer och grundläggande mått.
* [Spåra anpassade händelser och mått](app-insights-web-track-usage.md) toolearn om hur programmet används både på hello klient- och hello.

<!--Link references-->

[availability]: app-insights-monitor-web-app-availability.md
[diagnostic]: app-insights-diagnostic-search.md
[java]: app-insights-java-get-started.md
[javalogs]: app-insights-java-trace-logs.md
[metrics]: app-insights-metrics-explorer.md
[track]: app-insights-api-custom-events-metrics.md
[usage]: app-insights-javascript.md
