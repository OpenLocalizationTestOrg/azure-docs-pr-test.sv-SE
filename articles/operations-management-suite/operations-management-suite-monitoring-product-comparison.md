---
title: "aaaMicrosoft övervakning Produktjämförelse | Microsoft Docs"
description: "Microsoft Operations Management Suite (OMS) är Microsofts molnbaserade IT-hanteringslösning som hjälper dig att hantera och skydda dina lokala och molnet infrastruktur.  Den här artikeln anger hello olika tjänster som ingår i OMS och innehåller länkar tootheir detaljerad innehåll."
services: operations-management-suite
documentationcenter: 
author: bwren
manager: jwhit
editor: tysonn
ms.assetid: a63ca0ad-61f8-425d-a48c-d87ba518c104
ms.service: operations-management-suite
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 10/27/2016
ms.author: bwren
ms.openlocfilehash: 61144a298fe73c35181070d552c41b96fc445097
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="microsoft-monitoring-product-comparison"></a>Microsoft övervakning Produktjämförelse
Den här artikeln innehåller en jämförelse mellan System Center Operations Manager (SCOM) och logganalys i Operations Management Suite (OMS) i sina arkitektur, hello logiken för hur de övervaka resurser och hur de utför dataanalys hello de samla in.  Detta är toogive du en grundläggande förståelse av deras skillnader och relativa styrka.  

## <a name="basic-architecture"></a>Grundläggande arkitektur
### <a name="system-center-operations-manager"></a>System Center Operations Manager
Alla installeras SCOM i ditt datacenter.  [Agenter är installerade](http://technet.microsoft.com/library/hh551142.aspx) på Windows- och Linux-datorer som hanteras av SCOM.  Agenter för ansluta[hanteringsservrar](https://technet.microsoft.com/library/hh301922.aspx) som kommunicerar med hello SCOM-driftdatabasen och datalagret.  Agenter som förlitar sig på domänen autentiseringsservrar tooconnect toomanagement.  De utanför en betrodd domän kan utföra autentisering med datorcertifikat eller ansluta tooa [Gateway-servern](https://technet.microsoft.com/library/hh212823.aspx).

SCOM kräver två SQL-databaser, ett för användningsdata och en annan data warehouse toosupport rapportering och dataanalys.  En [rapportservern](https://technet.microsoft.com/library/hh298611.aspx) körs SQL Reporting Services tooreport på data från hello-datalagret. 

SCOM kan övervaka molnresurser med hanteringspaket för produkter som [Azure](https://www.microsoft.com/download/details.aspx?id=38414), [Office 365](https://www.microsoft.com/download/details.aspx?id=43708), och [AWS](http://docs.aws.amazon.com/AWSEC2/latest/WindowsGuide/AWSManagementPack.html).  Dessa hanteringspaket använda en eller flera lokala agenter som proxyservrar för identifiering av molnbaserade resurser och kör arbetsflöden toomeasure deras prestanda och tillgänglighet.  Proxyagenter används också för[övervaka nätverksenheter](https://technet.microsoft.com/library/hh212935.aspx) och andra externa resurser.

hello är Operations-konsolen ett windowsprogram som ansluter tooone hello hanteringsservrar och tillåter Hej administratör tooview och analysera insamlade data och konfigurera hello SCOM-miljö.  En webbaserad konsol kan finnas på en IIS-server och ger dataanalys via en webbläsare.

![SCOM-arkitektur](media/operations-management-suite-monitoring-product-comparison/scom-architecture.png)

### <a name="log-analytics"></a>Log Analytics
De flesta OMS-komponenter finns i hello Azure-molnet så att du kan distribuera och hantera den med minimal kostnad och administrativt arbete.  Alla data som samlas in av logganalys lagras i hello OMS-databasen.

Logganalys kan samla in data från en av tre källor:

* Fysiska och virtuella datorer som kör Windows och hello [Microsoft Monitoring Agent (MMA)](https://technet.microsoft.com/library/mt484108.aspx) eller Linux- och hello [Operations Management Suite-Agent för Linux](https://technet.microsoft.com/library/mt622052.aspx).  Dessa datorer kan vara lokala eller virtuella datorer i Azure eller en annan molnet.
* Ett Azure Storage-konto med [Azure Diagnostics](../cloud-services/cloud-services-dotnet-diagnostics.md) data som samlas in av virtuell dator, webbroll eller Azure-arbetsroll.
* [Anslutningen tooa SCOM-hanteringsgrupp](https://technet.microsoft.com/library/mt484104.aspx).  I den här konfigurationen kommunicera hello agenter med SCOM-hanteringsservrar som levererar hello data toohello SCOM databas där den sedan levereras toohello OMS-datalagret.
  Administratörer analysera insamlade data och konfigurera logganalys med hello OMS-portalen som finns i Azure och kan nås från valfri webbläsare.  Mobila appar tooaccess dessa data är tillgängliga för hello standard plattformar.

![Log Analytics-arkitektur](media/operations-management-suite-monitoring-product-comparison/log-analytics-architecture.png)

### <a name="integrating-scom-and-log-analytics"></a>Integrera SCOM och logganalys
När SCOM används som datakälla för logganalys kan du använda hello funktioner för båda produkterna i ett hybridscenario övervakning miljö.  Du kan konfigurera befintliga SCOM agenter via hello Driftkonsolen toobe hanteras av OMS, dessutom toocontinuing toorun hanteringspaket från SCOM.  
Data från en ansluten SCOM-hanteringsgrupp levereras tooLog Analytics med hjälp av någon av fyra metoder:

* Händelserna och prestandadata samlas in av hello-agenten och leverera tooSCOM.  Hanteringsservrar i SCOM skickar hello data tooLog Analytics.
* Vissa händelser, till exempel IIS-loggar och säkerhetshändelser fortsätta toobe levereras direkt tooLog Analytics från hello agent.
* Vissa lösningar ska leverera tilläggsprogramvara toohello agent eller kräver att programvaran är installerad toocollect ytterligare data.  Dessa data vanligtvis skickas direkt tooLog Analytics.
* Vissa lösningar samlar in data direkt från SCOM-hanteringsservrar som inte kommer från hello agent.  Till exempel hello [avisering hanteringslösning](https://technet.microsoft.com/library/mt484092.aspx) samlar in varningar från SCOM när de har skapats.

## <a name="monitoring-logic"></a>Övervakningslogik
SCOM och logganalys arbeta med liknande data som samlas in från agenter har viktiga skillnader i hur de definiera och implementera sin logik för insamling av data och hur de analysera hello som de samlar in

### <a name="operations-manager"></a>Operations Manager
Övervakningslogiken för SCOM är implementerad i [hanteringspaket](https://technet.microsoft.com/library/hh457558.aspx) som innehåller logik för identifiering av komponenter toomonitor, mäta hello hälsotillståndet för dessa komponenter, och för att samla in data tooanalyze.  Övervakning av data kan vara så enkelt som att samla in en händelser och prestandaräknare eller den kan använda komplex logik som implementeras i ett skript.  Hanteringspaket som innehåller fullständig övervakning är tillgängliga för olika [program från Microsoft och tredje parter](http://go.microsoft.com/fwlink/?LinkId=82105) i tillägget toohardware och nätverksenheter.  Du kan [skapa egna hanteringspaket](http://aka.ms/mpauthor) för anpassade program.

Hanteringspaket innehåller flera olika arbetsflöden som alla utför vissa övervakning specifik funktion som provtagning en prestandaräknare, kontrollerar hello tillstånd för en tjänst eller köra ett skript.  Varje arbetsflöde körs enskilt och definierar egna resultat, till exempel vilken databas skrivs tooand om det skapar en avisering. 

Du kan åsidosätta detaljer om arbetsflödet, till exempel hello frekvens de kör hello tröskelvärde när de anser att ett fel och hello allvarlighetsgraden hello avisering de genererar.  Du kan också ange ytterligare funktioner genom att lägga till egna arbetsflöden.

![Åsidosättningar](media/operations-management-suite-monitoring-product-comparison/scom-overrides.png)

Hanteringspaket är installerade i hello Operations Manager-databasen och distribueras automatiskt tooagents av hanteringsservrar.  Varje agent hämta hanteringspaket automatiskt och läsa in arbetsflöden relevanta toohello program som de har installerats.  Data som samlas in av hello agenten levereras tillbaka toohello hanteringsservern för infogning i hello SCOM-driftdatabasen och datalagret.  hello Operations-konsolen kan du tooview och analysera data via anpassade vyer, instrumentpaneler och rapporter som ingår i hello management pack.

hello distribution av hanteringspaket illustreras i följande diagram hello.

![Management pack-flöde](media/operations-management-suite-monitoring-product-comparison/scom-mpflow.png)

### <a name="log-analytics"></a>Log Analytics
#### <a name="event-and-performance-collection"></a>Händelse- och insamling av prestanda
Logganalys samlar in händelser och prestandaräknare från agent-system som använder källor som Windows-händelseloggen, IIS-loggar och Syslog.  Du kan ange kriterier för vilka data samlas in via hello logganalys-portalen och sedan skapa loggen frågor tooanalyze hello insamlade data.  En uppsättning villkor som standard definieras när du skapar din OMS-arbetsyta och du kan ange ytterligare information om specifika program. 

![Definiera händelseloggar i logganalys](media/operations-management-suite-monitoring-product-comparison/log-analytics-definedata.png)

SCOM har många detaljerad arbetsflöden som vanligtvis definierar specifika kriterier för data och hello-åtgärd som ska utföras i svaret, har Log Analytics mer allmänna villkor för datainsamling.  Loggen frågor och lösningar ger mer riktade kriterier för att analysera och fungerar på specifika data i molnet hello när den har samlats in.

#### <a name="solutions"></a>Lösningar
Lösningar som erbjuder ytterligare logik för insamling och analys.  Du kan välja lösningar tooadd tooyour OMS prenumeration från hello lösning Gallery.

![Lösningar galleri](media/operations-management-suite-monitoring-product-comparison/log-analytics-solutiongallery.png)

Lösningar kör främst i hello moln tillhandahåller analys av händelser och prestandaräknare som samlats in i hello OMS-databas.  De kan också definiera ytterligare data toobe som samlas in och som kan analyseras med loggen frågor eller genom ytterligare användargränssnittet i hello lösning i hello OMS-instrumentpanelen. 

Till exempel hello [ändringsspårning lösning](https://technet.microsoft.com/library/mt484099.aspx) identifierar konfiguration ändras på agenten system och skriver händelser toohello OMS databasen som kan analyseras med flera grafiska vyer som summerar identifierat ändringar.  Du kan öka detaljnivån från hello sammanfattas vyn i loggen frågor att visa hello detaljerad data som samlas in av hello-lösning.

Medan du kan välja vilka lösningar du lägger till tooyour prenumeration behöver du inte för närvarande hello möjlighet toocreate egna lösningar.  Du kan välja hello händelser och prestandaräknare toocollect och skapa anpassade vyer baserat på egna log-frågor.

hello övervakningslogiken för logganalys sammanfattas i följande diagram hello.

![Log Analytics lösning flöde](media/operations-management-suite-monitoring-product-comparison/log-analytics-solution-flow.png)

## <a name="health-monitoring"></a>Hälsoövervakning
### <a name="operations-manager"></a>Operations Manager
SCOM kan modellen hello olika komponenterna i ett program och ger en realtid hälsotillstånd för varje.  Detta gör att du toonot endast vyn identifierat fel och prestanda över tid, men även toovalidate hello faktiska hälsotillståndet för ett program eller system och varje komponent vid en given tidpunkt.  Eftersom den förstår hello tidsperioder som ett program är tillgängligt, stöder hello hälsa motorn i SCOM också Service Level avtal (SLA) som analysera och rapportera om hello tillgängligheten för ett program över tid.

Hello vyn nedan visar exempelvis hello realtid hälsotillståndet för SQL database motorerna övervakas av SCOM.  hello hälsotillståndet för varje hello databaser för en hello database motorerna visas på hello nedre hälften av hello vyn.

![Tillståndsvy](media/operations-management-suite-monitoring-product-comparison/scom-state-view.png)

Hej Hälsoutforskaren för ett hello database motorerna visas nedan med hello Övervakare som används toodetermine dess allmänna tillstånd.  Dessa Övervakare definieras i hello SQL management pack och köra mot alla SQL-databasprogram som identifierats av SCOM.

![Hälsoutforskaren](media/operations-management-suite-monitoring-product-comparison/scom-health-explorer.png)

Komponenter på flera datorer kan vara kombinerade toomeasure hello hälsotillståndet för ett distribuerat program.  Detta kan vara särskilt användbart för affärsprogram som innehåller flera distribuerade komponenter.  Du kan skapa en modell som mäter hello hälsotillståndet för varje komponent att den samlade in tillgänglighet för hello program.

Active Directory är ett exempel på ett hanteringspaket som innehåller en modell tooanalyze dess distribuerade komponenter.  hello exempel diagrammet nedan visar hello hälsotillstånd hello hela miljön och hello förhållandet mellan skogar, domäner och domänkontrollanter.  Var och en av dessa komponenter omfattar delkomponenter och flera bildskärmar liknande toohello SQL exemplet ovan.

![SCOM diagramvy](media/operations-management-suite-monitoring-product-comparison/scom-diagram-view.png)

### <a name="log-analytics"></a>Log Analytics
OMS inte inkludera en gemensam motorn toomodel program och mäter deras realtid hälsa.  Enskilda lösningar kan utvärdera hello övergripande hälsa för särskilda tjänster baserat på insamlade data och de kan installera egen kod hello agent tooperform realtid analys.  Eftersom lösningar körs i hello moln med åtkomst toohello OMS-databasen, kan de ofta ger djupare analys än normalt utförs av hanteringspaket. 

Till exempel hello [AD-bedömning och SQL-bedömning lösningar](https://technet.microsoft.com/library/mt484102.aspx) analysera insamlade data och ange en klassificering för olika aspekter av hello-miljö.  Den innehåller rekommendationer för förbättringar som kan göras tooimprove hello tillgänglighet och prestanda för hello-miljö.

![AD lösning](media/operations-management-suite-monitoring-product-comparison/log-analytics-ad-assessment.png)

## <a name="data-analysis"></a>Dataanalys
SCOM och logganalys tillhandahålla olika funktioner tooanalyze insamlade data.  SCOM har vyer och instrumentpaneler i hello Operations-konsolen för att analysera senaste data i en mängd olika format och rapporter för att presentera data från datalagret för hello i tabellform.  Log Analytics tillhandahåller en komplett logg frågespråket och gränssnitt för att analysera data i hello OMS-databas.  När SCOM används som datakälla för Log Analytics innehåller hello databasen data som samlas in av SCOM så hello logganalys verktyg kan vara används tooanalyze data från båda systemen.

### <a name="operations-manager"></a>Operations Manager
#### <a name="views"></a>Vyer
Vyer i hello Operations-konsolen kan du tooview olika datatyper som samlas in av SCOM i olika format, vanligtvis tabular för händelser, varningar och Systemtillstånd och linjediagram för prestandadata.  Vyer utföra minimal analys eller konsolidering av hello data, men tillåter toofilter enligt tooparticular kriterier. 

![Vyer](media/operations-management-suite-monitoring-product-comparison/scom-views.png)

Hanteringspaket ger vanligtvis flera vyer som stöder hello programmets eller systemets som den övervakar.  Detta kan inkludera tillståndsvyer för hello olika objekt som hello hanteringspaket identifierar, varna vyer för identifierade problem och prestandavyer för räknare.

Vyer är särskilt lämplig för att analysera hello aktuell status för hello miljö, inklusive öppna aviseringar och hello hälsotillståndet för övervakade system och objekt.  Du kan öka detaljnivån toodetailed händelser och data som stöder en viss avisering i ordning toodiagnose dess orsaken. På liknande sätt kan kan du visa hello prestanda- och olika komponenter i ett program tooassess dess aktuella hälsa.

#### <a name="dashboards"></a>Instrumentpaneler
Instrumentpaneler i hello Operations-konsolen i första hand arbeta med hello samma data som vyer men mer anpassningsbara och kan innehålla bättre visualiseringar.  En uppsättning standard instrumentpaneler är tillgängliga att du kan enkelt anpassa för egna ändamål.  Du kan också använda en PowerShell-widget som kan visa data som returnerats från en PowerShell-fråga.

![Instrumentpanel](media/operations-management-suite-monitoring-product-comparison/scom-dashboard.png)

Utvecklare har hello möjlighet tooadd anpassade komponenter toodashboards de inkluderar i sina hanteringspaket.  Dessa kan vara mycket specialiserade tooa visst program, till exempel hello instrumentpanelen i hello SQL management pack som visas nedan.  Den här instrumentpanelen kan också användas som en mall för anpassade versioner.

![SQL-instrumentpanelen](media/operations-management-suite-monitoring-product-comparison/scom-sql-dashboard.png)

#### <a name="reports"></a>Rapporter
Rapporter i SCOM analysera data från datalagret för hello i tabellform.  De kan skrivas ut och schemalagd automatisk i olika filformat, inklusive PDF, CSV och Word.  Rapporter arbeta med data från datalagret för hello så att de är särskilt lämplig för analys av långsiktiga trender.

Hanteringspaket ger vanligtvis anpassade rapporter för ett visst program.  Du kan också välja från ett bibliotek för allmänna rapporter som du kan anpassa för ditt eget program eller för att ad hoc-analys.

Följande är en exempelrapport prestanda som visar data som samlas in av hello Active Directory Management Pack.

![Rapport](media/operations-management-suite-monitoring-product-comparison/scom-report.png)

### <a name="log-analytics"></a>Log Analytics
Logganalys har en [frågespråket](https://technet.microsoft.com/library/mt484120.aspx) som du kan använda tooperform analys över data från flera program utan hello måste toocreate en anpassad vy eller rapport.  Eftersom OMS implementeras i hello molnet är inte ämne tooany maskinvarans begränsningar prestanda för frågor och analys av data och kan snabbt analysera inklusive miljontals poster. 

Frågorna i logganalys är också hello grunden för andra funktioner.  Du kan spara en fråga, exportera dess resultat tooExcel eller automatiskt köra regelbundet och generera en avisering om resultaten uppfyller specifika villkor.  

![Loggen frågan flöde](media/operations-management-suite-monitoring-product-comparison/log-analytics-query-flow.png)

Nedan visas ett exempel på en Log Analytics-fråga.  I det här exemplet returneras alla händelser med ”starta” hello namn och grupperade efter händelse-ID.  hello användaren helt enkelt hello frågan och logganalys genererar dynamiskt hello gränssnittet tooperform hello Användaranalys.  Att välja ett objekt i listan hello returnerar hello detaljerad händelsedata.

![Log-fråga](media/operations-management-suite-monitoring-product-comparison/log-analytics-query.png)

Dessutom tooproviding ad hoc-analys, frågor i logganalys kan sparas för framtida användning och tillagda tooyour [OMS instrumentpanelen](http://technet.microsoft.com/library/mt484090.aspx) som visas i följande exempel hello.

![OMS-instrumentpanelen](media/operations-management-suite-monitoring-product-comparison/log-analytics-dashboard.png)

## <a name="next-steps"></a>Nästa steg
* Distribuera [System Center Operations Manager (SCOM)](https://technet.microsoft.com/library/hh205987.aspx).
* Registrera dig för [logganalys](https://azure.microsoft.com/documentation/services/log-analytics).  

