---
title: "aaaMove data från Amazon enkla Storage-tjänsten med hjälp av Data Factory | Microsoft Docs"
description: "Mer information om hur toomove data från Amazon enkla lagringstjänst (S3) med hjälp av Azure Data Factory."
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
ms.openlocfilehash: 8a8cd2845fd1de74413bd0372f3aabfb4817549b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="move-data-from-amazon-simple-storage-service-by-using-azure-data-factory"></a><span data-ttu-id="2373b-103">Flytta data från Amazon enkla Storage-tjänsten med hjälp av Azure Data Factory</span><span class="sxs-lookup"><span data-stu-id="2373b-103">Move data from Amazon Simple Storage Service by using Azure Data Factory</span></span>
<span data-ttu-id="2373b-104">Den här artikeln förklarar hur toouse hello kopieringsaktiviteten i Azure Data Factory toomove data från Amazon enkla lagringstjänst (S3).</span><span class="sxs-lookup"><span data-stu-id="2373b-104">This article explains how toouse hello copy activity in Azure Data Factory toomove data from Amazon Simple Storage Service (S3).</span></span> <span data-ttu-id="2373b-105">Den bygger på hello [Data movement aktiviteter](data-factory-data-movement-activities.md) artikel som ger en allmän översikt över dataflyttning hello kopieringsaktiviteten.</span><span class="sxs-lookup"><span data-stu-id="2373b-105">It builds on hello [Data movement activities](data-factory-data-movement-activities.md) article, which presents a general overview of data movement with hello copy activity.</span></span>

<span data-ttu-id="2373b-106">Du kan kopiera data från Amazon S3 tooany stöds sink-datalagret.</span><span class="sxs-lookup"><span data-stu-id="2373b-106">You can copy data from Amazon S3 tooany supported sink data store.</span></span> <span data-ttu-id="2373b-107">En lista över data lagras som stöds när egenskaperna av hello kopieringsaktiviteten Se hello [stöds datalager](data-factory-data-movement-activities.md#supported-data-stores-and-formats) tabell.</span><span class="sxs-lookup"><span data-stu-id="2373b-107">For a list of data stores supported as sinks by hello copy activity, see hello [Supported data stores](data-factory-data-movement-activities.md#supported-data-stores-and-formats) table.</span></span> <span data-ttu-id="2373b-108">Data Factory stöder för närvarande endast flytta data från Amazon S3 tooother datalager, men inte flytta data från andra data lagrar tooAmazon S3.</span><span class="sxs-lookup"><span data-stu-id="2373b-108">Data Factory currently supports only moving data from Amazon S3 tooother data stores, but not moving data from other data stores tooAmazon S3.</span></span>

## <a name="required-permissions"></a><span data-ttu-id="2373b-109">Behörigheter som krävs</span><span class="sxs-lookup"><span data-stu-id="2373b-109">Required permissions</span></span>
<span data-ttu-id="2373b-110">toocopy data från Amazon S3, kontrollera att du har beviljats hello följande behörigheter:</span><span class="sxs-lookup"><span data-stu-id="2373b-110">toocopy data from Amazon S3, make sure you have been granted hello following permissions:</span></span>

* <span data-ttu-id="2373b-111">`s3:GetObject`och `s3:GetObjectVersion` för Amazon S3 objekt åtgärder.</span><span class="sxs-lookup"><span data-stu-id="2373b-111">`s3:GetObject` and `s3:GetObjectVersion` for Amazon S3 Object Operations.</span></span>
* <span data-ttu-id="2373b-112">`s3:ListBucket`för Amazon S3 Bucket-åtgärder.</span><span class="sxs-lookup"><span data-stu-id="2373b-112">`s3:ListBucket` for Amazon S3 Bucket Operations.</span></span> <span data-ttu-id="2373b-113">Om du använder hello Data Factory kopiera guiden `s3:ListAllMyBuckets` krävs också.</span><span class="sxs-lookup"><span data-stu-id="2373b-113">If you are using hello Data Factory Copy Wizard, `s3:ListAllMyBuckets` is also required.</span></span>

<span data-ttu-id="2373b-114">Mer information om hello fullständig lista över Amazon S3 behörigheter finns [att ange behörigheter i en princip](http://docs.aws.amazon.com/AmazonS3/latest/dev/using-with-s3-actions.html).</span><span class="sxs-lookup"><span data-stu-id="2373b-114">For details about hello full list of Amazon S3 permissions, see [Specifying Permissions in a Policy](http://docs.aws.amazon.com/AmazonS3/latest/dev/using-with-s3-actions.html).</span></span>

## <a name="getting-started"></a><span data-ttu-id="2373b-115">Komma igång</span><span class="sxs-lookup"><span data-stu-id="2373b-115">Getting started</span></span>
<span data-ttu-id="2373b-116">Du kan skapa en pipeline med en kopia-aktivitet som flyttar data från en källa för Amazon S3 med hjälp av olika verktyg eller API: er.</span><span class="sxs-lookup"><span data-stu-id="2373b-116">You can create a pipeline with a copy activity that moves data from an Amazon S3 source by using different tools or APIs.</span></span>

<span data-ttu-id="2373b-117">hello enklaste sättet toocreate en pipeline är toouse hello **guiden Kopiera**.</span><span class="sxs-lookup"><span data-stu-id="2373b-117">hello easiest way toocreate a pipeline is toouse hello **Copy Wizard**.</span></span> <span data-ttu-id="2373b-118">För en snabb genomgång, se [Självstudier: skapa en pipeline med hjälp av guiden Kopiera](data-factory-copy-data-wizard-tutorial.md).</span><span class="sxs-lookup"><span data-stu-id="2373b-118">For a quick walkthrough, see [Tutorial: Create a pipeline using Copy Wizard](data-factory-copy-data-wizard-tutorial.md).</span></span>

<span data-ttu-id="2373b-119">Du kan också använda följande verktyg toocreate en pipeline hello: **Azure-portalen**, **Visual Studio**, **Azure PowerShell**, **Azure Resource Manager-mall** , **.NET API**, och **REST API**.</span><span class="sxs-lookup"><span data-stu-id="2373b-119">You can also use hello following tools toocreate a pipeline: **Azure portal**, **Visual Studio**, **Azure PowerShell**, **Azure Resource Manager template**, **.NET API**, and **REST API**.</span></span> <span data-ttu-id="2373b-120">Stegvisa instruktioner toocreate en pipeline med en kopieringsaktiviteten finns hello [kopiera aktivitet kursen](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md).</span><span class="sxs-lookup"><span data-stu-id="2373b-120">For step-by-step instructions toocreate a pipeline with a copy activity, see hello [Copy activity tutorial](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md).</span></span>

<span data-ttu-id="2373b-121">Om du använder verktyg eller API: er kan utföra du hello följande steg toocreate en pipeline som flyttar data från en källdata lagra tooa sink-datalagret:</span><span class="sxs-lookup"><span data-stu-id="2373b-121">Whether you use tools or APIs, you perform hello following steps toocreate a pipeline that moves data from a source data store tooa sink data store:</span></span>

1. <span data-ttu-id="2373b-122">Skapa **länkade tjänster** toolink indata och utdata lagrar tooyour data factory.</span><span class="sxs-lookup"><span data-stu-id="2373b-122">Create **linked services** toolink input and output data stores tooyour data factory.</span></span>
2. <span data-ttu-id="2373b-123">Skapa **datauppsättningar** toorepresent indata och utdata för hello kopieringsåtgärden.</span><span class="sxs-lookup"><span data-stu-id="2373b-123">Create **datasets** toorepresent input and output data for hello copy operation.</span></span>
3. <span data-ttu-id="2373b-124">Skapa en **pipeline** med en kopia-aktivitet som tar en datamängd som indata och en dataset som utdata.</span><span class="sxs-lookup"><span data-stu-id="2373b-124">Create a **pipeline** with a copy activity that takes a dataset as an input and a dataset as an output.</span></span>

<span data-ttu-id="2373b-125">När du använder guiden hello skapas automatiskt JSON definitioner för dessa Data Factory-enheter (länkade tjänster, datauppsättningar och hello pipeline) för dig.</span><span class="sxs-lookup"><span data-stu-id="2373b-125">When you use hello wizard, JSON definitions for these Data Factory entities (linked services, datasets, and hello pipeline) are automatically created for you.</span></span> <span data-ttu-id="2373b-126">När du använder verktyg eller API: er (utom .NET API), kan du definiera dessa Data Factory-enheter med hjälp av hello JSON-format.</span><span class="sxs-lookup"><span data-stu-id="2373b-126">When you use tools or APIs (except .NET API), you define these Data Factory entities by using hello JSON format.</span></span> <span data-ttu-id="2373b-127">Ett exempel med JSON-definitioner för Data Factory-entiteter som ska använda toocopy data från ett dataarkiv för Amazon S3 finns hello [JSON-exempel: kopiera data från Amazon S3 tooAzure Blob](#json-example-copy-data-from-amazon-s3-to-azure-blob) i den här artikeln.</span><span class="sxs-lookup"><span data-stu-id="2373b-127">For a sample with JSON definitions for Data Factory entities that are used toocopy data from an Amazon S3 data store, see hello [JSON example: Copy data from Amazon S3 tooAzure Blob](#json-example-copy-data-from-amazon-s3-to-azure-blob) section of this article.</span></span>

> [!NOTE]
> <span data-ttu-id="2373b-128">Mer information om fil- och komprimering de format som stöds för en kopieringsaktiviteten finns [format och komprimering i Azure Data Factory](data-factory-supported-file-and-compression-formats.md).</span><span class="sxs-lookup"><span data-stu-id="2373b-128">For details about supported file and compression formats for a copy activity, see [File and compression formats in Azure Data Factory](data-factory-supported-file-and-compression-formats.md).</span></span>

<span data-ttu-id="2373b-129">hello följande avsnitt innehåller information om JSON-egenskaper används toodefine Data Factory entiteter specifika tooAmazon S3.</span><span class="sxs-lookup"><span data-stu-id="2373b-129">hello following sections provide details about JSON properties that are used toodefine Data Factory entities specific tooAmazon S3.</span></span>

## <a name="linked-service-properties"></a><span data-ttu-id="2373b-130">Länkad tjänstegenskaper</span><span class="sxs-lookup"><span data-stu-id="2373b-130">Linked service properties</span></span>
<span data-ttu-id="2373b-131">En länkad tjänst länkar en data store tooa data factory.</span><span class="sxs-lookup"><span data-stu-id="2373b-131">A linked service links a data store tooa data factory.</span></span> <span data-ttu-id="2373b-132">Du skapar en länkad tjänst av typen **AwsAccessKey** toolink Amazon S3 data lagra tooyour data factory.</span><span class="sxs-lookup"><span data-stu-id="2373b-132">You create a linked service of type **AwsAccessKey** toolink your Amazon S3 data store tooyour data factory.</span></span> <span data-ttu-id="2373b-133">hello i den följande tabellen finns beskrivning för JSON-element specifika tooAmazon S3 (AwsAccessKey) länkad tjänst.</span><span class="sxs-lookup"><span data-stu-id="2373b-133">hello following table provides description for JSON elements specific tooAmazon S3 (AwsAccessKey) linked service.</span></span>

| <span data-ttu-id="2373b-134">Egenskap</span><span class="sxs-lookup"><span data-stu-id="2373b-134">Property</span></span> | <span data-ttu-id="2373b-135">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="2373b-135">Description</span></span> | <span data-ttu-id="2373b-136">Tillåtna värden</span><span class="sxs-lookup"><span data-stu-id="2373b-136">Allowed values</span></span> | <span data-ttu-id="2373b-137">Krävs</span><span class="sxs-lookup"><span data-stu-id="2373b-137">Required</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="2373b-138">accessKeyID</span><span class="sxs-lookup"><span data-stu-id="2373b-138">accessKeyID</span></span> |<span data-ttu-id="2373b-139">ID för hello hemliga snabbtangent.</span><span class="sxs-lookup"><span data-stu-id="2373b-139">ID of hello secret access key.</span></span> |<span data-ttu-id="2373b-140">Sträng</span><span class="sxs-lookup"><span data-stu-id="2373b-140">string</span></span> |<span data-ttu-id="2373b-141">Ja</span><span class="sxs-lookup"><span data-stu-id="2373b-141">Yes</span></span> |
| <span data-ttu-id="2373b-142">secretAccessKey</span><span class="sxs-lookup"><span data-stu-id="2373b-142">secretAccessKey</span></span> |<span data-ttu-id="2373b-143">hello hemliga åtkomst själva nyckeln.</span><span class="sxs-lookup"><span data-stu-id="2373b-143">hello secret access key itself.</span></span> |<span data-ttu-id="2373b-144">Krypterad hemliga sträng</span><span class="sxs-lookup"><span data-stu-id="2373b-144">Encrypted secret string</span></span> |<span data-ttu-id="2373b-145">Ja</span><span class="sxs-lookup"><span data-stu-id="2373b-145">Yes</span></span> |

<span data-ttu-id="2373b-146">Här är ett exempel:</span><span class="sxs-lookup"><span data-stu-id="2373b-146">Here is an example:</span></span>

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

## <a name="dataset-properties"></a><span data-ttu-id="2373b-147">Egenskaper för datamängd</span><span class="sxs-lookup"><span data-stu-id="2373b-147">Dataset properties</span></span>
<span data-ttu-id="2373b-148">toospecify en dataset toorepresent mata in data i Azure Blob storage, ange hello type-egenskapen för hello dataset för**AmazonS3**.</span><span class="sxs-lookup"><span data-stu-id="2373b-148">toospecify a dataset toorepresent input data in Azure Blob storage, set hello type property of hello dataset too**AmazonS3**.</span></span> <span data-ttu-id="2373b-149">Ange hello **linkedServiceName** -egenskapen för hello dataset toohello namnet på hello Amazon S3 länkade tjänsten.</span><span class="sxs-lookup"><span data-stu-id="2373b-149">Set hello **linkedServiceName** property of hello dataset toohello name of hello Amazon S3 linked service.</span></span> <span data-ttu-id="2373b-150">En fullständig lista över avsnitt och egenskaper som är tillgängliga för att definiera datauppsättningar finns [skapa datauppsättningar](data-factory-create-datasets.md).</span><span class="sxs-lookup"><span data-stu-id="2373b-150">For a full list of sections and properties available for defining datasets, see [Creating datasets](data-factory-create-datasets.md).</span></span> 

<span data-ttu-id="2373b-151">Avsnitt som struktur, tillgänglighet och principen är liknande för alla dataset-typer (till exempel SQL-databas, Azure blob och Azure-tabellen).</span><span class="sxs-lookup"><span data-stu-id="2373b-151">Sections such as structure, availability, and policy are similar for all dataset types (such as SQL database, Azure blob, and Azure table).</span></span> <span data-ttu-id="2373b-152">Hej **typeProperties** avsnitt är olika för varje typ av dataset och ger information om hello platsen för hello data i datalagret hello.</span><span class="sxs-lookup"><span data-stu-id="2373b-152">hello **typeProperties** section is different for each type of dataset, and provides information about hello location of hello data in hello data store.</span></span> <span data-ttu-id="2373b-153">Hej **typeProperties** avsnittet för en dataset av typen **AmazonS3** (som omfattar hello Amazon S3 dataset) har hello följande egenskaper:</span><span class="sxs-lookup"><span data-stu-id="2373b-153">hello **typeProperties** section for a dataset of type **AmazonS3** (which includes hello Amazon S3 dataset) has hello following properties:</span></span>

| <span data-ttu-id="2373b-154">Egenskap</span><span class="sxs-lookup"><span data-stu-id="2373b-154">Property</span></span> | <span data-ttu-id="2373b-155">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="2373b-155">Description</span></span> | <span data-ttu-id="2373b-156">Tillåtna värden</span><span class="sxs-lookup"><span data-stu-id="2373b-156">Allowed values</span></span> | <span data-ttu-id="2373b-157">Krävs</span><span class="sxs-lookup"><span data-stu-id="2373b-157">Required</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="2373b-158">bucketName</span><span class="sxs-lookup"><span data-stu-id="2373b-158">bucketName</span></span> |<span data-ttu-id="2373b-159">hello S3 bucket-namn.</span><span class="sxs-lookup"><span data-stu-id="2373b-159">hello S3 bucket name.</span></span> |<span data-ttu-id="2373b-160">Sträng</span><span class="sxs-lookup"><span data-stu-id="2373b-160">String</span></span> |<span data-ttu-id="2373b-161">Ja</span><span class="sxs-lookup"><span data-stu-id="2373b-161">Yes</span></span> |
| <span data-ttu-id="2373b-162">key</span><span class="sxs-lookup"><span data-stu-id="2373b-162">key</span></span> |<span data-ttu-id="2373b-163">hello S3 Objektnyckel.</span><span class="sxs-lookup"><span data-stu-id="2373b-163">hello S3 object key.</span></span> |<span data-ttu-id="2373b-164">Sträng</span><span class="sxs-lookup"><span data-stu-id="2373b-164">String</span></span> |<span data-ttu-id="2373b-165">Nej</span><span class="sxs-lookup"><span data-stu-id="2373b-165">No</span></span> |
| <span data-ttu-id="2373b-166">prefix</span><span class="sxs-lookup"><span data-stu-id="2373b-166">prefix</span></span> |<span data-ttu-id="2373b-167">Prefix för hello S3 objekt nyckeln.</span><span class="sxs-lookup"><span data-stu-id="2373b-167">Prefix for hello S3 object key.</span></span> <span data-ttu-id="2373b-168">Objekt vars nycklar som börjar med prefixet är markerade.</span><span class="sxs-lookup"><span data-stu-id="2373b-168">Objects whose keys start with this prefix are selected.</span></span> <span data-ttu-id="2373b-169">Gäller endast när nyckeln är tom.</span><span class="sxs-lookup"><span data-stu-id="2373b-169">Applies only when key is empty.</span></span> |<span data-ttu-id="2373b-170">Sträng</span><span class="sxs-lookup"><span data-stu-id="2373b-170">String</span></span> |<span data-ttu-id="2373b-171">Nej</span><span class="sxs-lookup"><span data-stu-id="2373b-171">No</span></span> |
| <span data-ttu-id="2373b-172">Version</span><span class="sxs-lookup"><span data-stu-id="2373b-172">version</span></span> |<span data-ttu-id="2373b-173">hello version av hello S3 objekt, om S3 versionshantering är aktiverat.</span><span class="sxs-lookup"><span data-stu-id="2373b-173">hello version of hello S3 object, if S3 versioning is enabled.</span></span> |<span data-ttu-id="2373b-174">Sträng</span><span class="sxs-lookup"><span data-stu-id="2373b-174">String</span></span> |<span data-ttu-id="2373b-175">Nej</span><span class="sxs-lookup"><span data-stu-id="2373b-175">No</span></span> |
| <span data-ttu-id="2373b-176">Format</span><span class="sxs-lookup"><span data-stu-id="2373b-176">format</span></span> | <span data-ttu-id="2373b-177">hello följande formattyper stöds: **TextFormat**, **JsonFormat**, **AvroFormat**, **OrcFormat**,  **ParquetFormat**.</span><span class="sxs-lookup"><span data-stu-id="2373b-177">hello following format types are supported: **TextFormat**, **JsonFormat**, **AvroFormat**, **OrcFormat**, **ParquetFormat**.</span></span> <span data-ttu-id="2373b-178">Ange hello **typen** egenskap under format tooone av dessa värden.</span><span class="sxs-lookup"><span data-stu-id="2373b-178">Set hello **type** property under format tooone of these values.</span></span> <span data-ttu-id="2373b-179">Mer information finns i hello [textformat](data-factory-supported-file-and-compression-formats.md#text-format), [JSON-format](data-factory-supported-file-and-compression-formats.md#json-format), [Avro-formatet](data-factory-supported-file-and-compression-formats.md#avro-format), [Orc format](data-factory-supported-file-and-compression-formats.md#orc-format), och [parkettgolv format ](data-factory-supported-file-and-compression-formats.md#parquet-format) avsnitt.</span><span class="sxs-lookup"><span data-stu-id="2373b-179">For more information, see hello [Text format](data-factory-supported-file-and-compression-formats.md#text-format), [JSON format](data-factory-supported-file-and-compression-formats.md#json-format), [Avro format](data-factory-supported-file-and-compression-formats.md#avro-format), [Orc format](data-factory-supported-file-and-compression-formats.md#orc-format), and [Parquet format](data-factory-supported-file-and-compression-formats.md#parquet-format) sections.</span></span> <br><br> <span data-ttu-id="2373b-180">Om du vill toocopy filer som-mellan filbaserade butiker (binär kopia), hoppa över hello format-avsnittet i både inkommande och utgående dataset-definitioner.</span><span class="sxs-lookup"><span data-stu-id="2373b-180">If you want toocopy files as-is between file-based stores (binary copy), skip hello format section in both input and output dataset definitions.</span></span> |<span data-ttu-id="2373b-181">Nej</span><span class="sxs-lookup"><span data-stu-id="2373b-181">No</span></span> | |
| <span data-ttu-id="2373b-182">Komprimering</span><span class="sxs-lookup"><span data-stu-id="2373b-182">compression</span></span> | <span data-ttu-id="2373b-183">Ange hello typ och kompression för hello data.</span><span class="sxs-lookup"><span data-stu-id="2373b-183">Specify hello type and level of compression for hello data.</span></span> <span data-ttu-id="2373b-184">hello stöds typer är: **GZip**, **Deflate**, **BZip2**, och **ZipDeflate**.</span><span class="sxs-lookup"><span data-stu-id="2373b-184">hello supported types are: **GZip**, **Deflate**, **BZip2**, and **ZipDeflate**.</span></span> <span data-ttu-id="2373b-185">hello som stöds är: **Optimal** och **snabbast**.</span><span class="sxs-lookup"><span data-stu-id="2373b-185">hello supported levels are: **Optimal** and **Fastest**.</span></span> <span data-ttu-id="2373b-186">Mer information finns i [format och komprimering i Azure Data Factory](data-factory-supported-file-and-compression-formats.md#compression-support).</span><span class="sxs-lookup"><span data-stu-id="2373b-186">For more information, see [File and compression formats in Azure Data Factory](data-factory-supported-file-and-compression-formats.md#compression-support).</span></span> |<span data-ttu-id="2373b-187">Nej</span><span class="sxs-lookup"><span data-stu-id="2373b-187">No</span></span> | |


> [!NOTE]
> <span data-ttu-id="2373b-188">**bucketName + nyckeln** anger hello platsen för hello S3 objekt, där bucket är hello Rotbehållare för S3 objekt och nyckeln är hello fullständig sökväg toohello S3 objekt.</span><span class="sxs-lookup"><span data-stu-id="2373b-188">**bucketName + key** specifies hello location of hello S3 object, where bucket is hello root container for S3 objects, and key is hello full path toohello S3 object.</span></span>

### <a name="sample-dataset-with-prefix"></a><span data-ttu-id="2373b-189">Exempeldatamängd med prefix</span><span class="sxs-lookup"><span data-stu-id="2373b-189">Sample dataset with prefix</span></span>

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
### <a name="sample-dataset-with-version"></a><span data-ttu-id="2373b-190">Exempeldatamängd (med version)</span><span class="sxs-lookup"><span data-stu-id="2373b-190">Sample dataset (with version)</span></span>

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

### <a name="dynamic-paths-for-s3"></a><span data-ttu-id="2373b-191">Dynamisk sökvägar för S3</span><span class="sxs-lookup"><span data-stu-id="2373b-191">Dynamic paths for S3</span></span>
<span data-ttu-id="2373b-192">hello föregående exempel använder fasta värden för hello **nyckeln** och **bucketName** egenskaper i hello Amazon S3 dataset.</span><span class="sxs-lookup"><span data-stu-id="2373b-192">hello preceding sample uses fixed values for hello **key** and **bucketName** properties in hello Amazon S3 dataset.</span></span>

```json
"key": "testFolder/test.orc",
"bucketName": "testbucket",
```

<span data-ttu-id="2373b-193">Du kan ha Data Factory beräkna egenskaperna dynamiskt vid körning, med hjälp av systemvariabler som SliceStart.</span><span class="sxs-lookup"><span data-stu-id="2373b-193">You can have Data Factory calculate these properties dynamically at runtime, by using system variables such as SliceStart.</span></span>

```json
"key": "$$Text.Format('{0:MM}/{0:dd}/test.orc', SliceStart)"
"bucketName": "$$Text.Format('{0:yyyy}', SliceStart)"
```

<span data-ttu-id="2373b-194">Du kan göra hello samma för hello **prefix** -egenskapen för en Amazon S3 datauppsättning.</span><span class="sxs-lookup"><span data-stu-id="2373b-194">You can do hello same for hello **prefix** property of an Amazon S3 dataset.</span></span> <span data-ttu-id="2373b-195">En lista över funktioner som stöds och variabler, se [Data Factory-funktioner och systemvariabler](data-factory-functions-variables.md).</span><span class="sxs-lookup"><span data-stu-id="2373b-195">For a list of supported functions and variables, see [Data Factory functions and system variables](data-factory-functions-variables.md).</span></span>

## <a name="copy-activity-properties"></a><span data-ttu-id="2373b-196">Kopiera egenskaper för aktivitet</span><span class="sxs-lookup"><span data-stu-id="2373b-196">Copy activity properties</span></span>
<span data-ttu-id="2373b-197">En fullständig lista över avsnitt och egenskaper som är tillgängliga för att definiera aktiviteter, se [skapar pipelines](data-factory-create-pipelines.md).</span><span class="sxs-lookup"><span data-stu-id="2373b-197">For a full list of sections and properties available for defining activities, see [Creating pipelines](data-factory-create-pipelines.md).</span></span> <span data-ttu-id="2373b-198">Egenskaper som namn, beskrivning, ingående och utgående tabeller och principer är tillgängliga för alla typer av aktiviteter.</span><span class="sxs-lookup"><span data-stu-id="2373b-198">Properties such as name, description, input and output tables, and policies are available for all types of activities.</span></span> <span data-ttu-id="2373b-199">Egenskaper som är tillgängliga i hello **typeProperties** avsnittet hello aktivitet varierar med varje aktivitetstyp.</span><span class="sxs-lookup"><span data-stu-id="2373b-199">Properties available in hello **typeProperties** section of hello activity vary with each activity type.</span></span> <span data-ttu-id="2373b-200">För hello kopieringsaktiviteten varierar egenskaper beroende på hello typer av datakällor och sänkor.</span><span class="sxs-lookup"><span data-stu-id="2373b-200">For hello copy activity, properties vary depending on hello types of sources and sinks.</span></span> <span data-ttu-id="2373b-201">När en källa i hello kopieringsaktiviteten är av typen **FileSystemSource** (som omfattar Amazon S3), hello efter egenskapen finns i **typeProperties** avsnitt:</span><span class="sxs-lookup"><span data-stu-id="2373b-201">When a source in hello copy activity is of type **FileSystemSource** (which includes Amazon S3), hello following property is available in **typeProperties** section:</span></span>

| <span data-ttu-id="2373b-202">Egenskap</span><span class="sxs-lookup"><span data-stu-id="2373b-202">Property</span></span> | <span data-ttu-id="2373b-203">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="2373b-203">Description</span></span> | <span data-ttu-id="2373b-204">Tillåtna värden</span><span class="sxs-lookup"><span data-stu-id="2373b-204">Allowed values</span></span> | <span data-ttu-id="2373b-205">Krävs</span><span class="sxs-lookup"><span data-stu-id="2373b-205">Required</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="2373b-206">Rekursiva</span><span class="sxs-lookup"><span data-stu-id="2373b-206">recursive</span></span> |<span data-ttu-id="2373b-207">Anger om toorecursively lista S3 objekt under hello katalog.</span><span class="sxs-lookup"><span data-stu-id="2373b-207">Specifies whether toorecursively list S3 objects under hello directory.</span></span> |<span data-ttu-id="2373b-208">SANT/FALSKT</span><span class="sxs-lookup"><span data-stu-id="2373b-208">true/false</span></span> |<span data-ttu-id="2373b-209">Nej</span><span class="sxs-lookup"><span data-stu-id="2373b-209">No</span></span> |

## <a name="json-example-copy-data-from-amazon-s3-tooazure-blob-storage"></a><span data-ttu-id="2373b-210">JSON-exempel: kopiera data från Amazon S3 tooAzure Blob storage</span><span class="sxs-lookup"><span data-stu-id="2373b-210">JSON example: Copy data from Amazon S3 tooAzure Blob storage</span></span>
<span data-ttu-id="2373b-211">Det här exemplet visas hur toocopy data från Amazon S3 tooan Azure Blob storage.</span><span class="sxs-lookup"><span data-stu-id="2373b-211">This sample shows how toocopy data from Amazon S3 tooan Azure Blob storage.</span></span> <span data-ttu-id="2373b-212">Dock datan kan kopieras direkt för[någon hello sänkor som stöds](data-factory-data-movement-activities.md#supported-data-stores-and-formats) med hello kopieringsaktiviteten i Data Factory.</span><span class="sxs-lookup"><span data-stu-id="2373b-212">However, data can be copied directly too[any of hello sinks that are supported](data-factory-data-movement-activities.md#supported-data-stores-and-formats) by using hello copy activity in Data Factory.</span></span>

<span data-ttu-id="2373b-213">hello exempel innehåller JSON definitioner för hello följande Data Factory entiteter.</span><span class="sxs-lookup"><span data-stu-id="2373b-213">hello sample provides JSON definitions for hello following Data Factory entities.</span></span> <span data-ttu-id="2373b-214">Du kan använda dessa definitioner toocreate pipeline toocopy data från Amazon S3 tooBlob lagring med hjälp av hello [Azure-portalen](data-factory-copy-activity-tutorial-using-azure-portal.md), [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md), eller [PowerShell](data-factory-copy-activity-tutorial-using-powershell.md).</span><span class="sxs-lookup"><span data-stu-id="2373b-214">You can use these definitions toocreate a pipeline toocopy data from Amazon S3 tooBlob storage, by using hello [Azure portal](data-factory-copy-activity-tutorial-using-azure-portal.md), [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md), or [PowerShell](data-factory-copy-activity-tutorial-using-powershell.md).</span></span>   

* <span data-ttu-id="2373b-215">En länkad tjänst av typen [AwsAccessKey](#linked-service-properties).</span><span class="sxs-lookup"><span data-stu-id="2373b-215">A linked service of type [AwsAccessKey](#linked-service-properties).</span></span>
* <span data-ttu-id="2373b-216">En länkad tjänst av typen [AzureStorage](data-factory-azure-blob-connector.md#linked-service-properties).</span><span class="sxs-lookup"><span data-stu-id="2373b-216">A linked service of type [AzureStorage](data-factory-azure-blob-connector.md#linked-service-properties).</span></span>
* <span data-ttu-id="2373b-217">Indata [dataset](data-factory-create-datasets.md) av typen [AmazonS3](#dataset-properties).</span><span class="sxs-lookup"><span data-stu-id="2373b-217">An input [dataset](data-factory-create-datasets.md) of type [AmazonS3](#dataset-properties).</span></span>
* <span data-ttu-id="2373b-218">Utdata [dataset](data-factory-create-datasets.md) av typen [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties).</span><span class="sxs-lookup"><span data-stu-id="2373b-218">An output [dataset](data-factory-create-datasets.md) of type [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties).</span></span>
* <span data-ttu-id="2373b-219">En [pipeline](data-factory-create-pipelines.md) med kopieringsaktiviteten som använder [FileSystemSource](#copy-activity-properties) och [BlobSink](data-factory-azure-blob-connector.md#copy-activity-properties).</span><span class="sxs-lookup"><span data-stu-id="2373b-219">A [pipeline](data-factory-create-pipelines.md) with copy activity that uses [FileSystemSource](#copy-activity-properties) and [BlobSink](data-factory-azure-blob-connector.md#copy-activity-properties).</span></span>

<span data-ttu-id="2373b-220">hello exemplet kopierar data från Amazon S3 tooan Azure blob varje timme.</span><span class="sxs-lookup"><span data-stu-id="2373b-220">hello sample copies data from Amazon S3 tooan Azure blob every hour.</span></span> <span data-ttu-id="2373b-221">hello JSON egenskaper som används i exemplen beskrivs i hello-exempel i följande avsnitt.</span><span class="sxs-lookup"><span data-stu-id="2373b-221">hello JSON properties used in these samples are described in sections following hello samples.</span></span>

### <a name="amazon-s3-linked-service"></a><span data-ttu-id="2373b-222">Amazon S3 länkad tjänst</span><span class="sxs-lookup"><span data-stu-id="2373b-222">Amazon S3 linked service</span></span>

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

### <a name="azure-storage-linked-service"></a><span data-ttu-id="2373b-223">Länkad Azure-lagringstjänst</span><span class="sxs-lookup"><span data-stu-id="2373b-223">Azure Storage linked service</span></span>

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

### <a name="amazon-s3-input-dataset"></a><span data-ttu-id="2373b-224">Amazon S3 inkommande dataset</span><span class="sxs-lookup"><span data-stu-id="2373b-224">Amazon S3 input dataset</span></span>

<span data-ttu-id="2373b-225">Ange **”externa”: true** informerar hello Data Factory-tjänsten som hello dataset är externa toohello data factory.</span><span class="sxs-lookup"><span data-stu-id="2373b-225">Setting **"external": true** informs hello Data Factory service that hello dataset is external toohello data factory.</span></span> <span data-ttu-id="2373b-226">Ange den här egenskapen tootrue på ett inkommande datamängd som inte tillverkas av en aktivitet i hello pipeline.</span><span class="sxs-lookup"><span data-stu-id="2373b-226">Set this property tootrue on an input dataset that is not produced by an activity in hello pipeline.</span></span>

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


### <a name="azure-blob-output-dataset"></a><span data-ttu-id="2373b-227">Utdatauppsättning för Azure-blobb</span><span class="sxs-lookup"><span data-stu-id="2373b-227">Azure Blob output dataset</span></span>

<span data-ttu-id="2373b-228">Data skrivs tooa nya blob varje timme (frekvens: timme, intervall: 1).</span><span class="sxs-lookup"><span data-stu-id="2373b-228">Data is written tooa new blob every hour (frequency: hour, interval: 1).</span></span> <span data-ttu-id="2373b-229">hello mappsökväg för hello blob utvärderas dynamiskt baserat på hello starttiden för hello-segment som bearbetas.</span><span class="sxs-lookup"><span data-stu-id="2373b-229">hello folder path for hello blob is dynamically evaluated based on hello start time of hello slice that is being processed.</span></span> <span data-ttu-id="2373b-230">hello mappsökväg använder hello år, månad, dag och timmar delar av hello starttid.</span><span class="sxs-lookup"><span data-stu-id="2373b-230">hello folder path uses hello year, month, day, and hours parts of hello start time.</span></span>

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


### <a name="copy-activity-in-a-pipeline-with-an-amazon-s3-source-and-a-blob-sink"></a><span data-ttu-id="2373b-231">Kopiera aktivitet i en pipeline med en källa för Amazon S3 och en blob-mottagare</span><span class="sxs-lookup"><span data-stu-id="2373b-231">Copy activity in a pipeline with an Amazon S3 source and a blob sink</span></span>

<span data-ttu-id="2373b-232">hello pipelinen innehåller en kopia-aktivitet som är konfigurerade toouse hello inkommande och utgående datauppsättningar och är schemalagda toorun varje timme.</span><span class="sxs-lookup"><span data-stu-id="2373b-232">hello pipeline contains a copy activity that is configured toouse hello input and output datasets, and is scheduled toorun every hour.</span></span> <span data-ttu-id="2373b-233">I hello pipeline JSON-definitionen hello **källa** typ har angetts för**FileSystemSource**, och **sink** typ har angetts för**BlobSink**.</span><span class="sxs-lookup"><span data-stu-id="2373b-233">In hello pipeline JSON definition, hello **source** type is set too**FileSystemSource**, and **sink** type is set too**BlobSink**.</span></span>

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
> <span data-ttu-id="2373b-234">toomap kolumner från en källa dataset toocolumns från en mottagare datamängd finns [mappa dataset kolumner i Azure Data Factory](data-factory-map-columns.md).</span><span class="sxs-lookup"><span data-stu-id="2373b-234">toomap columns from a source dataset toocolumns from a sink dataset, see [Mapping dataset columns in Azure Data Factory](data-factory-map-columns.md).</span></span>


## <a name="next-steps"></a><span data-ttu-id="2373b-235">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="2373b-235">Next steps</span></span>
<span data-ttu-id="2373b-236">Se hello följande artiklar:</span><span class="sxs-lookup"><span data-stu-id="2373b-236">See hello following articles:</span></span>

* <span data-ttu-id="2373b-237">toolearn om nyckeln faktorer som påverkan prestanda för flytt av data (kopieringsaktiviteten) i Data Factory och olika sätt toooptimize, se hello [kopiera aktivitet prestanda och prestandajustering guiden](data-factory-copy-activity-performance.md).</span><span class="sxs-lookup"><span data-stu-id="2373b-237">toolearn about key factors that impact performance of data movement (copy activity) in Data Factory, and various ways toooptimize it, see hello [Copy activity performance and tuning guide](data-factory-copy-activity-performance.md).</span></span>

* <span data-ttu-id="2373b-238">Stegvisa instruktioner för att skapa en pipeline med en kopieringsaktiviteten finns hello [kopiera aktivitet kursen](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md).</span><span class="sxs-lookup"><span data-stu-id="2373b-238">For step-by-step instructions for creating a pipeline with a copy activity, see hello [Copy activity tutorial](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md).</span></span>
