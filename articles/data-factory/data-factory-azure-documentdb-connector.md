---
title: "aaaMove data till/från Azure Cosmos DB | Microsoft Docs"
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
ms.openlocfilehash: bd23ce4e004a972ce6f3e4165cfdea4f0c18fecc
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="move-data-tooand-from-azure-cosmos-db-using-azure-data-factory"></a><span data-ttu-id="33753-103">Flytta data tooand från Azure Cosmos-databasen med Azure Data Factory</span><span class="sxs-lookup"><span data-stu-id="33753-103">Move data tooand from Azure Cosmos DB using Azure Data Factory</span></span>
<span data-ttu-id="33753-104">Den här artikeln förklarar hur toouse hello Kopieringsaktiviteten i Azure Data Factory toomove data till och från Azure Cosmos DB (DocumentDB-API).</span><span class="sxs-lookup"><span data-stu-id="33753-104">This article explains how toouse hello Copy Activity in Azure Data Factory toomove data to/from Azure Cosmos DB (DocumentDB API).</span></span> <span data-ttu-id="33753-105">Den bygger på hello [Data Movement aktiviteter](data-factory-data-movement-activities.md) artikel som ger en allmän översikt över dataflyttning hello kopieringsaktiviteten.</span><span class="sxs-lookup"><span data-stu-id="33753-105">It builds on hello [Data Movement Activities](data-factory-data-movement-activities.md) article, which presents a general overview of data movement with hello copy activity.</span></span> 

<span data-ttu-id="33753-106">Du kan kopiera data från en stöds källa data tooAzure Cosmos DB eller från Azure Cosmos DB tooany stöds sink data.</span><span class="sxs-lookup"><span data-stu-id="33753-106">You can copy data from any supported source data store tooAzure Cosmos DB or from Azure Cosmos DB tooany supported sink data store.</span></span> <span data-ttu-id="33753-107">En lista över datakällor som stöds som datakällor eller sänkor av hello kopieringsaktiviteten finns hello [stöds datalager](data-factory-data-movement-activities.md#supported-data-stores-and-formats) tabell.</span><span class="sxs-lookup"><span data-stu-id="33753-107">For a list of data stores supported as sources or sinks by hello copy activity, see hello [Supported data stores](data-factory-data-movement-activities.md#supported-data-stores-and-formats) table.</span></span> 

> [!IMPORTANT]
> <span data-ttu-id="33753-108">Azure DB Cosmos-anslutningen har endast stöd för DocumentDB-API.</span><span class="sxs-lookup"><span data-stu-id="33753-108">Azure Cosmos DB connector only support DocumentDB API.</span></span>

<span data-ttu-id="33753-109">toocopy data som-är till/från JSON-filer eller en annan Cosmos DB samling finns [Import/Export JSON-dokument](#importexport-json-documents).</span><span class="sxs-lookup"><span data-stu-id="33753-109">toocopy data as-is to/from JSON files or another Cosmos DB collection, see [Import/Export JSON documents](#importexport-json-documents).</span></span>

## <a name="getting-started"></a><span data-ttu-id="33753-110">Komma igång</span><span class="sxs-lookup"><span data-stu-id="33753-110">Getting started</span></span>
<span data-ttu-id="33753-111">Du kan skapa en pipeline med en kopia-aktivitet som flyttar data till och från Azure Cosmos DB med hjälp av olika verktyg/API: er.</span><span class="sxs-lookup"><span data-stu-id="33753-111">You can create a pipeline with a copy activity that moves data to/from Azure Cosmos DB by using different tools/APIs.</span></span>

<span data-ttu-id="33753-112">hello enklaste sättet toocreate en pipeline är toouse hello **guiden Kopiera**.</span><span class="sxs-lookup"><span data-stu-id="33753-112">hello easiest way toocreate a pipeline is toouse hello **Copy Wizard**.</span></span> <span data-ttu-id="33753-113">Se [Självstudier: skapa en pipeline med hjälp av guiden Kopiera](data-factory-copy-data-wizard-tutorial.md) för en snabb genomgång om hur du skapar en pipeline med hjälp av guiden för hello kopiera data.</span><span class="sxs-lookup"><span data-stu-id="33753-113">See [Tutorial: Create a pipeline using Copy Wizard](data-factory-copy-data-wizard-tutorial.md) for a quick walkthrough on creating a pipeline using hello Copy data wizard.</span></span>

<span data-ttu-id="33753-114">Du kan också använda följande verktyg toocreate en pipeline hello: **Azure-portalen**, **Visual Studio**, **Azure PowerShell**, **Azure Resource Manager-mall** , **.NET API**, och **REST API**.</span><span class="sxs-lookup"><span data-stu-id="33753-114">You can also use hello following tools toocreate a pipeline: **Azure portal**, **Visual Studio**, **Azure PowerShell**, **Azure Resource Manager template**, **.NET API**, and **REST API**.</span></span> <span data-ttu-id="33753-115">Se [kopiera aktivitet kursen](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) för stegvisa instruktioner toocreate en pipeline med en Kopieringsaktivitet.</span><span class="sxs-lookup"><span data-stu-id="33753-115">See [Copy activity tutorial](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) for step-by-step instructions toocreate a pipeline with a copy activity.</span></span> 

<span data-ttu-id="33753-116">Om du använder hello verktyg eller API: er kan utföra du hello följande steg toocreate en pipeline som flyttar data från en källdata lagra tooa sink-datalagret:</span><span class="sxs-lookup"><span data-stu-id="33753-116">Whether you use hello tools or APIs, you perform hello following steps toocreate a pipeline that moves data from a source data store tooa sink data store:</span></span> 

1. <span data-ttu-id="33753-117">Skapa **länkade tjänster** toolink indata och utdata lagrar tooyour data factory.</span><span class="sxs-lookup"><span data-stu-id="33753-117">Create **linked services** toolink input and output data stores tooyour data factory.</span></span>
2. <span data-ttu-id="33753-118">Skapa **datauppsättningar** toorepresent indata och utdata för hello kopieringsåtgärden.</span><span class="sxs-lookup"><span data-stu-id="33753-118">Create **datasets** toorepresent input and output data for hello copy operation.</span></span> 
3. <span data-ttu-id="33753-119">Skapa en **pipeline** med en kopia-aktivitet som tar en datamängd som indata och en dataset som utdata.</span><span class="sxs-lookup"><span data-stu-id="33753-119">Create a **pipeline** with a copy activity that takes a dataset as an input and a dataset as an output.</span></span> 

<span data-ttu-id="33753-120">När du använder guiden hello skapas automatiskt JSON definitioner för dessa Data Factory-enheter (länkade tjänster, datauppsättningar och hello pipeline) för dig.</span><span class="sxs-lookup"><span data-stu-id="33753-120">When you use hello wizard, JSON definitions for these Data Factory entities (linked services, datasets, and hello pipeline) are automatically created for you.</span></span> <span data-ttu-id="33753-121">När du använder Verktyg/API: er (utom .NET API), kan du definiera dessa Data Factory-enheter med hjälp av hello JSON-format.</span><span class="sxs-lookup"><span data-stu-id="33753-121">When you use tools/APIs (except .NET API), you define these Data Factory entities by using hello JSON format.</span></span>  <span data-ttu-id="33753-122">Exempel med JSON-definitioner för Data Factory-entiteter som har använt toocopy data till/från Cosmos DB finns [JSON-exempel](#json-examples) i den här artikeln.</span><span class="sxs-lookup"><span data-stu-id="33753-122">For samples with JSON definitions for Data Factory entities that are used toocopy data to/from Cosmos DB, see [JSON examples](#json-examples) section of this article.</span></span> 

<span data-ttu-id="33753-123">hello följande avsnitt innehåller information om JSON-egenskaper används toodefine Data Factory entiteter specifika tooCosmos DB:</span><span class="sxs-lookup"><span data-stu-id="33753-123">hello following sections provide details about JSON properties that are used toodefine Data Factory entities specific tooCosmos DB:</span></span> 

## <a name="linked-service-properties"></a><span data-ttu-id="33753-124">Länkad tjänstegenskaper</span><span class="sxs-lookup"><span data-stu-id="33753-124">Linked service properties</span></span>
<span data-ttu-id="33753-125">hello följande tabell ger en beskrivning för JSON-element specifika tooAzure Cosmos DB länkade tjänsten.</span><span class="sxs-lookup"><span data-stu-id="33753-125">hello following table provides description for JSON elements specific tooAzure Cosmos DB linked service.</span></span>

| <span data-ttu-id="33753-126">**Egenskap**</span><span class="sxs-lookup"><span data-stu-id="33753-126">**Property**</span></span> | <span data-ttu-id="33753-127">**Beskrivning**</span><span class="sxs-lookup"><span data-stu-id="33753-127">**Description**</span></span> | <span data-ttu-id="33753-128">**Krävs**</span><span class="sxs-lookup"><span data-stu-id="33753-128">**Required**</span></span> |
| --- | --- | --- |
| <span data-ttu-id="33753-129">typ</span><span class="sxs-lookup"><span data-stu-id="33753-129">type</span></span> |<span data-ttu-id="33753-130">hello Typegenskapen måste anges till: **DocumentDb**</span><span class="sxs-lookup"><span data-stu-id="33753-130">hello type property must be set to: **DocumentDb**</span></span> |<span data-ttu-id="33753-131">Ja</span><span class="sxs-lookup"><span data-stu-id="33753-131">Yes</span></span> |
| <span data-ttu-id="33753-132">connectionString</span><span class="sxs-lookup"><span data-stu-id="33753-132">connectionString</span></span> |<span data-ttu-id="33753-133">Ange information som behövs för tooconnect tooAzure Cosmos-DB-databas.</span><span class="sxs-lookup"><span data-stu-id="33753-133">Specify information needed tooconnect tooAzure Cosmos DB database.</span></span> |<span data-ttu-id="33753-134">Ja</span><span class="sxs-lookup"><span data-stu-id="33753-134">Yes</span></span> |

<span data-ttu-id="33753-135">Exempel:</span><span class="sxs-lookup"><span data-stu-id="33753-135">Example:</span></span>

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

## <a name="dataset-properties"></a><span data-ttu-id="33753-136">Egenskaper för datamängd</span><span class="sxs-lookup"><span data-stu-id="33753-136">Dataset properties</span></span>
<span data-ttu-id="33753-137">En fullständig lista över egenskaper som är tillgängliga för att definiera datauppsättningarna & avsnitt finns toohello [skapa datauppsättningar](data-factory-create-datasets.md) artikel.</span><span class="sxs-lookup"><span data-stu-id="33753-137">For a full list of sections & properties available for defining datasets please refer toohello [Creating datasets](data-factory-create-datasets.md) article.</span></span> <span data-ttu-id="33753-138">Avsnitt som struktur, tillgänglighet och princip på en datamängd JSON är liknande för alla typer av dataset (Azure SQL Azure blob, Azure-tabellen, osv.).</span><span class="sxs-lookup"><span data-stu-id="33753-138">Sections like structure, availability, and policy of a dataset JSON are similar for all dataset types (Azure SQL, Azure blob, Azure table, etc.).</span></span>

<span data-ttu-id="33753-139">hello typeProperties avsnittet är olika för varje typ av dataset och ger information om hello platsen för hello data i datalagret hello.</span><span class="sxs-lookup"><span data-stu-id="33753-139">hello typeProperties section is different for each type of dataset and provides information about hello location of hello data in hello data store.</span></span> <span data-ttu-id="33753-140">Hej typeProperties avsnittet för hello dataset av typen **DocumentDbCollection** har hello följande egenskaper.</span><span class="sxs-lookup"><span data-stu-id="33753-140">hello typeProperties section for hello dataset of type **DocumentDbCollection** has hello following properties.</span></span>

| <span data-ttu-id="33753-141">**Egenskap**</span><span class="sxs-lookup"><span data-stu-id="33753-141">**Property**</span></span> | <span data-ttu-id="33753-142">**Beskrivning**</span><span class="sxs-lookup"><span data-stu-id="33753-142">**Description**</span></span> | <span data-ttu-id="33753-143">**Krävs**</span><span class="sxs-lookup"><span data-stu-id="33753-143">**Required**</span></span> |
| --- | --- | --- |
| <span data-ttu-id="33753-144">Samlingsnamn</span><span class="sxs-lookup"><span data-stu-id="33753-144">collectionName</span></span> |<span data-ttu-id="33753-145">Namnet på hello Cosmos DB dokumentsamlingen.</span><span class="sxs-lookup"><span data-stu-id="33753-145">Name of hello Cosmos DB document collection.</span></span> |<span data-ttu-id="33753-146">Ja</span><span class="sxs-lookup"><span data-stu-id="33753-146">Yes</span></span> |

<span data-ttu-id="33753-147">Exempel:</span><span class="sxs-lookup"><span data-stu-id="33753-147">Example:</span></span>

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
### <a name="schema-by-data-factory"></a><span data-ttu-id="33753-148">Schema som Data Factory</span><span class="sxs-lookup"><span data-stu-id="33753-148">Schema by Data Factory</span></span>
<span data-ttu-id="33753-149">För schemafria data butiker, till exempel Azure Cosmos DB skapar hello Data Factory-tjänsten hello schema i något av följande sätt hello:</span><span class="sxs-lookup"><span data-stu-id="33753-149">For schema-free data stores such as Azure Cosmos DB, hello Data Factory service infers hello schema in one of hello following ways:</span></span>  

1. <span data-ttu-id="33753-150">Om du anger hello strukturen för data med hjälp av hello **struktur** egenskap i hello Data Factory-tjänsten i datauppsättningsdefinitionen hello godkänner den här strukturen som hello schema.</span><span class="sxs-lookup"><span data-stu-id="33753-150">If you specify hello structure of data by using hello **structure** property in hello dataset definition, hello Data Factory service honors this structure as hello schema.</span></span> <span data-ttu-id="33753-151">Om en rad inte innehåller ett värde för en kolumn, tillhandahålls i det här fallet ett null-värde för den.</span><span class="sxs-lookup"><span data-stu-id="33753-151">In this case, if a row does not contain a value for a column, a null value will be provided for it.</span></span>
2. <span data-ttu-id="33753-152">Om du inte anger hello strukturen för data med hjälp av hello **struktur** egenskap i hello datauppsättningsdefinitionen, hello Data Factory-tjänsten skapar hello schema med hjälp av hello första raden i hello data.</span><span class="sxs-lookup"><span data-stu-id="33753-152">If you do not specify hello structure of data by using hello **structure** property in hello dataset definition, hello Data Factory service infers hello schema by using hello first row in hello data.</span></span> <span data-ttu-id="33753-153">I det här fallet om hello första raden inte innehåller fullständig hello-schemat, kommer vissa kolumner att saknas i hello resultatet av kopieringsåtgärden.</span><span class="sxs-lookup"><span data-stu-id="33753-153">In this case, if hello first row does not contain hello full schema, some columns will be missing in hello result of copy operation.</span></span>

<span data-ttu-id="33753-154">Därför är hello bästa praxis för schemafria datakällor toospecify hello struktur data med hjälp av hello **struktur** egenskapen.</span><span class="sxs-lookup"><span data-stu-id="33753-154">Therefore, for schema-free data sources, hello best practice is toospecify hello structure of data using hello **structure** property.</span></span>

## <a name="copy-activity-properties"></a><span data-ttu-id="33753-155">Kopiera egenskaper för aktivitet</span><span class="sxs-lookup"><span data-stu-id="33753-155">Copy activity properties</span></span>
<span data-ttu-id="33753-156">En fullständig lista över avsnitt & egenskaper som är tillgängliga för att definiera aktiviteter finns toohello [skapar Pipelines](data-factory-create-pipelines.md) artikel.</span><span class="sxs-lookup"><span data-stu-id="33753-156">For a full list of sections & properties available for defining activities please refer toohello [Creating Pipelines](data-factory-create-pipelines.md) article.</span></span> <span data-ttu-id="33753-157">Egenskaper som namn, beskrivning, ingående och utgående tabeller och principen är tillgängliga för alla typer av aktiviteter.</span><span class="sxs-lookup"><span data-stu-id="33753-157">Properties such as name, description, input and output tables, and policy are available for all types of activities.</span></span>

> [!NOTE]
> <span data-ttu-id="33753-158">Hej Kopieringsaktiviteten tar endast en inmatning och ger en enda utdata.</span><span class="sxs-lookup"><span data-stu-id="33753-158">hello Copy Activity takes only one input and produces only one output.</span></span>

<span data-ttu-id="33753-159">Egenskaper i hello typeProperties avsnittet hello aktivitet på hello andra sidan varierar med varje aktivitetstyp och vid kopieringsaktiviteten de varierar beroende på hello typer av datakällor och sänkor.</span><span class="sxs-lookup"><span data-stu-id="33753-159">Properties available in hello typeProperties section of hello activity on hello other hand vary with each activity type and in case of Copy activity they vary depending on hello types of sources and sinks.</span></span>

<span data-ttu-id="33753-160">Vid kopieringsaktiviteten när datakällan är av typen **DocumentDbCollectionSource** hello följande egenskaper är tillgängliga i **typeProperties** avsnitt:</span><span class="sxs-lookup"><span data-stu-id="33753-160">In case of Copy activity when source is of type **DocumentDbCollectionSource** hello following properties are available in **typeProperties** section:</span></span>

| <span data-ttu-id="33753-161">**Egenskap**</span><span class="sxs-lookup"><span data-stu-id="33753-161">**Property**</span></span> | <span data-ttu-id="33753-162">**Beskrivning**</span><span class="sxs-lookup"><span data-stu-id="33753-162">**Description**</span></span> | <span data-ttu-id="33753-163">**Tillåtna värden**</span><span class="sxs-lookup"><span data-stu-id="33753-163">**Allowed values**</span></span> | <span data-ttu-id="33753-164">**Krävs**</span><span class="sxs-lookup"><span data-stu-id="33753-164">**Required**</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="33753-165">DocumentDB</span><span class="sxs-lookup"><span data-stu-id="33753-165">query</span></span> |<span data-ttu-id="33753-166">Ange hello frågan tooread data.</span><span class="sxs-lookup"><span data-stu-id="33753-166">Specify hello query tooread data.</span></span> |<span data-ttu-id="33753-167">Frågesträng stöds av Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="33753-167">Query string supported by Azure Cosmos DB.</span></span> <br/><br/><span data-ttu-id="33753-168">Exempel:`SELECT c.BusinessEntityID, c.PersonType, c.NameStyle, c.Title, c.Name.First AS FirstName, c.Name.Last AS LastName, c.Suffix, c.EmailPromotion FROM c WHERE c.ModifiedDate > \"2009-01-01T00:00:00\"`</span><span class="sxs-lookup"><span data-stu-id="33753-168">Example: `SELECT c.BusinessEntityID, c.PersonType, c.NameStyle, c.Title, c.Name.First AS FirstName, c.Name.Last AS LastName, c.Suffix, c.EmailPromotion FROM c WHERE c.ModifiedDate > \"2009-01-01T00:00:00\"`</span></span> |<span data-ttu-id="33753-169">Nej</span><span class="sxs-lookup"><span data-stu-id="33753-169">No</span></span> <br/><br/><span data-ttu-id="33753-170">Om inget anges hello SQL-instruktionen som körs:`select <columns defined in structure> from mycollection`</span><span class="sxs-lookup"><span data-stu-id="33753-170">If not specified, hello SQL statement that is executed: `select <columns defined in structure> from mycollection`</span></span> |
| <span data-ttu-id="33753-171">nestingSeparator</span><span class="sxs-lookup"><span data-stu-id="33753-171">nestingSeparator</span></span> |<span data-ttu-id="33753-172">Specialtecken tooindicate som hello dokumentet är kapslad</span><span class="sxs-lookup"><span data-stu-id="33753-172">Special character tooindicate that hello document is nested</span></span> |<span data-ttu-id="33753-173">Valfritt tecken.</span><span class="sxs-lookup"><span data-stu-id="33753-173">Any character.</span></span> <br/><br/><span data-ttu-id="33753-174">Azure Cosmos-DB är en NoSQL store för JSON-dokument, där kapslade strukturer är tillåtna.</span><span class="sxs-lookup"><span data-stu-id="33753-174">Azure Cosmos DB is a NoSQL store for JSON documents, where nested structures are allowed.</span></span> <span data-ttu-id="33753-175">Azure Data Factory aktiverar toodenote användarhierarkin via nestingSeparator, vilket är ””.</span><span class="sxs-lookup"><span data-stu-id="33753-175">Azure Data Factory enables user toodenote hierarchy via nestingSeparator, which is “.”</span></span> <span data-ttu-id="33753-176">i hello exemplen ovan.</span><span class="sxs-lookup"><span data-stu-id="33753-176">in hello above examples.</span></span> <span data-ttu-id="33753-177">Med hello avgränsare hello kopieringsaktiviteten genererar hello ”Name”-objekt med tre underordnade element första mellersta och sista, bl.a too"Name.First”, ”Name.Middle” och ”Name.Last” i hello tabell definition.</span><span class="sxs-lookup"><span data-stu-id="33753-177">With hello separator, hello copy activity will generate hello “Name” object with three children elements First, Middle and Last, according too“Name.First”, “Name.Middle” and “Name.Last” in hello table definition.</span></span> |<span data-ttu-id="33753-178">Nej</span><span class="sxs-lookup"><span data-stu-id="33753-178">No</span></span> |

<span data-ttu-id="33753-179">**DocumentDbCollectionSink** stöder hello följande egenskaper:</span><span class="sxs-lookup"><span data-stu-id="33753-179">**DocumentDbCollectionSink** supports hello following properties:</span></span>

| <span data-ttu-id="33753-180">**Egenskap**</span><span class="sxs-lookup"><span data-stu-id="33753-180">**Property**</span></span> | <span data-ttu-id="33753-181">**Beskrivning**</span><span class="sxs-lookup"><span data-stu-id="33753-181">**Description**</span></span> | <span data-ttu-id="33753-182">**Tillåtna värden**</span><span class="sxs-lookup"><span data-stu-id="33753-182">**Allowed values**</span></span> | <span data-ttu-id="33753-183">**Krävs**</span><span class="sxs-lookup"><span data-stu-id="33753-183">**Required**</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="33753-184">nestingSeparator</span><span class="sxs-lookup"><span data-stu-id="33753-184">nestingSeparator</span></span> |<span data-ttu-id="33753-185">Det krävs ett specialtecken i hello källa kolumnen namn tooindicate som kapslade dokument.</span><span class="sxs-lookup"><span data-stu-id="33753-185">A special character in hello source column name tooindicate that nested document is needed.</span></span> <br/><br/><span data-ttu-id="33753-186">Till exempel ovan: `Name.First` i hello utdata producerar tabellen hello följande JSON-strukturen i hello Cosmos DB dokument:</span><span class="sxs-lookup"><span data-stu-id="33753-186">For example above: `Name.First` in hello output table produces hello following JSON structure in hello Cosmos DB document:</span></span><br/><br/><span data-ttu-id="33753-187">”Name”: {</span><span class="sxs-lookup"><span data-stu-id="33753-187">"Name": {</span></span><br/>    <span data-ttu-id="33753-188">”Första”: ”John”</span><span class="sxs-lookup"><span data-stu-id="33753-188">"First": "John"</span></span><br/><span data-ttu-id="33753-189">},</span><span class="sxs-lookup"><span data-stu-id="33753-189">},</span></span> |<span data-ttu-id="33753-190">Tecken som används tooseparate kapslingsnivåer.</span><span class="sxs-lookup"><span data-stu-id="33753-190">Character that is used tooseparate nesting levels.</span></span><br/><br/><span data-ttu-id="33753-191">Standardvärdet är `.` (punkt).</span><span class="sxs-lookup"><span data-stu-id="33753-191">Default value is `.` (dot).</span></span> |<span data-ttu-id="33753-192">Tecken som används tooseparate kapslingsnivåer.</span><span class="sxs-lookup"><span data-stu-id="33753-192">Character that is used tooseparate nesting levels.</span></span> <br/><br/><span data-ttu-id="33753-193">Standardvärdet är `.` (punkt).</span><span class="sxs-lookup"><span data-stu-id="33753-193">Default value is `.` (dot).</span></span> |
| <span data-ttu-id="33753-194">writeBatchSize</span><span class="sxs-lookup"><span data-stu-id="33753-194">writeBatchSize</span></span> |<span data-ttu-id="33753-195">Antalet parallella begäranden tooAzure Cosmos DB toocreate servicedokument.</span><span class="sxs-lookup"><span data-stu-id="33753-195">Number of parallel requests tooAzure Cosmos DB service toocreate documents.</span></span><br/><br/><span data-ttu-id="33753-196">Du kan finjustera hello prestanda vid kopiering av data till och från Cosmos-databas med hjälp av den här egenskapen.</span><span class="sxs-lookup"><span data-stu-id="33753-196">You can fine-tune hello performance when copying data to/from Cosmos DB by using this property.</span></span> <span data-ttu-id="33753-197">Du kan förvänta dig bättre prestanda om du ökar writeBatchSize eftersom flera parallella begäranden tooCosmos DB skickas.</span><span class="sxs-lookup"><span data-stu-id="33753-197">You can expect a better performance when you increase writeBatchSize because more parallel requests tooCosmos DB are sent.</span></span> <span data-ttu-id="33753-198">Men du behöver tooavoid begränsning som kan utlösa hello felmeddelande: ”begär frekvensen är stor”.</span><span class="sxs-lookup"><span data-stu-id="33753-198">However you’ll need tooavoid throttling that can throw hello error message: "Request rate is large".</span></span><br/><br/><span data-ttu-id="33753-199">Begränsning bestäms av ett antal faktorer, bland annat storlek dokument, antalet villkoren i dokument, indexering princip målsamling osv. Kopieringen, kan du använda en bättre samling (t.ex. S3) toohave hello de flesta genomströmning tillgänglig (2 500 begärande enheter per sekund).</span><span class="sxs-lookup"><span data-stu-id="33753-199">Throttling is decided by a number of factors, including size of documents, number of terms in documents, indexing policy of target collection, etc. For copy operations, you can use a better collection (e.g. S3) toohave hello most throughput available (2,500 request units/second).</span></span> |<span data-ttu-id="33753-200">Integer</span><span class="sxs-lookup"><span data-stu-id="33753-200">Integer</span></span> |<span data-ttu-id="33753-201">Nej (standard: 5)</span><span class="sxs-lookup"><span data-stu-id="33753-201">No (default: 5)</span></span> |
| <span data-ttu-id="33753-202">writeBatchTimeout</span><span class="sxs-lookup"><span data-stu-id="33753-202">writeBatchTimeout</span></span> |<span data-ttu-id="33753-203">Vänta tills hello åtgärden toocomplete innan tidsgränsen uppnås.</span><span class="sxs-lookup"><span data-stu-id="33753-203">Wait time for hello operation toocomplete before it times out.</span></span> |<span data-ttu-id="33753-204">TimeSpan</span><span class="sxs-lookup"><span data-stu-id="33753-204">timespan</span></span><br/><br/> <span data-ttu-id="33753-205">Exempel ”: 00: 30:00” (30 minuter).</span><span class="sxs-lookup"><span data-stu-id="33753-205">Example: “00:30:00” (30 minutes).</span></span> |<span data-ttu-id="33753-206">Nej</span><span class="sxs-lookup"><span data-stu-id="33753-206">No</span></span> |

## <a name="importexport-json-documents"></a><span data-ttu-id="33753-207">Importera och exportera JSON-dokument</span><span class="sxs-lookup"><span data-stu-id="33753-207">Import/Export JSON documents</span></span>
<span data-ttu-id="33753-208">Den här Cosmos-DB-anslutningen kan du enkelt</span><span class="sxs-lookup"><span data-stu-id="33753-208">Using this Cosmos DB connector, you can easily</span></span>

* <span data-ttu-id="33753-209">Importera JSON-dokument från olika källor till Cosmos-DB, inklusive Azure Blob Azure Data Lake, lokalt filsystem eller andra filbaserade butiker som stöds av Azure Data Factory.</span><span class="sxs-lookup"><span data-stu-id="33753-209">Import JSON documents from various sources into Cosmos DB, including Azure Blob, Azure Data Lake, on-premises File System or other file-based stores supported by Azure Data Factory.</span></span>
* <span data-ttu-id="33753-210">Exportera JSON-dokument från Cosmos DB collecton till olika filbaserade butiker.</span><span class="sxs-lookup"><span data-stu-id="33753-210">Export JSON documents from Cosmos DB collecton into various file-based stores.</span></span>
* <span data-ttu-id="33753-211">Migrera data mellan två Cosmos DB samlingar som-är.</span><span class="sxs-lookup"><span data-stu-id="33753-211">Migrate data between two Cosmos DB collections as-is.</span></span>

<span data-ttu-id="33753-212">tooachieve sådana schema-oberoende kopiera,</span><span class="sxs-lookup"><span data-stu-id="33753-212">tooachieve such schema-agnostic copy,</span></span> 
* <span data-ttu-id="33753-213">När du använder guiden Kopiera Kontrollera hello **”exportera som-tooJSON filer eller Cosmos DB samlingen”** alternativet.</span><span class="sxs-lookup"><span data-stu-id="33753-213">When using copy wizard, check hello **"Export as-is tooJSON files or Cosmos DB collection"** option.</span></span>
* <span data-ttu-id="33753-214">När med JSON redigering inte anger hello ”struktur” avsnittet i Cosmos DB datauppsättning/ar eller egenskapen ”nestingSeparator” i Cosmos DB källor/mottagare i en Kopieringsaktivitet.</span><span class="sxs-lookup"><span data-stu-id="33753-214">When using JSON editing, do not specify hello "structure" section in Cosmos DB dataset(s) nor "nestingSeparator" property on Cosmos DB source/sink in copy activity.</span></span> <span data-ttu-id="33753-215">tooimport från / exportera tooJSON filer, ange formattyp i hello filen store dataset som ”JsonFormat”, config ”filePattern” och hoppa över hello rest-formatinställningar, se [JSON-format](data-factory-supported-file-and-compression-formats.md#json-format) avsnittet detaljer.</span><span class="sxs-lookup"><span data-stu-id="33753-215">tooimport from/export tooJSON files, in hello file store dataset specify format type as "JsonFormat", config "filePattern" and skip hello rest format settings, see [JSON format](data-factory-supported-file-and-compression-formats.md#json-format) section on details.</span></span>

## <a name="json-examples"></a><span data-ttu-id="33753-216">JSON-exempel</span><span class="sxs-lookup"><span data-stu-id="33753-216">JSON examples</span></span>
<span data-ttu-id="33753-217">hello följande exempel ger exempel JSON definitioner som du kan använda toocreate en pipeline med hjälp av [Azure-portalen](data-factory-copy-activity-tutorial-using-azure-portal.md) eller [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md) eller [Azure PowerShell](data-factory-copy-activity-tutorial-using-powershell.md).</span><span class="sxs-lookup"><span data-stu-id="33753-217">hello following examples provide sample JSON definitions that you can use toocreate a pipeline by using [Azure portal](data-factory-copy-activity-tutorial-using-azure-portal.md) or [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md) or [Azure PowerShell](data-factory-copy-activity-tutorial-using-powershell.md).</span></span> <span data-ttu-id="33753-218">De visar hur toocopy data tooand från Azure Cosmos DB och Azure Blob Storage.</span><span class="sxs-lookup"><span data-stu-id="33753-218">They show how toocopy data tooand from Azure Cosmos DB and Azure Blob Storage.</span></span> <span data-ttu-id="33753-219">Dock datan kan kopieras **direkt** från någon av hello källor tooany av hello sänkor anges [här](data-factory-data-movement-activities.md#supported-data-stores-and-formats) med hello Kopieringsaktiviteten i Azure Data Factory.</span><span class="sxs-lookup"><span data-stu-id="33753-219">However, data can be copied **directly** from any of hello sources tooany of hello sinks stated [here](data-factory-data-movement-activities.md#supported-data-stores-and-formats) using hello Copy Activity in Azure Data Factory.</span></span>

## <a name="example-copy-data-from-azure-cosmos-db-tooazure-blob"></a><span data-ttu-id="33753-220">Exempel: Kopiera data från Azure Cosmos DB tooAzure Blob</span><span class="sxs-lookup"><span data-stu-id="33753-220">Example: Copy data from Azure Cosmos DB tooAzure Blob</span></span>
<span data-ttu-id="33753-221">hello exemplet nedan visar:</span><span class="sxs-lookup"><span data-stu-id="33753-221">hello sample below shows:</span></span>

1. <span data-ttu-id="33753-222">En länkad tjänst av typen [DocumentDb](#linked-service-properties).</span><span class="sxs-lookup"><span data-stu-id="33753-222">A linked service of type [DocumentDb](#linked-service-properties).</span></span>
2. <span data-ttu-id="33753-223">En länkad tjänst av typen [AzureStorage](data-factory-azure-blob-connector.md#linked-service-properties).</span><span class="sxs-lookup"><span data-stu-id="33753-223">A linked service of type [AzureStorage](data-factory-azure-blob-connector.md#linked-service-properties).</span></span>
3. <span data-ttu-id="33753-224">Indata [dataset](data-factory-create-datasets.md) av typen [DocumentDbCollection](#dataset-properties).</span><span class="sxs-lookup"><span data-stu-id="33753-224">An input [dataset](data-factory-create-datasets.md) of type [DocumentDbCollection](#dataset-properties).</span></span>
4. <span data-ttu-id="33753-225">Utdata [dataset](data-factory-create-datasets.md) av typen [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties).</span><span class="sxs-lookup"><span data-stu-id="33753-225">An output [dataset](data-factory-create-datasets.md) of type [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties).</span></span>
5. <span data-ttu-id="33753-226">En [pipeline](data-factory-create-pipelines.md) med Kopieringsaktiviteten som använder [DocumentDbCollectionSource](#copy-activity-properties) och [BlobSink](data-factory-azure-blob-connector.md#copy-activity-properties).</span><span class="sxs-lookup"><span data-stu-id="33753-226">A [pipeline](data-factory-create-pipelines.md) with Copy Activity that uses [DocumentDbCollectionSource](#copy-activity-properties) and [BlobSink](data-factory-azure-blob-connector.md#copy-activity-properties).</span></span>

<span data-ttu-id="33753-227">hello exemplet kopierar data i Azure Cosmos DB tooAzure Blob.</span><span class="sxs-lookup"><span data-stu-id="33753-227">hello sample copies data in Azure Cosmos DB tooAzure Blob.</span></span> <span data-ttu-id="33753-228">hello JSON egenskaper som används i exemplen beskrivs i hello-exempel i följande avsnitt.</span><span class="sxs-lookup"><span data-stu-id="33753-228">hello JSON properties used in these samples are described in sections following hello samples.</span></span>

<span data-ttu-id="33753-229">**Azure Cosmos-DB länkade tjänsten:**</span><span class="sxs-lookup"><span data-stu-id="33753-229">**Azure Cosmos DB linked service:**</span></span>

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
<span data-ttu-id="33753-230">**Azure Blob storage länkade tjänsten:**</span><span class="sxs-lookup"><span data-stu-id="33753-230">**Azure Blob storage linked service:**</span></span>

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
<span data-ttu-id="33753-231">**Azure dokumentet DB indatauppsättning:**</span><span class="sxs-lookup"><span data-stu-id="33753-231">**Azure Document DB input dataset:**</span></span>

<span data-ttu-id="33753-232">hello exemplet förutsätter att du har en samling med namnet **Person** i Azure DB som Cosmos-databasen.</span><span class="sxs-lookup"><span data-stu-id="33753-232">hello sample assumes you have a collection named **Person** in an Azure Cosmos DB database.</span></span>

<span data-ttu-id="33753-233">Inställningen ”externa”: ”true” och ange externalData principinformation hello Azure Data Factory-tjänsten hello tabellen är externa toohello data factory och inte kommer från en aktivitet i hello data factory.</span><span class="sxs-lookup"><span data-stu-id="33753-233">Setting “external”: ”true” and specifying externalData policy information hello Azure Data Factory service that hello table is external toohello data factory and not produced by an activity in hello data factory.</span></span>

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

<span data-ttu-id="33753-234">**Azure Blob utdatauppsättningen:**</span><span class="sxs-lookup"><span data-stu-id="33753-234">**Azure Blob output dataset:**</span></span>

<span data-ttu-id="33753-235">Data är kopierade tooa nya blob varje timme med hello sökvägen för hello blob reflektion hello specifika datetime med timme granularitet.</span><span class="sxs-lookup"><span data-stu-id="33753-235">Data is copied tooa new blob every hour with hello path for hello blob reflecting hello specific datetime with hour granularity.</span></span>

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
<span data-ttu-id="33753-236">Exempel JSON-dokumentet i hello Person samling i en Cosmos-DB-databas:</span><span class="sxs-lookup"><span data-stu-id="33753-236">Sample JSON document in hello Person collection in a Cosmos DB database:</span></span>

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
<span data-ttu-id="33753-237">Cosmos DB stöder förfrågningar till dokument med hjälp av en SQL som syntax över hierarkiska JSON-dokument.</span><span class="sxs-lookup"><span data-stu-id="33753-237">Cosmos DB supports querying documents using a SQL like syntax over hierarchical JSON documents.</span></span>

<span data-ttu-id="33753-238">Exempel:</span><span class="sxs-lookup"><span data-stu-id="33753-238">Example:</span></span> 

```sql
SELECT Person.PersonId, Person.Name.First AS FirstName, Person.Name.Middle as MiddleName, Person.Name.Last AS LastName FROM Person
```

<span data-ttu-id="33753-239">hello följande pipeline kopierar data från hello Person samling i hello Azure Cosmos DB databasen tooan Azure blob.</span><span class="sxs-lookup"><span data-stu-id="33753-239">hello following pipeline copies data from hello Person collection in hello Azure Cosmos DB database tooan Azure blob.</span></span> <span data-ttu-id="33753-240">Som en del av hello kopiera aktivitet hello har inkommande och utgående datauppsättningar angetts.</span><span class="sxs-lookup"><span data-stu-id="33753-240">As part of hello copy activity hello input and output datasets have been specified.</span></span>  

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
## <a name="example-copy-data-from-azure-blob-tooazure-cosmos-db"></a><span data-ttu-id="33753-241">Exempel: Kopiera data från Azure Blob-tooAzure Cosmos DB</span><span class="sxs-lookup"><span data-stu-id="33753-241">Example: Copy data from Azure Blob tooAzure Cosmos DB</span></span> 
<span data-ttu-id="33753-242">hello exemplet nedan visar:</span><span class="sxs-lookup"><span data-stu-id="33753-242">hello sample below shows:</span></span>

1. <span data-ttu-id="33753-243">En länkad tjänst av typen [DocumentDb](#azure-documentdb-linked-service-properties).</span><span class="sxs-lookup"><span data-stu-id="33753-243">A linked service of type [DocumentDb](#azure-documentdb-linked-service-properties).</span></span>
2. <span data-ttu-id="33753-244">En länkad tjänst av typen [AzureStorage](data-factory-azure-blob-connector.md#linked-service-properties).</span><span class="sxs-lookup"><span data-stu-id="33753-244">A linked service of type [AzureStorage](data-factory-azure-blob-connector.md#linked-service-properties).</span></span>
3. <span data-ttu-id="33753-245">Indata [dataset](data-factory-create-datasets.md) av typen [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties).</span><span class="sxs-lookup"><span data-stu-id="33753-245">An input [dataset](data-factory-create-datasets.md) of type [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties).</span></span>
4. <span data-ttu-id="33753-246">Utdata [dataset](data-factory-create-datasets.md) av typen [DocumentDbCollection](#azure-documentdb-dataset-type-properties).</span><span class="sxs-lookup"><span data-stu-id="33753-246">An output [dataset](data-factory-create-datasets.md) of type [DocumentDbCollection](#azure-documentdb-dataset-type-properties).</span></span>
5. <span data-ttu-id="33753-247">En [pipeline](data-factory-create-pipelines.md) med Kopieringsaktiviteten som använder [BlobSource](data-factory-azure-blob-connector.md#copy-activity-properties) och [DocumentDbCollectionSink](#azure-documentdb-copy-activity-type-properties).</span><span class="sxs-lookup"><span data-stu-id="33753-247">A [pipeline](data-factory-create-pipelines.md) with Copy Activity that uses [BlobSource](data-factory-azure-blob-connector.md#copy-activity-properties) and [DocumentDbCollectionSink](#azure-documentdb-copy-activity-type-properties).</span></span>

<span data-ttu-id="33753-248">hello exemplet kopierar data från Azure blob-tooAzure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="33753-248">hello sample copies data from Azure blob tooAzure Cosmos DB.</span></span> <span data-ttu-id="33753-249">hello JSON egenskaper som används i exemplen beskrivs i hello-exempel i följande avsnitt.</span><span class="sxs-lookup"><span data-stu-id="33753-249">hello JSON properties used in these samples are described in sections following hello samples.</span></span>

<span data-ttu-id="33753-250">**Azure Blob storage länkade tjänsten:**</span><span class="sxs-lookup"><span data-stu-id="33753-250">**Azure Blob storage linked service:**</span></span>

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
<span data-ttu-id="33753-251">**Azure Cosmos-DB länkade tjänsten:**</span><span class="sxs-lookup"><span data-stu-id="33753-251">**Azure Cosmos DB linked service:**</span></span>

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
<span data-ttu-id="33753-252">**Azure Blob inkommande datamängd:**</span><span class="sxs-lookup"><span data-stu-id="33753-252">**Azure Blob input dataset:**</span></span>

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
<span data-ttu-id="33753-253">**Azure Cosmos-DB utdatauppsättningen:**</span><span class="sxs-lookup"><span data-stu-id="33753-253">**Azure Cosmos DB output dataset:**</span></span>

<span data-ttu-id="33753-254">hello exemplet kopierar tooa datainsamling med namnet ”Person”.</span><span class="sxs-lookup"><span data-stu-id="33753-254">hello sample copies data tooa collection named “Person”.</span></span>

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
<span data-ttu-id="33753-255">hello följande pipeline kopierar data från Azure Blob toohello Person samling i hello Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="33753-255">hello following pipeline copies data from Azure Blob toohello Person collection in hello Cosmos DB.</span></span> <span data-ttu-id="33753-256">Som en del av hello kopiera aktivitet hello har inkommande och utgående datauppsättningar angetts.</span><span class="sxs-lookup"><span data-stu-id="33753-256">As part of hello copy activity hello input and output datasets have been specified.</span></span>

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
              "ColumnMappings": "FirstName: Name.First, MiddleName: Name.Middle, LastName: Name.Last, BusinessEntityID: BusinessEntityID, PersonType: PersonType, NameStyle: NameStyle, title: aaaTitle, Suffix: Suffix, EmailPromotion: EmailPromotion, rowguid: rowguid, ModifiedDate: ModifiedDate"
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
<span data-ttu-id="33753-257">Om hello Exempelindata blob som</span><span class="sxs-lookup"><span data-stu-id="33753-257">If hello sample blob input is as</span></span>

```
1,John,,Doe
```
<span data-ttu-id="33753-258">Hello utdata JSON i Cosmos DB blir som:</span><span class="sxs-lookup"><span data-stu-id="33753-258">Then hello output JSON in Cosmos DB will be as:</span></span>

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
<span data-ttu-id="33753-259">Azure Cosmos-DB är en NoSQL store för JSON-dokument, där kapslade strukturer är tillåtna.</span><span class="sxs-lookup"><span data-stu-id="33753-259">Azure Cosmos DB is a NoSQL store for JSON documents, where nested structures are allowed.</span></span> <span data-ttu-id="33753-260">Azure Data Factory aktiverar toodenote användarhierarkin via **nestingSeparator**, vilket är ””.</span><span class="sxs-lookup"><span data-stu-id="33753-260">Azure Data Factory enables user toodenote hierarchy via **nestingSeparator**, which is “.”</span></span> <span data-ttu-id="33753-261">i det här exemplet.</span><span class="sxs-lookup"><span data-stu-id="33753-261">in this example.</span></span> <span data-ttu-id="33753-262">Med hello avgränsare hello kopieringsaktiviteten genererar hello ”Name”-objekt med tre underordnade element första mellersta och sista, bl.a too"Name.First”, ”Name.Middle” och ”Name.Last” i hello tabell definition.</span><span class="sxs-lookup"><span data-stu-id="33753-262">With hello separator, hello copy activity will generate hello “Name” object with three children elements First, Middle and Last, according too“Name.First”, “Name.Middle” and “Name.Last” in hello table definition.</span></span>

## <a name="appendix"></a><span data-ttu-id="33753-263">Bilaga</span><span class="sxs-lookup"><span data-stu-id="33753-263">Appendix</span></span>
1. <span data-ttu-id="33753-264">**Fråga:** hello Kopieringsaktiviteten stöd för uppdatering av befintliga poster?</span><span class="sxs-lookup"><span data-stu-id="33753-264">**Question:** Does hello Copy Activity support update of existing records?</span></span>

    <span data-ttu-id="33753-265">**Svar:** Nej.</span><span class="sxs-lookup"><span data-stu-id="33753-265">**Answer:** No.</span></span>
2. <span data-ttu-id="33753-266">**Fråga:** hur redan har ett nytt försök till en kopia tooAzure Cosmos DB behandlar kopieras poster?</span><span class="sxs-lookup"><span data-stu-id="33753-266">**Question:** How does a retry of a copy tooAzure Cosmos DB deal with already copied records?</span></span>

    <span data-ttu-id="33753-267">**Svar:** om poster har ett ”ID”-fält och hello kopieringsåtgärden försöker tooinsert post med hello samma ID, hello kopieringsåtgärden genererar ett fel.</span><span class="sxs-lookup"><span data-stu-id="33753-267">**Answer:** If records have an "ID" field and hello copy operation tries tooinsert a record with hello same ID, hello copy operation throws an error.</span></span>  
3. <span data-ttu-id="33753-268">**Fråga:** stöder Data Factory [intervall eller hash-baserad Datapartitionering](../documentdb/documentdb-partition-data.md)?</span><span class="sxs-lookup"><span data-stu-id="33753-268">**Question:** Does Data Factory support [range or hash-based data partitioning](../documentdb/documentdb-partition-data.md)?</span></span>

    <span data-ttu-id="33753-269">**Svar:** Nej.</span><span class="sxs-lookup"><span data-stu-id="33753-269">**Answer:** No.</span></span>
4. <span data-ttu-id="33753-270">**Fråga:** kan jag ange mer än en Azure DB som Cosmos-samlingen för en tabell?</span><span class="sxs-lookup"><span data-stu-id="33753-270">**Question:** Can I specify more than one Azure Cosmos DB collection for a table?</span></span>

    <span data-ttu-id="33753-271">**Svar:** Nej.</span><span class="sxs-lookup"><span data-stu-id="33753-271">**Answer:** No.</span></span> <span data-ttu-id="33753-272">Endast en samling kan anges just nu.</span><span class="sxs-lookup"><span data-stu-id="33753-272">Only one collection can be specified at this time.</span></span>

## <a name="performance-and-tuning"></a><span data-ttu-id="33753-273">Prestanda och finjustering</span><span class="sxs-lookup"><span data-stu-id="33753-273">Performance and Tuning</span></span>
<span data-ttu-id="33753-274">Se [kopiera aktivitet prestanda och justera guiden](data-factory-copy-activity-performance.md) toolearn om nyckeln faktorer som påverkan prestanda för flytt av data (Kopieringsaktiviteten) i Azure Data Factory och olika sätt toooptimize den.</span><span class="sxs-lookup"><span data-stu-id="33753-274">See [Copy Activity Performance & Tuning Guide](data-factory-copy-activity-performance.md) toolearn about key factors that impact performance of data movement (Copy Activity) in Azure Data Factory and various ways toooptimize it.</span></span>
