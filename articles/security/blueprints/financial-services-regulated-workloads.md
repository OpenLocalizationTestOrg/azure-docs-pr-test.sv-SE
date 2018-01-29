---
title: "Plan för Azure Automation - finansiella tjänster för reglerade arbetsbelastningar"
description: "Plan för finansiella tjänster för reglerade arbetsbelastningar"
services: security
documentationcenter: na
author: simorjay
manager: mbaldwin
editor: tomsh
ms.assetid: 17794288-9074-44b5-acc8-1dacceb3f56c
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/29/2017
ms.author: frasim
ms.openlocfilehash: 19e26c16866dada8dcff04a520ce4c208d67c365
ms.sourcegitcommit: f46cbcff710f590aebe437c6dd459452ddf0af09
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 12/20/2017
---
# <a name="azure-blueprint-automation-financial-services-blueprint-for-regulated-workloads"></a>Plan för Azure Automation: Finansiella tjänster utkast för reglerade arbetsbelastningar

## <a name="overview"></a>Översikt

Plan för finansiella tjänster för regleras arbetsbelastningar kan distribuera en säker och kompatibel plattform som en tjänst (PaaS)-webbprogram som har utformats för att hantera känsliga data i molnet. Modell som består av automatiserade skript och riktlinjer som demonstrerar en enkel referens-arkitektur och en design som förenklar införandet av Microsoft Azure-lösningar. Det här utkastet illustrerar en lösning för att uppfylla behoven för organisationer som vill ha olika sätt att minska belastningen och kostnaden för att distribuera i molnet.

Det här utkastet är utformad för att uppfylla kraven för strikta kompatibla standarder som angetts av den American Institute av certifierad offentliga revisorer, till exempel - SOC 1, SOC 2, rådet Payment Card Industry Data Security Standards DSS 3.2 och FFIEC för den insamling, lagring och hämtning av känsliga ekonomiska data. Den visar korrekt hantering av sådana data genom att distribuera en lösning som hanterar ekonomiska data i en miljö med säker, kompatibla och flera nivåer. Lösningen distribueras som en slutpunkt till slutpunkt Azure-baserade PaaS-lösning. 

Modell är avsett att utgöra grunden för kunder att bygga och förstå kraven för att hantera säkra data i molnet. Lösningen ska inte användas i en Produktionsdistribution som-är, men att förstå, utforma och distribuera Azure-tjänster. den är utformad som utgångspunkt för att hjälpa kunderna att använda Microsoft Azure på ett sätt som skyddade och kompatibla.

En auktoriserad granskare måste certifiera en produktion kund-lösning som baseras på det här utkastet. Lösningar kan variera beroende på grundval av varje kund implementering och geografisk plats.

För en snabb överblick över hur den här lösningen fungerar, titta på den här [video](https://aka.ms/fsiblueprintvideo) som förklarar och visar dess distribution.

## <a name="solution-components"></a>Lösningskomponenter

Arkitekturen består av följande komponenter och använder funktioner för distribution från Azure PCI DSS efterlevnad lösning.

- **Arkitekturdiagram för**. Diagrammet visar referensarkitektur som används för Contoso Webstore lösningen.
- **Distributionsmallar**. I den här distributionen [Azure Resource Manager-mallar](/azure/azure-resource-manager/resource-group-overview#template-deployment) används för att automatiskt distribuera komponenterna i arkitekturen i Microsoft Azure genom att ange konfigurationsparametrar under installationen.
- **Automatisk distribution skript**. Dessa skript att distribuera lösningen för slutpunkt till slutpunkt. Skripten består av:
    - En installation av modulen och [global administratör](/azure/active-directory/active-directory-assign-admin-roles-azure-portal) installationsskriptet används för att installera och kontrollera att PowerShell-moduler som krävs och global administratörsroller är korrekt konfigurerade. 
    - En installation av PowerShell-skript används för att distribuera lösningen för slutpunkt till slutpunkt, som tillhandahålls via en .zip-fil och en .bacpac-fil som innehåller en förskapad demo webbprogrammet med [SQL-databas exempel](https://github.com/Microsoft/azure-sql-security-sample). innehåll. Källkoden för den här lösningen är tillgänglig för granskning [betalning bearbetning utkast databasen][code-repo]. 

## <a name="architectural-diagram"></a>Arkitekturdiagram

![](images/pci-architectural-diagram.png)

## <a name="user-scenario"></a>Användarscenario

Modell adresser följande användningsfall i nedan.

> Det här scenariot visas hur en fiktiv vi besvarar flyttas känsliga data i en PaaS molnet Azure-baserad lösning. Lösningen exemplet illustrerar hantering och insamling av grundläggande information och valda känsliga data. Detta verk lånar från Azure utkast Automation: betalning bearbetning för PCI DSS-kompatibel miljöer för payment card bearbetning. Mer information om expanderande på detta verk [”granska och riktlinjer för genomförande”](https://aka.ms/pciblueprintprocessingoverview) dokumentet innehåller en granskning av PCI DSS-kompatibel miljöer.

### <a name="use-case"></a>Användningsfall
En liten vi besvarar kallas *Contoso Webstore* är redo att flytta ekonomiska data som innehåller kundbetalningsinformation till molnet. 

Administratören för Contoso Webstore är ute efter en lösning som kan distribueras snabbt för att nå sina mål. Han använder den här--konceptbevis (POC) för att diskutera med sin intressenter hur Azure kan användas för att samla in, lagra och hämta ekonomiska data i enlighet med strikta efterlevnadskrav.

> Du ansvarar för att utföra lämpliga säkerhet och efterlevnad granskning av en lösning som skapats med den arkitektur som används av den här POC kraven kan variera baserat på egenskaperna för din implementering och geografisk plats. 

### <a name="elements-of-the-foundational-architecture"></a>Element i grundläggande arkitektur

Grundläggande arkitektur är utformad med fiktiva följande element:

Domän-plats`contosowebstore.com`

Användarroller utnyttjas för att illustrera användningsfallet och ger inblick i användargränssnittet.

#### <a name="role-site-and-subscription-admin"></a>Roll: Platsen och prenumeration admin

|Objekt      |Exempel|
|----------|------|
|Användarnamn: |`adminXX@contosowebstore.com`|
| Namn: |`Global Admin Azure PCI Samples`|
|Användartyp av:| `Subscription Administrator and Azure Active Directory Global Administrator`|

- Administratörskontot kan inte läsa informationen finansiella avmaskerad. Alla åtgärder har loggats.
- Administratörskontot kan inte hantera eller logga in på SQL-databas.
- Administratörskontot kan hantera Active Directory och prenumerationer.

#### <a name="role-sql-administrator"></a>Roll: SQL-administratör

|Objekt      |Exempel|
|----------|------|
|Användarnamn: |`sqlAdmin@contosowebstore.com`|
| Namn: |`SQLADAdministrator PCI Samples`|
| Förnamn: |`SQL AD Administrator`|
|Efternamn: |`PCI Samples`|
|Användartyp av:| `Administrator`|

- Sqladmin-konto kan inte visa ofiltrerade ekonomisk information. Alla åtgärder har loggats.
- Kontot sqladmin kan hantera SQL-databas.

#### <a name="role-clerk"></a>Roll: Clerk

|Objekt      |Exempel|
|----------|------|
|Användarnamn:| `receptionist_EdnaB@contosowebstore.com`|
| Namn: |`Edna Benson`|
| Förnamn:| `Edna`|
|Efternamn:| `Benson`|
| Användartyp av: |`Member`|

Edna Benson är hanteraren receptionist och verksamhet. Hon ansvarar för att säkerställa att kundinformation är korrekt och fakturering har slutförts. Edna är användaren inloggad för all interaktion med Contoso Webstore demo-webbplatsen. Edna har följande behörigheter: 

- Edna kan skapa och läsa kundinformation.
- Edna kan ändra kundinformation.
- Edna kan skriva över ekonomisk information.
- Edna konto kan inte visa ofiltrerade ekonomisk information.

> Contoso Webstore, användaren är automatiskt den **Edna** användaren för att testa funktionerna i en distribuerad miljö.

### <a name="contoso-webstore---estimated-pricing"></a>Contoso Webstore - uppskattade priser

Den här grundläggande arkitektur och exempel webbprogrammet har en månatlig avgift struktur och en användning kostnad per timme som måste beaktas vid bedömning av lösningen. Dessa kostnader kan beräknas med hjälp av den [Azure kostnadskrävande Kalkylatorn](https://azure.microsoft.com/pricing/calculator/). Från och med September 2017 månatliga uppskattade kostnaden för den här lösningen är ~ $2500 Detta omfattar en $ 1 000/månad användning kostnad för ASE v2. Dessa kostnader varierar beroende på hur användning och kan ändras. Har skyldighet att kunden beräkna de uppskattade kostnaderna för månatliga vid tidpunkten för distributionen för en mer tillförlitlig uppskattning. 

Den här lösningen används följande Azure-tjänster. Information om arkitektur för distribution finns i den [distributionsarkitektur](#deployment-architecture) avsnitt.

>- Application Gateway
>- Azure Active Directory
>- Apptjänst-miljö v2
>- OMS logganalys
>- Azure Key Vault
>- Nätverkssäkerhetsgrupper
>- Azure SQL-databas
>- Azure Load Balancer
>- Application Insights
>- Azure Security Center
>- Azure-webbapp
>- Azure Automation
>- Azure Automation-Runbooks
>- Azure DNS
>- Azure Virtual Network
>- Azure virtuell dator
>- Azure-resursgrupp och principer
>- Azure Blob Storage
>- Azure Active Directory rollbaserad åtkomstkontroll (RBAC)

## <a name="deployment-architecture"></a>Arkitektur för distribution

I följande avsnitt beskrivs element för utveckling och implementering.

### <a name="network-segmentation-and-security"></a>Nätverkssegmentering och säkerhet

![](images/pci-tiers-diagram.png)

#### <a name="application-gateway"></a>Application Gateway

Grundläggande arkitektur minskar risken för säkerhetsproblem som använder en Programgateway med en brandvägg för webbaserade program (Brandvägg) och OWASP RuleSet-metod aktiverad. Ytterligare funktioner är:

- [Slutpunkt till slutpunkt SSL](/azure/application-gateway/application-gateway-end-to-end-ssl-powershell)
- [SSL-avlastning](/azure/application-gateway/application-gateway-ssl-portal) aktiverad
- [TLS version 1.0 och v1.1](/azure/application-gateway/application-gateway-end-to-end-ssl-powershell) inaktiverad
- [Brandvägg för webbaserade program](/azure/application-gateway/application-gateway-webapplicationfirewall-overview) (Brandvägg läge)
- [Förebyggande läge](/azure/application-gateway/application-gateway-web-application-firewall-portal) med OWASP 3.0 RuleSet-metod
- [Diagnostikloggning](/azure/application-gateway/application-gateway-diagnostics) aktiverad
- [Anpassade hälsoavsökningar](/azure/application-gateway/application-gateway-create-gateway-portal)
- [Azure Security Center](https://azure.microsoft.com/services/security-center) och [Azure Advisor](/azure/advisor/advisor-security-recommendations), som ger ytterligare skydd och meddelanden. Azure Security Center innehåller också ett rykte system.

#### <a name="virtual-network"></a>Virtuellt nätverk

Grundläggande arkitektur definierar ett privat virtuellt nätverk med ett adressutrymme för 10.0.0.0/16.

#### <a name="network-security-groups"></a>Nätverkssäkerhetsgrupper

Varje nätverk nivåer har en dedikerad nätverkssäkerhetsgrupp (NSG):

- En DMZ nätverkssäkerhetsgrupp för brandväggen och programmet Gateway Brandvägg
- En NSG för hantering av jumpbox (skyddsmiljö host)
- En NSG för apptjänst-miljön

Var och en av de NSG: er har specifika portar och protokoll öppnas för säker och rätt drift av lösningen. Mer information finns i [PCI - vägledning för Nätverkssäkerhetsgrupper](#network-security-groups).

Dessutom kan är följande konfigurationer aktiverade för varje NSG:

- Aktiverad [diagnostikloggar och händelser](/azure/virtual-network/virtual-network-nsg-manage-log) lagras i storage-konto 
- Ansluten OMS logganalys till den [NSGS diagnostik](https://github.com/krnese/AzureDeploy/blob/master/AzureMgmt/AzureMonitor/nsgWithDiagnostics.json)

 
#### <a name="subnets"></a>Undernät
 Kontrollera varje undernät som är associerad med dess motsvarande NSG.

#### <a name="custom-domain-ssl-certificates"></a>Anpassad domän för SSL-certifikat
 HTTPS-trafik är aktiverad använder en anpassad domän för SSL-certifikat.

### <a name="data-at-rest"></a>Vilande data

Arkitekturen skyddar data i viloläge med hjälp av kryptering, granskning för databasen och andra åtgärder.

#### <a name="azure-storage"></a>Azure Storage

Till krypterade data i vila krav alla [Azure Storage](https://azure.microsoft.com/services/storage/) använder [Lagringstjänstens kryptering](/azure/storage/storage-service-encryption).

#### <a name="azure-sql-database"></a>Azure SQL Database

Azure SQL Database-instans använder följande säkerhetsåtgärder för databasen:

- [AD-autentisering och auktorisering](/azure/sql-database/sql-database-aad-authentication)
- [Granskning av SQL-databas](/azure/sql-database/sql-database-auditing-get-started)
- [Transparent datakryptering](/sql/relational-databases/security/encryption/transparent-data-encryption-azure-sql)
- [Regler i brandväggen](/azure/sql-database/sql-database-firewall-configure), så att för ASE arbetarpooler och hantering av IP-klient
- [SQL-Hotidentifiering](/azure/sql-database/sql-database-threat-detection-get-started)
- [Alltid krypterade kolumner](/azure/sql-database/sql-database-always-encrypted-azure-key-vault)
- [SQL-databas dynamisk datamaskning](/azure/sql-database/sql-database-dynamic-data-masking-get-started), med hjälp av PowerShell-skript efter distributionen

### <a name="logging-and-auditing"></a>Granskning och loggning

[Operations Management Suite (OMS)](/azure/operations-management-suite/) kan ge Contoso Webstore utförlig loggning för alla system- och användaraktivitet får innehålla ekonomiska dataloggning. Ändringar kan granskas och verifiera noggrannhet. 

- **Aktivitetsloggar.**  [Aktivitetsloggar](/azure/monitoring-and-diagnostics/monitoring-overview-activity-logs) ger kunskaper om de åtgärder som utfördes på resurser i din prenumeration.
- **Diagnostikloggar.**  [Diagnostikloggar](/azure/monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs) är alla loggar som orsakat av varje resurs. Loggarna finns Windows-händelsesystemloggar, Azure Blob storage loggar, tabeller och loggar för kön.
- **Brandväggsloggar.**  Programgatewayen ger fullständig diagnostik och komma åt loggar. Brandväggsloggar är tillgängliga för Programgateway resurser som har en Brandvägg är aktiverad.
- **Logga arkivering.**  Alla diagnostikloggar är konfigurerade för att skriva till en central och krypterad Azure Storage-konto för arkivering med en definierad period (2 dagar). Loggarna är nu ansluten till Azure Log Analytics för bearbetning, lagring och dashboarding. [Logga Analytics](https://azure.microsoft.com/services/log-analytics) är en OMS-tjänst som hjälper dig att samla in och analysera data som genereras av resurser i molnet och lokala miljöer.

### <a name="encryption-and-secrets-management"></a>Kryptering och hemligheter management

Contoso Webstore alla känsliga data krypteras och använder Azure Key Vault för att hantera nycklar och förhindra hämtning av CHD.

- [Azure Key Vault](https://azure.microsoft.com/services/key-vault/) hjälper dig att skydda krypteringsnycklar och hemligheter som används av molnprogram och tjänster. 
- [SQL TDE](/sql/relational-databases/security/encryption/transparent-data-encryption) används för att kryptera alla kundens finansiella data.
- Data lagras på disk med [Azure Disk Encryption](/azure/security/azure-security-disk-encryption) och BitLocker.

### <a name="identity-management"></a>Identitetshantering

Följande tekniker hanteringsfunktioner identitet i Azure-miljön.

- [Azure Active Directory (AD Azure)](https://azure.microsoft.com/services/active-directory/) är Microsoft flera innehavare molnbaserade och identitetshanteringsfunktioner hanteringstjänsten. Alla användare för lösningen har skapats i Azure Active Directory, inklusive användare med åtkomst till SQL-databasen.
- Tillämpningen utförs autentiseringen med hjälp av Azure AD. Mer information finns i [integrera program med Azure Active Directory](/azure/active-directory/develop/active-directory-integrating-applications). Dessutom använder kolumnen databaskryptering också Azure AD autentisera programmet till Azure SQL Database. Mer information finns i [Always Encrypted: skydda känsliga data i SQL-databas](/azure/sql-database/sql-database-always-encrypted-azure-key-vault). 
- [Azure Active Directory-identitetsskydd](/azure/active-directory/active-directory-identityprotection) identifierar potentiella säkerhetsrisker som kan påverka din organisations identiteter, konfigurerar du automatiska svar på identifierade misstänkta åtgärder som rör din organisations identiteter och undersöker misstänkta incidenter och vidtar lämpliga åtgärder som kan lösas.
- [Azure rollbaserad åtkomstkontroll (RBAC)](/azure/active-directory/role-based-access-control-configure) aktiverar exakt fokuserad åtkomsthantering för Azure. Prenumerationen åtkomst begränsas till administratör för prenumeration och Azure Key Vault åtkomsten är begränsad till alla användare.

Mer information om hur du använder säkerhetsfunktionerna i Azure SQL-databasen finns på [Contoso kurs Demo-programmet](https://github.com/Microsoft/azure-sql-security-sample) exempel.
   
### <a name="web-and-compute-resources"></a>Webb- och beräkna resurser

#### <a name="app-service-environment"></a>App Service Environment

[Azure Apptjänst](/azure/app-service/) är en hanterad tjänst för att distribuera webbprogram. Contoso Webstore program distribueras som en [App Service Web App](/azure/app-service-web/app-service-web-overview).

[Azure Apptjänst-miljö (ASE v2)](/azure/app-service/app-service-environment/intro) är en funktion i App Service som tillhandahåller en helt isolerad och dedikerad miljö för säker körning av Apptjänst-appar i hög skala. Det är en Premium service-plan som används av den här grundläggande arkitektur för att aktivera PCI DSS.

ASEs är isolerad för att endast en enskild kund program som körs och alltid har distribuerats till ett virtuellt nätverk. Kunder har detaljerad kontroll över både inkommande och utgående nätverkstrafik och program kan upprätta säkra höghastighetsanslutning över virtuella nätverk till lokala företagsresurser.

Användning av ASEs för den här arkitekturen som tillåts för följande kontroller/konfigurationer:

- Värden i en skyddad virtuella nätverket och nätverk säkerhetsregler
- ASE som konfigurerats med självsignerat ILB certifikat för HTTPS-kommunikation
- [Intern belastningsutjämning läge](/azure/app-service-web/app-service-environment-with-internal-load-balancer) (3-läge)
- [TLS 1.0](/azure/app-service-web/app-service-app-service-environment-custom-settings) inaktiverad
- [TLS-chiffer](/azure/app-service-web/app-service-app-service-environment-custom-settings) ändras
- Kontrollen [inkommande trafik N-/ skrivåtkomst portar](/azure/app-service-web/app-service-app-service-environment-control-inbound-traffic) 
- [Brandvägg - begränsa Data](/azure/app-service-web/app-service-app-service-environment-web-application-firewall)
- [SQL-databas trafik](/azure/app-service-web/app-service-app-service-environment-network-architecture-overview) tillåts


#### <a name="jumpbox-bastion-host"></a>Jumpbox (skyddsmiljö host)

Eftersom Apptjänst-miljön är skyddad och låst, måste det finnas en mekanism för att tillåta för DevOps-versioner eller ändringar som kan krävas, till exempel möjligheten att övervaka webbprogrammet med Kudu. En virtuell dator är skyddad bakom belastningsutjämnaren NAT, vilket ger möjlighet att ansluta till den virtuella datorn på en annan port än TCP 3389. 

En virtuell dator har skapats som en jumpbox (skyddsmiljö host) med följande konfigurationer:

-   [Tillägg för program mot skadlig kod](/azure/security/azure-security-antimalware)
-   [OMS-tillägg](/azure/virtual-machines/virtual-machines-windows-extensions-oms)
-   [Azure Diagnostics-tillägget](/azure/virtual-machines/virtual-machines-windows-extensions-diagnostics-template)
-   [Azure Disk Encryption](/azure/security/azure-security-disk-encryption) med hjälp av Azure Key Vault 
-   En [automatisk avstängning princip](https://azure.microsoft.com/blog/announcing-auto-shutdown-for-vms-using-azure-resource-manager/) du minskar användningen av virtuella datorresurser som

### <a name="security-and-malware-protection"></a>Säkerhet och skadlig kod protection

[Azure Security Center](https://azure.microsoft.com/services/security-center/) innehåller en centraliserad för säkerhetsläget för alla Azure-resurser. En överblick över kan du kontrollera att lämpliga säkerhetsåtgärder som är installerade och korrekt konfigurerad och att du snabbt kan identifiera alla resurser som kräver uppmärksamhet.  

[Azure Advisor](/azure/advisor/advisor-overview) är personliga molnet konsult som hjälper dig att följa bästa praxis för att optimera din Azure-distributioner. Den analyserar dina resurskonfigurationen och telemetri och rekommenderar lösningar som kan förbättra kostnadseffektivitet, prestanda, hög tillgänglighet och säkerhet för dina Azure-resurser.

[Microsoft Antimalware](/azure/security/azure-security-antimalware) för Azure-molntjänster och virtuella datorer är realtidsskydd funktion som hjälper dig att identifiera och ta bort virus, spionprogram och annan skadlig programvara med konfigurerbara aviseringar när kända skadliga eller oönskade programvara försöker installeras eller köras på Azure-system.

### <a name="operations-management"></a>Åtgärdshantering

#### <a name="application-insights"></a>Application Insights

Använd [Programinsikter](https://azure.microsoft.com/services/application-insights/) och få tillämplig insikter genom programhantering för prestanda och instant analyser.

#### <a name="log-analytics"></a>Log Analytics

[Logga Analytics](https://azure.microsoft.com/services/log-analytics/) är en tjänst i Operations Management Suite (OMS) och som hjälper dig att samla in och analysera data som genereras av resurser i molnet och lokala miljöer.

#### <a name="oms-solutions"></a>OMS-lösningar

Dessa ytterligare OMS-lösningar ska anses vara och konfigureras: 
- [Activity Log Analytics](/azure/monitoring-and-diagnostics/monitoring-overview-activity-logs)
- [Azure-nätverksanalys](/azure/log-analytics/log-analytics-azure-networking-analytics?toc=%2fazure%2foperations-management-suite%2ftoc.json)
- [Azure SQL Analytics](/azure/log-analytics/log-analytics-azure-sql)
- [Spårning av ändringar](/azure/log-analytics/log-analytics-change-tracking?toc=%2fazure%2foperations-management-suite%2ftoc.json)
- [Key Vault-analys](/azure/log-analytics/log-analytics-azure-key-vault?toc=%2fazure%2foperations-management-suite%2ftoc.json)
- [Tjänstkarta](/azure/operations-management-suite/operations-management-suite-service-map)
- [Säkerhet och granskning](https://www.microsoft.com/cloud-platform/security-and-compliance)
- [Programvara mot skadlig kod](/azure/log-analytics/log-analytics-malware?toc=%2fazure%2foperations-management-suite%2ftoc.json)
- [Hantering av uppdateringar](/azure/operations-management-suite/oms-solution-update-management)

### <a name="security-center-integration"></a>Security Center-integrering

Standarddistribution är avsedd att ge en basnivå av Security Center rekommendationer som indikerar konfigurationstillståndet felfri och säkra. Du kan aktivera insamling av data från Azure Security Center. Mer information finns i [Azure Security Center - komma igång](/azure/security-center/security-center-get-started).

## <a name="deploy-the-solution"></a>Distribuera lösningen

Komponenter för att distribuera den här lösningen är tillgängliga i den [betalning bearbetning utkast databasen][code-repo]. Distributionen av den grundläggande arkitekturen kräver flera steg utförs via Microsoft PowerShell v5. Du måste ange ett eget domännamn (t.ex contoso.com) för att ansluta till webbplatsen. Detta anges med hjälp av den `-customHostName` växla i steg 2. Mer information finns i [köpa ett anpassat domännamn för Azure Web Apps](/azure/app-service-web/custom-dns-web-site-buydomains-web-app). Ett anpassat domännamn krävs inte för att distribuera och köra lösningen, men du kan inte ansluta till webbplatsen i exempelsyfte.

Skripten lägga till domänanvändare i Azure AD-klient som du anger. Microsoft rekommenderar att skapa en ny Azure AD-klient om du vill använda som ett test.

Om det uppstår problem under distributionen finns [vanliga frågor och felsökning](https://github.com/Azure/pci-paas-webapp-ase-sqldb-appgateway-keyvault-oms/blob/master/pci-faq.md).

Microsoft rekommenderar starkt att en ren installation av PowerShell används för att distribuera lösningen. Du kan också kontrollera att du använder de senaste moduler som krävs för korrekt körning av installationsskript. Det här exemplet loggar in till jumpbox (skyddsmiljö host) och kör följande kommandon. Observera att detta gör att kommandot anpassade domäner.


1. Installera moduler som krävs och korrekt konfigurerade rollerna administratör.
 
    ```powershell
     .\0-Setup-AdministrativeAccountAndPermission.ps1 
        -azureADDomainName contosowebstore.com
        -tenantId XXXXXXXX-XXXX-XXXX-XXXX-XXXXXXXXXXXX
        -subscriptionId XXXXXXXX-XXXX-XXXX-XXXX-XXXXXXXXXXXX
        -configureGlobalAdmin 
        -installModules
    ```
    Detaljerade instruktioner finns i [skript instruktioner - installationsprogrammet administratörskonto och behörigheten](https://github.com/Azure/pci-paas-webapp-ase-sqldb-appgateway-keyvault-oms/blob/master/0-Setup-AdministrativeAccountAndPermission.md).
    
2. Installera lösningen-uppdateringshantering. 
 
    ```powershell
    .\1-DeployAndConfigureAzureResources.ps1 
        -resourceGroupName contosowebstore
        -globalAdminUserName adminXX@contosowebstore.com 
        -globalAdminPassword **************
        -azureADDomainName contosowebstore.com 
        -subscriptionID XXXXXXXX-XXXX-XXXX-XXXX-XXXXXXXXXXXX 
        -suffix PCIcontosowebstore
        -customHostName contosowebstore.com
        -sqlTDAlertEmailAddress edna@contosowebstore.com 
        -enableSSL
        -enableADDomainPasswordPolicy 
    ```
    
    Detaljerade instruktioner finns i [skript instruktioner – distribuera och konfigurera Azure-resurser](https://github.com/Azure/pci-paas-webapp-ase-sqldb-appgateway-keyvault-oms/blob/master/1-DeployAndConfigureAzureResources.md).
    
3. OMS loggning och övervakning. När lösningen har distribuerats en [Microsoft Operations Management Suite (OMS)](/azure/operations-management-suite/operations-management-suite-overview) arbetsytan kan öppnas och exempelmallarna i lösningen databasen kan användas för att illustrera hur du kan konfigurera en instrumentpanelen för övervakning . Exempelmallarna OMS finns i den [omsDashboards mappen](https://github.com/Azure/pci-paas-webapp-ase-sqldb-appgateway-keyvault-oms/blob/master/1-DeployAndConfigureAzureResources.md). Observera att måste samlas in i OMS mallar för att distribuera på rätt sätt. Detta kan ta upp till en timme eller mer beroende på platsaktivitet.
 
    När du konfigurerar din OMS-loggning överväga att använda följande resurser:
 
    - Microsoft.Network/applicationGateways
    - Microsoft.Network/NetworkSecurityGroups
    - Microsoft.Web/serverFarms
    - Microsoft.Sql/servers/databases
    - Microsoft.Compute/virtualMachines
    - Microsoft.Web/sites
    - Microsoft.KeyVault/Vaults
    - Microsoft.Automation/automationAccounts
 

    
## <a name="threat-model"></a>Hotmodell

En data-Flödesdiagram (DFD) och exempel hotmodell för Contoso Webstore [betalning bearbetning utkast Hotmodell](https://aka.ms/pciblueprintthreatmodel).

![](images/pci-threat-model.png)



## <a name="customer-responsibility-matrix"></a>Kunden ansvar matris

Kunder ansvarar för att behålla en kopia av den [ansvar sammanfattning matrisen](https://aka.ms/fsiblueprintcrm), vilket beskrivs FFIEC kraven som är ansvar för kunden och de som har ansvar för Microsoft Azure.



## <a name="disclaimer-and-acknowledgments"></a>FRISKRIVNING och bekräftelser

*September 2017*

- Det här dokumentet är endast i informativt syfte. MICROSOFT OCH AVYAN GÖR INGA GARANTIER, UTTRYCKLIGA, UNDERFÖRSTÅDDA ELLER UNDERFÖRSTÅDDA, AVSEENDE INFORMATIONEN I DET HÄR DOKUMENTET. Detta dokument tillhandahålls ”som-är”. Information och åsikter som uttrycks i detta dokument, inklusive Webbadresser och andra webbplatsreferenser, kan ändras utan föregående meddelande. Kunder som det här dokumentet ansvar använder den.  
- Det här dokumentet innehåller inte kunder med inga juridiska rättigheter till någon immateriell egendom i någon Microsoft eller Avyan produkt eller lösningar.  
- Kunderna får kopiera och använda det här dokumentet som intern referens.  

  > [!NOTE]
  > Vissa rekommendationerna i det här dokumentet kan resultera i ökade data, nätverk eller beräkning Resursanvändning i Azure och kan öka kostnaderna för en kund Azure licens eller prenumeration.  

- Lösningen i det här dokumentet är avsett som en grundläggande arkitektur och får inte användas som-är för produktion. För att uppnå kompatibilitet krävs att kunder konsultera sina granskare att verifiera den slutliga kund lösningen.  
- Alla kundnamn, transaktionsposter och eventuella relaterade data på den här sidan är fiktiva, skapas för denna grundläggande arkitektur och endast för jämförelseändamål. Ingen verklig associering eller koppling är avsedd och oavsiktliga ingen.  
- Den här lösningen har utvecklats gemensamt av Microsoft och Avyan rådgivning och är tillgängliga under den [MIT-licensen](https://opensource.org/licenses/MIT).

### <a name="document-authors"></a>Dokumentförfattare

* *Frank Simorjay (Microsoft)*  

[code-repo]: https://github.com/Azure/pci-paas-webapp-ase-sqldb-appgateway-keyvault-oms "Databasen"
