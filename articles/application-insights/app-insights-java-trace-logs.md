---
title: "Utforska Java spårningsloggar i Azure Application Insights | Microsoft Docs"
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
ms.openlocfilehash: 5baba3deaf58a1a24995c60381592a9c2ffefd81
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/18/2017
---
# <a name="explore-java-trace-logs-in-application-insights"></a><span data-ttu-id="be538-103">Utforska Java spårningsloggar i Application Insights</span><span class="sxs-lookup"><span data-stu-id="be538-103">Explore Java trace logs in Application Insights</span></span>
<span data-ttu-id="be538-104">Om du använder Logback eller Log4J (version 1.2 eller v2.0) för spårning, kan du ha spårloggarna skickas automatiskt till Application Insights där du kan utforska och söka i dem.</span><span class="sxs-lookup"><span data-stu-id="be538-104">If you're using Logback or Log4J (v1.2 or v2.0) for tracing, you can have your trace logs sent automatically to Application Insights where you can explore and search on them.</span></span>

## <a name="install-the-java-sdk"></a><span data-ttu-id="be538-105">Installera Java SDK</span><span class="sxs-lookup"><span data-stu-id="be538-105">Install the Java SDK</span></span>

<span data-ttu-id="be538-106">Installera [Application Insights SDK för Java][java], om du inte redan har gjort som.</span><span class="sxs-lookup"><span data-stu-id="be538-106">Install [Application Insights SDK for Java][java], if you haven't already done that.</span></span>

<span data-ttu-id="be538-107">(Om du inte vill spåra HTTP-begäranden, kan du utelämna de flesta av XML-konfigurationsfil, men du måste inkludera minst den `InstrumentationKey` element.</span><span class="sxs-lookup"><span data-stu-id="be538-107">(If you don't want to track HTTP requests, you can omit most of the .xml configuration file, but you must at least include the `InstrumentationKey` element.</span></span> <span data-ttu-id="be538-108">Du bör också kontakta `new TelemetryClient()` initieras SDK.)</span><span class="sxs-lookup"><span data-stu-id="be538-108">You should also call `new TelemetryClient()` to initialize the SDK.)</span></span>


## <a name="add-logging-libraries-to-your-project"></a><span data-ttu-id="be538-109">Lägg till loggning bibliotek i projektet</span><span class="sxs-lookup"><span data-stu-id="be538-109">Add logging libraries to your project</span></span>
<span data-ttu-id="be538-110">*Välj lämplig metod för ditt projekt.*</span><span class="sxs-lookup"><span data-stu-id="be538-110">*Choose the appropriate way for your project.*</span></span>

#### <a name="if-youre-using-maven"></a><span data-ttu-id="be538-111">Om du använder Maven …</span><span class="sxs-lookup"><span data-stu-id="be538-111">If you're using Maven...</span></span>
<span data-ttu-id="be538-112">Om projektet har redan konfigurerats att använda Maven för version, sammanfoga du något av följande kodavsnitt i koden i filen pom.xml.</span><span class="sxs-lookup"><span data-stu-id="be538-112">If your project is already set up to use Maven for build, merge one of the following snippets of code into your pom.xml file.</span></span>

<span data-ttu-id="be538-113">Uppdatera sedan projektberoenden för att få binärfiler som hämtats.</span><span class="sxs-lookup"><span data-stu-id="be538-113">Then refresh the project dependencies, to get the binaries downloaded.</span></span>

<span data-ttu-id="be538-114">*Logback*</span><span class="sxs-lookup"><span data-stu-id="be538-114">*Logback*</span></span>

```XML

    <dependencies>
       <dependency>
          <groupId>com.microsoft.azure</groupId>
          <artifactId>applicationinsights-logging-logback</artifactId>
          <version>[1.0,)</version>
       </dependency>
    </dependencies>
```

<span data-ttu-id="be538-115">*Log4J v2.0*</span><span class="sxs-lookup"><span data-stu-id="be538-115">*Log4J v2.0*</span></span>

```XML

    <dependencies>
       <dependency>
          <groupId>com.microsoft.azure</groupId>
          <artifactId>applicationinsights-logging-log4j2</artifactId>
          <version>[1.0,)</version>
       </dependency>
    </dependencies>
```

<span data-ttu-id="be538-116">*Log4J version 1.2*</span><span class="sxs-lookup"><span data-stu-id="be538-116">*Log4J v1.2*</span></span>

```XML

    <dependencies>
       <dependency>
          <groupId>com.microsoft.azure</groupId>
          <artifactId>applicationinsights-logging-log4j1_2</artifactId>
          <version>[1.0,)</version>
       </dependency>
    </dependencies>
```

#### <a name="if-youre-using-gradle"></a><span data-ttu-id="be538-117">Om du använder Gradle …</span><span class="sxs-lookup"><span data-stu-id="be538-117">If you're using Gradle...</span></span>
<span data-ttu-id="be538-118">Om projektet har redan konfigurerats att använda Gradle för version, lägga till en av följande rader till den `dependencies` i filen build.gradle:</span><span class="sxs-lookup"><span data-stu-id="be538-118">If your project is already set up to use Gradle for build, add one of the following lines to the `dependencies` group in your build.gradle file:</span></span>

<span data-ttu-id="be538-119">Uppdatera sedan projektberoenden för att få binärfiler som hämtats.</span><span class="sxs-lookup"><span data-stu-id="be538-119">Then refresh the project dependencies, to get the binaries downloaded.</span></span>

<span data-ttu-id="be538-120">**Logback**</span><span class="sxs-lookup"><span data-stu-id="be538-120">**Logback**</span></span>

```

    compile group: 'com.microsoft.azure', name: 'applicationinsights-logging-logback', version: '1.0.+'
```

<span data-ttu-id="be538-121">**Log4J v2.0**</span><span class="sxs-lookup"><span data-stu-id="be538-121">**Log4J v2.0**</span></span>

```
    compile group: 'com.microsoft.azure', name: 'applicationinsights-logging-log4j2', version: '1.0.+'
```

<span data-ttu-id="be538-122">**Log4J version 1.2**</span><span class="sxs-lookup"><span data-stu-id="be538-122">**Log4J v1.2**</span></span>

```
    compile group: 'com.microsoft.azure', name: 'applicationinsights-logging-log4j1_2', version: '1.0.+'
```

#### <a name="otherwise-"></a><span data-ttu-id="be538-123">Eller …</span><span class="sxs-lookup"><span data-stu-id="be538-123">Otherwise ...</span></span>
<span data-ttu-id="be538-124">Ladda ned och extrahera lämpliga loggbilaga och lägger till biblioteket i projektet:</span><span class="sxs-lookup"><span data-stu-id="be538-124">Download and extract the appropriate appender, then add the appropriate library to your project:</span></span>

| <span data-ttu-id="be538-125">Loggaren</span><span class="sxs-lookup"><span data-stu-id="be538-125">Logger</span></span> | <span data-ttu-id="be538-126">Ladda ned</span><span class="sxs-lookup"><span data-stu-id="be538-126">Download</span></span> | <span data-ttu-id="be538-127">Bibliotek</span><span class="sxs-lookup"><span data-stu-id="be538-127">Library</span></span> |
| --- | --- | --- |
| <span data-ttu-id="be538-128">Logback</span><span class="sxs-lookup"><span data-stu-id="be538-128">Logback</span></span> |[<span data-ttu-id="be538-129">SDK med Logback loggbilaga</span><span class="sxs-lookup"><span data-stu-id="be538-129">SDK with Logback appender</span></span>](https://aka.ms/xt62a4) |<span data-ttu-id="be538-130">logback-applicationinsights-loggning</span><span class="sxs-lookup"><span data-stu-id="be538-130">applicationinsights-logging-logback</span></span> |
| <span data-ttu-id="be538-131">Log4J v2.0</span><span class="sxs-lookup"><span data-stu-id="be538-131">Log4J v2.0</span></span> |[<span data-ttu-id="be538-132">SDK Log4J v2 loggbilaga</span><span class="sxs-lookup"><span data-stu-id="be538-132">SDK with Log4J v2 appender</span></span>](https://aka.ms/qypznq) |<span data-ttu-id="be538-133">log4j2-applicationinsights-loggning</span><span class="sxs-lookup"><span data-stu-id="be538-133">applicationinsights-logging-log4j2</span></span> |
| <span data-ttu-id="be538-134">Log4j version 1.2</span><span class="sxs-lookup"><span data-stu-id="be538-134">Log4j v1.2</span></span> |[<span data-ttu-id="be538-135">SDK Log4J version 1.2 loggbilaga</span><span class="sxs-lookup"><span data-stu-id="be538-135">SDK with Log4J v1.2 appender</span></span>](https://aka.ms/ky9cbo) |<span data-ttu-id="be538-136">log4j1_2-applicationinsights-loggning</span><span class="sxs-lookup"><span data-stu-id="be538-136">applicationinsights-logging-log4j1_2</span></span> |

## <a name="add-the-appender-to-your-logging-framework"></a><span data-ttu-id="be538-137">Lägg till loggbilaga i din loggningsramverk</span><span class="sxs-lookup"><span data-stu-id="be538-137">Add the appender to your logging framework</span></span>
<span data-ttu-id="be538-138">Om du vill börja få spårningar, sammanfoga det relevanta kodavsnittet till konfigurationsfilen Log4J eller Logback:</span><span class="sxs-lookup"><span data-stu-id="be538-138">To start getting traces, merge the relevant snippet of code to the Log4J or Logback configuration file:</span></span> 

<span data-ttu-id="be538-139">*Logback*</span><span class="sxs-lookup"><span data-stu-id="be538-139">*Logback*</span></span>

```XML

    <appender name="aiAppender" 
      class="com.microsoft.applicationinsights.logback.ApplicationInsightsAppender">
    </appender>
    <root level="trace">
      <appender-ref ref="aiAppender" />
    </root>
```

<span data-ttu-id="be538-140">*Log4J v2.0*</span><span class="sxs-lookup"><span data-stu-id="be538-140">*Log4J v2.0*</span></span>

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

<span data-ttu-id="be538-141">*Log4J version 1.2*</span><span class="sxs-lookup"><span data-stu-id="be538-141">*Log4J v1.2*</span></span>

```XML

    <appender name="aiAppender" 
         class="com.microsoft.applicationinsights.log4j.v1_2.ApplicationInsightsAppender">
    </appender>
    <root>
      <priority value ="trace" />
      <appender-ref ref="aiAppender" />
    </root>
```

<span data-ttu-id="be538-142">Application Insights appenders kan refereras genom att alla angivna loggaren och inte nödvändigtvis loggaren rot (som visas i kodexemplen ovan).</span><span class="sxs-lookup"><span data-stu-id="be538-142">The Application Insights appenders can be referenced by any configured logger, and not necessarily by the root logger (as shown in the code samples above).</span></span>

## <a name="explore-your-traces-in-the-application-insights-portal"></a><span data-ttu-id="be538-143">Utforska dina spår i Application Insights-portalen</span><span class="sxs-lookup"><span data-stu-id="be538-143">Explore your traces in the Application Insights portal</span></span>
<span data-ttu-id="be538-144">Nu när du har konfigurerat ditt projekt för att skicka spårningar till Application Insights kan du visa och söka efter dessa spåren i Application Insights-portalen i den [Sök] [ diagnostic] bladet.</span><span class="sxs-lookup"><span data-stu-id="be538-144">Now that you've configured your project to send traces to Application Insights, you can view and search these traces in the Application Insights portal, in the [Search][diagnostic] blade.</span></span>

![Öppna Sök i Application Insights-portalen](./media/app-insights-java-trace-logs/10-diagnostics.png)

## <a name="next-steps"></a><span data-ttu-id="be538-146">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="be538-146">Next steps</span></span>
<span data-ttu-id="be538-147">[Diagnostiska sökning][diagnostic]</span><span class="sxs-lookup"><span data-stu-id="be538-147">[Diagnostic search][diagnostic]</span></span>

<!--Link references-->

[diagnostic]: app-insights-diagnostic-search.md
[java]: app-insights-java-get-started.md


