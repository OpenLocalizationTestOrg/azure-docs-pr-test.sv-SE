---
title: "aaaPush data tooSearch index med hjälp av Data Factory | Microsoft Docs"
description: "Mer information om hur toopush data tooAzure sökindex med hjälp av Azure Data Factory."
services: data-factory
documentationcenter: 
author: linda33wj
manager: jhubbard
editor: monicar
ms.assetid: f8d46e1e-5c37-4408-80fb-c54be532a4ab
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/04/2017
ms.author: jingwang
ms.openlocfilehash: f2d973d0a2c24d6448e2d59e37e24503aa433018
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="push-data-tooan-azure-search-index-by-using-azure-data-factory"></a><span data-ttu-id="3e274-103">Push-data tooan Azure Search index med hjälp av Azure Data Factory</span><span class="sxs-lookup"><span data-stu-id="3e274-103">Push data tooan Azure Search index by using Azure Data Factory</span></span>
<span data-ttu-id="3e274-104">Den här artikeln beskriver hur toouse hello Kopieringsaktiviteten toopush data från en stöds källdata lagrar tooAzure sökindex.</span><span class="sxs-lookup"><span data-stu-id="3e274-104">This article describes how toouse hello Copy Activity toopush data from a supported source data store tooAzure Search index.</span></span> <span data-ttu-id="3e274-105">Datalager stöds källa anges i hello källkolumnen av hello [källor och sänkor stöds](data-factory-data-movement-activities.md#supported-data-stores-and-formats) tabell.</span><span class="sxs-lookup"><span data-stu-id="3e274-105">Supported source data stores are listed in hello Source column of hello [supported sources and sinks](data-factory-data-movement-activities.md#supported-data-stores-and-formats) table.</span></span> <span data-ttu-id="3e274-106">Den här artikeln bygger på hello [data movement aktiviteter](data-factory-data-movement-activities.md) artikel som ger en allmän översikt över dataflyttning stöds data store kombinationer och Kopieringsaktivitet.</span><span class="sxs-lookup"><span data-stu-id="3e274-106">This article builds on hello [data movement activities](data-factory-data-movement-activities.md) article, which presents a general overview of data movement with Copy Activity and supported data store combinations.</span></span>

## <a name="enabling-connectivity"></a><span data-ttu-id="3e274-107">Aktivera anslutning</span><span class="sxs-lookup"><span data-stu-id="3e274-107">Enabling connectivity</span></span>
<span data-ttu-id="3e274-108">tooallow Data Factory-tjänsten Anslut tooan lokalt datalager kan du installera Data Management Gateway i din lokala miljö.</span><span class="sxs-lookup"><span data-stu-id="3e274-108">tooallow Data Factory service connect tooan on-premises data store, you install Data Management Gateway in your on-premises environment.</span></span> <span data-ttu-id="3e274-109">Du kan installera gatewayen på hello samma dator som är värd för hello källdata eller på en separat dator tooavoid konkurrerar om resurser med hello data.</span><span class="sxs-lookup"><span data-stu-id="3e274-109">You can install gateway on hello same machine that hosts hello source data store or on a separate machine tooavoid competing for resources with hello data store.</span></span>

<span data-ttu-id="3e274-110">Data Management Gateway ansluter lokala data sources toocloud tjänster på en säker och hanterad sätt.</span><span class="sxs-lookup"><span data-stu-id="3e274-110">Data Management Gateway connects on-premises data sources toocloud services in a secure and managed way.</span></span> <span data-ttu-id="3e274-111">Se [flytta data mellan lokalt och i molnet](data-factory-move-data-between-onprem-and-cloud.md) artikeln för information om Data Management Gateway.</span><span class="sxs-lookup"><span data-stu-id="3e274-111">See [Move data between on-premises and cloud](data-factory-move-data-between-onprem-and-cloud.md) article for details about Data Management Gateway.</span></span>

## <a name="getting-started"></a><span data-ttu-id="3e274-112">Komma igång</span><span class="sxs-lookup"><span data-stu-id="3e274-112">Getting started</span></span>
<span data-ttu-id="3e274-113">Du kan skapa en pipeline med en kopia-aktivitet som skickar data från en källa data store tooAzure sökindex med hjälp av olika verktyg/API: er.</span><span class="sxs-lookup"><span data-stu-id="3e274-113">You can create a pipeline with a copy activity that pushes data from a source data store tooAzure Search index by using different tools/APIs.</span></span>

<span data-ttu-id="3e274-114">hello enklaste sättet toocreate en pipeline är toouse hello **guiden Kopiera**.</span><span class="sxs-lookup"><span data-stu-id="3e274-114">hello easiest way toocreate a pipeline is toouse hello **Copy Wizard**.</span></span> <span data-ttu-id="3e274-115">Se [Självstudier: skapa en pipeline med hjälp av guiden Kopiera](data-factory-copy-data-wizard-tutorial.md) för en snabb genomgång om hur du skapar en pipeline med hjälp av guiden för hello kopiera data.</span><span class="sxs-lookup"><span data-stu-id="3e274-115">See [Tutorial: Create a pipeline using Copy Wizard](data-factory-copy-data-wizard-tutorial.md) for a quick walkthrough on creating a pipeline using hello Copy data wizard.</span></span>

<span data-ttu-id="3e274-116">Du kan också använda följande verktyg toocreate en pipeline hello: **Azure-portalen**, **Visual Studio**, **Azure PowerShell**, **Azure Resource Manager-mall** , **.NET API**, och **REST API**.</span><span class="sxs-lookup"><span data-stu-id="3e274-116">You can also use hello following tools toocreate a pipeline: **Azure portal**, **Visual Studio**, **Azure PowerShell**, **Azure Resource Manager template**, **.NET API**, and **REST API**.</span></span> <span data-ttu-id="3e274-117">Se [kopiera aktivitet kursen](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) för stegvisa instruktioner toocreate en pipeline med en Kopieringsaktivitet.</span><span class="sxs-lookup"><span data-stu-id="3e274-117">See [Copy activity tutorial](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) for step-by-step instructions toocreate a pipeline with a copy activity.</span></span> 

<span data-ttu-id="3e274-118">Om du använder hello verktyg eller API: er kan utföra du hello följande steg toocreate en pipeline som flyttar data från en källdata lagra tooa sink-datalagret:</span><span class="sxs-lookup"><span data-stu-id="3e274-118">Whether you use hello tools or APIs, you perform hello following steps toocreate a pipeline that moves data from a source data store tooa sink data store:</span></span> 

1. <span data-ttu-id="3e274-119">Skapa **länkade tjänster** toolink indata och utdata lagrar tooyour data factory.</span><span class="sxs-lookup"><span data-stu-id="3e274-119">Create **linked services** toolink input and output data stores tooyour data factory.</span></span>
2. <span data-ttu-id="3e274-120">Skapa **datauppsättningar** toorepresent indata och utdata för hello kopieringsåtgärden.</span><span class="sxs-lookup"><span data-stu-id="3e274-120">Create **datasets** toorepresent input and output data for hello copy operation.</span></span> 
3. <span data-ttu-id="3e274-121">Skapa en **pipeline** med en kopia-aktivitet som tar en datamängd som indata och en dataset som utdata.</span><span class="sxs-lookup"><span data-stu-id="3e274-121">Create a **pipeline** with a copy activity that takes a dataset as an input and a dataset as an output.</span></span> 

<span data-ttu-id="3e274-122">När du använder guiden hello skapas automatiskt JSON definitioner för dessa Data Factory-enheter (länkade tjänster, datauppsättningar och hello pipeline) för dig.</span><span class="sxs-lookup"><span data-stu-id="3e274-122">When you use hello wizard, JSON definitions for these Data Factory entities (linked services, datasets, and hello pipeline) are automatically created for you.</span></span> <span data-ttu-id="3e274-123">När du använder Verktyg/API: er (utom .NET API), kan du definiera dessa Data Factory-enheter med hjälp av hello JSON-format.</span><span class="sxs-lookup"><span data-stu-id="3e274-123">When you use tools/APIs (except .NET API), you define these Data Factory entities by using hello JSON format.</span></span>  <span data-ttu-id="3e274-124">Ett exempel med JSON-definitioner för Data Factory-entiteter som har använt toocopy data tooAzure sökindex finns [JSON-exempel: kopiera data från lokala SQL Server tooAzure sökindex](#json-example-copy-data-from-on-premises-sql-server-to-azure-search-index) i den här artikeln.</span><span class="sxs-lookup"><span data-stu-id="3e274-124">For a sample with JSON definitions for Data Factory entities that are used toocopy data tooAzure Search index, see [JSON example: Copy data from on-premises SQL Server tooAzure Search index](#json-example-copy-data-from-on-premises-sql-server-to-azure-search-index) section of this article.</span></span> 

<span data-ttu-id="3e274-125">hello följande avsnitt innehåller information om JSON-egenskaper används toodefine Data Factory entiteter specifika tooAzure sökindex:</span><span class="sxs-lookup"><span data-stu-id="3e274-125">hello following sections provide details about JSON properties that are used toodefine Data Factory entities specific tooAzure Search Index:</span></span>

## <a name="linked-service-properties"></a><span data-ttu-id="3e274-126">Länkad tjänstegenskaper</span><span class="sxs-lookup"><span data-stu-id="3e274-126">Linked service properties</span></span>

<span data-ttu-id="3e274-127">hello i den följande tabellen finns beskrivningar JSON-element som är specifika toohello Azure Search länkade tjänsten.</span><span class="sxs-lookup"><span data-stu-id="3e274-127">hello following table provides descriptions for JSON elements that are specific toohello Azure Search linked service.</span></span>

| <span data-ttu-id="3e274-128">Egenskap</span><span class="sxs-lookup"><span data-stu-id="3e274-128">Property</span></span> | <span data-ttu-id="3e274-129">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="3e274-129">Description</span></span> | <span data-ttu-id="3e274-130">Krävs</span><span class="sxs-lookup"><span data-stu-id="3e274-130">Required</span></span> |
| -------- | ----------- | -------- |
| <span data-ttu-id="3e274-131">typ</span><span class="sxs-lookup"><span data-stu-id="3e274-131">type</span></span> | <span data-ttu-id="3e274-132">hello Typegenskapen måste anges till: **AzureSearch**.</span><span class="sxs-lookup"><span data-stu-id="3e274-132">hello type property must be set to: **AzureSearch**.</span></span> | <span data-ttu-id="3e274-133">Ja</span><span class="sxs-lookup"><span data-stu-id="3e274-133">Yes</span></span> |
| <span data-ttu-id="3e274-134">URL: en</span><span class="sxs-lookup"><span data-stu-id="3e274-134">url</span></span> | <span data-ttu-id="3e274-135">URL för hello Azure Search-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="3e274-135">URL for hello Azure Search service.</span></span> | <span data-ttu-id="3e274-136">Ja</span><span class="sxs-lookup"><span data-stu-id="3e274-136">Yes</span></span> |
| <span data-ttu-id="3e274-137">key</span><span class="sxs-lookup"><span data-stu-id="3e274-137">key</span></span> | <span data-ttu-id="3e274-138">Admin-nyckel för hello Azure Search-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="3e274-138">Admin key for hello Azure Search service.</span></span> | <span data-ttu-id="3e274-139">Ja</span><span class="sxs-lookup"><span data-stu-id="3e274-139">Yes</span></span> |

## <a name="dataset-properties"></a><span data-ttu-id="3e274-140">Egenskaper för datamängd</span><span class="sxs-lookup"><span data-stu-id="3e274-140">Dataset properties</span></span>

<span data-ttu-id="3e274-141">En fullständig lista över avsnitt och egenskaper som är tillgängliga för att definiera datauppsättningar finns hello [skapa datauppsättningar](data-factory-create-datasets.md) artikel.</span><span class="sxs-lookup"><span data-stu-id="3e274-141">For a full list of sections and properties that are available for defining datasets, see hello [Creating datasets](data-factory-create-datasets.md) article.</span></span> <span data-ttu-id="3e274-142">Avsnitt som struktur, tillgänglighet och princip på en datamängd JSON är liknande för alla typer av datauppsättningen.</span><span class="sxs-lookup"><span data-stu-id="3e274-142">Sections such as structure, availability, and policy of a dataset JSON are similar for all dataset types.</span></span> <span data-ttu-id="3e274-143">Hej **typeProperties** avsnittet är olika för varje typ av datauppsättningen.</span><span class="sxs-lookup"><span data-stu-id="3e274-143">hello **typeProperties** section is different for each type of dataset.</span></span> <span data-ttu-id="3e274-144">Hej typeProperties avsnittet för en dataset av hello typen **AzureSearchIndex** har hello följande egenskaper:</span><span class="sxs-lookup"><span data-stu-id="3e274-144">hello typeProperties section for a dataset of hello type **AzureSearchIndex** has hello following properties:</span></span>

| <span data-ttu-id="3e274-145">Egenskap</span><span class="sxs-lookup"><span data-stu-id="3e274-145">Property</span></span> | <span data-ttu-id="3e274-146">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="3e274-146">Description</span></span> | <span data-ttu-id="3e274-147">Krävs</span><span class="sxs-lookup"><span data-stu-id="3e274-147">Required</span></span> |
| -------- | ----------- | -------- |
| <span data-ttu-id="3e274-148">typ</span><span class="sxs-lookup"><span data-stu-id="3e274-148">type</span></span> | <span data-ttu-id="3e274-149">hello Typegenskapen måste anges för**AzureSearchIndex**.</span><span class="sxs-lookup"><span data-stu-id="3e274-149">hello type property must be set too**AzureSearchIndex**.</span></span>| <span data-ttu-id="3e274-150">Ja</span><span class="sxs-lookup"><span data-stu-id="3e274-150">Yes</span></span> |
| <span data-ttu-id="3e274-151">Indexnamn</span><span class="sxs-lookup"><span data-stu-id="3e274-151">indexName</span></span> | <span data-ttu-id="3e274-152">Namnet på hello Azure Search index.</span><span class="sxs-lookup"><span data-stu-id="3e274-152">Name of hello Azure Search index.</span></span> <span data-ttu-id="3e274-153">Data Factory skapar inte hello index.</span><span class="sxs-lookup"><span data-stu-id="3e274-153">Data Factory does not create hello index.</span></span> <span data-ttu-id="3e274-154">hello index måste finnas i Azure Search.</span><span class="sxs-lookup"><span data-stu-id="3e274-154">hello index must exist in Azure Search.</span></span> | <span data-ttu-id="3e274-155">Ja</span><span class="sxs-lookup"><span data-stu-id="3e274-155">Yes</span></span> |


## <a name="copy-activity-properties"></a><span data-ttu-id="3e274-156">Kopiera egenskaper för aktivitet</span><span class="sxs-lookup"><span data-stu-id="3e274-156">Copy activity properties</span></span>
<span data-ttu-id="3e274-157">En fullständig lista över avsnitt och egenskaper som är tillgängliga för att definiera aktiviteter finns hello [skapar pipelines](data-factory-create-pipelines.md) artikel.</span><span class="sxs-lookup"><span data-stu-id="3e274-157">For a full list of sections and properties that are available for defining activities, see hello [Creating pipelines](data-factory-create-pipelines.md) article.</span></span> <span data-ttu-id="3e274-158">Egenskaper, till exempel namn, beskrivning, indata och utdatatabeller och olika principer är tillgängliga för alla typer av aktiviteter.</span><span class="sxs-lookup"><span data-stu-id="3e274-158">Properties such as name, description, input and output tables, and various policies are available for all types of activities.</span></span> <span data-ttu-id="3e274-159">De egenskaper som är tillgängliga i hello typeProperties avsnittet varierar beroende på varje aktivitetstyp.</span><span class="sxs-lookup"><span data-stu-id="3e274-159">Whereas, properties available in hello typeProperties section vary with each activity type.</span></span> <span data-ttu-id="3e274-160">För Kopieringsaktivitet kan varierar de beroende på hello typer av datakällor och sänkor.</span><span class="sxs-lookup"><span data-stu-id="3e274-160">For Copy Activity, they vary depending on hello types of sources and sinks.</span></span>

<span data-ttu-id="3e274-161">För Kopieringsaktiviteten när hello mottagare är av typen hello **AzureSearchIndexSink**, hello följande egenskaper finns i avsnittet typeProperties:</span><span class="sxs-lookup"><span data-stu-id="3e274-161">For Copy Activity, when hello sink is of hello type **AzureSearchIndexSink**, hello following properties are available in typeProperties section:</span></span>

| <span data-ttu-id="3e274-162">Egenskap</span><span class="sxs-lookup"><span data-stu-id="3e274-162">Property</span></span> | <span data-ttu-id="3e274-163">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="3e274-163">Description</span></span> | <span data-ttu-id="3e274-164">Tillåtna värden</span><span class="sxs-lookup"><span data-stu-id="3e274-164">Allowed values</span></span> | <span data-ttu-id="3e274-165">Krävs</span><span class="sxs-lookup"><span data-stu-id="3e274-165">Required</span></span> |
| -------- | ----------- | -------------- | -------- |
| <span data-ttu-id="3e274-166">WriteBehavior</span><span class="sxs-lookup"><span data-stu-id="3e274-166">WriteBehavior</span></span> | <span data-ttu-id="3e274-167">Anger om toomerge eller ersätta när ett dokument det redan finns i hello index.</span><span class="sxs-lookup"><span data-stu-id="3e274-167">Specifies whether toomerge or replace when a document already exists in hello index.</span></span> <span data-ttu-id="3e274-168">Se hello [WriteBehavior egenskapen](#writebehavior-property).</span><span class="sxs-lookup"><span data-stu-id="3e274-168">See hello [WriteBehavior property](#writebehavior-property).</span></span>| <span data-ttu-id="3e274-169">sammanfoga (standard)</span><span class="sxs-lookup"><span data-stu-id="3e274-169">Merge (default)</span></span><br/><span data-ttu-id="3e274-170">Ladda upp</span><span class="sxs-lookup"><span data-stu-id="3e274-170">Upload</span></span>| <span data-ttu-id="3e274-171">Nej</span><span class="sxs-lookup"><span data-stu-id="3e274-171">No</span></span> |
| <span data-ttu-id="3e274-172">writeBatchSize</span><span class="sxs-lookup"><span data-stu-id="3e274-172">WriteBatchSize</span></span> | <span data-ttu-id="3e274-173">Överför data till hello Azure Search index när hello buffertstorlek når writeBatchSize.</span><span class="sxs-lookup"><span data-stu-id="3e274-173">Uploads data into hello Azure Search index when hello buffer size reaches writeBatchSize.</span></span> <span data-ttu-id="3e274-174">Se hello [WriteBatchSize egenskapen](#writebatchsize-property) mer information.</span><span class="sxs-lookup"><span data-stu-id="3e274-174">See hello [WriteBatchSize property](#writebatchsize-property) for details.</span></span> | <span data-ttu-id="3e274-175">1 too1 000.</span><span class="sxs-lookup"><span data-stu-id="3e274-175">1 too1,000.</span></span> <span data-ttu-id="3e274-176">Standardvärdet är 1000.</span><span class="sxs-lookup"><span data-stu-id="3e274-176">Default value is 1000.</span></span> | <span data-ttu-id="3e274-177">Nej</span><span class="sxs-lookup"><span data-stu-id="3e274-177">No</span></span> |

### <a name="writebehavior-property"></a><span data-ttu-id="3e274-178">Egenskapen WriteBehavior</span><span class="sxs-lookup"><span data-stu-id="3e274-178">WriteBehavior property</span></span>
<span data-ttu-id="3e274-179">AzureSearchSink upserts när data skrivs.</span><span class="sxs-lookup"><span data-stu-id="3e274-179">AzureSearchSink upserts when writing data.</span></span> <span data-ttu-id="3e274-180">När du skriver ett dokument, om hello dokumentnyckeln finns redan i hello Azure Search index, uppdaterar Azure Search med andra ord hello befintligt dokument i stället för att en konflikt undantag.</span><span class="sxs-lookup"><span data-stu-id="3e274-180">In other words, when writing a document, if hello document key already exists in hello Azure Search index, Azure Search updates hello existing document rather than throwing a conflict exception.</span></span>

<span data-ttu-id="3e274-181">Hej AzureSearchSink innehåller hello följande två upsert beteenden (med hjälp av AzureSearch SDK):</span><span class="sxs-lookup"><span data-stu-id="3e274-181">hello AzureSearchSink provides hello following two upsert behaviors (by using AzureSearch SDK):</span></span>

- <span data-ttu-id="3e274-182">**Sammanfoga**: kombinera alla hello kolumner i hello nytt dokument med hello befintliga en.</span><span class="sxs-lookup"><span data-stu-id="3e274-182">**Merge**: combine all hello columns in hello new document with hello existing one.</span></span> <span data-ttu-id="3e274-183">Hej värdet i hello befintliga en bevaras i kolumner med null-värde i hello nytt dokument.</span><span class="sxs-lookup"><span data-stu-id="3e274-183">For columns with null value in hello new document, hello value in hello existing one is preserved.</span></span>
- <span data-ttu-id="3e274-184">**Överför**: hello nya dokument ersätter hello befintlig.</span><span class="sxs-lookup"><span data-stu-id="3e274-184">**Upload**: hello new document replaces hello existing one.</span></span> <span data-ttu-id="3e274-185">För kolumner som inte har angetts i hello nytt dokument värdet hello toonull om det finns ett icke-null-värde i hello befintligt dokument eller inte.</span><span class="sxs-lookup"><span data-stu-id="3e274-185">For columns not specified in hello new document, hello value is set toonull whether there is a non-null value in hello existing document or not.</span></span>

<span data-ttu-id="3e274-186">hello standardbeteendet är **sammanfoga**.</span><span class="sxs-lookup"><span data-stu-id="3e274-186">hello default behavior is **Merge**.</span></span>

### <a name="writebatchsize-property"></a><span data-ttu-id="3e274-187">Egenskapen WriteBatchSize</span><span class="sxs-lookup"><span data-stu-id="3e274-187">WriteBatchSize Property</span></span>
<span data-ttu-id="3e274-188">Azure Search-tjänsten stöder skrivning dokument som en batch.</span><span class="sxs-lookup"><span data-stu-id="3e274-188">Azure Search service supports writing documents as a batch.</span></span> <span data-ttu-id="3e274-189">En grupp kan innehålla 1 too1, 000 åtgärder.</span><span class="sxs-lookup"><span data-stu-id="3e274-189">A batch can contain 1 too1,000 Actions.</span></span> <span data-ttu-id="3e274-190">En åtgärd som hanterar ett dokument tooperform hello överför/merge-operation.</span><span class="sxs-lookup"><span data-stu-id="3e274-190">An action handles one document tooperform hello upload/merge operation.</span></span>

### <a name="data-type-support"></a><span data-ttu-id="3e274-191">Stöd för datatypen</span><span class="sxs-lookup"><span data-stu-id="3e274-191">Data type support</span></span>
<span data-ttu-id="3e274-192">hello anger följande tabell om en Azure Search-datatyp stöds eller inte.</span><span class="sxs-lookup"><span data-stu-id="3e274-192">hello following table specifies whether an Azure Search data type is supported or not.</span></span>

| <span data-ttu-id="3e274-193">Azure Search-datatyp</span><span class="sxs-lookup"><span data-stu-id="3e274-193">Azure Search data type</span></span> | <span data-ttu-id="3e274-194">Stöds i Azure Search Sink</span><span class="sxs-lookup"><span data-stu-id="3e274-194">Supported in Azure Search Sink</span></span> |
| ---------------------- | ------------------------------ |
| <span data-ttu-id="3e274-195">Sträng</span><span class="sxs-lookup"><span data-stu-id="3e274-195">String</span></span> | <span data-ttu-id="3e274-196">Y</span><span class="sxs-lookup"><span data-stu-id="3e274-196">Y</span></span> |
| <span data-ttu-id="3e274-197">Int32</span><span class="sxs-lookup"><span data-stu-id="3e274-197">Int32</span></span> | <span data-ttu-id="3e274-198">Y</span><span class="sxs-lookup"><span data-stu-id="3e274-198">Y</span></span> |
| <span data-ttu-id="3e274-199">Int64</span><span class="sxs-lookup"><span data-stu-id="3e274-199">Int64</span></span> | <span data-ttu-id="3e274-200">Y</span><span class="sxs-lookup"><span data-stu-id="3e274-200">Y</span></span> |
| <span data-ttu-id="3e274-201">dubbla</span><span class="sxs-lookup"><span data-stu-id="3e274-201">Double</span></span> | <span data-ttu-id="3e274-202">Y</span><span class="sxs-lookup"><span data-stu-id="3e274-202">Y</span></span> |
| <span data-ttu-id="3e274-203">Booleskt värde</span><span class="sxs-lookup"><span data-stu-id="3e274-203">Boolean</span></span> | <span data-ttu-id="3e274-204">Y</span><span class="sxs-lookup"><span data-stu-id="3e274-204">Y</span></span> |
| <span data-ttu-id="3e274-205">DataTimeOffset</span><span class="sxs-lookup"><span data-stu-id="3e274-205">DataTimeOffset</span></span> | <span data-ttu-id="3e274-206">Y</span><span class="sxs-lookup"><span data-stu-id="3e274-206">Y</span></span> |
| <span data-ttu-id="3e274-207">Strängmatris</span><span class="sxs-lookup"><span data-stu-id="3e274-207">String Array</span></span> | <span data-ttu-id="3e274-208">N</span><span class="sxs-lookup"><span data-stu-id="3e274-208">N</span></span> |
| <span data-ttu-id="3e274-209">GeographyPoint</span><span class="sxs-lookup"><span data-stu-id="3e274-209">GeographyPoint</span></span> | <span data-ttu-id="3e274-210">N</span><span class="sxs-lookup"><span data-stu-id="3e274-210">N</span></span> |

## <a name="json-example-copy-data-from-on-premises-sql-server-tooazure-search-index"></a><span data-ttu-id="3e274-211">JSON-exempel: kopiera data från lokala SQL Server tooAzure sökindex</span><span class="sxs-lookup"><span data-stu-id="3e274-211">JSON example: Copy data from on-premises SQL Server tooAzure Search index</span></span>

<span data-ttu-id="3e274-212">följande exempel visar hello:</span><span class="sxs-lookup"><span data-stu-id="3e274-212">hello following sample shows:</span></span>

1.  <span data-ttu-id="3e274-213">En länkad tjänst av typen [AzureSearch](#linked-service-properties).</span><span class="sxs-lookup"><span data-stu-id="3e274-213">A linked service of type [AzureSearch](#linked-service-properties).</span></span>
2.  <span data-ttu-id="3e274-214">En länkad tjänst av typen [OnPremisesSqlServer](data-factory-sqlserver-connector.md#linked-service-properties).</span><span class="sxs-lookup"><span data-stu-id="3e274-214">A linked service of type [OnPremisesSqlServer](data-factory-sqlserver-connector.md#linked-service-properties).</span></span>
3.  <span data-ttu-id="3e274-215">Indata [dataset](data-factory-create-datasets.md) av typen [SqlServerTable](data-factory-sqlserver-connector.md#dataset-properties).</span><span class="sxs-lookup"><span data-stu-id="3e274-215">An input [dataset](data-factory-create-datasets.md) of type [SqlServerTable](data-factory-sqlserver-connector.md#dataset-properties).</span></span>
4.  <span data-ttu-id="3e274-216">Utdata [dataset](data-factory-create-datasets.md) av typen [AzureSearchIndex](#dataset-properties).</span><span class="sxs-lookup"><span data-stu-id="3e274-216">An output [dataset](data-factory-create-datasets.md) of type [AzureSearchIndex](#dataset-properties).</span></span>
4.  <span data-ttu-id="3e274-217">En [pipeline](data-factory-create-pipelines.md) med en kopia-aktivitet som använder [SqlSource](data-factory-sqlserver-connector.md#copy-activity-properties) och [AzureSearchIndexSink](#copy-activity-properties).</span><span class="sxs-lookup"><span data-stu-id="3e274-217">A [pipeline](data-factory-create-pipelines.md) with a Copy activity that uses [SqlSource](data-factory-sqlserver-connector.md#copy-activity-properties) and [AzureSearchIndexSink](#copy-activity-properties).</span></span>

<span data-ttu-id="3e274-218">hello exemplet kopierar time series-data från en lokal SQL Server-databasen tooan Azure Search index varje timme.</span><span class="sxs-lookup"><span data-stu-id="3e274-218">hello sample copies time-series data from an on-premises SQL Server database tooan Azure Search index hourly.</span></span> <span data-ttu-id="3e274-219">hello JSON egenskaper som används i det här exemplet beskrivs i hello-exempel i följande avsnitt.</span><span class="sxs-lookup"><span data-stu-id="3e274-219">hello JSON properties used in this sample are described in sections following hello samples.</span></span>

<span data-ttu-id="3e274-220">Konfigurera hello data management gateway på din lokala dator som ett första steg.</span><span class="sxs-lookup"><span data-stu-id="3e274-220">As a first step, setup hello data management gateway on your on-premises machine.</span></span> <span data-ttu-id="3e274-221">hello anvisningar finns i hello [flytta data mellan lokala platser och moln](data-factory-move-data-between-onprem-and-cloud.md) artikel.</span><span class="sxs-lookup"><span data-stu-id="3e274-221">hello instructions are in hello [moving data between on-premises locations and cloud](data-factory-move-data-between-onprem-and-cloud.md) article.</span></span>

<span data-ttu-id="3e274-222">**Azure Search länkad tjänst:**</span><span class="sxs-lookup"><span data-stu-id="3e274-222">**Azure Search linked service:**</span></span>

```JSON
{
    "name": "AzureSearchLinkedService",
    "properties": {
        "type": "AzureSearch",
        "typeProperties": {
            "url": "https://<service>.search.windows.net",
            "key": "<AdminKey>"
        }
    }
}
```

<span data-ttu-id="3e274-223">**SQL Server som är länkad tjänst**</span><span class="sxs-lookup"><span data-stu-id="3e274-223">**SQL Server linked service**</span></span>

```JSON
{
  "Name": "SqlServerLinkedService",
  "properties": {
    "type": "OnPremisesSqlServer",
    "typeProperties": {
      "connectionString": "Data Source=<servername>;Initial Catalog=<databasename>;Integrated Security=False;User ID=<username>;Password=<password>;",
      "gatewayName": "<gatewayname>"
    }
  }
}
```

<span data-ttu-id="3e274-224">**SQL Server inkommande dataset**</span><span class="sxs-lookup"><span data-stu-id="3e274-224">**SQL Server input dataset**</span></span>

<span data-ttu-id="3e274-225">hello exemplet förutsätter att du har skapat en tabell ”mytable” som prefix i SQL Server och den innehåller en kolumn med namnet ”timestampcolumn” för tid series-data.</span><span class="sxs-lookup"><span data-stu-id="3e274-225">hello sample assumes you have created a table “MyTable” in SQL Server and it contains a column called “timestampcolumn” for time series data.</span></span> <span data-ttu-id="3e274-226">Du kan fråga över flera tabeller i samma databas som använder en enda dataset, men en enskild tabell måste användas för hello dataset tableName typeProperty hello.</span><span class="sxs-lookup"><span data-stu-id="3e274-226">You can query over multiple tables within hello same database using a single dataset, but a single table must be used for hello dataset's tableName typeProperty.</span></span>

<span data-ttu-id="3e274-227">Inställningen ”externa”: ”true” informerar Data Factory-tjänsten som hello dataset är externa toohello data factory och inte tillverkas av en aktivitet i hello data factory.</span><span class="sxs-lookup"><span data-stu-id="3e274-227">Setting “external”: ”true” informs Data Factory service that hello dataset is external toohello data factory and is not produced by an activity in hello data factory.</span></span>

```JSON
{
  "name": "SqlServerDataset",
  "properties": {
    "type": "SqlServerTable",
    "linkedServiceName": "SqlServerLinkedService",
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

<span data-ttu-id="3e274-228">**Azure Search utdatauppsättningen:**</span><span class="sxs-lookup"><span data-stu-id="3e274-228">**Azure Search output dataset:**</span></span>

<span data-ttu-id="3e274-229">hello exempel kopior data tooan Azure Search index med namnet **produkter**.</span><span class="sxs-lookup"><span data-stu-id="3e274-229">hello sample copies data tooan Azure Search index named **products**.</span></span> <span data-ttu-id="3e274-230">Data Factory skapar inte hello index.</span><span class="sxs-lookup"><span data-stu-id="3e274-230">Data Factory does not create hello index.</span></span> <span data-ttu-id="3e274-231">tootest hello exempel, skapa ett index med det här namnet.</span><span class="sxs-lookup"><span data-stu-id="3e274-231">tootest hello sample, create an index with this name.</span></span> <span data-ttu-id="3e274-232">Skapa hello Azure Search index med hello samma antal kolumner som hello inkommande dataset.</span><span class="sxs-lookup"><span data-stu-id="3e274-232">Create hello Azure Search index with hello same number of columns as in hello input dataset.</span></span> <span data-ttu-id="3e274-233">Nya poster läggs toohello Azure Search index varje timme.</span><span class="sxs-lookup"><span data-stu-id="3e274-233">New entries are added toohello Azure Search index every hour.</span></span>

```JSON
{
    "name": "AzureSearchIndexDataset",
    "properties": {
        "type": "AzureSearchIndex",
        "linkedServiceName": "AzureSearchLinkedService",
        "typeProperties" : {
            "indexName": "products",
        },
        "availability": {
            "frequency": "Minute",
            "interval": 15
        }
   }
}
```

<span data-ttu-id="3e274-234">**Kopiera aktivitet i en pipeline med SQL-källa och mottagare för Azure Search Index:**</span><span class="sxs-lookup"><span data-stu-id="3e274-234">**Copy activity in a pipeline with SQL source and Azure Search Index sink:**</span></span>

<span data-ttu-id="3e274-235">hello pipelinen innehåller en kopia-aktivitet som är konfigurerade toouse hello inkommande och utgående datauppsättningar och är schemalagda toorun varje timme.</span><span class="sxs-lookup"><span data-stu-id="3e274-235">hello pipeline contains a Copy Activity that is configured toouse hello input and output datasets and is scheduled toorun every hour.</span></span> <span data-ttu-id="3e274-236">I hello pipeline JSON-definitionen hello **källa** typ har angetts för**SqlSource** och **sink** typ har angetts för**AzureSearchIndexSink**.</span><span class="sxs-lookup"><span data-stu-id="3e274-236">In hello pipeline JSON definition, hello **source** type is set too**SqlSource** and **sink** type is set too**AzureSearchIndexSink**.</span></span> <span data-ttu-id="3e274-237">hello SQL-frågan som angetts för hello **SqlReaderQuery** egenskapen väljer hello data i hello tidigare timme toocopy.</span><span class="sxs-lookup"><span data-stu-id="3e274-237">hello SQL query specified for hello **SqlReaderQuery** property selects hello data in hello past hour toocopy.</span></span>

```JSON
{  
    "name":"SamplePipeline",
    "properties":{  
    "start":"2014-06-01T18:00:00",
    "end":"2014-06-01T19:00:00",
    "description":"pipeline for copy activity",
    "activities":[  
      {
        "name": "SqlServertoAzureSearchIndex",
        "description": "copy activity",
        "type": "Copy",
        "inputs": [
          {
            "name": " SqlServerInput"
          }
        ],
        "outputs": [
          {
            "name": "AzureSearchIndexDataset"
          }
        ],
        "typeProperties": {
          "source": {
            "type": "SqlSource",
            "SqlReaderQuery": "$$Text.Format('select * from MyTable where timestampcolumn >= \\'{0:yyyy-MM-dd HH:mm}\\' AND timestampcolumn < \\'{1:yyyy-MM-dd HH:mm}\\'', WindowStart, WindowEnd)"
          },
          "sink": {
            "type": "AzureSearchIndexSink"
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

<span data-ttu-id="3e274-238">Om du vill kopiera data från ett moln-datalager i Azure Search `executionLocation` egenskapen måste anges.</span><span class="sxs-lookup"><span data-stu-id="3e274-238">If you are copying data from a cloud data store into Azure Search, `executionLocation` property is required.</span></span> <span data-ttu-id="3e274-239">hello följande JSON utdrag visar hello ändring behövs under Kopieringsaktiviteten `typeProperties` som exempel.</span><span class="sxs-lookup"><span data-stu-id="3e274-239">hello following JSON snippet shows hello change needed under Copy Activity `typeProperties` as an example.</span></span> <span data-ttu-id="3e274-240">Kontrollera [kopiera data mellan moln datalager](data-factory-data-movement-activities.md#global) avsnitt för mer information och de värden som stöds.</span><span class="sxs-lookup"><span data-stu-id="3e274-240">Check [Copy data between cloud data stores](data-factory-data-movement-activities.md#global) section for supported values and more details.</span></span>

```JSON
"typeProperties": {
  "source": {
    "type": "BlobSource"
  },
  "sink": {
    "type": "AzureSearchIndexSink"
  },
  "executionLocation": "West US"
}
```


## <a name="copy-from-a-cloud-source"></a><span data-ttu-id="3e274-241">Kopiera från en källa för molnet</span><span class="sxs-lookup"><span data-stu-id="3e274-241">Copy from a cloud source</span></span>
<span data-ttu-id="3e274-242">Om du vill kopiera data från ett moln-datalager i Azure Search `executionLocation` egenskapen måste anges.</span><span class="sxs-lookup"><span data-stu-id="3e274-242">If you are copying data from a cloud data store into Azure Search, `executionLocation` property is required.</span></span> <span data-ttu-id="3e274-243">hello följande JSON utdrag visar hello ändring behövs under Kopieringsaktiviteten `typeProperties` som exempel.</span><span class="sxs-lookup"><span data-stu-id="3e274-243">hello following JSON snippet shows hello change needed under Copy Activity `typeProperties` as an example.</span></span> <span data-ttu-id="3e274-244">Kontrollera [kopiera data mellan moln datalager](data-factory-data-movement-activities.md#global) avsnitt för mer information och de värden som stöds.</span><span class="sxs-lookup"><span data-stu-id="3e274-244">Check [Copy data between cloud data stores](data-factory-data-movement-activities.md#global) section for supported values and more details.</span></span>

```JSON
"typeProperties": {
  "source": {
    "type": "BlobSource"
  },
  "sink": {
    "type": "AzureSearchIndexSink"
  },
  "executionLocation": "West US"
}
```

<span data-ttu-id="3e274-245">Du kan också mappa kolumner från källan dataset toocolumns från sink datauppsättning i aktivitetsdefinitionen för hello kopia.</span><span class="sxs-lookup"><span data-stu-id="3e274-245">You can also map columns from source dataset toocolumns from sink dataset in hello copy activity definition.</span></span> <span data-ttu-id="3e274-246">Mer information finns i [mappa dataset kolumner i Azure Data Factory](data-factory-map-columns.md).</span><span class="sxs-lookup"><span data-stu-id="3e274-246">For details, see [Mapping dataset columns in Azure Data Factory](data-factory-map-columns.md).</span></span>

## <a name="performance-and-tuning"></a><span data-ttu-id="3e274-247">Prestanda- och justering</span><span class="sxs-lookup"><span data-stu-id="3e274-247">Performance and tuning</span></span>  
<span data-ttu-id="3e274-248">Se hello [prestandajustering guide och Kopieringsaktivitet prestanda](data-factory-copy-activity-performance.md) toolearn om nyckeln faktorer som effekten av dataflyttning (Kopieringsaktiviteten) och olika sätt toooptimize den.</span><span class="sxs-lookup"><span data-stu-id="3e274-248">See hello [Copy Activity performance and tuning guide](data-factory-copy-activity-performance.md) toolearn about key factors that impact performance of data movement (Copy Activity) and various ways toooptimize it.</span></span>

## <a name="next-steps"></a><span data-ttu-id="3e274-249">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="3e274-249">Next steps</span></span>
<span data-ttu-id="3e274-250">Se hello följande artiklar:</span><span class="sxs-lookup"><span data-stu-id="3e274-250">See hello following articles:</span></span>

* <span data-ttu-id="3e274-251">[Kopiera aktivitet kursen](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) stegvisa instruktioner för att skapa en pipeline med en kopia-aktivitet.</span><span class="sxs-lookup"><span data-stu-id="3e274-251">[Copy Activity tutorial](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) for step-by-step instructions for creating a pipeline with a Copy Activity.</span></span>
