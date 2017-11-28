---
title: "aaaMove data från SAP Business Warehouse med hjälp av Azure Data Factory | Microsoft Docs"
description: "Mer information om hur toomove data från SAP Business Warehouse med hjälp av Azure Data Factory."
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
ms.openlocfilehash: 85df16f4759a846f578cad301e3cf918179143d6
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="move-data-from-sap-business-warehouse-using-azure-data-factory"></a><span data-ttu-id="f44c4-103">Flytta data från SAP Business Warehouse med hjälp av Azure Data Factory</span><span class="sxs-lookup"><span data-stu-id="f44c4-103">Move data From SAP Business Warehouse using Azure Data Factory</span></span>
<span data-ttu-id="f44c4-104">Den här artikeln förklarar hur toouse hello Kopieringsaktiviteten i Azure Data Factory toomove data från en lokal SAP Business Warehouse (BW).</span><span class="sxs-lookup"><span data-stu-id="f44c4-104">This article explains how toouse hello Copy Activity in Azure Data Factory toomove data from an on-premises SAP Business Warehouse (BW).</span></span> <span data-ttu-id="f44c4-105">Den bygger på hello [Data Movement aktiviteter](data-factory-data-movement-activities.md) artikel som ger en allmän översikt över dataflyttning hello kopieringsaktiviteten.</span><span class="sxs-lookup"><span data-stu-id="f44c4-105">It builds on hello [Data Movement Activities](data-factory-data-movement-activities.md) article, which presents a general overview of data movement with hello copy activity.</span></span>

<span data-ttu-id="f44c4-106">Du kan kopiera data från en lokal SAP Business Warehouse data store tooany stöds sink dataarkiv.</span><span class="sxs-lookup"><span data-stu-id="f44c4-106">You can copy data from an on-premises SAP Business Warehouse data store tooany supported sink data store.</span></span> <span data-ttu-id="f44c4-107">En lista över data lagras som stöds när egenskaperna av hello kopieringsaktiviteten Se hello [stöds datalager](data-factory-data-movement-activities.md#supported-data-stores-and-formats) tabell.</span><span class="sxs-lookup"><span data-stu-id="f44c4-107">For a list of data stores supported as sinks by hello copy activity, see hello [Supported data stores](data-factory-data-movement-activities.md#supported-data-stores-and-formats) table.</span></span> <span data-ttu-id="f44c4-108">Data factory stöder för närvarande endast flytta data från en SAP Business Warehouse tooother data lagras, men inte för att flytta data från andra data lagras tooan SAP Business Warehouse.</span><span class="sxs-lookup"><span data-stu-id="f44c4-108">Data factory currently supports only moving data from an SAP Business Warehouse tooother data stores, but not for moving data from other data stores tooan SAP Business Warehouse.</span></span> 

## <a name="supported-versions-and-installation"></a><span data-ttu-id="f44c4-109">Versioner som stöds och installation</span><span class="sxs-lookup"><span data-stu-id="f44c4-109">Supported versions and installation</span></span>
<span data-ttu-id="f44c4-110">Den här anslutningen har stöd för SAP Business Warehouse version 7.x.</span><span class="sxs-lookup"><span data-stu-id="f44c4-110">This connector supports SAP Business Warehouse version 7.x.</span></span> <span data-ttu-id="f44c4-111">Det stöder kopiering av data från InfoCubes och QueryCubes (inklusive BEx frågor) med hjälp av MDX-frågor.</span><span class="sxs-lookup"><span data-stu-id="f44c4-111">It supports copying data from InfoCubes and QueryCubes (including BEx queries) using MDX queries.</span></span>

<span data-ttu-id="f44c4-112">tooenable hello anslutningen toohello SAP BW-instans, installera hello följande komponenter:</span><span class="sxs-lookup"><span data-stu-id="f44c4-112">tooenable hello connectivity toohello SAP BW instance, install hello following components:</span></span>
- <span data-ttu-id="f44c4-113">**Data Management Gateway**: Data Factory-tjänsten stöder anslutningar tooon lokala data Arkiv (inklusive SAP Business Warehouse) med hjälp av en komponent som kallas Data Management Gateway.</span><span class="sxs-lookup"><span data-stu-id="f44c4-113">**Data Management Gateway**: Data Factory service supports connecting tooon-premises data stores (including SAP Business Warehouse) using a component called Data Management Gateway.</span></span> <span data-ttu-id="f44c4-114">toolearn om Data Management Gateway och stegvisa instruktioner för hur du konfigurerar hello gateway finns [flytta data mellan lokala data lagra toocloud datalagret](data-factory-move-data-between-onprem-and-cloud.md) artikel.</span><span class="sxs-lookup"><span data-stu-id="f44c4-114">toolearn about Data Management Gateway and step-by-step instructions for setting up hello gateway, see [Moving data between on-premises data store toocloud data store](data-factory-move-data-between-onprem-and-cloud.md) article.</span></span> <span data-ttu-id="f44c4-115">Gateway krävs även om hello SAP Business Warehouse finns i en Azure IaaS-virtuella (VM).</span><span class="sxs-lookup"><span data-stu-id="f44c4-115">Gateway is required even if hello SAP Business Warehouse is hosted in an Azure IaaS virtual machine (VM).</span></span> <span data-ttu-id="f44c4-116">Du kan installera hello gateway på hello samma virtuella dator som hello data lagras eller på en annan virtuell dator så länge som hello gateway kan ansluta toohello databas.</span><span class="sxs-lookup"><span data-stu-id="f44c4-116">You can install hello gateway on hello same VM as hello data store or on a different VM as long as hello gateway can connect toohello database.</span></span>
- <span data-ttu-id="f44c4-117">**SAP NetWeaver biblioteket** på hello gateway-datorn.</span><span class="sxs-lookup"><span data-stu-id="f44c4-117">**SAP NetWeaver library** on hello gateway machine.</span></span> <span data-ttu-id="f44c4-118">Du kan hämta hello SAP Netweaver bibliotek från SAP-administratören eller direkt från hello [SAP Software Download Center](https://support.sap.com/swdc).</span><span class="sxs-lookup"><span data-stu-id="f44c4-118">You can get hello SAP Netweaver library from your SAP administrator, or directly from hello [SAP Software Download Center](https://support.sap.com/swdc).</span></span> <span data-ttu-id="f44c4-119">Sök efter hello **SAP Obs #1025361** tooget hello hämtningsplats för hello senaste versionen.</span><span class="sxs-lookup"><span data-stu-id="f44c4-119">Search for hello **SAP Note #1025361** tooget hello download location for hello most recent version.</span></span> <span data-ttu-id="f44c4-120">Kontrollera att hello arkitektur för hello SAP NetWeaver biblioteket (32-bitars eller 64-bitars) matchar din gateway-installation.</span><span class="sxs-lookup"><span data-stu-id="f44c4-120">Make sure that hello architecture for hello SAP NetWeaver library (32-bit or 64-bit) matches your gateway installation.</span></span> <span data-ttu-id="f44c4-121">Installera alla filer som ingår i hello SAP NetWeaver RFC SDK bl.a toohello SAP-kommentar.</span><span class="sxs-lookup"><span data-stu-id="f44c4-121">Then install all files included in hello SAP NetWeaver RFC SDK according toohello SAP Note.</span></span> <span data-ttu-id="f44c4-122">hello SAP NetWeaver biblioteket ingår också i hello SAP-klientverktyg installation.</span><span class="sxs-lookup"><span data-stu-id="f44c4-122">hello SAP NetWeaver library is also included in hello SAP Client Tools installation.</span></span>

> [!TIP]
> <span data-ttu-id="f44c4-123">Placera hello DLL-filer extraheras från hello NetWeaver RFC SDK i mappen system32.</span><span class="sxs-lookup"><span data-stu-id="f44c4-123">Put hello dlls extracted from hello NetWeaver RFC SDK into system32 folder.</span></span>

## <a name="getting-started"></a><span data-ttu-id="f44c4-124">Komma igång</span><span class="sxs-lookup"><span data-stu-id="f44c4-124">Getting started</span></span>
<span data-ttu-id="f44c4-125">Du kan skapa en pipeline med en kopia-aktivitet som flyttar data från en lokal Cassandra data store med hjälp av olika verktyg/API: er.</span><span class="sxs-lookup"><span data-stu-id="f44c4-125">You can create a pipeline with a copy activity that moves data from an on-premises Cassandra data store by using different tools/APIs.</span></span> 

- <span data-ttu-id="f44c4-126">hello enklaste sättet toocreate en pipeline är toouse hello **guiden Kopiera**.</span><span class="sxs-lookup"><span data-stu-id="f44c4-126">hello easiest way toocreate a pipeline is toouse hello **Copy Wizard**.</span></span> <span data-ttu-id="f44c4-127">Se [Självstudier: skapa en pipeline med hjälp av guiden Kopiera](data-factory-copy-data-wizard-tutorial.md) för en snabb genomgång om hur du skapar en pipeline med hjälp av guiden för hello kopiera data.</span><span class="sxs-lookup"><span data-stu-id="f44c4-127">See [Tutorial: Create a pipeline using Copy Wizard](data-factory-copy-data-wizard-tutorial.md) for a quick walkthrough on creating a pipeline using hello Copy data wizard.</span></span> 
- <span data-ttu-id="f44c4-128">Du kan också använda följande verktyg toocreate en pipeline hello: **Azure-portalen**, **Visual Studio**, **Azure PowerShell**, **Azure Resource Manager-mall** , **.NET API**, och **REST API**.</span><span class="sxs-lookup"><span data-stu-id="f44c4-128">You can also use hello following tools toocreate a pipeline: **Azure portal**, **Visual Studio**, **Azure PowerShell**, **Azure Resource Manager template**, **.NET API**, and **REST API**.</span></span> <span data-ttu-id="f44c4-129">Se [kopiera aktivitet kursen](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) för stegvisa instruktioner toocreate en pipeline med en Kopieringsaktivitet.</span><span class="sxs-lookup"><span data-stu-id="f44c4-129">See [Copy activity tutorial](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) for step-by-step instructions toocreate a pipeline with a copy activity.</span></span> 

<span data-ttu-id="f44c4-130">Om du använder hello verktyg eller API: er kan utföra du hello följande steg toocreate en pipeline som flyttar data från en källdata lagra tooa sink-datalagret:</span><span class="sxs-lookup"><span data-stu-id="f44c4-130">Whether you use hello tools or APIs, you perform hello following steps toocreate a pipeline that moves data from a source data store tooa sink data store:</span></span>

1. <span data-ttu-id="f44c4-131">Skapa **länkade tjänster** toolink indata och utdata lagrar tooyour data factory.</span><span class="sxs-lookup"><span data-stu-id="f44c4-131">Create **linked services** toolink input and output data stores tooyour data factory.</span></span>
2. <span data-ttu-id="f44c4-132">Skapa **datauppsättningar** toorepresent indata och utdata för hello kopieringsåtgärden.</span><span class="sxs-lookup"><span data-stu-id="f44c4-132">Create **datasets** toorepresent input and output data for hello copy operation.</span></span> 
3. <span data-ttu-id="f44c4-133">Skapa en **pipeline** med en kopia-aktivitet som tar en datamängd som indata och en dataset som utdata.</span><span class="sxs-lookup"><span data-stu-id="f44c4-133">Create a **pipeline** with a copy activity that takes a dataset as an input and a dataset as an output.</span></span> 

<span data-ttu-id="f44c4-134">När du använder guiden hello skapas automatiskt JSON definitioner för dessa Data Factory-enheter (länkade tjänster, datauppsättningar och hello pipeline) för dig.</span><span class="sxs-lookup"><span data-stu-id="f44c4-134">When you use hello wizard, JSON definitions for these Data Factory entities (linked services, datasets, and hello pipeline) are automatically created for you.</span></span> <span data-ttu-id="f44c4-135">När du använder Verktyg/API: er (utom .NET API), kan du definiera dessa Data Factory-enheter med hjälp av hello JSON-format.</span><span class="sxs-lookup"><span data-stu-id="f44c4-135">When you use tools/APIs (except .NET API), you define these Data Factory entities by using hello JSON format.</span></span>  <span data-ttu-id="f44c4-136">Ett exempel med JSON-definitioner för Data Factory-entiteter som ska använda toocopy data från en lokal SAP Business Warehouse finns [JSON-exempel: kopiera data från SAP Business Warehouse tooAzure Blob](#json-example-copy-data-from-sap-business-warehouse-to-azure-blob) i den här artikeln.</span><span class="sxs-lookup"><span data-stu-id="f44c4-136">For a sample with JSON definitions for Data Factory entities that are used toocopy data from an on-premises SAP Business Warehouse, see [JSON example: Copy data from SAP Business Warehouse tooAzure Blob](#json-example-copy-data-from-sap-business-warehouse-to-azure-blob) section of this article.</span></span> 

<span data-ttu-id="f44c4-137">hello följande avsnitt innehåller information om JSON-egenskaper används toodefine Data Factory entiteter specifika tooan SAP BW datalager:</span><span class="sxs-lookup"><span data-stu-id="f44c4-137">hello following sections provide details about JSON properties that are used toodefine Data Factory entities specific tooan SAP BW data store:</span></span>

## <a name="linked-service-properties"></a><span data-ttu-id="f44c4-138">Länkad tjänstegenskaper</span><span class="sxs-lookup"><span data-stu-id="f44c4-138">Linked service properties</span></span>
<span data-ttu-id="f44c4-139">hello följande tabell ger en beskrivning för JSON-element specifika tooSAP Business Warehouse (BW) länkade tjänsten.</span><span class="sxs-lookup"><span data-stu-id="f44c4-139">hello following table provides description for JSON elements specific tooSAP Business Warehouse (BW) linked service.</span></span>

<span data-ttu-id="f44c4-140">Egenskap</span><span class="sxs-lookup"><span data-stu-id="f44c4-140">Property</span></span> | <span data-ttu-id="f44c4-141">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="f44c4-141">Description</span></span> | <span data-ttu-id="f44c4-142">Tillåtna värden</span><span class="sxs-lookup"><span data-stu-id="f44c4-142">Allowed values</span></span> | <span data-ttu-id="f44c4-143">Krävs</span><span class="sxs-lookup"><span data-stu-id="f44c4-143">Required</span></span>
-------- | ----------- | -------------- | --------
<span data-ttu-id="f44c4-144">server</span><span class="sxs-lookup"><span data-stu-id="f44c4-144">server</span></span> | <span data-ttu-id="f44c4-145">Namnet på hello-server på vilken hello SAP BW instansen finns.</span><span class="sxs-lookup"><span data-stu-id="f44c4-145">Name of hello server on which hello SAP BW instance resides.</span></span> | <span data-ttu-id="f44c4-146">Sträng</span><span class="sxs-lookup"><span data-stu-id="f44c4-146">string</span></span> | <span data-ttu-id="f44c4-147">Ja</span><span class="sxs-lookup"><span data-stu-id="f44c4-147">Yes</span></span>
<span data-ttu-id="f44c4-148">systemNumber</span><span class="sxs-lookup"><span data-stu-id="f44c4-148">systemNumber</span></span> | <span data-ttu-id="f44c4-149">System antal hello SAP BW system.</span><span class="sxs-lookup"><span data-stu-id="f44c4-149">System number of hello SAP BW system.</span></span> | <span data-ttu-id="f44c4-150">Två siffror decimaltal representeras som en sträng.</span><span class="sxs-lookup"><span data-stu-id="f44c4-150">Two-digit decimal number represented as a string.</span></span> | <span data-ttu-id="f44c4-151">Ja</span><span class="sxs-lookup"><span data-stu-id="f44c4-151">Yes</span></span>
<span data-ttu-id="f44c4-152">clientId</span><span class="sxs-lookup"><span data-stu-id="f44c4-152">clientId</span></span> | <span data-ttu-id="f44c4-153">Klient-ID för hello klienten i hello SAP W system.</span><span class="sxs-lookup"><span data-stu-id="f44c4-153">Client ID of hello client in hello SAP W system.</span></span> | <span data-ttu-id="f44c4-154">Tre siffror decimaltal representeras som en sträng.</span><span class="sxs-lookup"><span data-stu-id="f44c4-154">Three-digit decimal number represented as a string.</span></span> | <span data-ttu-id="f44c4-155">Ja</span><span class="sxs-lookup"><span data-stu-id="f44c4-155">Yes</span></span>
<span data-ttu-id="f44c4-156">användarnamn</span><span class="sxs-lookup"><span data-stu-id="f44c4-156">username</span></span> | <span data-ttu-id="f44c4-157">Namnet på hello-användare som har åtkomst toohello SAP-server</span><span class="sxs-lookup"><span data-stu-id="f44c4-157">Name of hello user who has access toohello SAP server</span></span> | <span data-ttu-id="f44c4-158">Sträng</span><span class="sxs-lookup"><span data-stu-id="f44c4-158">string</span></span> | <span data-ttu-id="f44c4-159">Ja</span><span class="sxs-lookup"><span data-stu-id="f44c4-159">Yes</span></span>
<span data-ttu-id="f44c4-160">lösenord</span><span class="sxs-lookup"><span data-stu-id="f44c4-160">password</span></span> | <span data-ttu-id="f44c4-161">Lösenordet för hello.</span><span class="sxs-lookup"><span data-stu-id="f44c4-161">Password for hello user.</span></span> | <span data-ttu-id="f44c4-162">Sträng</span><span class="sxs-lookup"><span data-stu-id="f44c4-162">string</span></span> | <span data-ttu-id="f44c4-163">Ja</span><span class="sxs-lookup"><span data-stu-id="f44c4-163">Yes</span></span>
<span data-ttu-id="f44c4-164">gatewayName</span><span class="sxs-lookup"><span data-stu-id="f44c4-164">gatewayName</span></span> | <span data-ttu-id="f44c4-165">Namnet på hello-gateway som hello Data Factory-tjänsten ska använda tooconnect toohello lokal SAP BW instans.</span><span class="sxs-lookup"><span data-stu-id="f44c4-165">Name of hello gateway that hello Data Factory service should use tooconnect toohello on-premises SAP BW instance.</span></span> | <span data-ttu-id="f44c4-166">Sträng</span><span class="sxs-lookup"><span data-stu-id="f44c4-166">string</span></span> | <span data-ttu-id="f44c4-167">Ja</span><span class="sxs-lookup"><span data-stu-id="f44c4-167">Yes</span></span>
<span data-ttu-id="f44c4-168">encryptedCredential</span><span class="sxs-lookup"><span data-stu-id="f44c4-168">encryptedCredential</span></span> | <span data-ttu-id="f44c4-169">hello krypterade autentiseringsuppgifter strängen.</span><span class="sxs-lookup"><span data-stu-id="f44c4-169">hello encrypted credential string.</span></span> | <span data-ttu-id="f44c4-170">Sträng</span><span class="sxs-lookup"><span data-stu-id="f44c4-170">string</span></span> | <span data-ttu-id="f44c4-171">Nej</span><span class="sxs-lookup"><span data-stu-id="f44c4-171">No</span></span>

## <a name="dataset-properties"></a><span data-ttu-id="f44c4-172">Egenskaper för datamängd</span><span class="sxs-lookup"><span data-stu-id="f44c4-172">Dataset properties</span></span>
<span data-ttu-id="f44c4-173">En fullständig lista över egenskaper som är tillgängliga för att definiera datauppsättningarna & avsnitt finns hello [skapa datauppsättningar](data-factory-create-datasets.md) artikel.</span><span class="sxs-lookup"><span data-stu-id="f44c4-173">For a full list of sections & properties available for defining datasets, see hello [Creating datasets](data-factory-create-datasets.md) article.</span></span> <span data-ttu-id="f44c4-174">Avsnitt som struktur, tillgänglighet och princip på en datamängd JSON är liknande för alla typer av dataset (Azure SQL Azure blob, Azure-tabellen, osv.).</span><span class="sxs-lookup"><span data-stu-id="f44c4-174">Sections such as structure, availability, and policy of a dataset JSON are similar for all dataset types (Azure SQL, Azure blob, Azure table, etc.).</span></span>

<span data-ttu-id="f44c4-175">Hej **typeProperties** avsnitt är olika för varje typ av dataset och ger information om hello platsen för hello data i datalagret hello.</span><span class="sxs-lookup"><span data-stu-id="f44c4-175">hello **typeProperties** section is different for each type of dataset and provides information about hello location of hello data in hello data store.</span></span> <span data-ttu-id="f44c4-176">Det finns inga typspecifika egenskaper som stöds för hello SAP BW dataset av typen **RelationalTable**.</span><span class="sxs-lookup"><span data-stu-id="f44c4-176">There are no type-specific properties supported for hello SAP BW dataset of type **RelationalTable**.</span></span> 


## <a name="copy-activity-properties"></a><span data-ttu-id="f44c4-177">Kopiera egenskaper för aktivitet</span><span class="sxs-lookup"><span data-stu-id="f44c4-177">Copy activity properties</span></span>
<span data-ttu-id="f44c4-178">En fullständig lista över avsnitt & egenskaper som är tillgängliga för att definiera aktiviteter finns hello [skapar Pipelines](data-factory-create-pipelines.md) artikel.</span><span class="sxs-lookup"><span data-stu-id="f44c4-178">For a full list of sections & properties available for defining activities, see hello [Creating Pipelines](data-factory-create-pipelines.md) article.</span></span> <span data-ttu-id="f44c4-179">Egenskaper, till exempel namn, beskrivning, inkommande och utgående tabeller är principer är tillgängliga för alla typer av aktiviteter.</span><span class="sxs-lookup"><span data-stu-id="f44c4-179">Properties such as name, description, input and output tables, are policies are available for all types of activities.</span></span>

<span data-ttu-id="f44c4-180">Medan egenskaper som är tillgängliga i hello **typeProperties** avsnittet hello aktivitet varierar med varje aktivitetstyp.</span><span class="sxs-lookup"><span data-stu-id="f44c4-180">Whereas, properties available in hello **typeProperties** section of hello activity vary with each activity type.</span></span> <span data-ttu-id="f44c4-181">För Kopieringsaktivitet kan varierar de beroende på hello typer av datakällor och sänkor.</span><span class="sxs-lookup"><span data-stu-id="f44c4-181">For Copy activity, they vary depending on hello types of sources and sinks.</span></span>

<span data-ttu-id="f44c4-182">När datakällan i en Kopieringsaktivitet är av typen **RelationalSource** (som innehåller SAP BW), hello följande egenskaper finns i avsnittet typeProperties:</span><span class="sxs-lookup"><span data-stu-id="f44c4-182">When source in copy activity is of type **RelationalSource** (which includes SAP BW), hello following properties are available in typeProperties section:</span></span>

| <span data-ttu-id="f44c4-183">Egenskap</span><span class="sxs-lookup"><span data-stu-id="f44c4-183">Property</span></span> | <span data-ttu-id="f44c4-184">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="f44c4-184">Description</span></span> | <span data-ttu-id="f44c4-185">Tillåtna värden</span><span class="sxs-lookup"><span data-stu-id="f44c4-185">Allowed values</span></span> | <span data-ttu-id="f44c4-186">Krävs</span><span class="sxs-lookup"><span data-stu-id="f44c4-186">Required</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="f44c4-187">DocumentDB</span><span class="sxs-lookup"><span data-stu-id="f44c4-187">query</span></span> | <span data-ttu-id="f44c4-188">Anger hello MDX-fråga tooread data från hello SAP BW-instans.</span><span class="sxs-lookup"><span data-stu-id="f44c4-188">Specifies hello MDX query tooread data from hello SAP BW instance.</span></span> | <span data-ttu-id="f44c4-189">MDX-fråga.</span><span class="sxs-lookup"><span data-stu-id="f44c4-189">MDX query.</span></span> | <span data-ttu-id="f44c4-190">Ja</span><span class="sxs-lookup"><span data-stu-id="f44c4-190">Yes</span></span> |


## <a name="json-example-copy-data-from-sap-business-warehouse-tooazure-blob"></a><span data-ttu-id="f44c4-191">JSON-exempel: kopiera data från SAP Business Warehouse tooAzure Blob</span><span class="sxs-lookup"><span data-stu-id="f44c4-191">JSON example: Copy data from SAP Business Warehouse tooAzure Blob</span></span>
<span data-ttu-id="f44c4-192">hello följande exempel innehåller exempel JSON definitioner som du kan använda toocreate en pipeline med hjälp av [Azure-portalen](data-factory-copy-activity-tutorial-using-azure-portal.md) eller [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md) eller [Azure PowerShell](data-factory-copy-activity-tutorial-using-powershell.md).</span><span class="sxs-lookup"><span data-stu-id="f44c4-192">hello following example provides sample JSON definitions that you can use toocreate a pipeline by using [Azure portal](data-factory-copy-activity-tutorial-using-azure-portal.md) or [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md) or [Azure PowerShell](data-factory-copy-activity-tutorial-using-powershell.md).</span></span> <span data-ttu-id="f44c4-193">Det här exemplet visas hur toocopy data från en lokal SAP Business Warehouse tooan Azure Blob Storage.</span><span class="sxs-lookup"><span data-stu-id="f44c4-193">This sample shows how toocopy data from an on-premises SAP Business Warehouse tooan Azure Blob Storage.</span></span> <span data-ttu-id="f44c4-194">Dock datan kan kopieras **direkt** tooany av hello sänkor anges [här](data-factory-data-movement-activities.md#supported-data-stores-and-formats) med hello Kopieringsaktiviteten i Azure Data Factory.</span><span class="sxs-lookup"><span data-stu-id="f44c4-194">However, data can be copied **directly** tooany of hello sinks stated [here](data-factory-data-movement-activities.md#supported-data-stores-and-formats) using hello Copy Activity in Azure Data Factory.</span></span>  

> [!IMPORTANT]
> <span data-ttu-id="f44c4-195">Det här exemplet innehåller JSON kodavsnitt.</span><span class="sxs-lookup"><span data-stu-id="f44c4-195">This sample provides JSON snippets.</span></span> <span data-ttu-id="f44c4-196">Stegvisa instruktioner för att skapa datafabriken hello inkluderas inte.</span><span class="sxs-lookup"><span data-stu-id="f44c4-196">It does not include step-by-step instructions for creating hello data factory.</span></span> <span data-ttu-id="f44c4-197">Se [flytta data mellan lokala platser och moln](data-factory-move-data-between-onprem-and-cloud.md) artikel stegvisa instruktioner.</span><span class="sxs-lookup"><span data-stu-id="f44c4-197">See [moving data between on-premises locations and cloud](data-factory-move-data-between-onprem-and-cloud.md) article for step-by-step instructions.</span></span>

<span data-ttu-id="f44c4-198">hello exemplet har hello följande data factory-enheter:</span><span class="sxs-lookup"><span data-stu-id="f44c4-198">hello sample has hello following data factory entities:</span></span>

1. <span data-ttu-id="f44c4-199">En länkad tjänst av typen [SapBw](#linked-service-properties).</span><span class="sxs-lookup"><span data-stu-id="f44c4-199">A linked service of type [SapBw](#linked-service-properties).</span></span>
2. <span data-ttu-id="f44c4-200">En länkad tjänst av typen [AzureStorage](data-factory-azure-blob-connector.md#linked-service-properties).</span><span class="sxs-lookup"><span data-stu-id="f44c4-200">A linked service of type [AzureStorage](data-factory-azure-blob-connector.md#linked-service-properties).</span></span>
3. <span data-ttu-id="f44c4-201">Indata [dataset](data-factory-create-datasets.md) av typen [RelationalTable](#dataset-properties).</span><span class="sxs-lookup"><span data-stu-id="f44c4-201">An input [dataset](data-factory-create-datasets.md) of type [RelationalTable](#dataset-properties).</span></span>
4. <span data-ttu-id="f44c4-202">Utdata [dataset](data-factory-create-datasets.md) av typen [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties).</span><span class="sxs-lookup"><span data-stu-id="f44c4-202">An output [dataset](data-factory-create-datasets.md) of type [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties).</span></span>
5. <span data-ttu-id="f44c4-203">En [pipeline](data-factory-create-pipelines.md) med Kopieringsaktiviteten som använder [RelationalSource](#copy-activity-properties) och [BlobSink](data-factory-azure-blob-connector.md#copy-activity-properties).</span><span class="sxs-lookup"><span data-stu-id="f44c4-203">A [pipeline](data-factory-create-pipelines.md) with Copy Activity that uses [RelationalSource](#copy-activity-properties) and [BlobSink](data-factory-azure-blob-connector.md#copy-activity-properties).</span></span>

<span data-ttu-id="f44c4-204">hello exemplet kopierar data från en SAP Business Warehouse-instans tooan Azure blob varje timme.</span><span class="sxs-lookup"><span data-stu-id="f44c4-204">hello sample copies data from an SAP Business Warehouse instance tooan Azure blob hourly.</span></span> <span data-ttu-id="f44c4-205">hello JSON egenskaper som används i exemplen beskrivs i hello-exempel i följande avsnitt.</span><span class="sxs-lookup"><span data-stu-id="f44c4-205">hello JSON properties used in these samples are described in sections following hello samples.</span></span>

<span data-ttu-id="f44c4-206">Som ett första steg bör du konfigurera hello data management gateway.</span><span class="sxs-lookup"><span data-stu-id="f44c4-206">As a first step, setup hello data management gateway.</span></span> <span data-ttu-id="f44c4-207">hello anvisningar finns i hello [flytta data mellan lokala platser och moln](data-factory-move-data-between-onprem-and-cloud.md) artikel.</span><span class="sxs-lookup"><span data-stu-id="f44c4-207">hello instructions are in hello [moving data between on-premises locations and cloud](data-factory-move-data-between-onprem-and-cloud.md) article.</span></span>

### <a name="sap-business-warehouse-linked-service"></a><span data-ttu-id="f44c4-208">SAP Business Warehouse länkade tjänsten</span><span class="sxs-lookup"><span data-stu-id="f44c4-208">SAP Business Warehouse linked service</span></span>
<span data-ttu-id="f44c4-209">Den här länkade tjänsten länkar din SAP BW instans toohello data factory.</span><span class="sxs-lookup"><span data-stu-id="f44c4-209">This linked service links your SAP BW instance toohello data factory.</span></span> <span data-ttu-id="f44c4-210">hello typegenskapen har ställts in för**SapBw**.</span><span class="sxs-lookup"><span data-stu-id="f44c4-210">hello type property is set too**SapBw**.</span></span> <span data-ttu-id="f44c4-211">Hej typeProperties avsnittet innehåller anslutningsinformation för hello SAP BW-instans.</span><span class="sxs-lookup"><span data-stu-id="f44c4-211">hello typeProperties section provides connection information for hello SAP BW instance.</span></span> 

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

### <a name="azure-storage-linked-service"></a><span data-ttu-id="f44c4-212">Länkad Azure-lagringstjänst</span><span class="sxs-lookup"><span data-stu-id="f44c4-212">Azure Storage linked service</span></span>
<span data-ttu-id="f44c4-213">Det här länkade tjänsten länkar din Azure Storage-konto toohello data factory.</span><span class="sxs-lookup"><span data-stu-id="f44c4-213">This linked service links your Azure Storage account toohello data factory.</span></span> <span data-ttu-id="f44c4-214">hello typegenskapen har ställts in för**AzureStorage**.</span><span class="sxs-lookup"><span data-stu-id="f44c4-214">hello type property is set too**AzureStorage**.</span></span> <span data-ttu-id="f44c4-215">Hej typeProperties avsnittet innehåller anslutningsinformation för hello Azure Storage-konto.</span><span class="sxs-lookup"><span data-stu-id="f44c4-215">hello typeProperties section provides connection information for hello Azure Storage account.</span></span>

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

### <a name="sap-bw-input-dataset"></a><span data-ttu-id="f44c4-216">SAP BW inkommande dataset</span><span class="sxs-lookup"><span data-stu-id="f44c4-216">SAP BW input dataset</span></span>
<span data-ttu-id="f44c4-217">Den här datauppsättningen definierar hello SAP Business Warehouse dataset.</span><span class="sxs-lookup"><span data-stu-id="f44c4-217">This dataset defines hello SAP Business Warehouse dataset.</span></span> <span data-ttu-id="f44c4-218">Du ställer in hello på hello Data Factory datamängd för**RelationalTable**.</span><span class="sxs-lookup"><span data-stu-id="f44c4-218">You set hello type of hello Data Factory dataset too**RelationalTable**.</span></span> <span data-ttu-id="f44c4-219">För närvarande kan anger du inte några typspecifika egenskaper för en SAP BW datauppsättning.</span><span class="sxs-lookup"><span data-stu-id="f44c4-219">Currently, you do not specify any type-specific properties for an SAP BW dataset.</span></span> <span data-ttu-id="f44c4-220">hello frågan i hello kopiera aktivitetsdefinitionen anger vilka data tooread från hello SAP BW-instans.</span><span class="sxs-lookup"><span data-stu-id="f44c4-220">hello query in hello Copy Activity definition specifies what data tooread from hello SAP BW instance.</span></span> 

<span data-ttu-id="f44c4-221">Ange externa egenskapen tootrue informerar hello Data Factory-tjänsten hello tabellen är externa toohello data factory och inte tillverkas av en aktivitet i hello data factory.</span><span class="sxs-lookup"><span data-stu-id="f44c4-221">Setting external property tootrue informs hello Data Factory service that hello table is external toohello data factory and is not produced by an activity in hello data factory.</span></span>

<span data-ttu-id="f44c4-222">Frekvensen och intervall egenskaper definierar hello schema.</span><span class="sxs-lookup"><span data-stu-id="f44c4-222">Frequency and interval properties defines hello schedule.</span></span> <span data-ttu-id="f44c4-223">I det här fallet läses hello data från hello SAP BW instans varje timme.</span><span class="sxs-lookup"><span data-stu-id="f44c4-223">In this case, hello data is read from hello SAP BW instance hourly.</span></span> 

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



### <a name="azure-blob-output-dataset"></a><span data-ttu-id="f44c4-224">Utdatauppsättning för Azure-blobb</span><span class="sxs-lookup"><span data-stu-id="f44c4-224">Azure Blob output dataset</span></span>
<span data-ttu-id="f44c4-225">Den här datauppsättningen definierar hello utdata Azure-blobbdatauppsättning.</span><span class="sxs-lookup"><span data-stu-id="f44c4-225">This dataset defines hello output Azure Blob dataset.</span></span> <span data-ttu-id="f44c4-226">hello typegenskapen anges tooAzureBlob.</span><span class="sxs-lookup"><span data-stu-id="f44c4-226">hello type property is set tooAzureBlob.</span></span> <span data-ttu-id="f44c4-227">Hej typeProperties avsnittet innehåller hello data kopieras från hello SAP BW instans ska lagras.</span><span class="sxs-lookup"><span data-stu-id="f44c4-227">hello typeProperties section provides where hello data copied from hello SAP BW instance is stored.</span></span> <span data-ttu-id="f44c4-228">hello data skrivs tooa nya blob varje timme (frekvens: timme, intervall: 1).</span><span class="sxs-lookup"><span data-stu-id="f44c4-228">hello data is written tooa new blob every hour (frequency: hour, interval: 1).</span></span> <span data-ttu-id="f44c4-229">hello mappsökväg för hello blob utvärderas dynamiskt baserat på hello starttiden för hello-segment som bearbetas.</span><span class="sxs-lookup"><span data-stu-id="f44c4-229">hello folder path for hello blob is dynamically evaluated based on hello start time of hello slice that is being processed.</span></span> <span data-ttu-id="f44c4-230">hello mappsökväg använder år, månad, dag och timmar delar av hello starttid.</span><span class="sxs-lookup"><span data-stu-id="f44c4-230">hello folder path uses year, month, day, and hours parts of hello start time.</span></span>

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


### <a name="pipeline-with-copy-activity"></a><span data-ttu-id="f44c4-231">Pipeline med kopieringsaktiviteten</span><span class="sxs-lookup"><span data-stu-id="f44c4-231">Pipeline with Copy activity</span></span>
<span data-ttu-id="f44c4-232">hello pipelinen innehåller en kopia-aktivitet som är konfigurerade toouse hello inkommande och utgående datauppsättningar och är schemalagda toorun varje timme.</span><span class="sxs-lookup"><span data-stu-id="f44c4-232">hello pipeline contains a Copy Activity that is configured toouse hello input and output datasets and is scheduled toorun every hour.</span></span> <span data-ttu-id="f44c4-233">I hello pipeline JSON-definitionen hello **källa** typ har angetts för**RelationalSource** (för SAP BW källa) och **sink** typ har angetts för**BlobSink**.</span><span class="sxs-lookup"><span data-stu-id="f44c4-233">In hello pipeline JSON definition, hello **source** type is set too**RelationalSource** (for SAP BW source) and **sink** type is set too**BlobSink**.</span></span> <span data-ttu-id="f44c4-234">hello-fråga som angetts för hello **frågan** egenskapen väljer hello data i hello tidigare timme toocopy.</span><span class="sxs-lookup"><span data-stu-id="f44c4-234">hello query specified for hello **query** property selects hello data in hello past hour toocopy.</span></span>

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



### <a name="type-mapping-for-sap-bw"></a><span data-ttu-id="f44c4-235">Mappning för SAP BW</span><span class="sxs-lookup"><span data-stu-id="f44c4-235">Type mapping for SAP BW</span></span>
<span data-ttu-id="f44c4-236">Som anges i hello [data movement aktiviteter](data-factory-data-movement-activities.md) artikeln kopieringsaktiviteten utför automatisk konverteringar från källan typer toosink typer med hello följande två sätt:</span><span class="sxs-lookup"><span data-stu-id="f44c4-236">As mentioned in hello [data movement activities](data-factory-data-movement-activities.md) article, Copy activity performs automatic type conversions from source types toosink types with hello following two-step approach:</span></span>

1. <span data-ttu-id="f44c4-237">Konvertera från inbyggda typer too.NET källtypen</span><span class="sxs-lookup"><span data-stu-id="f44c4-237">Convert from native source types too.NET type</span></span>
2. <span data-ttu-id="f44c4-238">Konvertera från .NET typen toonative Mottagartypen</span><span class="sxs-lookup"><span data-stu-id="f44c4-238">Convert from .NET type toonative sink type</span></span>

<span data-ttu-id="f44c4-239">När du flyttar data från SAP BW används hello följande mappningar från SAP BW typer too.NET typer.</span><span class="sxs-lookup"><span data-stu-id="f44c4-239">When moving data from SAP BW, hello following mappings are used from SAP BW types too.NET types.</span></span>

<span data-ttu-id="f44c4-240">Datatypen i hello ABAP ordlista</span><span class="sxs-lookup"><span data-stu-id="f44c4-240">Data type in hello ABAP Dictionary</span></span> | <span data-ttu-id="f44c4-241">.NET-datatyp</span><span class="sxs-lookup"><span data-stu-id="f44c4-241">.Net Data Type</span></span>
-------------------------------- | --------------
<span data-ttu-id="f44c4-242">ACCP</span><span class="sxs-lookup"><span data-stu-id="f44c4-242">ACCP</span></span> |  <span data-ttu-id="f44c4-243">int</span><span class="sxs-lookup"><span data-stu-id="f44c4-243">Int</span></span>
<span data-ttu-id="f44c4-244">CHAR</span><span class="sxs-lookup"><span data-stu-id="f44c4-244">CHAR</span></span> | <span data-ttu-id="f44c4-245">Sträng</span><span class="sxs-lookup"><span data-stu-id="f44c4-245">String</span></span>
<span data-ttu-id="f44c4-246">CLNT</span><span class="sxs-lookup"><span data-stu-id="f44c4-246">CLNT</span></span> | <span data-ttu-id="f44c4-247">Sträng</span><span class="sxs-lookup"><span data-stu-id="f44c4-247">String</span></span>
<span data-ttu-id="f44c4-248">AKTUELLT DATUM</span><span class="sxs-lookup"><span data-stu-id="f44c4-248">CURR</span></span> | <span data-ttu-id="f44c4-249">Decimal</span><span class="sxs-lookup"><span data-stu-id="f44c4-249">Decimal</span></span>
<span data-ttu-id="f44c4-250">CUKY</span><span class="sxs-lookup"><span data-stu-id="f44c4-250">CUKY</span></span> | <span data-ttu-id="f44c4-251">Sträng</span><span class="sxs-lookup"><span data-stu-id="f44c4-251">String</span></span>
<span data-ttu-id="f44c4-252">DEC</span><span class="sxs-lookup"><span data-stu-id="f44c4-252">DEC</span></span> | <span data-ttu-id="f44c4-253">Decimal</span><span class="sxs-lookup"><span data-stu-id="f44c4-253">Decimal</span></span>
<span data-ttu-id="f44c4-254">FLTP</span><span class="sxs-lookup"><span data-stu-id="f44c4-254">FLTP</span></span> | <span data-ttu-id="f44c4-255">dubbla</span><span class="sxs-lookup"><span data-stu-id="f44c4-255">Double</span></span>
<span data-ttu-id="f44c4-256">INT1</span><span class="sxs-lookup"><span data-stu-id="f44c4-256">INT1</span></span> | <span data-ttu-id="f44c4-257">Mottagna byte</span><span class="sxs-lookup"><span data-stu-id="f44c4-257">Byte</span></span>
<span data-ttu-id="f44c4-258">INT2</span><span class="sxs-lookup"><span data-stu-id="f44c4-258">INT2</span></span> | <span data-ttu-id="f44c4-259">Int16</span><span class="sxs-lookup"><span data-stu-id="f44c4-259">Int16</span></span>
<span data-ttu-id="f44c4-260">INT4</span><span class="sxs-lookup"><span data-stu-id="f44c4-260">INT4</span></span> | <span data-ttu-id="f44c4-261">int</span><span class="sxs-lookup"><span data-stu-id="f44c4-261">Int</span></span>
<span data-ttu-id="f44c4-262">LANG</span><span class="sxs-lookup"><span data-stu-id="f44c4-262">LANG</span></span> | <span data-ttu-id="f44c4-263">Sträng</span><span class="sxs-lookup"><span data-stu-id="f44c4-263">String</span></span>
<span data-ttu-id="f44c4-264">LCHR</span><span class="sxs-lookup"><span data-stu-id="f44c4-264">LCHR</span></span> | <span data-ttu-id="f44c4-265">Sträng</span><span class="sxs-lookup"><span data-stu-id="f44c4-265">String</span></span>
<span data-ttu-id="f44c4-266">LRAW</span><span class="sxs-lookup"><span data-stu-id="f44c4-266">LRAW</span></span> | <span data-ttu-id="f44c4-267">byte]</span><span class="sxs-lookup"><span data-stu-id="f44c4-267">Byte[]</span></span>
<span data-ttu-id="f44c4-268">PREC</span><span class="sxs-lookup"><span data-stu-id="f44c4-268">PREC</span></span> | <span data-ttu-id="f44c4-269">Int16</span><span class="sxs-lookup"><span data-stu-id="f44c4-269">Int16</span></span>
<span data-ttu-id="f44c4-270">QUAN</span><span class="sxs-lookup"><span data-stu-id="f44c4-270">QUAN</span></span> | <span data-ttu-id="f44c4-271">Decimal</span><span class="sxs-lookup"><span data-stu-id="f44c4-271">Decimal</span></span>
<span data-ttu-id="f44c4-272">RÅDATA</span><span class="sxs-lookup"><span data-stu-id="f44c4-272">RAW</span></span> | <span data-ttu-id="f44c4-273">byte]</span><span class="sxs-lookup"><span data-stu-id="f44c4-273">Byte[]</span></span>
<span data-ttu-id="f44c4-274">RAWSTRING</span><span class="sxs-lookup"><span data-stu-id="f44c4-274">RAWSTRING</span></span> | <span data-ttu-id="f44c4-275">byte]</span><span class="sxs-lookup"><span data-stu-id="f44c4-275">Byte[]</span></span>
<span data-ttu-id="f44c4-276">STRÄNG</span><span class="sxs-lookup"><span data-stu-id="f44c4-276">STRING</span></span> | <span data-ttu-id="f44c4-277">Sträng</span><span class="sxs-lookup"><span data-stu-id="f44c4-277">String</span></span>
<span data-ttu-id="f44c4-278">ENHET</span><span class="sxs-lookup"><span data-stu-id="f44c4-278">UNIT</span></span> | <span data-ttu-id="f44c4-279">Sträng</span><span class="sxs-lookup"><span data-stu-id="f44c4-279">String</span></span>
<span data-ttu-id="f44c4-280">DATS</span><span class="sxs-lookup"><span data-stu-id="f44c4-280">DATS</span></span> | <span data-ttu-id="f44c4-281">Sträng</span><span class="sxs-lookup"><span data-stu-id="f44c4-281">String</span></span>
<span data-ttu-id="f44c4-282">NUMC</span><span class="sxs-lookup"><span data-stu-id="f44c4-282">NUMC</span></span> | <span data-ttu-id="f44c4-283">Sträng</span><span class="sxs-lookup"><span data-stu-id="f44c4-283">String</span></span>
<span data-ttu-id="f44c4-284">TIMS</span><span class="sxs-lookup"><span data-stu-id="f44c4-284">TIMS</span></span> | <span data-ttu-id="f44c4-285">Sträng</span><span class="sxs-lookup"><span data-stu-id="f44c4-285">String</span></span>

> [!NOTE]
> <span data-ttu-id="f44c4-286">toomap kolumner från källan dataset toocolumns från sink dataset finns [mappa dataset kolumner i Azure Data Factory](data-factory-map-columns.md).</span><span class="sxs-lookup"><span data-stu-id="f44c4-286">toomap columns from source dataset toocolumns from sink dataset, see [Mapping dataset columns in Azure Data Factory](data-factory-map-columns.md).</span></span>


## <a name="map-source-toosink-columns"></a><span data-ttu-id="f44c4-287">Mappa källkolumner toosink</span><span class="sxs-lookup"><span data-stu-id="f44c4-287">Map source toosink columns</span></span>
<span data-ttu-id="f44c4-288">toolearn mappning tabellkolumner i källan dataset toocolumns i sink dataset finns [mappa dataset kolumner i Azure Data Factory](data-factory-map-columns.md).</span><span class="sxs-lookup"><span data-stu-id="f44c4-288">toolearn about mapping columns in source dataset toocolumns in sink dataset, see [Mapping dataset columns in Azure Data Factory](data-factory-map-columns.md).</span></span>

## <a name="repeatable-read-from-relational-sources"></a><span data-ttu-id="f44c4-289">Upprepbar läsning från relationella källor</span><span class="sxs-lookup"><span data-stu-id="f44c4-289">Repeatable read from relational sources</span></span>
<span data-ttu-id="f44c4-290">När du kopierar data från relationella datalager, Kom ihåg tooavoid repeterbarhet oönskade resultat.</span><span class="sxs-lookup"><span data-stu-id="f44c4-290">When copying data from relational data stores, keep repeatability in mind tooavoid unintended outcomes.</span></span> <span data-ttu-id="f44c4-291">I Azure Data Factory, kan du köra en sektor manuellt.</span><span class="sxs-lookup"><span data-stu-id="f44c4-291">In Azure Data Factory, you can rerun a slice manually.</span></span> <span data-ttu-id="f44c4-292">Du kan också konfigurera i principen för en dataset så att ett segment som körs när ett fel uppstår.</span><span class="sxs-lookup"><span data-stu-id="f44c4-292">You can also configure retry policy for a dataset so that a slice is rerun when a failure occurs.</span></span> <span data-ttu-id="f44c4-293">När ett segment körs på antingen sätt måste toomake att som hello samma data läses oavsett hur många gånger ett segment körs.</span><span class="sxs-lookup"><span data-stu-id="f44c4-293">When a slice is rerun in either way, you need toomake sure that hello same data is read no matter how many times a slice is run.</span></span> <span data-ttu-id="f44c4-294">Se [Repeatable läsa från relationella källor](data-factory-repeatable-copy.md#repeatable-read-from-relational-sources)</span><span class="sxs-lookup"><span data-stu-id="f44c4-294">See [Repeatable read from relational sources](data-factory-repeatable-copy.md#repeatable-read-from-relational-sources)</span></span>

## <a name="performance-and-tuning"></a><span data-ttu-id="f44c4-295">Prestanda och finjustering</span><span class="sxs-lookup"><span data-stu-id="f44c4-295">Performance and Tuning</span></span>
<span data-ttu-id="f44c4-296">Se [kopiera aktivitet prestanda och justera guiden](data-factory-copy-activity-performance.md) toolearn om nyckeln faktorer som påverkan prestanda för flytt av data (Kopieringsaktiviteten) i Azure Data Factory och olika sätt toooptimize den.</span><span class="sxs-lookup"><span data-stu-id="f44c4-296">See [Copy Activity Performance & Tuning Guide](data-factory-copy-activity-performance.md) toolearn about key factors that impact performance of data movement (Copy Activity) in Azure Data Factory and various ways toooptimize it.</span></span>
