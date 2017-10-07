---
title: "aaaMove data från Teradata med hjälp av Azure Data Factory | Microsoft Docs"
description: "Läs mer om Teradata-koppling för hello Data Factory-tjänsten där du kan flytta data från Teradata-databasen"
services: data-factory
documentationcenter: 
author: linda33wj
manager: jhubbard
editor: monicar
ms.assetid: 98eb76d8-5f3d-4667-b76e-e59ed3eea3ae
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/11/2017
ms.author: jingwang
ms.openlocfilehash: 79153476157666463b499edaa7585adaf8ad3bee
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="move-data-from-teradata-using-azure-data-factory"></a><span data-ttu-id="0a727-103">Flytta data från Teradata med hjälp av Azure Data Factory</span><span class="sxs-lookup"><span data-stu-id="0a727-103">Move data from Teradata using Azure Data Factory</span></span>
<span data-ttu-id="0a727-104">Den här artikeln förklarar hur toouse hello Kopieringsaktiviteten i Azure Data Factory toomove data från en lokal Teradata-databasen.</span><span class="sxs-lookup"><span data-stu-id="0a727-104">This article explains how toouse hello Copy Activity in Azure Data Factory toomove data from an on-premises Teradata database.</span></span> <span data-ttu-id="0a727-105">Den bygger på hello [Data Movement aktiviteter](data-factory-data-movement-activities.md) artikel som ger en allmän översikt över dataflyttning hello kopieringsaktiviteten.</span><span class="sxs-lookup"><span data-stu-id="0a727-105">It builds on hello [Data Movement Activities](data-factory-data-movement-activities.md) article, which presents a general overview of data movement with hello copy activity.</span></span>

<span data-ttu-id="0a727-106">Du kan kopiera data från ett lokalt Teradata data store tooany stöds sink dataarkiv.</span><span class="sxs-lookup"><span data-stu-id="0a727-106">You can copy data from an on-premises Teradata data store tooany supported sink data store.</span></span> <span data-ttu-id="0a727-107">En lista över data lagras som stöds när egenskaperna av hello kopieringsaktiviteten Se hello [stöds datalager](data-factory-data-movement-activities.md#supported-data-stores-and-formats) tabell.</span><span class="sxs-lookup"><span data-stu-id="0a727-107">For a list of data stores supported as sinks by hello copy activity, see hello [Supported data stores](data-factory-data-movement-activities.md#supported-data-stores-and-formats) table.</span></span> <span data-ttu-id="0a727-108">Data factory stöder för närvarande endast flytta data från en Teradata lagra tooother datalager, men inte för att flytta data från andra lagrar tooa Teradata data dataarkiv.</span><span class="sxs-lookup"><span data-stu-id="0a727-108">Data factory currently supports only moving data from a Teradata data store tooother data stores, but not for moving data from other data stores tooa Teradata data store.</span></span> 

## <a name="prerequisites"></a><span data-ttu-id="0a727-109">Krav</span><span class="sxs-lookup"><span data-stu-id="0a727-109">Prerequisites</span></span>
<span data-ttu-id="0a727-110">Data factory stöder anslutande tooon lokala Teradata källor via hello Data Management Gateway.</span><span class="sxs-lookup"><span data-stu-id="0a727-110">Data factory supports connecting tooon-premises Teradata sources via hello Data Management Gateway.</span></span> <span data-ttu-id="0a727-111">Se [flytta data mellan lokala platser och moln](data-factory-move-data-between-onprem-and-cloud.md) artikel toolearn om Data Management Gateway och stegvisa instruktioner om hur du konfigurerar hello gateway.</span><span class="sxs-lookup"><span data-stu-id="0a727-111">See [moving data between on-premises locations and cloud](data-factory-move-data-between-onprem-and-cloud.md) article toolearn about Data Management Gateway and step-by-step instructions on setting up hello gateway.</span></span>

<span data-ttu-id="0a727-112">Gateway krävs även om hello Teradata finns i en Azure IaaS-VM.</span><span class="sxs-lookup"><span data-stu-id="0a727-112">Gateway is required even if hello Teradata is hosted in an Azure IaaS VM.</span></span> <span data-ttu-id="0a727-113">Du kan installera hello gateway på hello samma IaaS VM som hello data lagras eller på en annan virtuell dator så länge som hello gateway kan ansluta toohello databas.</span><span class="sxs-lookup"><span data-stu-id="0a727-113">You can install hello gateway on hello same IaaS VM as hello data store or on a different VM as long as hello gateway can connect toohello database.</span></span>

> [!NOTE]
> <span data-ttu-id="0a727-114">Se [felsökning av problem med gateway](data-factory-data-management-gateway.md#troubleshooting-gateway-issues) tips om hur du felsöker anslutning /-gateway relaterade problem.</span><span class="sxs-lookup"><span data-stu-id="0a727-114">See [Troubleshoot gateway issues](data-factory-data-management-gateway.md#troubleshooting-gateway-issues) for tips on troubleshooting connection/gateway related issues.</span></span>

## <a name="supported-versions-and-installation"></a><span data-ttu-id="0a727-115">Versioner som stöds och installation</span><span class="sxs-lookup"><span data-stu-id="0a727-115">Supported versions and installation</span></span>
<span data-ttu-id="0a727-116">För Data Management Gateway tooconnect toohello Teradata-databasen måste tooinstall hello [.NET Data Provider för Teradata](http://go.microsoft.com/fwlink/?LinkId=278886) version 14 eller ovan på hello samma system som hello Data Management Gateway.</span><span class="sxs-lookup"><span data-stu-id="0a727-116">For Data Management Gateway tooconnect toohello Teradata Database, you need tooinstall hello [.NET Data Provider for Teradata](http://go.microsoft.com/fwlink/?LinkId=278886) version 14 or above on hello same system as hello Data Management Gateway.</span></span> <span data-ttu-id="0a727-117">Teradata version 12 och senare stöds.</span><span class="sxs-lookup"><span data-stu-id="0a727-117">Teradata version 12 and above is supported.</span></span>

## <a name="getting-started"></a><span data-ttu-id="0a727-118">Komma igång</span><span class="sxs-lookup"><span data-stu-id="0a727-118">Getting started</span></span>
<span data-ttu-id="0a727-119">Du kan skapa en pipeline med en kopia-aktivitet som flyttar data från en lokal Cassandra data store med hjälp av olika verktyg/API: er.</span><span class="sxs-lookup"><span data-stu-id="0a727-119">You can create a pipeline with a copy activity that moves data from an on-premises Cassandra data store by using different tools/APIs.</span></span> 

- <span data-ttu-id="0a727-120">hello enklaste sättet toocreate en pipeline är toouse hello **guiden Kopiera**.</span><span class="sxs-lookup"><span data-stu-id="0a727-120">hello easiest way toocreate a pipeline is toouse hello **Copy Wizard**.</span></span> <span data-ttu-id="0a727-121">Se [Självstudier: skapa en pipeline med hjälp av guiden Kopiera](data-factory-copy-data-wizard-tutorial.md) för en snabb genomgång om hur du skapar en pipeline med hjälp av guiden för hello kopiera data.</span><span class="sxs-lookup"><span data-stu-id="0a727-121">See [Tutorial: Create a pipeline using Copy Wizard](data-factory-copy-data-wizard-tutorial.md) for a quick walkthrough on creating a pipeline using hello Copy data wizard.</span></span> 
- <span data-ttu-id="0a727-122">Du kan också använda följande verktyg toocreate en pipeline hello: **Azure-portalen**, **Visual Studio**, **Azure PowerShell**, **Azure Resource Manager-mall** , **.NET API**, och **REST API**.</span><span class="sxs-lookup"><span data-stu-id="0a727-122">You can also use hello following tools toocreate a pipeline: **Azure portal**, **Visual Studio**, **Azure PowerShell**, **Azure Resource Manager template**, **.NET API**, and **REST API**.</span></span> <span data-ttu-id="0a727-123">Se [kopiera aktivitet kursen](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) för stegvisa instruktioner toocreate en pipeline med en Kopieringsaktivitet.</span><span class="sxs-lookup"><span data-stu-id="0a727-123">See [Copy activity tutorial](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) for step-by-step instructions toocreate a pipeline with a copy activity.</span></span> 

<span data-ttu-id="0a727-124">Om du använder hello verktyg eller API: er kan utföra du hello följande steg toocreate en pipeline som flyttar data från en källdata lagra tooa sink-datalagret:</span><span class="sxs-lookup"><span data-stu-id="0a727-124">Whether you use hello tools or APIs, you perform hello following steps toocreate a pipeline that moves data from a source data store tooa sink data store:</span></span>

1. <span data-ttu-id="0a727-125">Skapa **länkade tjänster** toolink indata och utdata lagrar tooyour data factory.</span><span class="sxs-lookup"><span data-stu-id="0a727-125">Create **linked services** toolink input and output data stores tooyour data factory.</span></span>
2. <span data-ttu-id="0a727-126">Skapa **datauppsättningar** toorepresent indata och utdata för hello kopieringsåtgärden.</span><span class="sxs-lookup"><span data-stu-id="0a727-126">Create **datasets** toorepresent input and output data for hello copy operation.</span></span> 
3. <span data-ttu-id="0a727-127">Skapa en **pipeline** med en kopia-aktivitet som tar en datamängd som indata och en dataset som utdata.</span><span class="sxs-lookup"><span data-stu-id="0a727-127">Create a **pipeline** with a copy activity that takes a dataset as an input and a dataset as an output.</span></span> 

<span data-ttu-id="0a727-128">När du använder guiden hello skapas automatiskt JSON definitioner för dessa Data Factory-enheter (länkade tjänster, datauppsättningar och hello pipeline) för dig.</span><span class="sxs-lookup"><span data-stu-id="0a727-128">When you use hello wizard, JSON definitions for these Data Factory entities (linked services, datasets, and hello pipeline) are automatically created for you.</span></span> <span data-ttu-id="0a727-129">När du använder Verktyg/API: er (utom .NET API), kan du definiera dessa Data Factory-enheter med hjälp av hello JSON-format.</span><span class="sxs-lookup"><span data-stu-id="0a727-129">When you use tools/APIs (except .NET API), you define these Data Factory entities by using hello JSON format.</span></span>  <span data-ttu-id="0a727-130">Ett exempel med JSON-definitioner för Data Factory-entiteter som ska använda toocopy data från ett dataarkiv för lokala Teradata finns [JSON-exempel: kopiera data från Teradata tooAzure Blob](#json-example-copy-data-from-teradata-to-azure-blob) i den här artikeln.</span><span class="sxs-lookup"><span data-stu-id="0a727-130">For a sample with JSON definitions for Data Factory entities that are used toocopy data from an on-premises Teradata data store, see [JSON example: Copy data from Teradata tooAzure Blob](#json-example-copy-data-from-teradata-to-azure-blob) section of this article.</span></span> 

<span data-ttu-id="0a727-131">hello följande avsnitt innehåller information om JSON-egenskaper används toodefine Data Factory entiteter specifika tooa Teradata-datalager:</span><span class="sxs-lookup"><span data-stu-id="0a727-131">hello following sections provide details about JSON properties that are used toodefine Data Factory entities specific tooa Teradata data store:</span></span>

## <a name="linked-service-properties"></a><span data-ttu-id="0a727-132">Länkad tjänstegenskaper</span><span class="sxs-lookup"><span data-stu-id="0a727-132">Linked service properties</span></span>
<span data-ttu-id="0a727-133">hello följande tabell innehåller en beskrivning för JSON-element specifika tooTeradata länkad tjänst.</span><span class="sxs-lookup"><span data-stu-id="0a727-133">hello following table provides description for JSON elements specific tooTeradata linked service.</span></span>

| <span data-ttu-id="0a727-134">Egenskap</span><span class="sxs-lookup"><span data-stu-id="0a727-134">Property</span></span> | <span data-ttu-id="0a727-135">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="0a727-135">Description</span></span> | <span data-ttu-id="0a727-136">Krävs</span><span class="sxs-lookup"><span data-stu-id="0a727-136">Required</span></span> |
| --- | --- | --- |
| <span data-ttu-id="0a727-137">typ</span><span class="sxs-lookup"><span data-stu-id="0a727-137">type</span></span> |<span data-ttu-id="0a727-138">hello Typegenskapen måste anges till: **OnPremisesTeradata**</span><span class="sxs-lookup"><span data-stu-id="0a727-138">hello type property must be set to: **OnPremisesTeradata**</span></span> |<span data-ttu-id="0a727-139">Ja</span><span class="sxs-lookup"><span data-stu-id="0a727-139">Yes</span></span> |
| <span data-ttu-id="0a727-140">server</span><span class="sxs-lookup"><span data-stu-id="0a727-140">server</span></span> |<span data-ttu-id="0a727-141">Namnet på hello Teradata-server.</span><span class="sxs-lookup"><span data-stu-id="0a727-141">Name of hello Teradata server.</span></span> |<span data-ttu-id="0a727-142">Ja</span><span class="sxs-lookup"><span data-stu-id="0a727-142">Yes</span></span> |
| <span data-ttu-id="0a727-143">AuthenticationType</span><span class="sxs-lookup"><span data-stu-id="0a727-143">authenticationType</span></span> |<span data-ttu-id="0a727-144">Typ av autentisering används tooconnect toohello Teradata-databasen.</span><span class="sxs-lookup"><span data-stu-id="0a727-144">Type of authentication used tooconnect toohello Teradata database.</span></span> <span data-ttu-id="0a727-145">Möjliga värden är: anonym, grundläggande och Windows.</span><span class="sxs-lookup"><span data-stu-id="0a727-145">Possible values are: Anonymous, Basic, and Windows.</span></span> |<span data-ttu-id="0a727-146">Ja</span><span class="sxs-lookup"><span data-stu-id="0a727-146">Yes</span></span> |
| <span data-ttu-id="0a727-147">användarnamn</span><span class="sxs-lookup"><span data-stu-id="0a727-147">username</span></span> |<span data-ttu-id="0a727-148">Ange användarnamnet om du använder grundläggande eller Windows-autentisering.</span><span class="sxs-lookup"><span data-stu-id="0a727-148">Specify user name if you are using Basic or Windows authentication.</span></span> |<span data-ttu-id="0a727-149">Nej</span><span class="sxs-lookup"><span data-stu-id="0a727-149">No</span></span> |
| <span data-ttu-id="0a727-150">lösenord</span><span class="sxs-lookup"><span data-stu-id="0a727-150">password</span></span> |<span data-ttu-id="0a727-151">Ange lösenord för hello-användarkonto som du angav för hello användarnamn.</span><span class="sxs-lookup"><span data-stu-id="0a727-151">Specify password for hello user account you specified for hello username.</span></span> |<span data-ttu-id="0a727-152">Nej</span><span class="sxs-lookup"><span data-stu-id="0a727-152">No</span></span> |
| <span data-ttu-id="0a727-153">gatewayName</span><span class="sxs-lookup"><span data-stu-id="0a727-153">gatewayName</span></span> |<span data-ttu-id="0a727-154">Namnet på hello-gateway som hello Data Factory-tjänsten ska använda tooconnect toohello lokala Teradata-databasen.</span><span class="sxs-lookup"><span data-stu-id="0a727-154">Name of hello gateway that hello Data Factory service should use tooconnect toohello on-premises Teradata database.</span></span> |<span data-ttu-id="0a727-155">Ja</span><span class="sxs-lookup"><span data-stu-id="0a727-155">Yes</span></span> |

## <a name="dataset-properties"></a><span data-ttu-id="0a727-156">Egenskaper för datamängd</span><span class="sxs-lookup"><span data-stu-id="0a727-156">Dataset properties</span></span>
<span data-ttu-id="0a727-157">En fullständig lista över egenskaper som är tillgängliga för att definiera datauppsättningarna & avsnitt finns hello [skapa datauppsättningar](data-factory-create-datasets.md) artikel.</span><span class="sxs-lookup"><span data-stu-id="0a727-157">For a full list of sections & properties available for defining datasets, see hello [Creating datasets](data-factory-create-datasets.md) article.</span></span> <span data-ttu-id="0a727-158">Avsnitt som struktur, tillgänglighet och princip på en datamängd JSON är liknande för alla typer av dataset (Azure SQL Azure blob, Azure-tabellen, osv.).</span><span class="sxs-lookup"><span data-stu-id="0a727-158">Sections such as structure, availability, and policy of a dataset JSON are similar for all dataset types (Azure SQL, Azure blob, Azure table, etc.).</span></span>

<span data-ttu-id="0a727-159">Hej **typeProperties** avsnitt är olika för varje typ av dataset och ger information om hello platsen för hello data i datalagret hello.</span><span class="sxs-lookup"><span data-stu-id="0a727-159">hello **typeProperties** section is different for each type of dataset and provides information about hello location of hello data in hello data store.</span></span> <span data-ttu-id="0a727-160">Det finns för närvarande inga egenskaper som stöds för hello Teradata dataset.</span><span class="sxs-lookup"><span data-stu-id="0a727-160">Currently, there are no type properties supported for hello Teradata dataset.</span></span>

## <a name="copy-activity-properties"></a><span data-ttu-id="0a727-161">Kopiera egenskaper för aktivitet</span><span class="sxs-lookup"><span data-stu-id="0a727-161">Copy activity properties</span></span>
<span data-ttu-id="0a727-162">En fullständig lista över avsnitt & egenskaper som är tillgängliga för att definiera aktiviteter finns hello [skapar Pipelines](data-factory-create-pipelines.md) artikel.</span><span class="sxs-lookup"><span data-stu-id="0a727-162">For a full list of sections & properties available for defining activities, see hello [Creating Pipelines](data-factory-create-pipelines.md) article.</span></span> <span data-ttu-id="0a727-163">Egenskaper som namn, beskrivning, ingående och utgående tabeller och principer är tillgängliga för alla typer av aktiviteter.</span><span class="sxs-lookup"><span data-stu-id="0a727-163">Properties such as name, description, input and output tables, and policies are available for all types of activities.</span></span>

<span data-ttu-id="0a727-164">De egenskaper som är tillgängliga under hello typeProperties i hello aktivitet varierar beroende på varje aktivitetstyp.</span><span class="sxs-lookup"><span data-stu-id="0a727-164">Whereas, properties available in hello typeProperties section of hello activity vary with each activity type.</span></span> <span data-ttu-id="0a727-165">För Kopieringsaktivitet kan varierar de beroende på hello typer av datakällor och sänkor.</span><span class="sxs-lookup"><span data-stu-id="0a727-165">For Copy activity, they vary depending on hello types of sources and sinks.</span></span>

<span data-ttu-id="0a727-166">När hello källan är av typen **RelationalSource** (som omfattar Teradata), hello följande egenskaper är tillgängliga i **typeProperties** avsnitt:</span><span class="sxs-lookup"><span data-stu-id="0a727-166">When hello source is of type **RelationalSource** (which includes Teradata), hello following properties are available in **typeProperties** section:</span></span>

| <span data-ttu-id="0a727-167">Egenskap</span><span class="sxs-lookup"><span data-stu-id="0a727-167">Property</span></span> | <span data-ttu-id="0a727-168">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="0a727-168">Description</span></span> | <span data-ttu-id="0a727-169">Tillåtna värden</span><span class="sxs-lookup"><span data-stu-id="0a727-169">Allowed values</span></span> | <span data-ttu-id="0a727-170">Krävs</span><span class="sxs-lookup"><span data-stu-id="0a727-170">Required</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="0a727-171">DocumentDB</span><span class="sxs-lookup"><span data-stu-id="0a727-171">query</span></span> |<span data-ttu-id="0a727-172">Använda hello anpassad fråga tooread data.</span><span class="sxs-lookup"><span data-stu-id="0a727-172">Use hello custom query tooread data.</span></span> |<span data-ttu-id="0a727-173">SQL-sträng.</span><span class="sxs-lookup"><span data-stu-id="0a727-173">SQL query string.</span></span> <span data-ttu-id="0a727-174">Till exempel: Välj * från mytable prefix.</span><span class="sxs-lookup"><span data-stu-id="0a727-174">For example: select * from MyTable.</span></span> |<span data-ttu-id="0a727-175">Ja</span><span class="sxs-lookup"><span data-stu-id="0a727-175">Yes</span></span> |

### <a name="json-example-copy-data-from-teradata-tooazure-blob"></a><span data-ttu-id="0a727-176">JSON-exempel: kopiera data från Teradata tooAzure Blob</span><span class="sxs-lookup"><span data-stu-id="0a727-176">JSON example: Copy data from Teradata tooAzure Blob</span></span>
<span data-ttu-id="0a727-177">hello följande exempel innehåller exempel JSON definitioner som du kan använda toocreate en pipeline med hjälp av [Azure-portalen](data-factory-copy-activity-tutorial-using-azure-portal.md) eller [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md) eller [Azure PowerShell](data-factory-copy-activity-tutorial-using-powershell.md).</span><span class="sxs-lookup"><span data-stu-id="0a727-177">hello following example provides sample JSON definitions that you can use toocreate a pipeline by using [Azure portal](data-factory-copy-activity-tutorial-using-azure-portal.md) or [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md) or [Azure PowerShell](data-factory-copy-activity-tutorial-using-powershell.md).</span></span> <span data-ttu-id="0a727-178">De visar hur toocopy data från Teradata tooAzure Blob Storage.</span><span class="sxs-lookup"><span data-stu-id="0a727-178">They show how toocopy data from Teradata tooAzure Blob Storage.</span></span> <span data-ttu-id="0a727-179">Data kan dock vara kopierade tooany av hello sänkor anges [här](data-factory-data-movement-activities.md#supported-data-stores-and-formats) med hello Kopieringsaktiviteten i Azure Data Factory.</span><span class="sxs-lookup"><span data-stu-id="0a727-179">However, data can be copied tooany of hello sinks stated [here](data-factory-data-movement-activities.md#supported-data-stores-and-formats) using hello Copy Activity in Azure Data Factory.</span></span>   

<span data-ttu-id="0a727-180">hello exemplet har hello följande data factory-enheter:</span><span class="sxs-lookup"><span data-stu-id="0a727-180">hello sample has hello following data factory entities:</span></span>

1. <span data-ttu-id="0a727-181">En länkad tjänst av typen [OnPremisesTeradata](#linked-service-properties).</span><span class="sxs-lookup"><span data-stu-id="0a727-181">A linked service of type [OnPremisesTeradata](#linked-service-properties).</span></span>
2. <span data-ttu-id="0a727-182">En länkad tjänst av typen [AzureStorage](data-factory-azure-blob-connector.md#linked-service-properties).</span><span class="sxs-lookup"><span data-stu-id="0a727-182">A linked service of type [AzureStorage](data-factory-azure-blob-connector.md#linked-service-properties).</span></span>
3. <span data-ttu-id="0a727-183">Indata [dataset](data-factory-create-datasets.md) av typen [RelationalTable](#dataset-properties).</span><span class="sxs-lookup"><span data-stu-id="0a727-183">An input [dataset](data-factory-create-datasets.md) of type [RelationalTable](#dataset-properties).</span></span>
4. <span data-ttu-id="0a727-184">Utdata [dataset](data-factory-create-datasets.md) av typen [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties).</span><span class="sxs-lookup"><span data-stu-id="0a727-184">An output [dataset](data-factory-create-datasets.md) of type [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties).</span></span>
5. <span data-ttu-id="0a727-185">Hej [pipeline](data-factory-create-pipelines.md) med Kopieringsaktiviteten som använder [RelationalSource](#copy-activity-properties) och [BlobSink](data-factory-azure-blob-connector.md#copy-activity-properties).</span><span class="sxs-lookup"><span data-stu-id="0a727-185">hello [pipeline](data-factory-create-pipelines.md) with Copy Activity that uses [RelationalSource](#copy-activity-properties) and [BlobSink](data-factory-azure-blob-connector.md#copy-activity-properties).</span></span>

<span data-ttu-id="0a727-186">hello exemplet kopierar data från ett frågeresultat i Teradata-databasen tooa blob varje timme.</span><span class="sxs-lookup"><span data-stu-id="0a727-186">hello sample copies data from a query result in Teradata database tooa blob every hour.</span></span> <span data-ttu-id="0a727-187">hello JSON egenskaper som används i exemplen beskrivs i hello-exempel i följande avsnitt.</span><span class="sxs-lookup"><span data-stu-id="0a727-187">hello JSON properties used in these samples are described in sections following hello samples.</span></span>

<span data-ttu-id="0a727-188">Som ett första steg bör du konfigurera hello data management gateway.</span><span class="sxs-lookup"><span data-stu-id="0a727-188">As a first step, setup hello data management gateway.</span></span> <span data-ttu-id="0a727-189">hello anvisningar finns i hello [flytta data mellan lokala platser och moln](data-factory-move-data-between-onprem-and-cloud.md) artikel.</span><span class="sxs-lookup"><span data-stu-id="0a727-189">hello instructions are in hello [moving data between on-premises locations and cloud](data-factory-move-data-between-onprem-and-cloud.md) article.</span></span>

<span data-ttu-id="0a727-190">**Teradata länkade tjänsten:**</span><span class="sxs-lookup"><span data-stu-id="0a727-190">**Teradata linked service:**</span></span>

```json
{
    "name": "OnPremTeradataLinkedService",
    "properties": {
        "type": "OnPremisesTeradata",
        "typeProperties": {
            "server": "<server>",
            "authenticationType": "<authentication type>",
            "username": "<username>",
            "password": "<password>",
            "gatewayName": "<gatewayName>"
        }
    }
}
```

<span data-ttu-id="0a727-191">**Azure Blob storage länkade tjänsten:**</span><span class="sxs-lookup"><span data-stu-id="0a727-191">**Azure Blob storage linked service:**</span></span>

```json
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

<span data-ttu-id="0a727-192">**Teradata inkommande datauppsättningen:**</span><span class="sxs-lookup"><span data-stu-id="0a727-192">**Teradata input dataset:**</span></span>

<span data-ttu-id="0a727-193">hello exemplet förutsätter att du har skapat en tabell ”mytable” som prefix i Teradata och innehåller en kolumn med namnet ”tidsstämpel” för tid series-data.</span><span class="sxs-lookup"><span data-stu-id="0a727-193">hello sample assumes you have created a table “MyTable” in Teradata and it contains a column called “timestamp” for time series data.</span></span>

<span data-ttu-id="0a727-194">Inställningen ”externa”: true informerar hello Data Factory-tjänsten hello tabellen är externa toohello data factory och inte tillverkas av en aktivitet i hello data factory.</span><span class="sxs-lookup"><span data-stu-id="0a727-194">Setting “external”: true informs hello Data Factory service that hello table is external toohello data factory and is not produced by an activity in hello data factory.</span></span>

```json
{
    "name": "TeradataDataSet",
    "properties": {
        "published": false,
        "type": "RelationalTable",
        "linkedServiceName": "OnPremTeradataLinkedService",
        "typeProperties": {
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

<span data-ttu-id="0a727-195">**Azure Blob utdatauppsättningen:**</span><span class="sxs-lookup"><span data-stu-id="0a727-195">**Azure Blob output dataset:**</span></span>

<span data-ttu-id="0a727-196">Data skrivs tooa nya blob varje timme (frekvens: timme, intervall: 1).</span><span class="sxs-lookup"><span data-stu-id="0a727-196">Data is written tooa new blob every hour (frequency: hour, interval: 1).</span></span> <span data-ttu-id="0a727-197">hello mappsökväg för hello blob utvärderas dynamiskt baserat på hello starttiden för hello-segment som bearbetas.</span><span class="sxs-lookup"><span data-stu-id="0a727-197">hello folder path for hello blob is dynamically evaluated based on hello start time of hello slice that is being processed.</span></span> <span data-ttu-id="0a727-198">hello mappsökväg använder år, månad, dag och timmar delar av hello starttid.</span><span class="sxs-lookup"><span data-stu-id="0a727-198">hello folder path uses year, month, day, and hours parts of hello start time.</span></span>

```json
{
    "name": "AzureBlobTeradataDataSet",
    "properties": {
        "published": false,
        "location": {
            "type": "AzureBlobLocation",
            "folderPath": "mycontainer/teradata/yearno={Year}/monthno={Month}/dayno={Day}/hourno={Hour}",
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
            ],
            "linkedServiceName": "AzureStorageLinkedService"
        },
        "availability": {
            "frequency": "Hour",
            "interval": 1
        }
    }
}
```
<span data-ttu-id="0a727-199">**Pipeline med kopieringsaktiviteten:**</span><span class="sxs-lookup"><span data-stu-id="0a727-199">**Pipeline with Copy activity:**</span></span>

<span data-ttu-id="0a727-200">hello pipelinen innehåller en kopia-aktivitet som är konfigurerade toouse hello inkommande och utgående datauppsättningar och är schemalagda toorun varje timme.</span><span class="sxs-lookup"><span data-stu-id="0a727-200">hello pipeline contains a Copy Activity that is configured toouse hello input and output datasets and is scheduled toorun hourly.</span></span> <span data-ttu-id="0a727-201">I hello pipeline JSON-definitionen hello **källa** typ har angetts för**RelationalSource** och **sink** typ har angetts för**BlobSink**.</span><span class="sxs-lookup"><span data-stu-id="0a727-201">In hello pipeline JSON definition, hello **source** type is set too**RelationalSource** and **sink** type is set too**BlobSink**.</span></span> <span data-ttu-id="0a727-202">hello SQL-frågan som angetts för hello **frågan** egenskapen väljer hello data i hello tidigare timme toocopy.</span><span class="sxs-lookup"><span data-stu-id="0a727-202">hello SQL query specified for hello **query** property selects hello data in hello past hour toocopy.</span></span>

```json
{
    "name": "CopyTeradataToBlob",
    "properties": {
        "description": "pipeline for copy activity",
        "activities": [
            {
                "type": "Copy",
                "typeProperties": {
                    "source": {
                        "type": "RelationalSource",
                        "query": "$$Text.Format('select * from MyTable where timestamp >= \\'{0:yyyy-MM-ddTHH:mm:ss}\\' AND timestamp < \\'{1:yyyy-MM-ddTHH:mm:ss}\\'', SliceStart, SliceEnd)"
                    },
                    "sink": {
                        "type": "BlobSink",
                        "writeBatchSize": 0,
                        "writeBatchTimeout": "00:00:00"
                    }
                },
                "inputs": [
                    {
                        "name": "TeradataDataSet"
                    }
                ],
                "outputs": [
                    {
                        "name": "AzureBlobTeradataDataSet"
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
                "name": "TeradataToBlob"
            }
        ],
        "start": "2014-06-01T18:00:00Z",
        "end": "2014-06-01T19:00:00Z",
        "isPaused": false
    }
}
```
## <a name="type-mapping-for-teradata"></a><span data-ttu-id="0a727-203">Mappning för Teradata</span><span class="sxs-lookup"><span data-stu-id="0a727-203">Type mapping for Teradata</span></span>
<span data-ttu-id="0a727-204">Som anges i hello [data movement aktiviteter](data-factory-data-movement-activities.md) artikeln hello kopieringsaktiviteten utför automatisk konverteringar från källan typer toosink typer med hello följande metod i steg 2:</span><span class="sxs-lookup"><span data-stu-id="0a727-204">As mentioned in hello [data movement activities](data-factory-data-movement-activities.md) article, hello Copy activity performs automatic type conversions from source types toosink types with hello following 2-step approach:</span></span>

1. <span data-ttu-id="0a727-205">Konvertera från inbyggda typer too.NET källtypen</span><span class="sxs-lookup"><span data-stu-id="0a727-205">Convert from native source types too.NET type</span></span>
2. <span data-ttu-id="0a727-206">Konvertera från .NET typen toonative Mottagartypen</span><span class="sxs-lookup"><span data-stu-id="0a727-206">Convert from .NET type toonative sink type</span></span>

<span data-ttu-id="0a727-207">När du flyttar data tooTeradata som hello följande mappningar används från Teradata typ too.NET.</span><span class="sxs-lookup"><span data-stu-id="0a727-207">When moving data tooTeradata, hello following mappings are used from Teradata type too.NET type.</span></span>

| <span data-ttu-id="0a727-208">Typen för Teradata-databasen</span><span class="sxs-lookup"><span data-stu-id="0a727-208">Teradata Database type</span></span> | <span data-ttu-id="0a727-209">.NET framework-typ</span><span class="sxs-lookup"><span data-stu-id="0a727-209">.NET Framework type</span></span> |
| --- | --- |
| <span data-ttu-id="0a727-210">Char</span><span class="sxs-lookup"><span data-stu-id="0a727-210">Char</span></span> |<span data-ttu-id="0a727-211">Sträng</span><span class="sxs-lookup"><span data-stu-id="0a727-211">String</span></span> |
| <span data-ttu-id="0a727-212">CLOB</span><span class="sxs-lookup"><span data-stu-id="0a727-212">Clob</span></span> |<span data-ttu-id="0a727-213">Sträng</span><span class="sxs-lookup"><span data-stu-id="0a727-213">String</span></span> |
| <span data-ttu-id="0a727-214">Bild</span><span class="sxs-lookup"><span data-stu-id="0a727-214">Graphic</span></span> |<span data-ttu-id="0a727-215">Sträng</span><span class="sxs-lookup"><span data-stu-id="0a727-215">String</span></span> |
| <span data-ttu-id="0a727-216">VarChar</span><span class="sxs-lookup"><span data-stu-id="0a727-216">VarChar</span></span> |<span data-ttu-id="0a727-217">Sträng</span><span class="sxs-lookup"><span data-stu-id="0a727-217">String</span></span> |
| <span data-ttu-id="0a727-218">VarGraphic</span><span class="sxs-lookup"><span data-stu-id="0a727-218">VarGraphic</span></span> |<span data-ttu-id="0a727-219">Sträng</span><span class="sxs-lookup"><span data-stu-id="0a727-219">String</span></span> |
| <span data-ttu-id="0a727-220">Blob</span><span class="sxs-lookup"><span data-stu-id="0a727-220">Blob</span></span> |<span data-ttu-id="0a727-221">byte]</span><span class="sxs-lookup"><span data-stu-id="0a727-221">Byte[]</span></span> |
| <span data-ttu-id="0a727-222">Mottagna byte</span><span class="sxs-lookup"><span data-stu-id="0a727-222">Byte</span></span> |<span data-ttu-id="0a727-223">byte]</span><span class="sxs-lookup"><span data-stu-id="0a727-223">Byte[]</span></span> |
| <span data-ttu-id="0a727-224">VarByte</span><span class="sxs-lookup"><span data-stu-id="0a727-224">VarByte</span></span> |<span data-ttu-id="0a727-225">byte]</span><span class="sxs-lookup"><span data-stu-id="0a727-225">Byte[]</span></span> |
| <span data-ttu-id="0a727-226">BigInt</span><span class="sxs-lookup"><span data-stu-id="0a727-226">BigInt</span></span> |<span data-ttu-id="0a727-227">Int64</span><span class="sxs-lookup"><span data-stu-id="0a727-227">Int64</span></span> |
| <span data-ttu-id="0a727-228">ByteInt</span><span class="sxs-lookup"><span data-stu-id="0a727-228">ByteInt</span></span> |<span data-ttu-id="0a727-229">Int16</span><span class="sxs-lookup"><span data-stu-id="0a727-229">Int16</span></span> |
| <span data-ttu-id="0a727-230">Decimal</span><span class="sxs-lookup"><span data-stu-id="0a727-230">Decimal</span></span> |<span data-ttu-id="0a727-231">Decimal</span><span class="sxs-lookup"><span data-stu-id="0a727-231">Decimal</span></span> |
| <span data-ttu-id="0a727-232">dubbla</span><span class="sxs-lookup"><span data-stu-id="0a727-232">Double</span></span> |<span data-ttu-id="0a727-233">dubbla</span><span class="sxs-lookup"><span data-stu-id="0a727-233">Double</span></span> |
| <span data-ttu-id="0a727-234">Integer</span><span class="sxs-lookup"><span data-stu-id="0a727-234">Integer</span></span> |<span data-ttu-id="0a727-235">Int32</span><span class="sxs-lookup"><span data-stu-id="0a727-235">Int32</span></span> |
| <span data-ttu-id="0a727-236">Tal</span><span class="sxs-lookup"><span data-stu-id="0a727-236">Number</span></span> |<span data-ttu-id="0a727-237">dubbla</span><span class="sxs-lookup"><span data-stu-id="0a727-237">Double</span></span> |
| <span data-ttu-id="0a727-238">SmallInt</span><span class="sxs-lookup"><span data-stu-id="0a727-238">SmallInt</span></span> |<span data-ttu-id="0a727-239">Int16</span><span class="sxs-lookup"><span data-stu-id="0a727-239">Int16</span></span> |
| <span data-ttu-id="0a727-240">Date</span><span class="sxs-lookup"><span data-stu-id="0a727-240">Date</span></span> |<span data-ttu-id="0a727-241">Datum och tid</span><span class="sxs-lookup"><span data-stu-id="0a727-241">DateTime</span></span> |
| <span data-ttu-id="0a727-242">Tid</span><span class="sxs-lookup"><span data-stu-id="0a727-242">Time</span></span> |<span data-ttu-id="0a727-243">TimeSpan</span><span class="sxs-lookup"><span data-stu-id="0a727-243">TimeSpan</span></span> |
| <span data-ttu-id="0a727-244">Tid med tidszon</span><span class="sxs-lookup"><span data-stu-id="0a727-244">Time With Time Zone</span></span> |<span data-ttu-id="0a727-245">Sträng</span><span class="sxs-lookup"><span data-stu-id="0a727-245">String</span></span> |
| <span data-ttu-id="0a727-246">tidsstämpel</span><span class="sxs-lookup"><span data-stu-id="0a727-246">Timestamp</span></span> |<span data-ttu-id="0a727-247">Datum och tid</span><span class="sxs-lookup"><span data-stu-id="0a727-247">DateTime</span></span> |
| <span data-ttu-id="0a727-248">Tidsstämpel med tidszon</span><span class="sxs-lookup"><span data-stu-id="0a727-248">Timestamp With Time Zone</span></span> |<span data-ttu-id="0a727-249">DateTimeOffset</span><span class="sxs-lookup"><span data-stu-id="0a727-249">DateTimeOffset</span></span> |
| <span data-ttu-id="0a727-250">Intervall dag</span><span class="sxs-lookup"><span data-stu-id="0a727-250">Interval Day</span></span> |<span data-ttu-id="0a727-251">TimeSpan</span><span class="sxs-lookup"><span data-stu-id="0a727-251">TimeSpan</span></span> |
| <span data-ttu-id="0a727-252">Intervall dag tooHour</span><span class="sxs-lookup"><span data-stu-id="0a727-252">Interval Day tooHour</span></span> |<span data-ttu-id="0a727-253">TimeSpan</span><span class="sxs-lookup"><span data-stu-id="0a727-253">TimeSpan</span></span> |
| <span data-ttu-id="0a727-254">Intervall dag tooMinute</span><span class="sxs-lookup"><span data-stu-id="0a727-254">Interval Day tooMinute</span></span> |<span data-ttu-id="0a727-255">TimeSpan</span><span class="sxs-lookup"><span data-stu-id="0a727-255">TimeSpan</span></span> |
| <span data-ttu-id="0a727-256">Intervall dag tooSecond</span><span class="sxs-lookup"><span data-stu-id="0a727-256">Interval Day tooSecond</span></span> |<span data-ttu-id="0a727-257">TimeSpan</span><span class="sxs-lookup"><span data-stu-id="0a727-257">TimeSpan</span></span> |
| <span data-ttu-id="0a727-258">Intervall timme</span><span class="sxs-lookup"><span data-stu-id="0a727-258">Interval Hour</span></span> |<span data-ttu-id="0a727-259">TimeSpan</span><span class="sxs-lookup"><span data-stu-id="0a727-259">TimeSpan</span></span> |
| <span data-ttu-id="0a727-260">Intervall timme tooMinute</span><span class="sxs-lookup"><span data-stu-id="0a727-260">Interval Hour tooMinute</span></span> |<span data-ttu-id="0a727-261">TimeSpan</span><span class="sxs-lookup"><span data-stu-id="0a727-261">TimeSpan</span></span> |
| <span data-ttu-id="0a727-262">Intervall timme tooSecond</span><span class="sxs-lookup"><span data-stu-id="0a727-262">Interval Hour tooSecond</span></span> |<span data-ttu-id="0a727-263">TimeSpan</span><span class="sxs-lookup"><span data-stu-id="0a727-263">TimeSpan</span></span> |
| <span data-ttu-id="0a727-264">Intervall minut</span><span class="sxs-lookup"><span data-stu-id="0a727-264">Interval Minute</span></span> |<span data-ttu-id="0a727-265">TimeSpan</span><span class="sxs-lookup"><span data-stu-id="0a727-265">TimeSpan</span></span> |
| <span data-ttu-id="0a727-266">Intervall minut tooSecond</span><span class="sxs-lookup"><span data-stu-id="0a727-266">Interval Minute tooSecond</span></span> |<span data-ttu-id="0a727-267">TimeSpan</span><span class="sxs-lookup"><span data-stu-id="0a727-267">TimeSpan</span></span> |
| <span data-ttu-id="0a727-268">Intervall för andra</span><span class="sxs-lookup"><span data-stu-id="0a727-268">Interval Second</span></span> |<span data-ttu-id="0a727-269">TimeSpan</span><span class="sxs-lookup"><span data-stu-id="0a727-269">TimeSpan</span></span> |
| <span data-ttu-id="0a727-270">Intervall år</span><span class="sxs-lookup"><span data-stu-id="0a727-270">Interval Year</span></span> |<span data-ttu-id="0a727-271">Sträng</span><span class="sxs-lookup"><span data-stu-id="0a727-271">String</span></span> |
| <span data-ttu-id="0a727-272">Intervall år tooMonth</span><span class="sxs-lookup"><span data-stu-id="0a727-272">Interval Year tooMonth</span></span> |<span data-ttu-id="0a727-273">Sträng</span><span class="sxs-lookup"><span data-stu-id="0a727-273">String</span></span> |
| <span data-ttu-id="0a727-274">Intervall för månad</span><span class="sxs-lookup"><span data-stu-id="0a727-274">Interval Month</span></span> |<span data-ttu-id="0a727-275">Sträng</span><span class="sxs-lookup"><span data-stu-id="0a727-275">String</span></span> |
| <span data-ttu-id="0a727-276">Period(Date)</span><span class="sxs-lookup"><span data-stu-id="0a727-276">Period(Date)</span></span> |<span data-ttu-id="0a727-277">Sträng</span><span class="sxs-lookup"><span data-stu-id="0a727-277">String</span></span> |
| <span data-ttu-id="0a727-278">Period(Time)</span><span class="sxs-lookup"><span data-stu-id="0a727-278">Period(Time)</span></span> |<span data-ttu-id="0a727-279">Sträng</span><span class="sxs-lookup"><span data-stu-id="0a727-279">String</span></span> |
| <span data-ttu-id="0a727-280">Tid (Time med tidszon)</span><span class="sxs-lookup"><span data-stu-id="0a727-280">Period(Time With Time Zone)</span></span> |<span data-ttu-id="0a727-281">Sträng</span><span class="sxs-lookup"><span data-stu-id="0a727-281">String</span></span> |
| <span data-ttu-id="0a727-282">Period(timestamp)</span><span class="sxs-lookup"><span data-stu-id="0a727-282">Period(Timestamp)</span></span> |<span data-ttu-id="0a727-283">Sträng</span><span class="sxs-lookup"><span data-stu-id="0a727-283">String</span></span> |
| <span data-ttu-id="0a727-284">Tid (tidsstämpel med tidszon)</span><span class="sxs-lookup"><span data-stu-id="0a727-284">Period(Timestamp With Time Zone)</span></span> |<span data-ttu-id="0a727-285">Sträng</span><span class="sxs-lookup"><span data-stu-id="0a727-285">String</span></span> |
| <span data-ttu-id="0a727-286">XML</span><span class="sxs-lookup"><span data-stu-id="0a727-286">Xml</span></span> |<span data-ttu-id="0a727-287">Sträng</span><span class="sxs-lookup"><span data-stu-id="0a727-287">String</span></span> |

## <a name="map-source-toosink-columns"></a><span data-ttu-id="0a727-288">Mappa källkolumner toosink</span><span class="sxs-lookup"><span data-stu-id="0a727-288">Map source toosink columns</span></span>
<span data-ttu-id="0a727-289">toolearn mappning tabellkolumner i källan dataset toocolumns i sink dataset finns [mappa dataset kolumner i Azure Data Factory](data-factory-map-columns.md).</span><span class="sxs-lookup"><span data-stu-id="0a727-289">toolearn about mapping columns in source dataset toocolumns in sink dataset, see [Mapping dataset columns in Azure Data Factory](data-factory-map-columns.md).</span></span>

## <a name="repeatable-read-from-relational-sources"></a><span data-ttu-id="0a727-290">Upprepbar läsning från relationella källor</span><span class="sxs-lookup"><span data-stu-id="0a727-290">Repeatable read from relational sources</span></span>
<span data-ttu-id="0a727-291">När du kopierar data från relationella datalager, Kom ihåg tooavoid repeterbarhet oönskade resultat.</span><span class="sxs-lookup"><span data-stu-id="0a727-291">When copying data from relational data stores, keep repeatability in mind tooavoid unintended outcomes.</span></span> <span data-ttu-id="0a727-292">I Azure Data Factory, kan du köra en sektor manuellt.</span><span class="sxs-lookup"><span data-stu-id="0a727-292">In Azure Data Factory, you can rerun a slice manually.</span></span> <span data-ttu-id="0a727-293">Du kan också konfigurera i principen för en dataset så att ett segment som körs när ett fel uppstår.</span><span class="sxs-lookup"><span data-stu-id="0a727-293">You can also configure retry policy for a dataset so that a slice is rerun when a failure occurs.</span></span> <span data-ttu-id="0a727-294">När ett segment körs på antingen sätt måste toomake att som hello samma data läses oavsett hur många gånger ett segment körs.</span><span class="sxs-lookup"><span data-stu-id="0a727-294">When a slice is rerun in either way, you need toomake sure that hello same data is read no matter how many times a slice is run.</span></span> <span data-ttu-id="0a727-295">Se [Repeatable läsa från relationella källor](data-factory-repeatable-copy.md#repeatable-read-from-relational-sources).</span><span class="sxs-lookup"><span data-stu-id="0a727-295">See [Repeatable read from relational sources](data-factory-repeatable-copy.md#repeatable-read-from-relational-sources).</span></span>

## <a name="performance-and-tuning"></a><span data-ttu-id="0a727-296">Prestanda och finjustering</span><span class="sxs-lookup"><span data-stu-id="0a727-296">Performance and Tuning</span></span>
<span data-ttu-id="0a727-297">Se [kopiera aktivitet prestanda och justera guiden](data-factory-copy-activity-performance.md) toolearn om nyckeln faktorer som påverkan prestanda för flytt av data (Kopieringsaktiviteten) i Azure Data Factory och olika sätt toooptimize den.</span><span class="sxs-lookup"><span data-stu-id="0a727-297">See [Copy Activity Performance & Tuning Guide](data-factory-copy-activity-performance.md) toolearn about key factors that impact performance of data movement (Copy Activity) in Azure Data Factory and various ways toooptimize it.</span></span>
