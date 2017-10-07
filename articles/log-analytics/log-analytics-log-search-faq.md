---
title: "vanliga frågor och svar om aaaLog Analytics ny logg sökning | Microsoft Docs"
description: "Den här artikeln innehåller vanliga frågor och svar om hello uppgraderingen i logganalys toohello nya frågespråk."
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
ms.date: 08/27/2017
ms.author: bwren
ms.openlocfilehash: b8664c8329fab0547f270793fa13e8cdd06ba637
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="log-analytics-new-log-search-faq-and-known-issues"></a>Nya logganalys logga Sök vanliga frågor och kända problem

Den här artikeln innehåller vanliga frågor och kända problem angående hello uppgradering av [logganalys toohello nya frågespråket](log-analytics-log-search-upgrade.md).  Du bör läsa igenom hela artikeln innan du gör hello beslut tooupgrade din arbetsyta.


## <a name="alerts"></a>Aviseringar

### <a name="question-i-have-a-lot-of-alert-rules-do-i-need-toocreate-them-again-in-hello-new-language-after-i-upgrade"></a>Fråga: Jag har en stor mängd Varningsregler. Behöver jag toocreate dem igen i hello nya språk efter uppgraderingen?  
Nej, din Varningsregler är automatiskt konverterade toohello nytt sökspråk under uppgraderingen.  


## <a name="computer-groups"></a>Datorgrupper

### <a name="question-im-getting-errors-when-trying-toouse-computer-groups--has-their-syntax-changed"></a>Fråga: Jag får felmeddelanden när du försöker toouse datorgrupper.  Har ändrats för sina syntax
Ja, grupperas hello syntax för att använda datorn ändringar när ditt arbetsområde har uppgraderats.  Se [datorgrupper i logganalys logga sökningar](log-analytics-computer-groups.md) mer information.

### <a name="known-issue-groups-imported-from-active-directory"></a>Kända problem: grupper som importeras från Active Directory
För närvarande kan du skapa en fråga som använder en datorgrupp som importeras från Active Directory.  Skapa en ny datorgrupp med hello importeras Active Directory-grupp och sedan använda den nya gruppen i frågan som en tillfällig lösning förrän problemet har åtgärdats.

Ett exempel på en fråga-toocreate en ny datorgrupp som innehåller en importerade Active Directory-grupp är följande:

    ComputerGroup | where GroupSource == "ActiveDirectory" and Group == "AD Group Name" and TimeGenerated >= ago(24h) | distinct Computer


## <a name="dashboards"></a>Instrumentpaneler

### <a name="question-can-i-still-use-dashboards-in-an-upgraded-workspace"></a>Fråga: Kan jag fortsätta att använda instrumentpaneler i en uppgraderad arbetsytan?
Du kan fortsätta toouse alla paneler som du lagt till för**min instrumentpanel** innan ditt arbetsområde har uppgraderats, men du kan inte redigera dessa paneler eller lägga till nya.  Du kan fortsätta toocreate och redigera vyer med [Vydesigner](log-analytics-view-designer.md) och även skapa instrumentpaneler i hello Azure-portalen.


## <a name="log-searches"></a>Log-sökningar

### <a name="question-i-have-saved-searches-outside-of-my-upgraded-workspace-can-i-convert-them-toohello-new-search-language-automatically"></a>Fråga: Jag har sparade sökningar utanför Min uppgraderade arbetsyta. Kan jag omvandla dem toohello nytt sökspråk automatiskt?
Du kan använda hello språk konverteringsverktyget i hello loggen Sök sidan tooconvert var och en.  Det finns ingen metod tooautomatically konvertera flera sökningar utan att uppgradera hello arbetsytan.

### <a name="question-why-are-my-query-results-not-sorted"></a>Fråga: Varför min frågeresultaten sorteras inte?
Resultaten sorteras inte som standard i nya hello-frågespråket.  Använd hello [sort-operator](https://go.microsoft.com/fwlink/?linkid=856079) toosort resultaten av en eller flera egenskaper.

### <a name="known-issue-search-results-in-a-list-may-include-properties-with-no-data"></a>Kända problem: sökresultat i en lista kan innehålla egenskaper utan data
Loggen sökresultat i en lista kan visa egenskaper utan data.  Tidigare tooupgrade egenskaperna skulle ingå.  Det här problemet korrigeras så att tomma egenskaper inte visas.

### <a name="known-issue-selecting-a-value-in-a-chart-doesnt-display-detailed-results"></a>Kända problem: du väljer ett värde i ett diagram visas inte detaljerade resultat
Tidigare tooupgrade när du har markerat ett värde i ett diagram det returnerar en detaljerad lista över poster som matchar hello valt värde.  Efter uppgraderingen returneras endast hello sammanfattade rad.  Det här problemet undersöks för närvarande.

## <a name="log-search-api"></a>Loggsöknings-API

### <a name="question-does-hello-log-search-api-get-updated-after-i-upgrade"></a>Fråga: Hello loggen Sök API uppdateras efter uppgraderingen?
Hej [loggen Sök API](log-analytics-log-search-api.md) ännu inte har uppgraderade toohello nytt sökspråk.  Fortsätta toouse hello äldre frågespråket med den här API: et, även när du har uppgraderat din arbetsyta.  Uppdaterad dokumentation blir tillgängliga för hello loggen Sök-API när den uppdateras.


## <a name="portals"></a>Portaler

### <a name="question-should-i-use-hello-new-advanced-analytics-portal-or-keep-using-hello-log-search-portal"></a>Fråga: Ska jag använda hello nya Advanced Analytics-portalen eller Fortsätt att använda hello loggen Sök portal?
Du kan se en jämförelse av hello två portaler på [portaler för att skapa och redigera loggen frågor i Azure Log Analytics](log-analytics-log-search-portals.md).  Varje har fördelar så att du kan välja hello som passar dina behov bäst.  Det är vanliga toowrite frågor i hello Advanced Analytics-portalen och klistra in dem i andra platser, till exempel View Designer.  Du bör du läsa om [utfärdar tooconsider](log-analytics-log-search-portals.md#advanced-analytics-portal) när gör.


## <a name="power-bi"></a>Power BI

### <a name="question-does-anything-change-with-powerbi-integration"></a>Fråga: Ändras något med PowerBI-integration?
Ja.  När ditt arbetsområde har uppgraderats fungerar inte längre hello processen för att exportera logganalys data tooPower BI.  Alla befintliga scheman som du skapade innan du uppgraderar inaktiveras.  Efter uppgraderingen hello Azure Log Analytics använder samma plattform som Application Insights och du använder samma process tooexport logganalys frågor tooPower BI som hello [hello processen tooexport Application Insights frågar tooPower BI](../application-insights/app-insights-export-power-bi.md#export-analytics-queries).

### <a name="known-issue-power-bi-request-size-limit"></a>Kända problem: storleksgränsen för Powerbi-begäran
Det finns en maximal storlek på 8 MB för en Log Analytics-fråga som kan vara exporterade tooPower BI.  Den här gränsen ska ökas snart.


##<a name="powershell-cmdlets"></a>PowerShell-cmdletar

### <a name="question-does-hello-log-search-powershell-cmdlet-get-updated-after-i-upgrade"></a>Fråga: Hello loggen Sök PowerShell-cmdleten uppdateras efter uppgraderingen?
Hej [Get-AzureRmOperationalInsightsSearchResults](https://docs.microsoft.com/powershell/module/azurerm.operationalinsights/Get-AzureRmOperationalInsightsSearchResults) ännu inte har uppgraderade toohello nytt sökspråk.  Fortsätt toouse hello äldre frågespråket med denna cmdlet, även när du har uppgraderat din arbetsyta.  Uppdaterad dokumentation ska vara tillgänglig för hello cmdlet när den uppdateras.


## <a name="resource-manager-templates"></a>Mallar för Resurshanteraren

### <a name="question-can-i-create-an-upgraded-workspace-with-a-resource-manager-template"></a>Fråga: Kan jag skapa en uppgraderad arbetsyta med en Resource Manager-mall?
Ja.  Du måste använda en API-version av 2017-03-15-preview och inkluderar en **funktioner** avsnitt i mallen som i följande exempel hello.

    "resources": [
        {
            "type": "Microsoft.OperationalInsights/workspaces",
            "apiVersion": "2017-03-15-preview",
            "name": "[parameters('workspaceName')]",
            "location": "[parameters('workspaceRegion')]",
            "properties": {
                "sku": {
                    "name": "Free"
                },
                "features": {
                    "legacy": 0,
                    "searchVersion": 1
                }
            }
        }
    ],



## <a name="solutions"></a>Lösningar

### <a name="question-will-my-solutions-continue-toowork"></a>Fråga: Min lösningar fortsätter toowork?
Alla lösningar fortsätter toowork i en uppgraderad arbetsyta, även om deras prestanda förbättras om de konverterade toohello nya frågespråk.  Det finns kända problem med vissa befintliga lösningar som beskrivs i det här avsnittet.

### <a name="known-issue-capacity-and-performance-solution"></a>Kända problem: Kapacitets- och lösning
Vissa av hello delar i hello [kapacitet och prestanda](log-analytics-capacity.md) vyn kan vara tom.  En korrigering toothis problemet kommer snart att vara tillgänglig.

### <a name="known-issue-device-health-solution"></a>Kända problem: lösning för enhetens hälsotillstånd
Hej [enhetens hälsotillstånd lösningen](https://docs.microsoft.com/windows/deployment/update/device-health-monitor) kommer inte att samla in data i en uppgraderad arbetsyta.  En korrigering toothis problemet kommer snart att vara tillgänglig.

### <a name="known-issue-application-insights-connector"></a>Kända problem: Application Insights connector
Perspektiv i [Application Insights Connector lösning](log-analytics-app-insights-connector.md) stöds inte för närvarande i en uppgraderad arbetsyta.  En korrigering toothis problemet ligger under analys.

## <a name="upgrade-process"></a>Uppgraderingsprocessen

### <a name="question-i-have-several-workspaces-can-i-upgrade-all-workspaces-at-hello-same-time"></a>Fråga: Jag har flera arbetsytor. Kan jag uppgradera alla arbetsytor på hello samtidigt?  
Nej.  Uppgraderingen gäller tooa enda arbetsytan varje gång. Det finns för närvarande inget sätt att samtidigt uppgradera många arbetsytor. Observera att andra användare av hello uppgraderas arbetsytan kommer att påverkas också.  

### <a name="question-will-existing-log-data-collected-in-my-workspace-be-modified-if-i-upgrade"></a>Fråga: Befintliga loggdata som samlas in i Min arbetsyta ändras om jag uppgraderar?  
Nej. hello logga data tillgängliga tooyour arbetsytan sökningar påverkas inte av hello uppgradering. Sparade sökningar, kommer aviseringar, vyer och att konvertera toohello nytt sökspråk automatiskt.  

### <a name="question-what-happens-if-i-dont-upgrade-my-workspace"></a>Fråga: Vad händer om jag inte uppgradera Min arbetsyta?  
hello äldre loggen sökningen kommer att inaktualiseras i hello kommer månader. Arbetsytor som inte uppgraderas av den tiden kommer att uppgraderas automatiskt.

### <a name="question-i-didnt-choose-tooupgrade-but-my-workspace-has-been-upgraded-anyway-what-happened"></a>Fråga: Det gick inte att välja tooupgrade men Min arbetsyta har uppgraderats ändå! Vad hände?  
En annan administratör på den här arbetsytan kan har uppgraderat hello arbetsytan. Observera att alla arbetsytor uppgraderas automatiskt när hello nytt språk når allmän tillgänglighet.  

### <a name="question-i-have-upgraded-by-mistake-and-now-need-toocancel-it-and-restore-everything-back-what-should-i-do"></a>Fråga: Jag har uppgraderat av misstag och nu behöver toocancel den och Återställ allt tillbaka. Vad ska jag göra?  
Inga problem.  Vi skapa en ögonblicksbild av arbetsytan innan uppgraderingen, så kan du återställa den. Tänk på att söker aviseringar eller vyer som du sparade när hello uppgraderingen kommer att gå förlorade om.  toorestore arbetsytan miljön, följ hello proceduren på [kan det gå tillbaka efter uppgraderingen?](log-analytics-log-search-upgrade.md#can-i-go-back-after-i-upgrade)


## <a name="views"></a>Vyer

### <a name="question-how-do-i-create-a-new-view-with-view-designer"></a>Fråga: Hur skapar jag en ny vy med Vydesigner?
Tidigare tooupgrade du kan skapa en ny vy med Vydesigner från en panel i hello huvudinstrumentpanelen.  När din arbetsyta uppgraderas tas den här panelen bort.  Du kan skapa en ny vy med Vydesigner i hello OMS-portalen genom att klicka på hello grön + knappen hello vänstra menyn.

### <a name="known-issue-see-all-option-for-line-charts-in-views-doesnt-result-in-a-line-chart"></a>Kända problem: se alla alternativ för linjediagram i vyer inte resulterar i ett linjediagram
Om du klickar på hello *se alla* alternativet längst ned hello i en rad diagramdel i en vy, visas en tabell.  Tidigare tooupgrade skulle du visas med ett linjediagram.  Det här problemet analyseras för eventuella ändringar.


## <a name="next-steps"></a>Nästa steg

- Lär dig mer om [uppgradera din arbetsyta toohello nya Log Analytics-frågespråket](log-analytics-log-search-upgrade.md).
