---
title: "Flytta data från Cassandra med hjälp av Data Factory | Microsoft Docs"
description: "Läs mer om hur du flyttar data från en lokal Cassandra-databas med Azure Data Factory."
services: data-factory
documentationcenter: 
author: linda33wj
manager: jhubbard
editor: monicar
ms.assetid: 085cc312-42ca-4f43-aa35-535b35a102d5
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/27/2017
ms.author: jingwang
ms.openlocfilehash: f2b225bdbdf2880d26a6ab5f992301bf0a804b0d
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/03/2017
---
# <a name="move-data-from-an-on-premises-cassandra-database-using-azure-data-factory"></a><span data-ttu-id="f64f5-103">Flytta data från en lokal Cassandra-databas med Azure Data Factory</span><span class="sxs-lookup"><span data-stu-id="f64f5-103">Move data from an on-premises Cassandra database using Azure Data Factory</span></span>
<span data-ttu-id="f64f5-104">Den här artikeln förklarar hur du använder aktiviteten kopiera i Azure Data Factory för att flytta data från en lokal Cassandra databas.</span><span class="sxs-lookup"><span data-stu-id="f64f5-104">This article explains how to use the Copy Activity in Azure Data Factory to move data from an on-premises Cassandra database.</span></span> <span data-ttu-id="f64f5-105">Den bygger på den [Data Movement aktiviteter](data-factory-data-movement-activities.md) artikel som presenterar en allmän översikt över dataflyttning med copy-aktivitet.</span><span class="sxs-lookup"><span data-stu-id="f64f5-105">It builds on the [Data Movement Activities](data-factory-data-movement-activities.md) article, which presents a general overview of data movement with the copy activity.</span></span>

<span data-ttu-id="f64f5-106">Du kan kopiera data från ett lokalt Cassandra dataarkiv till alla stöds sink-datalagret.</span><span class="sxs-lookup"><span data-stu-id="f64f5-106">You can copy data from an on-premises Cassandra data store to any supported sink data store.</span></span> <span data-ttu-id="f64f5-107">En lista över datakällor som stöds som sänkor av kopieringsaktiviteten, finns det [stöds datalager](data-factory-data-movement-activities.md#supported-data-stores-and-formats) tabell.</span><span class="sxs-lookup"><span data-stu-id="f64f5-107">For a list of data stores supported as sinks by the copy activity, see the [Supported data stores](data-factory-data-movement-activities.md#supported-data-stores-and-formats) table.</span></span> <span data-ttu-id="f64f5-108">Data factory stöder för närvarande endast flytta data från ett dataarkiv som Cassandra till andra databaser, men inte för att flytta data från andra datalager till en Cassandra dataarkiv.</span><span class="sxs-lookup"><span data-stu-id="f64f5-108">Data factory currently supports only moving data from a Cassandra data store to other data stores, but not for moving data from other data stores to a Cassandra data store.</span></span> 

## <a name="supported-versions"></a><span data-ttu-id="f64f5-109">Versioner som stöds</span><span class="sxs-lookup"><span data-stu-id="f64f5-109">Supported versions</span></span>
<span data-ttu-id="f64f5-110">Stöder följande versioner av Cassandra för Cassandra-anslutningen: 2.X.</span><span class="sxs-lookup"><span data-stu-id="f64f5-110">The Cassandra connector supports the following versions of Cassandra: 2.X.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="f64f5-111">Krav</span><span class="sxs-lookup"><span data-stu-id="f64f5-111">Prerequisites</span></span>
<span data-ttu-id="f64f5-112">För Azure Data Factory-tjänsten för att kunna ansluta till din lokala Cassandra databas måste du installera en Data Management Gateway på samma dator som värd för databasen eller på en separat dator för att undvika konkurrerar om resurser med databasen.</span><span class="sxs-lookup"><span data-stu-id="f64f5-112">For the Azure Data Factory service to be able to connect to your on-premises Cassandra database, you must install a Data Management Gateway on the same machine that hosts the database or on a separate machine to avoid competing for resources with the database.</span></span> <span data-ttu-id="f64f5-113">Data Management Gateway är en komponent som ansluter lokala datakällor till molntjänster i en säker och hanterad sätt.</span><span class="sxs-lookup"><span data-stu-id="f64f5-113">Data Management Gateway is a component that connects on-premises data sources to cloud services in a secure and managed way.</span></span> <span data-ttu-id="f64f5-114">Se [Data Management Gateway](data-factory-data-management-gateway.md) artikeln för information om Data Management Gateway.</span><span class="sxs-lookup"><span data-stu-id="f64f5-114">See [Data Management Gateway](data-factory-data-management-gateway.md) article for details about Data Management Gateway.</span></span> <span data-ttu-id="f64f5-115">Se [flytta data från lokalt till molnet](data-factory-move-data-between-onprem-and-cloud.md) artikel stegvisa instruktioner om hur du konfigurerar gatewayen som en pipeline för data att flytta data.</span><span class="sxs-lookup"><span data-stu-id="f64f5-115">See [Move data from on-premises to cloud](data-factory-move-data-between-onprem-and-cloud.md) article for step-by-step instructions on setting up the gateway a data pipeline to move data.</span></span>

<span data-ttu-id="f64f5-116">Gatewayen måste du använda för att ansluta till en databas för Cassandra även om databasen finns i molnet, till exempel på en Azure IaaS-VM.</span><span class="sxs-lookup"><span data-stu-id="f64f5-116">You must use the gateway to connect to a Cassandra database even if the database is hosted in the cloud, for example, on an Azure IaaS VM.</span></span> <span data-ttu-id="f64f5-117">Y du kan ha gatewayen på samma virtuella dator som är värd för databasen eller på en separat virtuell dator så länge som gatewayen kan ansluta till databasen.</span><span class="sxs-lookup"><span data-stu-id="f64f5-117">Y You can have the gateway on the same VM that hosts the database or on a separate VM as long as the gateway can connect to the database.</span></span>  

<span data-ttu-id="f64f5-118">När du installerar gateway installeras automatiskt en Microsoft Cassandra ODBC-drivrutinen används för att ansluta till Cassandra-databasen.</span><span class="sxs-lookup"><span data-stu-id="f64f5-118">When you install the gateway, it automatically installs a Microsoft Cassandra ODBC driver used to connect to Cassandra database.</span></span> <span data-ttu-id="f64f5-119">Därför behöver du inte manuellt installera en drivrutin på gateway-datorn när du kopierar data från databasen Cassandra.</span><span class="sxs-lookup"><span data-stu-id="f64f5-119">Therefore, you don't need to manually install any driver on the gateway machine when copying data from the Cassandra database.</span></span> 

> [!NOTE]
> <span data-ttu-id="f64f5-120">Se [felsökning av problem med gateway](data-factory-data-management-gateway.md#troubleshooting-gateway-issues) tips om hur du felsöker anslutning /-gateway relaterade problem.</span><span class="sxs-lookup"><span data-stu-id="f64f5-120">See [Troubleshoot gateway issues](data-factory-data-management-gateway.md#troubleshooting-gateway-issues) for tips on troubleshooting connection/gateway related issues.</span></span>

## <a name="getting-started"></a><span data-ttu-id="f64f5-121">Komma igång</span><span class="sxs-lookup"><span data-stu-id="f64f5-121">Getting started</span></span>
<span data-ttu-id="f64f5-122">Du kan skapa en pipeline med en kopia-aktivitet som flyttar data från en lokal Cassandra data store med hjälp av olika verktyg/API: er.</span><span class="sxs-lookup"><span data-stu-id="f64f5-122">You can create a pipeline with a copy activity that moves data from an on-premises Cassandra data store by using different tools/APIs.</span></span> 

- <span data-ttu-id="f64f5-123">Det enklaste sättet att skapa en pipeline är att använda den **guiden Kopiera**.</span><span class="sxs-lookup"><span data-stu-id="f64f5-123">The easiest way to create a pipeline is to use the **Copy Wizard**.</span></span> <span data-ttu-id="f64f5-124">Finns [Självstudier: skapa en pipeline med hjälp av guiden Kopiera](data-factory-copy-data-wizard-tutorial.md) för en snabb genomgång om hur du skapar en pipeline med hjälp av guiden Kopiera data.</span><span class="sxs-lookup"><span data-stu-id="f64f5-124">See [Tutorial: Create a pipeline using Copy Wizard](data-factory-copy-data-wizard-tutorial.md) for a quick walkthrough on creating a pipeline using the Copy data wizard.</span></span> 
- <span data-ttu-id="f64f5-125">Du kan också använda följande verktyg för att skapa en pipeline: **Azure-portalen**, **Visual Studio**, **Azure PowerShell**, **Azure Resource Manager-mall**, **.NET API**, och **REST API**.</span><span class="sxs-lookup"><span data-stu-id="f64f5-125">You can also use the following tools to create a pipeline: **Azure portal**, **Visual Studio**, **Azure PowerShell**, **Azure Resource Manager template**, **.NET API**, and **REST API**.</span></span> <span data-ttu-id="f64f5-126">Se [kopiera aktivitet kursen](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) för stegvisa instruktioner för att skapa en pipeline med en Kopieringsaktivitet.</span><span class="sxs-lookup"><span data-stu-id="f64f5-126">See [Copy activity tutorial](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) for step-by-step instructions to create a pipeline with a copy activity.</span></span> 

<span data-ttu-id="f64f5-127">Om du använder verktyg eller API: er, kan du utföra följande steg för att skapa en pipeline som flyttar data från ett dataarkiv som källa till ett dataarkiv som mottagare:</span><span class="sxs-lookup"><span data-stu-id="f64f5-127">Whether you use the tools or APIs, you perform the following steps to create a pipeline that moves data from a source data store to a sink data store:</span></span>

1. <span data-ttu-id="f64f5-128">Skapa **länkade tjänster** att länka inkommande och utgående data lagras till din data factory.</span><span class="sxs-lookup"><span data-stu-id="f64f5-128">Create **linked services** to link input and output data stores to your data factory.</span></span>
2. <span data-ttu-id="f64f5-129">Skapa **datauppsättningar** att representera inkommande och utgående data för kopieringen.</span><span class="sxs-lookup"><span data-stu-id="f64f5-129">Create **datasets** to represent input and output data for the copy operation.</span></span> 
3. <span data-ttu-id="f64f5-130">Skapa en **pipeline** med en kopia-aktivitet som tar en datamängd som indata och en dataset som utdata.</span><span class="sxs-lookup"><span data-stu-id="f64f5-130">Create a **pipeline** with a copy activity that takes a dataset as an input and a dataset as an output.</span></span> 

<span data-ttu-id="f64f5-131">När du använder guiden skapas JSON definitioner för dessa Data Factory-enheter (länkade tjänster, datauppsättningar och pipelinen) automatiskt för dig.</span><span class="sxs-lookup"><span data-stu-id="f64f5-131">When you use the wizard, JSON definitions for these Data Factory entities (linked services, datasets, and the pipeline) are automatically created for you.</span></span> <span data-ttu-id="f64f5-132">När du använder Verktyg/API: er (utom .NET API), kan du definiera dessa Data Factory-enheter med hjälp av JSON-format.</span><span class="sxs-lookup"><span data-stu-id="f64f5-132">When you use tools/APIs (except .NET API), you define these Data Factory entities by using the JSON format.</span></span>  <span data-ttu-id="f64f5-133">Ett exempel med JSON-definitioner för Data Factory-entiteter som används för att kopiera data från ett lokalt Cassandra dataarkiv finns [JSON-exempel: kopiera data från Cassandra till Azure Blob](#json-example-copy-data-from-cassandra-to-azure-blob) i den här artikeln.</span><span class="sxs-lookup"><span data-stu-id="f64f5-133">For a sample with JSON definitions for Data Factory entities that are used to copy data from an on-premises Cassandra data store, see [JSON example: Copy data from Cassandra to Azure Blob](#json-example-copy-data-from-cassandra-to-azure-blob) section of this article.</span></span> 

<span data-ttu-id="f64f5-134">Följande avsnitt innehåller information om JSON-egenskaper som används för att definiera Data Factory entiteter till datakällan Cassandra:</span><span class="sxs-lookup"><span data-stu-id="f64f5-134">The following sections provide details about JSON properties that are used to define Data Factory entities specific to a Cassandra data store:</span></span>

## <a name="linked-service-properties"></a><span data-ttu-id="f64f5-135">Länkad tjänstegenskaper</span><span class="sxs-lookup"><span data-stu-id="f64f5-135">Linked service properties</span></span>
<span data-ttu-id="f64f5-136">Följande tabell innehåller en beskrivning för JSON-element som är specifika för Cassandra länkad tjänst.</span><span class="sxs-lookup"><span data-stu-id="f64f5-136">The following table provides description for JSON elements specific to Cassandra linked service.</span></span>

| <span data-ttu-id="f64f5-137">Egenskap</span><span class="sxs-lookup"><span data-stu-id="f64f5-137">Property</span></span> | <span data-ttu-id="f64f5-138">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="f64f5-138">Description</span></span> | <span data-ttu-id="f64f5-139">Krävs</span><span class="sxs-lookup"><span data-stu-id="f64f5-139">Required</span></span> |
| --- | --- | --- |
| <span data-ttu-id="f64f5-140">typ</span><span class="sxs-lookup"><span data-stu-id="f64f5-140">type</span></span> |<span data-ttu-id="f64f5-141">Egenskapen type måste anges till: **OnPremisesCassandra**</span><span class="sxs-lookup"><span data-stu-id="f64f5-141">The type property must be set to: **OnPremisesCassandra**</span></span> |<span data-ttu-id="f64f5-142">Ja</span><span class="sxs-lookup"><span data-stu-id="f64f5-142">Yes</span></span> |
| <span data-ttu-id="f64f5-143">värden</span><span class="sxs-lookup"><span data-stu-id="f64f5-143">host</span></span> |<span data-ttu-id="f64f5-144">En eller flera IP-adresser eller värdnamn Cassandra servrar.</span><span class="sxs-lookup"><span data-stu-id="f64f5-144">One or more IP addresses or host names of Cassandra servers.</span></span><br/><br/><span data-ttu-id="f64f5-145">Ange en kommaavgränsad lista med IP-adresser eller värdnamn för att ansluta till alla servrar samtidigt.</span><span class="sxs-lookup"><span data-stu-id="f64f5-145">Specify a comma-separated list of IP addresses or host names to connect to all servers concurrently.</span></span> |<span data-ttu-id="f64f5-146">Ja</span><span class="sxs-lookup"><span data-stu-id="f64f5-146">Yes</span></span> |
| <span data-ttu-id="f64f5-147">port</span><span class="sxs-lookup"><span data-stu-id="f64f5-147">port</span></span> |<span data-ttu-id="f64f5-148">TCP-porten som används av Cassandra-server för att lyssna efter anslutningar.</span><span class="sxs-lookup"><span data-stu-id="f64f5-148">The TCP port that the Cassandra server uses to listen for client connections.</span></span> |<span data-ttu-id="f64f5-149">Nej, standardvärde: 9042</span><span class="sxs-lookup"><span data-stu-id="f64f5-149">No, default value: 9042</span></span> |
| <span data-ttu-id="f64f5-150">AuthenticationType</span><span class="sxs-lookup"><span data-stu-id="f64f5-150">authenticationType</span></span> |<span data-ttu-id="f64f5-151">Grundläggande eller anonym</span><span class="sxs-lookup"><span data-stu-id="f64f5-151">Basic, or Anonymous</span></span> |<span data-ttu-id="f64f5-152">Ja</span><span class="sxs-lookup"><span data-stu-id="f64f5-152">Yes</span></span> |
| <span data-ttu-id="f64f5-153">användarnamn</span><span class="sxs-lookup"><span data-stu-id="f64f5-153">username</span></span> |<span data-ttu-id="f64f5-154">Ange användarnamnet för användarkontot.</span><span class="sxs-lookup"><span data-stu-id="f64f5-154">Specify user name for the user account.</span></span> |<span data-ttu-id="f64f5-155">Ja, om authenticationType anges till Basic.</span><span class="sxs-lookup"><span data-stu-id="f64f5-155">Yes, if authenticationType is set to Basic.</span></span> |
| <span data-ttu-id="f64f5-156">lösenord</span><span class="sxs-lookup"><span data-stu-id="f64f5-156">password</span></span> |<span data-ttu-id="f64f5-157">Ange lösenordet för användarkontot.</span><span class="sxs-lookup"><span data-stu-id="f64f5-157">Specify password for the user account.</span></span> |<span data-ttu-id="f64f5-158">Ja, om authenticationType anges till Basic.</span><span class="sxs-lookup"><span data-stu-id="f64f5-158">Yes, if authenticationType is set to Basic.</span></span> |
| <span data-ttu-id="f64f5-159">gatewayName</span><span class="sxs-lookup"><span data-stu-id="f64f5-159">gatewayName</span></span> |<span data-ttu-id="f64f5-160">Namnet på den gateway som används för att ansluta till lokalt Cassandra-databasen.</span><span class="sxs-lookup"><span data-stu-id="f64f5-160">The name of the gateway that is used to connect to the on-premises Cassandra database.</span></span> |<span data-ttu-id="f64f5-161">Ja</span><span class="sxs-lookup"><span data-stu-id="f64f5-161">Yes</span></span> |
| <span data-ttu-id="f64f5-162">encryptedCredential</span><span class="sxs-lookup"><span data-stu-id="f64f5-162">encryptedCredential</span></span> |<span data-ttu-id="f64f5-163">Autentiseringsuppgifter har krypterats med gatewayen.</span><span class="sxs-lookup"><span data-stu-id="f64f5-163">Credential encrypted by the gateway.</span></span> |<span data-ttu-id="f64f5-164">Nej</span><span class="sxs-lookup"><span data-stu-id="f64f5-164">No</span></span> |

## <a name="dataset-properties"></a><span data-ttu-id="f64f5-165">Egenskaper för datamängd</span><span class="sxs-lookup"><span data-stu-id="f64f5-165">Dataset properties</span></span>
<span data-ttu-id="f64f5-166">En fullständig lista över egenskaper som är tillgängliga för att definiera datauppsättningarna & avsnitt finns i [skapa datauppsättningar](data-factory-create-datasets.md) artikel.</span><span class="sxs-lookup"><span data-stu-id="f64f5-166">For a full list of sections & properties available for defining datasets, see the [Creating datasets](data-factory-create-datasets.md) article.</span></span> <span data-ttu-id="f64f5-167">Avsnitt som struktur, tillgänglighet och princip på en datamängd JSON är liknande för alla typer av dataset (Azure SQL Azure blob, Azure-tabellen, osv.).</span><span class="sxs-lookup"><span data-stu-id="f64f5-167">Sections such as structure, availability, and policy of a dataset JSON are similar for all dataset types (Azure SQL, Azure blob, Azure table, etc.).</span></span>

<span data-ttu-id="f64f5-168">Den **typeProperties** avsnitt är olika för varje typ av dataset och innehåller information om placeringen av data i datalagret.</span><span class="sxs-lookup"><span data-stu-id="f64f5-168">The **typeProperties** section is different for each type of dataset and provides information about the location of the data in the data store.</span></span> <span data-ttu-id="f64f5-169">TypeProperties avsnittet för dataset av typen **CassandraTable** har följande egenskaper</span><span class="sxs-lookup"><span data-stu-id="f64f5-169">The typeProperties section for dataset of type **CassandraTable** has the following properties</span></span>

| <span data-ttu-id="f64f5-170">Egenskap</span><span class="sxs-lookup"><span data-stu-id="f64f5-170">Property</span></span> | <span data-ttu-id="f64f5-171">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="f64f5-171">Description</span></span> | <span data-ttu-id="f64f5-172">Krävs</span><span class="sxs-lookup"><span data-stu-id="f64f5-172">Required</span></span> |
| --- | --- | --- |
| <span data-ttu-id="f64f5-173">keyspace</span><span class="sxs-lookup"><span data-stu-id="f64f5-173">keyspace</span></span> |<span data-ttu-id="f64f5-174">Namnet på keyspace eller schema i Cassandra databasen.</span><span class="sxs-lookup"><span data-stu-id="f64f5-174">Name of the keyspace or schema in Cassandra database.</span></span> |<span data-ttu-id="f64f5-175">Ja (om **frågan** för **CassandraSource** har inte definierats).</span><span class="sxs-lookup"><span data-stu-id="f64f5-175">Yes (If **query** for **CassandraSource** is not defined).</span></span> |
| <span data-ttu-id="f64f5-176">tableName</span><span class="sxs-lookup"><span data-stu-id="f64f5-176">tableName</span></span> |<span data-ttu-id="f64f5-177">Namnet på tabellen i Cassandra databas.</span><span class="sxs-lookup"><span data-stu-id="f64f5-177">Name of the table in Cassandra database.</span></span> |<span data-ttu-id="f64f5-178">Ja (om **frågan** för **CassandraSource** har inte definierats).</span><span class="sxs-lookup"><span data-stu-id="f64f5-178">Yes (If **query** for **CassandraSource** is not defined).</span></span> |

## <a name="copy-activity-properties"></a><span data-ttu-id="f64f5-179">Kopiera egenskaper för aktivitet</span><span class="sxs-lookup"><span data-stu-id="f64f5-179">Copy activity properties</span></span>
<span data-ttu-id="f64f5-180">En fullständig lista över avsnitt & egenskaper som är tillgängliga för att definiera aktiviteter finns i [skapar Pipelines](data-factory-create-pipelines.md) artikel.</span><span class="sxs-lookup"><span data-stu-id="f64f5-180">For a full list of sections & properties available for defining activities, see the [Creating Pipelines](data-factory-create-pipelines.md) article.</span></span> <span data-ttu-id="f64f5-181">Egenskaper som namn, beskrivning, ingående och utgående tabeller och principen är tillgängliga för alla typer av aktiviteter.</span><span class="sxs-lookup"><span data-stu-id="f64f5-181">Properties such as name, description, input and output tables, and policy are available for all types of activities.</span></span>

<span data-ttu-id="f64f5-182">De egenskaper som är tillgängliga i avsnittet typeProperties i aktiviteten varierar beroende på varje aktivitetstyp.</span><span class="sxs-lookup"><span data-stu-id="f64f5-182">Whereas, properties available in the typeProperties section of the activity vary with each activity type.</span></span> <span data-ttu-id="f64f5-183">För Kopieringsaktivitet kan variera de beroende på vilka typer av datakällor och sänkor.</span><span class="sxs-lookup"><span data-stu-id="f64f5-183">For Copy activity, they vary depending on the types of sources and sinks.</span></span>

<span data-ttu-id="f64f5-184">När datakällan är av typen **CassandraSource**, följande egenskaper finns i avsnittet typeProperties:</span><span class="sxs-lookup"><span data-stu-id="f64f5-184">When source is of type **CassandraSource**, the following properties are available in typeProperties section:</span></span>

| <span data-ttu-id="f64f5-185">Egenskap</span><span class="sxs-lookup"><span data-stu-id="f64f5-185">Property</span></span> | <span data-ttu-id="f64f5-186">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="f64f5-186">Description</span></span> | <span data-ttu-id="f64f5-187">Tillåtna värden</span><span class="sxs-lookup"><span data-stu-id="f64f5-187">Allowed values</span></span> | <span data-ttu-id="f64f5-188">Krävs</span><span class="sxs-lookup"><span data-stu-id="f64f5-188">Required</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="f64f5-189">DocumentDB</span><span class="sxs-lookup"><span data-stu-id="f64f5-189">query</span></span> |<span data-ttu-id="f64f5-190">Använd anpassad fråga för att läsa data.</span><span class="sxs-lookup"><span data-stu-id="f64f5-190">Use the custom query to read data.</span></span> |<span data-ttu-id="f64f5-191">SQL-92 frågan eller CQL frågan.</span><span class="sxs-lookup"><span data-stu-id="f64f5-191">SQL-92 query or CQL query.</span></span> <span data-ttu-id="f64f5-192">Se [CQL referens](https://docs.datastax.com/en/cql/3.1/cql/cql_reference/cqlReferenceTOC.html).</span><span class="sxs-lookup"><span data-stu-id="f64f5-192">See [CQL reference](https://docs.datastax.com/en/cql/3.1/cql/cql_reference/cqlReferenceTOC.html).</span></span> <br/><br/><span data-ttu-id="f64f5-193">När du använder SQL-frågan anger **keyspace name.table namn** som representerar den tabell som du vill fråga.</span><span class="sxs-lookup"><span data-stu-id="f64f5-193">When using SQL query, specify **keyspace name.table name** to represent the table you want to query.</span></span> |<span data-ttu-id="f64f5-194">Nej (om tabellnamn och keyspace för datauppsättningen har definierats).</span><span class="sxs-lookup"><span data-stu-id="f64f5-194">No (if tableName and keyspace on dataset are defined).</span></span> |
| <span data-ttu-id="f64f5-195">consistencyLevel</span><span class="sxs-lookup"><span data-stu-id="f64f5-195">consistencyLevel</span></span> |<span data-ttu-id="f64f5-196">Nivå för konsekvenskontroll anger hur många repliker måste svara på en läsbegäran innan den returnerar data till klientprogrammet.</span><span class="sxs-lookup"><span data-stu-id="f64f5-196">The consistency level specifies how many replicas must respond to a read request before returning data to the client application.</span></span> <span data-ttu-id="f64f5-197">Cassandra kontrollerar det angivna antalet repliker för data att uppfylla read-förfrågan.</span><span class="sxs-lookup"><span data-stu-id="f64f5-197">Cassandra checks the specified number of replicas for data to satisfy the read request.</span></span> |<span data-ttu-id="f64f5-198">EN, TVÅ, TRE, KVORUM, ALL, LOCAL_QUORUM EACH_QUORUM, LOCAL_ONE.</span><span class="sxs-lookup"><span data-stu-id="f64f5-198">ONE, TWO, THREE, QUORUM, ALL, LOCAL_QUORUM, EACH_QUORUM, LOCAL_ONE.</span></span> <span data-ttu-id="f64f5-199">Se [konfigurera datakonsekvens](http://docs.datastax.com/en//cassandra/2.0/cassandra/dml/dml_config_consistency_c.html) mer information.</span><span class="sxs-lookup"><span data-stu-id="f64f5-199">See [Configuring data consistency](http://docs.datastax.com/en//cassandra/2.0/cassandra/dml/dml_config_consistency_c.html) for details.</span></span> |<span data-ttu-id="f64f5-200">Nej.</span><span class="sxs-lookup"><span data-stu-id="f64f5-200">No.</span></span> <span data-ttu-id="f64f5-201">Standardvärdet är en.</span><span class="sxs-lookup"><span data-stu-id="f64f5-201">Default value is ONE.</span></span> |

## <a name="json-example-copy-data-from-cassandra-to-azure-blob"></a><span data-ttu-id="f64f5-202">JSON-exempel: kopiera data från Cassandra till Azure-Blob</span><span class="sxs-lookup"><span data-stu-id="f64f5-202">JSON example: Copy data from Cassandra to Azure Blob</span></span>
<span data-ttu-id="f64f5-203">Det här exemplet innehåller exempel JSON definitioner som du kan använda för att skapa en pipeline med [Azure-portalen](data-factory-copy-activity-tutorial-using-azure-portal.md) eller [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md) eller [Azure PowerShell](data-factory-copy-activity-tutorial-using-powershell.md).</span><span class="sxs-lookup"><span data-stu-id="f64f5-203">This example provides sample JSON definitions that you can use to create a pipeline by using [Azure portal](data-factory-copy-activity-tutorial-using-azure-portal.md) or [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md) or [Azure PowerShell](data-factory-copy-activity-tutorial-using-powershell.md).</span></span> <span data-ttu-id="f64f5-204">Den visar hur du kopierar data från en lokal Cassandra databas till en Azure Blob Storage.</span><span class="sxs-lookup"><span data-stu-id="f64f5-204">It shows how to copy data from an on-premises Cassandra database to an Azure Blob Storage.</span></span> <span data-ttu-id="f64f5-205">Dock datan kan kopieras till någon av sänkor anges [här](data-factory-data-movement-activities.md#supported-data-stores-and-formats) med hjälp av aktiviteten kopiera i Azure Data Factory.</span><span class="sxs-lookup"><span data-stu-id="f64f5-205">However, data can be copied to any of the sinks stated [here](data-factory-data-movement-activities.md#supported-data-stores-and-formats) using the Copy Activity in Azure Data Factory.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="f64f5-206">Det här exemplet innehåller JSON kodavsnitt.</span><span class="sxs-lookup"><span data-stu-id="f64f5-206">This sample provides JSON snippets.</span></span> <span data-ttu-id="f64f5-207">Stegvisa instruktioner för att skapa datafabriken inkluderas inte.</span><span class="sxs-lookup"><span data-stu-id="f64f5-207">It does not include step-by-step instructions for creating the data factory.</span></span> <span data-ttu-id="f64f5-208">Se [flytta data mellan lokala platser och moln](data-factory-move-data-between-onprem-and-cloud.md) artikel stegvisa instruktioner.</span><span class="sxs-lookup"><span data-stu-id="f64f5-208">See [moving data between on-premises locations and cloud](data-factory-move-data-between-onprem-and-cloud.md) article for step-by-step instructions.</span></span>

<span data-ttu-id="f64f5-209">Exemplet har följande data factory enheter:</span><span class="sxs-lookup"><span data-stu-id="f64f5-209">The sample has the following data factory entities:</span></span>

* <span data-ttu-id="f64f5-210">En länkad tjänst av typen [OnPremisesCassandra](#linked-service-properties).</span><span class="sxs-lookup"><span data-stu-id="f64f5-210">A linked service of type [OnPremisesCassandra](#linked-service-properties).</span></span>
* <span data-ttu-id="f64f5-211">En länkad tjänst av typen [AzureStorage](data-factory-azure-blob-connector.md#linked-service-properties).</span><span class="sxs-lookup"><span data-stu-id="f64f5-211">A linked service of type [AzureStorage](data-factory-azure-blob-connector.md#linked-service-properties).</span></span>
* <span data-ttu-id="f64f5-212">Indata [dataset](data-factory-create-datasets.md) av typen [CassandraTable](#dataset-properties).</span><span class="sxs-lookup"><span data-stu-id="f64f5-212">An input [dataset](data-factory-create-datasets.md) of type [CassandraTable](#dataset-properties).</span></span>
* <span data-ttu-id="f64f5-213">Utdata [dataset](data-factory-create-datasets.md) av typen [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties).</span><span class="sxs-lookup"><span data-stu-id="f64f5-213">An output [dataset](data-factory-create-datasets.md) of type [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties).</span></span>
* <span data-ttu-id="f64f5-214">En [pipeline](data-factory-create-pipelines.md) med Kopieringsaktiviteten som använder [CassandraSource](#copy-activity-properties) och [BlobSink](data-factory-azure-blob-connector.md#copy-activity-properties).</span><span class="sxs-lookup"><span data-stu-id="f64f5-214">A [pipeline](data-factory-create-pipelines.md) with Copy Activity that uses [CassandraSource](#copy-activity-properties) and [BlobSink](data-factory-azure-blob-connector.md#copy-activity-properties).</span></span>

<span data-ttu-id="f64f5-215">**Cassandra länkade tjänsten:**</span><span class="sxs-lookup"><span data-stu-id="f64f5-215">**Cassandra linked service:**</span></span>

<span data-ttu-id="f64f5-216">Det här exemplet används den **Cassandra** länkade tjänsten.</span><span class="sxs-lookup"><span data-stu-id="f64f5-216">This example uses the **Cassandra** linked service.</span></span> <span data-ttu-id="f64f5-217">Se [Cassandra länkade tjänsten](#linked-service-properties) avsnittet för egenskaper som stöds av den här länkade tjänsten.</span><span class="sxs-lookup"><span data-stu-id="f64f5-217">See [Cassandra linked service](#linked-service-properties) section for the properties supported by this linked service.</span></span>  

```json
{
    "name": "CassandraLinkedService",
    "properties":
    {
        "type": "OnPremisesCassandra",
        "typeProperties":
        {
            "authenticationType": "Basic",
            "host": "mycassandraserver",
            "port": 9042,
            "username": "user",
            "password": "password",
            "gatewayName": "mygateway"
        }
    }
}
```

<span data-ttu-id="f64f5-218">**Azure Storage länkade tjänsten:**</span><span class="sxs-lookup"><span data-stu-id="f64f5-218">**Azure Storage linked service:**</span></span>

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

<span data-ttu-id="f64f5-219">**Cassandra inkommande datauppsättningen:**</span><span class="sxs-lookup"><span data-stu-id="f64f5-219">**Cassandra input dataset:**</span></span>

```json
{
    "name": "CassandraInput",
    "properties": {
        "linkedServiceName": "CassandraLinkedService",
        "type": "CassandraTable",
        "typeProperties": {
            "tableName": "mytable",
            "keySpace": "mykeyspace"
        },
        "availability": {
            "frequency": "Hour",
            "interval": 1
        },
        "external": true,
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

<span data-ttu-id="f64f5-220">Ange **externa** till **SANT** informerar Data Factory-tjänsten att datamängden är extern till data factory och inte tillverkas av en aktivitet i datafabriken.</span><span class="sxs-lookup"><span data-stu-id="f64f5-220">Setting **external** to **true** informs the Data Factory service that the dataset is external to the data factory and is not produced by an activity in the data factory.</span></span>

<span data-ttu-id="f64f5-221">**Azure Blob utdatauppsättningen:**</span><span class="sxs-lookup"><span data-stu-id="f64f5-221">**Azure Blob output dataset:**</span></span>

<span data-ttu-id="f64f5-222">Data skrivs till en ny blob varje timme (frekvens: timme, intervall: 1).</span><span class="sxs-lookup"><span data-stu-id="f64f5-222">Data is written to a new blob every hour (frequency: hour, interval: 1).</span></span>

```json
{
    "name": "AzureBlobOutput",
    "properties":
    {
        "type": "AzureBlob",
        "linkedServiceName": "AzureStorageLinkedService",
        "typeProperties":
        {
            "folderPath": "adfgetstarted/fromcassandra"
        },
        "availability":
        {
            "frequency": "Hour",
            "interval": 1
        }
    }
}
```

<span data-ttu-id="f64f5-223">**Kopiera aktivitet i en pipeline med Cassandra källa och mottagare för Blob:**</span><span class="sxs-lookup"><span data-stu-id="f64f5-223">**Copy activity in a pipeline with Cassandra source and Blob sink:**</span></span>

<span data-ttu-id="f64f5-224">Pipelinen innehåller en kopia-aktivitet som är konfigurerad för att använda indata och utdata-datauppsättningar och är schemalagd att köras varje timme.</span><span class="sxs-lookup"><span data-stu-id="f64f5-224">The pipeline contains a Copy Activity that is configured to use the input and output datasets and is scheduled to run every hour.</span></span> <span data-ttu-id="f64f5-225">I pipeline-JSON-definitionen av **källa** är inställd på **CassandraSource** och **sink** är inställd på **BlobSink**.</span><span class="sxs-lookup"><span data-stu-id="f64f5-225">In the pipeline JSON definition, the **source** type is set to **CassandraSource** and **sink** type is set to **BlobSink**.</span></span>

<span data-ttu-id="f64f5-226">Se [RelationalSource Typegenskaper](#copy-activity-properties) lista över egenskaper som stöds av RelationalSource.</span><span class="sxs-lookup"><span data-stu-id="f64f5-226">See [RelationalSource type properties](#copy-activity-properties) for the list of properties supported by the RelationalSource.</span></span>

```json
{  
    "name":"SamplePipeline",
    "properties":{  
        "start":"2016-06-01T18:00:00",
        "end":"2016-06-01T19:00:00",
        "description":"pipeline with copy activity",
        "activities":[  
        {
            "name": "CassandraToAzureBlob",
            "description": "Copy from Cassandra to an Azure blob",
            "type": "Copy",
            "inputs": [
            {
                "name": "CassandraInput"
            }
            ],
            "outputs": [
            {
                "name": "AzureBlobOutput"
            }
            ],
            "typeProperties": {
                "source": {
                    "type": "CassandraSource",
                    "query": "select id, firstname, lastname from mykeyspace.mytable"

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

### <a name="type-mapping-for-cassandra"></a><span data-ttu-id="f64f5-227">Mappning för Cassandra</span><span class="sxs-lookup"><span data-stu-id="f64f5-227">Type mapping for Cassandra</span></span>
| <span data-ttu-id="f64f5-228">Cassandra typ</span><span class="sxs-lookup"><span data-stu-id="f64f5-228">Cassandra Type</span></span> | <span data-ttu-id="f64f5-229">.NET baserat typ</span><span class="sxs-lookup"><span data-stu-id="f64f5-229">.Net Based Type</span></span> |
| --- | --- |
| <span data-ttu-id="f64f5-230">ASCII</span><span class="sxs-lookup"><span data-stu-id="f64f5-230">ASCII</span></span> |<span data-ttu-id="f64f5-231">Sträng</span><span class="sxs-lookup"><span data-stu-id="f64f5-231">String</span></span> |
| <span data-ttu-id="f64f5-232">BIGINT</span><span class="sxs-lookup"><span data-stu-id="f64f5-232">BIGINT</span></span> |<span data-ttu-id="f64f5-233">Int64</span><span class="sxs-lookup"><span data-stu-id="f64f5-233">Int64</span></span> |
| <span data-ttu-id="f64f5-234">BLOB</span><span class="sxs-lookup"><span data-stu-id="f64f5-234">BLOB</span></span> |<span data-ttu-id="f64f5-235">byte]</span><span class="sxs-lookup"><span data-stu-id="f64f5-235">Byte[]</span></span> |
| <span data-ttu-id="f64f5-236">BOOLESKT VÄRDE</span><span class="sxs-lookup"><span data-stu-id="f64f5-236">BOOLEAN</span></span> |<span data-ttu-id="f64f5-237">Booleskt värde</span><span class="sxs-lookup"><span data-stu-id="f64f5-237">Boolean</span></span> |
| <span data-ttu-id="f64f5-238">DECIMAL</span><span class="sxs-lookup"><span data-stu-id="f64f5-238">DECIMAL</span></span> |<span data-ttu-id="f64f5-239">Decimal</span><span class="sxs-lookup"><span data-stu-id="f64f5-239">Decimal</span></span> |
| <span data-ttu-id="f64f5-240">DUBBEL</span><span class="sxs-lookup"><span data-stu-id="f64f5-240">DOUBLE</span></span> |<span data-ttu-id="f64f5-241">dubbla</span><span class="sxs-lookup"><span data-stu-id="f64f5-241">Double</span></span> |
| <span data-ttu-id="f64f5-242">FLYTTAL</span><span class="sxs-lookup"><span data-stu-id="f64f5-242">FLOAT</span></span> |<span data-ttu-id="f64f5-243">Enskild</span><span class="sxs-lookup"><span data-stu-id="f64f5-243">Single</span></span> |
| <span data-ttu-id="f64f5-244">INET</span><span class="sxs-lookup"><span data-stu-id="f64f5-244">INET</span></span> |<span data-ttu-id="f64f5-245">Sträng</span><span class="sxs-lookup"><span data-stu-id="f64f5-245">String</span></span> |
| <span data-ttu-id="f64f5-246">INT</span><span class="sxs-lookup"><span data-stu-id="f64f5-246">INT</span></span> |<span data-ttu-id="f64f5-247">Int32</span><span class="sxs-lookup"><span data-stu-id="f64f5-247">Int32</span></span> |
| <span data-ttu-id="f64f5-248">TEXT</span><span class="sxs-lookup"><span data-stu-id="f64f5-248">TEXT</span></span> |<span data-ttu-id="f64f5-249">Sträng</span><span class="sxs-lookup"><span data-stu-id="f64f5-249">String</span></span> |
| <span data-ttu-id="f64f5-250">TIDSSTÄMPEL</span><span class="sxs-lookup"><span data-stu-id="f64f5-250">TIMESTAMP</span></span> |<span data-ttu-id="f64f5-251">Datum och tid</span><span class="sxs-lookup"><span data-stu-id="f64f5-251">DateTime</span></span> |
| <span data-ttu-id="f64f5-252">TIMEUUID</span><span class="sxs-lookup"><span data-stu-id="f64f5-252">TIMEUUID</span></span> |<span data-ttu-id="f64f5-253">GUID</span><span class="sxs-lookup"><span data-stu-id="f64f5-253">Guid</span></span> |
| <span data-ttu-id="f64f5-254">UUID</span><span class="sxs-lookup"><span data-stu-id="f64f5-254">UUID</span></span> |<span data-ttu-id="f64f5-255">GUID</span><span class="sxs-lookup"><span data-stu-id="f64f5-255">Guid</span></span> |
| <span data-ttu-id="f64f5-256">VARCHAR</span><span class="sxs-lookup"><span data-stu-id="f64f5-256">VARCHAR</span></span> |<span data-ttu-id="f64f5-257">Sträng</span><span class="sxs-lookup"><span data-stu-id="f64f5-257">String</span></span> |
| <span data-ttu-id="f64f5-258">VARINT</span><span class="sxs-lookup"><span data-stu-id="f64f5-258">VARINT</span></span> |<span data-ttu-id="f64f5-259">Decimal</span><span class="sxs-lookup"><span data-stu-id="f64f5-259">Decimal</span></span> |

> [!NOTE]
> <span data-ttu-id="f64f5-260">Insamling av typer (karta, set, lista, etc.), referera till [arbeta med Cassandra samlingstyper med virtuella tabellen](#work-with-collections-using-virtual-table) avsnitt.</span><span class="sxs-lookup"><span data-stu-id="f64f5-260">For collection types (map, set, list, etc.), refer to [Work with Cassandra collection types using virtual table](#work-with-collections-using-virtual-table) section.</span></span>
>
> <span data-ttu-id="f64f5-261">Användardefinierade typer stöds inte.</span><span class="sxs-lookup"><span data-stu-id="f64f5-261">User-defined types are not supported.</span></span>
>
> <span data-ttu-id="f64f5-262">Längden på Binary-kolumn och strängkolumn längd får inte vara större än 4000.</span><span class="sxs-lookup"><span data-stu-id="f64f5-262">The length of Binary Column and String Column lengths cannot be greater than 4000.</span></span>
>
>

## <a name="work-with-collections-using-virtual-table"></a><span data-ttu-id="f64f5-263">Arbeta med samlingar med virtuella tabellen</span><span class="sxs-lookup"><span data-stu-id="f64f5-263">Work with collections using virtual table</span></span>
<span data-ttu-id="f64f5-264">Azure Data Factory använder en inbyggd ODBC-drivrutin för att ansluta till och kopiera data från databasen Cassandra.</span><span class="sxs-lookup"><span data-stu-id="f64f5-264">Azure Data Factory uses a built-in ODBC driver to connect to and copy data from your Cassandra database.</span></span> <span data-ttu-id="f64f5-265">För samlingstyper inklusive karta, uppsättning och lista, renormalizes drivrutinen data i motsvarande virtuella tabeller.</span><span class="sxs-lookup"><span data-stu-id="f64f5-265">For collection types including map, set and list, the driver renormalizes the data into corresponding virtual tables.</span></span> <span data-ttu-id="f64f5-266">Om en tabell innehåller några kolumner i samlingen, genererar drivrutinen mer specifikt kan följande virtuella tabeller:</span><span class="sxs-lookup"><span data-stu-id="f64f5-266">Specifically, if a table contains any collection columns, the driver generates the following virtual tables:</span></span>

* <span data-ttu-id="f64f5-267">En **bastabellen**, som innehåller samma data som verkliga tabell utom samling kolumner.</span><span class="sxs-lookup"><span data-stu-id="f64f5-267">A **base table**, which contains the same data as the real table except for the collection columns.</span></span> <span data-ttu-id="f64f5-268">Bastabellen använder samma namn som den verkliga tabell som representerar.</span><span class="sxs-lookup"><span data-stu-id="f64f5-268">The base table uses the same name as the real table that it represents.</span></span>
* <span data-ttu-id="f64f5-269">En **virtuella tabellen** för varje samling-kolumn som utökar kapslade data.</span><span class="sxs-lookup"><span data-stu-id="f64f5-269">A **virtual table** for each collection column, which expands the nested data.</span></span> <span data-ttu-id="f64f5-270">Virtuella tabeller som representerar samlingar namnges med namnet på tabellen verkliga avgränsare ”*vt*” och namnet på kolumnen.</span><span class="sxs-lookup"><span data-stu-id="f64f5-270">The virtual tables that represent collections are named using the name of the real table, a separator “*vt*” and the name of the column.</span></span>

<span data-ttu-id="f64f5-271">Virtuella tabellerna hänvisar till data i tabellen verkliga aktiverar drivrutinen att komma åt Avnormaliserade data.</span><span class="sxs-lookup"><span data-stu-id="f64f5-271">Virtual tables refer to the data in the real table, enabling the driver to access the denormalized data.</span></span> <span data-ttu-id="f64f5-272">Se avsnittet för information.</span><span class="sxs-lookup"><span data-stu-id="f64f5-272">See Example section for details.</span></span> <span data-ttu-id="f64f5-273">Du kan komma åt innehållet i Cassandra samlingar genom att fråga och ansluta till virtuella tabeller.</span><span class="sxs-lookup"><span data-stu-id="f64f5-273">You can access the content of Cassandra collections by querying and joining the virtual tables.</span></span>

<span data-ttu-id="f64f5-274">Du kan använda den [guiden Kopiera](data-factory-data-movement-activities.md#create-a-pipeline-with-copy-activity) intuitivt visa listan över tabeller i Cassandra databasen, inklusive virtuella tabeller och förhandsgranska data som ingår.</span><span class="sxs-lookup"><span data-stu-id="f64f5-274">You can use the [Copy Wizard](data-factory-data-movement-activities.md#create-a-pipeline-with-copy-activity) to intuitively view the list of tables in Cassandra database including the virtual tables, and preview the data inside.</span></span> <span data-ttu-id="f64f5-275">Du kan också skapa en fråga i guiden Kopiera och validera om du vill se resultatet.</span><span class="sxs-lookup"><span data-stu-id="f64f5-275">You can also construct a query in the Copy Wizard and validate to see the result.</span></span>

### <a name="example"></a><span data-ttu-id="f64f5-276">Exempel</span><span class="sxs-lookup"><span data-stu-id="f64f5-276">Example</span></span>
<span data-ttu-id="f64f5-277">Till exempel är följande ”ExampleTable” en Cassandra databastabell som innehåller ett heltal primärnyckelkolumnen med namnet ”pk_int”, en textkolumn namngivet värde, en kolumn, en karta kolumn och en set-kolumn (med namnet ”StringSet”).</span><span class="sxs-lookup"><span data-stu-id="f64f5-277">For example, the following “ExampleTable” is a Cassandra database table that contains an integer primary key column named “pk_int”, a text column named value, a list column, a map column, and a set column (named “StringSet”).</span></span>

| <span data-ttu-id="f64f5-278">pk_int</span><span class="sxs-lookup"><span data-stu-id="f64f5-278">pk_int</span></span> | <span data-ttu-id="f64f5-279">Värde</span><span class="sxs-lookup"><span data-stu-id="f64f5-279">Value</span></span> | <span data-ttu-id="f64f5-280">Visa lista</span><span class="sxs-lookup"><span data-stu-id="f64f5-280">List</span></span> | <span data-ttu-id="f64f5-281">karta</span><span class="sxs-lookup"><span data-stu-id="f64f5-281">Map</span></span> | <span data-ttu-id="f64f5-282">StringSet</span><span class="sxs-lookup"><span data-stu-id="f64f5-282">StringSet</span></span> |
| --- | --- | --- | --- | --- |
| <span data-ttu-id="f64f5-283">1</span><span class="sxs-lookup"><span data-stu-id="f64f5-283">1</span></span> |<span data-ttu-id="f64f5-284">”exempelvärde 1”</span><span class="sxs-lookup"><span data-stu-id="f64f5-284">"sample value 1"</span></span> |<span data-ttu-id="f64f5-285">["1", "2", "3"]</span><span class="sxs-lookup"><span data-stu-id="f64f5-285">["1", "2", "3"]</span></span> |<span data-ttu-id="f64f5-286">{”S1”: ”a”, ”S2”: ”b”}</span><span class="sxs-lookup"><span data-stu-id="f64f5-286">{"S1": "a", "S2": "b"}</span></span> |<span data-ttu-id="f64f5-287">{”A”, ”B”, ”C”}</span><span class="sxs-lookup"><span data-stu-id="f64f5-287">{"A", "B", "C"}</span></span> |
| <span data-ttu-id="f64f5-288">3</span><span class="sxs-lookup"><span data-stu-id="f64f5-288">3</span></span> |<span data-ttu-id="f64f5-289">”exempelvärde 3”</span><span class="sxs-lookup"><span data-stu-id="f64f5-289">"sample value 3"</span></span> |<span data-ttu-id="f64f5-290">["100", "101", "102", "105"]</span><span class="sxs-lookup"><span data-stu-id="f64f5-290">["100", "101", "102", "105"]</span></span> |<span data-ttu-id="f64f5-291">{”S1”: ”t”}</span><span class="sxs-lookup"><span data-stu-id="f64f5-291">{"S1": "t"}</span></span> |<span data-ttu-id="f64f5-292">{”A”, ”E”}</span><span class="sxs-lookup"><span data-stu-id="f64f5-292">{"A", "E"}</span></span> |

<span data-ttu-id="f64f5-293">Drivrutinen skulle generera flera virtuella tabeller som representerar denna tabell.</span><span class="sxs-lookup"><span data-stu-id="f64f5-293">The driver would generate multiple virtual tables to represent this single table.</span></span> <span data-ttu-id="f64f5-294">Sekundärnyckelskolumnerna i virtuella tabeller refererar till primärnyckelkolumnerna i tabellen verkliga och ange vilka verkliga tabellrad som motsvarar den virtuella tabellraden.</span><span class="sxs-lookup"><span data-stu-id="f64f5-294">The foreign key columns in the virtual tables reference the primary key columns in the real table, and indicate which real table row the virtual table row corresponds to.</span></span>

<span data-ttu-id="f64f5-295">Den första virtuella tabellen är bastabellen med namnet ”ExampleTable” visas i följande tabell.</span><span class="sxs-lookup"><span data-stu-id="f64f5-295">The first virtual table is the base table named “ExampleTable” is shown in the following table.</span></span> <span data-ttu-id="f64f5-296">Bastabellen innehåller samma data som den ursprungliga databastabellen förutom de samlingar som utelämnas från den här tabellen och expanderas i andra virtuella tabeller.</span><span class="sxs-lookup"><span data-stu-id="f64f5-296">The base table contains the same data as the original database table except for the collections, which are omitted from this table and expanded in other virtual tables.</span></span>

| <span data-ttu-id="f64f5-297">pk_int</span><span class="sxs-lookup"><span data-stu-id="f64f5-297">pk_int</span></span> | <span data-ttu-id="f64f5-298">Värde</span><span class="sxs-lookup"><span data-stu-id="f64f5-298">Value</span></span> |
| --- | --- |
| <span data-ttu-id="f64f5-299">1</span><span class="sxs-lookup"><span data-stu-id="f64f5-299">1</span></span> |<span data-ttu-id="f64f5-300">”exempelvärde 1”</span><span class="sxs-lookup"><span data-stu-id="f64f5-300">"sample value 1"</span></span> |
| <span data-ttu-id="f64f5-301">3</span><span class="sxs-lookup"><span data-stu-id="f64f5-301">3</span></span> |<span data-ttu-id="f64f5-302">”exempelvärde 3”</span><span class="sxs-lookup"><span data-stu-id="f64f5-302">"sample value 3"</span></span> |

<span data-ttu-id="f64f5-303">Följande tabeller visar virtuella register som renormalize data från listan och mappa StringSet kolumner.</span><span class="sxs-lookup"><span data-stu-id="f64f5-303">The following tables show the virtual tables that renormalize the data from the List, Map, and StringSet columns.</span></span> <span data-ttu-id="f64f5-304">Kolumner med namn som slutar med ”_index” eller ”_nyckelskyddsserverns” anger placeringen av data i den ursprungliga listan eller kartan.</span><span class="sxs-lookup"><span data-stu-id="f64f5-304">The columns with names that end with “_index” or “_key” indicate the position of the data within the original list or map.</span></span> <span data-ttu-id="f64f5-305">Kolumnerna med namn som slutar med ”_value” innehåller utökade data från samlingen.</span><span class="sxs-lookup"><span data-stu-id="f64f5-305">The columns with names that end with “_value” contain the expanded data from the collection.</span></span>

#### <a name="table-exampletablevtlist"></a><span data-ttu-id="f64f5-306">Tabell ”ExampleTable_vt_List”:</span><span class="sxs-lookup"><span data-stu-id="f64f5-306">Table “ExampleTable_vt_List”:</span></span>
| <span data-ttu-id="f64f5-307">pk_int</span><span class="sxs-lookup"><span data-stu-id="f64f5-307">pk_int</span></span> | <span data-ttu-id="f64f5-308">List_index</span><span class="sxs-lookup"><span data-stu-id="f64f5-308">List_index</span></span> | <span data-ttu-id="f64f5-309">List_value</span><span class="sxs-lookup"><span data-stu-id="f64f5-309">List_value</span></span> |
| --- | --- | --- |
| <span data-ttu-id="f64f5-310">1</span><span class="sxs-lookup"><span data-stu-id="f64f5-310">1</span></span> |<span data-ttu-id="f64f5-311">0</span><span class="sxs-lookup"><span data-stu-id="f64f5-311">0</span></span> |<span data-ttu-id="f64f5-312">1</span><span class="sxs-lookup"><span data-stu-id="f64f5-312">1</span></span> |
| <span data-ttu-id="f64f5-313">1</span><span class="sxs-lookup"><span data-stu-id="f64f5-313">1</span></span> |<span data-ttu-id="f64f5-314">1</span><span class="sxs-lookup"><span data-stu-id="f64f5-314">1</span></span> |<span data-ttu-id="f64f5-315">2</span><span class="sxs-lookup"><span data-stu-id="f64f5-315">2</span></span> |
| <span data-ttu-id="f64f5-316">1</span><span class="sxs-lookup"><span data-stu-id="f64f5-316">1</span></span> |<span data-ttu-id="f64f5-317">2</span><span class="sxs-lookup"><span data-stu-id="f64f5-317">2</span></span> |<span data-ttu-id="f64f5-318">3</span><span class="sxs-lookup"><span data-stu-id="f64f5-318">3</span></span> |
| <span data-ttu-id="f64f5-319">3</span><span class="sxs-lookup"><span data-stu-id="f64f5-319">3</span></span> |<span data-ttu-id="f64f5-320">0</span><span class="sxs-lookup"><span data-stu-id="f64f5-320">0</span></span> |<span data-ttu-id="f64f5-321">100</span><span class="sxs-lookup"><span data-stu-id="f64f5-321">100</span></span> |
| <span data-ttu-id="f64f5-322">3</span><span class="sxs-lookup"><span data-stu-id="f64f5-322">3</span></span> |<span data-ttu-id="f64f5-323">1</span><span class="sxs-lookup"><span data-stu-id="f64f5-323">1</span></span> |<span data-ttu-id="f64f5-324">101</span><span class="sxs-lookup"><span data-stu-id="f64f5-324">101</span></span> |
| <span data-ttu-id="f64f5-325">3</span><span class="sxs-lookup"><span data-stu-id="f64f5-325">3</span></span> |<span data-ttu-id="f64f5-326">2</span><span class="sxs-lookup"><span data-stu-id="f64f5-326">2</span></span> |<span data-ttu-id="f64f5-327">102</span><span class="sxs-lookup"><span data-stu-id="f64f5-327">102</span></span> |
| <span data-ttu-id="f64f5-328">3</span><span class="sxs-lookup"><span data-stu-id="f64f5-328">3</span></span> |<span data-ttu-id="f64f5-329">3</span><span class="sxs-lookup"><span data-stu-id="f64f5-329">3</span></span> |<span data-ttu-id="f64f5-330">103</span><span class="sxs-lookup"><span data-stu-id="f64f5-330">103</span></span> |

#### <a name="table-exampletablevtmap"></a><span data-ttu-id="f64f5-331">Tabell ”ExampleTable_vt_Map”:</span><span class="sxs-lookup"><span data-stu-id="f64f5-331">Table “ExampleTable_vt_Map”:</span></span>
| <span data-ttu-id="f64f5-332">pk_int</span><span class="sxs-lookup"><span data-stu-id="f64f5-332">pk_int</span></span> | <span data-ttu-id="f64f5-333">Map_key</span><span class="sxs-lookup"><span data-stu-id="f64f5-333">Map_key</span></span> | <span data-ttu-id="f64f5-334">Map_value</span><span class="sxs-lookup"><span data-stu-id="f64f5-334">Map_value</span></span> |
| --- | --- | --- |
| <span data-ttu-id="f64f5-335">1</span><span class="sxs-lookup"><span data-stu-id="f64f5-335">1</span></span> |<span data-ttu-id="f64f5-336">S1</span><span class="sxs-lookup"><span data-stu-id="f64f5-336">S1</span></span> |<span data-ttu-id="f64f5-337">A</span><span class="sxs-lookup"><span data-stu-id="f64f5-337">A</span></span> |
| <span data-ttu-id="f64f5-338">1</span><span class="sxs-lookup"><span data-stu-id="f64f5-338">1</span></span> |<span data-ttu-id="f64f5-339">S2</span><span class="sxs-lookup"><span data-stu-id="f64f5-339">S2</span></span> |<span data-ttu-id="f64f5-340">B</span><span class="sxs-lookup"><span data-stu-id="f64f5-340">b</span></span> |
| <span data-ttu-id="f64f5-341">3</span><span class="sxs-lookup"><span data-stu-id="f64f5-341">3</span></span> |<span data-ttu-id="f64f5-342">S1</span><span class="sxs-lookup"><span data-stu-id="f64f5-342">S1</span></span> |<span data-ttu-id="f64f5-343">T</span><span class="sxs-lookup"><span data-stu-id="f64f5-343">t</span></span> |

#### <a name="table-exampletablevtstringset"></a><span data-ttu-id="f64f5-344">Tabell ”ExampleTable_vt_StringSet”:</span><span class="sxs-lookup"><span data-stu-id="f64f5-344">Table “ExampleTable_vt_StringSet”:</span></span>
| <span data-ttu-id="f64f5-345">pk_int</span><span class="sxs-lookup"><span data-stu-id="f64f5-345">pk_int</span></span> | <span data-ttu-id="f64f5-346">StringSet_value</span><span class="sxs-lookup"><span data-stu-id="f64f5-346">StringSet_value</span></span> |
| --- | --- |
| <span data-ttu-id="f64f5-347">1</span><span class="sxs-lookup"><span data-stu-id="f64f5-347">1</span></span> |<span data-ttu-id="f64f5-348">A</span><span class="sxs-lookup"><span data-stu-id="f64f5-348">A</span></span> |
| <span data-ttu-id="f64f5-349">1</span><span class="sxs-lookup"><span data-stu-id="f64f5-349">1</span></span> |<span data-ttu-id="f64f5-350">B</span><span class="sxs-lookup"><span data-stu-id="f64f5-350">B</span></span> |
| <span data-ttu-id="f64f5-351">1</span><span class="sxs-lookup"><span data-stu-id="f64f5-351">1</span></span> |<span data-ttu-id="f64f5-352">C</span><span class="sxs-lookup"><span data-stu-id="f64f5-352">C</span></span> |
| <span data-ttu-id="f64f5-353">3</span><span class="sxs-lookup"><span data-stu-id="f64f5-353">3</span></span> |<span data-ttu-id="f64f5-354">A</span><span class="sxs-lookup"><span data-stu-id="f64f5-354">A</span></span> |
| <span data-ttu-id="f64f5-355">3</span><span class="sxs-lookup"><span data-stu-id="f64f5-355">3</span></span> |<span data-ttu-id="f64f5-356">E</span><span class="sxs-lookup"><span data-stu-id="f64f5-356">E</span></span> |

## <a name="map-source-to-sink-columns"></a><span data-ttu-id="f64f5-357">Karta källan till mottagare för kolumner</span><span class="sxs-lookup"><span data-stu-id="f64f5-357">Map source to sink columns</span></span>
<span data-ttu-id="f64f5-358">Mer information om mappning kolumner i datauppsättningen källan till kolumner i datauppsättning mottagare, se [mappa dataset kolumner i Azure Data Factory](data-factory-map-columns.md).</span><span class="sxs-lookup"><span data-stu-id="f64f5-358">To learn about mapping columns in source dataset to columns in sink dataset, see [Mapping dataset columns in Azure Data Factory](data-factory-map-columns.md).</span></span>

## <a name="repeatable-read-from-relational-sources"></a><span data-ttu-id="f64f5-359">Upprepbar läsning från relationella källor</span><span class="sxs-lookup"><span data-stu-id="f64f5-359">Repeatable read from relational sources</span></span>
<span data-ttu-id="f64f5-360">Tänk på att undvika oväntade resultat repeterbarhet när kopiering av data från relationella data lagras.</span><span class="sxs-lookup"><span data-stu-id="f64f5-360">When copying data from relational data stores, keep repeatability in mind to avoid unintended outcomes.</span></span> <span data-ttu-id="f64f5-361">I Azure Data Factory, kan du köra en sektor manuellt.</span><span class="sxs-lookup"><span data-stu-id="f64f5-361">In Azure Data Factory, you can rerun a slice manually.</span></span> <span data-ttu-id="f64f5-362">Du kan också konfigurera i principen för en dataset så att ett segment som körs när ett fel uppstår.</span><span class="sxs-lookup"><span data-stu-id="f64f5-362">You can also configure retry policy for a dataset so that a slice is rerun when a failure occurs.</span></span> <span data-ttu-id="f64f5-363">När ett segment körs på något sätt, måste du kontrollera att samma data läses oavsett hur många gånger ett segment körs.</span><span class="sxs-lookup"><span data-stu-id="f64f5-363">When a slice is rerun in either way, you need to make sure that the same data is read no matter how many times a slice is run.</span></span> <span data-ttu-id="f64f5-364">Se [Repeatable läsa från relationella källor](data-factory-repeatable-copy.md#repeatable-read-from-relational-sources).</span><span class="sxs-lookup"><span data-stu-id="f64f5-364">See [Repeatable read from relational sources](data-factory-repeatable-copy.md#repeatable-read-from-relational-sources).</span></span>

## <a name="performance-and-tuning"></a><span data-ttu-id="f64f5-365">Prestanda och finjustering</span><span class="sxs-lookup"><span data-stu-id="f64f5-365">Performance and Tuning</span></span>
<span data-ttu-id="f64f5-366">Se [kopiera aktivitet prestanda och justera guiden](data-factory-copy-activity-performance.md) vill veta mer om viktiga faktorer som påverkan prestanda för flytt av data (Kopieringsaktiviteten) i Azure Data Factory och olika sätt att optimera den.</span><span class="sxs-lookup"><span data-stu-id="f64f5-366">See [Copy Activity Performance & Tuning Guide](data-factory-copy-activity-performance.md) to learn about key factors that impact performance of data movement (Copy Activity) in Azure Data Factory and various ways to optimize it.</span></span>
