---
title: "aaaPerformance övervakning för Java-webbappar i Azure Application Insights | Microsoft Docs"
description: "Utökad prestanda och användning övervakning av Java-webbplats med Application Insights."
services: application-insights
documentationcenter: java
author: harelbr
manager: carmonm
ms.assetid: 84017a48-1cb3-40c8-aab1-ff68d65e2128
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 08/24/2016
ms.author: bwren
ms.openlocfilehash: bf3983e3b4a16e72bc606b6468a757288d05ebaa
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="monitor-dependencies-exceptions-and-execution-times-in-java-web-apps"></a>Övervaka beroenden, undantag och körningstider i Java-webbappar


Om du har [instrumenterats Java-webbappen med Application Insights][java], du kan använda hello agenten Java tooget djupare insikter, utan några ändringar i koden:

* **Beroenden:** Data om anrop som programmet gör tooother komponenter, inklusive:
  * **REST-anrop** görs via HttpClient, OkHttp och RestTemplate (källan).
  * **Redis** anrop som görs via hello Jedis klienten. Om hello anropet tar längre än 10-tal hämtar hello agent också hello anropet argument.
  * **[JDBC-anrop](http://docs.oracle.com/javase/7/docs/technotes/guides/jdbc/)**  -MySQL, SQL Server, PostgreSQL, SQLite, Oracle-databas eller Apache exempel DB. ”executeBatch” samtal stöds. För MySQL och PostgreSQL om hello anropet tar längre tid än 10-tal, rapporterar hello agent hello frågeplanen.
* **Undantag som fångats:** Data om undantag som hanteras av din kod.
* **Metoden körningstid:** Data om hello tid det tar tooexecute specifika metoder.

toouse hello Java-agent du installera det på servern. Dina webbprogram måste vara försett med hello [Application Insights SDK för Java][java]. 

## <a name="install-hello-application-insights-agent-for-java"></a>Installera hello Application Insights-agenten för Java
1. Hello datorn kör din Java-server [ladda ned hello agent](https://aka.ms/aijavasdk).
2. Redigera hello programskript server start och Lägg till följande JVM hello:
   
    `javaagent:`*fullständig sökväg toohello agent JAR-filen*
   
    Till exempel i Tomcat på en Linux-dator:
   
    `export JAVA_OPTS="$JAVA_OPTS -javaagent:<full path tooagent JAR file>"`
3. Starta om programservern.

## <a name="configure-hello-agent"></a>Konfigurera hello-agent
Skapa en fil med namnet `AI-Agent.xml` och placera det i hello samma mapp som hello agent JAR-filen.

Ange hello innehållet hello XML-filen. Redigera hello följande exempel tooinclude eller utelämna hello-funktioner som du vill ha.

```XML

    <?xml version="1.0" encoding="utf-8"?>
    <ApplicationInsightsAgent>
      <Instrumentation>

        <!-- Collect remote dependency data -->
        <BuiltIn enabled="true">
           <!-- Disable Redis or alter threshold call duration above which arguments are sent.
               Defaults: enabled, 10000 ms -->
           <Jedis enabled="true" thresholdInMS="1000"/>

           <!-- Set SQL query duration above which query plan is reported (MySQL, PostgreSQL). Default is 10000 ms. -->
           <MaxStatementQueryLimitInMS>1000</MaxStatementQueryLimitInMS>
        </BuiltIn>

        <!-- Collect data about caught exceptions
             and method execution times -->

        <Class name="com.myCompany.MyClass">
           <Method name="methodOne"
               reportCaughtExceptions="true"
               reportExecutionTime="true"
               />

           <!-- Report on hello particular signature
                void methodTwo(String, int) -->
           <Method name="methodTwo"
              reportExecutionTime="true"
              signature="(Ljava/lang/String;I)V" />
        </Class>

      </Instrumentation>
    </ApplicationInsightsAgent>

```

Du har tooenable rapporter undantag och metoden tidpunkten för enskilda metoder.

Som standard `reportExecutionTime` är true och `reportCaughtExceptions` är false.

## <a name="view-hello-data"></a>Visa hello data
Sammanställda remote beroende- och metoden körningstider visas i hello Application Insights-resurs, [under hello prestanda panelen][metrics].

toosearch för enskilda instanser av beroende, undantag och metoden rapporter, öppna [Sök][diagnostic].

[Diagnostisera problem med beroende - Läs mer](app-insights-asp-net-dependencies.md#diagnosis).

## <a name="questions-problems"></a>Frågor? Har du problem?
* Ser du inga data? [Ange undantag för brandväggen](app-insights-ip-addresses.md)
* [Felsöka Java](app-insights-java-troubleshoot.md)

<!--Link references-->

[api]: app-insights-api-custom-events-metrics.md
[apiexceptions]: app-insights-api-custom-events-metrics.md#track-exception
[availability]: app-insights-monitor-web-app-availability.md
[diagnostic]: app-insights-diagnostic-search.md
[eclipse]: app-insights-java-eclipse.md
[java]: app-insights-java-get-started.md
[javalogs]: app-insights-java-trace-logs.md
[metrics]: app-insights-metrics-explorer.md
