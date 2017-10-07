---
title: aaaIntegrating med Operations Management Suite (OMS) | Microsoft Docs
description: "Dessutom toousing Hej standardfunktioner i OMS, integrera det med andra hantering av program och tjänster tooprovide en hybridmiljö för hantering, tooprovide anpassade scenarier unika tooyour miljö eller tooprovide en anpassad hanteringsupplevelse för kunderna.  Den här artikeln innehåller en översikt över olika alternativ för att integrera med OMS och länkar tooarticles detaljerad teknisk information."
services: operations-management-suite
documentationcenter: 
author: bwren
manager: jwhit
editor: tysonn
ms.assetid: fc5f3a8a-77f7-4103-bd7e-744c15ffcca7
ms.service: operations-management-suite
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 04/11/2017
ms.author: bwren
ms.openlocfilehash: dce752dcdc6c725bbafd49db4a5055750487ecf9
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="integrating-with-operations-management-suite-oms"></a>Integrera med Operations Management Suite (OMS)
Operations Management Suite är Microsofts molnbaserade IT lösning som hjälper dig att hantera och skydda dina lokala och molnet infrastruktur.  Dessutom toousing Hej standardfunktioner i OMS, integrera det med andra hantering av program och tjänster tooprovide en hybridmiljö för hantering, tooprovide anpassade scenarier unika tooyour miljö eller tooprovide en anpassad hanteringsupplevelse för kunderna.  Den här artikeln innehåller en översikt över olika alternativ för att integrera med OMS tjänster och länkar tooarticles detaljerad teknisk information. 

## <a name="log-analytics"></a>Log Analytics
Av hanteringsdata som samlas in av logganalys lagras i en databas som finns i Azure.  Alla data som lagras i hello-databasen finns i loggen sökningar som ger snabb analys över mycket stora mängder data.  Din integreringskraven kanske toopopulate hello-databas med nya data, gör den tillgänglig för analys, eller tooextract data i hello databasen tooprovide nya visualiseringen eller toointegrate med ett annat hanteringsverktyg.

Varje datablock i hello databasen lagras som en post.  När du anger hello databasen bör du ge användare med hello posttyp som använder din lösning och en beskrivning av dess egenskaper.  När du hämtar data, måste den här informationen om hello data du arbetar med.

![Fylla hello OMS-databasen](media/operations-management-suite-integration/repository.png)

### <a name="populate-hello-log-analytics-repository"></a>Fylla hello logganalys databasen
Det finns flera metoder för att fylla hello OMS-databasen.  hello metod du använder beror på faktorer som där hello källdata finns, hello format hello data och vilka klienter som du behöver toosupport.  När data lagras i databasen hello spelar ingen roll som hur den samlats in.

hello beskrivs följande avsnitt hello olika alternativ för att fylla hello OMS-databasen.

#### <a name="connected-sources-and-data-sources"></a>Anslutna källor och datakällor
Anslutna källor är hello platser där du kan hämta data för hello OMS-databasen.  Datakällor och lösningar körs på anslutna källor och definiera hello specifika data som samlas in.  Om ditt program skriver data tooone för dessa datakällor, samla du den genom att konfigurera hello-datakälla.  Till exempel om ditt program skapar Syslog-händelser, kan sedan de samlas in av hello Syslog-datakälla på en Linux-agenten.

* [Datakällor i logganalys](../log-analytics/log-analytics-data-sources.md)

#### <a name="solutions"></a>Lösningar
Lösningar utöka hello funktionerna i OMS.  En lösning som kan samla in data från hello anslutna källa eller den kan utföra analyser på poster som redan samlats in i hello-databas.  Varje lösning som tillhandahålls av Microsoft har en enskild artikel som innehåller hello information om hello data som den samlar in.

* [Lösningar i logganalys](../log-analytics/log-analytics-add-solutions.md)

#### <a name="http-data-collector-api"></a>HTTP-datainsamlaren API
hello Log Analytics HTTP Data Collector API är en REST-API som gör att du tooadd JSON data toohello logganalys databasen.  Du kan utnyttja detta API när du har ett program som inte ger data via en hello andra datakällor eller lösningar.  Det kan vara används toopopulate hello databasen från klienter som kan anropa hello API och inte är beroende av hello samling schemat för en datakälla eller en lösning.

* [Log Analytics HTTP datainsamlaren API](../log-analytics/log-analytics-data-collector-api.md)

### <a name="retrieve-data-from-hello-log-analytics-repository"></a>Hämta data från hello Log Analytics-databas
Det finns flera metoder för att hämta data från hello OMS-databasen.  Du kanske vill användare tooretrieve data med hjälp av hello OMS-konsolen och ge dem med olika typer av grafik och analys.  Du kan också hämta hello data från en extern process, till exempel en annan lösning.

#### <a name="log-searches"></a>Log-sökningar
Alla data som lagras i hello OMS-databasen är tillgänglig via loggen sökningar.  Användare kan utföra sina egna ad hoc-analyser i hello OMS-konsolen eller skapa en instrumentpanel med en visualisering för en viss logg-sökning.  Lösningar kan innehålla anpassade vyer med grafik baserad på fördefinierade sökningar.  Du kan använda hello loggen Sök API tooaccess data i hello OMS-databas från ett externt program eller hantering verktyg.  

* [Loggen söker i logganalys](../log-analytics/log-analytics-log-searches.md)
* [Logganalys logga Sök REST API](../log-analytics/log-analytics-log-search-api.md)
* [Log Analytics-cmdlets](https://msdn.microsoft.com/library/mt188224.aspx)

#### <a name="custom-views"></a>Anpassade vyer
hello Vydesigner kan toocreate anpassade vyer i hello OMS-konsolen som ger användare visualisering och dataanalys hello i din lösning.  Varje vy innehåller en ikon som visas på huvudsidan för hello hello-konsolen och valfritt antal visualiseringen delar som är baserade på loggen sökningar som du definierar.

* [Log Analytics Vydesigner](../log-analytics/log-analytics-view-designer.md)

#### <a name="power-bi"></a>Power BI
Logganalys exportera automatiskt data från hello OMS-databasen till Power BI så att du kan utnyttja dess visualiseringar och verktyg för analys.  Den här exporten utförs enligt ett schema så hello data bevaras in toodate. 

* [Exportera logganalys data tooPower BI](../log-analytics/log-analytics-powerbi.md)

## <a name="automation"></a>Automation
OMS automatisera processer tooreact toocollected data eller tooperform andra hanteringsfunktioner.  Kan samla in data från ditt program och infoga i hello OMS-databasen eller du kan automatisera hello korrigering av ett känt problem i svaret toodata hittades i hello-databasen. 

![Automation](media/operations-management-suite-integration/automate.png)

### <a name="runbooks"></a>Runbooks
Runbooks i Azure Automation Kör PowerShell-skript och arbetsflöden i hello Azure-molnet.  Du kan använda dem toomanage i Azure eller andra resurser som kan nås från hello molnet.  Runbooks kan också köras i ett lokalt datacenter med hjälp av Hybrid Runbook Worker.  Du kan starta en runbook från hello Azure-portalen eller externa processer med hjälp av ett antal metoder som PowerShell eller hello Automation-API.

* [Starta en runbook i Azure Automation](../automation/automation-starting-a-runbook.md)
* [Azure Automation-cmdlets](https://msdn.microsoft.com/library/dn690262.aspx)
* [Automation REST API](https://msdn.microsoft.com/library/mt662285.aspx)
* [Automation .NET](https://msdn.microsoft.com//library/mt465763.aspx)

### <a name="alerts"></a>Aviseringar
Varningsregler kör automatiskt logga sökningar enligt tooa schema.  Om hello resultat som matchar särskilda villkor hello resulterande avisering startar en runbook i Azure Automation eller anropa en webhook som kan starta en extern process.  Båda dessa svar kan innehålla information om hello aviseringen, inklusive hello data som returneras i hello loggen sökning.

* [Aviseringar i logganalys](../log-analytics/log-analytics-alerts.md)
* [Log Analytics avisering API](../log-analytics/log-analytics-api-alerts.md)

## <a name="backup-and-site-recovery"></a>Säkerhetskopiering och återställning
Azure Backup och Site Recovery tillhandahåller tjänster för att skydda företagets data och säkerställa hello tillgängligheten för servrar och program.  Du kan använda dessa tjänster tooperform sådana scenarier som erbjuder tjänster för säkerhetskopiering för ditt program eller initiera en redundansväxling för en virtuell dator.

* [Azure Backup-cmdletar](https://msdn.microsoft.com/library/mt619253.aspx)
* [Azure Site Recovery REST-API](https://msdn.microsoft.com/library/azure/mt750497.aspx)
* [Azure Site Recovery-Cmdlets](https://msdn.microsoft.com/library/mt637930.aspx)

## <a name="custom-solutions"></a>Anpassade lösningar
Du kan kapsla integration logik i en anpassad lösning toorun i din arbetsyta eller i en kund arbetsytan.  Din lösning kan innehålla flera hello integration metoder i den här artikeln i tillägg tooother resurser tooprovide en fullständig hanteringsscenario.  hello resurser i hello lösning paketeras så att när hello lösningen tas bort alla hello resurser som den skapades tas bort från hello OMS-arbetsytan och Azure-prenumeration.

Din lösning kan inkludera en Automation runbook toogather och bearbeta data och fylla hello logganalys-databas med hjälp av hello HTTP Data Collector API: et.  Du kan också inkludera en anpassad vy som visar och analyserar hello insamlade data.  

* Skapa anpassade lösningar (kommer snart)    

## <a name="next-steps"></a>Nästa steg
* Referens hello [OMS SDK](operations-management-suite-sdk.md) teknisk information om automatisering OMS-tjänster.  

