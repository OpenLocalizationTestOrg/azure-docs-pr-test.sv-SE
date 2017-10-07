---
title: "aaaUser, session och händelsen analys i Azure Application Insights | Microsoft docs"
description: "Demografisk analys av användare av ditt webbprogram."
services: application-insights
documentationcenter: 
author: botatoes
manager: carmonm
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: multiple
ms.topic: article
ms.date: 05/03/2017
ms.author: bwren
ms.openlocfilehash: 152ab90e9a25c03087d3ebbde1263ec72acb227e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="users-sessions-and-events-analysis-in-application-insights"></a><span data-ttu-id="0b521-103">Användare, sessioner och händelser för analys i Application Insights</span><span class="sxs-lookup"><span data-stu-id="0b521-103">Users, sessions, and events analysis in Application Insights</span></span>

<span data-ttu-id="0b521-104">Ta reda om personer använder ditt webbprogram, vilka sidor som de är mest intresserad av, där användarna finns, vilka webbläsare och operativsystem som de använder.</span><span class="sxs-lookup"><span data-stu-id="0b521-104">Find out when people use your web app, what pages they're most interested in, where your users are located, what browsers and operating systems they use.</span></span> <span data-ttu-id="0b521-105">Analysera affärs- och användningsdata telemetri med hjälp av [Azure Application Insights](app-insights-overview.md).</span><span class="sxs-lookup"><span data-stu-id="0b521-105">Analyze business and usage telemetry by using [Azure Application Insights](app-insights-overview.md).</span></span>

## <a name="get-started"></a><span data-ttu-id="0b521-106">Kom igång</span><span class="sxs-lookup"><span data-stu-id="0b521-106">Get started</span></span>

<span data-ttu-id="0b521-107">Om du ännu inte ser data i hello användare, sessioner och händelser blad i hello Application Insights-portalen [Lär dig hur tooget igång med verktyg för användning av hello](app-insights-usage-overview.md).</span><span class="sxs-lookup"><span data-stu-id="0b521-107">If you don't yet see data in hello users, sessions, or events blades in hello Application Insights portal, [learn how tooget started with hello usage tools](app-insights-usage-overview.md).</span></span>

## <a name="hello-users-sessions-and-events-segmentation-tool"></a><span data-ttu-id="0b521-108">hello användare, sessioner och händelser segmentering-verktyget</span><span class="sxs-lookup"><span data-stu-id="0b521-108">hello Users, Sessions, and Events segmentation tool</span></span>

<span data-ttu-id="0b521-109">Tre hello användning blad använder hello samma verktyget-tooslice och rapportanvändarna telemetri från ditt program från tre perspektiv.</span><span class="sxs-lookup"><span data-stu-id="0b521-109">Three of hello usage blades use hello same tool tooslice and dice telemetry from your web app from three perspectives.</span></span> <span data-ttu-id="0b521-110">Genom filtrering och dela hello data kan få du insikter om hello relativa användning av olika sidor och funktioner.</span><span class="sxs-lookup"><span data-stu-id="0b521-110">By filtering and splitting hello data, you can uncover insights about hello relative usage of different pages and features.</span></span>

* <span data-ttu-id="0b521-111">**Användare verktyget**: hur många som används för appen och dess funktioner.</span><span class="sxs-lookup"><span data-stu-id="0b521-111">**Users tool**: How many people used your app and its features.</span></span>  <span data-ttu-id="0b521-112">Användare ska räknas med anonyma ID: N som lagras i cookies i webbläsaren.</span><span class="sxs-lookup"><span data-stu-id="0b521-112">Users are counted by using anonymous IDs stored in browser cookies.</span></span> <span data-ttu-id="0b521-113">En enda person som använder olika webbläsare eller datorer som ska räknas som flera användare.</span><span class="sxs-lookup"><span data-stu-id="0b521-113">A single person using different browsers or machines will be counted as more than one user.</span></span>
* <span data-ttu-id="0b521-114">**Sessioner verktyget**: hur många sessioner för användaraktiviteten finns det vissa sidor och funktioner i din app.</span><span class="sxs-lookup"><span data-stu-id="0b521-114">**Sessions tool**: How many sessions of user activity have included certain pages and features of your app.</span></span> <span data-ttu-id="0b521-115">En session räknas efter en halvtimme användaren inaktivitet, eller efter kontinuerlig 24 timmar för användning.</span><span class="sxs-lookup"><span data-stu-id="0b521-115">A session is counted after half an hour of user inactivity, or after continuous 24h of use.</span></span>
* <span data-ttu-id="0b521-116">**Händelser verktyget**: hur ofta vissa sidor och funktioner i din app används.</span><span class="sxs-lookup"><span data-stu-id="0b521-116">**Events tool**: How often certain pages and features of your app are used.</span></span> <span data-ttu-id="0b521-117">Vyn sida räknas när en webbläsare läser en sida från din app, förutsatt att du har [instrumenterats den](app-insights-javascript.md).</span><span class="sxs-lookup"><span data-stu-id="0b521-117">A page view is counted when a browser loads a page from your app, provided you have [instrumented it](app-insights-javascript.md).</span></span> 

    <span data-ttu-id="0b521-118">En anpassad händelse representerar en förekomst av något händer i din app, ofta en användarinteraktion som en knapp Klicka eller hello slutförandet av en uppgift.</span><span class="sxs-lookup"><span data-stu-id="0b521-118">A custom event represents one occurrence of something happening in your app, often a user interaction like a button click or hello completion of some task.</span></span> <span data-ttu-id="0b521-119">Du infoga kod i din app för[generera anpassade händelser](app-insights-api-custom-events-metrics.md#trackevent).</span><span class="sxs-lookup"><span data-stu-id="0b521-119">You insert code in your app too[generate custom events](app-insights-api-custom-events-metrics.md#trackevent).</span></span>

![Användning-verktyget](./media/app-insights-usage-segmentation/users.png)

## <a name="querying-for-certain-users"></a><span data-ttu-id="0b521-121">Frågor för vissa användare</span><span class="sxs-lookup"><span data-stu-id="0b521-121">Querying for Certain Users</span></span> 

<span data-ttu-id="0b521-122">Utforska olika grupper av användare genom att justera hello frågealternativ hello överst i hello användare verktyget:</span><span class="sxs-lookup"><span data-stu-id="0b521-122">Explore different groups of users by adjusting hello query options at hello top of hello Users tool:</span></span> 

* <span data-ttu-id="0b521-123">Som används: Välj anpassade händelser och sidvisningar.</span><span class="sxs-lookup"><span data-stu-id="0b521-123">Who used: Choose custom events and page views.</span></span> 
* <span data-ttu-id="0b521-124">Under: Välja ett tidsintervall.</span><span class="sxs-lookup"><span data-stu-id="0b521-124">During: Choose a time range.</span></span> 
* <span data-ttu-id="0b521-125">Av: Välj hur toobucket hello data, antingen genom en viss tidsperiod eller en annan egenskap, till exempel webbläsare eller ort.</span><span class="sxs-lookup"><span data-stu-id="0b521-125">By: Choose how toobucket hello data, either by a period of time or by another property such as browser or city.</span></span> 
* <span data-ttu-id="0b521-126">Delar av: Välj en egenskap av vilken toosplit eller segment hello-data.</span><span class="sxs-lookup"><span data-stu-id="0b521-126">Split By: Choose a property by which toosplit or segment hello data.</span></span> 
* <span data-ttu-id="0b521-127">Lägg till filter: Begränsa hello frågan toocertain användare, sessioner eller händelser utifrån deras egenskaper, till exempel webbläsare eller ort.</span><span class="sxs-lookup"><span data-stu-id="0b521-127">Add Filters: Limit hello query toocertain users, sessions, or events based on their properties, such as browser or city.</span></span> 
 
## <a name="saving-and-sharing-reports"></a><span data-ttu-id="0b521-128">Spara och dela rapporter</span><span class="sxs-lookup"><span data-stu-id="0b521-128">Saving and sharing reports</span></span> 
<span data-ttu-id="0b521-129">Du kan spara användare rapporter, antingen privat bara tooyou under hello Mina rapporter eller delas med alla andra med åtkomst toothis Application Insights-resurs i hello delade rapporter.</span><span class="sxs-lookup"><span data-stu-id="0b521-129">You can save Users reports, either private just tooyou in hello My Reports section, or shared with everyone else with access toothis Application Insights resource in hello Shared Reports section.</span></span>  
 
<span data-ttu-id="0b521-130">När du sparar en rapport eller redigerar egenskaper, väljer du ”aktuellt relativt tidsintervall” toosave en rapport kommer kontinuerligt uppdateras data, gå tillbaka vissa fast tidsperiod.</span><span class="sxs-lookup"><span data-stu-id="0b521-130">While saving a report or editing its properties, choose "Current Relative Time Range" toosave a report will continuously refreshed data, going back some fixed amount of time.</span></span>  
 
<span data-ttu-id="0b521-131">Välj ”aktuellt absolut tidsintervall” toosave en rapport med en fast mängd data.</span><span class="sxs-lookup"><span data-stu-id="0b521-131">Choose "Current Absolute Time Range" toosave a report with a fixed set of data.</span></span> <span data-ttu-id="0b521-132">Tänk på att data i Application Insights bara lagras under 90 dagarna, så om du har gått mer än 90 dagar sedan en rapport med ett absolut tidsintervall har sparats, hello rapporten visas tom.</span><span class="sxs-lookup"><span data-stu-id="0b521-132">Keep in mind that data in Application Insights is only stored for 90 days, so if more than 90 days have passed since a report with an absolute time range was saved, hello report will appear empty.</span></span> 
 
## <a name="example-instances"></a><span data-ttu-id="0b521-133">Exempel instanser</span><span class="sxs-lookup"><span data-stu-id="0b521-133">Example instances</span></span>

<span data-ttu-id="0b521-134">hello instanser avsnittet visar information om en handfull enskilda användare, sessioner och händelser som täcks av hello aktuella frågan.</span><span class="sxs-lookup"><span data-stu-id="0b521-134">hello Example instances section shows information about a handful of individual users, sessions, or events that are matched by hello current query.</span></span> <span data-ttu-id="0b521-135">Med tanke på och utforska hello beteenden för enskilda användare i tillägg tooaggregates kan ge insikter om hur personer faktiskt använder din app.</span><span class="sxs-lookup"><span data-stu-id="0b521-135">Considering and exploring hello behaviors of individuals, in addition tooaggregates, can provide insights about how people actually use your app.</span></span> 
 
## <a name="insights"></a><span data-ttu-id="0b521-136">Insikter</span><span class="sxs-lookup"><span data-stu-id="0b521-136">Insights</span></span> 

<span data-ttu-id="0b521-137">hello insikter sidopanelen visar stora kluster med användare som har gemensamma egenskaper.</span><span class="sxs-lookup"><span data-stu-id="0b521-137">hello Insights sidebar shows large clusters of users that share common properties.</span></span> <span data-ttu-id="0b521-138">Dessa kluster kan avslöja konstigt trender i hur personer använder din app.</span><span class="sxs-lookup"><span data-stu-id="0b521-138">These clusters can uncover surprising trends in how people use your app.</span></span> <span data-ttu-id="0b521-139">Om exempelvis 40% för alla hello användning av din app kommer från personer som använder en enda funktion.</span><span class="sxs-lookup"><span data-stu-id="0b521-139">For example, if 40% of all of hello usage of your app comes from people using a single feature.</span></span>  


## <a name="next-steps"></a><span data-ttu-id="0b521-140">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="0b521-140">Next steps</span></span>
- <span data-ttu-id="0b521-141">tooenable användning inträffar, börja skicka [anpassade händelser](https://docs.microsoft.com/en-us/azure/application-insights/app-insights-api-custom-events-metrics#trackevent) eller [sidvisningar](https://docs.microsoft.com/azure/application-insights/app-insights-api-custom-events-metrics#page-views).</span><span class="sxs-lookup"><span data-stu-id="0b521-141">tooenable usage experiences, start sending [custom events](https://docs.microsoft.com/en-us/azure/application-insights/app-insights-api-custom-events-metrics#trackevent) or [page views](https://docs.microsoft.com/azure/application-insights/app-insights-api-custom-events-metrics#page-views).</span></span>
- <span data-ttu-id="0b521-142">Om du redan skickar anpassade händelser eller sidvyer utforska hello användning verktyg toolearn hur användare att använda tjänsten.</span><span class="sxs-lookup"><span data-stu-id="0b521-142">If you already send custom events or page views, explore hello Usage tools toolearn how users use your service.</span></span>
    - [<span data-ttu-id="0b521-143">Trattar</span><span class="sxs-lookup"><span data-stu-id="0b521-143">Funnels</span></span>](usage-funnels.md)
    - [<span data-ttu-id="0b521-144">Kvarhållning</span><span class="sxs-lookup"><span data-stu-id="0b521-144">Retention</span></span>](app-insights-usage-retention.md)
    - [<span data-ttu-id="0b521-145">Användarflöden</span><span class="sxs-lookup"><span data-stu-id="0b521-145">User Flows</span></span>](app-insights-usage-flows.md)
    - [<span data-ttu-id="0b521-146">Arbetsböcker</span><span class="sxs-lookup"><span data-stu-id="0b521-146">Workbooks</span></span>](app-insights-usage-workbooks.md)
    - [<span data-ttu-id="0b521-147">Lägg till användarkontext</span><span class="sxs-lookup"><span data-stu-id="0b521-147">Add user context</span></span>](app-insights-usage-send-user-context.md)

