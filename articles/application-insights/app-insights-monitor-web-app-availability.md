---
title: "Övervaka tillgänglighet och svarstider på valfri webbplats | Microsoft Docs"
description: "Konfigurera webbtester i Application Insights. Få aviseringar om en webbplats blir otillgänglig eller svarar långsamt."
services: application-insights
documentationcenter: 
author: SoubhagyaDash
manager: carmonm
ms.assetid: 46dc13b4-eb2e-4142-a21c-94a156f760ee
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: get-started-article
ms.date: 05/25/2017
ms.author: bwren
ms.openlocfilehash: 6c7f52fc3998b0b29301206ffbc6a5a0c4134f6a
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/18/2017
---
# <a name="monitor-availability-and-responsiveness-of-any-web-site"></a><span data-ttu-id="a6e3d-104">Övervaka tillgänglighet och svarstider på valfri webbplats</span><span class="sxs-lookup"><span data-stu-id="a6e3d-104">Monitor availability and responsiveness of any web site</span></span>
<span data-ttu-id="a6e3d-105">När du har distribuerat din webbapp eller webbplats till en server kan du konfigurera tester för att övervaka appens tillgänglighet och svarstider.</span><span class="sxs-lookup"><span data-stu-id="a6e3d-105">After you've deployed your web app or web site to any server, you can set up tests to monitor its availability and responsiveness.</span></span> <span data-ttu-id="a6e3d-106">[Azure Application Insights](app-insights-overview.md) skickar begäranden till ditt program med jämna mellanrum från platser över hela världen.</span><span class="sxs-lookup"><span data-stu-id="a6e3d-106">[Azure Application Insights](app-insights-overview.md) sends web requests to your application at regular intervals from points around the world.</span></span> <span data-ttu-id="a6e3d-107">Den varnar dig om programmet inte svarar eller svarar långsamt.</span><span class="sxs-lookup"><span data-stu-id="a6e3d-107">It alerts you if your application doesn't respond, or responds slowly.</span></span>

<span data-ttu-id="a6e3d-108">Du kan konfigurera tillgänglighetstester för valfri HTTP- eller HTTPS-slutpunkt som kan nås från det offentliga Internet.</span><span class="sxs-lookup"><span data-stu-id="a6e3d-108">You can set up availability tests for any HTTP or HTTPS endpoint that is accessible from the public internet.</span></span> <span data-ttu-id="a6e3d-109">Du behöver inte lägga till något till den webbplats som du testar.</span><span class="sxs-lookup"><span data-stu-id="a6e3d-109">You don't have to add anything to the web site you're testing.</span></span> <span data-ttu-id="a6e3d-110">Det behöver inte ens vara din webbplats: du kan testa en REST API-tjänst som du är beroende av.</span><span class="sxs-lookup"><span data-stu-id="a6e3d-110">It doesn't even have to be your site: you could test a REST API service on which you depend.</span></span>

<span data-ttu-id="a6e3d-111">Det finns två typer av tillgänglighetstester:</span><span class="sxs-lookup"><span data-stu-id="a6e3d-111">There are two types of availability tests:</span></span>

* <span data-ttu-id="a6e3d-112">[URL-pingtest](#create): Ett enkelt test som du kan skapa på Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="a6e3d-112">[URL ping test](#create): a simple test that you can create in the Azure portal.</span></span>
* <span data-ttu-id="a6e3d-113">[Flerstegstest för webbplatser](#multi-step-web-tests): Ett test som du skapar i Visual Studio Enterprise och laddar upp till portalen.</span><span class="sxs-lookup"><span data-stu-id="a6e3d-113">[Multi-step web test](#multi-step-web-tests): which you create in Visual Studio Enterprise and upload to the portal.</span></span>

<span data-ttu-id="a6e3d-114">Du kan skapa upp till 25 tillgänglighetstester per programresurs.</span><span class="sxs-lookup"><span data-stu-id="a6e3d-114">You can create up to 25 availability tests per application resource.</span></span>

## <span data-ttu-id="a6e3d-115"><a name="create"></a>1. Öppna en resurs för dina tillgänglighetstestrapporter</span><span class="sxs-lookup"><span data-stu-id="a6e3d-115"><a name="create"></a>1. Open a resource for your availability test reports</span></span>

<span data-ttu-id="a6e3d-116">**Om du redan har konfigurerat Application Insights** för din webbapp öppnar du dess Application Insights-resurs i [Azure Portal](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="a6e3d-116">**If you have already configured Application Insights** for your web app, open its Application Insights resource in the [Azure portal](https://portal.azure.com).</span></span>

<span data-ttu-id="a6e3d-117">**Eller om du vill se dina rapporter i en ny resurs** registrerar du dig för [Microsoft Azure](http://azure.com), går till [Azure Portal](https://portal.azure.com) och skapar en Application Insights-resurs.</span><span class="sxs-lookup"><span data-stu-id="a6e3d-117">**Or, if you want to see your reports in a new resource,** sign up to [Microsoft Azure](http://azure.com), go to the [Azure portal](https://portal.azure.com), and create an Application Insights resource.</span></span>

![Nytt > Application Insights](./media/app-insights-monitor-web-app-availability/11-new-app.png)

<span data-ttu-id="a6e3d-119">Klicka på **alla resurser** för att öppna översiktsbladet för den nya resursen.</span><span class="sxs-lookup"><span data-stu-id="a6e3d-119">Click **All resources** to open the Overview blade for the new resource.</span></span>

## <span data-ttu-id="a6e3d-120"><a name="setup"></a>2. Skapa ett URL-pingtest</span><span class="sxs-lookup"><span data-stu-id="a6e3d-120"><a name="setup"></a>2. Create a URL ping test</span></span>
<span data-ttu-id="a6e3d-121">Öppna bladet Tillgänglighet och lägg till ett test.</span><span class="sxs-lookup"><span data-stu-id="a6e3d-121">Open the Availability blade and add a test.</span></span>

![Fyll åtminstone i URL:en för din webbplats](./media/app-insights-monitor-web-app-availability/13-availability.png)

* <span data-ttu-id="a6e3d-123">**URL: en** kan vara en webbsida som du vill testa, men den måste vara synlig från Internet.</span><span class="sxs-lookup"><span data-stu-id="a6e3d-123">**The URL** can be any web page you want to test, but it must be visible from the public internet.</span></span> <span data-ttu-id="a6e3d-124">URL: en kan innehålla en frågesträng.</span><span class="sxs-lookup"><span data-stu-id="a6e3d-124">The URL can include a query string.</span></span> <span data-ttu-id="a6e3d-125">Du kan arbeta med din databas om du vill.</span><span class="sxs-lookup"><span data-stu-id="a6e3d-125">So, for example, you can exercise your database a little.</span></span> <span data-ttu-id="a6e3d-126">Om URL-adressen matchar en omdirigering följer vi den upp till tio omdirigeringar.</span><span class="sxs-lookup"><span data-stu-id="a6e3d-126">If the URL resolves to a redirect, we follow it up to 10 redirects.</span></span>
* <span data-ttu-id="a6e3d-127">**Parsa beroendebegäranden**: om det här alternativet är markerat begärs bilder, skript, filer och andra filer som ingår i webbsidan under testet.</span><span class="sxs-lookup"><span data-stu-id="a6e3d-127">**Parse dependent requests**: If this option is checked, the test requests images, scripts, style files, and other files that are part of the web page under test.</span></span> <span data-ttu-id="a6e3d-128">Den registrerade svarstiden innefattar den tid det tar att hämta dessa filer.</span><span class="sxs-lookup"><span data-stu-id="a6e3d-128">The recorded response time includes the time taken to get these files.</span></span> <span data-ttu-id="a6e3d-129">Testet misslyckas om dessa resurser inte kan laddas ned inom tidsgränsen för hela testet.</span><span class="sxs-lookup"><span data-stu-id="a6e3d-129">The test fails if all these resources cannot be successfully downloaded within the timeout for the whole test.</span></span> 

    <span data-ttu-id="a6e3d-130">Om alternativet inte är markerat begärs endast filen på den URL som du har angett i testet.</span><span class="sxs-lookup"><span data-stu-id="a6e3d-130">If the option is not checked, the test only requests the file at the URL you specified.</span></span>
* <span data-ttu-id="a6e3d-131">**Aktivera återförsök**: Om det här alternativet är markerat och testet misslyckas görs ett nytt försök efter en liten stund.</span><span class="sxs-lookup"><span data-stu-id="a6e3d-131">**Enable retries**:  If this option is checked, when the test fails, it is retried after a short interval.</span></span> <span data-ttu-id="a6e3d-132">Ett fel rapporteras endast om tre på varandra följande försök misslyckas.</span><span class="sxs-lookup"><span data-stu-id="a6e3d-132">A failure is reported only if three successive attempts fail.</span></span> <span data-ttu-id="a6e3d-133">Efterföljande tester utförs sedan med den vanliga testfrekvensen.</span><span class="sxs-lookup"><span data-stu-id="a6e3d-133">Subsequent tests are then performed at the usual test frequency.</span></span> <span data-ttu-id="a6e3d-134">Återförsök pausas tillfälligt tills nästa lyckade test.</span><span class="sxs-lookup"><span data-stu-id="a6e3d-134">Retry is temporarily suspended until the next success.</span></span> <span data-ttu-id="a6e3d-135">Den här regeln tillämpas separat på varje testplats.</span><span class="sxs-lookup"><span data-stu-id="a6e3d-135">This rule is applied independently at each test location.</span></span> <span data-ttu-id="a6e3d-136">Vi rekommenderar det här alternativet.</span><span class="sxs-lookup"><span data-stu-id="a6e3d-136">We recommend this option.</span></span> <span data-ttu-id="a6e3d-137">I genomsnitt försvinner ca 80 % av felen vid återförsök.</span><span class="sxs-lookup"><span data-stu-id="a6e3d-137">On average, about 80% of failures disappear on retry.</span></span>
* <span data-ttu-id="a6e3d-138">**Testfrekvens**: Anger hur ofta testet körs från varje testplats.</span><span class="sxs-lookup"><span data-stu-id="a6e3d-138">**Test frequency**: Sets how often the test is run from each test location.</span></span> <span data-ttu-id="a6e3d-139">Med en frekvens på fem minuter och fem testplatser testas din webbplats i genomsnitt varje minut.</span><span class="sxs-lookup"><span data-stu-id="a6e3d-139">With a frequency of five minutes and five test locations, your site is tested on average every minute.</span></span>
* <span data-ttu-id="a6e3d-140">**Testplatser** är de platser som våra servrar skickar webbförfrågningar till din URL från.</span><span class="sxs-lookup"><span data-stu-id="a6e3d-140">**Test locations** are the places from where our servers send web requests to your URL.</span></span> <span data-ttu-id="a6e3d-141">Välj mer än en så att du kan skilja mellan problem på din webbplats och nätverksproblem.</span><span class="sxs-lookup"><span data-stu-id="a6e3d-141">Choose more than one so that you can distinguish problems in your website from network issues.</span></span> <span data-ttu-id="a6e3d-142">Du kan välja upp till 16 platser.</span><span class="sxs-lookup"><span data-stu-id="a6e3d-142">You can select up to 16 locations.</span></span>
* <span data-ttu-id="a6e3d-143">**Villkor för lyckad test**:</span><span class="sxs-lookup"><span data-stu-id="a6e3d-143">**Success criteria**:</span></span>

    <span data-ttu-id="a6e3d-144">**Timeout för test**: Minska det här värdet om du vill få aviseringar om långsamma svar.</span><span class="sxs-lookup"><span data-stu-id="a6e3d-144">**Test timeout**: Decrease this value to be alerted about slow responses.</span></span> <span data-ttu-id="a6e3d-145">Testet räknas som misslyckat om svaren från din webbplats inte har tagits emot inom denna period.</span><span class="sxs-lookup"><span data-stu-id="a6e3d-145">The test is counted as a failure if the responses from your site have not been received within this period.</span></span> <span data-ttu-id="a6e3d-146">Om du valde **Parsa beroende begäranden** måste alla bilder, formatfiler, skript och andra beroende resurser ha tagits emot inom denna period.</span><span class="sxs-lookup"><span data-stu-id="a6e3d-146">If you selected **Parse dependent requests**, then all the images, style files, scripts, and other dependent resources must have been received within this period.</span></span>

    <span data-ttu-id="a6e3d-147">**HTTP-svar**: Den returnerade statuskoden som räknas som ett lyckat test.</span><span class="sxs-lookup"><span data-stu-id="a6e3d-147">**HTTP response**: The returned status code that is counted as a success.</span></span> <span data-ttu-id="a6e3d-148">200 är koden som anger att en normal webbsida har returnerats.</span><span class="sxs-lookup"><span data-stu-id="a6e3d-148">200 is the code that indicates that a normal web page has been returned.</span></span>

    <span data-ttu-id="a6e3d-149">**Innehållsmatchning**: en sträng, t.ex. ”Välkommen!”.</span><span class="sxs-lookup"><span data-stu-id="a6e3d-149">**Content match**: a string, like "Welcome!"</span></span> <span data-ttu-id="a6e3d-150">Vi testar i varje svar om någon skiftlägeskänslig matchning förekommer.</span><span class="sxs-lookup"><span data-stu-id="a6e3d-150">We test that an exact case-sensitive match occurs in every response.</span></span> <span data-ttu-id="a6e3d-151">Den måste vara en enkel sträng utan jokertecken.</span><span class="sxs-lookup"><span data-stu-id="a6e3d-151">It must be a plain string, without wildcards.</span></span> <span data-ttu-id="a6e3d-152">Glöm inte att du kan behöva uppdatera sidan om innehållet ändras.</span><span class="sxs-lookup"><span data-stu-id="a6e3d-152">Don't forget that if your page content changes you might have to update it.</span></span>
* <span data-ttu-id="a6e3d-153">**Aviseringar** skickas som standard till dig om det uppstår fel på tre platser under en femminutersperiod.</span><span class="sxs-lookup"><span data-stu-id="a6e3d-153">**Alerts** are, by default, sent to you if there are failures in three locations over five minutes.</span></span> <span data-ttu-id="a6e3d-154">Ett fel på en enda plats är ofta ett nätverksproblem och inte ett problem med din webbplats.</span><span class="sxs-lookup"><span data-stu-id="a6e3d-154">A failure in one location is likely to be a network problem, and not a problem with your site.</span></span> <span data-ttu-id="a6e3d-155">Men du kan ändra tröskelvärdet så att det är mer eller mindre känsligt, och du kan också ändra vem e-postmeddelandena ska skickas till.</span><span class="sxs-lookup"><span data-stu-id="a6e3d-155">But you can change the threshold to be more or less sensitive, and you can also change who the emails should be sent to.</span></span>

    <span data-ttu-id="a6e3d-156">Du kan konfigurera en [webhook](../monitoring-and-diagnostics/insights-webhooks-alerts.md) som anropas när en avisering genereras.</span><span class="sxs-lookup"><span data-stu-id="a6e3d-156">You can set up a [webhook](../monitoring-and-diagnostics/insights-webhooks-alerts.md) that is called when an alert is raised.</span></span> <span data-ttu-id="a6e3d-157">(Observera dock att frågeparametrar inte skickas som egenskaper för närvarande.)</span><span class="sxs-lookup"><span data-stu-id="a6e3d-157">(But note that, at present, query parameters are not passed through as Properties.)</span></span>

### <a name="test-more-urls"></a><span data-ttu-id="a6e3d-158">Testa fler URL:er</span><span class="sxs-lookup"><span data-stu-id="a6e3d-158">Test more URLs</span></span>
<span data-ttu-id="a6e3d-159">Lägg till fler test.</span><span class="sxs-lookup"><span data-stu-id="a6e3d-159">Add more tests.</span></span> <span data-ttu-id="a6e3d-160">Förutom att testa din hemsida kan du till exempel kontrollera att din databas körs genom att testa URL:en för en sökning.</span><span class="sxs-lookup"><span data-stu-id="a6e3d-160">For example, In addition to testing your home page, you can make sure your database is running by testing the URL for a search.</span></span>


## <span data-ttu-id="a6e3d-161"><a name="monitor"></a>3. Visa tillgänglighetstestresultat</span><span class="sxs-lookup"><span data-stu-id="a6e3d-161"><a name="monitor"></a>3. See your availability test results</span></span>

<span data-ttu-id="a6e3d-162">Efter ett par minuter klickar du på **Uppdatera** för att visa testresultaten.</span><span class="sxs-lookup"><span data-stu-id="a6e3d-162">After a few minutes, click **Refresh** to see test results.</span></span> 

![Sammanfattningsresultat på startbladet](./media/app-insights-monitor-web-app-availability/14-availSummary-3.png)

<span data-ttu-id="a6e3d-164">Spridningsdiagrammet visar exempel på testresultat som innehåller information om diagnostiska test.</span><span class="sxs-lookup"><span data-stu-id="a6e3d-164">The scatterplot shows samples of the test results that have diagnostic test-step detail in them.</span></span> <span data-ttu-id="a6e3d-165">Testmotorn lagrar diagnosinformation för tester som innehåller fel.</span><span class="sxs-lookup"><span data-stu-id="a6e3d-165">The test engine stores diagnostic detail for tests that have failures.</span></span> <span data-ttu-id="a6e3d-166">För lyckade tester lagras diagnosinformation för en delmängd av körningarna.</span><span class="sxs-lookup"><span data-stu-id="a6e3d-166">For successful tests, diagnostic details are stored for a subset of the executions.</span></span> <span data-ttu-id="a6e3d-167">För pekaren över någon av de gröna/röda punkterna för att visa tidsstämpel, varaktighet, plats och namn för testet.</span><span class="sxs-lookup"><span data-stu-id="a6e3d-167">Hover over any of the green/red dots to see the test timestamp, test duration, location, and test name.</span></span> <span data-ttu-id="a6e3d-168">Klicka på någon av punkterna i spridningsdiagrammet för att visa detaljerad information om testresultatet.</span><span class="sxs-lookup"><span data-stu-id="a6e3d-168">Click through any dot in the scatter plot to see the details of the test result.</span></span>  

<span data-ttu-id="a6e3d-169">Välj ett visst test, en viss plats eller minska tidsperioden för att visa fler resultat för en viss tidsperiod som du är intresserad av.</span><span class="sxs-lookup"><span data-stu-id="a6e3d-169">Select a particular test, location, or reduce the time period to see more results around the time period of interest.</span></span> <span data-ttu-id="a6e3d-170">Använd Sökutforskaren för att visa resultat från alla körningar, eller använd analysfrågor om du vill köra anpassade rapporter för dessa data.</span><span class="sxs-lookup"><span data-stu-id="a6e3d-170">Use Search Explorer to see results from all executions, or use Analytics queries to run custom reports on this data.</span></span>

<span data-ttu-id="a6e3d-171">Förutom rådataresultat finns även två tillgänglighetsmått i Metrics Explorer:</span><span class="sxs-lookup"><span data-stu-id="a6e3d-171">In addition to the raw results, there are two Availability metrics in Metrics Explorer:</span></span> 

1. <span data-ttu-id="a6e3d-172">Tillgänglighet: antal procent av testerna som lyckades av alla testkörningar.</span><span class="sxs-lookup"><span data-stu-id="a6e3d-172">Availability: Percentage of the tests that were successful, across all test executions.</span></span> 
2. <span data-ttu-id="a6e3d-173">Testets varaktighet: genomsnittlig tid för alla testkörningar.</span><span class="sxs-lookup"><span data-stu-id="a6e3d-173">Test Duration: Average test duration across all test executions.</span></span>

<span data-ttu-id="a6e3d-174">Du kan använda filter för testnamn och plats om du vill analysera trender för ett visst test och/eller en viss plats.</span><span class="sxs-lookup"><span data-stu-id="a6e3d-174">You can apply filters on the test name, location to analyze trends of a particular test and/or location.</span></span>

## <span data-ttu-id="a6e3d-175"><a name="edit"></a>Granska och redigera tester</span><span class="sxs-lookup"><span data-stu-id="a6e3d-175"><a name="edit"></a> Inspect and edit tests</span></span>

<span data-ttu-id="a6e3d-176">Välj ett specifikt test på sammanfattningssidan.</span><span class="sxs-lookup"><span data-stu-id="a6e3d-176">From the summary page, select a specific test.</span></span> <span data-ttu-id="a6e3d-177">Där du kan se specifika resultat för testet och redigera eller tillfälligt inaktivera det.</span><span class="sxs-lookup"><span data-stu-id="a6e3d-177">There, you can see its specific results, and edit or temporarily disable it.</span></span>

![Redigera eller inaktivera ett webbtest](./media/app-insights-monitor-web-app-availability/19-availEdit-3.png)

<span data-ttu-id="a6e3d-179">Du kanske vill inaktivera tillgänglighetstester eller varningsreglerna som är associerade med dem medan du utför underhåll på tjänsten.</span><span class="sxs-lookup"><span data-stu-id="a6e3d-179">You might want to disable availability tests or the alert rules associated with them while you are performing maintenance on your service.</span></span> 

## <span data-ttu-id="a6e3d-180"><a name="failures"></a>Om du ser fel</span><span class="sxs-lookup"><span data-stu-id="a6e3d-180"><a name="failures"></a>If you see failures</span></span>
<span data-ttu-id="a6e3d-181">Klicka på en röd punkt.</span><span class="sxs-lookup"><span data-stu-id="a6e3d-181">Click a red dot.</span></span>

![Klicka på en röd punkt](./media/app-insights-monitor-web-app-availability/open-instance-3.png)


<span data-ttu-id="a6e3d-183">Från ett tillgänglighetstestresultat kan du:</span><span class="sxs-lookup"><span data-stu-id="a6e3d-183">From an availability test result, you can:</span></span>

* <span data-ttu-id="a6e3d-184">Kontrollera de svar som mottas från servern.</span><span class="sxs-lookup"><span data-stu-id="a6e3d-184">Inspect the response received from your server.</span></span>
* <span data-ttu-id="a6e3d-185">Öppna telemetrin som har skickas av serverappen samtidigt som den misslyckades begärandeinstansen behandlas.</span><span class="sxs-lookup"><span data-stu-id="a6e3d-185">Open the telemetry sent by your server app while processing the failed request instance.</span></span>
* <span data-ttu-id="a6e3d-186">Logga ett problem eller arbetsuppgift i Git eller VSTS för att spåra problemet.</span><span class="sxs-lookup"><span data-stu-id="a6e3d-186">Log an issue or work item in Git or VSTS to track the problem.</span></span> <span data-ttu-id="a6e3d-187">Buggen innehåller en länk till den här händelsen.</span><span class="sxs-lookup"><span data-stu-id="a6e3d-187">The bug will contain a link to this event.</span></span>
* <span data-ttu-id="a6e3d-188">Öppna resultatet av webbtestet i Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="a6e3d-188">Open the web test result in Visual Studio.</span></span>


<span data-ttu-id="a6e3d-189">*Ser det okej ut trots att fel har rapporterats?*</span><span class="sxs-lookup"><span data-stu-id="a6e3d-189">*Looks OK but reported as a failure?*</span></span> <span data-ttu-id="a6e3d-190">Kontrollera alla bilder, skript, formatmallar och andra filer som lästs in av sidan.</span><span class="sxs-lookup"><span data-stu-id="a6e3d-190">Check all the images, scripts, style sheets, and any other files loaded by the page.</span></span> <span data-ttu-id="a6e3d-191">Om någon av komponenterna inte kunde läsas in rapporteras testet som misslyckat, även om HTML-huvudsidan kan läsas in korrekt.</span><span class="sxs-lookup"><span data-stu-id="a6e3d-191">If any of them fails, the test is reported as failed, even if the main html page loads OK.</span></span>

<span data-ttu-id="a6e3d-192">*Inga relaterade objekt?*</span><span class="sxs-lookup"><span data-stu-id="a6e3d-192">*No related items?*</span></span> <span data-ttu-id="a6e3d-193">Om du har konfigurerat Application Insights för din app på serversidan kan detta bero på att [sampling](app-insights-sampling.md) pågår.</span><span class="sxs-lookup"><span data-stu-id="a6e3d-193">If you have Application Insights set up for your server-side application, that may be because [sampling](app-insights-sampling.md) is in operation.</span></span> 

## <a name="multi-step-web-tests"></a><span data-ttu-id="a6e3d-194">Webbtester med flera steg</span><span class="sxs-lookup"><span data-stu-id="a6e3d-194">Multi-step web tests</span></span>
<span data-ttu-id="a6e3d-195">Du kan övervaka ett scenario med en serie URL:er.</span><span class="sxs-lookup"><span data-stu-id="a6e3d-195">You can monitor a scenario that involves a sequence of URLs.</span></span> <span data-ttu-id="a6e3d-196">Om du till exempel övervakar en försäljningswebbplats kan du testa att det går att lägga till objekt i kundvagnen korrekt.</span><span class="sxs-lookup"><span data-stu-id="a6e3d-196">For example, if you are monitoring a sales website, you can test that adding items to the shopping cart works correctly.</span></span>

> [!NOTE] 
> <span data-ttu-id="a6e3d-197">Det utgår en avgift för webbtester med flera steg.</span><span class="sxs-lookup"><span data-stu-id="a6e3d-197">There is a charge for multi-step web tests.</span></span> <span data-ttu-id="a6e3d-198">[Prisschema](http://azure.microsoft.com/pricing/details/application-insights/).</span><span class="sxs-lookup"><span data-stu-id="a6e3d-198">[Pricing scheme](http://azure.microsoft.com/pricing/details/application-insights/).</span></span>
> 

<span data-ttu-id="a6e3d-199">Om du vill skapa ett test med flera steg spelar du in scenariot med hjälp av Visual Studio Enterprise och laddar sedan upp inspelningen till Application Insights.</span><span class="sxs-lookup"><span data-stu-id="a6e3d-199">To create a multi-step test, you record the scenario by using Visual Studio Enterprise, and then upload the recording to Application Insights.</span></span> <span data-ttu-id="a6e3d-200">Application Insights spelar upp scenariot i intervall och verifierar svaren.</span><span class="sxs-lookup"><span data-stu-id="a6e3d-200">Application Insights replays the scenario at intervals and verifies the responses.</span></span>

> [!NOTE]
> <span data-ttu-id="a6e3d-201">Du kan inte använda kodade funktioner eller loopar i dina tester.</span><span class="sxs-lookup"><span data-stu-id="a6e3d-201">You can't use coded functions or loops in your tests.</span></span> <span data-ttu-id="a6e3d-202">Testet måste ingå i .webtest-skriptet.</span><span class="sxs-lookup"><span data-stu-id="a6e3d-202">The test must be contained completely in the .webtest script.</span></span> <span data-ttu-id="a6e3d-203">Du kan dock använda standardplugin-program.</span><span class="sxs-lookup"><span data-stu-id="a6e3d-203">However, you can use standard plugins.</span></span>
>

#### <a name="1-record-a-scenario"></a><span data-ttu-id="a6e3d-204">1. Spela in ett scenario</span><span class="sxs-lookup"><span data-stu-id="a6e3d-204">1. Record a scenario</span></span>
<span data-ttu-id="a6e3d-205">Spela in en webbsession med Visual Studio Enterprise.</span><span class="sxs-lookup"><span data-stu-id="a6e3d-205">Use Visual Studio Enterprise to record a web session.</span></span>

1. <span data-ttu-id="a6e3d-206">Skapa ett testprojekt för webbprestanda.</span><span class="sxs-lookup"><span data-stu-id="a6e3d-206">Create a Web performance test project.</span></span>

    ![Skapa ett projekt i Visual Studio Enterprise från mallen Webbprestanda- och inläsningstest.](./media/app-insights-monitor-web-app-availability/appinsights-71webtest-multi-vs-create.png)

 * <span data-ttu-id="a6e3d-208">*Kan du inte se mallen Webbprestanda- och inläsningstest?*</span><span class="sxs-lookup"><span data-stu-id="a6e3d-208">*Don't see the Web Performance and Load Test template?*</span></span> <span data-ttu-id="a6e3d-209">- Stäng Visual Studio Enterprise.</span><span class="sxs-lookup"><span data-stu-id="a6e3d-209">- Close Visual Studio Enterprise.</span></span> <span data-ttu-id="a6e3d-210">Öppna **installationsprogrammet för Visual Studio** för att ändra Visual Studio Enterprise-installationen.</span><span class="sxs-lookup"><span data-stu-id="a6e3d-210">Open **Visual Studio Installer** to modify your Visual Studio Enterprise installation.</span></span> <span data-ttu-id="a6e3d-211">Välj **Web Performance and load testing tools** (Verktyg för webbprestanda- och inläsningstest) under **Individual Components** (Enskilda komponenter).</span><span class="sxs-lookup"><span data-stu-id="a6e3d-211">Under **Individual Components**, select **Web Performance and load testing tools**.</span></span>

2. <span data-ttu-id="a6e3d-212">Öppna filen .webtest och börja inspelningen.</span><span class="sxs-lookup"><span data-stu-id="a6e3d-212">Open the .webtest file and start recording.</span></span>

    ![Öppna filen .webtest och klicka på Spela in.](./media/app-insights-monitor-web-app-availability/appinsights-71webtest-multi-vs-start.png)
3. <span data-ttu-id="a6e3d-214">Utför de användaråtgärder som du vill simulera i testet: öppna webbplatsen, lägg till en produkt i kundvagnen och så vidare.</span><span class="sxs-lookup"><span data-stu-id="a6e3d-214">Do the user actions you want to simulate in your test: open your website, add a product to the cart, and so on.</span></span> <span data-ttu-id="a6e3d-215">Stoppa sedan testet.</span><span class="sxs-lookup"><span data-stu-id="a6e3d-215">Then stop your test.</span></span>

    ![Inspelningsverktyget för webbtestet körs i Internet Explorer.](./media/app-insights-monitor-web-app-availability/appinsights-71webtest-multi-vs-record.png)

    <span data-ttu-id="a6e3d-217">Håll scenariot kort.</span><span class="sxs-lookup"><span data-stu-id="a6e3d-217">Don't make a long scenario.</span></span> <span data-ttu-id="a6e3d-218">Det finns en gräns på 100 steg och 2 minuter.</span><span class="sxs-lookup"><span data-stu-id="a6e3d-218">There's a limit of 100 steps and 2 minutes.</span></span>
4. <span data-ttu-id="a6e3d-219">Redigera testet om du vill:</span><span class="sxs-lookup"><span data-stu-id="a6e3d-219">Edit the test to:</span></span>

   * <span data-ttu-id="a6e3d-220">Lägg till valideringar för att kontrollera texten som tas emot och svarskoderna.</span><span class="sxs-lookup"><span data-stu-id="a6e3d-220">Add validations to check the received text and response codes.</span></span>
   * <span data-ttu-id="a6e3d-221">Ta bort eventuella överflödiga interaktioner.</span><span class="sxs-lookup"><span data-stu-id="a6e3d-221">Remove any superfluous interactions.</span></span> <span data-ttu-id="a6e3d-222">Du kan också ta bort beroende förfrågningar för bilder eller till annons- eller spårningswebbplatser.</span><span class="sxs-lookup"><span data-stu-id="a6e3d-222">You could also remove dependent requests for pictures or to ad or tracking sites.</span></span>

     <span data-ttu-id="a6e3d-223">Kom ihåg att du bara kan redigera testskriptet – du kan inte lägga till anpassad kod eller anropa andra webbtester.</span><span class="sxs-lookup"><span data-stu-id="a6e3d-223">Remember that you can only edit the test script - you can't add custom code or call other web tests.</span></span> <span data-ttu-id="a6e3d-224">Infoga inte loopar i testet.</span><span class="sxs-lookup"><span data-stu-id="a6e3d-224">Don't insert loops in the test.</span></span> <span data-ttu-id="a6e3d-225">Du kan använda standard-plugin-program för webbtester.</span><span class="sxs-lookup"><span data-stu-id="a6e3d-225">You can use standard web test plug-ins.</span></span>
5. <span data-ttu-id="a6e3d-226">Kör testet i Visual Studio för att kontrollera att det fungerar.</span><span class="sxs-lookup"><span data-stu-id="a6e3d-226">Run the test in Visual Studio to make sure it works.</span></span>

    <span data-ttu-id="a6e3d-227">En webbläsare öppnas och de åtgärder som du har spelat in upprepas.</span><span class="sxs-lookup"><span data-stu-id="a6e3d-227">The web test runner opens a web browser and repeats the actions you recorded.</span></span> <span data-ttu-id="a6e3d-228">Kontrollera att det fungerar som förväntat.</span><span class="sxs-lookup"><span data-stu-id="a6e3d-228">Make sure it works as you expect.</span></span>

    ![Öppna filen .webtest i Visual Studio och klicka på Kör.](./media/app-insights-monitor-web-app-availability/appinsights-71webtest-multi-vs-run.png)

#### <a name="2-upload-the-web-test-to-application-insights"></a><span data-ttu-id="a6e3d-230">2. Ladda upp webbtestet till Application Insights</span><span class="sxs-lookup"><span data-stu-id="a6e3d-230">2. Upload the web test to Application Insights</span></span>
1. <span data-ttu-id="a6e3d-231">Skapa ett webbtest på Application Insights-portalen.</span><span class="sxs-lookup"><span data-stu-id="a6e3d-231">In the Application Insights portal, create a web test.</span></span>

    ![Välj Lägg till på bladet Webbtest.](./media/app-insights-monitor-web-app-availability/16-another-test.png)
2. <span data-ttu-id="a6e3d-233">Välj ett flerstegstest och ladda upp filen .webtest.</span><span class="sxs-lookup"><span data-stu-id="a6e3d-233">Select multi-step test, and upload the .webtest file.</span></span>

    ![Välj webbtestet med flera steg.](./media/app-insights-monitor-web-app-availability/appinsights-71webtestUpload.png)

    <span data-ttu-id="a6e3d-235">Ange testplatserna, frekvensen och aviseringsparametrarna på samma sätt som för pingtest.</span><span class="sxs-lookup"><span data-stu-id="a6e3d-235">Set the test locations, frequency, and alert parameters in the same way as for ping tests.</span></span>

#### <a name="3-see-the-results"></a><span data-ttu-id="a6e3d-236">3. Visa resultaten</span><span class="sxs-lookup"><span data-stu-id="a6e3d-236">3. See the results</span></span>

<span data-ttu-id="a6e3d-237">Visa testresultat och eventuella fel på samma sätt som för tester med en enskild URL.</span><span class="sxs-lookup"><span data-stu-id="a6e3d-237">View your test results and any failures in the same way as single-url tests.</span></span>

<span data-ttu-id="a6e3d-238">Dessutom kan du hämta testresultat och visa dem i Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="a6e3d-238">In addition, you can download the test results to view them in Visual Studio.</span></span>

#### <a name="too-many-failures"></a><span data-ttu-id="a6e3d-239">För många fel?</span><span class="sxs-lookup"><span data-stu-id="a6e3d-239">Too many failures?</span></span>

* <span data-ttu-id="a6e3d-240">En vanlig orsak till fel är att testet körs för länge.</span><span class="sxs-lookup"><span data-stu-id="a6e3d-240">A common reason for failure is that the test runs too long.</span></span> <span data-ttu-id="a6e3d-241">Det får inte köras längre än två minuter.</span><span class="sxs-lookup"><span data-stu-id="a6e3d-241">It mustn't run longer than two minutes.</span></span>

* <span data-ttu-id="a6e3d-242">Glöm inte att alla resurser på en sida måste läsas in korrekt för att testet ska lyckas, inklusive skript, formatmallar, bilder och så vidare.</span><span class="sxs-lookup"><span data-stu-id="a6e3d-242">Don't forget that all the resources of a page must load correctly for the test to succeed, including scripts, style sheets, images, and so forth.</span></span>

* <span data-ttu-id="a6e3d-243">Webbtestet måste finnas i .webtest-skriptet: du kan inte använda kodade funktioner i testet.</span><span class="sxs-lookup"><span data-stu-id="a6e3d-243">The web test must be entirely contained in the .webtest script: you can't use coded functions in the test.</span></span>

### <a name="plugging-time-and-random-numbers-into-your-multi-step-test"></a><span data-ttu-id="a6e3d-244">Använda tid och slumptal i flerstegstest</span><span class="sxs-lookup"><span data-stu-id="a6e3d-244">Plugging time and random numbers into your multi-step test</span></span>
<span data-ttu-id="a6e3d-245">Anta att du testar ett verktyg som hämtar tidsberoende data, till exempel aktier från ett externt flöde.</span><span class="sxs-lookup"><span data-stu-id="a6e3d-245">Suppose you're testing a tool that gets time-dependent data such as stocks from an external feed.</span></span> <span data-ttu-id="a6e3d-246">När du spelar in webbtestet måste du ange specifika tider, men du anger dem som parametrar för testet, StartTime och EndTime.</span><span class="sxs-lookup"><span data-stu-id="a6e3d-246">When you record your web test, you have to use specific times, but you set them as parameters of the test, StartTime and EndTime.</span></span>

![Ett webbtest med parametrar.](./media/app-insights-monitor-web-app-availability/appinsights-72webtest-parameters.png)

<span data-ttu-id="a6e3d-248">När du kör testet vill du att EndTime alltid ska vara den aktuella tiden, och StartTime 15 minuter tidigare.</span><span class="sxs-lookup"><span data-stu-id="a6e3d-248">When you run the test, you'd like EndTime always to be the present time, and StartTime should be 15 minutes ago.</span></span>

<span data-ttu-id="a6e3d-249">Du kan parameterisera tider med hjälp av webbtest-plugin-program.</span><span class="sxs-lookup"><span data-stu-id="a6e3d-249">Web Test Plug-ins provide the way to do parameterize times.</span></span>

1. <span data-ttu-id="a6e3d-250">Lägg till ett plugin-program för webbtester för varje variabelt parametervärde som du behöver.</span><span class="sxs-lookup"><span data-stu-id="a6e3d-250">Add a web test plug-in for each variable parameter value you want.</span></span> <span data-ttu-id="a6e3d-251">I verktygsfältet Webbtest väljer du **Lägg till plugin-program för webbtest**.</span><span class="sxs-lookup"><span data-stu-id="a6e3d-251">In the web test toolbar, choose **Add Web Test Plugin**.</span></span>

    ![Välj Lägg till plugin-program för webbtest och välj en typ.](./media/app-insights-monitor-web-app-availability/appinsights-72webtest-plugins.png)

    <span data-ttu-id="a6e3d-253">I det här exemplet ska vi använda två instanser av plugin-programmet för datum och tid.</span><span class="sxs-lookup"><span data-stu-id="a6e3d-253">In this example, we use two instances of the Date Time Plug-in.</span></span> <span data-ttu-id="a6e3d-254">En instans är för ”15 minuter tidigare” och en annan för ”nu”.</span><span class="sxs-lookup"><span data-stu-id="a6e3d-254">One instance is for "15 minutes ago" and another for "now."</span></span>
2. <span data-ttu-id="a6e3d-255">Öppna egenskaperna för varje plugin-program.</span><span class="sxs-lookup"><span data-stu-id="a6e3d-255">Open the properties of each plug-in.</span></span> <span data-ttu-id="a6e3d-256">Lägg till ett namn och ange att den aktuella tiden ska användas.</span><span class="sxs-lookup"><span data-stu-id="a6e3d-256">Give it a name and set it to use the current time.</span></span> <span data-ttu-id="a6e3d-257">För den ena anger du Lägg till minuter = -15.</span><span class="sxs-lookup"><span data-stu-id="a6e3d-257">For one of them, set Add Minutes = -15.</span></span>

    ![Ange namn, Använd aktuell tid och Lägg till minuter.](./media/app-insights-monitor-web-app-availability/appinsights-72webtest-plugin-parameters.png)
3. <span data-ttu-id="a6e3d-259">Du refererar till plugin-namn genom att använd {{plugin-name}} i parametrarna för webbtestet.</span><span class="sxs-lookup"><span data-stu-id="a6e3d-259">In the web test parameters, use {{plug-in name}} to reference a plug-in name.</span></span>

    ![Använd {{plugin-name}} i testparametern.](./media/app-insights-monitor-web-app-availability/appinsights-72webtest-plugin-name.png)

<span data-ttu-id="a6e3d-261">Ladda upp testet till portalen.</span><span class="sxs-lookup"><span data-stu-id="a6e3d-261">Now, upload your test to the portal.</span></span> <span data-ttu-id="a6e3d-262">De dynamiska värdena används i varje testkörning.</span><span class="sxs-lookup"><span data-stu-id="a6e3d-262">It uses the dynamic values on every run of the test.</span></span>

## <a name="dealing-with-sign-in"></a><span data-ttu-id="a6e3d-263">Hantera inloggning</span><span class="sxs-lookup"><span data-stu-id="a6e3d-263">Dealing with sign-in</span></span>
<span data-ttu-id="a6e3d-264">Om användarna måste logga in i din app kan du välja mellan olika alternativ för att simulera inloggningen, så att du kan testa sidorna bakom inloggningen.</span><span class="sxs-lookup"><span data-stu-id="a6e3d-264">If your users sign in to your app, you have various options for simulating sign-in so that you can test pages behind the sign-in.</span></span> <span data-ttu-id="a6e3d-265">Vilken metod du använder beror på vilken typ av säkerhet som tillhandahålls av appen.</span><span class="sxs-lookup"><span data-stu-id="a6e3d-265">The approach you use depends on the type of security provided by the app.</span></span>

<span data-ttu-id="a6e3d-266">I samtliga fall bör du skapa ett konto i ditt program som endast används för testning.</span><span class="sxs-lookup"><span data-stu-id="a6e3d-266">In all cases, you should create an account in your application just for the purpose of testing.</span></span> <span data-ttu-id="a6e3d-267">Om möjligt begränsar du behörigheterna för det här testkontot så att webbtestningen inte påverkar riktiga användare.</span><span class="sxs-lookup"><span data-stu-id="a6e3d-267">If possible, restrict the permissions of this test account so that there's no possibility of the web tests affecting real users.</span></span>

### <a name="simple-username-and-password"></a><span data-ttu-id="a6e3d-268">Enkelt användarnamn och lösenord</span><span class="sxs-lookup"><span data-stu-id="a6e3d-268">Simple username and password</span></span>
<span data-ttu-id="a6e3d-269">Spela in ett webbtest som vanligt.</span><span class="sxs-lookup"><span data-stu-id="a6e3d-269">Record a web test in the usual way.</span></span> <span data-ttu-id="a6e3d-270">Ta bort cookies först.</span><span class="sxs-lookup"><span data-stu-id="a6e3d-270">Delete cookies first.</span></span>

### <a name="saml-authentication"></a><span data-ttu-id="a6e3d-271">SAML-autentisering</span><span class="sxs-lookup"><span data-stu-id="a6e3d-271">SAML authentication</span></span>
<span data-ttu-id="a6e3d-272">Använd SAML-plugin-programmet som är tillgängligt för webbtester.</span><span class="sxs-lookup"><span data-stu-id="a6e3d-272">Use the SAML plugin that is available for web tests.</span></span>

### <a name="client-secret"></a><span data-ttu-id="a6e3d-273">Klienthemlighet</span><span class="sxs-lookup"><span data-stu-id="a6e3d-273">Client secret</span></span>
<span data-ttu-id="a6e3d-274">Om din app kräver inloggning med en klienthemlighet använder du det.</span><span class="sxs-lookup"><span data-stu-id="a6e3d-274">If your app has a sign-in route that involves a client secret, use that route.</span></span> <span data-ttu-id="a6e3d-275">Azure Active Directory (AAD) är ett exempel på en tjänst som erbjuder inloggning med klienthemligheter.</span><span class="sxs-lookup"><span data-stu-id="a6e3d-275">Azure Active Directory (AAD) is an example of a service that provides a client secret sign-in.</span></span> <span data-ttu-id="a6e3d-276">I AAD är klienthemligheten appnyckeln.</span><span class="sxs-lookup"><span data-stu-id="a6e3d-276">In AAD, the client secret is the App Key.</span></span>

<span data-ttu-id="a6e3d-277">Här är ett exempel på ett webbtest för en Azure-webbapp som använder en appnyckel:</span><span class="sxs-lookup"><span data-stu-id="a6e3d-277">Here's a sample web test of an Azure web app using an app key:</span></span>

![Exempel med en klienthemlighet](./media/app-insights-monitor-web-app-availability/110.png)

1. <span data-ttu-id="a6e3d-279">Hämta en token från AAD med hjälp av en klienthemlighet (AppKey).</span><span class="sxs-lookup"><span data-stu-id="a6e3d-279">Get token from AAD using client secret (AppKey).</span></span>
2. <span data-ttu-id="a6e3d-280">Extrahera ägartoken från svaret.</span><span class="sxs-lookup"><span data-stu-id="a6e3d-280">Extract bearer token from response.</span></span>
3. <span data-ttu-id="a6e3d-281">Anropa API:et med hjälp av ägartoken i auktoriseringshuvudet.</span><span class="sxs-lookup"><span data-stu-id="a6e3d-281">Call API using bearer token in the authorization header.</span></span>

<span data-ttu-id="a6e3d-282">Kontrollera att webbtestet är en riktig klient, dvs. att det har en egen app i AAD, och använd dess klient-ID och appnyckel.</span><span class="sxs-lookup"><span data-stu-id="a6e3d-282">Make sure that the web test is an actual client - that is, it has its own app in AAD - and use its clientId + appkey.</span></span> <span data-ttu-id="a6e3d-283">Tjänsten som testas har också sin egen app i AAD: appID-URI:n för den här appen visas i webbtestet i fältet ”resource”.</span><span class="sxs-lookup"><span data-stu-id="a6e3d-283">Your service under test also has its own app in AAD: the appID URI of this app is reflected in the web test in the “resource” field.</span></span>

### <a name="open-authentication"></a><span data-ttu-id="a6e3d-284">Öppen autentisering</span><span class="sxs-lookup"><span data-stu-id="a6e3d-284">Open Authentication</span></span>
<span data-ttu-id="a6e3d-285">Ett exempel på öppen autentisering är inloggning med ett Microsoft- eller Google-konto.</span><span class="sxs-lookup"><span data-stu-id="a6e3d-285">An example of open authentication is signing in with your Microsoft or Google account.</span></span> <span data-ttu-id="a6e3d-286">Många appar som använder OAuth erbjuder möjligheten att använda en klienthemlighet, så det första du bör göra är att ta reda på detta.</span><span class="sxs-lookup"><span data-stu-id="a6e3d-286">Many apps that use OAuth provide the client secret alternative, so your first tactic should be to investigate that possibility.</span></span>

<span data-ttu-id="a6e3d-287">Om testet måste logga in med OAuth är den allmänna riktlinjen att:</span><span class="sxs-lookup"><span data-stu-id="a6e3d-287">If your test must sign in using OAuth, the general approach is:</span></span>

* <span data-ttu-id="a6e3d-288">Använda ett verktyg som Fiddler för att undersöka trafiken mellan webbläsaren, autentiseringswebbplatsen och din app.</span><span class="sxs-lookup"><span data-stu-id="a6e3d-288">Use a tool such as Fiddler to examine the traffic between your web browser, the authentication site, and your app.</span></span>
* <span data-ttu-id="a6e3d-289">Utföra två eller flera inloggningar med olika datorer eller webbläsare, eller med långa intervall (så att token upphör att gälla).</span><span class="sxs-lookup"><span data-stu-id="a6e3d-289">Perform two or more sign-ins using different machines or browsers, or at long intervals (to allow tokens to expire).</span></span>
* <span data-ttu-id="a6e3d-290">Genom att jämföra olika sessioner identifiera den token som skickas tillbaka från autentiseringswebbplatsen, som sedan skickas till din appserver efter inloggningen.</span><span class="sxs-lookup"><span data-stu-id="a6e3d-290">By comparing different sessions, identify the token passed back from the authenticating site, that is then passed to your app server after sign-in.</span></span>
* <span data-ttu-id="a6e3d-291">Spela in ett webbtest med hjälp av Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="a6e3d-291">Record a web test using Visual Studio.</span></span>
* <span data-ttu-id="a6e3d-292">Parameterisera token genom att ange parametern när token returneras från autentiseraren och använda den i frågan till webbplatsen.</span><span class="sxs-lookup"><span data-stu-id="a6e3d-292">Parameterize the tokens, setting the parameter when the token is returned from the authenticator, and using it in the query to the site.</span></span>
  <span data-ttu-id="a6e3d-293">(Visual Studio försöker parameterisera testet, men kan inte parameterisera token korrekt.)</span><span class="sxs-lookup"><span data-stu-id="a6e3d-293">(Visual Studio attempts to parameterize the test, but does not correctly parameterize the tokens.)</span></span>


## <a name="performance-tests"></a><span data-ttu-id="a6e3d-294">Prestandatester</span><span class="sxs-lookup"><span data-stu-id="a6e3d-294">Performance tests</span></span>
<span data-ttu-id="a6e3d-295">Du kan köra ett inläsningstest på din webbplats.</span><span class="sxs-lookup"><span data-stu-id="a6e3d-295">You can run a load test on your website.</span></span> <span data-ttu-id="a6e3d-296">Som med tillgänglighetstestet kan du skicka antingen enkla begäranden eller begäranden med flera steg från våra platser runtom i världen.</span><span class="sxs-lookup"><span data-stu-id="a6e3d-296">Like the availability test, you can send either simple requests or multi-step requests from our points around the world.</span></span> <span data-ttu-id="a6e3d-297">Till skillnad från ett tillgänglighetstest skickas många begäranden, som simulerar flera samtidiga användare.</span><span class="sxs-lookup"><span data-stu-id="a6e3d-297">Unlike an availability test, many requests are sent, simulating multiple simultaneous users.</span></span>

<span data-ttu-id="a6e3d-298">Öppna **Inställningar**, **Prestandatest** från bladet Översikt.</span><span class="sxs-lookup"><span data-stu-id="a6e3d-298">From the Overview blade, open **Settings**, **Performance Tests**.</span></span> <span data-ttu-id="a6e3d-299">När du skapar ett test uppmanas du att ansluta till eller att skapa ett konto för Visual Studio Team Services.</span><span class="sxs-lookup"><span data-stu-id="a6e3d-299">When you create a test, you are invited to connect to or create a Visual Studio Team Services account.</span></span>

<span data-ttu-id="a6e3d-300">När testet är klart visas svarstiderna och slutförandefrekvens.</span><span class="sxs-lookup"><span data-stu-id="a6e3d-300">When the test is complete, you are shown response times and success rates.</span></span>


![Prestandatest](./media/app-insights-monitor-web-app-availability/perf-test.png)

> [!TIP]
> <span data-ttu-id="a6e3d-302">Om du vill se effekterna av ett prestandatest kan du använda [Live Stream](app-insights-live-stream.md) och [Profilerare](app-insights-profiler.md).</span><span class="sxs-lookup"><span data-stu-id="a6e3d-302">To observe the effects of a performance test, use [Live Stream](app-insights-live-stream.md) and [Profiler](app-insights-profiler.md).</span></span>
>

## <a name="automation"></a><span data-ttu-id="a6e3d-303">Automation</span><span class="sxs-lookup"><span data-stu-id="a6e3d-303">Automation</span></span>
* <span data-ttu-id="a6e3d-304">[Konfigurera ett tillgänglighetstest automatiskt med hjälp av PowerShell-skript](app-insights-powershell.md#add-an-availability-test).</span><span class="sxs-lookup"><span data-stu-id="a6e3d-304">[Use PowerShell scripts to set up an availability test](app-insights-powershell.md#add-an-availability-test) automatically.</span></span>
* <span data-ttu-id="a6e3d-305">Konfigurera en [webhook](../monitoring-and-diagnostics/insights-webhooks-alerts.md) som anropas när en avisering genereras.</span><span class="sxs-lookup"><span data-stu-id="a6e3d-305">Set up a [webhook](../monitoring-and-diagnostics/insights-webhooks-alerts.md) that is called when an alert is raised.</span></span>

## <span data-ttu-id="a6e3d-306"><a name="qna"></a>Har du några frågor?</span><span class="sxs-lookup"><span data-stu-id="a6e3d-306"><a name="qna"></a>Questions?</span></span> <span data-ttu-id="a6e3d-307">Har du problem?</span><span class="sxs-lookup"><span data-stu-id="a6e3d-307">Problems?</span></span>
* <span data-ttu-id="a6e3d-308">*Kan jag anropa kod från mitt webbtest?*</span><span class="sxs-lookup"><span data-stu-id="a6e3d-308">*Can I call code from my web test?*</span></span>

    <span data-ttu-id="a6e3d-309">Nej.</span><span class="sxs-lookup"><span data-stu-id="a6e3d-309">No.</span></span> <span data-ttu-id="a6e3d-310">Stegen i testet måste finnas i filen .webtest.</span><span class="sxs-lookup"><span data-stu-id="a6e3d-310">The steps of the test must be in the .webtest file.</span></span> <span data-ttu-id="a6e3d-311">Och du kan inte anropa andra webbtester eller använda loopar.</span><span class="sxs-lookup"><span data-stu-id="a6e3d-311">And you can't call other web tests or use loops.</span></span> <span data-ttu-id="a6e3d-312">Men det finns flera plugin-program som kan vara användbara.</span><span class="sxs-lookup"><span data-stu-id="a6e3d-312">But there are several plug-ins that you might find helpful.</span></span>
* <span data-ttu-id="a6e3d-313">*Stöds HTTPS?*</span><span class="sxs-lookup"><span data-stu-id="a6e3d-313">*Is HTTPS supported?*</span></span>

    <span data-ttu-id="a6e3d-314">Vi stöder TLS 1.1 och TLS 1.2.</span><span class="sxs-lookup"><span data-stu-id="a6e3d-314">We support TLS 1.1 and TLS 1.2.</span></span>
* <span data-ttu-id="a6e3d-315">*Är det någon skillnad mellan ”webbtester” och ”tillgänglighetstester”?*</span><span class="sxs-lookup"><span data-stu-id="a6e3d-315">*Is there a difference between "web tests" and "availability tests"?*</span></span>

    <span data-ttu-id="a6e3d-316">De är synonyma begrepp.</span><span class="sxs-lookup"><span data-stu-id="a6e3d-316">The two terms may be referenced interchangeably.</span></span> <span data-ttu-id="a6e3d-317">Tillgänglighetstest är ett mer allmänt begrepp som innefattar tester med en URL-ping utöver webbtester i flera steg.</span><span class="sxs-lookup"><span data-stu-id="a6e3d-317">Availability tests is a more generic term that includes the single URL ping tests in addition to the multi-step web tests.</span></span>
* <span data-ttu-id="a6e3d-318">*Jag vill använda tillgänglighetstester på vår interna server som körs bakom en brandvägg.*</span><span class="sxs-lookup"><span data-stu-id="a6e3d-318">*I'd like to use availability tests on our internal server that runs behind a firewall.*</span></span>

    <span data-ttu-id="a6e3d-319">Det finns två möjliga lösningar:</span><span class="sxs-lookup"><span data-stu-id="a6e3d-319">There are two possible solutions:</span></span>
    
    * <span data-ttu-id="a6e3d-320">Konfigurera din brandvägg att tillåta inkommande förfrågningar från [IP-adresserna för webbtestagenter](app-insights-ip-addresses.md).</span><span class="sxs-lookup"><span data-stu-id="a6e3d-320">Configure your firewall to permit incoming requests from the [IP addresses of our web test agents](app-insights-ip-addresses.md).</span></span>
    * <span data-ttu-id="a6e3d-321">Skriv koden för att regelbundet testa din interna server.</span><span class="sxs-lookup"><span data-stu-id="a6e3d-321">Write your own code to periodically test your internal server.</span></span> <span data-ttu-id="a6e3d-322">Kör koden i bakgrunden på en testserver bakom brandväggen.</span><span class="sxs-lookup"><span data-stu-id="a6e3d-322">Run the code as a background process on a test server behind your firewall.</span></span> <span data-ttu-id="a6e3d-323">Testprocessen kan skicka resultaten till Application Insights med [TrackAvailability()](https://docs.microsoft.com/dotnet/api/microsoft.applicationinsights.telemetryclient.trackavailability) API i core-SDK-paketet.</span><span class="sxs-lookup"><span data-stu-id="a6e3d-323">Your test process can send its results to Application Insights by using [TrackAvailability()](https://docs.microsoft.com/dotnet/api/microsoft.applicationinsights.telemetryclient.trackavailability) API in the core SDK package.</span></span> <span data-ttu-id="a6e3d-324">Detta kräver att din testserver har utgående åtkomst till Application Insights slutpunkt för inmatning, men detta utgör en mycket mindre säkerhetsrisk än alternativet att tillåta inkommande förfrågningar.</span><span class="sxs-lookup"><span data-stu-id="a6e3d-324">This requires your test server to have outgoing access to the Application Insights ingestion endpoint, but that is a much smaller security risk than the alternative of permitting incoming requests.</span></span> <span data-ttu-id="a6e3d-325">Resultatet visas inte på bladen för webbtillgänglighetstester, men däremot visas det som tillgänglighetsresultat i Analytics, Sök och Metric Explorer.</span><span class="sxs-lookup"><span data-stu-id="a6e3d-325">The results will not appear in the availability web tests blades, but appears as availability results in Analytics, Search, and Metric Explorer.</span></span>
* <span data-ttu-id="a6e3d-326">*Det går inte att överföra ett flerstegstest för webbplatser*</span><span class="sxs-lookup"><span data-stu-id="a6e3d-326">*Uploading a multi-step web test fails*</span></span>

    <span data-ttu-id="a6e3d-327">Det finns en storleksgräns på 300 kB.</span><span class="sxs-lookup"><span data-stu-id="a6e3d-327">There's a size limit of 300 K.</span></span>

    <span data-ttu-id="a6e3d-328">Loopar stöds inte.</span><span class="sxs-lookup"><span data-stu-id="a6e3d-328">Loops aren't supported.</span></span>

    <span data-ttu-id="a6e3d-329">Referenser till andra webbtester stöds inte.</span><span class="sxs-lookup"><span data-stu-id="a6e3d-329">References to other web tests aren't supported.</span></span>

    <span data-ttu-id="a6e3d-330">Datakällor stöds inte.</span><span class="sxs-lookup"><span data-stu-id="a6e3d-330">Data sources aren't supported.</span></span>
* <span data-ttu-id="a6e3d-331">*Mitt test med flera steg slutförs inte*</span><span class="sxs-lookup"><span data-stu-id="a6e3d-331">*My multi-step test doesn't complete*</span></span>

    <span data-ttu-id="a6e3d-332">Det finns en gräns på 100 förfrågningar per test.</span><span class="sxs-lookup"><span data-stu-id="a6e3d-332">There's a limit of 100 requests per test.</span></span>

    <span data-ttu-id="a6e3d-333">Testet stoppas om den körs längre än två minuter.</span><span class="sxs-lookup"><span data-stu-id="a6e3d-333">The test is stopped if it runs longer than two minutes.</span></span>
* <span data-ttu-id="a6e3d-334">*Hur kan jag köra ett test med klientcertifikat?*</span><span class="sxs-lookup"><span data-stu-id="a6e3d-334">*How can I run a test with client certificates?*</span></span>

    <span data-ttu-id="a6e3d-335">Det stöds tyvärr inte.</span><span class="sxs-lookup"><span data-stu-id="a6e3d-335">We don't support that, sorry.</span></span>


## <span data-ttu-id="a6e3d-336"><a name="next"></a>Nästa steg</span><span class="sxs-lookup"><span data-stu-id="a6e3d-336"><a name="next"></a>Next steps</span></span>
<span data-ttu-id="a6e3d-337">[Sök i diagnostikloggar][diagnostic]</span><span class="sxs-lookup"><span data-stu-id="a6e3d-337">[Search diagnostic logs][diagnostic]</span></span>

<span data-ttu-id="a6e3d-338">[Felsökning][qna]</span><span class="sxs-lookup"><span data-stu-id="a6e3d-338">[Troubleshooting][qna]</span></span>

[<span data-ttu-id="a6e3d-339">IP-adresser för webbtestagenter</span><span class="sxs-lookup"><span data-stu-id="a6e3d-339">IP addresses of web test agents</span></span>](app-insights-ip-addresses.md)

<!--Link references-->

[azure-availability]: ../insights-create-web-tests.md
[diagnostic]: app-insights-diagnostic-search.md
[qna]: app-insights-troubleshoot-faq.md
[start]: app-insights-overview.md
