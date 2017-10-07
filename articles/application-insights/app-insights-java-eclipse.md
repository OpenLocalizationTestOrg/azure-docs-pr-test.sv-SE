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
# <a name="get-started-with-application-insights-with-java-in-eclipse"></a><span data-ttu-id="4b464-103">Kom igång med Application Insights med Java i Eclipse</span><span class="sxs-lookup"><span data-stu-id="4b464-103">Get started with Application Insights with Java in Eclipse</span></span>
<span data-ttu-id="4b464-104">hello Application Insights SDK skickar telemetri från ditt webbprogram för Java så att du kan analysera användnings- och prestandadata.</span><span class="sxs-lookup"><span data-stu-id="4b464-104">hello Application Insights SDK sends telemetry from your Java web application so that you can analyze usage and performance.</span></span> <span data-ttu-id="4b464-105">hello Eclipse plugin-program för Application Insights installeras automatiskt hello SDK i projektet så att du får slut hello rutan telemetri plus en API som du kan använda toowrite anpassad telemetri.</span><span class="sxs-lookup"><span data-stu-id="4b464-105">hello Eclipse plug-in for Application Insights automatically installs hello SDK in your project so that you get out of hello box telemetry, plus an API that you can use toowrite custom telemetry.</span></span>   

## <a name="prerequisites"></a><span data-ttu-id="4b464-106">Krav</span><span class="sxs-lookup"><span data-stu-id="4b464-106">Prerequisites</span></span>
<span data-ttu-id="4b464-107">Plugin-programmet hello fungerar för närvarande för Maven-projekt och dynamiskt webbprojekt i Eclipse.</span><span class="sxs-lookup"><span data-stu-id="4b464-107">Currently hello plug-in works for Maven projects and Dynamic Web Projects in Eclipse.</span></span>
<span data-ttu-id="4b464-108">([Lägg till Application Insights tooother typer av Java-projekt][java].)</span><span class="sxs-lookup"><span data-stu-id="4b464-108">([Add Application Insights tooother types of Java project][java].)</span></span>

<span data-ttu-id="4b464-109">Du behöver:</span><span class="sxs-lookup"><span data-stu-id="4b464-109">You'll need:</span></span>

* <span data-ttu-id="4b464-110">Oracle JRE 1.6 eller senare</span><span class="sxs-lookup"><span data-stu-id="4b464-110">Oracle JRE 1.6 or later</span></span>
* <span data-ttu-id="4b464-111">En prenumeration för[Microsoft Azure](https://azure.microsoft.com/).</span><span class="sxs-lookup"><span data-stu-id="4b464-111">A subscription too[Microsoft Azure](https://azure.microsoft.com/).</span></span>
* <span data-ttu-id="4b464-112">[Solförmörkelse IDE för Java EE-utvecklare](http://www.eclipse.org/downloads/), Indigo eller senare.</span><span class="sxs-lookup"><span data-stu-id="4b464-112">[Eclipse IDE for Java EE Developers](http://www.eclipse.org/downloads/), Indigo or later.</span></span>
* <span data-ttu-id="4b464-113">Windows 7 eller senare och Windows Server 2008 eller senare</span><span class="sxs-lookup"><span data-stu-id="4b464-113">Windows 7 or later, or Windows Server 2008 or later</span></span>

## <a name="install-hello-sdk-on-eclipse-one-time"></a><span data-ttu-id="4b464-114">Installera hello SDK på Eclipse (en gång)</span><span class="sxs-lookup"><span data-stu-id="4b464-114">Install hello SDK on Eclipse (one time)</span></span>
<span data-ttu-id="4b464-115">Du har bara toodo en gång per dator.</span><span class="sxs-lookup"><span data-stu-id="4b464-115">You only have toodo this one time per machine.</span></span> <span data-ttu-id="4b464-116">Det här steget installerar en verktygslåda som sedan kan lägga till hello SDK tooeach dynamiskt webbprojekt.</span><span class="sxs-lookup"><span data-stu-id="4b464-116">This step installs a toolkit which can then add hello SDK tooeach Dynamic Web Project.</span></span>

1. <span data-ttu-id="4b464-117">Klicka på Hjälp, installera ny programvara i Eclipse.</span><span class="sxs-lookup"><span data-stu-id="4b464-117">In Eclipse, click Help, Install New Software.</span></span>

    ![Hjälp, installera ny programvara](./media/app-insights-java-eclipse/0-plugin.png)
2. <span data-ttu-id="4b464-119">hello SDK finns i http://dl.microsoft.com/eclipse under Azure Toolkit.</span><span class="sxs-lookup"><span data-stu-id="4b464-119">hello SDK is in http://dl.microsoft.com/eclipse, under Azure Toolkit.</span></span>
3. <span data-ttu-id="4b464-120">Avmarkera **kontakta alla update platser...**</span><span class="sxs-lookup"><span data-stu-id="4b464-120">Uncheck **Contact all update sites...**</span></span>

    ![För Application Insights SDK, rensar kontakta uppdatera alla platser](./media/app-insights-java-eclipse/1-plugin.png)

<span data-ttu-id="4b464-122">Följ hello återstående steg för varje Java-projekt.</span><span class="sxs-lookup"><span data-stu-id="4b464-122">Follow hello remaining steps for each Java project.</span></span>

## <a name="create-an-application-insights-resource-in-azure"></a><span data-ttu-id="4b464-123">Skapa en Application Insights-resurs i Azure</span><span class="sxs-lookup"><span data-stu-id="4b464-123">Create an Application Insights resource in Azure</span></span>
1. <span data-ttu-id="4b464-124">Logga in toohello [Azure-portalen](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="4b464-124">Sign in toohello [Azure portal](https://portal.azure.com).</span></span>
2. <span data-ttu-id="4b464-125">Skapa en ny Application Insights-resurs.</span><span class="sxs-lookup"><span data-stu-id="4b464-125">Create a new Application Insights resource.</span></span> <span data-ttu-id="4b464-126">Ange hello programmet typen tooJava webbprogram.</span><span class="sxs-lookup"><span data-stu-id="4b464-126">Set hello application type tooJava web application.</span></span>  

    ![Klicka på + och välj Application Insights](./media/app-insights-java-eclipse/01-create.png)  

4. <span data-ttu-id="4b464-128">Hitta hello instrumentation nyckeln för hello ny resurs.</span><span class="sxs-lookup"><span data-stu-id="4b464-128">Find hello instrumentation key of hello new resource.</span></span> <span data-ttu-id="4b464-129">Du behöver toopaste den i projektet koden inom kort.</span><span class="sxs-lookup"><span data-stu-id="4b464-129">You'll need toopaste this into your code project shortly.</span></span>  

    ![Klicka på egenskaper i hello nya Resursöversikt och kopiera hello Instrumentation nyckel](./media/app-insights-java-eclipse/03-key.png)  

## <a name="add-application-insights-tooyour-project"></a><span data-ttu-id="4b464-131">Lägga till Application Insights tooyour projekt</span><span class="sxs-lookup"><span data-stu-id="4b464-131">Add Application Insights tooyour project</span></span>
1. <span data-ttu-id="4b464-132">Lägg till Application Insights hello snabbmenyn för webbprojektet Java.</span><span class="sxs-lookup"><span data-stu-id="4b464-132">Add Application Insights from hello context menu of your Java web project.</span></span>

    ![Klicka på egenskaper i hello nya Resursöversikt och kopiera hello Instrumentation nyckel](./media/app-insights-java-eclipse/02-context-menu.png)
2. <span data-ttu-id="4b464-134">Klistra in hello instrumentation nyckel som du har fått från hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="4b464-134">Paste hello instrumentation key that you got from hello Azure portal.</span></span>

    ![Klicka på egenskaper i hello nya Resursöversikt och kopiera hello Instrumentation nyckel](./media/app-insights-java-eclipse/03-ikey.png)

<span data-ttu-id="4b464-136">hello nyckel skickas tillsammans med alla element på telemetri och talar om Application Insights toodisplay den i din resurs.</span><span class="sxs-lookup"><span data-stu-id="4b464-136">hello key is sent along with every item of telemetry and tells Application Insights toodisplay it in your resource.</span></span>

## <a name="run-hello-application-and-see-metrics"></a><span data-ttu-id="4b464-137">Kör programmet hello och se mått</span><span class="sxs-lookup"><span data-stu-id="4b464-137">Run hello application and see metrics</span></span>
<span data-ttu-id="4b464-138">Kör ditt program.</span><span class="sxs-lookup"><span data-stu-id="4b464-138">Run your application.</span></span>

<span data-ttu-id="4b464-139">Returnera tooyour Application Insights-resurs i Microsoft Azure.</span><span class="sxs-lookup"><span data-stu-id="4b464-139">Return tooyour Application Insights resource in Microsoft Azure.</span></span>

<span data-ttu-id="4b464-140">HTTP-begäranden data visas på hello översikt bladet.</span><span class="sxs-lookup"><span data-stu-id="4b464-140">HTTP requests data will appear on hello overview blade.</span></span> <span data-ttu-id="4b464-141">(Om informationen inte visas väntar du några sekunder och klickar på Uppdatera.)</span><span class="sxs-lookup"><span data-stu-id="4b464-141">(If it isn't there, wait a few seconds and then click Refresh.)</span></span>

![<span data-ttu-id="4b464-142">Svaret från servern och begäran antal fel</span><span class="sxs-lookup"><span data-stu-id="4b464-142">Server response, request counts, and failures</span></span> ](./media/app-insights-java-eclipse/5-results.png)

<span data-ttu-id="4b464-143">Klicka på via alla diagram toosee mer detaljerad mått.</span><span class="sxs-lookup"><span data-stu-id="4b464-143">Click through any chart toosee more detailed metrics.</span></span>

![Begär antal efter namn](./media/app-insights-java-eclipse/6-barchart.png)

<span data-ttu-id="4b464-145">[Lär dig mer om mätvärden.][metrics]</span><span class="sxs-lookup"><span data-stu-id="4b464-145">[Learn more about metrics.][metrics]</span></span>

<span data-ttu-id="4b464-146">Och när du visar hello egenskaperna för en begäran, kan du visa hello telemetri händelser som är kopplade till den, till exempel begäranden och undantag.</span><span class="sxs-lookup"><span data-stu-id="4b464-146">And when viewing hello properties of a request, you can see hello telemetry events associated with it such as requests and exceptions.</span></span>

![Alla spårningar för den här begäran](./media/app-insights-java-eclipse/7-instance.png)

## <a name="client-side-telemetry"></a><span data-ttu-id="4b464-148">Telemetri för klientsidan</span><span class="sxs-lookup"><span data-stu-id="4b464-148">Client-side telemetry</span></span>
<span data-ttu-id="4b464-149">Hello Snabbstart-bladet, klicka på Hämta koden toomonitor webbsidor:</span><span class="sxs-lookup"><span data-stu-id="4b464-149">From hello QuickStart blade, click Get code toomonitor my web pages:</span></span>

![Välj Snabbstart, hämta koden toomonitor webbsidor på bladet översikt över din app.](./media/app-insights-java-eclipse/02-monitor-web-page.png)

<span data-ttu-id="4b464-152">Infoga hello kodstycke i hello huvud HTML-filer.</span><span class="sxs-lookup"><span data-stu-id="4b464-152">Insert hello code snippet in hello head of your HTML files.</span></span>

#### <a name="view-client-side-data"></a><span data-ttu-id="4b464-153">Visa data för klientsidan</span><span class="sxs-lookup"><span data-stu-id="4b464-153">View client-side data</span></span>
<span data-ttu-id="4b464-154">Öppna din uppdaterade webbsidor och använder dem.</span><span class="sxs-lookup"><span data-stu-id="4b464-154">Open your updated web pages and use them.</span></span> <span data-ttu-id="4b464-155">Vänta en minut eller två och återgå sedan tooApplication insikter och öppna hello användning bladet.</span><span class="sxs-lookup"><span data-stu-id="4b464-155">Wait a minute or two, then return tooApplication Insights and open hello usage blade.</span></span> <span data-ttu-id="4b464-156">(Hello översikt bladet rulla nedåt och klicka på användning.)</span><span class="sxs-lookup"><span data-stu-id="4b464-156">(From hello Overview blade, scroll down and click Usage.)</span></span>

<span data-ttu-id="4b464-157">Sidan Visa, användare och session mått visas på bladet för användning av hello:</span><span class="sxs-lookup"><span data-stu-id="4b464-157">Page view, user, and session metrics will appear on hello usage blade:</span></span>

![Sessioner, användare och sidvyer](./media/app-insights-java-eclipse/appinsights-47usage-2.png)

<span data-ttu-id="4b464-159">[Mer information om hur du konfigurerar telemetri för klientsidan.][usage]</span><span class="sxs-lookup"><span data-stu-id="4b464-159">[Learn more about setting up client-side telemetry.][usage]</span></span>

## <a name="publish-your-application"></a><span data-ttu-id="4b464-160">Publicera programmet</span><span class="sxs-lookup"><span data-stu-id="4b464-160">Publish your application</span></span>
<span data-ttu-id="4b464-161">Nu publicera din app toohello server, låta personer använder den, och titta på hello telemetri som visas på hello-portalen.</span><span class="sxs-lookup"><span data-stu-id="4b464-161">Now publish your app toohello server, let people use it, and watch hello telemetry show up on hello portal.</span></span>

* <span data-ttu-id="4b464-162">Kontrollera att brandväggen tillåter programmet-toosend telemetri toothese portar:</span><span class="sxs-lookup"><span data-stu-id="4b464-162">Make sure your firewall allows your application toosend telemetry toothese ports:</span></span>

  * <span data-ttu-id="4b464-163">dc.services.visualstudio.com:443</span><span class="sxs-lookup"><span data-stu-id="4b464-163">dc.services.visualstudio.com:443</span></span>
  * <span data-ttu-id="4b464-164">dc.services.visualstudio.com:80</span><span class="sxs-lookup"><span data-stu-id="4b464-164">dc.services.visualstudio.com:80</span></span>
  * <span data-ttu-id="4b464-165">f5.services.visualstudio.com:443</span><span class="sxs-lookup"><span data-stu-id="4b464-165">f5.services.visualstudio.com:443</span></span>
  * <span data-ttu-id="4b464-166">f5.services.visualstudio.com:80</span><span class="sxs-lookup"><span data-stu-id="4b464-166">f5.services.visualstudio.com:80</span></span>
* <span data-ttu-id="4b464-167">Installera följande på Windows-servrar:</span><span class="sxs-lookup"><span data-stu-id="4b464-167">On Windows servers, install:</span></span>

  * [<span data-ttu-id="4b464-168">Microsoft Visual C++ Redistributable</span><span class="sxs-lookup"><span data-stu-id="4b464-168">Microsoft Visual C++ Redistributable</span></span>](http://www.microsoft.com/download/details.aspx?id=40784)

    <span data-ttu-id="4b464-169">(Gör att du kan använda prestandaräknare.)</span><span class="sxs-lookup"><span data-stu-id="4b464-169">(This enables performance counters.)</span></span>

## <a name="exceptions-and-request-failures"></a><span data-ttu-id="4b464-170">Fel relaterade till begäranden och undantag</span><span class="sxs-lookup"><span data-stu-id="4b464-170">Exceptions and request failures</span></span>
<span data-ttu-id="4b464-171">Ohanterade undantag samlas in automatiskt:</span><span class="sxs-lookup"><span data-stu-id="4b464-171">Unhandled exceptions are automatically collected:</span></span>

![](./media/app-insights-java-eclipse/21-exceptions.png)

<span data-ttu-id="4b464-172">toocollect data på andra undantag, har du två alternativ:</span><span class="sxs-lookup"><span data-stu-id="4b464-172">toocollect data on other exceptions, you have two options:</span></span>

* <span data-ttu-id="4b464-173">[Infoga anropar tooTrackException i koden](app-insights-api-custom-events-metrics.md#trackexception).</span><span class="sxs-lookup"><span data-stu-id="4b464-173">[Insert calls tooTrackException in your code](app-insights-api-custom-events-metrics.md#trackexception).</span></span>
* <span data-ttu-id="4b464-174">[Installera hello Java-agenten på servern](app-insights-java-agent.md).</span><span class="sxs-lookup"><span data-stu-id="4b464-174">[Install hello Java Agent on your server](app-insights-java-agent.md).</span></span> <span data-ttu-id="4b464-175">Du kan ange hello-metoder som du vill toowatch.</span><span class="sxs-lookup"><span data-stu-id="4b464-175">You specify hello methods you want toowatch.</span></span>

## <a name="monitor-method-calls-and-external-dependencies"></a><span data-ttu-id="4b464-176">Övervaka metodanrop och externa beroenden</span><span class="sxs-lookup"><span data-stu-id="4b464-176">Monitor method calls and external dependencies</span></span>
<span data-ttu-id="4b464-177">[Installera hello agenten Java](app-insights-java-agent.md) toolog angivna interna metoder och anrop som görs via JDBC, med tidsinställning data.</span><span class="sxs-lookup"><span data-stu-id="4b464-177">[Install hello Java Agent](app-insights-java-agent.md) toolog specified internal methods and calls made through JDBC, with timing data.</span></span>

## <a name="performance-counters"></a><span data-ttu-id="4b464-178">Prestandaräknare</span><span class="sxs-lookup"><span data-stu-id="4b464-178">Performance counters</span></span>
<span data-ttu-id="4b464-179">Rulla nedåt på Översikt-bladet och klicka på hello **servrar** panelen.</span><span class="sxs-lookup"><span data-stu-id="4b464-179">On your Overview blade, scroll down and click hello  **Servers** tile.</span></span> <span data-ttu-id="4b464-180">Ser du ett antal prestandaräknare.</span><span class="sxs-lookup"><span data-stu-id="4b464-180">You'll see a range of performance counters.</span></span>

![Rulla nedåt tooclick hello servrar sida vid sida](./media/app-insights-java-eclipse/11-perf-counters.png)

### <a name="customize-performance-counter-collection"></a><span data-ttu-id="4b464-182">Anpassa samlingen med prestandaräknare</span><span class="sxs-lookup"><span data-stu-id="4b464-182">Customize performance counter collection</span></span>
<span data-ttu-id="4b464-183">toodisable samling hello standarduppsättning prestandaräknare, lägger du till följande kod under hello rotnoden hello ApplicationInsights.xml filen hello:</span><span class="sxs-lookup"><span data-stu-id="4b464-183">toodisable collection of hello standard set of performance counters, add hello following code under hello root node of hello ApplicationInsights.xml file:</span></span>

```XML

    <PerformanceCounters>
       <UseBuiltIn>False</UseBuiltIn>
    </PerformanceCounters>
```

### <a name="collect-additional-performance-counters"></a><span data-ttu-id="4b464-184">Samla in data från fler prestandaräknare</span><span class="sxs-lookup"><span data-stu-id="4b464-184">Collect additional performance counters</span></span>
<span data-ttu-id="4b464-185">Du kan ange ytterligare prestandaräknare toobe samlas in.</span><span class="sxs-lookup"><span data-stu-id="4b464-185">You can specify additional performance counters toobe collected.</span></span>

#### <a name="jmx-counters-exposed-by-hello-java-virtual-machine"></a><span data-ttu-id="4b464-186">Räknare för JMX (som exponeras av hello Java Virtual Machine)</span><span class="sxs-lookup"><span data-stu-id="4b464-186">JMX counters (exposed by hello Java Virtual Machine)</span></span>

```XML

    <PerformanceCounters>
      <Jmx>
        <Add objectName="java.lang:type=ClassLoading" attribute="TotalLoadedClassCount" displayName="Loaded Class Count"/>
        <Add objectName="java.lang:type=Memory" attribute="HeapMemoryUsage.used" displayName="Heap Memory Usage-used" type="composite"/>
      </Jmx>
    </PerformanceCounters>
```

* <span data-ttu-id="4b464-187">`displayName`– hello-namnet som visas i hello Application Insights-portalen.</span><span class="sxs-lookup"><span data-stu-id="4b464-187">`displayName` – hello name displayed in hello Application Insights portal.</span></span>
* <span data-ttu-id="4b464-188">`objectName`– hello JMX-objektnamn.</span><span class="sxs-lookup"><span data-stu-id="4b464-188">`objectName` – hello JMX object name.</span></span>
* <span data-ttu-id="4b464-189">`attribute`– hello-attributet för hello JMX objektet namn toofetch</span><span class="sxs-lookup"><span data-stu-id="4b464-189">`attribute` – hello attribute of hello JMX object name toofetch</span></span>
* <span data-ttu-id="4b464-190">`type`(valfritt) – hello JMX objektets attributtyp:</span><span class="sxs-lookup"><span data-stu-id="4b464-190">`type` (optional) - hello type of JMX object’s attribute:</span></span>
  * <span data-ttu-id="4b464-191">Standardvärde: en enkel typ, till exempel int eller long.</span><span class="sxs-lookup"><span data-stu-id="4b464-191">Default: a simple type such as int or long.</span></span>
  * <span data-ttu-id="4b464-192">`composite`: hello prestandaräknardata har formatet hello av 'Attribute.Data'</span><span class="sxs-lookup"><span data-stu-id="4b464-192">`composite`: hello perf counter data is in hello format of 'Attribute.Data'</span></span>
  * <span data-ttu-id="4b464-193">`tabular`: hello prestandaräknardata har hello format för en tabellrad</span><span class="sxs-lookup"><span data-stu-id="4b464-193">`tabular`: hello perf counter data is in hello format of a table row</span></span>

#### <a name="windows-performance-counters"></a><span data-ttu-id="4b464-194">Windows-prestandaräknare</span><span class="sxs-lookup"><span data-stu-id="4b464-194">Windows performance counters</span></span>
<span data-ttu-id="4b464-195">Varje [Windows-prestandaräknare](https://msdn.microsoft.com/library/windows/desktop/aa373083.aspx) är medlem i en kategori (i hello samma sätt som att ett fält är medlem av en klass).</span><span class="sxs-lookup"><span data-stu-id="4b464-195">Each [Windows performance counter](https://msdn.microsoft.com/library/windows/desktop/aa373083.aspx) is a member of a category (in hello same way that a field is a member of a class).</span></span> <span data-ttu-id="4b464-196">Kategorier kan antingen vara globala eller ha numrerade eller namngivna instanser.</span><span class="sxs-lookup"><span data-stu-id="4b464-196">Categories can either be global, or can have numbered or named instances.</span></span>

```XML

    <PerformanceCounters>
      <Windows>
        <Add displayName="Process User Time" categoryName="Process" counterName="%User Time" instanceName="__SELF__" />
        <Add displayName="Bytes Printed per Second" categoryName="Print Queue" counterName="Bytes Printed/sec" instanceName="Fax" />
      </Windows>
    </PerformanceCounters>
```

* <span data-ttu-id="4b464-197">displayName – hello namnet visas i hello Application Insights-portalen.</span><span class="sxs-lookup"><span data-stu-id="4b464-197">displayName – hello name displayed in hello Application Insights portal.</span></span>
* <span data-ttu-id="4b464-198">Kategorinamn – hello prestandaräknarkategorin (prestandaobjekt) som är kopplad till den här prestandaräknaren.</span><span class="sxs-lookup"><span data-stu-id="4b464-198">categoryName – hello performance counter category (performance object) with which this performance counter is associated.</span></span>
* <span data-ttu-id="4b464-199">counterName – hello namn för hello prestandaräknare.</span><span class="sxs-lookup"><span data-stu-id="4b464-199">counterName – hello name of hello performance counter.</span></span>
* <span data-ttu-id="4b464-200">Instansnamn – hello kategori prestandaräknarinstans hello namn eller en tom sträng (””), om hello kategori innehåller en enda instans.</span><span class="sxs-lookup"><span data-stu-id="4b464-200">instanceName – hello name of hello performance counter category instance, or an empty string (""), if hello category contains a single instance.</span></span> <span data-ttu-id="4b464-201">Om hello categoryName är processen och hello prestandaräknaren som toocollect är från hello aktuella JVM-processen på som din app körs, ange `"__SELF__"`.</span><span class="sxs-lookup"><span data-stu-id="4b464-201">If hello categoryName is Process, and hello performance counter you'd like toocollect is from hello current JVM process on which your app is running, specify `"__SELF__"`.</span></span>

<span data-ttu-id="4b464-202">Dina prestandaräknare visas som anpassade mått i [Metrics Explorer][metrics].</span><span class="sxs-lookup"><span data-stu-id="4b464-202">Your performance counters are visible as custom metrics in [Metrics Explorer][metrics].</span></span>

![](./media/app-insights-java-eclipse/12-custom-perfs.png)

### <a name="unix-performance-counters"></a><span data-ttu-id="4b464-203">Unix-prestandaräknare</span><span class="sxs-lookup"><span data-stu-id="4b464-203">Unix performance counters</span></span>
* <span data-ttu-id="4b464-204">[Installera collectd med Application Insights-plugin-programmet hello](app-insights-java-collectd.md) tooget en mängd olika system- och data.</span><span class="sxs-lookup"><span data-stu-id="4b464-204">[Install collectd with hello Application Insights plugin](app-insights-java-collectd.md) tooget a wide variety of system and network data.</span></span>

## <a name="availability-web-tests"></a><span data-ttu-id="4b464-205">Webbtester för tillgänglighet</span><span class="sxs-lookup"><span data-stu-id="4b464-205">Availability web tests</span></span>
<span data-ttu-id="4b464-206">Application Insights kan testa din webbplats på regelbundet toocheck som är det upp och svarar också.</span><span class="sxs-lookup"><span data-stu-id="4b464-206">Application Insights can test your website at regular intervals toocheck that it's up and responding well.</span></span> <span data-ttu-id="4b464-207">[tooset in][availability], bläddra nedåt tooclick tillgänglighet.</span><span class="sxs-lookup"><span data-stu-id="4b464-207">[tooset up][availability], scroll down tooclick Availability.</span></span>

![Rulla ned, klicka på Tillgänglighet och sedan på Lägg till webbtest.](./media/app-insights-java-eclipse/31-config-web-test.png)

<span data-ttu-id="4b464-209">Du kan visa diagram över svarstider, samt få e-postaviseringar om platsen kraschar.</span><span class="sxs-lookup"><span data-stu-id="4b464-209">You'll get charts of response times, plus email notifications if your site goes down.</span></span>

![Exempel på webbtest](./media/app-insights-java-eclipse/appinsights-10webtestresult.png)

<span data-ttu-id="4b464-211">[Läs mer om webbtester för tillgänglighet.][availability]</span><span class="sxs-lookup"><span data-stu-id="4b464-211">[Learn more about availability web tests.][availability]</span></span>

## <a name="diagnostic-logs"></a><span data-ttu-id="4b464-212">Diagnostikloggar</span><span class="sxs-lookup"><span data-stu-id="4b464-212">Diagnostic logs</span></span>
<span data-ttu-id="4b464-213">Om du använder Logback eller Log4J (version 1.2 eller v2.0) för spårning, kan du ha spårloggarna skickas automatiskt tooApplication insikter där du kan utforska och söka i dem.</span><span class="sxs-lookup"><span data-stu-id="4b464-213">If you're using Logback or Log4J (v1.2 or v2.0) for tracing, you can have your trace logs sent automatically tooApplication Insights where you can explore and search on them.</span></span>

<span data-ttu-id="4b464-214">[Mer information om diagnostikloggar][javalogs]</span><span class="sxs-lookup"><span data-stu-id="4b464-214">[Learn more about diagnostic logs][javalogs]</span></span>

## <a name="custom-telemetry"></a><span data-ttu-id="4b464-215">Anpassad telemetri</span><span class="sxs-lookup"><span data-stu-id="4b464-215">Custom telemetry</span></span>
<span data-ttu-id="4b464-216">Infoga ett fåtal rader med kod i Java web application toofind reda på vad användarna gör med den eller toohelp diagnostisera problem.</span><span class="sxs-lookup"><span data-stu-id="4b464-216">Insert a few lines of code in your Java web application toofind out what users are doing with it or toohelp diagnose problems.</span></span>

<span data-ttu-id="4b464-217">Du kan infoga kod både webbsida JavaScript och hello serversidan Java.</span><span class="sxs-lookup"><span data-stu-id="4b464-217">You can insert code both in web page JavaScript and in hello server-side Java.</span></span>

<span data-ttu-id="4b464-218">[Lär dig mer om anpassad telemetri][track]</span><span class="sxs-lookup"><span data-stu-id="4b464-218">[Learn about custom telemetry][track]</span></span>

## <a name="next-steps"></a><span data-ttu-id="4b464-219">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="4b464-219">Next steps</span></span>
#### <a name="detect-and-diagnose-issues"></a><span data-ttu-id="4b464-220">Identifiera och diagnostisera problem</span><span class="sxs-lookup"><span data-stu-id="4b464-220">Detect and diagnose issues</span></span>
* <span data-ttu-id="4b464-221">[Lägg till webbplats klienten telemetri] [ usage] tooget prestanda telemetri från hello webbklienten.</span><span class="sxs-lookup"><span data-stu-id="4b464-221">[Add web client telemetry][usage] tooget performance telemetry from hello web client.</span></span>
* <span data-ttu-id="4b464-222">[Ställ in webbtester] [ availability] toomake säker tillämpningsprogrammet är live och svarstid.</span><span class="sxs-lookup"><span data-stu-id="4b464-222">[Set up web tests][availability] toomake sure your application stays live and responsive.</span></span>
* <span data-ttu-id="4b464-223">[Söka efter händelser och loggar] [ diagnostic] toohelp diagnostisera problem.</span><span class="sxs-lookup"><span data-stu-id="4b464-223">[Search events and logs][diagnostic] toohelp diagnose problems.</span></span>
* <span data-ttu-id="4b464-224">[Fånga in spårningar Log4J eller Logback][javalogs]</span><span class="sxs-lookup"><span data-stu-id="4b464-224">[Capture Log4J or Logback traces][javalogs]</span></span>

#### <a name="track-usage"></a><span data-ttu-id="4b464-225">Spåra användning</span><span class="sxs-lookup"><span data-stu-id="4b464-225">Track usage</span></span>
* <span data-ttu-id="4b464-226">[Lägg till webbplats klienten telemetri] [ usage] toomonitor sidan vyer och grundläggande mått.</span><span class="sxs-lookup"><span data-stu-id="4b464-226">[Add web client telemetry][usage] toomonitor page views and basic user metrics.</span></span>
* <span data-ttu-id="4b464-227">[Spåra anpassade händelser och mått](app-insights-web-track-usage.md) toolearn om hur programmet används både på hello klient- och hello.</span><span class="sxs-lookup"><span data-stu-id="4b464-227">[Track custom events and metrics](app-insights-web-track-usage.md) toolearn about how your application is used, both at hello client and hello server.</span></span>

<!--Link references-->

[availability]: app-insights-monitor-web-app-availability.md
[diagnostic]: app-insights-diagnostic-search.md
[java]: app-insights-java-get-started.md
[javalogs]: app-insights-java-trace-logs.md
[metrics]: app-insights-metrics-explorer.md
[track]: app-insights-api-custom-events-metrics.md
[usage]: app-insights-javascript.md
