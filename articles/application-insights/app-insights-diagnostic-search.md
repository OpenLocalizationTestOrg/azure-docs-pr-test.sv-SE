---
title: "Använda sökning i Azure Application Insights | Microsoft Docs"
description: "Sök och filtrera rådata telemetri som skickas av ditt webbprogram."
services: application-insights
documentationcenter: 
author: CFreemanwa
manager: carmonm
ms.assetid: 2a437555-8043-45ec-937a-225c9bf0066b
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 03/14/2017
ms.author: bwren
ms.openlocfilehash: e2d12f807756b778a64920b12a66fba184a99844
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/18/2017
---
# <a name="using-search-in-application-insights"></a><span data-ttu-id="1114c-103">Använda sökning i Application Insights</span><span class="sxs-lookup"><span data-stu-id="1114c-103">Using Search in Application Insights</span></span>
<span data-ttu-id="1114c-104">Sökningen är en funktion i [Programinsikter](app-insights-overview.md) att du använder för att söka efter och utforska enskilda telemetri objekt, till exempel sidvyer undantag, eller webbförfrågningar.</span><span class="sxs-lookup"><span data-stu-id="1114c-104">Search is a feature of [Application Insights](app-insights-overview.md) that you use to find and explore individual telemetry items, such as page views, exceptions, or web requests.</span></span> <span data-ttu-id="1114c-105">Och du kan visa loggspårningar och händelser som du har kodats.</span><span class="sxs-lookup"><span data-stu-id="1114c-105">And you can view log traces and events that you have coded.</span></span>

<span data-ttu-id="1114c-106">(Mer komplexa frågor över dina data, Använd [Analytics](app-insights-analytics-tour.md).)</span><span class="sxs-lookup"><span data-stu-id="1114c-106">(For more complex queries over your data, use [Analytics](app-insights-analytics-tour.md).)</span></span>

## <a name="where-do-you-see-search"></a><span data-ttu-id="1114c-107">Där kan du se sökningen?</span><span class="sxs-lookup"><span data-stu-id="1114c-107">Where do you see Search?</span></span>
### <a name="in-the-azure-portal"></a><span data-ttu-id="1114c-108">I Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="1114c-108">In the Azure portal</span></span>
<span data-ttu-id="1114c-109">Du kan öppna diagnostiska sökning uttryckligen från bladet översikt över Application Insights i ditt program:</span><span class="sxs-lookup"><span data-stu-id="1114c-109">You can open diagnostic search explicitly from the Application Insights Overview blade of your application:</span></span>

![Öppna diagnostiska sökning](./media/app-insights-diagnostic-search/01-open-Diagnostic.png)

<span data-ttu-id="1114c-111">Det kan öppnas när du klickar på via några diagram och rutnät artiklar.</span><span class="sxs-lookup"><span data-stu-id="1114c-111">It also opens when you click through some charts and grid items.</span></span> <span data-ttu-id="1114c-112">I det här fallet ställs inför dess filter att fokusera på typ av objekt du valt.</span><span class="sxs-lookup"><span data-stu-id="1114c-112">In this case, its filters are pre-set to focus on the type of item you selected.</span></span> 

<span data-ttu-id="1114c-113">Till exempel på bladet översikt finns ett stapeldiagram begäranden som klassificerats av svarstid.</span><span class="sxs-lookup"><span data-stu-id="1114c-113">For example, on the Overview blade, there's a bar chart of requests classified by response time.</span></span> <span data-ttu-id="1114c-114">Klicka dig igenom ett prestanda adressintervall som ska visa en lista med enskilda förfrågningar i tidsintervall som svar:</span><span class="sxs-lookup"><span data-stu-id="1114c-114">Click through a performance range to see a list of individual requests in that response time range:</span></span>

![Klicka dig igenom begäran prestanda](./media/app-insights-diagnostic-search/07-open-from-filters.png)

<span data-ttu-id="1114c-116">Huvuddelen av diagnostiska Search är en lista över telemetri objekt - serverbegäranden, sidan vyer, anpassade händelser som du har ett kodat och så vidare.</span><span class="sxs-lookup"><span data-stu-id="1114c-116">The main body of Diagnostic Search is a list of telemetry items - server requests, page views, custom events that you have coded, and so on.</span></span> <span data-ttu-id="1114c-117">Överst i listan är en sammanfattande diagram som visar antalet händelser över tid.</span><span class="sxs-lookup"><span data-stu-id="1114c-117">At the top of the list is a summary chart showing counts of events over time.</span></span>

<span data-ttu-id="1114c-118">Klicka på Uppdatera för att få nya händelser.</span><span class="sxs-lookup"><span data-stu-id="1114c-118">Click Refresh to get new events.</span></span>

### <a name="in-visual-studio"></a><span data-ttu-id="1114c-119">I Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="1114c-119">In Visual Studio</span></span>

<span data-ttu-id="1114c-120">I Visual Studio finns också en Application Insights sökfönstret.</span><span class="sxs-lookup"><span data-stu-id="1114c-120">In Visual Studio, there's also an Application Insights Search window.</span></span> <span data-ttu-id="1114c-121">Det är mest användbara för att visa telemetri händelser som genererats av det program som du felsöker.</span><span class="sxs-lookup"><span data-stu-id="1114c-121">It's most useful for displaying telemetry events generated by the application that you're debugging.</span></span> <span data-ttu-id="1114c-122">Men det kan också visa händelser som samlas in från din publicerade app på Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="1114c-122">But it can also show the events collected from your published app at the Azure portal.</span></span>

<span data-ttu-id="1114c-123">Öppna fönstret Sök i Visual Studio:</span><span class="sxs-lookup"><span data-stu-id="1114c-123">Open the Search window in Visual Studio:</span></span>

![Visual Studio öppna Application Insights-sökning](./media/app-insights-diagnostic-search/32.png)

<span data-ttu-id="1114c-125">Fönstret har funktioner som liknar webbportalen:</span><span class="sxs-lookup"><span data-stu-id="1114c-125">The Search window has features similar to the web portal:</span></span>

![Visual Studio Application Insights sökfönstret](./media/app-insights-diagnostic-search/34.png)

<span data-ttu-id="1114c-127">Fliken Spåra åtgärden är tillgänglig när du öppnar en begäran eller vyn sida.</span><span class="sxs-lookup"><span data-stu-id="1114c-127">The Track Operation tab is available when you open a request or a page view.</span></span> <span data-ttu-id="1114c-128">Ett ' åtgärden ' är en sekvens av händelser som är associerad med en enda vy för begäran eller sidan.</span><span class="sxs-lookup"><span data-stu-id="1114c-128">An 'operation' is a sequence of events that is associated with to a single request or page view.</span></span> <span data-ttu-id="1114c-129">Till exempel kan beroendeanrop, undantag, loggar och anpassade händelser ingå i en enda åtgärd.</span><span class="sxs-lookup"><span data-stu-id="1114c-129">For example, dependency calls, exceptions, trace logs, and custom events might be part of a single operation.</span></span> <span data-ttu-id="1114c-130">Fliken Spåra åtgärden visar grafiskt tidsinställning och varaktighet för dessa händelser i förhållande till vyn begäran eller sidan.</span><span class="sxs-lookup"><span data-stu-id="1114c-130">The Track Operation tab shows graphically the timing and duration of these events in relation to the request or page view.</span></span> 

## <a name="inspect-individual-items"></a><span data-ttu-id="1114c-131">Inspektera enskilda objekt</span><span class="sxs-lookup"><span data-stu-id="1114c-131">Inspect individual items</span></span>
<span data-ttu-id="1114c-132">Välj en telemetri post för att se nyckelfält och relaterade objekt.</span><span class="sxs-lookup"><span data-stu-id="1114c-132">Select any telemetry item to see key fields and related items.</span></span> <span data-ttu-id="1114c-133">Klicka på ”...” Om du vill se en fullständig uppsättning fält.</span><span class="sxs-lookup"><span data-stu-id="1114c-133">If you want to see the full set of fields, click "...".</span></span> 

![Klicka på nytt arbetsobjekt, redigera fälten och klicka sedan på OK.](./media/app-insights-diagnostic-search/10-detail.png)

## <a name="filter-event-types"></a><span data-ttu-id="1114c-135">Filtrera händelsetyper</span><span class="sxs-lookup"><span data-stu-id="1114c-135">Filter event types</span></span>
<span data-ttu-id="1114c-136">Öppna bladet Filter och välj händelsetyper som du vill se.</span><span class="sxs-lookup"><span data-stu-id="1114c-136">Open the Filter blade and choose the event types you want to see.</span></span> <span data-ttu-id="1114c-137">(Om du senare vill du återställa filtren som du har öppnat bladet, klickar du på Återställ.)</span><span class="sxs-lookup"><span data-stu-id="1114c-137">(If, later, you want to restore the filters with which you opened the blade, click Reset.)</span></span>

![Välj Filter och välj telemetri typer](./media/app-insights-diagnostic-search/02-filter-req.png)

<span data-ttu-id="1114c-139">Händelsetyper är:</span><span class="sxs-lookup"><span data-stu-id="1114c-139">The event types are:</span></span>

* <span data-ttu-id="1114c-140">**Spåra** - [diagnostikloggar](app-insights-asp-net-trace-logs.md) inklusive TrackTrace, log4Net, NLog och System.Diagnostic.Trace anrop.</span><span class="sxs-lookup"><span data-stu-id="1114c-140">**Trace** - [Diagnostic logs](app-insights-asp-net-trace-logs.md) including TrackTrace, log4Net, NLog, and System.Diagnostic.Trace calls.</span></span>
* <span data-ttu-id="1114c-141">**Begära** -HTTP-begäranden tas emot av serverprogrammet, inklusive sidor, skript, bilder, filer och data.</span><span class="sxs-lookup"><span data-stu-id="1114c-141">**Request** - HTTP requests received by your server application, including pages, scripts, images, style files, and data.</span></span> <span data-ttu-id="1114c-142">Dessa händelser används för att skapa förfrågan och svar översikt över diagram.</span><span class="sxs-lookup"><span data-stu-id="1114c-142">These events are used to create the request and response overview charts.</span></span>
* <span data-ttu-id="1114c-143">**Vyn sida** - [telemetri som skickas av webbklienten](app-insights-javascript.md), som används för att skapa sidan Visa rapporter.</span><span class="sxs-lookup"><span data-stu-id="1114c-143">**Page View** - [Telemetry sent by the web client](app-insights-javascript.md), used to create page view reports.</span></span> 
* <span data-ttu-id="1114c-144">**Anpassad händelse** - om du har infogat anrop till trackevent () för att [övervaka användningen](app-insights-api-custom-events-metrics.md), kan du söka dem här.</span><span class="sxs-lookup"><span data-stu-id="1114c-144">**Custom Event** - If you inserted calls to TrackEvent() in order to [monitor usage](app-insights-api-custom-events-metrics.md), you can search them here.</span></span>
* <span data-ttu-id="1114c-145">**Undantag** – utan felhantering [undantag i servern](app-insights-asp-net-exceptions.md), och de som du loggar in med hjälp av TrackException().</span><span class="sxs-lookup"><span data-stu-id="1114c-145">**Exception** - Uncaught [exceptions in the server](app-insights-asp-net-exceptions.md), and those that you log by using TrackException().</span></span>
* <span data-ttu-id="1114c-146">**Beroende** - [anrop från serverprogrammet](app-insights-asp-net-dependencies.md) till andra tjänster som REST API: er eller databaser och AJAX-anrop från din [klientkod](app-insights-javascript.md).</span><span class="sxs-lookup"><span data-stu-id="1114c-146">**Dependency** - [Calls from your server application](app-insights-asp-net-dependencies.md) to other services such as REST APIs or databases, and AJAX calls from your [client code](app-insights-javascript.md).</span></span>
* <span data-ttu-id="1114c-147">**Tillgänglighet** -resultaten av [tillgänglighetstester](app-insights-monitor-web-app-availability.md).</span><span class="sxs-lookup"><span data-stu-id="1114c-147">**Availability** - Results of [availability tests](app-insights-monitor-web-app-availability.md).</span></span>

## <a name="filter-on-property-values"></a><span data-ttu-id="1114c-148">Filtrera på egenskapsvärden</span><span class="sxs-lookup"><span data-stu-id="1114c-148">Filter on property values</span></span>
<span data-ttu-id="1114c-149">Du kan filtrera händelser på värdena i deras egenskaper.</span><span class="sxs-lookup"><span data-stu-id="1114c-149">You can filter events on the values of their properties.</span></span> <span data-ttu-id="1114c-150">De tillgängliga egenskaperna är beroende av händelsetyper som du har valt.</span><span class="sxs-lookup"><span data-stu-id="1114c-150">The available properties depend on the event types you selected.</span></span> 

<span data-ttu-id="1114c-151">Till exempel välja ut begäranden med en specifik svarskod.</span><span class="sxs-lookup"><span data-stu-id="1114c-151">For example, pick out requests with a specific response code.</span></span> 

![Expandera en egenskap och väljer ett värde](./media/app-insights-diagnostic-search/03-response500.png)

<span data-ttu-id="1114c-153">Om du väljer några värden för en viss egenskap har samma effekt som att välja alla värden.</span><span class="sxs-lookup"><span data-stu-id="1114c-153">Choosing no values of a particular property has the same effect as choosing all values.</span></span> <span data-ttu-id="1114c-154">Den växlar av filtreringen för den egenskapen.</span><span class="sxs-lookup"><span data-stu-id="1114c-154">It switches off filtering on that property.</span></span>

### <a name="narrow-your-search"></a><span data-ttu-id="1114c-155">Begränsa sökningen</span><span class="sxs-lookup"><span data-stu-id="1114c-155">Narrow your search</span></span>
<span data-ttu-id="1114c-156">Observera att antalet till höger om filtervärdena visas hur många förekomster det finns i den aktuella filtrerade uppsättningen.</span><span class="sxs-lookup"><span data-stu-id="1114c-156">Notice that the counts to the right of the filter values show how many occurrences there are in the current filtered set.</span></span> 

<span data-ttu-id="1114c-157">Det är tydligt att begära 'Rpthuvud/anställda' resultat i de flesta av fel som ”500” i det här exemplet:</span><span class="sxs-lookup"><span data-stu-id="1114c-157">In this example, it's clear that the 'Rpt/Employees' request results in most of the '500' errors:</span></span>

![Expandera en egenskap och väljer ett värde](./media/app-insights-diagnostic-search/04-failingReq.png)




## <a name="find-events-with-the-same-property"></a><span data-ttu-id="1114c-159">Sök efter händelser med samma egenskap</span><span class="sxs-lookup"><span data-stu-id="1114c-159">Find events with the same property</span></span>
<span data-ttu-id="1114c-160">Sök efter alla objekt med samma egenskapsvärde för:</span><span class="sxs-lookup"><span data-stu-id="1114c-160">Find all the items with the same property value:</span></span>

![Högerklicka på en egenskap](./media/app-insights-diagnostic-search/12-samevalue.png)


## <a name="search-the-data"></a><span data-ttu-id="1114c-162">Söka efter data</span><span class="sxs-lookup"><span data-stu-id="1114c-162">Search the data</span></span>

> [!NOTE]
> <span data-ttu-id="1114c-163">Om du vill skriva mer komplexa frågor, öppna [ **Analytics** ](app-insights-analytics-tour.md) högst upp på bladet sökning.</span><span class="sxs-lookup"><span data-stu-id="1114c-163">To write more complex queries, open [**Analytics**](app-insights-analytics-tour.md) from the top of the Search blade.</span></span>
> 

<span data-ttu-id="1114c-164">Du kan söka efter villkoren i någon av egenskapsvärden.</span><span class="sxs-lookup"><span data-stu-id="1114c-164">You can search for terms in any of the property values.</span></span> <span data-ttu-id="1114c-165">Detta är särskilt användbart om du har skrivit [anpassade händelser](app-insights-api-custom-events-metrics.md) med egenskapsvärden.</span><span class="sxs-lookup"><span data-stu-id="1114c-165">This is particularly useful if you have written [custom events](app-insights-api-custom-events-metrics.md) with property values.</span></span> 

<span data-ttu-id="1114c-166">Du kanske vill ange en tid som intervall, som söker över ett kortare intervall är snabbare.</span><span class="sxs-lookup"><span data-stu-id="1114c-166">You might want to set a time range, as searches over a shorter range are faster.</span></span> 

![Öppna diagnostiska sökning](./media/app-insights-diagnostic-search/appinsights-311search.png)

<span data-ttu-id="1114c-168">Sök efter fullständiga ord, inte delsträngar.</span><span class="sxs-lookup"><span data-stu-id="1114c-168">Search for complete words, not substrings.</span></span> <span data-ttu-id="1114c-169">Använd citattecken för att omsluta specialtecken.</span><span class="sxs-lookup"><span data-stu-id="1114c-169">Use quotation marks to enclose special characters.</span></span>

| <span data-ttu-id="1114c-170">Sträng</span><span class="sxs-lookup"><span data-stu-id="1114c-170">string</span></span> | <span data-ttu-id="1114c-171">är *inte* genom</span><span class="sxs-lookup"><span data-stu-id="1114c-171">is *not* found by</span></span> | <span data-ttu-id="1114c-172">men dessa hittar den</span><span class="sxs-lookup"><span data-stu-id="1114c-172">but these do find it</span></span> |
| --- | --- | --- |
| <span data-ttu-id="1114c-173">HomeController.About</span><span class="sxs-lookup"><span data-stu-id="1114c-173">HomeController.About</span></span> |<span data-ttu-id="1114c-174">hem</span><span class="sxs-lookup"><span data-stu-id="1114c-174">home</span></span><br/><span data-ttu-id="1114c-175">Domänkontrollant</span><span class="sxs-lookup"><span data-stu-id="1114c-175">controller</span></span><br/><span data-ttu-id="1114c-176">Ut</span><span class="sxs-lookup"><span data-stu-id="1114c-176">out</span></span> | <span data-ttu-id="1114c-177">homecontroller</span><span class="sxs-lookup"><span data-stu-id="1114c-177">homecontroller</span></span><br/><span data-ttu-id="1114c-178">Om</span><span class="sxs-lookup"><span data-stu-id="1114c-178">about</span></span><br/><span data-ttu-id="1114c-179">”homecontroller.about”</span><span class="sxs-lookup"><span data-stu-id="1114c-179">"homecontroller.about"</span></span>|
|<span data-ttu-id="1114c-180">USA</span><span class="sxs-lookup"><span data-stu-id="1114c-180">United States</span></span>|<span data-ttu-id="1114c-181">UNI</span><span class="sxs-lookup"><span data-stu-id="1114c-181">Uni</span></span><br/><span data-ttu-id="1114c-182">Tomas</span><span class="sxs-lookup"><span data-stu-id="1114c-182">ted</span></span>|<span data-ttu-id="1114c-183">Förenade</span><span class="sxs-lookup"><span data-stu-id="1114c-183">united</span></span><br/><span data-ttu-id="1114c-184">tillstånd</span><span class="sxs-lookup"><span data-stu-id="1114c-184">states</span></span><br/><span data-ttu-id="1114c-185">Förenade och tillstånd</span><span class="sxs-lookup"><span data-stu-id="1114c-185">united AND states</span></span><br/><span data-ttu-id="1114c-186">”USA”</span><span class="sxs-lookup"><span data-stu-id="1114c-186">"united states"</span></span>

<span data-ttu-id="1114c-187">Här följer sökuttryck som du kan använda:</span><span class="sxs-lookup"><span data-stu-id="1114c-187">Here are the search expressions you can use:</span></span>

| <span data-ttu-id="1114c-188">Exempelfråga</span><span class="sxs-lookup"><span data-stu-id="1114c-188">Sample query</span></span> | <span data-ttu-id="1114c-189">Verkan</span><span class="sxs-lookup"><span data-stu-id="1114c-189">Effect</span></span> |
| --- | --- |
| `apple` |<span data-ttu-id="1114c-190">Sök efter alla händelser i tidsintervall vars fält inkludera ordet ”apple”</span><span class="sxs-lookup"><span data-stu-id="1114c-190">Find all events in the time range whose fields include the word "apple"</span></span> |
| `apple AND banana` |<span data-ttu-id="1114c-191">Sök efter händelser som innehåller båda orden.</span><span class="sxs-lookup"><span data-stu-id="1114c-191">Find events that contain both words.</span></span> <span data-ttu-id="1114c-192">Använd kapital ”och”, inte ”och”.</span><span class="sxs-lookup"><span data-stu-id="1114c-192">Use capital "AND", not "and".</span></span> |
| `apple OR banana`<br/>`apple banana` |<span data-ttu-id="1114c-193">Sök efter händelser som innehåller antingen word.</span><span class="sxs-lookup"><span data-stu-id="1114c-193">Find events that contain either word.</span></span> <span data-ttu-id="1114c-194">Använda ”eller”, inte ”eller”.</span><span class="sxs-lookup"><span data-stu-id="1114c-194">Use "OR", not "or".</span></span><br/><span data-ttu-id="1114c-195">Kort form.</span><span class="sxs-lookup"><span data-stu-id="1114c-195">Short form.</span></span> |
| `apple NOT banana` |<span data-ttu-id="1114c-196">Sök efter händelser som innehåller ett ord, men inte på den andra.</span><span class="sxs-lookup"><span data-stu-id="1114c-196">Find events that contain one word but not the other.</span></span> |



## <a name="sampling"></a><span data-ttu-id="1114c-197">Samling</span><span class="sxs-lookup"><span data-stu-id="1114c-197">Sampling</span></span>
<span data-ttu-id="1114c-198">Om din app genererar mycket telemetri (och du använder SDK för ASP.NET version 2.0.0-beta3 eller senare), modulen anpassningsbar samplingsfrekvensen minskar automatiskt den volym som skickas till portalen genom att skicka en representativ del av händelser.</span><span class="sxs-lookup"><span data-stu-id="1114c-198">If your app generates a lot of telemetry (and you are using the ASP.NET SDK version 2.0.0-beta3 or later), the adaptive sampling module automatically reduces the volume that is sent to the portal by sending only a representative fraction of events.</span></span> <span data-ttu-id="1114c-199">Dock är händelser som är relaterade till samma begäran markeras eller avmarkeras som en grupp, så att du kan navigera mellan relaterade händelser</span><span class="sxs-lookup"><span data-stu-id="1114c-199">However, events that are related to the same request are selected or deselected as a group, so that you can navigate between related events.</span></span> 

<span data-ttu-id="1114c-200">[Lär dig mer om insamling](app-insights-sampling.md).</span><span class="sxs-lookup"><span data-stu-id="1114c-200">[Learn about sampling](app-insights-sampling.md).</span></span>



## <a name="create-work-item"></a><span data-ttu-id="1114c-201">Skapa arbetsobjekt</span><span class="sxs-lookup"><span data-stu-id="1114c-201">Create work item</span></span>
<span data-ttu-id="1114c-202">Du kan skapa ett programfel i GitHub eller Visual Studio Team Services med information från ett telemetri-objekt.</span><span class="sxs-lookup"><span data-stu-id="1114c-202">You can create a bug in GitHub or Visual Studio Team Services with the details from any telemetry item.</span></span> 

![Klicka på nytt arbetsobjekt, redigera fälten och klicka sedan på OK.](./media/app-insights-diagnostic-search/42.png)

<span data-ttu-id="1114c-204">Första gången du göra detta, uppmanas du att konfigurera en länk till ditt Team Services-konto och projekt.</span><span class="sxs-lookup"><span data-stu-id="1114c-204">The first time you do this, you are asked to configure a link to your Team Services account and project.</span></span>

![Fyll URL Team Services-server och projektets namn och klicka på auktorisera](./media/app-insights-diagnostic-search/41.png)

<span data-ttu-id="1114c-206">(Du kan också konfigurera länken på bladet arbetsobjekt.)</span><span class="sxs-lookup"><span data-stu-id="1114c-206">(You can also configure the link on the Work Items blade.)</span></span>

## <a name="save-your-search"></a><span data-ttu-id="1114c-207">Spara sökningen</span><span class="sxs-lookup"><span data-stu-id="1114c-207">Save your search</span></span>
<span data-ttu-id="1114c-208">När du har angett alla filter som du vill kan spara du sökningen som en favorit.</span><span class="sxs-lookup"><span data-stu-id="1114c-208">When you've set all the filters you want, you can save the search as a favorite.</span></span> <span data-ttu-id="1114c-209">Om du arbetar i ett organisationskonto kan välja du om du vill dela den med andra medlemmar.</span><span class="sxs-lookup"><span data-stu-id="1114c-209">If you work in an organizational account, you can choose whether to share it with other team members.</span></span>

![Klicka på favorit, ange namn och klicka på Spara](./media/app-insights-diagnostic-search/08-favorite-save.png)

<span data-ttu-id="1114c-211">Visa sökningen igen, **gå till bladet översikt** och öppna Favoriter:</span><span class="sxs-lookup"><span data-stu-id="1114c-211">To see the search again, **go to the overview blade** and open Favorites:</span></span>

![Panelen Favoriter](./media/app-insights-diagnostic-search/09-favorite-get.png)

<span data-ttu-id="1114c-213">Om du har sparat med relativa tidsintervall har den senaste informationen i bladet öppnas på nytt.</span><span class="sxs-lookup"><span data-stu-id="1114c-213">If you saved with Relative time range, the re-opened blade has the latest data.</span></span> <span data-ttu-id="1114c-214">Om du har sparat med absolut tidsintervall ser du samma data varje gång.</span><span class="sxs-lookup"><span data-stu-id="1114c-214">If you saved with Absolute time range, you see the same data every time.</span></span> <span data-ttu-id="1114c-215">(Om 'Relativa' är inte tillgänglig när du vill spara en favorit, klicka på tidsintervall i huvudet ange ett tidsintervall som inte är ett anpassat intervall)</span><span class="sxs-lookup"><span data-stu-id="1114c-215">(If 'Relative' isn't available when you want to save a favorite, click Time Range in the header, and set a time range that isn't a custom range.)</span></span>

## <a name="send-more-telemetry-to-application-insights"></a><span data-ttu-id="1114c-216">Skicka telemetri om flera till Application Insights</span><span class="sxs-lookup"><span data-stu-id="1114c-216">Send more telemetry to Application Insights</span></span>
<span data-ttu-id="1114c-217">Förutom out box telemetri som skickas av Application Insights SDK, kan du:</span><span class="sxs-lookup"><span data-stu-id="1114c-217">In addition to the out-of-the-box telemetry sent by Application Insights SDK, you can:</span></span>

* <span data-ttu-id="1114c-218">Avbilda loggspårningar från dina Favoriter loggningsramverk i [.NET](app-insights-asp-net-trace-logs.md) eller [Java](app-insights-java-trace-logs.md).</span><span class="sxs-lookup"><span data-stu-id="1114c-218">Capture log traces from your favorite logging framework in [.NET](app-insights-asp-net-trace-logs.md) or [Java](app-insights-java-trace-logs.md).</span></span> <span data-ttu-id="1114c-219">Detta innebär att du kan söka igenom din loggspårningar och korrelera dem med sidvisningar, undantag och andra händelser.</span><span class="sxs-lookup"><span data-stu-id="1114c-219">This means you can search through your log traces and correlate them with page views, exceptions, and other events.</span></span> 
* <span data-ttu-id="1114c-220">[Skriva kod](app-insights-api-custom-events-metrics.md) att skicka anpassade händelser, sidvisningar och undantag.</span><span class="sxs-lookup"><span data-stu-id="1114c-220">[Write code](app-insights-api-custom-events-metrics.md) to send custom events, page views, and exceptions.</span></span> 

<span data-ttu-id="1114c-221">[Lär dig att skicka loggar och anpassad telemetri till Application Insights](app-insights-asp-net-trace-logs.md).</span><span class="sxs-lookup"><span data-stu-id="1114c-221">[Learn how to send logs and custom telemetry to Application Insights](app-insights-asp-net-trace-logs.md).</span></span>

## <span data-ttu-id="1114c-222"><a name="questions"></a>FRÅGOR OCH SVAR</span><span class="sxs-lookup"><span data-stu-id="1114c-222"><a name="questions"></a>Q & A</span></span>
### <span data-ttu-id="1114c-223"><a name="limits"></a>Hur mycket data finns kvar?</span><span class="sxs-lookup"><span data-stu-id="1114c-223"><a name="limits"></a>How much data is retained?</span></span>

<span data-ttu-id="1114c-224">Finns det [gränser sammanfattning](app-insights-pricing.md#limits-summary).</span><span class="sxs-lookup"><span data-stu-id="1114c-224">See the [Limits summary](app-insights-pricing.md#limits-summary).</span></span>

### <a name="how-can-i-see-post-data-in-my-server-requests"></a><span data-ttu-id="1114c-225">Hur kan jag se postdata i min serverbegäranden?</span><span class="sxs-lookup"><span data-stu-id="1114c-225">How can I see POST data in my server requests?</span></span>
<span data-ttu-id="1114c-226">Vi inte logga data efter automatiskt, men du kan använda [TrackTrace eller loggfil anrop](app-insights-asp-net-trace-logs.md).</span><span class="sxs-lookup"><span data-stu-id="1114c-226">We don't log the POST data automatically, but you can use [TrackTrace or log calls](app-insights-asp-net-trace-logs.md).</span></span> <span data-ttu-id="1114c-227">Placera efter data i parametern meddelandet.</span><span class="sxs-lookup"><span data-stu-id="1114c-227">Put the POST data in the message parameter.</span></span> <span data-ttu-id="1114c-228">Det går inte att filtrera meddelandet på samma sätt som du kan filtrera efter egenskaper, men storleksgränsen är längre.</span><span class="sxs-lookup"><span data-stu-id="1114c-228">You can't filter on the message in the same way you can filter on properties, but the size limit is longer.</span></span>

## <a name="video"></a><span data-ttu-id="1114c-229">Video</span><span class="sxs-lookup"><span data-stu-id="1114c-229">Video</span></span>

> [!VIDEO https://channel9.msdn.com/events/Connect/2016/112/player]

## <span data-ttu-id="1114c-230"><a name="add"></a>Nästa steg</span><span class="sxs-lookup"><span data-stu-id="1114c-230"><a name="add"></a>Next steps</span></span>
* [<span data-ttu-id="1114c-231">Skriva komplexa frågor i Analytics</span><span class="sxs-lookup"><span data-stu-id="1114c-231">Write complex queries in Analytics</span></span>](app-insights-analytics-tour.md)
* [<span data-ttu-id="1114c-232">Skicka loggar och anpassad telemetri till Application Insights</span><span class="sxs-lookup"><span data-stu-id="1114c-232">Send logs and custom telemetry to Application Insights</span></span>](app-insights-asp-net-trace-logs.md)
* [<span data-ttu-id="1114c-233">Ställ in tillgänglighet och svarstider tester</span><span class="sxs-lookup"><span data-stu-id="1114c-233">Set up availability and responsiveness tests</span></span>](app-insights-monitor-web-app-availability.md)
* [<span data-ttu-id="1114c-234">Felsökning</span><span class="sxs-lookup"><span data-stu-id="1114c-234">Troubleshooting</span></span>](app-insights-troubleshoot-faq.md)
