---
title: "Flytta data från Amazon enkla Storage-tjänsten med hjälp av Data Factory | Microsoft Docs"
description: "Lär dig mer om hur du flyttar data från Amazon enkla lagringstjänst (S3) med hjälp av Azure Data Factory."
services: data-factory
documentationcenter: 
author: linda33wj
manager: jhubbard
editor: monicar
ms.assetid: 636d3179-eba8-4841-bcb4-3563f6822a26
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/20/2017
ms.author: jingwang
ms.openlocfilehash: 3e21f7dfccc3b235071344a28c7d94f65e6bf9ac
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="move-data-from-amazon-simple-storage-service-by-using-azure-data-factory"></a><span data-ttu-id="a8262-103">Flytta data från Amazon enkla Storage-tjänsten med hjälp av Azure Data Factory</span><span class="sxs-lookup"><span data-stu-id="a8262-103">Move data from Amazon Simple Storage Service by using Azure Data Factory</span></span>
<span data-ttu-id="a8262-104">Den här artikeln förklarar hur du använder kopieringsaktiviteten i Azure Data Factory för att flytta data från Amazon enkla Storage-tjänst (S3).</span><span class="sxs-lookup"><span data-stu-id="a8262-104">This article explains how to use the copy activity in Azure Data Factory to move data from Amazon Simple Storage Service (S3).</span></span> <span data-ttu-id="a8262-105">Den bygger på den [Data movement aktiviteter](data-factory-data-movement-activities.md) artikel som presenterar en allmän översikt över dataflyttning med copy-aktivitet.</span><span class="sxs-lookup"><span data-stu-id="a8262-105">It builds on the [Data movement activities](data-factory-data-movement-activities.md) article, which presents a general overview of data movement with the copy activity.</span></span>

<span data-ttu-id="a8262-106">Du kan kopiera data från Amazon S3 till alla stöds sink-datalagret.</span><span class="sxs-lookup"><span data-stu-id="a8262-106">You can copy data from Amazon S3 to any supported sink data store.</span></span> <span data-ttu-id="a8262-107">En lista över datakällor som stöds som sänkor av kopieringsaktiviteten, finns det [stöds datalager](data-factory-data-movement-activities.md#supported-data-stores-and-formats) tabell.</span><span class="sxs-lookup"><span data-stu-id="a8262-107">For a list of data stores supported as sinks by the copy activity, see the [Supported data stores](data-factory-data-movement-activities.md#supported-data-stores-and-formats) table.</span></span> <span data-ttu-id="a8262-108">Data Factory stöder för närvarande endast flytta data från Amazon S3 till andra databaser, men inte flytta data från andra data lagras på Amazon S3.</span><span class="sxs-lookup"><span data-stu-id="a8262-108">Data Factory currently supports only moving data from Amazon S3 to other data stores, but not moving data from other data stores to Amazon S3.</span></span>

## <a name="required-permissions"></a><span data-ttu-id="a8262-109">Behörigheter som krävs</span><span class="sxs-lookup"><span data-stu-id="a8262-109">Required permissions</span></span>
<span data-ttu-id="a8262-110">Om du vill kopiera data från Amazon S3, kontrollera att du har beviljats följande behörigheter:</span><span class="sxs-lookup"><span data-stu-id="a8262-110">To copy data from Amazon S3, make sure you have been granted the following permissions:</span></span>

* <span data-ttu-id="a8262-111">`s3:GetObject`och `s3:GetObjectVersion` för Amazon S3 objekt åtgärder.</span><span class="sxs-lookup"><span data-stu-id="a8262-111">`s3:GetObject` and `s3:GetObjectVersion` for Amazon S3 Object Operations.</span></span>
* <span data-ttu-id="a8262-112">`s3:ListBucket`för Amazon S3 Bucket-åtgärder.</span><span class="sxs-lookup"><span data-stu-id="a8262-112">`s3:ListBucket` for Amazon S3 Bucket Operations.</span></span> <span data-ttu-id="a8262-113">Om du använder guiden Data Factory kopiera `s3:ListAllMyBuckets` krävs också.</span><span class="sxs-lookup"><span data-stu-id="a8262-113">If you are using the Data Factory Copy Wizard, `s3:ListAllMyBuckets` is also required.</span></span>

<span data-ttu-id="a8262-114">Mer information om en fullständig lista över Amazon S3 behörigheter finns [att ange behörigheter i en princip](http://docs.aws.amazon.com/AmazonS3/latest/dev/using-with-s3-actions.html).</span><span class="sxs-lookup"><span data-stu-id="a8262-114">For details about the full list of Amazon S3 permissions, see [Specifying Permissions in a Policy](http://docs.aws.amazon.com/AmazonS3/latest/dev/using-with-s3-actions.html).</span></span>

## <a name="getting-started"></a><span data-ttu-id="a8262-115">Komma igång</span><span class="sxs-lookup"><span data-stu-id="a8262-115">Getting started</span></span>
<span data-ttu-id="a8262-116">Du kan skapa en pipeline med en kopia-aktivitet som flyttar data från en källa för Amazon S3 med hjälp av olika verktyg eller API: er.</span><span class="sxs-lookup"><span data-stu-id="a8262-116">You can create a pipeline with a copy activity that moves data from an Amazon S3 source by using different tools or APIs.</span></span>

<span data-ttu-id="a8262-117">Det enklaste sättet att skapa en pipeline är att använda den **guiden Kopiera**.</span><span class="sxs-lookup"><span data-stu-id="a8262-117">The easiest way to create a pipeline is to use the **Copy Wizard**.</span></span> <span data-ttu-id="a8262-118">För en snabb genomgång, se [Självstudier: skapa en pipeline med hjälp av guiden Kopiera](data-factory-copy-data-wizard-tutorial.md).</span><span class="sxs-lookup"><span data-stu-id="a8262-118">For a quick walkthrough, see [Tutorial: Create a pipeline using Copy Wizard](data-factory-copy-data-wizard-tutorial.md).</span></span>

<span data-ttu-id="a8262-119">Du kan också använda följande verktyg för att skapa en pipeline: **Azure-portalen**, **Visual Studio**, **Azure PowerShell**, **Azure Resource Manager-mall**, **.NET API**, och **REST API**.</span><span class="sxs-lookup"><span data-stu-id="a8262-119">You can also use the following tools to create a pipeline: **Azure portal**, **Visual Studio**, **Azure PowerShell**, **Azure Resource Manager template**, **.NET API**, and **REST API**.</span></span> <span data-ttu-id="a8262-120">Stegvisa instruktioner för att skapa en pipeline med en kopieringsaktiviteten, finns det [kopiera aktivitet kursen](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md).</span><span class="sxs-lookup"><span data-stu-id="a8262-120">For step-by-step instructions to create a pipeline with a copy activity, see the [Copy activity tutorial](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md).</span></span>

<span data-ttu-id="a8262-121">Om du använder verktyg eller API: er, kan du utföra följande steg för att skapa en pipeline som flyttar data från ett dataarkiv som källa till ett dataarkiv som mottagare:</span><span class="sxs-lookup"><span data-stu-id="a8262-121">Whether you use tools or APIs, you perform the following steps to create a pipeline that moves data from a source data store to a sink data store:</span></span>

1. <span data-ttu-id="a8262-122">Skapa **länkade tjänster** att länka inkommande och utgående data lagras till din data factory.</span><span class="sxs-lookup"><span data-stu-id="a8262-122">Create **linked services** to link input and output data stores to your data factory.</span></span>
2. <span data-ttu-id="a8262-123">Skapa **datauppsättningar** att representera inkommande och utgående data för kopieringen.</span><span class="sxs-lookup"><span data-stu-id="a8262-123">Create **datasets** to represent input and output data for the copy operation.</span></span>
3. <span data-ttu-id="a8262-124">Skapa en **pipeline** med en kopia-aktivitet som tar en datamängd som indata och en dataset som utdata.</span><span class="sxs-lookup"><span data-stu-id="a8262-124">Create a **pipeline** with a copy activity that takes a dataset as an input and a dataset as an output.</span></span>

<span data-ttu-id="a8262-125">När du använder guiden skapas JSON definitioner för dessa Data Factory-enheter (länkade tjänster, datauppsättningar och pipelinen) automatiskt för dig.</span><span class="sxs-lookup"><span data-stu-id="a8262-125">When you use the wizard, JSON definitions for these Data Factory entities (linked services, datasets, and the pipeline) are automatically created for you.</span></span> <span data-ttu-id="a8262-126">När du använder verktyg eller API: er (utom .NET API), kan du definiera dessa Data Factory-enheter med hjälp av JSON-format.</span><span class="sxs-lookup"><span data-stu-id="a8262-126">When you use tools or APIs (except .NET API), you define these Data Factory entities by using the JSON format.</span></span> <span data-ttu-id="a8262-127">Ett exempel med JSON-definitioner för Data Factory-entiteter som används för att kopiera data från ett dataarkiv för Amazon S3 finns i [JSON-exempel: kopiera data från Amazon S3 till Azure Blob](#json-example-copy-data-from-amazon-s3-to-azure-blob) i den här artikeln.</span><span class="sxs-lookup"><span data-stu-id="a8262-127">For a sample with JSON definitions for Data Factory entities that are used to copy data from an Amazon S3 data store, see the [JSON example: Copy data from Amazon S3 to Azure Blob](#json-example-copy-data-from-amazon-s3-to-azure-blob) section of this article.</span></span>

> [!NOTE]
> <span data-ttu-id="a8262-128">Mer information om fil- och komprimering de format som stöds för en kopieringsaktiviteten finns [format och komprimering i Azure Data Factory](data-factory-supported-file-and-compression-formats.md).</span><span class="sxs-lookup"><span data-stu-id="a8262-128">For details about supported file and compression formats for a copy activity, see [File and compression formats in Azure Data Factory](data-factory-supported-file-and-compression-formats.md).</span></span>

<span data-ttu-id="a8262-129">Följande avsnitt innehåller information om JSON-egenskaper som används för att definiera Data Factory entiteter för Amazon S3.</span><span class="sxs-lookup"><span data-stu-id="a8262-129">The following sections provide details about JSON properties that are used to define Data Factory entities specific to Amazon S3.</span></span>

## <a name="linked-service-properties"></a><span data-ttu-id="a8262-130">Länkad tjänstegenskaper</span><span class="sxs-lookup"><span data-stu-id="a8262-130">Linked service properties</span></span>
<span data-ttu-id="a8262-131">En länkad tjänst länkar ett datalager till en data factory.</span><span class="sxs-lookup"><span data-stu-id="a8262-131">A linked service links a data store to a data factory.</span></span> <span data-ttu-id="a8262-132">Du skapar en länkad tjänst av typen **AwsAccessKey** länka Amazon S3 datalager till din data factory.</span><span class="sxs-lookup"><span data-stu-id="a8262-132">You create a linked service of type **AwsAccessKey** to link your Amazon S3 data store to your data factory.</span></span> <span data-ttu-id="a8262-133">Följande tabell innehåller beskrivning för JSON-element som är specifika för Amazon S3 (AwsAccessKey) länkade tjänsten.</span><span class="sxs-lookup"><span data-stu-id="a8262-133">The following table provides description for JSON elements specific to Amazon S3 (AwsAccessKey) linked service.</span></span>

| <span data-ttu-id="a8262-134">Egenskap</span><span class="sxs-lookup"><span data-stu-id="a8262-134">Property</span></span> | <span data-ttu-id="a8262-135">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="a8262-135">Description</span></span> | <span data-ttu-id="a8262-136">Tillåtna värden</span><span class="sxs-lookup"><span data-stu-id="a8262-136">Allowed values</span></span> | <span data-ttu-id="a8262-137">Krävs</span><span class="sxs-lookup"><span data-stu-id="a8262-137">Required</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="a8262-138">accessKeyID</span><span class="sxs-lookup"><span data-stu-id="a8262-138">accessKeyID</span></span> |<span data-ttu-id="a8262-139">ID för den hemliga åtkomstnyckeln.</span><span class="sxs-lookup"><span data-stu-id="a8262-139">ID of the secret access key.</span></span> |<span data-ttu-id="a8262-140">Sträng</span><span class="sxs-lookup"><span data-stu-id="a8262-140">string</span></span> |<span data-ttu-id="a8262-141">Ja</span><span class="sxs-lookup"><span data-stu-id="a8262-141">Yes</span></span> |
| <span data-ttu-id="a8262-142">secretAccessKey</span><span class="sxs-lookup"><span data-stu-id="a8262-142">secretAccessKey</span></span> |<span data-ttu-id="a8262-143">Hemlig själva åtkomstnyckeln.</span><span class="sxs-lookup"><span data-stu-id="a8262-143">The secret access key itself.</span></span> |<span data-ttu-id="a8262-144">Krypterad hemliga sträng</span><span class="sxs-lookup"><span data-stu-id="a8262-144">Encrypted secret string</span></span> |<span data-ttu-id="a8262-145">Ja</span><span class="sxs-lookup"><span data-stu-id="a8262-145">Yes</span></span> |

<span data-ttu-id="a8262-146">Här är ett exempel:</span><span class="sxs-lookup"><span data-stu-id="a8262-146">Here is an example:</span></span>

```json
{
    "name": "AmazonS3LinkedService",
    "properties": {
        "type": "AwsAccessKey",
        "typeProperties": {
            "accessKeyId": "<access key id>",
            "secretAccessKey": "<secret access key>"
        }
    }
}
```

## <a name="dataset-properties"></a><span data-ttu-id="a8262-147">Egenskaper för datamängd</span><span class="sxs-lookup"><span data-stu-id="a8262-147">Dataset properties</span></span>
<span data-ttu-id="a8262-148">Om du vill ange en datamängd som representerar indata i Azure Blob storage ange egenskapen type för datauppsättningen till **AmazonS3**.</span><span class="sxs-lookup"><span data-stu-id="a8262-148">To specify a dataset to represent input data in Azure Blob storage, set the type property of the dataset to **AmazonS3**.</span></span> <span data-ttu-id="a8262-149">Ange den **linkedServiceName** -egenskapen för datauppsättningen till namnet på Amazon S3 länkade tjänsten.</span><span class="sxs-lookup"><span data-stu-id="a8262-149">Set the **linkedServiceName** property of the dataset to the name of the Amazon S3 linked service.</span></span> <span data-ttu-id="a8262-150">En fullständig lista över avsnitt och egenskaper som är tillgängliga för att definiera datauppsättningar finns [skapa datauppsättningar](data-factory-create-datasets.md).</span><span class="sxs-lookup"><span data-stu-id="a8262-150">For a full list of sections and properties available for defining datasets, see [Creating datasets](data-factory-create-datasets.md).</span></span> 

<span data-ttu-id="a8262-151">Avsnitt som struktur, tillgänglighet och principen är liknande för alla dataset-typer (till exempel SQL-databas, Azure blob och Azure-tabellen).</span><span class="sxs-lookup"><span data-stu-id="a8262-151">Sections such as structure, availability, and policy are similar for all dataset types (such as SQL database, Azure blob, and Azure table).</span></span> <span data-ttu-id="a8262-152">Den **typeProperties** avsnitt är olika för varje typ av dataset och innehåller information om placeringen av data i datalagret.</span><span class="sxs-lookup"><span data-stu-id="a8262-152">The **typeProperties** section is different for each type of dataset, and provides information about the location of the data in the data store.</span></span> <span data-ttu-id="a8262-153">Den **typeProperties** avsnittet för en dataset av typen **AmazonS3** (som omfattar Amazon S3 dataset) har följande egenskaper:</span><span class="sxs-lookup"><span data-stu-id="a8262-153">The **typeProperties** section for a dataset of type **AmazonS3** (which includes the Amazon S3 dataset) has the following properties:</span></span>

| <span data-ttu-id="a8262-154">Egenskap</span><span class="sxs-lookup"><span data-stu-id="a8262-154">Property</span></span> | <span data-ttu-id="a8262-155">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="a8262-155">Description</span></span> | <span data-ttu-id="a8262-156">Tillåtna värden</span><span class="sxs-lookup"><span data-stu-id="a8262-156">Allowed values</span></span> | <span data-ttu-id="a8262-157">Krävs</span><span class="sxs-lookup"><span data-stu-id="a8262-157">Required</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="a8262-158">bucketName</span><span class="sxs-lookup"><span data-stu-id="a8262-158">bucketName</span></span> |<span data-ttu-id="a8262-159">S3-Bucketnamn.</span><span class="sxs-lookup"><span data-stu-id="a8262-159">The S3 bucket name.</span></span> |<span data-ttu-id="a8262-160">Sträng</span><span class="sxs-lookup"><span data-stu-id="a8262-160">String</span></span> |<span data-ttu-id="a8262-161">Ja</span><span class="sxs-lookup"><span data-stu-id="a8262-161">Yes</span></span> |
| <span data-ttu-id="a8262-162">key</span><span class="sxs-lookup"><span data-stu-id="a8262-162">key</span></span> |<span data-ttu-id="a8262-163">S3 objekt nyckeln.</span><span class="sxs-lookup"><span data-stu-id="a8262-163">The S3 object key.</span></span> |<span data-ttu-id="a8262-164">Sträng</span><span class="sxs-lookup"><span data-stu-id="a8262-164">String</span></span> |<span data-ttu-id="a8262-165">Nej</span><span class="sxs-lookup"><span data-stu-id="a8262-165">No</span></span> |
| <span data-ttu-id="a8262-166">prefix</span><span class="sxs-lookup"><span data-stu-id="a8262-166">prefix</span></span> |<span data-ttu-id="a8262-167">Prefix för nyckeln S3 objekt.</span><span class="sxs-lookup"><span data-stu-id="a8262-167">Prefix for the S3 object key.</span></span> <span data-ttu-id="a8262-168">Objekt vars nycklar som börjar med prefixet är markerade.</span><span class="sxs-lookup"><span data-stu-id="a8262-168">Objects whose keys start with this prefix are selected.</span></span> <span data-ttu-id="a8262-169">Gäller endast när nyckeln är tom.</span><span class="sxs-lookup"><span data-stu-id="a8262-169">Applies only when key is empty.</span></span> |<span data-ttu-id="a8262-170">Sträng</span><span class="sxs-lookup"><span data-stu-id="a8262-170">String</span></span> |<span data-ttu-id="a8262-171">Nej</span><span class="sxs-lookup"><span data-stu-id="a8262-171">No</span></span> |
| <span data-ttu-id="a8262-172">Version</span><span class="sxs-lookup"><span data-stu-id="a8262-172">version</span></span> |<span data-ttu-id="a8262-173">Versionen av objektet S3 om S3 versionshantering är aktiverad.</span><span class="sxs-lookup"><span data-stu-id="a8262-173">The version of the S3 object, if S3 versioning is enabled.</span></span> |<span data-ttu-id="a8262-174">Sträng</span><span class="sxs-lookup"><span data-stu-id="a8262-174">String</span></span> |<span data-ttu-id="a8262-175">Nej</span><span class="sxs-lookup"><span data-stu-id="a8262-175">No</span></span> |
| <span data-ttu-id="a8262-176">Format</span><span class="sxs-lookup"><span data-stu-id="a8262-176">format</span></span> | <span data-ttu-id="a8262-177">Följande format stöds: **TextFormat**, **JsonFormat**, **AvroFormat**, **OrcFormat**, **ParquetFormat**.</span><span class="sxs-lookup"><span data-stu-id="a8262-177">The following format types are supported: **TextFormat**, **JsonFormat**, **AvroFormat**, **OrcFormat**, **ParquetFormat**.</span></span> <span data-ttu-id="a8262-178">Ange den **typen** egenskap under format till ett av dessa värden.</span><span class="sxs-lookup"><span data-stu-id="a8262-178">Set the **type** property under format to one of these values.</span></span> <span data-ttu-id="a8262-179">Mer information finns i [textformat](data-factory-supported-file-and-compression-formats.md#text-format), [JSON-format](data-factory-supported-file-and-compression-formats.md#json-format), [Avro-formatet](data-factory-supported-file-and-compression-formats.md#avro-format), [Orc format](data-factory-supported-file-and-compression-formats.md#orc-format), och [parkettgolv format ](data-factory-supported-file-and-compression-formats.md#parquet-format) avsnitt.</span><span class="sxs-lookup"><span data-stu-id="a8262-179">For more information, see the [Text format](data-factory-supported-file-and-compression-formats.md#text-format), [JSON format](data-factory-supported-file-and-compression-formats.md#json-format), [Avro format](data-factory-supported-file-and-compression-formats.md#avro-format), [Orc format](data-factory-supported-file-and-compression-formats.md#orc-format), and [Parquet format](data-factory-supported-file-and-compression-formats.md#parquet-format) sections.</span></span> <br><br> <span data-ttu-id="a8262-180">Om du vill kopiera filer som-är mellan filbaserade butiker (binär kopia), hoppa över avsnittet format i både inkommande och utgående dataset-definitioner.</span><span class="sxs-lookup"><span data-stu-id="a8262-180">If you want to copy files as-is between file-based stores (binary copy), skip the format section in both input and output dataset definitions.</span></span> |<span data-ttu-id="a8262-181">Nej</span><span class="sxs-lookup"><span data-stu-id="a8262-181">No</span></span> | |
| <span data-ttu-id="a8262-182">Komprimering</span><span class="sxs-lookup"><span data-stu-id="a8262-182">compression</span></span> | <span data-ttu-id="a8262-183">Ange typ och kompression för data.</span><span class="sxs-lookup"><span data-stu-id="a8262-183">Specify the type and level of compression for the data.</span></span> <span data-ttu-id="a8262-184">Typer som stöds är: **GZip**, **Deflate**, **BZip2**, och **ZipDeflate**.</span><span class="sxs-lookup"><span data-stu-id="a8262-184">The supported types are: **GZip**, **Deflate**, **BZip2**, and **ZipDeflate**.</span></span> <span data-ttu-id="a8262-185">Nivåerna som stöds är: **Optimal** och **snabbast**.</span><span class="sxs-lookup"><span data-stu-id="a8262-185">The supported levels are: **Optimal** and **Fastest**.</span></span> <span data-ttu-id="a8262-186">Mer information finns i [format och komprimering i Azure Data Factory](data-factory-supported-file-and-compression-formats.md#compression-support).</span><span class="sxs-lookup"><span data-stu-id="a8262-186">For more information, see [File and compression formats in Azure Data Factory](data-factory-supported-file-and-compression-formats.md#compression-support).</span></span> |<span data-ttu-id="a8262-187">Nej</span><span class="sxs-lookup"><span data-stu-id="a8262-187">No</span></span> | |


> [!NOTE]
> <span data-ttu-id="a8262-188">**bucketName + nyckeln** anger platsen för S3 objekt, där bucket är Rotbehållare för S3 objekt och nyckeln är den fullständiga sökvägen till S3 objekt.</span><span class="sxs-lookup"><span data-stu-id="a8262-188">**bucketName + key** specifies the location of the S3 object, where bucket is the root container for S3 objects, and key is the full path to the S3 object.</span></span>

### <a name="sample-dataset-with-prefix"></a><span data-ttu-id="a8262-189">Exempeldatamängd med prefix</span><span class="sxs-lookup"><span data-stu-id="a8262-189">Sample dataset with prefix</span></span>

```json
{
    "name": "dataset-s3",
    "properties": {
        "type": "AmazonS3",
        "linkedServiceName": "link- testS3",
        "typeProperties": {
            "prefix": "testFolder/test",
            "bucketName": "testbucket",
            "format": {
                "type": "OrcFormat"
            }
        },
        "availability": {
            "frequency": "Hour",
            "interval": 1
        },
        "external": true
    }
}
```
### <a name="sample-dataset-with-version"></a><span data-ttu-id="a8262-190">Exempeldatamängd (med version)</span><span class="sxs-lookup"><span data-stu-id="a8262-190">Sample dataset (with version)</span></span>

```json
{
    "name": "dataset-s3",
    "properties": {
        "type": "AmazonS3",
        "linkedServiceName": "link- testS3",
        "typeProperties": {
            "key": "testFolder/test.orc",
            "bucketName": "testbucket",
            "version": "XXXXXXXXXczm0CJajYkHf0_k6LhBmkcL",
            "format": {
                "type": "OrcFormat"
            }
        },
        "availability": {
            "frequency": "Hour",
            "interval": 1
        },
        "external": true
    }
}
```

### <a name="dynamic-paths-for-s3"></a><span data-ttu-id="a8262-191">Dynamisk sökvägar för S3</span><span class="sxs-lookup"><span data-stu-id="a8262-191">Dynamic paths for S3</span></span>
<span data-ttu-id="a8262-192">Föregående exempel använder fasta värden för den **nyckeln** och **bucketName** egenskaper i Amazon S3 datamängden.</span><span class="sxs-lookup"><span data-stu-id="a8262-192">The preceding sample uses fixed values for the **key** and **bucketName** properties in the Amazon S3 dataset.</span></span>

```json
"key": "testFolder/test.orc",
"bucketName": "testbucket",
```

<span data-ttu-id="a8262-193">Du kan ha Data Factory beräkna egenskaperna dynamiskt vid körning, med hjälp av systemvariabler som SliceStart.</span><span class="sxs-lookup"><span data-stu-id="a8262-193">You can have Data Factory calculate these properties dynamically at runtime, by using system variables such as SliceStart.</span></span>

```json
"key": "$$Text.Format('{0:MM}/{0:dd}/test.orc', SliceStart)"
"bucketName": "$$Text.Format('{0:yyyy}', SliceStart)"
```

<span data-ttu-id="a8262-194">Du kan göra samma för den **prefix** -egenskapen för en Amazon S3 datauppsättning.</span><span class="sxs-lookup"><span data-stu-id="a8262-194">You can do the same for the **prefix** property of an Amazon S3 dataset.</span></span> <span data-ttu-id="a8262-195">En lista över funktioner som stöds och variabler, se [Data Factory-funktioner och systemvariabler](data-factory-functions-variables.md).</span><span class="sxs-lookup"><span data-stu-id="a8262-195">For a list of supported functions and variables, see [Data Factory functions and system variables](data-factory-functions-variables.md).</span></span>

## <a name="copy-activity-properties"></a><span data-ttu-id="a8262-196">Kopiera egenskaper för aktivitet</span><span class="sxs-lookup"><span data-stu-id="a8262-196">Copy activity properties</span></span>
<span data-ttu-id="a8262-197">En fullständig lista över avsnitt och egenskaper som är tillgängliga för att definiera aktiviteter, se [skapar pipelines](data-factory-create-pipelines.md).</span><span class="sxs-lookup"><span data-stu-id="a8262-197">For a full list of sections and properties available for defining activities, see [Creating pipelines](data-factory-create-pipelines.md).</span></span> <span data-ttu-id="a8262-198">Egenskaper som namn, beskrivning, ingående och utgående tabeller och principer är tillgängliga för alla typer av aktiviteter.</span><span class="sxs-lookup"><span data-stu-id="a8262-198">Properties such as name, description, input and output tables, and policies are available for all types of activities.</span></span> <span data-ttu-id="a8262-199">Egenskaper som är tillgängliga i den **typeProperties** avsnitt i aktiviteten varierar med varje aktivitetstyp.</span><span class="sxs-lookup"><span data-stu-id="a8262-199">Properties available in the **typeProperties** section of the activity vary with each activity type.</span></span> <span data-ttu-id="a8262-200">För kopieringsaktiviteten variera egenskaperna beroende på vilka typer av datakällor och sänkor.</span><span class="sxs-lookup"><span data-stu-id="a8262-200">For the copy activity, properties vary depending on the types of sources and sinks.</span></span> <span data-ttu-id="a8262-201">När en källa i kopieringsaktiviteten är av typen **FileSystemSource** (som omfattar Amazon S3), av följande egenskap är tillgänglig i **typeProperties** avsnitt:</span><span class="sxs-lookup"><span data-stu-id="a8262-201">When a source in the copy activity is of type **FileSystemSource** (which includes Amazon S3), the following property is available in **typeProperties** section:</span></span>

| <span data-ttu-id="a8262-202">Egenskap</span><span class="sxs-lookup"><span data-stu-id="a8262-202">Property</span></span> | <span data-ttu-id="a8262-203">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="a8262-203">Description</span></span> | <span data-ttu-id="a8262-204">Tillåtna värden</span><span class="sxs-lookup"><span data-stu-id="a8262-204">Allowed values</span></span> | <span data-ttu-id="a8262-205">Krävs</span><span class="sxs-lookup"><span data-stu-id="a8262-205">Required</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="a8262-206">Rekursiva</span><span class="sxs-lookup"><span data-stu-id="a8262-206">recursive</span></span> |<span data-ttu-id="a8262-207">Anger om att rekursivt listan S3 objekt i katalogen.</span><span class="sxs-lookup"><span data-stu-id="a8262-207">Specifies whether to recursively list S3 objects under the directory.</span></span> |<span data-ttu-id="a8262-208">SANT/FALSKT</span><span class="sxs-lookup"><span data-stu-id="a8262-208">true/false</span></span> |<span data-ttu-id="a8262-209">Nej</span><span class="sxs-lookup"><span data-stu-id="a8262-209">No</span></span> |

## <a name="json-example-copy-data-from-amazon-s3-to-azure-blob-storage"></a><span data-ttu-id="a8262-210">JSON-exempel: kopiera data från Amazon S3 till Azure Blob storage</span><span class="sxs-lookup"><span data-stu-id="a8262-210">JSON example: Copy data from Amazon S3 to Azure Blob storage</span></span>
<span data-ttu-id="a8262-211">Det här exemplet visas hur du kopierar data från Amazon S3 till ett Azure Blob storage.</span><span class="sxs-lookup"><span data-stu-id="a8262-211">This sample shows how to copy data from Amazon S3 to an Azure Blob storage.</span></span> <span data-ttu-id="a8262-212">Dock datan kan kopieras direkt till [någon sänkor som stöds](data-factory-data-movement-activities.md#supported-data-stores-and-formats) med hjälp av kopieringsaktiviteten i Data Factory.</span><span class="sxs-lookup"><span data-stu-id="a8262-212">However, data can be copied directly to [any of the sinks that are supported](data-factory-data-movement-activities.md#supported-data-stores-and-formats) by using the copy activity in Data Factory.</span></span>

<span data-ttu-id="a8262-213">Exemplet innehåller JSON definitioner för följande Data Factory-enheter.</span><span class="sxs-lookup"><span data-stu-id="a8262-213">The sample provides JSON definitions for the following Data Factory entities.</span></span> <span data-ttu-id="a8262-214">Du kan använda dessa definitioner för att skapa en pipeline för att kopiera data från Amazon S3 till Blob storage med hjälp av den [Azure-portalen](data-factory-copy-activity-tutorial-using-azure-portal.md), [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md), eller [PowerShell](data-factory-copy-activity-tutorial-using-powershell.md).</span><span class="sxs-lookup"><span data-stu-id="a8262-214">You can use these definitions to create a pipeline to copy data from Amazon S3 to Blob storage, by using the [Azure portal](data-factory-copy-activity-tutorial-using-azure-portal.md), [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md), or [PowerShell](data-factory-copy-activity-tutorial-using-powershell.md).</span></span>   

* <span data-ttu-id="a8262-215">En länkad tjänst av typen [AwsAccessKey](#linked-service-properties).</span><span class="sxs-lookup"><span data-stu-id="a8262-215">A linked service of type [AwsAccessKey](#linked-service-properties).</span></span>
* <span data-ttu-id="a8262-216">En länkad tjänst av typen [AzureStorage](data-factory-azure-blob-connector.md#linked-service-properties).</span><span class="sxs-lookup"><span data-stu-id="a8262-216">A linked service of type [AzureStorage](data-factory-azure-blob-connector.md#linked-service-properties).</span></span>
* <span data-ttu-id="a8262-217">Indata [dataset](data-factory-create-datasets.md) av typen [AmazonS3](#dataset-properties).</span><span class="sxs-lookup"><span data-stu-id="a8262-217">An input [dataset](data-factory-create-datasets.md) of type [AmazonS3](#dataset-properties).</span></span>
* <span data-ttu-id="a8262-218">Utdata [dataset](data-factory-create-datasets.md) av typen [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties).</span><span class="sxs-lookup"><span data-stu-id="a8262-218">An output [dataset](data-factory-create-datasets.md) of type [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties).</span></span>
* <span data-ttu-id="a8262-219">En [pipeline](data-factory-create-pipelines.md) med kopieringsaktiviteten som använder [FileSystemSource](#copy-activity-properties) och [BlobSink](data-factory-azure-blob-connector.md#copy-activity-properties).</span><span class="sxs-lookup"><span data-stu-id="a8262-219">A [pipeline](data-factory-create-pipelines.md) with copy activity that uses [FileSystemSource](#copy-activity-properties) and [BlobSink](data-factory-azure-blob-connector.md#copy-activity-properties).</span></span>

<span data-ttu-id="a8262-220">Exemplet kopierar data från Amazon S3 till en Azure blob varje timme.</span><span class="sxs-lookup"><span data-stu-id="a8262-220">The sample copies data from Amazon S3 to an Azure blob every hour.</span></span> <span data-ttu-id="a8262-221">JSON-egenskaper som används i exemplen beskrivs i exemplen i följande avsnitt.</span><span class="sxs-lookup"><span data-stu-id="a8262-221">The JSON properties used in these samples are described in sections following the samples.</span></span>

### <a name="amazon-s3-linked-service"></a><span data-ttu-id="a8262-222">Amazon S3 länkad tjänst</span><span class="sxs-lookup"><span data-stu-id="a8262-222">Amazon S3 linked service</span></span>

```json
{
    "name": "AmazonS3LinkedService",
    "properties": {
        "type": "AwsAccessKey",
        "typeProperties": {
            "accessKeyId": "<access key id>",
            "secretAccessKey": "<secret access key>"
        }
    }
}
```

### <a name="azure-storage-linked-service"></a><span data-ttu-id="a8262-223">Länkad Azure-lagringstjänst</span><span class="sxs-lookup"><span data-stu-id="a8262-223">Azure Storage linked service</span></span>

```json
{
  "name": "AzureStorageLinkedService",
  "properties": {
    "type": "AzureStorage",
    "typeProperties": {
      "connectionString": "DefaultEndpointsProtocol=https;AccountName=<accountname>;AccountKey=<accountkey>"
    }
  }
}
```

### <a name="amazon-s3-input-dataset"></a><span data-ttu-id="a8262-224">Amazon S3 inkommande dataset</span><span class="sxs-lookup"><span data-stu-id="a8262-224">Amazon S3 input dataset</span></span>

<span data-ttu-id="a8262-225">Ange **”externa”: true** informerar Data Factory-tjänsten att datamängden är extern till datafabriken.</span><span class="sxs-lookup"><span data-stu-id="a8262-225">Setting **"external": true** informs the Data Factory service that the dataset is external to the data factory.</span></span> <span data-ttu-id="a8262-226">Ange den här egenskapen till true för en inkommande datauppsättningen som inte tillverkas av en aktivitet i pipelinen.</span><span class="sxs-lookup"><span data-stu-id="a8262-226">Set this property to true on an input dataset that is not produced by an activity in the pipeline.</span></span>

```json
    {
        "name": "AmazonS3InputDataset",
        "properties": {
            "type": "AmazonS3",
            "linkedServiceName": "AmazonS3LinkedService",
            "typeProperties": {
                "key": "testFolder/test.orc",
                "bucketName": "testbucket",
                "format": {
                    "type": "OrcFormat"
                }
            },
            "availability": {
                "frequency": "Hour",
                "interval": 1
            },
            "external": true
        }
    }
```


### <a name="azure-blob-output-dataset"></a><span data-ttu-id="a8262-227">Utdatauppsättning för Azure-blobb</span><span class="sxs-lookup"><span data-stu-id="a8262-227">Azure Blob output dataset</span></span>

<span data-ttu-id="a8262-228">Data skrivs till en ny blob varje timme (frekvens: timme, intervall: 1).</span><span class="sxs-lookup"><span data-stu-id="a8262-228">Data is written to a new blob every hour (frequency: hour, interval: 1).</span></span> <span data-ttu-id="a8262-229">Sökvägen till mappen för blobben utvärderas dynamiskt baserat på starttiden för den sektor som bearbetas.</span><span class="sxs-lookup"><span data-stu-id="a8262-229">The folder path for the blob is dynamically evaluated based on the start time of the slice that is being processed.</span></span> <span data-ttu-id="a8262-230">Mappsökvägen använder år, månad, dag och timmar delar av starttiden.</span><span class="sxs-lookup"><span data-stu-id="a8262-230">The folder path uses the year, month, day, and hours parts of the start time.</span></span>

```json
{
    "name": "AzureBlobOutputDataSet",
    "properties": {
        "type": "AzureBlob",
        "linkedServiceName": "AzureStorageLinkedService",
        "typeProperties": {
            "folderPath": "mycontainer/fromamazons3/yearno={Year}/monthno={Month}/dayno={Day}/hourno={Hour}",
            "format": {
                "type": "TextFormat",
                "rowDelimiter": "\n",
                "columnDelimiter": "\t"
            },
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
            ]
        },
        "availability": {
            "frequency": "Hour",
            "interval": 1
        }
    }
}
```


### <a name="copy-activity-in-a-pipeline-with-an-amazon-s3-source-and-a-blob-sink"></a><span data-ttu-id="a8262-231">Kopiera aktivitet i en pipeline med en källa för Amazon S3 och en blob-mottagare</span><span class="sxs-lookup"><span data-stu-id="a8262-231">Copy activity in a pipeline with an Amazon S3 source and a blob sink</span></span>

<span data-ttu-id="a8262-232">Pipelinen innehåller en kopia-aktivitet som är konfigurerad för att använda indata och utdata-datauppsättningar och är schemalagd att köras varje timme.</span><span class="sxs-lookup"><span data-stu-id="a8262-232">The pipeline contains a copy activity that is configured to use the input and output datasets, and is scheduled to run every hour.</span></span> <span data-ttu-id="a8262-233">I pipeline-JSON-definitionen av **källa** är inställd på **FileSystemSource**, och **sink** är inställd på **BlobSink**.</span><span class="sxs-lookup"><span data-stu-id="a8262-233">In the pipeline JSON definition, the **source** type is set to **FileSystemSource**, and **sink** type is set to **BlobSink**.</span></span>

```json
{
    "name": "CopyAmazonS3ToBlob",
    "properties": {
        "description": "pipeline for copy activity",
        "activities": [
            {
                "type": "Copy",
                "typeProperties": {
                    "source": {
                        "type": "FileSystemSource",
                        "recursive": true
                    },
                    "sink": {
                        "type": "BlobSink",
                        "writeBatchSize": 0,
                        "writeBatchTimeout": "00:00:00"
                    }
                },
                "inputs": [
                    {
                        "name": "AmazonS3InputDataset"
                    }
                ],
                "outputs": [
                    {
                        "name": "AzureBlobOutputDataSet"
                    }
                ],
                "policy": {
                    "timeout": "01:00:00",
                    "concurrency": 1
                },
                "scheduler": {
                    "frequency": "Hour",
                    "interval": 1
                },
                "name": "AmazonS3ToBlob"
            }
        ],
        "start": "2014-08-08T18:00:00Z",
        "end": "2014-08-08T19:00:00Z"
    }
}
```
> [!NOTE]
> <span data-ttu-id="a8262-234">Om du vill mappa kolumner från en datamängd för källan till kolumner från en datamängd sink finns [mappa dataset kolumner i Azure Data Factory](data-factory-map-columns.md).</span><span class="sxs-lookup"><span data-stu-id="a8262-234">To map columns from a source dataset to columns from a sink dataset, see [Mapping dataset columns in Azure Data Factory](data-factory-map-columns.md).</span></span>


## <a name="next-steps"></a><span data-ttu-id="a8262-235">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="a8262-235">Next steps</span></span>
<span data-ttu-id="a8262-236">Se följande artiklar:</span><span class="sxs-lookup"><span data-stu-id="a8262-236">See the following articles:</span></span>

* <span data-ttu-id="a8262-237">Mer information om viktiga faktorer som påverkan prestanda för flytt av data (kopieringsaktiviteten) i Data Factory och olika sätt att optimera den finns i [kopiera aktivitet prestanda och prestandajustering guiden](data-factory-copy-activity-performance.md).</span><span class="sxs-lookup"><span data-stu-id="a8262-237">To learn about key factors that impact performance of data movement (copy activity) in Data Factory, and various ways to optimize it, see the [Copy activity performance and tuning guide](data-factory-copy-activity-performance.md).</span></span>

* <span data-ttu-id="a8262-238">Stegvisa instruktioner för att skapa en pipeline med kopiera aktiviteter finns i [kopiera aktivitet kursen](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md).</span><span class="sxs-lookup"><span data-stu-id="a8262-238">For step-by-step instructions for creating a pipeline with a copy activity, see the [Copy activity tutorial](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md).</span></span>
