---
title: "aaaMove data från Sybase med Azure Data Factory | Microsoft Docs"
description: "Mer information om hur toomove data från Sybase-databas med hjälp av Azure Data Factory."
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
ms.openlocfilehash: ad003ec502028d56db9570fe08af329eb5b71817
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="move-data-from-sybase-using-azure-data-factory"></a><span data-ttu-id="a59bf-103">Flytta data från Sybase med Azure Data Factory</span><span class="sxs-lookup"><span data-stu-id="a59bf-103">Move data from Sybase using Azure Data Factory</span></span>
<span data-ttu-id="a59bf-104">Den här artikeln förklarar hur toouse hello Kopieringsaktiviteten i Azure Data Factory toomove data från en lokal Sybase-databas.</span><span class="sxs-lookup"><span data-stu-id="a59bf-104">This article explains how toouse hello Copy Activity in Azure Data Factory toomove data from an on-premises Sybase database.</span></span> <span data-ttu-id="a59bf-105">Den bygger på hello [Data Movement aktiviteter](data-factory-data-movement-activities.md) artikel som ger en allmän översikt över dataflyttning hello kopieringsaktiviteten.</span><span class="sxs-lookup"><span data-stu-id="a59bf-105">It builds on hello [Data Movement Activities](data-factory-data-movement-activities.md) article, which presents a general overview of data movement with hello copy activity.</span></span>

<span data-ttu-id="a59bf-106">Du kan kopiera data från ett lokalt Sybase data store tooany stöds sink dataarkiv.</span><span class="sxs-lookup"><span data-stu-id="a59bf-106">You can copy data from an on-premises Sybase data store tooany supported sink data store.</span></span> <span data-ttu-id="a59bf-107">En lista över data lagras som stöds när egenskaperna av hello kopieringsaktiviteten Se hello [stöds datalager](data-factory-data-movement-activities.md#supported-data-stores-and-formats) tabell.</span><span class="sxs-lookup"><span data-stu-id="a59bf-107">For a list of data stores supported as sinks by hello copy activity, see hello [Supported data stores](data-factory-data-movement-activities.md#supported-data-stores-and-formats) table.</span></span> <span data-ttu-id="a59bf-108">Data factory stöder för närvarande endast flytta data från en Sybase lagra tooother datalager, men inte för att flytta data från andra lagrar tooa Sybase data dataarkiv.</span><span class="sxs-lookup"><span data-stu-id="a59bf-108">Data factory currently supports only moving data from a Sybase data store tooother data stores, but not for moving data from other data stores tooa Sybase data store.</span></span> 

## <a name="prerequisites"></a><span data-ttu-id="a59bf-109">Krav</span><span class="sxs-lookup"><span data-stu-id="a59bf-109">Prerequisites</span></span>
<span data-ttu-id="a59bf-110">Data Factory-tjänsten stöder anslutande tooon lokala Sybase källor med hello Data Management Gateway.</span><span class="sxs-lookup"><span data-stu-id="a59bf-110">Data Factory service supports connecting tooon-premises Sybase sources using hello Data Management Gateway.</span></span> <span data-ttu-id="a59bf-111">Se [flytta data mellan lokala platser och moln](data-factory-move-data-between-onprem-and-cloud.md) artikel toolearn om Data Management Gateway och stegvisa instruktioner om hur du konfigurerar hello gateway.</span><span class="sxs-lookup"><span data-stu-id="a59bf-111">See [moving data between on-premises locations and cloud](data-factory-move-data-between-onprem-and-cloud.md) article toolearn about Data Management Gateway and step-by-step instructions on setting up hello gateway.</span></span>

<span data-ttu-id="a59bf-112">Gateway krävs även om hello Sybase-databasen finns i en Azure IaaS-VM.</span><span class="sxs-lookup"><span data-stu-id="a59bf-112">Gateway is required even if hello Sybase database is hosted in an Azure IaaS VM.</span></span> <span data-ttu-id="a59bf-113">Du kan installera hello gateway på hello samma IaaS VM som hello data lagras eller på en annan virtuell dator så länge som hello gateway kan ansluta toohello databas.</span><span class="sxs-lookup"><span data-stu-id="a59bf-113">You can install hello gateway on hello same IaaS VM as hello data store or on a different VM as long as hello gateway can connect toohello database.</span></span>

> [!NOTE]
> <span data-ttu-id="a59bf-114">Se [felsökning av problem med gateway](data-factory-data-management-gateway.md#troubleshooting-gateway-issues) tips om hur du felsöker anslutning /-gateway relaterade problem.</span><span class="sxs-lookup"><span data-stu-id="a59bf-114">See [Troubleshoot gateway issues](data-factory-data-management-gateway.md#troubleshooting-gateway-issues) for tips on troubleshooting connection/gateway related issues.</span></span>

## <a name="supported-versions-and-installation"></a><span data-ttu-id="a59bf-115">Versioner som stöds och installation</span><span class="sxs-lookup"><span data-stu-id="a59bf-115">Supported versions and installation</span></span>
<span data-ttu-id="a59bf-116">För Data Management Gateway tooconnect toohello Sybase-databas måste tooinstall hello [dataprovider för Sybase iAnywhere.Data.SQLAnywhere](http://go.microsoft.com/fwlink/?linkid=324846) 16 eller högre på hello samma system som hello Data Management Gateway.</span><span class="sxs-lookup"><span data-stu-id="a59bf-116">For Data Management Gateway tooconnect toohello Sybase Database, you need tooinstall hello [data provider for Sybase iAnywhere.Data.SQLAnywhere](http://go.microsoft.com/fwlink/?linkid=324846) 16 or above on hello same system as hello Data Management Gateway.</span></span> <span data-ttu-id="a59bf-117">Sybase version 16 och senare stöds.</span><span class="sxs-lookup"><span data-stu-id="a59bf-117">Sybase version 16 and above is supported.</span></span>

## <a name="getting-started"></a><span data-ttu-id="a59bf-118">Komma igång</span><span class="sxs-lookup"><span data-stu-id="a59bf-118">Getting started</span></span>
<span data-ttu-id="a59bf-119">Du kan skapa en pipeline med en kopia-aktivitet som flyttar data från en lokal Cassandra data store med hjälp av olika verktyg/API: er.</span><span class="sxs-lookup"><span data-stu-id="a59bf-119">You can create a pipeline with a copy activity that moves data from an on-premises Cassandra data store by using different tools/APIs.</span></span> 

- <span data-ttu-id="a59bf-120">hello enklaste sättet toocreate en pipeline är toouse hello **guiden Kopiera**.</span><span class="sxs-lookup"><span data-stu-id="a59bf-120">hello easiest way toocreate a pipeline is toouse hello **Copy Wizard**.</span></span> <span data-ttu-id="a59bf-121">Se [Självstudier: skapa en pipeline med hjälp av guiden Kopiera](data-factory-copy-data-wizard-tutorial.md) för en snabb genomgång om hur du skapar en pipeline med hjälp av guiden för hello kopiera data.</span><span class="sxs-lookup"><span data-stu-id="a59bf-121">See [Tutorial: Create a pipeline using Copy Wizard](data-factory-copy-data-wizard-tutorial.md) for a quick walkthrough on creating a pipeline using hello Copy data wizard.</span></span> 
- <span data-ttu-id="a59bf-122">Du kan också använda följande verktyg toocreate en pipeline hello: **Azure-portalen**, **Visual Studio**, **Azure PowerShell**, **Azure Resource Manager-mall** , **.NET API**, och **REST API**.</span><span class="sxs-lookup"><span data-stu-id="a59bf-122">You can also use hello following tools toocreate a pipeline: **Azure portal**, **Visual Studio**, **Azure PowerShell**, **Azure Resource Manager template**, **.NET API**, and **REST API**.</span></span> <span data-ttu-id="a59bf-123">Se [kopiera aktivitet kursen](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) för stegvisa instruktioner toocreate en pipeline med en Kopieringsaktivitet.</span><span class="sxs-lookup"><span data-stu-id="a59bf-123">See [Copy activity tutorial](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) for step-by-step instructions toocreate a pipeline with a copy activity.</span></span> 

<span data-ttu-id="a59bf-124">Om du använder hello verktyg eller API: er kan utföra du hello följande steg toocreate en pipeline som flyttar data från en källdata lagra tooa sink-datalagret:</span><span class="sxs-lookup"><span data-stu-id="a59bf-124">Whether you use hello tools or APIs, you perform hello following steps toocreate a pipeline that moves data from a source data store tooa sink data store:</span></span>

1. <span data-ttu-id="a59bf-125">Skapa **länkade tjänster** toolink indata och utdata lagrar tooyour data factory.</span><span class="sxs-lookup"><span data-stu-id="a59bf-125">Create **linked services** toolink input and output data stores tooyour data factory.</span></span>
2. <span data-ttu-id="a59bf-126">Skapa **datauppsättningar** toorepresent indata och utdata för hello kopieringsåtgärden.</span><span class="sxs-lookup"><span data-stu-id="a59bf-126">Create **datasets** toorepresent input and output data for hello copy operation.</span></span> 
3. <span data-ttu-id="a59bf-127">Skapa en **pipeline** med en kopia-aktivitet som tar en datamängd som indata och en dataset som utdata.</span><span class="sxs-lookup"><span data-stu-id="a59bf-127">Create a **pipeline** with a copy activity that takes a dataset as an input and a dataset as an output.</span></span> 

<span data-ttu-id="a59bf-128">När du använder guiden hello skapas automatiskt JSON definitioner för dessa Data Factory-enheter (länkade tjänster, datauppsättningar och hello pipeline) för dig.</span><span class="sxs-lookup"><span data-stu-id="a59bf-128">When you use hello wizard, JSON definitions for these Data Factory entities (linked services, datasets, and hello pipeline) are automatically created for you.</span></span> <span data-ttu-id="a59bf-129">När du använder Verktyg/API: er (utom .NET API), kan du definiera dessa Data Factory-enheter med hjälp av hello JSON-format.</span><span class="sxs-lookup"><span data-stu-id="a59bf-129">When you use tools/APIs (except .NET API), you define these Data Factory entities by using hello JSON format.</span></span>  <span data-ttu-id="a59bf-130">Ett exempel med JSON-definitioner för Data Factory-entiteter som ska använda toocopy data från ett dataarkiv för lokala Sybase finns [JSON-exempel: kopiera data från Sybase tooAzure Blob](#json-example-copy-data-from-sybase-to-azure-blob) i den här artikeln.</span><span class="sxs-lookup"><span data-stu-id="a59bf-130">For a sample with JSON definitions for Data Factory entities that are used toocopy data from an on-premises Sybase data store, see [JSON example: Copy data from Sybase tooAzure Blob](#json-example-copy-data-from-sybase-to-azure-blob) section of this article.</span></span> 

<span data-ttu-id="a59bf-131">hello följande avsnitt innehåller information om JSON-egenskaper används toodefine Data Factory entiteter specifika tooa Sybase-datalager:</span><span class="sxs-lookup"><span data-stu-id="a59bf-131">hello following sections provide details about JSON properties that are used toodefine Data Factory entities specific tooa Sybase data store:</span></span>

## <a name="linked-service-properties"></a><span data-ttu-id="a59bf-132">Länkad tjänstegenskaper</span><span class="sxs-lookup"><span data-stu-id="a59bf-132">Linked service properties</span></span>
<span data-ttu-id="a59bf-133">hello följande tabell innehåller en beskrivning för JSON-element specifika tooSybase länkad tjänst.</span><span class="sxs-lookup"><span data-stu-id="a59bf-133">hello following table provides description for JSON elements specific tooSybase linked service.</span></span>

| <span data-ttu-id="a59bf-134">Egenskap</span><span class="sxs-lookup"><span data-stu-id="a59bf-134">Property</span></span> | <span data-ttu-id="a59bf-135">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="a59bf-135">Description</span></span> | <span data-ttu-id="a59bf-136">Krävs</span><span class="sxs-lookup"><span data-stu-id="a59bf-136">Required</span></span> |
| --- | --- | --- |
| <span data-ttu-id="a59bf-137">typ</span><span class="sxs-lookup"><span data-stu-id="a59bf-137">type</span></span> |<span data-ttu-id="a59bf-138">hello Typegenskapen måste anges till: **OnPremisesSybase**</span><span class="sxs-lookup"><span data-stu-id="a59bf-138">hello type property must be set to: **OnPremisesSybase**</span></span> |<span data-ttu-id="a59bf-139">Ja</span><span class="sxs-lookup"><span data-stu-id="a59bf-139">Yes</span></span> |
| <span data-ttu-id="a59bf-140">server</span><span class="sxs-lookup"><span data-stu-id="a59bf-140">server</span></span> |<span data-ttu-id="a59bf-141">Namnet på hello Sybase-servern.</span><span class="sxs-lookup"><span data-stu-id="a59bf-141">Name of hello Sybase server.</span></span> |<span data-ttu-id="a59bf-142">Ja</span><span class="sxs-lookup"><span data-stu-id="a59bf-142">Yes</span></span> |
| <span data-ttu-id="a59bf-143">Databasen</span><span class="sxs-lookup"><span data-stu-id="a59bf-143">database</span></span> |<span data-ttu-id="a59bf-144">Namnet på hello Sybase-databas.</span><span class="sxs-lookup"><span data-stu-id="a59bf-144">Name of hello Sybase database.</span></span> |<span data-ttu-id="a59bf-145">Ja</span><span class="sxs-lookup"><span data-stu-id="a59bf-145">Yes</span></span> |
| <span data-ttu-id="a59bf-146">Schemat</span><span class="sxs-lookup"><span data-stu-id="a59bf-146">schema</span></span> |<span data-ttu-id="a59bf-147">Namnet på hello schema i hello-databasen.</span><span class="sxs-lookup"><span data-stu-id="a59bf-147">Name of hello schema in hello database.</span></span> |<span data-ttu-id="a59bf-148">Nej</span><span class="sxs-lookup"><span data-stu-id="a59bf-148">No</span></span> |
| <span data-ttu-id="a59bf-149">AuthenticationType</span><span class="sxs-lookup"><span data-stu-id="a59bf-149">authenticationType</span></span> |<span data-ttu-id="a59bf-150">Typ av autentisering används tooconnect toohello Sybase-databas.</span><span class="sxs-lookup"><span data-stu-id="a59bf-150">Type of authentication used tooconnect toohello Sybase database.</span></span> <span data-ttu-id="a59bf-151">Möjliga värden är: anonym, grundläggande och Windows.</span><span class="sxs-lookup"><span data-stu-id="a59bf-151">Possible values are: Anonymous, Basic, and Windows.</span></span> |<span data-ttu-id="a59bf-152">Ja</span><span class="sxs-lookup"><span data-stu-id="a59bf-152">Yes</span></span> |
| <span data-ttu-id="a59bf-153">användarnamn</span><span class="sxs-lookup"><span data-stu-id="a59bf-153">username</span></span> |<span data-ttu-id="a59bf-154">Ange användarnamnet om du använder grundläggande eller Windows-autentisering.</span><span class="sxs-lookup"><span data-stu-id="a59bf-154">Specify user name if you are using Basic or Windows authentication.</span></span> |<span data-ttu-id="a59bf-155">Nej</span><span class="sxs-lookup"><span data-stu-id="a59bf-155">No</span></span> |
| <span data-ttu-id="a59bf-156">lösenord</span><span class="sxs-lookup"><span data-stu-id="a59bf-156">password</span></span> |<span data-ttu-id="a59bf-157">Ange lösenord för hello-användarkonto som du angav för hello användarnamn.</span><span class="sxs-lookup"><span data-stu-id="a59bf-157">Specify password for hello user account you specified for hello username.</span></span> |<span data-ttu-id="a59bf-158">Nej</span><span class="sxs-lookup"><span data-stu-id="a59bf-158">No</span></span> |
| <span data-ttu-id="a59bf-159">gatewayName</span><span class="sxs-lookup"><span data-stu-id="a59bf-159">gatewayName</span></span> |<span data-ttu-id="a59bf-160">Namnet på hello-gateway som hello Data Factory-tjänsten ska använda tooconnect toohello lokala Sybase-databas.</span><span class="sxs-lookup"><span data-stu-id="a59bf-160">Name of hello gateway that hello Data Factory service should use tooconnect toohello on-premises Sybase database.</span></span> |<span data-ttu-id="a59bf-161">Ja</span><span class="sxs-lookup"><span data-stu-id="a59bf-161">Yes</span></span> |

## <a name="dataset-properties"></a><span data-ttu-id="a59bf-162">Egenskaper för datamängd</span><span class="sxs-lookup"><span data-stu-id="a59bf-162">Dataset properties</span></span>
<span data-ttu-id="a59bf-163">En fullständig lista över egenskaper som är tillgängliga för att definiera datauppsättningarna & avsnitt finns hello [skapa datauppsättningar](data-factory-create-datasets.md) artikel.</span><span class="sxs-lookup"><span data-stu-id="a59bf-163">For a full list of sections & properties available for defining datasets, see hello [Creating datasets](data-factory-create-datasets.md) article.</span></span> <span data-ttu-id="a59bf-164">Avsnitt som struktur, tillgänglighet och princip på en datamängd JSON är liknande för alla typer av dataset (Azure SQL Azure blob, Azure-tabellen, osv.).</span><span class="sxs-lookup"><span data-stu-id="a59bf-164">Sections such as structure, availability, and policy of a dataset JSON are similar for all dataset types (Azure SQL, Azure blob, Azure table, etc.).</span></span>

<span data-ttu-id="a59bf-165">hello typeProperties avsnittet är olika för varje typ av dataset och ger information om hello platsen för hello data i datalagret hello.</span><span class="sxs-lookup"><span data-stu-id="a59bf-165">hello typeProperties section is different for each type of dataset and provides information about hello location of hello data in hello data store.</span></span> <span data-ttu-id="a59bf-166">Hej **typeProperties** avsnittet för dataset av typen **RelationalTable** (som omfattar Sybase dataset) har hello följande egenskaper:</span><span class="sxs-lookup"><span data-stu-id="a59bf-166">hello **typeProperties** section for dataset of type **RelationalTable** (which includes Sybase dataset) has hello following properties:</span></span>

| <span data-ttu-id="a59bf-167">Egenskap</span><span class="sxs-lookup"><span data-stu-id="a59bf-167">Property</span></span> | <span data-ttu-id="a59bf-168">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="a59bf-168">Description</span></span> | <span data-ttu-id="a59bf-169">Krävs</span><span class="sxs-lookup"><span data-stu-id="a59bf-169">Required</span></span> |
| --- | --- | --- |
| <span data-ttu-id="a59bf-170">tableName</span><span class="sxs-lookup"><span data-stu-id="a59bf-170">tableName</span></span> |<span data-ttu-id="a59bf-171">Namnet på hello tabell i hello Sybase-databasinstansen som den länkade tjänsten refererar till.</span><span class="sxs-lookup"><span data-stu-id="a59bf-171">Name of hello table in hello Sybase Database instance that linked service refers to.</span></span> |<span data-ttu-id="a59bf-172">Nej (om **frågan** av **RelationalSource** har angetts)</span><span class="sxs-lookup"><span data-stu-id="a59bf-172">No (if **query** of **RelationalSource** is specified)</span></span> |

## <a name="copy-activity-properties"></a><span data-ttu-id="a59bf-173">Kopiera egenskaper för aktivitet</span><span class="sxs-lookup"><span data-stu-id="a59bf-173">Copy activity properties</span></span>
<span data-ttu-id="a59bf-174">En fullständig lista över avsnitt & egenskaper som är tillgängliga för att definiera aktiviteter, se [skapar Pipelines](data-factory-create-pipelines.md) artikel.</span><span class="sxs-lookup"><span data-stu-id="a59bf-174">For a full list of sections & properties available for defining activities, see [Creating Pipelines](data-factory-create-pipelines.md) article.</span></span> <span data-ttu-id="a59bf-175">Egenskaper som namn, beskrivning, ingående och utgående tabeller och principen är tillgängliga för alla typer av aktiviteter.</span><span class="sxs-lookup"><span data-stu-id="a59bf-175">Properties such as name, description, input and output tables, and policy are available for all types of activities.</span></span>

<span data-ttu-id="a59bf-176">De egenskaper som är tillgängliga under hello typeProperties i hello aktivitet varierar beroende på varje aktivitetstyp.</span><span class="sxs-lookup"><span data-stu-id="a59bf-176">Whereas, properties available in hello typeProperties section of hello activity vary with each activity type.</span></span> <span data-ttu-id="a59bf-177">För Kopieringsaktivitet kan varierar de beroende på hello typer av datakällor och sänkor.</span><span class="sxs-lookup"><span data-stu-id="a59bf-177">For Copy activity, they vary depending on hello types of sources and sinks.</span></span>

<span data-ttu-id="a59bf-178">När hello källan är av typen **RelationalSource** (som omfattar Sybase), hello följande egenskaper är tillgängliga i **typeProperties** avsnitt:</span><span class="sxs-lookup"><span data-stu-id="a59bf-178">When hello source is of type **RelationalSource** (which includes Sybase), hello following properties are available in **typeProperties** section:</span></span>

| <span data-ttu-id="a59bf-179">Egenskap</span><span class="sxs-lookup"><span data-stu-id="a59bf-179">Property</span></span> | <span data-ttu-id="a59bf-180">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="a59bf-180">Description</span></span> | <span data-ttu-id="a59bf-181">Tillåtna värden</span><span class="sxs-lookup"><span data-stu-id="a59bf-181">Allowed values</span></span> | <span data-ttu-id="a59bf-182">Krävs</span><span class="sxs-lookup"><span data-stu-id="a59bf-182">Required</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="a59bf-183">DocumentDB</span><span class="sxs-lookup"><span data-stu-id="a59bf-183">query</span></span> |<span data-ttu-id="a59bf-184">Använda hello anpassad fråga tooread data.</span><span class="sxs-lookup"><span data-stu-id="a59bf-184">Use hello custom query tooread data.</span></span> |<span data-ttu-id="a59bf-185">SQL-sträng.</span><span class="sxs-lookup"><span data-stu-id="a59bf-185">SQL query string.</span></span> <span data-ttu-id="a59bf-186">Till exempel: Välj * från mytable prefix.</span><span class="sxs-lookup"><span data-stu-id="a59bf-186">For example: select * from MyTable.</span></span> |<span data-ttu-id="a59bf-187">Nej (om **tableName** av **dataset** har angetts)</span><span class="sxs-lookup"><span data-stu-id="a59bf-187">No (if **tableName** of **dataset** is specified)</span></span> |


## <a name="json-example-copy-data-from-sybase-tooazure-blob"></a><span data-ttu-id="a59bf-188">JSON-exempel: kopiera data från Sybase tooAzure Blob</span><span class="sxs-lookup"><span data-stu-id="a59bf-188">JSON example: Copy data from Sybase tooAzure Blob</span></span>
<span data-ttu-id="a59bf-189">hello följande exempel innehåller exempel JSON definitioner som du kan använda toocreate en pipeline med hjälp av [Azure-portalen](data-factory-copy-activity-tutorial-using-azure-portal.md) eller [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md) eller [Azure PowerShell](data-factory-copy-activity-tutorial-using-powershell.md).</span><span class="sxs-lookup"><span data-stu-id="a59bf-189">hello following example provides sample JSON definitions that you can use toocreate a pipeline by using [Azure portal](data-factory-copy-activity-tutorial-using-azure-portal.md) or [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md) or [Azure PowerShell](data-factory-copy-activity-tutorial-using-powershell.md).</span></span> <span data-ttu-id="a59bf-190">De visar hur toocopy data från Sybase-databas tooAzure Blob Storage.</span><span class="sxs-lookup"><span data-stu-id="a59bf-190">They show how toocopy data from Sybase database tooAzure Blob Storage.</span></span> <span data-ttu-id="a59bf-191">Data kan dock vara kopierade tooany av hello sänkor anges [här](data-factory-data-movement-activities.md#supported-data-stores-and-formats) med hello Kopieringsaktiviteten i Azure Data Factory.</span><span class="sxs-lookup"><span data-stu-id="a59bf-191">However, data can be copied tooany of hello sinks stated [here](data-factory-data-movement-activities.md#supported-data-stores-and-formats) using hello Copy Activity in Azure Data Factory.</span></span>   

<span data-ttu-id="a59bf-192">hello exemplet har hello följande data factory-enheter:</span><span class="sxs-lookup"><span data-stu-id="a59bf-192">hello sample has hello following data factory entities:</span></span>

1. <span data-ttu-id="a59bf-193">En länkad tjänst av typen [OnPremisesSybase](data-factory-onprem-sybase-connector.md#linked-service-properties).</span><span class="sxs-lookup"><span data-stu-id="a59bf-193">A linked service of type [OnPremisesSybase](data-factory-onprem-sybase-connector.md#linked-service-properties).</span></span>
2. <span data-ttu-id="a59bf-194">En liked tjänst av typen [AzureStorage](data-factory-azure-blob-connector.md#linked-service-properties).</span><span class="sxs-lookup"><span data-stu-id="a59bf-194">A liked service of type [AzureStorage](data-factory-azure-blob-connector.md#linked-service-properties).</span></span>
3. <span data-ttu-id="a59bf-195">Indata [dataset](data-factory-create-datasets.md) av typen [RelationalTable](data-factory-onprem-sybase-connector.md#dataset-properties).</span><span class="sxs-lookup"><span data-stu-id="a59bf-195">An input [dataset](data-factory-create-datasets.md) of type [RelationalTable](data-factory-onprem-sybase-connector.md#dataset-properties).</span></span>
4. <span data-ttu-id="a59bf-196">Utdata [dataset](data-factory-create-datasets.md) av typen [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties).</span><span class="sxs-lookup"><span data-stu-id="a59bf-196">An output [dataset](data-factory-create-datasets.md) of type [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties).</span></span>
5. <span data-ttu-id="a59bf-197">Hej [pipeline](data-factory-create-pipelines.md) med Kopieringsaktiviteten som använder [RelationalSource](data-factory-onprem-sybase-connector.md#copy-activity-properties) och [BlobSink](data-factory-azure-blob-connector.md#copy-activity-properties).</span><span class="sxs-lookup"><span data-stu-id="a59bf-197">hello [pipeline](data-factory-create-pipelines.md) with Copy Activity that uses [RelationalSource](data-factory-onprem-sybase-connector.md#copy-activity-properties) and [BlobSink](data-factory-azure-blob-connector.md#copy-activity-properties).</span></span>

<span data-ttu-id="a59bf-198">hello exemplet kopierar data från ett frågeresultat i Sybase-databas tooa blob varje timme.</span><span class="sxs-lookup"><span data-stu-id="a59bf-198">hello sample copies data from a query result in Sybase database tooa blob every hour.</span></span> <span data-ttu-id="a59bf-199">hello JSON egenskaper som används i exemplen beskrivs i hello-exempel i följande avsnitt.</span><span class="sxs-lookup"><span data-stu-id="a59bf-199">hello JSON properties used in these samples are described in sections following hello samples.</span></span>

<span data-ttu-id="a59bf-200">Som ett första steg bör du konfigurera hello data management gateway.</span><span class="sxs-lookup"><span data-stu-id="a59bf-200">As a first step, setup hello data management gateway.</span></span> <span data-ttu-id="a59bf-201">hello anvisningar finns i hello [flytta data mellan lokala platser och moln](data-factory-move-data-between-onprem-and-cloud.md) artikel.</span><span class="sxs-lookup"><span data-stu-id="a59bf-201">hello instructions are in hello [moving data between on-premises locations and cloud](data-factory-move-data-between-onprem-and-cloud.md) article.</span></span>

<span data-ttu-id="a59bf-202">**Sybase länkade tjänsten:**</span><span class="sxs-lookup"><span data-stu-id="a59bf-202">**Sybase linked service:**</span></span>

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

<span data-ttu-id="a59bf-203">**Azure Blob storage länkade tjänsten:**</span><span class="sxs-lookup"><span data-stu-id="a59bf-203">**Azure Blob storage linked service:**</span></span>

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

<span data-ttu-id="a59bf-204">**Sybase inkommande datauppsättningen:**</span><span class="sxs-lookup"><span data-stu-id="a59bf-204">**Sybase input dataset:**</span></span>

<span data-ttu-id="a59bf-205">hello exemplet förutsätter att du har skapat en tabell ”mytable” som prefix i Sybase och innehåller en kolumn med namnet ”tidsstämpel” för tid series-data.</span><span class="sxs-lookup"><span data-stu-id="a59bf-205">hello sample assumes you have created a table “MyTable” in Sybase and it contains a column called “timestamp” for time series data.</span></span>

<span data-ttu-id="a59bf-206">Inställningen ”externa”: true informerar hello Data Factory-tjänsten att den här datauppsättningen är externa toohello data factory och inte tillverkas av en aktivitet i hello data factory.</span><span class="sxs-lookup"><span data-stu-id="a59bf-206">Setting “external”: true informs hello Data Factory service that this dataset is external toohello data factory and is not produced by an activity in hello data factory.</span></span> <span data-ttu-id="a59bf-207">Observera att hello **typen** av hello länkade tjänsten är inställd på: **RelationalTable**.</span><span class="sxs-lookup"><span data-stu-id="a59bf-207">Notice that hello **type** of hello linked service is set to: **RelationalTable**.</span></span>

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

<span data-ttu-id="a59bf-208">**Azure Blob utdatauppsättningen:**</span><span class="sxs-lookup"><span data-stu-id="a59bf-208">**Azure Blob output dataset:**</span></span>

<span data-ttu-id="a59bf-209">Data skrivs tooa nya blob varje timme (frekvens: timme, intervall: 1).</span><span class="sxs-lookup"><span data-stu-id="a59bf-209">Data is written tooa new blob every hour (frequency: hour, interval: 1).</span></span> <span data-ttu-id="a59bf-210">hello mappsökväg för hello blob utvärderas dynamiskt baserat på hello starttiden för hello-segment som bearbetas.</span><span class="sxs-lookup"><span data-stu-id="a59bf-210">hello folder path for hello blob is dynamically evaluated based on hello start time of hello slice that is being processed.</span></span> <span data-ttu-id="a59bf-211">hello mappsökväg använder år, månad, dag och timmar delar av hello starttid.</span><span class="sxs-lookup"><span data-stu-id="a59bf-211">hello folder path uses year, month, day, and hours parts of hello start time.</span></span>

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

<span data-ttu-id="a59bf-212">**Pipeline med kopieringsaktiviteten:**</span><span class="sxs-lookup"><span data-stu-id="a59bf-212">**Pipeline with Copy activity:**</span></span>

<span data-ttu-id="a59bf-213">hello pipelinen innehåller en kopia-aktivitet som är konfigurerade toouse hello inkommande och utgående datauppsättningar och är schemalagda toorun varje timme.</span><span class="sxs-lookup"><span data-stu-id="a59bf-213">hello pipeline contains a Copy Activity that is configured toouse hello input and output datasets and is scheduled toorun hourly.</span></span> <span data-ttu-id="a59bf-214">I hello pipeline JSON-definitionen hello **källa** typ har angetts för**RelationalSource** och **sink** typ har angetts för**BlobSink**.</span><span class="sxs-lookup"><span data-stu-id="a59bf-214">In hello pipeline JSON definition, hello **source** type is set too**RelationalSource** and **sink** type is set too**BlobSink**.</span></span> <span data-ttu-id="a59bf-215">hello SQL-frågan som angetts för hello **frågan** egenskapen väljer hello data från hello DBA. Ordertabellen i hello-databasen.</span><span class="sxs-lookup"><span data-stu-id="a59bf-215">hello SQL query specified for hello **query** property selects hello data from hello DBA.Orders table in hello database.</span></span>

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

## <a name="type-mapping-for-sybase"></a><span data-ttu-id="a59bf-216">Mappning för Sybase</span><span class="sxs-lookup"><span data-stu-id="a59bf-216">Type mapping for Sybase</span></span>
<span data-ttu-id="a59bf-217">Som anges i hello [Data Movement aktiviteter](data-factory-data-movement-activities.md) artikeln hello kopieringsaktiviteten utför automatisk konverteringar från källan typer toosink typer med hello följande metod i steg 2:</span><span class="sxs-lookup"><span data-stu-id="a59bf-217">As mentioned in hello [Data Movement Activities](data-factory-data-movement-activities.md) article, hello Copy activity performs automatic type conversions from source types toosink types with hello following 2-step approach:</span></span>

1. <span data-ttu-id="a59bf-218">Konvertera från inbyggda typer too.NET källtypen</span><span class="sxs-lookup"><span data-stu-id="a59bf-218">Convert from native source types too.NET type</span></span>
2. <span data-ttu-id="a59bf-219">Konvertera från .NET typen toonative Mottagartypen</span><span class="sxs-lookup"><span data-stu-id="a59bf-219">Convert from .NET type toonative sink type</span></span>

<span data-ttu-id="a59bf-220">Sybase stöder T-SQL och T-SQL-typer.</span><span class="sxs-lookup"><span data-stu-id="a59bf-220">Sybase supports T-SQL and T-SQL types.</span></span> <span data-ttu-id="a59bf-221">En Mappningstabell från sql-typer too.NET typ för finns [Azure SQL Connector](data-factory-azure-sql-connector.md) artikel.</span><span class="sxs-lookup"><span data-stu-id="a59bf-221">For a mapping table from sql types too.NET type, see [Azure SQL Connector](data-factory-azure-sql-connector.md) article.</span></span>

## <a name="map-source-toosink-columns"></a><span data-ttu-id="a59bf-222">Mappa källkolumner toosink</span><span class="sxs-lookup"><span data-stu-id="a59bf-222">Map source toosink columns</span></span>
<span data-ttu-id="a59bf-223">toolearn mappning tabellkolumner i källan dataset toocolumns i sink dataset finns [mappa dataset kolumner i Azure Data Factory](data-factory-map-columns.md).</span><span class="sxs-lookup"><span data-stu-id="a59bf-223">toolearn about mapping columns in source dataset toocolumns in sink dataset, see [Mapping dataset columns in Azure Data Factory](data-factory-map-columns.md).</span></span>

## <a name="repeatable-read-from-relational-sources"></a><span data-ttu-id="a59bf-224">Upprepbar läsning från relationella källor</span><span class="sxs-lookup"><span data-stu-id="a59bf-224">Repeatable read from relational sources</span></span>
<span data-ttu-id="a59bf-225">När du kopierar data från relationella datalager, Kom ihåg tooavoid repeterbarhet oönskade resultat.</span><span class="sxs-lookup"><span data-stu-id="a59bf-225">When copying data from relational data stores, keep repeatability in mind tooavoid unintended outcomes.</span></span> <span data-ttu-id="a59bf-226">I Azure Data Factory, kan du köra en sektor manuellt.</span><span class="sxs-lookup"><span data-stu-id="a59bf-226">In Azure Data Factory, you can rerun a slice manually.</span></span> <span data-ttu-id="a59bf-227">Du kan också konfigurera i principen för en dataset så att ett segment som körs när ett fel uppstår.</span><span class="sxs-lookup"><span data-stu-id="a59bf-227">You can also configure retry policy for a dataset so that a slice is rerun when a failure occurs.</span></span> <span data-ttu-id="a59bf-228">När ett segment körs på antingen sätt måste toomake att som hello samma data läses oavsett hur många gånger ett segment körs.</span><span class="sxs-lookup"><span data-stu-id="a59bf-228">When a slice is rerun in either way, you need toomake sure that hello same data is read no matter how many times a slice is run.</span></span> <span data-ttu-id="a59bf-229">Se [Repeatable läsa från relationella källor](data-factory-repeatable-copy.md#repeatable-read-from-relational-sources).</span><span class="sxs-lookup"><span data-stu-id="a59bf-229">See [Repeatable read from relational sources](data-factory-repeatable-copy.md#repeatable-read-from-relational-sources).</span></span>

## <a name="performance-and-tuning"></a><span data-ttu-id="a59bf-230">Prestanda och finjustering</span><span class="sxs-lookup"><span data-stu-id="a59bf-230">Performance and Tuning</span></span>
<span data-ttu-id="a59bf-231">Se [kopiera aktivitet prestanda och justera guiden](data-factory-copy-activity-performance.md) toolearn om nyckeln faktorer som påverkan prestanda för flytt av data (Kopieringsaktiviteten) i Azure Data Factory och olika sätt toooptimize den.</span><span class="sxs-lookup"><span data-stu-id="a59bf-231">See [Copy Activity Performance & Tuning Guide](data-factory-copy-activity-performance.md) toolearn about key factors that impact performance of data movement (Copy Activity) in Azure Data Factory and various ways toooptimize it.</span></span>
