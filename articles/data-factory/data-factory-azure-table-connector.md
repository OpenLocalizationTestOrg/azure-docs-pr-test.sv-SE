---
title: "Flytta data till/från Azure Table | Microsoft Docs"
description: "Lär dig mer om att flytta data till och från Azure Table Storage med hjälp av Azure Data Factory."
services: data-factory
documentationcenter: 
author: linda33wj
manager: jhubbard
editor: monicar
ms.assetid: 07b046b1-7884-4e57-a613-337292416319
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/22/2017
ms.author: jingwang
ms.openlocfilehash: 792a551ae3dae46c503e5f0dda74cd0ac3a69c3a
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="move-data-to-and-from-azure-table-using-azure-data-factory"></a><span data-ttu-id="b606b-103">Flytta data till och från Azure-tabellen med hjälp av Azure Data Factory</span><span class="sxs-lookup"><span data-stu-id="b606b-103">Move data to and from Azure Table using Azure Data Factory</span></span>
<span data-ttu-id="b606b-104">Den här artikeln förklarar hur du använder aktiviteten kopiera i Azure Data Factory för att flytta data till och från Azure Table Storage.</span><span class="sxs-lookup"><span data-stu-id="b606b-104">This article explains how to use the Copy Activity in Azure Data Factory to move data to/from Azure Table Storage.</span></span> <span data-ttu-id="b606b-105">Den bygger på den [Data Movement aktiviteter](data-factory-data-movement-activities.md) artikel som presenterar en allmän översikt över dataflyttning med copy-aktivitet.</span><span class="sxs-lookup"><span data-stu-id="b606b-105">It builds on the [Data Movement Activities](data-factory-data-movement-activities.md) article, which presents a general overview of data movement with the copy activity.</span></span> 

<span data-ttu-id="b606b-106">Du kan kopiera data från alla datalager för stöds källan till Azure Table Storage eller Azure Table Storage till alla stöds sink-datalagret.</span><span class="sxs-lookup"><span data-stu-id="b606b-106">You can copy data from any supported source data store to Azure Table Storage or from Azure Table Storage to any supported sink data store.</span></span> <span data-ttu-id="b606b-107">En lista över datakällor som stöds som datakällor eller sänkor av kopieringsaktiviteten, finns det [stöds datalager](data-factory-data-movement-activities.md#supported-data-stores-and-formats) tabell.</span><span class="sxs-lookup"><span data-stu-id="b606b-107">For a list of data stores supported as sources or sinks by the copy activity, see the [Supported data stores](data-factory-data-movement-activities.md#supported-data-stores-and-formats) table.</span></span> 

## <a name="getting-started"></a><span data-ttu-id="b606b-108">Komma igång</span><span class="sxs-lookup"><span data-stu-id="b606b-108">Getting started</span></span>
<span data-ttu-id="b606b-109">Du kan skapa en pipeline med en kopia-aktivitet som flyttar data till och från en Azure Table Storage med hjälp av olika verktyg/API: er.</span><span class="sxs-lookup"><span data-stu-id="b606b-109">You can create a pipeline with a copy activity that moves data to/from an Azure Table Storage by using different tools/APIs.</span></span>

<span data-ttu-id="b606b-110">Det enklaste sättet att skapa en pipeline är att använda den **guiden Kopiera**.</span><span class="sxs-lookup"><span data-stu-id="b606b-110">The easiest way to create a pipeline is to use the **Copy Wizard**.</span></span> <span data-ttu-id="b606b-111">Finns [Självstudier: skapa en pipeline med hjälp av guiden Kopiera](data-factory-copy-data-wizard-tutorial.md) för en snabb genomgång om hur du skapar en pipeline med hjälp av guiden Kopiera data.</span><span class="sxs-lookup"><span data-stu-id="b606b-111">See [Tutorial: Create a pipeline using Copy Wizard](data-factory-copy-data-wizard-tutorial.md) for a quick walkthrough on creating a pipeline using the Copy data wizard.</span></span>

<span data-ttu-id="b606b-112">Du kan också använda följande verktyg för att skapa en pipeline: **Azure-portalen**, **Visual Studio**, **Azure PowerShell**, **Azure Resource Manager-mall**, **.NET API**, och **REST API**.</span><span class="sxs-lookup"><span data-stu-id="b606b-112">You can also use the following tools to create a pipeline: **Azure portal**, **Visual Studio**, **Azure PowerShell**, **Azure Resource Manager template**, **.NET API**, and **REST API**.</span></span> <span data-ttu-id="b606b-113">Se [kopiera aktivitet kursen](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) för stegvisa instruktioner för att skapa en pipeline med en Kopieringsaktivitet.</span><span class="sxs-lookup"><span data-stu-id="b606b-113">See [Copy activity tutorial](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) for step-by-step instructions to create a pipeline with a copy activity.</span></span> 

<span data-ttu-id="b606b-114">Om du använder verktyg eller API: er, kan du utföra följande steg för att skapa en pipeline som flyttar data från ett dataarkiv som källa till ett dataarkiv som mottagare:</span><span class="sxs-lookup"><span data-stu-id="b606b-114">Whether you use the tools or APIs, you perform the following steps to create a pipeline that moves data from a source data store to a sink data store:</span></span> 

1. <span data-ttu-id="b606b-115">Skapa **länkade tjänster** att länka inkommande och utgående data lagras till din data factory.</span><span class="sxs-lookup"><span data-stu-id="b606b-115">Create **linked services** to link input and output data stores to your data factory.</span></span>
2. <span data-ttu-id="b606b-116">Skapa **datauppsättningar** att representera inkommande och utgående data för kopieringen.</span><span class="sxs-lookup"><span data-stu-id="b606b-116">Create **datasets** to represent input and output data for the copy operation.</span></span> 
3. <span data-ttu-id="b606b-117">Skapa en **pipeline** med en kopia-aktivitet som tar en datamängd som indata och en dataset som utdata.</span><span class="sxs-lookup"><span data-stu-id="b606b-117">Create a **pipeline** with a copy activity that takes a dataset as an input and a dataset as an output.</span></span> 

<span data-ttu-id="b606b-118">När du använder guiden skapas JSON definitioner för dessa Data Factory-enheter (länkade tjänster, datauppsättningar och pipelinen) automatiskt för dig.</span><span class="sxs-lookup"><span data-stu-id="b606b-118">When you use the wizard, JSON definitions for these Data Factory entities (linked services, datasets, and the pipeline) are automatically created for you.</span></span> <span data-ttu-id="b606b-119">När du använder Verktyg/API: er (utom .NET API), kan du definiera dessa Data Factory-enheter med hjälp av JSON-format.</span><span class="sxs-lookup"><span data-stu-id="b606b-119">When you use tools/APIs (except .NET API), you define these Data Factory entities by using the JSON format.</span></span>  <span data-ttu-id="b606b-120">Exempel med JSON-definitioner för Data Factory-entiteter som används för att kopiera data till/från en Azure-tabellagring finns [JSON-exempel](#json-examples) i den här artikeln.</span><span class="sxs-lookup"><span data-stu-id="b606b-120">For samples with JSON definitions for Data Factory entities that are used to copy data to/from an Azure Table Storage, see [JSON examples](#json-examples) section of this article.</span></span> 

<span data-ttu-id="b606b-121">Följande avsnitt innehåller information om JSON-egenskaper som används för att definiera Data Factory entiteter till Azure Table Storage:</span><span class="sxs-lookup"><span data-stu-id="b606b-121">The following sections provide details about JSON properties that are used to define Data Factory entities specific to Azure Table Storage:</span></span> 

## <a name="linked-service-properties"></a><span data-ttu-id="b606b-122">Länkad tjänstegenskaper</span><span class="sxs-lookup"><span data-stu-id="b606b-122">Linked service properties</span></span>
<span data-ttu-id="b606b-123">Det finns två typer av länkade tjänster som du kan använda för att länka en Azure-blobblagring till ett Azure data factory.</span><span class="sxs-lookup"><span data-stu-id="b606b-123">There are two types of linked services you can use to link an Azure blob storage to an Azure data factory.</span></span> <span data-ttu-id="b606b-124">De är: **AzureStorage** länkade tjänsten och **AzureStorageSas** länkade tjänsten.</span><span class="sxs-lookup"><span data-stu-id="b606b-124">They are: **AzureStorage** linked service and **AzureStorageSas** linked service.</span></span> <span data-ttu-id="b606b-125">Länkad Azure Storage-tjänsten tillhandahåller data factory med global åtkomst till Azure Storage.</span><span class="sxs-lookup"><span data-stu-id="b606b-125">The Azure Storage linked service provides the data factory with global access to the Azure Storage.</span></span> <span data-ttu-id="b606b-126">Medan det Azure Storage SAS (signatur för delad åtkomst) länkade ger tjänsten data factory med begränsad/Tidsbundna åtkomst till Azure Storage.</span><span class="sxs-lookup"><span data-stu-id="b606b-126">Whereas, The Azure Storage SAS (Shared Access Signature) linked service provides the data factory with restricted/time-bound access to the Azure Storage.</span></span> <span data-ttu-id="b606b-127">Det finns några skillnader mellan dessa två länkade tjänster.</span><span class="sxs-lookup"><span data-stu-id="b606b-127">There are no other differences between these two linked services.</span></span> <span data-ttu-id="b606b-128">Välj den länkade tjänst som passar dina behov.</span><span class="sxs-lookup"><span data-stu-id="b606b-128">Choose the linked service that suits your needs.</span></span> <span data-ttu-id="b606b-129">Följande avsnitt innehåller mer information om dessa två länkade tjänster.</span><span class="sxs-lookup"><span data-stu-id="b606b-129">The following sections provide more details on these two linked services.</span></span>

[!INCLUDE [data-factory-azure-storage-linked-services](../../includes/data-factory-azure-storage-linked-services.md)]

## <a name="dataset-properties"></a><span data-ttu-id="b606b-130">Egenskaper för datamängd</span><span class="sxs-lookup"><span data-stu-id="b606b-130">Dataset properties</span></span>
<span data-ttu-id="b606b-131">En fullständig lista över egenskaper som är tillgängliga för att definiera datauppsättningarna & avsnitt finns i [skapa datauppsättningar](data-factory-create-datasets.md) artikel.</span><span class="sxs-lookup"><span data-stu-id="b606b-131">For a full list of sections & properties available for defining datasets, see the [Creating datasets](data-factory-create-datasets.md) article.</span></span> <span data-ttu-id="b606b-132">Avsnitt som struktur, tillgänglighet och princip på en datamängd JSON är liknande för alla typer av dataset (Azure SQL Azure blob, Azure-tabellen, osv.).</span><span class="sxs-lookup"><span data-stu-id="b606b-132">Sections such as structure, availability, and policy of a dataset JSON are similar for all dataset types (Azure SQL, Azure blob, Azure table, etc.).</span></span>

<span data-ttu-id="b606b-133">Avsnittet typeProperties är olika för varje typ av dataset och ger information om placeringen av data i datalagret.</span><span class="sxs-lookup"><span data-stu-id="b606b-133">The typeProperties section is different for each type of dataset and provides information about the location of the data in the data store.</span></span> <span data-ttu-id="b606b-134">Den **typeProperties** avsnittet för datauppsättningen av typen **AzureTable** har följande egenskaper.</span><span class="sxs-lookup"><span data-stu-id="b606b-134">The **typeProperties** section for the dataset of type **AzureTable** has the following properties.</span></span>

| <span data-ttu-id="b606b-135">Egenskap</span><span class="sxs-lookup"><span data-stu-id="b606b-135">Property</span></span> | <span data-ttu-id="b606b-136">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="b606b-136">Description</span></span> | <span data-ttu-id="b606b-137">Krävs</span><span class="sxs-lookup"><span data-stu-id="b606b-137">Required</span></span> |
| --- | --- | --- |
| <span data-ttu-id="b606b-138">tableName</span><span class="sxs-lookup"><span data-stu-id="b606b-138">tableName</span></span> |<span data-ttu-id="b606b-139">Namnet på tabellen i Azure Table databasinstansen som den länkade tjänsten refererar till.</span><span class="sxs-lookup"><span data-stu-id="b606b-139">Name of the table in the Azure Table Database instance that linked service refers to.</span></span> |<span data-ttu-id="b606b-140">Ja.</span><span class="sxs-lookup"><span data-stu-id="b606b-140">Yes.</span></span> <span data-ttu-id="b606b-141">När ett tabellnamn har angetts utan ett azureTableSourceQuery, kopieras alla poster från tabellen till målet.</span><span class="sxs-lookup"><span data-stu-id="b606b-141">When a tableName is specified without an azureTableSourceQuery, all records from the table are copied to the destination.</span></span> <span data-ttu-id="b606b-142">Om en azureTableSourceQuery också anges kopieras poster från tabellen som uppfyller frågan till målet.</span><span class="sxs-lookup"><span data-stu-id="b606b-142">If an azureTableSourceQuery is also specified, records from the table that satisfies the query are copied to the destination.</span></span> |

### <a name="schema-by-data-factory"></a><span data-ttu-id="b606b-143">Schema som Data Factory</span><span class="sxs-lookup"><span data-stu-id="b606b-143">Schema by Data Factory</span></span>
<span data-ttu-id="b606b-144">För schemafria data butiker, till exempel Azure Table härleder Data Factory-tjänsten schemat på något av följande sätt:</span><span class="sxs-lookup"><span data-stu-id="b606b-144">For schema-free data stores such as Azure Table, the Data Factory service infers the schema in one of the following ways:</span></span>

1. <span data-ttu-id="b606b-145">Om du anger strukturen för data med hjälp av den **struktur** egenskap i datauppsättningsdefinitionen Data Factory-tjänsten godkänner den här strukturen som schema.</span><span class="sxs-lookup"><span data-stu-id="b606b-145">If you specify the structure of data by using the **structure** property in the dataset definition, the Data Factory service honors this structure as the schema.</span></span> <span data-ttu-id="b606b-146">I det här fallet ges en rad inte innehåller ett värde för en kolumn, ett null-värde för den.</span><span class="sxs-lookup"><span data-stu-id="b606b-146">In this case, if a row does not contain a value for a column, a null value is provided for it.</span></span>
2. <span data-ttu-id="b606b-147">Om du inte anger strukturen för data med hjälp av den **struktur** egenskap i datauppsättningsdefinitionen Data Factory härleder schemat med hjälp av den första raden i data.</span><span class="sxs-lookup"><span data-stu-id="b606b-147">If you don't specify the structure of data by using the **structure** property in the dataset definition, Data Factory infers the schema by using the first row in the data.</span></span> <span data-ttu-id="b606b-148">Om den första raden inte innehåller fullständig schemat missas i det här fallet vissa kolumner i resultatet av kopieringsåtgärden.</span><span class="sxs-lookup"><span data-stu-id="b606b-148">In this case, if the first row does not contain the full schema, some columns are missed in the result of copy operation.</span></span>

<span data-ttu-id="b606b-149">Därför för schemafria datakällor, det bästa sättet är att ange hur dina data med hjälp av den **struktur** egenskapen.</span><span class="sxs-lookup"><span data-stu-id="b606b-149">Therefore, for schema-free data sources, the best practice is to specify the structure of data using the **structure** property.</span></span>

## <a name="copy-activity-properties"></a><span data-ttu-id="b606b-150">Kopiera egenskaper för aktivitet</span><span class="sxs-lookup"><span data-stu-id="b606b-150">Copy activity properties</span></span>
<span data-ttu-id="b606b-151">En fullständig lista över avsnitt & egenskaper som är tillgängliga för att definiera aktiviteter finns i [skapar Pipelines](data-factory-create-pipelines.md) artikel.</span><span class="sxs-lookup"><span data-stu-id="b606b-151">For a full list of sections & properties available for defining activities, see the [Creating Pipelines](data-factory-create-pipelines.md) article.</span></span> <span data-ttu-id="b606b-152">Egenskaper som namn, beskrivning, indata och utdata-datauppsättningar och -principer är tillgängliga för alla typer av aktiviteter.</span><span class="sxs-lookup"><span data-stu-id="b606b-152">Properties such as name, description, input and output datasets, and policies are available for all types of activities.</span></span>

<span data-ttu-id="b606b-153">Egenskaper som är tillgängliga i avsnittet typeProperties i aktiviteten varierar å andra sidan beroende på varje aktivitetstyp.</span><span class="sxs-lookup"><span data-stu-id="b606b-153">Properties available in the typeProperties section of the activity on the other hand vary with each activity type.</span></span> <span data-ttu-id="b606b-154">För Kopieringsaktivitet kan variera de beroende på vilka typer av datakällor och sänkor.</span><span class="sxs-lookup"><span data-stu-id="b606b-154">For Copy activity, they vary depending on the types of sources and sinks.</span></span>

<span data-ttu-id="b606b-155">**AzureTableSource** stöder följande egenskaper i typeProperties avsnitt:</span><span class="sxs-lookup"><span data-stu-id="b606b-155">**AzureTableSource** supports the following properties in typeProperties section:</span></span>

| <span data-ttu-id="b606b-156">Egenskap</span><span class="sxs-lookup"><span data-stu-id="b606b-156">Property</span></span> | <span data-ttu-id="b606b-157">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="b606b-157">Description</span></span> | <span data-ttu-id="b606b-158">Tillåtna värden</span><span class="sxs-lookup"><span data-stu-id="b606b-158">Allowed values</span></span> | <span data-ttu-id="b606b-159">Krävs</span><span class="sxs-lookup"><span data-stu-id="b606b-159">Required</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="b606b-160">azureTableSourceQuery</span><span class="sxs-lookup"><span data-stu-id="b606b-160">azureTableSourceQuery</span></span> |<span data-ttu-id="b606b-161">Använd anpassad fråga för att läsa data.</span><span class="sxs-lookup"><span data-stu-id="b606b-161">Use the custom query to read data.</span></span> |<span data-ttu-id="b606b-162">Azure-tabellen frågesträngen.</span><span class="sxs-lookup"><span data-stu-id="b606b-162">Azure table query string.</span></span> <span data-ttu-id="b606b-163">Se exemplen i nästa avsnitt.</span><span class="sxs-lookup"><span data-stu-id="b606b-163">See examples in the next section.</span></span> |<span data-ttu-id="b606b-164">Nej.</span><span class="sxs-lookup"><span data-stu-id="b606b-164">No.</span></span> <span data-ttu-id="b606b-165">När ett tabellnamn har angetts utan ett azureTableSourceQuery, kopieras alla poster från tabellen till målet.</span><span class="sxs-lookup"><span data-stu-id="b606b-165">When a tableName is specified without an azureTableSourceQuery, all records from the table are copied to the destination.</span></span> <span data-ttu-id="b606b-166">Om en azureTableSourceQuery också anges kopieras poster från tabellen som uppfyller frågan till målet.</span><span class="sxs-lookup"><span data-stu-id="b606b-166">If an azureTableSourceQuery is also specified, records from the table that satisfies the query are copied to the destination.</span></span> |
| <span data-ttu-id="b606b-167">azureTableSourceIgnoreTableNotFound</span><span class="sxs-lookup"><span data-stu-id="b606b-167">azureTableSourceIgnoreTableNotFound</span></span> |<span data-ttu-id="b606b-168">Ange om swallow undantag av tabellen inte finns.</span><span class="sxs-lookup"><span data-stu-id="b606b-168">Indicate whether swallow the exception of table not exist.</span></span> |<span data-ttu-id="b606b-169">SANT</span><span class="sxs-lookup"><span data-stu-id="b606b-169">TRUE</span></span><br/><span data-ttu-id="b606b-170">FALSKT</span><span class="sxs-lookup"><span data-stu-id="b606b-170">FALSE</span></span> |<span data-ttu-id="b606b-171">Nej</span><span class="sxs-lookup"><span data-stu-id="b606b-171">No</span></span> |

### <a name="azuretablesourcequery-examples"></a><span data-ttu-id="b606b-172">azureTableSourceQuery-exempel</span><span class="sxs-lookup"><span data-stu-id="b606b-172">azureTableSourceQuery examples</span></span>
<span data-ttu-id="b606b-173">Om Azure Table kolumnen är av strängtypen:</span><span class="sxs-lookup"><span data-stu-id="b606b-173">If Azure Table column is of string type:</span></span>

```JSON
azureTableSourceQuery": "$$Text.Format('PartitionKey ge \\'{0:yyyyMMddHH00_0000}\\' and PartitionKey le \\'{0:yyyyMMddHH00_9999}\\'', SliceStart)"
```

<span data-ttu-id="b606b-174">Om Azure Table kolumnen är av typen datetime:</span><span class="sxs-lookup"><span data-stu-id="b606b-174">If Azure Table column is of datetime type:</span></span>

```JSON
"azureTableSourceQuery": "$$Text.Format('DeploymentEndTime gt datetime\\'{0:yyyy-MM-ddTHH:mm:ssZ}\\' and DeploymentEndTime le datetime\\'{1:yyyy-MM-ddTHH:mm:ssZ}\\'', SliceStart, SliceEnd)"
```

<span data-ttu-id="b606b-175">**AzureTableSink** stöder följande egenskaper i typeProperties avsnitt:</span><span class="sxs-lookup"><span data-stu-id="b606b-175">**AzureTableSink** supports the following properties in typeProperties section:</span></span>

| <span data-ttu-id="b606b-176">Egenskap</span><span class="sxs-lookup"><span data-stu-id="b606b-176">Property</span></span> | <span data-ttu-id="b606b-177">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="b606b-177">Description</span></span> | <span data-ttu-id="b606b-178">Tillåtna värden</span><span class="sxs-lookup"><span data-stu-id="b606b-178">Allowed values</span></span> | <span data-ttu-id="b606b-179">Krävs</span><span class="sxs-lookup"><span data-stu-id="b606b-179">Required</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="b606b-180">azureTableDefaultPartitionKeyValue</span><span class="sxs-lookup"><span data-stu-id="b606b-180">azureTableDefaultPartitionKeyValue</span></span> |<span data-ttu-id="b606b-181">Standard partitionsnyckelvärde som kan användas av sink.</span><span class="sxs-lookup"><span data-stu-id="b606b-181">Default partition key value that can be used by the sink.</span></span> |<span data-ttu-id="b606b-182">Ett strängvärde.</span><span class="sxs-lookup"><span data-stu-id="b606b-182">A string value.</span></span> |<span data-ttu-id="b606b-183">Nej</span><span class="sxs-lookup"><span data-stu-id="b606b-183">No</span></span> |
| <span data-ttu-id="b606b-184">azureTablePartitionKeyName</span><span class="sxs-lookup"><span data-stu-id="b606b-184">azureTablePartitionKeyName</span></span> |<span data-ttu-id="b606b-185">Ange namnet på den kolumn som används som partitionsnycklar.</span><span class="sxs-lookup"><span data-stu-id="b606b-185">Specify name of the column whose values are used as partition keys.</span></span> <span data-ttu-id="b606b-186">Om inget anges används AzureTableDefaultPartitionKeyValue som partitionsnyckel.</span><span class="sxs-lookup"><span data-stu-id="b606b-186">If not specified, AzureTableDefaultPartitionKeyValue is used as the partition key.</span></span> |<span data-ttu-id="b606b-187">Ett kolumnnamn.</span><span class="sxs-lookup"><span data-stu-id="b606b-187">A column name.</span></span> |<span data-ttu-id="b606b-188">Nej</span><span class="sxs-lookup"><span data-stu-id="b606b-188">No</span></span> |
| <span data-ttu-id="b606b-189">azureTableRowKeyName</span><span class="sxs-lookup"><span data-stu-id="b606b-189">azureTableRowKeyName</span></span> |<span data-ttu-id="b606b-190">Ange namnet på den kolumn vars kolumnvärdena används som radnyckel.</span><span class="sxs-lookup"><span data-stu-id="b606b-190">Specify name of the column whose column values are used as row key.</span></span> <span data-ttu-id="b606b-191">Om inget annat anges, kan du använda ett GUID för varje rad.</span><span class="sxs-lookup"><span data-stu-id="b606b-191">If not specified, use a GUID for each row.</span></span> |<span data-ttu-id="b606b-192">Ett kolumnnamn.</span><span class="sxs-lookup"><span data-stu-id="b606b-192">A column name.</span></span> |<span data-ttu-id="b606b-193">Nej</span><span class="sxs-lookup"><span data-stu-id="b606b-193">No</span></span> |
| <span data-ttu-id="b606b-194">azureTableInsertType</span><span class="sxs-lookup"><span data-stu-id="b606b-194">azureTableInsertType</span></span> |<span data-ttu-id="b606b-195">Läget att infoga data i Azure-tabellen.</span><span class="sxs-lookup"><span data-stu-id="b606b-195">The mode to insert data into Azure table.</span></span><br/><br/><span data-ttu-id="b606b-196">Den här egenskapen anger om befintliga rader i utdatatabellen med matchande partition och radnycklar få sina värden bytas ut eller samman.</span><span class="sxs-lookup"><span data-stu-id="b606b-196">This property controls whether existing rows in the output table with matching partition and row keys have their values replaced or merged.</span></span> <br/><br/><span data-ttu-id="b606b-197">Läs om hur dessa inställningar (dokument och Ersätt) fungerar i [Insert- eller Merge-entiteten](https://msdn.microsoft.com/library/azure/hh452241.aspx) och [infoga eller ersätta entiteten](https://msdn.microsoft.com/library/azure/hh452242.aspx) avsnitt.</span><span class="sxs-lookup"><span data-stu-id="b606b-197">To learn about how these settings (merge and replace) work, see [Insert or Merge Entity](https://msdn.microsoft.com/library/azure/hh452241.aspx) and [Insert or Replace Entity](https://msdn.microsoft.com/library/azure/hh452242.aspx) topics.</span></span> <br/><br> <span data-ttu-id="b606b-198">Den här inställningen gäller på radnivå inte tabellnivån, varken alternativet tar bort rader i utdatatabellen som inte finns i indata.</span><span class="sxs-lookup"><span data-stu-id="b606b-198">This setting applies at the row level, not the table level, and neither option deletes rows in the output table that do not exist in the input.</span></span> |<span data-ttu-id="b606b-199">sammanfoga (standard)</span><span class="sxs-lookup"><span data-stu-id="b606b-199">merge (default)</span></span><br/><span data-ttu-id="b606b-200">Ersätt</span><span class="sxs-lookup"><span data-stu-id="b606b-200">replace</span></span> |<span data-ttu-id="b606b-201">Nej</span><span class="sxs-lookup"><span data-stu-id="b606b-201">No</span></span> |
| <span data-ttu-id="b606b-202">writeBatchSize</span><span class="sxs-lookup"><span data-stu-id="b606b-202">writeBatchSize</span></span> |<span data-ttu-id="b606b-203">Infogar data i Azure-tabellen när writeBatchSize eller writeBatchTimeout namn.</span><span class="sxs-lookup"><span data-stu-id="b606b-203">Inserts data into the Azure table when the writeBatchSize or writeBatchTimeout is hit.</span></span> |<span data-ttu-id="b606b-204">Heltal (antalet rader)</span><span class="sxs-lookup"><span data-stu-id="b606b-204">Integer (number of rows)</span></span> |<span data-ttu-id="b606b-205">Nej (standard: 10000)</span><span class="sxs-lookup"><span data-stu-id="b606b-205">No (default: 10000)</span></span> |
| <span data-ttu-id="b606b-206">writeBatchTimeout</span><span class="sxs-lookup"><span data-stu-id="b606b-206">writeBatchTimeout</span></span> |<span data-ttu-id="b606b-207">Infogar data i Azure-tabellen när writeBatchSize eller writeBatchTimeout namn</span><span class="sxs-lookup"><span data-stu-id="b606b-207">Inserts data into the Azure table when the writeBatchSize or writeBatchTimeout is hit</span></span> |<span data-ttu-id="b606b-208">TimeSpan</span><span class="sxs-lookup"><span data-stu-id="b606b-208">timespan</span></span><br/><br/><span data-ttu-id="b606b-209">Exempel ”: 00: 20:00” (20 minuter)</span><span class="sxs-lookup"><span data-stu-id="b606b-209">Example: “00:20:00” (20 minutes)</span></span> |<span data-ttu-id="b606b-210">Nej (för lagring klienten standardtimeout standardvärdet 90 sek)</span><span class="sxs-lookup"><span data-stu-id="b606b-210">No (Default to storage client default timeout value 90 sec)</span></span> |

### <a name="azuretablepartitionkeyname"></a><span data-ttu-id="b606b-211">azureTablePartitionKeyName</span><span class="sxs-lookup"><span data-stu-id="b606b-211">azureTablePartitionKeyName</span></span>
<span data-ttu-id="b606b-212">Mappa en källkolumn till en målkolumn med översättare JSON-egenskapen innan du kan använda målkolumnen som azureTablePartitionKeyName.</span><span class="sxs-lookup"><span data-stu-id="b606b-212">Map a source column to a destination column using the translator JSON property before you can use the destination column as the azureTablePartitionKeyName.</span></span>

<span data-ttu-id="b606b-213">I följande exempel källkolumnen DivisionID mappas till målkolumnen: DivisionID.</span><span class="sxs-lookup"><span data-stu-id="b606b-213">In the following example, source column DivisionID is mapped to the destination column: DivisionID.</span></span>  

```JSON
"translator": {
    "type": "TabularTranslator",
    "columnMappings": "DivisionID: DivisionID, FirstName: FirstName, LastName: LastName"
}
```
<span data-ttu-id="b606b-214">DivisionID har angetts som partitionsnyckel.</span><span class="sxs-lookup"><span data-stu-id="b606b-214">The DivisionID is specified as the partition key.</span></span>

```JSON
"sink": {
    "type": "AzureTableSink",
    "azureTablePartitionKeyName": "DivisionID",
    "writeBatchSize": 100,
    "writeBatchTimeout": "01:00:00"
}
```
## <a name="json-examples"></a><span data-ttu-id="b606b-215">JSON-exempel</span><span class="sxs-lookup"><span data-stu-id="b606b-215">JSON examples</span></span>
<span data-ttu-id="b606b-216">Följande exempel ger exempel JSON definitioner som du kan använda för att skapa en pipeline med hjälp av [Azure-portalen](data-factory-copy-activity-tutorial-using-azure-portal.md) eller [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md) eller [Azure PowerShell](data-factory-copy-activity-tutorial-using-powershell.md).</span><span class="sxs-lookup"><span data-stu-id="b606b-216">The following examples provide sample JSON definitions that you can use to create a pipeline by using [Azure portal](data-factory-copy-activity-tutorial-using-azure-portal.md) or [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md) or [Azure PowerShell](data-factory-copy-activity-tutorial-using-powershell.md).</span></span> <span data-ttu-id="b606b-217">De visar hur du kopierar data till och från Azure Table Storage och Azure Blob-databas.</span><span class="sxs-lookup"><span data-stu-id="b606b-217">They show how to copy data to and from Azure Table Storage and Azure Blob Database.</span></span> <span data-ttu-id="b606b-218">Dock datan kan kopieras **direkt** från någon av källorna till någon av de stöds egenskaperna.</span><span class="sxs-lookup"><span data-stu-id="b606b-218">However, data can be copied **directly** from any of the sources to any of the supported sinks.</span></span> <span data-ttu-id="b606b-219">Mer information finns i avsnittet ”stöds datalager och format” i [flytta data med hjälp av Kopieringsaktiviteten](data-factory-data-movement-activities.md).</span><span class="sxs-lookup"><span data-stu-id="b606b-219">For more information, see the section "Supported data stores and formats" in [Move data by using Copy Activity](data-factory-data-movement-activities.md).</span></span>

## <a name="example-copy-data-from-azure-table-to-azure-blob"></a><span data-ttu-id="b606b-220">Exempel: Kopiera data från Azure Table till Azure-Blob</span><span class="sxs-lookup"><span data-stu-id="b606b-220">Example: Copy data from Azure Table to Azure Blob</span></span>
<span data-ttu-id="b606b-221">I följande exempel visas:</span><span class="sxs-lookup"><span data-stu-id="b606b-221">The following sample shows:</span></span>

1. <span data-ttu-id="b606b-222">En länkad tjänst av typen [AzureStorage](data-factory-azure-blob-connector.md#linked-service-properties) (används för blob & tabell).</span><span class="sxs-lookup"><span data-stu-id="b606b-222">A linked service of type [AzureStorage](data-factory-azure-blob-connector.md#linked-service-properties) (used for both table & blob).</span></span>
2. <span data-ttu-id="b606b-223">Indata [dataset](data-factory-create-datasets.md) av typen [AzureTable](#dataset-properties).</span><span class="sxs-lookup"><span data-stu-id="b606b-223">An input [dataset](data-factory-create-datasets.md) of type [AzureTable](#dataset-properties).</span></span>
3. <span data-ttu-id="b606b-224">Utdata [dataset](data-factory-create-datasets.md) av typen [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties).</span><span class="sxs-lookup"><span data-stu-id="b606b-224">An output [dataset](data-factory-create-datasets.md) of type [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties).</span></span>
4. <span data-ttu-id="b606b-225">Den [pipeline](data-factory-create-pipelines.md) med kopieringsaktiviteten som använder [AzureTableSource](#activity-properties) och [BlobSink](data-factory-azure-blob-connector.md#copy-activity-properties).</span><span class="sxs-lookup"><span data-stu-id="b606b-225">The [pipeline](data-factory-create-pipelines.md) with Copy activity that uses [AzureTableSource](#activity-properties) and [BlobSink](data-factory-azure-blob-connector.md#copy-activity-properties).</span></span>

<span data-ttu-id="b606b-226">Exemplet kopierar data som hör till standardpartition i en Azure-tabell till en blobb varje timme.</span><span class="sxs-lookup"><span data-stu-id="b606b-226">The sample copies data belonging to the default partition in an Azure Table to a blob every hour.</span></span> <span data-ttu-id="b606b-227">JSON-egenskaper som används i exemplen beskrivs i exemplen i följande avsnitt.</span><span class="sxs-lookup"><span data-stu-id="b606b-227">The JSON properties used in these samples are described in sections following the samples.</span></span>

<span data-ttu-id="b606b-228">**Länkad Azure storage-tjänst:**</span><span class="sxs-lookup"><span data-stu-id="b606b-228">**Azure storage linked service:**</span></span>

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
<span data-ttu-id="b606b-229">Azure Data Factory stöder två typer av länkad Azure Storage services: **AzureStorage** och **AzureStorageSas**.</span><span class="sxs-lookup"><span data-stu-id="b606b-229">Azure Data Factory supports two types of Azure Storage linked services: **AzureStorage** and **AzureStorageSas**.</span></span> <span data-ttu-id="b606b-230">För den första, anger du den anslutningssträng som innehåller nyckeln konto och du ska ange Uri för delad åtkomst signatur (SAS) för det senare.</span><span class="sxs-lookup"><span data-stu-id="b606b-230">For the first one, you specify the connection string that includes the account key and for the later one, you specify the Shared Access Signature (SAS) Uri.</span></span> <span data-ttu-id="b606b-231">Se [länkade tjänster](#linked-service-properties) information.</span><span class="sxs-lookup"><span data-stu-id="b606b-231">See [Linked Services](#linked-service-properties) section for details.</span></span>  

<span data-ttu-id="b606b-232">**Azure Table inkommande datamängd:**</span><span class="sxs-lookup"><span data-stu-id="b606b-232">**Azure Table input dataset:**</span></span>

<span data-ttu-id="b606b-233">Exemplet förutsätter att du har skapat en tabell ”mytable” som prefix i Azure Table.</span><span class="sxs-lookup"><span data-stu-id="b606b-233">The sample assumes you have created a table “MyTable” in Azure Table.</span></span>

<span data-ttu-id="b606b-234">Inställningen ”externa”: ”true” informerar Data Factory-tjänsten att datamängden är extern till data factory och inte tillverkas av en aktivitet i datafabriken.</span><span class="sxs-lookup"><span data-stu-id="b606b-234">Setting “external”: ”true” informs the Data Factory service that the dataset is external to the data factory and is not produced by an activity in the data factory.</span></span>

```JSON
{
  "name": "AzureTableInput",
  "properties": {
    "type": "AzureTable",
    "linkedServiceName": "StorageLinkedService",
    "typeProperties": {
      "tableName": "MyTable"
    },
    "external": true,
    "availability": {
      "frequency": "Hour",
      "interval": 1
    },
    "policy": {
      "externalData": {
        "retryInterval": "00:01:00",
        "retryTimeout": "00:10:00",
        "maximumRetry": 3
      }
    }
  }
}
```

<span data-ttu-id="b606b-235">**Azure Blob utdatauppsättningen:**</span><span class="sxs-lookup"><span data-stu-id="b606b-235">**Azure Blob output dataset:**</span></span>

<span data-ttu-id="b606b-236">Data skrivs till en ny blob varje timme (frekvens: timme, intervall: 1).</span><span class="sxs-lookup"><span data-stu-id="b606b-236">Data is written to a new blob every hour (frequency: hour, interval: 1).</span></span> <span data-ttu-id="b606b-237">Sökvägen till mappen för blobben utvärderas dynamiskt baserat på starttiden för den sektor som bearbetas.</span><span class="sxs-lookup"><span data-stu-id="b606b-237">The folder path for the blob is dynamically evaluated based on the start time of the slice that is being processed.</span></span> <span data-ttu-id="b606b-238">Mappsökvägen använder år, månad, dag och timmar delar av starttiden.</span><span class="sxs-lookup"><span data-stu-id="b606b-238">The folder path uses year, month, day, and hours parts of the start time.</span></span>

```JSON
{
  "name": "AzureBlobOutput",
  "properties": {
    "type": "AzureBlob",
    "linkedServiceName": "StorageLinkedService",
    "typeProperties": {
      "folderPath": "mycontainer/myfolder/yearno={Year}/monthno={Month}/dayno={Day}/hourno={Hour}",
      "partitionedBy": [
        {
          "name": "Year",
          "value": {
            "type": "DateTime",
            "date": "SliceStart",
            "format": "yyyy"
          }
        },
        {
          "name": "Month",
          "value": {
            "type": "DateTime",
            "date": "SliceStart",
            "format": "MM"
          }
        },
        {
          "name": "Day",
          "value": {
            "type": "DateTime",
            "date": "SliceStart",
            "format": "dd"
          }
        },
        {
          "name": "Hour",
          "value": {
            "type": "DateTime",
            "date": "SliceStart",
            "format": "HH"
          }
        }
      ],
      "format": {
        "type": "TextFormat",
        "columnDelimiter": "\t",
        "rowDelimiter": "\n"
      }
    },
    "availability": {
      "frequency": "Hour",
      "interval": 1
    }
  }
}
```

<span data-ttu-id="b606b-239">**Kopiera aktivitet i en pipeline med AzureTableSource och BlobSink:**</span><span class="sxs-lookup"><span data-stu-id="b606b-239">**Copy activity in a pipeline with AzureTableSource and BlobSink:**</span></span>

<span data-ttu-id="b606b-240">Pipelinen innehåller en kopia-aktivitet som är konfigurerad för att använda indata och utdata-datauppsättningar och är schemalagd att köras varje timme.</span><span class="sxs-lookup"><span data-stu-id="b606b-240">The pipeline contains a Copy Activity that is configured to use the input and output datasets and is scheduled to run every hour.</span></span> <span data-ttu-id="b606b-241">I pipeline-JSON-definitionen av **källa** är inställd på **AzureTableSource** och **sink** är inställd på **BlobSink**.</span><span class="sxs-lookup"><span data-stu-id="b606b-241">In the pipeline JSON definition, the **source** type is set to **AzureTableSource** and **sink** type is set to **BlobSink**.</span></span> <span data-ttu-id="b606b-242">SQL-frågan som anges med **AzureTableSourceQuery** egenskapen väljer vilka data från standardpartition varje timme för att kopiera.</span><span class="sxs-lookup"><span data-stu-id="b606b-242">The SQL query specified with **AzureTableSourceQuery** property selects the data from the default partition every hour to copy.</span></span>

```JSON
{  
    "name":"SamplePipeline",
    "properties":{  
        "start":"2014-06-01T18:00:00",
        "end":"2014-06-01T19:00:00",
        "description":"pipeline for copy activity",
        "activities":[  
            {
                "name": "AzureTabletoBlob",
                "description": "copy activity",
                "type": "Copy",
                "inputs": [
                      {
                        "name": "AzureTableInput"
                    }
                ],
                "outputs": [
                      {
                            "name": "AzureBlobOutput"
                      }
                ],
                "typeProperties": {
                      "source": {
                        "type": "AzureTableSource",
                        "AzureTableSourceQuery": "PartitionKey eq 'DefaultPartitionKey'"
                      },
                      "sink": {
                        "type": "BlobSink"
                      }
                },
                "scheduler": {
                      "frequency": "Hour",
                      "interval": 1
                },                
                "policy": {
                      "concurrency": 1,
                      "executionPriorityOrder": "OldestFirst",
                      "retry": 0,
                      "timeout": "01:00:00"
                }
            }
         ]    
    }
}
```

## <a name="example-copy-data-from-azure-blob-to-azure-table"></a><span data-ttu-id="b606b-243">Exempel: Kopiera data från Azure Blob till Azure Table</span><span class="sxs-lookup"><span data-stu-id="b606b-243">Example: Copy data from Azure Blob to Azure Table</span></span>
<span data-ttu-id="b606b-244">I följande exempel visas:</span><span class="sxs-lookup"><span data-stu-id="b606b-244">The following sample shows:</span></span>

1. <span data-ttu-id="b606b-245">En länkad tjänst av typen [AzureStorage](data-factory-azure-blob-connector.md#linked-service-properties) (används för blob & tabell)</span><span class="sxs-lookup"><span data-stu-id="b606b-245">A linked service of type [AzureStorage](data-factory-azure-blob-connector.md#linked-service-properties) (used for both table & blob)</span></span>
2. <span data-ttu-id="b606b-246">Indata [dataset](data-factory-create-datasets.md) av typen [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties).</span><span class="sxs-lookup"><span data-stu-id="b606b-246">An input [dataset](data-factory-create-datasets.md) of type [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties).</span></span>
3. <span data-ttu-id="b606b-247">Utdata [dataset](data-factory-create-datasets.md) av typen [AzureTable](#dataset-properties).</span><span class="sxs-lookup"><span data-stu-id="b606b-247">An output [dataset](data-factory-create-datasets.md) of type [AzureTable](#dataset-properties).</span></span>
4. <span data-ttu-id="b606b-248">Den [pipeline](data-factory-create-pipelines.md) med kopieringsaktiviteten som använder [BlobSource](data-factory-azure-blob-connector.md#copy-activity-properties) och [AzureTableSink](#copy-activity-properties).</span><span class="sxs-lookup"><span data-stu-id="b606b-248">The [pipeline](data-factory-create-pipelines.md) with Copy activity that uses [BlobSource](data-factory-azure-blob-connector.md#copy-activity-properties) and [AzureTableSink](#copy-activity-properties).</span></span>

<span data-ttu-id="b606b-249">Exempel kopiorna tidsserier data från ett Azure blob till en Azure tabellen varje timme.</span><span class="sxs-lookup"><span data-stu-id="b606b-249">The sample copies time-series data from an Azure blob to an Azure table hourly.</span></span> <span data-ttu-id="b606b-250">JSON-egenskaper som används i exemplen beskrivs i exemplen i följande avsnitt.</span><span class="sxs-lookup"><span data-stu-id="b606b-250">The JSON properties used in these samples are described in sections following the samples.</span></span>

<span data-ttu-id="b606b-251">**Länkad Azure storage (för både Azure Table & Blob)-tjänst:**</span><span class="sxs-lookup"><span data-stu-id="b606b-251">**Azure storage (for both Azure Table & Blob) linked service:**</span></span>

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

<span data-ttu-id="b606b-252">Azure Data Factory stöder två typer av länkad Azure Storage services: **AzureStorage** och **AzureStorageSas**.</span><span class="sxs-lookup"><span data-stu-id="b606b-252">Azure Data Factory supports two types of Azure Storage linked services: **AzureStorage** and **AzureStorageSas**.</span></span> <span data-ttu-id="b606b-253">För den första, anger du den anslutningssträng som innehåller nyckeln konto och du ska ange Uri för delad åtkomst signatur (SAS) för det senare.</span><span class="sxs-lookup"><span data-stu-id="b606b-253">For the first one, you specify the connection string that includes the account key and for the later one, you specify the Shared Access Signature (SAS) Uri.</span></span> <span data-ttu-id="b606b-254">Se [länkade tjänster](#linked-service-properties) information.</span><span class="sxs-lookup"><span data-stu-id="b606b-254">See [Linked Services](#linked-service-properties) section for details.</span></span>

<span data-ttu-id="b606b-255">**Azure Blob inkommande datamängd:**</span><span class="sxs-lookup"><span data-stu-id="b606b-255">**Azure Blob input dataset:**</span></span>

<span data-ttu-id="b606b-256">Data hämtas från en ny blob varje timme (frekvens: timme, intervall: 1).</span><span class="sxs-lookup"><span data-stu-id="b606b-256">Data is picked up from a new blob every hour (frequency: hour, interval: 1).</span></span> <span data-ttu-id="b606b-257">Mappen sökvägen och filnamnet för blobben utvärderas dynamiskt baserat på starttiden för den sektor som bearbetas.</span><span class="sxs-lookup"><span data-stu-id="b606b-257">The folder path and file name for the blob are dynamically evaluated based on the start time of the slice that is being processed.</span></span> <span data-ttu-id="b606b-258">Mappsökvägen använder år, månad och som en dagdel av starttiden filnamn används i timmen som en del av starttiden.</span><span class="sxs-lookup"><span data-stu-id="b606b-258">The folder path uses year, month, and day part of the start time and file name uses the hour part of the start time.</span></span> <span data-ttu-id="b606b-259">”externa”: ”true” inställningen informerar Data Factory-tjänsten att datamängden är extern till data factory och inte tillverkas av en aktivitet i datafabriken.</span><span class="sxs-lookup"><span data-stu-id="b606b-259">“external”: “true” setting informs the Data Factory service that the dataset is external to the data factory and is not produced by an activity in the data factory.</span></span>

```JSON
{
  "name": "AzureBlobInput",
  "properties": {
    "type": "AzureBlob",
    "linkedServiceName": "StorageLinkedService",
    "typeProperties": {
      "folderPath": "mycontainer/myfolder/yearno={Year}/monthno={Month}/dayno={Day}",
      "fileName": "{Hour}.csv",
      "partitionedBy": [
        {
          "name": "Year",
          "value": {
            "type": "DateTime",
            "date": "SliceStart",
            "format": "yyyy"
          }
        },
        {
          "name": "Month",
          "value": {
            "type": "DateTime",
            "date": "SliceStart",
            "format": "MM"
          }
        },
        {
          "name": "Day",
          "value": {
            "type": "DateTime",
            "date": "SliceStart",
            "format": "dd"
          }
        },
        {
          "name": "Hour",
          "value": {
            "type": "DateTime",
            "date": "SliceStart",
            "format": "HH"
          }
        }
      ],
      "format": {
        "type": "TextFormat",
        "columnDelimiter": ",",
        "rowDelimiter": "\n"
      }
    },
    "external": true,
    "availability": {
      "frequency": "Hour",
      "interval": 1
    },
    "policy": {
      "externalData": {
        "retryInterval": "00:01:00",
        "retryTimeout": "00:10:00",
        "maximumRetry": 3
      }
    }
  }
}
```

<span data-ttu-id="b606b-260">**Azure-tabellen utdatauppsättningen:**</span><span class="sxs-lookup"><span data-stu-id="b606b-260">**Azure Table output dataset:**</span></span>

<span data-ttu-id="b606b-261">Exemplet kopierar data till en tabell med namnet ”mytable” som prefix i Azure Table.</span><span class="sxs-lookup"><span data-stu-id="b606b-261">The sample copies data to a table named “MyTable” in Azure Table.</span></span> <span data-ttu-id="b606b-262">Skapa en Azure-tabellen med samma antal kolumner som du förväntar dig att Blob CSV-filen ska innehålla.</span><span class="sxs-lookup"><span data-stu-id="b606b-262">Create an Azure table with the same number of columns as you expect the Blob CSV file to contain.</span></span> <span data-ttu-id="b606b-263">Nya rader läggs till i tabell varje timme.</span><span class="sxs-lookup"><span data-stu-id="b606b-263">New rows are added to the table every hour.</span></span>

```JSON
{
  "name": "AzureTableOutput",
  "properties": {
    "type": "AzureTable",
    "linkedServiceName": "StorageLinkedService",
    "typeProperties": {
      "tableName": "MyOutputTable"
    },
    "availability": {
      "frequency": "Hour",
      "interval": 1
    }
  }
}
```

<span data-ttu-id="b606b-264">**Kopiera aktivitet i en pipeline med BlobSource och AzureTableSink:**</span><span class="sxs-lookup"><span data-stu-id="b606b-264">**Copy activity in a pipeline with BlobSource and AzureTableSink:**</span></span>

<span data-ttu-id="b606b-265">Pipelinen innehåller en kopia-aktivitet som är konfigurerad för att använda indata och utdata-datauppsättningar och är schemalagd att köras varje timme.</span><span class="sxs-lookup"><span data-stu-id="b606b-265">The pipeline contains a Copy Activity that is configured to use the input and output datasets and is scheduled to run every hour.</span></span> <span data-ttu-id="b606b-266">I pipeline-JSON-definitionen av **källa** är inställd på **BlobSource** och **sink** är inställd på **AzureTableSink**.</span><span class="sxs-lookup"><span data-stu-id="b606b-266">In the pipeline JSON definition, the **source** type is set to **BlobSource** and **sink** type is set to **AzureTableSink**.</span></span>

```JSON
{  
    "name":"SamplePipeline",
    "properties":{  
    "start":"2014-06-01T18:00:00",
    "end":"2014-06-01T19:00:00",
    "description":"pipeline with copy activity",
    "activities":[  
      {
        "name": "AzureBlobtoTable",
        "description": "Copy Activity",
        "type": "Copy",
        "inputs": [
          {
            "name": "AzureBlobInput"
          }
        ],
        "outputs": [
          {
            "name": "AzureTableOutput"
          }
        ],
        "typeProperties": {
          "source": {
            "type": "BlobSource"
          },
          "sink": {
            "type": "AzureTableSink",
            "writeBatchSize": 100,
            "writeBatchTimeout": "01:00:00"
          }
        },
        "scheduler": {
          "frequency": "Hour",
          "interval": 1
        },                        
        "policy": {
          "concurrency": 1,
          "executionPriorityOrder": "OldestFirst",
          "retry": 0,
          "timeout": "01:00:00"
        }
      }
      ]
   }
}
```
## <a name="type-mapping-for-azure-table"></a><span data-ttu-id="b606b-267">Mappning för Azure-tabellen</span><span class="sxs-lookup"><span data-stu-id="b606b-267">Type Mapping for Azure Table</span></span>
<span data-ttu-id="b606b-268">Som anges i den [data movement aktiviteter](data-factory-data-movement-activities.md) artikeln kopieringsaktiviteten utför automatisk konverteringar från källtyper att registrera typer med följande metod i två steg.</span><span class="sxs-lookup"><span data-stu-id="b606b-268">As mentioned in the [data movement activities](data-factory-data-movement-activities.md) article, Copy activity performs automatic type conversions from source types to sink types with the following two-step approach.</span></span>

1. <span data-ttu-id="b606b-269">Konvertera från interna källtyper till .NET-typ</span><span class="sxs-lookup"><span data-stu-id="b606b-269">Convert from native source types to .NET type</span></span>
2. <span data-ttu-id="b606b-270">Konvertera från .NET-typ till interna mottagare typ.</span><span class="sxs-lookup"><span data-stu-id="b606b-270">Convert from .NET type to native sink type</span></span>

<span data-ttu-id="b606b-271">När du flyttar data till och från Azure Table följande [mappningar som definierats av Azure Table-tjänsten](https://msdn.microsoft.com/library/azure/dd179338.aspx) som används från Azure Table OData-typer till .NET-typ och vice versa.</span><span class="sxs-lookup"><span data-stu-id="b606b-271">When moving data to & from Azure Table, the following [mappings defined by Azure Table service](https://msdn.microsoft.com/library/azure/dd179338.aspx) are used from Azure Table OData types to .NET type and vice versa.</span></span>

| <span data-ttu-id="b606b-272">OData-datatyp</span><span class="sxs-lookup"><span data-stu-id="b606b-272">OData Data Type</span></span> | <span data-ttu-id="b606b-273">.NET-typ</span><span class="sxs-lookup"><span data-stu-id="b606b-273">.NET Type</span></span> | <span data-ttu-id="b606b-274">Information</span><span class="sxs-lookup"><span data-stu-id="b606b-274">Details</span></span> |
| --- | --- | --- |
| <span data-ttu-id="b606b-275">Edm.Binary</span><span class="sxs-lookup"><span data-stu-id="b606b-275">Edm.Binary</span></span> |<span data-ttu-id="b606b-276">byte]</span><span class="sxs-lookup"><span data-stu-id="b606b-276">byte[]</span></span> |<span data-ttu-id="b606b-277">En matris med byte upp till 64 KB.</span><span class="sxs-lookup"><span data-stu-id="b606b-277">An array of bytes up to 64 KB.</span></span> |
| <span data-ttu-id="b606b-278">Edm.Boolean</span><span class="sxs-lookup"><span data-stu-id="b606b-278">Edm.Boolean</span></span> |<span data-ttu-id="b606b-279">bool</span><span class="sxs-lookup"><span data-stu-id="b606b-279">bool</span></span> |<span data-ttu-id="b606b-280">Ett booleskt värde.</span><span class="sxs-lookup"><span data-stu-id="b606b-280">A Boolean value.</span></span> |
| <span data-ttu-id="b606b-281">Edm.DateTime</span><span class="sxs-lookup"><span data-stu-id="b606b-281">Edm.DateTime</span></span> |<span data-ttu-id="b606b-282">Datum och tid</span><span class="sxs-lookup"><span data-stu-id="b606b-282">DateTime</span></span> |<span data-ttu-id="b606b-283">En 64-bitars värdet uttrycks som Coordinated Universal Time (UTC).</span><span class="sxs-lookup"><span data-stu-id="b606b-283">A 64-bit value expressed as Coordinated Universal Time (UTC).</span></span> <span data-ttu-id="b606b-284">Det intervall som stöds för den DateTime som börjar från midnatt, 1 januari, 1601 e. kr.</span><span class="sxs-lookup"><span data-stu-id="b606b-284">The supported DateTime range begins from 12:00 midnight, January 1, 1601 A.D.</span></span> <span data-ttu-id="b606b-285">(C.E.) UTC.</span><span class="sxs-lookup"><span data-stu-id="b606b-285">(C.E.), UTC.</span></span> <span data-ttu-id="b606b-286">Intervallet slutar vid den 31 December 9999.</span><span class="sxs-lookup"><span data-stu-id="b606b-286">The range ends at December 31, 9999.</span></span> |
| <span data-ttu-id="b606b-287">Edm.Double</span><span class="sxs-lookup"><span data-stu-id="b606b-287">Edm.Double</span></span> |<span data-ttu-id="b606b-288">dubbla</span><span class="sxs-lookup"><span data-stu-id="b606b-288">double</span></span> |<span data-ttu-id="b606b-289">En 64-bitars flytande punktvärdet.</span><span class="sxs-lookup"><span data-stu-id="b606b-289">A 64-bit floating point value.</span></span> |
| <span data-ttu-id="b606b-290">Edm.Guid</span><span class="sxs-lookup"><span data-stu-id="b606b-290">Edm.Guid</span></span> |<span data-ttu-id="b606b-291">GUID</span><span class="sxs-lookup"><span data-stu-id="b606b-291">Guid</span></span> |<span data-ttu-id="b606b-292">En 128-bitars globalt unik identifierare.</span><span class="sxs-lookup"><span data-stu-id="b606b-292">A 128-bit globally unique identifier.</span></span> |
| <span data-ttu-id="b606b-293">Edm.Int32</span><span class="sxs-lookup"><span data-stu-id="b606b-293">Edm.Int32</span></span> |<span data-ttu-id="b606b-294">Int32</span><span class="sxs-lookup"><span data-stu-id="b606b-294">Int32</span></span> |<span data-ttu-id="b606b-295">En 32-bitars heltal.</span><span class="sxs-lookup"><span data-stu-id="b606b-295">A 32-bit integer.</span></span> |
| <span data-ttu-id="b606b-296">Edm.Int64</span><span class="sxs-lookup"><span data-stu-id="b606b-296">Edm.Int64</span></span> |<span data-ttu-id="b606b-297">Int64</span><span class="sxs-lookup"><span data-stu-id="b606b-297">Int64</span></span> |<span data-ttu-id="b606b-298">En 64-bitars heltal.</span><span class="sxs-lookup"><span data-stu-id="b606b-298">A 64-bit integer.</span></span> |
| <span data-ttu-id="b606b-299">Edm.String</span><span class="sxs-lookup"><span data-stu-id="b606b-299">Edm.String</span></span> |<span data-ttu-id="b606b-300">Sträng</span><span class="sxs-lookup"><span data-stu-id="b606b-300">String</span></span> |<span data-ttu-id="b606b-301">Ett värde för UTF-16-kodad.</span><span class="sxs-lookup"><span data-stu-id="b606b-301">A UTF-16-encoded value.</span></span> <span data-ttu-id="b606b-302">Strängvärden kan vara upp till 64 KB.</span><span class="sxs-lookup"><span data-stu-id="b606b-302">String values may be up to 64 KB.</span></span> |

### <a name="type-conversion-sample"></a><span data-ttu-id="b606b-303">Exempel konvertering</span><span class="sxs-lookup"><span data-stu-id="b606b-303">Type Conversion Sample</span></span>
<span data-ttu-id="b606b-304">I följande exempel är för att kopiera data från en Azure-Blob till Azure Table med typkonverteringar.</span><span class="sxs-lookup"><span data-stu-id="b606b-304">The following sample is for copying data from an Azure Blob to Azure Table with type conversions.</span></span>

<span data-ttu-id="b606b-305">Anta att Blob-dataset i CSV-format och innehåller tre kolumner.</span><span class="sxs-lookup"><span data-stu-id="b606b-305">Suppose the Blob dataset is in CSV format and contains three columns.</span></span> <span data-ttu-id="b606b-306">En av dem är ett datetime-kolumn med en anpassad datetime-format med hjälp av förkortade franska namn för dag i veckan.</span><span class="sxs-lookup"><span data-stu-id="b606b-306">One of them is a datetime column with a custom datetime format using abbreviated French names for day of the week.</span></span>

<span data-ttu-id="b606b-307">Definiera datauppsättningen Blob källa på följande sätt tillsammans med typdefinitioner för kolumner.</span><span class="sxs-lookup"><span data-stu-id="b606b-307">Define the Blob Source dataset as follows along with type definitions for the columns.</span></span>

```JSON
{
    "name": " AzureBlobInput",
    "properties":
    {
         "structure":
          [
                { "name": "userid", "type": "Int64"},
                { "name": "name", "type": "String"},
                { "name": "lastlogindate", "type": "Datetime", "culture": "fr-fr", "format": "ddd-MM-YYYY"}
          ],
        "type": "AzureBlob",
        "linkedServiceName": "StorageLinkedService",
        "typeProperties": {
            "folderPath": "mycontainer/myfolder",
            "fileName":"myfile.csv",
            "format":
            {
                "type": "TextFormat",
                "columnDelimiter": ","
            }
        },
        "external": true,
        "availability":
        {
            "frequency": "Hour",
            "interval": 1,
        },
        "policy": {
            "externalData": {
                "retryInterval": "00:01:00",
                "retryTimeout": "00:10:00",
                "maximumRetry": 3
            }
        }
    }
}
```
<span data-ttu-id="b606b-308">Få typmappningen från Azure Table OData-typ till .NET-typ, definierar du tabellen i Azure-tabellen med följande schema.</span><span class="sxs-lookup"><span data-stu-id="b606b-308">Given the type mapping from Azure Table OData type to .NET type, you would define the table in Azure Table with the following schema.</span></span>

<span data-ttu-id="b606b-309">**Azure tabellschemat:**</span><span class="sxs-lookup"><span data-stu-id="b606b-309">**Azure Table schema:**</span></span>

| <span data-ttu-id="b606b-310">Kolumnnamn</span><span class="sxs-lookup"><span data-stu-id="b606b-310">Column name</span></span> | <span data-ttu-id="b606b-311">Typ</span><span class="sxs-lookup"><span data-stu-id="b606b-311">Type</span></span> |
| --- | --- |
| <span data-ttu-id="b606b-312">användar-ID</span><span class="sxs-lookup"><span data-stu-id="b606b-312">userid</span></span> |<span data-ttu-id="b606b-313">Edm.Int64</span><span class="sxs-lookup"><span data-stu-id="b606b-313">Edm.Int64</span></span> |
| <span data-ttu-id="b606b-314">namn</span><span class="sxs-lookup"><span data-stu-id="b606b-314">name</span></span> |<span data-ttu-id="b606b-315">Edm.String</span><span class="sxs-lookup"><span data-stu-id="b606b-315">Edm.String</span></span> |
| <span data-ttu-id="b606b-316">lastlogindate</span><span class="sxs-lookup"><span data-stu-id="b606b-316">lastlogindate</span></span> |<span data-ttu-id="b606b-317">Edm.DateTime</span><span class="sxs-lookup"><span data-stu-id="b606b-317">Edm.DateTime</span></span> |

<span data-ttu-id="b606b-318">Därefter definiera Azure Table-datauppsättningen.</span><span class="sxs-lookup"><span data-stu-id="b606b-318">Next, define the Azure Table dataset as follows.</span></span> <span data-ttu-id="b606b-319">Du behöver inte ange ”struktur” avsnittet med informationen eftersom informationen redan har angetts i den underliggande datalagringen.</span><span class="sxs-lookup"><span data-stu-id="b606b-319">You do not need to specify “structure” section with the type information since the type information is already specified in the underlying data store.</span></span>

```JSON
{
  "name": "AzureTableOutput",
  "properties": {
    "type": "AzureTable",
    "linkedServiceName": "StorageLinkedService",
    "typeProperties": {
      "tableName": "MyOutputTable"
    },
    "availability": {
      "frequency": "Hour",
      "interval": 1
    }
  }
}
```

<span data-ttu-id="b606b-320">I det här fallet Data Factory Skriv automatiskt konverteringar inklusive Datetime-fält med anpassade datetime-format med hjälp av ”fr-fr”-kulturen vid flytt av data från Blob till Azure Table.</span><span class="sxs-lookup"><span data-stu-id="b606b-320">In this case, Data Factory automatically does type conversions including the Datetime field with the custom datetime format using the "fr-fr" culture when moving data from Blob to Azure Table.</span></span>

> [!NOTE]
> <span data-ttu-id="b606b-321">Om du vill mappa kolumner från källan dataset till kolumner från sink dataset finns [mappa dataset kolumner i Azure Data Factory](data-factory-map-columns.md).</span><span class="sxs-lookup"><span data-stu-id="b606b-321">To map columns from source dataset to columns from sink dataset, see [Mapping dataset columns in Azure Data Factory](data-factory-map-columns.md).</span></span>

## <a name="performance-and-tuning"></a><span data-ttu-id="b606b-322">Prestanda och finjustering</span><span class="sxs-lookup"><span data-stu-id="b606b-322">Performance and Tuning</span></span>
<span data-ttu-id="b606b-323">Mer information om viktiga faktorer som påverkan prestanda för flytt av data (Kopieringsaktiviteten) i Azure Data Factory och olika sätt att optimera det finns [kopiera aktivitet prestanda och justera guiden](data-factory-copy-activity-performance.md).</span><span class="sxs-lookup"><span data-stu-id="b606b-323">To learn about key factors that impact performance of data movement (Copy Activity) in Azure Data Factory and various ways to optimize it, see [Copy Activity Performance & Tuning Guide](data-factory-copy-activity-performance.md).</span></span>
