---
title: "aaaMove data från SAP HANA med Azure Data Factory | Microsoft Docs"
description: "Mer information om hur toomove data från SAP HANA med Azure Data Factory."
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
ms.date: 06/04/2017
ms.author: jingwang
ms.openlocfilehash: 5cefe4c8ed01ea4e86e02496b2f8a9083d0b949c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="move-data-from-sap-hana-using-azure-data-factory"></a><span data-ttu-id="a254f-103">Flytta data från SAP HANA med Azure Data Factory</span><span class="sxs-lookup"><span data-stu-id="a254f-103">Move data From SAP HANA using Azure Data Factory</span></span>
<span data-ttu-id="a254f-104">Den här artikeln förklarar hur toouse hello Kopieringsaktiviteten i Azure Data Factory toomove data från en lokal SAP HANA.</span><span class="sxs-lookup"><span data-stu-id="a254f-104">This article explains how toouse hello Copy Activity in Azure Data Factory toomove data from an on-premises SAP HANA.</span></span> <span data-ttu-id="a254f-105">Den bygger på hello [Data Movement aktiviteter](data-factory-data-movement-activities.md) artikel som ger en allmän översikt över dataflyttning hello kopieringsaktiviteten.</span><span class="sxs-lookup"><span data-stu-id="a254f-105">It builds on hello [Data Movement Activities](data-factory-data-movement-activities.md) article, which presents a general overview of data movement with hello copy activity.</span></span>

<span data-ttu-id="a254f-106">Du kan kopiera data från en lokal SAP HANA data store tooany stöds sink dataarkiv.</span><span class="sxs-lookup"><span data-stu-id="a254f-106">You can copy data from an on-premises SAP HANA data store tooany supported sink data store.</span></span> <span data-ttu-id="a254f-107">En lista över data lagras som stöds när egenskaperna av hello kopieringsaktiviteten Se hello [stöds datalager](data-factory-data-movement-activities.md#supported-data-stores-and-formats) tabell.</span><span class="sxs-lookup"><span data-stu-id="a254f-107">For a list of data stores supported as sinks by hello copy activity, see hello [Supported data stores](data-factory-data-movement-activities.md#supported-data-stores-and-formats) table.</span></span> <span data-ttu-id="a254f-108">Data factory stöder för närvarande endast flytta data från ett SAP HANA tooother lagrar, men inte för att flytta data från andra data lagras tooan SAP HANA.</span><span class="sxs-lookup"><span data-stu-id="a254f-108">Data factory currently supports only moving data from an SAP HANA tooother data stores, but not for moving data from other data stores tooan SAP HANA.</span></span>

## <a name="supported-versions-and-installation"></a><span data-ttu-id="a254f-109">Versioner som stöds och installation</span><span class="sxs-lookup"><span data-stu-id="a254f-109">Supported versions and installation</span></span>
<span data-ttu-id="a254f-110">Den här anslutningen har stöd för någon version av SAP HANA-databas.</span><span class="sxs-lookup"><span data-stu-id="a254f-110">This connector supports any version of SAP HANA database.</span></span> <span data-ttu-id="a254f-111">Det stöder kopiering av data från HANA informationsmodeller (till exempel analys- och vyer) och raden/kolumnen tabeller med SQL-frågor.</span><span class="sxs-lookup"><span data-stu-id="a254f-111">It supports copying data from HANA information models (such as Analytic and Calculation views) and Row/Column tables using SQL queries.</span></span>

<span data-ttu-id="a254f-112">tooenable hello anslutningen toohello SAP HANA-instans, installera hello följande komponenter:</span><span class="sxs-lookup"><span data-stu-id="a254f-112">tooenable hello connectivity toohello SAP HANA instance, install hello following components:</span></span>
- <span data-ttu-id="a254f-113">**Data Management Gateway**: Data Factory-tjänsten stöder anslutningar tooon lokala data Arkiv (inklusive SAP HANA) med hjälp av en komponent som kallas Data Management Gateway.</span><span class="sxs-lookup"><span data-stu-id="a254f-113">**Data Management Gateway**: Data Factory service supports connecting tooon-premises data stores (including SAP HANA) using a component called Data Management Gateway.</span></span> <span data-ttu-id="a254f-114">toolearn om Data Management Gateway och stegvisa instruktioner för hur du konfigurerar hello gateway finns [flytta data mellan lokala data lagra toocloud datalagret](data-factory-move-data-between-onprem-and-cloud.md) artikel.</span><span class="sxs-lookup"><span data-stu-id="a254f-114">toolearn about Data Management Gateway and step-by-step instructions for setting up hello gateway, see [Moving data between on-premises data store toocloud data store](data-factory-move-data-between-onprem-and-cloud.md) article.</span></span> <span data-ttu-id="a254f-115">Gateway krävs även om hello SAP HANA finns i en Azure IaaS-virtuella (VM).</span><span class="sxs-lookup"><span data-stu-id="a254f-115">Gateway is required even if hello SAP HANA is hosted in an Azure IaaS virtual machine (VM).</span></span> <span data-ttu-id="a254f-116">Du kan installera hello gateway på hello samma virtuella dator som hello data lagras eller på en annan virtuell dator så länge som hello gateway kan ansluta toohello databas.</span><span class="sxs-lookup"><span data-stu-id="a254f-116">You can install hello gateway on hello same VM as hello data store or on a different VM as long as hello gateway can connect toohello database.</span></span>
- <span data-ttu-id="a254f-117">**SAP HANA ODBC-drivrutinen** på hello gateway-datorn.</span><span class="sxs-lookup"><span data-stu-id="a254f-117">**SAP HANA ODBC driver** on hello gateway machine.</span></span> <span data-ttu-id="a254f-118">Du kan hämta hello SAP HANA ODBC-drivrutinen från hello [SAP Software Download Center](https://support.sap.com/swdc).</span><span class="sxs-lookup"><span data-stu-id="a254f-118">You can download hello SAP HANA ODBC driver from hello [SAP Software Download Center](https://support.sap.com/swdc).</span></span> <span data-ttu-id="a254f-119">Sökning med hello nyckelordet **SAP HANA-klienten för Windows**.</span><span class="sxs-lookup"><span data-stu-id="a254f-119">Search with hello keyword **SAP HANA CLIENT for Windows**.</span></span> 

## <a name="getting-started"></a><span data-ttu-id="a254f-120">Komma igång</span><span class="sxs-lookup"><span data-stu-id="a254f-120">Getting started</span></span>
<span data-ttu-id="a254f-121">Du kan skapa en pipeline med en kopia-aktivitet som flyttar data från en lokal Cassandra data store med hjälp av olika verktyg/API: er.</span><span class="sxs-lookup"><span data-stu-id="a254f-121">You can create a pipeline with a copy activity that moves data from an on-premises Cassandra data store by using different tools/APIs.</span></span> 

- <span data-ttu-id="a254f-122">hello enklaste sättet toocreate en pipeline är toouse hello **guiden Kopiera**.</span><span class="sxs-lookup"><span data-stu-id="a254f-122">hello easiest way toocreate a pipeline is toouse hello **Copy Wizard**.</span></span> <span data-ttu-id="a254f-123">Se [Självstudier: skapa en pipeline med hjälp av guiden Kopiera](data-factory-copy-data-wizard-tutorial.md) för en snabb genomgång om hur du skapar en pipeline med hjälp av guiden för hello kopiera data.</span><span class="sxs-lookup"><span data-stu-id="a254f-123">See [Tutorial: Create a pipeline using Copy Wizard](data-factory-copy-data-wizard-tutorial.md) for a quick walkthrough on creating a pipeline using hello Copy data wizard.</span></span> 
- <span data-ttu-id="a254f-124">Du kan också använda följande verktyg toocreate en pipeline hello: **Azure-portalen**, **Visual Studio**, **Azure PowerShell**, **Azure Resource Manager-mall** , **.NET API**, och **REST API**.</span><span class="sxs-lookup"><span data-stu-id="a254f-124">You can also use hello following tools toocreate a pipeline: **Azure portal**, **Visual Studio**, **Azure PowerShell**, **Azure Resource Manager template**, **.NET API**, and **REST API**.</span></span> <span data-ttu-id="a254f-125">Se [kopiera aktivitet kursen](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) för stegvisa instruktioner toocreate en pipeline med en Kopieringsaktivitet.</span><span class="sxs-lookup"><span data-stu-id="a254f-125">See [Copy activity tutorial](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) for step-by-step instructions toocreate a pipeline with a copy activity.</span></span> 

<span data-ttu-id="a254f-126">Om du använder hello verktyg eller API: er kan utföra du hello följande steg toocreate en pipeline som flyttar data från en källdata lagra tooa sink-datalagret:</span><span class="sxs-lookup"><span data-stu-id="a254f-126">Whether you use hello tools or APIs, you perform hello following steps toocreate a pipeline that moves data from a source data store tooa sink data store:</span></span>

1. <span data-ttu-id="a254f-127">Skapa **länkade tjänster** toolink indata och utdata lagrar tooyour data factory.</span><span class="sxs-lookup"><span data-stu-id="a254f-127">Create **linked services** toolink input and output data stores tooyour data factory.</span></span>
2. <span data-ttu-id="a254f-128">Skapa **datauppsättningar** toorepresent indata och utdata för hello kopieringsåtgärden.</span><span class="sxs-lookup"><span data-stu-id="a254f-128">Create **datasets** toorepresent input and output data for hello copy operation.</span></span> 
3. <span data-ttu-id="a254f-129">Skapa en **pipeline** med en kopia-aktivitet som tar en datamängd som indata och en dataset som utdata.</span><span class="sxs-lookup"><span data-stu-id="a254f-129">Create a **pipeline** with a copy activity that takes a dataset as an input and a dataset as an output.</span></span> 

<span data-ttu-id="a254f-130">När du använder guiden hello skapas automatiskt JSON definitioner för dessa Data Factory-enheter (länkade tjänster, datauppsättningar och hello pipeline) för dig.</span><span class="sxs-lookup"><span data-stu-id="a254f-130">When you use hello wizard, JSON definitions for these Data Factory entities (linked services, datasets, and hello pipeline) are automatically created for you.</span></span> <span data-ttu-id="a254f-131">När du använder Verktyg/API: er (utom .NET API), kan du definiera dessa Data Factory-enheter med hjälp av hello JSON-format.</span><span class="sxs-lookup"><span data-stu-id="a254f-131">When you use tools/APIs (except .NET API), you define these Data Factory entities by using hello JSON format.</span></span>  <span data-ttu-id="a254f-132">Ett exempel med JSON-definitioner för Data Factory-entiteter som ska använda toocopy data från en lokal SAP HANA finns [JSON-exempel: kopiera data från SAP HANA tooAzure Blob](#json-example-copy-data-from-sap-hana-to-azure-blob) i den här artikeln.</span><span class="sxs-lookup"><span data-stu-id="a254f-132">For a sample with JSON definitions for Data Factory entities that are used toocopy data from an on-premises SAP HANA, see [JSON example: Copy data from SAP HANA tooAzure Blob](#json-example-copy-data-from-sap-hana-to-azure-blob) section of this article.</span></span> 

<span data-ttu-id="a254f-133">hello följande avsnitt innehåller information om JSON-egenskaper används toodefine Data Factory entiteter specifika tooan SAP HANA-datalager:</span><span class="sxs-lookup"><span data-stu-id="a254f-133">hello following sections provide details about JSON properties that are used toodefine Data Factory entities specific tooan SAP HANA data store:</span></span>

## <a name="linked-service-properties"></a><span data-ttu-id="a254f-134">Länkad tjänstegenskaper</span><span class="sxs-lookup"><span data-stu-id="a254f-134">Linked service properties</span></span>
<span data-ttu-id="a254f-135">hello följande tabell innehåller beskrivning för JSON-element specifika tooSAP HANA länkade tjänsten.</span><span class="sxs-lookup"><span data-stu-id="a254f-135">hello following table provides description for JSON elements specific tooSAP HANA linked service.</span></span>

<span data-ttu-id="a254f-136">Egenskap</span><span class="sxs-lookup"><span data-stu-id="a254f-136">Property</span></span> | <span data-ttu-id="a254f-137">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="a254f-137">Description</span></span> | <span data-ttu-id="a254f-138">Tillåtna värden</span><span class="sxs-lookup"><span data-stu-id="a254f-138">Allowed values</span></span> | <span data-ttu-id="a254f-139">Krävs</span><span class="sxs-lookup"><span data-stu-id="a254f-139">Required</span></span>
-------- | ----------- | -------------- | --------
<span data-ttu-id="a254f-140">server</span><span class="sxs-lookup"><span data-stu-id="a254f-140">server</span></span> | <span data-ttu-id="a254f-141">Namnet på hello-server på vilken hello SAP HANA instansen finns.</span><span class="sxs-lookup"><span data-stu-id="a254f-141">Name of hello server on which hello SAP HANA instance resides.</span></span> <span data-ttu-id="a254f-142">Om servern använder en anpassad port, ange `server:port`.</span><span class="sxs-lookup"><span data-stu-id="a254f-142">If your server is using a customized port, specify `server:port`.</span></span> | <span data-ttu-id="a254f-143">Sträng</span><span class="sxs-lookup"><span data-stu-id="a254f-143">string</span></span> | <span data-ttu-id="a254f-144">Ja</span><span class="sxs-lookup"><span data-stu-id="a254f-144">Yes</span></span>
<span data-ttu-id="a254f-145">AuthenticationType</span><span class="sxs-lookup"><span data-stu-id="a254f-145">authenticationType</span></span> | <span data-ttu-id="a254f-146">Typ av autentisering.</span><span class="sxs-lookup"><span data-stu-id="a254f-146">Type of authentication.</span></span> | <span data-ttu-id="a254f-147">Sträng.</span><span class="sxs-lookup"><span data-stu-id="a254f-147">string.</span></span> <span data-ttu-id="a254f-148">”Basic” eller ”Windows”</span><span class="sxs-lookup"><span data-stu-id="a254f-148">"Basic" or "Windows"</span></span> | <span data-ttu-id="a254f-149">Ja</span><span class="sxs-lookup"><span data-stu-id="a254f-149">Yes</span></span> 
<span data-ttu-id="a254f-150">användarnamn</span><span class="sxs-lookup"><span data-stu-id="a254f-150">username</span></span> | <span data-ttu-id="a254f-151">Namnet på hello-användare som har åtkomst toohello SAP-server</span><span class="sxs-lookup"><span data-stu-id="a254f-151">Name of hello user who has access toohello SAP server</span></span> | <span data-ttu-id="a254f-152">Sträng</span><span class="sxs-lookup"><span data-stu-id="a254f-152">string</span></span> | <span data-ttu-id="a254f-153">Ja</span><span class="sxs-lookup"><span data-stu-id="a254f-153">Yes</span></span>
<span data-ttu-id="a254f-154">lösenord</span><span class="sxs-lookup"><span data-stu-id="a254f-154">password</span></span> | <span data-ttu-id="a254f-155">Lösenordet för hello.</span><span class="sxs-lookup"><span data-stu-id="a254f-155">Password for hello user.</span></span> | <span data-ttu-id="a254f-156">Sträng</span><span class="sxs-lookup"><span data-stu-id="a254f-156">string</span></span> | <span data-ttu-id="a254f-157">Ja</span><span class="sxs-lookup"><span data-stu-id="a254f-157">Yes</span></span>
<span data-ttu-id="a254f-158">gatewayName</span><span class="sxs-lookup"><span data-stu-id="a254f-158">gatewayName</span></span> | <span data-ttu-id="a254f-159">Namnet på hello-gateway som hello Data Factory-tjänsten ska använda tooconnect toohello lokal SAP HANA-instans.</span><span class="sxs-lookup"><span data-stu-id="a254f-159">Name of hello gateway that hello Data Factory service should use tooconnect toohello on-premises SAP HANA instance.</span></span> | <span data-ttu-id="a254f-160">Sträng</span><span class="sxs-lookup"><span data-stu-id="a254f-160">string</span></span> | <span data-ttu-id="a254f-161">Ja</span><span class="sxs-lookup"><span data-stu-id="a254f-161">Yes</span></span>
<span data-ttu-id="a254f-162">encryptedCredential</span><span class="sxs-lookup"><span data-stu-id="a254f-162">encryptedCredential</span></span> | <span data-ttu-id="a254f-163">hello krypterade autentiseringsuppgifter strängen.</span><span class="sxs-lookup"><span data-stu-id="a254f-163">hello encrypted credential string.</span></span> | <span data-ttu-id="a254f-164">Sträng</span><span class="sxs-lookup"><span data-stu-id="a254f-164">string</span></span> | <span data-ttu-id="a254f-165">Nej</span><span class="sxs-lookup"><span data-stu-id="a254f-165">No</span></span>

## <a name="dataset-properties"></a><span data-ttu-id="a254f-166">Egenskaper för datamängd</span><span class="sxs-lookup"><span data-stu-id="a254f-166">Dataset properties</span></span>
<span data-ttu-id="a254f-167">En fullständig lista över egenskaper som är tillgängliga för att definiera datauppsättningarna & avsnitt finns hello [skapa datauppsättningar](data-factory-create-datasets.md) artikel.</span><span class="sxs-lookup"><span data-stu-id="a254f-167">For a full list of sections & properties available for defining datasets, see hello [Creating datasets](data-factory-create-datasets.md) article.</span></span> <span data-ttu-id="a254f-168">Avsnitt som struktur, tillgänglighet och princip på en datamängd JSON är liknande för alla typer av dataset (Azure SQL Azure blob, Azure-tabellen, osv.).</span><span class="sxs-lookup"><span data-stu-id="a254f-168">Sections such as structure, availability, and policy of a dataset JSON are similar for all dataset types (Azure SQL, Azure blob, Azure table, etc.).</span></span>

<span data-ttu-id="a254f-169">Hej **typeProperties** avsnitt är olika för varje typ av dataset och ger information om hello platsen för hello data i datalagret hello.</span><span class="sxs-lookup"><span data-stu-id="a254f-169">hello **typeProperties** section is different for each type of dataset and provides information about hello location of hello data in hello data store.</span></span> <span data-ttu-id="a254f-170">Det finns inga typspecifika egenskaper som stöds för hello SAP HANA dataset av typen **RelationalTable**.</span><span class="sxs-lookup"><span data-stu-id="a254f-170">There are no type-specific properties supported for hello SAP HANA dataset of type **RelationalTable**.</span></span> 


## <a name="copy-activity-properties"></a><span data-ttu-id="a254f-171">Kopiera egenskaper för aktivitet</span><span class="sxs-lookup"><span data-stu-id="a254f-171">Copy activity properties</span></span>
<span data-ttu-id="a254f-172">En fullständig lista över avsnitt & egenskaper som är tillgängliga för att definiera aktiviteter finns hello [skapar Pipelines](data-factory-create-pipelines.md) artikel.</span><span class="sxs-lookup"><span data-stu-id="a254f-172">For a full list of sections & properties available for defining activities, see hello [Creating Pipelines](data-factory-create-pipelines.md) article.</span></span> <span data-ttu-id="a254f-173">Egenskaper, till exempel namn, beskrivning, inkommande och utgående tabeller är principer är tillgängliga för alla typer av aktiviteter.</span><span class="sxs-lookup"><span data-stu-id="a254f-173">Properties such as name, description, input and output tables, are policies are available for all types of activities.</span></span>

<span data-ttu-id="a254f-174">Medan egenskaper som är tillgängliga i hello **typeProperties** avsnittet hello aktivitet varierar med varje aktivitetstyp.</span><span class="sxs-lookup"><span data-stu-id="a254f-174">Whereas, properties available in hello **typeProperties** section of hello activity vary with each activity type.</span></span> <span data-ttu-id="a254f-175">För Kopieringsaktivitet kan varierar de beroende på hello typer av datakällor och sänkor.</span><span class="sxs-lookup"><span data-stu-id="a254f-175">For Copy activity, they vary depending on hello types of sources and sinks.</span></span>

<span data-ttu-id="a254f-176">När datakällan i en Kopieringsaktivitet är av typen **RelationalSource** (som innehåller SAP HANA), hello följande egenskaper finns i avsnittet typeProperties:</span><span class="sxs-lookup"><span data-stu-id="a254f-176">When source in copy activity is of type **RelationalSource** (which includes SAP HANA), hello following properties are available in typeProperties section:</span></span>

| <span data-ttu-id="a254f-177">Egenskap</span><span class="sxs-lookup"><span data-stu-id="a254f-177">Property</span></span> | <span data-ttu-id="a254f-178">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="a254f-178">Description</span></span> | <span data-ttu-id="a254f-179">Tillåtna värden</span><span class="sxs-lookup"><span data-stu-id="a254f-179">Allowed values</span></span> | <span data-ttu-id="a254f-180">Krävs</span><span class="sxs-lookup"><span data-stu-id="a254f-180">Required</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="a254f-181">DocumentDB</span><span class="sxs-lookup"><span data-stu-id="a254f-181">query</span></span> | <span data-ttu-id="a254f-182">Anger hello SQL-frågan tooread data från hello SAP HANA-instans.</span><span class="sxs-lookup"><span data-stu-id="a254f-182">Specifies hello SQL query tooread data from hello SAP HANA instance.</span></span> | <span data-ttu-id="a254f-183">SQL-frågan.</span><span class="sxs-lookup"><span data-stu-id="a254f-183">SQL query.</span></span> | <span data-ttu-id="a254f-184">Ja</span><span class="sxs-lookup"><span data-stu-id="a254f-184">Yes</span></span> |

## <a name="json-example-copy-data-from-sap-hana-tooazure-blob"></a><span data-ttu-id="a254f-185">JSON-exempel: kopiera data från SAP HANA tooAzure Blob</span><span class="sxs-lookup"><span data-stu-id="a254f-185">JSON example: Copy data from SAP HANA tooAzure Blob</span></span>
<span data-ttu-id="a254f-186">hello följande exempel innehåller exempel JSON definitioner som du kan använda toocreate en pipeline med hjälp av [Azure-portalen](data-factory-copy-activity-tutorial-using-azure-portal.md) eller [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md) eller [Azure PowerShell](data-factory-copy-activity-tutorial-using-powershell.md).</span><span class="sxs-lookup"><span data-stu-id="a254f-186">hello following sample provides sample JSON definitions that you can use toocreate a pipeline by using [Azure portal](data-factory-copy-activity-tutorial-using-azure-portal.md) or [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md) or [Azure PowerShell](data-factory-copy-activity-tutorial-using-powershell.md).</span></span> <span data-ttu-id="a254f-187">Det här exemplet visas hur toocopy data från en lokal SAP HANA tooan Azure Blob Storage.</span><span class="sxs-lookup"><span data-stu-id="a254f-187">This sample shows how toocopy data from an on-premises SAP HANA tooan Azure Blob Storage.</span></span> <span data-ttu-id="a254f-188">Dock datan kan kopieras **direkt** tooany av hello sänkor visas [här](data-factory-data-movement-activities.md#supported-data-stores-and-formats) med hello Kopieringsaktiviteten i Azure Data Factory.</span><span class="sxs-lookup"><span data-stu-id="a254f-188">However, data can be copied **directly** tooany of hello sinks listed [here](data-factory-data-movement-activities.md#supported-data-stores-and-formats) using hello Copy Activity in Azure Data Factory.</span></span>  

> [!IMPORTANT]
> <span data-ttu-id="a254f-189">Det här exemplet innehåller JSON kodavsnitt.</span><span class="sxs-lookup"><span data-stu-id="a254f-189">This sample provides JSON snippets.</span></span> <span data-ttu-id="a254f-190">Stegvisa instruktioner för att skapa datafabriken hello inkluderas inte.</span><span class="sxs-lookup"><span data-stu-id="a254f-190">It does not include step-by-step instructions for creating hello data factory.</span></span> <span data-ttu-id="a254f-191">Se [flytta data mellan lokala platser och moln](data-factory-move-data-between-onprem-and-cloud.md) artikel stegvisa instruktioner.</span><span class="sxs-lookup"><span data-stu-id="a254f-191">See [moving data between on-premises locations and cloud](data-factory-move-data-between-onprem-and-cloud.md) article for step-by-step instructions.</span></span>

<span data-ttu-id="a254f-192">hello exemplet har hello följande data factory-enheter:</span><span class="sxs-lookup"><span data-stu-id="a254f-192">hello sample has hello following data factory entities:</span></span>

1. <span data-ttu-id="a254f-193">En länkad tjänst av typen [SapHana](#linked-service-properties).</span><span class="sxs-lookup"><span data-stu-id="a254f-193">A linked service of type [SapHana](#linked-service-properties).</span></span>
2. <span data-ttu-id="a254f-194">En länkad tjänst av typen [AzureStorage](data-factory-azure-blob-connector.md#linked-service-properties).</span><span class="sxs-lookup"><span data-stu-id="a254f-194">A linked service of type [AzureStorage](data-factory-azure-blob-connector.md#linked-service-properties).</span></span>
3. <span data-ttu-id="a254f-195">Indata [dataset](data-factory-create-datasets.md) av typen [RelationalTable](#dataset-properties).</span><span class="sxs-lookup"><span data-stu-id="a254f-195">An input [dataset](data-factory-create-datasets.md) of type [RelationalTable](#dataset-properties).</span></span>
4. <span data-ttu-id="a254f-196">Utdata [dataset](data-factory-create-datasets.md) av typen [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties).</span><span class="sxs-lookup"><span data-stu-id="a254f-196">An output [dataset](data-factory-create-datasets.md) of type [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties).</span></span>
5. <span data-ttu-id="a254f-197">En [pipeline](data-factory-create-pipelines.md) med Kopieringsaktiviteten som använder [RelationalSource](#copy-activity-properties) och [BlobSink](data-factory-azure-blob-connector.md#copy-activity-properties).</span><span class="sxs-lookup"><span data-stu-id="a254f-197">A [pipeline](data-factory-create-pipelines.md) with Copy Activity that uses [RelationalSource](#copy-activity-properties) and [BlobSink](data-factory-azure-blob-connector.md#copy-activity-properties).</span></span>

<span data-ttu-id="a254f-198">hello exemplet kopierar data från en SAP HANA instans tooan Azure blob varje timme.</span><span class="sxs-lookup"><span data-stu-id="a254f-198">hello sample copies data from an SAP HANA instance tooan Azure blob hourly.</span></span> <span data-ttu-id="a254f-199">hello JSON egenskaper som används i exemplen beskrivs i hello-exempel i följande avsnitt.</span><span class="sxs-lookup"><span data-stu-id="a254f-199">hello JSON properties used in these samples are described in sections following hello samples.</span></span>

<span data-ttu-id="a254f-200">Som ett första steg bör du konfigurera hello data management gateway.</span><span class="sxs-lookup"><span data-stu-id="a254f-200">As a first step, setup hello data management gateway.</span></span> <span data-ttu-id="a254f-201">hello anvisningar finns i hello [flytta data mellan lokala platser och moln](data-factory-move-data-between-onprem-and-cloud.md) artikel.</span><span class="sxs-lookup"><span data-stu-id="a254f-201">hello instructions are in hello [moving data between on-premises locations and cloud](data-factory-move-data-between-onprem-and-cloud.md) article.</span></span>

### <a name="sap-hana-linked-service"></a><span data-ttu-id="a254f-202">SAP HANA länkad tjänst</span><span class="sxs-lookup"><span data-stu-id="a254f-202">SAP HANA linked service</span></span>
<span data-ttu-id="a254f-203">Den här länkade tjänsten länkar din SAP HANA instans toohello data factory.</span><span class="sxs-lookup"><span data-stu-id="a254f-203">This linked service links your SAP HANA instance toohello data factory.</span></span> <span data-ttu-id="a254f-204">hello typegenskapen har ställts in för**SapHana**.</span><span class="sxs-lookup"><span data-stu-id="a254f-204">hello type property is set too**SapHana**.</span></span> <span data-ttu-id="a254f-205">Hej typeProperties avsnittet innehåller anslutningsinformation för hello SAP HANA-instans.</span><span class="sxs-lookup"><span data-stu-id="a254f-205">hello typeProperties section provides connection information for hello SAP HANA instance.</span></span>

```json
{
    "name": "SapHanaLinkedService",
    "properties":
    {
        "type": "SapHana",
        "typeProperties":
        {
            "server": "<server name>",
            "authenticationType": "<Basic, or Windows>",
            "username": "<SAP user>",
            "password": "<Password for SAP user>",
            "gatewayName": "<gateway name>"
        }
    }
}

```

### <a name="azure-storage-linked-service"></a><span data-ttu-id="a254f-206">Länkad Azure-lagringstjänst</span><span class="sxs-lookup"><span data-stu-id="a254f-206">Azure Storage linked service</span></span>
<span data-ttu-id="a254f-207">Det här länkade tjänsten länkar din Azure Storage-konto toohello data factory.</span><span class="sxs-lookup"><span data-stu-id="a254f-207">This linked service links your Azure Storage account toohello data factory.</span></span> <span data-ttu-id="a254f-208">hello typegenskapen har ställts in för**AzureStorage**.</span><span class="sxs-lookup"><span data-stu-id="a254f-208">hello type property is set too**AzureStorage**.</span></span> <span data-ttu-id="a254f-209">Hej typeProperties avsnittet innehåller anslutningsinformation för hello Azure Storage-konto.</span><span class="sxs-lookup"><span data-stu-id="a254f-209">hello typeProperties section provides connection information for hello Azure Storage account.</span></span>

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

### <a name="sap-hana-input-dataset"></a><span data-ttu-id="a254f-210">SAP HANA inkommande dataset</span><span class="sxs-lookup"><span data-stu-id="a254f-210">SAP HANA input dataset</span></span>

<span data-ttu-id="a254f-211">Den här datauppsättningen definierar hello SAP HANA dataset.</span><span class="sxs-lookup"><span data-stu-id="a254f-211">This dataset defines hello SAP HANA dataset.</span></span> <span data-ttu-id="a254f-212">Du ställer in hello på hello Data Factory datamängd för**RelationalTable**.</span><span class="sxs-lookup"><span data-stu-id="a254f-212">You set hello type of hello Data Factory dataset too**RelationalTable**.</span></span> <span data-ttu-id="a254f-213">För närvarande kan anger du inte några typspecifika egenskaper för en SAP HANA-datamängd.</span><span class="sxs-lookup"><span data-stu-id="a254f-213">Currently, you do not specify any type-specific properties for an SAP HANA dataset.</span></span> <span data-ttu-id="a254f-214">hello frågan i hello kopiera aktivitetsdefinitionen anger vilka data tooread från hello SAP HANA-instans.</span><span class="sxs-lookup"><span data-stu-id="a254f-214">hello query in hello Copy Activity definition specifies what data tooread from hello SAP HANA instance.</span></span> 

<span data-ttu-id="a254f-215">Ange externa egenskapen tootrue informerar hello Data Factory-tjänsten hello tabellen är externa toohello data factory och inte tillverkas av en aktivitet i hello data factory.</span><span class="sxs-lookup"><span data-stu-id="a254f-215">Setting external property tootrue informs hello Data Factory service that hello table is external toohello data factory and is not produced by an activity in hello data factory.</span></span>

<span data-ttu-id="a254f-216">Frekvensen och intervall egenskaper definierar hello schema.</span><span class="sxs-lookup"><span data-stu-id="a254f-216">Frequency and interval properties defines hello schedule.</span></span> <span data-ttu-id="a254f-217">I det här fallet läses hello data från hello SAP HANA instans varje timme.</span><span class="sxs-lookup"><span data-stu-id="a254f-217">In this case, hello data is read from hello SAP HANA instance hourly.</span></span> 

```json
{
    "name": "SapHanaDataset",
    "properties": {
        "type": "RelationalTable",
        "linkedServiceName": "SapHanaLinkedService",
        "typeProperties": {},
        "availability": {
            "frequency": "Hour",
            "interval": 1
        },
        "external": true
    }
}
```

### <a name="azure-blob-output-dataset"></a><span data-ttu-id="a254f-218">Utdatauppsättning för Azure-blobb</span><span class="sxs-lookup"><span data-stu-id="a254f-218">Azure Blob output dataset</span></span>
<span data-ttu-id="a254f-219">Den här datauppsättningen definierar hello utdata Azure-blobbdatauppsättning.</span><span class="sxs-lookup"><span data-stu-id="a254f-219">This dataset defines hello output Azure Blob dataset.</span></span> <span data-ttu-id="a254f-220">hello typegenskapen anges tooAzureBlob.</span><span class="sxs-lookup"><span data-stu-id="a254f-220">hello type property is set tooAzureBlob.</span></span> <span data-ttu-id="a254f-221">Hej typeProperties avsnittet innehåller hello data kopieras från hello SAP HANA-instans ska lagras.</span><span class="sxs-lookup"><span data-stu-id="a254f-221">hello typeProperties section provides where hello data copied from hello SAP HANA instance is stored.</span></span> <span data-ttu-id="a254f-222">hello data skrivs tooa nya blob varje timme (frekvens: timme, intervall: 1).</span><span class="sxs-lookup"><span data-stu-id="a254f-222">hello data is written tooa new blob every hour (frequency: hour, interval: 1).</span></span> <span data-ttu-id="a254f-223">hello mappsökväg för hello blob utvärderas dynamiskt baserat på hello starttiden för hello-segment som bearbetas.</span><span class="sxs-lookup"><span data-stu-id="a254f-223">hello folder path for hello blob is dynamically evaluated based on hello start time of hello slice that is being processed.</span></span> <span data-ttu-id="a254f-224">hello mappsökväg använder år, månad, dag och timmar delar av hello starttid.</span><span class="sxs-lookup"><span data-stu-id="a254f-224">hello folder path uses year, month, day, and hours parts of hello start time.</span></span>

```json
{
    "name": "AzureBlobDataSet",
    "properties": {
        "type": "AzureBlob",
        "linkedServiceName": "AzureStorageLinkedService",
        "typeProperties": {
            "folderPath": "mycontainer/saphana/yearno={Year}/monthno={Month}/dayno={Day}/hourno={Hour}",
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


### <a name="pipeline-with-copy-activity"></a><span data-ttu-id="a254f-225">Pipeline med kopieringsaktiviteten</span><span class="sxs-lookup"><span data-stu-id="a254f-225">Pipeline with Copy activity</span></span>

<span data-ttu-id="a254f-226">hello pipelinen innehåller en kopia-aktivitet som är konfigurerade toouse hello inkommande och utgående datauppsättningar och är schemalagda toorun varje timme.</span><span class="sxs-lookup"><span data-stu-id="a254f-226">hello pipeline contains a Copy Activity that is configured toouse hello input and output datasets and is scheduled toorun every hour.</span></span> <span data-ttu-id="a254f-227">I hello pipeline JSON-definitionen hello **källa** typ har angetts för**RelationalSource** (för SAP HANA-källa) och **sink** typ har angetts för**BlobSink**.</span><span class="sxs-lookup"><span data-stu-id="a254f-227">In hello pipeline JSON definition, hello **source** type is set too**RelationalSource** (for SAP HANA source) and **sink** type is set too**BlobSink**.</span></span> <span data-ttu-id="a254f-228">hello SQL-frågan som angetts för hello **frågan** egenskapen väljer hello data i hello tidigare timme toocopy.</span><span class="sxs-lookup"><span data-stu-id="a254f-228">hello SQL query specified for hello **query** property selects hello data in hello past hour toocopy.</span></span>

```json
{
    "name": "CopySapHanaToBlob",
    "properties": {
        "description": "pipeline for copy activity",
        "activities": [
            {
                "type": "Copy",
                "typeProperties": {
                    "source": {
                        "type": "RelationalSource",
                        "query": "<SQL Query for HANA>"
                    },
                    "sink": {
                        "type": "BlobSink",
                        "writeBatchSize": 0,
                        "writeBatchTimeout": "00:00:00"
                    }
                },
                "inputs": [
                    {
                        "name": "SapHanaDataset"
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
                "name": "SapHanaToBlob"
            }
        ],
        "start": "2017-03-01T18:00:00Z",
        "end": "2017-03-01T19:00:00Z"
    }
}
```


### <a name="type-mapping-for-sap-hana"></a><span data-ttu-id="a254f-229">Mappning för SAP HANA</span><span class="sxs-lookup"><span data-stu-id="a254f-229">Type mapping for SAP HANA</span></span>
<span data-ttu-id="a254f-230">Som anges i hello [data movement aktiviteter](data-factory-data-movement-activities.md) artikeln kopieringsaktiviteten utför automatisk konverteringar från källan typer toosink typer med hello följande två sätt:</span><span class="sxs-lookup"><span data-stu-id="a254f-230">As mentioned in hello [data movement activities](data-factory-data-movement-activities.md) article, Copy activity performs automatic type conversions from source types toosink types with hello following two-step approach:</span></span>

1. <span data-ttu-id="a254f-231">Konvertera från inbyggda typer too.NET källtypen</span><span class="sxs-lookup"><span data-stu-id="a254f-231">Convert from native source types too.NET type</span></span>
2. <span data-ttu-id="a254f-232">Konvertera från .NET typen toonative Mottagartypen</span><span class="sxs-lookup"><span data-stu-id="a254f-232">Convert from .NET type toonative sink type</span></span>

<span data-ttu-id="a254f-233">När du flyttar data från SAP HANA används hello följande mappningar från SAP HANA typer too.NET typer.</span><span class="sxs-lookup"><span data-stu-id="a254f-233">When moving data from SAP HANA, hello following mappings are used from SAP HANA types too.NET types.</span></span>

<span data-ttu-id="a254f-234">SAP HANA-typ</span><span class="sxs-lookup"><span data-stu-id="a254f-234">SAP HANA Type</span></span> | <span data-ttu-id="a254f-235">.NET baserat typ</span><span class="sxs-lookup"><span data-stu-id="a254f-235">.NET Based Type</span></span>
------------- | ---------------
<span data-ttu-id="a254f-236">TINYINT</span><span class="sxs-lookup"><span data-stu-id="a254f-236">TINYINT</span></span> | <span data-ttu-id="a254f-237">Mottagna byte</span><span class="sxs-lookup"><span data-stu-id="a254f-237">Byte</span></span>
<span data-ttu-id="a254f-238">SMALLINT</span><span class="sxs-lookup"><span data-stu-id="a254f-238">SMALLINT</span></span> | <span data-ttu-id="a254f-239">Int16</span><span class="sxs-lookup"><span data-stu-id="a254f-239">Int16</span></span>
<span data-ttu-id="a254f-240">INT</span><span class="sxs-lookup"><span data-stu-id="a254f-240">INT</span></span> | <span data-ttu-id="a254f-241">Int32</span><span class="sxs-lookup"><span data-stu-id="a254f-241">Int32</span></span>
<span data-ttu-id="a254f-242">BIGINT</span><span class="sxs-lookup"><span data-stu-id="a254f-242">BIGINT</span></span> | <span data-ttu-id="a254f-243">Int64</span><span class="sxs-lookup"><span data-stu-id="a254f-243">Int64</span></span>
<span data-ttu-id="a254f-244">VERKLIG</span><span class="sxs-lookup"><span data-stu-id="a254f-244">REAL</span></span> | <span data-ttu-id="a254f-245">Enskild</span><span class="sxs-lookup"><span data-stu-id="a254f-245">Single</span></span>
<span data-ttu-id="a254f-246">DUBBEL</span><span class="sxs-lookup"><span data-stu-id="a254f-246">DOUBLE</span></span> | <span data-ttu-id="a254f-247">Enskild</span><span class="sxs-lookup"><span data-stu-id="a254f-247">Single</span></span>
<span data-ttu-id="a254f-248">DECIMAL</span><span class="sxs-lookup"><span data-stu-id="a254f-248">DECIMAL</span></span> | <span data-ttu-id="a254f-249">Decimal</span><span class="sxs-lookup"><span data-stu-id="a254f-249">Decimal</span></span>
<span data-ttu-id="a254f-250">BOOLESKT VÄRDE</span><span class="sxs-lookup"><span data-stu-id="a254f-250">BOOLEAN</span></span> | <span data-ttu-id="a254f-251">Mottagna byte</span><span class="sxs-lookup"><span data-stu-id="a254f-251">Byte</span></span>
<span data-ttu-id="a254f-252">VARCHAR</span><span class="sxs-lookup"><span data-stu-id="a254f-252">VARCHAR</span></span> | <span data-ttu-id="a254f-253">Sträng</span><span class="sxs-lookup"><span data-stu-id="a254f-253">String</span></span>
<span data-ttu-id="a254f-254">NVARCHAR</span><span class="sxs-lookup"><span data-stu-id="a254f-254">NVARCHAR</span></span> | <span data-ttu-id="a254f-255">Sträng</span><span class="sxs-lookup"><span data-stu-id="a254f-255">String</span></span>
<span data-ttu-id="a254f-256">CLOB</span><span class="sxs-lookup"><span data-stu-id="a254f-256">CLOB</span></span> | <span data-ttu-id="a254f-257">byte]</span><span class="sxs-lookup"><span data-stu-id="a254f-257">Byte[]</span></span>
<span data-ttu-id="a254f-258">ALPHANUM</span><span class="sxs-lookup"><span data-stu-id="a254f-258">ALPHANUM</span></span> | <span data-ttu-id="a254f-259">Sträng</span><span class="sxs-lookup"><span data-stu-id="a254f-259">String</span></span>
<span data-ttu-id="a254f-260">BLOB</span><span class="sxs-lookup"><span data-stu-id="a254f-260">BLOB</span></span> | <span data-ttu-id="a254f-261">byte]</span><span class="sxs-lookup"><span data-stu-id="a254f-261">Byte[]</span></span>
<span data-ttu-id="a254f-262">DATUM</span><span class="sxs-lookup"><span data-stu-id="a254f-262">DATE</span></span> | <span data-ttu-id="a254f-263">Datum och tid</span><span class="sxs-lookup"><span data-stu-id="a254f-263">DateTime</span></span>
<span data-ttu-id="a254f-264">TID</span><span class="sxs-lookup"><span data-stu-id="a254f-264">TIME</span></span> | <span data-ttu-id="a254f-265">TimeSpan</span><span class="sxs-lookup"><span data-stu-id="a254f-265">TimeSpan</span></span>
<span data-ttu-id="a254f-266">TIDSSTÄMPEL</span><span class="sxs-lookup"><span data-stu-id="a254f-266">TIMESTAMP</span></span> | <span data-ttu-id="a254f-267">Datum och tid</span><span class="sxs-lookup"><span data-stu-id="a254f-267">DateTime</span></span>
<span data-ttu-id="a254f-268">SECONDDATE</span><span class="sxs-lookup"><span data-stu-id="a254f-268">SECONDDATE</span></span> | <span data-ttu-id="a254f-269">Datum och tid</span><span class="sxs-lookup"><span data-stu-id="a254f-269">DateTime</span></span>

## <a name="known-limitations"></a><span data-ttu-id="a254f-270">Kända begränsningar</span><span class="sxs-lookup"><span data-stu-id="a254f-270">Known limitations</span></span>
<span data-ttu-id="a254f-271">Det finns några kända begränsningar när du kopierar data från SAP HANA:</span><span class="sxs-lookup"><span data-stu-id="a254f-271">There are a few known limitations when copying data from SAP HANA:</span></span>

- <span data-ttu-id="a254f-272">NVARCHAR strängar är trunkerat toomaximum längd på 4 000 Unicode-tecken</span><span class="sxs-lookup"><span data-stu-id="a254f-272">NVARCHAR strings are truncated toomaximum length of 4000 Unicode characters</span></span>
- <span data-ttu-id="a254f-273">SMALLDECIMAL stöds inte</span><span class="sxs-lookup"><span data-stu-id="a254f-273">SMALLDECIMAL is not supported</span></span>
- <span data-ttu-id="a254f-274">VARBINARY stöds inte</span><span class="sxs-lookup"><span data-stu-id="a254f-274">VARBINARY is not supported</span></span>
- <span data-ttu-id="a254f-275">Giltiga datum är mellan 1899, 12, 30 och 9999-12-31</span><span class="sxs-lookup"><span data-stu-id="a254f-275">Valid Dates are between 1899/12/30 and 9999/12/31</span></span>

## <a name="map-source-toosink-columns"></a><span data-ttu-id="a254f-276">Mappa källkolumner toosink</span><span class="sxs-lookup"><span data-stu-id="a254f-276">Map source toosink columns</span></span>
<span data-ttu-id="a254f-277">toolearn mappning tabellkolumner i källan dataset toocolumns i sink dataset finns [mappa dataset kolumner i Azure Data Factory](data-factory-map-columns.md).</span><span class="sxs-lookup"><span data-stu-id="a254f-277">toolearn about mapping columns in source dataset toocolumns in sink dataset, see [Mapping dataset columns in Azure Data Factory](data-factory-map-columns.md).</span></span>

## <a name="repeatable-read-from-relational-sources"></a><span data-ttu-id="a254f-278">Upprepbar läsning från relationella källor</span><span class="sxs-lookup"><span data-stu-id="a254f-278">Repeatable read from relational sources</span></span>
<span data-ttu-id="a254f-279">När du kopierar data från relationella datalager, Kom ihåg tooavoid repeterbarhet oönskade resultat.</span><span class="sxs-lookup"><span data-stu-id="a254f-279">When copying data from relational data stores, keep repeatability in mind tooavoid unintended outcomes.</span></span> <span data-ttu-id="a254f-280">I Azure Data Factory, kan du köra en sektor manuellt.</span><span class="sxs-lookup"><span data-stu-id="a254f-280">In Azure Data Factory, you can rerun a slice manually.</span></span> <span data-ttu-id="a254f-281">Du kan också konfigurera i principen för en dataset så att ett segment som körs när ett fel uppstår.</span><span class="sxs-lookup"><span data-stu-id="a254f-281">You can also configure retry policy for a dataset so that a slice is rerun when a failure occurs.</span></span> <span data-ttu-id="a254f-282">När ett segment körs på antingen sätt måste toomake att som hello samma data läses oavsett hur många gånger ett segment körs.</span><span class="sxs-lookup"><span data-stu-id="a254f-282">When a slice is rerun in either way, you need toomake sure that hello same data is read no matter how many times a slice is run.</span></span> <span data-ttu-id="a254f-283">Se [Repeatable läsa från relationella källor](data-factory-repeatable-copy.md#repeatable-read-from-relational-sources)</span><span class="sxs-lookup"><span data-stu-id="a254f-283">See [Repeatable read from relational sources](data-factory-repeatable-copy.md#repeatable-read-from-relational-sources)</span></span>

## <a name="performance-and-tuning"></a><span data-ttu-id="a254f-284">Prestanda och finjustering</span><span class="sxs-lookup"><span data-stu-id="a254f-284">Performance and Tuning</span></span>
<span data-ttu-id="a254f-285">Se [kopiera aktivitet prestanda och justera guiden](data-factory-copy-activity-performance.md) toolearn om nyckeln faktorer som påverkan prestanda för flytt av data (Kopieringsaktiviteten) i Azure Data Factory och olika sätt toooptimize den.</span><span class="sxs-lookup"><span data-stu-id="a254f-285">See [Copy Activity Performance & Tuning Guide](data-factory-copy-activity-performance.md) toolearn about key factors that impact performance of data movement (Copy Activity) in Azure Data Factory and various ways toooptimize it.</span></span>
