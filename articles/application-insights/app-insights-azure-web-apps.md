---
title: "Övervaka prestanda för Azure-webbappar | Microsoft Docs"
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
ms.openlocfilehash: f2bbadfbcb93873ed910aeff050bd6896e2e8fec
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/18/2017
---
# <a name="monitor-azure-web-app-performance"></a><span data-ttu-id="8c9a5-104">Övervaka prestanda för Azure-webbappar</span><span class="sxs-lookup"><span data-stu-id="8c9a5-104">Monitor Azure web app performance</span></span>
<span data-ttu-id="8c9a5-105">På [Azure Portal](https://portal.azure.com) kan du konfigurera övervakning av programprestanda för dina [Azure-webbappar](../app-service-web/app-service-web-overview.md).</span><span class="sxs-lookup"><span data-stu-id="8c9a5-105">In the [Azure Portal](https://portal.azure.com) you can set up application performance monitoring for your [Azure web apps](../app-service-web/app-service-web-overview.md).</span></span> <span data-ttu-id="8c9a5-106">[Azure Application Insights](app-insights-overview.md) instrumenterar din app så att den skickar telemetri om sina aktiviteter till Application Insights-tjänsten, där informationen lagras och analyseras.</span><span class="sxs-lookup"><span data-stu-id="8c9a5-106">[Azure Application Insights](app-insights-overview.md) instruments your app to send telemetry about its activities to the Application Insights service, where it is stored and analyzed.</span></span> <span data-ttu-id="8c9a5-107">Där kan du använda diagram med mätvärden och sökverktyg för att diagnostisera problem, förbättra prestanda och utvärdera användningen.</span><span class="sxs-lookup"><span data-stu-id="8c9a5-107">There, metric charts and search tools can be used to help diagnose issues, improve performance, and assess usage.</span></span>

## <a name="run-time-or-build-time"></a><span data-ttu-id="8c9a5-108">Vid körning eller utveckling</span><span class="sxs-lookup"><span data-stu-id="8c9a5-108">Run time or build time</span></span>
<span data-ttu-id="8c9a5-109">Du kan konfigurera övervakning genom att instrumentera appen på något av två sätt:</span><span class="sxs-lookup"><span data-stu-id="8c9a5-109">You can configure monitoring by instrumenting the app in either of two ways:</span></span>

* <span data-ttu-id="8c9a5-110">**Vid körning** – Du kan välja ett tillägg för prestandaövervakning när din webbapp redan är live.</span><span class="sxs-lookup"><span data-stu-id="8c9a5-110">**Run-time** - You can select a performance monitoring extension when your web app is already live.</span></span> <span data-ttu-id="8c9a5-111">Du behöver inte återskapa eller installera om appen.</span><span class="sxs-lookup"><span data-stu-id="8c9a5-111">It isn't necessary to rebuild or re-install your app.</span></span> <span data-ttu-id="8c9a5-112">Du får en standarduppsättning med paket som övervakar svarstider, framgångsfrekvens, undantag, beroenden och så vidare.</span><span class="sxs-lookup"><span data-stu-id="8c9a5-112">You get a standard set of packages that monitor response times, success rates, exceptions, dependencies, and so on.</span></span> 
* <span data-ttu-id="8c9a5-113">**Vid utveckling** – Du kan installera ett paket i din app i samband med utvecklingen.</span><span class="sxs-lookup"><span data-stu-id="8c9a5-113">**Build time** - You can install a package in your app in development.</span></span> <span data-ttu-id="8c9a5-114">Det här alternativet är mer flexibelt.</span><span class="sxs-lookup"><span data-stu-id="8c9a5-114">This option is more versatile.</span></span> <span data-ttu-id="8c9a5-115">Förutom motsvarande standardpaket kan du skriva kod för att anpassa telemetrin eller skicka din egen telemetri.</span><span class="sxs-lookup"><span data-stu-id="8c9a5-115">In addition to the same standard packages, you can write code to customize the telemetry or to send your own telemetry.</span></span> <span data-ttu-id="8c9a5-116">Du kan logga specifika aktiviteter eller registrera händelser baserat på semantiken för din appdomän.</span><span class="sxs-lookup"><span data-stu-id="8c9a5-116">You can log specific activities or record events according to the semantics of your app domain.</span></span> 

## <a name="run-time-instrumentation-with-application-insights"></a><span data-ttu-id="8c9a5-117">Instrumentering i samband med körning med Application Insights</span><span class="sxs-lookup"><span data-stu-id="8c9a5-117">Run time instrumentation with Application Insights</span></span>
<span data-ttu-id="8c9a5-118">Om du redan kör en webbapp i Azure har du redan tillgång till viss övervakning: begärande- och felfrekvens.</span><span class="sxs-lookup"><span data-stu-id="8c9a5-118">If you're already running a web app in Azure, you already get some monitoring: request and error rates.</span></span> <span data-ttu-id="8c9a5-119">Lägg till Application Insights om du vill få tillgång till mer, till exempel svarstider, övervakning av anrop till beroenden, smart identifiering och det kraftfulla Log Analytics-frågespråket.</span><span class="sxs-lookup"><span data-stu-id="8c9a5-119">Add Application Insights to get more, such as response times, monitoring calls to dependencies, smart detection, and the powerful Log Analytics query language.</span></span> 

1. <span data-ttu-id="8c9a5-120">**Välj Application Insights** på Azure-kontrollpanelen för din webbapp.</span><span class="sxs-lookup"><span data-stu-id="8c9a5-120">**Select Application Insights** in the Azure control panel for your web app.</span></span>
   
    ![Välj Application Insights under Övervakning](./media/app-insights-azure-web-apps/05-extend.png)
   
   * <span data-ttu-id="8c9a5-122">Välj att skapa en ny resurs, såvida du inte redan har angett en Application Insights-resurs för den här appen på ett annat sätt.</span><span class="sxs-lookup"><span data-stu-id="8c9a5-122">Choose to create a new resource, unless you already set up an Application Insights resource for this app by another route.</span></span>
2. <span data-ttu-id="8c9a5-123">**Instrumentera din webbapp** när Application Insights har installerats.</span><span class="sxs-lookup"><span data-stu-id="8c9a5-123">**Instrument your web app** after Application Insights has been installed.</span></span> 
   
    ![Instrumentera din webbapp](./media/app-insights-azure-web-apps/restart-web-app-for-insights.png)

   <span data-ttu-id="8c9a5-125">**Aktivera övervakning på klientsidan** för sidvy och användartelemetri.</span><span class="sxs-lookup"><span data-stu-id="8c9a5-125">**Enable client side monitoring** for page view and user telemetry.</span></span>

   * <span data-ttu-id="8c9a5-126">Välj Inställningar > Programinställningar</span><span class="sxs-lookup"><span data-stu-id="8c9a5-126">Select Settings > Application Settings</span></span>
   * <span data-ttu-id="8c9a5-127">Under Appinställningar lägger du till ett nytt nyckel/värde-par:</span><span class="sxs-lookup"><span data-stu-id="8c9a5-127">Under App Settings, add a new key value pair:</span></span> 
   
    <span data-ttu-id="8c9a5-128">Nyckel: `APPINSIGHTS_JAVASCRIPT_ENABLED`</span><span class="sxs-lookup"><span data-stu-id="8c9a5-128">Key: `APPINSIGHTS_JAVASCRIPT_ENABLED`</span></span> 
    
    <span data-ttu-id="8c9a5-129">Värde:`true`
</span><span class="sxs-lookup"><span data-stu-id="8c9a5-129">Value: `true`</span></span>
   * <span data-ttu-id="8c9a5-130">**Spara** inställningarna och **starta om** din app.</span><span class="sxs-lookup"><span data-stu-id="8c9a5-130">**Save** the settings and **Restart** your app.</span></span>
3. <span data-ttu-id="8c9a5-131">**Övervaka din app**.</span><span class="sxs-lookup"><span data-stu-id="8c9a5-131">**Monitor your app**.</span></span>  <span data-ttu-id="8c9a5-132">[Utforska data](#explore-the-data).</span><span class="sxs-lookup"><span data-stu-id="8c9a5-132">[Expore the data](#explore-the-data).</span></span>

<span data-ttu-id="8c9a5-133">Senare kan du skapa appen med Application Insights om du vill.</span><span class="sxs-lookup"><span data-stu-id="8c9a5-133">Later, you can build the app with Application Insights if you want.</span></span>

<span data-ttu-id="8c9a5-134">*Hur tar jag bort Application Insights eller växlar till att skicka telemetri till en annan resurs?*</span><span class="sxs-lookup"><span data-stu-id="8c9a5-134">*How do I remove Application Insights, or switch to sending to another resource?*</span></span>

* <span data-ttu-id="8c9a5-135">Öppna kontrollbladet för webbappen i Azure och öppna **Tillägg** under Utvecklingsverktyg.</span><span class="sxs-lookup"><span data-stu-id="8c9a5-135">In Azure, open the web app control blade, and under Development Tools, open **Extensions**.</span></span> <span data-ttu-id="8c9a5-136">Ta bort Application Insights-tillägget.</span><span class="sxs-lookup"><span data-stu-id="8c9a5-136">Delete the Application Insights extension.</span></span> <span data-ttu-id="8c9a5-137">Välj Application Insights och skapa eller välj önskad resurs under Övervakning.</span><span class="sxs-lookup"><span data-stu-id="8c9a5-137">Then under Monitoring, choose Application Insights and create or select the resource you want.</span></span>

## <a name="build-the-app-with-application-insights"></a><span data-ttu-id="8c9a5-138">Skapa appen med Application Insights</span><span class="sxs-lookup"><span data-stu-id="8c9a5-138">Build the app with Application Insights</span></span>
<span data-ttu-id="8c9a5-139">Application Insights kan tillhandahålla mer detaljerad telemetri genom installationen av ett SDK i din app.</span><span class="sxs-lookup"><span data-stu-id="8c9a5-139">Application Insights can provide more detailed telemetry by installing an SDK into your app.</span></span> <span data-ttu-id="8c9a5-140">Mer specifikt kan du samla in spårningsloggar, [skriva anpassad telemetri](app-insights-api-custom-events-metrics.md) och få mer detaljerade undantagsrapporter.</span><span class="sxs-lookup"><span data-stu-id="8c9a5-140">In particular, you can collect trace logs, [write custom telemetry](app-insights-api-custom-events-metrics.md), and get more detailed exception reports.</span></span>

1. <span data-ttu-id="8c9a5-141">**I Visual Studio** (2013 uppdatering 2 eller senare) konfigurerar du Application Insights för ditt projekt.</span><span class="sxs-lookup"><span data-stu-id="8c9a5-141">**In Visual Studio** (2013 update 2 or later), configure Application Insights for your project.</span></span>

    <span data-ttu-id="8c9a5-142">Högerklicka på webbprojektet och välj **Lägg till > Application Insights** eller **Konfigurera Application Insights**.</span><span class="sxs-lookup"><span data-stu-id="8c9a5-142">Right-click the web project, and select **Add > Application Insights** or **Configure Application Insights**.</span></span>
   
    ![Högerklicka på webbprojektet och välj Lägg till Application Insights eller Konfigurera Application Insights](./media/app-insights-azure-web-apps/03-add.png)
   
    <span data-ttu-id="8c9a5-144">Om du uppmanas att logga in använder du autentiseringsuppgifterna för ditt Azure-konto.</span><span class="sxs-lookup"><span data-stu-id="8c9a5-144">If you're asked to sign in, use the credentials for your Azure account.</span></span>
   
    <span data-ttu-id="8c9a5-145">Åtgärden har två effekter:</span><span class="sxs-lookup"><span data-stu-id="8c9a5-145">The operation has two effects:</span></span>
   
   1. <span data-ttu-id="8c9a5-146">Skapar en Application Insights-resurs i Azure, där telemetri lagras, analyseras och visas.</span><span class="sxs-lookup"><span data-stu-id="8c9a5-146">Creates an Application Insights resource in Azure, where telemetry is stored, analyzed and displayed.</span></span>
   2. <span data-ttu-id="8c9a5-147">Lägger till NuGet-paketet för Application Insights i din kod (om det inte redan finns där) och konfigurerar det att skicka telemetri till Azure-resursen.</span><span class="sxs-lookup"><span data-stu-id="8c9a5-147">Adds the Application Insights NuGet package to your code (if it isn't there already), and configures it to send telemetry to the Azure resource.</span></span>
2. <span data-ttu-id="8c9a5-148">**Testa telemetrin** genom att köra appen på utvecklingsdatorn (F5).</span><span class="sxs-lookup"><span data-stu-id="8c9a5-148">**Test the telemetry** by running the app in your development machine (F5).</span></span>
3. <span data-ttu-id="8c9a5-149">**Publicera appen** till Azure som vanligt.</span><span class="sxs-lookup"><span data-stu-id="8c9a5-149">**Publish the app** to Azure in the usual way.</span></span> 

<span data-ttu-id="8c9a5-150">*Hur växlar jag till att skicka telemetri till en annan Application Insights-resurs?*</span><span class="sxs-lookup"><span data-stu-id="8c9a5-150">*How do I switch to sending to a different Application Insights resource?*</span></span>

* <span data-ttu-id="8c9a5-151">I Visual Studio högerklickar du på projektet, väljer **Konfigurera Application Insights** och väljer önskad resurs.</span><span class="sxs-lookup"><span data-stu-id="8c9a5-151">In Visual Studio, right-click the project, choose **Configure Application Insights** and choose the resource you want.</span></span> <span data-ttu-id="8c9a5-152">Du får möjlighet att skapa en ny resurs.</span><span class="sxs-lookup"><span data-stu-id="8c9a5-152">You get the option to create a new resource.</span></span> <span data-ttu-id="8c9a5-153">Återskapa och distribuera igen.</span><span class="sxs-lookup"><span data-stu-id="8c9a5-153">Rebuild and redeploy.</span></span>

## <a name="explore-the-data"></a><span data-ttu-id="8c9a5-154">Utforska data</span><span class="sxs-lookup"><span data-stu-id="8c9a5-154">Explore the data</span></span>
1. <span data-ttu-id="8c9a5-155">På bladet Application Insights på kontrollpanelen för webbappen ser du Live Metrics, som visar begäranden och fel inom en sekund eller två efter att de inträffat.</span><span class="sxs-lookup"><span data-stu-id="8c9a5-155">On the Application Insights blade of your web app control panel, you see Live Metrics, which shows requests and failures within a second or two of them occurring.</span></span> <span data-ttu-id="8c9a5-156">Detta är praktiskt när du publicera om din app, så att du genast ser eventuella problem.</span><span class="sxs-lookup"><span data-stu-id="8c9a5-156">It's very useful display when you're republishing your app - you can see any problems immediately.</span></span>
2. <span data-ttu-id="8c9a5-157">Klicka dig framåt till den fullständiga Application Insights-resursen.</span><span class="sxs-lookup"><span data-stu-id="8c9a5-157">Click through to the full Application Insights resource.</span></span>

    ![Klicka dig framåt](./media/app-insights-azure-web-apps/view-in-application-insights.png)

    <span data-ttu-id="8c9a5-159">Du kan också gå dit direkt från Azure-resursnavigeringen.</span><span class="sxs-lookup"><span data-stu-id="8c9a5-159">You can also go there either directly from Azure resource navigation.</span></span>

1. <span data-ttu-id="8c9a5-160">Klicka dig igenom valfritt diagram om du vill visa mer information:</span><span class="sxs-lookup"><span data-stu-id="8c9a5-160">Click through any chart to get more detail:</span></span>
   
    ![Klicka på ett diagram på översiktsbladet för Application Insights](./media/app-insights-azure-web-apps/07-dependency.png)
   
    <span data-ttu-id="8c9a5-162">Du kan [anpassa bladen med mätvärden](app-insights-metrics-explorer.md).</span><span class="sxs-lookup"><span data-stu-id="8c9a5-162">You can [customize metrics blades](app-insights-metrics-explorer.md).</span></span>
2. <span data-ttu-id="8c9a5-163">Klicka dig vidare om du vill visa enskilda händelser och deras egenskaper:</span><span class="sxs-lookup"><span data-stu-id="8c9a5-163">Click through further to see individual events and their properties:</span></span>
   
    ![Klicka på en händelsetyp för att öppna en sökning som filtrerats baserat på den typen](./media/app-insights-azure-web-apps/08-requests.png)
   
    <span data-ttu-id="8c9a5-165">Observera länken ”...” för att öppna alla egenskaper.</span><span class="sxs-lookup"><span data-stu-id="8c9a5-165">Notice the "..." link to open all properties.</span></span>
   
    <span data-ttu-id="8c9a5-166">Du kan [anpassa sökningar](app-insights-diagnostic-search.md).</span><span class="sxs-lookup"><span data-stu-id="8c9a5-166">You can [customize searches](app-insights-diagnostic-search.md).</span></span>

<span data-ttu-id="8c9a5-167">För mer kraftfulla sökningar över din telemetri kan du använda [Log Analytics-frågespråket](app-insights-analytics-tour.md).</span><span class="sxs-lookup"><span data-stu-id="8c9a5-167">For more powerful searches over your telemetry, use the [Log Analytics query language](app-insights-analytics-tour.md).</span></span>

## <a name="more-telemetry"></a><span data-ttu-id="8c9a5-168">Mer telemetri</span><span class="sxs-lookup"><span data-stu-id="8c9a5-168">More telemetry</span></span>

* [<span data-ttu-id="8c9a5-169">Data om webbsidesinläsning</span><span class="sxs-lookup"><span data-stu-id="8c9a5-169">Web page load data</span></span>](app-insights-javascript.md)
* [<span data-ttu-id="8c9a5-170">Anpassad telemetri</span><span class="sxs-lookup"><span data-stu-id="8c9a5-170">Custom telemetry</span></span>](app-insights-api-custom-events-metrics.md)

## <a name="video"></a><span data-ttu-id="8c9a5-171">Video</span><span class="sxs-lookup"><span data-stu-id="8c9a5-171">Video</span></span>

> [!VIDEO https://channel9.msdn.com/events/Connect/2016/100/player]

## <a name="next-steps"></a><span data-ttu-id="8c9a5-172">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="8c9a5-172">Next steps</span></span>
* <span data-ttu-id="8c9a5-173">[Kör profileraren för din live-app](app-insights-profiler.md).</span><span class="sxs-lookup"><span data-stu-id="8c9a5-173">[Run the profiler on your live app](app-insights-profiler.md).</span></span>
* <span data-ttu-id="8c9a5-174">[Azure Functions](https://github.com/christopheranderson/azure-functions-app-insights-sample) – övervaka Azure Functions med Application Insights</span><span class="sxs-lookup"><span data-stu-id="8c9a5-174">[Azure Functions](https://github.com/christopheranderson/azure-functions-app-insights-sample) - monitor Azure Functions with Application Insights</span></span>
* <span data-ttu-id="8c9a5-175">[Aktivera Azure-diagnostik](app-insights-azure-diagnostics.md) så att den skickas till Application Insights.</span><span class="sxs-lookup"><span data-stu-id="8c9a5-175">[Enable Azure diagnostics](app-insights-azure-diagnostics.md) to be sent to Application Insights.</span></span>
* <span data-ttu-id="8c9a5-176">[Övervaka mätvärden för tjänstens hälsotillstånd](../monitoring-and-diagnostics/insights-how-to-customize-monitoring.md) för att se till att tjänsten är tillgänglig och svarar.</span><span class="sxs-lookup"><span data-stu-id="8c9a5-176">[Monitor service health metrics](../monitoring-and-diagnostics/insights-how-to-customize-monitoring.md) to make sure your service is available and responsive.</span></span>
* <span data-ttu-id="8c9a5-177">[Få aviseringar](../monitoring-and-diagnostics/insights-receive-alert-notifications.md) när drifthändelser inträffar eller när mätvärden överskrider ett tröskelvärde.</span><span class="sxs-lookup"><span data-stu-id="8c9a5-177">[Receive alert notifications](../monitoring-and-diagnostics/insights-receive-alert-notifications.md) whenever operational events happen or metrics cross a threshold.</span></span>
* <span data-ttu-id="8c9a5-178">Använd [Application Insights för JavaScript-appar och webbsidor](app-insights-javascript.md) för att hämta klienttelemetri från webbläsare som besöker en webbsida.</span><span class="sxs-lookup"><span data-stu-id="8c9a5-178">Use [Application Insights for JavaScript apps and web pages](app-insights-javascript.md) to get client telemetry from the browsers that visit a web page.</span></span>
* <span data-ttu-id="8c9a5-179">[Konfigurera tillgänglighetswebbtester](app-insights-monitor-web-app-availability.md) så att du aviseras om webbplatsen inte fungerar.</span><span class="sxs-lookup"><span data-stu-id="8c9a5-179">[Set up Availability web tests](app-insights-monitor-web-app-availability.md) to be alerted if your site is down.</span></span>

