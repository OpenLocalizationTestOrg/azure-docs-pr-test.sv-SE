---
title: "Flytta data från webben tabellen med hjälp av Azure Data Factory | Microsoft Docs"
description: "Läs mer om hur du flyttar data från en tabell i en webbsida med Azure Data Factory."
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
ms.openlocfilehash: 9e006bc7289fa0239f1650ac6ad43dd159e3c7e0
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/03/2017
---
# <a name="move-data-from-a-web-table-source-using-azure-data-factory"></a><span data-ttu-id="1da21-103">Flytta data från en webbadress för tabellen med hjälp av Azure Data Factory</span><span class="sxs-lookup"><span data-stu-id="1da21-103">Move data from a Web table source using Azure Data Factory</span></span>
<span data-ttu-id="1da21-104">Den här artikeln beskrivs hur du använder aktiviteten kopiera i Azure Data Factory för att flytta data från en tabell i en webbsida i ett datalager stöds sink.</span><span class="sxs-lookup"><span data-stu-id="1da21-104">This article outlines how to use the Copy Activity in Azure Data Factory to move data from a table in a Web page to a supported sink data store.</span></span> <span data-ttu-id="1da21-105">Den här artikeln bygger på den [data movement aktiviteter](data-factory-data-movement-activities.md) artikel som ger en allmän översikt över dataflyttning kopieringsaktiviteten och listan över datakällor som stöds som källor/sänkor.</span><span class="sxs-lookup"><span data-stu-id="1da21-105">This article builds on the [data movement activities](data-factory-data-movement-activities.md) article that presents a general overview of data movement with copy activity and the list of data stores supported as sources/sinks.</span></span>

<span data-ttu-id="1da21-106">Data factory stöder för närvarande endast flytta data från en webbserver tabell till andra databaser, men inte flytta data från andra data lagrar till ett mål för Web-tabellen.</span><span class="sxs-lookup"><span data-stu-id="1da21-106">Data factory currently supports only moving data from a Web table to other data stores, but not moving data from other data stores to a Web table destination.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="1da21-107">Den här Web connector stöder för närvarande endast extrahera innehållet från en HTML-sida.</span><span class="sxs-lookup"><span data-stu-id="1da21-107">This Web connector currently supports only extracting table content from an HTML page.</span></span> <span data-ttu-id="1da21-108">Använd för att hämta data från en HTTP/s-slutpunkt [HTTP-anslutningen](data-factory-http-connector.md) i stället.</span><span class="sxs-lookup"><span data-stu-id="1da21-108">To retrieve data from a HTTP/s endpoint, use [HTTP connector](data-factory-http-connector.md) instead.</span></span>

## <a name="getting-started"></a><span data-ttu-id="1da21-109">Komma igång</span><span class="sxs-lookup"><span data-stu-id="1da21-109">Getting started</span></span>
<span data-ttu-id="1da21-110">Du kan skapa en pipeline med en kopia-aktivitet som flyttar data från en lokal Cassandra data store med hjälp av olika verktyg/API: er.</span><span class="sxs-lookup"><span data-stu-id="1da21-110">You can create a pipeline with a copy activity that moves data from an on-premises Cassandra data store by using different tools/APIs.</span></span> 

- <span data-ttu-id="1da21-111">Det enklaste sättet att skapa en pipeline är att använda den **guiden Kopiera**.</span><span class="sxs-lookup"><span data-stu-id="1da21-111">The easiest way to create a pipeline is to use the **Copy Wizard**.</span></span> <span data-ttu-id="1da21-112">Finns [Självstudier: skapa en pipeline med hjälp av guiden Kopiera](data-factory-copy-data-wizard-tutorial.md) för en snabb genomgång om hur du skapar en pipeline med hjälp av guiden Kopiera data.</span><span class="sxs-lookup"><span data-stu-id="1da21-112">See [Tutorial: Create a pipeline using Copy Wizard](data-factory-copy-data-wizard-tutorial.md) for a quick walkthrough on creating a pipeline using the Copy data wizard.</span></span> 
- <span data-ttu-id="1da21-113">Du kan också använda följande verktyg för att skapa en pipeline: **Azure-portalen**, **Visual Studio**, **Azure PowerShell**, **Azure Resource Manager-mall**, **.NET API**, och **REST API**.</span><span class="sxs-lookup"><span data-stu-id="1da21-113">You can also use the following tools to create a pipeline: **Azure portal**, **Visual Studio**, **Azure PowerShell**, **Azure Resource Manager template**, **.NET API**, and **REST API**.</span></span> <span data-ttu-id="1da21-114">Se [kopiera aktivitet kursen](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) för stegvisa instruktioner för att skapa en pipeline med en Kopieringsaktivitet.</span><span class="sxs-lookup"><span data-stu-id="1da21-114">See [Copy activity tutorial](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) for step-by-step instructions to create a pipeline with a copy activity.</span></span> 

<span data-ttu-id="1da21-115">Om du använder verktyg eller API: er, kan du utföra följande steg för att skapa en pipeline som flyttar data från ett dataarkiv som källa till ett dataarkiv som mottagare:</span><span class="sxs-lookup"><span data-stu-id="1da21-115">Whether you use the tools or APIs, you perform the following steps to create a pipeline that moves data from a source data store to a sink data store:</span></span>

1. <span data-ttu-id="1da21-116">Skapa **länkade tjänster** att länka inkommande och utgående data lagras till din data factory.</span><span class="sxs-lookup"><span data-stu-id="1da21-116">Create **linked services** to link input and output data stores to your data factory.</span></span>
2. <span data-ttu-id="1da21-117">Skapa **datauppsättningar** att representera inkommande och utgående data för kopieringen.</span><span class="sxs-lookup"><span data-stu-id="1da21-117">Create **datasets** to represent input and output data for the copy operation.</span></span> 
3. <span data-ttu-id="1da21-118">Skapa en **pipeline** med en kopia-aktivitet som tar en datamängd som indata och en dataset som utdata.</span><span class="sxs-lookup"><span data-stu-id="1da21-118">Create a **pipeline** with a copy activity that takes a dataset as an input and a dataset as an output.</span></span> 

<span data-ttu-id="1da21-119">När du använder guiden skapas JSON definitioner för dessa Data Factory-enheter (länkade tjänster, datauppsättningar och pipelinen) automatiskt för dig.</span><span class="sxs-lookup"><span data-stu-id="1da21-119">When you use the wizard, JSON definitions for these Data Factory entities (linked services, datasets, and the pipeline) are automatically created for you.</span></span> <span data-ttu-id="1da21-120">När du använder Verktyg/API: er (utom .NET API), kan du definiera dessa Data Factory-enheter med hjälp av JSON-format.</span><span class="sxs-lookup"><span data-stu-id="1da21-120">When you use tools/APIs (except .NET API), you define these Data Factory entities by using the JSON format.</span></span>  <span data-ttu-id="1da21-121">Ett exempel med JSON-definitioner för Data Factory-entiteter som används för att kopiera data från en webbtabell finns [JSON-exempel: kopiera data från Webbtabell till Azure Blob](#json-example-copy-data-from-web-table-to-azure-blob) i den här artikeln.</span><span class="sxs-lookup"><span data-stu-id="1da21-121">For a sample with JSON definitions for Data Factory entities that are used to copy data from a web table, see [JSON example: Copy data from Web table to Azure Blob](#json-example-copy-data-from-web-table-to-azure-blob) section of this article.</span></span> 

<span data-ttu-id="1da21-122">Följande avsnitt innehåller information om JSON-egenskaper som används för att definiera Data Factory entiteter i en webbtabell:</span><span class="sxs-lookup"><span data-stu-id="1da21-122">The following sections provide details about JSON properties that are used to define Data Factory entities specific to a Web table:</span></span>

## <a name="linked-service-properties"></a><span data-ttu-id="1da21-123">Länkad tjänstegenskaper</span><span class="sxs-lookup"><span data-stu-id="1da21-123">Linked service properties</span></span>
<span data-ttu-id="1da21-124">Följande tabell innehåller en beskrivning för JSON-element som är specifika för länkade webbtjänsten.</span><span class="sxs-lookup"><span data-stu-id="1da21-124">The following table provides description for JSON elements specific to Web linked service.</span></span>

| <span data-ttu-id="1da21-125">Egenskap</span><span class="sxs-lookup"><span data-stu-id="1da21-125">Property</span></span> | <span data-ttu-id="1da21-126">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="1da21-126">Description</span></span> | <span data-ttu-id="1da21-127">Krävs</span><span class="sxs-lookup"><span data-stu-id="1da21-127">Required</span></span> |
| --- | --- | --- |
| <span data-ttu-id="1da21-128">typ</span><span class="sxs-lookup"><span data-stu-id="1da21-128">type</span></span> |<span data-ttu-id="1da21-129">Egenskapen type måste anges till: **Web**</span><span class="sxs-lookup"><span data-stu-id="1da21-129">The type property must be set to: **Web**</span></span> |<span data-ttu-id="1da21-130">Ja</span><span class="sxs-lookup"><span data-stu-id="1da21-130">Yes</span></span> |
| <span data-ttu-id="1da21-131">URL</span><span class="sxs-lookup"><span data-stu-id="1da21-131">Url</span></span> |<span data-ttu-id="1da21-132">URL till webbadressen</span><span class="sxs-lookup"><span data-stu-id="1da21-132">URL to the Web source</span></span> |<span data-ttu-id="1da21-133">Ja</span><span class="sxs-lookup"><span data-stu-id="1da21-133">Yes</span></span> |
| <span data-ttu-id="1da21-134">AuthenticationType</span><span class="sxs-lookup"><span data-stu-id="1da21-134">authenticationType</span></span> |<span data-ttu-id="1da21-135">Anonym.</span><span class="sxs-lookup"><span data-stu-id="1da21-135">Anonymous.</span></span> |<span data-ttu-id="1da21-136">Ja</span><span class="sxs-lookup"><span data-stu-id="1da21-136">Yes</span></span> |

### <a name="using-anonymous-authentication"></a><span data-ttu-id="1da21-137">Använder anonym autentisering</span><span class="sxs-lookup"><span data-stu-id="1da21-137">Using Anonymous authentication</span></span>

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

## <a name="dataset-properties"></a><span data-ttu-id="1da21-138">Egenskaper för datamängd</span><span class="sxs-lookup"><span data-stu-id="1da21-138">Dataset properties</span></span>
<span data-ttu-id="1da21-139">En fullständig lista över egenskaper som är tillgängliga för att definiera datauppsättningarna & avsnitt finns i [skapa datauppsättningar](data-factory-create-datasets.md) artikel.</span><span class="sxs-lookup"><span data-stu-id="1da21-139">For a full list of sections & properties available for defining datasets, see the [Creating datasets](data-factory-create-datasets.md) article.</span></span> <span data-ttu-id="1da21-140">Avsnitt som struktur, tillgänglighet och princip på en datamängd JSON är liknande för alla typer av dataset (Azure SQL Azure blob, Azure-tabellen, osv.).</span><span class="sxs-lookup"><span data-stu-id="1da21-140">Sections such as structure, availability, and policy of a dataset JSON are similar for all dataset types (Azure SQL, Azure blob, Azure table, etc.).</span></span>

<span data-ttu-id="1da21-141">Den **typeProperties** avsnitt är olika för varje typ av dataset och innehåller information om placeringen av data i datalagret.</span><span class="sxs-lookup"><span data-stu-id="1da21-141">The **typeProperties** section is different for each type of dataset and provides information about the location of the data in the data store.</span></span> <span data-ttu-id="1da21-142">TypeProperties avsnittet för dataset av typen **WebTable** har följande egenskaper</span><span class="sxs-lookup"><span data-stu-id="1da21-142">The typeProperties section for dataset of type **WebTable** has the following properties</span></span>

| <span data-ttu-id="1da21-143">Egenskap</span><span class="sxs-lookup"><span data-stu-id="1da21-143">Property</span></span> | <span data-ttu-id="1da21-144">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="1da21-144">Description</span></span> | <span data-ttu-id="1da21-145">Krävs</span><span class="sxs-lookup"><span data-stu-id="1da21-145">Required</span></span> |
|:--- |:--- |:--- |
| <span data-ttu-id="1da21-146">typ</span><span class="sxs-lookup"><span data-stu-id="1da21-146">type</span></span> |<span data-ttu-id="1da21-147">Typ av datauppsättningen.</span><span class="sxs-lookup"><span data-stu-id="1da21-147">type of the dataset.</span></span> <span data-ttu-id="1da21-148">måste anges till **WebTable**</span><span class="sxs-lookup"><span data-stu-id="1da21-148">must be set to **WebTable**</span></span> |<span data-ttu-id="1da21-149">Ja</span><span class="sxs-lookup"><span data-stu-id="1da21-149">Yes</span></span> |
| <span data-ttu-id="1da21-150">Sökväg</span><span class="sxs-lookup"><span data-stu-id="1da21-150">path</span></span> |<span data-ttu-id="1da21-151">En relativ URL till den resurs som innehåller tabellen.</span><span class="sxs-lookup"><span data-stu-id="1da21-151">A relative URL to the resource that contains the table.</span></span> |<span data-ttu-id="1da21-152">Nej.</span><span class="sxs-lookup"><span data-stu-id="1da21-152">No.</span></span> <span data-ttu-id="1da21-153">Om sökvägen inte anges används den URL som angavs i definitionen länkade tjänsten.</span><span class="sxs-lookup"><span data-stu-id="1da21-153">When path is not specified, only the URL specified in the linked service definition is used.</span></span> |
| <span data-ttu-id="1da21-154">Index</span><span class="sxs-lookup"><span data-stu-id="1da21-154">index</span></span> |<span data-ttu-id="1da21-155">Index för tabellen i resursen.</span><span class="sxs-lookup"><span data-stu-id="1da21-155">The index of the table in the resource.</span></span> <span data-ttu-id="1da21-156">Se [Get-index för en tabell i en HTML-sida](#get-index-of-a-table-in-an-html-page) avsnittet steg för att få index för en tabell i en HTML-sida.</span><span class="sxs-lookup"><span data-stu-id="1da21-156">See [Get index of a table in an HTML page](#get-index-of-a-table-in-an-html-page) section for steps to getting index of a table in an HTML page.</span></span> |<span data-ttu-id="1da21-157">Ja</span><span class="sxs-lookup"><span data-stu-id="1da21-157">Yes</span></span> |

<span data-ttu-id="1da21-158">**Exempel:**</span><span class="sxs-lookup"><span data-stu-id="1da21-158">**Example:**</span></span>

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

## <a name="copy-activity-properties"></a><span data-ttu-id="1da21-159">Kopiera egenskaper för aktivitet</span><span class="sxs-lookup"><span data-stu-id="1da21-159">Copy activity properties</span></span>
<span data-ttu-id="1da21-160">En fullständig lista över avsnitt & egenskaper som är tillgängliga för att definiera aktiviteter finns i [skapar Pipelines](data-factory-create-pipelines.md) artikel.</span><span class="sxs-lookup"><span data-stu-id="1da21-160">For a full list of sections & properties available for defining activities, see the [Creating Pipelines](data-factory-create-pipelines.md) article.</span></span> <span data-ttu-id="1da21-161">Egenskaper som namn, beskrivning, ingående och utgående tabeller och principen är tillgängliga för alla typer av aktiviteter.</span><span class="sxs-lookup"><span data-stu-id="1da21-161">Properties such as name, description, input and output tables, and policy are available for all types of activities.</span></span>

<span data-ttu-id="1da21-162">De egenskaper som är tillgängliga i avsnittet typeProperties i aktiviteten varierar beroende på varje aktivitetstyp.</span><span class="sxs-lookup"><span data-stu-id="1da21-162">Whereas, properties available in the typeProperties section of the activity vary with each activity type.</span></span> <span data-ttu-id="1da21-163">För Kopieringsaktivitet kan variera de beroende på vilka typer av datakällor och sänkor.</span><span class="sxs-lookup"><span data-stu-id="1da21-163">For Copy activity, they vary depending on the types of sources and sinks.</span></span>

<span data-ttu-id="1da21-164">För närvarande när datakällan i en Kopieringsaktivitet är av typen **WebSource**, inga ytterligare egenskaper som stöds.</span><span class="sxs-lookup"><span data-stu-id="1da21-164">Currently, when the source in copy activity is of type **WebSource**, no additional properties are supported.</span></span>


## <a name="json-example-copy-data-from-web-table-to-azure-blob"></a><span data-ttu-id="1da21-165">JSON-exempel: kopiera data från Webbtabell till Azure-Blob</span><span class="sxs-lookup"><span data-stu-id="1da21-165">JSON example: Copy data from Web table to Azure Blob</span></span>
<span data-ttu-id="1da21-166">I följande exempel visas:</span><span class="sxs-lookup"><span data-stu-id="1da21-166">The following sample shows:</span></span>

1. <span data-ttu-id="1da21-167">En länkad tjänst av typen [Web](#linked-service-properties).</span><span class="sxs-lookup"><span data-stu-id="1da21-167">A linked service of type [Web](#linked-service-properties).</span></span>
2. <span data-ttu-id="1da21-168">En länkad tjänst av typen [AzureStorage](data-factory-azure-blob-connector.md#linked-service-properties).</span><span class="sxs-lookup"><span data-stu-id="1da21-168">A linked service of type [AzureStorage](data-factory-azure-blob-connector.md#linked-service-properties).</span></span>
3. <span data-ttu-id="1da21-169">Indata [dataset](data-factory-create-datasets.md) av typen [WebTable](#dataset-properties).</span><span class="sxs-lookup"><span data-stu-id="1da21-169">An input [dataset](data-factory-create-datasets.md) of type [WebTable](#dataset-properties).</span></span>
4. <span data-ttu-id="1da21-170">Utdata [dataset](data-factory-create-datasets.md) av typen [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties).</span><span class="sxs-lookup"><span data-stu-id="1da21-170">An output [dataset](data-factory-create-datasets.md) of type [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties).</span></span>
5. <span data-ttu-id="1da21-171">En [pipeline](data-factory-create-pipelines.md) med Kopieringsaktiviteten som använder [WebSource](#copy-activity-properties) och [BlobSink](data-factory-azure-blob-connector.md#copy-activity-properties).</span><span class="sxs-lookup"><span data-stu-id="1da21-171">A [pipeline](data-factory-create-pipelines.md) with Copy Activity that uses [WebSource](#copy-activity-properties) and [BlobSink](data-factory-azure-blob-connector.md#copy-activity-properties).</span></span>

<span data-ttu-id="1da21-172">Exemplet kopierar data från en webbserver-tabell till en Azure blob varje timme.</span><span class="sxs-lookup"><span data-stu-id="1da21-172">The sample copies data from a Web table to an Azure blob every hour.</span></span> <span data-ttu-id="1da21-173">JSON-egenskaper som används i exemplen beskrivs i exemplen i följande avsnitt.</span><span class="sxs-lookup"><span data-stu-id="1da21-173">The JSON properties used in these samples are described in sections following the samples.</span></span>

<span data-ttu-id="1da21-174">I följande exempel visas hur du kopierar data från en webbserver-tabell till en Azure blob.</span><span class="sxs-lookup"><span data-stu-id="1da21-174">The following sample shows how to copy data from a Web table to an Azure blob.</span></span> <span data-ttu-id="1da21-175">Dock datan kan kopieras direkt till någon av sänkor som anges i den [Data Movement aktiviteter](data-factory-data-movement-activities.md) artikel med hjälp av aktiviteten kopiera i Azure Data Factory.</span><span class="sxs-lookup"><span data-stu-id="1da21-175">However, data can be copied directly to any of the sinks stated in the [Data Movement Activities](data-factory-data-movement-activities.md) article by using the Copy Activity in Azure Data Factory.</span></span>

<span data-ttu-id="1da21-176">**Web länkade tjänsten** det här exemplet använder webbtjänsten länkade med anonym autentisering.</span><span class="sxs-lookup"><span data-stu-id="1da21-176">**Web linked service** This example uses the Web linked service with anonymous authentication.</span></span> <span data-ttu-id="1da21-177">Se [Web länkade tjänsten](#linked-service-properties) avsnittet för olika typer av autentisering som du kan använda.</span><span class="sxs-lookup"><span data-stu-id="1da21-177">See [Web linked service](#linked-service-properties) section for different types of authentication you can use.</span></span>

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

<span data-ttu-id="1da21-178">**Länkad Azure Storage-tjänst**</span><span class="sxs-lookup"><span data-stu-id="1da21-178">**Azure Storage linked service**</span></span>

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

<span data-ttu-id="1da21-179">**WebTable inkommande dataset** inställningen **externa** till **SANT** informerar Data Factory-tjänsten att datamängden är extern till data factory och inte tillverkas av en aktivitet i data fabriken.</span><span class="sxs-lookup"><span data-stu-id="1da21-179">**WebTable input dataset** Setting **external** to **true** informs the Data Factory service that the dataset is external to the data factory and is not produced by an activity in the data factory.</span></span>

> [!NOTE]
> <span data-ttu-id="1da21-180">Se [Get-index för en tabell i en HTML-sida](#get-index-of-a-table-in-an-html-page) avsnittet steg för att få index för en tabell i en HTML-sida.</span><span class="sxs-lookup"><span data-stu-id="1da21-180">See [Get index of a table in an HTML page](#get-index-of-a-table-in-an-html-page) section for steps to getting index of a table in an HTML page.</span></span>  
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


<span data-ttu-id="1da21-181">**Azure Blob utdatauppsättningen**</span><span class="sxs-lookup"><span data-stu-id="1da21-181">**Azure Blob output dataset**</span></span>

<span data-ttu-id="1da21-182">Data skrivs till en ny blob varje timme (frekvens: timme, intervall: 1).</span><span class="sxs-lookup"><span data-stu-id="1da21-182">Data is written to a new blob every hour (frequency: hour, interval: 1).</span></span>

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



<span data-ttu-id="1da21-183">**Pipeline med kopieringsaktiviteten**</span><span class="sxs-lookup"><span data-stu-id="1da21-183">**Pipeline with Copy activity**</span></span>

<span data-ttu-id="1da21-184">Pipelinen innehåller en kopia-aktivitet som är konfigurerad för att använda indata och utdata-datauppsättningar och är schemalagd att köras varje timme.</span><span class="sxs-lookup"><span data-stu-id="1da21-184">The pipeline contains a Copy Activity that is configured to use the input and output datasets and is scheduled to run every hour.</span></span> <span data-ttu-id="1da21-185">I pipeline-JSON-definitionen av **källa** är inställd på **WebSource** och **sink** är inställd på **BlobSink**.</span><span class="sxs-lookup"><span data-stu-id="1da21-185">In the pipeline JSON definition, the **source** type is set to **WebSource** and **sink** type is set to **BlobSink**.</span></span>

<span data-ttu-id="1da21-186">Se [WebSource Typegenskaper](#copy-activity-type-properties) lista över egenskaper som stöds av WebSource.</span><span class="sxs-lookup"><span data-stu-id="1da21-186">See [WebSource type properties](#copy-activity-type-properties) for the list of properties supported by the WebSource.</span></span>

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
        "description": "Copy from a Web table to an Azure blob",
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

## <a name="get-index-of-a-table-in-an-html-page"></a><span data-ttu-id="1da21-187">Hämta index för en tabell i en HTML-sida</span><span class="sxs-lookup"><span data-stu-id="1da21-187">Get index of a table in an HTML page</span></span>
1. <span data-ttu-id="1da21-188">Starta **Excel 2016** och växla till den **Data** fliken.</span><span class="sxs-lookup"><span data-stu-id="1da21-188">Launch **Excel 2016** and switch to the **Data** tab.</span></span>  
2. <span data-ttu-id="1da21-189">Klicka på **ny fråga** i verktygsfältet, pekar på **från andra källor** och på **från webben**.</span><span class="sxs-lookup"><span data-stu-id="1da21-189">Click **New Query** on the toolbar, point to **From Other Sources** and click **From Web**.</span></span>

    ![Power Query-menyn](./media/data-factory-web-table-connector/PowerQuery-Menu.png)
3. <span data-ttu-id="1da21-191">I den **från webben** dialogrutan Ange **URL** som du vill använda i länkad tjänst-JSON (till exempel: https://en.wikipedia.org/wiki/) tillsammans med sökvägen som du anger för datauppsättningen (till exempel: Afrika % 27s_ 100_Years... 100_Movies) och klicka på **OK**.</span><span class="sxs-lookup"><span data-stu-id="1da21-191">In the **From Web** dialog box, enter **URL** that you would use in linked service JSON (for example: https://en.wikipedia.org/wiki/) along with path you would specify for the dataset (for example: AFI%27s_100_Years...100_Movies), and click **OK**.</span></span>

    ![Från webben dialogrutan](./media/data-factory-web-table-connector/FromWeb-DialogBox.png)

    <span data-ttu-id="1da21-193">URL som används i det här exemplet: https://en.wikipedia.org/wiki/AFI%27s_100_Years...100_Movies</span><span class="sxs-lookup"><span data-stu-id="1da21-193">URL used in this example: https://en.wikipedia.org/wiki/AFI%27s_100_Years...100_Movies</span></span>
4. <span data-ttu-id="1da21-194">Om du ser **åtkomst till webbinnehåll** dialogrutan Välj höger **URL**, **autentisering**, och klicka på **Anslut**.</span><span class="sxs-lookup"><span data-stu-id="1da21-194">If you see **Access Web content** dialog box, select the right **URL**, **authentication**, and click **Connect**.</span></span>

   ![Åtkomst till innehåll dialogrutan för webbplats](./media/data-factory-web-table-connector/AccessWebContentDialog.png)
5. <span data-ttu-id="1da21-196">Klicka på en **tabell** objekt i trädvyn finns innehåll från tabellen och klicka sedan på **redigera** längst ned.</span><span class="sxs-lookup"><span data-stu-id="1da21-196">Click a **table** item in the tree view to see content from the table and then click **Edit** button at the bottom.</span></span>  

   ![Navigator dialogrutan](./media/data-factory-web-table-connector/Navigator-DialogBox.png)
6. <span data-ttu-id="1da21-198">I den **frågeredigeraren** -fönstret klickar du på **avancerade Editor** i verktygsfältet.</span><span class="sxs-lookup"><span data-stu-id="1da21-198">In the **Query Editor** window, click **Advanced Editor** button on the toolbar.</span></span>

    ![Knappen Avancerat redigeraren](./media/data-factory-web-table-connector/QueryEditor-AdvancedEditorButton.png)
7. <span data-ttu-id="1da21-200">I dialogrutan Avancerad redigerare är talet bredvid ”källa” indexet.</span><span class="sxs-lookup"><span data-stu-id="1da21-200">In the Advanced Editor dialog box, the number next to "Source" is the index.</span></span>

    ![Avancerad redigerare - Index](./media/data-factory-web-table-connector/AdvancedEditor-Index.png)

<span data-ttu-id="1da21-202">Om du använder Excel 2013 använder [Microsoft Power Query för Excel](https://www.microsoft.com/download/details.aspx?id=39379) att hämta index.</span><span class="sxs-lookup"><span data-stu-id="1da21-202">If you are using Excel 2013, use [Microsoft Power Query for Excel](https://www.microsoft.com/download/details.aspx?id=39379) to get the index.</span></span> <span data-ttu-id="1da21-203">Se [Anslut till en webbsida](https://support.office.com/article/Connect-to-a-web-page-Power-Query-b2725d67-c9e8-43e6-a590-c0a175bd64d8) artikeln för information.</span><span class="sxs-lookup"><span data-stu-id="1da21-203">See [Connect to a web page](https://support.office.com/article/Connect-to-a-web-page-Power-Query-b2725d67-c9e8-43e6-a590-c0a175bd64d8) article for details.</span></span> <span data-ttu-id="1da21-204">Stegen är ungefär som om du använder [Microsoft Power BI för skrivbordet](https://powerbi.microsoft.com/desktop/).</span><span class="sxs-lookup"><span data-stu-id="1da21-204">The steps are similar if you are using [Microsoft Power BI for Desktop](https://powerbi.microsoft.com/desktop/).</span></span>

> [!NOTE]
> <span data-ttu-id="1da21-205">Om du vill mappa kolumner från källan dataset till kolumner från sink dataset finns [mappa dataset kolumner i Azure Data Factory](data-factory-map-columns.md).</span><span class="sxs-lookup"><span data-stu-id="1da21-205">To map columns from source dataset to columns from sink dataset, see [Mapping dataset columns in Azure Data Factory](data-factory-map-columns.md).</span></span>

## <a name="performance-and-tuning"></a><span data-ttu-id="1da21-206">Prestanda och finjustering</span><span class="sxs-lookup"><span data-stu-id="1da21-206">Performance and Tuning</span></span>
<span data-ttu-id="1da21-207">Se [kopiera aktivitet prestanda och justera guiden](data-factory-copy-activity-performance.md) vill veta mer om viktiga faktorer som påverkan prestanda för flytt av data (Kopieringsaktiviteten) i Azure Data Factory och olika sätt att optimera den.</span><span class="sxs-lookup"><span data-stu-id="1da21-207">See [Copy Activity Performance & Tuning Guide](data-factory-copy-activity-performance.md) to learn about key factors that impact performance of data movement (Copy Activity) in Azure Data Factory and various ways to optimize it.</span></span>
