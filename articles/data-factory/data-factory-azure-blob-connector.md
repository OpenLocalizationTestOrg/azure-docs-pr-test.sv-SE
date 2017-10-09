---
title: "aaaCopy data till/från Azure Blob Storage | Microsoft Docs"
description: "Lär dig hur toocopy blob-data i Azure Data Factory. Använda våra exempel: hur toocopy data tooand från Azure Blob Storage och Azure SQL Database."
keywords: BLOB-data, azure blob-kopia
services: data-factory
documentationcenter: 
author: linda33wj
manager: jhubbard
editor: monicar
ms.assetid: bec8160f-5e07-47e4-8ee1-ebb14cfb805d
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/22/2017
ms.author: jingwang
ms.openlocfilehash: 8428c64e8e8b1084b3f2f680c4e1819559e4ffa3
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="copy-data-tooor-from-azure-blob-storage-using-azure-data-factory"></a><span data-ttu-id="87761-105">Kopiera data tooor från Azure Blob Storage med hjälp av Azure Data Factory</span><span class="sxs-lookup"><span data-stu-id="87761-105">Copy data tooor from Azure Blob Storage using Azure Data Factory</span></span>
<span data-ttu-id="87761-106">Den här artikeln förklarar hur toouse hello Kopieringsaktiviteten i Azure Data Factory toocopy data tooand från Azure Blob Storage.</span><span class="sxs-lookup"><span data-stu-id="87761-106">This article explains how toouse hello Copy Activity in Azure Data Factory toocopy data tooand from Azure Blob Storage.</span></span> <span data-ttu-id="87761-107">Den bygger på hello [Data Movement aktiviteter](data-factory-data-movement-activities.md) artikel som ger en allmän översikt över dataflyttning hello kopieringsaktiviteten.</span><span class="sxs-lookup"><span data-stu-id="87761-107">It builds on hello [Data Movement Activities](data-factory-data-movement-activities.md) article, which presents a general overview of data movement with hello copy activity.</span></span>

## <a name="overview"></a><span data-ttu-id="87761-108">Översikt</span><span class="sxs-lookup"><span data-stu-id="87761-108">Overview</span></span>
<span data-ttu-id="87761-109">Du kan kopiera data från en stöds källa data tooAzure Blob Storage eller Azure Blob Storage tooany stöds sink data.</span><span class="sxs-lookup"><span data-stu-id="87761-109">You can copy data from any supported source data store tooAzure Blob Storage or from Azure Blob Storage tooany supported sink data store.</span></span> <span data-ttu-id="87761-110">hello följande tabell innehåller en lista över datakällor som stöds som källor eller egenskaperna av hello kopieringsaktiviteten.</span><span class="sxs-lookup"><span data-stu-id="87761-110">hello following table provides a list of data stores supported as sources or sinks by hello copy activity.</span></span> <span data-ttu-id="87761-111">Du kan till exempel flytta data **från** en SQL Server-databas eller en Azure SQL database **till** en Azure-blobblagring.</span><span class="sxs-lookup"><span data-stu-id="87761-111">For example, you can move data **from** a SQL Server database or an Azure SQL database **to** an Azure blob storage.</span></span> <span data-ttu-id="87761-112">Och du kan kopiera data **från** Azure-blobblagring **till** en Azure SQL Data Warehouse eller en Azure DB som Cosmos-samling.</span><span class="sxs-lookup"><span data-stu-id="87761-112">And, you can copy data **from** Azure blob storage **to** an Azure SQL Data Warehouse or an Azure Cosmos DB collection.</span></span> 

## <a name="supported-scenarios"></a><span data-ttu-id="87761-113">Scenarier som stöds</span><span class="sxs-lookup"><span data-stu-id="87761-113">Supported scenarios</span></span>
<span data-ttu-id="87761-114">Du kan kopiera data **från Azure Blob Storage** toohello följande datakällor:</span><span class="sxs-lookup"><span data-stu-id="87761-114">You can copy data **from Azure Blob Storage** toohello following data stores:</span></span>

[!INCLUDE [data-factory-supported-sink](../../includes/data-factory-supported-sinks.md)]

<span data-ttu-id="87761-115">Du kan kopiera data från hello följande datalager **tooAzure Blob Storage**:</span><span class="sxs-lookup"><span data-stu-id="87761-115">You can copy data from hello following data stores **tooAzure Blob Storage**:</span></span>

[!INCLUDE [data-factory-supported-sources](../../includes/data-factory-supported-sources.md)]
 
> [!IMPORTANT]
> <span data-ttu-id="87761-116">Kopieringsaktiviteten stöder kopiering av data från / tooboth allmänna Azure Storage-konton och Hot/kall Blob storage.</span><span class="sxs-lookup"><span data-stu-id="87761-116">Copy Activity supports copying data from/tooboth general-purpose Azure Storage accounts and Hot/Cool Blob storage.</span></span> <span data-ttu-id="87761-117">hello aktiviteten stöder **läsning från block, Lägg till eller sidblobbar**, men stöder **skriva tooonly blockblobbar**.</span><span class="sxs-lookup"><span data-stu-id="87761-117">hello activity supports **reading from block, append, or page blobs**, but supports **writing tooonly block blobs**.</span></span> <span data-ttu-id="87761-118">Azure Premium-lagring stöds inte som en mottagare eftersom den backas upp av sidblobar.</span><span class="sxs-lookup"><span data-stu-id="87761-118">Azure Premium Storage is not supported as a sink because it is backed by page blobs.</span></span>
> 
> <span data-ttu-id="87761-119">Kopieringsaktiviteten tar inte bort data från hello källa när hello data är har kopierats toohello mål.</span><span class="sxs-lookup"><span data-stu-id="87761-119">Copy Activity does not delete data from hello source after hello data is successfully copied toohello destination.</span></span> <span data-ttu-id="87761-120">Om du behöver toodelete källdata efter en lyckad kopiering, skapa en [anpassad aktivitet](data-factory-use-custom-activities.md) toodelete hello data och använda hello aktivitet i hello pipeline.</span><span class="sxs-lookup"><span data-stu-id="87761-120">If you need toodelete source data after a successful copy, create a [custom activity](data-factory-use-custom-activities.md) toodelete hello data and use hello activity in hello pipeline.</span></span> <span data-ttu-id="87761-121">Ett exempel finns hello [ta bort blob- eller mappnamn prov på GitHub](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/DeleteBlobFileFolderCustomActivity).</span><span class="sxs-lookup"><span data-stu-id="87761-121">For an example, see hello [Delete blob or folder sample on GitHub](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/DeleteBlobFileFolderCustomActivity).</span></span> 

## <a name="get-started"></a><span data-ttu-id="87761-122">Kom igång</span><span class="sxs-lookup"><span data-stu-id="87761-122">Get started</span></span>
<span data-ttu-id="87761-123">Du kan skapa en pipeline med en kopia-aktivitet som flyttar data till och från ett Azure Blob Storage med hjälp av olika verktyg/API: er.</span><span class="sxs-lookup"><span data-stu-id="87761-123">You can create a pipeline with a copy activity that moves data to/from an Azure Blob Storage by using different tools/APIs.</span></span>

<span data-ttu-id="87761-124">hello enklaste sättet toocreate en pipeline är toouse hello **guiden Kopiera**.</span><span class="sxs-lookup"><span data-stu-id="87761-124">hello easiest way toocreate a pipeline is toouse hello **Copy Wizard**.</span></span> <span data-ttu-id="87761-125">Den här artikeln innehåller en [genomgången](#walkthrough-use-copy-wizard-to-copy-data-tofrom-blob-storage) för att skapa en pipeline toocopy data från ett Azure Blob Storage plats tooanother Azure Blob Storage-plats.</span><span class="sxs-lookup"><span data-stu-id="87761-125">This article has a [walkthrough](#walkthrough-use-copy-wizard-to-copy-data-tofrom-blob-storage) for creating a pipeline toocopy data from an Azure Blob Storage location  tooanother Azure Blob Storage location.</span></span> <span data-ttu-id="87761-126">En självstudiekurs om hur du skapar en pipeline toocopy data från ett Azure Blob Storage tooAzure SQL-databas finns [Självstudier: skapa en pipeline med hjälp av guiden Kopiera](data-factory-copy-data-wizard-tutorial.md).</span><span class="sxs-lookup"><span data-stu-id="87761-126">For a tutorial on creating a pipeline toocopy data from an Azure Blob Storage tooAzure SQL Database, see [Tutorial: Create a pipeline using Copy Wizard](data-factory-copy-data-wizard-tutorial.md).</span></span>

<span data-ttu-id="87761-127">Du kan också använda följande verktyg toocreate en pipeline hello: **Azure-portalen**, **Visual Studio**, **Azure PowerShell**, **Azure Resource Manager-mall** , **.NET API**, och **REST API**.</span><span class="sxs-lookup"><span data-stu-id="87761-127">You can also use hello following tools toocreate a pipeline: **Azure portal**, **Visual Studio**, **Azure PowerShell**, **Azure Resource Manager template**, **.NET API**, and **REST API**.</span></span> <span data-ttu-id="87761-128">Se [kopiera aktivitet kursen](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) för stegvisa instruktioner toocreate en pipeline med en Kopieringsaktivitet.</span><span class="sxs-lookup"><span data-stu-id="87761-128">See [Copy activity tutorial](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) for step-by-step instructions toocreate a pipeline with a copy activity.</span></span>

<span data-ttu-id="87761-129">Om du använder hello verktyg eller API: er kan utföra du hello följande steg toocreate en pipeline som flyttar data från en källdata lagra tooa sink-datalagret:</span><span class="sxs-lookup"><span data-stu-id="87761-129">Whether you use hello tools or APIs, you perform hello following steps toocreate a pipeline that moves data from a source data store tooa sink data store:</span></span>

1. <span data-ttu-id="87761-130">Skapa en **datafabriken**.</span><span class="sxs-lookup"><span data-stu-id="87761-130">Create a **data factory**.</span></span> <span data-ttu-id="87761-131">En datafabrik kan innehålla en eller flera pipelines.</span><span class="sxs-lookup"><span data-stu-id="87761-131">A data factory may contain one or more pipelines.</span></span> 
2. <span data-ttu-id="87761-132">Skapa **länkade tjänster** toolink indata och utdata lagrar tooyour data factory.</span><span class="sxs-lookup"><span data-stu-id="87761-132">Create **linked services** toolink input and output data stores tooyour data factory.</span></span> <span data-ttu-id="87761-133">Till exempel om du kopierar data från en Azure blob storage tooan Azure SQL database skapa du två länkade tjänster toolink dina Azure storage-konto och Azure SQL database tooyour data factory.</span><span class="sxs-lookup"><span data-stu-id="87761-133">For example, if you are copying data from an Azure blob storage tooan Azure SQL database, you create two linked services toolink your Azure storage account and Azure SQL database tooyour data factory.</span></span> <span data-ttu-id="87761-134">Länkad tjänstegenskaper som är specifika tooAzure Blob Storage, se [länkade tjänstegenskaper](#linked-service-properties) avsnitt.</span><span class="sxs-lookup"><span data-stu-id="87761-134">For linked service properties that are specific tooAzure Blob Storage, see [linked service properties](#linked-service-properties) section.</span></span> 
2. <span data-ttu-id="87761-135">Skapa **datauppsättningar** toorepresent indata och utdata för hello kopieringsåtgärden.</span><span class="sxs-lookup"><span data-stu-id="87761-135">Create **datasets** toorepresent input and output data for hello copy operation.</span></span> <span data-ttu-id="87761-136">I hello exempelvis nämns i hello sista steget skapar du en dataset toospecify hello blob-behållaren och mappen som innehåller hello indata.</span><span class="sxs-lookup"><span data-stu-id="87761-136">In hello example mentioned in hello last step, you create a dataset toospecify hello blob container and folder that contains hello input data.</span></span> <span data-ttu-id="87761-137">Och du skapar en annan dataset toospecify hello SQL-tabellen i hello Azure SQL-databas som innehåller hello data som kopieras från hello blob storage.</span><span class="sxs-lookup"><span data-stu-id="87761-137">And, you create another dataset toospecify hello SQL table in hello Azure SQL database that holds hello data copied from hello blob storage.</span></span> <span data-ttu-id="87761-138">Egenskaper för datamängd som är specifika tooAzure Blob Storage, se [egenskaper för datamängd](#dataset-properties) avsnitt.</span><span class="sxs-lookup"><span data-stu-id="87761-138">For dataset properties that are specific tooAzure Blob Storage, see [dataset properties](#dataset-properties) section.</span></span>
3. <span data-ttu-id="87761-139">Skapa en **pipeline** med en kopia-aktivitet som tar en datamängd som indata och en dataset som utdata.</span><span class="sxs-lookup"><span data-stu-id="87761-139">Create a **pipeline** with a copy activity that takes a dataset as an input and a dataset as an output.</span></span> <span data-ttu-id="87761-140">I hello-exemplet ovan, använder du BlobSource som en källa och SqlSink som en mottagare för hello kopieringsaktiviteten.</span><span class="sxs-lookup"><span data-stu-id="87761-140">In hello example mentioned earlier, you use BlobSource as a source and SqlSink as a sink for hello copy activity.</span></span> <span data-ttu-id="87761-141">På samma sätt om du vill kopiera från Azure SQL Database tooAzure Blob Storage, använder du SqlSource och BlobSink i hello kopieringsaktiviteten.</span><span class="sxs-lookup"><span data-stu-id="87761-141">Similarly, if you are copying from Azure SQL Database tooAzure Blob Storage, you use SqlSource and BlobSink in hello copy activity.</span></span> <span data-ttu-id="87761-142">Kopiera Aktivitetsegenskaper som är specifika tooAzure Blob Storage, se [kopiera Aktivitetsegenskaper](#copy-activity-properties) avsnitt.</span><span class="sxs-lookup"><span data-stu-id="87761-142">For copy activity properties that are specific tooAzure Blob Storage, see [copy activity properties](#copy-activity-properties) section.</span></span> <span data-ttu-id="87761-143">Mer information om hur toouse en databas som en källa eller en mottagare klickar du på hello länk under föregående hello för datalager.</span><span class="sxs-lookup"><span data-stu-id="87761-143">For details on how toouse a data store as a source or a sink, click hello link in hello previous section for your data store.</span></span>  

<span data-ttu-id="87761-144">När du använder guiden hello skapas automatiskt JSON definitioner för dessa Data Factory-enheter (länkade tjänster, datauppsättningar och hello pipeline) för dig.</span><span class="sxs-lookup"><span data-stu-id="87761-144">When you use hello wizard, JSON definitions for these Data Factory entities (linked services, datasets, and hello pipeline) are automatically created for you.</span></span> <span data-ttu-id="87761-145">När du använder Verktyg/API: er (utom .NET API), kan du definiera dessa Data Factory-enheter med hjälp av hello JSON-format.</span><span class="sxs-lookup"><span data-stu-id="87761-145">When you use tools/APIs (except .NET API), you define these Data Factory entities by using hello JSON format.</span></span>  <span data-ttu-id="87761-146">Exempel med JSON-definitioner för Data Factory-entiteter som har använt toocopy data till/från en Azure Blob Storage finns [JSON-exempel](#json-examples-for-copying-data-to-and-from-blob-storage  ) i den här artikeln.</span><span class="sxs-lookup"><span data-stu-id="87761-146">For samples with JSON definitions for Data Factory entities that are used toocopy data to/from an Azure Blob Storage, see [JSON examples](#json-examples-for-copying-data-to-and-from-blob-storage  ) section of this article.</span></span>

<span data-ttu-id="87761-147">hello följande avsnitt innehåller information om JSON-egenskaper används toodefine Data Factory entiteter specifika tooAzure Blob Storage.</span><span class="sxs-lookup"><span data-stu-id="87761-147">hello following sections provide details about JSON properties that are used toodefine Data Factory entities specific tooAzure Blob Storage.</span></span>

## <a name="linked-service-properties"></a><span data-ttu-id="87761-148">Länkad tjänstegenskaper</span><span class="sxs-lookup"><span data-stu-id="87761-148">Linked service properties</span></span>
<span data-ttu-id="87761-149">Det finns två typer av länkade tjänster kan du använda toolink ett Azure Storage tooan Azure data factory.</span><span class="sxs-lookup"><span data-stu-id="87761-149">There are two types of linked services you can use toolink an Azure Storage tooan Azure data factory.</span></span> <span data-ttu-id="87761-150">De är: **AzureStorage** länkade tjänsten och **AzureStorageSas** länkade tjänsten.</span><span class="sxs-lookup"><span data-stu-id="87761-150">They are: **AzureStorage** linked service and **AzureStorageSas** linked service.</span></span> <span data-ttu-id="87761-151">hello länkad Azure Storage-tjänst ger hello data factory med global åtkomst toohello Azure Storage.</span><span class="sxs-lookup"><span data-stu-id="87761-151">hello Azure Storage linked service provides hello data factory with global access toohello Azure Storage.</span></span> <span data-ttu-id="87761-152">Medan hello Azure Storage SAS (signatur för delad åtkomst) länkad ger tjänst hello data factory med begränsad/Tidsbundna åtkomst toohello Azure Storage.</span><span class="sxs-lookup"><span data-stu-id="87761-152">Whereas, hello Azure Storage SAS (Shared Access Signature) linked service provides hello data factory with restricted/time-bound access toohello Azure Storage.</span></span> <span data-ttu-id="87761-153">Det finns några skillnader mellan dessa två länkade tjänster.</span><span class="sxs-lookup"><span data-stu-id="87761-153">There are no other differences between these two linked services.</span></span> <span data-ttu-id="87761-154">Välj hello länkade tjänst som passar dina behov.</span><span class="sxs-lookup"><span data-stu-id="87761-154">Choose hello linked service that suits your needs.</span></span> <span data-ttu-id="87761-155">hello följande avsnitt innehåller mer information om dessa två länkade tjänster.</span><span class="sxs-lookup"><span data-stu-id="87761-155">hello following sections provide more details on these two linked services.</span></span>

[!INCLUDE [data-factory-azure-storage-linked-services](../../includes/data-factory-azure-storage-linked-services.md)]

## <a name="dataset-properties"></a><span data-ttu-id="87761-156">Egenskaper för datamängd</span><span class="sxs-lookup"><span data-stu-id="87761-156">Dataset properties</span></span>
<span data-ttu-id="87761-157">toospecify en dataset toorepresent inkommande eller utgående data i ett Azure Blob Storage, anger du egenskapen hello typ av hello datamängden: **AzureBlob**.</span><span class="sxs-lookup"><span data-stu-id="87761-157">toospecify a dataset toorepresent input or output data in an Azure Blob Storage, you set hello type property of hello dataset to: **AzureBlob**.</span></span> <span data-ttu-id="87761-158">Ange hello **linkedServiceName** -egenskapen för hello dataset toohello namnet på hello Azure Storage eller Azure Storage SAS länkade tjänsten.</span><span class="sxs-lookup"><span data-stu-id="87761-158">Set hello **linkedServiceName** property of hello dataset toohello name of hello Azure Storage or Azure Storage SAS linked service.</span></span>  <span data-ttu-id="87761-159">hello egenskaper i hello dataset ange hello **blobbehållaren** och hello **mappen** i hello blob storage.</span><span class="sxs-lookup"><span data-stu-id="87761-159">hello type properties of hello dataset specify hello **blob container** and hello **folder** in hello blob storage.</span></span>

<span data-ttu-id="87761-160">En fullständig lista över JSON avsnitt & egenskaper som är tillgängliga för att definiera datauppsättningar finns hello [skapa datauppsättningar](data-factory-create-datasets.md) artikel.</span><span class="sxs-lookup"><span data-stu-id="87761-160">For a full list of JSON sections & properties available for defining datasets, see hello [Creating datasets](data-factory-create-datasets.md) article.</span></span> <span data-ttu-id="87761-161">Avsnitt som struktur, tillgänglighet och princip på en datamängd JSON är liknande för alla typer av dataset (Azure SQL Azure blob, Azure-tabellen, osv.).</span><span class="sxs-lookup"><span data-stu-id="87761-161">Sections such as structure, availability, and policy of a dataset JSON are similar for all dataset types (Azure SQL, Azure blob, Azure table, etc.).</span></span>

<span data-ttu-id="87761-162">Data factory stöder hello följande CLS-kompatibelt .NET baserat värden av typen för att ange information i ”struktur” för schemat på Läs datakällor som Azure blob: Int16, Int32, Int64, enskild, Double, Decimal, Byte [], Bool, String, Guid, Datetime, DateTimeOffset Timespan.</span><span class="sxs-lookup"><span data-stu-id="87761-162">Data factory supports hello following CLS-compliant .NET based type values for providing type information in “structure” for schema-on-read data sources like Azure blob: Int16, Int32, Int64, Single, Double, Decimal, Byte[], Bool, String, Guid, Datetime, Datetimeoffset, Timespan.</span></span> <span data-ttu-id="87761-163">Data Factory utför konverteringar automatiskt när du flyttar från en källdata datalager tooa sink-datalagret.</span><span class="sxs-lookup"><span data-stu-id="87761-163">Data Factory automatically performs type conversions when moving data from a source data store tooa sink data store.</span></span>

<span data-ttu-id="87761-164">Hej **typeProperties** avsnitt är olika för varje typ av dataset och innehåller information om hello plats, format, etc. hello data i datalagret hello.</span><span class="sxs-lookup"><span data-stu-id="87761-164">hello **typeProperties** section is different for each type of dataset and provides information about hello location, format etc., of hello data in hello data store.</span></span> <span data-ttu-id="87761-165">Hej typeProperties avsnittet för dataset av typen **AzureBlob** datamängden har hello följande egenskaper:</span><span class="sxs-lookup"><span data-stu-id="87761-165">hello typeProperties section for dataset of type **AzureBlob** dataset has hello following properties:</span></span>

| <span data-ttu-id="87761-166">Egenskap</span><span class="sxs-lookup"><span data-stu-id="87761-166">Property</span></span> | <span data-ttu-id="87761-167">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="87761-167">Description</span></span> | <span data-ttu-id="87761-168">Krävs</span><span class="sxs-lookup"><span data-stu-id="87761-168">Required</span></span> |
| --- | --- | --- |
| <span data-ttu-id="87761-169">folderPath</span><span class="sxs-lookup"><span data-stu-id="87761-169">folderPath</span></span> |<span data-ttu-id="87761-170">Sökvägen toohello behållaren och mappen i hello blob storage.</span><span class="sxs-lookup"><span data-stu-id="87761-170">Path toohello container and folder in hello blob storage.</span></span> <span data-ttu-id="87761-171">Exempel: myblobcontainer\myblobfolder\\</span><span class="sxs-lookup"><span data-stu-id="87761-171">Example: myblobcontainer\myblobfolder\\</span></span> |<span data-ttu-id="87761-172">Ja</span><span class="sxs-lookup"><span data-stu-id="87761-172">Yes</span></span> |
| <span data-ttu-id="87761-173">fileName</span><span class="sxs-lookup"><span data-stu-id="87761-173">fileName</span></span> |<span data-ttu-id="87761-174">Namnet på hello-blob.</span><span class="sxs-lookup"><span data-stu-id="87761-174">Name of hello blob.</span></span> <span data-ttu-id="87761-175">Filnamnet är valfria och skiftlägeskänsligt.</span><span class="sxs-lookup"><span data-stu-id="87761-175">fileName is optional and case-sensitive.</span></span><br/><br/><span data-ttu-id="87761-176">Om du anger ett filnamn, hello aktivitet (inklusive kopia) fungerar på hello specifika Blob.</span><span class="sxs-lookup"><span data-stu-id="87761-176">If you specify a filename, hello activity (including Copy) works on hello specific Blob.</span></span><br/><br/><span data-ttu-id="87761-177">Om filnamnet har angetts innehåller kopiera alla BLOB i hello folderPath för inkommande dataset.</span><span class="sxs-lookup"><span data-stu-id="87761-177">When fileName is not specified, Copy includes all Blobs in hello folderPath for input dataset.</span></span><br/><br/><span data-ttu-id="87761-178">När **fileName** har inte angetts för en datamängd för utdata och **preserveHierarchy** har inte angetts i aktiviteten sink hello namnet på hello genereras skulle vara i hello efter det här formatet: Data.<Guid>. txt (till exempel:: Data.0a405f8a-93ff-4c6f-b3be-f69616f1df7a.txt</span><span class="sxs-lookup"><span data-stu-id="87761-178">When **fileName** is not specified for an output dataset and **preserveHierarchy** is not specified in activity sink, hello name of hello generated file would be in hello following this format: Data.<Guid>.txt (for example: : Data.0a405f8a-93ff-4c6f-b3be-f69616f1df7a.txt</span></span> |<span data-ttu-id="87761-179">Nej</span><span class="sxs-lookup"><span data-stu-id="87761-179">No</span></span> |
| <span data-ttu-id="87761-180">partitionedBy</span><span class="sxs-lookup"><span data-stu-id="87761-180">partitionedBy</span></span> |<span data-ttu-id="87761-181">partitionedBy är en valfri egenskap.</span><span class="sxs-lookup"><span data-stu-id="87761-181">partitionedBy is an optional property.</span></span> <span data-ttu-id="87761-182">Du kan använda det toospecify dynamiska folderPath och filnamnet för tid series-data.</span><span class="sxs-lookup"><span data-stu-id="87761-182">You can use it toospecify a dynamic folderPath and filename for time series data.</span></span> <span data-ttu-id="87761-183">Exempelvis kan folderPath parameteriseras för varje timme av data.</span><span class="sxs-lookup"><span data-stu-id="87761-183">For example, folderPath can be parameterized for every hour of data.</span></span> <span data-ttu-id="87761-184">Se hello [med partitionedBy egenskap](#using-partitionedBy-property) information och exempel.</span><span class="sxs-lookup"><span data-stu-id="87761-184">See hello [Using partitionedBy property section](#using-partitionedBy-property) for details and examples.</span></span> |<span data-ttu-id="87761-185">Nej</span><span class="sxs-lookup"><span data-stu-id="87761-185">No</span></span> |
| <span data-ttu-id="87761-186">Format</span><span class="sxs-lookup"><span data-stu-id="87761-186">format</span></span> | <span data-ttu-id="87761-187">hello följande formattyper stöds: **TextFormat**, **JsonFormat**, **AvroFormat**, **OrcFormat**,  **ParquetFormat**.</span><span class="sxs-lookup"><span data-stu-id="87761-187">hello following format types are supported: **TextFormat**, **JsonFormat**, **AvroFormat**, **OrcFormat**, **ParquetFormat**.</span></span> <span data-ttu-id="87761-188">Ange hello **typen** egenskap under format tooone av dessa värden.</span><span class="sxs-lookup"><span data-stu-id="87761-188">Set hello **type** property under format tooone of these values.</span></span> <span data-ttu-id="87761-189">Mer information finns i [textformat](data-factory-supported-file-and-compression-formats.md#text-format), [Json-Format](data-factory-supported-file-and-compression-formats.md#json-format), [Avro-formatet](data-factory-supported-file-and-compression-formats.md#avro-format), [Orc Format](data-factory-supported-file-and-compression-formats.md#orc-format), och [parkettgolv Format](data-factory-supported-file-and-compression-formats.md#parquet-format) avsnitt.</span><span class="sxs-lookup"><span data-stu-id="87761-189">For more information, see [Text Format](data-factory-supported-file-and-compression-formats.md#text-format), [Json Format](data-factory-supported-file-and-compression-formats.md#json-format), [Avro Format](data-factory-supported-file-and-compression-formats.md#avro-format), [Orc Format](data-factory-supported-file-and-compression-formats.md#orc-format), and [Parquet Format](data-factory-supported-file-and-compression-formats.md#parquet-format) sections.</span></span> <br><br> <span data-ttu-id="87761-190">Om du vill använda för**kopiera filer som-är** mellan filbaserade butiker (binär kopia), hoppar du över hello format-avsnittet i både inkommande och utgående dataset-definitioner.</span><span class="sxs-lookup"><span data-stu-id="87761-190">If you want too**copy files as-is** between file-based stores (binary copy), skip hello format section in both input and output dataset definitions.</span></span> |<span data-ttu-id="87761-191">Nej</span><span class="sxs-lookup"><span data-stu-id="87761-191">No</span></span> |
| <span data-ttu-id="87761-192">Komprimering</span><span class="sxs-lookup"><span data-stu-id="87761-192">compression</span></span> | <span data-ttu-id="87761-193">Ange hello typ och kompression för hello data.</span><span class="sxs-lookup"><span data-stu-id="87761-193">Specify hello type and level of compression for hello data.</span></span> <span data-ttu-id="87761-194">Typer som stöds är: **GZip**, **Deflate**, **BZip2**, och **ZipDeflate**.</span><span class="sxs-lookup"><span data-stu-id="87761-194">Supported types are: **GZip**, **Deflate**, **BZip2**, and **ZipDeflate**.</span></span> <span data-ttu-id="87761-195">Nivåer som stöds är: **Optimal** och **snabbast**.</span><span class="sxs-lookup"><span data-stu-id="87761-195">Supported levels are: **Optimal** and **Fastest**.</span></span> <span data-ttu-id="87761-196">Mer information finns i [format och komprimering i Azure Data Factory](data-factory-supported-file-and-compression-formats.md#compression-support).</span><span class="sxs-lookup"><span data-stu-id="87761-196">For more information, see [File and compression formats in Azure Data Factory](data-factory-supported-file-and-compression-formats.md#compression-support).</span></span> |<span data-ttu-id="87761-197">Nej</span><span class="sxs-lookup"><span data-stu-id="87761-197">No</span></span> |

### <a name="using-partitionedby-property"></a><span data-ttu-id="87761-198">Med hjälp av partitionedBy egenskap</span><span class="sxs-lookup"><span data-stu-id="87761-198">Using partitionedBy property</span></span>
<span data-ttu-id="87761-199">Som nämnts tidigare under hello, du kan ange en dynamisk folderPath och ett filnamn för tid series-data med hello **partitionedBy** egenskapen [Data Factory-funktioner och hello systemvariabler](data-factory-functions-variables.md).</span><span class="sxs-lookup"><span data-stu-id="87761-199">As mentioned in hello previous section, you can specify a dynamic folderPath and filename for time series data with hello **partitionedBy** property, [Data Factory functions, and hello system variables](data-factory-functions-variables.md).</span></span>

<span data-ttu-id="87761-200">Mer information om tid serien datauppsättningar, schemaläggning och segment finns [skapa datauppsättningar](data-factory-create-datasets.md) och [schemaläggning och utförande](data-factory-scheduling-and-execution.md) artiklar.</span><span class="sxs-lookup"><span data-stu-id="87761-200">For more information on time series datasets, scheduling, and slices, see [Creating Datasets](data-factory-create-datasets.md) and [Scheduling & Execution](data-factory-scheduling-and-execution.md) articles.</span></span>

#### <a name="sample-1"></a><span data-ttu-id="87761-201">Exempel 1</span><span class="sxs-lookup"><span data-stu-id="87761-201">Sample 1</span></span>

```json
"folderPath": "wikidatagateway/wikisampledataout/{Slice}",
"partitionedBy":
[
    { "name": "Slice", "value": { "type": "DateTime", "date": "SliceStart", "format": "yyyyMMddHH" } },
],
```

<span data-ttu-id="87761-202">I det här exemplet {segment} ersätts med hello värde för Data Factory systemvariabel SliceStart hello-format (YYYYMMDDHH).</span><span class="sxs-lookup"><span data-stu-id="87761-202">In this example, {Slice} is replaced with hello value of Data Factory system variable SliceStart in hello format (YYYYMMDDHH) specified.</span></span> <span data-ttu-id="87761-203">Hej SliceStart refererar toostart tiden för hello sektorn.</span><span class="sxs-lookup"><span data-stu-id="87761-203">hello SliceStart refers toostart time of hello slice.</span></span> <span data-ttu-id="87761-204">hello folderPath är olika för varje segment.</span><span class="sxs-lookup"><span data-stu-id="87761-204">hello folderPath is different for each slice.</span></span> <span data-ttu-id="87761-205">Till exempel: 2014100103-wikidatagateway/wikisampledataout eller 2014100104-wikidatagateway/wikisampledataout</span><span class="sxs-lookup"><span data-stu-id="87761-205">For example: wikidatagateway/wikisampledataout/2014100103 or wikidatagateway/wikisampledataout/2014100104</span></span>

#### <a name="sample-2"></a><span data-ttu-id="87761-206">Exempel 2</span><span class="sxs-lookup"><span data-stu-id="87761-206">Sample 2</span></span>

```json
"folderPath": "wikidatagateway/wikisampledataout/{Year}/{Month}/{Day}",
"fileName": "{Hour}.csv",
"partitionedBy":
 [
    { "name": "Year", "value": { "type": "DateTime", "date": "SliceStart", "format": "yyyy" } },
    { "name": "Month", "value": { "type": "DateTime", "date": "SliceStart", "format": "MM" } },
    { "name": "Day", "value": { "type": "DateTime", "date": "SliceStart", "format": "dd" } },
    { "name": "Hour", "value": { "type": "DateTime", "date": "SliceStart", "format": "hh" } }
],
```

<span data-ttu-id="87761-207">I det här exemplet extraheras år, månad, dag och tidpunkt för SliceStart till olika variabler som används av egenskaperna folderPath och filnamn.</span><span class="sxs-lookup"><span data-stu-id="87761-207">In this example, year, month, day, and time of SliceStart are extracted into separate variables that are used by folderPath and fileName properties.</span></span>

## <a name="copy-activity-properties"></a><span data-ttu-id="87761-208">Kopiera egenskaper för aktivitet</span><span class="sxs-lookup"><span data-stu-id="87761-208">Copy activity properties</span></span>
<span data-ttu-id="87761-209">En fullständig lista över avsnitt & egenskaper som är tillgängliga för att definiera aktiviteter finns hello [skapar Pipelines](data-factory-create-pipelines.md) artikel.</span><span class="sxs-lookup"><span data-stu-id="87761-209">For a full list of sections & properties available for defining activities, see hello [Creating Pipelines](data-factory-create-pipelines.md) article.</span></span> <span data-ttu-id="87761-210">Egenskaper som namn, beskrivning, indata och utdata-datauppsättningar och -principer är tillgängliga för alla typer av aktiviteter.</span><span class="sxs-lookup"><span data-stu-id="87761-210">Properties such as name, description, input and output datasets, and policies are available for all types of activities.</span></span> <span data-ttu-id="87761-211">Medan egenskaper som är tillgängliga i hello **typeProperties** avsnittet hello aktivitet varierar med varje aktivitetstyp.</span><span class="sxs-lookup"><span data-stu-id="87761-211">Whereas, properties available in hello **typeProperties** section of hello activity vary with each activity type.</span></span> <span data-ttu-id="87761-212">För Kopieringsaktivitet kan varierar de beroende på hello typer av datakällor och sänkor.</span><span class="sxs-lookup"><span data-stu-id="87761-212">For Copy activity, they vary depending on hello types of sources and sinks.</span></span> <span data-ttu-id="87761-213">Om du flyttar data från ett Azure Blob Storage måste du ställa in hello källtypen i hello kopieringsaktiviteten också**BlobSource**.</span><span class="sxs-lookup"><span data-stu-id="87761-213">If you are moving data from an Azure Blob Storage, you set hello source type in hello copy activity too**BlobSource**.</span></span> <span data-ttu-id="87761-214">På samma sätt om du flyttar data tooan Azure Blob Storage måste du ange hello Mottagartypen i hello kopieringsaktiviteten för**BlobSink**.</span><span class="sxs-lookup"><span data-stu-id="87761-214">Similarly, if you are moving data tooan Azure Blob Storage, you set hello sink type in hello copy activity too**BlobSink**.</span></span> <span data-ttu-id="87761-215">Det här avsnittet innehåller en lista över egenskaper som stöds av BlobSource och BlobSink.</span><span class="sxs-lookup"><span data-stu-id="87761-215">This section provides a list of properties supported by BlobSource and BlobSink.</span></span>

<span data-ttu-id="87761-216">**BlobSource** stöder följande egenskaper i hello hello **typeProperties** avsnitt:</span><span class="sxs-lookup"><span data-stu-id="87761-216">**BlobSource** supports hello following properties in hello **typeProperties** section:</span></span>

| <span data-ttu-id="87761-217">Egenskap</span><span class="sxs-lookup"><span data-stu-id="87761-217">Property</span></span> | <span data-ttu-id="87761-218">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="87761-218">Description</span></span> | <span data-ttu-id="87761-219">Tillåtna värden</span><span class="sxs-lookup"><span data-stu-id="87761-219">Allowed values</span></span> | <span data-ttu-id="87761-220">Krävs</span><span class="sxs-lookup"><span data-stu-id="87761-220">Required</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="87761-221">Rekursiva</span><span class="sxs-lookup"><span data-stu-id="87761-221">recursive</span></span> |<span data-ttu-id="87761-222">Anger om hello data läses rekursivt från hello undermappar eller endast hello angivna mappen.</span><span class="sxs-lookup"><span data-stu-id="87761-222">Indicates whether hello data is read recursively from hello sub folders or only from hello specified folder.</span></span> |<span data-ttu-id="87761-223">SANT (standardvärdet), FALSKT</span><span class="sxs-lookup"><span data-stu-id="87761-223">True (default value), False</span></span> |<span data-ttu-id="87761-224">Nej</span><span class="sxs-lookup"><span data-stu-id="87761-224">No</span></span> |

<span data-ttu-id="87761-225">**BlobSink** stöder följande egenskaper hello **typeProperties** avsnitt:</span><span class="sxs-lookup"><span data-stu-id="87761-225">**BlobSink** supports hello following properties **typeProperties** section:</span></span>

| <span data-ttu-id="87761-226">Egenskap</span><span class="sxs-lookup"><span data-stu-id="87761-226">Property</span></span> | <span data-ttu-id="87761-227">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="87761-227">Description</span></span> | <span data-ttu-id="87761-228">Tillåtna värden</span><span class="sxs-lookup"><span data-stu-id="87761-228">Allowed values</span></span> | <span data-ttu-id="87761-229">Krävs</span><span class="sxs-lookup"><span data-stu-id="87761-229">Required</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="87761-230">copyBehavior</span><span class="sxs-lookup"><span data-stu-id="87761-230">copyBehavior</span></span> |<span data-ttu-id="87761-231">Definierar hello kopiera beteende när hello källa är BlobSource eller filsystem.</span><span class="sxs-lookup"><span data-stu-id="87761-231">Defines hello copy behavior when hello source is BlobSource or FileSystem.</span></span> |<span data-ttu-id="87761-232"><b>PreserveHierarchy</b>: bevarar hello filstruktur i hello målmapp.</span><span class="sxs-lookup"><span data-stu-id="87761-232"><b>PreserveHierarchy</b>: preserves hello file hierarchy in hello target folder.</span></span> <span data-ttu-id="87761-233">hello relativa sökvägen till källmappen för filen toosource är identiska toohello relativa sökvägen till filen tootarget mapp.</span><span class="sxs-lookup"><span data-stu-id="87761-233">hello relative path of source file toosource folder is identical toohello relative path of target file tootarget folder.</span></span><br/><br/><span data-ttu-id="87761-234"><b>FlattenHierarchy</b>: alla filer från källmappen hello finns i hello först för målmappen.</span><span class="sxs-lookup"><span data-stu-id="87761-234"><b>FlattenHierarchy</b>: all files from hello source folder are in hello first level of target folder.</span></span> <span data-ttu-id="87761-235">hello målfilerna har genereras automatiskt namn.</span><span class="sxs-lookup"><span data-stu-id="87761-235">hello target files have auto generated name.</span></span> <br/><br/><span data-ttu-id="87761-236"><b>MergeFiles</b>: sammanfogar alla filer från hello källfil mappen tooone.</span><span class="sxs-lookup"><span data-stu-id="87761-236"><b>MergeFiles</b>: merges all files from hello source folder tooone file.</span></span> <span data-ttu-id="87761-237">Om hello fil/Blobbnamnet anges skulle hello kopplade filnamnet vara hello angivna name; annars skulle vara automatiskt genererade filnamn.</span><span class="sxs-lookup"><span data-stu-id="87761-237">If hello File/Blob Name is specified, hello merged file name would be hello specified name; otherwise, would be auto-generated file name.</span></span> |<span data-ttu-id="87761-238">Nej</span><span class="sxs-lookup"><span data-stu-id="87761-238">No</span></span> |

<span data-ttu-id="87761-239">**BlobSource** stöder också de här två egenskaperna för bakåtkompatibilitet.</span><span class="sxs-lookup"><span data-stu-id="87761-239">**BlobSource** also supports these two properties for backward compatibility.</span></span>

* <span data-ttu-id="87761-240">**treatEmptyAsNull**: Anger om tootreat null eller en tom sträng som null-värde.</span><span class="sxs-lookup"><span data-stu-id="87761-240">**treatEmptyAsNull**: Specifies whether tootreat null or empty string as null value.</span></span>
* <span data-ttu-id="87761-241">**skipHeaderLineCount** – anger hur många rader måste hoppas över.</span><span class="sxs-lookup"><span data-stu-id="87761-241">**skipHeaderLineCount** - Specifies how many lines need be skipped.</span></span> <span data-ttu-id="87761-242">Det gäller endast när inkommande dataset använder TextFormat.</span><span class="sxs-lookup"><span data-stu-id="87761-242">It is applicable only when input dataset is using TextFormat.</span></span>

<span data-ttu-id="87761-243">På liknande sätt **BlobSink** stöder hello efter egenskapen för bakåtkompatibilitet.</span><span class="sxs-lookup"><span data-stu-id="87761-243">Similarly, **BlobSink** supports hello following property for backward compatibility.</span></span>

* <span data-ttu-id="87761-244">**blobWriterAddHeader**: Anger om tooadd ett huvud för kolumndefinitionerna vid skrivning till tooan utdatauppsättningen.</span><span class="sxs-lookup"><span data-stu-id="87761-244">**blobWriterAddHeader**: Specifies whether tooadd a header of column definitions while writing tooan output dataset.</span></span>

<span data-ttu-id="87761-245">Följande egenskaper som implementerar datauppsättningar nu support hello hello samma funktion: **treatEmptyAsNull**, **skipLineCount**, **firstRowAsHeader**.</span><span class="sxs-lookup"><span data-stu-id="87761-245">Datasets now support hello following properties that implement hello same functionality: **treatEmptyAsNull**, **skipLineCount**, **firstRowAsHeader**.</span></span>

<span data-ttu-id="87761-246">hello ger följande tabell vägledning om hur du använder hello nya egenskaper för datamängd i stället för dessa källor/mottagare blob-egenskaper.</span><span class="sxs-lookup"><span data-stu-id="87761-246">hello following table provides guidance on using hello new dataset properties in place of these blob source/sink properties.</span></span>

| <span data-ttu-id="87761-247">Kopiera aktivitetsegenskap</span><span class="sxs-lookup"><span data-stu-id="87761-247">Copy Activity property</span></span> | <span data-ttu-id="87761-248">Egenskapen DataSet</span><span class="sxs-lookup"><span data-stu-id="87761-248">Dataset property</span></span> |
|:--- |:--- |
| <span data-ttu-id="87761-249">skipHeaderLineCount på BlobSource</span><span class="sxs-lookup"><span data-stu-id="87761-249">skipHeaderLineCount on BlobSource</span></span> |<span data-ttu-id="87761-250">skipLineCount och firstRowAsHeader.</span><span class="sxs-lookup"><span data-stu-id="87761-250">skipLineCount and firstRowAsHeader.</span></span> <span data-ttu-id="87761-251">Rader hoppas över först och sedan hello första rad läses som en rubrik.</span><span class="sxs-lookup"><span data-stu-id="87761-251">Lines are skipped first and then hello first row is read as a header.</span></span> |
| <span data-ttu-id="87761-252">treatEmptyAsNull på BlobSource</span><span class="sxs-lookup"><span data-stu-id="87761-252">treatEmptyAsNull on BlobSource</span></span> |<span data-ttu-id="87761-253">treatEmptyAsNull för inkommande datauppsättningen</span><span class="sxs-lookup"><span data-stu-id="87761-253">treatEmptyAsNull on input dataset</span></span> |
| <span data-ttu-id="87761-254">blobWriterAddHeader på BlobSink</span><span class="sxs-lookup"><span data-stu-id="87761-254">blobWriterAddHeader on BlobSink</span></span> |<span data-ttu-id="87761-255">firstRowAsHeader på datamängd för utdata</span><span class="sxs-lookup"><span data-stu-id="87761-255">firstRowAsHeader on output dataset</span></span> |

<span data-ttu-id="87761-256">Se [anger TextFormat](data-factory-supported-file-and-compression-formats.md#text-format) för detaljerad information om dessa egenskaper.</span><span class="sxs-lookup"><span data-stu-id="87761-256">See [Specifying TextFormat](data-factory-supported-file-and-compression-formats.md#text-format) section for detailed information on these properties.</span></span>    

### <a name="recursive-and-copybehavior-examples"></a><span data-ttu-id="87761-257">rekursiva och copyBehavior exempel</span><span class="sxs-lookup"><span data-stu-id="87761-257">recursive and copyBehavior examples</span></span>
<span data-ttu-id="87761-258">Det här avsnittet beskrivs hello resultatet av hello kopia för olika kombinationer av värden rekursiv och copyBehavior.</span><span class="sxs-lookup"><span data-stu-id="87761-258">This section describes hello resulting behavior of hello Copy operation for different combinations of recursive and copyBehavior values.</span></span>

| <span data-ttu-id="87761-259">Rekursiva</span><span class="sxs-lookup"><span data-stu-id="87761-259">recursive</span></span> | <span data-ttu-id="87761-260">copyBehavior</span><span class="sxs-lookup"><span data-stu-id="87761-260">copyBehavior</span></span> | <span data-ttu-id="87761-261">Resultatet</span><span class="sxs-lookup"><span data-stu-id="87761-261">Resulting behavior</span></span> |
| --- | --- | --- |
| <span data-ttu-id="87761-262">SANT</span><span class="sxs-lookup"><span data-stu-id="87761-262">true</span></span> |<span data-ttu-id="87761-263">preserveHierarchy</span><span class="sxs-lookup"><span data-stu-id="87761-263">preserveHierarchy</span></span> |<span data-ttu-id="87761-264">För en källmapp Mapp1 med hello följande struktur:</span><span class="sxs-lookup"><span data-stu-id="87761-264">For a source folder Folder1 with hello following structure:</span></span> <br/><br/><span data-ttu-id="87761-265">Mapp1</span><span class="sxs-lookup"><span data-stu-id="87761-265">Folder1</span></span><br/><span data-ttu-id="87761-266">&nbsp;&nbsp;&nbsp;&nbsp;File1</span><span class="sxs-lookup"><span data-stu-id="87761-266">&nbsp;&nbsp;&nbsp;&nbsp;File1</span></span><br/><span data-ttu-id="87761-267">&nbsp;&nbsp;&nbsp;&nbsp;Fil2</span><span class="sxs-lookup"><span data-stu-id="87761-267">&nbsp;&nbsp;&nbsp;&nbsp;File2</span></span><br/><span data-ttu-id="87761-268">&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1</span><span class="sxs-lookup"><span data-stu-id="87761-268">&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1</span></span><br/><span data-ttu-id="87761-269">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Fil3</span><span class="sxs-lookup"><span data-stu-id="87761-269">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File3</span></span><br/><span data-ttu-id="87761-270">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4</span><span class="sxs-lookup"><span data-stu-id="87761-270">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4</span></span><br/><span data-ttu-id="87761-271">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5</span><span class="sxs-lookup"><span data-stu-id="87761-271">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5</span></span><br/><br/><span data-ttu-id="87761-272">hello målmappen Mapp1 skapas med hello samma struktur som hello källa</span><span class="sxs-lookup"><span data-stu-id="87761-272">hello target folder Folder1 is created with hello same structure as hello source</span></span><br/><br/><span data-ttu-id="87761-273">Mapp1</span><span class="sxs-lookup"><span data-stu-id="87761-273">Folder1</span></span><br/><span data-ttu-id="87761-274">&nbsp;&nbsp;&nbsp;&nbsp;File1</span><span class="sxs-lookup"><span data-stu-id="87761-274">&nbsp;&nbsp;&nbsp;&nbsp;File1</span></span><br/><span data-ttu-id="87761-275">&nbsp;&nbsp;&nbsp;&nbsp;Fil2</span><span class="sxs-lookup"><span data-stu-id="87761-275">&nbsp;&nbsp;&nbsp;&nbsp;File2</span></span><br/><span data-ttu-id="87761-276">&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1</span><span class="sxs-lookup"><span data-stu-id="87761-276">&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1</span></span><br/><span data-ttu-id="87761-277">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Fil3</span><span class="sxs-lookup"><span data-stu-id="87761-277">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File3</span></span><br/><span data-ttu-id="87761-278">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4</span><span class="sxs-lookup"><span data-stu-id="87761-278">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4</span></span><br/><span data-ttu-id="87761-279">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5.</span><span class="sxs-lookup"><span data-stu-id="87761-279">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5.</span></span> |
| <span data-ttu-id="87761-280">SANT</span><span class="sxs-lookup"><span data-stu-id="87761-280">true</span></span> |<span data-ttu-id="87761-281">flattenHierarchy</span><span class="sxs-lookup"><span data-stu-id="87761-281">flattenHierarchy</span></span> |<span data-ttu-id="87761-282">För en källmapp Mapp1 med hello följande struktur:</span><span class="sxs-lookup"><span data-stu-id="87761-282">For a source folder Folder1 with hello following structure:</span></span> <br/><br/><span data-ttu-id="87761-283">Mapp1</span><span class="sxs-lookup"><span data-stu-id="87761-283">Folder1</span></span><br/><span data-ttu-id="87761-284">&nbsp;&nbsp;&nbsp;&nbsp;File1</span><span class="sxs-lookup"><span data-stu-id="87761-284">&nbsp;&nbsp;&nbsp;&nbsp;File1</span></span><br/><span data-ttu-id="87761-285">&nbsp;&nbsp;&nbsp;&nbsp;Fil2</span><span class="sxs-lookup"><span data-stu-id="87761-285">&nbsp;&nbsp;&nbsp;&nbsp;File2</span></span><br/><span data-ttu-id="87761-286">&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1</span><span class="sxs-lookup"><span data-stu-id="87761-286">&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1</span></span><br/><span data-ttu-id="87761-287">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Fil3</span><span class="sxs-lookup"><span data-stu-id="87761-287">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File3</span></span><br/><span data-ttu-id="87761-288">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4</span><span class="sxs-lookup"><span data-stu-id="87761-288">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4</span></span><br/><span data-ttu-id="87761-289">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5</span><span class="sxs-lookup"><span data-stu-id="87761-289">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5</span></span><br/><br/><span data-ttu-id="87761-290">hello mål Mapp1 skapas med följande struktur hello:</span><span class="sxs-lookup"><span data-stu-id="87761-290">hello target Folder1 is created with hello following structure:</span></span> <br/><br/><span data-ttu-id="87761-291">Mapp1</span><span class="sxs-lookup"><span data-stu-id="87761-291">Folder1</span></span><br/><span data-ttu-id="87761-292">&nbsp;&nbsp;&nbsp;&nbsp;automatiskt genererade namnet på File1</span><span class="sxs-lookup"><span data-stu-id="87761-292">&nbsp;&nbsp;&nbsp;&nbsp;auto-generated name for File1</span></span><br/><span data-ttu-id="87761-293">&nbsp;&nbsp;&nbsp;&nbsp;automatiskt genererat namn för fil2</span><span class="sxs-lookup"><span data-stu-id="87761-293">&nbsp;&nbsp;&nbsp;&nbsp;auto-generated name for File2</span></span><br/><span data-ttu-id="87761-294">&nbsp;&nbsp;&nbsp;&nbsp;automatiskt genererat namn för fil3</span><span class="sxs-lookup"><span data-stu-id="87761-294">&nbsp;&nbsp;&nbsp;&nbsp;auto-generated name for File3</span></span><br/><span data-ttu-id="87761-295">&nbsp;&nbsp;&nbsp;&nbsp;automatiskt genererat namn för File4</span><span class="sxs-lookup"><span data-stu-id="87761-295">&nbsp;&nbsp;&nbsp;&nbsp;auto-generated name for File4</span></span><br/><span data-ttu-id="87761-296">&nbsp;&nbsp;&nbsp;&nbsp;automatiskt genererat namn för File5</span><span class="sxs-lookup"><span data-stu-id="87761-296">&nbsp;&nbsp;&nbsp;&nbsp;auto-generated name for File5</span></span> |
| <span data-ttu-id="87761-297">SANT</span><span class="sxs-lookup"><span data-stu-id="87761-297">true</span></span> |<span data-ttu-id="87761-298">mergeFiles</span><span class="sxs-lookup"><span data-stu-id="87761-298">mergeFiles</span></span> |<span data-ttu-id="87761-299">För en källmapp Mapp1 med hello följande struktur:</span><span class="sxs-lookup"><span data-stu-id="87761-299">For a source folder Folder1 with hello following structure:</span></span> <br/><br/><span data-ttu-id="87761-300">Mapp1</span><span class="sxs-lookup"><span data-stu-id="87761-300">Folder1</span></span><br/><span data-ttu-id="87761-301">&nbsp;&nbsp;&nbsp;&nbsp;File1</span><span class="sxs-lookup"><span data-stu-id="87761-301">&nbsp;&nbsp;&nbsp;&nbsp;File1</span></span><br/><span data-ttu-id="87761-302">&nbsp;&nbsp;&nbsp;&nbsp;Fil2</span><span class="sxs-lookup"><span data-stu-id="87761-302">&nbsp;&nbsp;&nbsp;&nbsp;File2</span></span><br/><span data-ttu-id="87761-303">&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1</span><span class="sxs-lookup"><span data-stu-id="87761-303">&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1</span></span><br/><span data-ttu-id="87761-304">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Fil3</span><span class="sxs-lookup"><span data-stu-id="87761-304">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File3</span></span><br/><span data-ttu-id="87761-305">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4</span><span class="sxs-lookup"><span data-stu-id="87761-305">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4</span></span><br/><span data-ttu-id="87761-306">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5</span><span class="sxs-lookup"><span data-stu-id="87761-306">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5</span></span><br/><br/><span data-ttu-id="87761-307">hello mål Mapp1 skapas med följande struktur hello:</span><span class="sxs-lookup"><span data-stu-id="87761-307">hello target Folder1 is created with hello following structure:</span></span> <br/><br/><span data-ttu-id="87761-308">Mapp1</span><span class="sxs-lookup"><span data-stu-id="87761-308">Folder1</span></span><br/><span data-ttu-id="87761-309">&nbsp;&nbsp;&nbsp;&nbsp;Fil1 + fil2 + fil3 + File4 + filen 5 innehållet slås samman till en fil med automatiskt genererade namnet</span><span class="sxs-lookup"><span data-stu-id="87761-309">&nbsp;&nbsp;&nbsp;&nbsp;File1 + File2 + File3 + File4 + File 5 contents are merged into one file with auto-generated file name</span></span> |
| <span data-ttu-id="87761-310">FALSKT</span><span class="sxs-lookup"><span data-stu-id="87761-310">false</span></span> |<span data-ttu-id="87761-311">preserveHierarchy</span><span class="sxs-lookup"><span data-stu-id="87761-311">preserveHierarchy</span></span> |<span data-ttu-id="87761-312">För en källmapp Mapp1 med hello följande struktur:</span><span class="sxs-lookup"><span data-stu-id="87761-312">For a source folder Folder1 with hello following structure:</span></span> <br/><br/><span data-ttu-id="87761-313">Mapp1</span><span class="sxs-lookup"><span data-stu-id="87761-313">Folder1</span></span><br/><span data-ttu-id="87761-314">&nbsp;&nbsp;&nbsp;&nbsp;File1</span><span class="sxs-lookup"><span data-stu-id="87761-314">&nbsp;&nbsp;&nbsp;&nbsp;File1</span></span><br/><span data-ttu-id="87761-315">&nbsp;&nbsp;&nbsp;&nbsp;Fil2</span><span class="sxs-lookup"><span data-stu-id="87761-315">&nbsp;&nbsp;&nbsp;&nbsp;File2</span></span><br/><span data-ttu-id="87761-316">&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1</span><span class="sxs-lookup"><span data-stu-id="87761-316">&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1</span></span><br/><span data-ttu-id="87761-317">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Fil3</span><span class="sxs-lookup"><span data-stu-id="87761-317">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File3</span></span><br/><span data-ttu-id="87761-318">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4</span><span class="sxs-lookup"><span data-stu-id="87761-318">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4</span></span><br/><span data-ttu-id="87761-319">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5</span><span class="sxs-lookup"><span data-stu-id="87761-319">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5</span></span><br/><br/><span data-ttu-id="87761-320">hello målmappen Mapp1 skapas med följande struktur hello</span><span class="sxs-lookup"><span data-stu-id="87761-320">hello target folder Folder1 is created with hello following structure</span></span><br/><br/><span data-ttu-id="87761-321">Mapp1</span><span class="sxs-lookup"><span data-stu-id="87761-321">Folder1</span></span><br/><span data-ttu-id="87761-322">&nbsp;&nbsp;&nbsp;&nbsp;File1</span><span class="sxs-lookup"><span data-stu-id="87761-322">&nbsp;&nbsp;&nbsp;&nbsp;File1</span></span><br/><span data-ttu-id="87761-323">&nbsp;&nbsp;&nbsp;&nbsp;Fil2</span><span class="sxs-lookup"><span data-stu-id="87761-323">&nbsp;&nbsp;&nbsp;&nbsp;File2</span></span><br/><br/><br/><span data-ttu-id="87761-324">Subfolder1 med fil3, File4 och File5 har inte plockats.</span><span class="sxs-lookup"><span data-stu-id="87761-324">Subfolder1 with File3, File4, and File5 are not picked up.</span></span> |
| <span data-ttu-id="87761-325">FALSKT</span><span class="sxs-lookup"><span data-stu-id="87761-325">false</span></span> |<span data-ttu-id="87761-326">flattenHierarchy</span><span class="sxs-lookup"><span data-stu-id="87761-326">flattenHierarchy</span></span> |<span data-ttu-id="87761-327">För en källmapp Mapp1 med hello följande struktur:</span><span class="sxs-lookup"><span data-stu-id="87761-327">For a source folder Folder1 with hello following structure:</span></span><br/><br/><span data-ttu-id="87761-328">Mapp1</span><span class="sxs-lookup"><span data-stu-id="87761-328">Folder1</span></span><br/><span data-ttu-id="87761-329">&nbsp;&nbsp;&nbsp;&nbsp;File1</span><span class="sxs-lookup"><span data-stu-id="87761-329">&nbsp;&nbsp;&nbsp;&nbsp;File1</span></span><br/><span data-ttu-id="87761-330">&nbsp;&nbsp;&nbsp;&nbsp;Fil2</span><span class="sxs-lookup"><span data-stu-id="87761-330">&nbsp;&nbsp;&nbsp;&nbsp;File2</span></span><br/><span data-ttu-id="87761-331">&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1</span><span class="sxs-lookup"><span data-stu-id="87761-331">&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1</span></span><br/><span data-ttu-id="87761-332">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Fil3</span><span class="sxs-lookup"><span data-stu-id="87761-332">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File3</span></span><br/><span data-ttu-id="87761-333">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4</span><span class="sxs-lookup"><span data-stu-id="87761-333">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4</span></span><br/><span data-ttu-id="87761-334">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5</span><span class="sxs-lookup"><span data-stu-id="87761-334">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5</span></span><br/><br/><span data-ttu-id="87761-335">hello målmappen Mapp1 skapas med följande struktur hello</span><span class="sxs-lookup"><span data-stu-id="87761-335">hello target folder Folder1 is created with hello following structure</span></span><br/><br/><span data-ttu-id="87761-336">Mapp1</span><span class="sxs-lookup"><span data-stu-id="87761-336">Folder1</span></span><br/><span data-ttu-id="87761-337">&nbsp;&nbsp;&nbsp;&nbsp;automatiskt genererade namnet på File1</span><span class="sxs-lookup"><span data-stu-id="87761-337">&nbsp;&nbsp;&nbsp;&nbsp;auto-generated name for File1</span></span><br/><span data-ttu-id="87761-338">&nbsp;&nbsp;&nbsp;&nbsp;automatiskt genererat namn för fil2</span><span class="sxs-lookup"><span data-stu-id="87761-338">&nbsp;&nbsp;&nbsp;&nbsp;auto-generated name for File2</span></span><br/><br/><br/><span data-ttu-id="87761-339">Subfolder1 med fil3, File4 och File5 har inte plockats.</span><span class="sxs-lookup"><span data-stu-id="87761-339">Subfolder1 with File3, File4, and File5 are not picked up.</span></span> |
| <span data-ttu-id="87761-340">FALSKT</span><span class="sxs-lookup"><span data-stu-id="87761-340">false</span></span> |<span data-ttu-id="87761-341">mergeFiles</span><span class="sxs-lookup"><span data-stu-id="87761-341">mergeFiles</span></span> |<span data-ttu-id="87761-342">För en källmapp Mapp1 med hello följande struktur:</span><span class="sxs-lookup"><span data-stu-id="87761-342">For a source folder Folder1 with hello following structure:</span></span><br/><br/><span data-ttu-id="87761-343">Mapp1</span><span class="sxs-lookup"><span data-stu-id="87761-343">Folder1</span></span><br/><span data-ttu-id="87761-344">&nbsp;&nbsp;&nbsp;&nbsp;File1</span><span class="sxs-lookup"><span data-stu-id="87761-344">&nbsp;&nbsp;&nbsp;&nbsp;File1</span></span><br/><span data-ttu-id="87761-345">&nbsp;&nbsp;&nbsp;&nbsp;Fil2</span><span class="sxs-lookup"><span data-stu-id="87761-345">&nbsp;&nbsp;&nbsp;&nbsp;File2</span></span><br/><span data-ttu-id="87761-346">&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1</span><span class="sxs-lookup"><span data-stu-id="87761-346">&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1</span></span><br/><span data-ttu-id="87761-347">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Fil3</span><span class="sxs-lookup"><span data-stu-id="87761-347">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File3</span></span><br/><span data-ttu-id="87761-348">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4</span><span class="sxs-lookup"><span data-stu-id="87761-348">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4</span></span><br/><span data-ttu-id="87761-349">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5</span><span class="sxs-lookup"><span data-stu-id="87761-349">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5</span></span><br/><br/><span data-ttu-id="87761-350">hello målmappen Mapp1 skapas med följande struktur hello</span><span class="sxs-lookup"><span data-stu-id="87761-350">hello target folder Folder1 is created with hello following structure</span></span><br/><br/><span data-ttu-id="87761-351">Mapp1</span><span class="sxs-lookup"><span data-stu-id="87761-351">Folder1</span></span><br/><span data-ttu-id="87761-352">&nbsp;&nbsp;&nbsp;&nbsp;Fil1 + fil2 innehållet slås samman till en fil med automatiskt genererade namnet.</span><span class="sxs-lookup"><span data-stu-id="87761-352">&nbsp;&nbsp;&nbsp;&nbsp;File1 + File2 contents are merged into one file with auto-generated file name.</span></span> <span data-ttu-id="87761-353">automatiskt genererade namnet på File1</span><span class="sxs-lookup"><span data-stu-id="87761-353">auto-generated name for File1</span></span><br/><br/><span data-ttu-id="87761-354">Subfolder1 med fil3, File4 och File5 har inte plockats.</span><span class="sxs-lookup"><span data-stu-id="87761-354">Subfolder1 with File3, File4, and File5 are not picked up.</span></span> |

## <a name="walkthrough-use-copy-wizard-toocopy-data-tofrom-blob-storage"></a><span data-ttu-id="87761-355">Genomgång: Använd guiden för kopiera toocopy data till/från Blob Storage</span><span class="sxs-lookup"><span data-stu-id="87761-355">Walkthrough: Use Copy Wizard toocopy data to/from Blob Storage</span></span>
<span data-ttu-id="87761-356">Nu ska vi titta på hur tooquickly kopieringsdata till och från ett Azure blob storage.</span><span class="sxs-lookup"><span data-stu-id="87761-356">Let's look at how tooquickly copy data to/from an Azure blob storage.</span></span> <span data-ttu-id="87761-357">I den här genomgången lagrar data både källa och mål av typen: Azure Blob Storage.</span><span class="sxs-lookup"><span data-stu-id="87761-357">In this walkthrough, both source and destination data stores of type: Azure Blob Storage.</span></span> <span data-ttu-id="87761-358">hello pipeline i den här genomgången kopierar data från en tooanother-mappen i hello samma blob-behållare.</span><span class="sxs-lookup"><span data-stu-id="87761-358">hello pipeline in this walkthrough copies data from a folder tooanother folder in hello same blob container.</span></span> <span data-ttu-id="87761-359">Den här genomgången är avsiktligt enkla tooshow du egenskaper när du använder Blob Storage som källa eller sink eller inställningar.</span><span class="sxs-lookup"><span data-stu-id="87761-359">This walkthrough is intentionally simple tooshow you settings or properties when using Blob Storage as a source or sink.</span></span> 

### <a name="prerequisites"></a><span data-ttu-id="87761-360">Krav</span><span class="sxs-lookup"><span data-stu-id="87761-360">Prerequisites</span></span>
1. <span data-ttu-id="87761-361">Skapa en generell **Azure Storage-konto** om du inte redan har en.</span><span class="sxs-lookup"><span data-stu-id="87761-361">Create a general-purpose **Azure Storage Account** if you don't have one already.</span></span> <span data-ttu-id="87761-362">Du använder hello blob-lagring som båda **källa** och **mål** datalager i den här genomgången.</span><span class="sxs-lookup"><span data-stu-id="87761-362">You use hello blob storage as both **source** and **destination** data store in this walkthrough.</span></span> <span data-ttu-id="87761-363">Om du inte har ett Azure storage-konto finns hello [skapa ett lagringskonto](../storage/common/storage-create-storage-account.md#create-a-storage-account) toocreate i steg en artikel.</span><span class="sxs-lookup"><span data-stu-id="87761-363">if you don't have an Azure storage account, see hello [Create a storage account](../storage/common/storage-create-storage-account.md#create-a-storage-account) article for steps toocreate one.</span></span>
2. <span data-ttu-id="87761-364">Skapa en blobbbehållare med namnet **adfblobconnector** i hello storage-konto.</span><span class="sxs-lookup"><span data-stu-id="87761-364">Create a blob container named **adfblobconnector** in hello storage account.</span></span> 
4. <span data-ttu-id="87761-365">Skapa en mapp med namnet **inkommande** i hello **adfblobconnector** behållare.</span><span class="sxs-lookup"><span data-stu-id="87761-365">Create a folder named **input** in hello **adfblobconnector** container.</span></span>
5. <span data-ttu-id="87761-366">Skapa en fil med namnet **emp.txt** med hello efter innehåll och överföra den toohello **inkommande** mappen med hjälp av verktyg som [Azure Lagringsutforskaren](https://azurestorageexplorer.codeplex.com/)</span><span class="sxs-lookup"><span data-stu-id="87761-366">Create a file named **emp.txt** with hello following content and upload it toohello **input** folder by using tools such as [Azure Storage Explorer](https://azurestorageexplorer.codeplex.com/)</span></span>
    ```json
    John, Doe
    Jane, Doe
    ```
### <a name="create-hello-data-factory"></a><span data-ttu-id="87761-367">Skapa hello data factory</span><span class="sxs-lookup"><span data-stu-id="87761-367">Create hello data factory</span></span>
1. <span data-ttu-id="87761-368">Logga in toohello [Azure-portalen](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="87761-368">Sign in toohello [Azure portal](https://portal.azure.com).</span></span>
2. <span data-ttu-id="87761-369">Klicka på **+ ny** hello övre vänstra hörnet och klicka på **Intelligence + analys**, och klicka på **Data Factory**.</span><span class="sxs-lookup"><span data-stu-id="87761-369">Click **+ NEW** from hello top-left corner, click **Intelligence + analytics**, and click **Data Factory**.</span></span>
3. <span data-ttu-id="87761-370">I hello **nya data factory** bladet:</span><span class="sxs-lookup"><span data-stu-id="87761-370">In hello **New data factory** blade:</span></span>   
    1. <span data-ttu-id="87761-371">Ange **ADFBlobConnectorDF** för hello **namn**.</span><span class="sxs-lookup"><span data-stu-id="87761-371">Enter **ADFBlobConnectorDF** for hello **name**.</span></span> <span data-ttu-id="87761-372">hello namn i hello Azure data factory måste vara globalt unika.</span><span class="sxs-lookup"><span data-stu-id="87761-372">hello name of hello Azure data factory must be globally unique.</span></span> <span data-ttu-id="87761-373">Om felmeddelandet hello: `*Data factory name “ADFBlobConnectorDF” is not available`, ändra hello namn i hello data factory (till exempel yournameADFBlobConnectorDF) och försök att skapa igen.</span><span class="sxs-lookup"><span data-stu-id="87761-373">If you receive hello error: `*Data factory name “ADFBlobConnectorDF” is not available`, change hello name of hello data factory (for example, yournameADFBlobConnectorDF) and try creating again.</span></span> <span data-ttu-id="87761-374">Se artikeln [Data Factory – namnregler](data-factory-naming-rules.md) för namnregler för Data Factory-artefakter.</span><span class="sxs-lookup"><span data-stu-id="87761-374">See [Data Factory - Naming Rules](data-factory-naming-rules.md) topic for naming rules for Data Factory artifacts.</span></span>
    2. <span data-ttu-id="87761-375">Välj din Azure-**prenumeration**.</span><span class="sxs-lookup"><span data-stu-id="87761-375">Select your Azure **subscription**.</span></span>
    3. <span data-ttu-id="87761-376">Resursgrupp, Välj **Använd befintliga** tooselect en befintlig grupp (eller) Välj **Skapa nytt** tooenter ett namn för en resursgrupp.</span><span class="sxs-lookup"><span data-stu-id="87761-376">For Resource Group, select **Use existing** tooselect an existing resource group (or) select **Create new** tooenter a name for a resource group.</span></span>
    4. <span data-ttu-id="87761-377">Välj en **plats** för hello data factory.</span><span class="sxs-lookup"><span data-stu-id="87761-377">Select a **location** for hello data factory.</span></span>
    5. <span data-ttu-id="87761-378">Välj **PIN-kod toodashboard** kryssrutan längst hello hello-bladet.</span><span class="sxs-lookup"><span data-stu-id="87761-378">Select **Pin toodashboard** check box at hello bottom of hello blade.</span></span>
    6. <span data-ttu-id="87761-379">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="87761-379">Click **Create**.</span></span>
3. <span data-ttu-id="87761-380">När hello har skapats visas hello **Datafabriken** bladet som visas i följande bild hello: ![Data factory-startsida](./media/data-factory-azure-blob-connector/data-factory-home-page.png)</span><span class="sxs-lookup"><span data-stu-id="87761-380">After hello creation is complete, you see hello **Data Factory** blade as shown in hello following image: ![Data factory home page](./media/data-factory-azure-blob-connector/data-factory-home-page.png)</span></span>

### <a name="copy-wizard"></a><span data-ttu-id="87761-381">Guiden Kopiera</span><span class="sxs-lookup"><span data-stu-id="87761-381">Copy Wizard</span></span>
1. <span data-ttu-id="87761-382">Klicka på startsidan för hello Data Factory hello **kopiera data [FÖRHANDSGRANSKNING]** panelen toolaunch **guiden Kopiera** i en separat flik.</span><span class="sxs-lookup"><span data-stu-id="87761-382">On hello Data Factory home page, click hello **Copy data [PREVIEW]** tile toolaunch **Copy Data Wizard** in a separate tab.</span></span>    
    
    > [!NOTE]
    >    <span data-ttu-id="87761-383">Om du ser att hello webbläsare har ”auktorisera...”, inaktivera/avmarkera **blockerar cookies från tredje part och platsdata** inställning (eller) se till att den är aktiverad och skapa ett undantag för **login.microsoftonline.com**och försök sedan starta hello guiden igen.</span><span class="sxs-lookup"><span data-stu-id="87761-383">If you see that hello web browser is stuck at "Authorizing...", disable/uncheck **Block third-party cookies and site data** setting (or) keep it enabled and create an exception for **login.microsoftonline.com** and then try launching hello wizard again.</span></span>
2. <span data-ttu-id="87761-384">I hello **egenskaper** sidan:</span><span class="sxs-lookup"><span data-stu-id="87761-384">In hello **Properties** page:</span></span>
    1. <span data-ttu-id="87761-385">Ange **CopyPipeline** för **aktivitet**.</span><span class="sxs-lookup"><span data-stu-id="87761-385">Enter **CopyPipeline** for **Task name**.</span></span> <span data-ttu-id="87761-386">hello aktivitet är hello namn hello pipeline i din data factory.</span><span class="sxs-lookup"><span data-stu-id="87761-386">hello task name is hello name of hello pipeline in your data factory.</span></span>
    2. <span data-ttu-id="87761-387">Ange en **beskrivning** för hello aktivitet (valfritt).</span><span class="sxs-lookup"><span data-stu-id="87761-387">Enter a **description** for hello task (optional).</span></span>
    3. <span data-ttu-id="87761-388">För **aktivitet takt eller schemalägga**, hålla hello **körs regelbundet enligt schema** alternativet.</span><span class="sxs-lookup"><span data-stu-id="87761-388">For **Task cadence or Task schedule**, keep hello **Run regularly on schedule** option.</span></span> <span data-ttu-id="87761-389">Om du vill toorun aktiviteten en gång i stället för köras upprepade gånger enligt ett schema, Välj **kör nu en gång**.</span><span class="sxs-lookup"><span data-stu-id="87761-389">If you want toorun this task only once instead of run repeatedly on a schedule, select **Run once now**.</span></span> <span data-ttu-id="87761-390">Om du väljer **kör nu en gång** alternativet en [enstaka pipeline](data-factory-create-pipelines.md#onetime-pipeline) skapas.</span><span class="sxs-lookup"><span data-stu-id="87761-390">If you select, **Run once now** option, a [one-time pipeline](data-factory-create-pipelines.md#onetime-pipeline) is created.</span></span> 
    4. <span data-ttu-id="87761-391">Behålla hello-inställningar för **mönster för återkommande**.</span><span class="sxs-lookup"><span data-stu-id="87761-391">Keep hello settings for **Recurring pattern**.</span></span> <span data-ttu-id="87761-392">Den här aktiviteten körs dagligen mellan hello börja och sluta tider du anger i hello nästa steg.</span><span class="sxs-lookup"><span data-stu-id="87761-392">This task runs daily between hello start and end times you specify in hello next step.</span></span>
    5. <span data-ttu-id="87761-393">Ändra hello **startdatum tid** för**2017-04/21**.</span><span class="sxs-lookup"><span data-stu-id="87761-393">Change hello **Start date time** too**04/21/2017**.</span></span> 
    6. <span data-ttu-id="87761-394">Ändra hello **sluttiden datum** för**04/25/2017**.</span><span class="sxs-lookup"><span data-stu-id="87761-394">Change hello **End date time** too**04/25/2017**.</span></span> <span data-ttu-id="87761-395">Du kanske vill tootype hello datum i stället för att gå igenom hello kalender.</span><span class="sxs-lookup"><span data-stu-id="87761-395">You may want tootype hello date instead of browsing through hello calendar.</span></span>     
    8. <span data-ttu-id="87761-396">Klicka på **Nästa**.</span><span class="sxs-lookup"><span data-stu-id="87761-396">Click **Next**.</span></span>
      <span data-ttu-id="87761-397">![Kopiera verktyget - egenskapssidan](./media/data-factory-azure-blob-connector/copy-tool-properties-page.png)</span><span class="sxs-lookup"><span data-stu-id="87761-397">![Copy Tool - Properties page](./media/data-factory-azure-blob-connector/copy-tool-properties-page.png)</span></span> 
3. <span data-ttu-id="87761-398">På hello **källa datalagret** klickar du på **Azure Blob Storage** panelen.</span><span class="sxs-lookup"><span data-stu-id="87761-398">On hello **Source data store** page, click **Azure Blob Storage** tile.</span></span> <span data-ttu-id="87761-399">Du kan använda det här datalagret för sidan toospecify hello källa för hello kopiera aktivitet.</span><span class="sxs-lookup"><span data-stu-id="87761-399">You use this page toospecify hello source data store for hello copy task.</span></span> <span data-ttu-id="87761-400">Du kan använda en länkad tjänst för ett befintligt datalager (eller) ange ett nytt datalager.</span><span class="sxs-lookup"><span data-stu-id="87761-400">You can use an existing data store linked service (or) specify a new data store.</span></span> <span data-ttu-id="87761-401">en befintlig toouse länkade tjänsten, väljer du **från befintliga LÄNKADE tjänster** och välj hello länkade tjänst.</span><span class="sxs-lookup"><span data-stu-id="87761-401">toouse an existing linked service, you would select **FROM EXISTING LINKED SERVICES** and select hello right linked service.</span></span> 
    <span data-ttu-id="87761-402">![Kopiera verktyget - källdata lagra sidan](./media/data-factory-azure-blob-connector/copy-tool-source-data-store-page.png)</span><span class="sxs-lookup"><span data-stu-id="87761-402">![Copy Tool - Source data store page](./media/data-factory-azure-blob-connector/copy-tool-source-data-store-page.png)</span></span>
4. <span data-ttu-id="87761-403">På hello **ange hello Azure Blob storage-konto** sidan:</span><span class="sxs-lookup"><span data-stu-id="87761-403">On hello **Specify hello Azure Blob storage account** page:</span></span>
   1. <span data-ttu-id="87761-404">Behåll hello automatiskt genererade namnet för **anslutningsnamn**.</span><span class="sxs-lookup"><span data-stu-id="87761-404">Keep hello auto-generated name for **Connection name**.</span></span> <span data-ttu-id="87761-405">hello anslutning är hello namn på hello länkade tjänsten av typen: Azure Storage.</span><span class="sxs-lookup"><span data-stu-id="87761-405">hello connection name is hello name of hello linked service of type: Azure Storage.</span></span> 
   2. <span data-ttu-id="87761-406">Kontrollera att alternativet **Från Azure-prenumerationer** har valts för **Metod för kontoval**.</span><span class="sxs-lookup"><span data-stu-id="87761-406">Confirm that **From Azure subscriptions** option is selected for **Account selection method**.</span></span>
   3. <span data-ttu-id="87761-407">Välj din Azure-prenumeration eller behålla **Markera alla** för **Azure-prenumeration**.</span><span class="sxs-lookup"><span data-stu-id="87761-407">Select your Azure subscription or keep **Select all** for **Azure subscription**.</span></span>   
   4. <span data-ttu-id="87761-408">Välj en **Azure storage-konto** från hello lista över Azure storage-konton tillgängliga i hello valda prenumerationen.</span><span class="sxs-lookup"><span data-stu-id="87761-408">Select an **Azure storage account** from hello list of Azure storage accounts available in hello selected subscription.</span></span> <span data-ttu-id="87761-409">Du kan också välja tooenter lagringsutrymmet kontoinställningar manuellt genom att välja **ange manuellt** alternativ för hello **konto urvalsmetod**.</span><span class="sxs-lookup"><span data-stu-id="87761-409">You can also choose tooenter storage account settings manually by selecting **Enter manually** option for hello **Account selection method**.</span></span>
   5. <span data-ttu-id="87761-410">Klicka på **Nästa**.</span><span class="sxs-lookup"><span data-stu-id="87761-410">Click **Next**.</span></span> 
      <span data-ttu-id="87761-411">![Kopiera verktyget - Ange hello Azure Blob storage-konto](./media/data-factory-azure-blob-connector/copy-tool-specify-azure-blob-storage-account.png)</span><span class="sxs-lookup"><span data-stu-id="87761-411">![Copy Tool - Specify hello Azure Blob storage account](./media/data-factory-azure-blob-connector/copy-tool-specify-azure-blob-storage-account.png)</span></span>
5. <span data-ttu-id="87761-412">På **Välj hello inkommande fil eller mapp** sidan:</span><span class="sxs-lookup"><span data-stu-id="87761-412">On **Choose hello input file or folder** page:</span></span>
   1. <span data-ttu-id="87761-413">Dubbelklicka på **adfblobcontainer**.</span><span class="sxs-lookup"><span data-stu-id="87761-413">Double-click **adfblobcontainer**.</span></span>
   2. <span data-ttu-id="87761-414">Välj **inkommande**, och klicka på **Välj**.</span><span class="sxs-lookup"><span data-stu-id="87761-414">Select **input**, and click **Choose**.</span></span> <span data-ttu-id="87761-415">I den här genomgången kan du välja hello inkommande mapp.</span><span class="sxs-lookup"><span data-stu-id="87761-415">In this walkthrough, you select hello input folder.</span></span> <span data-ttu-id="87761-416">Du kan också markera hello emp.txt filen i mappen hello i stället.</span><span class="sxs-lookup"><span data-stu-id="87761-416">You could also select hello emp.txt file in hello folder instead.</span></span> 
      <span data-ttu-id="87761-417">![Kopiera verktyget – Välj hello inkommande fil eller mapp](./media/data-factory-azure-blob-connector/copy-tool-choose-input-file-or-folder.png)</span><span class="sxs-lookup"><span data-stu-id="87761-417">![Copy Tool - Choose hello input file or folder](./media/data-factory-azure-blob-connector/copy-tool-choose-input-file-or-folder.png)</span></span>
6. <span data-ttu-id="87761-418">På hello **Välj hello inkommande fil eller mapp** sidan:</span><span class="sxs-lookup"><span data-stu-id="87761-418">On hello **Choose hello input file or folder** page:</span></span>
    1. <span data-ttu-id="87761-419">Bekräfta att hello **filen eller mappen** har angetts för**adfblobconnector/indata**.</span><span class="sxs-lookup"><span data-stu-id="87761-419">Confirm that hello **file or folder** is set too**adfblobconnector/input**.</span></span> <span data-ttu-id="87761-420">Om hello filer i undermappar, till exempel 2017-04-01, 02-04-2017 och så vidare, ange adfblobconnector/indata / {year} / {month} / {day} för filen eller mappen.</span><span class="sxs-lookup"><span data-stu-id="87761-420">If hello files are in sub folders, for example, 2017/04/01, 2017/04/02, and so on, enter adfblobconnector/input/{year}/{month}/{day} for file or folder.</span></span> <span data-ttu-id="87761-421">När du trycker på FLIKEN utanför hello textruta finns tre listrutorna tooselect format för år (åååå), månad (MM) och dag (dd).</span><span class="sxs-lookup"><span data-stu-id="87761-421">When you press TAB out of hello text box, you see three drop-down lists tooselect formats for year (yyyy), month (MM), and day (dd).</span></span> 
    2. <span data-ttu-id="87761-422">Ange inte **kopiera filen rekursivt**.</span><span class="sxs-lookup"><span data-stu-id="87761-422">Do not set **Copy file recursively**.</span></span> <span data-ttu-id="87761-423">Välj det här alternativet toorecursively Bläddra igenom mappar för filer toobe kopierade toohello mål.</span><span class="sxs-lookup"><span data-stu-id="87761-423">Select this option toorecursively traverse through folders for files toobe copied toohello destination.</span></span> 
    3. <span data-ttu-id="87761-424">Inte hello **binära kopiera** alternativet.</span><span class="sxs-lookup"><span data-stu-id="87761-424">Do not hello **binary copy** option.</span></span> <span data-ttu-id="87761-425">Välj det här alternativet tooperform en binär kopia av källdatorn filen toohello mål.</span><span class="sxs-lookup"><span data-stu-id="87761-425">Select this option tooperform a binary copy of source file toohello destination.</span></span> <span data-ttu-id="87761-426">Markera inte den här genomgången så att du kan se fler alternativ i hello nästa sida.</span><span class="sxs-lookup"><span data-stu-id="87761-426">Do not select for this walkthrough so that you can see more options in hello next pages.</span></span> 
    4. <span data-ttu-id="87761-427">Bekräfta att hello **Komprimeringstypen** har angetts för**ingen**.</span><span class="sxs-lookup"><span data-stu-id="87761-427">Confirm that hello **Compression type** is set too**None**.</span></span> <span data-ttu-id="87761-428">Välj ett värde för det här alternativet om källfilerna komprimeras i något av formaten hello stöds.</span><span class="sxs-lookup"><span data-stu-id="87761-428">Select a value for this option if your source files are compressed in one of hello supported formats.</span></span> 
    5. <span data-ttu-id="87761-429">Klicka på **Nästa**.</span><span class="sxs-lookup"><span data-stu-id="87761-429">Click **Next**.</span></span>
    <span data-ttu-id="87761-430">![Kopiera verktyget – Välj hello inkommande fil eller mapp](./media/data-factory-azure-blob-connector/chose-input-file-folder.png)</span><span class="sxs-lookup"><span data-stu-id="87761-430">![Copy Tool - Choose hello input file or folder](./media/data-factory-azure-blob-connector/chose-input-file-folder.png)</span></span> 
7. <span data-ttu-id="87761-431">På hello **filen formatinställningar** kan du se hello avgränsare och hello schemat som är detekterade hello guiden med hello fil-parsning.</span><span class="sxs-lookup"><span data-stu-id="87761-431">On hello **File format settings** page, you see hello delimiters and hello schema that is auto-detected by hello wizard by parsing hello file.</span></span> 
    1. <span data-ttu-id="87761-432">Bekräfta hello följande alternativ: en.</span><span class="sxs-lookup"><span data-stu-id="87761-432">Confirm hello following options: a.</span></span> <span data-ttu-id="87761-433">Hej **filformatet** har angetts för**textformat**.</span><span class="sxs-lookup"><span data-stu-id="87761-433">hello **file format** is set too**Text format**.</span></span> <span data-ttu-id="87761-434">Du kan se alla hello stöds formaten i hello nedrullningsbara listan.</span><span class="sxs-lookup"><span data-stu-id="87761-434">You can see all hello supported formats in hello drop-down list.</span></span> <span data-ttu-id="87761-435">Till exempel: JSON, Avro, ORC, parkettgolv.</span><span class="sxs-lookup"><span data-stu-id="87761-435">For example: JSON, Avro, ORC, Parquet.</span></span>
        <span data-ttu-id="87761-436">b.</span><span class="sxs-lookup"><span data-stu-id="87761-436">b.</span></span> <span data-ttu-id="87761-437">Hej **kolumnen avgränsare** har angetts för`Comma (,)`.</span><span class="sxs-lookup"><span data-stu-id="87761-437">hello **column delimiter** is set too`Comma (,)`.</span></span> <span data-ttu-id="87761-438">Du kan se hello andra kolumn avgränsare som stöds av Data Factory i hello nedrullningsbara listan.</span><span class="sxs-lookup"><span data-stu-id="87761-438">You can see hello other column delimiters supported by Data Factory in hello drop-down list.</span></span> <span data-ttu-id="87761-439">Du kan även ange anpassade avgränsare.</span><span class="sxs-lookup"><span data-stu-id="87761-439">You can also specify a custom delimiter.</span></span>
        <span data-ttu-id="87761-440">c.</span><span class="sxs-lookup"><span data-stu-id="87761-440">c.</span></span> <span data-ttu-id="87761-441">Hej **raden avgränsare** har angetts för`Carriage Return + Line feed (\r\n)`.</span><span class="sxs-lookup"><span data-stu-id="87761-441">hello **row delimiter** is set too`Carriage Return + Line feed (\r\n)`.</span></span> <span data-ttu-id="87761-442">Du kan se hello andra raden avgränsare som stöds av Data Factory i hello nedrullningsbara listan.</span><span class="sxs-lookup"><span data-stu-id="87761-442">You can see hello other row delimiters supported by Data Factory in hello drop-down list.</span></span> <span data-ttu-id="87761-443">Du kan även ange anpassade avgränsare.</span><span class="sxs-lookup"><span data-stu-id="87761-443">You can also specify a custom delimiter.</span></span>
        <span data-ttu-id="87761-444">d.</span><span class="sxs-lookup"><span data-stu-id="87761-444">d.</span></span> <span data-ttu-id="87761-445">Hej **hoppa över radnummer** har angetts för**0**.</span><span class="sxs-lookup"><span data-stu-id="87761-445">hello **skip line count** is set too**0**.</span></span> <span data-ttu-id="87761-446">Om du vill att några rader toobe hoppas över hello överst i filen hello nummer hello här.</span><span class="sxs-lookup"><span data-stu-id="87761-446">If you want a few lines toobe skipped at hello top of hello file, enter hello number here.</span></span>
        <span data-ttu-id="87761-447">e.</span><span class="sxs-lookup"><span data-stu-id="87761-447">e.</span></span>  <span data-ttu-id="87761-448">Hej **första dataraden innehåller kolumnnamn** har inte angetts.</span><span class="sxs-lookup"><span data-stu-id="87761-448">hello **first data row contains column names** is not set.</span></span> <span data-ttu-id="87761-449">Om hello källfiler innehåller kolumnnamnen i hello första raden, väljer du det här alternativet.</span><span class="sxs-lookup"><span data-stu-id="87761-449">If hello source files contain column names in hello first row, select this option.</span></span>
        <span data-ttu-id="87761-450">f.</span><span class="sxs-lookup"><span data-stu-id="87761-450">f.</span></span> <span data-ttu-id="87761-451">Hej **behandlar tomma kolumnvärde som null** alternativet anges.</span><span class="sxs-lookup"><span data-stu-id="87761-451">hello **treat empty column value as null** option is set.</span></span>
    2. <span data-ttu-id="87761-452">Expandera **avancerade inställningar** toosee avancerade alternativ som är tillgänglig.</span><span class="sxs-lookup"><span data-stu-id="87761-452">Expand **Advanced settings** toosee advanced option available.</span></span>
    3. <span data-ttu-id="87761-453">Se hello hello längst ned på sidan hello i **preview** av data från hello emp.txt fil.</span><span class="sxs-lookup"><span data-stu-id="87761-453">At hello bottom of hello page, see hello **preview** of data from hello emp.txt file.</span></span>
    4. <span data-ttu-id="87761-454">Klicka på **schemat** fliken på hello nedre toosee hello schemat hello kopiera guiden härledas genom att titta på hello data i hello källfil.</span><span class="sxs-lookup"><span data-stu-id="87761-454">Click **SCHEMA** tab at hello bottom toosee hello schema that hello copy wizard inferred by looking at hello data in hello source file.</span></span>
    5. <span data-ttu-id="87761-455">Klicka på **nästa** när du granskar hello avgränsare och förhandsgranska data.</span><span class="sxs-lookup"><span data-stu-id="87761-455">Click **Next** after you review hello delimiters and preview data.</span></span>
    <span data-ttu-id="87761-456">![Kopiera verktyget - inställningar för format](./media/data-factory-azure-blob-connector/copy-tool-file-format-settings.png)</span><span class="sxs-lookup"><span data-stu-id="87761-456">![Copy Tool - File format settings](./media/data-factory-azure-blob-connector/copy-tool-file-format-settings.png)</span></span>  
8. <span data-ttu-id="87761-457">På hello **mål datalager sidan**väljer **Azure Blob Storage**, och klicka på **nästa**.</span><span class="sxs-lookup"><span data-stu-id="87761-457">On hello **Destination data store page**, select **Azure Blob Storage**, and click **Next**.</span></span> <span data-ttu-id="87761-458">Du använder hello Azure Blob Storage som båda hello käll- och datalager i den här genomgången.</span><span class="sxs-lookup"><span data-stu-id="87761-458">You are using hello Azure Blob Storage as both hello source and destination data stores in this walkthrough.</span></span>    
    <span data-ttu-id="87761-459">![Kopiera verktyget - Välj målserver datalager](media/data-factory-azure-blob-connector/select-destination-data-store.png)</span><span class="sxs-lookup"><span data-stu-id="87761-459">![Copy Tool - select destination data store](media/data-factory-azure-blob-connector/select-destination-data-store.png)</span></span>
9. <span data-ttu-id="87761-460">På **ange hello Azure Blob storage-konto** sidan:</span><span class="sxs-lookup"><span data-stu-id="87761-460">On **Specify hello Azure Blob storage account** page:</span></span>
   1. <span data-ttu-id="87761-461">Ange **AzureStorageLinkedService** för hello **anslutningsnamn** fältet.</span><span class="sxs-lookup"><span data-stu-id="87761-461">Enter **AzureStorageLinkedService** for hello **Connection name** field.</span></span>
   2. <span data-ttu-id="87761-462">Kontrollera att alternativet **Från Azure-prenumerationer** har valts för **Metod för kontoval**.</span><span class="sxs-lookup"><span data-stu-id="87761-462">Confirm that **From Azure subscriptions** option is selected for **Account selection method**.</span></span>
   3. <span data-ttu-id="87761-463">Välj din Azure-**prenumeration**.</span><span class="sxs-lookup"><span data-stu-id="87761-463">Select your Azure **subscription**.</span></span>  
   4. <span data-ttu-id="87761-464">Välj Azure storage-konto.</span><span class="sxs-lookup"><span data-stu-id="87761-464">Select your Azure storage account.</span></span> 
   5. <span data-ttu-id="87761-465">Klicka på **Nästa**.</span><span class="sxs-lookup"><span data-stu-id="87761-465">Click **Next**.</span></span>     
10. <span data-ttu-id="87761-466">På hello **Välj hello utdata filen eller mappen** sidan:</span><span class="sxs-lookup"><span data-stu-id="87761-466">On hello **Choose hello output file or folder** page:</span></span> 
    6. <span data-ttu-id="87761-467">Ange **mappsökväg** som **adfblobconnector-/ utdata / {year} / {month} / {day}**.</span><span class="sxs-lookup"><span data-stu-id="87761-467">specify **Folder path** as **adfblobconnector/output/{year}/{month}/{day}**.</span></span> <span data-ttu-id="87761-468">Ange **FLIKEN**.</span><span class="sxs-lookup"><span data-stu-id="87761-468">Enter **TAB**.</span></span>
    7. <span data-ttu-id="87761-469">För hello **år**väljer **åååå**.</span><span class="sxs-lookup"><span data-stu-id="87761-469">For hello **year**, select **yyyy**.</span></span>
    8. <span data-ttu-id="87761-470">För hello **månad**, bekräfta att den har angetts för**MM**.</span><span class="sxs-lookup"><span data-stu-id="87761-470">For hello **month**, confirm that it is set too**MM**.</span></span>
    9. <span data-ttu-id="87761-471">För hello **dag**, bekräfta att den har angetts för**dd**.</span><span class="sxs-lookup"><span data-stu-id="87761-471">For hello **day**, confirm that it is set too**dd**.</span></span>
    10. <span data-ttu-id="87761-472">Bekräfta att hello **Komprimeringstypen** har angetts för**ingen**.</span><span class="sxs-lookup"><span data-stu-id="87761-472">Confirm that hello **compression type** is set too**None**.</span></span>
    11. <span data-ttu-id="87761-473">Bekräfta att hello **kopiera beteende** har angetts för**Sammanfoga filer**.</span><span class="sxs-lookup"><span data-stu-id="87761-473">Confirm that hello **copy behavior** is set too**Merge files**.</span></span> <span data-ttu-id="87761-474">Om hello utdatafilen med samma namn finns redan hello, är hello nytt innehåll tillagda toohello samma fil hello slutet.</span><span class="sxs-lookup"><span data-stu-id="87761-474">If hello output file with hello same name already exists, hello new content is added toohello same file at hello end.</span></span>
    12. <span data-ttu-id="87761-475">Klicka på **Nästa**.</span><span class="sxs-lookup"><span data-stu-id="87761-475">Click **Next**.</span></span>
    <span data-ttu-id="87761-476">![Kopiera verktyget – Välj filen eller mappen](media/data-factory-azure-blob-connector/choose-the-output-file-or-folder.png)</span><span class="sxs-lookup"><span data-stu-id="87761-476">![Copy Tool - Choose output file or folder](media/data-factory-azure-blob-connector/choose-the-output-file-or-folder.png)</span></span>
11. <span data-ttu-id="87761-477">På hello **filen formatinställningar** , granska hello inställningarna och klicka på **nästa**.</span><span class="sxs-lookup"><span data-stu-id="87761-477">On hello **File format settings** page, review hello settings, and click **Next**.</span></span> <span data-ttu-id="87761-478">En av hello här ytterligare alternativ är tooadd ett sidhuvud toohello utdatafilen.</span><span class="sxs-lookup"><span data-stu-id="87761-478">One of hello additional options here is tooadd a header toohello output file.</span></span> <span data-ttu-id="87761-479">Om du väljer det alternativet läggs en rubrikrad med namnen på hello kolumner från hello schemat för hello källa.</span><span class="sxs-lookup"><span data-stu-id="87761-479">If you select that option, a header row is added with names of hello columns from hello schema of hello source.</span></span> <span data-ttu-id="87761-480">Du kan byta namn på hello standardkolumnvärdena när du visar hello schemat för hello källa.</span><span class="sxs-lookup"><span data-stu-id="87761-480">You can rename hello default column names when viewing hello schema for hello source.</span></span> <span data-ttu-id="87761-481">Du kan till exempel ändra hello första kolumnen tooFirst namn och den andra kolumnen tooLast namn.</span><span class="sxs-lookup"><span data-stu-id="87761-481">For example, you could change hello first column tooFirst Name and second column tooLast Name.</span></span> <span data-ttu-id="87761-482">Hello utdatafilen genereras sedan med ett huvud med dessa namn som kolumnnamn.</span><span class="sxs-lookup"><span data-stu-id="87761-482">Then, hello output file is generated with a header with these names as column names.</span></span> 
    <span data-ttu-id="87761-483">![Kopiera verktyget - format-inställningarna för mål](media/data-factory-azure-blob-connector/file-format-destination.png)</span><span class="sxs-lookup"><span data-stu-id="87761-483">![Copy Tool - File format settings for destination](media/data-factory-azure-blob-connector/file-format-destination.png)</span></span>
12. <span data-ttu-id="87761-484">På hello **prestandainställningar** bekräftar som **molnet enheter** och **parallell kopior** har angetts för**automatisk**, och klicka på Nästa.</span><span class="sxs-lookup"><span data-stu-id="87761-484">On hello **Performance settings** page, confirm that **cloud units** and **parallel copies** are set too**Auto**, and click Next.</span></span> <span data-ttu-id="87761-485">Mer information om dessa inställningar finns [kopiera aktivitet prestanda och prestandajustering guiden](data-factory-copy-activity-performance.md#parallel-copy).</span><span class="sxs-lookup"><span data-stu-id="87761-485">For details about these settings, see [Copy activity performance and tuning guide](data-factory-copy-activity-performance.md#parallel-copy).</span></span>
    <span data-ttu-id="87761-486">![Kopiera verktyget - prestandainställningar](media/data-factory-azure-blob-connector/copy-performance-settings.png)</span><span class="sxs-lookup"><span data-stu-id="87761-486">![Copy Tool - Performance settings](media/data-factory-azure-blob-connector/copy-performance-settings.png)</span></span> 
14. <span data-ttu-id="87761-487">På hello **sammanfattning** , granska alla inställningar (Aktivitetsegenskaper, inställningar för källa och mål och inställningar) och klicka på **nästa**.</span><span class="sxs-lookup"><span data-stu-id="87761-487">On hello **Summary** page, review all settings (task properties, settings for source and destination, and copy settings), and click **Next**.</span></span>
    <span data-ttu-id="87761-488">![Kopiera verktyget - sammanfattningssida](media/data-factory-azure-blob-connector/copy-tool-summary-page.png)</span><span class="sxs-lookup"><span data-stu-id="87761-488">![Copy Tool - Summary page](media/data-factory-azure-blob-connector/copy-tool-summary-page.png)</span></span>
15. <span data-ttu-id="87761-489">Granska informationen i hello **sammanfattning** och klicka på **Slutför**.</span><span class="sxs-lookup"><span data-stu-id="87761-489">Review information in hello **Summary** page, and click **Finish**.</span></span> <span data-ttu-id="87761-490">hello skapas två länkade tjänster, två datamängder (indata och utdata) och en pipeline i hello data factory (från där startas hello guiden Kopiera).</span><span class="sxs-lookup"><span data-stu-id="87761-490">hello wizard creates two linked services, two datasets (input and output), and one pipeline in hello data factory (from where you launched hello Copy Wizard).</span></span>
    <span data-ttu-id="87761-491">![Kopiera verktyget - distribution sida](media/data-factory-azure-blob-connector/copy-tool-deployment-page.png)</span><span class="sxs-lookup"><span data-stu-id="87761-491">![Copy Tool - Deployment page](media/data-factory-azure-blob-connector/copy-tool-deployment-page.png)</span></span>

### <a name="monitor-hello-pipeline-copy-task"></a><span data-ttu-id="87761-492">Övervaka hello pipeline (kopiera aktivitet)</span><span class="sxs-lookup"><span data-stu-id="87761-492">Monitor hello pipeline (copy task)</span></span>

1. <span data-ttu-id="87761-493">Klicka på länken hello `Click here toomonitor copy pipeline` på hello **distribution** sidan.</span><span class="sxs-lookup"><span data-stu-id="87761-493">Click hello link `Click here toomonitor copy pipeline` on hello **Deployment** page.</span></span> 
2. <span data-ttu-id="87761-494">Du bör se hello **övervaka och hantera program** i en separat flik.  ![Övervaka och hantera appen](media/data-factory-azure-blob-connector/monitor-manage-app.png)</span><span class="sxs-lookup"><span data-stu-id="87761-494">You should see hello **Monitor and Manage application** in a separate tab.  ![Monitor and Manage App](media/data-factory-azure-blob-connector/monitor-manage-app.png)</span></span>
3. <span data-ttu-id="87761-495">Ändra hello **starta** tid hello överst för`04/19/2017` och **end** tid för`04/27/2017`, och klicka sedan på **tillämpa**.</span><span class="sxs-lookup"><span data-stu-id="87761-495">Change hello **start** time at hello top too`04/19/2017` and **end** time too`04/27/2017`, and then click **Apply**.</span></span> 
4. <span data-ttu-id="87761-496">Du bör se fem aktivitet windows hello **aktivitet WINDOWS** lista.</span><span class="sxs-lookup"><span data-stu-id="87761-496">You should see five activity windows in hello **ACTIVITY WINDOWS** list.</span></span> <span data-ttu-id="87761-497">Hej **WindowStart** gånger bör omfatta alla dagar från pipeline starttiderna toopipeline slut.</span><span class="sxs-lookup"><span data-stu-id="87761-497">hello **WindowStart** times should cover all days from pipeline start toopipeline end times.</span></span> 
5. <span data-ttu-id="87761-498">Klicka på **uppdatera** knapp för hello **aktivitet WINDOWS** några gånger tills du ser alla hello aktivitet windows hello status är inställd tooReady.</span><span class="sxs-lookup"><span data-stu-id="87761-498">Click **Refresh** button for hello **ACTIVITY WINDOWS** list a few times until you see hello status of all hello activity windows is set tooReady.</span></span> 
6. <span data-ttu-id="87761-499">Nu, kontrollera att hello utdatafilerna genereras i hello utdatamapp för adfblobconnector behållare.</span><span class="sxs-lookup"><span data-stu-id="87761-499">Now, verify that hello output files are generated in hello output folder of adfblobconnector container.</span></span> <span data-ttu-id="87761-500">Du bör se följande mappstrukturen i hello utdatamapp hello:</span><span class="sxs-lookup"><span data-stu-id="87761-500">You should see hello following folder structure in hello output folder:</span></span> 
    ```
    2017/04/21
    2017/04/22
    2017/04/23
    2017/04/24
    2017/04/25    
    ```
<span data-ttu-id="87761-501">Detaljerad information om övervakning och hantering av datafabriker finns [övervaka och hantera Data Factory-pipelinen](data-factory-monitor-manage-app.md) artikel.</span><span class="sxs-lookup"><span data-stu-id="87761-501">For detailed information about monitoring and managing data factories, see [Monitor and manage Data Factory pipeline](data-factory-monitor-manage-app.md) article.</span></span> 
 
### <a name="data-factory-entities"></a><span data-ttu-id="87761-502">Data Factory entiteter</span><span class="sxs-lookup"><span data-stu-id="87761-502">Data Factory entities</span></span>
<span data-ttu-id="87761-503">Växla tillbaka toohello flik med hello Data Factory-startsidan.</span><span class="sxs-lookup"><span data-stu-id="87761-503">Now, switch back toohello tab with hello Data Factory home page.</span></span> <span data-ttu-id="87761-504">Observera att det finns två länkade tjänster, två datamängder och en pipeline i din data factory nu.</span><span class="sxs-lookup"><span data-stu-id="87761-504">Notice that there are two linked services, two datasets, and one pipeline in your data factory now.</span></span> 

![Startsida för data Factory med entiteter](media/data-factory-azure-blob-connector/data-factory-home-page-with-numbers.png)

<span data-ttu-id="87761-506">Klicka på **författare och distribuera** toolaunch Data Factory-redigeraren.</span><span class="sxs-lookup"><span data-stu-id="87761-506">Click **Author and deploy** toolaunch Data Factory Editor.</span></span> 

![Data Factory-redigeraren](media/data-factory-azure-blob-connector/data-factory-editor.png)

<span data-ttu-id="87761-508">Du bör se följande Data Factory-enheter i din data factory hello:</span><span class="sxs-lookup"><span data-stu-id="87761-508">You should see hello following Data Factory entities in your data factory:</span></span> 

 - <span data-ttu-id="87761-509">Två länkade tjänster.</span><span class="sxs-lookup"><span data-stu-id="87761-509">Two linked services.</span></span> <span data-ttu-id="87761-510">En för hello käll- och hello andra för hello mål.</span><span class="sxs-lookup"><span data-stu-id="87761-510">One for hello source and hello other one for hello destination.</span></span> <span data-ttu-id="87761-511">Båda hello länkade tjänster finns toohello samma Azure Storage-konto i den här genomgången.</span><span class="sxs-lookup"><span data-stu-id="87761-511">Both hello linked services refer toohello same Azure Storage account in this walkthrough.</span></span> 
 - <span data-ttu-id="87761-512">Två datauppsättningar.</span><span class="sxs-lookup"><span data-stu-id="87761-512">Two datasets.</span></span> <span data-ttu-id="87761-513">En inkommande datauppsättning och en datamängd för utdata.</span><span class="sxs-lookup"><span data-stu-id="87761-513">An input dataset and an output dataset.</span></span> <span data-ttu-id="87761-514">I den här genomgången använder båda hello samma blobbehållaren men finns toodifferent mappar (indata och utdata).</span><span class="sxs-lookup"><span data-stu-id="87761-514">In this walkthrough, both use hello same blob container but refer toodifferent folders (input and output).</span></span>
 - <span data-ttu-id="87761-515">En pipeline.</span><span class="sxs-lookup"><span data-stu-id="87761-515">A pipeline.</span></span> <span data-ttu-id="87761-516">hello pipelinen innehåller en kopia-aktivitet som använder en blob-källa och en mottagare toocopy blobbdata från ett Azure blob plats tooanother Azure blob-plats.</span><span class="sxs-lookup"><span data-stu-id="87761-516">hello pipeline contains a copy activity that uses a blob source and a blob sink toocopy data from an Azure blob location tooanother Azure blob location.</span></span> 

<span data-ttu-id="87761-517">hello innehåller följande avsnitt mer information om dessa enheter.</span><span class="sxs-lookup"><span data-stu-id="87761-517">hello following sections provide more information about these entities.</span></span> 

#### <a name="linked-services"></a><span data-ttu-id="87761-518">Länkade tjänster</span><span class="sxs-lookup"><span data-stu-id="87761-518">Linked services</span></span>
<span data-ttu-id="87761-519">Du bör se två länkade tjänster.</span><span class="sxs-lookup"><span data-stu-id="87761-519">You should see two linked services.</span></span> <span data-ttu-id="87761-520">En för hello käll- och hello andra för hello mål.</span><span class="sxs-lookup"><span data-stu-id="87761-520">One for hello source and hello other one for hello destination.</span></span> <span data-ttu-id="87761-521">I den här genomgången hello båda definitioner utseende samma förutom hello namn.</span><span class="sxs-lookup"><span data-stu-id="87761-521">In this walkthrough, both definitions look hello same except for hello names.</span></span> <span data-ttu-id="87761-522">Hej **typen** av hello länkade tjänsten är inställd för**AzureStorage**.</span><span class="sxs-lookup"><span data-stu-id="87761-522">hello **type** of hello linked service is set too**AzureStorage**.</span></span> <span data-ttu-id="87761-523">De viktigaste egenskapen hello länkade service definition är hello **connectionString**, som används av Data Factory tooconnect tooyour Azure Storage-konto vid körning.</span><span class="sxs-lookup"><span data-stu-id="87761-523">Most important property of hello linked service definition is hello **connectionString**, which is used by Data Factory tooconnect tooyour Azure Storage account at runtime.</span></span> <span data-ttu-id="87761-524">Ignorera hello hubName egenskap i hello definition.</span><span class="sxs-lookup"><span data-stu-id="87761-524">Ignore hello hubName property in hello definition.</span></span> 

##### <a name="source-blob-storage-linked-service"></a><span data-ttu-id="87761-525">Källan blob länkade lagringstjänsten</span><span class="sxs-lookup"><span data-stu-id="87761-525">Source blob storage linked service</span></span>
```json
{
    "name": "Source-BlobStorage-z4y",
    "properties": {
        "type": "AzureStorage",
        "typeProperties": {
            "connectionString": "DefaultEndpointsProtocol=https;AccountName=mystorageaccount;AccountKey=**********"
        }
    }
}
```

##### <a name="destination-blob-storage-linked-service"></a><span data-ttu-id="87761-526">Mål-blob länkade lagringstjänsten</span><span class="sxs-lookup"><span data-stu-id="87761-526">Destination blob storage linked service</span></span>

```json
{
    "name": "Destination-BlobStorage-z4y",
    "properties": {
        "type": "AzureStorage",
        "typeProperties": {
            "connectionString": "DefaultEndpointsProtocol=https;AccountName=mystorageaccount;AccountKey=**********"
        }
    }
}
```

<span data-ttu-id="87761-527">Mer information om länkad Azure Storage-tjänst finns [länkade tjänstegenskaper](#linked-service-properties) avsnitt.</span><span class="sxs-lookup"><span data-stu-id="87761-527">For more information about Azure Storage linked service, see [Linked service properties](#linked-service-properties) section.</span></span> 

#### <a name="datasets"></a><span data-ttu-id="87761-528">Datauppsättningar</span><span class="sxs-lookup"><span data-stu-id="87761-528">Datasets</span></span>
<span data-ttu-id="87761-529">Det finns två datamängder: en inkommande datauppsättning och en datamängd för utdata.</span><span class="sxs-lookup"><span data-stu-id="87761-529">There are two datasets: an input dataset and an output dataset.</span></span> <span data-ttu-id="87761-530">hello typ av hello datamängden har angetts för**AzureBlob** för båda.</span><span class="sxs-lookup"><span data-stu-id="87761-530">hello type of hello dataset is set too**AzureBlob** for both.</span></span> 

<span data-ttu-id="87761-531">hello inkommande dataset pekar toohello **inkommande** för hello **adfblobconnector** blob-behållare.</span><span class="sxs-lookup"><span data-stu-id="87761-531">hello input dataset points toohello **input** folder of hello **adfblobconnector** blob container.</span></span> <span data-ttu-id="87761-532">Hej **externa** egenskapen för**SANT** för denna dataset som hello data inte tillverkas av hello pipeline med hello kopieringsaktiviteten som äger denna dataset som indata.</span><span class="sxs-lookup"><span data-stu-id="87761-532">hello **external** property is set too**true** for this dataset as hello data is not produced by hello pipeline with hello copy activity that takes this dataset as an input.</span></span> 

<span data-ttu-id="87761-533">Hej utdata dataset pekar toohello **utdata** för hello samma blob-behållare.</span><span class="sxs-lookup"><span data-stu-id="87761-533">hello output dataset points toohello **output** folder of hello same blob container.</span></span> <span data-ttu-id="87761-534">Hej utdatauppsättningen använder också hello år, månad och dag av hello **SliceStart** system variabeln toodynamically utvärdera hello sökvägen för hello utdatafilen.</span><span class="sxs-lookup"><span data-stu-id="87761-534">hello output dataset also uses hello year, month, and day of hello **SliceStart** system variable toodynamically evaluate hello path for hello output file.</span></span> <span data-ttu-id="87761-535">En lista över funktioner och systemvariabler som stöds av Data Factory finns [Data Factory-funktioner och systemvariabler](data-factory-functions-variables.md).</span><span class="sxs-lookup"><span data-stu-id="87761-535">For a list of functions and system variables supported by Data Factory, see [Data Factory functions and system variables](data-factory-functions-variables.md).</span></span> <span data-ttu-id="87761-536">Hej **externa** egenskapen för**FALSKT** (standardvärdet) eftersom den här datauppsättningen produceras av hello pipeline.</span><span class="sxs-lookup"><span data-stu-id="87761-536">hello **external** property is set too**false** (default value) because this dataset is produced by hello pipeline.</span></span> 

<span data-ttu-id="87761-537">Mer information om egenskaper som stöds av Azure-blobbdatauppsättning finns [egenskaper för datamängd](#dataset-properties) avsnitt.</span><span class="sxs-lookup"><span data-stu-id="87761-537">For more information about properties supported by Azure Blob dataset, see [Dataset properties](#dataset-properties) section.</span></span>

##### <a name="input-dataset"></a><span data-ttu-id="87761-538">Indatauppsättning</span><span class="sxs-lookup"><span data-stu-id="87761-538">Input dataset</span></span>

```json
{
    "name": "InputDataset-z4y",
    "properties": {
        "structure": [
            { "name": "Prop_0", "type": "String" },
            { "name": "Prop_1", "type": "String" }
        ],
        "type": "AzureBlob",
        "linkedServiceName": "Source-BlobStorage-z4y",
        "typeProperties": {
            "folderPath": "adfblobconnector/input/",
            "format": {
                "type": "TextFormat",
                "columnDelimiter": ","
            }
        },
        "availability": {
            "frequency": "Day",
            "interval": 1
        },
        "external": true,
        "policy": {}
    }
}
```

##### <a name="output-dataset"></a><span data-ttu-id="87761-539">Datamängd för utdata</span><span class="sxs-lookup"><span data-stu-id="87761-539">Output dataset</span></span>

```json
{
    "name": "OutputDataset-z4y",
    "properties": {
        "structure": [
            { "name": "Prop_0", "type": "String" },
            { "name": "Prop_1", "type": "String" }
        ],
        "type": "AzureBlob",
        "linkedServiceName": "Destination-BlobStorage-z4y",
        "typeProperties": {
            "folderPath": "adfblobconnector/output/{year}/{month}/{day}",
            "format": {
                "type": "TextFormat",
                "columnDelimiter": ","
            },
            "partitionedBy": [
                { "name": "year", "value": { "type": "DateTime", "date": "SliceStart", "format": "yyyy" } },
                { "name": "month", "value": { "type": "DateTime", "date": "SliceStart", "format": "MM" } },
                { "name": "day", "value": { "type": "DateTime", "date": "SliceStart", "format": "dd" } }
            ]
        },
        "availability": {
            "frequency": "Day",
            "interval": 1
        },
        "external": false,
        "policy": {}
    }
}
```

#### <a name="pipeline"></a><span data-ttu-id="87761-540">Pipeline</span><span class="sxs-lookup"><span data-stu-id="87761-540">Pipeline</span></span>
<span data-ttu-id="87761-541">hello pipeline har bara en aktivitet.</span><span class="sxs-lookup"><span data-stu-id="87761-541">hello pipeline has just one activity.</span></span> <span data-ttu-id="87761-542">Hej **typen** av hello aktiviteten är inställd för**kopiera**.</span><span class="sxs-lookup"><span data-stu-id="87761-542">hello **type** of hello activity is set too**Copy**.</span></span>  <span data-ttu-id="87761-543">Det finns två delar, en för käll- och hello andra för sink i hello Typegenskaper för hello aktiviteten.</span><span class="sxs-lookup"><span data-stu-id="87761-543">In hello type properties for hello activity, there are two sections, one for source and hello other one for sink.</span></span> <span data-ttu-id="87761-544">hello källtypen har angetts för**BlobSource** som hello aktivitet kopierar data från en blob storage.</span><span class="sxs-lookup"><span data-stu-id="87761-544">hello source type is set too**BlobSource** as hello activity is copying data from a blob storage.</span></span> <span data-ttu-id="87761-545">hello mottagartypen har angetts för**BlobSink** som hello aktivitet kopierar data tooa blob-lagring.</span><span class="sxs-lookup"><span data-stu-id="87761-545">hello sink type is set too**BlobSink** as hello activity copying data tooa blob storage.</span></span> <span data-ttu-id="87761-546">Hej kopieringsaktiviteten använder InputDataset z4y som hello indata och OutputDataset z4y som hello utdata.</span><span class="sxs-lookup"><span data-stu-id="87761-546">hello copy activity takes InputDataset-z4y as hello input and OutputDataset-z4y as hello output.</span></span> 

<span data-ttu-id="87761-547">Mer information om egenskaper som stöds av BlobSource och BlobSink finns [kopiera Aktivitetsegenskaper](#copy-activity-properties) avsnitt.</span><span class="sxs-lookup"><span data-stu-id="87761-547">For more information about properties supported by BlobSource and BlobSink, see [Copy activity properties](#copy-activity-properties) section.</span></span> 

```json
{
    "name": "CopyPipeline",
    "properties": {
        "activities": [
            {
                "type": "Copy",
                "typeProperties": {
                    "source": {
                        "type": "BlobSource",
                        "recursive": false
                    },
                    "sink": {
                        "type": "BlobSink",
                        "copyBehavior": "MergeFiles",
                        "writeBatchSize": 0,
                        "writeBatchTimeout": "00:00:00"
                    }
                },
                "inputs": [
                    {
                        "name": "InputDataset-z4y"
                    }
                ],
                "outputs": [
                    {
                        "name": "OutputDataset-z4y"
                    }
                ],
                "policy": {
                    "timeout": "1.00:00:00",
                    "concurrency": 1,
                    "executionPriorityOrder": "NewestFirst",
                    "style": "StartOfInterval",
                    "retry": 3,
                    "longRetry": 0,
                    "longRetryInterval": "00:00:00"
                },
                "scheduler": {
                    "frequency": "Day",
                    "interval": 1
                },
                "name": "Activity-0-Blob path_ adfblobconnector_input_->OutputDataset-z4y"
            }
        ],
        "start": "2017-04-21T22:34:00Z",
        "end": "2017-04-25T05:00:00Z",
        "isPaused": false,
        "pipelineMode": "Scheduled"
    }
}
```

## <a name="json-examples-for-copying-data-tooand-from-blob-storage"></a><span data-ttu-id="87761-548">JSON-exempel för att kopiera data tooand från Blob Storage</span><span class="sxs-lookup"><span data-stu-id="87761-548">JSON examples for copying data tooand from Blob Storage</span></span>  
<span data-ttu-id="87761-549">hello följande exempel ger exempel JSON definitioner som du kan använda toocreate en pipeline med hjälp av [Azure-portalen](data-factory-copy-activity-tutorial-using-azure-portal.md) eller [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md) eller [Azure PowerShell](data-factory-copy-activity-tutorial-using-powershell.md).</span><span class="sxs-lookup"><span data-stu-id="87761-549">hello following examples provide sample JSON definitions that you can use toocreate a pipeline by using [Azure portal](data-factory-copy-activity-tutorial-using-azure-portal.md) or [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md) or [Azure PowerShell](data-factory-copy-activity-tutorial-using-powershell.md).</span></span> <span data-ttu-id="87761-550">De visar hur toocopy data tooand från Azure Blob Storage och Azure SQL Database.</span><span class="sxs-lookup"><span data-stu-id="87761-550">They show how toocopy data tooand from Azure Blob Storage and Azure SQL Database.</span></span> <span data-ttu-id="87761-551">Dock datan kan kopieras **direkt** från alla källor tooany av hello sänkor anges [här](data-factory-data-movement-activities.md#supported-data-stores-and-formats) med hello Kopieringsaktiviteten i Azure Data Factory.</span><span class="sxs-lookup"><span data-stu-id="87761-551">However, data can be copied **directly** from any of sources tooany of hello sinks stated [here](data-factory-data-movement-activities.md#supported-data-stores-and-formats) using hello Copy Activity in Azure Data Factory.</span></span>

### <a name="json-example-copy-data-from-blob-storage-toosql-database"></a><span data-ttu-id="87761-552">JSON-exempel: Kopiera data från Blob Storage tooSQL databas</span><span class="sxs-lookup"><span data-stu-id="87761-552">JSON Example: Copy data from Blob Storage tooSQL Database</span></span>
<span data-ttu-id="87761-553">följande exempel visar hello:</span><span class="sxs-lookup"><span data-stu-id="87761-553">hello following sample shows:</span></span>

1. <span data-ttu-id="87761-554">En länkad tjänst av typen [AzureSqlDatabase](data-factory-azure-sql-connector.md#linked-service-properties).</span><span class="sxs-lookup"><span data-stu-id="87761-554">A linked service of type [AzureSqlDatabase](data-factory-azure-sql-connector.md#linked-service-properties).</span></span>
2. <span data-ttu-id="87761-555">En länkad tjänst av typen [AzureStorage](#linked-service-properties).</span><span class="sxs-lookup"><span data-stu-id="87761-555">A linked service of type [AzureStorage](#linked-service-properties).</span></span>
3. <span data-ttu-id="87761-556">Indata [dataset](data-factory-create-datasets.md) av typen [AzureBlob](#dataset-properties).</span><span class="sxs-lookup"><span data-stu-id="87761-556">An input [dataset](data-factory-create-datasets.md) of type [AzureBlob](#dataset-properties).</span></span>
4. <span data-ttu-id="87761-557">Utdata [dataset](data-factory-create-datasets.md) av typen [AzureSqlTable](data-factory-azure-sql-connector.md#dataset-properties).</span><span class="sxs-lookup"><span data-stu-id="87761-557">An output [dataset](data-factory-create-datasets.md) of type [AzureSqlTable](data-factory-azure-sql-connector.md#dataset-properties).</span></span>
5. <span data-ttu-id="87761-558">En [pipeline](data-factory-create-pipelines.md) med en kopia-aktivitet som använder [BlobSource](#copy-activity-properties) och [SqlSink](data-factory-azure-sql-connector.md#copy-activity-properties).</span><span class="sxs-lookup"><span data-stu-id="87761-558">A [pipeline](data-factory-create-pipelines.md) with a Copy activity that uses [BlobSource](#copy-activity-properties) and [SqlSink](data-factory-azure-sql-connector.md#copy-activity-properties).</span></span>

<span data-ttu-id="87761-559">hello exemplet kopierar time series-data från en tabell i Azure blob tooan Azure SQL varje timme.</span><span class="sxs-lookup"><span data-stu-id="87761-559">hello sample copies time-series data from an Azure blob tooan Azure SQL table hourly.</span></span> <span data-ttu-id="87761-560">hello JSON egenskaper som används i exemplen beskrivs i hello-exempel i följande avsnitt.</span><span class="sxs-lookup"><span data-stu-id="87761-560">hello JSON properties used in these samples are described in sections following hello samples.</span></span>

<span data-ttu-id="87761-561">**Azure SQL länkade tjänsten:**</span><span class="sxs-lookup"><span data-stu-id="87761-561">**Azure SQL linked service:**</span></span>

```json
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
<span data-ttu-id="87761-562">**Azure Storage länkade tjänsten:**</span><span class="sxs-lookup"><span data-stu-id="87761-562">**Azure Storage linked service:**</span></span>

```json
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
<span data-ttu-id="87761-563">Azure Data Factory stöder två typer av länkad Azure Storage services: **AzureStorage** och **AzureStorageSas**.</span><span class="sxs-lookup"><span data-stu-id="87761-563">Azure Data Factory supports two types of Azure Storage linked services: **AzureStorage** and **AzureStorageSas**.</span></span> <span data-ttu-id="87761-564">För hello första, anger du hello anslutningssträng som innehåller hello kontonyckel och för hello senare, anger du hello delade signatur åtkomst (SAS)-Uri.</span><span class="sxs-lookup"><span data-stu-id="87761-564">For hello first one, you specify hello connection string that includes hello account key and for hello later one, you specify hello Shared Access Signature (SAS) Uri.</span></span> <span data-ttu-id="87761-565">Se [länkade tjänster](#linked-service-properties) information.</span><span class="sxs-lookup"><span data-stu-id="87761-565">See [Linked Services](#linked-service-properties) section for details.</span></span>  

<span data-ttu-id="87761-566">**Azure Blob inkommande datamängd:**</span><span class="sxs-lookup"><span data-stu-id="87761-566">**Azure Blob input dataset:**</span></span>

<span data-ttu-id="87761-567">Data hämtas från en ny blob varje timme (frekvens: timme, intervall: 1).</span><span class="sxs-lookup"><span data-stu-id="87761-567">Data is picked up from a new blob every hour (frequency: hour, interval: 1).</span></span> <span data-ttu-id="87761-568">hello mappen sökvägen och filnamnet för hello blob utvärderas dynamiskt baserat på hello starttiden för hello-segment som bearbetas.</span><span class="sxs-lookup"><span data-stu-id="87761-568">hello folder path and file name for hello blob are dynamically evaluated based on hello start time of hello slice that is being processed.</span></span> <span data-ttu-id="87761-569">hello mappsökväg använder år, månad och som en dagdel av hello starttid och filnamn använder hello timme tillhör hello starttid.</span><span class="sxs-lookup"><span data-stu-id="87761-569">hello folder path uses year, month, and day part of hello start time and file name uses hello hour part of hello start time.</span></span> <span data-ttu-id="87761-570">”externa”: ”true” inställningen informerar Data Factory hello tabellen är externa toohello data factory och inte tillverkas av en aktivitet i hello data factory.</span><span class="sxs-lookup"><span data-stu-id="87761-570">“external”: “true” setting informs Data Factory that hello table is external toohello data factory and is not produced by an activity in hello data factory.</span></span>

```json
{
  "name": "AzureBlobInput",
  "properties": {
    "type": "AzureBlob",
    "linkedServiceName": "StorageLinkedService",
    "typeProperties": {
      "folderPath": "mycontainer/myfolder/yearno={Year}/monthno={Month}/dayno={Day}/",
      "fileName": "{Hour}.csv",
      "partitionedBy": [
        { "name": "Year", "value": { "type": "DateTime", "date": "SliceStart", "format": "yyyy" } },
        { "name": "Month", "value": { "type": "DateTime", "date": "SliceStart", "format": "MM" } },
        { "name": "Day", "value": { "type": "DateTime", "date": "SliceStart", "format": "dd" } },
        { "name": "Hour", "value": { "type": "DateTime", "date": "SliceStart", "format": "HH" } }
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
<span data-ttu-id="87761-571">**Azure SQL utdatauppsättningen:**</span><span class="sxs-lookup"><span data-stu-id="87761-571">**Azure SQL output dataset:**</span></span>

<span data-ttu-id="87761-572">hello exemplet kopierar data tooa tabell med namnet ”mytable” som prefix i en Azure SQL database.</span><span class="sxs-lookup"><span data-stu-id="87761-572">hello sample copies data tooa table named “MyTable” in an Azure SQL database.</span></span> <span data-ttu-id="87761-573">Skapa hello tabell i Azure SQL database med hello samma antal kolumner som du förväntar dig hello Blob CSV-filen toocontain.</span><span class="sxs-lookup"><span data-stu-id="87761-573">Create hello table in your Azure SQL database with hello same number of columns as you expect hello Blob CSV file toocontain.</span></span> <span data-ttu-id="87761-574">Nya rader läggs toohello tabell varje timme.</span><span class="sxs-lookup"><span data-stu-id="87761-574">New rows are added toohello table every hour.</span></span>

```json
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
<span data-ttu-id="87761-575">**Kopieringsaktiviteten i en pipeline med Blob källa och mottagare för SQL:**</span><span class="sxs-lookup"><span data-stu-id="87761-575">**A copy activity in a pipeline with Blob source and SQL sink:**</span></span>

<span data-ttu-id="87761-576">hello pipelinen innehåller en kopia-aktivitet som är konfigurerade toouse hello inkommande och utgående datauppsättningar och är schemalagda toorun varje timme.</span><span class="sxs-lookup"><span data-stu-id="87761-576">hello pipeline contains a Copy Activity that is configured toouse hello input and output datasets and is scheduled toorun every hour.</span></span> <span data-ttu-id="87761-577">I hello pipeline JSON-definitionen hello **källa** typ har angetts för**BlobSource** och **sink** typ har angetts för**SqlSink**.</span><span class="sxs-lookup"><span data-stu-id="87761-577">In hello pipeline JSON definition, hello **source** type is set too**BlobSource** and **sink** type is set too**SqlSink**.</span></span>

```json
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
            "type": "BlobSource"
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
### <a name="json-example-copy-data-from-azure-sql-tooazure-blob"></a><span data-ttu-id="87761-578">JSON-exempel: Kopiera data från Azure SQL tooAzure Blob</span><span class="sxs-lookup"><span data-stu-id="87761-578">JSON Example: Copy data from Azure SQL tooAzure Blob</span></span>
<span data-ttu-id="87761-579">följande exempel visar hello:</span><span class="sxs-lookup"><span data-stu-id="87761-579">hello following sample shows:</span></span>

1. <span data-ttu-id="87761-580">En länkad tjänst av typen [AzureSqlDatabase](data-factory-azure-sql-connector.md#linked-service-properties).</span><span class="sxs-lookup"><span data-stu-id="87761-580">A linked service of type [AzureSqlDatabase](data-factory-azure-sql-connector.md#linked-service-properties).</span></span>
2. <span data-ttu-id="87761-581">En länkad tjänst av typen [AzureStorage](#linked-service-properties).</span><span class="sxs-lookup"><span data-stu-id="87761-581">A linked service of type [AzureStorage](#linked-service-properties).</span></span>
3. <span data-ttu-id="87761-582">Indata [dataset](data-factory-create-datasets.md) av typen [AzureSqlTable](data-factory-azure-sql-connector.md#dataset-properties).</span><span class="sxs-lookup"><span data-stu-id="87761-582">An input [dataset](data-factory-create-datasets.md) of type [AzureSqlTable](data-factory-azure-sql-connector.md#dataset-properties).</span></span>
4. <span data-ttu-id="87761-583">Utdata [dataset](data-factory-create-datasets.md) av typen [AzureBlob](#dataset-properties).</span><span class="sxs-lookup"><span data-stu-id="87761-583">An output [dataset](data-factory-create-datasets.md) of type [AzureBlob](#dataset-properties).</span></span>
5. <span data-ttu-id="87761-584">En [pipeline](data-factory-create-pipelines.md) med kopieringsaktiviteten som använder [SqlSource](data-factory-azure-sql-connector.md#copy-activity-properties) och [BlobSink](#copy-activity-properties).</span><span class="sxs-lookup"><span data-stu-id="87761-584">A [pipeline](data-factory-create-pipelines.md) with Copy activity that uses [SqlSource](data-factory-azure-sql-connector.md#copy-activity-properties) and [BlobSink](#copy-activity-properties).</span></span>

<span data-ttu-id="87761-585">hello exemplet kopierar time series-data från en Azure SQL-tabell tooan Azure blob varje timme.</span><span class="sxs-lookup"><span data-stu-id="87761-585">hello sample copies time-series data from an Azure SQL table tooan Azure blob hourly.</span></span> <span data-ttu-id="87761-586">hello JSON egenskaper som används i exemplen beskrivs i hello-exempel i följande avsnitt.</span><span class="sxs-lookup"><span data-stu-id="87761-586">hello JSON properties used in these samples are described in sections following hello samples.</span></span>

<span data-ttu-id="87761-587">**Azure SQL länkade tjänsten:**</span><span class="sxs-lookup"><span data-stu-id="87761-587">**Azure SQL linked service:**</span></span>

```json
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
<span data-ttu-id="87761-588">**Azure Storage länkade tjänsten:**</span><span class="sxs-lookup"><span data-stu-id="87761-588">**Azure Storage linked service:**</span></span>

```json
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
<span data-ttu-id="87761-589">Azure Data Factory stöder två typer av länkad Azure Storage services: **AzureStorage** och **AzureStorageSas**.</span><span class="sxs-lookup"><span data-stu-id="87761-589">Azure Data Factory supports two types of Azure Storage linked services: **AzureStorage** and **AzureStorageSas**.</span></span> <span data-ttu-id="87761-590">För hello första, anger du hello anslutningssträng som innehåller hello kontonyckel och för hello senare, anger du hello delade signatur åtkomst (SAS)-Uri.</span><span class="sxs-lookup"><span data-stu-id="87761-590">For hello first one, you specify hello connection string that includes hello account key and for hello later one, you specify hello Shared Access Signature (SAS) Uri.</span></span> <span data-ttu-id="87761-591">Se [länkade tjänster](#linked-service-properties) information.</span><span class="sxs-lookup"><span data-stu-id="87761-591">See [Linked Services](#linked-service-properties) section for details.</span></span>  

<span data-ttu-id="87761-592">**Azure SQL inkommande datamängd:**</span><span class="sxs-lookup"><span data-stu-id="87761-592">**Azure SQL input dataset:**</span></span>

<span data-ttu-id="87761-593">hello exemplet förutsätter att du har skapat en tabell ”mytable” som prefix i Azure SQL och innehåller en kolumn med namnet ”timestampcolumn” för tid series-data.</span><span class="sxs-lookup"><span data-stu-id="87761-593">hello sample assumes you have created a table “MyTable” in Azure SQL and it contains a column called “timestampcolumn” for time series data.</span></span>

<span data-ttu-id="87761-594">Inställningen ”externa”: ”true” informerar Data Factory-tjänsten hello tabellen är externa toohello data factory och inte tillverkas av en aktivitet i hello data factory.</span><span class="sxs-lookup"><span data-stu-id="87761-594">Setting “external”: ”true” informs Data Factory service that hello table is external toohello data factory and is not produced by an activity in hello data factory.</span></span>

```json
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

<span data-ttu-id="87761-595">**Azure Blob utdatauppsättningen:**</span><span class="sxs-lookup"><span data-stu-id="87761-595">**Azure Blob output dataset:**</span></span>

<span data-ttu-id="87761-596">Data skrivs tooa nya blob varje timme (frekvens: timme, intervall: 1).</span><span class="sxs-lookup"><span data-stu-id="87761-596">Data is written tooa new blob every hour (frequency: hour, interval: 1).</span></span> <span data-ttu-id="87761-597">hello mappsökväg för hello blob utvärderas dynamiskt baserat på hello starttiden för hello-segment som bearbetas.</span><span class="sxs-lookup"><span data-stu-id="87761-597">hello folder path for hello blob is dynamically evaluated based on hello start time of hello slice that is being processed.</span></span> <span data-ttu-id="87761-598">hello mappsökväg använder år, månad, dag och timmar delar av hello starttid.</span><span class="sxs-lookup"><span data-stu-id="87761-598">hello folder path uses year, month, day, and hours parts of hello start time.</span></span>

```json
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
          "value": { "type": "DateTime",  "date": "SliceStart", "format": "yyyy" } },
        { "name": "Month", "value": { "type": "DateTime", "date": "SliceStart", "format": "MM" } },
        { "name": "Day", "value": { "type": "DateTime", "date": "SliceStart", "format": "dd" } },
        { "name": "Hour", "value": { "type": "DateTime", "date": "SliceStart", "format": "HH" } }
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

<span data-ttu-id="87761-599">**Kopieringsaktiviteten i en pipeline med SQL-källa och mottagare för Blob:**</span><span class="sxs-lookup"><span data-stu-id="87761-599">**A copy activity in a pipeline with SQL source and Blob sink:**</span></span>

<span data-ttu-id="87761-600">hello pipelinen innehåller en kopia-aktivitet som är konfigurerade toouse hello inkommande och utgående datauppsättningar och är schemalagda toorun varje timme.</span><span class="sxs-lookup"><span data-stu-id="87761-600">hello pipeline contains a Copy Activity that is configured toouse hello input and output datasets and is scheduled toorun every hour.</span></span> <span data-ttu-id="87761-601">I hello pipeline JSON-definitionen hello **källa** typ har angetts för**SqlSource** och **sink** typ har angetts för**BlobSink**.</span><span class="sxs-lookup"><span data-stu-id="87761-601">In hello pipeline JSON definition, hello **source** type is set too**SqlSource** and **sink** type is set too**BlobSink**.</span></span> <span data-ttu-id="87761-602">hello SQL-frågan som angetts för hello **SqlReaderQuery** egenskapen väljer hello data i hello tidigare timme toocopy.</span><span class="sxs-lookup"><span data-stu-id="87761-602">hello SQL query specified for hello **SqlReaderQuery** property selects hello data in hello past hour toocopy.</span></span>

```json
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

> [!NOTE]
> <span data-ttu-id="87761-603">toomap kolumner från källan dataset toocolumns från sink dataset finns [mappa dataset kolumner i Azure Data Factory](data-factory-map-columns.md).</span><span class="sxs-lookup"><span data-stu-id="87761-603">toomap columns from source dataset toocolumns from sink dataset, see [Mapping dataset columns in Azure Data Factory](data-factory-map-columns.md).</span></span>

## <a name="performance-and-tuning"></a><span data-ttu-id="87761-604">Prestanda och finjustering</span><span class="sxs-lookup"><span data-stu-id="87761-604">Performance and Tuning</span></span>
<span data-ttu-id="87761-605">Se [kopiera aktivitet prestanda och justera guiden](data-factory-copy-activity-performance.md) toolearn om nyckeln faktorer som påverkan prestanda för flytt av data (Kopieringsaktiviteten) i Azure Data Factory och olika sätt toooptimize den.</span><span class="sxs-lookup"><span data-stu-id="87761-605">See [Copy Activity Performance & Tuning Guide](data-factory-copy-activity-performance.md) toolearn about key factors that impact performance of data movement (Copy Activity) in Azure Data Factory and various ways toooptimize it.</span></span>
