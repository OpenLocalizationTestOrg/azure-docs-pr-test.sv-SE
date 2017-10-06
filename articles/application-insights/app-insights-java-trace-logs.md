---
title: aaaExplore Java trace loggar i Azure Application Insights | Microsoft Docs
description: "Sök Log4J eller Logback spåren i Application Insights"
services: application-insights
documentationcenter: java
author: CFreemanwa
manager: carmonm
ms.assetid: fc0a9e2f-3beb-4f47-a9fe-3f86cd29d97a
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 12/12/2016
ms.author: bwren
ms.openlocfilehash: e5f8e8c67e57753ba7574b97aa96dbb41db00ce1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="explore-java-trace-logs-in-application-insights"></a>Utforska Java spårningsloggar i Application Insights
Om du använder Logback eller Log4J (version 1.2 eller v2.0) för spårning, kan du ha spårloggarna skickas automatiskt tooApplication insikter där du kan utforska och söka i dem.

## <a name="install-hello-java-sdk"></a>Installera hello Java SDK

Installera [Application Insights SDK för Java][java], om du inte redan har gjort som.

(Om du inte vill tootrack HTTP-begäranden, kan du utelämna de flesta av hello XML-konfigurationsfil, men du måste åtminstone innehålla hello `InstrumentationKey` element. Du bör också kontakta `new TelemetryClient()` tooinitialize hello SDK.)


## <a name="add-logging-libraries-tooyour-project"></a>Lägga till loggning bibliotek tooyour projekt
*Välj hello lämpligt sätt för projektet.*

#### <a name="if-youre-using-maven"></a>Om du använder Maven …
Om projektet har redan ställts in toouse Maven för version, koppla ett hello följande kodavsnitt i koden i filen pom.xml.

Uppdatera sedan hello projektberoenden, tooget hello binärfiler som hämtats.

*Logback*

```XML

    <dependencies>
       <dependency>
          <groupId>com.microsoft.azure</groupId>
          <artifactId>applicationinsights-logging-logback</artifactId>
          <version>[1.0,)</version>
       </dependency>
    </dependencies>
```

*Log4J v2.0*

```XML

    <dependencies>
       <dependency>
          <groupId>com.microsoft.azure</groupId>
          <artifactId>applicationinsights-logging-log4j2</artifactId>
          <version>[1.0,)</version>
       </dependency>
    </dependencies>
```

*Log4J version 1.2*

```XML

    <dependencies>
       <dependency>
          <groupId>com.microsoft.azure</groupId>
          <artifactId>applicationinsights-logging-log4j1_2</artifactId>
          <version>[1.0,)</version>
       </dependency>
    </dependencies>
```

#### <a name="if-youre-using-gradle"></a>Om du använder Gradle …
Om projektet har redan ställts in toouse Gradle för version, lägga till en av följande rader toohello hello `dependencies` i filen build.gradle:

Uppdatera sedan hello projektberoenden, tooget hello binärfiler som hämtats.

**Logback**

```

    compile group: 'com.microsoft.azure', name: 'applicationinsights-logging-logback', version: '1.0.+'
```

**Log4J v2.0**

```
    compile group: 'com.microsoft.azure', name: 'applicationinsights-logging-log4j2', version: '1.0.+'
```

**Log4J version 1.2**

```
    compile group: 'com.microsoft.azure', name: 'applicationinsights-logging-log4j1_2', version: '1.0.+'
```

#### <a name="otherwise-"></a>Eller …
Ladda ned och extrahera hello lämpliga loggbilaga och lägger till hello bibliotek tooyour projektet:

| Loggaren | Ladda ned | Bibliotek |
| --- | --- | --- |
| Logback |[SDK med Logback loggbilaga](https://aka.ms/xt62a4) |logback-applicationinsights-loggning |
| Log4J v2.0 |[SDK Log4J v2 loggbilaga](https://aka.ms/qypznq) |log4j2-applicationinsights-loggning |
| Log4j version 1.2 |[SDK Log4J version 1.2 loggbilaga](https://aka.ms/ky9cbo) |log4j1_2-applicationinsights-loggning |

## <a name="add-hello-appender-tooyour-logging-framework"></a>Lägg till hello loggbilaga tooyour loggningsramverk
toostart komma spårningar, merge hello relevanta kodavsnittet kod toohello Log4J eller Logback konfigurationsfilen: 

*Logback*

```XML

    <appender name="aiAppender" 
      class="com.microsoft.applicationinsights.logback.ApplicationInsightsAppender">
    </appender>
    <root level="trace">
      <appender-ref ref="aiAppender" />
    </root>
```

*Log4J v2.0*

```XML

    <Configuration packages="com.microsoft.applicationinsights.Log4j">
      <Appenders>
        <ApplicationInsightsAppender name="aiAppender" />
      </Appenders>
      <Loggers>
        <Root level="trace">
          <AppenderRef ref="aiAppender"/>
        </Root>
      </Loggers>
    </Configuration>
```

*Log4J version 1.2*

```XML

    <appender name="aiAppender" 
         class="com.microsoft.applicationinsights.log4j.v1_2.ApplicationInsightsAppender">
    </appender>
    <root>
      <priority value ="trace" />
      <appender-ref ref="aiAppender" />
    </root>
```

hello Application Insights appenders kan refereras genom att alla angivna loggaren och inte nödvändigtvis hello rot loggaren (som visas i hello-kodexempel ovan).

## <a name="explore-your-traces-in-hello-application-insights-portal"></a>Utforska dina spår i hello Application Insights-portalen
Nu när du har konfigurerat dina projekt toosend spårar tooApplication insikter, kan du visa och söka efter de i hello Application Insights-portalen i hello [Sök] [ diagnostic] bladet.

![Öppna Sök i hello Application Insights-portalen](./media/app-insights-java-trace-logs/10-diagnostics.png)

## <a name="next-steps"></a>Nästa steg
[Diagnostiska sökning][diagnostic]

<!--Link references-->

[diagnostic]: app-insights-diagnostic-search.md
[java]: app-insights-java-get-started.md


