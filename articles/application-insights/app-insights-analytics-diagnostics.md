---
title: "Diagnostik för web app prestanda förändringar i Azure Application Insights för smartkort | Microsoft Docs"
description: "Automatisk diagnos av toppar eller stegen i prestanda telemetri från ditt webbprogram."
services: application-insights
documentationcenter: 
author: CFreemanwa
manager: carmonm
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 04/16/2017
ms.author: cfreeman
ms.openlocfilehash: 5e53bc714d89bf6204681349e7890e0b8fbc7046
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/18/2017
---
# <a name="diagnose-sudden-changes-in-your-app-telemetry"></a><span data-ttu-id="6994a-103">Diagnostisera plötsliga förändringar i din app telemetri</span><span class="sxs-lookup"><span data-stu-id="6994a-103">Diagnose sudden changes in your app telemetry</span></span>

<span data-ttu-id="6994a-104">*Den här funktionen är i förhandsgranskningen.*</span><span class="sxs-lookup"><span data-stu-id="6994a-104">*This feature is in preview.*</span></span>

<span data-ttu-id="6994a-105">Diagnostisera plötsliga ändringar i ditt webbprogram prestanda eller med en enda klickning!</span><span class="sxs-lookup"><span data-stu-id="6994a-105">Diagnose sudden changes in your web app’s performance or usage with a single click!</span></span> <span data-ttu-id="6994a-106">Smart diagnostik-funktionen är tillgänglig när du skapar en diagrammets i [Analytics](app-insights-analytics.md) i [Programinsikter](app-insights-overview.md).</span><span class="sxs-lookup"><span data-stu-id="6994a-106">The Smart Diagnostics feature is available whenever you create a time chart in [Analytics](app-insights-analytics.md) in [Application Insights](app-insights-overview.md).</span></span> <span data-ttu-id="6994a-107">Då det finns en ovanlig ändring från resultaten, till exempel en topp eller en dip trend identifierar Smart diagnostik ett mönster av dimensioner och relaterade värden som kan förklara ändringen.</span><span class="sxs-lookup"><span data-stu-id="6994a-107">Wherever there is an unusual change from the trend of your results, such as a spike or a dip, Smart Diagnostics identifies a pattern of dimensions and related values that might explain the change.</span></span> <span data-ttu-id="6994a-108">Detta hjälper dig att diagnostisera problemet snabbt.</span><span class="sxs-lookup"><span data-stu-id="6994a-108">This helps you diagnose the problem quickly.</span></span> 

<span data-ttu-id="6994a-109">Smart diagnostik i det här exemplet har identifierat ett mönster av egenskapsvärden som är associerade med ändringen och visar skillnaden mellan resultat med och utan att mönster:</span><span class="sxs-lookup"><span data-stu-id="6994a-109">In this example, Smart Diagnostics has identified a pattern of property values associated with the change, and highlights the difference between results with and without that pattern:</span></span>

![exempel analytics diagnostik resultat](./media/app-insights-analytics-diagnostics/analytics-result.png)
 

## <a name="diagnose-data-changes"></a><span data-ttu-id="6994a-111">Diagnostisera dataändringar</span><span class="sxs-lookup"><span data-stu-id="6994a-111">Diagnose data changes</span></span>

1.  <span data-ttu-id="6994a-112">Kör en fråga i Analytics och återges som en diagrammets.</span><span class="sxs-lookup"><span data-stu-id="6994a-112">Run a query in Analytics, and render it as a time chart.</span></span> 
2.  <span data-ttu-id="6994a-113">Klicka på helst markerade belastning om det finns en.</span><span class="sxs-lookup"><span data-stu-id="6994a-113">Click any highlighted peak point, if there is one.</span></span>
 
    ![belastning punkt](./media/app-insights-analytics-diagnostics/peak.png)

    <span data-ttu-id="6994a-115">Diagnostik tar några sekunder att identifiera ett mönster.</span><span class="sxs-lookup"><span data-stu-id="6994a-115">Diagnostics takes a few seconds to discover a pattern.</span></span>

3. <span data-ttu-id="6994a-116">Fliken Diagnostikresultat visar ett mönster som kan förklara data-avvikelse.</span><span class="sxs-lookup"><span data-stu-id="6994a-116">The Diagnostics Results tab shows a pattern which may explain your data discontinuity.</span></span>

    ![Resultat](./media/app-insights-analytics-diagnostics/result.png)
 
    <span data-ttu-id="6994a-118">Texten som visar de värden som visas för att korrelera med SKIFT.</span><span class="sxs-lookup"><span data-stu-id="6994a-118">The text shows the dimension values that appear to correlate with the shift.</span></span> <span data-ttu-id="6994a-119">I det här exemplet associeras det med en viss begäran och en viss webbläsarversion.</span><span class="sxs-lookup"><span data-stu-id="6994a-119">In this example, it’s associated with a particular request and a particular browser version.</span></span>

    <span data-ttu-id="6994a-120">Observera också två komponenter i diagrammet med filtret true och false.</span><span class="sxs-lookup"><span data-stu-id="6994a-120">Notice also the two components of the chart, with the filter true and false.</span></span> <span data-ttu-id="6994a-121">Komponenten FALSKT visar en oförändrad trend.</span><span class="sxs-lookup"><span data-stu-id="6994a-121">The false component shows an unchanged trend.</span></span> <span data-ttu-id="6994a-122">Med andra ord påverkas inte i resultaten telemetri om vi undantar problematiska kombinationen av dimensioner som har identifierats av diagnostik.</span><span class="sxs-lookup"><span data-stu-id="6994a-122">In other words, there is no change in the telemetry results, if we exclude the problematic combination of dimensions that Diagnostics has identified.</span></span> <span data-ttu-id="6994a-123">Däremot visas resultat inom den kombinationen en dramatisk förändring i det markerade området undersökning.</span><span class="sxs-lookup"><span data-stu-id="6994a-123">By contrast, the results within that combination do show a dramatic change within the highlighted area of investigation.</span></span> <span data-ttu-id="6994a-124">Det visar att Diagnostics har hittat en kombination av egenskaper som beskriver ändringen.</span><span class="sxs-lookup"><span data-stu-id="6994a-124">This shows that Diagnostics has found a combination of properties that explains the change.</span></span>

4.  <span data-ttu-id="6994a-125">Om mönstret är komplex, behöver du hovrar över **visa alla** att visa dimensioner.</span><span class="sxs-lookup"><span data-stu-id="6994a-125">If the pattern is complex, you need to hover over **Show all** to see the dimensions.</span></span>

    ![visa alla](./media/app-insights-analytics-diagnostics/show-all.png)
 
5.  <span data-ttu-id="6994a-127">Om diagnostik hittar några betydande mönster för att meddela om, visas sidan inga resultat.</span><span class="sxs-lookup"><span data-stu-id="6994a-127">In case Diagnostics finds no significant pattern to notify about, the ‘no results’ page will be presented.</span></span> <span data-ttu-id="6994a-128">Nu kan du ändra din fråga.</span><span class="sxs-lookup"><span data-stu-id="6994a-128">At this point, you may change your query.</span></span> <span data-ttu-id="6994a-129">Du kan till exempel begränsa tidsintervallet och diskretisering i Analytics-fråga för ytterligare analys och potentiellt bättre resultat.</span><span class="sxs-lookup"><span data-stu-id="6994a-129">For example, you could narrow the time range and binning in Analytics query, for a further analysis and potentially better results.</span></span>

<span data-ttu-id="6994a-130">Tillsammans med vetskapen om att en viss sida för webbplatsen finns ett fel på en viss webbläsare, kan du nu gå direkt till sidan problem och undersök nyligen gjorda ändringar.</span><span class="sxs-lookup"><span data-stu-id="6994a-130">Armed with the knowledge that a particular page of your website has a problem on a particular browser, you can now go straight to the problem page, and investigate recent changes.</span></span>

## <a name="try-the-demo"></a><span data-ttu-id="6994a-131">Prova demonstrationen</span><span class="sxs-lookup"><span data-stu-id="6994a-131">Try the demo</span></span>

<span data-ttu-id="6994a-132">[Klicka här om du vill se en demonstration](https://analytics.applicationinsights.io/demo?q=H4sIAAAAAAAAA3VSTY%2FTQAy991dYPXWlLf0QIO2KIiGWA3duiMPsxEnMzhe2p6WIH48nVUsuGylRNPOe3%2FOzN5vFZgPfRhL4VZHPIGM%2BCdgHdESgpMjOKx0RnsgNKYuSF%2BjRaWUE7xKMGIoBgTpMSv2Z0jBxOWc1QBWEPjM4EMUCP2uc0A3x8E5HKMi%2BEQNC7oHRbIgKdJWdUk5vmr9PvdkArildit%2Fcrk0lBDjnyhBzk%2FKVxdTy0QhNY6RhDPYqdlCy9XMV96NjBZc68IH8y6Tzuf01iZxeIZ%2FI5DqMOYmaQQRXNUdz6qGb5WOdSKEXnOozHtEFK%2Bh0qnq5YQzGF9DcoinoqbcigkO0NOZRNGOZaaBkMuat5xznFOtULKhG%2BdrGlVDhy%2B8SMlsETV8dD6gTd0YrbsBrFq6U1v%2Filv4C%2FsJpRJuwUrQTZ0P7eIDOHLeD1X67e7%2Fe7dbbB9htH%2Ffbu4vQDfvhFez%2B8a1h%2F1f3VSy%2BJ4Ol1oN8X4qN0qMZWv44HJanzKFLeJIltKcRpcbomP7gbHNkdV2Xe1uqO3g%2BwzOl1c3PvbmMlC7KjKlry2GX0w4s%2FgFoo5%2BhBAMAAA%3D%3D&timespan=PT24H) på exempeldata.</span><span class="sxs-lookup"><span data-stu-id="6994a-132">[Click here to see a demonstration](https://analytics.applicationinsights.io/demo?q=H4sIAAAAAAAAA3VSTY%2FTQAy991dYPXWlLf0QIO2KIiGWA3duiMPsxEnMzhe2p6WIH48nVUsuGylRNPOe3%2FOzN5vFZgPfRhL4VZHPIGM%2BCdgHdESgpMjOKx0RnsgNKYuSF%2BjRaWUE7xKMGIoBgTpMSv2Z0jBxOWc1QBWEPjM4EMUCP2uc0A3x8E5HKMi%2BEQNC7oHRbIgKdJWdUk5vmr9PvdkArildit%2Fcrk0lBDjnyhBzk%2FKVxdTy0QhNY6RhDPYqdlCy9XMV96NjBZc68IH8y6Tzuf01iZxeIZ%2FI5DqMOYmaQQRXNUdz6qGb5WOdSKEXnOozHtEFK%2Bh0qnq5YQzGF9DcoinoqbcigkO0NOZRNGOZaaBkMuat5xznFOtULKhG%2BdrGlVDhy%2B8SMlsETV8dD6gTd0YrbsBrFq6U1v%2Filv4C%2FsJpRJuwUrQTZ0P7eIDOHLeD1X67e7%2Fe7dbbB9htH%2Ffbu4vQDfvhFez%2B8a1h%2F1f3VSy%2BJ4Ol1oN8X4qN0qMZWv44HJanzKFLeJIltKcRpcbomP7gbHNkdV2Xe1uqO3g%2BwzOl1c3PvbmMlC7KjKlry2GX0w4s%2FgFoo5%2BhBAMAAA%3D%3D&timespan=PT24H) on sample data.</span></span>

## <a name="how-it-works"></a><span data-ttu-id="6994a-133">Hur det fungerar</span><span class="sxs-lookup"><span data-stu-id="6994a-133">How it works</span></span>

<span data-ttu-id="6994a-134">Smart diagnostik använder en avancerad oövervakade maskininlärningsalgoritmen baserat på de [DiffPatterns](app-insights-analytics-reference.md) igen.</span><span class="sxs-lookup"><span data-stu-id="6994a-134">Smart Diagnostics uses an advanced unsupervised machine learning algorithm based on the [DiffPatterns](app-insights-analytics-reference.md) operation.</span></span> <span data-ttu-id="6994a-135">Det ser ut för kandidat mönster som kan förklara data ändras.</span><span class="sxs-lookup"><span data-stu-id="6994a-135">It looks for candidate patterns that might explain the data change.</span></span> <span data-ttu-id="6994a-136">Det analyser effekten av varje kandidat på måttet och visar mönstret som bäst korrelerar med ändringen.</span><span class="sxs-lookup"><span data-stu-id="6994a-136">It analyses the impact of each candidate on the metric, and shows the pattern that best correlates with the change.</span></span>

## <a name="no-diagnostic-points"></a><span data-ttu-id="6994a-137">Inga diagnostiska poäng?</span><span class="sxs-lookup"><span data-stu-id="6994a-137">No diagnostic points?</span></span>

<span data-ttu-id="6994a-138">Smart diagnostik fungerar endast när följande villkor uppfylls:</span><span class="sxs-lookup"><span data-stu-id="6994a-138">Smart Diagnostics only works when the following criteria are satisfied:</span></span>

 * <span data-ttu-id="6994a-139">Smart diagnostik-inställningen är påslagen.</span><span class="sxs-lookup"><span data-stu-id="6994a-139">Smart Diagnostics setting is switched on.</span></span> <span data-ttu-id="6994a-140">Titta under inställningsikonen i Analytics.</span><span class="sxs-lookup"><span data-stu-id="6994a-140">Look under the Settings icon in Analytics.</span></span>
 * <span data-ttu-id="6994a-141">Alternativet Smart diagnostik i inställningar är markerad.</span><span class="sxs-lookup"><span data-stu-id="6994a-141">The Smart Diagnostics option in Analytics settings is selected.</span></span> 
 * <span data-ttu-id="6994a-142">Tidsaxeln: på x-axeln i diagrammet måste vara av typen `datetime`.</span><span class="sxs-lookup"><span data-stu-id="6994a-142">Time axis: The X-axis of the chart must be of type `datetime`.</span></span>
 * <span data-ttu-id="6994a-143">Linje eller område: diagnostik fungerar endast dessa typer av diagram.</span><span class="sxs-lookup"><span data-stu-id="6994a-143">Line or area chart: Diagnostics only works these types of chart.</span></span> <span data-ttu-id="6994a-144">Använd `| render timechart` eller `| render areachart` i slutet av din fråga, eller Välj raden eller ytdiagram från listrutan Väljaren.</span><span class="sxs-lookup"><span data-stu-id="6994a-144">Use `| render timechart` or `| render areachart` at the end of your query; or select Line or Area Chart from the drop-down selector.</span></span>
 * <span data-ttu-id="6994a-145">Avvikelse: Det måste finnas en betydande avvikelse i data.</span><span class="sxs-lookup"><span data-stu-id="6994a-145">Discontinuity: There must be a significant discontinuity in the data.</span></span>
 * <span data-ttu-id="6994a-146">Tillräckligt pekar på analysera.</span><span class="sxs-lookup"><span data-stu-id="6994a-146">Sufficient points to analyze.</span></span>
 * <span data-ttu-id="6994a-147">Fler än en sammanfatta-sats i frågan.</span><span class="sxs-lookup"><span data-stu-id="6994a-147">No more than one summarize clause in the query.</span></span>
 * <span data-ttu-id="6994a-148">Inga projekt-sats som innehåller en definition av namn före sammanfatta-sats.</span><span class="sxs-lookup"><span data-stu-id="6994a-148">No project clause that contains a name definition before the summarize clause.</span></span>

 
 ## <a name="related-articles"></a><span data-ttu-id="6994a-149">Relaterade artiklar</span><span class="sxs-lookup"><span data-stu-id="6994a-149">Related articles</span></span>

 * [<span data-ttu-id="6994a-150">Analytics-självstudier</span><span class="sxs-lookup"><span data-stu-id="6994a-150">Analytics tutorial</span></span>](app-insights-analytics-tour.md)
 * <span data-ttu-id="6994a-151">[Identifiering för smartkort](app-insights-proactive-diagnostics.md) automatiskt aviserar prestandaproblem.</span><span class="sxs-lookup"><span data-stu-id="6994a-151">[Smart detection](app-insights-proactive-diagnostics.md) automatically alerts you to performance issues.</span></span>