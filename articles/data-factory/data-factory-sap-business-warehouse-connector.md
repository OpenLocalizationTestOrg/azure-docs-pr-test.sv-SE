---
title: "Flytta data från SAP Business Warehouse med hjälp av Azure Data Factory | Microsoft Docs"
description: "Läs mer om hur du flyttar data från SAP Business Warehouse med hjälp av Azure Data Factory."
services: data-factory
documentationcenter: 
author: linda33wj
manager: jhubbard
editor: 
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/16/2017
ms.author: jingwang
ms.openlocfilehash: 220ccc8b94797880d335385046001c5f3b17c862
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="move-data-from-sap-business-warehouse-using-azure-data-factory"></a><span data-ttu-id="7a4e4-103">Flytta data från SAP Business Warehouse med hjälp av Azure Data Factory</span><span class="sxs-lookup"><span data-stu-id="7a4e4-103">Move data From SAP Business Warehouse using Azure Data Factory</span></span>
<span data-ttu-id="7a4e4-104">Den här artikeln förklarar hur du använder aktiviteten kopiera i Azure Data Factory för att flytta data från en lokal SAP Business Warehouse (BW).</span><span class="sxs-lookup"><span data-stu-id="7a4e4-104">This article explains how to use the Copy Activity in Azure Data Factory to move data from an on-premises SAP Business Warehouse (BW).</span></span> <span data-ttu-id="7a4e4-105">Den bygger på den [Data Movement aktiviteter](data-factory-data-movement-activities.md) artikel som presenterar en allmän översikt över dataflyttning med copy-aktivitet.</span><span class="sxs-lookup"><span data-stu-id="7a4e4-105">It builds on the [Data Movement Activities](data-factory-data-movement-activities.md) article, which presents a general overview of data movement with the copy activity.</span></span>

<span data-ttu-id="7a4e4-106">Du kan kopiera data från en lokal SAP Business Warehouse dataarkiv till alla stöds sink-datalagret.</span><span class="sxs-lookup"><span data-stu-id="7a4e4-106">You can copy data from an on-premises SAP Business Warehouse data store to any supported sink data store.</span></span> <span data-ttu-id="7a4e4-107">En lista över datakällor som stöds som sänkor av kopieringsaktiviteten, finns det [stöds datalager](data-factory-data-movement-activities.md#supported-data-stores-and-formats) tabell.</span><span class="sxs-lookup"><span data-stu-id="7a4e4-107">For a list of data stores supported as sinks by the copy activity, see the [Supported data stores](data-factory-data-movement-activities.md#supported-data-stores-and-formats) table.</span></span> <span data-ttu-id="7a4e4-108">Data factory stöder för närvarande endast flytta data från en SAP Business Warehouse till andra databaser, men inte för att flytta data från andra datalager till en SAP Business Warehouse.</span><span class="sxs-lookup"><span data-stu-id="7a4e4-108">Data factory currently supports only moving data from an SAP Business Warehouse to other data stores, but not for moving data from other data stores to an SAP Business Warehouse.</span></span> 

## <a name="supported-versions-and-installation"></a><span data-ttu-id="7a4e4-109">Versioner som stöds och installation</span><span class="sxs-lookup"><span data-stu-id="7a4e4-109">Supported versions and installation</span></span>
<span data-ttu-id="7a4e4-110">Den här anslutningen har stöd för SAP Business Warehouse version 7.x.</span><span class="sxs-lookup"><span data-stu-id="7a4e4-110">This connector supports SAP Business Warehouse version 7.x.</span></span> <span data-ttu-id="7a4e4-111">Det stöder kopiering av data från InfoCubes och QueryCubes (inklusive BEx frågor) med hjälp av MDX-frågor.</span><span class="sxs-lookup"><span data-stu-id="7a4e4-111">It supports copying data from InfoCubes and QueryCubes (including BEx queries) using MDX queries.</span></span>

<span data-ttu-id="7a4e4-112">Om du vill aktivera anslutning till SAP BW-instans, installera följande komponenter:</span><span class="sxs-lookup"><span data-stu-id="7a4e4-112">To enable the connectivity to the SAP BW instance, install the following components:</span></span>
- <span data-ttu-id="7a4e4-113">**Data Management Gateway**: Data Factory-tjänsten har stöd för att ansluta till lokala data Arkiv (inklusive SAP Business Warehouse) med hjälp av en komponent som kallas Data Management Gateway.</span><span class="sxs-lookup"><span data-stu-id="7a4e4-113">**Data Management Gateway**: Data Factory service supports connecting to on-premises data stores (including SAP Business Warehouse) using a component called Data Management Gateway.</span></span> <span data-ttu-id="7a4e4-114">Mer information om Data Management Gateway samt stegvisa instruktioner för hur du konfigurerar en gateway, se [flytta mellan lokala data datalager till molnet datalagret](data-factory-move-data-between-onprem-and-cloud.md) artikel.</span><span class="sxs-lookup"><span data-stu-id="7a4e4-114">To learn about Data Management Gateway and step-by-step instructions for setting up the gateway, see [Moving data between on-premises data store to cloud data store](data-factory-move-data-between-onprem-and-cloud.md) article.</span></span> <span data-ttu-id="7a4e4-115">Gateway krävs även om SAP Business Warehouse finns i en Azure IaaS-virtuella (VM).</span><span class="sxs-lookup"><span data-stu-id="7a4e4-115">Gateway is required even if the SAP Business Warehouse is hosted in an Azure IaaS virtual machine (VM).</span></span> <span data-ttu-id="7a4e4-116">Du kan installera gatewayen på samma virtuella dator som dataarkiv eller på en annan virtuell dator, förutsatt att gatewayen kan ansluta till databasen.</span><span class="sxs-lookup"><span data-stu-id="7a4e4-116">You can install the gateway on the same VM as the data store or on a different VM as long as the gateway can connect to the database.</span></span>
- <span data-ttu-id="7a4e4-117">**SAP NetWeaver biblioteket** på gateway-datorn.</span><span class="sxs-lookup"><span data-stu-id="7a4e4-117">**SAP NetWeaver library** on the gateway machine.</span></span> <span data-ttu-id="7a4e4-118">Du kan hämta SAP Netweaver-bibliotek från SAP-administratören eller direkt från den [SAP Software Download Center](https://support.sap.com/swdc).</span><span class="sxs-lookup"><span data-stu-id="7a4e4-118">You can get the SAP Netweaver library from your SAP administrator, or directly from the [SAP Software Download Center](https://support.sap.com/swdc).</span></span> <span data-ttu-id="7a4e4-119">Sök efter den **SAP Obs #1025361** få hämtningsplatsen för den senaste versionen.</span><span class="sxs-lookup"><span data-stu-id="7a4e4-119">Search for the **SAP Note #1025361** to get the download location for the most recent version.</span></span> <span data-ttu-id="7a4e4-120">Kontrollera att arkitekturen för SAP NetWeaver biblioteket (32-bitars eller 64-bitars) matchar din gateway-installation.</span><span class="sxs-lookup"><span data-stu-id="7a4e4-120">Make sure that the architecture for the SAP NetWeaver library (32-bit or 64-bit) matches your gateway installation.</span></span> <span data-ttu-id="7a4e4-121">Installera alla filer som ingår i SAP NetWeaver RFC SDK enligt SAP-kommentar.</span><span class="sxs-lookup"><span data-stu-id="7a4e4-121">Then install all files included in the SAP NetWeaver RFC SDK according to the SAP Note.</span></span> <span data-ttu-id="7a4e4-122">Bibliotek för SAP NetWeaver ingår också i klientverktyg för SAP-installationen.</span><span class="sxs-lookup"><span data-stu-id="7a4e4-122">The SAP NetWeaver library is also included in the SAP Client Tools installation.</span></span>

> [!TIP]
> <span data-ttu-id="7a4e4-123">Placera DLL: er som extraheras från NetWeaver RFC SDK i mappen system32.</span><span class="sxs-lookup"><span data-stu-id="7a4e4-123">Put the dlls extracted from the NetWeaver RFC SDK into system32 folder.</span></span>

## <a name="getting-started"></a><span data-ttu-id="7a4e4-124">Komma igång</span><span class="sxs-lookup"><span data-stu-id="7a4e4-124">Getting started</span></span>
<span data-ttu-id="7a4e4-125">Du kan skapa en pipeline med en kopia-aktivitet som flyttar data från en lokal Cassandra data store med hjälp av olika verktyg/API: er.</span><span class="sxs-lookup"><span data-stu-id="7a4e4-125">You can create a pipeline with a copy activity that moves data from an on-premises Cassandra data store by using different tools/APIs.</span></span> 

- <span data-ttu-id="7a4e4-126">Det enklaste sättet att skapa en pipeline är att använda den **guiden Kopiera**.</span><span class="sxs-lookup"><span data-stu-id="7a4e4-126">The easiest way to create a pipeline is to use the **Copy Wizard**.</span></span> <span data-ttu-id="7a4e4-127">Finns [Självstudier: skapa en pipeline med hjälp av guiden Kopiera](data-factory-copy-data-wizard-tutorial.md) för en snabb genomgång om hur du skapar en pipeline med hjälp av guiden Kopiera data.</span><span class="sxs-lookup"><span data-stu-id="7a4e4-127">See [Tutorial: Create a pipeline using Copy Wizard](data-factory-copy-data-wizard-tutorial.md) for a quick walkthrough on creating a pipeline using the Copy data wizard.</span></span> 
- <span data-ttu-id="7a4e4-128">Du kan också använda följande verktyg för att skapa en pipeline: **Azure-portalen**, **Visual Studio**, **Azure PowerShell**, **Azure Resource Manager-mall**, **.NET API**, och **REST API**.</span><span class="sxs-lookup"><span data-stu-id="7a4e4-128">You can also use the following tools to create a pipeline: **Azure portal**, **Visual Studio**, **Azure PowerShell**, **Azure Resource Manager template**, **.NET API**, and **REST API**.</span></span> <span data-ttu-id="7a4e4-129">Se [kopiera aktivitet kursen](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) för stegvisa instruktioner för att skapa en pipeline med en Kopieringsaktivitet.</span><span class="sxs-lookup"><span data-stu-id="7a4e4-129">See [Copy activity tutorial](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) for step-by-step instructions to create a pipeline with a copy activity.</span></span> 

<span data-ttu-id="7a4e4-130">Om du använder verktyg eller API: er, kan du utföra följande steg för att skapa en pipeline som flyttar data från ett dataarkiv som källa till ett dataarkiv som mottagare:</span><span class="sxs-lookup"><span data-stu-id="7a4e4-130">Whether you use the tools or APIs, you perform the following steps to create a pipeline that moves data from a source data store to a sink data store:</span></span>

1. <span data-ttu-id="7a4e4-131">Skapa **länkade tjänster** att länka inkommande och utgående data lagras till din data factory.</span><span class="sxs-lookup"><span data-stu-id="7a4e4-131">Create **linked services** to link input and output data stores to your data factory.</span></span>
2. <span data-ttu-id="7a4e4-132">Skapa **datauppsättningar** att representera inkommande och utgående data för kopieringen.</span><span class="sxs-lookup"><span data-stu-id="7a4e4-132">Create **datasets** to represent input and output data for the copy operation.</span></span> 
3. <span data-ttu-id="7a4e4-133">Skapa en **pipeline** med en kopia-aktivitet som tar en datamängd som indata och en dataset som utdata.</span><span class="sxs-lookup"><span data-stu-id="7a4e4-133">Create a **pipeline** with a copy activity that takes a dataset as an input and a dataset as an output.</span></span> 

<span data-ttu-id="7a4e4-134">När du använder guiden skapas JSON definitioner för dessa Data Factory-enheter (länkade tjänster, datauppsättningar och pipelinen) automatiskt för dig.</span><span class="sxs-lookup"><span data-stu-id="7a4e4-134">When you use the wizard, JSON definitions for these Data Factory entities (linked services, datasets, and the pipeline) are automatically created for you.</span></span> <span data-ttu-id="7a4e4-135">När du använder Verktyg/API: er (utom .NET API), kan du definiera dessa Data Factory-enheter med hjälp av JSON-format.</span><span class="sxs-lookup"><span data-stu-id="7a4e4-135">When you use tools/APIs (except .NET API), you define these Data Factory entities by using the JSON format.</span></span>  <span data-ttu-id="7a4e4-136">Ett exempel med JSON-definitioner för Data Factory-entiteter som används för att kopiera data från en lokal SAP Business Warehouse finns [JSON-exempel: kopiera data från SAP Business Warehouse till Azure Blob](#json-example-copy-data-from-sap-business-warehouse-to-azure-blob) i den här artikeln.</span><span class="sxs-lookup"><span data-stu-id="7a4e4-136">For a sample with JSON definitions for Data Factory entities that are used to copy data from an on-premises SAP Business Warehouse, see [JSON example: Copy data from SAP Business Warehouse to Azure Blob](#json-example-copy-data-from-sap-business-warehouse-to-azure-blob) section of this article.</span></span> 

<span data-ttu-id="7a4e4-137">Följande avsnitt innehåller information om JSON-egenskaper som används för att definiera Data Factory entiteter till ett SAP BW-datalager:</span><span class="sxs-lookup"><span data-stu-id="7a4e4-137">The following sections provide details about JSON properties that are used to define Data Factory entities specific to an SAP BW data store:</span></span>

## <a name="linked-service-properties"></a><span data-ttu-id="7a4e4-138">Länkad tjänstegenskaper</span><span class="sxs-lookup"><span data-stu-id="7a4e4-138">Linked service properties</span></span>
<span data-ttu-id="7a4e4-139">Följande tabell innehåller en beskrivning för JSON-element som är specifika för SAP Business Warehouse (BW) länkad tjänst.</span><span class="sxs-lookup"><span data-stu-id="7a4e4-139">The following table provides description for JSON elements specific to SAP Business Warehouse (BW) linked service.</span></span>

<span data-ttu-id="7a4e4-140">Egenskap</span><span class="sxs-lookup"><span data-stu-id="7a4e4-140">Property</span></span> | <span data-ttu-id="7a4e4-141">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="7a4e4-141">Description</span></span> | <span data-ttu-id="7a4e4-142">Tillåtna värden</span><span class="sxs-lookup"><span data-stu-id="7a4e4-142">Allowed values</span></span> | <span data-ttu-id="7a4e4-143">Krävs</span><span class="sxs-lookup"><span data-stu-id="7a4e4-143">Required</span></span>
-------- | ----------- | -------------- | --------
<span data-ttu-id="7a4e4-144">server</span><span class="sxs-lookup"><span data-stu-id="7a4e4-144">server</span></span> | <span data-ttu-id="7a4e4-145">Namnet på den server som SAP BW-instansen finns.</span><span class="sxs-lookup"><span data-stu-id="7a4e4-145">Name of the server on which the SAP BW instance resides.</span></span> | <span data-ttu-id="7a4e4-146">Sträng</span><span class="sxs-lookup"><span data-stu-id="7a4e4-146">string</span></span> | <span data-ttu-id="7a4e4-147">Ja</span><span class="sxs-lookup"><span data-stu-id="7a4e4-147">Yes</span></span>
<span data-ttu-id="7a4e4-148">systemNumber</span><span class="sxs-lookup"><span data-stu-id="7a4e4-148">systemNumber</span></span> | <span data-ttu-id="7a4e4-149">Systemnummer för SAP BW-system.</span><span class="sxs-lookup"><span data-stu-id="7a4e4-149">System number of the SAP BW system.</span></span> | <span data-ttu-id="7a4e4-150">Två siffror decimaltal representeras som en sträng.</span><span class="sxs-lookup"><span data-stu-id="7a4e4-150">Two-digit decimal number represented as a string.</span></span> | <span data-ttu-id="7a4e4-151">Ja</span><span class="sxs-lookup"><span data-stu-id="7a4e4-151">Yes</span></span>
<span data-ttu-id="7a4e4-152">clientId</span><span class="sxs-lookup"><span data-stu-id="7a4e4-152">clientId</span></span> | <span data-ttu-id="7a4e4-153">Klient-ID för klienten i systemets SAP-W.</span><span class="sxs-lookup"><span data-stu-id="7a4e4-153">Client ID of the client in the SAP W system.</span></span> | <span data-ttu-id="7a4e4-154">Tre siffror decimaltal representeras som en sträng.</span><span class="sxs-lookup"><span data-stu-id="7a4e4-154">Three-digit decimal number represented as a string.</span></span> | <span data-ttu-id="7a4e4-155">Ja</span><span class="sxs-lookup"><span data-stu-id="7a4e4-155">Yes</span></span>
<span data-ttu-id="7a4e4-156">användarnamn</span><span class="sxs-lookup"><span data-stu-id="7a4e4-156">username</span></span> | <span data-ttu-id="7a4e4-157">Namnet på den användare som har åtkomst till SAP-server</span><span class="sxs-lookup"><span data-stu-id="7a4e4-157">Name of the user who has access to the SAP server</span></span> | <span data-ttu-id="7a4e4-158">Sträng</span><span class="sxs-lookup"><span data-stu-id="7a4e4-158">string</span></span> | <span data-ttu-id="7a4e4-159">Ja</span><span class="sxs-lookup"><span data-stu-id="7a4e4-159">Yes</span></span>
<span data-ttu-id="7a4e4-160">lösenord</span><span class="sxs-lookup"><span data-stu-id="7a4e4-160">password</span></span> | <span data-ttu-id="7a4e4-161">Lösenord för användaren.</span><span class="sxs-lookup"><span data-stu-id="7a4e4-161">Password for the user.</span></span> | <span data-ttu-id="7a4e4-162">Sträng</span><span class="sxs-lookup"><span data-stu-id="7a4e4-162">string</span></span> | <span data-ttu-id="7a4e4-163">Ja</span><span class="sxs-lookup"><span data-stu-id="7a4e4-163">Yes</span></span>
<span data-ttu-id="7a4e4-164">gatewayName</span><span class="sxs-lookup"><span data-stu-id="7a4e4-164">gatewayName</span></span> | <span data-ttu-id="7a4e4-165">Namnet på den gateway som Data Factory-tjänsten ska använda för att ansluta till lokal SAP BW-instans.</span><span class="sxs-lookup"><span data-stu-id="7a4e4-165">Name of the gateway that the Data Factory service should use to connect to the on-premises SAP BW instance.</span></span> | <span data-ttu-id="7a4e4-166">Sträng</span><span class="sxs-lookup"><span data-stu-id="7a4e4-166">string</span></span> | <span data-ttu-id="7a4e4-167">Ja</span><span class="sxs-lookup"><span data-stu-id="7a4e4-167">Yes</span></span>
<span data-ttu-id="7a4e4-168">encryptedCredential</span><span class="sxs-lookup"><span data-stu-id="7a4e4-168">encryptedCredential</span></span> | <span data-ttu-id="7a4e4-169">Strängen som krypterade autentiseringsuppgifter.</span><span class="sxs-lookup"><span data-stu-id="7a4e4-169">The encrypted credential string.</span></span> | <span data-ttu-id="7a4e4-170">Sträng</span><span class="sxs-lookup"><span data-stu-id="7a4e4-170">string</span></span> | <span data-ttu-id="7a4e4-171">Nej</span><span class="sxs-lookup"><span data-stu-id="7a4e4-171">No</span></span>

## <a name="dataset-properties"></a><span data-ttu-id="7a4e4-172">Egenskaper för datamängd</span><span class="sxs-lookup"><span data-stu-id="7a4e4-172">Dataset properties</span></span>
<span data-ttu-id="7a4e4-173">En fullständig lista över egenskaper som är tillgängliga för att definiera datauppsättningarna & avsnitt finns i [skapa datauppsättningar](data-factory-create-datasets.md) artikel.</span><span class="sxs-lookup"><span data-stu-id="7a4e4-173">For a full list of sections & properties available for defining datasets, see the [Creating datasets](data-factory-create-datasets.md) article.</span></span> <span data-ttu-id="7a4e4-174">Avsnitt som struktur, tillgänglighet och princip på en datamängd JSON är liknande för alla typer av dataset (Azure SQL Azure blob, Azure-tabellen, osv.).</span><span class="sxs-lookup"><span data-stu-id="7a4e4-174">Sections such as structure, availability, and policy of a dataset JSON are similar for all dataset types (Azure SQL, Azure blob, Azure table, etc.).</span></span>

<span data-ttu-id="7a4e4-175">Den **typeProperties** avsnitt är olika för varje typ av dataset och innehåller information om placeringen av data i datalagret.</span><span class="sxs-lookup"><span data-stu-id="7a4e4-175">The **typeProperties** section is different for each type of dataset and provides information about the location of the data in the data store.</span></span> <span data-ttu-id="7a4e4-176">Det finns inga typspecifika egenskaper som stöds för SAP BW datauppsättningen av typen **RelationalTable**.</span><span class="sxs-lookup"><span data-stu-id="7a4e4-176">There are no type-specific properties supported for the SAP BW dataset of type **RelationalTable**.</span></span> 


## <a name="copy-activity-properties"></a><span data-ttu-id="7a4e4-177">Kopiera egenskaper för aktivitet</span><span class="sxs-lookup"><span data-stu-id="7a4e4-177">Copy activity properties</span></span>
<span data-ttu-id="7a4e4-178">En fullständig lista över avsnitt & egenskaper som är tillgängliga för att definiera aktiviteter finns i [skapar Pipelines](data-factory-create-pipelines.md) artikel.</span><span class="sxs-lookup"><span data-stu-id="7a4e4-178">For a full list of sections & properties available for defining activities, see the [Creating Pipelines](data-factory-create-pipelines.md) article.</span></span> <span data-ttu-id="7a4e4-179">Egenskaper, till exempel namn, beskrivning, inkommande och utgående tabeller är principer är tillgängliga för alla typer av aktiviteter.</span><span class="sxs-lookup"><span data-stu-id="7a4e4-179">Properties such as name, description, input and output tables, are policies are available for all types of activities.</span></span>

<span data-ttu-id="7a4e4-180">Medan egenskaper som är tillgängliga i den **typeProperties** avsnitt i aktiviteten varierar med varje aktivitetstyp.</span><span class="sxs-lookup"><span data-stu-id="7a4e4-180">Whereas, properties available in the **typeProperties** section of the activity vary with each activity type.</span></span> <span data-ttu-id="7a4e4-181">För Kopieringsaktivitet kan variera de beroende på vilka typer av datakällor och sänkor.</span><span class="sxs-lookup"><span data-stu-id="7a4e4-181">For Copy activity, they vary depending on the types of sources and sinks.</span></span>

<span data-ttu-id="7a4e4-182">När datakällan i en Kopieringsaktivitet är av typen **RelationalSource** (som innehåller SAP BW), följande egenskaper finns i avsnittet typeProperties:</span><span class="sxs-lookup"><span data-stu-id="7a4e4-182">When source in copy activity is of type **RelationalSource** (which includes SAP BW), the following properties are available in typeProperties section:</span></span>

| <span data-ttu-id="7a4e4-183">Egenskap</span><span class="sxs-lookup"><span data-stu-id="7a4e4-183">Property</span></span> | <span data-ttu-id="7a4e4-184">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="7a4e4-184">Description</span></span> | <span data-ttu-id="7a4e4-185">Tillåtna värden</span><span class="sxs-lookup"><span data-stu-id="7a4e4-185">Allowed values</span></span> | <span data-ttu-id="7a4e4-186">Krävs</span><span class="sxs-lookup"><span data-stu-id="7a4e4-186">Required</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="7a4e4-187">DocumentDB</span><span class="sxs-lookup"><span data-stu-id="7a4e4-187">query</span></span> | <span data-ttu-id="7a4e4-188">Anger MDX-fråga för att läsa data från SAP BW-instans.</span><span class="sxs-lookup"><span data-stu-id="7a4e4-188">Specifies the MDX query to read data from the SAP BW instance.</span></span> | <span data-ttu-id="7a4e4-189">MDX-fråga.</span><span class="sxs-lookup"><span data-stu-id="7a4e4-189">MDX query.</span></span> | <span data-ttu-id="7a4e4-190">Ja</span><span class="sxs-lookup"><span data-stu-id="7a4e4-190">Yes</span></span> |


## <a name="json-example-copy-data-from-sap-business-warehouse-to-azure-blob"></a><span data-ttu-id="7a4e4-191">JSON-exempel: kopiera data från SAP Business Warehouse till Azure-Blob</span><span class="sxs-lookup"><span data-stu-id="7a4e4-191">JSON example: Copy data from SAP Business Warehouse to Azure Blob</span></span>
<span data-ttu-id="7a4e4-192">I följande exempel innehåller exempel JSON definitioner som du kan använda för att skapa en pipeline med [Azure-portalen](data-factory-copy-activity-tutorial-using-azure-portal.md) eller [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md) eller [Azure PowerShell](data-factory-copy-activity-tutorial-using-powershell.md).</span><span class="sxs-lookup"><span data-stu-id="7a4e4-192">The following example provides sample JSON definitions that you can use to create a pipeline by using [Azure portal](data-factory-copy-activity-tutorial-using-azure-portal.md) or [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md) or [Azure PowerShell](data-factory-copy-activity-tutorial-using-powershell.md).</span></span> <span data-ttu-id="7a4e4-193">Det här exemplet visas hur du kopierar data från en lokal SAP Business Warehouse till ett Azure Blob Storage.</span><span class="sxs-lookup"><span data-stu-id="7a4e4-193">This sample shows how to copy data from an on-premises SAP Business Warehouse to an Azure Blob Storage.</span></span> <span data-ttu-id="7a4e4-194">Dock datan kan kopieras **direkt** till någon av sänkor anges [här](data-factory-data-movement-activities.md#supported-data-stores-and-formats) med hjälp av aktiviteten kopiera i Azure Data Factory.</span><span class="sxs-lookup"><span data-stu-id="7a4e4-194">However, data can be copied **directly** to any of the sinks stated [here](data-factory-data-movement-activities.md#supported-data-stores-and-formats) using the Copy Activity in Azure Data Factory.</span></span>  

> [!IMPORTANT]
> <span data-ttu-id="7a4e4-195">Det här exemplet innehåller JSON kodavsnitt.</span><span class="sxs-lookup"><span data-stu-id="7a4e4-195">This sample provides JSON snippets.</span></span> <span data-ttu-id="7a4e4-196">Stegvisa instruktioner för att skapa datafabriken inkluderas inte.</span><span class="sxs-lookup"><span data-stu-id="7a4e4-196">It does not include step-by-step instructions for creating the data factory.</span></span> <span data-ttu-id="7a4e4-197">Se [flytta data mellan lokala platser och moln](data-factory-move-data-between-onprem-and-cloud.md) artikel stegvisa instruktioner.</span><span class="sxs-lookup"><span data-stu-id="7a4e4-197">See [moving data between on-premises locations and cloud](data-factory-move-data-between-onprem-and-cloud.md) article for step-by-step instructions.</span></span>

<span data-ttu-id="7a4e4-198">Exemplet har följande data factory enheter:</span><span class="sxs-lookup"><span data-stu-id="7a4e4-198">The sample has the following data factory entities:</span></span>

1. <span data-ttu-id="7a4e4-199">En länkad tjänst av typen [SapBw](#linked-service-properties).</span><span class="sxs-lookup"><span data-stu-id="7a4e4-199">A linked service of type [SapBw](#linked-service-properties).</span></span>
2. <span data-ttu-id="7a4e4-200">En länkad tjänst av typen [AzureStorage](data-factory-azure-blob-connector.md#linked-service-properties).</span><span class="sxs-lookup"><span data-stu-id="7a4e4-200">A linked service of type [AzureStorage](data-factory-azure-blob-connector.md#linked-service-properties).</span></span>
3. <span data-ttu-id="7a4e4-201">Indata [dataset](data-factory-create-datasets.md) av typen [RelationalTable](#dataset-properties).</span><span class="sxs-lookup"><span data-stu-id="7a4e4-201">An input [dataset](data-factory-create-datasets.md) of type [RelationalTable](#dataset-properties).</span></span>
4. <span data-ttu-id="7a4e4-202">Utdata [dataset](data-factory-create-datasets.md) av typen [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties).</span><span class="sxs-lookup"><span data-stu-id="7a4e4-202">An output [dataset](data-factory-create-datasets.md) of type [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties).</span></span>
5. <span data-ttu-id="7a4e4-203">En [pipeline](data-factory-create-pipelines.md) med Kopieringsaktiviteten som använder [RelationalSource](#copy-activity-properties) och [BlobSink](data-factory-azure-blob-connector.md#copy-activity-properties).</span><span class="sxs-lookup"><span data-stu-id="7a4e4-203">A [pipeline](data-factory-create-pipelines.md) with Copy Activity that uses [RelationalSource](#copy-activity-properties) and [BlobSink](data-factory-azure-blob-connector.md#copy-activity-properties).</span></span>

<span data-ttu-id="7a4e4-204">Exemplet kopierar data från en SAP Business Warehouse-instans till en Azure blob varje timme.</span><span class="sxs-lookup"><span data-stu-id="7a4e4-204">The sample copies data from an SAP Business Warehouse instance to an Azure blob hourly.</span></span> <span data-ttu-id="7a4e4-205">JSON-egenskaper som används i exemplen beskrivs i exemplen i följande avsnitt.</span><span class="sxs-lookup"><span data-stu-id="7a4e4-205">The JSON properties used in these samples are described in sections following the samples.</span></span>

<span data-ttu-id="7a4e4-206">Som ett första steg bör du konfigurera data management gateway.</span><span class="sxs-lookup"><span data-stu-id="7a4e4-206">As a first step, setup the data management gateway.</span></span> <span data-ttu-id="7a4e4-207">Anvisningarna är i den [flytta data mellan lokala platser och moln](data-factory-move-data-between-onprem-and-cloud.md) artikel.</span><span class="sxs-lookup"><span data-stu-id="7a4e4-207">The instructions are in the [moving data between on-premises locations and cloud](data-factory-move-data-between-onprem-and-cloud.md) article.</span></span>

### <a name="sap-business-warehouse-linked-service"></a><span data-ttu-id="7a4e4-208">SAP Business Warehouse länkade tjänsten</span><span class="sxs-lookup"><span data-stu-id="7a4e4-208">SAP Business Warehouse linked service</span></span>
<span data-ttu-id="7a4e4-209">Det här länkade tjänsten länkar SAP BW-instans till datafabriken.</span><span class="sxs-lookup"><span data-stu-id="7a4e4-209">This linked service links your SAP BW instance to the data factory.</span></span> <span data-ttu-id="7a4e4-210">Egenskapen type har angetts **SapBw**.</span><span class="sxs-lookup"><span data-stu-id="7a4e4-210">The type property is set to **SapBw**.</span></span> <span data-ttu-id="7a4e4-211">Avsnittet typeProperties innehåller anslutningsinformation för SAP BW-instans.</span><span class="sxs-lookup"><span data-stu-id="7a4e4-211">The typeProperties section provides connection information for the SAP BW instance.</span></span> 

```json
{
    "name": "SapBwLinkedService",
    "properties":
    {
        "type": "SapBw",
        "typeProperties":
        {
            "server": "<server name>",
            "systemNumber": "<system number>",
            "clientId": "<client id>",
            "username": "<SAP user>",
            "password": "<Password for SAP user>",
            "gatewayName": "<gateway name>"
        }
    }
}
```

### <a name="azure-storage-linked-service"></a><span data-ttu-id="7a4e4-212">Länkad Azure-lagringstjänst</span><span class="sxs-lookup"><span data-stu-id="7a4e4-212">Azure Storage linked service</span></span>
<span data-ttu-id="7a4e4-213">Det här länkade tjänsten länkar Azure Storage-konto till datafabriken.</span><span class="sxs-lookup"><span data-stu-id="7a4e4-213">This linked service links your Azure Storage account to the data factory.</span></span> <span data-ttu-id="7a4e4-214">Egenskapen type har angetts **AzureStorage**.</span><span class="sxs-lookup"><span data-stu-id="7a4e4-214">The type property is set to **AzureStorage**.</span></span> <span data-ttu-id="7a4e4-215">Avsnittet typeProperties innehåller anslutningsinformation för Azure Storage-konto.</span><span class="sxs-lookup"><span data-stu-id="7a4e4-215">The typeProperties section provides connection information for the Azure Storage account.</span></span>

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

### <a name="sap-bw-input-dataset"></a><span data-ttu-id="7a4e4-216">SAP BW inkommande dataset</span><span class="sxs-lookup"><span data-stu-id="7a4e4-216">SAP BW input dataset</span></span>
<span data-ttu-id="7a4e4-217">Den här datauppsättningen definierar SAP Business Warehouse-datauppsättningen.</span><span class="sxs-lookup"><span data-stu-id="7a4e4-217">This dataset defines the SAP Business Warehouse dataset.</span></span> <span data-ttu-id="7a4e4-218">Du kan ange vilken typ av Data Factory-dataset för att **RelationalTable**.</span><span class="sxs-lookup"><span data-stu-id="7a4e4-218">You set the type of the Data Factory dataset to **RelationalTable**.</span></span> <span data-ttu-id="7a4e4-219">För närvarande kan anger du inte några typspecifika egenskaper för en SAP BW datauppsättning.</span><span class="sxs-lookup"><span data-stu-id="7a4e4-219">Currently, you do not specify any type-specific properties for an SAP BW dataset.</span></span> <span data-ttu-id="7a4e4-220">Frågan i aktivitetsdefinitionen kopiera anger vilka data som ska läsas från SAP BW-instans.</span><span class="sxs-lookup"><span data-stu-id="7a4e4-220">The query in the Copy Activity definition specifies what data to read from the SAP BW instance.</span></span> 

<span data-ttu-id="7a4e4-221">Egenskapen true externa informerar Data Factory-tjänsten att tabellen är extern till data factory och inte tillverkas av en aktivitet i datafabriken.</span><span class="sxs-lookup"><span data-stu-id="7a4e4-221">Setting external property to true informs the Data Factory service that the table is external to the data factory and is not produced by an activity in the data factory.</span></span>

<span data-ttu-id="7a4e4-222">Frekvensen och intervall egenskaper definierar schemat.</span><span class="sxs-lookup"><span data-stu-id="7a4e4-222">Frequency and interval properties defines the schedule.</span></span> <span data-ttu-id="7a4e4-223">I det här fallet läses data från SAP BW-instans varje timme.</span><span class="sxs-lookup"><span data-stu-id="7a4e4-223">In this case, the data is read from the SAP BW instance hourly.</span></span> 

```json
{
    "name": "SapBwDataset",
    "properties": {
        "type": "RelationalTable",
        "linkedServiceName": "SapBwLinkedService",
        "typeProperties": {},
        "availability": {
            "frequency": "Hour",
            "interval": 1
        },
        "external": true
    }
}
```



### <a name="azure-blob-output-dataset"></a><span data-ttu-id="7a4e4-224">Utdatauppsättning för Azure-blobb</span><span class="sxs-lookup"><span data-stu-id="7a4e4-224">Azure Blob output dataset</span></span>
<span data-ttu-id="7a4e4-225">Den här datauppsättningen definierar Azure Blob-datamängd för utdata.</span><span class="sxs-lookup"><span data-stu-id="7a4e4-225">This dataset defines the output Azure Blob dataset.</span></span> <span data-ttu-id="7a4e4-226">Egenskapen type har angetts till AzureBlob.</span><span class="sxs-lookup"><span data-stu-id="7a4e4-226">The type property is set to AzureBlob.</span></span> <span data-ttu-id="7a4e4-227">Avsnittet typeProperties innehåller data som kopieras från SAP BW-instans ska lagras.</span><span class="sxs-lookup"><span data-stu-id="7a4e4-227">The typeProperties section provides where the data copied from the SAP BW instance is stored.</span></span> <span data-ttu-id="7a4e4-228">Data skrivs till en ny blob varje timme (frekvens: timme, intervall: 1).</span><span class="sxs-lookup"><span data-stu-id="7a4e4-228">The data is written to a new blob every hour (frequency: hour, interval: 1).</span></span> <span data-ttu-id="7a4e4-229">Sökvägen till mappen för blobben utvärderas dynamiskt baserat på starttiden för den sektor som bearbetas.</span><span class="sxs-lookup"><span data-stu-id="7a4e4-229">The folder path for the blob is dynamically evaluated based on the start time of the slice that is being processed.</span></span> <span data-ttu-id="7a4e4-230">Mappsökvägen använder år, månad, dag och timmar delar av starttiden.</span><span class="sxs-lookup"><span data-stu-id="7a4e4-230">The folder path uses year, month, day, and hours parts of the start time.</span></span>

```json
{
    "name": "AzureBlobDataSet",
    "properties": {
        "type": "AzureBlob",
        "linkedServiceName": "AzureStorageLinkedService",
        "typeProperties": {
            "folderPath": "mycontainer/sapbw/yearno={Year}/monthno={Month}/dayno={Day}/hourno={Hour}",
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


### <a name="pipeline-with-copy-activity"></a><span data-ttu-id="7a4e4-231">Pipeline med kopieringsaktiviteten</span><span class="sxs-lookup"><span data-stu-id="7a4e4-231">Pipeline with Copy activity</span></span>
<span data-ttu-id="7a4e4-232">Pipelinen innehåller en kopia-aktivitet som är konfigurerad för att använda indata och utdata-datauppsättningar och är schemalagd att köras varje timme.</span><span class="sxs-lookup"><span data-stu-id="7a4e4-232">The pipeline contains a Copy Activity that is configured to use the input and output datasets and is scheduled to run every hour.</span></span> <span data-ttu-id="7a4e4-233">I pipeline-JSON-definitionen av **källa** är inställd på **RelationalSource** (för SAP BW källa) och **sink** är inställd på **BlobSink**.</span><span class="sxs-lookup"><span data-stu-id="7a4e4-233">In the pipeline JSON definition, the **source** type is set to **RelationalSource** (for SAP BW source) and **sink** type is set to **BlobSink**.</span></span> <span data-ttu-id="7a4e4-234">Frågan som angetts för den **frågan** egenskapen väljer vilka data under den senaste timmen att kopiera.</span><span class="sxs-lookup"><span data-stu-id="7a4e4-234">The query specified for the **query** property selects the data in the past hour to copy.</span></span>

```json
{
    "name": "CopySapBwToBlob",
    "properties": {
        "description": "pipeline for copy activity",
        "activities": [
            {
                "type": "Copy",
                "typeProperties": {
                    "source": {
                        "type": "RelationalSource",
                        "query": "<MDX query for SAP BW>"
                    },
                    "sink": {
                        "type": "BlobSink",
                        "writeBatchSize": 0,
                        "writeBatchTimeout": "00:00:00"
                    }
                },
                "inputs": [
                    {
                        "name": "SapBwDataset"
                    }
                ],
                "outputs": [
                    {
                        "name": "AzureBlobDataSet"
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
                "name": "SapBwToBlob"
            }
        ],
        "start": "2017-03-01T18:00:00Z",
        "end": "2017-03-01T19:00:00Z"
    }
}
```



### <a name="type-mapping-for-sap-bw"></a><span data-ttu-id="7a4e4-235">Mappning för SAP BW</span><span class="sxs-lookup"><span data-stu-id="7a4e4-235">Type mapping for SAP BW</span></span>
<span data-ttu-id="7a4e4-236">Som anges i den [data movement aktiviteter](data-factory-data-movement-activities.md) artikeln kopieringsaktiviteten utför automatisk konverteringar från källtyper att registrera typer med följande metod i två steg:</span><span class="sxs-lookup"><span data-stu-id="7a4e4-236">As mentioned in the [data movement activities](data-factory-data-movement-activities.md) article, Copy activity performs automatic type conversions from source types to sink types with the following two-step approach:</span></span>

1. <span data-ttu-id="7a4e4-237">Konvertera från interna källtyper till .NET-typ</span><span class="sxs-lookup"><span data-stu-id="7a4e4-237">Convert from native source types to .NET type</span></span>
2. <span data-ttu-id="7a4e4-238">Konvertera från .NET-typ till interna mottagare typ.</span><span class="sxs-lookup"><span data-stu-id="7a4e4-238">Convert from .NET type to native sink type</span></span>

<span data-ttu-id="7a4e4-239">När du flyttar data från SAP BW, används följande mappningar från SAP BW typer för .NET-typer.</span><span class="sxs-lookup"><span data-stu-id="7a4e4-239">When moving data from SAP BW, the following mappings are used from SAP BW types to .NET types.</span></span>

<span data-ttu-id="7a4e4-240">Datatypen i ABAP ordlista</span><span class="sxs-lookup"><span data-stu-id="7a4e4-240">Data type in the ABAP Dictionary</span></span> | <span data-ttu-id="7a4e4-241">.NET-datatyp</span><span class="sxs-lookup"><span data-stu-id="7a4e4-241">.Net Data Type</span></span>
-------------------------------- | --------------
<span data-ttu-id="7a4e4-242">ACCP</span><span class="sxs-lookup"><span data-stu-id="7a4e4-242">ACCP</span></span> |  <span data-ttu-id="7a4e4-243">int</span><span class="sxs-lookup"><span data-stu-id="7a4e4-243">Int</span></span>
<span data-ttu-id="7a4e4-244">CHAR</span><span class="sxs-lookup"><span data-stu-id="7a4e4-244">CHAR</span></span> | <span data-ttu-id="7a4e4-245">Sträng</span><span class="sxs-lookup"><span data-stu-id="7a4e4-245">String</span></span>
<span data-ttu-id="7a4e4-246">CLNT</span><span class="sxs-lookup"><span data-stu-id="7a4e4-246">CLNT</span></span> | <span data-ttu-id="7a4e4-247">Sträng</span><span class="sxs-lookup"><span data-stu-id="7a4e4-247">String</span></span>
<span data-ttu-id="7a4e4-248">AKTUELLT DATUM</span><span class="sxs-lookup"><span data-stu-id="7a4e4-248">CURR</span></span> | <span data-ttu-id="7a4e4-249">Decimal</span><span class="sxs-lookup"><span data-stu-id="7a4e4-249">Decimal</span></span>
<span data-ttu-id="7a4e4-250">CUKY</span><span class="sxs-lookup"><span data-stu-id="7a4e4-250">CUKY</span></span> | <span data-ttu-id="7a4e4-251">Sträng</span><span class="sxs-lookup"><span data-stu-id="7a4e4-251">String</span></span>
<span data-ttu-id="7a4e4-252">DEC</span><span class="sxs-lookup"><span data-stu-id="7a4e4-252">DEC</span></span> | <span data-ttu-id="7a4e4-253">Decimal</span><span class="sxs-lookup"><span data-stu-id="7a4e4-253">Decimal</span></span>
<span data-ttu-id="7a4e4-254">FLTP</span><span class="sxs-lookup"><span data-stu-id="7a4e4-254">FLTP</span></span> | <span data-ttu-id="7a4e4-255">dubbla</span><span class="sxs-lookup"><span data-stu-id="7a4e4-255">Double</span></span>
<span data-ttu-id="7a4e4-256">INT1</span><span class="sxs-lookup"><span data-stu-id="7a4e4-256">INT1</span></span> | <span data-ttu-id="7a4e4-257">Mottagna byte</span><span class="sxs-lookup"><span data-stu-id="7a4e4-257">Byte</span></span>
<span data-ttu-id="7a4e4-258">INT2</span><span class="sxs-lookup"><span data-stu-id="7a4e4-258">INT2</span></span> | <span data-ttu-id="7a4e4-259">Int16</span><span class="sxs-lookup"><span data-stu-id="7a4e4-259">Int16</span></span>
<span data-ttu-id="7a4e4-260">INT4</span><span class="sxs-lookup"><span data-stu-id="7a4e4-260">INT4</span></span> | <span data-ttu-id="7a4e4-261">int</span><span class="sxs-lookup"><span data-stu-id="7a4e4-261">Int</span></span>
<span data-ttu-id="7a4e4-262">LANG</span><span class="sxs-lookup"><span data-stu-id="7a4e4-262">LANG</span></span> | <span data-ttu-id="7a4e4-263">Sträng</span><span class="sxs-lookup"><span data-stu-id="7a4e4-263">String</span></span>
<span data-ttu-id="7a4e4-264">LCHR</span><span class="sxs-lookup"><span data-stu-id="7a4e4-264">LCHR</span></span> | <span data-ttu-id="7a4e4-265">Sträng</span><span class="sxs-lookup"><span data-stu-id="7a4e4-265">String</span></span>
<span data-ttu-id="7a4e4-266">LRAW</span><span class="sxs-lookup"><span data-stu-id="7a4e4-266">LRAW</span></span> | <span data-ttu-id="7a4e4-267">byte]</span><span class="sxs-lookup"><span data-stu-id="7a4e4-267">Byte[]</span></span>
<span data-ttu-id="7a4e4-268">PREC</span><span class="sxs-lookup"><span data-stu-id="7a4e4-268">PREC</span></span> | <span data-ttu-id="7a4e4-269">Int16</span><span class="sxs-lookup"><span data-stu-id="7a4e4-269">Int16</span></span>
<span data-ttu-id="7a4e4-270">QUAN</span><span class="sxs-lookup"><span data-stu-id="7a4e4-270">QUAN</span></span> | <span data-ttu-id="7a4e4-271">Decimal</span><span class="sxs-lookup"><span data-stu-id="7a4e4-271">Decimal</span></span>
<span data-ttu-id="7a4e4-272">RÅDATA</span><span class="sxs-lookup"><span data-stu-id="7a4e4-272">RAW</span></span> | <span data-ttu-id="7a4e4-273">byte]</span><span class="sxs-lookup"><span data-stu-id="7a4e4-273">Byte[]</span></span>
<span data-ttu-id="7a4e4-274">RAWSTRING</span><span class="sxs-lookup"><span data-stu-id="7a4e4-274">RAWSTRING</span></span> | <span data-ttu-id="7a4e4-275">byte]</span><span class="sxs-lookup"><span data-stu-id="7a4e4-275">Byte[]</span></span>
<span data-ttu-id="7a4e4-276">STRÄNG</span><span class="sxs-lookup"><span data-stu-id="7a4e4-276">STRING</span></span> | <span data-ttu-id="7a4e4-277">Sträng</span><span class="sxs-lookup"><span data-stu-id="7a4e4-277">String</span></span>
<span data-ttu-id="7a4e4-278">ENHET</span><span class="sxs-lookup"><span data-stu-id="7a4e4-278">UNIT</span></span> | <span data-ttu-id="7a4e4-279">Sträng</span><span class="sxs-lookup"><span data-stu-id="7a4e4-279">String</span></span>
<span data-ttu-id="7a4e4-280">DATS</span><span class="sxs-lookup"><span data-stu-id="7a4e4-280">DATS</span></span> | <span data-ttu-id="7a4e4-281">Sträng</span><span class="sxs-lookup"><span data-stu-id="7a4e4-281">String</span></span>
<span data-ttu-id="7a4e4-282">NUMC</span><span class="sxs-lookup"><span data-stu-id="7a4e4-282">NUMC</span></span> | <span data-ttu-id="7a4e4-283">Sträng</span><span class="sxs-lookup"><span data-stu-id="7a4e4-283">String</span></span>
<span data-ttu-id="7a4e4-284">TIMS</span><span class="sxs-lookup"><span data-stu-id="7a4e4-284">TIMS</span></span> | <span data-ttu-id="7a4e4-285">Sträng</span><span class="sxs-lookup"><span data-stu-id="7a4e4-285">String</span></span>

> [!NOTE]
> <span data-ttu-id="7a4e4-286">Om du vill mappa kolumner från källan dataset till kolumner från sink dataset finns [mappa dataset kolumner i Azure Data Factory](data-factory-map-columns.md).</span><span class="sxs-lookup"><span data-stu-id="7a4e4-286">To map columns from source dataset to columns from sink dataset, see [Mapping dataset columns in Azure Data Factory](data-factory-map-columns.md).</span></span>


## <a name="map-source-to-sink-columns"></a><span data-ttu-id="7a4e4-287">Karta källan till mottagare för kolumner</span><span class="sxs-lookup"><span data-stu-id="7a4e4-287">Map source to sink columns</span></span>
<span data-ttu-id="7a4e4-288">Mer information om mappning kolumner i datauppsättningen källan till kolumner i datauppsättning mottagare, se [mappa dataset kolumner i Azure Data Factory](data-factory-map-columns.md).</span><span class="sxs-lookup"><span data-stu-id="7a4e4-288">To learn about mapping columns in source dataset to columns in sink dataset, see [Mapping dataset columns in Azure Data Factory](data-factory-map-columns.md).</span></span>

## <a name="repeatable-read-from-relational-sources"></a><span data-ttu-id="7a4e4-289">Upprepbar läsning från relationella källor</span><span class="sxs-lookup"><span data-stu-id="7a4e4-289">Repeatable read from relational sources</span></span>
<span data-ttu-id="7a4e4-290">Tänk på att undvika oväntade resultat repeterbarhet när kopiering av data från relationella data lagras.</span><span class="sxs-lookup"><span data-stu-id="7a4e4-290">When copying data from relational data stores, keep repeatability in mind to avoid unintended outcomes.</span></span> <span data-ttu-id="7a4e4-291">I Azure Data Factory, kan du köra en sektor manuellt.</span><span class="sxs-lookup"><span data-stu-id="7a4e4-291">In Azure Data Factory, you can rerun a slice manually.</span></span> <span data-ttu-id="7a4e4-292">Du kan också konfigurera i principen för en dataset så att ett segment som körs när ett fel uppstår.</span><span class="sxs-lookup"><span data-stu-id="7a4e4-292">You can also configure retry policy for a dataset so that a slice is rerun when a failure occurs.</span></span> <span data-ttu-id="7a4e4-293">När ett segment körs på något sätt, måste du kontrollera att samma data läses oavsett hur många gånger ett segment körs.</span><span class="sxs-lookup"><span data-stu-id="7a4e4-293">When a slice is rerun in either way, you need to make sure that the same data is read no matter how many times a slice is run.</span></span> <span data-ttu-id="7a4e4-294">Se [Repeatable läsa från relationella källor](data-factory-repeatable-copy.md#repeatable-read-from-relational-sources)</span><span class="sxs-lookup"><span data-stu-id="7a4e4-294">See [Repeatable read from relational sources](data-factory-repeatable-copy.md#repeatable-read-from-relational-sources)</span></span>

## <a name="performance-and-tuning"></a><span data-ttu-id="7a4e4-295">Prestanda och finjustering</span><span class="sxs-lookup"><span data-stu-id="7a4e4-295">Performance and Tuning</span></span>
<span data-ttu-id="7a4e4-296">Se [kopiera aktivitet prestanda och justera guiden](data-factory-copy-activity-performance.md) vill veta mer om viktiga faktorer som påverkan prestanda för flytt av data (Kopieringsaktiviteten) i Azure Data Factory och olika sätt att optimera den.</span><span class="sxs-lookup"><span data-stu-id="7a4e4-296">See [Copy Activity Performance & Tuning Guide](data-factory-copy-activity-performance.md) to learn about key factors that impact performance of data movement (Copy Activity) in Azure Data Factory and various ways to optimize it.</span></span>
