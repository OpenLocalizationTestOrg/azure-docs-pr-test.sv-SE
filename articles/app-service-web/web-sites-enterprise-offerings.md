---
title: "Azure App Service Web Apps erbjudanden för företag"
description: "Visar hur du använder Azure App Service Web Apps för att skapa enterprise webbplats lösningar för ditt företag"
services: app-service\web
documentationcenter: 
author: apwestgarth
manager: erikre
editor: 
ms.assetid: cf9ac3b2-0493-4461-8b64-251d3a5cd5b5
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/29/2016
ms.author: anwestg
ms.openlocfilehash: 4d46654f42a3fd5c9b491f1b565c2acfa0dc52c4
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="azure-app-service-web-apps-offerings-for-enterprise-whitepaper"></a>Azure App Service Web Apps erbjudanden för Enterprise-rapporten
Behovet av att minska kostnaderna och leverera IT-lösningar snabbare i en miljö med utvecklas snabbt skapar nya utmaningar för utvecklare och IT-proffs chefer. Användare söker allt sina rad Affärsappar (LOB)-webbprogram ska snabb, effektiv och tillgänglig från valfri enhet. På samma gång företag försöker utnyttja ökad produktivitet och effektivitet som kommer från integrering med molntjänster och mobila tjänster, detta kan vara något så enkelt som enkel inloggning över enheter med hjälp av Active Directory på samarbete i Office 365 hämtas med hjälp av data från en intern LOB-App som i sin tur tar emot data från företagets implementering av Salesforce. [Azure App Service Web Apps](http://go.microsoft.com/fwlink/?LinkId=529714) är en molntjänst i företagsklass för utveckling, testning och kör webb- och mobilprogram, webb-API: er och allmänna webbplatser. Den kan användas för att köra företagets webbplatser, intranätplatser, branschspecifika appar och digitala marknadsföringskampanjer på ett globalt nätverk av datacenter som är optimerade för skalning och tillgänglighet, tillsammans med stöd för kontinuerlig integrering och moderna DevOps-metoder.  

Den här rapporten visar funktionerna i den [Web Apps](/services/app-service/web/) service fokuserar särskilt på LOB-webbprogram, som omfattar migrering av befintliga webbprogram som körs och distribution av nya LOB webbprogram på plattformen.

## <a name="audience"></a>Målgrupp
IT-proffs, arkitekter och chefer som är ute efter för att migrera till molnet web arbetsbelastningar som körs lokalt. Web arbetsbelastningar kan sträcka sig över ett företag till medarbetare eller Business till Partners webbprogram.

## <a name="introduction"></a>Introduktion
App Service Web Apps är en utmärkt plattform som ska vara värd för både externa och interna webbprogram och tjänster som ger en kostnadseffektiva, skalbara, hanterade lösning som gör att du kan koncentrera dig på att leverera affärsvärde för användarna i stället utgiftsgränsen stora mängder tid och pengar underhåll och stöd för olika miljöer. Web Apps är en flexibel plattform som du vill distribuera webbprogram enterprise erbjuder möjligheten att fortsätta att autentisera mot lokala Active Directory via integrering med Microsoft Azure Active Directory, stöd för snabb och enkel distributioner som gör användningen av din interna kontinuerlig integrering och distribution praxis, samtidigt som automatiskt för att växa med affärsbehov - alla på en hanterad plattform som gör att du kan fokusera på programmet och inte infrastrukturen.

## <a name="problem-definition"></a>Problemdefinition
IT-liggande ändras snabbt, med en flytt från värd på traditionella servrar med sina high utgifter i långa ledtider till en som används på begäran som använder tjänster som skala automatiskt för att hantera belastningen. IT-avdelningar som anropas för att minska kostnaderna och storleken på infrastruktur och underhåll tillbringar med fokus på minskar CAPEX och även öka flexibilitet. I slutet av livslängden för äldre infrastruktur-plattformar, till exempel Windows Server 2003, leder IT-avdelningar att granska molnmigrering som potentiella så att nya långsiktiga utgifter. Tidigare blir IT-chefer inköpsbeslut för andra avdelningar; dock tar allt CMOs och andra företag enhet huvuden en mer aktiva roll i hur deras budget används och avkastning på investeringar är. Företag behöver mer och mer personalen att vara mycket mer mobila än någonsin med anställda arbetar via fjärranslutning, tillbringar mer tid med kunder som behöver åtkomst till enkel system.

Affärsbehov behöver ändras varje månad, varje vecka, varje dag. Företag söker efter instant global skala med regelbundet uppdaterade tjänsterna fullständig av nya funktioner som tillhandahålls av tredje part eller internt.  I vissa fall företag också söker funktioner att isolera sina program och åtkomst till resurser, samtidigt som också gör användningen av offentliga molntjänster lokaler. Användare har högre förväntningar med många som använder sig av tjänster i sina egna privatliv, till exempel Office 365. De förväntas att få tillgång till liknande, uppdaterad, funktionen omfattande tjänster i livscykeln arbete. Att hantera begäran, IT måste utseende som hjälper företag att aktivera detta via val och integrering med tredjeparts-tjänsterna, försiktig med plattformar som kan anpassas till affärsbehov, medan också som tillförlitliga med en minskad totalkostnad ägarskap.

Utvecklingsgrupper vill leverera omedelbar business fördelar, leverera nya funktioner regelbundet. De letar efter en kostnadseffektiv, tillförlitlig plattform som kan integreras med sina befintliga verktyg och praxis – utveckling, testa, släpper; och arbeta tillsammans med IT-avdelningar automatiserar distribution, hantering och avisering, alla med målet att driftstopp.

<a name="highlevel"></a>
## <a name="high-level-solution"></a>Hög nivå lösning
Web-plattformar och ramverk som allt används för att utveckla, testa och affärsprogram.  Med en typisk linje affärsprogram, t.ex en intern medarbetare utgifter system ofta som endast består av ett webbprogram med en stödjande-databas för att lagra data som är kopplad till programmet.

App Service Web Apps är ett bra alternativ för värd för sådana program med skalbar och tillförlitlig infrastruktur som hanteras och korrigerade med nära noll manuella åtgärder och driftstopp. Microsoft Azure-plattformen ger många alternativ för datalagring till stöd för webbprogram i webbprogram från Microsoft Azure SQL-databas, en hanterad skalbara relationell databas som en-tjänst, till populära tjänster från våra partners, till exempel ClearDB MySQL Databasen och MongoDB.

En annan metod är att använda din befintliga investeringar lokalt. I exemplet, en medarbetare utgifter system, kanske du vill underhålla datalager i din egen interna infrastrukturen. Det kan vara för integrering med interna system (reporting, lön, fakturering osv) eller att uppfylla kravet en IT-styrning.  Web Apps innehåller ett antal metoder för att du kan ansluta till din på lokala infrastruktur:

* [Apptjänstmiljöer](app-service-app-service-environment-intro.md) -App Service miljöer (ASE) är en ny Premium-funktion som nyligen har lägga till Microsoft Azure App Service-erbjudandet.  ASEs ger en helt isolerad och dedikerad miljö för att köra Azure Apptjänst-appar på ett säkert sätt i hög skala samtidigt som den ger isolering och säker nätverksåtkomst   
* [Hybridanslutningar](../biztalk-services/integration-hybrid-connection-overview.md) – Hybridanslutningar är en funktion i Microsoft Azure BizTalk-tjänst och aktivera Web Apps att ansluta till lokala resurser på ett säkert sätt, till exempel SQL Server, MySQL, webb-API: er och anpassade webbtjänster.
* [Virtuell nätverksintegration](https://azure.microsoft.com/blog/2014/09/15/azure-websites-virtual-network-integration/) – Web Apps integrering med Azure Virtual Network kan du ansluta ditt webbprogram till ett virtuellt Azure-nätverk som i sin tur kan vara ansluten till din på lokala infrastruktur via ett plats-till-plats-VPN.

Följande diagram visas ett exempel på hög nivå lösning med anslutningsalternativ för på lokala resurser.  Det första exemplet visar hur du kan göra detta med hjälp av standardfunktioner i Azure App Service och andra visas hur det kan vara uppnås med hjälp av premium ger Apptjänstmiljöer.

Använder funktioner som Standard Apptjänst:![](./media/web-sites-enterprise-offerings/on-premise-connectivity-solutions.png "Using Standard App Service Features")

Med hjälp av en Apptjänst-miljö:![](./media/web-sites-enterprise-offerings/on-premise-connectivity-solutions-ASE.png "Using an App Service Environment")

## <a name="business-benefits"></a>Företagets fördelar
App Service Web Apps innehåller en mängd fördelar för företag som gör det möjligt att funktionen ska vara mycket mer kostnadseffektivt och flexibel leverera för företagets behov.

### <a name="paas-model"></a>PaaS-modellen
App Service Web Apps bygger på en plattform som en modell som innehåller ett antal kostnader och effektivitet besparingar.  Inte längre behöver du tillbringar timmar hantera virtuella datorer, operativsystem och ramverk-korrigering. Web Apps är en automatiskt korrigeringsfil miljö där du kan fokusera på hanteringen av dina webbprogram och inte virtuella datorer, lämnar team kan ge ytterligare affärsvärde.

PaaS-modellen som ligger till grund för Web Apps aktiverar jobbar DevOps-metod att uppfylla sina mål. Som ett företag innebär fullständig hantering och integrering i hela programmet för den hela livscykeln, inklusive utveckling, testning, version, övervakning och hantering och support.

För utvecklingsgrupper, kontinuerlig integration och arbetsflöden för distribution kan konfigureras från Visual Studio Team Services, GitHub, TeamCity, Hudson eller BitBucket, aktivera automatisk build test och distribution aktiverar snabbare släpper cykler samtidigt som den friktion ingår i lanserar i den befintliga infrastrukturen. Web Apps också stöder skapandet av flera testnings- och mellanlagringsmiljöer för versionen arbetsflödet, inte längre behöver du reservera eller allokera maskinvara för dessa ändamål, du kan skapa så många miljöer som du vill och definiera dina egna befordran till Släpp arbetsflöde. Befordra till en plats i steg som ett företag som du kan välja att övergång till en test-fack från källkontroll, utföra en serie med tester och vid slutförande och slutligen växla till produktionsmiljö utan avbrott med extra förmån som webbprogram som är värd för webbprogram är förinstallerade och Varm ge bästa möjliga kundupplevelse.  Företag kan dessutom göra använder testar produktion funktionerna i App Service Web Apps för att dirigera en del av trafik till en annan plats, verifiera ändringarna, innan du byter all trafik till den nya distributionen eller återställa all trafik till den tidigare distribution.

Team lita på att de är bästa möjliga möjlighet att ta hänsyn till eventuella problem med någon av sina webbprogram på Web Apps med inbyggda funktioner för övervakning och aviseringar. Team bör redan har investerat i analyser och övervakar lösningar sådana från Microsoft Visual Studio Application Insights, New Relic och AppDynamics. Dessa stöds också fullt ut i Web Apps aktiverar affärskontinuitet och välkända miljöer som du vill övervaka ditt webbprogram.

Slutligen Web Apps ger funktioner för automatisk säkerhetskopiering av dina appar och värdbaserade databaser direkt till en Azure Blob Storage-behållare. Ger dig ett enkelt sätt och mycket kostnadseffektivt sätt som du vill återställa från katastrofåterställning, vilket minskar behovet av komplexa på lokal maskinvara och programvara.

### <a name="ease-of-migration"></a>Enkel migrering
Maskinvara underhåll och rotation är viktigt för företag som versionen cykler för maskinvara och operativsystem snabbare. Kanske du har ett antal Windows Server 2003 R2-servrar som kommer till slutet av stödet i 2015, men de fortfarande är värd för viktiga webbprogram för ditt företag? App Service Web Apps är en bra kandidat som ska vara värd för dessa webbprogram och att rationalisera egendom för företag-maskinvaran. Web Apps får du tillgång till en mängd olika maskinvaruspecifikationer som hanteras och underhålls som en del av tjänsten, vilket eliminerar behovet av att ta hänsyn till ersättning och hanteringskostnader som en del av din infrastruktur budget.  Migreringen kan vara så enkelt som en kopia och klistra in från den befintliga distributionen till webbprogram eller en mer komplex migrering där med hjälp av Web Apps migrering Assistant lägger till värdet. Migrerade webbprogram få hela skalan av Azure-tjänster, integrera ytterligare tjänster till webbprogram. Exempelvis kan du lägga till Azure Active Directory för att styra åtkomsten till ditt program baserat på användarorganisation till säkerhetsgrupper. Ett annat exempel kan att lägga till Cache-tjänster för att förbättra prestandan och minska svarstiden, förutsatt att den totala bättre användarupplevelse.

### <a name="enterprise-class-hosting"></a>Värd för Enterprise-klass
App Service Web Apps är en stabil, tillförlitlig plattform som har visat för att kunna hantera en mängd olika företag behöver från små interna utveckling och testning arbetsbelastningar, hög belastning på hög skalade webbplatser. Med hjälp av Web Apps gör du använder samma enterprise klassen värd plattform som Microsoft som företaget använder för högt värde web arbetsbelastningar. Webbappar, tillsammans med alla tjänster på Azure-plattformen är byggda med säkerhet och kompatibilitet med krav, till exempel ISO (ISO/IEC 27001: 2005); SOC1 och SOC2 SSAE 16/ISAE 3402 intyg, HIPAA BAA, PCI och Fedramp mycket hjärtat i varje element och funktionen för mer information finns [http://aka.ms/azurecompliance](/support/trust-center/compliance/).

Microsoft Azure-plattformen gör rollen auktorisering kontroller att aktivera enterprise nivåer av kontroll till resurser i Web Apps. RBAC kan företag att genomföra sina egna principer för hantering av åtkomst för alla sina tillgångar i Azure-miljö genom att tilldela användare till grupper och i sin tur ge behörighet till dessa grupper mot tillgången, till exempel en webbapp. Mer information om RBAC i Azure finns [http://aka.ms/azurerbac](../active-directory/role-based-access-control-configure.md). Du kan vara att ditt webbprogram har distribuerats i en säkrare miljö och du har fullständig behörighet till vilka territorium dina tillgångar distribueras genom att använda Web Apps.

Azure Apptjänstmiljöer [http://aka.ms/aseintro](http://aka.ms/aseintro) är ett nytt alternativ för premium service-plan för enterprise-kunder som vill använda Azure App Service och de anger en helt isolerad och dedikerad miljö.  Detta gör det möjligt för företagskunder att distribuera program som kan dra nytta av storskalighet samtidigt också ha full kontroll över inkommande och utgående nätverkstrafik och ASEs att programmen ska ha hög hastighet säkra anslutningar över virtuella nätverk till lokala resurser.

App Service Web Apps kan även utnyttja dina på lokala-investeringar genom att erbjuda möjligheten att ansluta till ditt interna resurser, till exempel din data warehouse eller SharePoint-miljön. Enligt beskrivningen i [hög lösning](#high-level-solution) kan du använda Hybridanslutningar och virtuell nätverksanslutning för att upprätta anslutningar till på lokal infrastruktur och tjänster.

### <a name="global-scale"></a>Global skala
App Service Web Apps är en global och skalbar plattform, aktivera ditt webbprogram att utöka och anpassa behoven hos ett växande företag snabbt och med minimal långsiktiga planerings- och kostnaden. I vanliga på lokal infrastruktur scenarier expansion och ökad efterfrågan både lokalt och geografiskt skulle kräver en stor del av hantering, planering och utgifter för att etablera och hantera ytterligare infrastruktur. Web Apps ger dig möjlighet att skala ditt webbprogram med ebb och flödet av dina krav. Till exempel med programmet kostnader som exempel, för flesta månaden användarna är lätta användare av programmet men när tidsgränsen ökar varje månad utgifter bidrag anges och användning för ditt program, Web Apps har den möjlighet att automatiskt etablera mer infrastrukturen för ditt program och sedan när användningen har subsided igen den kan skala tillbaka till baslinje-infrastruktur som du definierar.

Web Apps är tillgängligt globalt i 24 Datacenter över hela världen och växande. Senaste uppdaterade listan över regioner och plats, se [http://aka.ms/azlocations](http://aka.ms/azlocations). Med Web Apps kan ditt företag enkelt åstadkomma globalt omfattande och skala. När företaget växer i nya områden, reporting instrumentpanelerna för programmet som du använder och värden på Web Apps kan enkelt distribueras till ytterligare datacenter och hantera lokala användare mycket snabbare genom en kombination av Web Apps och Azure Traffic Manager alla med den ytterligare fördelen av skalbara infrastruktur under kan minimera och expandera när behoven i regionala kontor ändra.

## <a name="solution-details"></a>Information om lösningen
Nu ska vi titta på ett exempel på ett program Migreringsscenario. Detta visar information om hur App Service Web Apps funktioner komma samman för att ge bra lösning och värde för verksamheten.

I det här exemplet är raden affärsprogram som vi kommer att diskutera kostnad reporting program som gör det möjligt att skicka deras utgifterna för återbetalning. Programmet finns på en Windows Server 2003 R2 som kör IIS6 och databasen är en SQL Server 2005-databas. Orsaken vi väljer äldre server som ligger med kommande slutet av Service för Windows Server 2003 R2 och SQL Server 2005 och vi har [verktyg](http://aka.ms/websitesmigration) och [riktlinjer](http://aka.ms/websitesmigrationresources) att automatiskt migrera arbetsbelastningar till Azure. Med detta i åtanke gäller mönstret i detta exempel används en bred allvarlighetsgrad för inkompatibilitet för scenarier med migrering.

### <a name="migrate-existing-application"></a>Migrera befintliga program
Steg 1 av virtualiseringslösningen för att flytta ett line-of-business-program till webbprogram är att identifiera befintliga program tillgångar och arkitektur. Exemplet i det här dokumentet är en ASP.NET-webbprogram finns på en enda IIS-Server med finns på en separat SQL Server-databasen som visas i bilden nedan. Anställda inloggning i systemet med hjälp av en kombination av användarnamn och lösenord, de ange information om kostnader och ladda upp skannade kopior av inleveranser, till databasen, för varje dataobjekt utgifter.

![](./media/web-sites-enterprise-offerings/on-premise-app-example.png)

#### <a name="items-to-consider"></a>Sakerna att tänka på
När migreringsprogrammet från en lokal miljö, du kanske vill ha i åtanke, några begränsningar för Web Apps. Här följer några viktiga avsnitt vara medveten om när migrera webbprogram för Web Apps ([http://aka.ms/websitesmigrationresources](http://aka.ms/websitesmigrationresources)):

* Port bindningar – Web Apps endast har stöd för port 80 för HTTP och port 443 för HTTPS-trafik. Om ditt program använder någon annan port och sedan migreras när gör programmet använder port 80 för HTTP och port 443 för HTTPS-trafik. Det här är ofta en oskadliga problem Eftersom det är vanligt i lokala distributioner så att användning av olika portar för att förhindra användningen av domännamn, särskilt i miljöer för utveckling och testning
* Autentisering – Web Apps stöder anonym autentisering som standard och formulär för autentisering som identifieras av ett program. Web Apps kan erbjuda Windows-autentisering när programmet är integrerad med Azure Active Directory och AD FS endast. Detta är en funktion som beskrivs i detalj [här](http://aka.ms/azurebizapp)
* GAC baserat sammansättningar – Web Apps tillåter inte att distributionen av sammansättningar till den globala sammansättningscachen (GAC). Därför om programmet som ska migreras utnyttjar detta funktion lokalt bör du överväga att flytta sammansättningarna till bin-mappen för programmet.
* IIS5-Kompatibelt läge – Web Apps stöder inte IIS5-kompatibelt läge och som sådan varje Web Apps-instans och alla webbprogram under den överordnade Web Apps-instansen körs i samma arbetsprocesser i en enda programpool.
* Användning av COM-biblioteken – Web Apps tillåter inte registrering av COM-komponenter på plattformen. Därför om programmet gör användning av COM-komponenter kan dessa skulle behöva skrivas i förvaltad kod och distribueras med programmet.
* ISAPI-filter – ISAPI-filter har stöd för Web Apps. De måste distribueras som en del av programmet och registrerad i webbprogrammets web.config-filen. Mer information finns i [http://aka.ms/azurewebsitesxdt](web-sites-transform-extend.md).

När dessa avsnitt har bedömts vara, bör ditt webbprogram vara redo för molnet. Och oroa dig inte om vissa avsnitt inte uppfylls fullständigt Migreringsverktyget ger bästa prestanda för migrering.

Nästa steg i migreringen är att skapa en App Service webbapp och en Azure SQL Database. Det finns flera olika storlekar på Web Apps instanser med olika antal processorkärnor och RAM belopp som du kan välja baserat på din web applications krav. Mer information och prissättning, se [https://azure.microsoft.com/pricing/details/app-service/](https://azure.microsoft.com/pricing/details/app-service/). Microsoft Azure SQL Database caters på samma sätt för alla måste ett företag med olika servicenivåer och prestandanivåer för att uppfylla kraven. Mer information finns på [https://azure.microsoft.com/pricing/details/sql-database/](https://azure.microsoft.com/pricing/details/sql-database/). När du skapat programmet har överförts till App Service Web Apps, antingen via FTP- eller WebDeploy och gå sedan vidare till databasen.

I den här migreringen använder lösningen Azure SQL Database, men som inte bara databasen som stöds på Azure. Företag kan också göra använder MySQL MongoDB, Azure Cosmos DB och många fler via tillägg som kan köpas i den [Azure Store](/marketplace/partner-program/).

När skapar en Azure SQL Database till ett antal alternativ är tillgängliga för att importera en befintlig databas från en lokal server skapar ett skript för en befintlig databas till med hjälp av den [Data-tier Application-Export och Import](http://aka.ms/dacpac).

Programdatabasen utgifter har skapats genom att skapa en ny Azure SQL Database, ansluta till databasen med hjälp av SQL Server Management Studio och sedan köra ett skript för att skapa databasschemat och fylla det med data från den lokala databasen.

Det sista steget i den här första steget av migreringen kräver uppdatering av anslutningssträngar till databasen för programmet. Detta kan ske via Azure portal. Du kan ändra programspecifika inställningar, inklusive eventuella anslutningssträngar som används av programmet för att ansluta till en databas som används för varje webbprogram.

### <a name="alternatives-to-using-azure-sql-database"></a>Att använda Azure SQL Database
Azure-plattformen erbjuder ett antal alternativ till att använda Azure SQL-databas som en primär databas för web-program, detta är att låta olika arbetsbelastningar dvs. Använd en NoSQL-lösning eller för att aktivera plattformen efter behov för ett företag data. Ett företag kan innehålla data som inte måste lagras externt eller i en miljö med offentliga moln och därför skulle se ut för att upprätthålla användningen av sina lokala-databasen.

#### <a name="connectivity-to-on-premises-resources"></a>Anslutningen till lokala resurser
App Service Web Apps erbjuder flera alternativ för att ansluta till på lokala resurser, t.ex databaser, aktivera återanvändning av befintliga infrastruktur för högt värde. Alternativen är enligt nedan:

* Apptjänstmiljöer isolerade och skapas i ett undernät i ett virtuellt nätverk därför aktivera miljö för att kommunicera med privata slutpunkter i samma virtuella nätverk - [http://aka.ms/appserviceasenetworking](http://aka.ms/appserviceasenetworking)
* Web Apps virtuell nätverksintegration stöder integration mellan webbprogram och ett Azure Virtual Network, som tillåter åtkomst till resurser som körs i ditt virtuella nätverk som tillåter anslutning om anslutna till ditt lokala nätverk med plats-till-plats VPN direkt till din lokala system.
* Hybridanslutningar är en funktion i Azure BizTalk-tjänst och ger ett enkelt sätt att ansluta till enskilda lokala resurser, till exempel SQL Server, MySQL, http-webb-API: er och mest anpassade webbtjänster.

#### <a name="scale-and-resiliency"></a>Skalning och återhämtning
När ett företag växer dess personal via förvärv av organisationer eller fysisk organiskt, så måste för web skala program för att möta dessa nya behov. Verkligen idag är det vanligt att se en ännu större spridning av gruppen och fjärranslutna anställda, till exempel företag med kontor i USA, Europa och Asien, med en mobil försäljnings gällande många flera områden. Web Apps har möjlighet att hantera elastisk ändringar i skala bekvämt och automatiskt.

App Service Web Apps kan webbprogram konfigureras för att skala automatiskt via Azure portal, beroende på två angreppsmetoderna – schemalagda tider eller genom att CPU-användning. Autoskala för appar erbjuder en kostnadseffektiv och mycket flexibelt sätt till följd av större ändringar i användning för alla program, från webbprogram som vår bekostnad rapportsystem för försäljning webbplatser som får en hög burst av trafik under en kort period befordran. Mer information och tips om att skala ditt webbprogram med hjälp av Web Apps finns i [så skala webbplatser](web-sites-scale.md).

Förutom Web Apps skalning flexibilitet kan den övergripande plattformen kontinuitet för företag och återhämtning via möjlig distribution av webbprogram och deras tillgångar över flera datacenter och geografiska områden.

## <a name="summary"></a>Sammanfattning
App Service Web Apps erbjuder en flexibel, kostnadseffektiv, effektiv lösning för ett företag i en miljö med utvecklas snabbt dynamiska behov. Web Apps hjälper företag att öka produktiviteten och effektiviteten genom att använda en hanterad plattform med moderna DevOps-funktioner och minskade händerna på hantering, samtidigt som man enterprise-funktioner i skala, återhämtning, säkerhet och integrering med lokala tillgångar.

## <a name="call-to-action"></a>Anrop till åtgärd
Mer information om tjänsten Azure App Service Web Apps finns [http://aka.ms/enterprisewebsites](/services/websites/enterprise/) där mer information kan hämtas och registrera dig för en utvärderingsversion idag på [https://azure.microsoft.com/pricing /Free-Trial /](https://azure.microsoft.com/pricing/free-trial/) utvärdera tjänsten och identifiera fördelar för företaget.

[!INCLUDE [app-service-web-whats-changed](../../includes/app-service-web-whats-changed.md)]

[!INCLUDE [app-service-web-try-app-service](../../includes/app-service-web-try-app-service.md)]
