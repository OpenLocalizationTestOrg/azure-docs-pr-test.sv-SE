---
title: "aaaCopy data till/från Azure SQL Data Warehouse | Microsoft Docs"
description: "Lär dig hur toocopy data till/från Azure SQL Data Warehouse med Azure Data Factory"
services: data-factory
documentationcenter: 
author: linda33wj
manager: jhubbard
editor: monicar
ms.assetid: d90fa9bd-4b79-458a-8d40-e896835cfd4a
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/04/2017
ms.author: jingwang
ms.openlocfilehash: 75bfcf3c99844fc1297ca500107da23cf875e41f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="copy-data-tooand-from-azure-sql-data-warehouse-using-azure-data-factory"></a><span data-ttu-id="3e79f-103">Kopiera data tooand från Azure SQL Data Warehouse med Azure Data Factory</span><span class="sxs-lookup"><span data-stu-id="3e79f-103">Copy data tooand from Azure SQL Data Warehouse using Azure Data Factory</span></span>
<span data-ttu-id="3e79f-104">Den här artikeln förklarar hur toouse hello Kopieringsaktiviteten i Azure Data Factory toomove data till och från Azure SQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="3e79f-104">This article explains how toouse hello Copy Activity in Azure Data Factory toomove data to/from Azure SQL Data Warehouse.</span></span> <span data-ttu-id="3e79f-105">Den bygger på hello [Data Movement aktiviteter](data-factory-data-movement-activities.md) artikel som ger en allmän översikt över dataflyttning hello kopieringsaktiviteten.</span><span class="sxs-lookup"><span data-stu-id="3e79f-105">It builds on hello [Data Movement Activities](data-factory-data-movement-activities.md) article, which presents a general overview of data movement with hello copy activity.</span></span>  

> [!TIP]
> <span data-ttu-id="3e79f-106">tooachieve bästa prestanda, använda PolyBase tooload data till Azure SQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="3e79f-106">tooachieve best performance, use PolyBase tooload data into Azure SQL Data Warehouse.</span></span> <span data-ttu-id="3e79f-107">Hej [Använd PolyBase tooload data till Azure SQL Data Warehouse](data-factory-azure-sql-data-warehouse-connector.md#use-polybase-to-load-data-into-azure-sql-data-warehouse) avsnitt innehåller information.</span><span class="sxs-lookup"><span data-stu-id="3e79f-107">hello [Use PolyBase tooload data into Azure SQL Data Warehouse](data-factory-azure-sql-data-warehouse-connector.md#use-polybase-to-load-data-into-azure-sql-data-warehouse) section has details.</span></span> <span data-ttu-id="3e79f-108">En genomgång med ett användningsfall finns [läsa in 1 TB i Azure SQL Data Warehouse under 15 minuter med Azure Data Factory](data-factory-load-sql-data-warehouse.md).</span><span class="sxs-lookup"><span data-stu-id="3e79f-108">For a walkthrough with a use case, see [Load 1 TB into Azure SQL Data Warehouse under 15 minutes with Azure Data Factory](data-factory-load-sql-data-warehouse.md).</span></span>

## <a name="supported-scenarios"></a><span data-ttu-id="3e79f-109">Scenarier som stöds</span><span class="sxs-lookup"><span data-stu-id="3e79f-109">Supported scenarios</span></span>
<span data-ttu-id="3e79f-110">Du kan kopiera data **från Azure SQL Data Warehouse** toohello följande datakällor:</span><span class="sxs-lookup"><span data-stu-id="3e79f-110">You can copy data **from Azure SQL Data Warehouse** toohello following data stores:</span></span>

[!INCLUDE [data-factory-supported-sinks](../../includes/data-factory-supported-sinks.md)]

<span data-ttu-id="3e79f-111">Du kan kopiera data från hello följande datalager **tooAzure SQL Data Warehouse**:</span><span class="sxs-lookup"><span data-stu-id="3e79f-111">You can copy data from hello following data stores **tooAzure SQL Data Warehouse**:</span></span>

[!INCLUDE [data-factory-supported-sources](../../includes/data-factory-supported-sources.md)]

> [!TIP]
> <span data-ttu-id="3e79f-112">När du kopierar data från SQL Server eller Azure SQL Database tooAzure kan SQL Data Warehouse, om hello tabellen inte finns i hello målarkivet, Data Factory automatiskt skapa hello tabell i SQL Data Warehouse med hjälp av hello schemat för hello tabell i hello källan datalagret.</span><span class="sxs-lookup"><span data-stu-id="3e79f-112">When copying data from SQL Server or Azure SQL Database tooAzure SQL Data Warehouse, if hello table does not exist in hello destination store, Data Factory can automatically create hello table in SQL Data Warehouse by using hello schema of hello table in hello source data store.</span></span> <span data-ttu-id="3e79f-113">Se [automatiskt skapande av tabell](#auto-table-creation) mer information.</span><span class="sxs-lookup"><span data-stu-id="3e79f-113">See [Auto table creation](#auto-table-creation) for details.</span></span>

## <a name="supported-authentication-type"></a><span data-ttu-id="3e79f-114">Autentiseringstypen som stöds</span><span class="sxs-lookup"><span data-stu-id="3e79f-114">Supported authentication type</span></span>
<span data-ttu-id="3e79f-115">Azure SQL Data Warehouse connector stöder grundläggande autentisering.</span><span class="sxs-lookup"><span data-stu-id="3e79f-115">Azure SQL Data Warehouse connector support basic authentication.</span></span>

## <a name="getting-started"></a><span data-ttu-id="3e79f-116">Komma igång</span><span class="sxs-lookup"><span data-stu-id="3e79f-116">Getting started</span></span>
<span data-ttu-id="3e79f-117">Du kan skapa en pipeline med en kopia-aktivitet som flyttar data till och från ett Azure SQL Data Warehouse med hjälp av olika verktyg/API: er.</span><span class="sxs-lookup"><span data-stu-id="3e79f-117">You can create a pipeline with a copy activity that moves data to/from an Azure SQL Data Warehouse by using different tools/APIs.</span></span>

<span data-ttu-id="3e79f-118">hello enklaste sättet toocreate en pipeline som kopierar data till och från Azure SQL Data Warehouse är guiden för toouse hello kopiera data.</span><span class="sxs-lookup"><span data-stu-id="3e79f-118">hello easiest way toocreate a pipeline that copies data to/from Azure SQL Data Warehouse is toouse hello Copy data wizard.</span></span> <span data-ttu-id="3e79f-119">Se [Självstudier: Läs in data till SQL Data Warehouse med Data Factory](../sql-data-warehouse/sql-data-warehouse-load-with-data-factory.md) för en snabb genomgång om hur du skapar en pipeline med hjälp av guiden för hello kopiera data.</span><span class="sxs-lookup"><span data-stu-id="3e79f-119">See [Tutorial: Load data into SQL Data Warehouse with Data Factory](../sql-data-warehouse/sql-data-warehouse-load-with-data-factory.md) for a quick walkthrough on creating a pipeline using hello Copy data wizard.</span></span>

<span data-ttu-id="3e79f-120">Du kan också använda följande verktyg toocreate en pipeline hello: **Azure-portalen**, **Visual Studio**, **Azure PowerShell**, **Azure Resource Manager-mall** , **.NET API**, och **REST API**.</span><span class="sxs-lookup"><span data-stu-id="3e79f-120">You can also use hello following tools toocreate a pipeline: **Azure portal**, **Visual Studio**, **Azure PowerShell**, **Azure Resource Manager template**, **.NET API**, and **REST API**.</span></span> <span data-ttu-id="3e79f-121">Se [kopiera aktivitet kursen](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) för stegvisa instruktioner toocreate en pipeline med en Kopieringsaktivitet.</span><span class="sxs-lookup"><span data-stu-id="3e79f-121">See [Copy activity tutorial](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) for step-by-step instructions toocreate a pipeline with a copy activity.</span></span>

<span data-ttu-id="3e79f-122">Om du använder hello verktyg eller API: er kan utföra du hello följande steg toocreate en pipeline som flyttar data från en källdata lagra tooa sink-datalagret:</span><span class="sxs-lookup"><span data-stu-id="3e79f-122">Whether you use hello tools or APIs, you perform hello following steps toocreate a pipeline that moves data from a source data store tooa sink data store:</span></span>

1. <span data-ttu-id="3e79f-123">Skapa en **datafabriken**.</span><span class="sxs-lookup"><span data-stu-id="3e79f-123">Create a **data factory**.</span></span> <span data-ttu-id="3e79f-124">En datafabrik kan innehålla en eller flera pipelines.</span><span class="sxs-lookup"><span data-stu-id="3e79f-124">A data factory may contain one or more pipelines.</span></span> 
2. <span data-ttu-id="3e79f-125">Skapa **länkade tjänster** toolink indata och utdata lagrar tooyour data factory.</span><span class="sxs-lookup"><span data-stu-id="3e79f-125">Create **linked services** toolink input and output data stores tooyour data factory.</span></span> <span data-ttu-id="3e79f-126">Till exempel om du kopierar data från ett Azure blob storage tooan Azure SQL data warehouse skapa du två länkade tjänster toolink dina Azure storage-konto och Azure SQL data warehouse tooyour data factory.</span><span class="sxs-lookup"><span data-stu-id="3e79f-126">For example, if you are copying data from an Azure blob storage tooan Azure SQL data warehouse, you create two linked services toolink your Azure storage account and Azure SQL data warehouse tooyour data factory.</span></span> <span data-ttu-id="3e79f-127">Länkad tjänstegenskaper som är specifika tooAzure SQL Data Warehouse, se [länkade tjänstegenskaper](#linked-service-properties) avsnitt.</span><span class="sxs-lookup"><span data-stu-id="3e79f-127">For linked service properties that are specific tooAzure SQL Data Warehouse, see [linked service properties](#linked-service-properties) section.</span></span> 
3. <span data-ttu-id="3e79f-128">Skapa **datauppsättningar** toorepresent indata och utdata för hello kopieringsåtgärden.</span><span class="sxs-lookup"><span data-stu-id="3e79f-128">Create **datasets** toorepresent input and output data for hello copy operation.</span></span> <span data-ttu-id="3e79f-129">I hello exempelvis nämns i hello sista steget skapar du en dataset toospecify hello blob-behållaren och mappen som innehåller hello indata.</span><span class="sxs-lookup"><span data-stu-id="3e79f-129">In hello example mentioned in hello last step, you create a dataset toospecify hello blob container and folder that contains hello input data.</span></span> <span data-ttu-id="3e79f-130">Och du skapar en annan dataset toospecify hello tabell i hello Azure SQL data warehouse som innehåller hello data som kopieras från hello blob storage.</span><span class="sxs-lookup"><span data-stu-id="3e79f-130">And, you create another dataset toospecify hello table in hello Azure SQL data warehouse that holds hello data copied from hello blob storage.</span></span> <span data-ttu-id="3e79f-131">Egenskaper för datamängd som är specifika tooAzure SQL Data Warehouse, se [egenskaper för datamängd](#dataset-properties) avsnitt.</span><span class="sxs-lookup"><span data-stu-id="3e79f-131">For dataset properties that are specific tooAzure SQL Data Warehouse, see [dataset properties](#dataset-properties) section.</span></span>
4. <span data-ttu-id="3e79f-132">Skapa en **pipeline** med en kopia-aktivitet som tar en datamängd som indata och en dataset som utdata.</span><span class="sxs-lookup"><span data-stu-id="3e79f-132">Create a **pipeline** with a copy activity that takes a dataset as an input and a dataset as an output.</span></span> <span data-ttu-id="3e79f-133">I hello-exemplet ovan, använder du BlobSource som en källa och SqlDWSink som en mottagare för hello kopieringsaktiviteten.</span><span class="sxs-lookup"><span data-stu-id="3e79f-133">In hello example mentioned earlier, you use BlobSource as a source and SqlDWSink as a sink for hello copy activity.</span></span> <span data-ttu-id="3e79f-134">På samma sätt om du vill kopiera från Azure SQL Data Warehouse tooAzure Blob Storage, använder du SqlDWSource och BlobSink i hello kopieringsaktiviteten.</span><span class="sxs-lookup"><span data-stu-id="3e79f-134">Similarly, if you are copying from Azure SQL Data Warehouse tooAzure Blob Storage, you use SqlDWSource and BlobSink in hello copy activity.</span></span> <span data-ttu-id="3e79f-135">Kopiera Aktivitetsegenskaper som är specifika tooAzure SQL Data Warehouse, se [kopiera Aktivitetsegenskaper](#copy-activity-properties) avsnitt.</span><span class="sxs-lookup"><span data-stu-id="3e79f-135">For copy activity properties that are specific tooAzure SQL Data Warehouse, see [copy activity properties](#copy-activity-properties) section.</span></span> <span data-ttu-id="3e79f-136">Mer information om hur toouse en databas som en källa eller en mottagare klickar du på hello länk under föregående hello för datalager.</span><span class="sxs-lookup"><span data-stu-id="3e79f-136">For details on how toouse a data store as a source or a sink, click hello link in hello previous section for your data store.</span></span>

<span data-ttu-id="3e79f-137">När du använder guiden hello skapas automatiskt JSON definitioner för dessa Data Factory-enheter (länkade tjänster, datauppsättningar och hello pipeline) för dig.</span><span class="sxs-lookup"><span data-stu-id="3e79f-137">When you use hello wizard, JSON definitions for these Data Factory entities (linked services, datasets, and hello pipeline) are automatically created for you.</span></span> <span data-ttu-id="3e79f-138">När du använder Verktyg/API: er (utom .NET API), kan du definiera dessa Data Factory-enheter med hjälp av hello JSON-format.</span><span class="sxs-lookup"><span data-stu-id="3e79f-138">When you use tools/APIs (except .NET API), you define these Data Factory entities by using hello JSON format.</span></span>  <span data-ttu-id="3e79f-139">Exempel med JSON-definitioner för Data Factory-entiteter som har använt toocopy data till/från en Azure SQL Data Warehouse finns [JSON-exempel](#json-examples-for-copying-data-to-and-from-sql-data-warehouse) i den här artikeln.</span><span class="sxs-lookup"><span data-stu-id="3e79f-139">For samples with JSON definitions for Data Factory entities that are used toocopy data to/from an Azure SQL Data Warehouse, see [JSON examples](#json-examples-for-copying-data-to-and-from-sql-data-warehouse) section of this article.</span></span>

<span data-ttu-id="3e79f-140">hello följande avsnitt innehåller information om JSON-egenskaper används toodefine Data Factory entiteter specifika tooAzure SQL Data Warehouse:</span><span class="sxs-lookup"><span data-stu-id="3e79f-140">hello following sections provide details about JSON properties that are used toodefine Data Factory entities specific tooAzure SQL Data Warehouse:</span></span>

## <a name="linked-service-properties"></a><span data-ttu-id="3e79f-141">Länkad tjänstegenskaper</span><span class="sxs-lookup"><span data-stu-id="3e79f-141">Linked service properties</span></span>
<span data-ttu-id="3e79f-142">hello följande tabell ger en beskrivning för JSON-element specifika tooAzure SQL Data Warehouse länkade tjänsten.</span><span class="sxs-lookup"><span data-stu-id="3e79f-142">hello following table provides description for JSON elements specific tooAzure SQL Data Warehouse linked service.</span></span>

| <span data-ttu-id="3e79f-143">Egenskap</span><span class="sxs-lookup"><span data-stu-id="3e79f-143">Property</span></span> | <span data-ttu-id="3e79f-144">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="3e79f-144">Description</span></span> | <span data-ttu-id="3e79f-145">Krävs</span><span class="sxs-lookup"><span data-stu-id="3e79f-145">Required</span></span> |
| --- | --- | --- |
| <span data-ttu-id="3e79f-146">typ</span><span class="sxs-lookup"><span data-stu-id="3e79f-146">type</span></span> |<span data-ttu-id="3e79f-147">hello Typegenskapen måste anges till: **AzureSqlDW**</span><span class="sxs-lookup"><span data-stu-id="3e79f-147">hello type property must be set to: **AzureSqlDW**</span></span> |<span data-ttu-id="3e79f-148">Ja</span><span class="sxs-lookup"><span data-stu-id="3e79f-148">Yes</span></span> |
| <span data-ttu-id="3e79f-149">connectionString</span><span class="sxs-lookup"><span data-stu-id="3e79f-149">connectionString</span></span> |<span data-ttu-id="3e79f-150">Ange nödvändig information tooconnect toohello Azure SQL Data Warehouse-instans för hello-egenskapen connectionString.</span><span class="sxs-lookup"><span data-stu-id="3e79f-150">Specify information needed tooconnect toohello Azure SQL Data Warehouse instance for hello connectionString property.</span></span> <span data-ttu-id="3e79f-151">Endast grundläggande autentisering stöds.</span><span class="sxs-lookup"><span data-stu-id="3e79f-151">Only basic authentication is supported.</span></span> |<span data-ttu-id="3e79f-152">Ja</span><span class="sxs-lookup"><span data-stu-id="3e79f-152">Yes</span></span> |

> [!IMPORTANT]
> <span data-ttu-id="3e79f-153">Konfigurera [Azure SQL Database-brandvägg](https://msdn.microsoft.com/library/azure/ee621782.aspx#ConnectingFromAzure) och hello databasserver för[Tillåt Azure Services tooaccess hello server](https://msdn.microsoft.com/library/azure/ee621782.aspx#ConnectingFromAzure).</span><span class="sxs-lookup"><span data-stu-id="3e79f-153">Configure [Azure SQL Database Firewall](https://msdn.microsoft.com/library/azure/ee621782.aspx#ConnectingFromAzure) and hello database server too[allow Azure Services tooaccess hello server](https://msdn.microsoft.com/library/azure/ee621782.aspx#ConnectingFromAzure).</span></span> <span data-ttu-id="3e79f-154">Om du kopierar data tooAzure SQL Data Warehouse från utanför Azure inklusive från lokala datakällor med data factory-gateway måste du dessutom konfigurera rätt IP-adressintervall för hello-dator som skickar data tooAzure SQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="3e79f-154">Additionally, if you are copying data tooAzure SQL Data Warehouse from outside Azure including from on-premises data sources with data factory gateway, configure appropriate IP address range for hello machine that is sending data tooAzure SQL Data Warehouse.</span></span>

## <a name="dataset-properties"></a><span data-ttu-id="3e79f-155">Egenskaper för datamängd</span><span class="sxs-lookup"><span data-stu-id="3e79f-155">Dataset properties</span></span>
<span data-ttu-id="3e79f-156">En fullständig lista över egenskaper som är tillgängliga för att definiera datauppsättningarna & avsnitt finns hello [skapa datauppsättningar](data-factory-create-datasets.md) artikel.</span><span class="sxs-lookup"><span data-stu-id="3e79f-156">For a full list of sections & properties available for defining datasets, see hello [Creating datasets](data-factory-create-datasets.md) article.</span></span> <span data-ttu-id="3e79f-157">Avsnitt som struktur, tillgänglighet och princip på en datamängd JSON är liknande för alla typer av dataset (Azure SQL Azure blob, Azure-tabellen, osv.).</span><span class="sxs-lookup"><span data-stu-id="3e79f-157">Sections such as structure, availability, and policy of a dataset JSON are similar for all dataset types (Azure SQL, Azure blob, Azure table, etc.).</span></span>

<span data-ttu-id="3e79f-158">hello typeProperties avsnittet är olika för varje typ av dataset och ger information om hello platsen för hello data i datalagret hello.</span><span class="sxs-lookup"><span data-stu-id="3e79f-158">hello typeProperties section is different for each type of dataset and provides information about hello location of hello data in hello data store.</span></span> <span data-ttu-id="3e79f-159">Hej **typeProperties** avsnittet för hello dataset av typen **AzureSqlDWTable** har hello följande egenskaper:</span><span class="sxs-lookup"><span data-stu-id="3e79f-159">hello **typeProperties** section for hello dataset of type **AzureSqlDWTable** has hello following properties:</span></span>

| <span data-ttu-id="3e79f-160">Egenskap</span><span class="sxs-lookup"><span data-stu-id="3e79f-160">Property</span></span> | <span data-ttu-id="3e79f-161">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="3e79f-161">Description</span></span> | <span data-ttu-id="3e79f-162">Krävs</span><span class="sxs-lookup"><span data-stu-id="3e79f-162">Required</span></span> |
| --- | --- | --- |
| <span data-ttu-id="3e79f-163">tableName</span><span class="sxs-lookup"><span data-stu-id="3e79f-163">tableName</span></span> |<span data-ttu-id="3e79f-164">Namnet på hello tabellen eller vyn i hello Azure SQL Data Warehouse-databas som hello länkade tjänsten refererar till.</span><span class="sxs-lookup"><span data-stu-id="3e79f-164">Name of hello table or view in hello Azure SQL Data Warehouse database that hello linked service refers to.</span></span> |<span data-ttu-id="3e79f-165">Ja</span><span class="sxs-lookup"><span data-stu-id="3e79f-165">Yes</span></span> |

## <a name="copy-activity-properties"></a><span data-ttu-id="3e79f-166">Kopiera egenskaper för aktivitet</span><span class="sxs-lookup"><span data-stu-id="3e79f-166">Copy activity properties</span></span>
<span data-ttu-id="3e79f-167">En fullständig lista över avsnitt & egenskaper som är tillgängliga för att definiera aktiviteter finns hello [skapar Pipelines](data-factory-create-pipelines.md) artikel.</span><span class="sxs-lookup"><span data-stu-id="3e79f-167">For a full list of sections & properties available for defining activities, see hello [Creating Pipelines](data-factory-create-pipelines.md) article.</span></span> <span data-ttu-id="3e79f-168">Egenskaper som namn, beskrivning, ingående och utgående tabeller och principen är tillgängliga för alla typer av aktiviteter.</span><span class="sxs-lookup"><span data-stu-id="3e79f-168">Properties such as name, description, input and output tables, and policy are available for all types of activities.</span></span>

> [!NOTE]
> <span data-ttu-id="3e79f-169">Hej Kopieringsaktiviteten tar endast en inmatning och ger en enda utdata.</span><span class="sxs-lookup"><span data-stu-id="3e79f-169">hello Copy Activity takes only one input and produces only one output.</span></span>

<span data-ttu-id="3e79f-170">De egenskaper som är tillgängliga under hello typeProperties i hello aktivitet varierar beroende på varje aktivitetstyp.</span><span class="sxs-lookup"><span data-stu-id="3e79f-170">Whereas, properties available in hello typeProperties section of hello activity vary with each activity type.</span></span> <span data-ttu-id="3e79f-171">För Kopieringsaktivitet kan varierar de beroende på hello typer av datakällor och sänkor.</span><span class="sxs-lookup"><span data-stu-id="3e79f-171">For Copy activity, they vary depending on hello types of sources and sinks.</span></span>

### <a name="sqldwsource"></a><span data-ttu-id="3e79f-172">SqlDWSource</span><span class="sxs-lookup"><span data-stu-id="3e79f-172">SqlDWSource</span></span>
<span data-ttu-id="3e79f-173">När datakällan är av typen **SqlDWSource**, hello följande egenskaper är tillgängliga i **typeProperties** avsnitt:</span><span class="sxs-lookup"><span data-stu-id="3e79f-173">When source is of type **SqlDWSource**, hello following properties are available in **typeProperties** section:</span></span>

| <span data-ttu-id="3e79f-174">Egenskap</span><span class="sxs-lookup"><span data-stu-id="3e79f-174">Property</span></span> | <span data-ttu-id="3e79f-175">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="3e79f-175">Description</span></span> | <span data-ttu-id="3e79f-176">Tillåtna värden</span><span class="sxs-lookup"><span data-stu-id="3e79f-176">Allowed values</span></span> | <span data-ttu-id="3e79f-177">Krävs</span><span class="sxs-lookup"><span data-stu-id="3e79f-177">Required</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="3e79f-178">sqlReaderQuery</span><span class="sxs-lookup"><span data-stu-id="3e79f-178">sqlReaderQuery</span></span> |<span data-ttu-id="3e79f-179">Använda hello anpassad fråga tooread data.</span><span class="sxs-lookup"><span data-stu-id="3e79f-179">Use hello custom query tooread data.</span></span> |<span data-ttu-id="3e79f-180">SQL-sträng.</span><span class="sxs-lookup"><span data-stu-id="3e79f-180">SQL query string.</span></span> <span data-ttu-id="3e79f-181">Till exempel: Välj * från mytable prefix.</span><span class="sxs-lookup"><span data-stu-id="3e79f-181">For example: select * from MyTable.</span></span> |<span data-ttu-id="3e79f-182">Nej</span><span class="sxs-lookup"><span data-stu-id="3e79f-182">No</span></span> |
| <span data-ttu-id="3e79f-183">sqlReaderStoredProcedureName</span><span class="sxs-lookup"><span data-stu-id="3e79f-183">sqlReaderStoredProcedureName</span></span> |<span data-ttu-id="3e79f-184">Namnet på hello lagrad procedur som läser data från hello källtabellen.</span><span class="sxs-lookup"><span data-stu-id="3e79f-184">Name of hello stored procedure that reads data from hello source table.</span></span> |<span data-ttu-id="3e79f-185">Namnet på hello lagrad procedur.</span><span class="sxs-lookup"><span data-stu-id="3e79f-185">Name of hello stored procedure.</span></span> <span data-ttu-id="3e79f-186">hello senaste SQL-instruktionen måste vara en SELECT-instruktion i hello lagrad procedur.</span><span class="sxs-lookup"><span data-stu-id="3e79f-186">hello last SQL statement must be a SELECT statement in hello stored procedure.</span></span> |<span data-ttu-id="3e79f-187">Nej</span><span class="sxs-lookup"><span data-stu-id="3e79f-187">No</span></span> |
| <span data-ttu-id="3e79f-188">storedProcedureParameters</span><span class="sxs-lookup"><span data-stu-id="3e79f-188">storedProcedureParameters</span></span> |<span data-ttu-id="3e79f-189">Parametrar för hello lagrad procedur.</span><span class="sxs-lookup"><span data-stu-id="3e79f-189">Parameters for hello stored procedure.</span></span> |<span data-ttu-id="3e79f-190">Namn/värde-par.</span><span class="sxs-lookup"><span data-stu-id="3e79f-190">Name/value pairs.</span></span> <span data-ttu-id="3e79f-191">Namn och skiftläge parametrar måste matcha hello namn och versaler och gemener i hello lagrade procedurparametrar.</span><span class="sxs-lookup"><span data-stu-id="3e79f-191">Names and casing of parameters must match hello names and casing of hello stored procedure parameters.</span></span> |<span data-ttu-id="3e79f-192">Nej</span><span class="sxs-lookup"><span data-stu-id="3e79f-192">No</span></span> |

<span data-ttu-id="3e79f-193">Om hello **sqlReaderQuery** har angetts för hello SqlDWSource, hello Kopieringsaktiviteten kör den här frågan mot hello Azure SQL Data Warehouse källdata tooget hello.</span><span class="sxs-lookup"><span data-stu-id="3e79f-193">If hello **sqlReaderQuery** is specified for hello SqlDWSource, hello Copy Activity runs this query against hello Azure SQL Data Warehouse source tooget hello data.</span></span>

<span data-ttu-id="3e79f-194">Du kan också ange en lagrad procedur genom att ange hello **sqlReaderStoredProcedureName** och **storedProcedureParameters** (om hello tar lagrade proceduren parametrar).</span><span class="sxs-lookup"><span data-stu-id="3e79f-194">Alternatively, you can specify a stored procedure by specifying hello **sqlReaderStoredProcedureName** and **storedProcedureParameters** (if hello stored procedure takes parameters).</span></span>

<span data-ttu-id="3e79f-195">Om du inte anger sqlReaderQuery eller sqlReaderStoredProcedureName är hello kolumner som anges i hello strukturen i hello dataset JSON används toobuild en fråga toorun mot hello Azure SQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="3e79f-195">If you do not specify either sqlReaderQuery or sqlReaderStoredProcedureName, hello columns defined in hello structure section of hello dataset JSON are used toobuild a query toorun against hello Azure SQL Data Warehouse.</span></span> <span data-ttu-id="3e79f-196">Exempel: `select column1, column2 from mytable`.</span><span class="sxs-lookup"><span data-stu-id="3e79f-196">Example: `select column1, column2 from mytable`.</span></span> <span data-ttu-id="3e79f-197">Om hello datauppsättningsdefinitionen inte har hello struktur, markeras alla kolumner från tabellen hello.</span><span class="sxs-lookup"><span data-stu-id="3e79f-197">If hello dataset definition does not have hello structure, all columns are selected from hello table.</span></span>

#### <a name="sqldwsource-example"></a><span data-ttu-id="3e79f-198">SqlDWSource-exempel</span><span class="sxs-lookup"><span data-stu-id="3e79f-198">SqlDWSource example</span></span>

```JSON
"source": {
    "type": "SqlDWSource",
    "sqlReaderStoredProcedureName": "CopyTestSrcStoredProcedureWithParameters",
    "storedProcedureParameters": {
        "stringData": { "value": "str3" },
        "identifier": { "value": "$$Text.Format('{0:yyyy}', SliceStart)", "type": "Int"}
    }
}
```
<span data-ttu-id="3e79f-199">**definition av hello lagrade proceduren:**</span><span class="sxs-lookup"><span data-stu-id="3e79f-199">**hello stored procedure definition:**</span></span>

```SQL
CREATE PROCEDURE CopyTestSrcStoredProcedureWithParameters
(
    @stringData varchar(20),
    @identifier int
)
AS
SET NOCOUNT ON;
BEGIN
     select *
     from dbo.UnitTestSrcTable
     where dbo.UnitTestSrcTable.stringData != stringData
    and dbo.UnitTestSrcTable.identifier != identifier
END
GO
```

### <a name="sqldwsink"></a><span data-ttu-id="3e79f-200">SqlDWSink</span><span class="sxs-lookup"><span data-stu-id="3e79f-200">SqlDWSink</span></span>
<span data-ttu-id="3e79f-201">**SqlDWSink** stöder hello följande egenskaper:</span><span class="sxs-lookup"><span data-stu-id="3e79f-201">**SqlDWSink** supports hello following properties:</span></span>

| <span data-ttu-id="3e79f-202">Egenskap</span><span class="sxs-lookup"><span data-stu-id="3e79f-202">Property</span></span> | <span data-ttu-id="3e79f-203">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="3e79f-203">Description</span></span> | <span data-ttu-id="3e79f-204">Tillåtna värden</span><span class="sxs-lookup"><span data-stu-id="3e79f-204">Allowed values</span></span> | <span data-ttu-id="3e79f-205">Krävs</span><span class="sxs-lookup"><span data-stu-id="3e79f-205">Required</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="3e79f-206">sqlWriterCleanupScript</span><span class="sxs-lookup"><span data-stu-id="3e79f-206">sqlWriterCleanupScript</span></span> |<span data-ttu-id="3e79f-207">Ange en fråga för aktiviteten kopiera tooexecute så att data för ett visst segment har rensats bort.</span><span class="sxs-lookup"><span data-stu-id="3e79f-207">Specify a query for Copy Activity tooexecute such that data of a specific slice is cleaned up.</span></span> <span data-ttu-id="3e79f-208">Mer information finns i [repeterbarhet avsnittet](#repeatability-during-copy).</span><span class="sxs-lookup"><span data-stu-id="3e79f-208">For details, see [repeatability section](#repeatability-during-copy).</span></span> |<span data-ttu-id="3e79f-209">En frågesats.</span><span class="sxs-lookup"><span data-stu-id="3e79f-209">A query statement.</span></span> |<span data-ttu-id="3e79f-210">Nej</span><span class="sxs-lookup"><span data-stu-id="3e79f-210">No</span></span> |
| <span data-ttu-id="3e79f-211">allowPolyBase</span><span class="sxs-lookup"><span data-stu-id="3e79f-211">allowPolyBase</span></span> |<span data-ttu-id="3e79f-212">Anger om toouse PolyBase (i förekommande fall) i stället för BULKINSERT mekanism.</span><span class="sxs-lookup"><span data-stu-id="3e79f-212">Indicates whether toouse PolyBase (when applicable) instead of BULKINSERT mechanism.</span></span> <br/><br/> <span data-ttu-id="3e79f-213">**Hello rekommenderat sätt tooload data till SQL Data Warehouse är med PolyBase.**</span><span class="sxs-lookup"><span data-stu-id="3e79f-213">**Using PolyBase is hello recommended way tooload data into SQL Data Warehouse.**</span></span> <span data-ttu-id="3e79f-214">Se [Använd PolyBase tooload data till Azure SQL Data Warehouse](#use-polybase-to-load-data-into-azure-sql-data-warehouse) avsnittet begränsningar och information.</span><span class="sxs-lookup"><span data-stu-id="3e79f-214">See [Use PolyBase tooload data into Azure SQL Data Warehouse](#use-polybase-to-load-data-into-azure-sql-data-warehouse) section for constraints and details.</span></span> |<span data-ttu-id="3e79f-215">True</span><span class="sxs-lookup"><span data-stu-id="3e79f-215">True</span></span> <br/><span data-ttu-id="3e79f-216">FALSKT (standard)</span><span class="sxs-lookup"><span data-stu-id="3e79f-216">False (default)</span></span> |<span data-ttu-id="3e79f-217">Nej</span><span class="sxs-lookup"><span data-stu-id="3e79f-217">No</span></span> |
| <span data-ttu-id="3e79f-218">polyBaseSettings</span><span class="sxs-lookup"><span data-stu-id="3e79f-218">polyBaseSettings</span></span> |<span data-ttu-id="3e79f-219">En grupp egenskaper som kan anges när hello **allowPolybase** egenskapen för**SANT**.</span><span class="sxs-lookup"><span data-stu-id="3e79f-219">A group of properties that can be specified when hello **allowPolybase** property is set too**true**.</span></span> |&nbsp; |<span data-ttu-id="3e79f-220">Nej</span><span class="sxs-lookup"><span data-stu-id="3e79f-220">No</span></span> |
| <span data-ttu-id="3e79f-221">rejectValue</span><span class="sxs-lookup"><span data-stu-id="3e79f-221">rejectValue</span></span> |<span data-ttu-id="3e79f-222">Anger antalet hello eller procentandelen rader som kan avvisas innan hello frågan inte kunde köras.</span><span class="sxs-lookup"><span data-stu-id="3e79f-222">Specifies hello number or percentage of rows that can be rejected before hello query fails.</span></span> <br/><br/><span data-ttu-id="3e79f-223">Lär dig mer om hello PolyBase avvisa alternativ i hello **argument** avsnitt i [Skapa extern tabell (Transact-SQL)](https://msdn.microsoft.com/library/dn935021.aspx) avsnittet.</span><span class="sxs-lookup"><span data-stu-id="3e79f-223">Learn more about hello PolyBase’s reject options in hello **Arguments** section of [CREATE EXTERNAL TABLE (Transact-SQL)](https://msdn.microsoft.com/library/dn935021.aspx) topic.</span></span> |<span data-ttu-id="3e79f-224">0 (standard), 1, 2...</span><span class="sxs-lookup"><span data-stu-id="3e79f-224">0 (default), 1, 2, …</span></span> |<span data-ttu-id="3e79f-225">Nej</span><span class="sxs-lookup"><span data-stu-id="3e79f-225">No</span></span> |
| <span data-ttu-id="3e79f-226">rejectType</span><span class="sxs-lookup"><span data-stu-id="3e79f-226">rejectType</span></span> |<span data-ttu-id="3e79f-227">Anger om hello rejectValue alternativet har angetts som ett litteralvärde eller ett procentvärde.</span><span class="sxs-lookup"><span data-stu-id="3e79f-227">Specifies whether hello rejectValue option is specified as a literal value or a percentage.</span></span> |<span data-ttu-id="3e79f-228">Värdet (standard), procent</span><span class="sxs-lookup"><span data-stu-id="3e79f-228">Value (default), Percentage</span></span> |<span data-ttu-id="3e79f-229">Nej</span><span class="sxs-lookup"><span data-stu-id="3e79f-229">No</span></span> |
| <span data-ttu-id="3e79f-230">rejectSampleValue</span><span class="sxs-lookup"><span data-stu-id="3e79f-230">rejectSampleValue</span></span> |<span data-ttu-id="3e79f-231">Anger hello antalet rader tooretrieve innan hello PolyBase beräknar hello procentandelen Avvisade rader.</span><span class="sxs-lookup"><span data-stu-id="3e79f-231">Determines hello number of rows tooretrieve before hello PolyBase recalculates hello percentage of rejected rows.</span></span> |<span data-ttu-id="3e79f-232">1, 2, …</span><span class="sxs-lookup"><span data-stu-id="3e79f-232">1, 2, …</span></span> |<span data-ttu-id="3e79f-233">Ja, om **rejectType** är **procent**</span><span class="sxs-lookup"><span data-stu-id="3e79f-233">Yes, if **rejectType** is **percentage**</span></span> |
| <span data-ttu-id="3e79f-234">useTypeDefault</span><span class="sxs-lookup"><span data-stu-id="3e79f-234">useTypeDefault</span></span> |<span data-ttu-id="3e79f-235">Anger hur toohandle saknar värden i avgränsade textfiler när PolyBase hämtar data från hello textfil.</span><span class="sxs-lookup"><span data-stu-id="3e79f-235">Specifies how toohandle missing values in delimited text files when PolyBase retrieves data from hello text file.</span></span><br/><br/><span data-ttu-id="3e79f-236">Mer information om den här egenskapen från hello argument-avsnittet i [skapa externt FILFORMAT (Transact-SQL)](https://msdn.microsoft.com/library/dn935026.aspx).</span><span class="sxs-lookup"><span data-stu-id="3e79f-236">Learn more about this property from hello Arguments section in [CREATE EXTERNAL FILE FORMAT (Transact-SQL)](https://msdn.microsoft.com/library/dn935026.aspx).</span></span> |<span data-ttu-id="3e79f-237">SANT, FALSKT (standard)</span><span class="sxs-lookup"><span data-stu-id="3e79f-237">True, False (default)</span></span> |<span data-ttu-id="3e79f-238">Nej</span><span class="sxs-lookup"><span data-stu-id="3e79f-238">No</span></span> |
| <span data-ttu-id="3e79f-239">writeBatchSize</span><span class="sxs-lookup"><span data-stu-id="3e79f-239">writeBatchSize</span></span> |<span data-ttu-id="3e79f-240">Infogar data i hello SQL-tabellen när hello buffertstorlek når writeBatchSize</span><span class="sxs-lookup"><span data-stu-id="3e79f-240">Inserts data into hello SQL table when hello buffer size reaches writeBatchSize</span></span> |<span data-ttu-id="3e79f-241">Heltal (antalet rader)</span><span class="sxs-lookup"><span data-stu-id="3e79f-241">Integer (number of rows)</span></span> |<span data-ttu-id="3e79f-242">Nej (standard: 10000)</span><span class="sxs-lookup"><span data-stu-id="3e79f-242">No (default: 10000)</span></span> |
| <span data-ttu-id="3e79f-243">writeBatchTimeout</span><span class="sxs-lookup"><span data-stu-id="3e79f-243">writeBatchTimeout</span></span> |<span data-ttu-id="3e79f-244">Vänta tills hello batch insert-åtgärden toocomplete innan tidsgränsen uppnås.</span><span class="sxs-lookup"><span data-stu-id="3e79f-244">Wait time for hello batch insert operation toocomplete before it times out.</span></span> |<span data-ttu-id="3e79f-245">TimeSpan</span><span class="sxs-lookup"><span data-stu-id="3e79f-245">timespan</span></span><br/><br/> <span data-ttu-id="3e79f-246">Exempel ”: 00: 30:00” (30 minuter).</span><span class="sxs-lookup"><span data-stu-id="3e79f-246">Example: “00:30:00” (30 minutes).</span></span> |<span data-ttu-id="3e79f-247">Nej</span><span class="sxs-lookup"><span data-stu-id="3e79f-247">No</span></span> |

#### <a name="sqldwsink-example"></a><span data-ttu-id="3e79f-248">SqlDWSink-exempel</span><span class="sxs-lookup"><span data-stu-id="3e79f-248">SqlDWSink example</span></span>

```JSON
"sink": {
    "type": "SqlDWSink",
    "allowPolyBase": true
}
```

## <a name="use-polybase-tooload-data-into-azure-sql-data-warehouse"></a><span data-ttu-id="3e79f-249">Använd PolyBase tooload data till Azure SQL Data Warehouse</span><span class="sxs-lookup"><span data-stu-id="3e79f-249">Use PolyBase tooload data into Azure SQL Data Warehouse</span></span>
<span data-ttu-id="3e79f-250">Med hjälp av  **[PolyBase](https://docs.microsoft.com/sql/relational-databases/polybase/polybase-guide)**  är ett effektivt sätt för att läsa in stora mängder data till Azure SQL Data Warehouse med hög genomströmning.</span><span class="sxs-lookup"><span data-stu-id="3e79f-250">Using **[PolyBase](https://docs.microsoft.com/sql/relational-databases/polybase/polybase-guide)** is an efficient way of loading large amount of data into Azure SQL Data Warehouse with high throughput.</span></span> <span data-ttu-id="3e79f-251">Du kan se en stor vinst i hello dataflöde med PolyBase i stället för hello BULKINSERT standardmekanism.</span><span class="sxs-lookup"><span data-stu-id="3e79f-251">You can see a large gain in hello throughput by using PolyBase instead of hello default BULKINSERT mechanism.</span></span> <span data-ttu-id="3e79f-252">Se [kopiera prestanda referensnummer](data-factory-copy-activity-performance.md#performance-reference) med detaljerad jämförelse.</span><span class="sxs-lookup"><span data-stu-id="3e79f-252">See [copy performance reference number](data-factory-copy-activity-performance.md#performance-reference) with detailed comparison.</span></span> <span data-ttu-id="3e79f-253">En genomgång med ett användningsfall finns [läsa in 1 TB i Azure SQL Data Warehouse under 15 minuter med Azure Data Factory](data-factory-load-sql-data-warehouse.md).</span><span class="sxs-lookup"><span data-stu-id="3e79f-253">For a walkthrough with a use case, see [Load 1 TB into Azure SQL Data Warehouse under 15 minutes with Azure Data Factory](data-factory-load-sql-data-warehouse.md).</span></span>

* <span data-ttu-id="3e79f-254">Om datakällan finns i **Azure Blob- eller Azure Data Lake Store**, och hello format är kompatibelt med PolyBase, du kan kopiera tooAzure SQL Data Warehouse med PolyBase direkt.</span><span class="sxs-lookup"><span data-stu-id="3e79f-254">If your source data is in **Azure Blob or Azure Data Lake Store**, and hello format is compatible with PolyBase, you can directly copy tooAzure SQL Data Warehouse using PolyBase.</span></span> <span data-ttu-id="3e79f-255">Se  **[direkt kopia med PolyBase](#direct-copy-using-polybase)**  med information.</span><span class="sxs-lookup"><span data-stu-id="3e79f-255">See **[Direct copy using PolyBase](#direct-copy-using-polybase)** with details.</span></span>
* <span data-ttu-id="3e79f-256">Om din käll-datalagret och format inte stöds ursprungligen av PolyBase, kan du använda hello  **[mellanlagrad kopia med PolyBase](#staged-copy-using-polybase)**  funktion i stället.</span><span class="sxs-lookup"><span data-stu-id="3e79f-256">If your source data store and format is not originally supported by PolyBase, you can use hello **[Staged Copy using PolyBase](#staged-copy-using-polybase)** feature instead.</span></span> <span data-ttu-id="3e79f-257">Det ger dig bättre genomströmning genom att automatiskt konvertera hello data till PolyBase-kompatibelt format och lagra hello data i Azure Blob storage.</span><span class="sxs-lookup"><span data-stu-id="3e79f-257">It also provides you better throughput by automatically converting hello data into PolyBase-compatible format and storing hello data in Azure Blob storage.</span></span> <span data-ttu-id="3e79f-258">Data hämtas sedan till SQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="3e79f-258">It then loads data into SQL Data Warehouse.</span></span>

<span data-ttu-id="3e79f-259">Ange hello `allowPolyBase` egenskapen för**SANT** som visas i följande exempel för Azure Data Factory toouse PolyBase toocopy data till Azure SQL Data Warehouse hello.</span><span class="sxs-lookup"><span data-stu-id="3e79f-259">Set hello `allowPolyBase` property too**true** as shown in hello following example for Azure Data Factory toouse PolyBase toocopy data into Azure SQL Data Warehouse.</span></span> <span data-ttu-id="3e79f-260">När du ställer in allowPolyBase tootrue, anger du PolyBase specifika egenskaper med hello `polyBaseSettings` egenskapsgrupp.</span><span class="sxs-lookup"><span data-stu-id="3e79f-260">When you set allowPolyBase tootrue, you can specify PolyBase specific properties using hello `polyBaseSettings` property group.</span></span> <span data-ttu-id="3e79f-261">Se hello [SqlDWSink](#SqlDWSink) finns mer information om egenskaper som du kan använda med polyBaseSettings.</span><span class="sxs-lookup"><span data-stu-id="3e79f-261">see hello [SqlDWSink](#SqlDWSink) section for details about properties that you can use with polyBaseSettings.</span></span>

```JSON
"sink": {
    "type": "SqlDWSink",
    "allowPolyBase": true,
    "polyBaseSettings":
    {
        "rejectType": "percentage",
        "rejectValue": 10.0,
        "rejectSampleValue": 100,
        "useTypeDefault": true
    }
}
```

### <a name="direct-copy-using-polybase"></a><span data-ttu-id="3e79f-262">Direkt kopia med PolyBase</span><span class="sxs-lookup"><span data-stu-id="3e79f-262">Direct copy using PolyBase</span></span>
<span data-ttu-id="3e79f-263">SQL Data Warehouse PolyBase stöd direkt för Azure Blob och Azure Data Lake Store (med tjänstens huvudnamn) som källa och krav för fil-format.</span><span class="sxs-lookup"><span data-stu-id="3e79f-263">SQL Data Warehouse PolyBase directly support Azure Blob and Azure Data Lake Store (using service principal) as source and with specific file format requirements.</span></span> <span data-ttu-id="3e79f-264">Om alla källdata uppfyller hello kriterier som beskrivs i det här avsnittet, kan du direkt kopiera från källan data store tooAzure SQL Data Warehouse med PolyBase.</span><span class="sxs-lookup"><span data-stu-id="3e79f-264">If your source data meets hello criteria described in this section, you can directly copy from source data store tooAzure SQL Data Warehouse using PolyBase.</span></span> <span data-ttu-id="3e79f-265">Annars kan du använda [mellanlagrad kopia med PolyBase](#staged-copy-using-polybase).</span><span class="sxs-lookup"><span data-stu-id="3e79f-265">Otherwise, you can use [Staged Copy using PolyBase](#staged-copy-using-polybase).</span></span>

> [!TIP]
> <span data-ttu-id="3e79f-266">toocopy data från Data Lake Store tooSQL informationslager effektivt, mer information från [Azure Data Factory gör det även enklare och smidig toouncover insikter från data när du använder Data Lake Store med SQL Data Warehouse](https://blogs.msdn.microsoft.com/azuredatalake/2017/04/08/azure-data-factory-makes-it-even-easier-and-convenient-to-uncover-insights-from-data-when-using-data-lake-store-with-sql-data-warehouse/).</span><span class="sxs-lookup"><span data-stu-id="3e79f-266">toocopy data from Data Lake Store tooSQL Data Warehouse efficiently, learn more from [Azure Data Factory makes it even easier and convenient toouncover insights from data when using Data Lake Store with SQL Data Warehouse](https://blogs.msdn.microsoft.com/azuredatalake/2017/04/08/azure-data-factory-makes-it-even-easier-and-convenient-to-uncover-insights-from-data-when-using-data-lake-store-with-sql-data-warehouse/).</span></span>

<span data-ttu-id="3e79f-267">Om hello krav inte uppfylls, Azure Data Factory kontrollerar hello inställningar och faller automatiskt tillbaka toohello BULKINSERT mekanism för hello dataflyttning.</span><span class="sxs-lookup"><span data-stu-id="3e79f-267">If hello requirements are not met, Azure Data Factory checks hello settings and automatically falls back toohello BULKINSERT mechanism for hello data movement.</span></span>

1. <span data-ttu-id="3e79f-268">**Källan länkade tjänsten** är av typen: **AzureStorage** eller **AzureDataLakeStore med huvudsakliga autentiseringen av tjänsten**.</span><span class="sxs-lookup"><span data-stu-id="3e79f-268">**Source linked service** is of type: **AzureStorage** or **AzureDataLakeStore with service principal authentication**.</span></span>  
2. <span data-ttu-id="3e79f-269">Hej **inkommande dataset** är av typen: **AzureBlob** eller **AzureDataLakeStore**, och hello formattyp under `type` egenskaper är **OrcFormat** , eller **TextFormat** med hello följande konfigurationer:</span><span class="sxs-lookup"><span data-stu-id="3e79f-269">hello **input dataset** is of type: **AzureBlob** or **AzureDataLakeStore**, and hello format type under `type` properties is **OrcFormat**, or **TextFormat** with hello following configurations:</span></span>

   1. <span data-ttu-id="3e79f-270">`rowDelimiter`måste vara  **\n** .</span><span class="sxs-lookup"><span data-stu-id="3e79f-270">`rowDelimiter` must be **\n**.</span></span>
   2. <span data-ttu-id="3e79f-271">`nullValue`har angetts för**tom sträng** (””), eller `treatEmptyAsNull` har angetts för**SANT**.</span><span class="sxs-lookup"><span data-stu-id="3e79f-271">`nullValue` is set too**empty string** (""), or `treatEmptyAsNull` is set too**true**.</span></span>
   3. <span data-ttu-id="3e79f-272">`encodingName`har angetts för**utf-8**, vilket är **standard** värde.</span><span class="sxs-lookup"><span data-stu-id="3e79f-272">`encodingName` is set too**utf-8**, which is **default** value.</span></span>
   4. <span data-ttu-id="3e79f-273">`escapeChar`, `quoteChar`, `firstRowAsHeader`, och `skipLineCount` har inte angetts.</span><span class="sxs-lookup"><span data-stu-id="3e79f-273">`escapeChar`, `quoteChar`, `firstRowAsHeader`, and `skipLineCount` are not specified.</span></span>
   5. <span data-ttu-id="3e79f-274">`compression`kan vara **ingen komprimering**, **GZip**, eller **Deflate**.</span><span class="sxs-lookup"><span data-stu-id="3e79f-274">`compression` can be **no compression**, **GZip**, or **Deflate**.</span></span>

    ```JSON
    "typeProperties": {
       "folderPath": "<blobpath>",
       "format": {
           "type": "TextFormat",     
           "columnDelimiter": "<any delimiter>",
           "rowDelimiter": "\n",       
           "nullValue": "",           
           "encodingName": "utf-8"    
       },
       "compression": {  
           "type": "GZip",  
           "level": "Optimal"  
       }  
    },
    ```

3. <span data-ttu-id="3e79f-275">Det finns inga `skipHeaderLineCount` inställningen **BlobSource** eller **AzureDataLakeStore** för hello kopieringsaktiviteten i hello pipeline.</span><span class="sxs-lookup"><span data-stu-id="3e79f-275">There is no `skipHeaderLineCount` setting under **BlobSource** or **AzureDataLakeStore** for hello Copy activity in hello pipeline.</span></span>
4. <span data-ttu-id="3e79f-276">Det finns inga `sliceIdentifierColumnName` inställningen **SqlDWSink** för hello kopieringsaktiviteten i hello pipeline.</span><span class="sxs-lookup"><span data-stu-id="3e79f-276">There is no `sliceIdentifierColumnName` setting under **SqlDWSink** for hello Copy activity in hello pipeline.</span></span> <span data-ttu-id="3e79f-277">(PolyBase garanterar att alla data har uppdaterats eller ingenting uppdateras i en enda körning.</span><span class="sxs-lookup"><span data-stu-id="3e79f-277">(PolyBase guarantees that all data is updated or nothing is updated in a single run.</span></span> <span data-ttu-id="3e79f-278">tooachieve **repeterbarhet**, du kan använda `sqlWriterCleanupScript`).</span><span class="sxs-lookup"><span data-stu-id="3e79f-278">tooachieve **repeatability**, you could use `sqlWriterCleanupScript`).</span></span>
5. <span data-ttu-id="3e79f-279">Det finns ingen `columnMapping` som används i hello associerade i en Kopieringsaktivitet.</span><span class="sxs-lookup"><span data-stu-id="3e79f-279">There is no `columnMapping` being used in hello associated in Copy activity.</span></span>

### <a name="staged-copy-using-polybase"></a><span data-ttu-id="3e79f-280">Stegvis kopia med PolyBase</span><span class="sxs-lookup"><span data-stu-id="3e79f-280">Staged Copy using PolyBase</span></span>
<span data-ttu-id="3e79f-281">När källdata inte uppfyller hello villkor som introducerades i hello föregående avsnitt, kan du Aktivera kopiering av data via en mellanliggande fristående Azure Blob Storage (inte får vara Premium-lagring).</span><span class="sxs-lookup"><span data-stu-id="3e79f-281">When your source data doesn’t meet hello criteria introduced in hello previous section, you can enable copying data via an interim staging Azure Blob Storage (cannot be Premium Storage).</span></span> <span data-ttu-id="3e79f-282">I det här fallet Azure Data Factory automatiskt utför omformningar på hello data toomeet data format krav PolyBase och sedan använda PolyBase tooload data till SQL Data Warehouse och vid senaste Rensa temp data från hello Blob storage.</span><span class="sxs-lookup"><span data-stu-id="3e79f-282">In this case, Azure Data Factory automatically performs transformations on hello data toomeet data format requirements of PolyBase, then use PolyBase tooload data into SQL Data Warehouse, and at last clean-up your temp data from hello Blob storage.</span></span> <span data-ttu-id="3e79f-283">Se [mellanlagrad kopiera](data-factory-copy-activity-performance.md#staged-copy) för information om hur kopiering av data via en fristående Azure-Blob fungerar i allmänhet.</span><span class="sxs-lookup"><span data-stu-id="3e79f-283">See [Staged Copy](data-factory-copy-activity-performance.md#staged-copy) for details on how copying data via a staging Azure Blob works in general.</span></span>

> [!NOTE]
> <span data-ttu-id="3e79f-284">Vid kopiering av data från en lokal lagring till Azure SQL Data Warehouse med PolyBase och mellanlagring, om Data Management Gateway-version som är lägre än 2.4 JRE (Java Runtime Environment) krävs för din gateway datorn som är används tootransform källdata i rätt format.</span><span class="sxs-lookup"><span data-stu-id="3e79f-284">When copying data from an on-prem data store into Azure SQL Data Warehouse using PolyBase and staging, if your Data Management Gateway version is below 2.4, JRE (Java Runtime Environment) is required on your gateway machine that is used tootransform your source data into proper format.</span></span> <span data-ttu-id="3e79f-285">Rekommenderar att du uppgraderar din gateway toohello senaste tooavoid sådana beroende.</span><span class="sxs-lookup"><span data-stu-id="3e79f-285">Suggest you upgrade your gateway toohello latest tooavoid such dependency.</span></span>
>

<span data-ttu-id="3e79f-286">toouse denna funktion, skapa en [Azure länkade lagringstjänsten](data-factory-azure-blob-connector.md#azure-storage-linked-service) som refererar toohello Azure Storage-konto som har hello tillfälliga blob-lagring, ange hello `enableStaging` och `stagingSettings` egenskaper för hello Kopieringsaktiviteten enligt hello följande kod:</span><span class="sxs-lookup"><span data-stu-id="3e79f-286">toouse this feature, create an [Azure Storage linked service](data-factory-azure-blob-connector.md#azure-storage-linked-service) that refers toohello Azure Storage Account that has hello interim blob storage, then specify hello `enableStaging` and `stagingSettings` properties for hello Copy Activity as shown in hello following code:</span></span>

```json
"activities":[  
{
    "name": "Sample copy activity from SQL Server tooSQL Data Warehouse via PolyBase",
    "type": "Copy",
    "inputs": [{ "name": "OnpremisesSQLServerInput" }],
    "outputs": [{ "name": "AzureSQLDWOutput" }],
    "typeProperties": {
        "source": {
            "type": "SqlSource",
        },
        "sink": {
            "type": "SqlDwSink",
            "allowPolyBase": true
        },
        "enableStaging": true,
        "stagingSettings": {
            "linkedServiceName": "MyStagingBlob"
        }
    }
}
]
```

## <a name="best-practices-when-using-polybase"></a><span data-ttu-id="3e79f-287">Rekommenderade metoder när du använder PolyBase</span><span class="sxs-lookup"><span data-stu-id="3e79f-287">Best practices when using PolyBase</span></span>
<span data-ttu-id="3e79f-288">hello följande avsnitt innehåller ytterligare bästa praxis toohello de som nämns i [Metodtips för Azure SQL Data Warehouse](../sql-data-warehouse/sql-data-warehouse-best-practices.md).</span><span class="sxs-lookup"><span data-stu-id="3e79f-288">hello following sections provide additional best practices toohello ones that are mentioned in [Best practices for Azure SQL Data Warehouse](../sql-data-warehouse/sql-data-warehouse-best-practices.md).</span></span>

### <a name="required-database-permission"></a><span data-ttu-id="3e79f-289">Behörighet krävs</span><span class="sxs-lookup"><span data-stu-id="3e79f-289">Required database permission</span></span>
<span data-ttu-id="3e79f-290">toouse PolyBase kräver hello användare att använda tooload data till SQL Data Warehouse har hello [”behörighet”](https://msdn.microsoft.com/library/ms191291.aspx) på hello måldatabasen.</span><span class="sxs-lookup"><span data-stu-id="3e79f-290">toouse PolyBase, it requires hello user being used tooload data into SQL Data Warehouse has hello ["CONTROL" permission](https://msdn.microsoft.com/library/ms191291.aspx) on hello target database.</span></span> <span data-ttu-id="3e79f-291">Enkelriktade tooachieve som är tooadd användaren som en medlem i ”db_owner”.</span><span class="sxs-lookup"><span data-stu-id="3e79f-291">One way tooachieve that is tooadd that user as a member of "db_owner" role.</span></span> <span data-ttu-id="3e79f-292">Lär dig hur toodo som genom att följa [i det här avsnittet](../sql-data-warehouse/sql-data-warehouse-overview-manage-security.md#authorization).</span><span class="sxs-lookup"><span data-stu-id="3e79f-292">Learn how toodo that by following [this section](../sql-data-warehouse/sql-data-warehouse-overview-manage-security.md#authorization).</span></span>

### <a name="row-size-and-data-type-limitation"></a><span data-ttu-id="3e79f-293">Ange begränsning Radstorleken och data</span><span class="sxs-lookup"><span data-stu-id="3e79f-293">Row size and data type limitation</span></span>
<span data-ttu-id="3e79f-294">Polybase belastningar begränsas tooloading rader både mindre än **1 MB** och det går inte att läsa in tooVARCHR(MAX), NVARCHAR(MAX) eller VARBINARY(MAX).</span><span class="sxs-lookup"><span data-stu-id="3e79f-294">Polybase loads are limited tooloading rows both smaller than **1 MB** and cannot load tooVARCHR(MAX), NVARCHAR(MAX) or VARBINARY(MAX).</span></span> <span data-ttu-id="3e79f-295">Se för[här](../sql-data-warehouse/sql-data-warehouse-service-capacity-limits.md#loads).</span><span class="sxs-lookup"><span data-stu-id="3e79f-295">Refer too[here](../sql-data-warehouse/sql-data-warehouse-service-capacity-limits.md#loads).</span></span>

<span data-ttu-id="3e79f-296">Om du har källdata med rader av större än 1 MB kan kanske du vill toosplit hello källtabellerna lodrätt i flera små om hello största Radstorleken på var och en av dem inte överstiger gränsen hello.</span><span class="sxs-lookup"><span data-stu-id="3e79f-296">If you have source data with rows of size greater than 1 MB, you may want toosplit hello source tables vertically into several small ones where hello largest row size of each of them does not exceed hello limit.</span></span> <span data-ttu-id="3e79f-297">hello mindre tabeller kan sedan läsas med PolyBase och sammanfogas tillsammans i Azure SQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="3e79f-297">hello smaller tables can then be loaded using PolyBase and merged together in Azure SQL Data Warehouse.</span></span>

### <a name="sql-data-warehouse-resource-class"></a><span data-ttu-id="3e79f-298">SQL Data Warehouse resursklassen</span><span class="sxs-lookup"><span data-stu-id="3e79f-298">SQL Data Warehouse resource class</span></span>
<span data-ttu-id="3e79f-299">tooachieve bästa möjliga genomströmningen, Överväg tooassign större resurs klassen toohello användaren som används tooload data till SQL Data Warehouse via PolyBase.</span><span class="sxs-lookup"><span data-stu-id="3e79f-299">tooachieve best possible throughput, consider tooassign larger resource class toohello user being used tooload data into SQL Data Warehouse via PolyBase.</span></span> <span data-ttu-id="3e79f-300">Lär dig hur toodo som genom att följa [ändra ett exempel på användaren resurs klassen](../sql-data-warehouse/sql-data-warehouse-develop-concurrency.md#changing-user-resource-class-example).</span><span class="sxs-lookup"><span data-stu-id="3e79f-300">Learn how toodo that by following [Change a user resource class example](../sql-data-warehouse/sql-data-warehouse-develop-concurrency.md#changing-user-resource-class-example).</span></span>

### <a name="tablename-in-azure-sql-data-warehouse"></a><span data-ttu-id="3e79f-301">Tabellnamn i Azure SQL Data Warehouse</span><span class="sxs-lookup"><span data-stu-id="3e79f-301">tableName in Azure SQL Data Warehouse</span></span>
<span data-ttu-id="3e79f-302">hello följande tabell innehåller exempel på hur toospecify hello **tableName** egenskap i dataset JSON för olika kombinationer av schema och tabellnamn.</span><span class="sxs-lookup"><span data-stu-id="3e79f-302">hello following table provides examples on how toospecify hello **tableName** property in dataset JSON for various combinations of schema and table name.</span></span>

| <span data-ttu-id="3e79f-303">DB-Schema</span><span class="sxs-lookup"><span data-stu-id="3e79f-303">DB Schema</span></span> | <span data-ttu-id="3e79f-304">Tabellnamnet</span><span class="sxs-lookup"><span data-stu-id="3e79f-304">Table name</span></span> | <span data-ttu-id="3e79f-305">tableName JSON-egenskapen</span><span class="sxs-lookup"><span data-stu-id="3e79f-305">tableName JSON property</span></span> |
| --- | --- | --- |
| <span data-ttu-id="3e79f-306">dbo</span><span class="sxs-lookup"><span data-stu-id="3e79f-306">dbo</span></span> |<span data-ttu-id="3e79f-307">Mytable prefix</span><span class="sxs-lookup"><span data-stu-id="3e79f-307">MyTable</span></span> |<span data-ttu-id="3e79f-308">Mytable prefix eller dbo. Mytable prefix eller [dbo]. [MyTable]</span><span class="sxs-lookup"><span data-stu-id="3e79f-308">MyTable or dbo.MyTable or [dbo].[MyTable]</span></span> |
| <span data-ttu-id="3e79f-309">dbo1</span><span class="sxs-lookup"><span data-stu-id="3e79f-309">dbo1</span></span> |<span data-ttu-id="3e79f-310">Mytable prefix</span><span class="sxs-lookup"><span data-stu-id="3e79f-310">MyTable</span></span> |<span data-ttu-id="3e79f-311">dbo1. Mytable prefix eller [dbo1]. [MyTable]</span><span class="sxs-lookup"><span data-stu-id="3e79f-311">dbo1.MyTable or [dbo1].[MyTable]</span></span> |
| <span data-ttu-id="3e79f-312">dbo</span><span class="sxs-lookup"><span data-stu-id="3e79f-312">dbo</span></span> |<span data-ttu-id="3e79f-313">My.Table</span><span class="sxs-lookup"><span data-stu-id="3e79f-313">My.Table</span></span> |<span data-ttu-id="3e79f-314">[My.Table] eller [dbo]. [My.Table]</span><span class="sxs-lookup"><span data-stu-id="3e79f-314">[My.Table] or [dbo].[My.Table]</span></span> |
| <span data-ttu-id="3e79f-315">dbo1</span><span class="sxs-lookup"><span data-stu-id="3e79f-315">dbo1</span></span> |<span data-ttu-id="3e79f-316">My.Table</span><span class="sxs-lookup"><span data-stu-id="3e79f-316">My.Table</span></span> |<span data-ttu-id="3e79f-317">[dbo1]. [My.Table]</span><span class="sxs-lookup"><span data-stu-id="3e79f-317">[dbo1].[My.Table]</span></span> |

<span data-ttu-id="3e79f-318">Det kan vara ett problem med hello-värde som du angav för hello tableName-egenskapen om hello följande fel visas.</span><span class="sxs-lookup"><span data-stu-id="3e79f-318">If you see hello following error, it could be an issue with hello value you specified for hello tableName property.</span></span> <span data-ttu-id="3e79f-319">Se hello tabellen för hello rätt metod toospecify värden för hello tableName JSON-egenskapen.</span><span class="sxs-lookup"><span data-stu-id="3e79f-319">See hello table for hello correct way toospecify values for hello tableName JSON property.</span></span>  

```
Type=System.Data.SqlClient.SqlException,Message=Invalid object name 'stg.Account_test'.,Source=.Net SqlClient Data Provider
```

### <a name="columns-with-default-values"></a><span data-ttu-id="3e79f-320">Kolumner med standardvärden</span><span class="sxs-lookup"><span data-stu-id="3e79f-320">Columns with default values</span></span>
<span data-ttu-id="3e79f-321">För närvarande PolyBase-funktionen i Data Factory accepterar bara hello samma antal kolumner som hello måltabellen.</span><span class="sxs-lookup"><span data-stu-id="3e79f-321">Currently, PolyBase feature in Data Factory only accepts hello same number of columns as in hello target table.</span></span> <span data-ttu-id="3e79f-322">Att du har en tabell med fyra kolumner och en av dem har definierats med ett standardvärde.</span><span class="sxs-lookup"><span data-stu-id="3e79f-322">Say, you have a table with four columns and one of them is defined with a default value.</span></span> <span data-ttu-id="3e79f-323">hello-indata måste fortfarande innehålla fyra kolumner.</span><span class="sxs-lookup"><span data-stu-id="3e79f-323">hello input data should still contain four columns.</span></span> <span data-ttu-id="3e79f-324">Att tillhandahålla en inkommande dataset 3 kolumner ger ett felmeddelande liknande toohello följande meddelande:</span><span class="sxs-lookup"><span data-stu-id="3e79f-324">Providing a 3-column input dataset would yield an error similar toohello following message:</span></span>

```
All columns of hello table must be specified in hello INSERT BULK statement.
```
<span data-ttu-id="3e79f-325">NULL-värde är en speciell typ av standardvärdet.</span><span class="sxs-lookup"><span data-stu-id="3e79f-325">NULL value is a special form of default value.</span></span> <span data-ttu-id="3e79f-326">Om hello kolumnen är nullbar hello indata (i blob) för kolumnen vara tom (kan inte vara saknas från hello inkommande datamängd).</span><span class="sxs-lookup"><span data-stu-id="3e79f-326">If hello column is nullable, hello input data (in blob) for that column could be empty (cannot be missing from hello input dataset).</span></span> <span data-ttu-id="3e79f-327">PolyBase infogas NULL för dem i hello Azure SQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="3e79f-327">PolyBase inserts NULL for them in hello Azure SQL Data Warehouse.</span></span>  

## <a name="auto-table-creation"></a><span data-ttu-id="3e79f-328">Automatiskt skapande av tabell</span><span class="sxs-lookup"><span data-stu-id="3e79f-328">Auto table creation</span></span>
<span data-ttu-id="3e79f-329">Om du använder guiden Kopiera toocopy data från SQL Server eller Azure SQL Database tooAzure SQL Data Warehouse och hello tabell som motsvarar toohello källtabellen inte finns i hello målarkivet skapa Data Factory automatiskt hello tabell i hello datalager med hjälp av hello källa tabellens schema.</span><span class="sxs-lookup"><span data-stu-id="3e79f-329">If you are using Copy Wizard toocopy data from SQL Server or Azure SQL Database tooAzure SQL Data Warehouse and hello table that corresponds toohello source table does not exist in hello destination store, Data Factory can automatically create hello table in hello data warehouse by using hello source table schema.</span></span>

<span data-ttu-id="3e79f-330">Data Factory skapar hello tabell i hello målarkivet med hello samma tabellnamn i datalagret för hello källa.</span><span class="sxs-lookup"><span data-stu-id="3e79f-330">Data Factory creates hello table in hello destination store with hello same table name in hello source data store.</span></span> <span data-ttu-id="3e79f-331">hello-datatyper för kolumner väljs utifrån hello följande mappning.</span><span class="sxs-lookup"><span data-stu-id="3e79f-331">hello data types for columns are chosen based on hello following type mapping.</span></span> <span data-ttu-id="3e79f-332">Om det behövs, utför typen konverteringar toofix alla inkompatibiliteter mellan käll- och Arkiv.</span><span class="sxs-lookup"><span data-stu-id="3e79f-332">If needed, it performs type conversions toofix any incompatibilities between source and destination stores.</span></span> <span data-ttu-id="3e79f-333">Dessutom används resursallokering tabell distribution.</span><span class="sxs-lookup"><span data-stu-id="3e79f-333">It also uses Round Robin table distribution.</span></span>

| <span data-ttu-id="3e79f-334">Typ av kolumn SQL-databas</span><span class="sxs-lookup"><span data-stu-id="3e79f-334">Source SQL Database column type</span></span> | <span data-ttu-id="3e79f-335">Måltypen SQL DW kolumn (storleksbegränsning)</span><span class="sxs-lookup"><span data-stu-id="3e79f-335">Destination SQL DW column type (size limitation)</span></span> |
| --- | --- |
| <span data-ttu-id="3e79f-336">int</span><span class="sxs-lookup"><span data-stu-id="3e79f-336">Int</span></span> | <span data-ttu-id="3e79f-337">int</span><span class="sxs-lookup"><span data-stu-id="3e79f-337">Int</span></span> |
| <span data-ttu-id="3e79f-338">BigInt</span><span class="sxs-lookup"><span data-stu-id="3e79f-338">BigInt</span></span> | <span data-ttu-id="3e79f-339">BigInt</span><span class="sxs-lookup"><span data-stu-id="3e79f-339">BigInt</span></span> |
| <span data-ttu-id="3e79f-340">SmallInt</span><span class="sxs-lookup"><span data-stu-id="3e79f-340">SmallInt</span></span> | <span data-ttu-id="3e79f-341">SmallInt</span><span class="sxs-lookup"><span data-stu-id="3e79f-341">SmallInt</span></span> |
| <span data-ttu-id="3e79f-342">TinyInt</span><span class="sxs-lookup"><span data-stu-id="3e79f-342">TinyInt</span></span> | <span data-ttu-id="3e79f-343">TinyInt</span><span class="sxs-lookup"><span data-stu-id="3e79f-343">TinyInt</span></span> |
| <span data-ttu-id="3e79f-344">bitar</span><span class="sxs-lookup"><span data-stu-id="3e79f-344">Bit</span></span> | <span data-ttu-id="3e79f-345">bitar</span><span class="sxs-lookup"><span data-stu-id="3e79f-345">Bit</span></span> |
| <span data-ttu-id="3e79f-346">Decimal</span><span class="sxs-lookup"><span data-stu-id="3e79f-346">Decimal</span></span> | <span data-ttu-id="3e79f-347">Decimal</span><span class="sxs-lookup"><span data-stu-id="3e79f-347">Decimal</span></span> |
| <span data-ttu-id="3e79f-348">numeriskt</span><span class="sxs-lookup"><span data-stu-id="3e79f-348">Numeric</span></span> | <span data-ttu-id="3e79f-349">Decimal</span><span class="sxs-lookup"><span data-stu-id="3e79f-349">Decimal</span></span> |
| <span data-ttu-id="3e79f-350">flyttal</span><span class="sxs-lookup"><span data-stu-id="3e79f-350">Float</span></span> | <span data-ttu-id="3e79f-351">flyttal</span><span class="sxs-lookup"><span data-stu-id="3e79f-351">Float</span></span> |
| <span data-ttu-id="3e79f-352">Money</span><span class="sxs-lookup"><span data-stu-id="3e79f-352">Money</span></span> | <span data-ttu-id="3e79f-353">Money</span><span class="sxs-lookup"><span data-stu-id="3e79f-353">Money</span></span> |
| <span data-ttu-id="3e79f-354">Real</span><span class="sxs-lookup"><span data-stu-id="3e79f-354">Real</span></span> | <span data-ttu-id="3e79f-355">Real</span><span class="sxs-lookup"><span data-stu-id="3e79f-355">Real</span></span> |
| <span data-ttu-id="3e79f-356">SmallMoney</span><span class="sxs-lookup"><span data-stu-id="3e79f-356">SmallMoney</span></span> | <span data-ttu-id="3e79f-357">SmallMoney</span><span class="sxs-lookup"><span data-stu-id="3e79f-357">SmallMoney</span></span> |
| <span data-ttu-id="3e79f-358">Binär</span><span class="sxs-lookup"><span data-stu-id="3e79f-358">Binary</span></span> | <span data-ttu-id="3e79f-359">Binär</span><span class="sxs-lookup"><span data-stu-id="3e79f-359">Binary</span></span> |
| <span data-ttu-id="3e79f-360">varbinary</span><span class="sxs-lookup"><span data-stu-id="3e79f-360">Varbinary</span></span> | <span data-ttu-id="3e79f-361">Varbinary (upp too8000)</span><span class="sxs-lookup"><span data-stu-id="3e79f-361">Varbinary (up too8000)</span></span> |
| <span data-ttu-id="3e79f-362">Date</span><span class="sxs-lookup"><span data-stu-id="3e79f-362">Date</span></span> | <span data-ttu-id="3e79f-363">Date</span><span class="sxs-lookup"><span data-stu-id="3e79f-363">Date</span></span> |
| <span data-ttu-id="3e79f-364">Datum och tid</span><span class="sxs-lookup"><span data-stu-id="3e79f-364">DateTime</span></span> | <span data-ttu-id="3e79f-365">Datum och tid</span><span class="sxs-lookup"><span data-stu-id="3e79f-365">DateTime</span></span> |
| <span data-ttu-id="3e79f-366">DateTime2</span><span class="sxs-lookup"><span data-stu-id="3e79f-366">DateTime2</span></span> | <span data-ttu-id="3e79f-367">DateTime2</span><span class="sxs-lookup"><span data-stu-id="3e79f-367">DateTime2</span></span> |
| <span data-ttu-id="3e79f-368">Tid</span><span class="sxs-lookup"><span data-stu-id="3e79f-368">Time</span></span> | <span data-ttu-id="3e79f-369">Tid</span><span class="sxs-lookup"><span data-stu-id="3e79f-369">Time</span></span> |
| <span data-ttu-id="3e79f-370">DateTimeOffset</span><span class="sxs-lookup"><span data-stu-id="3e79f-370">DateTimeOffset</span></span> | <span data-ttu-id="3e79f-371">DateTimeOffset</span><span class="sxs-lookup"><span data-stu-id="3e79f-371">DateTimeOffset</span></span> |
| <span data-ttu-id="3e79f-372">SmallDateTime</span><span class="sxs-lookup"><span data-stu-id="3e79f-372">SmallDateTime</span></span> | <span data-ttu-id="3e79f-373">SmallDateTime</span><span class="sxs-lookup"><span data-stu-id="3e79f-373">SmallDateTime</span></span> |
| <span data-ttu-id="3e79f-374">Text</span><span class="sxs-lookup"><span data-stu-id="3e79f-374">Text</span></span> | <span data-ttu-id="3e79f-375">Varchar (upp too8000)</span><span class="sxs-lookup"><span data-stu-id="3e79f-375">Varchar (up too8000)</span></span> |
| <span data-ttu-id="3e79f-376">NText</span><span class="sxs-lookup"><span data-stu-id="3e79f-376">NText</span></span> | <span data-ttu-id="3e79f-377">NVarChar (upp too4000)</span><span class="sxs-lookup"><span data-stu-id="3e79f-377">NVarChar (up too4000)</span></span> |
| <span data-ttu-id="3e79f-378">Bild</span><span class="sxs-lookup"><span data-stu-id="3e79f-378">Image</span></span> | <span data-ttu-id="3e79f-379">VarBinary (upp too8000)</span><span class="sxs-lookup"><span data-stu-id="3e79f-379">VarBinary (up too8000)</span></span> |
| <span data-ttu-id="3e79f-380">Unik identifierare</span><span class="sxs-lookup"><span data-stu-id="3e79f-380">UniqueIdentifier</span></span> | <span data-ttu-id="3e79f-381">Unik identifierare</span><span class="sxs-lookup"><span data-stu-id="3e79f-381">UniqueIdentifier</span></span> |
| <span data-ttu-id="3e79f-382">Char</span><span class="sxs-lookup"><span data-stu-id="3e79f-382">Char</span></span> | <span data-ttu-id="3e79f-383">Char</span><span class="sxs-lookup"><span data-stu-id="3e79f-383">Char</span></span> |
| <span data-ttu-id="3e79f-384">NChar</span><span class="sxs-lookup"><span data-stu-id="3e79f-384">NChar</span></span> | <span data-ttu-id="3e79f-385">NChar</span><span class="sxs-lookup"><span data-stu-id="3e79f-385">NChar</span></span> |
| <span data-ttu-id="3e79f-386">VarChar</span><span class="sxs-lookup"><span data-stu-id="3e79f-386">VarChar</span></span> | <span data-ttu-id="3e79f-387">VarChar (upp too8000)</span><span class="sxs-lookup"><span data-stu-id="3e79f-387">VarChar (up too8000)</span></span> |
| <span data-ttu-id="3e79f-388">NVarChar</span><span class="sxs-lookup"><span data-stu-id="3e79f-388">NVarChar</span></span> | <span data-ttu-id="3e79f-389">NVarChar (upp too4000)</span><span class="sxs-lookup"><span data-stu-id="3e79f-389">NVarChar (up too4000)</span></span> |
| <span data-ttu-id="3e79f-390">XML</span><span class="sxs-lookup"><span data-stu-id="3e79f-390">Xml</span></span> | <span data-ttu-id="3e79f-391">Varchar (upp too8000)</span><span class="sxs-lookup"><span data-stu-id="3e79f-391">Varchar (up too8000)</span></span> |

[!INCLUDE [data-factory-type-repeatability-for-sql-sources](../../includes/data-factory-type-repeatability-for-sql-sources.md)]

## <a name="type-mapping-for-azure-sql-data-warehouse"></a><span data-ttu-id="3e79f-392">Mappning för Azure SQL Data Warehouse</span><span class="sxs-lookup"><span data-stu-id="3e79f-392">Type mapping for Azure SQL Data Warehouse</span></span>
<span data-ttu-id="3e79f-393">Som anges i hello [data movement aktiviteter](data-factory-data-movement-activities.md) artikeln kopieringsaktiviteten utför automatisk konverteringar från källan typer toosink typer med hello följande metod i steg 2:</span><span class="sxs-lookup"><span data-stu-id="3e79f-393">As mentioned in hello [data movement activities](data-factory-data-movement-activities.md) article, Copy activity performs automatic type conversions from source types toosink types with hello following 2-step approach:</span></span>

1. <span data-ttu-id="3e79f-394">Konvertera från inbyggda typer too.NET källtypen</span><span class="sxs-lookup"><span data-stu-id="3e79f-394">Convert from native source types too.NET type</span></span>
2. <span data-ttu-id="3e79f-395">Konvertera från .NET typen toonative Mottagartypen</span><span class="sxs-lookup"><span data-stu-id="3e79f-395">Convert from .NET type toonative sink type</span></span>

<span data-ttu-id="3e79f-396">När du flyttar data & från Azure SQL Data Warehouse, används hello följande mappningar från SQL too.NET typ och vice versa.</span><span class="sxs-lookup"><span data-stu-id="3e79f-396">When moving data too& from Azure SQL Data Warehouse, hello following mappings are used from SQL type too.NET type and vice versa.</span></span>

<span data-ttu-id="3e79f-397">hello-mappning är samma som hello [Datatypsmappningen i SQL Server för ADO.NET](https://msdn.microsoft.com/library/cc716729.aspx).</span><span class="sxs-lookup"><span data-stu-id="3e79f-397">hello mapping is same as hello [SQL Server Data Type Mapping for ADO.NET](https://msdn.microsoft.com/library/cc716729.aspx).</span></span>

| <span data-ttu-id="3e79f-398">SQL Server Database Engine-typ</span><span class="sxs-lookup"><span data-stu-id="3e79f-398">SQL Server Database Engine type</span></span> | <span data-ttu-id="3e79f-399">.NET framework-typ</span><span class="sxs-lookup"><span data-stu-id="3e79f-399">.NET Framework type</span></span> |
| --- | --- |
| <span data-ttu-id="3e79f-400">bigint</span><span class="sxs-lookup"><span data-stu-id="3e79f-400">bigint</span></span> |<span data-ttu-id="3e79f-401">Int64</span><span class="sxs-lookup"><span data-stu-id="3e79f-401">Int64</span></span> |
| <span data-ttu-id="3e79f-402">Binär</span><span class="sxs-lookup"><span data-stu-id="3e79f-402">binary</span></span> |<span data-ttu-id="3e79f-403">byte]</span><span class="sxs-lookup"><span data-stu-id="3e79f-403">Byte[]</span></span> |
| <span data-ttu-id="3e79f-404">bitar</span><span class="sxs-lookup"><span data-stu-id="3e79f-404">bit</span></span> |<span data-ttu-id="3e79f-405">Booleskt värde</span><span class="sxs-lookup"><span data-stu-id="3e79f-405">Boolean</span></span> |
| <span data-ttu-id="3e79f-406">Char</span><span class="sxs-lookup"><span data-stu-id="3e79f-406">char</span></span> |<span data-ttu-id="3e79f-407">Sträng, Char]</span><span class="sxs-lookup"><span data-stu-id="3e79f-407">String, Char[]</span></span> |
| <span data-ttu-id="3e79f-408">Datum</span><span class="sxs-lookup"><span data-stu-id="3e79f-408">date</span></span> |<span data-ttu-id="3e79f-409">Datum och tid</span><span class="sxs-lookup"><span data-stu-id="3e79f-409">DateTime</span></span> |
| <span data-ttu-id="3e79f-410">Datum och tid</span><span class="sxs-lookup"><span data-stu-id="3e79f-410">Datetime</span></span> |<span data-ttu-id="3e79f-411">Datum och tid</span><span class="sxs-lookup"><span data-stu-id="3e79f-411">DateTime</span></span> |
| <span data-ttu-id="3e79f-412">datetime2</span><span class="sxs-lookup"><span data-stu-id="3e79f-412">datetime2</span></span> |<span data-ttu-id="3e79f-413">Datum och tid</span><span class="sxs-lookup"><span data-stu-id="3e79f-413">DateTime</span></span> |
| <span data-ttu-id="3e79f-414">DateTimeOffset</span><span class="sxs-lookup"><span data-stu-id="3e79f-414">Datetimeoffset</span></span> |<span data-ttu-id="3e79f-415">DateTimeOffset</span><span class="sxs-lookup"><span data-stu-id="3e79f-415">DateTimeOffset</span></span> |
| <span data-ttu-id="3e79f-416">Decimal</span><span class="sxs-lookup"><span data-stu-id="3e79f-416">Decimal</span></span> |<span data-ttu-id="3e79f-417">Decimal</span><span class="sxs-lookup"><span data-stu-id="3e79f-417">Decimal</span></span> |
| <span data-ttu-id="3e79f-418">FILESTREAM-attributet (varbinary(max))</span><span class="sxs-lookup"><span data-stu-id="3e79f-418">FILESTREAM attribute (varbinary(max))</span></span> |<span data-ttu-id="3e79f-419">byte]</span><span class="sxs-lookup"><span data-stu-id="3e79f-419">Byte[]</span></span> |
| <span data-ttu-id="3e79f-420">flyttal</span><span class="sxs-lookup"><span data-stu-id="3e79f-420">Float</span></span> |<span data-ttu-id="3e79f-421">dubbla</span><span class="sxs-lookup"><span data-stu-id="3e79f-421">Double</span></span> |
| <span data-ttu-id="3e79f-422">Bild</span><span class="sxs-lookup"><span data-stu-id="3e79f-422">image</span></span> |<span data-ttu-id="3e79f-423">byte]</span><span class="sxs-lookup"><span data-stu-id="3e79f-423">Byte[]</span></span> |
| <span data-ttu-id="3e79f-424">int</span><span class="sxs-lookup"><span data-stu-id="3e79f-424">int</span></span> |<span data-ttu-id="3e79f-425">Int32</span><span class="sxs-lookup"><span data-stu-id="3e79f-425">Int32</span></span> |
| <span data-ttu-id="3e79f-426">Money</span><span class="sxs-lookup"><span data-stu-id="3e79f-426">money</span></span> |<span data-ttu-id="3e79f-427">Decimal</span><span class="sxs-lookup"><span data-stu-id="3e79f-427">Decimal</span></span> |
| <span data-ttu-id="3e79f-428">nchar</span><span class="sxs-lookup"><span data-stu-id="3e79f-428">nchar</span></span> |<span data-ttu-id="3e79f-429">Sträng, Char]</span><span class="sxs-lookup"><span data-stu-id="3e79f-429">String, Char[]</span></span> |
| <span data-ttu-id="3e79f-430">ntext</span><span class="sxs-lookup"><span data-stu-id="3e79f-430">ntext</span></span> |<span data-ttu-id="3e79f-431">Sträng, Char]</span><span class="sxs-lookup"><span data-stu-id="3e79f-431">String, Char[]</span></span> |
| <span data-ttu-id="3e79f-432">numeriskt</span><span class="sxs-lookup"><span data-stu-id="3e79f-432">numeric</span></span> |<span data-ttu-id="3e79f-433">Decimal</span><span class="sxs-lookup"><span data-stu-id="3e79f-433">Decimal</span></span> |
| <span data-ttu-id="3e79f-434">nvarchar</span><span class="sxs-lookup"><span data-stu-id="3e79f-434">nvarchar</span></span> |<span data-ttu-id="3e79f-435">Sträng, Char]</span><span class="sxs-lookup"><span data-stu-id="3e79f-435">String, Char[]</span></span> |
| <span data-ttu-id="3e79f-436">Verklig</span><span class="sxs-lookup"><span data-stu-id="3e79f-436">real</span></span> |<span data-ttu-id="3e79f-437">Enskild</span><span class="sxs-lookup"><span data-stu-id="3e79f-437">Single</span></span> |
| <span data-ttu-id="3e79f-438">ROWVERSION</span><span class="sxs-lookup"><span data-stu-id="3e79f-438">rowversion</span></span> |<span data-ttu-id="3e79f-439">byte]</span><span class="sxs-lookup"><span data-stu-id="3e79f-439">Byte[]</span></span> |
| <span data-ttu-id="3e79f-440">smalldatetime</span><span class="sxs-lookup"><span data-stu-id="3e79f-440">smalldatetime</span></span> |<span data-ttu-id="3e79f-441">Datum och tid</span><span class="sxs-lookup"><span data-stu-id="3e79f-441">DateTime</span></span> |
| <span data-ttu-id="3e79f-442">smallint</span><span class="sxs-lookup"><span data-stu-id="3e79f-442">smallint</span></span> |<span data-ttu-id="3e79f-443">Int16</span><span class="sxs-lookup"><span data-stu-id="3e79f-443">Int16</span></span> |
| <span data-ttu-id="3e79f-444">smallmoney</span><span class="sxs-lookup"><span data-stu-id="3e79f-444">smallmoney</span></span> |<span data-ttu-id="3e79f-445">Decimal</span><span class="sxs-lookup"><span data-stu-id="3e79f-445">Decimal</span></span> |
| <span data-ttu-id="3e79f-446">sql_variant</span><span class="sxs-lookup"><span data-stu-id="3e79f-446">sql_variant</span></span> |<span data-ttu-id="3e79f-447">Objektet *</span><span class="sxs-lookup"><span data-stu-id="3e79f-447">Object *</span></span> |
| <span data-ttu-id="3e79f-448">Text</span><span class="sxs-lookup"><span data-stu-id="3e79f-448">text</span></span> |<span data-ttu-id="3e79f-449">Sträng, Char]</span><span class="sxs-lookup"><span data-stu-id="3e79f-449">String, Char[]</span></span> |
| <span data-ttu-id="3e79f-450">time</span><span class="sxs-lookup"><span data-stu-id="3e79f-450">time</span></span> |<span data-ttu-id="3e79f-451">TimeSpan</span><span class="sxs-lookup"><span data-stu-id="3e79f-451">TimeSpan</span></span> |
| <span data-ttu-id="3e79f-452">tidsstämpel</span><span class="sxs-lookup"><span data-stu-id="3e79f-452">timestamp</span></span> |<span data-ttu-id="3e79f-453">byte]</span><span class="sxs-lookup"><span data-stu-id="3e79f-453">Byte[]</span></span> |
| <span data-ttu-id="3e79f-454">tinyint</span><span class="sxs-lookup"><span data-stu-id="3e79f-454">tinyint</span></span> |<span data-ttu-id="3e79f-455">Mottagna byte</span><span class="sxs-lookup"><span data-stu-id="3e79f-455">Byte</span></span> |
| <span data-ttu-id="3e79f-456">Unik identifierare</span><span class="sxs-lookup"><span data-stu-id="3e79f-456">uniqueidentifier</span></span> |<span data-ttu-id="3e79f-457">GUID</span><span class="sxs-lookup"><span data-stu-id="3e79f-457">Guid</span></span> |
| <span data-ttu-id="3e79f-458">varbinary</span><span class="sxs-lookup"><span data-stu-id="3e79f-458">varbinary</span></span> |<span data-ttu-id="3e79f-459">byte]</span><span class="sxs-lookup"><span data-stu-id="3e79f-459">Byte[]</span></span> |
| <span data-ttu-id="3e79f-460">varchar</span><span class="sxs-lookup"><span data-stu-id="3e79f-460">varchar</span></span> |<span data-ttu-id="3e79f-461">Sträng, Char]</span><span class="sxs-lookup"><span data-stu-id="3e79f-461">String, Char[]</span></span> |
| <span data-ttu-id="3e79f-462">xml</span><span class="sxs-lookup"><span data-stu-id="3e79f-462">xml</span></span> |<span data-ttu-id="3e79f-463">XML</span><span class="sxs-lookup"><span data-stu-id="3e79f-463">Xml</span></span> |

<span data-ttu-id="3e79f-464">Du kan också mappa kolumner från källan dataset toocolumns från sink datauppsättning i aktivitetsdefinitionen för hello kopia.</span><span class="sxs-lookup"><span data-stu-id="3e79f-464">You can also map columns from source dataset toocolumns from sink dataset in hello copy activity definition.</span></span> <span data-ttu-id="3e79f-465">Mer information finns i [mappa dataset kolumner i Azure Data Factory](data-factory-map-columns.md).</span><span class="sxs-lookup"><span data-stu-id="3e79f-465">For details, see [Mapping dataset columns in Azure Data Factory](data-factory-map-columns.md).</span></span>

## <a name="json-examples-for-copying-data-tooand-from-sql-data-warehouse"></a><span data-ttu-id="3e79f-466">JSON-exempel för att kopiera data tooand från SQL Data Warehouse</span><span class="sxs-lookup"><span data-stu-id="3e79f-466">JSON examples for copying data tooand from SQL Data Warehouse</span></span>
<span data-ttu-id="3e79f-467">hello följande exempel ger exempel JSON definitioner som du kan använda toocreate en pipeline med hjälp av [Azure-portalen](data-factory-copy-activity-tutorial-using-azure-portal.md) eller [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md) eller [Azure PowerShell](data-factory-copy-activity-tutorial-using-powershell.md).</span><span class="sxs-lookup"><span data-stu-id="3e79f-467">hello following examples provide sample JSON definitions that you can use toocreate a pipeline by using [Azure portal](data-factory-copy-activity-tutorial-using-azure-portal.md) or [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md) or [Azure PowerShell](data-factory-copy-activity-tutorial-using-powershell.md).</span></span> <span data-ttu-id="3e79f-468">De visar hur toocopy data tooand från Azure SQL Data Warehouse och Azure Blob Storage.</span><span class="sxs-lookup"><span data-stu-id="3e79f-468">They show how toocopy data tooand from Azure SQL Data Warehouse and Azure Blob Storage.</span></span> <span data-ttu-id="3e79f-469">Dock datan kan kopieras **direkt** från alla källor tooany av hello sänkor anges [här](data-factory-data-movement-activities.md#supported-data-stores-and-formats) med hello Kopieringsaktiviteten i Azure Data Factory.</span><span class="sxs-lookup"><span data-stu-id="3e79f-469">However, data can be copied **directly** from any of sources tooany of hello sinks stated [here](data-factory-data-movement-activities.md#supported-data-stores-and-formats) using hello Copy Activity in Azure Data Factory.</span></span>

### <a name="example-copy-data-from-azure-sql-data-warehouse-tooazure-blob"></a><span data-ttu-id="3e79f-470">Exempel: Kopiera data från Azure SQL Data Warehouse tooAzure Blob</span><span class="sxs-lookup"><span data-stu-id="3e79f-470">Example: Copy data from Azure SQL Data Warehouse tooAzure Blob</span></span>
<span data-ttu-id="3e79f-471">hello exemplet definierar hello följande Data Factory-enheter:</span><span class="sxs-lookup"><span data-stu-id="3e79f-471">hello sample defines hello following Data Factory entities:</span></span>

1. <span data-ttu-id="3e79f-472">En länkad tjänst av typen [AzureSqlDW](#linked-service-properties).</span><span class="sxs-lookup"><span data-stu-id="3e79f-472">A linked service of type [AzureSqlDW](#linked-service-properties).</span></span>
2. <span data-ttu-id="3e79f-473">En länkad tjänst av typen [AzureStorage](data-factory-azure-blob-connector.md#linked-service-properties).</span><span class="sxs-lookup"><span data-stu-id="3e79f-473">A linked service of type [AzureStorage](data-factory-azure-blob-connector.md#linked-service-properties).</span></span>
3. <span data-ttu-id="3e79f-474">Indata [dataset](data-factory-create-datasets.md) av typen [AzureSqlDWTable](#dataset-properties).</span><span class="sxs-lookup"><span data-stu-id="3e79f-474">An input [dataset](data-factory-create-datasets.md) of type [AzureSqlDWTable](#dataset-properties).</span></span>
4. <span data-ttu-id="3e79f-475">Utdata [dataset](data-factory-create-datasets.md) av typen [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties).</span><span class="sxs-lookup"><span data-stu-id="3e79f-475">An output [dataset](data-factory-create-datasets.md) of type [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties).</span></span>
5. <span data-ttu-id="3e79f-476">En [pipeline](data-factory-create-pipelines.md) med Kopieringsaktiviteten som använder [SqlDWSource](#copy-activity-properties) och [BlobSink](data-factory-azure-blob-connector.md#copy-activity-properties).</span><span class="sxs-lookup"><span data-stu-id="3e79f-476">A [pipeline](data-factory-create-pipelines.md) with Copy Activity that uses [SqlDWSource](#copy-activity-properties) and [BlobSink](data-factory-azure-blob-connector.md#copy-activity-properties).</span></span>

<span data-ttu-id="3e79f-477">hello exemplet kopierar tidsserier (varje timme, dag, etc.) data från en tabell i Azure SQL Data Warehouse-databas tooa blob varje timme.</span><span class="sxs-lookup"><span data-stu-id="3e79f-477">hello sample copies time-series (hourly, daily, etc.) data from a table in Azure SQL Data Warehouse database tooa blob every hour.</span></span> <span data-ttu-id="3e79f-478">hello JSON egenskaper som används i exemplen beskrivs i hello-exempel i följande avsnitt.</span><span class="sxs-lookup"><span data-stu-id="3e79f-478">hello JSON properties used in these samples are described in sections following hello samples.</span></span>

<span data-ttu-id="3e79f-479">**Azure SQL Data Warehouse länkade tjänsten:**</span><span class="sxs-lookup"><span data-stu-id="3e79f-479">**Azure SQL Data Warehouse linked service:**</span></span>

```JSON
{
  "name": "AzureSqlDWLinkedService",
  "properties": {
    "type": "AzureSqlDW",
    "typeProperties": {
      "connectionString": "Server=tcp:<servername>.database.windows.net,1433;Database=<databasename>;User ID=<username>@<servername>;Password=<password>;Trusted_Connection=False;Encrypt=True;Connection Timeout=30"
    }
  }
}
```
<span data-ttu-id="3e79f-480">**Azure Blob storage länkade tjänsten:**</span><span class="sxs-lookup"><span data-stu-id="3e79f-480">**Azure Blob storage linked service:**</span></span>

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
<span data-ttu-id="3e79f-481">**Azure SQL Data Warehouse-indatauppsättningen:**</span><span class="sxs-lookup"><span data-stu-id="3e79f-481">**Azure SQL Data Warehouse input dataset:**</span></span>

<span data-ttu-id="3e79f-482">hello exemplet förutsätter att du har skapat en tabell ”mytable” som prefix i Azure SQL Data Warehouse och innehåller en kolumn med namnet ”timestampcolumn” för tid series-data.</span><span class="sxs-lookup"><span data-stu-id="3e79f-482">hello sample assumes you have created a table “MyTable” in Azure SQL Data Warehouse and it contains a column called “timestampcolumn” for time series data.</span></span>

<span data-ttu-id="3e79f-483">Inställningen ”externa”: ”true” informerar hello Data Factory-tjänsten som hello dataset är externa toohello data factory och inte tillverkas av en aktivitet i hello data factory.</span><span class="sxs-lookup"><span data-stu-id="3e79f-483">Setting “external”: ”true” informs hello Data Factory service that hello dataset is external toohello data factory and is not produced by an activity in hello data factory.</span></span>

```JSON
{
  "name": "AzureSqlDWInput",
  "properties": {
    "type": "AzureSqlDWTable",
    "linkedServiceName": "AzureSqlDWLinkedService",
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
<span data-ttu-id="3e79f-484">**Azure Blob utdatauppsättningen:**</span><span class="sxs-lookup"><span data-stu-id="3e79f-484">**Azure Blob output dataset:**</span></span>

<span data-ttu-id="3e79f-485">Data skrivs tooa nya blob varje timme (frekvens: timme, intervall: 1).</span><span class="sxs-lookup"><span data-stu-id="3e79f-485">Data is written tooa new blob every hour (frequency: hour, interval: 1).</span></span> <span data-ttu-id="3e79f-486">hello mappsökväg för hello blob utvärderas dynamiskt baserat på hello starttiden för hello-segment som bearbetas.</span><span class="sxs-lookup"><span data-stu-id="3e79f-486">hello folder path for hello blob is dynamically evaluated based on hello start time of hello slice that is being processed.</span></span> <span data-ttu-id="3e79f-487">hello mappsökväg använder år, månad, dag och timmar delar av hello starttid.</span><span class="sxs-lookup"><span data-stu-id="3e79f-487">hello folder path uses year, month, day, and hours parts of hello start time.</span></span>

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

<span data-ttu-id="3e79f-488">**Kopiera aktivitet i en pipeline med SqlDWSource och BlobSink:**</span><span class="sxs-lookup"><span data-stu-id="3e79f-488">**Copy activity in a pipeline with SqlDWSource and BlobSink:**</span></span>

<span data-ttu-id="3e79f-489">hello pipelinen innehåller en kopia-aktivitet som är konfigurerade toouse hello inkommande och utgående datauppsättningar och är schemalagda toorun varje timme.</span><span class="sxs-lookup"><span data-stu-id="3e79f-489">hello pipeline contains a Copy Activity that is configured toouse hello input and output datasets and is scheduled toorun every hour.</span></span> <span data-ttu-id="3e79f-490">I hello pipeline JSON-definitionen hello **källa** typ har angetts för**SqlDWSource** och **sink** typ har angetts för**BlobSink**.</span><span class="sxs-lookup"><span data-stu-id="3e79f-490">In hello pipeline JSON definition, hello **source** type is set too**SqlDWSource** and **sink** type is set too**BlobSink**.</span></span> <span data-ttu-id="3e79f-491">hello SQL-frågan som angetts för hello **SqlReaderQuery** egenskapen väljer hello data i hello tidigare timme toocopy.</span><span class="sxs-lookup"><span data-stu-id="3e79f-491">hello SQL query specified for hello **SqlReaderQuery** property selects hello data in hello past hour toocopy.</span></span>

```JSON
{  
    "name":"SamplePipeline",
    "properties":{  
    "start":"2014-06-01T18:00:00",
    "end":"2014-06-01T19:00:00",
    "description":"pipeline for copy activity",
    "activities":[  
      {
        "name": "AzureSQLDWtoBlob",
        "description": "copy activity",
        "type": "Copy",
        "inputs": [
          {
            "name": "AzureSqlDWInput"
          }
        ],
        "outputs": [
          {
            "name": "AzureBlobOutput"
          }
        ],
        "typeProperties": {
          "source": {
            "type": "SqlDWSource",
            "sqlReaderQuery": "$$Text.Format('select * from MyTable where timestampcolumn >= \\'{0:yyyy-MM-dd HH:mm}\\' AND timestampcolumn < \\'{1:yyyy-MM-dd HH:mm}\\'', WindowStart, WindowEnd)"
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
> [!NOTE]
> <span data-ttu-id="3e79f-492">I exemplet hello **sqlReaderQuery** har angetts för hello SqlDWSource.</span><span class="sxs-lookup"><span data-stu-id="3e79f-492">In hello example, **sqlReaderQuery** is specified for hello SqlDWSource.</span></span> <span data-ttu-id="3e79f-493">Hej Kopieringsaktiviteten kör den här frågan mot hello Azure SQL Data Warehouse källdata tooget hello.</span><span class="sxs-lookup"><span data-stu-id="3e79f-493">hello Copy Activity runs this query against hello Azure SQL Data Warehouse source tooget hello data.</span></span>
>
> <span data-ttu-id="3e79f-494">Du kan också ange en lagrad procedur genom att ange hello **sqlReaderStoredProcedureName** och **storedProcedureParameters** (om hello tar lagrade proceduren parametrar).</span><span class="sxs-lookup"><span data-stu-id="3e79f-494">Alternatively, you can specify a stored procedure by specifying hello **sqlReaderStoredProcedureName** and **storedProcedureParameters** (if hello stored procedure takes parameters).</span></span>
>
> <span data-ttu-id="3e79f-495">Om du inte anger sqlReaderQuery eller sqlReaderStoredProcedureName hello kolumner som anges i hello strukturen i hello dataset JSON är används toobuild en fråga (Välj kolumn1, kolumn2 från mytable prefix) toorun mot hello Azure SQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="3e79f-495">If you do not specify either sqlReaderQuery or sqlReaderStoredProcedureName, hello columns defined in hello structure section of hello dataset JSON are used toobuild a query (select column1, column2 from mytable) toorun against hello Azure SQL Data Warehouse.</span></span> <span data-ttu-id="3e79f-496">Om hello datauppsättningsdefinitionen inte har hello struktur, markeras alla kolumner från tabellen hello.</span><span class="sxs-lookup"><span data-stu-id="3e79f-496">If hello dataset definition does not have hello structure, all columns are selected from hello table.</span></span>
>
>

### <a name="example-copy-data-from-azure-blob-tooazure-sql-data-warehouse"></a><span data-ttu-id="3e79f-497">Exempel: Kopiera data från Azure Blob tooAzure SQL Data Warehouse</span><span class="sxs-lookup"><span data-stu-id="3e79f-497">Example: Copy data from Azure Blob tooAzure SQL Data Warehouse</span></span>
<span data-ttu-id="3e79f-498">hello exemplet definierar hello följande Data Factory-enheter:</span><span class="sxs-lookup"><span data-stu-id="3e79f-498">hello sample defines hello following Data Factory entities:</span></span>

1. <span data-ttu-id="3e79f-499">En länkad tjänst av typen [AzureSqlDW](#linked-service-properties).</span><span class="sxs-lookup"><span data-stu-id="3e79f-499">A linked service of type [AzureSqlDW](#linked-service-properties).</span></span>
2. <span data-ttu-id="3e79f-500">En länkad tjänst av typen [AzureStorage](data-factory-azure-blob-connector.md#linked-service-properties).</span><span class="sxs-lookup"><span data-stu-id="3e79f-500">A linked service of type [AzureStorage](data-factory-azure-blob-connector.md#linked-service-properties).</span></span>
3. <span data-ttu-id="3e79f-501">Indata [dataset](data-factory-create-datasets.md) av typen [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties).</span><span class="sxs-lookup"><span data-stu-id="3e79f-501">An input [dataset](data-factory-create-datasets.md) of type [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties).</span></span>
4. <span data-ttu-id="3e79f-502">Utdata [dataset](data-factory-create-datasets.md) av typen [AzureSqlDWTable](#dataset-properties).</span><span class="sxs-lookup"><span data-stu-id="3e79f-502">An output [dataset](data-factory-create-datasets.md) of type [AzureSqlDWTable](#dataset-properties).</span></span>
5. <span data-ttu-id="3e79f-503">En [pipeline](data-factory-create-pipelines.md) med kopieringsaktiviteten som använder [BlobSource](data-factory-azure-blob-connector.md#copy-activity-properties) och [SqlDWSink](#copy-activity-properties).</span><span class="sxs-lookup"><span data-stu-id="3e79f-503">A [pipeline](data-factory-create-pipelines.md) with Copy activity that uses [BlobSource](data-factory-azure-blob-connector.md#copy-activity-properties) and [SqlDWSink](#copy-activity-properties).</span></span>

<span data-ttu-id="3e79f-504">hello exemplet kopierar time series-data (varje timme, varje dag, etc.) från Azure blob tooa tabell i Azure SQL Data Warehouse-databas i timmen.</span><span class="sxs-lookup"><span data-stu-id="3e79f-504">hello sample copies time-series data (hourly, daily, etc.) from Azure blob tooa table in Azure SQL Data Warehouse database every hour.</span></span> <span data-ttu-id="3e79f-505">hello JSON egenskaper som används i exemplen beskrivs i hello-exempel i följande avsnitt.</span><span class="sxs-lookup"><span data-stu-id="3e79f-505">hello JSON properties used in these samples are described in sections following hello samples.</span></span>

<span data-ttu-id="3e79f-506">**Azure SQL Data Warehouse länkade tjänsten:**</span><span class="sxs-lookup"><span data-stu-id="3e79f-506">**Azure SQL Data Warehouse linked service:**</span></span>

```JSON
{
  "name": "AzureSqlDWLinkedService",
  "properties": {
    "type": "AzureSqlDW",
    "typeProperties": {
      "connectionString": "Server=tcp:<servername>.database.windows.net,1433;Database=<databasename>;User ID=<username>@<servername>;Password=<password>;Trusted_Connection=False;Encrypt=True;Connection Timeout=30"
    }
  }
}
```
<span data-ttu-id="3e79f-507">**Azure Blob storage länkade tjänsten:**</span><span class="sxs-lookup"><span data-stu-id="3e79f-507">**Azure Blob storage linked service:**</span></span>

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
<span data-ttu-id="3e79f-508">**Azure Blob inkommande datamängd:**</span><span class="sxs-lookup"><span data-stu-id="3e79f-508">**Azure Blob input dataset:**</span></span>

<span data-ttu-id="3e79f-509">Data hämtas från en ny blob varje timme (frekvens: timme, intervall: 1).</span><span class="sxs-lookup"><span data-stu-id="3e79f-509">Data is picked up from a new blob every hour (frequency: hour, interval: 1).</span></span> <span data-ttu-id="3e79f-510">hello mappen sökvägen och filnamnet för hello blob utvärderas dynamiskt baserat på hello starttiden för hello-segment som bearbetas.</span><span class="sxs-lookup"><span data-stu-id="3e79f-510">hello folder path and file name for hello blob are dynamically evaluated based on hello start time of hello slice that is being processed.</span></span> <span data-ttu-id="3e79f-511">hello mappsökväg använder år, månad och som en dagdel av hello starttid och filnamn använder hello timme tillhör hello starttid.</span><span class="sxs-lookup"><span data-stu-id="3e79f-511">hello folder path uses year, month, and day part of hello start time and file name uses hello hour part of hello start time.</span></span> <span data-ttu-id="3e79f-512">”externa”: ”true” inställningen informerar hello Data Factory-tjänsten att den här tabellen är externa toohello data factory och inte tillverkas av en aktivitet i hello data factory.</span><span class="sxs-lookup"><span data-stu-id="3e79f-512">“external”: “true” setting informs hello Data Factory service that this table is external toohello data factory and is not produced by an activity in hello data factory.</span></span>

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
<span data-ttu-id="3e79f-513">**Azure SQL Data Warehouse utdatauppsättningen:**</span><span class="sxs-lookup"><span data-stu-id="3e79f-513">**Azure SQL Data Warehouse output dataset:**</span></span>

<span data-ttu-id="3e79f-514">hello exemplet kopierar data tooa tabell med namnet ”mytable” som prefix i Azure SQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="3e79f-514">hello sample copies data tooa table named “MyTable” in Azure SQL Data Warehouse.</span></span> <span data-ttu-id="3e79f-515">Skapa hello tabell i Azure SQL Data Warehouse med hello samma antal kolumner som du förväntar dig hello Blob CSV-filen toocontain.</span><span class="sxs-lookup"><span data-stu-id="3e79f-515">Create hello table in Azure SQL Data Warehouse with hello same number of columns as you expect hello Blob CSV file toocontain.</span></span> <span data-ttu-id="3e79f-516">Nya rader läggs toohello tabell varje timme.</span><span class="sxs-lookup"><span data-stu-id="3e79f-516">New rows are added toohello table every hour.</span></span>

```JSON
{
  "name": "AzureSqlDWOutput",
  "properties": {
    "type": "AzureSqlDWTable",
    "linkedServiceName": "AzureSqlDWLinkedService",
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
<span data-ttu-id="3e79f-517">**Kopiera aktivitet i en pipeline med BlobSource och SqlDWSink:**</span><span class="sxs-lookup"><span data-stu-id="3e79f-517">**Copy activity in a pipeline with BlobSource and SqlDWSink:**</span></span>

<span data-ttu-id="3e79f-518">hello pipelinen innehåller en kopia-aktivitet som är konfigurerade toouse hello inkommande och utgående datauppsättningar och är schemalagda toorun varje timme.</span><span class="sxs-lookup"><span data-stu-id="3e79f-518">hello pipeline contains a Copy Activity that is configured toouse hello input and output datasets and is scheduled toorun every hour.</span></span> <span data-ttu-id="3e79f-519">I hello pipeline JSON-definitionen hello **källa** typ har angetts för**BlobSource** och **sink** typ har angetts för**SqlDWSink**.</span><span class="sxs-lookup"><span data-stu-id="3e79f-519">In hello pipeline JSON definition, hello **source** type is set too**BlobSource** and **sink** type is set too**SqlDWSink**.</span></span>

```JSON
{  
    "name":"SamplePipeline",
    "properties":{  
    "start":"2014-06-01T18:00:00",
    "end":"2014-06-01T19:00:00",
    "description":"pipeline with copy activity",
    "activities":[  
      {
        "name": "AzureBlobtoSQLDW",
        "description": "Copy Activity",
        "type": "Copy",
        "inputs": [
          {
            "name": "AzureBlobInput"
          }
        ],
        "outputs": [
          {
            "name": "AzureSqlDWOutput"
          }
        ],
        "typeProperties": {
          "source": {
            "type": "BlobSource",
            "blobColumnSeparators": ","
          },
          "sink": {
            "type": "SqlDWSink",
            "allowPolyBase": true
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
<span data-ttu-id="3e79f-520">En genomgång finns hello finns [läsa in 1 TB i Azure SQL Data Warehouse under 15 minuter med Azure Data Factory](data-factory-load-sql-data-warehouse.md) och [Läs in data med Azure Data Factory](../sql-data-warehouse/sql-data-warehouse-get-started-load-with-azure-data-factory.md) artikeln i hello Azure SQL Data Warehouse dokumentation.</span><span class="sxs-lookup"><span data-stu-id="3e79f-520">For a walkthrough, see hello see [Load 1 TB into Azure SQL Data Warehouse under 15 minutes with Azure Data Factory](data-factory-load-sql-data-warehouse.md) and [Load data with Azure Data Factory](../sql-data-warehouse/sql-data-warehouse-get-started-load-with-azure-data-factory.md) article in hello Azure SQL Data Warehouse documentation.</span></span>

## <a name="performance-and-tuning"></a><span data-ttu-id="3e79f-521">Prestanda och finjustering</span><span class="sxs-lookup"><span data-stu-id="3e79f-521">Performance and Tuning</span></span>
<span data-ttu-id="3e79f-522">Se [kopiera aktivitet prestanda och justera guiden](data-factory-copy-activity-performance.md) toolearn om nyckeln faktorer som påverkan prestanda för flytt av data (Kopieringsaktiviteten) i Azure Data Factory och olika sätt toooptimize den.</span><span class="sxs-lookup"><span data-stu-id="3e79f-522">See [Copy Activity Performance & Tuning Guide](data-factory-copy-activity-performance.md) toolearn about key factors that impact performance of data movement (Copy Activity) in Azure Data Factory and various ways toooptimize it.</span></span>
