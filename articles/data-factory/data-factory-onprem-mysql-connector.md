---
title: "Flytta data från MySQL med Azure Data Factory | Microsoft Docs"
description: "Läs mer om hur du flyttar data från MySQL-databas med hjälp av Azure Data Factory."
services: data-factory
documentationcenter: 
author: linda33wj
manager: jhubbard
editor: monicar
ms.assetid: 452f4fce-9eb5-40a0-92f8-1e98691bea4c
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/15/2017
ms.author: jingwang
ms.openlocfilehash: 05159bfd98977d0b57b43fbc02e4579439f7ce4c
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="move-data-from-mysql-using-azure-data-factory"></a><span data-ttu-id="a6437-103">Flytta data från MySQL med Azure Data Factory</span><span class="sxs-lookup"><span data-stu-id="a6437-103">Move data From MySQL using Azure Data Factory</span></span>
<span data-ttu-id="a6437-104">Den här artikeln förklarar hur du använder aktiviteten kopiera i Azure Data Factory för att flytta data från en lokal MySQL-databas.</span><span class="sxs-lookup"><span data-stu-id="a6437-104">This article explains how to use the Copy Activity in Azure Data Factory to move data from an on-premises MySQL database.</span></span> <span data-ttu-id="a6437-105">Den bygger på den [Data Movement aktiviteter](data-factory-data-movement-activities.md) artikel som presenterar en allmän översikt över dataflyttning med copy-aktivitet.</span><span class="sxs-lookup"><span data-stu-id="a6437-105">It builds on the [Data Movement Activities](data-factory-data-movement-activities.md) article, which presents a general overview of data movement with the copy activity.</span></span>

<span data-ttu-id="a6437-106">Du kan kopiera data från ett dataarkiv för lokala MySQL till alla stöds sink-datalagret.</span><span class="sxs-lookup"><span data-stu-id="a6437-106">You can copy data from an on-premises MySQL data store to any supported sink data store.</span></span> <span data-ttu-id="a6437-107">En lista över datakällor som stöds som sänkor av kopieringsaktiviteten, finns det [stöds datalager](data-factory-data-movement-activities.md#supported-data-stores-and-formats) tabell.</span><span class="sxs-lookup"><span data-stu-id="a6437-107">For a list of data stores supported as sinks by the copy activity, see the [Supported data stores](data-factory-data-movement-activities.md#supported-data-stores-and-formats) table.</span></span> <span data-ttu-id="a6437-108">Data factory stöder för närvarande endast flytta data från en MySQL-databas till andra databaser, men inte för att flytta data från andra datalager till en MySQL-databasen.</span><span class="sxs-lookup"><span data-stu-id="a6437-108">Data factory currently supports only moving data from a MySQL data store to other data stores, but not for moving data from other data stores to an MySQL data store.</span></span> 

## <a name="prerequisites"></a><span data-ttu-id="a6437-109">Krav</span><span class="sxs-lookup"><span data-stu-id="a6437-109">Prerequisites</span></span>
<span data-ttu-id="a6437-110">Data Factory-tjänsten stöder anslutning till lokala MySQL källor med hjälp av Data Management Gateway.</span><span class="sxs-lookup"><span data-stu-id="a6437-110">Data Factory service supports connecting to on-premises MySQL sources using the Data Management Gateway.</span></span> <span data-ttu-id="a6437-111">Se [flytta data mellan lokala platser och moln](data-factory-move-data-between-onprem-and-cloud.md) artikeln innehåller information om Data Management Gateway och stegvisa instruktioner om hur du konfigurerar en gateway.</span><span class="sxs-lookup"><span data-stu-id="a6437-111">See [moving data between on-premises locations and cloud](data-factory-move-data-between-onprem-and-cloud.md) article to learn about Data Management Gateway and step-by-step instructions on setting up the gateway.</span></span>

<span data-ttu-id="a6437-112">Gateway krävs även om MySQL-databasen finns i en Azure IaaS-virtuella (VM).</span><span class="sxs-lookup"><span data-stu-id="a6437-112">Gateway is required even if the MySQL database is hosted in an Azure IaaS virtual machine (VM).</span></span> <span data-ttu-id="a6437-113">Du kan installera gatewayen på samma virtuella dator som dataarkiv eller på en annan virtuell dator, förutsatt att gatewayen kan ansluta till databasen.</span><span class="sxs-lookup"><span data-stu-id="a6437-113">You can install the gateway on the same VM as the data store or on a different VM as long as the gateway can connect to the database.</span></span>

> [!NOTE]
> <span data-ttu-id="a6437-114">Se [felsökning av problem med gateway](data-factory-data-management-gateway.md#troubleshooting-gateway-issues) tips om hur du felsöker anslutning /-gateway relaterade problem.</span><span class="sxs-lookup"><span data-stu-id="a6437-114">See [Troubleshoot gateway issues](data-factory-data-management-gateway.md#troubleshooting-gateway-issues) for tips on troubleshooting connection/gateway related issues.</span></span>

## <a name="supported-versions-and-installation"></a><span data-ttu-id="a6437-115">Versioner som stöds och installation</span><span class="sxs-lookup"><span data-stu-id="a6437-115">Supported versions and installation</span></span>
<span data-ttu-id="a6437-116">För Data Management Gateway att ansluta till MySQL-databas, måste du installera den [MySQL Connector/Net för Microsoft Windows](https://dev.mysql.com/downloads/connector/net/) (version 6.6.5 eller senare) på samma system som Data Management Gateway.</span><span class="sxs-lookup"><span data-stu-id="a6437-116">For Data Management Gateway to connect to the MySQL Database, you need to install the [MySQL Connector/Net for Microsoft Windows](https://dev.mysql.com/downloads/connector/net/) (version 6.6.5 or above) on the same system as the Data Management Gateway.</span></span> <span data-ttu-id="a6437-117">MySQL version 5.1 och senare stöds.</span><span class="sxs-lookup"><span data-stu-id="a6437-117">MySQL version 5.1 and above is supported.</span></span>

> [!TIP]
> <span data-ttu-id="a6437-118">Om du klickar på fel i ”autentisering misslyckades eftersom Fjärrpartnern har stängt transport dataströmmen”., bör du uppgradera MySQL Connector/Net till en senare version.</span><span class="sxs-lookup"><span data-stu-id="a6437-118">If you hit error on "Authentication failed because the remote party has closed the transport stream.", consider to upgrade the MySQL Connector/Net to higher version.</span></span>

## <a name="getting-started"></a><span data-ttu-id="a6437-119">Komma igång</span><span class="sxs-lookup"><span data-stu-id="a6437-119">Getting started</span></span>
<span data-ttu-id="a6437-120">Du kan skapa en pipeline med en kopia-aktivitet som flyttar data från en lokal Cassandra data store med hjälp av olika verktyg/API: er.</span><span class="sxs-lookup"><span data-stu-id="a6437-120">You can create a pipeline with a copy activity that moves data from an on-premises Cassandra data store by using different tools/APIs.</span></span> 

- <span data-ttu-id="a6437-121">Det enklaste sättet att skapa en pipeline är att använda den **guiden Kopiera**.</span><span class="sxs-lookup"><span data-stu-id="a6437-121">The easiest way to create a pipeline is to use the **Copy Wizard**.</span></span> <span data-ttu-id="a6437-122">Finns [Självstudier: skapa en pipeline med hjälp av guiden Kopiera](data-factory-copy-data-wizard-tutorial.md) för en snabb genomgång om hur du skapar en pipeline med hjälp av guiden Kopiera data.</span><span class="sxs-lookup"><span data-stu-id="a6437-122">See [Tutorial: Create a pipeline using Copy Wizard](data-factory-copy-data-wizard-tutorial.md) for a quick walkthrough on creating a pipeline using the Copy data wizard.</span></span> 
- <span data-ttu-id="a6437-123">Du kan också använda följande verktyg för att skapa en pipeline: **Azure-portalen**, **Visual Studio**, **Azure PowerShell**, **Azure Resource Manager-mall**, **.NET API**, och **REST API**.</span><span class="sxs-lookup"><span data-stu-id="a6437-123">You can also use the following tools to create a pipeline: **Azure portal**, **Visual Studio**, **Azure PowerShell**, **Azure Resource Manager template**, **.NET API**, and **REST API**.</span></span> <span data-ttu-id="a6437-124">Se [kopiera aktivitet kursen](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) för stegvisa instruktioner för att skapa en pipeline med en Kopieringsaktivitet.</span><span class="sxs-lookup"><span data-stu-id="a6437-124">See [Copy activity tutorial](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) for step-by-step instructions to create a pipeline with a copy activity.</span></span> 

<span data-ttu-id="a6437-125">Om du använder verktyg eller API: er, kan du utföra följande steg för att skapa en pipeline som flyttar data från ett dataarkiv som källa till ett dataarkiv som mottagare:</span><span class="sxs-lookup"><span data-stu-id="a6437-125">Whether you use the tools or APIs, you perform the following steps to create a pipeline that moves data from a source data store to a sink data store:</span></span>

1. <span data-ttu-id="a6437-126">Skapa **länkade tjänster** att länka inkommande och utgående data lagras till din data factory.</span><span class="sxs-lookup"><span data-stu-id="a6437-126">Create **linked services** to link input and output data stores to your data factory.</span></span>
2. <span data-ttu-id="a6437-127">Skapa **datauppsättningar** att representera inkommande och utgående data för kopieringen.</span><span class="sxs-lookup"><span data-stu-id="a6437-127">Create **datasets** to represent input and output data for the copy operation.</span></span> 
3. <span data-ttu-id="a6437-128">Skapa en **pipeline** med en kopia-aktivitet som tar en datamängd som indata och en dataset som utdata.</span><span class="sxs-lookup"><span data-stu-id="a6437-128">Create a **pipeline** with a copy activity that takes a dataset as an input and a dataset as an output.</span></span> 

<span data-ttu-id="a6437-129">När du använder guiden skapas JSON definitioner för dessa Data Factory-enheter (länkade tjänster, datauppsättningar och pipelinen) automatiskt för dig.</span><span class="sxs-lookup"><span data-stu-id="a6437-129">When you use the wizard, JSON definitions for these Data Factory entities (linked services, datasets, and the pipeline) are automatically created for you.</span></span> <span data-ttu-id="a6437-130">När du använder Verktyg/API: er (utom .NET API), kan du definiera dessa Data Factory-enheter med hjälp av JSON-format.</span><span class="sxs-lookup"><span data-stu-id="a6437-130">When you use tools/APIs (except .NET API), you define these Data Factory entities by using the JSON format.</span></span>  <span data-ttu-id="a6437-131">Ett exempel med JSON-definitioner för Data Factory-entiteter som används för att kopiera data från ett dataarkiv för lokala MySQL finns [JSON-exempel: kopiera data från MySQL till Azure Blob](#json-example-copy-data-from-mysql-to-azure-blob) i den här artikeln.</span><span class="sxs-lookup"><span data-stu-id="a6437-131">For a sample with JSON definitions for Data Factory entities that are used to copy data from an on-premises MySQL data store, see [JSON example: Copy data from MySQL to Azure Blob](#json-example-copy-data-from-mysql-to-azure-blob) section of this article.</span></span> 

<span data-ttu-id="a6437-132">Följande avsnitt innehåller information om JSON-egenskaper som används för att definiera Data Factory entiteter i en MySQL-datalager:</span><span class="sxs-lookup"><span data-stu-id="a6437-132">The following sections provide details about JSON properties that are used to define Data Factory entities specific to a MySQL data store:</span></span>

## <a name="linked-service-properties"></a><span data-ttu-id="a6437-133">Länkad tjänstegenskaper</span><span class="sxs-lookup"><span data-stu-id="a6437-133">Linked service properties</span></span>
<span data-ttu-id="a6437-134">Följande tabell innehåller en beskrivning för JSON-element som är specifika för MySQL länkad tjänst.</span><span class="sxs-lookup"><span data-stu-id="a6437-134">The following table provides description for JSON elements specific to MySQL linked service.</span></span>

| <span data-ttu-id="a6437-135">Egenskap</span><span class="sxs-lookup"><span data-stu-id="a6437-135">Property</span></span> | <span data-ttu-id="a6437-136">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="a6437-136">Description</span></span> | <span data-ttu-id="a6437-137">Krävs</span><span class="sxs-lookup"><span data-stu-id="a6437-137">Required</span></span> |
| --- | --- | --- |
| <span data-ttu-id="a6437-138">typ</span><span class="sxs-lookup"><span data-stu-id="a6437-138">type</span></span> |<span data-ttu-id="a6437-139">Egenskapen type måste anges till: **OnPremisesMySql**</span><span class="sxs-lookup"><span data-stu-id="a6437-139">The type property must be set to: **OnPremisesMySql**</span></span> |<span data-ttu-id="a6437-140">Ja</span><span class="sxs-lookup"><span data-stu-id="a6437-140">Yes</span></span> |
| <span data-ttu-id="a6437-141">server</span><span class="sxs-lookup"><span data-stu-id="a6437-141">server</span></span> |<span data-ttu-id="a6437-142">Namnet på MySQL-servern.</span><span class="sxs-lookup"><span data-stu-id="a6437-142">Name of the MySQL server.</span></span> |<span data-ttu-id="a6437-143">Ja</span><span class="sxs-lookup"><span data-stu-id="a6437-143">Yes</span></span> |
| <span data-ttu-id="a6437-144">Databasen</span><span class="sxs-lookup"><span data-stu-id="a6437-144">database</span></span> |<span data-ttu-id="a6437-145">Namnet på MySQL-databas.</span><span class="sxs-lookup"><span data-stu-id="a6437-145">Name of the MySQL database.</span></span> |<span data-ttu-id="a6437-146">Ja</span><span class="sxs-lookup"><span data-stu-id="a6437-146">Yes</span></span> |
| <span data-ttu-id="a6437-147">Schemat</span><span class="sxs-lookup"><span data-stu-id="a6437-147">schema</span></span> |<span data-ttu-id="a6437-148">Namnet på schemat i databasen.</span><span class="sxs-lookup"><span data-stu-id="a6437-148">Name of the schema in the database.</span></span> |<span data-ttu-id="a6437-149">Nej</span><span class="sxs-lookup"><span data-stu-id="a6437-149">No</span></span> |
| <span data-ttu-id="a6437-150">AuthenticationType</span><span class="sxs-lookup"><span data-stu-id="a6437-150">authenticationType</span></span> |<span data-ttu-id="a6437-151">Typ av autentisering som används för att ansluta till MySQL-databas.</span><span class="sxs-lookup"><span data-stu-id="a6437-151">Type of authentication used to connect to the MySQL database.</span></span> <span data-ttu-id="a6437-152">Möjliga värden är: `Basic`.</span><span class="sxs-lookup"><span data-stu-id="a6437-152">Possible values are: `Basic`.</span></span> |<span data-ttu-id="a6437-153">Ja</span><span class="sxs-lookup"><span data-stu-id="a6437-153">Yes</span></span> |
| <span data-ttu-id="a6437-154">användarnamn</span><span class="sxs-lookup"><span data-stu-id="a6437-154">username</span></span> |<span data-ttu-id="a6437-155">Ange användarnamn för att ansluta till MySQL-databas.</span><span class="sxs-lookup"><span data-stu-id="a6437-155">Specify user name to connect to the MySQL database.</span></span> |<span data-ttu-id="a6437-156">Ja</span><span class="sxs-lookup"><span data-stu-id="a6437-156">Yes</span></span> |
| <span data-ttu-id="a6437-157">lösenord</span><span class="sxs-lookup"><span data-stu-id="a6437-157">password</span></span> |<span data-ttu-id="a6437-158">Ange lösenordet för det användarkonto som du angett.</span><span class="sxs-lookup"><span data-stu-id="a6437-158">Specify password for the user account you specified.</span></span> |<span data-ttu-id="a6437-159">Ja</span><span class="sxs-lookup"><span data-stu-id="a6437-159">Yes</span></span> |
| <span data-ttu-id="a6437-160">gatewayName</span><span class="sxs-lookup"><span data-stu-id="a6437-160">gatewayName</span></span> |<span data-ttu-id="a6437-161">Namnet på den gateway som Data Factory-tjänsten ska använda för att ansluta till den lokala MySQL-databasen.</span><span class="sxs-lookup"><span data-stu-id="a6437-161">Name of the gateway that the Data Factory service should use to connect to the on-premises MySQL database.</span></span> |<span data-ttu-id="a6437-162">Ja</span><span class="sxs-lookup"><span data-stu-id="a6437-162">Yes</span></span> |

## <a name="dataset-properties"></a><span data-ttu-id="a6437-163">Egenskaper för datamängd</span><span class="sxs-lookup"><span data-stu-id="a6437-163">Dataset properties</span></span>
<span data-ttu-id="a6437-164">En fullständig lista över egenskaper som är tillgängliga för att definiera datauppsättningarna & avsnitt finns i [skapa datauppsättningar](data-factory-create-datasets.md) artikel.</span><span class="sxs-lookup"><span data-stu-id="a6437-164">For a full list of sections & properties available for defining datasets, see the [Creating datasets](data-factory-create-datasets.md) article.</span></span> <span data-ttu-id="a6437-165">Avsnitt som struktur, tillgänglighet och princip på en datamängd JSON är liknande för alla typer av dataset (Azure SQL Azure blob, Azure-tabellen, osv.).</span><span class="sxs-lookup"><span data-stu-id="a6437-165">Sections such as structure, availability, and policy of a dataset JSON are similar for all dataset types (Azure SQL, Azure blob, Azure table, etc.).</span></span>

<span data-ttu-id="a6437-166">Den **typeProperties** avsnitt är olika för varje typ av dataset och innehåller information om placeringen av data i datalagret.</span><span class="sxs-lookup"><span data-stu-id="a6437-166">The **typeProperties** section is different for each type of dataset and provides information about the location of the data in the data store.</span></span> <span data-ttu-id="a6437-167">TypeProperties avsnittet för dataset av typen **RelationalTable** (som omfattar MySQL dataset) har följande egenskaper</span><span class="sxs-lookup"><span data-stu-id="a6437-167">The typeProperties section for dataset of type **RelationalTable** (which includes MySQL dataset) has the following properties</span></span>

| <span data-ttu-id="a6437-168">Egenskap</span><span class="sxs-lookup"><span data-stu-id="a6437-168">Property</span></span> | <span data-ttu-id="a6437-169">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="a6437-169">Description</span></span> | <span data-ttu-id="a6437-170">Krävs</span><span class="sxs-lookup"><span data-stu-id="a6437-170">Required</span></span> |
| --- | --- | --- |
| <span data-ttu-id="a6437-171">tableName</span><span class="sxs-lookup"><span data-stu-id="a6437-171">tableName</span></span> |<span data-ttu-id="a6437-172">Namnet på tabellen i MySQL-databasinstans som den länkade tjänsten refererar till.</span><span class="sxs-lookup"><span data-stu-id="a6437-172">Name of the table in the MySQL Database instance that linked service refers to.</span></span> |<span data-ttu-id="a6437-173">Nej (om **frågan** av **RelationalSource** har angetts)</span><span class="sxs-lookup"><span data-stu-id="a6437-173">No (if **query** of **RelationalSource** is specified)</span></span> |

## <a name="copy-activity-properties"></a><span data-ttu-id="a6437-174">Kopiera egenskaper för aktivitet</span><span class="sxs-lookup"><span data-stu-id="a6437-174">Copy activity properties</span></span>
<span data-ttu-id="a6437-175">En fullständig lista över avsnitt & egenskaper som är tillgängliga för att definiera aktiviteter finns i [skapar Pipelines](data-factory-create-pipelines.md) artikel.</span><span class="sxs-lookup"><span data-stu-id="a6437-175">For a full list of sections & properties available for defining activities, see the [Creating Pipelines](data-factory-create-pipelines.md) article.</span></span> <span data-ttu-id="a6437-176">Egenskaper, till exempel namn, beskrivning, inkommande och utgående tabeller är principer är tillgängliga för alla typer av aktiviteter.</span><span class="sxs-lookup"><span data-stu-id="a6437-176">Properties such as name, description, input and output tables, are policies are available for all types of activities.</span></span>

<span data-ttu-id="a6437-177">Medan egenskaper som är tillgängliga i den **typeProperties** avsnitt i aktiviteten varierar med varje aktivitetstyp.</span><span class="sxs-lookup"><span data-stu-id="a6437-177">Whereas, properties available in the **typeProperties** section of the activity vary with each activity type.</span></span> <span data-ttu-id="a6437-178">För Kopieringsaktivitet kan variera de beroende på vilka typer av datakällor och sänkor.</span><span class="sxs-lookup"><span data-stu-id="a6437-178">For Copy activity, they vary depending on the types of sources and sinks.</span></span>

<span data-ttu-id="a6437-179">När datakällan i en Kopieringsaktivitet är av typen **RelationalSource** (som omfattar MySQL), följande egenskaper finns i avsnittet typeProperties:</span><span class="sxs-lookup"><span data-stu-id="a6437-179">When source in copy activity is of type **RelationalSource** (which includes MySQL), the following properties are available in typeProperties section:</span></span>

| <span data-ttu-id="a6437-180">Egenskap</span><span class="sxs-lookup"><span data-stu-id="a6437-180">Property</span></span> | <span data-ttu-id="a6437-181">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="a6437-181">Description</span></span> | <span data-ttu-id="a6437-182">Tillåtna värden</span><span class="sxs-lookup"><span data-stu-id="a6437-182">Allowed values</span></span> | <span data-ttu-id="a6437-183">Krävs</span><span class="sxs-lookup"><span data-stu-id="a6437-183">Required</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="a6437-184">DocumentDB</span><span class="sxs-lookup"><span data-stu-id="a6437-184">query</span></span> |<span data-ttu-id="a6437-185">Använd anpassad fråga för att läsa data.</span><span class="sxs-lookup"><span data-stu-id="a6437-185">Use the custom query to read data.</span></span> |<span data-ttu-id="a6437-186">SQL-sträng.</span><span class="sxs-lookup"><span data-stu-id="a6437-186">SQL query string.</span></span> <span data-ttu-id="a6437-187">Till exempel: Välj * från mytable prefix.</span><span class="sxs-lookup"><span data-stu-id="a6437-187">For example: select * from MyTable.</span></span> |<span data-ttu-id="a6437-188">Nej (om **tableName** av **dataset** har angetts)</span><span class="sxs-lookup"><span data-stu-id="a6437-188">No (if **tableName** of **dataset** is specified)</span></span> |


## <a name="json-example-copy-data-from-mysql-to-azure-blob"></a><span data-ttu-id="a6437-189">JSON-exempel: kopiera data från MySQL till Azure-Blob</span><span class="sxs-lookup"><span data-stu-id="a6437-189">JSON example: Copy data from MySQL to Azure Blob</span></span>
<span data-ttu-id="a6437-190">Det här exemplet innehåller exempel JSON definitioner som du kan använda för att skapa en pipeline med [Azure-portalen](data-factory-copy-activity-tutorial-using-azure-portal.md) eller [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md) eller [Azure PowerShell](data-factory-copy-activity-tutorial-using-powershell.md).</span><span class="sxs-lookup"><span data-stu-id="a6437-190">This example provides sample JSON definitions that you can use to create a pipeline by using [Azure portal](data-factory-copy-activity-tutorial-using-azure-portal.md) or [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md) or [Azure PowerShell](data-factory-copy-activity-tutorial-using-powershell.md).</span></span> <span data-ttu-id="a6437-191">Den visar hur du kopierar data från en lokal MySQL-databas till en Azure Blob Storage.</span><span class="sxs-lookup"><span data-stu-id="a6437-191">It shows how to copy data from an on-premises MySQL database to an Azure Blob Storage.</span></span> <span data-ttu-id="a6437-192">Dock datan kan kopieras till någon av sänkor anges [här](data-factory-data-movement-activities.md#supported-data-stores-and-formats) med hjälp av aktiviteten kopiera i Azure Data Factory.</span><span class="sxs-lookup"><span data-stu-id="a6437-192">However, data can be copied to any of the sinks stated [here](data-factory-data-movement-activities.md#supported-data-stores-and-formats) using the Copy Activity in Azure Data Factory.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="a6437-193">Det här exemplet innehåller JSON kodavsnitt.</span><span class="sxs-lookup"><span data-stu-id="a6437-193">This sample provides JSON snippets.</span></span> <span data-ttu-id="a6437-194">Stegvisa instruktioner för att skapa datafabriken inkluderas inte.</span><span class="sxs-lookup"><span data-stu-id="a6437-194">It does not include step-by-step instructions for creating the data factory.</span></span> <span data-ttu-id="a6437-195">Se [flytta data mellan lokala platser och moln](data-factory-move-data-between-onprem-and-cloud.md) artikel stegvisa instruktioner.</span><span class="sxs-lookup"><span data-stu-id="a6437-195">See [moving data between on-premises locations and cloud](data-factory-move-data-between-onprem-and-cloud.md) article for step-by-step instructions.</span></span>

<span data-ttu-id="a6437-196">Exemplet har följande data factory enheter:</span><span class="sxs-lookup"><span data-stu-id="a6437-196">The sample has the following data factory entities:</span></span>

1. <span data-ttu-id="a6437-197">En länkad tjänst av typen [OnPremisesMySql](data-factory-onprem-mysql-connector.md#linked-service-properties).</span><span class="sxs-lookup"><span data-stu-id="a6437-197">A linked service of type [OnPremisesMySql](data-factory-onprem-mysql-connector.md#linked-service-properties).</span></span>
2. <span data-ttu-id="a6437-198">En länkad tjänst av typen [AzureStorage](data-factory-azure-blob-connector.md#linked-service-properties).</span><span class="sxs-lookup"><span data-stu-id="a6437-198">A linked service of type [AzureStorage](data-factory-azure-blob-connector.md#linked-service-properties).</span></span>
3. <span data-ttu-id="a6437-199">Indata [dataset](data-factory-create-datasets.md) av typen [RelationalTable](data-factory-onprem-mysql-connector.md#dataset-properties).</span><span class="sxs-lookup"><span data-stu-id="a6437-199">An input [dataset](data-factory-create-datasets.md) of type [RelationalTable](data-factory-onprem-mysql-connector.md#dataset-properties).</span></span>
4. <span data-ttu-id="a6437-200">Utdata [dataset](data-factory-create-datasets.md) av typen [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties).</span><span class="sxs-lookup"><span data-stu-id="a6437-200">An output [dataset](data-factory-create-datasets.md) of type [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties).</span></span>
5. <span data-ttu-id="a6437-201">En [pipeline](data-factory-create-pipelines.md) med Kopieringsaktiviteten som använder [RelationalSource](data-factory-onprem-mysql-connector.md#copy-activity-properties) och [BlobSink](data-factory-azure-blob-connector.md#copy-activity-properties).</span><span class="sxs-lookup"><span data-stu-id="a6437-201">A [pipeline](data-factory-create-pipelines.md) with Copy Activity that uses [RelationalSource](data-factory-onprem-mysql-connector.md#copy-activity-properties) and [BlobSink](data-factory-azure-blob-connector.md#copy-activity-properties).</span></span>

<span data-ttu-id="a6437-202">Exemplet kopierar data från ett frågeresultat i MySQL-databas till en blobb per timme.</span><span class="sxs-lookup"><span data-stu-id="a6437-202">The sample copies data from a query result in MySQL database to a blob hourly.</span></span> <span data-ttu-id="a6437-203">JSON-egenskaper som används i exemplen beskrivs i exemplen i följande avsnitt.</span><span class="sxs-lookup"><span data-stu-id="a6437-203">The JSON properties used in these samples are described in sections following the samples.</span></span>

<span data-ttu-id="a6437-204">Som ett första steg bör du konfigurera data management gateway.</span><span class="sxs-lookup"><span data-stu-id="a6437-204">As a first step, setup the data management gateway.</span></span> <span data-ttu-id="a6437-205">Anvisningarna är i den [flytta data mellan lokala platser och moln](data-factory-move-data-between-onprem-and-cloud.md) artikel.</span><span class="sxs-lookup"><span data-stu-id="a6437-205">The instructions are in the [moving data between on-premises locations and cloud](data-factory-move-data-between-onprem-and-cloud.md) article.</span></span>

<span data-ttu-id="a6437-206">**MySQL länkade tjänsten:**</span><span class="sxs-lookup"><span data-stu-id="a6437-206">**MySQL linked service:**</span></span>

```JSON
    {
      "name": "OnPremMySqlLinkedService",
      "properties": {
        "type": "OnPremisesMySql",
        "typeProperties": {
          "server": "<server name>",
          "database": "<database name>",
          "schema": "<schema name>",
          "authenticationType": "<authentication type>",
          "userName": "<user name>",
          "password": "<password>",
          "gatewayName": "<gateway>"
        }
      }
    }
```

<span data-ttu-id="a6437-207">**Azure Storage länkade tjänsten:**</span><span class="sxs-lookup"><span data-stu-id="a6437-207">**Azure Storage linked service:**</span></span>

```JSON
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

<span data-ttu-id="a6437-208">**MySQL inkommande datauppsättningen:**</span><span class="sxs-lookup"><span data-stu-id="a6437-208">**MySQL input dataset:**</span></span>

<span data-ttu-id="a6437-209">Exemplet förutsätter att du har skapat en tabell ”mytable” som prefix i MySQL och innehåller en kolumn med namnet ”timestampcolumn” för tid series-data.</span><span class="sxs-lookup"><span data-stu-id="a6437-209">The sample assumes you have created a table “MyTable” in MySQL and it contains a column called “timestampcolumn” for time series data.</span></span>

<span data-ttu-id="a6437-210">Inställningen ”externa”: ”true” informerar Data Factory-tjänsten att tabellen är extern till data factory och inte tillverkas av en aktivitet i datafabriken.</span><span class="sxs-lookup"><span data-stu-id="a6437-210">Setting “external”: ”true” informs the Data Factory service that the table is external to the data factory and is not produced by an activity in the data factory.</span></span>

```JSON
    {
        "name": "MySqlDataSet",
        "properties": {
            "published": false,
            "type": "RelationalTable",
            "linkedServiceName": "OnPremMySqlLinkedService",
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

<span data-ttu-id="a6437-211">**Azure Blob utdatauppsättningen:**</span><span class="sxs-lookup"><span data-stu-id="a6437-211">**Azure Blob output dataset:**</span></span>

<span data-ttu-id="a6437-212">Data skrivs till en ny blob varje timme (frekvens: timme, intervall: 1).</span><span class="sxs-lookup"><span data-stu-id="a6437-212">Data is written to a new blob every hour (frequency: hour, interval: 1).</span></span> <span data-ttu-id="a6437-213">Sökvägen till mappen för blobben utvärderas dynamiskt baserat på starttiden för den sektor som bearbetas.</span><span class="sxs-lookup"><span data-stu-id="a6437-213">The folder path for the blob is dynamically evaluated based on the start time of the slice that is being processed.</span></span> <span data-ttu-id="a6437-214">Mappsökvägen använder år, månad, dag och timmar delar av starttiden.</span><span class="sxs-lookup"><span data-stu-id="a6437-214">The folder path uses year, month, day, and hours parts of the start time.</span></span>

```JSON
    {
        "name": "AzureBlobMySqlDataSet",
        "properties": {
            "type": "AzureBlob",
            "linkedServiceName": "AzureStorageLinkedService",
            "typeProperties": {
                "folderPath": "mycontainer/mysql/yearno={Year}/monthno={Month}/dayno={Day}/hourno={Hour}",
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

<span data-ttu-id="a6437-215">**Pipeline med kopieringsaktiviteten:**</span><span class="sxs-lookup"><span data-stu-id="a6437-215">**Pipeline with Copy activity:**</span></span>

<span data-ttu-id="a6437-216">Pipelinen innehåller en kopia-aktivitet som är konfigurerad för att använda indata och utdata-datauppsättningar och är schemalagd att köras varje timme.</span><span class="sxs-lookup"><span data-stu-id="a6437-216">The pipeline contains a Copy Activity that is configured to use the input and output datasets and is scheduled to run every hour.</span></span> <span data-ttu-id="a6437-217">I pipeline-JSON-definitionen av **källa** är inställd på **RelationalSource** och **sink** är inställd på **BlobSink**.</span><span class="sxs-lookup"><span data-stu-id="a6437-217">In the pipeline JSON definition, the **source** type is set to **RelationalSource** and **sink** type is set to **BlobSink**.</span></span> <span data-ttu-id="a6437-218">SQL-frågan som angetts för den **frågan** egenskapen väljer vilka data under den senaste timmen att kopiera.</span><span class="sxs-lookup"><span data-stu-id="a6437-218">The SQL query specified for the **query** property selects the data in the past hour to copy.</span></span>

```JSON
    {
        "name": "CopyMySqlToBlob",
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
                            "name": "MySqlDataSet"
                        }
                    ],
                    "outputs": [
                        {
                            "name": "AzureBlobMySqlDataSet"
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
                    "name": "MySqlToBlob"
                }
            ],
            "start": "2014-06-01T18:00:00Z",
            "end": "2014-06-01T19:00:00Z"
        }
    }
```


### <a name="type-mapping-for-mysql"></a><span data-ttu-id="a6437-219">Mappning för MySQL</span><span class="sxs-lookup"><span data-stu-id="a6437-219">Type mapping for MySQL</span></span>
<span data-ttu-id="a6437-220">Som anges i den [data movement aktiviteter](data-factory-data-movement-activities.md) artikeln kopieringsaktiviteten utför automatisk konverteringar från källtyper att registrera typer med följande metod i två steg:</span><span class="sxs-lookup"><span data-stu-id="a6437-220">As mentioned in the [data movement activities](data-factory-data-movement-activities.md) article, Copy activity performs automatic type conversions from source types to sink types with the following two-step approach:</span></span>

1. <span data-ttu-id="a6437-221">Konvertera från interna källtyper till .NET-typ</span><span class="sxs-lookup"><span data-stu-id="a6437-221">Convert from native source types to .NET type</span></span>
2. <span data-ttu-id="a6437-222">Konvertera från .NET-typ till interna mottagare typ.</span><span class="sxs-lookup"><span data-stu-id="a6437-222">Convert from .NET type to native sink type</span></span>

<span data-ttu-id="a6437-223">När du flyttar data att MySQL används följande mappningar från MySQL-typer till .NET-typer.</span><span class="sxs-lookup"><span data-stu-id="a6437-223">When moving data to MySQL, the following mappings are used from MySQL types to .NET types.</span></span>

| <span data-ttu-id="a6437-224">Typ av MySQL-databas</span><span class="sxs-lookup"><span data-stu-id="a6437-224">MySQL Database type</span></span> | <span data-ttu-id="a6437-225">.NET framework-typ</span><span class="sxs-lookup"><span data-stu-id="a6437-225">.NET Framework type</span></span> |
| --- | --- |
| <span data-ttu-id="a6437-226">bigint osignerade</span><span class="sxs-lookup"><span data-stu-id="a6437-226">bigint unsigned</span></span> |<span data-ttu-id="a6437-227">Decimal</span><span class="sxs-lookup"><span data-stu-id="a6437-227">Decimal</span></span> |
| <span data-ttu-id="a6437-228">bigint</span><span class="sxs-lookup"><span data-stu-id="a6437-228">bigint</span></span> |<span data-ttu-id="a6437-229">Int64</span><span class="sxs-lookup"><span data-stu-id="a6437-229">Int64</span></span> |
| <span data-ttu-id="a6437-230">bitar</span><span class="sxs-lookup"><span data-stu-id="a6437-230">bit</span></span> |<span data-ttu-id="a6437-231">Decimal</span><span class="sxs-lookup"><span data-stu-id="a6437-231">Decimal</span></span> |
| <span data-ttu-id="a6437-232">BLOB</span><span class="sxs-lookup"><span data-stu-id="a6437-232">blob</span></span> |<span data-ttu-id="a6437-233">byte]</span><span class="sxs-lookup"><span data-stu-id="a6437-233">Byte[]</span></span> |
| <span data-ttu-id="a6437-234">bool</span><span class="sxs-lookup"><span data-stu-id="a6437-234">bool</span></span> |<span data-ttu-id="a6437-235">Booleskt värde</span><span class="sxs-lookup"><span data-stu-id="a6437-235">Boolean</span></span> |
| <span data-ttu-id="a6437-236">Char</span><span class="sxs-lookup"><span data-stu-id="a6437-236">char</span></span> |<span data-ttu-id="a6437-237">Sträng</span><span class="sxs-lookup"><span data-stu-id="a6437-237">String</span></span> |
| <span data-ttu-id="a6437-238">Datum</span><span class="sxs-lookup"><span data-stu-id="a6437-238">date</span></span> |<span data-ttu-id="a6437-239">Datum och tid</span><span class="sxs-lookup"><span data-stu-id="a6437-239">Datetime</span></span> |
| <span data-ttu-id="a6437-240">Datum och tid</span><span class="sxs-lookup"><span data-stu-id="a6437-240">datetime</span></span> |<span data-ttu-id="a6437-241">Datum och tid</span><span class="sxs-lookup"><span data-stu-id="a6437-241">Datetime</span></span> |
| <span data-ttu-id="a6437-242">Decimal</span><span class="sxs-lookup"><span data-stu-id="a6437-242">decimal</span></span> |<span data-ttu-id="a6437-243">Decimal</span><span class="sxs-lookup"><span data-stu-id="a6437-243">Decimal</span></span> |
| <span data-ttu-id="a6437-244">dubbel precision</span><span class="sxs-lookup"><span data-stu-id="a6437-244">double precision</span></span> |<span data-ttu-id="a6437-245">dubbla</span><span class="sxs-lookup"><span data-stu-id="a6437-245">Double</span></span> |
| <span data-ttu-id="a6437-246">dubbla</span><span class="sxs-lookup"><span data-stu-id="a6437-246">double</span></span> |<span data-ttu-id="a6437-247">dubbla</span><span class="sxs-lookup"><span data-stu-id="a6437-247">Double</span></span> |
| <span data-ttu-id="a6437-248">Enum</span><span class="sxs-lookup"><span data-stu-id="a6437-248">enum</span></span> |<span data-ttu-id="a6437-249">Sträng</span><span class="sxs-lookup"><span data-stu-id="a6437-249">String</span></span> |
| <span data-ttu-id="a6437-250">flyttal</span><span class="sxs-lookup"><span data-stu-id="a6437-250">float</span></span> |<span data-ttu-id="a6437-251">Enskild</span><span class="sxs-lookup"><span data-stu-id="a6437-251">Single</span></span> |
| <span data-ttu-id="a6437-252">int osignerade</span><span class="sxs-lookup"><span data-stu-id="a6437-252">int unsigned</span></span> |<span data-ttu-id="a6437-253">Int64</span><span class="sxs-lookup"><span data-stu-id="a6437-253">Int64</span></span> |
| <span data-ttu-id="a6437-254">int</span><span class="sxs-lookup"><span data-stu-id="a6437-254">int</span></span> |<span data-ttu-id="a6437-255">Int32</span><span class="sxs-lookup"><span data-stu-id="a6437-255">Int32</span></span> |
| <span data-ttu-id="a6437-256">heltal osignerade</span><span class="sxs-lookup"><span data-stu-id="a6437-256">integer unsigned</span></span> |<span data-ttu-id="a6437-257">Int64</span><span class="sxs-lookup"><span data-stu-id="a6437-257">Int64</span></span> |
| <span data-ttu-id="a6437-258">heltal</span><span class="sxs-lookup"><span data-stu-id="a6437-258">integer</span></span> |<span data-ttu-id="a6437-259">Int32</span><span class="sxs-lookup"><span data-stu-id="a6437-259">Int32</span></span> |
| <span data-ttu-id="a6437-260">lång varbinary</span><span class="sxs-lookup"><span data-stu-id="a6437-260">long varbinary</span></span> |<span data-ttu-id="a6437-261">byte]</span><span class="sxs-lookup"><span data-stu-id="a6437-261">Byte[]</span></span> |
| <span data-ttu-id="a6437-262">lång varchar</span><span class="sxs-lookup"><span data-stu-id="a6437-262">long varchar</span></span> |<span data-ttu-id="a6437-263">Sträng</span><span class="sxs-lookup"><span data-stu-id="a6437-263">String</span></span> |
| <span data-ttu-id="a6437-264">longblob</span><span class="sxs-lookup"><span data-stu-id="a6437-264">longblob</span></span> |<span data-ttu-id="a6437-265">byte]</span><span class="sxs-lookup"><span data-stu-id="a6437-265">Byte[]</span></span> |
| <span data-ttu-id="a6437-266">LONGTEXT</span><span class="sxs-lookup"><span data-stu-id="a6437-266">longtext</span></span> |<span data-ttu-id="a6437-267">Sträng</span><span class="sxs-lookup"><span data-stu-id="a6437-267">String</span></span> |
| <span data-ttu-id="a6437-268">mediumblob</span><span class="sxs-lookup"><span data-stu-id="a6437-268">mediumblob</span></span> |<span data-ttu-id="a6437-269">byte]</span><span class="sxs-lookup"><span data-stu-id="a6437-269">Byte[]</span></span> |
| <span data-ttu-id="a6437-270">mediumint osignerade</span><span class="sxs-lookup"><span data-stu-id="a6437-270">mediumint unsigned</span></span> |<span data-ttu-id="a6437-271">Int64</span><span class="sxs-lookup"><span data-stu-id="a6437-271">Int64</span></span> |
| <span data-ttu-id="a6437-272">mediumint</span><span class="sxs-lookup"><span data-stu-id="a6437-272">mediumint</span></span> |<span data-ttu-id="a6437-273">Int32</span><span class="sxs-lookup"><span data-stu-id="a6437-273">Int32</span></span> |
| <span data-ttu-id="a6437-274">mediumtext</span><span class="sxs-lookup"><span data-stu-id="a6437-274">mediumtext</span></span> |<span data-ttu-id="a6437-275">Sträng</span><span class="sxs-lookup"><span data-stu-id="a6437-275">String</span></span> |
| <span data-ttu-id="a6437-276">numeriskt</span><span class="sxs-lookup"><span data-stu-id="a6437-276">numeric</span></span> |<span data-ttu-id="a6437-277">Decimal</span><span class="sxs-lookup"><span data-stu-id="a6437-277">Decimal</span></span> |
| <span data-ttu-id="a6437-278">Verklig</span><span class="sxs-lookup"><span data-stu-id="a6437-278">real</span></span> |<span data-ttu-id="a6437-279">dubbla</span><span class="sxs-lookup"><span data-stu-id="a6437-279">Double</span></span> |
| <span data-ttu-id="a6437-280">Ange</span><span class="sxs-lookup"><span data-stu-id="a6437-280">set</span></span> |<span data-ttu-id="a6437-281">Sträng</span><span class="sxs-lookup"><span data-stu-id="a6437-281">String</span></span> |
| <span data-ttu-id="a6437-282">smallint osignerade</span><span class="sxs-lookup"><span data-stu-id="a6437-282">smallint unsigned</span></span> |<span data-ttu-id="a6437-283">Int32</span><span class="sxs-lookup"><span data-stu-id="a6437-283">Int32</span></span> |
| <span data-ttu-id="a6437-284">smallint</span><span class="sxs-lookup"><span data-stu-id="a6437-284">smallint</span></span> |<span data-ttu-id="a6437-285">Int16</span><span class="sxs-lookup"><span data-stu-id="a6437-285">Int16</span></span> |
| <span data-ttu-id="a6437-286">Text</span><span class="sxs-lookup"><span data-stu-id="a6437-286">text</span></span> |<span data-ttu-id="a6437-287">Sträng</span><span class="sxs-lookup"><span data-stu-id="a6437-287">String</span></span> |
| <span data-ttu-id="a6437-288">time</span><span class="sxs-lookup"><span data-stu-id="a6437-288">time</span></span> |<span data-ttu-id="a6437-289">TimeSpan</span><span class="sxs-lookup"><span data-stu-id="a6437-289">TimeSpan</span></span> |
| <span data-ttu-id="a6437-290">tidsstämpel</span><span class="sxs-lookup"><span data-stu-id="a6437-290">timestamp</span></span> |<span data-ttu-id="a6437-291">Datum och tid</span><span class="sxs-lookup"><span data-stu-id="a6437-291">Datetime</span></span> |
| <span data-ttu-id="a6437-292">tinyblob</span><span class="sxs-lookup"><span data-stu-id="a6437-292">tinyblob</span></span> |<span data-ttu-id="a6437-293">byte]</span><span class="sxs-lookup"><span data-stu-id="a6437-293">Byte[]</span></span> |
| <span data-ttu-id="a6437-294">tinyint osignerade</span><span class="sxs-lookup"><span data-stu-id="a6437-294">tinyint unsigned</span></span> |<span data-ttu-id="a6437-295">Int16</span><span class="sxs-lookup"><span data-stu-id="a6437-295">Int16</span></span> |
| <span data-ttu-id="a6437-296">tinyint</span><span class="sxs-lookup"><span data-stu-id="a6437-296">tinyint</span></span> |<span data-ttu-id="a6437-297">Int16</span><span class="sxs-lookup"><span data-stu-id="a6437-297">Int16</span></span> |
| <span data-ttu-id="a6437-298">tinytext</span><span class="sxs-lookup"><span data-stu-id="a6437-298">tinytext</span></span> |<span data-ttu-id="a6437-299">Sträng</span><span class="sxs-lookup"><span data-stu-id="a6437-299">String</span></span> |
| <span data-ttu-id="a6437-300">varchar</span><span class="sxs-lookup"><span data-stu-id="a6437-300">varchar</span></span> |<span data-ttu-id="a6437-301">Sträng</span><span class="sxs-lookup"><span data-stu-id="a6437-301">String</span></span> |
| <span data-ttu-id="a6437-302">År</span><span class="sxs-lookup"><span data-stu-id="a6437-302">year</span></span> |<span data-ttu-id="a6437-303">int</span><span class="sxs-lookup"><span data-stu-id="a6437-303">Int</span></span> |

## <a name="map-source-to-sink-columns"></a><span data-ttu-id="a6437-304">Karta källan till mottagare för kolumner</span><span class="sxs-lookup"><span data-stu-id="a6437-304">Map source to sink columns</span></span>
<span data-ttu-id="a6437-305">Mer information om mappning kolumner i datauppsättningen källan till kolumner i datauppsättning mottagare, se [mappa dataset kolumner i Azure Data Factory](data-factory-map-columns.md).</span><span class="sxs-lookup"><span data-stu-id="a6437-305">To learn about mapping columns in source dataset to columns in sink dataset, see [Mapping dataset columns in Azure Data Factory](data-factory-map-columns.md).</span></span>

## <a name="repeatable-read-from-relational-sources"></a><span data-ttu-id="a6437-306">Upprepbar läsning från relationella källor</span><span class="sxs-lookup"><span data-stu-id="a6437-306">Repeatable read from relational sources</span></span>
<span data-ttu-id="a6437-307">Tänk på att undvika oväntade resultat repeterbarhet när kopiering av data från relationella data lagras.</span><span class="sxs-lookup"><span data-stu-id="a6437-307">When copying data from relational data stores, keep repeatability in mind to avoid unintended outcomes.</span></span> <span data-ttu-id="a6437-308">I Azure Data Factory, kan du köra en sektor manuellt.</span><span class="sxs-lookup"><span data-stu-id="a6437-308">In Azure Data Factory, you can rerun a slice manually.</span></span> <span data-ttu-id="a6437-309">Du kan också konfigurera i principen för en dataset så att ett segment som körs när ett fel uppstår.</span><span class="sxs-lookup"><span data-stu-id="a6437-309">You can also configure retry policy for a dataset so that a slice is rerun when a failure occurs.</span></span> <span data-ttu-id="a6437-310">När ett segment körs på något sätt, måste du kontrollera att samma data läses oavsett hur många gånger ett segment körs.</span><span class="sxs-lookup"><span data-stu-id="a6437-310">When a slice is rerun in either way, you need to make sure that the same data is read no matter how many times a slice is run.</span></span> <span data-ttu-id="a6437-311">Se [Repeatable läsa från relationella källor](data-factory-repeatable-copy.md#repeatable-read-from-relational-sources).</span><span class="sxs-lookup"><span data-stu-id="a6437-311">See [Repeatable read from relational sources](data-factory-repeatable-copy.md#repeatable-read-from-relational-sources).</span></span>

## <a name="performance-and-tuning"></a><span data-ttu-id="a6437-312">Prestanda och finjustering</span><span class="sxs-lookup"><span data-stu-id="a6437-312">Performance and Tuning</span></span>
<span data-ttu-id="a6437-313">Se [kopiera aktivitet prestanda och justera guiden](data-factory-copy-activity-performance.md) vill veta mer om viktiga faktorer som påverkan prestanda för flytt av data (Kopieringsaktiviteten) i Azure Data Factory och olika sätt att optimera den.</span><span class="sxs-lookup"><span data-stu-id="a6437-313">See [Copy Activity Performance & Tuning Guide](data-factory-copy-activity-performance.md) to learn about key factors that impact performance of data movement (Copy Activity) in Azure Data Factory and various ways to optimize it.</span></span>
