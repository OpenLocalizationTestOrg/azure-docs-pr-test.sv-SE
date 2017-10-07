---
title: "aaaUser kvarhållning analys för webbprogram med Azure Application Insights | Microsoft docs"
description: "Hur många användare tillbaka tooyour app?"
services: application-insights
documentationcenter: 
author: botatoes
manager: carmonm
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: multiple
ms.topic: article
ms.date: 05/03/2017
ms.author: bwren
ms.openlocfilehash: 8bcee5f1611afbd69016ec3eef27832c304762a4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="user-retention-analysis-for-web-applications-with-application-insights"></a>Kvarhållning av Användaranalys för webbprogram med Application Insights

hello kvarhållning funktion i [Azure Application Insights](app-insights-overview.md) kan du analysera hur många användare returnera tooyour appen och hur ofta de utföra vissa åtgärder eller uppnå mål. Om du kör en spel plats, kan du jämföra hello antal användare som returnerar toohello plats efter att förlora spel med hello nummer som returneras efter vann. Den här kunskapen kan hjälpa dig att förbättra både användarupplevelsen och din strategi.

## <a name="get-started"></a>Kom igång

Om du ännu inte ser data i hello kvarhållning verktyg i hello Application Insights-portalen, [Lär dig hur tooget igång med verktyg för användning av hello](app-insights-usage-overview.md).

## <a name="hello-retention-tool"></a>hello kvarhållning verktyget

![Kvarhållningsverktyg](./media/app-insights-usage-retention/retention.png)

1. hello verktygsfältet kan användare toocreate nya kvarhållning rapporter, öppna den befintliga kvarhållning rapporter, spara den aktuella kvarhållning rapporten eller spara som, återställa ändringar som gjorts toosaved rapporter, uppdatera data på hello rapport, dela rapport via e-post eller Direktlänk och åtkomst hello dokumentationssida. 
2. Som standard visas alla användare som har något Kom tillbaka sedan och uppfyllde något annat under en period kvarhållning. Du kan välja en annan kombination av händelser toonarrow hello fokusera på specifika användaraktiviteter.
3. Lägg till ett eller flera filter på Egenskaper. Du kan fokusera på användare i ett visst land eller region. Klicka på **uppdatering** när du har angett hello filter. 
4. hello övergripande kvarhållning diagrammet visar en sammanfattning av användardokumentation över hello tidsperiod. 
5. hello rutnätet visar hello antalet användare behålls bl.a toohello frågebyggaren i #2. Varje rad representerar en kommittén av användare som utförde alla händelser i hello tid tidsperiod visas. Varje cell i hello raden visar hur många av den kommittén returneras minst en gång i en senare tid. Vissa användare kan returnera i mer än en period. 
6. hello insikter kort Visa översta fem initierande händelser och fem returnerade händelser toogive användarna en bättre förståelse för deras kvarhållning rapporten. 

![Kvarhållning musen hovra](./media/app-insights-usage-retention/hover.png)

Användare kan markören över celler i hello kvarhållning tooaccess hello analytics för och verktygstips som förklarar vad hello cell innebär. hello tar Analytics användare toohello Analytics verktyget med en i förväg frågan toogenerate användare från hello cell. 

## <a name="use-business-events-tootrack-retention"></a>Använda business händelser tootrack kvarhållning

tooget hello mest användbara kvarhållning analys, mått händelser som representerar viktiga aktiviteter. 

Många användare kan till exempel öppna en sida i appen utan att spela upp hello-spelet visas. Spåra bara hello sidvisningar skulle därför innehåller en felaktig beräkning av hur många personer som returnerar tooplay hello spelet efter att använda det tidigare. tooget en tydlig bild av returnerar spelare appen bör skicka en anpassad händelse när en användare spelar faktiskt.  

Det är god sed toocode anpassade händelser som representerar viktiga åtgärder och använda dem för kvarhållning analyser. toocapture hello spel utfall, behöver du toowrite en rad med kod toosend anpassade händelsen-tooApplication insikter. Om du skriver i koden för webbsidan hello eller Node.JS, ser ut så här:

```JavaScript
    appinsights.trackEvent("won game");
```

Eller i serverkoden för ASP.NET:

```C#
   telemetry.TrackEvent("won game");
```

[Mer information om anpassade händelser skrivs](app-insights-api-custom-events-metrics.md#trackevent).


## <a name="next-steps"></a>Nästa steg
- tooenable användning inträffar, börja skicka [anpassade händelser](https://docs.microsoft.com/en-us/azure/application-insights/app-insights-api-custom-events-metrics#trackevent) eller [sidvisningar](https://docs.microsoft.com/azure/application-insights/app-insights-api-custom-events-metrics#page-views).
- Om du redan skickar anpassade händelser eller sidvyer utforska hello användning verktyg toolearn hur användare att använda tjänsten.
    - [Användare, sessioner, händelser](app-insights-usage-segmentation.md)
    - [Trattar](usage-funnels.md)
    - [Användarflöden](app-insights-usage-flows.md)
    - [Arbetsböcker](app-insights-usage-workbooks.md)
    - [Lägg till användarkontext](app-insights-usage-send-user-context.md)


