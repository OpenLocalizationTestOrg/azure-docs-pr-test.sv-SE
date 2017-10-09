---
title: "aaaWhat är logganalys i Operations Management Suite (OMS)? | Microsoft Docs"
description: "Log Analytics är en tjänst i Operations Management Suite (OMS) som hjälper dig att samla och analysera driftsdata som genererats av resurser i molnet och i lokala miljöer.  Den här artikeln innehåller en kort översikt över hello olika komponenterna i logganalys och länkar toodetailed innehåll."
services: log-analytics
documentationcenter: 
author: bwren
manager: jwhit
editor: tysonn
ms.assetid: bd90b460-bacf-4345-ae31-26e155beac0e
ms.service: log-analytics
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 06/12/2017
ms.author: bwren
ms.openlocfilehash: b35da77e3782f7c1fe54cc34142f8cd9f2eb57d7
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="what-is-log-analytics"></a>Vad är Log Analytics?
Log Analytics är en tjänst i [Operations Management Suite \(OMS\) ](../operations-management-suite/operations-management-suite-overview.md) som övervakar dina molntjänster och lokala miljöer toomaintain deras tillgänglighet och prestanda.  Den samlar in data som genereras av resurser i dina miljöer i molnet och lokalt och från andra övervakning verktyg tooprovide analys över flera källor.  Den här artikeln innehåller en kort beskrivning av hello värde att Log Analytics ger en översikt över hur det fungerar och länkar toomore detaljerade innehållet så att du kan gå vidare.

## <a name="is-log-analytics-for-you"></a>Är Log Analytics rätt val för dig?
Om du inte har någon aktuell övervakning för Azure-miljön ska du börja med [Azure Monitor](../monitoring-and-diagnostics/monitoring-overview.md) som samlar in och analyserar övervakningsdata för dina Azure-resurser.  Logganalys kan [samla in data från Azure-Monitor](log-analytics-azure-storage.md) toocorrelate den med andra data och ange ytterligare analys.

Om du vill toomonitor din lokala miljö eller du har befintliga övervakning med tjänster som Azure-Monitor eller System Center Operations Manager, sedan logganalys kan lägga till betydande värde.  Den kan samla in data direkt från dina agenter och från de andra verktygen till en databas.  Analysverktygen i Log Analytics som loggsökningar, vyer och lösningar fungerar mot alla insamlade data och ger en centraliserad analys av hela miljön.


## <a name="using-log-analytics"></a>Använda Log Analytics
Du kan använda logganalys via hello OMS-portalen eller hello Azure-portalen som körs i en webbläsare och tillhandahålla dig med inställningar för tooconfiguration och flera verktyg tooanalyze och fungerar på insamlade data.  Från hello-portalen kan du utnyttja [logga sökningar](log-analytics-log-searches.md) där du skapar frågor tooanalyze insamlade data [instrumentpaneler](log-analytics-dashboards.md) som du kan anpassa med grafiska vyer av dina mest värdefulla sökningar och [lösningar](log-analytics-add-solutions.md) som ger ytterligare funktioner och analys verktyg.

hello bilden nedan är från hello OMS-portalen som visar hello instrumentpanel som innehåller översiktsinformation om hello [lösningar](#add-functionality-with-management-solutions) som är installerade i hello arbetsyta.  Du kan klicka på varje sida vid sida-toodrill ytterligare i hello data för lösningen.

![OMS-portalen](media/log-analytics-overview/portal.png)

Logganalys innehåller en fråga språk tooquickly hämta och konsolidera data i hello-databas.  Du kan skapa och spara [loggen sökningar](log-analytics-log-searches.md) toodirectly analysera data i hello-portalen eller har loggen sökningar körs automatiskt toocreate en avisering om hello resultaten av hello-frågan anger ett viktigt villkor.

![Loggsökning](media/log-analytics-overview/log-search.png)

tooget en snabb grafisk vy hello hälsotillståndet hos din övergripande miljö kan du lägga till grafik för sparad logg sökningar tooyour [instrumentpanelen](log-analytics-dashboards.md).   

![Instrumentpanel](media/log-analytics-overview/dashboard.png)

Ordning tooanalyze data utanför logganalys, du kan exportera hello data från hello OMS databasen till verktyg som [Power BI](log-analytics-powerbi.md) eller Excel.  Du kan också använda hello [loggen Sök API](log-analytics-log-search-api.md) toobuild anpassade lösningar som utnyttjar logganalys data eller toointegrate med andra system.

## <a name="add-functionality-with-management-solutions"></a>Lägga till funktioner med hanteringslösningar
[Hanteringslösningar](log-analytics-add-solutions.md) Lägg till funktioner tooOMS, vilket ger ytterligare data och analys verktyg tooLog Analytics.  De kan också definiera nya posttyper toobe samlas in som kan analyseras med loggen sökningar eller genom ytterligare användargränssnittet i hello lösning i hello instrumentpanelen.  hello exempel bilden nedan visar hello [ändringsspårning lösning](log-analytics-change-tracking.md)

![Ändra spårningslösning](media/log-analytics-overview/change-tracking.png)

Lösningar är tillgängliga för en mängd funktioner och ytterligare lösningar läggs till hela tiden.  Du kan enkelt bläddra bland tillgängliga lösningar och [lägga till dem tooyour OMS-arbetsytan](log-analytics-add-solutions.md) från hello lösningar galleriet eller Azure Marketplace.  Många distribueras automatiskt och börjar fungera direkt, medan andra kan kräva en viss konfiguration.

![Lösningsgalleriet](media/log-analytics-overview/solution-gallery.png)

## <a name="log-analytics-components"></a>Komponenter i Log Analytics
Hello är center logganalys hello OMS-databasen som är värd för hello Azure-molnet.  Data samlas in hello databasen från anslutna källor genom att konfigurera datakällor och lägga till lösningar tooyour prenumeration.  Datakällor och lösningar ska skapa olika posttyper som har en egen uppsättning egenskaper, men kan fortfarande analyseras tillsammans i frågor toohello databas.  Detta ger dig toouse hello samma verktyg och metoder toowork med olika typer av data som samlas in av olika källor.

![OMS databasen](media/log-analytics-overview/overview.png)

Anslutna källor är hello datorer och andra resurser som genererar data som samlas in av logganalys.  Detta kan omfatta agenter som installerats på [Windows](log-analytics-windows-agents.md)- och [Linux](log-analytics-linux-agents.md)-datorer som ansluter direkt, eller agenter i en [ansluten System Center Operations Manager-hanteringsgrupp](log-analytics-om-agents.md).  För Azure-resurser samlar Log Analytics in data från [Azure Monitor och Azure Diagnostics](log-analytics-azure-storage.md).

[Datakällor](log-analytics-data-sources.md) finns hello olika typer av data som samlas in från varje anslutna källa.  Detta inkluderar [händelser](log-analytics-data-sources-windows-events.md) och [prestandadata](log-analytics-data-sources-performance-counters.md) från [Windows](log-analytics-data-sources-windows-events.md) och Linux-agenter i tillägg toosources som [IIS-loggar](log-analytics-data-sources-iis-logs.md), och [anpassade textloggar](log-analytics-data-sources-custom-logs.md).  Du kan konfigurera varje datakälla som du vill toocollect och hello konfigureras automatiskt levererat tooeach anslutna källa.

Om du har anpassade krav, så du kan använda hello [HTTP Data Collector API: et](log-analytics-data-collector-api.md) toowrite data toohello databasen från en REST API-klient.

## <a name="log-analytics-architecture"></a>Log Analytics-arkitekturen
hello distributionskrav för Log Analytics är minimal eftersom hello centrala komponenter finns i hello Azure-molnet.  Detta innefattar hello i tillägg toohello tjänster som gör att du toocorrelate och analysera insamlade data.  hello portal kan nås från valfri webbläsare, så det finns inga krav för klientprogramvara.

Du måste installera agenter på [Windows](log-analytics-windows-agents.md)- och [Linux](log-analytics-linux-agents.md)-datorer, men det krävs ingen ytterligare agent för datorer som redan tillhör en [ansluten SCOM-hanteringsgrupp](log-analytics-om-agents.md).  SCOM agenter fortsätter toocommunicate med hanteringsservrar som vidarebefordrar sina data tooLog Analytics.  Vissa lösningar kräver dock agenter toocommunicate direkt med logganalys.  hello dokumentationen för varje lösning ska ange krav för klientkommunikation.

När du [registrerar dig för Log Analytics](log-analytics-get-started.md) skapar du en OMS-arbetsyta.  Du kan se hello arbetsytan som en unik logganalys-miljö med en egen lagringsplats för data, datakällor och lösningar. Du kan skapa flera arbetsytor i din prenumeration toosupport flera miljöer, till exempel produktion och testa.

![Log Analytics-arkitekturen](media/log-analytics-overview/architecture.png)

## <a name="next-steps"></a>Nästa steg
* [Registrera dig för ett kostnadsfritt konto logganalys](log-analytics-get-started.md) tootest i din egen miljö.
* Visa hello olika [datakällor](log-analytics-data-sources.md) tillgängliga toocollect data till hello OMS-databasen.
* [Bläddra hello tillgängliga lösningar i hello lösningar galleriet](log-analytics-add-solutions.md) tooadd funktioner tooLog Analytics.

