---
title: "aaaAzure operativ Säkerhetsöversikt | Microsoft Docs"
description: "Den här artikeln innehåller en översikt över hello Azure operativ säkerhet."
services: security
documentationcenter: na
author: unifycloud
manager: swadhwa
editor: tomsh
ms.assetid: 
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/27/2017
ms.author: tomsh
ms.openlocfilehash: b91c7889660b32e4933c305007692bd6e1ded05f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-operational-security-overview"></a>Översikt över Azure operativ säkerhet
Azure operativ säkerhet refererar toohello tjänster, kontroller och funktioner tillgängliga toousers för att skydda sina data, program och andra resurser i Microsoft Azure. [Azure operativ säkerhet](https://docs.microsoft.com/azure/security/azure-operational-security) är ett ramverk som inkluderar hello knowledge som gjorts via en mängd funktioner som är unika tooMicrosoft, inklusive hello Microsoft Security Development Lifecycle (SDL), hello Microsoft Security Response Center programmet och djup medvetenhet om hello cyber security hotbild.

Den här Azure Operational översikt över säkerheten i artikeln fokuserar på hello följande områden:

- Azure Operations Management Suite
-   Azure Security Center
-   Azure Monitor
-   Azure nätverksbevakaren
-   Azure Storage analytics
-   Azure Active directory

## <a name="azure-operations-management-suite"></a>Azure Operations Management Suite
IT-avdelningen ansvarar för att hantera datacenter infrastruktur, program och data, inklusive hello stabilitet och säkerhet för dessa system. Få säkerhetsinsikter över öka komplexa IT-miljöer ofta kräver dock organisationer toocobble ihop data från flera system för säkerhet och hantering.

[Microsoft Operations Management Suite (OMS)](https://docs.microsoft.com/azure/operations-management-suite/operations-management-suite-overview) är Microsofts molnbaserade IT lösning som hjälper dig att hantera och skydda dina lokala och molnet infrastruktur.

OMS är en molnbaserad IT lösning med många erbjudanden, till exempel IT Automation, säkerhet och efterlevnad, Log Analytics och säkerhetskopiering och återställning. Därför det är en perfekt stöd toomanage och skydda din IT-infrastruktur, lokalt och i hello molnet.

hello huvudfunktionerna i OMS tillhandahålls av en uppsättning tjänster som körs i Azure. Varje tjänst innehåller en funktion för specifika hanteringsservern och du kan kombinera olika hanteringsscenarier för tjänster tooachieve. Som innehåller:

-   Log Analytics
-   Automation
-   Backup
-   Webbplatsåterställning

### <a name="log-analytics"></a>Log Analytics
[Log Analytics](http://azure.microsoft.com/documentation/services/log-analytics) tillhandahåller övervakning för OMS genom att samla in data från hanterade resurser i en central databas. Dessa data kan omfatta händelser, prestandadata, eller anpassade data som tillhandahålls via hello API. När samlas in, är hello data tillgängliga för aviseringar, analys och export. Den här metoden kan du tooconsolidate data från olika källor så kan du kombinera data från Azure-tjänster med din befintliga lokala miljö. Den också tydligt skiljer hello samling hello data från hello åtgärd på dessa data så att alla åtgärder är tillgängliga tooall typer av data.

### <a name="automation"></a>Automation
Microsoft [Azure Automation](https://docs.microsoft.com/azure/automation/automation-intro) gör det möjligt för användare tooautomate hello manuell, långvariga, felbenägna och ofta återkommande uppgifter som utförs ofta i en miljö med molnet och företagets. Det sparar tid och ökar tillförlitligheten hello vanliga administrativa uppgifter och även schemalägger dem toobe automatiskt utföras med jämna mellanrum. Du kan automatisera processer med hjälp av runbooks eller automatisera konfigurationshantering med Desired State Configuration.

### <a name="backup"></a>Säkerhetskopiering
[Azure-säkerhetskopiering](https://docs.microsoft.com/azure/backup/backup-introduction-to-azure-backup) är hello Azure-baserad tjänst som du kan använda tooback (eller skydda) och återställa data i hello Microsoft cloud. Azure Backup ersätter din befintliga lokala eller externa säkerhetskopieringslösning med en tillförlitlig och säker molnbaserad lösning med ett konkurrenskraftigt pris. Azure Backup erbjuder flera komponenter som du kan hämta och distribuera på hello lämplig dator, server, eller i hello molnet. hello komponenten eller en agent som du distribuerar beror på vad du vill tooprotect. Alla Azure Backup-komponenter (oavsett om du skyddar data lokalt eller i molnet hello) kan vara används tooback upp data tooa Recovery Services-valvet i Azure. Se hello [Azure Backup komponenter tabellen](https://docs.microsoft.com/azure/backup/backup-introduction-to-azure-backup#which-azure-backup-components-should-i-use).

### <a name="site-recovery"></a>Webbplatsåterställning
[Azure Site Recovery](http://azure.microsoft.com/documentation/services/site-recovery) ger affärskontinuitet genom att samordna replikeringen av lokala virtuella och fysiska datorer tooAzure eller tooa sekundär plats. Om den primära platsen är tillgänglig, växlar du över toohello sekundära platsen så att användarna kan hålla fungerande och växla tillbaka när system returnerar tooworking ordning. Intelligent och effektiv hotidentifiering.

## <a name="azure-active-directory"></a>Azure Active Directory
[Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-enable-sso-scenario) är Microsofts omfattande identitet som en tjänst (IDaaS) som:

-   Aktiverar IAM som en tjänst i molnet
-   Tillhandahåller centrala hantering, enkel inloggning (SSO) och rapportering
-   Stöder integrerad åtkomsthantering för [tusentals program](https://azure.microsoft.com/marketplace/active-directory/) i hello programgalleriet, inklusive Salesforce, Google Apps, rutan, Concur och mycket mer.

Azure AD innehåller också en fullständig uppsättning [identitetshanteringsfunktionerna](https://docs.microsoft.com/azure/security/security-identity-management-overview#security-monitoring-alerts-and-machine-learning-based-reports) inklusive [multifaktorautentisering](https://docs.microsoft.com/azure/multi-factor-authentication/multi-factor-authentication), [enhetsregistrering]( https://docs.microsoft.com/azure/active-directory/active-directory-device-registration-overview), [självbetjäning lösenordshantering](https://azure.microsoft.com/resources/videos/self-service-password-reset-azure-ad/), [grupphantering via självbetjäning](https://docs.microsoft.com/azure/active-directory/active-directory-passwords-update-your-own-password), [privilegierad kontohantering](https://docs.microsoft.com/azure/active-directory/active-directory-privileged-identity-management-configure), [rollbaserad åtkomstkontroll](https://docs.microsoft.com/azure/active-directory/role-based-access-control-what-is), [övervakning av programanvändning](https://docs.microsoft.com/azure/active-directory/connect-health/active-directory-aadconnect-health), [omfattande granskning](https://docs.microsoft.com/azure/active-directory/active-directory-reporting-activity-audit-logs), och [säkerhet övervakning och avisering](https://docs.microsoft.com/azure/operations-management-suite/oms-security-responding-alerts).

Med Azure Active Directory, alla program som du publicerar för partner och kunder (business eller konsumenten) har hello samma funktioner för hantering av identiteter och åtkomst. Det här kan du toosignificantly minska din driftskostnader.

## <a name="azure-security-center"></a>Azure Security Center
[Azure Security Center](https://docs.microsoft.com/azure/security-center/security-center-get-started) kan du förebygga, upptäcka och svara toothreats med bättre överblick och kontroll över hello säkerheten för dina Azure-resurser. Härifrån kan du övervaka och hantera principer för alla prenumerationer på en gång och upptäcka hot som annars kanske skulle förbli oupptäckta. Azure Security Center fungerar tillsammans med ett vittomfattande ekosystem med säkerhetslösningar.

[Security Center](https://docs.microsoft.com/azure/security-center/security-center-linux-virtual-machine) kan du skydda data för virtuell dator i Azure genom att tillhandahålla insyn i den virtuella datorns säkerhetsinställningar och övervakning för hot. Security Center kan övervaka dina virtuella datorer för:

-   Operativsystem (OS) säkerhetsinställningar med hello rekommenderas konfigurationsregler
-   Systemsäkerhet och viktiga uppdateringar som saknas
-   Rekommendationer för slutpunktsskydd
-   Verifiering av diskkryptering
-   Nätverksbaserade attacker

Azure Security Center använder [rollbaserad åtkomstkontroll (RBAC)](https://docs.microsoft.com/azure/active-directory/role-based-access-control-configure), vilket ger [inbyggda roller](https://docs.microsoft.com/azure/active-directory/role-based-access-built-in-roles) som kan tilldelas toousers, grupper och tjänster i Azure.

Security Center utvärderar hello konfigurationen av dina resurser tooidentify säkerhetsproblem och säkerhetsproblem. I Security Center, du kan bara se information relaterade tooa resurs när du har tilldelats hello rollen ägare, deltagare eller läsare för hello prenumerationen eller resursen gruppen som en resurs tillhör.

>[!Note]
>Se [behörigheter i Azure Security Center](https://docs.microsoft.com/azure/security-center/security-center-permissions) toolearn mer om roller och tillåtna åtgärder i Security Center.

Security Center använder hello Microsoft Monitoring Agent – detta är hello samma agent används hello Operations Management Suite och Log Analytics-tjänsten. Data som samlas in från den här agenten lagras i en befintlig logganalys [arbetsytan](https://docs.microsoft.com/azure/log-analytics/log-analytics-manage-access) som är associerade med din Azure-prenumeration eller en ny arbetsytor med hänsyn till kontot hello geolokalisering av hello VM.

## <a name="azure-monitor"></a>Azure Monitor
Prestandaproblem i din molnapp kan påverka din verksamhet. Med flera sammankopplade komponenter och ofta versioner kan degradations inträffa när som helst. Och om du utvecklar en app användarna identifiera vanligtvis problem som du inte kan hitta vid testning. Du bör känna till de här problemen omedelbart och har verktyg för att diagnostisera och åtgärda problem med hello.

[Azure-Monitor](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-azure-monitor) är grundläggande verktyg för att övervaka tjänster som körs på Azure. Den ger dig infrastrukturnivå data om hello genomflödet av en tjänst och hello omgivande miljö. Om du hanterar dina appar i Azure, bestämmer om tooscale uppåt eller nedåt resurser sedan Azure-Monitor ger dig vad du använder toostart.

Du kan dessutom använda övervakning data toogain djupa insikter om ditt program. Denna kunskap kan hjälpa dig att tooimprove programprestanda eller underhålla eller automatisera åtgärder som annars skulle kräva manuella åtgärder. Det innehåller:

-   Azure-aktivitetsloggen
-   Azure diagnostikloggar
-   Mått
-   Azure Diagnostics

### <a name="azure-activity-log"></a>Azure-aktivitetsloggen
Det är en logg som ger inblick i hello-åtgärder som utfördes på resurser i din prenumeration. Hej [aktivitetsloggen](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-activity-logs) kallades tidigare ”granskningsloggar” eller ”operativa loggar” eftersom den rapporterar kontroll-plan händelser för dina prenumerationer.

### <a name="azure-diagnostic-logs"></a>Azure diagnostikloggar
[Azure diagnostikloggar](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs) orsakat av en resurs och ger omfattande, ofta data om hello åtgärd för resursen. hello innehållet i de här loggarna varierar beroende på resurstypen.

Till exempel Windows-händelsesystemloggar är en kategori av diagnostiska loggar för virtuella datorer och blob, tabell och kön loggar är kategorier av diagnostiska loggar för lagringskonton.

Diagnostik loggar skiljer sig från hello [aktivitetsloggen (kallades granskningsloggen eller arbetsloggen)](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-activity-logs). hello aktivitetsloggen ger inblick i hello-åtgärder som utfördes på resurser i din prenumeration. Diagnostik-loggarna ger information om åtgärder som din resurs utförde sig själv.

### <a name="metrics"></a>Mått
Azure-Monitor kan du tooconsume telemetri toogain insyn i hello prestanda och hälsotillståndet för dina arbetsbelastningar i Azure. hello är viktigaste Azure telemetridata hello mått (kallas även prestandaräknare) sänds av mest Azure-resurser. Övervakare för Azure tillhandahåller flera olika sätt tooconfigure och använda dessa [mått](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-metrics) för övervakning och felsökning.

### <a name="azure-diagnostics"></a>Azure Diagnostics
Det är hello funktion i Azure som gör att hello insamlingen av diagnostiska data på ett distribuerat program. Du kan använda hello diagnostik tillägg från olika olika källor. Stöds för närvarande är [Azure Cloud Service webb- och arbetsroller](https://docs.microsoft.com/azure/vs-azure-tools-configure-roles-for-cloud-service), [Azure Virtual Machines](https://docs.microsoft.com/azure/vs-azure-tools-configure-roles-for-cloud-service) med Microsoft Windows och [Service Fabric](https://docs.microsoft.com/azure/monitoring-and-diagnostics/azure-diagnostics).


## <a name="network-watcher"></a>Network Watcher
Kunder kan du skapa ett nätverk i Azure genom att samordna och skapa olika enskilda nätverksresurser, till exempel virtuella nätverk, ExpressRoute, Programgateway, belastningsutjämnare och flera. Övervakning är tillgängligt på varje hello nätverksresurser.

Hej tooend nätverk kan ha komplexa konfigurationer och samverkan mellan resurser, skapa komplicerade scenarier som behöver scenariobaserade övervakning via Nätverksbevakaren.

[Nätverk Watcher](https://docs.microsoft.com/azure/network-watcher/network-watcher-monitoring-overview) kommer förenklar övervakning och diagnos av Azure-nätverk. Diagnostik- och visualisering verktyg som är tillgängliga med Nätverksbevakaren aktivera du tootake remote paket samlar in på en Azure-dator, insyn i nätverkstrafiken med flödet loggar och diagnostisera VPN-Gateway och anslutningar.

Nätverksbevakaren har för närvarande hello följande funktioner:

- [Topologi](https://docs.microsoft.com/azure/network-watcher/network-watcher-topology-overview) -tillhandahåller ett nätverk nivån som visar hello olika anslutningarna och associationer mellan nätverksresurser i en resursgrupp.
-   [Variabeln paketinsamling](https://docs.microsoft.com/azure/network-watcher/network-watcher-packet-capture-overview) -samlar in paketdata till och från en virtuell dator. Avancerade filter alternativ och finjustera kontroller, till exempel som kan tooset tid och storleksbegränsningar ger flexibilitet. hello paketdata kan lagras i en blobstore eller på hello lokal disk i CAP-format.
-   [Kontrollera IP-flöden](https://docs.microsoft.com/azure/network-watcher/network-watcher-ip-flow-verify-overview) -kontrollerar om ett paket tillåts eller nekas baserat på flödet information 5-tuppel paket parametrar (mål-IP, käll-IP, målport, källport och Protocol). Om hello paket nekas av en säkerhetsgrupp returneras hello regeln och grupp som nekas hello paket.
-   [Nästa hopp](https://docs.microsoft.com/azure/network-watcher/network-watcher-next-hop-overview) -avgör hello next hop för paket som vidarebefordras i hello Azure nätverksinfrastruktur, aktiverar du toodiagnose alla felkonfigurerad användardefinierade vägar.
-   [Säkerhet gruppvyn](https://docs.microsoft.com/azure/network-watcher/network-watcher-security-group-view-overview) -hämtar hello effektiva och tillämpade säkerhetsregler som tillämpas på en virtuell dator.
-   [NSG flöda loggning](https://docs.microsoft.com/azure/network-watcher/network-watcher-nsg-flow-logging-overview) -flöde Nätverkssäkerhetsgrupper loggarna aktivera toocapture loggar relaterade tootraffic som tillåts eller nekas av hello säkerhetsregler i hello-gruppen. hello flödet definieras av en 5-tuppel information – käll-IP, mål-IP, källport, målport och protokoll.
-   [Virtuella Nätverksgatewayen och anslutningen felsökning](https://docs.microsoft.com/azure/network-watcher/network-watcher-troubleshoot-manage-rest) -ger hello möjlighet tootroubleshoot virtuella Nätverksgatewayer och anslutningar.
-   [Nätverk prenumerationsbegränsningar](https://docs.microsoft.com/azure/network-watcher/network-watcher-monitoring-overview) -aktiverar tooview nätverksresursanvändning mot gränser.
-   [Konfigurera diagnostik loggen](https://docs.microsoft.com/azure/network-watcher/network-watcher-monitoring-overview) – ger en enda tooenable eller inaktivera diagnostik loggar för nätverksresurser i en resursgrupp.

toolearn mer hur tooconfigure nätverksbevakaren Se [konfigurera nätverksbevakaren](https://docs.microsoft.com/azure/network-watcher/network-watcher-create).

## <a name="developer-operations-devops"></a>Utvecklare Operations (DevOps)
Tidigare tooDevOps programutveckling team var ansvarig för insamling av affärskraven för ett program och skriva kod. Sedan testar ett separat team QA hello program i en isolerad utvecklingsmiljö om kraven var uppfyllda och versioner hello koden för operations toodeploy. hello Distributionsgrupperna är ytterligare fragmenterad i ”magasin” grupper som nätverk och databasen. Varje gång ett program ”genereras över hello brandvägg” tooan oberoende team läggs flaskhalsar.

[DevOps](https://www.visualstudio.com/learn/what-is-devops/) aktiverar team toodeliver säkrare högre kvalitet lösningar snabbare och billigare. Kunder förväntar sig en dynamisk och tillförlitlig upplevelse när förbrukar programvaror och tjänster.  Team måste snabbt iterera programuppdateringar, mäta hello effekten av hello uppdateringar och snabbt svara med nya development iterationer tooaddress problem eller ger mer värde.  Molnplattformar, till exempel Microsoft Azure har bort traditionella flaskhalsar och hjälpt commoditize infrastruktur. Programvara prestandarik i alla företag som hello nyckelfaktorn och faktor i affärsresultatet. Ingen organisation, utvecklare och IT-arbetare kan eller Undvik hello DevOps förflyttning.

Mogen jobbar med DevOps anta flera hello följande metoder. Dessa metoder [involvera personer](https://www.visualstudio.com/learn/what-is-devops-culture/) tooform strategier baserat på hello verksamhets scenarier.  Verktygsuppsättning kan hjälpa dig att automatisera hello olika metoder:

-   [Flexibel planering och projekthantering](https://www.visualstudio.com/learn/what-is-agile/) tekniker är används tooplan och isolera arbete i sprints, hantera team kapacitet och hjälpa grupper snabbt anpassa toochanging affärsbehov.
-   [Versionskontroll, vanligtvis med Git](https://www.visualstudio.com/learn/what-is-git/), aktiverar team som helst i hello world tooshare källa och integrera med software development tools tooautomate hello versionen pipeline.
-   [Kontinuerlig Integration](https://www.visualstudio.com/learn/what-is-continuous-integration/) enheter hello pågående sammanslagning och testning av kod som leder toofinding fel tidigt.  Andra fördelar mindre tid som gått förlorat på bekämpa merge problem och snabb feedback för utvecklingsgrupper.
-   [Kontinuerlig leverans](https://www.visualstudio.com/learn/what-is-continuous-delivery/) lösningar software tooproduction- och testmiljöer hjälpa organisationer att snabbt åtgärda buggar och hantera ändring av tooever business krav.
-   [Övervaka](https://www.visualstudio.com/learn/what-is-monitoring/) inklusive produktionsmiljöer för programmets hälsotillstånd som program som körs som kundens användning hjälp organisationer formulär plus en hypotes snabbt validera och disprove strategier.  Omfattande data fångas in och lagras i olika format för loggning.
-   [Infrastruktur som kod (IaC)](https://www.visualstudio.com/learn/what-is-infrastructure-as-code/) är en metod, vilket gör att hello automation och verifiering av skapande och teardown av nätverk och virtuella datorer toohelp med att leverera säker och stabil program som är värd för plattformar.
-   [Mikrotjänster](https://www.visualstudio.com/learn/what-are-microservices/) arkitektur är balanserad tooisolate användningsområden i små återanvändbara tjänster.  Den här arkitekturen möjliggör skalbarhet och effektivitet.

## <a name="next-steps"></a>Nästa steg
toolearn mer om OMS säkerhets- och granska lösningen finns hello följande artiklar:

- [Operations Management Suite | Säkerhet och efterlevnad](https://www.microsoft.com/cloud-platform/security-and-compliance).
- [Övervakning och svarar tooSecurity aviseringar i Operations Management Suite säkerhet och granska lösningen](https://docs.microsoft.com/en-us/azure/operations-management-suite/oms-security-responding-alerts).
- [Övervakning av resurser i Operations Management Suite säkerhet och granska lösningen](https://docs.microsoft.com/en-us/azure/operations-management-suite/oms-security-monitoring-resources).
