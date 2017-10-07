---
title: aaaMonitor Azure web app prestanda | Microsoft Docs
description: "Övervakning av programprestanda för Azure-webbappar. Skapa diagram över inläsnings- och svarstider och beroendeinformation och ställ in aviseringar för prestanda."
services: application-insights
documentationcenter: .net
author: CFreemanwa
manager: carmonm
ms.assetid: 0b2deb30-6ea8-4bc4-8ed0-26765b85149f
ms.service: azure-portal
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 05/05/2017
ms.author: bwren
ms.openlocfilehash: d1083254e5c504b18f2ac5ae2368610dc2790436
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="monitor-azure-web-app-performance"></a><span data-ttu-id="1795e-104">Övervaka prestanda för Azure-webbappar</span><span class="sxs-lookup"><span data-stu-id="1795e-104">Monitor Azure web app performance</span></span>
<span data-ttu-id="1795e-105">I hello [Azure Portal](https://portal.azure.com) du kan konfigurera övervakning av programprestanda för din [Azure-webbappar](../app-service-web/app-service-web-overview.md).</span><span class="sxs-lookup"><span data-stu-id="1795e-105">In hello [Azure Portal](https://portal.azure.com) you can set up application performance monitoring for your [Azure web apps](../app-service-web/app-service-web-overview.md).</span></span> <span data-ttu-id="1795e-106">[Azure Application Insights](app-insights-overview.md) instruments din app toosend telemetri om dess aktiviteter toohello Application Insights-tjänsten, där den lagras och analyseras.</span><span class="sxs-lookup"><span data-stu-id="1795e-106">[Azure Application Insights](app-insights-overview.md) instruments your app toosend telemetry about its activities toohello Application Insights service, where it is stored and analyzed.</span></span> <span data-ttu-id="1795e-107">Det, mått diagram och sökverktyg kan användas för toohelp diagnostisera problem, förbättra prestanda och utvärdera användning.</span><span class="sxs-lookup"><span data-stu-id="1795e-107">There, metric charts and search tools can be used toohelp diagnose issues, improve performance, and assess usage.</span></span>

## <a name="run-time-or-build-time"></a><span data-ttu-id="1795e-108">Vid körning eller utveckling</span><span class="sxs-lookup"><span data-stu-id="1795e-108">Run time or build time</span></span>
<span data-ttu-id="1795e-109">Du kan konfigurera övervakning av instrumentering hello app på två sätt:</span><span class="sxs-lookup"><span data-stu-id="1795e-109">You can configure monitoring by instrumenting hello app in either of two ways:</span></span>

* <span data-ttu-id="1795e-110">**Vid körning** – Du kan välja ett tillägg för prestandaövervakning när din webbapp redan är live.</span><span class="sxs-lookup"><span data-stu-id="1795e-110">**Run-time** - You can select a performance monitoring extension when your web app is already live.</span></span> <span data-ttu-id="1795e-111">Det är inte nödvändigt toorebuild eller installera om appen.</span><span class="sxs-lookup"><span data-stu-id="1795e-111">It isn't necessary toorebuild or re-install your app.</span></span> <span data-ttu-id="1795e-112">Du får en standarduppsättning med paket som övervakar svarstider, framgångsfrekvens, undantag, beroenden och så vidare.</span><span class="sxs-lookup"><span data-stu-id="1795e-112">You get a standard set of packages that monitor response times, success rates, exceptions, dependencies, and so on.</span></span> 
* <span data-ttu-id="1795e-113">**Vid utveckling** – Du kan installera ett paket i din app i samband med utvecklingen.</span><span class="sxs-lookup"><span data-stu-id="1795e-113">**Build time** - You can install a package in your app in development.</span></span> <span data-ttu-id="1795e-114">Det här alternativet är mer flexibelt.</span><span class="sxs-lookup"><span data-stu-id="1795e-114">This option is more versatile.</span></span> <span data-ttu-id="1795e-115">I tillägg toohello samma standard paket, du kan skriva kod toocustomize hello telemetri eller toosend egna telemetri.</span><span class="sxs-lookup"><span data-stu-id="1795e-115">In addition toohello same standard packages, you can write code toocustomize hello telemetry or toosend your own telemetry.</span></span> <span data-ttu-id="1795e-116">Du kan logga specifika aktiviteter eller post händelser enligt toohello semantiken för din app-domän.</span><span class="sxs-lookup"><span data-stu-id="1795e-116">You can log specific activities or record events according toohello semantics of your app domain.</span></span> 

## <a name="run-time-instrumentation-with-application-insights"></a><span data-ttu-id="1795e-117">Instrumentering i samband med körning med Application Insights</span><span class="sxs-lookup"><span data-stu-id="1795e-117">Run time instrumentation with Application Insights</span></span>
<span data-ttu-id="1795e-118">Om du redan kör en webbapp i Azure har du redan tillgång till viss övervakning: begärande- och felfrekvens.</span><span class="sxs-lookup"><span data-stu-id="1795e-118">If you're already running a web app in Azure, you already get some monitoring: request and error rates.</span></span> <span data-ttu-id="1795e-119">Lägg till Application Insights tooget mer, till exempel svarstider, övervakning anrop toodependencies, smart identifiering och hello kraftfulla Log Analytics-frågespråket.</span><span class="sxs-lookup"><span data-stu-id="1795e-119">Add Application Insights tooget more, such as response times, monitoring calls toodependencies, smart detection, and hello powerful Log Analytics query language.</span></span> 

1. <span data-ttu-id="1795e-120">**Välj Application Insights** hello Azure på Kontrollpanelen för ditt webbprogram.</span><span class="sxs-lookup"><span data-stu-id="1795e-120">**Select Application Insights** in hello Azure control panel for your web app.</span></span>
   
    ![Välj Application Insights under Övervakning](./media/app-insights-azure-web-apps/05-extend.png)
   
   * <span data-ttu-id="1795e-122">Välj toocreate en ny resurs, såvida du inte redan angett Application Insights-resurs för den här appen med en annan väg.</span><span class="sxs-lookup"><span data-stu-id="1795e-122">Choose toocreate a new resource, unless you already set up an Application Insights resource for this app by another route.</span></span>
2. <span data-ttu-id="1795e-123">**Instrumentera din webbapp** när Application Insights har installerats.</span><span class="sxs-lookup"><span data-stu-id="1795e-123">**Instrument your web app** after Application Insights has been installed.</span></span> 
   
    ![Instrumentera din webbapp](./media/app-insights-azure-web-apps/restart-web-app-for-insights.png)

   <span data-ttu-id="1795e-125">**Aktivera övervakning på klientsidan** för sidvy och användartelemetri.</span><span class="sxs-lookup"><span data-stu-id="1795e-125">**Enable client side monitoring** for page view and user telemetry.</span></span>

   * <span data-ttu-id="1795e-126">Välj Inställningar > Programinställningar</span><span class="sxs-lookup"><span data-stu-id="1795e-126">Select Settings > Application Settings</span></span>
   * <span data-ttu-id="1795e-127">Under Appinställningar lägger du till ett nytt nyckel/värde-par:</span><span class="sxs-lookup"><span data-stu-id="1795e-127">Under App Settings, add a new key value pair:</span></span> 
   
    <span data-ttu-id="1795e-128">Nyckel: `APPINSIGHTS_JAVASCRIPT_ENABLED`</span><span class="sxs-lookup"><span data-stu-id="1795e-128">Key: `APPINSIGHTS_JAVASCRIPT_ENABLED`</span></span> 
    
    <span data-ttu-id="1795e-129">Värde:`true`
</span><span class="sxs-lookup"><span data-stu-id="1795e-129">Value: `true`</span></span>
   * <span data-ttu-id="1795e-130">**Spara** hello inställningar och **starta om** din app.</span><span class="sxs-lookup"><span data-stu-id="1795e-130">**Save** hello settings and **Restart** your app.</span></span>
3. <span data-ttu-id="1795e-131">**Övervaka din app**.</span><span class="sxs-lookup"><span data-stu-id="1795e-131">**Monitor your app**.</span></span>  <span data-ttu-id="1795e-132">[Expore hello data](#explore-the-data).</span><span class="sxs-lookup"><span data-stu-id="1795e-132">[Expore hello data](#explore-the-data).</span></span>

<span data-ttu-id="1795e-133">Senare kan skapa du hello app med Application Insights om du vill.</span><span class="sxs-lookup"><span data-stu-id="1795e-133">Later, you can build hello app with Application Insights if you want.</span></span>

<span data-ttu-id="1795e-134">*Hur jag ta bort Application Insights eller växla toosending tooanother resurs?*</span><span class="sxs-lookup"><span data-stu-id="1795e-134">*How do I remove Application Insights, or switch toosending tooanother resource?*</span></span>

* <span data-ttu-id="1795e-135">I Azure, öppna hello web app kontroll bladet och under utvecklingsverktyg, öppna **tillägg**.</span><span class="sxs-lookup"><span data-stu-id="1795e-135">In Azure, open hello web app control blade, and under Development Tools, open **Extensions**.</span></span> <span data-ttu-id="1795e-136">Ta bort hello Application Insights-tillägget.</span><span class="sxs-lookup"><span data-stu-id="1795e-136">Delete hello Application Insights extension.</span></span> <span data-ttu-id="1795e-137">Sedan under övervakning, Välj Application Insights och skapa eller välj hello-resurs som du vill.</span><span class="sxs-lookup"><span data-stu-id="1795e-137">Then under Monitoring, choose Application Insights and create or select hello resource you want.</span></span>

## <a name="build-hello-app-with-application-insights"></a><span data-ttu-id="1795e-138">Skapa hello program med Application Insights</span><span class="sxs-lookup"><span data-stu-id="1795e-138">Build hello app with Application Insights</span></span>
<span data-ttu-id="1795e-139">Application Insights kan tillhandahålla mer detaljerad telemetri genom installationen av ett SDK i din app.</span><span class="sxs-lookup"><span data-stu-id="1795e-139">Application Insights can provide more detailed telemetry by installing an SDK into your app.</span></span> <span data-ttu-id="1795e-140">Mer specifikt kan du samla in spårningsloggar, [skriva anpassad telemetri](app-insights-api-custom-events-metrics.md) och få mer detaljerade undantagsrapporter.</span><span class="sxs-lookup"><span data-stu-id="1795e-140">In particular, you can collect trace logs, [write custom telemetry](app-insights-api-custom-events-metrics.md), and get more detailed exception reports.</span></span>

1. <span data-ttu-id="1795e-141">**I Visual Studio** (2013 uppdatering 2 eller senare) konfigurerar du Application Insights för ditt projekt.</span><span class="sxs-lookup"><span data-stu-id="1795e-141">**In Visual Studio** (2013 update 2 or later), configure Application Insights for your project.</span></span>

    <span data-ttu-id="1795e-142">Högerklicka på hello-projektet och välj **Lägg till > Application Insights** eller **konfigurera Application Insights**.</span><span class="sxs-lookup"><span data-stu-id="1795e-142">Right-click hello web project, and select **Add > Application Insights** or **Configure Application Insights**.</span></span>
   
    ![Högerklicka på hello-projektet och välj Lägg till eller konfigurera Application Insights](./media/app-insights-azure-web-apps/03-add.png)
   
    <span data-ttu-id="1795e-144">Om du tillfrågas toosign i Använd hello-autentiseringsuppgifter för Azure-konto.</span><span class="sxs-lookup"><span data-stu-id="1795e-144">If you're asked toosign in, use hello credentials for your Azure account.</span></span>
   
    <span data-ttu-id="1795e-145">hello-åtgärden har två effekter:</span><span class="sxs-lookup"><span data-stu-id="1795e-145">hello operation has two effects:</span></span>
   
   1. <span data-ttu-id="1795e-146">Skapar en Application Insights-resurs i Azure, där telemetri lagras, analyseras och visas.</span><span class="sxs-lookup"><span data-stu-id="1795e-146">Creates an Application Insights resource in Azure, where telemetry is stored, analyzed and displayed.</span></span>
   2. <span data-ttu-id="1795e-147">Lägger till hello Application Insights NuGet-paketet tooyour kod (om det inte fanns redan), samt konfigurerar den toosend telemetri toohello Azure-resurs.</span><span class="sxs-lookup"><span data-stu-id="1795e-147">Adds hello Application Insights NuGet package tooyour code (if it isn't there already), and configures it toosend telemetry toohello Azure resource.</span></span>
2. <span data-ttu-id="1795e-148">**Testa hello telemetri** av hello-app som körs på utvecklingsdatorn (F5).</span><span class="sxs-lookup"><span data-stu-id="1795e-148">**Test hello telemetry** by running hello app in your development machine (F5).</span></span>
3. <span data-ttu-id="1795e-149">**Publicera hello app** tooAzure i hello vanligt.</span><span class="sxs-lookup"><span data-stu-id="1795e-149">**Publish hello app** tooAzure in hello usual way.</span></span> 

<span data-ttu-id="1795e-150">*Hur växlar toosending tooa olika Application Insights-resurs?*</span><span class="sxs-lookup"><span data-stu-id="1795e-150">*How do I switch toosending tooa different Application Insights resource?*</span></span>

* <span data-ttu-id="1795e-151">I Visual Studio, högerklicka på hello projekt, väljer **konfigurera Application Insights** och välj hello-resurs som du vill.</span><span class="sxs-lookup"><span data-stu-id="1795e-151">In Visual Studio, right-click hello project, choose **Configure Application Insights** and choose hello resource you want.</span></span> <span data-ttu-id="1795e-152">Du får hello alternativet toocreate en ny resurs.</span><span class="sxs-lookup"><span data-stu-id="1795e-152">You get hello option toocreate a new resource.</span></span> <span data-ttu-id="1795e-153">Återskapa och distribuera igen.</span><span class="sxs-lookup"><span data-stu-id="1795e-153">Rebuild and redeploy.</span></span>

## <a name="explore-hello-data"></a><span data-ttu-id="1795e-154">Utforska hello data</span><span class="sxs-lookup"><span data-stu-id="1795e-154">Explore hello data</span></span>
1. <span data-ttu-id="1795e-155">Hello Application Insights bladet av Kontrollpanelen web app visas Live statistik, som visar begäranden och misslyckade inom en andra eller två av dem sker.</span><span class="sxs-lookup"><span data-stu-id="1795e-155">On hello Application Insights blade of your web app control panel, you see Live Metrics, which shows requests and failures within a second or two of them occurring.</span></span> <span data-ttu-id="1795e-156">Detta är praktiskt när du publicera om din app, så att du genast ser eventuella problem.</span><span class="sxs-lookup"><span data-stu-id="1795e-156">It's very useful display when you're republishing your app - you can see any problems immediately.</span></span>
2. <span data-ttu-id="1795e-157">Klicka dig igenom toohello fullständig Application Insights-resurs.</span><span class="sxs-lookup"><span data-stu-id="1795e-157">Click through toohello full Application Insights resource.</span></span>

    ![Klicka dig framåt](./media/app-insights-azure-web-apps/view-in-application-insights.png)

    <span data-ttu-id="1795e-159">Du kan också gå dit direkt från Azure-resursnavigeringen.</span><span class="sxs-lookup"><span data-stu-id="1795e-159">You can also go there either directly from Azure resource navigation.</span></span>

1. <span data-ttu-id="1795e-160">Klicka dig igenom några diagram tooget detalj:</span><span class="sxs-lookup"><span data-stu-id="1795e-160">Click through any chart tooget more detail:</span></span>
   
    ![Klicka på ett diagram på hello Application Insights översikt blad](./media/app-insights-azure-web-apps/07-dependency.png)
   
    <span data-ttu-id="1795e-162">Du kan [anpassa bladen med mätvärden](app-insights-metrics-explorer.md).</span><span class="sxs-lookup"><span data-stu-id="1795e-162">You can [customize metrics blades](app-insights-metrics-explorer.md).</span></span>
2. <span data-ttu-id="1795e-163">Klicka dig igenom ytterligare toosee enskilda händelser och deras egenskaper:</span><span class="sxs-lookup"><span data-stu-id="1795e-163">Click through further toosee individual events and their properties:</span></span>
   
    ![Klicka på en händelse typen tooopen en sökning filtrerade i denna typ.](./media/app-insights-azure-web-apps/08-requests.png)
   
    <span data-ttu-id="1795e-165">Observera hello ”...” länken tooopen alla egenskaper.</span><span class="sxs-lookup"><span data-stu-id="1795e-165">Notice hello "..." link tooopen all properties.</span></span>
   
    <span data-ttu-id="1795e-166">Du kan [anpassa sökningar](app-insights-diagnostic-search.md).</span><span class="sxs-lookup"><span data-stu-id="1795e-166">You can [customize searches](app-insights-diagnostic-search.md).</span></span>

<span data-ttu-id="1795e-167">För mer avancerade sökningar över din telemetri använder hello [Log Analytics-frågespråket](app-insights-analytics-tour.md).</span><span class="sxs-lookup"><span data-stu-id="1795e-167">For more powerful searches over your telemetry, use hello [Log Analytics query language](app-insights-analytics-tour.md).</span></span>

## <a name="more-telemetry"></a><span data-ttu-id="1795e-168">Mer telemetri</span><span class="sxs-lookup"><span data-stu-id="1795e-168">More telemetry</span></span>

* [<span data-ttu-id="1795e-169">Data om webbsidesinläsning</span><span class="sxs-lookup"><span data-stu-id="1795e-169">Web page load data</span></span>](app-insights-javascript.md)
* [<span data-ttu-id="1795e-170">Anpassad telemetri</span><span class="sxs-lookup"><span data-stu-id="1795e-170">Custom telemetry</span></span>](app-insights-api-custom-events-metrics.md)

## <a name="video"></a><span data-ttu-id="1795e-171">Video</span><span class="sxs-lookup"><span data-stu-id="1795e-171">Video</span></span>

> [!VIDEO https://channel9.msdn.com/events/Connect/2016/100/player]

## <a name="next-steps"></a><span data-ttu-id="1795e-172">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="1795e-172">Next steps</span></span>
* <span data-ttu-id="1795e-173">[Kör hello profiler på appen live](app-insights-profiler.md).</span><span class="sxs-lookup"><span data-stu-id="1795e-173">[Run hello profiler on your live app](app-insights-profiler.md).</span></span>
* <span data-ttu-id="1795e-174">[Azure Functions](https://github.com/christopheranderson/azure-functions-app-insights-sample) – övervaka Azure Functions med Application Insights</span><span class="sxs-lookup"><span data-stu-id="1795e-174">[Azure Functions](https://github.com/christopheranderson/azure-functions-app-insights-sample) - monitor Azure Functions with Application Insights</span></span>
* <span data-ttu-id="1795e-175">[Aktivera Azure diagnostics](app-insights-azure-diagnostics.md) toobe skickas tooApplication insikter.</span><span class="sxs-lookup"><span data-stu-id="1795e-175">[Enable Azure diagnostics](app-insights-azure-diagnostics.md) toobe sent tooApplication Insights.</span></span>
* <span data-ttu-id="1795e-176">[Övervaka tjänsten hälsa](../monitoring-and-diagnostics/insights-how-to-customize-monitoring.md) toomake att tjänsten är tillgänglig och svarstid.</span><span class="sxs-lookup"><span data-stu-id="1795e-176">[Monitor service health metrics](../monitoring-and-diagnostics/insights-how-to-customize-monitoring.md) toomake sure your service is available and responsive.</span></span>
* <span data-ttu-id="1795e-177">[Få aviseringar](../monitoring-and-diagnostics/insights-receive-alert-notifications.md) när drifthändelser inträffar eller när mätvärden överskrider ett tröskelvärde.</span><span class="sxs-lookup"><span data-stu-id="1795e-177">[Receive alert notifications](../monitoring-and-diagnostics/insights-receive-alert-notifications.md) whenever operational events happen or metrics cross a threshold.</span></span>
* <span data-ttu-id="1795e-178">Använd [Programinsikter för JavaScript-appar och webbsidor](app-insights-javascript.md) tooget klienten telemetri från hello webbläsare som besöker en webbsida.</span><span class="sxs-lookup"><span data-stu-id="1795e-178">Use [Application Insights for JavaScript apps and web pages](app-insights-javascript.md) tooget client telemetry from hello browsers that visit a web page.</span></span>
* <span data-ttu-id="1795e-179">[Ställ in tillgänglighet webbtester](app-insights-monitor-web-app-availability.md) toobe aviserad om webbplatsen är igång.</span><span class="sxs-lookup"><span data-stu-id="1795e-179">[Set up Availability web tests](app-insights-monitor-web-app-availability.md) toobe alerted if your site is down.</span></span>

