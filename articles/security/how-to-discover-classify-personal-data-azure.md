---
title: aaaDiscover, identifiera och klassificera personliga data i Microsoft Azure | Microsoft Docs
description: "Lär dig mer om sökning, klassificera, upptäcka och identifiera data"
services: security
documentationcenter: na
author: barclayn
manager: MBaldwin
editor: TShinder
ms.assetid: 
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/24/2017
ms.author: barclayn
ms.custom: 
ms.openlocfilehash: af4ced1c57699dc751d55cfdf3229c7d294648a9
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="discover-identify-and-classify-personal-data-in-microsoft-azure"></a>Identifiera, identifiera och klassificera personliga data i Microsoft Azure

Den här artikeln innehåller information om hur toodiscover, identifiera och klassificera personliga data i flera Azure-verktyg och tjänster, inklusive användning av Azure Data Catalog, Azure Active Directory, SQL-databas, Power Query för Hadoop-kluster i Azure HDInsight, Azure Informationsskydd, Azure Search och SQL-frågor för Azure Cosmos DB.

## <a name="scenario-problem-statement-and-goal"></a>Scenario, problembeskrivning och mål

Ett företag i USA-baserade sport samlar in en mängd olika personliga och andra data. Detta inkluderar kunder och information om anställda. hello företaget behålls i flera databaser och lagrar den i flera olika ställen i sina Azure-miljön. Dessutom tooselling sport utrustning, de också vara värd för och hantera registrering för elite inom händelser hello världen, inklusive i hello EU, och i vissa fall hello kundinformation som samlas in innehåller medicinska information.

Eftersom hello företaget värd många internationella bicycling visningar för varje år och har tillfällig personal på platser runt hello världen, ett par hello datauppsättningar är ganska stor. hello företaget har också inbyggd developer program som används av kunder och anställda.

hello företaget vill ha tooaddress hello följande problem:

- Kund- och personliga data måste vara klassificeras/särskiljas från hello andra data hello företaget samlar in i ordning tooensure rätt åtkomst och säkerhet.
- hello data administratören behöver tooeasily identifiera hello platsen för kundens personliga data mellan olika områden i hello Azure-miljön.
- Kund- och personliga data som visas i delade dokument och e-postmeddelanden måste märkas toohelp se till att den förblir säkra.
- hello företagets apputvecklare behöver ett sätt tooeasily Sök efter kund- och personliga data i webb- och mobilappar.
- Utvecklare måste också tooquery sina dokumentdatabasen för personliga data.

### <a name="company-goals"></a>Företagets mål

- Alla kund- och personliga data måste vara taggade/kommenterade i Azure Data Catalog kan hittas enkelt. Helst är kund- och personliga data märkta/kommenterats separat.
- Personliga data från customer och medarbetare användarprofiler och information om arbete som finns i Azure Active Directory måste vara enkel att hitta.
- Personliga data som finns i flera SQL-databaser måste efterfrågas enkelt. 
- Vissa av hello företagets stora datamängder hanteras via Azure HDInsight och lagras i Hadoop. De måste importeras till Excel så att de kan efterfrågas för personliga data.
- Personliga data delas i dokument och e-postmeddelanden måste klassificeras, etiketter och förvaras skyddade med Azure Information Protection.
- hello måste företagets apputveckling vara kan toodiscover kund- och personliga data i hello-appar som de har skapat och som de kan göra med Azure Search.
- Utvecklare måste vara kan toofind personliga data i sin databas för dokumentet.

## <a name="azure-active-directory-data-discovery"></a>Azure Active Directory: Identifiering av Data

[Azure Active Directory](https://azure.microsoft.com/services/active-directory/) är Microsofts molnbaserade, flera innehavare katalog och identity management-tjänsten. Du kan hitta kund- och användarprofiler och arbete användarinformation som innehåller personuppgifter inom din [Azure Active Directory](https://azure.microsoft.com/services/active-directory/) (AAD)-miljö med hjälp av hello [Azure-portalen](https://portal.azure.com/).

Detta är särskilt användbart om du vill toofind eller ändra personliga data för en viss användare. Du kan också lägga till eller ändra användarprofil och information om arbete. Du måste logga in med ett konto som är en global administratör för hello-katalogen.

### <a name="how-do-i-locate-or-view-user-profile-and-work-information"></a>Hur jag hitta eller visa användarprofil och fungerar information?

1. Logga in toohello [Azure-portalen](https://portal.azure.com) med ett konto som är en global administratör för hello-katalogen.

2. Välj **fler tjänster**, ange **användare och grupper** i hello textruta och välj sedan **RETUR**.

   ![Hur gör hitta användarprofil och information om arbete](media/how-to-discover-classify-personal-data-azure/user-profile.png)

3. På hello **användare och grupper** bladet väljer **användare**.

  ![Öppna användare och grupper](media/how-to-discover-classify-personal-data-azure/users-groups.png)

4. På hello **användare och grupper – användare** bladet Välj en användare hello listan, och på hello bladet för hello vald användare Markera **profil** tooview användarens profilinformation som kan innehålla personliga data .

  ![Välj användare](media/how-to-discover-classify-personal-data-azure/select-user.png)

5. Om du behöver tooadd eller ändra användarens profilinformation du göra det och markera i kommandofältet hello **spara.**
6. På hello bladet för valda hello-användaren väljer **fungerar Info** tooview arbete användarinformation som kan innehålla personuppgifter.

 ![Visa arbetsinformation](media/how-to-discover-classify-personal-data-azure/work-info.png)

7. Om du behöver tooadd eller ändrar användarinformation arbete du göra det och markera i kommandofältet hello **spara.**

## <a name="azure-sql-database-data-discovery"></a>Azure SQL Database: Data identifiering

[Azure SQL Database](https://azure.microsoft.com/services/sql-database/?v=16.50) är en databas i molnet som gör att utvecklare kan skapa och hantera program. Personliga data finns i [Azure SQL Database](https://azure.microsoft.com/services/sql-database/?v=16.50) med standard SQL-frågor. Azure SQL-elastisk fråga (förhandsgranskning) gör det möjligt för användare tooperform cross-databasfrågor.

En detaljerad [SQL-databas](../sql-database/sql-database-technical-overview.md) kursen förklaras många aspekter av att använda en SQL-databas, inklusive hur toobuild en och hur toorun datafrågor. hello följer en sammanfattning av hello information finns i hello självstudier med länkar toospecific avsnitt.

### <a name="how-do-i-build-a-sql-database"></a>Hur skapar jag en SQL-databas

Det finns tre sätt toodo den:

- Du kan skapa en Azure SQL database i hello [Azure-portalen](https://portal.azure.com/). I hello kursen ska du använda en specifik uppsättning beräkning och lagring resurser inom en resursgrupp och logiska server. Du ska använda exempeldata från exempeldatabasen AdventureWorks. Du måste också skapa en brandväggsregel på servernivå. toolearn hur toodo detta, besök hello [skapa en Azure SQL database i hello Azure-portalen](../sql-database/sql-database-get-started-portal.md) kursen.

  ![Skapa Azure SQL-databas](media/how-to-discover-classify-personal-data-azure/create-database.png)
- En SQL-databas kan även skapas i hello [Azure Cloud Shell](https://azure.microsoft.com/features/cloud-shell/) CLI, ett webbläsarbaserat kommandoradsverktyg. hello-verktyget är tillgängligt i hello Azure-portalen och kan köras direkt därifrån. I kursen får du starta verktyget hello, definiera skriptvariabler, skapa en resursgrupp och en logisk server och konfigurera en brandväggsregel. Sedan skapar du en databas med exempeldata. toolearn hur toocreate databasen på så sätt kan besöka hello [skapar en enda Azure SQL-databas med hello Azure CLI](../sql-database/sql-database-get-started-cli.md) kursen.

  ![CLIT självstudiekursen](media/how-to-discover-classify-personal-data-azure/cli-tutorial.png)

>[!NOTE]
Azure CLI används ofta av Linux-administratörer och utvecklare. Vissa användare vara enklare och mer intuitivt än PowerShell, vilket är ett tredje alternativ.

- Slutligen kan du skapa en SQL-databas med hjälp av PowerShell som är ett verktyg som används för toocreate för kommandoraden eller skript och hantera Azure och andra resurser. I kursen får du starta verktyget hello, definiera skriptvariabler, skapa en resursgrupp och en logisk server och konfigurera en brandväggsregel. Sedan ska du skapa en databas med exempeldata.

hello kursen kräver hello Azure PowerShell Modulversion 4.0 eller senare. Kör Get-Module - ListAvailable AzureRM toofind din version. Om du behöver tooinstall eller uppgradering finns i installera Azure PowerShell-modulen.

```PowerShell
New-AzureRmSQLDatabase -ResourceGroupName $resourcegroupname `
-ServerName $servername `
-DatabaseName $databasename `
-RequestedServiceObjectiveName "s0"
```

toolearn hur toocreate databasen på så sätt kan besöka hello [skapa en enda Azure SQL-databas med hjälp av Powershell](../sql-database/sql-database-get-started-powershell.md) kursen.

>[!Note]
Windows-administratörer tenderar toouse PowerShell, men några av dem föredrar Azure CLI.

### <a name="how-do-i-search-for-personal-data-in-sql-database-in-hello-azure-portal"></a>Hur jag söker efter personliga data i SQL-databasen i hello Azure-portalen? **

Du kan använda hello inbyggda frågan Redigeraren för inuti hello Azure portal toosearch för personliga data. Du måste logga in toohello verktyget med din SQL server-administratören inloggningsnamn och lösenord och sedan ange en fråga.

  ![Sök sql med hello-portalen](media/how-to-discover-classify-personal-data-azure/search-sql-portal.png)

Steg 5 i hello kursen visar en exempelfråga i hello query editor-fönstret, men det fokusera inte på personlig eller känslig information (det också kombinerar data från två tabeller och skapar alias för hello källkolumnen i hello datamängd som returnerades). hello följande skärmbild visar hello frågan från steg 5 samt hello resultatfönstret som returneras:

  ![frågeredigerare](media/how-to-discover-classify-personal-data-azure/query-editor.png)

Om din databas anropades mytable prefix, en exempelfråga för personlig information kan innehålla namn, personnummer och ID-nummer och skulle se ut så här:

”Välj namn, SSN, ID-nummer från mytable prefix”

Du skulle köra hello frågan och sedan visa hello resultatet i hello **resultat** fönstret.

Mer information om hur tooquery en SQL-databas i hello Azure-portalen finns hello [frågan hello SQL-databas](../sql-database/sql-database-get-started-portal.md) avsnittet av kursen hello.

### <a name="how-do-i-search-for-data-across-multiple-databases"></a>Hur jag söker efter data över flera databaser?

Elastisk SQL-fråga (förhandsversion) kan du tooperform flera databaser och flera databasfrågor och returnera ett enskilt resultat. Hej [självstudiekursen översikt](../sql-database/sql-database-elastic-query-overview.md) innehåller en detaljerad beskrivning av scenarier och förklarar hello skillnaden mellan vågräta och lodräta databasen partitioneras. Horisontell partitionering kallas ”horisontell partitionering”.

  ![Vertikal partitionering](media/how-to-discover-classify-personal-data-azure/vertical-partition.png)

  ![horisontell partitionering](media/how-to-discover-classify-personal-data-azure/horizontal.png)

tooget igång finns hello [översikt över Azure SQL Database elastisk fråga (förhandsgranskning)](../sql-database/sql-database-elastic-query-overview.md) sidan.

#### <a name="power-query-for-importing-azure-hdinsight-hadoop-clusters-data-discovery-for-large-data-sets"></a>Power Query (för att importera Azure HDInsight Hadoop-kluster): identifiering av data för stora datamängder

Hadoop är en öppen källkod Apache lagring och behandling av tjänsten för stora datamängder som analyseras och lagras i Hadoop-kluster. [Azure HDInsight](https://azure.microsoft.com/services/hdinsight/) kan användare toowork med Hadoop-kluster i Azure. Power Query är ett Excel-tillägg som bland annat hjälper användarna att identifiera data från olika källor.

Personliga data som är associerade med Hadoop-kluster i Azure HDInsight kan vara importerade tooExcel med Power Query. När hello data är i Excel kan du använda en fråga tooidentify den.

#### <a name="how-do-i-use-excel-power-query-tooimport-hadoop-clusters-in-azure-hdinsight-into-excel"></a>Hur använder Excel Power Query tooimport Hadoop-kluster i Azure HDInsight till Excel?

Ett HDInsight-kursen vägleder dig genom hela processen. Det beskrivs förutsättningar och innehåller en länk tooa [komma igång med Azure HDInsight](../hdinsight/hdinsight-hadoop-linux-tutorial-get-started.md) kursen. Anvisningar om Excel 2016 samt 2013 och 2010 (steg är något annorlunda för hello äldre versioner av Excel). Om du inte har hello Power Query för Excel-tillägget hello kursen visar hur tooget den. Du börjar hello kursen i Excel och behöver toohave ett Azure Blob storage-konto som är associerade med klustret.

  ![Frågan i Excel](media/how-to-discover-classify-personal-data-azure/excel.png)

toolearn hur toodo detta, besök hello [ansluta Excel tooHadoop med Power Query](../hdinsight/hdinsight-connect-excel-power-query.md) kursen.

Källa: [Anslut Excel tooHadoop med Power Query](../hdinsight/hdinsight-connect-excel-power-query.md)

## <a name="azure-information-protection-personal-data-classification-for-documents-and-email"></a>Azure Information Protection: personuppgifter klassificering för dokument och e-post

[Azure Information Protection](https://www.microsoft.com/cloud-platform/azure-information-protection) kan kunderna Azure gäller tooclassify etiketter och skydda internt eller externt delade dokument och e-kommunikation. Vissa av dessa element kan innehålla personlig information för kunden eller medarbetare. Regler och villkor kan definieras automatiskt eller manuellt av administratörer eller användare. Om en användare sparar ett dokument som innehåller information om kreditkort, till exempel visas han eller hon en rekommendation för etikett som konfigurerats av Hej administratör.

### <a name="how-do-i-try-it"></a>Hur försök den?

Om du vill toogive Azure Information Protection en försök toosee om kan det vara en anpassning för din organisation kan gå hello [Snabbstartsguide](https://docs.microsoft.com/information-protection/get-started/infoprotect-quick-start-tutorial). Den vägleder dig igenom fem grundläggande steg – från installationen tooconfiguring princip tooseeing klassificering, etikettering och delar i åtgärden, och bör ta mindre än en halvtimme.

### <a name="how-do-i-deploy-it"></a>Hur distribuerar jag det?

Om du vill toodeploy Azure Information Protection i din organisation kan gå hello [distributionsplan för klassificering, etikettering och skydd](https://docs.microsoft.com/information-protection/plan-design/deployment-roadmap).

### <a name="is-there-anything-else-i-should-know"></a>Finns det något annat jag veta?

Kompletterande information som hjälper dig att tänka igenom hur tooset det, finns hello [redo, ange kan skydda!](https://blogs.technet.microsoft.com/enterprisemobility/2017/02/21/azure-information-protection-ready-set-protect/)
blogginlägg. Och kontrollera hello Lär dig mer länkarna nedan för mer om Azure Information Protection.

## <a name="azure-search-data-discovery-for-developer-apps"></a>Azure Search: data identifiering för utvecklare appar

[Azure Search](https://azure.microsoft.com/services/search/) är en sökning molnlösning för utvecklare och ger en omfattande data sökinställningar för dina program. Azure Search kan du toolocate data över användardefinierade index, kommer från Azure Cosmo DB, Azure SQL Database, Azure Blob Storage, Azure Table storage eller anpassade kunden JSON-data. Du kan också struktur Lucene frågor med hello Azure Search REST API toosearch för personliga datatyper och hello personliga data för specifika personer. Funktioner är fulltextsökning och enkel frågesyntaxen Lucene frågesyntaxen. 

## <a name="how-do-i-use-sql-tooquery-data"></a>Hur använder SQL tooquery data?

toobegin med hello grunderna finns hello [Azure CosmosD DB: hur tooquery med hjälp av SQL](../cosmos-db/tutorial-query-documentdb.md) kursen. hello självstudierna innehåller ett exempel på dokument och två exempel SQL-frågor och resultat.

Mer detaljerad vägledning om hur du skapar SQL-frågor finns [SQL-frågor för Cosmos-dokumentet på Azure DB DB API: et.](../cosmos-db/documentdb-sql-query.md)

Om du är ny tooAzure Cosmos DB och skulle som toolearn hur toocreate en databas, lägga till en samling och lägga till data, besök hello [Azure Cosmos DB: skapa en webbapp med DocumentDB API](../cosmos-db/create-documentdb-dotnet.md) Snabbstartsguide. Om du vill toodo detta i ett annat språk än .NET, till exempel Java eller Python, Välj ditt språk när du har hämtat toohello plats.

## <a name="next-steps"></a>Nästa steg

[Azure SQL Database](https://azure.microsoft.com/services/sql-database/?v=16.50)

[Vad är SQL Database?](../sql-database/sql-database-technical-overview.md)

[SQL Database Query Editor finns i Azure-portalen] (https://azure.microsoft.com/blog/t-sql-query-editor-in-browser-azure-portal/)

[Vad är Azure Information Protection?](https://docs.microsoft.com/information-protection/understand-explore/what-is-information-protection)

[Vad är Azure Rights Management?](https://docs.microsoft.com/information-protection/understand-explore/what-is-azure-rms)

[Azure Information Protection: Klar, ange, skydda!](https://blogs.technet.microsoft.com/enterprisemobility/2017/02/21/azure-information-protection-ready-set-protect/)
