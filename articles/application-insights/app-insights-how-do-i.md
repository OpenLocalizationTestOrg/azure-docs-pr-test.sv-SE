---
title: "Hur gör jag... i Azure Application Insights | Microsoft Docs"
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
ms.openlocfilehash: ef63e06c0621753e0a706d6efb709b943e38ee42
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/18/2017
---
# <a name="how-do-i--in-application-insights"></a><span data-ttu-id="796d8-103">Hur kan jag ... i Application Insights?</span><span class="sxs-lookup"><span data-stu-id="796d8-103">How do I ... in Application Insights?</span></span>
## <a name="get-an-email-when-"></a><span data-ttu-id="796d8-104">Få ett e-postmeddelande när...</span><span class="sxs-lookup"><span data-stu-id="796d8-104">Get an email when ...</span></span>
### <a name="email-if-my-site-goes-down"></a><span data-ttu-id="796d8-105">E-post om min plats kraschar</span><span class="sxs-lookup"><span data-stu-id="796d8-105">Email if my site goes down</span></span>
<span data-ttu-id="796d8-106">Ange en [tillgänglighet webbtest](app-insights-monitor-web-app-availability.md).</span><span class="sxs-lookup"><span data-stu-id="796d8-106">Set an [availability web test](app-insights-monitor-web-app-availability.md).</span></span>

### <a name="email-if-my-site-is-overloaded"></a><span data-ttu-id="796d8-107">E-post om min plats är överbelastad.</span><span class="sxs-lookup"><span data-stu-id="796d8-107">Email if my site is overloaded</span></span>
<span data-ttu-id="796d8-108">Ange en [avisering](app-insights-alerts.md) på **serversvarstid**.</span><span class="sxs-lookup"><span data-stu-id="796d8-108">Set an [alert](app-insights-alerts.md) on **Server response time**.</span></span> <span data-ttu-id="796d8-109">Ett tröskelvärde mellan 1 och 2 sekunder ska fungera.</span><span class="sxs-lookup"><span data-stu-id="796d8-109">A threshold between 1 and 2 seconds should work.</span></span>

![](./media/app-insights-how-do-i/030-server.png)

<span data-ttu-id="796d8-110">Din app kan också visa loggar belastning genom att returnera felkoder.</span><span class="sxs-lookup"><span data-stu-id="796d8-110">Your app might also show signs of strain by returning failure codes.</span></span> <span data-ttu-id="796d8-111">Ange en varning på **misslyckade begäranden**.</span><span class="sxs-lookup"><span data-stu-id="796d8-111">Set an alert on **Failed requests**.</span></span>

<span data-ttu-id="796d8-112">Om du vill ange en varning på **undantag**, du kan behöva göra [vissa ytterligare inställningar](app-insights-asp-net-exceptions.md) för att se data.</span><span class="sxs-lookup"><span data-stu-id="796d8-112">If you want to set an alert on **Server exceptions**, you might have to do [some additional setup](app-insights-asp-net-exceptions.md) in order to see data.</span></span>

### <a name="email-on-exceptions"></a><span data-ttu-id="796d8-113">E-post på undantag</span><span class="sxs-lookup"><span data-stu-id="796d8-113">Email on exceptions</span></span>
1. [<span data-ttu-id="796d8-114">Konfigurera undantagsövervakning</span><span class="sxs-lookup"><span data-stu-id="796d8-114">Set up exception monitoring</span></span>](app-insights-asp-net-exceptions.md)
2. <span data-ttu-id="796d8-115">[Ställ in en varning](app-insights-alerts.md) undantaget räkna mått</span><span class="sxs-lookup"><span data-stu-id="796d8-115">[Set an alert](app-insights-alerts.md) on the Exception count metric</span></span>

### <a name="email-on-an-event-in-my-app"></a><span data-ttu-id="796d8-116">E-post på en händelse i min app</span><span class="sxs-lookup"><span data-stu-id="796d8-116">Email on an event in my app</span></span>
<span data-ttu-id="796d8-117">Anta att du vill få ett e-postmeddelande när en viss händelse inträffar.</span><span class="sxs-lookup"><span data-stu-id="796d8-117">Let's suppose you'd like to get an email when a specific event occurs.</span></span> <span data-ttu-id="796d8-118">Application Insights inte ger den här funktionen direkt, men den kan [skicka en avisering när en måttet överskrider ett tröskelvärde](app-insights-alerts.md).</span><span class="sxs-lookup"><span data-stu-id="796d8-118">Application Insights doesn't provide this facility directly, but it can [send an alert when a metric crosses a threshold](app-insights-alerts.md).</span></span>

<span data-ttu-id="796d8-119">Aviseringar kan ställas in på [anpassade mått](app-insights-api-custom-events-metrics.md#trackmetric), men inte anpassade händelser.</span><span class="sxs-lookup"><span data-stu-id="796d8-119">Alerts can be set on [custom metrics](app-insights-api-custom-events-metrics.md#trackmetric), though not custom events.</span></span> <span data-ttu-id="796d8-120">Skriva kod för att öka ett mått när händelsen inträffar:</span><span class="sxs-lookup"><span data-stu-id="796d8-120">Write some code to increase a metric when the event occurs:</span></span>

    telemetry.TrackMetric("Alarm", 10);

<span data-ttu-id="796d8-121">Eller:</span><span class="sxs-lookup"><span data-stu-id="796d8-121">or:</span></span>

    var measurements = new Dictionary<string,double>();
    measurements ["Alarm"] = 10;
    telemetry.TrackEvent("status", null, measurements);

<span data-ttu-id="796d8-122">Du måste skicka ett lågt värde när du funderar på att aviseringen ska ha avslutat eftersom aviseringar har två tillstånd:</span><span class="sxs-lookup"><span data-stu-id="796d8-122">Because alerts have two states, you have to send a low value when you consider the alert to have ended:</span></span>

    telemetry.TrackMetric("Alarm", 0.5);

<span data-ttu-id="796d8-123">Skapa ett diagram i [mått explorer](app-insights-metrics-explorer.md) att se din larm:</span><span class="sxs-lookup"><span data-stu-id="796d8-123">Create a chart in [metric explorer](app-insights-metrics-explorer.md) to see your alarm:</span></span>

![](./media/app-insights-how-do-i/010-alarm.png)

<span data-ttu-id="796d8-124">Ange en avisering eller om måttet går över ett mid värde under en kort period:</span><span class="sxs-lookup"><span data-stu-id="796d8-124">Now set an alert to fire when the metric goes above a mid value for a short period:</span></span>

![](./media/app-insights-how-do-i/020-threshold.png)

<span data-ttu-id="796d8-125">Ange ett genomsnitt perioden som minst.</span><span class="sxs-lookup"><span data-stu-id="796d8-125">Set the averaging period to the minimum.</span></span>

<span data-ttu-id="796d8-126">Du får e-postmeddelanden när måttet går över såväl under tröskeln.</span><span class="sxs-lookup"><span data-stu-id="796d8-126">You'll get emails both when the metric goes above and below the threshold.</span></span>

<span data-ttu-id="796d8-127">Vissa saker att tänka på:</span><span class="sxs-lookup"><span data-stu-id="796d8-127">Some points to consider:</span></span>

* <span data-ttu-id="796d8-128">En avisering har två lägen (”varning” och ”felfri”).</span><span class="sxs-lookup"><span data-stu-id="796d8-128">An alert has two states ("alert" and "healthy").</span></span> <span data-ttu-id="796d8-129">Tillståndet utvärderas bara när ett mått tas emot.</span><span class="sxs-lookup"><span data-stu-id="796d8-129">The state is evaluated only when a metric is received.</span></span>
* <span data-ttu-id="796d8-130">Ett e-postmeddelande skickas endast när tillståndet ändras.</span><span class="sxs-lookup"><span data-stu-id="796d8-130">An email is sent only when the state changes.</span></span> <span data-ttu-id="796d8-131">Detta är varför du behöver skicka både hög och låg-värde mått.</span><span class="sxs-lookup"><span data-stu-id="796d8-131">This is why you have to send both high and low-value metrics.</span></span>
* <span data-ttu-id="796d8-132">Om du vill utvärdera aviseringen tas medelvärdet av mottagna värden över föregående period.</span><span class="sxs-lookup"><span data-stu-id="796d8-132">To evaluate the alert, the average is taken of the received values over the preceding period.</span></span> <span data-ttu-id="796d8-133">Detta sker varje gång ett mått tas emot, så e-postmeddelanden skickas oftare än den angivna perioden.</span><span class="sxs-lookup"><span data-stu-id="796d8-133">This occurs every time a metric is received, so emails can be sent more frequently than the period you set.</span></span>
* <span data-ttu-id="796d8-134">Eftersom e-postmeddelanden skickas både ”varning” och ”felfri”, kanske du vill överväga nytt tänker din one-shot händelse som ett villkor för två tillstånd.</span><span class="sxs-lookup"><span data-stu-id="796d8-134">Since emails are sent both on "alert" and "healthy", you might want to consider re-thinking your one-shot event as a two-state condition.</span></span> <span data-ttu-id="796d8-135">Till exempel i stället för en ”jobbet har slutförts” händelsen har en ”jobb pågår” villkor, där du får e-post i början och slutet av ett jobb.</span><span class="sxs-lookup"><span data-stu-id="796d8-135">For example, instead of a "job completed" event, have a "job in progress" condition, where you get emails at the start and end of a job.</span></span>

### <a name="set-up-alerts-automatically"></a><span data-ttu-id="796d8-136">Ställa in aviseringar automatiskt</span><span class="sxs-lookup"><span data-stu-id="796d8-136">Set up alerts automatically</span></span>
[<span data-ttu-id="796d8-137">Använd PowerShell för att skapa nya aviseringar</span><span class="sxs-lookup"><span data-stu-id="796d8-137">Use PowerShell to create new alerts</span></span>](app-insights-alerts.md#automation)

## <a name="use-powershell-to-manage-application-insights"></a><span data-ttu-id="796d8-138">Använd PowerShell för att hantera Application Insights</span><span class="sxs-lookup"><span data-stu-id="796d8-138">Use PowerShell to Manage Application Insights</span></span>
* [<span data-ttu-id="796d8-139">Skapa nya resurser</span><span class="sxs-lookup"><span data-stu-id="796d8-139">Create new resources</span></span>](app-insights-powershell-script-create-resource.md)
* [<span data-ttu-id="796d8-140">Skapa nya aviseringar</span><span class="sxs-lookup"><span data-stu-id="796d8-140">Create new alerts</span></span>](app-insights-alerts.md#automation)

## <a name="separate-telemetry-from-different-versions"></a><span data-ttu-id="796d8-141">Separata telemetri från olika versioner</span><span class="sxs-lookup"><span data-stu-id="796d8-141">Separate telemetry from different versions</span></span>

* <span data-ttu-id="796d8-142">Flera roller i en app: använda en enda Application Insights-resurs och filtrera på cloud_Rolename.</span><span class="sxs-lookup"><span data-stu-id="796d8-142">Multiple roles in an app: Use a single Application Insights resource, and filter on cloud_Rolename.</span></span> [<span data-ttu-id="796d8-143">Läs mer</span><span class="sxs-lookup"><span data-stu-id="796d8-143">Learn more</span></span>](app-insights-monitor-multi-role-apps.md)
* <span data-ttu-id="796d8-144">Avgränsa utveckling och test-versioner: olika Application Insights-resurser.</span><span class="sxs-lookup"><span data-stu-id="796d8-144">Separating development, test, and release versions: Use different Application Insights resources.</span></span> <span data-ttu-id="796d8-145">Hämta instrumentation nycklarna från web.config. [Läs mer](app-insights-separate-resources.md)</span><span class="sxs-lookup"><span data-stu-id="796d8-145">Pick up the instrumentation keys from web.config. [Learn more](app-insights-separate-resources.md)</span></span>
* <span data-ttu-id="796d8-146">Rapportering skapa versioner: lägga till en egenskap med ett telemetri initieraren.</span><span class="sxs-lookup"><span data-stu-id="796d8-146">Reporting build versions: Add a property using a telemetry initializer.</span></span> [<span data-ttu-id="796d8-147">Läs mer</span><span class="sxs-lookup"><span data-stu-id="796d8-147">Learn more</span></span>](app-insights-separate-resources.md)

## <a name="monitor-backend-servers-and-desktop-apps"></a><span data-ttu-id="796d8-148">Övervaka backend-servrar och -program</span><span class="sxs-lookup"><span data-stu-id="796d8-148">Monitor backend servers and desktop apps</span></span>
<span data-ttu-id="796d8-149">[Modulen för Windows Server SDK](app-insights-windows-desktop.md).</span><span class="sxs-lookup"><span data-stu-id="796d8-149">[Use the Windows Server SDK module](app-insights-windows-desktop.md).</span></span>

## <a name="visualize-data"></a><span data-ttu-id="796d8-150">Visualisera data</span><span class="sxs-lookup"><span data-stu-id="796d8-150">Visualize data</span></span>
#### <a name="dashboard-with-metrics-from-multiple-apps"></a><span data-ttu-id="796d8-151">Instrumentpanel med mått för flera appar</span><span class="sxs-lookup"><span data-stu-id="796d8-151">Dashboard with metrics from multiple apps</span></span>
* <span data-ttu-id="796d8-152">I [mått Explorer](app-insights-metrics-explorer.md), anpassa ditt diagram och spara den som en favorit.</span><span class="sxs-lookup"><span data-stu-id="796d8-152">In [Metric Explorer](app-insights-metrics-explorer.md), customize your chart and save it as a favorite.</span></span> <span data-ttu-id="796d8-153">Fäst det på Azure instrumentpanelen.</span><span class="sxs-lookup"><span data-stu-id="796d8-153">Pin it to the Azure dashboard.</span></span>

#### <a name="dashboard-with-data-from-other-sources-and-application-insights"></a><span data-ttu-id="796d8-154">Instrumentpanel med data från andra källor och Application Insights</span><span class="sxs-lookup"><span data-stu-id="796d8-154">Dashboard with data from other sources and Application Insights</span></span>
* <span data-ttu-id="796d8-155">[Exportera telemetri till Power BI](app-insights-export-power-bi.md).</span><span class="sxs-lookup"><span data-stu-id="796d8-155">[Export telemetry to Power BI](app-insights-export-power-bi.md).</span></span>

<span data-ttu-id="796d8-156">Eller</span><span class="sxs-lookup"><span data-stu-id="796d8-156">Or</span></span>

* <span data-ttu-id="796d8-157">Använda SharePoint som instrumentpanelen visar data i SharePoint-webbdelar.</span><span class="sxs-lookup"><span data-stu-id="796d8-157">Use SharePoint as your dashboard, displaying data in SharePoint web parts.</span></span> <span data-ttu-id="796d8-158">[Använd den löpande exporten och Stream Analytics för att exportera till SQL](app-insights-code-sample-export-sql-stream-analytics.md).</span><span class="sxs-lookup"><span data-stu-id="796d8-158">[Use continuous export and Stream Analytics to export to SQL](app-insights-code-sample-export-sql-stream-analytics.md).</span></span>  <span data-ttu-id="796d8-159">Använda PowerView för att undersöka databasen och skapa en SharePoint-webbdel för PowerView.</span><span class="sxs-lookup"><span data-stu-id="796d8-159">Use PowerView to examine the database, and create a SharePoint web part for PowerView.</span></span>

<a name="search-specific-users"></a>

### <a name="filter-out-anonymous-or-authenticated-users"></a><span data-ttu-id="796d8-160">Filtrera ut anonyma och autentiserade användare</span><span class="sxs-lookup"><span data-stu-id="796d8-160">Filter out anonymous or authenticated users</span></span>
<span data-ttu-id="796d8-161">Om dina användare loggar in, kan du ange den [autentiserat användar-id](app-insights-api-custom-events-metrics.md#authenticated-users). (Det inte sker automatiskt.)</span><span class="sxs-lookup"><span data-stu-id="796d8-161">If your users sign in, you can set the [authenticated user id](app-insights-api-custom-events-metrics.md#authenticated-users). (It doesn't happen automatically.)</span></span>

<span data-ttu-id="796d8-162">Sedan kan du:</span><span class="sxs-lookup"><span data-stu-id="796d8-162">You can then:</span></span>

* <span data-ttu-id="796d8-163">Söka på särskilda användar-ID</span><span class="sxs-lookup"><span data-stu-id="796d8-163">Search on specific user ids</span></span>

![](./media/app-insights-how-do-i/110-search.png)

* <span data-ttu-id="796d8-164">Filtrera mått till anonyma och autentiserade användare</span><span class="sxs-lookup"><span data-stu-id="796d8-164">Filter metrics to either anonymous or authenticated users</span></span>

![](./media/app-insights-how-do-i/115-metrics.png)

## <a name="modify-property-names-or-values"></a><span data-ttu-id="796d8-165">Ändra egenskapsnamn eller värden</span><span class="sxs-lookup"><span data-stu-id="796d8-165">Modify property names or values</span></span>
<span data-ttu-id="796d8-166">Skapa en [filter](app-insights-api-filtering-sampling.md#filtering).</span><span class="sxs-lookup"><span data-stu-id="796d8-166">Create a [filter](app-insights-api-filtering-sampling.md#filtering).</span></span> <span data-ttu-id="796d8-167">På så sätt kan du ändra eller filtrera telemetri innan den skickas från din app till Application Insights.</span><span class="sxs-lookup"><span data-stu-id="796d8-167">This lets you modify or filter telemetry before it is sent from your app to Application Insights.</span></span>

## <a name="list-specific-users-and-their-usage"></a><span data-ttu-id="796d8-168">Lista över specifika användare och deras användning</span><span class="sxs-lookup"><span data-stu-id="796d8-168">List specific users and their usage</span></span>
<span data-ttu-id="796d8-169">Om du bara vill [Sök efter specifika användare](#search-specific-users), du kan ange den [autentiserat användar-id](app-insights-api-custom-events-metrics.md#authenticated-users).</span><span class="sxs-lookup"><span data-stu-id="796d8-169">If you just want to [search for specific users](#search-specific-users), you can set the [authenticated user id](app-insights-api-custom-events-metrics.md#authenticated-users).</span></span>

<span data-ttu-id="796d8-170">Om du vill ha en lista över användare med data, till exempel vilka sidor du de titta på eller hur ofta de loggar in, har du två alternativ:</span><span class="sxs-lookup"><span data-stu-id="796d8-170">If you want a list of users with data such as what pages they look at or how often they log in, you have two options:</span></span>

* <span data-ttu-id="796d8-171">[Ange autentiserat användar-id](app-insights-api-custom-events-metrics.md#authenticated-users), [exportera till en databas](app-insights-code-sample-export-sql-stream-analytics.md) och använda lämpliga verktyg för att analysera dina användardata.</span><span class="sxs-lookup"><span data-stu-id="796d8-171">[Set authenticated user id](app-insights-api-custom-events-metrics.md#authenticated-users), [export to a database](app-insights-code-sample-export-sql-stream-analytics.md) and use suitable tools to analyze your user data there.</span></span>
* <span data-ttu-id="796d8-172">Om du har mindre antal användare kan skicka anpassade händelser eller statistik, och använda data av intresse som måttnamnet värde eller händelse och ange användar-id som en egenskap.</span><span class="sxs-lookup"><span data-stu-id="796d8-172">If you have only a small number of users, send custom events or metrics, using the data of interest as the metric value or event name, and setting the user id as a property.</span></span> <span data-ttu-id="796d8-173">Ersätt standard JavaScript trackPageView anropet för att analysera sidvisningar.</span><span class="sxs-lookup"><span data-stu-id="796d8-173">To analyze page views, replace the standard JavaScript trackPageView call.</span></span> <span data-ttu-id="796d8-174">Använd en telemetri initieraren att lägga till användar-id all telemetri för server för att analysera serversidan telemetri.</span><span class="sxs-lookup"><span data-stu-id="796d8-174">To analyze server-side telemetry, use a telemetry initializer to add the user id to all server telemetry.</span></span> <span data-ttu-id="796d8-175">Därefter kan du filtrera och segment mått och sökningar på det användar-id.</span><span class="sxs-lookup"><span data-stu-id="796d8-175">You can then filter and segment metrics and searches on the user id.</span></span>

## <a name="reduce-traffic-from-my-app-to-application-insights"></a><span data-ttu-id="796d8-176">Minska trafiken från min app till Application Insights</span><span class="sxs-lookup"><span data-stu-id="796d8-176">Reduce traffic from my app to Application Insights</span></span>
* <span data-ttu-id="796d8-177">I [ApplicationInsights.config](app-insights-configuration-with-applicationinsights-config.md), inaktivera alla moduler som du inte behöver dessa prestandaräknaren insamlaren.</span><span class="sxs-lookup"><span data-stu-id="796d8-177">In [ApplicationInsights.config](app-insights-configuration-with-applicationinsights-config.md), disable any modules you don't need, such the performance counter collector.</span></span>
* <span data-ttu-id="796d8-178">Använd [provtagning och filtrering](app-insights-api-filtering-sampling.md) på SDK.</span><span class="sxs-lookup"><span data-stu-id="796d8-178">Use [Sampling and filtering](app-insights-api-filtering-sampling.md) at the SDK.</span></span>
* <span data-ttu-id="796d8-179">Begränsa antalet Ajax-anrop som rapporterats för alla sidor i dina webbsidor.</span><span class="sxs-lookup"><span data-stu-id="796d8-179">In your web pages, Limit the number of Ajax calls reported for every page view.</span></span> <span data-ttu-id="796d8-180">Skriptet kodutdrag efter `instrumentationKey:...` , infoga: `,maxAjaxCallsPerView:3` (eller ett lämpligt antal).</span><span class="sxs-lookup"><span data-stu-id="796d8-180">In the script snippet after `instrumentationKey:...` , insert: `,maxAjaxCallsPerView:3` (or a suitable number).</span></span>
* <span data-ttu-id="796d8-181">Om du använder [TrackMetric](app-insights-api-custom-events-metrics.md#trackmetric), compute aggregering av batchar av måttvärden innan du skickar resultatet.</span><span class="sxs-lookup"><span data-stu-id="796d8-181">If you're using [TrackMetric](app-insights-api-custom-events-metrics.md#trackmetric), compute the aggregate of batches of metric values before sending the result.</span></span> <span data-ttu-id="796d8-182">Det finns en överlagring av TrackMetric() som tillhandahåller för som.</span><span class="sxs-lookup"><span data-stu-id="796d8-182">There's an overload of TrackMetric() that provides for that.</span></span>

<span data-ttu-id="796d8-183">Lär dig mer om [priser och kvoter](app-insights-pricing.md).</span><span class="sxs-lookup"><span data-stu-id="796d8-183">Learn more about [pricing and quotas](app-insights-pricing.md).</span></span>

## <a name="disable-telemetry"></a><span data-ttu-id="796d8-184">Inaktivera telemetri</span><span class="sxs-lookup"><span data-stu-id="796d8-184">Disable telemetry</span></span>
<span data-ttu-id="796d8-185">Att **dynamiskt stoppa och starta** insamling och vidarebefordran av telemetri från servern:</span><span class="sxs-lookup"><span data-stu-id="796d8-185">To **dynamically stop and start** the collection and transmission of telemetry from the server:</span></span>

```

    using  Microsoft.ApplicationInsights.Extensibility;

    TelemetryConfiguration.Active.DisableTelemetry = true;
```



<span data-ttu-id="796d8-186">Att **inaktivera valda standard insamlare** – till exempel prestandaräknare, HTTP-begäranden eller beroenden - ta bort eller kommentera ut relevanta rader i [ApplicationInsights.config](app-insights-api-custom-events-metrics.md). Du kan göra detta, till exempel om du vill skicka TrackRequest data.</span><span class="sxs-lookup"><span data-stu-id="796d8-186">To **disable selected standard collectors** - for example, performance counters, HTTP requests, or dependencies - delete or comment out the relevant lines in [ApplicationInsights.config](app-insights-api-custom-events-metrics.md). You could do this, for example, if you want to send your own TrackRequest data.</span></span>

## <a name="view-system-performance-counters"></a><span data-ttu-id="796d8-187">Visa prestandaräknare för system</span><span class="sxs-lookup"><span data-stu-id="796d8-187">View system performance counters</span></span>
<span data-ttu-id="796d8-188">Bland de mätvärden som du kan visa i metrics explorer är en uppsättning system prestandaräknare.</span><span class="sxs-lookup"><span data-stu-id="796d8-188">Among the metrics you can show in metrics explorer are a set of system performance counters.</span></span> <span data-ttu-id="796d8-189">Det finns en fördefinierad bladet med titeln **servrar** som visar flera.</span><span class="sxs-lookup"><span data-stu-id="796d8-189">There's a predefined blade titled **Servers** that displays several of them.</span></span>

![Öppna Application Insights-resursen och klicka på servrar](./media/app-insights-how-do-i/121-servers.png)

### <a name="if-you-see-no-performance-counter-data"></a><span data-ttu-id="796d8-191">Om du ser inga prestandaräknardata</span><span class="sxs-lookup"><span data-stu-id="796d8-191">If you see no performance counter data</span></span>
* <span data-ttu-id="796d8-192">**IIS-servern** på din egen dator eller på en virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="796d8-192">**IIS server** on your own machine or on a VM.</span></span> <span data-ttu-id="796d8-193">[Installera statusövervakaren](app-insights-monitor-performance-live-website-now.md).</span><span class="sxs-lookup"><span data-stu-id="796d8-193">[Install Status Monitor](app-insights-monitor-performance-live-website-now.md).</span></span>
* <span data-ttu-id="796d8-194">**Webbplatsen för Azure** -vi stöder inte prestandaräknare ännu.</span><span class="sxs-lookup"><span data-stu-id="796d8-194">**Azure web site** - we don't support performance counters yet.</span></span> <span data-ttu-id="796d8-195">Det finns flera mått som du kan få som en del av Kontrollpanelen webbplatsen för Azure.</span><span class="sxs-lookup"><span data-stu-id="796d8-195">There are several metrics you can get as a standard part of the Azure web site control panel.</span></span>
* <span data-ttu-id="796d8-196">**UNIX-server** - [installera collectd](app-insights-java-collectd.md)</span><span class="sxs-lookup"><span data-stu-id="796d8-196">**Unix server** - [Install collectd](app-insights-java-collectd.md)</span></span>

### <a name="to-display-more-performance-counters"></a><span data-ttu-id="796d8-197">Visa mer prestandaräknare</span><span class="sxs-lookup"><span data-stu-id="796d8-197">To display more performance counters</span></span>
* <span data-ttu-id="796d8-198">Första, [lägga till ett nytt diagram](app-insights-metrics-explorer.md) och se om räknaren finns i grundläggande som vi erbjuder.</span><span class="sxs-lookup"><span data-stu-id="796d8-198">First, [add a new chart](app-insights-metrics-explorer.md) and see if the counter is in the basic set that we offer.</span></span>
* <span data-ttu-id="796d8-199">Om inte, [lägga till räknaren uppsättningen som samlas in av prestandaräknarmodulen](app-insights-performance-counters.md).</span><span class="sxs-lookup"><span data-stu-id="796d8-199">If not, [add the counter to the set collected by the performance counter module](app-insights-performance-counters.md).</span></span>
