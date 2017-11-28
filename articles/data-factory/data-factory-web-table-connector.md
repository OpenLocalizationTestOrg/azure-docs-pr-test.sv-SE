---
title: "aaaMove data från webben tabellen med hjälp av Azure Data Factory | Microsoft Docs"
description: "Läs mer om hur toomove data från en tabell i ett Webb sidan med Azure Data Factory."
services: data-factory
documentationcenter: 
author: linda33wj
manager: jhubbard
editor: monicar
ms.assetid: f54a26a4-baa4-4255-9791-5a8f935898e2
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/11/2017
ms.author: jingwang
ms.openlocfilehash: e52216305583ebbe71ed896522f361bb22f01278
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="move-data-from-a-web-table-source-using-azure-data-factory"></a><span data-ttu-id="78d62-103">Flytta data från en webbadress för tabellen med hjälp av Azure Data Factory</span><span class="sxs-lookup"><span data-stu-id="78d62-103">Move data from a Web table source using Azure Data Factory</span></span>
<span data-ttu-id="78d62-104">Den här artikeln beskrivs hur toouse hello Kopieringsaktiviteten i Azure Data Factory toomove data från en tabell i en webbsida tooa stöds sink-datalagret.</span><span class="sxs-lookup"><span data-stu-id="78d62-104">This article outlines how toouse hello Copy Activity in Azure Data Factory toomove data from a table in a Web page tooa supported sink data store.</span></span> <span data-ttu-id="78d62-105">Den här artikeln bygger på hello [data movement aktiviteter](data-factory-data-movement-activities.md) artikel som presenterar en allmän översikt över dataflyttning med kopiera aktivitet och hello lista över datakällor som stöds som källor/sänkor.</span><span class="sxs-lookup"><span data-stu-id="78d62-105">This article builds on hello [data movement activities](data-factory-data-movement-activities.md) article that presents a general overview of data movement with copy activity and hello list of data stores supported as sources/sinks.</span></span>

<span data-ttu-id="78d62-106">Data factory stöder för närvarande endast flytta data från en webbserver tabell tooother data lagras, men inte flytta data från andra data lagras tooa Web tabell mål.</span><span class="sxs-lookup"><span data-stu-id="78d62-106">Data factory currently supports only moving data from a Web table tooother data stores, but not moving data from other data stores tooa Web table destination.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="78d62-107">Den här Web connector stöder för närvarande endast extrahera innehållet från en HTML-sida.</span><span class="sxs-lookup"><span data-stu-id="78d62-107">This Web connector currently supports only extracting table content from an HTML page.</span></span> <span data-ttu-id="78d62-108">tooretrieve data från en HTTP/s-slutpunkt, Använd [HTTP-anslutningen](data-factory-http-connector.md) i stället.</span><span class="sxs-lookup"><span data-stu-id="78d62-108">tooretrieve data from a HTTP/s endpoint, use [HTTP connector](data-factory-http-connector.md) instead.</span></span>

## <a name="getting-started"></a><span data-ttu-id="78d62-109">Komma igång</span><span class="sxs-lookup"><span data-stu-id="78d62-109">Getting started</span></span>
<span data-ttu-id="78d62-110">Du kan skapa en pipeline med en kopia-aktivitet som flyttar data från en lokal Cassandra data store med hjälp av olika verktyg/API: er.</span><span class="sxs-lookup"><span data-stu-id="78d62-110">You can create a pipeline with a copy activity that moves data from an on-premises Cassandra data store by using different tools/APIs.</span></span> 

- <span data-ttu-id="78d62-111">hello enklaste sättet toocreate en pipeline är toouse hello **guiden Kopiera**.</span><span class="sxs-lookup"><span data-stu-id="78d62-111">hello easiest way toocreate a pipeline is toouse hello **Copy Wizard**.</span></span> <span data-ttu-id="78d62-112">Se [Självstudier: skapa en pipeline med hjälp av guiden Kopiera](data-factory-copy-data-wizard-tutorial.md) för en snabb genomgång om hur du skapar en pipeline med hjälp av guiden för hello kopiera data.</span><span class="sxs-lookup"><span data-stu-id="78d62-112">See [Tutorial: Create a pipeline using Copy Wizard](data-factory-copy-data-wizard-tutorial.md) for a quick walkthrough on creating a pipeline using hello Copy data wizard.</span></span> 
- <span data-ttu-id="78d62-113">Du kan också använda följande verktyg toocreate en pipeline hello: **Azure-portalen**, **Visual Studio**, **Azure PowerShell**, **Azure Resource Manager-mall** , **.NET API**, och **REST API**.</span><span class="sxs-lookup"><span data-stu-id="78d62-113">You can also use hello following tools toocreate a pipeline: **Azure portal**, **Visual Studio**, **Azure PowerShell**, **Azure Resource Manager template**, **.NET API**, and **REST API**.</span></span> <span data-ttu-id="78d62-114">Se [kopiera aktivitet kursen](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) för stegvisa instruktioner toocreate en pipeline med en Kopieringsaktivitet.</span><span class="sxs-lookup"><span data-stu-id="78d62-114">See [Copy activity tutorial](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) for step-by-step instructions toocreate a pipeline with a copy activity.</span></span> 

<span data-ttu-id="78d62-115">Om du använder hello verktyg eller API: er kan utföra du hello följande steg toocreate en pipeline som flyttar data från en källdata lagra tooa sink-datalagret:</span><span class="sxs-lookup"><span data-stu-id="78d62-115">Whether you use hello tools or APIs, you perform hello following steps toocreate a pipeline that moves data from a source data store tooa sink data store:</span></span>

1. <span data-ttu-id="78d62-116">Skapa **länkade tjänster** toolink indata och utdata lagrar tooyour data factory.</span><span class="sxs-lookup"><span data-stu-id="78d62-116">Create **linked services** toolink input and output data stores tooyour data factory.</span></span>
2. <span data-ttu-id="78d62-117">Skapa **datauppsättningar** toorepresent indata och utdata för hello kopieringsåtgärden.</span><span class="sxs-lookup"><span data-stu-id="78d62-117">Create **datasets** toorepresent input and output data for hello copy operation.</span></span> 
3. <span data-ttu-id="78d62-118">Skapa en **pipeline** med en kopia-aktivitet som tar en datamängd som indata och en dataset som utdata.</span><span class="sxs-lookup"><span data-stu-id="78d62-118">Create a **pipeline** with a copy activity that takes a dataset as an input and a dataset as an output.</span></span> 

<span data-ttu-id="78d62-119">När du använder guiden hello skapas automatiskt JSON definitioner för dessa Data Factory-enheter (länkade tjänster, datauppsättningar och hello pipeline) för dig.</span><span class="sxs-lookup"><span data-stu-id="78d62-119">When you use hello wizard, JSON definitions for these Data Factory entities (linked services, datasets, and hello pipeline) are automatically created for you.</span></span> <span data-ttu-id="78d62-120">När du använder Verktyg/API: er (utom .NET API), kan du definiera dessa Data Factory-enheter med hjälp av hello JSON-format.</span><span class="sxs-lookup"><span data-stu-id="78d62-120">When you use tools/APIs (except .NET API), you define these Data Factory entities by using hello JSON format.</span></span>  <span data-ttu-id="78d62-121">Ett exempel med JSON-definitioner för Data Factory-entiteter som ska använda toocopy data från en webbtabell finns [JSON-exempel: kopiera data från webben tabell tooAzure Blob](#json-example-copy-data-from-web-table-to-azure-blob) i den här artikeln.</span><span class="sxs-lookup"><span data-stu-id="78d62-121">For a sample with JSON definitions for Data Factory entities that are used toocopy data from a web table, see [JSON example: Copy data from Web table tooAzure Blob](#json-example-copy-data-from-web-table-to-azure-blob) section of this article.</span></span> 

<span data-ttu-id="78d62-122">hello följande avsnitt innehåller information om JSON-egenskaper används toodefine Data Factory entiteter specifika tooa Webbtabell:</span><span class="sxs-lookup"><span data-stu-id="78d62-122">hello following sections provide details about JSON properties that are used toodefine Data Factory entities specific tooa Web table:</span></span>

## <a name="linked-service-properties"></a><span data-ttu-id="78d62-123">Länkad tjänstegenskaper</span><span class="sxs-lookup"><span data-stu-id="78d62-123">Linked service properties</span></span>
<span data-ttu-id="78d62-124">hello följande tabell innehåller en beskrivning för JSON-element specifika tooWeb länkad tjänst.</span><span class="sxs-lookup"><span data-stu-id="78d62-124">hello following table provides description for JSON elements specific tooWeb linked service.</span></span>

| <span data-ttu-id="78d62-125">Egenskap</span><span class="sxs-lookup"><span data-stu-id="78d62-125">Property</span></span> | <span data-ttu-id="78d62-126">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="78d62-126">Description</span></span> | <span data-ttu-id="78d62-127">Krävs</span><span class="sxs-lookup"><span data-stu-id="78d62-127">Required</span></span> |
| --- | --- | --- |
| <span data-ttu-id="78d62-128">typ</span><span class="sxs-lookup"><span data-stu-id="78d62-128">type</span></span> |<span data-ttu-id="78d62-129">hello Typegenskapen måste anges till: **Web**</span><span class="sxs-lookup"><span data-stu-id="78d62-129">hello type property must be set to: **Web**</span></span> |<span data-ttu-id="78d62-130">Ja</span><span class="sxs-lookup"><span data-stu-id="78d62-130">Yes</span></span> |
| <span data-ttu-id="78d62-131">URL</span><span class="sxs-lookup"><span data-stu-id="78d62-131">Url</span></span> |<span data-ttu-id="78d62-132">URL: en toohello webbadress</span><span class="sxs-lookup"><span data-stu-id="78d62-132">URL toohello Web source</span></span> |<span data-ttu-id="78d62-133">Ja</span><span class="sxs-lookup"><span data-stu-id="78d62-133">Yes</span></span> |
| <span data-ttu-id="78d62-134">AuthenticationType</span><span class="sxs-lookup"><span data-stu-id="78d62-134">authenticationType</span></span> |<span data-ttu-id="78d62-135">Anonym.</span><span class="sxs-lookup"><span data-stu-id="78d62-135">Anonymous.</span></span> |<span data-ttu-id="78d62-136">Ja</span><span class="sxs-lookup"><span data-stu-id="78d62-136">Yes</span></span> |

### <a name="using-anonymous-authentication"></a><span data-ttu-id="78d62-137">Använder anonym autentisering</span><span class="sxs-lookup"><span data-stu-id="78d62-137">Using Anonymous authentication</span></span>

```json
{
    "name": "web",
    "properties":
    {
        "type": "Web",
        "typeProperties":
        {
            "authenticationType": "Anonymous",
            "url" : "https://en.wikipedia.org/wiki/"
        }
    }
}
```

## <a name="dataset-properties"></a><span data-ttu-id="78d62-138">Egenskaper för datamängd</span><span class="sxs-lookup"><span data-stu-id="78d62-138">Dataset properties</span></span>
<span data-ttu-id="78d62-139">En fullständig lista över egenskaper som är tillgängliga för att definiera datauppsättningarna & avsnitt finns hello [skapa datauppsättningar](data-factory-create-datasets.md) artikel.</span><span class="sxs-lookup"><span data-stu-id="78d62-139">For a full list of sections & properties available for defining datasets, see hello [Creating datasets](data-factory-create-datasets.md) article.</span></span> <span data-ttu-id="78d62-140">Avsnitt som struktur, tillgänglighet och princip på en datamängd JSON är liknande för alla typer av dataset (Azure SQL Azure blob, Azure-tabellen, osv.).</span><span class="sxs-lookup"><span data-stu-id="78d62-140">Sections such as structure, availability, and policy of a dataset JSON are similar for all dataset types (Azure SQL, Azure blob, Azure table, etc.).</span></span>

<span data-ttu-id="78d62-141">Hej **typeProperties** avsnitt är olika för varje typ av dataset och ger information om hello platsen för hello data i datalagret hello.</span><span class="sxs-lookup"><span data-stu-id="78d62-141">hello **typeProperties** section is different for each type of dataset and provides information about hello location of hello data in hello data store.</span></span> <span data-ttu-id="78d62-142">Hej typeProperties avsnittet för dataset av typen **WebTable** har hello följande egenskaper</span><span class="sxs-lookup"><span data-stu-id="78d62-142">hello typeProperties section for dataset of type **WebTable** has hello following properties</span></span>

| <span data-ttu-id="78d62-143">Egenskap</span><span class="sxs-lookup"><span data-stu-id="78d62-143">Property</span></span> | <span data-ttu-id="78d62-144">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="78d62-144">Description</span></span> | <span data-ttu-id="78d62-145">Krävs</span><span class="sxs-lookup"><span data-stu-id="78d62-145">Required</span></span> |
|:--- |:--- |:--- |
| <span data-ttu-id="78d62-146">typ</span><span class="sxs-lookup"><span data-stu-id="78d62-146">type</span></span> |<span data-ttu-id="78d62-147">typ av hello dataset.</span><span class="sxs-lookup"><span data-stu-id="78d62-147">type of hello dataset.</span></span> <span data-ttu-id="78d62-148">måste anges för**WebTable**</span><span class="sxs-lookup"><span data-stu-id="78d62-148">must be set too**WebTable**</span></span> |<span data-ttu-id="78d62-149">Ja</span><span class="sxs-lookup"><span data-stu-id="78d62-149">Yes</span></span> |
| <span data-ttu-id="78d62-150">Sökväg</span><span class="sxs-lookup"><span data-stu-id="78d62-150">path</span></span> |<span data-ttu-id="78d62-151">En relativ URL toohello resurs som innehåller hello tabell.</span><span class="sxs-lookup"><span data-stu-id="78d62-151">A relative URL toohello resource that contains hello table.</span></span> |<span data-ttu-id="78d62-152">Nej.</span><span class="sxs-lookup"><span data-stu-id="78d62-152">No.</span></span> <span data-ttu-id="78d62-153">Om sökvägen inte anges används endast hello-URL som anges i tjänstdefinitionen hello länkad.</span><span class="sxs-lookup"><span data-stu-id="78d62-153">When path is not specified, only hello URL specified in hello linked service definition is used.</span></span> |
| <span data-ttu-id="78d62-154">Index</span><span class="sxs-lookup"><span data-stu-id="78d62-154">index</span></span> |<span data-ttu-id="78d62-155">hello index för hello tabellen i hello resurs.</span><span class="sxs-lookup"><span data-stu-id="78d62-155">hello index of hello table in hello resource.</span></span> <span data-ttu-id="78d62-156">Se [Get-index för en tabell i en HTML-sida](#get-index-of-a-table-in-an-html-page) avsnittet steg toogetting index för en tabell i en HTML-sida.</span><span class="sxs-lookup"><span data-stu-id="78d62-156">See [Get index of a table in an HTML page](#get-index-of-a-table-in-an-html-page) section for steps toogetting index of a table in an HTML page.</span></span> |<span data-ttu-id="78d62-157">Ja</span><span class="sxs-lookup"><span data-stu-id="78d62-157">Yes</span></span> |

<span data-ttu-id="78d62-158">**Exempel:**</span><span class="sxs-lookup"><span data-stu-id="78d62-158">**Example:**</span></span>

```json
{
    "name": "WebTableInput",
    "properties": {
        "type": "WebTable",
        "linkedServiceName": "WebLinkedService",
        "typeProperties": {
            "index": 1,
            "path": "AFI's_100_Years...100_Movies"
        },
        "external": true,
        "availability": {
            "frequency": "Hour",
            "interval":  1
        }
    }
}
```

## <a name="copy-activity-properties"></a><span data-ttu-id="78d62-159">Kopiera egenskaper för aktivitet</span><span class="sxs-lookup"><span data-stu-id="78d62-159">Copy activity properties</span></span>
<span data-ttu-id="78d62-160">En fullständig lista över avsnitt & egenskaper som är tillgängliga för att definiera aktiviteter finns hello [skapar Pipelines](data-factory-create-pipelines.md) artikel.</span><span class="sxs-lookup"><span data-stu-id="78d62-160">For a full list of sections & properties available for defining activities, see hello [Creating Pipelines](data-factory-create-pipelines.md) article.</span></span> <span data-ttu-id="78d62-161">Egenskaper som namn, beskrivning, ingående och utgående tabeller och principen är tillgängliga för alla typer av aktiviteter.</span><span class="sxs-lookup"><span data-stu-id="78d62-161">Properties such as name, description, input and output tables, and policy are available for all types of activities.</span></span>

<span data-ttu-id="78d62-162">De egenskaper som är tillgängliga under hello typeProperties i hello aktivitet varierar beroende på varje aktivitetstyp.</span><span class="sxs-lookup"><span data-stu-id="78d62-162">Whereas, properties available in hello typeProperties section of hello activity vary with each activity type.</span></span> <span data-ttu-id="78d62-163">För Kopieringsaktivitet kan varierar de beroende på hello typer av datakällor och sänkor.</span><span class="sxs-lookup"><span data-stu-id="78d62-163">For Copy activity, they vary depending on hello types of sources and sinks.</span></span>

<span data-ttu-id="78d62-164">För närvarande när hello-källan i en Kopieringsaktivitet är av typen **WebSource**, inga ytterligare egenskaper som stöds.</span><span class="sxs-lookup"><span data-stu-id="78d62-164">Currently, when hello source in copy activity is of type **WebSource**, no additional properties are supported.</span></span>


## <a name="json-example-copy-data-from-web-table-tooazure-blob"></a><span data-ttu-id="78d62-165">JSON-exempel: kopiera data från webben tabell tooAzure Blob</span><span class="sxs-lookup"><span data-stu-id="78d62-165">JSON example: Copy data from Web table tooAzure Blob</span></span>
<span data-ttu-id="78d62-166">följande exempel visar hello:</span><span class="sxs-lookup"><span data-stu-id="78d62-166">hello following sample shows:</span></span>

1. <span data-ttu-id="78d62-167">En länkad tjänst av typen [Web](#linked-service-properties).</span><span class="sxs-lookup"><span data-stu-id="78d62-167">A linked service of type [Web](#linked-service-properties).</span></span>
2. <span data-ttu-id="78d62-168">En länkad tjänst av typen [AzureStorage](data-factory-azure-blob-connector.md#linked-service-properties).</span><span class="sxs-lookup"><span data-stu-id="78d62-168">A linked service of type [AzureStorage](data-factory-azure-blob-connector.md#linked-service-properties).</span></span>
3. <span data-ttu-id="78d62-169">Indata [dataset](data-factory-create-datasets.md) av typen [WebTable](#dataset-properties).</span><span class="sxs-lookup"><span data-stu-id="78d62-169">An input [dataset](data-factory-create-datasets.md) of type [WebTable](#dataset-properties).</span></span>
4. <span data-ttu-id="78d62-170">Utdata [dataset](data-factory-create-datasets.md) av typen [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties).</span><span class="sxs-lookup"><span data-stu-id="78d62-170">An output [dataset](data-factory-create-datasets.md) of type [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties).</span></span>
5. <span data-ttu-id="78d62-171">En [pipeline](data-factory-create-pipelines.md) med Kopieringsaktiviteten som använder [WebSource](#copy-activity-properties) och [BlobSink](data-factory-azure-blob-connector.md#copy-activity-properties).</span><span class="sxs-lookup"><span data-stu-id="78d62-171">A [pipeline](data-factory-create-pipelines.md) with Copy Activity that uses [WebSource](#copy-activity-properties) and [BlobSink](data-factory-azure-blob-connector.md#copy-activity-properties).</span></span>

<span data-ttu-id="78d62-172">hello exemplet kopierar data från en webbserver tabell tooan Azure blob varje timme.</span><span class="sxs-lookup"><span data-stu-id="78d62-172">hello sample copies data from a Web table tooan Azure blob every hour.</span></span> <span data-ttu-id="78d62-173">hello JSON egenskaper som används i exemplen beskrivs i hello-exempel i följande avsnitt.</span><span class="sxs-lookup"><span data-stu-id="78d62-173">hello JSON properties used in these samples are described in sections following hello samples.</span></span>

<span data-ttu-id="78d62-174">hello som följande exempel visar hur toocopy data från en webbserver tabell tooan Azure blob.</span><span class="sxs-lookup"><span data-stu-id="78d62-174">hello following sample shows how toocopy data from a Web table tooan Azure blob.</span></span> <span data-ttu-id="78d62-175">Dock datan kan kopieras direkt tooany av hello egenskaperna anges i hello [Data Movement aktiviteter](data-factory-data-movement-activities.md) artikel med hjälp av hello Kopieringsaktiviteten i Azure Data Factory.</span><span class="sxs-lookup"><span data-stu-id="78d62-175">However, data can be copied directly tooany of hello sinks stated in hello [Data Movement Activities](data-factory-data-movement-activities.md) article by using hello Copy Activity in Azure Data Factory.</span></span>

<span data-ttu-id="78d62-176">**Web länkade tjänsten** det här exemplet använder hello Web länkade tjänsten med anonym autentisering.</span><span class="sxs-lookup"><span data-stu-id="78d62-176">**Web linked service** This example uses hello Web linked service with anonymous authentication.</span></span> <span data-ttu-id="78d62-177">Se [Web länkade tjänsten](#linked-service-properties) avsnittet för olika typer av autentisering som du kan använda.</span><span class="sxs-lookup"><span data-stu-id="78d62-177">See [Web linked service](#linked-service-properties) section for different types of authentication you can use.</span></span>

```json
{
    "name": "WebLinkedService",
    "properties":
    {
        "type": "Web",
        "typeProperties":
        {
            "authenticationType": "Anonymous",
            "url" : "https://en.wikipedia.org/wiki/"
        }
    }
}
```

<span data-ttu-id="78d62-178">**Länkad Azure Storage-tjänst**</span><span class="sxs-lookup"><span data-stu-id="78d62-178">**Azure Storage linked service**</span></span>

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

<span data-ttu-id="78d62-179">**WebTable inkommande dataset** inställningen **externa** för**SANT** informerar hello Data Factory-tjänsten som hello dataset är externa toohello data factory och inte tillverkas av en aktivitet i hello data factory.</span><span class="sxs-lookup"><span data-stu-id="78d62-179">**WebTable input dataset** Setting **external** too**true** informs hello Data Factory service that hello dataset is external toohello data factory and is not produced by an activity in hello data factory.</span></span>

> [!NOTE]
> <span data-ttu-id="78d62-180">Se [Get-index för en tabell i en HTML-sida](#get-index-of-a-table-in-an-html-page) avsnittet steg toogetting index för en tabell i en HTML-sida.</span><span class="sxs-lookup"><span data-stu-id="78d62-180">See [Get index of a table in an HTML page](#get-index-of-a-table-in-an-html-page) section for steps toogetting index of a table in an HTML page.</span></span>  
>
>

```json
{
    "name": "WebTableInput",
    "properties": {
        "type": "WebTable",
        "linkedServiceName": "WebLinkedService",
        "typeProperties": {
            "index": 1,
            "path": "AFI's_100_Years...100_Movies"
        },
        "external": true,
        "availability": {
            "frequency": "Hour",
            "interval":  1
        }
    }
}
```


<span data-ttu-id="78d62-181">**Azure Blob utdatauppsättningen**</span><span class="sxs-lookup"><span data-stu-id="78d62-181">**Azure Blob output dataset**</span></span>

<span data-ttu-id="78d62-182">Data skrivs tooa nya blob varje timme (frekvens: timme, intervall: 1).</span><span class="sxs-lookup"><span data-stu-id="78d62-182">Data is written tooa new blob every hour (frequency: hour, interval: 1).</span></span>

```json
{
    "name": "AzureBlobOutput",
    "properties":
    {
        "type": "AzureBlob",
        "linkedServiceName": "AzureStorageLinkedService",
        "typeProperties":
        {
            "folderPath": "adfgetstarted/Movies"
        },
        "availability":
        {
            "frequency": "Hour",
            "interval": 1
        }
    }
}
```



<span data-ttu-id="78d62-183">**Pipeline med kopieringsaktiviteten**</span><span class="sxs-lookup"><span data-stu-id="78d62-183">**Pipeline with Copy activity**</span></span>

<span data-ttu-id="78d62-184">hello pipelinen innehåller en kopia-aktivitet som är konfigurerade toouse hello inkommande och utgående datauppsättningar och är schemalagda toorun varje timme.</span><span class="sxs-lookup"><span data-stu-id="78d62-184">hello pipeline contains a Copy Activity that is configured toouse hello input and output datasets and is scheduled toorun every hour.</span></span> <span data-ttu-id="78d62-185">I hello pipeline JSON-definitionen hello **källa** typ har angetts för**WebSource** och **sink** typ har angetts för**BlobSink**.</span><span class="sxs-lookup"><span data-stu-id="78d62-185">In hello pipeline JSON definition, hello **source** type is set too**WebSource** and **sink** type is set too**BlobSink**.</span></span>

<span data-ttu-id="78d62-186">Se [WebSource Typegenskaper](#copy-activity-type-properties) hello lista över egenskaper som stöds av hello WebSource.</span><span class="sxs-lookup"><span data-stu-id="78d62-186">See [WebSource type properties](#copy-activity-type-properties) for hello list of properties supported by hello WebSource.</span></span>

```json
{  
    "name":"SamplePipeline",
    "properties":{  
    "start":"2014-06-01T18:00:00",
    "end":"2014-06-01T19:00:00",
    "description":"pipeline with copy activity",
    "activities":[  
      {
        "name": "WebTableToAzureBlob",
        "description": "Copy from a Web table tooan Azure blob",
        "type": "Copy",
        "inputs": [
          {
            "name": "WebTableInput"
          }
        ],
        "outputs": [
          {
            "name": "AzureBlobOutput"
          }
        ],
        "typeProperties": {
          "source": {
            "type": "WebSource"
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

## <a name="get-index-of-a-table-in-an-html-page"></a><span data-ttu-id="78d62-187">Hämta index för en tabell i en HTML-sida</span><span class="sxs-lookup"><span data-stu-id="78d62-187">Get index of a table in an HTML page</span></span>
1. <span data-ttu-id="78d62-188">Starta **Excel 2016** och växla toohello **Data** fliken.</span><span class="sxs-lookup"><span data-stu-id="78d62-188">Launch **Excel 2016** and switch toohello **Data** tab.</span></span>  
2. <span data-ttu-id="78d62-189">Klicka på **ny fråga** hello peka på verktygsfältet för**från andra källor** och på **från webben**.</span><span class="sxs-lookup"><span data-stu-id="78d62-189">Click **New Query** on hello toolbar, point too**From Other Sources** and click **From Web**.</span></span>

    ![Power Query-menyn](./media/data-factory-web-table-connector/PowerQuery-Menu.png)
3. <span data-ttu-id="78d62-191">I hello **från webben** dialogrutan Ange **URL** som du vill använda i länkad tjänst-JSON (till exempel: https://en.wikipedia.org/wiki/) tillsammans med sökvägen som du anger för hello datauppsättningen (till exempel: Afrika % 27s_100_Years... 100_Movies) och klicka på **OK**.</span><span class="sxs-lookup"><span data-stu-id="78d62-191">In hello **From Web** dialog box, enter **URL** that you would use in linked service JSON (for example: https://en.wikipedia.org/wiki/) along with path you would specify for hello dataset (for example: AFI%27s_100_Years...100_Movies), and click **OK**.</span></span>

    ![Från webben dialogrutan](./media/data-factory-web-table-connector/FromWeb-DialogBox.png)

    <span data-ttu-id="78d62-193">URL som används i det här exemplet: https://en.wikipedia.org/wiki/AFI%27s_100_Years...100_Movies</span><span class="sxs-lookup"><span data-stu-id="78d62-193">URL used in this example: https://en.wikipedia.org/wiki/AFI%27s_100_Years...100_Movies</span></span>
4. <span data-ttu-id="78d62-194">Om du ser **åtkomst till webbinnehåll** dialogrutan, Välj hello höger **URL**, **autentisering**, och klicka på **Anslut**.</span><span class="sxs-lookup"><span data-stu-id="78d62-194">If you see **Access Web content** dialog box, select hello right **URL**, **authentication**, and click **Connect**.</span></span>

   ![Åtkomst till innehåll dialogrutan för webbplats](./media/data-factory-web-table-connector/AccessWebContentDialog.png)
5. <span data-ttu-id="78d62-196">Klicka på en **tabell** objektet i trädet hello visa toosee innehåll från hello tabell och klicka sedan på **redigera** knappen längst ned hello.</span><span class="sxs-lookup"><span data-stu-id="78d62-196">Click a **table** item in hello tree view toosee content from hello table and then click **Edit** button at hello bottom.</span></span>  

   ![Navigator dialogrutan](./media/data-factory-web-table-connector/Navigator-DialogBox.png)
6. <span data-ttu-id="78d62-198">I hello **frågeredigeraren** -fönstret klickar du på **avancerade Editor** hello i verktygsfältet.</span><span class="sxs-lookup"><span data-stu-id="78d62-198">In hello **Query Editor** window, click **Advanced Editor** button on hello toolbar.</span></span>

    ![Knappen Avancerat redigeraren](./media/data-factory-web-table-connector/QueryEditor-AdvancedEditorButton.png)
7. <span data-ttu-id="78d62-200">Hello avancerade redigeraren i dialogrutan hello nummer bredvid för ”källa” är hello index.</span><span class="sxs-lookup"><span data-stu-id="78d62-200">In hello Advanced Editor dialog box, hello number next too"Source" is hello index.</span></span>

    ![Avancerad redigerare - Index](./media/data-factory-web-table-connector/AdvancedEditor-Index.png)

<span data-ttu-id="78d62-202">Om du använder Excel 2013 använder [Microsoft Power Query för Excel](https://www.microsoft.com/download/details.aspx?id=39379) tooget hello index.</span><span class="sxs-lookup"><span data-stu-id="78d62-202">If you are using Excel 2013, use [Microsoft Power Query for Excel](https://www.microsoft.com/download/details.aspx?id=39379) tooget hello index.</span></span> <span data-ttu-id="78d62-203">Se [Anslut tooa webbsida](https://support.office.com/article/Connect-to-a-web-page-Power-Query-b2725d67-c9e8-43e6-a590-c0a175bd64d8) artikeln för information.</span><span class="sxs-lookup"><span data-stu-id="78d62-203">See [Connect tooa web page](https://support.office.com/article/Connect-to-a-web-page-Power-Query-b2725d67-c9e8-43e6-a590-c0a175bd64d8) article for details.</span></span> <span data-ttu-id="78d62-204">hello stegen är ungefär som om du använder [Microsoft Power BI för skrivbordet](https://powerbi.microsoft.com/desktop/).</span><span class="sxs-lookup"><span data-stu-id="78d62-204">hello steps are similar if you are using [Microsoft Power BI for Desktop](https://powerbi.microsoft.com/desktop/).</span></span>

> [!NOTE]
> <span data-ttu-id="78d62-205">toomap kolumner från källan dataset toocolumns från sink dataset finns [mappa dataset kolumner i Azure Data Factory](data-factory-map-columns.md).</span><span class="sxs-lookup"><span data-stu-id="78d62-205">toomap columns from source dataset toocolumns from sink dataset, see [Mapping dataset columns in Azure Data Factory](data-factory-map-columns.md).</span></span>

## <a name="performance-and-tuning"></a><span data-ttu-id="78d62-206">Prestanda och finjustering</span><span class="sxs-lookup"><span data-stu-id="78d62-206">Performance and Tuning</span></span>
<span data-ttu-id="78d62-207">Se [kopiera aktivitet prestanda och justera guiden](data-factory-copy-activity-performance.md) toolearn om nyckeln faktorer som påverkan prestanda för flytt av data (Kopieringsaktiviteten) i Azure Data Factory och olika sätt toooptimize den.</span><span class="sxs-lookup"><span data-stu-id="78d62-207">See [Copy Activity Performance & Tuning Guide](data-factory-copy-activity-performance.md) toolearn about key factors that impact performance of data movement (Copy Activity) in Azure Data Factory and various ways toooptimize it.</span></span>
