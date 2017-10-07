---
title: "aaaAzure Programinsikter för JavaScript-webbappar | Microsoft Docs"
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
ms.openlocfilehash: 986db3c3776471f9f8556f4e09f2d02aad022549
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="application-insights-for-web-pages"></a><span data-ttu-id="de17b-104">Application Insights för webbsidor</span><span class="sxs-lookup"><span data-stu-id="de17b-104">Application Insights for web pages</span></span>
<span data-ttu-id="de17b-105">Läs mer om hello prestanda och användning av din webbsida eller app.</span><span class="sxs-lookup"><span data-stu-id="de17b-105">Find out about hello performance and usage of your web page or app.</span></span> <span data-ttu-id="de17b-106">Om du lägger till [Programinsikter](app-insights-overview.md) tooyour sidan skript får du tidsinställningar för sidan läses in och AJAX-anrop, antal och information om webbläsarundantag och AJAX-fel som användare och session räknas.</span><span class="sxs-lookup"><span data-stu-id="de17b-106">If you add [Application Insights](app-insights-overview.md) tooyour page script, you get timings of page loads and AJAX calls, counts and details of browser exceptions and AJAX failures, as well as users and session counts.</span></span> <span data-ttu-id="de17b-107">Allt detta kan visas efter sida, klientoperativsystem- och webbläsarversion, geografisk plats och andra dimensioner.</span><span class="sxs-lookup"><span data-stu-id="de17b-107">All these can be segmented by page, client OS and browser version, geo location, and other dimensions.</span></span> <span data-ttu-id="de17b-108">Du kan ställa in varningar för antal fel eller långsam sidinläsning.</span><span class="sxs-lookup"><span data-stu-id="de17b-108">You can set alerts on failure counts or slow page loading.</span></span> <span data-ttu-id="de17b-109">Och infogar spårning av anrop i JavaScript-kod kan du spåra hur hello olika funktioner i tillämpningsprogrammet webbsida används.</span><span class="sxs-lookup"><span data-stu-id="de17b-109">And by inserting trace calls in your JavaScript code, you can track how hello different features of your web page application are used.</span></span>

<span data-ttu-id="de17b-110">Application Insights kan användas med alla webbsidor – du lägger bara till ett stycke JavaScript-kod.</span><span class="sxs-lookup"><span data-stu-id="de17b-110">Application Insights can be used with any web pages - you just add a short piece of JavaScript.</span></span> <span data-ttu-id="de17b-111">Om webbtjänsten är [Java](app-insights-java-get-started.md) eller [ASP.NET](app-insights-asp-net.md) kan du integrera telemetri från dina servrar och klienter.</span><span class="sxs-lookup"><span data-stu-id="de17b-111">If your web service is [Java](app-insights-java-get-started.md) or [ASP.NET](app-insights-asp-net.md), you can integrate telemetry from your server and clients.</span></span>

![Öppna appens resurs på portal.azure.com och klicka på Webbläsare](./media/app-insights-javascript/03.png)

<span data-ttu-id="de17b-113">Du behöver en prenumeration för[Microsoft Azure](https://azure.com).</span><span class="sxs-lookup"><span data-stu-id="de17b-113">You need a subscription too[Microsoft Azure](https://azure.com).</span></span> <span data-ttu-id="de17b-114">Om din grupp har en organisationens prenumeration kan du be hello ägare tooadd Account-tooit.</span><span class="sxs-lookup"><span data-stu-id="de17b-114">If your team has an organizational subscription, ask hello owner tooadd your Microsoft Account tooit.</span></span> <span data-ttu-id="de17b-115">Utveckling och småskalig användning kostar inget.</span><span class="sxs-lookup"><span data-stu-id="de17b-115">Development and small-scale use won't cost anything.</span></span>

## <a name="set-up-application-insights-for-your-web-page"></a><span data-ttu-id="de17b-116">Konfigurera Application Insights för din webbsida</span><span class="sxs-lookup"><span data-stu-id="de17b-116">Set up Application Insights for your web page</span></span>
<span data-ttu-id="de17b-117">Lägg till webbsidor för tooyour hello inläsaren kodfragment, enligt följande.</span><span class="sxs-lookup"><span data-stu-id="de17b-117">Add hello loader code snippet tooyour web pages, as follows.</span></span>

### <a name="open-or-create-application-insights-resource"></a><span data-ttu-id="de17b-118">Öppna eller skapa en Application Insights-resurs</span><span class="sxs-lookup"><span data-stu-id="de17b-118">Open or create Application Insights resource</span></span>
<span data-ttu-id="de17b-119">hello Application Insights-resurs är där data om prestanda och användning av din sida visas.</span><span class="sxs-lookup"><span data-stu-id="de17b-119">hello Application Insights resource is where data about your page's performance and usage is displayed.</span></span> 

<span data-ttu-id="de17b-120">Logga in på [Azure-portalen](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="de17b-120">Sign into [Azure portal](https://portal.azure.com).</span></span>

<span data-ttu-id="de17b-121">Om du redan har konfigurerat övervakning för hello på serversidan för din app, har du redan en resurs:</span><span class="sxs-lookup"><span data-stu-id="de17b-121">If you already set up monitoring for hello server side of your app, you already have a resource:</span></span>

![Välj Bläddra, Utvecklartjänster, Application Insights.](./media/app-insights-javascript/01-find.png)

<span data-ttu-id="de17b-123">Om du inte redan har en resurs skapar du en:</span><span class="sxs-lookup"><span data-stu-id="de17b-123">If you don't have one, create it:</span></span>

![Välj Nytt, Utvecklartjänster, Application Insights.](./media/app-insights-javascript/01-create.png)

<span data-ttu-id="de17b-125">*Har du redan frågor?*</span><span class="sxs-lookup"><span data-stu-id="de17b-125">*Questions already?*</span></span> <span data-ttu-id="de17b-126">[Mer information om hur du skapar en resurs](app-insights-create-new-resource.md).</span><span class="sxs-lookup"><span data-stu-id="de17b-126">[More about creating a resource](app-insights-create-new-resource.md).</span></span>

### <a name="add-hello-sdk-script-tooyour-app-or-web-pages"></a><span data-ttu-id="de17b-127">Lägg till hello SDK skriptet tooyour app eller webbsidor</span><span class="sxs-lookup"><span data-stu-id="de17b-127">Add hello SDK script tooyour app or web pages</span></span>
<span data-ttu-id="de17b-128">Hämta hello skript för webbsidor i Snabbstart:</span><span class="sxs-lookup"><span data-stu-id="de17b-128">In Quick Start, get hello script for web pages:</span></span>

![Välj Snabbstart, hämta koden toomonitor webbsidor på bladet översikt över din app.](./media/app-insights-javascript/02-monitor-web-page.png)

<span data-ttu-id="de17b-131">Infoga hello skript innan hello `</head>` taggen för varje sida som du vill tootrack.</span><span class="sxs-lookup"><span data-stu-id="de17b-131">Insert hello script just before hello `</head>` tag of every page you want tootrack.</span></span> <span data-ttu-id="de17b-132">Om din webbplats har en huvudsida, kan du placera hello skript det.</span><span class="sxs-lookup"><span data-stu-id="de17b-132">If your website has a master page, you can put hello script there.</span></span> <span data-ttu-id="de17b-133">Exempel:</span><span class="sxs-lookup"><span data-stu-id="de17b-133">For example:</span></span>

* <span data-ttu-id="de17b-134">I ett ASP.NET MVC-projekt lägger du till det i `View\Shared\_Layout.cshtml`</span><span class="sxs-lookup"><span data-stu-id="de17b-134">In an ASP.NET MVC project, you'd put it in `View\Shared\_Layout.cshtml`</span></span>
* <span data-ttu-id="de17b-135">Öppna i en SharePoint-webbplats på hello Kontrollpanelen [platsinställningar / huvudsida](app-insights-sharepoint.md).</span><span class="sxs-lookup"><span data-stu-id="de17b-135">In a SharePoint site, on hello control panel, open [Site Settings / Master Page](app-insights-sharepoint.md).</span></span>

<span data-ttu-id="de17b-136">hello skriptet innehåller hello instrumentation nyckeln som leder hello data tooyour Application Insights-resurs.</span><span class="sxs-lookup"><span data-stu-id="de17b-136">hello script contains hello instrumentation key that directs hello data tooyour Application Insights resource.</span></span> 

<span data-ttu-id="de17b-137">([Ingående förklaring av hello skript. ](http://apmtips.com/blog/2015/03/18/javascript-snippet-explained/))</span><span class="sxs-lookup"><span data-stu-id="de17b-137">([Deeper explanation of hello script.](http://apmtips.com/blog/2015/03/18/javascript-snippet-explained/))</span></span>

<span data-ttu-id="de17b-138">*(Om du använder ett välkänt ramverk för webbsidor letar du efter adaptrar till Application Insights. Det finns till exempel [en AngularJS-modul](http://ngmodules.org/modules/angular-appinsights).)*</span><span class="sxs-lookup"><span data-stu-id="de17b-138">*(If you're using a well-known web page framework, look around for Application Insights adaptors. For example, there's [an AngularJS module](http://ngmodules.org/modules/angular-appinsights).)*</span></span>

## <a name="detailed-configuration"></a><span data-ttu-id="de17b-139">Detaljerad konfiguration</span><span class="sxs-lookup"><span data-stu-id="de17b-139">Detailed configuration</span></span>
<span data-ttu-id="de17b-140">Det finns flera [Parametrar](https://github.com/Microsoft/ApplicationInsights-JS/blob/master/API-reference.md#config) som du kan ange, men i de flesta fall behöver du inte göra det.</span><span class="sxs-lookup"><span data-stu-id="de17b-140">There are several [parameters](https://github.com/Microsoft/ApplicationInsights-JS/blob/master/API-reference.md#config) you can set, though in most cases, you shouldn't need to.</span></span> <span data-ttu-id="de17b-141">Du kan till exempel inaktivera eller begränsa hello antalet Ajax-anrop som rapporteras per vyn sida (tooreduce trafik).</span><span class="sxs-lookup"><span data-stu-id="de17b-141">For example, you can disable or limit hello number of Ajax calls reported per page view (tooreduce traffic).</span></span> <span data-ttu-id="de17b-142">Alternativt kan du ange debug läge toohave telemetri flytta snabbt hello pipeline utan att grupperas.</span><span class="sxs-lookup"><span data-stu-id="de17b-142">Or you can set debug mode toohave telemetry move rapidly through hello pipeline without being batched.</span></span>

<span data-ttu-id="de17b-143">tooset parametrarna, leta efter den här raden i hello kodstycke och lägga till fler CSV-objekt efter att det:</span><span class="sxs-lookup"><span data-stu-id="de17b-143">tooset these parameters, look for this line in hello code snippet, and add more comma-separated items after it:</span></span>

    })({
      instrumentationKey: "..."
      // Insert here
    });

<span data-ttu-id="de17b-144">Hej [tillgängliga parametrar](https://github.com/Microsoft/ApplicationInsights-JS/blob/master/API-reference.md#config) omfattar:</span><span class="sxs-lookup"><span data-stu-id="de17b-144">hello [available parameters](https://github.com/Microsoft/ApplicationInsights-JS/blob/master/API-reference.md#config) include:</span></span>

    // Send telemetry immediately without batching.
    // Remember tooremove this when no longer required, as it
    // can affect browser performance.
    enableDebug: boolean,

    // Don't log browser exceptions.
    disableExceptionTracking: boolean,

    // Don't log ajax calls.
    disableAjaxTracking: boolean,

    // Limit number of Ajax calls logged, tooreduce traffic.
    maxAjaxCallsPerView: 10, // default is 500

    // Time page load up tooexecution of first trackPageView().
    overridePageViewDuration: boolean,

    // Set these dynamically for an authenticated user.
    appUserId: string,
    accountId: string,



## <span data-ttu-id="de17b-145"><a name="run"></a>Kör din app</span><span class="sxs-lookup"><span data-stu-id="de17b-145"><a name="run"></a>Run your app</span></span>
<span data-ttu-id="de17b-146">Köra ditt webbprogram, använder den en toogenerate telemetri och vänta ett par sekunder.</span><span class="sxs-lookup"><span data-stu-id="de17b-146">Run your web app, use it a while toogenerate telemetry, and wait a few seconds.</span></span> <span data-ttu-id="de17b-147">Du kan antingen köra den med hjälp av hello **F5** nyckeln på utvecklingsdatorn, eller publicera den och användarna kan spela upp till den.</span><span class="sxs-lookup"><span data-stu-id="de17b-147">You can either run it using hello **F5** key on your development machine, or publish it and let users play with it.</span></span>

<span data-ttu-id="de17b-148">Om du vill toocheck hello telemetri att ett webbprogram skickar tooApplication insikter, använda din webbläsare felsökningsverktyg (**F12** i många webbläsare).</span><span class="sxs-lookup"><span data-stu-id="de17b-148">If you want toocheck hello telemetry that a web app is sending tooApplication Insights, use your browser's debugging tools (**F12** on many browsers).</span></span> <span data-ttu-id="de17b-149">Informationen skickas toodc.services.visualstudio.com.</span><span class="sxs-lookup"><span data-stu-id="de17b-149">Data is sent toodc.services.visualstudio.com.</span></span>

## <a name="explore-your-browser-performance-data"></a><span data-ttu-id="de17b-150">Granska informationen om webbläsarens prestanda</span><span class="sxs-lookup"><span data-stu-id="de17b-150">Explore your browser performance data</span></span>
<span data-ttu-id="de17b-151">Öppna hello webbläsare bladet tooshow samman prestandadata från användarnas webbläsare.</span><span class="sxs-lookup"><span data-stu-id="de17b-151">Open hello Browser blade tooshow aggregated performance data from your users' browsers.</span></span>

![Öppna appens resurs på portal.azure.com och klicka på Inställningar, Webbläsare.](./media/app-insights-javascript/03.png)

<span data-ttu-id="de17b-153">*Ser du inga data än? Klicka på **uppdatera** hello överst på hello sidan. Ser du fortfarande ingenting? Mer information finns i [Felsökning](app-insights-troubleshoot-faq.md).*</span><span class="sxs-lookup"><span data-stu-id="de17b-153">*No data yet? Click **Refresh** at hello top of hello page. Still nothing? See [Troubleshooting](app-insights-troubleshoot-faq.md).*</span></span>

<span data-ttu-id="de17b-154">hello webbläsare bladet är en [Metrics Explorer-bladet](app-insights-metrics-explorer.md) med förinställda filter och diagraminställningar.</span><span class="sxs-lookup"><span data-stu-id="de17b-154">hello Browser blade is a [Metrics Explorer blade](app-insights-metrics-explorer.md) with preset filters and chart selections.</span></span> <span data-ttu-id="de17b-155">Du kan redigera hello tidsintervall, filter och diagram konfiguration om du vill och spara hello resultatet som en favorit.</span><span class="sxs-lookup"><span data-stu-id="de17b-155">You can edit hello time range, filters, and chart configuration if you want, and save hello result as a favorite.</span></span> <span data-ttu-id="de17b-156">Klicka på **Återställ standardvärden** tooget tillbaka toohello bladet originalkonfigurationen.</span><span class="sxs-lookup"><span data-stu-id="de17b-156">Click **Restore defaults** tooget back toohello original blade configuration.</span></span>

## <a name="page-load-performance"></a><span data-ttu-id="de17b-157">Sidinläsningsprestanda</span><span class="sxs-lookup"><span data-stu-id="de17b-157">Page load performance</span></span>
<span data-ttu-id="de17b-158">Vid hello är övre ett segmenterade sidinläsningstider-diagram.</span><span class="sxs-lookup"><span data-stu-id="de17b-158">At hello top is a segmented chart of page load times.</span></span> <span data-ttu-id="de17b-159">hello totala höjd hello diagrammet representerar hello genomsnittstiden tooload och visa sidor från din app i användarnas webbläsare.</span><span class="sxs-lookup"><span data-stu-id="de17b-159">hello total height of hello chart represents hello average time tooload and display pages from your app in your users' browsers.</span></span> <span data-ttu-id="de17b-160">hello tiden mäts från när hello webbläsaren skickar hello inledande HTTP-begäran tills alla synkron belastningen händelser har bearbetats, inklusive layout och skript som körs.</span><span class="sxs-lookup"><span data-stu-id="de17b-160">hello time is measured from when hello browser sends hello initial HTTP request until all synchronous load events have been processed, including layout and running scripts.</span></span> <span data-ttu-id="de17b-161">De omfattar inte asynkrona åtgärder, till exempel inläsning av webbdelar från AJAX-anrop.</span><span class="sxs-lookup"><span data-stu-id="de17b-161">It doesn't include asynchronous tasks such as loading web parts from AJAX calls.</span></span>

<span data-ttu-id="de17b-162">hello diagram segment hello totala sidinläsningstiden till hello [standard tidsinställningar som definieras av W3C](http://www.w3.org/TR/navigation-timing/#processing-model).</span><span class="sxs-lookup"><span data-stu-id="de17b-162">hello chart segments hello total page load time into hello [standard timings defined by W3C](http://www.w3.org/TR/navigation-timing/#processing-model).</span></span> 

![](./media/app-insights-javascript/08-client-split.png)

<span data-ttu-id="de17b-163">Observera att hello *nätverksanslutning* tid ofta är lägre än förväntat, eftersom det är ett medelvärde över alla förfrågningar från hello webbläsare toohello server.</span><span class="sxs-lookup"><span data-stu-id="de17b-163">Note that hello *network connect* time is often lower than you might expect, because it's an average over all requests from hello browser toohello server.</span></span> <span data-ttu-id="de17b-164">Många enskilda förfrågningar har en anslutningstid 0 eftersom det finns redan en aktiv anslutning toohello server.</span><span class="sxs-lookup"><span data-stu-id="de17b-164">Many individual requests have a connect time of 0 because there is already an active connection toohello server.</span></span>

### <a name="slow-loading"></a><span data-ttu-id="de17b-165">Är inläsningen långsam?</span><span class="sxs-lookup"><span data-stu-id="de17b-165">Slow loading?</span></span>
<span data-ttu-id="de17b-166">Långsamma sidinläsningar är ytterst frustrerande för användarna.</span><span class="sxs-lookup"><span data-stu-id="de17b-166">Slow page loads are a major source of dissatisfaction for your users.</span></span> <span data-ttu-id="de17b-167">Om hello diagram visar långsam sidan läses in, är det enkelt toodo vissa diagnostiska research.</span><span class="sxs-lookup"><span data-stu-id="de17b-167">If hello chart indicates slow page loads, it's easy toodo some diagnostic research.</span></span>

<span data-ttu-id="de17b-168">hello diagrammet visar hello medelvärdet av alla sidan läses in i din app.</span><span class="sxs-lookup"><span data-stu-id="de17b-168">hello chart shows hello average of all page loads in your app.</span></span> <span data-ttu-id="de17b-169">toosee om hello problem är begränsad tooparticular sidor, leta ytterligare ned hello-bladet, där det finns ett rutnät åtskilda med sidan URL:</span><span class="sxs-lookup"><span data-stu-id="de17b-169">toosee if hello problem is confined tooparticular pages, look further down hello blade, where there's a grid segmented by page URL:</span></span>

![](./media/app-insights-javascript/09-page-perf.png)

<span data-ttu-id="de17b-170">Observera hello sidan Visa antalet och standardavvikelsen.</span><span class="sxs-lookup"><span data-stu-id="de17b-170">Notice hello page view count and standard deviation.</span></span> <span data-ttu-id="de17b-171">Om antalet sidor för hello är mycket låg, sedan påverkar hello problemet inte användarna mycket.</span><span class="sxs-lookup"><span data-stu-id="de17b-171">If hello page count is very low, then hello issue isn't affecting users much.</span></span> <span data-ttu-id="de17b-172">En hög standardavvikelse (jämförbara toohello genomsnittlig själva) anger variationen mellan individuella mätningar mycket.</span><span class="sxs-lookup"><span data-stu-id="de17b-172">A high standard deviation (comparable toohello average itself) indicates a lot of variation between individual measurements.</span></span>

<span data-ttu-id="de17b-173">**Zooma in en URL och en sidvisning.**</span><span class="sxs-lookup"><span data-stu-id="de17b-173">**Zoom in on one URL and one page view.**</span></span> <span data-ttu-id="de17b-174">Klicka på varje sida namnet toosee ett blad av webbläsaren diagram filtrerade bara toothat URL; och sedan på en instans för en sida.</span><span class="sxs-lookup"><span data-stu-id="de17b-174">Click any page name toosee a blade of browser charts filtered just toothat URL; and then on an instance of a page view.</span></span>

![](./media/app-insights-javascript/35.png)

<span data-ttu-id="de17b-175">Klicka på `...` för en fullständig lista över egenskaper för den händelsen eller granska hello Ajax-anrop och relaterade händelser.</span><span class="sxs-lookup"><span data-stu-id="de17b-175">Click `...` for a full list of properties for that event, or inspect hello Ajax calls and related events.</span></span> <span data-ttu-id="de17b-176">Långsamma Ajax-anrop påverkar hello övergripande sidan inläsningstid om de är synkron.</span><span class="sxs-lookup"><span data-stu-id="de17b-176">Slow Ajax calls affect hello overall page load time if they are synchronous.</span></span> <span data-ttu-id="de17b-177">Relaterade händelser inkluderar serverbegäranden efter Hej samma URL (om du har konfigurerat Application Insights på din webbserver).</span><span class="sxs-lookup"><span data-stu-id="de17b-177">Related events include server requests for hello same URL (if you've set up Application Insights on your web server).</span></span>

<span data-ttu-id="de17b-178">**Sidprestanda över tid.**</span><span class="sxs-lookup"><span data-stu-id="de17b-178">**Page performance over time.**</span></span> <span data-ttu-id="de17b-179">Ändra hello inläsningstid för sidvisning rutnätet till en rad diagram toosee tillbaka på hello webbläsare bladet om det fanns toppar vid en viss tidpunkt:</span><span class="sxs-lookup"><span data-stu-id="de17b-179">Back at hello Browsers blade, change hello Page View Load Time grid into a line chart toosee if there were peaks at particular times:</span></span>

![Klicka på hello huvud hello rutnätet och välj en ny diagramtyp av](./media/app-insights-javascript/10-page-perf-area.png)

<span data-ttu-id="de17b-181">**Ändra uppdelningen baserat på andra dimensioner.**</span><span class="sxs-lookup"><span data-stu-id="de17b-181">**Segment by other dimensions.**</span></span> <span data-ttu-id="de17b-182">Sidorna är kanske långsammare tooload på webbläsare, klientoperativsystem eller användaren lokalt?</span><span class="sxs-lookup"><span data-stu-id="de17b-182">Maybe your pages are slower tooload on a particular browser, client OS, or user locality?</span></span> <span data-ttu-id="de17b-183">Lägg till ett nytt diagram och experimentera med hello **Group by** dimension.</span><span class="sxs-lookup"><span data-stu-id="de17b-183">Add a new chart and experiment with hello **Group-by** dimension.</span></span>

![](./media/app-insights-javascript/21.png)

## <a name="ajax-performance"></a><span data-ttu-id="de17b-184">AJAX-prestanda</span><span class="sxs-lookup"><span data-stu-id="de17b-184">AJAX Performance</span></span>
<span data-ttu-id="de17b-185">Kontrollera att alla AJAX-anrop på dina webbsidor fungerar som de ska.</span><span class="sxs-lookup"><span data-stu-id="de17b-185">Make sure any AJAX calls in your web pages are performing well.</span></span> <span data-ttu-id="de17b-186">De är ofta använda toofill delar av sidan asynkront.</span><span class="sxs-lookup"><span data-stu-id="de17b-186">They are often used toofill parts of your page asynchronously.</span></span> <span data-ttu-id="de17b-187">Även om hello övergripande kan sidinläsning omedelbart, kan dina användare vara motverkades som startar vid tomt webbdelar, väntar på att data tooappear i dem.</span><span class="sxs-lookup"><span data-stu-id="de17b-187">Although hello overall page might load promptly, your users could be frustrated by staring at blank web parts, waiting for data tooappear in them.</span></span>

<span data-ttu-id="de17b-188">AJAX-anrop från webbsidan visas på hello webbläsare blad som beroenden.</span><span class="sxs-lookup"><span data-stu-id="de17b-188">AJAX calls made from your web page are shown on hello Browsers blade as dependencies.</span></span>

<span data-ttu-id="de17b-189">Det finns översiktlig diagram i hello övre delen av hello bladet:</span><span class="sxs-lookup"><span data-stu-id="de17b-189">There are summary charts in hello upper part of hello blade:</span></span>

![](./media/app-insights-javascript/31.png)

<span data-ttu-id="de17b-190">Och detaljerade rutnät längre ned:</span><span class="sxs-lookup"><span data-stu-id="de17b-190">and detailed grids lower down:</span></span>

![](./media/app-insights-javascript/33.png)

<span data-ttu-id="de17b-191">Klicka på en rad om du vill visa specifik information.</span><span class="sxs-lookup"><span data-stu-id="de17b-191">Click any row for specific details.</span></span>

> [!NOTE]
> <span data-ttu-id="de17b-192">Om du tar bort hello webbläsare filter på hello-bladet, ingår både servern och AJAX-beroenden i dessa diagram.</span><span class="sxs-lookup"><span data-stu-id="de17b-192">If you delete hello Browsers filter on hello blade, both server and AJAX dependencies are included in these charts.</span></span> <span data-ttu-id="de17b-193">Klicka på Återställ standardvärden tooreconfigure hello filter.</span><span class="sxs-lookup"><span data-stu-id="de17b-193">Click Restore Defaults tooreconfigure hello filter.</span></span>
> 
> 

<span data-ttu-id="de17b-194">**toodrill till misslyckade Ajax-anrop** toohello beroende fel rutnätet rullar och klicka sedan på en rad toosee specifika instanser.</span><span class="sxs-lookup"><span data-stu-id="de17b-194">**toodrill into failed Ajax calls** scroll down toohello Dependency failures grid, and then click a row toosee specific instances.</span></span>

![](./media/app-insights-javascript/37.png)


<span data-ttu-id="de17b-195">Klicka på `...` för hello fullständig telemetri för Ajax-anrop.</span><span class="sxs-lookup"><span data-stu-id="de17b-195">Click `...` for hello full telemetry for an Ajax call.</span></span>

### <a name="no-ajax-calls-reported"></a><span data-ttu-id="de17b-196">Rapporteras inga Ajax-anrop?</span><span class="sxs-lookup"><span data-stu-id="de17b-196">No Ajax calls reported?</span></span>
<span data-ttu-id="de17b-197">AJAX-anrop inkluderar alla HTTP/HTTPS-anrop från hello skript på webbsidan.</span><span class="sxs-lookup"><span data-stu-id="de17b-197">Ajax calls include any HTTP/HTTPS  calls made from hello script of your web page.</span></span> <span data-ttu-id="de17b-198">Om du inte ser dem rapporterade, kontrollera att hello kodstycke inte hello `disableAjaxTracking` eller `maxAjaxCallsPerView` [parametrar](https://github.com/Microsoft/ApplicationInsights-JS/blob/master/API-reference.md#config).</span><span class="sxs-lookup"><span data-stu-id="de17b-198">If you don't see them reported, check that hello code snippet doesn't set hello `disableAjaxTracking` or `maxAjaxCallsPerView` [parameters](https://github.com/Microsoft/ApplicationInsights-JS/blob/master/API-reference.md#config).</span></span>

## <a name="browser-exceptions"></a><span data-ttu-id="de17b-199">Webbläsarundantag</span><span class="sxs-lookup"><span data-stu-id="de17b-199">Browser exceptions</span></span>
<span data-ttu-id="de17b-200">På bladet webbläsare hello finns ett undantag sammanfattning diagram och ett rutnät av undantag typer ytterligare ned hello-bladet.</span><span class="sxs-lookup"><span data-stu-id="de17b-200">On hello Browsers blade, there's an exceptions summary chart, and a grid of exception types further down hello blade.</span></span>

![](./media/app-insights-javascript/39.png)

<span data-ttu-id="de17b-201">Om du inte ser webbläsarundantag rapporterade, kontrollera att hello kodstycke inte hello `disableExceptionTracking` [parametern](https://github.com/Microsoft/ApplicationInsights-JS/blob/master/API-reference.md#config).</span><span class="sxs-lookup"><span data-stu-id="de17b-201">If you don't see browser exceptions reported, check that hello code snippet doesn't set hello `disableExceptionTracking` [parameter](https://github.com/Microsoft/ApplicationInsights-JS/blob/master/API-reference.md#config).</span></span>

## <a name="inspect-individual-page-view-events"></a><span data-ttu-id="de17b-202">Granska enskilda sidvisningshändelser</span><span class="sxs-lookup"><span data-stu-id="de17b-202">Inspect individual page view events</span></span>

<span data-ttu-id="de17b-203">Vanligtvis analyseras telemetri för sidvisningar av Application Insights och du ser endast kumulativa rapporter, som ett genomsnitt av alla användare.</span><span class="sxs-lookup"><span data-stu-id="de17b-203">Usually page view telemetry is analyzed by Application Insights and you see only cumulative reports, averaged over all your users.</span></span> <span data-ttu-id="de17b-204">Men för felsökningsändamål kan du även titta på enskilda sidvisningshändelser.</span><span class="sxs-lookup"><span data-stu-id="de17b-204">But for debugging purposes, you can also look at individual page view events.</span></span>

<span data-ttu-id="de17b-205">Ange filter tooPage vyn i hello diagnostiska Sök-bladet.</span><span class="sxs-lookup"><span data-stu-id="de17b-205">In hello Diagnostic Search blade, set Filters tooPage View.</span></span>

![](./media/app-insights-javascript/12-search-pages.png)

<span data-ttu-id="de17b-206">Välj en händelse toosee detalj.</span><span class="sxs-lookup"><span data-stu-id="de17b-206">Select any event toosee more detail.</span></span> <span data-ttu-id="de17b-207">Klicka på ”...” toosee även detalj hello information på sidan.</span><span class="sxs-lookup"><span data-stu-id="de17b-207">In hello details page, click "..." toosee even more detail.</span></span>

> [!NOTE]
> <span data-ttu-id="de17b-208">Om du använder [Sök](app-insights-diagnostic-search.md), Observera att du har toomatch hela ord: ”Abou” och ”om” matchar inte ”om”.</span><span class="sxs-lookup"><span data-stu-id="de17b-208">If you use [Search](app-insights-diagnostic-search.md), notice that you have toomatch whole words: "Abou" and "bout" do not match "About".</span></span>
> 
> 

<span data-ttu-id="de17b-209">Du kan också använda hello kraftfulla [Log Analytics-frågespråket](https://docs.microsoft.com/azure/application-insights/app-insights-analytics-tour#browser-timings-table) toosearch sidvisningar.</span><span class="sxs-lookup"><span data-stu-id="de17b-209">You can also use hello powerful [Log Analytics query language](https://docs.microsoft.com/azure/application-insights/app-insights-analytics-tour#browser-timings-table) toosearch page views.</span></span>

### <a name="page-view-properties"></a><span data-ttu-id="de17b-210">Egenskaper för sidvisning</span><span class="sxs-lookup"><span data-stu-id="de17b-210">Page view properties</span></span>
* <span data-ttu-id="de17b-211">**Sidvisningens varaktighet**</span><span class="sxs-lookup"><span data-stu-id="de17b-211">**Page view duration**</span></span> 
  
  * <span data-ttu-id="de17b-212">Som standard hello tid det tar tooload hello sida, ladda från klienten begäran toofull (inklusive extra filer men exklusive asynkrona åtgärder, till exempel Ajax-anrop).</span><span class="sxs-lookup"><span data-stu-id="de17b-212">By default, hello time it takes tooload hello page, from client request toofull load (including auxiliary files but excluding asynchronous tasks such as Ajax calls).</span></span> 
  * <span data-ttu-id="de17b-213">Om du ställer in `overridePageViewDuration` i hello [sidan configuration](#detailed-configuration), hello intervallet mellan klienten begäran tooexecution av hello först `trackPageView`.</span><span class="sxs-lookup"><span data-stu-id="de17b-213">If you set `overridePageViewDuration` in hello [page configuration](#detailed-configuration), hello interval between client request tooexecution of hello first `trackPageView`.</span></span> <span data-ttu-id="de17b-214">Om du flyttade trackPageView från dess normala position efter hello initiering av hello skript, visas ett annat värde.</span><span class="sxs-lookup"><span data-stu-id="de17b-214">If you moved trackPageView from its usual position after hello initialization of hello script, it will reflect a different value.</span></span>
  * <span data-ttu-id="de17b-215">Om `overridePageViewDuration` anges, och en varaktighet som argument har angetts i hello `trackPageView()` anropa hello argumentvärdet används i stället.</span><span class="sxs-lookup"><span data-stu-id="de17b-215">If `overridePageViewDuration` is set and a duration argument is provided in hello `trackPageView()` call, then hello argument value is used instead.</span></span> 

## <a name="custom-page-counts"></a><span data-ttu-id="de17b-216">Anpassade sidräkningar</span><span class="sxs-lookup"><span data-stu-id="de17b-216">Custom page counts</span></span>
<span data-ttu-id="de17b-217">Ett antal sidor sker som standard varje gång en ny sida läses in i hello klientens webbläsare.</span><span class="sxs-lookup"><span data-stu-id="de17b-217">By default, a page count occurs each time a new page loads into hello client browser.</span></span>  <span data-ttu-id="de17b-218">Men du kanske vill toocount ytterligare sidvisningar.</span><span class="sxs-lookup"><span data-stu-id="de17b-218">But you might want toocount additional page views.</span></span> <span data-ttu-id="de17b-219">Till exempel en sida kan visa innehållet i flikar och du vill toocount en sida när hello användare växlar flikar.</span><span class="sxs-lookup"><span data-stu-id="de17b-219">For example, a page might display its content in tabs and you want toocount a page when hello user switches tabs.</span></span> <span data-ttu-id="de17b-220">Eller JavaScript-kod på sidan hello kan läsa in nytt innehåll utan att ändra hello webbläsarens URL.</span><span class="sxs-lookup"><span data-stu-id="de17b-220">Or JavaScript code in hello page might load new content without changing hello browser's URL.</span></span>

<span data-ttu-id="de17b-221">Infoga ett JavaScript-anrop så här på hello lämplig plats i din klientkod:</span><span class="sxs-lookup"><span data-stu-id="de17b-221">Insert a JavaScript call like this at hello appropriate point in your client code:</span></span>

    appInsights.trackPageView(myPageName);

<span data-ttu-id="de17b-222">hello sidnamn kan innehålla hello samma tecken som en URL, men något annat efter ”#” eller ””? ignoreras.</span><span class="sxs-lookup"><span data-stu-id="de17b-222">hello page name can contain hello same characters as a URL, but anything after "#" or "?" is ignored.</span></span>

## <a name="usage-tracking"></a><span data-ttu-id="de17b-223">Användningsspårning</span><span class="sxs-lookup"><span data-stu-id="de17b-223">Usage tracking</span></span>
<span data-ttu-id="de17b-224">Vill du toofind reda på vad användarna göra med din app?</span><span class="sxs-lookup"><span data-stu-id="de17b-224">Want toofind out what your users do with your app?</span></span>

* [<span data-ttu-id="de17b-225">Lär dig mer om användningsspårning</span><span class="sxs-lookup"><span data-stu-id="de17b-225">Learn about usage tracking</span></span>](app-insights-web-track-usage.md)
* <span data-ttu-id="de17b-226">[Lär dig mer om API:er för mätvärden och anpassade händelser](app-insights-api-custom-events-metrics.md).</span><span class="sxs-lookup"><span data-stu-id="de17b-226">[Learn about custom events and metrics API](app-insights-api-custom-events-metrics.md).</span></span>

## <span data-ttu-id="de17b-227"><a name="video"></a> Video</span><span class="sxs-lookup"><span data-stu-id="de17b-227"><a name="video"></a> Video</span></span>


> [!VIDEO https://channel9.msdn.com/events/Connect/2016/100/player]



## <span data-ttu-id="de17b-228"><a name="next"></a>Nästa steg</span><span class="sxs-lookup"><span data-stu-id="de17b-228"><a name="next"></a> Next steps</span></span>
* [<span data-ttu-id="de17b-229">Spåra användning</span><span class="sxs-lookup"><span data-stu-id="de17b-229">Track usage</span></span>](app-insights-web-track-usage.md)
* [<span data-ttu-id="de17b-230">Anpassade händelser och mätvärden</span><span class="sxs-lookup"><span data-stu-id="de17b-230">Custom events and metrics</span></span>](app-insights-api-custom-events-metrics.md)
* [<span data-ttu-id="de17b-231">Skapa – mät – lär</span><span class="sxs-lookup"><span data-stu-id="de17b-231">Build-measure-learn</span></span>](app-insights-web-track-usage.md)

