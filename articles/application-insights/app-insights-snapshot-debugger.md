---
title: "aaaAzure Application Insights ögonblicksbild felsökare för .NET-appar | Microsoft Docs"
description: "Felsöka ögonblicksbilder automatiskt samlas in när undantag utlöstes i produktion .NET appar"
services: application-insights
documentationcenter: 
author: qubitron
manager: carmonm
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 07/03/2017
ms.author: bwren
ms.openlocfilehash: f0173a752b5795d934fbab1bd53eb077433edc90
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="debug-snapshots-on-exceptions-in-net-apps"></a><span data-ttu-id="b43be-103">Felsöka ögonblicksbilder på undantag i .NET-appar</span><span class="sxs-lookup"><span data-stu-id="b43be-103">Debug snapshots on exceptions in .NET apps</span></span>

<span data-ttu-id="b43be-104">När ett undantag inträffar kan du automatiskt samla in en debug ögonblicksbild webbtillämpningen live.</span><span class="sxs-lookup"><span data-stu-id="b43be-104">When an exception occurs, you can automatically collect a debug snapshot from your live web application.</span></span> <span data-ttu-id="b43be-105">hello ögonblicksbild visar hello tillstånd av källkoden och variabler vid hello ögonblick hello undantag utlöstes.</span><span class="sxs-lookup"><span data-stu-id="b43be-105">hello snapshot shows hello state of source code and variables at hello moment hello exception was thrown.</span></span> <span data-ttu-id="b43be-106">hello ögonblicksbild felsökare (förhandsgranskning) i [Azure Application Insights](app-insights-overview.md) övervakar undantagstelemetri från ditt webbprogram.</span><span class="sxs-lookup"><span data-stu-id="b43be-106">hello Snapshot Debugger (preview) in [Azure Application Insights](app-insights-overview.md) monitors exception telemetry from your web app.</span></span> <span data-ttu-id="b43be-107">Den samlar in ögonblicksbilder på de översta utlösande undantag så att du har hello information du behöver toodiagnose problem i produktion.</span><span class="sxs-lookup"><span data-stu-id="b43be-107">It collects snapshots on your top-throwing exceptions so that you have hello information you need toodiagnose issues in production.</span></span> <span data-ttu-id="b43be-108">Inkludera hello [ögonblicksbild insamlaren NuGet-paketet](http://www.nuget.org/packages/Microsoft.ApplicationInsights.SnapshotCollector) i ditt program och du kan också konfigurera samlingen parametrar i [ApplicationInsights.config](app-insights-configuration-with-applicationinsights-config.md). Ögonblicksbilder visas på [undantag](app-insights-asp-net-exceptions.md) i hello Application Insights-portalen.</span><span class="sxs-lookup"><span data-stu-id="b43be-108">Include hello [Snapshot collector NuGet package](http://www.nuget.org/packages/Microsoft.ApplicationInsights.SnapshotCollector) in your application, and optionally configure collection parameters in [ApplicationInsights.config](app-insights-configuration-with-applicationinsights-config.md). Snapshots appear on [exceptions](app-insights-asp-net-exceptions.md) in hello Application Insights portal.</span></span>

<span data-ttu-id="b43be-109">Du kan visa debug ögonblicksbilder i anropsstacken för hello portal toosee hello och inspektera variabler vid varje anropsstacken..</span><span class="sxs-lookup"><span data-stu-id="b43be-109">You can view debug snapshots in hello portal toosee hello call stack and inspect variables at each call stack frame.</span></span> <span data-ttu-id="b43be-110">tooget en kraftigare felsökning upplevelse med källkoden, öppna ögonblicksbilder med Visual Studio 2017 Enterprise av [hämtar hello ögonblicksbild felsökare tillägget för Visual Studio](https://aka.ms/snapshotdebugger).</span><span class="sxs-lookup"><span data-stu-id="b43be-110">tooget a more powerful debugging experience with source code, open snapshots with Visual Studio 2017 Enterprise by [downloading hello Snapshot Debugger extension for Visual Studio](https://aka.ms/snapshotdebugger).</span></span>

<span data-ttu-id="b43be-111">Ögonblicksbild samlingen är tillgänglig för:</span><span class="sxs-lookup"><span data-stu-id="b43be-111">Snapshot collection is available for:</span></span>
* <span data-ttu-id="b43be-112">.NET framework och ASP.NET-program med .NET Framework 4.5 eller senare.</span><span class="sxs-lookup"><span data-stu-id="b43be-112">.NET Framework and ASP.NET applications running .NET Framework 4.5 or later.</span></span>
* <span data-ttu-id="b43be-113">.NET core 2.0 och ASP.NET Core 2.0 program som körs på Windows.</span><span class="sxs-lookup"><span data-stu-id="b43be-113">.NET Core 2.0 and ASP.NET Core 2.0 applications running on Windows.</span></span>

### <a name="configure-snapshot-collection-for-aspnet-applications"></a><span data-ttu-id="b43be-114">Konfigurera ögonblicksbild samling för ASP.NET-program</span><span class="sxs-lookup"><span data-stu-id="b43be-114">Configure snapshot collection for ASP.NET applications</span></span>

1. <span data-ttu-id="b43be-115">[Aktivera Application Insights i ditt webbprogram](app-insights-asp-net.md), om du inte gjort det ännu.</span><span class="sxs-lookup"><span data-stu-id="b43be-115">[Enable Application Insights in your web app](app-insights-asp-net.md), if you haven't done it yet.</span></span>

2. <span data-ttu-id="b43be-116">Inkludera hello [Microsoft.ApplicationInsights.SnapshotCollector](http://www.nuget.org/packages/Microsoft.ApplicationInsights.SnapshotCollector) NuGet-paketet i din app.</span><span class="sxs-lookup"><span data-stu-id="b43be-116">Include hello [Microsoft.ApplicationInsights.SnapshotCollector](http://www.nuget.org/packages/Microsoft.ApplicationInsights.SnapshotCollector) NuGet package in your app.</span></span> 

3. <span data-ttu-id="b43be-117">Granska hello standardalternativen som hello paket som lagts till för[ApplicationInsights.config](app-insights-configuration-with-applicationinsights-config.md):</span><span class="sxs-lookup"><span data-stu-id="b43be-117">Review hello default options that hello package added too[ApplicationInsights.config](app-insights-configuration-with-applicationinsights-config.md):</span></span>

    ```xml
    <TelemetryProcessors>
        <Add Type="Microsoft.ApplicationInsights.SnapshotCollector.SnapshotCollectorTelemetryProcessor, Microsoft.ApplicationInsights.SnapshotCollector">
        <!-- hello default is true, but you can disable Snapshot Debugging by setting it toofalse -->
        <IsEnabled>true</IsEnabled>
        <!-- Snapshot Debugging is usually disabled in developer mode, but you can enable it by setting this tootrue. -->
        <!-- DeveloperMode is a property on hello active TelemetryChannel. -->
        <IsEnabledInDeveloperMode>false</IsEnabledInDeveloperMode>
        <!-- How many times we need toosee an exception before we ask for snapshots. -->
        <ThresholdForSnapshotting>5</ThresholdForSnapshotting>
        <!-- hello maximum number of examples we create for a single problem. -->
        <MaximumSnapshotsRequired>3</MaximumSnapshotsRequired>
        <!-- hello maximum number of problems that we can be tracking at any time. -->
        <MaximumCollectionPlanSize>50</MaximumCollectionPlanSize>
        <!-- How often tooreset problem counters. -->
        <ProblemCounterResetInterval>06:00:00</ProblemCounterResetInterval>
        <!-- hello maximum number of snapshots allowed in one minute. -->
        <SnapshotsPerMinuteLimit>2</SnapshotsPerMinuteLimit>
        <!-- hello maximum number of snapshots allowed per day. -->
        <SnapshotsPerDayLimit>50</SnapshotsPerDayLimit>
        </Add>
    </TelemetryProcessors>
    ```

4. <span data-ttu-id="b43be-118">Ögonblicksbilder samlas endast för undantag som har rapporterat tooApplication insikter.</span><span class="sxs-lookup"><span data-stu-id="b43be-118">Snapshots are collected only on exceptions that are reported tooApplication Insights.</span></span> <span data-ttu-id="b43be-119">I vissa fall (till exempel äldre versioner av hello .NET-plattformen) kan du behöva för[konfigurera undantag samling](app-insights-asp-net-exceptions.md#exceptions) toosee undantag med ögonblicksbilder i hello-portalen.</span><span class="sxs-lookup"><span data-stu-id="b43be-119">In some cases (for example, older versions of hello .NET platform), you might need too[configure exception collection](app-insights-asp-net-exceptions.md#exceptions) toosee exceptions with snapshots in hello portal.</span></span>


### <a name="configure-snapshot-collection-for-aspnet-core-20-applications"></a><span data-ttu-id="b43be-120">Konfigurera ögonblicksbild samlingen för ASP.NET Core 2.0-program</span><span class="sxs-lookup"><span data-stu-id="b43be-120">Configure snapshot collection for ASP.NET Core 2.0 applications</span></span>

1. <span data-ttu-id="b43be-121">[Aktivera Application Insights i ditt webbprogram för ASP.NET Core](app-insights-asp-net-core.md), om du inte gjort det ännu.</span><span class="sxs-lookup"><span data-stu-id="b43be-121">[Enable Application Insights in your ASP.NET Core web app](app-insights-asp-net-core.md), if you haven't done it yet.</span></span>

2. <span data-ttu-id="b43be-122">Inkludera hello [Microsoft.ApplicationInsights.SnapshotCollector](http://www.nuget.org/packages/Microsoft.ApplicationInsights.SnapshotCollector) NuGet-paketet i din app.</span><span class="sxs-lookup"><span data-stu-id="b43be-122">Include hello [Microsoft.ApplicationInsights.SnapshotCollector](http://www.nuget.org/packages/Microsoft.ApplicationInsights.SnapshotCollector) NuGet package in your app.</span></span>

3. <span data-ttu-id="b43be-123">Ändra hello `ConfigureServices` metod i ditt program `Startup` klassen tooadd hello ögonblicksbild insamlarens telemetri processor.</span><span class="sxs-lookup"><span data-stu-id="b43be-123">Modify hello `ConfigureServices` method in your application's `Startup` class tooadd hello Snapshot Collector's telemetry processor.</span></span> <span data-ttu-id="b43be-124">hello-kod som du bör lägga till beror på hello refererade version av hello Microsoft.ApplicationInsights.ASPNETCore NuGet-paketet.</span><span class="sxs-lookup"><span data-stu-id="b43be-124">hello code you should add depends on hello referenced version of hello Microsoft.ApplicationInsights.ASPNETCore NuGet package.</span></span>

   <span data-ttu-id="b43be-125">Lägg till för Microsoft.ApplicationInsights.AspNetCore 2.1.0:</span><span class="sxs-lookup"><span data-stu-id="b43be-125">For Microsoft.ApplicationInsights.AspNetCore 2.1.0 add:</span></span>
   ```C#
   using Microsoft.ApplicationInsights.SnapshotCollector;
   ...
   class Startup
   {
       // This method is called by hello runtime. Use it tooadd services toohello container.
       public void ConfigureServices(IServiceCollection services)
       {
           services.AddSingleton<Func<ITelemetryProcessor, ITelemetryProcessor>>(next => new SnapshotCollectorTelemetryProcessor(next));
           // TODO: Add any other services your application needs here.
       }
   }
   ```

   <span data-ttu-id="b43be-126">Lägg till för Microsoft.ApplicationInsights.AspNetCore 2.1.1:</span><span class="sxs-lookup"><span data-stu-id="b43be-126">For Microsoft.ApplicationInsights.AspNetCore 2.1.1 add:</span></span>
   ```C#
   using Microsoft.ApplicationInsights.SnapshotCollector;
   ...
   class Startup
   {
       private class SnapshotCollectorTelemetryProcessorFactory : ITelemetryProcessorFactory
       {
           public ITelemetryProcessor Create(ITelemetryProcessor next) =>
               new SnapshotCollectorTelemetryProcessor(next);
       }

       // This method is called by hello runtime. Use it tooadd services toohello container.
       public void ConfigureServices(IServiceCollection services)
       {
            services.AddSingleton<ITelemetryProcessorFactory>(new SnapshotCollectorTelemetryProcessorFactory());
           // TODO: Add any other services your application needs here.
       }
   }
   ```

### <a name="configure-snapshot-collection-for-other-net-applications"></a><span data-ttu-id="b43be-127">Konfigurera ögonblicksbild samling för andra .NET-program</span><span class="sxs-lookup"><span data-stu-id="b43be-127">Configure snapshot collection for other .NET applications</span></span>

1. <span data-ttu-id="b43be-128">Om programmet inte är redan instrumenterats med Application Insights, Kom igång genom att [aktivera Application Insights och inställningen hello instrumentation nyckeln](app-insights-windows-desktop.md).</span><span class="sxs-lookup"><span data-stu-id="b43be-128">If your application is not already instrumented with Application Insights, get started by [enabling Application Insights and setting hello instrumentation key](app-insights-windows-desktop.md).</span></span>

2. <span data-ttu-id="b43be-129">Lägg till hello [Microsoft.ApplicationInsights.SnapshotCollector](http://www.nuget.org/packages/Microsoft.ApplicationInsights.SnapshotCollector) NuGet-paketet i din app.</span><span class="sxs-lookup"><span data-stu-id="b43be-129">Add hello [Microsoft.ApplicationInsights.SnapshotCollector](http://www.nuget.org/packages/Microsoft.ApplicationInsights.SnapshotCollector) NuGet package in your app.</span></span>

3. <span data-ttu-id="b43be-130">Ögonblicksbilder samlas endast för undantag som har rapporterat tooApplication insikter.</span><span class="sxs-lookup"><span data-stu-id="b43be-130">Snapshots are collected only on exceptions that are reported tooApplication Insights.</span></span> <span data-ttu-id="b43be-131">Du kan behöva toomodify din kod tooreport dem.</span><span class="sxs-lookup"><span data-stu-id="b43be-131">You may need toomodify your code tooreport them.</span></span> <span data-ttu-id="b43be-132">hello undantagshantering kod beror på hello strukturen för ditt program, men ett exempel är lägre än:</span><span class="sxs-lookup"><span data-stu-id="b43be-132">hello exception handling code depends on hello structure of your application, but an example is below:</span></span>
    ```C#
   TelemetryClient _telemetryClient = new TelemetryClient();

   void ExampleRequest()
   {
        try
        {
            // TODO: Handle hello request.
        }
        catch (Exception ex)
        {
            // Report hello exception tooApplication Insights.
            _telemetryClient.TrackException(ex);

            // TODO: Rethrow hello exception if desired.
        }
   }
    ```
    
## <a name="grant-permissions"></a><span data-ttu-id="b43be-133">Bevilja behörighet</span><span class="sxs-lookup"><span data-stu-id="b43be-133">Grant permissions</span></span>

<span data-ttu-id="b43be-134">Ägarna av hello Azure-prenumeration kan inspektera ögonblicksbilder.</span><span class="sxs-lookup"><span data-stu-id="b43be-134">Owners of hello Azure subscription can inspect snapshots.</span></span> <span data-ttu-id="b43be-135">Andra användare måste beviljas behörighet genom en ägare.</span><span class="sxs-lookup"><span data-stu-id="b43be-135">Other users must be granted permission by an owner.</span></span>

<span data-ttu-id="b43be-136">toogrant behörighet, tilldela hello `Application Insights Snapshot Debugger` rollen toousers som ska granska ögonblicksbilder.</span><span class="sxs-lookup"><span data-stu-id="b43be-136">toogrant permission, assign hello `Application Insights Snapshot Debugger` role toousers who will inspect snapshots.</span></span> <span data-ttu-id="b43be-137">Den här rollen kan tilldelas tooindividual användare eller grupper av prenumeration ägare för hello mål Application Insights-resurs eller dess resursgrupp eller prenumeration.</span><span class="sxs-lookup"><span data-stu-id="b43be-137">This role can be assigned tooindividual users or groups by subscription owners for hello target Application Insights resource or its resource group or subscription.</span></span>

1. <span data-ttu-id="b43be-138">Öppna bladet för hello Access Control (IAM).</span><span class="sxs-lookup"><span data-stu-id="b43be-138">Open hello Access Control (IAM) blade.</span></span>
1. <span data-ttu-id="b43be-139">Klicka på hello + Lägg till.</span><span class="sxs-lookup"><span data-stu-id="b43be-139">Click hello +Add button.</span></span>
1. <span data-ttu-id="b43be-140">Välj Application Insights ögonblicksbild felsökare hello roller nedrullningsbara listan.</span><span class="sxs-lookup"><span data-stu-id="b43be-140">Select Application Insights Snapshot Debugger from hello Roles drop-down list.</span></span>
1. <span data-ttu-id="b43be-141">Sök efter och ange ett namn för hello användaren tooadd.</span><span class="sxs-lookup"><span data-stu-id="b43be-141">Search for and enter a name for hello user tooadd.</span></span>
1. <span data-ttu-id="b43be-142">Klicka på hello spara knappen tooadd hello toohello användarroll.</span><span class="sxs-lookup"><span data-stu-id="b43be-142">Click hello Save button tooadd hello user toohello role.</span></span>


[!IMPORTANT]
    <span data-ttu-id="b43be-143">Ögonblicksbilder kan innehålla personliga och annan känslig information i variabeln och parametervärden.</span><span class="sxs-lookup"><span data-stu-id="b43be-143">Snapshots can potentially contain personal and other sensitive information in variable and parameter values.</span></span>

## <a name="debug-snapshots-in-hello-application-insights-portal"></a><span data-ttu-id="b43be-144">Felsöka ögonblicksbilder i hello Application Insights-portalen</span><span class="sxs-lookup"><span data-stu-id="b43be-144">Debug snapshots in hello Application Insights portal</span></span>

<span data-ttu-id="b43be-145">Om en ögonblicksbild som är tillgänglig för en viss undantag eller problem-ID, en **öppna Debug ögonblicksbild** visas knappen på hello [undantag](app-insights-asp-net-exceptions.md) i hello Application Insights-portalen.</span><span class="sxs-lookup"><span data-stu-id="b43be-145">If a snapshot is available for a given exception or a problem ID, an **Open Debug Snapshot** button appears on hello [exception](app-insights-asp-net-exceptions.md) in hello Application Insights portal.</span></span>

![Öppna Debug ögonblicksbild knappen undantag](./media/app-insights-snapshot-debugger/snapshot-on-exception.png)

<span data-ttu-id="b43be-147">I hello Debug ögonblicksbild vy visas en anropsstacken och en variabler rutan.</span><span class="sxs-lookup"><span data-stu-id="b43be-147">In hello Debug Snapshot view, you see a call stack and a variables pane.</span></span> <span data-ttu-id="b43be-148">När du väljer ramar av hello anropet stacken i hello anropet stack rutan, kan du visa lokala variabler och parametrar för funktionen anropa i hello variabler rutan.</span><span class="sxs-lookup"><span data-stu-id="b43be-148">When you select frames of hello call stack in hello call stack pane, you can view local variables and parameters for that function call in hello variables pane.</span></span>

![Visa Debug ögonblicksbild i hello-portalen](./media/app-insights-snapshot-debugger/open-snapshot-portal.png)

<span data-ttu-id="b43be-150">Ögonblicksbilder kan innehålla känslig information, och som standard är de inte kan visas.</span><span class="sxs-lookup"><span data-stu-id="b43be-150">Snapshots might contain sensitive information, and by default they are not viewable.</span></span> <span data-ttu-id="b43be-151">tooview ögonblicksbilder, måste du ha hello `Application Insights Snapshot Debugger` tooyou tilldelats rollen.</span><span class="sxs-lookup"><span data-stu-id="b43be-151">tooview snapshots, you must have hello `Application Insights Snapshot Debugger` role assigned tooyou.</span></span>

## <a name="debug-snapshots-with-visual-studio-2017-enterprise"></a><span data-ttu-id="b43be-152">Felsöka ögonblicksbilder med Visual Studio 2017 Enterprise</span><span class="sxs-lookup"><span data-stu-id="b43be-152">Debug snapshots with Visual Studio 2017 Enterprise</span></span>
1. <span data-ttu-id="b43be-153">Klicka på hello **hämta ögonblicksbild** knappen toodownload en `.diagsession` fil som kan öppnas i Visual Studio 2017 Enterprise.</span><span class="sxs-lookup"><span data-stu-id="b43be-153">Click hello **Download Snapshot** button toodownload a `.diagsession` file, which can be opened by Visual Studio 2017 Enterprise.</span></span> 

2. <span data-ttu-id="b43be-154">tooopen hello `.diagsession` -fil, måste du först [ladda ned och installera hello ögonblicksbild felsökare tillägget för Visual Studio](https://aka.ms/snapshotdebugger).</span><span class="sxs-lookup"><span data-stu-id="b43be-154">tooopen hello `.diagsession` file, you must first [download and install hello Snapshot Debugger extension for Visual Studio](https://aka.ms/snapshotdebugger).</span></span>

3. <span data-ttu-id="b43be-155">När du har öppnat hello snapshot-fil visas hello Minidump felsökning i Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="b43be-155">After you open hello snapshot file, hello Minidump Debugging page in Visual Studio appears.</span></span> <span data-ttu-id="b43be-156">Klicka på **Debug förvaltad kod** toostart felsökning hello ögonblicksbild.</span><span class="sxs-lookup"><span data-stu-id="b43be-156">Click **Debug Managed Code** toostart debugging hello snapshot.</span></span> <span data-ttu-id="b43be-157">hello ögonblicksbild öppnas toohello kodrad där hello undantag utlöstes så att du kan felsöka hello hello processen aktuella tillstånd.</span><span class="sxs-lookup"><span data-stu-id="b43be-157">hello snapshot opens toohello line of code where hello exception was thrown so that you can debug hello current state of hello process.</span></span>

    ![Visa debug ögonblicksbild i Visual Studio](./media/app-insights-snapshot-debugger/open-snapshot-visualstudio.png)

<span data-ttu-id="b43be-159">hello hämtade ögonblicksbild innehåller symbolfiler som hittades på webbservern för programmet.</span><span class="sxs-lookup"><span data-stu-id="b43be-159">hello downloaded snapshot contains any symbol files that were found on your web application server.</span></span> <span data-ttu-id="b43be-160">Symbolen är obligatoriska tooassociate ögonblicksbilddata med källkod.</span><span class="sxs-lookup"><span data-stu-id="b43be-160">These symbol files are required tooassociate snapshot data with source code.</span></span> <span data-ttu-id="b43be-161">För Apptjänst-appar kontrollerar du att tooenable symbol distribution när du publicerar dina webbprogram.</span><span class="sxs-lookup"><span data-stu-id="b43be-161">For App Service apps, make sure tooenable symbol deployment when you publish your web apps.</span></span>

## <a name="how-snapshots-work"></a><span data-ttu-id="b43be-162">Så här fungerar ögonblicksbilder</span><span class="sxs-lookup"><span data-stu-id="b43be-162">How snapshots work</span></span>

<span data-ttu-id="b43be-163">När programmet startas skapas en separat ögonblicksbild överföring process som övervakar ditt program för snapshot-begäranden.</span><span class="sxs-lookup"><span data-stu-id="b43be-163">When your application starts, a separate snapshot uploader process is created that monitors your application for snapshot requests.</span></span> <span data-ttu-id="b43be-164">När en ögonblicksbild har begärts görs en skuggkopia av hello körs i cirka 10 too20 minuter.</span><span class="sxs-lookup"><span data-stu-id="b43be-164">When a snapshot is requested, a shadow copy of hello running process is made in about 10 too20 minutes.</span></span> <span data-ttu-id="b43be-165">hello shadow processen analyseras och en ögonblicksbild skapas när hello huvudsakliga processen fortsätter toorun och ger toousers trafik.</span><span class="sxs-lookup"><span data-stu-id="b43be-165">hello shadow process is then analyzed, and a snapshot is created while hello main process continues toorun and serve traffic toousers.</span></span> <span data-ttu-id="b43be-166">hello ögonblicksbild är överförda tooApplication insikter tillsammans med symbolfiler som är relevanta (.pdb) behövs tooview hello ögonblicksbild.</span><span class="sxs-lookup"><span data-stu-id="b43be-166">hello snapshot is then uploaded tooApplication Insights along with any relevant symbol (.pdb) files that are needed tooview hello snapshot.</span></span>

## <a name="current-limitations"></a><span data-ttu-id="b43be-167">Aktuella begränsningar</span><span class="sxs-lookup"><span data-stu-id="b43be-167">Current limitations</span></span>

### <a name="publish-symbols"></a><span data-ttu-id="b43be-168">Publicera symboler</span><span class="sxs-lookup"><span data-stu-id="b43be-168">Publish symbols</span></span>
<span data-ttu-id="b43be-169">hello ögonblicksbild felsökare kräver symbolfiler på hello produktion toodecode servervariabler och tooprovide Avbrottsfritt felsökning i Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="b43be-169">hello Snapshot Debugger requires symbol files on hello production server toodecode variables and tooprovide a debugging experience in Visual Studio.</span></span> <span data-ttu-id="b43be-170">hello 15,2 versionen av Visual Studio 2017 publicerar symboler för versionen versioner som standard när den publicerar tooApp Service.</span><span class="sxs-lookup"><span data-stu-id="b43be-170">hello 15.2 release of Visual Studio 2017 publishes symbols for release builds by default when it publishes tooApp Service.</span></span> <span data-ttu-id="b43be-171">I tidigare versioner måste tooadd hello följande rad tooyour publiceringsprofil `.pubxml` så att symboler publiceras i versionsläge:</span><span class="sxs-lookup"><span data-stu-id="b43be-171">In prior versions, you need tooadd hello following line tooyour publish profile `.pubxml` file so that symbols are published in release mode:</span></span>

```xml
    <ExcludeGeneratedDebugSymbol>False</ExcludeGeneratedDebugSymbol>
```

<span data-ttu-id="b43be-172">För Azure Compute och andra typer, se till att hello symbolfiler hello samma mapp för hello programmets DLL (vanligtvis `wwwroot/bin`) eller som är tillgängliga på hello aktuella sökvägen.</span><span class="sxs-lookup"><span data-stu-id="b43be-172">For Azure Compute and other types, ensure that hello symbol files are in hello same folder of hello main application .dll (typically, `wwwroot/bin`) or are available on hello current path.</span></span>

### <a name="optimized-builds"></a><span data-ttu-id="b43be-173">Optimerad versioner</span><span class="sxs-lookup"><span data-stu-id="b43be-173">Optimized builds</span></span>
<span data-ttu-id="b43be-174">I vissa fall, kan lokala variabler inte visas i versionen versioner på grund av optimeringar som tillämpas under hello build-processen.</span><span class="sxs-lookup"><span data-stu-id="b43be-174">In some cases, local variables cannot be viewed in release builds because of optimizations that are applied during hello build process.</span></span>

## <a name="troubleshooting"></a><span data-ttu-id="b43be-175">Felsökning</span><span class="sxs-lookup"><span data-stu-id="b43be-175">Troubleshooting</span></span>

<span data-ttu-id="b43be-176">De här tipsen hjälper dig att felsöka problem med hello ögonblicksbild felsökare.</span><span class="sxs-lookup"><span data-stu-id="b43be-176">These tips help you troubleshoot problems with hello Snapshot Debugger.</span></span>

### <a name="verify-hello-instrumentation-key"></a><span data-ttu-id="b43be-177">Verifiera hello instrumentation nyckeln</span><span class="sxs-lookup"><span data-stu-id="b43be-177">Verify hello instrumentation key</span></span>

<span data-ttu-id="b43be-178">Kontrollera att du använder hello rätt instrumentation nyckel i ditt publicerade program.</span><span class="sxs-lookup"><span data-stu-id="b43be-178">Make sure that you're using hello correct instrumentation key in your published application.</span></span> <span data-ttu-id="b43be-179">Application Insights läser vanligtvis hello instrumentation nyckeln från hello ApplicationInsights.config-filen.</span><span class="sxs-lookup"><span data-stu-id="b43be-179">Usually, Application Insights reads hello instrumentation key from hello ApplicationInsights.config file.</span></span> <span data-ttu-id="b43be-180">Kontrollera att hello värdet hello samma som hello instrumentation nyckel för hello Application Insights-resurs som du ser i hello-portalen.</span><span class="sxs-lookup"><span data-stu-id="b43be-180">Verify that hello value is hello same as hello instrumentation key for hello Application Insights resource that you see in hello portal.</span></span>

### <a name="check-hello-uploader-logs"></a><span data-ttu-id="b43be-181">Loggarna hello-överförare</span><span class="sxs-lookup"><span data-stu-id="b43be-181">Check hello uploader logs</span></span>

<span data-ttu-id="b43be-182">När du har skapat en ögonblicksbild skapas en minidumpfil (.dmp) på disken.</span><span class="sxs-lookup"><span data-stu-id="b43be-182">After a snapshot is created, a minidump file (.dmp) is created on disk.</span></span> <span data-ttu-id="b43be-183">En separat överförare-process tar minidump filen och överförs, tillsammans med alla associerade PDB-filer, tooApplication insikter ögonblicksbild felsökare lagring.</span><span class="sxs-lookup"><span data-stu-id="b43be-183">A separate uploader process takes that minidump file and uploads it, along with any associated PDBs, tooApplication Insights Snapshot Debugger storage.</span></span> <span data-ttu-id="b43be-184">När hello minidump har laddats bort den från disken.</span><span class="sxs-lookup"><span data-stu-id="b43be-184">After hello minidump has uploaded successfully, it is deleted from disk.</span></span> <span data-ttu-id="b43be-185">hello loggfiler för hello minidump överföring finns kvar på disken.</span><span class="sxs-lookup"><span data-stu-id="b43be-185">hello log files for hello minidump uploader are retained on disk.</span></span> <span data-ttu-id="b43be-186">I en Apptjänst-miljö kan du hitta dessa loggar i `D:\Home\LogFiles\Uploader_*.log`.</span><span class="sxs-lookup"><span data-stu-id="b43be-186">In an App Service environment, you can find these logs in `D:\Home\LogFiles\Uploader_*.log`.</span></span> <span data-ttu-id="b43be-187">Använd hello Kudu hanteringswebbplats för Apptjänst toofind dessa loggfiler.</span><span class="sxs-lookup"><span data-stu-id="b43be-187">Use hello Kudu management site for App Service toofind these log files.</span></span>

1. <span data-ttu-id="b43be-188">Öppna din App-tjänstprogrammet i hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="b43be-188">Open your App Service application in hello Azure portal.</span></span>

2. <span data-ttu-id="b43be-189">Välj hello **avancerade verktyg** bladet eller söka efter **Kudu**.</span><span class="sxs-lookup"><span data-stu-id="b43be-189">Select hello **Advanced Tools** blade, or search for **Kudu**.</span></span>
3. <span data-ttu-id="b43be-190">Klicka på **Gå**.</span><span class="sxs-lookup"><span data-stu-id="b43be-190">Click **Go**.</span></span>
4. <span data-ttu-id="b43be-191">I hello **Felsökningskonsolen** listrutan markerar **CMD**.</span><span class="sxs-lookup"><span data-stu-id="b43be-191">In hello **Debug console** drop-down list box, select **CMD**.</span></span>
5. <span data-ttu-id="b43be-192">Klicka på **loggfiler**.</span><span class="sxs-lookup"><span data-stu-id="b43be-192">Click **LogFiles**.</span></span>

<span data-ttu-id="b43be-193">Du bör se minst en fil med ett namn som börjar med `Uploader_` och en `.log` tillägg.</span><span class="sxs-lookup"><span data-stu-id="b43be-193">You should see at least one file with a name that begins with `Uploader_` and a `.log` extension.</span></span> <span data-ttu-id="b43be-194">Klicka på hello önskad ikon toodownload alla loggfiler eller öppna dem i en webbläsare.</span><span class="sxs-lookup"><span data-stu-id="b43be-194">Click hello appropriate icon toodownload any log files or open them in a browser.</span></span>
<span data-ttu-id="b43be-195">hello-filnamnet innehåller hello datornamnet.</span><span class="sxs-lookup"><span data-stu-id="b43be-195">hello file name includes hello machine name.</span></span> <span data-ttu-id="b43be-196">Om din App Service-instans finns på flera datorer, finns separata loggfiler för varje dator.</span><span class="sxs-lookup"><span data-stu-id="b43be-196">If your App Service instance is hosted on more than one machine, there are separate log files for each machine.</span></span> <span data-ttu-id="b43be-197">När hello överföring upptäcker en ny minidumpfil, registreras den i hello loggfilen.</span><span class="sxs-lookup"><span data-stu-id="b43be-197">When hello uploader detects a new minidump file, it is recorded in hello log file.</span></span> <span data-ttu-id="b43be-198">Här är ett exempel på uppladdningen lyckas:</span><span class="sxs-lookup"><span data-stu-id="b43be-198">Here's an example of a successful upload:</span></span>

```
MinidumpUploader.exe Information: 0 : Dump available 139e411a23934dc0b9ea08a626db16c5.dmp
    DateTime=2017-05-25T14:25:08.0349846Z
MinidumpUploader.exe Information: 0 : Uploading D:\local\Temp\Dumps\c12a605e73c44346a984e00000000000\139e411a23934dc0b9ea08a626db16c5.dmp, 329.12 MB
    DateTime=2017-05-25T14:25:16.0145444Z
MinidumpUploader.exe Information: 0 : Upload successful.
    DateTime=2017-05-25T14:25:42.9164120Z
MinidumpUploader.exe Information: 0 : Extracting PDB info from D:\local\Temp\Dumps\c12a605e73c44346a984e00000000000\139e411a23934dc0b9ea08a626db16c5.dmp.
    DateTime=2017-05-25T14:25:42.9164120Z
MinidumpUploader.exe Information: 0 : Matched 2 PDB(s) with local files.
    DateTime=2017-05-25T14:25:44.2310982Z
MinidumpUploader.exe Information: 0 : Stamp does not want any of our matched PDBs.
    DateTime=2017-05-25T14:25:44.5435948Z
MinidumpUploader.exe Information: 0 : Deleted D:\local\Temp\Dumps\c12a605e73c44346a984e00000000000\139e411a23934dc0b9ea08a626db16c5.dmp
    DateTime=2017-05-25T14:25:44.6095821Z
```

<span data-ttu-id="b43be-199">I föregående exempel hello hello instrumentation nyckeln är `c12a605e73c44346a984e00000000000`.</span><span class="sxs-lookup"><span data-stu-id="b43be-199">In hello previous example, hello instrumentation key is `c12a605e73c44346a984e00000000000`.</span></span> <span data-ttu-id="b43be-200">Det här värdet ska matcha hello instrumentation nyckeln för ditt program.</span><span class="sxs-lookup"><span data-stu-id="b43be-200">This value should match hello instrumentation key for your application.</span></span>
<span data-ttu-id="b43be-201">hello minidump är associerad med en ögonblicksbild med hello ID `139e411a23934dc0b9ea08a626db16c5`.</span><span class="sxs-lookup"><span data-stu-id="b43be-201">hello minidump is associated with a snapshot with hello ID `139e411a23934dc0b9ea08a626db16c5`.</span></span> <span data-ttu-id="b43be-202">Du kan använda detta ID senare toolocate hello associerade undantagstelemetri i Application Insights Analytics.</span><span class="sxs-lookup"><span data-stu-id="b43be-202">You can use this ID later toolocate hello associated exception telemetry in Application Insights Analytics.</span></span>

<span data-ttu-id="b43be-203">Hej överförare söker efter nya PDB-filer om var 15: e minut.</span><span class="sxs-lookup"><span data-stu-id="b43be-203">hello uploader scans for new PDBs about once every 15 minutes.</span></span> <span data-ttu-id="b43be-204">Här är ett exempel:</span><span class="sxs-lookup"><span data-stu-id="b43be-204">Here's an example:</span></span>

```
MinidumpUploader.exe Information: 0 : PDB rescan requested.
    DateTime=2017-05-25T15:11:38.8003886Z
MinidumpUploader.exe Information: 0 : Scanning D:\home\site\wwwroot\ for local PDBs.
    DateTime=2017-05-25T15:11:38.8003886Z
MinidumpUploader.exe Information: 0 : Scanning D:\local\Temporary ASP.NET Files\root\a6554c94\e3ad6f22\assembly\dl3\81d5008b\00b93cc8_dec5d201 for local PDBs.
    DateTime=2017-05-25T15:11:38.8160276Z
MinidumpUploader.exe Information: 0 : Local PDB scan complete. Found 2 PDB(s).
    DateTime=2017-05-25T15:11:38.8316450Z
MinidumpUploader.exe Information: 0 : Deleted PDB scan marker D:\local\Temp\Dumps\c12a605e73c44346a984e00000000000\.pdbscan.
    DateTime=2017-05-25T15:11:38.8316450Z
```

<span data-ttu-id="b43be-205">För program som är _inte_ finns i App Service, hello överföring loggar finns i hello samma mapp som hello minidumpar: `%TEMP%\Dumps\<ikey>` (där `<ikey>` är instrumentation-nyckel).</span><span class="sxs-lookup"><span data-stu-id="b43be-205">For applications that are _not_ hosted in App Service, hello uploader logs are in hello same folder as hello minidumps: `%TEMP%\Dumps\<ikey>` (where `<ikey>` is your instrumentation key).</span></span>

### <a name="use-application-insights-search-toofind-exceptions-with-snapshots"></a><span data-ttu-id="b43be-206">Använd Application Insights söka toofind undantag med ögonblicksbilder</span><span class="sxs-lookup"><span data-stu-id="b43be-206">Use Application Insights search toofind exceptions with snapshots</span></span>

<span data-ttu-id="b43be-207">När en ögonblicksbild skapas är hello att undantag märkta med en ögonblicksbild-ID.</span><span class="sxs-lookup"><span data-stu-id="b43be-207">When a snapshot is created, hello throwing exception is tagged with a snapshot ID.</span></span> <span data-ttu-id="b43be-208">När hello undantagstelemetri rapporterade tooApplication insikter ingår som ögonblicksbilds-ID som en anpassad egenskap.</span><span class="sxs-lookup"><span data-stu-id="b43be-208">When hello exception telemetry is reported tooApplication Insights, that snapshot ID is included as a custom property.</span></span> <span data-ttu-id="b43be-209">Använder hello Sök bladet i Application Insights, du kan hitta all telemetri med hello `ai.snapshot.id` anpassad egenskap.</span><span class="sxs-lookup"><span data-stu-id="b43be-209">Using hello Search blade in Application Insights, you can find all telemetry with hello `ai.snapshot.id` custom property.</span></span>

1. <span data-ttu-id="b43be-210">Bläddra tooyour Application Insights-resurs i hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="b43be-210">Browse tooyour Application Insights resource in hello Azure portal.</span></span>
2. <span data-ttu-id="b43be-211">Klicka på **Sök**.</span><span class="sxs-lookup"><span data-stu-id="b43be-211">Click **Search**.</span></span>
3. <span data-ttu-id="b43be-212">Typen `ai.snapshot.id` i hello sökrutan och tryck på RETUR.</span><span class="sxs-lookup"><span data-stu-id="b43be-212">Type `ai.snapshot.id` in hello Search text box and press Enter.</span></span>

![Sök efter telemetri med en ögonblicksbild ID i hello-portalen](./media/app-insights-snapshot-debugger/search-snapshot-portal.png)

<span data-ttu-id="b43be-214">Om den här sökningen returnerar inga resultat har inga rapporterade tooApplication insikter för ditt program i hello valt tidsintervall.</span><span class="sxs-lookup"><span data-stu-id="b43be-214">If this search returns no results, then no snapshots were reported tooApplication Insights for your application in hello selected time range.</span></span>

<span data-ttu-id="b43be-215">toosearch för en specifik ögonblicksbilds-ID från hello överföring loggar skriver detta ID i hello sökrutan.</span><span class="sxs-lookup"><span data-stu-id="b43be-215">toosearch for a specific snapshot ID from hello Uploader logs, type that ID in hello Search box.</span></span> <span data-ttu-id="b43be-216">Om du inte hittar telemetri för en ögonblicksbild som du vet överfördes, gör du följande:</span><span class="sxs-lookup"><span data-stu-id="b43be-216">If you can't find telemetry for a snapshot that you know was uploaded, follow these steps:</span></span>

1. <span data-ttu-id="b43be-217">Kontrollera att du tittar på hello rätt Application Insights-resursen genom att verifiera hello instrumentation nyckel.</span><span class="sxs-lookup"><span data-stu-id="b43be-217">Double-check that you're looking at hello right Application Insights resource by verifying hello instrumentation key.</span></span>

2. <span data-ttu-id="b43be-218">Med hello tidsstämpel från hello överföring loggen kan justera hello tidsintervall filter för hello Sök toocover att tidsintervallet för rapporten.</span><span class="sxs-lookup"><span data-stu-id="b43be-218">Using hello timestamp from hello Uploader log, adjust hello Time Range filter of hello search toocover that time range.</span></span>

<span data-ttu-id="b43be-219">Om du fortfarande inte ser ett undantag med detta ID ögonblicksbild inte hello undantagstelemetri rapporterade tooApplication insikter.</span><span class="sxs-lookup"><span data-stu-id="b43be-219">If you still don't see an exception with that snapshot ID, then hello exception telemetry wasn't reported tooApplication Insights.</span></span> <span data-ttu-id="b43be-220">Detta kan inträffa om programmet kraschade när det tog hello ögonblicksbild men innan den rapporterades hello undantagstelemetri.</span><span class="sxs-lookup"><span data-stu-id="b43be-220">This situation can happen if your application crashed after it took hello snapshot but before it reported hello exception telemetry.</span></span> <span data-ttu-id="b43be-221">I det här fallet Kontrollera hello Apptjänst loggar under `Diagnose and solve problems` toosee om det fanns oväntat startar om eller ohanterade undantag.</span><span class="sxs-lookup"><span data-stu-id="b43be-221">In this case, check hello App Service logs under `Diagnose and solve problems` toosee if there were unexpected restarts or unhandled exceptions.</span></span>

## <a name="next-steps"></a><span data-ttu-id="b43be-222">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="b43be-222">Next steps</span></span>

* <span data-ttu-id="b43be-223">[Ange snappoints i koden](https://azure.microsoft.com/blog/snapshot-debugger-for-azure/) tooget ögonblicksbilder utan att vänta på ett undantag.</span><span class="sxs-lookup"><span data-stu-id="b43be-223">[Set snappoints in your code](https://azure.microsoft.com/blog/snapshot-debugger-for-azure/) tooget snapshots without waiting for an exception.</span></span>
* <span data-ttu-id="b43be-224">[Diagnostisera undantag i web apps](app-insights-asp-net-exceptions.md) förklarar hur toomake flera undantag synliga tooApplication insikter.</span><span class="sxs-lookup"><span data-stu-id="b43be-224">[Diagnose exceptions in your web apps](app-insights-asp-net-exceptions.md) explains how toomake more exceptions visible tooApplication Insights.</span></span> 
* <span data-ttu-id="b43be-225">[Identifiering för smartkort](app-insights-proactive-diagnostics.md) upptäcker automatiskt prestandaavvikelser.</span><span class="sxs-lookup"><span data-stu-id="b43be-225">[Smart Detection](app-insights-proactive-diagnostics.md) automatically discovers performance anomalies.</span></span>
