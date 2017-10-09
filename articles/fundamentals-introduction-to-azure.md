---
title: aaaIntro tooMicrosoft Azure | Microsoft Docs
description: "Nya tooMicrosoft Azure? Få en översikt över hello services det erbjudanden med exempel på hur de är användbara."
services: " "
documentationcenter: .net
author: rboucher
manager: carolz
editor: 
ms.assetid: 6f47f711-2208-4c21-8c1d-826a54c05c29
ms.service: multiple
ms.workload: multiple
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/30/2015
ms.author: robb
ms.openlocfilehash: 6791506abe813e19ca7b04d78d35cb46352a9028
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="introducing-microsoft-azure"></a>Introduktion till Microsoft Azure
Microsoft Azure är Microsofts programplattform för hello offentligt moln.  hello syftet med den här artikeln är toogive du en grund för att förstå hello grunderna i Azure, även om du inte vet ingenting om molntjänster.

**Hur tooread den här artikeln**

Azure växer alla hello tid så att det är enkelt tooget överbelastad.  Börja med grundläggande hello-tjänster, som visas först i den här artikeln och fortsätter sedan tooadditional tjänster. Det betyder inte att inte du kan bara hello ytterligare tjänster ensamt, men hello grundläggande tjänster utgör hello kärnan i ett program som körs i Azure.

**Ge feedback**

Din feedback är viktig. Den här artikeln bör du få en effektiv översikt över Azure. Om det inte berätta under hello kommentarer längst ned hello hello-sidan. Ge viss information om vad du förväntade dig toosee och hur tooimprove hello artikel.  

## <a name="hello-components-of-azure"></a>hello komponenter i Azure
Azure grupperar tjänster i kategorier i hello-hanteringsportalen och på olika visuella hjälpmedel som hello [vad är Azure Infographic](https://azure.microsoft.com/documentation/infographics/azure/) . hello Management Portal är det du använder toomanage mest (men inte alla) services i Azure.

Den här artikeln använder en **annan organisation** tootalk om tjänster baserat på liknande funktion och toocall ut viktiga underordnade tjänster som ingår i större de.  

![Azure-komponenter](./media/fundamentals-introduction-to-azure/AzureComponentsIntroNew780.png)   
 *Bild: Azure tillhandahåller Internet-tillgängliga programtjänster körs i Azure-datacenter.*

## <a name="management-portal"></a>Hanteringsportal
Azure har ett webbgränssnitt som kallas hello [hanteringsportalen](http://manage.windowsazure.com) som gör det möjligt för administratörer tooaccess och administrera de flesta, men inte alla Azure-funktioner.  Microsoft släpper vanligtvis hello senare UI-portal i beta innan den dras tillbaka en tidigare version. hello nyare kallas hello [”Azure Preview Portal”](https://portal.azure.com/).

Det är vanligtvis en lång överlappning när båda portaler är aktiva. Även om kärntjänster visas i båda portalerna kanske inte alla funktioner tillgängliga i båda. Nyare tjänster kan visas i hello senare portal första och äldre tjänster och funktioner kan endast finnas i hello äldre.  här hello-meddelande är om du inte hittar något i äldre hello-portalen kontrollerar hello nyare och vice versa.

## <a name="compute"></a>Compute
Hello mest grundläggande saker har en plattform i molnet är att köra program. Varje hello Azure compute modeller har sin egen roll tooplay.

Du kan använda dessa tekniker separat eller kombinera dem efter behov toocreate hello rätt foundation för ditt program. hello-metod som du väljer beror på vilka problem du försöker toosolve.

### <a name="azure-virtual-machines"></a>Azure Virtual Machines
![Virtuella Azure-datorer ROBBCSIART_TEST](./media/fundamentals-introduction-to-azure/mscsiart_VirtualMachinesIntroNew_12345.png)   
*Bild: Virtuella datorer i Azure ger dig fullständig kontroll över instanser för virtuella datorer i hello molnet.*

hello möjlighet toocreate en virtuell dator på begäran, kan om du anger från en standardiserad avbildning eller från en, vara användbar. Den här metoden, ofta kallade infrastruktur som en tjänst (IaaS), är den Azure Virtual Machines erbjuder. Bild 2 visar en kombination av hur en virtuell dator (VM) körs och hur toocreate ett från en virtuell Hårddisk.  

toocreate en virtuell dator, ange vilken VHD toouse och hello Virtuella datorns storlek.  Du sedan betalar för hello tid att hello virtuella datorn körs. Du betalar med hello minut och endast när den körs, men det finns en minimal lagring kostnad för att hålla hello VHD som är tillgängliga. Azure erbjuder ett galleri med börs virtuella hårddiskar (kallas ”bilder”) som innehåller en startbar operativsystemet toostart från. Dessa inkluderar Microsoft och partner alternativ, till exempel Windows Server och Linux, SQL Server, Oracle och mycket mer. Du har ledigt toocreate virtuella hårddiskar och bilder och överför dem själv. Du kan även överföra virtuella hårddiskar som innehåller endast data och komma åt dem från din virtuella datorer som körs.

Oavsett var hello VHD kommer från, kan du lagra ändringar som görs när en virtuell dator körs beständigt. hello ta nästa gång du skapar en virtuell dator från den virtuella Hårddisken, saker vid där du slutade. hello virtuella hårddiskar som tillbaka hello virtuella datorer lagras i Azure Storage blob som vi pratar om senare.  Det innebär att du får redundans tooensure dina virtuella datorer inte försvinner på grund av toohardware och disk-fel. Det är också möjligt toocopy hello ändras VHD utanför Azure, köra det lokalt.

Programmet körs inom en eller flera virtuella datorer, beroende på hur du har skapat det innan eller bestämma toocreate det från grunden nu.

Den här allmän metod toocloud databehandling kan vara används tooaddress många olika problem.

**Scenarier för virtuell dator**

1. **Utveckling och testning** -du kan använda dem toocreate en billig utveckling och testning plattform som du kan stänga av när du är klar. Du kan också skapa och köra program som använder oavsett språk och bibliotek som du vill. Dessa program kan använda någon av hello data hanteringsalternativ som Azure tillhandahåller och du kan också välja toouse SQLServer eller en annan DBMS som körs i en eller flera virtuella datorer.
2. **Flytta program tooAzure (Lift och SKIFT)** -”Lift och SKIFT” refererar toomoving programmet mycket som du skulle använda en gaffeltruck toomove ett stort objekt.  Du ”lift” Hej VHD från ditt lokala datacenter och ”växlas” den tooAzure och kör det det.  Vanligtvis behöver toodo vissa arbete tooremove beroenden i andra system. Om det finns för många, kan du välja alternativ 3 i stället.  
3. **Utöka ditt Datacenter** -Använd Azure virtuella datorer som ett tillägg till ditt lokala datacenter kör SharePoint eller andra program. toosupport, det är möjligt toocreate Windows-domäner i hello molnet genom att köra Active Directory på virtuella Azure-datorer. Du kan använda Azure Virtual Network (beskrivs senare) tootie ditt lokala nätverk och nätverk i Azure tillsammans.

### <a name="web-apps"></a>Web Apps
![Azure Web Apps ROBBCSIART_TEST](./media/fundamentals-introduction-to-azure/mscsiart_AzureWebsitesIntroNew_12345.png)   
 *Bild: Azure Web Apps kör en webbplatsprogrammet hello molnet utan att behöva toomanage hello underliggande webbservern.*

Hello vanligaste saker som människor gör i hello molnet körs webbplatser och webbprogram. Virtuella Azure-datorer kan detta, men den lämnar du fortfarande med hello ansvar för att administrera en eller flera virtuella datorer och hello underliggande operativsystem. Cloud services web-roller kan göra detta, men distribuera och underhålla dem. fortfarande tar administrativt arbete.  Vad händer om du bara vill ha en webbplats där någon annan tar hand om hello administrativt arbete för dig?

Detta är exakt vad Web Apps erbjuder. Den här modellen för beräkning erbjuder en hanterad webbmiljö med hjälp av hello Azure Management portal samt API: er. Du kan flytta ett befintligt program för webbplatsen i Web Apps oförändrade eller du kan skapa en ny direkt i hello molnet. När en webbplats körs kan du lägga till eller ta bort instanser dynamiskt, förlita dig på Azure Web Apps tooload Utjämna begäranden över dem. Appar i Azure erbjuder både en delad, där webbplatsen körs i en virtuell dator med andra platser, och en standard som gör att en webbplats toorun på egen virtuell dator. hello standard alternativet kan du öka hello storleken (prestanda) för dina instanser om det behövs.

För utveckling stöder Web Apps .NET, PHP, Node.js, Java och Python tillsammans med SQL Database och MySQL (från ClearDB en Microsoft-partner) för relationslagret. Den innehåller också inbyggda stöd för flera populära program, inklusive WordPress, Joomla och Drupal. hello målet är tooprovide en billig, skalbara och fritt och praktiskt plattform för att skapa webbplatser och webbprogram i hello offentligt moln.

**Web Apps scenarier**

Web Apps är avsedda toobe som är användbara för företag, utvecklare och web design myndigheter. För företag är det ett enkelt att hantera, skalbar, mycket säkert och hög tillgänglighet lösning för att köra förekomst webbplatser. När du behöver tooset upp en webbplats, är bästa toostart med Webbappar i Azure och fortsätta tooCloud tjänster när du behöver en funktion som inte är tillgänglig. Se hello slutet av hello ”Compute” avsnitt för mer länkar som kan hjälpa toochoose mellan hello alternativ.

### <a name="cloud-services"></a>Molntjänster
![Azure-molntjänst](./media/fundamentals-introduction-to-azure/CloudServicesIntroNew.png)   
*Bild: Azure Cloud Services tillhandahåller plats toorun skalbara anpassad kod på en plattform som en tjänst (PaaS)-miljö*

Anta att du vill toobuild ett moln-program som har stöd för många samtidiga användare, inte kräver mycket administration och aldrig kraschar. Du kan vara en upprättad programvaruleverantör, till exempel som bestämt tooembrace programvara som en tjänst (SaaS) genom att skapa en version av ett program i hello molnet. Eller kanske är en Startup som skapar en konsument-program som du förväntar dig ökar snabbt. Om du bygger på Azure, vilka körningsmodell bör du använda?

Azure Web Apps gör den här typen av webbprogram, men det finns vissa begränsningar. Du har inte administratörsbehörighet, till exempel vilket innebär att du inte kan installera skadlig programvara. Virtuella datorer i Azure ger dig stor flexibilitet, inklusive administrativ åtkomst och har kan du använda det toobuild ett mycket skalbart program, men du har toohandle många aspekter av tillförlitlighet och administration av dig själv. Vad som är ett alternativ som ger dig hello kontroll du behöver, men hanterar också de flesta av hello arbetet som krävs för tillförlitlighet och administration.

Detta är exakt vad som tillhandahålls av Azure Cloud Services. Den här tekniken är utformad toosupport skalbar, tillförlitlig och låg-admin-program, och det är ett exempel på hur ofta kallas plattform som en tjänst (PaaS). toouse, skapa ett program med hjälp av hello-teknik som du väljer, till exempel C#, Java, PHP, Python, Node.js eller något annat. Koden körs sedan i virtuella datorer (refererad tooas instanser) kör en version av Windows Server.

Men dessa virtuella datorer skiljer sig från hello som du skapar med Azure Virtual Machines. För en sak, Azure själva hanterar dem göra sådant som installerar korrigeringsfiler för operativsystem och automatiskt lansera nya korrigeras bilder. Detta innebär att ditt program inte bör underhåller tillstånd i webb- eller arbetarroll rollinstanser; Det bör i stället hållas i ett hello Azure data management alternativ som beskrivs i nästa avsnitt om hello. Azure övervakar även dessa virtuella datorer starta om alla att misslyckas. Du kan ange cloud services tooautomatically skapa fler eller färre instanser i svaret toodemand. Detta ger dig toohandle ökad användning och skala tillbaka så att du inte betalar så mycket om det finns mindre användning.

Du har två roller toochoose från när du skapar en instans måste båda baseras på Windows Server. hello största skillnaden mellan hello två är att en instans av en webbroll körs IIS, men inte av en instans av en arbetsroll. Både hanteras i hello samma sätt, och den är gemensamma för ett program toouse båda. Till exempel en rollinstans web kan godkänna begäranden från användare och sedan skicka dem tooa worker rollinstansen för bearbetning. tooscale programmet upp eller ned, kan du begära att Azure skapar flera instanser av antingen rollen eller stänga av befintliga instanser. Och liknande tooAzure virtuella datorer du är debiteras endast för hello tid att varje webb eller arbetare roll-instansen körs.

**Cloud Services scenarier**

Molntjänster är perfekt toosupport massiv skala ut när du behöver mer kontroll över hello plattform än Azure Web Apps men behöver inte kontroll över hello underliggande operativsystemet.

#### <a name="choosing-a-compute-model"></a>Om du väljer en beräknings-modell
hello sidan [jämförelse mellan Azure Web Apps, molntjänster och virtuella datorer](app-service-web/choose-web-site-cloud-service-vm.md) ger en mer detaljerad information om hur toochoose en beräknings-modell.

## <a name="data-management"></a>Databearbetning
Program behöver data och olika typer av program behöver olika typer av data. Därför Azure tillhandahåller flera olika sätt toostore och hantera data. Azure tillhandahåller många lagringsalternativ, men alla är utformade för mycket beständig lagring.  Med någon av dessa alternativ finns det alltid 3 kopior av dina data synkroniseras mellan ett Azure-datacenter – 6 om du tillåter Azure toouse geo-redundans tooback in tooanother datacenter minst 300 mil bort.     

### <a name="in-virtual-machines"></a>I virtuella datorer
redan har ovannämnda hello möjlighet toorun SQL Server eller en annan DBMS på en virtuell dator som skapats med virtuella datorer i Azure. Tänk på att det här alternativet inte är begränsad toorelational system. Du kan också ledigt toorun NoSQL teknologier som MongoDB och Cassandra. Kör databassystemet är enkla it replikerar vad vi används tooin våra eget Datacenter- men det krävs också hantering hello administration av den DBMS.  Andra alternativ hanterar Azure flera eller alla hello administration för dig.

Igen, hello tillståndet för hello virtuella datorn och alla ytterligare datadisk du skapar eller överför backas upp av blob-lagring (som vi pratar om senare).  

### <a name="azure-sql-database"></a>Azure SQL Database
![Azure Storage SQL-databas](./media/fundamentals-introduction-to-azure/StorageAzureSQLDatabaseIntroNew.png)   

*Bild: Azure SQL Database är en hanterad relationsdatabas-tjänst i molnet hello.*

Azure tillhandahåller för relationella lagring hello funktionen SQL-databas. Låt inte hello naming lura dig. Detta skiljer sig en typisk SQL-databas som tillhandahålls av SQL Server som körs på Windows Server.  

Kallades SQL Azure, innehåller Azure SQL-databasen alla hello viktiga funktioner i en relationsdatabas management-systemet, inklusive atomiska transaktioner samtidig dataåtkomst av flera användare med dataintegritet, ANSI SQL-frågor och en bekant programmering modell. Öppna tekniker som SQL Server, SQL-databasen kan öppnas med hjälp av Entity Framework ADO.NET, JDBC och andra välkända data. Det stöder också de flesta av hello T-SQL-språket, tillsammans med SQL Server-verktyg som SQL Server Management Studio. Vem som helst bekant med SQL Server (eller en annan relationsdatabas) med hjälp av SQL-databasen är för enkelt.

Men SQL-databas är inte bara ett DBMS i hello molnet it är en PaaS-tjänst. Du fortfarande kontrollera dina data och som har åtkomst till den, men SQL-databas som tar hand om hello administrativa grunt arbete, till exempel hantera hello maskinvara infrastruktur och hålla hello databasen och operativsystemet programvara in toodate automatiskt. SQL-databasen dessutom ger hög tillgänglighet, automatisk säkerhetskopiering i tidpunkt återställa funktioner och kan replikeras geografiska regioner kopior.  

**Scenarier för SQL-databas**

Om du skapar ett Azure-program (med någon av hello beräkning) som behöver relationslagret, kan SQL-databasen vara ett bra alternativ. Program som körs utanför hello molnet kan också använda den här tjänsten, så att det finns tillräckligt med andra scenarier. Data som lagras i SQL-databas kan exempelvis nås från annan klientsystem, inklusive stationära datorer, bärbara datorer, surfplattor och telefoner. Och eftersom den innehåller inbyggd hög tillgänglighet via replikering med hjälp av SQL-databas kan du minimera driftstopp.

### <a name="tables"></a>Tabeller
![Azure Storage-tabeller](./media/fundamentals-introduction-to-azure/StorageTablesIntroNew.png)  

*Bild: Azure-tabeller innehåller en platt NoSQL sätt toostore data.*

Den här funktionen kallas ibland olika villkor som det är en del av en större funktion som kallas ”Azure Storage”. Om du ser ”tabeller”, ”Azure-tabeller” eller ”storage-tabeller”, är alla hello samma sak.  

Och inte förväxlas med hello namn: den här tekniken inte ger relationslagret. Faktum är att det är ett exempel på en NoSQL-metod som kallas för en nyckel/värde-lagring. Azure-tabeller kan ett program som lagrar egenskaper av olika typer, till exempel strängar, heltal och datum. Ett program kan sedan hämta en grupp egenskaper genom att tillhandahålla en unik nyckel för den gruppen. När avancerade åtgärder som kopplingar inte stöds, tabeller ger snabb åtkomst till tootyped data. Det är också mycket skalbar, med en enda tabell kan toohold så mycket som en terabyte data. Och matchar deras enkelhet tabeller är oftast billigare toouse än relationslagret för SQL-databas.

**Scenarier för register**

Anta att du vill toocreate ett Azure-program som kräver snabb åtkomst till tootyped data kan vara mycket av det, men måste inte tooperform SQL-frågor med dessa data. Anta att du skapar en konsument-program som behöver toostore kundinformation profil för varje användare. Appen ska toobe populära, så du behöver tooallow för stora mängder data, men du inte göra mycket med dessa data förutom lagra, hämta den på ett enkelt sätt. Detta är exakt hello slags scenario där klokt att Azure-tabeller.

### <a name="blobs"></a>Blobar
![Azure Storage-Blobbar](./media/fundamentals-introduction-to-azure/StorageBlobsIntroNew.png)    
*Bild: Azure-Blobbar ger Ostrukturerade binära data.*  

Azure BLOB (igen ”Blob Storage” och bara ”Storage-Blobbar” är hello samma sak) är utformad toostore Ostrukturerade binära data. Blobbar ger kostnadseffektiv lagring som tabeller, och en enda blob kan vara så stor som 1TB (en terabyte). Azure-program kan också använda Azure enheter, som gör att BLOB tillhandahålla beständiga lagring för ett Windows-filsystem som är monterat i en Azure-instans. programmet hello ser vanliga Windows-filer, men hello innehållet lagras i en blob.

BLOB-lagring som används av andra Azure-funktioner (inklusive virtuella datorer), så den kan fungera utmärkt hantera dina arbetsbelastningar för.

**Scenarier för Blobbar**

Ett program som lagrar video, massiv filer eller andra binär information kan använda BLOB för enkel och billig lagring. Blobbar används också ofta tillsammans med andra tjänster som Content Delivery Network, som vi ska tala om senare.  

### <a name="import--export"></a>Import/Export
![Azure-importen Export Service](./media/fundamentals-introduction-to-azure/ImportExportIntroNew.png)  

*Bild: Azure Import / Export ger hello möjlighet tooship en fysisk hårddisk tooor från Azure för snabbare och billigare data massimport eller exportera.*  

Ibland vill du toomove stora mängder data i Azure. Som skulle ta lång tid, kanske dagar och använder mycket bandbredd. I dessa fall kan använda du Azure Import/Export, vilket gör att du tooship Bitlocker-krypterad 3,5-tums SATA-hårddiskar direkt tooAzure datacenter där Microsoft överför hello data till blob-lagring för dig.  När hello överföringen är klar levereras Microsoft hello enheter tillbaka tooyou.  Du kan också begära att stora mängder data från Blob Storage exporteras till hårddiskar och skickas tillbaka tooyou via e-post.

**Scenarier för Import / Export**

* **Stora datamigrering** – när du har stora mängder data (terabyte) att du vill tooupload tooAzure, hello Import/Export service är ofta mycket snabbare och kanske billigare än överförs via hello internet. När hello data i BLOB kan bearbeta du det till andra former, till exempel tabellagring eller en SQL-databas.
* **Arkiverade dataåterställning** -du kan använda Import/Export toohave Microsoft överför stora mängder data som lagras i Azure Blob Storage tooa lagringsenhet som du skickar och har enheten levereras tillbaka tooa plats som du önskar. Eftersom detta kan ta lite tid, men det är inte ett bra alternativ för katastrofåterställning. Det är bäst för arkiverade data som du inte behöver snabb åtkomst till.

### <a name="file-service"></a>Tjänsten File
![Tjänsten Azure File](./media/fundamentals-introduction-to-azure/FileServiceIntroNew.png)    
*Bild: Azure Filtjänster tillhandahåller SMB \\ \\server\share sökvägar för program som körs i molnet hello.*

Lokalt, är det vanliga toohave stora mängder lagring av filer som är tillgängligt via hello Server Message Block (SMB) protokollet använder en \\ \\Server\share format. Azure har nu en tjänst som gör att du toouse protokollet i hello molnet. Program som körs i Azure kan använda den tooshare filer mellan virtuella datorer med hjälp av välbekanta filsystem API: er som ReadFile och WriteFile. Dessutom hello filer kan också komma åt på hello samtidigt via ett REST-gränssnitt som gör att du tooaccess hello resurser från lokala när du också konfigurera ett virtuellt nätverk. Azure Files är byggt på hello blob-tjänsten, så den ärver hello samma tillgänglighet, hållbarhet, skalbarhet och geo-redundans är inbyggda i Azure Storage.

**Scenarier för Azure-filer**

* **Migrera befintliga appar toohello moln** -delar dess enklare toohello moln för toomigrate lokalt program som använder tooshare data mellan olika delar av programmet hello. Varje virtuell dator ansluter toohello filresurs och sedan den kan läsa och skriva filer precis som det skulle mot en lokal fil dela.
* **Delade programinställningar** -vanliga mönstret för distribuerade program är toohave configuration-filer på en central plats där de kan nås från många olika virtuella datorer. Dessa konfigurationsfiler kan lagras i en filresurs på Azure och läses av alla instanser av programmet. hello inställningar kan också hanteras via hello REST-gränssnitt, vilket ger globalt åtkomst toohello konfigurationsfiler.
* **Diagnostiska resursen** – du kan spara och dela filer av diagnostiska loggar, mått, och kraschdumpar. Med dessa filer som är tillgängliga via hello SMB- och REST-gränssnittet kan program toouse olika analysverktyg för att bearbeta och analysera hello diagnostikdata.
* **Dev/Test/Debug** – när utvecklare och administratörer arbetar med virtuella datorer i hello molnet ofta måste en uppsättning verktyg. Installera och distribuera dessa verktyg på varje virtuell dator är tidskrävande. En utvecklare eller administratören kan lagra sina favoritverktyg på en filresurs och ansluta toothem från en virtuell dator med Azure-filer.

## <a name="networking"></a>Nätverk
Azure körs idag i många sprids hello world-datacenter. Du kan välja en eller flera av dessa Datacenter toouse när du kör ett program eller lagra data. Du kan också ansluta toothese Datacenter på olika sätt med hello services nedan.

### <a name="virtual-network"></a>Virtual Network
![VirtualNetwork](./media/fundamentals-introduction-to-azure/VirtualNetworkIntroNew.png)   

*Bild: Virtuella nätverk tillhandahåller ett privat nätverk i molnet hello så olika tjänster kan kommunicera tooeach andra eller tooon lokala resurser om du konfigurerar en VPN-anslutning mellan platser.*  

Ett bra sätt toouse ett offentligt moln är tootreat den som ett tillägg för ditt eget datacenter.

Eftersom du kan skapa virtuella datorer på begäran, sedan ta bort dem (och stoppa betalar) när de inte längre behövs, du kan ha datorkraft bara när du vill. Och eftersom Azure virtuella datorer kan du skapa virtuella datorer som kör SharePoint, Active Directory och andra välkända lokal programvara, den här metoden kan arbeta med hello-program som du redan har.

toomake den här användbart, men användarna bör toobe kan tootreat programmen som om de kördes i ditt eget datacenter. Detta är exakt vad Azure Virtual Network. Med en VPN-gateway-enhet kan kan en administratör konfigurera ett virtuellt privat nätverk (VPN) mellan ditt lokala nätverk och dina virtuella datorer som är distribuerade tooa virtuella nätverk i Azure. Eftersom du tilldelar egna IP v4-adresser toohello moln virtuella datorer, visas de toobe i ditt eget nätverk. Användare i din organisation kan komma åt hello program dessa virtuella datorer innehåller som om de kördes lokalt.

Mer information om planering och skapa ett virtuellt nätverk som passar dig finns [virtuellt nätverk](virtual-network/virtual-networks-overview.md).

### <a name="express-route"></a>Express Route
![ExpressRoute](./media/fundamentals-introduction-to-azure/ExpressRouteIntroNew.png)   

*Bild: ExpressRoute använder ett virtuellt Azure-nätverk, men vägar anslutningar via snabbare dedikerad raderna i stället för hello offentliga Internet.*  

Om du behöver mer bandbredd eller säkerhet än ett Azure Virtual Network anslutning kan ge, kan du söka i ExpressRoute. I vissa fall kan ExpressRoute också spara pengar. Du behöver fortfarande ett virtuellt nätverk i Azure, men hello länken mellan Azure och din plats använder en dedikerad anslutning som inte går via hello offentliga Internet. I ordning toouse den här tjänsten måste du toohave ett avtal med en nätverkstjänstleverantören eller en exchange-provider.

Konfigurera en ExpressRoute-anslutning kräver mer tid och planering, så du kanske vill toostart med en plats-till-plats-VPN och sedan migrera tooan ExpressRoute-anslutning.

Mer information om ExpressRoute finns [teknisk översikt för ExpressRoute](expressroute/expressroute-introduction.md).

### <a name="traffic-manager"></a>Traffic Manager
![TrafficManager](./media/fundamentals-introduction-to-azure/TrafficManagerIntroNew.png)   

*Bild: Azure Traffic Manager kan du tooroute globala trafik tooyour tjänsten baserat på intelligent regler.*

Om din Azure-program körs i flera datacenter, kan du använda Azure Traffic Manager tooroute begäranden från användare som använder det Intelligent mellan instanser av programmet hello. Du kan också dirigera trafik tooservices inte körs i Azure så länge de är tillgängliga från hello internet.  

Ett Azure-program med användare i en enda del av hello world kan köras i endast en Azure-datacenter. Ett program med användare spridda runtom hello world, men är mer troligt toorun i flera Datacenter kanske även alla. Står inför ett problem i den här andra situationen: hur du använder det Intelligent dirigera användare tooapplication instanser? De flesta hello tid vill du förmodligen varje användare tooaccess hello datacenter närmaste tooher, eftersom sannolikt får sitt hello bästa svarstid. Men vad händer om instansen av programmet hello är överbelastad eller inte är tillgängligt? I så fall skulle vara bra toodirect sin begäran automatiskt tooanother datacenter. Detta är exakt vad görs av Azure Traffic Manager.

hello ägaren av ett program definierar regler som anger hur begäranden från användare som ska vara riktad toodatacenters sedan förlitar sig på Traffic Manager toocarry ut dessa regler. Användare kanske normalt dirigerad toohello närmaste Azure-datacentret men skickas tooanother en när hello svarstid från sina datacenter standard överskrider hello svarstid från andra datacenter. Globalt distribuerade program med många användare är har en inbyggd tjänst toohandle problem som dessa användbart.

Traffic manager använder Directory Name Service (DNS) tooroute användare tooservice slutpunkter, men ytterligare trafik går inte via Traffic Manager när anslutningen görs. Detta håller Traffic Manager från att vara en flaskhals kan sakta ned service-kommunikation.

## <a name="developer-services"></a>Utvecklartjänster
Azure erbjuder ett antal verktyg toohelp utvecklare och IT-proffs skapa och hantera program i hello molnet.  

### <a name="azure-sdk"></a>Azure SDK
Tillbaka stöds 2008 hello första förhandsversionen av Azure i .NET-utveckling. Idag, men kan du skapa Azure-program i nästan alla språk. Microsoft tillhandahåller för närvarande språkspecifika SDK för .NET, Java, PHP, Node.js, Ruby och Python. Det finns också en allmän Azure SDK som tillhandahåller grundläggande stöd för alla språk, till exempel C++.  

Dessa SDK hjälpa dig att skapa, distribuera och hantera Azure-program. De är tillgängliga antingen från [www.microsoftazure.com](https://azure.microsoft.com/downloads/) eller GitHub och de kan användas med Visual Studio och Eclipse. Azure erbjuder också kommandoradsverktyg som utvecklare kan använda med alla redigerare eller utvecklingsmiljö, inklusive verktyg för att distribuera program tooAzure från Linux- och Macintosh-system.

Tillsammans med hjälper dig att skapa Azure-program, dessa SDK: er ger också klientbibliotek som hjälper dig att skapa program som använder Azure-tjänster. Du kan till exempel skapa ett program som läser och skriver Azure-blobbar eller skapa ett verktyg som distribuerar Azure program via hello Azure hanteringsgränssnitt.

### <a name="visual-studio-team-services"></a>Visual Studio Team Services
Visual Studio Team Services är ett marknadsföring namn som omfattar ett antal tjänster som hjälper toodevelop program i hello Azure.

tooavoid förvirring - det inte finns en värdbaserad eller webbaserade version av Visual Studio. Du måste fortfarande din lokala körande kopia av Visual Studio. Men den innehåller många andra verktyg som kan vara användbar.

Det omfattar en värdbaserad källkontrollsystem kallas Team Foundation-tjänst som erbjuder versionskontroll och arbete objektet spårning.  Du kan även använda Git för versionskontroll om du föredrar som. Och du kan variera hello källkontrollsystem som du använder av projektet. Du kan skapa obegränsade privata grupprojekt nås från var som helst i hälsningsmeddelande.  

Visual Studio Team Services innehåller en belastningen tester. Du kan köra belastningstester som skapats i Visual Studio på virtuella datorer i hello molnet. Du anger hello totala antalet användare som du vill testa tooload med och Visual Studio Team Services automatiskt avgör hur många agenter som krävs, få igång hello som krävs för virtuella datorer och köra din belastningstester. Om du är en MSDN-prenumerant kan hämta du tusentals ledigt användarminuter belastningen Testa varje månad.

Visual Studio Team Services har också stöd för flexibel utveckling med funktioner som kontinuerlig integration bygger, Kanban: er och virtuella team lokaler.

**Visual Studio Team Services scenarier**

Visual Studio Team Services är ett bra alternativ för företag som behöver toocollaborate över hela världen och inte redan har hello-infrastruktur i plats toodo så. Du kan hämta installationen i minuter, välja ett system för källa och börja skriva kod och skapa den dagen.  hello team verktyg är en plats för samordning och samarbete och hello ytterligare verktyg ger hello analys behövs tootest och finjustera tillämpningsprogrammet snabbt.

Men organisationer som redan har ett lokalt system kan testa nya projekt i Visual Studio Team Services toosee om det är mer effektivt.   

### <a name="application-insights"></a>Application Insights
![Application Insights](./media/fundamentals-introduction-to-azure/ApplicationInsights.png)  

*Bild: Application Insights övervakar prestanda och användning av appen live webb- eller enhet.*

När du har publicerat din app – om den körs på mobila enheter, datorer eller webbläsare - anger Application Insights hur den fungerar och vad användarna gör med den. Kommer att antalet krascher och lång svarstid, varning om hello bilder mellan oacceptabel tröskelvärden och hjälpa dig att diagnostisera problemen.

När du utvecklar en ny funktion kan du planera toomeasure dess lyckas med användare. Genom att analysera användningsmönster, förstå vad som fungerar bäst för dina kunder och förbättra din app i varje utvecklingscykeln.

Även om den finns i Azure, fungerar Application Insights för ett växande antal appar, både på och från Azure. J2EE- och ASP.NET web apps omfattas, samt iOS, Android, OS x och Windows-program. Telemetri skickas från en SDK som skapats med hello app toobe analyseras och visas i hello Application Insights-tjänsten i Azure.

Om du vill ha mer specialiserad analytics exportera hello telemetri dataströmmen tooa databasen eller tooPower BI eller andra verktyg.

**Application Insights-scenarier**

Du utvecklar en app. Det kan vara ett webbprogram eller en enhetsapp eller en enhetsapp med en webb-serverdel.

* Justera hello prestandan för din app när den har publicerats, eller när den är i belastningstester.  Application Insights aggregerar telemetri från alla hello installerade instanser och ger dig diagram över svarstider, begäran och antalet undantag, beroende svarstider och andra nyckeltal. Dessa hjälpa dig att justera prestandan för din app. Du kan infoga kod tooreport mer specifika data om det behövs.
* Identifiera och diagnostisera problem i din aktiva app. Du kan få aviseringar via e-post om nyckeltal mellan godkända tröskelvärden. Du kan undersöka specifika användarsessioner, till exempel toosee hello begäran som orsakade ett undantag.
* Spåra användning tooassess hello framgången för varje ny funktion. När du skapar en ny artikel användare kan planera toomeasure hur mycket den används, och om användarna få sina målen. Application Insights ger dig grundläggande användningsdata, till exempel Webbsidevyer och du kan infoga kod tootrack hello användarupplevelsen i detalj.

### <a name="automation"></a>Automation
Ingen gillar toowaste tid göra hello samma manuell bearbetar flera gånger. Azure Automation är ett sätt för toocreate, övervaka, hantera och distribuera resurser i Azure-miljön.  

Automation använder ”runbooks”, som använder Windows PowerShell-arbetsflöden (jämfört med vanliga PowerShell) under hello omfattar. Runbooks är avsedda toobe köras utan användaråtgärder. PowerShell-arbetsflöden kan hello tillståndet för ett skript toobe sparade kontrollpunkter längs hello sätt. Om ett fel inträffar, har du ingen toostart ett skript från hello början. Du kan starta om den senaste kontrollpunkten för hello. Detta sparar mycket arbete försök toomake hello skript referensen var har misslyckats.

**Automatiseringsscenarier**

Azure Automation är en bra tooautomate hello manuell, långvariga, felbenägna, och ofta återkommande uppgifter i Azure.

### <a name="api-management"></a>API Management
Skapa och publicera programmerare programgränssnitt (API) på hello är internet ett vanligt sätt tooprovide services tooapplications. Om dessa tjänster är resellable (till exempel väder data) kan en organisation kan tillåta någon tredje parts tooaccess samma services för en avgift. När du skalar toomore partners du vanligtvis behöver toooptimize och åtkomstkontroll.  Vissa partner kan även ha hello data i ett annat format.

Azure API Management gör det lättare för organisationer toopublish API: er toopartners, anställda och tredjeparts-utvecklare på ett säkert sätt och i större skala. Det ger en annan API-slutpunkt och fungerar som en proxy-toocall hello faktisk slutpunkt när tillhandahåller tjänster till exempel cachelagring, transformation, begränsning, komma åt kontroll och analytics sammanställning.

**Scenarier för API-hantering**

Anta att ditt företag har en uppsättning enheter som alla måste toocall tillbaka tooa centralt tooget data – till exempel ett leverans företag som har enheter i varje lastbil på hello väg.  Verkligen vill hello företag tooset upp en system-tootrack är det egna lastbilar så den kan förutsäga och uppdatera leveranstider på ett tillförlitligt sätt. Den kan vet hur många lastbilar har och planera på lämpligt sätt.  Varje lastbil måste en enhet som anropar tillbaka tooa central plats med dess placering och hastighet data och kanske.

En kund av hello leverans företagets skulle förmodligen också dra nytta av komma informationen placering.  hello-kund kan använda den tooknow hur långt produkter har tootravel, där de fastna, hur mycket de betalar längs vissa vägar (om det kombineras med vad de betalt tooship). Om hello leverans företagets sammanställer informationen redan, kan många kunder betalar för den.  Men sedan hello leverans företaget måste tooprovide sätt toogive kunder hello data. När de ger åtkomst toocustomers kanske de inte har kontroll över hur ofta hello data efterfrågas. De måste tooprovide regler om vem som kan komma åt vilka data. Alla dessa regler skulle ha toobe som är inbyggda i sina externt API. Detta är där API-hantering kan hjälpa.  

## <a name="identity-and-access"></a>Identitet och åtkomst
Arbeta med identity ingår i de flesta program. Att veta vem en användare är kan ett program bestämma hur den ska interagera med användaren. Azure tillhandahåller tjänster toohelp spåra samt integrera den med identitetslagringar kanske du redan använder.

### <a name="active-directory"></a>Active Directory
Precis som de flesta katalogtjänster lagrar Azure Active Directory information om användare och hello organisationer som de tillhör. Användarna kan logga in, därefter tillhandahåller dem med token presentera tooapplications tooprove sin identitet. Här kan också synkronisera användarinformation med Windows Server Active Directory körs lokalt i det lokala nätverket. Hello metoder och dataformat som används av Azure Active Directory inte är identiska med de som används i Windows Server Active Directory, är hello-funktioner som den utför ganska lika.

Det är viktigt toounderstand att Azure Active Directory har utformats för användning av molnprogram. Den kan användas av program som körs på Azure, till exempel eller för andra molnplattformar. Det används också av Microsofts egna molnprogram, till exempel de som finns i Office 365. Om du vill tooextend ditt datacenter i hello moln med hjälp av Azure virtuella datorer och Azure Virtual Network, men Azure Active Directory är inte hello rätt val. I stället ska du toorun Windows Server Active Directory på virtuella datorer.

toolet program åtkomst hello informationen, Azure Active Directory innehåller en RESTful-API kallas Azure Active Directory Graph. Detta API kan program som körs på någon plattform katalogobjekt för åtkomst och hello förhållanden mellan dem..  Ett auktoriserade program kan till exempel använda den här API-toolearn om en användare, hello grupper han tillhör och annan information. Program kan också visa relationer mellan användare deras sociala graph-låta de fungerar bättre med hello anslutningar mellan personer.

En funktion i den här tjänsten Azure Active Directory-åtkomstkontroll, gör det enklare för ett program tooaccept identitetsinformation från Facebook, Google, Windows Live ID och andra populära identitetsleverantörer. I stället för att kräva olika dataformat för hello program för toounderstand hello och protokoll som används av var och en av dessa providers översätter åtkomstkontroll dem till ett enda vanliga format. Du kan också ett program som accepterar inloggningar från en eller flera Active Directory-domäner. En leverantör som tillhandahåller ett SaaS-program kan till exempel använda Azure Active Directory-åtkomstkontroll toogive användare i var och en av dess kunder enkel inloggning toohello program.

Katalogtjänster är en core gäller lokala datorer. Det får inte vara konstigt att de är också viktigt i hello molnet.

### <a name="multi-factor-authentication"></a>Multi-Factor Authentication
![Azure Multi-Factor Authentication](./media/fundamentals-introduction-to-azure/MFAIntroNew.png)   

*Bild: Multi-Factor Authentication ger hello funktioner för ditt program tooverify mer än en identifieringsmetod*

Säkerhet är alltid viktigt. Multifaktorautentisering (MFA) hjälper till att säkerställa att användarna själva åtkomst till sina konton. MFA (även kallat tvåfaktorsautentisering eller ”2FA”) kräver att användare anger två av dessa tre metoder för att verifiera identiteten för användarinloggningar och transaktioner.

* Något du vet (normalt ett lösenord)
* Något du har (betrodda enheter som inte enkelt dubbleras, t.ex. en telefon)
* Något du är (biometrik)

Så när en användare loggar in, du behöver dem tooalso verifiera sin identitet med en mobilapp, ett telefonsamtal eller ett textmeddelande i kombination med sitt lösenord. Som standard stöder Azure Active Directory hello lösenord ska användas som enda verifieringsmetod för användarinloggningar. Du kan använda MFA tillsammans med Azure AD eller med anpassade program och kataloger med hjälp av hello MFA SDK. Du kan också använda den tillsammans med lokala program med hjälp av Multi-Factor Authentication-servern.

**MFA-scenarier**

Logga in skydd på känsliga konton, till exempel bank inloggningar och koden åtkomst till datakällan där obehörig transaktion kan ha en hög finansiella eller immateriella egenskap kostnad.   

## <a name="mobile"></a>Mobil
Om du skapar en app för en mobil enhet kan det hjälpa Azure lagra data i molnet hello autentisera användare och skicka push-meddelanden utan att behöva toowrite mycket anpassad kod.

Medan du kan visserligen skapa hello serverdelen för en mobil app med molntjänster, virtuella datorer eller Web Apps måste tillbringar du mycket mindre tid skrivning hello underliggande tjänstkomponenter med Azures tjänster.

### <a name="mobile-apps"></a>Mobile Apps
![Mobile Apps](./media/fundamentals-introduction-to-azure/MobileServicesIntroNew.png)

*Bild: Mobilappar ger funktioner som krävs vanligtvis av program som har gränssnitt med mobila enheter.*

Azure Mobile Apps innehåller många användbara funktioner som kan du spara tid när du skapar en serverdel för mobila program. Det gör du toodo enkel etablering och hantering av data som lagras i en SQL-databas. Du kan enkelt använda ytterligare data lagringsalternativ som blob-lagring eller MongoDB med serverkod. Mobilappar har stöd för meddelanden, men i vissa fall du i stället använda Notification Hubs som beskrivs nedan.  hello-tjänsten har också en REST-API kan mobila programmet anropa tooget arbete. Mobilappar ger också möjlighet hello tooauthenticate användare via Microsoft och Active Directory och andra välkända identitetsleverantörer som Facebook, Twitter och Google.   

Du kan använda andra Azure-tjänster som Service Bus- och arbetsroller och ansluta tooon lokala system. Du kan även använda 3 part tillägg från hello Azure Store (till exempel SendGrid för e-post) tooprovide ytterligare funktioner.

Intern klientbibliotek för Android, iOS, HTML/JavaScript, Windows Phone och Windows Store gör det enklare toodevelop för appar på alla större mobila plattformar. En REST-API kan toouse Mobile Services data och funktioner för autentisering med appar på olika plattformar. En enda Mobiltjänst kan säkerhetskopiera flera klientappar så att du får en konsekvent användarupplevelse över enheter.

Eftersom Azure stöder omfattande utskalning redan, kan du hantera hello trafik när appen blir mer populära.  Övervakning och loggning stöds toohelp felsöka problem och hantera prestanda.

### <a name="notification-hubs"></a>Notification Hubs
![NotificationHubs](./media/fundamentals-introduction-to-azure/NotificationHubsIntroNew.png)  

*Bild: Notification Hubs ger funktioner som krävs vanligtvis av program som har gränssnitt med mobila enheter.*

Du kan skriva kod toodo meddelanden i Azure Mobile Apps, är Meddelandehubbar optimerad toobroadcast miljoner av personanpassat push-meddelanden i minuter.  Du har inte tooworry om detaljer som mobiloperatör eller enhetstillverkaren. Du kan rikta person eller miljontals användare med ett enda API-anrop.

Notification Hubs är utformad toowork med alla serverdelar. Du kan använda Azure Mobile Apps en anpassad serverdel i hello molnet körs på valfri provider eller en lokal serverdel.

**Notification Hub scenarier** om du skriver ett mobila spel där spelare tog aktiverar kan du behöva toonotify spelare 2 som player 1 slutfört sin tur. Om det är allt du behöver toodo kan du bara använda Mobile Apps. Men om du har 100 000 användare spelar spel och du vill toosend känsliga kostnadsfria erbjudandet tooeveryone, Meddelandehubbar är hello bättre val.

Du kan skicka de senaste nyheterna, sporting händelser och produkten meddelande meddelanden toomillions av användare med låg latens. Företag kan meddela sina anställda om ny tid känsliga kommunikationer, till exempel säljleads, så att anställda inte behöver tooconstantly kontrollera e-post eller andra program toostay informeras. Du kan också skicka en-gång-lösenord som krävs för multi-Factor authentication.

## <a name="back-up"></a>Säkerhetskopiera
Alla företag behöver toobackup och återställning av data. Du kan använda Azure toobackup och Återställ ditt program i hello molnet eller lokalt. Azure erbjuder toohelp olika alternativ beroende på hello typ av säkerhetskopiering.

### <a name="site-recovery"></a>Webbplatsåterställning
Med hjälp av Azure Site Recovery (tidigare Hyper-V Recovery Manager) kan du skydda viktiga program genom att samordna replikering hello och återställning mellan platser. Site Recovery tillhandahåller kapaciteten tooprotect program baserat på Hyper-v, VMWare eller SAN tooyour egna sekundär plats, tooa värdens plats eller tooAzure och undvika hello kostnaden och komplexiteten med att skapa och hantera egna sekundär plats. Azure krypteras data och kommunikationen och att du har hello alternativet Aktivera kryptering för data i vila för.

Det övervakar hello hälsotillståndet för dina tjänster kontinuerligt och hjälper till att automatisera hello rätt återställning av tjänster i en plats avbrott på hello primära datacenter hello-händelse. Virtuella datorer kan tas i en framförhållning sätt toohelp återställning tjänst snabbt, även för komplexa arbetsbelastningar i flera nivåer.

Site Recovery fungerar med befintlig teknik som Hyper-V-replikering, System Center och SQL Server alltid aktiverad. Checka ut [översikt över Azure Site Recovery](site-recovery/site-recovery-overview.md) för mer information.

### <a name="azure-backup"></a>Azure Backup
![Azure Backup](./media/fundamentals-introduction-to-azure/AzureBackupIntroNew.png)  

*Bild: Azure-säkerhetskopiering säkerhetskopierar data från lokala Windows-servrar i hello moln.*  

Azure-säkerhetskopiering säkerhetskopierar data från lokala servrar som kör Windows Server till hello molnet. Du kan hantera dina säkerhetskopieringar direkt från hello säkerhetskopieringsverktyg i Windows Server 2012, Windows Server 2012 Essentials eller System Center 2012 – Data Protection Manager. Du kan också använda en särskild backup-agenten.

Data är säkrare eftersom krypteras säkerhetskopior före överföringen och lagras krypterade i Azure och skyddas av ett certifikat som du överför. hello används hello samma redundant och hög tillgänglighet dataskydd finns i Azure Storage.  Du kan säkerhetskopiera filer och mappar enligt ett schema eller direkt, med fullständig eller inkrementell säkerhetskopiering. När data säkerhetskopieras toohello molnet, kan behöriga användare enkelt återställa säkerhetskopior tooany server. Den erbjuder konfigurerbara datakvarhållningsprinciper, komprimering och data transfer begränsning så att du kan hantera hello kostnaden toostore och överföra data.

**Scenarier för Azure-säkerhetskopiering**

Om du redan använder Windows Server eller System Center Azure backup är en fysisk lösning för att säkerhetskopiera dina servrar filsystem, virtuella datorer och SQL Server-databaser.  Den fungerar med krypterade, sparse och komprimerade filer. Det finns vissa begränsningar, så du bör [Kontrollera hello Azure Backup förutsättningar](http://technet.microsoft.com/library/dn296608.aspx) första.

## <a name="messaging-and-integration"></a>Meddelandefunktioner och integration
Oavsett vad det gör, måste kod ofta toointeract med annan kod.  I vissa fall är allt som behövs grundläggande köade meddelanden. I annat fall krävs mer komplexa interaktioner. Azure tillhandahåller ett par olika sätt toosolve dessa problem. Bild 5 visar hello val.

### <a name="queues"></a>Köer
![Azure Service Bus Relay](./media/fundamentals-introduction-to-azure/QueuesIntroNew.png)

*Bild: Köer kan förlora koppling mellan delar av ett program och underlätta skalning.*  

Queuing är en enkel idé: ett program visas ett meddelande i en kö och meddelandet så småningom läsa av ett annat program. Om ditt program behöver bara den här enkla tjänsten, vara köer i Azure hello bästa valet.

Ange liknande queuing tjänster på grund av hello sätt hello Azure vuxen över tid, Azure Storage-köer och Service Bus-köer. hello skäl varför du vill ha en toouse över hello andra beskrivs i hello ganska tekniska dokumentet [Azure köer och Service Bus-köer - skillnad från och med](http://msdn.microsoft.com/library/azure/hh767287.aspx).  I många fall är antingen kommer att fungera.

**Scenarier för kön**

Ett vanligt användningsområde för köer är idag toolet en rollinstans web kommunicera med en worker rollinstans inom hello samma Cloud Services-program.

Anta att du skapar ett Azure-program för delning av video. hello program består av PHP-kod som körs i en webbroll som låter användarna ladda upp och titta på videor, tillsammans med en arbetsroll som implementerats i C# som omvandlar upp video i olika format.

När en web rollinstans hämtar en ny video från en användare, den kan lagra hello video i en blob och sedan skicka ett meddelande tooa arbetsrollen via en kö om den där det här nya toofind video. En worker-rollen instans-den inte någon roll som en kommer sedan läsa hello-meddelande från kön hello och utföra hello krävs video översättningar i hello bakgrund.

Ett program i det här sättet att strukturera kan asynkron bearbetning och gör det också hello program enklare tooscale, eftersom hello antalet rollinstanser för webb och worker rollinstanser varieras oberoende av varandra. Du kan också använda hello köstorlek som en utlösare tooscale hello antal arbetsroller uppåt och nedåt. För hög och du lägga till fler roller. När det hämtar lägre, kan du minska hello antal kör roller toosave pengar.  

Du kan använda den här samma mönster mellan många olika delar av programmet även om de inte använder webb-och arbetsroller.  Det gör att du tooscale hello delar på endera sidan av hello kön uppåt och nedåt som begäran och bearbetningstid kräver.

### <a name="service-bus"></a>Service Bus
Om de körs i molnet hello i ditt datacenter på en mobil enhet eller någon annanstans, måste toointeract program. hello målet med Azure Service Bus är toolet program som körs nästan var som helst utbyta data.

I tillägg toohello köer (1) ovan, innehåller Service Bus också tooother kommunikationsmetoder.

#### <a name="service-bus-relay"></a>Service Bus Relay
![Azure Service Bus Relay](./media/fundamentals-introduction-to-azure/ServiceBusRelayIntroNew.png)

*Bild: Service Bus Relay tillåter kommunikation mellan program på olika sidor av en brandvägg.*

Service Bus kan direkt kommunikation via dess vidarebefordringstjänst, vilket ger ett säkert sätt toointeract genom brandväggar. Service Bus reläer Aktivera program toocommunicate genom att utbyta meddelanden via en slutpunkt som finns i molnet hello i stället för lokalt.

**Service Bus Relay-scenarier**

Program som kommunicerar via Service Bus kan vara Azure-program eller program som körs på vissa andra molnplattform. De kan även vara program som körs utanför hello moln, men. Se exempelvis ett flygbolag som implementerar reservation tjänster på datorer i ett eget datacenter. hello flygbolag måste tooexpose dessa tjänster toomany klienter, inklusive incheckningskiosker på flygplatser, reservation agent terminaler och kanske även kunder telefoner. Kan den använda Service Bus toodo detta skapar löst kopplade samverkan mellan hello olika program.

#### <a name="service-bus-topics-and-subscriptions"></a>Service Bus-ämnen och prenumerationer
![Azure Service Bus-ämnen](./media/fundamentals-introduction-to-azure/ServiceBusTopicsSubsIntroNew.png)   
 *Bild: Service Bus-ämnen kan flera appar toopost meddelanden och andra program toosubscribe tooreceive meddelanden som uppfyller ett visst villkor.*

Service Bus innehåller en publicera och prenumerera-funktion som kallas ämnen och prenumerationer. Med publish-subscribe, kan ett program skicka meddelanden tooa avsnittet, medan andra program kan skapa prenumerationer toothis avsnittet. Detta kan en-till-många-kommunikationen mellan en uppsättning program, att låta hello samma meddelande att läsa av flera mottagare.

**Service Bus-ämnen och prenumerationer scenarier**

När du ställer in där det finns många alla viktiga meddelanden, men olika underordnade system behöver bara toolisten toodiffering delar av kommunikationen, är ett bra alternativ för Service Bus-ämne och prenumerationer.

### <a name="biztalk-services"></a>BizTalk Services
![BizTalk-tjänst](./media/fundamentals-introduction-to-azure/BizTalkServicesIntroNew.png)   
 *Bild: BizTalk-tjänst ger hello möjlighet tootransform XML-meddelanden format i hello molnet.*

Ibland måste du ansluta datorer som kommunicerar med olika meddelanden format. Det är vanligt för business toohave annan databas scheman och XML-meddelanden format, även om en gemensam standard är tillgänglig. I stället för att skriva egen kod mycket kan du använda BizTalk Server lokalt toointegrate olika system.  BizTalk-tjänst som Azure tillhandahåller hello samma typ av tjänst, men i hello molnet. Du kan betalar bara vad du använder och oroa dig inte om skala som du skulle ha tooon lokalt.

**BizTalk Services-scenarier**

Interaktioner Business-to-Business (B2B) kräver ofta den här typen av översättning.  Till exempel är ett företag och skapa flygplan behov tooorder delar från den olika delar leverantörer. Har många delar leverantörer.  Dessa order ska vara automatisk toogo direkt från hello flygplan builders system till hello leverantörer system.  Varken företag vill toochange core system och meddelandeformat och det är troligt att dessa format är hello samma. BizTalk-tjänst kan ta meddelanden och översätta mellan hello nya format båda hållen. Antingen hello flygplan leverantören kan göra hello arbete tootranslate eller hello olika leverantörer kan, beroende på som vill ha mer kontroll och hello mängden översättning som behövs.     

## <a name="compute-assistance"></a>Compute-hjälp
Azure tillhandahåller stöd för tjänster som inte behöver toorun alla hello tid.  

### <a name="scheduler"></a>Scheduler
![Azure Scheduler](./media/fundamentals-introduction-to-azure/SchedulerIntroNew.png)   
*Bild: Azure Scheduler ger ett sätt tooschedule jobb vid en viss tid för en viss tid.*

Program behöver ibland bara toorun vid en viss tidpunkt. Du kan spara pengar med den här typen av app i stället för att låta ett program bara hålla körs 24 x 7 väntar på att data tooprocess på Azure. Azure Schemaläggaren kan tooschedule när ett program ska köras baserat på intervall eller en kalender. Det är tillförlitlig och verifierar att en process körs även om det förekommer fel för nätverk, datorer och data center. Du använder hello Scheduler REST API toomanage åtgärderna.

När en schemalagd larm Scheduler skickar HTTP eller HTTPS meddelanden tooa specifika endpoint eller placera ett meddelande i en kö för lagring.  Så du måste toohave programmet har en tillgänglig slutpunkt eller det övervaka kön för lagring Den kan sedan utföra de åtgärder som den är programmerad att när det hämtar hello-meddelande.

**Scenarier för Schemaläggaren**

* Återkommande programmet åtgärder: exempelvis kan en tjänst med jämna mellanrum hämta data från twitter och samla in hello data i en vanlig feed.
* Dagligt underhåll: loggen bearbetning eller rensas, säkerhetskopiera och andra periodvis schemalägga aktiviteter.
* Aktiviteter som körs på natten.
* Web applications uppgifter som dagliga rensning av loggar, göra säkerhetskopior och andra underhållsaktiviteter. En administratör kan välja toobackup sin databas kl 1 varje dag för hello nästa nio månader, till exempel.

hello Scheduler API kan du toocreate, uppdatera, ta bort, visa och hantera jobbsamlingar och schemalagda jobb via programmering.

## <a name="performance"></a>Prestanda
Prestanda är alltid viktigt för ett program. Program brukar tooaccess hello samma information flera gånger. Enkelriktade tooimprove prestanda är tookeep en kopia av data närmare toohello programmet, vilket minimerar hello tid som krävs för tooretrieve den. Azure tillhandahåller olika tjänster för att göra detta.

### <a name="azure-caching"></a>Azures cachelagring
![Azures cachelagring](./media/fundamentals-introduction-to-azure/AzureCacheIntroNew.png)   
 **Bild: Ett Azure-program kan cachelagra data i minnet och även dela den på många arbetsroller**

Åtkomst till data som lagras i något av hantering av Azures tjänster SQL-databas, tabeller eller BLOB-är ganska snabbt. Har åtkomst till data som lagras i minnet är snabbare. Därför förbättra att behålla en kopia i minnet av data som används ofta programmets prestanda. Du kan använda Azures i minnet cachelagring toodo den.

Ett Cloud Services-program kan lagra data i det här cacheminnet och sedan hämta direkt utan att behöva tooaccess beständig lagring. hello cache kan underhållas i ditt program har virtuella datorer eller anges av virtuella datorer dedikerad enbart toocaching. I båda fallen hello cache kan distribueras, med hello data innehåller sprids över flera virtuella datorer i ett Azure-datacenter.

Azure har ett antal olika cache-tekniker som har flyttat över tid. Hello ordning som de har introducerats, det finns en delad i rollen hanteras och Redis-cache. hello delade cachelagring är en äldre teknik och du bör inte skapa nya implementeringar med den. hello har hanteras Cache hello samma funktioner för hello i rollen cachelagra, men som hanterad tjänst utanför hello Azure-hanteringsportalen. Hej Redis-Cache är i förhandsgranskningen. Hej Redis-implementering har hello största antal funktioner och rekommenderas när du skriver nya cachelagring kod.

**Scenarier för Azure Cache**

Ett program som läser en produktkatalogen upprepade gånger kan dra nytta av den här typen av cachelagring, till exempel sedan hello data måste blir tillgängliga snabbare. hello-tekniken stöder också låsa, så att den användas med läsning och skrivning som skrivskyddade data. Och ASP.NET-program kan använda hello service toostore sessionsdata med bara en konfigurationsändring.

### <a name="content-delivery-network"></a>Content Delivery Network
![Azure CDN](./media/fundamentals-introduction-to-azure/CDNIntroNew.png)   
 **Bild: Kopior av en blob kan cachelagras på platser hello världen.**

Anta att du behöver toostore blob-data som ska användas av användare hello världen. Kanske är det en video hello senaste VM matchar, till exempel eller drivrutinsuppdateringar populära e-bok. Spara en kopia av hello data i Azure-datacenter hjälper, men om det finns många användare kan det förmodligen inte är tillräckligt. Du kan använda hello Azure CDN för ännu bättre prestanda.

hello CDN har dussintals platser hello världen, alla kan lagra kopior av Azure-blobbar. hello kopieras första gången en användare i en del av hello world har åtkomst till en viss blob hello informationen från ett Azure-datacenter till lokal CDN lagring i den geografiska. Senare, har åtkomst från den del av hello world att använda hello blob kopia som cachelagrats i hello CDN-de behöver inte toogo alla hello sätt toohello närmaste Azure-datacenter. hello resultatet är snabbare åtkomst toofrequently komma åt data för användare var som helst i hälsningsmeddelande.

**CDN-scenarier**

Det är vanligt toouse CDN med Media Services toodeliver video över hela världen. Video är vanligtvis stor och kräver en stor mängd bandbredd.  Media Services är talade vi om någon annanstans på den här sidan.

## <a name="big-data-and-big-compute"></a>Big Data och stor beräkning
### <a name="hdinsight-hadoop"></a>HDInsight (Hadoop)
![HDInsight](./media/fundamentals-introduction-to-azure/HDInsightIntroNew.png)   
 **Bild: Kan hjälpa dig med hello bulk bearbetning av stora mängder data med HDInsight**

För många år arbetat hello huvuddelen av dataanalys med relationella data som lagras i ett datalager som skapats med en relationella DBMS. Den här typen av Företagsanalys fortfarande är viktigt och det är en toocome för lång tid. Men vad händer om hello data som du vill tooanalyze är så stort att relationsdatabaser bara inte kan hantera? Och anta hello data inte är relationell? Det kan vara serverloggen i ett datacenter, till exempel historiska händelsedata från sensorer eller något annat. I detta fall har du som kallas stordata problem. Du måste en annan metod.

hello företag teknik idag för analys av stordata är Hadoop. En Apache öppna projekt, den här tekniken lagrar data med hjälp av hello Hadoop Distributed File System (HDFS) och sedan kan utvecklare skapa MapReduce-jobb tooanalyze data. HDFS sprider ut data över flera servrar och sedan kör mängder hello MapReduce-jobb på var och en, så att hello stordata bearbetas parallellt.

HDInsight är hello namn hello Azure Apache Hadoop-baserad tjänst. HDInsight kan HDFS lagra data på hello kluster och fördela över flera virtuella datorer. Det också sprider sig hello logiken för ett MapReduce-jobb över de virtuella datorerna. Precis som med lokala Hadoop data är bearbetade lokalt-hello logik och hello data det fungerar på finns i hello samma virtuella dator- och parallellt för bättre prestanda. HDInsight kan också lagra data i Azure Storage valvet (ASV), som använder blobbar.  Använder ASV kan du toosave pengar eftersom du kan ta bort ditt HDInsight-kluster som, utan att dina data i hello molnet.

HDinsight stöder andra komponenter i hello Hadoop-ekosystemet, inklusive Hive och Pig. Microsoft har också skapat som gör det enklare toowork med data som produceras av HDInsight med hjälp av traditionella BI-verktyg, till exempel hello HiveODBC nätverkskort och Data Explorer som fungerar med Excel.

### <a name="high-performance-computing-big-compute"></a>Högpresterande datorbearbetning (stort beräkning)
En av hello mest bra sätt toouse en plattform i molnet är toorun högpresterande datorbearbetning (HPC) och andra ”stor Compute” program. Inkluderar specialiserade engineering program som bygger toouse hello branschstandard Message Passing Interface (MPI), samt s.k. embarrassingly parallella program, sådana finansiella risk modeller.

hello grunden för stora Compute kod körs på flera datorer på hello samtidigt. I Azure innebär det många virtuella datorer som körs samtidigt, arbetar i parallella toosolve problem. Detta kräver vissa sätt tooresources och tooschedule program, d.v.s. toodistribute sitt arbete på dessa instanser. Microsofts ledigt HPC Pack och andra lösningar för beräkning kan fungera bra i Azure, dra fördel av Azure beräkning och infrastruktur-tjänster tooadd kapacitet på begäran tooan lokalt beräkningskluster eller köra ett stort Compute program helt i hello moln.

Azure tillhandahåller ett intervall för VM instans storlekar med olika konfigurationer för CPU-kärnor, minne, diskkapacitet och andra egenskaper toomeet hello krav för olika program. hello nyligen introduceras A8 och A9 instanser fungerar bra för många compute-intensiv arbetsbelastning och parallella MPI program särskilt eftersom de har hög hastighet, flera kärnor processorer och stora mängder minne. I vissa konfigurationer dra hello instanser nytta av ett nätverk med låg latens och hög genomströmning i hello moln som innehåller remote direct memory access (RDMA)-teknik för maximal effektivitet parallella MPI applikationer.

Azure erbjuder också ett stort Compute programutvecklare och partners en fullständig uppsättning beräkningskapacitet, tjänster, arkitektur val och utvecklingsverktyg. Azure stöder anpassade stort Compute-arbetsflöden som rör särskilda data arbetsflöden och jobbet och schemaläggningstjänsten mönster som kan skalas toothousands av compute kärnor.

## <a name="media"></a>Media
![Azure Media Services](./media/fundamentals-introduction-to-azure/MediaServicesIntroNew.png)   
 **Bild: Media Services är en plattform för program som ger video och andra media tooclients hello världen.**

Video utgör en stor del av Internettrafik idag och så stor procentandel blir ännu större i morgon. Tillhandahåller ännu videon på hello web är enkel. Det finns många variabler, till exempel hello kodning algoritmen och hello visa lösning av hello användarens skärm. Video tenderar också toohave belastning i begäran som en lördag natt insamling när många bestämmer de som toowatch en online-film.

Dess popularitet är, det ett säkert val att många nya program kommer att skapas att använda bilden. Men alla behöver toosolve vissa av hello samma problem och att varje grupp lösa dessa problem på sin egen är meningslös. Är det bättre toocreate en plattform som innehåller vanliga lösningar för många program toouse. Och skapa den här plattformen i hello molnet har vissa Rensa fördelar. Det kan vara tillgänglig på grundval av betalning per användning och den kan också hantera hello variationer i begäran video program mot ofta.

Azure Media Services löser problemet. Det ger en uppsättning komponenter i molnet som gör liv enklare för användare att skapa och köra program med hjälp av video och andra.

Media Services tillhandahåller en uppsättning komponenter för program som fungerar med video och andra media som visar hello bild. Innehåller till exempel ett medium mata in komponenten tooupload video i Media Services (där den lagras i Azure BLOB), en kodning komponenten som har stöd för olika ljud och video-format, en komponent i innehållsskydd som tillhandahåller digital rights management en komponent för att infoga annonser i en video-ström och komponenter för strömning. Microsoft-partner kan också ange komponenter för hello plattform och har Microsoft distribuera dessa komponenter och debiterar åt.

Program som använder den här plattformen kan köras på Azure eller någon annanstans. Till exempel ett skrivbordsprogram för en video produktion house kan användarna ladda upp video tooMedia tjänster, sedan behandlas på olika sätt. Du kan också en molnbaserad innehållshantering-tjänsten körs på Azure förlitar sig på Media Services tooprocess och distribuera video. När den körs och vad som har varje program väljer vilka komponenter som behövs för toouse, komma åt dem via RESTful-gränssnitt.

toodistribute vad den genererar, ett program kan använda hello Azure CDN en annan CDN eller bara skicka bits direkt toousers. Men det får det, video som skapats med hjälp av Media Services kan användas av olika klientsystem, inklusive Windows, Macintosh, HTML 5, iOS, Android, Windows Phone, Flash och Silverlight. hello målet är toomake den enklare toocreate moderna medieprogram.

**Referenser**

En mer visuell översikt över hur Media Services fungerar kan du hämta hello [Azure Media Services affisch][Azure Media Services Poster].

## <a name="commerce"></a>Handel
hello ökning av programvara som en tjänst förändrar hur vi skapa program. Det även Omforma hur vi säljer program. Eftersom SaaS-program finns i molnet hello är logiskt att dess potentiella kunder ska söka efter lösningar online. Och den här ändringen gäller toodata samt tooapplications. Varför bör inte personer ut toohello molnet för kommersiellt tillgängliga datauppsättningar? Microsoft adresser båda dessa problem med hello [Azure Marketplace](https://azure.microsoft.com/marketplace/).

![Azure handel](./media/fundamentals-introduction-to-azure/CommerceIntroNew.png)   
 **Bild: Azure Marketplace och Azure Store kan du söka efter och köpa Azure-program och kommersiella datauppsättningar och använda dem som en del av din Azure-program.**

hello skillnaden mellan hello två är att Marketplace ligger utanför hello Azure-hanteringsportalen, men hello Store kan nås från inuti hello-portalen. Potentiella kunder kan söka toofind Azure program som uppfyller deras behov. Kunder kan söka efter kommersiell datauppsättningar, inklusive demografisk data, ekonomiska data, geografiska data och mycket mer. När de hittar något de som kommer de åt den från hello leverantör direkt via hello Marketplace eller Store webbplatser eller i vissa fall från hello-hanteringsportalen. Program kan också använda hello Bing Search API via hello Marketplace, ger dem åtkomst toohello resultat av webbsökningar.

**Commerce-scenarier**

SendGrid är ett program i hello Azure Store som du kan använda toosend e-post. Den erbjuder ytterligare funktioner som tillförlitliga leverans och statistik.  Du kan köpa programmet och relaterade tjänster i stället försök toobuild denna infrastruktur själv.  

## <a name="getting-started"></a>Komma igång
Nu när du har hello helheten, hello nästa steg är toowrite ditt första Azure-program. Välj önskat språk och [hämta hello lämpliga SDK](/downloads/), och gå för den. Cloud computing är hello nya standarden--Kom igång nu.

[Azure Media Services Poster]: http://azure.microsoft.com/documentation/infographics/media-services/
