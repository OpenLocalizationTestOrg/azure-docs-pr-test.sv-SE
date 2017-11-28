---
title: "aaaUser kvarhållning analys för webbprogram med Azure Application Insights | Microsoft docs"
description: "Hur många användare tillbaka tooyour app?"
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
ms.openlocfilehash: 8bcee5f1611afbd69016ec3eef27832c304762a4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="user-retention-analysis-for-web-applications-with-application-insights"></a><span data-ttu-id="f5397-103">Kvarhållning av Användaranalys för webbprogram med Application Insights</span><span class="sxs-lookup"><span data-stu-id="f5397-103">User retention analysis for web applications with Application Insights</span></span>

<span data-ttu-id="f5397-104">hello kvarhållning funktion i [Azure Application Insights](app-insights-overview.md) kan du analysera hur många användare returnera tooyour appen och hur ofta de utföra vissa åtgärder eller uppnå mål.</span><span class="sxs-lookup"><span data-stu-id="f5397-104">hello retention feature in [Azure Application Insights](app-insights-overview.md) helps you analyze how many users return tooyour app, and how often they perform particular tasks or achieve goals.</span></span> <span data-ttu-id="f5397-105">Om du kör en spel plats, kan du jämföra hello antal användare som returnerar toohello plats efter att förlora spel med hello nummer som returneras efter vann.</span><span class="sxs-lookup"><span data-stu-id="f5397-105">For example, if you run a game site, you could compare hello numbers of users who return toohello site after losing a game with hello number who return after winning.</span></span> <span data-ttu-id="f5397-106">Den här kunskapen kan hjälpa dig att förbättra både användarupplevelsen och din strategi.</span><span class="sxs-lookup"><span data-stu-id="f5397-106">This knowledge can help you improve both your user experience and your business strategy.</span></span>

## <a name="get-started"></a><span data-ttu-id="f5397-107">Kom igång</span><span class="sxs-lookup"><span data-stu-id="f5397-107">Get started</span></span>

<span data-ttu-id="f5397-108">Om du ännu inte ser data i hello kvarhållning verktyg i hello Application Insights-portalen, [Lär dig hur tooget igång med verktyg för användning av hello](app-insights-usage-overview.md).</span><span class="sxs-lookup"><span data-stu-id="f5397-108">If you don't yet see data in hello retention tool in hello Application Insights portal, [learn how tooget started with hello usage tools](app-insights-usage-overview.md).</span></span>

## <a name="hello-retention-tool"></a><span data-ttu-id="f5397-109">hello kvarhållning verktyget</span><span class="sxs-lookup"><span data-stu-id="f5397-109">hello Retention tool</span></span>

![Kvarhållningsverktyg](./media/app-insights-usage-retention/retention.png)

1. <span data-ttu-id="f5397-111">hello verktygsfältet kan användare toocreate nya kvarhållning rapporter, öppna den befintliga kvarhållning rapporter, spara den aktuella kvarhållning rapporten eller spara som, återställa ändringar som gjorts toosaved rapporter, uppdatera data på hello rapport, dela rapport via e-post eller Direktlänk och åtkomst hello dokumentationssida.</span><span class="sxs-lookup"><span data-stu-id="f5397-111">hello toolbar allows users toocreate new retention reports, open existing retention reports, save current retention report or save as, revert changes made toosaved reports, refresh data on hello report, share report via email or direct link, and access hello documentation page.</span></span> 
2. <span data-ttu-id="f5397-112">Som standard visas alla användare som har något Kom tillbaka sedan och uppfyllde något annat under en period kvarhållning.</span><span class="sxs-lookup"><span data-stu-id="f5397-112">By default, retention shows all users who did anything then came back and did anything else over a period.</span></span> <span data-ttu-id="f5397-113">Du kan välja en annan kombination av händelser toonarrow hello fokusera på specifika användaraktiviteter.</span><span class="sxs-lookup"><span data-stu-id="f5397-113">You can select different combination of events toonarrow hello focus on specific user activities.</span></span>
3. <span data-ttu-id="f5397-114">Lägg till ett eller flera filter på Egenskaper.</span><span class="sxs-lookup"><span data-stu-id="f5397-114">Add one or more filters on properties.</span></span> <span data-ttu-id="f5397-115">Du kan fokusera på användare i ett visst land eller region.</span><span class="sxs-lookup"><span data-stu-id="f5397-115">For example, you can focus on users in a particular country or region.</span></span> <span data-ttu-id="f5397-116">Klicka på **uppdatering** när du har angett hello filter.</span><span class="sxs-lookup"><span data-stu-id="f5397-116">Click **Update** after setting hello filters.</span></span> 
4. <span data-ttu-id="f5397-117">hello övergripande kvarhållning diagrammet visar en sammanfattning av användardokumentation över hello tidsperiod.</span><span class="sxs-lookup"><span data-stu-id="f5397-117">hello overall retention chart shows a summary of user retention across hello selected time period.</span></span> 
5. <span data-ttu-id="f5397-118">hello rutnätet visar hello antalet användare behålls bl.a toohello frågebyggaren i #2.</span><span class="sxs-lookup"><span data-stu-id="f5397-118">hello grid shows hello number of users retained according toohello query builder in #2.</span></span> <span data-ttu-id="f5397-119">Varje rad representerar en kommittén av användare som utförde alla händelser i hello tid tidsperiod visas.</span><span class="sxs-lookup"><span data-stu-id="f5397-119">Each row represents a cohort of users who performed any event in hello time period shown.</span></span> <span data-ttu-id="f5397-120">Varje cell i hello raden visar hur många av den kommittén returneras minst en gång i en senare tid.</span><span class="sxs-lookup"><span data-stu-id="f5397-120">Each cell in hello row shows how many of that cohort returned at least once in a later period.</span></span> <span data-ttu-id="f5397-121">Vissa användare kan returnera i mer än en period.</span><span class="sxs-lookup"><span data-stu-id="f5397-121">Some users may return in more than one period.</span></span> 
6. <span data-ttu-id="f5397-122">hello insikter kort Visa översta fem initierande händelser och fem returnerade händelser toogive användarna en bättre förståelse för deras kvarhållning rapporten.</span><span class="sxs-lookup"><span data-stu-id="f5397-122">hello insights cards show top five initiating events, and top five returned events toogive users a better understanding of their retention report.</span></span> 

![Kvarhållning musen hovra](./media/app-insights-usage-retention/hover.png)

<span data-ttu-id="f5397-124">Användare kan markören över celler i hello kvarhållning tooaccess hello analytics för och verktygstips som förklarar vad hello cell innebär.</span><span class="sxs-lookup"><span data-stu-id="f5397-124">Users can hover over cells on hello retention tool tooaccess hello analytics button and tool tips explaining what hello cell means.</span></span> <span data-ttu-id="f5397-125">hello tar Analytics användare toohello Analytics verktyget med en i förväg frågan toogenerate användare från hello cell.</span><span class="sxs-lookup"><span data-stu-id="f5397-125">hello Analytics button takes users toohello Analytics tool with a pre-populated query toogenerate users from hello cell.</span></span> 

## <a name="use-business-events-tootrack-retention"></a><span data-ttu-id="f5397-126">Använda business händelser tootrack kvarhållning</span><span class="sxs-lookup"><span data-stu-id="f5397-126">Use business events tootrack retention</span></span>

<span data-ttu-id="f5397-127">tooget hello mest användbara kvarhållning analys, mått händelser som representerar viktiga aktiviteter.</span><span class="sxs-lookup"><span data-stu-id="f5397-127">tooget hello most useful retention analysis, measure events that represent significant business activities.</span></span> 

<span data-ttu-id="f5397-128">Många användare kan till exempel öppna en sida i appen utan att spela upp hello-spelet visas.</span><span class="sxs-lookup"><span data-stu-id="f5397-128">For example, many users might open a page in your app without playing hello game that it displays.</span></span> <span data-ttu-id="f5397-129">Spåra bara hello sidvisningar skulle därför innehåller en felaktig beräkning av hur många personer som returnerar tooplay hello spelet efter att använda det tidigare.</span><span class="sxs-lookup"><span data-stu-id="f5397-129">Tracking just hello page views would therefore provide an inaccurate estimate of how many people return tooplay hello game after enjoying it previously.</span></span> <span data-ttu-id="f5397-130">tooget en tydlig bild av returnerar spelare appen bör skicka en anpassad händelse när en användare spelar faktiskt.</span><span class="sxs-lookup"><span data-stu-id="f5397-130">tooget a clear picture of returning players, your app should send a custom event when a user actually plays.</span></span>  

<span data-ttu-id="f5397-131">Det är god sed toocode anpassade händelser som representerar viktiga åtgärder och använda dem för kvarhållning analyser.</span><span class="sxs-lookup"><span data-stu-id="f5397-131">It's good practice toocode custom events that represent key business actions, and use these for your retention analysis.</span></span> <span data-ttu-id="f5397-132">toocapture hello spel utfall, behöver du toowrite en rad med kod toosend anpassade händelsen-tooApplication insikter.</span><span class="sxs-lookup"><span data-stu-id="f5397-132">toocapture hello game outcome, you need toowrite a line of code toosend a custom event tooApplication Insights.</span></span> <span data-ttu-id="f5397-133">Om du skriver i koden för webbsidan hello eller Node.JS, ser ut så här:</span><span class="sxs-lookup"><span data-stu-id="f5397-133">If you write it in hello web page code or in Node.JS, it looks like this:</span></span>

```JavaScript
    appinsights.trackEvent("won game");
```

<span data-ttu-id="f5397-134">Eller i serverkoden för ASP.NET:</span><span class="sxs-lookup"><span data-stu-id="f5397-134">Or in ASP.NET server code:</span></span>

```C#
   telemetry.TrackEvent("won game");
```

<span data-ttu-id="f5397-135">[Mer information om anpassade händelser skrivs](app-insights-api-custom-events-metrics.md#trackevent).</span><span class="sxs-lookup"><span data-stu-id="f5397-135">[Learn more about writing custom events](app-insights-api-custom-events-metrics.md#trackevent).</span></span>


## <a name="next-steps"></a><span data-ttu-id="f5397-136">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="f5397-136">Next steps</span></span>
- <span data-ttu-id="f5397-137">tooenable användning inträffar, börja skicka [anpassade händelser](https://docs.microsoft.com/en-us/azure/application-insights/app-insights-api-custom-events-metrics#trackevent) eller [sidvisningar](https://docs.microsoft.com/azure/application-insights/app-insights-api-custom-events-metrics#page-views).</span><span class="sxs-lookup"><span data-stu-id="f5397-137">tooenable usage experiences, start sending [custom events](https://docs.microsoft.com/en-us/azure/application-insights/app-insights-api-custom-events-metrics#trackevent) or [page views](https://docs.microsoft.com/azure/application-insights/app-insights-api-custom-events-metrics#page-views).</span></span>
- <span data-ttu-id="f5397-138">Om du redan skickar anpassade händelser eller sidvyer utforska hello användning verktyg toolearn hur användare att använda tjänsten.</span><span class="sxs-lookup"><span data-stu-id="f5397-138">If you already send custom events or page views, explore hello Usage tools toolearn how users use your service.</span></span>
    - [<span data-ttu-id="f5397-139">Användare, sessioner, händelser</span><span class="sxs-lookup"><span data-stu-id="f5397-139">Users, Sessions, Events</span></span>](app-insights-usage-segmentation.md)
    - [<span data-ttu-id="f5397-140">Trattar</span><span class="sxs-lookup"><span data-stu-id="f5397-140">Funnels</span></span>](usage-funnels.md)
    - [<span data-ttu-id="f5397-141">Användarflöden</span><span class="sxs-lookup"><span data-stu-id="f5397-141">User Flows</span></span>](app-insights-usage-flows.md)
    - [<span data-ttu-id="f5397-142">Arbetsböcker</span><span class="sxs-lookup"><span data-stu-id="f5397-142">Workbooks</span></span>](app-insights-usage-workbooks.md)
    - [<span data-ttu-id="f5397-143">Lägg till användarkontext</span><span class="sxs-lookup"><span data-stu-id="f5397-143">Add user context</span></span>](app-insights-usage-send-user-context.md)


