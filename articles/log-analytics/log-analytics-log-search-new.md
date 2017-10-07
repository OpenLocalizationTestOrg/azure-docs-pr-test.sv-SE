---
title: "aaaLog söker i OMS Log Analytics | Microsoft Docs"
description: "Du behöver en logg Sök tooretrieve några data från logganalys.  Den här artikeln beskriver hur ny logg sökningar används i logganalys och innehåller begrepp som du behöver toounderstand innan du skapar en."
services: log-analytics
documentationcenter: 
author: bwren
manager: carmonm
editor: 
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/25/2017
ms.author: bwren
ms.openlocfilehash: 08fda1d9eb9e6ab824ffb9e12af09832c3e3fad2
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="understanding-log-searches-in-log-analytics"></a>Förstå loggen söker i logganalys

> [!NOTE]
> Den här artikeln beskriver loggen söker i Azure Log Analytics med hjälp av hello nya frågespråk.  Du lär dig mer om hello nytt språk och få hello proceduren tooupgrade ditt arbetsområde på [uppgradera sökningen Azure logganalys arbetsytan toonew loggen](log-analytics-log-search-upgrade.md).  
>
> Om ditt arbetsområde inte har varit uppgraderade toohello nya frågespråk, bör du läsa för[söka efter data med hjälp av loggen sökningar i logganalys](log-analytics-log-searches.md).

Du behöver en logg Sök tooretrieve några data från logganalys.  Om du analyserar data i hello portal, meddelande konfigurera en regel för varning toobe om ett visst villkor eller att hämta data med hjälp av hello Log Analytics API du ska använda en sökning toospecify hello loggdata du vill.  Den här artikeln beskriver hur du använder loggen sökningar i logganalys samt begrepp som du bör känna till innan du skapar en. Se hello [nästa steg](#next-steps) avsnittet för information om att skapa och redigera loggen sökningar och referenser på hello frågespråk.

## <a name="where-log-searches-are-used"></a>Där loggen sökningar används

hello olika sätt som du ska använda loggen sökningar i logganalys inkludera hello följande:

- **Portaler.** Du kan utföra interaktiv dataanalys i hello-databas med hello [loggen Sök portal](log-analytics-log-search-log-search-portal.md) eller hello [Advanced Analytics portal](https://go.microsoft.com/fwlink/?linkid=856587).  Detta gör att du tooedit din fråga och analys av hello resulterar i en mängd olika format och visualiseringar.  De flesta frågor som du skapar kommer att starta i ett av hello portaler och kopieras sedan när du kontrollera att den fungerar som förväntat.
- **Varningsregler.** [Varna regler](log-analytics-alerts.md) proaktivt identifiera problem utifrån data i din arbetsyta.  Varje regel för varning baseras på en logg sökning som körs automatiskt med jämna mellanrum.  hello resultat är inspekterade toodetermine om en avisering ska skapas.
- **Vyer.**  Du kan skapa visualiseringar av data toobe ingår i instrumentpaneler för användare med [Vydesigner](log-analytics-view-designer.md).  Loggen sökningar tillhandahåller hello data som används av [paneler](log-analytics-view-designer-tiles.md) och [visualiseringen delar](log-analytics-view-designer-parts.md) i varje vy.  Du kan detaljnivån från visualiseringen delar hello loggen Sök portal tooperform ytterligare analyser av hello data.
- **Exportera.**  När du exporterar data från hello logganalys-arbetsytan tooExcel eller [Power BI](log-analytics-powerbi.md), du skapar en logg Sök toodefine hello data tooexport.
- **PowerShell.** Du kan köra ett PowerShell-skript från en kommandorad eller ett Azure Automation-runbook som använder [Get-AzureRmOperationalInsightsSearchResults](https://docs.microsoft.com/powershell/module/azurerm.operationalinsights/get-azurermoperationalinsightssearchresults?view=azurermps-4.0.0) tooretrieve data från logganalys.  Den här cmdleten kräver en fråga toodetermine hello data tooretrieve.
- **Log Analytics-API.**  Hej [logganalys logga Sök API](log-analytics-log-search-api.md) gör REST API-klient tooretrieve data från hello arbetsyta.  hello-API-begäran innehåller en fråga som körs mot logganalys toodetermine hello data tooretrieve.

![Log-sökningar](media/log-analytics-log-search-new/log-search-overview.png)

## <a name="how-log-analytics-data-is-organized"></a>Hur ordnas Log Analytics-data
När du skapar en fråga kan starta du genom att bestämma vilka tabeller har hello data som du letar efter. Varje [datakällan](log-analytics-data-sources.md) och [lösning](../operations-management-suite/operations-management-suite-solutions.md) lagrar data i dedikerade tabeller i hello logganalys-arbetsytan.  Dokumentation för varje datakälla och lösningen innehåller hello namnet på hello-datatyp som skapas och en beskrivning av var och en av dess egenskaper.     Många frågor kräver endast data från en enda tabeller, men andra kan använda en mängd olika alternativ tooinclude data från flera tabeller.

![Tabeller](media/log-analytics-log-search-new/queries-tables.png)


## <a name="writing-a-query"></a>Skriva en fråga
Hello kärnan i loggen söker i logganalys är [ett omfattande frågespråket](https://docs.loganalytics.io/) som låter dig hämta och analysera data från hello-databasen på en mängd olika sätt.  Samma fråga språk används för [Programinsikter](../application-insights/app-insights-analytics.md).  Lär dig hur toowrite en fråga är kritiska toocreating loggen söker i logganalys.  Normalt börjar med grundläggande frågor och sedan förlopp toouse avancerade mer funktioner enligt dina krav blir mer komplexa.

hello grundstrukturen för en fråga är en källtabell följt av en serie operatörer avgränsade med ett vertikalstreck `|`.  Du kan kedja samman flera operatorer toorefine hello data och utföra avancerade funktioner.

Anta att du vill ha toofind hello översta tio datorer med hello felhändelser för de flesta över hello senaste dagen.

    Event
    | where (EventLevelName == "Error")
    | where (TimeGenerated > ago(1days))
    | summarize ErrorCount = count() by Computer
    | top 10 by ErrorCount desc

Eller så kanske du vill toofind datorer som inte hade ett pulsslag hello sista dagen.

    Heartbeat
    | where TimeGenerated > ago(7d)
    | summarize max(TimeGenerated) by Computer
    | where max_TimeGenerated < ago(1d)  

Hur är det med linjediagram med hello processorbelastning för varje dator från förra veckan?

    Perf
    | where ObjectName == "Processor" and CounterName == "% Processor Time"
    | where TimeGenerated  between (startofweek(ago(7d)) .. endofweek(ago(7d)) )
    | summarize avg(CounterValue) by Computer, bin(TimeGenerated, 5min)
    | render timechart    

Du kan se från exemplen snabb som oavsett hello typ av data som du arbetar med, hello liknar strukturen för hello frågan.  Du kan dela upp det i steg där hello resulterande data från ett kommando skickas via hello pipeline toohello nästa kommando.

Fullständig dokumentation för hello Azure Log Analytics-frågespråket inklusive självstudier och Språkreferens finns hello [Azure logganalys fråga språk dokumentationen](https://docs.loganalytics.io/).

## <a name="next-steps"></a>Nästa steg

- Lär dig mer om hello [portaler om du använder toocreate och redigera loggen sökningar](log-analytics-log-search-portals.md).
- Checka ut en [självstudier om hur du skriver frågor](https://go.microsoft.com/fwlink/?linkid=856078) med hello nya frågespråk.
