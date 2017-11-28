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
# <a name="monitor-dependencies-exceptions-and-execution-times-in-java-web-apps"></a><span data-ttu-id="5e0c2-103">Övervaka beroenden, undantag och körningstider i Java-webbappar</span><span class="sxs-lookup"><span data-stu-id="5e0c2-103">Monitor dependencies, exceptions and execution times in Java web apps</span></span>


<span data-ttu-id="5e0c2-104">Om du har [instrumenterats Java-webbappen med Application Insights][java], du kan använda hello agenten Java tooget djupare insikter, utan några ändringar i koden:</span><span class="sxs-lookup"><span data-stu-id="5e0c2-104">If you have [instrumented your Java web app with Application Insights][java], you can use hello Java Agent tooget deeper insights, without any code changes:</span></span>

* <span data-ttu-id="5e0c2-105">**Beroenden:** Data om anrop som programmet gör tooother komponenter, inklusive:</span><span class="sxs-lookup"><span data-stu-id="5e0c2-105">**Dependencies:** Data about calls that your application makes tooother components, including:</span></span>
  * <span data-ttu-id="5e0c2-106">**REST-anrop** görs via HttpClient, OkHttp och RestTemplate (källan).</span><span class="sxs-lookup"><span data-stu-id="5e0c2-106">**REST calls** made via HttpClient, OkHttp, and RestTemplate (Spring).</span></span>
  * <span data-ttu-id="5e0c2-107">**Redis** anrop som görs via hello Jedis klienten.</span><span class="sxs-lookup"><span data-stu-id="5e0c2-107">**Redis** calls made via hello Jedis client.</span></span> <span data-ttu-id="5e0c2-108">Om hello anropet tar längre än 10-tal hämtar hello agent också hello anropet argument.</span><span class="sxs-lookup"><span data-stu-id="5e0c2-108">If hello call takes longer than 10s, hello agent also fetches hello call arguments.</span></span>
  * <span data-ttu-id="5e0c2-109">**[JDBC-anrop](http://docs.oracle.com/javase/7/docs/technotes/guides/jdbc/)**  -MySQL, SQL Server, PostgreSQL, SQLite, Oracle-databas eller Apache exempel DB.</span><span class="sxs-lookup"><span data-stu-id="5e0c2-109">**[JDBC calls](http://docs.oracle.com/javase/7/docs/technotes/guides/jdbc/)** - MySQL, SQL Server, PostgreSQL, SQLite, Oracle DB or Apache Derby DB.</span></span> <span data-ttu-id="5e0c2-110">”executeBatch” samtal stöds.</span><span class="sxs-lookup"><span data-stu-id="5e0c2-110">"executeBatch" calls are supported.</span></span> <span data-ttu-id="5e0c2-111">För MySQL och PostgreSQL om hello anropet tar längre tid än 10-tal, rapporterar hello agent hello frågeplanen.</span><span class="sxs-lookup"><span data-stu-id="5e0c2-111">For MySQL and PostgreSQL, if hello call takes longer than 10s, hello agent reports hello query plan.</span></span>
* <span data-ttu-id="5e0c2-112">**Undantag som fångats:** Data om undantag som hanteras av din kod.</span><span class="sxs-lookup"><span data-stu-id="5e0c2-112">**Caught exceptions:** Data about exceptions that are handled by your code.</span></span>
* <span data-ttu-id="5e0c2-113">**Metoden körningstid:** Data om hello tid det tar tooexecute specifika metoder.</span><span class="sxs-lookup"><span data-stu-id="5e0c2-113">**Method execution time:** Data about hello time it takes tooexecute specific methods.</span></span>

<span data-ttu-id="5e0c2-114">toouse hello Java-agent du installera det på servern.</span><span class="sxs-lookup"><span data-stu-id="5e0c2-114">toouse hello Java agent, you install it on your server.</span></span> <span data-ttu-id="5e0c2-115">Dina webbprogram måste vara försett med hello [Application Insights SDK för Java][java].</span><span class="sxs-lookup"><span data-stu-id="5e0c2-115">Your web apps must be instrumented with hello [Application Insights Java SDK][java].</span></span> 

## <a name="install-hello-application-insights-agent-for-java"></a><span data-ttu-id="5e0c2-116">Installera hello Application Insights-agenten för Java</span><span class="sxs-lookup"><span data-stu-id="5e0c2-116">Install hello Application Insights agent for Java</span></span>
1. <span data-ttu-id="5e0c2-117">Hello datorn kör din Java-server [ladda ned hello agent](https://aka.ms/aijavasdk).</span><span class="sxs-lookup"><span data-stu-id="5e0c2-117">On hello machine running your Java server, [download hello agent](https://aka.ms/aijavasdk).</span></span>
2. <span data-ttu-id="5e0c2-118">Redigera hello programskript server start och Lägg till följande JVM hello:</span><span class="sxs-lookup"><span data-stu-id="5e0c2-118">Edit hello application server startup script, and add hello following JVM:</span></span>
   
    <span data-ttu-id="5e0c2-119">`javaagent:`*fullständig sökväg toohello agent JAR-filen*</span><span class="sxs-lookup"><span data-stu-id="5e0c2-119">`javaagent:`*full path toohello agent JAR file*</span></span>
   
    <span data-ttu-id="5e0c2-120">Till exempel i Tomcat på en Linux-dator:</span><span class="sxs-lookup"><span data-stu-id="5e0c2-120">For example, in Tomcat on a Linux machine:</span></span>
   
    `export JAVA_OPTS="$JAVA_OPTS -javaagent:<full path tooagent JAR file>"`
3. <span data-ttu-id="5e0c2-121">Starta om programservern.</span><span class="sxs-lookup"><span data-stu-id="5e0c2-121">Restart your application server.</span></span>

## <a name="configure-hello-agent"></a><span data-ttu-id="5e0c2-122">Konfigurera hello-agent</span><span class="sxs-lookup"><span data-stu-id="5e0c2-122">Configure hello agent</span></span>
<span data-ttu-id="5e0c2-123">Skapa en fil med namnet `AI-Agent.xml` och placera det i hello samma mapp som hello agent JAR-filen.</span><span class="sxs-lookup"><span data-stu-id="5e0c2-123">Create a file named `AI-Agent.xml` and place it in hello same folder as hello agent JAR file.</span></span>

<span data-ttu-id="5e0c2-124">Ange hello innehållet hello XML-filen.</span><span class="sxs-lookup"><span data-stu-id="5e0c2-124">Set hello content of hello xml file.</span></span> <span data-ttu-id="5e0c2-125">Redigera hello följande exempel tooinclude eller utelämna hello-funktioner som du vill ha.</span><span class="sxs-lookup"><span data-stu-id="5e0c2-125">Edit hello following example tooinclude or omit hello features you want.</span></span>

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

<span data-ttu-id="5e0c2-126">Du har tooenable rapporter undantag och metoden tidpunkten för enskilda metoder.</span><span class="sxs-lookup"><span data-stu-id="5e0c2-126">You have tooenable reports exception and method timing for individual methods.</span></span>

<span data-ttu-id="5e0c2-127">Som standard `reportExecutionTime` är true och `reportCaughtExceptions` är false.</span><span class="sxs-lookup"><span data-stu-id="5e0c2-127">By default, `reportExecutionTime` is true and `reportCaughtExceptions` is false.</span></span>

## <a name="view-hello-data"></a><span data-ttu-id="5e0c2-128">Visa hello data</span><span class="sxs-lookup"><span data-stu-id="5e0c2-128">View hello data</span></span>
<span data-ttu-id="5e0c2-129">Sammanställda remote beroende- och metoden körningstider visas i hello Application Insights-resurs, [under hello prestanda panelen][metrics].</span><span class="sxs-lookup"><span data-stu-id="5e0c2-129">In hello Application Insights resource, aggregated remote dependency and method execution times appears [under hello Performance tile][metrics].</span></span>

<span data-ttu-id="5e0c2-130">toosearch för enskilda instanser av beroende, undantag och metoden rapporter, öppna [Sök][diagnostic].</span><span class="sxs-lookup"><span data-stu-id="5e0c2-130">toosearch for individual instances of dependency, exception, and method reports, open [Search][diagnostic].</span></span>

<span data-ttu-id="5e0c2-131">[Diagnostisera problem med beroende - Läs mer](app-insights-asp-net-dependencies.md#diagnosis).</span><span class="sxs-lookup"><span data-stu-id="5e0c2-131">[Diagnosing dependency issues - learn more](app-insights-asp-net-dependencies.md#diagnosis).</span></span>

## <a name="questions-problems"></a><span data-ttu-id="5e0c2-132">Frågor?</span><span class="sxs-lookup"><span data-stu-id="5e0c2-132">Questions?</span></span> <span data-ttu-id="5e0c2-133">Har du problem?</span><span class="sxs-lookup"><span data-stu-id="5e0c2-133">Problems?</span></span>
* <span data-ttu-id="5e0c2-134">Ser du inga data?</span><span class="sxs-lookup"><span data-stu-id="5e0c2-134">No data?</span></span> [<span data-ttu-id="5e0c2-135">Ange undantag för brandväggen</span><span class="sxs-lookup"><span data-stu-id="5e0c2-135">Set firewall exceptions</span></span>](app-insights-ip-addresses.md)
* [<span data-ttu-id="5e0c2-136">Felsöka Java</span><span class="sxs-lookup"><span data-stu-id="5e0c2-136">Troubleshooting Java</span></span>](app-insights-java-troubleshoot.md)

<!--Link references-->

[api]: app-insights-api-custom-events-metrics.md
[apiexceptions]: app-insights-api-custom-events-metrics.md#track-exception
[availability]: app-insights-monitor-web-app-availability.md
[diagnostic]: app-insights-diagnostic-search.md
[eclipse]: app-insights-java-eclipse.md
[java]: app-insights-java-get-started.md
[javalogs]: app-insights-java-trace-logs.md
[metrics]: app-insights-metrics-explorer.md
