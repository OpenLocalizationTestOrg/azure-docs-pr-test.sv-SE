---
title: "aaaUser, session och händelsen analys i Azure Application Insights | Microsoft docs"
description: "Demografisk analys av användare av ditt webbprogram."
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
ms.openlocfilehash: 152ab90e9a25c03087d3ebbde1263ec72acb227e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="users-sessions-and-events-analysis-in-application-insights"></a>Användare, sessioner och händelser för analys i Application Insights

Ta reda om personer använder ditt webbprogram, vilka sidor som de är mest intresserad av, där användarna finns, vilka webbläsare och operativsystem som de använder. Analysera affärs- och användningsdata telemetri med hjälp av [Azure Application Insights](app-insights-overview.md).

## <a name="get-started"></a>Kom igång

Om du ännu inte ser data i hello användare, sessioner och händelser blad i hello Application Insights-portalen [Lär dig hur tooget igång med verktyg för användning av hello](app-insights-usage-overview.md).

## <a name="hello-users-sessions-and-events-segmentation-tool"></a>hello användare, sessioner och händelser segmentering-verktyget

Tre hello användning blad använder hello samma verktyget-tooslice och rapportanvändarna telemetri från ditt program från tre perspektiv. Genom filtrering och dela hello data kan få du insikter om hello relativa användning av olika sidor och funktioner.

* **Användare verktyget**: hur många som används för appen och dess funktioner.  Användare ska räknas med anonyma ID: N som lagras i cookies i webbläsaren. En enda person som använder olika webbläsare eller datorer som ska räknas som flera användare.
* **Sessioner verktyget**: hur många sessioner för användaraktiviteten finns det vissa sidor och funktioner i din app. En session räknas efter en halvtimme användaren inaktivitet, eller efter kontinuerlig 24 timmar för användning.
* **Händelser verktyget**: hur ofta vissa sidor och funktioner i din app används. Vyn sida räknas när en webbläsare läser en sida från din app, förutsatt att du har [instrumenterats den](app-insights-javascript.md). 

    En anpassad händelse representerar en förekomst av något händer i din app, ofta en användarinteraktion som en knapp Klicka eller hello slutförandet av en uppgift. Du infoga kod i din app för[generera anpassade händelser](app-insights-api-custom-events-metrics.md#trackevent).

![Användning-verktyget](./media/app-insights-usage-segmentation/users.png)

## <a name="querying-for-certain-users"></a>Frågor för vissa användare 

Utforska olika grupper av användare genom att justera hello frågealternativ hello överst i hello användare verktyget: 

* Som används: Välj anpassade händelser och sidvisningar. 
* Under: Välja ett tidsintervall. 
* Av: Välj hur toobucket hello data, antingen genom en viss tidsperiod eller en annan egenskap, till exempel webbläsare eller ort. 
* Delar av: Välj en egenskap av vilken toosplit eller segment hello-data. 
* Lägg till filter: Begränsa hello frågan toocertain användare, sessioner eller händelser utifrån deras egenskaper, till exempel webbläsare eller ort. 
 
## <a name="saving-and-sharing-reports"></a>Spara och dela rapporter 
Du kan spara användare rapporter, antingen privat bara tooyou under hello Mina rapporter eller delas med alla andra med åtkomst toothis Application Insights-resurs i hello delade rapporter.  
 
När du sparar en rapport eller redigerar egenskaper, väljer du ”aktuellt relativt tidsintervall” toosave en rapport kommer kontinuerligt uppdateras data, gå tillbaka vissa fast tidsperiod.  
 
Välj ”aktuellt absolut tidsintervall” toosave en rapport med en fast mängd data. Tänk på att data i Application Insights bara lagras under 90 dagarna, så om du har gått mer än 90 dagar sedan en rapport med ett absolut tidsintervall har sparats, hello rapporten visas tom. 
 
## <a name="example-instances"></a>Exempel instanser

hello instanser avsnittet visar information om en handfull enskilda användare, sessioner och händelser som täcks av hello aktuella frågan. Med tanke på och utforska hello beteenden för enskilda användare i tillägg tooaggregates kan ge insikter om hur personer faktiskt använder din app. 
 
## <a name="insights"></a>Insikter 

hello insikter sidopanelen visar stora kluster med användare som har gemensamma egenskaper. Dessa kluster kan avslöja konstigt trender i hur personer använder din app. Om exempelvis 40% för alla hello användning av din app kommer från personer som använder en enda funktion.  


## <a name="next-steps"></a>Nästa steg
- tooenable användning inträffar, börja skicka [anpassade händelser](https://docs.microsoft.com/en-us/azure/application-insights/app-insights-api-custom-events-metrics#trackevent) eller [sidvisningar](https://docs.microsoft.com/azure/application-insights/app-insights-api-custom-events-metrics#page-views).
- Om du redan skickar anpassade händelser eller sidvyer utforska hello användning verktyg toolearn hur användare att använda tjänsten.
    - [Trattar](usage-funnels.md)
    - [Kvarhållning](app-insights-usage-retention.md)
    - [Användarflöden](app-insights-usage-flows.md)
    - [Arbetsböcker](app-insights-usage-workbooks.md)
    - [Lägg till användarkontext](app-insights-usage-send-user-context.md)

