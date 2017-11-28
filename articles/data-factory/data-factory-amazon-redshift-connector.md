---
title: "aaaMove data från Amazon Redshift med hjälp av Data Factory | Microsoft Docs"
description: "Mer information om hur toomove data från Amazon Redshift med hjälp av Azure Data Factory."
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
ms.openlocfilehash: 2a097320734ebdd57282d250f7fdba35741777f5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="move-data-from-amazon-redshift-using-azure-data-factory"></a><span data-ttu-id="e1cc2-103">Flytta data från Amazon Redshift med hjälp av Azure Data Factory</span><span class="sxs-lookup"><span data-stu-id="e1cc2-103">Move data From Amazon Redshift using Azure Data Factory</span></span>
<span data-ttu-id="e1cc2-104">Den här artikeln förklarar hur toouse hello Kopieringsaktiviteten i Azure Data Factory toomove data från Amazon Redshift.</span><span class="sxs-lookup"><span data-stu-id="e1cc2-104">This article explains how toouse hello Copy Activity in Azure Data Factory toomove data from Amazon Redshift.</span></span> <span data-ttu-id="e1cc2-105">hello artikel bygger på hello [Data Movement aktiviteter](data-factory-data-movement-activities.md) artikel som ger en allmän översikt över dataflyttning hello kopieringsaktiviteten.</span><span class="sxs-lookup"><span data-stu-id="e1cc2-105">hello article builds on hello [Data Movement Activities](data-factory-data-movement-activities.md) article, which presents a general overview of data movement with hello copy activity.</span></span> 

<span data-ttu-id="e1cc2-106">Du kan kopiera data från Amazon Redshift tooany stöds sink-datalagret.</span><span class="sxs-lookup"><span data-stu-id="e1cc2-106">You can copy data from Amazon Redshift tooany supported sink data store.</span></span> <span data-ttu-id="e1cc2-107">En lista över datakällor som stöds som sänkor av hello kopieringsaktiviteten finns [stöds datalager](data-factory-data-movement-activities.md#supported-data-stores-and-formats).</span><span class="sxs-lookup"><span data-stu-id="e1cc2-107">For a list of data stores supported as sinks by hello copy activity, see [supported data stores](data-factory-data-movement-activities.md#supported-data-stores-and-formats).</span></span> <span data-ttu-id="e1cc2-108">Data factory stöder för närvarande flytta data från Amazon Redshift tooother datalager, men inte för att flytta data från andra data lagras tooAmazon Redshift.</span><span class="sxs-lookup"><span data-stu-id="e1cc2-108">Data factory currently supports moving data from Amazon Redshift tooother data stores, but not for moving data from other data stores tooAmazon Redshift.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="e1cc2-109">Krav</span><span class="sxs-lookup"><span data-stu-id="e1cc2-109">Prerequisites</span></span>
* <span data-ttu-id="e1cc2-110">Om du flyttar data tooan lokala datalagret, installera [Data Management Gateway](data-factory-data-management-gateway.md) på en lokal dator.</span><span class="sxs-lookup"><span data-stu-id="e1cc2-110">If you are moving data tooan on-premises data store, install [Data Management Gateway](data-factory-data-management-gateway.md) on an on-premises machine.</span></span> <span data-ttu-id="e1cc2-111">Sedan bevilja Data Management Gateway (Använd IP-adressen för datorn hello) hello åtkomst tooAmazon Redshift kluster.</span><span class="sxs-lookup"><span data-stu-id="e1cc2-111">Then, Grant Data Management Gateway (use IP address of hello machine) hello access tooAmazon Redshift cluster.</span></span> <span data-ttu-id="e1cc2-112">Se [auktorisera åtkomst toohello klustret](http://docs.aws.amazon.com/redshift/latest/gsg/rs-gsg-authorize-cluster-access.html) anvisningar.</span><span class="sxs-lookup"><span data-stu-id="e1cc2-112">See [Authorize access toohello cluster](http://docs.aws.amazon.com/redshift/latest/gsg/rs-gsg-authorize-cluster-access.html) for instructions.</span></span>
* <span data-ttu-id="e1cc2-113">Om du flyttar data tooan Azure datalagret finns [Azure Data Center IP-adressintervall](https://www.microsoft.com/download/details.aspx?id=41653) för hello Compute IP-adress och SQL-intervall som används av hello Azure-datacenter.</span><span class="sxs-lookup"><span data-stu-id="e1cc2-113">If you are moving data tooan Azure data store, see [Azure Data Center IP Ranges](https://www.microsoft.com/download/details.aspx?id=41653) for hello Compute IP address and SQL ranges used by hello Azure data centers.</span></span>

## <a name="getting-started"></a><span data-ttu-id="e1cc2-114">Komma igång</span><span class="sxs-lookup"><span data-stu-id="e1cc2-114">Getting started</span></span>
<span data-ttu-id="e1cc2-115">Du kan skapa en pipeline med en kopia-aktivitet som flyttar data från en källa för Amazon Redshift med hjälp av olika verktyg/API: er.</span><span class="sxs-lookup"><span data-stu-id="e1cc2-115">You can create a pipeline with a copy activity that moves data from an Amazon Redshift source by using different tools/APIs.</span></span>

<span data-ttu-id="e1cc2-116">hello enklaste sättet toocreate en pipeline är toouse hello **guiden Kopiera**.</span><span class="sxs-lookup"><span data-stu-id="e1cc2-116">hello easiest way toocreate a pipeline is toouse hello **Copy Wizard**.</span></span> <span data-ttu-id="e1cc2-117">Se [Självstudier: skapa en pipeline med hjälp av guiden Kopiera](data-factory-copy-data-wizard-tutorial.md) för en snabb genomgång om hur du skapar en pipeline med hjälp av guiden för hello kopiera data.</span><span class="sxs-lookup"><span data-stu-id="e1cc2-117">See [Tutorial: Create a pipeline using Copy Wizard](data-factory-copy-data-wizard-tutorial.md) for a quick walkthrough on creating a pipeline using hello Copy data wizard.</span></span>

<span data-ttu-id="e1cc2-118">Du kan också använda följande verktyg toocreate en pipeline hello: **Azure-portalen**, **Visual Studio**, **Azure PowerShell**, **Azure Resource Manager-mall** , **.NET API**, och **REST API**.</span><span class="sxs-lookup"><span data-stu-id="e1cc2-118">You can also use hello following tools toocreate a pipeline: **Azure portal**, **Visual Studio**, **Azure PowerShell**, **Azure Resource Manager template**, **.NET API**, and **REST API**.</span></span> <span data-ttu-id="e1cc2-119">Se [kopiera aktivitet kursen](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) för stegvisa instruktioner toocreate en pipeline med en Kopieringsaktivitet.</span><span class="sxs-lookup"><span data-stu-id="e1cc2-119">See [Copy activity tutorial](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) for step-by-step instructions toocreate a pipeline with a copy activity.</span></span> 

<span data-ttu-id="e1cc2-120">Om du använder hello verktyg eller API: er kan utföra du hello följande steg toocreate en pipeline som flyttar data från en källdata lagra tooa sink-datalagret:</span><span class="sxs-lookup"><span data-stu-id="e1cc2-120">Whether you use hello tools or APIs, you perform hello following steps toocreate a pipeline that moves data from a source data store tooa sink data store:</span></span> 

1. <span data-ttu-id="e1cc2-121">Skapa **länkade tjänster** toolink indata och utdata lagrar tooyour data factory.</span><span class="sxs-lookup"><span data-stu-id="e1cc2-121">Create **linked services** toolink input and output data stores tooyour data factory.</span></span>
2. <span data-ttu-id="e1cc2-122">Skapa **datauppsättningar** toorepresent indata och utdata för hello kopieringsåtgärden.</span><span class="sxs-lookup"><span data-stu-id="e1cc2-122">Create **datasets** toorepresent input and output data for hello copy operation.</span></span> 
3. <span data-ttu-id="e1cc2-123">Skapa en **pipeline** med en kopia-aktivitet som tar en datamängd som indata och en dataset som utdata.</span><span class="sxs-lookup"><span data-stu-id="e1cc2-123">Create a **pipeline** with a copy activity that takes a dataset as an input and a dataset as an output.</span></span> 

<span data-ttu-id="e1cc2-124">När du använder guiden hello skapas automatiskt JSON definitioner för dessa Data Factory-enheter (länkade tjänster, datauppsättningar och hello pipeline) för dig.</span><span class="sxs-lookup"><span data-stu-id="e1cc2-124">When you use hello wizard, JSON definitions for these Data Factory entities (linked services, datasets, and hello pipeline) are automatically created for you.</span></span> <span data-ttu-id="e1cc2-125">När du använder Verktyg/API: er (utom .NET API), kan du definiera dessa Data Factory-enheter med hjälp av hello JSON-format.</span><span class="sxs-lookup"><span data-stu-id="e1cc2-125">When you use tools/APIs (except .NET API), you define these Data Factory entities by using hello JSON format.</span></span>  <span data-ttu-id="e1cc2-126">Ett exempel med JSON-definitioner för Data Factory-entiteter som ska använda toocopy data från ett dataarkiv för Amazon Redshift finns [JSON-exempel: kopiera data från Amazon Redshift tooAzure Blob](#json-example-copy-data-from-amazon-redshift-to-azure-blob) i den här artikeln.</span><span class="sxs-lookup"><span data-stu-id="e1cc2-126">For a sample with JSON definitions for Data Factory entities that are used toocopy data from an Amazon Redshift data store, see [JSON example: Copy data from Amazon Redshift tooAzure Blob](#json-example-copy-data-from-amazon-redshift-to-azure-blob) section of this article.</span></span> 

<span data-ttu-id="e1cc2-127">hello följande avsnitt innehåller information om JSON-egenskaper används toodefine Data Factory entiteter specifika tooAmazon Redshift:</span><span class="sxs-lookup"><span data-stu-id="e1cc2-127">hello following sections provide details about JSON properties that are used toodefine Data Factory entities specific tooAmazon Redshift:</span></span> 

## <a name="linked-service-properties"></a><span data-ttu-id="e1cc2-128">Länkad tjänstegenskaper</span><span class="sxs-lookup"><span data-stu-id="e1cc2-128">Linked service properties</span></span>
<span data-ttu-id="e1cc2-129">hello följande tabell ger en beskrivning för JSON-element specifika tooAmazon Redshift länkade tjänsten.</span><span class="sxs-lookup"><span data-stu-id="e1cc2-129">hello following table provides description for JSON elements specific tooAmazon Redshift linked service.</span></span>

| <span data-ttu-id="e1cc2-130">Egenskap</span><span class="sxs-lookup"><span data-stu-id="e1cc2-130">Property</span></span> | <span data-ttu-id="e1cc2-131">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="e1cc2-131">Description</span></span> | <span data-ttu-id="e1cc2-132">Krävs</span><span class="sxs-lookup"><span data-stu-id="e1cc2-132">Required</span></span> |
| --- | --- | --- |
| <span data-ttu-id="e1cc2-133">typ</span><span class="sxs-lookup"><span data-stu-id="e1cc2-133">type</span></span> |<span data-ttu-id="e1cc2-134">hello Typegenskapen måste anges till: **AmazonRedshift**.</span><span class="sxs-lookup"><span data-stu-id="e1cc2-134">hello type property must be set to: **AmazonRedshift**.</span></span> |<span data-ttu-id="e1cc2-135">Ja</span><span class="sxs-lookup"><span data-stu-id="e1cc2-135">Yes</span></span> |
| <span data-ttu-id="e1cc2-136">server</span><span class="sxs-lookup"><span data-stu-id="e1cc2-136">server</span></span> |<span data-ttu-id="e1cc2-137">IP-adressen eller värdnamnet namnet på hello Amazon Redshift server.</span><span class="sxs-lookup"><span data-stu-id="e1cc2-137">IP address or host name of hello Amazon Redshift server.</span></span> |<span data-ttu-id="e1cc2-138">Ja</span><span class="sxs-lookup"><span data-stu-id="e1cc2-138">Yes</span></span> |
| <span data-ttu-id="e1cc2-139">port</span><span class="sxs-lookup"><span data-stu-id="e1cc2-139">port</span></span> |<span data-ttu-id="e1cc2-140">hello antal hello TCP-port som hello Amazon Redshift server använder toolisten för klientanslutningar.</span><span class="sxs-lookup"><span data-stu-id="e1cc2-140">hello number of hello TCP port that hello Amazon Redshift server uses toolisten for client connections.</span></span> |<span data-ttu-id="e1cc2-141">Nej, standardvärde: 5439</span><span class="sxs-lookup"><span data-stu-id="e1cc2-141">No, default value: 5439</span></span> |
| <span data-ttu-id="e1cc2-142">Databasen</span><span class="sxs-lookup"><span data-stu-id="e1cc2-142">database</span></span> |<span data-ttu-id="e1cc2-143">Namnet på hello Amazon Redshift databas.</span><span class="sxs-lookup"><span data-stu-id="e1cc2-143">Name of hello Amazon Redshift database.</span></span> |<span data-ttu-id="e1cc2-144">Ja</span><span class="sxs-lookup"><span data-stu-id="e1cc2-144">Yes</span></span> |
| <span data-ttu-id="e1cc2-145">användarnamn</span><span class="sxs-lookup"><span data-stu-id="e1cc2-145">username</span></span> |<span data-ttu-id="e1cc2-146">Namnet på användaren som har åtkomst till toohello databas.</span><span class="sxs-lookup"><span data-stu-id="e1cc2-146">Name of user who has access toohello database.</span></span> |<span data-ttu-id="e1cc2-147">Ja</span><span class="sxs-lookup"><span data-stu-id="e1cc2-147">Yes</span></span> |
| <span data-ttu-id="e1cc2-148">lösenord</span><span class="sxs-lookup"><span data-stu-id="e1cc2-148">password</span></span> |<span data-ttu-id="e1cc2-149">Lösenordet för användarkontot för hello.</span><span class="sxs-lookup"><span data-stu-id="e1cc2-149">Password for hello user account.</span></span> |<span data-ttu-id="e1cc2-150">Ja</span><span class="sxs-lookup"><span data-stu-id="e1cc2-150">Yes</span></span> |

## <a name="dataset-properties"></a><span data-ttu-id="e1cc2-151">Egenskaper för datamängd</span><span class="sxs-lookup"><span data-stu-id="e1cc2-151">Dataset properties</span></span>
<span data-ttu-id="e1cc2-152">En fullständig lista över egenskaper som är tillgängliga för att definiera datauppsättningarna & avsnitt finns hello [skapa datauppsättningar](data-factory-create-datasets.md) artikel.</span><span class="sxs-lookup"><span data-stu-id="e1cc2-152">For a full list of sections & properties available for defining datasets, see hello [Creating datasets](data-factory-create-datasets.md) article.</span></span> <span data-ttu-id="e1cc2-153">Avsnitt som struktur, tillgänglighet och principen är liknande för alla typer av dataset (Azure SQL Azure blob, Azure-tabellen, osv.).</span><span class="sxs-lookup"><span data-stu-id="e1cc2-153">Sections such as structure, availability, and policy are similar for all dataset types (Azure SQL, Azure blob, Azure table, etc.).</span></span>

<span data-ttu-id="e1cc2-154">Hej **typeProperties** avsnittet är olika för varje typ av datauppsättningen.</span><span class="sxs-lookup"><span data-stu-id="e1cc2-154">hello **typeProperties** section is different for each type of dataset.</span></span> <span data-ttu-id="e1cc2-155">Den ger information om hello platsen för hello data i datalagret hello.</span><span class="sxs-lookup"><span data-stu-id="e1cc2-155">It provides information about hello location of hello data in hello data store.</span></span> <span data-ttu-id="e1cc2-156">Hej typeProperties avsnittet för dataset av typen **RelationalTable** (som omfattar Amazon Redshift dataset) har hello följande egenskaper</span><span class="sxs-lookup"><span data-stu-id="e1cc2-156">hello typeProperties section for dataset of type **RelationalTable** (which includes Amazon Redshift dataset) has hello following properties</span></span>

| <span data-ttu-id="e1cc2-157">Egenskap</span><span class="sxs-lookup"><span data-stu-id="e1cc2-157">Property</span></span> | <span data-ttu-id="e1cc2-158">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="e1cc2-158">Description</span></span> | <span data-ttu-id="e1cc2-159">Krävs</span><span class="sxs-lookup"><span data-stu-id="e1cc2-159">Required</span></span> |
| --- | --- | --- |
| <span data-ttu-id="e1cc2-160">tableName</span><span class="sxs-lookup"><span data-stu-id="e1cc2-160">tableName</span></span> |<span data-ttu-id="e1cc2-161">Namnet på hello tabell i hello Amazon Redshift databas som den länkade tjänsten refererar till.</span><span class="sxs-lookup"><span data-stu-id="e1cc2-161">Name of hello table in hello Amazon Redshift database that linked service refers to.</span></span> |<span data-ttu-id="e1cc2-162">Nej (om **frågan** av **RelationalSource** har angetts)</span><span class="sxs-lookup"><span data-stu-id="e1cc2-162">No (if **query** of **RelationalSource** is specified)</span></span> |

## <a name="copy-activity-properties"></a><span data-ttu-id="e1cc2-163">Kopiera egenskaper för aktivitet</span><span class="sxs-lookup"><span data-stu-id="e1cc2-163">Copy activity properties</span></span>
<span data-ttu-id="e1cc2-164">En fullständig lista över avsnitt & egenskaper som är tillgängliga för att definiera aktiviteter finns hello [skapar Pipelines](data-factory-create-pipelines.md) artikel.</span><span class="sxs-lookup"><span data-stu-id="e1cc2-164">For a full list of sections & properties available for defining activities, see hello [Creating Pipelines](data-factory-create-pipelines.md) article.</span></span> <span data-ttu-id="e1cc2-165">Egenskaper som namn, beskrivning, ingående och utgående tabeller och principer är tillgängliga för alla typer av aktiviteter.</span><span class="sxs-lookup"><span data-stu-id="e1cc2-165">Properties such as name, description, input and output tables, and policies are available for all types of activities.</span></span>

<span data-ttu-id="e1cc2-166">Medan egenskaper som är tillgängliga i hello **typeProperties** avsnittet hello aktivitet varierar med varje aktivitetstyp.</span><span class="sxs-lookup"><span data-stu-id="e1cc2-166">Whereas, properties available in hello **typeProperties** section of hello activity vary with each activity type.</span></span> <span data-ttu-id="e1cc2-167">För Kopieringsaktivitet kan varierar de beroende på hello typer av datakällor och sänkor.</span><span class="sxs-lookup"><span data-stu-id="e1cc2-167">For Copy activity, they vary depending on hello types of sources and sinks.</span></span>

<span data-ttu-id="e1cc2-168">Om källan för kopieringsaktiviteten är av typen **RelationalSource** (som omfattar Amazon Redshift), hello följande egenskaper finns i avsnittet typeProperties:</span><span class="sxs-lookup"><span data-stu-id="e1cc2-168">When source of copy activity is of type **RelationalSource** (which includes Amazon Redshift), hello following properties are available in typeProperties section:</span></span>

| <span data-ttu-id="e1cc2-169">Egenskap</span><span class="sxs-lookup"><span data-stu-id="e1cc2-169">Property</span></span> | <span data-ttu-id="e1cc2-170">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="e1cc2-170">Description</span></span> | <span data-ttu-id="e1cc2-171">Tillåtna värden</span><span class="sxs-lookup"><span data-stu-id="e1cc2-171">Allowed values</span></span> | <span data-ttu-id="e1cc2-172">Krävs</span><span class="sxs-lookup"><span data-stu-id="e1cc2-172">Required</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="e1cc2-173">DocumentDB</span><span class="sxs-lookup"><span data-stu-id="e1cc2-173">query</span></span> |<span data-ttu-id="e1cc2-174">Använda hello anpassad fråga tooread data.</span><span class="sxs-lookup"><span data-stu-id="e1cc2-174">Use hello custom query tooread data.</span></span> |<span data-ttu-id="e1cc2-175">SQL-sträng.</span><span class="sxs-lookup"><span data-stu-id="e1cc2-175">SQL query string.</span></span> <span data-ttu-id="e1cc2-176">Till exempel: Välj * från mytable prefix.</span><span class="sxs-lookup"><span data-stu-id="e1cc2-176">For example: select * from MyTable.</span></span> |<span data-ttu-id="e1cc2-177">Nej (om **tableName** av **dataset** har angetts)</span><span class="sxs-lookup"><span data-stu-id="e1cc2-177">No (if **tableName** of **dataset** is specified)</span></span> |

## <a name="json-example-copy-data-from-amazon-redshift-tooazure-blob"></a><span data-ttu-id="e1cc2-178">JSON-exempel: kopiera data från Amazon Redshift tooAzure Blob</span><span class="sxs-lookup"><span data-stu-id="e1cc2-178">JSON example: Copy data from Amazon Redshift tooAzure Blob</span></span>
<span data-ttu-id="e1cc2-179">Det här exemplet visas hur toocopy data från en Amazon Redshift databasen tooan Azure Blob Storage.</span><span class="sxs-lookup"><span data-stu-id="e1cc2-179">This sample shows how toocopy data from an Amazon Redshift database tooan Azure Blob Storage.</span></span> <span data-ttu-id="e1cc2-180">Dock datan kan kopieras **direkt** tooany av hello sänkor anges [här](data-factory-data-movement-activities.md#supported-data-stores-and-formats) med hello Kopieringsaktiviteten i Azure Data Factory.</span><span class="sxs-lookup"><span data-stu-id="e1cc2-180">However, data can be copied **directly** tooany of hello sinks stated [here](data-factory-data-movement-activities.md#supported-data-stores-and-formats) using hello Copy Activity in Azure Data Factory.</span></span>  

<span data-ttu-id="e1cc2-181">hello exemplet har hello följande data factory-enheter:</span><span class="sxs-lookup"><span data-stu-id="e1cc2-181">hello sample has hello following data factory entities:</span></span>

* <span data-ttu-id="e1cc2-182">En länkad tjänst av typen [AmazonRedshift](#linked-service-properties).</span><span class="sxs-lookup"><span data-stu-id="e1cc2-182">A linked service of type [AmazonRedshift](#linked-service-properties).</span></span>
* <span data-ttu-id="e1cc2-183">En länkad tjänst av typen [AzureStorage](data-factory-azure-blob-connector.md#linked-service-properties).</span><span class="sxs-lookup"><span data-stu-id="e1cc2-183">A linked service of type [AzureStorage](data-factory-azure-blob-connector.md#linked-service-properties).</span></span>
* <span data-ttu-id="e1cc2-184">Indata [dataset](data-factory-create-datasets.md) av typen [RelationalTable](#dataset-properties).</span><span class="sxs-lookup"><span data-stu-id="e1cc2-184">An input [dataset](data-factory-create-datasets.md) of type [RelationalTable](#dataset-properties).</span></span>
* <span data-ttu-id="e1cc2-185">Utdata [dataset](data-factory-create-datasets.md) av typen [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties).</span><span class="sxs-lookup"><span data-stu-id="e1cc2-185">An output [dataset](data-factory-create-datasets.md) of type [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties).</span></span>
* <span data-ttu-id="e1cc2-186">En [pipeline](data-factory-create-pipelines.md) med Kopieringsaktiviteten som använder [RelationalSource](#copy-activity-properties) och [BlobSink](data-factory-azure-blob-connector.md##copy-activity-properties).</span><span class="sxs-lookup"><span data-stu-id="e1cc2-186">A [pipeline](data-factory-create-pipelines.md) with Copy Activity that uses [RelationalSource](#copy-activity-properties) and [BlobSink](data-factory-azure-blob-connector.md##copy-activity-properties).</span></span>

<span data-ttu-id="e1cc2-187">hello exemplet kopierar data från ett frågeresultat i Amazon Redshift tooa blob varje timme.</span><span class="sxs-lookup"><span data-stu-id="e1cc2-187">hello sample copies data from a query result in Amazon Redshift tooa blob every hour.</span></span> <span data-ttu-id="e1cc2-188">hello JSON egenskaper som används i exemplen beskrivs i hello-exempel i följande avsnitt.</span><span class="sxs-lookup"><span data-stu-id="e1cc2-188">hello JSON properties used in these samples are described in sections following hello samples.</span></span>

<span data-ttu-id="e1cc2-189">**Amazon Redshift länkade tjänsten:**</span><span class="sxs-lookup"><span data-stu-id="e1cc2-189">**Amazon Redshift linked service:**</span></span>

```json
{
    "name": "AmazonRedshiftLinkedService",
    "properties":
    {
        "type": "AmazonRedshift",
        "typeProperties":
        {
            "server": "< hello IP address or host name of hello Amazon Redshift server >",
            "port": <hello number of hello TCP port that hello Amazon Redshift server uses toolisten for client connections.>,
            "database": "<hello database name of hello Amazon Redshift database>",
            "username": "<username>",
            "password": "<password>"
        }
    }
}
```

<span data-ttu-id="e1cc2-190">**Azure Storage länkade tjänsten:**</span><span class="sxs-lookup"><span data-stu-id="e1cc2-190">**Azure Storage linked service:**</span></span>

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
<span data-ttu-id="e1cc2-191">**Amazon Redshift inkommande datauppsättningen:**</span><span class="sxs-lookup"><span data-stu-id="e1cc2-191">**Amazon Redshift input dataset:**</span></span>

<span data-ttu-id="e1cc2-192">Ange `"external": true` informerar hello Data Factory-tjänsten som hello dataset är externa toohello data factory och inte tillverkas av en aktivitet i hello data factory.</span><span class="sxs-lookup"><span data-stu-id="e1cc2-192">Setting `"external": true` informs hello Data Factory service that hello dataset is external toohello data factory and is not produced by an activity in hello data factory.</span></span> <span data-ttu-id="e1cc2-193">Ange den här egenskapen tootrue på ett inkommande datamängd som inte tillverkas av en aktivitet i hello pipeline.</span><span class="sxs-lookup"><span data-stu-id="e1cc2-193">Set this property tootrue on an input dataset that is not produced by an activity in hello pipeline.</span></span>

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

<span data-ttu-id="e1cc2-194">**Azure Blob utdatauppsättningen:**</span><span class="sxs-lookup"><span data-stu-id="e1cc2-194">**Azure Blob output dataset:**</span></span>

<span data-ttu-id="e1cc2-195">Data skrivs tooa nya blob varje timme (frekvens: timme, intervall: 1).</span><span class="sxs-lookup"><span data-stu-id="e1cc2-195">Data is written tooa new blob every hour (frequency: hour, interval: 1).</span></span> <span data-ttu-id="e1cc2-196">hello mappsökväg för hello blob utvärderas dynamiskt baserat på hello starttiden för hello-segment som bearbetas.</span><span class="sxs-lookup"><span data-stu-id="e1cc2-196">hello folder path for hello blob is dynamically evaluated based on hello start time of hello slice that is being processed.</span></span> <span data-ttu-id="e1cc2-197">hello mappsökväg använder år, månad, dag och timmar delar av hello starttid.</span><span class="sxs-lookup"><span data-stu-id="e1cc2-197">hello folder path uses year, month, day, and hours parts of hello start time.</span></span>

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

<span data-ttu-id="e1cc2-198">**Kopiera aktivitet i en pipeline med Azure Redshift källa (RelationalSource) och Blob mottagare:**</span><span class="sxs-lookup"><span data-stu-id="e1cc2-198">**Copy activity in a pipeline with Azure Redshift source (RelationalSource) and Blob sink:**</span></span>

<span data-ttu-id="e1cc2-199">hello pipelinen innehåller en kopia-aktivitet som är konfigurerade toouse hello inkommande och utgående datauppsättningar och är schemalagda toorun varje timme.</span><span class="sxs-lookup"><span data-stu-id="e1cc2-199">hello pipeline contains a Copy Activity that is configured toouse hello input and output datasets and is scheduled toorun every hour.</span></span> <span data-ttu-id="e1cc2-200">I hello pipeline JSON-definitionen hello **källa** typ har angetts för**RelationalSource** och **sink** typ har angetts för**BlobSink**.</span><span class="sxs-lookup"><span data-stu-id="e1cc2-200">In hello pipeline JSON definition, hello **source** type is set too**RelationalSource** and **sink** type is set too**BlobSink**.</span></span> <span data-ttu-id="e1cc2-201">hello SQL-frågan som angetts för hello **frågan** egenskapen väljer hello data i hello tidigare timme toocopy.</span><span class="sxs-lookup"><span data-stu-id="e1cc2-201">hello SQL query specified for hello **query** property selects hello data in hello past hour toocopy.</span></span>

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
### <a name="type-mapping-for-amazon-redshift"></a><span data-ttu-id="e1cc2-202">Mappning för Amazon Redshift</span><span class="sxs-lookup"><span data-stu-id="e1cc2-202">Type mapping for Amazon Redshift</span></span>
<span data-ttu-id="e1cc2-203">Som anges i hello [data movement aktiviteter](data-factory-data-movement-activities.md) artikeln kopieringsaktiviteten utför automatisk konverteringar från källan typer toosink typer med hello följande två sätt:</span><span class="sxs-lookup"><span data-stu-id="e1cc2-203">As mentioned in hello [data movement activities](data-factory-data-movement-activities.md) article, Copy activity performs automatic type conversions from source types toosink types with hello following two-step approach:</span></span>

1. <span data-ttu-id="e1cc2-204">Konvertera från inbyggda typer too.NET källtypen</span><span class="sxs-lookup"><span data-stu-id="e1cc2-204">Convert from native source types too.NET type</span></span>
2. <span data-ttu-id="e1cc2-205">Konvertera från .NET typen toonative Mottagartypen</span><span class="sxs-lookup"><span data-stu-id="e1cc2-205">Convert from .NET type toonative sink type</span></span>

<span data-ttu-id="e1cc2-206">När du flyttar data tooAmazon Redshift används hello följande mappningar från Amazon Redshift typer too.NET typer.</span><span class="sxs-lookup"><span data-stu-id="e1cc2-206">When moving data tooAmazon Redshift, hello following mappings are used from Amazon Redshift types too.NET types.</span></span>

| <span data-ttu-id="e1cc2-207">Amazon Redshift typ</span><span class="sxs-lookup"><span data-stu-id="e1cc2-207">Amazon Redshift Type</span></span> | <span data-ttu-id="e1cc2-208">.NET baserat typ</span><span class="sxs-lookup"><span data-stu-id="e1cc2-208">.NET Based Type</span></span> |
| --- | --- |
| <span data-ttu-id="e1cc2-209">SMALLINT</span><span class="sxs-lookup"><span data-stu-id="e1cc2-209">SMALLINT</span></span> |<span data-ttu-id="e1cc2-210">Int16</span><span class="sxs-lookup"><span data-stu-id="e1cc2-210">Int16</span></span> |
| <span data-ttu-id="e1cc2-211">HELTAL</span><span class="sxs-lookup"><span data-stu-id="e1cc2-211">INTEGER</span></span> |<span data-ttu-id="e1cc2-212">Int32</span><span class="sxs-lookup"><span data-stu-id="e1cc2-212">Int32</span></span> |
| <span data-ttu-id="e1cc2-213">BIGINT</span><span class="sxs-lookup"><span data-stu-id="e1cc2-213">BIGINT</span></span> |<span data-ttu-id="e1cc2-214">Int64</span><span class="sxs-lookup"><span data-stu-id="e1cc2-214">Int64</span></span> |
| <span data-ttu-id="e1cc2-215">DECIMAL</span><span class="sxs-lookup"><span data-stu-id="e1cc2-215">DECIMAL</span></span> |<span data-ttu-id="e1cc2-216">Decimal</span><span class="sxs-lookup"><span data-stu-id="e1cc2-216">Decimal</span></span> |
| <span data-ttu-id="e1cc2-217">VERKLIG</span><span class="sxs-lookup"><span data-stu-id="e1cc2-217">REAL</span></span> |<span data-ttu-id="e1cc2-218">Enskild</span><span class="sxs-lookup"><span data-stu-id="e1cc2-218">Single</span></span> |
| <span data-ttu-id="e1cc2-219">DUBBEL PRECISION</span><span class="sxs-lookup"><span data-stu-id="e1cc2-219">DOUBLE PRECISION</span></span> |<span data-ttu-id="e1cc2-220">dubbla</span><span class="sxs-lookup"><span data-stu-id="e1cc2-220">Double</span></span> |
| <span data-ttu-id="e1cc2-221">BOOLESKT VÄRDE</span><span class="sxs-lookup"><span data-stu-id="e1cc2-221">BOOLEAN</span></span> |<span data-ttu-id="e1cc2-222">Sträng</span><span class="sxs-lookup"><span data-stu-id="e1cc2-222">String</span></span> |
| <span data-ttu-id="e1cc2-223">CHAR</span><span class="sxs-lookup"><span data-stu-id="e1cc2-223">CHAR</span></span> |<span data-ttu-id="e1cc2-224">Sträng</span><span class="sxs-lookup"><span data-stu-id="e1cc2-224">String</span></span> |
| <span data-ttu-id="e1cc2-225">VARCHAR</span><span class="sxs-lookup"><span data-stu-id="e1cc2-225">VARCHAR</span></span> |<span data-ttu-id="e1cc2-226">Sträng</span><span class="sxs-lookup"><span data-stu-id="e1cc2-226">String</span></span> |
| <span data-ttu-id="e1cc2-227">DATUM</span><span class="sxs-lookup"><span data-stu-id="e1cc2-227">DATE</span></span> |<span data-ttu-id="e1cc2-228">Datum och tid</span><span class="sxs-lookup"><span data-stu-id="e1cc2-228">DateTime</span></span> |
| <span data-ttu-id="e1cc2-229">TIDSSTÄMPEL</span><span class="sxs-lookup"><span data-stu-id="e1cc2-229">TIMESTAMP</span></span> |<span data-ttu-id="e1cc2-230">Datum och tid</span><span class="sxs-lookup"><span data-stu-id="e1cc2-230">DateTime</span></span> |
| <span data-ttu-id="e1cc2-231">TEXT</span><span class="sxs-lookup"><span data-stu-id="e1cc2-231">TEXT</span></span> |<span data-ttu-id="e1cc2-232">Sträng</span><span class="sxs-lookup"><span data-stu-id="e1cc2-232">String</span></span> |

## <a name="map-source-toosink-columns"></a><span data-ttu-id="e1cc2-233">Mappa källkolumner toosink</span><span class="sxs-lookup"><span data-stu-id="e1cc2-233">Map source toosink columns</span></span>
<span data-ttu-id="e1cc2-234">toolearn mappning tabellkolumner i källan dataset toocolumns i sink dataset finns [mappa dataset kolumner i Azure Data Factory](data-factory-map-columns.md).</span><span class="sxs-lookup"><span data-stu-id="e1cc2-234">toolearn about mapping columns in source dataset toocolumns in sink dataset, see [Mapping dataset columns in Azure Data Factory](data-factory-map-columns.md).</span></span>

## <a name="repeatable-read-from-relational-sources"></a><span data-ttu-id="e1cc2-235">Upprepbar läsning från relationella källor</span><span class="sxs-lookup"><span data-stu-id="e1cc2-235">Repeatable read from relational sources</span></span>
<span data-ttu-id="e1cc2-236">När du kopierar data från relationella datalager, Kom ihåg tooavoid repeterbarhet oönskade resultat.</span><span class="sxs-lookup"><span data-stu-id="e1cc2-236">When copying data from relational data stores, keep repeatability in mind tooavoid unintended outcomes.</span></span> <span data-ttu-id="e1cc2-237">I Azure Data Factory, kan du köra en sektor manuellt.</span><span class="sxs-lookup"><span data-stu-id="e1cc2-237">In Azure Data Factory, you can rerun a slice manually.</span></span> <span data-ttu-id="e1cc2-238">Du kan också konfigurera i principen för en dataset så att ett segment som körs när ett fel uppstår.</span><span class="sxs-lookup"><span data-stu-id="e1cc2-238">You can also configure retry policy for a dataset so that a slice is rerun when a failure occurs.</span></span> <span data-ttu-id="e1cc2-239">När ett segment körs på antingen sätt måste toomake att som hello samma data läses oavsett hur många gånger ett segment körs.</span><span class="sxs-lookup"><span data-stu-id="e1cc2-239">When a slice is rerun in either way, you need toomake sure that hello same data is read no matter how many times a slice is run.</span></span> <span data-ttu-id="e1cc2-240">Se [Repeatable läsa från relationella källor](data-factory-repeatable-copy.md#repeatable-read-from-relational-sources)</span><span class="sxs-lookup"><span data-stu-id="e1cc2-240">See [Repeatable read from relational sources](data-factory-repeatable-copy.md#repeatable-read-from-relational-sources)</span></span>

## <a name="performance-and-tuning"></a><span data-ttu-id="e1cc2-241">Prestanda och finjustering</span><span class="sxs-lookup"><span data-stu-id="e1cc2-241">Performance and Tuning</span></span>
<span data-ttu-id="e1cc2-242">Se [kopiera aktivitet prestanda och justera guiden](data-factory-copy-activity-performance.md) toolearn om nyckeln faktorer som påverkan prestanda för flytt av data (Kopieringsaktiviteten) i Azure Data Factory och olika sätt toooptimize den.</span><span class="sxs-lookup"><span data-stu-id="e1cc2-242">See [Copy Activity Performance & Tuning Guide](data-factory-copy-activity-performance.md) toolearn about key factors that impact performance of data movement (Copy Activity) in Azure Data Factory and various ways toooptimize it.</span></span>

## <a name="next-steps"></a><span data-ttu-id="e1cc2-243">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="e1cc2-243">Next Steps</span></span>
<span data-ttu-id="e1cc2-244">Se hello följande artiklar:</span><span class="sxs-lookup"><span data-stu-id="e1cc2-244">See hello following articles:</span></span>

* <span data-ttu-id="e1cc2-245">[Kopiera aktivitet kursen](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) stegvisa instruktioner för att skapa en pipeline med en kopia-aktivitet.</span><span class="sxs-lookup"><span data-stu-id="e1cc2-245">[Copy Activity tutorial](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) for step-by-step instructions for creating a pipeline with a Copy Activity.</span></span>
