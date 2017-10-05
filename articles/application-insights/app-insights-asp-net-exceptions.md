---
title: Diagnostisera fel och undantag i web apps med Azure Application Insights | Microsoft Docs
description: "Fånga undantag från ASP.NET appar tillsammans med begärandetelemetri."
services: application-insights
documentationcenter: .net
author: CFreemanwa
manager: carmonm
ms.assetid: d1e98390-3ce4-4d04-9351-144314a42aa2
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 03/14/2017
ms.author: bwren
ms.openlocfilehash: 7eeacdc6677ccdebb1653e94a163ecb47090b7ee
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/18/2017
---
# <a name="diagnose-exceptions-in-your-web-apps-with-application-insights"></a><span data-ttu-id="7202c-103">Diagnostisera undantag i ditt webbprogram med Application Insights</span><span class="sxs-lookup"><span data-stu-id="7202c-103">Diagnose exceptions in your web apps with Application Insights</span></span>
<span data-ttu-id="7202c-104">Undantag i livewebbappar rapporteras av [Programinsikter](app-insights-overview.md).</span><span class="sxs-lookup"><span data-stu-id="7202c-104">Exceptions in your live web app are reported by [Application Insights](app-insights-overview.md).</span></span> <span data-ttu-id="7202c-105">Du kan jämföra misslyckade begäranden med undantag och andra händelser på både klienten och servern, så att du snabbt kan diagnostisera orsaker.</span><span class="sxs-lookup"><span data-stu-id="7202c-105">You can correlate failed requests with exceptions and other events at both the client and server, so that you can quickly diagnose the causes.</span></span>

## <a name="set-up-exception-reporting"></a><span data-ttu-id="7202c-106">Konfigurera undantag reporting</span><span class="sxs-lookup"><span data-stu-id="7202c-106">Set up exception reporting</span></span>
* <span data-ttu-id="7202c-107">Ha undantag som rapporterats från din server:</span><span class="sxs-lookup"><span data-stu-id="7202c-107">To have exceptions reported from your server app:</span></span>
  * <span data-ttu-id="7202c-108">Installera [Application Insights SDK](app-insights-asp-net.md) i din Appkod eller</span><span class="sxs-lookup"><span data-stu-id="7202c-108">Install [Application Insights SDK](app-insights-asp-net.md) in your app code, or</span></span>
  * <span data-ttu-id="7202c-109">IIS-webbservrar: kör [Application Insights Agent](app-insights-monitor-performance-live-website-now.md); eller</span><span class="sxs-lookup"><span data-stu-id="7202c-109">IIS web servers: Run [Application Insights Agent](app-insights-monitor-performance-live-website-now.md); or</span></span>
  * <span data-ttu-id="7202c-110">Azure-webbappar: lägga till den [Application Insights Extension](app-insights-azure-web-apps.md)</span><span class="sxs-lookup"><span data-stu-id="7202c-110">Azure web apps: Add the [Application Insights Extension](app-insights-azure-web-apps.md)</span></span>
  * <span data-ttu-id="7202c-111">Java-webbappar: installera den [Java-agent](app-insights-java-agent.md)</span><span class="sxs-lookup"><span data-stu-id="7202c-111">Java web apps: Install the [Java agent](app-insights-java-agent.md)</span></span>
* <span data-ttu-id="7202c-112">Installera den [JavaScript-kodfragment](app-insights-javascript.md) på webbsidorna för att fånga undantag från webbläsaren.</span><span class="sxs-lookup"><span data-stu-id="7202c-112">Install the [JavaScript snippet](app-insights-javascript.md) in your web pages to catch browser exceptions.</span></span>
* <span data-ttu-id="7202c-113">I vissa ramverk för programmet eller med vissa inställningar måste du vidta ytterligare åtgärder för att fånga flera undantag:</span><span class="sxs-lookup"><span data-stu-id="7202c-113">In some application frameworks or with some settings, you need to take some extra steps to catch more exceptions:</span></span>
  * [<span data-ttu-id="7202c-114">Webbformulär</span><span class="sxs-lookup"><span data-stu-id="7202c-114">Web forms</span></span>](#web-forms)
  * [<span data-ttu-id="7202c-115">MVC</span><span class="sxs-lookup"><span data-stu-id="7202c-115">MVC</span></span>](#mvc)
  * [<span data-ttu-id="7202c-116">Webb-API-1.*</span><span class="sxs-lookup"><span data-stu-id="7202c-116">Web API 1.*</span></span>](#web-api-1)
  * [<span data-ttu-id="7202c-117">Webb-API 2.*</span><span class="sxs-lookup"><span data-stu-id="7202c-117">Web API 2.*</span></span>](#web-api-2)
  * [<span data-ttu-id="7202c-118">WCF</span><span class="sxs-lookup"><span data-stu-id="7202c-118">WCF</span></span>](#wcf)

## <a name="diagnosing-exceptions-using-visual-studio"></a><span data-ttu-id="7202c-119">Diagnostisera undantag med Visual Studio</span><span class="sxs-lookup"><span data-stu-id="7202c-119">Diagnosing exceptions using Visual Studio</span></span>
<span data-ttu-id="7202c-120">Öppna appen lösningen i Visual Studio för att hjälpa dig med felsökning.</span><span class="sxs-lookup"><span data-stu-id="7202c-120">Open the app solution in Visual Studio to help with debugging.</span></span>

<span data-ttu-id="7202c-121">Kör appen på din server eller på utvecklingsdatorn med F5.</span><span class="sxs-lookup"><span data-stu-id="7202c-121">Run the app, either on your server or on your development machine by using F5.</span></span>

<span data-ttu-id="7202c-122">Öppna fönstret Application Insights i Visual Studio och ställas in att visa händelser från din app.</span><span class="sxs-lookup"><span data-stu-id="7202c-122">Open the Application Insights Search window in Visual Studio, and set it to display events from your app.</span></span> <span data-ttu-id="7202c-123">När du felsöker kan du göra detta genom att klicka på knappen Application Insights.</span><span class="sxs-lookup"><span data-stu-id="7202c-123">While you're debugging, you can do this just by clicking the Application Insights button.</span></span>

![Högerklicka på projektet och välj Application Insights, öppna.](./media/app-insights-asp-net-exceptions/34.png)

<span data-ttu-id="7202c-125">Observera att du kan filtrera rapporten ska visa bara undantag.</span><span class="sxs-lookup"><span data-stu-id="7202c-125">Notice that you can filter the report to show just exceptions.</span></span>

<span data-ttu-id="7202c-126">*Inga undantag visar? Se [fånga undantag](#exceptions).*</span><span class="sxs-lookup"><span data-stu-id="7202c-126">*No exceptions showing? See [Capture exceptions](#exceptions).*</span></span>

<span data-ttu-id="7202c-127">Klicka på en felrapport att visa dess stack-spårning.</span><span class="sxs-lookup"><span data-stu-id="7202c-127">Click an exception report to show its stack trace.</span></span>
<span data-ttu-id="7202c-128">Klicka på en radreferens i stackspårningen att öppna filen relevant kod.</span><span class="sxs-lookup"><span data-stu-id="7202c-128">Click a line reference in the stack trace, to open the relevant code file.</span></span>  

<span data-ttu-id="7202c-129">Observera att CodeLens visar information om undantagen i koden:</span><span class="sxs-lookup"><span data-stu-id="7202c-129">In the code, notice that CodeLens shows data about the exceptions:</span></span>

![CodeLens meddelande om undantag.](./media/app-insights-asp-net-exceptions/35.png)

## <a name="diagnosing-failures-using-the-azure-portal"></a><span data-ttu-id="7202c-131">Diagnostisera fel i Azure Portal</span><span class="sxs-lookup"><span data-stu-id="7202c-131">Diagnosing failures using the Azure portal</span></span>
<span data-ttu-id="7202c-132">Från Application Insights-översikten över appen panelen fel visar diagram av undantag och misslyckade HTTP-begäranden, tillsammans med en lista över begäran URL: er som orsakar de vanligaste felen.</span><span class="sxs-lookup"><span data-stu-id="7202c-132">From the Application Insights overview of your app, the Failures tile shows you charts of exceptions and failed HTTP requests, together with a list of the request URLs that cause the most frequent failures.</span></span>

![Välj inställningar för fel](./media/app-insights-asp-net-exceptions/012-start.png)

<span data-ttu-id="7202c-134">Klicka dig igenom en av de misslyckade undantag typerna i listan till enskilda förekomster av undantag, där du kan se detaljer och Stackspårning:</span><span class="sxs-lookup"><span data-stu-id="7202c-134">Click through one of the failed exception types in the list to get to individual occurrences of the exception, where you can see the details and stack trace:</span></span>

![Välj en instans av en misslyckad begäran och under undantagsinformation, hämta till instanser av undantaget.](./media/app-insights-asp-net-exceptions/030-req-drill.png)

<span data-ttu-id="7202c-136">**Du kan också** du kan starta från listan med begäranden och hitta undantag som är relaterade till den.</span><span class="sxs-lookup"><span data-stu-id="7202c-136">**Alternatively,** you can start from the list of requests and find exceptions related to it.</span></span>

<span data-ttu-id="7202c-137">*Inga undantag visar? Se [fånga undantag](#exceptions).*</span><span class="sxs-lookup"><span data-stu-id="7202c-137">*No exceptions showing? See [Capture exceptions](#exceptions).*</span></span>


## <a name="custom-tracing-and-log-data"></a><span data-ttu-id="7202c-138">Anpassade spårning och loggdata</span><span class="sxs-lookup"><span data-stu-id="7202c-138">Custom tracing and log data</span></span>
<span data-ttu-id="7202c-139">Du kan infoga kod för att skicka telemetridata för att få diagnostiska uppgifter som är specifika för din app.</span><span class="sxs-lookup"><span data-stu-id="7202c-139">To get diagnostic data specific to your app, you can insert code to send your own telemetry data.</span></span> <span data-ttu-id="7202c-140">Detta visas i diagnostiska tillsammans med begäran, vyn sida och andra automatiskt insamlade data.</span><span class="sxs-lookup"><span data-stu-id="7202c-140">This displayed in diagnostic search alongside the request, page view and other automatically-collected data.</span></span>

<span data-ttu-id="7202c-141">Har du flera alternativ:</span><span class="sxs-lookup"><span data-stu-id="7202c-141">You have several options:</span></span>

* <span data-ttu-id="7202c-142">[Trackevent ()](app-insights-api-custom-events-metrics.md#trackevent) används vanligtvis för att övervaka användningsmönster, men data som skickas också visas under Anpassad händelser i diagnostiska sökningen.</span><span class="sxs-lookup"><span data-stu-id="7202c-142">[TrackEvent()](app-insights-api-custom-events-metrics.md#trackevent) is typically used for monitoring usage patterns, but the data it sends also appears under Custom Events in diagnostic search.</span></span> <span data-ttu-id="7202c-143">Händelser är namngivna och kan utföra strängegenskaper och numeriska mått som du kan [filtrera sökningen diagnostiska](app-insights-diagnostic-search.md).</span><span class="sxs-lookup"><span data-stu-id="7202c-143">Events are named, and can carry string properties and numeric metrics on which you can [filter your diagnostic searches](app-insights-diagnostic-search.md).</span></span>
* <span data-ttu-id="7202c-144">[TrackTrace()](app-insights-api-custom-events-metrics.md#tracktrace) kan du skicka längre data, till exempel efter information.</span><span class="sxs-lookup"><span data-stu-id="7202c-144">[TrackTrace()](app-insights-api-custom-events-metrics.md#tracktrace) lets you send longer data such as POST information.</span></span>
* <span data-ttu-id="7202c-145">[TrackException()](#exceptions) skickar Stacka spårningar.</span><span class="sxs-lookup"><span data-stu-id="7202c-145">[TrackException()](#exceptions) sends stack traces.</span></span> <span data-ttu-id="7202c-146">[Mer information om undantag](#exceptions).</span><span class="sxs-lookup"><span data-stu-id="7202c-146">[More about exceptions](#exceptions).</span></span>
* <span data-ttu-id="7202c-147">Om du redan använder ett loggningsramverk som Log4Net eller NLog, kan du [samla in dessa loggar](app-insights-asp-net-trace-logs.md) och se dem i diagnostiska sökningen tillsammans med data för begäran och undantag.</span><span class="sxs-lookup"><span data-stu-id="7202c-147">If you already use a logging framework like Log4Net or NLog, you can [capture those logs](app-insights-asp-net-trace-logs.md) and see them in diagnostic search alongside request and exception data.</span></span>

<span data-ttu-id="7202c-148">Om du vill se händelserna [Sök](app-insights-diagnostic-search.md), öppna Filter och välj sedan Custom Event, spårning eller undantag.</span><span class="sxs-lookup"><span data-stu-id="7202c-148">To see these events, open [Search](app-insights-diagnostic-search.md), open Filter, and then choose Custom Event, Trace, or Exception.</span></span>

![Detaljvisning](./media/app-insights-asp-net-exceptions/viewCustomEvents.png)

> [!NOTE]
> <span data-ttu-id="7202c-150">Om din app genererar mycket telemetri minskar den anpassningsbara insamlingsmodulen automatiskt den mängd som skickas till portalen genom att bara skicka en representativ del av händelserna.</span><span class="sxs-lookup"><span data-stu-id="7202c-150">If your app generates a lot of telemetry, the adaptive sampling module will automatically reduce the volume that is sent to the portal by sending only a representative fraction of events.</span></span> <span data-ttu-id="7202c-151">Händelser som ingår i samma åtgärd ska markeras eller avmarkeras som en grupp, så att du kan navigera mellan relaterade händelser.</span><span class="sxs-lookup"><span data-stu-id="7202c-151">Events that are part of the same operation will be selected or deselected as a group, so that you can navigate between related events.</span></span> [<span data-ttu-id="7202c-152">Läs mer om sampling.</span><span class="sxs-lookup"><span data-stu-id="7202c-152">Learn about sampling.</span></span>](app-insights-sampling.md)
>
>

### <a name="how-to-see-request-post-data"></a><span data-ttu-id="7202c-153">Hur man ser begäran postdata</span><span class="sxs-lookup"><span data-stu-id="7202c-153">How to see request POST data</span></span>
<span data-ttu-id="7202c-154">Information om begäran innehåller inte data som skickas till din app i en POST-anrop.</span><span class="sxs-lookup"><span data-stu-id="7202c-154">Request details don't include the data sent to your app in a POST call.</span></span> <span data-ttu-id="7202c-155">Rapporterade att dessa data:</span><span class="sxs-lookup"><span data-stu-id="7202c-155">To have this data reported:</span></span>

* <span data-ttu-id="7202c-156">[Installera SDK](app-insights-asp-net.md) i projektet program.</span><span class="sxs-lookup"><span data-stu-id="7202c-156">[Install the SDK](app-insights-asp-net.md) in your application project.</span></span>
* <span data-ttu-id="7202c-157">Infoga kod i ditt program att anropa [Microsoft.ApplicationInsights.TrackTrace()](app-insights-api-custom-events-metrics.md#tracktrace).</span><span class="sxs-lookup"><span data-stu-id="7202c-157">Insert code in your application to call [Microsoft.ApplicationInsights.TrackTrace()](app-insights-api-custom-events-metrics.md#tracktrace).</span></span> <span data-ttu-id="7202c-158">Skicka efter data i parametern meddelandet.</span><span class="sxs-lookup"><span data-stu-id="7202c-158">Send the POST data in the message parameter.</span></span> <span data-ttu-id="7202c-159">Det finns en gräns för den tillåtna storleken du ska försöka skicka bara viktiga data.</span><span class="sxs-lookup"><span data-stu-id="7202c-159">There is a limit to the permitted size, so you should try to send just the essential data.</span></span>
* <span data-ttu-id="7202c-160">När du undersöker en misslyckad begäran att hitta de associerade spårningar.</span><span class="sxs-lookup"><span data-stu-id="7202c-160">When you investigate a failed request, find the associated traces.</span></span>  

![Detaljvisning](./media/app-insights-asp-net-exceptions/060-req-related.png)

## <span data-ttu-id="7202c-162"><a name="exceptions"></a>Fånga undantag och relaterade diagnostikdata</span><span class="sxs-lookup"><span data-stu-id="7202c-162"><a name="exceptions"></a> Capturing exceptions and related diagnostic data</span></span>
<span data-ttu-id="7202c-163">Först visas inte i portalen alla undantag som orsakar fel i din app.</span><span class="sxs-lookup"><span data-stu-id="7202c-163">At first, you won't see in the portal all the exceptions that cause failures in your app.</span></span> <span data-ttu-id="7202c-164">Du ser alla webbläsarundantag (om du använder den [JavaScript SDK](app-insights-javascript.md) på webbsidorna).</span><span class="sxs-lookup"><span data-stu-id="7202c-164">You'll see any browser exceptions (if you're using the [JavaScript SDK](app-insights-javascript.md) in your web pages).</span></span> <span data-ttu-id="7202c-165">Men de flesta undantag fångas av IIS och du måste skriva koden för att se dem lite.</span><span class="sxs-lookup"><span data-stu-id="7202c-165">But most server exceptions are caught by IIS and you have to write a bit of code to see them.</span></span>

<span data-ttu-id="7202c-166">Du kan:</span><span class="sxs-lookup"><span data-stu-id="7202c-166">You can:</span></span>

* <span data-ttu-id="7202c-167">**Logga undantag uttryckligen** genom att infoga kod i undantagshanterare att rapportera undantagen.</span><span class="sxs-lookup"><span data-stu-id="7202c-167">**Log exceptions explicitly** by inserting code in exception handlers to report the exceptions.</span></span>
* <span data-ttu-id="7202c-168">**Fånga undantag automatiskt** genom att konfigurera ASP.NET framework.</span><span class="sxs-lookup"><span data-stu-id="7202c-168">**Capture exceptions automatically** by configuring your ASP.NET framework.</span></span> <span data-ttu-id="7202c-169">De nödvändiga tilläggen är olika för olika typer av framework.</span><span class="sxs-lookup"><span data-stu-id="7202c-169">The necessary additions are different for different types of framework.</span></span>

## <a name="reporting-exceptions-explicitly"></a><span data-ttu-id="7202c-170">Rapportering undantag uttryckligen</span><span class="sxs-lookup"><span data-stu-id="7202c-170">Reporting exceptions explicitly</span></span>
<span data-ttu-id="7202c-171">Det enklaste sättet är att infoga ett anrop till TrackException() i en undantagshanterare.</span><span class="sxs-lookup"><span data-stu-id="7202c-171">The simplest way is to insert a call to TrackException() in an exception handler.</span></span>

<span data-ttu-id="7202c-172">JavaScript</span><span class="sxs-lookup"><span data-stu-id="7202c-172">JavaScript</span></span>

    try
    { ...
    }
    catch (ex)
    {
      appInsights.trackException(ex, "handler loc",
        {Game: currentGame.Name,
         State: currentGame.State.ToString()});
    }

<span data-ttu-id="7202c-173">C#</span><span class="sxs-lookup"><span data-stu-id="7202c-173">C#</span></span>

    var telemetry = new TelemetryClient();
    ...
    try
    { ...
    }
    catch (Exception ex)
    {
       // Set up some properties:
       var properties = new Dictionary <string, string>
         {{"Game", currentGame.Name}};

       var measurements = new Dictionary <string, double>
         {{"Users", currentGame.Users.Count}};

       // Send the exception telemetry:
       telemetry.TrackException(ex, properties, measurements);
    }

<span data-ttu-id="7202c-174">VB</span><span class="sxs-lookup"><span data-stu-id="7202c-174">VB</span></span>

    Dim telemetry = New TelemetryClient
    ...
    Try
      ...
    Catch ex as Exception
      ' Set up some properties:
      Dim properties = New Dictionary (Of String, String)
      properties.Add("Game", currentGame.Name)

      Dim measurements = New Dictionary (Of String, Double)
      measurements.Add("Users", currentGame.Users.Count)

      ' Send the exception telemetry:
      telemetry.TrackException(ex, properties, measurements)
    End Try

<span data-ttu-id="7202c-175">Egenskaper och mätningar parametrar är valfria, men är användbara för [filtrering och lägga till](app-insights-diagnostic-search.md) extra information.</span><span class="sxs-lookup"><span data-stu-id="7202c-175">The properties and measurements parameters are optional, but are useful for [filtering and adding](app-insights-diagnostic-search.md) extra information.</span></span> <span data-ttu-id="7202c-176">Om du har en app som kan köra flera spel hitta du till exempel alla undantag rapporter som hör till ett visst spel.</span><span class="sxs-lookup"><span data-stu-id="7202c-176">For example, if you have an app that can run several games, you could find all the exception reports related to a particular game.</span></span> <span data-ttu-id="7202c-177">Du kan lägga till så många objekt som du vill att varje ordlista.</span><span class="sxs-lookup"><span data-stu-id="7202c-177">You can add as many items as you like to each dictionary.</span></span>

## <a name="browser-exceptions"></a><span data-ttu-id="7202c-178">Webbläsarundantag</span><span class="sxs-lookup"><span data-stu-id="7202c-178">Browser exceptions</span></span>
<span data-ttu-id="7202c-179">De flesta webbläsarundantag rapporteras.</span><span class="sxs-lookup"><span data-stu-id="7202c-179">Most browser exceptions are reported.</span></span>

<span data-ttu-id="7202c-180">Om din webbsida innehåller skriptfiler från nätverk för innehållsleverans eller andra domäner, kontrollera din skripttypen har attributet ```crossorigin="anonymous"```, och att servern skickar [CORS huvuden](http://enable-cors.org/).</span><span class="sxs-lookup"><span data-stu-id="7202c-180">If your web page includes script files from content delivery networks or other domains, ensure your script tag has the attribute ```crossorigin="anonymous"```,  and that the server sends [CORS headers](http://enable-cors.org/).</span></span> <span data-ttu-id="7202c-181">Detta kan du få ett stack-spårning och detaljer för ohanterade JavaScript-undantag från dessa resurser.</span><span class="sxs-lookup"><span data-stu-id="7202c-181">This will allow you to get a stack trace and detail for unhandled JavaScript exceptions from these resources.</span></span>

## <a name="web-forms"></a><span data-ttu-id="7202c-182">Webbformulär</span><span class="sxs-lookup"><span data-stu-id="7202c-182">Web forms</span></span>
<span data-ttu-id="7202c-183">HTTP-modul kommer att kunna samla in undantag när det finns inga omdirigeringar som konfigurerats med CustomErrors för web forms.</span><span class="sxs-lookup"><span data-stu-id="7202c-183">For web forms, the HTTP Module will be able to collect the exceptions when there are no redirects configured with CustomErrors.</span></span>

<span data-ttu-id="7202c-184">Men om du har active omdirigeringar, lägger du till följande rader funktionen Application_Error i Global.asax.cs.</span><span class="sxs-lookup"><span data-stu-id="7202c-184">But if you have active redirects, add the following lines to the Application_Error function in Global.asax.cs.</span></span> <span data-ttu-id="7202c-185">(Lägg till en Global.asax-fil om du inte redan har ett.)</span><span class="sxs-lookup"><span data-stu-id="7202c-185">(Add a Global.asax file if you don't already have one.)</span></span>

<span data-ttu-id="7202c-186">*C#*</span><span class="sxs-lookup"><span data-stu-id="7202c-186">*C#*</span></span>

    void Application_Error(object sender, EventArgs e)
    {
      if (HttpContext.Current.IsCustomErrorEnabled && Server.GetLastError  () != null)
      {
         var ai = new TelemetryClient(); // or re-use an existing instance

         ai.TrackException(Server.GetLastError());
      }
    }


## <a name="mvc"></a><span data-ttu-id="7202c-187">MVC</span><span class="sxs-lookup"><span data-stu-id="7202c-187">MVC</span></span>
<span data-ttu-id="7202c-188">Om den [CustomErrors](https://msdn.microsoft.com/library/h0hfz6fc.aspx) konfigurationen är `Off`, undantag ska vara tillgängliga för den [HTTP-modul](https://msdn.microsoft.com/library/ms178468.aspx) att samla in.</span><span class="sxs-lookup"><span data-stu-id="7202c-188">If the [CustomErrors](https://msdn.microsoft.com/library/h0hfz6fc.aspx) configuration is `Off`, then exceptions will be available for the [HTTP Module](https://msdn.microsoft.com/library/ms178468.aspx) to collect.</span></span> <span data-ttu-id="7202c-189">Men om det är `RemoteOnly` (standard), eller `On`, undantaget ska vara avmarkerad och inte tillgängliga för Application Insights att automatiskt samla in.</span><span class="sxs-lookup"><span data-stu-id="7202c-189">However, if it is `RemoteOnly` (default), or `On`, then the exception will be cleared and not available for Application Insights to automatically collect.</span></span> <span data-ttu-id="7202c-190">Du kan åtgärda detta genom att åsidosätta den [System.Web.Mvc.HandleErrorAttribute klassen](http://msdn.microsoft.com/library/system.web.mvc.handleerrorattribute.aspx), och tillämpa åsidosatt klassen som visas för de olika MVC-versionerna nedan ([github-källan](https://github.com/AppInsightsSamples/Mvc2UnhandledExceptions/blob/master/MVC2App/Controllers/AiHandleErrorAttribute.cs)):</span><span class="sxs-lookup"><span data-stu-id="7202c-190">You can fix that by overriding the [System.Web.Mvc.HandleErrorAttribute class](http://msdn.microsoft.com/library/system.web.mvc.handleerrorattribute.aspx), and applying the overridden class as shown for the different MVC versions below ([github source](https://github.com/AppInsightsSamples/Mvc2UnhandledExceptions/blob/master/MVC2App/Controllers/AiHandleErrorAttribute.cs)):</span></span>

    using System;
    using System.Web.Mvc;
    using Microsoft.ApplicationInsights;

    namespace MVC2App.Controllers
    {
      [AttributeUsage(AttributeTargets.Class | AttributeTargets.Method, Inherited = true, AllowMultiple = true)]
      public class AiHandleErrorAttribute : HandleErrorAttribute
      {
        public override void OnException(ExceptionContext filterContext)
        {
            if (filterContext != null && filterContext.HttpContext != null && filterContext.Exception != null)
            {
                //If customError is Off, then AI HTTPModule will report the exception
                if (filterContext.HttpContext.IsCustomErrorEnabled)
                {   //or reuse instance (recommended!). see note above  
                    var ai = new TelemetryClient();
                    ai.TrackException(filterContext.Exception);
                }
            }
            base.OnException(filterContext);
        }
      }
    }

#### <a name="mvc-2"></a><span data-ttu-id="7202c-191">MVC 2</span><span class="sxs-lookup"><span data-stu-id="7202c-191">MVC 2</span></span>
<span data-ttu-id="7202c-192">Ersätt attributet HandleError med din nya attribut i dina domänkontrollanter.</span><span class="sxs-lookup"><span data-stu-id="7202c-192">Replace the HandleError attribute with your new attribute in your controllers.</span></span>

    namespace MVC2App.Controllers
    {
       [AiHandleError]
       public class HomeController : Controller
       {
    ...

[<span data-ttu-id="7202c-193">Exempel</span><span class="sxs-lookup"><span data-stu-id="7202c-193">Sample</span></span>](https://github.com/AppInsightsSamples/Mvc2UnhandledExceptions)

#### <a name="mvc-3"></a><span data-ttu-id="7202c-194">MVC 3</span><span class="sxs-lookup"><span data-stu-id="7202c-194">MVC 3</span></span>
<span data-ttu-id="7202c-195">Registrera `AiHandleErrorAttribute` som ett globalt filter i Global.asax.cs:</span><span class="sxs-lookup"><span data-stu-id="7202c-195">Register `AiHandleErrorAttribute` as a global filter in Global.asax.cs:</span></span>

    public class MyMvcApplication : System.Web.HttpApplication
    {
      public static void RegisterGlobalFilters(GlobalFilterCollection filters)
      {
         filters.Add(new AiHandleErrorAttribute());
      }
     ...

[<span data-ttu-id="7202c-196">Exempel</span><span class="sxs-lookup"><span data-stu-id="7202c-196">Sample</span></span>](https://github.com/AppInsightsSamples/Mvc3UnhandledExceptionTelemetry)

#### <a name="mvc-4-mvc5"></a><span data-ttu-id="7202c-197">MVC 4, MVC5</span><span class="sxs-lookup"><span data-stu-id="7202c-197">MVC 4, MVC5</span></span>
<span data-ttu-id="7202c-198">Registrera AiHandleErrorAttribute som ett globalt filter i FilterConfig.cs:</span><span class="sxs-lookup"><span data-stu-id="7202c-198">Register AiHandleErrorAttribute as a global filter in FilterConfig.cs:</span></span>

    public class FilterConfig
    {
      public static void RegisterGlobalFilters(GlobalFilterCollection filters)
      {
        // Default replaced with the override to track unhandled exceptions
        filters.Add(new AiHandleErrorAttribute());
      }
    }

[<span data-ttu-id="7202c-199">Exempel</span><span class="sxs-lookup"><span data-stu-id="7202c-199">Sample</span></span>](https://github.com/AppInsightsSamples/Mvc5UnhandledExceptionTelemetry)

## <a name="web-api-1x"></a><span data-ttu-id="7202c-200">Webb-API 1.x</span><span class="sxs-lookup"><span data-stu-id="7202c-200">Web API 1.x</span></span>
<span data-ttu-id="7202c-201">Åsidosätt System.Web.Http.Filters.ExceptionFilterAttribute:</span><span class="sxs-lookup"><span data-stu-id="7202c-201">Override System.Web.Http.Filters.ExceptionFilterAttribute:</span></span>

    using System.Web.Http.Filters;
    using Microsoft.ApplicationInsights;

    namespace WebAPI.App_Start
    {
      public class AiExceptionFilterAttribute : ExceptionFilterAttribute
      {
        public override void OnException(HttpActionExecutedContext actionExecutedContext)
        {
            if (actionExecutedContext != null && actionExecutedContext.Exception != null)
            {  //or reuse instance (recommended!). see note above
                var ai = new TelemetryClient();
                ai.TrackException(actionExecutedContext.Exception);    
            }
            base.OnException(actionExecutedContext);
        }
      }
    }

<span data-ttu-id="7202c-202">Du kan lägga till den här åsidosatt attribut till specifika domänkontrollanter eller lägga till den globala filterkonfiguration i klassen WebApiConfig:</span><span class="sxs-lookup"><span data-stu-id="7202c-202">You could add this overridden attribute to specific controllers, or add it to the global filter configuration in the WebApiConfig class:</span></span>

    using System.Web.Http;
    using WebApi1.x.App_Start;

    namespace WebApi1.x
    {
      public static class WebApiConfig
      {
        public static void Register(HttpConfiguration config)
        {
            config.Routes.MapHttpRoute(name: "DefaultApi", routeTemplate: "api/{controller}/{id}",
                defaults: new { id = RouteParameter.Optional });
            ...
            config.EnableSystemDiagnosticsTracing();

            // Capture exceptions for Application Insights:
            config.Filters.Add(new AiExceptionFilterAttribute());
        }
      }
    }

[<span data-ttu-id="7202c-203">Exempel</span><span class="sxs-lookup"><span data-stu-id="7202c-203">Sample</span></span>](https://github.com/AppInsightsSamples/WebApi_1.x_UnhandledExceptions)

<span data-ttu-id="7202c-204">Det finns ett antal fall som undantagsfilter inte kan hantera.</span><span class="sxs-lookup"><span data-stu-id="7202c-204">There are a number of cases that the exception filters cannot handle.</span></span> <span data-ttu-id="7202c-205">Exempel:</span><span class="sxs-lookup"><span data-stu-id="7202c-205">For example:</span></span>

* <span data-ttu-id="7202c-206">Undantag från domänkontrollanten konstruktorer.</span><span class="sxs-lookup"><span data-stu-id="7202c-206">Exceptions thrown from controller constructors.</span></span>
* <span data-ttu-id="7202c-207">Undantag från meddelandehanterare.</span><span class="sxs-lookup"><span data-stu-id="7202c-207">Exceptions thrown from message handlers.</span></span>
* <span data-ttu-id="7202c-208">Undantag under routning.</span><span class="sxs-lookup"><span data-stu-id="7202c-208">Exceptions thrown during routing.</span></span>
* <span data-ttu-id="7202c-209">Undantag under innehåll serialiseringssvar.</span><span class="sxs-lookup"><span data-stu-id="7202c-209">Exceptions thrown during response content serialization.</span></span>

## <a name="web-api-2x"></a><span data-ttu-id="7202c-210">Webb-API 2.x</span><span class="sxs-lookup"><span data-stu-id="7202c-210">Web API 2.x</span></span>
<span data-ttu-id="7202c-211">Lägg till en implementering av IExceptionLogger:</span><span class="sxs-lookup"><span data-stu-id="7202c-211">Add an implementation of IExceptionLogger:</span></span>

    using System.Web.Http.ExceptionHandling;
    using Microsoft.ApplicationInsights;

    namespace ProductsAppPureWebAPI.App_Start
    {
      public class AiExceptionLogger : ExceptionLogger
      {
        public override void Log(ExceptionLoggerContext context)
        {
            if (context !=null && context.Exception != null)
            {//or reuse instance (recommended!). see note above
                var ai = new TelemetryClient();
                ai.TrackException(context.Exception);
            }
            base.Log(context);
        }
      }
    }

<span data-ttu-id="7202c-212">Lägg till det här tjänsterna i WebApiConfig:</span><span class="sxs-lookup"><span data-stu-id="7202c-212">Add this to the services in WebApiConfig:</span></span>

    using System.Web.Http;
    using System.Web.Http.ExceptionHandling;
    using ProductsAppPureWebAPI.App_Start;

    namespace WebApi2WithMVC
    {
      public static class WebApiConfig
      {
        public static void Register(HttpConfiguration config)
        {
            // Web API configuration and services

            // Web API routes
            config.MapHttpAttributeRoutes();

            config.Routes.MapHttpRoute(
                name: "DefaultApi",
                routeTemplate: "api/{controller}/{id}",
                defaults: new { id = RouteParameter.Optional }
            );
            config.Services.Add(typeof(IExceptionLogger), new AiExceptionLogger());
        }
      }
  <span data-ttu-id="7202c-213">}</span><span class="sxs-lookup"><span data-stu-id="7202c-213">}</span></span>

[<span data-ttu-id="7202c-214">Exempel</span><span class="sxs-lookup"><span data-stu-id="7202c-214">Sample</span></span>](https://github.com/AppInsightsSamples/WebApi_2.x_UnhandledExceptions)

<span data-ttu-id="7202c-215">Alternativt kan du:</span><span class="sxs-lookup"><span data-stu-id="7202c-215">As alternatives, you could:</span></span>

1. <span data-ttu-id="7202c-216">Ersätt endast ExceptionHandler med en anpassad implementering av IExceptionHandler.</span><span class="sxs-lookup"><span data-stu-id="7202c-216">Replace the only ExceptionHandler with a custom implementation of IExceptionHandler.</span></span> <span data-ttu-id="7202c-217">Detta är endast anropas när ramen är fortfarande kunna välja vilken svarsmeddelande att skicka (inte när anslutningen avbryts exempelvis)</span><span class="sxs-lookup"><span data-stu-id="7202c-217">This is only called when the framework is still able to choose which response message to send (not when the connection is aborted for instance)</span></span>
2. <span data-ttu-id="7202c-218">Undantagsfilter (enligt beskrivningen i avsnittet Web API 1.x domänkontrollanter ovan) - inte anropas i samtliga fall.</span><span class="sxs-lookup"><span data-stu-id="7202c-218">Exception Filters (as described in the section on Web API 1.x controllers above) - not called in all cases.</span></span>

## <a name="wcf"></a><span data-ttu-id="7202c-219">WCF</span><span class="sxs-lookup"><span data-stu-id="7202c-219">WCF</span></span>
<span data-ttu-id="7202c-220">Lägg till en klass som utökar Attribute och implementerar IErrorHandler och IServiceBehavior.</span><span class="sxs-lookup"><span data-stu-id="7202c-220">Add a class that extends Attribute and implements IErrorHandler and IServiceBehavior.</span></span>

    using System;
    using System.Collections.Generic;
    using System.Linq;
    using System.ServiceModel.Description;
    using System.ServiceModel.Dispatcher;
    using System.Web;
    using Microsoft.ApplicationInsights;

    namespace WcfService4.ErrorHandling
    {
      public class AiLogExceptionAttribute : Attribute, IErrorHandler, IServiceBehavior
      {
        public void AddBindingParameters(ServiceDescription serviceDescription,
            System.ServiceModel.ServiceHostBase serviceHostBase,
            System.Collections.ObjectModel.Collection<ServiceEndpoint> endpoints,
            System.ServiceModel.Channels.BindingParameterCollection bindingParameters)
        {
        }

        public void ApplyDispatchBehavior(ServiceDescription serviceDescription,
            System.ServiceModel.ServiceHostBase serviceHostBase)
        {
            foreach (ChannelDispatcher disp in serviceHostBase.ChannelDispatchers)
            {
                disp.ErrorHandlers.Add(this);
            }
        }

        public void Validate(ServiceDescription serviceDescription,
            System.ServiceModel.ServiceHostBase serviceHostBase)
        {
        }

        bool IErrorHandler.HandleError(Exception error)
        {//or reuse instance (recommended!). see note above
            var ai = new TelemetryClient();

            ai.TrackException(error);
            return false;
        }

        void IErrorHandler.ProvideFault(Exception error,
            System.ServiceModel.Channels.MessageVersion version,
            ref System.ServiceModel.Channels.Message fault)
        {
        }
      }
    }

<span data-ttu-id="7202c-221">Lägg till attributet implementeringar tjänsten:</span><span class="sxs-lookup"><span data-stu-id="7202c-221">Add the attribute to the service implementations:</span></span>

    namespace WcfService4
    {
        [AiLogException]
        public class Service1 : IService1
        {
         ...

[<span data-ttu-id="7202c-222">Exempel</span><span class="sxs-lookup"><span data-stu-id="7202c-222">Sample</span></span>](https://github.com/AppInsightsSamples/WCFUnhandledExceptions)

## <a name="exception-performance-counters"></a><span data-ttu-id="7202c-223">Prestandaräknare för undantag</span><span class="sxs-lookup"><span data-stu-id="7202c-223">Exception performance counters</span></span>
<span data-ttu-id="7202c-224">Om du har [installerat Application Insights Agent](app-insights-monitor-performance-live-website-now.md) på servern, du kan få ett diagram över undantag hastighet, mätt av .NET.</span><span class="sxs-lookup"><span data-stu-id="7202c-224">If you have [installed the Application Insights Agent](app-insights-monitor-performance-live-website-now.md) on your server, you can get a chart of the exceptions rate, measured by .NET.</span></span> <span data-ttu-id="7202c-225">Detta inkluderar både hanterade och ohanterade undantag för .NET.</span><span class="sxs-lookup"><span data-stu-id="7202c-225">This includes both handled and unhandled .NET exceptions.</span></span>

<span data-ttu-id="7202c-226">Öppna ett mått Explorer-bladet, lägga till ett nytt diagram och välj **undantag hastighet**, listade under prestandaräknare.</span><span class="sxs-lookup"><span data-stu-id="7202c-226">Open a Metric Explorer blade, add a new chart, and select **Exception rate**, listed under Performance Counters.</span></span>

<span data-ttu-id="7202c-227">.NET framework beräknar hastigheten genom att räkna antalet undantag i ett intervall och dividera med längden på intervallet.</span><span class="sxs-lookup"><span data-stu-id="7202c-227">The .NET framework calculates the rate by counting the number of exceptions in an interval and dividing by the length of the interval.</span></span>

<span data-ttu-id="7202c-228">Observera att det är samma som antalet 'Undantag' beräknas av Application Insights-portalen genom att räkna TrackException rapporter.</span><span class="sxs-lookup"><span data-stu-id="7202c-228">Note that it will be different from the 'Exceptions' count calculated by the Application Insights portal by counting TrackException reports.</span></span> <span data-ttu-id="7202c-229">Insamlingsintervallen är olika och SDK skickar inte TrackException rapporter för alla hanterade och ohanterade undantag.</span><span class="sxs-lookup"><span data-stu-id="7202c-229">The sampling intervals are different, and the SDK doesn't send TrackException reports for all handled and unhandled exceptions.</span></span>

## <a name="video"></a><span data-ttu-id="7202c-230">Video</span><span class="sxs-lookup"><span data-stu-id="7202c-230">Video</span></span>

> [!VIDEO https://channel9.msdn.com/events/Connect/2016/112/player] 

## <a name="next-steps"></a><span data-ttu-id="7202c-231">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="7202c-231">Next steps</span></span>
* [<span data-ttu-id="7202c-232">Övervaka REST, SQL och andra anrop till beroenden</span><span class="sxs-lookup"><span data-stu-id="7202c-232">Monitor REST, SQL and other calls to dependencies</span></span>](app-insights-asp-net-dependencies.md)
* [<span data-ttu-id="7202c-233">Övervaka sidinläsningstider, webbläsarundantag och AJAX-anrop</span><span class="sxs-lookup"><span data-stu-id="7202c-233">Monitor page load times, browser exceptions, and AJAX calls</span></span>](app-insights-javascript.md)
* [<span data-ttu-id="7202c-234">Övervaka prestandaräknare</span><span class="sxs-lookup"><span data-stu-id="7202c-234">Monitor performance counters</span></span>](app-insights-performance-counters.md)
