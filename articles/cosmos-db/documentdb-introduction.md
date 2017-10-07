---
title: "DocumentDB-API:et för Azure Cosmos DB | Microsoft Docs"
description: "Lär dig hur du kan använda Azure Cosmos DB toostore och fråga massiva mängder JSON-dokument med låg latens med hjälp av SQL- och JavaScript."
keywords: json-databas, dokumentdatabas
services: cosmos-db
author: mimig1
manager: jhubbard
editor: monicar
documentationcenter: 
ms.assetid: 686cdd2b-704a-4488-921e-8eefb70d5c63
ms.service: cosmos-db
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 05/22/2017
ms.author: mimig
ms.openlocfilehash: c96dfa5e2685782a99d2bcaf992f88dd2bef920b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="introduction-tooazure-cosmos-db-documentdb-api"></a>Introduktion tooAzure Cosmos DB: DocumentDB-API

[Azure Cosmos DB](introduction.md) är Microsofts globalt distribuerade databastjänst för flera datamodeller för verksamhetskritiska program. Azure Cosmos-DB tillhandahåller [nyckelfärdig global distributionsplatsen](distribute-data-globally.md), [elastisk skalbarhet av dataflöden och lagringsutrymmen](partition-data.md) över hela världen, en siffra millisekunders latens vid hello 99th percentilen, [fem väldefinierade konsekvensnivåer](consistency-levels.md), och garanteras hög tillgänglighet, alla säkerhetskopieras av [branschledande serviceavtal](https://azure.microsoft.com/support/legal/sla/cosmos-db/). Azure Cosmos-DB [indexerar automatiskt data](http://www.vldb.org/pvldb/vol8/p1668-shukla.pdf) utan att du toodeal med hantering av schemat och index. Det stöder flera modeller och dokument, nyckelvärde graf och kolumndatamodeller. 

![Azure DocumentDB-API:et](./media/documentdb-introduction/cosmosdb-documentdb.png) 

Med hello DocumentDB API, Azure Cosmos DB tillhandahåller omfattande och välbekanta [SQL frågefunktioner](documentdb-sql-query.md) med genomgående korta svarstiderna över schemat mindre JSON-data. Vi ger en översikt över hello Azure Cosmos DB'S DocumentDB API och hur du kan använda den toostore massiva mängder JSON-data, fråga dem i ordning millisekunder svarstid och enkelt utvecklas hello schema i den här artikeln. 

## <a name="what-capabilities-and-key-features-does-azure-cosmos-db-offer"></a>Vilka är de viktigaste funktionerna i Azure Cosmos DB?
Azure Cosmos DB via hello DocumentDB API ger hello följande viktiga funktioner och fördelar:

* **Elastiska och skalbara dataflöden och lagring:** enkelt skala upp eller ned din JSON-databas toomeet programmet behöver. Dina data lagras på SSD-diskar (Solid State Disk) för korta och förutsägbara svarstider. Azure Cosmos-DB stöder behållare för JSON-data lagras anropade samlingar som kan skalas toovirtually obegränsade lagringsstorlekar och dataflöde. Du kan skala Azure Cosmos DB elastiskt och smidigt med förutsägbara prestanda allteftersom programmet växer. 


* **Flera regioner replikering:** Azure Cosmos DB replikerar transparent tooall dataområden som du har kopplat till ditt Azure DB som Cosmos-konto så att du toodevelop program som kräver global åtkomst toodata samtidigt som din kompromisser mellan konsekvens, tillgänglighet och prestanda, alla med motsvarande garantier. Azure Cosmos-DB ger transparent regional växling vid fel med flera API: er och hello möjlighet tooelastically skalbara dataflöden och lagringsutrymmen över hello jordglob. Mer information finns i [Distribute data globally with Azure Cosmos DB](distribute-data-globally.md) (Distribuera data globalt med Azure Cosmos DB).

* **Ad hoc-frågor med välbekant SQL-syntax:** Lagra heterogena JSON-dokument och skicka frågor mot dokumenten med hjälp av en välbekant SQL-syntax. Azure Cosmos-DB använder en samtidig, låsfri, loggstrukturerad indexering teknik tooautomatically index allt dokumentinnehåll. Detta gör omfattande förfrågningar i realtid utan hello måste toospecify schematips, sekundärindex eller vyer. Mer information finns i [Query Azure Cosmos DB](documentdb-sql-query.md) (Skicka frågor mot Azure Cosmos DB). 
* **JavaScript-körning inom hello databasen:** uttryck programlogik som lagrade procedurer, utlösare och användardefinierade funktioner (UDF) med standard JavaScript. Detta gör dina program logik toooperate över data utan att oroa hello matchningsfel mellan hello program och hello databasschemat. Hej DocumentDB API ger fullständig transaktionell körning av JavaScript-programlogik direkt i databasmotorn hello. hello djupgående integration av JavaScript aktiverar hello körning av Infoga, Ersätt, ta bort och välj köras från ett JavaScript-program som en isolerad transaktion. Mer information finns i [Programmering av serversidan i DocumentDB](programming.md).

* **Justerbara konsekvensnivåer:** väljer från fem väldefinierade konsekvenskontroll nivåer tooachieve optimal kompromiss mellan konsekvens och prestanda. Azure Cosmos DB erbjuder fem olika konsekvensnivåer för frågor och läsåtgärder: stark, bunden utgång, session, enhetligt prefix och slutlig. Dessa detaljerade, väldefinierade konsekvensnivåerna kan toomake själv avgöra balansen mellan konsekvens, tillgänglighet och svarstid. Läs mer i [med konsekvenskontroll nivåer toomaximize tillgänglighet och prestanda](consistency-levels.md).

* **Fullständigt hanterade:** eliminera hello måste toomanage databasen och datorresurser. Som en fullständigt hanterad Microsoft Azure-tjänst, gör inte behöver toomanage virtuella datorer, distribuera och konfigurera programvara, hantera skalning eller hantera komplexa datanivå uppgraderingar. Alla databaser säkerhetskopieras och skyddas automatiskt mot regionala fel. Du kan enkelt lägga till ett Azure DB som Cosmos-konto och etablera kapacitet när du behöver den, så att du toofocus för ditt program i stället för att använda och hantera din databas. 

* **Öppen design:** Kom igång snabbt med hjälp av befintliga kunskaper och verktyg. Programmera mot hello DocumentDB API är enkelt och användarvänligt, och inte kräver tooadopt nya verktyg eller följer toocustom tillägg tooJSON eller JavaScript. Du kan komma åt alla hello databasfunktioner inklusive CRUD-, fråge- och JavaScript-bearbetning, över ett enkelt RESTful HTTP-gränssnitt. hello API DocumentDB omfattar befintliga format, språk och standarder och erbjuder värdefulla databasen funktioner utöver dem.

* **Automatisk indexering:** som standard, Azure Cosmos DB automatiskt indexerar alla hello dokument i hello-databasen och inte förväntar sig eller kräver något schema eller att sekundärindex. Inte vill tooindex allt? Oroa dig inte, du kan även [välja bort sökvägar i JSON-filer](indexing-policies.md).

* **Ändra feed stöd:** ändra feed ger en sorterad lista över dokument inom en samling Azure Cosmos DB i hello ordning de ändrades. Denna feed kan använda toolisten för ändringar toodata i ordning tooreplicate data, utlöser API-anrop eller utföra dataströmmen bearbetning av uppdateringar. Ändra feed är aktiverad och automatiskt enkelt toouse: [Lär dig mer om att ändra feed](https://docs.microsoft.com/azure/cosmos-db/change-feed). 

## <a name="data-management"></a>Hur hanterar du data med hello API DocumentDB?
Hej DocumentDB API hjälper dig att hantera JSON-data via väldefinierade databasresurser. Dessa resurser replikeras för hög tillgänglighet och är unikt adresserbara genom sina logiska URI:er. Hej DocumentDB API erbjuder en enkel HTTP-baserad RESTful-programmeringsmiljö för alla resurser. 


hello Azure DB som Cosmos-databaskontot är ett unikt namnområde som ger dig åtkomst till tooAzure Cosmos DB. Innan du kan skapa ett databaskonto, du måste ha en Azure-prenumeration som ger dig åtkomst till tooa olika Azure-tjänster. 

Alla resurser i Azure Cosmos DB modelleras och lagras som JSON-dokument. Resurser hanteras som objekt, som är JSON-dokument med metadata, och som flöden, som är samlingar av objekt. Objektuppsättningar ingår i respektive flöde.

hello bilden nedan visar hello relationerna mellan hello Azure DB som Cosmos-resurser:

![hello hierarkiska relationen mellan resurser i Azure Cosmos DB][1] 

Ett databaskonto består av en uppsättning databaser som alla innehåller flera samlingar, som i sin tur kan innehålla lagrade procedurer, utlösare, UDF:er, dokument och relaterade bilagor. En databas har också associerade användare med en uppsättning behörigheter tooaccess olika andra samlingar, lagrade procedurer, utlösare, UDF: er, dokument eller bilagor. Databaser, användare, behörigheter och samlingar är systemdefinierade resurser med välkända scheman, men dokument, lagrade procedurer, utlösare, UDF:er och bilagor innehåller godtyckligt användardefinierat JSON-innehåll.  

> [!NOTE]
> Eftersom det fanns tidigare hello DocumentDB API som hello Azure DocumentDB-tjänsten, kan du fortsätta tooprovision, övervaka och hantera konton som skapats via hello REST-API för Azure-hantering eller med hjälp av antingen hello Azure DocumentDB eller Azure Cosmos DB resursnamn. Vi använder synonymt hello namn när det gäller toohello Azure DocumentDB APIs. 

## <a name="develop"></a>Hur kan jag utveckla appar med hello API DocumentDB?

Azure Cosmos-DB visar resurser via hello REST API: er som kan anropas med valfritt språk som kan göra HTTP/HTTPS-förfrågningar. Vi erbjuder dessutom programmeringsbibliotek för flera populära språk för hello DocumentDB-API. hello klientbibliotek förenklar många aspekter av arbetet med hello API genom att hantera information om till exempel cachelagring av adresser, undantagshantering, automatiska omförsök och så vidare. Bibliotek är tillgängliga för hello följande språk och plattformar:  

| Ladda ned | Dokumentation |
| --- | --- |
| [.NET SDK](http://go.microsoft.com/fwlink/?LinkID=402989) |[.NET-bibliotek](/dotnet/api/overview/azure/cosmosdb?view=azure-dotnet) |
| [Node.js SDK](http://go.microsoft.com/fwlink/?LinkID=402990) |[Node.js-bibliotek](http://azure.github.io/azure-documentdb-node/) |
| [Java SDK](http://go.microsoft.com/fwlink/?LinkID=402380) |[Java-bibliotek](/java/api/com.microsoft.azure.documentdb) |
| [JavaScript SDK](http://go.microsoft.com/fwlink/?LinkID=402991) |[JavaScript-bibliotek](http://azure.github.io/azure-documentdb-js/) |
| Saknas |[JavaScript SDK för serversidan](http://azure.github.io/azure-documentdb-js-server/) |
| [Python SDK](https://pypi.python.org/pypi/pydocumentdb) |[Python-bibliotek](http://azure.github.io/azure-documentdb-python/) |
| Saknas | [API för MongoDB](mongodb-introduction.md)


Med hjälp av hello [Azure Cosmos DB emulatorn](local-emulator.md), du kan utveckla och testa programmet lokalt med hello DocumentDB API, utan att skapa en Azure-prenumeration eller kostnader. När du är nöjd med hur programmet fungerar i hello-emulatorn kan växla du toousing ett Azure DB som Cosmos-konto i hello molnet.

Utöver basic skapa, läsa, uppdatera och ta bort hello DocumentDB API innehåller en omfattande SQL-gränssnitt för att hämta JSON-dokument och server-sida-stöd för transaktionell körning av JavaScript-programlogik. hello fråge- och körningen gränssnitt är tillgängliga via alla plattformsbibliotek samt hello REST API: er. 

### <a name="sql-query"></a>SQL-fråga
Hej DocumentDB API stöder frågar dokument med hjälp av en SQL-språk, som finns i hello JavaScript skriver system och uttryck med stöd för relationella hierarkiska och spatial frågor. Hej DocumentDB-frågespråket är ett enkelt men kraftfullt gränssnitt tooquery JSON-dokument. hello språk stöder en delmängd av ANSI SQL-grammatiken och dessutom djupgående integration av JavaScript-objekt, matriser, objektkonstruktion och funktionsanrop. Hej DocumentDB API ger frågemodellen utan uttryckliga scheman eller indexeringstips från hello-utvecklare.

Användardefinierade funktioner (UDF) kan registreras med hello DocumentDB-API och refereras till som en del av en SQL-fråga, vilket innebär att hello grammatik toosupport anpassad programlogik. Dessa UDF: er skrivs som JavaScript-program och köras inom hello-databasen. 

Hej DocumentDB API för .NET-utvecklare [.NET SDK](https://msdn.microsoft.com/library/azure/microsoft.azure.documents.linq.aspx) erbjuder även en LINQ-frågeleverantör. 

### <a name="transactions-and-javascript-execution"></a>Transaktioner och JavaScript-körning
Hej DocumentDB API kan du toowrite programlogiken som namngivna program helt skrivna i JavaScript. Programmen registreras för en samling och kan utfärda databasåtgärder i hello dokument inom en viss samling. JavaScript kan registreras för körning som en utlösare, lagrad procedur eller användardefinierad funktion. Utlösare och lagrade procedurer kan skapa, läsa, uppdatera och ta bort dokument medan användardefinierade funktioner körs som en del av hello frågans körningslogik utan skrivåtkomst toohello samling.

JavaScript-körning inom hello Cosmos DB modelleras efter begrepp som hello stöds av relationsdatabassystem, med JavaScript som en modern ersättning för Transact-SQL. All JavaScript-logik körs inom en omgivande ACID-transaktion med ögonblicksbildisolering. Under hello att körningen avbryts om JavaScript genererar ett undantag hello hello sedan hela transaktionen.

## <a name="are-there-any-online-courses-on-azure-cosmos-db"></a>Finns det några onlinekurser om Azure Cosmos DB?

Ja, det finns en kurs om [Microsoft Virtual Academy](https://mva.microsoft.com/en-US/training-courses/azure-documentdb-planetscale-nosql-16847) på Azure DocumentDB. 

>[!VIDEO https://mva.microsoft.com/en-US/training-courses-embed/azure-documentdb-planetscale-nosql-16847]
>
>

## <a name="next-steps"></a>Nästa steg
Har du redan ett Azure-konto? Sedan kan du sätta igång med Azure Cosmos DB genom att följa våra [snabbstarter](../cosmos-db/create-documentdb-dotnet.md), som hjälper dig att skapa ett konto och att komma igång med Cosmos DB.

[1]: ./media/documentdb-introduction/json-database-resources1.png

