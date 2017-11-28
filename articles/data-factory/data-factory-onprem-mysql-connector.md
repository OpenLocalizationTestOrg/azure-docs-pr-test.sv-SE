---
title: "aaaMove data från MySQL med Azure Data Factory | Microsoft Docs"
description: "Läs mer om hur toomove data från MySQL-databas med hjälp av Azure Data Factory."
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
ms.openlocfilehash: 3ffe969e42ce1a54b265c4739df43fdc594ea891
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="move-data-from-mysql-using-azure-data-factory"></a><span data-ttu-id="55ccd-103">Flytta data från MySQL med Azure Data Factory</span><span class="sxs-lookup"><span data-stu-id="55ccd-103">Move data From MySQL using Azure Data Factory</span></span>
<span data-ttu-id="55ccd-104">Den här artikeln förklarar hur toouse hello Kopieringsaktiviteten i Azure Data Factory toomove data från en lokal MySQL-databas.</span><span class="sxs-lookup"><span data-stu-id="55ccd-104">This article explains how toouse hello Copy Activity in Azure Data Factory toomove data from an on-premises MySQL database.</span></span> <span data-ttu-id="55ccd-105">Den bygger på hello [Data Movement aktiviteter](data-factory-data-movement-activities.md) artikel som ger en allmän översikt över dataflyttning hello kopieringsaktiviteten.</span><span class="sxs-lookup"><span data-stu-id="55ccd-105">It builds on hello [Data Movement Activities](data-factory-data-movement-activities.md) article, which presents a general overview of data movement with hello copy activity.</span></span>

<span data-ttu-id="55ccd-106">Du kan kopiera data från ett lokalt MySQL data store tooany stöds sink dataarkiv.</span><span class="sxs-lookup"><span data-stu-id="55ccd-106">You can copy data from an on-premises MySQL data store tooany supported sink data store.</span></span> <span data-ttu-id="55ccd-107">En lista över data lagras som stöds när egenskaperna av hello kopieringsaktiviteten Se hello [stöds datalager](data-factory-data-movement-activities.md#supported-data-stores-and-formats) tabell.</span><span class="sxs-lookup"><span data-stu-id="55ccd-107">For a list of data stores supported as sinks by hello copy activity, see hello [Supported data stores](data-factory-data-movement-activities.md#supported-data-stores-and-formats) table.</span></span> <span data-ttu-id="55ccd-108">Data factory stöder för närvarande endast flytta data från en MySQL lagra tooother datalager, men inte för att flytta data från andra lagrar tooan MySQL data dataarkiv.</span><span class="sxs-lookup"><span data-stu-id="55ccd-108">Data factory currently supports only moving data from a MySQL data store tooother data stores, but not for moving data from other data stores tooan MySQL data store.</span></span> 

## <a name="prerequisites"></a><span data-ttu-id="55ccd-109">Krav</span><span class="sxs-lookup"><span data-stu-id="55ccd-109">Prerequisites</span></span>
<span data-ttu-id="55ccd-110">Data Factory-tjänsten stöder anslutande tooon lokala MySQL källor med hello Data Management Gateway.</span><span class="sxs-lookup"><span data-stu-id="55ccd-110">Data Factory service supports connecting tooon-premises MySQL sources using hello Data Management Gateway.</span></span> <span data-ttu-id="55ccd-111">Se [flytta data mellan lokala platser och moln](data-factory-move-data-between-onprem-and-cloud.md) artikel toolearn om Data Management Gateway och stegvisa instruktioner om hur du konfigurerar hello gateway.</span><span class="sxs-lookup"><span data-stu-id="55ccd-111">See [moving data between on-premises locations and cloud](data-factory-move-data-between-onprem-and-cloud.md) article toolearn about Data Management Gateway and step-by-step instructions on setting up hello gateway.</span></span>

<span data-ttu-id="55ccd-112">Gateway krävs även om hello MySQL-databas finns i en Azure IaaS-virtuella (VM).</span><span class="sxs-lookup"><span data-stu-id="55ccd-112">Gateway is required even if hello MySQL database is hosted in an Azure IaaS virtual machine (VM).</span></span> <span data-ttu-id="55ccd-113">Du kan installera hello gateway på hello samma virtuella dator som hello data lagras eller på en annan virtuell dator så länge som hello gateway kan ansluta toohello databas.</span><span class="sxs-lookup"><span data-stu-id="55ccd-113">You can install hello gateway on hello same VM as hello data store or on a different VM as long as hello gateway can connect toohello database.</span></span>

> [!NOTE]
> <span data-ttu-id="55ccd-114">Se [felsökning av problem med gateway](data-factory-data-management-gateway.md#troubleshooting-gateway-issues) tips om hur du felsöker anslutning /-gateway relaterade problem.</span><span class="sxs-lookup"><span data-stu-id="55ccd-114">See [Troubleshoot gateway issues](data-factory-data-management-gateway.md#troubleshooting-gateway-issues) for tips on troubleshooting connection/gateway related issues.</span></span>

## <a name="supported-versions-and-installation"></a><span data-ttu-id="55ccd-115">Versioner som stöds och installation</span><span class="sxs-lookup"><span data-stu-id="55ccd-115">Supported versions and installation</span></span>
<span data-ttu-id="55ccd-116">För Data Management Gateway tooconnect toohello MySQL-databas måste tooinstall hello [MySQL Connector/Net för Microsoft Windows](https://dev.mysql.com/downloads/connector/net/) (version 6.6.5 eller senare) hello på samma system som hello Data Management Gateway.</span><span class="sxs-lookup"><span data-stu-id="55ccd-116">For Data Management Gateway tooconnect toohello MySQL Database, you need tooinstall hello [MySQL Connector/Net for Microsoft Windows](https://dev.mysql.com/downloads/connector/net/) (version 6.6.5 or above) on hello same system as hello Data Management Gateway.</span></span> <span data-ttu-id="55ccd-117">MySQL version 5.1 och senare stöds.</span><span class="sxs-lookup"><span data-stu-id="55ccd-117">MySQL version 5.1 and above is supported.</span></span>

> [!TIP]
> <span data-ttu-id="55ccd-118">Om du klickar på fel ”autentisering misslyckades eftersom hello Fjärrpartnern har stängt hello transport dataström”. bör du överväga att tooupgrade hello MySQL Connector/Net toohigher version.</span><span class="sxs-lookup"><span data-stu-id="55ccd-118">If you hit error on "Authentication failed because hello remote party has closed hello transport stream.", consider tooupgrade hello MySQL Connector/Net toohigher version.</span></span>

## <a name="getting-started"></a><span data-ttu-id="55ccd-119">Komma igång</span><span class="sxs-lookup"><span data-stu-id="55ccd-119">Getting started</span></span>
<span data-ttu-id="55ccd-120">Du kan skapa en pipeline med en kopia-aktivitet som flyttar data från en lokal Cassandra data store med hjälp av olika verktyg/API: er.</span><span class="sxs-lookup"><span data-stu-id="55ccd-120">You can create a pipeline with a copy activity that moves data from an on-premises Cassandra data store by using different tools/APIs.</span></span> 

- <span data-ttu-id="55ccd-121">hello enklaste sättet toocreate en pipeline är toouse hello **guiden Kopiera**.</span><span class="sxs-lookup"><span data-stu-id="55ccd-121">hello easiest way toocreate a pipeline is toouse hello **Copy Wizard**.</span></span> <span data-ttu-id="55ccd-122">Se [Självstudier: skapa en pipeline med hjälp av guiden Kopiera](data-factory-copy-data-wizard-tutorial.md) för en snabb genomgång om hur du skapar en pipeline med hjälp av guiden för hello kopiera data.</span><span class="sxs-lookup"><span data-stu-id="55ccd-122">See [Tutorial: Create a pipeline using Copy Wizard](data-factory-copy-data-wizard-tutorial.md) for a quick walkthrough on creating a pipeline using hello Copy data wizard.</span></span> 
- <span data-ttu-id="55ccd-123">Du kan också använda följande verktyg toocreate en pipeline hello: **Azure-portalen**, **Visual Studio**, **Azure PowerShell**, **Azure Resource Manager-mall** , **.NET API**, och **REST API**.</span><span class="sxs-lookup"><span data-stu-id="55ccd-123">You can also use hello following tools toocreate a pipeline: **Azure portal**, **Visual Studio**, **Azure PowerShell**, **Azure Resource Manager template**, **.NET API**, and **REST API**.</span></span> <span data-ttu-id="55ccd-124">Se [kopiera aktivitet kursen](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) för stegvisa instruktioner toocreate en pipeline med en Kopieringsaktivitet.</span><span class="sxs-lookup"><span data-stu-id="55ccd-124">See [Copy activity tutorial](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) for step-by-step instructions toocreate a pipeline with a copy activity.</span></span> 

<span data-ttu-id="55ccd-125">Om du använder hello verktyg eller API: er kan utföra du hello följande steg toocreate en pipeline som flyttar data från en källdata lagra tooa sink-datalagret:</span><span class="sxs-lookup"><span data-stu-id="55ccd-125">Whether you use hello tools or APIs, you perform hello following steps toocreate a pipeline that moves data from a source data store tooa sink data store:</span></span>

1. <span data-ttu-id="55ccd-126">Skapa **länkade tjänster** toolink indata och utdata lagrar tooyour data factory.</span><span class="sxs-lookup"><span data-stu-id="55ccd-126">Create **linked services** toolink input and output data stores tooyour data factory.</span></span>
2. <span data-ttu-id="55ccd-127">Skapa **datauppsättningar** toorepresent indata och utdata för hello kopieringsåtgärden.</span><span class="sxs-lookup"><span data-stu-id="55ccd-127">Create **datasets** toorepresent input and output data for hello copy operation.</span></span> 
3. <span data-ttu-id="55ccd-128">Skapa en **pipeline** med en kopia-aktivitet som tar en datamängd som indata och en dataset som utdata.</span><span class="sxs-lookup"><span data-stu-id="55ccd-128">Create a **pipeline** with a copy activity that takes a dataset as an input and a dataset as an output.</span></span> 

<span data-ttu-id="55ccd-129">När du använder guiden hello skapas automatiskt JSON definitioner för dessa Data Factory-enheter (länkade tjänster, datauppsättningar och hello pipeline) för dig.</span><span class="sxs-lookup"><span data-stu-id="55ccd-129">When you use hello wizard, JSON definitions for these Data Factory entities (linked services, datasets, and hello pipeline) are automatically created for you.</span></span> <span data-ttu-id="55ccd-130">När du använder Verktyg/API: er (utom .NET API), kan du definiera dessa Data Factory-enheter med hjälp av hello JSON-format.</span><span class="sxs-lookup"><span data-stu-id="55ccd-130">When you use tools/APIs (except .NET API), you define these Data Factory entities by using hello JSON format.</span></span>  <span data-ttu-id="55ccd-131">Ett exempel med JSON-definitioner för Data Factory-entiteter som ska använda toocopy data från ett dataarkiv för lokala MySQL finns [JSON-exempel: kopiera data från MySQL tooAzure Blob](#json-example-copy-data-from-mysql-to-azure-blob) i den här artikeln.</span><span class="sxs-lookup"><span data-stu-id="55ccd-131">For a sample with JSON definitions for Data Factory entities that are used toocopy data from an on-premises MySQL data store, see [JSON example: Copy data from MySQL tooAzure Blob](#json-example-copy-data-from-mysql-to-azure-blob) section of this article.</span></span> 

<span data-ttu-id="55ccd-132">hello följande avsnitt innehåller information om JSON-egenskaper används toodefine Data Factory entiteter specifika tooa MySQL-datalager:</span><span class="sxs-lookup"><span data-stu-id="55ccd-132">hello following sections provide details about JSON properties that are used toodefine Data Factory entities specific tooa MySQL data store:</span></span>

## <a name="linked-service-properties"></a><span data-ttu-id="55ccd-133">Länkad tjänstegenskaper</span><span class="sxs-lookup"><span data-stu-id="55ccd-133">Linked service properties</span></span>
<span data-ttu-id="55ccd-134">hello följande tabell innehåller en beskrivning för JSON-element specifika tooMySQL länkad tjänst.</span><span class="sxs-lookup"><span data-stu-id="55ccd-134">hello following table provides description for JSON elements specific tooMySQL linked service.</span></span>

| <span data-ttu-id="55ccd-135">Egenskap</span><span class="sxs-lookup"><span data-stu-id="55ccd-135">Property</span></span> | <span data-ttu-id="55ccd-136">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="55ccd-136">Description</span></span> | <span data-ttu-id="55ccd-137">Krävs</span><span class="sxs-lookup"><span data-stu-id="55ccd-137">Required</span></span> |
| --- | --- | --- |
| <span data-ttu-id="55ccd-138">typ</span><span class="sxs-lookup"><span data-stu-id="55ccd-138">type</span></span> |<span data-ttu-id="55ccd-139">hello Typegenskapen måste anges till: **OnPremisesMySql**</span><span class="sxs-lookup"><span data-stu-id="55ccd-139">hello type property must be set to: **OnPremisesMySql**</span></span> |<span data-ttu-id="55ccd-140">Ja</span><span class="sxs-lookup"><span data-stu-id="55ccd-140">Yes</span></span> |
| <span data-ttu-id="55ccd-141">server</span><span class="sxs-lookup"><span data-stu-id="55ccd-141">server</span></span> |<span data-ttu-id="55ccd-142">Namnet på hello MySQL-server.</span><span class="sxs-lookup"><span data-stu-id="55ccd-142">Name of hello MySQL server.</span></span> |<span data-ttu-id="55ccd-143">Ja</span><span class="sxs-lookup"><span data-stu-id="55ccd-143">Yes</span></span> |
| <span data-ttu-id="55ccd-144">Databasen</span><span class="sxs-lookup"><span data-stu-id="55ccd-144">database</span></span> |<span data-ttu-id="55ccd-145">Namnet på hello MySQL-databas.</span><span class="sxs-lookup"><span data-stu-id="55ccd-145">Name of hello MySQL database.</span></span> |<span data-ttu-id="55ccd-146">Ja</span><span class="sxs-lookup"><span data-stu-id="55ccd-146">Yes</span></span> |
| <span data-ttu-id="55ccd-147">Schemat</span><span class="sxs-lookup"><span data-stu-id="55ccd-147">schema</span></span> |<span data-ttu-id="55ccd-148">Namnet på hello schema i hello-databasen.</span><span class="sxs-lookup"><span data-stu-id="55ccd-148">Name of hello schema in hello database.</span></span> |<span data-ttu-id="55ccd-149">Nej</span><span class="sxs-lookup"><span data-stu-id="55ccd-149">No</span></span> |
| <span data-ttu-id="55ccd-150">AuthenticationType</span><span class="sxs-lookup"><span data-stu-id="55ccd-150">authenticationType</span></span> |<span data-ttu-id="55ccd-151">Typ av autentisering används tooconnect toohello MySQL-databas.</span><span class="sxs-lookup"><span data-stu-id="55ccd-151">Type of authentication used tooconnect toohello MySQL database.</span></span> <span data-ttu-id="55ccd-152">Möjliga värden är: `Basic`.</span><span class="sxs-lookup"><span data-stu-id="55ccd-152">Possible values are: `Basic`.</span></span> |<span data-ttu-id="55ccd-153">Ja</span><span class="sxs-lookup"><span data-stu-id="55ccd-153">Yes</span></span> |
| <span data-ttu-id="55ccd-154">användarnamn</span><span class="sxs-lookup"><span data-stu-id="55ccd-154">username</span></span> |<span data-ttu-id="55ccd-155">Ange användarens namn tooconnect toohello MySQL-databas.</span><span class="sxs-lookup"><span data-stu-id="55ccd-155">Specify user name tooconnect toohello MySQL database.</span></span> |<span data-ttu-id="55ccd-156">Ja</span><span class="sxs-lookup"><span data-stu-id="55ccd-156">Yes</span></span> |
| <span data-ttu-id="55ccd-157">lösenord</span><span class="sxs-lookup"><span data-stu-id="55ccd-157">password</span></span> |<span data-ttu-id="55ccd-158">Ange lösenord för hello-användarkonto som du angav.</span><span class="sxs-lookup"><span data-stu-id="55ccd-158">Specify password for hello user account you specified.</span></span> |<span data-ttu-id="55ccd-159">Ja</span><span class="sxs-lookup"><span data-stu-id="55ccd-159">Yes</span></span> |
| <span data-ttu-id="55ccd-160">gatewayName</span><span class="sxs-lookup"><span data-stu-id="55ccd-160">gatewayName</span></span> |<span data-ttu-id="55ccd-161">Namnet på hello-gateway som hello Data Factory-tjänsten ska använda tooconnect toohello lokala MySQL-databas.</span><span class="sxs-lookup"><span data-stu-id="55ccd-161">Name of hello gateway that hello Data Factory service should use tooconnect toohello on-premises MySQL database.</span></span> |<span data-ttu-id="55ccd-162">Ja</span><span class="sxs-lookup"><span data-stu-id="55ccd-162">Yes</span></span> |

## <a name="dataset-properties"></a><span data-ttu-id="55ccd-163">Egenskaper för datamängd</span><span class="sxs-lookup"><span data-stu-id="55ccd-163">Dataset properties</span></span>
<span data-ttu-id="55ccd-164">En fullständig lista över egenskaper som är tillgängliga för att definiera datauppsättningarna & avsnitt finns hello [skapa datauppsättningar](data-factory-create-datasets.md) artikel.</span><span class="sxs-lookup"><span data-stu-id="55ccd-164">For a full list of sections & properties available for defining datasets, see hello [Creating datasets](data-factory-create-datasets.md) article.</span></span> <span data-ttu-id="55ccd-165">Avsnitt som struktur, tillgänglighet och princip på en datamängd JSON är liknande för alla typer av dataset (Azure SQL Azure blob, Azure-tabellen, osv.).</span><span class="sxs-lookup"><span data-stu-id="55ccd-165">Sections such as structure, availability, and policy of a dataset JSON are similar for all dataset types (Azure SQL, Azure blob, Azure table, etc.).</span></span>

<span data-ttu-id="55ccd-166">Hej **typeProperties** avsnitt är olika för varje typ av dataset och ger information om hello platsen för hello data i datalagret hello.</span><span class="sxs-lookup"><span data-stu-id="55ccd-166">hello **typeProperties** section is different for each type of dataset and provides information about hello location of hello data in hello data store.</span></span> <span data-ttu-id="55ccd-167">Hej typeProperties avsnittet för dataset av typen **RelationalTable** (som omfattar MySQL dataset) har hello följande egenskaper</span><span class="sxs-lookup"><span data-stu-id="55ccd-167">hello typeProperties section for dataset of type **RelationalTable** (which includes MySQL dataset) has hello following properties</span></span>

| <span data-ttu-id="55ccd-168">Egenskap</span><span class="sxs-lookup"><span data-stu-id="55ccd-168">Property</span></span> | <span data-ttu-id="55ccd-169">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="55ccd-169">Description</span></span> | <span data-ttu-id="55ccd-170">Krävs</span><span class="sxs-lookup"><span data-stu-id="55ccd-170">Required</span></span> |
| --- | --- | --- |
| <span data-ttu-id="55ccd-171">tableName</span><span class="sxs-lookup"><span data-stu-id="55ccd-171">tableName</span></span> |<span data-ttu-id="55ccd-172">Namnet på hello tabell i hello MySQL-databasinstans som den länkade tjänsten refererar till.</span><span class="sxs-lookup"><span data-stu-id="55ccd-172">Name of hello table in hello MySQL Database instance that linked service refers to.</span></span> |<span data-ttu-id="55ccd-173">Nej (om **frågan** av **RelationalSource** har angetts)</span><span class="sxs-lookup"><span data-stu-id="55ccd-173">No (if **query** of **RelationalSource** is specified)</span></span> |

## <a name="copy-activity-properties"></a><span data-ttu-id="55ccd-174">Kopiera egenskaper för aktivitet</span><span class="sxs-lookup"><span data-stu-id="55ccd-174">Copy activity properties</span></span>
<span data-ttu-id="55ccd-175">En fullständig lista över avsnitt & egenskaper som är tillgängliga för att definiera aktiviteter finns hello [skapar Pipelines](data-factory-create-pipelines.md) artikel.</span><span class="sxs-lookup"><span data-stu-id="55ccd-175">For a full list of sections & properties available for defining activities, see hello [Creating Pipelines](data-factory-create-pipelines.md) article.</span></span> <span data-ttu-id="55ccd-176">Egenskaper, till exempel namn, beskrivning, inkommande och utgående tabeller är principer är tillgängliga för alla typer av aktiviteter.</span><span class="sxs-lookup"><span data-stu-id="55ccd-176">Properties such as name, description, input and output tables, are policies are available for all types of activities.</span></span>

<span data-ttu-id="55ccd-177">Medan egenskaper som är tillgängliga i hello **typeProperties** avsnittet hello aktivitet varierar med varje aktivitetstyp.</span><span class="sxs-lookup"><span data-stu-id="55ccd-177">Whereas, properties available in hello **typeProperties** section of hello activity vary with each activity type.</span></span> <span data-ttu-id="55ccd-178">För Kopieringsaktivitet kan varierar de beroende på hello typer av datakällor och sänkor.</span><span class="sxs-lookup"><span data-stu-id="55ccd-178">For Copy activity, they vary depending on hello types of sources and sinks.</span></span>

<span data-ttu-id="55ccd-179">När datakällan i en Kopieringsaktivitet är av typen **RelationalSource** (som omfattar MySQL), hello följande egenskaper finns i avsnittet typeProperties:</span><span class="sxs-lookup"><span data-stu-id="55ccd-179">When source in copy activity is of type **RelationalSource** (which includes MySQL), hello following properties are available in typeProperties section:</span></span>

| <span data-ttu-id="55ccd-180">Egenskap</span><span class="sxs-lookup"><span data-stu-id="55ccd-180">Property</span></span> | <span data-ttu-id="55ccd-181">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="55ccd-181">Description</span></span> | <span data-ttu-id="55ccd-182">Tillåtna värden</span><span class="sxs-lookup"><span data-stu-id="55ccd-182">Allowed values</span></span> | <span data-ttu-id="55ccd-183">Krävs</span><span class="sxs-lookup"><span data-stu-id="55ccd-183">Required</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="55ccd-184">DocumentDB</span><span class="sxs-lookup"><span data-stu-id="55ccd-184">query</span></span> |<span data-ttu-id="55ccd-185">Använda hello anpassad fråga tooread data.</span><span class="sxs-lookup"><span data-stu-id="55ccd-185">Use hello custom query tooread data.</span></span> |<span data-ttu-id="55ccd-186">SQL-sträng.</span><span class="sxs-lookup"><span data-stu-id="55ccd-186">SQL query string.</span></span> <span data-ttu-id="55ccd-187">Till exempel: Välj * från mytable prefix.</span><span class="sxs-lookup"><span data-stu-id="55ccd-187">For example: select * from MyTable.</span></span> |<span data-ttu-id="55ccd-188">Nej (om **tableName** av **dataset** har angetts)</span><span class="sxs-lookup"><span data-stu-id="55ccd-188">No (if **tableName** of **dataset** is specified)</span></span> |


## <a name="json-example-copy-data-from-mysql-tooazure-blob"></a><span data-ttu-id="55ccd-189">JSON-exempel: kopiera data från MySQL tooAzure Blob</span><span class="sxs-lookup"><span data-stu-id="55ccd-189">JSON example: Copy data from MySQL tooAzure Blob</span></span>
<span data-ttu-id="55ccd-190">Det här exemplet innehåller exempel JSON definitioner som du kan använda toocreate en pipeline med hjälp av [Azure-portalen](data-factory-copy-activity-tutorial-using-azure-portal.md) eller [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md) eller [Azure PowerShell](data-factory-copy-activity-tutorial-using-powershell.md).</span><span class="sxs-lookup"><span data-stu-id="55ccd-190">This example provides sample JSON definitions that you can use toocreate a pipeline by using [Azure portal](data-factory-copy-activity-tutorial-using-azure-portal.md) or [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md) or [Azure PowerShell](data-factory-copy-activity-tutorial-using-powershell.md).</span></span> <span data-ttu-id="55ccd-191">Den visar hur toocopy data från en lokal MySQL-databas tooan Azure Blob Storage.</span><span class="sxs-lookup"><span data-stu-id="55ccd-191">It shows how toocopy data from an on-premises MySQL database tooan Azure Blob Storage.</span></span> <span data-ttu-id="55ccd-192">Data kan dock vara kopierade tooany av hello sänkor anges [här](data-factory-data-movement-activities.md#supported-data-stores-and-formats) med hello Kopieringsaktiviteten i Azure Data Factory.</span><span class="sxs-lookup"><span data-stu-id="55ccd-192">However, data can be copied tooany of hello sinks stated [here](data-factory-data-movement-activities.md#supported-data-stores-and-formats) using hello Copy Activity in Azure Data Factory.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="55ccd-193">Det här exemplet innehåller JSON kodavsnitt.</span><span class="sxs-lookup"><span data-stu-id="55ccd-193">This sample provides JSON snippets.</span></span> <span data-ttu-id="55ccd-194">Stegvisa instruktioner för att skapa datafabriken hello inkluderas inte.</span><span class="sxs-lookup"><span data-stu-id="55ccd-194">It does not include step-by-step instructions for creating hello data factory.</span></span> <span data-ttu-id="55ccd-195">Se [flytta data mellan lokala platser och moln](data-factory-move-data-between-onprem-and-cloud.md) artikel stegvisa instruktioner.</span><span class="sxs-lookup"><span data-stu-id="55ccd-195">See [moving data between on-premises locations and cloud](data-factory-move-data-between-onprem-and-cloud.md) article for step-by-step instructions.</span></span>

<span data-ttu-id="55ccd-196">hello exemplet har hello följande data factory-enheter:</span><span class="sxs-lookup"><span data-stu-id="55ccd-196">hello sample has hello following data factory entities:</span></span>

1. <span data-ttu-id="55ccd-197">En länkad tjänst av typen [OnPremisesMySql](data-factory-onprem-mysql-connector.md#linked-service-properties).</span><span class="sxs-lookup"><span data-stu-id="55ccd-197">A linked service of type [OnPremisesMySql](data-factory-onprem-mysql-connector.md#linked-service-properties).</span></span>
2. <span data-ttu-id="55ccd-198">En länkad tjänst av typen [AzureStorage](data-factory-azure-blob-connector.md#linked-service-properties).</span><span class="sxs-lookup"><span data-stu-id="55ccd-198">A linked service of type [AzureStorage](data-factory-azure-blob-connector.md#linked-service-properties).</span></span>
3. <span data-ttu-id="55ccd-199">Indata [dataset](data-factory-create-datasets.md) av typen [RelationalTable](data-factory-onprem-mysql-connector.md#dataset-properties).</span><span class="sxs-lookup"><span data-stu-id="55ccd-199">An input [dataset](data-factory-create-datasets.md) of type [RelationalTable](data-factory-onprem-mysql-connector.md#dataset-properties).</span></span>
4. <span data-ttu-id="55ccd-200">Utdata [dataset](data-factory-create-datasets.md) av typen [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties).</span><span class="sxs-lookup"><span data-stu-id="55ccd-200">An output [dataset](data-factory-create-datasets.md) of type [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties).</span></span>
5. <span data-ttu-id="55ccd-201">En [pipeline](data-factory-create-pipelines.md) med Kopieringsaktiviteten som använder [RelationalSource](data-factory-onprem-mysql-connector.md#copy-activity-properties) och [BlobSink](data-factory-azure-blob-connector.md#copy-activity-properties).</span><span class="sxs-lookup"><span data-stu-id="55ccd-201">A [pipeline](data-factory-create-pipelines.md) with Copy Activity that uses [RelationalSource](data-factory-onprem-mysql-connector.md#copy-activity-properties) and [BlobSink](data-factory-azure-blob-connector.md#copy-activity-properties).</span></span>

<span data-ttu-id="55ccd-202">hello exemplet kopierar data från ett frågeresultat i MySQL-databas tooa blob varje timme.</span><span class="sxs-lookup"><span data-stu-id="55ccd-202">hello sample copies data from a query result in MySQL database tooa blob hourly.</span></span> <span data-ttu-id="55ccd-203">hello JSON egenskaper som används i exemplen beskrivs i hello-exempel i följande avsnitt.</span><span class="sxs-lookup"><span data-stu-id="55ccd-203">hello JSON properties used in these samples are described in sections following hello samples.</span></span>

<span data-ttu-id="55ccd-204">Som ett första steg bör du konfigurera hello data management gateway.</span><span class="sxs-lookup"><span data-stu-id="55ccd-204">As a first step, setup hello data management gateway.</span></span> <span data-ttu-id="55ccd-205">hello anvisningar finns i hello [flytta data mellan lokala platser och moln](data-factory-move-data-between-onprem-and-cloud.md) artikel.</span><span class="sxs-lookup"><span data-stu-id="55ccd-205">hello instructions are in hello [moving data between on-premises locations and cloud](data-factory-move-data-between-onprem-and-cloud.md) article.</span></span>

<span data-ttu-id="55ccd-206">**MySQL länkade tjänsten:**</span><span class="sxs-lookup"><span data-stu-id="55ccd-206">**MySQL linked service:**</span></span>

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

<span data-ttu-id="55ccd-207">**Azure Storage länkade tjänsten:**</span><span class="sxs-lookup"><span data-stu-id="55ccd-207">**Azure Storage linked service:**</span></span>

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

<span data-ttu-id="55ccd-208">**MySQL inkommande datauppsättningen:**</span><span class="sxs-lookup"><span data-stu-id="55ccd-208">**MySQL input dataset:**</span></span>

<span data-ttu-id="55ccd-209">hello exemplet förutsätter att du har skapat en tabell ”mytable” som prefix i MySQL och innehåller en kolumn med namnet ”timestampcolumn” för tid series-data.</span><span class="sxs-lookup"><span data-stu-id="55ccd-209">hello sample assumes you have created a table “MyTable” in MySQL and it contains a column called “timestampcolumn” for time series data.</span></span>

<span data-ttu-id="55ccd-210">Inställningen ”externa”: ”true” informerar hello Data Factory-tjänsten hello tabellen är externa toohello data factory och inte tillverkas av en aktivitet i hello data factory.</span><span class="sxs-lookup"><span data-stu-id="55ccd-210">Setting “external”: ”true” informs hello Data Factory service that hello table is external toohello data factory and is not produced by an activity in hello data factory.</span></span>

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

<span data-ttu-id="55ccd-211">**Azure Blob utdatauppsättningen:**</span><span class="sxs-lookup"><span data-stu-id="55ccd-211">**Azure Blob output dataset:**</span></span>

<span data-ttu-id="55ccd-212">Data skrivs tooa nya blob varje timme (frekvens: timme, intervall: 1).</span><span class="sxs-lookup"><span data-stu-id="55ccd-212">Data is written tooa new blob every hour (frequency: hour, interval: 1).</span></span> <span data-ttu-id="55ccd-213">hello mappsökväg för hello blob utvärderas dynamiskt baserat på hello starttiden för hello-segment som bearbetas.</span><span class="sxs-lookup"><span data-stu-id="55ccd-213">hello folder path for hello blob is dynamically evaluated based on hello start time of hello slice that is being processed.</span></span> <span data-ttu-id="55ccd-214">hello mappsökväg använder år, månad, dag och timmar delar av hello starttid.</span><span class="sxs-lookup"><span data-stu-id="55ccd-214">hello folder path uses year, month, day, and hours parts of hello start time.</span></span>

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

<span data-ttu-id="55ccd-215">**Pipeline med kopieringsaktiviteten:**</span><span class="sxs-lookup"><span data-stu-id="55ccd-215">**Pipeline with Copy activity:**</span></span>

<span data-ttu-id="55ccd-216">hello pipelinen innehåller en kopia-aktivitet som är konfigurerade toouse hello inkommande och utgående datauppsättningar och är schemalagda toorun varje timme.</span><span class="sxs-lookup"><span data-stu-id="55ccd-216">hello pipeline contains a Copy Activity that is configured toouse hello input and output datasets and is scheduled toorun every hour.</span></span> <span data-ttu-id="55ccd-217">I hello pipeline JSON-definitionen hello **källa** typ har angetts för**RelationalSource** och **sink** typ har angetts för**BlobSink**.</span><span class="sxs-lookup"><span data-stu-id="55ccd-217">In hello pipeline JSON definition, hello **source** type is set too**RelationalSource** and **sink** type is set too**BlobSink**.</span></span> <span data-ttu-id="55ccd-218">hello SQL-frågan som angetts för hello **frågan** egenskapen väljer hello data i hello tidigare timme toocopy.</span><span class="sxs-lookup"><span data-stu-id="55ccd-218">hello SQL query specified for hello **query** property selects hello data in hello past hour toocopy.</span></span>

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


### <a name="type-mapping-for-mysql"></a><span data-ttu-id="55ccd-219">Mappning för MySQL</span><span class="sxs-lookup"><span data-stu-id="55ccd-219">Type mapping for MySQL</span></span>
<span data-ttu-id="55ccd-220">Som anges i hello [data movement aktiviteter](data-factory-data-movement-activities.md) artikeln kopieringsaktiviteten utför automatisk konverteringar från källan typer toosink typer med hello följande två sätt:</span><span class="sxs-lookup"><span data-stu-id="55ccd-220">As mentioned in hello [data movement activities](data-factory-data-movement-activities.md) article, Copy activity performs automatic type conversions from source types toosink types with hello following two-step approach:</span></span>

1. <span data-ttu-id="55ccd-221">Konvertera från inbyggda typer too.NET källtypen</span><span class="sxs-lookup"><span data-stu-id="55ccd-221">Convert from native source types too.NET type</span></span>
2. <span data-ttu-id="55ccd-222">Konvertera från .NET typen toonative Mottagartypen</span><span class="sxs-lookup"><span data-stu-id="55ccd-222">Convert from .NET type toonative sink type</span></span>

<span data-ttu-id="55ccd-223">När du flyttar data tooMySQL som hello följande mappningar används från MySQL typer too.NET typer.</span><span class="sxs-lookup"><span data-stu-id="55ccd-223">When moving data tooMySQL, hello following mappings are used from MySQL types too.NET types.</span></span>

| <span data-ttu-id="55ccd-224">Typ av MySQL-databas</span><span class="sxs-lookup"><span data-stu-id="55ccd-224">MySQL Database type</span></span> | <span data-ttu-id="55ccd-225">.NET framework-typ</span><span class="sxs-lookup"><span data-stu-id="55ccd-225">.NET Framework type</span></span> |
| --- | --- |
| <span data-ttu-id="55ccd-226">bigint osignerade</span><span class="sxs-lookup"><span data-stu-id="55ccd-226">bigint unsigned</span></span> |<span data-ttu-id="55ccd-227">Decimal</span><span class="sxs-lookup"><span data-stu-id="55ccd-227">Decimal</span></span> |
| <span data-ttu-id="55ccd-228">bigint</span><span class="sxs-lookup"><span data-stu-id="55ccd-228">bigint</span></span> |<span data-ttu-id="55ccd-229">Int64</span><span class="sxs-lookup"><span data-stu-id="55ccd-229">Int64</span></span> |
| <span data-ttu-id="55ccd-230">bitar</span><span class="sxs-lookup"><span data-stu-id="55ccd-230">bit</span></span> |<span data-ttu-id="55ccd-231">Decimal</span><span class="sxs-lookup"><span data-stu-id="55ccd-231">Decimal</span></span> |
| <span data-ttu-id="55ccd-232">BLOB</span><span class="sxs-lookup"><span data-stu-id="55ccd-232">blob</span></span> |<span data-ttu-id="55ccd-233">byte]</span><span class="sxs-lookup"><span data-stu-id="55ccd-233">Byte[]</span></span> |
| <span data-ttu-id="55ccd-234">bool</span><span class="sxs-lookup"><span data-stu-id="55ccd-234">bool</span></span> |<span data-ttu-id="55ccd-235">Booleskt värde</span><span class="sxs-lookup"><span data-stu-id="55ccd-235">Boolean</span></span> |
| <span data-ttu-id="55ccd-236">Char</span><span class="sxs-lookup"><span data-stu-id="55ccd-236">char</span></span> |<span data-ttu-id="55ccd-237">Sträng</span><span class="sxs-lookup"><span data-stu-id="55ccd-237">String</span></span> |
| <span data-ttu-id="55ccd-238">Datum</span><span class="sxs-lookup"><span data-stu-id="55ccd-238">date</span></span> |<span data-ttu-id="55ccd-239">Datum och tid</span><span class="sxs-lookup"><span data-stu-id="55ccd-239">Datetime</span></span> |
| <span data-ttu-id="55ccd-240">Datum och tid</span><span class="sxs-lookup"><span data-stu-id="55ccd-240">datetime</span></span> |<span data-ttu-id="55ccd-241">Datum och tid</span><span class="sxs-lookup"><span data-stu-id="55ccd-241">Datetime</span></span> |
| <span data-ttu-id="55ccd-242">Decimal</span><span class="sxs-lookup"><span data-stu-id="55ccd-242">decimal</span></span> |<span data-ttu-id="55ccd-243">Decimal</span><span class="sxs-lookup"><span data-stu-id="55ccd-243">Decimal</span></span> |
| <span data-ttu-id="55ccd-244">dubbel precision</span><span class="sxs-lookup"><span data-stu-id="55ccd-244">double precision</span></span> |<span data-ttu-id="55ccd-245">dubbla</span><span class="sxs-lookup"><span data-stu-id="55ccd-245">Double</span></span> |
| <span data-ttu-id="55ccd-246">dubbla</span><span class="sxs-lookup"><span data-stu-id="55ccd-246">double</span></span> |<span data-ttu-id="55ccd-247">dubbla</span><span class="sxs-lookup"><span data-stu-id="55ccd-247">Double</span></span> |
| <span data-ttu-id="55ccd-248">Enum</span><span class="sxs-lookup"><span data-stu-id="55ccd-248">enum</span></span> |<span data-ttu-id="55ccd-249">Sträng</span><span class="sxs-lookup"><span data-stu-id="55ccd-249">String</span></span> |
| <span data-ttu-id="55ccd-250">flyttal</span><span class="sxs-lookup"><span data-stu-id="55ccd-250">float</span></span> |<span data-ttu-id="55ccd-251">Enskild</span><span class="sxs-lookup"><span data-stu-id="55ccd-251">Single</span></span> |
| <span data-ttu-id="55ccd-252">int osignerade</span><span class="sxs-lookup"><span data-stu-id="55ccd-252">int unsigned</span></span> |<span data-ttu-id="55ccd-253">Int64</span><span class="sxs-lookup"><span data-stu-id="55ccd-253">Int64</span></span> |
| <span data-ttu-id="55ccd-254">int</span><span class="sxs-lookup"><span data-stu-id="55ccd-254">int</span></span> |<span data-ttu-id="55ccd-255">Int32</span><span class="sxs-lookup"><span data-stu-id="55ccd-255">Int32</span></span> |
| <span data-ttu-id="55ccd-256">heltal osignerade</span><span class="sxs-lookup"><span data-stu-id="55ccd-256">integer unsigned</span></span> |<span data-ttu-id="55ccd-257">Int64</span><span class="sxs-lookup"><span data-stu-id="55ccd-257">Int64</span></span> |
| <span data-ttu-id="55ccd-258">heltal</span><span class="sxs-lookup"><span data-stu-id="55ccd-258">integer</span></span> |<span data-ttu-id="55ccd-259">Int32</span><span class="sxs-lookup"><span data-stu-id="55ccd-259">Int32</span></span> |
| <span data-ttu-id="55ccd-260">lång varbinary</span><span class="sxs-lookup"><span data-stu-id="55ccd-260">long varbinary</span></span> |<span data-ttu-id="55ccd-261">byte]</span><span class="sxs-lookup"><span data-stu-id="55ccd-261">Byte[]</span></span> |
| <span data-ttu-id="55ccd-262">lång varchar</span><span class="sxs-lookup"><span data-stu-id="55ccd-262">long varchar</span></span> |<span data-ttu-id="55ccd-263">Sträng</span><span class="sxs-lookup"><span data-stu-id="55ccd-263">String</span></span> |
| <span data-ttu-id="55ccd-264">longblob</span><span class="sxs-lookup"><span data-stu-id="55ccd-264">longblob</span></span> |<span data-ttu-id="55ccd-265">byte]</span><span class="sxs-lookup"><span data-stu-id="55ccd-265">Byte[]</span></span> |
| <span data-ttu-id="55ccd-266">LONGTEXT</span><span class="sxs-lookup"><span data-stu-id="55ccd-266">longtext</span></span> |<span data-ttu-id="55ccd-267">Sträng</span><span class="sxs-lookup"><span data-stu-id="55ccd-267">String</span></span> |
| <span data-ttu-id="55ccd-268">mediumblob</span><span class="sxs-lookup"><span data-stu-id="55ccd-268">mediumblob</span></span> |<span data-ttu-id="55ccd-269">byte]</span><span class="sxs-lookup"><span data-stu-id="55ccd-269">Byte[]</span></span> |
| <span data-ttu-id="55ccd-270">mediumint osignerade</span><span class="sxs-lookup"><span data-stu-id="55ccd-270">mediumint unsigned</span></span> |<span data-ttu-id="55ccd-271">Int64</span><span class="sxs-lookup"><span data-stu-id="55ccd-271">Int64</span></span> |
| <span data-ttu-id="55ccd-272">mediumint</span><span class="sxs-lookup"><span data-stu-id="55ccd-272">mediumint</span></span> |<span data-ttu-id="55ccd-273">Int32</span><span class="sxs-lookup"><span data-stu-id="55ccd-273">Int32</span></span> |
| <span data-ttu-id="55ccd-274">mediumtext</span><span class="sxs-lookup"><span data-stu-id="55ccd-274">mediumtext</span></span> |<span data-ttu-id="55ccd-275">Sträng</span><span class="sxs-lookup"><span data-stu-id="55ccd-275">String</span></span> |
| <span data-ttu-id="55ccd-276">numeriskt</span><span class="sxs-lookup"><span data-stu-id="55ccd-276">numeric</span></span> |<span data-ttu-id="55ccd-277">Decimal</span><span class="sxs-lookup"><span data-stu-id="55ccd-277">Decimal</span></span> |
| <span data-ttu-id="55ccd-278">Verklig</span><span class="sxs-lookup"><span data-stu-id="55ccd-278">real</span></span> |<span data-ttu-id="55ccd-279">dubbla</span><span class="sxs-lookup"><span data-stu-id="55ccd-279">Double</span></span> |
| <span data-ttu-id="55ccd-280">Ange</span><span class="sxs-lookup"><span data-stu-id="55ccd-280">set</span></span> |<span data-ttu-id="55ccd-281">Sträng</span><span class="sxs-lookup"><span data-stu-id="55ccd-281">String</span></span> |
| <span data-ttu-id="55ccd-282">smallint osignerade</span><span class="sxs-lookup"><span data-stu-id="55ccd-282">smallint unsigned</span></span> |<span data-ttu-id="55ccd-283">Int32</span><span class="sxs-lookup"><span data-stu-id="55ccd-283">Int32</span></span> |
| <span data-ttu-id="55ccd-284">smallint</span><span class="sxs-lookup"><span data-stu-id="55ccd-284">smallint</span></span> |<span data-ttu-id="55ccd-285">Int16</span><span class="sxs-lookup"><span data-stu-id="55ccd-285">Int16</span></span> |
| <span data-ttu-id="55ccd-286">Text</span><span class="sxs-lookup"><span data-stu-id="55ccd-286">text</span></span> |<span data-ttu-id="55ccd-287">Sträng</span><span class="sxs-lookup"><span data-stu-id="55ccd-287">String</span></span> |
| <span data-ttu-id="55ccd-288">time</span><span class="sxs-lookup"><span data-stu-id="55ccd-288">time</span></span> |<span data-ttu-id="55ccd-289">TimeSpan</span><span class="sxs-lookup"><span data-stu-id="55ccd-289">TimeSpan</span></span> |
| <span data-ttu-id="55ccd-290">tidsstämpel</span><span class="sxs-lookup"><span data-stu-id="55ccd-290">timestamp</span></span> |<span data-ttu-id="55ccd-291">Datum och tid</span><span class="sxs-lookup"><span data-stu-id="55ccd-291">Datetime</span></span> |
| <span data-ttu-id="55ccd-292">tinyblob</span><span class="sxs-lookup"><span data-stu-id="55ccd-292">tinyblob</span></span> |<span data-ttu-id="55ccd-293">byte]</span><span class="sxs-lookup"><span data-stu-id="55ccd-293">Byte[]</span></span> |
| <span data-ttu-id="55ccd-294">tinyint osignerade</span><span class="sxs-lookup"><span data-stu-id="55ccd-294">tinyint unsigned</span></span> |<span data-ttu-id="55ccd-295">Int16</span><span class="sxs-lookup"><span data-stu-id="55ccd-295">Int16</span></span> |
| <span data-ttu-id="55ccd-296">tinyint</span><span class="sxs-lookup"><span data-stu-id="55ccd-296">tinyint</span></span> |<span data-ttu-id="55ccd-297">Int16</span><span class="sxs-lookup"><span data-stu-id="55ccd-297">Int16</span></span> |
| <span data-ttu-id="55ccd-298">tinytext</span><span class="sxs-lookup"><span data-stu-id="55ccd-298">tinytext</span></span> |<span data-ttu-id="55ccd-299">Sträng</span><span class="sxs-lookup"><span data-stu-id="55ccd-299">String</span></span> |
| <span data-ttu-id="55ccd-300">varchar</span><span class="sxs-lookup"><span data-stu-id="55ccd-300">varchar</span></span> |<span data-ttu-id="55ccd-301">Sträng</span><span class="sxs-lookup"><span data-stu-id="55ccd-301">String</span></span> |
| <span data-ttu-id="55ccd-302">År</span><span class="sxs-lookup"><span data-stu-id="55ccd-302">year</span></span> |<span data-ttu-id="55ccd-303">int</span><span class="sxs-lookup"><span data-stu-id="55ccd-303">Int</span></span> |

## <a name="map-source-toosink-columns"></a><span data-ttu-id="55ccd-304">Mappa källkolumner toosink</span><span class="sxs-lookup"><span data-stu-id="55ccd-304">Map source toosink columns</span></span>
<span data-ttu-id="55ccd-305">toolearn mappning tabellkolumner i källan dataset toocolumns i sink dataset finns [mappa dataset kolumner i Azure Data Factory](data-factory-map-columns.md).</span><span class="sxs-lookup"><span data-stu-id="55ccd-305">toolearn about mapping columns in source dataset toocolumns in sink dataset, see [Mapping dataset columns in Azure Data Factory](data-factory-map-columns.md).</span></span>

## <a name="repeatable-read-from-relational-sources"></a><span data-ttu-id="55ccd-306">Upprepbar läsning från relationella källor</span><span class="sxs-lookup"><span data-stu-id="55ccd-306">Repeatable read from relational sources</span></span>
<span data-ttu-id="55ccd-307">När du kopierar data från relationella datalager, Kom ihåg tooavoid repeterbarhet oönskade resultat.</span><span class="sxs-lookup"><span data-stu-id="55ccd-307">When copying data from relational data stores, keep repeatability in mind tooavoid unintended outcomes.</span></span> <span data-ttu-id="55ccd-308">I Azure Data Factory, kan du köra en sektor manuellt.</span><span class="sxs-lookup"><span data-stu-id="55ccd-308">In Azure Data Factory, you can rerun a slice manually.</span></span> <span data-ttu-id="55ccd-309">Du kan också konfigurera i principen för en dataset så att ett segment som körs när ett fel uppstår.</span><span class="sxs-lookup"><span data-stu-id="55ccd-309">You can also configure retry policy for a dataset so that a slice is rerun when a failure occurs.</span></span> <span data-ttu-id="55ccd-310">När ett segment körs på antingen sätt måste toomake att som hello samma data läses oavsett hur många gånger ett segment körs.</span><span class="sxs-lookup"><span data-stu-id="55ccd-310">When a slice is rerun in either way, you need toomake sure that hello same data is read no matter how many times a slice is run.</span></span> <span data-ttu-id="55ccd-311">Se [Repeatable läsa från relationella källor](data-factory-repeatable-copy.md#repeatable-read-from-relational-sources).</span><span class="sxs-lookup"><span data-stu-id="55ccd-311">See [Repeatable read from relational sources](data-factory-repeatable-copy.md#repeatable-read-from-relational-sources).</span></span>

## <a name="performance-and-tuning"></a><span data-ttu-id="55ccd-312">Prestanda och finjustering</span><span class="sxs-lookup"><span data-stu-id="55ccd-312">Performance and Tuning</span></span>
<span data-ttu-id="55ccd-313">Se [kopiera aktivitet prestanda och justera guiden](data-factory-copy-activity-performance.md) toolearn om nyckeln faktorer som påverkan prestanda för flytt av data (Kopieringsaktiviteten) i Azure Data Factory och olika sätt toooptimize den.</span><span class="sxs-lookup"><span data-stu-id="55ccd-313">See [Copy Activity Performance & Tuning Guide](data-factory-copy-activity-performance.md) toolearn about key factors that impact performance of data movement (Copy Activity) in Azure Data Factory and various ways toooptimize it.</span></span>
