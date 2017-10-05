---
title: "Övervaka hälsotillstånd och användning med Application Insights för din app"
description: "Kom igång med Application Insights. Analysera användning, tillgänglighet och prestanda för din lokala eller Microsoft Azure-program."
services: application-insights
documentationcenter: 
author: CFreemanwa
manager: carmonm
ms.assetid: 40650472-e860-4c1b-a589-9956245df307
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 11/25/2015
ms.author: bwren
ms.openlocfilehash: 5b7b1f4a53cd2624ee8e2ab684ba6ba63252674f
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/18/2017
---
# <a name="monitor-performance-in-web-applications"></a><span data-ttu-id="4f3ce-104">Övervaka prestanda i webbprogram</span><span class="sxs-lookup"><span data-stu-id="4f3ce-104">Monitor performance in web applications</span></span>


<span data-ttu-id="4f3ce-105">Kontrollera att programmet fungerar bra och lär dig snabbt eventuella fel.</span><span class="sxs-lookup"><span data-stu-id="4f3ce-105">Make sure your application is performing well, and find out quickly about any failures.</span></span> <span data-ttu-id="4f3ce-106">[Application Insights] [ start] information om eventuella prestandaproblem och undantag, och hjälper dig att hitta och diagnostisera orsaken.</span><span class="sxs-lookup"><span data-stu-id="4f3ce-106">[Application Insights][start] will tell you about any performance issues and exceptions, and help you find and diagnose the root causes.</span></span>

<span data-ttu-id="4f3ce-107">Application Insights kan övervaka Java- och ASP.NET-webbprogram och tjänster, WCF-tjänster.</span><span class="sxs-lookup"><span data-stu-id="4f3ce-107">Application Insights can monitor both Java and ASP.NET web applications and services, WCF services.</span></span> <span data-ttu-id="4f3ce-108">De kan vara lokalt, på virtuella datorer eller som Microsoft Azure-webbplatser.</span><span class="sxs-lookup"><span data-stu-id="4f3ce-108">They can be hosted on-premises, on virtual machines, or as Microsoft Azure websites.</span></span> 

<span data-ttu-id="4f3ce-109">På klientsidan ta Programinsikter telemetri från webbsidor och en mängd olika enheter, inklusive iOS, Android och Windows Store-appar.</span><span class="sxs-lookup"><span data-stu-id="4f3ce-109">On the client side, Application Insights can take telemetry from web pages and a wide variety of devices including iOS, Android and Windows Store apps.</span></span>

>[!Note]
> <span data-ttu-id="4f3ce-110">Vi har gjort en ny miljö för att hitta långsamt utför sidor i ditt webbprogram.</span><span class="sxs-lookup"><span data-stu-id="4f3ce-110">We have made a new experience available for finding slow performing pages in your web application.</span></span> <span data-ttu-id="4f3ce-111">Om du inte har åtkomst till den, aktivera den genom att konfigurera alternativen preview med den [Preview bladet](app-insights-previews.md).</span><span class="sxs-lookup"><span data-stu-id="4f3ce-111">If you don't have access to it, enable it by configuring your preview options with the [Preview blade](app-insights-previews.md).</span></span> <span data-ttu-id="4f3ce-112">Läs mer om den nya upplevelsen i [hitta och åtgärda flaskhalsar i interaktiva prestanda undersökningen](#Find-and-fix-performance-bottlenecks-with-an-interactive-Performance-investigation).</span><span class="sxs-lookup"><span data-stu-id="4f3ce-112">Read about this new experience in [Find and fix performance bottlenecks with the interactive Performance investigation](#Find-and-fix-performance-bottlenecks-with-an-interactive-Performance-investigation).</span></span>

## <span data-ttu-id="4f3ce-113"><a name="setup"></a>Konfigurera övervakning av programprestanda</span><span class="sxs-lookup"><span data-stu-id="4f3ce-113"><a name="setup"></a>Set up performance monitoring</span></span>
<span data-ttu-id="4f3ce-114">Om du inte har lagt till Application Insights i projektet (om det inte har ApplicationInsights.config), väljer du något av följande sätt att komma igång:</span><span class="sxs-lookup"><span data-stu-id="4f3ce-114">If you haven't yet added Application Insights to your project (that is, if it doesn't have ApplicationInsights.config), choose one of these ways to get started:</span></span>

* [<span data-ttu-id="4f3ce-115">ASP.NET-webbprogram</span><span class="sxs-lookup"><span data-stu-id="4f3ce-115">ASP.NET web apps</span></span>](app-insights-asp-net.md)
  * [<span data-ttu-id="4f3ce-116">Lägg till undantagsövervakning</span><span class="sxs-lookup"><span data-stu-id="4f3ce-116">Add exception monitoring</span></span>](app-insights-asp-net-exceptions.md)
  * [<span data-ttu-id="4f3ce-117">Lägg till beroende övervakning</span><span class="sxs-lookup"><span data-stu-id="4f3ce-117">Add dependency monitoring</span></span>](app-insights-monitor-performance-live-website-now.md)
* [<span data-ttu-id="4f3ce-118">J2EE-webbprogram</span><span class="sxs-lookup"><span data-stu-id="4f3ce-118">J2EE web apps</span></span>](app-insights-java-get-started.md)
  * [<span data-ttu-id="4f3ce-119">Lägg till beroende övervakning</span><span class="sxs-lookup"><span data-stu-id="4f3ce-119">Add dependency monitoring</span></span>](app-insights-java-agent.md)

## <span data-ttu-id="4f3ce-120"><a name="view"></a>Utforska prestandamått</span><span class="sxs-lookup"><span data-stu-id="4f3ce-120"><a name="view"></a>Exploring performance metrics</span></span>
<span data-ttu-id="4f3ce-121">I [Azure-portalen](https://portal.azure.com), bläddra till Application Insights-resursen som du har skapat för ditt program.</span><span class="sxs-lookup"><span data-stu-id="4f3ce-121">In [the Azure portal](https://portal.azure.com), browse to the Application Insights resource that you set up for your application.</span></span> <span data-ttu-id="4f3ce-122">Översikt över bladet visar grundläggande prestandadata:</span><span class="sxs-lookup"><span data-stu-id="4f3ce-122">The overview blade shows basic performance data:</span></span>

<span data-ttu-id="4f3ce-123">Klicka på diagram finns mer information och visa resultat för en längre period.</span><span class="sxs-lookup"><span data-stu-id="4f3ce-123">Click any chart to see more detail, and to see results for a longer period.</span></span> <span data-ttu-id="4f3ce-124">Exempel på panelen begäranden och välj sedan ett tidsintervall:</span><span class="sxs-lookup"><span data-stu-id="4f3ce-124">For example, click the Requests tile and then select a time range:</span></span>

![Klicka här för att mer data och välj ett tidsintervall](./media/app-insights-web-monitor-performance/appinsights-48metrics.png)

<span data-ttu-id="4f3ce-126">Klicka på ett diagram om du vill välja vilka mått som visas, eller lägga till ett nytt diagram och välja dess mått:</span><span class="sxs-lookup"><span data-stu-id="4f3ce-126">Click a chart to choose which metrics it displays, or add a new chart and select its metrics:</span></span>

![Klicka på ett diagram om du vill välja mått](./media/app-insights-web-monitor-performance/appinsights-61perfchoices.png)

> [!NOTE]
> <span data-ttu-id="4f3ce-128">**Avmarkera alla mätvärden** Visa fullständig urval som är tillgänglig.</span><span class="sxs-lookup"><span data-stu-id="4f3ce-128">**Uncheck all the metrics** to see the full selection that is available.</span></span> <span data-ttu-id="4f3ce-129">Mätvärdena är indelade i grupper. När alla medlemmar i en grupp väljs, visas bara andra medlemmar av gruppen.</span><span class="sxs-lookup"><span data-stu-id="4f3ce-129">The metrics fall into groups; when any member of a group is selected, only the other members of that group appear.</span></span>

## <span data-ttu-id="4f3ce-130"><a name="metrics"></a>Vad betyder det alla?</span><span class="sxs-lookup"><span data-stu-id="4f3ce-130"><a name="metrics"></a>What does it all mean?</span></span> <span data-ttu-id="4f3ce-131">Prestanda paneler och rapporter</span><span class="sxs-lookup"><span data-stu-id="4f3ce-131">Performance tiles and reports</span></span>
<span data-ttu-id="4f3ce-132">Det finns olika prestandamått som du kan hämta.</span><span class="sxs-lookup"><span data-stu-id="4f3ce-132">There are various performance metrics you can get.</span></span> <span data-ttu-id="4f3ce-133">Vi börjar med de som visas som standard på bladet program.</span><span class="sxs-lookup"><span data-stu-id="4f3ce-133">Let's start with those that appear by default on the application blade.</span></span>

### <a name="requests"></a><span data-ttu-id="4f3ce-134">Begäranden</span><span class="sxs-lookup"><span data-stu-id="4f3ce-134">Requests</span></span>
<span data-ttu-id="4f3ce-135">Antal HTTP-begäranden tas emot i en angiven period.</span><span class="sxs-lookup"><span data-stu-id="4f3ce-135">The number of HTTP requests received in a specified period.</span></span> <span data-ttu-id="4f3ce-136">Jämför detta med resultat på andra rapporter för att se hur din app fungerar som belastningen varierar.</span><span class="sxs-lookup"><span data-stu-id="4f3ce-136">Compare this with the results on other reports to see how your app behaves as the load varies.</span></span>

<span data-ttu-id="4f3ce-137">HTTP-begäranden som innehåller alla begäranden om sidor, data och avbildningar som GET eller POST.</span><span class="sxs-lookup"><span data-stu-id="4f3ce-137">HTTP requests include all GET or POST requests for pages, data, and images.</span></span>

<span data-ttu-id="4f3ce-138">Klicka på rutan för att få fram för specifika URL: er.</span><span class="sxs-lookup"><span data-stu-id="4f3ce-138">Click on the tile to get counts for specific URLs.</span></span>

### <a name="average-response-time"></a><span data-ttu-id="4f3ce-139">Genomsnittlig svarstid</span><span class="sxs-lookup"><span data-stu-id="4f3ce-139">Average response time</span></span>
<span data-ttu-id="4f3ce-140">Mäter tiden mellan en webbegäran att ange ditt program och svaret returneras.</span><span class="sxs-lookup"><span data-stu-id="4f3ce-140">Measures the time between a web request entering your application and the response being returned.</span></span>

<span data-ttu-id="4f3ce-141">Punkterna visar det genomsnittliga glidande dem.</span><span class="sxs-lookup"><span data-stu-id="4f3ce-141">The points show a moving average.</span></span> <span data-ttu-id="4f3ce-142">Om det finns en mängd begäranden, kan det finnas några som avviker från medelvärdet utan en uppenbara belastning eller sjunker i diagrammet.</span><span class="sxs-lookup"><span data-stu-id="4f3ce-142">If there are a lot of requests, there might be some that deviate from the average without an obvious peak or dip in the graph.</span></span>

<span data-ttu-id="4f3ce-143">Leta efter ovanliga toppar.</span><span class="sxs-lookup"><span data-stu-id="4f3ce-143">Look for unusual peaks.</span></span> <span data-ttu-id="4f3ce-144">I allmänhet förvänta svarstid för att öka med en ökning av begäranden.</span><span class="sxs-lookup"><span data-stu-id="4f3ce-144">In general, expect response time to rise with a rise in requests.</span></span> <span data-ttu-id="4f3ce-145">Om ökar oproportionerligt att din app träffa en resursgräns t.ex CPU eller kapaciteten för en tjänst som används.</span><span class="sxs-lookup"><span data-stu-id="4f3ce-145">If the rise is disproportionate, your app might be hitting a resource limit such as CPU or the capacity of a service it uses.</span></span>

<span data-ttu-id="4f3ce-146">Klicka på panelen om du vill hämta gånger för specifika URL: er.</span><span class="sxs-lookup"><span data-stu-id="4f3ce-146">Click the tile to get times for specific URLs.</span></span>

![](./media/app-insights-web-monitor-performance/appinsights-42reqs.png)

### <a name="slowest-requests"></a><span data-ttu-id="4f3ce-147">Långsammaste begäranden</span><span class="sxs-lookup"><span data-stu-id="4f3ce-147">Slowest requests</span></span>
![](./media/app-insights-web-monitor-performance/appinsights-44slowest.png)

<span data-ttu-id="4f3ce-148">Visar vilka begäranden kan behöva prestandajustering.</span><span class="sxs-lookup"><span data-stu-id="4f3ce-148">Shows which requests might need performance tuning.</span></span>

### <a name="failed-requests"></a><span data-ttu-id="4f3ce-149">Misslyckade begäranden</span><span class="sxs-lookup"><span data-stu-id="4f3ce-149">Failed requests</span></span>
![](./media/app-insights-web-monitor-performance/appinsights-46failed.png)

<span data-ttu-id="4f3ce-150">Antalet begäranden som utlöste undantagsfel utan felhantering.</span><span class="sxs-lookup"><span data-stu-id="4f3ce-150">A count of requests that threw uncaught exceptions.</span></span>

<span data-ttu-id="4f3ce-151">Klicka på panelen om du vill se information om specifika problem och välj en enskild begäran kan se dess detaljer.</span><span class="sxs-lookup"><span data-stu-id="4f3ce-151">Click the tile to see the details of specific failures, and select an individual request to see its detail.</span></span> 

<span data-ttu-id="4f3ce-152">Ett representativt urval fel sparas för enskilda kontroll.</span><span class="sxs-lookup"><span data-stu-id="4f3ce-152">Only a representative sample of failures is retained for individual inspection.</span></span>

### <a name="other-metrics"></a><span data-ttu-id="4f3ce-153">Andra mått</span><span class="sxs-lookup"><span data-stu-id="4f3ce-153">Other metrics</span></span>
<span data-ttu-id="4f3ce-154">Om du vill se vad anges andra mått som du kan visa, klicka på ett diagram och sedan avmarkera alla mått att visa hela tillgängliga.</span><span class="sxs-lookup"><span data-stu-id="4f3ce-154">To see what other metrics you can display, click a graph, and then deselect all the metrics to see the full available set.</span></span> <span data-ttu-id="4f3ce-155">Klicka på (i) om du vill se varje mått definition.</span><span class="sxs-lookup"><span data-stu-id="4f3ce-155">Click (i) to see each metric's definition.</span></span>

![Avmarkera alla mått för att se hela uppsättningen](./media/app-insights-web-monitor-performance/appinsights-62allchoices.png)

<span data-ttu-id="4f3ce-157">Om du väljer alla mått inaktiveras andra som inte får finnas i samma schema.</span><span class="sxs-lookup"><span data-stu-id="4f3ce-157">Selecting any metric disables the others that can't appear on the same chart.</span></span>

## <a name="set-alerts"></a><span data-ttu-id="4f3ce-158">Ange aviseringar</span><span class="sxs-lookup"><span data-stu-id="4f3ce-158">Set alerts</span></span>
<span data-ttu-id="4f3ce-159">Lägga till en avisering om du vill meddelas via e-post med ovanliga värden för alla mått.</span><span class="sxs-lookup"><span data-stu-id="4f3ce-159">To be notified by email of unusual values of any metric, add an alert.</span></span> <span data-ttu-id="4f3ce-160">Du kan välja att skicka e-postmeddelandet till kontoadministratörer eller till specifika e-postadresser.</span><span class="sxs-lookup"><span data-stu-id="4f3ce-160">You can choose either to send the email to the account administrators, or to specific email addresses.</span></span>

![](./media/app-insights-web-monitor-performance/appinsights-413setMetricAlert.png)

<span data-ttu-id="4f3ce-161">Ange resursen innan de andra egenskaperna.</span><span class="sxs-lookup"><span data-stu-id="4f3ce-161">Set the resource before the other properties.</span></span> <span data-ttu-id="4f3ce-162">Välj inte webtest resurser om du vill ställa in varningar på prestanda eller användningsområden mätvärden.</span><span class="sxs-lookup"><span data-stu-id="4f3ce-162">Don't choose the webtest resources if you want to set alerts on performance or usage metrics.</span></span>

<span data-ttu-id="4f3ce-163">Var noga med att känna till de enheter där du uppmanas att ange ett tröskelvärde.</span><span class="sxs-lookup"><span data-stu-id="4f3ce-163">Be careful to note the units in which you're asked to enter the threshold value.</span></span>

<span data-ttu-id="4f3ce-164">*Knappen Lägg till avisering visas inte.*</span><span class="sxs-lookup"><span data-stu-id="4f3ce-164">*I don't see the Add Alert button.*</span></span> <span data-ttu-id="4f3ce-165">-Är detta en grupp som du har skrivskyddad åtkomst till kontot?</span><span class="sxs-lookup"><span data-stu-id="4f3ce-165">- Is this a group account to which you have read-only access?</span></span> <span data-ttu-id="4f3ce-166">Kontakta kontoadministratören.</span><span class="sxs-lookup"><span data-stu-id="4f3ce-166">Check with the account administrator.</span></span>

## <span data-ttu-id="4f3ce-167"><a name="diagnosis"></a>Diagnostisera problem</span><span class="sxs-lookup"><span data-stu-id="4f3ce-167"><a name="diagnosis"></a>Diagnosing issues</span></span>
<span data-ttu-id="4f3ce-168">Här följer några tips för att hitta och diagnostisera prestandaproblem:</span><span class="sxs-lookup"><span data-stu-id="4f3ce-168">Here are a few tips for finding and diagnosing performance issues:</span></span>

* <span data-ttu-id="4f3ce-169">Ställ in [webbtester] [ availability] om webbplatsen kraschar eller svarar långsamt eller felaktigt.</span><span class="sxs-lookup"><span data-stu-id="4f3ce-169">Set up [web tests][availability] to be alerted if your web site goes down or responds incorrectly or slowly.</span></span> 
* <span data-ttu-id="4f3ce-170">Jämför antalet begäranden med andra mått för att se om misslyckade eller långsamma svar är relaterade till att läsa in.</span><span class="sxs-lookup"><span data-stu-id="4f3ce-170">Compare the Request count with other metrics to see if failures or slow response are related to load.</span></span>
* <span data-ttu-id="4f3ce-171">[Infoga och Sök trace-satser] [ diagnostic] i koden för att identifiera problem.</span><span class="sxs-lookup"><span data-stu-id="4f3ce-171">[Insert and search trace statements][diagnostic] in your code to help pinpoint problems.</span></span>
* <span data-ttu-id="4f3ce-172">Övervaka ditt webbprogram i åtgärden med [direktsänd dataström med mått][livestream].</span><span class="sxs-lookup"><span data-stu-id="4f3ce-172">Monitor your Web app in operation with [Live Metrics Stream][livestream].</span></span>
* <span data-ttu-id="4f3ce-173">Spara tillståndet för .net-program med [ögonblicksbild felsökare][snapshot].</span><span class="sxs-lookup"><span data-stu-id="4f3ce-173">Capture the state of your .Net application with [Snapshot Debugger][snapshot].</span></span>

## <a name="find-and-fix-performance-bottlenecks-with-an-interactive-performance-investigation"></a><span data-ttu-id="4f3ce-174">Hitta och åtgärda flaskhalsar med en interaktiv prestanda undersökning</span><span class="sxs-lookup"><span data-stu-id="4f3ce-174">Find and fix performance bottlenecks with an interactive performance investigation</span></span>

<span data-ttu-id="4f3ce-175">Du kan använda den nya Application Insights interaktiva prestanda undersökningen för att hitta områden av ditt webbprogram som saktar ned prestandan.</span><span class="sxs-lookup"><span data-stu-id="4f3ce-175">You can use the new Application Insights interactive performance investigation to locate areas of your Web app that are slowing down overall performance.</span></span> <span data-ttu-id="4f3ce-176">Du kan snabbt hitta vissa sidor som saktar ned och använder den [Profiling verktyget](app-insights-profiler.md) om det finns en korrelation mellan dessa sidor.</span><span class="sxs-lookup"><span data-stu-id="4f3ce-176">You can quickly find specific pages that are slowing down, and use the [Profiling tool](app-insights-profiler.md) to see if there is a correlation between these pages.</span></span>

### <a name="create-a-list-of-slow-performing-pages"></a><span data-ttu-id="4f3ce-177">Skapa en lista över långsamma prestanda sidor</span><span class="sxs-lookup"><span data-stu-id="4f3ce-177">Create a list of slow performing pages</span></span> 

<span data-ttu-id="4f3ce-178">Det första steget för att söka efter prestandaproblem är att hämta en lista över långsamma svarar sidor.</span><span class="sxs-lookup"><span data-stu-id="4f3ce-178">The first step for finding performance issues is to get a list of the slow responding pages.</span></span> <span data-ttu-id="4f3ce-179">Skärmbilden nedan visar hur du använder bladet prestanda för att hämta en lista över möjliga sidor för att undersöka vidare.</span><span class="sxs-lookup"><span data-stu-id="4f3ce-179">The screen shot below demonstrates using the Performance blade to get a list of potential pages to investigate further.</span></span> <span data-ttu-id="4f3ce-180">Du kan snabbt se från den här sidan att det var en nedgång svarstiden för appen på cirka 6:00 PM och igen på cirka 10 PM.</span><span class="sxs-lookup"><span data-stu-id="4f3ce-180">You can quickly see from this page that there was a slow-down in the response time of the app at approximately 6:00 PM and again at approximately 10 PM.</span></span> <span data-ttu-id="4f3ce-181">Du kan också se att hämta kundinformation /-åtgärden har vissa långvariga åtgärder med en median svarstid 507.05 millisekunder.</span><span class="sxs-lookup"><span data-stu-id="4f3ce-181">You can also see that the GET customer/details operation had some long running operations with a median response time of 507.05 milliseconds.</span></span> 

![Application Insights interaktiva prestanda](./media/app-insights-web-monitor-performance/performance1.png)

### <a name="drill-down-on-specific-pages"></a><span data-ttu-id="4f3ce-183">Detaljnivån på specifika sidor</span><span class="sxs-lookup"><span data-stu-id="4f3ce-183">Drill down on specific pages</span></span>

<span data-ttu-id="4f3ce-184">När du har en ögonblicksbild av appens prestanda kan få du mer information på specifika åtgärder för långsamma prestanda.</span><span class="sxs-lookup"><span data-stu-id="4f3ce-184">Once you have a snapshot of your app's performance, you can get more details on specific slow-performing operations.</span></span> <span data-ttu-id="4f3ce-185">Klicka på alla åtgärder i listan visas information som visas nedan.</span><span class="sxs-lookup"><span data-stu-id="4f3ce-185">Click on any operation in the list to see the details as shown below.</span></span> <span data-ttu-id="4f3ce-186">Du kan se om prestanda var baserat på ett beroende i diagrammet.</span><span class="sxs-lookup"><span data-stu-id="4f3ce-186">From the chart you can see if the performance was based on a dependency.</span></span> <span data-ttu-id="4f3ce-187">Du kan också se hur många användare haft olika svarstider.</span><span class="sxs-lookup"><span data-stu-id="4f3ce-187">You can also see how many users experienced the various response times.</span></span> 

![Application Insights operations bladet](./media/app-insights-web-monitor-performance/performance5.png)

### <a name="drill-down-on-a-specific-time-period"></a><span data-ttu-id="4f3ce-189">Detaljnivån för en viss tidsperiod</span><span class="sxs-lookup"><span data-stu-id="4f3ce-189">Drill down on a specific time period</span></span>

<span data-ttu-id="4f3ce-190">När du har identifierat en punkt i tid att undersöka, detaljnivån även titta på åtgärderna som kan ha orsakat prestanda sidåtergivningen.</span><span class="sxs-lookup"><span data-stu-id="4f3ce-190">After you have identified a point in time to investigate, drill down even further to look at the specific operations that might have caused the performance slow-down.</span></span> <span data-ttu-id="4f3ce-191">När du klickar på en viss punkt i tiden få information om sidan som visas nedan.</span><span class="sxs-lookup"><span data-stu-id="4f3ce-191">When you click on a specific point in time you get the details of the page as shown below.</span></span> <span data-ttu-id="4f3ce-192">I exemplet nedan du kan se de åtgärder som anges för en viss tidsperiod tillsammans med svarskoder server och varaktighet för åtgärden.</span><span class="sxs-lookup"><span data-stu-id="4f3ce-192">In the example below you can see the operations listed for a given time period along with the server response codes and the operation duration.</span></span> <span data-ttu-id="4f3ce-193">Du kan också ha URL: en för att öppna en TFS-arbetsobjekt om du behöver skicka denna information till Utvecklingsteamet.</span><span class="sxs-lookup"><span data-stu-id="4f3ce-193">You also have the url for opening a TFS work item if you need to send this information to your development team.</span></span>

![Tidsintervallet för Application Insights](./media/app-insights-web-monitor-performance/performance2.png)

### <a name="drill-down-on-a-specific-operation"></a><span data-ttu-id="4f3ce-195">Detaljnivån med en specifik åtgärd</span><span class="sxs-lookup"><span data-stu-id="4f3ce-195">Drill down on a specific operation</span></span>

<span data-ttu-id="4f3ce-196">När du har identifierat en punkt i tid att undersöka, detaljnivån även titta på åtgärderna som kan ha orsakat prestanda sidåtergivningen.</span><span class="sxs-lookup"><span data-stu-id="4f3ce-196">After you have identified a point in time to investigate, drill down even further to look at the specific operations that might have caused the performance slow-down.</span></span> <span data-ttu-id="4f3ce-197">Klicka på en åtgärd från listan för att visa detaljer om åtgärden som visas nedan.</span><span class="sxs-lookup"><span data-stu-id="4f3ce-197">Click on an operation from the list to see the details of the operation as shown below.</span></span> <span data-ttu-id="4f3ce-198">I det här exemplet ser du att åtgärden misslyckades och Application Insights tillhandahåller information om undantaget inträffade i programmet.</span><span class="sxs-lookup"><span data-stu-id="4f3ce-198">In this example you can see that the operation failed, and Application Insights has provided the details of the exception the application threw.</span></span> <span data-ttu-id="4f3ce-199">Igen, kan du enkelt skapa en TFS-arbetsobjekt från det här bladet.</span><span class="sxs-lookup"><span data-stu-id="4f3ce-199">Again, you can easily create a TFS work item from this blade.</span></span>

![Application Insights åtgärden bladet](./media/app-insights-web-monitor-performance/performance3.png)


## <span data-ttu-id="4f3ce-201"><a name="next"></a>Nästa steg</span><span class="sxs-lookup"><span data-stu-id="4f3ce-201"><a name="next"></a>Next steps</span></span>
<span data-ttu-id="4f3ce-202">[Webbtester] [ availability] -har webbförfrågningar som skickas till ditt program med jämna mellanrum från hela världen.</span><span class="sxs-lookup"><span data-stu-id="4f3ce-202">[Web tests][availability] - Have web requests sent to your application at regular intervals from around the world.</span></span>

<span data-ttu-id="4f3ce-203">[Fånga och söka diagnostikspår] [ diagnostic] – Infoga spårning av anrop och gå igenom resultat för att identifiera problem.</span><span class="sxs-lookup"><span data-stu-id="4f3ce-203">[Capture and search diagnostic traces][diagnostic] - Insert trace calls and sift through the results to pinpoint issues.</span></span>

<span data-ttu-id="4f3ce-204">[Spårning av användning] [ usage] -ta reda på hur personer använder ditt program.</span><span class="sxs-lookup"><span data-stu-id="4f3ce-204">[Usage tracking][usage] - Find out how people use your application.</span></span>

<span data-ttu-id="4f3ce-205">[Felsökning av] [ qna] - och frågor och svar</span><span class="sxs-lookup"><span data-stu-id="4f3ce-205">[Troubleshooting][qna] - and Q & A</span></span>



<!--Link references-->

[availability]: app-insights-monitor-web-app-availability.md
[diagnostic]: app-insights-diagnostic-search.md
[greenbrown]: app-insights-asp-net.md
[qna]: app-insights-troubleshoot-faq.md
[redfield]: app-insights-monitor-performance-live-website-now.md
[start]: app-insights-overview.md
[usage]: app-insights-web-track-usage.md
[livestream]: app-insights-live-stream.md
[snapshot]: app-insights-snapshot-debugger.md



