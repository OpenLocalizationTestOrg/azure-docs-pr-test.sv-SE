---
title: "aaaWhat är nytt i Azure Data Catalog | Microsoft Docs"
description: "Den här artikeln innehåller en översikt över nya funktioner lagts till tooAzure Data Catalog."
services: data-catalog
documentationcenter: 
author: steelanddata
manager: NA
editor: 
tags: 
ms.assetid: 1201f8d4-6f26-4182-af3f-91e758a12303
ms.service: data-catalog
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-catalog
ms.date: 08/22/2017
ms.author: maroche
ms.openlocfilehash: f60130487ece39e110446b68544945089d2ab37e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="whats-new-in-azure-data-catalog"></a>Vad är nytt i Azure Data Catalog
Uppdateringar för**Azure Data Catalog** släpps regelbundet. Inte alla viktig innehåller nya funktioner för användarinriktad, vissa versioner fokuserar på funktioner för backend-tjänst. Den här sidan visar nya användarinriktad funktioner lagts till toohello Azure Data Catalog-tjänsten.

## <a name="whats-new-for-august-2017"></a>Vad är nytt för augusti 2017 
Från och med augusti 2017 har hello följande funktioner lagts tooAzure Data Catalog:

*   Ett exempel på en ny utvecklare är tillgänglig för att skapa och hantera relationen metadata med hjälp av hello Data Catalog REST API: et. Hej *importera relationsinformation till Data Catalog* exempel finns på hello [Data Catalog teckentabellen exempel](https://azure.microsoft.com/resources/samples/?service=data-catalog&sort=0). 
* Stöd för att extrahera Anslut relationen metadata från Teradata-datakällor när du registrerar relaterade tabeller med hello registreringsverktyget för datakällor.
* Stöd för SQL Server Tabellvärdesfunktionen (Tabellvärdesfunktion) objekt vid registrering av SQL Server-datakällor med hjälp av hello registreringsverktyget för datakällor.
* Flera uppdateringar och förfining tooincrease hello prestanda och användningsmöjligheter hello Data Catalog-portalen.

## <a name="whats-new-for-july-2017"></a>Vad är nytt för juli 2017 
Från och med juli 2017 har hello följande funktioner lagts tooAzure Data Catalog:
*   Stöd för mer detaljerad kontroll över tillåts metadataåtgärder inklusive:
    - Katalogadministratörer kan begränsa användarnas möjlighet toocontribute taggar och tillhörande metadata toohello katalog att aktivera skrivskyddad åtkomst toohello katalog.
    - Katalogadministratörer kan begränsa användarnas möjlighet tooregister nya datakällor i hello-katalogen.
    - Katalogadministratörer kan begränsa användarnas möjlighet tootake ägarskap för data tillgångens metadata i hello-katalogen.
    - Behörighet kan beviljas tooAzure Active Directory-säkerhetsgrupper och användare för att underlätta hanteringen av behörigheter.
* Stöd för relationer mellan registrerade datatillgångar och identifierande relaterade datatillgångar i hello Data Catalog-portalen, inklusive:
    - Extrahering av relationen metadata från SQL Server (inklusive Azure SQL Database), Oracle och MySQL hello datakällor när du använder Data Catalog registreringsverktyget för datakällor.
    - Identifiering av relaterade datatillgångar när du visar tillgångens metadata i hello Data Catalog-portalen.
    - Operations toodefine, identifiera och hantera relationer mellan datatillgångar med hjälp av hello Data Catalog REST API: et.

Mer information om hur du hanterar behörigheter i Data Catalog finns [hur toosecure åt toodata katalog och data tillgångar](data-catalog-how-to-secure-catalog.md).
Ytterligare information om relationer i Data Catalog finns [hur tooview relaterade datatillgångar i Azure Data Catalog](data-catalog-how-to-view-related-data-assets.md).

## <a name="whats-new-for-june-2017"></a>Vad är nytt för juni 2017 
Från och med juni 2017 har hello följande funktioner lagts tooAzure Data Catalog:
*   Stöd för Sybase och Apache Cassandra MongoDB-datakällor. Användare kan nu registrera och identifiera Cassandra och MongoDB databaser och tabeller och Sybase-databaser tabeller och vyer.

> [!NOTE]
> När registreringen MongoDB och Cassandra tabeller som innehåller kolumner med komplexa datatyper, till exempel poster och matriser, dessa kolumner tas inte med i hello metadata läggs tooData katalog.

## <a name="whats-new-for-may-2017"></a>Vad är nytt för maj 2017 
Från och med kan 2017 har hello följande funktioner lagts tooAzure Data Catalog:
*   Ett exempel på en ny utvecklare är tillgänglig för hello massimport ordlista för företag. hello ordlista massimport exempel finns på hello [exempelsida för Data Catalog developer](https://docs.microsoft.com/azure/data-catalog/data-catalog-samples). 
*   Stöd för att redigera anslutningsinformationen för ODBC hello Azure Data Catalog-portalen. Data tillgångens ägare och administratörer för Data Catalog kan nu redigera hello anslutningsinformationen för registrerade ODBC-datakällor utan att behöva registrera toore hello-datakällor.
*   Stöd för klickbara URL: er i företag ordlista definitioner och beskrivningar. När URL: er ingår i egenskaperna för hello beskrivning och definition för business ordlista, visas hello URL-adresser som hyperlänkar i hello Data Catalog-portalen.
*   Stöd för att visa expert namn dessutom tooexpert e-postadresser. När du visar experter i hello egenskaperna för en datatillgång i hello Data Catalog-portalen, visas hello expert fullständiga namn från Azure Active Directory i hello verktygstipset.

## <a name="whats-new-for-april-2017"></a>Vad är nytt för April 2017 
Från och med April 2017 har hello följande funktioner lagts tooAzure Data Catalog:
*   Stöd för ODBC-datakällor. Användare kan nu registrera och identifiera ODBC-databaser, tabeller och vyer med hjälp av hello Data Catalog registreringsverktyget för datakällor.

## <a name="whats-new-for-march-2017"></a>Vad är nytt för mars 2017 
Från och med mars 2017 har hello följande funktioner lagts tooAzure Data Catalog:
*   Stöd för användning av AAD-säkerhetsgrupper för Data Catalog-administratörer.
*   Azure Data Catalog är nu tillgänglig i en ny Azure-region. Kunder kan etablera hello Azure Data Catalog i hello Väst centrala oss region med tillägget tooEast USA, västra USA, västra Europa och östra, Norra Europa och Sydostasien. Mer information finns i [Azure-regioner](https://azure.microsoft.com/regions/).

## <a name="whats-new-for-february-2017"></a>Vad är nytt för februari 2017 
Från och med februari 2017 har hello följande funktioner lagts tooAzure Data Catalog:
*   Stöd för avancerade inställningar i hello Data Catalog registreringsverktyget för datakällor. Användarna kan ange timeout-värdena för kommandot vid registrering av stora datakällor.
*   Stöd för FTP- och PostgreSQL-datakällor. Användare kan nu registrera och identifiera FTP-filer och kataloger, och PostgreSQL-databaser, tabeller och vyer.

## <a name="whats-new-for-january-2017"></a>Vad är nytt för januari 2017 
Från och med januari 2017 har hello följande funktioner lagts tooAzure Data Catalog:
*   Azure Data Catalog är nu [CSA STAR](https://www.microsoft.com/trustcenter/compliance/csa-star-certification) kompatibla.
*   Integrering med [Hämta & transformeringen i Excel 2016 och Power Query för Excel](https://support.office.com/article/Introduction-to-Microsoft-Power-Query-for-Excel-6E92E2F4-2079-4E1F-BAD5-89F6269CD605). Excel-användare kan dela frågor och identifiera frågor med Azure Data Catalog från i Excel. Den här funktionen är tillgänglig toousers med Power BI Pro licenser.

## <a name="whats-new-for-december-2016"></a>Vad är nytt för December 2016
Från och med December 2016 har hello följande funktioner lagts tooAzure Data Catalog:
*   Azure Data Catalog är nu [HIPAA](https://www.microsoft.com/trustcenter/Compliance/HIPAA) och [EU-standardavtalsklausuler](https://www.microsoft.com/TrustCenter/Compliance/EU-Model-Clauses) kompatibla.
*   Stöd för att redigera anslutningsinformationen för datakällan. Data tillgångens ägare och administratörer för Data Catalog kan nu redigera hello anslutningsinformationen för registrerade datakällor utan att behöva registrera toore hello-datakällor.
*   Stöd för Salesforce.com-datakällor. Användare kan nu registrera och identifiera Salesforce-objekt.


## <a name="whats-new-for-november-2016"></a>Vad är nytt för November 2016
Från och med November 2016 har hello följande funktioner lagts tooAzure Data Catalog:
*   Azure Data Catalog är nu [ISO/IEC 27001](https://www.microsoft.com/trustcenter/compliance/iso-iec-27001) och [ISO/IEC 27018](https://www.microsoft.com/TrustCenter/Compliance/iso-iec-27018) kompatibla.
*   Stöd för hello manuell registrering av ODBC-datakällor med hjälp av REST API: er och hello Data Catalog-portalen.

## <a name="whats-new-for-september-2016"></a>Vad är nytt för September 2016
Från och med September 2016 har hello följande funktioner lagts tooAzure Data Catalog:

* Stöd för IBM DB2-datakällor. Användare kan nu registrera och identifiera DB2 databaser, tabeller och vyer.
* Stöd för Azure Cosmos DB-datakällor. Användare kan nu registrera och identifiera Cosmos-DB-databaser och samlingar.
* Stöd för att anpassa hello katalognamnet i hello Data Catalog-portalen. Katalogadministratörer kan nu ange text som visas i hello portal rubrik, till exempel hello organisationsnamn.

## <a name="whats-new-for-august-2016"></a>Vad är nytt för augusti 2016
Från och med augusti 2016 har hello följande funktioner lagts tooAzure Data Catalog:

* Förbättringar för registrering av SQL Server Master Data Services (MDS) datakällor. Användare kan nu inkludera profiler för förhandsversioner och data när du registrerar MDS-enheter med hjälp av hello Data Catalog registreringsverktyget för datakällor.
* Stöd för administratörsdefinierad organisationens sparade sökningar. När du sparar en sökning i hello Data Catalog-portalen, kan Data Catalog-administratörer nu välja toosave sökningar för personligt bruk eller för alla användare i katalogen. Organisationens sparade sökningar delas med alla användare i katalogen och kan erbjuda standardiserad startpunkter för upptäckt av datakälla.
* Uppdaterar toohello egenskapsvyn i hello Data Catalog-portalen. Egenskaper för resurs för alla data visas och hanteras i en enda, ändringsbara fönstret tooprovide en mer konsekvent och identifierbart upplevelse.

## <a name="whats-new-for-july-2016"></a>Vad är nytt för juli 2016
Hello följande funktioner har lagts tooAzure Data Catalog för juli 2016:

* Stöd för SQL Server Master Data Services (MDS)-datakällor. Användare kan nu registrera och identifiera MDS-modeller och entiteter.
* Stöd för SQL-lagrade procedurer. Användare kan nu registrera och identifiera lagrade proceduren objekt i SQL Server-datakällor.
* Stöd för ytterligare språk i hello Azure Data Catalog-portalen och hello registreringsverktyget för datakällor, totalt 18 språk som stöds. hello Azure Data Catalog användarupplevelsen är lokaliserade baserat på hello språkinställningar ställa in i Windows eller hello webbläsare.
* Uppdateringar och förfining för hello Data Catalog portalens startsida, inklusive prestandaförbättringar och en effektiviserad upplevelse.

## <a name="whats-new-for-june-2016"></a>Vad är nytt för juni 2016
Från och med juni 2016 har hello följande funktioner lagts tooAzure Data Catalog:

* Stöd för att ändra storlek på kolumner i listvyn hello när identifiera datatillgångar i hello Data Catalog-portalen. Användare kan nu ändra storlek på enskilda kolumner toomore enkelt läsa långa tillgångens metadata, till exempel taggar och beskrivningar.
* Power Query för Excel har lagts toohello ”öppna i”-menyn i hello Data Catalog-portalen. Nu kan du öppna datakällor som stöds i Excel 2016 eller i Excel 2010 och Excel 2013 med hello [Power Query för Excel](https://support.office.com/article/Introduction-to-Microsoft-Power-Query-for-Excel-6E92E2F4-2079-4E1F-BAD5-89F6269CD605) installerat.
* Stöd för Azure Table Storage-datakällor. Användare kan nu registrera och identifiera tabellobjekt i Azure Storage-datakällor.

## <a name="whats-new-for-may-2016"></a>Vad är nytt för maj 2016
Från och med maj 2016 har hello följande funktioner lagts tooAzure Data Catalog:

* En Företagsordlista som hjälper katalogadministratörer toodefine business villkoren och hierarkier toocreate en gemensam ordlista för företag. Användare kan tagga registrerade datatillgångar med ordlista villkoren toomake den enklare toodiscover och förstå hello innehållet i hello katalog. Mer information finns i [hur tooset in hello Företagsordlista för hanterade taggar](data-catalog-how-to-business-glossary.md)  
* Förbättringar av toohello Data Catalog Företagsordlista som hjälper användare tooupdate flera ordlista i en enda åtgärd. Användare kan välja flera villkor tooedit hello följande fält:
  * Överordnad Term: hello användaren kan välja en ny överordnad term och alla valda villkor är uppdaterade toobe underordnade hello valda överordnade har löpt ut. Om hello markerade villkoren har hello samma överordnade och det överordnat visas i textrutan hello, annars hello överordnade termen fältet är uppsättningen tooblank.   
  * Taggar och intressenter: användare kan lägga till och ta bort taggar och intressenter för flera ordlista med hello samma upplevelse som taggning flera datatillgångar.

> [!NOTE]
> Hej företagsordlista är endast tillgänglig i hello Standard Edition av Azure Data Catalog. hello ger Free Edition inte några funktioner för styrda märkning eller en företagsordlista.

## <a name="whats-new-for-march-2016"></a>Vad är nytt för mars 2016
Hello följande funktioner har lagts tooAzure Data Catalog: g för mars 2016:

* Konsoliderade REST API-slutpunkt för att komma åt hello sökfunktioner och katalogen tillgången funktioner för hantering av hello Azure Data Catalog-tjänsten. Den här sökningen API-slutpunkten och katalogen API-slutpunkten har föråldrad och upphöra att gälla den 21 mars 2016. Det finns inga ändringar toohello semantiken för hello API. Hello-slutpunkten URI: N ändras. Mer information finns i hello [Azure Data Catalog REST API-referens](https://msdn.microsoft.com/library/azure/mt267595.aspx). API-exempel finns [Azure Data Catalog developer exempel](data-catalog-samples.md).

## <a name="whats-new-for-february-2016"></a>Vad är nytt för februari 2016
Från och med februari 2016 har hello följande funktioner lagts tooAzure Data Catalog:

* Källan för nyligen omdesignat dataurval upplevelse i hello Azure Data Catalog registreringsverktyget för datakällor. hello registreringsverktyget för datakällor har uppdaterats toomake it enklare du toolocate och välj från hello datakällor som stöds av Azure Data Catalog.
* Stöd för 10 ytterligare språk i hello Azure Data Catalog-portalen och hello registreringsverktyget för datakällor. Dessutom tooEnglish, hello Azure Data Catalog upplevelse är nu tillgänglig i tyska, spanska, franska, italienska, japanska, koreanska, portugisiska (Brasilien), ryska, förenklad och traditionell kinesiska. hello Azure Data Catalog användarupplevelse lokaliserade baserat på hello språkinställningar ställa in i Windows eller hello användarens webbläsare.
* Stöd för geo-replikering av data i Azure Data Catalog för verksamhetskontinuitet och katastrofåterställning. Allt innehåll i Azure Data Catalog, inklusive data käll-metadata och användargenererade anteckningar, replikeras nu mellan två Azure-regioner på toocustomers utan extra kostnad. hello Azure-regioner är före länkas till minst 500 miles från varandra, och följ hello mappning enligt beskrivningen i [Business affärskontinuitet och haveriberedskap återställning (BCDR): parad Azure-regioner](../best-practices-availability-paired-regions.md).
* Stöd för att ändra hello Azure-prenumeration används av Azure Data Catalog. Azure Data Catalog-administratörer kan använda hello inställningssidan i hello Azure Data Catalog-portalen tooselect Azure-prenumeration för fakturering.

## <a name="whats-new-for-january-2016"></a>Vad är nytt för januari 2016
Från och med januari 2016 har hello följande funktioner lagts tooAzure Data Catalog:

* Stöd för att manuellt registrera ytterligare datakällor. Du kan nu använda ”skapa manuella transaktionen” hello Azure Data Catalog-portalen eller använda hello Azure Data Catalog REST API: et tooregister hello följande datakällor:
  * OData - funktionen, Entitetsuppsättning och Entitetsbehållaren
  * HTTP - fil, slutpunkt, rapporten och plats
  * Filsystem - fil
  * SharePoint - lista
  * FTP - filer och kataloger
  * Salesforce.com - objekt
  * DB2 - tabellen, vyn och databas
  * PostgreSQL - tabellen, vyn och databas
* Stöd för ”öppna i SQL Server Data Tools” för SQL Server (inklusive Azure SQL DB och Azure SQL Data Warehouse)-datakällor.  
* Stöd för registrering och identifiera SAP HANA-vyer och paket. Du kan registrera data för SAP HANA källor med hjälp av hello Azure Data Catalog data source registreringsverktyget kan kommentera och identifiera registrerade datakällor för SAP HANA med hello Azure Data Catalog-portalen.
* Hej möjlighet toopin och ta bort datatillgångar i hello Azure Data Catalog-portalen. Du kan välja toopin data tillgångar toomake dem enklare toorediscover och återanvändning.
* En nyligen omdesignat startsida hello Azure Data Catalog-portalen. hello ny hemsida innehåller inblick i hello aktuell användare aktivitet – inklusive nyligen publicerade tillgångar Fäst tillgångar och sparade sökningar – och insyn i hello aktivitet i hello katalog som helhet.
* Stöd för beständig användarinställningar i hello Azure Data Catalog-portalen. Inställningar användargränssnitt - inklusive rutnät eller panelen Visa hello antalet resultat per sida och tryck eller inaktivera markering - beständiga mellan sessioner.
* Azure Data Catalog finns nu i två nya Azure-regioner. Kunder kan etablera hello Azure Data Catalog i hello Norra Europa och Sydostasien regioner i tillägg tooEast USA, västra USA, västra Europa och östra. Mer information finns i [Azure-regioner](https://azure.microsoft.com/regions/).

> [!NOTE]
> ”Öppna i SQL Server Data Tools” kräver Visual Studio 2013 med Update 4 och SQL Server Tooling toobe installerad. tooinstall hello senaste versionen av SQL Server Data Tools finns [Hämta SQL Server Data Tools (SSDT)](https://msdn.microsoft.com/library/mt204009.aspx).


## <a name="whats-new-for-december-2015"></a>Vad är nytt för December 2015
Från och med December 2015 har hello följande funktioner lagts tooAzure Data Catalog:

* Stöd för data-profiler för Azure SQL Data Warehouse-datakällor. När du registrerar Azure SQL Data Warehouse-tabeller och vyer, kan användare välja tooinclude data profil mått med hello metadata som extraherats från hello-datakälla.
* Stöd för registrering och identifiera MySQL-objekt och databaser. Användarna kan registrera MySQL data källor med hjälp av hello Azure Data Catalog data source registreringsverktyget kan kommentera och identifiera registrerade MySQL-datakällor med hello Azure Data Catalog-portalen.
* Stöd för SPNEGO och Windows-autentisering för Teradata-datakällor. När du registrerar Teradata-tabeller och vyer, kan användare välja tooconnect tooTeradata med SPNEGO och Windows, och LDAP- och TD2 autentisering.
* Stöd för Azure Data Lake Store-datakällor. Användare kan nu registrera och identifiera datakällor i Azure Data Lake Store med hjälp av Azure Data Catalog.
* Stöd för att manuellt ange proxyinställningar för nätverk i hello Azure Data Catalog registreringsverktyget för datakällor. Användare kan välja ”Ändra proxyinställningar” hello verktyget välkomstsidan och kan ange hello proxy-adress och port toobe används av hello-verktyget.

## <a name="whats-new-for-november-2015"></a>Vad är nytt för November 2015
Från och med November 2015 har hello följande funktioner lagts tooAzure Data Catalog:

* Hej möjlighet tooview och kopiera anslutningssträngar i hello Azure Data Catalog-portalen för SQL Server (inklusive Azure SQL Database) och Oracle-datakällor. Användarna kan klicka på länken ”Visa anslutningssträngar” i hello anslutningsinformationen för SQL Server- eller Oracle tabeller, vyer och databasen toosee hello anslutningssträngar används tooconnect toohello datakälla. ADO.NET, ODBC, OLEDB och JDBC-anslutningssträngar har angetts för SQL Server-datakällor. ODBC- och OLEDB-anslutningssträngar tillhandahålls för Oracle-datakällor.
* Stöd för inklusive data profiler när du registrerar Teradata-tabeller och vyer.
* Stöd för ”öppna i Power BI Desktop” för SQL Server (inklusive Azure SQL DB och Azure SQL Data Warehouse), SQL Server Analysis Services, Azure Storage och HDFS källor.  
* Stöd för LDAP-autentisering för Teradata-datakällor. När du registrerar Teradata-tabeller och vyer som användarna kan välja tooconnect tooTeradata använder LDAP, och TD2 autentisering.
* Stöd för ”öppna i Excel” för Teradata-datakällor.
* Stöd för senaste sökorden i hello Azure Data Catalog-portalen. När du söker i hello portal kan kan användarna välja från nyligen använda termer tooaccelerate hello identifiering sökinställningar.
* Stöd för förhandsgranskning för Teradata-datakällor. När du registrerar Teradata-tabeller och vyer, kan användare välja tooinclude ögonblicksbild poster med hello metadata som extraherats från hello-datakälla.
* Stöd för ”öppna i Excel” för Azure SQL Data Warehouse-datakällor.
* Stöd för att definiera och redigera kolumnnivå scheman för manuellt registrerade datatillgångar. När du har skapat en datatillgång med hello Azure Data Catalog-portalen manuellt, användare kan lägga till kolumndefinitionerna hello dataegenskaper för tillgången.
* Stöd för ”har” frågor när du söker Azure Data Catalog, tooenable hello identifiering av registrerade datatillgångar som har specifika metadata. Azure Data Catalog frågesyntaxen innehåller nu:

| Frågesyntaxen | Syfte |
| --- | --- |
| `has:previews` |Söker efter datatillgångar som innehåller en förhandsgranskning |
| `has:documentation` |Söker efter datatillgångar som dokumentationen har angetts |
| `has:tableDataProfiles` |Söker efter datatillgångar med data på tabellen nivå profilinformation |
| `has:columnsDataProfiles` |Söker efter datatillgångar med kolumnnivå data profilinformation |


> [!NOTE]
> ”Öppna i Power BI Desktop” kräver en aktuell version av hello Power BI Desktop programmet toobe installerad. Om det uppstår problem eller fel med den här funktionen kan du se till att du har hello senaste versionen av Power BI Desktop från [PowerBI.com](https://powerbi.com).


## <a name="whats-new-for-october-2015"></a>Vad är nytt för oktober 2015
Från och med oktober 2015 har hello följande funktioner lagts tooAzure Data Catalog:

* Stöd för kryptering av vilande data förhandsversioner och data profiler för registrerade datakällor. Azure Data Catalog krypterar transparent alla poster för förhandsgranskning och data profiler datakällor som har registrerats hello-tjänsten, utan något behov för hantering av nycklar av katalogadministratörer.
* Stöd för Teradata-datakällor. Användare kan nu registrera och identifiera Teradata-tabeller och vyer.
* Stöd för lokala Hive-datakällor. Användare kan nu registrera och identifierar Hive-tabeller för Apache Hive i Hadoop lokala datakällor.
* Stöd för sparade sökningar i hello Azure Data Catalog-portalen. Användare kan spara sökvillkoren och filter val tooeasily Upprepa föregående sökningar och definiera användbar vyer för hello katalogens innehåll. Användaren kan också markera en sparad sökning som standardsökningen. När en användare klickar på hello ”” Sök förstoringsglaset från hello Azure Data Catalog portalens startsida eller hello ”Kom-igång-sidan, tas hello användaren direkt toohello sparad sökning som flaggats som standard.
* Stöd för RTF-dokumentation för registrerade datatillgångar och behållare i hello Azure Data Catalog-portalen. Användare kan nu tillhandahålla dokumentation för datatillgångar som tabeller, vyer och rapporter, och för behållare, till exempel databaser och modeller för scenarier där taggar och beskrivningar inte är tillräckliga.
* Stöd för manuell registrering av kända typer av datakällor. Användare kan manuellt ange information om datakällan använder hello Azure Data Catalog-portalen för alla typer av datakällor stöds av Azure Data Catalog.
* Stöd för auktorisering av Azure Active Directory-säkerhetsgrupper. Katalogadministratörer kan aktivera åtkomst toosecurity kataloggrupper, användarkonton, vilket gör det enklare toomanage åtkomst tooAzure Data Catalog.
* Stöd för att öppna Hive-datakällor i Excel från hello Azure Data Catalog-portalen.

> [!NOTE]
> För hello aktuella versionen kan stöds endast Teradata TD2 autentisering. Ytterligare autentisering autentiseringsmetoder som stöds i framtida versioner.

> [!NOTE]
> toouse hello ”öppna i Excel” funktionen för Hive-datakällor, användare måste ha installerat hello ODBC-drivrutinen för Hive.

## <a name="whats-new-for-september-2015"></a>Vad är nytt för September 2015
Från och med September 2015 har hello följande funktioner lagts tooAzure Data Catalog:

* Stöd för inklusive data profiler när du registrerar Hive-datakällor.
* Stöd för identifiering av programmässigt hello Catalog-API, vilket gör det lättare för toointegrate program med Azure Data Catalog.
* En ny ”komma igång” datakälla upplevelsen av identifieringen hello Azure Data Catalog-portalen. När användarna skriver in hello ”identifiera” sidan hello Azure Data Catalog-portalen utan att ange en sökterm, visas en översikt över hello katalog innehåll inklusive hello används oftast taggar, experter, typer av datakällor och objekttyper.
* Stöd för registrering och identifiera Azure SQL Data Warehouse-objekt och databaser. Ytterligare information om Azure SQL Data Warehouse finns [SQL Data Warehouse](https://azure.microsoft.com/services/sql-data-warehouse/).
* Stöd för registrering och identifiering av SQL Server Analysis Services-modeller och SQL Server Reporting Services-servrar som behållare. När registrering SSAS och SSRS-objekt, skapas en post för hello SSAS modellen och SSRS-servern och för hello rapporter och andra objekt i Azure Data Catalog. hello behållare kan identifieras och kommenterats med hello Azure Data Catalog-portalen. Användarna kan också söka och filtrera hello-innehållet i en modell eller en server i tillägget toosearching och filtrering hello innehållet i hello katalog.
* Stöd för registrering och identifiera SQL Server Analysis Services-objekt via HTTP eller HTTPS. Användare kan nu ansluta tooSSAS servrar med hjälp av en URL (t.ex https://servername/olap/msmdpump.dll) i stället för ett servernamn och använda grundläggande autentisering och anonyma anslutningar i tillägg tooWindows autentisering. Ytterligare information om HTTP/HTTPS-anslutningar tooSSAS finns [konfigurera HTTP-tooAnalysis åtkomsttjänster](https://msdn.microsoft.com/library/gg492140.aspx).
* Stöd för Hive-datakällor på HDInsight. Användare kan nu registrera och identifierar Hive-tabeller för Apache Hive i Hadoop på HDInsight-datakällor. Ytterligare information om Hive i HDInsight finns hello [HDInsight Dokumentationscenter](../hdinsight/hdinsight-use-hive.md).
* Stöd för registrering och identifiering av Oracle-databaser och HDFS-kluster som behållare. När registrering Oracle-tabeller och vyer eller HDFS, skapas en post för hello-databasen, tabeller och vyer i Azure Data Catalog. hello databasen kan identifieras och kommenterats med hello Azure Data Catalog-portalen. Användarna kan också söka och filtrera hello innehållet i en databas eller kluster dessutom toosearching och filtrering hello innehållet i hello katalog.
* Stöd för manuell registrering av okända typer av datakällor. Användare kan ange information om datakälla med hjälp av hello Azure Data Catalog-portalen manuellt så att datakällor stöds inte uttryckligen av hello registreringsverktyget för datakällor kan kommenterats och identifieras.
* Stöd för registrering och identifiering av SQL Server-databaser som behållare. När registrering SQL Server-tabeller och vyer, skapas en post för hello-databasen, tabeller och vyer i Azure Data Catalog. hello databasen kan identifieras och kommenterats med hello Azure Data Catalog-portalen. Användarna kan också söka och filtrera hello innehållet i en databas i tillägget toosearching och filtrering hello innehållet i hello katalog.

> [!NOTE]
> SSAS och SSRS-objekt som har varit registrerade tidigare toohello September 18 versionen måste registreras igen med hjälp av registreringsverktyget för datakällor hello innan hello modell eller server läggs post toohello katalog. Registrera en datakälla påverkar inte eventuella kommentarer som lagts till av användare i hello Azure Data Catalog-portalen.

> [!NOTE]
> Oracle-tabeller och vyer och HDFS filer och kataloger som är registrerade tidigare toohello September 11 versionen måste registreras igen med hjälp av registreringsverktyget för datakällor hello innan hello databas eller ett kluster läggs toohello katalog. Registrera en datakälla påverkar inte eventuella kommentarer som lagts till av användare i hello Azure Data Catalog-portalen.

> [!NOTE]
> SQL Server-tabeller och vyer som är registrerade tidigare toohello September 4 versionen måste registreras igen med hjälp av registreringsverktyget för datakällor hello innan hello databasposten läggs toohello katalog. Registrera en datakälla påverkar inte eventuella kommentarer som lagts till av användare i hello Azure Data Catalog-portalen.

## <a name="whats-new-for-august-2015"></a>Vad är nytt för augusti 2015
Från och med augusti 2015 har hello följande funktioner lagts tooAzure Data Catalog:

* Stöd för data profilering av SQL Server och Oracle-datakällor. När du registrerar SQL Server och Oracle-tabeller och vyer, kan användare välja tooinclude data profilinformation för hello objekt registreras. hello data profilen innehåller statistik på objektnivå och på kolumnnivå.
* Stöd för Hadoop HDFS-datakällor. Användare kan nu registrera och identifiera HDFS filer och kataloger.
* Stöd för att tillhandahålla information om åtkomstbegäran för registrerade datakällor. Användare kan nu ange instruktioner för att begära åtkomst, inklusive e-länkar eller URL: er för alla registrerade datatillgångar, tooeasily integrera med befintliga verktyg och processer.
* Verktyget som tips för taggar och experter, toomake-den-enklare toodiscover vad användare har angett vilka metadata för registrerade datatillgångar.
* Vi har lagt till en ny ”användare” knappen och menyn tooour övre navigeringsfältet. Den här menyn tillåter hello användaren finns hello konto som används för toolog på tooAzure Data Catalog och toosign ut om du vill. Den här menyn visas även hello katalognamn som är värdefull toodevelopers med hello Azure Data Catalog REST API: et.
* Standard Edition: När du lägger till ägare toodata tillgångar Azure Data Catalog stöder nu både användarkonton och säkerhetsgrupper som ägare. tooadd en säkerhetsgrupp som ägare för valda datatillgångar, du kan ange antingen hello gruppens namn eller hello grupp UPN-e-postadress om den har en.
* Stöd för Azure Blob Storage-datakällor. Användare kan nu registrera och identifiera Azure Storage-blobbar och kataloger.

