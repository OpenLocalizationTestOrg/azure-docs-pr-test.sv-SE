---
title: "aaaMigrating från Orchestrator tooAzure Automation | Microsoft Docs"
description: "Beskriver hur toomigrate runbooks och integrering hanteringspaketen från System Center Orchestrator tooAzure Automation."
services: automation
documentationcenter: 
author: bwren
manager: stevenka
editor: tysonn
ms.assetid: 1a7da58c-7a98-49b5-9d9d-001a9f6e631a
ms.service: automation
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/09/2016
ms.author: bwren
ms.openlocfilehash: 797b50067ef2aa68470760e99d494b89ab7baacf
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="migrating-from-orchestrator-tooazure-automation-beta"></a>Migrera från Orchestrator tooAzure Automation (Beta)
Runbooks i [System Center Orchestrator](http://technet.microsoft.com/library/hh237242.aspx) baseras på aktiviteter från integrationspaketen som är skrivna specifikt för Orchestrator medan runbooks i Azure Automation baseras på Windows PowerShell.  [Grafiska runbook-flöden](automation-runbook-types.md#graphical-runbooks) har en liknande utseende tooOrchestrator runbooks med deras aktiviteter som representerar PowerShell cmdlets, underordnade runbooks och tillgångar i Azure Automation.

Hej [System Center Orchestrator Migration Toolkit](http://www.microsoft.com/download/details.aspx?id=47323&WT.mc_id=rss_alldownloads_all) innehåller verktyg tooassist du konverterar runbooks från Orchestrator tooAzure Automation.  Dessutom tooconverting hello runbooks själva, måste du konvertera hello integreringspaket med hello aktiviteter att hello runbooks använder toointegration moduler med Windows PowerShell-cmdlets.  

Följande är hello grundläggande process för att konvertera Orchestrator runbooks tooAzure Automation.  Var och en av dessa steg beskrivs i detalj i hello avsnitten nedan.

1. Hämta hello [System Center Orchestrator Migration Toolkit](http://www.microsoft.com/download/details.aspx?id=47323&WT.mc_id=rss_alldownloads_all) som innehåller hello verktyg och moduler som beskrivs i den här artikeln.
2. Importera [standardaktiviteter modulen](#standard-activities-module) i Azure Automation.  Detta inkluderar konverterade versioner av standard Orchestrator-aktiviteter som kan användas av konverterade runbooks.
3. Importera [integreringsmoduler för System Center Orchestrator](#system-center-orchestrator-integration-modules) i Azure Automation för dessa integreringspaket som används av runbooks som har åtkomst till System Center.
4. Konvertera anpassade och tredjeparts integrationspaket med hello [Integration Pack konverteraren](#integration-pack-converter) och importera till Azure Automation.
5. Konvertera Orchestrator runbooks med hello [Runbook konverteraren](#runbook-converter) och installera i Azure Automation.
6. Manuellt skapa nödvändiga Orchestrator tillgångar i Azure Automation eftersom hello Runbook konverteraren inte konverterar dessa resurser.
7. Konfigurera en [Hybrid Runbook Worker](#hybrid-runbook-worker) i dina lokala data center toorun konverteras runbooks som får åtkomst till lokala resurser.

## <a name="service-management-automation"></a>Service Management Automation
[Service Management Automation](http://technet.microsoft.com/library/dn469260.aspx) (SMA) lagrar och kör runbooks i ditt lokala datacenter som Orchestrator och hello använder samma integreringsmoduler som Azure Automation. Hej [Runbook konverteraren](#runbook-converter) konverterar Orchestrator runbooks toographical runbooks men som inte stöds i SMA.  Du kan fortfarande installera hello [Standard aktiviteter modulen](#standard-activities-module) och [integreringsmoduler för System Center Orchestrator](#system-center-orchestrator-integration-modules) till SMA, men du måste manuellt [Skriv om dina runbooks](http://technet.microsoft.com/library/dn469262.aspx).

## <a name="hybrid-runbook-worker"></a>Hybrid Runbook Worker
Runbooks i Orchestrator lagras på en databasserver och körs på runbook-servrar, både i ditt lokala datacenter.  Runbooks i Azure Automation lagras i hello Azure-molnet och kan köras i din lokala data center genom att använda en [Hybrid Runbook Worker](automation-hybrid-runbook-worker.md).  Detta är hur kommer du vanligtvis köra runbooks konverteras från Orchestrator eftersom de är utformad toorun på lokala servrar.

## <a name="integration-pack-converter"></a>Integration Pack konverterare
hello Integration Pack Konverteraren konverterar integreringspaket som har skapats med hello [Orchestrator Integration Toolkit (OIT)](http://technet.microsoft.com/library/hh855853.aspx) toointegration moduler baserat på Windows PowerShell som kan importeras till Azure Automation eller Service Management Automation.  

När du kör hello Integration Pack konverterare visas med en guide som gör att du tooselect en integrationspaketfilen (.oip).  hello guiden visar hello aktiviteter som ingår i paketet och låter dig tooselect som ska migreras.  När du har slutfört hello guiden skapar en integrationsmodul som innehåller en motsvarande cmdlet för varje hello aktiviteter i hello ursprungliga integreringspaketet.

### <a name="parameters"></a>Parametrar
Alla egenskaper för en aktivitet i hello integration pack är konverterade tooparameters för hello motsvarande cmdlet i hello integrationsmodul.  Windows PowerShell-cmdlets har en uppsättning [gemensamma parametrar](http://technet.microsoft.com/library/hh847884.aspx) som kan användas med alla cmdletar.  Till exempel hello - Verbose parametern orsakar en cmdlet toooutput detaljerad information om driften.  Inga cmdlet kan ha en parameter med hello samma namn som en gemensam parameter.  Om en aktivitet har en egenskap med hello samma namn som en gemensam parameter, uppmanas hello du tooprovide ett annat namn för hello-parametern.

### <a name="monitor-activities"></a>Övervakaraktiviteter
Övervaka runbooks i Orchestrator starta med en [övervaka aktiviteten](http://technet.microsoft.com/library/hh403827.aspx) och kör kontinuerligt väntar toobe anropas av en viss händelse.  Azure Automation stöder inte övervaka runbooks, så alla övervakaraktiviteter i hello integration pack inte kommer att konverteras.  I stället skapas en platshållare cmdlet i hello integrationsmodul för hello övervakaraktiviteten.  Denna cmdlet har inga funktioner, men det gör att alla konverterade runbook som använder den toobe installerad.  Denna runbook inte kan toorun i Azure Automation, men den kan installeras så att du kan ändra den.

### <a name="integration-packs-that-cannot-be-converted"></a>Integreringspaket som inte kan konverteras
Integreringspaket som inte har skapats med OIT kan inte konverteras med hello Integration Pack konverteraren. Det finns även vissa integreringspaket som tillhandahålls av Microsoft som för närvarande inte kan konverteras med det här verktyget.  Konverterade versioner av integreringspaketen har [som finns att hämta](#system-center-orchestrator-integration-modules) så att de kan installeras i Azure Automation eller Service Management Automation.

## <a name="standard-activities-module"></a>Standardaktiviteter modul
Orchestrator innehåller en uppsättning [standardaktiviteter](http://technet.microsoft.com/library/hh403832.aspx) som inte ingår i ett integreringspaket men används av många runbooks.  hello standardaktiviteter modul är en modul för integrering med en motsvarande cmdlet för var och en av dessa aktiviteter.  Innan du importerar alla konverterade runbooks som använder en aktivitet som standard måste du installera den här integrationsmodul i Azure Automation.

Dessutom toosupporting konverteras runbooks, hello-cmdlets i hello standardaktiviteter modul kan användas av någon som är bekant med Orchestrator toobuild nya runbooks i Azure Automation.  Medan hello funktionalitet i hello standardaktiviteter kan utföras med cmdlet: ar, fungerar de på olika sätt.  hello hello-cmdletar i hello konverteras standardaktiviteter modulen fungerar samma som deras motsvarande aktiviteter och Använd hello samma parametrar.  Detta kan hjälpa hello befintliga Orchestrator runbook-redigerare i sina övergången tooAzure Automation-runbooks.

## <a name="system-center-orchestrator-integration-modules"></a>System Center Orchestrator integreringsmoduler
Microsoft tillhandahåller [integreringspaket](http://technet.microsoft.com/library/hh295851.aspx) för att bygga runbooks tooautomate System Center-komponenter och andra produkter.  Vissa av dessa integreringspaket för närvarande är baserade på OIT men kan för närvarande inte konverterade toointegration moduler på grund av kända problem.  [System Center Orchestrator integreringsmoduler](https://www.microsoft.com/download/details.aspx?id=49555) innehåller konverterade versioner av dessa integreringspaket som kan importeras till Azure Automation och Service Management Automation.  

Uppdaterade versioner av hello integreringspaket baserat på OIT som kan konverteras med hello Integration Pack konverteraren kommer att publiceras av hello RTM-versionen av det här verktyget.  Vägledning ges också tooassist du konverterar runbooks med aktiviteter från integrationspaketen hello inte baserat på OIT.

## <a name="runbook-converter"></a>Runbook-konverteraren
Hej Runbook Konverteraren konverterar Orchestrator runbooks till [grafiska runbook-flöden](automation-runbook-types.md#graphical-runbooks) som kan importeras till Azure Automation.  

Runbook-konverteraren implementeras som en PowerShell-modul med en cmdlet som kallas **ConvertFrom SCORunbook** som utför hello konvertering.  När du installerar hello verktyget skapas en genväg tooa PowerShell-session som läser in hello cmdlet.   

Följande är hello grundläggande processen tooconvert Orchestrator-runbook och importera den till Azure Automation.  hello följande avsnitt innehåller mer information om verktyget hello och arbetar med konverterade runbooks.

1. Exportera en eller flera runbooks från Orchestrator.
2. Hämta integreringsmoduler för alla aktiviteter i hello runbook.
3. Konvertera hello Orchestrator runbooks i hello exporterade filen.
4. Granska informationen i loggarna toovalidate hello konvertering och toodetermine alla manuella uppgifter som krävs.
5. Importera konverterade runbooks i Azure Automation.
6. Skapa alla nödvändiga resurser i Azure Automation.
7. Redigera hello runbook i Azure Automation toomodify alla aktiviteter som krävs.

### <a name="using-runbook-converter"></a>Med Runbook-konverteraren
Hej syntax för **ConvertFrom SCORunbook** är följande:

    ConvertFrom-SCORunbook -RunbookPath <string> -Module <string[]> -OutputFolder <string>

* RunbookPath - sökvägen toohello export-fil som innehåller hello runbooks tooconvert.
* Modul - kommaavgränsad lista över integreringsmoduler som innehåller aktiviteter i hello runbooks.
* OutputFolder - sökvägen toohello mappen toocreate konverteras grafiska runbook-flöden.

följande exempelkommando konverterar hello runbooks i en export-fil som kallas hello **MyRunbooks.ois_export**.  Dessa runbooks använder hello Active Directory och integreringspaket för Data Protection Manager.

    ConvertFrom-SCORunbook -RunbookPath "c:\runbooks\MyRunbooks.ois_export" -Module c:\ip\SystemCenter_IntegrationModule_ActiveDirectory.zip,c:\ip\SystemCenter_IntegrationModule_DPM.zip -OutputFolder "c:\runbooks"


### <a name="log-files"></a>Loggfiler
Hej Runbook konverteraren skapar hello följande loggfiler i hello samma plats som hello konverteras runbook.  Om hello filerna redan finns, skrivs sedan de över med informationen från hello senaste konvertering.

| Fil | Innehåll |
|:--- |:--- |
| Runbook-konverteraren - Progress.log |Detaljerade anvisningar för hello konvertering, inklusive information om varje aktivitet har konverterats och varning för varje aktivitet som inte konverteras. |
| Runbook-konverteraren - Summary.log |Sammanfattning av hello senaste konvertering, inklusive alla varningar och följa upp aktiviteter som du behöver tooperform, till exempel skapa en variabel som krävs för hello konverteras runbook. |

### <a name="exporting-runbooks-from-orchestrator"></a>Exportera runbooks från Orchestrator
Hej Runbook konverteraren fungerar med en exportfil från Orchestrator som innehåller en eller flera runbooks.  Den skapar en motsvarande Azure Automation-runbook för varje runbook i Orchestrator i hello exportfilen.  

tooexport en runbook från Orchestrator, högerklicka på hello namnet på hello runbook i Runbook Designer och välj **exportera**.  tooexport alla runbooks i en mapp, högerklickar du på hello och välj hello namnet **exportera**.

### <a name="runbook-activities"></a>Runbook-aktiviteter
Hej Runbook Konverteraren konverterar varje aktivitet i hello Orchestrator runbook tooa motsvarande aktivitet i Azure Automation.  För de aktiviteter som inte kan konverteras, skapas en platshållare för aktiviteten i hello runbook med varningstext.  När du har importerat hello konverteras runbook till Azure Automation måste du ersätta någon av dessa aktiviteter med giltig aktiviteter som utför hello krävs funktioner.

Alla Orchestrator-aktiviteter i hello [Standard aktiviteter modulen](#standard-activities-module) ska konverteras.  Det finns vissa standard Orchestrator-aktiviteter som inte ingår i den här modulen men och konverteras inte.  Till exempel **skicka Plattformshändelsen** inte har något Azure Automation-motsvarande eftersom hello händelse är specifik tooOrchestrator.

[Övervaka aktiviteter](https://technet.microsoft.com/library/hh403827.aspx) konverteras inte eftersom det inte finns några motsvarande toothem i Azure Automation.  hello undantag är övervakaren aktiviteter i [konverteras integreringspaket](#integration-pack-converter) som ska vara konverterade toohello platshållare för aktiviteten.

Alla aktiviteter från en [konverteras integreringspaketet](#integration-pack-converter) konverteras om du tillhandahåller hello sökvägen toohello integrationsmodul hello **moduler** parameter.  För System Center-integreringspaket, kan du använda hello [integreringsmoduler för System Center Orchestrator](#system-center-orchestrator-integration-modules).

### <a name="orchestrator-resources"></a>Orchestrator-resurser
Hej Runbook Konverteraren konverterar enbart runbooks, inte andra informationskällor för Orchestrator, till exempel räknare, variabler eller anslutningar.  Räknare stöds inte i Azure Automation.  Variabler och anslutningar stöds, men du måste skapa dem manuellt.  hello loggfiler ska får du information om hello runbook kräver sådana resurser och ange motsvarande resurser som du behöver toocreate i Azure Automation för hello konverteras runbook toooperate korrekt.

En runbook kan till exempel använda en variabel toopopulate ett visst värde i en aktivitet.  hello konverterade runbook konverterar hello aktivitet och ange en variabel tillgång i Azure Automation med samma namn som hello Orchestrator variabel hello.  Detta kommer att märka i hello **Runbook konverteraren - Summary.log** filen som skapas efter hello konverteringen.  Du måste skapa toomanually variabeln tillgången i Azure Automation innan du använder hello runbook.

### <a name="input-parameters"></a>Indataparametrar
Runbooks i Orchestrator godkänna indataparametrar med hello **initiera Data** aktivitet.  Om hello runbook omvandlas innehåller den här aktiviteten och sedan en [Indataparametern](automation-graphical-authoring-intro.md#runbook-input-and-output) i hello Azure Automation runbook skapas för varje parameter i hello-aktivitet.  En [Arbetsflödesskriptet kontrollen](automation-graphical-authoring-intro.md#activities) aktivitet skapas i hello konverteras runbook som hämtar och returnerar varje parameter.  Aktiviteter i hello runbook som använder en indataparameter finns toohello utdata från den här aktiviteten.

hello beror att den här strategin används på toobest spegling hello funktioner i hello Orchestrator runbook.  Aktiviteter i nya grafiska runbook-flöden ska hänvisa direkt tooinput parametrar med en källa för Runbook-indata.

### <a name="invoke-runbook-activity"></a>Aktiviteten anropa Runbook
Runbooks i Orchestrator starta andra runbooks med hello **anropa Runbook** aktivitet. Om hello runbook omvandlas inkluderar denna aktivitet och hello **vänta på slutförande** alternativet anges och sedan en runbook-aktivitet skapas för den i hello konverteras runbook.  Om hello **vänta på slutförande** alternativet inte anges, och sedan en Arbetsflödesskriptet aktivitet skapas som använder **Start AzureAutomationRunbook** toostart hello runbook.  När du har importerat hello konverteras runbook till Azure Automation måste du ändra den här aktiviteten med hello information som anges i hello-aktivitet.

## <a name="related-articles"></a>Relaterade artiklar
* [System Center 2012 – Orchestrator](http://technet.microsoft.com/library/hh237242.aspx)
* [Service Management Automation](https://technet.microsoft.com/library/dn469260.aspx)
* [Runbook Worker-hybrid](automation-hybrid-runbook-worker.md)
* [Standardaktiviteter för orchestrator](http://technet.microsoft.com/library/hh403832.aspx)
* [Hämta System Center Orchestrator Migration Toolkit](https://www.microsoft.com/en-us/download/details.aspx?id=47323)
