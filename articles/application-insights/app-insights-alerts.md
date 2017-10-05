---
title: "Ställ in aviseringar i Azure Application Insights | Microsoft Docs"
description: "Håll dig informerad om långa svarstider, undantag, andra prestanda och användning av ändringar i ditt webbprogram."
services: application-insights
documentationcenter: 
author: CFreemanwa
manager: carmonm
ms.assetid: f8ebde72-f819-4ba5-afa2-31dbd49509a5
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 03/14/2017
ms.author: bwren
ms.openlocfilehash: c8386ffc5d68260eeb75edf7efb77db1163dcef8
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/18/2017
---
# <a name="set-alerts-in-application-insights"></a><span data-ttu-id="3f06a-103">Ställ in aviseringar i Application Insights</span><span class="sxs-lookup"><span data-stu-id="3f06a-103">Set Alerts in Application Insights</span></span>
<span data-ttu-id="3f06a-104">[Azure Application Insights] [ start] kan meddela dig för ändringar i prestanda eller användningsområden mätvärden i ditt webbprogram.</span><span class="sxs-lookup"><span data-stu-id="3f06a-104">[Azure Application Insights][start] can alert you to changes in performance or usage metrics in your web app.</span></span> 

<span data-ttu-id="3f06a-105">Application Insights övervakar din aktiva app på en [flera olika plattformar] [ platforms] som hjälper dig att diagnostisera prestandaproblem och förstå användningsmönster.</span><span class="sxs-lookup"><span data-stu-id="3f06a-105">Application Insights monitors your live app on a [wide variety of platforms][platforms] to help you diagnose performance issues and understand usage patterns.</span></span>

<span data-ttu-id="3f06a-106">Det finns tre typer av aviseringar:</span><span class="sxs-lookup"><span data-stu-id="3f06a-106">There are three kinds of alerts:</span></span>

* <span data-ttu-id="3f06a-107">**Mått aviseringar** berättar när ett mått överskrider ett tröskelvärde för vissa punkt - till exempel svarstider, antalet undantag, CPU-användning eller sidvisningar.</span><span class="sxs-lookup"><span data-stu-id="3f06a-107">**Metric alerts** tell you when a metric crosses a threshold value for some period - such as response times, exception counts, CPU usage, or page views.</span></span> 
* <span data-ttu-id="3f06a-108">[**Webbtester** ] [ availability] berättar som din webbplats är tillgänglig på internet, eller så svarar långsamt.</span><span class="sxs-lookup"><span data-stu-id="3f06a-108">[**Web tests**][availability] tell you when your site is unavailable on the internet, or responding slowly.</span></span> <span data-ttu-id="3f06a-109">[Lär dig mer][availability].</span><span class="sxs-lookup"><span data-stu-id="3f06a-109">[Learn more][availability].</span></span>
* <span data-ttu-id="3f06a-110">[**Proaktiv diagnostik** ](app-insights-proactive-diagnostics.md) konfigureras automatiskt så att du meddelas om ovanliga prestandamönster.</span><span class="sxs-lookup"><span data-stu-id="3f06a-110">[**Proactive diagnostics**](app-insights-proactive-diagnostics.md) are configured automatically to notify you about unusual performance patterns.</span></span>

<span data-ttu-id="3f06a-111">Vi fokusera på mått aviseringar i den här artikeln.</span><span class="sxs-lookup"><span data-stu-id="3f06a-111">We focus on metric alerts in this article.</span></span>

## <a name="set-a-metric-alert"></a><span data-ttu-id="3f06a-112">Ange en avisering om mått</span><span class="sxs-lookup"><span data-stu-id="3f06a-112">Set a Metric alert</span></span>
<span data-ttu-id="3f06a-113">Öppna bladet Varningsregler och sedan använda knappen Lägg till.</span><span class="sxs-lookup"><span data-stu-id="3f06a-113">Open the Alert rules blade, and then use the add button.</span></span> 

![Välj Lägg till avisering i bladet Varningsregler.](./media/app-insights-alerts/01-set-metric.png)

* <span data-ttu-id="3f06a-116">Ange resursen innan de andra egenskaperna.</span><span class="sxs-lookup"><span data-stu-id="3f06a-116">Set the resource before the other properties.</span></span> <span data-ttu-id="3f06a-117">**Välj ”(komponenter)” resursen** om du vill ställa in varningar på prestanda eller användningsområden mätvärden.</span><span class="sxs-lookup"><span data-stu-id="3f06a-117">**Choose the "(components)" resource** if you want to set alerts on performance or usage metrics.</span></span>
* <span data-ttu-id="3f06a-118">Namnet som du ger aviseringen måste vara unikt inom resursgruppen (inte bara program).</span><span class="sxs-lookup"><span data-stu-id="3f06a-118">The name that you give to the alert must be unique within the resource group (not just your application).</span></span>
* <span data-ttu-id="3f06a-119">Var noga med att känna till de enheter där du uppmanas att ange ett tröskelvärde.</span><span class="sxs-lookup"><span data-stu-id="3f06a-119">Be careful to note the units in which you're asked to enter the threshold value.</span></span>
* <span data-ttu-id="3f06a-120">Om du markerar rutan ”e-ägare...” skickas aviseringar via e-post till alla som har åtkomst till den här resursgruppen.</span><span class="sxs-lookup"><span data-stu-id="3f06a-120">If you check the box "Email owners...", alerts are sent by email to everyone who has access to this resource group.</span></span> <span data-ttu-id="3f06a-121">Om du vill expandera den här uppsättningen personer, lägga till dem i den [resursgrupp eller prenumeration](app-insights-resources-roles-access-control.md) (inte på resursen).</span><span class="sxs-lookup"><span data-stu-id="3f06a-121">To expand this set of people, add them to the [resource group or subscription](app-insights-resources-roles-access-control.md) (not the resource).</span></span>
* <span data-ttu-id="3f06a-122">Om du anger ”ytterligare e-postmeddelanden” skickas aviseringar till de personer eller grupper (om du markerade kryssrutan ”Skicka e-ägare...”).</span><span class="sxs-lookup"><span data-stu-id="3f06a-122">If you specify "Additional emails", alerts are sent to those individuals or groups (whether or not you checked the "email owners..." box).</span></span> 
* <span data-ttu-id="3f06a-123">Ange en [webhook adress](../monitoring-and-diagnostics/insights-webhooks-alerts.md) om du har skapat ett webbprogram som svarar på aviseringar.</span><span class="sxs-lookup"><span data-stu-id="3f06a-123">Set a [webhook address](../monitoring-and-diagnostics/insights-webhooks-alerts.md) if you have set up a web app that responds to alerts.</span></span> <span data-ttu-id="3f06a-124">Det kallas både när aviseringen aktiveras och när den är löst.</span><span class="sxs-lookup"><span data-stu-id="3f06a-124">It is called both when the alert is Activated and when it is Resolved.</span></span> <span data-ttu-id="3f06a-125">(Men Observera att för närvarande, frågeparametrar skickas inte via webhook-egenskaper.)</span><span class="sxs-lookup"><span data-stu-id="3f06a-125">(But note that at present, query parameters are not passed through as webhook properties.)</span></span>
* <span data-ttu-id="3f06a-126">Du kan inaktivera eller aktivera en avisering: Se knappar längst upp på bladet.</span><span class="sxs-lookup"><span data-stu-id="3f06a-126">You can Disable or Enable the alert: see the buttons at the top of the blade.</span></span>

<span data-ttu-id="3f06a-127">*Knappen Lägg till avisering visas inte.*</span><span class="sxs-lookup"><span data-stu-id="3f06a-127">*I don't see the Add Alert button.*</span></span> 

* <span data-ttu-id="3f06a-128">Använder du ett organisationskonto?</span><span class="sxs-lookup"><span data-stu-id="3f06a-128">Are you using an organizational account?</span></span> <span data-ttu-id="3f06a-129">Du kan ställa in varningar om du har ägare eller deltagare som har åtkomst till den här resursen i programmet.</span><span class="sxs-lookup"><span data-stu-id="3f06a-129">You can set alerts if you have owner or contributor access to this application resource.</span></span> <span data-ttu-id="3f06a-130">Ta en titt på bladet för åtkomstkontroll.</span><span class="sxs-lookup"><span data-stu-id="3f06a-130">Take a look at the Access Control blade.</span></span> <span data-ttu-id="3f06a-131">[Lär dig mer om åtkomstkontroll][roles].</span><span class="sxs-lookup"><span data-stu-id="3f06a-131">[Learn about access control][roles].</span></span>

> [!NOTE]
> <span data-ttu-id="3f06a-132">Du ser att det finns redan en avisering uppsättning i bladet aviseringar: [Proactive Diagnostics](app-insights-proactive-failure-diagnostics.md).</span><span class="sxs-lookup"><span data-stu-id="3f06a-132">In the alerts blade, you see that there's already an alert set up: [Proactive Diagnostics](app-insights-proactive-failure-diagnostics.md).</span></span> <span data-ttu-id="3f06a-133">Automatisk avisering övervakar ett viss mått, misslyckandegrad.</span><span class="sxs-lookup"><span data-stu-id="3f06a-133">The automatic alert monitors one particular metric, request failure rate.</span></span> <span data-ttu-id="3f06a-134">Om du inte vill inaktivera proaktiv avisering behöver du inte ange egna avisering på misslyckandegrad.</span><span class="sxs-lookup"><span data-stu-id="3f06a-134">Unless you decide to disable the proactive alert, you don't need to set your own alert on request failure rate.</span></span> 
> 
> 

## <a name="see-your-alerts"></a><span data-ttu-id="3f06a-135">Se aviseringar</span><span class="sxs-lookup"><span data-stu-id="3f06a-135">See your alerts</span></span>
<span data-ttu-id="3f06a-136">Du får ett e-postmeddelande när en avisering ändrar tillstånd mellan inaktivt och aktivt.</span><span class="sxs-lookup"><span data-stu-id="3f06a-136">You get an email when an alert changes state between inactive and active.</span></span> 

<span data-ttu-id="3f06a-137">Det aktuella tillståndet för varje avisering visas i bladet Varningsregler.</span><span class="sxs-lookup"><span data-stu-id="3f06a-137">The current state of each alert is shown in the Alert rules blade.</span></span>

<span data-ttu-id="3f06a-138">Det finns en sammanfattning av de senaste aktivitet i aviseringar listrutan:</span><span class="sxs-lookup"><span data-stu-id="3f06a-138">There's a summary of recent activity in the alerts drop-down:</span></span>

![Aviseringar listrutan](./media/app-insights-alerts/010-alert-drop.png)

<span data-ttu-id="3f06a-140">Historik över tillståndsändringar är i aktivitetsloggen:</span><span class="sxs-lookup"><span data-stu-id="3f06a-140">The history of state changes is in the Activity Log:</span></span>

![Klicka på inställningar, granskningsloggar på bladet översikt](./media/app-insights-alerts/09-alerts.png)

## <a name="how-alerts-work"></a><span data-ttu-id="3f06a-142">Så fungerar aviseringar</span><span class="sxs-lookup"><span data-stu-id="3f06a-142">How alerts work</span></span>
* <span data-ttu-id="3f06a-143">En avisering har tre lägen: ”aldrig aktiverats”, ”aktiverad” och ”löst”.</span><span class="sxs-lookup"><span data-stu-id="3f06a-143">An alert has three states: "Never activated", "Activated", and "Resolved."</span></span> <span data-ttu-id="3f06a-144">Aktiverad innebär det villkor du anger var true när den senast utvärderades.</span><span class="sxs-lookup"><span data-stu-id="3f06a-144">Activated means the condition you specified was true, when it was last evaluated.</span></span>
* <span data-ttu-id="3f06a-145">En avisering genereras när en avisering ändrar tillstånd.</span><span class="sxs-lookup"><span data-stu-id="3f06a-145">A notification is generated when an alert changes state.</span></span> <span data-ttu-id="3f06a-146">(Om aviseringstillståndet var redan sant när du skapade aviseringen kan du kanske inte får ett meddelande tills villkoret blir FALSKT.)</span><span class="sxs-lookup"><span data-stu-id="3f06a-146">(If the alert condition was already true when you created the alert, you might not get a notification until the condition goes false.)</span></span>
* <span data-ttu-id="3f06a-147">Varje meddelande genererar ett e-postmeddelande om du har markerat kryssrutan e-postmeddelanden eller angivna e-postadresser.</span><span class="sxs-lookup"><span data-stu-id="3f06a-147">Each notification generates an email if you checked the emails box, or provided email addresses.</span></span> <span data-ttu-id="3f06a-148">Du kan också titta på listan meddelanden.</span><span class="sxs-lookup"><span data-stu-id="3f06a-148">You can also look at the Notifications drop-down list.</span></span>
* <span data-ttu-id="3f06a-149">En avisering utvärderas för varje gång ett mått tas emot, men inte på annat sätt.</span><span class="sxs-lookup"><span data-stu-id="3f06a-149">An alert is evaluated each time a metric arrives, but not otherwise.</span></span>
* <span data-ttu-id="3f06a-150">Utvärderingen aggregerar måttet under föregående period och sedan jämförs med tröskelvärdet för att fastställa det nya läget.</span><span class="sxs-lookup"><span data-stu-id="3f06a-150">The evaluation aggregates the metric over the preceding period, and then compares it to the threshold to determine the new state.</span></span>
* <span data-ttu-id="3f06a-151">Den period som du anger intervallet över vilket aggregeras mått.</span><span class="sxs-lookup"><span data-stu-id="3f06a-151">The period that you choose specifies the interval over which metrics are aggregated.</span></span> <span data-ttu-id="3f06a-152">Det påverkar inte hur ofta aviseringen ska utvärderas: som beror på frekvensen för ankomsten av mått.</span><span class="sxs-lookup"><span data-stu-id="3f06a-152">It doesn't affect how often the alert is evaluated: that depends on the frequency of arrival of metrics.</span></span>
* <span data-ttu-id="3f06a-153">Om inga data kommer för ett viss mått under en viss tid, har mellanrummet olika effekter på aviseringen utvärdering och diagram i mått explorer.</span><span class="sxs-lookup"><span data-stu-id="3f06a-153">If no data arrives for a particular metric for some time, the gap has different effects on alert evaluation and on the charts in metric explorer.</span></span> <span data-ttu-id="3f06a-154">I mått explorer om inga data visas under längre tid än diagrammets exempelintervall visar diagrammet värdet 0.</span><span class="sxs-lookup"><span data-stu-id="3f06a-154">In metric explorer, if no data is seen for longer than the chart's sampling interval, the chart shows a value of 0.</span></span> <span data-ttu-id="3f06a-155">Men en avisering baserat på samma mått kan inte vara reevaluated och den avisering tillstånd ändras inte.</span><span class="sxs-lookup"><span data-stu-id="3f06a-155">But an alert based on the same metric is not be reevaluated, and the alert's state remains unchanged.</span></span> 
  
    <span data-ttu-id="3f06a-156">När data kommer så småningom, hoppar diagrammet tillbaka till ett värde som inte är noll.</span><span class="sxs-lookup"><span data-stu-id="3f06a-156">When data eventually arrives, the chart jumps back to a non-zero value.</span></span> <span data-ttu-id="3f06a-157">Aviseringen utvärderar baserat på data som är tillgängliga för den angivna perioden.</span><span class="sxs-lookup"><span data-stu-id="3f06a-157">The alert evaluates based on the data available for the period you specified.</span></span> <span data-ttu-id="3f06a-158">Om den nya datapunkten är enda tillgängliga under perioden, baseras mängdfunktionen bara att datapunkt.</span><span class="sxs-lookup"><span data-stu-id="3f06a-158">If the new data point is the only one available in the period, the aggregate is based just on that data point.</span></span>
* <span data-ttu-id="3f06a-159">En avisering flimra ofta mellan varning och felfritt tillstånd, även om du anger en längre tid.</span><span class="sxs-lookup"><span data-stu-id="3f06a-159">An alert can flicker frequently between alert and healthy states, even if you set a long period.</span></span> <span data-ttu-id="3f06a-160">Detta kan inträffa om måttet är placerad runt tröskelvärdet.</span><span class="sxs-lookup"><span data-stu-id="3f06a-160">This can happen if the metric value hovers around the threshold.</span></span> <span data-ttu-id="3f06a-161">Det finns inga betraktas som tröskelvärde: övergången till avisering sker på samma värde som övergång till felfritt.</span><span class="sxs-lookup"><span data-stu-id="3f06a-161">There is no hysteresis in the threshold: the transition to alert happens at the same value as the transition to healthy.</span></span>

## <a name="what-are-good-alerts-to-set"></a><span data-ttu-id="3f06a-162">Vad är bra aviseringar för att ställa in?</span><span class="sxs-lookup"><span data-stu-id="3f06a-162">What are good alerts to set?</span></span>
<span data-ttu-id="3f06a-163">Det beror på ditt program.</span><span class="sxs-lookup"><span data-stu-id="3f06a-163">It depends on your application.</span></span> <span data-ttu-id="3f06a-164">Börja med, rekommenderas inte att ställa in för många mått.</span><span class="sxs-lookup"><span data-stu-id="3f06a-164">To start with, it's best not to set too many metrics.</span></span> <span data-ttu-id="3f06a-165">Ägna en stund tittar på dina mått diagram när appen körs, om du vill få en bild av hur den fungerar normalt.</span><span class="sxs-lookup"><span data-stu-id="3f06a-165">Spend some time looking at your metric charts while your app is running, to get a feel for how it behaves normally.</span></span> <span data-ttu-id="3f06a-166">Den här övningen hjälper dig att hitta sätt att förbättra prestandan.</span><span class="sxs-lookup"><span data-stu-id="3f06a-166">This practice helps you find ways to improve its performance.</span></span> <span data-ttu-id="3f06a-167">Ställa in aviseringar som talar om när mätvärdena går utanför normal zonen.</span><span class="sxs-lookup"><span data-stu-id="3f06a-167">Then set up alerts to tell you when the metrics go outside the normal zone.</span></span> 

<span data-ttu-id="3f06a-168">Populära aviseringar är:</span><span class="sxs-lookup"><span data-stu-id="3f06a-168">Popular alerts include:</span></span>

* <span data-ttu-id="3f06a-169">[Webbläsaren mått][client], särskilt webbläsare **sidan laddningstider**, är bra för webbprogram.</span><span class="sxs-lookup"><span data-stu-id="3f06a-169">[Browser metrics][client], especially Browser **page load times**, are good for web applications.</span></span> <span data-ttu-id="3f06a-170">Om sidan har många skript, ska du leta efter **webbläsarundantag**.</span><span class="sxs-lookup"><span data-stu-id="3f06a-170">If your page has many scripts, you should look for **browser exceptions**.</span></span> <span data-ttu-id="3f06a-171">För att få dessa mätvärden och aviseringar, du måste registrera [webbsida övervakning][client].</span><span class="sxs-lookup"><span data-stu-id="3f06a-171">In order to get these metrics and alerts, you have to set up [web page monitoring][client].</span></span>
* <span data-ttu-id="3f06a-172">**Serversvarstid** för serversidan av webbprogram.</span><span class="sxs-lookup"><span data-stu-id="3f06a-172">**Server response time** for the server side of web applications.</span></span> <span data-ttu-id="3f06a-173">Hålla ett öga på det här måttet att se om det beror oproportionerligt hög begäran priser samt att konfigurera aviseringar: variationen kan tyda på att din app körs slut på resurser.</span><span class="sxs-lookup"><span data-stu-id="3f06a-173">As well as setting up alerts, keep an eye on this metric to see if it varies disproportionately with high request rates: variation might indicate that your app is running out of resources.</span></span> 
* <span data-ttu-id="3f06a-174">**Undantag för** - om du vill se dem, du behöver göra några [ytterligare inställningar](app-insights-asp-net-exceptions.md).</span><span class="sxs-lookup"><span data-stu-id="3f06a-174">**Server exceptions** - to see them, you have to do some [additional setup](app-insights-asp-net-exceptions.md).</span></span>

<span data-ttu-id="3f06a-175">Glöm inte som [proaktiv fel hastighet diagnostik](app-insights-proactive-failure-diagnostics.md) automatiskt övervaka den hastighet som din app svarar på begäranden med felkoder.</span><span class="sxs-lookup"><span data-stu-id="3f06a-175">Don't forget that [proactive failure rate diagnostics](app-insights-proactive-failure-diagnostics.md) automatically monitor the rate at which your app responds to requests with failure codes.</span></span> 

## <a name="automation"></a><span data-ttu-id="3f06a-176">Automation</span><span class="sxs-lookup"><span data-stu-id="3f06a-176">Automation</span></span>
* [<span data-ttu-id="3f06a-177">Använd PowerShell för att automatisera ställa in aviseringar</span><span class="sxs-lookup"><span data-stu-id="3f06a-177">Use PowerShell to automate setting up alerts</span></span>](app-insights-powershell-alerts.md)
* [<span data-ttu-id="3f06a-178">Använd webhooks för att automatisera svarar på aviseringar</span><span class="sxs-lookup"><span data-stu-id="3f06a-178">Use webhooks to automate responding to alerts</span></span>](../monitoring-and-diagnostics/insights-webhooks-alerts.md)

## <a name="video"></a><span data-ttu-id="3f06a-179">Video</span><span class="sxs-lookup"><span data-stu-id="3f06a-179">Video</span></span>

> [!VIDEO https://channel9.msdn.com/events/Connect/2016/112/player]

## <a name="see-also"></a><span data-ttu-id="3f06a-180">Se även</span><span class="sxs-lookup"><span data-stu-id="3f06a-180">See also</span></span>
* [<span data-ttu-id="3f06a-181">Tillgänglighetstester för webbprogram</span><span class="sxs-lookup"><span data-stu-id="3f06a-181">Availability web tests</span></span>](app-insights-monitor-web-app-availability.md)
* [<span data-ttu-id="3f06a-182">Automatisera ställa in aviseringar</span><span class="sxs-lookup"><span data-stu-id="3f06a-182">Automate setting up alerts</span></span>](app-insights-powershell-alerts.md)
* [<span data-ttu-id="3f06a-183">Proaktiv diagnostik</span><span class="sxs-lookup"><span data-stu-id="3f06a-183">Proactive diagnostics</span></span>](app-insights-proactive-diagnostics.md) 

<!--Link references-->

[availability]: app-insights-monitor-web-app-availability.md
[client]: app-insights-javascript.md
[platforms]: app-insights-platforms.md
[roles]: app-insights-resources-roles-access-control.md
[start]: app-insights-overview.md

