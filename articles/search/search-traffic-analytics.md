---
title: "aaaSearch trafik Analytics för Azure Search | Microsoft Docs"
description: "Aktivera sökning trafik analytics för Azure Search molnbaserade search-tjänsten i Microsoft Azure, toounlock insikter om dina användare och dina data."
services: search
documentationcenter: 
author: bernitorres
manager: jlembicz
editor: 
ms.assetid: b31d79cf-5924-4522-9276-a1bb5d527b13
ms.service: search
ms.devlang: multiple
ms.workload: na
ms.topic: article
ms.tgt_pltfrm: na
ms.date: 04/05/2017
ms.author: betorres
ms.openlocfilehash: 1d16aa63d05c1c3df1bbfbb4f09ac77705ed9d9f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="what-is-search-traffic-analytics"></a><span data-ttu-id="8221d-103">Vad är Sök trafik analytics</span><span class="sxs-lookup"><span data-stu-id="8221d-103">What is search traffic analytics</span></span>
<span data-ttu-id="8221d-104">Sök trafik analytics är ett mönster för att implementera en feedback-slinga för din söktjänst.</span><span class="sxs-lookup"><span data-stu-id="8221d-104">Search traffic analytics is a pattern for implementing a feedback loop for your search service.</span></span> <span data-ttu-id="8221d-105">Det här mönstret beskriver hello nödvändiga data och hur toocollect den med hjälp av Application Insights, marknadsledande för övervakning av tjänster på flera olika plattformar.</span><span class="sxs-lookup"><span data-stu-id="8221d-105">This pattern describes hello necessary data and how toocollect it using Application Insights, an industry leader for monitoring services in multiple platforms.</span></span>

<span data-ttu-id="8221d-106">Sök trafik analytics kan du få insyn i din söktjänst och låsa upp insikter om dina användare och deras beteende.</span><span class="sxs-lookup"><span data-stu-id="8221d-106">Search traffic analytics lets you gain visibility into your search service and unlock insights about your users and their behavior.</span></span> <span data-ttu-id="8221d-107">Genom att data om vad användarna väljer, är det möjligt toomake beslut som ytterligare förbättrar dina sökinställningar och tooback ut när hello resultat är vad förväntades inte.</span><span class="sxs-lookup"><span data-stu-id="8221d-107">By having data about what your users choose, it's possible toomake decisions that further improve your search experience, and tooback off when hello results are not what expected.</span></span>

<span data-ttu-id="8221d-108">Azure Search ger en telemetri som integrerar Azure Application Insights och Power BI tooprovide djupgående övervakning och spårning.</span><span class="sxs-lookup"><span data-stu-id="8221d-108">Azure Search offers a telemetry solution that integrates Azure Application Insights and Power BI tooprovide in-depth monitoring and tracking.</span></span> <span data-ttu-id="8221d-109">Eftersom interaktion med Azure Search är endast via API: er, implementeras hello telemetri med hello utvecklare sökning, hello instruktionerna i den här sidan.</span><span class="sxs-lookup"><span data-stu-id="8221d-109">Because interaction with Azure Search is only through APIs, hello telemetry must be implemented by hello developers using search, following hello instructions in this page.</span></span>

## <a name="identify-hello-relevant-search-data"></a><span data-ttu-id="8221d-110">Identifiera hello relevanta sökdata</span><span class="sxs-lookup"><span data-stu-id="8221d-110">Identify hello relevant search data</span></span>

<span data-ttu-id="8221d-111">toohave användbar sökning statistik, är det nödvändigt toolog vissa signaler från hello användare av hello sökprogram.</span><span class="sxs-lookup"><span data-stu-id="8221d-111">toohave useful search metrics, it's necessary toolog some signals from hello users of hello search application.</span></span> <span data-ttu-id="8221d-112">Dessa signaler tillkännage innehåll som användare vill och som de anser relevanta tootheir måste.</span><span class="sxs-lookup"><span data-stu-id="8221d-112">These signals signify content that users are interested in and that they consider relevant tootheir needs.</span></span>

<span data-ttu-id="8221d-113">Det finns två signaler Sök trafik Analytics måste:</span><span class="sxs-lookup"><span data-stu-id="8221d-113">There are two signals Search Traffic Analytics needs:</span></span>

1. <span data-ttu-id="8221d-114">Användargenererat Sök händelser: endast sökfrågor som initierats av en användare är intressant.</span><span class="sxs-lookup"><span data-stu-id="8221d-114">User generated search events: only search queries initiated by a user are interesting.</span></span> <span data-ttu-id="8221d-115">Search-begäranden används toopopulate facets, ytterligare innehåll eller interna information, är inte viktiga och ge skeva och bias dina resultat.</span><span class="sxs-lookup"><span data-stu-id="8221d-115">Search requests used toopopulate facets, additional content or any internal information, are not important and they skew and bias your results.</span></span>

2. <span data-ttu-id="8221d-116">Användargenererat på händelser: klick i det här dokumentet vi hänvisa tooa användaren att välja ett särskilt sökresultat som har returnerats från en sökfråga.</span><span class="sxs-lookup"><span data-stu-id="8221d-116">User generated click events: By clicks in this document, we refer tooa user selecting a particular search result returned from a search query.</span></span> <span data-ttu-id="8221d-117">Klicka innebär vanligtvis att dokumentet är ett resultat som är relevanta för en specifik sökfråga.</span><span class="sxs-lookup"><span data-stu-id="8221d-117">A click generally means that a document is a relevant result for a specific search query.</span></span>

<span data-ttu-id="8221d-118">Genom att länka sökningen och klicka på händelser med en Korrelations-id, är det möjligt tooanalyze hello beteenden för användare i ditt program.</span><span class="sxs-lookup"><span data-stu-id="8221d-118">By linking search and click events with a correlation id, it's possible tooanalyze hello behaviors of users on your application.</span></span> <span data-ttu-id="8221d-119">Dessa Sök insikter är omöjligt tooobtain med endast trafik sökloggar.</span><span class="sxs-lookup"><span data-stu-id="8221d-119">These search insights are impossible tooobtain with only search traffic logs.</span></span>

## <a name="how-tooimplement-search-traffic-analytics"></a><span data-ttu-id="8221d-120">Hur tooimplement söka trafik analytics</span><span class="sxs-lookup"><span data-stu-id="8221d-120">How tooimplement search traffic analytics</span></span>

<span data-ttu-id="8221d-121">hello signaler som nämns i föregående hello avsnitt måste samlas in från hello sökprogram som hello användaren interagerar med den.</span><span class="sxs-lookup"><span data-stu-id="8221d-121">hello signals mentioned in hello preceding section must be gathered from hello search application as hello user interacts with it.</span></span> <span data-ttu-id="8221d-122">Application Insights är en utökningsbar övervakningslösning, tillgänglig för flera plattformar med flexibel instrumentation alternativ.</span><span class="sxs-lookup"><span data-stu-id="8221d-122">Application Insights is an extensible monitoring solution, available for multiple platforms, with flexible instrumentation options.</span></span> <span data-ttu-id="8221d-123">Användning av Application Insights kan du dra nytta av hello Power BI Sök rapporter som skapas med Azure Search toomake hello dataanalys enklare.</span><span class="sxs-lookup"><span data-stu-id="8221d-123">Usage of Application Insights lets you take advantage of hello Power BI search reports created by Azure Search toomake hello analysis of data easier.</span></span>

<span data-ttu-id="8221d-124">I hello [portal](https://portal.azure.com) för din Azure Search-tjänst, hello Sök trafik bladet innehåller en fusklapp för att följa det här mönstret telemetri.</span><span class="sxs-lookup"><span data-stu-id="8221d-124">In hello [portal](https://portal.azure.com) page for your Azure Search service, hello Search Traffic Analytics blade contains a cheat sheet for following this telemetry pattern.</span></span> <span data-ttu-id="8221d-125">Du kan också markera eller skapa en Application Insights-resurs och se hello nödvändiga data på ett ställe.</span><span class="sxs-lookup"><span data-stu-id="8221d-125">You can also select or create an Application Insights resource, and see hello necessary data, all in one place.</span></span>

![Sök trafik Analytics instruktioner][1]

### <a name="1-select-an-application-insights-resource"></a><span data-ttu-id="8221d-127">1. Välj en Application Insights-resurs</span><span class="sxs-lookup"><span data-stu-id="8221d-127">1. Select an Application Insights resource</span></span>

<span data-ttu-id="8221d-128">Du måste tooselect toouse en Application Insights-resurs eller skapa en om du inte redan har en.</span><span class="sxs-lookup"><span data-stu-id="8221d-128">You need tooselect an Application Insights resource toouse or create one if you don't have one already.</span></span> <span data-ttu-id="8221d-129">Du kan använda en resurs som har redan i Använd toolog hello krävs anpassade händelser.</span><span class="sxs-lookup"><span data-stu-id="8221d-129">You can use a resource that's already in use toolog hello required custom events.</span></span>

<span data-ttu-id="8221d-130">När du skapar en ny Application Insights-resurs, gäller alla programtyper för det här scenariot.</span><span class="sxs-lookup"><span data-stu-id="8221d-130">When creating a new Application Insights resource, all application types are valid for this scenario.</span></span> <span data-ttu-id="8221d-131">Välj hello något som bäst passar hello-plattform som du använder.</span><span class="sxs-lookup"><span data-stu-id="8221d-131">Select hello one that best fits hello platform you are using.</span></span>

<span data-ttu-id="8221d-132">Du behöver hello instrumentation nyckeln för att skapa hello telemetri klienten för ditt program.</span><span class="sxs-lookup"><span data-stu-id="8221d-132">You need hello instrumentation key for creating hello telemetry client for your application.</span></span> <span data-ttu-id="8221d-133">Du kan hämta den från instrumentpanelen hello Application Insights-portalen eller du hämta det från hello Sök trafik Analytics sida välja hello-instans som du vill toouse.</span><span class="sxs-lookup"><span data-stu-id="8221d-133">You can get it from hello Application Insights portal dashboard, or you can get it from hello Search Traffic Analytics page, selecting hello instance you want toouse.</span></span>

### <a name="2-instrument-your-application"></a><span data-ttu-id="8221d-134">2. Instrumentera ditt program</span><span class="sxs-lookup"><span data-stu-id="8221d-134">2. Instrument your application</span></span>

<span data-ttu-id="8221d-135">Den här fasen är där instrumentera egna sökprogram med hello Application Insights-resurs din hello som skapats i steget ovan.</span><span class="sxs-lookup"><span data-stu-id="8221d-135">This phase is where you instrument your own search application, using hello Application Insights resource your created in hello step above.</span></span> <span data-ttu-id="8221d-136">Det finns fyra steg toothis processen:</span><span class="sxs-lookup"><span data-stu-id="8221d-136">There are four steps toothis process:</span></span>

<span data-ttu-id="8221d-137">**JAG. Skapa en klient telemetri** detta är hello-objekt som skickar händelser toohello Application Insights-resurs.</span><span class="sxs-lookup"><span data-stu-id="8221d-137">**I. Create a telemetry client** This is hello object that sends events toohello Application Insights Resource.</span></span>

<span data-ttu-id="8221d-138">*C#*</span><span class="sxs-lookup"><span data-stu-id="8221d-138">*C#*</span></span>

    private TelemetryClient telemetryClient = new TelemetryClient();
    telemetryClient.InstrumentationKey = "<YOUR INSTRUMENTATION KEY>";

<span data-ttu-id="8221d-139">*JavaScript*</span><span class="sxs-lookup"><span data-stu-id="8221d-139">*JavaScript*</span></span>

    <script type="text/javascript">var appInsights=window.appInsights||function(config){function r(config){t[config]=function(){var i=arguments;t.queue.push(function(){t[config].apply(t,i)})}}var t={config:config},u=document,e=window,o="script",s=u.createElement(o),i,f;s.src=config.url||"//az416426.vo.msecnd.net/scripts/a/ai.0.js";u.getElementsByTagName(o)[0].parentNode.appendChild(s);try{t.cookie=u.cookie}catch(h){}for(t.queue=[],i=["Event","Exception","Metric","PageView","Trace","Dependency"];i.length;)r("track"+i.pop());return r("setAuthenticatedUserContext"),r("clearAuthenticatedUserContext"),config.disableExceptionTracking||(i="onerror",r("_"+i),f=e[i],e[i]=function(config,r,u,e,o){var s=f&&f(config,r,u,e,o);return s!==!0&&t["_"+i](config,r,u,e,o),s}),t}
    ({
    instrumentationKey: "<YOUR INSTRUMENTATION KEY>"
    });
    window.appInsights=appInsights;
    </script>

<span data-ttu-id="8221d-140">För andra språk och plattformar, se hello fullständig [listan](https://docs.microsoft.com/azure/application-insights/app-insights-platforms).</span><span class="sxs-lookup"><span data-stu-id="8221d-140">For other languages and platforms, see hello complete [list](https://docs.microsoft.com/azure/application-insights/app-insights-platforms).</span></span>

<span data-ttu-id="8221d-141">**II. Begära ett Sök-ID för korrelation** toocorrelate search-begäranden med klick är det nödvändigt toohave Korrelations-id som är kopplat dessa händelser.</span><span class="sxs-lookup"><span data-stu-id="8221d-141">**II. Request a Search ID for correlation** toocorrelate search requests with clicks, it's necessary toohave a correlation id that relates these two distinct events.</span></span> <span data-ttu-id="8221d-142">Azure Search innehåller Sök-Id när du begär med ett huvud:</span><span class="sxs-lookup"><span data-stu-id="8221d-142">Azure Search provides you with a Search Id when you request it with a header:</span></span>

<span data-ttu-id="8221d-143">*C#*</span><span class="sxs-lookup"><span data-stu-id="8221d-143">*C#*</span></span>

    // This sample uses hello Azure Search .NET SDK https://www.nuget.org/packages/Microsoft.Azure.Search

    var client = new SearchIndexClient(<ServiceName>, <IndexName>, new SearchCredentials(<QueryKey>)
    var headers = new Dictionary<string, List<string>>() { { "x-ms-azs-return-searchid", new List<string>() { "true" } } };
    var response = await client.Documents.SearchWithHttpMessagesAsync(searchText: searchText, searchParameters: parameters, customHeaders: headers);
    IEnumerable<string> headerValues;
    string searchId = string.Empty;
    if (response.Response.Headers.TryGetValues("x-ms-azs-searchid", out headerValues)){
     searchId = headerValues.FirstOrDefault();
    }

<span data-ttu-id="8221d-144">*JavaScript*</span><span class="sxs-lookup"><span data-stu-id="8221d-144">*JavaScript*</span></span>

    request.setRequestHeader("x-ms-azs-return-searchid", "true");
    request.setRequestHeader("Access-Control-Expose-Headers", "x-ms-azs-searchid");
    var searchId = request.getResponseHeader('x-ms-azs-searchid');

<span data-ttu-id="8221d-145">**III. Sök händelser**</span><span class="sxs-lookup"><span data-stu-id="8221d-145">**III. Log Search events**</span></span>

<span data-ttu-id="8221d-146">Varje gång som en sökbegäran utfärdas av en användare ska du logga som som en sökning-händelse med hello följa schemat på en anpassad Application Insights-händelse:</span><span class="sxs-lookup"><span data-stu-id="8221d-146">Every time that a search request is issued by a user, you should log that as a search event with hello following schema on an Application Insights custom event:</span></span>

<span data-ttu-id="8221d-147">**ServiceName**: (sträng) för tjänsten söknamn **SearchId**: (guid) Unik identifierare för hello sökfråga (kommer in hello search-svar) **indexnamn**: (sträng) för tjänsten sökindex toobe efterfrågas **QueryTerms**: (sträng) söktermer som angetts av användaren hello **ResultCount**: (int) antal dokument som returnerades (kommer in hello search-svar)  **ScoringProfile**: (sträng) namnet på hello bedömningen profil som används, om sådana</span><span class="sxs-lookup"><span data-stu-id="8221d-147">**ServiceName**: (string) search service name **SearchId**: (guid) unique identifier of hello search query (comes in hello search response) **IndexName**: (string) search service index toobe queried **QueryTerms**: (string) search terms entered by hello user **ResultCount**: (int) number of documents that were returned (comes in hello search response) **ScoringProfile**: (string) name of hello scoring profile used, if any</span></span>

> [!NOTE]
> <span data-ttu-id="8221d-148">Begär antal på användargenererat frågor genom att lägga till $count = true tooyour sökfråga.</span><span class="sxs-lookup"><span data-stu-id="8221d-148">Request count on user generated queries by adding $count=true tooyour search query.</span></span> <span data-ttu-id="8221d-149">Mer information [här](https://docs.microsoft.com/rest/api/searchservice/search-documents#request)</span><span class="sxs-lookup"><span data-stu-id="8221d-149">See more information [here](https://docs.microsoft.com/rest/api/searchservice/search-documents#request)</span></span>
>

> [!NOTE]
> <span data-ttu-id="8221d-150">Kom ihåg tooonly loggen sökfrågor som genereras av användare.</span><span class="sxs-lookup"><span data-stu-id="8221d-150">Remember tooonly log search queries that are generated by users.</span></span>
>

<span data-ttu-id="8221d-151">*C#*</span><span class="sxs-lookup"><span data-stu-id="8221d-151">*C#*</span></span>

    var properties = new Dictionary <string, string> {
    {"SearchServiceName", <service name>},
    {"SearchId", <search Id>},
    {"IndexName", <index name>},
    {"QueryTerms", <search terms>},
    {"ResultCount", <results count>},
    {"ScoringProfile", <scoring profile used>}
    };
    telemetryClient.TrackEvent("Search", properties);

<span data-ttu-id="8221d-152">*JavaScript*</span><span class="sxs-lookup"><span data-stu-id="8221d-152">*JavaScript*</span></span>

    appInsights.trackEvent("Search", {
    SearchServiceName: <service name>,
    SearchId: <search id>,
    IndexName: <index name>,
    QueryTerms: <search terms>,
    ResultCount: <results count>,
    ScoringProfile: <scoring profile used>
    });

<span data-ttu-id="8221d-153">**IV. Klicka på Logga händelser**</span><span class="sxs-lookup"><span data-stu-id="8221d-153">**IV. Log Click events**</span></span>

<span data-ttu-id="8221d-154">Varje gång en användare klickar på ett dokument som är en signal som ska loggas för sökningar för analys.</span><span class="sxs-lookup"><span data-stu-id="8221d-154">Every time that a user clicks on a document, that's a signal that must be logged for search analysis purposes.</span></span> <span data-ttu-id="8221d-155">Använda Application Insights anpassade händelser toolog dessa händelser med hello följer schemat:</span><span class="sxs-lookup"><span data-stu-id="8221d-155">Use Application Insights custom events toolog these events with hello following schema:</span></span>

<span data-ttu-id="8221d-156">**ServiceName**: (sträng) för tjänsten söknamn **SearchId**: (guid) Unik identifierare för hello relaterade sökfråga **DocId**: (sträng) dokument-ID **Position** : (int) rangordningen för hello dokument i hello Sök resultatsida</span><span class="sxs-lookup"><span data-stu-id="8221d-156">**ServiceName**: (string) search service name **SearchId**: (guid) unique identifier of hello related search query **DocId**: (string) document identifier **Position**: (int) rank of hello document in hello search results page</span></span>

> [!NOTE]
> <span data-ttu-id="8221d-157">Positionen refererar toohello kardinalkurvelementets ordning i ditt program.</span><span class="sxs-lookup"><span data-stu-id="8221d-157">Position refers toohello cardinal order in your application.</span></span> <span data-ttu-id="8221d-158">Du är ledigt tooset detta number så länge den har alltid hello samma tooallow för jämförelse.</span><span class="sxs-lookup"><span data-stu-id="8221d-158">You are free tooset this number, as long as it's always hello same, tooallow for comparison.</span></span>
>

<span data-ttu-id="8221d-159">*C#*</span><span class="sxs-lookup"><span data-stu-id="8221d-159">*C#*</span></span>

    var properties = new Dictionary <string, string> {
    {"SearchServiceName", <service name>},
    {"SearchId", <search id>},
    {"ClickedDocId", <clicked document id>},
    {"Rank", <clicked document position>}
    };
    telemetryClient.TrackEvent("Click", properties);

<span data-ttu-id="8221d-160">*JavaScript*</span><span class="sxs-lookup"><span data-stu-id="8221d-160">*JavaScript*</span></span>

    appInsights.TrackEvent("Click", {
        SearchServiceName: <service name>,
        SearchId: <search id>,
        ClickedDocId: <clicked document id>,
        Rank: <clicked document position>
    });

### <a name="3-analyze-with-power-bi-desktop"></a><span data-ttu-id="8221d-161">3. Analysera med Power BI Desktop</span><span class="sxs-lookup"><span data-stu-id="8221d-161">3. Analyze with Power BI Desktop</span></span>

<span data-ttu-id="8221d-162">När du har instrumenterats din app och verifiera ditt program är korrekt anslutna tooApplication insikter, kan du använda en fördefinierad mall som skapats med Azure Search för Power BI desktop.</span><span class="sxs-lookup"><span data-stu-id="8221d-162">After you have instrumented your app and verified your application is correctly connected tooApplication Insights, you can use a predefined template created by Azure Search for Power BI desktop.</span></span>
<span data-ttu-id="8221d-163">Den här mallen innehåller diagram och tabeller som hjälper dig se välgrundade beslut tooimprove din sökningsprestanda och relevans.</span><span class="sxs-lookup"><span data-stu-id="8221d-163">This template contains charts and tables that help you make more informed decisions tooimprove your search performance and relevance.</span></span>

<span data-ttu-id="8221d-164">tooinstantiate hello Power BI desktop mallen du behöver tre uppgifter om Application Insights.</span><span class="sxs-lookup"><span data-stu-id="8221d-164">tooinstantiate hello Power BI desktop template, you need three pieces of information about Application Insights.</span></span> <span data-ttu-id="8221d-165">Dessa data finns i hello Sök trafik Analytics sida när du väljer hello resurs toouse</span><span class="sxs-lookup"><span data-stu-id="8221d-165">This data can be found in hello Search Traffic Analytics page, when you select hello resource toouse</span></span>

![Application Insights Data i hello Sök trafik bladet för användningsanalys][2]

<span data-ttu-id="8221d-167">Mått som ingår i hello Power BI desktop mallen:</span><span class="sxs-lookup"><span data-stu-id="8221d-167">Metrics included in hello Power BI desktop template:</span></span>

*   <span data-ttu-id="8221d-168">Klicka på via hastighet (CTR): förhållandet mellan användare och klicka på ett visst dokument toohello antal totala sökningar.</span><span class="sxs-lookup"><span data-stu-id="8221d-168">Click through Rate (CTR): ratio of users who click on a specific document toohello number of total searches.</span></span>
*   <span data-ttu-id="8221d-169">Söker utan klick: villkoren för vanliga frågor som registrerar några klick</span><span class="sxs-lookup"><span data-stu-id="8221d-169">Searches without clicks: terms for top queries that register no clicks</span></span>
*   <span data-ttu-id="8221d-170">Mest klickade dokument: mest klickade på dokument-ID: t i hello senaste 24 timmarna, 7 dagar och 30 dagar.</span><span class="sxs-lookup"><span data-stu-id="8221d-170">Most clicked documents: most clicked documents by ID in hello last 24 hours, 7 days, and 30 days.</span></span>
*   <span data-ttu-id="8221d-171">Populära termen dokument par: villkor som resulterar i samma dokument klickade hello sorterade efter klick.</span><span class="sxs-lookup"><span data-stu-id="8221d-171">Popular term-document pairs: terms that result in hello same document clicked, ordered by clicks.</span></span>
*   <span data-ttu-id="8221d-172">Tid tooclick: klick bucketgrupperade av tid sedan hello sökfråga</span><span class="sxs-lookup"><span data-stu-id="8221d-172">Time tooclick: clicks bucketed by time since hello search query</span></span>

![Power BI-mall för att läsa från Application Insights][3]


## <a name="next-steps"></a><span data-ttu-id="8221d-174">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="8221d-174">Next Steps</span></span>
<span data-ttu-id="8221d-175">Instrumentera Sök program tooget kraftfulla och insiktsfulla data om din söktjänst.</span><span class="sxs-lookup"><span data-stu-id="8221d-175">Instrument your search application tooget powerful and insightful data about your search service.</span></span>

<span data-ttu-id="8221d-176">Du hittar mer information om Application Insights [här](https://go.microsoft.com/fwlink/?linkid=842905).</span><span class="sxs-lookup"><span data-stu-id="8221d-176">You can find more information on Application Insights [here](https://go.microsoft.com/fwlink/?linkid=842905).</span></span> <span data-ttu-id="8221d-177">Besök Application Insights [sida med priser](https://azure.microsoft.com/pricing/details/application-insights/) toolearn mer om de olika tjänstnivåerna.</span><span class="sxs-lookup"><span data-stu-id="8221d-177">Visit Application Insights [pricing page](https://azure.microsoft.com/pricing/details/application-insights/) toolearn more about their different service tiers.</span></span>

<span data-ttu-id="8221d-178">Mer information om hur du skapar häpnadsväckande rapporter.</span><span class="sxs-lookup"><span data-stu-id="8221d-178">Learn more about creating amazing reports.</span></span> <span data-ttu-id="8221d-179">Se [komma igång med Power BI Desktop](https://powerbi.microsoft.com/en-us/documentation/powerbi-desktop-getting-started/) information</span><span class="sxs-lookup"><span data-stu-id="8221d-179">See [Getting started with Power BI Desktop](https://powerbi.microsoft.com/en-us/documentation/powerbi-desktop-getting-started/) for details</span></span>

<!--Image references-->
[1]: ./media/search-traffic-analytics/AzureSearch-TrafficAnalytics.png
[2]: ./media/search-traffic-analytics/AzureSearch-AppInsightsData.png
[3]: ./media/search-traffic-analytics/AzureSearch-PBITemplate.png
