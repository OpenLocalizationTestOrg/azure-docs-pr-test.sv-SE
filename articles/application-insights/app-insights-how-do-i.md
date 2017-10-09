---
title: "aaaHow gör jag... i Azure Application Insights | Microsoft Docs"
description: "Vanliga frågor och svar i Application Insights."
services: application-insights
documentationcenter: 
author: CFreemanwa
manager: carmonm
ms.assetid: 48b2b644-92e4-44c3-bc14-068f1bbedd22
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 04/04/2017
ms.author: bwren
ms.openlocfilehash: 89294c3583b7c4e7998143be6d359f2deb3c8f49
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="how-do-i--in-application-insights"></a><span data-ttu-id="dcacc-103">Hur kan jag ... i Application Insights?</span><span class="sxs-lookup"><span data-stu-id="dcacc-103">How do I ... in Application Insights?</span></span>
## <a name="get-an-email-when-"></a><span data-ttu-id="dcacc-104">Få ett e-postmeddelande när...</span><span class="sxs-lookup"><span data-stu-id="dcacc-104">Get an email when ...</span></span>
### <a name="email-if-my-site-goes-down"></a><span data-ttu-id="dcacc-105">E-post om min plats kraschar</span><span class="sxs-lookup"><span data-stu-id="dcacc-105">Email if my site goes down</span></span>
<span data-ttu-id="dcacc-106">Ange en [tillgänglighet webbtest](app-insights-monitor-web-app-availability.md).</span><span class="sxs-lookup"><span data-stu-id="dcacc-106">Set an [availability web test](app-insights-monitor-web-app-availability.md).</span></span>

### <a name="email-if-my-site-is-overloaded"></a><span data-ttu-id="dcacc-107">E-post om min plats är överbelastad.</span><span class="sxs-lookup"><span data-stu-id="dcacc-107">Email if my site is overloaded</span></span>
<span data-ttu-id="dcacc-108">Ange en [avisering](app-insights-alerts.md) på **serversvarstid**.</span><span class="sxs-lookup"><span data-stu-id="dcacc-108">Set an [alert](app-insights-alerts.md) on **Server response time**.</span></span> <span data-ttu-id="dcacc-109">Ett tröskelvärde mellan 1 och 2 sekunder ska fungera.</span><span class="sxs-lookup"><span data-stu-id="dcacc-109">A threshold between 1 and 2 seconds should work.</span></span>

![](./media/app-insights-how-do-i/030-server.png)

<span data-ttu-id="dcacc-110">Din app kan också visa loggar belastning genom att returnera felkoder.</span><span class="sxs-lookup"><span data-stu-id="dcacc-110">Your app might also show signs of strain by returning failure codes.</span></span> <span data-ttu-id="dcacc-111">Ange en varning på **misslyckade begäranden**.</span><span class="sxs-lookup"><span data-stu-id="dcacc-111">Set an alert on **Failed requests**.</span></span>

<span data-ttu-id="dcacc-112">Om du vill tooset en avisering i **undantag**, du måste kanske toodo [vissa ytterligare inställningar](app-insights-asp-net-exceptions.md) i ordning toosee data.</span><span class="sxs-lookup"><span data-stu-id="dcacc-112">If you want tooset an alert on **Server exceptions**, you might have toodo [some additional setup](app-insights-asp-net-exceptions.md) in order toosee data.</span></span>

### <a name="email-on-exceptions"></a><span data-ttu-id="dcacc-113">E-post på undantag</span><span class="sxs-lookup"><span data-stu-id="dcacc-113">Email on exceptions</span></span>
1. [<span data-ttu-id="dcacc-114">Konfigurera undantagsövervakning</span><span class="sxs-lookup"><span data-stu-id="dcacc-114">Set up exception monitoring</span></span>](app-insights-asp-net-exceptions.md)
2. <span data-ttu-id="dcacc-115">[Ställ in en varning](app-insights-alerts.md) räkna mått på hello undantag</span><span class="sxs-lookup"><span data-stu-id="dcacc-115">[Set an alert](app-insights-alerts.md) on hello Exception count metric</span></span>

### <a name="email-on-an-event-in-my-app"></a><span data-ttu-id="dcacc-116">E-post på en händelse i min app</span><span class="sxs-lookup"><span data-stu-id="dcacc-116">Email on an event in my app</span></span>
<span data-ttu-id="dcacc-117">Anta att du vill att tooget ett e-postmeddelande när en viss händelse inträffar.</span><span class="sxs-lookup"><span data-stu-id="dcacc-117">Let's suppose you'd like tooget an email when a specific event occurs.</span></span> <span data-ttu-id="dcacc-118">Application Insights inte ger den här funktionen direkt, men den kan [skicka en avisering när en måttet överskrider ett tröskelvärde](app-insights-alerts.md).</span><span class="sxs-lookup"><span data-stu-id="dcacc-118">Application Insights doesn't provide this facility directly, but it can [send an alert when a metric crosses a threshold](app-insights-alerts.md).</span></span>

<span data-ttu-id="dcacc-119">Aviseringar kan ställas in på [anpassade mått](app-insights-api-custom-events-metrics.md#trackmetric), men inte anpassade händelser.</span><span class="sxs-lookup"><span data-stu-id="dcacc-119">Alerts can be set on [custom metrics](app-insights-api-custom-events-metrics.md#trackmetric), though not custom events.</span></span> <span data-ttu-id="dcacc-120">Skriva vissa kod tooincrease ett mått när hello händelse inträffar:</span><span class="sxs-lookup"><span data-stu-id="dcacc-120">Write some code tooincrease a metric when hello event occurs:</span></span>

    telemetry.TrackMetric("Alarm", 10);

<span data-ttu-id="dcacc-121">Eller:</span><span class="sxs-lookup"><span data-stu-id="dcacc-121">or:</span></span>

    var measurements = new Dictionary<string,double>();
    measurements ["Alarm"] = 10;
    telemetry.TrackEvent("status", null, measurements);

<span data-ttu-id="dcacc-122">Eftersom aviseringar har två tillstånd, har du toosend ett lågt värde när du funderar på hello avisering toohave avslutades:</span><span class="sxs-lookup"><span data-stu-id="dcacc-122">Because alerts have two states, you have toosend a low value when you consider hello alert toohave ended:</span></span>

    telemetry.TrackMetric("Alarm", 0.5);

<span data-ttu-id="dcacc-123">Skapa ett diagram i [mått explorer](app-insights-metrics-explorer.md) toosee din larm:</span><span class="sxs-lookup"><span data-stu-id="dcacc-123">Create a chart in [metric explorer](app-insights-metrics-explorer.md) toosee your alarm:</span></span>

![](./media/app-insights-how-do-i/010-alarm.png)

<span data-ttu-id="dcacc-124">Ange en avisering toofire om hello mått går över ett mid värde under en kort period:</span><span class="sxs-lookup"><span data-stu-id="dcacc-124">Now set an alert toofire when hello metric goes above a mid value for a short period:</span></span>

![](./media/app-insights-how-do-i/020-threshold.png)

<span data-ttu-id="dcacc-125">Ange hello medelvärdet period toohello minimum.</span><span class="sxs-lookup"><span data-stu-id="dcacc-125">Set hello averaging period toohello minimum.</span></span>

<span data-ttu-id="dcacc-126">Du får e-postmeddelanden när hello mått går över såväl under hello tröskelvärde.</span><span class="sxs-lookup"><span data-stu-id="dcacc-126">You'll get emails both when hello metric goes above and below hello threshold.</span></span>

<span data-ttu-id="dcacc-127">Vissa punkter tooconsider:</span><span class="sxs-lookup"><span data-stu-id="dcacc-127">Some points tooconsider:</span></span>

* <span data-ttu-id="dcacc-128">En avisering har två lägen (”varning” och ”felfri”).</span><span class="sxs-lookup"><span data-stu-id="dcacc-128">An alert has two states ("alert" and "healthy").</span></span> <span data-ttu-id="dcacc-129">hello tillstånd utvärderas bara när ett mått tas emot.</span><span class="sxs-lookup"><span data-stu-id="dcacc-129">hello state is evaluated only when a metric is received.</span></span>
* <span data-ttu-id="dcacc-130">Ett e-postmeddelande skickas endast när hello tillstånd ändras.</span><span class="sxs-lookup"><span data-stu-id="dcacc-130">An email is sent only when hello state changes.</span></span> <span data-ttu-id="dcacc-131">Det är därför du har toosend mått för hög och låg-värde.</span><span class="sxs-lookup"><span data-stu-id="dcacc-131">This is why you have toosend both high and low-value metrics.</span></span>
* <span data-ttu-id="dcacc-132">tooevaluate hello aviseringen hello medelvärde tas emot hello värden över hello föregående period.</span><span class="sxs-lookup"><span data-stu-id="dcacc-132">tooevaluate hello alert, hello average is taken of hello received values over hello preceding period.</span></span> <span data-ttu-id="dcacc-133">Detta sker varje gång ett mått tas emot, så e-postmeddelanden skickas oftare än hello angivna perioden.</span><span class="sxs-lookup"><span data-stu-id="dcacc-133">This occurs every time a metric is received, so emails can be sent more frequently than hello period you set.</span></span>
* <span data-ttu-id="dcacc-134">Eftersom e-postmeddelanden skickas både ”varning” och ”felfri”, kanske du vill tooconsider nytt tänker din one-shot händelse som ett villkor för två tillstånd.</span><span class="sxs-lookup"><span data-stu-id="dcacc-134">Since emails are sent both on "alert" and "healthy", you might want tooconsider re-thinking your one-shot event as a two-state condition.</span></span> <span data-ttu-id="dcacc-135">Till exempel i stället för en ”jobbet är slutfört”-händelsen har en ”jobb pågår” villkor, där du får e-post i hello början och slutet av ett jobb.</span><span class="sxs-lookup"><span data-stu-id="dcacc-135">For example, instead of a "job completed" event, have a "job in progress" condition, where you get emails at hello start and end of a job.</span></span>

### <a name="set-up-alerts-automatically"></a><span data-ttu-id="dcacc-136">Ställa in aviseringar automatiskt</span><span class="sxs-lookup"><span data-stu-id="dcacc-136">Set up alerts automatically</span></span>
[<span data-ttu-id="dcacc-137">Använd PowerShell toocreate nya aviseringar</span><span class="sxs-lookup"><span data-stu-id="dcacc-137">Use PowerShell toocreate new alerts</span></span>](app-insights-alerts.md#automation)

## <a name="use-powershell-toomanage-application-insights"></a><span data-ttu-id="dcacc-138">Använd PowerShell tooManage Application Insights</span><span class="sxs-lookup"><span data-stu-id="dcacc-138">Use PowerShell tooManage Application Insights</span></span>
* [<span data-ttu-id="dcacc-139">Skapa nya resurser</span><span class="sxs-lookup"><span data-stu-id="dcacc-139">Create new resources</span></span>](app-insights-powershell-script-create-resource.md)
* [<span data-ttu-id="dcacc-140">Skapa nya aviseringar</span><span class="sxs-lookup"><span data-stu-id="dcacc-140">Create new alerts</span></span>](app-insights-alerts.md#automation)

## <a name="separate-telemetry-from-different-versions"></a><span data-ttu-id="dcacc-141">Separata telemetri från olika versioner</span><span class="sxs-lookup"><span data-stu-id="dcacc-141">Separate telemetry from different versions</span></span>

* <span data-ttu-id="dcacc-142">Flera roller i en app: använda en enda Application Insights-resurs och filtrera på cloud_Rolename.</span><span class="sxs-lookup"><span data-stu-id="dcacc-142">Multiple roles in an app: Use a single Application Insights resource, and filter on cloud_Rolename.</span></span> [<span data-ttu-id="dcacc-143">Läs mer</span><span class="sxs-lookup"><span data-stu-id="dcacc-143">Learn more</span></span>](app-insights-monitor-multi-role-apps.md)
* <span data-ttu-id="dcacc-144">Avgränsa utveckling och test-versioner: olika Application Insights-resurser.</span><span class="sxs-lookup"><span data-stu-id="dcacc-144">Separating development, test, and release versions: Use different Application Insights resources.</span></span> <span data-ttu-id="dcacc-145">Hämta hello instrumentation nycklar från web.config. [Läs mer](app-insights-separate-resources.md)</span><span class="sxs-lookup"><span data-stu-id="dcacc-145">Pick up hello instrumentation keys from web.config. [Learn more](app-insights-separate-resources.md)</span></span>
* <span data-ttu-id="dcacc-146">Rapportering skapa versioner: lägga till en egenskap med ett telemetri initieraren.</span><span class="sxs-lookup"><span data-stu-id="dcacc-146">Reporting build versions: Add a property using a telemetry initializer.</span></span> [<span data-ttu-id="dcacc-147">Läs mer</span><span class="sxs-lookup"><span data-stu-id="dcacc-147">Learn more</span></span>](app-insights-separate-resources.md)

## <a name="monitor-backend-servers-and-desktop-apps"></a><span data-ttu-id="dcacc-148">Övervaka backend-servrar och -program</span><span class="sxs-lookup"><span data-stu-id="dcacc-148">Monitor backend servers and desktop apps</span></span>
<span data-ttu-id="dcacc-149">[Använd hello Windows Server SDK modulen](app-insights-windows-desktop.md).</span><span class="sxs-lookup"><span data-stu-id="dcacc-149">[Use hello Windows Server SDK module](app-insights-windows-desktop.md).</span></span>

## <a name="visualize-data"></a><span data-ttu-id="dcacc-150">Visualisera data</span><span class="sxs-lookup"><span data-stu-id="dcacc-150">Visualize data</span></span>
#### <a name="dashboard-with-metrics-from-multiple-apps"></a><span data-ttu-id="dcacc-151">Instrumentpanel med mått för flera appar</span><span class="sxs-lookup"><span data-stu-id="dcacc-151">Dashboard with metrics from multiple apps</span></span>
* <span data-ttu-id="dcacc-152">I [mått Explorer](app-insights-metrics-explorer.md), anpassa ditt diagram och spara den som en favorit.</span><span class="sxs-lookup"><span data-stu-id="dcacc-152">In [Metric Explorer](app-insights-metrics-explorer.md), customize your chart and save it as a favorite.</span></span> <span data-ttu-id="dcacc-153">Fäst den toohello Azure instrumentpanelen.</span><span class="sxs-lookup"><span data-stu-id="dcacc-153">Pin it toohello Azure dashboard.</span></span>

#### <a name="dashboard-with-data-from-other-sources-and-application-insights"></a><span data-ttu-id="dcacc-154">Instrumentpanel med data från andra källor och Application Insights</span><span class="sxs-lookup"><span data-stu-id="dcacc-154">Dashboard with data from other sources and Application Insights</span></span>
* <span data-ttu-id="dcacc-155">[Exportera telemetri tooPower BI](app-insights-export-power-bi.md).</span><span class="sxs-lookup"><span data-stu-id="dcacc-155">[Export telemetry tooPower BI](app-insights-export-power-bi.md).</span></span>

<span data-ttu-id="dcacc-156">Eller</span><span class="sxs-lookup"><span data-stu-id="dcacc-156">Or</span></span>

* <span data-ttu-id="dcacc-157">Använda SharePoint som instrumentpanelen visar data i SharePoint-webbdelar.</span><span class="sxs-lookup"><span data-stu-id="dcacc-157">Use SharePoint as your dashboard, displaying data in SharePoint web parts.</span></span> <span data-ttu-id="dcacc-158">[Använd den löpande exporten och Stream Analytics tooexport tooSQL](app-insights-code-sample-export-sql-stream-analytics.md).</span><span class="sxs-lookup"><span data-stu-id="dcacc-158">[Use continuous export and Stream Analytics tooexport tooSQL](app-insights-code-sample-export-sql-stream-analytics.md).</span></span>  <span data-ttu-id="dcacc-159">Använd PowerView tooexamine hello databas och skapa en SharePoint-webbdel för PowerView.</span><span class="sxs-lookup"><span data-stu-id="dcacc-159">Use PowerView tooexamine hello database, and create a SharePoint web part for PowerView.</span></span>

<a name="search-specific-users"></a>

### <a name="filter-out-anonymous-or-authenticated-users"></a><span data-ttu-id="dcacc-160">Filtrera ut anonyma och autentiserade användare</span><span class="sxs-lookup"><span data-stu-id="dcacc-160">Filter out anonymous or authenticated users</span></span>
<span data-ttu-id="dcacc-161">Om dina användare loggar in, kan du ange hello [autentiserat användar-id](app-insights-api-custom-events-metrics.md#authenticated-users). (Det inte sker automatiskt.)</span><span class="sxs-lookup"><span data-stu-id="dcacc-161">If your users sign in, you can set hello [authenticated user id](app-insights-api-custom-events-metrics.md#authenticated-users). (It doesn't happen automatically.)</span></span>

<span data-ttu-id="dcacc-162">Sedan kan du:</span><span class="sxs-lookup"><span data-stu-id="dcacc-162">You can then:</span></span>

* <span data-ttu-id="dcacc-163">Söka på särskilda användar-ID</span><span class="sxs-lookup"><span data-stu-id="dcacc-163">Search on specific user ids</span></span>

![](./media/app-insights-how-do-i/110-search.png)

* <span data-ttu-id="dcacc-164">Filtrera mått tooeither anonyma och autentiserade användare</span><span class="sxs-lookup"><span data-stu-id="dcacc-164">Filter metrics tooeither anonymous or authenticated users</span></span>

![](./media/app-insights-how-do-i/115-metrics.png)

## <a name="modify-property-names-or-values"></a><span data-ttu-id="dcacc-165">Ändra egenskapsnamn eller värden</span><span class="sxs-lookup"><span data-stu-id="dcacc-165">Modify property names or values</span></span>
<span data-ttu-id="dcacc-166">Skapa en [filter](app-insights-api-filtering-sampling.md#filtering).</span><span class="sxs-lookup"><span data-stu-id="dcacc-166">Create a [filter](app-insights-api-filtering-sampling.md#filtering).</span></span> <span data-ttu-id="dcacc-167">På så sätt kan du ändra eller filtrera telemetri innan det skickas från din app tooApplication insikter.</span><span class="sxs-lookup"><span data-stu-id="dcacc-167">This lets you modify or filter telemetry before it is sent from your app tooApplication Insights.</span></span>

## <a name="list-specific-users-and-their-usage"></a><span data-ttu-id="dcacc-168">Lista över specifika användare och deras användning</span><span class="sxs-lookup"><span data-stu-id="dcacc-168">List specific users and their usage</span></span>
<span data-ttu-id="dcacc-169">Om du bara vill för[Sök efter specifika användare](#search-specific-users), du kan ange hello [autentiserat användar-id](app-insights-api-custom-events-metrics.md#authenticated-users).</span><span class="sxs-lookup"><span data-stu-id="dcacc-169">If you just want too[search for specific users](#search-specific-users), you can set hello [authenticated user id](app-insights-api-custom-events-metrics.md#authenticated-users).</span></span>

<span data-ttu-id="dcacc-170">Om du vill ha en lista över användare med data, till exempel vilka sidor du de titta på eller hur ofta de loggar in, har du två alternativ:</span><span class="sxs-lookup"><span data-stu-id="dcacc-170">If you want a list of users with data such as what pages they look at or how often they log in, you have two options:</span></span>

* <span data-ttu-id="dcacc-171">[Ange autentiserat användar-id](app-insights-api-custom-events-metrics.md#authenticated-users), [exportera tooa databasen](app-insights-code-sample-export-sql-stream-analytics.md) och Använd lämpliga verktyg tooanalyze dina användardata.</span><span class="sxs-lookup"><span data-stu-id="dcacc-171">[Set authenticated user id](app-insights-api-custom-events-metrics.md#authenticated-users), [export tooa database](app-insights-code-sample-export-sql-stream-analytics.md) and use suitable tools tooanalyze your user data there.</span></span>
* <span data-ttu-id="dcacc-172">Om du har mindre antal användare kan skicka anpassade händelser eller mått, med hello data av intresse som hello värde eller händelsenamnet och inställningen hello användar-id som en egenskap.</span><span class="sxs-lookup"><span data-stu-id="dcacc-172">If you have only a small number of users, send custom events or metrics, using hello data of interest as hello metric value or event name, and setting hello user id as a property.</span></span> <span data-ttu-id="dcacc-173">tooanalyze sidvisningar Ersätt hello standard JavaScript trackPageView anrop.</span><span class="sxs-lookup"><span data-stu-id="dcacc-173">tooanalyze page views, replace hello standard JavaScript trackPageView call.</span></span> <span data-ttu-id="dcacc-174">tooanalyze serversidan telemetri använder en telemetri initieraren tooadd hello användar-id tooall server telemetri.</span><span class="sxs-lookup"><span data-stu-id="dcacc-174">tooanalyze server-side telemetry, use a telemetry initializer tooadd hello user id tooall server telemetry.</span></span> <span data-ttu-id="dcacc-175">Därefter kan du filtrera och segment mått och sökningar på hello användar-id.</span><span class="sxs-lookup"><span data-stu-id="dcacc-175">You can then filter and segment metrics and searches on hello user id.</span></span>

## <a name="reduce-traffic-from-my-app-tooapplication-insights"></a><span data-ttu-id="dcacc-176">Minska trafiken från min app tooApplication insikter</span><span class="sxs-lookup"><span data-stu-id="dcacc-176">Reduce traffic from my app tooApplication Insights</span></span>
* <span data-ttu-id="dcacc-177">I [ApplicationInsights.config](app-insights-configuration-with-applicationinsights-config.md), inaktivera alla moduler som du inte behöver dessa hello prestandaräknaren insamlaren.</span><span class="sxs-lookup"><span data-stu-id="dcacc-177">In [ApplicationInsights.config](app-insights-configuration-with-applicationinsights-config.md), disable any modules you don't need, such hello performance counter collector.</span></span>
* <span data-ttu-id="dcacc-178">Använd [provtagning och filtrering](app-insights-api-filtering-sampling.md) på hello SDK.</span><span class="sxs-lookup"><span data-stu-id="dcacc-178">Use [Sampling and filtering](app-insights-api-filtering-sampling.md) at hello SDK.</span></span>
* <span data-ttu-id="dcacc-179">Begränsa hello antalet Ajax-anrop som rapporterats för alla sidor i dina webbsidor.</span><span class="sxs-lookup"><span data-stu-id="dcacc-179">In your web pages, Limit hello number of Ajax calls reported for every page view.</span></span> <span data-ttu-id="dcacc-180">Hello skript kodutdrag efter `instrumentationKey:...` , infoga: `,maxAjaxCallsPerView:3` (eller ett lämpligt antal).</span><span class="sxs-lookup"><span data-stu-id="dcacc-180">In hello script snippet after `instrumentationKey:...` , insert: `,maxAjaxCallsPerView:3` (or a suitable number).</span></span>
* <span data-ttu-id="dcacc-181">Om du använder [TrackMetric](app-insights-api-custom-events-metrics.md#trackmetric), compute hello aggregering av batchar av måttvärden innan du skickar hello resultat.</span><span class="sxs-lookup"><span data-stu-id="dcacc-181">If you're using [TrackMetric](app-insights-api-custom-events-metrics.md#trackmetric), compute hello aggregate of batches of metric values before sending hello result.</span></span> <span data-ttu-id="dcacc-182">Det finns en överlagring av TrackMetric() som tillhandahåller för som.</span><span class="sxs-lookup"><span data-stu-id="dcacc-182">There's an overload of TrackMetric() that provides for that.</span></span>

<span data-ttu-id="dcacc-183">Lär dig mer om [priser och kvoter](app-insights-pricing.md).</span><span class="sxs-lookup"><span data-stu-id="dcacc-183">Learn more about [pricing and quotas](app-insights-pricing.md).</span></span>

## <a name="disable-telemetry"></a><span data-ttu-id="dcacc-184">Inaktivera telemetri</span><span class="sxs-lookup"><span data-stu-id="dcacc-184">Disable telemetry</span></span>
<span data-ttu-id="dcacc-185">för**dynamiskt stoppa och starta** hello insamling och vidarebefordran av telemetri från hello-servern:</span><span class="sxs-lookup"><span data-stu-id="dcacc-185">too**dynamically stop and start** hello collection and transmission of telemetry from hello server:</span></span>

```

    using  Microsoft.ApplicationInsights.Extensibility;

    TelemetryConfiguration.Active.DisableTelemetry = true;
```



<span data-ttu-id="dcacc-186">för**inaktivera valda standard insamlare** – till exempel prestandaräknare, HTTP-begäranden eller beroenden - ta bort eller kommentera ut hello relevanta rader i [ApplicationInsights.config](app-insights-api-custom-events-metrics.md). Du kan göra detta, till exempel om du vill toosend TrackRequest data.</span><span class="sxs-lookup"><span data-stu-id="dcacc-186">too**disable selected standard collectors** - for example, performance counters, HTTP requests, or dependencies - delete or comment out hello relevant lines in [ApplicationInsights.config](app-insights-api-custom-events-metrics.md). You could do this, for example, if you want toosend your own TrackRequest data.</span></span>

## <a name="view-system-performance-counters"></a><span data-ttu-id="dcacc-187">Visa prestandaräknare för system</span><span class="sxs-lookup"><span data-stu-id="dcacc-187">View system performance counters</span></span>
<span data-ttu-id="dcacc-188">Bland hello är mått som du kan visa i metrics explorer en uppsättning system prestandaräknare.</span><span class="sxs-lookup"><span data-stu-id="dcacc-188">Among hello metrics you can show in metrics explorer are a set of system performance counters.</span></span> <span data-ttu-id="dcacc-189">Det finns en fördefinierad bladet med titeln **servrar** som visar flera.</span><span class="sxs-lookup"><span data-stu-id="dcacc-189">There's a predefined blade titled **Servers** that displays several of them.</span></span>

![Öppna Application Insights-resursen och klicka på servrar](./media/app-insights-how-do-i/121-servers.png)

### <a name="if-you-see-no-performance-counter-data"></a><span data-ttu-id="dcacc-191">Om du ser inga prestandaräknardata</span><span class="sxs-lookup"><span data-stu-id="dcacc-191">If you see no performance counter data</span></span>
* <span data-ttu-id="dcacc-192">**IIS-servern** på din egen dator eller på en virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="dcacc-192">**IIS server** on your own machine or on a VM.</span></span> <span data-ttu-id="dcacc-193">[Installera statusövervakaren](app-insights-monitor-performance-live-website-now.md).</span><span class="sxs-lookup"><span data-stu-id="dcacc-193">[Install Status Monitor](app-insights-monitor-performance-live-website-now.md).</span></span>
* <span data-ttu-id="dcacc-194">**Webbplatsen för Azure** -vi stöder inte prestandaräknare ännu.</span><span class="sxs-lookup"><span data-stu-id="dcacc-194">**Azure web site** - we don't support performance counters yet.</span></span> <span data-ttu-id="dcacc-195">Det finns flera mått som du kan få som en del av hello Azure-webbplats på Kontrollpanelen.</span><span class="sxs-lookup"><span data-stu-id="dcacc-195">There are several metrics you can get as a standard part of hello Azure web site control panel.</span></span>
* <span data-ttu-id="dcacc-196">**UNIX-server** - [installera collectd](app-insights-java-collectd.md)</span><span class="sxs-lookup"><span data-stu-id="dcacc-196">**Unix server** - [Install collectd](app-insights-java-collectd.md)</span></span>

### <a name="toodisplay-more-performance-counters"></a><span data-ttu-id="dcacc-197">toodisplay mer prestandaräknare</span><span class="sxs-lookup"><span data-stu-id="dcacc-197">toodisplay more performance counters</span></span>
* <span data-ttu-id="dcacc-198">Första, [lägga till ett nytt diagram](app-insights-metrics-explorer.md) och se om hello räknare i grundläggande hello anges vi erbjuder.</span><span class="sxs-lookup"><span data-stu-id="dcacc-198">First, [add a new chart](app-insights-metrics-explorer.md) and see if hello counter is in hello basic set that we offer.</span></span>
* <span data-ttu-id="dcacc-199">Om inte, [lägga till hello toohello räknaruppsättningen samlas in av hello prestandaräknarmodulen](app-insights-performance-counters.md).</span><span class="sxs-lookup"><span data-stu-id="dcacc-199">If not, [add hello counter toohello set collected by hello performance counter module](app-insights-performance-counters.md).</span></span>
