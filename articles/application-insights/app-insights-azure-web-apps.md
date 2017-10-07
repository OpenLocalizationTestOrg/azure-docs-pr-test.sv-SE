---
title: aaaMonitor Azure web app prestanda | Microsoft Docs
description: "Övervakning av programprestanda för Azure-webbappar. Skapa diagram över inläsnings- och svarstider och beroendeinformation och ställ in aviseringar för prestanda."
services: application-insights
documentationcenter: .net
author: CFreemanwa
manager: carmonm
ms.assetid: 0b2deb30-6ea8-4bc4-8ed0-26765b85149f
ms.service: azure-portal
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 05/05/2017
ms.author: bwren
ms.openlocfilehash: d1083254e5c504b18f2ac5ae2368610dc2790436
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="monitor-azure-web-app-performance"></a>Övervaka prestanda för Azure-webbappar
I hello [Azure Portal](https://portal.azure.com) du kan konfigurera övervakning av programprestanda för din [Azure-webbappar](../app-service-web/app-service-web-overview.md). [Azure Application Insights](app-insights-overview.md) instruments din app toosend telemetri om dess aktiviteter toohello Application Insights-tjänsten, där den lagras och analyseras. Det, mått diagram och sökverktyg kan användas för toohelp diagnostisera problem, förbättra prestanda och utvärdera användning.

## <a name="run-time-or-build-time"></a>Vid körning eller utveckling
Du kan konfigurera övervakning av instrumentering hello app på två sätt:

* **Vid körning** – Du kan välja ett tillägg för prestandaövervakning när din webbapp redan är live. Det är inte nödvändigt toorebuild eller installera om appen. Du får en standarduppsättning med paket som övervakar svarstider, framgångsfrekvens, undantag, beroenden och så vidare. 
* **Vid utveckling** – Du kan installera ett paket i din app i samband med utvecklingen. Det här alternativet är mer flexibelt. I tillägg toohello samma standard paket, du kan skriva kod toocustomize hello telemetri eller toosend egna telemetri. Du kan logga specifika aktiviteter eller post händelser enligt toohello semantiken för din app-domän. 

## <a name="run-time-instrumentation-with-application-insights"></a>Instrumentering i samband med körning med Application Insights
Om du redan kör en webbapp i Azure har du redan tillgång till viss övervakning: begärande- och felfrekvens. Lägg till Application Insights tooget mer, till exempel svarstider, övervakning anrop toodependencies, smart identifiering och hello kraftfulla Log Analytics-frågespråket. 

1. **Välj Application Insights** hello Azure på Kontrollpanelen för ditt webbprogram.
   
    ![Välj Application Insights under Övervakning](./media/app-insights-azure-web-apps/05-extend.png)
   
   * Välj toocreate en ny resurs, såvida du inte redan angett Application Insights-resurs för den här appen med en annan väg.
2. **Instrumentera din webbapp** när Application Insights har installerats. 
   
    ![Instrumentera din webbapp](./media/app-insights-azure-web-apps/restart-web-app-for-insights.png)

   **Aktivera övervakning på klientsidan** för sidvy och användartelemetri.

   * Välj Inställningar > Programinställningar
   * Under Appinställningar lägger du till ett nytt nyckel/värde-par: 
   
    Nyckel: `APPINSIGHTS_JAVASCRIPT_ENABLED` 
    
    Värde:`true`

   * **Spara** hello inställningar och **starta om** din app.
3. **Övervaka din app**.  [Expore hello data](#explore-the-data).

Senare kan skapa du hello app med Application Insights om du vill.

*Hur jag ta bort Application Insights eller växla toosending tooanother resurs?*

* I Azure, öppna hello web app kontroll bladet och under utvecklingsverktyg, öppna **tillägg**. Ta bort hello Application Insights-tillägget. Sedan under övervakning, Välj Application Insights och skapa eller välj hello-resurs som du vill.

## <a name="build-hello-app-with-application-insights"></a>Skapa hello program med Application Insights
Application Insights kan tillhandahålla mer detaljerad telemetri genom installationen av ett SDK i din app. Mer specifikt kan du samla in spårningsloggar, [skriva anpassad telemetri](app-insights-api-custom-events-metrics.md) och få mer detaljerade undantagsrapporter.

1. **I Visual Studio** (2013 uppdatering 2 eller senare) konfigurerar du Application Insights för ditt projekt.

    Högerklicka på hello-projektet och välj **Lägg till > Application Insights** eller **konfigurera Application Insights**.
   
    ![Högerklicka på hello-projektet och välj Lägg till eller konfigurera Application Insights](./media/app-insights-azure-web-apps/03-add.png)
   
    Om du tillfrågas toosign i Använd hello-autentiseringsuppgifter för Azure-konto.
   
    hello-åtgärden har två effekter:
   
   1. Skapar en Application Insights-resurs i Azure, där telemetri lagras, analyseras och visas.
   2. Lägger till hello Application Insights NuGet-paketet tooyour kod (om det inte fanns redan), samt konfigurerar den toosend telemetri toohello Azure-resurs.
2. **Testa hello telemetri** av hello-app som körs på utvecklingsdatorn (F5).
3. **Publicera hello app** tooAzure i hello vanligt. 

*Hur växlar toosending tooa olika Application Insights-resurs?*

* I Visual Studio, högerklicka på hello projekt, väljer **konfigurera Application Insights** och välj hello-resurs som du vill. Du får hello alternativet toocreate en ny resurs. Återskapa och distribuera igen.

## <a name="explore-hello-data"></a>Utforska hello data
1. Hello Application Insights bladet av Kontrollpanelen web app visas Live statistik, som visar begäranden och misslyckade inom en andra eller två av dem sker. Detta är praktiskt när du publicera om din app, så att du genast ser eventuella problem.
2. Klicka dig igenom toohello fullständig Application Insights-resurs.

    ![Klicka dig framåt](./media/app-insights-azure-web-apps/view-in-application-insights.png)

    Du kan också gå dit direkt från Azure-resursnavigeringen.

1. Klicka dig igenom några diagram tooget detalj:
   
    ![Klicka på ett diagram på hello Application Insights översikt blad](./media/app-insights-azure-web-apps/07-dependency.png)
   
    Du kan [anpassa bladen med mätvärden](app-insights-metrics-explorer.md).
2. Klicka dig igenom ytterligare toosee enskilda händelser och deras egenskaper:
   
    ![Klicka på en händelse typen tooopen en sökning filtrerade i denna typ.](./media/app-insights-azure-web-apps/08-requests.png)
   
    Observera hello ”...” länken tooopen alla egenskaper.
   
    Du kan [anpassa sökningar](app-insights-diagnostic-search.md).

För mer avancerade sökningar över din telemetri använder hello [Log Analytics-frågespråket](app-insights-analytics-tour.md).

## <a name="more-telemetry"></a>Mer telemetri

* [Data om webbsidesinläsning](app-insights-javascript.md)
* [Anpassad telemetri](app-insights-api-custom-events-metrics.md)

## <a name="video"></a>Video

> [!VIDEO https://channel9.msdn.com/events/Connect/2016/100/player]

## <a name="next-steps"></a>Nästa steg
* [Kör hello profiler på appen live](app-insights-profiler.md).
* [Azure Functions](https://github.com/christopheranderson/azure-functions-app-insights-sample) – övervaka Azure Functions med Application Insights
* [Aktivera Azure diagnostics](app-insights-azure-diagnostics.md) toobe skickas tooApplication insikter.
* [Övervaka tjänsten hälsa](../monitoring-and-diagnostics/insights-how-to-customize-monitoring.md) toomake att tjänsten är tillgänglig och svarstid.
* [Få aviseringar](../monitoring-and-diagnostics/insights-receive-alert-notifications.md) när drifthändelser inträffar eller när mätvärden överskrider ett tröskelvärde.
* Använd [Programinsikter för JavaScript-appar och webbsidor](app-insights-javascript.md) tooget klienten telemetri från hello webbläsare som besöker en webbsida.
* [Ställ in tillgänglighet webbtester](app-insights-monitor-web-app-availability.md) toobe aviserad om webbplatsen är igång.

