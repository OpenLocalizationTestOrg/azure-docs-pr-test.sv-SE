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
# <a name="explore-java-trace-logs-in-application-insights"></a><span data-ttu-id="61ec5-103">Utforska Java spårningsloggar i Application Insights</span><span class="sxs-lookup"><span data-stu-id="61ec5-103">Explore Java trace logs in Application Insights</span></span>
<span data-ttu-id="61ec5-104">Om du använder Logback eller Log4J (version 1.2 eller v2.0) för spårning, kan du ha spårloggarna skickas automatiskt tooApplication insikter där du kan utforska och söka i dem.</span><span class="sxs-lookup"><span data-stu-id="61ec5-104">If you're using Logback or Log4J (v1.2 or v2.0) for tracing, you can have your trace logs sent automatically tooApplication Insights where you can explore and search on them.</span></span>

## <a name="install-hello-java-sdk"></a><span data-ttu-id="61ec5-105">Installera hello Java SDK</span><span class="sxs-lookup"><span data-stu-id="61ec5-105">Install hello Java SDK</span></span>

<span data-ttu-id="61ec5-106">Installera [Application Insights SDK för Java][java], om du inte redan har gjort som.</span><span class="sxs-lookup"><span data-stu-id="61ec5-106">Install [Application Insights SDK for Java][java], if you haven't already done that.</span></span>

<span data-ttu-id="61ec5-107">(Om du inte vill tootrack HTTP-begäranden, kan du utelämna de flesta av hello XML-konfigurationsfil, men du måste åtminstone innehålla hello `InstrumentationKey` element.</span><span class="sxs-lookup"><span data-stu-id="61ec5-107">(If you don't want tootrack HTTP requests, you can omit most of hello .xml configuration file, but you must at least include hello `InstrumentationKey` element.</span></span> <span data-ttu-id="61ec5-108">Du bör också kontakta `new TelemetryClient()` tooinitialize hello SDK.)</span><span class="sxs-lookup"><span data-stu-id="61ec5-108">You should also call `new TelemetryClient()` tooinitialize hello SDK.)</span></span>


## <a name="add-logging-libraries-tooyour-project"></a><span data-ttu-id="61ec5-109">Lägga till loggning bibliotek tooyour projekt</span><span class="sxs-lookup"><span data-stu-id="61ec5-109">Add logging libraries tooyour project</span></span>
<span data-ttu-id="61ec5-110">*Välj hello lämpligt sätt för projektet.*</span><span class="sxs-lookup"><span data-stu-id="61ec5-110">*Choose hello appropriate way for your project.*</span></span>

#### <a name="if-youre-using-maven"></a><span data-ttu-id="61ec5-111">Om du använder Maven …</span><span class="sxs-lookup"><span data-stu-id="61ec5-111">If you're using Maven...</span></span>
<span data-ttu-id="61ec5-112">Om projektet har redan ställts in toouse Maven för version, koppla ett hello följande kodavsnitt i koden i filen pom.xml.</span><span class="sxs-lookup"><span data-stu-id="61ec5-112">If your project is already set up toouse Maven for build, merge one of hello following snippets of code into your pom.xml file.</span></span>

<span data-ttu-id="61ec5-113">Uppdatera sedan hello projektberoenden, tooget hello binärfiler som hämtats.</span><span class="sxs-lookup"><span data-stu-id="61ec5-113">Then refresh hello project dependencies, tooget hello binaries downloaded.</span></span>

<span data-ttu-id="61ec5-114">*Logback*</span><span class="sxs-lookup"><span data-stu-id="61ec5-114">*Logback*</span></span>

```XML

    <dependencies>
       <dependency>
          <groupId>com.microsoft.azure</groupId>
          <artifactId>applicationinsights-logging-logback</artifactId>
          <version>[1.0,)</version>
       </dependency>
    </dependencies>
```

<span data-ttu-id="61ec5-115">*Log4J v2.0*</span><span class="sxs-lookup"><span data-stu-id="61ec5-115">*Log4J v2.0*</span></span>

```XML

    <dependencies>
       <dependency>
          <groupId>com.microsoft.azure</groupId>
          <artifactId>applicationinsights-logging-log4j2</artifactId>
          <version>[1.0,)</version>
       </dependency>
    </dependencies>
```

<span data-ttu-id="61ec5-116">*Log4J version 1.2*</span><span class="sxs-lookup"><span data-stu-id="61ec5-116">*Log4J v1.2*</span></span>

```XML

    <dependencies>
       <dependency>
          <groupId>com.microsoft.azure</groupId>
          <artifactId>applicationinsights-logging-log4j1_2</artifactId>
          <version>[1.0,)</version>
       </dependency>
    </dependencies>
```

#### <a name="if-youre-using-gradle"></a><span data-ttu-id="61ec5-117">Om du använder Gradle …</span><span class="sxs-lookup"><span data-stu-id="61ec5-117">If you're using Gradle...</span></span>
<span data-ttu-id="61ec5-118">Om projektet har redan ställts in toouse Gradle för version, lägga till en av följande rader toohello hello `dependencies` i filen build.gradle:</span><span class="sxs-lookup"><span data-stu-id="61ec5-118">If your project is already set up toouse Gradle for build, add one of hello following lines toohello `dependencies` group in your build.gradle file:</span></span>

<span data-ttu-id="61ec5-119">Uppdatera sedan hello projektberoenden, tooget hello binärfiler som hämtats.</span><span class="sxs-lookup"><span data-stu-id="61ec5-119">Then refresh hello project dependencies, tooget hello binaries downloaded.</span></span>

<span data-ttu-id="61ec5-120">**Logback**</span><span class="sxs-lookup"><span data-stu-id="61ec5-120">**Logback**</span></span>

```

    compile group: 'com.microsoft.azure', name: 'applicationinsights-logging-logback', version: '1.0.+'
```

<span data-ttu-id="61ec5-121">**Log4J v2.0**</span><span class="sxs-lookup"><span data-stu-id="61ec5-121">**Log4J v2.0**</span></span>

```
    compile group: 'com.microsoft.azure', name: 'applicationinsights-logging-log4j2', version: '1.0.+'
```

<span data-ttu-id="61ec5-122">**Log4J version 1.2**</span><span class="sxs-lookup"><span data-stu-id="61ec5-122">**Log4J v1.2**</span></span>

```
    compile group: 'com.microsoft.azure', name: 'applicationinsights-logging-log4j1_2', version: '1.0.+'
```

#### <a name="otherwise-"></a><span data-ttu-id="61ec5-123">Eller …</span><span class="sxs-lookup"><span data-stu-id="61ec5-123">Otherwise ...</span></span>
<span data-ttu-id="61ec5-124">Ladda ned och extrahera hello lämpliga loggbilaga och lägger till hello bibliotek tooyour projektet:</span><span class="sxs-lookup"><span data-stu-id="61ec5-124">Download and extract hello appropriate appender, then add hello appropriate library tooyour project:</span></span>

| <span data-ttu-id="61ec5-125">Loggaren</span><span class="sxs-lookup"><span data-stu-id="61ec5-125">Logger</span></span> | <span data-ttu-id="61ec5-126">Ladda ned</span><span class="sxs-lookup"><span data-stu-id="61ec5-126">Download</span></span> | <span data-ttu-id="61ec5-127">Bibliotek</span><span class="sxs-lookup"><span data-stu-id="61ec5-127">Library</span></span> |
| --- | --- | --- |
| <span data-ttu-id="61ec5-128">Logback</span><span class="sxs-lookup"><span data-stu-id="61ec5-128">Logback</span></span> |[<span data-ttu-id="61ec5-129">SDK med Logback loggbilaga</span><span class="sxs-lookup"><span data-stu-id="61ec5-129">SDK with Logback appender</span></span>](https://aka.ms/xt62a4) |<span data-ttu-id="61ec5-130">logback-applicationinsights-loggning</span><span class="sxs-lookup"><span data-stu-id="61ec5-130">applicationinsights-logging-logback</span></span> |
| <span data-ttu-id="61ec5-131">Log4J v2.0</span><span class="sxs-lookup"><span data-stu-id="61ec5-131">Log4J v2.0</span></span> |[<span data-ttu-id="61ec5-132">SDK Log4J v2 loggbilaga</span><span class="sxs-lookup"><span data-stu-id="61ec5-132">SDK with Log4J v2 appender</span></span>](https://aka.ms/qypznq) |<span data-ttu-id="61ec5-133">log4j2-applicationinsights-loggning</span><span class="sxs-lookup"><span data-stu-id="61ec5-133">applicationinsights-logging-log4j2</span></span> |
| <span data-ttu-id="61ec5-134">Log4j version 1.2</span><span class="sxs-lookup"><span data-stu-id="61ec5-134">Log4j v1.2</span></span> |[<span data-ttu-id="61ec5-135">SDK Log4J version 1.2 loggbilaga</span><span class="sxs-lookup"><span data-stu-id="61ec5-135">SDK with Log4J v1.2 appender</span></span>](https://aka.ms/ky9cbo) |<span data-ttu-id="61ec5-136">log4j1_2-applicationinsights-loggning</span><span class="sxs-lookup"><span data-stu-id="61ec5-136">applicationinsights-logging-log4j1_2</span></span> |

## <a name="add-hello-appender-tooyour-logging-framework"></a><span data-ttu-id="61ec5-137">Lägg till hello loggbilaga tooyour loggningsramverk</span><span class="sxs-lookup"><span data-stu-id="61ec5-137">Add hello appender tooyour logging framework</span></span>
<span data-ttu-id="61ec5-138">toostart komma spårningar, merge hello relevanta kodavsnittet kod toohello Log4J eller Logback konfigurationsfilen:</span><span class="sxs-lookup"><span data-stu-id="61ec5-138">toostart getting traces, merge hello relevant snippet of code toohello Log4J or Logback configuration file:</span></span> 

<span data-ttu-id="61ec5-139">*Logback*</span><span class="sxs-lookup"><span data-stu-id="61ec5-139">*Logback*</span></span>

```XML

    <appender name="aiAppender" 
      class="com.microsoft.applicationinsights.logback.ApplicationInsightsAppender">
    </appender>
    <root level="trace">
      <appender-ref ref="aiAppender" />
    </root>
```

<span data-ttu-id="61ec5-140">*Log4J v2.0*</span><span class="sxs-lookup"><span data-stu-id="61ec5-140">*Log4J v2.0*</span></span>

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

<span data-ttu-id="61ec5-141">*Log4J version 1.2*</span><span class="sxs-lookup"><span data-stu-id="61ec5-141">*Log4J v1.2*</span></span>

```XML

    <appender name="aiAppender" 
         class="com.microsoft.applicationinsights.log4j.v1_2.ApplicationInsightsAppender">
    </appender>
    <root>
      <priority value ="trace" />
      <appender-ref ref="aiAppender" />
    </root>
```

<span data-ttu-id="61ec5-142">hello Application Insights appenders kan refereras genom att alla angivna loggaren och inte nödvändigtvis hello rot loggaren (som visas i hello-kodexempel ovan).</span><span class="sxs-lookup"><span data-stu-id="61ec5-142">hello Application Insights appenders can be referenced by any configured logger, and not necessarily by hello root logger (as shown in hello code samples above).</span></span>

## <a name="explore-your-traces-in-hello-application-insights-portal"></a><span data-ttu-id="61ec5-143">Utforska dina spår i hello Application Insights-portalen</span><span class="sxs-lookup"><span data-stu-id="61ec5-143">Explore your traces in hello Application Insights portal</span></span>
<span data-ttu-id="61ec5-144">Nu när du har konfigurerat dina projekt toosend spårar tooApplication insikter, kan du visa och söka efter de i hello Application Insights-portalen i hello [Sök] [ diagnostic] bladet.</span><span class="sxs-lookup"><span data-stu-id="61ec5-144">Now that you've configured your project toosend traces tooApplication Insights, you can view and search these traces in hello Application Insights portal, in hello [Search][diagnostic] blade.</span></span>

![Öppna Sök i hello Application Insights-portalen](./media/app-insights-java-trace-logs/10-diagnostics.png)

## <a name="next-steps"></a><span data-ttu-id="61ec5-146">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="61ec5-146">Next steps</span></span>
<span data-ttu-id="61ec5-147">[Diagnostiska sökning][diagnostic]</span><span class="sxs-lookup"><span data-stu-id="61ec5-147">[Diagnostic search][diagnostic]</span></span>

<!--Link references-->

[diagnostic]: app-insights-diagnostic-search.md
[java]: app-insights-java-get-started.md


