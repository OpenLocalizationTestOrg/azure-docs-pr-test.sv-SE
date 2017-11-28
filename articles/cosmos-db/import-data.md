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
# <a name="how-tooimport-data-into-azure-cosmos-db-for-hello-documentdb-api"></a><span data-ttu-id="e3e9d-105">Hur tooimport data till Azure Cosmos DB för hello API DocumentDB?</span><span class="sxs-lookup"><span data-stu-id="e3e9d-105">How tooimport data into Azure Cosmos DB for hello DocumentDB API?</span></span>

<span data-ttu-id="e3e9d-106">Den här självstudiekursen innehåller anvisningar om hur du använder hello Azure Cosmos DB: DocumentDB API datamigrering verktyg som kan importera data från olika källor, inklusive JSON-filer, CSV-filer, SQL, MongoDB, Azure Table storage, Amazon DynamoDB och Azure Cosmos DB DocumentDB API-samlingar i samlingar för använda med Azure Cosmos DB och hello DocumentDB-API.</span><span class="sxs-lookup"><span data-stu-id="e3e9d-106">This tutorial provides instructions on using hello Azure Cosmos DB: DocumentDB API Data Migration tool, which can import data from various sources, including JSON files, CSV files, SQL, MongoDB, Azure Table storage, Amazon DynamoDB and Azure Cosmos DB DocumentDB API collections into collections for use with Azure Cosmos DB and hello DocumentDB API.</span></span> <span data-ttu-id="e3e9d-107">Hej datamigreringsverktyget kan också användas när du migrerar från en enda partition samling tooa flera partition samling för hello DocumentDB-API.</span><span class="sxs-lookup"><span data-stu-id="e3e9d-107">hello Data Migration tool can also be used when migrating from a single partition collection tooa multi-partition collection for hello DocumentDB API.</span></span>

<span data-ttu-id="e3e9d-108">Hej datamigreringsverktyget fungerar endast när importera data till Azure Cosmos DB för använder med hello DocumentDB-API.</span><span class="sxs-lookup"><span data-stu-id="e3e9d-108">hello Data Migration tool only works when importing data into Azure Cosmos DB for use with hello DocumentDB API.</span></span> <span data-ttu-id="e3e9d-109">Importerar data för användning med hello tabell API eller Graph API stöds inte just nu.</span><span class="sxs-lookup"><span data-stu-id="e3e9d-109">Importing data for use with hello Table API or Graph API is not supported at this time.</span></span> 

<span data-ttu-id="e3e9d-110">tooimport data för användning med hello MongoDB-API, se [Azure Cosmos DB: hur toomigrate data för hello MongoDB API?](mongodb-migrate.md).</span><span class="sxs-lookup"><span data-stu-id="e3e9d-110">tooimport data for use with hello MongoDB API, see [Azure Cosmos DB: How toomigrate data for hello MongoDB API?](mongodb-migrate.md).</span></span>

<span data-ttu-id="e3e9d-111">Den här kursen ingår hello följande uppgifter:</span><span class="sxs-lookup"><span data-stu-id="e3e9d-111">This tutorial covers hello following tasks:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="e3e9d-112">Installera hello datamigreringsverktyget</span><span class="sxs-lookup"><span data-stu-id="e3e9d-112">Installing hello Data Migration tool</span></span>
> * <span data-ttu-id="e3e9d-113">Importera data från olika datakällor</span><span class="sxs-lookup"><span data-stu-id="e3e9d-113">Importing data from different data sources</span></span>
> * <span data-ttu-id="e3e9d-114">Exportera från Azure Cosmos DB tooJSON</span><span class="sxs-lookup"><span data-stu-id="e3e9d-114">Exporting from Azure Cosmos DB tooJSON</span></span>

## <span data-ttu-id="e3e9d-115"><a id="Prerequisites"></a>Förhandskrav</span><span class="sxs-lookup"><span data-stu-id="e3e9d-115"><a id="Prerequisites"></a>Prerequisites</span></span>
<span data-ttu-id="e3e9d-116">Innan du följer hello anvisningarna i den här artikeln bör du kontrollera att du har hello följande installerat:</span><span class="sxs-lookup"><span data-stu-id="e3e9d-116">Before following hello instructions in this article, ensure that you have hello following installed:</span></span>

* <span data-ttu-id="e3e9d-117">[Microsoft .NET Framework 4.51](https://www.microsoft.com/download/developer-tools.aspx) eller högre.</span><span class="sxs-lookup"><span data-stu-id="e3e9d-117">[Microsoft .NET Framework 4.51](https://www.microsoft.com/download/developer-tools.aspx) or higher.</span></span>

## <span data-ttu-id="e3e9d-118"><a id="Overviewl"></a>Översikt över hello datamigreringsverktyget</span><span class="sxs-lookup"><span data-stu-id="e3e9d-118"><a id="Overviewl"></a>Overview of hello Data Migration tool</span></span>
<span data-ttu-id="e3e9d-119">Hej datamigreringsverktyget är en öppen källkod som importerar data tooAzure Cosmos DB från olika källor, inklusive:</span><span class="sxs-lookup"><span data-stu-id="e3e9d-119">hello Data Migration tool is an open source solution that imports data tooAzure Cosmos DB from a variety of sources, including:</span></span>

* <span data-ttu-id="e3e9d-120">JSON-filer</span><span class="sxs-lookup"><span data-stu-id="e3e9d-120">JSON files</span></span>
* <span data-ttu-id="e3e9d-121">MongoDB</span><span class="sxs-lookup"><span data-stu-id="e3e9d-121">MongoDB</span></span>
* <span data-ttu-id="e3e9d-122">SQL Server</span><span class="sxs-lookup"><span data-stu-id="e3e9d-122">SQL Server</span></span>
* <span data-ttu-id="e3e9d-123">CSV-filer</span><span class="sxs-lookup"><span data-stu-id="e3e9d-123">CSV files</span></span>
* <span data-ttu-id="e3e9d-124">Azure Table Storage</span><span class="sxs-lookup"><span data-stu-id="e3e9d-124">Azure Table storage</span></span>
* <span data-ttu-id="e3e9d-125">Amazon DynamoDB</span><span class="sxs-lookup"><span data-stu-id="e3e9d-125">Amazon DynamoDB</span></span>
* <span data-ttu-id="e3e9d-126">HBase</span><span class="sxs-lookup"><span data-stu-id="e3e9d-126">HBase</span></span>
* <span data-ttu-id="e3e9d-127">Azure DB Cosmos-samlingar</span><span class="sxs-lookup"><span data-stu-id="e3e9d-127">Azure Cosmos DB collections</span></span>

<span data-ttu-id="e3e9d-128">Medan hello-Importverktyget innehåller ett grafiskt användargränssnitt (dtui.exe), kan den också drivas från hello kommandorad (dt.exe).</span><span class="sxs-lookup"><span data-stu-id="e3e9d-128">While hello import tool includes a graphical user interface (dtui.exe), it can also be driven from hello command line (dt.exe).</span></span> <span data-ttu-id="e3e9d-129">Det finns i själva verket ett alternativet toooutput hello associerade kommando när du har installerat en import via hello Användargränssnittet.</span><span class="sxs-lookup"><span data-stu-id="e3e9d-129">In fact, there is an option toooutput hello associated command after setting up an import through hello UI.</span></span> <span data-ttu-id="e3e9d-130">Tabell källdata (t.ex. SQL Server- eller CSV-filer) kan omvandlas så att hierarkiska relationer (underdokument) kan skapas under importen.</span><span class="sxs-lookup"><span data-stu-id="e3e9d-130">Tabular source data (e.g. SQL Server or CSV files) can be transformed such that hierarchical relationships (subdocuments) can be created during import.</span></span> <span data-ttu-id="e3e9d-131">Håll läsa toolearn mer om alternativ för datakällor exempel kommandorader tooimport från varje källa, mål-alternativ och visa resultat av import.</span><span class="sxs-lookup"><span data-stu-id="e3e9d-131">Keep reading toolearn more about source options, sample command lines tooimport from each source, target options, and viewing import results.</span></span>

## <span data-ttu-id="e3e9d-132"><a id="Install"></a>Installera verktyget för hello datamigrering</span><span class="sxs-lookup"><span data-stu-id="e3e9d-132"><a id="Install"></a>Install hello Data Migration tool</span></span>
<span data-ttu-id="e3e9d-133">källkoden för hello migrering verktyget finns på GitHub i [den här lagringsplatsen](https://github.com/azure/azure-documentdb-datamigrationtool) och en kompilerad version är tillgänglig från [Microsoft Download Center](http://www.microsoft.com/downloads/details.aspx?FamilyID=cda7703a-2774-4c07-adcc-ad02ddc1a44d).</span><span class="sxs-lookup"><span data-stu-id="e3e9d-133">hello migration tool source code is available on GitHub in [this repository](https://github.com/azure/azure-documentdb-datamigrationtool) and a compiled version is available from [Microsoft Download Center](http://www.microsoft.com/downloads/details.aspx?FamilyID=cda7703a-2774-4c07-adcc-ad02ddc1a44d).</span></span> <span data-ttu-id="e3e9d-134">Du kan kompilera hello lösning eller bara ladda ned och extrahera hello kompilerad version tooa katalogen för ditt val.</span><span class="sxs-lookup"><span data-stu-id="e3e9d-134">You may either compile hello solution or simply download and extract hello compiled version tooa directory of your choice.</span></span> <span data-ttu-id="e3e9d-135">Kör sedan antingen:</span><span class="sxs-lookup"><span data-stu-id="e3e9d-135">Then run either:</span></span>

* <span data-ttu-id="e3e9d-136">**Dtui.exe**: grafiskt gränssnittsversion hello-verktyget</span><span class="sxs-lookup"><span data-stu-id="e3e9d-136">**Dtui.exe**: Graphical interface version of hello tool</span></span>
* <span data-ttu-id="e3e9d-137">**DT.exe**: kommandoraden version hello-verktyget</span><span class="sxs-lookup"><span data-stu-id="e3e9d-137">**Dt.exe**: Command-line version of hello tool</span></span>

## <a name="import-data"></a><span data-ttu-id="e3e9d-138">Importera data</span><span class="sxs-lookup"><span data-stu-id="e3e9d-138">Import data</span></span>

<span data-ttu-id="e3e9d-139">När du har installerat hello-verktyget är det tid tooimport dina data.</span><span class="sxs-lookup"><span data-stu-id="e3e9d-139">Once you've installed hello tool, it's time tooimport your data.</span></span> <span data-ttu-id="e3e9d-140">Vilken typ av data vill du tooimport?</span><span class="sxs-lookup"><span data-stu-id="e3e9d-140">What kind of data do you want tooimport?</span></span>

* [<span data-ttu-id="e3e9d-141">JSON-filer</span><span class="sxs-lookup"><span data-stu-id="e3e9d-141">JSON files</span></span>](#JSON)
* [<span data-ttu-id="e3e9d-142">MongoDB</span><span class="sxs-lookup"><span data-stu-id="e3e9d-142">MongoDB</span></span>](#MongoDB)
* [<span data-ttu-id="e3e9d-143">MongoDB exportfilerna</span><span class="sxs-lookup"><span data-stu-id="e3e9d-143">MongoDB Export files</span></span>](#MongoDBExport)
* [<span data-ttu-id="e3e9d-144">SQL Server</span><span class="sxs-lookup"><span data-stu-id="e3e9d-144">SQL Server</span></span>](#SQL)
* [<span data-ttu-id="e3e9d-145">CSV-filer</span><span class="sxs-lookup"><span data-stu-id="e3e9d-145">CSV files</span></span>](#CSV)
* [<span data-ttu-id="e3e9d-146">Azure Table Storage</span><span class="sxs-lookup"><span data-stu-id="e3e9d-146">Azure Table storage</span></span>](#AzureTableSource)
* [<span data-ttu-id="e3e9d-147">Amazon DynamoDB</span><span class="sxs-lookup"><span data-stu-id="e3e9d-147">Amazon DynamoDB</span></span>](#DynamoDBSource)
* [<span data-ttu-id="e3e9d-148">BLOB</span><span class="sxs-lookup"><span data-stu-id="e3e9d-148">Blob</span></span>](#BlobImport)
* [<span data-ttu-id="e3e9d-149">Azure DB Cosmos-samlingar</span><span class="sxs-lookup"><span data-stu-id="e3e9d-149">Azure Cosmos DB collections</span></span>](#DocumentDBSource)
* [<span data-ttu-id="e3e9d-150">HBase</span><span class="sxs-lookup"><span data-stu-id="e3e9d-150">HBase</span></span>](#HBaseSource)
* [<span data-ttu-id="e3e9d-151">Azure DB Cosmos-massimport</span><span class="sxs-lookup"><span data-stu-id="e3e9d-151">Azure Cosmos DB bulk import</span></span>](#DocumentDBBulkImport)
* [<span data-ttu-id="e3e9d-152">Azure DB Cosmos sekventiella post import</span><span class="sxs-lookup"><span data-stu-id="e3e9d-152">Azure Cosmos DB sequential record import</span></span>](#DocumentDSeqTarget)


## <span data-ttu-id="e3e9d-153"><a id="JSON"></a>tooimport JSON-filer</span><span class="sxs-lookup"><span data-stu-id="e3e9d-153"><a id="JSON"></a>tooimport JSON files</span></span>
<span data-ttu-id="e3e9d-154">hello JSON filalternativet källa Importverktyget kan du tooimport en eller flera dokument JSON-filer eller JSON-filer att var och en innehåller en matris av JSON-dokument.</span><span class="sxs-lookup"><span data-stu-id="e3e9d-154">hello JSON file source importer option allows you tooimport one or more single document JSON files or JSON files that each contain an array of JSON documents.</span></span> <span data-ttu-id="e3e9d-155">När du lägger till mapparna som innehåller tooimport för JSON-filer har hello möjlighet att rekursivt söker efter filer i undermappar.</span><span class="sxs-lookup"><span data-stu-id="e3e9d-155">When adding folders that contain JSON files tooimport, you have hello option of recursively searching for files in subfolders.</span></span>

![Skärmbild av JSON-Filalternativ - Databasverktyg för migrering](./media/import-data/jsonsource.png)

<span data-ttu-id="e3e9d-157">Här följer några kommandoraden prover tooimport JSON-filer:</span><span class="sxs-lookup"><span data-stu-id="e3e9d-157">Here are some command line samples tooimport JSON files:</span></span>

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

## <span data-ttu-id="e3e9d-158"><a id="MongoDB"></a>tooimport från MongoDB</span><span class="sxs-lookup"><span data-stu-id="e3e9d-158"><a id="MongoDB"></a>tooimport from MongoDB</span></span>

> [!IMPORTANT]
> <span data-ttu-id="e3e9d-159">Om du importerar tooan Azure DB som Cosmos-konto med stöd för MongoDB, följer du dessa [instruktioner](mongodb-migrate.md).</span><span class="sxs-lookup"><span data-stu-id="e3e9d-159">If you are importing tooan Azure Cosmos DB account with Support for MongoDB, follow these [instructions](mongodb-migrate.md).</span></span>
> 
> 

<span data-ttu-id="e3e9d-160">hello MongoDB källa Importverktyget alternativet kan du tooimport från en enskild MongoDB-samling och du kan också filtrera dokument med hjälp av en fråga och/eller ändra hello dokumentstruktur med hjälp av en projektion.</span><span class="sxs-lookup"><span data-stu-id="e3e9d-160">hello MongoDB source importer option allows you tooimport from an individual MongoDB collection and optionally filter documents using a query and/or modify hello document structure by using a projection.</span></span>  

![Skärmbild av MongoDB alternativ](./media/import-data/mongodbsource.png)

<span data-ttu-id="e3e9d-162">hello anslutningssträngen är hello standard MongoDB-format:</span><span class="sxs-lookup"><span data-stu-id="e3e9d-162">hello connection string is in hello standard MongoDB format:</span></span>

    mongodb://<dbuser>:<dbpassword>@<host>:<port>/<database>

> [!NOTE]
> <span data-ttu-id="e3e9d-163">Använd hello Kontrollera kommandot tooensure som hello MongoDB-instansen som anges i hello anslutningsfältet sträng kan nås.</span><span class="sxs-lookup"><span data-stu-id="e3e9d-163">Use hello Verify command tooensure that hello MongoDB instance specified in hello connection string field can be accessed.</span></span>
> 
> 

<span data-ttu-id="e3e9d-164">Ange namn på hello mängden hello varifrån data ska importeras.</span><span class="sxs-lookup"><span data-stu-id="e3e9d-164">Enter hello name of hello collection from which data will be imported.</span></span> <span data-ttu-id="e3e9d-165">Du kan om du vill ange eller ange en fil för en fråga (t.ex. {pop: {$gt: 5000}}) och/eller projektion (t.ex. {loc:0}) tooboth filter och form hello data toobe importeras.</span><span class="sxs-lookup"><span data-stu-id="e3e9d-165">You may optionally specify or provide a file for a query (e.g. {pop: {$gt:5000}} ) and/or projection (e.g. {loc:0} ) tooboth filter and shape hello data toobe imported.</span></span>

<span data-ttu-id="e3e9d-166">Här följer några exempel tooimport i kommandoraden från MongoDB:</span><span class="sxs-lookup"><span data-stu-id="e3e9d-166">Here are some command line samples tooimport from MongoDB:</span></span>

    #Import all documents from a MongoDB collection
    dt.exe /s:MongoDB /s.ConnectionString:mongodb://<dbuser>:<dbpassword>@<host>:<port>/<database> /s.Collection:zips /t:CosmosDBBulk /t.ConnectionString:"AccountEndpoint=<CosmosDB Endpoint>;AccountKey=<CosmosDB Key>;Database=<CosmosDB Database>;" /t.Collection:BulkZips /t.IdField:_id /t.CollectionThroughput:2500

    #Import documents from a MongoDB collection which match hello query and exclude hello loc field
    dt.exe /s:MongoDB /s.ConnectionString:mongodb://<dbuser>:<dbpassword>@<host>:<port>/<database> /s.Collection:zips /s.Query:{pop:{$gt:50000}} /s.Projection:{loc:0} /t:CosmosDBBulk /t.ConnectionString:"AccountEndpoint=<CosmosDB Endpoint>;AccountKey=<CosmosDB Key>;Database=<CosmosDB Database>;" /t.Collection:BulkZipsTransform /t.IdField:_id/t.CollectionThroughput:2500

## <span data-ttu-id="e3e9d-167"><a id="MongoDBExport"></a>tooimport MongoDB exportfilerna</span><span class="sxs-lookup"><span data-stu-id="e3e9d-167"><a id="MongoDBExport"></a>tooimport MongoDB export files</span></span>

> [!IMPORTANT]
> <span data-ttu-id="e3e9d-168">Om du importerar tooan Azure DB som Cosmos-konto med stöd för MongoDB, följer du dessa [instruktioner](mongodb-migrate.md).</span><span class="sxs-lookup"><span data-stu-id="e3e9d-168">If you are importing tooan Azure Cosmos DB account with support for MongoDB, follow these [instructions](mongodb-migrate.md).</span></span>
> 
> 

<span data-ttu-id="e3e9d-169">Hej MongoDB export JSON källa Importverktyget filalternativet kan du tooimport en eller flera JSON-filer som genereras från hello mongoexport verktyg.</span><span class="sxs-lookup"><span data-stu-id="e3e9d-169">hello MongoDB export JSON file source importer option allows you tooimport one or more JSON files produced from hello mongoexport utility.</span></span>  

![Skärmbild av MongoDB exportalternativ för källa](./media/import-data/mongodbexportsource.png)

<span data-ttu-id="e3e9d-171">När du lägger till mapparna som innehåller MongoDB export JSON-filer för import, har hello möjlighet att rekursivt söker efter filer i undermappar.</span><span class="sxs-lookup"><span data-stu-id="e3e9d-171">When adding folders that contain MongoDB export JSON files for import, you have hello option of recursively searching for files in subfolders.</span></span>

<span data-ttu-id="e3e9d-172">Här är en kommandorad exempel tooimport från MongoDB export JSON-filer:</span><span class="sxs-lookup"><span data-stu-id="e3e9d-172">Here is a command line sample tooimport from MongoDB export JSON files:</span></span>

    dt.exe /s:MongoDBExport /s.Files:D:\mongoemployees.json /t:CosmosDBBulk /t.ConnectionString:"AccountEndpoint=<CosmosDB Endpoint>;AccountKey=<CosmosDB Key>;Database=<CosmosDB Database>;" /t.Collection:employees /t.IdField:_id /t.Dates:Epoch /t.CollectionThroughput:2500

## <span data-ttu-id="e3e9d-173"><a id="SQL"></a>tooimport från SQL Server</span><span class="sxs-lookup"><span data-stu-id="e3e9d-173"><a id="SQL"></a>tooimport from SQL Server</span></span>
<span data-ttu-id="e3e9d-174">hello SQL källa Importverktyget alternativet kan du tooimport från en enskild SQL Server-databas och du kan också filtrera hello poster toobe importeras med hjälp av en fråga.</span><span class="sxs-lookup"><span data-stu-id="e3e9d-174">hello SQL source importer option allows you tooimport from an individual SQL Server database and optionally filter hello records toobe imported using a query.</span></span> <span data-ttu-id="e3e9d-175">Du kan dessutom ändra hello dokumentstruktur genom att ange kapslade avgränsare (Mer information om det finns en liten stund).</span><span class="sxs-lookup"><span data-stu-id="e3e9d-175">In addition, you can modify hello document structure by specifying a nesting separator (more on that in a moment).</span></span>  

![Skärmbild av SQL - alternativ för databas Migreringsverktyg](./media/import-data/sqlexportsource.png)

<span data-ttu-id="e3e9d-177">hello hello Anslutningssträngens format är hello standard SQL connection-strängformat.</span><span class="sxs-lookup"><span data-stu-id="e3e9d-177">hello format of hello connection string is hello standard SQL connection string format.</span></span>

> [!NOTE]
> <span data-ttu-id="e3e9d-178">Använd hello Kontrollera kommandot tooensure som hello SQL Server-instans som angetts i hello anslutningsfältet sträng kan nås.</span><span class="sxs-lookup"><span data-stu-id="e3e9d-178">Use hello Verify command tooensure that hello SQL Server instance specified in hello connection string field can be accessed.</span></span>
> 
> 

<span data-ttu-id="e3e9d-179">hello kapsla avgränsare egenskapen är används toocreate hierarkiska relationer (underordnade dokument) under importen.</span><span class="sxs-lookup"><span data-stu-id="e3e9d-179">hello nesting separator property is used toocreate hierarchical relationships (sub-documents) during import.</span></span> <span data-ttu-id="e3e9d-180">Tänk hello följande SQL-frågan:</span><span class="sxs-lookup"><span data-stu-id="e3e9d-180">Consider hello following SQL query:</span></span>

<span data-ttu-id="e3e9d-181">*Välj OMVANDLINGEN (BusinessEntityID AS varchar) som Id, namn, adresstyp som [Address.AddressType], AddressLine1 som [Address.AddressLine1], ort som [Address.Location.City], StateProvinceName som [Address.Location.StateProvinceName], postnummer som [ Address.PostalCode] CountryRegionName som [Address.CountryRegionName] från Sales.vStoreWithAddresses där adresstyp = 'Huvudkontoret'*</span><span class="sxs-lookup"><span data-stu-id="e3e9d-181">*select CAST(BusinessEntityID AS varchar) as Id, Name, AddressType as [Address.AddressType], AddressLine1 as [Address.AddressLine1], City as [Address.Location.City], StateProvinceName as [Address.Location.StateProvinceName], PostalCode as [Address.PostalCode], CountryRegionName as [Address.CountryRegionName] from Sales.vStoreWithAddresses WHERE AddressType='Main Office'*</span></span>

<span data-ttu-id="e3e9d-182">Som returnerar hello följande (ofullständiga) resultat:</span><span class="sxs-lookup"><span data-stu-id="e3e9d-182">Which returns hello following (partial) results:</span></span>

![Skärmbild av SQL-frågeresultat](./media/import-data/sqlqueryresults.png)

<span data-ttu-id="e3e9d-184">Observera hello-alias som Address.AddressType och Address.Location.StateProvinceName.</span><span class="sxs-lookup"><span data-stu-id="e3e9d-184">Note hello aliases such as Address.AddressType and Address.Location.StateProvinceName.</span></span> <span data-ttu-id="e3e9d-185">Genom att ange kapslade avgränsare av '.', hello-Importverktyget skapar adress och Address.Location underdokument under hello importera.</span><span class="sxs-lookup"><span data-stu-id="e3e9d-185">By specifying a nesting separator of ‘.’, hello import tool creates Address and Address.Location subdocuments during hello import.</span></span> <span data-ttu-id="e3e9d-186">Här är ett exempel på ett resulterande dokument i Azure Cosmos DB:</span><span class="sxs-lookup"><span data-stu-id="e3e9d-186">Here is an example of a resulting document in Azure Cosmos DB:</span></span>

<span data-ttu-id="e3e9d-187">*{”id”: ”956”, ”Name”: ”ökad försäljning och tjänst”, ”adress”: {”adresstyp”: ”Main Office”, ”AddressLine1”: ”#500 75 O'Connor gata”, ”plats”: {”stad”: ”Ottawa”, ”StateProvinceName”: ”Ontario”}, ”postnummer”: ”K4B 1S2”, ”CountryRegionName” ”: Kanada ”}}*</span><span class="sxs-lookup"><span data-stu-id="e3e9d-187">*{ "id": "956", "Name": "Finer Sales and Service", "Address": { "AddressType": "Main Office", "AddressLine1": "#500-75 O'Connor Street", "Location": { "City": "Ottawa", "StateProvinceName": "Ontario" }, "PostalCode": "K4B 1S2", "CountryRegionName": "Canada" } }*</span></span>

<span data-ttu-id="e3e9d-188">Här följer några exempel tooimport i kommandoraden från SQL Server:</span><span class="sxs-lookup"><span data-stu-id="e3e9d-188">Here are some command line samples tooimport from SQL Server:</span></span>

    #Import records from SQL which match a query
    dt.exe /s:SQL /s.ConnectionString:"Data Source=<server>;Initial Catalog=AdventureWorks;User Id=advworks;Password=<password>;" /s.Query:"select CAST(BusinessEntityID AS varchar) as Id, * from Sales.vStoreWithAddresses WHERE AddressType='Main Office'" /t:CosmosDBBulk /t.ConnectionString:" AccountEndpoint=<CosmosDB Endpoint>;AccountKey=<CosmosDB Key>;Database=<CosmosDB Database>;" /t.Collection:Stores /t.IdField:Id /t.CollectionThroughput:2500

    #Import records from sql which match a query and create hierarchical relationships
    dt.exe /s:SQL /s.ConnectionString:"Data Source=<server>;Initial Catalog=AdventureWorks;User Id=advworks;Password=<password>;" /s.Query:"select CAST(BusinessEntityID AS varchar) as Id, Name, AddressType as [Address.AddressType], AddressLine1 as [Address.AddressLine1], City as [Address.Location.City], StateProvinceName as [Address.Location.StateProvinceName], PostalCode as [Address.PostalCode], CountryRegionName as [Address.CountryRegionName] from Sales.vStoreWithAddresses WHERE AddressType='Main Office'" /s.NestingSeparator:. /t:CosmosDBBulk /t.ConnectionString:" AccountEndpoint=<CosmosDB Endpoint>;AccountKey=<CosmosDB Key>;Database=<CosmosDB Database>;" /t.Collection:StoresSub /t.IdField:Id /t.CollectionThroughput:2500

## <span data-ttu-id="e3e9d-189"><a id="CSV"></a>tooimport CSV-filer och konvertera CSV tooJSON</span><span class="sxs-lookup"><span data-stu-id="e3e9d-189"><a id="CSV"></a>tooimport CSV files and convert CSV tooJSON</span></span>
<span data-ttu-id="e3e9d-190">hello CSV-filen källa Importverktyget alternativet kan du tooimport en eller flera CSV-filer.</span><span class="sxs-lookup"><span data-stu-id="e3e9d-190">hello CSV file source importer option enables you tooimport one or more CSV files.</span></span> <span data-ttu-id="e3e9d-191">När du lägger till mapparna som innehåller CSV-filer för import, har hello möjlighet att rekursivt söker efter filer i undermappar.</span><span class="sxs-lookup"><span data-stu-id="e3e9d-191">When adding folders that contain CSV files for import, you have hello option of recursively searching for files in subfolders.</span></span>

![Skärmbild av CSV - alternativ för CSV-tooJSON](media/import-data/csvsource.png)

<span data-ttu-id="e3e9d-193">Liknande toohello SQL-källans, hello kapsla avgränsare egenskapen kanske används toocreate hierarkiska relationer (underordnade dokument) under importen.</span><span class="sxs-lookup"><span data-stu-id="e3e9d-193">Similar toohello SQL source, hello nesting separator property may be used toocreate hierarchical relationships (sub-documents) during import.</span></span> <span data-ttu-id="e3e9d-194">Överväg följande CSV rubrikrad och datarader hello:</span><span class="sxs-lookup"><span data-stu-id="e3e9d-194">Consider hello following CSV header row and data rows:</span></span>

![Skärmbild av CSV Exempelposter - CSV tooJSON](./media/import-data/csvsample.png)

<span data-ttu-id="e3e9d-196">Observera hello-alias som DomainInfo.Domain_Name och RedirectInfo.Redirecting.</span><span class="sxs-lookup"><span data-stu-id="e3e9d-196">Note hello aliases such as DomainInfo.Domain_Name and RedirectInfo.Redirecting.</span></span> <span data-ttu-id="e3e9d-197">Genom att ange kapslade avgränsare av '.', hello-Importverktyget skapar DomainInfo och RedirectInfo underdokument under hello importera.</span><span class="sxs-lookup"><span data-stu-id="e3e9d-197">By specifying a nesting separator of ‘.’, hello import tool will create DomainInfo and RedirectInfo subdocuments during hello import.</span></span> <span data-ttu-id="e3e9d-198">Här är ett exempel på ett resulterande dokument i Azure Cosmos DB:</span><span class="sxs-lookup"><span data-stu-id="e3e9d-198">Here is an example of a resulting document in Azure Cosmos DB:</span></span>

<span data-ttu-id="e3e9d-199">*{”DomainInfo”: {”Domain_Name”: ”ACUS.GOV”, ”Domain_Name_Address”: ”http://www. ACUS.GOV ”},” Federal myndighet ”:” administrativa konferens med hello USA ”,” RedirectInfo ”: {” omdirigera ”:” 0 ”,” Redirect_Destination ””: ”},” id ”:” 9cc565c5-ebcd-1c03-ebd3-cc3e2ecd814d ”}*</span><span class="sxs-lookup"><span data-stu-id="e3e9d-199">*{ "DomainInfo": { "Domain_Name": "ACUS.GOV", "Domain_Name_Address": "http://www.ACUS.GOV" }, "Federal Agency": "Administrative Conference of hello United States", "RedirectInfo": { "Redirecting": "0", "Redirect_Destination": "" }, "id": "9cc565c5-ebcd-1c03-ebd3-cc3e2ecd814d" }*</span></span>

<span data-ttu-id="e3e9d-200">hello-Importverktyget försöker tooinfer typinformation för ociterade värden i CSV-filer (inom citattecken värden behandlas alltid som strängar).</span><span class="sxs-lookup"><span data-stu-id="e3e9d-200">hello import tool will attempt tooinfer type information for unquoted values in CSV files (quoted values are always treated as strings).</span></span>  <span data-ttu-id="e3e9d-201">Typer som identifieras i hello följande ordning: nummer, datetime, booleskt värde.</span><span class="sxs-lookup"><span data-stu-id="e3e9d-201">Types are identified in hello following order: number, datetime, boolean.</span></span>  

<span data-ttu-id="e3e9d-202">Det finns två andra saker toonote om CSV-import:</span><span class="sxs-lookup"><span data-stu-id="e3e9d-202">There are two other things toonote about CSV import:</span></span>

1. <span data-ttu-id="e3e9d-203">Som standard ociterade värden alltid bort för flikar och blanksteg, medan inom citattecken värden sparas som-är.</span><span class="sxs-lookup"><span data-stu-id="e3e9d-203">By default, unquoted values are always trimmed for tabs and spaces, while quoted values are preserved as-is.</span></span> <span data-ttu-id="e3e9d-204">Det här beteendet kan åsidosättas med hello trimning citattecken värden kryssrutan eller hello /s.TrimQuoted kommandoradsalternativet.</span><span class="sxs-lookup"><span data-stu-id="e3e9d-204">This behavior can be overridden with hello Trim quoted values checkbox or hello /s.TrimQuoted command line option.</span></span>
2. <span data-ttu-id="e3e9d-205">Som standard behandlas en ociterade null som ett null-värde.</span><span class="sxs-lookup"><span data-stu-id="e3e9d-205">By default, an unquoted null is treated as a null value.</span></span> <span data-ttu-id="e3e9d-206">Det här beteendet kan åsidosättas (d.v.s. behandla en ociterade null som en ”null-sträng) med hello Treat onoterade NULL som sträng kryssrutan eller hello /s.NoUnquotedNulls kommandoradsalternativet.</span><span class="sxs-lookup"><span data-stu-id="e3e9d-206">This behavior can be overridden (i.e. treat an unquoted null as a “null” string) with hello Treat unquoted NULL as string checkbox or hello /s.NoUnquotedNulls command line option.</span></span>

<span data-ttu-id="e3e9d-207">Här följer ett exempel på kommandoraden för CSV-import:</span><span class="sxs-lookup"><span data-stu-id="e3e9d-207">Here is a command line sample for CSV import:</span></span>

    dt.exe /s:CsvFile /s.Files:.\Employees.csv /t:CosmosDBBulk /t.ConnectionString:"AccountEndpoint=<CosmosDB Endpoint>;AccountKey=<CosmosDB Key>;Database=<CosmosDB Database>;" /t.Collection:Employees /t.IdField:EntityID /t.CollectionThroughput:2500

## <span data-ttu-id="e3e9d-208"><a id="AzureTableSource"></a>tooimport från Azure-tabellagring</span><span class="sxs-lookup"><span data-stu-id="e3e9d-208"><a id="AzureTableSource"></a>tooimport from Azure Table storage</span></span>
<span data-ttu-id="e3e9d-209">hello Azure Table storage källa Importverktyget alternativet kan du tooimport från en enskild Azure Table storage tabell och du kan också filtrera hello tabell entiteter toobe importeras.</span><span class="sxs-lookup"><span data-stu-id="e3e9d-209">hello Azure Table storage source importer option allows you tooimport from an individual Azure Table storage table and optionally filter hello table entities toobe imported.</span></span> <span data-ttu-id="e3e9d-210">Observera att du inte kan använda hello datamigrering verktyget tooimport Azure Table storage-data i Azure Cosmos DB för användning med hello tabell API.</span><span class="sxs-lookup"><span data-stu-id="e3e9d-210">Note that you cannot use hello Data Migration tool tooimport Azure Table storage data into Azure Cosmos DB for use with hello Table API.</span></span> <span data-ttu-id="e3e9d-211">Endast import tooAzure Cosmos DB för användning med hello DocumentDB API stöds just nu.</span><span class="sxs-lookup"><span data-stu-id="e3e9d-211">Only importing tooAzure Cosmos DB for use with hello DocumentDB API is supported at this time.</span></span>

![Skärmbild av Azure Table storage alternativ](./media/import-data/azuretablesource.png)

<span data-ttu-id="e3e9d-213">hello format hello anslutningssträngen för Azure Table storage är:</span><span class="sxs-lookup"><span data-stu-id="e3e9d-213">hello format of hello Azure Table storage connection string is:</span></span>

    DefaultEndpointsProtocol=<protocol>;AccountName=<Account Name>;AccountKey=<Account Key>;

> [!NOTE]
> <span data-ttu-id="e3e9d-214">Använd hello Kontrollera kommandot tooensure som hello Azure Table storage instans anges i hello anslutningsfältet sträng kan nås.</span><span class="sxs-lookup"><span data-stu-id="e3e9d-214">Use hello Verify command tooensure that hello Azure Table storage instance specified in hello connection string field can be accessed.</span></span>
> 
> 

<span data-ttu-id="e3e9d-215">Ange hello namnet på hello Azure tabell från vilken data ska importeras.</span><span class="sxs-lookup"><span data-stu-id="e3e9d-215">Enter hello name of hello Azure table from which data will be imported.</span></span> <span data-ttu-id="e3e9d-216">Du kan du ange en [filter](https://msdn.microsoft.com/library/azure/ff683669.aspx).</span><span class="sxs-lookup"><span data-stu-id="e3e9d-216">You may optionally specify a [filter](https://msdn.microsoft.com/library/azure/ff683669.aspx).</span></span>

<span data-ttu-id="e3e9d-217">hello Azure källa Importverktyget tabellagring har hello följande ytterligare alternativ:</span><span class="sxs-lookup"><span data-stu-id="e3e9d-217">hello Azure Table storage source importer option has hello following additional options:</span></span>

1. <span data-ttu-id="e3e9d-218">Inkludera interna fält</span><span class="sxs-lookup"><span data-stu-id="e3e9d-218">Include Internal Fields</span></span>
   1. <span data-ttu-id="e3e9d-219">Innehåller alla - alla interna fält (PartitionKey, RowKey och tidsstämpel)</span><span class="sxs-lookup"><span data-stu-id="e3e9d-219">All - Include all internal fields (PartitionKey, RowKey, and Timestamp)</span></span>
   2. <span data-ttu-id="e3e9d-220">Ingen - undanta alla interna fält</span><span class="sxs-lookup"><span data-stu-id="e3e9d-220">None - Exclude all internal fields</span></span>
   3. <span data-ttu-id="e3e9d-221">RowKey - bara innehålla hello RowKey fält</span><span class="sxs-lookup"><span data-stu-id="e3e9d-221">RowKey - Only include hello RowKey field</span></span>
2. <span data-ttu-id="e3e9d-222">Välj kolumner</span><span class="sxs-lookup"><span data-stu-id="e3e9d-222">Select Columns</span></span>
   1. <span data-ttu-id="e3e9d-223">Azure Table storage filter stöder inte projektioner.</span><span class="sxs-lookup"><span data-stu-id="e3e9d-223">Azure Table storage filters do not support projections.</span></span> <span data-ttu-id="e3e9d-224">Om du vill tooonly importera specifika Azure Table Entitetsegenskaper, lägga till dem toohello Välj kolumner lista.</span><span class="sxs-lookup"><span data-stu-id="e3e9d-224">If you want tooonly import specific Azure Table entity properties, add them toohello Select Columns list.</span></span> <span data-ttu-id="e3e9d-225">Alla andra Entitetsegenskaper kommer att ignoreras.</span><span class="sxs-lookup"><span data-stu-id="e3e9d-225">All other entity properties will be ignored.</span></span>

<span data-ttu-id="e3e9d-226">Här är en kommandorad exempel tooimport från Azure Table storage:</span><span class="sxs-lookup"><span data-stu-id="e3e9d-226">Here is a command line sample tooimport from Azure Table storage:</span></span>

    dt.exe /s:AzureTable /s.ConnectionString:"DefaultEndpointsProtocol=https;AccountName=<Account Name>;AccountKey=<Account Key>" /s.Table:metrics /s.InternalFields:All /s.Filter:"PartitionKey eq 'Partition1' and RowKey gt '00001'" /s.Projection:ObjectCount;ObjectSize  /t:CosmosDBBulk /t.ConnectionString:" AccountEndpoint=<CosmosDB Endpoint>;AccountKey=<CosmosDB Key>;Database=<CosmosDB Database>;" /t.Collection:metrics /t.CollectionThroughput:2500

## <span data-ttu-id="e3e9d-227"><a id="DynamoDBSource"></a>tooimport från Amazon DynamoDB</span><span class="sxs-lookup"><span data-stu-id="e3e9d-227"><a id="DynamoDBSource"></a>tooimport from Amazon DynamoDB</span></span>
<span data-ttu-id="e3e9d-228">hello Amazon DynamoDB källa Importverktyget alternativet kan du tooimport från en enskild tabell Amazon DynamoDB och du kan också filtrera hello entiteter toobe importeras.</span><span class="sxs-lookup"><span data-stu-id="e3e9d-228">hello Amazon DynamoDB source importer option allows you tooimport from an individual Amazon DynamoDB table and optionally filter hello entities toobe imported.</span></span> <span data-ttu-id="e3e9d-229">Flera mallar tillhandahålls så att ställa in en import är så enkel som möjligt.</span><span class="sxs-lookup"><span data-stu-id="e3e9d-229">Several templates are provided so that setting up an import is as easy as possible.</span></span>

![Skärmbild av Amazon DynamoDB alternativ - Databasverktyg för migrering](./media/import-data/dynamodbsource1.png)

![Skärmbild av Amazon DynamoDB alternativ - Databasverktyg för migrering](./media/import-data/dynamodbsource2.png)

<span data-ttu-id="e3e9d-232">hello Amazon DynamoDB anslutningssträngen hello format är:</span><span class="sxs-lookup"><span data-stu-id="e3e9d-232">hello format of hello Amazon DynamoDB connection string is:</span></span>

    ServiceURL=<Service Address>;AccessKey=<Access Key>;SecretKey=<Secret Key>;

> [!NOTE]
> <span data-ttu-id="e3e9d-233">Använd hello Kontrollera kommandot tooensure som hello Amazon DynamoDB-instans som angetts i hello anslutningsfältet sträng kan nås.</span><span class="sxs-lookup"><span data-stu-id="e3e9d-233">Use hello Verify command tooensure that hello Amazon DynamoDB instance specified in hello connection string field can be accessed.</span></span>
> 
> 

<span data-ttu-id="e3e9d-234">Här är en kommandorad exempel tooimport från Amazon DynamoDB:</span><span class="sxs-lookup"><span data-stu-id="e3e9d-234">Here is a command line sample tooimport from Amazon DynamoDB:</span></span>

    dt.exe /s:DynamoDB /s.ConnectionString:ServiceURL=https://dynamodb.us-east-1.amazonaws.com;AccessKey=<accessKey>;SecretKey=<secretKey> /s.Request:"{   """TableName""": """ProductCatalog""" }" /t:DocumentDBBulk /t.ConnectionString:"AccountEndpoint=<Azure Cosmos DB Endpoint>;AccountKey=<Azure Cosmos DB Key>;Database=<Azure Cosmos DB Database>;" /t.Collection:catalogCollection /t.CollectionThroughput:2500

## <span data-ttu-id="e3e9d-235"><a id="BlobImport"></a>tooimport filer från Azure Blob storage</span><span class="sxs-lookup"><span data-stu-id="e3e9d-235"><a id="BlobImport"></a>tooimport files from Azure Blob storage</span></span>
<span data-ttu-id="e3e9d-236">hello JSON-fil, MongoDB exportfilen och CSV-filen Importverktyget alternativ kan du tooimport en eller flera filer från Azure Blob storage.</span><span class="sxs-lookup"><span data-stu-id="e3e9d-236">hello JSON file, MongoDB export file, and CSV file source importer options allow you tooimport one or more files from Azure Blob storage.</span></span> <span data-ttu-id="e3e9d-237">När du har angett en URL för Blob-behållaren och Kontonyckel, anger du bara ett reguljärt uttryck tooselect hello fil(er) tooimport.</span><span class="sxs-lookup"><span data-stu-id="e3e9d-237">After specifying a Blob container URL and Account Key, simply provide a regular expression tooselect hello file(s) tooimport.</span></span>

![Skärmbild av Blob alternativ för källa](./media/import-data/blobsource.png)

<span data-ttu-id="e3e9d-239">Här är kommandoraden tooimport JSON exempelfiler från Azure Blob storage:</span><span class="sxs-lookup"><span data-stu-id="e3e9d-239">Here is command line sample tooimport JSON files from Azure Blob storage:</span></span>

    dt.exe /s:JsonFile /s.Files:"blobs://<account key>@account.blob.core.windows.net:443/importcontainer/.*" /t:CosmosDBBulk /t.ConnectionString:"AccountEndpoint=<CosmosDB Endpoint>;AccountKey=<CosmosDB Key>;Database=<CosmosDB Database>;" /t.Collection:doctest

## <span data-ttu-id="e3e9d-240"><a id="DocumentDBSource"></a>tooimport från en samling Azure Cosmos DB DocumentDB API</span><span class="sxs-lookup"><span data-stu-id="e3e9d-240"><a id="DocumentDBSource"></a>tooimport from an Azure Cosmos DB DocumentDB API collection</span></span>
<span data-ttu-id="e3e9d-241">hello Azure Cosmos DB källa Importverktyget alternativet kan du tooimport data från en eller flera Azure Cosmos DB samlingar och du kan också filtrera dokument med hjälp av en fråga.</span><span class="sxs-lookup"><span data-stu-id="e3e9d-241">hello Azure Cosmos DB source importer option allows you tooimport data from one or more Azure Cosmos DB collections and optionally filter documents using a query.</span></span>  

![Skärmbild av Azure Cosmos DB alternativ](./media/import-data/documentdbsource.png)

<span data-ttu-id="e3e9d-243">hello Azure Cosmos DB-anslutningssträngen hello format är:</span><span class="sxs-lookup"><span data-stu-id="e3e9d-243">hello format of hello Azure Cosmos DB connection string is:</span></span>

    AccountEndpoint=<CosmosDB Endpoint>;AccountKey=<CosmosDB Key>;Database=<CosmosDB Database>;

<span data-ttu-id="e3e9d-244">hello Azure Cosmos DB anslutningssträngen för kontot som kan hämtas från hello nycklar bladet för hello Azure-portalen, enligt beskrivningen i [hur toomanage ett konto i Azure Cosmos DB](manage-account.md), men hello namnet på databasen som hello måste toobe läggs toohello anslutningssträngen i hello följande format:</span><span class="sxs-lookup"><span data-stu-id="e3e9d-244">hello Azure Cosmos DB account connection string can be retrieved from hello Keys blade of hello Azure portal, as described in [How toomanage an Azure Cosmos DB account](manage-account.md), however hello name of hello database needs toobe appended toohello connection string in hello following format:</span></span>

    Database=<CosmosDB Database>;

> [!NOTE]
> <span data-ttu-id="e3e9d-245">Använd hello Kontrollera kommandot tooensure som hello Azure DB som Cosmos-instans som anges i hello anslutningsfältet sträng kan nås.</span><span class="sxs-lookup"><span data-stu-id="e3e9d-245">Use hello Verify command tooensure that hello Azure Cosmos DB instance specified in hello connection string field can be accessed.</span></span>
> 
> 

<span data-ttu-id="e3e9d-246">tooimport från en enda Azure DB som Cosmos-samling ange hello namn mängden hello varifrån data ska importeras.</span><span class="sxs-lookup"><span data-stu-id="e3e9d-246">tooimport from a single Azure Cosmos DB collection, enter hello name of hello collection from which data will be imported.</span></span> <span data-ttu-id="e3e9d-247">tooimport från flera Azure Cosmos DB samlingar, ange ett reguljärt uttryck toomatch ett eller flera samlingsnamn (t.ex. collection01 | collection02 | collection03).</span><span class="sxs-lookup"><span data-stu-id="e3e9d-247">tooimport from multiple Azure Cosmos DB collections, provide a regular expression toomatch one or more collection names (e.g. collection01 | collection02 | collection03).</span></span> <span data-ttu-id="e3e9d-248">Du kan alternativt anger, eller ange en fil för en fråga tooboth filter och formar hello data toobe importeras.</span><span class="sxs-lookup"><span data-stu-id="e3e9d-248">You may optionally specify, or provide a file for, a query tooboth filter and shape hello data toobe imported.</span></span>

> [!NOTE]
> <span data-ttu-id="e3e9d-249">Eftersom hello samling fältet accepterar reguljära uttryck om du importerar från en enda samling vars namn innehåller specialtecken i reguljära uttryck, måste dessa tecken hoppas därför.</span><span class="sxs-lookup"><span data-stu-id="e3e9d-249">Since hello collection field accepts regular expressions, if you are importing from a single collection whose name contains regular expression characters, then those characters must be escaped accordingly.</span></span>
> 
> 

<span data-ttu-id="e3e9d-250">hello Azure Cosmos DB källalternativet Importverktyget har hello följande avancerade alternativ:</span><span class="sxs-lookup"><span data-stu-id="e3e9d-250">hello Azure Cosmos DB source importer option has hello following advanced options:</span></span>

1. <span data-ttu-id="e3e9d-251">Ta med intern fält: Anger huruvida tooinclude Azure Cosmos DB dokumentet Systemegenskaper hello exportera (t.ex. _rid, _ts).</span><span class="sxs-lookup"><span data-stu-id="e3e9d-251">Include Internal Fields: Specifies whether or not tooinclude Azure Cosmos DB document system properties in hello export (e.g. _rid, _ts).</span></span>
2. <span data-ttu-id="e3e9d-252">Antalet återförsök vid fel: Anger hello antal gånger tooretry hello anslutning tooAzure Cosmos DB vid tillfälligt fel (t.ex. anslutningen avbrott i nätverket).</span><span class="sxs-lookup"><span data-stu-id="e3e9d-252">Number of Retries on Failure: Specifies hello number of times tooretry hello connection tooAzure Cosmos DB in case of transient failures (e.g. network connectivity interruption).</span></span>
3. <span data-ttu-id="e3e9d-253">Återförsöksintervall: Anger hur länge toowait mellan du försöker hello anslutning tooAzure Cosmos DB vid tillfälligt fel (t.ex. anslutningen avbrott i nätverket).</span><span class="sxs-lookup"><span data-stu-id="e3e9d-253">Retry Interval: Specifies how long toowait between retrying hello connection tooAzure Cosmos DB in case of transient failures (e.g. network connectivity interruption).</span></span>
4. <span data-ttu-id="e3e9d-254">Anslutningsläge: Anger hello anslutning läge toouse med Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="e3e9d-254">Connection Mode: Specifies hello connection mode toouse with Azure Cosmos DB.</span></span> <span data-ttu-id="e3e9d-255">hello tillgängliga alternativen är DirectTcp, DirectHttps och Gateway.</span><span class="sxs-lookup"><span data-stu-id="e3e9d-255">hello available choices are DirectTcp, DirectHttps, and Gateway.</span></span> <span data-ttu-id="e3e9d-256">hello direktanslutning lägen är snabbare och medan hello gateway läge är mer brandvägg eget eftersom den endast använder port 443.</span><span class="sxs-lookup"><span data-stu-id="e3e9d-256">hello direct connection modes are faster, while hello gateway mode is more firewall friendly as it only uses port 443.</span></span>

![Skärmbild av Azure Cosmos DB-datakälla avancerade alternativ](./media/import-data/documentdbsourceoptions.png)

> [!TIP]
> <span data-ttu-id="e3e9d-258">hello importläge verktyget standardvärden tooconnection DirectTcp.</span><span class="sxs-lookup"><span data-stu-id="e3e9d-258">hello import tool defaults tooconnection mode DirectTcp.</span></span> <span data-ttu-id="e3e9d-259">Om det uppstår brandväggsproblem med Växla tooconnection läge Gateway, som kräver endast port 443.</span><span class="sxs-lookup"><span data-stu-id="e3e9d-259">If you experience firewall issues, switch tooconnection mode Gateway, as it only requires port 443.</span></span>
> 
> 

<span data-ttu-id="e3e9d-260">Här följer några kommandoraden exempel tooimport från Azure Cosmos DB:</span><span class="sxs-lookup"><span data-stu-id="e3e9d-260">Here are some command line samples tooimport from Azure Cosmos DB:</span></span>

    #Migrate data from one Azure Cosmos DB collection tooanother Azure Cosmos DB collections
    dt.exe /s:CosmosDB /s.ConnectionString:"AccountEndpoint=<CosmosDB Endpoint>;AccountKey=<CosmosDB Key>;Database=<CosmosDB Database>;" /s.Collection:TEColl /t:CosmosDBBulk /t.ConnectionString:" AccountEndpoint=<CosmosDB Endpoint>;AccountKey=<CosmosDB Key>;Database=<CosmosDB Database>;" /t.Collection:TESessions /t.CollectionThroughput:2500

    #Migrate data from multiple Azure Cosmos DB collections tooa single Azure Cosmos DB collection
    dt.exe /s:CosmosDB /s.ConnectionString:"AccountEndpoint=<CosmosDB Endpoint>;AccountKey=<CosmosDB Key>;Database=<CosmosDB Database>;" /s.Collection:comp1|comp2|comp3|comp4 /t:CosmosDBBulk /t.ConnectionString:"AccountEndpoint=<CosmosDB Endpoint>;AccountKey=<CosmosDB Key>;Database=<CosmosDB Database>;" /t.Collection:singleCollection /t.CollectionThroughput:2500

    #Export an Azure Cosmos DB collection tooa JSON file
    dt.exe /s:CosmosDB /s.ConnectionString:"AccountEndpoint=<CosmosDB Endpoint>;AccountKey=<CosmosDB Key>;Database=<CosmosDB Database>;" /s.Collection:StoresSub /t:JsonFile /t.File:StoresExport.json /t.Overwrite /t.CollectionThroughput:2500

> [!TIP]
> <span data-ttu-id="e3e9d-261">hello Azure Cosmos DB Data-Importverktyget har också stöd för import av data från hello [Azure Cosmos DB emulatorn](local-emulator.md).</span><span class="sxs-lookup"><span data-stu-id="e3e9d-261">hello Azure Cosmos DB Data Import Tool also supports import of data from hello [Azure Cosmos DB Emulator](local-emulator.md).</span></span> <span data-ttu-id="e3e9d-262">När du importerar data från en lokal emulator, ange hello slutpunkt för`https://localhost:<port>`.</span><span class="sxs-lookup"><span data-stu-id="e3e9d-262">When importing data from a local emulator, set hello endpoint too`https://localhost:<port>`.</span></span> 
> 
> 

## <span data-ttu-id="e3e9d-263"><a id="HBaseSource"></a>tooimport från HBase</span><span class="sxs-lookup"><span data-stu-id="e3e9d-263"><a id="HBaseSource"></a>tooimport from HBase</span></span>
<span data-ttu-id="e3e9d-264">Hej HBase källa Importverktyget alternativet kan du tooimport data från en HBase-tabell och du kan också filtrera hello data.</span><span class="sxs-lookup"><span data-stu-id="e3e9d-264">hello HBase source importer option allows you tooimport data from an HBase table and optionally filter hello data.</span></span> <span data-ttu-id="e3e9d-265">Flera mallar tillhandahålls så att ställa in en import är så enkel som möjligt.</span><span class="sxs-lookup"><span data-stu-id="e3e9d-265">Several templates are provided so that setting up an import is as easy as possible.</span></span>

![Skärmbild av HBase alternativ](./media/import-data/hbasesource1.png)

![Skärmbild av HBase alternativ](./media/import-data/hbasesource2.png)

<span data-ttu-id="e3e9d-268">Hej HBase Stargate anslutningssträngen hello format är:</span><span class="sxs-lookup"><span data-stu-id="e3e9d-268">hello format of hello HBase Stargate connection string is:</span></span>

    ServiceURL=<server-address>;Username=<username>;Password=<password>

> [!NOTE]
> <span data-ttu-id="e3e9d-269">Använd hello Kontrollera kommandot tooensure som hello HBase-instans som angetts i hello anslutningsfältet sträng kan nås.</span><span class="sxs-lookup"><span data-stu-id="e3e9d-269">Use hello Verify command tooensure that hello HBase instance specified in hello connection string field can be accessed.</span></span>
> 
> 

<span data-ttu-id="e3e9d-270">Här är en kommandorad exempel tooimport från HBase:</span><span class="sxs-lookup"><span data-stu-id="e3e9d-270">Here is a command line sample tooimport from HBase:</span></span>

    dt.exe /s:HBase /s.ConnectionString:ServiceURL=<server-address>;Username=<username>;Password=<password> /s.Table:Contacts /t:CosmosDBBulk /t.ConnectionString:"AccountEndpoint=<CosmosDB Endpoint>;AccountKey=<CosmosDB Key>;Database=<CosmosDB Database>;" /t.Collection:hbaseimport

## <span data-ttu-id="e3e9d-271"><a id="DocumentDBBulkTarget"></a>tooimport toohello DocumentDB-API (massimport)</span><span class="sxs-lookup"><span data-stu-id="e3e9d-271"><a id="DocumentDBBulkTarget"></a>tooimport toohello DocumentDB API (Bulk Import)</span></span>
<span data-ttu-id="e3e9d-272">hello Azure Cosmos DB Bulk Importverktyget kan tooimport från någon av hello tillgängliga alternativ för datakällor med hjälp av en Azure Cosmos DB lagrade proceduren för effektivitet.</span><span class="sxs-lookup"><span data-stu-id="e3e9d-272">hello Azure Cosmos DB Bulk importer allows you tooimport from any of hello available source options, using an Azure Cosmos DB stored procedure for efficiency.</span></span> <span data-ttu-id="e3e9d-273">hello-verktyget stöder tooone en partitionerad Azure Cosmos DB importsamlingen samt delat import genom vilken data är partitionerad över flera samlingar för en partitionerad Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="e3e9d-273">hello tool supports import tooone single-partitioned Azure Cosmos DB collection, as well as sharded import whereby data is partitioned across multiple single-partitioned Azure Cosmos DB collections.</span></span> <span data-ttu-id="e3e9d-274">Mer information om partitionering data finns [partitionering och skalning i Azure Cosmos DB](partition-data.md).</span><span class="sxs-lookup"><span data-stu-id="e3e9d-274">For more information about partitioning data, see [Partitioning and scaling in Azure Cosmos DB](partition-data.md).</span></span> <span data-ttu-id="e3e9d-275">hello verktyget skapar, köra och ta sedan bort hello lagrade procedur från hello mål samling(ar).</span><span class="sxs-lookup"><span data-stu-id="e3e9d-275">hello tool will create, execute, and then delete hello stored procedure from hello target collection(s).</span></span>  

![Skärmbild av Azure Cosmos DB bulk-alternativ](./media/import-data/documentdbbulk.png)

<span data-ttu-id="e3e9d-277">hello Azure Cosmos DB-anslutningssträngen hello format är:</span><span class="sxs-lookup"><span data-stu-id="e3e9d-277">hello format of hello Azure Cosmos DB connection string is:</span></span>

    AccountEndpoint=<CosmosDB Endpoint>;AccountKey=<CosmosDB Key>;Database=<CosmosDB Database>;

<span data-ttu-id="e3e9d-278">hello Azure Cosmos DB anslutningssträngen för kontot som kan hämtas från hello nycklar bladet för hello Azure-portalen, enligt beskrivningen i [hur toomanage ett konto i Azure Cosmos DB](manage-account.md), men hello namnet på databasen som hello måste toobe läggs toohello anslutningssträngen i hello följande format:</span><span class="sxs-lookup"><span data-stu-id="e3e9d-278">hello Azure Cosmos DB account connection string can be retrieved from hello Keys blade of hello Azure portal, as described in [How toomanage an Azure Cosmos DB account](manage-account.md), however hello name of hello database needs toobe appended toohello connection string in hello following format:</span></span>

    Database=<CosmosDB Database>;

> [!NOTE]
> <span data-ttu-id="e3e9d-279">Använd hello Kontrollera kommandot tooensure som hello Azure DB som Cosmos-instans som anges i hello anslutningsfältet sträng kan nås.</span><span class="sxs-lookup"><span data-stu-id="e3e9d-279">Use hello Verify command tooensure that hello Azure Cosmos DB instance specified in hello connection string field can be accessed.</span></span>
> 
> 

<span data-ttu-id="e3e9d-280">tooimport tooa enkel samling, anger hello namnet på samlingen hello toowhich data importeras och klicka hello Lägg till.</span><span class="sxs-lookup"><span data-stu-id="e3e9d-280">tooimport tooa single collection, enter hello name of hello collection toowhich data will be imported and click hello Add button.</span></span> <span data-ttu-id="e3e9d-281">tooimport toomultiple samlingar, ange varje samlingsnamn individuellt eller hello följande syntax toospecify flera samlingar: *collection_prefix*[startIndex - end index].</span><span class="sxs-lookup"><span data-stu-id="e3e9d-281">tooimport toomultiple collections, either enter each collection name individually or use hello following syntax toospecify multiple collections: *collection_prefix*[start index - end index].</span></span> <span data-ttu-id="e3e9d-282">Tänk hello följande när du anger flera samlingar via hello ovannämnda syntax:</span><span class="sxs-lookup"><span data-stu-id="e3e9d-282">When specifying multiple collections via hello aforementioned syntax, keep hello following in mind:</span></span>

1. <span data-ttu-id="e3e9d-283">Endast heltal intervallet namnet mönster stöds.</span><span class="sxs-lookup"><span data-stu-id="e3e9d-283">Only integer range name patterns are supported.</span></span> <span data-ttu-id="e3e9d-284">Till exempel ange samlingen [0-3] genererar hello följande samlingar: collection0, collection1, collection2 collection3.</span><span class="sxs-lookup"><span data-stu-id="e3e9d-284">For example, specifying collection[0-3] will produce hello following collections: collection0, collection1, collection2, collection3.</span></span>
2. <span data-ttu-id="e3e9d-285">Du kan använda en förkortad syntax: samlingen [3] genererar samma uppsättning samlingar som nämns i steg 1.</span><span class="sxs-lookup"><span data-stu-id="e3e9d-285">You can use an abbreviated syntax: collection[3] will emit same set of collections mentioned in step 1.</span></span>
3. <span data-ttu-id="e3e9d-286">Mer än en ersättning kan anges.</span><span class="sxs-lookup"><span data-stu-id="e3e9d-286">More than one substitution can be provided.</span></span> <span data-ttu-id="e3e9d-287">Till exempel samlingen [0-1] [0-9] genererar 20 samlingsnamn med nollor (collection01... 02... 03).</span><span class="sxs-lookup"><span data-stu-id="e3e9d-287">For example, collection[0-1] [0-9] will generate 20 collection names with leading zeros (collection01, ..02, ..03).</span></span>

<span data-ttu-id="e3e9d-288">När hello samling namn har angetts, välja hello önskade genomflödet i hello samling(ar) (400 RUs too10, 000 RUs).</span><span class="sxs-lookup"><span data-stu-id="e3e9d-288">Once hello collection name(s) have been specified, choose hello desired throughput of hello collection(s) (400 RUs too10,000 RUs).</span></span> <span data-ttu-id="e3e9d-289">Välj en högre genomströmning för bästa prestanda för import.</span><span class="sxs-lookup"><span data-stu-id="e3e9d-289">For best import performance, choose a higher throughput.</span></span> <span data-ttu-id="e3e9d-290">Läs mer om prestandanivåer [prestandanivåer i Azure Cosmos DB](performance-levels.md).</span><span class="sxs-lookup"><span data-stu-id="e3e9d-290">For more information about performance levels, see [Performance levels in Azure Cosmos DB](performance-levels.md).</span></span>

> [!NOTE]
> <span data-ttu-id="e3e9d-291">hello prestanda genomströmning inställningen gäller endast toocollection skapas.</span><span class="sxs-lookup"><span data-stu-id="e3e9d-291">hello performance throughput setting only applies toocollection creation.</span></span> <span data-ttu-id="e3e9d-292">Om hello har angetts finns redan i samlingen, ändras inte dess genomflöde.</span><span class="sxs-lookup"><span data-stu-id="e3e9d-292">If hello specified collection already exists, its throughput will not be modified.</span></span>
> 
> 

<span data-ttu-id="e3e9d-293">När du importerar toomultiple samlingar stöder hello-Importverktyget hash-värde baserat horisontell partitionering.</span><span class="sxs-lookup"><span data-stu-id="e3e9d-293">When importing toomultiple collections, hello import tool supports hash based sharding.</span></span> <span data-ttu-id="e3e9d-294">I det här scenariot, ange hello dokumentegenskap gärna toouse som hello partitionsnyckel (om Partitionsnyckeln är tomt dokument är delat slumpmässigt över hello mål samlingarna).</span><span class="sxs-lookup"><span data-stu-id="e3e9d-294">In this scenario, specify hello document property you wish toouse as hello Partition Key (if Partition Key is left blank, documents will be sharded randomly across hello target collections).</span></span>

<span data-ttu-id="e3e9d-295">Alternativt kan du ange vilket fält i hello importera källa som ska användas som hello Azure Cosmos DB dokumentet id-egenskap under hello import (Observera att om dokument inte innehåller den här egenskapen, sedan hello Importverktyget att generera ett GUID som egenskapsvärde för hello-id).</span><span class="sxs-lookup"><span data-stu-id="e3e9d-295">You may optionally specify which field in hello import source should be used as hello Azure Cosmos DB document id property during hello import (note that if documents do not contain this property, then hello import tool will generate a GUID as hello id property value).</span></span>

<span data-ttu-id="e3e9d-296">Det finns ett antal avancerade alternativ under importen.</span><span class="sxs-lookup"><span data-stu-id="e3e9d-296">There are a number of advanced options available during import.</span></span> <span data-ttu-id="e3e9d-297">Först hello verktyget innehåller en standard massimport lagrad procedur (BulkInsert.js), kan du välja toospecify importera lagrade proceduren:</span><span class="sxs-lookup"><span data-stu-id="e3e9d-297">First, while hello tool includes a default bulk import stored procedure (BulkInsert.js), you may choose toospecify your own import stored procedure:</span></span>

 ![Skärmbild av Azure Cosmos DB bulk insert sproc alternativet](./media/import-data/bulkinsertsp.png)

<span data-ttu-id="e3e9d-299">När du importerar datum typer (t.ex. från SQL Server eller MongoDB) kan välja du dessutom mellan tre importalternativ:</span><span class="sxs-lookup"><span data-stu-id="e3e9d-299">Additionally, when importing date types (e.g. from SQL Server or MongoDB), you can choose between three import options:</span></span>

 ![Skärmbild av Azure Cosmos DB datum tid importalternativ](./media/import-data/datetimeoptions.png)

* <span data-ttu-id="e3e9d-301">Sträng: Spara som ett strängvärde</span><span class="sxs-lookup"><span data-stu-id="e3e9d-301">String: Persist as a string value</span></span>
* <span data-ttu-id="e3e9d-302">Epok: Spara som en epok numeriskt värde</span><span class="sxs-lookup"><span data-stu-id="e3e9d-302">Epoch: Persist as an Epoch number value</span></span>
* <span data-ttu-id="e3e9d-303">Både: Spara både sträng och epok numeriska värden.</span><span class="sxs-lookup"><span data-stu-id="e3e9d-303">Both: Persist both string and Epoch number values.</span></span> <span data-ttu-id="e3e9d-304">Det här alternativet skapas en underdokument, till exempel: ”date_joined”: {”värde” ”: 2013-10-21T21:17:25.2410000Z” ”, epok”: 1382390245}</span><span class="sxs-lookup"><span data-stu-id="e3e9d-304">This option will create a subdocument, for example: "date_joined": { "Value": "2013-10-21T21:17:25.2410000Z", "Epoch": 1382390245 }</span></span>

<span data-ttu-id="e3e9d-305">hello Azure Cosmos DB Bulk Importverktyget har hello följande ytterligare avancerade alternativ:</span><span class="sxs-lookup"><span data-stu-id="e3e9d-305">hello Azure Cosmos DB Bulk importer has hello following additional advanced options:</span></span>

1. <span data-ttu-id="e3e9d-306">Batchstorlek: hello verktyget standardvärden tooa batchstorlek 50.</span><span class="sxs-lookup"><span data-stu-id="e3e9d-306">Batch Size: hello tool defaults tooa batch size of 50.</span></span>  <span data-ttu-id="e3e9d-307">Om hello dokument toobe importeras är stor kan du sänka hello batchstorlek.</span><span class="sxs-lookup"><span data-stu-id="e3e9d-307">If hello documents toobe imported are large, consider lowering hello batch size.</span></span> <span data-ttu-id="e3e9d-308">Om hello dokument toobe importeras är liten kan du däremot du höja hello batchstorlek.</span><span class="sxs-lookup"><span data-stu-id="e3e9d-308">Conversely, if hello documents toobe imported are small, consider raising hello batch size.</span></span>
2. <span data-ttu-id="e3e9d-309">Maxstorlek för skript (byte): hello verktyget standardvärden tooa max skript storleken på 512KB</span><span class="sxs-lookup"><span data-stu-id="e3e9d-309">Max Script Size (bytes): hello tool defaults tooa max script size of 512KB</span></span>
3. <span data-ttu-id="e3e9d-310">Inaktivera automatisk generering: Om varje dokument toobe importeras innehåller ett ID-fält kan det här alternativet kan öka prestanda.</span><span class="sxs-lookup"><span data-stu-id="e3e9d-310">Disable Automatic Id Generation: If every document toobe imported contains an id field, then selecting this option can increase performance.</span></span> <span data-ttu-id="e3e9d-311">Kommer inte att importera dokument som har ett unikt id-fält saknas.</span><span class="sxs-lookup"><span data-stu-id="e3e9d-311">Documents missing a unique id field will not be imported.</span></span>
4. <span data-ttu-id="e3e9d-312">Uppdatera befintliga dokument: hello verktyget standardvärden toonot ersätta befintliga dokument med id-konflikter.</span><span class="sxs-lookup"><span data-stu-id="e3e9d-312">Update Existing Documents: hello tool defaults toonot replacing existing documents with id conflicts.</span></span> <span data-ttu-id="e3e9d-313">Det här alternativet kan skriva över befintliga dokument med matchande ID: n.</span><span class="sxs-lookup"><span data-stu-id="e3e9d-313">Selecting this option will allow overwriting existing documents with matching ids.</span></span> <span data-ttu-id="e3e9d-314">Den här funktionen är användbart för schemalagd datauppsättning migreringar som uppdaterar befintliga dokument.</span><span class="sxs-lookup"><span data-stu-id="e3e9d-314">This feature is useful for scheduled data migrations that update existing documents.</span></span>
5. <span data-ttu-id="e3e9d-315">Antalet återförsök vid fel: Anger hello antal gånger tooretry hello anslutning tooAzure Cosmos DB vid tillfälligt fel (t.ex. anslutningen avbrott i nätverket).</span><span class="sxs-lookup"><span data-stu-id="e3e9d-315">Number of Retries on Failure: Specifies hello number of times tooretry hello connection tooAzure Cosmos DB in case of transient failures (e.g. network connectivity interruption).</span></span>
6. <span data-ttu-id="e3e9d-316">Återförsöksintervall: Anger hur länge toowait mellan du försöker hello anslutning tooAzure Cosmos DB vid tillfälligt fel (t.ex. anslutningen avbrott i nätverket).</span><span class="sxs-lookup"><span data-stu-id="e3e9d-316">Retry Interval: Specifies how long toowait between retrying hello connection tooAzure Cosmos DB in case of transient failures (e.g. network connectivity interruption).</span></span>
7. <span data-ttu-id="e3e9d-317">Anslutningsläge: Anger hello anslutning läge toouse med Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="e3e9d-317">Connection Mode: Specifies hello connection mode toouse with Azure Cosmos DB.</span></span> <span data-ttu-id="e3e9d-318">hello tillgängliga alternativen är DirectTcp, DirectHttps och Gateway.</span><span class="sxs-lookup"><span data-stu-id="e3e9d-318">hello available choices are DirectTcp, DirectHttps, and Gateway.</span></span> <span data-ttu-id="e3e9d-319">hello direktanslutning lägen är snabbare och medan hello gateway läge är mer brandvägg eget eftersom den endast använder port 443.</span><span class="sxs-lookup"><span data-stu-id="e3e9d-319">hello direct connection modes are faster, while hello gateway mode is more firewall friendly as it only uses port 443.</span></span>

![Skärmbild av Azure Cosmos DB massimport avancerade alternativ](./media/import-data/docdbbulkoptions.png)

> [!TIP]
> <span data-ttu-id="e3e9d-321">hello importläge verktyget standardvärden tooconnection DirectTcp.</span><span class="sxs-lookup"><span data-stu-id="e3e9d-321">hello import tool defaults tooconnection mode DirectTcp.</span></span> <span data-ttu-id="e3e9d-322">Om det uppstår brandväggsproblem med Växla tooconnection läge Gateway, som kräver endast port 443.</span><span class="sxs-lookup"><span data-stu-id="e3e9d-322">If you experience firewall issues, switch tooconnection mode Gateway, as it only requires port 443.</span></span>
> 
> 

## <span data-ttu-id="e3e9d-323"><a id="DocumentDBSeqTarget"></a>tooimport toohello DocumentDB-API (sekventiella post Import)</span><span class="sxs-lookup"><span data-stu-id="e3e9d-323"><a id="DocumentDBSeqTarget"></a>tooimport toohello DocumentDB API (Sequential Record Import)</span></span>
<span data-ttu-id="e3e9d-324">hello Azure Cosmos DB sekventiella post Importverktyget kan tooimport från någon av hello tillgängliga alternativ på en post med basis.</span><span class="sxs-lookup"><span data-stu-id="e3e9d-324">hello Azure Cosmos DB sequential record importer allows you tooimport from any of hello available source options on a record by record basis.</span></span> <span data-ttu-id="e3e9d-325">Du kan välja det här alternativet om du importerar tooan befintlig samling som har uppnått sin kvot av lagrade procedurer.</span><span class="sxs-lookup"><span data-stu-id="e3e9d-325">You might choose this option if you’re importing tooan existing collection that has reached its quota of stored procedures.</span></span> <span data-ttu-id="e3e9d-326">hello verktyget stöder import tooa enkel (enskild partition och flera partition) Azure DB som Cosmos-samling som delat import genom vilken data är partitionerad över flera enskild partition och/eller flera partition Azure DB som Cosmos-samlingar.</span><span class="sxs-lookup"><span data-stu-id="e3e9d-326">hello tool supports import tooa single (both single-partition and multi-partition) Azure Cosmos DB collection, as well as sharded import whereby data is partitioned across multiple single-partition and/or multi-partition Azure Cosmos DB collections.</span></span> <span data-ttu-id="e3e9d-327">Mer information om partitionering data finns [partitionering och skalning i Azure Cosmos DB](partition-data.md).</span><span class="sxs-lookup"><span data-stu-id="e3e9d-327">For more information about partitioning data, see [Partitioning and scaling in Azure Cosmos DB](partition-data.md).</span></span>

![Skärmbild av Azure Cosmos DB importalternativ för sekventiella poster](./media/import-data/documentdbsequential.png)

<span data-ttu-id="e3e9d-329">hello Azure Cosmos DB-anslutningssträngen hello format är:</span><span class="sxs-lookup"><span data-stu-id="e3e9d-329">hello format of hello Azure Cosmos DB connection string is:</span></span>

    AccountEndpoint=<CosmosDB Endpoint>;AccountKey=<CosmosDB Key>;Database=<CosmosDB Database>;

<span data-ttu-id="e3e9d-330">hello Azure Cosmos DB anslutningssträngen för kontot som kan hämtas från hello nycklar bladet för hello Azure-portalen, enligt beskrivningen i [hur toomanage ett konto i Azure Cosmos DB](manage-account.md), men hello namnet på databasen som hello måste toobe läggs toohello anslutningssträngen i hello följande format:</span><span class="sxs-lookup"><span data-stu-id="e3e9d-330">hello Azure Cosmos DB account connection string can be retrieved from hello Keys blade of hello Azure portal, as described in [How toomanage an Azure Cosmos DB account](manage-account.md), however hello name of hello database needs toobe appended toohello connection string in hello following format:</span></span>

    Database=<Azure Cosmos DB Database>;

> [!NOTE]
> <span data-ttu-id="e3e9d-331">Använd hello Kontrollera kommandot tooensure som hello Azure DB som Cosmos-instans som anges i hello anslutningsfältet sträng kan nås.</span><span class="sxs-lookup"><span data-stu-id="e3e9d-331">Use hello Verify command tooensure that hello Azure Cosmos DB instance specified in hello connection string field can be accessed.</span></span>
> 
> 

<span data-ttu-id="e3e9d-332">tooimport tooa enkel samling, anger hello namnet på samlingen hello toowhich data importeras och klicka hello Lägg till.</span><span class="sxs-lookup"><span data-stu-id="e3e9d-332">tooimport tooa single collection, enter hello name of hello collection toowhich data will be imported and click hello Add button.</span></span> <span data-ttu-id="e3e9d-333">tooimport toomultiple samlingar, ange varje samlingsnamn individuellt eller hello följande syntax toospecify flera samlingar: *collection_prefix*[startIndex - end index].</span><span class="sxs-lookup"><span data-stu-id="e3e9d-333">tooimport toomultiple collections, either enter each collection name individually or use hello following syntax toospecify multiple collections: *collection_prefix*[start index - end index].</span></span> <span data-ttu-id="e3e9d-334">Tänk hello följande när du anger flera samlingar via hello ovannämnda syntax:</span><span class="sxs-lookup"><span data-stu-id="e3e9d-334">When specifying multiple collections via hello aforementioned syntax, keep hello following in mind:</span></span>

1. <span data-ttu-id="e3e9d-335">Endast heltal intervallet namnet mönster stöds.</span><span class="sxs-lookup"><span data-stu-id="e3e9d-335">Only integer range name patterns are supported.</span></span> <span data-ttu-id="e3e9d-336">Till exempel ange samlingen [0-3] genererar hello följande samlingar: collection0, collection1, collection2 collection3.</span><span class="sxs-lookup"><span data-stu-id="e3e9d-336">For example, specifying collection[0-3] will produce hello following collections: collection0, collection1, collection2, collection3.</span></span>
2. <span data-ttu-id="e3e9d-337">Du kan använda en förkortad syntax: samlingen [3] genererar samma uppsättning samlingar som nämns i steg 1.</span><span class="sxs-lookup"><span data-stu-id="e3e9d-337">You can use an abbreviated syntax: collection[3] will emit same set of collections mentioned in step 1.</span></span>
3. <span data-ttu-id="e3e9d-338">Mer än en ersättning kan anges.</span><span class="sxs-lookup"><span data-stu-id="e3e9d-338">More than one substitution can be provided.</span></span> <span data-ttu-id="e3e9d-339">Till exempel samlingen [0-1] [0-9] genererar 20 samlingsnamn med nollor (collection01... 02... 03).</span><span class="sxs-lookup"><span data-stu-id="e3e9d-339">For example, collection[0-1] [0-9] will generate 20 collection names with leading zeros (collection01, ..02, ..03).</span></span>

<span data-ttu-id="e3e9d-340">När hello samling namn har angetts, välja hello önskade genomflödet i hello samling(ar) (400 RUs too250, 000 RUs).</span><span class="sxs-lookup"><span data-stu-id="e3e9d-340">Once hello collection name(s) have been specified, choose hello desired throughput of hello collection(s) (400 RUs too250,000 RUs).</span></span> <span data-ttu-id="e3e9d-341">Välj en högre genomströmning för bästa prestanda för import.</span><span class="sxs-lookup"><span data-stu-id="e3e9d-341">For best import performance, choose a higher throughput.</span></span> <span data-ttu-id="e3e9d-342">Läs mer om prestandanivåer [prestandanivåer i Azure Cosmos DB](performance-levels.md).</span><span class="sxs-lookup"><span data-stu-id="e3e9d-342">For more information about performance levels, see [Performance levels in Azure Cosmos DB](performance-levels.md).</span></span> <span data-ttu-id="e3e9d-343">Någon importera toocollections med genomströmning > 10 000 RUs kräver en partitionsnyckel.</span><span class="sxs-lookup"><span data-stu-id="e3e9d-343">Any import toocollections with throughput >10,000 RUs will require a partition key.</span></span> <span data-ttu-id="e3e9d-344">Om du väljer toohave 250 000 RUs behöver toofile en begäran i hello portal toohave ökat för ditt konto.</span><span class="sxs-lookup"><span data-stu-id="e3e9d-344">If you choose toohave more than 250,000 RUs, you will need toofile a request in hello portal toohave your account increased.</span></span>

> [!NOTE]
> <span data-ttu-id="e3e9d-345">hello genomströmning inställningen gäller endast toocollection skapas.</span><span class="sxs-lookup"><span data-stu-id="e3e9d-345">hello throughput setting only applies toocollection creation.</span></span> <span data-ttu-id="e3e9d-346">Om hello har angetts finns redan i samlingen, ändras inte dess genomflöde.</span><span class="sxs-lookup"><span data-stu-id="e3e9d-346">If hello specified collection already exists, its throughput will not be modified.</span></span>
> 
> 

<span data-ttu-id="e3e9d-347">När du importerar toomultiple samlingar stöder hello-Importverktyget hash-värde baserat horisontell partitionering.</span><span class="sxs-lookup"><span data-stu-id="e3e9d-347">When importing toomultiple collections, hello import tool supports hash based sharding.</span></span> <span data-ttu-id="e3e9d-348">I det här scenariot, ange hello dokumentegenskap gärna toouse som hello partitionsnyckel (om Partitionsnyckeln är tomt dokument är delat slumpmässigt över hello mål samlingarna).</span><span class="sxs-lookup"><span data-stu-id="e3e9d-348">In this scenario, specify hello document property you wish toouse as hello Partition Key (if Partition Key is left blank, documents will be sharded randomly across hello target collections).</span></span>

<span data-ttu-id="e3e9d-349">Alternativt kan du ange vilket fält i hello importera källa som ska användas som hello Azure Cosmos DB dokumentet id-egenskap under hello import (Observera att om dokument inte innehåller den här egenskapen, sedan hello Importverktyget att generera ett GUID som egenskapsvärde för hello-id).</span><span class="sxs-lookup"><span data-stu-id="e3e9d-349">You may optionally specify which field in hello import source should be used as hello Azure Cosmos DB document id property during hello import (note that if documents do not contain this property, then hello import tool will generate a GUID as hello id property value).</span></span>

<span data-ttu-id="e3e9d-350">Det finns ett antal avancerade alternativ under importen.</span><span class="sxs-lookup"><span data-stu-id="e3e9d-350">There are a number of advanced options available during import.</span></span> <span data-ttu-id="e3e9d-351">När du importerar datum typer (t.ex. från SQL Server eller MongoDB) måste välja du först mellan tre importalternativ:</span><span class="sxs-lookup"><span data-stu-id="e3e9d-351">First, when importing date types (e.g. from SQL Server or MongoDB), you can choose between three import options:</span></span>

 ![Skärmbild av Azure Cosmos DB datum tid importalternativ](./media/import-data/datetimeoptions.png)

* <span data-ttu-id="e3e9d-353">Sträng: Spara som ett strängvärde</span><span class="sxs-lookup"><span data-stu-id="e3e9d-353">String: Persist as a string value</span></span>
* <span data-ttu-id="e3e9d-354">Epok: Spara som en epok numeriskt värde</span><span class="sxs-lookup"><span data-stu-id="e3e9d-354">Epoch: Persist as an Epoch number value</span></span>
* <span data-ttu-id="e3e9d-355">Både: Spara både sträng och epok numeriska värden.</span><span class="sxs-lookup"><span data-stu-id="e3e9d-355">Both: Persist both string and Epoch number values.</span></span> <span data-ttu-id="e3e9d-356">Det här alternativet skapas en underdokument, till exempel: ”date_joined”: {”värde” ”: 2013-10-21T21:17:25.2410000Z” ”, epok”: 1382390245}</span><span class="sxs-lookup"><span data-stu-id="e3e9d-356">This option will create a subdocument, for example: "date_joined": { "Value": "2013-10-21T21:17:25.2410000Z", "Epoch": 1382390245 }</span></span>

<span data-ttu-id="e3e9d-357">hello Azure Cosmos DB - sekventiella post Importverktyget har hello följande ytterligare avancerade alternativ:</span><span class="sxs-lookup"><span data-stu-id="e3e9d-357">hello Azure Cosmos DB - Sequential record importer has hello following additional advanced options:</span></span>

1. <span data-ttu-id="e3e9d-358">Antalet parallella begäranden: hello verktyget too2 parallella begäranden som standard.</span><span class="sxs-lookup"><span data-stu-id="e3e9d-358">Number of Parallel Requests: hello tool defaults too2 parallel requests.</span></span> <span data-ttu-id="e3e9d-359">Överväg att höja hello antalet parallella begäranden om hello dokument toobe importeras är små.</span><span class="sxs-lookup"><span data-stu-id="e3e9d-359">If hello documents toobe imported are small, consider raising hello number of parallel requests.</span></span> <span data-ttu-id="e3e9d-360">Observera att om det här antalet aktiveras för mycket hello importera uppstå begränsning.</span><span class="sxs-lookup"><span data-stu-id="e3e9d-360">Note that if this number is raised too much, hello import may experience throttling.</span></span>
2. <span data-ttu-id="e3e9d-361">Inaktivera automatisk generering: Om varje dokument toobe importeras innehåller ett ID-fält kan det här alternativet kan öka prestanda.</span><span class="sxs-lookup"><span data-stu-id="e3e9d-361">Disable Automatic Id Generation: If every document toobe imported contains an id field, then selecting this option can increase performance.</span></span> <span data-ttu-id="e3e9d-362">Kommer inte att importera dokument som har ett unikt id-fält saknas.</span><span class="sxs-lookup"><span data-stu-id="e3e9d-362">Documents missing a unique id field will not be imported.</span></span>
3. <span data-ttu-id="e3e9d-363">Uppdatera befintliga dokument: hello verktyget standardvärden toonot ersätta befintliga dokument med id-konflikter.</span><span class="sxs-lookup"><span data-stu-id="e3e9d-363">Update Existing Documents: hello tool defaults toonot replacing existing documents with id conflicts.</span></span> <span data-ttu-id="e3e9d-364">Det här alternativet kan skriva över befintliga dokument med matchande ID: n.</span><span class="sxs-lookup"><span data-stu-id="e3e9d-364">Selecting this option will allow overwriting existing documents with matching ids.</span></span> <span data-ttu-id="e3e9d-365">Den här funktionen är användbart för schemalagd datauppsättning migreringar som uppdaterar befintliga dokument.</span><span class="sxs-lookup"><span data-stu-id="e3e9d-365">This feature is useful for scheduled data migrations that update existing documents.</span></span>
4. <span data-ttu-id="e3e9d-366">Antalet återförsök vid fel: Anger hello antal gånger tooretry hello anslutning tooAzure Cosmos DB vid tillfälligt fel (t.ex. anslutningen avbrott i nätverket).</span><span class="sxs-lookup"><span data-stu-id="e3e9d-366">Number of Retries on Failure: Specifies hello number of times tooretry hello connection tooAzure Cosmos DB in case of transient failures (e.g. network connectivity interruption).</span></span>
5. <span data-ttu-id="e3e9d-367">Återförsöksintervall: Anger hur länge toowait mellan du försöker hello anslutning tooAzure Cosmos DB vid tillfälligt fel (t.ex. anslutningen avbrott i nätverket).</span><span class="sxs-lookup"><span data-stu-id="e3e9d-367">Retry Interval: Specifies how long toowait between retrying hello connection tooAzure Cosmos DB in case of transient failures (e.g. network connectivity interruption).</span></span>
6. <span data-ttu-id="e3e9d-368">Anslutningsläge: Anger hello anslutning läge toouse med Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="e3e9d-368">Connection Mode: Specifies hello connection mode toouse with Azure Cosmos DB.</span></span> <span data-ttu-id="e3e9d-369">hello tillgängliga alternativen är DirectTcp, DirectHttps och Gateway.</span><span class="sxs-lookup"><span data-stu-id="e3e9d-369">hello available choices are DirectTcp, DirectHttps, and Gateway.</span></span> <span data-ttu-id="e3e9d-370">hello direktanslutning lägen är snabbare och medan hello gateway läge är mer brandvägg eget eftersom den endast använder port 443.</span><span class="sxs-lookup"><span data-stu-id="e3e9d-370">hello direct connection modes are faster, while hello gateway mode is more firewall friendly as it only uses port 443.</span></span>

![Skärmbild av Azure Cosmos DB sekventiella post importera avancerade alternativ](./media/import-data/documentdbsequentialoptions.png)

> [!TIP]
> <span data-ttu-id="e3e9d-372">hello importläge verktyget standardvärden tooconnection DirectTcp.</span><span class="sxs-lookup"><span data-stu-id="e3e9d-372">hello import tool defaults tooconnection mode DirectTcp.</span></span> <span data-ttu-id="e3e9d-373">Om det uppstår brandväggsproblem med Växla tooconnection läge Gateway, som kräver endast port 443.</span><span class="sxs-lookup"><span data-stu-id="e3e9d-373">If you experience firewall issues, switch tooconnection mode Gateway, as it only requires port 443.</span></span>
> 
> 

## <span data-ttu-id="e3e9d-374"><a id="IndexingPolicy"></a>Ange ett indexprincip när du skapar Azure Cosmos DB samlingar</span><span class="sxs-lookup"><span data-stu-id="e3e9d-374"><a id="IndexingPolicy"></a>Specify an indexing policy when creating Azure Cosmos DB collections</span></span>
<span data-ttu-id="e3e9d-375">Du kan ange hello indexprincip av hello samlingar när du tillåter hello migrering verktyget toocreate samlingar under importen.</span><span class="sxs-lookup"><span data-stu-id="e3e9d-375">When you allow hello migration tool toocreate collections during import, you can specify hello indexing policy of hello collections.</span></span> <span data-ttu-id="e3e9d-376">Navigera toohello indexering principavdelningen i hello avancerade alternativ avsnittet hello Azure Cosmos DB massimport och Azure Cosmos DB sekventiella post alternativ.</span><span class="sxs-lookup"><span data-stu-id="e3e9d-376">In hello advanced options section of hello Azure Cosmos DB Bulk import and Azure Cosmos DB Sequential record options, navigate toohello Indexing Policy section.</span></span>

![Skärmbild av Azure Cosmos DB indexering princip avancerade alternativ](./media/import-data/indexingpolicy1.png)

<span data-ttu-id="e3e9d-378">Använder hello indexering avancerade alternativ, kan du välja en indexering principfil, manuellt ange ett indexprincip, eller välj från en uppsättning standardmallar (genom att högerklicka i hello indexering princip textruta).</span><span class="sxs-lookup"><span data-stu-id="e3e9d-378">Using hello Indexing Policy advanced option, you can select an indexing policy file, manually enter an indexing policy, or select from a set of default templates (by right clicking in hello indexing policy textbox).</span></span>

<span data-ttu-id="e3e9d-379">hello principmallar hello verktyget tillhandahåller är:</span><span class="sxs-lookup"><span data-stu-id="e3e9d-379">hello policy templates hello tool provides are:</span></span>

* <span data-ttu-id="e3e9d-380">Som standard.</span><span class="sxs-lookup"><span data-stu-id="e3e9d-380">Default.</span></span> <span data-ttu-id="e3e9d-381">Den här principen är bäst när du utför likhetsfrågor för strängar och använda ORDER BY, intervalls och likhetsfrågor för tal.</span><span class="sxs-lookup"><span data-stu-id="e3e9d-381">This policy is best when you’re performing equality queries against strings and using ORDER BY, range, and equality queries for numbers.</span></span> <span data-ttu-id="e3e9d-382">Den här principen har en lägre Omkostnad för indexlagring än intervall.</span><span class="sxs-lookup"><span data-stu-id="e3e9d-382">This policy has a lower index storage overhead than Range.</span></span>
* <span data-ttu-id="e3e9d-383">Intervall.</span><span class="sxs-lookup"><span data-stu-id="e3e9d-383">Range.</span></span> <span data-ttu-id="e3e9d-384">Den här principen är bäst om du använder ORDER BY, intervalls och likhetsfrågor för både tal och strängar.</span><span class="sxs-lookup"><span data-stu-id="e3e9d-384">This policy is best you’re using ORDER BY, range and equality queries on both numbers and strings.</span></span> <span data-ttu-id="e3e9d-385">Den här principen har en högre Omkostnad för indexlagring än standard eller Hash.</span><span class="sxs-lookup"><span data-stu-id="e3e9d-385">This policy has a higher index storage overhead than Default or Hash.</span></span>

![Skärmbild av Azure Cosmos DB indexering princip avancerade alternativ](./media/import-data/indexingpolicy2.png)

> [!NOTE]
> <span data-ttu-id="e3e9d-387">Om du inte anger en indexprincip tillämpas hello standardprincipen.</span><span class="sxs-lookup"><span data-stu-id="e3e9d-387">If you do not specify an indexing policy, then hello default policy will be applied.</span></span> <span data-ttu-id="e3e9d-388">Mer information om principer för fulltextindexering finns [Azure Cosmos DB indexering principer](indexing-policies.md).</span><span class="sxs-lookup"><span data-stu-id="e3e9d-388">For more information about indexing policies, see [Azure Cosmos DB indexing policies](indexing-policies.md).</span></span>
> 
> 

## <a name="export-toojson-file"></a><span data-ttu-id="e3e9d-389">Exportera tooJSON fil</span><span class="sxs-lookup"><span data-stu-id="e3e9d-389">Export tooJSON file</span></span>
<span data-ttu-id="e3e9d-390">hello Azure Cosmos DB JSON Exportverktyget kan du tooexport någon hello tillgänglig källa alternativ tooa JSON-fil som innehåller en matris av JSON-dokument.</span><span class="sxs-lookup"><span data-stu-id="e3e9d-390">hello Azure Cosmos DB JSON exporter allows you tooexport any of hello available source options tooa JSON file that contains an array of JSON documents.</span></span> <span data-ttu-id="e3e9d-391">hello verktyget hanterar hello export du eller väljer tooview hello resulterande migrering kommandot och kör hello kommando själv.</span><span class="sxs-lookup"><span data-stu-id="e3e9d-391">hello tool will handle hello export for you, or you can choose tooview hello resulting migration command and run hello command yourself.</span></span> <span data-ttu-id="e3e9d-392">hello resulterande JSON-fil kan sparas lokalt eller i Azure Blob storage.</span><span class="sxs-lookup"><span data-stu-id="e3e9d-392">hello resulting JSON file may be stored locally or in Azure Blob storage.</span></span>

![Skärmbild av Azure Cosmos DB JSON lokal fil exportalternativ](./media/import-data/jsontarget.png)

![Skärmbild av Azure Cosmos DB JSON Azure Blob storage-exportalternativ](./media/import-data/jsontarget2.png)

<span data-ttu-id="e3e9d-395">Du kan välja tooprettify hello resulterande JSON som ökar hello storlek på hello resulterande dokument medan gör hello innehåll mer läsbar.</span><span class="sxs-lookup"><span data-stu-id="e3e9d-395">You may optionally choose tooprettify hello resulting JSON, which will increase hello size of hello resulting document while making hello contents more human readable.</span></span>

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

## <a name="advanced-configuration"></a><span data-ttu-id="e3e9d-396">Avancerad konfiguration</span><span class="sxs-lookup"><span data-stu-id="e3e9d-396">Advanced configuration</span></span>
<span data-ttu-id="e3e9d-397">Ange hello platsen för hello log file toowhich du vill att eventuella fel som skrivs i hello avancerad konfigurationsskärmen.</span><span class="sxs-lookup"><span data-stu-id="e3e9d-397">In hello Advanced configuration screen, specify hello location of hello log file toowhich you would like any errors written.</span></span> <span data-ttu-id="e3e9d-398">hello följande regler gäller toothis sidan:</span><span class="sxs-lookup"><span data-stu-id="e3e9d-398">hello following rules apply toothis page:</span></span>

1. <span data-ttu-id="e3e9d-399">Om ett filnamn inte anges returneras alla fel på hello resultatsidan.</span><span class="sxs-lookup"><span data-stu-id="e3e9d-399">If a file name is not provided, then all errors will be returned on hello Results page.</span></span>
2. <span data-ttu-id="e3e9d-400">Om ett filnamn anges utan en katalog, kommer sedan hello filen att skapas (eller skrivs över) i miljön hello katalogen.</span><span class="sxs-lookup"><span data-stu-id="e3e9d-400">If a file name is provided without a directory, then hello file will be created (or overwritten) in hello current environment directory.</span></span>
3. <span data-ttu-id="e3e9d-401">Om du väljer en befintlig fil och sedan hello filen kommer att skrivas över finns det inget alternativ för Lägg till.</span><span class="sxs-lookup"><span data-stu-id="e3e9d-401">If you select an existing file, then hello file will be overwritten, there is no append option.</span></span>

<span data-ttu-id="e3e9d-402">Välj om alla toolog, kritisk, eller inga felmeddelanden.</span><span class="sxs-lookup"><span data-stu-id="e3e9d-402">Then, choose whether toolog all, critical, or no error messages.</span></span> <span data-ttu-id="e3e9d-403">Slutligen besluta hur ofta hello på skärmen överföring meddelandet kommer att uppdateras med dess förlopp.</span><span class="sxs-lookup"><span data-stu-id="e3e9d-403">Finally, decide how frequently hello on screen transfer message will be updated with its progress.</span></span>

    ![Screenshot of Advanced configuration screen](./media/import-data/AdvancedConfiguration.png)

## <a name="confirm-import-settings-and-view-command-line"></a><span data-ttu-id="e3e9d-404">Bekräfta importera inställningar och visa kommandoraden</span><span class="sxs-lookup"><span data-stu-id="e3e9d-404">Confirm import settings and view command line</span></span>
1. <span data-ttu-id="e3e9d-405">När du har angett information om källdatorn och målet information avancerad konfiguration, granska hello migrering sammanfattning och du kan också visa/kopiera hello resulterande migrering kommando (kopiera hello-kommandot är användbara tooautomate importåtgärder):</span><span class="sxs-lookup"><span data-stu-id="e3e9d-405">After specifying source information, target information, and advanced configuration, review hello migration summary and, optionally, view/copy hello resulting migration command (copying hello command is useful tooautomate import operations):</span></span>
   
    ![Skärmbild av översiktsskärm](./media/import-data/summary.png)
   
    ![Skärmbild av översiktsskärm](./media/import-data/summarycommand.png)
2. <span data-ttu-id="e3e9d-408">När du är nöjd med din käll- och alternativ klickar du på **importera**.</span><span class="sxs-lookup"><span data-stu-id="e3e9d-408">Once you’re satisfied with your source and target options, click **Import**.</span></span> <span data-ttu-id="e3e9d-409">hello förfluten tid, överförda antal och felinformation (om du inte anger ett filnamn i hello avancerad konfiguration) uppdateras när hello importen pågår.</span><span class="sxs-lookup"><span data-stu-id="e3e9d-409">hello elapsed time, transferred count, and failure information (if you didn't provide a file name in hello Advanced configuration) will update as hello import is in process.</span></span> <span data-ttu-id="e3e9d-410">När installationen är klar kan du exportera hello resultat (t.ex. toodeal med importfel).</span><span class="sxs-lookup"><span data-stu-id="e3e9d-410">Once complete, you can export hello results (e.g. toodeal with any import failures).</span></span>
   
    ![Skärmbild av Azure Cosmos DB JSON exportalternativ](./media/import-data/viewresults.png)
3. <span data-ttu-id="e3e9d-412">Du kan också starta en ny import antingen behålla hello befintliga inställningar (t.ex. anslutning sträng information, källa och mål för val osv.) eller återställa alla värden.</span><span class="sxs-lookup"><span data-stu-id="e3e9d-412">You may also start a new import, either keeping hello existing settings (e.g. connection string information, source and target choice, etc.) or resetting all values.</span></span>
   
    ![Skärmbild av Azure Cosmos DB JSON exportalternativ](./media/import-data/newimport.png)

## <a name="next-steps"></a><span data-ttu-id="e3e9d-414">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="e3e9d-414">Next steps</span></span>

<span data-ttu-id="e3e9d-415">I den här kursen har du gjort hello följande:</span><span class="sxs-lookup"><span data-stu-id="e3e9d-415">In this tutorial, you've done hello following:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="e3e9d-416">Installerat hello datamigreringsverktyget</span><span class="sxs-lookup"><span data-stu-id="e3e9d-416">Installed hello Data Migration tool</span></span>
> * <span data-ttu-id="e3e9d-417">Importerade data från olika datakällor</span><span class="sxs-lookup"><span data-stu-id="e3e9d-417">Imported data from different data sources</span></span>
> * <span data-ttu-id="e3e9d-418">Exporterats från Azure Cosmos DB tooJSON</span><span class="sxs-lookup"><span data-stu-id="e3e9d-418">Exported from Azure Cosmos DB tooJSON</span></span>

<span data-ttu-id="e3e9d-419">Du kan nu fortsätta toohello nästa kurs och lära dig hur tooquery data med hjälp av Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="e3e9d-419">You can now proceed toohello next tutorial and learn how tooquery data using Azure Cosmos DB.</span></span> 

> [!div class="nextstepaction"]
>[<span data-ttu-id="e3e9d-420">Hur tooquery data?</span><span class="sxs-lookup"><span data-stu-id="e3e9d-420">How tooquery data?</span></span>](../cosmos-db/tutorial-query-documentdb.md)
