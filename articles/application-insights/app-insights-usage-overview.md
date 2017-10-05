---
title: "Användningsanalys för webbprogram med Azure Application Insights | Microsoft docs"
description: "Förstå dina användare och vad de gör för din webbapp."
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
ms.openlocfilehash: 63b74399790b718e14a5b6e09bc009a336caf928
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/18/2017
---
# <a name="usage-analysis-for-web-applications-with-application-insights"></a><span data-ttu-id="2fc61-103">Användningsanalys för webbprogram med Application Insights</span><span class="sxs-lookup"><span data-stu-id="2fc61-103">Usage analysis for web applications with Application Insights</span></span>

<span data-ttu-id="2fc61-104">Vilka funktioner av ditt webbprogram som är mest populär?</span><span class="sxs-lookup"><span data-stu-id="2fc61-104">Which features of your web app are most popular?</span></span> <span data-ttu-id="2fc61-105">Dina användare nå sina mål med din app?</span><span class="sxs-lookup"><span data-stu-id="2fc61-105">Do your users achieve their goals with your app?</span></span> <span data-ttu-id="2fc61-106">De släppa vid särskilda tidpunkter och de kom tillbaka senare?</span><span class="sxs-lookup"><span data-stu-id="2fc61-106">Do they drop out at particular points, and do they return later?</span></span>  <span data-ttu-id="2fc61-107">[Azure Application Insights](app-insights-overview.md) hjälper dig att få kraftfulla insikter om hur personer använder ditt webbprogram.</span><span class="sxs-lookup"><span data-stu-id="2fc61-107">[Azure Application Insights](app-insights-overview.md) helps you gain powerful insights into how people use your web app.</span></span> <span data-ttu-id="2fc61-108">Varje gång du uppdaterar din app kan bedöma du hur bra fungerar för användare.</span><span class="sxs-lookup"><span data-stu-id="2fc61-108">Every time you update your app, you can assess how well it works for users.</span></span> <span data-ttu-id="2fc61-109">Med denna kunskap kan du datadrivna beslut om din nästa utvecklingscykler.</span><span class="sxs-lookup"><span data-stu-id="2fc61-109">With this knowledge, you can make data driven decisions about your next development cycles.</span></span>

## <a name="send-telemetry-from-your-app"></a><span data-ttu-id="2fc61-110">Skicka telemetri från din app</span><span class="sxs-lookup"><span data-stu-id="2fc61-110">Send telemetry from your app</span></span>

<span data-ttu-id="2fc61-111">Bästa möjliga upplevelse hämtas genom att installera Application Insights både i appen serverkoden och på webbsidor.</span><span class="sxs-lookup"><span data-stu-id="2fc61-111">The best experience is obtained by installing Application Insights both in your app server code, and in your web pages.</span></span> <span data-ttu-id="2fc61-112">Klient- och serverkomponenter för din app skicka telemetri tillbaka till Azure-portalen för analys.</span><span class="sxs-lookup"><span data-stu-id="2fc61-112">The client and server components of your app send telemetry back to the Azure portal for analysis.</span></span>

1. <span data-ttu-id="2fc61-113">**Serverkod:** installera modulen lämpliga för din [ASP.NET](app-insights-asp-net.md), [Azure](app-insights-azure.md), [Java](app-insights-java-get-started.md), [Node.js](app-insights-nodejs.md), eller [andra](app-insights-platforms.md) app.</span><span class="sxs-lookup"><span data-stu-id="2fc61-113">**Server code:** Install the appropriate module for your [ASP.NET](app-insights-asp-net.md), [Azure](app-insights-azure.md), [Java](app-insights-java-get-started.md), [Node.js](app-insights-nodejs.md), or [other](app-insights-platforms.md) app.</span></span>

    * <span data-ttu-id="2fc61-114">*Inte vill installera serverkod? Bara [skapa en Azure Application Insights-resurs](app-insights-create-new-resource.md).*</span><span class="sxs-lookup"><span data-stu-id="2fc61-114">*Don't want to install server code? Just [create an Azure Application Insights resource](app-insights-create-new-resource.md).*</span></span>

2. <span data-ttu-id="2fc61-115">**Koden webbsidan:** öppna den [Azure-portalen](https://portal.azure.com), öppna Application Insights-resurs för din app och öppna sedan **komma igång > övervaka och diagnostisera klientsidan**.</span><span class="sxs-lookup"><span data-stu-id="2fc61-115">**Web page code:** Open the [Azure portal](https://portal.azure.com), open the Application Insights resource for your app, and then open **Getting Started > Monitor and Diagnose Client-Side**.</span></span> 

    ![Kopiera skriptet till chefen för webbsidan master.](./media/app-insights-usage-overview/02-monitor-web-page.png)


3. <span data-ttu-id="2fc61-117">**Hämta telemetri:** köra projektet i felsökningsläge i några minuter och Sök efter resulterar i bladet översikt i Application Insights.</span><span class="sxs-lookup"><span data-stu-id="2fc61-117">**Get telemetry:** Run your project in debug mode for a few minutes, and then look for results in the Overview blade in Application Insights.</span></span>

    <span data-ttu-id="2fc61-118">Publicera en app för att övervaka prestanda för din app och ta reda på vad användarna gör med din app.</span><span class="sxs-lookup"><span data-stu-id="2fc61-118">Publish your app to monitor your app's performance and find out what your users are doing with your app.</span></span>

## <a name="include-user-and-session-id-in-your-telemetry"></a><span data-ttu-id="2fc61-119">Användar- och session-ID inkluderas i din telemetri</span><span class="sxs-lookup"><span data-stu-id="2fc61-119">Include user and session ID in your telemetry</span></span>
<span data-ttu-id="2fc61-120">Application Insights kräver ett sätt att identifiera dem för att spåra användare över tid.</span><span class="sxs-lookup"><span data-stu-id="2fc61-120">To track users over time, Application Insights requires a way to identify them.</span></span> <span data-ttu-id="2fc61-121">Verktyget händelser är det enda verktyg för användning som inte kräver ett användar-ID eller ett sessions-ID.</span><span class="sxs-lookup"><span data-stu-id="2fc61-121">The Events tool is the only Usage tool that does not require a user ID or a session ID.</span></span>

<span data-ttu-id="2fc61-122">Börja skicka dessa ID: N [här](https://docs.microsoft.com/azure/application-insights/app-insights-usage-send-user-context).</span><span class="sxs-lookup"><span data-stu-id="2fc61-122">Start sending these IDs [here](https://docs.microsoft.com/azure/application-insights/app-insights-usage-send-user-context).</span></span>

## <a name="explore-usage-demographics-and-statistics"></a><span data-ttu-id="2fc61-123">Utforska användning demografi och statistik</span><span class="sxs-lookup"><span data-stu-id="2fc61-123">Explore usage demographics and statistics</span></span>
<span data-ttu-id="2fc61-124">Ta reda om personer använder din app, vilka sidor som de är mest intresserad av, där användarna finns, vilka webbläsare och operativsystem som de använder.</span><span class="sxs-lookup"><span data-stu-id="2fc61-124">Find out when people use your app, what pages they're most interested in, where your users are located, what browsers and operating systems they use.</span></span> 

<span data-ttu-id="2fc61-125">Rapporter för användare och sessioner filtrera dina data efter sidor eller anpassade händelser och segmentera dem efter egenskaper som plats, miljö och sida.</span><span class="sxs-lookup"><span data-stu-id="2fc61-125">The Users and Sessions reports filter your data by pages or custom events, and segment them by properties such as location, environment, and page.</span></span> <span data-ttu-id="2fc61-126">Du kan också lägga till egna filter.</span><span class="sxs-lookup"><span data-stu-id="2fc61-126">You can also add your own filters.</span></span>

![Användare](./media/app-insights-usage-overview/users.png)  

<span data-ttu-id="2fc61-128">Insikter till höger peka ut intressanta mönster i datauppsättningen.</span><span class="sxs-lookup"><span data-stu-id="2fc61-128">Insights on the right point out interesting patterns in the set of data.</span></span>  

* <span data-ttu-id="2fc61-129">Den **användare** rapporten räknar antalet unika användare som har åtkomst till dina sidor i din valda tidsperioder.</span><span class="sxs-lookup"><span data-stu-id="2fc61-129">The **Users** report counts the numbers of unique users that access your pages within your chosen time periods.</span></span> <span data-ttu-id="2fc61-130">(Användare ska räknas genom att använda cookies.</span><span class="sxs-lookup"><span data-stu-id="2fc61-130">(Users are counted by using cookies.</span></span> <span data-ttu-id="2fc61-131">Om någon har åtkomst till din plats med olika webbläsare eller klientdatorer eller rensar sina cookies, och sedan ska räknas mer än en gång.)</span><span class="sxs-lookup"><span data-stu-id="2fc61-131">If someone accesses your site with different browsers or client machines, or clears their cookies, then they will be counted more than once.)</span></span>
* <span data-ttu-id="2fc61-132">Den **sessioner** rapporten antalet användarsessioner som har åtkomst till webbplatsen.</span><span class="sxs-lookup"><span data-stu-id="2fc61-132">The **Sessions** report counts the number of user sessions that access your site.</span></span> <span data-ttu-id="2fc61-133">En session är en period av aktivitet av en användare eller avslutas med en period av inaktivitet av mer än en halvtimme.</span><span class="sxs-lookup"><span data-stu-id="2fc61-133">A session is a period of activity by a user, terminated by a period of inactivity of more than half an hour.</span></span>

[<span data-ttu-id="2fc61-134">Mer information om verktyg för användare, sessioner och händelser</span><span class="sxs-lookup"><span data-stu-id="2fc61-134">More about the Users, Sessions, and Events tools</span></span>](app-insights-usage-segmentation.md)  

## <a name="page-views"></a><span data-ttu-id="2fc61-135">Sidvisningar</span><span class="sxs-lookup"><span data-stu-id="2fc61-135">Page views</span></span>

<span data-ttu-id="2fc61-136">Klicka på via panelen sidvisningar få en detaljerad analys av de mest populära sidorna i bladet användning:</span><span class="sxs-lookup"><span data-stu-id="2fc61-136">From the Usage blade, click through the Page Views tile to get a breakdown of your most popular pages:</span></span>

![Översikt över-bladet klickar du på sidan vyer diagrammet](./media/app-insights-usage-overview/05-games.png)

<span data-ttu-id="2fc61-138">Exemplet ovan är från en webbplats för spel.</span><span class="sxs-lookup"><span data-stu-id="2fc61-138">The example above is from a games web site.</span></span> <span data-ttu-id="2fc61-139">Diagram kan se vi omedelbart:</span><span class="sxs-lookup"><span data-stu-id="2fc61-139">From the charts, we can instantly see:</span></span>

* <span data-ttu-id="2fc61-140">Användning har förbättrats i den senaste veckan.</span><span class="sxs-lookup"><span data-stu-id="2fc61-140">Usage hasn't improved in the past week.</span></span> <span data-ttu-id="2fc61-141">Kanske fundera över sökmotoroptimering?</span><span class="sxs-lookup"><span data-stu-id="2fc61-141">Maybe we should think about search engine optimization?</span></span>
* <span data-ttu-id="2fc61-142">Tennis är den mest populära spel sidan.</span><span class="sxs-lookup"><span data-stu-id="2fc61-142">Tennis is the most popular game page.</span></span> <span data-ttu-id="2fc61-143">Nu ska vi fokusera på ytterligare förbättringar i den här sidan.</span><span class="sxs-lookup"><span data-stu-id="2fc61-143">Let's focus on further improvements to this page.</span></span>
* <span data-ttu-id="2fc61-144">I genomsnitt gå användare Tennis cirka tre gånger per vecka.</span><span class="sxs-lookup"><span data-stu-id="2fc61-144">On average, users visit the Tennis page about three times per week.</span></span> <span data-ttu-id="2fc61-145">(Det finns ungefär tre gånger så mycket sessioner än användare.)</span><span class="sxs-lookup"><span data-stu-id="2fc61-145">(There are about three times more sessions than users.)</span></span>
* <span data-ttu-id="2fc61-146">De flesta användare besöker du webbplatsen under Arbetsvecka USA och i arbetstid.</span><span class="sxs-lookup"><span data-stu-id="2fc61-146">Most users visit the site during the U.S. working week, and in working hours.</span></span> <span data-ttu-id="2fc61-147">Vi bör kanske ge knappen ”snabb Dölj” på webbsidan.</span><span class="sxs-lookup"><span data-stu-id="2fc61-147">Perhaps we should provide a "quick hide" button on the web page.</span></span>
* <span data-ttu-id="2fc61-148">Den [anteckningar](app-insights-annotations.md) visa diagrammet när nya versioner av webbplatsen har distribuerats.</span><span class="sxs-lookup"><span data-stu-id="2fc61-148">The [annotations](app-insights-annotations.md) on the chart show when new versions of the website were deployed.</span></span> <span data-ttu-id="2fc61-149">Ingen av de senaste distributionerna hade en märkbar effekt på användning.</span><span class="sxs-lookup"><span data-stu-id="2fc61-149">None of the recent deployments had a noticeable effect on usage.</span></span>

<span data-ttu-id="2fc61-150">Vad händer om du vill undersöka trafiken till webbplatsen i detalj som delas av en anpassad egenskap som ska skickas på webbplatsen i dess sidan Visa telemetri?</span><span class="sxs-lookup"><span data-stu-id="2fc61-150">What if you want to investigate the traffic to your site in more detail, like splitting by a custom property your site sends in its page view telemetry?</span></span>

1. <span data-ttu-id="2fc61-151">Öppna den **händelser** verktyg i menyn Application Insights-resurs.</span><span class="sxs-lookup"><span data-stu-id="2fc61-151">Open the **Events** tool in the Application Insights resource menu.</span></span> <span data-ttu-id="2fc61-152">Det här verktyget kan du analysera hur många sidvisningar och anpassade händelser skickades från din app, baserat på en mängd olika alternativ för filtrering, cohorting och segmentering.</span><span class="sxs-lookup"><span data-stu-id="2fc61-152">This tool lets you analyze how many page views and custom events were sent from your app, based on a variety of filtering, cohorting, and segmentation options.</span></span>
2. <span data-ttu-id="2fc61-153">I listrutan ”som används för” Välj ”alla sidvisningen”.</span><span class="sxs-lookup"><span data-stu-id="2fc61-153">In the "Who used" dropdown, select "Any Page View".</span></span>
3. <span data-ttu-id="2fc61-154">Välj en egenskap som du vill dela dina sidan Visa telemetri i listrutan ”delning av”.</span><span class="sxs-lookup"><span data-stu-id="2fc61-154">In the "Split by" dropdown, select a property by which to split your page view telemetry.</span></span>

## <a name="retention---how-many-users-come-back"></a><span data-ttu-id="2fc61-155">Innehållen – hur många användare försöka igen?</span><span class="sxs-lookup"><span data-stu-id="2fc61-155">Retention - how many users come back?</span></span>

<span data-ttu-id="2fc61-156">Kvarhållning hjälper dig att förstå hur ofta användarna återgå till att använda appen, baserat på kohorter av användare som utförde vissa företag åtgärden under en viss tidsperiod.</span><span class="sxs-lookup"><span data-stu-id="2fc61-156">Retention helps you understand how often your users return to use their app, based on cohorts of users that performed some business action during a certain time bucket.</span></span> 

- <span data-ttu-id="2fc61-157">Förstå vilka specifika funktioner orsaka användare att komma tillbaka mer än andra</span><span class="sxs-lookup"><span data-stu-id="2fc61-157">Understand what specific features cause users to come back more than others</span></span> 
- <span data-ttu-id="2fc61-158">Formuläret hypoteser baserat på verkliga användardata</span><span class="sxs-lookup"><span data-stu-id="2fc61-158">Form hypotheses based on real user data</span></span> 
- <span data-ttu-id="2fc61-159">Avgöra om kvarhållning är ett problem i produkten</span><span class="sxs-lookup"><span data-stu-id="2fc61-159">Determine whether retention is a problem in your product</span></span> 

![Kvarhållning](./media/app-insights-usage-overview/retention.png) 

<span data-ttu-id="2fc61-161">Kvarhållning kontroller överst kan du definiera specifika händelser och tidsintervallet för att beräkna kvarhållning.</span><span class="sxs-lookup"><span data-stu-id="2fc61-161">The retention controls on top allow you to define specific events and time range to calculate retention.</span></span> <span data-ttu-id="2fc61-162">Diagrammet i mitten ger en bild av övergripande kvarhållning procentandelen av det angivna tidsintervallet.</span><span class="sxs-lookup"><span data-stu-id="2fc61-162">The graph in the middle gives a visual representation of the overall retention percentage by the time range specified.</span></span> <span data-ttu-id="2fc61-163">Längst ned i diagrammet representerar enskilda kvarhållning i en viss tidsperiod.</span><span class="sxs-lookup"><span data-stu-id="2fc61-163">The graph on the bottom represents individual retention in a given time period.</span></span> <span data-ttu-id="2fc61-164">Den här detaljnivån tillåter dig att förstå vad användarna gör och vad kan påverka returnerar användare på en mer detaljerad granularitet.</span><span class="sxs-lookup"><span data-stu-id="2fc61-164">This level of detail allows you to understand what your users are doing and what might affect returning users on a more detailed granularity.</span></span>  

[<span data-ttu-id="2fc61-165">Mer information om verktyget kvarhållning</span><span class="sxs-lookup"><span data-stu-id="2fc61-165">More about the Retention tool</span></span>](app-insights-usage-retention.md)

## <a name="custom-business-events"></a><span data-ttu-id="2fc61-166">Anpassade affärshändelser</span><span class="sxs-lookup"><span data-stu-id="2fc61-166">Custom business events</span></span>

<span data-ttu-id="2fc61-167">För att få en förståelse för vad användarna göra med ditt webbprogram, är det praktiskt att infoga rader med kod anpassade händelser.</span><span class="sxs-lookup"><span data-stu-id="2fc61-167">To get a clear understanding of what users do with your web app, it's useful to insert lines of code to log custom events.</span></span> <span data-ttu-id="2fc61-168">Dessa händelser kan spåra allt från detaljerad användaråtgärder, till exempel att klicka på specifika knapparna till större affärshändelser, till exempel inköp eller vinna spel.</span><span class="sxs-lookup"><span data-stu-id="2fc61-168">These events can track anything from detailed user actions such as clicking specific buttons, to more significant business events such as making a purchase or winning a game.</span></span> 

<span data-ttu-id="2fc61-169">Men i vissa fall sidvisningar representerar användbar händelser, det är inte sant i allmänhet.</span><span class="sxs-lookup"><span data-stu-id="2fc61-169">Although in some cases, page views can represent useful events, it isn't true in general.</span></span> <span data-ttu-id="2fc61-170">En användare kan öppna en produktsidan utan inköp av produkten.</span><span class="sxs-lookup"><span data-stu-id="2fc61-170">A user can open a product page without buying the product.</span></span> 

<span data-ttu-id="2fc61-171">Med specifika händelser, kan du skapa diagram användarnas pågår via webbplatsen.</span><span class="sxs-lookup"><span data-stu-id="2fc61-171">With specific business events, you can chart your users' progress through your site.</span></span> <span data-ttu-id="2fc61-172">Du kan ta reda på inställningar för olika alternativ, och där de släppa ut eller har problem.</span><span class="sxs-lookup"><span data-stu-id="2fc61-172">You can find out their preferences for different options, and where they drop out or have difficulties.</span></span> <span data-ttu-id="2fc61-173">Med denna kunskap kan fatta du välgrundade beslut om prioriteringarna i eftersläpning för utveckling.</span><span class="sxs-lookup"><span data-stu-id="2fc61-173">With this knowledge, you can make informed decisions about the priorities in your development backlog.</span></span>

<span data-ttu-id="2fc61-174">Händelser kan loggas på webbsidan:</span><span class="sxs-lookup"><span data-stu-id="2fc61-174">Events can be logged in the web page:</span></span>

```JavaScript

    appInsights.trackEvent("ExpandDetailTab", {DetailTab: tabName});
```

<span data-ttu-id="2fc61-175">Eller i serverdelen av webbprogrammet:</span><span class="sxs-lookup"><span data-stu-id="2fc61-175">Or in the server side of the web app:</span></span>

```C#
    var tc = new Microsoft.ApplicationInsights.TelemetryClient();
    tc.TrackEvent("CreatedAccount", new Dictionary<string,string> {"AccountType":account.Type}, null);
    ...
    tc.TrackEvent("AddedItemToCart", new Dictionary<string,string> {"Item":item.Name}, null);
    ...
    tc.TrackEvent("CompletedPurchase");
```

<span data-ttu-id="2fc61-176">Du kan koppla egenskapsvärden till dessa händelser så att du kan filtrera eller dela upp händelser när du inspektera dem i portalen.</span><span class="sxs-lookup"><span data-stu-id="2fc61-176">You can attach property values to these events, so that you can filter or split the events when you inspect them in the portal.</span></span> <span data-ttu-id="2fc61-177">Dessutom kan är en standarduppsättning egenskaper kopplad till varje händelse, till exempel anonyma användar-ID, vilket gör att du kan spåra sekvensen av aktiviteter för en enskild användare.</span><span class="sxs-lookup"><span data-stu-id="2fc61-177">In addition, a standard set of properties is attached to each event, such as anonymous user ID, which allows you to trace the sequence of activities of an individual user.</span></span>

<span data-ttu-id="2fc61-178">Lär dig mer om [anpassade händelser](app-insights-api-custom-events-metrics.md#trackevent) och [egenskaper](app-insights-api-custom-events-metrics.md#properties).</span><span class="sxs-lookup"><span data-stu-id="2fc61-178">Learn more about [custom events](app-insights-api-custom-events-metrics.md#trackevent) and [properties](app-insights-api-custom-events-metrics.md#properties).</span></span>

### <a name="slice-and-dice-events"></a><span data-ttu-id="2fc61-179">Rapportanvändarna händelser</span><span class="sxs-lookup"><span data-stu-id="2fc61-179">Slice and dice events</span></span>

<span data-ttu-id="2fc61-180">I Verktyg för användare, sessioner och händelser du segmentera och undersök anpassade händelser efter användarnamn, händelse och egenskaper.</span><span class="sxs-lookup"><span data-stu-id="2fc61-180">In the Users, Sessions, and Events tools, you can slice and dice custom events by user, event name, and properties.</span></span>
<span data-ttu-id="2fc61-181">![Användare](./media/app-insights-usage-overview/users.png)</span><span class="sxs-lookup"><span data-stu-id="2fc61-181">![Users](./media/app-insights-usage-overview/users.png)</span></span>  
  
## <a name="design-the-telemetry-with-the-app"></a><span data-ttu-id="2fc61-182">Design telemetri med appen</span><span class="sxs-lookup"><span data-stu-id="2fc61-182">Design the telemetry with the app</span></span>

<span data-ttu-id="2fc61-183">När du utformar varje funktion i din app kan du överväga hur du kommer att mäta dess framgång med dina användare.</span><span class="sxs-lookup"><span data-stu-id="2fc61-183">When you are designing each feature of your app, consider how you are going to measure its success with your users.</span></span> <span data-ttu-id="2fc61-184">Bestäm vilka affärshändelser som du behöver registrera och koden spårnings-anrop för dessa händelser i din app från början.</span><span class="sxs-lookup"><span data-stu-id="2fc61-184">Decide what business events you need to record, and code the tracking calls for those events into your app from the start.</span></span>

## <a name="a--b-testing"></a><span data-ttu-id="2fc61-185">EN | B-testning</span><span class="sxs-lookup"><span data-stu-id="2fc61-185">A | B Testing</span></span>
<span data-ttu-id="2fc61-186">Om du inte vet vilken variant av en funktion blir bättre, släpp båda, vilket gör varje tillgängliga för olika användare.</span><span class="sxs-lookup"><span data-stu-id="2fc61-186">If you don't know which variant of a feature will be more successful, release both of them, making each accessible to different users.</span></span> <span data-ttu-id="2fc61-187">Mäter framgången för varje och gå sedan till en enhetlig version.</span><span class="sxs-lookup"><span data-stu-id="2fc61-187">Measure the success of each, and then move to a unified version.</span></span>

<span data-ttu-id="2fc61-188">För den här tekniken kopplar du distinkta värden till all telemetri som skickas av varje version av appen.</span><span class="sxs-lookup"><span data-stu-id="2fc61-188">For this technique, you attach distinct property values to all the telemetry that is sent by each version of your app.</span></span> <span data-ttu-id="2fc61-189">Du kan göra det genom att definiera egenskaperna i den aktiva TelemetryContext.</span><span class="sxs-lookup"><span data-stu-id="2fc61-189">You can do that by defining properties in the active TelemetryContext.</span></span> <span data-ttu-id="2fc61-190">Dessa standardegenskaperna läggs till varje telemetri meddelande som programmet skickar – inte bara din egna meddelanden, men även standard telemetri.</span><span class="sxs-lookup"><span data-stu-id="2fc61-190">These default properties are added to every telemetry message that the application sends - not just your custom messages, but the standard telemetry as well.</span></span>

<span data-ttu-id="2fc61-191">Filtrera i Application Insights-portalen och dela dina data på egenskapsvärden för att jämföra olika versioner.</span><span class="sxs-lookup"><span data-stu-id="2fc61-191">In the Application Insights portal, filter and split your data on the property values, so as to compare the different versions.</span></span>

<span data-ttu-id="2fc61-192">Gör [ställa in en telemetri initieraren](app-insights-api-filtering-sampling.md##add-properties-itelemetryinitializer):</span><span class="sxs-lookup"><span data-stu-id="2fc61-192">To do this, [set up a telemetry initializer](app-insights-api-filtering-sampling.md##add-properties-itelemetryinitializer):</span></span>

```C#


    // Telemetry initializer class
    public class MyTelemetryInitializer : ITelemetryInitializer
    {
        public void Initialize (ITelemetry telemetry)
        {
            telemetry.Properties["AppVersion"] = "v2.1";
        }
    }
```

<span data-ttu-id="2fc61-193">I web app initieraren, till exempel Global.asax.cs:</span><span class="sxs-lookup"><span data-stu-id="2fc61-193">In the web app initializer such as Global.asax.cs:</span></span>

```C#

    protected void Application_Start()
    {
        // ...
        TelemetryConfiguration.Active.TelemetryInitializers
        .Add(new MyTelemetryInitializer());
    }
```

<span data-ttu-id="2fc61-194">Alla nya TelemetryClients lägger automatiskt till det angivna egenskapsvärdet.</span><span class="sxs-lookup"><span data-stu-id="2fc61-194">All new TelemetryClients automatically add the property value you specify.</span></span> <span data-ttu-id="2fc61-195">Enskilda telemetriska händelser kan åsidosätta standardvärden.</span><span class="sxs-lookup"><span data-stu-id="2fc61-195">Individual telemetry events can override the default values.</span></span>

## <a name="next-steps"></a><span data-ttu-id="2fc61-196">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="2fc61-196">Next steps</span></span>
   - [<span data-ttu-id="2fc61-197">Användare, sessioner, händelser</span><span class="sxs-lookup"><span data-stu-id="2fc61-197">Users, Sessions, Events</span></span>](app-insights-usage-segmentation.md)
   - [<span data-ttu-id="2fc61-198">Trattar</span><span class="sxs-lookup"><span data-stu-id="2fc61-198">Funnels</span></span>](usage-funnels.md)
   - [<span data-ttu-id="2fc61-199">Kvarhållning</span><span class="sxs-lookup"><span data-stu-id="2fc61-199">Retention</span></span>](app-insights-usage-retention.md)
   - [<span data-ttu-id="2fc61-200">Användarflöden</span><span class="sxs-lookup"><span data-stu-id="2fc61-200">User Flows</span></span>](app-insights-usage-flows.md)
   - [<span data-ttu-id="2fc61-201">Arbetsböcker</span><span class="sxs-lookup"><span data-stu-id="2fc61-201">Workbooks</span></span>](app-insights-usage-workbooks.md)
   - [<span data-ttu-id="2fc61-202">Lägg till användarkontext</span><span class="sxs-lookup"><span data-stu-id="2fc61-202">Add user context</span></span>](app-insights-usage-send-user-context.md)
