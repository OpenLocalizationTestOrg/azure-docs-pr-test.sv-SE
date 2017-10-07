---
title: "aaaMove data från DB2 med hjälp av Azure Data Factory | Microsoft Docs"
description: "Lär dig hur toomove data från en lokal DB2-databas med hjälp av Azure Data Factory-Kopieringsaktiviteten"
services: data-factory
documentationcenter: 
author: linda33wj
manager: jhubbard
editor: monicar
ms.assetid: c1644e17-4560-46bb-bf3c-b923126671f1
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/19/2017
ms.author: jingwang
ms.openlocfilehash: 696ac059be644cb3901c37d2fc746e0682c65a1f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="move-data-from-db2-by-using-azure-data-factory-copy-activity"></a><span data-ttu-id="53c8d-103">Flytta data från DB2 med hjälp av Azure Data Factory-Kopieringsaktiviteten</span><span class="sxs-lookup"><span data-stu-id="53c8d-103">Move data from DB2 by using Azure Data Factory Copy Activity</span></span>
<span data-ttu-id="53c8d-104">Den här artikeln beskriver hur du kan använda Kopieringsaktiviteten i Azure Data Factory toocopy data från ett dataarkiv för lokala DB2-databas tooa.</span><span class="sxs-lookup"><span data-stu-id="53c8d-104">This article describes how you can use Copy Activity in Azure Data Factory toocopy data from an on-premises DB2 database tooa data store.</span></span> <span data-ttu-id="53c8d-105">Du kan kopiera tooany datalager som har listats som en mottagare som stöds i hello [Data Factory data movement aktiviteter](data-factory-data-movement-activities.md#supported-data-stores-and-formats) artikel.</span><span class="sxs-lookup"><span data-stu-id="53c8d-105">You can copy data tooany store that is listed as a supported sink in hello [Data Factory data movement activities](data-factory-data-movement-activities.md#supported-data-stores-and-formats) article.</span></span> <span data-ttu-id="53c8d-106">Det här avsnittet bygger på hello Data Factory artikel som visar en översikt över data flyttas med hjälp av Kopieringsaktiviteten och visar hello stöds data store kombinationer.</span><span class="sxs-lookup"><span data-stu-id="53c8d-106">This topic builds on hello Data Factory article, which presents an overview of data movement by using Copy Activity and lists hello supported data store combinations.</span></span> 

<span data-ttu-id="53c8d-107">Data Factory stöder för närvarande endast flytta data från en DB2-databas tooa [stöds sink datalagret](data-factory-data-movement-activities.md#supported-data-stores-and-formats).</span><span class="sxs-lookup"><span data-stu-id="53c8d-107">Data Factory currently supports only moving data from a DB2 database tooa [supported sink data store](data-factory-data-movement-activities.md#supported-data-stores-and-formats).</span></span> <span data-ttu-id="53c8d-108">Flytta data från andra data lagras tooa DB2-databas inte stöds.</span><span class="sxs-lookup"><span data-stu-id="53c8d-108">Moving data from other data stores tooa DB2 database is not supported.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="53c8d-109">Krav</span><span class="sxs-lookup"><span data-stu-id="53c8d-109">Prerequisites</span></span>
<span data-ttu-id="53c8d-110">Data Factory stöder anslutande tooan lokala DB2-databas med hjälp av hello [data management gateway](data-factory-data-management-gateway.md).</span><span class="sxs-lookup"><span data-stu-id="53c8d-110">Data Factory supports connecting tooan on-premises DB2 database by using hello [data management gateway](data-factory-data-management-gateway.md).</span></span> <span data-ttu-id="53c8d-111">Stegvisa instruktioner tooset hello gateway data i pipeline toomove dina data finns i avsnittet hello [flytta data från lokala toocloud](data-factory-move-data-between-onprem-and-cloud.md) artikel.</span><span class="sxs-lookup"><span data-stu-id="53c8d-111">For step-by-step instructions tooset up hello gateway data pipeline toomove your data, see hello [Move data from on-premises toocloud](data-factory-move-data-between-onprem-and-cloud.md) article.</span></span>

<span data-ttu-id="53c8d-112">En gateway krävs även om hello DB2 finns på Azure IaaS-VM.</span><span class="sxs-lookup"><span data-stu-id="53c8d-112">A gateway is required even if hello DB2 is hosted on Azure IaaS VM.</span></span> <span data-ttu-id="53c8d-113">Du kan installera hello gateway på hello samma IaaS VM som hello datalager.</span><span class="sxs-lookup"><span data-stu-id="53c8d-113">You can install hello gateway on hello same IaaS VM as hello data store.</span></span> <span data-ttu-id="53c8d-114">Om hello gateway kan ansluta toohello databasen kan installera du hello gateway på en annan virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="53c8d-114">If hello gateway can connect toohello database, you can install hello gateway on a different VM.</span></span>

<span data-ttu-id="53c8d-115">hello data management gateway innehåller en inbyggd DB2-drivrutin, så att du inte behöver toomanually installera en drivrutin toocopy data från DB2.</span><span class="sxs-lookup"><span data-stu-id="53c8d-115">hello data management gateway provides a built-in DB2 driver, so you don't need toomanually install a driver toocopy data from DB2.</span></span>

> [!NOTE]
> <span data-ttu-id="53c8d-116">Tips om hur du felsöker anslutning och gateway-problem finns i hello [felsökning av problem med gateway](data-factory-data-management-gateway.md#troubleshooting-gateway-issues) artikel.</span><span class="sxs-lookup"><span data-stu-id="53c8d-116">For tips on troubleshooting connection and gateway issues, see hello [Troubleshoot gateway issues](data-factory-data-management-gateway.md#troubleshooting-gateway-issues) article.</span></span>


## <a name="supported-versions"></a><span data-ttu-id="53c8d-117">Versioner som stöds</span><span class="sxs-lookup"><span data-stu-id="53c8d-117">Supported versions</span></span>
<span data-ttu-id="53c8d-118">hello Data Factory DB2-koppling stöder hello följande IBM DB2-plattformar och versioner med distribuerade relationella Database Architecture (DRDA) SQL åtkomst Manager version 9, 10 och 11:</span><span class="sxs-lookup"><span data-stu-id="53c8d-118">hello Data Factory DB2 connector supports hello following IBM DB2 platforms and versions with Distributed Relational Database Architecture (DRDA) SQL Access Manager versions 9, 10, and 11:</span></span>

* <span data-ttu-id="53c8d-119">IBM DB2 för z/OS-versionen 11,1</span><span class="sxs-lookup"><span data-stu-id="53c8d-119">IBM DB2 for z/OS version 11.1</span></span>
* <span data-ttu-id="53c8d-120">IBM DB2 för z/OS-version 10.1</span><span class="sxs-lookup"><span data-stu-id="53c8d-120">IBM DB2 for z/OS version 10.1</span></span>
* <span data-ttu-id="53c8d-121">IBM DB2 för i (AS400) version 7.2</span><span class="sxs-lookup"><span data-stu-id="53c8d-121">IBM DB2 for i (AS400) version 7.2</span></span>
* <span data-ttu-id="53c8d-122">IBM DB2 för i (AS400) version 7.1</span><span class="sxs-lookup"><span data-stu-id="53c8d-122">IBM DB2 for i (AS400) version 7.1</span></span>
* <span data-ttu-id="53c8d-123">IBM DB2 för Linux, UNIX- och Windows (LUW) version 11</span><span class="sxs-lookup"><span data-stu-id="53c8d-123">IBM DB2 for Linux, UNIX, and Windows (LUW) version 11</span></span>
* <span data-ttu-id="53c8d-124">IBM DB2 för LUW version 10.5</span><span class="sxs-lookup"><span data-stu-id="53c8d-124">IBM DB2 for LUW version 10.5</span></span>
* <span data-ttu-id="53c8d-125">IBM DB2 för LUW version 10.1</span><span class="sxs-lookup"><span data-stu-id="53c8d-125">IBM DB2 for LUW version 10.1</span></span>

> [!TIP]
> <span data-ttu-id="53c8d-126">Om felmeddelande hello ”hello paketet motsvarande tooan SQL-instruktionen Körningsbegäran hittades inte.</span><span class="sxs-lookup"><span data-stu-id="53c8d-126">If you receive hello error message "hello package corresponding tooan SQL statement execution request was not found.</span></span> <span data-ttu-id="53c8d-127">SQLSTATE = 51002 SQLCODE =-805 ”, Hej orsaken är ett nödvändigt paket inte skapas för hello normal användare på hello OS.</span><span class="sxs-lookup"><span data-stu-id="53c8d-127">SQLSTATE=51002 SQLCODE=-805," hello reason is a necessary package is not created for hello normal user on hello OS.</span></span> <span data-ttu-id="53c8d-128">tooresolve i detta fall, följ instruktionerna för din servertyp DB2:</span><span class="sxs-lookup"><span data-stu-id="53c8d-128">tooresolve this issue, follow these instructions for your DB2 server type:</span></span>
> - <span data-ttu-id="53c8d-129">DB2 för i (AS400): låta en användare skapar hello samling för hello normal användare innan du kör Kopieringsaktiviteten.</span><span class="sxs-lookup"><span data-stu-id="53c8d-129">DB2 for i (AS400): Let a power user create hello collection for hello normal user before running Copy Activity.</span></span> <span data-ttu-id="53c8d-130">toocreate hello insamling, användning hello kommando:`create collection <username>`</span><span class="sxs-lookup"><span data-stu-id="53c8d-130">toocreate hello collection, use hello command: `create collection <username>`</span></span>
> - <span data-ttu-id="53c8d-131">DB2 för z/OS eller LUW: Använd ett konto med hög behörighet--privilegierad användare eller administratör som har paketet myndigheter och BIND BINDADD, BEVILJA behörighet för EXECUTE tooPUBLIC--toorun hello kopiera en gång.</span><span class="sxs-lookup"><span data-stu-id="53c8d-131">DB2 for z/OS or LUW: Use a high privilege account--a power user or admin that has package authorities and BIND, BINDADD, GRANT EXECUTE tooPUBLIC permissions--toorun hello copy once.</span></span> <span data-ttu-id="53c8d-132">hello nödvändiga paketet skapas automatiskt under hello kopia.</span><span class="sxs-lookup"><span data-stu-id="53c8d-132">hello necessary package is automatically created during hello copy.</span></span> <span data-ttu-id="53c8d-133">Därefter kan du växla tillbaka toohello normal användare för efterföljande kopia-körs.</span><span class="sxs-lookup"><span data-stu-id="53c8d-133">Afterward, you can switch back toohello normal user for your subsequent copy runs.</span></span>

## <a name="getting-started"></a><span data-ttu-id="53c8d-134">Komma igång</span><span class="sxs-lookup"><span data-stu-id="53c8d-134">Getting started</span></span>
<span data-ttu-id="53c8d-135">Du kan skapa en pipeline med en aktivitet toomove data från en lokal DB2-datalagret med hjälp av olika verktyg och API: er:</span><span class="sxs-lookup"><span data-stu-id="53c8d-135">You can create a pipeline with a copy activity toomove data from an on-premises DB2 data store by using different tools and APIs:</span></span> 

- <span data-ttu-id="53c8d-136">hello enklaste sättet toocreate en pipeline är toouse hello Azure Data Factory kopiera guiden.</span><span class="sxs-lookup"><span data-stu-id="53c8d-136">hello easiest way toocreate a pipeline is toouse hello Azure Data Factory Copy Wizard.</span></span> <span data-ttu-id="53c8d-137">En snabb genomgång om hur du skapar en pipeline med hjälp av hello guiden Kopiera finns hello [Självstudier: skapa en pipeline med hello guiden Kopiera](data-factory-copy-data-wizard-tutorial.md).</span><span class="sxs-lookup"><span data-stu-id="53c8d-137">For a quick walkthrough on creating a pipeline by using hello Copy Wizard, see hello [Tutorial: Create a pipeline by using hello Copy Wizard](data-factory-copy-data-wizard-tutorial.md).</span></span> 
- <span data-ttu-id="53c8d-138">Du kan också använda verktygen toocreate en pipeline, inklusive hello Azure-portalen, Visual Studio, Azure PowerShell, en Azure Resource Manager-mall, hello .NET-API och hello REST API.</span><span class="sxs-lookup"><span data-stu-id="53c8d-138">You can also use tools toocreate a pipeline, including hello Azure portal, Visual Studio, Azure PowerShell, an Azure Resource Manager template, hello .NET API, and hello REST API.</span></span> <span data-ttu-id="53c8d-139">Stegvisa instruktioner toocreate en pipeline med en kopieringsaktiviteten finns hello [Kopieringsaktiviteten kursen](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md).</span><span class="sxs-lookup"><span data-stu-id="53c8d-139">For step-by-step instructions toocreate a pipeline with a copy activity, see hello [Copy Activity tutorial](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md).</span></span> 

<span data-ttu-id="53c8d-140">Om du använder hello verktyg eller API: er kan utföra du hello följande steg toocreate en pipeline som flyttar data från en källdata lagra tooa sink-datalagret:</span><span class="sxs-lookup"><span data-stu-id="53c8d-140">Whether you use hello tools or APIs, you perform hello following steps toocreate a pipeline that moves data from a source data store tooa sink data store:</span></span>

1. <span data-ttu-id="53c8d-141">Skapa länkade tjänster toolink indata och utdata lagrar tooyour data factory.</span><span class="sxs-lookup"><span data-stu-id="53c8d-141">Create linked services toolink input and output data stores tooyour data factory.</span></span>
2. <span data-ttu-id="53c8d-142">Skapa datauppsättningar toorepresent indata och utdata för hello kopieringen.</span><span class="sxs-lookup"><span data-stu-id="53c8d-142">Create datasets toorepresent input and output data for hello copy operation.</span></span> 
3. <span data-ttu-id="53c8d-143">Skapa en pipeline med en kopia-aktivitet som tar en datamängd som indata och en dataset som utdata.</span><span class="sxs-lookup"><span data-stu-id="53c8d-143">Create a pipeline with a copy activity that takes a dataset as an input and a dataset as an output.</span></span> 

<span data-ttu-id="53c8d-144">När du använder hello guiden Kopiera, JSON definitioner för hello Data Factory länkade tjänster, skapas automatiskt datauppsättningar och pipeline entiteter för dig.</span><span class="sxs-lookup"><span data-stu-id="53c8d-144">When you use hello Copy Wizard, JSON definitions for hello Data Factory linked services, datasets, and pipeline entities are automatically created for you.</span></span> <span data-ttu-id="53c8d-145">När du använder verktyg eller API: er (utom hello .NET-API) kan du definiera hello Data Factory-enheter med hjälp av hello JSON-format.</span><span class="sxs-lookup"><span data-stu-id="53c8d-145">When you use tools or APIs (except hello .NET API), you define hello Data Factory entities by using hello JSON format.</span></span> <span data-ttu-id="53c8d-146">Hej [JSON-exempel: kopiera data från DB2 tooAzure Blob storage](#json-example-copy-data-from-db2-to-azure-blob) visar hello JSON definitioner för hello Data Factory-enheter som ska använda toocopy data från ett lokalt DB2-dataarkiv.</span><span class="sxs-lookup"><span data-stu-id="53c8d-146">hello [JSON example: Copy data from DB2 tooAzure Blob storage](#json-example-copy-data-from-db2-to-azure-blob) shows hello JSON definitions for hello Data Factory entities that are used toocopy data from an on-premises DB2 data store.</span></span>

<span data-ttu-id="53c8d-147">hello följande avsnitt innehåller information om hello JSON-egenskaper används toodefine hello Data Factory enheter som är specifika tooa DB2-datalagret.</span><span class="sxs-lookup"><span data-stu-id="53c8d-147">hello following sections provide details about hello JSON properties that are used toodefine hello Data Factory entities that are specific tooa DB2 data store.</span></span>

## <a name="db2-linked-service-properties"></a><span data-ttu-id="53c8d-148">DB2 länkade tjänstens egenskaper</span><span class="sxs-lookup"><span data-stu-id="53c8d-148">DB2 linked service properties</span></span>
<span data-ttu-id="53c8d-149">hello i den följande tabellen listas hello JSON egenskaper som är specifika tooa DB2 länkade tjänsten.</span><span class="sxs-lookup"><span data-stu-id="53c8d-149">hello following table lists hello JSON properties that are specific tooa DB2 linked service.</span></span>

| <span data-ttu-id="53c8d-150">Egenskap</span><span class="sxs-lookup"><span data-stu-id="53c8d-150">Property</span></span> | <span data-ttu-id="53c8d-151">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="53c8d-151">Description</span></span> | <span data-ttu-id="53c8d-152">Krävs</span><span class="sxs-lookup"><span data-stu-id="53c8d-152">Required</span></span> |
| --- | --- | --- |
| <span data-ttu-id="53c8d-153">**typ**</span><span class="sxs-lookup"><span data-stu-id="53c8d-153">**type**</span></span> |<span data-ttu-id="53c8d-154">Den här egenskapen måste anges för**OnPremisesDB2**.</span><span class="sxs-lookup"><span data-stu-id="53c8d-154">This property must be set too**OnPremisesDB2**.</span></span> |<span data-ttu-id="53c8d-155">Ja</span><span class="sxs-lookup"><span data-stu-id="53c8d-155">Yes</span></span> |
| <span data-ttu-id="53c8d-156">**Server**</span><span class="sxs-lookup"><span data-stu-id="53c8d-156">**server**</span></span> |<span data-ttu-id="53c8d-157">hello namnet på hello DB2-server.</span><span class="sxs-lookup"><span data-stu-id="53c8d-157">hello name of hello DB2 server.</span></span> |<span data-ttu-id="53c8d-158">Ja</span><span class="sxs-lookup"><span data-stu-id="53c8d-158">Yes</span></span> |
| <span data-ttu-id="53c8d-159">**databasen**</span><span class="sxs-lookup"><span data-stu-id="53c8d-159">**database**</span></span> |<span data-ttu-id="53c8d-160">hello namnet på hello DB2-databas.</span><span class="sxs-lookup"><span data-stu-id="53c8d-160">hello name of hello DB2 database.</span></span> |<span data-ttu-id="53c8d-161">Ja</span><span class="sxs-lookup"><span data-stu-id="53c8d-161">Yes</span></span> |
| <span data-ttu-id="53c8d-162">**schemat**</span><span class="sxs-lookup"><span data-stu-id="53c8d-162">**schema**</span></span> |<span data-ttu-id="53c8d-163">hello namnet på hello schema i hello DB2-databasen.</span><span class="sxs-lookup"><span data-stu-id="53c8d-163">hello name of hello schema in hello DB2 database.</span></span> <span data-ttu-id="53c8d-164">Den här egenskapen är skiftlägeskänsliga.</span><span class="sxs-lookup"><span data-stu-id="53c8d-164">This property is case-sensitive.</span></span> |<span data-ttu-id="53c8d-165">Nej</span><span class="sxs-lookup"><span data-stu-id="53c8d-165">No</span></span> |
| <span data-ttu-id="53c8d-166">**authenticationType**</span><span class="sxs-lookup"><span data-stu-id="53c8d-166">**authenticationType**</span></span> |<span data-ttu-id="53c8d-167">hello typ av autentisering som används tooconnect toohello DB2-databasen.</span><span class="sxs-lookup"><span data-stu-id="53c8d-167">hello type of authentication that is used tooconnect toohello DB2 database.</span></span> <span data-ttu-id="53c8d-168">hello möjliga värden är: anonym, grundläggande och Windows.</span><span class="sxs-lookup"><span data-stu-id="53c8d-168">hello possible values are: Anonymous, Basic, and Windows.</span></span> |<span data-ttu-id="53c8d-169">Ja</span><span class="sxs-lookup"><span data-stu-id="53c8d-169">Yes</span></span> |
| <span data-ttu-id="53c8d-170">**användarnamn**</span><span class="sxs-lookup"><span data-stu-id="53c8d-170">**username**</span></span> |<span data-ttu-id="53c8d-171">hello namnet för hello användarkonto om du använder grundläggande eller Windows-autentisering.</span><span class="sxs-lookup"><span data-stu-id="53c8d-171">hello name for hello user account if you use Basic or Windows authentication.</span></span> |<span data-ttu-id="53c8d-172">Nej</span><span class="sxs-lookup"><span data-stu-id="53c8d-172">No</span></span> |
| <span data-ttu-id="53c8d-173">**lösenord**</span><span class="sxs-lookup"><span data-stu-id="53c8d-173">**password**</span></span> |<span data-ttu-id="53c8d-174">hello lösenordet för användarkontot för hello.</span><span class="sxs-lookup"><span data-stu-id="53c8d-174">hello password for hello user account.</span></span> |<span data-ttu-id="53c8d-175">Nej</span><span class="sxs-lookup"><span data-stu-id="53c8d-175">No</span></span> |
| <span data-ttu-id="53c8d-176">**gatewayName**</span><span class="sxs-lookup"><span data-stu-id="53c8d-176">**gatewayName**</span></span> |<span data-ttu-id="53c8d-177">hello namnet på hello-gateway som hello Data Factory-tjänsten ska använda tooconnect toohello lokala DB2-databas.</span><span class="sxs-lookup"><span data-stu-id="53c8d-177">hello name of hello gateway that hello Data Factory service should use tooconnect toohello on-premises DB2 database.</span></span> |<span data-ttu-id="53c8d-178">Ja</span><span class="sxs-lookup"><span data-stu-id="53c8d-178">Yes</span></span> |

## <a name="dataset-properties"></a><span data-ttu-id="53c8d-179">Egenskaper för datamängd</span><span class="sxs-lookup"><span data-stu-id="53c8d-179">Dataset properties</span></span>
<span data-ttu-id="53c8d-180">En lista över egenskaper som är tillgängliga för att definiera datauppsättningar och hello avsnitt finns hello [skapa datauppsättningar](data-factory-create-datasets.md) artikel.</span><span class="sxs-lookup"><span data-stu-id="53c8d-180">For a list of hello sections and properties that are available for defining datasets, see hello [Creating datasets](data-factory-create-datasets.md) article.</span></span> <span data-ttu-id="53c8d-181">Avsnitten som **struktur**, **tillgänglighet**, och hello **princip** för en dataset JSON är liknande för alla typer av dataset Azure SQL (Azure Blob storage, Azure Table lagring och så vidare).</span><span class="sxs-lookup"><span data-stu-id="53c8d-181">Sections, such as **structure**, **availability**, and hello **policy** for a dataset JSON, are similar for all dataset types (Azure SQL, Azure Blob storage, Azure Table storage, and so on).</span></span>

<span data-ttu-id="53c8d-182">Hej **typeProperties** avsnitt är olika för varje typ av dataset och ger information om hello platsen för hello data i datalagret hello.</span><span class="sxs-lookup"><span data-stu-id="53c8d-182">hello **typeProperties** section is different for each type of dataset and provides information about hello location of hello data in hello data store.</span></span> <span data-ttu-id="53c8d-183">Hej **typeProperties** avsnittet för en dataset av typen **RelationalTable**, vilket inkluderar hello DB2 dataset, har hello följande egenskap:</span><span class="sxs-lookup"><span data-stu-id="53c8d-183">hello **typeProperties** section for a dataset of type **RelationalTable**, which includes hello DB2 dataset, has hello following property:</span></span>

| <span data-ttu-id="53c8d-184">Egenskap</span><span class="sxs-lookup"><span data-stu-id="53c8d-184">Property</span></span> | <span data-ttu-id="53c8d-185">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="53c8d-185">Description</span></span> | <span data-ttu-id="53c8d-186">Krävs</span><span class="sxs-lookup"><span data-stu-id="53c8d-186">Required</span></span> |
| --- | --- | --- |
| <span data-ttu-id="53c8d-187">**tableName**</span><span class="sxs-lookup"><span data-stu-id="53c8d-187">**tableName**</span></span> |<span data-ttu-id="53c8d-188">hello namn på hello tabell i hello DB2-databasinstansen som hello länkade tjänsten refererar till.</span><span class="sxs-lookup"><span data-stu-id="53c8d-188">hello name of hello table in hello DB2 database instance that hello linked service refers to.</span></span> <span data-ttu-id="53c8d-189">Den här egenskapen är skiftlägeskänsliga.</span><span class="sxs-lookup"><span data-stu-id="53c8d-189">This property is case-sensitive.</span></span> |<span data-ttu-id="53c8d-190">Nej (om hello **frågan** -egenskapen för en kopia aktivitet av typen **RelationalSource** har angetts)</span><span class="sxs-lookup"><span data-stu-id="53c8d-190">No (if hello **query** property of a copy activity of type **RelationalSource** is specified)</span></span> |

## <a name="copy-activity-properties"></a><span data-ttu-id="53c8d-191">Kopiera egenskaper för aktivitet</span><span class="sxs-lookup"><span data-stu-id="53c8d-191">Copy Activity properties</span></span>
<span data-ttu-id="53c8d-192">En lista över egenskaper som är tillgängliga för att definiera kopiera aktiviteter och hello avsnitt finns hello [skapar Pipelines](data-factory-create-pipelines.md) artikel.</span><span class="sxs-lookup"><span data-stu-id="53c8d-192">For a list of hello sections and properties that are available for defining copy activities, see hello [Creating Pipelines](data-factory-create-pipelines.md) article.</span></span> <span data-ttu-id="53c8d-193">Kopiera egenskaper för aktivitet som **namn**, **beskrivning**, **indata** tabellen **matar ut** tabellen och **princip**, är tillgängliga för alla typer av aktiviteter.</span><span class="sxs-lookup"><span data-stu-id="53c8d-193">Copy Activity properties, such as **name**, **description**, **inputs** table, **outputs** table, and **policy**, are available for all types of activities.</span></span> <span data-ttu-id="53c8d-194">Hej egenskaper som är tillgängliga i hello **typeProperties** avsnittet hello aktivitet för varje aktivitetstyp.</span><span class="sxs-lookup"><span data-stu-id="53c8d-194">hello properties that are available in hello **typeProperties** section of hello activity for each activity type.</span></span> <span data-ttu-id="53c8d-195">För Kopieringsaktiviteten varierar hello egenskaper beroende på hello typer av datakällor och sänkor.</span><span class="sxs-lookup"><span data-stu-id="53c8d-195">For Copy Activity, hello properties vary depending on hello types of data sources and sinks.</span></span>

<span data-ttu-id="53c8d-196">För Kopieringsaktiviteten när hello källa är av typen **RelationalSource** (som omfattar DB2), hello följande egenskaper är tillgängliga i hello **typeProperties** avsnitt:</span><span class="sxs-lookup"><span data-stu-id="53c8d-196">For Copy Activity, when hello source is of type **RelationalSource** (which includes DB2), hello following properties are available in hello **typeProperties** section:</span></span>

| <span data-ttu-id="53c8d-197">Egenskap</span><span class="sxs-lookup"><span data-stu-id="53c8d-197">Property</span></span> | <span data-ttu-id="53c8d-198">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="53c8d-198">Description</span></span> | <span data-ttu-id="53c8d-199">Tillåtna värden</span><span class="sxs-lookup"><span data-stu-id="53c8d-199">Allowed values</span></span> | <span data-ttu-id="53c8d-200">Krävs</span><span class="sxs-lookup"><span data-stu-id="53c8d-200">Required</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="53c8d-201">**frågan**</span><span class="sxs-lookup"><span data-stu-id="53c8d-201">**query**</span></span> |<span data-ttu-id="53c8d-202">Använda hello anpassad fråga tooread hello data.</span><span class="sxs-lookup"><span data-stu-id="53c8d-202">Use hello custom query tooread hello data.</span></span> |<span data-ttu-id="53c8d-203">SQL-sträng.</span><span class="sxs-lookup"><span data-stu-id="53c8d-203">SQL query string.</span></span> <span data-ttu-id="53c8d-204">Exempel: `"query": "select * from "MySchema"."MyTable""`</span><span class="sxs-lookup"><span data-stu-id="53c8d-204">For example: `"query": "select * from "MySchema"."MyTable""`</span></span> |<span data-ttu-id="53c8d-205">Nej (om hello **tableName** -egenskapen för en dataset har angetts)</span><span class="sxs-lookup"><span data-stu-id="53c8d-205">No (if hello **tableName** property of a dataset is specified)</span></span> |

> [!NOTE]
> <span data-ttu-id="53c8d-206">Schema och tabellnamn är skiftlägeskänsliga.</span><span class="sxs-lookup"><span data-stu-id="53c8d-206">Schema and table names are case-sensitive.</span></span> <span data-ttu-id="53c8d-207">I hello frågeuttrycket omge egenskapsnamn med ”” (dubbla citattecken).</span><span class="sxs-lookup"><span data-stu-id="53c8d-207">In hello query statement, enclose property names by using "" (double quotes).</span></span> <span data-ttu-id="53c8d-208">Exempel:</span><span class="sxs-lookup"><span data-stu-id="53c8d-208">For example:</span></span>
>
> ```sql
> "query": "select * from "DB2ADMIN"."Customers""
> ```

## <a name="json-example-copy-data-from-db2-tooazure-blob-storage"></a><span data-ttu-id="53c8d-209">JSON-exempel: kopiera data från DB2 tooAzure Blob storage</span><span class="sxs-lookup"><span data-stu-id="53c8d-209">JSON example: Copy data from DB2 tooAzure Blob storage</span></span>
<span data-ttu-id="53c8d-210">Det här exemplet innehåller exempel JSON definitioner som du kan använda toocreate en pipeline med hjälp av hello [Azure-portalen](data-factory-copy-activity-tutorial-using-azure-portal.md), [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md), eller [Azure PowerShell](data-factory-copy-activity-tutorial-using-powershell.md).</span><span class="sxs-lookup"><span data-stu-id="53c8d-210">This example provides sample JSON definitions that you can use toocreate a pipeline by using hello [Azure portal](data-factory-copy-activity-tutorial-using-azure-portal.md), [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md), or [Azure PowerShell](data-factory-copy-activity-tutorial-using-powershell.md).</span></span> <span data-ttu-id="53c8d-211">hello exempel visas hur toocopy data från en DB2-databas tooBlob lagring.</span><span class="sxs-lookup"><span data-stu-id="53c8d-211">hello example shows you how toocopy data from a DB2 database tooBlob storage.</span></span> <span data-ttu-id="53c8d-212">Dock datan kan kopieras för[stöds data lagra Mottagartypen](data-factory-data-movement-activities.md#supported-data-stores-and-formats) med hjälp av Azure Data Factory-Kopieringsaktiviteten.</span><span class="sxs-lookup"><span data-stu-id="53c8d-212">However, data can be copied too[any supported data store sink type](data-factory-data-movement-activities.md#supported-data-stores-and-formats) by using Azure Data Factory Copy Activity.</span></span>

<span data-ttu-id="53c8d-213">hello exemplet har hello följande Data Factory-enheter:</span><span class="sxs-lookup"><span data-stu-id="53c8d-213">hello sample has hello following Data Factory entities:</span></span>

- <span data-ttu-id="53c8d-214">En DB2 länkade tjänsten av typen [OnPremisesDb2](data-factory-onprem-db2-connector.md#linked-service-properties)</span><span class="sxs-lookup"><span data-stu-id="53c8d-214">A DB2 linked service of type [OnPremisesDb2](data-factory-onprem-db2-connector.md#linked-service-properties)</span></span>
- <span data-ttu-id="53c8d-215">Ett Azure Blob storage länkade tjänsten av typen [AzureStorage](data-factory-azure-blob-connector.md#linked-service-properties)</span><span class="sxs-lookup"><span data-stu-id="53c8d-215">An Azure Blob storage linked service of type [AzureStorage](data-factory-azure-blob-connector.md#linked-service-properties)</span></span>
- <span data-ttu-id="53c8d-216">Indata [dataset](data-factory-create-datasets.md) av typen [RelationalTable](data-factory-onprem-db2-connector.md#dataset-properties)</span><span class="sxs-lookup"><span data-stu-id="53c8d-216">An input [dataset](data-factory-create-datasets.md) of type [RelationalTable](data-factory-onprem-db2-connector.md#dataset-properties)</span></span>
- <span data-ttu-id="53c8d-217">Utdata [dataset](data-factory-create-datasets.md) av typen [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties)</span><span class="sxs-lookup"><span data-stu-id="53c8d-217">An output [dataset](data-factory-create-datasets.md) of type [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties)</span></span>
- <span data-ttu-id="53c8d-218">En [pipeline](data-factory-create-pipelines.md) med en kopia-aktivitet som använder hello [RelationalSource](data-factory-onprem-db2-connector.md#copy-activity-properties) och [BlobSink](data-factory-azure-blob-connector.md#copy-activity-properties) egenskaper</span><span class="sxs-lookup"><span data-stu-id="53c8d-218">A [pipeline](data-factory-create-pipelines.md) with a copy activity that uses hello [RelationalSource](data-factory-onprem-db2-connector.md#copy-activity-properties) and [BlobSink](data-factory-azure-blob-connector.md#copy-activity-properties) properties</span></span>

<span data-ttu-id="53c8d-219">hello exemplet kopierar data från ett frågeresultat i Azure blob för tooan en DB2-databasen varje timme.</span><span class="sxs-lookup"><span data-stu-id="53c8d-219">hello sample copies data from a query result in a DB2 database tooan Azure blob hourly.</span></span> <span data-ttu-id="53c8d-220">hello JSON egenskaper som används i hello exemplet beskrivs i hello avsnitten som följer hello entitet definitioner.</span><span class="sxs-lookup"><span data-stu-id="53c8d-220">hello JSON properties that are used in hello sample are described in hello sections that follow hello entity definitions.</span></span>

<span data-ttu-id="53c8d-221">Som ett första steg bör du installera och konfigurera en datagateway.</span><span class="sxs-lookup"><span data-stu-id="53c8d-221">As a first step, install and configure a data gateway.</span></span> <span data-ttu-id="53c8d-222">Anvisningar finns i hello [flytta data mellan lokala platser och moln](data-factory-move-data-between-onprem-and-cloud.md) artikel.</span><span class="sxs-lookup"><span data-stu-id="53c8d-222">Instructions are in hello [Moving data between on-premises locations and cloud](data-factory-move-data-between-onprem-and-cloud.md) article.</span></span>

<span data-ttu-id="53c8d-223">**DB2 länkad tjänst**</span><span class="sxs-lookup"><span data-stu-id="53c8d-223">**DB2 linked service**</span></span>

```json
{
    "name": "OnPremDb2LinkedService",
    "properties": {
        "type": "OnPremisesDb2",
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

<span data-ttu-id="53c8d-224">**Azure Blob länkade lagringstjänsten**</span><span class="sxs-lookup"><span data-stu-id="53c8d-224">**Azure Blob storage linked service**</span></span>

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

<span data-ttu-id="53c8d-225">**DB2 inkommande dataset**</span><span class="sxs-lookup"><span data-stu-id="53c8d-225">**DB2 input dataset**</span></span>

<span data-ttu-id="53c8d-226">hello exemplet förutsätter att du har skapat en tabell i DB2 med namnet ”mytable” som prefix som har en kolumn med rubriken ”tidsstämpel” för hello tid series-data.</span><span class="sxs-lookup"><span data-stu-id="53c8d-226">hello sample assumes that you have created a table in DB2 named "MyTable" that has a column labeled "timestamp" for hello time series data.</span></span>

<span data-ttu-id="53c8d-227">Hej **externa** -egenskapen anges för ”true”.</span><span class="sxs-lookup"><span data-stu-id="53c8d-227">hello **external** property is set too"true."</span></span> <span data-ttu-id="53c8d-228">Den här inställningen informerar hello Data Factory-tjänsten att den här datauppsättningen är externa toohello data factory och inte tillverkas av en aktivitet i hello data factory.</span><span class="sxs-lookup"><span data-stu-id="53c8d-228">This setting informs hello Data Factory service that this dataset is external toohello data factory and is not produced by an activity in hello data factory.</span></span> <span data-ttu-id="53c8d-229">Observera att hello **typen** egenskapen för**RelationalTable**.</span><span class="sxs-lookup"><span data-stu-id="53c8d-229">Notice that hello **type** property is set too**RelationalTable**.</span></span>


```json
{
    "name": "Db2DataSet",
    "properties": {
        "type": "RelationalTable",
        "linkedServiceName": "OnPremDb2LinkedService",
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

<span data-ttu-id="53c8d-230">**Azure Blob utdatauppsättningen**</span><span class="sxs-lookup"><span data-stu-id="53c8d-230">**Azure Blob output dataset**</span></span>

<span data-ttu-id="53c8d-231">Data skrivs tooa nya blob varje timme genom att ange hello **frekvens** egenskapen för ”timme” och hello **intervall** egenskapen too1.</span><span class="sxs-lookup"><span data-stu-id="53c8d-231">Data is written tooa new blob every hour by setting hello **frequency** property too"Hour" and hello **interval** property too1.</span></span> <span data-ttu-id="53c8d-232">Hej **folderPath** egenskapen för blob hello utvärderas dynamiskt baserat på hello starttiden för hello-segment som bearbetas.</span><span class="sxs-lookup"><span data-stu-id="53c8d-232">hello **folderPath** property for hello blob is dynamically evaluated based on hello start time of hello slice that is being processed.</span></span> <span data-ttu-id="53c8d-233">hello mappsökväg använder hello år, månad, dag och timme delar av hello starttid.</span><span class="sxs-lookup"><span data-stu-id="53c8d-233">hello folder path uses hello year, month, day, and hour parts of hello start time.</span></span>

```json
{
    "name": "AzureBlobDb2DataSet",
    "properties": {
        "type": "AzureBlob",
        "linkedServiceName": "AzureStorageLinkedService",
        "typeProperties": {
            "folderPath": "mycontainer/db2/yearno={Year}/monthno={Month}/dayno={Day}/hourno={Hour}",
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

<span data-ttu-id="53c8d-234">**Pipeline för hello kopieringsaktiviteten**</span><span class="sxs-lookup"><span data-stu-id="53c8d-234">**Pipeline for hello copy activity**</span></span>

<span data-ttu-id="53c8d-235">hello pipelinen innehåller en kopia-aktivitet som är konfigurerad toouse angivna indata och utdata-datauppsättningar och som är schemalagda toorun varje timme.</span><span class="sxs-lookup"><span data-stu-id="53c8d-235">hello pipeline contains a copy activity that is configured toouse specified input and output datasets and which is scheduled toorun every hour.</span></span> <span data-ttu-id="53c8d-236">I hello JSON-definitionen för hello pipelinen hello **källa** typ har angetts för**RelationalSource** och hello **sink** typ har angetts för**BlobSink**.</span><span class="sxs-lookup"><span data-stu-id="53c8d-236">In hello JSON definition for hello pipeline, hello **source** type is set too**RelationalSource** and hello **sink** type is set too**BlobSink**.</span></span> <span data-ttu-id="53c8d-237">hello SQL-frågan som angetts för hello **frågan** egenskapen väljer hello data från hello ”order”-tabellen.</span><span class="sxs-lookup"><span data-stu-id="53c8d-237">hello SQL query specified for hello **query** property selects hello data from hello "Orders" table.</span></span>

```json
{
    "name": "CopyDb2ToBlob",
    "properties": {
        "description": "pipeline for hello copy activity",
        "activities": [
            {
                "type": "Copy",
                "typeProperties": {
                    "source": {
                        "type": "RelationalSource",
                        "query": "select * from \"Orders\""
                    },
                    "sink": {
                        "type": "BlobSink"
                    }
                },
                "inputs": [
                    {
                        "name": "Db2DataSet"
                    }
                ],
                "outputs": [
                    {
                        "name": "AzureBlobDb2DataSet"
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
                "name": "Db2ToBlob"
            }
        ],
        "start": "2014-06-01T18:00:00Z",
        "end": "2014-06-01T19:00:00Z"
    }
}
```

## <a name="type-mapping-for-db2"></a><span data-ttu-id="53c8d-238">Mappning för DB2</span><span class="sxs-lookup"><span data-stu-id="53c8d-238">Type mapping for DB2</span></span>
<span data-ttu-id="53c8d-239">Som anges i hello [data movement aktiviteter](data-factory-data-movement-activities.md) artikeln Kopieringsaktiviteten utför automatisk konverteringar från källtyp typen toosink med hello följande två sätt:</span><span class="sxs-lookup"><span data-stu-id="53c8d-239">As mentioned in hello [data movement activities](data-factory-data-movement-activities.md) article, Copy Activity performs automatic type conversions from source type toosink type by using hello following two-step approach:</span></span>

1. <span data-ttu-id="53c8d-240">Konvertera från en inbyggd typ tooa .NET källtypen</span><span class="sxs-lookup"><span data-stu-id="53c8d-240">Convert from a native source type tooa .NET type</span></span>
2. <span data-ttu-id="53c8d-241">Konvertera från en .NET typen tooa interna sink-typ</span><span class="sxs-lookup"><span data-stu-id="53c8d-241">Convert from a .NET type tooa native sink type</span></span>

<span data-ttu-id="53c8d-242">hello används följande mappningar när Kopieringsaktiviteten konverterar hello data från en DB2 typen tooa .NET-typ:</span><span class="sxs-lookup"><span data-stu-id="53c8d-242">hello following mappings are used when Copy Activity converts hello data from a DB2 type tooa .NET type:</span></span>

| <span data-ttu-id="53c8d-243">Typen för DB2-databas</span><span class="sxs-lookup"><span data-stu-id="53c8d-243">DB2 database type</span></span> | <span data-ttu-id="53c8d-244">.NET framework-typ</span><span class="sxs-lookup"><span data-stu-id="53c8d-244">.NET Framework type</span></span> |
| --- | --- |
| <span data-ttu-id="53c8d-245">SmallInt</span><span class="sxs-lookup"><span data-stu-id="53c8d-245">SmallInt</span></span> |<span data-ttu-id="53c8d-246">Int16</span><span class="sxs-lookup"><span data-stu-id="53c8d-246">Int16</span></span> |
| <span data-ttu-id="53c8d-247">Integer</span><span class="sxs-lookup"><span data-stu-id="53c8d-247">Integer</span></span> |<span data-ttu-id="53c8d-248">Int32</span><span class="sxs-lookup"><span data-stu-id="53c8d-248">Int32</span></span> |
| <span data-ttu-id="53c8d-249">BigInt</span><span class="sxs-lookup"><span data-stu-id="53c8d-249">BigInt</span></span> |<span data-ttu-id="53c8d-250">Int64</span><span class="sxs-lookup"><span data-stu-id="53c8d-250">Int64</span></span> |
| <span data-ttu-id="53c8d-251">Real</span><span class="sxs-lookup"><span data-stu-id="53c8d-251">Real</span></span> |<span data-ttu-id="53c8d-252">Enskild</span><span class="sxs-lookup"><span data-stu-id="53c8d-252">Single</span></span> |
| <span data-ttu-id="53c8d-253">dubbla</span><span class="sxs-lookup"><span data-stu-id="53c8d-253">Double</span></span> |<span data-ttu-id="53c8d-254">dubbla</span><span class="sxs-lookup"><span data-stu-id="53c8d-254">Double</span></span> |
| <span data-ttu-id="53c8d-255">flyttal</span><span class="sxs-lookup"><span data-stu-id="53c8d-255">Float</span></span> |<span data-ttu-id="53c8d-256">dubbla</span><span class="sxs-lookup"><span data-stu-id="53c8d-256">Double</span></span> |
| <span data-ttu-id="53c8d-257">Decimal</span><span class="sxs-lookup"><span data-stu-id="53c8d-257">Decimal</span></span> |<span data-ttu-id="53c8d-258">Decimal</span><span class="sxs-lookup"><span data-stu-id="53c8d-258">Decimal</span></span> |
| <span data-ttu-id="53c8d-259">DecimalFloat</span><span class="sxs-lookup"><span data-stu-id="53c8d-259">DecimalFloat</span></span> |<span data-ttu-id="53c8d-260">Decimal</span><span class="sxs-lookup"><span data-stu-id="53c8d-260">Decimal</span></span> |
| <span data-ttu-id="53c8d-261">numeriskt</span><span class="sxs-lookup"><span data-stu-id="53c8d-261">Numeric</span></span> |<span data-ttu-id="53c8d-262">Decimal</span><span class="sxs-lookup"><span data-stu-id="53c8d-262">Decimal</span></span> |
| <span data-ttu-id="53c8d-263">Date</span><span class="sxs-lookup"><span data-stu-id="53c8d-263">Date</span></span> |<span data-ttu-id="53c8d-264">Datum och tid</span><span class="sxs-lookup"><span data-stu-id="53c8d-264">DateTime</span></span> |
| <span data-ttu-id="53c8d-265">Tid</span><span class="sxs-lookup"><span data-stu-id="53c8d-265">Time</span></span> |<span data-ttu-id="53c8d-266">TimeSpan</span><span class="sxs-lookup"><span data-stu-id="53c8d-266">TimeSpan</span></span> |
| <span data-ttu-id="53c8d-267">tidsstämpel</span><span class="sxs-lookup"><span data-stu-id="53c8d-267">Timestamp</span></span> |<span data-ttu-id="53c8d-268">Datum och tid</span><span class="sxs-lookup"><span data-stu-id="53c8d-268">DateTime</span></span> |
| <span data-ttu-id="53c8d-269">XML</span><span class="sxs-lookup"><span data-stu-id="53c8d-269">Xml</span></span> |<span data-ttu-id="53c8d-270">byte]</span><span class="sxs-lookup"><span data-stu-id="53c8d-270">Byte[]</span></span> |
| <span data-ttu-id="53c8d-271">Char</span><span class="sxs-lookup"><span data-stu-id="53c8d-271">Char</span></span> |<span data-ttu-id="53c8d-272">Sträng</span><span class="sxs-lookup"><span data-stu-id="53c8d-272">String</span></span> |
| <span data-ttu-id="53c8d-273">VarChar</span><span class="sxs-lookup"><span data-stu-id="53c8d-273">VarChar</span></span> |<span data-ttu-id="53c8d-274">Sträng</span><span class="sxs-lookup"><span data-stu-id="53c8d-274">String</span></span> |
| <span data-ttu-id="53c8d-275">LongVarChar</span><span class="sxs-lookup"><span data-stu-id="53c8d-275">LongVarChar</span></span> |<span data-ttu-id="53c8d-276">Sträng</span><span class="sxs-lookup"><span data-stu-id="53c8d-276">String</span></span> |
| <span data-ttu-id="53c8d-277">DB2DynArray</span><span class="sxs-lookup"><span data-stu-id="53c8d-277">DB2DynArray</span></span> |<span data-ttu-id="53c8d-278">Sträng</span><span class="sxs-lookup"><span data-stu-id="53c8d-278">String</span></span> |
| <span data-ttu-id="53c8d-279">Binär</span><span class="sxs-lookup"><span data-stu-id="53c8d-279">Binary</span></span> |<span data-ttu-id="53c8d-280">byte]</span><span class="sxs-lookup"><span data-stu-id="53c8d-280">Byte[]</span></span> |
| <span data-ttu-id="53c8d-281">VarBinary</span><span class="sxs-lookup"><span data-stu-id="53c8d-281">VarBinary</span></span> |<span data-ttu-id="53c8d-282">byte]</span><span class="sxs-lookup"><span data-stu-id="53c8d-282">Byte[]</span></span> |
| <span data-ttu-id="53c8d-283">LongVarBinary</span><span class="sxs-lookup"><span data-stu-id="53c8d-283">LongVarBinary</span></span> |<span data-ttu-id="53c8d-284">byte]</span><span class="sxs-lookup"><span data-stu-id="53c8d-284">Byte[]</span></span> |
| <span data-ttu-id="53c8d-285">Bild</span><span class="sxs-lookup"><span data-stu-id="53c8d-285">Graphic</span></span> |<span data-ttu-id="53c8d-286">Sträng</span><span class="sxs-lookup"><span data-stu-id="53c8d-286">String</span></span> |
| <span data-ttu-id="53c8d-287">VarGraphic</span><span class="sxs-lookup"><span data-stu-id="53c8d-287">VarGraphic</span></span> |<span data-ttu-id="53c8d-288">Sträng</span><span class="sxs-lookup"><span data-stu-id="53c8d-288">String</span></span> |
| <span data-ttu-id="53c8d-289">LongVarGraphic</span><span class="sxs-lookup"><span data-stu-id="53c8d-289">LongVarGraphic</span></span> |<span data-ttu-id="53c8d-290">Sträng</span><span class="sxs-lookup"><span data-stu-id="53c8d-290">String</span></span> |
| <span data-ttu-id="53c8d-291">CLOB</span><span class="sxs-lookup"><span data-stu-id="53c8d-291">Clob</span></span> |<span data-ttu-id="53c8d-292">Sträng</span><span class="sxs-lookup"><span data-stu-id="53c8d-292">String</span></span> |
| <span data-ttu-id="53c8d-293">Blob</span><span class="sxs-lookup"><span data-stu-id="53c8d-293">Blob</span></span> |<span data-ttu-id="53c8d-294">byte]</span><span class="sxs-lookup"><span data-stu-id="53c8d-294">Byte[]</span></span> |
| <span data-ttu-id="53c8d-295">DbClob</span><span class="sxs-lookup"><span data-stu-id="53c8d-295">DbClob</span></span> |<span data-ttu-id="53c8d-296">Sträng</span><span class="sxs-lookup"><span data-stu-id="53c8d-296">String</span></span> |
| <span data-ttu-id="53c8d-297">SmallInt</span><span class="sxs-lookup"><span data-stu-id="53c8d-297">SmallInt</span></span> |<span data-ttu-id="53c8d-298">Int16</span><span class="sxs-lookup"><span data-stu-id="53c8d-298">Int16</span></span> |
| <span data-ttu-id="53c8d-299">Integer</span><span class="sxs-lookup"><span data-stu-id="53c8d-299">Integer</span></span> |<span data-ttu-id="53c8d-300">Int32</span><span class="sxs-lookup"><span data-stu-id="53c8d-300">Int32</span></span> |
| <span data-ttu-id="53c8d-301">BigInt</span><span class="sxs-lookup"><span data-stu-id="53c8d-301">BigInt</span></span> |<span data-ttu-id="53c8d-302">Int64</span><span class="sxs-lookup"><span data-stu-id="53c8d-302">Int64</span></span> |
| <span data-ttu-id="53c8d-303">Real</span><span class="sxs-lookup"><span data-stu-id="53c8d-303">Real</span></span> |<span data-ttu-id="53c8d-304">Enskild</span><span class="sxs-lookup"><span data-stu-id="53c8d-304">Single</span></span> |
| <span data-ttu-id="53c8d-305">dubbla</span><span class="sxs-lookup"><span data-stu-id="53c8d-305">Double</span></span> |<span data-ttu-id="53c8d-306">dubbla</span><span class="sxs-lookup"><span data-stu-id="53c8d-306">Double</span></span> |
| <span data-ttu-id="53c8d-307">flyttal</span><span class="sxs-lookup"><span data-stu-id="53c8d-307">Float</span></span> |<span data-ttu-id="53c8d-308">dubbla</span><span class="sxs-lookup"><span data-stu-id="53c8d-308">Double</span></span> |
| <span data-ttu-id="53c8d-309">Decimal</span><span class="sxs-lookup"><span data-stu-id="53c8d-309">Decimal</span></span> |<span data-ttu-id="53c8d-310">Decimal</span><span class="sxs-lookup"><span data-stu-id="53c8d-310">Decimal</span></span> |
| <span data-ttu-id="53c8d-311">DecimalFloat</span><span class="sxs-lookup"><span data-stu-id="53c8d-311">DecimalFloat</span></span> |<span data-ttu-id="53c8d-312">Decimal</span><span class="sxs-lookup"><span data-stu-id="53c8d-312">Decimal</span></span> |
| <span data-ttu-id="53c8d-313">numeriskt</span><span class="sxs-lookup"><span data-stu-id="53c8d-313">Numeric</span></span> |<span data-ttu-id="53c8d-314">Decimal</span><span class="sxs-lookup"><span data-stu-id="53c8d-314">Decimal</span></span> |
| <span data-ttu-id="53c8d-315">Date</span><span class="sxs-lookup"><span data-stu-id="53c8d-315">Date</span></span> |<span data-ttu-id="53c8d-316">Datum och tid</span><span class="sxs-lookup"><span data-stu-id="53c8d-316">DateTime</span></span> |
| <span data-ttu-id="53c8d-317">Tid</span><span class="sxs-lookup"><span data-stu-id="53c8d-317">Time</span></span> |<span data-ttu-id="53c8d-318">TimeSpan</span><span class="sxs-lookup"><span data-stu-id="53c8d-318">TimeSpan</span></span> |
| <span data-ttu-id="53c8d-319">tidsstämpel</span><span class="sxs-lookup"><span data-stu-id="53c8d-319">Timestamp</span></span> |<span data-ttu-id="53c8d-320">Datum och tid</span><span class="sxs-lookup"><span data-stu-id="53c8d-320">DateTime</span></span> |
| <span data-ttu-id="53c8d-321">XML</span><span class="sxs-lookup"><span data-stu-id="53c8d-321">Xml</span></span> |<span data-ttu-id="53c8d-322">byte]</span><span class="sxs-lookup"><span data-stu-id="53c8d-322">Byte[]</span></span> |
| <span data-ttu-id="53c8d-323">Char</span><span class="sxs-lookup"><span data-stu-id="53c8d-323">Char</span></span> |<span data-ttu-id="53c8d-324">Sträng</span><span class="sxs-lookup"><span data-stu-id="53c8d-324">String</span></span> |

## <a name="map-source-toosink-columns"></a><span data-ttu-id="53c8d-325">Mappa källkolumner toosink</span><span class="sxs-lookup"><span data-stu-id="53c8d-325">Map source toosink columns</span></span>
<span data-ttu-id="53c8d-326">hur toomap kolumner i hello källa dataset toocolumns i hello sink dataset Se toolearn [mappa dataset kolumner i Azure Data Factory](data-factory-map-columns.md).</span><span class="sxs-lookup"><span data-stu-id="53c8d-326">toolearn how toomap columns in hello source dataset toocolumns in hello sink dataset, see [Mapping dataset columns in Azure Data Factory](data-factory-map-columns.md).</span></span>

## <a name="repeatable-reads-from-relational-sources"></a><span data-ttu-id="53c8d-327">Repeterbara läsningar från relationella källor</span><span class="sxs-lookup"><span data-stu-id="53c8d-327">Repeatable reads from relational sources</span></span>
<span data-ttu-id="53c8d-328">När du kopierar data från en relationsdatabas, Kom ihåg tooavoid repeterbarhet oönskade resultat.</span><span class="sxs-lookup"><span data-stu-id="53c8d-328">When you copy data from a relational data store, keep repeatability in mind tooavoid unintended outcomes.</span></span> <span data-ttu-id="53c8d-329">I Azure Data Factory, kan du köra en sektor manuellt.</span><span class="sxs-lookup"><span data-stu-id="53c8d-329">In Azure Data Factory, you can rerun a slice manually.</span></span> <span data-ttu-id="53c8d-330">Du kan också konfigurera hello försök **princip** -egenskapen för en dataset toorerun ett segment när ett fel uppstår.</span><span class="sxs-lookup"><span data-stu-id="53c8d-330">You can also configure hello retry **policy** property for a dataset toorerun a slice when a failure occurs.</span></span> <span data-ttu-id="53c8d-331">Kontrollera som hello samma data läses oavsett hur många gånger hello sektorn är kör och oavsett hur du kör hello sektorn.</span><span class="sxs-lookup"><span data-stu-id="53c8d-331">Make sure that hello same data is read no matter how many times hello slice is rerun, and regardless of how you rerun hello slice.</span></span> <span data-ttu-id="53c8d-332">Mer information finns i [Repeatable läser från relationella källor](data-factory-repeatable-copy.md#repeatable-read-from-relational-sources).</span><span class="sxs-lookup"><span data-stu-id="53c8d-332">For more information, see [Repeatable reads from relational sources](data-factory-repeatable-copy.md#repeatable-read-from-relational-sources).</span></span>

## <a name="performance-and-tuning"></a><span data-ttu-id="53c8d-333">Prestanda- och justering</span><span class="sxs-lookup"><span data-stu-id="53c8d-333">Performance and tuning</span></span>
<span data-ttu-id="53c8d-334">Lär dig mer om viktiga faktorer som påverkar prestanda hello Kopieringsaktiviteten och sätt toooptimize prestanda i hello [kopiera aktivitet prestanda och justera guiden](data-factory-copy-activity-performance.md).</span><span class="sxs-lookup"><span data-stu-id="53c8d-334">Learn about key factors that affect hello performance of Copy Activity and ways toooptimize performance in hello [Copy Activity Performance and Tuning Guide](data-factory-copy-activity-performance.md).</span></span>
