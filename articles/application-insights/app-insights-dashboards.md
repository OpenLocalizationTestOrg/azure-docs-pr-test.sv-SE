---
title: aaaDashboards och navigering i hello Azure Application Insights | Microsoft Docs
description: "Skapa vyer av din nyckel APM diagram och frågor."
services: application-insights
documentationcenter: 
author: CFreemanwa
manager: carmonm
ms.assetid: 39b0701b-2fec-4683-842a-8a19424f67bd
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: multiple
ms.topic: article
ms.date: 03/14/2017
ms.author: bwren
ms.openlocfilehash: 58811388205643bb672e0405b3226f12d0f447a9
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="navigation-and-dashboards-in-hello-application-insights-portal"></a>Navigering och instrumentpaneler i hello Application Insights-portalen
När du har [konfigurera Application Insights i ditt projekt](app-insights-overview.md), telemetridata om prestanda och användning av din app visas i Application Insights-resurs i ditt projekt i hello [Azure-portalen](https://portal.azure.com).

## <a name="find-your-telemetry"></a>Hitta din telemetri
Logga in toohello [Azure-portalen](https://portal.azure.com) och navigera toohello Application Insights-resurs som du skapade för din app.

![Klicka på Bläddra, Välj Application Insights och sedan din app.](./media/app-insights-dashboards/00-start.png)

hello översikt bladet (sidan) för din app visar en sammanfattning av hello diagnostiska nyckelvärden för din app och är en gateway-toohello andra funktioner i hello-portalen.

![Högre vägar tooview din telemetri](./media/app-insights-dashboards/010-oview.png)

Du kan anpassa någon av hello diagram och rutnät och fästa dem tooa instrumentpanelen. På så sätt kan du kan hämta hello viktiga telemetri från olika appar på en central instrumentpanel.

## <a name="dashboards"></a>Instrumentpaneler
hello först öppna visas när du loggar in toohello [Microsoft Azure-portalen](https://portal.azure.com) är en instrumentpanel. Här kan du sammanfoga hello diagram som är viktigast tooyou för alla dina Azure-resurser, inklusive telemetri från [Azure Application Insights](app-insights-overview.md).

![En anpassad instrumentpanel.](./media/app-insights-dashboards/31.png)

1. **Navigera toospecific resurser** som din app i Application Insights: Använd hello vänster stapel.
2. **Returnerar toohello den aktuella instrumentpanelen**, eller växla tooother senaste vyer: Använd hello listrutan längst upp till vänster.
3. **Växla instrumentpaneler**: Använd hello rullgardinsmenyn på hello instrumentpanelen rubrik
4. **Skapa, redigera och dela instrumentpaneler** i hello instrumentpanelen verktygsfältet.
5. **Redigera hello instrumentpanelen**: hovra över en panel och sedan använda överkanten liggande toomove, anpassa eller ta bort den.

## <a name="add-tooa-dashboard"></a>Lägga till tooa instrumentpanel
När du tittar på ett blad eller uppsättning diagram som är särskilt intressanta, kan du fästa en kopia av den toohello instrumentpanelen. Ser du den nästa gång du kommer tillbaka det.

![toopin ett diagram hovrar över det och klicka på ”...” i hello-huvudet.](./media/app-insights-dashboards/33.png)

1. Fästa diagrammet toodashboard. En kopia av hello schemat visas på hello instrumentpanelen.
2. PIN-kod hello hela bladet toohello instrumentpanelen - visas den på hello instrumentpanel som en panel som du kan klicka på via.
3. Klicka på hello övre vänstra hörnet tooreturn toohello den aktuella instrumentpanelen. Du kan använda hello nedrullningsbara menyn tooreturn toohello aktuell vy.

Observera att diagram grupperas i panelerna: en panel kan innehålla fler än ett schema. Du kan fästa hello hela panelen toohello instrumentpanelen.

hello diagrammet uppdateras automatiskt med en frekvens som är beroende av hello diagrammets tidsintervall:

* Tidsintervallet in too1 timme: uppdatera var femte minut
* Tidsintervallet 1-24 timmar: uppdatera var 15: e minut
* Tidsintervallet ovan 24 timmar: (tidsintervallet) / 60.

### <a name="pin-any-query-in-analytics"></a>Fästa en fråga i Analytics
Du kan också [fästa Analytics](app-insights-analytics-using.md#pin-to-dashboard) diagram tooa [delade](#share-dashboards-with-your-team) instrumentpanelen. Detta ger dig tooadd diagram av en skadlig fråga tillsammans med hello standard mått. 

Resultaten beräknas automatiskt varje timme. Klicka på hello-ikonen på hello diagram toorecalculate omedelbart. (Uppdatera webbläsaren omberäknar inte.)

## <a name="adjust-a-tile-on-hello-dashboard"></a>Justera en panel på instrumentpanelen för hello
Du kan justera när en panel på instrumentpanelen för hello.

![Hovra över ett diagram i ordning tooedit den.](./media/app-insights-dashboards/36.png)

1. Lägg till ett diagram toohello sida vid sida.
2. Ange hello mått, gruppera efter dimension och format (tabell, diagram) i ett diagram.
3. Dra över hello diagram toozoom i; Klicka på hello Ångra knappen tooreset hello timespan; Ange filteregenskaper för hello diagram på hello sida vid sida.
4. Ange panelrubriken.

Paneler fästs från mått explorer blad har fler redigera alternativ än paneler fästs från en översikt över bladet.

hello ursprungliga panelen som du har fäst påverkas inte av ändringarna.

## <a name="switch-between-dashboards"></a>Växla mellan instrumentpaneler
Du kan spara mer än en instrumentpanel och växla mellan dem. När du fäster ett diagram eller bladet läggs de toohello den aktuella instrumentpanelen.

![tooswitch mellan instrumentpaneler, klicka på instrumentpanelen och välj en sparad instrumentpanel. toocreate och spara en ny instrumentpanel, klickar du på nytt. toorearrange, klicka på Redigera.](./media/app-insights-dashboards/32.png)

Du kan till exempel ha en instrumentpanel om du vill visa hela skärmen i hello team plats och en annan för allmänna utvecklingen.

På instrumentpanelen hello visas ett blad som en panel: Klicka på den toogo toohello bladet. Ett diagram replikerar hello diagram på den ursprungliga platsen.

![Klicka på panelen tooopen hello bladet representerar](./media/app-insights-dashboards/35.png)

## <a name="share-dashboards"></a>Dela instrumentpaneler
När du har skapat en instrumentpanel kan dela du den med andra användare.

![Klicka på resurs i hello instrumentpanelen sidhuvud](./media/app-insights-dashboards/41.png)

Lär dig mer om [roller och åtkomstkontroll](app-insights-resources-roles-access-control.md).

## <a name="app-navigation"></a>App-navigering
hello översikt bladet är hello gateway toomore information om din app.

* **Alla diagram eller panelen** – Klicka på panelen eller diagram toosee mer information om vad som visas.

### <a name="overview-blade-buttons"></a>Översikt över bladet knappar
![Översikt över bladet övre navigeringsfält](./media/app-insights-dashboards/app-overview-top-nav.png)

* [**Metrics Explorer** ](app-insights-metrics-explorer.md) -skapa egna diagram av prestanda och användning.
* [**Sök** ](app-insights-diagnostic-search.md) – undersöka specifika instanser av händelser, till exempel begäranden, undantag, eller logga in spårningar.
* [**Analytics** ](app-insights-analytics.md) -kraftfulla frågor via din telemetri.
* **Tidsintervallet** -justera hello-intervall som visas av alla hello diagram på hello-bladet.
* **Ta bort** -ta bort hello Application Insights-resurs för den här appen. Du bör också ta bort hello Application Insights paket från din Appkod, eller redigera hello [instrumentation nyckeln](app-insights-create-new-resource.md#copy-the-instrumentation-key) i din app toodirect telemetri tooa olika Application Insights-resurs.

### <a name="essentials-tab"></a>Fliken Essentials
* [Instrumentation nyckeln](app-insights-create-new-resource.md#copy-the-instrumentation-key) -identifierar den här appen resurs.
* Priser för - och göra funktioner tillgängliga och ange volym versaler.

### <a name="app-navigation-bar"></a>Navigeringsfältet i appen
![Vänstra navigeringsfältet](./media/app-insights-dashboards/app-left-nav-bar.png)

* **Översikt över** -returnerade toohello app översikt bladet.
* **Aktivitetsloggen** -aviseringar och Azure administrativa händelser.
* [**Åtkomstkontroll** ](app-insights-resources-roles-access-control.md) -ge åtkomst tooteam medlemmar med mera.
* [**Taggar** ](../azure-resource-manager/resource-group-using-tags.md) -Använd taggar toogroup din app med andra.

UNDERSÖK

* [**Programavbildningen** ](app-insights-app-map.md) -aktiv karta visar hello serverkomponenter i ditt program härlett från hello beroendeinformation.
* [**Identifiering för smartkort** ](app-insights-proactive-diagnostics.md) -granska de senaste prestanda aviseringarna.
* [**Direktsänd dataström** ](app-insights-live-stream.md) – en fast uppsättning nästan omedelbar statistik, användbart när du distribuerar en ny version eller felsökning.
* [**Tillgänglighet / Webbtester** ](app-insights-monitor-web-app-availability.md) -skickar regelbundet förfrågningar tooyour webbprogrammet från runt hello world.*
* [**Fel, prestanda** ](app-insights-web-monitor-performance.md) -undantag, fel och svar tidsgränsen för begäran tooyour app och förfrågningar från din app för[beroenden](app-insights-asp-net-dependencies.md).
* [**Prestanda** ](app-insights-web-monitor-performance.md) -svarstid, beroende svarstider.
* [Servrar](app-insights-web-monitor-performance.md) -prestandaräknare. Tillgänglig om du [installera statusövervakaren](app-insights-monitor-performance-live-website-now.md).
* **Webbläsaren** -vy och AJAX-prestanda. Tillgänglig om du [instrumentera webbsidorna](app-insights-javascript.md).
* **Användning** -sidan Visa, användare och session räknas. Tillgänglig om du [instrumentera webbsidorna](app-insights-javascript.md).

KONFIGURERA

* **Komma igång** -infogade kursen.
* **Egenskaper för** -instrumentation nyckel, prenumeration och resurs-id.
* [Aviseringar](app-insights-alerts.md) -mått varningskonfigurationen.
* [Löpande export](app-insights-export-telemetry.md) -konfigurera export av telemetri tooAzure lagring.
* [Prestandatestning](app-insights-monitor-web-app-availability.md#performance-tests) -Ställ in en syntetisk belastningen på din webbplats.
* [Kvoten och prissättning](app-insights-pricing.md) och [införandet provtagning](app-insights-sampling.md).
* **API-åtkomst** -skapa [släpper anteckningar](app-insights-annotations.md) och för hello Data Access-API.
* [**Arbetsobjekt som** ](app-insights-diagnostic-search.md#create-work-item) -ansluta tooa arbete system för uppföljning så att du kan skapa buggar vid undersökning av telemetri.

INSTÄLLNINGAR

* [**Låser** ](../azure-resource-manager/resource-group-lock-resources.md) -låsa Azure-resurser
* [**Automatiseringsskriptet** ](app-insights-powershell.md) -exportera en definition av hello Azure-resurs så att du kan använda den som en mall toocreate nya resurser.


## <a name="video"></a>Video

> [!VIDEO https://channel9.msdn.com/events/Connect/2016/112/player]

## <a name="next-steps"></a>Nästa steg

|  |  |
| --- | --- |
| [Metrics explorer](app-insights-metrics-explorer.md)<br/>Filter och segment mått |![Sök-exempel](./media/app-insights-dashboards/64.png) |
| [Diagnostiska sökning](app-insights-diagnostic-search.md)<br/>Söka efter och granska händelser, relaterade händelser och skapa programfel |![Sök-exempel](./media/app-insights-dashboards/61.png) |
| [Analys](app-insights-analytics.md)<br/>Kraftfulla frågespråket |![Sök-exempel](./media/app-insights-dashboards/63.png) |
