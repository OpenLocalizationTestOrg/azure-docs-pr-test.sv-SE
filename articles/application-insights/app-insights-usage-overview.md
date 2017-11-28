---
title: "aaaUsage analys för webbprogram med Azure Application Insights | Microsoft docs"
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
ms.openlocfilehash: f7f9173cf411fa0d2dfb3b5ba99134a02bbc0e89
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="usage-analysis-for-web-applications-with-application-insights"></a><span data-ttu-id="3b01f-103">Användningsanalys för webbprogram med Application Insights</span><span class="sxs-lookup"><span data-stu-id="3b01f-103">Usage analysis for web applications with Application Insights</span></span>

<span data-ttu-id="3b01f-104">Vilka funktioner av ditt webbprogram som är mest populär?</span><span class="sxs-lookup"><span data-stu-id="3b01f-104">Which features of your web app are most popular?</span></span> <span data-ttu-id="3b01f-105">Dina användare nå sina mål med din app?</span><span class="sxs-lookup"><span data-stu-id="3b01f-105">Do your users achieve their goals with your app?</span></span> <span data-ttu-id="3b01f-106">De släppa vid särskilda tidpunkter och de kom tillbaka senare?</span><span class="sxs-lookup"><span data-stu-id="3b01f-106">Do they drop out at particular points, and do they return later?</span></span>  <span data-ttu-id="3b01f-107">[Azure Application Insights](app-insights-overview.md) hjälper dig att få kraftfulla insikter om hur personer använder ditt webbprogram.</span><span class="sxs-lookup"><span data-stu-id="3b01f-107">[Azure Application Insights](app-insights-overview.md) helps you gain powerful insights into how people use your web app.</span></span> <span data-ttu-id="3b01f-108">Varje gång du uppdaterar din app kan bedöma du hur bra fungerar för användare.</span><span class="sxs-lookup"><span data-stu-id="3b01f-108">Every time you update your app, you can assess how well it works for users.</span></span> <span data-ttu-id="3b01f-109">Med denna kunskap kan du datadrivna beslut om din nästa utvecklingscykler.</span><span class="sxs-lookup"><span data-stu-id="3b01f-109">With this knowledge, you can make data driven decisions about your next development cycles.</span></span>

## <a name="send-telemetry-from-your-app"></a><span data-ttu-id="3b01f-110">Skicka telemetri från din app</span><span class="sxs-lookup"><span data-stu-id="3b01f-110">Send telemetry from your app</span></span>

<span data-ttu-id="3b01f-111">hello bästa möjliga upplevelse hämtas genom att installera Application Insights både i appen serverkoden och på webbsidor.</span><span class="sxs-lookup"><span data-stu-id="3b01f-111">hello best experience is obtained by installing Application Insights both in your app server code, and in your web pages.</span></span> <span data-ttu-id="3b01f-112">hello klient- och serverkomponenter för din app skicka telemetri tillbaka toohello Azure-portalen för analys.</span><span class="sxs-lookup"><span data-stu-id="3b01f-112">hello client and server components of your app send telemetry back toohello Azure portal for analysis.</span></span>

1. <span data-ttu-id="3b01f-113">**Serverkod:** lämpliga Install hello-modulen för din [ASP.NET](app-insights-asp-net.md), [Azure](app-insights-azure.md), [Java](app-insights-java-get-started.md), [Node.js](app-insights-nodejs.md), eller [andra](app-insights-platforms.md) app.</span><span class="sxs-lookup"><span data-stu-id="3b01f-113">**Server code:** Install hello appropriate module for your [ASP.NET](app-insights-asp-net.md), [Azure](app-insights-azure.md), [Java](app-insights-java-get-started.md), [Node.js](app-insights-nodejs.md), or [other](app-insights-platforms.md) app.</span></span>

    * <span data-ttu-id="3b01f-114">*Inte vill tooinstall serverkoden? Bara [skapa en Azure Application Insights-resurs](app-insights-create-new-resource.md).*</span><span class="sxs-lookup"><span data-stu-id="3b01f-114">*Don't want tooinstall server code? Just [create an Azure Application Insights resource](app-insights-create-new-resource.md).*</span></span>

2. <span data-ttu-id="3b01f-115">**Koden webbsidan:** öppna hello [Azure-portalen](https://portal.azure.com), öppna hello Application Insights-resurs för din app och öppna sedan **komma igång > övervaka och diagnostisera klientsidan**.</span><span class="sxs-lookup"><span data-stu-id="3b01f-115">**Web page code:** Open hello [Azure portal](https://portal.azure.com), open hello Application Insights resource for your app, and then open **Getting Started > Monitor and Diagnose Client-Side**.</span></span> 

    ![Kopiera hello skript till hello head av webbsidan master.](./media/app-insights-usage-overview/02-monitor-web-page.png)


3. <span data-ttu-id="3b01f-117">**Hämta telemetri:** köra projektet i felsökningsläge i några minuter och Sök efter resulterar i hello översikt bladet i Application Insights.</span><span class="sxs-lookup"><span data-stu-id="3b01f-117">**Get telemetry:** Run your project in debug mode for a few minutes, and then look for results in hello Overview blade in Application Insights.</span></span>

    <span data-ttu-id="3b01f-118">Publicera din app toomonitor appens prestanda och ta reda på vad användarna gör med din app.</span><span class="sxs-lookup"><span data-stu-id="3b01f-118">Publish your app toomonitor your app's performance and find out what your users are doing with your app.</span></span>

## <a name="include-user-and-session-id-in-your-telemetry"></a><span data-ttu-id="3b01f-119">Användar- och session-ID inkluderas i din telemetri</span><span class="sxs-lookup"><span data-stu-id="3b01f-119">Include user and session ID in your telemetry</span></span>
<span data-ttu-id="3b01f-120">tootrack användare över tid, Programinsikter kräver en sätt tooidentify dem.</span><span class="sxs-lookup"><span data-stu-id="3b01f-120">tootrack users over time, Application Insights requires a way tooidentify them.</span></span> <span data-ttu-id="3b01f-121">hello händelser verktyget hello endast användning verktyg som inte kräver ett användar-ID eller ett sessions-ID.</span><span class="sxs-lookup"><span data-stu-id="3b01f-121">hello Events tool is hello only Usage tool that does not require a user ID or a session ID.</span></span>

<span data-ttu-id="3b01f-122">Börja skicka dessa ID: N [här](https://docs.microsoft.com/azure/application-insights/app-insights-usage-send-user-context).</span><span class="sxs-lookup"><span data-stu-id="3b01f-122">Start sending these IDs [here](https://docs.microsoft.com/azure/application-insights/app-insights-usage-send-user-context).</span></span>

## <a name="explore-usage-demographics-and-statistics"></a><span data-ttu-id="3b01f-123">Utforska användning demografi och statistik</span><span class="sxs-lookup"><span data-stu-id="3b01f-123">Explore usage demographics and statistics</span></span>
<span data-ttu-id="3b01f-124">Ta reda om personer använder din app, vilka sidor som de är mest intresserad av, där användarna finns, vilka webbläsare och operativsystem som de använder.</span><span class="sxs-lookup"><span data-stu-id="3b01f-124">Find out when people use your app, what pages they're most interested in, where your users are located, what browsers and operating systems they use.</span></span> 

<span data-ttu-id="3b01f-125">hello användare och sessioner rapporter filtrera dina data efter sidor eller anpassade händelser och segmentera dem efter egenskaper som plats, miljö och sida.</span><span class="sxs-lookup"><span data-stu-id="3b01f-125">hello Users and Sessions reports filter your data by pages or custom events, and segment them by properties such as location, environment, and page.</span></span> <span data-ttu-id="3b01f-126">Du kan också lägga till egna filter.</span><span class="sxs-lookup"><span data-stu-id="3b01f-126">You can also add your own filters.</span></span>

![Användare](./media/app-insights-usage-overview/users.png)  

<span data-ttu-id="3b01f-128">Om rätt hello peka ut intressanta mönster i hello uppsättning data.</span><span class="sxs-lookup"><span data-stu-id="3b01f-128">Insights on hello right point out interesting patterns in hello set of data.</span></span>  

* <span data-ttu-id="3b01f-129">Hej **användare** rapporten räknar hello antal unika användare som har åtkomst till dina sidor i din valda tidsperioder.</span><span class="sxs-lookup"><span data-stu-id="3b01f-129">hello **Users** report counts hello numbers of unique users that access your pages within your chosen time periods.</span></span> <span data-ttu-id="3b01f-130">(Användare ska räknas genom att använda cookies.</span><span class="sxs-lookup"><span data-stu-id="3b01f-130">(Users are counted by using cookies.</span></span> <span data-ttu-id="3b01f-131">Om någon har åtkomst till din plats med olika webbläsare eller klientdatorer eller rensar sina cookies, och sedan ska räknas mer än en gång.)</span><span class="sxs-lookup"><span data-stu-id="3b01f-131">If someone accesses your site with different browsers or client machines, or clears their cookies, then they will be counted more than once.)</span></span>
* <span data-ttu-id="3b01f-132">Hej **sessioner** rapporten är hello antalet användarsessioner som har åtkomst till webbplatsen.</span><span class="sxs-lookup"><span data-stu-id="3b01f-132">hello **Sessions** report counts hello number of user sessions that access your site.</span></span> <span data-ttu-id="3b01f-133">En session är en period av aktivitet av en användare eller avslutas med en period av inaktivitet av mer än en halvtimme.</span><span class="sxs-lookup"><span data-stu-id="3b01f-133">A session is a period of activity by a user, terminated by a period of inactivity of more than half an hour.</span></span>

[<span data-ttu-id="3b01f-134">Mer information om verktyg för hello användare, sessioner och händelser</span><span class="sxs-lookup"><span data-stu-id="3b01f-134">More about hello Users, Sessions, and Events tools</span></span>](app-insights-usage-segmentation.md)  

## <a name="page-views"></a><span data-ttu-id="3b01f-135">Sidvisningar</span><span class="sxs-lookup"><span data-stu-id="3b01f-135">Page views</span></span>

<span data-ttu-id="3b01f-136">Från hello användning bladet, klickar du på via hello sidvisningar panelen tooget en detaljerad analys av dina mest populära sidor:</span><span class="sxs-lookup"><span data-stu-id="3b01f-136">From hello Usage blade, click through hello Page Views tile tooget a breakdown of your most popular pages:</span></span>

![Hello översikt bladet, klickar du på hello sidan vyerna diagram](./media/app-insights-usage-overview/05-games.png)

<span data-ttu-id="3b01f-138">hello-exemplet ovan är från en webbplats för spel.</span><span class="sxs-lookup"><span data-stu-id="3b01f-138">hello example above is from a games web site.</span></span> <span data-ttu-id="3b01f-139">Från hello diagram ser omedelbart vi:</span><span class="sxs-lookup"><span data-stu-id="3b01f-139">From hello charts, we can instantly see:</span></span>

* <span data-ttu-id="3b01f-140">Användning har förbättrats i hello gångna veckan.</span><span class="sxs-lookup"><span data-stu-id="3b01f-140">Usage hasn't improved in hello past week.</span></span> <span data-ttu-id="3b01f-141">Kanske fundera över sökmotoroptimering?</span><span class="sxs-lookup"><span data-stu-id="3b01f-141">Maybe we should think about search engine optimization?</span></span>
* <span data-ttu-id="3b01f-142">Tennis är hello populäraste spel sida.</span><span class="sxs-lookup"><span data-stu-id="3b01f-142">Tennis is hello most popular game page.</span></span> <span data-ttu-id="3b01f-143">Nu ska vi fokusera på ytterligare förbättringar toothis sidan.</span><span class="sxs-lookup"><span data-stu-id="3b01f-143">Let's focus on further improvements toothis page.</span></span>
* <span data-ttu-id="3b01f-144">I genomsnitt besöka användare hello Tennis cirka tre gånger per vecka.</span><span class="sxs-lookup"><span data-stu-id="3b01f-144">On average, users visit hello Tennis page about three times per week.</span></span> <span data-ttu-id="3b01f-145">(Det finns ungefär tre gånger så mycket sessioner än användare.)</span><span class="sxs-lookup"><span data-stu-id="3b01f-145">(There are about three times more sessions than users.)</span></span>
* <span data-ttu-id="3b01f-146">De flesta användare besöka hello under hello arbetsvecka för USA och i arbetstid.</span><span class="sxs-lookup"><span data-stu-id="3b01f-146">Most users visit hello site during hello U.S. working week, and in working hours.</span></span> <span data-ttu-id="3b01f-147">Vi bör kanske ge knappen ”snabb Dölj” på hello webbsida.</span><span class="sxs-lookup"><span data-stu-id="3b01f-147">Perhaps we should provide a "quick hide" button on hello web page.</span></span>
* <span data-ttu-id="3b01f-148">Hej [anteckningar](app-insights-annotations.md) visar hello diagrammet när nya versioner av hello webbplats har distribuerats.</span><span class="sxs-lookup"><span data-stu-id="3b01f-148">hello [annotations](app-insights-annotations.md) on hello chart show when new versions of hello website were deployed.</span></span> <span data-ttu-id="3b01f-149">Ingen av hello senaste distributioner hade en märkbar effekt på användning.</span><span class="sxs-lookup"><span data-stu-id="3b01f-149">None of hello recent deployments had a noticeable effect on usage.</span></span>

<span data-ttu-id="3b01f-150">Vad händer om du vill tooinvestigate hello trafik tooyour platsen i detalj som delas av en anpassad egenskap som ska skickas på webbplatsen i dess sidan Visa telemetri?</span><span class="sxs-lookup"><span data-stu-id="3b01f-150">What if you want tooinvestigate hello traffic tooyour site in more detail, like splitting by a custom property your site sends in its page view telemetry?</span></span>

1. <span data-ttu-id="3b01f-151">Öppna hello **händelser** verktyg i menyn för hello Application Insights-resurs.</span><span class="sxs-lookup"><span data-stu-id="3b01f-151">Open hello **Events** tool in hello Application Insights resource menu.</span></span> <span data-ttu-id="3b01f-152">Det här verktyget kan du analysera hur många sidvisningar och anpassade händelser skickades från din app, baserat på en mängd olika alternativ för filtrering, cohorting och segmentering.</span><span class="sxs-lookup"><span data-stu-id="3b01f-152">This tool lets you analyze how many page views and custom events were sent from your app, based on a variety of filtering, cohorting, and segmentation options.</span></span>
2. <span data-ttu-id="3b01f-153">I hello ”som används för” listrutan, väljer du ”alla sidvisningen”.</span><span class="sxs-lookup"><span data-stu-id="3b01f-153">In hello "Who used" dropdown, select "Any Page View".</span></span>
3. <span data-ttu-id="3b01f-154">Välj en egenskap genom vilka toosplit sidan Visa telemetri i hello ”delning av” listrutan.</span><span class="sxs-lookup"><span data-stu-id="3b01f-154">In hello "Split by" dropdown, select a property by which toosplit your page view telemetry.</span></span>

## <a name="retention---how-many-users-come-back"></a><span data-ttu-id="3b01f-155">Innehållen – hur många användare försöka igen?</span><span class="sxs-lookup"><span data-stu-id="3b01f-155">Retention - how many users come back?</span></span>

<span data-ttu-id="3b01f-156">Kvarhållning hjälper dig att förstå hur ofta returnera användarna toouse appen, baserat på kohorter av användare som utförde vissa företag åtgärden under en viss tidsperiod.</span><span class="sxs-lookup"><span data-stu-id="3b01f-156">Retention helps you understand how often your users return toouse their app, based on cohorts of users that performed some business action during a certain time bucket.</span></span> 

- <span data-ttu-id="3b01f-157">Förstå vilka specifika funktioner orsaka användare toocome tillbaka mer än andra</span><span class="sxs-lookup"><span data-stu-id="3b01f-157">Understand what specific features cause users toocome back more than others</span></span> 
- <span data-ttu-id="3b01f-158">Formuläret hypoteser baserat på verkliga användardata</span><span class="sxs-lookup"><span data-stu-id="3b01f-158">Form hypotheses based on real user data</span></span> 
- <span data-ttu-id="3b01f-159">Avgöra om kvarhållning är ett problem i produkten</span><span class="sxs-lookup"><span data-stu-id="3b01f-159">Determine whether retention is a problem in your product</span></span> 

![Kvarhållning](./media/app-insights-usage-overview/retention.png) 

<span data-ttu-id="3b01f-161">hello kvarhållning kontroller överst kan du toodefine specifika händelser och tid intervallet toocalculate lagring.</span><span class="sxs-lookup"><span data-stu-id="3b01f-161">hello retention controls on top allow you toodefine specific events and time range toocalculate retention.</span></span> <span data-ttu-id="3b01f-162">hello diagrammet hello mitten ger en bild av hello övergripande kvarhållning procentandel av hello tidsintervall har angetts.</span><span class="sxs-lookup"><span data-stu-id="3b01f-162">hello graph in hello middle gives a visual representation of hello overall retention percentage by hello time range specified.</span></span> <span data-ttu-id="3b01f-163">hello diagram på hello nedre representerar enskilda kvarhållning i en viss tidsperiod.</span><span class="sxs-lookup"><span data-stu-id="3b01f-163">hello graph on hello bottom represents individual retention in a given time period.</span></span> <span data-ttu-id="3b01f-164">Den här detaljnivån kan du toounderstand vad användarna gör och vad kan påverka returnerar användare på en mer detaljerad granularitet.</span><span class="sxs-lookup"><span data-stu-id="3b01f-164">This level of detail allows you toounderstand what your users are doing and what might affect returning users on a more detailed granularity.</span></span>  

[<span data-ttu-id="3b01f-165">Mer om hello kvarhållning verktyget</span><span class="sxs-lookup"><span data-stu-id="3b01f-165">More about hello Retention tool</span></span>](app-insights-usage-retention.md)

## <a name="custom-business-events"></a><span data-ttu-id="3b01f-166">Anpassade affärshändelser</span><span class="sxs-lookup"><span data-stu-id="3b01f-166">Custom business events</span></span>

<span data-ttu-id="3b01f-167">tooget förstår vad användare göra med ditt webbprogram, är det användbart tooinsert rader med kod toolog anpassade händelser.</span><span class="sxs-lookup"><span data-stu-id="3b01f-167">tooget a clear understanding of what users do with your web app, it's useful tooinsert lines of code toolog custom events.</span></span> <span data-ttu-id="3b01f-168">Dessa händelser kan spåra allt från detaljerad användaråtgärder, till exempel klicka på specifika knappar, toomore betydande affärshändelser, till exempel inköp eller vinna spel.</span><span class="sxs-lookup"><span data-stu-id="3b01f-168">These events can track anything from detailed user actions such as clicking specific buttons, toomore significant business events such as making a purchase or winning a game.</span></span> 

<span data-ttu-id="3b01f-169">Men i vissa fall sidvisningar representerar användbar händelser, det är inte sant i allmänhet.</span><span class="sxs-lookup"><span data-stu-id="3b01f-169">Although in some cases, page views can represent useful events, it isn't true in general.</span></span> <span data-ttu-id="3b01f-170">En användare kan öppna en produktsidan utan att köpa hello produkten.</span><span class="sxs-lookup"><span data-stu-id="3b01f-170">A user can open a product page without buying hello product.</span></span> 

<span data-ttu-id="3b01f-171">Med specifika händelser, kan du skapa diagram användarnas pågår via webbplatsen.</span><span class="sxs-lookup"><span data-stu-id="3b01f-171">With specific business events, you can chart your users' progress through your site.</span></span> <span data-ttu-id="3b01f-172">Du kan ta reda på inställningar för olika alternativ, och där de släppa ut eller har problem.</span><span class="sxs-lookup"><span data-stu-id="3b01f-172">You can find out their preferences for different options, and where they drop out or have difficulties.</span></span> <span data-ttu-id="3b01f-173">Med denna kunskap kan fatta du välgrundade beslut om hello prioriteter i eftersläpning för utveckling.</span><span class="sxs-lookup"><span data-stu-id="3b01f-173">With this knowledge, you can make informed decisions about hello priorities in your development backlog.</span></span>

<span data-ttu-id="3b01f-174">Händelser kan loggas in webbsidan hello:</span><span class="sxs-lookup"><span data-stu-id="3b01f-174">Events can be logged in hello web page:</span></span>

```JavaScript

    appInsights.trackEvent("ExpandDetailTab", {DetailTab: tabName});
```

<span data-ttu-id="3b01f-175">Eller i hello på serversidan av hello-webbprogram:</span><span class="sxs-lookup"><span data-stu-id="3b01f-175">Or in hello server side of hello web app:</span></span>

```C#
    var tc = new Microsoft.ApplicationInsights.TelemetryClient();
    tc.TrackEvent("CreatedAccount", new Dictionary<string,string> {"AccountType":account.Type}, null);
    ...
    tc.TrackEvent("AddedItemToCart", new Dictionary<string,string> {"Item":item.Name}, null);
    ...
    tc.TrackEvent("CompletedPurchase");
```

<span data-ttu-id="3b01f-176">Du kan koppla egenskapen värden toothese händelser så att du kan filtrera eller dela upp hello händelser när du inspektera dem i hello-portalen.</span><span class="sxs-lookup"><span data-stu-id="3b01f-176">You can attach property values toothese events, so that you can filter or split hello events when you inspect them in hello portal.</span></span> <span data-ttu-id="3b01f-177">Dessutom är en standarduppsättning med egenskaper bifogade tooeach händelse, till exempel anonyma användar-ID, vilket gör att du tootrace hello sekvensen av aktiviteter för en enskild användare.</span><span class="sxs-lookup"><span data-stu-id="3b01f-177">In addition, a standard set of properties is attached tooeach event, such as anonymous user ID, which allows you tootrace hello sequence of activities of an individual user.</span></span>

<span data-ttu-id="3b01f-178">Lär dig mer om [anpassade händelser](app-insights-api-custom-events-metrics.md#trackevent) och [egenskaper](app-insights-api-custom-events-metrics.md#properties).</span><span class="sxs-lookup"><span data-stu-id="3b01f-178">Learn more about [custom events](app-insights-api-custom-events-metrics.md#trackevent) and [properties](app-insights-api-custom-events-metrics.md#properties).</span></span>

### <a name="slice-and-dice-events"></a><span data-ttu-id="3b01f-179">Rapportanvändarna händelser</span><span class="sxs-lookup"><span data-stu-id="3b01f-179">Slice and dice events</span></span>

<span data-ttu-id="3b01f-180">I hello användare, sessioner och händelser verktyg du segmentera och undersök anpassade händelser efter användarnamn, händelse och egenskaper.</span><span class="sxs-lookup"><span data-stu-id="3b01f-180">In hello Users, Sessions, and Events tools, you can slice and dice custom events by user, event name, and properties.</span></span>
<span data-ttu-id="3b01f-181">![Användare](./media/app-insights-usage-overview/users.png)</span><span class="sxs-lookup"><span data-stu-id="3b01f-181">![Users](./media/app-insights-usage-overview/users.png)</span></span>  
  
## <a name="design-hello-telemetry-with-hello-app"></a><span data-ttu-id="3b01f-182">Design hello telemetri med hello app</span><span class="sxs-lookup"><span data-stu-id="3b01f-182">Design hello telemetry with hello app</span></span>

<span data-ttu-id="3b01f-183">När du utformar varje funktion i din app kan du överväga hur du kommer toomeasure dess framgång med dina användare.</span><span class="sxs-lookup"><span data-stu-id="3b01f-183">When you are designing each feature of your app, consider how you are going toomeasure its success with your users.</span></span> <span data-ttu-id="3b01f-184">Bestäm vad du behöver toorecord och code hello spåra anrop för dessa händelser i din app från hello affärshändelser start.</span><span class="sxs-lookup"><span data-stu-id="3b01f-184">Decide what business events you need toorecord, and code hello tracking calls for those events into your app from hello start.</span></span>

## <a name="a--b-testing"></a><span data-ttu-id="3b01f-185">EN | B-testning</span><span class="sxs-lookup"><span data-stu-id="3b01f-185">A | B Testing</span></span>
<span data-ttu-id="3b01f-186">Om du inte vet vilken variant av en funktion blir bättre, släpp båda, gör varje tillgänglig toodifferent användare.</span><span class="sxs-lookup"><span data-stu-id="3b01f-186">If you don't know which variant of a feature will be more successful, release both of them, making each accessible toodifferent users.</span></span> <span data-ttu-id="3b01f-187">Mäta hello framgångsrik varje och flytta tooa enhetlig version.</span><span class="sxs-lookup"><span data-stu-id="3b01f-187">Measure hello success of each, and then move tooa unified version.</span></span>

<span data-ttu-id="3b01f-188">För den här tekniken kan du bifoga distinkta egenskapen värden tooall hello telemetri som skickas av varje version av appen.</span><span class="sxs-lookup"><span data-stu-id="3b01f-188">For this technique, you attach distinct property values tooall hello telemetry that is sent by each version of your app.</span></span> <span data-ttu-id="3b01f-189">Du kan göra det genom att definiera egenskaperna i hello active TelemetryContext.</span><span class="sxs-lookup"><span data-stu-id="3b01f-189">You can do that by defining properties in hello active TelemetryContext.</span></span> <span data-ttu-id="3b01f-190">Dessa standardegenskaperna läggs tooevery telemetri som programmet hello postmeddelandet – inte bara din egna meddelanden, men hello samt standard telemetri.</span><span class="sxs-lookup"><span data-stu-id="3b01f-190">These default properties are added tooevery telemetry message that hello application sends - not just your custom messages, but hello standard telemetry as well.</span></span>

<span data-ttu-id="3b01f-191">Filtrera i hello Application Insights-portalen och dela dina data på hello egenskapsvärden som toocompare hello olika versioner.</span><span class="sxs-lookup"><span data-stu-id="3b01f-191">In hello Application Insights portal, filter and split your data on hello property values, so as toocompare hello different versions.</span></span>

<span data-ttu-id="3b01f-192">toodo, [ställa in en telemetri initieraren](app-insights-api-filtering-sampling.md##add-properties-itelemetryinitializer):</span><span class="sxs-lookup"><span data-stu-id="3b01f-192">toodo this, [set up a telemetry initializer](app-insights-api-filtering-sampling.md##add-properties-itelemetryinitializer):</span></span>

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

<span data-ttu-id="3b01f-193">I hello web app initieraren, till exempel Global.asax.cs:</span><span class="sxs-lookup"><span data-stu-id="3b01f-193">In hello web app initializer such as Global.asax.cs:</span></span>

```C#

    protected void Application_Start()
    {
        // ...
        TelemetryConfiguration.Active.TelemetryInitializers
        .Add(new MyTelemetryInitializer());
    }
```

<span data-ttu-id="3b01f-194">Alla nya TelemetryClients lägger automatiskt till hello-egenskapsvärde som du anger.</span><span class="sxs-lookup"><span data-stu-id="3b01f-194">All new TelemetryClients automatically add hello property value you specify.</span></span> <span data-ttu-id="3b01f-195">Enskilda telemetriska händelser kan åsidosätta hello standardvärden.</span><span class="sxs-lookup"><span data-stu-id="3b01f-195">Individual telemetry events can override hello default values.</span></span>

## <a name="next-steps"></a><span data-ttu-id="3b01f-196">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="3b01f-196">Next steps</span></span>
   - [<span data-ttu-id="3b01f-197">Användare, sessioner, händelser</span><span class="sxs-lookup"><span data-stu-id="3b01f-197">Users, Sessions, Events</span></span>](app-insights-usage-segmentation.md)
   - [<span data-ttu-id="3b01f-198">Trattar</span><span class="sxs-lookup"><span data-stu-id="3b01f-198">Funnels</span></span>](usage-funnels.md)
   - [<span data-ttu-id="3b01f-199">Kvarhållning</span><span class="sxs-lookup"><span data-stu-id="3b01f-199">Retention</span></span>](app-insights-usage-retention.md)
   - [<span data-ttu-id="3b01f-200">Användarflöden</span><span class="sxs-lookup"><span data-stu-id="3b01f-200">User Flows</span></span>](app-insights-usage-flows.md)
   - [<span data-ttu-id="3b01f-201">Arbetsböcker</span><span class="sxs-lookup"><span data-stu-id="3b01f-201">Workbooks</span></span>](app-insights-usage-workbooks.md)
   - [<span data-ttu-id="3b01f-202">Lägg till användarkontext</span><span class="sxs-lookup"><span data-stu-id="3b01f-202">Add user context</span></span>](app-insights-usage-send-user-context.md)
