---
title: "aaaAzure App Service Web Apps erbjudanden för Enterprise"
description: "Visar hur toouse Azure App Service Web Apps toocreate webbplats företagslösningar för ditt företag"
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
ms.openlocfilehash: 5d551509219988160858ff35298d3c7275d1d071
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-app-service-web-apps-offerings-for-enterprise-whitepaper"></a>Azure App Service Web Apps erbjudanden för Enterprise-rapporten
Hej måste tooreduce kostnader och leverera IT-lösningar snabbare i en utvecklas snabbt miljö skapar nya utmaningar för utvecklare och IT-proffs chefer. Användare söker allt efter deras rad Affärsappar (LOB) web applications toobe snabb, effektiv och tillgänglig från valfri enhet. Vid hello samtidigt, företag försöker toocapitalize på hello ökad produktivitet och effektivitet som kommer från integrering med molntjänster och mobila tjänster, detta kan vara något så enkelt som enkel inloggning över enheter med hjälp av Active Directory toocollaboration i Office 365 med hjälp av data hämtas från en intern LOB-App som i sin tur tar emot data från hello företagets implementering av Salesforce. [Azure App Service Web Apps](http://go.microsoft.com/fwlink/?LinkId=529714) är en molntjänst i företagsklass för utveckling, testning och kör webb- och mobilprogram, webb-API: er och allmänna webbplatser. Det kan vara används toorun företagets webbplatser, intranätplatser, branschspecifika appar och digitala marknadsföringskampanjer på ett globalt nätverk av datacenter som optimerats för skalning och tillgänglighet, samt support för kontinuerlig integrering och moderna DevOps metoder.  

Den här rapporten visar hello funktionerna i hello [Web Apps](/services/app-service/web/) service fokuserar särskilt på LOB-webbprogram, som omfattar migrering av befintliga webbprogram som körs och distribution av nya LOB webbprogram på hello plattform.

## <a name="audience"></a>Målgrupp
IT-proffs, arkitekter och chefer som är ute efter toomigrate toohello web molnarbetsbelastningar som körs lokalt. Web arbetsbelastningar kan sträcka sig över antingen Business tooEmployee eller Business tooPartners webbprogram.

## <a name="introduction"></a>Introduktion
App Service Web Apps är en utmärkt plattform på vilken toohost både externa och interna web program och tjänster, eftersom det ger dig en kostnadseffektiv, mycket skalbar och hanterad lösning så att du tooconcentrate på att leverera affärsvärde för användarna i stället Avgränsa miljöer än utgifter stora mängder tid och pengar underhållet och ge support. Web Apps är en flexibel plattform på vilken toodeploy webbprogrammen enterprise erbjuder hello möjlighet toocontinue tooauthenticate mot lokala Active Directory via integrering med Microsoft Azure Active Directory har stöd för snabb och enkel distributioner som gör användningen av din interna kontinuerlig integrering och distribution praxis, samtidigt som automatiskt toogrow med hello affärsbehov - alla på en hanterad plattform som gör att du toofocus på ditt program och inte din infrastruktur.

## <a name="problem-definition"></a>Problemdefinition
hello ändrar IT liggande snabbt, med en flytt från värd på traditionella servrar med sina high utgifter på länge lead gånger tooone som använder på begäran användning av tjänster som kan skalas automatiskt toohandle belastningen. IT-avdelningar som handlingar tooreduce hello kostnad och storleken på infrastruktur och underhåll tillbringar med fokus på minskar CAPEX och även öka flexibilitet. hello slutet på livscykeln på äldre plattformar för infrastrukturen, till exempel Windows Server 2003, leder IT-avdelningar tooreview molnmigrering som en potentiella sätt tooavoid nya långsiktiga utgifter. I tidigare hello blir IT-chefer inköpsbeslut för andra avdelningar; dock allt CMOs och andra företag enhet huvuden tar en mer aktiva roll i hur deras budget används och vilka hello avkastning på investeringar är. Mer och mer behöver företag sina arbetsstyrka toobe mycket mer mobila än någonsin tidigare med anställda arbetar via fjärranslutning, tillbringar mer tid med kunder som behöver åtkomst toosystems problem ledigt.

Affärsbehov behöver ändras varje månad, varje vecka, varje dag. Företag söker efter instant global skala med regelbundet uppdaterade tjänsterna fullständig av nya funktioner som tillhandahålls av tredje part eller internt.  I vissa fall företag vill även ha hello funktioner tooisolate sina program och komma åt tooresources samtidigt som också gör användningen av offentliga molntjänster lokaler. Användare har högre förväntningar med många som använder sig av tjänster i sina egna privatliv, till exempel Office 365. De förväntar sig toohave åtkomst toosimilar in toodate funktionen omfattande tjänster i livscykeln arbete. toocope med den här begäran IT måste titta toohelp hello business tooenable detta via markering och integrering med tredjeparts-tjänsterna, försiktig med plattformar som du kan anpassa toohello affärsbehov samtidigt också som det är tillförlitlig med en minskad totalkostnad ägandekostnad.

Utvecklingsgrupper söker toodeliver omedelbar business förmånen, leverera nya funktioner regelbundet. De letar efter en kostnadseffektiv, tillförlitlig plattform som kan integreras med sina befintliga verktyg och praxis – utveckling, testa, släpper; och arbeta tillsammans med IT-avdelningar automatiserar distribution, hantering och aviseringar med hello målet för driftstopp.

<a name="highlevel"></a>
## <a name="high-level-solution"></a>Hög nivå lösning
Web-plattformar och ramverk allt som används för toodevelop, testa och värden verksamhetsspecifika program.  Med en typisk linje affärsprogram, t.ex en intern medarbetare utgifter system ofta som endast består av ett webbprogram med en stödjande toostore hello databasdata ansluten med hello program.

App Service Web Apps är ett bra alternativ för värd för sådana program med skalbar och tillförlitlig infrastruktur som hanteras och korrigerade med nära noll manuella åtgärder och driftstopp. hello Microsoft Azure-plattformen ger många data lagring alternativ toosupport webbprogram finns på Web Apps från Microsoft Azure SQL-databas, en hanterad skalbara relationell databas som en-tjänst, toopopular tjänster från våra partners, till exempel ClearDB MySQL-databas och MongoDB.

En annan metod är toomake använda din befintliga investeringar lokalt. I en medarbetare utgifter system i exemplet hello gärna toomaintain datalager i din egen interna infrastrukturen. Det kan vara för integrering med interna system (reporting, lön, fakturering osv) eller toosatisfy ett krav för IT-styrning.  Web Apps innehåller ett antal metoder för att aktivera du tooconnect tooyour på lokal infrastruktur:

* [Apptjänstmiljöer](app-service-app-service-environment-intro.md) -App Service miljöer (ASE) är en ny Premium-funktion som har nyligen lagt till toohello Microsoft Azure App Service erbjudande.  ASEs ger en helt isolerad och dedikerad miljö för att köra Azure Apptjänst-appar på ett säkert sätt i hög skala samtidigt som den ger isolering och säker nätverksåtkomst   
* [Hybridanslutningar](../biztalk-services/integration-hybrid-connection-overview.md) – Hybridanslutningar är en funktion i Microsoft Azure BizTalk-tjänst och aktivera Web Apps tooconnect tooon lokala resurser på ett säkert sätt, till exempel SQL Server, MySQL, webb-API: er och anpassade webbtjänster.
* [Virtuell nätverksintegration](https://azure.microsoft.com/blog/2014/09/15/azure-websites-virtual-network-integration/) – Web Apps integrering med Azure Virtual Network kan tooconnect din web app tooan Azure virtuella nätverk som i tur kan vara anslutna tooyour på lokal infrastruktur via ett plats-till-plats-VPN.

hello visas följande diagram ett exempel på hög nivå lösning med anslutningsalternativ för på lokala resurser.  hello första exemplet visar hur du kan göra detta med hjälp av standardfunktioner i Azure App Service och hello andra visar hur det kan vara uppnås med hjälp av hello premium ger Apptjänstmiljöer.

Använder funktioner som Standard Apptjänst:![](./media/web-sites-enterprise-offerings/on-premise-connectivity-solutions.png "Using Standard App Service Features")

Med hjälp av en Apptjänst-miljö:![](./media/web-sites-enterprise-offerings/on-premise-connectivity-solutions-ASE.png "Using an App Service Environment")

## <a name="business-benefits"></a>Företagets fördelar
App Service Web Apps innehåller en mängd fördelar för företag som gör det möjligt för din funktion toobe kostnadseffektiv och mycket mer flexibel leverera för hello affärsbehov.

### <a name="paas-model"></a>PaaS-modellen
App Service Web Apps bygger på en plattform som en modell som innehåller ett antal kostnader och effektivitet besparingar.  Inte längre behöver du toospend timmar hantera virtuella datorer, operativsystem och ramverk-korrigering. Web Apps är en automatiskt korrigeringsfil miljö där du toofocus på hanteringen av dina webbprogram och inte virtuella datorer, lämnar team ledigt tooprovide ytterligare affärsvärde.

hello PaaS-modell som ligger till grund för Web Apps aktiverar de som jobbar hello DevOps metoder toofulfill sina mål. Som ett företag innebär fullständig hantering och integrering under hello programmet hela livscykel, inklusive utveckling, testning, version, övervakning och hantering och support.

För utvecklingsgrupper, kontinuerlig integration och arbetsflöden för distribution kan konfigureras från Visual Studio Team Services, GitHub, TeamCity, Hudson eller BitBucket, släpper aktivera automatisk build test och distribution aktiverar snabbare cykler samtidigt hello friktion ingår i lanserar i den befintliga infrastrukturen. Web Apps stöder också hello skapandet av flera testnings- och mellanlagringsmiljöer för arbetsflödet versionen inte längre du behöver tooreserve eller allokera maskinvara för dessa ändamål, du kan skapa så många miljöer som du vill och definiera dina egna befordran toorelease arbetsflöde. Ett företag som du kan bestämma toorelease tooa test fack från källkontroll, utföra en serie tester och befordra tooa steget fack vid slutförande och slutligen växla tooproduction utan avbrott med hello lägga till förmån webbprogram finns på Webbprogram i förväg och hot tooprovide hello bästa möjliga kundupplevelse.  Dessutom kan företag att användning av hello testa i produktion funktionerna i App Service Web Apps toodirect en del av trafiken tooa annan plats, verifiera hello ändringar innan du byter alla trafik toohello ny distribution eller återställa all trafik toohello föregående distribution.

Team lita på att de är i hello bästa möjliga position tooreact tooany problem med någon av sina webbprogram på Web Apps med hello inbyggda funktioner för övervakning och aviseringar. Team bör redan har investerat i analyser och övervakar lösningar sådana från Microsoft Visual Studio Application Insights, New Relic och AppDynamics. Dessa stöds också fullt ut i Web Apps aktiverar affärskontinuitet och välkända miljöer från vilken toomonitor webbaserade program.

Slutligen innehåller Web Apps funktioner tooautomatically säkerhetskopiera appar och värdbaserade databaser direkt tooan Azure Blob Storage-behållare. Ger dig ett enkelt sätt och mycket kostnadseffektiv metod med vilken toorecover katastrofåterställning minskar hello behöver för komplex på lokal maskinvara och programvara.

### <a name="ease-of-migration"></a>Enkel migrering
Maskinvara underhåll och rotation är viktigt för företag som versionen cykler för maskinvara och operativsystem snabbare. Kanske du har ett antal Windows Server 2003 R2-servrar som kommer toohello slutet av stödet i 2015, men de fortfarande är värd för viktiga webbprogram för ditt företag? App Service Web Apps är en bra kandidat på vilka toohost de webbprogram och för du toorationalize hello business maskinvara egendom. Web Apps ger dig åtkomst tooa mängd maskinvaruspecifikationer som hanteras och underhålls som en del av hello tjänst eliminera hello måste toofactor i ersättning och hantering som en del av din infrastruktur budget.  Migreringen kan vara så enkelt som en kopia och klistra in från din befintliga distribution tooWeb appar eller en mer komplex migrering där med hjälp av Web Apps migrering Assistant hello lägger till värdet. Migrerade webbprogram få hello hela skalan av Azure-tjänster, integrera ytterligare tjänster toohello webbprogram. Exempelvis kan du lägga till Azure Active Directory toocontrol åtkomst tooyour program baserat på användarnas association toosecurity grupper. Ett annat exempel kan lägga till Cache Services tooimprove prestanda och minska svarstiden, tillhandahåller övergripande bättre användarupplevelse.

### <a name="enterprise-class-hosting"></a>Värd för Enterprise-klass
App Service Web Apps ger en stabil, tillförlitlig plattform som har varit visat toobe kan toohandle en mängd olika företag måste från små interna utveckling och testning arbetsbelastningar, toohighly skalas hög belastning på webbplatser. Med hjälp av Web Apps gör du använder hello samma enterprise-klass som värd för plattform som används av Microsoft som ett företag för högt värde web arbetsbelastningar. Webbappar, tillsammans med alla tjänster på hello Azure-plattformen är byggda med säkerhet och kompatibilitet med krav, till exempel ISO (ISO/IEC 27001: 2005); SOC1 och SOC2 SSAE 16/ISAE 3402 intyg, HIPAA BAA, PCI och Fedramp på hello mycket hjärtat i varje element och funktionen för mer information finns [http://aka.ms/azurecompliance](/support/trust-center/compliance/).

Microsoft Azure-plattformen gör rollen auktorisering kontroller att aktivera enterprise nivåer av kontroll tooresources i Web Apps. RBAC ger företag hello power tooimplement sina egna principer för hantering av åtkomst för alla sina tillgångar i hello Azure-miljön genom att tilldela användare toogroups och i sin tur tilldelar hello krävs behörigheter toothose grupper mot hello tillgång till exempel en webbplats App. Mer information om RBAC i Azure finns [http://aka.ms/azurerbac](../active-directory/role-based-access-control-configure.md). Du kan vara att ditt webbprogram har distribuerats i en säkrare miljö och du har fullständig behörighet till vilka territorium dina tillgångar distribueras genom att använda Web Apps.

Azure Apptjänstmiljöer [http://aka.ms/aseintro](http://aka.ms/aseintro) är ett nytt alternativ för premium service-plan för enterprise-kunder som toomake användning av Azure App Service och de anger en helt isolerad och dedikerad miljö.  Detta gör att kunder toodeploy företagsprogram som kan dra nytta av storskalighet samtidigt också ha full kontroll över inkommande och utgående nätverkstrafik och ASEs Aktivera program toohave hög hastighet säkra anslutningar över virtuella nätverk tooon lokala resurser.

App Service Web Apps är också kan toomake full användning av dina på lokala-investeringar genom att erbjuda hello möjlighet tooconnect tillbaka tooyour interna resurser, till exempel din data warehouse eller SharePoint-miljön. Enligt beskrivningen i [hög lösning](#high-level-solution) kan du använda Hybridanslutningar och virtuell nätverksanslutning tooestablish anslutningar tooon lokal infrastruktur och tjänster.

### <a name="global-scale"></a>Global skala
App Service Web Apps är en global och skalbar plattform, aktivera din web applications toogrow och anpassa toohello behoven hos ett växande företag snabbt och med minimal långsiktiga planering och kostnader. I vanliga på lokal infrastruktur scenarier expansion och ökad efterfrågan både lokalt och geografiskt skulle kräva en stor del av hantering, planering och utgifter tooprovision och hantera ytterligare infrastruktur. Web Apps erbjuder hello möjlighet tooscale webbprogrammen med hello ebb och flödet av dina krav. Till exempel använder hello utgifter program som exempel, för hello merparten av hello månad användarna är lätta användare av programmet hello men som hello tidsgräns ökar varje månad utgifter bidrag toobe angav och användning för ditt program, Web Apps har hello kapaciteten tooautomatically etablera mer infrastrukturen för ditt program och sedan när hello användning har subsided igen den kan skala tillbaka toohello baslinjen infrastruktur som du definierar.

Web Apps är tillgängligt globalt i 24 Datacenter över hela världen och växande. Hello mest uppdatera listan över regioner och plats, se [http://aka.ms/azlocations](http://aka.ms/azlocations). Med Web Apps kan ditt företag enkelt åstadkomma globalt omfattande och skala. När företaget växer i nya områden, hello reporting instrumentpaneler för programmet som du använder och värden på Web Apps kan enkelt distribueras till ytterligare datacenter och hantera lokala användare mycket snabbare genom hello kombination av Web Apps och Azure Traffic Manager alla med hello läggs fördelen hello skalbar infrastruktur under som kan toocontract och utöka när måste hello hello regionala kontor ändra.

## <a name="solution-details"></a>Information om lösningen
Nu ska vi titta på ett exempel på ett program Migreringsscenario. Detta beskrivs hello information om hur App Service Web Apps funktioner kommer tootogether tooprovide bra lösning och värde för verksamheten.

I det här exemplet är hello driftsapplikationer kommer vi att diskutera kostnad reporting program som gör att anställda toosubmit sina utgifterna för återbetalning. hello program finns i Windows Server 2003 R2 kör IIS6 och hello-databasen är en SQL Server 2005-databas. Hej orsak vi väljer äldre server som ligger med hello kommer slutet av Service för Windows Server 2003 R2 och SQL Server 2005 och vi har [verktyg](http://aka.ms/websitesmigration) och [vägledning](http://aka.ms/websitesmigrationresources) tooautomatically migrera arbetsbelastningar till Azure. Använda tooa allvarlighetsgrad för bred inkompatibilitet över Migreringsscenarier med detta i åtanke hello mönster som används i det här exemplet.

### <a name="migrate-existing-application"></a>Migrera befintliga program
Steg ett hello övergripande lösning för att flytta en av branschspecifika program tooWeb appar är tooidentify hello befintliga program tillgångar och arkitektur. hello exemplet i det här dokumentet är en ASP.NET-webbprogram finns på en IIS-Server med hello-databasen finns på en separat SQL Server enligt hello bilden nedan. Anställda inloggningen toohello system med hjälp av en kombination av användarnamn och lösenord, de ange information om kostnader och ladda upp skannade kopior av inleveranser, till hello-databasen, för varje dataobjekt utgifter.

![](./media/web-sites-enterprise-offerings/on-premise-app-example.png)

#### <a name="items-tooconsider"></a>Objekt tooconsider
När migreringsprogrammet från en lokal miljö, du kanske vill tookeep i något kan några begränsningar för Web Apps. Här följer några viktiga ämnen toobe medveten om när migrera web applications tooWeb appar ([http://aka.ms/websitesmigrationresources](http://aka.ms/websitesmigrationresources)):

* Port bindningar – Web Apps endast har stöd för port 80 för HTTP och port 443 för HTTPS-trafik. Om programmet använder någon annan port, gör sedan en gång migrerade hello program använder port 80 för HTTP och port 443 för HTTPS-trafik. Det här är ofta en oskadliga problem Eftersom det är vanligt i på lokala distributioner toomake användning av olika portar ordning tooovercome hello används för domännamn, särskilt i miljöer för utveckling och testning
* Autentisering – Web Apps stöder anonym autentisering som standard och formulär för autentisering som identifieras av ett program. Web Apps kan erbjuda Windows-autentisering när hello program är integrerat med Azure Active Directory och AD FS endast. Detta är en funktion som beskrivs i detalj [här](http://aka.ms/azurebizapp)
* GAC baserat sammansättningar – Web Apps tillåter inte hello distribution av sammansättningar toohello Global Assembly Cache (GAC). Därför om programmet hello migreras utnyttjar detta funktion lokalt bör du överväga att flytta hello sammansättningar toohello bin-mappen för hello program.
* IIS5-Kompatibelt läge – Web Apps stöder inte IIS5-kompatibelt läge och som sådan hello varje Web Apps-instans och alla webbprogram under hello överordnade Web Apps instansen köras i samma arbetsprocesser i en enda programpool.
* Användning av COM-biblioteken – Web Apps tillåter inte hello registrering av COM-komponenter på hello-plattformen. Om programmet hello gör användning av COM-komponenter kan behöver dessa därför toobe omskrivningar i förvaltad kod och distribueras med hello program.
* ISAPI-filter – ISAPI-filter har stöd för Web Apps. De måste toobe distribueras som en del av programmet hello och registreras i filen web.config för programmet hello-web. Mer information finns i [http://aka.ms/azurewebsitesxdt](web-sites-transform-extend.md).

När dessa avsnitt har bedömts vara, ska ditt webbprogram vara klar för hello molnet. Och oroa dig inte om vissa avsnitt inte uppfylls fullständigt hello Migreringsverktyget ger bästa prestanda toomigration.

hello nästa steg i hello migreringsprocessen är toocreate en App Service webbapp och en Azure SQL Database. Det finns flera olika storlekar på Web Apps instanser med olika antal processorkärnor och RAM belopp för tooselect baserat på din web applications krav. Mer information och prissättning, se [https://azure.microsoft.com/pricing/details/app-service/](https://azure.microsoft.com/pricing/details/app-service/). På samma sätt kan Microsoft Azure SQL Database caters tooall av ett företags behov med olika tjänstnivåer och prestanda nivåer toofulfill krav. Mer information finns på [https://azure.microsoft.com/pricing/details/sql-database/](https://azure.microsoft.com/pricing/details/sql-database/). När du skapat hello program är överförda tooApp Service Web Apps, antingen via FTP- eller WebDeploy och gå sedan vidare till hello-databasen.

I den här migreringen hello använder lösningen Azure SQL Database, men som inte hello databasen som stöds på Azure. Företag kan också göra använder MySQL MongoDB, Azure Cosmos DB och många fler via tillägg som kan köpas på hello [Azure Store](/marketplace/partner-program/).

När du skapar en Azure SQL Database till ett antal alternativ är tillgängliga tooimport en befintlig databas från en lokal server skapar ett skript för en befintlig databas toousing hello [Data-tier Application-Export och Import](http://aka.ms/dacpac) .

hello utgifter databas har skapats genom att skapa en ny Azure SQL Database, ansluter toohello databasen med hjälp av SQL Server Management Studio och sedan köra ett skript toobuild hello databasschemat och fylla det med data från hello lokala databas.

hello kräver sista steget i det första steget av hello migrering hello uppdatering av strängar toohello databas för hello program. Detta kan ske via hello Azure-portalen. Du kan ändra programspecifika inställningar, inklusive eventuella anslutningssträngar som används av hello tooconnect tooany databas som används för varje webbprogram.

### <a name="alternatives-toousing-azure-sql-database"></a>Alternativ toousing Azure SQL Database
hello Azure-plattformen erbjuder ett antal alternativ toousing Azure SQL Database som en primär databas för web-program är detta tooenable olika arbetsbelastningar dvs använder en NoSQL-lösning eller tooenable hello plattform toosuit måste ett företag data. Ett företag kan innehålla data som inte måste lagras utanför kontoret eller i en miljö med offentliga moln och därför skulle se ut toomaintain hello användning av sina lokala-databasen.

#### <a name="connectivity-tooon-premises-resources"></a>Anslutningen tooOn lokala resurser
App Service Web Apps erbjuder flera alternativ för att ansluta tooon lokala resurser, t.ex databaser, aktivera återanvändning av befintliga infrastruktur för högt värde. hello alternativ är enligt nedan:

* Apptjänstmiljöer isolerade och skapas i ett undernät i ett virtuellt nätverk, därför aktivera hello miljö toocommunicate med privata slutpunkter finns i hello samma virtuella nätverk - [http://aka.ms/appserviceasenetworking](http://aka.ms/appserviceasenetworking)
* Web Apps virtuell nätverksintegration stöder integration mellan webbprogram och ett virtuellt Azure-nätverk så att du har åtkomst till tooresources som körs i ditt virtuella nätverk som tillåter anslutning om ansluten tooyour på lokala nätverk med plats-till-plats VPN direkt tooyour på lokala system.
* Hybridanslutningar är en funktion i Azure BizTalk-tjänst och ger ett enkelt sätt tooconnect tooindividual lokala resurser, till exempel SQL Server, MySQL, http-webb-API: er och de flesta anpassade webbtjänster.

#### <a name="scale-and-resiliency"></a>Skalning och återhämtning
När ett företag växer dess personal via förvärv av organisationer eller fysisk organiskt så för webbprogram skala toomeet dessa nya behov. Verkligen idag är vanliga toosee en ännu större spridning av gruppen och fjärranslutna anställda, till exempel företag med kontor i hello USA, Europa och Asien, med en mobil säljstyrkan i många flera områden. Web Apps har hello kapaciteten toohandle elastisk ändringar i skala bekvämt och automatiskt.

App Service Web Apps kan web applications toobe konfigurerats tooscale automatiskt via hello Azure-portalen, beroende på två angreppsmetoderna – schemalagda tider eller genom att CPU-användning. Autoskala för appar som innehåller en kostnadseffektiv och mycket flexibelt sätt toocater för större ändringar i användning för alla program, från webbprogram som vår bekostnad reporting system toomarketing webbplatser som får en hög burst av trafik under en kort period befordran. Mer information och tips om att skala ditt webbprogram med hjälp av Web Apps finns i [hur tooScale webbplatser](web-sites-scale.md).

Dessutom hello toohello skalning flexibilitet för Web Apps övergripande plattform aktiverar affärskontinuitet och återhämtning via hello möjlig distribution av webbprogram och deras tillgångar över flera datacenter och geografiska områden.

## <a name="summary"></a>Sammanfattning
App Service Web Apps erbjuder en flexibel, kostnadseffektiv, effektiv lösning toohello dynamiska behoven hos ett företag i en miljö med utvecklas snabbt. Web Apps hjälper företag att öka produktiviteten och effektiviteten genom att använda en hanterad plattform med moderna DevOps-funktioner och minskade händerna på hantering, samtidigt som man enterprise-funktioner i skala, återhämtning, säkerhet och integrering med lokala tillgångar.

## <a name="call-tooaction"></a>Anropa tooAction
Mer information om hello Azure App Service Web Apps-tjänsten finns [http://aka.ms/enterprisewebsites](/services/websites/enterprise/) där mer information kan hämtas och registrera dig för en utvärderingsversion idag på [https://azure.microsoft.com/ priser/kostnadsfria-utvärderingsversion /](https://azure.microsoft.com/pricing/free-trial/) tooevaluate hello service och identifiera hello fördelar för företaget.

[!INCLUDE [app-service-web-whats-changed](../../includes/app-service-web-whats-changed.md)]

[!INCLUDE [app-service-web-try-app-service](../../includes/app-service-web-try-app-service.md)]
