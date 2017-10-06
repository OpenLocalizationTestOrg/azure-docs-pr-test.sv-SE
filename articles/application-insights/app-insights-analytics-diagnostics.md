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
# <a name="diagnose-sudden-changes-in-your-app-telemetry"></a>Diagnostisera plötsliga förändringar i din app telemetri

*Den här funktionen är i förhandsgranskningen.*

Diagnostisera plötsliga ändringar i ditt webbprogram prestanda eller med en enda klickning! hello Smart diagnostik funktionen är tillgänglig när du skapar en diagrammets i [Analytics](app-insights-analytics.md) i [Programinsikter](app-insights-overview.md). Då det finns en ovanlig ändring från hello trend för dina resultatet, till exempel en topp eller en dip identifierar Smart diagnostik ett mönster av dimensioner och relaterade värden som kan förklara hello ändringen. Detta hjälper dig att diagnostisera problemet hello snabbt. 

Smart diagnostik i det här exemplet har identifierat ett mönster av egenskapsvärden som är associerade med hello ändringen och visar hello skillnaden mellan resultat med och utan att mönster:

![exempel analytics diagnostik resultat](./media/app-insights-analytics-diagnostics/analytics-result.png)
 

## <a name="diagnose-data-changes"></a>Diagnostisera dataändringar

1.  Kör en fråga i Analytics och återges som en diagrammets. 
2.  Klicka på helst markerade belastning om det finns en.
 
    ![belastning punkt](./media/app-insights-analytics-diagnostics/peak.png)

    Diagnostik tar några sekunder toodiscover ett mönster.

3. hello diagnostik resultaten på fliken visas ett mönster som kan förklara data-avvikelse.

    ![Resultat](./media/app-insights-analytics-diagnostics/result.png)
 
    hello text visar hello värden som visas toocorrelate med hello SKIFT. I det här exemplet associeras det med en viss begäran och en viss webbläsarversion.

    Notera också hello två komponenter i hello-diagram, hello filter true och false. hello FALSKT komponent visas en oförändrad trend. Med andra ord har ingen ändring i hello telemetri resultat om vi undantar hello problematiska kombination av dimensioner som har identifierats av diagnostik. Däremot visas hello resultat inom den kombinationen en dramatisk förändring inom hello markerade området undersökning. Det visar att Diagnostics har hittat en kombination av egenskaper som förklarar hello ändringen.

4.  Om hello mönstret är komplex, behöver du toohover över **visa alla** toosee hello dimensioner.

    ![visa alla](./media/app-insights-analytics-diagnostics/show-all.png)
 
5.  Om diagnostik hittar några betydande mönster toonotify om hello inga resultatsida visas. Nu kan du ändra din fråga. Du kan till exempel begränsa hello tidsintervall och diskretisering i Analytics-fråga för ytterligare analys och potentiellt bättre resultat.

Tillsammans med hello kunskap att en viss sida för webbplatsen finns ett fel på en viss webbläsare, kan du nu gå raka toohello problemet sidan och undersök nyligen gjorda ändringar.

## <a name="try-hello-demo"></a>Prova hello demonstrationer

[Klicka här toosee en demonstration](https://analytics.applicationinsights.io/demo?q=H4sIAAAAAAAAA3VSTY%2FTQAy991dYPXWlLf0QIO2KIiGWA3duiMPsxEnMzhe2p6WIH48nVUsuGylRNPOe3%2FOzN5vFZgPfRhL4VZHPIGM%2BCdgHdESgpMjOKx0RnsgNKYuSF%2BjRaWUE7xKMGIoBgTpMSv2Z0jBxOWc1QBWEPjM4EMUCP2uc0A3x8E5HKMi%2BEQNC7oHRbIgKdJWdUk5vmr9PvdkArildit%2Fcrk0lBDjnyhBzk%2FKVxdTy0QhNY6RhDPYqdlCy9XMV96NjBZc68IH8y6Tzuf01iZxeIZ%2FI5DqMOYmaQQRXNUdz6qGb5WOdSKEXnOozHtEFK%2Bh0qnq5YQzGF9DcoinoqbcigkO0NOZRNGOZaaBkMuat5xznFOtULKhG%2BdrGlVDhy%2B8SMlsETV8dD6gTd0YrbsBrFq6U1v%2Filv4C%2FsJpRJuwUrQTZ0P7eIDOHLeD1X67e7%2Fe7dbbB9htH%2Ffbu4vQDfvhFez%2B8a1h%2F1f3VSy%2BJ4Ol1oN8X4qN0qMZWv44HJanzKFLeJIltKcRpcbomP7gbHNkdV2Xe1uqO3g%2BwzOl1c3PvbmMlC7KjKlry2GX0w4s%2FgFoo5%2BhBAMAAA%3D%3D&timespan=PT24H) på exempeldata.

## <a name="how-it-works"></a>Hur det fungerar

Smart diagnostik använder en avancerad oövervakade maskininlärning algoritm baserat på hello [DiffPatterns](app-insights-analytics-reference.md) igen. Det ser ut för kandidat mönster som kan förklara hello data ändras. Den analyser hello effekten av varje kandidat på hello mått och visar hello mönster som bäst korrelerar med hello ändringen.

## <a name="no-diagnostic-points"></a>Inga diagnostiska poäng?

Smart diagnostik fungerar bara om hello följande villkor är uppfyllda:

 * Smart diagnostik-inställningen är påslagen. Titta under hello inställningsikonen i Analytics.
 * hello Smart diagnostik alternativ i inställningar har valts. 
 * Tidsaxeln: hello x-axeln i diagrammet hello måste vara av typen `datetime`.
 * Linje eller område: diagnostik fungerar endast dessa typer av diagram. Använd `| render timechart` eller `| render areachart` hello slutet av din fråga; eller Välj raden eller ytdiagram hello nedrullningsbara väljare.
 * Avvikelse: Det måste finnas en betydande avvikelse i hello data.
 * Tillräckligt punkter tooanalyze.
 * Fler än en sammanfattar-satsen i hello frågan.
 * Inga projekt-sats som innehåller en definition av namn innan hello sammanfattar satsen.

 
 ## <a name="related-articles"></a>Relaterade artiklar

 * [Analytics-självstudier](app-insights-analytics-tour.md)
 * [Identifiering för smartkort](app-insights-proactive-diagnostics.md) automatiskt aviserar tooperformance problem.
