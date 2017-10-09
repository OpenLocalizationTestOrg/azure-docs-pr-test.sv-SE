---
title: "aaaMove data från Cassandra med hjälp av Data Factory | Microsoft Docs"
description: "Läs mer om hur toomove data från en lokal Cassandra databasen med Azure Data Factory."
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
ms.openlocfilehash: 0e265d3a8439d0a2cb2a5c32e5ea8348a1617621
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="move-data-from-an-on-premises-cassandra-database-using-azure-data-factory"></a><span data-ttu-id="3ed1e-103">Flytta data från en lokal Cassandra-databas med Azure Data Factory</span><span class="sxs-lookup"><span data-stu-id="3ed1e-103">Move data from an on-premises Cassandra database using Azure Data Factory</span></span>
<span data-ttu-id="3ed1e-104">Den här artikeln förklarar hur toouse hello Kopieringsaktiviteten i Azure Data Factory toomove data från en lokal Cassandra databas.</span><span class="sxs-lookup"><span data-stu-id="3ed1e-104">This article explains how toouse hello Copy Activity in Azure Data Factory toomove data from an on-premises Cassandra database.</span></span> <span data-ttu-id="3ed1e-105">Den bygger på hello [Data Movement aktiviteter](data-factory-data-movement-activities.md) artikel som ger en allmän översikt över dataflyttning hello kopieringsaktiviteten.</span><span class="sxs-lookup"><span data-stu-id="3ed1e-105">It builds on hello [Data Movement Activities](data-factory-data-movement-activities.md) article, which presents a general overview of data movement with hello copy activity.</span></span>

<span data-ttu-id="3ed1e-106">Du kan kopiera data från ett lokalt Cassandra data store tooany stöds sink dataarkiv.</span><span class="sxs-lookup"><span data-stu-id="3ed1e-106">You can copy data from an on-premises Cassandra data store tooany supported sink data store.</span></span> <span data-ttu-id="3ed1e-107">En lista över data lagras som stöds när egenskaperna av hello kopieringsaktiviteten Se hello [stöds datalager](data-factory-data-movement-activities.md#supported-data-stores-and-formats) tabell.</span><span class="sxs-lookup"><span data-stu-id="3ed1e-107">For a list of data stores supported as sinks by hello copy activity, see hello [Supported data stores](data-factory-data-movement-activities.md#supported-data-stores-and-formats) table.</span></span> <span data-ttu-id="3ed1e-108">Data factory stöder för närvarande endast flytta data från en Cassandra lagra tooother datalager, men inte för att flytta data från andra lagrar tooa Cassandra data dataarkiv.</span><span class="sxs-lookup"><span data-stu-id="3ed1e-108">Data factory currently supports only moving data from a Cassandra data store tooother data stores, but not for moving data from other data stores tooa Cassandra data store.</span></span> 

## <a name="supported-versions"></a><span data-ttu-id="3ed1e-109">Versioner som stöds</span><span class="sxs-lookup"><span data-stu-id="3ed1e-109">Supported versions</span></span>
<span data-ttu-id="3ed1e-110">Hej Cassandra connector stöder följande versioner av Cassandra hello: 2.X.</span><span class="sxs-lookup"><span data-stu-id="3ed1e-110">hello Cassandra connector supports hello following versions of Cassandra: 2.X.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="3ed1e-111">Krav</span><span class="sxs-lookup"><span data-stu-id="3ed1e-111">Prerequisites</span></span>
<span data-ttu-id="3ed1e-112">Hello Azure Data Factory-tjänsten toobe kan tooconnect tooyour lokalt Cassandra-databas måste du installera en Data Management Gateway på hello datorn samma värdar hello databasen eller på en separat dator tooavoid konkurrerar om resurser med hello databas.</span><span class="sxs-lookup"><span data-stu-id="3ed1e-112">For hello Azure Data Factory service toobe able tooconnect tooyour on-premises Cassandra database, you must install a Data Management Gateway on hello same machine that hosts hello database or on a separate machine tooavoid competing for resources with hello database.</span></span> <span data-ttu-id="3ed1e-113">Data Management Gateway är en komponent som ansluter lokala data sources toocloud tjänster i en säker och hanterad sätt.</span><span class="sxs-lookup"><span data-stu-id="3ed1e-113">Data Management Gateway is a component that connects on-premises data sources toocloud services in a secure and managed way.</span></span> <span data-ttu-id="3ed1e-114">Se [Data Management Gateway](data-factory-data-management-gateway.md) artikeln för information om Data Management Gateway.</span><span class="sxs-lookup"><span data-stu-id="3ed1e-114">See [Data Management Gateway](data-factory-data-management-gateway.md) article for details about Data Management Gateway.</span></span> <span data-ttu-id="3ed1e-115">Se [flytta data från lokala toocloud](data-factory-move-data-between-onprem-and-cloud.md) artikel stegvisa instruktioner om hur du konfigurerar hello gateway data pipeline toomove data.</span><span class="sxs-lookup"><span data-stu-id="3ed1e-115">See [Move data from on-premises toocloud](data-factory-move-data-between-onprem-and-cloud.md) article for step-by-step instructions on setting up hello gateway a data pipeline toomove data.</span></span>

<span data-ttu-id="3ed1e-116">Du måste använda hello gateway tooconnect tooa Cassandra databasen även om hello-databasen finns i hello molnet, till exempel på en Azure IaaS-VM.</span><span class="sxs-lookup"><span data-stu-id="3ed1e-116">You must use hello gateway tooconnect tooa Cassandra database even if hello database is hosted in hello cloud, for example, on an Azure IaaS VM.</span></span> <span data-ttu-id="3ed1e-117">Y kan innehålla hello gateway hello samma Virtuella värdar hello databasen eller på en separat virtuell dator så länge som hello gateway kan ansluta toohello databas.</span><span class="sxs-lookup"><span data-stu-id="3ed1e-117">Y You can have hello gateway on hello same VM that hosts hello database or on a separate VM as long as hello gateway can connect toohello database.</span></span>  

<span data-ttu-id="3ed1e-118">När du installerar hello gateway installeras automatiskt en Microsoft Cassandra ODBC-drivrutinen används tooconnect tooCassandra databas.</span><span class="sxs-lookup"><span data-stu-id="3ed1e-118">When you install hello gateway, it automatically installs a Microsoft Cassandra ODBC driver used tooconnect tooCassandra database.</span></span> <span data-ttu-id="3ed1e-119">Därför behöver du inte toomanually installera en drivrutin på hello gateway-datorn när du kopierar data från hello Cassandra databas.</span><span class="sxs-lookup"><span data-stu-id="3ed1e-119">Therefore, you don't need toomanually install any driver on hello gateway machine when copying data from hello Cassandra database.</span></span> 

> [!NOTE]
> <span data-ttu-id="3ed1e-120">Se [felsökning av problem med gateway](data-factory-data-management-gateway.md#troubleshooting-gateway-issues) tips om hur du felsöker anslutning /-gateway relaterade problem.</span><span class="sxs-lookup"><span data-stu-id="3ed1e-120">See [Troubleshoot gateway issues](data-factory-data-management-gateway.md#troubleshooting-gateway-issues) for tips on troubleshooting connection/gateway related issues.</span></span>

## <a name="getting-started"></a><span data-ttu-id="3ed1e-121">Komma igång</span><span class="sxs-lookup"><span data-stu-id="3ed1e-121">Getting started</span></span>
<span data-ttu-id="3ed1e-122">Du kan skapa en pipeline med en kopia-aktivitet som flyttar data från en lokal Cassandra data store med hjälp av olika verktyg/API: er.</span><span class="sxs-lookup"><span data-stu-id="3ed1e-122">You can create a pipeline with a copy activity that moves data from an on-premises Cassandra data store by using different tools/APIs.</span></span> 

- <span data-ttu-id="3ed1e-123">hello enklaste sättet toocreate en pipeline är toouse hello **guiden Kopiera**.</span><span class="sxs-lookup"><span data-stu-id="3ed1e-123">hello easiest way toocreate a pipeline is toouse hello **Copy Wizard**.</span></span> <span data-ttu-id="3ed1e-124">Se [Självstudier: skapa en pipeline med hjälp av guiden Kopiera](data-factory-copy-data-wizard-tutorial.md) för en snabb genomgång om hur du skapar en pipeline med hjälp av guiden för hello kopiera data.</span><span class="sxs-lookup"><span data-stu-id="3ed1e-124">See [Tutorial: Create a pipeline using Copy Wizard](data-factory-copy-data-wizard-tutorial.md) for a quick walkthrough on creating a pipeline using hello Copy data wizard.</span></span> 
- <span data-ttu-id="3ed1e-125">Du kan också använda följande verktyg toocreate en pipeline hello: **Azure-portalen**, **Visual Studio**, **Azure PowerShell**, **Azure Resource Manager-mall** , **.NET API**, och **REST API**.</span><span class="sxs-lookup"><span data-stu-id="3ed1e-125">You can also use hello following tools toocreate a pipeline: **Azure portal**, **Visual Studio**, **Azure PowerShell**, **Azure Resource Manager template**, **.NET API**, and **REST API**.</span></span> <span data-ttu-id="3ed1e-126">Se [kopiera aktivitet kursen](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) för stegvisa instruktioner toocreate en pipeline med en Kopieringsaktivitet.</span><span class="sxs-lookup"><span data-stu-id="3ed1e-126">See [Copy activity tutorial](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) for step-by-step instructions toocreate a pipeline with a copy activity.</span></span> 

<span data-ttu-id="3ed1e-127">Om du använder hello verktyg eller API: er kan utföra du hello följande steg toocreate en pipeline som flyttar data från en källdata lagra tooa sink-datalagret:</span><span class="sxs-lookup"><span data-stu-id="3ed1e-127">Whether you use hello tools or APIs, you perform hello following steps toocreate a pipeline that moves data from a source data store tooa sink data store:</span></span>

1. <span data-ttu-id="3ed1e-128">Skapa **länkade tjänster** toolink indata och utdata lagrar tooyour data factory.</span><span class="sxs-lookup"><span data-stu-id="3ed1e-128">Create **linked services** toolink input and output data stores tooyour data factory.</span></span>
2. <span data-ttu-id="3ed1e-129">Skapa **datauppsättningar** toorepresent indata och utdata för hello kopieringsåtgärden.</span><span class="sxs-lookup"><span data-stu-id="3ed1e-129">Create **datasets** toorepresent input and output data for hello copy operation.</span></span> 
3. <span data-ttu-id="3ed1e-130">Skapa en **pipeline** med en kopia-aktivitet som tar en datamängd som indata och en dataset som utdata.</span><span class="sxs-lookup"><span data-stu-id="3ed1e-130">Create a **pipeline** with a copy activity that takes a dataset as an input and a dataset as an output.</span></span> 

<span data-ttu-id="3ed1e-131">När du använder guiden hello skapas automatiskt JSON definitioner för dessa Data Factory-enheter (länkade tjänster, datauppsättningar och hello pipeline) för dig.</span><span class="sxs-lookup"><span data-stu-id="3ed1e-131">When you use hello wizard, JSON definitions for these Data Factory entities (linked services, datasets, and hello pipeline) are automatically created for you.</span></span> <span data-ttu-id="3ed1e-132">När du använder Verktyg/API: er (utom .NET API), kan du definiera dessa Data Factory-enheter med hjälp av hello JSON-format.</span><span class="sxs-lookup"><span data-stu-id="3ed1e-132">When you use tools/APIs (except .NET API), you define these Data Factory entities by using hello JSON format.</span></span>  <span data-ttu-id="3ed1e-133">Ett exempel med JSON-definitioner för Data Factory-entiteter som ska använda toocopy data från ett lokalt Cassandra dataarkiv finns [JSON-exempel: kopiera data från Cassandra tooAzure Blob](#json-example-copy-data-from-cassandra-to-azure-blob) i den här artikeln.</span><span class="sxs-lookup"><span data-stu-id="3ed1e-133">For a sample with JSON definitions for Data Factory entities that are used toocopy data from an on-premises Cassandra data store, see [JSON example: Copy data from Cassandra tooAzure Blob](#json-example-copy-data-from-cassandra-to-azure-blob) section of this article.</span></span> 

<span data-ttu-id="3ed1e-134">hello följande avsnitt innehåller information om JSON-egenskaper används toodefine Data Factory entiteter specifika tooa Cassandra datalager:</span><span class="sxs-lookup"><span data-stu-id="3ed1e-134">hello following sections provide details about JSON properties that are used toodefine Data Factory entities specific tooa Cassandra data store:</span></span>

## <a name="linked-service-properties"></a><span data-ttu-id="3ed1e-135">Länkad tjänstegenskaper</span><span class="sxs-lookup"><span data-stu-id="3ed1e-135">Linked service properties</span></span>
<span data-ttu-id="3ed1e-136">hello följande tabell innehåller en beskrivning för JSON-element specifika tooCassandra länkad tjänst.</span><span class="sxs-lookup"><span data-stu-id="3ed1e-136">hello following table provides description for JSON elements specific tooCassandra linked service.</span></span>

| <span data-ttu-id="3ed1e-137">Egenskap</span><span class="sxs-lookup"><span data-stu-id="3ed1e-137">Property</span></span> | <span data-ttu-id="3ed1e-138">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="3ed1e-138">Description</span></span> | <span data-ttu-id="3ed1e-139">Krävs</span><span class="sxs-lookup"><span data-stu-id="3ed1e-139">Required</span></span> |
| --- | --- | --- |
| <span data-ttu-id="3ed1e-140">typ</span><span class="sxs-lookup"><span data-stu-id="3ed1e-140">type</span></span> |<span data-ttu-id="3ed1e-141">hello Typegenskapen måste anges till: **OnPremisesCassandra**</span><span class="sxs-lookup"><span data-stu-id="3ed1e-141">hello type property must be set to: **OnPremisesCassandra**</span></span> |<span data-ttu-id="3ed1e-142">Ja</span><span class="sxs-lookup"><span data-stu-id="3ed1e-142">Yes</span></span> |
| <span data-ttu-id="3ed1e-143">värden</span><span class="sxs-lookup"><span data-stu-id="3ed1e-143">host</span></span> |<span data-ttu-id="3ed1e-144">En eller flera IP-adresser eller värdnamn Cassandra servrar.</span><span class="sxs-lookup"><span data-stu-id="3ed1e-144">One or more IP addresses or host names of Cassandra servers.</span></span><br/><br/><span data-ttu-id="3ed1e-145">Ange en kommaavgränsad lista med IP-adresser eller tooall värdservrar namn tooconnect samtidigt.</span><span class="sxs-lookup"><span data-stu-id="3ed1e-145">Specify a comma-separated list of IP addresses or host names tooconnect tooall servers concurrently.</span></span> |<span data-ttu-id="3ed1e-146">Ja</span><span class="sxs-lookup"><span data-stu-id="3ed1e-146">Yes</span></span> |
| <span data-ttu-id="3ed1e-147">port</span><span class="sxs-lookup"><span data-stu-id="3ed1e-147">port</span></span> |<span data-ttu-id="3ed1e-148">hello TCP-port som hello Cassandra server använder toolisten för klientanslutningar.</span><span class="sxs-lookup"><span data-stu-id="3ed1e-148">hello TCP port that hello Cassandra server uses toolisten for client connections.</span></span> |<span data-ttu-id="3ed1e-149">Nej, standardvärde: 9042</span><span class="sxs-lookup"><span data-stu-id="3ed1e-149">No, default value: 9042</span></span> |
| <span data-ttu-id="3ed1e-150">AuthenticationType</span><span class="sxs-lookup"><span data-stu-id="3ed1e-150">authenticationType</span></span> |<span data-ttu-id="3ed1e-151">Grundläggande eller anonym</span><span class="sxs-lookup"><span data-stu-id="3ed1e-151">Basic, or Anonymous</span></span> |<span data-ttu-id="3ed1e-152">Ja</span><span class="sxs-lookup"><span data-stu-id="3ed1e-152">Yes</span></span> |
| <span data-ttu-id="3ed1e-153">användarnamn</span><span class="sxs-lookup"><span data-stu-id="3ed1e-153">username</span></span> |<span data-ttu-id="3ed1e-154">Ange användarnamn för hello användarkonto.</span><span class="sxs-lookup"><span data-stu-id="3ed1e-154">Specify user name for hello user account.</span></span> |<span data-ttu-id="3ed1e-155">Ja, om authenticationType anges tooBasic.</span><span class="sxs-lookup"><span data-stu-id="3ed1e-155">Yes, if authenticationType is set tooBasic.</span></span> |
| <span data-ttu-id="3ed1e-156">lösenord</span><span class="sxs-lookup"><span data-stu-id="3ed1e-156">password</span></span> |<span data-ttu-id="3ed1e-157">Ange lösenordet för användarkontot för hello.</span><span class="sxs-lookup"><span data-stu-id="3ed1e-157">Specify password for hello user account.</span></span> |<span data-ttu-id="3ed1e-158">Ja, om authenticationType anges tooBasic.</span><span class="sxs-lookup"><span data-stu-id="3ed1e-158">Yes, if authenticationType is set tooBasic.</span></span> |
| <span data-ttu-id="3ed1e-159">gatewayName</span><span class="sxs-lookup"><span data-stu-id="3ed1e-159">gatewayName</span></span> |<span data-ttu-id="3ed1e-160">hello namnet på hello-gateway som har använt tooconnect toohello lokalt Cassandra databas.</span><span class="sxs-lookup"><span data-stu-id="3ed1e-160">hello name of hello gateway that is used tooconnect toohello on-premises Cassandra database.</span></span> |<span data-ttu-id="3ed1e-161">Ja</span><span class="sxs-lookup"><span data-stu-id="3ed1e-161">Yes</span></span> |
| <span data-ttu-id="3ed1e-162">encryptedCredential</span><span class="sxs-lookup"><span data-stu-id="3ed1e-162">encryptedCredential</span></span> |<span data-ttu-id="3ed1e-163">Autentiseringsuppgifter har krypterats av hello gateway.</span><span class="sxs-lookup"><span data-stu-id="3ed1e-163">Credential encrypted by hello gateway.</span></span> |<span data-ttu-id="3ed1e-164">Nej</span><span class="sxs-lookup"><span data-stu-id="3ed1e-164">No</span></span> |

## <a name="dataset-properties"></a><span data-ttu-id="3ed1e-165">Egenskaper för datamängd</span><span class="sxs-lookup"><span data-stu-id="3ed1e-165">Dataset properties</span></span>
<span data-ttu-id="3ed1e-166">En fullständig lista över egenskaper som är tillgängliga för att definiera datauppsättningarna & avsnitt finns hello [skapa datauppsättningar](data-factory-create-datasets.md) artikel.</span><span class="sxs-lookup"><span data-stu-id="3ed1e-166">For a full list of sections & properties available for defining datasets, see hello [Creating datasets](data-factory-create-datasets.md) article.</span></span> <span data-ttu-id="3ed1e-167">Avsnitt som struktur, tillgänglighet och princip på en datamängd JSON är liknande för alla typer av dataset (Azure SQL Azure blob, Azure-tabellen, osv.).</span><span class="sxs-lookup"><span data-stu-id="3ed1e-167">Sections such as structure, availability, and policy of a dataset JSON are similar for all dataset types (Azure SQL, Azure blob, Azure table, etc.).</span></span>

<span data-ttu-id="3ed1e-168">Hej **typeProperties** avsnitt är olika för varje typ av dataset och ger information om hello platsen för hello data i datalagret hello.</span><span class="sxs-lookup"><span data-stu-id="3ed1e-168">hello **typeProperties** section is different for each type of dataset and provides information about hello location of hello data in hello data store.</span></span> <span data-ttu-id="3ed1e-169">Hej typeProperties avsnittet för dataset av typen **CassandraTable** har hello följande egenskaper</span><span class="sxs-lookup"><span data-stu-id="3ed1e-169">hello typeProperties section for dataset of type **CassandraTable** has hello following properties</span></span>

| <span data-ttu-id="3ed1e-170">Egenskap</span><span class="sxs-lookup"><span data-stu-id="3ed1e-170">Property</span></span> | <span data-ttu-id="3ed1e-171">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="3ed1e-171">Description</span></span> | <span data-ttu-id="3ed1e-172">Krävs</span><span class="sxs-lookup"><span data-stu-id="3ed1e-172">Required</span></span> |
| --- | --- | --- |
| <span data-ttu-id="3ed1e-173">keyspace</span><span class="sxs-lookup"><span data-stu-id="3ed1e-173">keyspace</span></span> |<span data-ttu-id="3ed1e-174">Namnet på hello keyspace eller schema i Cassandra databasen.</span><span class="sxs-lookup"><span data-stu-id="3ed1e-174">Name of hello keyspace or schema in Cassandra database.</span></span> |<span data-ttu-id="3ed1e-175">Ja (om **frågan** för **CassandraSource** har inte definierats).</span><span class="sxs-lookup"><span data-stu-id="3ed1e-175">Yes (If **query** for **CassandraSource** is not defined).</span></span> |
| <span data-ttu-id="3ed1e-176">tableName</span><span class="sxs-lookup"><span data-stu-id="3ed1e-176">tableName</span></span> |<span data-ttu-id="3ed1e-177">Namnet på hello tabell i Cassandra databas.</span><span class="sxs-lookup"><span data-stu-id="3ed1e-177">Name of hello table in Cassandra database.</span></span> |<span data-ttu-id="3ed1e-178">Ja (om **frågan** för **CassandraSource** har inte definierats).</span><span class="sxs-lookup"><span data-stu-id="3ed1e-178">Yes (If **query** for **CassandraSource** is not defined).</span></span> |

## <a name="copy-activity-properties"></a><span data-ttu-id="3ed1e-179">Kopiera egenskaper för aktivitet</span><span class="sxs-lookup"><span data-stu-id="3ed1e-179">Copy activity properties</span></span>
<span data-ttu-id="3ed1e-180">En fullständig lista över avsnitt & egenskaper som är tillgängliga för att definiera aktiviteter finns hello [skapar Pipelines](data-factory-create-pipelines.md) artikel.</span><span class="sxs-lookup"><span data-stu-id="3ed1e-180">For a full list of sections & properties available for defining activities, see hello [Creating Pipelines](data-factory-create-pipelines.md) article.</span></span> <span data-ttu-id="3ed1e-181">Egenskaper som namn, beskrivning, ingående och utgående tabeller och principen är tillgängliga för alla typer av aktiviteter.</span><span class="sxs-lookup"><span data-stu-id="3ed1e-181">Properties such as name, description, input and output tables, and policy are available for all types of activities.</span></span>

<span data-ttu-id="3ed1e-182">De egenskaper som är tillgängliga under hello typeProperties i hello aktivitet varierar beroende på varje aktivitetstyp.</span><span class="sxs-lookup"><span data-stu-id="3ed1e-182">Whereas, properties available in hello typeProperties section of hello activity vary with each activity type.</span></span> <span data-ttu-id="3ed1e-183">För Kopieringsaktivitet kan varierar de beroende på hello typer av datakällor och sänkor.</span><span class="sxs-lookup"><span data-stu-id="3ed1e-183">For Copy activity, they vary depending on hello types of sources and sinks.</span></span>

<span data-ttu-id="3ed1e-184">När datakällan är av typen **CassandraSource**, hello följande egenskaper finns i avsnittet typeProperties:</span><span class="sxs-lookup"><span data-stu-id="3ed1e-184">When source is of type **CassandraSource**, hello following properties are available in typeProperties section:</span></span>

| <span data-ttu-id="3ed1e-185">Egenskap</span><span class="sxs-lookup"><span data-stu-id="3ed1e-185">Property</span></span> | <span data-ttu-id="3ed1e-186">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="3ed1e-186">Description</span></span> | <span data-ttu-id="3ed1e-187">Tillåtna värden</span><span class="sxs-lookup"><span data-stu-id="3ed1e-187">Allowed values</span></span> | <span data-ttu-id="3ed1e-188">Krävs</span><span class="sxs-lookup"><span data-stu-id="3ed1e-188">Required</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="3ed1e-189">DocumentDB</span><span class="sxs-lookup"><span data-stu-id="3ed1e-189">query</span></span> |<span data-ttu-id="3ed1e-190">Använda hello anpassad fråga tooread data.</span><span class="sxs-lookup"><span data-stu-id="3ed1e-190">Use hello custom query tooread data.</span></span> |<span data-ttu-id="3ed1e-191">SQL-92 frågan eller CQL frågan.</span><span class="sxs-lookup"><span data-stu-id="3ed1e-191">SQL-92 query or CQL query.</span></span> <span data-ttu-id="3ed1e-192">Se [CQL referens](https://docs.datastax.com/en/cql/3.1/cql/cql_reference/cqlReferenceTOC.html).</span><span class="sxs-lookup"><span data-stu-id="3ed1e-192">See [CQL reference](https://docs.datastax.com/en/cql/3.1/cql/cql_reference/cqlReferenceTOC.html).</span></span> <br/><br/><span data-ttu-id="3ed1e-193">När du använder SQL-frågan anger **keyspace name.table namn** toorepresent hello tabell tooquery.</span><span class="sxs-lookup"><span data-stu-id="3ed1e-193">When using SQL query, specify **keyspace name.table name** toorepresent hello table you want tooquery.</span></span> |<span data-ttu-id="3ed1e-194">Nej (om tabellnamn och keyspace för datauppsättningen har definierats).</span><span class="sxs-lookup"><span data-stu-id="3ed1e-194">No (if tableName and keyspace on dataset are defined).</span></span> |
| <span data-ttu-id="3ed1e-195">consistencyLevel</span><span class="sxs-lookup"><span data-stu-id="3ed1e-195">consistencyLevel</span></span> |<span data-ttu-id="3ed1e-196">hello konsekvensnivå anger hur många repliker måste svara tooa läsbegäran innan det returneras data toohello klientprogrammet.</span><span class="sxs-lookup"><span data-stu-id="3ed1e-196">hello consistency level specifies how many replicas must respond tooa read request before returning data toohello client application.</span></span> <span data-ttu-id="3ed1e-197">Cassandra kontrollerar hello angivet antal repliker för data toosatisfy hello läsa begäran.</span><span class="sxs-lookup"><span data-stu-id="3ed1e-197">Cassandra checks hello specified number of replicas for data toosatisfy hello read request.</span></span> |<span data-ttu-id="3ed1e-198">EN, TVÅ, TRE, KVORUM, ALL, LOCAL_QUORUM EACH_QUORUM, LOCAL_ONE.</span><span class="sxs-lookup"><span data-stu-id="3ed1e-198">ONE, TWO, THREE, QUORUM, ALL, LOCAL_QUORUM, EACH_QUORUM, LOCAL_ONE.</span></span> <span data-ttu-id="3ed1e-199">Se [konfigurera datakonsekvens](http://docs.datastax.com/en//cassandra/2.0/cassandra/dml/dml_config_consistency_c.html) mer information.</span><span class="sxs-lookup"><span data-stu-id="3ed1e-199">See [Configuring data consistency](http://docs.datastax.com/en//cassandra/2.0/cassandra/dml/dml_config_consistency_c.html) for details.</span></span> |<span data-ttu-id="3ed1e-200">Nej.</span><span class="sxs-lookup"><span data-stu-id="3ed1e-200">No.</span></span> <span data-ttu-id="3ed1e-201">Standardvärdet är en.</span><span class="sxs-lookup"><span data-stu-id="3ed1e-201">Default value is ONE.</span></span> |

## <a name="json-example-copy-data-from-cassandra-tooazure-blob"></a><span data-ttu-id="3ed1e-202">JSON-exempel: kopiera data från Cassandra tooAzure Blob</span><span class="sxs-lookup"><span data-stu-id="3ed1e-202">JSON example: Copy data from Cassandra tooAzure Blob</span></span>
<span data-ttu-id="3ed1e-203">Det här exemplet innehåller exempel JSON definitioner som du kan använda toocreate en pipeline med hjälp av [Azure-portalen](data-factory-copy-activity-tutorial-using-azure-portal.md) eller [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md) eller [Azure PowerShell](data-factory-copy-activity-tutorial-using-powershell.md).</span><span class="sxs-lookup"><span data-stu-id="3ed1e-203">This example provides sample JSON definitions that you can use toocreate a pipeline by using [Azure portal](data-factory-copy-activity-tutorial-using-azure-portal.md) or [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md) or [Azure PowerShell](data-factory-copy-activity-tutorial-using-powershell.md).</span></span> <span data-ttu-id="3ed1e-204">Den visar hur toocopy data från en lokal Cassandra databasen tooan Azure Blob Storage.</span><span class="sxs-lookup"><span data-stu-id="3ed1e-204">It shows how toocopy data from an on-premises Cassandra database tooan Azure Blob Storage.</span></span> <span data-ttu-id="3ed1e-205">Data kan dock vara kopierade tooany av hello sänkor anges [här](data-factory-data-movement-activities.md#supported-data-stores-and-formats) med hello Kopieringsaktiviteten i Azure Data Factory.</span><span class="sxs-lookup"><span data-stu-id="3ed1e-205">However, data can be copied tooany of hello sinks stated [here](data-factory-data-movement-activities.md#supported-data-stores-and-formats) using hello Copy Activity in Azure Data Factory.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="3ed1e-206">Det här exemplet innehåller JSON kodavsnitt.</span><span class="sxs-lookup"><span data-stu-id="3ed1e-206">This sample provides JSON snippets.</span></span> <span data-ttu-id="3ed1e-207">Stegvisa instruktioner för att skapa datafabriken hello inkluderas inte.</span><span class="sxs-lookup"><span data-stu-id="3ed1e-207">It does not include step-by-step instructions for creating hello data factory.</span></span> <span data-ttu-id="3ed1e-208">Se [flytta data mellan lokala platser och moln](data-factory-move-data-between-onprem-and-cloud.md) artikel stegvisa instruktioner.</span><span class="sxs-lookup"><span data-stu-id="3ed1e-208">See [moving data between on-premises locations and cloud](data-factory-move-data-between-onprem-and-cloud.md) article for step-by-step instructions.</span></span>

<span data-ttu-id="3ed1e-209">hello exemplet har hello följande data factory-enheter:</span><span class="sxs-lookup"><span data-stu-id="3ed1e-209">hello sample has hello following data factory entities:</span></span>

* <span data-ttu-id="3ed1e-210">En länkad tjänst av typen [OnPremisesCassandra](#linked-service-properties).</span><span class="sxs-lookup"><span data-stu-id="3ed1e-210">A linked service of type [OnPremisesCassandra](#linked-service-properties).</span></span>
* <span data-ttu-id="3ed1e-211">En länkad tjänst av typen [AzureStorage](data-factory-azure-blob-connector.md#linked-service-properties).</span><span class="sxs-lookup"><span data-stu-id="3ed1e-211">A linked service of type [AzureStorage](data-factory-azure-blob-connector.md#linked-service-properties).</span></span>
* <span data-ttu-id="3ed1e-212">Indata [dataset](data-factory-create-datasets.md) av typen [CassandraTable](#dataset-properties).</span><span class="sxs-lookup"><span data-stu-id="3ed1e-212">An input [dataset](data-factory-create-datasets.md) of type [CassandraTable](#dataset-properties).</span></span>
* <span data-ttu-id="3ed1e-213">Utdata [dataset](data-factory-create-datasets.md) av typen [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties).</span><span class="sxs-lookup"><span data-stu-id="3ed1e-213">An output [dataset](data-factory-create-datasets.md) of type [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties).</span></span>
* <span data-ttu-id="3ed1e-214">En [pipeline](data-factory-create-pipelines.md) med Kopieringsaktiviteten som använder [CassandraSource](#copy-activity-properties) och [BlobSink](data-factory-azure-blob-connector.md#copy-activity-properties).</span><span class="sxs-lookup"><span data-stu-id="3ed1e-214">A [pipeline](data-factory-create-pipelines.md) with Copy Activity that uses [CassandraSource](#copy-activity-properties) and [BlobSink](data-factory-azure-blob-connector.md#copy-activity-properties).</span></span>

<span data-ttu-id="3ed1e-215">**Cassandra länkade tjänsten:**</span><span class="sxs-lookup"><span data-stu-id="3ed1e-215">**Cassandra linked service:**</span></span>

<span data-ttu-id="3ed1e-216">Det här exemplet används hello **Cassandra** länkade tjänsten.</span><span class="sxs-lookup"><span data-stu-id="3ed1e-216">This example uses hello **Cassandra** linked service.</span></span> <span data-ttu-id="3ed1e-217">Se [Cassandra länkade tjänsten](#linked-service-properties) avsnittet hello egenskaper som stöds av den här länkade tjänsten.</span><span class="sxs-lookup"><span data-stu-id="3ed1e-217">See [Cassandra linked service](#linked-service-properties) section for hello properties supported by this linked service.</span></span>  

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

<span data-ttu-id="3ed1e-218">**Azure Storage länkade tjänsten:**</span><span class="sxs-lookup"><span data-stu-id="3ed1e-218">**Azure Storage linked service:**</span></span>

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

<span data-ttu-id="3ed1e-219">**Cassandra inkommande datauppsättningen:**</span><span class="sxs-lookup"><span data-stu-id="3ed1e-219">**Cassandra input dataset:**</span></span>

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

<span data-ttu-id="3ed1e-220">Ange **externa** för**SANT** informerar hello Data Factory-tjänsten som hello dataset är externa toohello data factory och inte tillverkas av en aktivitet i hello data factory.</span><span class="sxs-lookup"><span data-stu-id="3ed1e-220">Setting **external** too**true** informs hello Data Factory service that hello dataset is external toohello data factory and is not produced by an activity in hello data factory.</span></span>

<span data-ttu-id="3ed1e-221">**Azure Blob utdatauppsättningen:**</span><span class="sxs-lookup"><span data-stu-id="3ed1e-221">**Azure Blob output dataset:**</span></span>

<span data-ttu-id="3ed1e-222">Data skrivs tooa nya blob varje timme (frekvens: timme, intervall: 1).</span><span class="sxs-lookup"><span data-stu-id="3ed1e-222">Data is written tooa new blob every hour (frequency: hour, interval: 1).</span></span>

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

<span data-ttu-id="3ed1e-223">**Kopiera aktivitet i en pipeline med Cassandra källa och mottagare för Blob:**</span><span class="sxs-lookup"><span data-stu-id="3ed1e-223">**Copy activity in a pipeline with Cassandra source and Blob sink:**</span></span>

<span data-ttu-id="3ed1e-224">hello pipelinen innehåller en kopia-aktivitet som är konfigurerade toouse hello inkommande och utgående datauppsättningar och är schemalagda toorun varje timme.</span><span class="sxs-lookup"><span data-stu-id="3ed1e-224">hello pipeline contains a Copy Activity that is configured toouse hello input and output datasets and is scheduled toorun every hour.</span></span> <span data-ttu-id="3ed1e-225">I hello pipeline JSON-definitionen hello **källa** typ har angetts för**CassandraSource** och **sink** typ har angetts för**BlobSink**.</span><span class="sxs-lookup"><span data-stu-id="3ed1e-225">In hello pipeline JSON definition, hello **source** type is set too**CassandraSource** and **sink** type is set too**BlobSink**.</span></span>

<span data-ttu-id="3ed1e-226">Se [RelationalSource Typegenskaper](#copy-activity-properties) hello lista över egenskaper som stöds av hello RelationalSource.</span><span class="sxs-lookup"><span data-stu-id="3ed1e-226">See [RelationalSource type properties](#copy-activity-properties) for hello list of properties supported by hello RelationalSource.</span></span>

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
            "description": "Copy from Cassandra tooan Azure blob",
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

### <a name="type-mapping-for-cassandra"></a><span data-ttu-id="3ed1e-227">Mappning för Cassandra</span><span class="sxs-lookup"><span data-stu-id="3ed1e-227">Type mapping for Cassandra</span></span>
| <span data-ttu-id="3ed1e-228">Cassandra typ</span><span class="sxs-lookup"><span data-stu-id="3ed1e-228">Cassandra Type</span></span> | <span data-ttu-id="3ed1e-229">.NET baserat typ</span><span class="sxs-lookup"><span data-stu-id="3ed1e-229">.Net Based Type</span></span> |
| --- | --- |
| <span data-ttu-id="3ed1e-230">ASCII</span><span class="sxs-lookup"><span data-stu-id="3ed1e-230">ASCII</span></span> |<span data-ttu-id="3ed1e-231">Sträng</span><span class="sxs-lookup"><span data-stu-id="3ed1e-231">String</span></span> |
| <span data-ttu-id="3ed1e-232">BIGINT</span><span class="sxs-lookup"><span data-stu-id="3ed1e-232">BIGINT</span></span> |<span data-ttu-id="3ed1e-233">Int64</span><span class="sxs-lookup"><span data-stu-id="3ed1e-233">Int64</span></span> |
| <span data-ttu-id="3ed1e-234">BLOB</span><span class="sxs-lookup"><span data-stu-id="3ed1e-234">BLOB</span></span> |<span data-ttu-id="3ed1e-235">byte]</span><span class="sxs-lookup"><span data-stu-id="3ed1e-235">Byte[]</span></span> |
| <span data-ttu-id="3ed1e-236">BOOLESKT VÄRDE</span><span class="sxs-lookup"><span data-stu-id="3ed1e-236">BOOLEAN</span></span> |<span data-ttu-id="3ed1e-237">Booleskt värde</span><span class="sxs-lookup"><span data-stu-id="3ed1e-237">Boolean</span></span> |
| <span data-ttu-id="3ed1e-238">DECIMAL</span><span class="sxs-lookup"><span data-stu-id="3ed1e-238">DECIMAL</span></span> |<span data-ttu-id="3ed1e-239">Decimal</span><span class="sxs-lookup"><span data-stu-id="3ed1e-239">Decimal</span></span> |
| <span data-ttu-id="3ed1e-240">DUBBEL</span><span class="sxs-lookup"><span data-stu-id="3ed1e-240">DOUBLE</span></span> |<span data-ttu-id="3ed1e-241">dubbla</span><span class="sxs-lookup"><span data-stu-id="3ed1e-241">Double</span></span> |
| <span data-ttu-id="3ed1e-242">FLYTTAL</span><span class="sxs-lookup"><span data-stu-id="3ed1e-242">FLOAT</span></span> |<span data-ttu-id="3ed1e-243">Enskild</span><span class="sxs-lookup"><span data-stu-id="3ed1e-243">Single</span></span> |
| <span data-ttu-id="3ed1e-244">INET</span><span class="sxs-lookup"><span data-stu-id="3ed1e-244">INET</span></span> |<span data-ttu-id="3ed1e-245">Sträng</span><span class="sxs-lookup"><span data-stu-id="3ed1e-245">String</span></span> |
| <span data-ttu-id="3ed1e-246">INT</span><span class="sxs-lookup"><span data-stu-id="3ed1e-246">INT</span></span> |<span data-ttu-id="3ed1e-247">Int32</span><span class="sxs-lookup"><span data-stu-id="3ed1e-247">Int32</span></span> |
| <span data-ttu-id="3ed1e-248">TEXT</span><span class="sxs-lookup"><span data-stu-id="3ed1e-248">TEXT</span></span> |<span data-ttu-id="3ed1e-249">Sträng</span><span class="sxs-lookup"><span data-stu-id="3ed1e-249">String</span></span> |
| <span data-ttu-id="3ed1e-250">TIDSSTÄMPEL</span><span class="sxs-lookup"><span data-stu-id="3ed1e-250">TIMESTAMP</span></span> |<span data-ttu-id="3ed1e-251">Datum och tid</span><span class="sxs-lookup"><span data-stu-id="3ed1e-251">DateTime</span></span> |
| <span data-ttu-id="3ed1e-252">TIMEUUID</span><span class="sxs-lookup"><span data-stu-id="3ed1e-252">TIMEUUID</span></span> |<span data-ttu-id="3ed1e-253">GUID</span><span class="sxs-lookup"><span data-stu-id="3ed1e-253">Guid</span></span> |
| <span data-ttu-id="3ed1e-254">UUID</span><span class="sxs-lookup"><span data-stu-id="3ed1e-254">UUID</span></span> |<span data-ttu-id="3ed1e-255">GUID</span><span class="sxs-lookup"><span data-stu-id="3ed1e-255">Guid</span></span> |
| <span data-ttu-id="3ed1e-256">VARCHAR</span><span class="sxs-lookup"><span data-stu-id="3ed1e-256">VARCHAR</span></span> |<span data-ttu-id="3ed1e-257">Sträng</span><span class="sxs-lookup"><span data-stu-id="3ed1e-257">String</span></span> |
| <span data-ttu-id="3ed1e-258">VARINT</span><span class="sxs-lookup"><span data-stu-id="3ed1e-258">VARINT</span></span> |<span data-ttu-id="3ed1e-259">Decimal</span><span class="sxs-lookup"><span data-stu-id="3ed1e-259">Decimal</span></span> |

> [!NOTE]
> <span data-ttu-id="3ed1e-260">Insamling av typer (karta, set, lista, etc.) finns för[arbeta med Cassandra samlingstyper med virtuella tabellen](#work-with-collections-using-virtual-table) avsnitt.</span><span class="sxs-lookup"><span data-stu-id="3ed1e-260">For collection types (map, set, list, etc.), refer too[Work with Cassandra collection types using virtual table](#work-with-collections-using-virtual-table) section.</span></span>
>
> <span data-ttu-id="3ed1e-261">Användardefinierade typer stöds inte.</span><span class="sxs-lookup"><span data-stu-id="3ed1e-261">User-defined types are not supported.</span></span>
>
> <span data-ttu-id="3ed1e-262">hello längden av binär kolumn och strängkolumn längd får inte vara större än 4000.</span><span class="sxs-lookup"><span data-stu-id="3ed1e-262">hello length of Binary Column and String Column lengths cannot be greater than 4000.</span></span>
>
>

## <a name="work-with-collections-using-virtual-table"></a><span data-ttu-id="3ed1e-263">Arbeta med samlingar med virtuella tabellen</span><span class="sxs-lookup"><span data-stu-id="3ed1e-263">Work with collections using virtual table</span></span>
<span data-ttu-id="3ed1e-264">Azure Data Factory använder en inbyggd ODBC-drivrutinen tooconnect tooand kopieringsdata från databasen Cassandra.</span><span class="sxs-lookup"><span data-stu-id="3ed1e-264">Azure Data Factory uses a built-in ODBC driver tooconnect tooand copy data from your Cassandra database.</span></span> <span data-ttu-id="3ed1e-265">För samlingstyper inklusive karta, uppsättning och lista, renormalizes hello drivrutinen hello data i motsvarande virtuella tabeller.</span><span class="sxs-lookup"><span data-stu-id="3ed1e-265">For collection types including map, set and list, hello driver renormalizes hello data into corresponding virtual tables.</span></span> <span data-ttu-id="3ed1e-266">I synnerhet om en tabell innehåller några kolumner i samlingen, hello-drivrutinen genererar hello följande virtuella tabeller:</span><span class="sxs-lookup"><span data-stu-id="3ed1e-266">Specifically, if a table contains any collection columns, hello driver generates hello following virtual tables:</span></span>

* <span data-ttu-id="3ed1e-267">En **bastabellen**, som innehåller hello samma data som hello verkliga tabell utom hello samling kolumner.</span><span class="sxs-lookup"><span data-stu-id="3ed1e-267">A **base table**, which contains hello same data as hello real table except for hello collection columns.</span></span> <span data-ttu-id="3ed1e-268">hello bastabellen använder hello samma namn som hello verkliga tabell som representerar.</span><span class="sxs-lookup"><span data-stu-id="3ed1e-268">hello base table uses hello same name as hello real table that it represents.</span></span>
* <span data-ttu-id="3ed1e-269">En **virtuella tabellen** för varje samling-kolumn som utökar hello kapslade data.</span><span class="sxs-lookup"><span data-stu-id="3ed1e-269">A **virtual table** for each collection column, which expands hello nested data.</span></span> <span data-ttu-id="3ed1e-270">hello virtuella tabeller som representerar samlingar namnges med hello namnet på hello verkliga tabell, avgränsare ”*vt*” och hello hello kolumnens namn.</span><span class="sxs-lookup"><span data-stu-id="3ed1e-270">hello virtual tables that represent collections are named using hello name of hello real table, a separator “*vt*” and hello name of hello column.</span></span>

<span data-ttu-id="3ed1e-271">Virtuella tabeller finns toohello data i hello verkliga tabell, aktiverar hello drivrutinen tooaccess hello Avnormaliserade data.</span><span class="sxs-lookup"><span data-stu-id="3ed1e-271">Virtual tables refer toohello data in hello real table, enabling hello driver tooaccess hello denormalized data.</span></span> <span data-ttu-id="3ed1e-272">Se avsnittet för information.</span><span class="sxs-lookup"><span data-stu-id="3ed1e-272">See Example section for details.</span></span> <span data-ttu-id="3ed1e-273">Du kan komma åt hello innehållet i Cassandra samlingar genom att fråga och hello virtuella tabeller.</span><span class="sxs-lookup"><span data-stu-id="3ed1e-273">You can access hello content of Cassandra collections by querying and joining hello virtual tables.</span></span>

<span data-ttu-id="3ed1e-274">Du kan använda hello [guiden Kopiera](data-factory-data-movement-activities.md#create-a-pipeline-with-copy-activity) toointuitively hello listan över tabeller i Cassandra databasen, inklusive hello virtuella tabeller och förhandsgranska hello data i.</span><span class="sxs-lookup"><span data-stu-id="3ed1e-274">You can use hello [Copy Wizard](data-factory-data-movement-activities.md#create-a-pipeline-with-copy-activity) toointuitively view hello list of tables in Cassandra database including hello virtual tables, and preview hello data inside.</span></span> <span data-ttu-id="3ed1e-275">Du kan också skapa en fråga i hello guiden Kopiera och validera toosee hello resultatet.</span><span class="sxs-lookup"><span data-stu-id="3ed1e-275">You can also construct a query in hello Copy Wizard and validate toosee hello result.</span></span>

### <a name="example"></a><span data-ttu-id="3ed1e-276">Exempel</span><span class="sxs-lookup"><span data-stu-id="3ed1e-276">Example</span></span>
<span data-ttu-id="3ed1e-277">Till exempel är hello följande ”ExampleTable” en Cassandra databastabell som innehåller ett heltal primärnyckelkolumnen med namnet ”pk_int”, en textkolumn namngivet värde, en kolumn, en karta kolumn och en set-kolumn (med namnet ”StringSet”).</span><span class="sxs-lookup"><span data-stu-id="3ed1e-277">For example, hello following “ExampleTable” is a Cassandra database table that contains an integer primary key column named “pk_int”, a text column named value, a list column, a map column, and a set column (named “StringSet”).</span></span>

| <span data-ttu-id="3ed1e-278">pk_int</span><span class="sxs-lookup"><span data-stu-id="3ed1e-278">pk_int</span></span> | <span data-ttu-id="3ed1e-279">Värde</span><span class="sxs-lookup"><span data-stu-id="3ed1e-279">Value</span></span> | <span data-ttu-id="3ed1e-280">Visa lista</span><span class="sxs-lookup"><span data-stu-id="3ed1e-280">List</span></span> | <span data-ttu-id="3ed1e-281">karta</span><span class="sxs-lookup"><span data-stu-id="3ed1e-281">Map</span></span> | <span data-ttu-id="3ed1e-282">StringSet</span><span class="sxs-lookup"><span data-stu-id="3ed1e-282">StringSet</span></span> |
| --- | --- | --- | --- | --- |
| <span data-ttu-id="3ed1e-283">1</span><span class="sxs-lookup"><span data-stu-id="3ed1e-283">1</span></span> |<span data-ttu-id="3ed1e-284">”exempelvärde 1”</span><span class="sxs-lookup"><span data-stu-id="3ed1e-284">"sample value 1"</span></span> |<span data-ttu-id="3ed1e-285">["1", "2", "3"]</span><span class="sxs-lookup"><span data-stu-id="3ed1e-285">["1", "2", "3"]</span></span> |<span data-ttu-id="3ed1e-286">{”S1”: ”a”, ”S2”: ”b”}</span><span class="sxs-lookup"><span data-stu-id="3ed1e-286">{"S1": "a", "S2": "b"}</span></span> |<span data-ttu-id="3ed1e-287">{”A”, ”B”, ”C”}</span><span class="sxs-lookup"><span data-stu-id="3ed1e-287">{"A", "B", "C"}</span></span> |
| <span data-ttu-id="3ed1e-288">3</span><span class="sxs-lookup"><span data-stu-id="3ed1e-288">3</span></span> |<span data-ttu-id="3ed1e-289">”exempelvärde 3”</span><span class="sxs-lookup"><span data-stu-id="3ed1e-289">"sample value 3"</span></span> |<span data-ttu-id="3ed1e-290">["100", "101", "102", "105"]</span><span class="sxs-lookup"><span data-stu-id="3ed1e-290">["100", "101", "102", "105"]</span></span> |<span data-ttu-id="3ed1e-291">{”S1”: ”t”}</span><span class="sxs-lookup"><span data-stu-id="3ed1e-291">{"S1": "t"}</span></span> |<span data-ttu-id="3ed1e-292">{”A”, ”E”}</span><span class="sxs-lookup"><span data-stu-id="3ed1e-292">{"A", "E"}</span></span> |

<span data-ttu-id="3ed1e-293">hello drivrutinen skulle generera flera virtuella tabeller toorepresent denna tabell.</span><span class="sxs-lookup"><span data-stu-id="3ed1e-293">hello driver would generate multiple virtual tables toorepresent this single table.</span></span> <span data-ttu-id="3ed1e-294">hello sekundärnyckelskolumnerna i hello virtuella tabeller referera hello primärnyckelkolumnerna i hello verkliga tabell och ange vilka verkligt tabellrad raden hello virtuella tabellen motsvarar.</span><span class="sxs-lookup"><span data-stu-id="3ed1e-294">hello foreign key columns in hello virtual tables reference hello primary key columns in hello real table, and indicate which real table row hello virtual table row corresponds to.</span></span>

<span data-ttu-id="3ed1e-295">hello första virtuella tabellen är hello bastabellen med namnet ”ExampleTable” visas i följande tabell hello.</span><span class="sxs-lookup"><span data-stu-id="3ed1e-295">hello first virtual table is hello base table named “ExampleTable” is shown in hello following table.</span></span> <span data-ttu-id="3ed1e-296">hello bastabellen innehåller hello samma data som hello ursprungliga databastabell förutom hello samlingar, som utelämnas från den här tabellen och expanderas i andra virtuella tabeller.</span><span class="sxs-lookup"><span data-stu-id="3ed1e-296">hello base table contains hello same data as hello original database table except for hello collections, which are omitted from this table and expanded in other virtual tables.</span></span>

| <span data-ttu-id="3ed1e-297">pk_int</span><span class="sxs-lookup"><span data-stu-id="3ed1e-297">pk_int</span></span> | <span data-ttu-id="3ed1e-298">Värde</span><span class="sxs-lookup"><span data-stu-id="3ed1e-298">Value</span></span> |
| --- | --- |
| <span data-ttu-id="3ed1e-299">1</span><span class="sxs-lookup"><span data-stu-id="3ed1e-299">1</span></span> |<span data-ttu-id="3ed1e-300">”exempelvärde 1”</span><span class="sxs-lookup"><span data-stu-id="3ed1e-300">"sample value 1"</span></span> |
| <span data-ttu-id="3ed1e-301">3</span><span class="sxs-lookup"><span data-stu-id="3ed1e-301">3</span></span> |<span data-ttu-id="3ed1e-302">”exempelvärde 3”</span><span class="sxs-lookup"><span data-stu-id="3ed1e-302">"sample value 3"</span></span> |

<span data-ttu-id="3ed1e-303">hello visar följande tabeller hello virtuella register som renormalize hello data från hello lista och mappa StringSet kolumner.</span><span class="sxs-lookup"><span data-stu-id="3ed1e-303">hello following tables show hello virtual tables that renormalize hello data from hello List, Map, and StringSet columns.</span></span> <span data-ttu-id="3ed1e-304">hello kolumner med namn som slutar med ”_index” eller ”_nyckelskyddsserverns” ange hello placeringen av hello inom hello ursprungliga listan eller karta.</span><span class="sxs-lookup"><span data-stu-id="3ed1e-304">hello columns with names that end with “_index” or “_key” indicate hello position of hello data within hello original list or map.</span></span> <span data-ttu-id="3ed1e-305">hello kolumnerna med namn som slutar med ”_value” innehåller hello expanderas data från hello samling.</span><span class="sxs-lookup"><span data-stu-id="3ed1e-305">hello columns with names that end with “_value” contain hello expanded data from hello collection.</span></span>

#### <a name="table-exampletablevtlist"></a><span data-ttu-id="3ed1e-306">Tabell ”ExampleTable_vt_List”:</span><span class="sxs-lookup"><span data-stu-id="3ed1e-306">Table “ExampleTable_vt_List”:</span></span>
| <span data-ttu-id="3ed1e-307">pk_int</span><span class="sxs-lookup"><span data-stu-id="3ed1e-307">pk_int</span></span> | <span data-ttu-id="3ed1e-308">List_index</span><span class="sxs-lookup"><span data-stu-id="3ed1e-308">List_index</span></span> | <span data-ttu-id="3ed1e-309">List_value</span><span class="sxs-lookup"><span data-stu-id="3ed1e-309">List_value</span></span> |
| --- | --- | --- |
| <span data-ttu-id="3ed1e-310">1</span><span class="sxs-lookup"><span data-stu-id="3ed1e-310">1</span></span> |<span data-ttu-id="3ed1e-311">0</span><span class="sxs-lookup"><span data-stu-id="3ed1e-311">0</span></span> |<span data-ttu-id="3ed1e-312">1</span><span class="sxs-lookup"><span data-stu-id="3ed1e-312">1</span></span> |
| <span data-ttu-id="3ed1e-313">1</span><span class="sxs-lookup"><span data-stu-id="3ed1e-313">1</span></span> |<span data-ttu-id="3ed1e-314">1</span><span class="sxs-lookup"><span data-stu-id="3ed1e-314">1</span></span> |<span data-ttu-id="3ed1e-315">2</span><span class="sxs-lookup"><span data-stu-id="3ed1e-315">2</span></span> |
| <span data-ttu-id="3ed1e-316">1</span><span class="sxs-lookup"><span data-stu-id="3ed1e-316">1</span></span> |<span data-ttu-id="3ed1e-317">2</span><span class="sxs-lookup"><span data-stu-id="3ed1e-317">2</span></span> |<span data-ttu-id="3ed1e-318">3</span><span class="sxs-lookup"><span data-stu-id="3ed1e-318">3</span></span> |
| <span data-ttu-id="3ed1e-319">3</span><span class="sxs-lookup"><span data-stu-id="3ed1e-319">3</span></span> |<span data-ttu-id="3ed1e-320">0</span><span class="sxs-lookup"><span data-stu-id="3ed1e-320">0</span></span> |<span data-ttu-id="3ed1e-321">100</span><span class="sxs-lookup"><span data-stu-id="3ed1e-321">100</span></span> |
| <span data-ttu-id="3ed1e-322">3</span><span class="sxs-lookup"><span data-stu-id="3ed1e-322">3</span></span> |<span data-ttu-id="3ed1e-323">1</span><span class="sxs-lookup"><span data-stu-id="3ed1e-323">1</span></span> |<span data-ttu-id="3ed1e-324">101</span><span class="sxs-lookup"><span data-stu-id="3ed1e-324">101</span></span> |
| <span data-ttu-id="3ed1e-325">3</span><span class="sxs-lookup"><span data-stu-id="3ed1e-325">3</span></span> |<span data-ttu-id="3ed1e-326">2</span><span class="sxs-lookup"><span data-stu-id="3ed1e-326">2</span></span> |<span data-ttu-id="3ed1e-327">102</span><span class="sxs-lookup"><span data-stu-id="3ed1e-327">102</span></span> |
| <span data-ttu-id="3ed1e-328">3</span><span class="sxs-lookup"><span data-stu-id="3ed1e-328">3</span></span> |<span data-ttu-id="3ed1e-329">3</span><span class="sxs-lookup"><span data-stu-id="3ed1e-329">3</span></span> |<span data-ttu-id="3ed1e-330">103</span><span class="sxs-lookup"><span data-stu-id="3ed1e-330">103</span></span> |

#### <a name="table-exampletablevtmap"></a><span data-ttu-id="3ed1e-331">Tabell ”ExampleTable_vt_Map”:</span><span class="sxs-lookup"><span data-stu-id="3ed1e-331">Table “ExampleTable_vt_Map”:</span></span>
| <span data-ttu-id="3ed1e-332">pk_int</span><span class="sxs-lookup"><span data-stu-id="3ed1e-332">pk_int</span></span> | <span data-ttu-id="3ed1e-333">Map_key</span><span class="sxs-lookup"><span data-stu-id="3ed1e-333">Map_key</span></span> | <span data-ttu-id="3ed1e-334">Map_value</span><span class="sxs-lookup"><span data-stu-id="3ed1e-334">Map_value</span></span> |
| --- | --- | --- |
| <span data-ttu-id="3ed1e-335">1</span><span class="sxs-lookup"><span data-stu-id="3ed1e-335">1</span></span> |<span data-ttu-id="3ed1e-336">S1</span><span class="sxs-lookup"><span data-stu-id="3ed1e-336">S1</span></span> |<span data-ttu-id="3ed1e-337">A</span><span class="sxs-lookup"><span data-stu-id="3ed1e-337">A</span></span> |
| <span data-ttu-id="3ed1e-338">1</span><span class="sxs-lookup"><span data-stu-id="3ed1e-338">1</span></span> |<span data-ttu-id="3ed1e-339">S2</span><span class="sxs-lookup"><span data-stu-id="3ed1e-339">S2</span></span> |<span data-ttu-id="3ed1e-340">B</span><span class="sxs-lookup"><span data-stu-id="3ed1e-340">b</span></span> |
| <span data-ttu-id="3ed1e-341">3</span><span class="sxs-lookup"><span data-stu-id="3ed1e-341">3</span></span> |<span data-ttu-id="3ed1e-342">S1</span><span class="sxs-lookup"><span data-stu-id="3ed1e-342">S1</span></span> |<span data-ttu-id="3ed1e-343">T</span><span class="sxs-lookup"><span data-stu-id="3ed1e-343">t</span></span> |

#### <a name="table-exampletablevtstringset"></a><span data-ttu-id="3ed1e-344">Tabell ”ExampleTable_vt_StringSet”:</span><span class="sxs-lookup"><span data-stu-id="3ed1e-344">Table “ExampleTable_vt_StringSet”:</span></span>
| <span data-ttu-id="3ed1e-345">pk_int</span><span class="sxs-lookup"><span data-stu-id="3ed1e-345">pk_int</span></span> | <span data-ttu-id="3ed1e-346">StringSet_value</span><span class="sxs-lookup"><span data-stu-id="3ed1e-346">StringSet_value</span></span> |
| --- | --- |
| <span data-ttu-id="3ed1e-347">1</span><span class="sxs-lookup"><span data-stu-id="3ed1e-347">1</span></span> |<span data-ttu-id="3ed1e-348">A</span><span class="sxs-lookup"><span data-stu-id="3ed1e-348">A</span></span> |
| <span data-ttu-id="3ed1e-349">1</span><span class="sxs-lookup"><span data-stu-id="3ed1e-349">1</span></span> |<span data-ttu-id="3ed1e-350">B</span><span class="sxs-lookup"><span data-stu-id="3ed1e-350">B</span></span> |
| <span data-ttu-id="3ed1e-351">1</span><span class="sxs-lookup"><span data-stu-id="3ed1e-351">1</span></span> |<span data-ttu-id="3ed1e-352">C</span><span class="sxs-lookup"><span data-stu-id="3ed1e-352">C</span></span> |
| <span data-ttu-id="3ed1e-353">3</span><span class="sxs-lookup"><span data-stu-id="3ed1e-353">3</span></span> |<span data-ttu-id="3ed1e-354">A</span><span class="sxs-lookup"><span data-stu-id="3ed1e-354">A</span></span> |
| <span data-ttu-id="3ed1e-355">3</span><span class="sxs-lookup"><span data-stu-id="3ed1e-355">3</span></span> |<span data-ttu-id="3ed1e-356">E</span><span class="sxs-lookup"><span data-stu-id="3ed1e-356">E</span></span> |

## <a name="map-source-toosink-columns"></a><span data-ttu-id="3ed1e-357">Mappa källkolumner toosink</span><span class="sxs-lookup"><span data-stu-id="3ed1e-357">Map source toosink columns</span></span>
<span data-ttu-id="3ed1e-358">toolearn mappning tabellkolumner i källan dataset toocolumns i sink dataset finns [mappa dataset kolumner i Azure Data Factory](data-factory-map-columns.md).</span><span class="sxs-lookup"><span data-stu-id="3ed1e-358">toolearn about mapping columns in source dataset toocolumns in sink dataset, see [Mapping dataset columns in Azure Data Factory](data-factory-map-columns.md).</span></span>

## <a name="repeatable-read-from-relational-sources"></a><span data-ttu-id="3ed1e-359">Upprepbar läsning från relationella källor</span><span class="sxs-lookup"><span data-stu-id="3ed1e-359">Repeatable read from relational sources</span></span>
<span data-ttu-id="3ed1e-360">När du kopierar data från relationella datalager, Kom ihåg tooavoid repeterbarhet oönskade resultat.</span><span class="sxs-lookup"><span data-stu-id="3ed1e-360">When copying data from relational data stores, keep repeatability in mind tooavoid unintended outcomes.</span></span> <span data-ttu-id="3ed1e-361">I Azure Data Factory, kan du köra en sektor manuellt.</span><span class="sxs-lookup"><span data-stu-id="3ed1e-361">In Azure Data Factory, you can rerun a slice manually.</span></span> <span data-ttu-id="3ed1e-362">Du kan också konfigurera i principen för en dataset så att ett segment som körs när ett fel uppstår.</span><span class="sxs-lookup"><span data-stu-id="3ed1e-362">You can also configure retry policy for a dataset so that a slice is rerun when a failure occurs.</span></span> <span data-ttu-id="3ed1e-363">När ett segment körs på antingen sätt måste toomake att som hello samma data läses oavsett hur många gånger ett segment körs.</span><span class="sxs-lookup"><span data-stu-id="3ed1e-363">When a slice is rerun in either way, you need toomake sure that hello same data is read no matter how many times a slice is run.</span></span> <span data-ttu-id="3ed1e-364">Se [Repeatable läsa från relationella källor](data-factory-repeatable-copy.md#repeatable-read-from-relational-sources).</span><span class="sxs-lookup"><span data-stu-id="3ed1e-364">See [Repeatable read from relational sources](data-factory-repeatable-copy.md#repeatable-read-from-relational-sources).</span></span>

## <a name="performance-and-tuning"></a><span data-ttu-id="3ed1e-365">Prestanda och finjustering</span><span class="sxs-lookup"><span data-stu-id="3ed1e-365">Performance and Tuning</span></span>
<span data-ttu-id="3ed1e-366">Se [kopiera aktivitet prestanda och justera guiden](data-factory-copy-activity-performance.md) toolearn om nyckeln faktorer som påverkan prestanda för flytt av data (Kopieringsaktiviteten) i Azure Data Factory och olika sätt toooptimize den.</span><span class="sxs-lookup"><span data-stu-id="3ed1e-366">See [Copy Activity Performance & Tuning Guide](data-factory-copy-activity-performance.md) toolearn about key factors that impact performance of data movement (Copy Activity) in Azure Data Factory and various ways toooptimize it.</span></span>
