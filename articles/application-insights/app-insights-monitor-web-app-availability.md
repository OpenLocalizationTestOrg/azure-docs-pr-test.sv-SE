---
title: "aaaMonitor tillgänglighet och svarstider för någon webbplats | Microsoft Docs"
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
ms.openlocfilehash: 4c5425c948770cc57a648ca50e217c75ac75fbd7
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="monitor-availability-and-responsiveness-of-any-web-site"></a><span data-ttu-id="d535e-104">Övervaka tillgänglighet och svarstider på valfri webbplats</span><span class="sxs-lookup"><span data-stu-id="d535e-104">Monitor availability and responsiveness of any web site</span></span>
<span data-ttu-id="d535e-105">När du har distribuerat din webbapp eller webbplats tooany server kan ställa du in testerna toomonitor dess tillgänglighet och svarstider.</span><span class="sxs-lookup"><span data-stu-id="d535e-105">After you've deployed your web app or web site tooany server, you can set up tests toomonitor its availability and responsiveness.</span></span> <span data-ttu-id="d535e-106">[Azure Application Insights](app-insights-overview.md) skickar webbegäranden tooyour program med jämna mellanrum från punkter runt hälsningsmeddelande.</span><span class="sxs-lookup"><span data-stu-id="d535e-106">[Azure Application Insights](app-insights-overview.md) sends web requests tooyour application at regular intervals from points around hello world.</span></span> <span data-ttu-id="d535e-107">Den varnar dig om programmet inte svarar eller svarar långsamt.</span><span class="sxs-lookup"><span data-stu-id="d535e-107">It alerts you if your application doesn't respond, or responds slowly.</span></span>

<span data-ttu-id="d535e-108">Du kan ställa in tillgänglighetstester för alla HTTP eller HTTPS-slutpunkt som är tillgänglig från hello offentliga internet.</span><span class="sxs-lookup"><span data-stu-id="d535e-108">You can set up availability tests for any HTTP or HTTPS endpoint that is accessible from hello public internet.</span></span> <span data-ttu-id="d535e-109">Du har inte tooadd något annat toohello webbplats som du testar.</span><span class="sxs-lookup"><span data-stu-id="d535e-109">You don't have tooadd anything toohello web site you're testing.</span></span> <span data-ttu-id="d535e-110">Ingen även har toobe webbplatsen: du kan testa en REST API-tjänst som du är beroende av.</span><span class="sxs-lookup"><span data-stu-id="d535e-110">It doesn't even have toobe your site: you could test a REST API service on which you depend.</span></span>

<span data-ttu-id="d535e-111">Det finns två typer av tillgänglighetstester:</span><span class="sxs-lookup"><span data-stu-id="d535e-111">There are two types of availability tests:</span></span>

* <span data-ttu-id="d535e-112">[URL: en ping-testet](#create): ett enkelt test som du kan skapa i hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="d535e-112">[URL ping test](#create): a simple test that you can create in hello Azure portal.</span></span>
* <span data-ttu-id="d535e-113">[Flera steg webbtest](#multi-step-web-tests): som du skapar i Visual Studio Enterprise och överför toohello portal.</span><span class="sxs-lookup"><span data-stu-id="d535e-113">[Multi-step web test](#multi-step-web-tests): which you create in Visual Studio Enterprise and upload toohello portal.</span></span>

<span data-ttu-id="d535e-114">Du kan skapa upp too25 tillgänglighetstester per resurs för program.</span><span class="sxs-lookup"><span data-stu-id="d535e-114">You can create up too25 availability tests per application resource.</span></span>

## <span data-ttu-id="d535e-115"><a name="create"></a>1. Öppna en resurs för dina tillgänglighetstestrapporter</span><span class="sxs-lookup"><span data-stu-id="d535e-115"><a name="create"></a>1. Open a resource for your availability test reports</span></span>

<span data-ttu-id="d535e-116">**Om du redan har konfigurerat Application Insights** för ditt webbprogram, öppna dess Application Insights-resurs i hello [Azure-portalen](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="d535e-116">**If you have already configured Application Insights** for your web app, open its Application Insights resource in hello [Azure portal](https://portal.azure.com).</span></span>

<span data-ttu-id="d535e-117">**Eller, om du vill toosee dina rapporter i en ny resurs** registrera dig för[Microsoft Azure](http://azure.com), gå toohello [Azure-portalen](https://portal.azure.com), och skapa en Application Insights-resurs.</span><span class="sxs-lookup"><span data-stu-id="d535e-117">**Or, if you want toosee your reports in a new resource,** sign up too[Microsoft Azure](http://azure.com), go toohello [Azure portal](https://portal.azure.com), and create an Application Insights resource.</span></span>

![Nytt > Application Insights](./media/app-insights-monitor-web-app-availability/11-new-app.png)

<span data-ttu-id="d535e-119">Klicka på **alla resurser** tooopen hello översikt bladet för hello ny resurs.</span><span class="sxs-lookup"><span data-stu-id="d535e-119">Click **All resources** tooopen hello Overview blade for hello new resource.</span></span>

## <span data-ttu-id="d535e-120"><a name="setup"></a>2. Skapa ett URL-pingtest</span><span class="sxs-lookup"><span data-stu-id="d535e-120"><a name="setup"></a>2. Create a URL ping test</span></span>
<span data-ttu-id="d535e-121">Öppna hello tillgänglighet bladet och Lägg till ett test.</span><span class="sxs-lookup"><span data-stu-id="d535e-121">Open hello Availability blade and add a test.</span></span>

![Fill minst hello URL för din webbplats](./media/app-insights-monitor-web-app-availability/13-availability.png)

* <span data-ttu-id="d535e-123">**hello URL** kan vara en webbsida som du vill tootest, men det måste vara synlig från hello offentliga internet.</span><span class="sxs-lookup"><span data-stu-id="d535e-123">**hello URL** can be any web page you want tootest, but it must be visible from hello public internet.</span></span> <span data-ttu-id="d535e-124">hello-URL kan innehålla en frågesträng.</span><span class="sxs-lookup"><span data-stu-id="d535e-124">hello URL can include a query string.</span></span> <span data-ttu-id="d535e-125">Du kan arbeta med din databas om du vill.</span><span class="sxs-lookup"><span data-stu-id="d535e-125">So, for example, you can exercise your database a little.</span></span> <span data-ttu-id="d535e-126">Om hello URL matchar tooa omdirigering, följ vi in too10 omdirigeringar.</span><span class="sxs-lookup"><span data-stu-id="d535e-126">If hello URL resolves tooa redirect, we follow it up too10 redirects.</span></span>
* <span data-ttu-id="d535e-127">**Parsa beroende begäranden**: om det här alternativet är markerat hello test begär avbildningar, skript, filer och andra filer som ingår i hello webbsida under testet.</span><span class="sxs-lookup"><span data-stu-id="d535e-127">**Parse dependent requests**: If this option is checked, hello test requests images, scripts, style files, and other files that are part of hello web page under test.</span></span> <span data-ttu-id="d535e-128">hello innehåller inspelade svarstid hello tidsåtgång tooget dessa filer.</span><span class="sxs-lookup"><span data-stu-id="d535e-128">hello recorded response time includes hello time taken tooget these files.</span></span> <span data-ttu-id="d535e-129">hello testet misslyckas om dessa resurser inte kan har hämtats inom hello tidsgränsen för hello hela testet.</span><span class="sxs-lookup"><span data-stu-id="d535e-129">hello test fails if all these resources cannot be successfully downloaded within hello timeout for hello whole test.</span></span> 

    <span data-ttu-id="d535e-130">Om hello-alternativet inte är markerat begär hello test endast hello fil vid hello-URL som du angav.</span><span class="sxs-lookup"><span data-stu-id="d535e-130">If hello option is not checked, hello test only requests hello file at hello URL you specified.</span></span>
* <span data-ttu-id="d535e-131">**Aktivera återförsök**: om det här alternativet är markerat när hello testet misslyckas, det är ett nytt försök efter en kort tid.</span><span class="sxs-lookup"><span data-stu-id="d535e-131">**Enable retries**:  If this option is checked, when hello test fails, it is retried after a short interval.</span></span> <span data-ttu-id="d535e-132">Ett fel rapporteras endast om tre på varandra följande försök misslyckas.</span><span class="sxs-lookup"><span data-stu-id="d535e-132">A failure is reported only if three successive attempts fail.</span></span> <span data-ttu-id="d535e-133">Efterföljande tester utförs sedan på hello vanliga Testfrekvensen.</span><span class="sxs-lookup"><span data-stu-id="d535e-133">Subsequent tests are then performed at hello usual test frequency.</span></span> <span data-ttu-id="d535e-134">Försök igen är tillfälligt tills hello nästa lyckades.</span><span class="sxs-lookup"><span data-stu-id="d535e-134">Retry is temporarily suspended until hello next success.</span></span> <span data-ttu-id="d535e-135">Den här regeln tillämpas separat på varje testplats.</span><span class="sxs-lookup"><span data-stu-id="d535e-135">This rule is applied independently at each test location.</span></span> <span data-ttu-id="d535e-136">Vi rekommenderar det här alternativet.</span><span class="sxs-lookup"><span data-stu-id="d535e-136">We recommend this option.</span></span> <span data-ttu-id="d535e-137">I genomsnitt försvinner ca 80 % av felen vid återförsök.</span><span class="sxs-lookup"><span data-stu-id="d535e-137">On average, about 80% of failures disappear on retry.</span></span>
* <span data-ttu-id="d535e-138">**Testfrekvens**: Anger hur ofta hello testet körs från varje test-plats.</span><span class="sxs-lookup"><span data-stu-id="d535e-138">**Test frequency**: Sets how often hello test is run from each test location.</span></span> <span data-ttu-id="d535e-139">Med en frekvens på fem minuter och fem testplatser testas din webbplats i genomsnitt varje minut.</span><span class="sxs-lookup"><span data-stu-id="d535e-139">With a frequency of five minutes and five test locations, your site is tested on average every minute.</span></span>
* <span data-ttu-id="d535e-140">**Testa platser** är hello placerar från som våra servrar skicka Webbadress begäranden tooyour.</span><span class="sxs-lookup"><span data-stu-id="d535e-140">**Test locations** are hello places from where our servers send web requests tooyour URL.</span></span> <span data-ttu-id="d535e-141">Välj mer än en så att du kan skilja mellan problem på din webbplats och nätverksproblem.</span><span class="sxs-lookup"><span data-stu-id="d535e-141">Choose more than one so that you can distinguish problems in your website from network issues.</span></span> <span data-ttu-id="d535e-142">Du kan välja in too16 platser.</span><span class="sxs-lookup"><span data-stu-id="d535e-142">You can select up too16 locations.</span></span>
* <span data-ttu-id="d535e-143">**Villkor för lyckad test**:</span><span class="sxs-lookup"><span data-stu-id="d535e-143">**Success criteria**:</span></span>

    <span data-ttu-id="d535e-144">**Testtidsgräns**: minska det här värdet toobe aviserad om långsamt svar.</span><span class="sxs-lookup"><span data-stu-id="d535e-144">**Test timeout**: Decrease this value toobe alerted about slow responses.</span></span> <span data-ttu-id="d535e-145">hello test räknas som ett fel om hello svar från din plats inte har tagits emot inom den tiden.</span><span class="sxs-lookup"><span data-stu-id="d535e-145">hello test is counted as a failure if hello responses from your site have not been received within this period.</span></span> <span data-ttu-id="d535e-146">Om du har valt **parsa beroende begäranden**sedan alla hello bilder, filer, skript och andra beroende resurser måste har tagits emot inom den tiden.</span><span class="sxs-lookup"><span data-stu-id="d535e-146">If you selected **Parse dependent requests**, then all hello images, style files, scripts, and other dependent resources must have been received within this period.</span></span>

    <span data-ttu-id="d535e-147">**HTTP-svar**: hello returnerade statuskoden räknas som lyckas.</span><span class="sxs-lookup"><span data-stu-id="d535e-147">**HTTP response**: hello returned status code that is counted as a success.</span></span> <span data-ttu-id="d535e-148">200 är hello-kod som anger att en normal webbsida har returnerats.</span><span class="sxs-lookup"><span data-stu-id="d535e-148">200 is hello code that indicates that a normal web page has been returned.</span></span>

    <span data-ttu-id="d535e-149">**Innehållsmatchning**: en sträng, t.ex. ”Välkommen!”.</span><span class="sxs-lookup"><span data-stu-id="d535e-149">**Content match**: a string, like "Welcome!"</span></span> <span data-ttu-id="d535e-150">Vi testar i varje svar om någon skiftlägeskänslig matchning förekommer.</span><span class="sxs-lookup"><span data-stu-id="d535e-150">We test that an exact case-sensitive match occurs in every response.</span></span> <span data-ttu-id="d535e-151">Den måste vara en enkel sträng utan jokertecken.</span><span class="sxs-lookup"><span data-stu-id="d535e-151">It must be a plain string, without wildcards.</span></span> <span data-ttu-id="d535e-152">Glöm inte att om sidan innehåll ändringarna du kanske tooupdate den.</span><span class="sxs-lookup"><span data-stu-id="d535e-152">Don't forget that if your page content changes you might have tooupdate it.</span></span>
* <span data-ttu-id="d535e-153">**Aviseringar** , som standard skickas tooyou om det finns fel på tre platser över fem minuter.</span><span class="sxs-lookup"><span data-stu-id="d535e-153">**Alerts** are, by default, sent tooyou if there are failures in three locations over five minutes.</span></span> <span data-ttu-id="d535e-154">Ett fel på en plats är sannolikt toobe nätverksproblem och inte ett problem med din webbplats.</span><span class="sxs-lookup"><span data-stu-id="d535e-154">A failure in one location is likely toobe a network problem, and not a problem with your site.</span></span> <span data-ttu-id="d535e-155">Men du kan ändra hello tröskelvärdet toobe mer eller mindre känslig, och du kan också ändra som hello e-postmeddelanden ska skickas till.</span><span class="sxs-lookup"><span data-stu-id="d535e-155">But you can change hello threshold toobe more or less sensitive, and you can also change who hello emails should be sent to.</span></span>

    <span data-ttu-id="d535e-156">Du kan konfigurera en [webhook](../monitoring-and-diagnostics/insights-webhooks-alerts.md) som anropas när en avisering genereras.</span><span class="sxs-lookup"><span data-stu-id="d535e-156">You can set up a [webhook](../monitoring-and-diagnostics/insights-webhooks-alerts.md) that is called when an alert is raised.</span></span> <span data-ttu-id="d535e-157">(Observera dock att frågeparametrar inte skickas som egenskaper för närvarande.)</span><span class="sxs-lookup"><span data-stu-id="d535e-157">(But note that, at present, query parameters are not passed through as Properties.)</span></span>

### <a name="test-more-urls"></a><span data-ttu-id="d535e-158">Testa fler URL:er</span><span class="sxs-lookup"><span data-stu-id="d535e-158">Test more URLs</span></span>
<span data-ttu-id="d535e-159">Lägg till fler test.</span><span class="sxs-lookup"><span data-stu-id="d535e-159">Add more tests.</span></span> <span data-ttu-id="d535e-160">Exempelvis lägga tootesting startsidan, du kan kontrollera i din databas körs genom att testa hello URL: en för en sökning.</span><span class="sxs-lookup"><span data-stu-id="d535e-160">For example, In addition tootesting your home page, you can make sure your database is running by testing hello URL for a search.</span></span>


## <span data-ttu-id="d535e-161"><a name="monitor"></a>3. Visa tillgänglighetstestresultat</span><span class="sxs-lookup"><span data-stu-id="d535e-161"><a name="monitor"></a>3. See your availability test results</span></span>

<span data-ttu-id="d535e-162">Efter några minuter, klickar du på **uppdatera** toosee testresultaten.</span><span class="sxs-lookup"><span data-stu-id="d535e-162">After a few minutes, click **Refresh** toosee test results.</span></span> 

![Sammanfattningen av resultaten på hello hem blad](./media/app-insights-monitor-web-app-availability/14-availSummary-3.png)

<span data-ttu-id="d535e-164">Hej scatterplot innehåller exempel på hello testresultaten som har diagnostiska test-steg-information.</span><span class="sxs-lookup"><span data-stu-id="d535e-164">hello scatterplot shows samples of hello test results that have diagnostic test-step detail in them.</span></span> <span data-ttu-id="d535e-165">hello test motorn lagrar diagnostisk information för tester med fel.</span><span class="sxs-lookup"><span data-stu-id="d535e-165">hello test engine stores diagnostic detail for tests that have failures.</span></span> <span data-ttu-id="d535e-166">För lyckad tester som diagnostisk information lagras för en delmängd av hello körningar.</span><span class="sxs-lookup"><span data-stu-id="d535e-166">For successful tests, diagnostic details are stored for a subset of hello executions.</span></span> <span data-ttu-id="d535e-167">Hovra över alla hello grönt/red punkter toosee hello test tidsstämpel, testperioden, plats och namn på testet.</span><span class="sxs-lookup"><span data-stu-id="d535e-167">Hover over any of hello green/red dots toosee hello test timestamp, test duration, location, and test name.</span></span> <span data-ttu-id="d535e-168">Klicka dig igenom några punkt i hello punktdiagram ritytans toosee hello information om testresultat för hello.</span><span class="sxs-lookup"><span data-stu-id="d535e-168">Click through any dot in hello scatter plot toosee hello details of hello test result.</span></span>  

<span data-ttu-id="d535e-169">Välj en viss test, plats, eller minska tiden för hello period toosee resultat till runt hello perioden av intresse.</span><span class="sxs-lookup"><span data-stu-id="d535e-169">Select a particular test, location, or reduce hello time period toosee more results around hello time period of interest.</span></span> <span data-ttu-id="d535e-170">Använd Sök Explorer toosee resultat från alla körningar, eller Analytics frågor toorun anpassade rapporter på dessa data.</span><span class="sxs-lookup"><span data-stu-id="d535e-170">Use Search Explorer toosee results from all executions, or use Analytics queries toorun custom reports on this data.</span></span>

<span data-ttu-id="d535e-171">I tillägg toohello raw resultat, att det finns två mått för tillgänglighet i Metrics Explorer:</span><span class="sxs-lookup"><span data-stu-id="d535e-171">In addition toohello raw results, there are two Availability metrics in Metrics Explorer:</span></span> 

1. <span data-ttu-id="d535e-172">Tillgänglighet: Procentandelen hello tester, som över alla test körningar.</span><span class="sxs-lookup"><span data-stu-id="d535e-172">Availability: Percentage of hello tests that were successful, across all test executions.</span></span> 
2. <span data-ttu-id="d535e-173">Testets varaktighet: genomsnittlig tid för alla testkörningar.</span><span class="sxs-lookup"><span data-stu-id="d535e-173">Test Duration: Average test duration across all test executions.</span></span>

<span data-ttu-id="d535e-174">Du kan använda filter på hello test namn, plats tooanalyze trender för en viss test och/eller plats.</span><span class="sxs-lookup"><span data-stu-id="d535e-174">You can apply filters on hello test name, location tooanalyze trends of a particular test and/or location.</span></span>

## <span data-ttu-id="d535e-175"><a name="edit"></a>Granska och redigera tester</span><span class="sxs-lookup"><span data-stu-id="d535e-175"><a name="edit"></a> Inspect and edit tests</span></span>

<span data-ttu-id="d535e-176">Välj en specifik test hello sammanfattningssidan.</span><span class="sxs-lookup"><span data-stu-id="d535e-176">From hello summary page, select a specific test.</span></span> <span data-ttu-id="d535e-177">Där du kan se specifika resultat för testet och redigera eller tillfälligt inaktivera det.</span><span class="sxs-lookup"><span data-stu-id="d535e-177">There, you can see its specific results, and edit or temporarily disable it.</span></span>

![Redigera eller inaktivera ett webbtest](./media/app-insights-monitor-web-app-availability/19-availEdit-3.png)

<span data-ttu-id="d535e-179">Du kanske vill toodisable tillgänglighetstester eller hello varningen regler som är kopplade till dem när du utför underhåll på din tjänst.</span><span class="sxs-lookup"><span data-stu-id="d535e-179">You might want toodisable availability tests or hello alert rules associated with them while you are performing maintenance on your service.</span></span> 

## <span data-ttu-id="d535e-180"><a name="failures"></a>Om du ser fel</span><span class="sxs-lookup"><span data-stu-id="d535e-180"><a name="failures"></a>If you see failures</span></span>
<span data-ttu-id="d535e-181">Klicka på en röd punkt.</span><span class="sxs-lookup"><span data-stu-id="d535e-181">Click a red dot.</span></span>

![Klicka på en röd punkt](./media/app-insights-monitor-web-app-availability/open-instance-3.png)


<span data-ttu-id="d535e-183">Från ett tillgänglighetstestresultat kan du:</span><span class="sxs-lookup"><span data-stu-id="d535e-183">From an availability test result, you can:</span></span>

* <span data-ttu-id="d535e-184">Kontrollera hello svar togs emot från servern.</span><span class="sxs-lookup"><span data-stu-id="d535e-184">Inspect hello response received from your server.</span></span>
* <span data-ttu-id="d535e-185">Öppna hello telemetri som skickas av server-appen under bearbetning av hello-instans för misslyckade begäranden.</span><span class="sxs-lookup"><span data-stu-id="d535e-185">Open hello telemetry sent by your server app while processing hello failed request instance.</span></span>
* <span data-ttu-id="d535e-186">Logga ett problem eller arbetsobjekt i Git eller VSTS tootrack hello problem.</span><span class="sxs-lookup"><span data-stu-id="d535e-186">Log an issue or work item in Git or VSTS tootrack hello problem.</span></span> <span data-ttu-id="d535e-187">hello-programfel innehåller en länk toothis händelse.</span><span class="sxs-lookup"><span data-stu-id="d535e-187">hello bug will contain a link toothis event.</span></span>
* <span data-ttu-id="d535e-188">Öppna hello web testresultat i Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="d535e-188">Open hello web test result in Visual Studio.</span></span>


<span data-ttu-id="d535e-189">*Ser det okej ut trots att fel har rapporterats?*</span><span class="sxs-lookup"><span data-stu-id="d535e-189">*Looks OK but reported as a failure?*</span></span> <span data-ttu-id="d535e-190">Kontrollera alla hello-avbildningar, skript, formatmallar och andra filer som lästs in av hello-sidan.</span><span class="sxs-lookup"><span data-stu-id="d535e-190">Check all hello images, scripts, style sheets, and any other files loaded by hello page.</span></span> <span data-ttu-id="d535e-191">Om någon av dem inte rapporteras hello test som misslyckad, även om hello huvudsakliga HTML-sidan läses in OK.</span><span class="sxs-lookup"><span data-stu-id="d535e-191">If any of them fails, hello test is reported as failed, even if hello main html page loads OK.</span></span>

<span data-ttu-id="d535e-192">*Inga relaterade objekt?*</span><span class="sxs-lookup"><span data-stu-id="d535e-192">*No related items?*</span></span> <span data-ttu-id="d535e-193">Om du har konfigurerat Application Insights för din app på serversidan kan detta bero på att [sampling](app-insights-sampling.md) pågår.</span><span class="sxs-lookup"><span data-stu-id="d535e-193">If you have Application Insights set up for your server-side application, that may be because [sampling](app-insights-sampling.md) is in operation.</span></span> 

## <a name="multi-step-web-tests"></a><span data-ttu-id="d535e-194">Webbtester med flera steg</span><span class="sxs-lookup"><span data-stu-id="d535e-194">Multi-step web tests</span></span>
<span data-ttu-id="d535e-195">Du kan övervaka ett scenario med en serie URL:er.</span><span class="sxs-lookup"><span data-stu-id="d535e-195">You can monitor a scenario that involves a sequence of URLs.</span></span> <span data-ttu-id="d535e-196">Till exempel om du övervakar en försäljning webbplats, kan du testa att lägga till objekt toohello kundvagn fungerar.</span><span class="sxs-lookup"><span data-stu-id="d535e-196">For example, if you are monitoring a sales website, you can test that adding items toohello shopping cart works correctly.</span></span>

> [!NOTE] 
> <span data-ttu-id="d535e-197">Det utgår en avgift för webbtester med flera steg.</span><span class="sxs-lookup"><span data-stu-id="d535e-197">There is a charge for multi-step web tests.</span></span> <span data-ttu-id="d535e-198">[Prisschema](http://azure.microsoft.com/pricing/details/application-insights/).</span><span class="sxs-lookup"><span data-stu-id="d535e-198">[Pricing scheme](http://azure.microsoft.com/pricing/details/application-insights/).</span></span>
> 

<span data-ttu-id="d535e-199">toocreate ett test i flera steg du registrera hello scenario med hjälp av Visual Studio Enterprise och överför hello inspelning tooApplication insikter.</span><span class="sxs-lookup"><span data-stu-id="d535e-199">toocreate a multi-step test, you record hello scenario by using Visual Studio Enterprise, and then upload hello recording tooApplication Insights.</span></span> <span data-ttu-id="d535e-200">Application Insights spelar upp hello scenario med intervall och verifierar hello svar.</span><span class="sxs-lookup"><span data-stu-id="d535e-200">Application Insights replays hello scenario at intervals and verifies hello responses.</span></span>

> [!NOTE]
> <span data-ttu-id="d535e-201">Du kan inte använda kodade funktioner eller loopar i dina tester.</span><span class="sxs-lookup"><span data-stu-id="d535e-201">You can't use coded functions or loops in your tests.</span></span> <span data-ttu-id="d535e-202">hello test måste finnas helt i hello .webtest skript.</span><span class="sxs-lookup"><span data-stu-id="d535e-202">hello test must be contained completely in hello .webtest script.</span></span> <span data-ttu-id="d535e-203">Du kan dock använda standardplugin-program.</span><span class="sxs-lookup"><span data-stu-id="d535e-203">However, you can use standard plugins.</span></span>
>

#### <a name="1-record-a-scenario"></a><span data-ttu-id="d535e-204">1. Spela in ett scenario</span><span class="sxs-lookup"><span data-stu-id="d535e-204">1. Record a scenario</span></span>
<span data-ttu-id="d535e-205">Använd Visual Studio Enterprise toorecord en webbsession.</span><span class="sxs-lookup"><span data-stu-id="d535e-205">Use Visual Studio Enterprise toorecord a web session.</span></span>

1. <span data-ttu-id="d535e-206">Skapa ett testprojekt för webbprestanda.</span><span class="sxs-lookup"><span data-stu-id="d535e-206">Create a Web performance test project.</span></span>

    ![Skapa ett projekt i Visual Studio Enterprise-utgåvan från hello webbprestanda och belastningstest mall.](./media/app-insights-monitor-web-app-availability/appinsights-71webtest-multi-vs-create.png)

 * <span data-ttu-id="d535e-208">*Ser hello webbprestanda och belastningstest mallen?*</span><span class="sxs-lookup"><span data-stu-id="d535e-208">*Don't see hello Web Performance and Load Test template?*</span></span> <span data-ttu-id="d535e-209">- Stäng Visual Studio Enterprise.</span><span class="sxs-lookup"><span data-stu-id="d535e-209">- Close Visual Studio Enterprise.</span></span> <span data-ttu-id="d535e-210">Öppna **Visual Studio Installer** toomodify Visual Studio Enterprise-installation.</span><span class="sxs-lookup"><span data-stu-id="d535e-210">Open **Visual Studio Installer** toomodify your Visual Studio Enterprise installation.</span></span> <span data-ttu-id="d535e-211">Välj **Web Performance and load testing tools** (Verktyg för webbprestanda- och inläsningstest) under **Individual Components** (Enskilda komponenter).</span><span class="sxs-lookup"><span data-stu-id="d535e-211">Under **Individual Components**, select **Web Performance and load testing tools**.</span></span>

2. <span data-ttu-id="d535e-212">Öppna hello .webtest fil och börja spela in.</span><span class="sxs-lookup"><span data-stu-id="d535e-212">Open hello .webtest file and start recording.</span></span>

    ![Öppna hello .webtest filen och klicka på posten.](./media/app-insights-monitor-web-app-availability/appinsights-71webtest-multi-vs-start.png)
3. <span data-ttu-id="d535e-214">Hello användaråtgärder som du vill använda toosimulate i testet: Öppna din webbplats, lägga till en produkt toohello kundvagn och så vidare.</span><span class="sxs-lookup"><span data-stu-id="d535e-214">Do hello user actions you want toosimulate in your test: open your website, add a product toohello cart, and so on.</span></span> <span data-ttu-id="d535e-215">Stoppa sedan testet.</span><span class="sxs-lookup"><span data-stu-id="d535e-215">Then stop your test.</span></span>

    ![Hej webbtestet lådan körs i Internet Explorer.](./media/app-insights-monitor-web-app-availability/appinsights-71webtest-multi-vs-record.png)

    <span data-ttu-id="d535e-217">Håll scenariot kort.</span><span class="sxs-lookup"><span data-stu-id="d535e-217">Don't make a long scenario.</span></span> <span data-ttu-id="d535e-218">Det finns en gräns på 100 steg och 2 minuter.</span><span class="sxs-lookup"><span data-stu-id="d535e-218">There's a limit of 100 steps and 2 minutes.</span></span>
4. <span data-ttu-id="d535e-219">Redigera hello test till:</span><span class="sxs-lookup"><span data-stu-id="d535e-219">Edit hello test to:</span></span>

   * <span data-ttu-id="d535e-220">Lägg till verifieringar toocheck hello emot text och svar koder.</span><span class="sxs-lookup"><span data-stu-id="d535e-220">Add validations toocheck hello received text and response codes.</span></span>
   * <span data-ttu-id="d535e-221">Ta bort eventuella överflödiga interaktioner.</span><span class="sxs-lookup"><span data-stu-id="d535e-221">Remove any superfluous interactions.</span></span> <span data-ttu-id="d535e-222">Du kan också ta bort beroende begäranden för bilder eller tooad eller spåra platser.</span><span class="sxs-lookup"><span data-stu-id="d535e-222">You could also remove dependent requests for pictures or tooad or tracking sites.</span></span>

     <span data-ttu-id="d535e-223">Kom ihåg att du bara redigera hello testskript - du kan inte lägga till egen kod eller anropa andra webbtester.</span><span class="sxs-lookup"><span data-stu-id="d535e-223">Remember that you can only edit hello test script - you can't add custom code or call other web tests.</span></span> <span data-ttu-id="d535e-224">Infoga inte slingor i hello test.</span><span class="sxs-lookup"><span data-stu-id="d535e-224">Don't insert loops in hello test.</span></span> <span data-ttu-id="d535e-225">Du kan använda standard-plugin-program för webbtester.</span><span class="sxs-lookup"><span data-stu-id="d535e-225">You can use standard web test plug-ins.</span></span>
5. <span data-ttu-id="d535e-226">Kör hello test i Visual Studio toomake att den fungerar.</span><span class="sxs-lookup"><span data-stu-id="d535e-226">Run hello test in Visual Studio toomake sure it works.</span></span>

    <span data-ttu-id="d535e-227">hello web testaren öppnar en webbläsare och upprepas hello åtgärder som du antecknade.</span><span class="sxs-lookup"><span data-stu-id="d535e-227">hello web test runner opens a web browser and repeats hello actions you recorded.</span></span> <span data-ttu-id="d535e-228">Kontrollera att det fungerar som förväntat.</span><span class="sxs-lookup"><span data-stu-id="d535e-228">Make sure it works as you expect.</span></span>

    ![Öppna hello .webtest filen i Visual Studio och klicka på Kör.](./media/app-insights-monitor-web-app-availability/appinsights-71webtest-multi-vs-run.png)

#### <a name="2-upload-hello-web-test-tooapplication-insights"></a><span data-ttu-id="d535e-230">2. Överför hello web test tooApplication insikter</span><span class="sxs-lookup"><span data-stu-id="d535e-230">2. Upload hello web test tooApplication Insights</span></span>
1. <span data-ttu-id="d535e-231">Skapa ett webbtest i hello Application Insights-portalen.</span><span class="sxs-lookup"><span data-stu-id="d535e-231">In hello Application Insights portal, create a web test.</span></span>

    ![Välj Lägg till på hello web tester bladet.](./media/app-insights-monitor-web-app-availability/16-another-test.png)
2. <span data-ttu-id="d535e-233">Markera flera steg test och överför hello .webtest-fil.</span><span class="sxs-lookup"><span data-stu-id="d535e-233">Select multi-step test, and upload hello .webtest file.</span></span>

    ![Välj webbtestet med flera steg.](./media/app-insights-monitor-web-app-availability/appinsights-71webtestUpload.png)

    <span data-ttu-id="d535e-235">Ange hello test platser, frekvens och avisering parametrar i hello testar samma sätt som för ping.</span><span class="sxs-lookup"><span data-stu-id="d535e-235">Set hello test locations, frequency, and alert parameters in hello same way as for ping tests.</span></span>

#### <a name="3-see-hello-results"></a><span data-ttu-id="d535e-236">3. Se hello resultat</span><span class="sxs-lookup"><span data-stu-id="d535e-236">3. See hello results</span></span>

<span data-ttu-id="d535e-237">Visa testet resultat och eventuella fel i hello samma sätt som enda-url-tester.</span><span class="sxs-lookup"><span data-stu-id="d535e-237">View your test results and any failures in hello same way as single-url tests.</span></span>

<span data-ttu-id="d535e-238">Dessutom kan du hämta hello test resultat tooview dem i Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="d535e-238">In addition, you can download hello test results tooview them in Visual Studio.</span></span>

#### <a name="too-many-failures"></a><span data-ttu-id="d535e-239">För många fel?</span><span class="sxs-lookup"><span data-stu-id="d535e-239">Too many failures?</span></span>

* <span data-ttu-id="d535e-240">En vanlig orsak till felet är att hello testet körs för lång.</span><span class="sxs-lookup"><span data-stu-id="d535e-240">A common reason for failure is that hello test runs too long.</span></span> <span data-ttu-id="d535e-241">Det får inte köras längre än två minuter.</span><span class="sxs-lookup"><span data-stu-id="d535e-241">It mustn't run longer than two minutes.</span></span>

* <span data-ttu-id="d535e-242">Glöm inte att alla hello resurser på en sida måste läsa in korrekt för hello test toosucceed, inklusive skript, formatmallar, bilder och så vidare.</span><span class="sxs-lookup"><span data-stu-id="d535e-242">Don't forget that all hello resources of a page must load correctly for hello test toosucceed, including scripts, style sheets, images, and so forth.</span></span>

* <span data-ttu-id="d535e-243">Hej webbtest helt finnas i hello .webtest skript: du kan inte använda kodade i hello test.</span><span class="sxs-lookup"><span data-stu-id="d535e-243">hello web test must be entirely contained in hello .webtest script: you can't use coded functions in hello test.</span></span>

### <a name="plugging-time-and-random-numbers-into-your-multi-step-test"></a><span data-ttu-id="d535e-244">Använda tid och slumptal i flerstegstest</span><span class="sxs-lookup"><span data-stu-id="d535e-244">Plugging time and random numbers into your multi-step test</span></span>
<span data-ttu-id="d535e-245">Anta att du testar ett verktyg som hämtar tidsberoende data, till exempel aktier från ett externt flöde.</span><span class="sxs-lookup"><span data-stu-id="d535e-245">Suppose you're testing a tool that gets time-dependent data such as stocks from an external feed.</span></span> <span data-ttu-id="d535e-246">När du registrerar ditt webbtest toouse vissa tider är, men du har angett dem som parametrar av hello testa kan StartTime och EndTime.</span><span class="sxs-lookup"><span data-stu-id="d535e-246">When you record your web test, you have toouse specific times, but you set them as parameters of hello test, StartTime and EndTime.</span></span>

![Ett webbtest med parametrar.](./media/app-insights-monitor-web-app-availability/appinsights-72webtest-parameters.png)

<span data-ttu-id="d535e-248">När du kör hello test, som EndTime toobe hello finns alltid tid och StartTime bör vara 15 minuter sedan.</span><span class="sxs-lookup"><span data-stu-id="d535e-248">When you run hello test, you'd like EndTime always toobe hello present time, and StartTime should be 15 minutes ago.</span></span>

<span data-ttu-id="d535e-249">Web Test plugin-program ger hello sätt toodo parameterstyra gånger.</span><span class="sxs-lookup"><span data-stu-id="d535e-249">Web Test Plug-ins provide hello way toodo parameterize times.</span></span>

1. <span data-ttu-id="d535e-250">Lägg till ett plugin-program för webbtester för varje variabelt parametervärde som du behöver.</span><span class="sxs-lookup"><span data-stu-id="d535e-250">Add a web test plug-in for each variable parameter value you want.</span></span> <span data-ttu-id="d535e-251">I hello test verktygsfältet väljer **lägga till Web testa plugin-programmet**.</span><span class="sxs-lookup"><span data-stu-id="d535e-251">In hello web test toolbar, choose **Add Web Test Plugin**.</span></span>

    ![Välj Lägg till plugin-program för webbtest och välj en typ.](./media/app-insights-monitor-web-app-availability/appinsights-72webtest-plugins.png)

    <span data-ttu-id="d535e-253">I det här exemplet använder vi två instanser av hello datum tid plugin-program.</span><span class="sxs-lookup"><span data-stu-id="d535e-253">In this example, we use two instances of hello Date Time Plug-in.</span></span> <span data-ttu-id="d535e-254">En instans är för ”15 minuter tidigare” och en annan för ”nu”.</span><span class="sxs-lookup"><span data-stu-id="d535e-254">One instance is for "15 minutes ago" and another for "now."</span></span>
2. <span data-ttu-id="d535e-255">Öppna hello egenskaper för varje plugin-program.</span><span class="sxs-lookup"><span data-stu-id="d535e-255">Open hello properties of each plug-in.</span></span> <span data-ttu-id="d535e-256">Ge det ett namn och ange det toouse hello aktuell tid.</span><span class="sxs-lookup"><span data-stu-id="d535e-256">Give it a name and set it toouse hello current time.</span></span> <span data-ttu-id="d535e-257">För den ena anger du Lägg till minuter = -15.</span><span class="sxs-lookup"><span data-stu-id="d535e-257">For one of them, set Add Minutes = -15.</span></span>

    ![Ange namn, Använd aktuell tid och Lägg till minuter.](./media/app-insights-monitor-web-app-availability/appinsights-72webtest-plugin-parameters.png)
3. <span data-ttu-id="d535e-259">I hello parametrar för web, använder du {{plugin-name}} tooreference ett plugin-namn.</span><span class="sxs-lookup"><span data-stu-id="d535e-259">In hello web test parameters, use {{plug-in name}} tooreference a plug-in name.</span></span>

    ![Använd {{plugin-name}} i hello test-parametern.](./media/app-insights-monitor-web-app-availability/appinsights-72webtest-plugin-name.png)

<span data-ttu-id="d535e-261">Ladda upp din test toohello portal.</span><span class="sxs-lookup"><span data-stu-id="d535e-261">Now, upload your test toohello portal.</span></span> <span data-ttu-id="d535e-262">Hello dynamiska värden används vid varje körning av hello test.</span><span class="sxs-lookup"><span data-stu-id="d535e-262">It uses hello dynamic values on every run of hello test.</span></span>

## <a name="dealing-with-sign-in"></a><span data-ttu-id="d535e-263">Hantera inloggning</span><span class="sxs-lookup"><span data-stu-id="d535e-263">Dealing with sign-in</span></span>
<span data-ttu-id="d535e-264">Om dina användare loggar in tooyour app, har du olika alternativ för att simulera inloggning så att du kan testa sidor bakom hello-inloggning.</span><span class="sxs-lookup"><span data-stu-id="d535e-264">If your users sign in tooyour app, you have various options for simulating sign-in so that you can test pages behind hello sign-in.</span></span> <span data-ttu-id="d535e-265">hello-metod som du använder beror på hello tillhandahålls av hello app.</span><span class="sxs-lookup"><span data-stu-id="d535e-265">hello approach you use depends on hello type of security provided by hello app.</span></span>

<span data-ttu-id="d535e-266">I samtliga fall bör du skapa ett konto i ditt program för hello syftet testning.</span><span class="sxs-lookup"><span data-stu-id="d535e-266">In all cases, you should create an account in your application just for hello purpose of testing.</span></span> <span data-ttu-id="d535e-267">Om möjligt begränsa hello behörigheter för det här testet kontot så att det inte finns någon möjlighet till hello webbtester påverkar verkliga användare.</span><span class="sxs-lookup"><span data-stu-id="d535e-267">If possible, restrict hello permissions of this test account so that there's no possibility of hello web tests affecting real users.</span></span>

### <a name="simple-username-and-password"></a><span data-ttu-id="d535e-268">Enkelt användarnamn och lösenord</span><span class="sxs-lookup"><span data-stu-id="d535e-268">Simple username and password</span></span>
<span data-ttu-id="d535e-269">Registrera ett webbtest i hello vanligt.</span><span class="sxs-lookup"><span data-stu-id="d535e-269">Record a web test in hello usual way.</span></span> <span data-ttu-id="d535e-270">Ta bort cookies först.</span><span class="sxs-lookup"><span data-stu-id="d535e-270">Delete cookies first.</span></span>

### <a name="saml-authentication"></a><span data-ttu-id="d535e-271">SAML-autentisering</span><span class="sxs-lookup"><span data-stu-id="d535e-271">SAML authentication</span></span>
<span data-ttu-id="d535e-272">Använd hello SAML-plugin-programmet som är tillgänglig för web tests.</span><span class="sxs-lookup"><span data-stu-id="d535e-272">Use hello SAML plugin that is available for web tests.</span></span>

### <a name="client-secret"></a><span data-ttu-id="d535e-273">Klienthemlighet</span><span class="sxs-lookup"><span data-stu-id="d535e-273">Client secret</span></span>
<span data-ttu-id="d535e-274">Om din app kräver inloggning med en klienthemlighet använder du det.</span><span class="sxs-lookup"><span data-stu-id="d535e-274">If your app has a sign-in route that involves a client secret, use that route.</span></span> <span data-ttu-id="d535e-275">Azure Active Directory (AAD) är ett exempel på en tjänst som erbjuder inloggning med klienthemligheter.</span><span class="sxs-lookup"><span data-stu-id="d535e-275">Azure Active Directory (AAD) is an example of a service that provides a client secret sign-in.</span></span> <span data-ttu-id="d535e-276">I AAD är hello klienthemlighet hello Appkey.</span><span class="sxs-lookup"><span data-stu-id="d535e-276">In AAD, hello client secret is hello App Key.</span></span>

<span data-ttu-id="d535e-277">Här är ett exempel på ett webbtest för en Azure-webbapp som använder en appnyckel:</span><span class="sxs-lookup"><span data-stu-id="d535e-277">Here's a sample web test of an Azure web app using an app key:</span></span>

![Exempel med en klienthemlighet](./media/app-insights-monitor-web-app-availability/110.png)

1. <span data-ttu-id="d535e-279">Hämta en token från AAD med hjälp av en klienthemlighet (AppKey).</span><span class="sxs-lookup"><span data-stu-id="d535e-279">Get token from AAD using client secret (AppKey).</span></span>
2. <span data-ttu-id="d535e-280">Extrahera ägartoken från svaret.</span><span class="sxs-lookup"><span data-stu-id="d535e-280">Extract bearer token from response.</span></span>
3. <span data-ttu-id="d535e-281">Anropa API: et med ägartoken i hello authorization-huvud.</span><span class="sxs-lookup"><span data-stu-id="d535e-281">Call API using bearer token in hello authorization header.</span></span>

<span data-ttu-id="d535e-282">Kontrollera att hello webbtest är en verklig klient - som är den har en egen app i AAD - och använda dess clientId + appkey.</span><span class="sxs-lookup"><span data-stu-id="d535e-282">Make sure that hello web test is an actual client - that is, it has its own app in AAD - and use its clientId + appkey.</span></span> <span data-ttu-id="d535e-283">Tjänsten provas också har sin egen app i AAD: hello appID URI: N för den här appen avspeglas i hello webbtest i fältet för hello ”resurser”.</span><span class="sxs-lookup"><span data-stu-id="d535e-283">Your service under test also has its own app in AAD: hello appID URI of this app is reflected in hello web test in hello “resource” field.</span></span>

### <a name="open-authentication"></a><span data-ttu-id="d535e-284">Öppen autentisering</span><span class="sxs-lookup"><span data-stu-id="d535e-284">Open Authentication</span></span>
<span data-ttu-id="d535e-285">Ett exempel på öppen autentisering är inloggning med ett Microsoft- eller Google-konto.</span><span class="sxs-lookup"><span data-stu-id="d535e-285">An example of open authentication is signing in with your Microsoft or Google account.</span></span> <span data-ttu-id="d535e-286">Många appar att Använd OAuth innebär hello klienten hemliga alternativ, så att din första skräpposten bör vara tooinvestigate möjligheter.</span><span class="sxs-lookup"><span data-stu-id="d535e-286">Many apps that use OAuth provide hello client secret alternative, so your first tactic should be tooinvestigate that possibility.</span></span>

<span data-ttu-id="d535e-287">Om testet måste logga in med hjälp av OAuth är hello allmänna sättet:</span><span class="sxs-lookup"><span data-stu-id="d535e-287">If your test must sign in using OAuth, hello general approach is:</span></span>

* <span data-ttu-id="d535e-288">Använd ett verktyg som Fiddler tooexamine hello trafik mellan webbläsaren och hello autentisering plats din app.</span><span class="sxs-lookup"><span data-stu-id="d535e-288">Use a tool such as Fiddler tooexamine hello traffic between your web browser, hello authentication site, and your app.</span></span>
* <span data-ttu-id="d535e-289">Utföra minst två inloggningar med olika datorer eller webbläsare, eller vid långt intervall (tooallow token tooexpire).</span><span class="sxs-lookup"><span data-stu-id="d535e-289">Perform two or more sign-ins using different machines or browsers, or at long intervals (tooallow tokens tooexpire).</span></span>
* <span data-ttu-id="d535e-290">Identifiera hello-token som skickas tillbaka från hello autentisera plats som skickas sedan tooyour app-servern efter inloggning genom att jämföra olika sessioner.</span><span class="sxs-lookup"><span data-stu-id="d535e-290">By comparing different sessions, identify hello token passed back from hello authenticating site, that is then passed tooyour app server after sign-in.</span></span>
* <span data-ttu-id="d535e-291">Spela in ett webbtest med hjälp av Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="d535e-291">Record a web test using Visual Studio.</span></span>
* <span data-ttu-id="d535e-292">Parameterstyra hello tokens inställningen hello parametern när hello token har returnerats från hello authenticator och använda den i hello frågan toohello plats.</span><span class="sxs-lookup"><span data-stu-id="d535e-292">Parameterize hello tokens, setting hello parameter when hello token is returned from hello authenticator, and using it in hello query toohello site.</span></span>
  <span data-ttu-id="d535e-293">(Visual Studio försöker tooparameterize hello test, men korrekt parameterstyra inte hello token.)</span><span class="sxs-lookup"><span data-stu-id="d535e-293">(Visual Studio attempts tooparameterize hello test, but does not correctly parameterize hello tokens.)</span></span>


## <a name="performance-tests"></a><span data-ttu-id="d535e-294">Prestandatester</span><span class="sxs-lookup"><span data-stu-id="d535e-294">Performance tests</span></span>
<span data-ttu-id="d535e-295">Du kan köra ett inläsningstest på din webbplats.</span><span class="sxs-lookup"><span data-stu-id="d535e-295">You can run a load test on your website.</span></span> <span data-ttu-id="d535e-296">Som hello tillgänglighetstest, kan du skicka enkla begäranden eller flera steg begäranden från våra punkter runt hello world.</span><span class="sxs-lookup"><span data-stu-id="d535e-296">Like hello availability test, you can send either simple requests or multi-step requests from our points around hello world.</span></span> <span data-ttu-id="d535e-297">Till skillnad från ett tillgänglighetstest skickas många begäranden, som simulerar flera samtidiga användare.</span><span class="sxs-lookup"><span data-stu-id="d535e-297">Unlike an availability test, many requests are sent, simulating multiple simultaneous users.</span></span>

<span data-ttu-id="d535e-298">Hello översikt bladet öppna **inställningar**, **prestandatesterna**.</span><span class="sxs-lookup"><span data-stu-id="d535e-298">From hello Overview blade, open **Settings**, **Performance Tests**.</span></span> <span data-ttu-id="d535e-299">När du skapar ett test du inbjuds tooconnect tooor skapa ett Visual Studio Team Services-konto.</span><span class="sxs-lookup"><span data-stu-id="d535e-299">When you create a test, you are invited tooconnect tooor create a Visual Studio Team Services account.</span></span>

<span data-ttu-id="d535e-300">När hello test har slutförts visas svarstider och slutförandefrekvenser.</span><span class="sxs-lookup"><span data-stu-id="d535e-300">When hello test is complete, you are shown response times and success rates.</span></span>


![Prestandatest](./media/app-insights-monitor-web-app-availability/perf-test.png)

> [!TIP]
> <span data-ttu-id="d535e-302">Använd tooobserve hello effekterna av en prestandatest [Liveströmning](app-insights-live-stream.md) och [Profiler](app-insights-profiler.md).</span><span class="sxs-lookup"><span data-stu-id="d535e-302">tooobserve hello effects of a performance test, use [Live Stream](app-insights-live-stream.md) and [Profiler](app-insights-profiler.md).</span></span>
>

## <a name="automation"></a><span data-ttu-id="d535e-303">Automation</span><span class="sxs-lookup"><span data-stu-id="d535e-303">Automation</span></span>
* <span data-ttu-id="d535e-304">[Använd PowerShell-skript tooset upp ett tillgänglighetstest](app-insights-powershell.md#add-an-availability-test) automatiskt.</span><span class="sxs-lookup"><span data-stu-id="d535e-304">[Use PowerShell scripts tooset up an availability test](app-insights-powershell.md#add-an-availability-test) automatically.</span></span>
* <span data-ttu-id="d535e-305">Konfigurera en [webhook](../monitoring-and-diagnostics/insights-webhooks-alerts.md) som anropas när en avisering genereras.</span><span class="sxs-lookup"><span data-stu-id="d535e-305">Set up a [webhook](../monitoring-and-diagnostics/insights-webhooks-alerts.md) that is called when an alert is raised.</span></span>

## <span data-ttu-id="d535e-306"><a name="qna"></a>Har du några frågor?</span><span class="sxs-lookup"><span data-stu-id="d535e-306"><a name="qna"></a>Questions?</span></span> <span data-ttu-id="d535e-307">Har du problem?</span><span class="sxs-lookup"><span data-stu-id="d535e-307">Problems?</span></span>
* <span data-ttu-id="d535e-308">*Kan jag anropa kod från mitt webbtest?*</span><span class="sxs-lookup"><span data-stu-id="d535e-308">*Can I call code from my web test?*</span></span>

    <span data-ttu-id="d535e-309">Nej.</span><span class="sxs-lookup"><span data-stu-id="d535e-309">No.</span></span> <span data-ttu-id="d535e-310">hello stegen i test hello måste vara i hello .webtest-filen.</span><span class="sxs-lookup"><span data-stu-id="d535e-310">hello steps of hello test must be in hello .webtest file.</span></span> <span data-ttu-id="d535e-311">Och du kan inte anropa andra webbtester eller använda loopar.</span><span class="sxs-lookup"><span data-stu-id="d535e-311">And you can't call other web tests or use loops.</span></span> <span data-ttu-id="d535e-312">Men det finns flera plugin-program som kan vara användbara.</span><span class="sxs-lookup"><span data-stu-id="d535e-312">But there are several plug-ins that you might find helpful.</span></span>
* <span data-ttu-id="d535e-313">*Stöds HTTPS?*</span><span class="sxs-lookup"><span data-stu-id="d535e-313">*Is HTTPS supported?*</span></span>

    <span data-ttu-id="d535e-314">Vi stöder TLS 1.1 och TLS 1.2.</span><span class="sxs-lookup"><span data-stu-id="d535e-314">We support TLS 1.1 and TLS 1.2.</span></span>
* <span data-ttu-id="d535e-315">*Är det någon skillnad mellan ”webbtester” och ”tillgänglighetstester”?*</span><span class="sxs-lookup"><span data-stu-id="d535e-315">*Is there a difference between "web tests" and "availability tests"?*</span></span>

    <span data-ttu-id="d535e-316">hello kan två termer refereras synonymt.</span><span class="sxs-lookup"><span data-stu-id="d535e-316">hello two terms may be referenced interchangeably.</span></span> <span data-ttu-id="d535e-317">Tillgänglighetstester är en generisk term som inkluderar hello enstaka URL-ping testar dessutom toohello webbtester med flera steg.</span><span class="sxs-lookup"><span data-stu-id="d535e-317">Availability tests is a more generic term that includes hello single URL ping tests in addition toohello multi-step web tests.</span></span>
* <span data-ttu-id="d535e-318">*Jag vill toouse tillgänglighetstester på vår interna server som kör bakom en brandvägg.*</span><span class="sxs-lookup"><span data-stu-id="d535e-318">*I'd like toouse availability tests on our internal server that runs behind a firewall.*</span></span>

    <span data-ttu-id="d535e-319">Det finns två möjliga lösningar:</span><span class="sxs-lookup"><span data-stu-id="d535e-319">There are two possible solutions:</span></span>
    
    * <span data-ttu-id="d535e-320">Konfigurera din brandvägg toopermit inkommande begäranden från hello [IP-adresser i vår webbplats testa agenter](app-insights-ip-addresses.md).</span><span class="sxs-lookup"><span data-stu-id="d535e-320">Configure your firewall toopermit incoming requests from hello [IP addresses of our web test agents](app-insights-ip-addresses.md).</span></span>
    * <span data-ttu-id="d535e-321">Skriva egen kod tooperiodically testet interna servern.</span><span class="sxs-lookup"><span data-stu-id="d535e-321">Write your own code tooperiodically test your internal server.</span></span> <span data-ttu-id="d535e-322">Köra hello kod i bakgrunden på en testserver bakom brandväggen.</span><span class="sxs-lookup"><span data-stu-id="d535e-322">Run hello code as a background process on a test server behind your firewall.</span></span> <span data-ttu-id="d535e-323">Testa processen kan skicka resultaten tooApplication insikter med hjälp av [TrackAvailability()](https://docs.microsoft.com/dotnet/api/microsoft.applicationinsights.telemetryclient.trackavailability) API i hello core SDK-paketet.</span><span class="sxs-lookup"><span data-stu-id="d535e-323">Your test process can send its results tooApplication Insights by using [TrackAvailability()](https://docs.microsoft.com/dotnet/api/microsoft.applicationinsights.telemetryclient.trackavailability) API in hello core SDK package.</span></span> <span data-ttu-id="d535e-324">Detta kräver test server toohave utgående åtkomst toohello Application Insights införandet slutpunkten, men det är mycket mindre säkerhetsrisk än hello alternativ för att tillåta inkommande begäranden.</span><span class="sxs-lookup"><span data-stu-id="d535e-324">This requires your test server toohave outgoing access toohello Application Insights ingestion endpoint, but that is a much smaller security risk than hello alternative of permitting incoming requests.</span></span> <span data-ttu-id="d535e-325">hello resultat visas inte i hello tillgänglighet web tester blad, men visas som tillgänglighet resultaten i Analytics, Sök och mått Explorer.</span><span class="sxs-lookup"><span data-stu-id="d535e-325">hello results will not appear in hello availability web tests blades, but appears as availability results in Analytics, Search, and Metric Explorer.</span></span>
* <span data-ttu-id="d535e-326">*Det går inte att överföra ett flerstegstest för webbplatser*</span><span class="sxs-lookup"><span data-stu-id="d535e-326">*Uploading a multi-step web test fails*</span></span>

    <span data-ttu-id="d535e-327">Det finns en storleksgräns på 300 kB.</span><span class="sxs-lookup"><span data-stu-id="d535e-327">There's a size limit of 300 K.</span></span>

    <span data-ttu-id="d535e-328">Loopar stöds inte.</span><span class="sxs-lookup"><span data-stu-id="d535e-328">Loops aren't supported.</span></span>

    <span data-ttu-id="d535e-329">Referenser tooother webbtester stöds inte.</span><span class="sxs-lookup"><span data-stu-id="d535e-329">References tooother web tests aren't supported.</span></span>

    <span data-ttu-id="d535e-330">Datakällor stöds inte.</span><span class="sxs-lookup"><span data-stu-id="d535e-330">Data sources aren't supported.</span></span>
* <span data-ttu-id="d535e-331">*Mitt test med flera steg slutförs inte*</span><span class="sxs-lookup"><span data-stu-id="d535e-331">*My multi-step test doesn't complete*</span></span>

    <span data-ttu-id="d535e-332">Det finns en gräns på 100 förfrågningar per test.</span><span class="sxs-lookup"><span data-stu-id="d535e-332">There's a limit of 100 requests per test.</span></span>

    <span data-ttu-id="d535e-333">Stoppa hello testet om den körs längre än två minuter.</span><span class="sxs-lookup"><span data-stu-id="d535e-333">hello test is stopped if it runs longer than two minutes.</span></span>
* <span data-ttu-id="d535e-334">*Hur kan jag köra ett test med klientcertifikat?*</span><span class="sxs-lookup"><span data-stu-id="d535e-334">*How can I run a test with client certificates?*</span></span>

    <span data-ttu-id="d535e-335">Det stöds tyvärr inte.</span><span class="sxs-lookup"><span data-stu-id="d535e-335">We don't support that, sorry.</span></span>


## <span data-ttu-id="d535e-336"><a name="next"></a>Nästa steg</span><span class="sxs-lookup"><span data-stu-id="d535e-336"><a name="next"></a>Next steps</span></span>
<span data-ttu-id="d535e-337">[Sök i diagnostikloggar][diagnostic]</span><span class="sxs-lookup"><span data-stu-id="d535e-337">[Search diagnostic logs][diagnostic]</span></span>

<span data-ttu-id="d535e-338">[Felsökning][qna]</span><span class="sxs-lookup"><span data-stu-id="d535e-338">[Troubleshooting][qna]</span></span>

[<span data-ttu-id="d535e-339">IP-adresser för webbtestagenter</span><span class="sxs-lookup"><span data-stu-id="d535e-339">IP addresses of web test agents</span></span>](app-insights-ip-addresses.md)

<!--Link references-->

[azure-availability]: ../insights-create-web-tests.md
[diagnostic]: app-insights-diagnostic-search.md
[qna]: app-insights-troubleshoot-faq.md
[start]: app-insights-overview.md
