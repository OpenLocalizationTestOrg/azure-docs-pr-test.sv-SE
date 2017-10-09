---
title: "aaaMove data från PostgreSQL med hjälp av Azure Data Factory | Microsoft Docs"
description: "Mer information om hur toomove data från PostgreSQL-databas med hjälp av Azure Data Factory."
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
ms.openlocfilehash: ea384f4e06f7d7bedae2949e4ea727c8f8806614
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="move-data-from-postgresql-using-azure-data-factory"></a><span data-ttu-id="b840a-103">Flytta data från PostgreSQL med hjälp av Azure Data Factory</span><span class="sxs-lookup"><span data-stu-id="b840a-103">Move data from PostgreSQL using Azure Data Factory</span></span>
<span data-ttu-id="b840a-104">Den här artikeln förklarar hur toouse hello Kopieringsaktiviteten i Azure Data Factory toomove data från en lokal PostgreSQL-databas.</span><span class="sxs-lookup"><span data-stu-id="b840a-104">This article explains how toouse hello Copy Activity in Azure Data Factory toomove data from an on-premises PostgreSQL database.</span></span> <span data-ttu-id="b840a-105">Den bygger på hello [Data Movement aktiviteter](data-factory-data-movement-activities.md) artikel som ger en allmän översikt över dataflyttning hello kopieringsaktiviteten.</span><span class="sxs-lookup"><span data-stu-id="b840a-105">It builds on hello [Data Movement Activities](data-factory-data-movement-activities.md) article, which presents a general overview of data movement with hello copy activity.</span></span>

<span data-ttu-id="b840a-106">Du kan kopiera data från ett lokalt PostgreSQL data store tooany stöds sink dataarkiv.</span><span class="sxs-lookup"><span data-stu-id="b840a-106">You can copy data from an on-premises PostgreSQL data store tooany supported sink data store.</span></span> <span data-ttu-id="b840a-107">En lista över datakällor som stöds som sänkor av hello kopieringsaktiviteten finns [stöds datalager](data-factory-data-movement-activities.md#supported-data-stores-and-formats).</span><span class="sxs-lookup"><span data-stu-id="b840a-107">For a list of data stores supported as sinks by hello copy activity, see [supported data stores](data-factory-data-movement-activities.md#supported-data-stores-and-formats).</span></span> <span data-ttu-id="b840a-108">Data factory för närvarande stöder flyttning av data från en PostgreSQL-databas tooother datalager, men inte för att flytta data från andra data lagras tooan PostgreSQL-databas.</span><span class="sxs-lookup"><span data-stu-id="b840a-108">Data factory currently supports moving data from a PostgreSQL database tooother data stores, but not for moving data from other data stores tooan PostgreSQL database.</span></span> 

## <a name="prerequisites"></a><span data-ttu-id="b840a-109">Nödvändiga komponenter</span><span class="sxs-lookup"><span data-stu-id="b840a-109">prerequisites</span></span>

<span data-ttu-id="b840a-110">Data Factory-tjänsten stöder anslutande tooon lokala PostgreSQL källor med hello Data Management Gateway.</span><span class="sxs-lookup"><span data-stu-id="b840a-110">Data Factory service supports connecting tooon-premises PostgreSQL sources using hello Data Management Gateway.</span></span> <span data-ttu-id="b840a-111">Se [flytta data mellan lokala platser och moln](data-factory-move-data-between-onprem-and-cloud.md) artikel toolearn om Data Management Gateway och stegvisa instruktioner om hur du konfigurerar hello gateway.</span><span class="sxs-lookup"><span data-stu-id="b840a-111">See [moving data between on-premises locations and cloud](data-factory-move-data-between-onprem-and-cloud.md) article toolearn about Data Management Gateway and step-by-step instructions on setting up hello gateway.</span></span>

<span data-ttu-id="b840a-112">Gateway krävs även om hello PostgreSQL-databasen finns i en Azure IaaS-VM.</span><span class="sxs-lookup"><span data-stu-id="b840a-112">Gateway is required even if hello PostgreSQL database is hosted in an Azure IaaS VM.</span></span> <span data-ttu-id="b840a-113">Du kan installera gatewayen på hello samma IaaS VM som hello data lagras eller på en annan virtuell dator så länge som hello gateway kan ansluta toohello databas.</span><span class="sxs-lookup"><span data-stu-id="b840a-113">You can install gateway on hello same IaaS VM as hello data store or on a different VM as long as hello gateway can connect toohello database.</span></span>

> [!NOTE]
> <span data-ttu-id="b840a-114">Se [felsökning av problem med gateway](data-factory-data-management-gateway.md#troubleshooting-gateway-issues) tips om hur du felsöker anslutning /-gateway relaterade problem.</span><span class="sxs-lookup"><span data-stu-id="b840a-114">See [Troubleshoot gateway issues](data-factory-data-management-gateway.md#troubleshooting-gateway-issues) for tips on troubleshooting connection/gateway related issues.</span></span>

## <a name="supported-versions-and-installation"></a><span data-ttu-id="b840a-115">Versioner som stöds och installation</span><span class="sxs-lookup"><span data-stu-id="b840a-115">Supported versions and installation</span></span>
<span data-ttu-id="b840a-116">För Data Management Gateway tooconnect toohello PostgreSQL-databas, installera hello [Ngpsql dataprovider för PostgreSQL](http://go.microsoft.com/fwlink/?linkid=282716) 2.0.12 eller ovan på hello samma system som hello Data Management Gateway.</span><span class="sxs-lookup"><span data-stu-id="b840a-116">For Data Management Gateway tooconnect toohello PostgreSQL Database, install hello [Ngpsql data provider for PostgreSQL](http://go.microsoft.com/fwlink/?linkid=282716) 2.0.12 or above on hello same system as hello Data Management Gateway.</span></span> <span data-ttu-id="b840a-117">PostgreSQL version 7.4 och senare stöds.</span><span class="sxs-lookup"><span data-stu-id="b840a-117">PostgreSQL version 7.4 and above is supported.</span></span>

## <a name="getting-started"></a><span data-ttu-id="b840a-118">Komma igång</span><span class="sxs-lookup"><span data-stu-id="b840a-118">Getting started</span></span>
<span data-ttu-id="b840a-119">Du kan skapa en pipeline med en kopia-aktivitet som flyttar data från ett dataarkiv för lokala PostgreSQL med hjälp av olika verktyg/API: er.</span><span class="sxs-lookup"><span data-stu-id="b840a-119">You can create a pipeline with a copy activity that moves data from an on-premises PostgreSQL data store by using different tools/APIs.</span></span> 

- <span data-ttu-id="b840a-120">hello enklaste sättet toocreate en pipeline är toouse hello **guiden Kopiera**.</span><span class="sxs-lookup"><span data-stu-id="b840a-120">hello easiest way toocreate a pipeline is toouse hello **Copy Wizard**.</span></span> <span data-ttu-id="b840a-121">Se [Självstudier: skapa en pipeline med hjälp av guiden Kopiera](data-factory-copy-data-wizard-tutorial.md) för en snabb genomgång om hur du skapar en pipeline med hjälp av guiden för hello kopiera data.</span><span class="sxs-lookup"><span data-stu-id="b840a-121">See [Tutorial: Create a pipeline using Copy Wizard](data-factory-copy-data-wizard-tutorial.md) for a quick walkthrough on creating a pipeline using hello Copy data wizard.</span></span> 
- <span data-ttu-id="b840a-122">Du kan också använda följande verktyg toocreate en pipeline hello:</span><span class="sxs-lookup"><span data-stu-id="b840a-122">You can also use hello following tools toocreate a pipeline:</span></span> 
    - <span data-ttu-id="b840a-123">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="b840a-123">Azure portal</span></span>
    - <span data-ttu-id="b840a-124">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="b840a-124">Visual Studio</span></span>
    - <span data-ttu-id="b840a-125">Azure PowerShell</span><span class="sxs-lookup"><span data-stu-id="b840a-125">Azure PowerShell</span></span>
    - <span data-ttu-id="b840a-126">Azure Resource Manager-mall</span><span class="sxs-lookup"><span data-stu-id="b840a-126">Azure Resource Manager template</span></span>
    - <span data-ttu-id="b840a-127">.NET-API</span><span class="sxs-lookup"><span data-stu-id="b840a-127">.NET API</span></span>
    - <span data-ttu-id="b840a-128">REST API</span><span class="sxs-lookup"><span data-stu-id="b840a-128">REST API</span></span>

     <span data-ttu-id="b840a-129">Se [kopiera aktivitet kursen](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) för stegvisa instruktioner toocreate en pipeline med en Kopieringsaktivitet.</span><span class="sxs-lookup"><span data-stu-id="b840a-129">See [Copy activity tutorial](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) for step-by-step instructions toocreate a pipeline with a copy activity.</span></span> 

<span data-ttu-id="b840a-130">Om du använder hello verktyg eller API: er kan utföra du hello följande steg toocreate en pipeline som flyttar data från en källdata lagra tooa sink-datalagret:</span><span class="sxs-lookup"><span data-stu-id="b840a-130">Whether you use hello tools or APIs, you perform hello following steps toocreate a pipeline that moves data from a source data store tooa sink data store:</span></span>

1. <span data-ttu-id="b840a-131">Skapa **länkade tjänster** toolink indata och utdata lagrar tooyour data factory.</span><span class="sxs-lookup"><span data-stu-id="b840a-131">Create **linked services** toolink input and output data stores tooyour data factory.</span></span>
2. <span data-ttu-id="b840a-132">Skapa **datauppsättningar** toorepresent indata och utdata för hello kopieringsåtgärden.</span><span class="sxs-lookup"><span data-stu-id="b840a-132">Create **datasets** toorepresent input and output data for hello copy operation.</span></span> 
3. <span data-ttu-id="b840a-133">Skapa en **pipeline** med en kopia-aktivitet som tar en datamängd som indata och en dataset som utdata.</span><span class="sxs-lookup"><span data-stu-id="b840a-133">Create a **pipeline** with a copy activity that takes a dataset as an input and a dataset as an output.</span></span> 

<span data-ttu-id="b840a-134">När du använder guiden hello skapas automatiskt JSON definitioner för dessa Data Factory-enheter (länkade tjänster, datauppsättningar och hello pipeline) för dig.</span><span class="sxs-lookup"><span data-stu-id="b840a-134">When you use hello wizard, JSON definitions for these Data Factory entities (linked services, datasets, and hello pipeline) are automatically created for you.</span></span> <span data-ttu-id="b840a-135">När du använder Verktyg/API: er (utom .NET API), kan du definiera dessa Data Factory-enheter med hjälp av hello JSON-format.</span><span class="sxs-lookup"><span data-stu-id="b840a-135">When you use tools/APIs (except .NET API), you define these Data Factory entities by using hello JSON format.</span></span>  <span data-ttu-id="b840a-136">Ett exempel med JSON-definitioner för Data Factory-entiteter som ska använda toocopy data från ett dataarkiv för lokala PostgreSQL finns [JSON-exempel: kopiera data från PostgreSQL tooAzure Blob](#json-example-copy-data-from-postgresql-to-azure-blob) i den här artikeln.</span><span class="sxs-lookup"><span data-stu-id="b840a-136">For a sample with JSON definitions for Data Factory entities that are used toocopy data from an on-premises PostgreSQL data store, see [JSON example: Copy data from PostgreSQL tooAzure Blob](#json-example-copy-data-from-postgresql-to-azure-blob) section of this article.</span></span> 

<span data-ttu-id="b840a-137">hello följande avsnitt innehåller information om JSON-egenskaper används toodefine Data Factory entiteter specifika tooa PostgreSQL-datalager:</span><span class="sxs-lookup"><span data-stu-id="b840a-137">hello following sections provide details about JSON properties that are used toodefine Data Factory entities specific tooa PostgreSQL data store:</span></span>

## <a name="linked-service-properties"></a><span data-ttu-id="b840a-138">Länkad tjänstegenskaper</span><span class="sxs-lookup"><span data-stu-id="b840a-138">Linked service properties</span></span>
<span data-ttu-id="b840a-139">hello följande tabell innehåller en beskrivning för JSON-element specifika tooPostgreSQL länkad tjänst.</span><span class="sxs-lookup"><span data-stu-id="b840a-139">hello following table provides description for JSON elements specific tooPostgreSQL linked service.</span></span>

| <span data-ttu-id="b840a-140">Egenskap</span><span class="sxs-lookup"><span data-stu-id="b840a-140">Property</span></span> | <span data-ttu-id="b840a-141">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="b840a-141">Description</span></span> | <span data-ttu-id="b840a-142">Krävs</span><span class="sxs-lookup"><span data-stu-id="b840a-142">Required</span></span> |
| --- | --- | --- |
| <span data-ttu-id="b840a-143">typ</span><span class="sxs-lookup"><span data-stu-id="b840a-143">type</span></span> |<span data-ttu-id="b840a-144">hello Typegenskapen måste anges till: **OnPremisesPostgreSql**</span><span class="sxs-lookup"><span data-stu-id="b840a-144">hello type property must be set to: **OnPremisesPostgreSql**</span></span> |<span data-ttu-id="b840a-145">Ja</span><span class="sxs-lookup"><span data-stu-id="b840a-145">Yes</span></span> |
| <span data-ttu-id="b840a-146">server</span><span class="sxs-lookup"><span data-stu-id="b840a-146">server</span></span> |<span data-ttu-id="b840a-147">Namnet på hello PostgreSQL-server.</span><span class="sxs-lookup"><span data-stu-id="b840a-147">Name of hello PostgreSQL server.</span></span> |<span data-ttu-id="b840a-148">Ja</span><span class="sxs-lookup"><span data-stu-id="b840a-148">Yes</span></span> |
| <span data-ttu-id="b840a-149">Databasen</span><span class="sxs-lookup"><span data-stu-id="b840a-149">database</span></span> |<span data-ttu-id="b840a-150">Namnet på hello PostgreSQL-databas.</span><span class="sxs-lookup"><span data-stu-id="b840a-150">Name of hello PostgreSQL database.</span></span> |<span data-ttu-id="b840a-151">Ja</span><span class="sxs-lookup"><span data-stu-id="b840a-151">Yes</span></span> |
| <span data-ttu-id="b840a-152">Schemat</span><span class="sxs-lookup"><span data-stu-id="b840a-152">schema</span></span> |<span data-ttu-id="b840a-153">Namnet på hello schema i hello-databasen.</span><span class="sxs-lookup"><span data-stu-id="b840a-153">Name of hello schema in hello database.</span></span> <span data-ttu-id="b840a-154">hello schemanamnet är skiftlägeskänslig.</span><span class="sxs-lookup"><span data-stu-id="b840a-154">hello schema name is case-sensitive.</span></span> |<span data-ttu-id="b840a-155">Nej</span><span class="sxs-lookup"><span data-stu-id="b840a-155">No</span></span> |
| <span data-ttu-id="b840a-156">AuthenticationType</span><span class="sxs-lookup"><span data-stu-id="b840a-156">authenticationType</span></span> |<span data-ttu-id="b840a-157">Typ av autentisering används tooconnect toohello PostgreSQL-databas.</span><span class="sxs-lookup"><span data-stu-id="b840a-157">Type of authentication used tooconnect toohello PostgreSQL database.</span></span> <span data-ttu-id="b840a-158">Möjliga värden är: anonym, grundläggande och Windows.</span><span class="sxs-lookup"><span data-stu-id="b840a-158">Possible values are: Anonymous, Basic, and Windows.</span></span> |<span data-ttu-id="b840a-159">Ja</span><span class="sxs-lookup"><span data-stu-id="b840a-159">Yes</span></span> |
| <span data-ttu-id="b840a-160">användarnamn</span><span class="sxs-lookup"><span data-stu-id="b840a-160">username</span></span> |<span data-ttu-id="b840a-161">Ange användarnamnet om du använder grundläggande eller Windows-autentisering.</span><span class="sxs-lookup"><span data-stu-id="b840a-161">Specify user name if you are using Basic or Windows authentication.</span></span> |<span data-ttu-id="b840a-162">Nej</span><span class="sxs-lookup"><span data-stu-id="b840a-162">No</span></span> |
| <span data-ttu-id="b840a-163">lösenord</span><span class="sxs-lookup"><span data-stu-id="b840a-163">password</span></span> |<span data-ttu-id="b840a-164">Ange lösenord för hello-användarkonto som du angav för hello användarnamn.</span><span class="sxs-lookup"><span data-stu-id="b840a-164">Specify password for hello user account you specified for hello username.</span></span> |<span data-ttu-id="b840a-165">Nej</span><span class="sxs-lookup"><span data-stu-id="b840a-165">No</span></span> |
| <span data-ttu-id="b840a-166">gatewayName</span><span class="sxs-lookup"><span data-stu-id="b840a-166">gatewayName</span></span> |<span data-ttu-id="b840a-167">Namnet på hello-gateway som hello Data Factory-tjänsten ska använda tooconnect toohello lokala PostgreSQL-databas.</span><span class="sxs-lookup"><span data-stu-id="b840a-167">Name of hello gateway that hello Data Factory service should use tooconnect toohello on-premises PostgreSQL database.</span></span> |<span data-ttu-id="b840a-168">Ja</span><span class="sxs-lookup"><span data-stu-id="b840a-168">Yes</span></span> |

## <a name="dataset-properties"></a><span data-ttu-id="b840a-169">Egenskaper för datamängd</span><span class="sxs-lookup"><span data-stu-id="b840a-169">Dataset properties</span></span>
<span data-ttu-id="b840a-170">En fullständig lista över egenskaper som är tillgängliga för att definiera datauppsättningarna & avsnitt finns hello [skapa datauppsättningar](data-factory-create-datasets.md) artikel.</span><span class="sxs-lookup"><span data-stu-id="b840a-170">For a full list of sections & properties available for defining datasets, see hello [Creating datasets](data-factory-create-datasets.md) article.</span></span> <span data-ttu-id="b840a-171">Avsnitt som struktur, tillgänglighet och princip på en datamängd JSON är liknande för alla typer av datauppsättningen.</span><span class="sxs-lookup"><span data-stu-id="b840a-171">Sections such as structure, availability, and policy of a dataset JSON are similar for all dataset types.</span></span>

<span data-ttu-id="b840a-172">hello typeProperties avsnittet är olika för varje typ av dataset och ger information om hello platsen för hello data i datalagret hello.</span><span class="sxs-lookup"><span data-stu-id="b840a-172">hello typeProperties section is different for each type of dataset and provides information about hello location of hello data in hello data store.</span></span> <span data-ttu-id="b840a-173">Hej typeProperties avsnittet för dataset av typen **RelationalTable** (som omfattar PostgreSQL dataset) har hello följande egenskaper:</span><span class="sxs-lookup"><span data-stu-id="b840a-173">hello typeProperties section for dataset of type **RelationalTable** (which includes PostgreSQL dataset) has hello following properties:</span></span>

| <span data-ttu-id="b840a-174">Egenskap</span><span class="sxs-lookup"><span data-stu-id="b840a-174">Property</span></span> | <span data-ttu-id="b840a-175">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="b840a-175">Description</span></span> | <span data-ttu-id="b840a-176">Krävs</span><span class="sxs-lookup"><span data-stu-id="b840a-176">Required</span></span> |
| --- | --- | --- |
| <span data-ttu-id="b840a-177">tableName</span><span class="sxs-lookup"><span data-stu-id="b840a-177">tableName</span></span> |<span data-ttu-id="b840a-178">Namnet på hello tabell i hello PostgreSQL-databasinstansen som den länkade tjänsten refererar till.</span><span class="sxs-lookup"><span data-stu-id="b840a-178">Name of hello table in hello PostgreSQL Database instance that linked service refers to.</span></span> <span data-ttu-id="b840a-179">hello tableName är skiftlägeskänslig.</span><span class="sxs-lookup"><span data-stu-id="b840a-179">hello tableName is case-sensitive.</span></span> |<span data-ttu-id="b840a-180">Nej (om **frågan** av **RelationalSource** har angetts)</span><span class="sxs-lookup"><span data-stu-id="b840a-180">No (if **query** of **RelationalSource** is specified)</span></span> |

## <a name="copy-activity-properties"></a><span data-ttu-id="b840a-181">Kopiera egenskaper för aktivitet</span><span class="sxs-lookup"><span data-stu-id="b840a-181">Copy activity properties</span></span>
<span data-ttu-id="b840a-182">En fullständig lista över avsnitt & egenskaper som är tillgängliga för att definiera aktiviteter finns hello [skapar Pipelines](data-factory-create-pipelines.md) artikel.</span><span class="sxs-lookup"><span data-stu-id="b840a-182">For a full list of sections & properties available for defining activities, see hello [Creating Pipelines](data-factory-create-pipelines.md) article.</span></span> <span data-ttu-id="b840a-183">Egenskaper som namn, beskrivning, ingående och utgående tabeller och principen är tillgängliga för alla typer av aktiviteter.</span><span class="sxs-lookup"><span data-stu-id="b840a-183">Properties such as name, description, input and output tables, and policy are available for all types of activities.</span></span>

<span data-ttu-id="b840a-184">De egenskaper som är tillgängliga under hello typeProperties i hello aktivitet varierar beroende på varje aktivitetstyp.</span><span class="sxs-lookup"><span data-stu-id="b840a-184">Whereas, properties available in hello typeProperties section of hello activity vary with each activity type.</span></span> <span data-ttu-id="b840a-185">För Kopieringsaktivitet kan varierar de beroende på hello typer av datakällor och sänkor.</span><span class="sxs-lookup"><span data-stu-id="b840a-185">For Copy activity, they vary depending on hello types of sources and sinks.</span></span>

<span data-ttu-id="b840a-186">När datakällan är av typen **RelationalSource** (som omfattar PostgreSQL), hello följande egenskaper finns i avsnittet typeProperties:</span><span class="sxs-lookup"><span data-stu-id="b840a-186">When source is of type **RelationalSource** (which includes PostgreSQL), hello following properties are available in typeProperties section:</span></span>

| <span data-ttu-id="b840a-187">Egenskap</span><span class="sxs-lookup"><span data-stu-id="b840a-187">Property</span></span> | <span data-ttu-id="b840a-188">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="b840a-188">Description</span></span> | <span data-ttu-id="b840a-189">Tillåtna värden</span><span class="sxs-lookup"><span data-stu-id="b840a-189">Allowed values</span></span> | <span data-ttu-id="b840a-190">Krävs</span><span class="sxs-lookup"><span data-stu-id="b840a-190">Required</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="b840a-191">DocumentDB</span><span class="sxs-lookup"><span data-stu-id="b840a-191">query</span></span> |<span data-ttu-id="b840a-192">Använda hello anpassad fråga tooread data.</span><span class="sxs-lookup"><span data-stu-id="b840a-192">Use hello custom query tooread data.</span></span> |<span data-ttu-id="b840a-193">SQL-sträng.</span><span class="sxs-lookup"><span data-stu-id="b840a-193">SQL query string.</span></span> <span data-ttu-id="b840a-194">Till exempel: ”frågan” ”: Välj * från \"MySchema\".\" Mytable prefix\"”.</span><span class="sxs-lookup"><span data-stu-id="b840a-194">For example: "query": "select * from \"MySchema\".\"MyTable\"".</span></span> |<span data-ttu-id="b840a-195">Nej (om **tableName** av **dataset** har angetts)</span><span class="sxs-lookup"><span data-stu-id="b840a-195">No (if **tableName** of **dataset** is specified)</span></span> |

> [!NOTE]
> <span data-ttu-id="b840a-196">Schema och tabellnamn är skiftlägeskänsliga.</span><span class="sxs-lookup"><span data-stu-id="b840a-196">Schema and table names are case-sensitive.</span></span> <span data-ttu-id="b840a-197">Innesluts i `""` (dubbla citattecken) i hello-frågan.</span><span class="sxs-lookup"><span data-stu-id="b840a-197">Enclose them in `""` (double quotes) in hello query.</span></span>  

<span data-ttu-id="b840a-198">**Exempel:**</span><span class="sxs-lookup"><span data-stu-id="b840a-198">**Example:**</span></span>

 `"query": "select * from \"MySchema\".\"MyTable\""`

## <a name="json-example-copy-data-from-postgresql-tooazure-blob"></a><span data-ttu-id="b840a-199">JSON-exempel: kopiera data från PostgreSQL tooAzure Blob</span><span class="sxs-lookup"><span data-stu-id="b840a-199">JSON example: Copy data from PostgreSQL tooAzure Blob</span></span>
<span data-ttu-id="b840a-200">Det här exemplet innehåller exempel JSON definitioner som du kan använda toocreate en pipeline med hjälp av [Azure-portalen](data-factory-copy-activity-tutorial-using-azure-portal.md) eller [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md) eller [Azure PowerShell](data-factory-copy-activity-tutorial-using-powershell.md).</span><span class="sxs-lookup"><span data-stu-id="b840a-200">This example provides sample JSON definitions that you can use toocreate a pipeline by using [Azure portal](data-factory-copy-activity-tutorial-using-azure-portal.md) or [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md) or [Azure PowerShell](data-factory-copy-activity-tutorial-using-powershell.md).</span></span> <span data-ttu-id="b840a-201">De visar hur toocopy data från PostgreSQL-databas tooAzure Blob Storage.</span><span class="sxs-lookup"><span data-stu-id="b840a-201">They show how toocopy data from PostgreSQL database tooAzure Blob Storage.</span></span> <span data-ttu-id="b840a-202">Data kan dock vara kopierade tooany av hello sänkor anges [här](data-factory-data-movement-activities.md#supported-data-stores-and-formats) med hello Kopieringsaktiviteten i Azure Data Factory.</span><span class="sxs-lookup"><span data-stu-id="b840a-202">However, data can be copied tooany of hello sinks stated [here](data-factory-data-movement-activities.md#supported-data-stores-and-formats) using hello Copy Activity in Azure Data Factory.</span></span>   

> [!IMPORTANT]
> <span data-ttu-id="b840a-203">Det här exemplet innehåller JSON kodavsnitt.</span><span class="sxs-lookup"><span data-stu-id="b840a-203">This sample provides JSON snippets.</span></span> <span data-ttu-id="b840a-204">Stegvisa instruktioner för att skapa datafabriken hello inkluderas inte.</span><span class="sxs-lookup"><span data-stu-id="b840a-204">It does not include step-by-step instructions for creating hello data factory.</span></span> <span data-ttu-id="b840a-205">Se [flytta data mellan lokala platser och moln](data-factory-move-data-between-onprem-and-cloud.md) artikel stegvisa instruktioner.</span><span class="sxs-lookup"><span data-stu-id="b840a-205">See [moving data between on-premises locations and cloud](data-factory-move-data-between-onprem-and-cloud.md) article for step-by-step instructions.</span></span>

<span data-ttu-id="b840a-206">hello exemplet har hello följande data factory-enheter:</span><span class="sxs-lookup"><span data-stu-id="b840a-206">hello sample has hello following data factory entities:</span></span>

1. <span data-ttu-id="b840a-207">En länkad tjänst av typen [OnPremisesPostgreSql](data-factory-onprem-postgresql-connector.md#linked-service-properties).</span><span class="sxs-lookup"><span data-stu-id="b840a-207">A linked service of type [OnPremisesPostgreSql](data-factory-onprem-postgresql-connector.md#linked-service-properties).</span></span>
2. <span data-ttu-id="b840a-208">En länkad tjänst av typen [AzureStorage](data-factory-azure-blob-connector.md#linked-service-properties).</span><span class="sxs-lookup"><span data-stu-id="b840a-208">A linked service of type [AzureStorage](data-factory-azure-blob-connector.md#linked-service-properties).</span></span>
3. <span data-ttu-id="b840a-209">Indata [dataset](data-factory-create-datasets.md) av typen [RelationalTable](data-factory-onprem-postgresql-connector.md#dataset-properties).</span><span class="sxs-lookup"><span data-stu-id="b840a-209">An input [dataset](data-factory-create-datasets.md) of type [RelationalTable](data-factory-onprem-postgresql-connector.md#dataset-properties).</span></span>
4. <span data-ttu-id="b840a-210">Utdata [dataset](data-factory-create-datasets.md) av typen [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties).</span><span class="sxs-lookup"><span data-stu-id="b840a-210">An output [dataset](data-factory-create-datasets.md) of type [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties).</span></span>
5. <span data-ttu-id="b840a-211">Hej [pipeline](data-factory-create-pipelines.md) med Kopieringsaktiviteten som använder [RelationalSource](data-factory-onprem-postgresql-connector.md#copy-activity-properties) och [BlobSink](data-factory-azure-blob-connector.md#copy-activity-properties).</span><span class="sxs-lookup"><span data-stu-id="b840a-211">hello [pipeline](data-factory-create-pipelines.md) with Copy Activity that uses [RelationalSource](data-factory-onprem-postgresql-connector.md#copy-activity-properties) and [BlobSink](data-factory-azure-blob-connector.md#copy-activity-properties).</span></span>

<span data-ttu-id="b840a-212">hello exemplet kopierar data från ett frågeresultat i PostgreSQL-databas tooa blob varje timme.</span><span class="sxs-lookup"><span data-stu-id="b840a-212">hello sample copies data from a query result in PostgreSQL database tooa blob every hour.</span></span> <span data-ttu-id="b840a-213">hello JSON egenskaper som används i exemplen beskrivs i hello-exempel i följande avsnitt.</span><span class="sxs-lookup"><span data-stu-id="b840a-213">hello JSON properties used in these samples are described in sections following hello samples.</span></span>

<span data-ttu-id="b840a-214">Som ett första steg bör du konfigurera hello data management gateway.</span><span class="sxs-lookup"><span data-stu-id="b840a-214">As a first step, setup hello data management gateway.</span></span> <span data-ttu-id="b840a-215">hello anvisningar finns i hello [flytta data mellan lokala platser och moln](data-factory-move-data-between-onprem-and-cloud.md) artikel.</span><span class="sxs-lookup"><span data-stu-id="b840a-215">hello instructions are in hello [moving data between on-premises locations and cloud](data-factory-move-data-between-onprem-and-cloud.md) article.</span></span>

<span data-ttu-id="b840a-216">**PostgreSQL länkade tjänsten:**</span><span class="sxs-lookup"><span data-stu-id="b840a-216">**PostgreSQL linked service:**</span></span>

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
<span data-ttu-id="b840a-217">**Azure Blob storage länkade tjänsten:**</span><span class="sxs-lookup"><span data-stu-id="b840a-217">**Azure Blob storage linked service:**</span></span>

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
<span data-ttu-id="b840a-218">**PostgreSQL inkommande datauppsättningen:**</span><span class="sxs-lookup"><span data-stu-id="b840a-218">**PostgreSQL input dataset:**</span></span>

<span data-ttu-id="b840a-219">hello exemplet förutsätter att du har skapat en tabell ”mytable” som prefix i PostgreSQL och innehåller en kolumn med namnet ”tidsstämpel” för tid series-data.</span><span class="sxs-lookup"><span data-stu-id="b840a-219">hello sample assumes you have created a table “MyTable” in PostgreSQL and it contains a column called “timestamp” for time series data.</span></span>

<span data-ttu-id="b840a-220">Ange `"external": true` informerar hello Data Factory-tjänsten som hello dataset är externa toohello data factory och inte tillverkas av en aktivitet i hello data factory.</span><span class="sxs-lookup"><span data-stu-id="b840a-220">Setting `"external": true` informs hello Data Factory service that hello dataset is external toohello data factory and is not produced by an activity in hello data factory.</span></span>

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

<span data-ttu-id="b840a-221">**Azure Blob utdatauppsättningen:**</span><span class="sxs-lookup"><span data-stu-id="b840a-221">**Azure Blob output dataset:**</span></span>

<span data-ttu-id="b840a-222">Data skrivs tooa nya blob varje timme (frekvens: timme, intervall: 1).</span><span class="sxs-lookup"><span data-stu-id="b840a-222">Data is written tooa new blob every hour (frequency: hour, interval: 1).</span></span> <span data-ttu-id="b840a-223">hello mappen sökvägen och filnamnet för hello blob utvärderas dynamiskt baserat på hello starttiden för hello-segment som bearbetas.</span><span class="sxs-lookup"><span data-stu-id="b840a-223">hello folder path and file name for hello blob are dynamically evaluated based on hello start time of hello slice that is being processed.</span></span> <span data-ttu-id="b840a-224">hello mappsökväg använder år, månad, dag och timmar delar av hello starttid.</span><span class="sxs-lookup"><span data-stu-id="b840a-224">hello folder path uses year, month, day, and hours parts of hello start time.</span></span>

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

<span data-ttu-id="b840a-225">**Pipeline med kopieringsaktiviteten:**</span><span class="sxs-lookup"><span data-stu-id="b840a-225">**Pipeline with Copy activity:**</span></span>

<span data-ttu-id="b840a-226">hello pipelinen innehåller en kopia-aktivitet som är konfigurerade toouse hello inkommande och utgående datauppsättningar och är schemalagda toorun varje timme.</span><span class="sxs-lookup"><span data-stu-id="b840a-226">hello pipeline contains a Copy Activity that is configured toouse hello input and output datasets and is scheduled toorun hourly.</span></span> <span data-ttu-id="b840a-227">I hello pipeline JSON-definitionen hello **källa** typ har angetts för**RelationalSource** och **sink** typ har angetts för**BlobSink**.</span><span class="sxs-lookup"><span data-stu-id="b840a-227">In hello pipeline JSON definition, hello **source** type is set too**RelationalSource** and **sink** type is set too**BlobSink**.</span></span> <span data-ttu-id="b840a-228">hello SQL-frågan som angetts för hello **frågan** egenskapen väljer hello data från hello public.usstates tabell i hello PostgreSQL-databas.</span><span class="sxs-lookup"><span data-stu-id="b840a-228">hello SQL query specified for hello **query** property selects hello data from hello public.usstates table in hello PostgreSQL database.</span></span>

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
## <a name="type-mapping-for-postgresql"></a><span data-ttu-id="b840a-229">Mappning för PostgreSQL</span><span class="sxs-lookup"><span data-stu-id="b840a-229">Type mapping for PostgreSQL</span></span>
<span data-ttu-id="b840a-230">Som anges i hello [data movement aktiviteter](data-factory-data-movement-activities.md) artikel kopieringsaktiviteten utför automatisk konverteringar från källan typer toosink typer med hello följande metod i steg 2:</span><span class="sxs-lookup"><span data-stu-id="b840a-230">As mentioned in hello [data movement activities](data-factory-data-movement-activities.md) article Copy activity performs automatic type conversions from source types toosink types with hello following 2-step approach:</span></span>

1. <span data-ttu-id="b840a-231">Konvertera från inbyggda typer too.NET källtypen</span><span class="sxs-lookup"><span data-stu-id="b840a-231">Convert from native source types too.NET type</span></span>
2. <span data-ttu-id="b840a-232">Konvertera från .NET typen toonative Mottagartypen</span><span class="sxs-lookup"><span data-stu-id="b840a-232">Convert from .NET type toonative sink type</span></span>

<span data-ttu-id="b840a-233">När du flyttar data tooPostgreSQL som hello följande mappningar används från PostgreSQL typ too.NET.</span><span class="sxs-lookup"><span data-stu-id="b840a-233">When moving data tooPostgreSQL, hello following mappings are used from PostgreSQL type too.NET type.</span></span>

| <span data-ttu-id="b840a-234">Typ av PostgreSQL-databas</span><span class="sxs-lookup"><span data-stu-id="b840a-234">PostgreSQL Database type</span></span> | <span data-ttu-id="b840a-235">PostgresSQL alias</span><span class="sxs-lookup"><span data-stu-id="b840a-235">PostgresSQL aliases</span></span> | <span data-ttu-id="b840a-236">.NET framework-typ</span><span class="sxs-lookup"><span data-stu-id="b840a-236">.NET Framework type</span></span> |
| --- | --- | --- |
| <span data-ttu-id="b840a-237">abstime</span><span class="sxs-lookup"><span data-stu-id="b840a-237">abstime</span></span> | |<span data-ttu-id="b840a-238">Datum och tid</span><span class="sxs-lookup"><span data-stu-id="b840a-238">Datetime</span></span> | &nbsp;
| <span data-ttu-id="b840a-239">bigint</span><span class="sxs-lookup"><span data-stu-id="b840a-239">bigint</span></span> |<span data-ttu-id="b840a-240">int8</span><span class="sxs-lookup"><span data-stu-id="b840a-240">int8</span></span> |<span data-ttu-id="b840a-241">Int64</span><span class="sxs-lookup"><span data-stu-id="b840a-241">Int64</span></span> |
| <span data-ttu-id="b840a-242">bigserial</span><span class="sxs-lookup"><span data-stu-id="b840a-242">bigserial</span></span> |<span data-ttu-id="b840a-243">serial8</span><span class="sxs-lookup"><span data-stu-id="b840a-243">serial8</span></span> |<span data-ttu-id="b840a-244">Int64</span><span class="sxs-lookup"><span data-stu-id="b840a-244">Int64</span></span> |
| <span data-ttu-id="b840a-245">bitars [(n)]</span><span class="sxs-lookup"><span data-stu-id="b840a-245">bit [ (n) ]</span></span> | |<span data-ttu-id="b840a-246">Byte [], sträng</span><span class="sxs-lookup"><span data-stu-id="b840a-246">Byte[], String</span></span> | &nbsp;
| <span data-ttu-id="b840a-247">bit varierande [(n)]</span><span class="sxs-lookup"><span data-stu-id="b840a-247">bit varying [ (n) ]</span></span> |<span data-ttu-id="b840a-248">varbit</span><span class="sxs-lookup"><span data-stu-id="b840a-248">varbit</span></span> |<span data-ttu-id="b840a-249">Byte [], sträng</span><span class="sxs-lookup"><span data-stu-id="b840a-249">Byte[], String</span></span> |
| <span data-ttu-id="b840a-250">Booleskt värde</span><span class="sxs-lookup"><span data-stu-id="b840a-250">boolean</span></span> |<span data-ttu-id="b840a-251">bool</span><span class="sxs-lookup"><span data-stu-id="b840a-251">bool</span></span> |<span data-ttu-id="b840a-252">Booleskt värde</span><span class="sxs-lookup"><span data-stu-id="b840a-252">Boolean</span></span> |
| <span data-ttu-id="b840a-253">Rutan</span><span class="sxs-lookup"><span data-stu-id="b840a-253">box</span></span> | |<span data-ttu-id="b840a-254">Byte [], sträng</span><span class="sxs-lookup"><span data-stu-id="b840a-254">Byte[], String</span></span> |&nbsp;
| <span data-ttu-id="b840a-255">bytea</span><span class="sxs-lookup"><span data-stu-id="b840a-255">bytea</span></span> | |<span data-ttu-id="b840a-256">Byte [], sträng</span><span class="sxs-lookup"><span data-stu-id="b840a-256">Byte[], String</span></span> |&nbsp;
| <span data-ttu-id="b840a-257">tecknet [(n)]</span><span class="sxs-lookup"><span data-stu-id="b840a-257">character [ (n) ]</span></span> |<span data-ttu-id="b840a-258">char [(n)]</span><span class="sxs-lookup"><span data-stu-id="b840a-258">char [ (n) ]</span></span> |<span data-ttu-id="b840a-259">Sträng</span><span class="sxs-lookup"><span data-stu-id="b840a-259">String</span></span> |
| <span data-ttu-id="b840a-260">tecknet varierande [(n)]</span><span class="sxs-lookup"><span data-stu-id="b840a-260">character varying [ (n) ]</span></span> |<span data-ttu-id="b840a-261">varchar [(n)]</span><span class="sxs-lookup"><span data-stu-id="b840a-261">varchar [ (n) ]</span></span> |<span data-ttu-id="b840a-262">Sträng</span><span class="sxs-lookup"><span data-stu-id="b840a-262">String</span></span> |
| <span data-ttu-id="b840a-263">CID</span><span class="sxs-lookup"><span data-stu-id="b840a-263">cid</span></span> | |<span data-ttu-id="b840a-264">Sträng</span><span class="sxs-lookup"><span data-stu-id="b840a-264">String</span></span> |&nbsp;
| <span data-ttu-id="b840a-265">CIDR</span><span class="sxs-lookup"><span data-stu-id="b840a-265">cidr</span></span> | |<span data-ttu-id="b840a-266">Sträng</span><span class="sxs-lookup"><span data-stu-id="b840a-266">String</span></span> |&nbsp;
| <span data-ttu-id="b840a-267">cirkel</span><span class="sxs-lookup"><span data-stu-id="b840a-267">circle</span></span> | |<span data-ttu-id="b840a-268">Byte [], sträng</span><span class="sxs-lookup"><span data-stu-id="b840a-268">Byte[], String</span></span> |&nbsp;
| <span data-ttu-id="b840a-269">Datum</span><span class="sxs-lookup"><span data-stu-id="b840a-269">date</span></span> | |<span data-ttu-id="b840a-270">Datum och tid</span><span class="sxs-lookup"><span data-stu-id="b840a-270">Datetime</span></span> |&nbsp;
| <span data-ttu-id="b840a-271">DateRange</span><span class="sxs-lookup"><span data-stu-id="b840a-271">daterange</span></span> | |<span data-ttu-id="b840a-272">Sträng</span><span class="sxs-lookup"><span data-stu-id="b840a-272">String</span></span> |&nbsp;
| <span data-ttu-id="b840a-273">dubbel precision</span><span class="sxs-lookup"><span data-stu-id="b840a-273">double precision</span></span> |<span data-ttu-id="b840a-274">FLOAT8</span><span class="sxs-lookup"><span data-stu-id="b840a-274">float8</span></span> |<span data-ttu-id="b840a-275">dubbla</span><span class="sxs-lookup"><span data-stu-id="b840a-275">Double</span></span> |
| <span data-ttu-id="b840a-276">inet</span><span class="sxs-lookup"><span data-stu-id="b840a-276">inet</span></span> | |<span data-ttu-id="b840a-277">Byte [], sträng</span><span class="sxs-lookup"><span data-stu-id="b840a-277">Byte[], String</span></span> |&nbsp;
| <span data-ttu-id="b840a-278">intarry</span><span class="sxs-lookup"><span data-stu-id="b840a-278">intarry</span></span> | |<span data-ttu-id="b840a-279">Sträng</span><span class="sxs-lookup"><span data-stu-id="b840a-279">String</span></span> |&nbsp;
| <span data-ttu-id="b840a-280">int4range</span><span class="sxs-lookup"><span data-stu-id="b840a-280">int4range</span></span> | |<span data-ttu-id="b840a-281">Sträng</span><span class="sxs-lookup"><span data-stu-id="b840a-281">String</span></span> |&nbsp;
| <span data-ttu-id="b840a-282">int8range</span><span class="sxs-lookup"><span data-stu-id="b840a-282">int8range</span></span> | |<span data-ttu-id="b840a-283">Sträng</span><span class="sxs-lookup"><span data-stu-id="b840a-283">String</span></span> |&nbsp;
| <span data-ttu-id="b840a-284">heltal</span><span class="sxs-lookup"><span data-stu-id="b840a-284">integer</span></span> |<span data-ttu-id="b840a-285">int, int4</span><span class="sxs-lookup"><span data-stu-id="b840a-285">int, int4</span></span> |<span data-ttu-id="b840a-286">Int32</span><span class="sxs-lookup"><span data-stu-id="b840a-286">Int32</span></span> |
| <span data-ttu-id="b840a-287">intervallet [fält] [(p)]</span><span class="sxs-lookup"><span data-stu-id="b840a-287">interval [ fields ] [ (p) ]</span></span> | |<span data-ttu-id="b840a-288">Tidsintervall</span><span class="sxs-lookup"><span data-stu-id="b840a-288">Timespan</span></span> |&nbsp;
| <span data-ttu-id="b840a-289">JSON</span><span class="sxs-lookup"><span data-stu-id="b840a-289">json</span></span> | |<span data-ttu-id="b840a-290">Sträng</span><span class="sxs-lookup"><span data-stu-id="b840a-290">String</span></span> |&nbsp;
| <span data-ttu-id="b840a-291">jsonb</span><span class="sxs-lookup"><span data-stu-id="b840a-291">jsonb</span></span> | |<span data-ttu-id="b840a-292">byte]</span><span class="sxs-lookup"><span data-stu-id="b840a-292">Byte[]</span></span> |&nbsp;
| <span data-ttu-id="b840a-293">raden</span><span class="sxs-lookup"><span data-stu-id="b840a-293">line</span></span> | |<span data-ttu-id="b840a-294">Byte [], sträng</span><span class="sxs-lookup"><span data-stu-id="b840a-294">Byte[], String</span></span> |&nbsp;
| <span data-ttu-id="b840a-295">lseg</span><span class="sxs-lookup"><span data-stu-id="b840a-295">lseg</span></span> | |<span data-ttu-id="b840a-296">Byte [], sträng</span><span class="sxs-lookup"><span data-stu-id="b840a-296">Byte[], String</span></span> |&nbsp;
| <span data-ttu-id="b840a-297">macaddr</span><span class="sxs-lookup"><span data-stu-id="b840a-297">macaddr</span></span> | |<span data-ttu-id="b840a-298">Byte [], sträng</span><span class="sxs-lookup"><span data-stu-id="b840a-298">Byte[], String</span></span> |&nbsp;
| <span data-ttu-id="b840a-299">Money</span><span class="sxs-lookup"><span data-stu-id="b840a-299">money</span></span> | |<span data-ttu-id="b840a-300">Decimal</span><span class="sxs-lookup"><span data-stu-id="b840a-300">Decimal</span></span> |&nbsp;
| <span data-ttu-id="b840a-301">numeriska [(p, s)]</span><span class="sxs-lookup"><span data-stu-id="b840a-301">numeric [ (p, s) ]</span></span> |<span data-ttu-id="b840a-302">decimal [(p, s)]</span><span class="sxs-lookup"><span data-stu-id="b840a-302">decimal [ (p, s) ]</span></span> |<span data-ttu-id="b840a-303">Decimal</span><span class="sxs-lookup"><span data-stu-id="b840a-303">Decimal</span></span> |
| <span data-ttu-id="b840a-304">numrange</span><span class="sxs-lookup"><span data-stu-id="b840a-304">numrange</span></span> | |<span data-ttu-id="b840a-305">Sträng</span><span class="sxs-lookup"><span data-stu-id="b840a-305">String</span></span> |&nbsp;
| <span data-ttu-id="b840a-306">OID</span><span class="sxs-lookup"><span data-stu-id="b840a-306">oid</span></span> | |<span data-ttu-id="b840a-307">Int32</span><span class="sxs-lookup"><span data-stu-id="b840a-307">Int32</span></span> |&nbsp;
| <span data-ttu-id="b840a-308">Sökväg</span><span class="sxs-lookup"><span data-stu-id="b840a-308">path</span></span> | |<span data-ttu-id="b840a-309">Byte [], sträng</span><span class="sxs-lookup"><span data-stu-id="b840a-309">Byte[], String</span></span> |&nbsp;
| <span data-ttu-id="b840a-310">pg_lsn</span><span class="sxs-lookup"><span data-stu-id="b840a-310">pg_lsn</span></span> | |<span data-ttu-id="b840a-311">Int64</span><span class="sxs-lookup"><span data-stu-id="b840a-311">Int64</span></span> |&nbsp;
| <span data-ttu-id="b840a-312">punkt</span><span class="sxs-lookup"><span data-stu-id="b840a-312">point</span></span> | |<span data-ttu-id="b840a-313">Byte [], sträng</span><span class="sxs-lookup"><span data-stu-id="b840a-313">Byte[], String</span></span> |&nbsp;
| <span data-ttu-id="b840a-314">polygon</span><span class="sxs-lookup"><span data-stu-id="b840a-314">polygon</span></span> | |<span data-ttu-id="b840a-315">Byte [], sträng</span><span class="sxs-lookup"><span data-stu-id="b840a-315">Byte[], String</span></span> |&nbsp;
| <span data-ttu-id="b840a-316">Verklig</span><span class="sxs-lookup"><span data-stu-id="b840a-316">real</span></span> |<span data-ttu-id="b840a-317">FLOAT4</span><span class="sxs-lookup"><span data-stu-id="b840a-317">float4</span></span> |<span data-ttu-id="b840a-318">Enskild</span><span class="sxs-lookup"><span data-stu-id="b840a-318">Single</span></span> |
| <span data-ttu-id="b840a-319">smallint</span><span class="sxs-lookup"><span data-stu-id="b840a-319">smallint</span></span> |<span data-ttu-id="b840a-320">int2</span><span class="sxs-lookup"><span data-stu-id="b840a-320">int2</span></span> |<span data-ttu-id="b840a-321">Int16</span><span class="sxs-lookup"><span data-stu-id="b840a-321">Int16</span></span> |
| <span data-ttu-id="b840a-322">smallserial</span><span class="sxs-lookup"><span data-stu-id="b840a-322">smallserial</span></span> |<span data-ttu-id="b840a-323">serial2</span><span class="sxs-lookup"><span data-stu-id="b840a-323">serial2</span></span> |<span data-ttu-id="b840a-324">Int16</span><span class="sxs-lookup"><span data-stu-id="b840a-324">Int16</span></span> |
| <span data-ttu-id="b840a-325">Seriell</span><span class="sxs-lookup"><span data-stu-id="b840a-325">serial</span></span> |<span data-ttu-id="b840a-326">serial4</span><span class="sxs-lookup"><span data-stu-id="b840a-326">serial4</span></span> |<span data-ttu-id="b840a-327">Int32</span><span class="sxs-lookup"><span data-stu-id="b840a-327">Int32</span></span> |
| <span data-ttu-id="b840a-328">Text</span><span class="sxs-lookup"><span data-stu-id="b840a-328">text</span></span> | |<span data-ttu-id="b840a-329">Sträng</span><span class="sxs-lookup"><span data-stu-id="b840a-329">String</span></span> |&nbsp;

## <a name="map-source-toosink-columns"></a><span data-ttu-id="b840a-330">Mappa källkolumner toosink</span><span class="sxs-lookup"><span data-stu-id="b840a-330">Map source toosink columns</span></span>
<span data-ttu-id="b840a-331">toolearn mappning tabellkolumner i källan dataset toocolumns i sink dataset finns [mappa dataset kolumner i Azure Data Factory](data-factory-map-columns.md).</span><span class="sxs-lookup"><span data-stu-id="b840a-331">toolearn about mapping columns in source dataset toocolumns in sink dataset, see [Mapping dataset columns in Azure Data Factory](data-factory-map-columns.md).</span></span>

## <a name="repeatable-read-from-relational-sources"></a><span data-ttu-id="b840a-332">Upprepbar läsning från relationella källor</span><span class="sxs-lookup"><span data-stu-id="b840a-332">Repeatable read from relational sources</span></span>
<span data-ttu-id="b840a-333">När du kopierar data från relationella datalager, Kom ihåg tooavoid repeterbarhet oönskade resultat.</span><span class="sxs-lookup"><span data-stu-id="b840a-333">When copying data from relational data stores, keep repeatability in mind tooavoid unintended outcomes.</span></span> <span data-ttu-id="b840a-334">I Azure Data Factory, kan du köra en sektor manuellt.</span><span class="sxs-lookup"><span data-stu-id="b840a-334">In Azure Data Factory, you can rerun a slice manually.</span></span> <span data-ttu-id="b840a-335">Du kan också konfigurera i principen för en dataset så att ett segment som körs när ett fel uppstår.</span><span class="sxs-lookup"><span data-stu-id="b840a-335">You can also configure retry policy for a dataset so that a slice is rerun when a failure occurs.</span></span> <span data-ttu-id="b840a-336">När ett segment körs på antingen sätt måste toomake att som hello samma data läses oavsett hur många gånger ett segment körs.</span><span class="sxs-lookup"><span data-stu-id="b840a-336">When a slice is rerun in either way, you need toomake sure that hello same data is read no matter how many times a slice is run.</span></span> <span data-ttu-id="b840a-337">Se [Repeatable läsa från relationella källor](data-factory-repeatable-copy.md#repeatable-read-from-relational-sources).</span><span class="sxs-lookup"><span data-stu-id="b840a-337">See [Repeatable read from relational sources](data-factory-repeatable-copy.md#repeatable-read-from-relational-sources).</span></span>

## <a name="performance-and-tuning"></a><span data-ttu-id="b840a-338">Prestanda och finjustering</span><span class="sxs-lookup"><span data-stu-id="b840a-338">Performance and Tuning</span></span>
<span data-ttu-id="b840a-339">Se [kopiera aktivitet prestanda och justera guiden](data-factory-copy-activity-performance.md) toolearn om nyckeln faktorer som påverkan prestanda för flytt av data (Kopieringsaktiviteten) i Azure Data Factory och olika sätt toooptimize den.</span><span class="sxs-lookup"><span data-stu-id="b840a-339">See [Copy Activity Performance & Tuning Guide](data-factory-copy-activity-performance.md) toolearn about key factors that impact performance of data movement (Copy Activity) in Azure Data Factory and various ways toooptimize it.</span></span>
