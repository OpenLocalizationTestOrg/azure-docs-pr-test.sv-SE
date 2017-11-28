---
title: "aaaMove data från MongoDB med hjälp av Data Factory | Microsoft Docs"
description: "Läs mer om hur toomove data från MongoDB-databas med hjälp av Azure Data Factory."
services: data-factory
documentationcenter: 
author: linda33wj
manager: jhubbard
editor: monicar
ms.assetid: 10ca7d9a-7715-4446-bf59-2d2876584550
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/27/2017
ms.author: jingwang
ms.openlocfilehash: 154e85712f27b978976c7499c43dde9429f124c4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="move-data-from-mongodb-using-azure-data-factory"></a><span data-ttu-id="2511f-103">Flytta data från MongoDB med hjälp av Azure Data Factory</span><span class="sxs-lookup"><span data-stu-id="2511f-103">Move data From MongoDB using Azure Data Factory</span></span>
<span data-ttu-id="2511f-104">Den här artikeln förklarar hur toouse hello Kopieringsaktiviteten i Azure Data Factory toomove data från en lokal MongoDB-databas.</span><span class="sxs-lookup"><span data-stu-id="2511f-104">This article explains how toouse hello Copy Activity in Azure Data Factory toomove data from an on-premises MongoDB database.</span></span> <span data-ttu-id="2511f-105">Den bygger på hello [Data Movement aktiviteter](data-factory-data-movement-activities.md) artikel som ger en allmän översikt över dataflyttning hello kopieringsaktiviteten.</span><span class="sxs-lookup"><span data-stu-id="2511f-105">It builds on hello [Data Movement Activities](data-factory-data-movement-activities.md) article, which presents a general overview of data movement with hello copy activity.</span></span>

<span data-ttu-id="2511f-106">Du kan kopiera data från ett lokalt MongoDB data store tooany stöds sink dataarkiv.</span><span class="sxs-lookup"><span data-stu-id="2511f-106">You can copy data from an on-premises MongoDB data store tooany supported sink data store.</span></span> <span data-ttu-id="2511f-107">En lista över data lagras som stöds när egenskaperna av hello kopieringsaktiviteten Se hello [stöds datalager](data-factory-data-movement-activities.md#supported-data-stores-and-formats) tabell.</span><span class="sxs-lookup"><span data-stu-id="2511f-107">For a list of data stores supported as sinks by hello copy activity, see hello [Supported data stores](data-factory-data-movement-activities.md#supported-data-stores-and-formats) table.</span></span> <span data-ttu-id="2511f-108">Data factory stöder för närvarande endast flytta data från en MongoDB lagra tooother datalager, men inte för att flytta data från andra data lagras tooan MongoDB-datalagret.</span><span class="sxs-lookup"><span data-stu-id="2511f-108">Data factory currently supports only moving data from a MongoDB data store tooother data stores, but not for moving data from other data stores tooan MongoDB datastore.</span></span> 

## <a name="prerequisites"></a><span data-ttu-id="2511f-109">Krav</span><span class="sxs-lookup"><span data-stu-id="2511f-109">Prerequisites</span></span>
<span data-ttu-id="2511f-110">Hello Azure Data Factory-tjänsten toobe kan tooconnect tooyour lokalt MongoDB-databas måste du installera hello följande komponenter:</span><span class="sxs-lookup"><span data-stu-id="2511f-110">For hello Azure Data Factory service toobe able tooconnect tooyour on-premises MongoDB database, you must install hello following components:</span></span>

- <span data-ttu-id="2511f-111">MongoDB-versioner som stöds är: 2.4, 2.6, 3.0 och 3.2.</span><span class="sxs-lookup"><span data-stu-id="2511f-111">Supported MongoDB versions are:  2.4, 2.6, 3.0, and 3.2.</span></span>
- <span data-ttu-id="2511f-112">Data Management Gateway på hello samma dator värdar hello databasen eller på en separat dator tooavoid konkurrerar om resurser med hello-databasen.</span><span class="sxs-lookup"><span data-stu-id="2511f-112">Data Management Gateway on hello same machine that hosts hello database or on a separate machine tooavoid competing for resources with hello database.</span></span> <span data-ttu-id="2511f-113">Data Management Gateway är en programvara som ansluter lokala data sources toocloud tjänster på en säker och hanterad sätt.</span><span class="sxs-lookup"><span data-stu-id="2511f-113">Data Management Gateway is a software that connects on-premises data sources toocloud services in a secure and managed way.</span></span> <span data-ttu-id="2511f-114">Se [Data Management Gateway](data-factory-data-management-gateway.md) artikeln för information om Data Management Gateway.</span><span class="sxs-lookup"><span data-stu-id="2511f-114">See [Data Management Gateway](data-factory-data-management-gateway.md) article for details about Data Management Gateway.</span></span> <span data-ttu-id="2511f-115">Se [flytta data från lokala toocloud](data-factory-move-data-between-onprem-and-cloud.md) artikel stegvisa instruktioner om hur du konfigurerar hello gateway data pipeline toomove data.</span><span class="sxs-lookup"><span data-stu-id="2511f-115">See [Move data from on-premises toocloud](data-factory-move-data-between-onprem-and-cloud.md) article for step-by-step instructions on setting up hello gateway a data pipeline toomove data.</span></span>

    <span data-ttu-id="2511f-116">När du installerar hello gateway installeras automatiskt en Microsoft MongoDB ODBC-drivrutinen används tooconnect tooMongoDB.</span><span class="sxs-lookup"><span data-stu-id="2511f-116">When you install hello gateway, it automatically installs a Microsoft MongoDB ODBC driver used tooconnect tooMongoDB.</span></span>

    > [!NOTE]
    > <span data-ttu-id="2511f-117">Du måste toouse hello gateway tooconnect tooMongoDB även om den finns i Azure IaaS-VM.</span><span class="sxs-lookup"><span data-stu-id="2511f-117">You need toouse hello gateway tooconnect tooMongoDB even if it is hosted in Azure IaaS VMs.</span></span> <span data-ttu-id="2511f-118">Du kan också installera hello gateway-instans i hello IaaS VM om du försöker tooconnect tooan instans av MongoDB som finns i molnet.</span><span class="sxs-lookup"><span data-stu-id="2511f-118">If you are trying tooconnect tooan instance of MongoDB hosted in cloud, you can also install hello gateway instance in hello IaaS VM.</span></span>

## <a name="getting-started"></a><span data-ttu-id="2511f-119">Komma igång</span><span class="sxs-lookup"><span data-stu-id="2511f-119">Getting started</span></span>
<span data-ttu-id="2511f-120">Du kan skapa en pipeline med en kopia-aktivitet som flyttar data från en lokal MongoDB-datalagret med hjälp av olika verktyg/API: er.</span><span class="sxs-lookup"><span data-stu-id="2511f-120">You can create a pipeline with a copy activity that moves data from an on-premises MongoDB data store by using different tools/APIs.</span></span>

<span data-ttu-id="2511f-121">hello enklaste sättet toocreate en pipeline är toouse hello **guiden Kopiera**.</span><span class="sxs-lookup"><span data-stu-id="2511f-121">hello easiest way toocreate a pipeline is toouse hello **Copy Wizard**.</span></span> <span data-ttu-id="2511f-122">Se [Självstudier: skapa en pipeline med hjälp av guiden Kopiera](data-factory-copy-data-wizard-tutorial.md) för en snabb genomgång om hur du skapar en pipeline med hjälp av guiden för hello kopiera data.</span><span class="sxs-lookup"><span data-stu-id="2511f-122">See [Tutorial: Create a pipeline using Copy Wizard](data-factory-copy-data-wizard-tutorial.md) for a quick walkthrough on creating a pipeline using hello Copy data wizard.</span></span>

<span data-ttu-id="2511f-123">Du kan också använda följande verktyg toocreate en pipeline hello: **Azure-portalen**, **Visual Studio**, **Azure PowerShell**, **Azure Resource Manager-mall** , **.NET API**, och **REST API**.</span><span class="sxs-lookup"><span data-stu-id="2511f-123">You can also use hello following tools toocreate a pipeline: **Azure portal**, **Visual Studio**, **Azure PowerShell**, **Azure Resource Manager template**, **.NET API**, and **REST API**.</span></span> <span data-ttu-id="2511f-124">Se [kopiera aktivitet kursen](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) för stegvisa instruktioner toocreate en pipeline med en Kopieringsaktivitet.</span><span class="sxs-lookup"><span data-stu-id="2511f-124">See [Copy activity tutorial](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) for step-by-step instructions toocreate a pipeline with a copy activity.</span></span> 

<span data-ttu-id="2511f-125">Om du använder hello verktyg eller API: er kan utföra du hello följande steg toocreate en pipeline som flyttar data från en källdata lagra tooa sink-datalagret:</span><span class="sxs-lookup"><span data-stu-id="2511f-125">Whether you use hello tools or APIs, you perform hello following steps toocreate a pipeline that moves data from a source data store tooa sink data store:</span></span> 

1. <span data-ttu-id="2511f-126">Skapa **länkade tjänster** toolink indata och utdata lagrar tooyour data factory.</span><span class="sxs-lookup"><span data-stu-id="2511f-126">Create **linked services** toolink input and output data stores tooyour data factory.</span></span>
2. <span data-ttu-id="2511f-127">Skapa **datauppsättningar** toorepresent indata och utdata för hello kopieringsåtgärden.</span><span class="sxs-lookup"><span data-stu-id="2511f-127">Create **datasets** toorepresent input and output data for hello copy operation.</span></span> 
3. <span data-ttu-id="2511f-128">Skapa en **pipeline** med en kopia-aktivitet som tar en datamängd som indata och en dataset som utdata.</span><span class="sxs-lookup"><span data-stu-id="2511f-128">Create a **pipeline** with a copy activity that takes a dataset as an input and a dataset as an output.</span></span> 

<span data-ttu-id="2511f-129">När du använder guiden hello skapas automatiskt JSON definitioner för dessa Data Factory-enheter (länkade tjänster, datauppsättningar och hello pipeline) för dig.</span><span class="sxs-lookup"><span data-stu-id="2511f-129">When you use hello wizard, JSON definitions for these Data Factory entities (linked services, datasets, and hello pipeline) are automatically created for you.</span></span> <span data-ttu-id="2511f-130">När du använder Verktyg/API: er (utom .NET API), kan du definiera dessa Data Factory-enheter med hjälp av hello JSON-format.</span><span class="sxs-lookup"><span data-stu-id="2511f-130">When you use tools/APIs (except .NET API), you define these Data Factory entities by using hello JSON format.</span></span>  <span data-ttu-id="2511f-131">Ett exempel med JSON-definitioner för Data Factory-entiteter som ska använda toocopy data från ett lokalt MongoDB-dataarkiv finns [JSON-exempel: kopiera data från MongoDB tooAzure Blob](#json-example-copy-data-from-mongodb-to-azure-blob) i den här artikeln.</span><span class="sxs-lookup"><span data-stu-id="2511f-131">For a sample with JSON definitions for Data Factory entities that are used toocopy data from an on-premises MongoDB data store, see [JSON example: Copy data from MongoDB tooAzure Blob](#json-example-copy-data-from-mongodb-to-azure-blob) section of this article.</span></span> 

<span data-ttu-id="2511f-132">hello följande avsnitt innehåller information om JSON-egenskaper som används toodefine Data Factory entiteter specifika tooMongoDB källan:</span><span class="sxs-lookup"><span data-stu-id="2511f-132">hello following sections provide details about JSON properties that are used toodefine Data Factory entities specific tooMongoDB source:</span></span>

## <a name="linked-service-properties"></a><span data-ttu-id="2511f-133">Länkad tjänstegenskaper</span><span class="sxs-lookup"><span data-stu-id="2511f-133">Linked service properties</span></span>
<span data-ttu-id="2511f-134">hello följande tabell innehåller beskrivning för JSON-element som är specifika för**OnPremisesMongoDB** länkade tjänsten.</span><span class="sxs-lookup"><span data-stu-id="2511f-134">hello following table provides description for JSON elements specific too**OnPremisesMongoDB** linked service.</span></span>

| <span data-ttu-id="2511f-135">Egenskap</span><span class="sxs-lookup"><span data-stu-id="2511f-135">Property</span></span> | <span data-ttu-id="2511f-136">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="2511f-136">Description</span></span> | <span data-ttu-id="2511f-137">Krävs</span><span class="sxs-lookup"><span data-stu-id="2511f-137">Required</span></span> |
| --- | --- | --- |
| <span data-ttu-id="2511f-138">typ</span><span class="sxs-lookup"><span data-stu-id="2511f-138">type</span></span> |<span data-ttu-id="2511f-139">hello Typegenskapen måste anges till: **OnPremisesMongoDb**</span><span class="sxs-lookup"><span data-stu-id="2511f-139">hello type property must be set to: **OnPremisesMongoDb**</span></span> |<span data-ttu-id="2511f-140">Ja</span><span class="sxs-lookup"><span data-stu-id="2511f-140">Yes</span></span> |
| <span data-ttu-id="2511f-141">server</span><span class="sxs-lookup"><span data-stu-id="2511f-141">server</span></span> |<span data-ttu-id="2511f-142">IP-adressen eller värdnamnet namnet på hello MongoDB-servern.</span><span class="sxs-lookup"><span data-stu-id="2511f-142">IP address or host name of hello MongoDB server.</span></span> |<span data-ttu-id="2511f-143">Ja</span><span class="sxs-lookup"><span data-stu-id="2511f-143">Yes</span></span> |
| <span data-ttu-id="2511f-144">port</span><span class="sxs-lookup"><span data-stu-id="2511f-144">port</span></span> |<span data-ttu-id="2511f-145">TCP-port som hello MongoDB-servern använder toolisten för klientanslutningar.</span><span class="sxs-lookup"><span data-stu-id="2511f-145">TCP port that hello MongoDB server uses toolisten for client connections.</span></span> |<span data-ttu-id="2511f-146">Valfritt, standardvärdet: 27017</span><span class="sxs-lookup"><span data-stu-id="2511f-146">Optional, default value: 27017</span></span> |
| <span data-ttu-id="2511f-147">AuthenticationType</span><span class="sxs-lookup"><span data-stu-id="2511f-147">authenticationType</span></span> |<span data-ttu-id="2511f-148">Grundläggande eller anonym.</span><span class="sxs-lookup"><span data-stu-id="2511f-148">Basic, or Anonymous.</span></span> |<span data-ttu-id="2511f-149">Ja</span><span class="sxs-lookup"><span data-stu-id="2511f-149">Yes</span></span> |
| <span data-ttu-id="2511f-150">användarnamn</span><span class="sxs-lookup"><span data-stu-id="2511f-150">username</span></span> |<span data-ttu-id="2511f-151">Användarens konto tooaccess MongoDB.</span><span class="sxs-lookup"><span data-stu-id="2511f-151">User account tooaccess MongoDB.</span></span> |<span data-ttu-id="2511f-152">Ja (om grundläggande autentisering används).</span><span class="sxs-lookup"><span data-stu-id="2511f-152">Yes (if basic authentication is used).</span></span> |
| <span data-ttu-id="2511f-153">lösenord</span><span class="sxs-lookup"><span data-stu-id="2511f-153">password</span></span> |<span data-ttu-id="2511f-154">Lösenordet för hello.</span><span class="sxs-lookup"><span data-stu-id="2511f-154">Password for hello user.</span></span> |<span data-ttu-id="2511f-155">Ja (om grundläggande autentisering används).</span><span class="sxs-lookup"><span data-stu-id="2511f-155">Yes (if basic authentication is used).</span></span> |
| <span data-ttu-id="2511f-156">authSource</span><span class="sxs-lookup"><span data-stu-id="2511f-156">authSource</span></span> |<span data-ttu-id="2511f-157">Namnet på hello MongoDB-databas som du vill toouse toocheck dina autentiseringsuppgifter för autentisering.</span><span class="sxs-lookup"><span data-stu-id="2511f-157">Name of hello MongoDB database that you want toouse toocheck your credentials for authentication.</span></span> |<span data-ttu-id="2511f-158">Valfritt (om grundläggande autentisering används).</span><span class="sxs-lookup"><span data-stu-id="2511f-158">Optional (if basic authentication is used).</span></span> <span data-ttu-id="2511f-159">standard: använder hello administratörskonto och hello databas som har angetts med egenskapen databaseName.</span><span class="sxs-lookup"><span data-stu-id="2511f-159">default: uses hello admin account and hello database specified using databaseName property.</span></span> |
| <span data-ttu-id="2511f-160">DatabaseName</span><span class="sxs-lookup"><span data-stu-id="2511f-160">databaseName</span></span> |<span data-ttu-id="2511f-161">Namnet på hello MongoDB-databas som du vill tooaccess.</span><span class="sxs-lookup"><span data-stu-id="2511f-161">Name of hello MongoDB database that you want tooaccess.</span></span> |<span data-ttu-id="2511f-162">Ja</span><span class="sxs-lookup"><span data-stu-id="2511f-162">Yes</span></span> |
| <span data-ttu-id="2511f-163">gatewayName</span><span class="sxs-lookup"><span data-stu-id="2511f-163">gatewayName</span></span> |<span data-ttu-id="2511f-164">Namnet på hello-gateway som har åtkomst till hello-datalagret.</span><span class="sxs-lookup"><span data-stu-id="2511f-164">Name of hello gateway that accesses hello data store.</span></span> |<span data-ttu-id="2511f-165">Ja</span><span class="sxs-lookup"><span data-stu-id="2511f-165">Yes</span></span> |
| <span data-ttu-id="2511f-166">encryptedCredential</span><span class="sxs-lookup"><span data-stu-id="2511f-166">encryptedCredential</span></span> |<span data-ttu-id="2511f-167">Autentiseringsuppgifter har krypterats av gateway.</span><span class="sxs-lookup"><span data-stu-id="2511f-167">Credential encrypted by gateway.</span></span> |<span data-ttu-id="2511f-168">Valfri</span><span class="sxs-lookup"><span data-stu-id="2511f-168">Optional</span></span> |

## <a name="dataset-properties"></a><span data-ttu-id="2511f-169">Egenskaper för datamängd</span><span class="sxs-lookup"><span data-stu-id="2511f-169">Dataset properties</span></span>
<span data-ttu-id="2511f-170">En fullständig lista över egenskaper som är tillgängliga för att definiera datauppsättningarna & avsnitt finns hello [skapa datauppsättningar](data-factory-create-datasets.md) artikel.</span><span class="sxs-lookup"><span data-stu-id="2511f-170">For a full list of sections & properties available for defining datasets, see hello [Creating datasets](data-factory-create-datasets.md) article.</span></span> <span data-ttu-id="2511f-171">Avsnitt som struktur, tillgänglighet och princip på en datamängd JSON är liknande för alla typer av dataset (Azure SQL Azure blob, Azure-tabellen, osv.).</span><span class="sxs-lookup"><span data-stu-id="2511f-171">Sections such as structure, availability, and policy of a dataset JSON are similar for all dataset types (Azure SQL, Azure blob, Azure table, etc.).</span></span>

<span data-ttu-id="2511f-172">Hej **typeProperties** avsnitt är olika för varje typ av dataset och ger information om hello platsen för hello data i datalagret hello.</span><span class="sxs-lookup"><span data-stu-id="2511f-172">hello **typeProperties** section is different for each type of dataset and provides information about hello location of hello data in hello data store.</span></span> <span data-ttu-id="2511f-173">Hej typeProperties avsnittet för dataset av typen **MongoDbCollection** har hello följande egenskaper:</span><span class="sxs-lookup"><span data-stu-id="2511f-173">hello typeProperties section for dataset of type **MongoDbCollection** has hello following properties:</span></span>

| <span data-ttu-id="2511f-174">Egenskap</span><span class="sxs-lookup"><span data-stu-id="2511f-174">Property</span></span> | <span data-ttu-id="2511f-175">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="2511f-175">Description</span></span> | <span data-ttu-id="2511f-176">Krävs</span><span class="sxs-lookup"><span data-stu-id="2511f-176">Required</span></span> |
| --- | --- | --- |
| <span data-ttu-id="2511f-177">Samlingsnamn</span><span class="sxs-lookup"><span data-stu-id="2511f-177">collectionName</span></span> |<span data-ttu-id="2511f-178">Namnet på hello-samlingen i MongoDB-databas.</span><span class="sxs-lookup"><span data-stu-id="2511f-178">Name of hello collection in MongoDB database.</span></span> |<span data-ttu-id="2511f-179">Ja</span><span class="sxs-lookup"><span data-stu-id="2511f-179">Yes</span></span> |

## <a name="copy-activity-properties"></a><span data-ttu-id="2511f-180">Kopiera egenskaper för aktivitet</span><span class="sxs-lookup"><span data-stu-id="2511f-180">Copy activity properties</span></span>
<span data-ttu-id="2511f-181">En fullständig lista över avsnitt & egenskaper som är tillgängliga för att definiera aktiviteter finns hello [skapar Pipelines](data-factory-create-pipelines.md) artikel.</span><span class="sxs-lookup"><span data-stu-id="2511f-181">For a full list of sections & properties available for defining activities, see hello [Creating Pipelines](data-factory-create-pipelines.md) article.</span></span> <span data-ttu-id="2511f-182">Egenskaper som namn, beskrivning, ingående och utgående tabeller och principen är tillgängliga för alla typer av aktiviteter.</span><span class="sxs-lookup"><span data-stu-id="2511f-182">Properties such as name, description, input and output tables, and policy are available for all types of activities.</span></span>

<span data-ttu-id="2511f-183">Egenskaper som är tillgängliga i hello **typeProperties** avsnittet hello aktivitet på hello andra sidan varierar med varje aktivitetstyp.</span><span class="sxs-lookup"><span data-stu-id="2511f-183">Properties available in hello **typeProperties** section of hello activity on hello other hand vary with each activity type.</span></span> <span data-ttu-id="2511f-184">För Kopieringsaktivitet kan varierar de beroende på hello typer av datakällor och sänkor.</span><span class="sxs-lookup"><span data-stu-id="2511f-184">For Copy activity, they vary depending on hello types of sources and sinks.</span></span>

<span data-ttu-id="2511f-185">När hello källan är av typen **MongoDbSource** hello följande egenskaper finns i avsnittet typeProperties:</span><span class="sxs-lookup"><span data-stu-id="2511f-185">When hello source is of type **MongoDbSource** hello following properties are available in typeProperties section:</span></span>

| <span data-ttu-id="2511f-186">Egenskap</span><span class="sxs-lookup"><span data-stu-id="2511f-186">Property</span></span> | <span data-ttu-id="2511f-187">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="2511f-187">Description</span></span> | <span data-ttu-id="2511f-188">Tillåtna värden</span><span class="sxs-lookup"><span data-stu-id="2511f-188">Allowed values</span></span> | <span data-ttu-id="2511f-189">Krävs</span><span class="sxs-lookup"><span data-stu-id="2511f-189">Required</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="2511f-190">DocumentDB</span><span class="sxs-lookup"><span data-stu-id="2511f-190">query</span></span> |<span data-ttu-id="2511f-191">Använda hello anpassad fråga tooread data.</span><span class="sxs-lookup"><span data-stu-id="2511f-191">Use hello custom query tooread data.</span></span> |<span data-ttu-id="2511f-192">SQL-92 frågesträngen.</span><span class="sxs-lookup"><span data-stu-id="2511f-192">SQL-92 query string.</span></span> <span data-ttu-id="2511f-193">Till exempel: Välj * från mytable prefix.</span><span class="sxs-lookup"><span data-stu-id="2511f-193">For example: select * from MyTable.</span></span> |<span data-ttu-id="2511f-194">Nej (om **samlingsnamn** av **dataset** har angetts)</span><span class="sxs-lookup"><span data-stu-id="2511f-194">No (if **collectionName** of **dataset** is specified)</span></span> |



## <a name="json-example-copy-data-from-mongodb-tooazure-blob"></a><span data-ttu-id="2511f-195">JSON-exempel: kopiera data från MongoDB tooAzure Blob</span><span class="sxs-lookup"><span data-stu-id="2511f-195">JSON example: Copy data from MongoDB tooAzure Blob</span></span>
<span data-ttu-id="2511f-196">Det här exemplet innehåller exempel JSON definitioner som du kan använda toocreate en pipeline med hjälp av [Azure-portalen](data-factory-copy-activity-tutorial-using-azure-portal.md) eller [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md) eller [Azure PowerShell](data-factory-copy-activity-tutorial-using-powershell.md).</span><span class="sxs-lookup"><span data-stu-id="2511f-196">This example provides sample JSON definitions that you can use toocreate a pipeline by using [Azure portal](data-factory-copy-activity-tutorial-using-azure-portal.md) or [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md) or [Azure PowerShell](data-factory-copy-activity-tutorial-using-powershell.md).</span></span> <span data-ttu-id="2511f-197">Den visar hur toocopy data från en lokal MongoDB tooan Azure Blob Storage.</span><span class="sxs-lookup"><span data-stu-id="2511f-197">It shows how toocopy data from an on-premises MongoDB tooan Azure Blob Storage.</span></span> <span data-ttu-id="2511f-198">Data kan dock vara kopierade tooany av hello sänkor anges [här](data-factory-data-movement-activities.md#supported-data-stores-and-formats) med hello Kopieringsaktiviteten i Azure Data Factory.</span><span class="sxs-lookup"><span data-stu-id="2511f-198">However, data can be copied tooany of hello sinks stated [here](data-factory-data-movement-activities.md#supported-data-stores-and-formats) using hello Copy Activity in Azure Data Factory.</span></span>

<span data-ttu-id="2511f-199">hello exemplet har hello följande data factory-enheter:</span><span class="sxs-lookup"><span data-stu-id="2511f-199">hello sample has hello following data factory entities:</span></span>

1. <span data-ttu-id="2511f-200">En länkad tjänst av typen [OnPremisesMongoDb](#linked-service-properties).</span><span class="sxs-lookup"><span data-stu-id="2511f-200">A linked service of type [OnPremisesMongoDb](#linked-service-properties).</span></span>
2. <span data-ttu-id="2511f-201">En länkad tjänst av typen [AzureStorage](data-factory-azure-blob-connector.md#linked-service-properties).</span><span class="sxs-lookup"><span data-stu-id="2511f-201">A linked service of type [AzureStorage](data-factory-azure-blob-connector.md#linked-service-properties).</span></span>
3. <span data-ttu-id="2511f-202">Indata [dataset](data-factory-create-datasets.md) av typen [MongoDbCollection](#dataset-properties).</span><span class="sxs-lookup"><span data-stu-id="2511f-202">An input [dataset](data-factory-create-datasets.md) of type [MongoDbCollection](#dataset-properties).</span></span>
4. <span data-ttu-id="2511f-203">Utdata [dataset](data-factory-create-datasets.md) av typen [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties).</span><span class="sxs-lookup"><span data-stu-id="2511f-203">An output [dataset](data-factory-create-datasets.md) of type [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties).</span></span>
5. <span data-ttu-id="2511f-204">En [pipeline](data-factory-create-pipelines.md) med Kopieringsaktiviteten som använder [MongoDbSource](#copy-activity-properties) och [BlobSink](data-factory-azure-blob-connector.md#copy-activity-properties).</span><span class="sxs-lookup"><span data-stu-id="2511f-204">A [pipeline](data-factory-create-pipelines.md) with Copy Activity that uses [MongoDbSource](#copy-activity-properties) and [BlobSink](data-factory-azure-blob-connector.md#copy-activity-properties).</span></span>

<span data-ttu-id="2511f-205">hello exemplet kopierar data från ett frågeresultat i MongoDB databasen tooa blob varje timme.</span><span class="sxs-lookup"><span data-stu-id="2511f-205">hello sample copies data from a query result in MongoDB database tooa blob every hour.</span></span> <span data-ttu-id="2511f-206">hello JSON egenskaper som används i exemplen beskrivs i hello-exempel i följande avsnitt.</span><span class="sxs-lookup"><span data-stu-id="2511f-206">hello JSON properties used in these samples are described in sections following hello samples.</span></span>

<span data-ttu-id="2511f-207">Konfigurera som ett första steg hello data management gateway enligt hello instruktionerna i hello [Data Management Gateway](data-factory-data-management-gateway.md) artikel.</span><span class="sxs-lookup"><span data-stu-id="2511f-207">As a first step, setup hello data management gateway as per hello instructions in hello [Data Management Gateway](data-factory-data-management-gateway.md) article.</span></span>

<span data-ttu-id="2511f-208">**MongoDB länkade tjänsten:**</span><span class="sxs-lookup"><span data-stu-id="2511f-208">**MongoDB linked service:**</span></span>

```json
{
    "name": "OnPremisesMongoDbLinkedService",
    "properties":
    {
        "type": "OnPremisesMongoDb",
        "typeProperties":
        {
            "authenticationType": "<Basic or Anonymous>",
            "server": "< hello IP address or host name of hello MongoDB server >",  
            "port": "<hello number of hello TCP port that hello MongoDB server uses toolisten for client connections.>",
            "username": "<username>",
            "password": "<password>",
           "authSource": "< hello database that you want toouse toocheck your credentials for authentication. >",
            "databaseName": "<database name>",
            "gatewayName": "<mygateway>"
        }
    }
}
```

<span data-ttu-id="2511f-209">**Azure Storage länkade tjänsten:**</span><span class="sxs-lookup"><span data-stu-id="2511f-209">**Azure Storage linked service:**</span></span>

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

<span data-ttu-id="2511f-210">**MongoDB inkommande dataset:** inställningen ”externa”: ”true” informerar hello Data Factory-tjänsten hello tabellen är externa toohello data factory och inte tillverkas av en aktivitet i hello data factory.</span><span class="sxs-lookup"><span data-stu-id="2511f-210">**MongoDB input dataset:** Setting “external”: ”true” informs hello Data Factory service that hello table is external toohello data factory and is not produced by an activity in hello data factory.</span></span>

```json
{
     "name":  "MongoDbInputDataset",
    "properties": {
        "type": "MongoDbCollection",
        "linkedServiceName": "OnPremisesMongoDbLinkedService",
        "typeProperties": {
            "collectionName": "<Collection name>"    
        },
        "availability": {
            "frequency": "Hour",
            "interval": 1
        },
        "external": true
    }
}
```

<span data-ttu-id="2511f-211">**Azure Blob utdatauppsättningen:**</span><span class="sxs-lookup"><span data-stu-id="2511f-211">**Azure Blob output dataset:**</span></span>

<span data-ttu-id="2511f-212">Data skrivs tooa nya blob varje timme (frekvens: timme, intervall: 1).</span><span class="sxs-lookup"><span data-stu-id="2511f-212">Data is written tooa new blob every hour (frequency: hour, interval: 1).</span></span> <span data-ttu-id="2511f-213">hello mappsökväg för hello blob utvärderas dynamiskt baserat på hello starttiden för hello-segment som bearbetas.</span><span class="sxs-lookup"><span data-stu-id="2511f-213">hello folder path for hello blob is dynamically evaluated based on hello start time of hello slice that is being processed.</span></span> <span data-ttu-id="2511f-214">hello mappsökväg använder år, månad, dag och timmar delar av hello starttid.</span><span class="sxs-lookup"><span data-stu-id="2511f-214">hello folder path uses year, month, day, and hours parts of hello start time.</span></span>

```json
{
    "name": "AzureBlobOutputDataSet",
    "properties": {
        "type": "AzureBlob",
        "linkedServiceName": "AzureStorageLinkedService",
        "typeProperties": {
            "folderPath": "mycontainer/frommongodb/yearno={Year}/monthno={Month}/dayno={Day}/hourno={Hour}",
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

<span data-ttu-id="2511f-215">**Kopiera aktivitet i en pipeline med MongoDB källa och mottagare för Blob:**</span><span class="sxs-lookup"><span data-stu-id="2511f-215">**Copy activity in a pipeline with MongoDB source and Blob sink:**</span></span>

<span data-ttu-id="2511f-216">hello pipelinen innehåller en kopia-aktivitet som är konfigurerade toouse hello ovan inkommande och utgående datauppsättningar och schemalagda toorun varje timme.</span><span class="sxs-lookup"><span data-stu-id="2511f-216">hello pipeline contains a Copy Activity that is configured toouse hello above input and output datasets and is scheduled toorun every hour.</span></span> <span data-ttu-id="2511f-217">I hello pipeline JSON-definitionen hello **källa** typ har angetts för**MongoDbSource** och **sink** typ har angetts för**BlobSink**.</span><span class="sxs-lookup"><span data-stu-id="2511f-217">In hello pipeline JSON definition, hello **source** type is set too**MongoDbSource** and **sink** type is set too**BlobSink**.</span></span> <span data-ttu-id="2511f-218">hello SQL-frågan som angetts för hello **frågan** egenskapen väljer hello data i hello tidigare timme toocopy.</span><span class="sxs-lookup"><span data-stu-id="2511f-218">hello SQL query specified for hello **query** property selects hello data in hello past hour toocopy.</span></span>

```json
{
    "name": "CopyMongoDBToBlob",
    "properties": {
        "description": "pipeline for copy activity",
        "activities": [
            {
                "type": "Copy",
                "typeProperties": {
                    "source": {
                        "type": "MongoDbSource",
                        "query": "$$Text.Format('select * from  MyTable where LastModifiedDate >= {{ts\'{0:yyyy-MM-dd HH:mm:ss}\'}} AND LastModifiedDate < {{ts\'{1:yyyy-MM-dd HH:mm:ss}\'}}', WindowStart, WindowEnd)"
                    },
                    "sink": {
                        "type": "BlobSink",
                        "writeBatchSize": 0,
                        "writeBatchTimeout": "00:00:00"
                    }
                },
                "inputs": [
                    {
                        "name": "MongoDbInputDataset"
                    }
                ],
                "outputs": [
                    {
                        "name": "AzureBlobOutputDataSet"
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
                "name": "MongoDBToAzureBlob"
            }
        ],
        "start": "2016-06-01T18:00:00Z",
        "end": "2016-06-01T19:00:00Z"
    }
}
```


## <a name="schema-by-data-factory"></a><span data-ttu-id="2511f-219">Schema som Data Factory</span><span class="sxs-lookup"><span data-stu-id="2511f-219">Schema by Data Factory</span></span>
<span data-ttu-id="2511f-220">Azure Data Factory-tjänsten skapar schema från en MongoDB-samling med hjälp av hello senaste 100 dokument i hello samling.</span><span class="sxs-lookup"><span data-stu-id="2511f-220">Azure Data Factory service infers schema from a MongoDB collection by using hello latest 100 documents in hello collection.</span></span> <span data-ttu-id="2511f-221">Om dokumenten 100 inte innehåller fullständig schemat, att vissa kolumner ignoreras under hello kopieringen.</span><span class="sxs-lookup"><span data-stu-id="2511f-221">If these 100 documents do not contain full schema, some columns may be ignored during hello copy operation.</span></span>

## <a name="type-mapping-for-mongodb"></a><span data-ttu-id="2511f-222">Mappning för MongoDB</span><span class="sxs-lookup"><span data-stu-id="2511f-222">Type mapping for MongoDB</span></span>
<span data-ttu-id="2511f-223">Som anges i hello [data movement aktiviteter](data-factory-data-movement-activities.md) artikeln kopieringsaktiviteten utför automatisk konverteringar från källan typer toosink typer med hello följande metod i steg 2:</span><span class="sxs-lookup"><span data-stu-id="2511f-223">As mentioned in hello [data movement activities](data-factory-data-movement-activities.md) article, Copy activity performs automatic type conversions from source types toosink types with hello following 2-step approach:</span></span>

1. <span data-ttu-id="2511f-224">Konvertera från inbyggda typer too.NET källtypen</span><span class="sxs-lookup"><span data-stu-id="2511f-224">Convert from native source types too.NET type</span></span>
2. <span data-ttu-id="2511f-225">Konvertera från .NET typen toonative Mottagartypen</span><span class="sxs-lookup"><span data-stu-id="2511f-225">Convert from .NET type toonative sink type</span></span>

<span data-ttu-id="2511f-226">När du flyttar data tooMongoDB hello efter används mappningar från MongoDB typer too.NET typer.</span><span class="sxs-lookup"><span data-stu-id="2511f-226">When moving data tooMongoDB hello following mappings are used from MongoDB types too.NET types.</span></span>

| <span data-ttu-id="2511f-227">MongoDB-typ</span><span class="sxs-lookup"><span data-stu-id="2511f-227">MongoDB type</span></span> | <span data-ttu-id="2511f-228">.NET framework-typ</span><span class="sxs-lookup"><span data-stu-id="2511f-228">.NET Framework type</span></span> |
| --- | --- |
| <span data-ttu-id="2511f-229">Binär</span><span class="sxs-lookup"><span data-stu-id="2511f-229">Binary</span></span> |<span data-ttu-id="2511f-230">byte]</span><span class="sxs-lookup"><span data-stu-id="2511f-230">Byte[]</span></span> |
| <span data-ttu-id="2511f-231">Booleskt värde</span><span class="sxs-lookup"><span data-stu-id="2511f-231">Boolean</span></span> |<span data-ttu-id="2511f-232">Booleskt värde</span><span class="sxs-lookup"><span data-stu-id="2511f-232">Boolean</span></span> |
| <span data-ttu-id="2511f-233">Date</span><span class="sxs-lookup"><span data-stu-id="2511f-233">Date</span></span> |<span data-ttu-id="2511f-234">Datum och tid</span><span class="sxs-lookup"><span data-stu-id="2511f-234">DateTime</span></span> |
| <span data-ttu-id="2511f-235">NumberDouble</span><span class="sxs-lookup"><span data-stu-id="2511f-235">NumberDouble</span></span> |<span data-ttu-id="2511f-236">dubbla</span><span class="sxs-lookup"><span data-stu-id="2511f-236">Double</span></span> |
| <span data-ttu-id="2511f-237">NumberInt</span><span class="sxs-lookup"><span data-stu-id="2511f-237">NumberInt</span></span> |<span data-ttu-id="2511f-238">Int32</span><span class="sxs-lookup"><span data-stu-id="2511f-238">Int32</span></span> |
| <span data-ttu-id="2511f-239">NumberLong</span><span class="sxs-lookup"><span data-stu-id="2511f-239">NumberLong</span></span> |<span data-ttu-id="2511f-240">Int64</span><span class="sxs-lookup"><span data-stu-id="2511f-240">Int64</span></span> |
| <span data-ttu-id="2511f-241">Objekt-ID</span><span class="sxs-lookup"><span data-stu-id="2511f-241">ObjectID</span></span> |<span data-ttu-id="2511f-242">Sträng</span><span class="sxs-lookup"><span data-stu-id="2511f-242">String</span></span> |
| <span data-ttu-id="2511f-243">Sträng</span><span class="sxs-lookup"><span data-stu-id="2511f-243">String</span></span> |<span data-ttu-id="2511f-244">Sträng</span><span class="sxs-lookup"><span data-stu-id="2511f-244">String</span></span> |
| <span data-ttu-id="2511f-245">UUID</span><span class="sxs-lookup"><span data-stu-id="2511f-245">UUID</span></span> |<span data-ttu-id="2511f-246">GUID</span><span class="sxs-lookup"><span data-stu-id="2511f-246">Guid</span></span> |
| <span data-ttu-id="2511f-247">Objekt</span><span class="sxs-lookup"><span data-stu-id="2511f-247">Object</span></span> |<span data-ttu-id="2511f-248">Renormalized förenkla i kolumner med ”_” som kapslad avgränsare</span><span class="sxs-lookup"><span data-stu-id="2511f-248">Renormalized into flatten columns with “_” as nested separator</span></span> |

> [!NOTE]
> <span data-ttu-id="2511f-249">toolearn om stöd för matriser med virtuella tabeller finns för[stöd för komplexa typer som använder virtuella tabeller](#support-for-complex-types-using-virtual-tables) nedan.</span><span class="sxs-lookup"><span data-stu-id="2511f-249">toolearn about support for arrays using virtual tables, refer too[Support for complex types using virtual tables](#support-for-complex-types-using-virtual-tables) section below.</span></span>

<span data-ttu-id="2511f-250">För närvarande hello följande MongoDB-datatyper stöds inte: DBPointer, JavaScript, Max per minut nyckel, reguljärt uttryck, symboler, Timestamp, Undefined</span><span class="sxs-lookup"><span data-stu-id="2511f-250">Currently, hello following MongoDB data types are not supported: DBPointer, JavaScript, Max/Min key, Regular Expression, Symbol, Timestamp, Undefined</span></span>

## <a name="support-for-complex-types-using-virtual-tables"></a><span data-ttu-id="2511f-251">Stöd för komplexa typer som använder virtuella tabeller</span><span class="sxs-lookup"><span data-stu-id="2511f-251">Support for complex types using virtual tables</span></span>
<span data-ttu-id="2511f-252">Azure Data Factory använder en inbyggd ODBC-drivrutinen tooconnect tooand kopieringsdata från MongoDB-databas.</span><span class="sxs-lookup"><span data-stu-id="2511f-252">Azure Data Factory uses a built-in ODBC driver tooconnect tooand copy data from your MongoDB database.</span></span> <span data-ttu-id="2511f-253">För komplexa typer som matriser eller objekt med olika typer i hello dokument normaliserar hello drivrutinen igen data i motsvarande virtuella tabeller.</span><span class="sxs-lookup"><span data-stu-id="2511f-253">For complex types such as arrays or objects with different types across hello documents, hello driver re-normalizes data into corresponding virtual tables.</span></span> <span data-ttu-id="2511f-254">I synnerhet om en tabell innehåller sådana kolumner, hello-drivrutinen genererar hello följande virtuella tabeller:</span><span class="sxs-lookup"><span data-stu-id="2511f-254">Specifically, if a table contains such columns, hello driver generates hello following virtual tables:</span></span>

* <span data-ttu-id="2511f-255">En **bastabellen**, som innehåller hello samma data som hello verkliga tabell utom hello komplex typkolumner.</span><span class="sxs-lookup"><span data-stu-id="2511f-255">A **base table**, which contains hello same data as hello real table except for hello complex type columns.</span></span> <span data-ttu-id="2511f-256">hello bastabellen använder hello samma namn som hello verkliga tabell som representerar.</span><span class="sxs-lookup"><span data-stu-id="2511f-256">hello base table uses hello same name as hello real table that it represents.</span></span>
* <span data-ttu-id="2511f-257">En **virtuella tabellen** för varje kolumn för komplex typ, som utökar hello kapslade data.</span><span class="sxs-lookup"><span data-stu-id="2511f-257">A **virtual table** for each complex type column, which expands hello nested data.</span></span> <span data-ttu-id="2511f-258">hello virtuella tabeller namnges med hello namnet på hello verkliga tabell, avgränsare ”_” och hello namnet på hello matris eller ett objekt.</span><span class="sxs-lookup"><span data-stu-id="2511f-258">hello virtual tables are named using hello name of hello real table, a separator “_” and hello name of hello array or object.</span></span>

<span data-ttu-id="2511f-259">Virtuella tabeller finns toohello data i hello verkliga tabell, aktiverar hello drivrutinen tooaccess hello Avnormaliserade data.</span><span class="sxs-lookup"><span data-stu-id="2511f-259">Virtual tables refer toohello data in hello real table, enabling hello driver tooaccess hello denormalized data.</span></span> <span data-ttu-id="2511f-260">Se avsnittet nedan information.</span><span class="sxs-lookup"><span data-stu-id="2511f-260">See Example section below details.</span></span> <span data-ttu-id="2511f-261">Du kan komma åt hello innehållet i MongoDB matriser genom att fråga och hello virtuella tabeller.</span><span class="sxs-lookup"><span data-stu-id="2511f-261">You can access hello content of MongoDB arrays by querying and joining hello virtual tables.</span></span>

<span data-ttu-id="2511f-262">Du kan använda hello [guiden Kopiera](data-factory-data-movement-activities.md#create-a-pipeline-with-copy-activity) toointuitively hello listan över tabeller i databasen MongoDB, inklusive hello virtuella tabeller och förhandsgranska hello data i.</span><span class="sxs-lookup"><span data-stu-id="2511f-262">You can use hello [Copy Wizard](data-factory-data-movement-activities.md#create-a-pipeline-with-copy-activity) toointuitively view hello list of tables in MongoDB database including hello virtual tables, and preview hello data inside.</span></span> <span data-ttu-id="2511f-263">Du kan också skapa en fråga i hello guiden Kopiera och validera toosee hello resultatet.</span><span class="sxs-lookup"><span data-stu-id="2511f-263">You can also construct a query in hello Copy Wizard and validate toosee hello result.</span></span>

### <a name="example"></a><span data-ttu-id="2511f-264">Exempel</span><span class="sxs-lookup"><span data-stu-id="2511f-264">Example</span></span>
<span data-ttu-id="2511f-265">Till exempel är ”ExampleTable” nedan en MongoDB-tabell som har en kolumn med en matris med objekt i varje cell – fakturor och en kolumn med en matris med skalära typer – klassificeringar.</span><span class="sxs-lookup"><span data-stu-id="2511f-265">For example, “ExampleTable” below is a MongoDB table that has one column with an array of Objects in each cell – Invoices, and one column with an array of Scalar types – Ratings.</span></span>

| <span data-ttu-id="2511f-266">_id</span><span class="sxs-lookup"><span data-stu-id="2511f-266">_id</span></span> | <span data-ttu-id="2511f-267">Kundens namn</span><span class="sxs-lookup"><span data-stu-id="2511f-267">Customer Name</span></span> | <span data-ttu-id="2511f-268">Fakturor</span><span class="sxs-lookup"><span data-stu-id="2511f-268">Invoices</span></span> | <span data-ttu-id="2511f-269">Servicenivå</span><span class="sxs-lookup"><span data-stu-id="2511f-269">Service Level</span></span> | <span data-ttu-id="2511f-270">Klassificering</span><span class="sxs-lookup"><span data-stu-id="2511f-270">Ratings</span></span> |
| --- | --- | --- | --- | --- |
| <span data-ttu-id="2511f-271">1111</span><span class="sxs-lookup"><span data-stu-id="2511f-271">1111</span></span> |<span data-ttu-id="2511f-272">ABC</span><span class="sxs-lookup"><span data-stu-id="2511f-272">ABC</span></span> |<span data-ttu-id="2511f-273">[{invoice_id: ”123” objektet: ”toaster”, pris: ”456” rabatt: ”0,2”}, {invoice_id: ”124” objektet: ”vara”, pris: rabatt ”1235”: ”0,2”}]</span><span class="sxs-lookup"><span data-stu-id="2511f-273">[{invoice_id:”123”, item:”toaster”, price:”456”, discount:”0.2”}, {invoice_id:”124”, item:”oven”, price: ”1235”, discount: ”0.2”}]</span></span> |<span data-ttu-id="2511f-274">Silver</span><span class="sxs-lookup"><span data-stu-id="2511f-274">Silver</span></span> |<span data-ttu-id="2511f-275">[5,6]</span><span class="sxs-lookup"><span data-stu-id="2511f-275">[5,6]</span></span> |
| <span data-ttu-id="2511f-276">2222</span><span class="sxs-lookup"><span data-stu-id="2511f-276">2222</span></span> |<span data-ttu-id="2511f-277">XYZ</span><span class="sxs-lookup"><span data-stu-id="2511f-277">XYZ</span></span> |<span data-ttu-id="2511f-278">[{invoice_id: objektet ”135”: ”kombinerad kyl”, pris: ”12543” rabatt: ”0,0”}]</span><span class="sxs-lookup"><span data-stu-id="2511f-278">[{invoice_id:”135”, item:”fridge”, price: ”12543”, discount: ”0.0”}]</span></span> |<span data-ttu-id="2511f-279">Guld</span><span class="sxs-lookup"><span data-stu-id="2511f-279">Gold</span></span> |<span data-ttu-id="2511f-280">[1,2]</span><span class="sxs-lookup"><span data-stu-id="2511f-280">[1,2]</span></span> |

<span data-ttu-id="2511f-281">hello drivrutinen skulle generera flera virtuella tabeller toorepresent denna tabell.</span><span class="sxs-lookup"><span data-stu-id="2511f-281">hello driver would generate multiple virtual tables toorepresent this single table.</span></span> <span data-ttu-id="2511f-282">hello första virtuella tabellen är hello bastabellen med namnet ”ExampleTable” nedan.</span><span class="sxs-lookup"><span data-stu-id="2511f-282">hello first virtual table is hello base table named “ExampleTable”, shown below.</span></span> <span data-ttu-id="2511f-283">hello bastabellen innehåller alla hello data för hello ursprungliga tabellen, men hello data från hello matriser har utelämnats och utökas i hello virtuella register.</span><span class="sxs-lookup"><span data-stu-id="2511f-283">hello base table contains all hello data of hello original table, but hello data from hello arrays has been omitted and is expanded in hello virtual tables.</span></span>

| <span data-ttu-id="2511f-284">_id</span><span class="sxs-lookup"><span data-stu-id="2511f-284">_id</span></span> | <span data-ttu-id="2511f-285">Kundens namn</span><span class="sxs-lookup"><span data-stu-id="2511f-285">Customer Name</span></span> | <span data-ttu-id="2511f-286">Servicenivå</span><span class="sxs-lookup"><span data-stu-id="2511f-286">Service Level</span></span> |
| --- | --- | --- |
| <span data-ttu-id="2511f-287">1111</span><span class="sxs-lookup"><span data-stu-id="2511f-287">1111</span></span> |<span data-ttu-id="2511f-288">ABC</span><span class="sxs-lookup"><span data-stu-id="2511f-288">ABC</span></span> |<span data-ttu-id="2511f-289">Silver</span><span class="sxs-lookup"><span data-stu-id="2511f-289">Silver</span></span> |
| <span data-ttu-id="2511f-290">2222</span><span class="sxs-lookup"><span data-stu-id="2511f-290">2222</span></span> |<span data-ttu-id="2511f-291">XYZ</span><span class="sxs-lookup"><span data-stu-id="2511f-291">XYZ</span></span> |<span data-ttu-id="2511f-292">Guld</span><span class="sxs-lookup"><span data-stu-id="2511f-292">Gold</span></span> |

<span data-ttu-id="2511f-293">hello följande tabeller visar hello virtuella tabeller som representerar hello ursprungliga matriser i hello exempel.</span><span class="sxs-lookup"><span data-stu-id="2511f-293">hello following tables show hello virtual tables that represent hello original arrays in hello example.</span></span> <span data-ttu-id="2511f-294">Dessa tabeller innehåller hello följande:</span><span class="sxs-lookup"><span data-stu-id="2511f-294">These tables contain hello following:</span></span>

* <span data-ttu-id="2511f-295">En referens tillbaka toohello ursprungliga primärnyckelkolumnen motsvarande toohello rad hello ursprungliga matrisen (via hello _id kolumn)</span><span class="sxs-lookup"><span data-stu-id="2511f-295">A reference back toohello original primary key column corresponding toohello row of hello original array (via hello _id column)</span></span>
* <span data-ttu-id="2511f-296">Uppgift om hello placeringen av hello inom hello ursprungliga matris</span><span class="sxs-lookup"><span data-stu-id="2511f-296">An indication of hello position of hello data within hello original array</span></span>
* <span data-ttu-id="2511f-297">hello expanderas data för varje element i matrisen hello</span><span class="sxs-lookup"><span data-stu-id="2511f-297">hello expanded data for each element within hello array</span></span>

<span data-ttu-id="2511f-298">Tabell ”ExampleTable_Invoices”:</span><span class="sxs-lookup"><span data-stu-id="2511f-298">Table “ExampleTable_Invoices”:</span></span>

| <span data-ttu-id="2511f-299">_id</span><span class="sxs-lookup"><span data-stu-id="2511f-299">_id</span></span> | <span data-ttu-id="2511f-300">ExampleTable_Invoices_dim1_idx</span><span class="sxs-lookup"><span data-stu-id="2511f-300">ExampleTable_Invoices_dim1_idx</span></span> | <span data-ttu-id="2511f-301">invoice_id</span><span class="sxs-lookup"><span data-stu-id="2511f-301">invoice_id</span></span> | <span data-ttu-id="2511f-302">Objektet</span><span class="sxs-lookup"><span data-stu-id="2511f-302">item</span></span> | <span data-ttu-id="2511f-303">price</span><span class="sxs-lookup"><span data-stu-id="2511f-303">price</span></span> | <span data-ttu-id="2511f-304">Rabatt</span><span class="sxs-lookup"><span data-stu-id="2511f-304">Discount</span></span> |
| --- | --- | --- | --- | --- | --- |
| <span data-ttu-id="2511f-305">1111</span><span class="sxs-lookup"><span data-stu-id="2511f-305">1111</span></span> |<span data-ttu-id="2511f-306">0</span><span class="sxs-lookup"><span data-stu-id="2511f-306">0</span></span> |<span data-ttu-id="2511f-307">123</span><span class="sxs-lookup"><span data-stu-id="2511f-307">123</span></span> |<span data-ttu-id="2511f-308">Toaster</span><span class="sxs-lookup"><span data-stu-id="2511f-308">toaster</span></span> |<span data-ttu-id="2511f-309">456</span><span class="sxs-lookup"><span data-stu-id="2511f-309">456</span></span> |<span data-ttu-id="2511f-310">0.2</span><span class="sxs-lookup"><span data-stu-id="2511f-310">0.2</span></span> |
| <span data-ttu-id="2511f-311">1111</span><span class="sxs-lookup"><span data-stu-id="2511f-311">1111</span></span> |<span data-ttu-id="2511f-312">1</span><span class="sxs-lookup"><span data-stu-id="2511f-312">1</span></span> |<span data-ttu-id="2511f-313">124</span><span class="sxs-lookup"><span data-stu-id="2511f-313">124</span></span> |<span data-ttu-id="2511f-314">vara</span><span class="sxs-lookup"><span data-stu-id="2511f-314">oven</span></span> |<span data-ttu-id="2511f-315">1235</span><span class="sxs-lookup"><span data-stu-id="2511f-315">1235</span></span> |<span data-ttu-id="2511f-316">0.2</span><span class="sxs-lookup"><span data-stu-id="2511f-316">0.2</span></span> |
| <span data-ttu-id="2511f-317">2222</span><span class="sxs-lookup"><span data-stu-id="2511f-317">2222</span></span> |<span data-ttu-id="2511f-318">0</span><span class="sxs-lookup"><span data-stu-id="2511f-318">0</span></span> |<span data-ttu-id="2511f-319">135</span><span class="sxs-lookup"><span data-stu-id="2511f-319">135</span></span> |<span data-ttu-id="2511f-320">kombinerad kyl</span><span class="sxs-lookup"><span data-stu-id="2511f-320">fridge</span></span> |<span data-ttu-id="2511f-321">12543</span><span class="sxs-lookup"><span data-stu-id="2511f-321">12543</span></span> |<span data-ttu-id="2511f-322">0.0</span><span class="sxs-lookup"><span data-stu-id="2511f-322">0.0</span></span> |

<span data-ttu-id="2511f-323">Tabell ”ExampleTable_Ratings”:</span><span class="sxs-lookup"><span data-stu-id="2511f-323">Table “ExampleTable_Ratings”:</span></span>

| <span data-ttu-id="2511f-324">_id</span><span class="sxs-lookup"><span data-stu-id="2511f-324">_id</span></span> | <span data-ttu-id="2511f-325">ExampleTable_Ratings_dim1_idx</span><span class="sxs-lookup"><span data-stu-id="2511f-325">ExampleTable_Ratings_dim1_idx</span></span> | <span data-ttu-id="2511f-326">ExampleTable_Ratings</span><span class="sxs-lookup"><span data-stu-id="2511f-326">ExampleTable_Ratings</span></span> |
| --- | --- | --- |
| <span data-ttu-id="2511f-327">1111</span><span class="sxs-lookup"><span data-stu-id="2511f-327">1111</span></span> |<span data-ttu-id="2511f-328">0</span><span class="sxs-lookup"><span data-stu-id="2511f-328">0</span></span> |<span data-ttu-id="2511f-329">5</span><span class="sxs-lookup"><span data-stu-id="2511f-329">5</span></span> |
| <span data-ttu-id="2511f-330">1111</span><span class="sxs-lookup"><span data-stu-id="2511f-330">1111</span></span> |<span data-ttu-id="2511f-331">1</span><span class="sxs-lookup"><span data-stu-id="2511f-331">1</span></span> |<span data-ttu-id="2511f-332">6</span><span class="sxs-lookup"><span data-stu-id="2511f-332">6</span></span> |
| <span data-ttu-id="2511f-333">2222</span><span class="sxs-lookup"><span data-stu-id="2511f-333">2222</span></span> |<span data-ttu-id="2511f-334">0</span><span class="sxs-lookup"><span data-stu-id="2511f-334">0</span></span> |<span data-ttu-id="2511f-335">1</span><span class="sxs-lookup"><span data-stu-id="2511f-335">1</span></span> |
| <span data-ttu-id="2511f-336">2222</span><span class="sxs-lookup"><span data-stu-id="2511f-336">2222</span></span> |<span data-ttu-id="2511f-337">1</span><span class="sxs-lookup"><span data-stu-id="2511f-337">1</span></span> |<span data-ttu-id="2511f-338">2</span><span class="sxs-lookup"><span data-stu-id="2511f-338">2</span></span> |

## <a name="map-source-toosink-columns"></a><span data-ttu-id="2511f-339">Mappa källkolumner toosink</span><span class="sxs-lookup"><span data-stu-id="2511f-339">Map source toosink columns</span></span>
<span data-ttu-id="2511f-340">toolearn mappning tabellkolumner i källan dataset toocolumns i sink dataset finns [mappa dataset kolumner i Azure Data Factory](data-factory-map-columns.md).</span><span class="sxs-lookup"><span data-stu-id="2511f-340">toolearn about mapping columns in source dataset toocolumns in sink dataset, see [Mapping dataset columns in Azure Data Factory](data-factory-map-columns.md).</span></span>

## <a name="repeatable-read-from-relational-sources"></a><span data-ttu-id="2511f-341">Upprepbar läsning från relationella källor</span><span class="sxs-lookup"><span data-stu-id="2511f-341">Repeatable read from relational sources</span></span>
<span data-ttu-id="2511f-342">När du kopierar data från relationella datalager, Kom ihåg tooavoid repeterbarhet oönskade resultat.</span><span class="sxs-lookup"><span data-stu-id="2511f-342">When copying data from relational data stores, keep repeatability in mind tooavoid unintended outcomes.</span></span> <span data-ttu-id="2511f-343">I Azure Data Factory, kan du köra en sektor manuellt.</span><span class="sxs-lookup"><span data-stu-id="2511f-343">In Azure Data Factory, you can rerun a slice manually.</span></span> <span data-ttu-id="2511f-344">Du kan också konfigurera i principen för en dataset så att ett segment som körs när ett fel uppstår.</span><span class="sxs-lookup"><span data-stu-id="2511f-344">You can also configure retry policy for a dataset so that a slice is rerun when a failure occurs.</span></span> <span data-ttu-id="2511f-345">När ett segment körs på antingen sätt måste toomake att som hello samma data läses oavsett hur många gånger ett segment körs.</span><span class="sxs-lookup"><span data-stu-id="2511f-345">When a slice is rerun in either way, you need toomake sure that hello same data is read no matter how many times a slice is run.</span></span> <span data-ttu-id="2511f-346">Se [Repeatable läsa från relationella källor](data-factory-repeatable-copy.md#repeatable-read-from-relational-sources).</span><span class="sxs-lookup"><span data-stu-id="2511f-346">See [Repeatable read from relational sources](data-factory-repeatable-copy.md#repeatable-read-from-relational-sources).</span></span>

## <a name="performance-and-tuning"></a><span data-ttu-id="2511f-347">Prestanda och finjustering</span><span class="sxs-lookup"><span data-stu-id="2511f-347">Performance and Tuning</span></span>
<span data-ttu-id="2511f-348">Se [kopiera aktivitet prestanda och justera guiden](data-factory-copy-activity-performance.md) toolearn om nyckeln faktorer som påverkan prestanda för flytt av data (Kopieringsaktiviteten) i Azure Data Factory och olika sätt toooptimize den.</span><span class="sxs-lookup"><span data-stu-id="2511f-348">See [Copy Activity Performance & Tuning Guide](data-factory-copy-activity-performance.md) toolearn about key factors that impact performance of data movement (Copy Activity) in Azure Data Factory and various ways toooptimize it.</span></span>

## <a name="next-steps"></a><span data-ttu-id="2511f-349">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="2511f-349">Next Steps</span></span>
<span data-ttu-id="2511f-350">Se [flytta data mellan lokalt och i molnet](data-factory-move-data-between-onprem-and-cloud.md) artikel stegvisa instruktioner för att skapa en pipeline för data som flyttas från ett lokalt datalager tooan Azure datalagret.</span><span class="sxs-lookup"><span data-stu-id="2511f-350">See [Move data between on-premises and cloud](data-factory-move-data-between-onprem-and-cloud.md) article for step-by-step instructions for creating a data pipeline that moves data from an on-premises data store tooan Azure data store.</span></span>
