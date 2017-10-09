---
title: "aaaOperations Management Suite (OMS) översikt | Microsoft Docs"
description: "Microsoft Operations Management Suite (OMS) är Microsofts molnbaserade IT-hanteringslösning som hjälper dig att hantera och skydda din lokala och molnbaserade infrastruktur.  Den här artikeln beskriver hello värdet för OMS, identifierar hello olika tjänster och erbjudanden som ingår i OMS och länkar tootheir detaljerad innehåll."
services: operations-management-suite
documentationcenter: 
author: bwren
manager: jwhit
editor: tysonn
ms.assetid: 9dc437b9-e83c-45da-917c-cb4f4d8d6333
ms.service: operations-management-suite
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/16/2017
ms.author: bwren
ms.openlocfilehash: ec3fe6d82aec46d1f715a4338f126e79e04a9147
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="what-is-operations-management-suite-oms"></a>Vad är Operations Management Suite (OMS)?
Den här artikeln innehåller en introduktion tooOperations Management Suite (OMS) inklusive en kort översikt över hello affärsvärde ger och hello tjänster och hanteringslösningar innehåller hello-erbjudanden som paketet ihop olika tjänster och lösningar.  Finns länkar toohello detaljerad dokumentation om distribuerar och använder varje tjänst och en lösning.

## <a name="from-on-premises-toohello-cloud"></a>Från lokala toohello moln
Microsoft har länge erbjudit produkter för att hantera företagsmiljöer.  Flera produkter har sammanställs till hello System Center-produkter för hantering i 2007.  Detta med Configuration Manager som innehåller funktioner som programvarudistribution och lager, Operations Manager som innehåller Proaktiv övervakning av system och program, Orchestrator som innehåller runbooks tooautomate manuella processer , och Data Protection Manager för säkerhetskopiering och återställning av kritiska data.

Med mer datorresurser flytta toohello molnet, med System Center-produkter fler molnfunktioner som Operations Manager och Orchestrator hantera resurser i Azure.  Dessa var fortfarande utformade som lokala lösningar och krävde stora investeringar i distribution och underhåll av en lokal hanteringsmiljö.  toocompletely utnyttja hello molnet och stöd för framtida program, en ny metod toomanagement krävdes.

## <a name="introducing-operations-management-suite"></a>Operations Management Suite
Operations Management Suite (även kallat OMS) är en uppsättning tjänster som har utformats i hello molnet från hello start.  I stället för att distribuera och hantera lokala resurser finns alla OMS-komponenter i Azure.  Konfigurationen är minimal, och du kommer igång på bara några minuter.  

- **Distribution med minimal kostnad och komplexitet.**  Eftersom alla hello komponenter och data för OMS lagras i Azure vara igång och körs på kort tid utan hello komplexitet och investering i lokala komponenter.
- **Skala toocloud nivåer.**  Du har inte tooworry om betalning av beräkningsresurser som du inte behöver eller om slut på utrymme i lagringspoolen eftersom hello molnet kan du toopay endast för vad du faktiskt använder och skalar lätt tooany belastning som du behöver.  Du kan börja med att hantera några resurser tooget igång och sedan skala upp tooyour hela miljön.
- **Dra nytta av hello senaste funktionerna.**  Nya och uppdaterade funktioner läggs hela tiden till i OMS-tjänsterna.  Du har ofta toohello senaste åtkomstfunktioner utan några krav toodeploy uppdateringar.
- **Integrerade tjänster.**  När varje hello OMS services ger betydande värdet på egen hand, kan de arbeta tillsammans toosolve avancerade scenarier.  En runbook i Azure Automation kan driva en process för växling vid fel med Azure Site Recovery och loggar information tooLog Analytics toogenerate en avisering.
- **Globala kunskaper.**  Lösningar för hantering i OMS har alltid åtkomst toohello senaste informationen.  hello säkerhet och granska lösningen till exempel kan analysera hot med hello senaste hoten identifieras hello världen.
- **Åtkomst överallt.**  Få åtkomst till din hanteringsmiljö överallt via webbläsaren.  Installera hello OMS-appen på din smartphone för direktåtkomst tooyour övervakningsdata.

### <a name="is-it-just-for-hello-cloud"></a>Är det bara för hello molnet?
Bara för att OMS-tjänster körs i betyder hello molnet att de effektivt kan hantera din lokala miljö.  Placera en agent på alla Windows eller Linux-dator i datacentret och den skickar data tooLog Analytics där den kan analyseras tillsammans med alla data som samlas in från molnet eller lokala tjänster.  Använda Azure Backup och Azure Site Recovery tooleverage hello moln för säkerhetskopiering och hög tillgänglighet för lokala resurser.  
Runbooks i hello molnet normalt inte kan komma åt lokala resurser, men du kan installera en agent på en eller flera datorer för som ska vara värd för runbooks i ditt datacenter.  När du startar en runbook kan ange du bara om du vill att den toorun i hello moln eller på en lokal worker.

## <a name="hybrid-management-with-system-center"></a>Hybridhantering med System Center
Om du har en befintlig installation av System Center kan du integrera komponenterna med OMS services tooprovide hybridlösning för både dina lokala och molnet miljöer utnyttja hello relativa lösningar för varje produkt.  Anslut dina befintliga Operations Manager management grupp tooLog Analytics tooanalyze hanteras agenter i hello molnet.  Använda din befintliga säkerhetskopieringen med Data Protection Manager toobackup data toohello molnet.  


## <a name="oms-services"></a>OMS-tjänster
hello huvudfunktionerna i OMS tillhandahålls av en uppsättning tjänster som körs i Azure.  Varje tjänst innehåller en funktion för specifika hanteringsservern och du kan kombinera olika hanteringsscenarier för tjänster tooachieve.

|| Tjänst | Beskrivning |
|:--|:--|:--|
| ![Log Analytics](media/operations-management-suite-overview/icon-log-analytics.png) | Log Analytics | Övervaka och analysera hello tillgänglighet och prestanda för olika resurser, inklusive fysiska och virtuella datorer. |
| ![Azure Automation](media/operations-management-suite-overview/icon-automation.png) | Automation | Automatisera manuella processer och tillämpa konfigurationer för fysiska och virtuella datorer. |
| ![Azure Backup](media/operations-management-suite-overview/icon-backup.png) | Säkerhetskopiering | Säkerhetskopiera och återställa kritiska data. |
| ![Azure Site Recovery](media/operations-management-suite-overview/icon-site-recovery.png) | Site Recovery | Ge hög tillgänglighet för viktiga program. |

### <a name="log-analytics"></a>Log Analytics
[Log Analytics](http://azure.microsoft.com/documentation/services/log-analytics) tillhandahåller övervakning för OMS genom att samla in data från hanterade resurser i en central databas.  Dessa data kan omfatta händelser, prestandadata, eller anpassade data som tillhandahålls via hello API. När samlas in, är hello data tillgängliga för aviseringar, analys och export.  Den här metoden kan du tooconsolidate data från olika källor så kan du kombinera data från Azure-tjänster med din befintliga lokala miljö.  Den också tydligt skiljer hello samling hello data från hello åtgärd på dessa data så att alla åtgärder är tillgängliga tooall typer av data.  

![Översikt över Log Analytics](media/operations-management-suite-overview/overview-log-analytics.png)

#### <a name="collecting-data"></a>Samla in data
Det finns flera olika sätt som du kan hämta data till hello lagringsplatsen för Log Analytics tooanalyze.

- **Windows- eller Linux-datorer och virtuella datorer.**  Installation av hello Microsoft Monitoring Agent på [Windows](../log-analytics/log-analytics-windows-agents.md) och [Linux](../log-analytics/log-analytics-linux-agents.md) och virtuella datorer som du vill toocollect data från.  hello agent hämtar automatiskt från konfigurationen för logganalys som definierar händelserna och prestanda data toocollect.  Du kan enkelt installera hello agent på virtuella datorer som körs i Azure med hjälp av hello Azure-portalen.  Om du har en befintlig Operations Manager-miljö kan du ansluta hello management group tooLog Analytics och startar automatiskt samla in data från alla befintliga agenter.
- **Azure-tjänster.**  Logganalys samlar in telemetri från [Azure Diagnostics och Azure-övervakning](../log-analytics/log-analytics-azure-storage.md) i hello databas så att du kan övervaka Azure-resurser.
- **API för datainsamling.**  Log Analytics har ett [REST-API för att fylla i data från alla klienter](../log-analytics/log-analytics-data-collector-api.md).  Detta ger dig toocollect data från program från tredje part eller implementera anpassade hanteringsscenarier.  En vanlig metod är toouse en runbook i Azure Automation toocollect data och sedan använda hello Data Collector API toowrite den toohello databasen.

#### <a name="reporting-and-analyzing-data"></a>Rapportera och analysera data
Logganalys innehåller en kraftfull frågan språk tooextract data som lagras i hello-databasen.  Eftersom data från alla källor lagras som poster kan du analysera data från flera källor med en enda fråga.
  
Dessutom innehåller flera olika sätt tooreport tooad hoc analys, logganalys och analysera data från en fråga.

- **Vyer och instrumentpaneler.**  [Vyer](../log-analytics/log-analytics-view-designer.md) och [instrumentpaneler](../log-analytics/log-analytics-dashboards.md) visualisera hello resultatet av en fråga i hello-portalen.  Hanteringslösningar inkluderar vanligtvis vyer som analyserar hello data från hello lösning.  Du kan också skapa egna anpassade vyer tooanalyze data och göra den tillgänglig i din anpassade portal.
- **Exportera.**  Du har hello alternativet tooexport hello resultatet av en fråga så att du kan analysera utanför logganalys.  Du kan även schemalägga en regelbunden export för[Power BI](../log-analytics/log-analytics-powerbi.md) som tillhandahåller viktiga funktioner för visualisering och analys.
- **Loggsöknings-API.**  Log Analytics har ett [REST-API för att samla in data från alla klienter](../log-analytics/log-analytics-log-search-api.md).  Detta ger dig tooprogrammatically arbete med data som samlas in i hello databas eller komma åt den från en annan övervakningsverktyg.

#### <a name="alerting"></a>Aviseringar
Log Analytics kan skicka [proaktiva aviseringar](../log-analytics/log-analytics-alerts.md) till dig eller vidta åtgärder när ett problem upptäcks.  Precis som med alla andra analyser i Log Analytics utförs detta med en loggsökning.  Sökningen körs enligt ett schema och en avisering skapas om hello resultatet överensstämmer med särskilda kriterier.

![Log Analytics-aviseringar](media/operations-management-suite-overview/overview-alerts.png)

Dessutom toocreating en avisering post i hello logganalys databasen aviseringar kan ta hello följande åtgärder.

- **E-post.**  Skicka ett e-postmeddelande tooproactively meddelar dig om ett identifierade problem.
- **Runbook.**  En avisering i Log Analytics kan starta en runbook i Azure Automation.  Detta görs vanligtvis tooattempt toocorrect hello identifierat problemet.  Hej runbook kan startas i hello moln i hello fall av ett problem i Azure eller ett annat moln, eller så kunde startas på en lokal agent på ett problem på en fysisk eller virtuell dator.
- **Webhook.**  En avisering kan starta en webhook och skicka data från hello resultaten av hello loggen.  Detta möjliggör integrering med externa tjänster, till exempel ett alternativt aviseringar system eller den kan försöka tootake korrigerande åtgärd för en extern webbplats.

### <a name="azure-automation"></a>Azure Automation
[Azure Automation](http://azure.microsoft.com/documentation/services/automation) innehåller processen automation och konfiguration management tooOMS.  Den automatiserar manuella processer och hjälper tooenforce konfigurationer för fysiska och virtuella datorer.  

#### <a name="process-automation"></a>Processautomatisering
Azure Automation automatiserar manuella processer med hjälp av [runbook-flöden](../automation/automation-runbook-types.md) som är baserade på PowerShell-skript eller PowerShell-arbetsflöden.  Den omfattar också tillgångar stöder runbooks, till exempel variabler som kan delas mellan flera runbooks och autentiseringsuppgifter och anslutningar som kan toostore krypterad information som kan krävas för en runbook för autentisering.
Runbooks erbjuder Processautomatisering för hello andra tjänster i hello suite.  Eftersom varje hello andra tjänster kan användas med PowerShell eller via ett REST-API kan du skapa runbooks tooperform sådana funktioner som samlar in hanteringsdata i logganalys eller initierar en säkerhetskopia med Azure Backup.

##### <a name="accessing-resources"></a>Komma åt resurser
Eftersom runbook-flöden baseras på PowerShell kan de hantera alla resurser som går att komma åt med PowerShell-cmdlets.  När du [läsa in en modul](../automation/automation-integration-modules.md) till ditt Automation-konto blir tillgängliga tooall runbooks i kontot. 
 
När runbooks körs i molnet hello, de komma åt några resurser som är tillgänglig från hello molnet.  Det kan till exempel vara resurser i din Azure-prenumeration, i ett annat moln som Amazon Web Services (AWS) eller i en tjänst som kan nås via ett REST-API.  Runbooks i molnet hello köras inte under autentiseringsuppgifterna för eventuella, men de kan dra nytta av automatisering tillgångar som autentiseringsuppgifter, anslutningar och certifikat tooauthenticate tooresources som de har åtkomst till.

Resurser i ditt datacenter kan mest sannolikt inte nås från en runbook som körs i hello moln.  Du kan installera en eller flera [Runbook Worker-hybrider](../automation/automation-hybrid-runbook-worker.md) i dina data center om toorun runbooks som kräver åtkomst toolocal resurser.  När du startar en runbook kan ange du om det ska köras i hello moln eller på en specifik arbetsprocess.

![Runbook-flöden för Azure Automation](media/operations-management-suite-overview/overview-runbooks.png)

##### <a name="starting-a-runbook"></a>Starta en runbook
Runbook-flöden kan [startas på flera olika sätt](../automation/automation-starting-a-runbook.md) beroende på vad de ska användas till.  

- **Azure Portal.**  Azure Automation kan hanteras från hello Azure-portalen som andra Azure-tjänster.  Dessutom toostarting runbooks, du kan importera dem eller skapa egna.
- **Schemalägga.**  Du kan schemalägga runbooks toostart med jämna mellanrum.  Detta ger dig tooautomatically Upprepa en vanlig hanteringen eller samla in data tooLog Analytics.
- **PowerShell och API.**  Du kan starta runbooks och skicka dem krävs parameterinformation från en PowerShell-cmdlet eller hello Azure Automation REST API.  
- **Webhook.**  Du kan skapa en webhook för valfri runbook som gör att det toobe som startas från externa program eller webbplatser.
- **Avisering i Log Analytics.**  En avisering i logganalys kan starta en runbook tooattempt toocorrect hello problem som identifieras av hello aviseringen automatiskt.

#### <a name="configuration-management"></a>Konfigurationshantering
[PowerShell önskad tillstånd Configuration (DSC)](../automation/automation-dsc-overview.md) är en plattform i Windows PowerShell gör du det toodeploy och genomdriva hello konfiguration av fysiska och virtuella datorer.  Azure Automation hanterar DSC-konfigurationer och ger en pull-server i hello moln att agenterna kan komma åt tooretrieve krävs konfigurationer.

![Azure Automation DSC](media/operations-management-suite-overview/overview-dsc.png)

### <a name="azure-backup-and-azure-site-recovery"></a>Azure Backup och Azure Site Recovery
Azure Backup och Azure Site Recovery bidrar toobusiness affärskontinuitet och haveriberedskap återställning.  De har funktioner som hjälper dig tooensure att program fortfarande är tillgängliga när avbrott inträffar och returnera toonormal åtgärder när system är tillbaka online.  Båda tjänsterna bidra toohello återställningspunktmål (återställningspunkter) och återställningstiden (RTOs) har definierats för din organisation. Din Återställningpunktsmål definierar hello godtagbara gränsen där data är inte tillgängligt under ett avbrott och hello RTO begränsar hello acceptabel tidsperiod som en tjänst eller appen inte tillgängliga under ett avbrott.

#### <a name="azure-backup"></a>Azure Backup
[Azure Backup](http://azure.microsoft.com/documentation/services/backup) förser OMS med tjänster för säkerhetskopiering och återställning av data.  Tjänsten skyddar dina programdata och sparar dem i åratal utan stora investeringar och med minimala driftkostnader.  Det kan säkerhetskopiera data från fysiska och virtuella Windows-servrar i tillägg tooapplication arbetsbelastningar som till exempel SQL Server och SharePoint.  Det kan även användas av System Center Data Protection Manager (DPM) tooreplicate skyddade data tooAzure för redundans och långtidslagring.

Skyddade data i Azure Backup lagras i ett säkerhetskopieringsvalv som finns i en viss geografisk region. hello data replikeras inom hello samma region och, beroende på hello typ av valvet, kan också vara replikerade tooanother region för ytterligare återhämtning.

Azure Backup har tre grundläggande scenarier.

- **Windows-dator med Azure Backup-agent.** Säkerhetskopiera filer och mappar från Windows server eller klient direkt tooyour Azure backup-valvet.<br><br>![Windows-dator med Azure Backup-agent](media/operations-management-suite-overview/overview-backup-01.png)
- **System Center Data Protection Manager (DPM) eller Microsoft Azure Backup Server.** Dra nytta av DPM eller Microsoft Azure Backup-Server toobackup filer och mappar i tillägg tooapplication arbetsbelastningar som till exempel SQL och SharePoint toolocal lagring och replikera tooyour Azure backup-valvet. Har stöd för virtuella Windows- och Linux-datorer på Hyper-V eller VMware.<br><br>![System Center Data Protection Manager (DPM) eller Microsoft Azure Backup Server](media/operations-management-suite-overview/overview-backup-02.png)
- **Azure Virtual Machine-tillägg.** Säkerhetskopiera Windows eller Linux virtuella datorer i Azure tooyour Azure backup-valvet.<br><br>![Azure Virtual Machine-tillägg](media/operations-management-suite-overview/overview-backup-03.png)



#### <a name="azure-site-recovery"></a>Azure Site Recovery
[Azure Site Recovery](http://azure.microsoft.com/documentation/services/site-recovery) ger affärskontinuitet genom att samordna replikeringen av lokala virtuella och fysiska datorer tooAzure eller tooa sekundär plats. Om den primära platsen är tillgänglig, växlar du över toohello sekundära platsen så att användarna kan hålla fungerande och växla tillbaka när system returnerar tooworking ordning. 

Azure Site Recovery ger hög tillgänglighet för servrar och program.  Det bidrar tooyour kontinuitet för företag och ett haveriberedskap (BCDR) genom att samordna replikering, redundans och återställning av lokala Hyper-V virtuella datorer, virtuella VMware-datorer och fysiska Windows-/ Linux-servrar. Du kan replikera datorer tooa sekundära Datacenter eller utöka ditt datacenter genom att replikera dem tooAzure. Site Recovery tillhandahåller också enkel redundansväxling och återställning för arbetsbelastningar. Funktionen kan integreras med mekanismer för haveriberedskap som SQL Server AlwaysOn och tillhandahåller återställningsplaner för enkel redundansväxling av arbetsbelastningar som är nivåindelade över flera datorer.

Azure Site Recovery har tre grundläggande replikeringsscenarier.

- **Replikering av virtuella Hyper-V-datorer.**  Om Hyper-V-datorer hanteras i VMM-moln, kan du replikera tooa sekundära center eller tooAzure datalagring. Replikering tooAzure är via en säker Internetanslutning. Replikering tooa sekundärt datacenter är över hello LAN.  Om Hyper-V-datorer inte hanteras av VMM, kan du replikera tooAzure-lagring. Replikering tooAzure är via en säker Internetanslutning.<br><br>![Replikering av virtuella Hyper-V-datorer](media/operations-management-suite-overview/overview-siterecovery-hyperv.png)
- **Replikering av virtuella VMware-datorer.**  Du kan replikera VMware virtuella datorer tooa sekundärt datacenter kör VMware eller tooAzure lagringsutrymme. Replikering tooAzure kan ske via en plats-till-plats VPN- eller Azure ExpressRoute eller via en säker Internetanslutning. Replikering tooa sekundärt datacenter sker via hello InMage Scout datakanalen.<br><br>![Replikering av virtuella VMware-datorer](media/operations-management-suite-overview/overview-siterecovery-vmware.png)
- **Replikering av fysiska Windows- och Linux-servrar.**  Du kan replikera fysiska servrar tooa sekundära datacenter eller tooAzure lagringsutrymme. Replikering tooAzure kan ske via en plats-till-plats VPN- eller Azure ExpressRoute eller via en säker Internetanslutning. Replikering tooa sekundärt datacenter sker via hello InMage Scout datakanalen. Azure Site Recovery har en OMS-lösning som visar del statistik, men du måste använda hello Azure-portalen för alla åtgärder.<br><br>![Replikering av fysiska Windows- och Linux-servrar](media/operations-management-suite-overview/overview-siterecovery-physical.png)


Site Recovery lagrar metadata i valv som finns i en viss geografisk Azure-region. Inga replikerade data lagras av hello Site Recovery-tjänsten.

## <a name="management-solutions"></a>Hanteringslösningar
[Hanteringslösningar](operations-management-suite-solutions.md) är färdiga logikuppsättningar för olika scenarier som utnyttjar en eller flera OMS-tjänster.  Olika lösningar är tillgängliga från Microsoft och att du kan enkelt lägga till Azure-prenumeration tooyour tooincrease hello värdet på din investering i OMS-partner.  Du kan skapa egna lösningar toosupport dina program och tjänster och ge dem toousers via hello Azure Marketplace eller Snabbstartsmallar som partner.

Ett bra exempel på en lösning som använder flera tjänster tooprovide ytterligare funktioner är hello [uppdatering hanteringslösning](oms-solution-update-management.md).  Den här lösningen använder hello logganalys agenten för Windows och Linux toocollect information om nödvändiga uppdateringar på varje agent.  Skriver data toohello logganalys databasen där du kan analysera dem med en inkluderade instrumentpanel.  När du skapar en distribution, är runbooks i Azure Automation används tooinstall som krävs för uppdateringar.  Du hanterar hela processen i hello portal och behöver inte tooworry om hello underliggande information.

![Lösning](media/operations-management-suite-overview/overview-solution.png)

De flesta lösningar kan utföra en eller flera av följande funktioner hello.

- Samla in ytterligare information.  Log Analytics samlar in en mängd olika data från klienter och tjänster, inklusive händelse- och prestandadata.  En hanteringslösning kan samla in ytterligare information som inte är tillgänglig från andra datakällor, ofta med hjälp av runbook-flöden för Azure Automation.
- Tillhandahålla ytterligare analys av insamlad information.  I hanteringslösningarna ingår instrumentpaneler och vyer för att analysera och visualisera data.  Länken tillbaka toopredefined loggen sökningar som du toodrill till hello detaljerad data.  De kan också utföra analyser på data som är redan samlats in hello-databasen, till exempel att söka i säkerhetshändelser för mönster som indikerar ett hot.
- Lägga till funktioner.  Vissa lösningar som tillhandahålls av Microsoft kan bygger på hello funktionerna i hello core services tooprovide ytterligare funktioner.  Tjänstkarta till exempel innehåller sin egen konsol toodiscover och mappar server och Processberoenden i realtid.
Lösningar läggs regelbundet tooOMS av Microsoft och partners så att du toocontinuously öka hello värdet av investeringen.  Du kan bläddra och installera Microsoft solutions via hello lösningar katalog i hello OMS-portalen eller bläddrar och installerar både Microsoft och partner solutions via hello Azure Marketplace i hello Azure-portalen.  

![Lösningsgalleriet](media/operations-management-suite-overview/solution-gallery.png)


## <a name="next-steps"></a>Nästa steg
* Läs mer om [Log Analytics](http://azure.microsoft.com/documentation/services/log-analytics).
* Läs mer om [Azure Automation](../automation/automation-intro.md).
* Läs mer om [Azure Backup](http://azure.microsoft.com/documentation/services/backup).
* Läs mer om [Azure Site Recovery](http://azure.microsoft.com/documentation/services/site-recovery).
* Identifiera hello [lösningar som är tillgängliga](../log-analytics/log-analytics-add-solutions.md) i hello olika OMS-erbjudanden. 

