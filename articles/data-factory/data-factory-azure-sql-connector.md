---
title: "aaaCopy data till/från Azure SQL Database | Microsoft Docs"
description: "Lär dig hur toocopy data till/från Azure SQL Database med Azure Data Factory."
services: data-factory
documentationcenter: 
author: linda33wj
manager: jhubbard
editor: monicar
ms.assetid: 484f735b-8464-40ba-a9fc-820e6553159e
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/04/2017
ms.author: jingwang
ms.openlocfilehash: d2ff16191afb028da75699c5e4d0bb310538db0f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="copy-data-tooand-from-azure-sql-database-using-azure-data-factory"></a><span data-ttu-id="5f639-103">Kopiera data tooand från Azure SQL Database med Azure Data Factory</span><span class="sxs-lookup"><span data-stu-id="5f639-103">Copy data tooand from Azure SQL Database using Azure Data Factory</span></span>
<span data-ttu-id="5f639-104">Den här artikeln förklarar hur toouse hello Kopieringsaktiviteten i Azure Data Factory toomove data tooand från Azure SQL-databas.</span><span class="sxs-lookup"><span data-stu-id="5f639-104">This article explains how toouse hello Copy Activity in Azure Data Factory toomove data tooand from Azure SQL Database.</span></span> <span data-ttu-id="5f639-105">Den bygger på hello [Data Movement aktiviteter](data-factory-data-movement-activities.md) artikel som ger en allmän översikt över dataflyttning hello kopieringsaktiviteten.</span><span class="sxs-lookup"><span data-stu-id="5f639-105">It builds on hello [Data Movement Activities](data-factory-data-movement-activities.md) article, which presents a general overview of data movement with hello copy activity.</span></span>  

## <a name="supported-scenarios"></a><span data-ttu-id="5f639-106">Scenarier som stöds</span><span class="sxs-lookup"><span data-stu-id="5f639-106">Supported scenarios</span></span>
<span data-ttu-id="5f639-107">Du kan kopiera data **från Azure SQL Database** toohello följande datakällor:</span><span class="sxs-lookup"><span data-stu-id="5f639-107">You can copy data **from Azure SQL Database** toohello following data stores:</span></span>

[!INCLUDE [data-factory-supported-sinks](../../includes/data-factory-supported-sinks.md)]

<span data-ttu-id="5f639-108">Du kan kopiera data från hello följande datalager **tooAzure SQL-databas**:</span><span class="sxs-lookup"><span data-stu-id="5f639-108">You can copy data from hello following data stores **tooAzure SQL Database**:</span></span>

[!INCLUDE [data-factory-supported-sources](../../includes/data-factory-supported-sources.md)]

## <a name="supported-authentication-type"></a><span data-ttu-id="5f639-109">Autentiseringstypen som stöds</span><span class="sxs-lookup"><span data-stu-id="5f639-109">Supported authentication type</span></span>
<span data-ttu-id="5f639-110">Azure SQL Database-anslutningen har stöd för grundläggande autentisering.</span><span class="sxs-lookup"><span data-stu-id="5f639-110">Azure SQL Database connector supports basic authentication.</span></span>

## <a name="getting-started"></a><span data-ttu-id="5f639-111">Komma igång</span><span class="sxs-lookup"><span data-stu-id="5f639-111">Getting started</span></span>
<span data-ttu-id="5f639-112">Du kan skapa en pipeline med en kopia-aktivitet som flyttar data till eller från en Azure SQL Database med hjälp av olika verktyg/API: er.</span><span class="sxs-lookup"><span data-stu-id="5f639-112">You can create a pipeline with a copy activity that moves data to/from an Azure SQL Database by using different tools/APIs.</span></span>

<span data-ttu-id="5f639-113">hello enklaste sättet toocreate en pipeline är toouse hello **guiden Kopiera**.</span><span class="sxs-lookup"><span data-stu-id="5f639-113">hello easiest way toocreate a pipeline is toouse hello **Copy Wizard**.</span></span> <span data-ttu-id="5f639-114">Se [Självstudier: skapa en pipeline med hjälp av guiden Kopiera](data-factory-copy-data-wizard-tutorial.md) för en snabb genomgång om hur du skapar en pipeline med hjälp av guiden för hello kopiera data.</span><span class="sxs-lookup"><span data-stu-id="5f639-114">See [Tutorial: Create a pipeline using Copy Wizard](data-factory-copy-data-wizard-tutorial.md) for a quick walkthrough on creating a pipeline using hello Copy data wizard.</span></span>

<span data-ttu-id="5f639-115">Du kan också använda följande verktyg toocreate en pipeline hello: **Azure-portalen**, **Visual Studio**, **Azure PowerShell**, **Azure Resource Manager-mall** , **.NET API**, och **REST API**.</span><span class="sxs-lookup"><span data-stu-id="5f639-115">You can also use hello following tools toocreate a pipeline: **Azure portal**, **Visual Studio**, **Azure PowerShell**, **Azure Resource Manager template**, **.NET API**, and **REST API**.</span></span> <span data-ttu-id="5f639-116">Se [kopiera aktivitet kursen](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) för stegvisa instruktioner toocreate en pipeline med en Kopieringsaktivitet.</span><span class="sxs-lookup"><span data-stu-id="5f639-116">See [Copy activity tutorial](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) for step-by-step instructions toocreate a pipeline with a copy activity.</span></span> 

<span data-ttu-id="5f639-117">Om du använder hello verktyg eller API: er kan utföra du hello följande steg toocreate en pipeline som flyttar data från en källdata lagra tooa sink-datalagret:</span><span class="sxs-lookup"><span data-stu-id="5f639-117">Whether you use hello tools or APIs, you perform hello following steps toocreate a pipeline that moves data from a source data store tooa sink data store:</span></span> 

1. <span data-ttu-id="5f639-118">Skapa en **datafabriken**.</span><span class="sxs-lookup"><span data-stu-id="5f639-118">Create a **data factory**.</span></span> <span data-ttu-id="5f639-119">En datafabrik kan innehålla en eller flera pipelines.</span><span class="sxs-lookup"><span data-stu-id="5f639-119">A data factory may contain one or more pipelines.</span></span> 
2. <span data-ttu-id="5f639-120">Skapa **länkade tjänster** toolink indata och utdata lagrar tooyour data factory.</span><span class="sxs-lookup"><span data-stu-id="5f639-120">Create **linked services** toolink input and output data stores tooyour data factory.</span></span> <span data-ttu-id="5f639-121">Till exempel om du kopierar data från en Azure blob storage tooan Azure SQL database skapa du två länkade tjänster toolink dina Azure storage-konto och Azure SQL database tooyour data factory.</span><span class="sxs-lookup"><span data-stu-id="5f639-121">For example, if you are copying data from an Azure blob storage tooan Azure SQL database, you create two linked services toolink your Azure storage account and Azure SQL database tooyour data factory.</span></span> <span data-ttu-id="5f639-122">Länkad tjänstegenskaper som är specifika tooAzure SQL-databasen, se [länkade tjänstegenskaper](#linked-service-properties) avsnitt.</span><span class="sxs-lookup"><span data-stu-id="5f639-122">For linked service properties that are specific tooAzure SQL Database, see [linked service properties](#linked-service-properties) section.</span></span> 
3. <span data-ttu-id="5f639-123">Skapa **datauppsättningar** toorepresent indata och utdata för hello kopieringsåtgärden.</span><span class="sxs-lookup"><span data-stu-id="5f639-123">Create **datasets** toorepresent input and output data for hello copy operation.</span></span> <span data-ttu-id="5f639-124">I hello exempelvis nämns i hello sista steget skapar du en dataset toospecify hello blob-behållaren och mappen som innehåller hello indata.</span><span class="sxs-lookup"><span data-stu-id="5f639-124">In hello example mentioned in hello last step, you create a dataset toospecify hello blob container and folder that contains hello input data.</span></span> <span data-ttu-id="5f639-125">Och du skapar en annan dataset toospecify hello SQL-tabellen i hello Azure SQL-databas som innehåller hello data som kopieras från hello blob storage.</span><span class="sxs-lookup"><span data-stu-id="5f639-125">And, you create another dataset toospecify hello SQL table in hello Azure SQL database  that holds hello data copied from hello blob storage.</span></span> <span data-ttu-id="5f639-126">Egenskaper för datamängd som är specifika tooAzure Data Lake Store, se [egenskaper för datamängd](#dataset-properties) avsnitt.</span><span class="sxs-lookup"><span data-stu-id="5f639-126">For dataset properties that are specific tooAzure Data Lake Store, see [dataset properties](#dataset-properties) section.</span></span>
4. <span data-ttu-id="5f639-127">Skapa en **pipeline** med en kopia-aktivitet som tar en datamängd som indata och en dataset som utdata.</span><span class="sxs-lookup"><span data-stu-id="5f639-127">Create a **pipeline** with a copy activity that takes a dataset as an input and a dataset as an output.</span></span> <span data-ttu-id="5f639-128">I hello-exemplet ovan, använder du BlobSource som en källa och SqlSink som en mottagare för hello kopieringsaktiviteten.</span><span class="sxs-lookup"><span data-stu-id="5f639-128">In hello example mentioned earlier, you use BlobSource as a source and SqlSink as a sink for hello copy activity.</span></span> <span data-ttu-id="5f639-129">På samma sätt om du vill kopiera från Azure SQL Database tooAzure Blob Storage, använder du SqlSource och BlobSink i hello kopieringsaktiviteten.</span><span class="sxs-lookup"><span data-stu-id="5f639-129">Similarly, if you are copying from Azure SQL Database tooAzure Blob Storage, you use SqlSource and BlobSink in hello copy activity.</span></span> <span data-ttu-id="5f639-130">Kopiera Aktivitetsegenskaper som är specifika tooAzure SQL-databasen, se [kopiera Aktivitetsegenskaper](#copy-activity-properties) avsnitt.</span><span class="sxs-lookup"><span data-stu-id="5f639-130">For copy activity properties that are specific tooAzure SQL Database, see [copy activity properties](#copy-activity-properties) section.</span></span> <span data-ttu-id="5f639-131">Mer information om hur toouse en databas som en källa eller en mottagare klickar du på hello länk under föregående hello för datalager.</span><span class="sxs-lookup"><span data-stu-id="5f639-131">For details on how toouse a data store as a source or a sink, click hello link in hello previous section for your data store.</span></span>

<span data-ttu-id="5f639-132">När du använder guiden hello skapas automatiskt JSON definitioner för dessa Data Factory-enheter (länkade tjänster, datauppsättningar och hello pipeline) för dig.</span><span class="sxs-lookup"><span data-stu-id="5f639-132">When you use hello wizard, JSON definitions for these Data Factory entities (linked services, datasets, and hello pipeline) are automatically created for you.</span></span> <span data-ttu-id="5f639-133">När du använder Verktyg/API: er (utom .NET API), kan du definiera dessa Data Factory-enheter med hjälp av hello JSON-format.</span><span class="sxs-lookup"><span data-stu-id="5f639-133">When you use tools/APIs (except .NET API), you define these Data Factory entities by using hello JSON format.</span></span>  <span data-ttu-id="5f639-134">Exempel med JSON-definitioner för Data Factory-entiteter som har använt toocopy data till/från en Azure SQL Database finns [JSON-exempel](#json-examples-for-copying-data-to-and-from-sql-database) i den här artikeln.</span><span class="sxs-lookup"><span data-stu-id="5f639-134">For samples with JSON definitions for Data Factory entities that are used toocopy data to/from an Azure SQL Database, see [JSON examples](#json-examples-for-copying-data-to-and-from-sql-database) section of this article.</span></span> 

<span data-ttu-id="5f639-135">hello följande avsnitt innehåller information om JSON-egenskaper används toodefine Data Factory entiteter specifika tooAzure SQL-databas:</span><span class="sxs-lookup"><span data-stu-id="5f639-135">hello following sections provide details about JSON properties that are used toodefine Data Factory entities specific tooAzure SQL Database:</span></span> 

## <a name="linked-service-properties"></a><span data-ttu-id="5f639-136">Länkad tjänstegenskaper</span><span class="sxs-lookup"><span data-stu-id="5f639-136">Linked service properties</span></span>
<span data-ttu-id="5f639-137">En Azure SQL länkade tjänsten länkar en Azure SQL database tooyour data factory.</span><span class="sxs-lookup"><span data-stu-id="5f639-137">An Azure SQL linked service links an Azure SQL database tooyour data factory.</span></span> <span data-ttu-id="5f639-138">hello följande tabell innehåller beskrivning för JSON-element specifika tooAzure SQL länkade tjänsten.</span><span class="sxs-lookup"><span data-stu-id="5f639-138">hello following table provides description for JSON elements specific tooAzure SQL linked service.</span></span>

| <span data-ttu-id="5f639-139">Egenskap</span><span class="sxs-lookup"><span data-stu-id="5f639-139">Property</span></span> | <span data-ttu-id="5f639-140">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="5f639-140">Description</span></span> | <span data-ttu-id="5f639-141">Krävs</span><span class="sxs-lookup"><span data-stu-id="5f639-141">Required</span></span> |
| --- | --- | --- |
| <span data-ttu-id="5f639-142">typ</span><span class="sxs-lookup"><span data-stu-id="5f639-142">type</span></span> |<span data-ttu-id="5f639-143">hello Typegenskapen måste anges till: **AzureSqlDatabase**</span><span class="sxs-lookup"><span data-stu-id="5f639-143">hello type property must be set to: **AzureSqlDatabase**</span></span> |<span data-ttu-id="5f639-144">Ja</span><span class="sxs-lookup"><span data-stu-id="5f639-144">Yes</span></span> |
| <span data-ttu-id="5f639-145">connectionString</span><span class="sxs-lookup"><span data-stu-id="5f639-145">connectionString</span></span> |<span data-ttu-id="5f639-146">Ange nödvändig information tooconnect toohello Azure SQL Database-instans för hello-egenskapen connectionString.</span><span class="sxs-lookup"><span data-stu-id="5f639-146">Specify information needed tooconnect toohello Azure SQL Database instance for hello connectionString property.</span></span> <span data-ttu-id="5f639-147">Endast grundläggande autentisering stöds.</span><span class="sxs-lookup"><span data-stu-id="5f639-147">Only basic authentication is supported.</span></span> |<span data-ttu-id="5f639-148">Ja</span><span class="sxs-lookup"><span data-stu-id="5f639-148">Yes</span></span> |

> [!IMPORTANT]
> <span data-ttu-id="5f639-149">Konfigurera [Azure SQL Database-brandvägg](https://msdn.microsoft.com/library/azure/ee621782.aspx#ConnectingFromAzure) hello databasserver för[Tillåt Azure Services tooaccess hello server](https://msdn.microsoft.com/library/azure/ee621782.aspx#ConnectingFromAzure).</span><span class="sxs-lookup"><span data-stu-id="5f639-149">Configure [Azure SQL Database Firewall](https://msdn.microsoft.com/library/azure/ee621782.aspx#ConnectingFromAzure) hello database server too[allow Azure Services tooaccess hello server](https://msdn.microsoft.com/library/azure/ee621782.aspx#ConnectingFromAzure).</span></span> <span data-ttu-id="5f639-150">Om du kopierar data tooAzure SQL-databas från utanför Azure inklusive från lokala datakällor med data factory-gateway måste du dessutom konfigurera lämplig IP-adressintervall för hello-dator som skickar data tooAzure SQL-databas.</span><span class="sxs-lookup"><span data-stu-id="5f639-150">Additionally, if you are copying data tooAzure SQL Database from outside Azure including from on-premises data sources with data factory gateway, configure appropriate IP address range for hello machine that is sending data tooAzure SQL Database.</span></span>

## <a name="dataset-properties"></a><span data-ttu-id="5f639-151">Egenskaper för datamängd</span><span class="sxs-lookup"><span data-stu-id="5f639-151">Dataset properties</span></span>
<span data-ttu-id="5f639-152">toospecify en dataset toorepresent inkommande eller utgående data i en Azure SQL database, anger du egenskapen hello typ av hello datamängden: **AzureSqlTable**.</span><span class="sxs-lookup"><span data-stu-id="5f639-152">toospecify a dataset toorepresent input or output data in an Azure SQL database, you set hello type property of hello dataset to: **AzureSqlTable**.</span></span> <span data-ttu-id="5f639-153">Ange hello **linkedServiceName** länkad egenskap hello dataset toohello namnet på hello Azure SQL-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="5f639-153">Set hello **linkedServiceName** property of hello dataset toohello name of hello Azure SQL linked service.</span></span>  

<span data-ttu-id="5f639-154">En fullständig lista över egenskaper som är tillgängliga för att definiera datauppsättningarna & avsnitt finns hello [skapa datauppsättningar](data-factory-create-datasets.md) artikel.</span><span class="sxs-lookup"><span data-stu-id="5f639-154">For a full list of sections & properties available for defining datasets, see hello [Creating datasets](data-factory-create-datasets.md) article.</span></span> <span data-ttu-id="5f639-155">Avsnitt som struktur, tillgänglighet och princip på en datamängd JSON är liknande för alla typer av dataset (Azure SQL Azure blob, Azure-tabellen, osv.).</span><span class="sxs-lookup"><span data-stu-id="5f639-155">Sections such as structure, availability, and policy of a dataset JSON are similar for all dataset types (Azure SQL, Azure blob, Azure table, etc.).</span></span>

<span data-ttu-id="5f639-156">hello typeProperties avsnittet är olika för varje typ av dataset och ger information om hello platsen för hello data i datalagret hello.</span><span class="sxs-lookup"><span data-stu-id="5f639-156">hello typeProperties section is different for each type of dataset and provides information about hello location of hello data in hello data store.</span></span> <span data-ttu-id="5f639-157">Hej **typeProperties** avsnittet för hello dataset av typen **AzureSqlTable** har hello följande egenskaper:</span><span class="sxs-lookup"><span data-stu-id="5f639-157">hello **typeProperties** section for hello dataset of type **AzureSqlTable** has hello following properties:</span></span>

| <span data-ttu-id="5f639-158">Egenskap</span><span class="sxs-lookup"><span data-stu-id="5f639-158">Property</span></span> | <span data-ttu-id="5f639-159">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="5f639-159">Description</span></span> | <span data-ttu-id="5f639-160">Krävs</span><span class="sxs-lookup"><span data-stu-id="5f639-160">Required</span></span> |
| --- | --- | --- |
| <span data-ttu-id="5f639-161">tableName</span><span class="sxs-lookup"><span data-stu-id="5f639-161">tableName</span></span> |<span data-ttu-id="5f639-162">Namnet på hello tabellen eller vyn i hello Azure SQL Database-instans som den länkade tjänsten refererar till.</span><span class="sxs-lookup"><span data-stu-id="5f639-162">Name of hello table or view in hello Azure SQL Database instance that linked service refers to.</span></span> |<span data-ttu-id="5f639-163">Ja</span><span class="sxs-lookup"><span data-stu-id="5f639-163">Yes</span></span> |

## <a name="copy-activity-properties"></a><span data-ttu-id="5f639-164">Kopiera egenskaper för aktivitet</span><span class="sxs-lookup"><span data-stu-id="5f639-164">Copy activity properties</span></span>
<span data-ttu-id="5f639-165">En fullständig lista över avsnitt & egenskaper som är tillgängliga för att definiera aktiviteter finns hello [skapar Pipelines](data-factory-create-pipelines.md) artikel.</span><span class="sxs-lookup"><span data-stu-id="5f639-165">For a full list of sections & properties available for defining activities, see hello [Creating Pipelines](data-factory-create-pipelines.md) article.</span></span> <span data-ttu-id="5f639-166">Egenskaper som namn, beskrivning, ingående och utgående tabeller och principen är tillgängliga för alla typer av aktiviteter.</span><span class="sxs-lookup"><span data-stu-id="5f639-166">Properties such as name, description, input and output tables, and policy are available for all types of activities.</span></span>

> [!NOTE]
> <span data-ttu-id="5f639-167">Hej Kopieringsaktiviteten tar endast en inmatning och ger en enda utdata.</span><span class="sxs-lookup"><span data-stu-id="5f639-167">hello Copy Activity takes only one input and produces only one output.</span></span>

<span data-ttu-id="5f639-168">Medan egenskaper som är tillgängliga i hello **typeProperties** avsnittet hello aktivitet varierar med varje aktivitetstyp.</span><span class="sxs-lookup"><span data-stu-id="5f639-168">Whereas, properties available in hello **typeProperties** section of hello activity vary with each activity type.</span></span> <span data-ttu-id="5f639-169">För Kopieringsaktivitet kan varierar de beroende på hello typer av datakällor och sänkor.</span><span class="sxs-lookup"><span data-stu-id="5f639-169">For Copy activity, they vary depending on hello types of sources and sinks.</span></span>

<span data-ttu-id="5f639-170">Om du flyttar data från en Azure SQL database måste du ange hello källtypen i hello kopieringsaktiviteten för**SqlSource**.</span><span class="sxs-lookup"><span data-stu-id="5f639-170">If you are moving data from an Azure SQL database, you set hello source type in hello copy activity too**SqlSource**.</span></span> <span data-ttu-id="5f639-171">På samma sätt om du flyttar data tooan Azure SQL-databas måste du ange hello Mottagartypen i hello kopieringsaktiviteten för**SqlSink**.</span><span class="sxs-lookup"><span data-stu-id="5f639-171">Similarly, if you are moving data tooan Azure SQL database, you set hello sink type in hello copy activity too**SqlSink**.</span></span> <span data-ttu-id="5f639-172">Det här avsnittet innehåller en lista över egenskaper som stöds av SqlSource och SqlSink.</span><span class="sxs-lookup"><span data-stu-id="5f639-172">This section provides a list of properties supported by SqlSource and SqlSink.</span></span>

### <a name="sqlsource"></a><span data-ttu-id="5f639-173">SqlSource</span><span class="sxs-lookup"><span data-stu-id="5f639-173">SqlSource</span></span>
<span data-ttu-id="5f639-174">I en Kopieringsaktivitet när hello källa är av typen **SqlSource**, hello följande egenskaper är tillgängliga i **typeProperties** avsnitt:</span><span class="sxs-lookup"><span data-stu-id="5f639-174">In copy activity, when hello source is of type **SqlSource**, hello following properties are available in **typeProperties** section:</span></span>

| <span data-ttu-id="5f639-175">Egenskap</span><span class="sxs-lookup"><span data-stu-id="5f639-175">Property</span></span> | <span data-ttu-id="5f639-176">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="5f639-176">Description</span></span> | <span data-ttu-id="5f639-177">Tillåtna värden</span><span class="sxs-lookup"><span data-stu-id="5f639-177">Allowed values</span></span> | <span data-ttu-id="5f639-178">Krävs</span><span class="sxs-lookup"><span data-stu-id="5f639-178">Required</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="5f639-179">sqlReaderQuery</span><span class="sxs-lookup"><span data-stu-id="5f639-179">sqlReaderQuery</span></span> |<span data-ttu-id="5f639-180">Använda hello anpassad fråga tooread data.</span><span class="sxs-lookup"><span data-stu-id="5f639-180">Use hello custom query tooread data.</span></span> |<span data-ttu-id="5f639-181">SQL-sträng.</span><span class="sxs-lookup"><span data-stu-id="5f639-181">SQL query string.</span></span> <span data-ttu-id="5f639-182">Exempel: `select * from MyTable`.</span><span class="sxs-lookup"><span data-stu-id="5f639-182">Example: `select * from MyTable`.</span></span> |<span data-ttu-id="5f639-183">Nej</span><span class="sxs-lookup"><span data-stu-id="5f639-183">No</span></span> |
| <span data-ttu-id="5f639-184">sqlReaderStoredProcedureName</span><span class="sxs-lookup"><span data-stu-id="5f639-184">sqlReaderStoredProcedureName</span></span> |<span data-ttu-id="5f639-185">Namnet på hello lagrad procedur som läser data från hello källtabellen.</span><span class="sxs-lookup"><span data-stu-id="5f639-185">Name of hello stored procedure that reads data from hello source table.</span></span> |<span data-ttu-id="5f639-186">Namnet på hello lagrad procedur.</span><span class="sxs-lookup"><span data-stu-id="5f639-186">Name of hello stored procedure.</span></span> <span data-ttu-id="5f639-187">hello senaste SQL-instruktionen måste vara en SELECT-instruktion i hello lagrad procedur.</span><span class="sxs-lookup"><span data-stu-id="5f639-187">hello last SQL statement must be a SELECT statement in hello stored procedure.</span></span> |<span data-ttu-id="5f639-188">Nej</span><span class="sxs-lookup"><span data-stu-id="5f639-188">No</span></span> |
| <span data-ttu-id="5f639-189">storedProcedureParameters</span><span class="sxs-lookup"><span data-stu-id="5f639-189">storedProcedureParameters</span></span> |<span data-ttu-id="5f639-190">Parametrar för hello lagrad procedur.</span><span class="sxs-lookup"><span data-stu-id="5f639-190">Parameters for hello stored procedure.</span></span> |<span data-ttu-id="5f639-191">Namn/värde-par.</span><span class="sxs-lookup"><span data-stu-id="5f639-191">Name/value pairs.</span></span> <span data-ttu-id="5f639-192">Namn och skiftläge parametrar måste matcha hello namn och versaler och gemener i hello lagrade procedurparametrar.</span><span class="sxs-lookup"><span data-stu-id="5f639-192">Names and casing of parameters must match hello names and casing of hello stored procedure parameters.</span></span> |<span data-ttu-id="5f639-193">Nej</span><span class="sxs-lookup"><span data-stu-id="5f639-193">No</span></span> |

<span data-ttu-id="5f639-194">Om hello **sqlReaderQuery** har angetts för hello SqlSource, hello Kopieringsaktiviteten kör den här frågan mot hello Azure SQL Database källa tooget hello data.</span><span class="sxs-lookup"><span data-stu-id="5f639-194">If hello **sqlReaderQuery** is specified for hello SqlSource, hello Copy Activity runs this query against hello Azure SQL Database source tooget hello data.</span></span> <span data-ttu-id="5f639-195">Du kan också ange en lagrad procedur genom att ange hello **sqlReaderStoredProcedureName** och **storedProcedureParameters** (om hello tar lagrade proceduren parametrar).</span><span class="sxs-lookup"><span data-stu-id="5f639-195">Alternatively, you can specify a stored procedure by specifying hello **sqlReaderStoredProcedureName** and **storedProcedureParameters** (if hello stored procedure takes parameters).</span></span>

<span data-ttu-id="5f639-196">Om du inte anger sqlReaderQuery eller sqlReaderStoredProcedureName hello kolumner som anges i hello strukturen i hello dataset JSON är används toobuild en fråga (`select column1, column2 from mytable`) toorun mot hello Azure SQL Database.</span><span class="sxs-lookup"><span data-stu-id="5f639-196">If you do not specify either sqlReaderQuery or sqlReaderStoredProcedureName, hello columns defined in hello structure section of hello dataset JSON are used toobuild a query (`select column1, column2 from mytable`) toorun against hello Azure SQL Database.</span></span> <span data-ttu-id="5f639-197">Om hello datauppsättningsdefinitionen inte har hello struktur, markeras alla kolumner från tabellen hello.</span><span class="sxs-lookup"><span data-stu-id="5f639-197">If hello dataset definition does not have hello structure, all columns are selected from hello table.</span></span>

> [!NOTE]
> <span data-ttu-id="5f639-198">När du använder **sqlReaderStoredProcedureName**, du behöver fortfarande toospecify ett värde för hello **tableName** egenskap i hello dataset JSON.</span><span class="sxs-lookup"><span data-stu-id="5f639-198">When you use **sqlReaderStoredProcedureName**, you still need toospecify a value for hello **tableName** property in hello dataset JSON.</span></span> <span data-ttu-id="5f639-199">Det finns inga verifieringar utföras om mot den här tabellen.</span><span class="sxs-lookup"><span data-stu-id="5f639-199">There are no validations performed against this table though.</span></span>
>
>

### <a name="sqlsource-example"></a><span data-ttu-id="5f639-200">SqlSource-exempel</span><span class="sxs-lookup"><span data-stu-id="5f639-200">SqlSource example</span></span>

```JSON
"source": {
    "type": "SqlSource",
    "sqlReaderStoredProcedureName": "CopyTestSrcStoredProcedureWithParameters",
    "storedProcedureParameters": {
        "stringData": { "value": "str3" },
        "identifier": { "value": "$$Text.Format('{0:yyyy}', SliceStart)", "type": "Int"}
    }
}
```

<span data-ttu-id="5f639-201">**definition av hello lagrade proceduren:**</span><span class="sxs-lookup"><span data-stu-id="5f639-201">**hello stored procedure definition:**</span></span>

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

### <a name="sqlsink"></a><span data-ttu-id="5f639-202">SqlSink</span><span class="sxs-lookup"><span data-stu-id="5f639-202">SqlSink</span></span>
<span data-ttu-id="5f639-203">**SqlSink** stöder hello följande egenskaper:</span><span class="sxs-lookup"><span data-stu-id="5f639-203">**SqlSink** supports hello following properties:</span></span>

| <span data-ttu-id="5f639-204">Egenskap</span><span class="sxs-lookup"><span data-stu-id="5f639-204">Property</span></span> | <span data-ttu-id="5f639-205">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="5f639-205">Description</span></span> | <span data-ttu-id="5f639-206">Tillåtna värden</span><span class="sxs-lookup"><span data-stu-id="5f639-206">Allowed values</span></span> | <span data-ttu-id="5f639-207">Krävs</span><span class="sxs-lookup"><span data-stu-id="5f639-207">Required</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="5f639-208">writeBatchTimeout</span><span class="sxs-lookup"><span data-stu-id="5f639-208">writeBatchTimeout</span></span> |<span data-ttu-id="5f639-209">Vänta tills hello batch insert-åtgärden toocomplete innan tidsgränsen uppnås.</span><span class="sxs-lookup"><span data-stu-id="5f639-209">Wait time for hello batch insert operation toocomplete before it times out.</span></span> |<span data-ttu-id="5f639-210">TimeSpan</span><span class="sxs-lookup"><span data-stu-id="5f639-210">timespan</span></span><br/><br/> <span data-ttu-id="5f639-211">Exempel ”: 00: 30:00” (30 minuter).</span><span class="sxs-lookup"><span data-stu-id="5f639-211">Example: “00:30:00” (30 minutes).</span></span> |<span data-ttu-id="5f639-212">Nej</span><span class="sxs-lookup"><span data-stu-id="5f639-212">No</span></span> |
| <span data-ttu-id="5f639-213">writeBatchSize</span><span class="sxs-lookup"><span data-stu-id="5f639-213">writeBatchSize</span></span> |<span data-ttu-id="5f639-214">Infogar data i hello SQL-tabellen när hello buffertstorlek når writeBatchSize.</span><span class="sxs-lookup"><span data-stu-id="5f639-214">Inserts data into hello SQL table when hello buffer size reaches writeBatchSize.</span></span> |<span data-ttu-id="5f639-215">Heltal (antalet rader)</span><span class="sxs-lookup"><span data-stu-id="5f639-215">Integer (number of rows)</span></span> |<span data-ttu-id="5f639-216">Nej (standard: 10000)</span><span class="sxs-lookup"><span data-stu-id="5f639-216">No (default: 10000)</span></span> |
| <span data-ttu-id="5f639-217">sqlWriterCleanupScript</span><span class="sxs-lookup"><span data-stu-id="5f639-217">sqlWriterCleanupScript</span></span> |<span data-ttu-id="5f639-218">Ange en fråga för aktiviteten kopiera tooexecute så att data för ett visst segment har rensats bort.</span><span class="sxs-lookup"><span data-stu-id="5f639-218">Specify a query for Copy Activity tooexecute such that data of a specific slice is cleaned up.</span></span> <span data-ttu-id="5f639-219">Mer information finns i [repeterbara kopiera](#repeatable-copy).</span><span class="sxs-lookup"><span data-stu-id="5f639-219">For more information, see [repeatable copy](#repeatable-copy).</span></span> |<span data-ttu-id="5f639-220">En frågesats.</span><span class="sxs-lookup"><span data-stu-id="5f639-220">A query statement.</span></span> |<span data-ttu-id="5f639-221">Nej</span><span class="sxs-lookup"><span data-stu-id="5f639-221">No</span></span> |
| <span data-ttu-id="5f639-222">sliceIdentifierColumnName</span><span class="sxs-lookup"><span data-stu-id="5f639-222">sliceIdentifierColumnName</span></span> |<span data-ttu-id="5f639-223">Ange ett kolumnnamn för Kopieringsaktiviteten toofill med genereras automatiskt segment-ID som har använt tooclean in data för ett visst segment när köras på nytt.</span><span class="sxs-lookup"><span data-stu-id="5f639-223">Specify a column name for Copy Activity toofill with auto generated slice identifier, which is used tooclean up data of a specific slice when rerun.</span></span> <span data-ttu-id="5f639-224">Mer information finns i [repeterbara kopiera](#repeatable-copy).</span><span class="sxs-lookup"><span data-stu-id="5f639-224">For more information, see [repeatable copy](#repeatable-copy).</span></span> |<span data-ttu-id="5f639-225">Kolumnnamnet för en kolumn med datatypen för binary(32).</span><span class="sxs-lookup"><span data-stu-id="5f639-225">Column name of a column with data type of binary(32).</span></span> |<span data-ttu-id="5f639-226">Nej</span><span class="sxs-lookup"><span data-stu-id="5f639-226">No</span></span> |
| <span data-ttu-id="5f639-227">sqlWriterStoredProcedureName</span><span class="sxs-lookup"><span data-stu-id="5f639-227">sqlWriterStoredProcedureName</span></span> |<span data-ttu-id="5f639-228">Namnet på hello lagrad procedur upserts (uppdateringar/infogar) data i hello måltabellen.</span><span class="sxs-lookup"><span data-stu-id="5f639-228">Name of hello stored procedure that upserts (updates/inserts) data into hello target table.</span></span> |<span data-ttu-id="5f639-229">Namnet på hello lagrad procedur.</span><span class="sxs-lookup"><span data-stu-id="5f639-229">Name of hello stored procedure.</span></span> |<span data-ttu-id="5f639-230">Nej</span><span class="sxs-lookup"><span data-stu-id="5f639-230">No</span></span> |
| <span data-ttu-id="5f639-231">storedProcedureParameters</span><span class="sxs-lookup"><span data-stu-id="5f639-231">storedProcedureParameters</span></span> |<span data-ttu-id="5f639-232">Parametrar för hello lagrad procedur.</span><span class="sxs-lookup"><span data-stu-id="5f639-232">Parameters for hello stored procedure.</span></span> |<span data-ttu-id="5f639-233">Namn/värde-par.</span><span class="sxs-lookup"><span data-stu-id="5f639-233">Name/value pairs.</span></span> <span data-ttu-id="5f639-234">Namn och skiftläge parametrar måste matcha hello namn och versaler och gemener i hello lagrade procedurparametrar.</span><span class="sxs-lookup"><span data-stu-id="5f639-234">Names and casing of parameters must match hello names and casing of hello stored procedure parameters.</span></span> |<span data-ttu-id="5f639-235">Nej</span><span class="sxs-lookup"><span data-stu-id="5f639-235">No</span></span> |
| <span data-ttu-id="5f639-236">sqlWriterTableType</span><span class="sxs-lookup"><span data-stu-id="5f639-236">sqlWriterTableType</span></span> |<span data-ttu-id="5f639-237">Ange tabellen typen namnet toobe används i hello lagrade proceduren.</span><span class="sxs-lookup"><span data-stu-id="5f639-237">Specify a table type name toobe used in hello stored procedure.</span></span> <span data-ttu-id="5f639-238">Kopieringsaktiviteten tillgängliggör hello data flyttas i en temporär tabell med den här tabellen.</span><span class="sxs-lookup"><span data-stu-id="5f639-238">Copy activity makes hello data being moved available in a temp table with this table type.</span></span> <span data-ttu-id="5f639-239">Lagrade procedurer kan sedan koppla hello data kopieras med befintliga data.</span><span class="sxs-lookup"><span data-stu-id="5f639-239">Stored procedure code can then merge hello data being copied with existing data.</span></span> |<span data-ttu-id="5f639-240">Ett namn för tabellen.</span><span class="sxs-lookup"><span data-stu-id="5f639-240">A table type name.</span></span> |<span data-ttu-id="5f639-241">Nej</span><span class="sxs-lookup"><span data-stu-id="5f639-241">No</span></span> |

#### <a name="sqlsink-example"></a><span data-ttu-id="5f639-242">SqlSink-exempel</span><span class="sxs-lookup"><span data-stu-id="5f639-242">SqlSink example</span></span>

```JSON
"sink": {
    "type": "SqlSink",
    "writeBatchSize": 1000000,
    "writeBatchTimeout": "00:05:00",
    "sqlWriterStoredProcedureName": "CopyTestStoredProcedureWithParameters",
    "sqlWriterTableType": "CopyTestTableType",
    "storedProcedureParameters": {
        "identifier": { "value": "1", "type": "Int" },
        "stringData": { "value": "str1" },
        "decimalData": { "value": "1", "type": "Decimal" }
    }
}
```

## <a name="json-examples-for-copying-data-tooand-from-sql-database"></a><span data-ttu-id="5f639-243">JSON-exempel för att kopiera data tooand från SQL-databas</span><span class="sxs-lookup"><span data-stu-id="5f639-243">JSON examples for copying data tooand from SQL Database</span></span>
<span data-ttu-id="5f639-244">hello följande exempel ger exempel JSON definitioner som du kan använda toocreate en pipeline med hjälp av [Azure-portalen](data-factory-copy-activity-tutorial-using-azure-portal.md) eller [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md) eller [Azure PowerShell](data-factory-copy-activity-tutorial-using-powershell.md).</span><span class="sxs-lookup"><span data-stu-id="5f639-244">hello following examples provide sample JSON definitions that you can use toocreate a pipeline by using [Azure portal](data-factory-copy-activity-tutorial-using-azure-portal.md) or [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md) or [Azure PowerShell](data-factory-copy-activity-tutorial-using-powershell.md).</span></span> <span data-ttu-id="5f639-245">De visar hur toocopy data tooand från Azure SQL Database och Azure Blob Storage.</span><span class="sxs-lookup"><span data-stu-id="5f639-245">They show how toocopy data tooand from Azure SQL Database and Azure Blob Storage.</span></span> <span data-ttu-id="5f639-246">Dock datan kan kopieras **direkt** från alla källor tooany av hello sänkor anges [här](data-factory-data-movement-activities.md#supported-data-stores-and-formats) med hello Kopieringsaktiviteten i Azure Data Factory.</span><span class="sxs-lookup"><span data-stu-id="5f639-246">However, data can be copied **directly** from any of sources tooany of hello sinks stated [here](data-factory-data-movement-activities.md#supported-data-stores-and-formats) using hello Copy Activity in Azure Data Factory.</span></span>

### <a name="example-copy-data-from-azure-sql-database-tooazure-blob"></a><span data-ttu-id="5f639-247">Exempel: Kopiera data från Azure SQL Database tooAzure Blob</span><span class="sxs-lookup"><span data-stu-id="5f639-247">Example: Copy data from Azure SQL Database tooAzure Blob</span></span>
<span data-ttu-id="5f639-248">hello definierar samma hello följande Data Factory-enheter:</span><span class="sxs-lookup"><span data-stu-id="5f639-248">hello same defines hello following Data Factory entities:</span></span>

1. <span data-ttu-id="5f639-249">En länkad tjänst av typen [AzureSqlDatabase](#linked-service-properties).</span><span class="sxs-lookup"><span data-stu-id="5f639-249">A linked service of type [AzureSqlDatabase](#linked-service-properties).</span></span>
2. <span data-ttu-id="5f639-250">En länkad tjänst av typen [AzureStorage](data-factory-azure-blob-connector.md#linked-service-properties).</span><span class="sxs-lookup"><span data-stu-id="5f639-250">A linked service of type [AzureStorage](data-factory-azure-blob-connector.md#linked-service-properties).</span></span>
3. <span data-ttu-id="5f639-251">Indata [dataset](data-factory-create-datasets.md) av typen [AzureSqlTable](#dataset-properties).</span><span class="sxs-lookup"><span data-stu-id="5f639-251">An input [dataset](data-factory-create-datasets.md) of type [AzureSqlTable](#dataset-properties).</span></span>
4. <span data-ttu-id="5f639-252">Utdata [dataset](data-factory-create-datasets.md) av typen [Azure Blob](data-factory-azure-blob-connector.md#dataset-properties).</span><span class="sxs-lookup"><span data-stu-id="5f639-252">An output [dataset](data-factory-create-datasets.md) of type [Azure Blob](data-factory-azure-blob-connector.md#dataset-properties).</span></span>
5. <span data-ttu-id="5f639-253">En [pipeline](data-factory-create-pipelines.md) med en kopia-aktivitet som använder [SqlSource](#copy-activity-properties) och [BlobSink](data-factory-azure-blob-connector.md#copy-activity-properties).</span><span class="sxs-lookup"><span data-stu-id="5f639-253">A [pipeline](data-factory-create-pipelines.md) with a Copy activity that uses [SqlSource](#copy-activity-properties) and [BlobSink](data-factory-azure-blob-connector.md#copy-activity-properties).</span></span>

<span data-ttu-id="5f639-254">hello exemplet kopierar time series-data (varje timme, varje dag, etc.) från en tabell i Azure SQL database tooa blob varje timme.</span><span class="sxs-lookup"><span data-stu-id="5f639-254">hello sample copies time-series data (hourly, daily, etc.) from a table in Azure SQL database tooa blob every hour.</span></span> <span data-ttu-id="5f639-255">hello JSON egenskaper som används i exemplen beskrivs i hello-exempel i följande avsnitt.</span><span class="sxs-lookup"><span data-stu-id="5f639-255">hello JSON properties used in these samples are described in sections following hello samples.</span></span>  

<span data-ttu-id="5f639-256">**Azure SQL Database länkade tjänsten:**</span><span class="sxs-lookup"><span data-stu-id="5f639-256">**Azure SQL Database linked service:**</span></span>

```JSON
{
  "name": "AzureSqlLinkedService",
  "properties": {
    "type": "AzureSqlDatabase",
    "typeProperties": {
      "connectionString": "Server=tcp:<servername>.database.windows.net,1433;Database=<databasename>;User ID=<username>@<servername>;Password=<password>;Trusted_Connection=False;Encrypt=True;Connection Timeout=30"
    }
  }
}
```
<span data-ttu-id="5f639-257">Se hello [länkad Azure SQL-tjänst](#linked-service) avsnittet hello lista över egenskaper som stöds av den här länkade tjänsten.</span><span class="sxs-lookup"><span data-stu-id="5f639-257">See hello [Azure SQL Linked Service](#linked-service) section for hello list of properties supported by this linked service.</span></span>

<span data-ttu-id="5f639-258">**Azure Blob storage länkade tjänsten:**</span><span class="sxs-lookup"><span data-stu-id="5f639-258">**Azure Blob storage linked service:**</span></span>

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
<span data-ttu-id="5f639-259">Se hello [Azure Blob](data-factory-azure-blob-connector.md#azure-storage-linked-service) artikel hello lista över egenskaper som stöds av den här länkade tjänsten.</span><span class="sxs-lookup"><span data-stu-id="5f639-259">See hello [Azure Blob](data-factory-azure-blob-connector.md#azure-storage-linked-service) article for hello list of properties supported by this linked service.</span></span>


<span data-ttu-id="5f639-260">**Azure SQL inkommande datamängd:**</span><span class="sxs-lookup"><span data-stu-id="5f639-260">**Azure SQL input dataset:**</span></span>

<span data-ttu-id="5f639-261">hello exemplet förutsätter att du har skapat en tabell ”mytable” som prefix i Azure SQL och innehåller en kolumn med namnet ”timestampcolumn” för tid series-data.</span><span class="sxs-lookup"><span data-stu-id="5f639-261">hello sample assumes you have created a table “MyTable” in Azure SQL and it contains a column called “timestampcolumn” for time series data.</span></span>

<span data-ttu-id="5f639-262">Inställningen ”externa”: ”true” informerar hello Azure Data Factory-tjänsten som hello dataset är externa toohello data factory och inte tillverkas av en aktivitet i hello data factory.</span><span class="sxs-lookup"><span data-stu-id="5f639-262">Setting “external”: ”true” informs hello Azure Data Factory service that hello dataset is external toohello data factory and is not produced by an activity in hello data factory.</span></span>

```JSON
{
  "name": "AzureSqlInput",
  "properties": {
    "type": "AzureSqlTable",
    "linkedServiceName": "AzureSqlLinkedService",
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

<span data-ttu-id="5f639-263">Se hello [Azure SQL dataset Typegenskaper](#dataset) avsnittet hello lista över egenskaper som stöds av den här dataset-typen.</span><span class="sxs-lookup"><span data-stu-id="5f639-263">See hello [Azure SQL dataset type properties](#dataset) section for hello list of properties supported by this dataset type.</span></span>  

<span data-ttu-id="5f639-264">**Azure Blob utdatauppsättningen:**</span><span class="sxs-lookup"><span data-stu-id="5f639-264">**Azure Blob output dataset:**</span></span>

<span data-ttu-id="5f639-265">Data skrivs tooa nya blob varje timme (frekvens: timme, intervall: 1).</span><span class="sxs-lookup"><span data-stu-id="5f639-265">Data is written tooa new blob every hour (frequency: hour, interval: 1).</span></span> <span data-ttu-id="5f639-266">hello mappsökväg för hello blob utvärderas dynamiskt baserat på hello starttiden för hello-segment som bearbetas.</span><span class="sxs-lookup"><span data-stu-id="5f639-266">hello folder path for hello blob is dynamically evaluated based on hello start time of hello slice that is being processed.</span></span> <span data-ttu-id="5f639-267">hello mappsökväg använder år, månad, dag och timmar delar av hello starttid.</span><span class="sxs-lookup"><span data-stu-id="5f639-267">hello folder path uses year, month, day, and hours parts of hello start time.</span></span>

```JSON
{
  "name": "AzureBlobOutput",
  "properties": {
    "type": "AzureBlob",
    "linkedServiceName": "StorageLinkedService",
    "typeProperties": {
      "folderPath": "mycontainer/myfolder/yearno={Year}/monthno={Month}/dayno={Day}/hourno={Hour}/",
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
<span data-ttu-id="5f639-268">Se hello [Azure Blob dataset Typegenskaper](data-factory-azure-blob-connector.md#dataset-properties) avsnittet hello lista över egenskaper som stöds av den här dataset-typen.</span><span class="sxs-lookup"><span data-stu-id="5f639-268">See hello [Azure Blob dataset type properties](data-factory-azure-blob-connector.md#dataset-properties) section for hello list of properties supported by this dataset type.</span></span>  

<span data-ttu-id="5f639-269">**Kopieringsaktiviteten i en pipeline med SQL-källa och mottagare för Blob:**</span><span class="sxs-lookup"><span data-stu-id="5f639-269">**A copy activity in a pipeline with SQL source and Blob sink:**</span></span>

<span data-ttu-id="5f639-270">hello pipelinen innehåller en kopia-aktivitet som är konfigurerade toouse hello inkommande och utgående datauppsättningar och är schemalagda toorun varje timme.</span><span class="sxs-lookup"><span data-stu-id="5f639-270">hello pipeline contains a Copy Activity that is configured toouse hello input and output datasets and is scheduled toorun every hour.</span></span> <span data-ttu-id="5f639-271">I hello pipeline JSON-definitionen hello **källa** typ har angetts för**SqlSource** och **sink** typ har angetts för**BlobSink**.</span><span class="sxs-lookup"><span data-stu-id="5f639-271">In hello pipeline JSON definition, hello **source** type is set too**SqlSource** and **sink** type is set too**BlobSink**.</span></span> <span data-ttu-id="5f639-272">hello SQL-frågan som angetts för hello **SqlReaderQuery** egenskapen väljer hello data i hello tidigare timme toocopy.</span><span class="sxs-lookup"><span data-stu-id="5f639-272">hello SQL query specified for hello **SqlReaderQuery** property selects hello data in hello past hour toocopy.</span></span>

```JSON
{  
    "name":"SamplePipeline",
    "properties":{  
    "start":"2014-06-01T18:00:00",
    "end":"2014-06-01T19:00:00",
    "description":"pipeline for copy activity",
    "activities":[  
      {
        "name": "AzureSQLtoBlob",
        "description": "copy activity",
        "type": "Copy",
        "inputs": [
          {
            "name": "AzureSQLInput"
          }
        ],
        "outputs": [
          {
            "name": "AzureBlobOutput"
          }
        ],
        "typeProperties": {
          "source": {
            "type": "SqlSource",
            "SqlReaderQuery": "$$Text.Format('select * from MyTable where timestampcolumn >= \\'{0:yyyy-MM-dd HH:mm}\\' AND timestampcolumn < \\'{1:yyyy-MM-dd HH:mm}\\'', WindowStart, WindowEnd)"
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
<span data-ttu-id="5f639-273">I exemplet hello **sqlReaderQuery** har angetts för hello SqlSource.</span><span class="sxs-lookup"><span data-stu-id="5f639-273">In hello example, **sqlReaderQuery** is specified for hello SqlSource.</span></span> <span data-ttu-id="5f639-274">Hej Kopieringsaktiviteten kör den här frågan mot hello Azure SQL Database källdata tooget hello.</span><span class="sxs-lookup"><span data-stu-id="5f639-274">hello Copy Activity runs this query against hello Azure SQL Database source tooget hello data.</span></span> <span data-ttu-id="5f639-275">Du kan också ange en lagrad procedur genom att ange hello **sqlReaderStoredProcedureName** och **storedProcedureParameters** (om hello tar lagrade proceduren parametrar).</span><span class="sxs-lookup"><span data-stu-id="5f639-275">Alternatively, you can specify a stored procedure by specifying hello **sqlReaderStoredProcedureName** and **storedProcedureParameters** (if hello stored procedure takes parameters).</span></span>

<span data-ttu-id="5f639-276">Om du inte anger sqlReaderQuery eller sqlReaderStoredProcedureName är hello kolumner som anges i hello strukturen i hello dataset JSON används toobuild en fråga toorun mot hello Azure SQL Database.</span><span class="sxs-lookup"><span data-stu-id="5f639-276">If you do not specify either sqlReaderQuery or sqlReaderStoredProcedureName, hello columns defined in hello structure section of hello dataset JSON are used toobuild a query toorun against hello Azure SQL Database.</span></span> <span data-ttu-id="5f639-277">Till exempel: `select column1, column2 from mytable`.</span><span class="sxs-lookup"><span data-stu-id="5f639-277">For example: `select column1, column2 from mytable`.</span></span> <span data-ttu-id="5f639-278">Om hello datauppsättningsdefinitionen inte har hello struktur, markeras alla kolumner från tabellen hello.</span><span class="sxs-lookup"><span data-stu-id="5f639-278">If hello dataset definition does not have hello structure, all columns are selected from hello table.</span></span>

<span data-ttu-id="5f639-279">Se hello [Sql-källans](#sqlsource) avsnitt och [BlobSink](data-factory-azure-blob-connector.md#copy-activity-properties) hello lista över egenskaper som stöds av SqlSource och BlobSink.</span><span class="sxs-lookup"><span data-stu-id="5f639-279">See hello [Sql Source](#sqlsource) section and [BlobSink](data-factory-azure-blob-connector.md#copy-activity-properties) for hello list of properties supported by SqlSource and BlobSink.</span></span>

### <a name="example-copy-data-from-azure-blob-tooazure-sql-database"></a><span data-ttu-id="5f639-280">Exempel: Kopiera data från Azure Blob tooAzure SQL-databas</span><span class="sxs-lookup"><span data-stu-id="5f639-280">Example: Copy data from Azure Blob tooAzure SQL Database</span></span>
<span data-ttu-id="5f639-281">hello exemplet definierar hello följande Data Factory-enheter:</span><span class="sxs-lookup"><span data-stu-id="5f639-281">hello sample defines hello following Data Factory entities:</span></span>  

1. <span data-ttu-id="5f639-282">En länkad tjänst av typen [AzureSqlDatabase](#linked-service-properties).</span><span class="sxs-lookup"><span data-stu-id="5f639-282">A linked service of type [AzureSqlDatabase](#linked-service-properties).</span></span>
2. <span data-ttu-id="5f639-283">En länkad tjänst av typen [AzureStorage](data-factory-azure-blob-connector.md#linked-service-properties).</span><span class="sxs-lookup"><span data-stu-id="5f639-283">A linked service of type [AzureStorage](data-factory-azure-blob-connector.md#linked-service-properties).</span></span>
3. <span data-ttu-id="5f639-284">Indata [dataset](data-factory-create-datasets.md) av typen [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties).</span><span class="sxs-lookup"><span data-stu-id="5f639-284">An input [dataset](data-factory-create-datasets.md) of type [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties).</span></span>
4. <span data-ttu-id="5f639-285">Utdata [dataset](data-factory-create-datasets.md) av typen [AzureSqlTable](#dataset-properties).</span><span class="sxs-lookup"><span data-stu-id="5f639-285">An output [dataset](data-factory-create-datasets.md) of type [AzureSqlTable](#dataset-properties).</span></span>
5. <span data-ttu-id="5f639-286">En [pipeline](data-factory-create-pipelines.md) med kopieringsaktiviteten som använder [BlobSource](data-factory-azure-blob-connector.md#copy-activity-properties) och [SqlSink](#copy-activity-properties).</span><span class="sxs-lookup"><span data-stu-id="5f639-286">A [pipeline](data-factory-create-pipelines.md) with Copy activity that uses [BlobSource](data-factory-azure-blob-connector.md#copy-activity-properties) and [SqlSink](#copy-activity-properties).</span></span>

<span data-ttu-id="5f639-287">hello exemplet kopierar time series-data (varje timme, varje dag, etc.) från Azure blob tooa tabell i Azure SQL database varje timme.</span><span class="sxs-lookup"><span data-stu-id="5f639-287">hello sample copies time-series data (hourly, daily, etc.) from Azure blob tooa table in Azure SQL database every hour.</span></span> <span data-ttu-id="5f639-288">hello JSON egenskaper som används i exemplen beskrivs i hello-exempel i följande avsnitt.</span><span class="sxs-lookup"><span data-stu-id="5f639-288">hello JSON properties used in these samples are described in sections following hello samples.</span></span>

<span data-ttu-id="5f639-289">**Azure SQL länkade tjänsten:**</span><span class="sxs-lookup"><span data-stu-id="5f639-289">**Azure SQL linked service:**</span></span>

```JSON
{
  "name": "AzureSqlLinkedService",
  "properties": {
    "type": "AzureSqlDatabase",
    "typeProperties": {
      "connectionString": "Server=tcp:<servername>.database.windows.net,1433;Database=<databasename>;User ID=<username>@<servername>;Password=<password>;Trusted_Connection=False;Encrypt=True;Connection Timeout=30"
    }
  }
}
```
<span data-ttu-id="5f639-290">Se hello [länkad Azure SQL-tjänst](#linked-service) avsnittet hello lista över egenskaper som stöds av den här länkade tjänsten.</span><span class="sxs-lookup"><span data-stu-id="5f639-290">See hello [Azure SQL Linked Service](#linked-service) section for hello list of properties supported by this linked service.</span></span>

<span data-ttu-id="5f639-291">**Azure Blob storage länkade tjänsten:**</span><span class="sxs-lookup"><span data-stu-id="5f639-291">**Azure Blob storage linked service:**</span></span>

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
<span data-ttu-id="5f639-292">Se hello [Azure Blob](data-factory-azure-blob-connector.md#azure-storage-linked-service) artikel hello lista över egenskaper som stöds av den här länkade tjänsten.</span><span class="sxs-lookup"><span data-stu-id="5f639-292">See hello [Azure Blob](data-factory-azure-blob-connector.md#azure-storage-linked-service) article for hello list of properties supported by this linked service.</span></span>


<span data-ttu-id="5f639-293">**Azure Blob inkommande datamängd:**</span><span class="sxs-lookup"><span data-stu-id="5f639-293">**Azure Blob input dataset:**</span></span>

<span data-ttu-id="5f639-294">Data hämtas från en ny blob varje timme (frekvens: timme, intervall: 1).</span><span class="sxs-lookup"><span data-stu-id="5f639-294">Data is picked up from a new blob every hour (frequency: hour, interval: 1).</span></span> <span data-ttu-id="5f639-295">hello mappen sökvägen och filnamnet för hello blob utvärderas dynamiskt baserat på hello starttiden för hello-segment som bearbetas.</span><span class="sxs-lookup"><span data-stu-id="5f639-295">hello folder path and file name for hello blob are dynamically evaluated based on hello start time of hello slice that is being processed.</span></span> <span data-ttu-id="5f639-296">hello mappsökväg använder år, månad och som en dagdel av hello starttid och filnamn använder hello timme tillhör hello starttid.</span><span class="sxs-lookup"><span data-stu-id="5f639-296">hello folder path uses year, month, and day part of hello start time and file name uses hello hour part of hello start time.</span></span> <span data-ttu-id="5f639-297">”externa”: ”true” inställningen informerar hello Data Factory-tjänsten att den här tabellen är externa toohello data factory och inte tillverkas av en aktivitet i hello data factory.</span><span class="sxs-lookup"><span data-stu-id="5f639-297">“external”: “true” setting informs hello Data Factory service that this table is external toohello data factory and is not produced by an activity in hello data factory.</span></span>

```JSON
{
  "name": "AzureBlobInput",
  "properties": {
    "type": "AzureBlob",
    "linkedServiceName": "StorageLinkedService",
    "typeProperties": {
      "folderPath": "mycontainer/myfolder/yearno={Year}/monthno={Month}/dayno={Day}/",
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
<span data-ttu-id="5f639-298">Se hello [Azure Blob dataset Typegenskaper](data-factory-azure-blob-connector.md#dataset-properties) avsnittet hello lista över egenskaper som stöds av den här dataset-typen.</span><span class="sxs-lookup"><span data-stu-id="5f639-298">See hello [Azure Blob dataset type properties](data-factory-azure-blob-connector.md#dataset-properties) section for hello list of properties supported by this dataset type.</span></span>

<span data-ttu-id="5f639-299">**Azure SQL Database utdatauppsättningen:**</span><span class="sxs-lookup"><span data-stu-id="5f639-299">**Azure SQL Database output dataset:**</span></span>

<span data-ttu-id="5f639-300">hello exemplet kopierar data tooa tabell med namnet ”mytable” som prefix i SQL Azure.</span><span class="sxs-lookup"><span data-stu-id="5f639-300">hello sample copies data tooa table named “MyTable” in Azure SQL.</span></span> <span data-ttu-id="5f639-301">Skapa hello tabell i Azure SQL med hello samma antal kolumner som du förväntar dig hello Blob CSV-filen toocontain.</span><span class="sxs-lookup"><span data-stu-id="5f639-301">Create hello table in Azure SQL with hello same number of columns as you expect hello Blob CSV file toocontain.</span></span> <span data-ttu-id="5f639-302">Nya rader läggs toohello tabell varje timme.</span><span class="sxs-lookup"><span data-stu-id="5f639-302">New rows are added toohello table every hour.</span></span>

```JSON
{
  "name": "AzureSqlOutput",
  "properties": {
    "type": "AzureSqlTable",
    "linkedServiceName": "AzureSqlLinkedService",
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
<span data-ttu-id="5f639-303">Se hello [Azure SQL dataset Typegenskaper](#dataset) avsnittet hello lista över egenskaper som stöds av den här dataset-typen.</span><span class="sxs-lookup"><span data-stu-id="5f639-303">See hello [Azure SQL dataset type properties](#dataset) section for hello list of properties supported by this dataset type.</span></span>

<span data-ttu-id="5f639-304">**Kopieringsaktiviteten i en pipeline med Blob källa och mottagare för SQL:**</span><span class="sxs-lookup"><span data-stu-id="5f639-304">**A copy activity in a pipeline with Blob source and SQL sink:**</span></span>

<span data-ttu-id="5f639-305">hello pipelinen innehåller en kopia-aktivitet som är konfigurerade toouse hello inkommande och utgående datauppsättningar och är schemalagda toorun varje timme.</span><span class="sxs-lookup"><span data-stu-id="5f639-305">hello pipeline contains a Copy Activity that is configured toouse hello input and output datasets and is scheduled toorun every hour.</span></span> <span data-ttu-id="5f639-306">I hello pipeline JSON-definitionen hello **källa** typ har angetts för**BlobSource** och **sink** typ har angetts för**SqlSink**.</span><span class="sxs-lookup"><span data-stu-id="5f639-306">In hello pipeline JSON definition, hello **source** type is set too**BlobSource** and **sink** type is set too**SqlSink**.</span></span>

```JSON
{  
    "name":"SamplePipeline",
    "properties":{  
    "start":"2014-06-01T18:00:00",
    "end":"2014-06-01T19:00:00",
    "description":"pipeline with copy activity",
    "activities":[  
      {
        "name": "AzureBlobtoSQL",
        "description": "Copy Activity",
        "type": "Copy",
        "inputs": [
          {
            "name": "AzureBlobInput"
          }
        ],
        "outputs": [
          {
            "name": "AzureSqlOutput"
          }
        ],
        "typeProperties": {
          "source": {
            "type": "BlobSource",
            "blobColumnSeparators": ","
          },
          "sink": {
            "type": "SqlSink"
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
<span data-ttu-id="5f639-307">Se hello [Sql Sink](#sqlsink) avsnitt och [BlobSource](data-factory-azure-blob-connector.md#copy-activity-properties) hello lista över egenskaper som stöds av SqlSink och BlobSource.</span><span class="sxs-lookup"><span data-stu-id="5f639-307">See hello [Sql Sink](#sqlsink) section and [BlobSource](data-factory-azure-blob-connector.md#copy-activity-properties) for hello list of properties supported by SqlSink and BlobSource.</span></span>

## <a name="identity-columns-in-hello-target-database"></a><span data-ttu-id="5f639-308">Identitetskolumner i hello måldatabasen</span><span class="sxs-lookup"><span data-stu-id="5f639-308">Identity columns in hello target database</span></span>
<span data-ttu-id="5f639-309">Det här avsnittet innehåller ett exempel för att kopiera data från en källtabell utan en identitet kolumnen tooa måltabellen med en identitetskolumn.</span><span class="sxs-lookup"><span data-stu-id="5f639-309">This section provides an example for copying data from a source table without an identity column tooa destination table with an identity column.</span></span>

<span data-ttu-id="5f639-310">**Källtabellen:**</span><span class="sxs-lookup"><span data-stu-id="5f639-310">**Source table:**</span></span>

```SQL
create table dbo.SourceTbl
(
       name varchar(100),
       age int
)
```
<span data-ttu-id="5f639-311">**Måltabellen:**</span><span class="sxs-lookup"><span data-stu-id="5f639-311">**Destination table:**</span></span>

```SQL
create table dbo.TargetTbl
(
       identifier int identity(1,1),
       name varchar(100),
       age int
)
```
<span data-ttu-id="5f639-312">Observera att hello måltabellen har en identitetskolumn.</span><span class="sxs-lookup"><span data-stu-id="5f639-312">Notice that hello target table has an identity column.</span></span>

<span data-ttu-id="5f639-313">**Källa JSON-definitionen för datauppsättning**</span><span class="sxs-lookup"><span data-stu-id="5f639-313">**Source dataset JSON definition**</span></span>

```JSON
{
    "name": "SampleSource",
    "properties": {
        "type": " SqlServerTable",
        "linkedServiceName": "TestIdentitySQL",
        "typeProperties": {
            "tableName": "SourceTbl"
        },
        "availability": {
            "frequency": "Hour",
            "interval": 1
        },
        "external": true,
        "policy": {}
    }
}
```
<span data-ttu-id="5f639-314">**Mål JSON-definitionen för datauppsättning**</span><span class="sxs-lookup"><span data-stu-id="5f639-314">**Destination dataset JSON definition**</span></span>

```JSON
{
    "name": "SampleTarget",
    "properties": {
        "structure": [
            { "name": "name" },
            { "name": "age" }
        ],
        "type": "AzureSqlTable",
        "linkedServiceName": "TestIdentitySQLSource",
        "typeProperties": {
            "tableName": "TargetTbl"
        },
        "availability": {
            "frequency": "Hour",
            "interval": 1
        },
        "external": false,
        "policy": {}
    }    
}
```

<span data-ttu-id="5f639-315">Observera att som käll- och tabellen har olika schema (målet har en ytterligare kolumn med identity).</span><span class="sxs-lookup"><span data-stu-id="5f639-315">Notice that as your source and target table have different schema (target has an additional column with identity).</span></span> <span data-ttu-id="5f639-316">I det här scenariot behöver du toospecify **struktur** egenskap i hello mål datauppsättningsdefinitionen, som inte innehåller hello identity-kolumn.</span><span class="sxs-lookup"><span data-stu-id="5f639-316">In this scenario, you need toospecify **structure** property in hello target dataset definition, which doesn’t include hello identity column.</span></span>

## <a name="invoke-stored-procedure-from-sql-sink"></a><span data-ttu-id="5f639-317">Anropa lagrade procedur från SQL-mottagare</span><span class="sxs-lookup"><span data-stu-id="5f639-317">Invoke stored procedure from SQL sink</span></span>
<span data-ttu-id="5f639-318">Ett exempel för att anropa en lagrad procedur från SQL-mottagare i en Kopieringsaktivitet för en pipeline finns [anropa lagrad procedur för SQL-mottagare i en Kopieringsaktivitet](data-factory-invoke-stored-procedure-from-copy-activity.md) artikel.</span><span class="sxs-lookup"><span data-stu-id="5f639-318">For an example of invoking a stored procedure from SQL sink in a copy activity of a pipeline, see [Invoke stored procedure for SQL sink in copy activity](data-factory-invoke-stored-procedure-from-copy-activity.md) article.</span></span> 

## <a name="type-mapping-for-azure-sql-database"></a><span data-ttu-id="5f639-319">Mappning för Azure SQL Database</span><span class="sxs-lookup"><span data-stu-id="5f639-319">Type mapping for Azure SQL Database</span></span>
<span data-ttu-id="5f639-320">Som anges i hello [data movement aktiviteter](data-factory-data-movement-activities.md) artikel kopieringsaktiviteten utför automatisk konverteringar från källan typer toosink typer med hello följande metod i steg 2:</span><span class="sxs-lookup"><span data-stu-id="5f639-320">As mentioned in hello [data movement activities](data-factory-data-movement-activities.md) article Copy activity performs automatic type conversions from source types toosink types with hello following 2-step approach:</span></span>

1. <span data-ttu-id="5f639-321">Konvertera från inbyggda typer too.NET källtypen</span><span class="sxs-lookup"><span data-stu-id="5f639-321">Convert from native source types too.NET type</span></span>
2. <span data-ttu-id="5f639-322">Konvertera från .NET typen toonative Mottagartypen</span><span class="sxs-lookup"><span data-stu-id="5f639-322">Convert from .NET type toonative sink type</span></span>

<span data-ttu-id="5f639-323">När du flyttar data tooand från Azure SQL Database, används hello följande mappningar från SQL too.NET typ och vice versa.</span><span class="sxs-lookup"><span data-stu-id="5f639-323">When moving data tooand from Azure SQL Database, hello following mappings are used from SQL type too.NET type and vice versa.</span></span> <span data-ttu-id="5f639-324">hello-mappning är samma som hello Datatypsmappningen i SQL Server för ADO.NET.</span><span class="sxs-lookup"><span data-stu-id="5f639-324">hello mapping is same as hello SQL Server Data Type Mapping for ADO.NET.</span></span>

| <span data-ttu-id="5f639-325">SQL Server Database Engine-typ</span><span class="sxs-lookup"><span data-stu-id="5f639-325">SQL Server Database Engine type</span></span> | <span data-ttu-id="5f639-326">.NET framework-typ</span><span class="sxs-lookup"><span data-stu-id="5f639-326">.NET Framework type</span></span> |
| --- | --- |
| <span data-ttu-id="5f639-327">bigint</span><span class="sxs-lookup"><span data-stu-id="5f639-327">bigint</span></span> |<span data-ttu-id="5f639-328">Int64</span><span class="sxs-lookup"><span data-stu-id="5f639-328">Int64</span></span> |
| <span data-ttu-id="5f639-329">Binär</span><span class="sxs-lookup"><span data-stu-id="5f639-329">binary</span></span> |<span data-ttu-id="5f639-330">byte]</span><span class="sxs-lookup"><span data-stu-id="5f639-330">Byte[]</span></span> |
| <span data-ttu-id="5f639-331">bitar</span><span class="sxs-lookup"><span data-stu-id="5f639-331">bit</span></span> |<span data-ttu-id="5f639-332">Booleskt värde</span><span class="sxs-lookup"><span data-stu-id="5f639-332">Boolean</span></span> |
| <span data-ttu-id="5f639-333">Char</span><span class="sxs-lookup"><span data-stu-id="5f639-333">char</span></span> |<span data-ttu-id="5f639-334">Sträng, Char]</span><span class="sxs-lookup"><span data-stu-id="5f639-334">String, Char[]</span></span> |
| <span data-ttu-id="5f639-335">Datum</span><span class="sxs-lookup"><span data-stu-id="5f639-335">date</span></span> |<span data-ttu-id="5f639-336">Datum och tid</span><span class="sxs-lookup"><span data-stu-id="5f639-336">DateTime</span></span> |
| <span data-ttu-id="5f639-337">Datum och tid</span><span class="sxs-lookup"><span data-stu-id="5f639-337">Datetime</span></span> |<span data-ttu-id="5f639-338">Datum och tid</span><span class="sxs-lookup"><span data-stu-id="5f639-338">DateTime</span></span> |
| <span data-ttu-id="5f639-339">datetime2</span><span class="sxs-lookup"><span data-stu-id="5f639-339">datetime2</span></span> |<span data-ttu-id="5f639-340">Datum och tid</span><span class="sxs-lookup"><span data-stu-id="5f639-340">DateTime</span></span> |
| <span data-ttu-id="5f639-341">DateTimeOffset</span><span class="sxs-lookup"><span data-stu-id="5f639-341">Datetimeoffset</span></span> |<span data-ttu-id="5f639-342">DateTimeOffset</span><span class="sxs-lookup"><span data-stu-id="5f639-342">DateTimeOffset</span></span> |
| <span data-ttu-id="5f639-343">Decimal</span><span class="sxs-lookup"><span data-stu-id="5f639-343">Decimal</span></span> |<span data-ttu-id="5f639-344">Decimal</span><span class="sxs-lookup"><span data-stu-id="5f639-344">Decimal</span></span> |
| <span data-ttu-id="5f639-345">FILESTREAM-attributet (varbinary(max))</span><span class="sxs-lookup"><span data-stu-id="5f639-345">FILESTREAM attribute (varbinary(max))</span></span> |<span data-ttu-id="5f639-346">byte]</span><span class="sxs-lookup"><span data-stu-id="5f639-346">Byte[]</span></span> |
| <span data-ttu-id="5f639-347">flyttal</span><span class="sxs-lookup"><span data-stu-id="5f639-347">Float</span></span> |<span data-ttu-id="5f639-348">dubbla</span><span class="sxs-lookup"><span data-stu-id="5f639-348">Double</span></span> |
| <span data-ttu-id="5f639-349">Bild</span><span class="sxs-lookup"><span data-stu-id="5f639-349">image</span></span> |<span data-ttu-id="5f639-350">byte]</span><span class="sxs-lookup"><span data-stu-id="5f639-350">Byte[]</span></span> |
| <span data-ttu-id="5f639-351">int</span><span class="sxs-lookup"><span data-stu-id="5f639-351">int</span></span> |<span data-ttu-id="5f639-352">Int32</span><span class="sxs-lookup"><span data-stu-id="5f639-352">Int32</span></span> |
| <span data-ttu-id="5f639-353">Money</span><span class="sxs-lookup"><span data-stu-id="5f639-353">money</span></span> |<span data-ttu-id="5f639-354">Decimal</span><span class="sxs-lookup"><span data-stu-id="5f639-354">Decimal</span></span> |
| <span data-ttu-id="5f639-355">nchar</span><span class="sxs-lookup"><span data-stu-id="5f639-355">nchar</span></span> |<span data-ttu-id="5f639-356">Sträng, Char]</span><span class="sxs-lookup"><span data-stu-id="5f639-356">String, Char[]</span></span> |
| <span data-ttu-id="5f639-357">ntext</span><span class="sxs-lookup"><span data-stu-id="5f639-357">ntext</span></span> |<span data-ttu-id="5f639-358">Sträng, Char]</span><span class="sxs-lookup"><span data-stu-id="5f639-358">String, Char[]</span></span> |
| <span data-ttu-id="5f639-359">numeriskt</span><span class="sxs-lookup"><span data-stu-id="5f639-359">numeric</span></span> |<span data-ttu-id="5f639-360">Decimal</span><span class="sxs-lookup"><span data-stu-id="5f639-360">Decimal</span></span> |
| <span data-ttu-id="5f639-361">nvarchar</span><span class="sxs-lookup"><span data-stu-id="5f639-361">nvarchar</span></span> |<span data-ttu-id="5f639-362">Sträng, Char]</span><span class="sxs-lookup"><span data-stu-id="5f639-362">String, Char[]</span></span> |
| <span data-ttu-id="5f639-363">Verklig</span><span class="sxs-lookup"><span data-stu-id="5f639-363">real</span></span> |<span data-ttu-id="5f639-364">Enskild</span><span class="sxs-lookup"><span data-stu-id="5f639-364">Single</span></span> |
| <span data-ttu-id="5f639-365">ROWVERSION</span><span class="sxs-lookup"><span data-stu-id="5f639-365">rowversion</span></span> |<span data-ttu-id="5f639-366">byte]</span><span class="sxs-lookup"><span data-stu-id="5f639-366">Byte[]</span></span> |
| <span data-ttu-id="5f639-367">smalldatetime</span><span class="sxs-lookup"><span data-stu-id="5f639-367">smalldatetime</span></span> |<span data-ttu-id="5f639-368">Datum och tid</span><span class="sxs-lookup"><span data-stu-id="5f639-368">DateTime</span></span> |
| <span data-ttu-id="5f639-369">smallint</span><span class="sxs-lookup"><span data-stu-id="5f639-369">smallint</span></span> |<span data-ttu-id="5f639-370">Int16</span><span class="sxs-lookup"><span data-stu-id="5f639-370">Int16</span></span> |
| <span data-ttu-id="5f639-371">smallmoney</span><span class="sxs-lookup"><span data-stu-id="5f639-371">smallmoney</span></span> |<span data-ttu-id="5f639-372">Decimal</span><span class="sxs-lookup"><span data-stu-id="5f639-372">Decimal</span></span> |
| <span data-ttu-id="5f639-373">sql_variant</span><span class="sxs-lookup"><span data-stu-id="5f639-373">sql_variant</span></span> |<span data-ttu-id="5f639-374">Objektet *</span><span class="sxs-lookup"><span data-stu-id="5f639-374">Object *</span></span> |
| <span data-ttu-id="5f639-375">Text</span><span class="sxs-lookup"><span data-stu-id="5f639-375">text</span></span> |<span data-ttu-id="5f639-376">Sträng, Char]</span><span class="sxs-lookup"><span data-stu-id="5f639-376">String, Char[]</span></span> |
| <span data-ttu-id="5f639-377">time</span><span class="sxs-lookup"><span data-stu-id="5f639-377">time</span></span> |<span data-ttu-id="5f639-378">TimeSpan</span><span class="sxs-lookup"><span data-stu-id="5f639-378">TimeSpan</span></span> |
| <span data-ttu-id="5f639-379">tidsstämpel</span><span class="sxs-lookup"><span data-stu-id="5f639-379">timestamp</span></span> |<span data-ttu-id="5f639-380">byte]</span><span class="sxs-lookup"><span data-stu-id="5f639-380">Byte[]</span></span> |
| <span data-ttu-id="5f639-381">tinyint</span><span class="sxs-lookup"><span data-stu-id="5f639-381">tinyint</span></span> |<span data-ttu-id="5f639-382">Mottagna byte</span><span class="sxs-lookup"><span data-stu-id="5f639-382">Byte</span></span> |
| <span data-ttu-id="5f639-383">Unik identifierare</span><span class="sxs-lookup"><span data-stu-id="5f639-383">uniqueidentifier</span></span> |<span data-ttu-id="5f639-384">GUID</span><span class="sxs-lookup"><span data-stu-id="5f639-384">Guid</span></span> |
| <span data-ttu-id="5f639-385">varbinary</span><span class="sxs-lookup"><span data-stu-id="5f639-385">varbinary</span></span> |<span data-ttu-id="5f639-386">byte]</span><span class="sxs-lookup"><span data-stu-id="5f639-386">Byte[]</span></span> |
| <span data-ttu-id="5f639-387">varchar</span><span class="sxs-lookup"><span data-stu-id="5f639-387">varchar</span></span> |<span data-ttu-id="5f639-388">Sträng, Char]</span><span class="sxs-lookup"><span data-stu-id="5f639-388">String, Char[]</span></span> |
| <span data-ttu-id="5f639-389">xml</span><span class="sxs-lookup"><span data-stu-id="5f639-389">xml</span></span> |<span data-ttu-id="5f639-390">XML</span><span class="sxs-lookup"><span data-stu-id="5f639-390">Xml</span></span> |

## <a name="map-source-toosink-columns"></a><span data-ttu-id="5f639-391">Mappa källkolumner toosink</span><span class="sxs-lookup"><span data-stu-id="5f639-391">Map source toosink columns</span></span>
<span data-ttu-id="5f639-392">toolearn mappning tabellkolumner i källan dataset toocolumns i sink dataset finns [mappa dataset kolumner i Azure Data Factory](data-factory-map-columns.md).</span><span class="sxs-lookup"><span data-stu-id="5f639-392">toolearn about mapping columns in source dataset toocolumns in sink dataset, see [Mapping dataset columns in Azure Data Factory](data-factory-map-columns.md).</span></span>

## <a name="repeatable-copy"></a><span data-ttu-id="5f639-393">Repeterbara kopia</span><span class="sxs-lookup"><span data-stu-id="5f639-393">Repeatable copy</span></span>
<span data-ttu-id="5f639-394">När du kopierar data tooSQL Server-databasen läggs hello kopieringsaktiviteten datatabell toohello mottagare som standard.</span><span class="sxs-lookup"><span data-stu-id="5f639-394">When copying data tooSQL Server Database, hello copy activity appends data toohello sink table by default.</span></span> <span data-ttu-id="5f639-395">tooperform en UPSERT Läs [repeterbara skrivåtgärder tooSqlSink](data-factory-repeatable-copy.md#repeatable-write-to-sqlsink) artikel.</span><span class="sxs-lookup"><span data-stu-id="5f639-395">tooperform an UPSERT instead,  See [Repeatable write tooSqlSink](data-factory-repeatable-copy.md#repeatable-write-to-sqlsink) article.</span></span> 

<span data-ttu-id="5f639-396">När du kopierar data från relationella datalager, Kom ihåg tooavoid repeterbarhet oönskade resultat.</span><span class="sxs-lookup"><span data-stu-id="5f639-396">When copying data from relational data stores, keep repeatability in mind tooavoid unintended outcomes.</span></span> <span data-ttu-id="5f639-397">I Azure Data Factory, kan du köra en sektor manuellt.</span><span class="sxs-lookup"><span data-stu-id="5f639-397">In Azure Data Factory, you can rerun a slice manually.</span></span> <span data-ttu-id="5f639-398">Du kan också konfigurera i principen för en dataset så att ett segment som körs när ett fel uppstår.</span><span class="sxs-lookup"><span data-stu-id="5f639-398">You can also configure retry policy for a dataset so that a slice is rerun when a failure occurs.</span></span> <span data-ttu-id="5f639-399">När ett segment körs på antingen sätt måste toomake att som hello samma data läses oavsett hur många gånger ett segment körs.</span><span class="sxs-lookup"><span data-stu-id="5f639-399">When a slice is rerun in either way, you need toomake sure that hello same data is read no matter how many times a slice is run.</span></span> <span data-ttu-id="5f639-400">Se [Repeatable läsa från relationella källor](data-factory-repeatable-copy.md#repeatable-read-from-relational-sources).</span><span class="sxs-lookup"><span data-stu-id="5f639-400">See [Repeatable read from relational sources](data-factory-repeatable-copy.md#repeatable-read-from-relational-sources).</span></span>

## <a name="performance-and-tuning"></a><span data-ttu-id="5f639-401">Prestanda och finjustering</span><span class="sxs-lookup"><span data-stu-id="5f639-401">Performance and Tuning</span></span>
<span data-ttu-id="5f639-402">Se [kopiera aktivitet prestanda och justera guiden](data-factory-copy-activity-performance.md) toolearn om nyckeln faktorer som påverkan prestanda för flytt av data (Kopieringsaktiviteten) i Azure Data Factory och olika sätt toooptimize den.</span><span class="sxs-lookup"><span data-stu-id="5f639-402">See [Copy Activity Performance & Tuning Guide](data-factory-copy-activity-performance.md) toolearn about key factors that impact performance of data movement (Copy Activity) in Azure Data Factory and various ways toooptimize it.</span></span>
