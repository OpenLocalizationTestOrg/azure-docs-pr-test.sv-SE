---
title: "Azure Application Insights för JavaScript-webbappar | Microsoft Docs"
description: "Hämta sidvisnings- och sessionsantal, webbklientdata och spåra användningsmönster. Identifiera undantag och prestandaproblem på JavaScript-baserade webbsidor."
services: application-insights
documentationcenter: 
author: CFreemanwa
manager: carmonm
ms.assetid: 3b710d09-6ab4-4004-b26a-4fa840039500
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: get-started-article
ms.date: 03/14/2017
ms.author: bwren
ms.openlocfilehash: 4e8a77e3644bb726d1b8e2050dab61893ccfa3c9
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/18/2017
---
# <a name="application-insights-for-web-pages"></a><span data-ttu-id="ecb3c-104">Application Insights för webbsidor</span><span class="sxs-lookup"><span data-stu-id="ecb3c-104">Application Insights for web pages</span></span>
<span data-ttu-id="ecb3c-105">Visa prestanda och användning för webbsidor eller appar.</span><span class="sxs-lookup"><span data-stu-id="ecb3c-105">Find out about the performance and usage of your web page or app.</span></span> <span data-ttu-id="ecb3c-106">Om du lägger till [Application Insights](app-insights-overview.md) i webbsidans skript så visas information om tider för sidinläsningar och AJAX-anrop, information om och antalet webbläsarundantag och AJAX-fel, samt information om antalet användare och sessioner.</span><span class="sxs-lookup"><span data-stu-id="ecb3c-106">If you add [Application Insights](app-insights-overview.md) to your page script, you get timings of page loads and AJAX calls, counts and details of browser exceptions and AJAX failures, as well as users and session counts.</span></span> <span data-ttu-id="ecb3c-107">Allt detta kan visas efter sida, klientoperativsystem- och webbläsarversion, geografisk plats och andra dimensioner.</span><span class="sxs-lookup"><span data-stu-id="ecb3c-107">All these can be segmented by page, client OS and browser version, geo location, and other dimensions.</span></span> <span data-ttu-id="ecb3c-108">Du kan ställa in varningar för antal fel eller långsam sidinläsning.</span><span class="sxs-lookup"><span data-stu-id="ecb3c-108">You can set alerts on failure counts or slow page loading.</span></span> <span data-ttu-id="ecb3c-109">Och genom att infoga spårning av anrop i JavaScript-kod kan du spåra hur olika funktioner i ditt webbsideprogram används.</span><span class="sxs-lookup"><span data-stu-id="ecb3c-109">And by inserting trace calls in your JavaScript code, you can track how the different features of your web page application are used.</span></span>

<span data-ttu-id="ecb3c-110">Application Insights kan användas med alla webbsidor – du lägger bara till ett stycke JavaScript-kod.</span><span class="sxs-lookup"><span data-stu-id="ecb3c-110">Application Insights can be used with any web pages - you just add a short piece of JavaScript.</span></span> <span data-ttu-id="ecb3c-111">Om webbtjänsten är [Java](app-insights-java-get-started.md) eller [ASP.NET](app-insights-asp-net.md) kan du integrera telemetri från dina servrar och klienter.</span><span class="sxs-lookup"><span data-stu-id="ecb3c-111">If your web service is [Java](app-insights-java-get-started.md) or [ASP.NET](app-insights-asp-net.md), you can integrate telemetry from your server and clients.</span></span>

![Öppna appens resurs på portal.azure.com och klicka på Webbläsare](./media/app-insights-javascript/03.png)

<span data-ttu-id="ecb3c-113">Du behöver en prenumeration på [Microsoft Azure](https://azure.com).</span><span class="sxs-lookup"><span data-stu-id="ecb3c-113">You need a subscription to [Microsoft Azure](https://azure.com).</span></span> <span data-ttu-id="ecb3c-114">Om ditt team har en organisationsprenumeration ber du ägaren att lägga till ditt Microsoft-Account till den.</span><span class="sxs-lookup"><span data-stu-id="ecb3c-114">If your team has an organizational subscription, ask the owner to add your Microsoft Account to it.</span></span> <span data-ttu-id="ecb3c-115">Utveckling och småskalig användning kostar inget.</span><span class="sxs-lookup"><span data-stu-id="ecb3c-115">Development and small-scale use won't cost anything.</span></span>

## <a name="set-up-application-insights-for-your-web-page"></a><span data-ttu-id="ecb3c-116">Konfigurera Application Insights för din webbsida</span><span class="sxs-lookup"><span data-stu-id="ecb3c-116">Set up Application Insights for your web page</span></span>
<span data-ttu-id="ecb3c-117">Lägg till inläsningen av kodfragmentet i dina webbsidor enligt följande.</span><span class="sxs-lookup"><span data-stu-id="ecb3c-117">Add the loader code snippet to your web pages, as follows.</span></span>

### <a name="open-or-create-application-insights-resource"></a><span data-ttu-id="ecb3c-118">Öppna eller skapa en Application Insights-resurs</span><span class="sxs-lookup"><span data-stu-id="ecb3c-118">Open or create Application Insights resource</span></span>
<span data-ttu-id="ecb3c-119">Application Insights-resursen är den plats där data om sidans prestanda och användning visas.</span><span class="sxs-lookup"><span data-stu-id="ecb3c-119">The Application Insights resource is where data about your page's performance and usage is displayed.</span></span> 

<span data-ttu-id="ecb3c-120">Logga in på [Azure-portalen](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="ecb3c-120">Sign into [Azure portal](https://portal.azure.com).</span></span>

<span data-ttu-id="ecb3c-121">Om du redan har konfigurerat övervakning för serversidan i din app så har du redan en resurs:</span><span class="sxs-lookup"><span data-stu-id="ecb3c-121">If you already set up monitoring for the server side of your app, you already have a resource:</span></span>

![Välj Bläddra, Utvecklartjänster, Application Insights.](./media/app-insights-javascript/01-find.png)

<span data-ttu-id="ecb3c-123">Om du inte redan har en resurs skapar du en:</span><span class="sxs-lookup"><span data-stu-id="ecb3c-123">If you don't have one, create it:</span></span>

![Välj Nytt, Utvecklartjänster, Application Insights.](./media/app-insights-javascript/01-create.png)

<span data-ttu-id="ecb3c-125">*Har du redan frågor?*</span><span class="sxs-lookup"><span data-stu-id="ecb3c-125">*Questions already?*</span></span> <span data-ttu-id="ecb3c-126">[Mer information om hur du skapar en resurs](app-insights-create-new-resource.md).</span><span class="sxs-lookup"><span data-stu-id="ecb3c-126">[More about creating a resource](app-insights-create-new-resource.md).</span></span>

### <a name="add-the-sdk-script-to-your-app-or-web-pages"></a><span data-ttu-id="ecb3c-127">Lägga till SDK-skriptet till appen eller webbsidorna</span><span class="sxs-lookup"><span data-stu-id="ecb3c-127">Add the SDK script to your app or web pages</span></span>
<span data-ttu-id="ecb3c-128">Hämta skriptet för webbsidor i Snabbstart:</span><span class="sxs-lookup"><span data-stu-id="ecb3c-128">In Quick Start, get the script for web pages:</span></span>

![På översiktsbladet för appen väljer du Snabbstart, Hämta kod för att övervaka webbplatser.](./media/app-insights-javascript/02-monitor-web-page.png)

<span data-ttu-id="ecb3c-131">Infoga skriptet precis före `</head>`-taggen för alla sidor som du vill spåra. Om din webbplats har en huvudsida kan du placera skriptet där.</span><span class="sxs-lookup"><span data-stu-id="ecb3c-131">Insert the script just before the `</head>` tag of every page you want to track. If your website has a master page, you can put the script there.</span></span> <span data-ttu-id="ecb3c-132">Exempel:</span><span class="sxs-lookup"><span data-stu-id="ecb3c-132">For example:</span></span>

* <span data-ttu-id="ecb3c-133">I ett ASP.NET MVC-projekt lägger du till det i `View\Shared\_Layout.cshtml`</span><span class="sxs-lookup"><span data-stu-id="ecb3c-133">In an ASP.NET MVC project, you'd put it in `View\Shared\_Layout.cshtml`</span></span>
* <span data-ttu-id="ecb3c-134">På en SharePoint-plats öppnar du [Webbplatsinställningar/Huvudsida](app-insights-sharepoint.md) på kontrollpanelen.</span><span class="sxs-lookup"><span data-stu-id="ecb3c-134">In a SharePoint site, on the control panel, open [Site Settings / Master Page](app-insights-sharepoint.md).</span></span>

<span data-ttu-id="ecb3c-135">Skriptet innehåller instrumenteringsnyckeln som dirigerar data till din Application Insights-resurs.</span><span class="sxs-lookup"><span data-stu-id="ecb3c-135">The script contains the instrumentation key that directs the data to your Application Insights resource.</span></span> 

<span data-ttu-id="ecb3c-136">([Mer detaljerad förklaring av skriptet.](http://apmtips.com/blog/2015/03/18/javascript-snippet-explained/))</span><span class="sxs-lookup"><span data-stu-id="ecb3c-136">([Deeper explanation of the script.](http://apmtips.com/blog/2015/03/18/javascript-snippet-explained/))</span></span>

<span data-ttu-id="ecb3c-137">*(Om du använder ett välkänt ramverk för webbsidor letar du efter adaptrar till Application Insights. Det finns till exempel [en AngularJS-modul](http://ngmodules.org/modules/angular-appinsights).)*</span><span class="sxs-lookup"><span data-stu-id="ecb3c-137">*(If you're using a well-known web page framework, look around for Application Insights adaptors. For example, there's [an AngularJS module](http://ngmodules.org/modules/angular-appinsights).)*</span></span>

## <a name="detailed-configuration"></a><span data-ttu-id="ecb3c-138">Detaljerad konfiguration</span><span class="sxs-lookup"><span data-stu-id="ecb3c-138">Detailed configuration</span></span>
<span data-ttu-id="ecb3c-139">Det finns flera [Parametrar](https://github.com/Microsoft/ApplicationInsights-JS/blob/master/API-reference.md#config) som du kan ange, men i de flesta fall behöver du inte göra det.</span><span class="sxs-lookup"><span data-stu-id="ecb3c-139">There are several [parameters](https://github.com/Microsoft/ApplicationInsights-JS/blob/master/API-reference.md#config) you can set, though in most cases, you shouldn't need to.</span></span> <span data-ttu-id="ecb3c-140">Du kan t.ex. inaktivera eller begränsa antalet Ajax-anrop som rapporteras per sidvy (om du vill minska trafiken).</span><span class="sxs-lookup"><span data-stu-id="ecb3c-140">For example, you can disable or limit the number of Ajax calls reported per page view (to reduce traffic).</span></span> <span data-ttu-id="ecb3c-141">Eller så kan du ställa in felsökningsläge så att telemetrin rör sig snabbt genom pipelinen utan att batchhanteras.</span><span class="sxs-lookup"><span data-stu-id="ecb3c-141">Or you can set debug mode to have telemetry move rapidly through the pipeline without being batched.</span></span>

<span data-ttu-id="ecb3c-142">Du anger dessa parametrar genom att leta upp den här raden i kodfragmentet och lägga till fler kommaavgränsade poster efter den:</span><span class="sxs-lookup"><span data-stu-id="ecb3c-142">To set these parameters, look for this line in the code snippet, and add more comma-separated items after it:</span></span>

    })({
      instrumentationKey: "..."
      // Insert here
    });

<span data-ttu-id="ecb3c-143">Exempel på [tillgängliga parametrar](https://github.com/Microsoft/ApplicationInsights-JS/blob/master/API-reference.md#config) är:</span><span class="sxs-lookup"><span data-stu-id="ecb3c-143">The [available parameters](https://github.com/Microsoft/ApplicationInsights-JS/blob/master/API-reference.md#config) include:</span></span>

    // Send telemetry immediately without batching.
    // Remember to remove this when no longer required, as it
    // can affect browser performance.
    enableDebug: boolean,

    // Don't log browser exceptions.
    disableExceptionTracking: boolean,

    // Don't log ajax calls.
    disableAjaxTracking: boolean,

    // Limit number of Ajax calls logged, to reduce traffic.
    maxAjaxCallsPerView: 10, // default is 500

    // Time page load up to execution of first trackPageView().
    overridePageViewDuration: boolean,

    // Set these dynamically for an authenticated user.
    appUserId: string,
    accountId: string,



## <span data-ttu-id="ecb3c-144"><a name="run"></a>Kör din app</span><span class="sxs-lookup"><span data-stu-id="ecb3c-144"><a name="run"></a>Run your app</span></span>
<span data-ttu-id="ecb3c-145">Kör webbappen, använd den en stund för att generera telemetri och vänta några sekunder.</span><span class="sxs-lookup"><span data-stu-id="ecb3c-145">Run your web app, use it a while to generate telemetry, and wait a few seconds.</span></span> <span data-ttu-id="ecb3c-146">Du kan antingen köra den genom att trycka på **F5** på utvecklingsdatorn, eller publicera den och låta användarna använda den.</span><span class="sxs-lookup"><span data-stu-id="ecb3c-146">You can either run it using the **F5** key on your development machine, or publish it and let users play with it.</span></span>

<span data-ttu-id="ecb3c-147">Om du vill kontrollera telemetrin som en webbapp skickar till Application Insights använder du webbläsarens felsökningsverktyg (**F12** i många webbläsare).</span><span class="sxs-lookup"><span data-stu-id="ecb3c-147">If you want to check the telemetry that a web app is sending to Application Insights, use your browser's debugging tools (**F12** on many browsers).</span></span> <span data-ttu-id="ecb3c-148">Informationen skickas till dc.services.visualstudio.com.</span><span class="sxs-lookup"><span data-stu-id="ecb3c-148">Data is sent to dc.services.visualstudio.com.</span></span>

## <a name="explore-your-browser-performance-data"></a><span data-ttu-id="ecb3c-149">Granska informationen om webbläsarens prestanda</span><span class="sxs-lookup"><span data-stu-id="ecb3c-149">Explore your browser performance data</span></span>
<span data-ttu-id="ecb3c-150">Öppna bladet Webbläsare om du vill visa aggregerade data från användarnas webbläsare.</span><span class="sxs-lookup"><span data-stu-id="ecb3c-150">Open the Browser blade to show aggregated performance data from your users' browsers.</span></span>

![Öppna appens resurs på portal.azure.com och klicka på Inställningar, Webbläsare.](./media/app-insights-javascript/03.png)

<span data-ttu-id="ecb3c-152">*Ser du inga data än? Klicka på **Uppdatera** längst upp på sidan. Ser du fortfarande ingenting? Mer information finns i [Felsökning](app-insights-troubleshoot-faq.md).*</span><span class="sxs-lookup"><span data-stu-id="ecb3c-152">*No data yet? Click **Refresh** at the top of the page. Still nothing? See [Troubleshooting](app-insights-troubleshoot-faq.md).*</span></span>

<span data-ttu-id="ecb3c-153">Bladet Webbläsare är ett [Metrics Explorer-blad](app-insights-metrics-explorer.md) med förinställda filter och diagraminställningar.</span><span class="sxs-lookup"><span data-stu-id="ecb3c-153">The Browser blade is a [Metrics Explorer blade](app-insights-metrics-explorer.md) with preset filters and chart selections.</span></span> <span data-ttu-id="ecb3c-154">Du kan redigera tidsintervallet, filtren och diagramkonfigurationen om du vill och spara resultatet som en favorit.</span><span class="sxs-lookup"><span data-stu-id="ecb3c-154">You can edit the time range, filters, and chart configuration if you want, and save the result as a favorite.</span></span> <span data-ttu-id="ecb3c-155">Klicka på **Återställ standardvärden** för att återgå till det ursprungliga konfigurationsbladet.</span><span class="sxs-lookup"><span data-stu-id="ecb3c-155">Click **Restore defaults** to get back to the original blade configuration.</span></span>

## <a name="page-load-performance"></a><span data-ttu-id="ecb3c-156">Sidinläsningsprestanda</span><span class="sxs-lookup"><span data-stu-id="ecb3c-156">Page load performance</span></span>
<span data-ttu-id="ecb3c-157">Längst upp på sidan finns ett segmenterat diagram över sidinläsningstider.</span><span class="sxs-lookup"><span data-stu-id="ecb3c-157">At the top is a segmented chart of page load times.</span></span> <span data-ttu-id="ecb3c-158">Diagrammets totala höjd representerar den genomsnittliga tid det tar att läsa in och visa sidor från appen i användarnas webbläsare.</span><span class="sxs-lookup"><span data-stu-id="ecb3c-158">The total height of the chart represents the average time to load and display pages from your app in your users' browsers.</span></span> <span data-ttu-id="ecb3c-159">Tiden mäts från tidpunkten då webbläsaren skickar den första HTTP-begäran tills alla synkrona belastningshändelser har bearbetats, inklusive layout och skriptkörning.</span><span class="sxs-lookup"><span data-stu-id="ecb3c-159">The time is measured from when the browser sends the initial HTTP request until all synchronous load events have been processed, including layout and running scripts.</span></span> <span data-ttu-id="ecb3c-160">De omfattar inte asynkrona åtgärder, till exempel inläsning av webbdelar från AJAX-anrop.</span><span class="sxs-lookup"><span data-stu-id="ecb3c-160">It doesn't include asynchronous tasks such as loading web parts from AJAX calls.</span></span>

<span data-ttu-id="ecb3c-161">I diagrammet delas den totala sidinläsningstiden upp baserat på [standardtiderna som definierats av W3C](http://www.w3.org/TR/navigation-timing/#processing-model).</span><span class="sxs-lookup"><span data-stu-id="ecb3c-161">The chart segments the total page load time into the [standard timings defined by W3C](http://www.w3.org/TR/navigation-timing/#processing-model).</span></span> 

![](./media/app-insights-javascript/08-client-split.png)

<span data-ttu-id="ecb3c-162">Observera att *nätverksanslutningstiden* ofta är lägre än förväntat eftersom det är ett medelvärde över alla förfrågningar från webbläsaren till servern.</span><span class="sxs-lookup"><span data-stu-id="ecb3c-162">Note that the *network connect* time is often lower than you might expect, because it's an average over all requests from the browser to the server.</span></span> <span data-ttu-id="ecb3c-163">Många enskilda förfrågningar har anslutningstiden 0 eftersom det redan finns en aktiv anslutning till servern.</span><span class="sxs-lookup"><span data-stu-id="ecb3c-163">Many individual requests have a connect time of 0 because there is already an active connection to the server.</span></span>

### <a name="slow-loading"></a><span data-ttu-id="ecb3c-164">Är inläsningen långsam?</span><span class="sxs-lookup"><span data-stu-id="ecb3c-164">Slow loading?</span></span>
<span data-ttu-id="ecb3c-165">Långsamma sidinläsningar är ytterst frustrerande för användarna.</span><span class="sxs-lookup"><span data-stu-id="ecb3c-165">Slow page loads are a major source of dissatisfaction for your users.</span></span> <span data-ttu-id="ecb3c-166">Om diagrammet visar på långsamma sidinläsningar är det enkelt att undersöka varför.</span><span class="sxs-lookup"><span data-stu-id="ecb3c-166">If the chart indicates slow page loads, it's easy to do some diagnostic research.</span></span>

<span data-ttu-id="ecb3c-167">Diagrammet visar medelvärdet av alla sidinläsningar i appen.</span><span class="sxs-lookup"><span data-stu-id="ecb3c-167">The chart shows the average of all page loads in your app.</span></span> <span data-ttu-id="ecb3c-168">Om du vill se om problemet är begränsat till specifika sidor går du längre ned i bladet, där du ser ett rutnät uppdelat efter sid-URL:</span><span class="sxs-lookup"><span data-stu-id="ecb3c-168">To see if the problem is confined to particular pages, look further down the blade, where there's a grid segmented by page URL:</span></span>

![](./media/app-insights-javascript/09-page-perf.png)

<span data-ttu-id="ecb3c-169">Notera antalet sidvisningar och standardavvikelsen.</span><span class="sxs-lookup"><span data-stu-id="ecb3c-169">Notice the page view count and standard deviation.</span></span> <span data-ttu-id="ecb3c-170">Om sidantalet är mycket lågt så påverkas inte användarna särskilt mycket av problemet.</span><span class="sxs-lookup"><span data-stu-id="ecb3c-170">If the page count is very low, then the issue isn't affecting users much.</span></span> <span data-ttu-id="ecb3c-171">En hög standardavvikelse (jämförbar med genomsnittet) anger en stor avvikelse mellan enskilda mått.</span><span class="sxs-lookup"><span data-stu-id="ecb3c-171">A high standard deviation (comparable to the average itself) indicates a lot of variation between individual measurements.</span></span>

<span data-ttu-id="ecb3c-172">**Zooma in en URL och en sidvisning.**</span><span class="sxs-lookup"><span data-stu-id="ecb3c-172">**Zoom in on one URL and one page view.**</span></span> <span data-ttu-id="ecb3c-173">Klicka på ett sidnamn för att visa ett blad med webbläsardiagram filtrerade på just den URL:en, och sedan på en instans av en sidvy.</span><span class="sxs-lookup"><span data-stu-id="ecb3c-173">Click any page name to see a blade of browser charts filtered just to that URL; and then on an instance of a page view.</span></span>

![](./media/app-insights-javascript/35.png)

<span data-ttu-id="ecb3c-174">Klicka på `...` om du vill visa en fullständig lista över egenskaper för händelsen eller granska Ajax-anropen och relaterade händelser.</span><span class="sxs-lookup"><span data-stu-id="ecb3c-174">Click `...` for a full list of properties for that event, or inspect the Ajax calls and related events.</span></span> <span data-ttu-id="ecb3c-175">Långsamma Ajax-anrop påverkar den allmänna sidinläsningstiden om de är synkrona.</span><span class="sxs-lookup"><span data-stu-id="ecb3c-175">Slow Ajax calls affect the overall page load time if they are synchronous.</span></span> <span data-ttu-id="ecb3c-176">Exempel på relaterade händelser är serverbegäranden för samma URL (om du har konfigurerat Application Insights på webbservern).</span><span class="sxs-lookup"><span data-stu-id="ecb3c-176">Related events include server requests for the same URL (if you've set up Application Insights on your web server).</span></span>

<span data-ttu-id="ecb3c-177">**Sidprestanda över tid.**</span><span class="sxs-lookup"><span data-stu-id="ecb3c-177">**Page performance over time.**</span></span> <span data-ttu-id="ecb3c-178">Ändra rutnätet Inläsningstid för sidvisning på bladet Webbläsare till ett linjediagram och se om det förekommit toppar vid specifika tidpunkter:</span><span class="sxs-lookup"><span data-stu-id="ecb3c-178">Back at the Browsers blade, change the Page View Load Time grid into a line chart to see if there were peaks at particular times:</span></span>

![Klicka i sidhuvudet i rutnätet och välj en annan diagramtyp.](./media/app-insights-javascript/10-page-perf-area.png)

<span data-ttu-id="ecb3c-180">**Ändra uppdelningen baserat på andra dimensioner.**</span><span class="sxs-lookup"><span data-stu-id="ecb3c-180">**Segment by other dimensions.**</span></span> <span data-ttu-id="ecb3c-181">Kanske tar det längre tid att läsa in sidorna i en viss webbläsare, i ett visst klientoperativsystem eller på en viss användarplats?</span><span class="sxs-lookup"><span data-stu-id="ecb3c-181">Maybe your pages are slower to load on a particular browser, client OS, or user locality?</span></span> <span data-ttu-id="ecb3c-182">Lägg till ett nytt diagram och experimentera med dimensionen **Gruppera efter**.</span><span class="sxs-lookup"><span data-stu-id="ecb3c-182">Add a new chart and experiment with the **Group-by** dimension.</span></span>

![](./media/app-insights-javascript/21.png)

## <a name="ajax-performance"></a><span data-ttu-id="ecb3c-183">AJAX-prestanda</span><span class="sxs-lookup"><span data-stu-id="ecb3c-183">AJAX Performance</span></span>
<span data-ttu-id="ecb3c-184">Kontrollera att alla AJAX-anrop på dina webbsidor fungerar som de ska.</span><span class="sxs-lookup"><span data-stu-id="ecb3c-184">Make sure any AJAX calls in your web pages are performing well.</span></span> <span data-ttu-id="ecb3c-185">De används ofta för att fylla delar av sidan asynkront.</span><span class="sxs-lookup"><span data-stu-id="ecb3c-185">They are often used to fill parts of your page asynchronously.</span></span> <span data-ttu-id="ecb3c-186">Även om den övergripande sidan kan läsas in snabbt kan användarna bli frustrerade om de öppnar nya webbdelar och måste vänta på att data ska visas i dem.</span><span class="sxs-lookup"><span data-stu-id="ecb3c-186">Although the overall page might load promptly, your users could be frustrated by staring at blank web parts, waiting for data to appear in them.</span></span>

<span data-ttu-id="ecb3c-187">AJAX-anrop som görs från din webbsida visas på bladet Webbläsare som beroenden.</span><span class="sxs-lookup"><span data-stu-id="ecb3c-187">AJAX calls made from your web page are shown on the Browsers blade as dependencies.</span></span>

<span data-ttu-id="ecb3c-188">Du hittar sammanfattningsdiagram i den övre delen av bladet:</span><span class="sxs-lookup"><span data-stu-id="ecb3c-188">There are summary charts in the upper part of the blade:</span></span>

![](./media/app-insights-javascript/31.png)

<span data-ttu-id="ecb3c-189">Och detaljerade rutnät längre ned:</span><span class="sxs-lookup"><span data-stu-id="ecb3c-189">and detailed grids lower down:</span></span>

![](./media/app-insights-javascript/33.png)

<span data-ttu-id="ecb3c-190">Klicka på en rad om du vill visa specifik information.</span><span class="sxs-lookup"><span data-stu-id="ecb3c-190">Click any row for specific details.</span></span>

> [!NOTE]
> <span data-ttu-id="ecb3c-191">Om du tar bort filtret Webbläsare på bladet tas både server- och AJAX-beroenden med i dessa diagram.</span><span class="sxs-lookup"><span data-stu-id="ecb3c-191">If you delete the Browsers filter on the blade, both server and AJAX dependencies are included in these charts.</span></span> <span data-ttu-id="ecb3c-192">Klicka på Återställ standardvärden om du vill konfigurera om filtret.</span><span class="sxs-lookup"><span data-stu-id="ecb3c-192">Click Restore Defaults to reconfigure the filter.</span></span>
> 
> 

<span data-ttu-id="ecb3c-193">**Gå till misslyckades Ajax-anrop** genom att bläddra ned till rutnätet över beroendefel och klicka på en rad för att visa specifika instanser.</span><span class="sxs-lookup"><span data-stu-id="ecb3c-193">**To drill into failed Ajax calls** scroll down to the Dependency failures grid, and then click a row to see specific instances.</span></span>

![](./media/app-insights-javascript/37.png)


<span data-ttu-id="ecb3c-194">Klicka på `...` om du vill visa fullständig telemetri om ett Ajax-anrop.</span><span class="sxs-lookup"><span data-stu-id="ecb3c-194">Click `...` for the full telemetry for an Ajax call.</span></span>

### <a name="no-ajax-calls-reported"></a><span data-ttu-id="ecb3c-195">Rapporteras inga Ajax-anrop?</span><span class="sxs-lookup"><span data-stu-id="ecb3c-195">No Ajax calls reported?</span></span>
<span data-ttu-id="ecb3c-196">AJAX-anrop omfattar alla HTTP/HTTPS-anrop som görs från skriptet på din webbsida.</span><span class="sxs-lookup"><span data-stu-id="ecb3c-196">Ajax calls include any HTTP/HTTPS  calls made from the script of your web page.</span></span> <span data-ttu-id="ecb3c-197">Om de inte har rapporterats kontrollerar du att kodfragmentet inte innehåller [parametern](https://github.com/Microsoft/ApplicationInsights-JS/blob/master/API-reference.md#config) `disableAjaxTracking` eller `maxAjaxCallsPerView`.</span><span class="sxs-lookup"><span data-stu-id="ecb3c-197">If you don't see them reported, check that the code snippet doesn't set the `disableAjaxTracking` or `maxAjaxCallsPerView` [parameters](https://github.com/Microsoft/ApplicationInsights-JS/blob/master/API-reference.md#config).</span></span>

## <a name="browser-exceptions"></a><span data-ttu-id="ecb3c-198">Webbläsarundantag</span><span class="sxs-lookup"><span data-stu-id="ecb3c-198">Browser exceptions</span></span>
<span data-ttu-id="ecb3c-199">Bladet Webbläsare innehåller ett sammanfattningsdiagram över undantag och ett rutnät med undantagstyper längre ned.</span><span class="sxs-lookup"><span data-stu-id="ecb3c-199">On the Browsers blade, there's an exceptions summary chart, and a grid of exception types further down the blade.</span></span>

![](./media/app-insights-javascript/39.png)

<span data-ttu-id="ecb3c-200">Om du inte ser webbläsarundantag kontrollerar du att kodfragmentet inte innehåller [parametern](https://github.com/Microsoft/ApplicationInsights-JS/blob/master/API-reference.md#config) `disableExceptionTracking`.</span><span class="sxs-lookup"><span data-stu-id="ecb3c-200">If you don't see browser exceptions reported, check that the code snippet doesn't set the `disableExceptionTracking` [parameter](https://github.com/Microsoft/ApplicationInsights-JS/blob/master/API-reference.md#config).</span></span>

## <a name="inspect-individual-page-view-events"></a><span data-ttu-id="ecb3c-201">Granska enskilda sidvisningshändelser</span><span class="sxs-lookup"><span data-stu-id="ecb3c-201">Inspect individual page view events</span></span>

<span data-ttu-id="ecb3c-202">Vanligtvis analyseras telemetri för sidvisningar av Application Insights och du ser endast kumulativa rapporter, som ett genomsnitt av alla användare.</span><span class="sxs-lookup"><span data-stu-id="ecb3c-202">Usually page view telemetry is analyzed by Application Insights and you see only cumulative reports, averaged over all your users.</span></span> <span data-ttu-id="ecb3c-203">Men för felsökningsändamål kan du även titta på enskilda sidvisningshändelser.</span><span class="sxs-lookup"><span data-stu-id="ecb3c-203">But for debugging purposes, you can also look at individual page view events.</span></span>

<span data-ttu-id="ecb3c-204">Ställ in Filter till Sidvy på bladet Diagnostiksökning.</span><span class="sxs-lookup"><span data-stu-id="ecb3c-204">In the Diagnostic Search blade, set Filters to Page View.</span></span>

![](./media/app-insights-javascript/12-search-pages.png)

<span data-ttu-id="ecb3c-205">Välj en händelse om du vill visa mer information.</span><span class="sxs-lookup"><span data-stu-id="ecb3c-205">Select any event to see more detail.</span></span> <span data-ttu-id="ecb3c-206">Klicka på ”...” på detaljsidan om du vill visa ännu fler detaljer.</span><span class="sxs-lookup"><span data-stu-id="ecb3c-206">In the details page, click "..." to see even more detail.</span></span>

> [!NOTE]
> <span data-ttu-id="ecb3c-207">Om du använder [Sök](app-insights-diagnostic-search.md) är det viktigt att du matchar hela ord: ”Info” och ”formation” matchar inte ”Information”.</span><span class="sxs-lookup"><span data-stu-id="ecb3c-207">If you use [Search](app-insights-diagnostic-search.md), notice that you have to match whole words: "Abou" and "bout" do not match "About".</span></span>
> 
> 

<span data-ttu-id="ecb3c-208">Du kan också använda det kraftfulla [Log Analytics-frågespråket](https://docs.microsoft.com/azure/application-insights/app-insights-analytics-tour#browser-timings-table) när du söker efter sidvyer.</span><span class="sxs-lookup"><span data-stu-id="ecb3c-208">You can also use the powerful [Log Analytics query language](https://docs.microsoft.com/azure/application-insights/app-insights-analytics-tour#browser-timings-table) to search page views.</span></span>

### <a name="page-view-properties"></a><span data-ttu-id="ecb3c-209">Egenskaper för sidvisning</span><span class="sxs-lookup"><span data-stu-id="ecb3c-209">Page view properties</span></span>
* <span data-ttu-id="ecb3c-210">**Sidvisningens varaktighet**</span><span class="sxs-lookup"><span data-stu-id="ecb3c-210">**Page view duration**</span></span> 
  
  * <span data-ttu-id="ecb3c-211">Som standard den tid det tar att läsa in sidan, från klientbegäran till full inläsning (inklusive extra filer men exklusive asynkrona åtgärder som Ajax-anrop).</span><span class="sxs-lookup"><span data-stu-id="ecb3c-211">By default, the time it takes to load the page, from client request to full load (including auxiliary files but excluding asynchronous tasks such as Ajax calls).</span></span> 
  * <span data-ttu-id="ecb3c-212">Intervallet mellan klientbegäran till körningen av den första `trackPageView` om du anger `overridePageViewDuration` i [sidkonfigurationen](#detailed-configuration).</span><span class="sxs-lookup"><span data-stu-id="ecb3c-212">If you set `overridePageViewDuration` in the [page configuration](#detailed-configuration), the interval between client request to execution of the first `trackPageView`.</span></span> <span data-ttu-id="ecb3c-213">Om du har flyttat trackPageView från dess normala position efter initieringen av skriptet visas ett annat värde.</span><span class="sxs-lookup"><span data-stu-id="ecb3c-213">If you moved trackPageView from its usual position after the initialization of the script, it will reflect a different value.</span></span>
  * <span data-ttu-id="ecb3c-214">Om du har angett `overridePageViewDuration` och ett varaktighetsargument anges i `trackPageView()`-anropet, så används argumentvärdet istället.</span><span class="sxs-lookup"><span data-stu-id="ecb3c-214">If `overridePageViewDuration` is set and a duration argument is provided in the `trackPageView()` call, then the argument value is used instead.</span></span> 

## <a name="custom-page-counts"></a><span data-ttu-id="ecb3c-215">Anpassade sidräkningar</span><span class="sxs-lookup"><span data-stu-id="ecb3c-215">Custom page counts</span></span>
<span data-ttu-id="ecb3c-216">Som standard ökar sidräkningen varje gång en ny sida läses in i webbläsaren.</span><span class="sxs-lookup"><span data-stu-id="ecb3c-216">By default, a page count occurs each time a new page loads into the client browser.</span></span>  <span data-ttu-id="ecb3c-217">Men du kanske vill räkna fler slags sidvisningar.</span><span class="sxs-lookup"><span data-stu-id="ecb3c-217">But you might want to count additional page views.</span></span> <span data-ttu-id="ecb3c-218">En sida kan till exempel visa innehåll på flikar, och du kanske vill att en sida ska räknas när användaren byter flik.</span><span class="sxs-lookup"><span data-stu-id="ecb3c-218">For example, a page might display its content in tabs and you want to count a page when the user switches tabs.</span></span> <span data-ttu-id="ecb3c-219">Eller så kanske JavaScript-kod på sidan läser in nytt innehåll utan att webbläsarens URL ändras.</span><span class="sxs-lookup"><span data-stu-id="ecb3c-219">Or JavaScript code in the page might load new content without changing the browser's URL.</span></span>

<span data-ttu-id="ecb3c-220">Infoga ett JavaScript-anrop som det här på lämpligt ställe i klientkoden:</span><span class="sxs-lookup"><span data-stu-id="ecb3c-220">Insert a JavaScript call like this at the appropriate point in your client code:</span></span>

    appInsights.trackPageView(myPageName);

<span data-ttu-id="ecb3c-221">Sidans namn kan innehålla samma tecken som en URL, men allt efter ”#” eller ”?” ignoreras.</span><span class="sxs-lookup"><span data-stu-id="ecb3c-221">The page name can contain the same characters as a URL, but anything after "#" or "?" is ignored.</span></span>

## <a name="usage-tracking"></a><span data-ttu-id="ecb3c-222">Användningsspårning</span><span class="sxs-lookup"><span data-stu-id="ecb3c-222">Usage tracking</span></span>
<span data-ttu-id="ecb3c-223">Vill du veta vad användarna gör med din app?</span><span class="sxs-lookup"><span data-stu-id="ecb3c-223">Want to find out what your users do with your app?</span></span>

* [<span data-ttu-id="ecb3c-224">Lär dig mer om användningsspårning</span><span class="sxs-lookup"><span data-stu-id="ecb3c-224">Learn about usage tracking</span></span>](app-insights-web-track-usage.md)
* <span data-ttu-id="ecb3c-225">[Lär dig mer om API:er för mätvärden och anpassade händelser](app-insights-api-custom-events-metrics.md).</span><span class="sxs-lookup"><span data-stu-id="ecb3c-225">[Learn about custom events and metrics API](app-insights-api-custom-events-metrics.md).</span></span>

## <span data-ttu-id="ecb3c-226"><a name="video"></a> Video</span><span class="sxs-lookup"><span data-stu-id="ecb3c-226"><a name="video"></a> Video</span></span>


> [!VIDEO https://channel9.msdn.com/events/Connect/2016/100/player]



## <span data-ttu-id="ecb3c-227"><a name="next"></a>Nästa steg</span><span class="sxs-lookup"><span data-stu-id="ecb3c-227"><a name="next"></a> Next steps</span></span>
* [<span data-ttu-id="ecb3c-228">Spåra användning</span><span class="sxs-lookup"><span data-stu-id="ecb3c-228">Track usage</span></span>](app-insights-web-track-usage.md)
* [<span data-ttu-id="ecb3c-229">Anpassade händelser och mätvärden</span><span class="sxs-lookup"><span data-stu-id="ecb3c-229">Custom events and metrics</span></span>](app-insights-api-custom-events-metrics.md)
* [<span data-ttu-id="ecb3c-230">Skapa – mät – lär</span><span class="sxs-lookup"><span data-stu-id="ecb3c-230">Build-measure-learn</span></span>](app-insights-web-track-usage.md)

