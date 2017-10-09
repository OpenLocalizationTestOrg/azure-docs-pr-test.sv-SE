---
title: "aaaCopy data till/från Oracle med hjälp av Data Factory | Microsoft Docs"
description: "Lär dig hur toocopy data till och från Oracle-databas som är lokalt med Azure Data Factory."
services: data-factory
documentationcenter: 
author: linda33wj
manager: jhubbard
editor: monicar
ms.assetid: 3c20aa95-a8a1-4aae-9180-a6a16d64a109
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/04/2017
ms.author: jingwang
ms.openlocfilehash: adb6d5fbe38e18791616ac77e8179970bbea37fb
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="copy-data-tofrom-on-premises-oracle-using-azure-data-factory"></a><span data-ttu-id="72175-103">Kopiera data till och från lokala Oracle med Azure Data Factory</span><span class="sxs-lookup"><span data-stu-id="72175-103">Copy data to/from on-premises Oracle using Azure Data Factory</span></span>
<span data-ttu-id="72175-104">Den här artikeln förklarar hur toouse hello Kopieringsaktiviteten i Azure Data Factory toomove data till eller från en lokal Oracle-databas.</span><span class="sxs-lookup"><span data-stu-id="72175-104">This article explains how toouse hello Copy Activity in Azure Data Factory toomove data to/from an on-premises Oracle database.</span></span> <span data-ttu-id="72175-105">Den bygger på hello [Data Movement aktiviteter](data-factory-data-movement-activities.md) artikel som ger en allmän översikt över dataflyttning hello kopieringsaktiviteten.</span><span class="sxs-lookup"><span data-stu-id="72175-105">It builds on hello [Data Movement Activities](data-factory-data-movement-activities.md) article, which presents a general overview of data movement with hello copy activity.</span></span>

## <a name="supported-scenarios"></a><span data-ttu-id="72175-106">Scenarier som stöds</span><span class="sxs-lookup"><span data-stu-id="72175-106">Supported scenarios</span></span>
<span data-ttu-id="72175-107">Du kan kopiera data **från en Oracle-databas** toohello följande datakällor:</span><span class="sxs-lookup"><span data-stu-id="72175-107">You can copy data **from an Oracle database** toohello following data stores:</span></span>

[!INCLUDE [data-factory-supported-sink](../../includes/data-factory-supported-sinks.md)]

<span data-ttu-id="72175-108">Du kan kopiera data från hello följande datalager **tooan Oracle-databas**:</span><span class="sxs-lookup"><span data-stu-id="72175-108">You can copy data from hello following data stores **tooan Oracle database**:</span></span>

[!INCLUDE [data-factory-supported-sources](../../includes/data-factory-supported-sources.md)]

## <a name="prerequisites"></a><span data-ttu-id="72175-109">Krav</span><span class="sxs-lookup"><span data-stu-id="72175-109">Prerequisites</span></span>
<span data-ttu-id="72175-110">Data Factory stöder anslutande tooon lokal Oracle källor med hello Data Management Gateway.</span><span class="sxs-lookup"><span data-stu-id="72175-110">Data Factory supports connecting tooon-premises Oracle sources using hello Data Management Gateway.</span></span> <span data-ttu-id="72175-111">Se [Data Management Gateway](data-factory-data-management-gateway.md) artikel toolearn om Data Management Gateway och [flytta data från lokala toocloud](data-factory-move-data-between-onprem-and-cloud.md) artikel stegvisa instruktioner om hur du konfigurerar hello gateway en data-pipeline toomove data.</span><span class="sxs-lookup"><span data-stu-id="72175-111">See [Data Management Gateway](data-factory-data-management-gateway.md) article toolearn about Data Management Gateway and [Move data from on-premises toocloud](data-factory-move-data-between-onprem-and-cloud.md) article for step-by-step instructions on setting up hello gateway a data pipeline toomove data.</span></span>

<span data-ttu-id="72175-112">Gateway krävs även om hello Oracle finns i en Azure IaaS-VM.</span><span class="sxs-lookup"><span data-stu-id="72175-112">Gateway is required even if hello Oracle is hosted in an Azure IaaS VM.</span></span> <span data-ttu-id="72175-113">Du kan installera hello gateway på hello samma IaaS VM som hello data lagras eller på en annan virtuell dator så länge som hello gateway kan ansluta toohello databas.</span><span class="sxs-lookup"><span data-stu-id="72175-113">You can install hello gateway on hello same IaaS VM as hello data store or on a different VM as long as hello gateway can connect toohello database.</span></span>

> [!NOTE]
> <span data-ttu-id="72175-114">Se [felsökning av problem med gateway](data-factory-data-management-gateway.md#troubleshooting-gateway-issues) tips om hur du felsöker anslutning /-gateway relaterade problem.</span><span class="sxs-lookup"><span data-stu-id="72175-114">See [Troubleshoot gateway issues](data-factory-data-management-gateway.md#troubleshooting-gateway-issues) for tips on troubleshooting connection/gateway related issues.</span></span>

## <a name="supported-versions-and-installation"></a><span data-ttu-id="72175-115">Versioner som stöds och installation</span><span class="sxs-lookup"><span data-stu-id="72175-115">Supported versions and installation</span></span>
<span data-ttu-id="72175-116">Den här anslutningen Oracle stöder två versioner av drivrutiner:</span><span class="sxs-lookup"><span data-stu-id="72175-116">This Oracle connector support two versions of drivers:</span></span>

- <span data-ttu-id="72175-117">**Microsoft-drivrutin för Oracle (rekommenderas)**: från Data Management Gateway version 2.7, en Microsoft-drivrutin för Oracle installeras automatiskt tillsammans med hello gateway, så behöver du inte tooadditionally referensen hello drivrutin tooestablish anslutning tooOracle, och du kan också prestanda förbättras kopiera med hjälp av den här drivrutinen.</span><span class="sxs-lookup"><span data-stu-id="72175-117">**Microsoft driver for Oracle (recommended)**: starting from Data Management Gateway version 2.7, a Microsoft driver for Oracle is automatically installed along with hello gateway, so you don't need tooadditionally handle hello driver in order tooestablish connectivity tooOracle, and you can also experience better copy performance using this driver.</span></span> <span data-ttu-id="72175-118">Nedan versioner av Oracle stöds databaser:</span><span class="sxs-lookup"><span data-stu-id="72175-118">Below versions of Oracle databases are supported:</span></span>
    - <span data-ttu-id="72175-119">Oracle 12c R1 (12.1)</span><span class="sxs-lookup"><span data-stu-id="72175-119">Oracle 12c R1 (12.1)</span></span>
    - <span data-ttu-id="72175-120">Oracle 11g R1, R2 (11.1, 11.2)</span><span class="sxs-lookup"><span data-stu-id="72175-120">Oracle 11g R1, R2 (11.1, 11.2)</span></span>
    - <span data-ttu-id="72175-121">Oracle 10g R1, R2 (10.1, 10,2)</span><span class="sxs-lookup"><span data-stu-id="72175-121">Oracle 10g R1, R2 (10.1, 10.2)</span></span>
    - <span data-ttu-id="72175-122">Oracle 9i R1, R2 (9.0.1, 9.2)</span><span class="sxs-lookup"><span data-stu-id="72175-122">Oracle 9i R1, R2 (9.0.1, 9.2)</span></span>
    - <span data-ttu-id="72175-123">Oracle 8i R3 (8.1.7)</span><span class="sxs-lookup"><span data-stu-id="72175-123">Oracle 8i R3 (8.1.7)</span></span>

> [!IMPORTANT]
> <span data-ttu-id="72175-124">Microsoft-drivrutin för Oracle stöder för närvarande endast kopiering av data från Oracle men inte skriva tooOracle.</span><span class="sxs-lookup"><span data-stu-id="72175-124">Currently Microsoft driver for Oracle only supports copying data from Oracle but not writing tooOracle.</span></span> <span data-ttu-id="72175-125">Och Observera hello Testa anslutning funktionen fliken diagnostik för Data Management Gateway har inte stöd för den här drivrutinen.</span><span class="sxs-lookup"><span data-stu-id="72175-125">And note hello test connection capability in Data Management Gateway Diagnostics tab does not support this driver.</span></span> <span data-ttu-id="72175-126">Du kan också använda hello kopiera guiden toovalidate hello anslutningen.</span><span class="sxs-lookup"><span data-stu-id="72175-126">Alternatively, you can use hello copy wizard toovalidate hello connectivity.</span></span>
>

- <span data-ttu-id="72175-127">**Oracle Data Provider för .NET:** du kan också välja toouse Oracle Data Provider toocopy data från / tooOracle.</span><span class="sxs-lookup"><span data-stu-id="72175-127">**Oracle Data Provider for .NET:** you can also choose toouse Oracle Data Provider toocopy data from/tooOracle.</span></span> <span data-ttu-id="72175-128">Den här komponenten ingår i [Oracle Data Access-komponenter för Windows](http://www.oracle.com/technetwork/topics/dotnet/downloads/).</span><span class="sxs-lookup"><span data-stu-id="72175-128">This component is included in [Oracle Data Access Components for Windows](http://www.oracle.com/technetwork/topics/dotnet/downloads/).</span></span> <span data-ttu-id="72175-129">Installera hello rätt version (32/64-bitars) på hello datorn där hello gatewayen har installerats.</span><span class="sxs-lookup"><span data-stu-id="72175-129">Install hello appropriate version (32/64 bit) on hello machine where hello gateway is installed.</span></span> <span data-ttu-id="72175-130">[Oracle-dataprovidern .NET 12.1](http://docs.oracle.com/database/121/ODPNT/InstallSystemRequirements.htm#ODPNT149) kan komma åt tooOracle Database 10 g version 2 eller senare.</span><span class="sxs-lookup"><span data-stu-id="72175-130">[Oracle Data Provider .NET 12.1](http://docs.oracle.com/database/121/ODPNT/InstallSystemRequirements.htm#ODPNT149) can access tooOracle Database 10g Release 2 or later.</span></span>

    <span data-ttu-id="72175-131">Om du väljer ”XCopy-Installation”, följer du anvisningarna i hello viktigt.htm.</span><span class="sxs-lookup"><span data-stu-id="72175-131">If you choose “XCopy Installation”, follow steps in hello readme.htm.</span></span> <span data-ttu-id="72175-132">Vi rekommenderar att du väljer hello installer med användargränssnitt (icke-XCopy en).</span><span class="sxs-lookup"><span data-stu-id="72175-132">We recommend you choose hello installer with UI (non-XCopy one).</span></span>

    <span data-ttu-id="72175-133">När du har installerat hello providern **starta om** hello Data Management Gateway-värdtjänsten på datorn med tjänster appleten (eller) Data Management Gateway Configuration Manager.</span><span class="sxs-lookup"><span data-stu-id="72175-133">After installing hello provider, **restart** hello Data Management Gateway host service on your machine using Services applet (or) Data Management Gateway Configuration Manager.</span></span>  

<span data-ttu-id="72175-134">Om du använder kopiera guiden tooauthor hello kopiera pipeline kommer hello drivrutinen typen att automatiskt identifiera.</span><span class="sxs-lookup"><span data-stu-id="72175-134">If you use copy wizard tooauthor hello copy pipeline, hello driver type will be auto-determined.</span></span> <span data-ttu-id="72175-135">Microsoft-drivrutinen används som standard om inte din gatewayversionen är lägre än 2.7 eller Oracle som mottagare som du anger.</span><span class="sxs-lookup"><span data-stu-id="72175-135">Microsoft driver will be used by default, unless your gateway version is lower than 2.7 or you choose Oracle as sink.</span></span>

## <a name="getting-started"></a><span data-ttu-id="72175-136">Komma igång</span><span class="sxs-lookup"><span data-stu-id="72175-136">Getting started</span></span>
<span data-ttu-id="72175-137">Du kan skapa en pipeline med en kopia-aktivitet som flyttar data till eller från en lokal Oracle-databas med hjälp av olika verktyg/API: er.</span><span class="sxs-lookup"><span data-stu-id="72175-137">You can create a pipeline with a copy activity that moves data to/from an on-premises Oracle database by using different tools/APIs.</span></span>

<span data-ttu-id="72175-138">hello enklaste sättet toocreate en pipeline är toouse hello **guiden Kopiera**.</span><span class="sxs-lookup"><span data-stu-id="72175-138">hello easiest way toocreate a pipeline is toouse hello **Copy Wizard**.</span></span> <span data-ttu-id="72175-139">Se [Självstudier: skapa en pipeline med hjälp av guiden Kopiera](data-factory-copy-data-wizard-tutorial.md) för en snabb genomgång om hur du skapar en pipeline med hjälp av guiden för hello kopiera data.</span><span class="sxs-lookup"><span data-stu-id="72175-139">See [Tutorial: Create a pipeline using Copy Wizard](data-factory-copy-data-wizard-tutorial.md) for a quick walkthrough on creating a pipeline using hello Copy data wizard.</span></span>

<span data-ttu-id="72175-140">Du kan också använda följande verktyg toocreate en pipeline hello: **Azure-portalen**, **Visual Studio**, **Azure PowerShell**, **Azure Resource Manager-mall** , **.NET API**, och **REST API**.</span><span class="sxs-lookup"><span data-stu-id="72175-140">You can also use hello following tools toocreate a pipeline: **Azure portal**, **Visual Studio**, **Azure PowerShell**, **Azure Resource Manager template**, **.NET API**, and **REST API**.</span></span> <span data-ttu-id="72175-141">Se [kopiera aktivitet kursen](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) för stegvisa instruktioner toocreate en pipeline med en Kopieringsaktivitet.</span><span class="sxs-lookup"><span data-stu-id="72175-141">See [Copy activity tutorial](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) for step-by-step instructions toocreate a pipeline with a copy activity.</span></span>

<span data-ttu-id="72175-142">Om du använder hello verktyg eller API: er kan utföra du hello följande steg toocreate en pipeline som flyttar data från en källdata lagra tooa sink-datalagret:</span><span class="sxs-lookup"><span data-stu-id="72175-142">Whether you use hello tools or APIs, you perform hello following steps toocreate a pipeline that moves data from a source data store tooa sink data store:</span></span>

1. <span data-ttu-id="72175-143">Skapa en **datafabriken**.</span><span class="sxs-lookup"><span data-stu-id="72175-143">Create a **data factory**.</span></span> <span data-ttu-id="72175-144">En datafabrik kan innehålla en eller flera pipelines.</span><span class="sxs-lookup"><span data-stu-id="72175-144">A data factory may contain one or more pipelines.</span></span> 
2. <span data-ttu-id="72175-145">Skapa **länkade tjänster** toolink indata och utdata lagrar tooyour data factory.</span><span class="sxs-lookup"><span data-stu-id="72175-145">Create **linked services** toolink input and output data stores tooyour data factory.</span></span> <span data-ttu-id="72175-146">Till exempel om du kopierar data från en Oralce databasen tooan Azure-blobblagring skapa du två länkade tjänster toolink till Oracle-databasen och Azure storage-konto tooyour data factory.</span><span class="sxs-lookup"><span data-stu-id="72175-146">For example, if you are copying data from an Oralce database tooan Azure blob storage, you create two linked services toolink your Oracle database and Azure storage account tooyour data factory.</span></span> <span data-ttu-id="72175-147">Länkad tjänstegenskaper som är specifika tooOracle, se [länkade tjänstegenskaper](#linked-service-properties) avsnitt.</span><span class="sxs-lookup"><span data-stu-id="72175-147">For linked service properties that are specific tooOracle, see [linked service properties](#linked-service-properties) section.</span></span>
3. <span data-ttu-id="72175-148">Skapa **datauppsättningar** toorepresent indata och utdata för hello kopieringsåtgärden.</span><span class="sxs-lookup"><span data-stu-id="72175-148">Create **datasets** toorepresent input and output data for hello copy operation.</span></span> <span data-ttu-id="72175-149">I hello exempelvis nämns i hello sista steget skapar du en dataset toospecify hello tabell i Oracle-databasen som innehåller hello indata.</span><span class="sxs-lookup"><span data-stu-id="72175-149">In hello example mentioned in hello last step, you create a dataset toospecify hello table in your Oracle database that contains hello input data.</span></span> <span data-ttu-id="72175-150">Och, skapar du en annan dataset toospecify hello blob-behållaren och hello-mappen som innehåller hello data kopieras från hello Oracle-databas.</span><span class="sxs-lookup"><span data-stu-id="72175-150">And, you create another dataset toospecify hello blob container and hello folder that holds hello data copied from hello Oracle database.</span></span> <span data-ttu-id="72175-151">Egenskaper för datamängd som är specifika tooOracle, se [egenskaper för datamängd](#dataset-properties) avsnitt.</span><span class="sxs-lookup"><span data-stu-id="72175-151">For dataset properties that are specific tooOracle, see [dataset properties](#dataset-properties) section.</span></span>
4. <span data-ttu-id="72175-152">Skapa en **pipeline** med en kopia-aktivitet som tar en datamängd som indata och en dataset som utdata.</span><span class="sxs-lookup"><span data-stu-id="72175-152">Create a **pipeline** with a copy activity that takes a dataset as an input and a dataset as an output.</span></span> <span data-ttu-id="72175-153">I hello-exemplet ovan, använder du OracleSource som en källa och BlobSink som en mottagare för hello kopieringsaktiviteten.</span><span class="sxs-lookup"><span data-stu-id="72175-153">In hello example mentioned earlier, you use OracleSource as a source and BlobSink as a sink for hello copy activity.</span></span> <span data-ttu-id="72175-154">På samma sätt om du kopierar från Azure Blob Storage tooOracle databasen använder du BlobSource och OracleSink i hello kopieringsaktiviteten.</span><span class="sxs-lookup"><span data-stu-id="72175-154">Similarly, if you are copying from Azure Blob Storage tooOracle Database, you use BlobSource and OracleSink in hello copy activity.</span></span> <span data-ttu-id="72175-155">Kopiera Aktivitetsegenskaper som är specifika tooOracle databasen, se [kopiera Aktivitetsegenskaper](#copy-activity-properties) avsnitt.</span><span class="sxs-lookup"><span data-stu-id="72175-155">For copy activity properties that are specific tooOracle database, see [copy activity properties](#copy-activity-properties) section.</span></span> <span data-ttu-id="72175-156">Mer information om hur toouse en databas som en källa eller en mottagare klickar du på hello länk under föregående hello för datalager.</span><span class="sxs-lookup"><span data-stu-id="72175-156">For details on how toouse a data store as a source or a sink, click hello link in hello previous section for your data store.</span></span> 

<span data-ttu-id="72175-157">När du använder guiden hello skapas automatiskt JSON definitioner för dessa Data Factory-enheter (länkade tjänster, datauppsättningar och hello pipeline) för dig.</span><span class="sxs-lookup"><span data-stu-id="72175-157">When you use hello wizard, JSON definitions for these Data Factory entities (linked services, datasets, and hello pipeline) are automatically created for you.</span></span> <span data-ttu-id="72175-158">När du använder Verktyg/API: er (utom .NET API), kan du definiera dessa Data Factory-enheter med hjälp av hello JSON-format.</span><span class="sxs-lookup"><span data-stu-id="72175-158">When you use tools/APIs (except .NET API), you define these Data Factory entities by using hello JSON format.</span></span>  <span data-ttu-id="72175-159">Exempel med JSON-definitioner för Data Factory-entiteter som har använt toocopy data till/från en lokal Oracle-databas finns [JSON-exempel](#json-examples-for-copying-data-to-and-from-oracle-database) i den här artikeln.</span><span class="sxs-lookup"><span data-stu-id="72175-159">For samples with JSON definitions for Data Factory entities that are used toocopy data to/from an on-premises Oracle database, see [JSON examples](#json-examples-for-copying-data-to-and-from-oracle-database) section of this article.</span></span>

<span data-ttu-id="72175-160">hello följande avsnitt innehåller information om JSON-egenskaper används toodefine Data Factory-enheter:</span><span class="sxs-lookup"><span data-stu-id="72175-160">hello following sections provide details about JSON properties that are used toodefine Data Factory entities:</span></span>

## <a name="linked-service-properties"></a><span data-ttu-id="72175-161">Länkad tjänstegenskaper</span><span class="sxs-lookup"><span data-stu-id="72175-161">Linked service properties</span></span>
<span data-ttu-id="72175-162">hello följande tabell innehåller en beskrivning för JSON-element specifika tooOracle länkad tjänst.</span><span class="sxs-lookup"><span data-stu-id="72175-162">hello following table provides description for JSON elements specific tooOracle linked service.</span></span>

| <span data-ttu-id="72175-163">Egenskap</span><span class="sxs-lookup"><span data-stu-id="72175-163">Property</span></span> | <span data-ttu-id="72175-164">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="72175-164">Description</span></span> | <span data-ttu-id="72175-165">Krävs</span><span class="sxs-lookup"><span data-stu-id="72175-165">Required</span></span> |
| --- | --- | --- |
| <span data-ttu-id="72175-166">typ</span><span class="sxs-lookup"><span data-stu-id="72175-166">type</span></span> |<span data-ttu-id="72175-167">hello Typegenskapen måste anges till: **OnPremisesOracle**</span><span class="sxs-lookup"><span data-stu-id="72175-167">hello type property must be set to: **OnPremisesOracle**</span></span> |<span data-ttu-id="72175-168">Ja</span><span class="sxs-lookup"><span data-stu-id="72175-168">Yes</span></span> |
| <span data-ttu-id="72175-169">driverType</span><span class="sxs-lookup"><span data-stu-id="72175-169">driverType</span></span> | <span data-ttu-id="72175-170">Ange vilka drivrutinen toouse toocopy data från / tooOracle databasen.</span><span class="sxs-lookup"><span data-stu-id="72175-170">Specify which driver toouse toocopy data from/tooOracle Database.</span></span> <span data-ttu-id="72175-171">Tillåtna värden är **Microsoft** eller **ODP** (standard).</span><span class="sxs-lookup"><span data-stu-id="72175-171">Allowed values are **Microsoft** or **ODP** (default).</span></span> <span data-ttu-id="72175-172">Se [stöds version och vilka installationsalternativ](#supported-versions-and-installation) avsnitt för mer information.</span><span class="sxs-lookup"><span data-stu-id="72175-172">See [Supported version and installation](#supported-versions-and-installation) section on driver details.</span></span> | <span data-ttu-id="72175-173">Nej</span><span class="sxs-lookup"><span data-stu-id="72175-173">No</span></span> |
| <span data-ttu-id="72175-174">connectionString</span><span class="sxs-lookup"><span data-stu-id="72175-174">connectionString</span></span> | <span data-ttu-id="72175-175">Ange nödvändig information tooconnect toohello Oracle-databasinstans för hello-egenskapen connectionString.</span><span class="sxs-lookup"><span data-stu-id="72175-175">Specify information needed tooconnect toohello Oracle Database instance for hello connectionString property.</span></span> | <span data-ttu-id="72175-176">Ja</span><span class="sxs-lookup"><span data-stu-id="72175-176">Yes</span></span> |
| <span data-ttu-id="72175-177">gatewayName</span><span class="sxs-lookup"><span data-stu-id="72175-177">gatewayName</span></span> | <span data-ttu-id="72175-178">Namnet på hello gateway som som används tooconnect toohello lokal Oracle-server</span><span class="sxs-lookup"><span data-stu-id="72175-178">Name of hello gateway that that is used tooconnect toohello on-premises Oracle server</span></span> |<span data-ttu-id="72175-179">Ja</span><span class="sxs-lookup"><span data-stu-id="72175-179">Yes</span></span> |

<span data-ttu-id="72175-180">**Exempel: använda Microsoft-drivrutinen:**</span><span class="sxs-lookup"><span data-stu-id="72175-180">**Example: using Microsoft driver:**</span></span>
```json
{
    "name": "OnPremisesOracleLinkedService",
    "properties": {
        "type": "OnPremisesOracle",
        "typeProperties": {
            "driverType": "Microsoft",
            "connectionString":"Host=<host>;Port=<port>;Sid=<sid>;User Id=<username>;Password=<password>;",
            "gatewayName": "<gateway name>"
        }
    }
}
```

<span data-ttu-id="72175-181">**Exempel: med ODP drivrutin**</span><span class="sxs-lookup"><span data-stu-id="72175-181">**Example: using ODP driver**</span></span>

<span data-ttu-id="72175-182">Se för[platsen](https://www.connectionstrings.com/oracle-data-provider-for-net-odp-net/) för hello tillåtet format.</span><span class="sxs-lookup"><span data-stu-id="72175-182">Refer too[this site](https://www.connectionstrings.com/oracle-data-provider-for-net-odp-net/) for hello allowed formats.</span></span>

```json
{
    "name": "OnPremisesOracleLinkedService",
    "properties": {
        "type": "OnPremisesOracle",
        "typeProperties": {
            "connectionString": "Data Source=(DESCRIPTION=(ADDRESS=(PROTOCOL=TCP)(HOST=<hostname>)(PORT=<port number>))(CONNECT_DATA=(SERVICE_NAME=<SID>)));
User Id=<username>;Password=<password>;",
            "gatewayName": "<gateway name>"
        }
    }
}
```

## <a name="dataset-properties"></a><span data-ttu-id="72175-183">Egenskaper för datamängd</span><span class="sxs-lookup"><span data-stu-id="72175-183">Dataset properties</span></span>
<span data-ttu-id="72175-184">En fullständig lista över egenskaper som är tillgängliga för att definiera datauppsättningarna & avsnitt finns hello [skapa datauppsättningar](data-factory-create-datasets.md) artikel.</span><span class="sxs-lookup"><span data-stu-id="72175-184">For a full list of sections & properties available for defining datasets, see hello [Creating datasets](data-factory-create-datasets.md) article.</span></span> <span data-ttu-id="72175-185">Avsnitt som struktur, tillgänglighet och princip på en datamängd JSON är liknande för alla typer av datauppsättningen (Oracle, Azure blob, Azure-tabellen, osv.).</span><span class="sxs-lookup"><span data-stu-id="72175-185">Sections such as structure, availability, and policy of a dataset JSON are similar for all dataset types (Oracle, Azure blob, Azure table, etc.).</span></span>

<span data-ttu-id="72175-186">hello typeProperties avsnittet är olika för varje typ av dataset och ger information om hello platsen för hello data i datalagret hello.</span><span class="sxs-lookup"><span data-stu-id="72175-186">hello typeProperties section is different for each type of dataset and provides information about hello location of hello data in hello data store.</span></span> <span data-ttu-id="72175-187">Hej typeProperties avsnittet för hello dataset av typen OracleTable har hello följande egenskaper:</span><span class="sxs-lookup"><span data-stu-id="72175-187">hello typeProperties section for hello dataset of type OracleTable has hello following properties:</span></span>

| <span data-ttu-id="72175-188">Egenskap</span><span class="sxs-lookup"><span data-stu-id="72175-188">Property</span></span> | <span data-ttu-id="72175-189">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="72175-189">Description</span></span> | <span data-ttu-id="72175-190">Krävs</span><span class="sxs-lookup"><span data-stu-id="72175-190">Required</span></span> |
| --- | --- | --- |
| <span data-ttu-id="72175-191">tableName</span><span class="sxs-lookup"><span data-stu-id="72175-191">tableName</span></span> |<span data-ttu-id="72175-192">Namnet på hello tabell i hello Oracle-databas som hello länkade tjänsten refererar till.</span><span class="sxs-lookup"><span data-stu-id="72175-192">Name of hello table in hello Oracle Database that hello linked service refers to.</span></span> |<span data-ttu-id="72175-193">Nej (om **oracleReaderQuery** av **OracleSource** har angetts)</span><span class="sxs-lookup"><span data-stu-id="72175-193">No (if **oracleReaderQuery** of **OracleSource** is specified)</span></span> |

## <a name="copy-activity-properties"></a><span data-ttu-id="72175-194">Kopiera egenskaper för aktivitet</span><span class="sxs-lookup"><span data-stu-id="72175-194">Copy activity properties</span></span>
<span data-ttu-id="72175-195">En fullständig lista över avsnitt & egenskaper som är tillgängliga för att definiera aktiviteter finns hello [skapar Pipelines](data-factory-create-pipelines.md) artikel.</span><span class="sxs-lookup"><span data-stu-id="72175-195">For a full list of sections & properties available for defining activities, see hello [Creating Pipelines](data-factory-create-pipelines.md) article.</span></span> <span data-ttu-id="72175-196">Egenskaper som namn, beskrivning, ingående och utgående tabeller och principen är tillgängliga för alla typer av aktiviteter.</span><span class="sxs-lookup"><span data-stu-id="72175-196">Properties such as name, description, input and output tables, and policy are available for all types of activities.</span></span>

> [!NOTE]
> <span data-ttu-id="72175-197">Hej Kopieringsaktiviteten tar endast en inmatning och ger en enda utdata.</span><span class="sxs-lookup"><span data-stu-id="72175-197">hello Copy Activity takes only one input and produces only one output.</span></span>

<span data-ttu-id="72175-198">De egenskaper som är tillgängliga under hello typeProperties i hello aktivitet varierar beroende på varje aktivitetstyp.</span><span class="sxs-lookup"><span data-stu-id="72175-198">Whereas, properties available in hello typeProperties section of hello activity vary with each activity type.</span></span> <span data-ttu-id="72175-199">För Kopieringsaktivitet kan varierar de beroende på hello typer av datakällor och sänkor.</span><span class="sxs-lookup"><span data-stu-id="72175-199">For Copy activity, they vary depending on hello types of sources and sinks.</span></span>

### <a name="oraclesource"></a><span data-ttu-id="72175-200">OracleSource</span><span class="sxs-lookup"><span data-stu-id="72175-200">OracleSource</span></span>
<span data-ttu-id="72175-201">I en Kopieringsaktivitet när hello källa är av typen **OracleSource** hello följande egenskaper är tillgängliga i **typeProperties** avsnitt:</span><span class="sxs-lookup"><span data-stu-id="72175-201">In Copy activity, when hello source is of type **OracleSource** hello following properties are available in **typeProperties** section:</span></span>

| <span data-ttu-id="72175-202">Egenskap</span><span class="sxs-lookup"><span data-stu-id="72175-202">Property</span></span> | <span data-ttu-id="72175-203">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="72175-203">Description</span></span> | <span data-ttu-id="72175-204">Tillåtna värden</span><span class="sxs-lookup"><span data-stu-id="72175-204">Allowed values</span></span> | <span data-ttu-id="72175-205">Krävs</span><span class="sxs-lookup"><span data-stu-id="72175-205">Required</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="72175-206">oracleReaderQuery</span><span class="sxs-lookup"><span data-stu-id="72175-206">oracleReaderQuery</span></span> |<span data-ttu-id="72175-207">Använda hello anpassad fråga tooread data.</span><span class="sxs-lookup"><span data-stu-id="72175-207">Use hello custom query tooread data.</span></span> |<span data-ttu-id="72175-208">SQL-sträng.</span><span class="sxs-lookup"><span data-stu-id="72175-208">SQL query string.</span></span> <span data-ttu-id="72175-209">Till exempel: Välj * från mytable prefix</span><span class="sxs-lookup"><span data-stu-id="72175-209">For example: select * from MyTable</span></span> <br/><br/><span data-ttu-id="72175-210">Om inget anges hello SQL-instruktionen som körs: Välj * från mytable prefix</span><span class="sxs-lookup"><span data-stu-id="72175-210">If not specified, hello SQL statement that is executed: select * from MyTable</span></span> |<span data-ttu-id="72175-211">Nej (om **tableName** av **dataset** har angetts)</span><span class="sxs-lookup"><span data-stu-id="72175-211">No (if **tableName** of **dataset** is specified)</span></span> |

### <a name="oraclesink"></a><span data-ttu-id="72175-212">OracleSink</span><span class="sxs-lookup"><span data-stu-id="72175-212">OracleSink</span></span>
<span data-ttu-id="72175-213">**OracleSink** stöder hello följande egenskaper:</span><span class="sxs-lookup"><span data-stu-id="72175-213">**OracleSink** supports hello following properties:</span></span>

| <span data-ttu-id="72175-214">Egenskap</span><span class="sxs-lookup"><span data-stu-id="72175-214">Property</span></span> | <span data-ttu-id="72175-215">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="72175-215">Description</span></span> | <span data-ttu-id="72175-216">Tillåtna värden</span><span class="sxs-lookup"><span data-stu-id="72175-216">Allowed values</span></span> | <span data-ttu-id="72175-217">Krävs</span><span class="sxs-lookup"><span data-stu-id="72175-217">Required</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="72175-218">writeBatchTimeout</span><span class="sxs-lookup"><span data-stu-id="72175-218">writeBatchTimeout</span></span> |<span data-ttu-id="72175-219">Vänta tills hello batch insert-åtgärden toocomplete innan tidsgränsen uppnås.</span><span class="sxs-lookup"><span data-stu-id="72175-219">Wait time for hello batch insert operation toocomplete before it times out.</span></span> |<span data-ttu-id="72175-220">TimeSpan</span><span class="sxs-lookup"><span data-stu-id="72175-220">timespan</span></span><br/><br/> <span data-ttu-id="72175-221">Exempel: 00:30:00 (30 minuter).</span><span class="sxs-lookup"><span data-stu-id="72175-221">Example: 00:30:00 (30 minutes).</span></span> |<span data-ttu-id="72175-222">Nej</span><span class="sxs-lookup"><span data-stu-id="72175-222">No</span></span> |
| <span data-ttu-id="72175-223">writeBatchSize</span><span class="sxs-lookup"><span data-stu-id="72175-223">writeBatchSize</span></span> |<span data-ttu-id="72175-224">Infogar data i hello SQL-tabellen när hello buffertstorlek når writeBatchSize.</span><span class="sxs-lookup"><span data-stu-id="72175-224">Inserts data into hello SQL table when hello buffer size reaches writeBatchSize.</span></span> |<span data-ttu-id="72175-225">Heltal (antalet rader)</span><span class="sxs-lookup"><span data-stu-id="72175-225">Integer (number of rows)</span></span> |<span data-ttu-id="72175-226">Nej (standard: 100)</span><span class="sxs-lookup"><span data-stu-id="72175-226">No (default: 100)</span></span> |
| <span data-ttu-id="72175-227">sqlWriterCleanupScript</span><span class="sxs-lookup"><span data-stu-id="72175-227">sqlWriterCleanupScript</span></span> |<span data-ttu-id="72175-228">Ange en fråga för aktiviteten kopiera tooexecute så att data för ett visst segment har rensats bort.</span><span class="sxs-lookup"><span data-stu-id="72175-228">Specify a query for Copy Activity tooexecute such that data of a specific slice is cleaned up.</span></span> |<span data-ttu-id="72175-229">En frågesats.</span><span class="sxs-lookup"><span data-stu-id="72175-229">A query statement.</span></span> |<span data-ttu-id="72175-230">Nej</span><span class="sxs-lookup"><span data-stu-id="72175-230">No</span></span> |
| <span data-ttu-id="72175-231">sliceIdentifierColumnName</span><span class="sxs-lookup"><span data-stu-id="72175-231">sliceIdentifierColumnName</span></span> |<span data-ttu-id="72175-232">Ange kolumnnamn för Kopieringsaktiviteten toofill med genereras automatiskt segment-ID som har använt tooclean in data för ett visst segment när köras på nytt.</span><span class="sxs-lookup"><span data-stu-id="72175-232">Specify column name for Copy Activity toofill with auto generated slice identifier, which is used tooclean up data of a specific slice when rerun.</span></span> |<span data-ttu-id="72175-233">Kolumnnamnet för en kolumn med datatypen för binary(32).</span><span class="sxs-lookup"><span data-stu-id="72175-233">Column name of a column with data type of binary(32).</span></span> |<span data-ttu-id="72175-234">Nej</span><span class="sxs-lookup"><span data-stu-id="72175-234">No</span></span> |

## <a name="json-examples-for-copying-data-tooand-from-oracle-database"></a><span data-ttu-id="72175-235">JSON-exempel för att kopiera data tooand från Oracle-databas</span><span class="sxs-lookup"><span data-stu-id="72175-235">JSON examples for copying data tooand from Oracle database</span></span>
<span data-ttu-id="72175-236">hello följande exempel innehåller exempel JSON definitioner som du kan använda toocreate en pipeline med hjälp av [Azure-portalen](data-factory-copy-activity-tutorial-using-azure-portal.md) eller [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md) eller [Azure PowerShell](data-factory-copy-activity-tutorial-using-powershell.md).</span><span class="sxs-lookup"><span data-stu-id="72175-236">hello following example provides sample JSON definitions that you can use toocreate a pipeline by using [Azure portal](data-factory-copy-activity-tutorial-using-azure-portal.md) or [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md) or [Azure PowerShell](data-factory-copy-activity-tutorial-using-powershell.md).</span></span> <span data-ttu-id="72175-237">De visar hur toocopy data från / tooan Oracle-databas till/från Azure Blob Storage.</span><span class="sxs-lookup"><span data-stu-id="72175-237">They show how toocopy data from/tooan Oracle database to/from Azure Blob Storage.</span></span> <span data-ttu-id="72175-238">Data kan dock vara kopierade tooany av hello sänkor anges [här](data-factory-data-movement-activities.md#supported-data-stores-and-formats) med hello Kopieringsaktiviteten i Azure Data Factory.</span><span class="sxs-lookup"><span data-stu-id="72175-238">However, data can be copied tooany of hello sinks stated [here](data-factory-data-movement-activities.md#supported-data-stores-and-formats) using hello Copy Activity in Azure Data Factory.</span></span>   

## <a name="example-copy-data-from-oracle-tooazure-blob"></a><span data-ttu-id="72175-239">Exempel: Kopiera data från Oracle tooAzure Blob</span><span class="sxs-lookup"><span data-stu-id="72175-239">Example: Copy data from Oracle tooAzure Blob</span></span>

<span data-ttu-id="72175-240">hello exemplet har hello följande data factory-enheter:</span><span class="sxs-lookup"><span data-stu-id="72175-240">hello sample has hello following data factory entities:</span></span>

1. <span data-ttu-id="72175-241">En länkad tjänst av typen [OnPremisesOracle](data-factory-onprem-oracle-connector.md#linked-service-properties).</span><span class="sxs-lookup"><span data-stu-id="72175-241">A linked service of type [OnPremisesOracle](data-factory-onprem-oracle-connector.md#linked-service-properties).</span></span>
2. <span data-ttu-id="72175-242">En länkad tjänst av typen [AzureStorage](data-factory-azure-blob-connector.md#linked-service-properties).</span><span class="sxs-lookup"><span data-stu-id="72175-242">A linked service of type [AzureStorage](data-factory-azure-blob-connector.md#linked-service-properties).</span></span>
3. <span data-ttu-id="72175-243">Indata [dataset](data-factory-create-datasets.md) av typen [OracleTable](data-factory-onprem-oracle-connector.md#dataset-properties).</span><span class="sxs-lookup"><span data-stu-id="72175-243">An input [dataset](data-factory-create-datasets.md) of type [OracleTable](data-factory-onprem-oracle-connector.md#dataset-properties).</span></span>
4. <span data-ttu-id="72175-244">Utdata [dataset](data-factory-create-datasets.md) av typen [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties).</span><span class="sxs-lookup"><span data-stu-id="72175-244">An output [dataset](data-factory-create-datasets.md) of type [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties).</span></span>
5. <span data-ttu-id="72175-245">En [pipeline](data-factory-create-pipelines.md) med kopieringsaktiviteten som använder [OracleSource](data-factory-onprem-oracle-connector.md#copy-activity-properties) som källa och [BlobSink](data-factory-azure-blob-connector.md#copy-activity-properties) som mottagare.</span><span class="sxs-lookup"><span data-stu-id="72175-245">A [pipeline](data-factory-create-pipelines.md) with Copy activity that uses [OracleSource](data-factory-onprem-oracle-connector.md#copy-activity-properties) as source and [BlobSink](data-factory-azure-blob-connector.md#copy-activity-properties) as sink.</span></span>

<span data-ttu-id="72175-246">hello exemplet kopierar data från en tabell i en lokal Oracle-databasen tooa blob varje timme.</span><span class="sxs-lookup"><span data-stu-id="72175-246">hello sample copies data from a table in an on-premises Oracle database tooa blob hourly.</span></span> <span data-ttu-id="72175-247">Mer information om olika egenskaper som används i hello-exempel finns i dokumentationen i hello-exempel i följande avsnitt.</span><span class="sxs-lookup"><span data-stu-id="72175-247">For more information on various properties used in hello sample, see documentation in sections following hello samples.</span></span>

<span data-ttu-id="72175-248">**Oracle länkad tjänst:**</span><span class="sxs-lookup"><span data-stu-id="72175-248">**Oracle linked service:**</span></span>

```json
{
    "name": "OnPremisesOracleLinkedService",
    "properties": {
        "type": "OnPremisesOracle",
        "typeProperties": {
            "driverType": "Microsoft",
            "connectionString":"Host=<host>;Port=<port>;Sid=<sid>;User Id=<username>;Password=<password>;",
            "gatewayName": "<gateway name>"
        }
    }
}
```

<span data-ttu-id="72175-249">**Azure Blob storage länkade tjänsten:**</span><span class="sxs-lookup"><span data-stu-id="72175-249">**Azure Blob storage linked service:**</span></span>

```json
{
    "name": "StorageLinkedService",
    "properties": {
        "type": "AzureStorage",
        "typeProperties": {
            "connectionString": "DefaultEndpointsProtocol=https;AccountName=<account name>;AccountKey=<Account key>"
        }
    }
}
```

<span data-ttu-id="72175-250">**Oracle inkommande datauppsättningen:**</span><span class="sxs-lookup"><span data-stu-id="72175-250">**Oracle input dataset:**</span></span>

<span data-ttu-id="72175-251">hello exemplet förutsätter att du har skapat en tabell ”mytable” som prefix i Oracle och innehåller en kolumn med namnet ”timestampcolumn” för tid series-data.</span><span class="sxs-lookup"><span data-stu-id="72175-251">hello sample assumes you have created a table “MyTable” in Oracle and it contains a column called “timestampcolumn” for time series data.</span></span>

<span data-ttu-id="72175-252">Inställningen ”externa”: ”true” informerar hello Data Factory-tjänsten som hello dataset är externa toohello data factory och inte tillverkas av en aktivitet i hello data factory.</span><span class="sxs-lookup"><span data-stu-id="72175-252">Setting “external”: ”true” informs hello Data Factory service that hello dataset is external toohello data factory and is not produced by an activity in hello data factory.</span></span>

```json
{
    "name": "OracleInput",
    "properties": {
        "type": "OracleTable",
        "linkedServiceName": "OnPremisesOracleLinkedService",
        "typeProperties": {
            "tableName": "MyTable"
        },
        "external": true,
        "availability": {
            "offset": "01:00:00",
            "interval": "1",
            "anchorDateTime": "2014-02-27T12:00:00",
            "frequency": "Hour"
        },
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

<span data-ttu-id="72175-253">**Azure Blob utdatauppsättningen:**</span><span class="sxs-lookup"><span data-stu-id="72175-253">**Azure Blob output dataset:**</span></span>

<span data-ttu-id="72175-254">Data skrivs tooa nya blob varje timme (frekvens: timme, intervall: 1).</span><span class="sxs-lookup"><span data-stu-id="72175-254">Data is written tooa new blob every hour (frequency: hour, interval: 1).</span></span> <span data-ttu-id="72175-255">hello mappen sökvägen och filnamnet för hello blob utvärderas dynamiskt baserat på hello starttiden för hello-segment som bearbetas.</span><span class="sxs-lookup"><span data-stu-id="72175-255">hello folder path and file name for hello blob are dynamically evaluated based on hello start time of hello slice that is being processed.</span></span> <span data-ttu-id="72175-256">hello mappsökväg använder år, månad, dag och timmar delar av hello starttid.</span><span class="sxs-lookup"><span data-stu-id="72175-256">hello folder path uses year, month, day, and hours parts of hello start time.</span></span>

```json
{
    "name": "AzureBlobOutput",
    "properties": {
        "type": "AzureBlob",
        "linkedServiceName": "StorageLinkedService",
        "typeProperties": {
            "folderPath": "mycontainer/myfolder/yearno={Year}/monthno={Month}/dayno={Day}/hourno={Hour}",
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
            "format": {
                "type": "TextFormat",
                "columnDelimiter": "\t",
                "rowDelimiter": "\n"
            }
        },
        "availability": {
            "frequency": "Hour",
            "interval": 1
        }
    }
}
```

<span data-ttu-id="72175-257">**Pipeline med kopieringsaktiviteten:**</span><span class="sxs-lookup"><span data-stu-id="72175-257">**Pipeline with Copy activity:**</span></span>

<span data-ttu-id="72175-258">hello pipelinen innehåller en kopia-aktivitet som är konfigurerade toouse hello inkommande och utgående datauppsättningar och är schemalagda toorun varje timme.</span><span class="sxs-lookup"><span data-stu-id="72175-258">hello pipeline contains a Copy Activity that is configured toouse hello input and output datasets and is scheduled toorun hourly.</span></span> <span data-ttu-id="72175-259">I hello pipeline JSON-definitionen hello **källa** typ har angetts för**OracleSource** och **sink** typ har angetts för**BlobSink**.</span><span class="sxs-lookup"><span data-stu-id="72175-259">In hello pipeline JSON definition, hello **source** type is set too**OracleSource** and **sink** type is set too**BlobSink**.</span></span>  <span data-ttu-id="72175-260">hello SQL-frågan som anges med **oracleReaderQuery** egenskapen väljer hello data i hello tidigare timme toocopy.</span><span class="sxs-lookup"><span data-stu-id="72175-260">hello SQL query specified with **oracleReaderQuery** property selects hello data in hello past hour toocopy.</span></span>

```json
{  
    "name":"SamplePipeline",
    "properties":{  
        "start":"2014-06-01T18:00:00",
        "end":"2014-06-01T19:00:00",
        "description":"pipeline for copy activity",
        "activities":[  
            {
                "name": "OracletoBlob",
                "description": "copy activity",
                "type": "Copy",
                "inputs": [
                    {
                        "name": " OracleInput"
                    }
                ],
                "outputs": [
                    {
                        "name": "AzureBlobOutput"
                    }
                ],
                "typeProperties": {
                    "source": {
                        "type": "OracleSource",
                        "oracleReaderQuery": "$$Text.Format('select * from MyTable where timestampcolumn >= \\'{0:yyyy-MM-dd HH:mm}\\' AND timestampcolumn < \\'{1:yyyy-MM-dd HH:mm}\\'', WindowStart, WindowEnd)"
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

## <a name="example-copy-data-from-azure-blob-toooracle"></a><span data-ttu-id="72175-261">Exempel: Kopiera data från Azure Blob-tooOracle</span><span class="sxs-lookup"><span data-stu-id="72175-261">Example: Copy data from Azure Blob tooOracle</span></span>
<span data-ttu-id="72175-262">Det här exemplet visas hur toocopy data från ett Azure Blob Storage-tooan lokal Oracle-databas.</span><span class="sxs-lookup"><span data-stu-id="72175-262">This sample shows how toocopy data from an Azure Blob Storage tooan on-premises Oracle database.</span></span> <span data-ttu-id="72175-263">Dock datan kan kopieras **direkt** från någon av hello källor anges [här](data-factory-data-movement-activities.md#supported-data-stores-and-formats) med hello Kopieringsaktiviteten i Azure Data Factory.</span><span class="sxs-lookup"><span data-stu-id="72175-263">However, data can be copied **directly** from any of hello sources stated [here](data-factory-data-movement-activities.md#supported-data-stores-and-formats) using hello Copy Activity in Azure Data Factory.</span></span>  

<span data-ttu-id="72175-264">hello exemplet har hello följande data factory-enheter:</span><span class="sxs-lookup"><span data-stu-id="72175-264">hello sample has hello following data factory entities:</span></span>

1. <span data-ttu-id="72175-265">En länkad tjänst av typen [OnPremisesOracle](data-factory-onprem-oracle-connector.md#linked-service-properties).</span><span class="sxs-lookup"><span data-stu-id="72175-265">A linked service of type [OnPremisesOracle](data-factory-onprem-oracle-connector.md#linked-service-properties).</span></span>
2. <span data-ttu-id="72175-266">En länkad tjänst av typen [AzureStorage](data-factory-azure-blob-connector.md#linked-service-properties).</span><span class="sxs-lookup"><span data-stu-id="72175-266">A linked service of type [AzureStorage](data-factory-azure-blob-connector.md#linked-service-properties).</span></span>
3. <span data-ttu-id="72175-267">Indata [dataset](data-factory-create-datasets.md) av typen [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties).</span><span class="sxs-lookup"><span data-stu-id="72175-267">An input [dataset](data-factory-create-datasets.md) of type [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties).</span></span>
4. <span data-ttu-id="72175-268">Utdata [dataset](data-factory-create-datasets.md) av typen [OracleTable](data-factory-onprem-oracle-connector.md#dataset-properties).</span><span class="sxs-lookup"><span data-stu-id="72175-268">An output [dataset](data-factory-create-datasets.md) of type [OracleTable](data-factory-onprem-oracle-connector.md#dataset-properties).</span></span>
5. <span data-ttu-id="72175-269">En [pipeline](data-factory-create-pipelines.md) med kopieringsaktiviteten som använder [BlobSource](data-factory-azure-blob-connector.md#copy-activity-properties) som källa [OracleSink](data-factory-onprem-oracle-connector.md#copy-activity-properties) som mottagare.</span><span class="sxs-lookup"><span data-stu-id="72175-269">A [pipeline](data-factory-create-pipelines.md) with Copy activity that uses [BlobSource](data-factory-azure-blob-connector.md#copy-activity-properties) as source [OracleSink](data-factory-onprem-oracle-connector.md#copy-activity-properties) as sink.</span></span>

<span data-ttu-id="72175-270">hello exemplet kopierar data från en tabell med blob tooa i en lokal Oracle-databas i timmen.</span><span class="sxs-lookup"><span data-stu-id="72175-270">hello sample copies data from a blob tooa table in an on-premises Oracle database every hour.</span></span> <span data-ttu-id="72175-271">Mer information om olika egenskaper som används i hello-exempel finns i dokumentationen i hello-exempel i följande avsnitt.</span><span class="sxs-lookup"><span data-stu-id="72175-271">For more information on various properties used in hello sample, see documentation in sections following hello samples.</span></span>

<span data-ttu-id="72175-272">**Oracle länkad tjänst:**</span><span class="sxs-lookup"><span data-stu-id="72175-272">**Oracle linked service:**</span></span>
```json
{
    "name": "OnPremisesOracleLinkedService",
    "properties": {
        "type": "OnPremisesOracle",
        "typeProperties": {
            "connectionString": "Data Source=(DESCRIPTION=(ADDRESS=(PROTOCOL=TCP)(HOST=<hostname>)(PORT=<port number>))(CONNECT_DATA=(SERVICE_NAME=<SID>)));
            User Id=<username>;Password=<password>;",
            "gatewayName": "<gateway name>"
        }
    }
}
```

<span data-ttu-id="72175-273">**Azure Blob storage länkade tjänsten:**</span><span class="sxs-lookup"><span data-stu-id="72175-273">**Azure Blob storage linked service:**</span></span>
```json
{
    "name": "StorageLinkedService",
    "properties": {
        "type": "AzureStorage",
        "typeProperties": {
            "connectionString": "DefaultEndpointsProtocol=https;AccountName=<account name>;AccountKey=<Account key>"
        }
    }
}
```

<span data-ttu-id="72175-274">**Azure Blob-inkommande datamängd**</span><span class="sxs-lookup"><span data-stu-id="72175-274">**Azure Blob input dataset**</span></span>

<span data-ttu-id="72175-275">Data hämtas från en ny blob varje timme (frekvens: timme, intervall: 1).</span><span class="sxs-lookup"><span data-stu-id="72175-275">Data is picked up from a new blob every hour (frequency: hour, interval: 1).</span></span> <span data-ttu-id="72175-276">hello mappen sökvägen och filnamnet för hello blob utvärderas dynamiskt baserat på hello starttiden för hello-segment som bearbetas.</span><span class="sxs-lookup"><span data-stu-id="72175-276">hello folder path and file name for hello blob are dynamically evaluated based on hello start time of hello slice that is being processed.</span></span> <span data-ttu-id="72175-277">hello mappsökväg använder år, månad och som en dagdel av hello starttid och filnamn använder hello timme tillhör hello starttid.</span><span class="sxs-lookup"><span data-stu-id="72175-277">hello folder path uses year, month, and day part of hello start time and file name uses hello hour part of hello start time.</span></span> <span data-ttu-id="72175-278">”externa”: ”true” inställningen informerar hello Data Factory-tjänsten att den här tabellen är externa toohello data factory och inte tillverkas av en aktivitet i hello data factory.</span><span class="sxs-lookup"><span data-stu-id="72175-278">“external”: “true” setting informs hello Data Factory service that this table is external toohello data factory and is not produced by an activity in hello data factory.</span></span>

```json
{
    "name": "AzureBlobInput",
    "properties": {
        "type": "AzureBlob",
        "linkedServiceName": "StorageLinkedService",
        "typeProperties": {
            "folderPath": "mycontainer/myfolder/yearno={Year}/monthno={Month}/dayno={Day}",
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
                }
            ],
            "format": {
                "type": "TextFormat",
                "columnDelimiter": ",",
                "rowDelimiter": "\n"
            }
        },
        "external": true,
        "availability": {
            "frequency": "Day",
            "interval": 1
        },
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

<span data-ttu-id="72175-279">**Oracle datamängd för utdata:**</span><span class="sxs-lookup"><span data-stu-id="72175-279">**Oracle output dataset:**</span></span>

<span data-ttu-id="72175-280">hello exemplet förutsätter att du har skapat en tabell ”mytable” som prefix i Oracle.</span><span class="sxs-lookup"><span data-stu-id="72175-280">hello sample assumes you have created a table “MyTable” in Oracle.</span></span> <span data-ttu-id="72175-281">Skapa hello tabell i Oracle med hello samma antal kolumner som du förväntar dig hello Blob CSV-filen toocontain.</span><span class="sxs-lookup"><span data-stu-id="72175-281">Create hello table in Oracle with hello same number of columns as you expect hello Blob CSV file toocontain.</span></span> <span data-ttu-id="72175-282">Nya rader läggs toohello tabell varje timme.</span><span class="sxs-lookup"><span data-stu-id="72175-282">New rows are added toohello table every hour.</span></span>

```json
{
    "name": "OracleOutput",
    "properties": {
        "type": "OracleTable",
        "linkedServiceName": "OnPremisesOracleLinkedService",
        "typeProperties": {
            "tableName": "MyTable"
        },
        "availability": {
            "frequency": "Day",
            "interval": "1"
        }
    }
}
```

<span data-ttu-id="72175-283">**Pipeline med kopieringsaktiviteten:**</span><span class="sxs-lookup"><span data-stu-id="72175-283">**Pipeline with Copy activity:**</span></span>

<span data-ttu-id="72175-284">hello pipelinen innehåller en kopia-aktivitet som är konfigurerade toouse hello inkommande och utgående datauppsättningar och är schemalagda toorun varje timme.</span><span class="sxs-lookup"><span data-stu-id="72175-284">hello pipeline contains a Copy Activity that is configured toouse hello input and output datasets and is scheduled toorun every hour.</span></span> <span data-ttu-id="72175-285">I hello pipeline JSON-definitionen hello **källa** typ har angetts för**BlobSource** och hello **sink** typ har angetts för**OracleSink**.</span><span class="sxs-lookup"><span data-stu-id="72175-285">In hello pipeline JSON definition, hello **source** type is set too**BlobSource** and hello **sink** type is set too**OracleSink**.</span></span>  

```json
{  
    "name":"SamplePipeline",
    "properties":{  
        "start":"2014-06-01T18:00:00",
        "end":"2014-06-05T19:00:00",
        "description":"pipeline with copy activity",
        "activities":[  
            {
                "name": "AzureBlobtoOracle",
                "description": "Copy Activity",
                "type": "Copy",
                "inputs": [
                    {
                        "name": "AzureBlobInput"
                    }
                ],
                "outputs": [
                    {
                        "name": "OracleOutput"
                    }
                ],
                "typeProperties": {
                    "source": {
                        "type": "BlobSource"
                    },
                    "sink": {
                        "type": "OracleSink"
                    }
                },
                "scheduler": {
                    "frequency": "Day",
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


## <a name="troubleshooting-tips"></a><span data-ttu-id="72175-286">Felsökningstips</span><span class="sxs-lookup"><span data-stu-id="72175-286">Troubleshooting tips</span></span>
### <a name="problem-1-net-framework-data-provider"></a><span data-ttu-id="72175-287">Problem 1: .NET Framework dataprovider</span><span class="sxs-lookup"><span data-stu-id="72175-287">Problem 1: .NET Framework Data Provider</span></span>

<span data-ttu-id="72175-288">Du ser följande hello **felmeddelande**:</span><span class="sxs-lookup"><span data-stu-id="72175-288">You see hello following **error message**:</span></span>

    Copy activity met invalid parameters: 'UnknownParameterName', Detailed message: Unable toofind hello requested .Net Framework Data Provider. It may not be installed”.  

<span data-ttu-id="72175-289">**Möjliga orsaker:**</span><span class="sxs-lookup"><span data-stu-id="72175-289">**Possible causes:**</span></span>

1. <span data-ttu-id="72175-290">hello .NET Framework Data Provider för Oracle har inte installerats.</span><span class="sxs-lookup"><span data-stu-id="72175-290">hello .NET Framework Data Provider for Oracle was not installed.</span></span>
2. <span data-ttu-id="72175-291">hello .NET Framework Data Provider för Oracle var installerade too.NET Framework 2.0 och är inte finns i hello .NET Framework 4.0 mappar.</span><span class="sxs-lookup"><span data-stu-id="72175-291">hello .NET Framework Data Provider for Oracle was installed too.NET Framework 2.0 and is not found in hello .NET Framework 4.0 folders.</span></span>

<span data-ttu-id="72175-292">**Lösning/lösning:**</span><span class="sxs-lookup"><span data-stu-id="72175-292">**Resolution/Workaround:**</span></span>

1. <span data-ttu-id="72175-293">Om du inte har installerat hello .NET-Provider för Oracle, [installera den](http://www.oracle.com/technetwork/topics/dotnet/downloads/) och försök hello scenario.</span><span class="sxs-lookup"><span data-stu-id="72175-293">If you haven't installed hello .NET Provider for Oracle, [install it](http://www.oracle.com/technetwork/topics/dotnet/downloads/) and retry hello scenario.</span></span>
2. <span data-ttu-id="72175-294">Om du får felmeddelandet hello även när du har installerat providern hello hello gör du följande steg:</span><span class="sxs-lookup"><span data-stu-id="72175-294">If you get hello error message even after installing hello provider, do hello following steps:</span></span>
   1. <span data-ttu-id="72175-295">Öppna datorn config av .NET 2.0 från hello mapp: <system disk>: \Windows\Microsoft.NET\Framework64\v2.0.50727\CONFIG\machine.config.</span><span class="sxs-lookup"><span data-stu-id="72175-295">Open machine config of .NET 2.0 from hello folder: <system disk>:\Windows\Microsoft.NET\Framework64\v2.0.50727\CONFIG\machine.config.</span></span>
   2. <span data-ttu-id="72175-296">Sök efter **Oracle Data Provider för .NET**, och du bör vara kan toofind en post som visas i följande exempel under hello **system.data** -> **DbProviderFactories**”:<add name="Oracle Data Provider for .NET" invariant="Oracle.DataAccess.Client" description="Oracle dataprovider för .NET</span><span class="sxs-lookup"><span data-stu-id="72175-296">Search for **Oracle Data Provider for .NET**, and you should be able toofind an entry as shown in hello following sample under **system.data** -> **DbProviderFactories**: “<add name="Oracle Data Provider for .NET" invariant="Oracle.DataAccess.Client" description="Oracle Data Provider for .NET</span></span>" type="Oracle.DataAccess.Client.OracleClientFactory, Oracle.DataAccess, Version=2.112.3.0, Culture=neutral, PublicKeyToken=89b483f429c47342" /><span data-ttu-id="72175-297">”</span><span class="sxs-lookup"><span data-stu-id="72175-297">”</span></span>
3. <span data-ttu-id="72175-298">Kopiera den här posten toohello machine.config-filen i hello följande v4.0 mapp: <system disk>: \Windows\Microsoft.NET\Framework64\v4.0.30319\Config\machine.config och ändra hello version too4.xxx.x.x.</span><span class="sxs-lookup"><span data-stu-id="72175-298">Copy this entry toohello machine.config file in hello following v4.0 folder: <system disk>:\Windows\Microsoft.NET\Framework64\v4.0.30319\Config\machine.config, and change hello version too4.xxx.x.x.</span></span>
4. <span data-ttu-id="72175-299">Installera ”< ODP.NET installerad sökväg > \11.2.0\client_1\odp.net\bin\4\Oracle.DataAccess.dll” i hello globala sammansättningscachen (GAC) genom att köra `gacutil /i [provider path]`. ## felsökningstips</span><span class="sxs-lookup"><span data-stu-id="72175-299">Install “<ODP.NET Installed Path>\11.2.0\client_1\odp.net\bin\4\Oracle.DataAccess.dll” into hello global assembly cache (GAC) by running `gacutil /i [provider path]`.## Troubleshooting tips</span></span>

### <a name="problem-2-datetime-formatting"></a><span data-ttu-id="72175-300">Problem 2: datetime formatering</span><span class="sxs-lookup"><span data-stu-id="72175-300">Problem 2: datetime formatting</span></span>

<span data-ttu-id="72175-301">Du ser följande hello **felmeddelande**:</span><span class="sxs-lookup"><span data-stu-id="72175-301">You see hello following **error message**:</span></span>

    Message=Operation failed in Oracle Database with hello following error: 'ORA-01861: literal does not match format string'.,Source=,''Type=Oracle.DataAccess.Client.OracleException,Message=ORA-01861: literal does not match format string,Source=Oracle Data Provider for .NET,'.

<span data-ttu-id="72175-302">**Lösning/lösning:**</span><span class="sxs-lookup"><span data-stu-id="72175-302">**Resolution/Workaround:**</span></span>

<span data-ttu-id="72175-303">Du kanske måste tooadjust hello frågesträng i din kopieringsaktiviteten baserat på hur datum är konfigurerade i Oracle-databas enligt hello följande exempel (med hjälp av funktionen för hello to_date):</span><span class="sxs-lookup"><span data-stu-id="72175-303">You may need tooadjust hello query string in your copy activity based on how dates are configured in your Oracle database, as shown in hello following sample (using hello to_date function):</span></span>

    "oracleReaderQuery": "$$Text.Format('select * from MyTable where timestampcolumn >= to_date(\\'{0:MM-dd-yyyy HH:mm}\\',\\'MM/DD/YYYY HH24:MI\\')  AND timestampcolumn < to_date(\\'{1:MM-dd-yyyy HH:mm}\\',\\'MM/DD/YYYY HH24:MI\\') ', WindowStart, WindowEnd)"


## <a name="type-mapping-for-oracle"></a><span data-ttu-id="72175-304">Mappning för Oracle</span><span class="sxs-lookup"><span data-stu-id="72175-304">Type mapping for Oracle</span></span>
<span data-ttu-id="72175-305">Som anges i hello [data movement aktiviteter](data-factory-data-movement-activities.md) artikel kopieringsaktiviteten utför automatisk konverteringar från källan typer toosink typer med hello följande metod i steg 2:</span><span class="sxs-lookup"><span data-stu-id="72175-305">As mentioned in hello [data movement activities](data-factory-data-movement-activities.md) article Copy activity performs automatic type conversions from source types toosink types with hello following 2-step approach:</span></span>

1. <span data-ttu-id="72175-306">Konvertera från inbyggda typer too.NET källtypen</span><span class="sxs-lookup"><span data-stu-id="72175-306">Convert from native source types too.NET type</span></span>
2. <span data-ttu-id="72175-307">Konvertera från .NET typen toonative Mottagartypen</span><span class="sxs-lookup"><span data-stu-id="72175-307">Convert from .NET type toonative sink type</span></span>

<span data-ttu-id="72175-308">När du flyttar data från Oracle, används följande mappningar hello från Oracle-typen too.NET datatyp och vice versa.</span><span class="sxs-lookup"><span data-stu-id="72175-308">When moving data from Oracle, hello following mappings are used from Oracle data type too.NET type and vice versa.</span></span>

| <span data-ttu-id="72175-309">Oracle-datatyp</span><span class="sxs-lookup"><span data-stu-id="72175-309">Oracle data type</span></span> | <span data-ttu-id="72175-310">.NET framework-datatyp</span><span class="sxs-lookup"><span data-stu-id="72175-310">.NET Framework data type</span></span> |
| --- | --- |
| <span data-ttu-id="72175-311">BFILE</span><span class="sxs-lookup"><span data-stu-id="72175-311">BFILE</span></span> |<span data-ttu-id="72175-312">byte]</span><span class="sxs-lookup"><span data-stu-id="72175-312">Byte[]</span></span> |
| <span data-ttu-id="72175-313">BLOB</span><span class="sxs-lookup"><span data-stu-id="72175-313">BLOB</span></span> |<span data-ttu-id="72175-314">byte]</span><span class="sxs-lookup"><span data-stu-id="72175-314">Byte[]</span></span> |
| <span data-ttu-id="72175-315">CHAR</span><span class="sxs-lookup"><span data-stu-id="72175-315">CHAR</span></span> |<span data-ttu-id="72175-316">Sträng</span><span class="sxs-lookup"><span data-stu-id="72175-316">String</span></span> |
| <span data-ttu-id="72175-317">CLOB</span><span class="sxs-lookup"><span data-stu-id="72175-317">CLOB</span></span> |<span data-ttu-id="72175-318">Sträng</span><span class="sxs-lookup"><span data-stu-id="72175-318">String</span></span> |
| <span data-ttu-id="72175-319">DATUM</span><span class="sxs-lookup"><span data-stu-id="72175-319">DATE</span></span> |<span data-ttu-id="72175-320">Datum och tid</span><span class="sxs-lookup"><span data-stu-id="72175-320">DateTime</span></span> |
| <span data-ttu-id="72175-321">FLYTTAL</span><span class="sxs-lookup"><span data-stu-id="72175-321">FLOAT</span></span> |<span data-ttu-id="72175-322">Decimal, sträng (om precision > 28)</span><span class="sxs-lookup"><span data-stu-id="72175-322">Decimal, String (if precision > 28)</span></span> |
| <span data-ttu-id="72175-323">HELTAL</span><span class="sxs-lookup"><span data-stu-id="72175-323">INTEGER</span></span> |<span data-ttu-id="72175-324">Decimal, sträng (om precision > 28)</span><span class="sxs-lookup"><span data-stu-id="72175-324">Decimal, String (if precision > 28)</span></span> |
| <span data-ttu-id="72175-325">INTERVALL år tooMONTH</span><span class="sxs-lookup"><span data-stu-id="72175-325">INTERVAL YEAR tooMONTH</span></span> |<span data-ttu-id="72175-326">Int32</span><span class="sxs-lookup"><span data-stu-id="72175-326">Int32</span></span> |
| <span data-ttu-id="72175-327">INTERVALL dag tooSECOND</span><span class="sxs-lookup"><span data-stu-id="72175-327">INTERVAL DAY tooSECOND</span></span> |<span data-ttu-id="72175-328">TimeSpan</span><span class="sxs-lookup"><span data-stu-id="72175-328">TimeSpan</span></span> |
| <span data-ttu-id="72175-329">LÅNG</span><span class="sxs-lookup"><span data-stu-id="72175-329">LONG</span></span> |<span data-ttu-id="72175-330">Sträng</span><span class="sxs-lookup"><span data-stu-id="72175-330">String</span></span> |
| <span data-ttu-id="72175-331">LÅNGT RÅDATA</span><span class="sxs-lookup"><span data-stu-id="72175-331">LONG RAW</span></span> |<span data-ttu-id="72175-332">byte]</span><span class="sxs-lookup"><span data-stu-id="72175-332">Byte[]</span></span> |
| <span data-ttu-id="72175-333">NCHAR</span><span class="sxs-lookup"><span data-stu-id="72175-333">NCHAR</span></span> |<span data-ttu-id="72175-334">Sträng</span><span class="sxs-lookup"><span data-stu-id="72175-334">String</span></span> |
| <span data-ttu-id="72175-335">NCLOB</span><span class="sxs-lookup"><span data-stu-id="72175-335">NCLOB</span></span> |<span data-ttu-id="72175-336">Sträng</span><span class="sxs-lookup"><span data-stu-id="72175-336">String</span></span> |
| <span data-ttu-id="72175-337">ANTAL</span><span class="sxs-lookup"><span data-stu-id="72175-337">NUMBER</span></span> |<span data-ttu-id="72175-338">Decimal, sträng (om precision > 28)</span><span class="sxs-lookup"><span data-stu-id="72175-338">Decimal, String (if precision > 28)</span></span> |
| <span data-ttu-id="72175-339">NVARCHAR2</span><span class="sxs-lookup"><span data-stu-id="72175-339">NVARCHAR2</span></span> |<span data-ttu-id="72175-340">Sträng</span><span class="sxs-lookup"><span data-stu-id="72175-340">String</span></span> |
| <span data-ttu-id="72175-341">RÅDATA</span><span class="sxs-lookup"><span data-stu-id="72175-341">RAW</span></span> |<span data-ttu-id="72175-342">byte]</span><span class="sxs-lookup"><span data-stu-id="72175-342">Byte[]</span></span> |
| <span data-ttu-id="72175-343">ROWID</span><span class="sxs-lookup"><span data-stu-id="72175-343">ROWID</span></span> |<span data-ttu-id="72175-344">Sträng</span><span class="sxs-lookup"><span data-stu-id="72175-344">String</span></span> |
| <span data-ttu-id="72175-345">TIDSSTÄMPEL</span><span class="sxs-lookup"><span data-stu-id="72175-345">TIMESTAMP</span></span> |<span data-ttu-id="72175-346">Datum och tid</span><span class="sxs-lookup"><span data-stu-id="72175-346">DateTime</span></span> |
| <span data-ttu-id="72175-347">TIDSSTÄMPEL MED LOKALA TIDSZON</span><span class="sxs-lookup"><span data-stu-id="72175-347">TIMESTAMP WITH LOCAL TIME ZONE</span></span> |<span data-ttu-id="72175-348">Datum och tid</span><span class="sxs-lookup"><span data-stu-id="72175-348">DateTime</span></span> |
| <span data-ttu-id="72175-349">TIDSSTÄMPEL MED TIDSZON</span><span class="sxs-lookup"><span data-stu-id="72175-349">TIMESTAMP WITH TIME ZONE</span></span> |<span data-ttu-id="72175-350">Datum och tid</span><span class="sxs-lookup"><span data-stu-id="72175-350">DateTime</span></span> |
| <span data-ttu-id="72175-351">OSIGNERAT HELTAL</span><span class="sxs-lookup"><span data-stu-id="72175-351">UNSIGNED INTEGER</span></span> |<span data-ttu-id="72175-352">Tal</span><span class="sxs-lookup"><span data-stu-id="72175-352">Number</span></span> |
| <span data-ttu-id="72175-353">VARCHAR2</span><span class="sxs-lookup"><span data-stu-id="72175-353">VARCHAR2</span></span> |<span data-ttu-id="72175-354">Sträng</span><span class="sxs-lookup"><span data-stu-id="72175-354">String</span></span> |
| <span data-ttu-id="72175-355">XML</span><span class="sxs-lookup"><span data-stu-id="72175-355">XML</span></span> |<span data-ttu-id="72175-356">Sträng</span><span class="sxs-lookup"><span data-stu-id="72175-356">String</span></span> |

> [!NOTE]
> <span data-ttu-id="72175-357">Datatypen **intervall år tooMONTH** och **intervall dag tooSECOND** stöds inte när du använder Microsoft-drivrutinen.</span><span class="sxs-lookup"><span data-stu-id="72175-357">Data type **INTERVAL YEAR tooMONTH** and **INTERVAL DAY tooSECOND** are not supported when using Microsoft driver.</span></span>

## <a name="map-source-toosink-columns"></a><span data-ttu-id="72175-358">Mappa källkolumner toosink</span><span class="sxs-lookup"><span data-stu-id="72175-358">Map source toosink columns</span></span>
<span data-ttu-id="72175-359">toolearn mappning tabellkolumner i källan dataset toocolumns i sink dataset finns [mappa dataset kolumner i Azure Data Factory](data-factory-map-columns.md).</span><span class="sxs-lookup"><span data-stu-id="72175-359">toolearn about mapping columns in source dataset toocolumns in sink dataset, see [Mapping dataset columns in Azure Data Factory](data-factory-map-columns.md).</span></span>

## <a name="repeatable-read-from-relational-sources"></a><span data-ttu-id="72175-360">Upprepbar läsning från relationella källor</span><span class="sxs-lookup"><span data-stu-id="72175-360">Repeatable read from relational sources</span></span>
<span data-ttu-id="72175-361">När du kopierar data från relationella datalager, Kom ihåg tooavoid repeterbarhet oönskade resultat.</span><span class="sxs-lookup"><span data-stu-id="72175-361">When copying data from relational data stores, keep repeatability in mind tooavoid unintended outcomes.</span></span> <span data-ttu-id="72175-362">I Azure Data Factory, kan du köra en sektor manuellt.</span><span class="sxs-lookup"><span data-stu-id="72175-362">In Azure Data Factory, you can rerun a slice manually.</span></span> <span data-ttu-id="72175-363">Du kan också konfigurera i principen för en dataset så att ett segment som körs när ett fel uppstår.</span><span class="sxs-lookup"><span data-stu-id="72175-363">You can also configure retry policy for a dataset so that a slice is rerun when a failure occurs.</span></span> <span data-ttu-id="72175-364">När ett segment körs på antingen sätt måste toomake att som hello samma data läses oavsett hur många gånger ett segment körs.</span><span class="sxs-lookup"><span data-stu-id="72175-364">When a slice is rerun in either way, you need toomake sure that hello same data is read no matter how many times a slice is run.</span></span> <span data-ttu-id="72175-365">Se [Repeatable läsa från relationella källor](data-factory-repeatable-copy.md#repeatable-read-from-relational-sources).</span><span class="sxs-lookup"><span data-stu-id="72175-365">See [Repeatable read from relational sources](data-factory-repeatable-copy.md#repeatable-read-from-relational-sources).</span></span>

## <a name="performance-and-tuning"></a><span data-ttu-id="72175-366">Prestanda och finjustering</span><span class="sxs-lookup"><span data-stu-id="72175-366">Performance and Tuning</span></span>
<span data-ttu-id="72175-367">Se [kopiera aktivitet prestanda och justera guiden](data-factory-copy-activity-performance.md) toolearn om nyckeln faktorer som påverkan prestanda för flytt av data (Kopieringsaktiviteten) i Azure Data Factory och olika sätt toooptimize den.</span><span class="sxs-lookup"><span data-stu-id="72175-367">See [Copy Activity Performance & Tuning Guide](data-factory-copy-activity-performance.md) toolearn about key factors that impact performance of data movement (Copy Activity) in Azure Data Factory and various ways toooptimize it.</span></span>
