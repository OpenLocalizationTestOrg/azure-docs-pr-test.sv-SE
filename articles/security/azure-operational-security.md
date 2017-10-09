---
title: "aaaAzure operativ säkerhet | Microsoft Docs"
description: "Lär dig mer om Microsoft Operations Management Suite (OMS), dess tjänster och hur det fungerar."
services: security
documentationcenter: na
author: UnifyCloud
manager: swadhwa
editor: TomSh
ms.assetid: 
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 04/27/2017
ms.author: TomSh
ms.openlocfilehash: 85e0c74314ed97a53d395b209e348b779a5d14c8
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-operational-security"></a>Azure operativ säkerhet
## <a name="introduction"></a>Introduktion

### <a name="overview"></a>Översikt
Vi vet att säkerheten är jobbet en i hello molnet och det är viktigt att du hitta korrekt och rimlig information om säkerheten i Azure. En av hello bästa orsaker toouse Azure för dina program och tjänster är tootake nytta av hello mängd säkerhetsverktyg och funktioner som är tillgängliga. Dessa verktyg och funktioner gör det möjligt toocreate säkra lösningar på hello säker Azure-plattformen. Windows Azure måste tillhandahålla sekretess, integritet och tillgänglighet av kundinformation, samtidigt som transparent accountability.

toohelp kunder bättre förstå hello matris med säkerhetsåtgärder som implementerats i Microsoft Azure från båda hello-kund och Microsoft operativa perspektiv, faktabladet ”Azure operativ säkerhet”, skrivs som ger en omfattande titt på hello operativ säkerhet som är tillgängliga med Windows Azure.

### <a name="azure-platform"></a>Azure-plattformen
Azure är en offentlig molntjänstplattform som har stöd för ett brett urval av operativsystem, programmeringsspråk, ramverk, verktyg, databaser, och enheter. Det kan köras Linux behållare med Docker-integration. skapa appar med JavaScript, Python, .NET, PHP, Java och Node.js; build-servrar för iOS, Android och Windows enheter. Molntjänsten Azure stöder hello samma tekniker miljoner utvecklare och IT-proffs redan förlitar sig på och litar på.

När du bygger på, eller migrera IT tillgångar, en offentlig molntjänstleverantören måste den organisationens förmåga tooprotect dina program och data med hello tjänster och hello kontroller ger toomanage hello säkerheten för din molnbaserade tillgångar.

Azures infrastruktur har utformats från hello anläggning tooapplications som värd för miljontals kunder samtidigt och det ger en säker grund som företag kan uppfylla sina säkerhetskrav. Dessutom Azure ger dig en mängd olika konfigurerbara säkerhet alternativ och hello möjlighet toocontrol dem så att du kan anpassa toomeet hello unika säkerhetskrav för distributioner i din organisation. Det här dokumentet kommer hjälper dig att förstå hur Azure-säkerhet funktioner kan hjälpa dig att uppfylla kraven.

### <a name="abstract"></a>Abstrakt
Azure operativ säkerhet refererar toohello tjänster, kontroller och funktioner tillgängliga toousers för att skydda sina data, program och andra resurser i Microsoft Azure. Azure operativ säkerhet bygger på ett ramverk som inkluderar hello kunskap via olika funktioner som är unika tooMicrosoft, inklusive hello Microsoft Security Development Lifecycle (SDL), hello Microsoft Security Response Center programmet och djup medvetenhet om hello cybersecurity hotbild.

I det här dokumentet beskrivs Microsofts metoden tooAzure operativ säkerhet i hello Microsoft Azure cloud plattform och omfattar följande tjänster:
1.  [Azure Operations Management Suite](https://docs.microsoft.com/azure/operations-management-suite/operations-management-suite-overview)

2.  [Azure Security Center](https://docs.microsoft.com/azure/security-center/security-center-intro)

3.  [Azure Monitor](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview)

4.  [Azure nätverksbevakaren](https://docs.microsoft.com/azure/network-watcher/network-watcher-monitoring-overview)

5.  [Azure Storage analytics](https://docs.microsoft.com/rest/api/storageservices/fileservices/storage-analytics)

6.  [Azure Active directory](https://docs.microsoft.com/azure/active-directory/active-directory-whatis)


## <a name="microsoft-operations-management-suite"></a>Microsoft Operations Management Suite

Microsoft Operations Management Suite (OMS) är hello IT-hanteringslösning för hello hybridmoln. Ensamt eller tooextend din befintliga System Center-distribution, OMS ger dig hello maximal flexibilitet och kontroll för molnbaserad hantering av infrastrukturen.

![Microsoft Operations Management Suite](./media/azure-operational-security/azure-operational-security-fig1.png)

Du kan hantera valfri instans i något moln, inklusive lokalt, Azure, AWS, Windows Server, Linux, VMware och OpenStack, till en lägre kostnad än konkurrenskraftiga lösningar med OMS. OMS erbjuder byggt för molnet första hälsningsmeddelande en ny metod toomanaging företaget som är hello snabbaste och mest kostnadseffektiva sätt toomeet nya utmaningar och hantera nya arbetsbelastningar, program och miljöer i molnet.

### <a name="oms-services"></a>OMS-tjänster

hello huvudfunktionerna i OMS tillhandahålls av en uppsättning tjänster som körs i Azure. Varje tjänst innehåller en funktion för specifika hanteringsservern och du kan kombinera olika hanteringsscenarier för tjänster tooachieve.

| Tjänst  | Beskrivning|
| :------------- | :-------------|
| Log Analytics | Övervaka och analysera hello tillgänglighet och prestanda för olika resurser, inklusive fysiska och virtuella datorer. |
|Automation | Automatisera manuella processer och tillämpa konfigurationer för fysiska och virtuella datorer. |
| Säkerhetskopiering | Säkerhetskopiera och återställa kritiska data. |
| Webbplatsåterställning | Ge hög tillgänglighet för viktiga program. |

### <a name="log-analytics"></a>Log Analytics

[Log Analytics](http://azure.microsoft.com/documentation/services/log-analytics) tillhandahåller övervakning för OMS genom att samla in data från hanterade resurser i en central databas. Dessa data kan omfatta händelser, prestandadata, eller anpassade data som tillhandahålls via hello API. När samlas in, är hello data tillgängliga för aviseringar, analys och export.


Den här metoden kan du tooconsolidate data från olika källor, så kan du kombinera data från Azure-tjänster med din befintliga lokala miljö. Den också tydligt skiljer hello samling hello data från hello åtgärd på dessa data så att alla åtgärder är tillgängliga tooall typer av data.


![Log Analytics](./media/azure-operational-security/azure-operational-security-fig2.png)

hello logganalys-tjänsten hanterar molnbaserade data på ett säkert sätt med hjälp av hello följande metoder:
-   Dataavgränsning
-   datalagring
-   Fysisk säkerhet
-   Hantering av incidenter
-   Kompatibilitet
-   standarder Säkerhetscertifieringar

### <a name="azure-backup"></a>Azure Backup

[Azure-säkerhetskopiering](http://azure.microsoft.com/documentation/services/backup) innehåller data att säkerhetskopiera och återställa tjänster och hello OMS uppsättning produkter och tjänster.
Tjänsten skyddar dina programdata och sparar dem i åratal utan stora investeringar och med minimala driftkostnader. Det kan säkerhetskopiera data från fysiska och virtuella servrar dessutom tooapplication arbetsbelastningar som till exempel SQL Server och SharePoint. Det kan även användas av [System Center Data Protection Manager (DPM)](https://en.wikipedia.org/wiki/System_Center_Data_Protection_Manager) tooreplicate skyddade data tooAzure för redundans och långsiktig lagring.


Skyddade data i Azure Backup lagras i ett säkerhetskopieringsvalv som finns i en viss geografisk region. hello data replikeras inom hello samma region och, beroende på hello typ av valvet, kan också vara replikerade tooanother region för ytterligare återhämtning.

### <a name="management-solutions"></a>Hanteringslösningar
[Microsoft Operations Management Suite (OMS)](https://docs.microsoft.com/azure/operations-management-suite/oms-security-getting-started) är Microsofts molnbaserade IT lösning som hjälper dig att hantera och skydda dina lokala och molnet infrastruktur.


[Hanteringslösningar](https://docs.microsoft.com/azure/operations-management-suite/operations-management-suite-solutions) färdigförpackade består av logics som implementerar en viss management-scenario med en eller flera OMS-tjänster. Olika lösningar är tillgängliga från Microsoft och att du kan enkelt lägga till Azure-prenumeration tooyour tooincrease hello värdet på din investering i OMS-partner. Som partner kan du skapa egna lösningar toosupport dina program och tjänster och ge dem toousers via hello Azure Marketplace eller snabb Start mallar.


![Hanteringslösningar](./media/azure-operational-security/azure-operational-security-fig4.png)

Ett bra exempel på en lösning som använder flera tjänster tooprovide ytterligare funktioner är hello [uppdatering hanteringslösning](https://docs.microsoft.com/azure/operations-management-suite/oms-solution-update-management). Den här lösningen använder hello [logganalys](https://docs.microsoft.com/azure/log-analytics/log-analytics-overview) agent för Windows och Linux toocollect information om nödvändiga uppdateringar på varje agent. Skriver data toohello logganalys databasen där du kan analysera dem med en inkluderade instrumentpanel.

När du skapar en distribution av runbooks i [Azure Automation](https://docs.microsoft.com/azure/automation/automation-intro) uppdateras används tooinstall krävs. Du hanterar hela processen i hello portal och behöver inte tooworry om hello underliggande information.

## <a name="azure-security-center"></a>Azure Security Center

Azure Security Center skyddar resurserna i Azure. Det ger integrerad säkerhet övervaka och hantera principer för dina Azure-prenumerationer. Inom hello-tjänsten, kan du inte toodefine principerna inte bara mot dina Azure-prenumerationer, utan även mot [resursgrupper](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview#resource-groups), så du kan vara mer detaljerade.

### <a name="security-policies-and-recommendations"></a>Säkerhetsprinciper och säkerhetsrekommendationer

En säkerhetsprincip definierar hello kontrollfunktioner som rekommenderas för resurser inom hello angivna prenumerationen eller resursen gruppen.

I Security Center kan du definiera principer enligt tooyour företagets säkerhetskrav och hello typ av program eller känslighet hello data.

![Säkerhetsprinciper och säkerhetsrekommendationer](./media/azure-operational-security/azure-operational-security-fig5.png)


Principer som är aktiverade i hello prenumerationsnivån automatiskt sprida tooall resursgrupper i hello prenumeration som visas i hello diagram i hello höger:


### <a name="data-collection"></a>Datainsamling

Security Center samlar in data från virtuella datorer (VM)-tooassess sina säkerhetstillstånd anger säkerhetsrekommendationer och varna dig toothreats. När din första åtkomst Security Center, insamling av data är aktiverat på alla virtuella datorer i din prenumeration. Insamling av data rekommenderas, men du kan avanmäla dig genom att stänga av insamling av data i hello Security Center-princip.

### <a name="data-sources"></a>Datakällor

- Azure Security Center analyserar data från följande källor tooprovide synlighet i din säkerhetstillstånd hello, identifiera säkerhetsrisker och rekommenderar åtgärder och identifiera active hot:

-   Azure-tjänster: Använder information om hello konfiguration av Azure-tjänster som du har distribuerat genom att kommunicera med tjänstens resursprovidern.

- Nätverkstrafik: Använder samplade metadata för nätverkstrafiken från Microsofts infrastruktur, till exempel käll- och mål IP-adress/port, paketstorlek och nätverksprotokoll.

-   Partnerlösningar: Använder säkerhetsvarningar från integrerade partnerlösningar, till exempel brandväggar och lösningar mot skadlig kod.

-   Virtual Machines: Använder information om konfigurationer och säkerhetshändelser, till exempel händelser och granskningsloggar i Windows, IIS-loggar, syslog-meddelanden och kraschdumpfiler från dina virtuella datorer.

### <a name="data-protection"></a>Dataskydd

toohelp kunder förebygga, upptäcka och åtgärda toothreats, Azure Security Center samlar in och bearbetar säkerhetsrelaterade data, inklusive konfigurationsinformation, metadata, händelseloggar, kraschdumpfiler och mycket mer. Microsoft följer riktlinjer för toostrict efterlevnad och säkerhet – från kodning toooperating en tjänst.

-   **Dataavgränsning**: Data lagras logiskt separat på varje komponent i hela hello-tjänsten. Alla data taggas efter organisation. Den här taggning kvarstår under hello data livscykel och den tillämpas på varje nivå av hello-tjänsten.

-   **Dataåtkomst**: tooprovide säkerhetsrekommendationer och undersöka potentiella säkerhetshot, Microsoft-Personal kan komma åt information som samlas in eller analyseras av Azure-tjänster, inklusive kraschdumpfiler, bearbeta-skapande händelser, virtuell disk ögonblicksbilder och artefakter som kan oavsiktligt innehålla kundinformation eller personliga data från dina virtuella datorer. Vi följer toohello [Microsoft Online Services-licensvillkoren och sekretesspolicyn](http://www.microsoftvolumelicensing.com/DocumentSearch.aspx?Mode=3&DocumentTypeId=31), vilka tillstånd som inte är Microsoft använder kundinformation eller härledd information från den i reklam eller liknande kommersiellt syfte.

-   **Använd data**: Microsoft använder mönster och hotinformation ses över flera innehavare tooenhance våra förhindra och upptäcka funktioner; vi gör i enlighet med hello sekretess åtaganden som beskrivs i vår [sekretess Instruktionen](https://www.microsoft.com/privacystatement/OnlineServices/Default.aspx).

### <a name="data-location"></a>Dataplats

Azure Security Center samlar in tillfälliga kopior av dina kraschdumpfiler och analyserar dem efter tecken på kryphål och säkerhetsintrång. Azure Security Center utför den här analysen inom hello samma Geo som hello arbetsytan och tar bort hello tillfälliga kopior när analysen är klar. Datorn artefakter lagras centralt i hello samma region som hello VM.

-   **Storage-konton**: ett lagringskonto har angetts för varje region där virtuella datorer körs. Det här kan du toostore data i hello samma region som hello virtuell dator från vilken hello data samlas in.

-   **Azure Security Center lagring**: Information om säkerhetsaviseringar, inklusive partner aviseringar, rekommendationer och säkerhet hälsostatus lagras centralt, för närvarande i hello USA. Den här informationen kan omfatta relaterade konfigurationsinformation och säkerhetshändelser samlas in från dina virtuella datorer som behövs tooprovide du med hello säkerhet avisering, rekommendation eller säkerhet hälsostatus.


## <a name="azure-monitor"></a>Azure Monitor

Hej [OMS säkerhet](https://docs.microsoft.com/azure/operations-management-suite/oms-security-monitoring-resources) och Audit lösningen låter IT tooactively övervaka alla resurser som hjälper dig att minimera hello konsekvenserna av säkerhet. OMS säkerhet och granska har säkerhetsdomäner som kan användas för att övervaka resurser. hello säkerhetsdomän ger snabb åtkomst toooptions, för säkerhetsövervakning hello följande domäner beskrivs mer detaljerat:

-   kontroll av skadlig kod
-   Kontroll av uppdateringar
-   Identitets- och.

Azure-Monitor innehåller pekare tooinformation för vissa typer av resurser. Den erbjuder visualisering, fråga, routning, varningar, automatisk skalning och automation på data både från hello Azure-infrastrukturen (aktivitetsloggen) och varje enskild Azure resurs (diagnostikloggar).

![Azure Monitor](./media/azure-operational-security/azure-operational-security-fig6.png)


Molnprogram är komplicerade med många rörliga delar. Övervakning innehåller data tooensure tillämpningsprogrammet stanna upp och körs i ett felfritt tillstånd. Toostave av eventuella problem eller felsöka efter de som hjälper också till.

Du kan dessutom använda övervakning data toogain djupa insikter om ditt program. Denna kunskap kan hjälpa dig att tooimprove programprestanda eller underhålla eller automatisera åtgärder som annars skulle kräva manuella åtgärder.

### <a name="azure-activity-log"></a>Azure-aktivitetsloggen


Det är en logg som ger inblick i hello-åtgärder som utfördes på resurser i din prenumeration. hello aktivitetsloggen tidigare kallades ”granskningsloggar” eller ”operativa loggar” eftersom den rapporterar kontroll-plan händelser för dina prenumerationer.

![Azure-aktivitetsloggen](./media/azure-operational-security/azure-operational-security-fig7.png)

Använder hello aktivitetsloggen, kan du bestämma hello ' vad som, och när ' för alla skrivåtgärder (PUT, POST, ta bort) vidtagits hello resurser i din prenumeration. Du kan också förstå hello status för hello igen och andra relevanta egenskaper. hello aktivitetsloggen innehåller inte skrivskyddade (GET) åtgärder eller åtgärder för resurser som använder hello modell.

### <a name="azure-diagnostic-logs"></a>Azure diagnostikloggar

Dessa loggar orsakat av en resurs och ger omfattande, ofta data om hello åtgärd för resursen. hello innehållet i de här loggarna varierar beroende på resurstypen.

Till exempel Windows-händelsesystemloggar är en kategori av diagnostiska loggar för virtuella datorer och blob, tabell och kön loggar är kategorier av diagnostiska loggar för lagringskonton.

Diagnostik loggar skiljer sig från hello [aktivitetsloggen (kallades granskningsloggen eller arbetsloggen)](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-activity-logs). hello aktivitetsloggen ger inblick i hello-åtgärder som utfördes på resurser i din prenumeration. Diagnostik-loggarna ger information om åtgärder som din resurs utförde sig själv.

### <a name="metrics"></a>Mått

Azure-Monitor kan du tooconsume telemetri toogain insyn i hello prestanda och hälsotillståndet för dina arbetsbelastningar i Azure. hello är viktigaste Azure telemetridata hello mått (kallas även prestandaräknare) sänds av mest Azure-resurser. Övervakare för Azure tillhandahåller flera olika sätt tooconfigure och använda dessa [mått](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-metrics) för övervakning och felsökning. Mått är en del av telemetri och aktivera toodo hello följande uppgifter:

-   **Spåra hello prestanda** för din resurs (till exempel en VM, webbplats eller logik app) genom att rita upp dess mått i en portal diagram och fästa diagrammet tooa instrumentpanelen.

-   **Få ett meddelande om ett problem** att hello påverkan på prestandan för din resurs när en måttet överskrider ett visst tröskelvärde.

-   **Konfigurera automatiska åtgärder**, till exempel automatisk skalning av en resurs eller startar en runbook när en måttet överskrider ett visst tröskelvärde.

-   **Utföra avancerade analyser** eller rapportering på prestanda eller användning trender för din resurs.

-   **Arkivera** hello prestanda eller hälsa historiken för din resurs för kompatibilitet eller granskningsändamål.

### <a name="azure-diagnostics"></a>Azure Diagnostics

Det är hello funktion i Azure som gör att hello insamlingen av diagnostiska data på ett distribuerat program. Du kan använda hello diagnostik tillägg från olika olika källor. Stöds för närvarande är [Azure Cloud Service webb- och arbetsroller](https://docs.microsoft.com/azure/vs-azure-tools-configure-roles-for-cloud-service), [Azure Virtual Machines](https://docs.microsoft.com/azure/virtual-machines/windows/overview) med Microsoft Windows och [Service Fabric](https://docs.microsoft.com/azure/monitoring-and-diagnostics/azure-diagnostics). Andra Azure-tjänster har sina egna separata diagnostik.

## <a name="azure-network-watcher"></a>Azure Nätverksbevakaren

Det är viktigt för att identifiera säkerhetsproblem i nätverket och att säkerställa kompatibilitet med din IT-säkerhet och regelverk styrning modellen granskning nätverkssäkerheten. Med säkerhetsgruppen vy kan du hämta hello konfigureras regler för Nätverkssäkerhetsgruppen och säkerhet och hello effektiva säkerhetsregler. Du kan fastställa hello-portar som är öppna och utvärdera nätverket säkerhetsproblem med hello listan över regler tillämpas.

[Nätverk Watcher](https://docs.microsoft.com/azure/network-watcher/network-watcher-monitoring-overview#network-watcher) är en regionala tjänst som gör att du toomonitor och diagnostisera villkor på en nivå i till och från Azure. Diagnostiska nätverks- och visualiseringsverktyg som finns tillgängliga med Nätverksbevakaren hjälpa dig att förstå, diagnostisera och få insikter tooyour nätverk i Azure. Den här tjänsten innehåller paketinsamling, nästa hopp, IP-flöde Kontrollera NSG flödet loggar i gruppvyn säkerhet. Scenariot nivån övervakning innehåller en end tooend för nätverksresurser i kontrast tooindividual resurs nätverksövervakning.

![Azure Nätverksbevakaren](./media/azure-operational-security/azure-operational-security-fig8.png)

Nätverksbevakaren har för närvarande hello följande funktioner:

-   **<a href="https://docs.microsoft.com/azure/network-watcher/network-watcher-monitoring-overview">Granskningsloggar</a>**-åtgärder som utförs som en del av hello konfiguration av nätverk loggas. Dessa loggar kan visas i hello Azure-portalen eller hämtas med hjälp av Microsoft-verktyg, till exempel Power BI eller verktyg från tredje part. Granskningsloggar är tillgängliga via hello portal, PowerShell, CLI och Rest-API. Mer information om granskningsloggarna finns i Audit åtgärder med Resource Manager. Granskningsloggar är tillgängliga för åtgärder som utförs på alla nätverksresurser.


-   **<a href="https://docs.microsoft.com/azure/network-watcher/network-watcher-ip-flow-verify-overview">IP-flöde verifierar </a>**  -kontrollerar om ett paket tillåts eller nekas baserat på flödet information 5-tuppel paket parametrar (mål-IP, käll-IP, målport, källport och Protocol). Om hello paket nekas av en Nätverkssäkerhetsgrupp returneras hello regeln och Nätverkssäkerhetsgruppen som nekats hello paket.

-   **<a href="https://docs.microsoft.com/azure/network-watcher/network-watcher-next-hop-overview">Nästa hopp</a>**  -avgör hello next hop för paket som vidarebefordras i hello Azure nätverksinfrastruktur, aktiverar du toodiagnose alla felkonfigurerad användardefinierade vägar.

-   **<a href="https://docs.microsoft.com/azure/network-watcher/network-watcher-security-group-view-overview">Säkerhet gruppvyn</a>**  -hämtar hello effektiva och tillämpade säkerhetsregler som tillämpas på en virtuell dator.

-   **<a href="https://docs.microsoft.com/azure/network-watcher/network-watcher-nsg-flow-logging-overview">NSG flöda loggning</a>**  -flöde Nätverkssäkerhetsgrupper loggarna aktivera toocapture loggar relaterade tootraffic som tillåts eller nekas av hello säkerhetsregler i hello-gruppen. hello flödet definieras av en 5-tuppel information – käll-IP, mål-IP, källport, målport och protokoll.

## <a name="azure-storage-analytics"></a>Azure Storage-analys

[Storage Analytics](https://docs.microsoft.com/rest/api/storageservices/fileservices/storage-analytics) kan lagra mått som innehåller aggregerade statistik och kapacitet transaktionsdata om begäranden tooa storage-tjänst. Transaktioner rapporteras på både åtgärden hello API-nivå och hello storage-servicenivå och kapacitet rapporteras vid hello storage-servicenivå. Mätvärdesdata kan vara används tooanalyze lagringskvoten på tjänsten, diagnostisera problem med begäranden som görs mot hello storage-tjänst och tooimprove hello prestanda för program som använder en tjänst.

[Azure Storage Analytics](https://docs.microsoft.com/rest/api/storageservices/fileservices/storage-analytics) utför loggning och tillhandahåller data för mått för ett lagringskonto. Du kan använda den här tootrace begäranden, analysera användningstrender och diagnostisera problem med ditt lagringskonto. Storage Analytics loggning är tillgänglig för hello [Blob-, kö-och tabellen](https://docs.microsoft.com/azure/storage/storage-introduction). Storage Analytics loggar detaljerad information om lyckade och misslyckade begäranden tooa storage-tjänst.

Den här informationen kan vara används toomonitor enskilda förfrågningar och toodiagnose problem med en storage-tjänst. Begäranden loggas för bästa prestanda. Loggposter skapas bara om det finns begäranden som görs mot hello tjänstslutpunkten. För exempel om ett lagringskonto har aktivitet i sin slutpunkt för Blob men inte i tabellen eller kö slutpunkter, endast loggar som rör har toohello Blob-tjänsten skapats.

toouse Storage Analytics, måste du aktivera det individuellt för varje tjänst du vill toomonitor. Du kan aktivera den i hello [Azure-portalen](https://portal.azure.com/); mer information finns i [övervaka ett lagringskonto i hello Azure-portalen](https://docs.microsoft.com/azure/storage/storage-monitor-storage-account). Du kan också aktivera Storage Analytics programmässigt via hello REST API eller klientbibliotek för hello. Använd hello ange tjänstegenskaper åtgärden tooenable Storage Analytics individuellt för varje tjänst.

hello aggregerade data lagras i en välkänd blob (för loggning) och välkända tabeller (för mått) som kan användas med hello Blob- och tabelltjänsterna API: er.

Storage Analytics har en gräns på 20 TB på hello mängden lagrade data som är oberoende av hello totala gränsen för ditt lagringskonto. Alla loggar lagras i [blockblobbar](https://docs.microsoft.com/azure/storage/storage-analytics) i en behållare med namnet $logs, som skapas automatiskt när Storage Analytics har aktiverats för ett lagringskonto.

hello följande åtgärder som utförs av Storage Analytics är fakturerbar:

-   Begäranden toocreate BLOB för loggning
-   Begäranden toocreate tabellentiteter för mått.

> [!Note]
> Mer information om fakturering och datakvarhållningsprinciper finns [Storage Analytics och fakturering för](https://docs.microsoft.com/rest/api/storageservices/fileservices/storage-analytics-and-billing).
> För bästa prestanda bör du toolimit hello antalet hög utnyttjade diskar kopplade toohello virtuella tooavoid möjliga begränsning. Om alla diskar som hög inte används vid hello samtidigt hello lagringskonto stöder ett större antal disk.

> [!Note]
> Läs mer om lagringskontogränser [Azure Storage skalbarhets- och prestandamål](https://docs.microsoft.com/azure/storage/storage-scalability-targets).


följande typer av autentiserade och anonyma begäranden hello loggas.

| Autentiserad  | Anonym|
| :------------- | :-------------|
| Lyckade begäranden | Lyckade begäranden |
|Misslyckade begäranden, inklusive timeout, begränsning, nätverk, auktorisering och andra fel | Begäranden som använder en delad signatur åtkomst (SAS), inklusive misslyckade och lyckade begäranden |
| Begäranden som använder en delad signatur åtkomst (SAS), inklusive misslyckade och lyckade begäranden |Timeout-fel för både klient och server |
|   Begäranden tooanalytics data |    Misslyckade GET-begäranden med felkoden 304 (ändra inte) |
| Förfrågningar som görs av Storage Analytics, till exempel loggen skapas eller tas bort, loggas inte. En fullständig lista över hello loggade data dokumenteras i hello [Storage Analytics loggade åtgärder och statusmeddelanden](https://docs.microsoft.com/rest/api/storageservices/fileservices/storage-analytics-logged-operations-and-status-messages) och [Storage Analytics loggformatet](https://docs.microsoft.com/rest/api/storageservices/fileservices/storage-analytics-log-format) avsnitt. | Alla andra misslyckade anonyma begäranden har inte loggats. En fullständig lista över hello loggade data dokumenteras i hello [Storage Analytics loggade åtgärder och statusmeddelanden](https://docs.microsoft.com/rest/api/storageservices/fileservices/storage-analytics-logged-operations-and-status-messages) och [Storage Analytics loggformatet](https://docs.microsoft.com/rest/api/storageservices/fileservices/storage-analytics-log-format). |
## <a name="azure-active-directory"></a>Azure Active Directory

Azure AD innehåller också en fullständig uppsättning funktioner för hantering av identiteter inklusive multifaktorautentisering, registrering av enheten, självbetjäning lösenordshantering, grupphantering via självbetjäning, hantering av privilegierat konto, rollbaserad åtkomst kontroll, övervakning av programanvändning, omfattande granskning och säkerhetsövervakning och aviseringar.

-   Förbättra säkerheten för program med Azure AD-flerfunktionsautentisering och villkorlig åtkomst.

-   Övervaka programanvändning och skydda företaget från avancerade hot med säkerhet övervakning och rapportering.

Azure AD (Active Directory Azure) tillhandahåller säkerhets-, aktivitets- och granskningsrapporter för din katalog. [hello Azure Active Directory-kontrollrapport](https://docs.microsoft.com/azure/active-directory/active-directory-reporting-guide) hjälper kunder att tooidentify Privilegierade åtgärder som inträffade i sina Azure Active Directory. Privilegierade åtgärder omfattar höjning ändringar (till exempel skapa användarrollen eller återställning av lösenord), ändra principkonfigurationer (till exempel lösenordsprinciper) eller ändringar toodirectory konfigurationen (till exempel ändrar toodomain federation inställningar).

hello-rapporter tillhandahåller hello Granskningspost för hello händelsenamnet, hello aktören som utförde hello åtgärd, hello målresurs påverkas av hello ändringen och hello datum och tid (i UTC). Kunder är kan tooretrieve hello lista över granskningshändelser för sina Azure Active Directory via hello [Azure-portalen](https://portal.azure.com/), enligt beskrivningen i [visa granskningsloggarna](https://docs.microsoft.com/azure/active-directory/active-directory-reporting-azure-portal). Här är en lista över rapporter hello:

| Säkerhetsrapporter  | Aktivitetsrapporter| Granskningsrapporter |
| :------------- | :-------------| :-------------|
|Inloggningar från okända källor | Programanvändning: sammanfattning | Kataloggranskningsrapport |
|Inloggningar efter flera fel | Programanvändning: detaljerad |   |
|Inloggningar från flera geografiska områden | Instrumentpanel för program |  |
|Inloggningar från IP-adresser med misstänkt aktivitet |Kontoetableringsfel |  |
|Oregelbunden inloggningsaktivitet |Enskilda användarenheter |  |
|Inloggningar från potentiellt infekterade enheter |Aktivitet för enskilda användare |   |
|Användare med avvikande inloggningsaktivitet |Aktivitetsrapport för grupper |   |
| |Aktivitetsrapport över registrering av lösenordsåterställning |   |
| |Lösenordsåterställningsaktivitet |   | |



hello data för de här rapporterna kan vara användbart tooyour program, till exempel SIEM-system, granskning och business intelligence-verktyg. hello Azure AD reporting [API: er](https://docs.microsoft.com/azure/active-directory/active-directory-reporting-api-getting-started) ger Programmeringsåtkomst toohello data via en uppsättning REST-baserad API: er. Du kan anropa API: erna från olika programmeringsspråk språk och verktyg.

Händelser i hello Azure AD granska rapporten finns kvar i 180 dagar.

> [!Note]
> Mer information om kvarhållning i rapporter finns [Azure Active Directory Rapportkvarhållningsregler](https://docs.microsoft.com/azure/active-directory/active-directory-reporting-retention).

För kunder som vill lagra sina [granskningshändelser](https://docs.microsoft.com/azure/active-directory/active-directory-reporting-audit-events) för längre kvarhållningsperioder hello Reporting API kan använda tooregularly pull granskningshändelser i ett separat datalager.

## <a name="summary"></a>Sammanfattning

Den här artikeln sammanfattningar skydda din integritet och skydda dina data, samtidigt som de levererar programvaror och tjänster som hjälper dig hantera hello IT-infrastrukturen i organisationen. Microsoft förstår att när de överlåta sina data tooothers förtroendet kräver omfattande säkerhet. Microsoft följer riktlinjer för toostrict efterlevnad och säkerhet – från kodning toooperating en tjänst. Säkra och skydda data är högsta prioritet på Microsoft.

Den här artikeln

-   Hur data som samlas in, bearbetas och skyddas i hello Operations Management Suite (OMS).

-   Analysera händelser snabbt via flera datakällor. Identifiera säkerhetsrisker och förstå hello scope och effekten av hot och attacker toomitigate hello skador av ett säkerhetsintrång.

-   Identifiera attackmönster genom att visualisera utgående IP-trafik och typer av skadliga hot. Förstå hello säkerhetstillståndet av hela miljön oavsett plattform.

-   Samla in alla hello loggen och händelsen data som krävs för en säkerhets- eller efterlevnad granskning. Behövs toosupply en säkerhets-och granska med en fullständig sökbara och exportera loggen och händelsen datauppsättning snedstreck hello tid och resurser.

<ul>
<li>Samla in säkerhetshändelser, granska och intrång analys tookeep en Stäng ögon dina tillgångar:</li>
<ul>
<li>Säkerhet</li>
<li>Viktiga problem</li>
<li>Sammanfattningar hot</li>
</ul>
</ul>

## <a name="next-steps"></a>Nästa steg

- [Design- och operativ säkerhet](https://www.microsoft.com/trustcenter/security/designopsecurity)

Microsoft utformar dess tjänster och program med säkerhet i åtanke toohelp se till att dess molninfrastruktur är flexibel och försvaras från attacker.

- [Operations Management Suite | Säkerhet och efterlevnad](https://www.microsoft.com/cloud-platform/security-and-compliance)

Använd Microsoft security data och analys tooperform mer intelligent och effektivt hotidentifiering.

- [Planera för Azure Security Center och åtgärder](https://docs.microsoft.com/azure/security-center/security-center-planning-and-operations-guide) en uppsättning steg och aktiviteter som du kan följa toooptimize din användning av Security Center utifrån din organisations säkerhetskrav och molnhanteringsmodell.

