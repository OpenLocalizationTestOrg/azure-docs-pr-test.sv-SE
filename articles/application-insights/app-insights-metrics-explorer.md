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
# <a name="exploring-metrics-in-application-insights"></a><span data-ttu-id="93620-103">Utforska mått i Application Insights</span><span class="sxs-lookup"><span data-stu-id="93620-103">Exploring Metrics in Application Insights</span></span>
<span data-ttu-id="93620-104">Mått i [Programinsikter] [ start] mätvärden och antalet händelser som skickas i telemetri från ditt program.</span><span class="sxs-lookup"><span data-stu-id="93620-104">Metrics in [Application Insights][start] are measured values and counts of events that are sent in telemetry from your application.</span></span> <span data-ttu-id="93620-105">De hjälper dig att identifiera prestandaproblem med och titta på trender i hur programmet används.</span><span class="sxs-lookup"><span data-stu-id="93620-105">They help you detect performance issues and watch trends in how your application is being used.</span></span> <span data-ttu-id="93620-106">Det finns en mängd olika standard mått och du kan också skapa egna anpassade mått och händelser.</span><span class="sxs-lookup"><span data-stu-id="93620-106">There's a wide range of standard metrics, and you can also create your own custom metrics and events.</span></span>

<span data-ttu-id="93620-107">Antal mått och händelse visas i diagram av mätvärden, till exempel summor, medelvärden eller antal.</span><span class="sxs-lookup"><span data-stu-id="93620-107">Metrics and event counts are displayed in charts of aggregated values such as sums, averages, or counts.</span></span>

<span data-ttu-id="93620-108">Här är ett exempel uppsättning diagram:</span><span class="sxs-lookup"><span data-stu-id="93620-108">Here's a sample set of charts:</span></span>

![](./media/app-insights-metrics-explorer/01-overview.png)

<span data-ttu-id="93620-109">Du hittar mått diagram överallt i hello Application Insights-portalen.</span><span class="sxs-lookup"><span data-stu-id="93620-109">You find metrics charts everywhere in hello Application Insights portal.</span></span> <span data-ttu-id="93620-110">I de flesta fall kan anpassas och du kan lägga till flera diagram toohello bladet.</span><span class="sxs-lookup"><span data-stu-id="93620-110">In most cases, they can be customized, and you can add more charts toohello blade.</span></span> <span data-ttu-id="93620-111">Hello översikt bladet klicka dig igenom toomore detaljerad diagram (som har titlar, till exempel ”servrar”), eller klicka på **Metrics Explorer** tooopen ett nytt blad där du kan skapa anpassade diagram.</span><span class="sxs-lookup"><span data-stu-id="93620-111">From hello Overview blade, click through toomore detailed charts (which have titles such as "Servers"), or click **Metrics Explorer** tooopen a new blade where you can create custom charts.</span></span>

## <a name="time-range"></a><span data-ttu-id="93620-112">Tidsintervall</span><span class="sxs-lookup"><span data-stu-id="93620-112">Time range</span></span>
<span data-ttu-id="93620-113">Du kan ändra hello tidsintervall som omfattas av hello diagram eller rutnät på ett blad.</span><span class="sxs-lookup"><span data-stu-id="93620-113">You can change hello Time range covered by hello charts or grids on any blade.</span></span>

![Öppna hello översikt bladet för ditt program i hello Azure-portalen](./media/app-insights-metrics-explorer/03-range.png)

<span data-ttu-id="93620-115">Klicka på Uppdatera om du väntar vissa data som inte fanns ännu.</span><span class="sxs-lookup"><span data-stu-id="93620-115">If you're expecting some data that hasn't appeared yet, click Refresh.</span></span> <span data-ttu-id="93620-116">Diagram uppdatera sig själva med intervall, men hello är längre med större tidsintervall.</span><span class="sxs-lookup"><span data-stu-id="93620-116">Charts refresh themselves at intervals, but hello intervals are longer for larger time ranges.</span></span> <span data-ttu-id="93620-117">Det kan ta en stund innan data toocome via hello analys pipeline till ett diagram.</span><span class="sxs-lookup"><span data-stu-id="93620-117">It can take a while for data toocome through hello analysis pipeline onto a chart.</span></span>

<span data-ttu-id="93620-118">att dra toozoom till en del av ett diagram över den:</span><span class="sxs-lookup"><span data-stu-id="93620-118">toozoom into part of a chart, drag over it:</span></span>

![Dra över en del av ett diagram.](./media/app-insights-metrics-explorer/12-drag.png)

<span data-ttu-id="93620-120">Klicka på hello Ångra Zooma-knappen toorestore den.</span><span class="sxs-lookup"><span data-stu-id="93620-120">Click hello Undo Zoom button toorestore it.</span></span>

## <a name="granularity-and-point-values"></a><span data-ttu-id="93620-121">Granularitet och punkt värden</span><span class="sxs-lookup"><span data-stu-id="93620-121">Granularity and point values</span></span>
<span data-ttu-id="93620-122">Placera pekaren över hello toodisplay hello värden i diagram av mätvärden, Hej då.</span><span class="sxs-lookup"><span data-stu-id="93620-122">Hover your mouse over hello chart toodisplay hello values of hello metrics at that point.</span></span>

![Hovra hello muspekaren över ett diagram](./media/app-insights-metrics-explorer/02-focus.png)

<span data-ttu-id="93620-124">hello-värdet för hello mått vid en viss slås samman under hello föregående insamlingsintervall.</span><span class="sxs-lookup"><span data-stu-id="93620-124">hello value of hello metric at a particular point is aggregated over hello preceding sampling interval.</span></span>

<span data-ttu-id="93620-125">hello insamlingsintervall eller ”granularitet” visas hello överst i hello-bladet.</span><span class="sxs-lookup"><span data-stu-id="93620-125">hello sampling interval or "granularity" is shown at hello top of hello blade.</span></span>

![hello-huvudet på ett blad.](./media/app-insights-metrics-explorer/11-grain.png)

<span data-ttu-id="93620-127">Du kan justera hello granularitet i hello tid intervallet bladet:</span><span class="sxs-lookup"><span data-stu-id="93620-127">You can adjust hello granularity in hello Time range blade:</span></span>

![hello-huvudet på ett blad.](./media/app-insights-metrics-explorer/grain.png)

<span data-ttu-id="93620-129">Hej granulariteter som är tillgängliga beror på hello tidsintervall du väljer.</span><span class="sxs-lookup"><span data-stu-id="93620-129">hello granularities available depend on hello time range you select.</span></span> <span data-ttu-id="93620-130">hello explicit granulariteter är alternativen toohello ”automatisk” granularitet för hello tidsintervallet.</span><span class="sxs-lookup"><span data-stu-id="93620-130">hello explicit granularities are alternatives toohello "automatic" granularity for hello time range.</span></span>


## <a name="editing-charts-and-grids"></a><span data-ttu-id="93620-131">Redigera diagram och rutnät</span><span class="sxs-lookup"><span data-stu-id="93620-131">Editing charts and grids</span></span>
<span data-ttu-id="93620-132">tooadd ett nytt diagram toohello blad:</span><span class="sxs-lookup"><span data-stu-id="93620-132">tooadd a new chart toohello blade:</span></span>

![Välj Lägg till diagram i Metrics Explorer](./media/app-insights-metrics-explorer/04-add.png)

<span data-ttu-id="93620-134">Välj **redigera** på en befintlig eller ny diagram tooedit vad visas:</span><span class="sxs-lookup"><span data-stu-id="93620-134">Select **Edit** on an existing or new chart tooedit what it shows:</span></span>

![Välj ett eller flera mått](./media/app-insights-metrics-explorer/08-select.png)

<span data-ttu-id="93620-136">Du kan visa fler än ett mått i ett diagram, även om det finns begränsningar om hello kombinationer som kan visas tillsammans.</span><span class="sxs-lookup"><span data-stu-id="93620-136">You can display more than one metric on a chart, though there are restrictions about hello combinations that can be displayed together.</span></span> <span data-ttu-id="93620-137">När du väljer en mått vissa hello som andra har inaktiverats.</span><span class="sxs-lookup"><span data-stu-id="93620-137">As soon as you choose one metric, some of hello others are disabled.</span></span>

<span data-ttu-id="93620-138">Om du har kodats [anpassade mått] [ track] i din app (anrop tooTrackMetric och TrackEvent) visas här.</span><span class="sxs-lookup"><span data-stu-id="93620-138">If you coded [custom metrics][track] into your app (calls tooTrackMetric and TrackEvent) they will be listed here.</span></span>

## <a name="segment-your-data"></a><span data-ttu-id="93620-139">Segmentera dina data</span><span class="sxs-lookup"><span data-stu-id="93620-139">Segment your data</span></span>
<span data-ttu-id="93620-140">Du kan dela ett mått med egenskapen – till exempel toocompare sidvyer på klienter med olika operativsystem.</span><span class="sxs-lookup"><span data-stu-id="93620-140">You can split a metric by property - for example, toocompare page views on clients with different operating systems.</span></span>

<span data-ttu-id="93620-141">Välj ett diagram eller rutnät, växla på gruppering och välj en egenskap toogroup av:</span><span class="sxs-lookup"><span data-stu-id="93620-141">Select a chart or grid, switch on grouping and pick a property toogroup by:</span></span>

![Välj gruppering på och ange en egenskap i Group By](./media/app-insights-metrics-explorer/15-segment.png)

> [!NOTE]
> <span data-ttu-id="93620-143">När du använder gruppering ange hello området och stapeldiagram typer en staplad visning.</span><span class="sxs-lookup"><span data-stu-id="93620-143">When you use grouping, hello Area and Bar chart types provide a stacked display.</span></span> <span data-ttu-id="93620-144">Detta är lämpligt där hello sammanställningsmetod summan.</span><span class="sxs-lookup"><span data-stu-id="93620-144">This is suitable where hello Aggregation method is Sum.</span></span> <span data-ttu-id="93620-145">Men om hello aggregeringstypen medelvärde, välja hello raden eller rutnät Visa typer.</span><span class="sxs-lookup"><span data-stu-id="93620-145">But where hello aggregation type is Average, choose hello Line or Grid display types.</span></span>
>
>

<span data-ttu-id="93620-146">Om du har kodats [anpassade mått] [ track] i din app och de innehåller egenskapsvärden, kommer du att kunna tooselect hello egenskap i hello-listan.</span><span class="sxs-lookup"><span data-stu-id="93620-146">If you coded [custom metrics][track] into your app and they include property values, you'll be able tooselect hello property in hello list.</span></span>

<span data-ttu-id="93620-147">Är hello diagram för liten för segmenterade data?</span><span class="sxs-lookup"><span data-stu-id="93620-147">Is hello chart too small for segmented data?</span></span> <span data-ttu-id="93620-148">Justera höjden:</span><span class="sxs-lookup"><span data-stu-id="93620-148">Adjust its height:</span></span>

![Justera hello skjutreglaget](./media/app-insights-metrics-explorer/18-height.png)

## <a name="aggregation-types"></a><span data-ttu-id="93620-150">Aggregeringen typer</span><span class="sxs-lookup"><span data-stu-id="93620-150">Aggregation types</span></span>
<span data-ttu-id="93620-151">hello förklaringen på hello-sida som standard visas vanligtvis hello samman värde under hello hello diagram.</span><span class="sxs-lookup"><span data-stu-id="93620-151">hello legend at hello side by default usually shows hello aggregated value over hello period of hello chart.</span></span> <span data-ttu-id="93620-152">Om du hovrar över hello diagram visar hello värdet vid den punkten.</span><span class="sxs-lookup"><span data-stu-id="93620-152">If you hover over hello chart, it shows hello value at that point.</span></span>

<span data-ttu-id="93620-153">Varje datapunkt i hello-schemat är en aggregering av hello datavärden i föregående provtagning intervall eller ”granularitet” Hej.</span><span class="sxs-lookup"><span data-stu-id="93620-153">Each data point on hello chart is an aggregate of hello data values received in hello preceding sampling interval or "granularity".</span></span> <span data-ttu-id="93620-154">hello granularitet visas hello överst på bladet hello och varierar beroende på hello övergripande tidsrymd av hello diagram.</span><span class="sxs-lookup"><span data-stu-id="93620-154">hello granularity is shown at hello top of hello blade, and varies with hello overall timescale of hello chart.</span></span>

<span data-ttu-id="93620-155">Mått kan sammanställas på olika sätt:</span><span class="sxs-lookup"><span data-stu-id="93620-155">Metrics can be aggregated in different ways:</span></span>

* <span data-ttu-id="93620-156">**Antal** antal hello händelser mottagna på hello insamlingsintervall.</span><span class="sxs-lookup"><span data-stu-id="93620-156">**Count** is a count of hello events received in hello sampling interval.</span></span> <span data-ttu-id="93620-157">Den används för händelser, till exempel begäranden.</span><span class="sxs-lookup"><span data-stu-id="93620-157">It is used for events such as requests.</span></span> <span data-ttu-id="93620-158">Förändringar i hello höjd hello diagram visar variationer i hello hastighet med vilken hello händelser inträffar.</span><span class="sxs-lookup"><span data-stu-id="93620-158">Variations in hello height of hello chart indicates variations in hello rate at which hello events occur.</span></span> <span data-ttu-id="93620-159">Men Observera att hello numeriskt värde ändras när du ändrar hello insamlingsintervall.</span><span class="sxs-lookup"><span data-stu-id="93620-159">But note that hello numeric value changes when you change hello sampling interval.</span></span>
* <span data-ttu-id="93620-160">**Summa** lägger till hello värden för alla hello datapunkter som tagits emot över hello insamlingsintervall eller hello tidsperiod hello diagram.</span><span class="sxs-lookup"><span data-stu-id="93620-160">**Sum** adds up hello values of all hello data points received over hello sampling interval, or hello period of hello chart.</span></span>
* <span data-ttu-id="93620-161">**Genomsnittlig** dividerar hello summan av hello antalet datapunkter som tagits emot över hello intervall.</span><span class="sxs-lookup"><span data-stu-id="93620-161">**Average** divides hello Sum by hello number of data points received over hello interval.</span></span>
* <span data-ttu-id="93620-162">**Unik** antal används för antal användare och konton.</span><span class="sxs-lookup"><span data-stu-id="93620-162">**Unique** counts are used for counts of users and accounts.</span></span> <span data-ttu-id="93620-163">Hello insamlingsintervall, eller under hello hello diagram visar hello bilden hello antal olika användare visas i den tiden.</span><span class="sxs-lookup"><span data-stu-id="93620-163">Over hello sampling interval, or over hello period of hello chart, hello figure shows hello count of different users seen in that time.</span></span>
* <span data-ttu-id="93620-164">**%**-procent versioner av varje aggregerings kan bara användas med segmenterade diagram.</span><span class="sxs-lookup"><span data-stu-id="93620-164">**%** - percentage versions of each aggregation are used only with segmented charts.</span></span> <span data-ttu-id="93620-165">hello totala läggs alltid in too100% och hello diagrammet visar hello relativa bidrag olika komponenterna i totalt.</span><span class="sxs-lookup"><span data-stu-id="93620-165">hello total always adds up too100%, and hello chart shows hello relative contribution of different components of a total.</span></span>

    ![Procentandel aggregering](./media/app-insights-metrics-explorer/percentage-aggregation.png)

### <a name="change-hello-aggregation-type"></a><span data-ttu-id="93620-167">Ändra hello sammansättningstyp</span><span class="sxs-lookup"><span data-stu-id="93620-167">Change hello aggregation type</span></span>

![Redigera hello diagrammet och välj sedan aggregering](./media/app-insights-metrics-explorer/05-aggregation.png)

<span data-ttu-id="93620-169">hello standardmetoden för varje mått visas när du skapar ett nytt schema eller när alla avmarkeras:</span><span class="sxs-lookup"><span data-stu-id="93620-169">hello default method for each metric is shown when you create a new chart or when all metrics are deselected:</span></span>

![Avmarkera alla mått toosee hello standardvärden](./media/app-insights-metrics-explorer/06-total.png)

## <a name="pin-y-axis"></a><span data-ttu-id="93620-171">PIN-kod y-axeln</span><span class="sxs-lookup"><span data-stu-id="93620-171">Pin Y-axis</span></span> 
<span data-ttu-id="93620-172">Som standard visar ett diagram Y-axeln värden med början från noll till maxvärdena i toogive en bild av quantum hello värden i dataområdet hello.</span><span class="sxs-lookup"><span data-stu-id="93620-172">By default a chart shows Y axis values starting from zero till maximum values in hello data range, toogive a visual representation of quantum of hello values.</span></span> <span data-ttu-id="93620-173">Men i vissa fall mer än hello quantum kan det vara intressant toovisually inspektera mindre ändringar i värden.</span><span class="sxs-lookup"><span data-stu-id="93620-173">But in some cases more than hello quantum it might be interesting toovisually inspect minor changes in values.</span></span> <span data-ttu-id="93620-174">För anpassningar som den här användningen hello y-axeln redigera funktionen toopin hello y-axeln lägsta eller högsta intervallvärdet på rätt plats.</span><span class="sxs-lookup"><span data-stu-id="93620-174">For customizations like this use hello Y-axis range editing feature toopin hello Y-axis minimum or maximum value at desired place.</span></span>
<span data-ttu-id="93620-175">Klicka på ”Avancerade inställningar” kryssrutan toobring in hello y-axeln intervallet inställningar</span><span class="sxs-lookup"><span data-stu-id="93620-175">Click on "Advanced Settings" check box toobring up hello Y-axis range Settings</span></span>

![Klicka på Avancerade inställningar, välja eget intervall och ange min maxvärde](./media/app-insights-metrics-explorer/y-axis-range.png)

## <a name="filter-your-data"></a><span data-ttu-id="93620-177">Filtrera dina data</span><span class="sxs-lookup"><span data-stu-id="93620-177">Filter your data</span></span>
<span data-ttu-id="93620-178">toosee bara hello mått för en vald uppsättning egenskapsvärden:</span><span class="sxs-lookup"><span data-stu-id="93620-178">toosee just hello metrics for a selected set of property values:</span></span>

![Klicka på filtret, expandera en egenskap och kontrollera vissa värden](./media/app-insights-metrics-explorer/19-filter.png)

<span data-ttu-id="93620-180">Om du inte markerar alla värden för en viss egenskap, den har hello detsamma som att markera dem: det finns inget filter på egenskapen.</span><span class="sxs-lookup"><span data-stu-id="93620-180">If you don't select any values for a particular property, it's hello same as selecting them all: there is no filter on that property.</span></span>

<span data-ttu-id="93620-181">Meddelande hello räknar händelser tillsammans med varje egenskapsvärde.</span><span class="sxs-lookup"><span data-stu-id="93620-181">Notice hello counts of events alongside each property value.</span></span> <span data-ttu-id="93620-182">När du väljer värden för en egenskap, räknar hello tillsammans med andra egenskapen värdena justeras.</span><span class="sxs-lookup"><span data-stu-id="93620-182">When you select values of one property, hello counts alongside other property values are adjusted.</span></span>

<span data-ttu-id="93620-183">Filter tillämpar tooall hello diagram på ett blad.</span><span class="sxs-lookup"><span data-stu-id="93620-183">Filters apply tooall hello charts on a blade.</span></span> <span data-ttu-id="93620-184">Om du vill att olika filter toodifferent diagram, skapa och spara olika mått blad.</span><span class="sxs-lookup"><span data-stu-id="93620-184">If you want different filters applied toodifferent charts, create and save different metrics blades.</span></span> <span data-ttu-id="93620-185">Om du vill kan kan du fästa diagram från olika blad toohello instrumentpanelen så att du kan se dem tillsammans med varandra.</span><span class="sxs-lookup"><span data-stu-id="93620-185">If you want, you can pin charts from different blades toohello dashboard, so that you can see them alongside each other.</span></span>

### <a name="remove-bot-and-web-test-traffic"></a><span data-ttu-id="93620-186">Ta bort bot och web test-trafik</span><span class="sxs-lookup"><span data-stu-id="93620-186">Remove bot and web test traffic</span></span>
<span data-ttu-id="93620-187">Använd hello filter **verklig eller syntetisk trafik** och kontrollera **verkliga**.</span><span class="sxs-lookup"><span data-stu-id="93620-187">Use hello filter **Real or synthetic traffic** and check **Real**.</span></span>

<span data-ttu-id="93620-188">Du kan också filtrera efter **källa för syntetisk trafik**.</span><span class="sxs-lookup"><span data-stu-id="93620-188">You can also filter by **Source of synthetic traffic**.</span></span>

### <a name="tooadd-properties-toohello-filter-list"></a><span data-ttu-id="93620-189">tooadd egenskaper toohello-filterlista</span><span class="sxs-lookup"><span data-stu-id="93620-189">tooadd properties toohello filter list</span></span>
<span data-ttu-id="93620-190">Vill du toofilter telemetri för en kategori av egna välja?</span><span class="sxs-lookup"><span data-stu-id="93620-190">Would you like toofilter telemetry on a category of your own choosing?</span></span> <span data-ttu-id="93620-191">Exempelvis kanske du dela upp användarna i olika kategorier och du vill att segmentera dina data med dessa kategorier.</span><span class="sxs-lookup"><span data-stu-id="93620-191">For example, maybe you divide up your users into different categories, and you would like segment your data by these categories.</span></span>

<span data-ttu-id="93620-192">[Skapa en egen egenskap](app-insights-api-custom-events-metrics.md#properties).</span><span class="sxs-lookup"><span data-stu-id="93620-192">[Create your own property](app-insights-api-custom-events-metrics.md#properties).</span></span> <span data-ttu-id="93620-193">Ange den i en [telemetri initieraren](app-insights-api-custom-events-metrics.md#defaults) toohave som det visas i alla telemetri - inklusive hello standard telemetri som skickas av olika SDK-moduler.</span><span class="sxs-lookup"><span data-stu-id="93620-193">Set it in a [Telemetry Initializer](app-insights-api-custom-events-metrics.md#defaults) toohave it appear in all telemetry - including hello standard telemetry sent by different SDK modules.</span></span>

## <a name="edit-hello-chart-type"></a><span data-ttu-id="93620-194">Redigera hello-diagram</span><span class="sxs-lookup"><span data-stu-id="93620-194">Edit hello chart type</span></span>
<span data-ttu-id="93620-195">Observera att du kan växla mellan rutnät och diagram:</span><span class="sxs-lookup"><span data-stu-id="93620-195">Notice that you can switch between grids and graphs:</span></span>

![Markera ett rutnät eller diagram och välj en diagramtyp](./media/app-insights-metrics-explorer/16-chart-grid.png)

## <a name="save-your-metrics-blade"></a><span data-ttu-id="93620-197">Spara mått-bladet</span><span class="sxs-lookup"><span data-stu-id="93620-197">Save your metrics blade</span></span>
<span data-ttu-id="93620-198">När du har skapat vissa diagram kan du spara dem som en favorit.</span><span class="sxs-lookup"><span data-stu-id="93620-198">When you've created some charts, save them as a favorite.</span></span> <span data-ttu-id="93620-199">Du kan välja om tooshare den med andra teammedlemmar, om du använder ett organisationskonto.</span><span class="sxs-lookup"><span data-stu-id="93620-199">You can choose whether tooshare it with other team members, if you use an organizational account.</span></span>

![Välj favorit](./media/app-insights-metrics-explorer/21-favorite-save.png)

<span data-ttu-id="93620-201">toosee hello bladet igen, **gå toohello översikt bladet** och öppna Favoriter:</span><span class="sxs-lookup"><span data-stu-id="93620-201">toosee hello blade again, **go toohello overview blade** and open Favorites:</span></span>

![Hello översikt bladet välj Favoriter](./media/app-insights-metrics-explorer/22-favorite-get.png)

<span data-ttu-id="93620-203">Om du väljer relativt tidsintervall när du har sparat uppdateras hello bladet med senaste hello.</span><span class="sxs-lookup"><span data-stu-id="93620-203">If you chose Relative time range when you saved, hello blade will be updated with hello latest metrics.</span></span> <span data-ttu-id="93620-204">Om du har valt absolut tidsintervall visas hello samma data varje gång.</span><span class="sxs-lookup"><span data-stu-id="93620-204">If you chose Absolute time range, it will show hello same data every time.</span></span>

## <a name="reset-hello-blade"></a><span data-ttu-id="93620-205">Återställ hello-bladet</span><span class="sxs-lookup"><span data-stu-id="93620-205">Reset hello blade</span></span>
<span data-ttu-id="93620-206">Om du redigerar ett blad men sedan som tooget tillbaka toohello ursprungliga spara uppsättning, klickar du på Återställ.</span><span class="sxs-lookup"><span data-stu-id="93620-206">If you edit a blade but then you'd like tooget back toohello original saved set, just click Reset.</span></span>

![I hello knappar hello överst i måttet Explorer](./media/app-insights-metrics-explorer/17-reset.png)

## <a name="live-metrics-stream"></a><span data-ttu-id="93620-208">Live mått dataström</span><span class="sxs-lookup"><span data-stu-id="93620-208">Live metrics stream</span></span>

<span data-ttu-id="93620-209">En mycket mer omedelbar vy över din telemetri öppna [Liveströmning](app-insights-live-stream.md).</span><span class="sxs-lookup"><span data-stu-id="93620-209">For a much more immediate view of your telemetry, open [Live Stream](app-insights-live-stream.md).</span></span> <span data-ttu-id="93620-210">De flesta mått ta några minuter tooappear på grund av hello processen för aggregering.</span><span class="sxs-lookup"><span data-stu-id="93620-210">Most metrics take a few minutes tooappear, because of hello process of aggregation.</span></span> <span data-ttu-id="93620-211">Däremot är live mått optimerade för låg latens.</span><span class="sxs-lookup"><span data-stu-id="93620-211">By contrast, live metrics are optimized for low latency.</span></span> 

## <a name="set-alerts"></a><span data-ttu-id="93620-212">Ange aviseringar</span><span class="sxs-lookup"><span data-stu-id="93620-212">Set alerts</span></span>
<span data-ttu-id="93620-213">toobe meddelas via e-post med ovanliga värden för alla mått, lägga till en avisering.</span><span class="sxs-lookup"><span data-stu-id="93620-213">toobe notified by email of unusual values of any metric, add an alert.</span></span> <span data-ttu-id="93620-214">Du kan välja toosend hello e-toohello kontoadministratörer eller toospecific e-postadresser.</span><span class="sxs-lookup"><span data-stu-id="93620-214">You can choose either toosend hello email toohello account administrators, or toospecific email addresses.</span></span>

![Välj Varningsregler, Lägg till avisering i Metrics Explorer](./media/app-insights-metrics-explorer/appinsights-413setMetricAlert.png)

<span data-ttu-id="93620-216">[Mer information om aviseringar][alerts].</span><span class="sxs-lookup"><span data-stu-id="93620-216">[Learn more about alerts][alerts].</span></span>


## <a name="continuous-export"></a><span data-ttu-id="93620-217">Kontinuerlig export</span><span class="sxs-lookup"><span data-stu-id="93620-217">Continuous Export</span></span>
<span data-ttu-id="93620-218">Om du vill att data exporterats kontinuerligt så att du kan bearbeta externt, bör du använda [löpande export](app-insights-export-telemetry.md).</span><span class="sxs-lookup"><span data-stu-id="93620-218">If you want data continuously exported so that you can process it externally, consider using [Continuous export](app-insights-export-telemetry.md).</span></span>

### <a name="power-bi"></a><span data-ttu-id="93620-219">Power BI</span><span class="sxs-lookup"><span data-stu-id="93620-219">Power BI</span></span>
<span data-ttu-id="93620-220">Om du vill att ännu bättre vyer för dina data, kan du [exportera tooPower BI](http://blogs.msdn.com/b/powerbi/archive/2015/11/04/explore-your-application-insights-data-with-power-bi.aspx).</span><span class="sxs-lookup"><span data-stu-id="93620-220">If you want even richer views of your data, you can [export tooPower BI](http://blogs.msdn.com/b/powerbi/archive/2015/11/04/explore-your-application-insights-data-with-power-bi.aspx).</span></span>

## <a name="analytics"></a><span data-ttu-id="93620-221">Analys</span><span class="sxs-lookup"><span data-stu-id="93620-221">Analytics</span></span>
<span data-ttu-id="93620-222">[Analytics](app-insights-analytics.md) är ett mer flexibelt sätt tooanalyze din telemetri med hjälp av ett kraftfullt frågespråk.</span><span class="sxs-lookup"><span data-stu-id="93620-222">[Analytics](app-insights-analytics.md) is a more versatile way tooanalyze your telemetry using a powerful query language.</span></span> <span data-ttu-id="93620-223">Använd det om du vill toocombine compute resultat från mått eller utföra en ingående undersökning av din app senaste prestanda.</span><span class="sxs-lookup"><span data-stu-id="93620-223">Use it if you want toocombine or compute results from metrics, or perform an in-depth exploration of your app's recent performance.</span></span> 

<span data-ttu-id="93620-224">Från ett mått diagram, kan du klicka på hello Analytics ikonen tooget direkt toohello motsvarande Analytics-fråga.</span><span class="sxs-lookup"><span data-stu-id="93620-224">From a metric chart, you can click hello Analytics icon tooget directly toohello equivalent Analytics query.</span></span>

## <a name="troubleshooting"></a><span data-ttu-id="93620-225">Felsökning</span><span class="sxs-lookup"><span data-stu-id="93620-225">Troubleshooting</span></span>
<span data-ttu-id="93620-226">*Alla data visas inte i diagrammet.*</span><span class="sxs-lookup"><span data-stu-id="93620-226">*I don't see any data on my chart.*</span></span>

* <span data-ttu-id="93620-227">Filter tillämpar tooall hello diagram på hello-bladet.</span><span class="sxs-lookup"><span data-stu-id="93620-227">Filters apply tooall hello charts on hello blade.</span></span> <span data-ttu-id="93620-228">Kontrollera att du inte definiera ett filter som utesluter alla hello data på en annan när du fokusera på ett diagram.</span><span class="sxs-lookup"><span data-stu-id="93620-228">Make sure that, while you're focusing on one chart, you didn't set a filter that excludes all hello data on another.</span></span>

    <span data-ttu-id="93620-229">Om du vill tooset olika filter på olika diagram, skapa dem i olika blad, spara dem som separata Favoriter.</span><span class="sxs-lookup"><span data-stu-id="93620-229">If you want tooset different filters on different charts, create them in different blades, save them as separate favorites.</span></span> <span data-ttu-id="93620-230">Om du vill kan fästa du dem toohello instrumentpanelen så att du kan se dem tillsammans med varandra.</span><span class="sxs-lookup"><span data-stu-id="93620-230">If you want, you can pin them toohello dashboard so that you can see them alongside each other.</span></span>
* <span data-ttu-id="93620-231">Om du grupperar ett diagram av en egenskap som inte har definierats för hello mått, att kommer det finnas ingenting i hello diagram.</span><span class="sxs-lookup"><span data-stu-id="93620-231">If you group a chart by a property that is not defined on hello metric, then there will be nothing on hello chart.</span></span> <span data-ttu-id="93620-232">Prova att rensa 'group by- eller välj en annan gruppering-egenskap.</span><span class="sxs-lookup"><span data-stu-id="93620-232">Try clearing 'group by', or choose a different grouping property.</span></span>
* <span data-ttu-id="93620-233">Prestandadata (CPU, IO-frekvens och så vidare) är tillgängligt för Java-webbtjänster, Windows-skrivbordsappar [av IIS-program och tjänster om du installerar statusövervakaren](app-insights-monitor-performance-live-website-now.md), och [Azure Cloud Services](app-insights-azure.md).</span><span class="sxs-lookup"><span data-stu-id="93620-233">Performance data (CPU, IO rate, and so on) is available for Java web services, Windows desktop apps, [IIS web apps and services if you install status monitor](app-insights-monitor-performance-live-website-now.md), and [Azure Cloud Services](app-insights-azure.md).</span></span> <span data-ttu-id="93620-234">Det är inte tillgänglig för Azure websites.</span><span class="sxs-lookup"><span data-stu-id="93620-234">It isn't available for Azure websites.</span></span>

## <a name="video"></a><span data-ttu-id="93620-235">Video</span><span class="sxs-lookup"><span data-stu-id="93620-235">Video</span></span>

> [!VIDEO https://channel9.msdn.com/events/Connect/2016/112/player]

## <a name="next-steps"></a><span data-ttu-id="93620-236">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="93620-236">Next steps</span></span>
* [<span data-ttu-id="93620-237">Övervakning med Application Insights</span><span class="sxs-lookup"><span data-stu-id="93620-237">Monitoring usage with Application Insights</span></span>](app-insights-web-track-usage.md)
* [<span data-ttu-id="93620-238">Diagnostiska sökning</span><span class="sxs-lookup"><span data-stu-id="93620-238">Using Diagnostic Search</span></span>](app-insights-diagnostic-search.md)

<!--Link references-->

[alerts]: app-insights-alerts.md
[start]: app-insights-overview.md
[track]: app-insights-api-custom-events-metrics.md
