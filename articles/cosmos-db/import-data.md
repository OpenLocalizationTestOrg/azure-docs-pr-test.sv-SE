---
title: "aaaDatabase Migreringsverktyget för Azure Cosmos DB | Microsoft Docs"
description: "Lär dig hur toouse hello Öppna källa Azure Cosmos DB data migrering verktyg tooimport data tooAzure Cosmos DB från olika källor, inklusive MongoDB, SQL Server, Table storage, Amazon DynamoDB, CSV och JSON-filer. CSV-tooJSON konvertering."
keywords: "CSV-toojson, Migreringsverktyg för databasen, konvertera csv toojson"
services: cosmos-db
author: andrewhoh
manager: jhubbard
editor: monicar
documentationcenter: 
ms.assetid: d173581d-782a-445c-98d9-5e3c49b00e25
ms.service: cosmos-db
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/06/2017
ms.author: anhoh
ms.custom: mvc
ms.openlocfilehash: 997648a31602d854db75bb6ce4e2ecff36fc1069
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="how-tooimport-data-into-azure-cosmos-db-for-hello-documentdb-api"></a>Hur tooimport data till Azure Cosmos DB för hello API DocumentDB?

Den här självstudiekursen innehåller anvisningar om hur du använder hello Azure Cosmos DB: DocumentDB API datamigrering verktyg som kan importera data från olika källor, inklusive JSON-filer, CSV-filer, SQL, MongoDB, Azure Table storage, Amazon DynamoDB och Azure Cosmos DB DocumentDB API-samlingar i samlingar för använda med Azure Cosmos DB och hello DocumentDB-API. Hej datamigreringsverktyget kan också användas när du migrerar från en enda partition samling tooa flera partition samling för hello DocumentDB-API.

Hej datamigreringsverktyget fungerar endast när importera data till Azure Cosmos DB för använder med hello DocumentDB-API. Importerar data för användning med hello tabell API eller Graph API stöds inte just nu. 

tooimport data för användning med hello MongoDB-API, se [Azure Cosmos DB: hur toomigrate data för hello MongoDB API?](mongodb-migrate.md).

Den här kursen ingår hello följande uppgifter:

> [!div class="checklist"]
> * Installera hello datamigreringsverktyget
> * Importera data från olika datakällor
> * Exportera från Azure Cosmos DB tooJSON

## <a id="Prerequisites"></a>Förhandskrav
Innan du följer hello anvisningarna i den här artikeln bör du kontrollera att du har hello följande installerat:

* [Microsoft .NET Framework 4.51](https://www.microsoft.com/download/developer-tools.aspx) eller högre.

## <a id="Overviewl"></a>Översikt över hello datamigreringsverktyget
Hej datamigreringsverktyget är en öppen källkod som importerar data tooAzure Cosmos DB från olika källor, inklusive:

* JSON-filer
* MongoDB
* SQL Server
* CSV-filer
* Azure Table Storage
* Amazon DynamoDB
* HBase
* Azure DB Cosmos-samlingar

Medan hello-Importverktyget innehåller ett grafiskt användargränssnitt (dtui.exe), kan den också drivas från hello kommandorad (dt.exe). Det finns i själva verket ett alternativet toooutput hello associerade kommando när du har installerat en import via hello Användargränssnittet. Tabell källdata (t.ex. SQL Server- eller CSV-filer) kan omvandlas så att hierarkiska relationer (underdokument) kan skapas under importen. Håll läsa toolearn mer om alternativ för datakällor exempel kommandorader tooimport från varje källa, mål-alternativ och visa resultat av import.

## <a id="Install"></a>Installera verktyget för hello datamigrering
källkoden för hello migrering verktyget finns på GitHub i [den här lagringsplatsen](https://github.com/azure/azure-documentdb-datamigrationtool) och en kompilerad version är tillgänglig från [Microsoft Download Center](http://www.microsoft.com/downloads/details.aspx?FamilyID=cda7703a-2774-4c07-adcc-ad02ddc1a44d). Du kan kompilera hello lösning eller bara ladda ned och extrahera hello kompilerad version tooa katalogen för ditt val. Kör sedan antingen:

* **Dtui.exe**: grafiskt gränssnittsversion hello-verktyget
* **DT.exe**: kommandoraden version hello-verktyget

## <a name="import-data"></a>Importera data

När du har installerat hello-verktyget är det tid tooimport dina data. Vilken typ av data vill du tooimport?

* [JSON-filer](#JSON)
* [MongoDB](#MongoDB)
* [MongoDB exportfilerna](#MongoDBExport)
* [SQL Server](#SQL)
* [CSV-filer](#CSV)
* [Azure Table Storage](#AzureTableSource)
* [Amazon DynamoDB](#DynamoDBSource)
* [BLOB](#BlobImport)
* [Azure DB Cosmos-samlingar](#DocumentDBSource)
* [HBase](#HBaseSource)
* [Azure DB Cosmos-massimport](#DocumentDBBulkImport)
* [Azure DB Cosmos sekventiella post import](#DocumentDSeqTarget)


## <a id="JSON"></a>tooimport JSON-filer
hello JSON filalternativet källa Importverktyget kan du tooimport en eller flera dokument JSON-filer eller JSON-filer att var och en innehåller en matris av JSON-dokument. När du lägger till mapparna som innehåller tooimport för JSON-filer har hello möjlighet att rekursivt söker efter filer i undermappar.

![Skärmbild av JSON-Filalternativ - Databasverktyg för migrering](./media/import-data/jsonsource.png)

Här följer några kommandoraden prover tooimport JSON-filer:

    #Import a single JSON file
    dt.exe /s:JsonFile /s.Files:.\Sessions.json /t:CosmosDBBulk /t.ConnectionString:"AccountEndpoint=<CosmosDB Endpoint>;AccountKey=<CosmosDB Key>;Database=<CosmosDB Database>;" /t.Collection:Sessions /t.CollectionThroughput:2500

    #Import a directory of JSON files
    dt.exe /s:JsonFile /s.Files:C:\TESessions\*.json /t:CosmosDBBulk /t.ConnectionString:" AccountEndpoint=<CosmosDB Endpoint>;AccountKey=<CosmosDB Key>;Database=<CosmosDB Database>;" /t.Collection:Sessions /t.CollectionThroughput:2500

    #Import a directory (including sub-directories) of JSON files
    dt.exe /s:JsonFile /s.Files:C:\LastFMMusic\**\*.json /t:CosmosDBBulk /t.ConnectionString:" AccountEndpoint=<CosmosDB Endpoint>;AccountKey=<CosmosDB Key>;Database=<CosmosDB Database>;" /t.Collection:Music /t.CollectionThroughput:2500

    #Import a directory (single), directory (recursive), and individual JSON files
    dt.exe /s:JsonFile /s.Files:C:\Tweets\*.*;C:\LargeDocs\**\*.*;C:\TESessions\Session48172.json;C:\TESessions\Session48173.json;C:\TESessions\Session48174.json;C:\TESessions\Session48175.json;C:\TESessions\Session48177.json /t:CosmosDBBulk /t.ConnectionString:"AccountEndpoint=<CosmosDB Endpoint>;AccountKey=<CosmosDB Key>;Database=<CosmosDB Database>;" /t.Collection:subs /t.CollectionThroughput:2500

    #Import a single JSON file and partition hello data across 4 collections
    dt.exe /s:JsonFile /s.Files:D:\\CompanyData\\Companies.json /t:CosmosDBBulk /t.ConnectionString:"AccountEndpoint=<CosmosDB Endpoint>;AccountKey=<CosmosDB Key>;Database=<CosmosDB Database>;" /t.Collection:comp[1-4] /t.PartitionKey:name /t.CollectionThroughput:2500

## <a id="MongoDB"></a>tooimport från MongoDB

> [!IMPORTANT]
> Om du importerar tooan Azure DB som Cosmos-konto med stöd för MongoDB, följer du dessa [instruktioner](mongodb-migrate.md).
> 
> 

hello MongoDB källa Importverktyget alternativet kan du tooimport från en enskild MongoDB-samling och du kan också filtrera dokument med hjälp av en fråga och/eller ändra hello dokumentstruktur med hjälp av en projektion.  

![Skärmbild av MongoDB alternativ](./media/import-data/mongodbsource.png)

hello anslutningssträngen är hello standard MongoDB-format:

    mongodb://<dbuser>:<dbpassword>@<host>:<port>/<database>

> [!NOTE]
> Använd hello Kontrollera kommandot tooensure som hello MongoDB-instansen som anges i hello anslutningsfältet sträng kan nås.
> 
> 

Ange namn på hello mängden hello varifrån data ska importeras. Du kan om du vill ange eller ange en fil för en fråga (t.ex. {pop: {$gt: 5000}}) och/eller projektion (t.ex. {loc:0}) tooboth filter och form hello data toobe importeras.

Här följer några exempel tooimport i kommandoraden från MongoDB:

    #Import all documents from a MongoDB collection
    dt.exe /s:MongoDB /s.ConnectionString:mongodb://<dbuser>:<dbpassword>@<host>:<port>/<database> /s.Collection:zips /t:CosmosDBBulk /t.ConnectionString:"AccountEndpoint=<CosmosDB Endpoint>;AccountKey=<CosmosDB Key>;Database=<CosmosDB Database>;" /t.Collection:BulkZips /t.IdField:_id /t.CollectionThroughput:2500

    #Import documents from a MongoDB collection which match hello query and exclude hello loc field
    dt.exe /s:MongoDB /s.ConnectionString:mongodb://<dbuser>:<dbpassword>@<host>:<port>/<database> /s.Collection:zips /s.Query:{pop:{$gt:50000}} /s.Projection:{loc:0} /t:CosmosDBBulk /t.ConnectionString:"AccountEndpoint=<CosmosDB Endpoint>;AccountKey=<CosmosDB Key>;Database=<CosmosDB Database>;" /t.Collection:BulkZipsTransform /t.IdField:_id/t.CollectionThroughput:2500

## <a id="MongoDBExport"></a>tooimport MongoDB exportfilerna

> [!IMPORTANT]
> Om du importerar tooan Azure DB som Cosmos-konto med stöd för MongoDB, följer du dessa [instruktioner](mongodb-migrate.md).
> 
> 

Hej MongoDB export JSON källa Importverktyget filalternativet kan du tooimport en eller flera JSON-filer som genereras från hello mongoexport verktyg.  

![Skärmbild av MongoDB exportalternativ för källa](./media/import-data/mongodbexportsource.png)

När du lägger till mapparna som innehåller MongoDB export JSON-filer för import, har hello möjlighet att rekursivt söker efter filer i undermappar.

Här är en kommandorad exempel tooimport från MongoDB export JSON-filer:

    dt.exe /s:MongoDBExport /s.Files:D:\mongoemployees.json /t:CosmosDBBulk /t.ConnectionString:"AccountEndpoint=<CosmosDB Endpoint>;AccountKey=<CosmosDB Key>;Database=<CosmosDB Database>;" /t.Collection:employees /t.IdField:_id /t.Dates:Epoch /t.CollectionThroughput:2500

## <a id="SQL"></a>tooimport från SQL Server
hello SQL källa Importverktyget alternativet kan du tooimport från en enskild SQL Server-databas och du kan också filtrera hello poster toobe importeras med hjälp av en fråga. Du kan dessutom ändra hello dokumentstruktur genom att ange kapslade avgränsare (Mer information om det finns en liten stund).  

![Skärmbild av SQL - alternativ för databas Migreringsverktyg](./media/import-data/sqlexportsource.png)

hello hello Anslutningssträngens format är hello standard SQL connection-strängformat.

> [!NOTE]
> Använd hello Kontrollera kommandot tooensure som hello SQL Server-instans som angetts i hello anslutningsfältet sträng kan nås.
> 
> 

hello kapsla avgränsare egenskapen är används toocreate hierarkiska relationer (underordnade dokument) under importen. Tänk hello följande SQL-frågan:

*Välj OMVANDLINGEN (BusinessEntityID AS varchar) som Id, namn, adresstyp som [Address.AddressType], AddressLine1 som [Address.AddressLine1], ort som [Address.Location.City], StateProvinceName som [Address.Location.StateProvinceName], postnummer som [ Address.PostalCode] CountryRegionName som [Address.CountryRegionName] från Sales.vStoreWithAddresses där adresstyp = 'Huvudkontoret'*

Som returnerar hello följande (ofullständiga) resultat:

![Skärmbild av SQL-frågeresultat](./media/import-data/sqlqueryresults.png)

Observera hello-alias som Address.AddressType och Address.Location.StateProvinceName. Genom att ange kapslade avgränsare av '.', hello-Importverktyget skapar adress och Address.Location underdokument under hello importera. Här är ett exempel på ett resulterande dokument i Azure Cosmos DB:

*{”id”: ”956”, ”Name”: ”ökad försäljning och tjänst”, ”adress”: {”adresstyp”: ”Main Office”, ”AddressLine1”: ”#500 75 O'Connor gata”, ”plats”: {”stad”: ”Ottawa”, ”StateProvinceName”: ”Ontario”}, ”postnummer”: ”K4B 1S2”, ”CountryRegionName” ”: Kanada ”}}*

Här följer några exempel tooimport i kommandoraden från SQL Server:

    #Import records from SQL which match a query
    dt.exe /s:SQL /s.ConnectionString:"Data Source=<server>;Initial Catalog=AdventureWorks;User Id=advworks;Password=<password>;" /s.Query:"select CAST(BusinessEntityID AS varchar) as Id, * from Sales.vStoreWithAddresses WHERE AddressType='Main Office'" /t:CosmosDBBulk /t.ConnectionString:" AccountEndpoint=<CosmosDB Endpoint>;AccountKey=<CosmosDB Key>;Database=<CosmosDB Database>;" /t.Collection:Stores /t.IdField:Id /t.CollectionThroughput:2500

    #Import records from sql which match a query and create hierarchical relationships
    dt.exe /s:SQL /s.ConnectionString:"Data Source=<server>;Initial Catalog=AdventureWorks;User Id=advworks;Password=<password>;" /s.Query:"select CAST(BusinessEntityID AS varchar) as Id, Name, AddressType as [Address.AddressType], AddressLine1 as [Address.AddressLine1], City as [Address.Location.City], StateProvinceName as [Address.Location.StateProvinceName], PostalCode as [Address.PostalCode], CountryRegionName as [Address.CountryRegionName] from Sales.vStoreWithAddresses WHERE AddressType='Main Office'" /s.NestingSeparator:. /t:CosmosDBBulk /t.ConnectionString:" AccountEndpoint=<CosmosDB Endpoint>;AccountKey=<CosmosDB Key>;Database=<CosmosDB Database>;" /t.Collection:StoresSub /t.IdField:Id /t.CollectionThroughput:2500

## <a id="CSV"></a>tooimport CSV-filer och konvertera CSV tooJSON
hello CSV-filen källa Importverktyget alternativet kan du tooimport en eller flera CSV-filer. När du lägger till mapparna som innehåller CSV-filer för import, har hello möjlighet att rekursivt söker efter filer i undermappar.

![Skärmbild av CSV - alternativ för CSV-tooJSON](media/import-data/csvsource.png)

Liknande toohello SQL-källans, hello kapsla avgränsare egenskapen kanske används toocreate hierarkiska relationer (underordnade dokument) under importen. Överväg följande CSV rubrikrad och datarader hello:

![Skärmbild av CSV Exempelposter - CSV tooJSON](./media/import-data/csvsample.png)

Observera hello-alias som DomainInfo.Domain_Name och RedirectInfo.Redirecting. Genom att ange kapslade avgränsare av '.', hello-Importverktyget skapar DomainInfo och RedirectInfo underdokument under hello importera. Här är ett exempel på ett resulterande dokument i Azure Cosmos DB:

*{”DomainInfo”: {”Domain_Name”: ”ACUS.GOV”, ”Domain_Name_Address”: ”http://www. ACUS.GOV ”},” Federal myndighet ”:” administrativa konferens med hello USA ”,” RedirectInfo ”: {” omdirigera ”:” 0 ”,” Redirect_Destination ””: ”},” id ”:” 9cc565c5-ebcd-1c03-ebd3-cc3e2ecd814d ”}*

hello-Importverktyget försöker tooinfer typinformation för ociterade värden i CSV-filer (inom citattecken värden behandlas alltid som strängar).  Typer som identifieras i hello följande ordning: nummer, datetime, booleskt värde.  

Det finns två andra saker toonote om CSV-import:

1. Som standard ociterade värden alltid bort för flikar och blanksteg, medan inom citattecken värden sparas som-är. Det här beteendet kan åsidosättas med hello trimning citattecken värden kryssrutan eller hello /s.TrimQuoted kommandoradsalternativet.
2. Som standard behandlas en ociterade null som ett null-värde. Det här beteendet kan åsidosättas (d.v.s. behandla en ociterade null som en ”null-sträng) med hello Treat onoterade NULL som sträng kryssrutan eller hello /s.NoUnquotedNulls kommandoradsalternativet.

Här följer ett exempel på kommandoraden för CSV-import:

    dt.exe /s:CsvFile /s.Files:.\Employees.csv /t:CosmosDBBulk /t.ConnectionString:"AccountEndpoint=<CosmosDB Endpoint>;AccountKey=<CosmosDB Key>;Database=<CosmosDB Database>;" /t.Collection:Employees /t.IdField:EntityID /t.CollectionThroughput:2500

## <a id="AzureTableSource"></a>tooimport från Azure-tabellagring
hello Azure Table storage källa Importverktyget alternativet kan du tooimport från en enskild Azure Table storage tabell och du kan också filtrera hello tabell entiteter toobe importeras. Observera att du inte kan använda hello datamigrering verktyget tooimport Azure Table storage-data i Azure Cosmos DB för användning med hello tabell API. Endast import tooAzure Cosmos DB för användning med hello DocumentDB API stöds just nu.

![Skärmbild av Azure Table storage alternativ](./media/import-data/azuretablesource.png)

hello format hello anslutningssträngen för Azure Table storage är:

    DefaultEndpointsProtocol=<protocol>;AccountName=<Account Name>;AccountKey=<Account Key>;

> [!NOTE]
> Använd hello Kontrollera kommandot tooensure som hello Azure Table storage instans anges i hello anslutningsfältet sträng kan nås.
> 
> 

Ange hello namnet på hello Azure tabell från vilken data ska importeras. Du kan du ange en [filter](https://msdn.microsoft.com/library/azure/ff683669.aspx).

hello Azure källa Importverktyget tabellagring har hello följande ytterligare alternativ:

1. Inkludera interna fält
   1. Innehåller alla - alla interna fält (PartitionKey, RowKey och tidsstämpel)
   2. Ingen - undanta alla interna fält
   3. RowKey - bara innehålla hello RowKey fält
2. Välj kolumner
   1. Azure Table storage filter stöder inte projektioner. Om du vill tooonly importera specifika Azure Table Entitetsegenskaper, lägga till dem toohello Välj kolumner lista. Alla andra Entitetsegenskaper kommer att ignoreras.

Här är en kommandorad exempel tooimport från Azure Table storage:

    dt.exe /s:AzureTable /s.ConnectionString:"DefaultEndpointsProtocol=https;AccountName=<Account Name>;AccountKey=<Account Key>" /s.Table:metrics /s.InternalFields:All /s.Filter:"PartitionKey eq 'Partition1' and RowKey gt '00001'" /s.Projection:ObjectCount;ObjectSize  /t:CosmosDBBulk /t.ConnectionString:" AccountEndpoint=<CosmosDB Endpoint>;AccountKey=<CosmosDB Key>;Database=<CosmosDB Database>;" /t.Collection:metrics /t.CollectionThroughput:2500

## <a id="DynamoDBSource"></a>tooimport från Amazon DynamoDB
hello Amazon DynamoDB källa Importverktyget alternativet kan du tooimport från en enskild tabell Amazon DynamoDB och du kan också filtrera hello entiteter toobe importeras. Flera mallar tillhandahålls så att ställa in en import är så enkel som möjligt.

![Skärmbild av Amazon DynamoDB alternativ - Databasverktyg för migrering](./media/import-data/dynamodbsource1.png)

![Skärmbild av Amazon DynamoDB alternativ - Databasverktyg för migrering](./media/import-data/dynamodbsource2.png)

hello Amazon DynamoDB anslutningssträngen hello format är:

    ServiceURL=<Service Address>;AccessKey=<Access Key>;SecretKey=<Secret Key>;

> [!NOTE]
> Använd hello Kontrollera kommandot tooensure som hello Amazon DynamoDB-instans som angetts i hello anslutningsfältet sträng kan nås.
> 
> 

Här är en kommandorad exempel tooimport från Amazon DynamoDB:

    dt.exe /s:DynamoDB /s.ConnectionString:ServiceURL=https://dynamodb.us-east-1.amazonaws.com;AccessKey=<accessKey>;SecretKey=<secretKey> /s.Request:"{   """TableName""": """ProductCatalog""" }" /t:DocumentDBBulk /t.ConnectionString:"AccountEndpoint=<Azure Cosmos DB Endpoint>;AccountKey=<Azure Cosmos DB Key>;Database=<Azure Cosmos DB Database>;" /t.Collection:catalogCollection /t.CollectionThroughput:2500

## <a id="BlobImport"></a>tooimport filer från Azure Blob storage
hello JSON-fil, MongoDB exportfilen och CSV-filen Importverktyget alternativ kan du tooimport en eller flera filer från Azure Blob storage. När du har angett en URL för Blob-behållaren och Kontonyckel, anger du bara ett reguljärt uttryck tooselect hello fil(er) tooimport.

![Skärmbild av Blob alternativ för källa](./media/import-data/blobsource.png)

Här är kommandoraden tooimport JSON exempelfiler från Azure Blob storage:

    dt.exe /s:JsonFile /s.Files:"blobs://<account key>@account.blob.core.windows.net:443/importcontainer/.*" /t:CosmosDBBulk /t.ConnectionString:"AccountEndpoint=<CosmosDB Endpoint>;AccountKey=<CosmosDB Key>;Database=<CosmosDB Database>;" /t.Collection:doctest

## <a id="DocumentDBSource"></a>tooimport från en samling Azure Cosmos DB DocumentDB API
hello Azure Cosmos DB källa Importverktyget alternativet kan du tooimport data från en eller flera Azure Cosmos DB samlingar och du kan också filtrera dokument med hjälp av en fråga.  

![Skärmbild av Azure Cosmos DB alternativ](./media/import-data/documentdbsource.png)

hello Azure Cosmos DB-anslutningssträngen hello format är:

    AccountEndpoint=<CosmosDB Endpoint>;AccountKey=<CosmosDB Key>;Database=<CosmosDB Database>;

hello Azure Cosmos DB anslutningssträngen för kontot som kan hämtas från hello nycklar bladet för hello Azure-portalen, enligt beskrivningen i [hur toomanage ett konto i Azure Cosmos DB](manage-account.md), men hello namnet på databasen som hello måste toobe läggs toohello anslutningssträngen i hello följande format:

    Database=<CosmosDB Database>;

> [!NOTE]
> Använd hello Kontrollera kommandot tooensure som hello Azure DB som Cosmos-instans som anges i hello anslutningsfältet sträng kan nås.
> 
> 

tooimport från en enda Azure DB som Cosmos-samling ange hello namn mängden hello varifrån data ska importeras. tooimport från flera Azure Cosmos DB samlingar, ange ett reguljärt uttryck toomatch ett eller flera samlingsnamn (t.ex. collection01 | collection02 | collection03). Du kan alternativt anger, eller ange en fil för en fråga tooboth filter och formar hello data toobe importeras.

> [!NOTE]
> Eftersom hello samling fältet accepterar reguljära uttryck om du importerar från en enda samling vars namn innehåller specialtecken i reguljära uttryck, måste dessa tecken hoppas därför.
> 
> 

hello Azure Cosmos DB källalternativet Importverktyget har hello följande avancerade alternativ:

1. Ta med intern fält: Anger huruvida tooinclude Azure Cosmos DB dokumentet Systemegenskaper hello exportera (t.ex. _rid, _ts).
2. Antalet återförsök vid fel: Anger hello antal gånger tooretry hello anslutning tooAzure Cosmos DB vid tillfälligt fel (t.ex. anslutningen avbrott i nätverket).
3. Återförsöksintervall: Anger hur länge toowait mellan du försöker hello anslutning tooAzure Cosmos DB vid tillfälligt fel (t.ex. anslutningen avbrott i nätverket).
4. Anslutningsläge: Anger hello anslutning läge toouse med Azure Cosmos DB. hello tillgängliga alternativen är DirectTcp, DirectHttps och Gateway. hello direktanslutning lägen är snabbare och medan hello gateway läge är mer brandvägg eget eftersom den endast använder port 443.

![Skärmbild av Azure Cosmos DB-datakälla avancerade alternativ](./media/import-data/documentdbsourceoptions.png)

> [!TIP]
> hello importläge verktyget standardvärden tooconnection DirectTcp. Om det uppstår brandväggsproblem med Växla tooconnection läge Gateway, som kräver endast port 443.
> 
> 

Här följer några kommandoraden exempel tooimport från Azure Cosmos DB:

    #Migrate data from one Azure Cosmos DB collection tooanother Azure Cosmos DB collections
    dt.exe /s:CosmosDB /s.ConnectionString:"AccountEndpoint=<CosmosDB Endpoint>;AccountKey=<CosmosDB Key>;Database=<CosmosDB Database>;" /s.Collection:TEColl /t:CosmosDBBulk /t.ConnectionString:" AccountEndpoint=<CosmosDB Endpoint>;AccountKey=<CosmosDB Key>;Database=<CosmosDB Database>;" /t.Collection:TESessions /t.CollectionThroughput:2500

    #Migrate data from multiple Azure Cosmos DB collections tooa single Azure Cosmos DB collection
    dt.exe /s:CosmosDB /s.ConnectionString:"AccountEndpoint=<CosmosDB Endpoint>;AccountKey=<CosmosDB Key>;Database=<CosmosDB Database>;" /s.Collection:comp1|comp2|comp3|comp4 /t:CosmosDBBulk /t.ConnectionString:"AccountEndpoint=<CosmosDB Endpoint>;AccountKey=<CosmosDB Key>;Database=<CosmosDB Database>;" /t.Collection:singleCollection /t.CollectionThroughput:2500

    #Export an Azure Cosmos DB collection tooa JSON file
    dt.exe /s:CosmosDB /s.ConnectionString:"AccountEndpoint=<CosmosDB Endpoint>;AccountKey=<CosmosDB Key>;Database=<CosmosDB Database>;" /s.Collection:StoresSub /t:JsonFile /t.File:StoresExport.json /t.Overwrite /t.CollectionThroughput:2500

> [!TIP]
> hello Azure Cosmos DB Data-Importverktyget har också stöd för import av data från hello [Azure Cosmos DB emulatorn](local-emulator.md). När du importerar data från en lokal emulator, ange hello slutpunkt för`https://localhost:<port>`. 
> 
> 

## <a id="HBaseSource"></a>tooimport från HBase
Hej HBase källa Importverktyget alternativet kan du tooimport data från en HBase-tabell och du kan också filtrera hello data. Flera mallar tillhandahålls så att ställa in en import är så enkel som möjligt.

![Skärmbild av HBase alternativ](./media/import-data/hbasesource1.png)

![Skärmbild av HBase alternativ](./media/import-data/hbasesource2.png)

Hej HBase Stargate anslutningssträngen hello format är:

    ServiceURL=<server-address>;Username=<username>;Password=<password>

> [!NOTE]
> Använd hello Kontrollera kommandot tooensure som hello HBase-instans som angetts i hello anslutningsfältet sträng kan nås.
> 
> 

Här är en kommandorad exempel tooimport från HBase:

    dt.exe /s:HBase /s.ConnectionString:ServiceURL=<server-address>;Username=<username>;Password=<password> /s.Table:Contacts /t:CosmosDBBulk /t.ConnectionString:"AccountEndpoint=<CosmosDB Endpoint>;AccountKey=<CosmosDB Key>;Database=<CosmosDB Database>;" /t.Collection:hbaseimport

## <a id="DocumentDBBulkTarget"></a>tooimport toohello DocumentDB-API (massimport)
hello Azure Cosmos DB Bulk Importverktyget kan tooimport från någon av hello tillgängliga alternativ för datakällor med hjälp av en Azure Cosmos DB lagrade proceduren för effektivitet. hello-verktyget stöder tooone en partitionerad Azure Cosmos DB importsamlingen samt delat import genom vilken data är partitionerad över flera samlingar för en partitionerad Azure Cosmos DB. Mer information om partitionering data finns [partitionering och skalning i Azure Cosmos DB](partition-data.md). hello verktyget skapar, köra och ta sedan bort hello lagrade procedur från hello mål samling(ar).  

![Skärmbild av Azure Cosmos DB bulk-alternativ](./media/import-data/documentdbbulk.png)

hello Azure Cosmos DB-anslutningssträngen hello format är:

    AccountEndpoint=<CosmosDB Endpoint>;AccountKey=<CosmosDB Key>;Database=<CosmosDB Database>;

hello Azure Cosmos DB anslutningssträngen för kontot som kan hämtas från hello nycklar bladet för hello Azure-portalen, enligt beskrivningen i [hur toomanage ett konto i Azure Cosmos DB](manage-account.md), men hello namnet på databasen som hello måste toobe läggs toohello anslutningssträngen i hello följande format:

    Database=<CosmosDB Database>;

> [!NOTE]
> Använd hello Kontrollera kommandot tooensure som hello Azure DB som Cosmos-instans som anges i hello anslutningsfältet sträng kan nås.
> 
> 

tooimport tooa enkel samling, anger hello namnet på samlingen hello toowhich data importeras och klicka hello Lägg till. tooimport toomultiple samlingar, ange varje samlingsnamn individuellt eller hello följande syntax toospecify flera samlingar: *collection_prefix*[startIndex - end index]. Tänk hello följande när du anger flera samlingar via hello ovannämnda syntax:

1. Endast heltal intervallet namnet mönster stöds. Till exempel ange samlingen [0-3] genererar hello följande samlingar: collection0, collection1, collection2 collection3.
2. Du kan använda en förkortad syntax: samlingen [3] genererar samma uppsättning samlingar som nämns i steg 1.
3. Mer än en ersättning kan anges. Till exempel samlingen [0-1] [0-9] genererar 20 samlingsnamn med nollor (collection01... 02... 03).

När hello samling namn har angetts, välja hello önskade genomflödet i hello samling(ar) (400 RUs too10, 000 RUs). Välj en högre genomströmning för bästa prestanda för import. Läs mer om prestandanivåer [prestandanivåer i Azure Cosmos DB](performance-levels.md).

> [!NOTE]
> hello prestanda genomströmning inställningen gäller endast toocollection skapas. Om hello har angetts finns redan i samlingen, ändras inte dess genomflöde.
> 
> 

När du importerar toomultiple samlingar stöder hello-Importverktyget hash-värde baserat horisontell partitionering. I det här scenariot, ange hello dokumentegenskap gärna toouse som hello partitionsnyckel (om Partitionsnyckeln är tomt dokument är delat slumpmässigt över hello mål samlingarna).

Alternativt kan du ange vilket fält i hello importera källa som ska användas som hello Azure Cosmos DB dokumentet id-egenskap under hello import (Observera att om dokument inte innehåller den här egenskapen, sedan hello Importverktyget att generera ett GUID som egenskapsvärde för hello-id).

Det finns ett antal avancerade alternativ under importen. Först hello verktyget innehåller en standard massimport lagrad procedur (BulkInsert.js), kan du välja toospecify importera lagrade proceduren:

 ![Skärmbild av Azure Cosmos DB bulk insert sproc alternativet](./media/import-data/bulkinsertsp.png)

När du importerar datum typer (t.ex. från SQL Server eller MongoDB) kan välja du dessutom mellan tre importalternativ:

 ![Skärmbild av Azure Cosmos DB datum tid importalternativ](./media/import-data/datetimeoptions.png)

* Sträng: Spara som ett strängvärde
* Epok: Spara som en epok numeriskt värde
* Både: Spara både sträng och epok numeriska värden. Det här alternativet skapas en underdokument, till exempel: ”date_joined”: {”värde” ”: 2013-10-21T21:17:25.2410000Z” ”, epok”: 1382390245}

hello Azure Cosmos DB Bulk Importverktyget har hello följande ytterligare avancerade alternativ:

1. Batchstorlek: hello verktyget standardvärden tooa batchstorlek 50.  Om hello dokument toobe importeras är stor kan du sänka hello batchstorlek. Om hello dokument toobe importeras är liten kan du däremot du höja hello batchstorlek.
2. Maxstorlek för skript (byte): hello verktyget standardvärden tooa max skript storleken på 512KB
3. Inaktivera automatisk generering: Om varje dokument toobe importeras innehåller ett ID-fält kan det här alternativet kan öka prestanda. Kommer inte att importera dokument som har ett unikt id-fält saknas.
4. Uppdatera befintliga dokument: hello verktyget standardvärden toonot ersätta befintliga dokument med id-konflikter. Det här alternativet kan skriva över befintliga dokument med matchande ID: n. Den här funktionen är användbart för schemalagd datauppsättning migreringar som uppdaterar befintliga dokument.
5. Antalet återförsök vid fel: Anger hello antal gånger tooretry hello anslutning tooAzure Cosmos DB vid tillfälligt fel (t.ex. anslutningen avbrott i nätverket).
6. Återförsöksintervall: Anger hur länge toowait mellan du försöker hello anslutning tooAzure Cosmos DB vid tillfälligt fel (t.ex. anslutningen avbrott i nätverket).
7. Anslutningsläge: Anger hello anslutning läge toouse med Azure Cosmos DB. hello tillgängliga alternativen är DirectTcp, DirectHttps och Gateway. hello direktanslutning lägen är snabbare och medan hello gateway läge är mer brandvägg eget eftersom den endast använder port 443.

![Skärmbild av Azure Cosmos DB massimport avancerade alternativ](./media/import-data/docdbbulkoptions.png)

> [!TIP]
> hello importläge verktyget standardvärden tooconnection DirectTcp. Om det uppstår brandväggsproblem med Växla tooconnection läge Gateway, som kräver endast port 443.
> 
> 

## <a id="DocumentDBSeqTarget"></a>tooimport toohello DocumentDB-API (sekventiella post Import)
hello Azure Cosmos DB sekventiella post Importverktyget kan tooimport från någon av hello tillgängliga alternativ på en post med basis. Du kan välja det här alternativet om du importerar tooan befintlig samling som har uppnått sin kvot av lagrade procedurer. hello verktyget stöder import tooa enkel (enskild partition och flera partition) Azure DB som Cosmos-samling som delat import genom vilken data är partitionerad över flera enskild partition och/eller flera partition Azure DB som Cosmos-samlingar. Mer information om partitionering data finns [partitionering och skalning i Azure Cosmos DB](partition-data.md).

![Skärmbild av Azure Cosmos DB importalternativ för sekventiella poster](./media/import-data/documentdbsequential.png)

hello Azure Cosmos DB-anslutningssträngen hello format är:

    AccountEndpoint=<CosmosDB Endpoint>;AccountKey=<CosmosDB Key>;Database=<CosmosDB Database>;

hello Azure Cosmos DB anslutningssträngen för kontot som kan hämtas från hello nycklar bladet för hello Azure-portalen, enligt beskrivningen i [hur toomanage ett konto i Azure Cosmos DB](manage-account.md), men hello namnet på databasen som hello måste toobe läggs toohello anslutningssträngen i hello följande format:

    Database=<Azure Cosmos DB Database>;

> [!NOTE]
> Använd hello Kontrollera kommandot tooensure som hello Azure DB som Cosmos-instans som anges i hello anslutningsfältet sträng kan nås.
> 
> 

tooimport tooa enkel samling, anger hello namnet på samlingen hello toowhich data importeras och klicka hello Lägg till. tooimport toomultiple samlingar, ange varje samlingsnamn individuellt eller hello följande syntax toospecify flera samlingar: *collection_prefix*[startIndex - end index]. Tänk hello följande när du anger flera samlingar via hello ovannämnda syntax:

1. Endast heltal intervallet namnet mönster stöds. Till exempel ange samlingen [0-3] genererar hello följande samlingar: collection0, collection1, collection2 collection3.
2. Du kan använda en förkortad syntax: samlingen [3] genererar samma uppsättning samlingar som nämns i steg 1.
3. Mer än en ersättning kan anges. Till exempel samlingen [0-1] [0-9] genererar 20 samlingsnamn med nollor (collection01... 02... 03).

När hello samling namn har angetts, välja hello önskade genomflödet i hello samling(ar) (400 RUs too250, 000 RUs). Välj en högre genomströmning för bästa prestanda för import. Läs mer om prestandanivåer [prestandanivåer i Azure Cosmos DB](performance-levels.md). Någon importera toocollections med genomströmning > 10 000 RUs kräver en partitionsnyckel. Om du väljer toohave 250 000 RUs behöver toofile en begäran i hello portal toohave ökat för ditt konto.

> [!NOTE]
> hello genomströmning inställningen gäller endast toocollection skapas. Om hello har angetts finns redan i samlingen, ändras inte dess genomflöde.
> 
> 

När du importerar toomultiple samlingar stöder hello-Importverktyget hash-värde baserat horisontell partitionering. I det här scenariot, ange hello dokumentegenskap gärna toouse som hello partitionsnyckel (om Partitionsnyckeln är tomt dokument är delat slumpmässigt över hello mål samlingarna).

Alternativt kan du ange vilket fält i hello importera källa som ska användas som hello Azure Cosmos DB dokumentet id-egenskap under hello import (Observera att om dokument inte innehåller den här egenskapen, sedan hello Importverktyget att generera ett GUID som egenskapsvärde för hello-id).

Det finns ett antal avancerade alternativ under importen. När du importerar datum typer (t.ex. från SQL Server eller MongoDB) måste välja du först mellan tre importalternativ:

 ![Skärmbild av Azure Cosmos DB datum tid importalternativ](./media/import-data/datetimeoptions.png)

* Sträng: Spara som ett strängvärde
* Epok: Spara som en epok numeriskt värde
* Både: Spara både sträng och epok numeriska värden. Det här alternativet skapas en underdokument, till exempel: ”date_joined”: {”värde” ”: 2013-10-21T21:17:25.2410000Z” ”, epok”: 1382390245}

hello Azure Cosmos DB - sekventiella post Importverktyget har hello följande ytterligare avancerade alternativ:

1. Antalet parallella begäranden: hello verktyget too2 parallella begäranden som standard. Överväg att höja hello antalet parallella begäranden om hello dokument toobe importeras är små. Observera att om det här antalet aktiveras för mycket hello importera uppstå begränsning.
2. Inaktivera automatisk generering: Om varje dokument toobe importeras innehåller ett ID-fält kan det här alternativet kan öka prestanda. Kommer inte att importera dokument som har ett unikt id-fält saknas.
3. Uppdatera befintliga dokument: hello verktyget standardvärden toonot ersätta befintliga dokument med id-konflikter. Det här alternativet kan skriva över befintliga dokument med matchande ID: n. Den här funktionen är användbart för schemalagd datauppsättning migreringar som uppdaterar befintliga dokument.
4. Antalet återförsök vid fel: Anger hello antal gånger tooretry hello anslutning tooAzure Cosmos DB vid tillfälligt fel (t.ex. anslutningen avbrott i nätverket).
5. Återförsöksintervall: Anger hur länge toowait mellan du försöker hello anslutning tooAzure Cosmos DB vid tillfälligt fel (t.ex. anslutningen avbrott i nätverket).
6. Anslutningsläge: Anger hello anslutning läge toouse med Azure Cosmos DB. hello tillgängliga alternativen är DirectTcp, DirectHttps och Gateway. hello direktanslutning lägen är snabbare och medan hello gateway läge är mer brandvägg eget eftersom den endast använder port 443.

![Skärmbild av Azure Cosmos DB sekventiella post importera avancerade alternativ](./media/import-data/documentdbsequentialoptions.png)

> [!TIP]
> hello importläge verktyget standardvärden tooconnection DirectTcp. Om det uppstår brandväggsproblem med Växla tooconnection läge Gateway, som kräver endast port 443.
> 
> 

## <a id="IndexingPolicy"></a>Ange ett indexprincip när du skapar Azure Cosmos DB samlingar
Du kan ange hello indexprincip av hello samlingar när du tillåter hello migrering verktyget toocreate samlingar under importen. Navigera toohello indexering principavdelningen i hello avancerade alternativ avsnittet hello Azure Cosmos DB massimport och Azure Cosmos DB sekventiella post alternativ.

![Skärmbild av Azure Cosmos DB indexering princip avancerade alternativ](./media/import-data/indexingpolicy1.png)

Använder hello indexering avancerade alternativ, kan du välja en indexering principfil, manuellt ange ett indexprincip, eller välj från en uppsättning standardmallar (genom att högerklicka i hello indexering princip textruta).

hello principmallar hello verktyget tillhandahåller är:

* Som standard. Den här principen är bäst när du utför likhetsfrågor för strängar och använda ORDER BY, intervalls och likhetsfrågor för tal. Den här principen har en lägre Omkostnad för indexlagring än intervall.
* Intervall. Den här principen är bäst om du använder ORDER BY, intervalls och likhetsfrågor för både tal och strängar. Den här principen har en högre Omkostnad för indexlagring än standard eller Hash.

![Skärmbild av Azure Cosmos DB indexering princip avancerade alternativ](./media/import-data/indexingpolicy2.png)

> [!NOTE]
> Om du inte anger en indexprincip tillämpas hello standardprincipen. Mer information om principer för fulltextindexering finns [Azure Cosmos DB indexering principer](indexing-policies.md).
> 
> 

## <a name="export-toojson-file"></a>Exportera tooJSON fil
hello Azure Cosmos DB JSON Exportverktyget kan du tooexport någon hello tillgänglig källa alternativ tooa JSON-fil som innehåller en matris av JSON-dokument. hello verktyget hanterar hello export du eller väljer tooview hello resulterande migrering kommandot och kör hello kommando själv. hello resulterande JSON-fil kan sparas lokalt eller i Azure Blob storage.

![Skärmbild av Azure Cosmos DB JSON lokal fil exportalternativ](./media/import-data/jsontarget.png)

![Skärmbild av Azure Cosmos DB JSON Azure Blob storage-exportalternativ](./media/import-data/jsontarget2.png)

Du kan välja tooprettify hello resulterande JSON som ökar hello storlek på hello resulterande dokument medan gör hello innehåll mer läsbar.

    Standard JSON export
    [{"id":"Sample","Title":"About Paris","Language":{"Name":"English"},"Author":{"Name":"Don","Location":{"City":"Paris","Country":"France"}},"Content":"Don's document in Azure Cosmos DB is a valid JSON document as defined by hello JSON spec.","PageViews":10000,"Topics":[{"Title":"History of Paris"},{"Title":"Places toosee in Paris"}]}]

    Prettified JSON export
    [
     {
    "id": "Sample",
    "Title": "About Paris",
    "Language": {
      "Name": "English"
    },
    "Author": {
      "Name": "Don",
      "Location": {
        "City": "Paris",
        "Country": "France"
      }
    },
    "Content": "Don's document in Azure Cosmos DB is a valid JSON document as defined by hello JSON spec.",
    "PageViews": 10000,
    "Topics": [
      {
        "Title": "History of Paris"
      },
      {
        "Title": "Places toosee in Paris"
      }
    ]
    }]

## <a name="advanced-configuration"></a>Avancerad konfiguration
Ange hello platsen för hello log file toowhich du vill att eventuella fel som skrivs i hello avancerad konfigurationsskärmen. hello följande regler gäller toothis sidan:

1. Om ett filnamn inte anges returneras alla fel på hello resultatsidan.
2. Om ett filnamn anges utan en katalog, kommer sedan hello filen att skapas (eller skrivs över) i miljön hello katalogen.
3. Om du väljer en befintlig fil och sedan hello filen kommer att skrivas över finns det inget alternativ för Lägg till.

Välj om alla toolog, kritisk, eller inga felmeddelanden. Slutligen besluta hur ofta hello på skärmen överföring meddelandet kommer att uppdateras med dess förlopp.

    ![Screenshot of Advanced configuration screen](./media/import-data/AdvancedConfiguration.png)

## <a name="confirm-import-settings-and-view-command-line"></a>Bekräfta importera inställningar och visa kommandoraden
1. När du har angett information om källdatorn och målet information avancerad konfiguration, granska hello migrering sammanfattning och du kan också visa/kopiera hello resulterande migrering kommando (kopiera hello-kommandot är användbara tooautomate importåtgärder):
   
    ![Skärmbild av översiktsskärm](./media/import-data/summary.png)
   
    ![Skärmbild av översiktsskärm](./media/import-data/summarycommand.png)
2. När du är nöjd med din käll- och alternativ klickar du på **importera**. hello förfluten tid, överförda antal och felinformation (om du inte anger ett filnamn i hello avancerad konfiguration) uppdateras när hello importen pågår. När installationen är klar kan du exportera hello resultat (t.ex. toodeal med importfel).
   
    ![Skärmbild av Azure Cosmos DB JSON exportalternativ](./media/import-data/viewresults.png)
3. Du kan också starta en ny import antingen behålla hello befintliga inställningar (t.ex. anslutning sträng information, källa och mål för val osv.) eller återställa alla värden.
   
    ![Skärmbild av Azure Cosmos DB JSON exportalternativ](./media/import-data/newimport.png)

## <a name="next-steps"></a>Nästa steg

I den här kursen har du gjort hello följande:

> [!div class="checklist"]
> * Installerat hello datamigreringsverktyget
> * Importerade data från olika datakällor
> * Exporterats från Azure Cosmos DB tooJSON

Du kan nu fortsätta toohello nästa kurs och lära dig hur tooquery data med hjälp av Azure Cosmos DB. 

> [!div class="nextstepaction"]
>[Hur tooquery data?](../cosmos-db/tutorial-query-documentdb.md)
