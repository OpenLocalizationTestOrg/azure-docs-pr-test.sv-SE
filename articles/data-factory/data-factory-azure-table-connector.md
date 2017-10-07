---
title: "aaaMove data till/från Azure Table | Microsoft Docs"
description: "Lär dig hur toomove data till/från Azure Table Storage med hjälp av Azure Data Factory."
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
ms.openlocfilehash: 3dc3da6d88854674a9108b600534bc5d07575f15
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="move-data-tooand-from-azure-table-using-azure-data-factory"></a><span data-ttu-id="26df9-103">Flytta data tooand från Azure-tabellen med hjälp av Azure Data Factory</span><span class="sxs-lookup"><span data-stu-id="26df9-103">Move data tooand from Azure Table using Azure Data Factory</span></span>
<span data-ttu-id="26df9-104">Den här artikeln förklarar hur toouse hello Kopieringsaktiviteten i Azure Data Factory toomove data till och från Azure Table Storage.</span><span class="sxs-lookup"><span data-stu-id="26df9-104">This article explains how toouse hello Copy Activity in Azure Data Factory toomove data to/from Azure Table Storage.</span></span> <span data-ttu-id="26df9-105">Den bygger på hello [Data Movement aktiviteter](data-factory-data-movement-activities.md) artikel som ger en allmän översikt över dataflyttning hello kopieringsaktiviteten.</span><span class="sxs-lookup"><span data-stu-id="26df9-105">It builds on hello [Data Movement Activities](data-factory-data-movement-activities.md) article, which presents a general overview of data movement with hello copy activity.</span></span> 

<span data-ttu-id="26df9-106">Du kan kopiera data från en stöds källa data tooAzure tabellagring eller från Azure Table Storage tooany stöds sink data.</span><span class="sxs-lookup"><span data-stu-id="26df9-106">You can copy data from any supported source data store tooAzure Table Storage or from Azure Table Storage tooany supported sink data store.</span></span> <span data-ttu-id="26df9-107">En lista över datakällor som stöds som datakällor eller sänkor av hello kopieringsaktiviteten finns hello [stöds datalager](data-factory-data-movement-activities.md#supported-data-stores-and-formats) tabell.</span><span class="sxs-lookup"><span data-stu-id="26df9-107">For a list of data stores supported as sources or sinks by hello copy activity, see hello [Supported data stores](data-factory-data-movement-activities.md#supported-data-stores-and-formats) table.</span></span> 

## <a name="getting-started"></a><span data-ttu-id="26df9-108">Komma igång</span><span class="sxs-lookup"><span data-stu-id="26df9-108">Getting started</span></span>
<span data-ttu-id="26df9-109">Du kan skapa en pipeline med en kopia-aktivitet som flyttar data till och från en Azure Table Storage med hjälp av olika verktyg/API: er.</span><span class="sxs-lookup"><span data-stu-id="26df9-109">You can create a pipeline with a copy activity that moves data to/from an Azure Table Storage by using different tools/APIs.</span></span>

<span data-ttu-id="26df9-110">hello enklaste sättet toocreate en pipeline är toouse hello **guiden Kopiera**.</span><span class="sxs-lookup"><span data-stu-id="26df9-110">hello easiest way toocreate a pipeline is toouse hello **Copy Wizard**.</span></span> <span data-ttu-id="26df9-111">Se [Självstudier: skapa en pipeline med hjälp av guiden Kopiera](data-factory-copy-data-wizard-tutorial.md) för en snabb genomgång om hur du skapar en pipeline med hjälp av guiden för hello kopiera data.</span><span class="sxs-lookup"><span data-stu-id="26df9-111">See [Tutorial: Create a pipeline using Copy Wizard](data-factory-copy-data-wizard-tutorial.md) for a quick walkthrough on creating a pipeline using hello Copy data wizard.</span></span>

<span data-ttu-id="26df9-112">Du kan också använda följande verktyg toocreate en pipeline hello: **Azure-portalen**, **Visual Studio**, **Azure PowerShell**, **Azure Resource Manager-mall** , **.NET API**, och **REST API**.</span><span class="sxs-lookup"><span data-stu-id="26df9-112">You can also use hello following tools toocreate a pipeline: **Azure portal**, **Visual Studio**, **Azure PowerShell**, **Azure Resource Manager template**, **.NET API**, and **REST API**.</span></span> <span data-ttu-id="26df9-113">Se [kopiera aktivitet kursen](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) för stegvisa instruktioner toocreate en pipeline med en Kopieringsaktivitet.</span><span class="sxs-lookup"><span data-stu-id="26df9-113">See [Copy activity tutorial](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) for step-by-step instructions toocreate a pipeline with a copy activity.</span></span> 

<span data-ttu-id="26df9-114">Om du använder hello verktyg eller API: er kan utföra du hello följande steg toocreate en pipeline som flyttar data från en källdata lagra tooa sink-datalagret:</span><span class="sxs-lookup"><span data-stu-id="26df9-114">Whether you use hello tools or APIs, you perform hello following steps toocreate a pipeline that moves data from a source data store tooa sink data store:</span></span> 

1. <span data-ttu-id="26df9-115">Skapa **länkade tjänster** toolink indata och utdata lagrar tooyour data factory.</span><span class="sxs-lookup"><span data-stu-id="26df9-115">Create **linked services** toolink input and output data stores tooyour data factory.</span></span>
2. <span data-ttu-id="26df9-116">Skapa **datauppsättningar** toorepresent indata och utdata för hello kopieringsåtgärden.</span><span class="sxs-lookup"><span data-stu-id="26df9-116">Create **datasets** toorepresent input and output data for hello copy operation.</span></span> 
3. <span data-ttu-id="26df9-117">Skapa en **pipeline** med en kopia-aktivitet som tar en datamängd som indata och en dataset som utdata.</span><span class="sxs-lookup"><span data-stu-id="26df9-117">Create a **pipeline** with a copy activity that takes a dataset as an input and a dataset as an output.</span></span> 

<span data-ttu-id="26df9-118">När du använder guiden hello skapas automatiskt JSON definitioner för dessa Data Factory-enheter (länkade tjänster, datauppsättningar och hello pipeline) för dig.</span><span class="sxs-lookup"><span data-stu-id="26df9-118">When you use hello wizard, JSON definitions for these Data Factory entities (linked services, datasets, and hello pipeline) are automatically created for you.</span></span> <span data-ttu-id="26df9-119">När du använder Verktyg/API: er (utom .NET API), kan du definiera dessa Data Factory-enheter med hjälp av hello JSON-format.</span><span class="sxs-lookup"><span data-stu-id="26df9-119">When you use tools/APIs (except .NET API), you define these Data Factory entities by using hello JSON format.</span></span>  <span data-ttu-id="26df9-120">Exempel med JSON-definitioner för Data Factory-entiteter som har använt toocopy data till/från en Azure-tabellagring finns [JSON-exempel](#json-examples) i den här artikeln.</span><span class="sxs-lookup"><span data-stu-id="26df9-120">For samples with JSON definitions for Data Factory entities that are used toocopy data to/from an Azure Table Storage, see [JSON examples](#json-examples) section of this article.</span></span> 

<span data-ttu-id="26df9-121">hello följande avsnitt innehåller information om JSON-egenskaper används toodefine Data Factory entiteter specifika tooAzure Table Storage:</span><span class="sxs-lookup"><span data-stu-id="26df9-121">hello following sections provide details about JSON properties that are used toodefine Data Factory entities specific tooAzure Table Storage:</span></span> 

## <a name="linked-service-properties"></a><span data-ttu-id="26df9-122">Länkad tjänstegenskaper</span><span class="sxs-lookup"><span data-stu-id="26df9-122">Linked service properties</span></span>
<span data-ttu-id="26df9-123">Det finns två typer av länkade tjänster kan du använda toolink ett Azure blob storage tooan Azure data factory.</span><span class="sxs-lookup"><span data-stu-id="26df9-123">There are two types of linked services you can use toolink an Azure blob storage tooan Azure data factory.</span></span> <span data-ttu-id="26df9-124">De är: **AzureStorage** länkade tjänsten och **AzureStorageSas** länkade tjänsten.</span><span class="sxs-lookup"><span data-stu-id="26df9-124">They are: **AzureStorage** linked service and **AzureStorageSas** linked service.</span></span> <span data-ttu-id="26df9-125">hello länkad Azure Storage-tjänst ger hello data factory med global åtkomst toohello Azure Storage.</span><span class="sxs-lookup"><span data-stu-id="26df9-125">hello Azure Storage linked service provides hello data factory with global access toohello Azure Storage.</span></span> <span data-ttu-id="26df9-126">Medan hello Azure Storage SAS (signatur för delad åtkomst) länkad ger tjänst hello data factory med begränsad/Tidsbundna åtkomst toohello Azure Storage.</span><span class="sxs-lookup"><span data-stu-id="26df9-126">Whereas, hello Azure Storage SAS (Shared Access Signature) linked service provides hello data factory with restricted/time-bound access toohello Azure Storage.</span></span> <span data-ttu-id="26df9-127">Det finns några skillnader mellan dessa två länkade tjänster.</span><span class="sxs-lookup"><span data-stu-id="26df9-127">There are no other differences between these two linked services.</span></span> <span data-ttu-id="26df9-128">Välj hello länkade tjänst som passar dina behov.</span><span class="sxs-lookup"><span data-stu-id="26df9-128">Choose hello linked service that suits your needs.</span></span> <span data-ttu-id="26df9-129">hello följande avsnitt innehåller mer information om dessa två länkade tjänster.</span><span class="sxs-lookup"><span data-stu-id="26df9-129">hello following sections provide more details on these two linked services.</span></span>

[!INCLUDE [data-factory-azure-storage-linked-services](../../includes/data-factory-azure-storage-linked-services.md)]

## <a name="dataset-properties"></a><span data-ttu-id="26df9-130">Egenskaper för datamängd</span><span class="sxs-lookup"><span data-stu-id="26df9-130">Dataset properties</span></span>
<span data-ttu-id="26df9-131">En fullständig lista över egenskaper som är tillgängliga för att definiera datauppsättningarna & avsnitt finns hello [skapa datauppsättningar](data-factory-create-datasets.md) artikel.</span><span class="sxs-lookup"><span data-stu-id="26df9-131">For a full list of sections & properties available for defining datasets, see hello [Creating datasets](data-factory-create-datasets.md) article.</span></span> <span data-ttu-id="26df9-132">Avsnitt som struktur, tillgänglighet och princip på en datamängd JSON är liknande för alla typer av dataset (Azure SQL Azure blob, Azure-tabellen, osv.).</span><span class="sxs-lookup"><span data-stu-id="26df9-132">Sections such as structure, availability, and policy of a dataset JSON are similar for all dataset types (Azure SQL, Azure blob, Azure table, etc.).</span></span>

<span data-ttu-id="26df9-133">hello typeProperties avsnittet är olika för varje typ av dataset och ger information om hello platsen för hello data i datalagret hello.</span><span class="sxs-lookup"><span data-stu-id="26df9-133">hello typeProperties section is different for each type of dataset and provides information about hello location of hello data in hello data store.</span></span> <span data-ttu-id="26df9-134">Hej **typeProperties** avsnittet för hello dataset av typen **AzureTable** har hello följande egenskaper.</span><span class="sxs-lookup"><span data-stu-id="26df9-134">hello **typeProperties** section for hello dataset of type **AzureTable** has hello following properties.</span></span>

| <span data-ttu-id="26df9-135">Egenskap</span><span class="sxs-lookup"><span data-stu-id="26df9-135">Property</span></span> | <span data-ttu-id="26df9-136">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="26df9-136">Description</span></span> | <span data-ttu-id="26df9-137">Krävs</span><span class="sxs-lookup"><span data-stu-id="26df9-137">Required</span></span> |
| --- | --- | --- |
| <span data-ttu-id="26df9-138">tableName</span><span class="sxs-lookup"><span data-stu-id="26df9-138">tableName</span></span> |<span data-ttu-id="26df9-139">Namnet på hello tabell i hello Azure Table-databasinstansen som den länkade tjänsten refererar till.</span><span class="sxs-lookup"><span data-stu-id="26df9-139">Name of hello table in hello Azure Table Database instance that linked service refers to.</span></span> |<span data-ttu-id="26df9-140">Ja.</span><span class="sxs-lookup"><span data-stu-id="26df9-140">Yes.</span></span> <span data-ttu-id="26df9-141">När ett tabellnamn har angetts utan ett azureTableSourceQuery, är alla poster från hello tabell kopierade toohello mål.</span><span class="sxs-lookup"><span data-stu-id="26df9-141">When a tableName is specified without an azureTableSourceQuery, all records from hello table are copied toohello destination.</span></span> <span data-ttu-id="26df9-142">Om en azureTableSourceQuery också anges är poster från hello-tabell som uppfyller frågan hello kopierade toohello mål.</span><span class="sxs-lookup"><span data-stu-id="26df9-142">If an azureTableSourceQuery is also specified, records from hello table that satisfies hello query are copied toohello destination.</span></span> |

### <a name="schema-by-data-factory"></a><span data-ttu-id="26df9-143">Schema som Data Factory</span><span class="sxs-lookup"><span data-stu-id="26df9-143">Schema by Data Factory</span></span>
<span data-ttu-id="26df9-144">För schemafria data lagras till exempel Azure Table skapar hello Data Factory-tjänsten hello schema i något av följande sätt hello:</span><span class="sxs-lookup"><span data-stu-id="26df9-144">For schema-free data stores such as Azure Table, hello Data Factory service infers hello schema in one of hello following ways:</span></span>

1. <span data-ttu-id="26df9-145">Om du anger hello strukturen för data med hjälp av hello **struktur** egenskap i hello Data Factory-tjänsten i datauppsättningsdefinitionen hello godkänner den här strukturen som hello schema.</span><span class="sxs-lookup"><span data-stu-id="26df9-145">If you specify hello structure of data by using hello **structure** property in hello dataset definition, hello Data Factory service honors this structure as hello schema.</span></span> <span data-ttu-id="26df9-146">I det här fallet ges en rad inte innehåller ett värde för en kolumn, ett null-värde för den.</span><span class="sxs-lookup"><span data-stu-id="26df9-146">In this case, if a row does not contain a value for a column, a null value is provided for it.</span></span>
2. <span data-ttu-id="26df9-147">Om du inte anger hello strukturen för data med hjälp av hello **struktur** egenskap i hello datauppsättningsdefinitionen, Data Factory skapar hello schema med hjälp av hello första raden i hello data.</span><span class="sxs-lookup"><span data-stu-id="26df9-147">If you don't specify hello structure of data by using hello **structure** property in hello dataset definition, Data Factory infers hello schema by using hello first row in hello data.</span></span> <span data-ttu-id="26df9-148">I det här fallet missas hello första raden inte innehåller hello fullständig schemat, vissa kolumner i hello resultatet av kopieringsåtgärden.</span><span class="sxs-lookup"><span data-stu-id="26df9-148">In this case, if hello first row does not contain hello full schema, some columns are missed in hello result of copy operation.</span></span>

<span data-ttu-id="26df9-149">Därför är hello bästa praxis för schemafria datakällor toospecify hello struktur data med hjälp av hello **struktur** egenskapen.</span><span class="sxs-lookup"><span data-stu-id="26df9-149">Therefore, for schema-free data sources, hello best practice is toospecify hello structure of data using hello **structure** property.</span></span>

## <a name="copy-activity-properties"></a><span data-ttu-id="26df9-150">Kopiera egenskaper för aktivitet</span><span class="sxs-lookup"><span data-stu-id="26df9-150">Copy activity properties</span></span>
<span data-ttu-id="26df9-151">En fullständig lista över avsnitt & egenskaper som är tillgängliga för att definiera aktiviteter finns hello [skapar Pipelines](data-factory-create-pipelines.md) artikel.</span><span class="sxs-lookup"><span data-stu-id="26df9-151">For a full list of sections & properties available for defining activities, see hello [Creating Pipelines](data-factory-create-pipelines.md) article.</span></span> <span data-ttu-id="26df9-152">Egenskaper som namn, beskrivning, indata och utdata-datauppsättningar och -principer är tillgängliga för alla typer av aktiviteter.</span><span class="sxs-lookup"><span data-stu-id="26df9-152">Properties such as name, description, input and output datasets, and policies are available for all types of activities.</span></span>

<span data-ttu-id="26df9-153">Egenskaper i hello typeProperties avsnittet hello aktivitet på hello andra sidan varierar med varje aktivitetstyp.</span><span class="sxs-lookup"><span data-stu-id="26df9-153">Properties available in hello typeProperties section of hello activity on hello other hand vary with each activity type.</span></span> <span data-ttu-id="26df9-154">För Kopieringsaktivitet kan varierar de beroende på hello typer av datakällor och sänkor.</span><span class="sxs-lookup"><span data-stu-id="26df9-154">For Copy activity, they vary depending on hello types of sources and sinks.</span></span>

<span data-ttu-id="26df9-155">**AzureTableSource** stöder hello följande egenskaper i typeProperties avsnitt:</span><span class="sxs-lookup"><span data-stu-id="26df9-155">**AzureTableSource** supports hello following properties in typeProperties section:</span></span>

| <span data-ttu-id="26df9-156">Egenskap</span><span class="sxs-lookup"><span data-stu-id="26df9-156">Property</span></span> | <span data-ttu-id="26df9-157">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="26df9-157">Description</span></span> | <span data-ttu-id="26df9-158">Tillåtna värden</span><span class="sxs-lookup"><span data-stu-id="26df9-158">Allowed values</span></span> | <span data-ttu-id="26df9-159">Krävs</span><span class="sxs-lookup"><span data-stu-id="26df9-159">Required</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="26df9-160">azureTableSourceQuery</span><span class="sxs-lookup"><span data-stu-id="26df9-160">azureTableSourceQuery</span></span> |<span data-ttu-id="26df9-161">Använda hello anpassad fråga tooread data.</span><span class="sxs-lookup"><span data-stu-id="26df9-161">Use hello custom query tooread data.</span></span> |<span data-ttu-id="26df9-162">Azure-tabellen frågesträngen.</span><span class="sxs-lookup"><span data-stu-id="26df9-162">Azure table query string.</span></span> <span data-ttu-id="26df9-163">Se exemplen i hello nästa avsnitt.</span><span class="sxs-lookup"><span data-stu-id="26df9-163">See examples in hello next section.</span></span> |<span data-ttu-id="26df9-164">Nej.</span><span class="sxs-lookup"><span data-stu-id="26df9-164">No.</span></span> <span data-ttu-id="26df9-165">När ett tabellnamn har angetts utan ett azureTableSourceQuery, är alla poster från hello tabell kopierade toohello mål.</span><span class="sxs-lookup"><span data-stu-id="26df9-165">When a tableName is specified without an azureTableSourceQuery, all records from hello table are copied toohello destination.</span></span> <span data-ttu-id="26df9-166">Om en azureTableSourceQuery också anges är poster från hello-tabell som uppfyller frågan hello kopierade toohello mål.</span><span class="sxs-lookup"><span data-stu-id="26df9-166">If an azureTableSourceQuery is also specified, records from hello table that satisfies hello query are copied toohello destination.</span></span> |
| <span data-ttu-id="26df9-167">azureTableSourceIgnoreTableNotFound</span><span class="sxs-lookup"><span data-stu-id="26df9-167">azureTableSourceIgnoreTableNotFound</span></span> |<span data-ttu-id="26df9-168">Ange om swallow hello undantag av tabellen inte finns.</span><span class="sxs-lookup"><span data-stu-id="26df9-168">Indicate whether swallow hello exception of table not exist.</span></span> |<span data-ttu-id="26df9-169">SANT</span><span class="sxs-lookup"><span data-stu-id="26df9-169">TRUE</span></span><br/><span data-ttu-id="26df9-170">FALSKT</span><span class="sxs-lookup"><span data-stu-id="26df9-170">FALSE</span></span> |<span data-ttu-id="26df9-171">Nej</span><span class="sxs-lookup"><span data-stu-id="26df9-171">No</span></span> |

### <a name="azuretablesourcequery-examples"></a><span data-ttu-id="26df9-172">azureTableSourceQuery-exempel</span><span class="sxs-lookup"><span data-stu-id="26df9-172">azureTableSourceQuery examples</span></span>
<span data-ttu-id="26df9-173">Om Azure Table kolumnen är av strängtypen:</span><span class="sxs-lookup"><span data-stu-id="26df9-173">If Azure Table column is of string type:</span></span>

```JSON
azureTableSourceQuery": "$$Text.Format('PartitionKey ge \\'{0:yyyyMMddHH00_0000}\\' and PartitionKey le \\'{0:yyyyMMddHH00_9999}\\'', SliceStart)"
```

<span data-ttu-id="26df9-174">Om Azure Table kolumnen är av typen datetime:</span><span class="sxs-lookup"><span data-stu-id="26df9-174">If Azure Table column is of datetime type:</span></span>

```JSON
"azureTableSourceQuery": "$$Text.Format('DeploymentEndTime gt datetime\\'{0:yyyy-MM-ddTHH:mm:ssZ}\\' and DeploymentEndTime le datetime\\'{1:yyyy-MM-ddTHH:mm:ssZ}\\'', SliceStart, SliceEnd)"
```

<span data-ttu-id="26df9-175">**AzureTableSink** stöder hello följande egenskaper i typeProperties avsnitt:</span><span class="sxs-lookup"><span data-stu-id="26df9-175">**AzureTableSink** supports hello following properties in typeProperties section:</span></span>

| <span data-ttu-id="26df9-176">Egenskap</span><span class="sxs-lookup"><span data-stu-id="26df9-176">Property</span></span> | <span data-ttu-id="26df9-177">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="26df9-177">Description</span></span> | <span data-ttu-id="26df9-178">Tillåtna värden</span><span class="sxs-lookup"><span data-stu-id="26df9-178">Allowed values</span></span> | <span data-ttu-id="26df9-179">Krävs</span><span class="sxs-lookup"><span data-stu-id="26df9-179">Required</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="26df9-180">azureTableDefaultPartitionKeyValue</span><span class="sxs-lookup"><span data-stu-id="26df9-180">azureTableDefaultPartitionKeyValue</span></span> |<span data-ttu-id="26df9-181">Standard partitionsnyckelvärde som kan användas av hello mottagare.</span><span class="sxs-lookup"><span data-stu-id="26df9-181">Default partition key value that can be used by hello sink.</span></span> |<span data-ttu-id="26df9-182">Ett strängvärde.</span><span class="sxs-lookup"><span data-stu-id="26df9-182">A string value.</span></span> |<span data-ttu-id="26df9-183">Nej</span><span class="sxs-lookup"><span data-stu-id="26df9-183">No</span></span> |
| <span data-ttu-id="26df9-184">azureTablePartitionKeyName</span><span class="sxs-lookup"><span data-stu-id="26df9-184">azureTablePartitionKeyName</span></span> |<span data-ttu-id="26df9-185">Ange namnet på hello kolumn vars värden används som partitionsnycklar.</span><span class="sxs-lookup"><span data-stu-id="26df9-185">Specify name of hello column whose values are used as partition keys.</span></span> <span data-ttu-id="26df9-186">Om inget anges används AzureTableDefaultPartitionKeyValue som hello partitionsnyckel.</span><span class="sxs-lookup"><span data-stu-id="26df9-186">If not specified, AzureTableDefaultPartitionKeyValue is used as hello partition key.</span></span> |<span data-ttu-id="26df9-187">Ett kolumnnamn.</span><span class="sxs-lookup"><span data-stu-id="26df9-187">A column name.</span></span> |<span data-ttu-id="26df9-188">Nej</span><span class="sxs-lookup"><span data-stu-id="26df9-188">No</span></span> |
| <span data-ttu-id="26df9-189">azureTableRowKeyName</span><span class="sxs-lookup"><span data-stu-id="26df9-189">azureTableRowKeyName</span></span> |<span data-ttu-id="26df9-190">Ange namnet på hello kolumn vars kolumnvärdena används som radnyckel.</span><span class="sxs-lookup"><span data-stu-id="26df9-190">Specify name of hello column whose column values are used as row key.</span></span> <span data-ttu-id="26df9-191">Om inget annat anges, kan du använda ett GUID för varje rad.</span><span class="sxs-lookup"><span data-stu-id="26df9-191">If not specified, use a GUID for each row.</span></span> |<span data-ttu-id="26df9-192">Ett kolumnnamn.</span><span class="sxs-lookup"><span data-stu-id="26df9-192">A column name.</span></span> |<span data-ttu-id="26df9-193">Nej</span><span class="sxs-lookup"><span data-stu-id="26df9-193">No</span></span> |
| <span data-ttu-id="26df9-194">azureTableInsertType</span><span class="sxs-lookup"><span data-stu-id="26df9-194">azureTableInsertType</span></span> |<span data-ttu-id="26df9-195">hello läge tooinsert data till Azure-tabellen.</span><span class="sxs-lookup"><span data-stu-id="26df9-195">hello mode tooinsert data into Azure table.</span></span><br/><br/><span data-ttu-id="26df9-196">Den här egenskapen anger om befintliga rader i hello utdatatabell med matchande partition och radnycklar få sina värden bytas ut eller samman.</span><span class="sxs-lookup"><span data-stu-id="26df9-196">This property controls whether existing rows in hello output table with matching partition and row keys have their values replaced or merged.</span></span> <br/><br/><span data-ttu-id="26df9-197">toolearn om hur dessa inställningar (dokument och Ersätt) fungerar, se [Insert- eller Merge-entiteten](https://msdn.microsoft.com/library/azure/hh452241.aspx) och [infoga eller ersätta entiteten](https://msdn.microsoft.com/library/azure/hh452242.aspx) avsnitt.</span><span class="sxs-lookup"><span data-stu-id="26df9-197">toolearn about how these settings (merge and replace) work, see [Insert or Merge Entity](https://msdn.microsoft.com/library/azure/hh452241.aspx) and [Insert or Replace Entity](https://msdn.microsoft.com/library/azure/hh452242.aspx) topics.</span></span> <br/><br> <span data-ttu-id="26df9-198">Den här inställningen gäller vid hello raden nivån, inte hello tabell nivån, varken alternativet tar bort rader i hello utdatatabell som inte finns i hello indata.</span><span class="sxs-lookup"><span data-stu-id="26df9-198">This setting applies at hello row level, not hello table level, and neither option deletes rows in hello output table that do not exist in hello input.</span></span> |<span data-ttu-id="26df9-199">sammanfoga (standard)</span><span class="sxs-lookup"><span data-stu-id="26df9-199">merge (default)</span></span><br/><span data-ttu-id="26df9-200">Ersätt</span><span class="sxs-lookup"><span data-stu-id="26df9-200">replace</span></span> |<span data-ttu-id="26df9-201">Nej</span><span class="sxs-lookup"><span data-stu-id="26df9-201">No</span></span> |
| <span data-ttu-id="26df9-202">writeBatchSize</span><span class="sxs-lookup"><span data-stu-id="26df9-202">writeBatchSize</span></span> |<span data-ttu-id="26df9-203">Infogar data i hello Azure-tabellen när hello writeBatchSize eller writeBatchTimeout namn.</span><span class="sxs-lookup"><span data-stu-id="26df9-203">Inserts data into hello Azure table when hello writeBatchSize or writeBatchTimeout is hit.</span></span> |<span data-ttu-id="26df9-204">Heltal (antalet rader)</span><span class="sxs-lookup"><span data-stu-id="26df9-204">Integer (number of rows)</span></span> |<span data-ttu-id="26df9-205">Nej (standard: 10000)</span><span class="sxs-lookup"><span data-stu-id="26df9-205">No (default: 10000)</span></span> |
| <span data-ttu-id="26df9-206">writeBatchTimeout</span><span class="sxs-lookup"><span data-stu-id="26df9-206">writeBatchTimeout</span></span> |<span data-ttu-id="26df9-207">Infogar data i hello Azure-tabellen när hello writeBatchSize eller writeBatchTimeout namn</span><span class="sxs-lookup"><span data-stu-id="26df9-207">Inserts data into hello Azure table when hello writeBatchSize or writeBatchTimeout is hit</span></span> |<span data-ttu-id="26df9-208">TimeSpan</span><span class="sxs-lookup"><span data-stu-id="26df9-208">timespan</span></span><br/><br/><span data-ttu-id="26df9-209">Exempel ”: 00: 20:00” (20 minuter)</span><span class="sxs-lookup"><span data-stu-id="26df9-209">Example: “00:20:00” (20 minutes)</span></span> |<span data-ttu-id="26df9-210">Nej (standard toostorage klienten standardtimeout-värdet 90 sek)</span><span class="sxs-lookup"><span data-stu-id="26df9-210">No (Default toostorage client default timeout value 90 sec)</span></span> |

### <a name="azuretablepartitionkeyname"></a><span data-ttu-id="26df9-211">azureTablePartitionKeyName</span><span class="sxs-lookup"><span data-stu-id="26df9-211">azureTablePartitionKeyName</span></span>
<span data-ttu-id="26df9-212">Mappa en kolumn tooa mål källkolumn använda hello översättare JSON-egenskapen innan du kan använda hello målkolumnen som hello azureTablePartitionKeyName.</span><span class="sxs-lookup"><span data-stu-id="26df9-212">Map a source column tooa destination column using hello translator JSON property before you can use hello destination column as hello azureTablePartitionKeyName.</span></span>

<span data-ttu-id="26df9-213">I följande exempel hello, källkolumnen DivisionID är mappade toohello målkolumnen: DivisionID.</span><span class="sxs-lookup"><span data-stu-id="26df9-213">In hello following example, source column DivisionID is mapped toohello destination column: DivisionID.</span></span>  

```JSON
"translator": {
    "type": "TabularTranslator",
    "columnMappings": "DivisionID: DivisionID, FirstName: FirstName, LastName: LastName"
}
```
<span data-ttu-id="26df9-214">Hej DivisionID har angetts som hello partitionsnyckel.</span><span class="sxs-lookup"><span data-stu-id="26df9-214">hello DivisionID is specified as hello partition key.</span></span>

```JSON
"sink": {
    "type": "AzureTableSink",
    "azureTablePartitionKeyName": "DivisionID",
    "writeBatchSize": 100,
    "writeBatchTimeout": "01:00:00"
}
```
## <a name="json-examples"></a><span data-ttu-id="26df9-215">JSON-exempel</span><span class="sxs-lookup"><span data-stu-id="26df9-215">JSON examples</span></span>
<span data-ttu-id="26df9-216">hello följande exempel ger exempel JSON definitioner som du kan använda toocreate en pipeline med hjälp av [Azure-portalen](data-factory-copy-activity-tutorial-using-azure-portal.md) eller [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md) eller [Azure PowerShell](data-factory-copy-activity-tutorial-using-powershell.md).</span><span class="sxs-lookup"><span data-stu-id="26df9-216">hello following examples provide sample JSON definitions that you can use toocreate a pipeline by using [Azure portal](data-factory-copy-activity-tutorial-using-azure-portal.md) or [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md) or [Azure PowerShell](data-factory-copy-activity-tutorial-using-powershell.md).</span></span> <span data-ttu-id="26df9-217">De visar hur toocopy data tooand från Azure Table Storage och Azure Blob-databas.</span><span class="sxs-lookup"><span data-stu-id="26df9-217">They show how toocopy data tooand from Azure Table Storage and Azure Blob Database.</span></span> <span data-ttu-id="26df9-218">Dock datan kan kopieras **direkt** från någon av hello källor tooany av sänkor hello stöds.</span><span class="sxs-lookup"><span data-stu-id="26df9-218">However, data can be copied **directly** from any of hello sources tooany of hello supported sinks.</span></span> <span data-ttu-id="26df9-219">Mer information finns i avsnittet hello ”stöds datalager och format” i [flytta data med hjälp av Kopieringsaktiviteten](data-factory-data-movement-activities.md).</span><span class="sxs-lookup"><span data-stu-id="26df9-219">For more information, see hello section "Supported data stores and formats" in [Move data by using Copy Activity](data-factory-data-movement-activities.md).</span></span>

## <a name="example-copy-data-from-azure-table-tooazure-blob"></a><span data-ttu-id="26df9-220">Exempel: Kopiera data från Azure Table tooAzure Blob</span><span class="sxs-lookup"><span data-stu-id="26df9-220">Example: Copy data from Azure Table tooAzure Blob</span></span>
<span data-ttu-id="26df9-221">följande exempel visar hello:</span><span class="sxs-lookup"><span data-stu-id="26df9-221">hello following sample shows:</span></span>

1. <span data-ttu-id="26df9-222">En länkad tjänst av typen [AzureStorage](data-factory-azure-blob-connector.md#linked-service-properties) (används för blob & tabell).</span><span class="sxs-lookup"><span data-stu-id="26df9-222">A linked service of type [AzureStorage](data-factory-azure-blob-connector.md#linked-service-properties) (used for both table & blob).</span></span>
2. <span data-ttu-id="26df9-223">Indata [dataset](data-factory-create-datasets.md) av typen [AzureTable](#dataset-properties).</span><span class="sxs-lookup"><span data-stu-id="26df9-223">An input [dataset](data-factory-create-datasets.md) of type [AzureTable](#dataset-properties).</span></span>
3. <span data-ttu-id="26df9-224">Utdata [dataset](data-factory-create-datasets.md) av typen [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties).</span><span class="sxs-lookup"><span data-stu-id="26df9-224">An output [dataset](data-factory-create-datasets.md) of type [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties).</span></span>
4. <span data-ttu-id="26df9-225">Hej [pipeline](data-factory-create-pipelines.md) med kopieringsaktiviteten som använder [AzureTableSource](#activity-properties) och [BlobSink](data-factory-azure-blob-connector.md#copy-activity-properties).</span><span class="sxs-lookup"><span data-stu-id="26df9-225">hello [pipeline](data-factory-create-pipelines.md) with Copy activity that uses [AzureTableSource](#activity-properties) and [BlobSink](data-factory-azure-blob-connector.md#copy-activity-properties).</span></span>

<span data-ttu-id="26df9-226">hello exemplet kopierar data som tillhör toohello standardpartition i en Azure Table tooa blob varje timme.</span><span class="sxs-lookup"><span data-stu-id="26df9-226">hello sample copies data belonging toohello default partition in an Azure Table tooa blob every hour.</span></span> <span data-ttu-id="26df9-227">hello JSON egenskaper som används i exemplen beskrivs i hello-exempel i följande avsnitt.</span><span class="sxs-lookup"><span data-stu-id="26df9-227">hello JSON properties used in these samples are described in sections following hello samples.</span></span>

<span data-ttu-id="26df9-228">**Länkad Azure storage-tjänst:**</span><span class="sxs-lookup"><span data-stu-id="26df9-228">**Azure storage linked service:**</span></span>

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
<span data-ttu-id="26df9-229">Azure Data Factory stöder två typer av länkad Azure Storage services: **AzureStorage** och **AzureStorageSas**.</span><span class="sxs-lookup"><span data-stu-id="26df9-229">Azure Data Factory supports two types of Azure Storage linked services: **AzureStorage** and **AzureStorageSas**.</span></span> <span data-ttu-id="26df9-230">För hello första, anger du hello anslutningssträng som innehåller hello kontonyckel och för hello senare, anger du hello delade signatur åtkomst (SAS)-Uri.</span><span class="sxs-lookup"><span data-stu-id="26df9-230">For hello first one, you specify hello connection string that includes hello account key and for hello later one, you specify hello Shared Access Signature (SAS) Uri.</span></span> <span data-ttu-id="26df9-231">Se [länkade tjänster](#linked-service-properties) information.</span><span class="sxs-lookup"><span data-stu-id="26df9-231">See [Linked Services](#linked-service-properties) section for details.</span></span>  

<span data-ttu-id="26df9-232">**Azure Table inkommande datamängd:**</span><span class="sxs-lookup"><span data-stu-id="26df9-232">**Azure Table input dataset:**</span></span>

<span data-ttu-id="26df9-233">hello exemplet förutsätter att du har skapat en tabell ”mytable” som prefix i Azure Table.</span><span class="sxs-lookup"><span data-stu-id="26df9-233">hello sample assumes you have created a table “MyTable” in Azure Table.</span></span>

<span data-ttu-id="26df9-234">Inställningen ”externa”: ”true” informerar hello Data Factory-tjänsten som hello dataset är externa toohello data factory och inte tillverkas av en aktivitet i hello data factory.</span><span class="sxs-lookup"><span data-stu-id="26df9-234">Setting “external”: ”true” informs hello Data Factory service that hello dataset is external toohello data factory and is not produced by an activity in hello data factory.</span></span>

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

<span data-ttu-id="26df9-235">**Azure Blob utdatauppsättningen:**</span><span class="sxs-lookup"><span data-stu-id="26df9-235">**Azure Blob output dataset:**</span></span>

<span data-ttu-id="26df9-236">Data skrivs tooa nya blob varje timme (frekvens: timme, intervall: 1).</span><span class="sxs-lookup"><span data-stu-id="26df9-236">Data is written tooa new blob every hour (frequency: hour, interval: 1).</span></span> <span data-ttu-id="26df9-237">hello mappsökväg för hello blob utvärderas dynamiskt baserat på hello starttiden för hello-segment som bearbetas.</span><span class="sxs-lookup"><span data-stu-id="26df9-237">hello folder path for hello blob is dynamically evaluated based on hello start time of hello slice that is being processed.</span></span> <span data-ttu-id="26df9-238">hello mappsökväg använder år, månad, dag och timmar delar av hello starttid.</span><span class="sxs-lookup"><span data-stu-id="26df9-238">hello folder path uses year, month, day, and hours parts of hello start time.</span></span>

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

<span data-ttu-id="26df9-239">**Kopiera aktivitet i en pipeline med AzureTableSource och BlobSink:**</span><span class="sxs-lookup"><span data-stu-id="26df9-239">**Copy activity in a pipeline with AzureTableSource and BlobSink:**</span></span>

<span data-ttu-id="26df9-240">hello pipelinen innehåller en kopia-aktivitet som är konfigurerade toouse hello inkommande och utgående datauppsättningar och är schemalagda toorun varje timme.</span><span class="sxs-lookup"><span data-stu-id="26df9-240">hello pipeline contains a Copy Activity that is configured toouse hello input and output datasets and is scheduled toorun every hour.</span></span> <span data-ttu-id="26df9-241">I hello pipeline JSON-definitionen hello **källa** typ har angetts för**AzureTableSource** och **sink** typ har angetts för**BlobSink**.</span><span class="sxs-lookup"><span data-stu-id="26df9-241">In hello pipeline JSON definition, hello **source** type is set too**AzureTableSource** and **sink** type is set too**BlobSink**.</span></span> <span data-ttu-id="26df9-242">hello SQL-frågan som anges med **AzureTableSourceQuery** egenskapen väljer hello data från hello standardpartition toocopy varje timme.</span><span class="sxs-lookup"><span data-stu-id="26df9-242">hello SQL query specified with **AzureTableSourceQuery** property selects hello data from hello default partition every hour toocopy.</span></span>

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

## <a name="example-copy-data-from-azure-blob-tooazure-table"></a><span data-ttu-id="26df9-243">Exempel: Kopiera data från Azure Blob tooAzure tabell</span><span class="sxs-lookup"><span data-stu-id="26df9-243">Example: Copy data from Azure Blob tooAzure Table</span></span>
<span data-ttu-id="26df9-244">följande exempel visar hello:</span><span class="sxs-lookup"><span data-stu-id="26df9-244">hello following sample shows:</span></span>

1. <span data-ttu-id="26df9-245">En länkad tjänst av typen [AzureStorage](data-factory-azure-blob-connector.md#linked-service-properties) (används för blob & tabell)</span><span class="sxs-lookup"><span data-stu-id="26df9-245">A linked service of type [AzureStorage](data-factory-azure-blob-connector.md#linked-service-properties) (used for both table & blob)</span></span>
2. <span data-ttu-id="26df9-246">Indata [dataset](data-factory-create-datasets.md) av typen [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties).</span><span class="sxs-lookup"><span data-stu-id="26df9-246">An input [dataset](data-factory-create-datasets.md) of type [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties).</span></span>
3. <span data-ttu-id="26df9-247">Utdata [dataset](data-factory-create-datasets.md) av typen [AzureTable](#dataset-properties).</span><span class="sxs-lookup"><span data-stu-id="26df9-247">An output [dataset](data-factory-create-datasets.md) of type [AzureTable](#dataset-properties).</span></span>
4. <span data-ttu-id="26df9-248">Hej [pipeline](data-factory-create-pipelines.md) med kopieringsaktiviteten som använder [BlobSource](data-factory-azure-blob-connector.md#copy-activity-properties) och [AzureTableSink](#copy-activity-properties).</span><span class="sxs-lookup"><span data-stu-id="26df9-248">hello [pipeline](data-factory-create-pipelines.md) with Copy activity that uses [BlobSource](data-factory-azure-blob-connector.md#copy-activity-properties) and [AzureTableSink](#copy-activity-properties).</span></span>

<span data-ttu-id="26df9-249">hello exemplet kopierar time series-data från ett Azure blob-tooan Azure-tabellen varje timme.</span><span class="sxs-lookup"><span data-stu-id="26df9-249">hello sample copies time-series data from an Azure blob tooan Azure table hourly.</span></span> <span data-ttu-id="26df9-250">hello JSON egenskaper som används i exemplen beskrivs i hello-exempel i följande avsnitt.</span><span class="sxs-lookup"><span data-stu-id="26df9-250">hello JSON properties used in these samples are described in sections following hello samples.</span></span>

<span data-ttu-id="26df9-251">**Länkad Azure storage (för både Azure Table & Blob)-tjänst:**</span><span class="sxs-lookup"><span data-stu-id="26df9-251">**Azure storage (for both Azure Table & Blob) linked service:**</span></span>

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

<span data-ttu-id="26df9-252">Azure Data Factory stöder två typer av länkad Azure Storage services: **AzureStorage** och **AzureStorageSas**.</span><span class="sxs-lookup"><span data-stu-id="26df9-252">Azure Data Factory supports two types of Azure Storage linked services: **AzureStorage** and **AzureStorageSas**.</span></span> <span data-ttu-id="26df9-253">För hello första, anger du hello anslutningssträng som innehåller hello kontonyckel och för hello senare, anger du hello delade signatur åtkomst (SAS)-Uri.</span><span class="sxs-lookup"><span data-stu-id="26df9-253">For hello first one, you specify hello connection string that includes hello account key and for hello later one, you specify hello Shared Access Signature (SAS) Uri.</span></span> <span data-ttu-id="26df9-254">Se [länkade tjänster](#linked-service-properties) information.</span><span class="sxs-lookup"><span data-stu-id="26df9-254">See [Linked Services](#linked-service-properties) section for details.</span></span>

<span data-ttu-id="26df9-255">**Azure Blob inkommande datamängd:**</span><span class="sxs-lookup"><span data-stu-id="26df9-255">**Azure Blob input dataset:**</span></span>

<span data-ttu-id="26df9-256">Data hämtas från en ny blob varje timme (frekvens: timme, intervall: 1).</span><span class="sxs-lookup"><span data-stu-id="26df9-256">Data is picked up from a new blob every hour (frequency: hour, interval: 1).</span></span> <span data-ttu-id="26df9-257">hello mappen sökvägen och filnamnet för hello blob utvärderas dynamiskt baserat på hello starttiden för hello-segment som bearbetas.</span><span class="sxs-lookup"><span data-stu-id="26df9-257">hello folder path and file name for hello blob are dynamically evaluated based on hello start time of hello slice that is being processed.</span></span> <span data-ttu-id="26df9-258">hello mappsökväg använder år, månad och som en dagdel av hello starttid och filnamn använder hello timme tillhör hello starttid.</span><span class="sxs-lookup"><span data-stu-id="26df9-258">hello folder path uses year, month, and day part of hello start time and file name uses hello hour part of hello start time.</span></span> <span data-ttu-id="26df9-259">”externa”: ”true” inställningen informerar hello Data Factory-tjänsten som hello dataset är externa toohello data factory och inte tillverkas av en aktivitet i hello data factory.</span><span class="sxs-lookup"><span data-stu-id="26df9-259">“external”: “true” setting informs hello Data Factory service that hello dataset is external toohello data factory and is not produced by an activity in hello data factory.</span></span>

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

<span data-ttu-id="26df9-260">**Azure-tabellen utdatauppsättningen:**</span><span class="sxs-lookup"><span data-stu-id="26df9-260">**Azure Table output dataset:**</span></span>

<span data-ttu-id="26df9-261">hello exemplet kopierar data tooa tabell med namnet ”mytable” som prefix i Azure Table.</span><span class="sxs-lookup"><span data-stu-id="26df9-261">hello sample copies data tooa table named “MyTable” in Azure Table.</span></span> <span data-ttu-id="26df9-262">Skapa en Azure-tabell med hello samma antal kolumner som du förväntar dig hello Blob CSV-filen toocontain.</span><span class="sxs-lookup"><span data-stu-id="26df9-262">Create an Azure table with hello same number of columns as you expect hello Blob CSV file toocontain.</span></span> <span data-ttu-id="26df9-263">Nya rader läggs toohello tabell varje timme.</span><span class="sxs-lookup"><span data-stu-id="26df9-263">New rows are added toohello table every hour.</span></span>

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

<span data-ttu-id="26df9-264">**Kopiera aktivitet i en pipeline med BlobSource och AzureTableSink:**</span><span class="sxs-lookup"><span data-stu-id="26df9-264">**Copy activity in a pipeline with BlobSource and AzureTableSink:**</span></span>

<span data-ttu-id="26df9-265">hello pipelinen innehåller en kopia-aktivitet som är konfigurerade toouse hello inkommande och utgående datauppsättningar och är schemalagda toorun varje timme.</span><span class="sxs-lookup"><span data-stu-id="26df9-265">hello pipeline contains a Copy Activity that is configured toouse hello input and output datasets and is scheduled toorun every hour.</span></span> <span data-ttu-id="26df9-266">I hello pipeline JSON-definitionen hello **källa** typ har angetts för**BlobSource** och **sink** typ har angetts för**AzureTableSink**.</span><span class="sxs-lookup"><span data-stu-id="26df9-266">In hello pipeline JSON definition, hello **source** type is set too**BlobSource** and **sink** type is set too**AzureTableSink**.</span></span>

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
## <a name="type-mapping-for-azure-table"></a><span data-ttu-id="26df9-267">Mappning för Azure-tabellen</span><span class="sxs-lookup"><span data-stu-id="26df9-267">Type Mapping for Azure Table</span></span>
<span data-ttu-id="26df9-268">Som anges i hello [data movement aktiviteter](data-factory-data-movement-activities.md) artikeln kopieringsaktiviteten utför automatisk konverteringar från källan typer toosink typer med hello följande metod i två steg.</span><span class="sxs-lookup"><span data-stu-id="26df9-268">As mentioned in hello [data movement activities](data-factory-data-movement-activities.md) article, Copy activity performs automatic type conversions from source types toosink types with hello following two-step approach.</span></span>

1. <span data-ttu-id="26df9-269">Konvertera från inbyggda typer too.NET källtypen</span><span class="sxs-lookup"><span data-stu-id="26df9-269">Convert from native source types too.NET type</span></span>
2. <span data-ttu-id="26df9-270">Konvertera från .NET typen toonative Mottagartypen</span><span class="sxs-lookup"><span data-stu-id="26df9-270">Convert from .NET type toonative sink type</span></span>

<span data-ttu-id="26df9-271">När du flyttar data & från Azure Table, hello följande [mappningar som definierats av Azure Table-tjänsten](https://msdn.microsoft.com/library/azure/dd179338.aspx) används från Azure Table OData typer too.NET typ och vice versa.</span><span class="sxs-lookup"><span data-stu-id="26df9-271">When moving data too& from Azure Table, hello following [mappings defined by Azure Table service](https://msdn.microsoft.com/library/azure/dd179338.aspx) are used from Azure Table OData types too.NET type and vice versa.</span></span>

| <span data-ttu-id="26df9-272">OData-datatyp</span><span class="sxs-lookup"><span data-stu-id="26df9-272">OData Data Type</span></span> | <span data-ttu-id="26df9-273">.NET-typ</span><span class="sxs-lookup"><span data-stu-id="26df9-273">.NET Type</span></span> | <span data-ttu-id="26df9-274">Information</span><span class="sxs-lookup"><span data-stu-id="26df9-274">Details</span></span> |
| --- | --- | --- |
| <span data-ttu-id="26df9-275">Edm.Binary</span><span class="sxs-lookup"><span data-stu-id="26df9-275">Edm.Binary</span></span> |<span data-ttu-id="26df9-276">byte]</span><span class="sxs-lookup"><span data-stu-id="26df9-276">byte[]</span></span> |<span data-ttu-id="26df9-277">En matris med byte in too64 KB.</span><span class="sxs-lookup"><span data-stu-id="26df9-277">An array of bytes up too64 KB.</span></span> |
| <span data-ttu-id="26df9-278">Edm.Boolean</span><span class="sxs-lookup"><span data-stu-id="26df9-278">Edm.Boolean</span></span> |<span data-ttu-id="26df9-279">bool</span><span class="sxs-lookup"><span data-stu-id="26df9-279">bool</span></span> |<span data-ttu-id="26df9-280">Ett booleskt värde.</span><span class="sxs-lookup"><span data-stu-id="26df9-280">A Boolean value.</span></span> |
| <span data-ttu-id="26df9-281">Edm.DateTime</span><span class="sxs-lookup"><span data-stu-id="26df9-281">Edm.DateTime</span></span> |<span data-ttu-id="26df9-282">Datum och tid</span><span class="sxs-lookup"><span data-stu-id="26df9-282">DateTime</span></span> |<span data-ttu-id="26df9-283">En 64-bitars värdet uttrycks som Coordinated Universal Time (UTC).</span><span class="sxs-lookup"><span data-stu-id="26df9-283">A 64-bit value expressed as Coordinated Universal Time (UTC).</span></span> <span data-ttu-id="26df9-284">hello stöds DateTime intervallet börjar från midnatt, 1 januari, 1601 e. kr.</span><span class="sxs-lookup"><span data-stu-id="26df9-284">hello supported DateTime range begins from 12:00 midnight, January 1, 1601 A.D.</span></span> <span data-ttu-id="26df9-285">(C.E.) UTC.</span><span class="sxs-lookup"><span data-stu-id="26df9-285">(C.E.), UTC.</span></span> <span data-ttu-id="26df9-286">hello intervallet slutar vid den 31 December 9999.</span><span class="sxs-lookup"><span data-stu-id="26df9-286">hello range ends at December 31, 9999.</span></span> |
| <span data-ttu-id="26df9-287">Edm.Double</span><span class="sxs-lookup"><span data-stu-id="26df9-287">Edm.Double</span></span> |<span data-ttu-id="26df9-288">dubbla</span><span class="sxs-lookup"><span data-stu-id="26df9-288">double</span></span> |<span data-ttu-id="26df9-289">En 64-bitars flytande punktvärdet.</span><span class="sxs-lookup"><span data-stu-id="26df9-289">A 64-bit floating point value.</span></span> |
| <span data-ttu-id="26df9-290">Edm.Guid</span><span class="sxs-lookup"><span data-stu-id="26df9-290">Edm.Guid</span></span> |<span data-ttu-id="26df9-291">GUID</span><span class="sxs-lookup"><span data-stu-id="26df9-291">Guid</span></span> |<span data-ttu-id="26df9-292">En 128-bitars globalt unik identifierare.</span><span class="sxs-lookup"><span data-stu-id="26df9-292">A 128-bit globally unique identifier.</span></span> |
| <span data-ttu-id="26df9-293">Edm.Int32</span><span class="sxs-lookup"><span data-stu-id="26df9-293">Edm.Int32</span></span> |<span data-ttu-id="26df9-294">Int32</span><span class="sxs-lookup"><span data-stu-id="26df9-294">Int32</span></span> |<span data-ttu-id="26df9-295">En 32-bitars heltal.</span><span class="sxs-lookup"><span data-stu-id="26df9-295">A 32-bit integer.</span></span> |
| <span data-ttu-id="26df9-296">Edm.Int64</span><span class="sxs-lookup"><span data-stu-id="26df9-296">Edm.Int64</span></span> |<span data-ttu-id="26df9-297">Int64</span><span class="sxs-lookup"><span data-stu-id="26df9-297">Int64</span></span> |<span data-ttu-id="26df9-298">En 64-bitars heltal.</span><span class="sxs-lookup"><span data-stu-id="26df9-298">A 64-bit integer.</span></span> |
| <span data-ttu-id="26df9-299">Edm.String</span><span class="sxs-lookup"><span data-stu-id="26df9-299">Edm.String</span></span> |<span data-ttu-id="26df9-300">Sträng</span><span class="sxs-lookup"><span data-stu-id="26df9-300">String</span></span> |<span data-ttu-id="26df9-301">Ett värde för UTF-16-kodad.</span><span class="sxs-lookup"><span data-stu-id="26df9-301">A UTF-16-encoded value.</span></span> <span data-ttu-id="26df9-302">Strängvärden kanske in too64 KB.</span><span class="sxs-lookup"><span data-stu-id="26df9-302">String values may be up too64 KB.</span></span> |

### <a name="type-conversion-sample"></a><span data-ttu-id="26df9-303">Exempel konvertering</span><span class="sxs-lookup"><span data-stu-id="26df9-303">Type Conversion Sample</span></span>
<span data-ttu-id="26df9-304">följande exempel hello är för att kopiera data från Azure Blob-tooAzure tabell med typkonverteringar.</span><span class="sxs-lookup"><span data-stu-id="26df9-304">hello following sample is for copying data from an Azure Blob tooAzure Table with type conversions.</span></span>

<span data-ttu-id="26df9-305">Anta att hello-blobbdatauppsättning är CSV-format och innehåller tre kolumner.</span><span class="sxs-lookup"><span data-stu-id="26df9-305">Suppose hello Blob dataset is in CSV format and contains three columns.</span></span> <span data-ttu-id="26df9-306">En av dem är ett datetime-kolumn med en anpassad datetime-format med hjälp av förkortade franska namn för hello veckodag.</span><span class="sxs-lookup"><span data-stu-id="26df9-306">One of them is a datetime column with a custom datetime format using abbreviated French names for day of hello week.</span></span>

<span data-ttu-id="26df9-307">Definiera hello Blob källa dataset enligt följande tillsammans med typdefinitioner för hello kolumner.</span><span class="sxs-lookup"><span data-stu-id="26df9-307">Define hello Blob Source dataset as follows along with type definitions for hello columns.</span></span>

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
<span data-ttu-id="26df9-308">Hello mappning från Azure Table OData too.NET typ får definierar du hello tabell i Azure-tabellen med hello följer schemat.</span><span class="sxs-lookup"><span data-stu-id="26df9-308">Given hello type mapping from Azure Table OData type too.NET type, you would define hello table in Azure Table with hello following schema.</span></span>

<span data-ttu-id="26df9-309">**Azure tabellschemat:**</span><span class="sxs-lookup"><span data-stu-id="26df9-309">**Azure Table schema:**</span></span>

| <span data-ttu-id="26df9-310">Kolumnnamn</span><span class="sxs-lookup"><span data-stu-id="26df9-310">Column name</span></span> | <span data-ttu-id="26df9-311">Typ</span><span class="sxs-lookup"><span data-stu-id="26df9-311">Type</span></span> |
| --- | --- |
| <span data-ttu-id="26df9-312">användar-ID</span><span class="sxs-lookup"><span data-stu-id="26df9-312">userid</span></span> |<span data-ttu-id="26df9-313">Edm.Int64</span><span class="sxs-lookup"><span data-stu-id="26df9-313">Edm.Int64</span></span> |
| <span data-ttu-id="26df9-314">namn</span><span class="sxs-lookup"><span data-stu-id="26df9-314">name</span></span> |<span data-ttu-id="26df9-315">Edm.String</span><span class="sxs-lookup"><span data-stu-id="26df9-315">Edm.String</span></span> |
| <span data-ttu-id="26df9-316">lastlogindate</span><span class="sxs-lookup"><span data-stu-id="26df9-316">lastlogindate</span></span> |<span data-ttu-id="26df9-317">Edm.DateTime</span><span class="sxs-lookup"><span data-stu-id="26df9-317">Edm.DateTime</span></span> |

<span data-ttu-id="26df9-318">Därefter definiera hello Azure Table dataset.</span><span class="sxs-lookup"><span data-stu-id="26df9-318">Next, define hello Azure Table dataset as follows.</span></span> <span data-ttu-id="26df9-319">Du behöver inte toospecify ”struktur” avsnitt med information om hello eftersom hello typinformation har redan angetts i hello underliggande datalagret.</span><span class="sxs-lookup"><span data-stu-id="26df9-319">You do not need toospecify “structure” section with hello type information since hello type information is already specified in hello underlying data store.</span></span>

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

<span data-ttu-id="26df9-320">I det här fallet Data Factory Skriv automatiskt konverteringar inklusive hello Datetime-fält med hello anpassade datetime-format med hjälp av hello ”fr-fr” kulturen när du flyttar data från Blob tooAzure tabell.</span><span class="sxs-lookup"><span data-stu-id="26df9-320">In this case, Data Factory automatically does type conversions including hello Datetime field with hello custom datetime format using hello "fr-fr" culture when moving data from Blob tooAzure Table.</span></span>

> [!NOTE]
> <span data-ttu-id="26df9-321">toomap kolumner från källan dataset toocolumns från sink dataset finns [mappa dataset kolumner i Azure Data Factory](data-factory-map-columns.md).</span><span class="sxs-lookup"><span data-stu-id="26df9-321">toomap columns from source dataset toocolumns from sink dataset, see [Mapping dataset columns in Azure Data Factory](data-factory-map-columns.md).</span></span>

## <a name="performance-and-tuning"></a><span data-ttu-id="26df9-322">Prestanda och finjustering</span><span class="sxs-lookup"><span data-stu-id="26df9-322">Performance and Tuning</span></span>
<span data-ttu-id="26df9-323">toolearn om nyckeln faktorer som påverkan prestanda för flytt av data (Kopieringsaktiviteten) i Azure Data Factory och olika sätt toooptimize, se [kopiera aktivitet prestanda och justera guiden](data-factory-copy-activity-performance.md).</span><span class="sxs-lookup"><span data-stu-id="26df9-323">toolearn about key factors that impact performance of data movement (Copy Activity) in Azure Data Factory and various ways toooptimize it, see [Copy Activity Performance & Tuning Guide](data-factory-copy-activity-performance.md).</span></span>
