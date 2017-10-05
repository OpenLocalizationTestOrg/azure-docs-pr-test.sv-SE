---
title: "Utforska mätvärden i Azure Application Insights | Microsoft Docs"
description: "Så här tolkar diagram på mått explorer och hur du anpassar mått explorer blad."
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
ms.openlocfilehash: a13500284caab79bbe221060ccf3d925ffb1fab5
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/18/2017
---
# <a name="exploring-metrics-in-application-insights"></a><span data-ttu-id="ddd14-103">Utforska mått i Application Insights</span><span class="sxs-lookup"><span data-stu-id="ddd14-103">Exploring Metrics in Application Insights</span></span>
<span data-ttu-id="ddd14-104">Mått i [Programinsikter] [ start] mätvärden och antalet händelser som skickas i telemetri från ditt program.</span><span class="sxs-lookup"><span data-stu-id="ddd14-104">Metrics in [Application Insights][start] are measured values and counts of events that are sent in telemetry from your application.</span></span> <span data-ttu-id="ddd14-105">De hjälper dig att identifiera prestandaproblem med och titta på trender i hur programmet används.</span><span class="sxs-lookup"><span data-stu-id="ddd14-105">They help you detect performance issues and watch trends in how your application is being used.</span></span> <span data-ttu-id="ddd14-106">Det finns en mängd olika standard mått och du kan också skapa egna anpassade mått och händelser.</span><span class="sxs-lookup"><span data-stu-id="ddd14-106">There's a wide range of standard metrics, and you can also create your own custom metrics and events.</span></span>

<span data-ttu-id="ddd14-107">Antal mått och händelse visas i diagram av mätvärden, till exempel summor, medelvärden eller antal.</span><span class="sxs-lookup"><span data-stu-id="ddd14-107">Metrics and event counts are displayed in charts of aggregated values such as sums, averages, or counts.</span></span>

<span data-ttu-id="ddd14-108">Här är ett exempel uppsättning diagram:</span><span class="sxs-lookup"><span data-stu-id="ddd14-108">Here's a sample set of charts:</span></span>

![](./media/app-insights-metrics-explorer/01-overview.png)

<span data-ttu-id="ddd14-109">Du hittar mått diagram överallt i Application Insights-portalen.</span><span class="sxs-lookup"><span data-stu-id="ddd14-109">You find metrics charts everywhere in the Application Insights portal.</span></span> <span data-ttu-id="ddd14-110">I de flesta fall kan anpassas och du kan lägga till fler diagram bladet.</span><span class="sxs-lookup"><span data-stu-id="ddd14-110">In most cases, they can be customized, and you can add more charts to the blade.</span></span> <span data-ttu-id="ddd14-111">I bladet översikt Klicka vidare till mer detaljerad diagram (som har titlar, till exempel ”servrar”), eller klicka på **Metrics Explorer** att öppna ett nytt blad där du kan skapa anpassade diagram.</span><span class="sxs-lookup"><span data-stu-id="ddd14-111">From the Overview blade, click through to more detailed charts (which have titles such as "Servers"), or click **Metrics Explorer** to open a new blade where you can create custom charts.</span></span>

## <a name="time-range"></a><span data-ttu-id="ddd14-112">Tidsintervall</span><span class="sxs-lookup"><span data-stu-id="ddd14-112">Time range</span></span>
<span data-ttu-id="ddd14-113">Du kan ändra det tidsintervall som omfattas av diagram eller rutnät på ett blad.</span><span class="sxs-lookup"><span data-stu-id="ddd14-113">You can change the Time range covered by the charts or grids on any blade.</span></span>

![Öppna bladet översikt av ditt program i Azure-portalen](./media/app-insights-metrics-explorer/03-range.png)

<span data-ttu-id="ddd14-115">Klicka på Uppdatera om du väntar vissa data som inte fanns ännu.</span><span class="sxs-lookup"><span data-stu-id="ddd14-115">If you're expecting some data that hasn't appeared yet, click Refresh.</span></span> <span data-ttu-id="ddd14-116">Diagram uppdatera sig själva med intervall, men intervall är längre för större tidsintervall.</span><span class="sxs-lookup"><span data-stu-id="ddd14-116">Charts refresh themselves at intervals, but the intervals are longer for larger time ranges.</span></span> <span data-ttu-id="ddd14-117">Det kan ta en stund innan data att komma via analyser pipeline till ett diagram.</span><span class="sxs-lookup"><span data-stu-id="ddd14-117">It can take a while for data to come through the analysis pipeline onto a chart.</span></span>

<span data-ttu-id="ddd14-118">Dra över den för att zooma in en del av ett diagram:</span><span class="sxs-lookup"><span data-stu-id="ddd14-118">To zoom into part of a chart, drag over it:</span></span>

![Dra över en del av ett diagram.](./media/app-insights-metrics-explorer/12-drag.png)

<span data-ttu-id="ddd14-120">Klicka på knappen Ångra Zooma om du vill återställa den.</span><span class="sxs-lookup"><span data-stu-id="ddd14-120">Click the Undo Zoom button to restore it.</span></span>

## <a name="granularity-and-point-values"></a><span data-ttu-id="ddd14-121">Granularitet och punkt värden</span><span class="sxs-lookup"><span data-stu-id="ddd14-121">Granularity and point values</span></span>
<span data-ttu-id="ddd14-122">Placera pekaren över diagrammet för att visa värdena i mätvärdena som vid den punkten.</span><span class="sxs-lookup"><span data-stu-id="ddd14-122">Hover your mouse over the chart to display the values of the metrics at that point.</span></span>

![Placera markören över ett diagram](./media/app-insights-metrics-explorer/02-focus.png)

<span data-ttu-id="ddd14-124">Värdet för måttet vid en viss slås samman under föregående exempelintervallet.</span><span class="sxs-lookup"><span data-stu-id="ddd14-124">The value of the metric at a particular point is aggregated over the preceding sampling interval.</span></span>

<span data-ttu-id="ddd14-125">Exempelintervall eller ”granularitet” visas längst upp på bladet.</span><span class="sxs-lookup"><span data-stu-id="ddd14-125">The sampling interval or "granularity" is shown at the top of the blade.</span></span>

![Rubriken för ett blad.](./media/app-insights-metrics-explorer/11-grain.png)

<span data-ttu-id="ddd14-127">Du kan justera granularitet i bladet tid intervall:</span><span class="sxs-lookup"><span data-stu-id="ddd14-127">You can adjust the granularity in the Time range blade:</span></span>

![Rubriken för ett blad.](./media/app-insights-metrics-explorer/grain.png)

<span data-ttu-id="ddd14-129">De tillgängliga kornigheterna beror på vilket tidsintervall du väljer.</span><span class="sxs-lookup"><span data-stu-id="ddd14-129">The granularities available depend on the time range you select.</span></span> <span data-ttu-id="ddd14-130">Explicit granulariteter är alternativ till ”automatisk” Granulariteten för tidsintervallet.</span><span class="sxs-lookup"><span data-stu-id="ddd14-130">The explicit granularities are alternatives to the "automatic" granularity for the time range.</span></span>


## <a name="editing-charts-and-grids"></a><span data-ttu-id="ddd14-131">Redigera diagram och rutnät</span><span class="sxs-lookup"><span data-stu-id="ddd14-131">Editing charts and grids</span></span>
<span data-ttu-id="ddd14-132">Lägga till ett nytt diagram i bladet:</span><span class="sxs-lookup"><span data-stu-id="ddd14-132">To add a new chart to the blade:</span></span>

![Välj Lägg till diagram i Metrics Explorer](./media/app-insights-metrics-explorer/04-add.png)

<span data-ttu-id="ddd14-134">Välj **redigera** på en befintlig eller ny diagram för att redigera den visar:</span><span class="sxs-lookup"><span data-stu-id="ddd14-134">Select **Edit** on an existing or new chart to edit what it shows:</span></span>

![Välj ett eller flera mått](./media/app-insights-metrics-explorer/08-select.png)

<span data-ttu-id="ddd14-136">Du kan visa fler än ett mått i ett diagram, även om det finns begränsningar om kombinationer som kan visas tillsammans.</span><span class="sxs-lookup"><span data-stu-id="ddd14-136">You can display more than one metric on a chart, though there are restrictions about the combinations that can be displayed together.</span></span> <span data-ttu-id="ddd14-137">När du väljer ett enskilt mått är några av de andra inaktiverade.</span><span class="sxs-lookup"><span data-stu-id="ddd14-137">As soon as you choose one metric, some of the others are disabled.</span></span>

<span data-ttu-id="ddd14-138">Om du har kodats [anpassade mått] [ track] i din app (anrop till TrackMetric och TrackEvent) visas här.</span><span class="sxs-lookup"><span data-stu-id="ddd14-138">If you coded [custom metrics][track] into your app (calls to TrackMetric and TrackEvent) they will be listed here.</span></span>

## <a name="segment-your-data"></a><span data-ttu-id="ddd14-139">Segmentera dina data</span><span class="sxs-lookup"><span data-stu-id="ddd14-139">Segment your data</span></span>
<span data-ttu-id="ddd14-140">Du kan dela ett mått med egenskapen – till exempel om du vill jämföra sidvyer på klienter med olika operativsystem.</span><span class="sxs-lookup"><span data-stu-id="ddd14-140">You can split a metric by property - for example, to compare page views on clients with different operating systems.</span></span>

<span data-ttu-id="ddd14-141">Välj ett diagram eller rutnät, aktivera gruppering och välj en egenskap som ska grupperas efter:</span><span class="sxs-lookup"><span data-stu-id="ddd14-141">Select a chart or grid, switch on grouping and pick a property to group by:</span></span>

![Välj gruppering på och ange en egenskap i Group By](./media/app-insights-metrics-explorer/15-segment.png)

> [!NOTE]
> <span data-ttu-id="ddd14-143">När du använder gruppering ger området och stapeldiagram typer en staplad visning.</span><span class="sxs-lookup"><span data-stu-id="ddd14-143">When you use grouping, the Area and Bar chart types provide a stacked display.</span></span> <span data-ttu-id="ddd14-144">Detta är lämpligt där metoden aggregering summan.</span><span class="sxs-lookup"><span data-stu-id="ddd14-144">This is suitable where the Aggregation method is Sum.</span></span> <span data-ttu-id="ddd14-145">Men om Aggregeringstyp medelvärde, välja vilka raden eller rutnät visas.</span><span class="sxs-lookup"><span data-stu-id="ddd14-145">But where the aggregation type is Average, choose the Line or Grid display types.</span></span>
>
>

<span data-ttu-id="ddd14-146">Om du har kodats [anpassade mått] [ track] i din app och de innehåller egenskapsvärden, du kommer att kunna markerar du egenskapen i listan.</span><span class="sxs-lookup"><span data-stu-id="ddd14-146">If you coded [custom metrics][track] into your app and they include property values, you'll be able to select the property in the list.</span></span>

<span data-ttu-id="ddd14-147">Är ett diagram för liten för segmenterade data?</span><span class="sxs-lookup"><span data-stu-id="ddd14-147">Is the chart too small for segmented data?</span></span> <span data-ttu-id="ddd14-148">Justera höjden:</span><span class="sxs-lookup"><span data-stu-id="ddd14-148">Adjust its height:</span></span>

![Justera skjutreglaget](./media/app-insights-metrics-explorer/18-height.png)

## <a name="aggregation-types"></a><span data-ttu-id="ddd14-150">Aggregeringen typer</span><span class="sxs-lookup"><span data-stu-id="ddd14-150">Aggregation types</span></span>
<span data-ttu-id="ddd14-151">Förklaring till sidan som standard visas vanligtvis aggregerade värde under diagrammet.</span><span class="sxs-lookup"><span data-stu-id="ddd14-151">The legend at the side by default usually shows the aggregated value over the period of the chart.</span></span> <span data-ttu-id="ddd14-152">Om du hovrar över diagrammet visar värdet vid den punkten.</span><span class="sxs-lookup"><span data-stu-id="ddd14-152">If you hover over the chart, it shows the value at that point.</span></span>

<span data-ttu-id="ddd14-153">Varje datapunkt i diagrammet är en aggregering av datavärden i föregående insamlingsintervall eller ”granularitet”.</span><span class="sxs-lookup"><span data-stu-id="ddd14-153">Each data point on the chart is an aggregate of the data values received in the preceding sampling interval or "granularity".</span></span> <span data-ttu-id="ddd14-154">Granulariteten varierar beroende på den övergripande tidsrymd i diagrammet och visas längst upp på bladet.</span><span class="sxs-lookup"><span data-stu-id="ddd14-154">The granularity is shown at the top of the blade, and varies with the overall timescale of the chart.</span></span>

<span data-ttu-id="ddd14-155">Mått kan sammanställas på olika sätt:</span><span class="sxs-lookup"><span data-stu-id="ddd14-155">Metrics can be aggregated in different ways:</span></span>

* <span data-ttu-id="ddd14-156">**Antal** antal händelser mottagna på exempelintervallet.</span><span class="sxs-lookup"><span data-stu-id="ddd14-156">**Count** is a count of the events received in the sampling interval.</span></span> <span data-ttu-id="ddd14-157">Den används för händelser, till exempel begäranden.</span><span class="sxs-lookup"><span data-stu-id="ddd14-157">It is used for events such as requests.</span></span> <span data-ttu-id="ddd14-158">Variationer i höjden av diagrammet anger variationer i händelserna ska inträffa.</span><span class="sxs-lookup"><span data-stu-id="ddd14-158">Variations in the height of the chart indicates variations in the rate at which the events occur.</span></span> <span data-ttu-id="ddd14-159">Men Observera att det numeriska värdet ändras när du ändrar exempelintervallet.</span><span class="sxs-lookup"><span data-stu-id="ddd14-159">But note that the numeric value changes when you change the sampling interval.</span></span>
* <span data-ttu-id="ddd14-160">**Summa** lägger till värden för alla datapunkter som tagits emot över exempelintervallet eller punkt i diagrammet.</span><span class="sxs-lookup"><span data-stu-id="ddd14-160">**Sum** adds up the values of all the data points received over the sampling interval, or the period of the chart.</span></span>
* <span data-ttu-id="ddd14-161">**Genomsnittlig** delas summan av antalet datapunkter som togs emot under period.</span><span class="sxs-lookup"><span data-stu-id="ddd14-161">**Average** divides the Sum by the number of data points received over the interval.</span></span>
* <span data-ttu-id="ddd14-162">**Unik** antal används för antal användare och konton.</span><span class="sxs-lookup"><span data-stu-id="ddd14-162">**Unique** counts are used for counts of users and accounts.</span></span> <span data-ttu-id="ddd14-163">Över exempelintervallet eller under diagrammet visar bilden antalet för olika användare visas i den tiden.</span><span class="sxs-lookup"><span data-stu-id="ddd14-163">Over the sampling interval, or over the period of the chart, the figure shows the count of different users seen in that time.</span></span>
* <span data-ttu-id="ddd14-164">**%**-procent versioner av varje aggregerings kan bara användas med segmenterade diagram.</span><span class="sxs-lookup"><span data-stu-id="ddd14-164">**%** - percentage versions of each aggregation are used only with segmented charts.</span></span> <span data-ttu-id="ddd14-165">Det totala antalet läggs alltid upp till 100% och i diagrammet visas olika komponenterna i totalt relativa bidrag.</span><span class="sxs-lookup"><span data-stu-id="ddd14-165">The total always adds up to 100%, and the chart shows the relative contribution of different components of a total.</span></span>

    ![Procentandel aggregering](./media/app-insights-metrics-explorer/percentage-aggregation.png)

### <a name="change-the-aggregation-type"></a><span data-ttu-id="ddd14-167">Ändra Aggregeringstyp</span><span class="sxs-lookup"><span data-stu-id="ddd14-167">Change the aggregation type</span></span>

![Redigera diagrammet och välj sedan aggregering](./media/app-insights-metrics-explorer/05-aggregation.png)

<span data-ttu-id="ddd14-169">Standardmetoden för varje mått visas när du skapar ett nytt schema eller när alla är avmarkerat:</span><span class="sxs-lookup"><span data-stu-id="ddd14-169">The default method for each metric is shown when you create a new chart or when all metrics are deselected:</span></span>

![Avmarkera alla mått att visa standardinställningar](./media/app-insights-metrics-explorer/06-total.png)

## <a name="pin-y-axis"></a><span data-ttu-id="ddd14-171">PIN-kod y-axeln</span><span class="sxs-lookup"><span data-stu-id="ddd14-171">Pin Y-axis</span></span> 
<span data-ttu-id="ddd14-172">Som standard visar ett diagram Y-axeln värden med början från noll till maxvärdena i dataområdet att ge en bild av quantum av värden.</span><span class="sxs-lookup"><span data-stu-id="ddd14-172">By default a chart shows Y axis values starting from zero till maximum values in the data range, to give a visual representation of quantum of the values.</span></span> <span data-ttu-id="ddd14-173">Men i vissa fall mer än quantum kan det vara intressant att visuellt inspektera mindre ändringar i värden.</span><span class="sxs-lookup"><span data-stu-id="ddd14-173">But in some cases more than the quantum it might be interesting to visually inspect minor changes in values.</span></span> <span data-ttu-id="ddd14-174">Y-axeln intervallet för anpassningar som den här användningen redigera funktionen fästa y-axeln lägsta eller högsta värdet på rätt plats.</span><span class="sxs-lookup"><span data-stu-id="ddd14-174">For customizations like this use the Y-axis range editing feature to pin the Y-axis minimum or maximum value at desired place.</span></span>
<span data-ttu-id="ddd14-175">Klicka på ”Avancerade inställningar” kryssrutan för att få fram det y-axel intervallet inställningar</span><span class="sxs-lookup"><span data-stu-id="ddd14-175">Click on "Advanced Settings" check box to bring up the Y-axis range Settings</span></span>

![Klicka på Avancerade inställningar, välja eget intervall och ange min maxvärde](./media/app-insights-metrics-explorer/y-axis-range.png)

## <a name="filter-your-data"></a><span data-ttu-id="ddd14-177">Filtrera dina data</span><span class="sxs-lookup"><span data-stu-id="ddd14-177">Filter your data</span></span>
<span data-ttu-id="ddd14-178">Visa bara mått för en vald uppsättning egenskapsvärden:</span><span class="sxs-lookup"><span data-stu-id="ddd14-178">To see just the metrics for a selected set of property values:</span></span>

![Klicka på filtret, expandera en egenskap och kontrollera vissa värden](./media/app-insights-metrics-explorer/19-filter.png)

<span data-ttu-id="ddd14-180">Om du inte markerar alla värden för en viss egenskap, är det samma som att markera dem: det finns inget filter på egenskapen.</span><span class="sxs-lookup"><span data-stu-id="ddd14-180">If you don't select any values for a particular property, it's the same as selecting them all: there is no filter on that property.</span></span>

<span data-ttu-id="ddd14-181">Observera att antalet händelser tillsammans med varje egenskapsvärde.</span><span class="sxs-lookup"><span data-stu-id="ddd14-181">Notice the counts of events alongside each property value.</span></span> <span data-ttu-id="ddd14-182">När du väljer värden för en egenskap, justeras antalet tillsammans med andra egenskapsvärden.</span><span class="sxs-lookup"><span data-stu-id="ddd14-182">When you select values of one property, the counts alongside other property values are adjusted.</span></span>

<span data-ttu-id="ddd14-183">Filter tillämpas på alla diagram på ett blad.</span><span class="sxs-lookup"><span data-stu-id="ddd14-183">Filters apply to all the charts on a blade.</span></span> <span data-ttu-id="ddd14-184">Om du vill att olika filter som används för olika diagram, skapa och spara olika mått blad.</span><span class="sxs-lookup"><span data-stu-id="ddd14-184">If you want different filters applied to different charts, create and save different metrics blades.</span></span> <span data-ttu-id="ddd14-185">Om du vill kan kan du fästa diagram från olika blad på instrumentpanelen så att du kan se dem tillsammans med varandra.</span><span class="sxs-lookup"><span data-stu-id="ddd14-185">If you want, you can pin charts from different blades to the dashboard, so that you can see them alongside each other.</span></span>

### <a name="remove-bot-and-web-test-traffic"></a><span data-ttu-id="ddd14-186">Ta bort bot och web test-trafik</span><span class="sxs-lookup"><span data-stu-id="ddd14-186">Remove bot and web test traffic</span></span>
<span data-ttu-id="ddd14-187">Med filtret **verklig eller syntetisk trafik** och kontrollera **verkliga**.</span><span class="sxs-lookup"><span data-stu-id="ddd14-187">Use the filter **Real or synthetic traffic** and check **Real**.</span></span>

<span data-ttu-id="ddd14-188">Du kan också filtrera efter **källa för syntetisk trafik**.</span><span class="sxs-lookup"><span data-stu-id="ddd14-188">You can also filter by **Source of synthetic traffic**.</span></span>

### <a name="to-add-properties-to-the-filter-list"></a><span data-ttu-id="ddd14-189">Att lägga till egenskaper i filterlistan</span><span class="sxs-lookup"><span data-stu-id="ddd14-189">To add properties to the filter list</span></span>
<span data-ttu-id="ddd14-190">Vill du filtrera telemetri för en kategori väljer själv?</span><span class="sxs-lookup"><span data-stu-id="ddd14-190">Would you like to filter telemetry on a category of your own choosing?</span></span> <span data-ttu-id="ddd14-191">Exempelvis kanske du dela upp användarna i olika kategorier och du vill att segmentera dina data med dessa kategorier.</span><span class="sxs-lookup"><span data-stu-id="ddd14-191">For example, maybe you divide up your users into different categories, and you would like segment your data by these categories.</span></span>

<span data-ttu-id="ddd14-192">[Skapa en egen egenskap](app-insights-api-custom-events-metrics.md#properties).</span><span class="sxs-lookup"><span data-stu-id="ddd14-192">[Create your own property](app-insights-api-custom-events-metrics.md#properties).</span></span> <span data-ttu-id="ddd14-193">Ange den i en [telemetri initieraren](app-insights-api-custom-events-metrics.md#defaults) så att det visas i alla telemetri - inklusive standard telemetri som skickas av olika SDK-moduler.</span><span class="sxs-lookup"><span data-stu-id="ddd14-193">Set it in a [Telemetry Initializer](app-insights-api-custom-events-metrics.md#defaults) to have it appear in all telemetry - including the standard telemetry sent by different SDK modules.</span></span>

## <a name="edit-the-chart-type"></a><span data-ttu-id="ddd14-194">Redigera diagramtypen</span><span class="sxs-lookup"><span data-stu-id="ddd14-194">Edit the chart type</span></span>
<span data-ttu-id="ddd14-195">Observera att du kan växla mellan rutnät och diagram:</span><span class="sxs-lookup"><span data-stu-id="ddd14-195">Notice that you can switch between grids and graphs:</span></span>

![Markera ett rutnät eller diagram och välj en diagramtyp](./media/app-insights-metrics-explorer/16-chart-grid.png)

## <a name="save-your-metrics-blade"></a><span data-ttu-id="ddd14-197">Spara mått-bladet</span><span class="sxs-lookup"><span data-stu-id="ddd14-197">Save your metrics blade</span></span>
<span data-ttu-id="ddd14-198">När du har skapat vissa diagram kan du spara dem som en favorit.</span><span class="sxs-lookup"><span data-stu-id="ddd14-198">When you've created some charts, save them as a favorite.</span></span> <span data-ttu-id="ddd14-199">Du kan välja att dela den med andra medlemmar, om du använder ett organisationskonto.</span><span class="sxs-lookup"><span data-stu-id="ddd14-199">You can choose whether to share it with other team members, if you use an organizational account.</span></span>

![Välj favorit](./media/app-insights-metrics-explorer/21-favorite-save.png)

<span data-ttu-id="ddd14-201">Visa bladet igen, **gå till bladet översikt** och öppna Favoriter:</span><span class="sxs-lookup"><span data-stu-id="ddd14-201">To see the blade again, **go to the overview blade** and open Favorites:</span></span>

![I bladet översikt välja Favoriter](./media/app-insights-metrics-explorer/22-favorite-get.png)

<span data-ttu-id="ddd14-203">Om du väljer relativt tidsintervall när du har sparat uppdateras bladet med senaste mått.</span><span class="sxs-lookup"><span data-stu-id="ddd14-203">If you chose Relative time range when you saved, the blade will be updated with the latest metrics.</span></span> <span data-ttu-id="ddd14-204">Om du väljer absolut tidsintervall visas samma data varje gång.</span><span class="sxs-lookup"><span data-stu-id="ddd14-204">If you chose Absolute time range, it will show the same data every time.</span></span>

## <a name="reset-the-blade"></a><span data-ttu-id="ddd14-205">Återställ bladet</span><span class="sxs-lookup"><span data-stu-id="ddd14-205">Reset the blade</span></span>
<span data-ttu-id="ddd14-206">Om du redigerar ett blad, men du vill gå tillbaka till den ursprungliga sparat, klickar du på Återställ.</span><span class="sxs-lookup"><span data-stu-id="ddd14-206">If you edit a blade but then you'd like to get back to the original saved set, just click Reset.</span></span>

![I knapparna överst i måttet Explorer](./media/app-insights-metrics-explorer/17-reset.png)

## <a name="live-metrics-stream"></a><span data-ttu-id="ddd14-208">Live mått dataström</span><span class="sxs-lookup"><span data-stu-id="ddd14-208">Live metrics stream</span></span>

<span data-ttu-id="ddd14-209">En mycket mer omedelbar vy över din telemetri öppna [Liveströmning](app-insights-live-stream.md).</span><span class="sxs-lookup"><span data-stu-id="ddd14-209">For a much more immediate view of your telemetry, open [Live Stream](app-insights-live-stream.md).</span></span> <span data-ttu-id="ddd14-210">De flesta mått ta några minuter innan den visas på grund av processen för aggregering.</span><span class="sxs-lookup"><span data-stu-id="ddd14-210">Most metrics take a few minutes to appear, because of the process of aggregation.</span></span> <span data-ttu-id="ddd14-211">Däremot är live mått optimerade för låg latens.</span><span class="sxs-lookup"><span data-stu-id="ddd14-211">By contrast, live metrics are optimized for low latency.</span></span> 

## <a name="set-alerts"></a><span data-ttu-id="ddd14-212">Ange aviseringar</span><span class="sxs-lookup"><span data-stu-id="ddd14-212">Set alerts</span></span>
<span data-ttu-id="ddd14-213">Lägga till en avisering om du vill meddelas via e-post med ovanliga värden för alla mått.</span><span class="sxs-lookup"><span data-stu-id="ddd14-213">To be notified by email of unusual values of any metric, add an alert.</span></span> <span data-ttu-id="ddd14-214">Du kan välja att skicka e-postmeddelandet till kontoadministratörer eller till specifika e-postadresser.</span><span class="sxs-lookup"><span data-stu-id="ddd14-214">You can choose either to send the email to the account administrators, or to specific email addresses.</span></span>

![Välj Varningsregler, Lägg till avisering i Metrics Explorer](./media/app-insights-metrics-explorer/appinsights-413setMetricAlert.png)

<span data-ttu-id="ddd14-216">[Mer information om aviseringar][alerts].</span><span class="sxs-lookup"><span data-stu-id="ddd14-216">[Learn more about alerts][alerts].</span></span>


## <a name="continuous-export"></a><span data-ttu-id="ddd14-217">Kontinuerlig export</span><span class="sxs-lookup"><span data-stu-id="ddd14-217">Continuous Export</span></span>
<span data-ttu-id="ddd14-218">Om du vill att data exporterats kontinuerligt så att du kan bearbeta externt, bör du använda [löpande export](app-insights-export-telemetry.md).</span><span class="sxs-lookup"><span data-stu-id="ddd14-218">If you want data continuously exported so that you can process it externally, consider using [Continuous export](app-insights-export-telemetry.md).</span></span>

### <a name="power-bi"></a><span data-ttu-id="ddd14-219">Power BI</span><span class="sxs-lookup"><span data-stu-id="ddd14-219">Power BI</span></span>
<span data-ttu-id="ddd14-220">Om du vill att ännu bättre vyer för dina data, kan du [exportera till Power BI](http://blogs.msdn.com/b/powerbi/archive/2015/11/04/explore-your-application-insights-data-with-power-bi.aspx).</span><span class="sxs-lookup"><span data-stu-id="ddd14-220">If you want even richer views of your data, you can [export to Power BI](http://blogs.msdn.com/b/powerbi/archive/2015/11/04/explore-your-application-insights-data-with-power-bi.aspx).</span></span>

## <a name="analytics"></a><span data-ttu-id="ddd14-221">Analys</span><span class="sxs-lookup"><span data-stu-id="ddd14-221">Analytics</span></span>
<span data-ttu-id="ddd14-222">[Analytics](app-insights-analytics.md) mer flexibla kan analysera dina telemetri med hjälp av ett kraftfullt frågespråk.</span><span class="sxs-lookup"><span data-stu-id="ddd14-222">[Analytics](app-insights-analytics.md) is a more versatile way to analyze your telemetry using a powerful query language.</span></span> <span data-ttu-id="ddd14-223">Använd det om du vill kombinera compute resultat från mått eller utföra en ingående undersökning av din app senaste prestanda.</span><span class="sxs-lookup"><span data-stu-id="ddd14-223">Use it if you want to combine or compute results from metrics, or perform an in-depth exploration of your app's recent performance.</span></span> 

<span data-ttu-id="ddd14-224">Från ett mått diagram, kan du klicka på ikonen Analytics direkt till motsvarande Analytics-fråga.</span><span class="sxs-lookup"><span data-stu-id="ddd14-224">From a metric chart, you can click the Analytics icon to get directly to the equivalent Analytics query.</span></span>

## <a name="troubleshooting"></a><span data-ttu-id="ddd14-225">Felsökning</span><span class="sxs-lookup"><span data-stu-id="ddd14-225">Troubleshooting</span></span>
<span data-ttu-id="ddd14-226">*Alla data visas inte i diagrammet.*</span><span class="sxs-lookup"><span data-stu-id="ddd14-226">*I don't see any data on my chart.*</span></span>

* <span data-ttu-id="ddd14-227">Filter tillämpas på alla diagram på bladet.</span><span class="sxs-lookup"><span data-stu-id="ddd14-227">Filters apply to all the charts on the blade.</span></span> <span data-ttu-id="ddd14-228">Se till att även om du fokusera på ett diagram, utan att ställa in ett filter som utesluter alla data på en annan.</span><span class="sxs-lookup"><span data-stu-id="ddd14-228">Make sure that, while you're focusing on one chart, you didn't set a filter that excludes all the data on another.</span></span>

    <span data-ttu-id="ddd14-229">Om du vill ange olika filter på olika diagram, skapa dem i olika blad, spara dem som separata Favoriter.</span><span class="sxs-lookup"><span data-stu-id="ddd14-229">If you want to set different filters on different charts, create them in different blades, save them as separate favorites.</span></span> <span data-ttu-id="ddd14-230">Om du vill kan fästa du dem på instrumentpanelen så att du kan se dem tillsammans med varandra.</span><span class="sxs-lookup"><span data-stu-id="ddd14-230">If you want, you can pin them to the dashboard so that you can see them alongside each other.</span></span>
* <span data-ttu-id="ddd14-231">Om du grupperar ett diagram av en egenskap som inte har definierats för måttet att kommer det finnas ingenting i diagrammet.</span><span class="sxs-lookup"><span data-stu-id="ddd14-231">If you group a chart by a property that is not defined on the metric, then there will be nothing on the chart.</span></span> <span data-ttu-id="ddd14-232">Prova att rensa 'group by- eller välj en annan gruppering-egenskap.</span><span class="sxs-lookup"><span data-stu-id="ddd14-232">Try clearing 'group by', or choose a different grouping property.</span></span>
* <span data-ttu-id="ddd14-233">Prestandadata (CPU, IO-frekvens och så vidare) är tillgängligt för Java-webbtjänster, Windows-skrivbordsappar [av IIS-program och tjänster om du installerar statusövervakaren](app-insights-monitor-performance-live-website-now.md), och [Azure Cloud Services](app-insights-azure.md).</span><span class="sxs-lookup"><span data-stu-id="ddd14-233">Performance data (CPU, IO rate, and so on) is available for Java web services, Windows desktop apps, [IIS web apps and services if you install status monitor](app-insights-monitor-performance-live-website-now.md), and [Azure Cloud Services](app-insights-azure.md).</span></span> <span data-ttu-id="ddd14-234">Det är inte tillgänglig för Azure websites.</span><span class="sxs-lookup"><span data-stu-id="ddd14-234">It isn't available for Azure websites.</span></span>

## <a name="video"></a><span data-ttu-id="ddd14-235">Video</span><span class="sxs-lookup"><span data-stu-id="ddd14-235">Video</span></span>

> [!VIDEO https://channel9.msdn.com/events/Connect/2016/112/player]

## <a name="next-steps"></a><span data-ttu-id="ddd14-236">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="ddd14-236">Next steps</span></span>
* [<span data-ttu-id="ddd14-237">Övervakning med Application Insights</span><span class="sxs-lookup"><span data-stu-id="ddd14-237">Monitoring usage with Application Insights</span></span>](app-insights-web-track-usage.md)
* [<span data-ttu-id="ddd14-238">Diagnostiska sökning</span><span class="sxs-lookup"><span data-stu-id="ddd14-238">Using Diagnostic Search</span></span>](app-insights-diagnostic-search.md)

<!--Link references-->

[alerts]: app-insights-alerts.md
[start]: app-insights-overview.md
[track]: app-insights-api-custom-events-metrics.md
