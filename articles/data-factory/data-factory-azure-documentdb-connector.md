---
title: "Flytta data till/från Azure Cosmos DB | Microsoft Docs"
description: "Lär dig hur du ska flytta data till/från Azure Cosmos DB samlingen med hjälp av Azure Data Factory"
services: data-factory, cosmosdb
documentationcenter: 
author: linda33wj
manager: jhubbard
editor: monicar
ms.assetid: c9297b71-1bb4-4b29-ba3c-4cf1f5575fac
ms.service: multiple
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/20/2017
ms.author: jingwang
ms.openlocfilehash: 7a11c6ade0325b08ad520448bbf82d64a0a555f3
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="move-data-to-and-from-azure-cosmos-db-using-azure-data-factory"></a><span data-ttu-id="12fd7-103">Flytta data till och från Azure Cosmos-databasen med Azure Data Factory</span><span class="sxs-lookup"><span data-stu-id="12fd7-103">Move data to and from Azure Cosmos DB using Azure Data Factory</span></span>
<span data-ttu-id="12fd7-104">Den här artikeln förklarar hur du använder aktiviteten kopiera i Azure Data Factory för att flytta data till och från Azure Cosmos DB (DocumentDB-API).</span><span class="sxs-lookup"><span data-stu-id="12fd7-104">This article explains how to use the Copy Activity in Azure Data Factory to move data to/from Azure Cosmos DB (DocumentDB API).</span></span> <span data-ttu-id="12fd7-105">Den bygger på den [Data Movement aktiviteter](data-factory-data-movement-activities.md) artikel som presenterar en allmän översikt över dataflyttning med copy-aktivitet.</span><span class="sxs-lookup"><span data-stu-id="12fd7-105">It builds on the [Data Movement Activities](data-factory-data-movement-activities.md) article, which presents a general overview of data movement with the copy activity.</span></span> 

<span data-ttu-id="12fd7-106">Du kan kopiera data från alla stöds källa datalagret till Azure Cosmos DB eller Azure Cosmos DB till alla stöds sink-datalagret.</span><span class="sxs-lookup"><span data-stu-id="12fd7-106">You can copy data from any supported source data store to Azure Cosmos DB or from Azure Cosmos DB to any supported sink data store.</span></span> <span data-ttu-id="12fd7-107">En lista över datakällor som stöds som datakällor eller sänkor av kopieringsaktiviteten, finns det [stöds datalager](data-factory-data-movement-activities.md#supported-data-stores-and-formats) tabell.</span><span class="sxs-lookup"><span data-stu-id="12fd7-107">For a list of data stores supported as sources or sinks by the copy activity, see the [Supported data stores](data-factory-data-movement-activities.md#supported-data-stores-and-formats) table.</span></span> 

> [!IMPORTANT]
> <span data-ttu-id="12fd7-108">Azure DB Cosmos-anslutningen har endast stöd för DocumentDB-API.</span><span class="sxs-lookup"><span data-stu-id="12fd7-108">Azure Cosmos DB connector only support DocumentDB API.</span></span>

<span data-ttu-id="12fd7-109">Att kopiera data som-är till/från JSON-filer eller en annan Cosmos DB samling finns [Import/Export JSON-dokument](#importexport-json-documents).</span><span class="sxs-lookup"><span data-stu-id="12fd7-109">To copy data as-is to/from JSON files or another Cosmos DB collection, see [Import/Export JSON documents](#importexport-json-documents).</span></span>

## <a name="getting-started"></a><span data-ttu-id="12fd7-110">Komma igång</span><span class="sxs-lookup"><span data-stu-id="12fd7-110">Getting started</span></span>
<span data-ttu-id="12fd7-111">Du kan skapa en pipeline med en kopia-aktivitet som flyttar data till och från Azure Cosmos DB med hjälp av olika verktyg/API: er.</span><span class="sxs-lookup"><span data-stu-id="12fd7-111">You can create a pipeline with a copy activity that moves data to/from Azure Cosmos DB by using different tools/APIs.</span></span>

<span data-ttu-id="12fd7-112">Det enklaste sättet att skapa en pipeline är att använda den **guiden Kopiera**.</span><span class="sxs-lookup"><span data-stu-id="12fd7-112">The easiest way to create a pipeline is to use the **Copy Wizard**.</span></span> <span data-ttu-id="12fd7-113">Finns [Självstudier: skapa en pipeline med hjälp av guiden Kopiera](data-factory-copy-data-wizard-tutorial.md) för en snabb genomgång om hur du skapar en pipeline med hjälp av guiden Kopiera data.</span><span class="sxs-lookup"><span data-stu-id="12fd7-113">See [Tutorial: Create a pipeline using Copy Wizard](data-factory-copy-data-wizard-tutorial.md) for a quick walkthrough on creating a pipeline using the Copy data wizard.</span></span>

<span data-ttu-id="12fd7-114">Du kan också använda följande verktyg för att skapa en pipeline: **Azure-portalen**, **Visual Studio**, **Azure PowerShell**, **Azure Resource Manager-mall**, **.NET API**, och **REST API**.</span><span class="sxs-lookup"><span data-stu-id="12fd7-114">You can also use the following tools to create a pipeline: **Azure portal**, **Visual Studio**, **Azure PowerShell**, **Azure Resource Manager template**, **.NET API**, and **REST API**.</span></span> <span data-ttu-id="12fd7-115">Se [kopiera aktivitet kursen](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) för stegvisa instruktioner för att skapa en pipeline med en Kopieringsaktivitet.</span><span class="sxs-lookup"><span data-stu-id="12fd7-115">See [Copy activity tutorial](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) for step-by-step instructions to create a pipeline with a copy activity.</span></span> 

<span data-ttu-id="12fd7-116">Om du använder verktyg eller API: er, kan du utföra följande steg för att skapa en pipeline som flyttar data från ett dataarkiv som källa till ett dataarkiv som mottagare:</span><span class="sxs-lookup"><span data-stu-id="12fd7-116">Whether you use the tools or APIs, you perform the following steps to create a pipeline that moves data from a source data store to a sink data store:</span></span> 

1. <span data-ttu-id="12fd7-117">Skapa **länkade tjänster** att länka inkommande och utgående data lagras till din data factory.</span><span class="sxs-lookup"><span data-stu-id="12fd7-117">Create **linked services** to link input and output data stores to your data factory.</span></span>
2. <span data-ttu-id="12fd7-118">Skapa **datauppsättningar** att representera inkommande och utgående data för kopieringen.</span><span class="sxs-lookup"><span data-stu-id="12fd7-118">Create **datasets** to represent input and output data for the copy operation.</span></span> 
3. <span data-ttu-id="12fd7-119">Skapa en **pipeline** med en kopia-aktivitet som tar en datamängd som indata och en dataset som utdata.</span><span class="sxs-lookup"><span data-stu-id="12fd7-119">Create a **pipeline** with a copy activity that takes a dataset as an input and a dataset as an output.</span></span> 

<span data-ttu-id="12fd7-120">När du använder guiden skapas JSON definitioner för dessa Data Factory-enheter (länkade tjänster, datauppsättningar och pipelinen) automatiskt för dig.</span><span class="sxs-lookup"><span data-stu-id="12fd7-120">When you use the wizard, JSON definitions for these Data Factory entities (linked services, datasets, and the pipeline) are automatically created for you.</span></span> <span data-ttu-id="12fd7-121">När du använder Verktyg/API: er (utom .NET API), kan du definiera dessa Data Factory-enheter med hjälp av JSON-format.</span><span class="sxs-lookup"><span data-stu-id="12fd7-121">When you use tools/APIs (except .NET API), you define these Data Factory entities by using the JSON format.</span></span>  <span data-ttu-id="12fd7-122">Exempel med JSON-definitioner för Data Factory-entiteter som används för att kopiera data till/från Cosmos DB finns [JSON-exempel](#json-examples) i den här artikeln.</span><span class="sxs-lookup"><span data-stu-id="12fd7-122">For samples with JSON definitions for Data Factory entities that are used to copy data to/from Cosmos DB, see [JSON examples](#json-examples) section of this article.</span></span> 

<span data-ttu-id="12fd7-123">Följande avsnitt innehåller information om JSON-egenskaper som används för att definiera Data Factory entiteter Cosmos DB:</span><span class="sxs-lookup"><span data-stu-id="12fd7-123">The following sections provide details about JSON properties that are used to define Data Factory entities specific to Cosmos DB:</span></span> 

## <a name="linked-service-properties"></a><span data-ttu-id="12fd7-124">Länkad tjänstegenskaper</span><span class="sxs-lookup"><span data-stu-id="12fd7-124">Linked service properties</span></span>
<span data-ttu-id="12fd7-125">Följande tabell innehåller en beskrivning för JSON-element som är specifika för Azure Cosmos DB länkad tjänst.</span><span class="sxs-lookup"><span data-stu-id="12fd7-125">The following table provides description for JSON elements specific to Azure Cosmos DB linked service.</span></span>

| <span data-ttu-id="12fd7-126">**Egenskap**</span><span class="sxs-lookup"><span data-stu-id="12fd7-126">**Property**</span></span> | <span data-ttu-id="12fd7-127">**Beskrivning**</span><span class="sxs-lookup"><span data-stu-id="12fd7-127">**Description**</span></span> | <span data-ttu-id="12fd7-128">**Krävs**</span><span class="sxs-lookup"><span data-stu-id="12fd7-128">**Required**</span></span> |
| --- | --- | --- |
| <span data-ttu-id="12fd7-129">typ</span><span class="sxs-lookup"><span data-stu-id="12fd7-129">type</span></span> |<span data-ttu-id="12fd7-130">Egenskapen type måste anges till: **DocumentDb**</span><span class="sxs-lookup"><span data-stu-id="12fd7-130">The type property must be set to: **DocumentDb**</span></span> |<span data-ttu-id="12fd7-131">Ja</span><span class="sxs-lookup"><span data-stu-id="12fd7-131">Yes</span></span> |
| <span data-ttu-id="12fd7-132">connectionString</span><span class="sxs-lookup"><span data-stu-id="12fd7-132">connectionString</span></span> |<span data-ttu-id="12fd7-133">Ange information som behövs för att ansluta till Azure DB som Cosmos-databasen.</span><span class="sxs-lookup"><span data-stu-id="12fd7-133">Specify information needed to connect to Azure Cosmos DB database.</span></span> |<span data-ttu-id="12fd7-134">Ja</span><span class="sxs-lookup"><span data-stu-id="12fd7-134">Yes</span></span> |

<span data-ttu-id="12fd7-135">Exempel:</span><span class="sxs-lookup"><span data-stu-id="12fd7-135">Example:</span></span>

```JSON
{
  "name": "CosmosDbLinkedService",
  "properties": {
    "type": "DocumentDb",
    "typeProperties": {
      "connectionString": "AccountEndpoint=<EndpointUrl>;AccountKey=<AccessKey>;Database=<Database>"
    }
  }
}
```

## <a name="dataset-properties"></a><span data-ttu-id="12fd7-136">Egenskaper för datamängd</span><span class="sxs-lookup"><span data-stu-id="12fd7-136">Dataset properties</span></span>
<span data-ttu-id="12fd7-137">En fullständig lista över egenskaper som är tillgängliga för att definiera datauppsättningarna & avsnitt hittar du den [skapa datauppsättningar](data-factory-create-datasets.md) artikel.</span><span class="sxs-lookup"><span data-stu-id="12fd7-137">For a full list of sections & properties available for defining datasets please refer to the [Creating datasets](data-factory-create-datasets.md) article.</span></span> <span data-ttu-id="12fd7-138">Avsnitt som struktur, tillgänglighet och princip på en datamängd JSON är liknande för alla typer av dataset (Azure SQL Azure blob, Azure-tabellen, osv.).</span><span class="sxs-lookup"><span data-stu-id="12fd7-138">Sections like structure, availability, and policy of a dataset JSON are similar for all dataset types (Azure SQL, Azure blob, Azure table, etc.).</span></span>

<span data-ttu-id="12fd7-139">Avsnittet typeProperties är olika för varje typ av dataset och ger information om placeringen av data i datalagret.</span><span class="sxs-lookup"><span data-stu-id="12fd7-139">The typeProperties section is different for each type of dataset and provides information about the location of the data in the data store.</span></span> <span data-ttu-id="12fd7-140">TypeProperties avsnittet för datauppsättningen av typen **DocumentDbCollection** har följande egenskaper.</span><span class="sxs-lookup"><span data-stu-id="12fd7-140">The typeProperties section for the dataset of type **DocumentDbCollection** has the following properties.</span></span>

| <span data-ttu-id="12fd7-141">**Egenskap**</span><span class="sxs-lookup"><span data-stu-id="12fd7-141">**Property**</span></span> | <span data-ttu-id="12fd7-142">**Beskrivning**</span><span class="sxs-lookup"><span data-stu-id="12fd7-142">**Description**</span></span> | <span data-ttu-id="12fd7-143">**Krävs**</span><span class="sxs-lookup"><span data-stu-id="12fd7-143">**Required**</span></span> |
| --- | --- | --- |
| <span data-ttu-id="12fd7-144">Samlingsnamn</span><span class="sxs-lookup"><span data-stu-id="12fd7-144">collectionName</span></span> |<span data-ttu-id="12fd7-145">Namnet på samlingen Cosmos DB dokumentet.</span><span class="sxs-lookup"><span data-stu-id="12fd7-145">Name of the Cosmos DB document collection.</span></span> |<span data-ttu-id="12fd7-146">Ja</span><span class="sxs-lookup"><span data-stu-id="12fd7-146">Yes</span></span> |

<span data-ttu-id="12fd7-147">Exempel:</span><span class="sxs-lookup"><span data-stu-id="12fd7-147">Example:</span></span>

```JSON
{
  "name": "PersonCosmosDbTable",
  "properties": {
    "type": "DocumentDbCollection",
    "linkedServiceName": "CosmosDbLinkedService",
    "typeProperties": {
      "collectionName": "Person"
    },
    "external": true,
    "availability": {
      "frequency": "Day",
      "interval": 1
    }
  }
}
```
### <a name="schema-by-data-factory"></a><span data-ttu-id="12fd7-148">Schema som Data Factory</span><span class="sxs-lookup"><span data-stu-id="12fd7-148">Schema by Data Factory</span></span>
<span data-ttu-id="12fd7-149">För schemafria data butiker, till exempel Azure Cosmos DB härleder Data Factory-tjänsten schemat på något av följande sätt:</span><span class="sxs-lookup"><span data-stu-id="12fd7-149">For schema-free data stores such as Azure Cosmos DB, the Data Factory service infers the schema in one of the following ways:</span></span>  

1. <span data-ttu-id="12fd7-150">Om du anger strukturen för data med hjälp av den **struktur** egenskap i datauppsättningsdefinitionen Data Factory-tjänsten godkänner den här strukturen som schema.</span><span class="sxs-lookup"><span data-stu-id="12fd7-150">If you specify the structure of data by using the **structure** property in the dataset definition, the Data Factory service honors this structure as the schema.</span></span> <span data-ttu-id="12fd7-151">Om en rad inte innehåller ett värde för en kolumn, tillhandahålls i det här fallet ett null-värde för den.</span><span class="sxs-lookup"><span data-stu-id="12fd7-151">In this case, if a row does not contain a value for a column, a null value will be provided for it.</span></span>
2. <span data-ttu-id="12fd7-152">Om du inte anger strukturen för data med hjälp av den **struktur** egenskap i datauppsättningsdefinitionen Data Factory-tjänsten härleder schemat med hjälp av den första raden i data.</span><span class="sxs-lookup"><span data-stu-id="12fd7-152">If you do not specify the structure of data by using the **structure** property in the dataset definition, the Data Factory service infers the schema by using the first row in the data.</span></span> <span data-ttu-id="12fd7-153">I det här fallet om den första raden inte innehåller fullständig schemat kommer vissa kolumner att saknas i resultatet av kopieringsåtgärden.</span><span class="sxs-lookup"><span data-stu-id="12fd7-153">In this case, if the first row does not contain the full schema, some columns will be missing in the result of copy operation.</span></span>

<span data-ttu-id="12fd7-154">Därför för schemafria datakällor, det bästa sättet är att ange hur dina data med hjälp av den **struktur** egenskapen.</span><span class="sxs-lookup"><span data-stu-id="12fd7-154">Therefore, for schema-free data sources, the best practice is to specify the structure of data using the **structure** property.</span></span>

## <a name="copy-activity-properties"></a><span data-ttu-id="12fd7-155">Kopiera egenskaper för aktivitet</span><span class="sxs-lookup"><span data-stu-id="12fd7-155">Copy activity properties</span></span>
<span data-ttu-id="12fd7-156">En fullständig lista över egenskaper som är tillgängliga för att definiera aktiviteter & avsnitt hittar du den [skapar Pipelines](data-factory-create-pipelines.md) artikel.</span><span class="sxs-lookup"><span data-stu-id="12fd7-156">For a full list of sections & properties available for defining activities please refer to the [Creating Pipelines](data-factory-create-pipelines.md) article.</span></span> <span data-ttu-id="12fd7-157">Egenskaper som namn, beskrivning, ingående och utgående tabeller och principen är tillgängliga för alla typer av aktiviteter.</span><span class="sxs-lookup"><span data-stu-id="12fd7-157">Properties such as name, description, input and output tables, and policy are available for all types of activities.</span></span>

> [!NOTE]
> <span data-ttu-id="12fd7-158">Kopieringsaktiviteten tar endast en inmatning och ger en enda utdata.</span><span class="sxs-lookup"><span data-stu-id="12fd7-158">The Copy Activity takes only one input and produces only one output.</span></span>

<span data-ttu-id="12fd7-159">Egenskaper som är tillgängliga i avsnittet typeProperties i aktiviteten å andra sidan varierar med varje aktivitetstyp och vid kopieringsaktiviteten de varierar beroende på vilka typer av datakällor och sänkor.</span><span class="sxs-lookup"><span data-stu-id="12fd7-159">Properties available in the typeProperties section of the activity on the other hand vary with each activity type and in case of Copy activity they vary depending on the types of sources and sinks.</span></span>

<span data-ttu-id="12fd7-160">Vid kopieringsaktiviteten när datakällan är av typen **DocumentDbCollectionSource** följande egenskaper är tillgängliga i **typeProperties** avsnitt:</span><span class="sxs-lookup"><span data-stu-id="12fd7-160">In case of Copy activity when source is of type **DocumentDbCollectionSource** the following properties are available in **typeProperties** section:</span></span>

| <span data-ttu-id="12fd7-161">**Egenskap**</span><span class="sxs-lookup"><span data-stu-id="12fd7-161">**Property**</span></span> | <span data-ttu-id="12fd7-162">**Beskrivning**</span><span class="sxs-lookup"><span data-stu-id="12fd7-162">**Description**</span></span> | <span data-ttu-id="12fd7-163">**Tillåtna värden**</span><span class="sxs-lookup"><span data-stu-id="12fd7-163">**Allowed values**</span></span> | <span data-ttu-id="12fd7-164">**Krävs**</span><span class="sxs-lookup"><span data-stu-id="12fd7-164">**Required**</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="12fd7-165">DocumentDB</span><span class="sxs-lookup"><span data-stu-id="12fd7-165">query</span></span> |<span data-ttu-id="12fd7-166">Ange frågan som läser data.</span><span class="sxs-lookup"><span data-stu-id="12fd7-166">Specify the query to read data.</span></span> |<span data-ttu-id="12fd7-167">Frågesträng stöds av Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="12fd7-167">Query string supported by Azure Cosmos DB.</span></span> <br/><br/><span data-ttu-id="12fd7-168">Exempel:`SELECT c.BusinessEntityID, c.PersonType, c.NameStyle, c.Title, c.Name.First AS FirstName, c.Name.Last AS LastName, c.Suffix, c.EmailPromotion FROM c WHERE c.ModifiedDate > \"2009-01-01T00:00:00\"`</span><span class="sxs-lookup"><span data-stu-id="12fd7-168">Example: `SELECT c.BusinessEntityID, c.PersonType, c.NameStyle, c.Title, c.Name.First AS FirstName, c.Name.Last AS LastName, c.Suffix, c.EmailPromotion FROM c WHERE c.ModifiedDate > \"2009-01-01T00:00:00\"`</span></span> |<span data-ttu-id="12fd7-169">Nej</span><span class="sxs-lookup"><span data-stu-id="12fd7-169">No</span></span> <br/><br/><span data-ttu-id="12fd7-170">Om inget annat anges, SQL-instruktionen som körs:`select <columns defined in structure> from mycollection`</span><span class="sxs-lookup"><span data-stu-id="12fd7-170">If not specified, the SQL statement that is executed: `select <columns defined in structure> from mycollection`</span></span> |
| <span data-ttu-id="12fd7-171">nestingSeparator</span><span class="sxs-lookup"><span data-stu-id="12fd7-171">nestingSeparator</span></span> |<span data-ttu-id="12fd7-172">Specialtecken som visar att dokumentet är kapslad</span><span class="sxs-lookup"><span data-stu-id="12fd7-172">Special character to indicate that the document is nested</span></span> |<span data-ttu-id="12fd7-173">Valfritt tecken.</span><span class="sxs-lookup"><span data-stu-id="12fd7-173">Any character.</span></span> <br/><br/><span data-ttu-id="12fd7-174">Azure Cosmos-DB är en NoSQL store för JSON-dokument, där kapslade strukturer är tillåtna.</span><span class="sxs-lookup"><span data-stu-id="12fd7-174">Azure Cosmos DB is a NoSQL store for JSON documents, where nested structures are allowed.</span></span> <span data-ttu-id="12fd7-175">Azure Data Factory gör det möjligt för användaren att ange hierarkin via nestingSeparator, vilket är ””.</span><span class="sxs-lookup"><span data-stu-id="12fd7-175">Azure Data Factory enables user to denote hierarchy via nestingSeparator, which is “.”</span></span> <span data-ttu-id="12fd7-176">i ovanstående exempel.</span><span class="sxs-lookup"><span data-stu-id="12fd7-176">in the above examples.</span></span> <span data-ttu-id="12fd7-177">Med avgränsare, kopieringsaktiviteten genererar ”Name”-objektet med tre underordnade element först mellan- och efternamn enligt ”Name.First”, ”Name.Middle” och ”Name.Last” i tabelldefinitionen.</span><span class="sxs-lookup"><span data-stu-id="12fd7-177">With the separator, the copy activity will generate the “Name” object with three children elements First, Middle and Last, according to “Name.First”, “Name.Middle” and “Name.Last” in the table definition.</span></span> |<span data-ttu-id="12fd7-178">Nej</span><span class="sxs-lookup"><span data-stu-id="12fd7-178">No</span></span> |

<span data-ttu-id="12fd7-179">**DocumentDbCollectionSink** stöder följande egenskaper:</span><span class="sxs-lookup"><span data-stu-id="12fd7-179">**DocumentDbCollectionSink** supports the following properties:</span></span>

| <span data-ttu-id="12fd7-180">**Egenskap**</span><span class="sxs-lookup"><span data-stu-id="12fd7-180">**Property**</span></span> | <span data-ttu-id="12fd7-181">**Beskrivning**</span><span class="sxs-lookup"><span data-stu-id="12fd7-181">**Description**</span></span> | <span data-ttu-id="12fd7-182">**Tillåtna värden**</span><span class="sxs-lookup"><span data-stu-id="12fd7-182">**Allowed values**</span></span> | <span data-ttu-id="12fd7-183">**Krävs**</span><span class="sxs-lookup"><span data-stu-id="12fd7-183">**Required**</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="12fd7-184">nestingSeparator</span><span class="sxs-lookup"><span data-stu-id="12fd7-184">nestingSeparator</span></span> |<span data-ttu-id="12fd7-185">Ett specialtecken i källkolumnsnamnet att ange kapslade dokumentet krävs.</span><span class="sxs-lookup"><span data-stu-id="12fd7-185">A special character in the source column name to indicate that nested document is needed.</span></span> <br/><br/><span data-ttu-id="12fd7-186">Till exempel ovan: `Name.First` i utdata tabell ger följande JSON-strukturen i Cosmos-DB-dokument:</span><span class="sxs-lookup"><span data-stu-id="12fd7-186">For example above: `Name.First` in the output table produces the following JSON structure in the Cosmos DB document:</span></span><br/><br/><span data-ttu-id="12fd7-187">”Name”: {</span><span class="sxs-lookup"><span data-stu-id="12fd7-187">"Name": {</span></span><br/>    <span data-ttu-id="12fd7-188">”Första”: ”John”</span><span class="sxs-lookup"><span data-stu-id="12fd7-188">"First": "John"</span></span><br/><span data-ttu-id="12fd7-189">},</span><span class="sxs-lookup"><span data-stu-id="12fd7-189">},</span></span> |<span data-ttu-id="12fd7-190">Tecken som används för att avgränsa kapslingsnivåer.</span><span class="sxs-lookup"><span data-stu-id="12fd7-190">Character that is used to separate nesting levels.</span></span><br/><br/><span data-ttu-id="12fd7-191">Standardvärdet är `.` (punkt).</span><span class="sxs-lookup"><span data-stu-id="12fd7-191">Default value is `.` (dot).</span></span> |<span data-ttu-id="12fd7-192">Tecken som används för att avgränsa kapslingsnivåer.</span><span class="sxs-lookup"><span data-stu-id="12fd7-192">Character that is used to separate nesting levels.</span></span> <br/><br/><span data-ttu-id="12fd7-193">Standardvärdet är `.` (punkt).</span><span class="sxs-lookup"><span data-stu-id="12fd7-193">Default value is `.` (dot).</span></span> |
| <span data-ttu-id="12fd7-194">writeBatchSize</span><span class="sxs-lookup"><span data-stu-id="12fd7-194">writeBatchSize</span></span> |<span data-ttu-id="12fd7-195">Antalet parallella begäranden till Azure DB som Cosmos-tjänsten att skapa dokument.</span><span class="sxs-lookup"><span data-stu-id="12fd7-195">Number of parallel requests to Azure Cosmos DB service to create documents.</span></span><br/><br/><span data-ttu-id="12fd7-196">Du kan finjustera prestanda vid kopiering av data till och från Cosmos-databas med hjälp av den här egenskapen.</span><span class="sxs-lookup"><span data-stu-id="12fd7-196">You can fine-tune the performance when copying data to/from Cosmos DB by using this property.</span></span> <span data-ttu-id="12fd7-197">Du kan förvänta dig bättre prestanda om du ökar writeBatchSize eftersom flera parallella begäranden till Cosmos DB skickas.</span><span class="sxs-lookup"><span data-stu-id="12fd7-197">You can expect a better performance when you increase writeBatchSize because more parallel requests to Cosmos DB are sent.</span></span> <span data-ttu-id="12fd7-198">Men du behöver undvika begränsning som kan utlösa ett felmeddelande: ”begär frekvensen är stor”.</span><span class="sxs-lookup"><span data-stu-id="12fd7-198">However you’ll need to avoid throttling that can throw the error message: "Request rate is large".</span></span><br/><br/><span data-ttu-id="12fd7-199">Begränsning bestäms av ett antal faktorer, bland annat storlek dokument, antalet villkoren i dokument, indexering princip målsamling osv. För kopieringsåtgärd, du kan använda en bättre samling (t.ex. S3) har mest genomströmning tillgänglig (2 500 begärande enheter per sekund).</span><span class="sxs-lookup"><span data-stu-id="12fd7-199">Throttling is decided by a number of factors, including size of documents, number of terms in documents, indexing policy of target collection, etc. For copy operations, you can use a better collection (e.g. S3) to have the most throughput available (2,500 request units/second).</span></span> |<span data-ttu-id="12fd7-200">Integer</span><span class="sxs-lookup"><span data-stu-id="12fd7-200">Integer</span></span> |<span data-ttu-id="12fd7-201">Nej (standard: 5)</span><span class="sxs-lookup"><span data-stu-id="12fd7-201">No (default: 5)</span></span> |
| <span data-ttu-id="12fd7-202">writeBatchTimeout</span><span class="sxs-lookup"><span data-stu-id="12fd7-202">writeBatchTimeout</span></span> |<span data-ttu-id="12fd7-203">Vänta tills åtgärden har slutförts innan tidsgränsen uppnås.</span><span class="sxs-lookup"><span data-stu-id="12fd7-203">Wait time for the operation to complete before it times out.</span></span> |<span data-ttu-id="12fd7-204">TimeSpan</span><span class="sxs-lookup"><span data-stu-id="12fd7-204">timespan</span></span><br/><br/> <span data-ttu-id="12fd7-205">Exempel ”: 00: 30:00” (30 minuter).</span><span class="sxs-lookup"><span data-stu-id="12fd7-205">Example: “00:30:00” (30 minutes).</span></span> |<span data-ttu-id="12fd7-206">Nej</span><span class="sxs-lookup"><span data-stu-id="12fd7-206">No</span></span> |

## <a name="importexport-json-documents"></a><span data-ttu-id="12fd7-207">Importera och exportera JSON-dokument</span><span class="sxs-lookup"><span data-stu-id="12fd7-207">Import/Export JSON documents</span></span>
<span data-ttu-id="12fd7-208">Den här Cosmos-DB-anslutningen kan du enkelt</span><span class="sxs-lookup"><span data-stu-id="12fd7-208">Using this Cosmos DB connector, you can easily</span></span>

* <span data-ttu-id="12fd7-209">Importera JSON-dokument från olika källor till Cosmos-DB, inklusive Azure Blob Azure Data Lake, lokalt filsystem eller andra filbaserade butiker som stöds av Azure Data Factory.</span><span class="sxs-lookup"><span data-stu-id="12fd7-209">Import JSON documents from various sources into Cosmos DB, including Azure Blob, Azure Data Lake, on-premises File System or other file-based stores supported by Azure Data Factory.</span></span>
* <span data-ttu-id="12fd7-210">Exportera JSON-dokument från Cosmos DB collecton till olika filbaserade butiker.</span><span class="sxs-lookup"><span data-stu-id="12fd7-210">Export JSON documents from Cosmos DB collecton into various file-based stores.</span></span>
* <span data-ttu-id="12fd7-211">Migrera data mellan två Cosmos DB samlingar som-är.</span><span class="sxs-lookup"><span data-stu-id="12fd7-211">Migrate data between two Cosmos DB collections as-is.</span></span>

<span data-ttu-id="12fd7-212">Att uppnå kopian schema-oberoende</span><span class="sxs-lookup"><span data-stu-id="12fd7-212">To achieve such schema-agnostic copy,</span></span> 
* <span data-ttu-id="12fd7-213">När du använder guiden Kopiera, kontrollera den **”exportera som – som JSON-filer eller Cosmos DB samling”** alternativet.</span><span class="sxs-lookup"><span data-stu-id="12fd7-213">When using copy wizard, check the **"Export as-is to JSON files or Cosmos DB collection"** option.</span></span>
* <span data-ttu-id="12fd7-214">När med JSON redigering inte anger avsnittet ”struktur” i Cosmos DB datauppsättning/ar eller egenskapen ”nestingSeparator” i Cosmos DB källor/mottagare i en Kopieringsaktivitet.</span><span class="sxs-lookup"><span data-stu-id="12fd7-214">When using JSON editing, do not specify the "structure" section in Cosmos DB dataset(s) nor "nestingSeparator" property on Cosmos DB source/sink in copy activity.</span></span> <span data-ttu-id="12fd7-215">Om du vill importera från / exportera till JSON-filer, ange formatet i filen store datamängden som ”JsonFormat”, config ”filePattern” och hoppa över inställningarna för rest-format, se [JSON-format](data-factory-supported-file-and-compression-formats.md#json-format) avsnittet detaljer.</span><span class="sxs-lookup"><span data-stu-id="12fd7-215">To import from/export to JSON files, in the file store dataset specify format type as "JsonFormat", config "filePattern" and skip the rest format settings, see [JSON format](data-factory-supported-file-and-compression-formats.md#json-format) section on details.</span></span>

## <a name="json-examples"></a><span data-ttu-id="12fd7-216">JSON-exempel</span><span class="sxs-lookup"><span data-stu-id="12fd7-216">JSON examples</span></span>
<span data-ttu-id="12fd7-217">Följande exempel ger exempel JSON definitioner som du kan använda för att skapa en pipeline med hjälp av [Azure-portalen](data-factory-copy-activity-tutorial-using-azure-portal.md) eller [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md) eller [Azure PowerShell](data-factory-copy-activity-tutorial-using-powershell.md).</span><span class="sxs-lookup"><span data-stu-id="12fd7-217">The following examples provide sample JSON definitions that you can use to create a pipeline by using [Azure portal](data-factory-copy-activity-tutorial-using-azure-portal.md) or [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md) or [Azure PowerShell](data-factory-copy-activity-tutorial-using-powershell.md).</span></span> <span data-ttu-id="12fd7-218">De visar hur du kopierar data till och från Azure Cosmos DB och Azure Blob Storage.</span><span class="sxs-lookup"><span data-stu-id="12fd7-218">They show how to copy data to and from Azure Cosmos DB and Azure Blob Storage.</span></span> <span data-ttu-id="12fd7-219">Dock datan kan kopieras **direkt** från någon av källorna till någon av sänkor anges [här](data-factory-data-movement-activities.md#supported-data-stores-and-formats) med hjälp av aktiviteten kopiera i Azure Data Factory.</span><span class="sxs-lookup"><span data-stu-id="12fd7-219">However, data can be copied **directly** from any of the sources to any of the sinks stated [here](data-factory-data-movement-activities.md#supported-data-stores-and-formats) using the Copy Activity in Azure Data Factory.</span></span>

## <a name="example-copy-data-from-azure-cosmos-db-to-azure-blob"></a><span data-ttu-id="12fd7-220">Exempel: Kopiera data från Azure Cosmos DB till Azure-Blob</span><span class="sxs-lookup"><span data-stu-id="12fd7-220">Example: Copy data from Azure Cosmos DB to Azure Blob</span></span>
<span data-ttu-id="12fd7-221">Exemplet nedan visar:</span><span class="sxs-lookup"><span data-stu-id="12fd7-221">The sample below shows:</span></span>

1. <span data-ttu-id="12fd7-222">En länkad tjänst av typen [DocumentDb](#linked-service-properties).</span><span class="sxs-lookup"><span data-stu-id="12fd7-222">A linked service of type [DocumentDb](#linked-service-properties).</span></span>
2. <span data-ttu-id="12fd7-223">En länkad tjänst av typen [AzureStorage](data-factory-azure-blob-connector.md#linked-service-properties).</span><span class="sxs-lookup"><span data-stu-id="12fd7-223">A linked service of type [AzureStorage](data-factory-azure-blob-connector.md#linked-service-properties).</span></span>
3. <span data-ttu-id="12fd7-224">Indata [dataset](data-factory-create-datasets.md) av typen [DocumentDbCollection](#dataset-properties).</span><span class="sxs-lookup"><span data-stu-id="12fd7-224">An input [dataset](data-factory-create-datasets.md) of type [DocumentDbCollection](#dataset-properties).</span></span>
4. <span data-ttu-id="12fd7-225">Utdata [dataset](data-factory-create-datasets.md) av typen [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties).</span><span class="sxs-lookup"><span data-stu-id="12fd7-225">An output [dataset](data-factory-create-datasets.md) of type [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties).</span></span>
5. <span data-ttu-id="12fd7-226">En [pipeline](data-factory-create-pipelines.md) med Kopieringsaktiviteten som använder [DocumentDbCollectionSource](#copy-activity-properties) och [BlobSink](data-factory-azure-blob-connector.md#copy-activity-properties).</span><span class="sxs-lookup"><span data-stu-id="12fd7-226">A [pipeline](data-factory-create-pipelines.md) with Copy Activity that uses [DocumentDbCollectionSource](#copy-activity-properties) and [BlobSink](data-factory-azure-blob-connector.md#copy-activity-properties).</span></span>

<span data-ttu-id="12fd7-227">Exemplet kopierar data i Azure Cosmos DB till Azure-Blob.</span><span class="sxs-lookup"><span data-stu-id="12fd7-227">The sample copies data in Azure Cosmos DB to Azure Blob.</span></span> <span data-ttu-id="12fd7-228">JSON-egenskaper som används i exemplen beskrivs i exemplen i följande avsnitt.</span><span class="sxs-lookup"><span data-stu-id="12fd7-228">The JSON properties used in these samples are described in sections following the samples.</span></span>

<span data-ttu-id="12fd7-229">**Azure Cosmos-DB länkade tjänsten:**</span><span class="sxs-lookup"><span data-stu-id="12fd7-229">**Azure Cosmos DB linked service:**</span></span>

```JSON
{
  "name": "CosmosDbLinkedService",
  "properties": {
    "type": "DocumentDb",
    "typeProperties": {
      "connectionString": "AccountEndpoint=<EndpointUrl>;AccountKey=<AccessKey>;Database=<Database>"
    }
  }
}
```
<span data-ttu-id="12fd7-230">**Azure Blob storage länkade tjänsten:**</span><span class="sxs-lookup"><span data-stu-id="12fd7-230">**Azure Blob storage linked service:**</span></span>

```JSON
{
  "name": "StorageLinkedService",
  "properties": {
    "type": "AzureStorage",
    "typeProperties": {
      "connectionString": "DefaultEndpointsProtocol=https;AccountName=<accountname>;AccountKey=<accountkey>"
    }
  }
}
```
<span data-ttu-id="12fd7-231">**Azure dokumentet DB indatauppsättning:**</span><span class="sxs-lookup"><span data-stu-id="12fd7-231">**Azure Document DB input dataset:**</span></span>

<span data-ttu-id="12fd7-232">Exemplet förutsätter att du har en samling med namnet **Person** i Azure DB som Cosmos-databasen.</span><span class="sxs-lookup"><span data-stu-id="12fd7-232">The sample assumes you have a collection named **Person** in an Azure Cosmos DB database.</span></span>

<span data-ttu-id="12fd7-233">Inställningen ”externa”: ”true” och ange externalData principinformation Azure Data Factory-tjänsten att tabellen är extern till data factory och inte kommer från en aktivitet i datafabriken.</span><span class="sxs-lookup"><span data-stu-id="12fd7-233">Setting “external”: ”true” and specifying externalData policy information the Azure Data Factory service that the table is external to the data factory and not produced by an activity in the data factory.</span></span>

```JSON
{
  "name": "PersonCosmosDbTable",
  "properties": {
    "type": "DocumentDbCollection",
    "linkedServiceName": "CosmosDbLinkedService",
    "typeProperties": {
      "collectionName": "Person"
    },
    "external": true,
    "availability": {
      "frequency": "Day",
      "interval": 1
    }
  }
}
```

<span data-ttu-id="12fd7-234">**Azure Blob utdatauppsättningen:**</span><span class="sxs-lookup"><span data-stu-id="12fd7-234">**Azure Blob output dataset:**</span></span>

<span data-ttu-id="12fd7-235">Data kopieras till en ny blob varje timme med sökvägen för blobben reflektion specifika datetime med timme granularitet.</span><span class="sxs-lookup"><span data-stu-id="12fd7-235">Data is copied to a new blob every hour with the path for the blob reflecting the specific datetime with hour granularity.</span></span>

```JSON
{
  "name": "PersonBlobTableOut",
  "properties": {
    "type": "AzureBlob",
    "linkedServiceName": "StorageLinkedService",
    "typeProperties": {
      "folderPath": "docdb",
      "format": {
        "type": "TextFormat",
        "columnDelimiter": ",",
        "nullValue": "NULL"
      }
    },
    "availability": {
      "frequency": "Day",
      "interval": 1
    }
  }
}
```
<span data-ttu-id="12fd7-236">Exempel JSON-dokumentet i samlingen Person i en Cosmos-DB-databas:</span><span class="sxs-lookup"><span data-stu-id="12fd7-236">Sample JSON document in the Person collection in a Cosmos DB database:</span></span>

```JSON
{
  "PersonId": 2,
  "Name": {
    "First": "Jane",
    "Middle": "",
    "Last": "Doe"
  }
}
```
<span data-ttu-id="12fd7-237">Cosmos DB stöder förfrågningar till dokument med hjälp av en SQL som syntax över hierarkiska JSON-dokument.</span><span class="sxs-lookup"><span data-stu-id="12fd7-237">Cosmos DB supports querying documents using a SQL like syntax over hierarchical JSON documents.</span></span>

<span data-ttu-id="12fd7-238">Exempel:</span><span class="sxs-lookup"><span data-stu-id="12fd7-238">Example:</span></span> 

```sql
SELECT Person.PersonId, Person.Name.First AS FirstName, Person.Name.Middle as MiddleName, Person.Name.Last AS LastName FROM Person
```

<span data-ttu-id="12fd7-239">Följande pipeline kopierar data från samlingen Person i Azure DB som Cosmos-databasen till en Azure blob.</span><span class="sxs-lookup"><span data-stu-id="12fd7-239">The following pipeline copies data from the Person collection in the Azure Cosmos DB database to an Azure blob.</span></span> <span data-ttu-id="12fd7-240">Datauppsättningar har angetts som en del av kopieringsaktiviteten indata och utdata.</span><span class="sxs-lookup"><span data-stu-id="12fd7-240">As part of the copy activity the input and output datasets have been specified.</span></span>  

```JSON
{
  "name": "DocDbToBlobPipeline",
  "properties": {
    "activities": [
      {
        "type": "Copy",
        "typeProperties": {
          "source": {
            "type": "DocumentDbCollectionSource",
            "query": "SELECT Person.Id, Person.Name.First AS FirstName, Person.Name.Middle as MiddleName, Person.Name.Last AS LastName FROM Person",
            "nestingSeparator": "."
          },
          "sink": {
            "type": "BlobSink",
            "blobWriterAddHeader": true,
            "writeBatchSize": 1000,
            "writeBatchTimeout": "00:00:59"
          }
        },
        "inputs": [
          {
            "name": "PersonCosmosDbTable"
          }
        ],
        "outputs": [
          {
            "name": "PersonBlobTableOut"
          }
        ],
        "policy": {
          "concurrency": 1
        },
        "name": "CopyFromDocDbToBlob"
      }
    ],
    "start": "2015-04-01T00:00:00Z",
    "end": "2015-04-02T00:00:00Z"
  }
}
```
## <a name="example-copy-data-from-azure-blob-to-azure-cosmos-db"></a><span data-ttu-id="12fd7-241">Exempel: Kopiera data från Azure Blob till Azure Cosmos DB</span><span class="sxs-lookup"><span data-stu-id="12fd7-241">Example: Copy data from Azure Blob to Azure Cosmos DB</span></span> 
<span data-ttu-id="12fd7-242">Exemplet nedan visar:</span><span class="sxs-lookup"><span data-stu-id="12fd7-242">The sample below shows:</span></span>

1. <span data-ttu-id="12fd7-243">En länkad tjänst av typen [DocumentDb](#azure-documentdb-linked-service-properties).</span><span class="sxs-lookup"><span data-stu-id="12fd7-243">A linked service of type [DocumentDb](#azure-documentdb-linked-service-properties).</span></span>
2. <span data-ttu-id="12fd7-244">En länkad tjänst av typen [AzureStorage](data-factory-azure-blob-connector.md#linked-service-properties).</span><span class="sxs-lookup"><span data-stu-id="12fd7-244">A linked service of type [AzureStorage](data-factory-azure-blob-connector.md#linked-service-properties).</span></span>
3. <span data-ttu-id="12fd7-245">Indata [dataset](data-factory-create-datasets.md) av typen [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties).</span><span class="sxs-lookup"><span data-stu-id="12fd7-245">An input [dataset](data-factory-create-datasets.md) of type [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties).</span></span>
4. <span data-ttu-id="12fd7-246">Utdata [dataset](data-factory-create-datasets.md) av typen [DocumentDbCollection](#azure-documentdb-dataset-type-properties).</span><span class="sxs-lookup"><span data-stu-id="12fd7-246">An output [dataset](data-factory-create-datasets.md) of type [DocumentDbCollection](#azure-documentdb-dataset-type-properties).</span></span>
5. <span data-ttu-id="12fd7-247">En [pipeline](data-factory-create-pipelines.md) med Kopieringsaktiviteten som använder [BlobSource](data-factory-azure-blob-connector.md#copy-activity-properties) och [DocumentDbCollectionSink](#azure-documentdb-copy-activity-type-properties).</span><span class="sxs-lookup"><span data-stu-id="12fd7-247">A [pipeline](data-factory-create-pipelines.md) with Copy Activity that uses [BlobSource](data-factory-azure-blob-connector.md#copy-activity-properties) and [DocumentDbCollectionSink](#azure-documentdb-copy-activity-type-properties).</span></span>

<span data-ttu-id="12fd7-248">Exemplet kopierar data från Azure blob till Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="12fd7-248">The sample copies data from Azure blob to Azure Cosmos DB.</span></span> <span data-ttu-id="12fd7-249">JSON-egenskaper som används i exemplen beskrivs i exemplen i följande avsnitt.</span><span class="sxs-lookup"><span data-stu-id="12fd7-249">The JSON properties used in these samples are described in sections following the samples.</span></span>

<span data-ttu-id="12fd7-250">**Azure Blob storage länkade tjänsten:**</span><span class="sxs-lookup"><span data-stu-id="12fd7-250">**Azure Blob storage linked service:**</span></span>

```JSON
{
  "name": "StorageLinkedService",
  "properties": {
    "type": "AzureStorage",
    "typeProperties": {
      "connectionString": "DefaultEndpointsProtocol=https;AccountName=<accountname>;AccountKey=<accountkey>"
    }
  }
}
```
<span data-ttu-id="12fd7-251">**Azure Cosmos-DB länkade tjänsten:**</span><span class="sxs-lookup"><span data-stu-id="12fd7-251">**Azure Cosmos DB linked service:**</span></span>

```JSON
{
  "name": "CosmosDbLinkedService",
  "properties": {
    "type": "DocumentDb",
    "typeProperties": {
      "connectionString": "AccountEndpoint=<EndpointUrl>;AccountKey=<AccessKey>;Database=<Database>"
    }
  }
}
```
<span data-ttu-id="12fd7-252">**Azure Blob inkommande datamängd:**</span><span class="sxs-lookup"><span data-stu-id="12fd7-252">**Azure Blob input dataset:**</span></span>

```JSON
{
  "name": "PersonBlobTableIn",
  "properties": {
    "structure": [
      {
        "name": "Id",
        "type": "Int"
      },
      {
        "name": "FirstName",
        "type": "String"
      },
      {
        "name": "MiddleName",
        "type": "String"
      },
      {
        "name": "LastName",
        "type": "String"
      }
    ],
    "type": "AzureBlob",
    "linkedServiceName": "StorageLinkedService",
    "typeProperties": {
      "fileName": "input.csv",
      "folderPath": "docdb",
      "format": {
        "type": "TextFormat",
        "columnDelimiter": ",",
        "nullValue": "NULL"
      }
    },
    "external": true,
    "availability": {
      "frequency": "Day",
      "interval": 1
    }
  }
}
```
<span data-ttu-id="12fd7-253">**Azure Cosmos-DB utdatauppsättningen:**</span><span class="sxs-lookup"><span data-stu-id="12fd7-253">**Azure Cosmos DB output dataset:**</span></span>

<span data-ttu-id="12fd7-254">Exemplet kopierar data till en samling med namnet ”Person”.</span><span class="sxs-lookup"><span data-stu-id="12fd7-254">The sample copies data to a collection named “Person”.</span></span>

```JSON
{
  "name": "PersonCosmosDbTableOut",
  "properties": {
    "structure": [
      {
        "name": "Id",
        "type": "Int"
      },
      {
        "name": "Name.First",
        "type": "String"
      },
      {
        "name": "Name.Middle",
        "type": "String"
      },
      {
        "name": "Name.Last",
        "type": "String"
      }
    ],
    "type": "DocumentDbCollection",
    "linkedServiceName": "CosmosDbLinkedService",
    "typeProperties": {
      "collectionName": "Person"
    },
    "availability": {
      "frequency": "Day",
      "interval": 1
    }
  }
}
```
<span data-ttu-id="12fd7-255">Följande pipeline kopierar data från Azure Blob till samlingen Person i Cosmos-databasen.</span><span class="sxs-lookup"><span data-stu-id="12fd7-255">The following pipeline copies data from Azure Blob to the Person collection in the Cosmos DB.</span></span> <span data-ttu-id="12fd7-256">Datauppsättningar har angetts som en del av kopieringsaktiviteten indata och utdata.</span><span class="sxs-lookup"><span data-stu-id="12fd7-256">As part of the copy activity the input and output datasets have been specified.</span></span>

```JSON
{
  "name": "BlobToDocDbPipeline",
  "properties": {
    "activities": [
      {
        "type": "Copy",
        "typeProperties": {
          "source": {
            "type": "BlobSource"
          },
          "sink": {
            "type": "DocumentDbCollectionSink",
            "nestingSeparator": ".",
            "writeBatchSize": 2,
            "writeBatchTimeout": "00:00:00"
          }
          "translator": {
              "type": "TabularTranslator",
              "ColumnMappings": "FirstName: Name.First, MiddleName: Name.Middle, LastName: Name.Last, BusinessEntityID: BusinessEntityID, PersonType: PersonType, NameStyle: NameStyle, Title: Title, Suffix: Suffix, EmailPromotion: EmailPromotion, rowguid: rowguid, ModifiedDate: ModifiedDate"
          }
        },
        "inputs": [
          {
            "name": "PersonBlobTableIn"
          }
        ],
        "outputs": [
          {
            "name": "PersonCosmosDbTableOut"
          }
        ],
        "policy": {
          "concurrency": 1
        },
        "name": "CopyFromBlobToDocDb"
      }
    ],
    "start": "2015-04-14T00:00:00Z",
    "end": "2015-04-15T00:00:00Z"
  }
}
```
<span data-ttu-id="12fd7-257">Om blob Exempelindata som</span><span class="sxs-lookup"><span data-stu-id="12fd7-257">If the sample blob input is as</span></span>

```
1,John,,Doe
```
<span data-ttu-id="12fd7-258">Utdata JSON i Cosmos DB blir som:</span><span class="sxs-lookup"><span data-stu-id="12fd7-258">Then the output JSON in Cosmos DB will be as:</span></span>

```JSON
{
  "Id": 1,
  "Name": {
    "First": "John",
    "Middle": null,
    "Last": "Doe"
  },
  "id": "a5e8595c-62ec-4554-a118-3940f4ff70b6"
}
```
<span data-ttu-id="12fd7-259">Azure Cosmos-DB är en NoSQL store för JSON-dokument, där kapslade strukturer är tillåtna.</span><span class="sxs-lookup"><span data-stu-id="12fd7-259">Azure Cosmos DB is a NoSQL store for JSON documents, where nested structures are allowed.</span></span> <span data-ttu-id="12fd7-260">Azure Data Factory gör det möjligt för användaren att ange hierarkin via **nestingSeparator**, vilket är ””.</span><span class="sxs-lookup"><span data-stu-id="12fd7-260">Azure Data Factory enables user to denote hierarchy via **nestingSeparator**, which is “.”</span></span> <span data-ttu-id="12fd7-261">i det här exemplet.</span><span class="sxs-lookup"><span data-stu-id="12fd7-261">in this example.</span></span> <span data-ttu-id="12fd7-262">Med avgränsare, kopieringsaktiviteten genererar ”Name”-objektet med tre underordnade element först mellan- och efternamn enligt ”Name.First”, ”Name.Middle” och ”Name.Last” i tabelldefinitionen.</span><span class="sxs-lookup"><span data-stu-id="12fd7-262">With the separator, the copy activity will generate the “Name” object with three children elements First, Middle and Last, according to “Name.First”, “Name.Middle” and “Name.Last” in the table definition.</span></span>

## <a name="appendix"></a><span data-ttu-id="12fd7-263">Bilaga</span><span class="sxs-lookup"><span data-stu-id="12fd7-263">Appendix</span></span>
1. <span data-ttu-id="12fd7-264">**Fråga:** har stöd för att uppdatera Kopieringsaktiviteten befintliga poster?</span><span class="sxs-lookup"><span data-stu-id="12fd7-264">**Question:** Does the Copy Activity support update of existing records?</span></span>

    <span data-ttu-id="12fd7-265">**Svar:** Nej.</span><span class="sxs-lookup"><span data-stu-id="12fd7-265">**Answer:** No.</span></span>
2. <span data-ttu-id="12fd7-266">**Fråga:** hur redan har ett nytt försök till en kopia till Azure Cosmos DB behandlar kopieras poster?</span><span class="sxs-lookup"><span data-stu-id="12fd7-266">**Question:** How does a retry of a copy to Azure Cosmos DB deal with already copied records?</span></span>

    <span data-ttu-id="12fd7-267">**Svar:** om poster har ett ”ID”-fält och kopieringen görs ett försök att infoga en post med samma ID, kopieringen genererar ett fel.</span><span class="sxs-lookup"><span data-stu-id="12fd7-267">**Answer:** If records have an "ID" field and the copy operation tries to insert a record with the same ID, the copy operation throws an error.</span></span>  
3. <span data-ttu-id="12fd7-268">**Fråga:** stöder Data Factory [intervall eller hash-baserad Datapartitionering](../documentdb/documentdb-partition-data.md)?</span><span class="sxs-lookup"><span data-stu-id="12fd7-268">**Question:** Does Data Factory support [range or hash-based data partitioning](../documentdb/documentdb-partition-data.md)?</span></span>

    <span data-ttu-id="12fd7-269">**Svar:** Nej.</span><span class="sxs-lookup"><span data-stu-id="12fd7-269">**Answer:** No.</span></span>
4. <span data-ttu-id="12fd7-270">**Fråga:** kan jag ange mer än en Azure DB som Cosmos-samlingen för en tabell?</span><span class="sxs-lookup"><span data-stu-id="12fd7-270">**Question:** Can I specify more than one Azure Cosmos DB collection for a table?</span></span>

    <span data-ttu-id="12fd7-271">**Svar:** Nej.</span><span class="sxs-lookup"><span data-stu-id="12fd7-271">**Answer:** No.</span></span> <span data-ttu-id="12fd7-272">Endast en samling kan anges just nu.</span><span class="sxs-lookup"><span data-stu-id="12fd7-272">Only one collection can be specified at this time.</span></span>

## <a name="performance-and-tuning"></a><span data-ttu-id="12fd7-273">Prestanda och finjustering</span><span class="sxs-lookup"><span data-stu-id="12fd7-273">Performance and Tuning</span></span>
<span data-ttu-id="12fd7-274">Se [kopiera aktivitet prestanda och justera guiden](data-factory-copy-activity-performance.md) vill veta mer om viktiga faktorer som påverkan prestanda för flytt av data (Kopieringsaktiviteten) i Azure Data Factory och olika sätt att optimera den.</span><span class="sxs-lookup"><span data-stu-id="12fd7-274">See [Copy Activity Performance & Tuning Guide](data-factory-copy-activity-performance.md) to learn about key factors that impact performance of data movement (Copy Activity) in Azure Data Factory and various ways to optimize it.</span></span>
