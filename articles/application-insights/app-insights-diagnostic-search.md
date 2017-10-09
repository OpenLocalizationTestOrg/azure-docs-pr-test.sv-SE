---
title: "aaaUsing Sök i Azure Application Insights | Microsoft Docs"
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
ms.openlocfilehash: df2b0eb017ad48bcdc6ef57d8dff207d120143b3
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="using-search-in-application-insights"></a><span data-ttu-id="62463-103">Använda sökning i Application Insights</span><span class="sxs-lookup"><span data-stu-id="62463-103">Using Search in Application Insights</span></span>
<span data-ttu-id="62463-104">Sökningen är en funktion i [Programinsikter](app-insights-overview.md) som du använder toofind och utforska enskilda telemetri objekt, till exempel sidvyer undantag, eller webbförfrågningar.</span><span class="sxs-lookup"><span data-stu-id="62463-104">Search is a feature of [Application Insights](app-insights-overview.md) that you use toofind and explore individual telemetry items, such as page views, exceptions, or web requests.</span></span> <span data-ttu-id="62463-105">Och du kan visa loggspårningar och händelser som du har kodats.</span><span class="sxs-lookup"><span data-stu-id="62463-105">And you can view log traces and events that you have coded.</span></span>

<span data-ttu-id="62463-106">(Mer komplexa frågor över dina data, Använd [Analytics](app-insights-analytics-tour.md).)</span><span class="sxs-lookup"><span data-stu-id="62463-106">(For more complex queries over your data, use [Analytics](app-insights-analytics-tour.md).)</span></span>

## <a name="where-do-you-see-search"></a><span data-ttu-id="62463-107">Där kan du se sökningen?</span><span class="sxs-lookup"><span data-stu-id="62463-107">Where do you see Search?</span></span>
### <a name="in-hello-azure-portal"></a><span data-ttu-id="62463-108">I hello Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="62463-108">In hello Azure portal</span></span>
<span data-ttu-id="62463-109">Du kan öppna diagnostiska sökning uttryckligen från hello insikter Programöversikt bladet för ditt program:</span><span class="sxs-lookup"><span data-stu-id="62463-109">You can open diagnostic search explicitly from hello Application Insights Overview blade of your application:</span></span>

![Öppna diagnostiska sökning](./media/app-insights-diagnostic-search/01-open-Diagnostic.png)

<span data-ttu-id="62463-111">Det kan öppnas när du klickar på via några diagram och rutnät artiklar.</span><span class="sxs-lookup"><span data-stu-id="62463-111">It also opens when you click through some charts and grid items.</span></span> <span data-ttu-id="62463-112">Dess filter konfigureras i det här fallet före toofocus för hello typ av objekt du valt.</span><span class="sxs-lookup"><span data-stu-id="62463-112">In this case, its filters are pre-set toofocus on hello type of item you selected.</span></span> 

<span data-ttu-id="62463-113">Till exempel på hello översikt bladet finns ett stapeldiagram begäranden som klassificerats av svarstid.</span><span class="sxs-lookup"><span data-stu-id="62463-113">For example, on hello Overview blade, there's a bar chart of requests classified by response time.</span></span> <span data-ttu-id="62463-114">Klickar du på via en prestanda intervallet toosee en lista med enskilda begäranden i den svarstid intervall:</span><span class="sxs-lookup"><span data-stu-id="62463-114">Click through a performance range toosee a list of individual requests in that response time range:</span></span>

![Klicka dig igenom begäran prestanda](./media/app-insights-diagnostic-search/07-open-from-filters.png)

<span data-ttu-id="62463-116">hello huvuddelen av diagnostiska Search är en lista över telemetri objekt - serverbegäranden, sidan vyer, anpassade händelser som du har ett kodat och så vidare.</span><span class="sxs-lookup"><span data-stu-id="62463-116">hello main body of Diagnostic Search is a list of telemetry items - server requests, page views, custom events that you have coded, and so on.</span></span> <span data-ttu-id="62463-117">Hello är överst i listan hello en sammanfattande diagram som visar antalet händelser över tid.</span><span class="sxs-lookup"><span data-stu-id="62463-117">At hello top of hello list is a summary chart showing counts of events over time.</span></span>

<span data-ttu-id="62463-118">Klicka på Uppdatera tooget nya händelser.</span><span class="sxs-lookup"><span data-stu-id="62463-118">Click Refresh tooget new events.</span></span>

### <a name="in-visual-studio"></a><span data-ttu-id="62463-119">I Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="62463-119">In Visual Studio</span></span>

<span data-ttu-id="62463-120">I Visual Studio finns också en Application Insights sökfönstret.</span><span class="sxs-lookup"><span data-stu-id="62463-120">In Visual Studio, there's also an Application Insights Search window.</span></span> <span data-ttu-id="62463-121">Det är mest användbara för att visa telemetriska händelser som genererats av hello-program som du felsöker.</span><span class="sxs-lookup"><span data-stu-id="62463-121">It's most useful for displaying telemetry events generated by hello application that you're debugging.</span></span> <span data-ttu-id="62463-122">Men det kan också visa hello händelser som samlas in från din publicerade app på hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="62463-122">But it can also show hello events collected from your published app at hello Azure portal.</span></span>

<span data-ttu-id="62463-123">Öppna fönstret för hello Sök i Visual Studio:</span><span class="sxs-lookup"><span data-stu-id="62463-123">Open hello Search window in Visual Studio:</span></span>

![Visual Studio öppna Application Insights-sökning](./media/app-insights-diagnostic-search/32.png)

<span data-ttu-id="62463-125">hello sökfönstret har funktioner liknande toohello web-portalen:</span><span class="sxs-lookup"><span data-stu-id="62463-125">hello Search window has features similar toohello web portal:</span></span>

![Visual Studio Application Insights sökfönstret](./media/app-insights-diagnostic-search/34.png)

<span data-ttu-id="62463-127">hello spåra åtgärden fliken är tillgänglig när du öppnar en begäran eller vyn sida.</span><span class="sxs-lookup"><span data-stu-id="62463-127">hello Track Operation tab is available when you open a request or a page view.</span></span> <span data-ttu-id="62463-128">Ett ' åtgärden ' är en sekvens av händelser som är associerad med tooa begäran eller sidan vy.</span><span class="sxs-lookup"><span data-stu-id="62463-128">An 'operation' is a sequence of events that is associated with tooa single request or page view.</span></span> <span data-ttu-id="62463-129">Till exempel kan beroendeanrop, undantag, loggar och anpassade händelser ingå i en enda åtgärd.</span><span class="sxs-lookup"><span data-stu-id="62463-129">For example, dependency calls, exceptions, trace logs, and custom events might be part of a single operation.</span></span> <span data-ttu-id="62463-130">hello spåra åtgärden fliken visar hello grafiskt tidsinställning och varaktighet för dessa händelser i en relation toohello begäran eller sidan.</span><span class="sxs-lookup"><span data-stu-id="62463-130">hello Track Operation tab shows graphically hello timing and duration of these events in relation toohello request or page view.</span></span> 

## <a name="inspect-individual-items"></a><span data-ttu-id="62463-131">Inspektera enskilda objekt</span><span class="sxs-lookup"><span data-stu-id="62463-131">Inspect individual items</span></span>
<span data-ttu-id="62463-132">Markera alla objekt telemetri toosee nyckelfält och relaterade objekt.</span><span class="sxs-lookup"><span data-stu-id="62463-132">Select any telemetry item toosee key fields and related items.</span></span> <span data-ttu-id="62463-133">Klicka på ”...” Om du vill toosee hello fullständig uppsättning fält.</span><span class="sxs-lookup"><span data-stu-id="62463-133">If you want toosee hello full set of fields, click "...".</span></span> 

![Klicka på nytt arbetsobjekt och redigera hello fält.](./media/app-insights-diagnostic-search/10-detail.png)

## <a name="filter-event-types"></a><span data-ttu-id="62463-135">Filtrera händelsetyper</span><span class="sxs-lookup"><span data-stu-id="62463-135">Filter event types</span></span>
<span data-ttu-id="62463-136">Öppna hello filterbladet och välj hello händelse skriver du vill toosee.</span><span class="sxs-lookup"><span data-stu-id="62463-136">Open hello Filter blade and choose hello event types you want toosee.</span></span> <span data-ttu-id="62463-137">(Om du vill senare toorestore hello filter som du har öppnat hello-bladet, klickar du på Återställ.)</span><span class="sxs-lookup"><span data-stu-id="62463-137">(If, later, you want toorestore hello filters with which you opened hello blade, click Reset.)</span></span>

![Välj Filter och välj telemetri typer](./media/app-insights-diagnostic-search/02-filter-req.png)

<span data-ttu-id="62463-139">hello händelsetyper är:</span><span class="sxs-lookup"><span data-stu-id="62463-139">hello event types are:</span></span>

* <span data-ttu-id="62463-140">**Spåra** - [diagnostikloggar](app-insights-asp-net-trace-logs.md) inklusive TrackTrace, log4Net, NLog och System.Diagnostic.Trace anrop.</span><span class="sxs-lookup"><span data-stu-id="62463-140">**Trace** - [Diagnostic logs](app-insights-asp-net-trace-logs.md) including TrackTrace, log4Net, NLog, and System.Diagnostic.Trace calls.</span></span>
* <span data-ttu-id="62463-141">**Begära** -HTTP-begäranden tas emot av serverprogrammet, inklusive sidor, skript, bilder, filer och data.</span><span class="sxs-lookup"><span data-stu-id="62463-141">**Request** - HTTP requests received by your server application, including pages, scripts, images, style files, and data.</span></span> <span data-ttu-id="62463-142">Dessa händelser finns används toocreate hello förfrågan och svar översikt över diagram.</span><span class="sxs-lookup"><span data-stu-id="62463-142">These events are used toocreate hello request and response overview charts.</span></span>
* <span data-ttu-id="62463-143">**Vyn sida** - [telemetri som skickas av hello webbklient](app-insights-javascript.md), används toocreate sidan Visa rapporter.</span><span class="sxs-lookup"><span data-stu-id="62463-143">**Page View** - [Telemetry sent by hello web client](app-insights-javascript.md), used toocreate page view reports.</span></span> 
* <span data-ttu-id="62463-144">**Anpassad händelse** - om du har infogat anrop tooTrackEvent() i ordning för[övervaka användningen](app-insights-api-custom-events-metrics.md), kan du söka dem här.</span><span class="sxs-lookup"><span data-stu-id="62463-144">**Custom Event** - If you inserted calls tooTrackEvent() in order too[monitor usage](app-insights-api-custom-events-metrics.md), you can search them here.</span></span>
* <span data-ttu-id="62463-145">**Undantag** – utan felhantering [undantag i hello server](app-insights-asp-net-exceptions.md), och de som du loggar in med hjälp av TrackException().</span><span class="sxs-lookup"><span data-stu-id="62463-145">**Exception** - Uncaught [exceptions in hello server](app-insights-asp-net-exceptions.md), and those that you log by using TrackException().</span></span>
* <span data-ttu-id="62463-146">**Beroende** - [anrop från serverprogrammet](app-insights-asp-net-dependencies.md) tooother tjänster, till exempel REST API: er eller databaser och AJAX-anrop från din [klientkod](app-insights-javascript.md).</span><span class="sxs-lookup"><span data-stu-id="62463-146">**Dependency** - [Calls from your server application](app-insights-asp-net-dependencies.md) tooother services such as REST APIs or databases, and AJAX calls from your [client code](app-insights-javascript.md).</span></span>
* <span data-ttu-id="62463-147">**Tillgänglighet** -resultaten av [tillgänglighetstester](app-insights-monitor-web-app-availability.md).</span><span class="sxs-lookup"><span data-stu-id="62463-147">**Availability** - Results of [availability tests](app-insights-monitor-web-app-availability.md).</span></span>

## <a name="filter-on-property-values"></a><span data-ttu-id="62463-148">Filtrera på egenskapsvärden</span><span class="sxs-lookup"><span data-stu-id="62463-148">Filter on property values</span></span>
<span data-ttu-id="62463-149">Du kan filtrera händelser på hello värden för deras egenskaper.</span><span class="sxs-lookup"><span data-stu-id="62463-149">You can filter events on hello values of their properties.</span></span> <span data-ttu-id="62463-150">hello tillgängliga egenskaper är beroende av hello händelsetyper som du har valt.</span><span class="sxs-lookup"><span data-stu-id="62463-150">hello available properties depend on hello event types you selected.</span></span> 

<span data-ttu-id="62463-151">Till exempel välja ut begäranden med en specifik svarskod.</span><span class="sxs-lookup"><span data-stu-id="62463-151">For example, pick out requests with a specific response code.</span></span> 

![Expandera en egenskap och väljer ett värde](./media/app-insights-diagnostic-search/03-response500.png)

<span data-ttu-id="62463-153">Om du väljer några värden för en viss egenskap har hello samma effekt som att välja alla värden.</span><span class="sxs-lookup"><span data-stu-id="62463-153">Choosing no values of a particular property has hello same effect as choosing all values.</span></span> <span data-ttu-id="62463-154">Den växlar av filtreringen för den egenskapen.</span><span class="sxs-lookup"><span data-stu-id="62463-154">It switches off filtering on that property.</span></span>

### <a name="narrow-your-search"></a><span data-ttu-id="62463-155">Begränsa sökningen</span><span class="sxs-lookup"><span data-stu-id="62463-155">Narrow your search</span></span>
<span data-ttu-id="62463-156">Observera att hello räknar toohello höger i hello filtervärden visar hur många förekomster det i hello aktuella filtreringen.</span><span class="sxs-lookup"><span data-stu-id="62463-156">Notice that hello counts toohello right of hello filter values show how many occurrences there are in hello current filtered set.</span></span> 

<span data-ttu-id="62463-157">Det är tydligt hello rpthuvud/medarbetare begäran resulterar i de flesta hello ”500” fel i det här exemplet:</span><span class="sxs-lookup"><span data-stu-id="62463-157">In this example, it's clear that hello 'Rpt/Employees' request results in most of hello '500' errors:</span></span>

![Expandera en egenskap och väljer ett värde](./media/app-insights-diagnostic-search/04-failingReq.png)




## <a name="find-events-with-hello-same-property"></a><span data-ttu-id="62463-159">Sök efter händelser med hello samma egenskap</span><span class="sxs-lookup"><span data-stu-id="62463-159">Find events with hello same property</span></span>
<span data-ttu-id="62463-160">Hitta alla hello objekt hello samma värde för egenskap:</span><span class="sxs-lookup"><span data-stu-id="62463-160">Find all hello items with hello same property value:</span></span>

![Högerklicka på en egenskap](./media/app-insights-diagnostic-search/12-samevalue.png)


## <a name="search-hello-data"></a><span data-ttu-id="62463-162">Sök hello data</span><span class="sxs-lookup"><span data-stu-id="62463-162">Search hello data</span></span>

> [!NOTE]
> <span data-ttu-id="62463-163">toowrite mer komplexa frågor, öppna [ **Analytics** ](app-insights-analytics-tour.md) från hello överkant hello Sök bladet.</span><span class="sxs-lookup"><span data-stu-id="62463-163">toowrite more complex queries, open [**Analytics**](app-insights-analytics-tour.md) from hello top of hello Search blade.</span></span>
> 

<span data-ttu-id="62463-164">Du kan söka efter villkoren i någon av hello egenskapsvärden.</span><span class="sxs-lookup"><span data-stu-id="62463-164">You can search for terms in any of hello property values.</span></span> <span data-ttu-id="62463-165">Detta är särskilt användbart om du har skrivit [anpassade händelser](app-insights-api-custom-events-metrics.md) med egenskapsvärden.</span><span class="sxs-lookup"><span data-stu-id="62463-165">This is particularly useful if you have written [custom events](app-insights-api-custom-events-metrics.md) with property values.</span></span> 

<span data-ttu-id="62463-166">Du kanske vill tooset ett tidsintervall som söker över ett kortare intervall är snabbare.</span><span class="sxs-lookup"><span data-stu-id="62463-166">You might want tooset a time range, as searches over a shorter range are faster.</span></span> 

![Öppna diagnostiska sökning](./media/app-insights-diagnostic-search/appinsights-311search.png)

<span data-ttu-id="62463-168">Sök efter fullständiga ord, inte delsträngar.</span><span class="sxs-lookup"><span data-stu-id="62463-168">Search for complete words, not substrings.</span></span> <span data-ttu-id="62463-169">Använd citattecken tooenclose specialtecken.</span><span class="sxs-lookup"><span data-stu-id="62463-169">Use quotation marks tooenclose special characters.</span></span>

| <span data-ttu-id="62463-170">Sträng</span><span class="sxs-lookup"><span data-stu-id="62463-170">string</span></span> | <span data-ttu-id="62463-171">är *inte* genom</span><span class="sxs-lookup"><span data-stu-id="62463-171">is *not* found by</span></span> | <span data-ttu-id="62463-172">men dessa hittar den</span><span class="sxs-lookup"><span data-stu-id="62463-172">but these do find it</span></span> |
| --- | --- | --- |
| <span data-ttu-id="62463-173">HomeController.About</span><span class="sxs-lookup"><span data-stu-id="62463-173">HomeController.About</span></span> |<span data-ttu-id="62463-174">hem</span><span class="sxs-lookup"><span data-stu-id="62463-174">home</span></span><br/><span data-ttu-id="62463-175">Domänkontrollant</span><span class="sxs-lookup"><span data-stu-id="62463-175">controller</span></span><br/><span data-ttu-id="62463-176">Ut</span><span class="sxs-lookup"><span data-stu-id="62463-176">out</span></span> | <span data-ttu-id="62463-177">homecontroller</span><span class="sxs-lookup"><span data-stu-id="62463-177">homecontroller</span></span><br/><span data-ttu-id="62463-178">Om</span><span class="sxs-lookup"><span data-stu-id="62463-178">about</span></span><br/><span data-ttu-id="62463-179">”homecontroller.about”</span><span class="sxs-lookup"><span data-stu-id="62463-179">"homecontroller.about"</span></span>|
|<span data-ttu-id="62463-180">USA</span><span class="sxs-lookup"><span data-stu-id="62463-180">United States</span></span>|<span data-ttu-id="62463-181">UNI</span><span class="sxs-lookup"><span data-stu-id="62463-181">Uni</span></span><br/><span data-ttu-id="62463-182">Tomas</span><span class="sxs-lookup"><span data-stu-id="62463-182">ted</span></span>|<span data-ttu-id="62463-183">Förenade</span><span class="sxs-lookup"><span data-stu-id="62463-183">united</span></span><br/><span data-ttu-id="62463-184">tillstånd</span><span class="sxs-lookup"><span data-stu-id="62463-184">states</span></span><br/><span data-ttu-id="62463-185">Förenade och tillstånd</span><span class="sxs-lookup"><span data-stu-id="62463-185">united AND states</span></span><br/><span data-ttu-id="62463-186">”USA”</span><span class="sxs-lookup"><span data-stu-id="62463-186">"united states"</span></span>

<span data-ttu-id="62463-187">Här följer hello sökuttryck som du kan använda:</span><span class="sxs-lookup"><span data-stu-id="62463-187">Here are hello search expressions you can use:</span></span>

| <span data-ttu-id="62463-188">Exempelfråga</span><span class="sxs-lookup"><span data-stu-id="62463-188">Sample query</span></span> | <span data-ttu-id="62463-189">Verkan</span><span class="sxs-lookup"><span data-stu-id="62463-189">Effect</span></span> |
| --- | --- |
| `apple` |<span data-ttu-id="62463-190">Sök efter alla händelser i hello tidsintervall vars fält innehåller hello ordet ”apple”</span><span class="sxs-lookup"><span data-stu-id="62463-190">Find all events in hello time range whose fields include hello word "apple"</span></span> |
| `apple AND banana` |<span data-ttu-id="62463-191">Sök efter händelser som innehåller båda orden.</span><span class="sxs-lookup"><span data-stu-id="62463-191">Find events that contain both words.</span></span> <span data-ttu-id="62463-192">Använd kapital ”och”, inte ”och”.</span><span class="sxs-lookup"><span data-stu-id="62463-192">Use capital "AND", not "and".</span></span> |
| `apple OR banana`<br/>`apple banana` |<span data-ttu-id="62463-193">Sök efter händelser som innehåller antingen word.</span><span class="sxs-lookup"><span data-stu-id="62463-193">Find events that contain either word.</span></span> <span data-ttu-id="62463-194">Använda ”eller”, inte ”eller”.</span><span class="sxs-lookup"><span data-stu-id="62463-194">Use "OR", not "or".</span></span><br/><span data-ttu-id="62463-195">Kort form.</span><span class="sxs-lookup"><span data-stu-id="62463-195">Short form.</span></span> |
| `apple NOT banana` |<span data-ttu-id="62463-196">Sök efter händelser som innehåller ett ord men inte hello andra.</span><span class="sxs-lookup"><span data-stu-id="62463-196">Find events that contain one word but not hello other.</span></span> |



## <a name="sampling"></a><span data-ttu-id="62463-197">Samling</span><span class="sxs-lookup"><span data-stu-id="62463-197">Sampling</span></span>
<span data-ttu-id="62463-198">Om din app genererar mycket telemetri (och du använder hello SDK för ASP.NET version 2.0.0-beta3 eller senare), hello anpassningsbar provtagning modulen automatiskt minskar hello volymen som skickas toohello portal genom att skicka en representativ del av händelser.</span><span class="sxs-lookup"><span data-stu-id="62463-198">If your app generates a lot of telemetry (and you are using hello ASP.NET SDK version 2.0.0-beta3 or later), hello adaptive sampling module automatically reduces hello volume that is sent toohello portal by sending only a representative fraction of events.</span></span> <span data-ttu-id="62463-199">Dock händelser som är relaterade toohello samma begäran markeras eller avmarkeras som en grupp, så att du kan navigera mellan relaterade händelser.</span><span class="sxs-lookup"><span data-stu-id="62463-199">However, events that are related toohello same request are selected or deselected as a group, so that you can navigate between related events.</span></span> 

<span data-ttu-id="62463-200">[Lär dig mer om insamling](app-insights-sampling.md).</span><span class="sxs-lookup"><span data-stu-id="62463-200">[Learn about sampling](app-insights-sampling.md).</span></span>



## <a name="create-work-item"></a><span data-ttu-id="62463-201">Skapa arbetsobjekt</span><span class="sxs-lookup"><span data-stu-id="62463-201">Create work item</span></span>
<span data-ttu-id="62463-202">Du kan skapa ett programfel i GitHub eller Visual Studio Team Services med hello information från ett telemetri-objekt.</span><span class="sxs-lookup"><span data-stu-id="62463-202">You can create a bug in GitHub or Visual Studio Team Services with hello details from any telemetry item.</span></span> 

![Klicka på nytt arbetsobjekt och redigera hello fält.](./media/app-insights-diagnostic-search/42.png)

<span data-ttu-id="62463-204">hello första gången som du gör det uppmanas du tooconfigure en länk tooyour Team Services-konto och projektet.</span><span class="sxs-lookup"><span data-stu-id="62463-204">hello first time you do this, you are asked tooconfigure a link tooyour Team Services account and project.</span></span>

![Fyll hello URL på Team Services-servern och hello projektnamn och klicka på auktorisera](./media/app-insights-diagnostic-search/41.png)

<span data-ttu-id="62463-206">(Du kan också konfigurera hello länk på hello arbetsobjekt blad.)</span><span class="sxs-lookup"><span data-stu-id="62463-206">(You can also configure hello link on hello Work Items blade.)</span></span>

## <a name="save-your-search"></a><span data-ttu-id="62463-207">Spara sökningen</span><span class="sxs-lookup"><span data-stu-id="62463-207">Save your search</span></span>
<span data-ttu-id="62463-208">När du har angett alla hello filter som du vill kan spara du hello sökning som en favorit.</span><span class="sxs-lookup"><span data-stu-id="62463-208">When you've set all hello filters you want, you can save hello search as a favorite.</span></span> <span data-ttu-id="62463-209">Om du arbetar i ett organisationskonto kan du välja om tooshare den med andra medlemmar.</span><span class="sxs-lookup"><span data-stu-id="62463-209">If you work in an organizational account, you can choose whether tooshare it with other team members.</span></span>

![Klicka på favoriten hello namn och klicka på Spara](./media/app-insights-diagnostic-search/08-favorite-save.png)

<span data-ttu-id="62463-211">toosee hello Sök igen, **gå toohello översikt bladet** och öppna Favoriter:</span><span class="sxs-lookup"><span data-stu-id="62463-211">toosee hello search again, **go toohello overview blade** and open Favorites:</span></span>

![Panelen Favoriter](./media/app-insights-diagnostic-search/09-favorite-get.png)

<span data-ttu-id="62463-213">Om du har sparat med relativa tidsintervall har hello öppnas på nytt bladet hello senaste data.</span><span class="sxs-lookup"><span data-stu-id="62463-213">If you saved with Relative time range, hello re-opened blade has hello latest data.</span></span> <span data-ttu-id="62463-214">Om du har sparat med absolut tidsintervall du ser hello samma data varje gång.</span><span class="sxs-lookup"><span data-stu-id="62463-214">If you saved with Absolute time range, you see hello same data every time.</span></span> <span data-ttu-id="62463-215">(Om 'Relativa' är inte tillgänglig när du vill toosave favorit, klicka på tidsintervall i hello-huvudet ange ett tidsintervall som inte är ett anpassat intervall)</span><span class="sxs-lookup"><span data-stu-id="62463-215">(If 'Relative' isn't available when you want toosave a favorite, click Time Range in hello header, and set a time range that isn't a custom range.)</span></span>

## <a name="send-more-telemetry-tooapplication-insights"></a><span data-ttu-id="62463-216">Skicka telemetri om flera tooApplication insikter</span><span class="sxs-lookup"><span data-stu-id="62463-216">Send more telemetry tooApplication Insights</span></span>
<span data-ttu-id="62463-217">I tillägg toohello out box telemetri skickas av Application Insights SDK kan du:</span><span class="sxs-lookup"><span data-stu-id="62463-217">In addition toohello out-of-the-box telemetry sent by Application Insights SDK, you can:</span></span>

* <span data-ttu-id="62463-218">Avbilda loggspårningar från dina Favoriter loggningsramverk i [.NET](app-insights-asp-net-trace-logs.md) eller [Java](app-insights-java-trace-logs.md).</span><span class="sxs-lookup"><span data-stu-id="62463-218">Capture log traces from your favorite logging framework in [.NET](app-insights-asp-net-trace-logs.md) or [Java](app-insights-java-trace-logs.md).</span></span> <span data-ttu-id="62463-219">Detta innebär att du kan söka igenom din loggspårningar och korrelera dem med sidvisningar, undantag och andra händelser.</span><span class="sxs-lookup"><span data-stu-id="62463-219">This means you can search through your log traces and correlate them with page views, exceptions, and other events.</span></span> 
* <span data-ttu-id="62463-220">[Skriva kod](app-insights-api-custom-events-metrics.md) toosend anpassade händelser, sidvisningar och undantag.</span><span class="sxs-lookup"><span data-stu-id="62463-220">[Write code](app-insights-api-custom-events-metrics.md) toosend custom events, page views, and exceptions.</span></span> 

<span data-ttu-id="62463-221">[Lär dig hur toosend loggar och anpassad telemetri tooApplication insikter](app-insights-asp-net-trace-logs.md).</span><span class="sxs-lookup"><span data-stu-id="62463-221">[Learn how toosend logs and custom telemetry tooApplication Insights](app-insights-asp-net-trace-logs.md).</span></span>

## <span data-ttu-id="62463-222"><a name="questions"></a>FRÅGOR OCH SVAR</span><span class="sxs-lookup"><span data-stu-id="62463-222"><a name="questions"></a>Q & A</span></span>
### <span data-ttu-id="62463-223"><a name="limits"></a>Hur mycket data finns kvar?</span><span class="sxs-lookup"><span data-stu-id="62463-223"><a name="limits"></a>How much data is retained?</span></span>

<span data-ttu-id="62463-224">Se hello [gränser sammanfattning](app-insights-pricing.md#limits-summary).</span><span class="sxs-lookup"><span data-stu-id="62463-224">See hello [Limits summary](app-insights-pricing.md#limits-summary).</span></span>

### <a name="how-can-i-see-post-data-in-my-server-requests"></a><span data-ttu-id="62463-225">Hur kan jag se postdata i min serverbegäranden?</span><span class="sxs-lookup"><span data-stu-id="62463-225">How can I see POST data in my server requests?</span></span>
<span data-ttu-id="62463-226">Vi inte logga hello postdata automatiskt, men du kan använda [TrackTrace eller loggfil anrop](app-insights-asp-net-trace-logs.md).</span><span class="sxs-lookup"><span data-stu-id="62463-226">We don't log hello POST data automatically, but you can use [TrackTrace or log calls](app-insights-asp-net-trace-logs.md).</span></span> <span data-ttu-id="62463-227">Placera hello postdata i hello message-parameter.</span><span class="sxs-lookup"><span data-stu-id="62463-227">Put hello POST data in hello message parameter.</span></span> <span data-ttu-id="62463-228">Du kan filtrera efter hello-meddelande i hello samma sätt kan du filtrera efter egenskaper, men hello storleksgränsen är längre.</span><span class="sxs-lookup"><span data-stu-id="62463-228">You can't filter on hello message in hello same way you can filter on properties, but hello size limit is longer.</span></span>

## <a name="video"></a><span data-ttu-id="62463-229">Video</span><span class="sxs-lookup"><span data-stu-id="62463-229">Video</span></span>

> [!VIDEO https://channel9.msdn.com/events/Connect/2016/112/player]

## <span data-ttu-id="62463-230"><a name="add"></a>Nästa steg</span><span class="sxs-lookup"><span data-stu-id="62463-230"><a name="add"></a>Next steps</span></span>
* [<span data-ttu-id="62463-231">Skriva komplexa frågor i Analytics</span><span class="sxs-lookup"><span data-stu-id="62463-231">Write complex queries in Analytics</span></span>](app-insights-analytics-tour.md)
* [<span data-ttu-id="62463-232">Skicka loggar och anpassad telemetri tooApplication insikter</span><span class="sxs-lookup"><span data-stu-id="62463-232">Send logs and custom telemetry tooApplication Insights</span></span>](app-insights-asp-net-trace-logs.md)
* [<span data-ttu-id="62463-233">Ställ in tillgänglighet och svarstider tester</span><span class="sxs-lookup"><span data-stu-id="62463-233">Set up availability and responsiveness tests</span></span>](app-insights-monitor-web-app-availability.md)
* [<span data-ttu-id="62463-234">Felsökning</span><span class="sxs-lookup"><span data-stu-id="62463-234">Troubleshooting</span></span>](app-insights-troubleshoot-faq.md)
