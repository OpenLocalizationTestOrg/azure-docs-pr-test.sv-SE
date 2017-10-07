---
title: "aaaAzure Programinsikter för JavaScript-webbappar | Microsoft Docs"
description: "Hämta sidvisnings- och sessionsantal, webbklientdata och spåra användningsmönster. Identifiera undantag och prestandaproblem på JavaScript-baserade webbsidor."
services: application-insights
documentationcenter: 
author: CFreemanwa
manager: carmonm
ms.assetid: 3b710d09-6ab4-4004-b26a-4fa840039500
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: get-started-article
ms.date: 03/14/2017
ms.author: bwren
ms.openlocfilehash: 986db3c3776471f9f8556f4e09f2d02aad022549
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="application-insights-for-web-pages"></a>Application Insights för webbsidor
Läs mer om hello prestanda och användning av din webbsida eller app. Om du lägger till [Programinsikter](app-insights-overview.md) tooyour sidan skript får du tidsinställningar för sidan läses in och AJAX-anrop, antal och information om webbläsarundantag och AJAX-fel som användare och session räknas. Allt detta kan visas efter sida, klientoperativsystem- och webbläsarversion, geografisk plats och andra dimensioner. Du kan ställa in varningar för antal fel eller långsam sidinläsning. Och infogar spårning av anrop i JavaScript-kod kan du spåra hur hello olika funktioner i tillämpningsprogrammet webbsida används.

Application Insights kan användas med alla webbsidor – du lägger bara till ett stycke JavaScript-kod. Om webbtjänsten är [Java](app-insights-java-get-started.md) eller [ASP.NET](app-insights-asp-net.md) kan du integrera telemetri från dina servrar och klienter.

![Öppna appens resurs på portal.azure.com och klicka på Webbläsare](./media/app-insights-javascript/03.png)

Du behöver en prenumeration för[Microsoft Azure](https://azure.com). Om din grupp har en organisationens prenumeration kan du be hello ägare tooadd Account-tooit. Utveckling och småskalig användning kostar inget.

## <a name="set-up-application-insights-for-your-web-page"></a>Konfigurera Application Insights för din webbsida
Lägg till webbsidor för tooyour hello inläsaren kodfragment, enligt följande.

### <a name="open-or-create-application-insights-resource"></a>Öppna eller skapa en Application Insights-resurs
hello Application Insights-resurs är där data om prestanda och användning av din sida visas. 

Logga in på [Azure-portalen](https://portal.azure.com).

Om du redan har konfigurerat övervakning för hello på serversidan för din app, har du redan en resurs:

![Välj Bläddra, Utvecklartjänster, Application Insights.](./media/app-insights-javascript/01-find.png)

Om du inte redan har en resurs skapar du en:

![Välj Nytt, Utvecklartjänster, Application Insights.](./media/app-insights-javascript/01-create.png)

*Har du redan frågor?* [Mer information om hur du skapar en resurs](app-insights-create-new-resource.md).

### <a name="add-hello-sdk-script-tooyour-app-or-web-pages"></a>Lägg till hello SDK skriptet tooyour app eller webbsidor
Hämta hello skript för webbsidor i Snabbstart:

![Välj Snabbstart, hämta koden toomonitor webbsidor på bladet översikt över din app. Kopiera hello skript.](./media/app-insights-javascript/02-monitor-web-page.png)

Infoga hello skript innan hello `</head>` taggen för varje sida som du vill tootrack. Om din webbplats har en huvudsida, kan du placera hello skript det. Exempel:

* I ett ASP.NET MVC-projekt lägger du till det i `View\Shared\_Layout.cshtml`
* Öppna i en SharePoint-webbplats på hello Kontrollpanelen [platsinställningar / huvudsida](app-insights-sharepoint.md).

hello skriptet innehåller hello instrumentation nyckeln som leder hello data tooyour Application Insights-resurs. 

([Ingående förklaring av hello skript. ](http://apmtips.com/blog/2015/03/18/javascript-snippet-explained/))

*(Om du använder ett välkänt ramverk för webbsidor letar du efter adaptrar till Application Insights. Det finns till exempel [en AngularJS-modul](http://ngmodules.org/modules/angular-appinsights).)*

## <a name="detailed-configuration"></a>Detaljerad konfiguration
Det finns flera [Parametrar](https://github.com/Microsoft/ApplicationInsights-JS/blob/master/API-reference.md#config) som du kan ange, men i de flesta fall behöver du inte göra det. Du kan till exempel inaktivera eller begränsa hello antalet Ajax-anrop som rapporteras per vyn sida (tooreduce trafik). Alternativt kan du ange debug läge toohave telemetri flytta snabbt hello pipeline utan att grupperas.

tooset parametrarna, leta efter den här raden i hello kodstycke och lägga till fler CSV-objekt efter att det:

    })({
      instrumentationKey: "..."
      // Insert here
    });

Hej [tillgängliga parametrar](https://github.com/Microsoft/ApplicationInsights-JS/blob/master/API-reference.md#config) omfattar:

    // Send telemetry immediately without batching.
    // Remember tooremove this when no longer required, as it
    // can affect browser performance.
    enableDebug: boolean,

    // Don't log browser exceptions.
    disableExceptionTracking: boolean,

    // Don't log ajax calls.
    disableAjaxTracking: boolean,

    // Limit number of Ajax calls logged, tooreduce traffic.
    maxAjaxCallsPerView: 10, // default is 500

    // Time page load up tooexecution of first trackPageView().
    overridePageViewDuration: boolean,

    // Set these dynamically for an authenticated user.
    appUserId: string,
    accountId: string,



## <a name="run"></a>Kör din app
Köra ditt webbprogram, använder den en toogenerate telemetri och vänta ett par sekunder. Du kan antingen köra den med hjälp av hello **F5** nyckeln på utvecklingsdatorn, eller publicera den och användarna kan spela upp till den.

Om du vill toocheck hello telemetri att ett webbprogram skickar tooApplication insikter, använda din webbläsare felsökningsverktyg (**F12** i många webbläsare). Informationen skickas toodc.services.visualstudio.com.

## <a name="explore-your-browser-performance-data"></a>Granska informationen om webbläsarens prestanda
Öppna hello webbläsare bladet tooshow samman prestandadata från användarnas webbläsare.

![Öppna appens resurs på portal.azure.com och klicka på Inställningar, Webbläsare.](./media/app-insights-javascript/03.png)

*Ser du inga data än? Klicka på **uppdatera** hello överst på hello sidan. Ser du fortfarande ingenting? Mer information finns i [Felsökning](app-insights-troubleshoot-faq.md).*

hello webbläsare bladet är en [Metrics Explorer-bladet](app-insights-metrics-explorer.md) med förinställda filter och diagraminställningar. Du kan redigera hello tidsintervall, filter och diagram konfiguration om du vill och spara hello resultatet som en favorit. Klicka på **Återställ standardvärden** tooget tillbaka toohello bladet originalkonfigurationen.

## <a name="page-load-performance"></a>Sidinläsningsprestanda
Vid hello är övre ett segmenterade sidinläsningstider-diagram. hello totala höjd hello diagrammet representerar hello genomsnittstiden tooload och visa sidor från din app i användarnas webbläsare. hello tiden mäts från när hello webbläsaren skickar hello inledande HTTP-begäran tills alla synkron belastningen händelser har bearbetats, inklusive layout och skript som körs. De omfattar inte asynkrona åtgärder, till exempel inläsning av webbdelar från AJAX-anrop.

hello diagram segment hello totala sidinläsningstiden till hello [standard tidsinställningar som definieras av W3C](http://www.w3.org/TR/navigation-timing/#processing-model). 

![](./media/app-insights-javascript/08-client-split.png)

Observera att hello *nätverksanslutning* tid ofta är lägre än förväntat, eftersom det är ett medelvärde över alla förfrågningar från hello webbläsare toohello server. Många enskilda förfrågningar har en anslutningstid 0 eftersom det finns redan en aktiv anslutning toohello server.

### <a name="slow-loading"></a>Är inläsningen långsam?
Långsamma sidinläsningar är ytterst frustrerande för användarna. Om hello diagram visar långsam sidan läses in, är det enkelt toodo vissa diagnostiska research.

hello diagrammet visar hello medelvärdet av alla sidan läses in i din app. toosee om hello problem är begränsad tooparticular sidor, leta ytterligare ned hello-bladet, där det finns ett rutnät åtskilda med sidan URL:

![](./media/app-insights-javascript/09-page-perf.png)

Observera hello sidan Visa antalet och standardavvikelsen. Om antalet sidor för hello är mycket låg, sedan påverkar hello problemet inte användarna mycket. En hög standardavvikelse (jämförbara toohello genomsnittlig själva) anger variationen mellan individuella mätningar mycket.

**Zooma in en URL och en sidvisning.** Klicka på varje sida namnet toosee ett blad av webbläsaren diagram filtrerade bara toothat URL; och sedan på en instans för en sida.

![](./media/app-insights-javascript/35.png)

Klicka på `...` för en fullständig lista över egenskaper för den händelsen eller granska hello Ajax-anrop och relaterade händelser. Långsamma Ajax-anrop påverkar hello övergripande sidan inläsningstid om de är synkron. Relaterade händelser inkluderar serverbegäranden efter Hej samma URL (om du har konfigurerat Application Insights på din webbserver).

**Sidprestanda över tid.** Ändra hello inläsningstid för sidvisning rutnätet till en rad diagram toosee tillbaka på hello webbläsare bladet om det fanns toppar vid en viss tidpunkt:

![Klicka på hello huvud hello rutnätet och välj en ny diagramtyp av](./media/app-insights-javascript/10-page-perf-area.png)

**Ändra uppdelningen baserat på andra dimensioner.** Sidorna är kanske långsammare tooload på webbläsare, klientoperativsystem eller användaren lokalt? Lägg till ett nytt diagram och experimentera med hello **Group by** dimension.

![](./media/app-insights-javascript/21.png)

## <a name="ajax-performance"></a>AJAX-prestanda
Kontrollera att alla AJAX-anrop på dina webbsidor fungerar som de ska. De är ofta använda toofill delar av sidan asynkront. Även om hello övergripande kan sidinläsning omedelbart, kan dina användare vara motverkades som startar vid tomt webbdelar, väntar på att data tooappear i dem.

AJAX-anrop från webbsidan visas på hello webbläsare blad som beroenden.

Det finns översiktlig diagram i hello övre delen av hello bladet:

![](./media/app-insights-javascript/31.png)

Och detaljerade rutnät längre ned:

![](./media/app-insights-javascript/33.png)

Klicka på en rad om du vill visa specifik information.

> [!NOTE]
> Om du tar bort hello webbläsare filter på hello-bladet, ingår både servern och AJAX-beroenden i dessa diagram. Klicka på Återställ standardvärden tooreconfigure hello filter.
> 
> 

**toodrill till misslyckade Ajax-anrop** toohello beroende fel rutnätet rullar och klicka sedan på en rad toosee specifika instanser.

![](./media/app-insights-javascript/37.png)


Klicka på `...` för hello fullständig telemetri för Ajax-anrop.

### <a name="no-ajax-calls-reported"></a>Rapporteras inga Ajax-anrop?
AJAX-anrop inkluderar alla HTTP/HTTPS-anrop från hello skript på webbsidan. Om du inte ser dem rapporterade, kontrollera att hello kodstycke inte hello `disableAjaxTracking` eller `maxAjaxCallsPerView` [parametrar](https://github.com/Microsoft/ApplicationInsights-JS/blob/master/API-reference.md#config).

## <a name="browser-exceptions"></a>Webbläsarundantag
På bladet webbläsare hello finns ett undantag sammanfattning diagram och ett rutnät av undantag typer ytterligare ned hello-bladet.

![](./media/app-insights-javascript/39.png)

Om du inte ser webbläsarundantag rapporterade, kontrollera att hello kodstycke inte hello `disableExceptionTracking` [parametern](https://github.com/Microsoft/ApplicationInsights-JS/blob/master/API-reference.md#config).

## <a name="inspect-individual-page-view-events"></a>Granska enskilda sidvisningshändelser

Vanligtvis analyseras telemetri för sidvisningar av Application Insights och du ser endast kumulativa rapporter, som ett genomsnitt av alla användare. Men för felsökningsändamål kan du även titta på enskilda sidvisningshändelser.

Ange filter tooPage vyn i hello diagnostiska Sök-bladet.

![](./media/app-insights-javascript/12-search-pages.png)

Välj en händelse toosee detalj. Klicka på ”...” toosee även detalj hello information på sidan.

> [!NOTE]
> Om du använder [Sök](app-insights-diagnostic-search.md), Observera att du har toomatch hela ord: ”Abou” och ”om” matchar inte ”om”.
> 
> 

Du kan också använda hello kraftfulla [Log Analytics-frågespråket](https://docs.microsoft.com/azure/application-insights/app-insights-analytics-tour#browser-timings-table) toosearch sidvisningar.

### <a name="page-view-properties"></a>Egenskaper för sidvisning
* **Sidvisningens varaktighet** 
  
  * Som standard hello tid det tar tooload hello sida, ladda från klienten begäran toofull (inklusive extra filer men exklusive asynkrona åtgärder, till exempel Ajax-anrop). 
  * Om du ställer in `overridePageViewDuration` i hello [sidan configuration](#detailed-configuration), hello intervallet mellan klienten begäran tooexecution av hello först `trackPageView`. Om du flyttade trackPageView från dess normala position efter hello initiering av hello skript, visas ett annat värde.
  * Om `overridePageViewDuration` anges, och en varaktighet som argument har angetts i hello `trackPageView()` anropa hello argumentvärdet används i stället. 

## <a name="custom-page-counts"></a>Anpassade sidräkningar
Ett antal sidor sker som standard varje gång en ny sida läses in i hello klientens webbläsare.  Men du kanske vill toocount ytterligare sidvisningar. Till exempel en sida kan visa innehållet i flikar och du vill toocount en sida när hello användare växlar flikar. Eller JavaScript-kod på sidan hello kan läsa in nytt innehåll utan att ändra hello webbläsarens URL.

Infoga ett JavaScript-anrop så här på hello lämplig plats i din klientkod:

    appInsights.trackPageView(myPageName);

hello sidnamn kan innehålla hello samma tecken som en URL, men något annat efter ”#” eller ””? ignoreras.

## <a name="usage-tracking"></a>Användningsspårning
Vill du toofind reda på vad användarna göra med din app?

* [Lär dig mer om användningsspårning](app-insights-web-track-usage.md)
* [Lär dig mer om API:er för mätvärden och anpassade händelser](app-insights-api-custom-events-metrics.md).

## <a name="video"></a> Video


> [!VIDEO https://channel9.msdn.com/events/Connect/2016/100/player]



## <a name="next"></a>Nästa steg
* [Spåra användning](app-insights-web-track-usage.md)
* [Anpassade händelser och mätvärden](app-insights-api-custom-events-metrics.md)
* [Skapa – mät – lär](app-insights-web-track-usage.md)

