---
title: "Flytta data från PostgreSQL med hjälp av Azure Data Factory | Microsoft Docs"
description: "Läs mer om hur du flyttar data från PostgreSQL-databas med hjälp av Azure Data Factory."
services: data-factory
documentationcenter: 
author: linda33wj
manager: jhubbard
editor: monicar
ms.assetid: 888d9ebc-2500-4071-b6d1-0f6bd1b5997c
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/11/2017
ms.author: jingwang
ms.openlocfilehash: fd26f0d03f8b0b352a6544a81ad952d2e2a1b7a0
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/03/2017
---
# <a name="move-data-from-postgresql-using-azure-data-factory"></a><span data-ttu-id="74b3e-103">Flytta data från PostgreSQL med hjälp av Azure Data Factory</span><span class="sxs-lookup"><span data-stu-id="74b3e-103">Move data from PostgreSQL using Azure Data Factory</span></span>
<span data-ttu-id="74b3e-104">Den här artikeln förklarar hur du använder aktiviteten kopiera i Azure Data Factory för att flytta data från en lokal PostgreSQL-databas.</span><span class="sxs-lookup"><span data-stu-id="74b3e-104">This article explains how to use the Copy Activity in Azure Data Factory to move data from an on-premises PostgreSQL database.</span></span> <span data-ttu-id="74b3e-105">Den bygger på den [Data Movement aktiviteter](data-factory-data-movement-activities.md) artikel som presenterar en allmän översikt över dataflyttning med copy-aktivitet.</span><span class="sxs-lookup"><span data-stu-id="74b3e-105">It builds on the [Data Movement Activities](data-factory-data-movement-activities.md) article, which presents a general overview of data movement with the copy activity.</span></span>

<span data-ttu-id="74b3e-106">Du kan kopiera data från ett dataarkiv för lokala PostgreSQL till alla stöds sink-datalagret.</span><span class="sxs-lookup"><span data-stu-id="74b3e-106">You can copy data from an on-premises PostgreSQL data store to any supported sink data store.</span></span> <span data-ttu-id="74b3e-107">En lista över datakällor som stöds som sänkor av kopieringsaktiviteten finns [stöds datalager](data-factory-data-movement-activities.md#supported-data-stores-and-formats).</span><span class="sxs-lookup"><span data-stu-id="74b3e-107">For a list of data stores supported as sinks by the copy activity, see [supported data stores](data-factory-data-movement-activities.md#supported-data-stores-and-formats).</span></span> <span data-ttu-id="74b3e-108">Data factory stöder för närvarande flytta data från en PostgreSQL-databas till andra databaser, men inte för att flytta data från andra datalager till en PostgreSQL-databas.</span><span class="sxs-lookup"><span data-stu-id="74b3e-108">Data factory currently supports moving data from a PostgreSQL database to other data stores, but not for moving data from other data stores to an PostgreSQL database.</span></span> 

## <a name="prerequisites"></a><span data-ttu-id="74b3e-109">Nödvändiga komponenter</span><span class="sxs-lookup"><span data-stu-id="74b3e-109">prerequisites</span></span>

<span data-ttu-id="74b3e-110">Data Factory-tjänsten stöder anslutning till lokala PostgreSQL källor med hjälp av Data Management Gateway.</span><span class="sxs-lookup"><span data-stu-id="74b3e-110">Data Factory service supports connecting to on-premises PostgreSQL sources using the Data Management Gateway.</span></span> <span data-ttu-id="74b3e-111">Se [flytta data mellan lokala platser och moln](data-factory-move-data-between-onprem-and-cloud.md) artikeln innehåller information om Data Management Gateway och stegvisa instruktioner om hur du konfigurerar en gateway.</span><span class="sxs-lookup"><span data-stu-id="74b3e-111">See [moving data between on-premises locations and cloud](data-factory-move-data-between-onprem-and-cloud.md) article to learn about Data Management Gateway and step-by-step instructions on setting up the gateway.</span></span>

<span data-ttu-id="74b3e-112">Gateway krävs även om PostgreSQL-databasen finns i en Azure IaaS-VM.</span><span class="sxs-lookup"><span data-stu-id="74b3e-112">Gateway is required even if the PostgreSQL database is hosted in an Azure IaaS VM.</span></span> <span data-ttu-id="74b3e-113">Du kan installera gatewayen på samma IaaS-VM som dataarkiv eller på en annan virtuell dator som en gateway kan ansluta till databasen.</span><span class="sxs-lookup"><span data-stu-id="74b3e-113">You can install gateway on the same IaaS VM as the data store or on a different VM as long as the gateway can connect to the database.</span></span>

> [!NOTE]
> <span data-ttu-id="74b3e-114">Se [felsökning av problem med gateway](data-factory-data-management-gateway.md#troubleshooting-gateway-issues) tips om hur du felsöker anslutning /-gateway relaterade problem.</span><span class="sxs-lookup"><span data-stu-id="74b3e-114">See [Troubleshoot gateway issues](data-factory-data-management-gateway.md#troubleshooting-gateway-issues) for tips on troubleshooting connection/gateway related issues.</span></span>

## <a name="supported-versions-and-installation"></a><span data-ttu-id="74b3e-115">Versioner som stöds och installation</span><span class="sxs-lookup"><span data-stu-id="74b3e-115">Supported versions and installation</span></span>
<span data-ttu-id="74b3e-116">Data Management Gateway att ansluta till PostgreSQL-databasen, installera den [Ngpsql dataprovider för PostgreSQL](http://go.microsoft.com/fwlink/?linkid=282716) 2.0.12 eller senare på samma system som Data Management Gateway.</span><span class="sxs-lookup"><span data-stu-id="74b3e-116">For Data Management Gateway to connect to the PostgreSQL Database, install the [Ngpsql data provider for PostgreSQL](http://go.microsoft.com/fwlink/?linkid=282716) 2.0.12 or above on the same system as the Data Management Gateway.</span></span> <span data-ttu-id="74b3e-117">PostgreSQL version 7.4 och senare stöds.</span><span class="sxs-lookup"><span data-stu-id="74b3e-117">PostgreSQL version 7.4 and above is supported.</span></span>

## <a name="getting-started"></a><span data-ttu-id="74b3e-118">Komma igång</span><span class="sxs-lookup"><span data-stu-id="74b3e-118">Getting started</span></span>
<span data-ttu-id="74b3e-119">Du kan skapa en pipeline med en kopia-aktivitet som flyttar data från ett dataarkiv för lokala PostgreSQL med hjälp av olika verktyg/API: er.</span><span class="sxs-lookup"><span data-stu-id="74b3e-119">You can create a pipeline with a copy activity that moves data from an on-premises PostgreSQL data store by using different tools/APIs.</span></span> 

- <span data-ttu-id="74b3e-120">Det enklaste sättet att skapa en pipeline är att använda den **guiden Kopiera**.</span><span class="sxs-lookup"><span data-stu-id="74b3e-120">The easiest way to create a pipeline is to use the **Copy Wizard**.</span></span> <span data-ttu-id="74b3e-121">Finns [Självstudier: skapa en pipeline med hjälp av guiden Kopiera](data-factory-copy-data-wizard-tutorial.md) för en snabb genomgång om hur du skapar en pipeline med hjälp av guiden Kopiera data.</span><span class="sxs-lookup"><span data-stu-id="74b3e-121">See [Tutorial: Create a pipeline using Copy Wizard](data-factory-copy-data-wizard-tutorial.md) for a quick walkthrough on creating a pipeline using the Copy data wizard.</span></span> 
- <span data-ttu-id="74b3e-122">Du kan också använda följande verktyg för att skapa en pipeline:</span><span class="sxs-lookup"><span data-stu-id="74b3e-122">You can also use the following tools to create a pipeline:</span></span> 
    - <span data-ttu-id="74b3e-123">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="74b3e-123">Azure portal</span></span>
    - <span data-ttu-id="74b3e-124">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="74b3e-124">Visual Studio</span></span>
    - <span data-ttu-id="74b3e-125">Azure PowerShell</span><span class="sxs-lookup"><span data-stu-id="74b3e-125">Azure PowerShell</span></span>
    - <span data-ttu-id="74b3e-126">Azure Resource Manager-mall</span><span class="sxs-lookup"><span data-stu-id="74b3e-126">Azure Resource Manager template</span></span>
    - <span data-ttu-id="74b3e-127">.NET-API</span><span class="sxs-lookup"><span data-stu-id="74b3e-127">.NET API</span></span>
    - <span data-ttu-id="74b3e-128">REST API</span><span class="sxs-lookup"><span data-stu-id="74b3e-128">REST API</span></span>

     <span data-ttu-id="74b3e-129">Se [kopiera aktivitet kursen](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) för stegvisa instruktioner för att skapa en pipeline med en Kopieringsaktivitet.</span><span class="sxs-lookup"><span data-stu-id="74b3e-129">See [Copy activity tutorial](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) for step-by-step instructions to create a pipeline with a copy activity.</span></span> 

<span data-ttu-id="74b3e-130">Om du använder verktyg eller API: er, kan du utföra följande steg för att skapa en pipeline som flyttar data från ett dataarkiv som källa till ett dataarkiv som mottagare:</span><span class="sxs-lookup"><span data-stu-id="74b3e-130">Whether you use the tools or APIs, you perform the following steps to create a pipeline that moves data from a source data store to a sink data store:</span></span>

1. <span data-ttu-id="74b3e-131">Skapa **länkade tjänster** att länka inkommande och utgående data lagras till din data factory.</span><span class="sxs-lookup"><span data-stu-id="74b3e-131">Create **linked services** to link input and output data stores to your data factory.</span></span>
2. <span data-ttu-id="74b3e-132">Skapa **datauppsättningar** att representera inkommande och utgående data för kopieringen.</span><span class="sxs-lookup"><span data-stu-id="74b3e-132">Create **datasets** to represent input and output data for the copy operation.</span></span> 
3. <span data-ttu-id="74b3e-133">Skapa en **pipeline** med en kopia-aktivitet som tar en datamängd som indata och en dataset som utdata.</span><span class="sxs-lookup"><span data-stu-id="74b3e-133">Create a **pipeline** with a copy activity that takes a dataset as an input and a dataset as an output.</span></span> 

<span data-ttu-id="74b3e-134">När du använder guiden skapas JSON definitioner för dessa Data Factory-enheter (länkade tjänster, datauppsättningar och pipelinen) automatiskt för dig.</span><span class="sxs-lookup"><span data-stu-id="74b3e-134">When you use the wizard, JSON definitions for these Data Factory entities (linked services, datasets, and the pipeline) are automatically created for you.</span></span> <span data-ttu-id="74b3e-135">När du använder Verktyg/API: er (utom .NET API), kan du definiera dessa Data Factory-enheter med hjälp av JSON-format.</span><span class="sxs-lookup"><span data-stu-id="74b3e-135">When you use tools/APIs (except .NET API), you define these Data Factory entities by using the JSON format.</span></span>  <span data-ttu-id="74b3e-136">Ett exempel med JSON-definitioner för Data Factory-entiteter som används för att kopiera data från ett dataarkiv för lokala PostgreSQL finns [JSON-exempel: kopiera data från PostgreSQL till Azure Blob](#json-example-copy-data-from-postgresql-to-azure-blob) i den här artikeln.</span><span class="sxs-lookup"><span data-stu-id="74b3e-136">For a sample with JSON definitions for Data Factory entities that are used to copy data from an on-premises PostgreSQL data store, see [JSON example: Copy data from PostgreSQL to Azure Blob](#json-example-copy-data-from-postgresql-to-azure-blob) section of this article.</span></span> 

<span data-ttu-id="74b3e-137">Följande avsnitt innehåller information om JSON-egenskaper som används för att definiera Data Factory entiteter i en PostgreSQL-datalager:</span><span class="sxs-lookup"><span data-stu-id="74b3e-137">The following sections provide details about JSON properties that are used to define Data Factory entities specific to a PostgreSQL data store:</span></span>

## <a name="linked-service-properties"></a><span data-ttu-id="74b3e-138">Länkad tjänstegenskaper</span><span class="sxs-lookup"><span data-stu-id="74b3e-138">Linked service properties</span></span>
<span data-ttu-id="74b3e-139">Följande tabell innehåller en beskrivning för JSON-element som är specifika för PostgreSQL länkad tjänst.</span><span class="sxs-lookup"><span data-stu-id="74b3e-139">The following table provides description for JSON elements specific to PostgreSQL linked service.</span></span>

| <span data-ttu-id="74b3e-140">Egenskap</span><span class="sxs-lookup"><span data-stu-id="74b3e-140">Property</span></span> | <span data-ttu-id="74b3e-141">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="74b3e-141">Description</span></span> | <span data-ttu-id="74b3e-142">Krävs</span><span class="sxs-lookup"><span data-stu-id="74b3e-142">Required</span></span> |
| --- | --- | --- |
| <span data-ttu-id="74b3e-143">typ</span><span class="sxs-lookup"><span data-stu-id="74b3e-143">type</span></span> |<span data-ttu-id="74b3e-144">Egenskapen type måste anges till: **OnPremisesPostgreSql**</span><span class="sxs-lookup"><span data-stu-id="74b3e-144">The type property must be set to: **OnPremisesPostgreSql**</span></span> |<span data-ttu-id="74b3e-145">Ja</span><span class="sxs-lookup"><span data-stu-id="74b3e-145">Yes</span></span> |
| <span data-ttu-id="74b3e-146">server</span><span class="sxs-lookup"><span data-stu-id="74b3e-146">server</span></span> |<span data-ttu-id="74b3e-147">Namnet på PostgreSQL-server.</span><span class="sxs-lookup"><span data-stu-id="74b3e-147">Name of the PostgreSQL server.</span></span> |<span data-ttu-id="74b3e-148">Ja</span><span class="sxs-lookup"><span data-stu-id="74b3e-148">Yes</span></span> |
| <span data-ttu-id="74b3e-149">Databasen</span><span class="sxs-lookup"><span data-stu-id="74b3e-149">database</span></span> |<span data-ttu-id="74b3e-150">Namnet på PostgreSQL-databas.</span><span class="sxs-lookup"><span data-stu-id="74b3e-150">Name of the PostgreSQL database.</span></span> |<span data-ttu-id="74b3e-151">Ja</span><span class="sxs-lookup"><span data-stu-id="74b3e-151">Yes</span></span> |
| <span data-ttu-id="74b3e-152">Schemat</span><span class="sxs-lookup"><span data-stu-id="74b3e-152">schema</span></span> |<span data-ttu-id="74b3e-153">Namnet på schemat i databasen.</span><span class="sxs-lookup"><span data-stu-id="74b3e-153">Name of the schema in the database.</span></span> <span data-ttu-id="74b3e-154">Schemanamnet är skiftlägeskänslig.</span><span class="sxs-lookup"><span data-stu-id="74b3e-154">The schema name is case-sensitive.</span></span> |<span data-ttu-id="74b3e-155">Nej</span><span class="sxs-lookup"><span data-stu-id="74b3e-155">No</span></span> |
| <span data-ttu-id="74b3e-156">AuthenticationType</span><span class="sxs-lookup"><span data-stu-id="74b3e-156">authenticationType</span></span> |<span data-ttu-id="74b3e-157">Typ av autentisering som används för att ansluta till PostgreSQL-databasen.</span><span class="sxs-lookup"><span data-stu-id="74b3e-157">Type of authentication used to connect to the PostgreSQL database.</span></span> <span data-ttu-id="74b3e-158">Möjliga värden är: anonym, grundläggande och Windows.</span><span class="sxs-lookup"><span data-stu-id="74b3e-158">Possible values are: Anonymous, Basic, and Windows.</span></span> |<span data-ttu-id="74b3e-159">Ja</span><span class="sxs-lookup"><span data-stu-id="74b3e-159">Yes</span></span> |
| <span data-ttu-id="74b3e-160">användarnamn</span><span class="sxs-lookup"><span data-stu-id="74b3e-160">username</span></span> |<span data-ttu-id="74b3e-161">Ange användarnamnet om du använder grundläggande eller Windows-autentisering.</span><span class="sxs-lookup"><span data-stu-id="74b3e-161">Specify user name if you are using Basic or Windows authentication.</span></span> |<span data-ttu-id="74b3e-162">Nej</span><span class="sxs-lookup"><span data-stu-id="74b3e-162">No</span></span> |
| <span data-ttu-id="74b3e-163">lösenord</span><span class="sxs-lookup"><span data-stu-id="74b3e-163">password</span></span> |<span data-ttu-id="74b3e-164">Ange lösenordet för det användarkonto som du angav för användarnamnet.</span><span class="sxs-lookup"><span data-stu-id="74b3e-164">Specify password for the user account you specified for the username.</span></span> |<span data-ttu-id="74b3e-165">Nej</span><span class="sxs-lookup"><span data-stu-id="74b3e-165">No</span></span> |
| <span data-ttu-id="74b3e-166">gatewayName</span><span class="sxs-lookup"><span data-stu-id="74b3e-166">gatewayName</span></span> |<span data-ttu-id="74b3e-167">Namnet på den gateway som Data Factory-tjänsten ska använda för att ansluta till den lokala PostgreSQL-databasen.</span><span class="sxs-lookup"><span data-stu-id="74b3e-167">Name of the gateway that the Data Factory service should use to connect to the on-premises PostgreSQL database.</span></span> |<span data-ttu-id="74b3e-168">Ja</span><span class="sxs-lookup"><span data-stu-id="74b3e-168">Yes</span></span> |

## <a name="dataset-properties"></a><span data-ttu-id="74b3e-169">Egenskaper för datamängd</span><span class="sxs-lookup"><span data-stu-id="74b3e-169">Dataset properties</span></span>
<span data-ttu-id="74b3e-170">En fullständig lista över egenskaper som är tillgängliga för att definiera datauppsättningarna & avsnitt finns i [skapa datauppsättningar](data-factory-create-datasets.md) artikel.</span><span class="sxs-lookup"><span data-stu-id="74b3e-170">For a full list of sections & properties available for defining datasets, see the [Creating datasets](data-factory-create-datasets.md) article.</span></span> <span data-ttu-id="74b3e-171">Avsnitt som struktur, tillgänglighet och princip på en datamängd JSON är liknande för alla typer av datauppsättningen.</span><span class="sxs-lookup"><span data-stu-id="74b3e-171">Sections such as structure, availability, and policy of a dataset JSON are similar for all dataset types.</span></span>

<span data-ttu-id="74b3e-172">Avsnittet typeProperties är olika för varje typ av dataset och ger information om placeringen av data i datalagret.</span><span class="sxs-lookup"><span data-stu-id="74b3e-172">The typeProperties section is different for each type of dataset and provides information about the location of the data in the data store.</span></span> <span data-ttu-id="74b3e-173">TypeProperties avsnittet för dataset av typen **RelationalTable** (som omfattar PostgreSQL dataset) har följande egenskaper:</span><span class="sxs-lookup"><span data-stu-id="74b3e-173">The typeProperties section for dataset of type **RelationalTable** (which includes PostgreSQL dataset) has the following properties:</span></span>

| <span data-ttu-id="74b3e-174">Egenskap</span><span class="sxs-lookup"><span data-stu-id="74b3e-174">Property</span></span> | <span data-ttu-id="74b3e-175">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="74b3e-175">Description</span></span> | <span data-ttu-id="74b3e-176">Krävs</span><span class="sxs-lookup"><span data-stu-id="74b3e-176">Required</span></span> |
| --- | --- | --- |
| <span data-ttu-id="74b3e-177">tableName</span><span class="sxs-lookup"><span data-stu-id="74b3e-177">tableName</span></span> |<span data-ttu-id="74b3e-178">Namnet på tabellen i PostgreSQL-databasinstansen som den länkade tjänsten refererar till.</span><span class="sxs-lookup"><span data-stu-id="74b3e-178">Name of the table in the PostgreSQL Database instance that linked service refers to.</span></span> <span data-ttu-id="74b3e-179">TableName är skiftlägeskänslig.</span><span class="sxs-lookup"><span data-stu-id="74b3e-179">The tableName is case-sensitive.</span></span> |<span data-ttu-id="74b3e-180">Nej (om **frågan** av **RelationalSource** har angetts)</span><span class="sxs-lookup"><span data-stu-id="74b3e-180">No (if **query** of **RelationalSource** is specified)</span></span> |

## <a name="copy-activity-properties"></a><span data-ttu-id="74b3e-181">Kopiera egenskaper för aktivitet</span><span class="sxs-lookup"><span data-stu-id="74b3e-181">Copy activity properties</span></span>
<span data-ttu-id="74b3e-182">En fullständig lista över avsnitt & egenskaper som är tillgängliga för att definiera aktiviteter finns i [skapar Pipelines](data-factory-create-pipelines.md) artikel.</span><span class="sxs-lookup"><span data-stu-id="74b3e-182">For a full list of sections & properties available for defining activities, see the [Creating Pipelines](data-factory-create-pipelines.md) article.</span></span> <span data-ttu-id="74b3e-183">Egenskaper som namn, beskrivning, ingående och utgående tabeller och principen är tillgängliga för alla typer av aktiviteter.</span><span class="sxs-lookup"><span data-stu-id="74b3e-183">Properties such as name, description, input and output tables, and policy are available for all types of activities.</span></span>

<span data-ttu-id="74b3e-184">De egenskaper som är tillgängliga i avsnittet typeProperties i aktiviteten varierar beroende på varje aktivitetstyp.</span><span class="sxs-lookup"><span data-stu-id="74b3e-184">Whereas, properties available in the typeProperties section of the activity vary with each activity type.</span></span> <span data-ttu-id="74b3e-185">För Kopieringsaktivitet kan variera de beroende på vilka typer av datakällor och sänkor.</span><span class="sxs-lookup"><span data-stu-id="74b3e-185">For Copy activity, they vary depending on the types of sources and sinks.</span></span>

<span data-ttu-id="74b3e-186">När datakällan är av typen **RelationalSource** (som omfattar PostgreSQL), följande egenskaper finns i avsnittet typeProperties:</span><span class="sxs-lookup"><span data-stu-id="74b3e-186">When source is of type **RelationalSource** (which includes PostgreSQL), the following properties are available in typeProperties section:</span></span>

| <span data-ttu-id="74b3e-187">Egenskap</span><span class="sxs-lookup"><span data-stu-id="74b3e-187">Property</span></span> | <span data-ttu-id="74b3e-188">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="74b3e-188">Description</span></span> | <span data-ttu-id="74b3e-189">Tillåtna värden</span><span class="sxs-lookup"><span data-stu-id="74b3e-189">Allowed values</span></span> | <span data-ttu-id="74b3e-190">Krävs</span><span class="sxs-lookup"><span data-stu-id="74b3e-190">Required</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="74b3e-191">DocumentDB</span><span class="sxs-lookup"><span data-stu-id="74b3e-191">query</span></span> |<span data-ttu-id="74b3e-192">Använd anpassad fråga för att läsa data.</span><span class="sxs-lookup"><span data-stu-id="74b3e-192">Use the custom query to read data.</span></span> |<span data-ttu-id="74b3e-193">SQL-sträng.</span><span class="sxs-lookup"><span data-stu-id="74b3e-193">SQL query string.</span></span> <span data-ttu-id="74b3e-194">Till exempel: ”frågan” ”: Välj * från \"MySchema\".\" Mytable prefix\"”.</span><span class="sxs-lookup"><span data-stu-id="74b3e-194">For example: "query": "select * from \"MySchema\".\"MyTable\"".</span></span> |<span data-ttu-id="74b3e-195">Nej (om **tableName** av **dataset** har angetts)</span><span class="sxs-lookup"><span data-stu-id="74b3e-195">No (if **tableName** of **dataset** is specified)</span></span> |

> [!NOTE]
> <span data-ttu-id="74b3e-196">Schema och tabellnamn är skiftlägeskänsliga.</span><span class="sxs-lookup"><span data-stu-id="74b3e-196">Schema and table names are case-sensitive.</span></span> <span data-ttu-id="74b3e-197">Innesluts i `""` (dubbla citattecken) i frågan.</span><span class="sxs-lookup"><span data-stu-id="74b3e-197">Enclose them in `""` (double quotes) in the query.</span></span>  

<span data-ttu-id="74b3e-198">**Exempel:**</span><span class="sxs-lookup"><span data-stu-id="74b3e-198">**Example:**</span></span>

 `"query": "select * from \"MySchema\".\"MyTable\""`

## <a name="json-example-copy-data-from-postgresql-to-azure-blob"></a><span data-ttu-id="74b3e-199">JSON-exempel: kopiera data från PostgreSQL till Azure-Blob</span><span class="sxs-lookup"><span data-stu-id="74b3e-199">JSON example: Copy data from PostgreSQL to Azure Blob</span></span>
<span data-ttu-id="74b3e-200">Det här exemplet innehåller exempel JSON definitioner som du kan använda för att skapa en pipeline med [Azure-portalen](data-factory-copy-activity-tutorial-using-azure-portal.md) eller [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md) eller [Azure PowerShell](data-factory-copy-activity-tutorial-using-powershell.md).</span><span class="sxs-lookup"><span data-stu-id="74b3e-200">This example provides sample JSON definitions that you can use to create a pipeline by using [Azure portal](data-factory-copy-activity-tutorial-using-azure-portal.md) or [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md) or [Azure PowerShell](data-factory-copy-activity-tutorial-using-powershell.md).</span></span> <span data-ttu-id="74b3e-201">De visar hur du kopierar data från PostgreSQL-databas till Azure Blob Storage.</span><span class="sxs-lookup"><span data-stu-id="74b3e-201">They show how to copy data from PostgreSQL database to Azure Blob Storage.</span></span> <span data-ttu-id="74b3e-202">Dock datan kan kopieras till någon av sänkor anges [här](data-factory-data-movement-activities.md#supported-data-stores-and-formats) med hjälp av aktiviteten kopiera i Azure Data Factory.</span><span class="sxs-lookup"><span data-stu-id="74b3e-202">However, data can be copied to any of the sinks stated [here](data-factory-data-movement-activities.md#supported-data-stores-and-formats) using the Copy Activity in Azure Data Factory.</span></span>   

> [!IMPORTANT]
> <span data-ttu-id="74b3e-203">Det här exemplet innehåller JSON kodavsnitt.</span><span class="sxs-lookup"><span data-stu-id="74b3e-203">This sample provides JSON snippets.</span></span> <span data-ttu-id="74b3e-204">Stegvisa instruktioner för att skapa datafabriken inkluderas inte.</span><span class="sxs-lookup"><span data-stu-id="74b3e-204">It does not include step-by-step instructions for creating the data factory.</span></span> <span data-ttu-id="74b3e-205">Se [flytta data mellan lokala platser och moln](data-factory-move-data-between-onprem-and-cloud.md) artikel stegvisa instruktioner.</span><span class="sxs-lookup"><span data-stu-id="74b3e-205">See [moving data between on-premises locations and cloud](data-factory-move-data-between-onprem-and-cloud.md) article for step-by-step instructions.</span></span>

<span data-ttu-id="74b3e-206">Exemplet har följande data factory enheter:</span><span class="sxs-lookup"><span data-stu-id="74b3e-206">The sample has the following data factory entities:</span></span>

1. <span data-ttu-id="74b3e-207">En länkad tjänst av typen [OnPremisesPostgreSql](data-factory-onprem-postgresql-connector.md#linked-service-properties).</span><span class="sxs-lookup"><span data-stu-id="74b3e-207">A linked service of type [OnPremisesPostgreSql](data-factory-onprem-postgresql-connector.md#linked-service-properties).</span></span>
2. <span data-ttu-id="74b3e-208">En länkad tjänst av typen [AzureStorage](data-factory-azure-blob-connector.md#linked-service-properties).</span><span class="sxs-lookup"><span data-stu-id="74b3e-208">A linked service of type [AzureStorage](data-factory-azure-blob-connector.md#linked-service-properties).</span></span>
3. <span data-ttu-id="74b3e-209">Indata [dataset](data-factory-create-datasets.md) av typen [RelationalTable](data-factory-onprem-postgresql-connector.md#dataset-properties).</span><span class="sxs-lookup"><span data-stu-id="74b3e-209">An input [dataset](data-factory-create-datasets.md) of type [RelationalTable](data-factory-onprem-postgresql-connector.md#dataset-properties).</span></span>
4. <span data-ttu-id="74b3e-210">Utdata [dataset](data-factory-create-datasets.md) av typen [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties).</span><span class="sxs-lookup"><span data-stu-id="74b3e-210">An output [dataset](data-factory-create-datasets.md) of type [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties).</span></span>
5. <span data-ttu-id="74b3e-211">Den [pipeline](data-factory-create-pipelines.md) med Kopieringsaktiviteten som använder [RelationalSource](data-factory-onprem-postgresql-connector.md#copy-activity-properties) och [BlobSink](data-factory-azure-blob-connector.md#copy-activity-properties).</span><span class="sxs-lookup"><span data-stu-id="74b3e-211">The [pipeline](data-factory-create-pipelines.md) with Copy Activity that uses [RelationalSource](data-factory-onprem-postgresql-connector.md#copy-activity-properties) and [BlobSink](data-factory-azure-blob-connector.md#copy-activity-properties).</span></span>

<span data-ttu-id="74b3e-212">Exemplet kopierar data från ett frågeresultat i PostgreSQL-databas till en blobb varje timme.</span><span class="sxs-lookup"><span data-stu-id="74b3e-212">The sample copies data from a query result in PostgreSQL database to a blob every hour.</span></span> <span data-ttu-id="74b3e-213">JSON-egenskaper som används i exemplen beskrivs i exemplen i följande avsnitt.</span><span class="sxs-lookup"><span data-stu-id="74b3e-213">The JSON properties used in these samples are described in sections following the samples.</span></span>

<span data-ttu-id="74b3e-214">Som ett första steg bör du konfigurera data management gateway.</span><span class="sxs-lookup"><span data-stu-id="74b3e-214">As a first step, setup the data management gateway.</span></span> <span data-ttu-id="74b3e-215">Anvisningarna är i den [flytta data mellan lokala platser och moln](data-factory-move-data-between-onprem-and-cloud.md) artikel.</span><span class="sxs-lookup"><span data-stu-id="74b3e-215">The instructions are in the [moving data between on-premises locations and cloud](data-factory-move-data-between-onprem-and-cloud.md) article.</span></span>

<span data-ttu-id="74b3e-216">**PostgreSQL länkade tjänsten:**</span><span class="sxs-lookup"><span data-stu-id="74b3e-216">**PostgreSQL linked service:**</span></span>

```json
{
    "name": "OnPremPostgreSqlLinkedService",
    "properties": {
        "type": "OnPremisesPostgreSql",
        "typeProperties": {
            "server": "<server>",
            "database": "<database>",
            "schema": "<schema>",
            "authenticationType": "<authentication type>",
            "username": "<username>",
            "password": "<password>",
            "gatewayName": "<gatewayName>"
        }
    }
}
```
<span data-ttu-id="74b3e-217">**Azure Blob storage länkade tjänsten:**</span><span class="sxs-lookup"><span data-stu-id="74b3e-217">**Azure Blob storage linked service:**</span></span>

```json
{
    "name": "AzureStorageLinkedService",
    "properties": {
    "type": "AzureStorage",
    "typeProperties": {
        "connectionString": "DefaultEndpointsProtocol=https;AccountName=<AccountName>;AccountKey=<AccountKey>"
    }
    }
}
```
<span data-ttu-id="74b3e-218">**PostgreSQL inkommande datauppsättningen:**</span><span class="sxs-lookup"><span data-stu-id="74b3e-218">**PostgreSQL input dataset:**</span></span>

<span data-ttu-id="74b3e-219">Exemplet förutsätter att du har skapat en tabell ”mytable” som prefix i PostgreSQL och innehåller en kolumn med namnet ”tidsstämpel” för tid series-data.</span><span class="sxs-lookup"><span data-stu-id="74b3e-219">The sample assumes you have created a table “MyTable” in PostgreSQL and it contains a column called “timestamp” for time series data.</span></span>

<span data-ttu-id="74b3e-220">Ange `"external": true` informerar Data Factory-tjänsten att datamängden är extern till data factory och inte tillverkas av en aktivitet i datafabriken.</span><span class="sxs-lookup"><span data-stu-id="74b3e-220">Setting `"external": true` informs the Data Factory service that the dataset is external to the data factory and is not produced by an activity in the data factory.</span></span>

```json
{
    "name": "PostgreSqlDataSet",
    "properties": {
        "type": "RelationalTable",
        "linkedServiceName": "OnPremPostgreSqlLinkedService",
        "typeProperties": {},
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

<span data-ttu-id="74b3e-221">**Azure Blob utdatauppsättningen:**</span><span class="sxs-lookup"><span data-stu-id="74b3e-221">**Azure Blob output dataset:**</span></span>

<span data-ttu-id="74b3e-222">Data skrivs till en ny blob varje timme (frekvens: timme, intervall: 1).</span><span class="sxs-lookup"><span data-stu-id="74b3e-222">Data is written to a new blob every hour (frequency: hour, interval: 1).</span></span> <span data-ttu-id="74b3e-223">Mappen sökvägen och filnamnet för blobben utvärderas dynamiskt baserat på starttiden för den sektor som bearbetas.</span><span class="sxs-lookup"><span data-stu-id="74b3e-223">The folder path and file name for the blob are dynamically evaluated based on the start time of the slice that is being processed.</span></span> <span data-ttu-id="74b3e-224">Mappsökvägen använder år, månad, dag och timmar delar av starttiden.</span><span class="sxs-lookup"><span data-stu-id="74b3e-224">The folder path uses year, month, day, and hours parts of the start time.</span></span>

```json
{
    "name": "AzureBlobPostgreSqlDataSet",
    "properties": {
        "type": "AzureBlob",
        "linkedServiceName": "AzureStorageLinkedService",
        "typeProperties": {
            "folderPath": "mycontainer/postgresql/yearno={Year}/monthno={Month}/dayno={Day}/hourno={Hour}",
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

<span data-ttu-id="74b3e-225">**Pipeline med kopieringsaktiviteten:**</span><span class="sxs-lookup"><span data-stu-id="74b3e-225">**Pipeline with Copy activity:**</span></span>

<span data-ttu-id="74b3e-226">Pipelinen innehåller en kopia-aktivitet som är konfigurerad för att använda indata och utdata-datauppsättningar och har schemalagts att köras varje timme.</span><span class="sxs-lookup"><span data-stu-id="74b3e-226">The pipeline contains a Copy Activity that is configured to use the input and output datasets and is scheduled to run hourly.</span></span> <span data-ttu-id="74b3e-227">I pipeline-JSON-definitionen av **källa** är inställd på **RelationalSource** och **sink** är inställd på **BlobSink**.</span><span class="sxs-lookup"><span data-stu-id="74b3e-227">In the pipeline JSON definition, the **source** type is set to **RelationalSource** and **sink** type is set to **BlobSink**.</span></span> <span data-ttu-id="74b3e-228">SQL-frågan som angetts för den **frågan** egenskapen väljer vilka data från tabellen public.usstates i PostgreSQL-databas.</span><span class="sxs-lookup"><span data-stu-id="74b3e-228">The SQL query specified for the **query** property selects the data from the public.usstates table in the PostgreSQL database.</span></span>

```json
{
    "name": "CopyPostgreSqlToBlob",
    "properties": {
        "description": "pipeline for copy activity",
        "activities": [
            {
                "type": "Copy",
                "typeProperties": {
                    "source": {
                        "type": "RelationalSource",
                        "query": "select * from \"public\".\"usstates\""
                    },
                    "sink": {
                        "type": "BlobSink"
                    }
                },
                "inputs": [
                    {
                        "name": "PostgreSqlDataSet"
                    }
                ],
                "outputs": [
                    {
                        "name": "AzureBlobPostgreSqlDataSet"
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
                "name": "PostgreSqlToBlob"
            }
        ],
        "start": "2014-06-01T18:00:00Z",
        "end": "2014-06-01T19:00:00Z"
    }
}
```
## <a name="type-mapping-for-postgresql"></a><span data-ttu-id="74b3e-229">Mappning för PostgreSQL</span><span class="sxs-lookup"><span data-stu-id="74b3e-229">Type mapping for PostgreSQL</span></span>
<span data-ttu-id="74b3e-230">Som anges i den [data movement aktiviteter](data-factory-data-movement-activities.md) artikel kopieringsaktiviteten utför automatisk konverteringar från källtyper att registrera typer med följande metod i steg 2:</span><span class="sxs-lookup"><span data-stu-id="74b3e-230">As mentioned in the [data movement activities](data-factory-data-movement-activities.md) article Copy activity performs automatic type conversions from source types to sink types with the following 2-step approach:</span></span>

1. <span data-ttu-id="74b3e-231">Konvertera från interna källtyper till .NET-typ</span><span class="sxs-lookup"><span data-stu-id="74b3e-231">Convert from native source types to .NET type</span></span>
2. <span data-ttu-id="74b3e-232">Konvertera från .NET-typ till interna mottagare typ.</span><span class="sxs-lookup"><span data-stu-id="74b3e-232">Convert from .NET type to native sink type</span></span>

<span data-ttu-id="74b3e-233">När data flyttas till PostgreSQL, används följande mappningar från PostgreSQL-typ för .NET-typ.</span><span class="sxs-lookup"><span data-stu-id="74b3e-233">When moving data to PostgreSQL, the following mappings are used from PostgreSQL type to .NET type.</span></span>

| <span data-ttu-id="74b3e-234">Typ av PostgreSQL-databas</span><span class="sxs-lookup"><span data-stu-id="74b3e-234">PostgreSQL Database type</span></span> | <span data-ttu-id="74b3e-235">PostgresSQL alias</span><span class="sxs-lookup"><span data-stu-id="74b3e-235">PostgresSQL aliases</span></span> | <span data-ttu-id="74b3e-236">.NET framework-typ</span><span class="sxs-lookup"><span data-stu-id="74b3e-236">.NET Framework type</span></span> |
| --- | --- | --- |
| <span data-ttu-id="74b3e-237">abstime</span><span class="sxs-lookup"><span data-stu-id="74b3e-237">abstime</span></span> | |<span data-ttu-id="74b3e-238">Datum och tid</span><span class="sxs-lookup"><span data-stu-id="74b3e-238">Datetime</span></span> | &nbsp;
| <span data-ttu-id="74b3e-239">bigint</span><span class="sxs-lookup"><span data-stu-id="74b3e-239">bigint</span></span> |<span data-ttu-id="74b3e-240">int8</span><span class="sxs-lookup"><span data-stu-id="74b3e-240">int8</span></span> |<span data-ttu-id="74b3e-241">Int64</span><span class="sxs-lookup"><span data-stu-id="74b3e-241">Int64</span></span> |
| <span data-ttu-id="74b3e-242">bigserial</span><span class="sxs-lookup"><span data-stu-id="74b3e-242">bigserial</span></span> |<span data-ttu-id="74b3e-243">serial8</span><span class="sxs-lookup"><span data-stu-id="74b3e-243">serial8</span></span> |<span data-ttu-id="74b3e-244">Int64</span><span class="sxs-lookup"><span data-stu-id="74b3e-244">Int64</span></span> |
| <span data-ttu-id="74b3e-245">bitars [(n)]</span><span class="sxs-lookup"><span data-stu-id="74b3e-245">bit [ (n) ]</span></span> | |<span data-ttu-id="74b3e-246">Byte [], sträng</span><span class="sxs-lookup"><span data-stu-id="74b3e-246">Byte[], String</span></span> | &nbsp;
| <span data-ttu-id="74b3e-247">bit varierande [(n)]</span><span class="sxs-lookup"><span data-stu-id="74b3e-247">bit varying [ (n) ]</span></span> |<span data-ttu-id="74b3e-248">varbit</span><span class="sxs-lookup"><span data-stu-id="74b3e-248">varbit</span></span> |<span data-ttu-id="74b3e-249">Byte [], sträng</span><span class="sxs-lookup"><span data-stu-id="74b3e-249">Byte[], String</span></span> |
| <span data-ttu-id="74b3e-250">Booleskt värde</span><span class="sxs-lookup"><span data-stu-id="74b3e-250">boolean</span></span> |<span data-ttu-id="74b3e-251">bool</span><span class="sxs-lookup"><span data-stu-id="74b3e-251">bool</span></span> |<span data-ttu-id="74b3e-252">Booleskt värde</span><span class="sxs-lookup"><span data-stu-id="74b3e-252">Boolean</span></span> |
| <span data-ttu-id="74b3e-253">Rutan</span><span class="sxs-lookup"><span data-stu-id="74b3e-253">box</span></span> | |<span data-ttu-id="74b3e-254">Byte [], sträng</span><span class="sxs-lookup"><span data-stu-id="74b3e-254">Byte[], String</span></span> |&nbsp;
| <span data-ttu-id="74b3e-255">bytea</span><span class="sxs-lookup"><span data-stu-id="74b3e-255">bytea</span></span> | |<span data-ttu-id="74b3e-256">Byte [], sträng</span><span class="sxs-lookup"><span data-stu-id="74b3e-256">Byte[], String</span></span> |&nbsp;
| <span data-ttu-id="74b3e-257">tecknet [(n)]</span><span class="sxs-lookup"><span data-stu-id="74b3e-257">character [ (n) ]</span></span> |<span data-ttu-id="74b3e-258">char [(n)]</span><span class="sxs-lookup"><span data-stu-id="74b3e-258">char [ (n) ]</span></span> |<span data-ttu-id="74b3e-259">Sträng</span><span class="sxs-lookup"><span data-stu-id="74b3e-259">String</span></span> |
| <span data-ttu-id="74b3e-260">tecknet varierande [(n)]</span><span class="sxs-lookup"><span data-stu-id="74b3e-260">character varying [ (n) ]</span></span> |<span data-ttu-id="74b3e-261">varchar [(n)]</span><span class="sxs-lookup"><span data-stu-id="74b3e-261">varchar [ (n) ]</span></span> |<span data-ttu-id="74b3e-262">Sträng</span><span class="sxs-lookup"><span data-stu-id="74b3e-262">String</span></span> |
| <span data-ttu-id="74b3e-263">CID</span><span class="sxs-lookup"><span data-stu-id="74b3e-263">cid</span></span> | |<span data-ttu-id="74b3e-264">Sträng</span><span class="sxs-lookup"><span data-stu-id="74b3e-264">String</span></span> |&nbsp;
| <span data-ttu-id="74b3e-265">CIDR</span><span class="sxs-lookup"><span data-stu-id="74b3e-265">cidr</span></span> | |<span data-ttu-id="74b3e-266">Sträng</span><span class="sxs-lookup"><span data-stu-id="74b3e-266">String</span></span> |&nbsp;
| <span data-ttu-id="74b3e-267">cirkel</span><span class="sxs-lookup"><span data-stu-id="74b3e-267">circle</span></span> | |<span data-ttu-id="74b3e-268">Byte [], sträng</span><span class="sxs-lookup"><span data-stu-id="74b3e-268">Byte[], String</span></span> |&nbsp;
| <span data-ttu-id="74b3e-269">Datum</span><span class="sxs-lookup"><span data-stu-id="74b3e-269">date</span></span> | |<span data-ttu-id="74b3e-270">Datum och tid</span><span class="sxs-lookup"><span data-stu-id="74b3e-270">Datetime</span></span> |&nbsp;
| <span data-ttu-id="74b3e-271">DateRange</span><span class="sxs-lookup"><span data-stu-id="74b3e-271">daterange</span></span> | |<span data-ttu-id="74b3e-272">Sträng</span><span class="sxs-lookup"><span data-stu-id="74b3e-272">String</span></span> |&nbsp;
| <span data-ttu-id="74b3e-273">dubbel precision</span><span class="sxs-lookup"><span data-stu-id="74b3e-273">double precision</span></span> |<span data-ttu-id="74b3e-274">FLOAT8</span><span class="sxs-lookup"><span data-stu-id="74b3e-274">float8</span></span> |<span data-ttu-id="74b3e-275">dubbla</span><span class="sxs-lookup"><span data-stu-id="74b3e-275">Double</span></span> |
| <span data-ttu-id="74b3e-276">inet</span><span class="sxs-lookup"><span data-stu-id="74b3e-276">inet</span></span> | |<span data-ttu-id="74b3e-277">Byte [], sträng</span><span class="sxs-lookup"><span data-stu-id="74b3e-277">Byte[], String</span></span> |&nbsp;
| <span data-ttu-id="74b3e-278">intarry</span><span class="sxs-lookup"><span data-stu-id="74b3e-278">intarry</span></span> | |<span data-ttu-id="74b3e-279">Sträng</span><span class="sxs-lookup"><span data-stu-id="74b3e-279">String</span></span> |&nbsp;
| <span data-ttu-id="74b3e-280">int4range</span><span class="sxs-lookup"><span data-stu-id="74b3e-280">int4range</span></span> | |<span data-ttu-id="74b3e-281">Sträng</span><span class="sxs-lookup"><span data-stu-id="74b3e-281">String</span></span> |&nbsp;
| <span data-ttu-id="74b3e-282">int8range</span><span class="sxs-lookup"><span data-stu-id="74b3e-282">int8range</span></span> | |<span data-ttu-id="74b3e-283">Sträng</span><span class="sxs-lookup"><span data-stu-id="74b3e-283">String</span></span> |&nbsp;
| <span data-ttu-id="74b3e-284">heltal</span><span class="sxs-lookup"><span data-stu-id="74b3e-284">integer</span></span> |<span data-ttu-id="74b3e-285">int, int4</span><span class="sxs-lookup"><span data-stu-id="74b3e-285">int, int4</span></span> |<span data-ttu-id="74b3e-286">Int32</span><span class="sxs-lookup"><span data-stu-id="74b3e-286">Int32</span></span> |
| <span data-ttu-id="74b3e-287">intervallet [fält] [(p)]</span><span class="sxs-lookup"><span data-stu-id="74b3e-287">interval [ fields ] [ (p) ]</span></span> | |<span data-ttu-id="74b3e-288">TimeSpan</span><span class="sxs-lookup"><span data-stu-id="74b3e-288">Timespan</span></span> |&nbsp;
| <span data-ttu-id="74b3e-289">JSON</span><span class="sxs-lookup"><span data-stu-id="74b3e-289">json</span></span> | |<span data-ttu-id="74b3e-290">Sträng</span><span class="sxs-lookup"><span data-stu-id="74b3e-290">String</span></span> |&nbsp;
| <span data-ttu-id="74b3e-291">jsonb</span><span class="sxs-lookup"><span data-stu-id="74b3e-291">jsonb</span></span> | |<span data-ttu-id="74b3e-292">byte]</span><span class="sxs-lookup"><span data-stu-id="74b3e-292">Byte[]</span></span> |&nbsp;
| <span data-ttu-id="74b3e-293">raden</span><span class="sxs-lookup"><span data-stu-id="74b3e-293">line</span></span> | |<span data-ttu-id="74b3e-294">Byte [], sträng</span><span class="sxs-lookup"><span data-stu-id="74b3e-294">Byte[], String</span></span> |&nbsp;
| <span data-ttu-id="74b3e-295">lseg</span><span class="sxs-lookup"><span data-stu-id="74b3e-295">lseg</span></span> | |<span data-ttu-id="74b3e-296">Byte [], sträng</span><span class="sxs-lookup"><span data-stu-id="74b3e-296">Byte[], String</span></span> |&nbsp;
| <span data-ttu-id="74b3e-297">macaddr</span><span class="sxs-lookup"><span data-stu-id="74b3e-297">macaddr</span></span> | |<span data-ttu-id="74b3e-298">Byte [], sträng</span><span class="sxs-lookup"><span data-stu-id="74b3e-298">Byte[], String</span></span> |&nbsp;
| <span data-ttu-id="74b3e-299">Money</span><span class="sxs-lookup"><span data-stu-id="74b3e-299">money</span></span> | |<span data-ttu-id="74b3e-300">Decimal</span><span class="sxs-lookup"><span data-stu-id="74b3e-300">Decimal</span></span> |&nbsp;
| <span data-ttu-id="74b3e-301">numeriska [(p, s)]</span><span class="sxs-lookup"><span data-stu-id="74b3e-301">numeric [ (p, s) ]</span></span> |<span data-ttu-id="74b3e-302">decimal [(p, s)]</span><span class="sxs-lookup"><span data-stu-id="74b3e-302">decimal [ (p, s) ]</span></span> |<span data-ttu-id="74b3e-303">Decimal</span><span class="sxs-lookup"><span data-stu-id="74b3e-303">Decimal</span></span> |
| <span data-ttu-id="74b3e-304">numrange</span><span class="sxs-lookup"><span data-stu-id="74b3e-304">numrange</span></span> | |<span data-ttu-id="74b3e-305">Sträng</span><span class="sxs-lookup"><span data-stu-id="74b3e-305">String</span></span> |&nbsp;
| <span data-ttu-id="74b3e-306">OID</span><span class="sxs-lookup"><span data-stu-id="74b3e-306">oid</span></span> | |<span data-ttu-id="74b3e-307">Int32</span><span class="sxs-lookup"><span data-stu-id="74b3e-307">Int32</span></span> |&nbsp;
| <span data-ttu-id="74b3e-308">Sökväg</span><span class="sxs-lookup"><span data-stu-id="74b3e-308">path</span></span> | |<span data-ttu-id="74b3e-309">Byte [], sträng</span><span class="sxs-lookup"><span data-stu-id="74b3e-309">Byte[], String</span></span> |&nbsp;
| <span data-ttu-id="74b3e-310">pg_lsn</span><span class="sxs-lookup"><span data-stu-id="74b3e-310">pg_lsn</span></span> | |<span data-ttu-id="74b3e-311">Int64</span><span class="sxs-lookup"><span data-stu-id="74b3e-311">Int64</span></span> |&nbsp;
| <span data-ttu-id="74b3e-312">punkt</span><span class="sxs-lookup"><span data-stu-id="74b3e-312">point</span></span> | |<span data-ttu-id="74b3e-313">Byte [], sträng</span><span class="sxs-lookup"><span data-stu-id="74b3e-313">Byte[], String</span></span> |&nbsp;
| <span data-ttu-id="74b3e-314">polygon</span><span class="sxs-lookup"><span data-stu-id="74b3e-314">polygon</span></span> | |<span data-ttu-id="74b3e-315">Byte [], sträng</span><span class="sxs-lookup"><span data-stu-id="74b3e-315">Byte[], String</span></span> |&nbsp;
| <span data-ttu-id="74b3e-316">Verklig</span><span class="sxs-lookup"><span data-stu-id="74b3e-316">real</span></span> |<span data-ttu-id="74b3e-317">FLOAT4</span><span class="sxs-lookup"><span data-stu-id="74b3e-317">float4</span></span> |<span data-ttu-id="74b3e-318">Enskild</span><span class="sxs-lookup"><span data-stu-id="74b3e-318">Single</span></span> |
| <span data-ttu-id="74b3e-319">smallint</span><span class="sxs-lookup"><span data-stu-id="74b3e-319">smallint</span></span> |<span data-ttu-id="74b3e-320">int2</span><span class="sxs-lookup"><span data-stu-id="74b3e-320">int2</span></span> |<span data-ttu-id="74b3e-321">Int16</span><span class="sxs-lookup"><span data-stu-id="74b3e-321">Int16</span></span> |
| <span data-ttu-id="74b3e-322">smallserial</span><span class="sxs-lookup"><span data-stu-id="74b3e-322">smallserial</span></span> |<span data-ttu-id="74b3e-323">serial2</span><span class="sxs-lookup"><span data-stu-id="74b3e-323">serial2</span></span> |<span data-ttu-id="74b3e-324">Int16</span><span class="sxs-lookup"><span data-stu-id="74b3e-324">Int16</span></span> |
| <span data-ttu-id="74b3e-325">Seriell</span><span class="sxs-lookup"><span data-stu-id="74b3e-325">serial</span></span> |<span data-ttu-id="74b3e-326">serial4</span><span class="sxs-lookup"><span data-stu-id="74b3e-326">serial4</span></span> |<span data-ttu-id="74b3e-327">Int32</span><span class="sxs-lookup"><span data-stu-id="74b3e-327">Int32</span></span> |
| <span data-ttu-id="74b3e-328">Text</span><span class="sxs-lookup"><span data-stu-id="74b3e-328">text</span></span> | |<span data-ttu-id="74b3e-329">Sträng</span><span class="sxs-lookup"><span data-stu-id="74b3e-329">String</span></span> |&nbsp;

## <a name="map-source-to-sink-columns"></a><span data-ttu-id="74b3e-330">Karta källan till mottagare för kolumner</span><span class="sxs-lookup"><span data-stu-id="74b3e-330">Map source to sink columns</span></span>
<span data-ttu-id="74b3e-331">Mer information om mappning kolumner i datauppsättningen källan till kolumner i datauppsättning mottagare, se [mappa dataset kolumner i Azure Data Factory](data-factory-map-columns.md).</span><span class="sxs-lookup"><span data-stu-id="74b3e-331">To learn about mapping columns in source dataset to columns in sink dataset, see [Mapping dataset columns in Azure Data Factory](data-factory-map-columns.md).</span></span>

## <a name="repeatable-read-from-relational-sources"></a><span data-ttu-id="74b3e-332">Upprepbar läsning från relationella källor</span><span class="sxs-lookup"><span data-stu-id="74b3e-332">Repeatable read from relational sources</span></span>
<span data-ttu-id="74b3e-333">Tänk på att undvika oväntade resultat repeterbarhet när kopiering av data från relationella data lagras.</span><span class="sxs-lookup"><span data-stu-id="74b3e-333">When copying data from relational data stores, keep repeatability in mind to avoid unintended outcomes.</span></span> <span data-ttu-id="74b3e-334">I Azure Data Factory, kan du köra en sektor manuellt.</span><span class="sxs-lookup"><span data-stu-id="74b3e-334">In Azure Data Factory, you can rerun a slice manually.</span></span> <span data-ttu-id="74b3e-335">Du kan också konfigurera i principen för en dataset så att ett segment som körs när ett fel uppstår.</span><span class="sxs-lookup"><span data-stu-id="74b3e-335">You can also configure retry policy for a dataset so that a slice is rerun when a failure occurs.</span></span> <span data-ttu-id="74b3e-336">När ett segment körs på något sätt, måste du kontrollera att samma data läses oavsett hur många gånger ett segment körs.</span><span class="sxs-lookup"><span data-stu-id="74b3e-336">When a slice is rerun in either way, you need to make sure that the same data is read no matter how many times a slice is run.</span></span> <span data-ttu-id="74b3e-337">Se [Repeatable läsa från relationella källor](data-factory-repeatable-copy.md#repeatable-read-from-relational-sources).</span><span class="sxs-lookup"><span data-stu-id="74b3e-337">See [Repeatable read from relational sources](data-factory-repeatable-copy.md#repeatable-read-from-relational-sources).</span></span>

## <a name="performance-and-tuning"></a><span data-ttu-id="74b3e-338">Prestanda och finjustering</span><span class="sxs-lookup"><span data-stu-id="74b3e-338">Performance and Tuning</span></span>
<span data-ttu-id="74b3e-339">Se [kopiera aktivitet prestanda och justera guiden](data-factory-copy-activity-performance.md) vill veta mer om viktiga faktorer som påverkan prestanda för flytt av data (Kopieringsaktiviteten) i Azure Data Factory och olika sätt att optimera den.</span><span class="sxs-lookup"><span data-stu-id="74b3e-339">See [Copy Activity Performance & Tuning Guide](data-factory-copy-activity-performance.md) to learn about key factors that impact performance of data movement (Copy Activity) in Azure Data Factory and various ways to optimize it.</span></span>
