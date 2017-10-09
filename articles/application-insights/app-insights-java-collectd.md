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
# <a name="collectd-linux-performance-metrics-in-application-insights"></a>collectd: Linux prestandamått i Application Insights


Linux tooexplore system prestandamått i [Programinsikter](app-insights-overview.md), installera [collectd](http://collectd.org/), tillsammans med dess plugin Application Insights. Den här lösningen med öppen källkod samlar in olika system- och statistik.

Normalt ska du använda collectd om du redan har [instrumenterats Java webbtjänsten med Application Insights][java]. Den ger dig mer data toohelp du tooenhance appens prestanda eller diagnostisera problem. 

![exempel diagram](./media/app-insights-java-collectd/sample.png)

## <a name="get-your-instrumentation-key"></a>Hämta nyckel för instrumentation
I hello [Microsoft Azure-portalen](https://portal.azure.com)öppnar hello [Programinsikter](app-insights-overview.md) resurs där du vill att hello data tooappear. (Eller [skapar en ny resurs](app-insights-create-new-resource.md).)

Ta en kopia av hello instrumentation nyckel som identifierar hello resurs.

![Bläddra igenom alla öppna resursen och sedan i hello Essentials listrutan, markera och kopiera hello Instrumentation nyckel](./media/app-insights-java-collectd/02-props.png)

## <a name="install-collectd-and-hello-plug-in"></a>Installera collectd och hello plugin-program
På ditt Linux-servrar:

1. Installera [collectd](http://collectd.org/) version 5.4.0 eller senare.
2. Hämta hello [Programinsikter collectd writer plugin](https://aka.ms/aijavasdk). Observera hello versionsnumret.
3. Kopiera hello plugin JAR i `/usr/share/collectd/java`.
4. Redigera `/etc/collectd/collectd.conf`:
   * Se till att [hello Java-plugin-programmet](https://collectd.org/wiki/index.php/Plugin:Java) är aktiverad.
   * Uppdatera hello JVMArg för hello java.class.path tooinclude hello följande JAR. Uppdatera hello version nummer toomatch hello som du hämtade:
   * `/usr/share/collectd/java/applicationinsights-collectd-1.0.5.jar`
   * Lägg till den här fragment med hello Instrumentation nyckeln från din resurs:

```XML

     LoadPlugin "com.microsoft.applicationinsights.collectd.ApplicationInsightsWriter"
     <Plugin ApplicationInsightsWriter>
        InstrumentationKey "Your key"
     </Plugin>
```

Här är en del av en exempelkonfigurationsfilen:

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

Konfigurera andra [collectd plugin-program](https://collectd.org/wiki/index.php/Table_of_Plugins), som kan samla in olika data från olika källor.

Starta om collectd enligt tooits [manuell](https://collectd.org/wiki/index.php/First_steps).

## <a name="view-hello-data-in-application-insights"></a>Visa hello data i Application Insights
Öppna i Application Insights-resurs [Metrics Explorer och Lägg till diagram][metrics], välja hello mått du vill använda toosee från hello anpassad kategori.

![](./media/app-insights-java-collectd/result.png)

Som standard samman hello mått över alla värddatorerna som hello mått har samlats in. tooview hello mått per värd i hello diagram informationsbladet, aktivera gruppering och välj sedan toogroup CollectD-värden.

## <a name="tooexclude-upload-of-specific-statistics"></a>tooexclude överföringen av specifika statistik
Som standard skickar Application Insights-plugin-programmet hello alla hello data som samlas in av alla hello aktiverat collectd ”läsa” plugin-program. 

tooexclude data från specifika plugin-program eller datakällor:

* Redigera hello konfigurationsfilen. 
* I `<Plugin ApplicationInsightsWriter>`, Lägg till direktivet rader så här:

| Direktivet | Verkan |
| --- | --- |
| `Exclude disk` |Undanta alla data som samlas in av hello `disk` plugin-program |
| `Exclude disk:read,write` |Exkludera hello källor med namnet `read` och `write` från hello `disk` plugin-programmet. |

Separata direktiv med en ny rad.

## <a name="problems"></a>Har du problem?
*Data i hello portal visas inte*

* Öppna [Sök] [ diagnostic] toosee om hello rådata händelser har tagits emot. Ibland kan de ta längre tooappear i metrics explorer.
* Du kan behöva för[ange brandväggsundantag för utgående data](app-insights-ip-addresses.md)
* Aktivera spårning i hello Application Insights plugin-programmet. Lägg till följande rad i `<Plugin ApplicationInsightsWriter>`:
  * `SDKLogger true`
* Öppna en terminal och starta collectd i utförligt läge, toosee eventuella problem som den rapporterar:
  * `sudo collectd -f`

## <a name="known-issue"></a>Kända problem

hello Application Insights skriva plugin-programmet är inte kompatibel med vissa Läs plugin-program. Vissa plugin-program att ibland skicka ”NaN” där Application Insights-plugin-programmet hello förväntar sig ett flyttal.

Symptom: hello collectd loggen visar fel som innehåller ”AI:... SyntaxError: Oväntad token N ”.

Lösning: Utesluta data som samlas in av hello problemet skrivåtgärder plugin-program. 

<!--Link references-->

[api]: app-insights-api-custom-events-metrics.md
[apiexceptions]: app-insights-api-custom-events-metrics.md#track-exception
[availability]: app-insights-monitor-web-app-availability.md
[diagnostic]: app-insights-diagnostic-search.md
[eclipse]: app-insights-java-eclipse.md
[java]: app-insights-java-get-started.md
[javalogs]: app-insights-java-trace-logs.md
[metrics]: app-insights-metrics-explorer.md


