---
title: "aaaExploring mått i Azure Application Insights | Microsoft Docs"
description: "Hur toointerpret diagram på mått explorer och hur toocustomize mått explorer blad."
services: application-insights
documentationcenter: 
author: CFreemanwa
manager: carmonm
ms.assetid: 1f471176-38f3-40b3-bc6d-3f47d0cbaaa2
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 03/08/2017
ms.author: bwren
ms.openlocfilehash: b77ae227ae61e800ad6f3af8a05cd123ea1d69e1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="exploring-metrics-in-application-insights"></a>Utforska mått i Application Insights
Mått i [Programinsikter] [ start] mätvärden och antalet händelser som skickas i telemetri från ditt program. De hjälper dig att identifiera prestandaproblem med och titta på trender i hur programmet används. Det finns en mängd olika standard mått och du kan också skapa egna anpassade mått och händelser.

Antal mått och händelse visas i diagram av mätvärden, till exempel summor, medelvärden eller antal.

Här är ett exempel uppsättning diagram:

![](./media/app-insights-metrics-explorer/01-overview.png)

Du hittar mått diagram överallt i hello Application Insights-portalen. I de flesta fall kan anpassas och du kan lägga till flera diagram toohello bladet. Hello översikt bladet klicka dig igenom toomore detaljerad diagram (som har titlar, till exempel ”servrar”), eller klicka på **Metrics Explorer** tooopen ett nytt blad där du kan skapa anpassade diagram.

## <a name="time-range"></a>Tidsintervall
Du kan ändra hello tidsintervall som omfattas av hello diagram eller rutnät på ett blad.

![Öppna hello översikt bladet för ditt program i hello Azure-portalen](./media/app-insights-metrics-explorer/03-range.png)

Klicka på Uppdatera om du väntar vissa data som inte fanns ännu. Diagram uppdatera sig själva med intervall, men hello är längre med större tidsintervall. Det kan ta en stund innan data toocome via hello analys pipeline till ett diagram.

att dra toozoom till en del av ett diagram över den:

![Dra över en del av ett diagram.](./media/app-insights-metrics-explorer/12-drag.png)

Klicka på hello Ångra Zooma-knappen toorestore den.

## <a name="granularity-and-point-values"></a>Granularitet och punkt värden
Placera pekaren över hello toodisplay hello värden i diagram av mätvärden, Hej då.

![Hovra hello muspekaren över ett diagram](./media/app-insights-metrics-explorer/02-focus.png)

hello-värdet för hello mått vid en viss slås samman under hello föregående insamlingsintervall.

hello insamlingsintervall eller ”granularitet” visas hello överst i hello-bladet.

![hello-huvudet på ett blad.](./media/app-insights-metrics-explorer/11-grain.png)

Du kan justera hello granularitet i hello tid intervallet bladet:

![hello-huvudet på ett blad.](./media/app-insights-metrics-explorer/grain.png)

Hej granulariteter som är tillgängliga beror på hello tidsintervall du väljer. hello explicit granulariteter är alternativen toohello ”automatisk” granularitet för hello tidsintervallet.


## <a name="editing-charts-and-grids"></a>Redigera diagram och rutnät
tooadd ett nytt diagram toohello blad:

![Välj Lägg till diagram i Metrics Explorer](./media/app-insights-metrics-explorer/04-add.png)

Välj **redigera** på en befintlig eller ny diagram tooedit vad visas:

![Välj ett eller flera mått](./media/app-insights-metrics-explorer/08-select.png)

Du kan visa fler än ett mått i ett diagram, även om det finns begränsningar om hello kombinationer som kan visas tillsammans. När du väljer en mått vissa hello som andra har inaktiverats.

Om du har kodats [anpassade mått] [ track] i din app (anrop tooTrackMetric och TrackEvent) visas här.

## <a name="segment-your-data"></a>Segmentera dina data
Du kan dela ett mått med egenskapen – till exempel toocompare sidvyer på klienter med olika operativsystem.

Välj ett diagram eller rutnät, växla på gruppering och välj en egenskap toogroup av:

![Välj gruppering på och ange en egenskap i Group By](./media/app-insights-metrics-explorer/15-segment.png)

> [!NOTE]
> När du använder gruppering ange hello området och stapeldiagram typer en staplad visning. Detta är lämpligt där hello sammanställningsmetod summan. Men om hello aggregeringstypen medelvärde, välja hello raden eller rutnät Visa typer.
>
>

Om du har kodats [anpassade mått] [ track] i din app och de innehåller egenskapsvärden, kommer du att kunna tooselect hello egenskap i hello-listan.

Är hello diagram för liten för segmenterade data? Justera höjden:

![Justera hello skjutreglaget](./media/app-insights-metrics-explorer/18-height.png)

## <a name="aggregation-types"></a>Aggregeringen typer
hello förklaringen på hello-sida som standard visas vanligtvis hello samman värde under hello hello diagram. Om du hovrar över hello diagram visar hello värdet vid den punkten.

Varje datapunkt i hello-schemat är en aggregering av hello datavärden i föregående provtagning intervall eller ”granularitet” Hej. hello granularitet visas hello överst på bladet hello och varierar beroende på hello övergripande tidsrymd av hello diagram.

Mått kan sammanställas på olika sätt:

* **Antal** antal hello händelser mottagna på hello insamlingsintervall. Den används för händelser, till exempel begäranden. Förändringar i hello höjd hello diagram visar variationer i hello hastighet med vilken hello händelser inträffar. Men Observera att hello numeriskt värde ändras när du ändrar hello insamlingsintervall.
* **Summa** lägger till hello värden för alla hello datapunkter som tagits emot över hello insamlingsintervall eller hello tidsperiod hello diagram.
* **Genomsnittlig** dividerar hello summan av hello antalet datapunkter som tagits emot över hello intervall.
* **Unik** antal används för antal användare och konton. Hello insamlingsintervall, eller under hello hello diagram visar hello bilden hello antal olika användare visas i den tiden.
* **%**-procent versioner av varje aggregerings kan bara användas med segmenterade diagram. hello totala läggs alltid in too100% och hello diagrammet visar hello relativa bidrag olika komponenterna i totalt.

    ![Procentandel aggregering](./media/app-insights-metrics-explorer/percentage-aggregation.png)

### <a name="change-hello-aggregation-type"></a>Ändra hello sammansättningstyp

![Redigera hello diagrammet och välj sedan aggregering](./media/app-insights-metrics-explorer/05-aggregation.png)

hello standardmetoden för varje mått visas när du skapar ett nytt schema eller när alla avmarkeras:

![Avmarkera alla mått toosee hello standardvärden](./media/app-insights-metrics-explorer/06-total.png)

## <a name="pin-y-axis"></a>PIN-kod y-axeln 
Som standard visar ett diagram Y-axeln värden med början från noll till maxvärdena i toogive en bild av quantum hello värden i dataområdet hello. Men i vissa fall mer än hello quantum kan det vara intressant toovisually inspektera mindre ändringar i värden. För anpassningar som den här användningen hello y-axeln redigera funktionen toopin hello y-axeln lägsta eller högsta intervallvärdet på rätt plats.
Klicka på ”Avancerade inställningar” kryssrutan toobring in hello y-axeln intervallet inställningar

![Klicka på Avancerade inställningar, välja eget intervall och ange min maxvärde](./media/app-insights-metrics-explorer/y-axis-range.png)

## <a name="filter-your-data"></a>Filtrera dina data
toosee bara hello mått för en vald uppsättning egenskapsvärden:

![Klicka på filtret, expandera en egenskap och kontrollera vissa värden](./media/app-insights-metrics-explorer/19-filter.png)

Om du inte markerar alla värden för en viss egenskap, den har hello detsamma som att markera dem: det finns inget filter på egenskapen.

Meddelande hello räknar händelser tillsammans med varje egenskapsvärde. När du väljer värden för en egenskap, räknar hello tillsammans med andra egenskapen värdena justeras.

Filter tillämpar tooall hello diagram på ett blad. Om du vill att olika filter toodifferent diagram, skapa och spara olika mått blad. Om du vill kan kan du fästa diagram från olika blad toohello instrumentpanelen så att du kan se dem tillsammans med varandra.

### <a name="remove-bot-and-web-test-traffic"></a>Ta bort bot och web test-trafik
Använd hello filter **verklig eller syntetisk trafik** och kontrollera **verkliga**.

Du kan också filtrera efter **källa för syntetisk trafik**.

### <a name="tooadd-properties-toohello-filter-list"></a>tooadd egenskaper toohello-filterlista
Vill du toofilter telemetri för en kategori av egna välja? Exempelvis kanske du dela upp användarna i olika kategorier och du vill att segmentera dina data med dessa kategorier.

[Skapa en egen egenskap](app-insights-api-custom-events-metrics.md#properties). Ange den i en [telemetri initieraren](app-insights-api-custom-events-metrics.md#defaults) toohave som det visas i alla telemetri - inklusive hello standard telemetri som skickas av olika SDK-moduler.

## <a name="edit-hello-chart-type"></a>Redigera hello-diagram
Observera att du kan växla mellan rutnät och diagram:

![Markera ett rutnät eller diagram och välj en diagramtyp](./media/app-insights-metrics-explorer/16-chart-grid.png)

## <a name="save-your-metrics-blade"></a>Spara mått-bladet
När du har skapat vissa diagram kan du spara dem som en favorit. Du kan välja om tooshare den med andra teammedlemmar, om du använder ett organisationskonto.

![Välj favorit](./media/app-insights-metrics-explorer/21-favorite-save.png)

toosee hello bladet igen, **gå toohello översikt bladet** och öppna Favoriter:

![Hello översikt bladet välj Favoriter](./media/app-insights-metrics-explorer/22-favorite-get.png)

Om du väljer relativt tidsintervall när du har sparat uppdateras hello bladet med senaste hello. Om du har valt absolut tidsintervall visas hello samma data varje gång.

## <a name="reset-hello-blade"></a>Återställ hello-bladet
Om du redigerar ett blad men sedan som tooget tillbaka toohello ursprungliga spara uppsättning, klickar du på Återställ.

![I hello knappar hello överst i måttet Explorer](./media/app-insights-metrics-explorer/17-reset.png)

## <a name="live-metrics-stream"></a>Live mått dataström

En mycket mer omedelbar vy över din telemetri öppna [Liveströmning](app-insights-live-stream.md). De flesta mått ta några minuter tooappear på grund av hello processen för aggregering. Däremot är live mått optimerade för låg latens. 

## <a name="set-alerts"></a>Ange aviseringar
toobe meddelas via e-post med ovanliga värden för alla mått, lägga till en avisering. Du kan välja toosend hello e-toohello kontoadministratörer eller toospecific e-postadresser.

![Välj Varningsregler, Lägg till avisering i Metrics Explorer](./media/app-insights-metrics-explorer/appinsights-413setMetricAlert.png)

[Mer information om aviseringar][alerts].


## <a name="continuous-export"></a>Kontinuerlig export
Om du vill att data exporterats kontinuerligt så att du kan bearbeta externt, bör du använda [löpande export](app-insights-export-telemetry.md).

### <a name="power-bi"></a>Power BI
Om du vill att ännu bättre vyer för dina data, kan du [exportera tooPower BI](http://blogs.msdn.com/b/powerbi/archive/2015/11/04/explore-your-application-insights-data-with-power-bi.aspx).

## <a name="analytics"></a>Analys
[Analytics](app-insights-analytics.md) är ett mer flexibelt sätt tooanalyze din telemetri med hjälp av ett kraftfullt frågespråk. Använd det om du vill toocombine compute resultat från mått eller utföra en ingående undersökning av din app senaste prestanda. 

Från ett mått diagram, kan du klicka på hello Analytics ikonen tooget direkt toohello motsvarande Analytics-fråga.

## <a name="troubleshooting"></a>Felsökning
*Alla data visas inte i diagrammet.*

* Filter tillämpar tooall hello diagram på hello-bladet. Kontrollera att du inte definiera ett filter som utesluter alla hello data på en annan när du fokusera på ett diagram.

    Om du vill tooset olika filter på olika diagram, skapa dem i olika blad, spara dem som separata Favoriter. Om du vill kan fästa du dem toohello instrumentpanelen så att du kan se dem tillsammans med varandra.
* Om du grupperar ett diagram av en egenskap som inte har definierats för hello mått, att kommer det finnas ingenting i hello diagram. Prova att rensa 'group by- eller välj en annan gruppering-egenskap.
* Prestandadata (CPU, IO-frekvens och så vidare) är tillgängligt för Java-webbtjänster, Windows-skrivbordsappar [av IIS-program och tjänster om du installerar statusövervakaren](app-insights-monitor-performance-live-website-now.md), och [Azure Cloud Services](app-insights-azure.md). Det är inte tillgänglig för Azure websites.

## <a name="video"></a>Video

> [!VIDEO https://channel9.msdn.com/events/Connect/2016/112/player]

## <a name="next-steps"></a>Nästa steg
* [Övervakning med Application Insights](app-insights-web-track-usage.md)
* [Diagnostiska sökning](app-insights-diagnostic-search.md)

<!--Link references-->

[alerts]: app-insights-alerts.md
[start]: app-insights-overview.md
[track]: app-insights-api-custom-events-metrics.md
