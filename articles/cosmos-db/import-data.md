---
title: "Verktyg för Azure Cosmos DB | Microsoft Docs"
description: "Lär dig använda Migreringsverktyg för öppen källkod Azure Cosmos DB data för att importera data till Azure Cosmos DB från olika källor, inklusive MongoDB, SQL Server, Table storage, Amazon DynamoDB, CSV och JSON-filer. CSV-fil för JSON-konvertering."
keywords: "CSV-fil för json Migreringsverktyg för databasen, konvertera csv till json"
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
ms.openlocfilehash: 23a4a82dbdb611f4da90562af936fca28da9b24d
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="how-to-import-data-into-azure-cosmos-db-for-the-documentdb-api"></a><span data-ttu-id="bcc86-105">Hur du importerar data till Azure Cosmos DB DocumentDB-API: t?</span><span class="sxs-lookup"><span data-stu-id="bcc86-105">How to import data into Azure Cosmos DB for the DocumentDB API?</span></span>

<span data-ttu-id="bcc86-106">Den här självstudiekursen innehåller instruktioner om hur du använder Azure Cosmos DB: DocumentDB API datamigrering verktyg som kan importera data från olika källor, inklusive JSON-filer, CSV-filer, SQL, MongoDB, Azure Table storage, Amazon DynamoDB och Azure Cosmos DB DocumentDB API samlingar i samlingar för användning med Azure Cosmos DB och DocumentDB-API.</span><span class="sxs-lookup"><span data-stu-id="bcc86-106">This tutorial provides instructions on using the Azure Cosmos DB: DocumentDB API Data Migration tool, which can import data from various sources, including JSON files, CSV files, SQL, MongoDB, Azure Table storage, Amazon DynamoDB and Azure Cosmos DB DocumentDB API collections into collections for use with Azure Cosmos DB and the DocumentDB API.</span></span> <span data-ttu-id="bcc86-107">Verktyget migrering av Data kan också användas när du migrerar från en enda partition samling till en samling med flera partition för DocumentDB-API.</span><span class="sxs-lookup"><span data-stu-id="bcc86-107">The Data Migration tool can also be used when migrating from a single partition collection to a multi-partition collection for the DocumentDB API.</span></span>

<span data-ttu-id="bcc86-108">Verktyget datamigrering fungerar endast när importera data till Azure Cosmos DB för använder med DocumentDB-API.</span><span class="sxs-lookup"><span data-stu-id="bcc86-108">The Data Migration tool only works when importing data into Azure Cosmos DB for use with the DocumentDB API.</span></span> <span data-ttu-id="bcc86-109">Importerar data för tabell API eller Graph API stöds inte just nu.</span><span class="sxs-lookup"><span data-stu-id="bcc86-109">Importing data for use with the Table API or Graph API is not supported at this time.</span></span> 

<span data-ttu-id="bcc86-110">När du importerar data för användning med MongoDB-API finns [Azure Cosmos DB: hur du migrerar data MongoDB-API: t?](mongodb-migrate.md).</span><span class="sxs-lookup"><span data-stu-id="bcc86-110">To import data for use with the MongoDB API, see [Azure Cosmos DB: How to migrate data for the MongoDB API?](mongodb-migrate.md).</span></span>

<span data-ttu-id="bcc86-111">Den här kursen ingår följande uppgifter:</span><span class="sxs-lookup"><span data-stu-id="bcc86-111">This tutorial covers the following tasks:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="bcc86-112">Installera verktyget datamigrering</span><span class="sxs-lookup"><span data-stu-id="bcc86-112">Installing the Data Migration tool</span></span>
> * <span data-ttu-id="bcc86-113">Importera data från olika datakällor</span><span class="sxs-lookup"><span data-stu-id="bcc86-113">Importing data from different data sources</span></span>
> * <span data-ttu-id="bcc86-114">Exportera från Azure Cosmos DB till JSON</span><span class="sxs-lookup"><span data-stu-id="bcc86-114">Exporting from Azure Cosmos DB to JSON</span></span>

## <span data-ttu-id="bcc86-115"><a id="Prerequisites"></a>Förhandskrav</span><span class="sxs-lookup"><span data-stu-id="bcc86-115"><a id="Prerequisites"></a>Prerequisites</span></span>
<span data-ttu-id="bcc86-116">Innan du följer anvisningarna i den här artikeln bör du kontrollera att du har följande installerat:</span><span class="sxs-lookup"><span data-stu-id="bcc86-116">Before following the instructions in this article, ensure that you have the following installed:</span></span>

* <span data-ttu-id="bcc86-117">[Microsoft .NET Framework 4.51](https://www.microsoft.com/download/developer-tools.aspx) eller högre.</span><span class="sxs-lookup"><span data-stu-id="bcc86-117">[Microsoft .NET Framework 4.51](https://www.microsoft.com/download/developer-tools.aspx) or higher.</span></span>

## <span data-ttu-id="bcc86-118"><a id="Overviewl"></a>Översikt över verktyget datamigrering</span><span class="sxs-lookup"><span data-stu-id="bcc86-118"><a id="Overviewl"></a>Overview of the Data Migration tool</span></span>
<span data-ttu-id="bcc86-119">Verktyget datamigrering är en öppen källkod som importerar data till Azure Cosmos DB från olika källor, inklusive:</span><span class="sxs-lookup"><span data-stu-id="bcc86-119">The Data Migration tool is an open source solution that imports data to Azure Cosmos DB from a variety of sources, including:</span></span>

* <span data-ttu-id="bcc86-120">JSON-filer</span><span class="sxs-lookup"><span data-stu-id="bcc86-120">JSON files</span></span>
* <span data-ttu-id="bcc86-121">MongoDB</span><span class="sxs-lookup"><span data-stu-id="bcc86-121">MongoDB</span></span>
* <span data-ttu-id="bcc86-122">SQL Server</span><span class="sxs-lookup"><span data-stu-id="bcc86-122">SQL Server</span></span>
* <span data-ttu-id="bcc86-123">CSV-filer</span><span class="sxs-lookup"><span data-stu-id="bcc86-123">CSV files</span></span>
* <span data-ttu-id="bcc86-124">Azure Table Storage</span><span class="sxs-lookup"><span data-stu-id="bcc86-124">Azure Table storage</span></span>
* <span data-ttu-id="bcc86-125">Amazon DynamoDB</span><span class="sxs-lookup"><span data-stu-id="bcc86-125">Amazon DynamoDB</span></span>
* <span data-ttu-id="bcc86-126">HBase</span><span class="sxs-lookup"><span data-stu-id="bcc86-126">HBase</span></span>
* <span data-ttu-id="bcc86-127">Azure DB Cosmos-samlingar</span><span class="sxs-lookup"><span data-stu-id="bcc86-127">Azure Cosmos DB collections</span></span>

<span data-ttu-id="bcc86-128">Medan Importverktyget innehåller ett grafiskt användargränssnitt (dtui.exe), kan den också drivas från kommandoraden (dt.exe).</span><span class="sxs-lookup"><span data-stu-id="bcc86-128">While the import tool includes a graphical user interface (dtui.exe), it can also be driven from the command line (dt.exe).</span></span> <span data-ttu-id="bcc86-129">Faktum är är ett alternativ till utdata associerat kommando när du har installerat en import via Användargränssnittet.</span><span class="sxs-lookup"><span data-stu-id="bcc86-129">In fact, there is an option to output the associated command after setting up an import through the UI.</span></span> <span data-ttu-id="bcc86-130">Tabell källdata (t.ex. SQL Server- eller CSV-filer) kan omvandlas så att hierarkiska relationer (underdokument) kan skapas under importen.</span><span class="sxs-lookup"><span data-stu-id="bcc86-130">Tabular source data (e.g. SQL Server or CSV files) can be transformed such that hierarchical relationships (subdocuments) can be created during import.</span></span> <span data-ttu-id="bcc86-131">Vill du fortsätta läsa vill lära dig mer om alternativ för exempel på kommandorader för att importera från varje källa och mål alternativ visning importera resultat.</span><span class="sxs-lookup"><span data-stu-id="bcc86-131">Keep reading to learn more about source options, sample command lines to import from each source, target options, and viewing import results.</span></span>

## <span data-ttu-id="bcc86-132"><a id="Install"></a>Installera verktyget för migrering av Data</span><span class="sxs-lookup"><span data-stu-id="bcc86-132"><a id="Install"></a>Install the Data Migration tool</span></span>
<span data-ttu-id="bcc86-133">Källkoden för migrering verktyget finns på GitHub i [den här lagringsplatsen](https://github.com/azure/azure-documentdb-datamigrationtool) och en kompilerad version är tillgänglig från [Microsoft Download Center](http://www.microsoft.com/downloads/details.aspx?FamilyID=cda7703a-2774-4c07-adcc-ad02ddc1a44d).</span><span class="sxs-lookup"><span data-stu-id="bcc86-133">The migration tool source code is available on GitHub in [this repository](https://github.com/azure/azure-documentdb-datamigrationtool) and a compiled version is available from [Microsoft Download Center](http://www.microsoft.com/downloads/details.aspx?FamilyID=cda7703a-2774-4c07-adcc-ad02ddc1a44d).</span></span> <span data-ttu-id="bcc86-134">Du kan sammanställa lösningen eller bara ladda ned och extrahera den kompilerade versionen till en katalog som du väljer.</span><span class="sxs-lookup"><span data-stu-id="bcc86-134">You may either compile the solution or simply download and extract the compiled version to a directory of your choice.</span></span> <span data-ttu-id="bcc86-135">Kör sedan antingen:</span><span class="sxs-lookup"><span data-stu-id="bcc86-135">Then run either:</span></span>

* <span data-ttu-id="bcc86-136">**Dtui.exe**: grafiskt gränssnittsversionen av verktyget</span><span class="sxs-lookup"><span data-stu-id="bcc86-136">**Dtui.exe**: Graphical interface version of the tool</span></span>
* <span data-ttu-id="bcc86-137">**DT.exe**: kommandoraden versionen av verktyget</span><span class="sxs-lookup"><span data-stu-id="bcc86-137">**Dt.exe**: Command-line version of the tool</span></span>

## <a name="import-data"></a><span data-ttu-id="bcc86-138">Importera data</span><span class="sxs-lookup"><span data-stu-id="bcc86-138">Import data</span></span>

<span data-ttu-id="bcc86-139">När du har installerat verktyget är det dags att importera dina data.</span><span class="sxs-lookup"><span data-stu-id="bcc86-139">Once you've installed the tool, it's time to import your data.</span></span> <span data-ttu-id="bcc86-140">Vilken typ av data du vill importera?</span><span class="sxs-lookup"><span data-stu-id="bcc86-140">What kind of data do you want to import?</span></span>

* [<span data-ttu-id="bcc86-141">JSON-filer</span><span class="sxs-lookup"><span data-stu-id="bcc86-141">JSON files</span></span>](#JSON)
* [<span data-ttu-id="bcc86-142">MongoDB</span><span class="sxs-lookup"><span data-stu-id="bcc86-142">MongoDB</span></span>](#MongoDB)
* [<span data-ttu-id="bcc86-143">MongoDB exportfilerna</span><span class="sxs-lookup"><span data-stu-id="bcc86-143">MongoDB Export files</span></span>](#MongoDBExport)
* [<span data-ttu-id="bcc86-144">SQL Server</span><span class="sxs-lookup"><span data-stu-id="bcc86-144">SQL Server</span></span>](#SQL)
* [<span data-ttu-id="bcc86-145">CSV-filer</span><span class="sxs-lookup"><span data-stu-id="bcc86-145">CSV files</span></span>](#CSV)
* [<span data-ttu-id="bcc86-146">Azure Table Storage</span><span class="sxs-lookup"><span data-stu-id="bcc86-146">Azure Table storage</span></span>](#AzureTableSource)
* [<span data-ttu-id="bcc86-147">Amazon DynamoDB</span><span class="sxs-lookup"><span data-stu-id="bcc86-147">Amazon DynamoDB</span></span>](#DynamoDBSource)
* [<span data-ttu-id="bcc86-148">BLOB</span><span class="sxs-lookup"><span data-stu-id="bcc86-148">Blob</span></span>](#BlobImport)
* [<span data-ttu-id="bcc86-149">Azure DB Cosmos-samlingar</span><span class="sxs-lookup"><span data-stu-id="bcc86-149">Azure Cosmos DB collections</span></span>](#DocumentDBSource)
* [<span data-ttu-id="bcc86-150">HBase</span><span class="sxs-lookup"><span data-stu-id="bcc86-150">HBase</span></span>](#HBaseSource)
* [<span data-ttu-id="bcc86-151">Azure DB Cosmos-massimport</span><span class="sxs-lookup"><span data-stu-id="bcc86-151">Azure Cosmos DB bulk import</span></span>](#DocumentDBBulkImport)
* [<span data-ttu-id="bcc86-152">Azure DB Cosmos sekventiella post import</span><span class="sxs-lookup"><span data-stu-id="bcc86-152">Azure Cosmos DB sequential record import</span></span>](#DocumentDSeqTarget)


## <span data-ttu-id="bcc86-153"><a id="JSON"></a>Så här importerar du JSON-filer</span><span class="sxs-lookup"><span data-stu-id="bcc86-153"><a id="JSON"></a>To import JSON files</span></span>
<span data-ttu-id="bcc86-154">JSON-filen källa Importverktyget alternativet kan du importera en eller flera dokument JSON-filer eller JSON-filer att var och en innehåller en matris av JSON-dokument.</span><span class="sxs-lookup"><span data-stu-id="bcc86-154">The JSON file source importer option allows you to import one or more single document JSON files or JSON files that each contain an array of JSON documents.</span></span> <span data-ttu-id="bcc86-155">När du lägger till mapparna som innehåller JSON-filer som ska importeras, har du möjlighet att rekursivt söker efter filer i undermappar.</span><span class="sxs-lookup"><span data-stu-id="bcc86-155">When adding folders that contain JSON files to import, you have the option of recursively searching for files in subfolders.</span></span>

![Skärmbild av JSON-Filalternativ - Databasverktyg för migrering](./media/import-data/jsonsource.png)

<span data-ttu-id="bcc86-157">Här följer några exempel som kommandoraden ska importera JSON-filer:</span><span class="sxs-lookup"><span data-stu-id="bcc86-157">Here are some command line samples to import JSON files:</span></span>

    #Import a single JSON file
    dt.exe /s:JsonFile /s.Files:.\Sessions.json /t:CosmosDBBulk /t.ConnectionString:"AccountEndpoint=<CosmosDB Endpoint>;AccountKey=<CosmosDB Key>;Database=<CosmosDB Database>;" /t.Collection:Sessions /t.CollectionThroughput:2500

    #Import a directory of JSON files
    dt.exe /s:JsonFile /s.Files:C:\TESessions\*.json /t:CosmosDBBulk /t.ConnectionString:" AccountEndpoint=<CosmosDB Endpoint>;AccountKey=<CosmosDB Key>;Database=<CosmosDB Database>;" /t.Collection:Sessions /t.CollectionThroughput:2500

    #Import a directory (including sub-directories) of JSON files
    dt.exe /s:JsonFile /s.Files:C:\LastFMMusic\**\*.json /t:CosmosDBBulk /t.ConnectionString:" AccountEndpoint=<CosmosDB Endpoint>;AccountKey=<CosmosDB Key>;Database=<CosmosDB Database>;" /t.Collection:Music /t.CollectionThroughput:2500

    #Import a directory (single), directory (recursive), and individual JSON files
    dt.exe /s:JsonFile /s.Files:C:\Tweets\*.*;C:\LargeDocs\**\*.*;C:\TESessions\Session48172.json;C:\TESessions\Session48173.json;C:\TESessions\Session48174.json;C:\TESessions\Session48175.json;C:\TESessions\Session48177.json /t:CosmosDBBulk /t.ConnectionString:"AccountEndpoint=<CosmosDB Endpoint>;AccountKey=<CosmosDB Key>;Database=<CosmosDB Database>;" /t.Collection:subs /t.CollectionThroughput:2500

    #Import a single JSON file and partition the data across 4 collections
    dt.exe /s:JsonFile /s.Files:D:\\CompanyData\\Companies.json /t:CosmosDBBulk /t.ConnectionString:"AccountEndpoint=<CosmosDB Endpoint>;AccountKey=<CosmosDB Key>;Database=<CosmosDB Database>;" /t.Collection:comp[1-4] /t.PartitionKey:name /t.CollectionThroughput:2500

## <span data-ttu-id="bcc86-158"><a id="MongoDB"></a>Importera från MongoDB</span><span class="sxs-lookup"><span data-stu-id="bcc86-158"><a id="MongoDB"></a>To import from MongoDB</span></span>

> [!IMPORTANT]
> <span data-ttu-id="bcc86-159">Om du importerar till ett Azure DB som Cosmos-konto med stöd för MongoDB, följer du dessa [instruktioner](mongodb-migrate.md).</span><span class="sxs-lookup"><span data-stu-id="bcc86-159">If you are importing to an Azure Cosmos DB account with Support for MongoDB, follow these [instructions](mongodb-migrate.md).</span></span>
> 
> 

<span data-ttu-id="bcc86-160">MongoDB källa Importverktyget alternativet kan du importera från en enskild MongoDB-samling och du kan också filtrera dokument med hjälp av en fråga och/eller ändra dokumentets struktur med hjälp av en projektion.</span><span class="sxs-lookup"><span data-stu-id="bcc86-160">The MongoDB source importer option allows you to import from an individual MongoDB collection and optionally filter documents using a query and/or modify the document structure by using a projection.</span></span>  

![Skärmbild av MongoDB alternativ](./media/import-data/mongodbsource.png)

<span data-ttu-id="bcc86-162">Anslutningssträngen är i formatet standard MongoDB:</span><span class="sxs-lookup"><span data-stu-id="bcc86-162">The connection string is in the standard MongoDB format:</span></span>

    mongodb://<dbuser>:<dbpassword>@<host>:<port>/<database>

> [!NOTE]
> <span data-ttu-id="bcc86-163">Använd kommandot Kontrollera så att den angivna i strängen anslutningsfältet MongoDB-instansen är tillgänglig.</span><span class="sxs-lookup"><span data-stu-id="bcc86-163">Use the Verify command to ensure that the MongoDB instance specified in the connection string field can be accessed.</span></span>
> 
> 

<span data-ttu-id="bcc86-164">Ange namnet på samlingen som data ska importeras.</span><span class="sxs-lookup"><span data-stu-id="bcc86-164">Enter the name of the collection from which data will be imported.</span></span> <span data-ttu-id="bcc86-165">Du kan om du vill ange eller ange en fil för en fråga (t.ex. {pop: {$gt: 5000}}) och/eller projektion (t.ex. {loc:0}) att både filtrera och utforma data som ska importeras.</span><span class="sxs-lookup"><span data-stu-id="bcc86-165">You may optionally specify or provide a file for a query (e.g. {pop: {$gt:5000}} ) and/or projection (e.g. {loc:0} ) to both filter and shape the data to be imported.</span></span>

<span data-ttu-id="bcc86-166">Här följer några exempel som kommandoraden ska importera från MongoDB:</span><span class="sxs-lookup"><span data-stu-id="bcc86-166">Here are some command line samples to import from MongoDB:</span></span>

    #Import all documents from a MongoDB collection
    dt.exe /s:MongoDB /s.ConnectionString:mongodb://<dbuser>:<dbpassword>@<host>:<port>/<database> /s.Collection:zips /t:CosmosDBBulk /t.ConnectionString:"AccountEndpoint=<CosmosDB Endpoint>;AccountKey=<CosmosDB Key>;Database=<CosmosDB Database>;" /t.Collection:BulkZips /t.IdField:_id /t.CollectionThroughput:2500

    #Import documents from a MongoDB collection which match the query and exclude the loc field
    dt.exe /s:MongoDB /s.ConnectionString:mongodb://<dbuser>:<dbpassword>@<host>:<port>/<database> /s.Collection:zips /s.Query:{pop:{$gt:50000}} /s.Projection:{loc:0} /t:CosmosDBBulk /t.ConnectionString:"AccountEndpoint=<CosmosDB Endpoint>;AccountKey=<CosmosDB Key>;Database=<CosmosDB Database>;" /t.Collection:BulkZipsTransform /t.IdField:_id/t.CollectionThroughput:2500

## <span data-ttu-id="bcc86-167"><a id="MongoDBExport"></a>Så här importerar du MongoDB exportfilerna</span><span class="sxs-lookup"><span data-stu-id="bcc86-167"><a id="MongoDBExport"></a>To import MongoDB export files</span></span>

> [!IMPORTANT]
> <span data-ttu-id="bcc86-168">Om du importerar till ett Azure DB som Cosmos-konto med stöd för MongoDB, följer du dessa [instruktioner](mongodb-migrate.md).</span><span class="sxs-lookup"><span data-stu-id="bcc86-168">If you are importing to an Azure Cosmos DB account with support for MongoDB, follow these [instructions](mongodb-migrate.md).</span></span>
> 
> 

<span data-ttu-id="bcc86-169">MongoDB export JSON källa Importverktyget filalternativet kan du importera en eller flera JSON-filer som skapas från verktyget mongoexport.</span><span class="sxs-lookup"><span data-stu-id="bcc86-169">The MongoDB export JSON file source importer option allows you to import one or more JSON files produced from the mongoexport utility.</span></span>  

![Skärmbild av MongoDB exportalternativ för källa](./media/import-data/mongodbexportsource.png)

<span data-ttu-id="bcc86-171">När du lägger till mapparna som innehåller MongoDB export JSON-filer för import, har du möjlighet att rekursivt söker efter filer i undermappar.</span><span class="sxs-lookup"><span data-stu-id="bcc86-171">When adding folders that contain MongoDB export JSON files for import, you have the option of recursively searching for files in subfolders.</span></span>

<span data-ttu-id="bcc86-172">Här följer ett exempel på kommandoraden att importera från MongoDB export JSON-filer:</span><span class="sxs-lookup"><span data-stu-id="bcc86-172">Here is a command line sample to import from MongoDB export JSON files:</span></span>

    dt.exe /s:MongoDBExport /s.Files:D:\mongoemployees.json /t:CosmosDBBulk /t.ConnectionString:"AccountEndpoint=<CosmosDB Endpoint>;AccountKey=<CosmosDB Key>;Database=<CosmosDB Database>;" /t.Collection:employees /t.IdField:_id /t.Dates:Epoch /t.CollectionThroughput:2500

## <span data-ttu-id="bcc86-173"><a id="SQL"></a>Importera från SQL Server</span><span class="sxs-lookup"><span data-stu-id="bcc86-173"><a id="SQL"></a>To import from SQL Server</span></span>
<span data-ttu-id="bcc86-174">SQL-Importverktyget källalternativet kan du importera från en enskild SQL Server-databas och du kan också filtrera poster som ska importeras med hjälp av en fråga.</span><span class="sxs-lookup"><span data-stu-id="bcc86-174">The SQL source importer option allows you to import from an individual SQL Server database and optionally filter the records to be imported using a query.</span></span> <span data-ttu-id="bcc86-175">Du kan dessutom ändra dokumentets struktur genom att ange kapslade avgränsare (Mer information om det finns en liten stund).</span><span class="sxs-lookup"><span data-stu-id="bcc86-175">In addition, you can modify the document structure by specifying a nesting separator (more on that in a moment).</span></span>  

![Skärmbild av SQL - alternativ för databas Migreringsverktyg](./media/import-data/sqlexportsource.png)

<span data-ttu-id="bcc86-177">Formatet för anslutningssträngen är standard SQL connection-strängformat.</span><span class="sxs-lookup"><span data-stu-id="bcc86-177">The format of the connection string is the standard SQL connection string format.</span></span>

> [!NOTE]
> <span data-ttu-id="bcc86-178">Använd kommandot Kontrollera så att SQL Server-instansen som anges i strängen anslutningsfältet kan nås.</span><span class="sxs-lookup"><span data-stu-id="bcc86-178">Use the Verify command to ensure that the SQL Server instance specified in the connection string field can be accessed.</span></span>
> 
> 

<span data-ttu-id="bcc86-179">Egenskapen kapslade avgränsare används för att skapa hierarkiska relationer (underordnade dokument) under importen.</span><span class="sxs-lookup"><span data-stu-id="bcc86-179">The nesting separator property is used to create hierarchical relationships (sub-documents) during import.</span></span> <span data-ttu-id="bcc86-180">Överväg följande SQL-fråga:</span><span class="sxs-lookup"><span data-stu-id="bcc86-180">Consider the following SQL query:</span></span>

<span data-ttu-id="bcc86-181">*Välj OMVANDLINGEN (BusinessEntityID AS varchar) som Id, namn, adresstyp som [Address.AddressType], AddressLine1 som [Address.AddressLine1], ort som [Address.Location.City], StateProvinceName som [Address.Location.StateProvinceName], postnummer som [ Address.PostalCode] CountryRegionName som [Address.CountryRegionName] från Sales.vStoreWithAddresses där adresstyp = 'Huvudkontoret'*</span><span class="sxs-lookup"><span data-stu-id="bcc86-181">*select CAST(BusinessEntityID AS varchar) as Id, Name, AddressType as [Address.AddressType], AddressLine1 as [Address.AddressLine1], City as [Address.Location.City], StateProvinceName as [Address.Location.StateProvinceName], PostalCode as [Address.PostalCode], CountryRegionName as [Address.CountryRegionName] from Sales.vStoreWithAddresses WHERE AddressType='Main Office'*</span></span>

<span data-ttu-id="bcc86-182">Som returnerar följande resultat (den partiella):</span><span class="sxs-lookup"><span data-stu-id="bcc86-182">Which returns the following (partial) results:</span></span>

![Skärmbild av SQL-frågeresultat](./media/import-data/sqlqueryresults.png)

<span data-ttu-id="bcc86-184">Observera alias som Address.AddressType och Address.Location.StateProvinceName.</span><span class="sxs-lookup"><span data-stu-id="bcc86-184">Note the aliases such as Address.AddressType and Address.Location.StateProvinceName.</span></span> <span data-ttu-id="bcc86-185">Genom att ange kapslade avgränsare av '.', Importverktyget skapar adress och Address.Location underdokument under importen.</span><span class="sxs-lookup"><span data-stu-id="bcc86-185">By specifying a nesting separator of ‘.’, the import tool creates Address and Address.Location subdocuments during the import.</span></span> <span data-ttu-id="bcc86-186">Här är ett exempel på ett resulterande dokument i Azure Cosmos DB:</span><span class="sxs-lookup"><span data-stu-id="bcc86-186">Here is an example of a resulting document in Azure Cosmos DB:</span></span>

<span data-ttu-id="bcc86-187">*{”id”: ”956”, ”Name”: ”ökad försäljning och tjänst”, ”adress”: {”adresstyp”: ”Main Office”, ”AddressLine1”: ”#500 75 O'Connor gata”, ”plats”: {”stad”: ”Ottawa”, ”StateProvinceName”: ”Ontario”}, ”postnummer”: ”K4B 1S2”, ”CountryRegionName” ”: Kanada ”}}*</span><span class="sxs-lookup"><span data-stu-id="bcc86-187">*{ "id": "956", "Name": "Finer Sales and Service", "Address": { "AddressType": "Main Office", "AddressLine1": "#500-75 O'Connor Street", "Location": { "City": "Ottawa", "StateProvinceName": "Ontario" }, "PostalCode": "K4B 1S2", "CountryRegionName": "Canada" } }*</span></span>

<span data-ttu-id="bcc86-188">Här följer några exempel som kommandoraden ska importera från SQL Server:</span><span class="sxs-lookup"><span data-stu-id="bcc86-188">Here are some command line samples to import from SQL Server:</span></span>

    #Import records from SQL which match a query
    dt.exe /s:SQL /s.ConnectionString:"Data Source=<server>;Initial Catalog=AdventureWorks;User Id=advworks;Password=<password>;" /s.Query:"select CAST(BusinessEntityID AS varchar) as Id, * from Sales.vStoreWithAddresses WHERE AddressType='Main Office'" /t:CosmosDBBulk /t.ConnectionString:" AccountEndpoint=<CosmosDB Endpoint>;AccountKey=<CosmosDB Key>;Database=<CosmosDB Database>;" /t.Collection:Stores /t.IdField:Id /t.CollectionThroughput:2500

    #Import records from sql which match a query and create hierarchical relationships
    dt.exe /s:SQL /s.ConnectionString:"Data Source=<server>;Initial Catalog=AdventureWorks;User Id=advworks;Password=<password>;" /s.Query:"select CAST(BusinessEntityID AS varchar) as Id, Name, AddressType as [Address.AddressType], AddressLine1 as [Address.AddressLine1], City as [Address.Location.City], StateProvinceName as [Address.Location.StateProvinceName], PostalCode as [Address.PostalCode], CountryRegionName as [Address.CountryRegionName] from Sales.vStoreWithAddresses WHERE AddressType='Main Office'" /s.NestingSeparator:. /t:CosmosDBBulk /t.ConnectionString:" AccountEndpoint=<CosmosDB Endpoint>;AccountKey=<CosmosDB Key>;Database=<CosmosDB Database>;" /t.Collection:StoresSub /t.IdField:Id /t.CollectionThroughput:2500

## <span data-ttu-id="bcc86-189"><a id="CSV"></a>Importera CSV-filer och konvertera CSV till JSON</span><span class="sxs-lookup"><span data-stu-id="bcc86-189"><a id="CSV"></a>To import CSV files and convert CSV to JSON</span></span>
<span data-ttu-id="bcc86-190">Alternativet Importverktyget källa för CSV-fil kan du importera en eller flera CSV-filer.</span><span class="sxs-lookup"><span data-stu-id="bcc86-190">The CSV file source importer option enables you to import one or more CSV files.</span></span> <span data-ttu-id="bcc86-191">När du lägger till mapparna som innehåller CSV-filer för import, har du möjlighet att rekursivt söker efter filer i undermappar.</span><span class="sxs-lookup"><span data-stu-id="bcc86-191">When adding folders that contain CSV files for import, you have the option of recursively searching for files in subfolders.</span></span>

![Skärmbild av CSV - alternativ för CSV-fil för JSON](media/import-data/csvsource.png)

<span data-ttu-id="bcc86-193">Liknar SQL-källans, egenskapen kapslade avgränsare kan användas för att skapa hierarkiska relationer (underordnade dokument) under importen.</span><span class="sxs-lookup"><span data-stu-id="bcc86-193">Similar to the SQL source, the nesting separator property may be used to create hierarchical relationships (sub-documents) during import.</span></span> <span data-ttu-id="bcc86-194">Överväg följande CSV-huvud rad- och rader:</span><span class="sxs-lookup"><span data-stu-id="bcc86-194">Consider the following CSV header row and data rows:</span></span>

![Skärmbild av CSV Exempelposter - CSV-fil för JSON](./media/import-data/csvsample.png)

<span data-ttu-id="bcc86-196">Observera alias som DomainInfo.Domain_Name och RedirectInfo.Redirecting.</span><span class="sxs-lookup"><span data-stu-id="bcc86-196">Note the aliases such as DomainInfo.Domain_Name and RedirectInfo.Redirecting.</span></span> <span data-ttu-id="bcc86-197">Genom att ange kapslade avgränsare av '.', Importverktyget skapar DomainInfo och RedirectInfo underdokument under importen.</span><span class="sxs-lookup"><span data-stu-id="bcc86-197">By specifying a nesting separator of ‘.’, the import tool will create DomainInfo and RedirectInfo subdocuments during the import.</span></span> <span data-ttu-id="bcc86-198">Här är ett exempel på ett resulterande dokument i Azure Cosmos DB:</span><span class="sxs-lookup"><span data-stu-id="bcc86-198">Here is an example of a resulting document in Azure Cosmos DB:</span></span>

<span data-ttu-id="bcc86-199">*{”DomainInfo”: {”Domain_Name”: ”ACUS.GOV”, ”Domain_Name_Address”: ”http://www.ACUS.GOV”}, ”Federal myndighet” ”: administrativa konferens för USA”, ”RedirectInfo”: {”omdirigera”: ”0”, ”Redirect_Destination” ”:”}, ”id”: ”9cc565c5-ebcd-1c03-ebd3-cc3e2ecd814d”}*</span><span class="sxs-lookup"><span data-stu-id="bcc86-199">*{ "DomainInfo": { "Domain_Name": "ACUS.GOV", "Domain_Name_Address": "http://www.ACUS.GOV" }, "Federal Agency": "Administrative Conference of the United States", "RedirectInfo": { "Redirecting": "0", "Redirect_Destination": "" }, "id": "9cc565c5-ebcd-1c03-ebd3-cc3e2ecd814d" }*</span></span>

<span data-ttu-id="bcc86-200">Importverktyget försöker att härleda typinformation för ociterade värden i CSV-filer (inom citattecken värden behandlas alltid som strängar).</span><span class="sxs-lookup"><span data-stu-id="bcc86-200">The import tool will attempt to infer type information for unquoted values in CSV files (quoted values are always treated as strings).</span></span>  <span data-ttu-id="bcc86-201">Typer som identifieras i följande ordning: nummer, datetime, booleskt värde.</span><span class="sxs-lookup"><span data-stu-id="bcc86-201">Types are identified in the following order: number, datetime, boolean.</span></span>  

<span data-ttu-id="bcc86-202">Det finns två saker att Observera om CSV-import:</span><span class="sxs-lookup"><span data-stu-id="bcc86-202">There are two other things to note about CSV import:</span></span>

1. <span data-ttu-id="bcc86-203">Som standard ociterade värden alltid bort för flikar och blanksteg, medan inom citattecken värden sparas som-är.</span><span class="sxs-lookup"><span data-stu-id="bcc86-203">By default, unquoted values are always trimmed for tabs and spaces, while quoted values are preserved as-is.</span></span> <span data-ttu-id="bcc86-204">Det här beteendet kan åsidosättas med kryssrutan Rensa inom citattecken värden eller /s.TrimQuoted kommandoradsalternativet.</span><span class="sxs-lookup"><span data-stu-id="bcc86-204">This behavior can be overridden with the Trim quoted values checkbox or the /s.TrimQuoted command line option.</span></span>
2. <span data-ttu-id="bcc86-205">Som standard behandlas en ociterade null som ett null-värde.</span><span class="sxs-lookup"><span data-stu-id="bcc86-205">By default, an unquoted null is treated as a null value.</span></span> <span data-ttu-id="bcc86-206">Det här beteendet kan åsidosättas (d.v.s. behandla en ociterade null som en ”null-sträng) med behandla onoterade NULL som sträng kryssrutan eller /s.NoUnquotedNulls kommandoradsalternativet.</span><span class="sxs-lookup"><span data-stu-id="bcc86-206">This behavior can be overridden (i.e. treat an unquoted null as a “null” string) with the Treat unquoted NULL as string checkbox or the /s.NoUnquotedNulls command line option.</span></span>

<span data-ttu-id="bcc86-207">Här följer ett exempel på kommandoraden för CSV-import:</span><span class="sxs-lookup"><span data-stu-id="bcc86-207">Here is a command line sample for CSV import:</span></span>

    dt.exe /s:CsvFile /s.Files:.\Employees.csv /t:CosmosDBBulk /t.ConnectionString:"AccountEndpoint=<CosmosDB Endpoint>;AccountKey=<CosmosDB Key>;Database=<CosmosDB Database>;" /t.Collection:Employees /t.IdField:EntityID /t.CollectionThroughput:2500

## <span data-ttu-id="bcc86-208"><a id="AzureTableSource"></a>Importera från Azure Table storage</span><span class="sxs-lookup"><span data-stu-id="bcc86-208"><a id="AzureTableSource"></a>To import from Azure Table storage</span></span>
<span data-ttu-id="bcc86-209">Azure Table storage källa Importverktyget alternativet kan du importera från en enskild Azure Table storage tabell och du kan också filtrera tabellentiteter som ska importeras.</span><span class="sxs-lookup"><span data-stu-id="bcc86-209">The Azure Table storage source importer option allows you to import from an individual Azure Table storage table and optionally filter the table entities to be imported.</span></span> <span data-ttu-id="bcc86-210">Observera att du inte kan använda verktyget datamigrering importera Azure Table storage data till Azure Cosmos DB för användning med tabell-API.</span><span class="sxs-lookup"><span data-stu-id="bcc86-210">Note that you cannot use the Data Migration tool to import Azure Table storage data into Azure Cosmos DB for use with the Table API.</span></span> <span data-ttu-id="bcc86-211">Endast import till Azure Cosmos DB för användning med DocumentDB-API: et stöds just nu.</span><span class="sxs-lookup"><span data-stu-id="bcc86-211">Only importing to Azure Cosmos DB for use with the DocumentDB API is supported at this time.</span></span>

![Skärmbild av Azure Table storage alternativ](./media/import-data/azuretablesource.png)

<span data-ttu-id="bcc86-213">Formatet för anslutningssträngen för Azure Table storage är:</span><span class="sxs-lookup"><span data-stu-id="bcc86-213">The format of the Azure Table storage connection string is:</span></span>

    DefaultEndpointsProtocol=<protocol>;AccountName=<Account Name>;AccountKey=<Account Key>;

> [!NOTE]
> <span data-ttu-id="bcc86-214">Använd kommandot Kontrollera så att den angivna i strängen anslutningsfältet i Azure Table storage instansen kan nås.</span><span class="sxs-lookup"><span data-stu-id="bcc86-214">Use the Verify command to ensure that the Azure Table storage instance specified in the connection string field can be accessed.</span></span>
> 
> 

<span data-ttu-id="bcc86-215">Ange namnet på tabellen Azure från vilken data ska importeras.</span><span class="sxs-lookup"><span data-stu-id="bcc86-215">Enter the name of the Azure table from which data will be imported.</span></span> <span data-ttu-id="bcc86-216">Du kan du ange en [filter](https://msdn.microsoft.com/library/azure/ff683669.aspx).</span><span class="sxs-lookup"><span data-stu-id="bcc86-216">You may optionally specify a [filter](https://msdn.microsoft.com/library/azure/ff683669.aspx).</span></span>

<span data-ttu-id="bcc86-217">Azure Table storage källalternativet Importverktyget har följande alternativ:</span><span class="sxs-lookup"><span data-stu-id="bcc86-217">The Azure Table storage source importer option has the following additional options:</span></span>

1. <span data-ttu-id="bcc86-218">Inkludera interna fält</span><span class="sxs-lookup"><span data-stu-id="bcc86-218">Include Internal Fields</span></span>
   1. <span data-ttu-id="bcc86-219">Innehåller alla - alla interna fält (PartitionKey, RowKey och tidsstämpel)</span><span class="sxs-lookup"><span data-stu-id="bcc86-219">All - Include all internal fields (PartitionKey, RowKey, and Timestamp)</span></span>
   2. <span data-ttu-id="bcc86-220">Ingen - undanta alla interna fält</span><span class="sxs-lookup"><span data-stu-id="bcc86-220">None - Exclude all internal fields</span></span>
   3. <span data-ttu-id="bcc86-221">RowKey - bara innehålla fältet RowKey</span><span class="sxs-lookup"><span data-stu-id="bcc86-221">RowKey - Only include the RowKey field</span></span>
2. <span data-ttu-id="bcc86-222">Välj kolumner</span><span class="sxs-lookup"><span data-stu-id="bcc86-222">Select Columns</span></span>
   1. <span data-ttu-id="bcc86-223">Azure Table storage filter stöder inte projektioner.</span><span class="sxs-lookup"><span data-stu-id="bcc86-223">Azure Table storage filters do not support projections.</span></span> <span data-ttu-id="bcc86-224">Om du vill importera bara specifika Azure Table-Entitetsegenskaper lägger du till dem i listan Välj kolumner.</span><span class="sxs-lookup"><span data-stu-id="bcc86-224">If you want to only import specific Azure Table entity properties, add them to the Select Columns list.</span></span> <span data-ttu-id="bcc86-225">Alla andra Entitetsegenskaper kommer att ignoreras.</span><span class="sxs-lookup"><span data-stu-id="bcc86-225">All other entity properties will be ignored.</span></span>

<span data-ttu-id="bcc86-226">Här följer ett exempel på kommandoraden att importera från Azure Table storage:</span><span class="sxs-lookup"><span data-stu-id="bcc86-226">Here is a command line sample to import from Azure Table storage:</span></span>

    dt.exe /s:AzureTable /s.ConnectionString:"DefaultEndpointsProtocol=https;AccountName=<Account Name>;AccountKey=<Account Key>" /s.Table:metrics /s.InternalFields:All /s.Filter:"PartitionKey eq 'Partition1' and RowKey gt '00001'" /s.Projection:ObjectCount;ObjectSize  /t:CosmosDBBulk /t.ConnectionString:" AccountEndpoint=<CosmosDB Endpoint>;AccountKey=<CosmosDB Key>;Database=<CosmosDB Database>;" /t.Collection:metrics /t.CollectionThroughput:2500

## <span data-ttu-id="bcc86-227"><a id="DynamoDBSource"></a>Importera från Amazon DynamoDB</span><span class="sxs-lookup"><span data-stu-id="bcc86-227"><a id="DynamoDBSource"></a>To import from Amazon DynamoDB</span></span>
<span data-ttu-id="bcc86-228">Amazon DynamoDB källa Importverktyget alternativet kan du importera från en enskild tabell Amazon DynamoDB och du kan också filtrera enheterna som ska importeras.</span><span class="sxs-lookup"><span data-stu-id="bcc86-228">The Amazon DynamoDB source importer option allows you to import from an individual Amazon DynamoDB table and optionally filter the entities to be imported.</span></span> <span data-ttu-id="bcc86-229">Flera mallar tillhandahålls så att ställa in en import är så enkel som möjligt.</span><span class="sxs-lookup"><span data-stu-id="bcc86-229">Several templates are provided so that setting up an import is as easy as possible.</span></span>

![Skärmbild av Amazon DynamoDB alternativ - Databasverktyg för migrering](./media/import-data/dynamodbsource1.png)

![Skärmbild av Amazon DynamoDB alternativ - Databasverktyg för migrering](./media/import-data/dynamodbsource2.png)

<span data-ttu-id="bcc86-232">Formatet för anslutningssträngen Amazon DynamoDB är:</span><span class="sxs-lookup"><span data-stu-id="bcc86-232">The format of the Amazon DynamoDB connection string is:</span></span>

    ServiceURL=<Service Address>;AccessKey=<Access Key>;SecretKey=<Secret Key>;

> [!NOTE]
> <span data-ttu-id="bcc86-233">Använd kommandot Kontrollera så att den angivna i strängen anslutningsfältet Amazon DynamoDB instansen kan nås.</span><span class="sxs-lookup"><span data-stu-id="bcc86-233">Use the Verify command to ensure that the Amazon DynamoDB instance specified in the connection string field can be accessed.</span></span>
> 
> 

<span data-ttu-id="bcc86-234">Här följer ett exempel på kommandoraden att importera från Amazon DynamoDB:</span><span class="sxs-lookup"><span data-stu-id="bcc86-234">Here is a command line sample to import from Amazon DynamoDB:</span></span>

    dt.exe /s:DynamoDB /s.ConnectionString:ServiceURL=https://dynamodb.us-east-1.amazonaws.com;AccessKey=<accessKey>;SecretKey=<secretKey> /s.Request:"{   """TableName""": """ProductCatalog""" }" /t:DocumentDBBulk /t.ConnectionString:"AccountEndpoint=<Azure Cosmos DB Endpoint>;AccountKey=<Azure Cosmos DB Key>;Database=<Azure Cosmos DB Database>;" /t.Collection:catalogCollection /t.CollectionThroughput:2500

## <span data-ttu-id="bcc86-235"><a id="BlobImport"></a>Importera filer från Azure Blob storage</span><span class="sxs-lookup"><span data-stu-id="bcc86-235"><a id="BlobImport"></a>To import files from Azure Blob storage</span></span>
<span data-ttu-id="bcc86-236">JSON-fil, MongoDB exportfilen och CSV-filen Importverktyget alternativ kan du importera en eller flera filer från Azure Blob storage.</span><span class="sxs-lookup"><span data-stu-id="bcc86-236">The JSON file, MongoDB export file, and CSV file source importer options allow you to import one or more files from Azure Blob storage.</span></span> <span data-ttu-id="bcc86-237">När du har angett en URL för Blob-behållaren och Kontonyckel, anger du bara ett reguljärt uttryck för att välja filen eller filerna ska importeras.</span><span class="sxs-lookup"><span data-stu-id="bcc86-237">After specifying a Blob container URL and Account Key, simply provide a regular expression to select the file(s) to import.</span></span>

![Skärmbild av Blob alternativ för källa](./media/import-data/blobsource.png)

<span data-ttu-id="bcc86-239">Här följer exempel på kommandoraden att importera JSON-filer från Azure Blob storage:</span><span class="sxs-lookup"><span data-stu-id="bcc86-239">Here is command line sample to import JSON files from Azure Blob storage:</span></span>

    dt.exe /s:JsonFile /s.Files:"blobs://<account key>@account.blob.core.windows.net:443/importcontainer/.*" /t:CosmosDBBulk /t.ConnectionString:"AccountEndpoint=<CosmosDB Endpoint>;AccountKey=<CosmosDB Key>;Database=<CosmosDB Database>;" /t.Collection:doctest

## <span data-ttu-id="bcc86-240"><a id="DocumentDBSource"></a>Importera från en samling Azure Cosmos DB DocumentDB API</span><span class="sxs-lookup"><span data-stu-id="bcc86-240"><a id="DocumentDBSource"></a>To import from an Azure Cosmos DB DocumentDB API collection</span></span>
<span data-ttu-id="bcc86-241">Azure Cosmos DB källa Importverktyget alternativet kan du importera data från en eller flera Azure Cosmos DB samlingar och du kan också filtrera dokument med hjälp av en fråga.</span><span class="sxs-lookup"><span data-stu-id="bcc86-241">The Azure Cosmos DB source importer option allows you to import data from one or more Azure Cosmos DB collections and optionally filter documents using a query.</span></span>  

![Skärmbild av Azure Cosmos DB alternativ](./media/import-data/documentdbsource.png)

<span data-ttu-id="bcc86-243">Formatet för anslutningssträngen för Azure Cosmos DB är:</span><span class="sxs-lookup"><span data-stu-id="bcc86-243">The format of the Azure Cosmos DB connection string is:</span></span>

    AccountEndpoint=<CosmosDB Endpoint>;AccountKey=<CosmosDB Key>;Database=<CosmosDB Database>;

<span data-ttu-id="bcc86-244">Anslutningssträngen för Azure DB som Cosmos-konto kan hämtas från bladet nycklar i Azure-portalen, enligt beskrivningen i [så här hanterar du ett konto i Azure Cosmos DB](manage-account.md), men namnet på databasen måste läggas till anslutning sträng i följande format:</span><span class="sxs-lookup"><span data-stu-id="bcc86-244">The Azure Cosmos DB account connection string can be retrieved from the Keys blade of the Azure portal, as described in [How to manage an Azure Cosmos DB account](manage-account.md), however the name of the database needs to be appended to the connection string in the following format:</span></span>

    Database=<CosmosDB Database>;

> [!NOTE]
> <span data-ttu-id="bcc86-245">Använd kommandot Kontrollera så att den angivna i strängen anslutningsfältet Azure DB som Cosmos-instansen är tillgänglig.</span><span class="sxs-lookup"><span data-stu-id="bcc86-245">Use the Verify command to ensure that the Azure Cosmos DB instance specified in the connection string field can be accessed.</span></span>
> 
> 

<span data-ttu-id="bcc86-246">Ange namnet på samlingen som data ska importeras för att importera från en enda Azure DB som Cosmos-samling.</span><span class="sxs-lookup"><span data-stu-id="bcc86-246">To import from a single Azure Cosmos DB collection, enter the name of the collection from which data will be imported.</span></span> <span data-ttu-id="bcc86-247">Om du vill importera från flera Azure Cosmos DB samlingar, ange ett reguljärt uttryck för att matcha samlingsnamn för en eller flera (t.ex. collection01 | collection02 | collection03).</span><span class="sxs-lookup"><span data-stu-id="bcc86-247">To import from multiple Azure Cosmos DB collections, provide a regular expression to match one or more collection names (e.g. collection01 | collection02 | collection03).</span></span> <span data-ttu-id="bcc86-248">Alternativt kan du ange eller ange en fil för en fråga till både filter och form data som ska importeras.</span><span class="sxs-lookup"><span data-stu-id="bcc86-248">You may optionally specify, or provide a file for, a query to both filter and shape the data to be imported.</span></span>

> [!NOTE]
> <span data-ttu-id="bcc86-249">Eftersom fältet samling accepterar reguljära uttryck om du importerar från en enda samling vars namn innehåller specialtecken i reguljära uttryck, måste dessa tecken hoppas därför.</span><span class="sxs-lookup"><span data-stu-id="bcc86-249">Since the collection field accepts regular expressions, if you are importing from a single collection whose name contains regular expression characters, then those characters must be escaped accordingly.</span></span>
> 
> 

<span data-ttu-id="bcc86-250">Importverktyget för Azure Cosmos DB källalternativet har avancerade alternativ:</span><span class="sxs-lookup"><span data-stu-id="bcc86-250">The Azure Cosmos DB source importer option has the following advanced options:</span></span>

1. <span data-ttu-id="bcc86-251">Ta med intern fält: Anger om du vill inkludera Azure Cosmos DB dokumentegenskaper system i exporten (t.ex. _rid, _ts) eller inte.</span><span class="sxs-lookup"><span data-stu-id="bcc86-251">Include Internal Fields: Specifies whether or not to include Azure Cosmos DB document system properties in the export (e.g. _rid, _ts).</span></span>
2. <span data-ttu-id="bcc86-252">Antalet återförsök vid fel: Anger antalet gånger för att försöka anslutningen till Azure Cosmos DB vid tillfälligt fel (t.ex. anslutningen avbrott i nätverket).</span><span class="sxs-lookup"><span data-stu-id="bcc86-252">Number of Retries on Failure: Specifies the number of times to retry the connection to Azure Cosmos DB in case of transient failures (e.g. network connectivity interruption).</span></span>
3. <span data-ttu-id="bcc86-253">Återförsöksintervall: Anger hur länge väntetiden mellan anslutningsförsök görs till Azure Cosmos DB vid tillfälligt fel (t.ex. anslutningen avbrott i nätverket).</span><span class="sxs-lookup"><span data-stu-id="bcc86-253">Retry Interval: Specifies how long to wait between retrying the connection to Azure Cosmos DB in case of transient failures (e.g. network connectivity interruption).</span></span>
4. <span data-ttu-id="bcc86-254">Anslutningsläge: Anger Anslutningsläge ska användas med Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="bcc86-254">Connection Mode: Specifies the connection mode to use with Azure Cosmos DB.</span></span> <span data-ttu-id="bcc86-255">Tillgängliga alternativ är DirectTcp, DirectHttps och Gateway.</span><span class="sxs-lookup"><span data-stu-id="bcc86-255">The available choices are DirectTcp, DirectHttps, and Gateway.</span></span> <span data-ttu-id="bcc86-256">Direktanslutning lägena är snabbare, medan gateway-läge är mer brandvägg eget eftersom den endast använder port 443.</span><span class="sxs-lookup"><span data-stu-id="bcc86-256">The direct connection modes are faster, while the gateway mode is more firewall friendly as it only uses port 443.</span></span>

![Skärmbild av Azure Cosmos DB-datakälla avancerade alternativ](./media/import-data/documentdbsourceoptions.png)

> [!TIP]
> <span data-ttu-id="bcc86-258">Importverktyget standard anslutningsläge DirectTcp.</span><span class="sxs-lookup"><span data-stu-id="bcc86-258">The import tool defaults to connection mode DirectTcp.</span></span> <span data-ttu-id="bcc86-259">Om det uppstår brandväggsproblem med kan växla till anslutningsläge Gateway, eftersom den kräver endast port 443.</span><span class="sxs-lookup"><span data-stu-id="bcc86-259">If you experience firewall issues, switch to connection mode Gateway, as it only requires port 443.</span></span>
> 
> 

<span data-ttu-id="bcc86-260">Här följer några kommandoraden-exempel för att importera från Azure Cosmos DB:</span><span class="sxs-lookup"><span data-stu-id="bcc86-260">Here are some command line samples to import from Azure Cosmos DB:</span></span>

    #Migrate data from one Azure Cosmos DB collection to another Azure Cosmos DB collections
    dt.exe /s:CosmosDB /s.ConnectionString:"AccountEndpoint=<CosmosDB Endpoint>;AccountKey=<CosmosDB Key>;Database=<CosmosDB Database>;" /s.Collection:TEColl /t:CosmosDBBulk /t.ConnectionString:" AccountEndpoint=<CosmosDB Endpoint>;AccountKey=<CosmosDB Key>;Database=<CosmosDB Database>;" /t.Collection:TESessions /t.CollectionThroughput:2500

    #Migrate data from multiple Azure Cosmos DB collections to a single Azure Cosmos DB collection
    dt.exe /s:CosmosDB /s.ConnectionString:"AccountEndpoint=<CosmosDB Endpoint>;AccountKey=<CosmosDB Key>;Database=<CosmosDB Database>;" /s.Collection:comp1|comp2|comp3|comp4 /t:CosmosDBBulk /t.ConnectionString:"AccountEndpoint=<CosmosDB Endpoint>;AccountKey=<CosmosDB Key>;Database=<CosmosDB Database>;" /t.Collection:singleCollection /t.CollectionThroughput:2500

    #Export an Azure Cosmos DB collection to a JSON file
    dt.exe /s:CosmosDB /s.ConnectionString:"AccountEndpoint=<CosmosDB Endpoint>;AccountKey=<CosmosDB Key>;Database=<CosmosDB Database>;" /s.Collection:StoresSub /t:JsonFile /t.File:StoresExport.json /t.Overwrite /t.CollectionThroughput:2500

> [!TIP]
> <span data-ttu-id="bcc86-261">Azure Cosmos DB Data importera verktyget även stöd för import av data från den [Azure Cosmos DB emulatorn](local-emulator.md).</span><span class="sxs-lookup"><span data-stu-id="bcc86-261">The Azure Cosmos DB Data Import Tool also supports import of data from the [Azure Cosmos DB Emulator](local-emulator.md).</span></span> <span data-ttu-id="bcc86-262">När du importerar data från en lokal emulator sägs slutpunkten `https://localhost:<port>`.</span><span class="sxs-lookup"><span data-stu-id="bcc86-262">When importing data from a local emulator, set the endpoint to `https://localhost:<port>`.</span></span> 
> 
> 

## <span data-ttu-id="bcc86-263"><a id="HBaseSource"></a>Importera från HBase</span><span class="sxs-lookup"><span data-stu-id="bcc86-263"><a id="HBaseSource"></a>To import from HBase</span></span>
<span data-ttu-id="bcc86-264">HBase källa Importverktyget alternativet kan du importera data från en HBase-tabell och du kan också filtrera data.</span><span class="sxs-lookup"><span data-stu-id="bcc86-264">The HBase source importer option allows you to import data from an HBase table and optionally filter the data.</span></span> <span data-ttu-id="bcc86-265">Flera mallar tillhandahålls så att ställa in en import är så enkel som möjligt.</span><span class="sxs-lookup"><span data-stu-id="bcc86-265">Several templates are provided so that setting up an import is as easy as possible.</span></span>

![Skärmbild av HBase alternativ](./media/import-data/hbasesource1.png)

![Skärmbild av HBase alternativ](./media/import-data/hbasesource2.png)

<span data-ttu-id="bcc86-268">Formatet för anslutningssträngen HBase Stargate är:</span><span class="sxs-lookup"><span data-stu-id="bcc86-268">The format of the HBase Stargate connection string is:</span></span>

    ServiceURL=<server-address>;Username=<username>;Password=<password>

> [!NOTE]
> <span data-ttu-id="bcc86-269">Använd kommandot Kontrollera så att den angivna i strängen anslutningsfältet HBase-instansen är tillgänglig.</span><span class="sxs-lookup"><span data-stu-id="bcc86-269">Use the Verify command to ensure that the HBase instance specified in the connection string field can be accessed.</span></span>
> 
> 

<span data-ttu-id="bcc86-270">Här följer ett exempel på kommandoraden att importera från HBase:</span><span class="sxs-lookup"><span data-stu-id="bcc86-270">Here is a command line sample to import from HBase:</span></span>

    dt.exe /s:HBase /s.ConnectionString:ServiceURL=<server-address>;Username=<username>;Password=<password> /s.Table:Contacts /t:CosmosDBBulk /t.ConnectionString:"AccountEndpoint=<CosmosDB Endpoint>;AccountKey=<CosmosDB Key>;Database=<CosmosDB Database>;" /t.Collection:hbaseimport

## <span data-ttu-id="bcc86-271"><a id="DocumentDBBulkTarget"></a>Importera till DocumentDB-API (massimport)</span><span class="sxs-lookup"><span data-stu-id="bcc86-271"><a id="DocumentDBBulkTarget"></a>To import to the DocumentDB API (Bulk Import)</span></span>
<span data-ttu-id="bcc86-272">Importverktyget för Azure Cosmos DB samtidigt kan du importera från något av alternativen tillgänglig källa med en Azure Cosmos DB lagrade proceduren för effektivitet.</span><span class="sxs-lookup"><span data-stu-id="bcc86-272">The Azure Cosmos DB Bulk importer allows you to import from any of the available source options, using an Azure Cosmos DB stored procedure for efficiency.</span></span> <span data-ttu-id="bcc86-273">Verktyget stöder import till en enda partitionerad Azure Cosmos DB samling samt delat import genom vilken data är partitionerad över flera samlingar för en partitionerad Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="bcc86-273">The tool supports import to one single-partitioned Azure Cosmos DB collection, as well as sharded import whereby data is partitioned across multiple single-partitioned Azure Cosmos DB collections.</span></span> <span data-ttu-id="bcc86-274">Mer information om partitionering data finns [partitionering och skalning i Azure Cosmos DB](partition-data.md).</span><span class="sxs-lookup"><span data-stu-id="bcc86-274">For more information about partitioning data, see [Partitioning and scaling in Azure Cosmos DB](partition-data.md).</span></span> <span data-ttu-id="bcc86-275">Verktyget skapar, köra och ta sedan bort den lagrade proceduren från samling(ar) för målet.</span><span class="sxs-lookup"><span data-stu-id="bcc86-275">The tool will create, execute, and then delete the stored procedure from the target collection(s).</span></span>  

![Skärmbild av Azure Cosmos DB bulk-alternativ](./media/import-data/documentdbbulk.png)

<span data-ttu-id="bcc86-277">Formatet för anslutningssträngen för Azure Cosmos DB är:</span><span class="sxs-lookup"><span data-stu-id="bcc86-277">The format of the Azure Cosmos DB connection string is:</span></span>

    AccountEndpoint=<CosmosDB Endpoint>;AccountKey=<CosmosDB Key>;Database=<CosmosDB Database>;

<span data-ttu-id="bcc86-278">Anslutningssträngen för Azure DB som Cosmos-konto kan hämtas från bladet nycklar i Azure-portalen, enligt beskrivningen i [så här hanterar du ett konto i Azure Cosmos DB](manage-account.md), men namnet på databasen måste läggas till anslutning sträng i följande format:</span><span class="sxs-lookup"><span data-stu-id="bcc86-278">The Azure Cosmos DB account connection string can be retrieved from the Keys blade of the Azure portal, as described in [How to manage an Azure Cosmos DB account](manage-account.md), however the name of the database needs to be appended to the connection string in the following format:</span></span>

    Database=<CosmosDB Database>;

> [!NOTE]
> <span data-ttu-id="bcc86-279">Använd kommandot Kontrollera så att den angivna i strängen anslutningsfältet Azure DB som Cosmos-instansen är tillgänglig.</span><span class="sxs-lookup"><span data-stu-id="bcc86-279">Use the Verify command to ensure that the Azure Cosmos DB instance specified in the connection string field can be accessed.</span></span>
> 
> 

<span data-ttu-id="bcc86-280">Ange namnet på den samling som data ska importeras och klicka på Lägg till om du vill importera till en enda samling.</span><span class="sxs-lookup"><span data-stu-id="bcc86-280">To import to a single collection, enter the name of the collection to which data will be imported and click the Add button.</span></span> <span data-ttu-id="bcc86-281">Om du vill importera till flera samlingar, ange varje samlingsnamn individuellt eller Använd följande syntax för att ange flera samlingar: *collection_prefix*[startIndex - end index].</span><span class="sxs-lookup"><span data-stu-id="bcc86-281">To import to multiple collections, either enter each collection name individually or use the following syntax to specify multiple collections: *collection_prefix*[start index - end index].</span></span> <span data-ttu-id="bcc86-282">Tänk på följande när du anger flera samlingar via ovannämnda syntax:</span><span class="sxs-lookup"><span data-stu-id="bcc86-282">When specifying multiple collections via the aforementioned syntax, keep the following in mind:</span></span>

1. <span data-ttu-id="bcc86-283">Endast heltal intervallet namnet mönster stöds.</span><span class="sxs-lookup"><span data-stu-id="bcc86-283">Only integer range name patterns are supported.</span></span> <span data-ttu-id="bcc86-284">Till exempel ange samlingen [0-3] genererar följande samlingar: collection0, collection1, collection2 collection3.</span><span class="sxs-lookup"><span data-stu-id="bcc86-284">For example, specifying collection[0-3] will produce the following collections: collection0, collection1, collection2, collection3.</span></span>
2. <span data-ttu-id="bcc86-285">Du kan använda en förkortad syntax: samlingen [3] genererar samma uppsättning samlingar som nämns i steg 1.</span><span class="sxs-lookup"><span data-stu-id="bcc86-285">You can use an abbreviated syntax: collection[3] will emit same set of collections mentioned in step 1.</span></span>
3. <span data-ttu-id="bcc86-286">Mer än en ersättning kan anges.</span><span class="sxs-lookup"><span data-stu-id="bcc86-286">More than one substitution can be provided.</span></span> <span data-ttu-id="bcc86-287">Till exempel samlingen [0-1] [0-9] genererar 20 samlingsnamn med nollor (collection01... 02... 03).</span><span class="sxs-lookup"><span data-stu-id="bcc86-287">For example, collection[0-1] [0-9] will generate 20 collection names with leading zeros (collection01, ..02, ..03).</span></span>

<span data-ttu-id="bcc86-288">När samlingen namn har angetts, väljer du önskad genomflödet av samling(ar) (400 RUs till 10 000 RUs).</span><span class="sxs-lookup"><span data-stu-id="bcc86-288">Once the collection name(s) have been specified, choose the desired throughput of the collection(s) (400 RUs to 10,000 RUs).</span></span> <span data-ttu-id="bcc86-289">Välj en högre genomströmning för bästa prestanda för import.</span><span class="sxs-lookup"><span data-stu-id="bcc86-289">For best import performance, choose a higher throughput.</span></span> <span data-ttu-id="bcc86-290">Läs mer om prestandanivåer [prestandanivåer i Azure Cosmos DB](performance-levels.md).</span><span class="sxs-lookup"><span data-stu-id="bcc86-290">For more information about performance levels, see [Performance levels in Azure Cosmos DB](performance-levels.md).</span></span>

> [!NOTE]
> <span data-ttu-id="bcc86-291">Prestanda genomströmning inställningen gäller bara skapa en samling.</span><span class="sxs-lookup"><span data-stu-id="bcc86-291">The performance throughput setting only applies to collection creation.</span></span> <span data-ttu-id="bcc86-292">Om den angivna samlingen finns redan, ändras inte dess genomflöde.</span><span class="sxs-lookup"><span data-stu-id="bcc86-292">If the specified collection already exists, its throughput will not be modified.</span></span>
> 
> 

<span data-ttu-id="bcc86-293">När du importerar till flera samlingar baserat import verktyget stöder hash horisontell partitionering.</span><span class="sxs-lookup"><span data-stu-id="bcc86-293">When importing to multiple collections, the import tool supports hash based sharding.</span></span> <span data-ttu-id="bcc86-294">I det här scenariot, ange egenskapen document som du vill använda som partitionsnyckel (om Partitionsnyckeln är tomt dokument är delat slumpmässigt över mål samlingarna).</span><span class="sxs-lookup"><span data-stu-id="bcc86-294">In this scenario, specify the document property you wish to use as the Partition Key (if Partition Key is left blank, documents will be sharded randomly across the target collections).</span></span>

<span data-ttu-id="bcc86-295">Du kan ange vilket fält i import-källa som ska användas som egenskapen Azure Cosmos DB dokument-id vid import (Observera att om dokument inte innehåller den här egenskapen, sedan importera verktyget att generera ett GUID som egenskapsvärdet id).</span><span class="sxs-lookup"><span data-stu-id="bcc86-295">You may optionally specify which field in the import source should be used as the Azure Cosmos DB document id property during the import (note that if documents do not contain this property, then the import tool will generate a GUID as the id property value).</span></span>

<span data-ttu-id="bcc86-296">Det finns ett antal avancerade alternativ under importen.</span><span class="sxs-lookup"><span data-stu-id="bcc86-296">There are a number of advanced options available during import.</span></span> <span data-ttu-id="bcc86-297">Först, medan verktyget innehåller en standard massimport lagrad procedur (BulkInsert.js), kan du ange egna importera lagrade proceduren:</span><span class="sxs-lookup"><span data-stu-id="bcc86-297">First, while the tool includes a default bulk import stored procedure (BulkInsert.js), you may choose to specify your own import stored procedure:</span></span>

 ![Skärmbild av Azure Cosmos DB bulk insert sproc alternativet](./media/import-data/bulkinsertsp.png)

<span data-ttu-id="bcc86-299">När du importerar datum typer (t.ex. från SQL Server eller MongoDB) kan välja du dessutom mellan tre importalternativ:</span><span class="sxs-lookup"><span data-stu-id="bcc86-299">Additionally, when importing date types (e.g. from SQL Server or MongoDB), you can choose between three import options:</span></span>

 ![Skärmbild av Azure Cosmos DB datum tid importalternativ](./media/import-data/datetimeoptions.png)

* <span data-ttu-id="bcc86-301">Sträng: Spara som ett strängvärde</span><span class="sxs-lookup"><span data-stu-id="bcc86-301">String: Persist as a string value</span></span>
* <span data-ttu-id="bcc86-302">Epok: Spara som en epok numeriskt värde</span><span class="sxs-lookup"><span data-stu-id="bcc86-302">Epoch: Persist as an Epoch number value</span></span>
* <span data-ttu-id="bcc86-303">Både: Spara både sträng och epok numeriska värden.</span><span class="sxs-lookup"><span data-stu-id="bcc86-303">Both: Persist both string and Epoch number values.</span></span> <span data-ttu-id="bcc86-304">Det här alternativet skapas en underdokument, till exempel: ”date_joined”: {”värde” ”: 2013-10-21T21:17:25.2410000Z” ”, epok”: 1382390245}</span><span class="sxs-lookup"><span data-stu-id="bcc86-304">This option will create a subdocument, for example: "date_joined": { "Value": "2013-10-21T21:17:25.2410000Z", "Epoch": 1382390245 }</span></span>

<span data-ttu-id="bcc86-305">Importverktyget för Azure Cosmos DB Bulk har följande ytterligare avancerade alternativ:</span><span class="sxs-lookup"><span data-stu-id="bcc86-305">The Azure Cosmos DB Bulk importer has the following additional advanced options:</span></span>

1. <span data-ttu-id="bcc86-306">Batchstorlek: Verktyget som standard en batchstorlek 50.</span><span class="sxs-lookup"><span data-stu-id="bcc86-306">Batch Size: The tool defaults to a batch size of 50.</span></span>  <span data-ttu-id="bcc86-307">Om de dokument som ska importeras är stor kan du sänka batchstorleken.</span><span class="sxs-lookup"><span data-stu-id="bcc86-307">If the documents to be imported are large, consider lowering the batch size.</span></span> <span data-ttu-id="bcc86-308">Om de dokument som ska importeras är liten kan du däremot du höja batchstorleken.</span><span class="sxs-lookup"><span data-stu-id="bcc86-308">Conversely, if the documents to be imported are small, consider raising the batch size.</span></span>
2. <span data-ttu-id="bcc86-309">Maxstorlek för skript (byte): verktyget som standard max skript storleken 512KB</span><span class="sxs-lookup"><span data-stu-id="bcc86-309">Max Script Size (bytes): The tool defaults to a max script size of 512KB</span></span>
3. <span data-ttu-id="bcc86-310">Inaktivera automatisk generering: Om alla dokument som ska importeras innehåller ett ID-fält kan det här alternativet kan öka prestanda.</span><span class="sxs-lookup"><span data-stu-id="bcc86-310">Disable Automatic Id Generation: If every document to be imported contains an id field, then selecting this option can increase performance.</span></span> <span data-ttu-id="bcc86-311">Kommer inte att importera dokument som har ett unikt id-fält saknas.</span><span class="sxs-lookup"><span data-stu-id="bcc86-311">Documents missing a unique id field will not be imported.</span></span>
4. <span data-ttu-id="bcc86-312">Uppdatera befintliga dokument: Verktyget som standard inte ersätta befintliga dokument med id-konflikter.</span><span class="sxs-lookup"><span data-stu-id="bcc86-312">Update Existing Documents: The tool defaults to not replacing existing documents with id conflicts.</span></span> <span data-ttu-id="bcc86-313">Det här alternativet kan skriva över befintliga dokument med matchande ID: n.</span><span class="sxs-lookup"><span data-stu-id="bcc86-313">Selecting this option will allow overwriting existing documents with matching ids.</span></span> <span data-ttu-id="bcc86-314">Den här funktionen är användbart för schemalagd datauppsättning migreringar som uppdaterar befintliga dokument.</span><span class="sxs-lookup"><span data-stu-id="bcc86-314">This feature is useful for scheduled data migrations that update existing documents.</span></span>
5. <span data-ttu-id="bcc86-315">Antalet återförsök vid fel: Anger antalet gånger för att försöka anslutningen till Azure Cosmos DB vid tillfälligt fel (t.ex. anslutningen avbrott i nätverket).</span><span class="sxs-lookup"><span data-stu-id="bcc86-315">Number of Retries on Failure: Specifies the number of times to retry the connection to Azure Cosmos DB in case of transient failures (e.g. network connectivity interruption).</span></span>
6. <span data-ttu-id="bcc86-316">Återförsöksintervall: Anger hur länge väntetiden mellan anslutningsförsök görs till Azure Cosmos DB vid tillfälligt fel (t.ex. anslutningen avbrott i nätverket).</span><span class="sxs-lookup"><span data-stu-id="bcc86-316">Retry Interval: Specifies how long to wait between retrying the connection to Azure Cosmos DB in case of transient failures (e.g. network connectivity interruption).</span></span>
7. <span data-ttu-id="bcc86-317">Anslutningsläge: Anger Anslutningsläge ska användas med Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="bcc86-317">Connection Mode: Specifies the connection mode to use with Azure Cosmos DB.</span></span> <span data-ttu-id="bcc86-318">Tillgängliga alternativ är DirectTcp, DirectHttps och Gateway.</span><span class="sxs-lookup"><span data-stu-id="bcc86-318">The available choices are DirectTcp, DirectHttps, and Gateway.</span></span> <span data-ttu-id="bcc86-319">Direktanslutning lägena är snabbare, medan gateway-läge är mer brandvägg eget eftersom den endast använder port 443.</span><span class="sxs-lookup"><span data-stu-id="bcc86-319">The direct connection modes are faster, while the gateway mode is more firewall friendly as it only uses port 443.</span></span>

![Skärmbild av Azure Cosmos DB massimport avancerade alternativ](./media/import-data/docdbbulkoptions.png)

> [!TIP]
> <span data-ttu-id="bcc86-321">Importverktyget standard anslutningsläge DirectTcp.</span><span class="sxs-lookup"><span data-stu-id="bcc86-321">The import tool defaults to connection mode DirectTcp.</span></span> <span data-ttu-id="bcc86-322">Om det uppstår brandväggsproblem med kan växla till anslutningsläge Gateway, eftersom den kräver endast port 443.</span><span class="sxs-lookup"><span data-stu-id="bcc86-322">If you experience firewall issues, switch to connection mode Gateway, as it only requires port 443.</span></span>
> 
> 

## <span data-ttu-id="bcc86-323"><a id="DocumentDBSeqTarget"></a>Importera till DocumentDB-API (sekventiella post importera)</span><span class="sxs-lookup"><span data-stu-id="bcc86-323"><a id="DocumentDBSeqTarget"></a>To import to the DocumentDB API (Sequential Record Import)</span></span>
<span data-ttu-id="bcc86-324">Importverktyget för Azure Cosmos DB sekventiella post kan du importera från något av alternativen på grundval av post med tillgängliga källservrar.</span><span class="sxs-lookup"><span data-stu-id="bcc86-324">The Azure Cosmos DB sequential record importer allows you to import from any of the available source options on a record by record basis.</span></span> <span data-ttu-id="bcc86-325">Du kan välja det här alternativet om du importerar till en befintlig samling som har uppnått sin kvot av lagrade procedurer.</span><span class="sxs-lookup"><span data-stu-id="bcc86-325">You might choose this option if you’re importing to an existing collection that has reached its quota of stored procedures.</span></span> <span data-ttu-id="bcc86-326">Verktyget stöder import till en enda (enskild partition och flera partition) Azure Cosmos DB-samling som delat import genom vilken data är partitionerad över flera enskild partition och/eller flera partition Azure DB som Cosmos-samlingar.</span><span class="sxs-lookup"><span data-stu-id="bcc86-326">The tool supports import to a single (both single-partition and multi-partition) Azure Cosmos DB collection, as well as sharded import whereby data is partitioned across multiple single-partition and/or multi-partition Azure Cosmos DB collections.</span></span> <span data-ttu-id="bcc86-327">Mer information om partitionering data finns [partitionering och skalning i Azure Cosmos DB](partition-data.md).</span><span class="sxs-lookup"><span data-stu-id="bcc86-327">For more information about partitioning data, see [Partitioning and scaling in Azure Cosmos DB](partition-data.md).</span></span>

![Skärmbild av Azure Cosmos DB importalternativ för sekventiella poster](./media/import-data/documentdbsequential.png)

<span data-ttu-id="bcc86-329">Formatet för anslutningssträngen för Azure Cosmos DB är:</span><span class="sxs-lookup"><span data-stu-id="bcc86-329">The format of the Azure Cosmos DB connection string is:</span></span>

    AccountEndpoint=<CosmosDB Endpoint>;AccountKey=<CosmosDB Key>;Database=<CosmosDB Database>;

<span data-ttu-id="bcc86-330">Anslutningssträngen för Azure DB som Cosmos-konto kan hämtas från bladet nycklar i Azure-portalen, enligt beskrivningen i [så här hanterar du ett konto i Azure Cosmos DB](manage-account.md), men namnet på databasen måste läggas till anslutning sträng i följande format:</span><span class="sxs-lookup"><span data-stu-id="bcc86-330">The Azure Cosmos DB account connection string can be retrieved from the Keys blade of the Azure portal, as described in [How to manage an Azure Cosmos DB account](manage-account.md), however the name of the database needs to be appended to the connection string in the following format:</span></span>

    Database=<Azure Cosmos DB Database>;

> [!NOTE]
> <span data-ttu-id="bcc86-331">Använd kommandot Kontrollera så att den angivna i strängen anslutningsfältet Azure DB som Cosmos-instansen är tillgänglig.</span><span class="sxs-lookup"><span data-stu-id="bcc86-331">Use the Verify command to ensure that the Azure Cosmos DB instance specified in the connection string field can be accessed.</span></span>
> 
> 

<span data-ttu-id="bcc86-332">Ange namnet på den samling som data ska importeras och klicka på Lägg till om du vill importera till en enda samling.</span><span class="sxs-lookup"><span data-stu-id="bcc86-332">To import to a single collection, enter the name of the collection to which data will be imported and click the Add button.</span></span> <span data-ttu-id="bcc86-333">Om du vill importera till flera samlingar, ange varje samlingsnamn individuellt eller Använd följande syntax för att ange flera samlingar: *collection_prefix*[startIndex - end index].</span><span class="sxs-lookup"><span data-stu-id="bcc86-333">To import to multiple collections, either enter each collection name individually or use the following syntax to specify multiple collections: *collection_prefix*[start index - end index].</span></span> <span data-ttu-id="bcc86-334">Tänk på följande när du anger flera samlingar via ovannämnda syntax:</span><span class="sxs-lookup"><span data-stu-id="bcc86-334">When specifying multiple collections via the aforementioned syntax, keep the following in mind:</span></span>

1. <span data-ttu-id="bcc86-335">Endast heltal intervallet namnet mönster stöds.</span><span class="sxs-lookup"><span data-stu-id="bcc86-335">Only integer range name patterns are supported.</span></span> <span data-ttu-id="bcc86-336">Till exempel ange samlingen [0-3] genererar följande samlingar: collection0, collection1, collection2 collection3.</span><span class="sxs-lookup"><span data-stu-id="bcc86-336">For example, specifying collection[0-3] will produce the following collections: collection0, collection1, collection2, collection3.</span></span>
2. <span data-ttu-id="bcc86-337">Du kan använda en förkortad syntax: samlingen [3] genererar samma uppsättning samlingar som nämns i steg 1.</span><span class="sxs-lookup"><span data-stu-id="bcc86-337">You can use an abbreviated syntax: collection[3] will emit same set of collections mentioned in step 1.</span></span>
3. <span data-ttu-id="bcc86-338">Mer än en ersättning kan anges.</span><span class="sxs-lookup"><span data-stu-id="bcc86-338">More than one substitution can be provided.</span></span> <span data-ttu-id="bcc86-339">Till exempel samlingen [0-1] [0-9] genererar 20 samlingsnamn med nollor (collection01... 02... 03).</span><span class="sxs-lookup"><span data-stu-id="bcc86-339">For example, collection[0-1] [0-9] will generate 20 collection names with leading zeros (collection01, ..02, ..03).</span></span>

<span data-ttu-id="bcc86-340">När samlingen namn har angetts, väljer du önskad genomflödet av samling(ar) (400 RUs till 250 000 RUs).</span><span class="sxs-lookup"><span data-stu-id="bcc86-340">Once the collection name(s) have been specified, choose the desired throughput of the collection(s) (400 RUs to 250,000 RUs).</span></span> <span data-ttu-id="bcc86-341">Välj en högre genomströmning för bästa prestanda för import.</span><span class="sxs-lookup"><span data-stu-id="bcc86-341">For best import performance, choose a higher throughput.</span></span> <span data-ttu-id="bcc86-342">Läs mer om prestandanivåer [prestandanivåer i Azure Cosmos DB](performance-levels.md).</span><span class="sxs-lookup"><span data-stu-id="bcc86-342">For more information about performance levels, see [Performance levels in Azure Cosmos DB](performance-levels.md).</span></span> <span data-ttu-id="bcc86-343">Import till samlingar med genomströmning > 10 000 RUs kräver en partitionsnyckel.</span><span class="sxs-lookup"><span data-stu-id="bcc86-343">Any import to collections with throughput >10,000 RUs will require a partition key.</span></span> <span data-ttu-id="bcc86-344">Om du vill ha mer än 250 000 RUs måste till filen en förfrågan till ditt konto ökade Portal.</span><span class="sxs-lookup"><span data-stu-id="bcc86-344">If you choose to have more than 250,000 RUs, you will need to file a request in the portal to have your account increased.</span></span>

> [!NOTE]
> <span data-ttu-id="bcc86-345">Genomströmning inställningen gäller bara skapa en samling.</span><span class="sxs-lookup"><span data-stu-id="bcc86-345">The throughput setting only applies to collection creation.</span></span> <span data-ttu-id="bcc86-346">Om den angivna samlingen finns redan, ändras inte dess genomflöde.</span><span class="sxs-lookup"><span data-stu-id="bcc86-346">If the specified collection already exists, its throughput will not be modified.</span></span>
> 
> 

<span data-ttu-id="bcc86-347">När du importerar till flera samlingar baserat import verktyget stöder hash horisontell partitionering.</span><span class="sxs-lookup"><span data-stu-id="bcc86-347">When importing to multiple collections, the import tool supports hash based sharding.</span></span> <span data-ttu-id="bcc86-348">I det här scenariot, ange egenskapen document som du vill använda som partitionsnyckel (om Partitionsnyckeln är tomt dokument är delat slumpmässigt över mål samlingarna).</span><span class="sxs-lookup"><span data-stu-id="bcc86-348">In this scenario, specify the document property you wish to use as the Partition Key (if Partition Key is left blank, documents will be sharded randomly across the target collections).</span></span>

<span data-ttu-id="bcc86-349">Du kan ange vilket fält i import-källa som ska användas som egenskapen Azure Cosmos DB dokument-id vid import (Observera att om dokument inte innehåller den här egenskapen, sedan importera verktyget att generera ett GUID som egenskapsvärdet id).</span><span class="sxs-lookup"><span data-stu-id="bcc86-349">You may optionally specify which field in the import source should be used as the Azure Cosmos DB document id property during the import (note that if documents do not contain this property, then the import tool will generate a GUID as the id property value).</span></span>

<span data-ttu-id="bcc86-350">Det finns ett antal avancerade alternativ under importen.</span><span class="sxs-lookup"><span data-stu-id="bcc86-350">There are a number of advanced options available during import.</span></span> <span data-ttu-id="bcc86-351">När du importerar datum typer (t.ex. från SQL Server eller MongoDB) måste välja du först mellan tre importalternativ:</span><span class="sxs-lookup"><span data-stu-id="bcc86-351">First, when importing date types (e.g. from SQL Server or MongoDB), you can choose between three import options:</span></span>

 ![Skärmbild av Azure Cosmos DB datum tid importalternativ](./media/import-data/datetimeoptions.png)

* <span data-ttu-id="bcc86-353">Sträng: Spara som ett strängvärde</span><span class="sxs-lookup"><span data-stu-id="bcc86-353">String: Persist as a string value</span></span>
* <span data-ttu-id="bcc86-354">Epok: Spara som en epok numeriskt värde</span><span class="sxs-lookup"><span data-stu-id="bcc86-354">Epoch: Persist as an Epoch number value</span></span>
* <span data-ttu-id="bcc86-355">Både: Spara både sträng och epok numeriska värden.</span><span class="sxs-lookup"><span data-stu-id="bcc86-355">Both: Persist both string and Epoch number values.</span></span> <span data-ttu-id="bcc86-356">Det här alternativet skapas en underdokument, till exempel: ”date_joined”: {”värde” ”: 2013-10-21T21:17:25.2410000Z” ”, epok”: 1382390245}</span><span class="sxs-lookup"><span data-stu-id="bcc86-356">This option will create a subdocument, for example: "date_joined": { "Value": "2013-10-21T21:17:25.2410000Z", "Epoch": 1382390245 }</span></span>

<span data-ttu-id="bcc86-357">Azure Cosmos DB - sekventiella post Importverktyget har följande ytterligare avancerade alternativ:</span><span class="sxs-lookup"><span data-stu-id="bcc86-357">The Azure Cosmos DB - Sequential record importer has the following additional advanced options:</span></span>

1. <span data-ttu-id="bcc86-358">Antalet parallella begäranden: verktyget standardvärdet 2 parallella begäranden.</span><span class="sxs-lookup"><span data-stu-id="bcc86-358">Number of Parallel Requests: The tool defaults to 2 parallel requests.</span></span> <span data-ttu-id="bcc86-359">Överväg att öka antalet parallella begäranden om dokument som ska importeras är små.</span><span class="sxs-lookup"><span data-stu-id="bcc86-359">If the documents to be imported are small, consider raising the number of parallel requests.</span></span> <span data-ttu-id="bcc86-360">Observera att det här antalet utlöses för mycket importen kan uppstå om begränsning.</span><span class="sxs-lookup"><span data-stu-id="bcc86-360">Note that if this number is raised too much, the import may experience throttling.</span></span>
2. <span data-ttu-id="bcc86-361">Inaktivera automatisk generering: Om alla dokument som ska importeras innehåller ett ID-fält kan det här alternativet kan öka prestanda.</span><span class="sxs-lookup"><span data-stu-id="bcc86-361">Disable Automatic Id Generation: If every document to be imported contains an id field, then selecting this option can increase performance.</span></span> <span data-ttu-id="bcc86-362">Kommer inte att importera dokument som har ett unikt id-fält saknas.</span><span class="sxs-lookup"><span data-stu-id="bcc86-362">Documents missing a unique id field will not be imported.</span></span>
3. <span data-ttu-id="bcc86-363">Uppdatera befintliga dokument: Verktyget som standard inte ersätta befintliga dokument med id-konflikter.</span><span class="sxs-lookup"><span data-stu-id="bcc86-363">Update Existing Documents: The tool defaults to not replacing existing documents with id conflicts.</span></span> <span data-ttu-id="bcc86-364">Det här alternativet kan skriva över befintliga dokument med matchande ID: n.</span><span class="sxs-lookup"><span data-stu-id="bcc86-364">Selecting this option will allow overwriting existing documents with matching ids.</span></span> <span data-ttu-id="bcc86-365">Den här funktionen är användbart för schemalagd datauppsättning migreringar som uppdaterar befintliga dokument.</span><span class="sxs-lookup"><span data-stu-id="bcc86-365">This feature is useful for scheduled data migrations that update existing documents.</span></span>
4. <span data-ttu-id="bcc86-366">Antalet återförsök vid fel: Anger antalet gånger för att försöka anslutningen till Azure Cosmos DB vid tillfälligt fel (t.ex. anslutningen avbrott i nätverket).</span><span class="sxs-lookup"><span data-stu-id="bcc86-366">Number of Retries on Failure: Specifies the number of times to retry the connection to Azure Cosmos DB in case of transient failures (e.g. network connectivity interruption).</span></span>
5. <span data-ttu-id="bcc86-367">Återförsöksintervall: Anger hur länge väntetiden mellan anslutningsförsök görs till Azure Cosmos DB vid tillfälligt fel (t.ex. anslutningen avbrott i nätverket).</span><span class="sxs-lookup"><span data-stu-id="bcc86-367">Retry Interval: Specifies how long to wait between retrying the connection to Azure Cosmos DB in case of transient failures (e.g. network connectivity interruption).</span></span>
6. <span data-ttu-id="bcc86-368">Anslutningsläge: Anger Anslutningsläge ska användas med Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="bcc86-368">Connection Mode: Specifies the connection mode to use with Azure Cosmos DB.</span></span> <span data-ttu-id="bcc86-369">Tillgängliga alternativ är DirectTcp, DirectHttps och Gateway.</span><span class="sxs-lookup"><span data-stu-id="bcc86-369">The available choices are DirectTcp, DirectHttps, and Gateway.</span></span> <span data-ttu-id="bcc86-370">Direktanslutning lägena är snabbare, medan gateway-läge är mer brandvägg eget eftersom den endast använder port 443.</span><span class="sxs-lookup"><span data-stu-id="bcc86-370">The direct connection modes are faster, while the gateway mode is more firewall friendly as it only uses port 443.</span></span>

![Skärmbild av Azure Cosmos DB sekventiella post importera avancerade alternativ](./media/import-data/documentdbsequentialoptions.png)

> [!TIP]
> <span data-ttu-id="bcc86-372">Importverktyget standard anslutningsläge DirectTcp.</span><span class="sxs-lookup"><span data-stu-id="bcc86-372">The import tool defaults to connection mode DirectTcp.</span></span> <span data-ttu-id="bcc86-373">Om det uppstår brandväggsproblem med kan växla till anslutningsläge Gateway, eftersom den kräver endast port 443.</span><span class="sxs-lookup"><span data-stu-id="bcc86-373">If you experience firewall issues, switch to connection mode Gateway, as it only requires port 443.</span></span>
> 
> 

## <span data-ttu-id="bcc86-374"><a id="IndexingPolicy"></a>Ange ett indexprincip när du skapar Azure Cosmos DB samlingar</span><span class="sxs-lookup"><span data-stu-id="bcc86-374"><a id="IndexingPolicy"></a>Specify an indexing policy when creating Azure Cosmos DB collections</span></span>
<span data-ttu-id="bcc86-375">När du tillåter Migreringsverktyget att skapa samlingar under importen kan du ange indexprincip samlingarna.</span><span class="sxs-lookup"><span data-stu-id="bcc86-375">When you allow the migration tool to create collections during import, you can specify the indexing policy of the collections.</span></span> <span data-ttu-id="bcc86-376">Gå till avsnittet indexering princip i avsnittet avancerade alternativ i Azure Cosmos DB massimport och alternativ för Azure Cosmos DB sekventiella poster.</span><span class="sxs-lookup"><span data-stu-id="bcc86-376">In the advanced options section of the Azure Cosmos DB Bulk import and Azure Cosmos DB Sequential record options, navigate to the Indexing Policy section.</span></span>

![Skärmbild av Azure Cosmos DB indexering princip avancerade alternativ](./media/import-data/indexingpolicy1.png)

<span data-ttu-id="bcc86-378">Med principer för indexering avancerade alternativ, kan du välja en indexering principfil, manuellt ange ett indexprincip, eller välj från en uppsättning standardmallar (genom att högerklicka i textrutan indexering princip).</span><span class="sxs-lookup"><span data-stu-id="bcc86-378">Using the Indexing Policy advanced option, you can select an indexing policy file, manually enter an indexing policy, or select from a set of default templates (by right clicking in the indexing policy textbox).</span></span>

<span data-ttu-id="bcc86-379">Verktyget ger principmallarna är:</span><span class="sxs-lookup"><span data-stu-id="bcc86-379">The policy templates the tool provides are:</span></span>

* <span data-ttu-id="bcc86-380">Som standard.</span><span class="sxs-lookup"><span data-stu-id="bcc86-380">Default.</span></span> <span data-ttu-id="bcc86-381">Den här principen är bäst när du utför likhetsfrågor för strängar och använda ORDER BY, intervalls och likhetsfrågor för tal.</span><span class="sxs-lookup"><span data-stu-id="bcc86-381">This policy is best when you’re performing equality queries against strings and using ORDER BY, range, and equality queries for numbers.</span></span> <span data-ttu-id="bcc86-382">Den här principen har en lägre Omkostnad för indexlagring än intervall.</span><span class="sxs-lookup"><span data-stu-id="bcc86-382">This policy has a lower index storage overhead than Range.</span></span>
* <span data-ttu-id="bcc86-383">Intervall.</span><span class="sxs-lookup"><span data-stu-id="bcc86-383">Range.</span></span> <span data-ttu-id="bcc86-384">Den här principen är bäst om du använder ORDER BY, intervalls och likhetsfrågor för både tal och strängar.</span><span class="sxs-lookup"><span data-stu-id="bcc86-384">This policy is best you’re using ORDER BY, range and equality queries on both numbers and strings.</span></span> <span data-ttu-id="bcc86-385">Den här principen har en högre Omkostnad för indexlagring än standard eller Hash.</span><span class="sxs-lookup"><span data-stu-id="bcc86-385">This policy has a higher index storage overhead than Default or Hash.</span></span>

![Skärmbild av Azure Cosmos DB indexering princip avancerade alternativ](./media/import-data/indexingpolicy2.png)

> [!NOTE]
> <span data-ttu-id="bcc86-387">Om du inte anger en indexprincip tillämpas standardprincipen.</span><span class="sxs-lookup"><span data-stu-id="bcc86-387">If you do not specify an indexing policy, then the default policy will be applied.</span></span> <span data-ttu-id="bcc86-388">Mer information om principer för fulltextindexering finns [Azure Cosmos DB indexering principer](indexing-policies.md).</span><span class="sxs-lookup"><span data-stu-id="bcc86-388">For more information about indexing policies, see [Azure Cosmos DB indexing policies](indexing-policies.md).</span></span>
> 
> 

## <a name="export-to-json-file"></a><span data-ttu-id="bcc86-389">Exportera till JSON-fil</span><span class="sxs-lookup"><span data-stu-id="bcc86-389">Export to JSON file</span></span>
<span data-ttu-id="bcc86-390">Exportverktyget Azure Cosmos DB JSON kan du exportera någon av de tillgängliga alternativ till en JSON-fil som innehåller en matris av JSON-dokument.</span><span class="sxs-lookup"><span data-stu-id="bcc86-390">The Azure Cosmos DB JSON exporter allows you to export any of the available source options to a JSON file that contains an array of JSON documents.</span></span> <span data-ttu-id="bcc86-391">Verktyget hanterar export du eller kan du visa det resulterande migrering kommandot och kör kommandot själv.</span><span class="sxs-lookup"><span data-stu-id="bcc86-391">The tool will handle the export for you, or you can choose to view the resulting migration command and run the command yourself.</span></span> <span data-ttu-id="bcc86-392">Den resulterande JSON-filen vara lagrade lokalt eller i Azure Blob storage.</span><span class="sxs-lookup"><span data-stu-id="bcc86-392">The resulting JSON file may be stored locally or in Azure Blob storage.</span></span>

![Skärmbild av Azure Cosmos DB JSON lokal fil exportalternativ](./media/import-data/jsontarget.png)

![Skärmbild av Azure Cosmos DB JSON Azure Blob storage-exportalternativ](./media/import-data/jsontarget2.png)

<span data-ttu-id="bcc86-395">Du kan välja att prettify resulterande JSON som ökar storleken på resulterande dokumentet när innehållet mer läsbar.</span><span class="sxs-lookup"><span data-stu-id="bcc86-395">You may optionally choose to prettify the resulting JSON, which will increase the size of the resulting document while making the contents more human readable.</span></span>

    Standard JSON export
    [{"id":"Sample","Title":"About Paris","Language":{"Name":"English"},"Author":{"Name":"Don","Location":{"City":"Paris","Country":"France"}},"Content":"Don's document in Azure Cosmos DB is a valid JSON document as defined by the JSON spec.","PageViews":10000,"Topics":[{"Title":"History of Paris"},{"Title":"Places to see in Paris"}]}]

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
    "Content": "Don's document in Azure Cosmos DB is a valid JSON document as defined by the JSON spec.",
    "PageViews": 10000,
    "Topics": [
      {
        "Title": "History of Paris"
      },
      {
        "Title": "Places to see in Paris"
      }
    ]
    }]

## <a name="advanced-configuration"></a><span data-ttu-id="bcc86-396">Avancerad konfiguration</span><span class="sxs-lookup"><span data-stu-id="bcc86-396">Advanced configuration</span></span>
<span data-ttu-id="bcc86-397">Ange platsen för filen som du vill att eventuella fel som skrivs på skärmen avancerad konfiguration.</span><span class="sxs-lookup"><span data-stu-id="bcc86-397">In the Advanced configuration screen, specify the location of the log file to which you would like any errors written.</span></span> <span data-ttu-id="bcc86-398">Följande regler gäller för den här sidan:</span><span class="sxs-lookup"><span data-stu-id="bcc86-398">The following rules apply to this page:</span></span>

1. <span data-ttu-id="bcc86-399">Om ett filnamn inte anges returneras alla fel på resultatsidan.</span><span class="sxs-lookup"><span data-stu-id="bcc86-399">If a file name is not provided, then all errors will be returned on the Results page.</span></span>
2. <span data-ttu-id="bcc86-400">Om ett filnamn anges utan en katalog, ska sedan filen skapas (eller skrivs över) i den aktuella katalogen i miljön.</span><span class="sxs-lookup"><span data-stu-id="bcc86-400">If a file name is provided without a directory, then the file will be created (or overwritten) in the current environment directory.</span></span>
3. <span data-ttu-id="bcc86-401">Om du väljer en befintlig fil och sedan filen kommer att skrivas över finns det inget alternativ för Lägg till.</span><span class="sxs-lookup"><span data-stu-id="bcc86-401">If you select an existing file, then the file will be overwritten, there is no append option.</span></span>

<span data-ttu-id="bcc86-402">Välj om du vill logga alla, kritisk, eller inga felmeddelanden.</span><span class="sxs-lookup"><span data-stu-id="bcc86-402">Then, choose whether to log all, critical, or no error messages.</span></span> <span data-ttu-id="bcc86-403">Slutligen besluta hur ofta på skärmen överföring meddelandet kommer att uppdateras med dess förlopp.</span><span class="sxs-lookup"><span data-stu-id="bcc86-403">Finally, decide how frequently the on screen transfer message will be updated with its progress.</span></span>

    ![Screenshot of Advanced configuration screen](./media/import-data/AdvancedConfiguration.png)

## <a name="confirm-import-settings-and-view-command-line"></a><span data-ttu-id="bcc86-404">Bekräfta importera inställningar och visa kommandoraden</span><span class="sxs-lookup"><span data-stu-id="bcc86-404">Confirm import settings and view command line</span></span>
1. <span data-ttu-id="bcc86-405">När du har angett information om källdatorn och målet information avancerad konfiguration, granska översikt över migreringen och du kan också visa/kopiera kommandot resulterande migrering (kopiera kommandot är användbart för att automatisera importåtgärder):</span><span class="sxs-lookup"><span data-stu-id="bcc86-405">After specifying source information, target information, and advanced configuration, review the migration summary and, optionally, view/copy the resulting migration command (copying the command is useful to automate import operations):</span></span>
   
    ![Skärmbild av översiktsskärm](./media/import-data/summary.png)
   
    ![Skärmbild av översiktsskärm](./media/import-data/summarycommand.png)
2. <span data-ttu-id="bcc86-408">När du är nöjd med din käll- och alternativ klickar du på **importera**.</span><span class="sxs-lookup"><span data-stu-id="bcc86-408">Once you’re satisfied with your source and target options, click **Import**.</span></span> <span data-ttu-id="bcc86-409">Förfluten tid, antal överförda och felinformation (om du inte anger ett filnamn i avancerad konfiguration) uppdateras när importen pågår.</span><span class="sxs-lookup"><span data-stu-id="bcc86-409">The elapsed time, transferred count, and failure information (if you didn't provide a file name in the Advanced configuration) will update as the import is in process.</span></span> <span data-ttu-id="bcc86-410">När installationen är klar kan du exportera resultaten (t.ex. att åtgärda eventuella importfel).</span><span class="sxs-lookup"><span data-stu-id="bcc86-410">Once complete, you can export the results (e.g. to deal with any import failures).</span></span>
   
    ![Skärmbild av Azure Cosmos DB JSON exportalternativ](./media/import-data/viewresults.png)
3. <span data-ttu-id="bcc86-412">Du kan också starta en ny import antingen behålla de befintliga inställningarna (t.ex. anslutning sträng information, källa och mål för val osv.) eller återställa alla värden.</span><span class="sxs-lookup"><span data-stu-id="bcc86-412">You may also start a new import, either keeping the existing settings (e.g. connection string information, source and target choice, etc.) or resetting all values.</span></span>
   
    ![Skärmbild av Azure Cosmos DB JSON exportalternativ](./media/import-data/newimport.png)

## <a name="next-steps"></a><span data-ttu-id="bcc86-414">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="bcc86-414">Next steps</span></span>

<span data-ttu-id="bcc86-415">I den här självstudiekursen kommer du har gjort följande:</span><span class="sxs-lookup"><span data-stu-id="bcc86-415">In this tutorial, you've done the following:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="bcc86-416">Installerat verktyget för migrering av Data</span><span class="sxs-lookup"><span data-stu-id="bcc86-416">Installed the Data Migration tool</span></span>
> * <span data-ttu-id="bcc86-417">Importerade data från olika datakällor</span><span class="sxs-lookup"><span data-stu-id="bcc86-417">Imported data from different data sources</span></span>
> * <span data-ttu-id="bcc86-418">Exporteras från Azure Cosmos DB till JSON</span><span class="sxs-lookup"><span data-stu-id="bcc86-418">Exported from Azure Cosmos DB to JSON</span></span>

<span data-ttu-id="bcc86-419">Du kan nu gå vidare till nästa kurs och lär dig hur du frågar efter data med hjälp av Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="bcc86-419">You can now proceed to the next tutorial and learn how to query data using Azure Cosmos DB.</span></span> 

> [!div class="nextstepaction"]
>[<span data-ttu-id="bcc86-420">Hur du frågar efter data?</span><span class="sxs-lookup"><span data-stu-id="bcc86-420">How to query data?</span></span>](../cosmos-db/tutorial-query-documentdb.md)
