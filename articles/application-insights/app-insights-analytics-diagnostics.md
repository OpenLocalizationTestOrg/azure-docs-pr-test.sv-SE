---
title: "aaaSmart diagnostik för web app prestanda förändringar i Azure Application Insights | Microsoft Docs"
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
ms.openlocfilehash: 8891762c4a4bfdb08b647fe3b702349eb30ec9c0
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="diagnose-sudden-changes-in-your-app-telemetry"></a><span data-ttu-id="3fe70-103">Diagnostisera plötsliga förändringar i din app telemetri</span><span class="sxs-lookup"><span data-stu-id="3fe70-103">Diagnose sudden changes in your app telemetry</span></span>

<span data-ttu-id="3fe70-104">*Den här funktionen är i förhandsgranskningen.*</span><span class="sxs-lookup"><span data-stu-id="3fe70-104">*This feature is in preview.*</span></span>

<span data-ttu-id="3fe70-105">Diagnostisera plötsliga ändringar i ditt webbprogram prestanda eller med en enda klickning!</span><span class="sxs-lookup"><span data-stu-id="3fe70-105">Diagnose sudden changes in your web app’s performance or usage with a single click!</span></span> <span data-ttu-id="3fe70-106">hello Smart diagnostik funktionen är tillgänglig när du skapar en diagrammets i [Analytics](app-insights-analytics.md) i [Programinsikter](app-insights-overview.md).</span><span class="sxs-lookup"><span data-stu-id="3fe70-106">hello Smart Diagnostics feature is available whenever you create a time chart in [Analytics](app-insights-analytics.md) in [Application Insights](app-insights-overview.md).</span></span> <span data-ttu-id="3fe70-107">Då det finns en ovanlig ändring från hello trend för dina resultatet, till exempel en topp eller en dip identifierar Smart diagnostik ett mönster av dimensioner och relaterade värden som kan förklara hello ändringen.</span><span class="sxs-lookup"><span data-stu-id="3fe70-107">Wherever there is an unusual change from hello trend of your results, such as a spike or a dip, Smart Diagnostics identifies a pattern of dimensions and related values that might explain hello change.</span></span> <span data-ttu-id="3fe70-108">Detta hjälper dig att diagnostisera problemet hello snabbt.</span><span class="sxs-lookup"><span data-stu-id="3fe70-108">This helps you diagnose hello problem quickly.</span></span> 

<span data-ttu-id="3fe70-109">Smart diagnostik i det här exemplet har identifierat ett mönster av egenskapsvärden som är associerade med hello ändringen och visar hello skillnaden mellan resultat med och utan att mönster:</span><span class="sxs-lookup"><span data-stu-id="3fe70-109">In this example, Smart Diagnostics has identified a pattern of property values associated with hello change, and highlights hello difference between results with and without that pattern:</span></span>

![exempel analytics diagnostik resultat](./media/app-insights-analytics-diagnostics/analytics-result.png)
 

## <a name="diagnose-data-changes"></a><span data-ttu-id="3fe70-111">Diagnostisera dataändringar</span><span class="sxs-lookup"><span data-stu-id="3fe70-111">Diagnose data changes</span></span>

1.  <span data-ttu-id="3fe70-112">Kör en fråga i Analytics och återges som en diagrammets.</span><span class="sxs-lookup"><span data-stu-id="3fe70-112">Run a query in Analytics, and render it as a time chart.</span></span> 
2.  <span data-ttu-id="3fe70-113">Klicka på helst markerade belastning om det finns en.</span><span class="sxs-lookup"><span data-stu-id="3fe70-113">Click any highlighted peak point, if there is one.</span></span>
 
    ![belastning punkt](./media/app-insights-analytics-diagnostics/peak.png)

    <span data-ttu-id="3fe70-115">Diagnostik tar några sekunder toodiscover ett mönster.</span><span class="sxs-lookup"><span data-stu-id="3fe70-115">Diagnostics takes a few seconds toodiscover a pattern.</span></span>

3. <span data-ttu-id="3fe70-116">hello diagnostik resultaten på fliken visas ett mönster som kan förklara data-avvikelse.</span><span class="sxs-lookup"><span data-stu-id="3fe70-116">hello Diagnostics Results tab shows a pattern which may explain your data discontinuity.</span></span>

    ![Resultat](./media/app-insights-analytics-diagnostics/result.png)
 
    <span data-ttu-id="3fe70-118">hello text visar hello värden som visas toocorrelate med hello SKIFT.</span><span class="sxs-lookup"><span data-stu-id="3fe70-118">hello text shows hello dimension values that appear toocorrelate with hello shift.</span></span> <span data-ttu-id="3fe70-119">I det här exemplet associeras det med en viss begäran och en viss webbläsarversion.</span><span class="sxs-lookup"><span data-stu-id="3fe70-119">In this example, it’s associated with a particular request and a particular browser version.</span></span>

    <span data-ttu-id="3fe70-120">Notera också hello två komponenter i hello-diagram, hello filter true och false.</span><span class="sxs-lookup"><span data-stu-id="3fe70-120">Notice also hello two components of hello chart, with hello filter true and false.</span></span> <span data-ttu-id="3fe70-121">hello FALSKT komponent visas en oförändrad trend.</span><span class="sxs-lookup"><span data-stu-id="3fe70-121">hello false component shows an unchanged trend.</span></span> <span data-ttu-id="3fe70-122">Med andra ord har ingen ändring i hello telemetri resultat om vi undantar hello problematiska kombination av dimensioner som har identifierats av diagnostik.</span><span class="sxs-lookup"><span data-stu-id="3fe70-122">In other words, there is no change in hello telemetry results, if we exclude hello problematic combination of dimensions that Diagnostics has identified.</span></span> <span data-ttu-id="3fe70-123">Däremot visas hello resultat inom den kombinationen en dramatisk förändring inom hello markerade området undersökning.</span><span class="sxs-lookup"><span data-stu-id="3fe70-123">By contrast, hello results within that combination do show a dramatic change within hello highlighted area of investigation.</span></span> <span data-ttu-id="3fe70-124">Det visar att Diagnostics har hittat en kombination av egenskaper som förklarar hello ändringen.</span><span class="sxs-lookup"><span data-stu-id="3fe70-124">This shows that Diagnostics has found a combination of properties that explains hello change.</span></span>

4.  <span data-ttu-id="3fe70-125">Om hello mönstret är komplex, behöver du toohover över **visa alla** toosee hello dimensioner.</span><span class="sxs-lookup"><span data-stu-id="3fe70-125">If hello pattern is complex, you need toohover over **Show all** toosee hello dimensions.</span></span>

    ![visa alla](./media/app-insights-analytics-diagnostics/show-all.png)
 
5.  <span data-ttu-id="3fe70-127">Om diagnostik hittar några betydande mönster toonotify om hello inga resultatsida visas.</span><span class="sxs-lookup"><span data-stu-id="3fe70-127">In case Diagnostics finds no significant pattern toonotify about, hello ‘no results’ page will be presented.</span></span> <span data-ttu-id="3fe70-128">Nu kan du ändra din fråga.</span><span class="sxs-lookup"><span data-stu-id="3fe70-128">At this point, you may change your query.</span></span> <span data-ttu-id="3fe70-129">Du kan till exempel begränsa hello tidsintervall och diskretisering i Analytics-fråga för ytterligare analys och potentiellt bättre resultat.</span><span class="sxs-lookup"><span data-stu-id="3fe70-129">For example, you could narrow hello time range and binning in Analytics query, for a further analysis and potentially better results.</span></span>

<span data-ttu-id="3fe70-130">Tillsammans med hello kunskap att en viss sida för webbplatsen finns ett fel på en viss webbläsare, kan du nu gå raka toohello problemet sidan och undersök nyligen gjorda ändringar.</span><span class="sxs-lookup"><span data-stu-id="3fe70-130">Armed with hello knowledge that a particular page of your website has a problem on a particular browser, you can now go straight toohello problem page, and investigate recent changes.</span></span>

## <a name="try-hello-demo"></a><span data-ttu-id="3fe70-131">Prova hello demonstrationer</span><span class="sxs-lookup"><span data-stu-id="3fe70-131">Try hello demo</span></span>

<span data-ttu-id="3fe70-132">[Klicka här toosee en demonstration](https://analytics.applicationinsights.io/demo?q=H4sIAAAAAAAAA3VSTY%2FTQAy991dYPXWlLf0QIO2KIiGWA3duiMPsxEnMzhe2p6WIH48nVUsuGylRNPOe3%2FOzN5vFZgPfRhL4VZHPIGM%2BCdgHdESgpMjOKx0RnsgNKYuSF%2BjRaWUE7xKMGIoBgTpMSv2Z0jBxOWc1QBWEPjM4EMUCP2uc0A3x8E5HKMi%2BEQNC7oHRbIgKdJWdUk5vmr9PvdkArildit%2Fcrk0lBDjnyhBzk%2FKVxdTy0QhNY6RhDPYqdlCy9XMV96NjBZc68IH8y6Tzuf01iZxeIZ%2FI5DqMOYmaQQRXNUdz6qGb5WOdSKEXnOozHtEFK%2Bh0qnq5YQzGF9DcoinoqbcigkO0NOZRNGOZaaBkMuat5xznFOtULKhG%2BdrGlVDhy%2B8SMlsETV8dD6gTd0YrbsBrFq6U1v%2Filv4C%2FsJpRJuwUrQTZ0P7eIDOHLeD1X67e7%2Fe7dbbB9htH%2Ffbu4vQDfvhFez%2B8a1h%2F1f3VSy%2BJ4Ol1oN8X4qN0qMZWv44HJanzKFLeJIltKcRpcbomP7gbHNkdV2Xe1uqO3g%2BwzOl1c3PvbmMlC7KjKlry2GX0w4s%2FgFoo5%2BhBAMAAA%3D%3D&timespan=PT24H) på exempeldata.</span><span class="sxs-lookup"><span data-stu-id="3fe70-132">[Click here toosee a demonstration](https://analytics.applicationinsights.io/demo?q=H4sIAAAAAAAAA3VSTY%2FTQAy991dYPXWlLf0QIO2KIiGWA3duiMPsxEnMzhe2p6WIH48nVUsuGylRNPOe3%2FOzN5vFZgPfRhL4VZHPIGM%2BCdgHdESgpMjOKx0RnsgNKYuSF%2BjRaWUE7xKMGIoBgTpMSv2Z0jBxOWc1QBWEPjM4EMUCP2uc0A3x8E5HKMi%2BEQNC7oHRbIgKdJWdUk5vmr9PvdkArildit%2Fcrk0lBDjnyhBzk%2FKVxdTy0QhNY6RhDPYqdlCy9XMV96NjBZc68IH8y6Tzuf01iZxeIZ%2FI5DqMOYmaQQRXNUdz6qGb5WOdSKEXnOozHtEFK%2Bh0qnq5YQzGF9DcoinoqbcigkO0NOZRNGOZaaBkMuat5xznFOtULKhG%2BdrGlVDhy%2B8SMlsETV8dD6gTd0YrbsBrFq6U1v%2Filv4C%2FsJpRJuwUrQTZ0P7eIDOHLeD1X67e7%2Fe7dbbB9htH%2Ffbu4vQDfvhFez%2B8a1h%2F1f3VSy%2BJ4Ol1oN8X4qN0qMZWv44HJanzKFLeJIltKcRpcbomP7gbHNkdV2Xe1uqO3g%2BwzOl1c3PvbmMlC7KjKlry2GX0w4s%2FgFoo5%2BhBAMAAA%3D%3D&timespan=PT24H) on sample data.</span></span>

## <a name="how-it-works"></a><span data-ttu-id="3fe70-133">Hur det fungerar</span><span class="sxs-lookup"><span data-stu-id="3fe70-133">How it works</span></span>

<span data-ttu-id="3fe70-134">Smart diagnostik använder en avancerad oövervakade maskininlärning algoritm baserat på hello [DiffPatterns](app-insights-analytics-reference.md) igen.</span><span class="sxs-lookup"><span data-stu-id="3fe70-134">Smart Diagnostics uses an advanced unsupervised machine learning algorithm based on hello [DiffPatterns](app-insights-analytics-reference.md) operation.</span></span> <span data-ttu-id="3fe70-135">Det ser ut för kandidat mönster som kan förklara hello data ändras.</span><span class="sxs-lookup"><span data-stu-id="3fe70-135">It looks for candidate patterns that might explain hello data change.</span></span> <span data-ttu-id="3fe70-136">Den analyser hello effekten av varje kandidat på hello mått och visar hello mönster som bäst korrelerar med hello ändringen.</span><span class="sxs-lookup"><span data-stu-id="3fe70-136">It analyses hello impact of each candidate on hello metric, and shows hello pattern that best correlates with hello change.</span></span>

## <a name="no-diagnostic-points"></a><span data-ttu-id="3fe70-137">Inga diagnostiska poäng?</span><span class="sxs-lookup"><span data-stu-id="3fe70-137">No diagnostic points?</span></span>

<span data-ttu-id="3fe70-138">Smart diagnostik fungerar bara om hello följande villkor är uppfyllda:</span><span class="sxs-lookup"><span data-stu-id="3fe70-138">Smart Diagnostics only works when hello following criteria are satisfied:</span></span>

 * <span data-ttu-id="3fe70-139">Smart diagnostik-inställningen är påslagen.</span><span class="sxs-lookup"><span data-stu-id="3fe70-139">Smart Diagnostics setting is switched on.</span></span> <span data-ttu-id="3fe70-140">Titta under hello inställningsikonen i Analytics.</span><span class="sxs-lookup"><span data-stu-id="3fe70-140">Look under hello Settings icon in Analytics.</span></span>
 * <span data-ttu-id="3fe70-141">hello Smart diagnostik alternativ i inställningar har valts.</span><span class="sxs-lookup"><span data-stu-id="3fe70-141">hello Smart Diagnostics option in Analytics settings is selected.</span></span> 
 * <span data-ttu-id="3fe70-142">Tidsaxeln: hello x-axeln i diagrammet hello måste vara av typen `datetime`.</span><span class="sxs-lookup"><span data-stu-id="3fe70-142">Time axis: hello X-axis of hello chart must be of type `datetime`.</span></span>
 * <span data-ttu-id="3fe70-143">Linje eller område: diagnostik fungerar endast dessa typer av diagram.</span><span class="sxs-lookup"><span data-stu-id="3fe70-143">Line or area chart: Diagnostics only works these types of chart.</span></span> <span data-ttu-id="3fe70-144">Använd `| render timechart` eller `| render areachart` hello slutet av din fråga; eller Välj raden eller ytdiagram hello nedrullningsbara väljare.</span><span class="sxs-lookup"><span data-stu-id="3fe70-144">Use `| render timechart` or `| render areachart` at hello end of your query; or select Line or Area Chart from hello drop-down selector.</span></span>
 * <span data-ttu-id="3fe70-145">Avvikelse: Det måste finnas en betydande avvikelse i hello data.</span><span class="sxs-lookup"><span data-stu-id="3fe70-145">Discontinuity: There must be a significant discontinuity in hello data.</span></span>
 * <span data-ttu-id="3fe70-146">Tillräckligt punkter tooanalyze.</span><span class="sxs-lookup"><span data-stu-id="3fe70-146">Sufficient points tooanalyze.</span></span>
 * <span data-ttu-id="3fe70-147">Fler än en sammanfattar-satsen i hello frågan.</span><span class="sxs-lookup"><span data-stu-id="3fe70-147">No more than one summarize clause in hello query.</span></span>
 * <span data-ttu-id="3fe70-148">Inga projekt-sats som innehåller en definition av namn innan hello sammanfattar satsen.</span><span class="sxs-lookup"><span data-stu-id="3fe70-148">No project clause that contains a name definition before hello summarize clause.</span></span>

 
 ## <a name="related-articles"></a><span data-ttu-id="3fe70-149">Relaterade artiklar</span><span class="sxs-lookup"><span data-stu-id="3fe70-149">Related articles</span></span>

 * [<span data-ttu-id="3fe70-150">Analytics-självstudier</span><span class="sxs-lookup"><span data-stu-id="3fe70-150">Analytics tutorial</span></span>](app-insights-analytics-tour.md)
 * <span data-ttu-id="3fe70-151">[Identifiering för smartkort](app-insights-proactive-diagnostics.md) automatiskt aviserar tooperformance problem.</span><span class="sxs-lookup"><span data-stu-id="3fe70-151">[Smart detection](app-insights-proactive-diagnostics.md) automatically alerts you tooperformance issues.</span></span>
