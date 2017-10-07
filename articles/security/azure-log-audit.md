---
title: aaaAzure loggning och granskning | Microsoft Docs
description: "Läs mer om hur du kan använda loggning data toogain djupa insikter om ditt program."
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
ms.openlocfilehash: d0e817b071962ad9bef6250267092b5f9282bc7a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-logging-and-auditing"></a>Azure-loggning och granskning
## <a name="introduction"></a>Introduktion
### <a name="overview"></a>Översikt
tooassist aktuella och framtida Azure-kunder för att förstå och använda hello olika säkerhetsrelaterade funktioner tillgängliga i och omgivande hello Azure-plattformen Microsoft har utvecklat en serie faktablad, säkerhet översikter, bästa praxis och checklistor. Hej avsnitt angivet bredd och djup och uppdateras regelbundet. Det här dokumentet är en del av den serien som sammanfattas i hello efter abstrakt avsnittet.
### <a name="azure-platform"></a>Azure-plattformen
Azure är en öppen och flexibla molntjänstplattform som stöder hello stort urval av operativsystem, programmeringsspråk, ramverk, verktyg, databaser, och enheter.

Du kan till exempel:
-   Kör Linux behållare med Docker-integration.

-   Skapa appar med JavaScript, Python, .NET, PHP, Java och Node.js

-   Build-servrar för iOS, Android och Windows enheter.

Offentliga Azure-molnet och support hello samma tekniker miljoner utvecklare och IT-proffs redan förlitar sig på och litar på.

När du bygger på eller migrera IT tillgångar till en molntjänstleverantör, måste den organisationens förmåga tooprotect dina program och data med hello tjänster och hello kontroller ger toomanage hello säkerheten för din molnbaserade tillgångar.

Azures infrastruktur har utformats från hello anläggning tooapplications som värd för miljontals kunder samtidigt och det ger en säker grund som företag uppfyller deras behov. Dessutom Azure ger dig en mängd olika konfigurerbara säkerhet alternativ och hello möjlighet toocontrol dem så att du kan anpassa toomeet hello unika säkerhetskrav för dina distributioner. Det här dokumentet kommer hjälper dig att möta dessa krav.

### <a name="abstract"></a>Abstrakt
Granskning och loggning av säkerhetsrelaterade händelser och relaterade aviseringar är viktiga komponenter i en strategi för skydd av effektiva data. Säkerhetsloggar och rapporterna ger dig en elektronisk post av misstänkta aktiviteter och hjälp dig att upptäcka mönster som kan indikera försök eller lyckad externa intrång av hello nätverk som interna attacker. Du kan använda användaraktivitet granskning toomonitor, dokumentet regelefterlevnad, utföra kriminaltekniska analyser och mycket mer. Omedelbar avisering med aviseringar när säkerhetshändelser inträffar.

Microsoft Azure-tjänster och produkter som ger dig konfigurerbara säkerhetsgranskning och loggning alternativ toohelp du identifiera brister i säkerhetsprinciper och mekanismer och åtgärda dessa luckor toohelp förhindra överträdelser. Microsoft-tjänster erbjuder vissa (och i vissa fall alla) av följande alternativ för hello: centraliserad övervakning, loggning och analys system tooprovide kontinuerlig synlighet; rimlig aviseringar; och rapporter toohelp som du hanterar hello stor mängd information som genereras av enheter och tjänster.

Microsoft Azure-loggdata kan vara exporterade tooSecurity Incident och händelsen Management SIEM ()-system för analys och integreras med granskning tredjepartslösningar.

Den här rapporten innehåller en introduktion för generering, samla in och analysera säkerhetsloggar från i Azure-tjänster och kan hjälpa dig få säkerhetsinsikter om dina Azure-distributioner. hello omfånget för den här vitboken är begränsad tooapplications och inbyggda och distribuerade i Azure services.

> [!Note]
> Vissa rekommendationerna häri kan leda till ökad data, nätverk eller beräkning Resursanvändning och öka kostnaderna licens eller prenumeration.

## <a name="types-of-logs-in-azure"></a>Typer av loggar i Azure
Molnprogram är komplicerade med många rörliga delar. Loggarna ger data tooensure tillämpningsprogrammet stanna upp och körs i ett felfritt tillstånd. Toostave av eventuella problem eller felsöka efter de som hjälper också till. Du kan dessutom använda loggning data toogain djupa insikter om ditt program. Denna kunskap kan hjälpa dig att tooimprove programprestanda eller underhålla eller automatisera åtgärder som annars skulle kräva manuella åtgärder.

Azure ger utförlig loggning för varje Azure-tjänst. Dessa loggar kategoriseras av dessa typer:
-   **Kontroll och hantering loggar** ger inblick i hello Azure Resource Manager skapa-, UPDATE- och DELETE-åtgärder. [Azure aktivitetsloggar](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-activity-logs) är ett exempel på den här typen av loggen.

-   **Data plan loggar** ger inblick i hello händelser aktiveras som en del av hello användning av en Azure-resurs. Exempel på den här typen av loggen är hello Windows-händelse System, säkerhet, och programloggar i en virtuell dator och hello [diagnostik loggar](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs) konfigureras via Azure-Monitor


-   **Bearbetade händelser** ge information om analyserade/aviseringar som har bearbetats å dina vägnar. Exempel på den här typen är [Azure Security Center-aviseringar](https://docs.microsoft.com/azure/security-center/security-center-managing-and-responding-alerts) där [Azure Security Center](https://docs.microsoft.com/azure/security-center/security-center-intro) har bearbetats analyseras och din prenumeration och ger kortare säkerhetsaviseringar

hello den följande tabellen viktigaste listtypen loggar som är tillgängliga i Azure.

| Loggen kategori | Loggtyp | Användningsområden | Integrering |
| ------------ | -------- | ------ | ----------- |
|[Aktivitetsloggar](https://docs.microsoft.com/en-us/azure/monitoring-and-diagnostics/monitoring-overview-activity-logs)|Kontroll-plan händelser på Azure Resource Manager-resurser| Ger kunskaper om hello-åtgärder som utfördes på resurser i din prenumeration.|   REST-API: T & [Azure Övervakare](https://docs.microsoft.com/en-us/azure/monitoring-and-diagnostics/monitoring-overview-activity-logs)|
|[Azure diagnostikloggar](https://docs.microsoft.com/en-us/azure/monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs)|ofta data om hello driften av Azure Resource Manager resurserna i prenumerationen| Ger information om åtgärder som din resurs utförde själva| Övervakare för Azure [dataström](https://docs.microsoft.com/en-us/azure/monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs)|
|[AAD-rapportering](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-reporting-azure-portal)|Loggar och rapporter|Logga in användaraktiviteter & System aktivitetsinformation om användare och grupphantering|[Graph API](https://docs.microsoft.com/en-us/azure/active-directory/develop/active-directory-graph-api-quickstart)|
|[Virtuella & molntjänster](https://docs.microsoft.com/en-us/azure/cloud-services/cloud-services-dotnet-diagnostics-storage)|Windows-händelseloggen & Linux Syslog|  Samlar in systemdata och loggningsdata på hello virtuella datorer och överför data till ett lagringskonto som du väljer.| Windows använder [BOMULLSTUSS](https://docs.microsoft.com/en-us/azure/azure-diagnostics) (Windows Azure-diagnostik lagring) och Linux i Azure-monitor|
|[Lagringsanalys](https://docs.microsoft.com/en-us/rest/api/storageservices/fileservices/storage-analytics)|Loggning för lagring och ger mätvärdesdata för ett lagringskonto|Ger inblick i trace-begäranden, analysera användningstrender och diagnostisera problem med ditt lagringskonto.|  REST-API eller hello [klientbibliotek](https://msdn.microsoft.com/en-us/library/azure/mt347887.aspx)|
|[Loggar för flödet av NSG (Nätverkssäkerhetsgrupp)](https://docs.microsoft.com/en-us/azure/network-watcher/network-watcher-nsg-flow-logging-overview)|JSON-format och visar utgående och inkommande flöden på grundval av per regel|Visa information om ingående och utgående IP-trafik via en Nätverkssäkerhetsgrupp|[Nätverksbevakaren](https://docs.microsoft.com/en-us/azure/network-watcher/network-watcher-monitoring-overview)|
|[Application Insights](https://docs.microsoft.com/en-us/azure/application-insights/app-insights-overview)|Loggar, undantag och anpassade diagnostik|  Performance Management (APM) programtjänsten för webbutvecklare på flera plattformar.| REST API [Power BI](https://powerbi.microsoft.com/en-us/documentation/powerbi-azure-and-power-bi/)|
|Bearbeta Data / Säkerhetsvarning| Azure Security Center-avisering, OMS-avisering| Information om säkerhet och aviseringar.|   REST API: er, JSON|

### <a name="activity-log"></a>Aktivitetslogg
Hej [Azure-aktivitetsloggen](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-activity-logs), ger inblick i hello-åtgärder som utfördes på resurser i din prenumeration. hello aktivitetsloggen kallades tidigare ”granskningsloggar” eller ”operativa loggar” eftersom den rapporterar [kontroll-plan händelser](https://driftboatdave.com/2016/10/13/azure-auditing-options-for-your-custom-reporting-needs/) för dina prenumerationer. Använder hello aktivitetsloggen, kan du bestämma hello ”vad som, och när” för alla skrivåtgärder (PUT, POST, ta bort) vidtagits hello resurser i din prenumeration. Du kan också förstå hello status för hello igen och andra relevanta egenskaper. hello aktivitetsloggen inkluderar inte läsåtgärder (GET).

Här refererar PUT, POST, ta bort tooall hello skrivåtgärder aktivitetsloggen innehåller på hello resurser. Du kan till exempel använda hello aktivitet loggar toofind ett fel när du felsöker eller toomonitor hur en användare i din organisation ändrades en resurs.

![Aktivitetslogg](./media/azure-log-audit/azure-log-audit-fig1.png)


Du kan hämta händelser från din aktivitetsloggen med hello Azure-portalen [CLI](https://docs.microsoft.com/azure/storage/storage-azure-cli), PowerShell-cmdlets och [REST-API för Azure-Monitor](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-rest-api-walkthrough). Aktivitetsloggar har 19 dagars datakvarhållningsperiod.

Integrationsscenarier
-   [Skapa en e-post eller webhook avisering som utlöses av en aktivitet händelselogg.](https://docs.microsoft.com/azure/monitoring-and-diagnostics/insights-auditlog-to-webhook-email)

-   [Strömma den tooan Event Hub](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-stream-activity-logs-event-hubs) för införandet av en tjänst från tredje part eller anpassade analytics lösning, till exempel PowerBI.

-   Analysera i PowerBI med hello [PowerBI-Innehållspaketet.](https://powerbi.microsoft.com/documentation/powerbi-content-pack-azure-audit-logs/)

-   [Spara den tooa Storage-konto för arkivering eller Manuell kontroll.](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-archive-activity-log) Du kan ange hello kvarhållningstiden (i dagar) med hjälp av loggen profiler.

-   Fråga efter och visa den i hello Azure-portalen.

-   Fråga den via PowerShell-cmdleten, CLI eller REST API.

-   Exportera hello aktivitetsloggen med loggen profiler för[logga Analytics](https://docs.microsoft.com/azure/log-analytics/log-analytics-overview).

Du kan använda ett lagringskonto eller [event hub namnområde](https://docs.microsoft.com/azure/event-hubs/event-hubs-resource-manager-namespace-event-hub-enable-archive) som inte är i hello samma prenumeration som hello en ljusavgivande logg. hello-användare som konfigurerar hello inställning måste ha lämpliga hello [RBAC](https://docs.microsoft.com/azure/active-directory/role-based-access-control-configure) komma åt tooboth prenumerationer
### <a name="azure-diagnostic-logs"></a>Azure diagnostikloggar
Azure diagnostikloggar orsakat av en resurs som innehåller omfattande, ofta data om hello driften av den här resursen. hello innehållet i de här loggarna varierar efter resurstyp (till exempel [Windows-händelsesystemloggar](https://docs.microsoft.com/azure/log-analytics/log-analytics-data-sources-windows-events)är en kategori av diagnostiska loggar för virtuella datorer och [blob-, tabell- och kön loggar](https://docs.microsoft.com/azure/storage/storage-monitor-storage-account) finns kategorier av diagnostiska loggar storage-konton) och skiljer sig från hello aktivitetsloggen, som ger inblick i hello-åtgärder som utfördes på resurser i din prenumeration.

![Azure diagnostikloggar](./media/azure-log-audit/azure-log-audit-fig2.png)

Azure Diagnostics loggar erbjuder flera konfigurationsalternativ som är, Azure-portalen med hjälp av PowerShell-kommandoradsgränssnittet (CLI) och REST-API.

**Integrationsscenarier**
-   Spara dem tooa [Lagringskonto](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-archive-diagnostic-logs) för granskning eller Manuell kontroll. Du kan ange hello kvarhållningstiden (i dagar) med hjälp av hello diagnostikinställningarna.

-   [Strömma dem tooEvent Hubs](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-stream-diagnostic-logs-to-event-hubs) för införandet av en tjänst från tredje part eller anpassade analytics lösning som [PowerBI.](https://powerbi.microsoft.com/documentation/powerbi-azure-and-power-bi/)

-   Analysera dem med [OMS logganalys.](https://docs.microsoft.com/azure/log-analytics/log-analytics-overview)

**Tjänster som stöds, schemat för diagnostikloggar och stöds loggen kategorier per resurstyp**


| Tjänst | Schemat & Docs | Resurstyp | Kategori |
| ------- | ------------- | ------------- | -------- |
|Belastningsutjämnare| [Logganalys för Azure-belastningsutjämnaren (förhandsgranskning)](https://docs.microsoft.com/en-us/azure/load-balancer/load-balancer-monitor-log)|Microsoft.Network/loadBalancers|  LoadBalancerAlertEvent|
|||Microsoft.Network/loadBalancers| LoadBalancerProbeHealthStatus
|Nätverkssäkerhetsgrupper|[Log Analytics för nätverkssäkerhetsgrupper (NSG)](https://docs.microsoft.com/en-us/azure/virtual-network/virtual-network-nsg-manage-log)|Microsoft.Network/networksecuritygroups|NetworkSecurityGroupEvent|
|||Microsoft.Network/networksecuritygroups|NetworkSecurityGroupRuleCounter|
|Application Gateways|[Diagnostikloggning för Programgateway](https://docs.microsoft.com/en-us/azure/application-gateway/application-gateway-diagnostics)|Microsoft.Network/applicationGateways|ApplicationGatewayAccessLog|
|||Microsoft.Network/applicationGateways|ApplicationGatewayPerformanceLog|
|||Microsoft.Network/applicationGateways|ApplicationGatewayFirewallLog|
|Key Vault|[Azure Key Vault-loggning](https://docs.microsoft.com/en-us/azure/key-vault/key-vault-logging)|Microsoft.KeyVault/vaults|AuditEvent|
|Azure Search|[Att aktivera och använda Sök trafik Analytics](https://docs.microsoft.com/en-us/azure/search/search-traffic-analytics)|Microsoft.Search/searchServices|OperationLogs|
|Data Lake Store|[Åtkomst till diagnostikloggarna för Azure Data Lake Store](https://docs.microsoft.com/en-us/azure/data-lake-store/data-lake-store-diagnostic-logs)|Microsoft.DataLakeStore/accounts|Granska|
|Data Lake Analytics|[Åtkomst till diagnostikloggar för Azure Data Lake Analytics](https://docs.microsoft.com/en-us/azure/data-lake-analytics/data-lake-analytics-diagnostic-logs)|Microsoft.DataLakeAnalytics/accounts|Granska|
|||Microsoft.DataLakeAnalytics/accounts|Begäranden|
|||Microsoft.DataLakeStore/accounts|Begäranden|
|Logic Apps|[Anpassat Logic Apps B2B-spårningsschema](https://docs.microsoft.com/en-us/azure/logic-apps/logic-apps-track-integration-account-custom-tracking-schema)|Microsoft.Logic/workflows|WorkflowRuntime|
|||Microsoft.Logic/integrationAccounts|IntegrationAccountTrackingEvents|
|Azure Batch|[Azure Batch-diagnostikloggning](https://docs.microsoft.com/en-us/azure/batch/batch-diagnostics)|Microsoft.Batch/batchAccounts|ServiceLog|
|Azure Automation|[Logganalys för Azure Automation](https://docs.microsoft.com/en-us/azure/automation/automation-manage-send-joblogs-log-analytics)|Microsoft.Automation/automationAccounts|JobLogs|
|||Microsoft.Automation/automationAccounts|JobStreams|
|Händelsehubbar|[Azure Event Hubs diagnostiska loggar](https://docs.microsoft.com/en-us/azure/event-hubs/event-hubs-diagnostic-logs)|Microsoft.EventHub/namespaces|ArchiveLogs|
|||Microsoft.EventHub/namespaces|OperationalLogs|
|Stream Analytics|[Jobbet diagnostikloggar](https://docs.microsoft.com/en-us/azure/stream-analytics/stream-analytics-job-diagnostic-logs)|Microsoft.StreamAnalytics/streamingjobs|Körning|
|||Microsoft.StreamAnalytics/streamingjobs|Redigering|
|Service Bus|[Azure Service Bus-diagnostikloggar](https://docs.microsoft.com/en-us/azure/service-bus-messaging/service-bus-diagnostic-logs)|Microsoft.ServiceBus/namespaces|OperationalLogs|

### <a name="azure-active-directory-reporting"></a>Azure Active Directory Reporting
Azure AD (Active Directory Azure) tillhandahåller säkerhets-, aktivitets- och granskningsrapporter för din katalog. Hej [Azure Active Directory-kontrollrapport](https://docs.microsoft.com/azure/active-directory/active-directory-reporting-guide) hjälper kunder att tooidentify Privilegierade åtgärder som inträffade i sina Azure Active Directory. Privilegierade åtgärder omfattar höjning ändringar (till exempel skapa användarrollen eller återställning av lösenord), ändra principkonfigurationer (till exempel lösenordsprinciper) eller ändringar toodirectory konfigurationen (till exempel ändrar toodomain federation inställningar).

hello-rapporter tillhandahåller hello Granskningspost för hello händelsenamnet, hello aktören som utförde hello åtgärd, hello målresurs påverkas av hello ändringen och hello datum och tid (i UTC). Kunder är kan tooretrieve hello lista över granskningshändelser för sina Azure Active Directory via hello [Azure-portalen](https://portal.azure.com/), enligt beskrivningen i [visa granskningsloggarna](https://docs.microsoft.com/azure/active-directory/active-directory-reporting-azure-portal). Här är en lista över rapporter hello:

| Säkerhetsrapporter | Aktivitetsrapporter | Granskningsrapporter |
| :--------------- | :--------------- | :------------ |
|Inloggningar från okända källor| Programanvändning: sammanfattning| Kataloggranskningsrapport|
|Inloggningar efter flera fel|  Programanvändning: detaljerad||
|Inloggningar från flera geografiska områden|    Instrumentpanel för program||
|Inloggningar från IP-adresser med misstänkt aktivitet|   Kontoetableringsfel||
|Oregelbunden inloggningsaktivitet|    Enskilda användarenheter||
|Inloggningar från potentiellt infekterade enheter|   Aktivitet för enskilda användare||
|Användare med avvikande inloggningsaktivitet| Aktivitetsrapport för grupper||
||Aktivitetsrapport över registrering av lösenordsåterställning||
||Lösenordsåterställningsaktivitet|||

hello data för de här rapporterna kan vara användbart tooyour program, till exempel SIEM-system, granskning och business intelligence-verktyg. hello Azure AD reporting API: er ger Programmeringsåtkomst toohello data via en uppsättning REST-baserad API: er. Du kan anropa dessa [API: er](https://docs.microsoft.com/azure/active-directory/active-directory-reporting-api-getting-started) från olika programmeringsspråk språk och verktyg.

Händelser i hello Azure AD granska rapporten finns kvar i 180 dagar.

> [!Note]
> Mer information om kvarhållning i rapporter finns [Azure Active Directory Rapportkvarhållningsregler.](https://docs.microsoft.com/azure/active-directory/active-directory-reporting-retention)

För kunder som vill lagra sina granskningshändelser under längre bevarandeperioder, hello Reporting API kan vara används tooregularly pull [granskningshändelser](https://docs.microsoft.com/azure/active-directory/active-directory-reporting-audit-events) i ett separat datalager.

### <a name="virtual-machine-logs-using-azure-diagnostics"></a>Loggar för virtuell dator med hjälp av Azure-diagnostik
[Azure Diagnostics](https://docs.microsoft.com/azure/azure-diagnostics) är hello funktion i Azure som gör att hello insamlingen av diagnostiska data på ett distribuerat program. Du kan använda hello diagnostik tillägg från flera olika källor. Stöds för närvarande är [Azure Cloud Service webb- och arbetsroller](https://docs.microsoft.com/azure/cloud-services/cloud-services-choose-me),

![Loggar för virtuell dator med hjälp av Azure-diagnostik](./media/azure-log-audit/azure-log-audit-fig3.png)

[Virtuella Azure-datorer](https://azure.microsoft.com/documentation/learning-paths/virtual-machines/) med Microsoft Windows och [Service Fabric](https://docs.microsoft.com/azure/service-fabric/service-fabric-overview).

Du kan aktivera Azure diagnostisk på en virtuell dator med hjälp av följande:

-   Med hjälp av Visual Studio finns [Använd Visual Studio tootrace Azure virtuella datorer](https://docs.microsoft.com/azure/vs-azure-tools-debug-cloud-services-virtual-machines)

-   [Konfigurera Azure-diagnostik på en Azure virtuell dator från en fjärrdator](https://docs.microsoft.com/azure/virtual-machines-dotnet-diagnostics)

-   [Använd PowerShell tooset in diagnostik på Azure Virtual Machines](https://docs.microsoft.com/azure/virtual-machines/virtual-machines-windows-ps-extensions-diagnostics?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)

-   [Skapa en Windows virtuell dator med övervakning och diagnostik med Azure Resource Manager-mall](https://docs.microsoft.com/azure/virtual-machines/virtual-machines-windows-extensions-diagnostics-template?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)

### <a name="storage-analytics"></a>Lagringsanalys
[Azure Storage Analytics](https://docs.microsoft.com/rest/api/storageservices/fileservices/storage-analytics) utför loggning och tillhandahåller data för mått för ett lagringskonto. Du kan använda den här tootrace begäranden, analysera användningstrender och diagnostisera problem med ditt lagringskonto. Storage Analytics loggning är tillgänglig för hello [Blob-, kö-och tabellen.](https://docs.microsoft.com/azure/storage/storage-introduction) Storage Analytics loggar detaljerad information om lyckade och misslyckade begäranden tooa storage-tjänst.

Den här informationen kan vara används toomonitor enskilda förfrågningar och toodiagnose problem med en storage-tjänst. Begäranden loggas för bästa prestanda. Loggposter skapas bara om det finns begäranden som görs mot hello tjänstslutpunkten. Om ett lagringskonto har aktivitet i sin slutpunkt för Blob men inte i tabellen eller kö slutpunkter, endast loggar som rör skapa toohello Blob-tjänsten.

toouse Storage Analytics, måste du aktivera det individuellt för varje tjänst du vill toomonitor. Du kan aktivera den i hello [Azure-portalen](https://portal.azure.com/); mer information finns i [övervaka ett lagringskonto i hello Azure-portalen.](https://docs.microsoft.com/azure/storage/storage-monitor-storage-account) Du kan också aktivera Storage Analytics programmässigt via hello REST API eller klientbibliotek för hello. Använd hello ange tjänstegenskaper åtgärden tooenable Storage Analytics individuellt för varje tjänst.

hello aggregerade data lagras i en välkänd blob (för loggning) och välkända tabeller (för mått) som kan användas med hello Blob- och tabelltjänsterna API: er.

Storage Analytics har en gräns på 20 TB på hello mängden lagrade data som är oberoende av hello totala gränsen för ditt lagringskonto. Alla loggar lagras i [blockblobbar](https://docs.microsoft.com/azure/storage/storage-analytics) i en behållare med namnet $logs, som skapas automatiskt när Storage Analytics har aktiverats för ett lagringskonto.

> [!Note]
> Mer information om fakturering och datakvarhållningsprinciper finns [Storage Analytics och fakturering.](https://docs.microsoft.com/rest/api/storageservices/fileservices/storage-analytics-and-billing)
>
> [!Note]
> Läs mer om lagringskontogränser [och prestandamål för Azure Storage skalbarhet.](https://docs.microsoft.com/azure/storage/storage-scalability-targets)

följande typer av autentiserade och anonyma begäranden hello loggas.



| Autentiserad  | Anonym|
| :------------- | :-------------|
| Lyckade begäranden | Lyckade begäranden |
|Misslyckade begäranden, inklusive timeout, begränsning, nätverk, auktorisering och andra fel | Begäranden som använder en delad signatur åtkomst (SAS), inklusive misslyckade och lyckade begäranden |
| Begäranden som använder en delad signatur åtkomst (SAS), inklusive misslyckade och lyckade begäranden |Timeout-fel för både klient och server |
|   Begäranden tooanalytics data |    Misslyckade GET-begäranden med felkoden 304 (ändra inte) |
| Förfrågningar som görs av Storage Analytics, till exempel loggen skapas eller tas bort, loggas inte. En fullständig lista över hello loggade data dokumenteras i hello [Storage Analytics loggade åtgärder och statusmeddelanden](https://docs.microsoft.com/rest/api/storageservices/fileservices/storage-analytics-logged-operations-and-status-messages) och [Storage Analytics loggformatet](https://docs.microsoft.com/rest/api/storageservices/fileservices/storage-analytics-log-format) avsnitt. | Alla andra misslyckade anonyma begäranden har inte loggats. En fullständig lista över hello loggade data dokumenteras i hello [Storage Analytics loggade åtgärder och statusmeddelanden](https://docs.microsoft.com/rest/api/storageservices/fileservices/storage-analytics-logged-operations-and-status-messages) och [Storage Analytics loggformatet](https://docs.microsoft.com/rest/api/storageservices/fileservices/storage-analytics-log-format). |

### <a name="azure-networking-logs"></a>Azure-nätverk loggar
Loggning och nätverksövervakning i Azure är omfattande och omfattar två kategorier:

-   [Nätverk Watcher](https://docs.microsoft.com/azure/network-watcher/network-watcher-monitoring-overview#network-watcher) -scenariobaserade nätverksövervakning tillhandahålls med hello funktioner i Nätverksbevakaren. Den här tjänsten innehåller paketinsamling, nästa hopp, IP-flöde Kontrollera NSG flödet loggar i gruppvyn säkerhet. Scenariot nivån övervakning innehåller en end tooend för nätverksresurser i kontrast tooindividual resurs nätverksövervakning.

-   [Resursövervakning](https://docs.microsoft.com/azure/network-watcher/network-watcher-monitoring-overview#network-resource-level-monitoring) -nivå Resursövervakning består av fyra funktioner, diagnostiska loggar, mått, felsökning och resurshälsa. Alla dessa funktioner är byggda på hello nätverksnivån resurs.

![Azure-nätverk loggar](./media/azure-log-audit/azure-log-audit-fig4.png)

Nätverksbevakaren är en regionala tjänst som gör att du toomonitor och diagnostisera villkor på nätverket scenariot nivå, till och från Azure. Diagnostiska nätverks- och visualiseringsverktyg som finns tillgängliga med Nätverksbevakaren hjälpa dig att förstå, diagnostisera och få insikter tooyour nätverk i Azure.

**NSG flöda loggning** -flöde Nätverkssäkerhetsgrupper loggarna aktivera toocapture loggar relaterade tootraffic som tillåts eller nekas av hello säkerhetsregler i hello-gruppen. Loggarna flödet skrivs i JSON-format och visa utgående och inkommande flöden på grundval av per regel hello NIC hello flödet gäller för 5-tuppel information om hello flödet (källan/målet IP-källan/målet Port Protocol) och om hello trafik tilläts eller nekad.

### <a name="network-security-group-flow-logging"></a>Nätverkssäkerhetsgruppen flöde loggning

[Nätverkssäkerhetsgruppen flöde loggar](https://docs.microsoft.com/azure/network-watcher/network-watcher-nsg-flow-logging-overview) är en funktion i Nätverksbevakaren som gör att du tooview information om ingående och utgående IP-trafik via en Nätverkssäkerhetsgrupp. Loggarna flödet skrivs i JSON-format och visa utgående och inkommande flöden på grundval av per regel hello NIC hello flödet gäller för 5-tuppel information om hello flödet (källan/målet IP-källan/målet Port Protocol) och om hello trafik tilläts eller nekad.

Medan flödet loggar mål Nätverkssäkerhetsgrupper, de visas inte hello samma som hello andra loggar. Flödet loggfilerna lagras i ett lagringskonto.

Hej samma bevarandeprinciper som det visas på andra loggar tillämpa tooflow loggar. Loggar har en bevarandeprincip som kan anges från 1 dag too365 dagar. Om en bevarandeprincip inte har angetts bibehålls hello loggar alltid.

**Diagnostikloggar**

Periodiska och eget initiativ händelser skapas av nätverksresurser och inloggad storage-konton, skickas tooan Event Hub eller logganalys. Dessa loggar ger insikter om hello hälsotillståndet för en resurs. Dessa loggar kan visas i verktyg som Power BI och logganalys. toolearn hur tooview diagnostikloggar, besök [logganalys.](https://docs.microsoft.com/azure/log-analytics/log-analytics-azure-networking-analytics)

![Diagnostikloggar](./media/azure-log-audit/azure-log-audit-fig5.png)

Diagnostikloggar är tillgängliga för [belastningsutjämnaren](https://docs.microsoft.com/azure/load-balancer/load-balancer-monitor-log), [Nätverkssäkerhetsgrupper](https://docs.microsoft.com/azure/virtual-network/virtual-network-nsg-manage-log), vägar och [Application Gateway.](https://docs.microsoft.com/azure/application-gateway/application-gateway-diagnostics)

Nätverksbevakaren ger diagnostikloggar vyn. Den här vyn innehåller alla nätverksresurser som stöder diagnostikloggning. I den här vyn kan du aktivera och inaktivera nätverksresurser lätt och snabbt.


I tillägg toopreceding loggningsfunktioner har Nätverksbevakaren hello följande funktioner:
- [Topologi](https://docs.microsoft.com/azure/network-watcher/network-watcher-topology-overview) -tillhandahåller ett nätverk nivån som visar hello olika anslutningarna och associationer mellan nätverksresurser i en resursgrupp.

- [Variabeln paketinsamling](https://docs.microsoft.com/azure/network-watcher/network-watcher-packet-capture-overview) -samlar in paketdata till och från en virtuell dator. Avancerade alternativ för filtrering och finjustera kontroller, till exempel som kan tooset ger tiden och storlek begränsningar versatility.hello paketdata kan lagras i en blobstore eller på hello lokal disk i CAP-format.

-   [IP-flöde verifierar](https://docs.microsoft.com/azure/network-watcher/network-watcher-ip-flow-verify-overview) -kontrollerar om ett paket tillåts eller nekas baserat på flödet information 5-tuppel paket parametrar (mål-IP, käll-IP, målport, källport och Protocol). Om hello paket nekas av en säkerhetsgrupp returneras hello regeln och grupp som nekas hello paket.

-   [Nästa hopp](https://docs.microsoft.com/azure/network-watcher/network-watcher-next-hop-overview) -avgör hello next hop för paket som vidarebefordras i hello Azure nätverksinfrastruktur, aktiverar du toodiagnose alla felkonfigurerad användardefinierade vägar.

-   [Säkerhet gruppvyn](https://docs.microsoft.com/azure/network-watcher/network-watcher-security-group-view-overview) -hämtar hello effektiva och tillämpade säkerhetsregler som tillämpas på en virtuell dator.

-   [Virtuella Nätverksgatewayen och anslutningen felsökning](https://docs.microsoft.com/azure/network-watcher/network-watcher-troubleshoot-manage-rest) -ger hello möjlighet tootroubleshoot virtuella Nätverksgatewayer och anslutningar.

-   [Nätverk prenumerationsbegränsningar](https://docs.microsoft.com/azure/network-watcher/network-watcher-monitoring-overview#network-subscription-limits) -aktiverar tooview nätverksresursanvändning mot gränser.

### <a name="application-insight"></a>Application Insights

[Application Insights](https://docs.microsoft.com/azure/application-insights/app-insights-overview) är en utökningsbar Management-program (APM)-tjänst för webbutvecklare på flera plattformar. Använd den toomonitor live webbprogrammet. Det är att automatiskt identifiera prestandaavvikelser. Den innehåller kraftfulla analytics verktyg toohelp du diagnostisera problem och toounderstand vad användare utför med din app.

 Den är utformad toohelp kontinuerligt förbättra prestanda och användbarhet.

 Det fungerar för appar på en mängd olika plattformar, inklusive .NET, Node.js och J2EE finns lokalt eller i hello molnet. Den kan integreras med devOps-processen och har anslutning punkter toovarious utvecklingsverktyg.

![Application Insights](./media/azure-log-audit/azure-log-audit-fig6.png)

Application Insights syftar hello Utvecklingsteamet, toohelp du förstår hur din app fungerar och hur den används. Tjänsten övervakar:

-   **Begärandefrekvens, svarstider och felfrekvens** – Ta reda på vilka sidor som är mest populära, vid vilka tidpunkter på dagen och var dina användare finns. Se vilka sidor som fungerar bäst. Om svarstiden och felfrekvensen är hög när det finns många begäranden kan det bero på ett resurstilldelningsproblem.

-   **Beroendefrekvens, svarstider och felfrekvens** – Ta reda på om externa tjänster gör systemet långsammare.

-   **Undantag** – analysera hello samman statistik, eller välja specifika instanser och detaljerat hello stackspårning och relaterade begäranden. Både server- och webbläsarundantag rapporteras.

-   **Sidvyer och inläsningsprestanda** – Rapporteras av användarnas webbläsare.

-   **AJAX-anrop** från webbsidor – frekvens, svarstider och felfrekvens.

-   **Antal användare och sessioner**.

-   **Prestandaräknare** från dina Windows- eller Linux-serverdatorer, till exempel processor, minne och nätverksanvändning.

-   **Värddiagnostik** från Docker eller Azure.

-   **Diagnostikspårningsloggar** från din app – så att du kan jämföra spårningshändelser med begäranden.

-   **Anpassade händelser och mått** att du kan skriva själv i hello klient eller server kod, tootrack affärshändelser som objekt säljs eller spel vann.

**Lista över integrationsscenarier och beskrivning:**

| Integrationsscenarier | Beskrivning |
| --------------------- | :---------- |
|[Programavbildningen](https://docs.microsoft.com/en-us/azure/application-insights/app-insights-app-map)|hello komponenter i appen med nyckelvärden och -varningar.||
|[Diagnostiska Sök till exempel data](https://docs.microsoft.com/en-us/azure/application-insights/app-insights-diagnostic-search)| Sök efter och filtrera händelser, till exempel begäranden, undantag, beroendeanrop, loggspårningar och sidvyer.||
|[Metrics Explorer för aggregerade data](https://docs.microsoft.com/en-us/azure/application-insights/app-insights-metrics-explorer)|Utforska, filtrera och segmentera aggregerade data, till exempel begärande-, fel- och undantagsfrekvens, svarstider och sidinläsningstider.||
|[Instrumentpaneler](https://docs.microsoft.com/en-us/azure/application-insights/app-insights-dashboards#dashboards)|Kombinera data från flera resurser och dela med andra. Bra för flera komponenten program och för kontinuerlig visning i hello team rum.||
|[Direktsänd dataström mått](https://docs.microsoft.com/en-us/azure/application-insights/app-insights-live-stream)|När du distribuerar en ny version kan du titta på dessa nära realtid prestanda indikatorer toomake att allt fungerar som förväntat.||
|[Analys](https://docs.microsoft.com/en-us/azure/application-insights/app-insights-analytics)|Besvara svåra frågor om appens prestanda och användning med hjälp av det här kraftfulla frågespråket.||
|[Automatisk och manuell aviseringar](https://docs.microsoft.com/en-us/azure/application-insights/app-insights-alerts)|Automatisk aviseringar anpassa tooyour app normal mönster av telemetri och utlösare när det finns något utanför hello vanliga mönstret. Du kan också ställa in aviseringar på särskilda nivåer med anpassade mätvärden eller standardmätvärden.||
|[Visual Studio](https://docs.microsoft.com/en-us/azure/application-insights/app-insights-visual-studio)|Se prestandadata i hello kod. Gå toocode från stackspår.||
|[Power BI](https://docs.microsoft.com/en-us/azure/application-insights/app-insights-export-power-bi)|Integrera användningsmätvärden med annan Business Intelligence.||
|[REST-API](https://dev.applicationinsights.io/)|Skriva koden toorun frågor via din mått och rådata.||
|[Löpande export](https://docs.microsoft.com/en-us/azure/application-insights/app-insights-export-telemetry)|Massredigera export av rådata toostorage när det kommer fram.||

### <a name="azure-security-center-alerts"></a>Azure Security Center-aviseringar
[Azure Security Center](https://docs.microsoft.com/azure/security-center/security-center-intro) automatiskt samlar in, analyserar, och integrerar loggdata från din Azure-resurser och hello nätverk anslutna partnerlösningar som brandväggen och endpoint protection lösningar, toodetect verkliga hot och minska falska positiva identifieringar. En lista med prioritetssorterade säkerhetsaviseringar visas i Security Center tillsammans med hello information du behöver tooquickly undersöka hello problem och rekommendationer för hur tooremediate angreppet.

Hotidentifiering Security Center fungerar genom att automatiskt samla in säkerhetsinformation från Azure-resurser, hello nätverket och anslutna partnerlösningar. Den här informationen kan ofta korrelerar information från flera källor, tooidentify hot analyseras. Säkerhetsaviseringar prioriteras i Security Center tillsammans med rekommendationer om hur tooremediate hello hot.

![Azure Security Center](./media/azure-log-audit/azure-log-audit-fig7.png)

Security Center använder avancerade säkerhetsanalyser, som går mycket längre än signaturbaserade lösningar. Genombrott i stora mängder data och [maskininlärning](https://azure.microsoft.com/blog/machine-learning-in-azure-security-center/) tekniker är tillämpade tooevaluate händelser över hello hela molninfrastrukturresurs – upptäcka hot som skulle vara omöjligt tooidentify med manuella metoder och förutsäga hello utvecklingen av attacker. Dessa säkerhetsanalyser omfattar:

-   **Integrerad hotinformation:** ser ut för kända obehöriga genom att använda globala hotinformation från Microsoftprodukter och tjänster, hello Microsoft Digital Crimes Unit (DCU), hello Microsoft Security Response Center (MSRC) och externa feeds.

-   **Beteendeanalys:** gäller kända mönster toodiscover skadligt beteende.

-   **Avvikelseidentifiering:** använder statistiska toobuild utgångspunkt historisk profilering. Varnar på avvikelser från etablerade baslinjer som följer tooa potentiella angrepp.


Förlitar sig på en lösning för Security Information and Event Management (SIEM) som startpunkt för triaging och undersöker säkerhetsaviseringar hello många säkerhetsåtgärder och incidenter team. Med Azure logganalys-integration kan kunder synkronisera varningsmeddelanden och virtuella säkerhetshändelser samlas in av Azure-diagnostik och Azure-granskningsloggarna med deras logganalys eller SIEM-lösning i nära realtid.


## <a name="log-analytics"></a>Log Analytics

Log Analytics är en tjänst i [Operations Management Suite (OMS)](https://docs.microsoft.com/azure/operations-management-suite/operations-management-suite-overview) som hjälper dig att samla in och analysera data som genereras av resurser i molnet och lokala miljöer. Den ger dig realtidsinsikter integrerad sökning och anpassade instrumentpaneler tooreadily analysera miljontals poster för alla arbetsbelastningar och servrar oavsett fysiska plats.

![Log Analytics](./media/azure-log-audit/azure-log-audit-fig8.png)

Hello är center logganalys hello OMS-lagringsplatsen, som är värd för hello Azure-molnet. Data samlas in hello databasen från anslutna källor genom att konfigurera datakällor och lägga till lösningar tooyour prenumeration. Datakällor och lösningar ska skapa olika posttyper som har en egen uppsättning egenskaper, men kan fortfarande analyseras tillsammans i frågor toohello databas. Detta ger dig toouse hello samma verktyg och metoder toowork med olika typer av data som samlas in av olika källor.

Anslutna källor är hello datorer och andra resurser som genererar data som samlas in av logganalys. Detta kan inkludera agenter som installerats på [Windows](https://docs.microsoft.com/azure/log-analytics/log-analytics-windows-agents) och [Linux](https://docs.microsoft.com/azure/log-analytics/log-analytics-linux-agents) datorer som ansluter direkt eller agenter i [en ansluten hanteringsgrupp för System Center Operations Manager.](https://docs.microsoft.com/azure/log-analytics/log-analytics-om-agents) Log Analytics kan också samla in data från [Azure storage.](https://docs.microsoft.com/azure/log-analytics/log-analytics-azure-storage)

[Datakällor](https://docs.microsoft.com/azure/log-analytics/log-analytics-data-sources) finns hello olika typer av data som samlas in från varje anslutna källa. Detta innefattar händelser och [prestandadata](https://docs.microsoft.com/azure/log-analytics/log-analytics-data-sources-performance-counters) från [Windows](https://docs.microsoft.com/azure/log-analytics/log-analytics-data-sources-windows-events) och Linux-agenter i tillägg toosources som [IIS-loggar](https://docs.microsoft.com/azure/log-analytics/log-analytics-data-sources-iis-logs), och [anpassade textloggar.](https://docs.microsoft.com/azure/log-analytics/log-analytics-data-sources-custom-logs) Du kan konfigurera varje datakälla som du vill toocollect och hello konfigureras automatiskt levererat tooeach anslutna källa.

Det finns fyra olika sätt att [att samla in loggar och mått för Azure-tjänster:](https://docs.microsoft.com/azure/log-analytics/log-analytics-azure-storage)
1.  Azure diagnostics direkt tooLog Analytics (diagnostik i den följande tabellen hello)

2.  Azure diagnostics tooAzure lagring tooLog Analytics (lagring i den följande tabellen hello)

3.  Kopplingar för Azure-tjänster (kopplingar i den följande tabellen hello)

4.  Skript toocollect och skicka data till logganalys (tomma celler i den följande tabellen hello och för tjänster som inte är listade)

| Tjänst | Resurstyp | Logs | Mått | Lösning |
| :------ | :------------ | :--- | :------ | :------- |
|Programgateways|  Microsoft.Network/<br>applicationGateways|  Diagnostik|Diagnostik|    [Azure-program](https://docs.microsoft.com/en-us/azure/log-analytics/log-analytics-azure-networking-analytics#azure-application-gateway-analytics-solution-in-log-analytics) [Gateway Analytics](https://docs.microsoft.com/en-us/azure/log-analytics/log-analytics-azure-networking-analytics#azure-application-gateway-analytics-solution-in-log-analytics)|
|Programinsikter||     koppling|  koppling|  [Application Insights](https://blogs.technet.microsoft.com/msoms/2016/09/26/application-insights-connector-in-oms/) [anslutningsprogrammet (förhandsversion)](https://blogs.technet.microsoft.com/msoms/2016/09/26/application-insights-connector-in-oms/)|
|Automation-konton|   Microsoft.Automation/<br>AutomationAccounts|    Diagnostik||       [Mer information](https://docs.microsoft.com/en-us/azure/automation/automation-manage-send-joblogs-log-analytics)|
|Batch-konton|    Microsoft.Batch/<br>batchAccounts|  Diagnostik|    Diagnostik||
|Klassiska molntjänster||       Lagring||       [Mer information](https://docs.microsoft.com/en-us/azure/log-analytics/log-analytics-azure-storage-iis-table)|
|Cognitive Services|    Microsoft.CognitiveServices/<br>konton|       Diagnostik|||
|Data Lake analytics|   Microsoft.DataLakeAnalytics/<br>konton|   Diagnostik|||
|Data Lake store|   Microsoft.DataLakeStore/<br>konton|   Diagnostik|||
|Event Hub namnområde|   Microsoft.EventHub/<br>namnområden|  Diagnostik|    Diagnostik||
|IoT-hubbar|  Microsoft.Devices/<br>IotHubs||     Diagnostik||
|Key Vault| Microsoft.KeyVault/<br>Valv|  Diagnostik  || [KeyVault Analytics](https://docs.microsoft.com/en-us/azure/log-analytics/log-analytics-azure-key-vault)|
|Belastningsutjämnare|    Microsoft.Network/<br>loadBalancers|    Diagnostik|||
|Logic Apps|    Microsoft.Logic/<br>Arbetsflöden|  Diagnostik|    Diagnostik||
||Microsoft.Logic/<br>integrationAccounts||||
|Nätverkssäkerhetsgrupper|   Microsoft.Network/<br>networksecuritygroups|Diagnostik||   [Azure Nätverkssäkerhetsgruppen Analytics](https://docs.microsoft.com/en-us/azure/log-analytics/log-analytics-azure-networking-analytics#azure-network-security-group-analytics-solution-in-log-analytics)|
|Recovery-valv|   Microsoft.RecoveryServices/<br>Valv|||[Azure Recovery Services Analytics (förhandsgranskning)](https://github.com/krnese/AzureDeploy/blob/master/OMS/MSOMS/Solutions/recoveryservices/)|
|Söktjänster|   Microsoft.Search/<br>searchServices|    Diagnostik|    Diagnostik||
|Service Bus-namnrymd| Microsoft.ServiceBus/<br>namnområden|    Diagnostik|Diagnostik|    [Service Bus Analytics (förhandsgranskning)](https://github.com/Azure/azure-quickstart-templates/tree/master/oms-servicebus-solution)|
|Service Fabric||       Lagring||    [Service Fabric Analytics (förhandsgranskning)](https://docs.microsoft.com/en-us/azure/log-analytics/log-analytics-service-fabric)|
|SQL (v12)| Microsoft.Sql/<br>servrar /<br>databaser||       Diagnostik||
||Microsoft.Sql/<br>servrar /<br>elasticPools||||
|Lagring|||         Skript| [Azure Storage Analytics (förhandsgranskning)](https://github.com/Azure/azure-quickstart-templates/tree/master/oms-azure-storage-analytics-solution)|
|Virtuella datorer|  Microsoft.Compute/<br>virtuella datorer|  Anknytning|  Anknytning||
||||Diagnostik||
|Skalningsuppsättningar i virtuella datorer|   Microsoft.Compute/<br>virtuella datorer    ||Diagnostik||
||Microsoft.Compute/<br>virtualMachineScaleSets /<br>virtuella datorer||||
|Webbservergrupper|Microsoft.Web/<br>serverfarms||   Diagnostik
|Webbplatser| Microsoft.Web/<br>webbplatser ||      Diagnostik|    [Mer information](https://github.com/Azure/azure-quickstart-templates/tree/master/101-webappazure-oms-monitoring)|
||Microsoft.Web/<br>platser /<br>fack|||||


## <a name="log-integration-with-on-premises-siem-systems"></a>Log-integrering med lokala SIEM-system
[Azure logganalys integration](https://www.microsoft.com/download/details.aspx?id=53324) kan du toointegrate loggarna från Azure-resurser i tooyour lokala **Security Information and Event Management (SIEM) system**.

![Log-integrering](./media/azure-log-audit/azure-log-audit-fig9.png)

Azure logganalys integration samlar in Azure-diagnostik från Windows (BOMULLSTUSS) virtuella datorer, Azure Security Center-aviseringar i aktivitetsloggar Azure och Azure Resource Provider loggar. Den här integreringen ger en enhetlig instrumentpanel för alla dina tillgångar, lokalt eller i hello molnet så att du kan aggregera, korrelera, analysera och varna för säkerhetshändelser.



Azure logganalys-integration stöder för närvarande integrering av aktivitetsloggar Azure, Windows-händelselogg från Windows-dator i Azure-prenumerationen, Azure Security Center-aviseringar, Azure diagnostikloggar och Azure Active Directory-granskningsloggar.

| Loggtyp | Logganalys stöder JSON (Splunk ArcSight, Qradar) |
| :------- | :-------------------------------------------------------- |
|AAD-granskningsloggar|    Ja|
|Aktivitetsloggar| Ja|
|ASC aviseringar |Ja|
|Diagnostik-loggar (resurs-loggarna)|  Ja|
|VM-loggar|   Ja via vidarebefordrade händelser och inte via JSON|


hello följande tabell förklarar hello loggen kategori och information om integrering av SIEM.

[Kom igång med Azure logganalys-integration](https://docs.microsoft.com/azure/security/security-azure-log-integration-get-started) – självstudier vägleder dig genom installationen av Azure logganalys-integrering och integrera loggar från Azure BOMULLSTUSS lagring, Azure aktivitetsloggar Azure Security Center-aviseringar och Azure Active Directory granskningsloggar.

Integrationsscenarier

-   [Samarbeta konfigurationssteg](https://blogs.msdn.microsoft.com/azuresecurity/2016/08/23/azure-log-siem-configuration-steps/) – det här blogginlägget visar hur tooconfigure Azure logga toowork integrering med partnerlösningar Splunk, HP ArcSight och IBM QRadar.

-   [Azure logganalys Integration vanliga frågor (FAQ)](https://docs.microsoft.com/azure/security/security-azure-log-integration-faq) – det här avsnittet får du svar frågor om Azure logganalys-integration.

-   [Integrera Security Center aviseringar med Azure logga Integration](https://docs.microsoft.com/azure/security-center/security-center-integrating-alerts-with-log-integration) – det här dokumentet beskrivs hur toosync Security Center aviseringar, tillsammans med virtuella säkerhetshändelser samlas in av Azure-diagnostik och Azure-granskningsloggarna med din logganalys eller SIEM-lösning.

## <a name="next-steps"></a>Nästa steg

- [Granskning och loggning](https://www.microsoft.com/trustcenter/security/auditingandlogging)

Skydda data genom att underhålla synlighet och snabbt svara tootimely säkerhetsaviseringar

- [Loggning och granskning Logginsamling i Azure](https://azure.microsoft.com/resources/videos/security-logging-and-audit-log-collection/)

Vilka inställningar du behöver tooenforce toomake till din Azure-instanser att samla in hello rätt säkerhet och granskningsloggar.

- [Konfigurera granskningsinställningar för en webbplatssamling](https://support.office.com/article/Configure-audit-settings-for-a-site-collection-A9920C97-38C0-44F2-8BCB-4CF1E2AE22D2?ui=&rs=&ad=US)

Som administratör för en webbplatssamling, en kan hämta hello historik över åtgärder som vidtas för en viss användare och kan också hämta hello historik över åtgärder under ett visst datumintervall. 

- [Sök hello granskningsloggen i hello Office 365-säkerhet och efterlevnad Center](https://support.office.com/article/Search-the-audit-log-in-the-Office-365-Security-Compliance-Center-0d4d0f35-390b-4518-800e-0c7ec95e946c?ui=&rs=&ad=US)

En kan använda hello Office 365 säkerhets- och Efterlevnadscentret toosearch hello enhetlig audit logga tooview användare och administratörer aktivitet i din Office 365-organisation.


