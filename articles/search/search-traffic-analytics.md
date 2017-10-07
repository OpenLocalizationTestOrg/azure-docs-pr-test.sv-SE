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
# <a name="what-is-search-traffic-analytics"></a>Vad är Sök trafik analytics
Sök trafik analytics är ett mönster för att implementera en feedback-slinga för din söktjänst. Det här mönstret beskriver hello nödvändiga data och hur toocollect den med hjälp av Application Insights, marknadsledande för övervakning av tjänster på flera olika plattformar.

Sök trafik analytics kan du få insyn i din söktjänst och låsa upp insikter om dina användare och deras beteende. Genom att data om vad användarna väljer, är det möjligt toomake beslut som ytterligare förbättrar dina sökinställningar och tooback ut när hello resultat är vad förväntades inte.

Azure Search ger en telemetri som integrerar Azure Application Insights och Power BI tooprovide djupgående övervakning och spårning. Eftersom interaktion med Azure Search är endast via API: er, implementeras hello telemetri med hello utvecklare sökning, hello instruktionerna i den här sidan.

## <a name="identify-hello-relevant-search-data"></a>Identifiera hello relevanta sökdata

toohave användbar sökning statistik, är det nödvändigt toolog vissa signaler från hello användare av hello sökprogram. Dessa signaler tillkännage innehåll som användare vill och som de anser relevanta tootheir måste.

Det finns två signaler Sök trafik Analytics måste:

1. Användargenererat Sök händelser: endast sökfrågor som initierats av en användare är intressant. Search-begäranden används toopopulate facets, ytterligare innehåll eller interna information, är inte viktiga och ge skeva och bias dina resultat.

2. Användargenererat på händelser: klick i det här dokumentet vi hänvisa tooa användaren att välja ett särskilt sökresultat som har returnerats från en sökfråga. Klicka innebär vanligtvis att dokumentet är ett resultat som är relevanta för en specifik sökfråga.

Genom att länka sökningen och klicka på händelser med en Korrelations-id, är det möjligt tooanalyze hello beteenden för användare i ditt program. Dessa Sök insikter är omöjligt tooobtain med endast trafik sökloggar.

## <a name="how-tooimplement-search-traffic-analytics"></a>Hur tooimplement söka trafik analytics

hello signaler som nämns i föregående hello avsnitt måste samlas in från hello sökprogram som hello användaren interagerar med den. Application Insights är en utökningsbar övervakningslösning, tillgänglig för flera plattformar med flexibel instrumentation alternativ. Användning av Application Insights kan du dra nytta av hello Power BI Sök rapporter som skapas med Azure Search toomake hello dataanalys enklare.

I hello [portal](https://portal.azure.com) för din Azure Search-tjänst, hello Sök trafik bladet innehåller en fusklapp för att följa det här mönstret telemetri. Du kan också markera eller skapa en Application Insights-resurs och se hello nödvändiga data på ett ställe.

![Sök trafik Analytics instruktioner][1]

### <a name="1-select-an-application-insights-resource"></a>1. Välj en Application Insights-resurs

Du måste tooselect toouse en Application Insights-resurs eller skapa en om du inte redan har en. Du kan använda en resurs som har redan i Använd toolog hello krävs anpassade händelser.

När du skapar en ny Application Insights-resurs, gäller alla programtyper för det här scenariot. Välj hello något som bäst passar hello-plattform som du använder.

Du behöver hello instrumentation nyckeln för att skapa hello telemetri klienten för ditt program. Du kan hämta den från instrumentpanelen hello Application Insights-portalen eller du hämta det från hello Sök trafik Analytics sida välja hello-instans som du vill toouse.

### <a name="2-instrument-your-application"></a>2. Instrumentera ditt program

Den här fasen är där instrumentera egna sökprogram med hello Application Insights-resurs din hello som skapats i steget ovan. Det finns fyra steg toothis processen:

**JAG. Skapa en klient telemetri** detta är hello-objekt som skickar händelser toohello Application Insights-resurs.

*C#*

    private TelemetryClient telemetryClient = new TelemetryClient();
    telemetryClient.InstrumentationKey = "<YOUR INSTRUMENTATION KEY>";

*JavaScript*

    <script type="text/javascript">var appInsights=window.appInsights||function(config){function r(config){t[config]=function(){var i=arguments;t.queue.push(function(){t[config].apply(t,i)})}}var t={config:config},u=document,e=window,o="script",s=u.createElement(o),i,f;s.src=config.url||"//az416426.vo.msecnd.net/scripts/a/ai.0.js";u.getElementsByTagName(o)[0].parentNode.appendChild(s);try{t.cookie=u.cookie}catch(h){}for(t.queue=[],i=["Event","Exception","Metric","PageView","Trace","Dependency"];i.length;)r("track"+i.pop());return r("setAuthenticatedUserContext"),r("clearAuthenticatedUserContext"),config.disableExceptionTracking||(i="onerror",r("_"+i),f=e[i],e[i]=function(config,r,u,e,o){var s=f&&f(config,r,u,e,o);return s!==!0&&t["_"+i](config,r,u,e,o),s}),t}
    ({
    instrumentationKey: "<YOUR INSTRUMENTATION KEY>"
    });
    window.appInsights=appInsights;
    </script>

För andra språk och plattformar, se hello fullständig [listan](https://docs.microsoft.com/azure/application-insights/app-insights-platforms).

**II. Begära ett Sök-ID för korrelation** toocorrelate search-begäranden med klick är det nödvändigt toohave Korrelations-id som är kopplat dessa händelser. Azure Search innehåller Sök-Id när du begär med ett huvud:

*C#*

    // This sample uses hello Azure Search .NET SDK https://www.nuget.org/packages/Microsoft.Azure.Search

    var client = new SearchIndexClient(<ServiceName>, <IndexName>, new SearchCredentials(<QueryKey>)
    var headers = new Dictionary<string, List<string>>() { { "x-ms-azs-return-searchid", new List<string>() { "true" } } };
    var response = await client.Documents.SearchWithHttpMessagesAsync(searchText: searchText, searchParameters: parameters, customHeaders: headers);
    IEnumerable<string> headerValues;
    string searchId = string.Empty;
    if (response.Response.Headers.TryGetValues("x-ms-azs-searchid", out headerValues)){
     searchId = headerValues.FirstOrDefault();
    }

*JavaScript*

    request.setRequestHeader("x-ms-azs-return-searchid", "true");
    request.setRequestHeader("Access-Control-Expose-Headers", "x-ms-azs-searchid");
    var searchId = request.getResponseHeader('x-ms-azs-searchid');

**III. Sök händelser**

Varje gång som en sökbegäran utfärdas av en användare ska du logga som som en sökning-händelse med hello följa schemat på en anpassad Application Insights-händelse:

**ServiceName**: (sträng) för tjänsten söknamn **SearchId**: (guid) Unik identifierare för hello sökfråga (kommer in hello search-svar) **indexnamn**: (sträng) för tjänsten sökindex toobe efterfrågas **QueryTerms**: (sträng) söktermer som angetts av användaren hello **ResultCount**: (int) antal dokument som returnerades (kommer in hello search-svar)  **ScoringProfile**: (sträng) namnet på hello bedömningen profil som används, om sådana

> [!NOTE]
> Begär antal på användargenererat frågor genom att lägga till $count = true tooyour sökfråga. Mer information [här](https://docs.microsoft.com/rest/api/searchservice/search-documents#request)
>

> [!NOTE]
> Kom ihåg tooonly loggen sökfrågor som genereras av användare.
>

*C#*

    var properties = new Dictionary <string, string> {
    {"SearchServiceName", <service name>},
    {"SearchId", <search Id>},
    {"IndexName", <index name>},
    {"QueryTerms", <search terms>},
    {"ResultCount", <results count>},
    {"ScoringProfile", <scoring profile used>}
    };
    telemetryClient.TrackEvent("Search", properties);

*JavaScript*

    appInsights.trackEvent("Search", {
    SearchServiceName: <service name>,
    SearchId: <search id>,
    IndexName: <index name>,
    QueryTerms: <search terms>,
    ResultCount: <results count>,
    ScoringProfile: <scoring profile used>
    });

**IV. Klicka på Logga händelser**

Varje gång en användare klickar på ett dokument som är en signal som ska loggas för sökningar för analys. Använda Application Insights anpassade händelser toolog dessa händelser med hello följer schemat:

**ServiceName**: (sträng) för tjänsten söknamn **SearchId**: (guid) Unik identifierare för hello relaterade sökfråga **DocId**: (sträng) dokument-ID **Position** : (int) rangordningen för hello dokument i hello Sök resultatsida

> [!NOTE]
> Positionen refererar toohello kardinalkurvelementets ordning i ditt program. Du är ledigt tooset detta number så länge den har alltid hello samma tooallow för jämförelse.
>

*C#*

    var properties = new Dictionary <string, string> {
    {"SearchServiceName", <service name>},
    {"SearchId", <search id>},
    {"ClickedDocId", <clicked document id>},
    {"Rank", <clicked document position>}
    };
    telemetryClient.TrackEvent("Click", properties);

*JavaScript*

    appInsights.TrackEvent("Click", {
        SearchServiceName: <service name>,
        SearchId: <search id>,
        ClickedDocId: <clicked document id>,
        Rank: <clicked document position>
    });

### <a name="3-analyze-with-power-bi-desktop"></a>3. Analysera med Power BI Desktop

När du har instrumenterats din app och verifiera ditt program är korrekt anslutna tooApplication insikter, kan du använda en fördefinierad mall som skapats med Azure Search för Power BI desktop.
Den här mallen innehåller diagram och tabeller som hjälper dig se välgrundade beslut tooimprove din sökningsprestanda och relevans.

tooinstantiate hello Power BI desktop mallen du behöver tre uppgifter om Application Insights. Dessa data finns i hello Sök trafik Analytics sida när du väljer hello resurs toouse

![Application Insights Data i hello Sök trafik bladet för användningsanalys][2]

Mått som ingår i hello Power BI desktop mallen:

*   Klicka på via hastighet (CTR): förhållandet mellan användare och klicka på ett visst dokument toohello antal totala sökningar.
*   Söker utan klick: villkoren för vanliga frågor som registrerar några klick
*   Mest klickade dokument: mest klickade på dokument-ID: t i hello senaste 24 timmarna, 7 dagar och 30 dagar.
*   Populära termen dokument par: villkor som resulterar i samma dokument klickade hello sorterade efter klick.
*   Tid tooclick: klick bucketgrupperade av tid sedan hello sökfråga

![Power BI-mall för att läsa från Application Insights][3]


## <a name="next-steps"></a>Nästa steg
Instrumentera Sök program tooget kraftfulla och insiktsfulla data om din söktjänst.

Du hittar mer information om Application Insights [här](https://go.microsoft.com/fwlink/?linkid=842905). Besök Application Insights [sida med priser](https://azure.microsoft.com/pricing/details/application-insights/) toolearn mer om de olika tjänstnivåerna.

Mer information om hur du skapar häpnadsväckande rapporter. Se [komma igång med Power BI Desktop](https://powerbi.microsoft.com/en-us/documentation/powerbi-desktop-getting-started/) information

<!--Image references-->
[1]: ./media/search-traffic-analytics/AzureSearch-TrafficAnalytics.png
[2]: ./media/search-traffic-analytics/AzureSearch-AppInsightsData.png
[3]: ./media/search-traffic-analytics/AzureSearch-PBITemplate.png
