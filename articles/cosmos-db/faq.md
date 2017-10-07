---
title: "aaaAzure Cosmos DB vanliga frågor och svar | Microsoft Docs"
description: "Få svar toofrequently frågor och svar om Azure Cosmos DB, ett globalt distribuerade och flera olika modeller database-tjänsten. Läs mer om kapacitet, prestandanivåer och skalning."
keywords: "Databasfrågor, vanliga frågor, documentdb, azure, Microsoft azure"
services: cosmos-db
author: mimig1
manager: jhubbard
editor: monicar
documentationcenter: 
ms.assetid: b68d1831-35f9-443d-a0ac-dad0c89f245b
ms.service: cosmos-db
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/31/2017
ms.author: mimig
ms.openlocfilehash: 59e047d9acd8ac05facc96655747d7495a45317a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-cosmos-db-faq"></a>Vanliga frågor om Azure Cosmos DB
## <a name="azure-cosmos-db-fundamentals"></a>Azure DB Cosmos-grunderna
### <a name="what-is-azure-cosmos-db"></a>Vad är Azure Cosmos DB?
Azure Cosmos-databas är en tjänst för flera modell globalt replikerad databas som som erbjuder många frågealternativ över schemafria data, hjälper till att leverera konfigurerbar och tillförlitlig prestanda och möjliggör snabb utveckling. Det är alla uppnås genom en hanterad plattform som backas upp av hello kraften och räckvidden hos Microsoft Azure. 

Azure Cosmos-DB är hello rätta lösningen för webb-, mobil-, spel- och IoT-appar när förutsägbart dataflöde, hög tillgänglighet, låg latens och en schemafri datamodell är viktiga krav. Det ger schemaflexibilitet och omfattande indexering och innehåller transaktionellt stöd för flera dokument med integrerat JavaScript. 

För mer databasfrågor, svar och instruktioner för att distribuera och använda den här tjänsten finns hello [dokumentationssidan för Azure Cosmos DB](https://azure.microsoft.com/documentation/services/cosmos-db/).

### <a name="what-happened-toodocumentdb"></a>Vilka happened tooDocumentDB?
Hej DocumentDB API är en av hello stöds API: er och datamodeller för Azure Cosmos DB. Dessutom stöder Azure Cosmos DB du med Graph API (förhandsgranskning), tabell-API (förhandsversion) och MongoDB-API. Mer information finns i [frågor från DocumentDB kunder](#moving-to-cosmos-db).

### <a name="how-do-i-get-toomy-documentdb-account-in-hello-azure-portal"></a>Hur skaffar jag toomy DocumentDB-konto i hello Azure-portalen
Klicka på hello Azure Cosmos DB-ikonen i hello till vänster i hello Azure-portalen. Om du har ett DocumentDB-konto innan har du nu ett Azure DB som Cosmos-konto med ingen ändring tooyour fakturering.

### <a name="what-are-hello-typical-use-cases-for-azure-cosmos-db"></a>Vad är hello vanliga användningsområden för Azure Cosmos DB?
Azure Cosmos-DB är ett bra alternativ för nya webb-, mobil-, spel- och IoT-appar där automatisk skalning, förutsägbar prestanda, snabba ordning med svarstider på millisekunder och hello möjlighet tooquery över schemafria data är viktigt. Azure Cosmos-DB lämpar sig toorapid utveckling och stöder hello kontinuerlig iteration av appens datamodeller. Program som hanterar innehåll som skapats av användare och data är [vanliga användningsområden för Azure Cosmos DB](use-cases.md). 

### <a name="how-does-azure-cosmos-db-offer-predictable-performance"></a>Hur erbjuder Azure Cosmos DB förutsägbar prestanda?
En [begäran enhet](request-units.md) (RU) är hello mått på dataflödet i Azure Cosmos DB. En genomströmning 1 RU motsvarar toohello genomflödet i hello GET av ett dokument på 1 KB. Varje åtgärd i Azure Cosmos DB, inklusive läsning, skrivning, SQL-frågor och lagrade procedurkörningar har en deterministisk RU-värde som baseras på hello dataflödet som krävs för toocomplete hello åtgärden. I stället för att fundera på CPU, IO, och minne och hur de påverkar appens dataflöde tänka du på ett enda RU mått.

Du kan reservera varje Azure DB som Cosmos-behållare med etablerat dataflöde räknat RUs genomströmning per sekund. För appar oavsett skala, kan du mäta enskilda förfrågningar toomeasure deras RU-värden och etablera behållaren toohandle hello totalt frågeenheter över alla förfrågningar. Du kan även skala upp eller ned din behållaren genomströmning som hello behov utvecklas. Mer information om frågeenheter och hjälp att fastställa din behållaren måste, se [uppskatta behov för genomströmning](request-units.md#estimating-throughput-needs) och försök hello [genomströmning Kalkylatorn](https://www.documentdb.com/capacityplanner). hello termen *behållare* här refererar toorefers tooa DocumentDB API insamling, Graph API diagram, MongoDB API-samling och tabellen API-tabellen. 

### <a name="how-does-azure-cosmos-db-support-various-data-models-such-as-keyvalue-columnar-document-and-graph"></a>Hur stöder Azure Cosmos DB olika datamodeller som nyckel/värde, kolumner, dokument och diagrammet?

Nyckel/värde (tabellen)-kolumner, dokumentera och diagramdata modeller är alla inbyggt stöd på grund av hello ARS (atomer, poster och sekvenser) design som Azure Cosmos DB bygger på. Atomer, poster och sekvenser kan vara enkelt mappade och planerade toovarious datamodeller. hello API: er för en delmängd av modeller är tillgängliga just nu (DocumentDB, MongoDB, tabell och Graph API: er) och andra specifika tooadditional datamodeller blir tillgängliga i hello framtiden.

Azure Cosmos-DB har ett schema oberoende indexering motor kan indexera automatiskt alla hello data den en utan att något schema eller sekundärindex från hello-utvecklare. hello-motorn är beroende av en uppsättning logiska index layouter (inverterad, kolumner, träd) som frikopplar hello lagringslayout från hello index och frågebearbetning undersystem. Cosmos DB även har hello möjlighet toosupport en överföring protokoll och API: er på ett utökningsbart sätt och översätta dem effektivt toohello core modellen (1) och hello logiska index layouter (2) vilket gör det unikt kan stödja flera datamodeller internt .

### <a name="is-azure-cosmos-db-hipaa-compliant"></a>Är Azure Cosmos DB HIPAA kompatibel?
Ja, Azure Cosmos DB är HIPAA-kompatibelt. HIPAA fastställer kraven för hello använder, avslöjande av och skydd av individuellt identifierbar hälsoinformation. Mer information finns i hello [Microsoft Trust Center](https://www.microsoft.com/en-us/TrustCenter/Compliance/HIPAA).

### <a name="what-are-hello-storage-limits-of-azure-cosmos-db"></a>Vad är Azure Cosmos DB hello Lagringsgränser?
Det finns ingen gräns toohello totala mängden data som en behållare kan lagra i Azure Cosmos DB.

### <a name="what-are-hello-throughput-limits-of-azure-cosmos-db"></a>Vad är hello genomströmning gränserna för Azure Cosmos DB?
Det finns ingen gräns toohello totala mängden dataflödet som en behållare kan stödja i Azure Cosmos DB. Hej viktiga idé är toodistribute din arbetsbelastning ungefär jämnt mellan ett tillräckligt stort antal partitionsnycklar.

### <a name="how-much-does-azure-cosmos-db-cost"></a>Hur mycket kostar Azure Cosmos DB?
Mer information finns i toohello [Azure Cosmos DB prisinformation](https://azure.microsoft.com/pricing/details/cosmos-db/) sidan. Avgifterna för användning av Azure DB Cosmos bestäms av hello antalet etablerade behållare, hello antal timmar hello behållare var online och hello etableras genomflödet för varje behållare. hello termen *behållare* här refererar toohello DocumentDB API insamling, Graph API diagram, MongoDB API-samling och tabellen API-tabeller. 

### <a name="is-a-free-account-available"></a>Finns det ett kostnadsfritt konto?
Om du är ny tooAzure kan du registrera dig för en [kostnadsfritt Azure-konto](https://azure.microsoft.com/free/), som ger dig 30 dagar och och en kredit tootry alla hello Azure-tjänster. Om du har en prenumeration på Visual Studio kan du också är berättigad till [kostnadsfria Azure-krediter](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) toouse i Azure-tjänster. 

Du kan också använda hello [Azure Cosmos DB emulatorn](local-emulator.md) toodevelop och testa programmet lokalt för kostnadsfri, utan att skapa en Azure-prenumeration. När du är nöjd med hur programmet fungerar i hello Azure Cosmos DB-emulatorn kan växla du toousing ett Azure DB som Cosmos-konto i hello molnet.

### <a name="how-can-i-get-additional-help-with-azure-cosmos-db"></a>Hur kan jag få ytterligare hjälp med Azure Cosmos DB?
Om du behöver hjälp med något kan nå ut toous på [Stack Overflow](http://stackoverflow.com/questions/tagged/azure-cosmosdb) eller hello [MSDN-forum](https://social.msdn.microsoft.com/forums/azure/en-US/home?forum=AzureDocumentDB), eller schemalägga en one-on-one chatt med teknikteamet för hello Azure Cosmos DB genom att skicka e-post för[ askcosmosdb@microsoft.com](mailto:askcosmosdb@microsoft.com). 

## <a name="set-up-azure-cosmos-db"></a>Konfigurera Azure Cosmos DB
### <a name="how-do-i-sign-up-for-azure-cosmos-db"></a>Hur loggar jag för Azure Cosmos DB?
Azure Cosmos-databasen är tillgänglig i hello Azure-portalen. Först registrera dig för en Azure-prenumeration. När du har registrerat dig kan du lägga till ett DocumentDB-API, Graph API (förhandsgranskning), tabell-API (förhandsgranskning) eller MongoDB API kontot tooyour Azure-prenumeration.

### <a name="what-is-a-master-key"></a>Vad är en huvudnyckel?
En huvudnyckel är en token tooaccess säkerhet alla resurser för ett konto. Personer med hello nyckeln har läs- och skrivbehörighet tooall resurser i databaskontot hello. Var försiktig när du distribuerar huvudnycklar. hello master primärnyckel och sekundärnyckel master är tillgängliga på hello **nycklar** bladet för hello [Azure-portalen][azure-portal]. Mer information om nycklar finns i [Visa, kopiera och generera åtkomstnycklar på nytt](manage-account.md#keys).

### <a name="what-are-hello-regions-that-preferredlocations-can-be-set-to"></a>Vad är hello regioner som PreferredLocations kan anges till? 
Hej PreferredLocations värdet kan ställas in tooany av hello Azure regioner där Cosmos-databasen är tillgänglig. En lista över tillgängliga regioner, se [Azure-regioner](https://azure.microsoft.com/regions/).

### <a name="is-there-anything-i-should-be-aware-of-when-distributing-data-across-hello-world-via-hello-azure-datacenters"></a>Finns det något jag bör vara medveten om när du distribuerar data över hello world via hello Azure-Datacenter? 
Azure Cosmos-DB finns över alla Azure-regioner, som anges på hello [Azure-regioner](https://azure.microsoft.com/regions/) sidan. Eftersom den är hello kärntjänsten har varje ny datacenter en Azure Cosmos DB förekomst. 

Kom ihåg att Azure Cosmos DB respekterar statliga och offentliga moln när du ställer in en region. Som är om du skapar ett konto i en suveräna region, kan du replikera utanför suveräna regionen. På samma sätt kan du aktivera replikering till andra suveräna platser från ett utanför konto. 

## <a name="develop-against-hello-documentdb-api"></a>Utveckla mot hello DocumentDB-API

### <a name="how-do-i-start-developing-against-hello-documentdb-api"></a>Hur börjar jag utveckla mot hello API DocumentDB?
Microsoft DocumentDB-API finns i hello [Azure-portalen][azure-portal]. Du måste först registrera dig för en Azure-prenumeration. När du registrerar dig för en Azure-prenumeration kan du lägga till DocumentDB API behållaren tooyour Azure-prenumeration. Anvisningar för att lägga till ett konto i Azure Cosmos DB finns [skapa ett databaskonto Azure Cosmos DB](create-documentdb-dotnet.md#create-account). Om du har ett DocumentDB-konto i hello tidigare, nu har du ett konto i Azure Cosmos DB. 

[SDK:er](documentdb-sdk-dotnet.md) är tillgängliga för .NET, Python, Node.js, JavaScript och Java. Utvecklare kan också använda hello [RESTful http-API: er](/rest/api/documentdb/) toointeract med Azure Cosmos DB resurser från olika plattformar och språk.

### <a name="can-i-access-some-ready-made-samples-tooget-a-head-start"></a>Kan jag använda vissa färdiga prover tooget igång?
Exempel för hello DocumentDB API [.NET](documentdb-dotnet-samples.md), [Java](https://github.com/Azure/azure-documentdb-java), [Node.js](documentdb-nodejs-samples.md), och [Python](documentdb-python-samples.md) SDK finns på GitHub.


### <a name="does-hello-documentdb-api-database-support-schema-free-data"></a>Stöder hello DocumentDB API databasen stöd för schemafria data?
Ja, hello DocumentDB API kan program toostore godtyckliga JSON-dokument utan schemadefinitioner eller tips. Data är omedelbart tillgängligt för frågor via hello Azure Cosmos-Databasens SQL-gränssnitt.  

### <a name="does-hello-documentdb-api-support-acid-transactions"></a>Stöder hello API DocumentDB ACID-transaktioner?
Ja, hello API DocumentDB stöder transaktioner mellan dokument uttryckta som JavaScript-lagrade procedurer och utlösare. Transaktioner är begränsad tooa enda partition inom varje samling och utförs med ACID-semantik som ”allt eller inget”, isoleras från andra samtidigt köra kod och begäranden. Om undantag utlöses via hello serversidan körning av JavaScript, återställs hello hela transaktionen. Mer information om transaktioner finns [databasen transaktioner automatiskt](programming.md#database-program-transactions).

### <a name="what-is-a-collection"></a>Vad är en samling?
En samling är en grupp av dokument och deras associerade JavaScript-programlogik. En samling är en fakturerbar enhet där hello [kostnaden](performance-levels.md) bestäms av hello dataflöde och använt lagring. Samlingar kan omfatta en eller flera partitioner eller servrar och kan skalas toohandle praktiskt taget obegränsade volymer av lagring eller dataflöde.

Samlingar är också hello faktureringsenheterna för Azure Cosmos DB. Varje samling faktureras timvis, baserat på hello etablerat dataflöde och använt lagringsutrymme. Mer information finns i [priser för Azure Cosmos DB](https://azure.microsoft.com/pricing/details/cosmos-db/). 

### <a name="how-do-i-create-a-database"></a>Hur skapar jag en databas?
Du kan skapa databaser med hjälp av hello [Azure-portalen](https://portal.azure.com), enligt beskrivningen i [lägger till en samling](create-documentdb-dotnet.md#create-collection), en hello [Azure Cosmos DB SDK](documentdb-sdk-dotnet.md), eller hello [REST API: er](/rest/api/documentdb/). 

### <a name="how-do-i-set-up-users-and-permissions"></a>Hur ställer jag in användare och behörigheter?
Du kan skapa användare och behörigheter genom att använda en hello [Cosmos DB API SDK](documentdb-sdk-dotnet.md) eller hello [REST API: er](/rest/api/documentdb/).  

### <a name="does-hello-documentdb-api-support-sql"></a>Stöder hello API DocumentDB SQL?
hello SQL-frågespråket är en förbättrad underuppsättning av hello frågefunktioner som stöds av SQL. hello Azure Cosmos-Databasens SQL-frågespråket ger omfattande hierarkiska och relationella operatorer och kan utvidgas via JavaScript-baserade, användardefinierade funktioner (UDF). JSON-grammatik gör det möjligt för att modellera JSON-dokument som träd med märkt noder som används av både hello Azure Cosmos DB automatisk indexering tekniker och hello SQL-frågedialekt Azure Cosmos DB. Information om hur du använder SQL-grammatik finns hello [QueryDocumentDB] [ query] artikel.

### <a name="does-hello-documentdb-api-support-sql-aggregation-functions"></a>Stöder hello DocumentDB API-stöd SQL aggregeringsfunktioner?
Hej DocumentDB API stöder låg latens sammanställning skaländras via mängdfunktioner `COUNT`, `MIN`, `MAX`, `AVG`, och `SUM` via hello SQL-grammatik. Mer information finns i [mängdfunktionerna](documentdb-sql-query.md#Aggregates).

### <a name="how-does-hello-documentdb-api-provide-concurrency"></a>Hur tillhandahåller hello API DocumentDB samtidighet?
hello API DocumentDB stöder optimistisk samtidighetskontroll (OCC) via HTTP-entitetstaggar eller ETags. Varje DocumentDB-API-resurs har en ETag och hello ETag är inställd på hello server varje gång ett dokument har uppdaterats. hello ETag-sidhuvud och det aktuella värdet för hello ingår i alla svarsmeddelanden. ETags kan användas med hello If-Match-huvud tooallow hello server toodecide om en resurs som ska uppdateras. hello If-Match-värdet är hello ETag-värde toobe kontrolleras mot. Om hello ETag-värde matchar hello server ETag-värde, uppdateras hello resurs. Om du inte längre kan aktuella hello ETag avvisar hello servern hello igen med ett ”HTTP 412 Förutsättningsfel” svarskoden. hello-klienten hämtar sedan nytt hello resurs tooacquire hello aktuella ETag-värde för hello resurs. ETags kan dessutom användas med hello If-None-Match-sidhuvudet toodetermine om det behövs för att hämta en resurs.

toouse Optimistisk samtidighet i .NET, Använd hello [AccessCondition](https://msdn.microsoft.com/library/azure/microsoft.azure.documents.client.accesscondition.aspx) klass. Ett .NET-exempel finns [Program.cs](https://github.com/Azure/azure-documentdb-dotnet/blob/master/samples/code-samples/DocumentManagement/Program.cs) i hello DocumentManagement prov på GitHub.

### <a name="how-do-i-perform-transactions-in-hello-documentdb-api"></a>Hur gör jag transaktioner i hello DocumentDB-API
hello API DocumentDB stöder språkintegrerade transaktioner via JavaScript-lagrade procedurer och utlösare. Alla databasåtgärder i skript körs under ögonblicksbildisolering. Om det är en enskild partition samling är körningen hello begränsade toohello samling. Om hello samlingen är partitionerad hello körningen är begränsade toodocuments med hello samma partitionsnyckel värde i hello samling. En ögonblicksbild av hello dokumentversionerna (ETags) tas hello början av hello transaktionen och verkställs endast om hello skriptet lyckas. Om hello JavaScript genererar ett fel återställs hello transaktionen. Mer information finns i [programmering av serversidan JavaScript för Azure Cosmos DB](programming.md).

### <a name="how-can-i-bulk-insert-documents-into-cosmos-db"></a>Hur kan jag-massinfogning dokument i Cosmos DB?
Du kan-massinfogning dokument i Azure Cosmos DB på två sätt:

* Hej datamigreringsverktyget som beskrivs i [verktyg för Azure Cosmos DB](import-data.md).
* Lagrade procedurer som beskrivs i [programmering av serversidan JavaScript för Azure Cosmos DB](programming.md).

### <a name="does-hello-documentdb-api-support-resource-link-caching"></a>Stöder hello DocumentDB API cachelagring av resurslänkar?
Ja, eftersom Azure Cosmos DB är en RESTful-tjänst, resurslänkar är oföränderlig och kan cachelagras. DocumentDB-API-klienter kan ange en ”If-None-Match”-rubrik för läsning mot ett resurs-liknande dokument eller en samling och sedan uppdatera sina lokala kopior när hello serverversionen har ändrats.

### <a name="is-a-local-instance-of-documentdb-api-available"></a>Är en lokal instans av DocumentDB API tillgängligt?
Ja. Hej [Azure Cosmos DB emulatorn](local-emulator.md) ger en hög återgivning emulering av hello Cosmos-DB-tjänsten. Den stöder funktioner som är identiska tooAzure Cosmos DB, inklusive stöd för att skapa och hämtning av JSON-dokument, etablering och skalning samlingar och köra lagrade procedurer och utlösare. Du kan utveckla och testa program med hjälp av hello Azure Cosmos DB-emulatorn och distribuera dem tooAzure på global nivå genom att göra en enda konfigurationsändring toohello Anslutningens slutpunkt för Azure Cosmos DB.

## <a name="develop-against-hello-api-for-mongodb"></a>Utveckla mot hello API för MongoDB
### <a name="what-is-hello-azure-cosmos-db-api-for-mongodb"></a>Vad är hello Azure Cosmos DB API för MongoDB?
hello Azure Cosmos DB API för MongoDB är ett lager för kompatibilitet som gör att program tooeasily och transparent kommunicera med hello interna Azure Cosmos DB databasmotorn via befintliga, community-stödda Apache MongoDB APIs och drivrutiner. Utvecklare kan nu använda befintliga MongoDB verktyget kedjor och kunskaper toobuild program som utnyttjar Azure Cosmos DB. Utvecklare nytta av hello unika funktionerna i Azure Cosmos DB, bland annat underhåll för automatisk indexering, säkerhetskopiering, säkerhetskopieras ekonomiskt servicenivåavtal (SLA) och så vidare.

### <a name="how-do-i-connect-toomy-api-for-mongodb-database"></a>Hur ansluter toomy API för MongoDB-databas?
Hej snabbaste sättet tooconnect toohello Azure Cosmos DB API för MongoDB är toohead över toohello [Azure-portalen](https://portal.azure.com). Gå tooyour konto och klicka sedan på hello vänstra navigeringsmenyn **Snabbstart**. Snabbstart är hello bästa sätt tooget kod kodavsnitt tooconnect tooyour databas. 

Azure Cosmos-DB tillämpar stränga säkerhetskrav och standarder. Azure DB Cosmos-konton kräver autentisering och säker kommunikation via SSL, så att toouse TLSv1.2.

Mer information finns i [ansluta tooyour API för MongoDB databasen](connect-mongodb-account.md).

### <a name="are-there-additional-error-codes-for-an-api-for-mongodb-database"></a>Finns det ytterligare felkoder för en API för MongoDB-databas?
I tillägg toohello vanliga MongoDB felkoder har hello MongoDB API sin egen specifika felkoder:


| Fel               | Kod  | Beskrivning  | Lösning  |
|---------------------|-------|--------------|-----------|
| TooManyRequests     | 16500 | hello totala antalet frågeenheter förbrukas överskred hello etablerade begäran-enhet hastighet för hello samlingen och har begränsats. | Överväg att skalning hello genomflöde i hello samling från hello Azure-portalen eller du försöker igen. |
| ExceededMemoryLimit | 16501 | Som en tjänst med flera innehavare överskred hello åtgärden hello klientens minne tilldelning. | Minska hello omfång hello åtgärden via mer restriktiva frågevillkor eller kontakta support från hello [Azure-portalen](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade). <br><br>Exempel:  *&nbsp; &nbsp; &nbsp; &nbsp;db.getCollection('users').aggregate ([<br> &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;{$match: {namn: ”Anders”}}, <br> &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;{$sort: {ålder: -1} }<br>&nbsp;&nbsp;&nbsp;&nbsp;])*) |

## <a name="develop-with-hello-table-api-preview"></a>Utveckla med hello tabell-API (förhandsgranskning)

### <a name="terms"></a>Villkor 
hello Azure Cosmos DB: tabell-API (förhandsgranskning) refererar toohello premium erbjudande med Azure Cosmos DB för tabellen support presenterades vid Build 2017. 

hello Standardtabell SDK är hello befintlig Azure Storage tabell SDK. 

### <a name="how-can-i-use-hello-new-table-api-preview-offering"></a>Hur kan jag använda hello nytt erbjudande för tabell-API (förhandsgranskning)? 
hello Azure Cosmos DB tabell API finns i hello [Azure-portalen][azure-portal]. Du måste först registrera dig för en Azure-prenumeration. När du har registrerat dig kan du lägga till en Azure Cosmos DB tabell API konto tooyour Azure-prenumeration och sedan lägga till tabeller tooyour konto. 

Under hello förhandsversionen när [SDK](../cosmos-db/table-sdk-dotnet.md) är tillgänglig för .NET, kan du sätta igång genom att slutföra hello [tabell API](../cosmos-db/create-table-dotnet.md) Snabbkurs artikel.

### <a name="do-i-need-a-new-sdk-toouse-hello-table-api-preview"></a>Måste en ny SDK toouse hello tabell-API (förhandsgranskning)? 
Ja, hello [Windows Azure Storage Premium Table (förhandsgranskning) SDK](https://www.nuget.org/packages/WindowsAzure.Storage-PremiumTable) är tillgängligt på NuGet. Mer information finns på hello [Cosmos-tabellen på Azure DB .NET API: et: hämta och viktig information](https://github.com/Microsoft/azure-docs-pr/cosmos-db/table-sdk-dotnet.md) sidan. 

### <a name="how-do-i-provide-feedback-about-hello-sdk-or-bugs"></a>Hur jag för att ge feedback om hello SDK eller buggar?
Du kan dela din feedback i något av följande sätt hello:

* [Uservoice](https://feedback.azure.com/forums/599062-azure-cosmos-db-table-api)
* [MSDN-forum](https://social.msdn.microsoft.com/forums/azure/en-US/home?forum=AzureDocumentDB)
* [StackOverflow](http://stackoverflow.com/questions/tagged/azure-cosmosdb)

### <a name="what-is-hello-connection-string-that-i-need-toouse-tooconnect-toohello-table-api-preview"></a>Vad är hello anslutningssträngen som jag behöver toouse tooconnect toohello tabell-API (förhandsgranskning)?
hello anslutningssträngen är:
```
DefaultEndpointsProtocol=https;AccountName=<AccountNamefromCosmos DB;AccountKey=<FromKeysPaneofCosmosDB>;TableEndpoint=https://<AccountNameFromDocumentDB>.documents.azure.com
```
Du kan hämta hello anslutningssträngen från hello nycklar sida i hello Azure-portalen. 

### <a name="how-do-i-override-hello-config-settings-for-hello-request-options-in-hello-new-table-api-preview"></a>Hur Åsidosätt hello konfigurationsinställningarna för hello begäran i hello ny tabell API (förhandsgranskning)?
Information om konfigurationsinställningarna finns [Azure Cosmos DB funktioner](../cosmos-db/tutorial-develop-table-dotnet.md#azure-cosmos-db-capabilities). Du kan ändra hello inställningar genom att lägga till dem tooapp.config i hello AppSettings i hello-klientprogrammet.

    <appSettings>
        <add key="TableConsistencyLevel" value="Eventual|Strong|Session|BoundedStaleness|ConsistentPrefix"/>
        <add key="TableThroughput" value="<PositiveIntegerValue"/>
        <add key="TableIndexingPolicy" value="<jsonindexdefn>"/>
        <add key="TableUseGatewayMode" value="True|False"/>
        <add key="TablePreferredLocations" value="Location1|Location2|Location3|Location4>"/>....
    </appSettings>


### <a name="are-there-any-changes-for-customers-who-are-using-hello-existing-standard-table-sdk"></a>Finns det några ändringar för kunder som använder hello befintliga Standardtabell SDK?
Ingen. Det finns inga ändringar för befintliga eller nya kunder som använder hello befintliga Standardtabell SDK. 

### <a name="how-do-i-view-table-data-that-is-stored-in-azure-cosmos-db-for-use-with-hello-table-api-review"></a>Hur visar tabelldata som lagras i Azure Cosmos-databasen för användning med hello tabell-API (granskning)? 
Du kan använda hello Azure portal toobrowse hello data. Du kan också använda hello tabell-API (förhandsgranskning)-koden eller hello verktyg som nämns i hello nästa svar. 

### <a name="which-tools-work-with-hello-table-api-preview"></a>Vilka verktyg tillsammans med hello tabell-API (förhandsgranskning)? 
Du kan använda hello äldre version av Azure Explorer (0.8.9).

Verktyg med hello flexibilitet tootake en anslutningssträng med hello har angetts tidigare kan stödja hello ny tabell-API (förhandsversion). En lista över Tabellverktyg finns på hello [Azure Storage-klientverktyg](../storage/common/storage-explorers.md) sidan. 

### <a name="do-powershell-or-azure-cli-work-with-hello-new-table-api-preview"></a>PowerShell eller Azure CLI fungerar med hello nya tabell API (förhandsgranskning)?
Vi planerar tooadd stöd för PowerShell och Azure CLI för tabell-API (förhandsversion). 

### <a name="is-hello-concurrency-on-operations-controlled"></a>Är hello samtidighet på åtgärder som styrs?
Ja, Optimistisk samtidighet tillhandahålls via hello användning av hello ETag mekanism. 

### <a name="is-hello-odata-query-model-supported-for-entities"></a>Hello OData frågemodell stöds för enheter? 
Ja, stöder hello tabell-API (förhandsgranskning) OData-fråge- och LINQ-fråga. 

### <a name="can-i-connect-toohello-standard-azure-table-and-hello-new-premium-table-api-preview-side-by-side-in-hello-same-application"></a>Kan jag ansluta toohello Azure Standardtabell och hello nya premium tabell-API (förhandsgranskning) sida vid sida i hello samma program? 
Ja, du kan ansluta genom att skapa två separata instanser av hello CloudTableClient, varje pekar tooits äger URI via hello anslutningssträngen.

### <a name="how-do-i-migrate-an-existing-azure-table-storage-application-toothis-new-offering"></a>Hur jag för att migrera en befintlig Azure Table storage programmet toothis nytt erbjudande?
tootake nytta av hello nytt tabell API erbjudande på din befintliga lagring tabelldata, kontakta [ askcosmosdb@microsoft.com ](mailto:askcosmosdb@microsoft.com). 

### <a name="what-is-hello-roadmap-for-this-service-and-when-will-you-offer-other-standard-table-api-functionality"></a>Vad är hello översikt för den här tjänsten och när du erbjuder andra standard tabell API-funktioner?
Vi planerar tooadd stöd för SAS-token, ServiceContext, statistik, Client side Encryption, Analytics och andra funktioner som vi fortsätta mot GA. Du kan ge oss feedback om [Uservoice](https://feedback.azure.com/forums/599062-azure-cosmos-db-table-api). 

### <a name="how-is-expansion-of-hello-storage-size-done-for-this-service-if-for-example-i-start-with-n-gb-of-data-and-my-data-will-grow-too1-tb-over-time"></a>Hur är expandering av hello lagringsstorlek Utfärdad för den här tjänsten om du till exempel jag börja med  *n*  GB data och Mina data kommer att växa too1 TB över tid? 
Azure Cosmos-DB är utformad tooprovide obegränsad lagring via hello användning av teckenbredden. hello-tjänsten kan övervaka och effektivt öka din lagring. 

### <a name="how-do-i-monitor-hello-table-api-preview-offering"></a>Hur övervakar hello tabell-API (förhandsgranskning) erbjudande?
Du kan använda hello tabell-API (förhandsgranskning) **mått** fönstret toomonitor begäranden och lagringskvoten. 

### <a name="how-do-i-calculate-hello-throughput-i-require"></a>Hur jag för att beräkna hello genomflöde I kräver?
Du kan använda hello kapacitet exteriörbedömning toocalculate hello TableThroughput som krävs för hello-åtgärder. Mer information finns i [uppskattning begära enheter och datalagring](https://www.documentdb.com/capacityplanner). I allmänhet kan du representera entiteten som JSON och ange hello siffror för din verksamhet. 

### <a name="can-i-use-hello-new-table-api-preview-sdk-locally-with-hello-emulator"></a>Kan jag använda hello ny tabell-API (förhandsgranskning) SDK lokalt med hello-emulatorn?
Ja, du kan använda hello tabell-API (förhandsversion) med hello lokala emulator när du använder hello nya SDK. toodownload nya emulatorn gå för[Använd hello Azure Cosmos DB-emulatorn för lokal utveckling och testning](local-emulator.md). Hej StorageConnectionString värdet i app.config måste toobe:

```
DefaultEndpointsProtocol=https;AccountName=localhost;AccountKey=C2y6yDjf5/R+ob0N8A7Cgv30VRDJIWEHLM+4QDU5DE2nQ9nDuVTqobD4b8mGGyPMbIZnqyMsEcaGQy67XIw/Jw==;TableEndpoint=https://localhost:8081`. 
```

### <a name="can-my-existing-application-work-with-hello-table-api-preview"></a>Fungerar mitt befintliga program med hello tabell-API (förhandsgranskning) 
hello ytan på hello ny tabell-API (förhandsversion) är kompatibel med hello befintliga Azure Standardtabell SDK över hello skapa, ta bort, uppdatera och fråga åtgärder i hello .NET-API. Se till att du har en radnyckel eftersom hello tabell-API (förhandsgranskning) kräver både en partition och en nyckel för raden. Vi planerar också tooadd mer SDK-stöd när vi går vidare mot GA för det här tjänsterbjudandet.

### <a name="do-i-need-toomigrate-my-existing-azure-table-based-applications-toohello-new-sdk-if-i-do-not-want-toouse-hello-table-api-preview-features"></a>Behöver jag toomigrate Mina befintliga Azure table-baserade program toohello nya SDK om jag inte vill toouse hello tabell-API (förhandsgranskning)-funktioner?
Nej, kan du skapa och använda den befintliga Standardtabell tillgångar utan avbrott av något slag. Om du inte använder hello ny tabell API (förhandsgranskning) kan inte du omfattas av hello automatisk index, hello ytterligare konsekvenskontroll alternativet eller distributionslistor. 

### <a name="how-do-i-add-replication-of-hello-data-in-hello-premium-table-api-preview-across-multiple-regions-of-azure"></a>Hur lägger jag till replikering av data från hello i hello premium tabell-API (förhandsgranskning) över flera regioner Azure?
Du kan använda hello Azure Cosmos DB portalen [globala replikeringsinställningarna](tutorial-global-distribution-documentdb.md#portal) tooadd regioner som är lämpliga för ditt program. toodevelop ett globalt distribuerade program du bör också lägga till ditt program med hello PreferredLocation information set toohello lokala region för att tillhandahålla låg latens som skrivskyddade. 

### <a name="how-do-i-change-hello-primary-write-region-for-hello-account-in-hello-premium-table-api-preview"></a>Hur ändrar jag hello primära skrivåtgärder region för hello konto i hello premium tabell-API (förhandsgranskning)
Du kan använda hello Azure Cosmos DB globala replikering portal fönstret tooadd en region och växlar sedan över toohello krävs region. Instruktioner finns i [utveckling med flera regioner Azure Cosmos DB konton](regional-failover.md). 

### <a name="how-do-i-configure-my-preferred-read-regions-for-low-latency-when-i-distribute-my-data"></a>Hur konfigurerar jag min primära skrivskyddade regioner för låg fördröjning när jag fördela mina data? 
toohelp läsa från hello lokal plats, Använd hello PreferredLocation nyckeln i hello app.config-fil. Hello tabell-API (förhandsgranskning) genererar ett fel om LocationMode har angetts för befintliga program. Ta bort den koden eftersom hello premium tabell-API (förhandsgranskning) hämtar informationen från hello app.config-fil. Mer information finns i [Azure Cosmos DB funktioner](../cosmos-db/tutorial-develop-table-dotnet.md#azure-cosmos-db-capabilities).

### <a name="how-should-i-think-about-consistency-levels-in-hello-table-api-preview"></a>Hur får jag tänker konsekvensnivåer i hello tabell-API (förhandsgranskning)? 
Azure Cosmos-DB tillhandahåller välmotiverat avvägningarna mellan konsekvens, tillgänglighet och svarstid. Azure Cosmos-DB erbjuder fem konsekvensnivåer tooTable API (förhandsgranskning) utvecklare, så att du kan välja hello rätt konsekvenskontroll modellen på hello tabell nivå och enskilda begär när datafrågor hello. När en klient ansluter ange den en konsekvenskontroll nivå. Du kan ändra hello nivå via hello app.config inställning för hello värdet för hello TableConsistencyLevel nyckeln. 

hello tabell-API (förhandsversion) ger låg latens läser med ”Läs egna skrivningar” med sessionskonsekvens som hello standard. Mer information finns i [konsekvensnivåer](consistency-levels.md). 

Som standard ger Azure Table storage stark konsekvens inom en region och Eventual konsekvens hello sekundära platser. 

### <a name="does-azure-cosmos-db-offer-more-consistency-levels-than-standard-tables"></a>Erbjuder Azure Cosmos DB mer konsekvensnivåer än standardtabeller?
Ja, finns information om hur toobenefit från hello distribuerade uppbyggnad Azure Cosmos DB [konsekvensnivåer](consistency-levels.md). Eftersom garantier för hello konsekvensnivåer, kan du använda dem med förtroende. Mer information finns i [Azure Cosmos DB funktioner](../cosmos-db/tutorial-develop-table-dotnet.md#azure-cosmos-db-capabilities).

### <a name="when-global-distribution-is-enabled-how-long-does-it-take-tooreplicate-hello-data"></a>När globala distribution är aktiverat, hur lång tid tar det tooreplicate hello data?
Vi genomför hello data varaktigt i hello lokala region och skicka hello tooother dataområden direkt i en fråga i millisekunder. Replikeringen är beroende av endast hello fram och åter tid för hello datacenter. toolearn mer om hello global distributionsplatsen möjligheterna för Azure Cosmos DB finns [Azure Cosmos DB: en tjänst för globalt distribuerad databas på Azure](distribute-data-globally.md).

### <a name="can-hello-read-request-consistency-level-be-changed"></a>Kan hello läsbegäran konsekvensnivå ändras?
Med Azure Cosmos DB, kan du ange hello konsekvensnivå på hello behållaren nivå (på hello tabell). Du kan ändra hello nivå genom att tillhandahålla hello värde för nyckeln TableConsistencyLevel i hello app.config-fil med hjälp av hello SDK. hello möjliga värden är: stark, begränsat föråldrad, Session, konsekvent Prefix och Eventual. Mer information finns i [data justerbara konsekvensnivåer i Azure Cosmos DB](consistency-levels.md). hello viktiga idé är att du inte kan ange hello begäran konsekvenskontroll nivån på mer än hello inställningen för hello tabellen. Du kan till exempel ange hello konsekvensnivå för hello tabellen på Eventual och hello begäran konsekvensnivå på starka. 

### <a name="how-does-hello-premium-table-api-preview-account-handle-failover-if-a-region-goes-down"></a>Hur hello premium tabell-API (förhandsgranskning) konto hanterar redundans om en region kraschar? 
hello premium tabell-API (förhandsgranskning) lånar från hello globalt distribuerade plattform på Azure Cosmos DB. tooensure att ditt program kan tolerera datacenter driftstopp, aktivera minst ett mer regionen för hello konto i hello Azure Cosmos DB portal [utveckling med flera regioner Azure Cosmos DB konton](regional-failover.md). Du kan ange hello prioriteten för hello region med hello portal [utveckling med flera regioner Azure Cosmos DB konton](regional-failover.md). 

Du kan lägga till så många områden som du vill använda för hello kontot och styra där den kan växla över tooby som tillhandahåller en prioritet för växling vid fel. I kursen toouse hello databasen måste tooprovide det ett program för. När du gör det får inte driftstopp för dina kunder. hello klient-SDK är auto homing. Det vill säga kan identifiera hello region som är igång och automatiskt växla över toohello ny region.

### <a name="is-hello-premium-table-api-preview-enabled-for-backups"></a>Hello premium tabell-API (förhandsversion) har aktiverats för säkerhetskopiering?
Ja, hello premium tabell-API (förhandsgranskning) lånar från hello plattform på Azure DB som Cosmos för säkerhetskopiering. Säkerhetskopieringar görs automatiskt. Mer information finns i [Online säkerhetskopiering och återställning med Azure Cosmos DB](online-backup-and-restore.md).

 
### <a name="does-hello-table-api-preview-index-all-attributes-of-an-entity-by-default"></a>Hello tabell-API (förhandsversion) index alla attribut för en entitet som standard?
Ja, alla attribut för en entitet indexeras som standard. Mer information finns i [Azure Cosmos DB: indexering principer](indexing-policies.md). 

### <a name="does-this-mean-i-do-not-have-toocreate-multiple-indexes-toosatisfy-hello-queries"></a>Detta betyder inte toocreate flera index toosatisfy hello frågor? 
Ja, Azure Cosmos DB ger automatisk indexering av alla attribut utan någon schemadefinition. Det här automation Frigör utvecklare toofocus på hello program istället för på skapandet av index och hantering. Mer information finns i [Azure Cosmos DB: indexering principer](indexing-policies.md).

### <a name="can-i-change-hello-indexing-policy"></a>Kan jag ändra hello indexprincip?
Ja, du kan ändra hello indexprincip genom att tillhandahålla hello indexdefinitionen. Mer information finns i [Azure Cosmos DB funktioner](../cosmos-db/tutorial-develop-table-dotnet.md#azure-cosmos-db-capabilities). Du behöver tooproperly koda och undvika hello inställningar. 

I strängen json-format i hello app.config-fil:
```
{
  "indexingMode": "consistent",
  "automatic": true,
  "includedPaths": [
    {
      "path": "/somepath",
      "indexes": [
        {
          "kind": "Range",
          "dataType": "Number",
          "precision": -1
        },
        {
          "kind": "Range",
          "dataType": "String",
          "precision": -1
        } 
      ]
    }
  ],
  "excludedPaths": 
[
 {
      "path": "/anotherpath"
 }
]
}
```

### <a name="azure-cosmos-db-as-a-platform-seems-toohave-lot-of-capabilities-such-as-sorting-aggregates-hierarchy-and-other-functionality-will-you-be-adding-these-capabilities-toohello-table-api"></a>Azure Cosmos-DB som en plattform verkar toohave många funktioner, t.ex sortera, aggregeringar, hierarkin och andra funktioner. Du lägger till dessa funktioner toohello tabell API? 
I Förhandsgranska hello tabell API ger hello samma fråga funktioner som Azure Table storage. Azure Cosmos-DB också stöd för sortering, aggregeringar, geospatiala frågan, hierarki och en mängd olika inbyggda funktioner. Vi ger ytterligare funktioner i hello tabell API i en framtida tjänstuppdatering. Mer information finns i [SQL-frågor för Azure Cosmos DB DocumentDB API](../documentdb/documentdb-sql-query.md).
 
### <a name="when-should-i-change-tablethroughput-for-hello-table-api-preview"></a>När bör jag ändra TableThroughput för hello tabell-API (förhandsgranskning)
Du bör ändra TableThroughput när någon av hello följande villkor gäller:
* Du utför en extrahering, transformering och laddning (ETL) av data eller om du vill tooupload stora mängder data på kort tid. 
* Du behöver mer genomströmning från hello behållaren på hello serverdel. Till exempel visas att hello används genomströmning är mer än hello etablerat dataflöde och du är komma begränsas. Mer information finns i [Set genomströmning för Azure Cosmos DB behållare](set-throughput.md).

### <a name="can-i-scale-up-or-scale-down-hello-throughput-of-my-table-api-preview-table"></a>Kan jag skala upp eller ned hello genomflödet i tabellens tabell-API (förhandsgranskning)? 
Ja, du kan använda hello Azure Cosmos DB portal skala fönstret tooscale hello genomflöde. Mer information finns i [Set genomströmning](set-throughput.md).

### <a name="is-a-default-tablethroughput-set-for-newly-provisioned-tables"></a>Är en standarduppsättning TableThroughput för nyetablerade tabeller?
Ja, om du inte åsidosätter hello TableThroughput via app.config och Använd inte en förskapad behållare i Azure Cosmos DB hello tjänsten skapar en tabell med genomströmning på 400.
 
### <a name="is-there-any-change-of-pricing-for-existing-customers-of-hello-standard-table-api"></a>Finns det några ändringar av priser för befintliga kunder hello standard tabell API?
Ingen. Det finns ingen ändring i priset för befintliga standard tabell API-kunder. 

### <a name="how-is-hello-price-calculated-for-hello-table-api-preview"></a>Hur beräknas hello priset för hello tabell-API (förhandsgranskning)? 
hello pris beror på hello allokerade TableThroughput. 

### <a name="how-do-i-handle-any-throttling-on-hello-tables-in-table-api-preview-offering"></a>Hur hanterar jag en begränsning på hello tabeller i tabell-API (förhandsversion) ger? 
Om hello förfrågningar överskrider hello kapacitet hello etablerat dataflöde för hello underliggande behållare, du får ett fel och hello SDK kommer att försöka hello anrop genom att använda hello i principen.

### <a name="why-do-i-need-toochoose-a-throughput-apart-from-partitionkey-and-rowkey-tootake-advantage-of-hello-premium-table-api-preview-offering-of-azure-cosmos-db"></a>Varför behöver toochoose en genomströmning förutom PartitionKey och RowKey tootake utnyttja hello premium tabell-API (förhandsgranskning) erbjudande på Azure Cosmos DB?
Azure Cosmos-DB anger en standard-genomströmning för din behållaren om du inte anger något i hello app.config-fil. 

Azure Cosmos-DB tillhandahåller garantier för prestanda och fördröjning med övre gränser för åtgärden. Garantin är möjligt när hello-motorn kan tillämpa styrning på hello innehavare. Ange TableThroughput garanterar att du får hello garanteras genomflöde och svarstid, eftersom hello plattform reserverar denna kapacitet och garanterar operativa lyckades. 

Med hjälp av hello genomströmning specifikation du Elastiskt ändra toobenefit från hello säsongsvärdet för programmet, hello genomströmning behov och spara kostnader.

### <a name="azure-storage-sdk-has-been-very-inexpensive-for-me-because-i-pay-only-toostore-hello-data-and-i-rarely-query-hello-new-azure-cosmos-db-offering-seems-toobe-charging-me-even-though-i-have-not-performed-a-single-transaction-or-stored-anything-can-you-please-explain"></a>Azure Storage SDK: N har så billigt mig, eftersom jag betala endast toostore hello data, och jag sällan fråga. hello nytt Azure Cosmos DB erbjudande verkar toobe debitering mig även om jag inte har utfört en enda transaktion eller lagras något. Kan du ange förklara?

Azure Cosmos-DB är utformad toobe ett globalt distribuerade, SLA-baserade system med garantier för tillgänglighet, svarstid och genomströmning. När du reserverar genomflöde i Azure Cosmos DB garanteras den, till skillnad från hello genomflödet i andra system. Azure Cosmos-DB tillhandahåller ytterligare funktioner som kunder har begärt, till exempel sekundärindex och distributionslistor. Vi ger en optimerad för genomströmning modell under hello förhandsversionen och så småningom kommer vi planerar tooprovide en optimerad lagring modellen toomeet kundernas behov. 

### <a name="i-never-get-a-quota-full-notification-indicating-that-a-partition-is-full-when-i-ingest-data-into-table-storage-with-hello-table-api-preview-i-do-get-this-message-is-this-offering-limiting-me-and-forcing-me-toochange-my-existing-application"></a>Jag får aldrig en ”full” kvotmeddelanden (som anger att en partition är full) när jag mata in data i Table storage. Med hello tabell-API (förhandsgranskning) detta meddelande visas. Detta ger att begränsa mig och tvinga mig toochange mitt befintliga program?

Azure Cosmos-DB är ett SLA-baserat system som ger obegränsad skala garantier för latens, dataflöde, tillgänglighet och konsekvenskontroll. tooensure garanteras premium prestanda, kontrollera att index och Datastorleken är hanterbar och skalbar. hello 10 GB-gränsen på hello antal entiteter eller objekt per Partitionsnyckeln är tooensure att vi ger utmärkt prestanda för sökning och fråga. tooensure som programmet skalas bra även för Azure Storage, rekommenderar vi att du *inte* skapa en varm partition genom att lagra all information i en partition och exempelvärden. 

### <a name="so-partitionkey-and-rowkey-are-still-required-with-hello-new-table-api-preview"></a>Så PartitionKey och RowKey krävs fortfarande med hello ny tabell-API (förhandsgranskning)? 
Ja. Eftersom hello ytan på hello tabell-API (förhandsversion) är liknande toothat av hello tabellagring SDK, tillhandahåller hello partitionsnyckel ett effektivt sätt toodistribute hello data. Hej radnyckel är unikt i den aktuella partitionen. Hej raden nyckel måste toobe finns och kan inte vara null i hello standard SDK. Hej RowKey längd är 255 byte och hello PartitionKey är 100 byte (snart toobe ökat too1 KB). 

### <a name="what-are-hello-error-messages-for-hello-table-api-preview"></a>Vad är hello felmeddelanden för hello tabell-API (förhandsgranskning)?
Eftersom den här förhandsgranskningen är kompatibel med hello Standardtabell mappar de flesta av hello fel toohello fel från hello Standardtabell. 

### <a name="why-do-i-get-throttled-when-i-try-toocreate-lot-of-tables-one-after-another-in-hello-table-api-preview"></a>Varför jag hämta begränsas när jag försöker toocreate mängd tabeller efter varandra i hello tabell-API (förhandsgranskning)?
Azure Cosmos-DB är ett SLA-baserat system som tillhandahåller svarstid, dataflöde, tillgänglighet och konsekvens garanterar. Eftersom det är ett etablerade system, reserverar resurser tooguarantee dessa krav. hello snabb frekvensen för skapande av tabeller identifieras och begränsas. Vi rekommenderar att du granskar hello frekvensen för skapande av tabeller och sänka den tooless än 5 per minut. Kom ihåg att hello tabell-API (förhandsversion) är ett etablerade system. hello tidpunkt då du etablerar den börjar du toopay för den. 

## <a name="develop-against-hello-graph-api-preview"></a>Utveckla mot hello Graph API (förhandsgranskning)
### <a name="how-can-i-apply-hello-functionality-of-graph-api-preview-tooazure-cosmos-db"></a>Hur kan jag använda hello funktionerna för Graph API (förhandsgranskning) tooAzure Cosmos DB?
Du kan använda ett tillägg tooapply hello funktionalitet för Graph API (förhandsversion). Det här biblioteket kallas Microsoft Azure diagram och det är tillgängligt på NuGet. 

### <a name="it-looks-like-you-support-hello-gremlin-graph-traversal-language-do-you-plan-tooadd-more-forms-of-query"></a>Det verkar som du stöder hello Gremlin diagram traversal språk. Planeras tooadd flera typer av frågan?
Ja, vi planerar tooadd andra mekanismer för frågan i hello framtida. 

### <a name="how-can-i-use-hello-new-graph-api-preview-offering"></a>Hur kan jag använda hello nytt erbjudande för Graph API (förhandsgranskning)? 
tooget igång, fullständig hello [Graph API](../cosmos-db/create-graph-dotnet.md) Snabbkurs artikel.

<a id="moving-to-cosmos-db"></a>
## <a name="questions-from-documentdb-customers"></a>Frågor från DocumentDB kunder
### <a name="why-are-you-moving-tooazure-cosmos-db"></a>Varför flyttar du tooAzure Cosmos DB? 

Azure Cosmos-DB är hello nästa stora leap i globalt distribuerade med skalning molnet databaser. Som en DocumentDB-kund nu har du åtkomst toohello banbrytande system och funktioner som erbjuds av Azure Cosmos DB.

Azure Cosmos-DB starta som en ”projekt Florens” i 2010 tooaddress hello stötestenar som utvecklare bygga storskaliga program i Microsoft. hello utmaningar för att skapa globalt distribuerade appar är inte unikt tooMicrosoft så vi gjort hello första generationen av den här tekniken finns i 2015 tooAzure utvecklare i hello form av Azure DocumentDB. 

Sedan dess har vi lagt till nya funktioner och introduceras viktiga nya funktioner. Azure Cosmos-DB är hello resultat. Som en del av den här versionen blir DocumentDB kunder med sina data automatiskt och sömlöst du Azure DB som Cosmos-kunder. Dessa funktioner finns i hello områden hello core databasmotorn, samt global distributionsplatsen, elastisk skalbarhet och branschledande, omfattande SLA: er. Vi har särskilt utvecklats hello Azure Cosmos DB database engine tooefficiently kartan alla populära datamodeller typen System och API: er toohello underliggande datamodellen av Azure Cosmos DB. 

hello aktuella developer-riktade uttryck för att arbetet är hello nytt stöd för [Gremlin](../cosmos-db/graph-introduction.md) och [Table storage API: er](../cosmos-db/table-introduction.md). Detta är bara hello början. Vi planerar tooadd andra populära API: er och nyare datamodeller över tid, med mer utvecklingen av prestanda och lagringsutrymme på global nivå. 

Det är viktigt toopoint ut den hello DocumentDB [SQL dialect](../documentdb/documentdb-sql-query.md) har alltid varit en av hello många API: er som hello underliggande Azure Cosmos DB kan stödja. För utvecklare som använder en helt hanterad tjänst, till exempel Azure Cosmos DB hello endast gränssnittet toohello tjänsten är hello API: er som exponeras av hello-tjänsten. Inget verkligen ändringar för befintliga DocumentDB-kunder. I Azure Cosmos DB få exakt hello samma SQL API som DocumentDB erbjuder. Och nu (och i hello framtida) du kan komma åt andra funktioner som tidigare inte tillgänglig 

Ett annat uttryck för våra fortsatt arbete är hello utökad grunden för global och elastisk skalbarhet dataflöden och lagringsutrymmen. Vi har gjort flera förbättringar av grundläggande toohello global distributionsplatsen undersystemet. En av hello många developer-riktade funktioner är hello konsekvent prefixet konsekvent modell, som gör en totala fem väldefinierade konsekvenskontroll modeller. Vi kommer att släppa många mer intressant funktioner som de förfaller. 

### <a name="what-do-i-need-toodo-tooensure-that-my-documentdb-resources-continue-toorun-on-azure-cosmos-db"></a>Vad gör jag behöver toodo tooensure att DocumentDB-resurser fortsätter toorun på Azure Cosmos DB?

Du måste toomake inga ändringar alls. DocumentDB-resurser är nu Azure Cosmos DB resurser och utan avbrott i tjänsten hello uppstod när flyttningen inträffade.

### <a name="what-changes-do-i-need-toomake-for-my-app-toowork-with-azure-cosmos-db"></a>Ändringar har jag behöver toomake för min app toowork med Azure Cosmos DB?

Det finns inga ändringar toomake. Namn för klasser, namnområden och NuGet-paketet har inte ändrats. Som alltid rekommenderar vi att du behåller din SDK in toodate tootake nytta av hello senaste funktioner och förbättringar. 

### <a name="whats-changed-in-hello-azure-portal"></a>Nyheter i hello Azure-portalen?

DocumentDB visas inte längre i hello-portalen som en Azure-tjänst. Är en ny Azure DB som Cosmos-ikon som visas i följande bild hello i dess ställe. Alla samlingar är tillgängliga, som de fanns innan, och du kan fortfarande skala dataflöde, ändra konsekvensnivåer och övervaka SLA: er. hello-funktionerna i Data Explorer (förhandsversion) har förbättrats. Du kan nu visa och redigera dokument, skapa och köra frågor och arbeta med lagrade procedurer, utlösare och UDF från en sida som visas i följande bild hello: 

![hello Azure Cosmos DB samlingar bladet](./media/faq/cosmos-db-data-explorer.png)

### <a name="are-there-changes-toopricing"></a>Finns det ändringar toopricing?

Nej, hello kostnaden för körs appen på Azure Cosmos DB är hello samma innan.

### <a name="are-there-changes-toohello-slas"></a>Finns det ändringar toohello SLA: er?

Nej, Hej serviceavtal för tillgänglighet, konsekvens, svarstid och genomströmning ändras inte och visas fortfarande i hello-portalen. Mer information finns i [SLA för Azure Cosmos DB](https://azure.microsoft.com/support/legal/sla/cosmos-db/).
   
![Att göra-app med exempeldata](./media/faq/azure-cosmosdb-portal-metrics-slas.png)


[azure-portal]: https://portal.azure.com
[query]: documentdb-sql-query.md
