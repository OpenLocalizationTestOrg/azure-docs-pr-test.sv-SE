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
# <a name="application-insights-for-java-web-apps-that-are-already-live"></a><span data-ttu-id="d2c35-103">Application Insights för Java-webbappar som redan är live</span><span class="sxs-lookup"><span data-stu-id="d2c35-103">Application Insights for Java web apps that are already live</span></span>


<span data-ttu-id="d2c35-104">Om du har ett program som redan körs på J2EE-server kan du börja övervaka med [Programinsikter](app-insights-overview.md) utan hello behöver toomake code ändras eller kompileras projektet.</span><span class="sxs-lookup"><span data-stu-id="d2c35-104">If you have a web application that is already running on your J2EE server, you can start monitoring it with [Application Insights](app-insights-overview.md) without hello need toomake code changes or recompile your project.</span></span> <span data-ttu-id="d2c35-105">Med det här alternativet får du information om HTTP-begäranden skickas tooyour server, ohanterade undantag och prestandaräknare.</span><span class="sxs-lookup"><span data-stu-id="d2c35-105">With this option, you get information about HTTP requests sent tooyour server, unhandled exceptions, and performance counters.</span></span>

<span data-ttu-id="d2c35-106">Du behöver en prenumeration för[Microsoft Azure](https://azure.com).</span><span class="sxs-lookup"><span data-stu-id="d2c35-106">You'll need a subscription too[Microsoft Azure](https://azure.com).</span></span>

> [!NOTE]
> <span data-ttu-id="d2c35-107">hello procedur på den här sidan lägger till webbprogrammet för hello SDK tooyour vid körning.</span><span class="sxs-lookup"><span data-stu-id="d2c35-107">hello procedure on this page adds hello SDK tooyour web app at runtime.</span></span> <span data-ttu-id="d2c35-108">Den här runtime instrumentation är användbart om du inte vill att tooupdate eller återskapa din källkod.</span><span class="sxs-lookup"><span data-stu-id="d2c35-108">This runtime instrumentation is useful if you don't want tooupdate or rebuild your source code.</span></span> <span data-ttu-id="d2c35-109">Men om möjligt rekommenderar vi att du [lägga till hello SDK toohello källkoden](app-insights-java-get-started.md) i stället.</span><span class="sxs-lookup"><span data-stu-id="d2c35-109">But if you can, we recommend you [add hello SDK toohello source code](app-insights-java-get-started.md) instead.</span></span> <span data-ttu-id="d2c35-110">Som ger dig fler alternativ, till exempel skriva kod tootrack användaraktivitet.</span><span class="sxs-lookup"><span data-stu-id="d2c35-110">That gives you more options such as writing code tootrack user activity.</span></span>
> 
> 

## <a name="1-get-an-application-insights-instrumentation-key"></a><span data-ttu-id="d2c35-111">1. Hämta en Application Insights-instrumenteringsnyckel</span><span class="sxs-lookup"><span data-stu-id="d2c35-111">1. Get an Application Insights instrumentation key</span></span>
1. <span data-ttu-id="d2c35-112">Logga in toohello [Microsoft Azure-portalen](https://portal.azure.com)</span><span class="sxs-lookup"><span data-stu-id="d2c35-112">Sign in toohello [Microsoft Azure portal](https://portal.azure.com)</span></span>
2. <span data-ttu-id="d2c35-113">Skapa en ny Application Insights-resurs och ange hello programmet typen tooJava webbprogram.</span><span class="sxs-lookup"><span data-stu-id="d2c35-113">Create a new Application Insights resource and set hello application type tooJava web application.</span></span>
   
    ![Fyll i ett namn, välj Java-webbapp och klicka på Skapa](./media/app-insights-java-live/02-create.png)

    <span data-ttu-id="d2c35-115">hello resursen skapas i några sekunder.</span><span class="sxs-lookup"><span data-stu-id="d2c35-115">hello resource is created in a few seconds.</span></span>

4. <span data-ttu-id="d2c35-116">Öppna hello ny resurs och dess instrumentation nyckel.</span><span class="sxs-lookup"><span data-stu-id="d2c35-116">Open hello new resource and get its instrumentation key.</span></span> <span data-ttu-id="d2c35-117">Du behöver toopaste nyckeln i projektet koden inom kort.</span><span class="sxs-lookup"><span data-stu-id="d2c35-117">You'll need toopaste this key into your code project shortly.</span></span>
   
    ![Klicka på egenskaper i hello nya Resursöversikt och kopiera hello Instrumentation nyckel](./media/app-insights-java-live/03-key.png)

## <a name="2-download-hello-sdk"></a><span data-ttu-id="d2c35-119">2. Hämta hello SDK</span><span class="sxs-lookup"><span data-stu-id="d2c35-119">2. Download hello SDK</span></span>
1. <span data-ttu-id="d2c35-120">Hämta hello [Application Insights SDK för Java](https://aka.ms/aijavasdk).</span><span class="sxs-lookup"><span data-stu-id="d2c35-120">Download hello [Application Insights SDK for Java](https://aka.ms/aijavasdk).</span></span> 
2. <span data-ttu-id="d2c35-121">Extrahera hello SDK innehållet toohello-katalogen som dina projekt binärfiler har lästs in på servern.</span><span class="sxs-lookup"><span data-stu-id="d2c35-121">On your server, extract hello SDK contents toohello directory from which your project binaries are loaded.</span></span> <span data-ttu-id="d2c35-122">Om du använder Tomcat kan katalogen vara under`webapps/<your_app_name>/WEB-INF/lib`</span><span class="sxs-lookup"><span data-stu-id="d2c35-122">If you’re using Tomcat, this directory would typically be under `webapps/<your_app_name>/WEB-INF/lib`</span></span>

<span data-ttu-id="d2c35-123">Observera att du behöver toorepeat detta på varje server-instans och för varje app.</span><span class="sxs-lookup"><span data-stu-id="d2c35-123">Note that you need toorepeat this on each server instance, and for each app.</span></span>

## <a name="3-add-an-application-insights-xml-file"></a><span data-ttu-id="d2c35-124">3. Lägg till en XML-fil för Application Insights</span><span class="sxs-lookup"><span data-stu-id="d2c35-124">3. Add an Application Insights xml file</span></span>
<span data-ttu-id="d2c35-125">Skapa ApplicationInsights.xml i hello mappen där du har lagt till hello SDK.</span><span class="sxs-lookup"><span data-stu-id="d2c35-125">Create ApplicationInsights.xml in hello folder in which you added hello SDK.</span></span> <span data-ttu-id="d2c35-126">Placera i den hello följande XML.</span><span class="sxs-lookup"><span data-stu-id="d2c35-126">Put into it hello following XML.</span></span>

<span data-ttu-id="d2c35-127">Ersätt hello instrumentation nyckel som du har fått från hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="d2c35-127">Substitute hello instrumentation key that you got from hello Azure portal.</span></span>

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

* <span data-ttu-id="d2c35-128">hello instrumentation nyckel skickas tillsammans med alla element på telemetri och talar om Application Insights toodisplay den i din resurs.</span><span class="sxs-lookup"><span data-stu-id="d2c35-128">hello instrumentation key is sent along with every item of telemetry and tells Application Insights toodisplay it in your resource.</span></span>
* <span data-ttu-id="d2c35-129">hello HTTP-begäran-komponenten är valfritt.</span><span class="sxs-lookup"><span data-stu-id="d2c35-129">hello HTTP Request component is optional.</span></span> <span data-ttu-id="d2c35-130">Telemetri om begäran och svar gånger toohello portal skickas automatiskt.</span><span class="sxs-lookup"><span data-stu-id="d2c35-130">It automatically sends telemetry about requests and response times toohello portal.</span></span>
* <span data-ttu-id="d2c35-131">Händelser korrelationen är en komponent som tillägg toohello HTTP-begäran.</span><span class="sxs-lookup"><span data-stu-id="d2c35-131">Events correlation is an addition toohello HTTP request component.</span></span> <span data-ttu-id="d2c35-132">Den tilldelas en identifierare tooeach begäran tas emot av hello-servern och lägger till den här identifieraren som ett objekt med egenskapen tooevery av telemetri som hello egenskapen 'Operation.Id'.</span><span class="sxs-lookup"><span data-stu-id="d2c35-132">It assigns an identifier tooeach request received by hello server, and adds this identifier as a property tooevery item of telemetry as hello property 'Operation.Id'.</span></span> <span data-ttu-id="d2c35-133">Det gör att du toocorrelate hello telemetri som är associerade med varje begäran genom att ange ett filter i [diagnostiska Sök](app-insights-diagnostic-search.md).</span><span class="sxs-lookup"><span data-stu-id="d2c35-133">It allows you toocorrelate hello telemetry associated with each request by setting a filter in [diagnostic search](app-insights-diagnostic-search.md).</span></span>

## <a name="4-add-an-http-filter"></a><span data-ttu-id="d2c35-134">4. Lägga till ett HTTP-filter</span><span class="sxs-lookup"><span data-stu-id="d2c35-134">4. Add an HTTP filter</span></span>
<span data-ttu-id="d2c35-135">Leta upp och öppna hello web.xml filen i projektet och merge hello följande kodavsnittet under hello webbprogrammet nod där programmet filter har konfigurerats.</span><span class="sxs-lookup"><span data-stu-id="d2c35-135">Locate and open hello web.xml file in your project, and merge hello following snippet of code under hello web-app node, where your application filters are configured.</span></span>

<span data-ttu-id="d2c35-136">tooget hello mest korrekta resultat, hello filter ska mappas innan alla filter.</span><span class="sxs-lookup"><span data-stu-id="d2c35-136">tooget hello most accurate results, hello filter should be mapped before all other filters.</span></span>

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

## <a name="5-check-firewall-exceptions"></a><span data-ttu-id="d2c35-137">5. Kontrollera brandväggsundantag</span><span class="sxs-lookup"><span data-stu-id="d2c35-137">5. Check firewall exceptions</span></span>
<span data-ttu-id="d2c35-138">Du kan behöva för[ange undantag toosend utgående data](app-insights-ip-addresses.md).</span><span class="sxs-lookup"><span data-stu-id="d2c35-138">You might need too[set exceptions toosend outgoing data](app-insights-ip-addresses.md).</span></span>

## <a name="6-restart-your-web-app"></a><span data-ttu-id="d2c35-139">6. Starta om ditt webbprogram</span><span class="sxs-lookup"><span data-stu-id="d2c35-139">6. Restart your web app</span></span>
## <a name="7-view-your-telemetry-in-application-insights"></a><span data-ttu-id="d2c35-140">7. Visa telemetrin i Application Insights</span><span class="sxs-lookup"><span data-stu-id="d2c35-140">7. View your telemetry in Application Insights</span></span>
<span data-ttu-id="d2c35-141">Returnera tooyour Application Insights-resurs i [Microsoft Azure-portalen](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="d2c35-141">Return tooyour Application Insights resource in [Microsoft Azure portal](https://portal.azure.com).</span></span>

<span data-ttu-id="d2c35-142">Telemetri om HTTP-begäranden som visas på hello översikt bladet.</span><span class="sxs-lookup"><span data-stu-id="d2c35-142">Telemetry about HTTP requests appears on hello overview blade.</span></span> <span data-ttu-id="d2c35-143">(Om informationen inte visas väntar du några sekunder och klickar på Uppdatera.)</span><span class="sxs-lookup"><span data-stu-id="d2c35-143">(If it isn't there, wait a few seconds and then click Refresh.)</span></span>

![exempeldata](./media/app-insights-java-live/5-results.png)

<span data-ttu-id="d2c35-145">Klicka på via alla diagram toosee mer detaljerad mått.</span><span class="sxs-lookup"><span data-stu-id="d2c35-145">Click through any chart toosee more detailed metrics.</span></span> 

![](./media/app-insights-java-live/6-barchart.png)

<span data-ttu-id="d2c35-146">Och när du visar hello egenskaperna för en begäran, kan du visa hello telemetri händelser som är kopplade till den, till exempel begäranden och undantag.</span><span class="sxs-lookup"><span data-stu-id="d2c35-146">And when viewing hello properties of a request, you can see hello telemetry events associated with it such as requests and exceptions.</span></span>

![](./media/app-insights-java-live/7-instance.png)

[<span data-ttu-id="d2c35-147">Lär dig mer om mätvärden.</span><span class="sxs-lookup"><span data-stu-id="d2c35-147">Learn more about metrics.</span></span>](app-insights-metrics-explorer.md)

## <a name="next-steps"></a><span data-ttu-id="d2c35-148">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="d2c35-148">Next steps</span></span>
* <span data-ttu-id="d2c35-149">[Lägga till telemetri tooyour webbsidor](app-insights-javascript.md) toomonitor sidan vyer och användaren mått.</span><span class="sxs-lookup"><span data-stu-id="d2c35-149">[Add telemetry tooyour web pages](app-insights-javascript.md) toomonitor page views and user metrics.</span></span>
* <span data-ttu-id="d2c35-150">[Ställ in webbtester](app-insights-monitor-web-app-availability.md) toomake säker tillämpningsprogrammet är live och svarstid.</span><span class="sxs-lookup"><span data-stu-id="d2c35-150">[Set up web tests](app-insights-monitor-web-app-availability.md) toomake sure your application stays live and responsive.</span></span>
* [<span data-ttu-id="d2c35-151">Avbilda loggspårningar</span><span class="sxs-lookup"><span data-stu-id="d2c35-151">Capture log traces</span></span>](app-insights-java-trace-logs.md)
* <span data-ttu-id="d2c35-152">[Söka efter händelser och loggar](app-insights-diagnostic-search.md) toohelp diagnostisera problem.</span><span class="sxs-lookup"><span data-stu-id="d2c35-152">[Search events and logs](app-insights-diagnostic-search.md) toohelp diagnose problems.</span></span>

