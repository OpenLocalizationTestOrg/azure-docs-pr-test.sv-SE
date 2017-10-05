---
title: "Flytta data från Sybase med Azure Data Factory | Microsoft Docs"
description: "Läs mer om hur du flyttar data från Sybase-databas med hjälp av Azure Data Factory."
services: data-factory
documentationcenter: 
author: linda33wj
manager: jhubbard
editor: monicar
ms.assetid: b379ee10-0ff5-4974-8c87-c95f82f1c5c6
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/11/2017
ms.author: jingwang
ms.openlocfilehash: 617e604b220b8bc1c452e67da83f733448e16c0b
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/03/2017
---
# <a name="move-data-from-sybase-using-azure-data-factory"></a><span data-ttu-id="7a8bb-103">Flytta data från Sybase med Azure Data Factory</span><span class="sxs-lookup"><span data-stu-id="7a8bb-103">Move data from Sybase using Azure Data Factory</span></span>
<span data-ttu-id="7a8bb-104">Den här artikeln förklarar hur du använder aktiviteten kopiera i Azure Data Factory för att flytta data från en lokal Sybase-databas.</span><span class="sxs-lookup"><span data-stu-id="7a8bb-104">This article explains how to use the Copy Activity in Azure Data Factory to move data from an on-premises Sybase database.</span></span> <span data-ttu-id="7a8bb-105">Den bygger på den [Data Movement aktiviteter](data-factory-data-movement-activities.md) artikel som presenterar en allmän översikt över dataflyttning med copy-aktivitet.</span><span class="sxs-lookup"><span data-stu-id="7a8bb-105">It builds on the [Data Movement Activities](data-factory-data-movement-activities.md) article, which presents a general overview of data movement with the copy activity.</span></span>

<span data-ttu-id="7a8bb-106">Du kan kopiera data från ett dataarkiv för lokala Sybase till alla stöds sink-datalagret.</span><span class="sxs-lookup"><span data-stu-id="7a8bb-106">You can copy data from an on-premises Sybase data store to any supported sink data store.</span></span> <span data-ttu-id="7a8bb-107">En lista över datakällor som stöds som sänkor av kopieringsaktiviteten, finns det [stöds datalager](data-factory-data-movement-activities.md#supported-data-stores-and-formats) tabell.</span><span class="sxs-lookup"><span data-stu-id="7a8bb-107">For a list of data stores supported as sinks by the copy activity, see the [Supported data stores](data-factory-data-movement-activities.md#supported-data-stores-and-formats) table.</span></span> <span data-ttu-id="7a8bb-108">Data factory stöder för närvarande endast flytta data från en Sybase-databas till andra databaser, men inte för att flytta data från andra datalager till en Sybase-databas.</span><span class="sxs-lookup"><span data-stu-id="7a8bb-108">Data factory currently supports only moving data from a Sybase data store to other data stores, but not for moving data from other data stores to a Sybase data store.</span></span> 

## <a name="prerequisites"></a><span data-ttu-id="7a8bb-109">Krav</span><span class="sxs-lookup"><span data-stu-id="7a8bb-109">Prerequisites</span></span>
<span data-ttu-id="7a8bb-110">Data Factory-tjänsten stöder anslutning till lokala Sybase källor med hjälp av Data Management Gateway.</span><span class="sxs-lookup"><span data-stu-id="7a8bb-110">Data Factory service supports connecting to on-premises Sybase sources using the Data Management Gateway.</span></span> <span data-ttu-id="7a8bb-111">Se [flytta data mellan lokala platser och moln](data-factory-move-data-between-onprem-and-cloud.md) artikeln innehåller information om Data Management Gateway och stegvisa instruktioner om hur du konfigurerar en gateway.</span><span class="sxs-lookup"><span data-stu-id="7a8bb-111">See [moving data between on-premises locations and cloud](data-factory-move-data-between-onprem-and-cloud.md) article to learn about Data Management Gateway and step-by-step instructions on setting up the gateway.</span></span>

<span data-ttu-id="7a8bb-112">Gateway krävs även om Sybase-databasen finns i en Azure IaaS-VM.</span><span class="sxs-lookup"><span data-stu-id="7a8bb-112">Gateway is required even if the Sybase database is hosted in an Azure IaaS VM.</span></span> <span data-ttu-id="7a8bb-113">Du kan installera gatewayen på samma IaaS-VM som dataarkiv eller på en annan virtuell dator, förutsatt att gatewayen kan ansluta till databasen.</span><span class="sxs-lookup"><span data-stu-id="7a8bb-113">You can install the gateway on the same IaaS VM as the data store or on a different VM as long as the gateway can connect to the database.</span></span>

> [!NOTE]
> <span data-ttu-id="7a8bb-114">Se [felsökning av problem med gateway](data-factory-data-management-gateway.md#troubleshooting-gateway-issues) tips om hur du felsöker anslutning /-gateway relaterade problem.</span><span class="sxs-lookup"><span data-stu-id="7a8bb-114">See [Troubleshoot gateway issues](data-factory-data-management-gateway.md#troubleshooting-gateway-issues) for tips on troubleshooting connection/gateway related issues.</span></span>

## <a name="supported-versions-and-installation"></a><span data-ttu-id="7a8bb-115">Versioner som stöds och installation</span><span class="sxs-lookup"><span data-stu-id="7a8bb-115">Supported versions and installation</span></span>
<span data-ttu-id="7a8bb-116">För Data Management Gateway att ansluta till Sybase-databasen, måste du installera den [dataprovider för Sybase iAnywhere.Data.SQLAnywhere](http://go.microsoft.com/fwlink/?linkid=324846) 16 eller senare på samma system som Data Management Gateway.</span><span class="sxs-lookup"><span data-stu-id="7a8bb-116">For Data Management Gateway to connect to the Sybase Database, you need to install the [data provider for Sybase iAnywhere.Data.SQLAnywhere](http://go.microsoft.com/fwlink/?linkid=324846) 16 or above on the same system as the Data Management Gateway.</span></span> <span data-ttu-id="7a8bb-117">Sybase version 16 och senare stöds.</span><span class="sxs-lookup"><span data-stu-id="7a8bb-117">Sybase version 16 and above is supported.</span></span>

## <a name="getting-started"></a><span data-ttu-id="7a8bb-118">Komma igång</span><span class="sxs-lookup"><span data-stu-id="7a8bb-118">Getting started</span></span>
<span data-ttu-id="7a8bb-119">Du kan skapa en pipeline med en kopia-aktivitet som flyttar data från en lokal Cassandra data store med hjälp av olika verktyg/API: er.</span><span class="sxs-lookup"><span data-stu-id="7a8bb-119">You can create a pipeline with a copy activity that moves data from an on-premises Cassandra data store by using different tools/APIs.</span></span> 

- <span data-ttu-id="7a8bb-120">Det enklaste sättet att skapa en pipeline är att använda den **guiden Kopiera**.</span><span class="sxs-lookup"><span data-stu-id="7a8bb-120">The easiest way to create a pipeline is to use the **Copy Wizard**.</span></span> <span data-ttu-id="7a8bb-121">Finns [Självstudier: skapa en pipeline med hjälp av guiden Kopiera](data-factory-copy-data-wizard-tutorial.md) för en snabb genomgång om hur du skapar en pipeline med hjälp av guiden Kopiera data.</span><span class="sxs-lookup"><span data-stu-id="7a8bb-121">See [Tutorial: Create a pipeline using Copy Wizard](data-factory-copy-data-wizard-tutorial.md) for a quick walkthrough on creating a pipeline using the Copy data wizard.</span></span> 
- <span data-ttu-id="7a8bb-122">Du kan också använda följande verktyg för att skapa en pipeline: **Azure-portalen**, **Visual Studio**, **Azure PowerShell**, **Azure Resource Manager-mall**, **.NET API**, och **REST API**.</span><span class="sxs-lookup"><span data-stu-id="7a8bb-122">You can also use the following tools to create a pipeline: **Azure portal**, **Visual Studio**, **Azure PowerShell**, **Azure Resource Manager template**, **.NET API**, and **REST API**.</span></span> <span data-ttu-id="7a8bb-123">Se [kopiera aktivitet kursen](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) för stegvisa instruktioner för att skapa en pipeline med en Kopieringsaktivitet.</span><span class="sxs-lookup"><span data-stu-id="7a8bb-123">See [Copy activity tutorial](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) for step-by-step instructions to create a pipeline with a copy activity.</span></span> 

<span data-ttu-id="7a8bb-124">Om du använder verktyg eller API: er, kan du utföra följande steg för att skapa en pipeline som flyttar data från ett dataarkiv som källa till ett dataarkiv som mottagare:</span><span class="sxs-lookup"><span data-stu-id="7a8bb-124">Whether you use the tools or APIs, you perform the following steps to create a pipeline that moves data from a source data store to a sink data store:</span></span>

1. <span data-ttu-id="7a8bb-125">Skapa **länkade tjänster** att länka inkommande och utgående data lagras till din data factory.</span><span class="sxs-lookup"><span data-stu-id="7a8bb-125">Create **linked services** to link input and output data stores to your data factory.</span></span>
2. <span data-ttu-id="7a8bb-126">Skapa **datauppsättningar** att representera inkommande och utgående data för kopieringen.</span><span class="sxs-lookup"><span data-stu-id="7a8bb-126">Create **datasets** to represent input and output data for the copy operation.</span></span> 
3. <span data-ttu-id="7a8bb-127">Skapa en **pipeline** med en kopia-aktivitet som tar en datamängd som indata och en dataset som utdata.</span><span class="sxs-lookup"><span data-stu-id="7a8bb-127">Create a **pipeline** with a copy activity that takes a dataset as an input and a dataset as an output.</span></span> 

<span data-ttu-id="7a8bb-128">När du använder guiden skapas JSON definitioner för dessa Data Factory-enheter (länkade tjänster, datauppsättningar och pipelinen) automatiskt för dig.</span><span class="sxs-lookup"><span data-stu-id="7a8bb-128">When you use the wizard, JSON definitions for these Data Factory entities (linked services, datasets, and the pipeline) are automatically created for you.</span></span> <span data-ttu-id="7a8bb-129">När du använder Verktyg/API: er (utom .NET API), kan du definiera dessa Data Factory-enheter med hjälp av JSON-format.</span><span class="sxs-lookup"><span data-stu-id="7a8bb-129">When you use tools/APIs (except .NET API), you define these Data Factory entities by using the JSON format.</span></span>  <span data-ttu-id="7a8bb-130">Ett exempel med JSON-definitioner för Data Factory-entiteter som används för att kopiera data från ett dataarkiv för lokala Sybase finns [JSON-exempel: kopiera data från Sybase till Azure Blob](#json-example-copy-data-from-sybase-to-azure-blob) i den här artikeln.</span><span class="sxs-lookup"><span data-stu-id="7a8bb-130">For a sample with JSON definitions for Data Factory entities that are used to copy data from an on-premises Sybase data store, see [JSON example: Copy data from Sybase to Azure Blob](#json-example-copy-data-from-sybase-to-azure-blob) section of this article.</span></span> 

<span data-ttu-id="7a8bb-131">Följande avsnitt innehåller information om JSON-egenskaper som används för att definiera Data Factory entiteter till en Sybase-databasen:</span><span class="sxs-lookup"><span data-stu-id="7a8bb-131">The following sections provide details about JSON properties that are used to define Data Factory entities specific to a Sybase data store:</span></span>

## <a name="linked-service-properties"></a><span data-ttu-id="7a8bb-132">Länkad tjänstegenskaper</span><span class="sxs-lookup"><span data-stu-id="7a8bb-132">Linked service properties</span></span>
<span data-ttu-id="7a8bb-133">Följande tabell innehåller en beskrivning för JSON-element som är specifika för Sybase länkad tjänst.</span><span class="sxs-lookup"><span data-stu-id="7a8bb-133">The following table provides description for JSON elements specific to Sybase linked service.</span></span>

| <span data-ttu-id="7a8bb-134">Egenskap</span><span class="sxs-lookup"><span data-stu-id="7a8bb-134">Property</span></span> | <span data-ttu-id="7a8bb-135">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="7a8bb-135">Description</span></span> | <span data-ttu-id="7a8bb-136">Krävs</span><span class="sxs-lookup"><span data-stu-id="7a8bb-136">Required</span></span> |
| --- | --- | --- |
| <span data-ttu-id="7a8bb-137">typ</span><span class="sxs-lookup"><span data-stu-id="7a8bb-137">type</span></span> |<span data-ttu-id="7a8bb-138">Egenskapen type måste anges till: **OnPremisesSybase**</span><span class="sxs-lookup"><span data-stu-id="7a8bb-138">The type property must be set to: **OnPremisesSybase**</span></span> |<span data-ttu-id="7a8bb-139">Ja</span><span class="sxs-lookup"><span data-stu-id="7a8bb-139">Yes</span></span> |
| <span data-ttu-id="7a8bb-140">server</span><span class="sxs-lookup"><span data-stu-id="7a8bb-140">server</span></span> |<span data-ttu-id="7a8bb-141">Namnet på Sybase-servern.</span><span class="sxs-lookup"><span data-stu-id="7a8bb-141">Name of the Sybase server.</span></span> |<span data-ttu-id="7a8bb-142">Ja</span><span class="sxs-lookup"><span data-stu-id="7a8bb-142">Yes</span></span> |
| <span data-ttu-id="7a8bb-143">Databasen</span><span class="sxs-lookup"><span data-stu-id="7a8bb-143">database</span></span> |<span data-ttu-id="7a8bb-144">Namnet på Sybase-databasen.</span><span class="sxs-lookup"><span data-stu-id="7a8bb-144">Name of the Sybase database.</span></span> |<span data-ttu-id="7a8bb-145">Ja</span><span class="sxs-lookup"><span data-stu-id="7a8bb-145">Yes</span></span> |
| <span data-ttu-id="7a8bb-146">Schemat</span><span class="sxs-lookup"><span data-stu-id="7a8bb-146">schema</span></span> |<span data-ttu-id="7a8bb-147">Namnet på schemat i databasen.</span><span class="sxs-lookup"><span data-stu-id="7a8bb-147">Name of the schema in the database.</span></span> |<span data-ttu-id="7a8bb-148">Nej</span><span class="sxs-lookup"><span data-stu-id="7a8bb-148">No</span></span> |
| <span data-ttu-id="7a8bb-149">AuthenticationType</span><span class="sxs-lookup"><span data-stu-id="7a8bb-149">authenticationType</span></span> |<span data-ttu-id="7a8bb-150">Typ av autentisering som används för att ansluta till Sybase-databasen.</span><span class="sxs-lookup"><span data-stu-id="7a8bb-150">Type of authentication used to connect to the Sybase database.</span></span> <span data-ttu-id="7a8bb-151">Möjliga värden är: anonym, grundläggande och Windows.</span><span class="sxs-lookup"><span data-stu-id="7a8bb-151">Possible values are: Anonymous, Basic, and Windows.</span></span> |<span data-ttu-id="7a8bb-152">Ja</span><span class="sxs-lookup"><span data-stu-id="7a8bb-152">Yes</span></span> |
| <span data-ttu-id="7a8bb-153">användarnamn</span><span class="sxs-lookup"><span data-stu-id="7a8bb-153">username</span></span> |<span data-ttu-id="7a8bb-154">Ange användarnamnet om du använder grundläggande eller Windows-autentisering.</span><span class="sxs-lookup"><span data-stu-id="7a8bb-154">Specify user name if you are using Basic or Windows authentication.</span></span> |<span data-ttu-id="7a8bb-155">Nej</span><span class="sxs-lookup"><span data-stu-id="7a8bb-155">No</span></span> |
| <span data-ttu-id="7a8bb-156">lösenord</span><span class="sxs-lookup"><span data-stu-id="7a8bb-156">password</span></span> |<span data-ttu-id="7a8bb-157">Ange lösenordet för det användarkonto som du angav för användarnamnet.</span><span class="sxs-lookup"><span data-stu-id="7a8bb-157">Specify password for the user account you specified for the username.</span></span> |<span data-ttu-id="7a8bb-158">Nej</span><span class="sxs-lookup"><span data-stu-id="7a8bb-158">No</span></span> |
| <span data-ttu-id="7a8bb-159">gatewayName</span><span class="sxs-lookup"><span data-stu-id="7a8bb-159">gatewayName</span></span> |<span data-ttu-id="7a8bb-160">Namnet på den gateway som Data Factory-tjänsten ska använda för att ansluta till den lokala Sybase-databasen.</span><span class="sxs-lookup"><span data-stu-id="7a8bb-160">Name of the gateway that the Data Factory service should use to connect to the on-premises Sybase database.</span></span> |<span data-ttu-id="7a8bb-161">Ja</span><span class="sxs-lookup"><span data-stu-id="7a8bb-161">Yes</span></span> |

## <a name="dataset-properties"></a><span data-ttu-id="7a8bb-162">Egenskaper för datamängd</span><span class="sxs-lookup"><span data-stu-id="7a8bb-162">Dataset properties</span></span>
<span data-ttu-id="7a8bb-163">En fullständig lista över egenskaper som är tillgängliga för att definiera datauppsättningarna & avsnitt finns i [skapa datauppsättningar](data-factory-create-datasets.md) artikel.</span><span class="sxs-lookup"><span data-stu-id="7a8bb-163">For a full list of sections & properties available for defining datasets, see the [Creating datasets](data-factory-create-datasets.md) article.</span></span> <span data-ttu-id="7a8bb-164">Avsnitt som struktur, tillgänglighet och princip på en datamängd JSON är liknande för alla typer av dataset (Azure SQL Azure blob, Azure-tabellen, osv.).</span><span class="sxs-lookup"><span data-stu-id="7a8bb-164">Sections such as structure, availability, and policy of a dataset JSON are similar for all dataset types (Azure SQL, Azure blob, Azure table, etc.).</span></span>

<span data-ttu-id="7a8bb-165">Avsnittet typeProperties är olika för varje typ av dataset och ger information om placeringen av data i datalagret.</span><span class="sxs-lookup"><span data-stu-id="7a8bb-165">The typeProperties section is different for each type of dataset and provides information about the location of the data in the data store.</span></span> <span data-ttu-id="7a8bb-166">Den **typeProperties** avsnittet för dataset av typen **RelationalTable** (som omfattar Sybase dataset) har följande egenskaper:</span><span class="sxs-lookup"><span data-stu-id="7a8bb-166">The **typeProperties** section for dataset of type **RelationalTable** (which includes Sybase dataset) has the following properties:</span></span>

| <span data-ttu-id="7a8bb-167">Egenskap</span><span class="sxs-lookup"><span data-stu-id="7a8bb-167">Property</span></span> | <span data-ttu-id="7a8bb-168">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="7a8bb-168">Description</span></span> | <span data-ttu-id="7a8bb-169">Krävs</span><span class="sxs-lookup"><span data-stu-id="7a8bb-169">Required</span></span> |
| --- | --- | --- |
| <span data-ttu-id="7a8bb-170">tableName</span><span class="sxs-lookup"><span data-stu-id="7a8bb-170">tableName</span></span> |<span data-ttu-id="7a8bb-171">Namnet på tabellen i Sybase-databasinstansen som den länkade tjänsten refererar till.</span><span class="sxs-lookup"><span data-stu-id="7a8bb-171">Name of the table in the Sybase Database instance that linked service refers to.</span></span> |<span data-ttu-id="7a8bb-172">Nej (om **frågan** av **RelationalSource** har angetts)</span><span class="sxs-lookup"><span data-stu-id="7a8bb-172">No (if **query** of **RelationalSource** is specified)</span></span> |

## <a name="copy-activity-properties"></a><span data-ttu-id="7a8bb-173">Kopiera egenskaper för aktivitet</span><span class="sxs-lookup"><span data-stu-id="7a8bb-173">Copy activity properties</span></span>
<span data-ttu-id="7a8bb-174">En fullständig lista över avsnitt & egenskaper som är tillgängliga för att definiera aktiviteter, se [skapar Pipelines](data-factory-create-pipelines.md) artikel.</span><span class="sxs-lookup"><span data-stu-id="7a8bb-174">For a full list of sections & properties available for defining activities, see [Creating Pipelines](data-factory-create-pipelines.md) article.</span></span> <span data-ttu-id="7a8bb-175">Egenskaper som namn, beskrivning, ingående och utgående tabeller och principen är tillgängliga för alla typer av aktiviteter.</span><span class="sxs-lookup"><span data-stu-id="7a8bb-175">Properties such as name, description, input and output tables, and policy are available for all types of activities.</span></span>

<span data-ttu-id="7a8bb-176">De egenskaper som är tillgängliga i avsnittet typeProperties i aktiviteten varierar beroende på varje aktivitetstyp.</span><span class="sxs-lookup"><span data-stu-id="7a8bb-176">Whereas, properties available in the typeProperties section of the activity vary with each activity type.</span></span> <span data-ttu-id="7a8bb-177">För Kopieringsaktivitet kan variera de beroende på vilka typer av datakällor och sänkor.</span><span class="sxs-lookup"><span data-stu-id="7a8bb-177">For Copy activity, they vary depending on the types of sources and sinks.</span></span>

<span data-ttu-id="7a8bb-178">När källan är av typen **RelationalSource** (som omfattar Sybase), följande egenskaper är tillgängliga i **typeProperties** avsnitt:</span><span class="sxs-lookup"><span data-stu-id="7a8bb-178">When the source is of type **RelationalSource** (which includes Sybase), the following properties are available in **typeProperties** section:</span></span>

| <span data-ttu-id="7a8bb-179">Egenskap</span><span class="sxs-lookup"><span data-stu-id="7a8bb-179">Property</span></span> | <span data-ttu-id="7a8bb-180">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="7a8bb-180">Description</span></span> | <span data-ttu-id="7a8bb-181">Tillåtna värden</span><span class="sxs-lookup"><span data-stu-id="7a8bb-181">Allowed values</span></span> | <span data-ttu-id="7a8bb-182">Krävs</span><span class="sxs-lookup"><span data-stu-id="7a8bb-182">Required</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="7a8bb-183">DocumentDB</span><span class="sxs-lookup"><span data-stu-id="7a8bb-183">query</span></span> |<span data-ttu-id="7a8bb-184">Använd anpassad fråga för att läsa data.</span><span class="sxs-lookup"><span data-stu-id="7a8bb-184">Use the custom query to read data.</span></span> |<span data-ttu-id="7a8bb-185">SQL-sträng.</span><span class="sxs-lookup"><span data-stu-id="7a8bb-185">SQL query string.</span></span> <span data-ttu-id="7a8bb-186">Till exempel: Välj * från mytable prefix.</span><span class="sxs-lookup"><span data-stu-id="7a8bb-186">For example: select * from MyTable.</span></span> |<span data-ttu-id="7a8bb-187">Nej (om **tableName** av **dataset** har angetts)</span><span class="sxs-lookup"><span data-stu-id="7a8bb-187">No (if **tableName** of **dataset** is specified)</span></span> |


## <a name="json-example-copy-data-from-sybase-to-azure-blob"></a><span data-ttu-id="7a8bb-188">JSON-exempel: kopiera data från Sybase till Azure-Blob</span><span class="sxs-lookup"><span data-stu-id="7a8bb-188">JSON example: Copy data from Sybase to Azure Blob</span></span>
<span data-ttu-id="7a8bb-189">I följande exempel innehåller exempel JSON definitioner som du kan använda för att skapa en pipeline med [Azure-portalen](data-factory-copy-activity-tutorial-using-azure-portal.md) eller [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md) eller [Azure PowerShell](data-factory-copy-activity-tutorial-using-powershell.md).</span><span class="sxs-lookup"><span data-stu-id="7a8bb-189">The following example provides sample JSON definitions that you can use to create a pipeline by using [Azure portal](data-factory-copy-activity-tutorial-using-azure-portal.md) or [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md) or [Azure PowerShell](data-factory-copy-activity-tutorial-using-powershell.md).</span></span> <span data-ttu-id="7a8bb-190">De visar hur du kopierar data från Sybase-databas till Azure Blob Storage.</span><span class="sxs-lookup"><span data-stu-id="7a8bb-190">They show how to copy data from Sybase database to Azure Blob Storage.</span></span> <span data-ttu-id="7a8bb-191">Dock datan kan kopieras till någon av sänkor anges [här](data-factory-data-movement-activities.md#supported-data-stores-and-formats) med hjälp av aktiviteten kopiera i Azure Data Factory.</span><span class="sxs-lookup"><span data-stu-id="7a8bb-191">However, data can be copied to any of the sinks stated [here](data-factory-data-movement-activities.md#supported-data-stores-and-formats) using the Copy Activity in Azure Data Factory.</span></span>   

<span data-ttu-id="7a8bb-192">Exemplet har följande data factory enheter:</span><span class="sxs-lookup"><span data-stu-id="7a8bb-192">The sample has the following data factory entities:</span></span>

1. <span data-ttu-id="7a8bb-193">En länkad tjänst av typen [OnPremisesSybase](data-factory-onprem-sybase-connector.md#linked-service-properties).</span><span class="sxs-lookup"><span data-stu-id="7a8bb-193">A linked service of type [OnPremisesSybase](data-factory-onprem-sybase-connector.md#linked-service-properties).</span></span>
2. <span data-ttu-id="7a8bb-194">En liked tjänst av typen [AzureStorage](data-factory-azure-blob-connector.md#linked-service-properties).</span><span class="sxs-lookup"><span data-stu-id="7a8bb-194">A liked service of type [AzureStorage](data-factory-azure-blob-connector.md#linked-service-properties).</span></span>
3. <span data-ttu-id="7a8bb-195">Indata [dataset](data-factory-create-datasets.md) av typen [RelationalTable](data-factory-onprem-sybase-connector.md#dataset-properties).</span><span class="sxs-lookup"><span data-stu-id="7a8bb-195">An input [dataset](data-factory-create-datasets.md) of type [RelationalTable](data-factory-onprem-sybase-connector.md#dataset-properties).</span></span>
4. <span data-ttu-id="7a8bb-196">Utdata [dataset](data-factory-create-datasets.md) av typen [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties).</span><span class="sxs-lookup"><span data-stu-id="7a8bb-196">An output [dataset](data-factory-create-datasets.md) of type [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties).</span></span>
5. <span data-ttu-id="7a8bb-197">Den [pipeline](data-factory-create-pipelines.md) med Kopieringsaktiviteten som använder [RelationalSource](data-factory-onprem-sybase-connector.md#copy-activity-properties) och [BlobSink](data-factory-azure-blob-connector.md#copy-activity-properties).</span><span class="sxs-lookup"><span data-stu-id="7a8bb-197">The [pipeline](data-factory-create-pipelines.md) with Copy Activity that uses [RelationalSource](data-factory-onprem-sybase-connector.md#copy-activity-properties) and [BlobSink](data-factory-azure-blob-connector.md#copy-activity-properties).</span></span>

<span data-ttu-id="7a8bb-198">Exemplet kopierar data från ett frågeresultat i Sybase-databas till en blobb varje timme.</span><span class="sxs-lookup"><span data-stu-id="7a8bb-198">The sample copies data from a query result in Sybase database to a blob every hour.</span></span> <span data-ttu-id="7a8bb-199">JSON-egenskaper som används i exemplen beskrivs i exemplen i följande avsnitt.</span><span class="sxs-lookup"><span data-stu-id="7a8bb-199">The JSON properties used in these samples are described in sections following the samples.</span></span>

<span data-ttu-id="7a8bb-200">Som ett första steg bör du konfigurera data management gateway.</span><span class="sxs-lookup"><span data-stu-id="7a8bb-200">As a first step, setup the data management gateway.</span></span> <span data-ttu-id="7a8bb-201">Anvisningarna är i den [flytta data mellan lokala platser och moln](data-factory-move-data-between-onprem-and-cloud.md) artikel.</span><span class="sxs-lookup"><span data-stu-id="7a8bb-201">The instructions are in the [moving data between on-premises locations and cloud](data-factory-move-data-between-onprem-and-cloud.md) article.</span></span>

<span data-ttu-id="7a8bb-202">**Sybase länkade tjänsten:**</span><span class="sxs-lookup"><span data-stu-id="7a8bb-202">**Sybase linked service:**</span></span>

```JSON
{
    "name": "OnPremSybaseLinkedService",
    "properties": {
        "type": "OnPremisesSybase",
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

<span data-ttu-id="7a8bb-203">**Azure Blob storage länkade tjänsten:**</span><span class="sxs-lookup"><span data-stu-id="7a8bb-203">**Azure Blob storage linked service:**</span></span>

```JSON
{
    "name": "AzureStorageLinkedService",
    "properties": {
        "type": "AzureStorageLinkedService",
        "typeProperties": {
            "connectionString": "DefaultEndpointsProtocol=https;AccountName=<AccountName>;AccountKey=<AccountKey>"
        }
    }
}
```

<span data-ttu-id="7a8bb-204">**Sybase inkommande datauppsättningen:**</span><span class="sxs-lookup"><span data-stu-id="7a8bb-204">**Sybase input dataset:**</span></span>

<span data-ttu-id="7a8bb-205">Exemplet förutsätter att du har skapat en tabell ”mytable” som prefix i Sybase och innehåller en kolumn med namnet ”tidsstämpel” för tid series-data.</span><span class="sxs-lookup"><span data-stu-id="7a8bb-205">The sample assumes you have created a table “MyTable” in Sybase and it contains a column called “timestamp” for time series data.</span></span>

<span data-ttu-id="7a8bb-206">Inställningen ”externa”: true informerar Data Factory-tjänsten att den här datauppsättningen är extern till data factory och inte tillverkas av en aktivitet i datafabriken.</span><span class="sxs-lookup"><span data-stu-id="7a8bb-206">Setting “external”: true informs the Data Factory service that this dataset is external to the data factory and is not produced by an activity in the data factory.</span></span> <span data-ttu-id="7a8bb-207">Observera att den **typen** på den länkade tjänsten är inställd på: **RelationalTable**.</span><span class="sxs-lookup"><span data-stu-id="7a8bb-207">Notice that the **type** of the linked service is set to: **RelationalTable**.</span></span>

```JSON
{
    "name": "SybaseDataSet",
    "properties": {
        "type": "RelationalTable",
        "linkedServiceName": "OnPremSybaseLinkedService",
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

<span data-ttu-id="7a8bb-208">**Azure Blob utdatauppsättningen:**</span><span class="sxs-lookup"><span data-stu-id="7a8bb-208">**Azure Blob output dataset:**</span></span>

<span data-ttu-id="7a8bb-209">Data skrivs till en ny blob varje timme (frekvens: timme, intervall: 1).</span><span class="sxs-lookup"><span data-stu-id="7a8bb-209">Data is written to a new blob every hour (frequency: hour, interval: 1).</span></span> <span data-ttu-id="7a8bb-210">Sökvägen till mappen för blobben utvärderas dynamiskt baserat på starttiden för den sektor som bearbetas.</span><span class="sxs-lookup"><span data-stu-id="7a8bb-210">The folder path for the blob is dynamically evaluated based on the start time of the slice that is being processed.</span></span> <span data-ttu-id="7a8bb-211">Mappsökvägen använder år, månad, dag och timmar delar av starttiden.</span><span class="sxs-lookup"><span data-stu-id="7a8bb-211">The folder path uses year, month, day, and hours parts of the start time.</span></span>

```JSON
{
    "name": "AzureBlobSybaseDataSet",
    "properties": {
        "type": "AzureBlob",
        "linkedServiceName": "AzureStorageLinkedService",
        "typeProperties": {
            "folderPath": "mycontainer/sybase/yearno={Year}/monthno={Month}/dayno={Day}/hourno={Hour}",
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

<span data-ttu-id="7a8bb-212">**Pipeline med kopieringsaktiviteten:**</span><span class="sxs-lookup"><span data-stu-id="7a8bb-212">**Pipeline with Copy activity:**</span></span>

<span data-ttu-id="7a8bb-213">Pipelinen innehåller en kopia-aktivitet som är konfigurerad för att använda indata och utdata-datauppsättningar och har schemalagts att köras varje timme.</span><span class="sxs-lookup"><span data-stu-id="7a8bb-213">The pipeline contains a Copy Activity that is configured to use the input and output datasets and is scheduled to run hourly.</span></span> <span data-ttu-id="7a8bb-214">I pipeline-JSON-definitionen av **källa** är inställd på **RelationalSource** och **sink** är inställd på **BlobSink**.</span><span class="sxs-lookup"><span data-stu-id="7a8bb-214">In the pipeline JSON definition, the **source** type is set to **RelationalSource** and **sink** type is set to **BlobSink**.</span></span> <span data-ttu-id="7a8bb-215">SQL-frågan som angetts för den **frågan** egenskapen väljer vilka data från DBA. Order tabell i databasen.</span><span class="sxs-lookup"><span data-stu-id="7a8bb-215">The SQL query specified for the **query** property selects the data from the DBA.Orders table in the database.</span></span>

```JSON
{
    "name": "CopySybaseToBlob",
    "properties": {
        "description": "pipeline for copy activity",
        "activities": [
            {
                "type": "Copy",
                "typeProperties": {
                    "source": {
                        "type": "RelationalSource",
                        "query": "select * from DBA.Orders"
                    },
                    "sink": {
                        "type": "BlobSink"
                    }
                },
                "inputs": [
                    {
                        "name": "SybaseDataSet"
                    }
                ],
                "outputs": [
                    {
                        "name": "AzureBlobSybaseDataSet"
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
                "name": "SybaseToBlob"
            }
        ],
        "start": "2014-06-01T18:00:00Z",
        "end": "2014-06-01T19:00:00Z"
    }
}
```

## <a name="type-mapping-for-sybase"></a><span data-ttu-id="7a8bb-216">Mappning för Sybase</span><span class="sxs-lookup"><span data-stu-id="7a8bb-216">Type mapping for Sybase</span></span>
<span data-ttu-id="7a8bb-217">Som anges i den [Data Movement aktiviteter](data-factory-data-movement-activities.md) artikel, kopieringsaktiviteten utför automatisk konverteringar från källtyper att registrera typer med följande metod i steg 2:</span><span class="sxs-lookup"><span data-stu-id="7a8bb-217">As mentioned in the [Data Movement Activities](data-factory-data-movement-activities.md) article, the Copy activity performs automatic type conversions from source types to sink types with the following 2-step approach:</span></span>

1. <span data-ttu-id="7a8bb-218">Konvertera från interna källtyper till .NET-typ</span><span class="sxs-lookup"><span data-stu-id="7a8bb-218">Convert from native source types to .NET type</span></span>
2. <span data-ttu-id="7a8bb-219">Konvertera från .NET-typ till interna mottagare typ.</span><span class="sxs-lookup"><span data-stu-id="7a8bb-219">Convert from .NET type to native sink type</span></span>

<span data-ttu-id="7a8bb-220">Sybase stöder T-SQL och T-SQL-typer.</span><span class="sxs-lookup"><span data-stu-id="7a8bb-220">Sybase supports T-SQL and T-SQL types.</span></span> <span data-ttu-id="7a8bb-221">En Mappningstabell från sql-typer till .NET-typ, finns [Azure SQL Connector](data-factory-azure-sql-connector.md) artikel.</span><span class="sxs-lookup"><span data-stu-id="7a8bb-221">For a mapping table from sql types to .NET type, see [Azure SQL Connector](data-factory-azure-sql-connector.md) article.</span></span>

## <a name="map-source-to-sink-columns"></a><span data-ttu-id="7a8bb-222">Karta källan till mottagare för kolumner</span><span class="sxs-lookup"><span data-stu-id="7a8bb-222">Map source to sink columns</span></span>
<span data-ttu-id="7a8bb-223">Mer information om mappning kolumner i datauppsättningen källan till kolumner i datauppsättning mottagare, se [mappa dataset kolumner i Azure Data Factory](data-factory-map-columns.md).</span><span class="sxs-lookup"><span data-stu-id="7a8bb-223">To learn about mapping columns in source dataset to columns in sink dataset, see [Mapping dataset columns in Azure Data Factory](data-factory-map-columns.md).</span></span>

## <a name="repeatable-read-from-relational-sources"></a><span data-ttu-id="7a8bb-224">Upprepbar läsning från relationella källor</span><span class="sxs-lookup"><span data-stu-id="7a8bb-224">Repeatable read from relational sources</span></span>
<span data-ttu-id="7a8bb-225">Tänk på att undvika oväntade resultat repeterbarhet när kopiering av data från relationella data lagras.</span><span class="sxs-lookup"><span data-stu-id="7a8bb-225">When copying data from relational data stores, keep repeatability in mind to avoid unintended outcomes.</span></span> <span data-ttu-id="7a8bb-226">I Azure Data Factory, kan du köra en sektor manuellt.</span><span class="sxs-lookup"><span data-stu-id="7a8bb-226">In Azure Data Factory, you can rerun a slice manually.</span></span> <span data-ttu-id="7a8bb-227">Du kan också konfigurera i principen för en dataset så att ett segment som körs när ett fel uppstår.</span><span class="sxs-lookup"><span data-stu-id="7a8bb-227">You can also configure retry policy for a dataset so that a slice is rerun when a failure occurs.</span></span> <span data-ttu-id="7a8bb-228">När ett segment körs på något sätt, måste du kontrollera att samma data läses oavsett hur många gånger ett segment körs.</span><span class="sxs-lookup"><span data-stu-id="7a8bb-228">When a slice is rerun in either way, you need to make sure that the same data is read no matter how many times a slice is run.</span></span> <span data-ttu-id="7a8bb-229">Se [Repeatable läsa från relationella källor](data-factory-repeatable-copy.md#repeatable-read-from-relational-sources).</span><span class="sxs-lookup"><span data-stu-id="7a8bb-229">See [Repeatable read from relational sources](data-factory-repeatable-copy.md#repeatable-read-from-relational-sources).</span></span>

## <a name="performance-and-tuning"></a><span data-ttu-id="7a8bb-230">Prestanda och finjustering</span><span class="sxs-lookup"><span data-stu-id="7a8bb-230">Performance and Tuning</span></span>
<span data-ttu-id="7a8bb-231">Se [kopiera aktivitet prestanda och justera guiden](data-factory-copy-activity-performance.md) vill veta mer om viktiga faktorer som påverkan prestanda för flytt av data (Kopieringsaktiviteten) i Azure Data Factory och olika sätt att optimera den.</span><span class="sxs-lookup"><span data-stu-id="7a8bb-231">See [Copy Activity Performance & Tuning Guide](data-factory-copy-activity-performance.md) to learn about key factors that impact performance of data movement (Copy Activity) in Azure Data Factory and various ways to optimize it.</span></span>
