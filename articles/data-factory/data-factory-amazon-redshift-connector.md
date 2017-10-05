---
title: "Flytta data från Amazon Redshift med hjälp av Data Factory | Microsoft Docs"
description: "Läs mer om hur du flyttar data från Amazon Redshift med hjälp av Azure Data Factory."
services: data-factory
documentationcenter: 
author: linda33wj
manager: jhubbard
editor: monicar
ms.assetid: 01d15078-58dc-455c-9d9d-98fbdf4ea51e
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/27/2017
ms.author: jingwang
ms.openlocfilehash: bccb941363952bb2251629240a88148a6527d62e
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/03/2017
---
# <a name="move-data-from-amazon-redshift-using-azure-data-factory"></a><span data-ttu-id="46349-103">Flytta data från Amazon Redshift med hjälp av Azure Data Factory</span><span class="sxs-lookup"><span data-stu-id="46349-103">Move data From Amazon Redshift using Azure Data Factory</span></span>
<span data-ttu-id="46349-104">Den här artikeln förklarar hur du använder aktiviteten kopiera i Azure Data Factory för att flytta data från Amazon Redshift.</span><span class="sxs-lookup"><span data-stu-id="46349-104">This article explains how to use the Copy Activity in Azure Data Factory to move data from Amazon Redshift.</span></span> <span data-ttu-id="46349-105">Artikeln bygger på den [Data Movement aktiviteter](data-factory-data-movement-activities.md) artikel som presenterar en allmän översikt över dataflyttning med copy-aktivitet.</span><span class="sxs-lookup"><span data-stu-id="46349-105">The article builds on the [Data Movement Activities](data-factory-data-movement-activities.md) article, which presents a general overview of data movement with the copy activity.</span></span> 

<span data-ttu-id="46349-106">Du kan kopiera data från Amazon Redshift till alla stöds sink-datalagret.</span><span class="sxs-lookup"><span data-stu-id="46349-106">You can copy data from Amazon Redshift to any supported sink data store.</span></span> <span data-ttu-id="46349-107">En lista över datakällor som stöds som sänkor av kopieringsaktiviteten finns [stöds datalager](data-factory-data-movement-activities.md#supported-data-stores-and-formats).</span><span class="sxs-lookup"><span data-stu-id="46349-107">For a list of data stores supported as sinks by the copy activity, see [supported data stores](data-factory-data-movement-activities.md#supported-data-stores-and-formats).</span></span> <span data-ttu-id="46349-108">Data factory stöder för närvarande flytta data från Amazon Redshift till andra databaser, men inte för att flytta data från andra dataarkiv till Amazon Redshift.</span><span class="sxs-lookup"><span data-stu-id="46349-108">Data factory currently supports moving data from Amazon Redshift to other data stores, but not for moving data from other data stores to Amazon Redshift.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="46349-109">Krav</span><span class="sxs-lookup"><span data-stu-id="46349-109">Prerequisites</span></span>
* <span data-ttu-id="46349-110">Om du flyttar data till ett lokalt datalager, installera [Data Management Gateway](data-factory-data-management-gateway.md) på en lokal dator.</span><span class="sxs-lookup"><span data-stu-id="46349-110">If you are moving data to an on-premises data store, install [Data Management Gateway](data-factory-data-management-gateway.md) on an on-premises machine.</span></span> <span data-ttu-id="46349-111">Ge sedan Data Management Gateway (Använd IP-adress för datorn) åtkomst till Amazon Redshift klustret.</span><span class="sxs-lookup"><span data-stu-id="46349-111">Then, Grant Data Management Gateway (use IP address of the machine) the access to Amazon Redshift cluster.</span></span> <span data-ttu-id="46349-112">Se [auktorisera åtkomst till klustret](http://docs.aws.amazon.com/redshift/latest/gsg/rs-gsg-authorize-cluster-access.html) anvisningar.</span><span class="sxs-lookup"><span data-stu-id="46349-112">See [Authorize access to the cluster](http://docs.aws.amazon.com/redshift/latest/gsg/rs-gsg-authorize-cluster-access.html) for instructions.</span></span>
* <span data-ttu-id="46349-113">Om du flyttar data till ett Azure datalager, se [Azure Data Center IP-adressintervall](https://www.microsoft.com/download/details.aspx?id=41653) för Compute IP-adressen och SQL-adressintervall som används av Azure data datacenter.</span><span class="sxs-lookup"><span data-stu-id="46349-113">If you are moving data to an Azure data store, see [Azure Data Center IP Ranges](https://www.microsoft.com/download/details.aspx?id=41653) for the Compute IP address and SQL ranges used by the Azure data centers.</span></span>

## <a name="getting-started"></a><span data-ttu-id="46349-114">Komma igång</span><span class="sxs-lookup"><span data-stu-id="46349-114">Getting started</span></span>
<span data-ttu-id="46349-115">Du kan skapa en pipeline med en kopia-aktivitet som flyttar data från en källa för Amazon Redshift med hjälp av olika verktyg/API: er.</span><span class="sxs-lookup"><span data-stu-id="46349-115">You can create a pipeline with a copy activity that moves data from an Amazon Redshift source by using different tools/APIs.</span></span>

<span data-ttu-id="46349-116">Det enklaste sättet att skapa en pipeline är att använda den **guiden Kopiera**.</span><span class="sxs-lookup"><span data-stu-id="46349-116">The easiest way to create a pipeline is to use the **Copy Wizard**.</span></span> <span data-ttu-id="46349-117">Finns [Självstudier: skapa en pipeline med hjälp av guiden Kopiera](data-factory-copy-data-wizard-tutorial.md) för en snabb genomgång om hur du skapar en pipeline med hjälp av guiden Kopiera data.</span><span class="sxs-lookup"><span data-stu-id="46349-117">See [Tutorial: Create a pipeline using Copy Wizard](data-factory-copy-data-wizard-tutorial.md) for a quick walkthrough on creating a pipeline using the Copy data wizard.</span></span>

<span data-ttu-id="46349-118">Du kan också använda följande verktyg för att skapa en pipeline: **Azure-portalen**, **Visual Studio**, **Azure PowerShell**, **Azure Resource Manager-mall**, **.NET API**, och **REST API**.</span><span class="sxs-lookup"><span data-stu-id="46349-118">You can also use the following tools to create a pipeline: **Azure portal**, **Visual Studio**, **Azure PowerShell**, **Azure Resource Manager template**, **.NET API**, and **REST API**.</span></span> <span data-ttu-id="46349-119">Se [kopiera aktivitet kursen](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) för stegvisa instruktioner för att skapa en pipeline med en Kopieringsaktivitet.</span><span class="sxs-lookup"><span data-stu-id="46349-119">See [Copy activity tutorial](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) for step-by-step instructions to create a pipeline with a copy activity.</span></span> 

<span data-ttu-id="46349-120">Om du använder verktyg eller API: er, kan du utföra följande steg för att skapa en pipeline som flyttar data från ett dataarkiv som källa till ett dataarkiv som mottagare:</span><span class="sxs-lookup"><span data-stu-id="46349-120">Whether you use the tools or APIs, you perform the following steps to create a pipeline that moves data from a source data store to a sink data store:</span></span> 

1. <span data-ttu-id="46349-121">Skapa **länkade tjänster** att länka inkommande och utgående data lagras till din data factory.</span><span class="sxs-lookup"><span data-stu-id="46349-121">Create **linked services** to link input and output data stores to your data factory.</span></span>
2. <span data-ttu-id="46349-122">Skapa **datauppsättningar** att representera inkommande och utgående data för kopieringen.</span><span class="sxs-lookup"><span data-stu-id="46349-122">Create **datasets** to represent input and output data for the copy operation.</span></span> 
3. <span data-ttu-id="46349-123">Skapa en **pipeline** med en kopia-aktivitet som tar en datamängd som indata och en dataset som utdata.</span><span class="sxs-lookup"><span data-stu-id="46349-123">Create a **pipeline** with a copy activity that takes a dataset as an input and a dataset as an output.</span></span> 

<span data-ttu-id="46349-124">När du använder guiden skapas JSON definitioner för dessa Data Factory-enheter (länkade tjänster, datauppsättningar och pipelinen) automatiskt för dig.</span><span class="sxs-lookup"><span data-stu-id="46349-124">When you use the wizard, JSON definitions for these Data Factory entities (linked services, datasets, and the pipeline) are automatically created for you.</span></span> <span data-ttu-id="46349-125">När du använder Verktyg/API: er (utom .NET API), kan du definiera dessa Data Factory-enheter med hjälp av JSON-format.</span><span class="sxs-lookup"><span data-stu-id="46349-125">When you use tools/APIs (except .NET API), you define these Data Factory entities by using the JSON format.</span></span>  <span data-ttu-id="46349-126">Ett exempel med JSON-definitioner för Data Factory-entiteter som används för att kopiera data från ett dataarkiv för Amazon Redshift finns [JSON-exempel: kopiera data från Amazon Redshift till Azure Blob](#json-example-copy-data-from-amazon-redshift-to-azure-blob) i den här artikeln.</span><span class="sxs-lookup"><span data-stu-id="46349-126">For a sample with JSON definitions for Data Factory entities that are used to copy data from an Amazon Redshift data store, see [JSON example: Copy data from Amazon Redshift to Azure Blob](#json-example-copy-data-from-amazon-redshift-to-azure-blob) section of this article.</span></span> 

<span data-ttu-id="46349-127">Följande avsnitt innehåller information om JSON-egenskaper som används för att definiera Data Factory entiteter till Amazon Redshift:</span><span class="sxs-lookup"><span data-stu-id="46349-127">The following sections provide details about JSON properties that are used to define Data Factory entities specific to Amazon Redshift:</span></span> 

## <a name="linked-service-properties"></a><span data-ttu-id="46349-128">Länkad tjänstegenskaper</span><span class="sxs-lookup"><span data-stu-id="46349-128">Linked service properties</span></span>
<span data-ttu-id="46349-129">Följande tabell innehåller en beskrivning för JSON-element som är specifika för Amazon Redshift länkad tjänst.</span><span class="sxs-lookup"><span data-stu-id="46349-129">The following table provides description for JSON elements specific to Amazon Redshift linked service.</span></span>

| <span data-ttu-id="46349-130">Egenskap</span><span class="sxs-lookup"><span data-stu-id="46349-130">Property</span></span> | <span data-ttu-id="46349-131">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="46349-131">Description</span></span> | <span data-ttu-id="46349-132">Krävs</span><span class="sxs-lookup"><span data-stu-id="46349-132">Required</span></span> |
| --- | --- | --- |
| <span data-ttu-id="46349-133">typ</span><span class="sxs-lookup"><span data-stu-id="46349-133">type</span></span> |<span data-ttu-id="46349-134">Egenskapen type måste anges till: **AmazonRedshift**.</span><span class="sxs-lookup"><span data-stu-id="46349-134">The type property must be set to: **AmazonRedshift**.</span></span> |<span data-ttu-id="46349-135">Ja</span><span class="sxs-lookup"><span data-stu-id="46349-135">Yes</span></span> |
| <span data-ttu-id="46349-136">server</span><span class="sxs-lookup"><span data-stu-id="46349-136">server</span></span> |<span data-ttu-id="46349-137">IP-adressen eller värdnamnet namnet på Amazon Redshift-servern.</span><span class="sxs-lookup"><span data-stu-id="46349-137">IP address or host name of the Amazon Redshift server.</span></span> |<span data-ttu-id="46349-138">Ja</span><span class="sxs-lookup"><span data-stu-id="46349-138">Yes</span></span> |
| <span data-ttu-id="46349-139">port</span><span class="sxs-lookup"><span data-stu-id="46349-139">port</span></span> |<span data-ttu-id="46349-140">Antalet TCP-porten som Amazon Redshift-servern använder för att lyssna efter anslutningar.</span><span class="sxs-lookup"><span data-stu-id="46349-140">The number of the TCP port that the Amazon Redshift server uses to listen for client connections.</span></span> |<span data-ttu-id="46349-141">Nej, standardvärde: 5439</span><span class="sxs-lookup"><span data-stu-id="46349-141">No, default value: 5439</span></span> |
| <span data-ttu-id="46349-142">Databasen</span><span class="sxs-lookup"><span data-stu-id="46349-142">database</span></span> |<span data-ttu-id="46349-143">Namnet på Amazon Redshift-databasen.</span><span class="sxs-lookup"><span data-stu-id="46349-143">Name of the Amazon Redshift database.</span></span> |<span data-ttu-id="46349-144">Ja</span><span class="sxs-lookup"><span data-stu-id="46349-144">Yes</span></span> |
| <span data-ttu-id="46349-145">användarnamn</span><span class="sxs-lookup"><span data-stu-id="46349-145">username</span></span> |<span data-ttu-id="46349-146">Namnet på användaren som har åtkomst till databasen.</span><span class="sxs-lookup"><span data-stu-id="46349-146">Name of user who has access to the database.</span></span> |<span data-ttu-id="46349-147">Ja</span><span class="sxs-lookup"><span data-stu-id="46349-147">Yes</span></span> |
| <span data-ttu-id="46349-148">lösenord</span><span class="sxs-lookup"><span data-stu-id="46349-148">password</span></span> |<span data-ttu-id="46349-149">Lösenordet för användarkontot.</span><span class="sxs-lookup"><span data-stu-id="46349-149">Password for the user account.</span></span> |<span data-ttu-id="46349-150">Ja</span><span class="sxs-lookup"><span data-stu-id="46349-150">Yes</span></span> |

## <a name="dataset-properties"></a><span data-ttu-id="46349-151">Egenskaper för datamängd</span><span class="sxs-lookup"><span data-stu-id="46349-151">Dataset properties</span></span>
<span data-ttu-id="46349-152">En fullständig lista över egenskaper som är tillgängliga för att definiera datauppsättningarna & avsnitt finns i [skapa datauppsättningar](data-factory-create-datasets.md) artikel.</span><span class="sxs-lookup"><span data-stu-id="46349-152">For a full list of sections & properties available for defining datasets, see the [Creating datasets](data-factory-create-datasets.md) article.</span></span> <span data-ttu-id="46349-153">Avsnitt som struktur, tillgänglighet och principen är liknande för alla typer av dataset (Azure SQL Azure blob, Azure-tabellen, osv.).</span><span class="sxs-lookup"><span data-stu-id="46349-153">Sections such as structure, availability, and policy are similar for all dataset types (Azure SQL, Azure blob, Azure table, etc.).</span></span>

<span data-ttu-id="46349-154">Den **typeProperties** avsnittet är olika för varje typ av datauppsättningen.</span><span class="sxs-lookup"><span data-stu-id="46349-154">The **typeProperties** section is different for each type of dataset.</span></span> <span data-ttu-id="46349-155">Den ger information om placeringen av data i datalagret.</span><span class="sxs-lookup"><span data-stu-id="46349-155">It provides information about the location of the data in the data store.</span></span> <span data-ttu-id="46349-156">TypeProperties avsnittet för dataset av typen **RelationalTable** (som omfattar Amazon Redshift dataset) har följande egenskaper</span><span class="sxs-lookup"><span data-stu-id="46349-156">The typeProperties section for dataset of type **RelationalTable** (which includes Amazon Redshift dataset) has the following properties</span></span>

| <span data-ttu-id="46349-157">Egenskap</span><span class="sxs-lookup"><span data-stu-id="46349-157">Property</span></span> | <span data-ttu-id="46349-158">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="46349-158">Description</span></span> | <span data-ttu-id="46349-159">Krävs</span><span class="sxs-lookup"><span data-stu-id="46349-159">Required</span></span> |
| --- | --- | --- |
| <span data-ttu-id="46349-160">tableName</span><span class="sxs-lookup"><span data-stu-id="46349-160">tableName</span></span> |<span data-ttu-id="46349-161">Namnet på tabellen i databasen Amazon Redshift som den länkade tjänsten refererar till.</span><span class="sxs-lookup"><span data-stu-id="46349-161">Name of the table in the Amazon Redshift database that linked service refers to.</span></span> |<span data-ttu-id="46349-162">Nej (om **frågan** av **RelationalSource** har angetts)</span><span class="sxs-lookup"><span data-stu-id="46349-162">No (if **query** of **RelationalSource** is specified)</span></span> |

## <a name="copy-activity-properties"></a><span data-ttu-id="46349-163">Kopiera egenskaper för aktivitet</span><span class="sxs-lookup"><span data-stu-id="46349-163">Copy activity properties</span></span>
<span data-ttu-id="46349-164">En fullständig lista över avsnitt & egenskaper som är tillgängliga för att definiera aktiviteter finns i [skapar Pipelines](data-factory-create-pipelines.md) artikel.</span><span class="sxs-lookup"><span data-stu-id="46349-164">For a full list of sections & properties available for defining activities, see the [Creating Pipelines](data-factory-create-pipelines.md) article.</span></span> <span data-ttu-id="46349-165">Egenskaper som namn, beskrivning, ingående och utgående tabeller och principer är tillgängliga för alla typer av aktiviteter.</span><span class="sxs-lookup"><span data-stu-id="46349-165">Properties such as name, description, input and output tables, and policies are available for all types of activities.</span></span>

<span data-ttu-id="46349-166">Medan egenskaper som är tillgängliga i den **typeProperties** avsnitt i aktiviteten varierar med varje aktivitetstyp.</span><span class="sxs-lookup"><span data-stu-id="46349-166">Whereas, properties available in the **typeProperties** section of the activity vary with each activity type.</span></span> <span data-ttu-id="46349-167">För Kopieringsaktivitet kan variera de beroende på vilka typer av datakällor och sänkor.</span><span class="sxs-lookup"><span data-stu-id="46349-167">For Copy activity, they vary depending on the types of sources and sinks.</span></span>

<span data-ttu-id="46349-168">Om källan för kopieringsaktiviteten är av typen **RelationalSource** (som omfattar Amazon Redshift), följande egenskaper finns i avsnittet typeProperties:</span><span class="sxs-lookup"><span data-stu-id="46349-168">When source of copy activity is of type **RelationalSource** (which includes Amazon Redshift), the following properties are available in typeProperties section:</span></span>

| <span data-ttu-id="46349-169">Egenskap</span><span class="sxs-lookup"><span data-stu-id="46349-169">Property</span></span> | <span data-ttu-id="46349-170">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="46349-170">Description</span></span> | <span data-ttu-id="46349-171">Tillåtna värden</span><span class="sxs-lookup"><span data-stu-id="46349-171">Allowed values</span></span> | <span data-ttu-id="46349-172">Krävs</span><span class="sxs-lookup"><span data-stu-id="46349-172">Required</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="46349-173">DocumentDB</span><span class="sxs-lookup"><span data-stu-id="46349-173">query</span></span> |<span data-ttu-id="46349-174">Använd anpassad fråga för att läsa data.</span><span class="sxs-lookup"><span data-stu-id="46349-174">Use the custom query to read data.</span></span> |<span data-ttu-id="46349-175">SQL-sträng.</span><span class="sxs-lookup"><span data-stu-id="46349-175">SQL query string.</span></span> <span data-ttu-id="46349-176">Till exempel: Välj * från mytable prefix.</span><span class="sxs-lookup"><span data-stu-id="46349-176">For example: select * from MyTable.</span></span> |<span data-ttu-id="46349-177">Nej (om **tableName** av **dataset** har angetts)</span><span class="sxs-lookup"><span data-stu-id="46349-177">No (if **tableName** of **dataset** is specified)</span></span> |

## <a name="json-example-copy-data-from-amazon-redshift-to-azure-blob"></a><span data-ttu-id="46349-178">JSON-exempel: kopiera data från Amazon Redshift till Azure-Blob</span><span class="sxs-lookup"><span data-stu-id="46349-178">JSON example: Copy data from Amazon Redshift to Azure Blob</span></span>
<span data-ttu-id="46349-179">Det här exemplet visas hur du kopierar data från en databas med Amazon Redshift till ett Azure Blob Storage.</span><span class="sxs-lookup"><span data-stu-id="46349-179">This sample shows how to copy data from an Amazon Redshift database to an Azure Blob Storage.</span></span> <span data-ttu-id="46349-180">Dock datan kan kopieras **direkt** till någon av sänkor anges [här](data-factory-data-movement-activities.md#supported-data-stores-and-formats) med hjälp av aktiviteten kopiera i Azure Data Factory.</span><span class="sxs-lookup"><span data-stu-id="46349-180">However, data can be copied **directly** to any of the sinks stated [here](data-factory-data-movement-activities.md#supported-data-stores-and-formats) using the Copy Activity in Azure Data Factory.</span></span>  

<span data-ttu-id="46349-181">Exemplet har följande data factory enheter:</span><span class="sxs-lookup"><span data-stu-id="46349-181">The sample has the following data factory entities:</span></span>

* <span data-ttu-id="46349-182">En länkad tjänst av typen [AmazonRedshift](#linked-service-properties).</span><span class="sxs-lookup"><span data-stu-id="46349-182">A linked service of type [AmazonRedshift](#linked-service-properties).</span></span>
* <span data-ttu-id="46349-183">En länkad tjänst av typen [AzureStorage](data-factory-azure-blob-connector.md#linked-service-properties).</span><span class="sxs-lookup"><span data-stu-id="46349-183">A linked service of type [AzureStorage](data-factory-azure-blob-connector.md#linked-service-properties).</span></span>
* <span data-ttu-id="46349-184">Indata [dataset](data-factory-create-datasets.md) av typen [RelationalTable](#dataset-properties).</span><span class="sxs-lookup"><span data-stu-id="46349-184">An input [dataset](data-factory-create-datasets.md) of type [RelationalTable](#dataset-properties).</span></span>
* <span data-ttu-id="46349-185">Utdata [dataset](data-factory-create-datasets.md) av typen [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties).</span><span class="sxs-lookup"><span data-stu-id="46349-185">An output [dataset](data-factory-create-datasets.md) of type [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties).</span></span>
* <span data-ttu-id="46349-186">En [pipeline](data-factory-create-pipelines.md) med Kopieringsaktiviteten som använder [RelationalSource](#copy-activity-properties) och [BlobSink](data-factory-azure-blob-connector.md##copy-activity-properties).</span><span class="sxs-lookup"><span data-stu-id="46349-186">A [pipeline](data-factory-create-pipelines.md) with Copy Activity that uses [RelationalSource](#copy-activity-properties) and [BlobSink](data-factory-azure-blob-connector.md##copy-activity-properties).</span></span>

<span data-ttu-id="46349-187">Exemplet kopierar data från ett frågeresultat i Amazon Redshift till en blobb varje timme.</span><span class="sxs-lookup"><span data-stu-id="46349-187">The sample copies data from a query result in Amazon Redshift to a blob every hour.</span></span> <span data-ttu-id="46349-188">JSON-egenskaper som används i exemplen beskrivs i exemplen i följande avsnitt.</span><span class="sxs-lookup"><span data-stu-id="46349-188">The JSON properties used in these samples are described in sections following the samples.</span></span>

<span data-ttu-id="46349-189">**Amazon Redshift länkade tjänsten:**</span><span class="sxs-lookup"><span data-stu-id="46349-189">**Amazon Redshift linked service:**</span></span>

```json
{
    "name": "AmazonRedshiftLinkedService",
    "properties":
    {
        "type": "AmazonRedshift",
        "typeProperties":
        {
            "server": "< The IP address or host name of the Amazon Redshift server >",
            "port": <The number of the TCP port that the Amazon Redshift server uses to listen for client connections.>,
            "database": "<The database name of the Amazon Redshift database>",
            "username": "<username>",
            "password": "<password>"
        }
    }
}
```

<span data-ttu-id="46349-190">**Azure Storage länkade tjänsten:**</span><span class="sxs-lookup"><span data-stu-id="46349-190">**Azure Storage linked service:**</span></span>

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
<span data-ttu-id="46349-191">**Amazon Redshift inkommande datauppsättningen:**</span><span class="sxs-lookup"><span data-stu-id="46349-191">**Amazon Redshift input dataset:**</span></span>

<span data-ttu-id="46349-192">Ange `"external": true` informerar Data Factory-tjänsten att datamängden är extern till data factory och inte tillverkas av en aktivitet i datafabriken.</span><span class="sxs-lookup"><span data-stu-id="46349-192">Setting `"external": true` informs the Data Factory service that the dataset is external to the data factory and is not produced by an activity in the data factory.</span></span> <span data-ttu-id="46349-193">Ange den här egenskapen till true för en inkommande datauppsättningen som inte tillverkas av en aktivitet i pipelinen.</span><span class="sxs-lookup"><span data-stu-id="46349-193">Set this property to true on an input dataset that is not produced by an activity in the pipeline.</span></span>

```json
{
    "name": "AmazonRedshiftInputDataset",
    "properties": {
        "type": "RelationalTable",
        "linkedServiceName": "AmazonRedshiftLinkedService",
        "typeProperties": {
            "tableName": "<Table name>"
        },
        "availability": {
            "frequency": "Hour",
            "interval": 1
        },
        "external": true
    }
}
```

<span data-ttu-id="46349-194">**Azure Blob utdatauppsättningen:**</span><span class="sxs-lookup"><span data-stu-id="46349-194">**Azure Blob output dataset:**</span></span>

<span data-ttu-id="46349-195">Data skrivs till en ny blob varje timme (frekvens: timme, intervall: 1).</span><span class="sxs-lookup"><span data-stu-id="46349-195">Data is written to a new blob every hour (frequency: hour, interval: 1).</span></span> <span data-ttu-id="46349-196">Sökvägen till mappen för blobben utvärderas dynamiskt baserat på starttiden för den sektor som bearbetas.</span><span class="sxs-lookup"><span data-stu-id="46349-196">The folder path for the blob is dynamically evaluated based on the start time of the slice that is being processed.</span></span> <span data-ttu-id="46349-197">Mappsökvägen använder år, månad, dag och timmar delar av starttiden.</span><span class="sxs-lookup"><span data-stu-id="46349-197">The folder path uses year, month, day, and hours parts of the start time.</span></span>

```json
{
    "name": "AzureBlobOutputDataSet",
    "properties": {
        "type": "AzureBlob",
        "linkedServiceName": "AzureStorageLinkedService",
        "typeProperties": {
            "folderPath": "mycontainer/fromamazonredshift/yearno={Year}/monthno={Month}/dayno={Day}/hourno={Hour}",
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

<span data-ttu-id="46349-198">**Kopiera aktivitet i en pipeline med Azure Redshift källa (RelationalSource) och Blob mottagare:**</span><span class="sxs-lookup"><span data-stu-id="46349-198">**Copy activity in a pipeline with Azure Redshift source (RelationalSource) and Blob sink:**</span></span>

<span data-ttu-id="46349-199">Pipelinen innehåller en kopia-aktivitet som är konfigurerad för att använda indata och utdata-datauppsättningar och är schemalagd att köras varje timme.</span><span class="sxs-lookup"><span data-stu-id="46349-199">The pipeline contains a Copy Activity that is configured to use the input and output datasets and is scheduled to run every hour.</span></span> <span data-ttu-id="46349-200">I pipeline-JSON-definitionen av **källa** är inställd på **RelationalSource** och **sink** är inställd på **BlobSink**.</span><span class="sxs-lookup"><span data-stu-id="46349-200">In the pipeline JSON definition, the **source** type is set to **RelationalSource** and **sink** type is set to **BlobSink**.</span></span> <span data-ttu-id="46349-201">SQL-frågan som angetts för den **frågan** egenskapen väljer vilka data under den senaste timmen att kopiera.</span><span class="sxs-lookup"><span data-stu-id="46349-201">The SQL query specified for the **query** property selects the data in the past hour to copy.</span></span>

```json
{
    "name": "CopyAmazonRedshiftToBlob",
    "properties": {
        "description": "pipeline for copy activity",
        "activities": [
            {
                "type": "Copy",
                "typeProperties": {
                    "source": {
                        "type": "RelationalSource",
                        "query": "$$Text.Format('select * from MyTable where timestamp >= \\'{0:yyyy-MM-ddTHH:mm:ss}\\' AND timestamp < \\'{1:yyyy-MM-ddTHH:mm:ss}\\'', WindowStart, WindowEnd)"
                    },
                    "sink": {
                        "type": "BlobSink",
                        "writeBatchSize": 0,
                        "writeBatchTimeout": "00:00:00"
                    }
                },
                "inputs": [
                    {
                        "name": "AmazonRedshiftInputDataset"
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
                "name": "AmazonRedshiftToBlob"
            }
        ],
        "start": "2014-06-01T18:00:00Z",
        "end": "2014-06-01T19:00:00Z"
    }
}
```
### <a name="type-mapping-for-amazon-redshift"></a><span data-ttu-id="46349-202">Mappning för Amazon Redshift</span><span class="sxs-lookup"><span data-stu-id="46349-202">Type mapping for Amazon Redshift</span></span>
<span data-ttu-id="46349-203">Som anges i den [data movement aktiviteter](data-factory-data-movement-activities.md) artikeln kopieringsaktiviteten utför automatisk konverteringar från källtyper att registrera typer med följande metod i två steg:</span><span class="sxs-lookup"><span data-stu-id="46349-203">As mentioned in the [data movement activities](data-factory-data-movement-activities.md) article, Copy activity performs automatic type conversions from source types to sink types with the following two-step approach:</span></span>

1. <span data-ttu-id="46349-204">Konvertera från interna källtyper till .NET-typ</span><span class="sxs-lookup"><span data-stu-id="46349-204">Convert from native source types to .NET type</span></span>
2. <span data-ttu-id="46349-205">Konvertera från .NET-typ till interna mottagare typ.</span><span class="sxs-lookup"><span data-stu-id="46349-205">Convert from .NET type to native sink type</span></span>

<span data-ttu-id="46349-206">När du flyttar data Amazon Redshift, används följande mappningar från Amazon Redshift typer för .NET-typer.</span><span class="sxs-lookup"><span data-stu-id="46349-206">When moving data to Amazon Redshift, the following mappings are used from Amazon Redshift types to .NET types.</span></span>

| <span data-ttu-id="46349-207">Amazon Redshift typ</span><span class="sxs-lookup"><span data-stu-id="46349-207">Amazon Redshift Type</span></span> | <span data-ttu-id="46349-208">.NET baserat typ</span><span class="sxs-lookup"><span data-stu-id="46349-208">.NET Based Type</span></span> |
| --- | --- |
| <span data-ttu-id="46349-209">SMALLINT</span><span class="sxs-lookup"><span data-stu-id="46349-209">SMALLINT</span></span> |<span data-ttu-id="46349-210">Int16</span><span class="sxs-lookup"><span data-stu-id="46349-210">Int16</span></span> |
| <span data-ttu-id="46349-211">HELTAL</span><span class="sxs-lookup"><span data-stu-id="46349-211">INTEGER</span></span> |<span data-ttu-id="46349-212">Int32</span><span class="sxs-lookup"><span data-stu-id="46349-212">Int32</span></span> |
| <span data-ttu-id="46349-213">BIGINT</span><span class="sxs-lookup"><span data-stu-id="46349-213">BIGINT</span></span> |<span data-ttu-id="46349-214">Int64</span><span class="sxs-lookup"><span data-stu-id="46349-214">Int64</span></span> |
| <span data-ttu-id="46349-215">DECIMAL</span><span class="sxs-lookup"><span data-stu-id="46349-215">DECIMAL</span></span> |<span data-ttu-id="46349-216">Decimal</span><span class="sxs-lookup"><span data-stu-id="46349-216">Decimal</span></span> |
| <span data-ttu-id="46349-217">VERKLIG</span><span class="sxs-lookup"><span data-stu-id="46349-217">REAL</span></span> |<span data-ttu-id="46349-218">Enskild</span><span class="sxs-lookup"><span data-stu-id="46349-218">Single</span></span> |
| <span data-ttu-id="46349-219">DUBBEL PRECISION</span><span class="sxs-lookup"><span data-stu-id="46349-219">DOUBLE PRECISION</span></span> |<span data-ttu-id="46349-220">dubbla</span><span class="sxs-lookup"><span data-stu-id="46349-220">Double</span></span> |
| <span data-ttu-id="46349-221">BOOLESKT VÄRDE</span><span class="sxs-lookup"><span data-stu-id="46349-221">BOOLEAN</span></span> |<span data-ttu-id="46349-222">Sträng</span><span class="sxs-lookup"><span data-stu-id="46349-222">String</span></span> |
| <span data-ttu-id="46349-223">CHAR</span><span class="sxs-lookup"><span data-stu-id="46349-223">CHAR</span></span> |<span data-ttu-id="46349-224">Sträng</span><span class="sxs-lookup"><span data-stu-id="46349-224">String</span></span> |
| <span data-ttu-id="46349-225">VARCHAR</span><span class="sxs-lookup"><span data-stu-id="46349-225">VARCHAR</span></span> |<span data-ttu-id="46349-226">Sträng</span><span class="sxs-lookup"><span data-stu-id="46349-226">String</span></span> |
| <span data-ttu-id="46349-227">DATUM</span><span class="sxs-lookup"><span data-stu-id="46349-227">DATE</span></span> |<span data-ttu-id="46349-228">Datum och tid</span><span class="sxs-lookup"><span data-stu-id="46349-228">DateTime</span></span> |
| <span data-ttu-id="46349-229">TIDSSTÄMPEL</span><span class="sxs-lookup"><span data-stu-id="46349-229">TIMESTAMP</span></span> |<span data-ttu-id="46349-230">Datum och tid</span><span class="sxs-lookup"><span data-stu-id="46349-230">DateTime</span></span> |
| <span data-ttu-id="46349-231">TEXT</span><span class="sxs-lookup"><span data-stu-id="46349-231">TEXT</span></span> |<span data-ttu-id="46349-232">Sträng</span><span class="sxs-lookup"><span data-stu-id="46349-232">String</span></span> |

## <a name="map-source-to-sink-columns"></a><span data-ttu-id="46349-233">Karta källan till mottagare för kolumner</span><span class="sxs-lookup"><span data-stu-id="46349-233">Map source to sink columns</span></span>
<span data-ttu-id="46349-234">Mer information om mappning kolumner i datauppsättningen källan till kolumner i datauppsättning mottagare, se [mappa dataset kolumner i Azure Data Factory](data-factory-map-columns.md).</span><span class="sxs-lookup"><span data-stu-id="46349-234">To learn about mapping columns in source dataset to columns in sink dataset, see [Mapping dataset columns in Azure Data Factory](data-factory-map-columns.md).</span></span>

## <a name="repeatable-read-from-relational-sources"></a><span data-ttu-id="46349-235">Upprepbar läsning från relationella källor</span><span class="sxs-lookup"><span data-stu-id="46349-235">Repeatable read from relational sources</span></span>
<span data-ttu-id="46349-236">Tänk på att undvika oväntade resultat repeterbarhet när kopiering av data från relationella data lagras.</span><span class="sxs-lookup"><span data-stu-id="46349-236">When copying data from relational data stores, keep repeatability in mind to avoid unintended outcomes.</span></span> <span data-ttu-id="46349-237">I Azure Data Factory, kan du köra en sektor manuellt.</span><span class="sxs-lookup"><span data-stu-id="46349-237">In Azure Data Factory, you can rerun a slice manually.</span></span> <span data-ttu-id="46349-238">Du kan också konfigurera i principen för en dataset så att ett segment som körs när ett fel uppstår.</span><span class="sxs-lookup"><span data-stu-id="46349-238">You can also configure retry policy for a dataset so that a slice is rerun when a failure occurs.</span></span> <span data-ttu-id="46349-239">När ett segment körs på något sätt, måste du kontrollera att samma data läses oavsett hur många gånger ett segment körs.</span><span class="sxs-lookup"><span data-stu-id="46349-239">When a slice is rerun in either way, you need to make sure that the same data is read no matter how many times a slice is run.</span></span> <span data-ttu-id="46349-240">Se [Repeatable läsa från relationella källor](data-factory-repeatable-copy.md#repeatable-read-from-relational-sources)</span><span class="sxs-lookup"><span data-stu-id="46349-240">See [Repeatable read from relational sources](data-factory-repeatable-copy.md#repeatable-read-from-relational-sources)</span></span>

## <a name="performance-and-tuning"></a><span data-ttu-id="46349-241">Prestanda och finjustering</span><span class="sxs-lookup"><span data-stu-id="46349-241">Performance and Tuning</span></span>
<span data-ttu-id="46349-242">Se [kopiera aktivitet prestanda och justera guiden](data-factory-copy-activity-performance.md) vill veta mer om viktiga faktorer som påverkan prestanda för flytt av data (Kopieringsaktiviteten) i Azure Data Factory och olika sätt att optimera den.</span><span class="sxs-lookup"><span data-stu-id="46349-242">See [Copy Activity Performance & Tuning Guide](data-factory-copy-activity-performance.md) to learn about key factors that impact performance of data movement (Copy Activity) in Azure Data Factory and various ways to optimize it.</span></span>

## <a name="next-steps"></a><span data-ttu-id="46349-243">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="46349-243">Next Steps</span></span>
<span data-ttu-id="46349-244">Se följande artiklar:</span><span class="sxs-lookup"><span data-stu-id="46349-244">See the following articles:</span></span>

* <span data-ttu-id="46349-245">[Kopiera aktivitet kursen](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) stegvisa instruktioner för att skapa en pipeline med en kopia-aktivitet.</span><span class="sxs-lookup"><span data-stu-id="46349-245">[Copy Activity tutorial](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) for step-by-step instructions for creating a pipeline with a Copy Activity.</span></span>
