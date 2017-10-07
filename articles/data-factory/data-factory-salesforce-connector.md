---
title: "aaaMove data från Salesforce med hjälp av Data Factory | Microsoft Docs"
description: "Mer information om hur toomove data från Salesforce med hjälp av Azure Data Factory."
services: data-factory
documentationcenter: 
author: linda33wj
manager: jhubbard
editor: monicar
ms.assetid: dbe3bfd6-fa6a-491a-9638-3a9a10d396d1
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/04/2017
ms.author: jingwang
ms.openlocfilehash: c1bde2a333f5a3c0a995eb8c13ecf585132888b9
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="move-data-from-salesforce-by-using-azure-data-factory"></a><span data-ttu-id="a3624-103">Flytta data från Salesforce med hjälp av Azure Data Factory</span><span class="sxs-lookup"><span data-stu-id="a3624-103">Move data from Salesforce by using Azure Data Factory</span></span>
<span data-ttu-id="a3624-104">Den här artikeln beskrivs hur du kan använda Kopieringsaktiviteten i ett Azure data factory toocopy data från datalagret för Salesforce tooany som visas under hello Sink-kolumn i hello [källor och sänkor stöds](data-factory-data-movement-activities.md#supported-data-stores-and-formats) tabell.</span><span class="sxs-lookup"><span data-stu-id="a3624-104">This article outlines how you can use Copy Activity in an Azure data factory toocopy data from Salesforce tooany data store that is listed under hello Sink column in hello [supported sources and sinks](data-factory-data-movement-activities.md#supported-data-stores-and-formats) table.</span></span> <span data-ttu-id="a3624-105">Den här artikeln bygger på hello [data movement aktiviteter](data-factory-data-movement-activities.md) artikel som ger en allmän översikt över dataflyttning stöds data store kombinationer och Kopieringsaktivitet.</span><span class="sxs-lookup"><span data-stu-id="a3624-105">This article builds on hello [data movement activities](data-factory-data-movement-activities.md) article, which presents a general overview of data movement with Copy Activity and supported data store combinations.</span></span>

<span data-ttu-id="a3624-106">Azure Data Factory stöder för närvarande endast flytta data från Salesforce för[stöds sink datalager](data-factory-data-movement-activities.md#supported-data-stores-and-formats), men stöder inte flytta data från andra data lagras tooSalesforce.</span><span class="sxs-lookup"><span data-stu-id="a3624-106">Azure Data Factory currently supports only moving data from Salesforce too[supported sink data stores](data-factory-data-movement-activities.md#supported-data-stores-and-formats), but does not support moving data from other data stores tooSalesforce.</span></span>

## <a name="supported-versions"></a><span data-ttu-id="a3624-107">Versioner som stöds</span><span class="sxs-lookup"><span data-stu-id="a3624-107">Supported versions</span></span>
<span data-ttu-id="a3624-108">Den här anslutningen har stöd för följande versioner av Salesforce hello: Developer Edition, Professional Edition, Enterprise Edition eller obegränsade Edition.</span><span class="sxs-lookup"><span data-stu-id="a3624-108">This connector supports hello following editions of Salesforce: Developer Edition, Professional Edition, Enterprise Edition, or Unlimited Edition.</span></span> <span data-ttu-id="a3624-109">Och kopiera från Salesforce produktion, sandbox och anpassade domäner.</span><span class="sxs-lookup"><span data-stu-id="a3624-109">And it supports copying from Salesforce production, sandbox and custom domain.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="a3624-110">Krav</span><span class="sxs-lookup"><span data-stu-id="a3624-110">Prerequisites</span></span>
* <span data-ttu-id="a3624-111">API-behörighet måste aktiveras.</span><span class="sxs-lookup"><span data-stu-id="a3624-111">API permission must be enabled.</span></span> <span data-ttu-id="a3624-112">Se [hur aktivera API-åtkomst i Salesforce av behörighetsgrupp?](https://www.data2crm.com/migration/faqs/enable-api-access-salesforce-permission-set/)</span><span class="sxs-lookup"><span data-stu-id="a3624-112">See [How do I enable API access in Salesforce by permission set?](https://www.data2crm.com/migration/faqs/enable-api-access-salesforce-permission-set/)</span></span>
* <span data-ttu-id="a3624-113">toocopy data från Salesforce tooon lokala datalager, du måste ha minst Data Management Gateway 2.0 är installerat i din lokala miljö.</span><span class="sxs-lookup"><span data-stu-id="a3624-113">toocopy data from Salesforce tooon-premises data stores, you must have at least Data Management Gateway 2.0 installed in your on-premises environment.</span></span>

## <a name="salesforce-request-limits"></a><span data-ttu-id="a3624-114">Salesforce begärandebegränsningar</span><span class="sxs-lookup"><span data-stu-id="a3624-114">Salesforce request limits</span></span>
<span data-ttu-id="a3624-115">Salesforce har begränsningar för både förfrågningarna API och samtidiga API-begäranden.</span><span class="sxs-lookup"><span data-stu-id="a3624-115">Salesforce has limits for both total API requests and concurrent API requests.</span></span> <span data-ttu-id="a3624-116">Observera följande punkter hello:</span><span class="sxs-lookup"><span data-stu-id="a3624-116">Note hello following points:</span></span>

- <span data-ttu-id="a3624-117">Om hello antalet samtidiga begäranden överskrider gränsen hello, begränsning inträffar och slumpmässiga fel visas.</span><span class="sxs-lookup"><span data-stu-id="a3624-117">If hello number of concurrent requests exceeds hello limit, throttling occurs and you will see random failures.</span></span>
- <span data-ttu-id="a3624-118">Om hello Totalt antal begäranden överskrider gränsen hello, blockeras hello Salesforce-konto i 24 timmar.</span><span class="sxs-lookup"><span data-stu-id="a3624-118">If hello total number of requests exceeds hello limit, hello Salesforce account will be blocked for 24 hours.</span></span>

<span data-ttu-id="a3624-119">Du kan också få hello ”REQUEST_LIMIT_EXCEEDED” fel i båda scenarierna.</span><span class="sxs-lookup"><span data-stu-id="a3624-119">You might also receive hello “REQUEST_LIMIT_EXCEEDED“ error in both scenarios.</span></span> <span data-ttu-id="a3624-120">I avsnittet hello ”API Förfrågningsgränser” i hello [Salesforce Developer gränser](http://resources.docs.salesforce.com/200/20/en-us/sfdc/pdf/salesforce_app_limits_cheatsheet.pdf) artikeln för information.</span><span class="sxs-lookup"><span data-stu-id="a3624-120">See hello "API Request Limits" section in hello [Salesforce Developer Limits](http://resources.docs.salesforce.com/200/20/en-us/sfdc/pdf/salesforce_app_limits_cheatsheet.pdf) article for details.</span></span>

## <a name="getting-started"></a><span data-ttu-id="a3624-121">Komma igång</span><span class="sxs-lookup"><span data-stu-id="a3624-121">Getting started</span></span>
<span data-ttu-id="a3624-122">Du kan skapa en pipeline med en kopia-aktivitet som flyttar data från Salesforce med hjälp av olika verktyg/API: er.</span><span class="sxs-lookup"><span data-stu-id="a3624-122">You can create a pipeline with a copy activity that moves data from Salesforce by using different tools/APIs.</span></span>

<span data-ttu-id="a3624-123">hello enklaste sättet toocreate en pipeline är toouse hello **guiden Kopiera**.</span><span class="sxs-lookup"><span data-stu-id="a3624-123">hello easiest way toocreate a pipeline is toouse hello **Copy Wizard**.</span></span> <span data-ttu-id="a3624-124">Se [Självstudier: skapa en pipeline med hjälp av guiden Kopiera](data-factory-copy-data-wizard-tutorial.md) för en snabb genomgång om hur du skapar en pipeline med hjälp av guiden för hello kopiera data.</span><span class="sxs-lookup"><span data-stu-id="a3624-124">See [Tutorial: Create a pipeline using Copy Wizard](data-factory-copy-data-wizard-tutorial.md) for a quick walkthrough on creating a pipeline using hello Copy data wizard.</span></span>

<span data-ttu-id="a3624-125">Du kan också använda följande verktyg toocreate en pipeline hello: **Azure-portalen**, **Visual Studio**, **Azure PowerShell**, **Azure Resource Manager-mall** , **.NET API**, och **REST API**.</span><span class="sxs-lookup"><span data-stu-id="a3624-125">You can also use hello following tools toocreate a pipeline: **Azure portal**, **Visual Studio**, **Azure PowerShell**, **Azure Resource Manager template**, **.NET API**, and **REST API**.</span></span> <span data-ttu-id="a3624-126">Se [kopiera aktivitet kursen](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) för stegvisa instruktioner toocreate en pipeline med en Kopieringsaktivitet.</span><span class="sxs-lookup"><span data-stu-id="a3624-126">See [Copy activity tutorial](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) for step-by-step instructions toocreate a pipeline with a copy activity.</span></span> 

<span data-ttu-id="a3624-127">Om du använder hello verktyg eller API: er kan utföra du hello följande steg toocreate en pipeline som flyttar data från en källdata lagra tooa sink-datalagret:</span><span class="sxs-lookup"><span data-stu-id="a3624-127">Whether you use hello tools or APIs, you perform hello following steps toocreate a pipeline that moves data from a source data store tooa sink data store:</span></span> 

1. <span data-ttu-id="a3624-128">Skapa **länkade tjänster** toolink indata och utdata lagrar tooyour data factory.</span><span class="sxs-lookup"><span data-stu-id="a3624-128">Create **linked services** toolink input and output data stores tooyour data factory.</span></span>
2. <span data-ttu-id="a3624-129">Skapa **datauppsättningar** toorepresent indata och utdata för hello kopieringsåtgärden.</span><span class="sxs-lookup"><span data-stu-id="a3624-129">Create **datasets** toorepresent input and output data for hello copy operation.</span></span> 
3. <span data-ttu-id="a3624-130">Skapa en **pipeline** med en kopia-aktivitet som tar en datamängd som indata och en dataset som utdata.</span><span class="sxs-lookup"><span data-stu-id="a3624-130">Create a **pipeline** with a copy activity that takes a dataset as an input and a dataset as an output.</span></span> 

<span data-ttu-id="a3624-131">När du använder guiden hello skapas automatiskt JSON definitioner för dessa Data Factory-enheter (länkade tjänster, datauppsättningar och hello pipeline) för dig.</span><span class="sxs-lookup"><span data-stu-id="a3624-131">When you use hello wizard, JSON definitions for these Data Factory entities (linked services, datasets, and hello pipeline) are automatically created for you.</span></span> <span data-ttu-id="a3624-132">När du använder Verktyg/API: er (utom .NET API), kan du definiera dessa Data Factory-enheter med hjälp av hello JSON-format.</span><span class="sxs-lookup"><span data-stu-id="a3624-132">When you use tools/APIs (except .NET API), you define these Data Factory entities by using hello JSON format.</span></span>  <span data-ttu-id="a3624-133">Ett exempel med JSON-definitioner för Data Factory-entiteter som ska använda toocopy data från Salesforce finns [JSON-exempel: kopiera data från Salesforce tooAzure Blob](#json-example-copy-data-from-salesforce-to-azure-blob) i den här artikeln.</span><span class="sxs-lookup"><span data-stu-id="a3624-133">For a sample with JSON definitions for Data Factory entities that are used toocopy data from Salesforce, see [JSON example: Copy data from Salesforce tooAzure Blob](#json-example-copy-data-from-salesforce-to-azure-blob) section of this article.</span></span> 

<span data-ttu-id="a3624-134">hello följande avsnitt innehåller information om JSON-egenskaper som är specifika tooSalesforce för används toodefine Data Factory-enheter:</span><span class="sxs-lookup"><span data-stu-id="a3624-134">hello following sections provide details about JSON properties that are used toodefine Data Factory entities specific tooSalesforce:</span></span> 

## <a name="linked-service-properties"></a><span data-ttu-id="a3624-135">Länkad tjänstegenskaper</span><span class="sxs-lookup"><span data-stu-id="a3624-135">Linked service properties</span></span>
<span data-ttu-id="a3624-136">hello i den följande tabellen finns beskrivningar JSON-element som är specifika toohello Salesforce länkade tjänsten.</span><span class="sxs-lookup"><span data-stu-id="a3624-136">hello following table provides descriptions for JSON elements that are specific toohello Salesforce linked service.</span></span>

| <span data-ttu-id="a3624-137">Egenskap</span><span class="sxs-lookup"><span data-stu-id="a3624-137">Property</span></span> | <span data-ttu-id="a3624-138">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="a3624-138">Description</span></span> | <span data-ttu-id="a3624-139">Krävs</span><span class="sxs-lookup"><span data-stu-id="a3624-139">Required</span></span> |
| --- | --- | --- |
| <span data-ttu-id="a3624-140">typ</span><span class="sxs-lookup"><span data-stu-id="a3624-140">type</span></span> |<span data-ttu-id="a3624-141">hello Typegenskapen måste anges till: **Salesforce**.</span><span class="sxs-lookup"><span data-stu-id="a3624-141">hello type property must be set to: **Salesforce**.</span></span> |<span data-ttu-id="a3624-142">Ja</span><span class="sxs-lookup"><span data-stu-id="a3624-142">Yes</span></span> |
| <span data-ttu-id="a3624-143">environmentUrl</span><span class="sxs-lookup"><span data-stu-id="a3624-143">environmentUrl</span></span> | <span data-ttu-id="a3624-144">Ange hello URL Salesforce-instans.</span><span class="sxs-lookup"><span data-stu-id="a3624-144">Specify hello URL of Salesforce instance.</span></span> <br><br> <span data-ttu-id="a3624-145">– Standardvärdet är ”https://login.salesforce.com”.</span><span class="sxs-lookup"><span data-stu-id="a3624-145">- Default is "https://login.salesforce.com".</span></span> <br> <span data-ttu-id="a3624-146">-toocopy data från sandbox, ange ”https://test.salesforce.com”.</span><span class="sxs-lookup"><span data-stu-id="a3624-146">- toocopy data from sandbox, specify "https://test.salesforce.com".</span></span> <br> <span data-ttu-id="a3624-147">-toocopy data från domän, till exempel ange ”https://[domain].my.salesforce.com”.</span><span class="sxs-lookup"><span data-stu-id="a3624-147">- toocopy data from custom domain, specify, for example, "https://[domain].my.salesforce.com".</span></span> |<span data-ttu-id="a3624-148">Nej</span><span class="sxs-lookup"><span data-stu-id="a3624-148">No</span></span> |
| <span data-ttu-id="a3624-149">användarnamn</span><span class="sxs-lookup"><span data-stu-id="a3624-149">username</span></span> |<span data-ttu-id="a3624-150">Ange ett användarnamn för hello användarkonto.</span><span class="sxs-lookup"><span data-stu-id="a3624-150">Specify a user name for hello user account.</span></span> |<span data-ttu-id="a3624-151">Ja</span><span class="sxs-lookup"><span data-stu-id="a3624-151">Yes</span></span> |
| <span data-ttu-id="a3624-152">lösenord</span><span class="sxs-lookup"><span data-stu-id="a3624-152">password</span></span> |<span data-ttu-id="a3624-153">Ange ett lösenord för hello användarkonto.</span><span class="sxs-lookup"><span data-stu-id="a3624-153">Specify a password for hello user account.</span></span> |<span data-ttu-id="a3624-154">Ja</span><span class="sxs-lookup"><span data-stu-id="a3624-154">Yes</span></span> |
| <span data-ttu-id="a3624-155">securityToken</span><span class="sxs-lookup"><span data-stu-id="a3624-155">securityToken</span></span> |<span data-ttu-id="a3624-156">Ange en säkerhetstoken för hello användarkonto.</span><span class="sxs-lookup"><span data-stu-id="a3624-156">Specify a security token for hello user account.</span></span> <span data-ttu-id="a3624-157">Se [hämta säkerhetstoken](https://help.salesforce.com/apex/HTViewHelpDoc?id=user_security_token.htm) anvisningar för hur tooreset/hämta en säkerhetstoken.</span><span class="sxs-lookup"><span data-stu-id="a3624-157">See [Get security token](https://help.salesforce.com/apex/HTViewHelpDoc?id=user_security_token.htm) for instructions on how tooreset/get a security token.</span></span> <span data-ttu-id="a3624-158">i allmänhet finns i toolearn om säkerhetstoken [säkerhet och hello API](https://developer.salesforce.com/docs/atlas.en-us.api.meta/api/sforce_api_concepts_security.htm).</span><span class="sxs-lookup"><span data-stu-id="a3624-158">toolearn about security tokens in general, see [Security and hello API](https://developer.salesforce.com/docs/atlas.en-us.api.meta/api/sforce_api_concepts_security.htm).</span></span> |<span data-ttu-id="a3624-159">Ja</span><span class="sxs-lookup"><span data-stu-id="a3624-159">Yes</span></span> |

## <a name="dataset-properties"></a><span data-ttu-id="a3624-160">Egenskaper för datamängd</span><span class="sxs-lookup"><span data-stu-id="a3624-160">Dataset properties</span></span>
<span data-ttu-id="a3624-161">En fullständig lista över avsnitt och egenskaper som är tillgängliga för att definiera datauppsättningar finns hello [skapa datauppsättningar](data-factory-create-datasets.md) artikel.</span><span class="sxs-lookup"><span data-stu-id="a3624-161">For a full list of sections and properties that are available for defining datasets, see hello [Creating datasets](data-factory-create-datasets.md) article.</span></span> <span data-ttu-id="a3624-162">Avsnitt som struktur, tillgänglighet och princip på en datamängd JSON är liknande för alla typer av datauppsättningen (Azure SQL Azure blob, Azure-tabellen och så vidare).</span><span class="sxs-lookup"><span data-stu-id="a3624-162">Sections such as structure, availability, and policy of a dataset JSON are similar for all dataset types (Azure SQL, Azure blob, Azure table, and so on).</span></span>

<span data-ttu-id="a3624-163">Hej **typeProperties** avsnitt är olika för varje typ av dataset och ger information om hello platsen för hello data i datalagret hello.</span><span class="sxs-lookup"><span data-stu-id="a3624-163">hello **typeProperties** section is different for each type of dataset and provides information about hello location of hello data in hello data store.</span></span> <span data-ttu-id="a3624-164">Hej typeProperties avsnittet för en dataset av hello typen **RelationalTable** har hello följande egenskaper:</span><span class="sxs-lookup"><span data-stu-id="a3624-164">hello typeProperties section for a dataset of hello type **RelationalTable** has hello following properties:</span></span>

| <span data-ttu-id="a3624-165">Egenskap</span><span class="sxs-lookup"><span data-stu-id="a3624-165">Property</span></span> | <span data-ttu-id="a3624-166">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="a3624-166">Description</span></span> | <span data-ttu-id="a3624-167">Krävs</span><span class="sxs-lookup"><span data-stu-id="a3624-167">Required</span></span> |
| --- | --- | --- |
| <span data-ttu-id="a3624-168">tableName</span><span class="sxs-lookup"><span data-stu-id="a3624-168">tableName</span></span> |<span data-ttu-id="a3624-169">Namnet på hello tabell i Salesforce.</span><span class="sxs-lookup"><span data-stu-id="a3624-169">Name of hello table in Salesforce.</span></span> |<span data-ttu-id="a3624-170">Nej (om en **frågan** av **RelationalSource** har angetts)</span><span class="sxs-lookup"><span data-stu-id="a3624-170">No (if a **query** of **RelationalSource** is specified)</span></span> |

> [!IMPORTANT]
> <span data-ttu-id="a3624-171">Hej ”__c” tillhör hello API namn krävs för alla anpassade objekt.</span><span class="sxs-lookup"><span data-stu-id="a3624-171">hello "__c" part of hello API Name is needed for any custom object.</span></span>

![Namnet på data Factory - anslutning Salesforce - API](media/data-factory-salesforce-connector/data-factory-salesforce-api-name.png)

## <a name="copy-activity-properties"></a><span data-ttu-id="a3624-173">Kopiera egenskaper för aktivitet</span><span class="sxs-lookup"><span data-stu-id="a3624-173">Copy activity properties</span></span>
<span data-ttu-id="a3624-174">En fullständig lista över avsnitt och egenskaper som är tillgängliga för att definiera aktiviteter finns hello [skapar pipelines](data-factory-create-pipelines.md) artikel.</span><span class="sxs-lookup"><span data-stu-id="a3624-174">For a full list of sections and properties that are available for defining activities, see hello [Creating pipelines](data-factory-create-pipelines.md) article.</span></span> <span data-ttu-id="a3624-175">Egenskaper som namn, beskrivning, indata och utdata tabeller och olika principer är tillgängliga för alla typer av aktiviteter.</span><span class="sxs-lookup"><span data-stu-id="a3624-175">Properties like name, description, input and output tables, and various policies are available for all types of activities.</span></span>

<span data-ttu-id="a3624-176">hello-egenskaper som är tillgängliga i hello typeProperties avsnittet hello aktivitet på hello andra sidan, varierar med varje aktivitetstyp.</span><span class="sxs-lookup"><span data-stu-id="a3624-176">hello properties that are available in hello typeProperties section of hello activity, on hello other hand, vary with each activity type.</span></span> <span data-ttu-id="a3624-177">För Kopieringsaktivitet kan varierar de beroende på hello typer av datakällor och sänkor.</span><span class="sxs-lookup"><span data-stu-id="a3624-177">For Copy Activity, they vary depending on hello types of sources and sinks.</span></span>

<span data-ttu-id="a3624-178">I en Kopieringsaktivitet när hello källan är av typen hello **RelationalSource** (som omfattar Salesforce), hello följande egenskaper finns i avsnittet typeProperties:</span><span class="sxs-lookup"><span data-stu-id="a3624-178">In copy activity, when hello source is of hello type **RelationalSource** (which includes Salesforce), hello following properties are available in typeProperties section:</span></span>

| <span data-ttu-id="a3624-179">Egenskap</span><span class="sxs-lookup"><span data-stu-id="a3624-179">Property</span></span> | <span data-ttu-id="a3624-180">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="a3624-180">Description</span></span> | <span data-ttu-id="a3624-181">Tillåtna värden</span><span class="sxs-lookup"><span data-stu-id="a3624-181">Allowed values</span></span> | <span data-ttu-id="a3624-182">Krävs</span><span class="sxs-lookup"><span data-stu-id="a3624-182">Required</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="a3624-183">DocumentDB</span><span class="sxs-lookup"><span data-stu-id="a3624-183">query</span></span> |<span data-ttu-id="a3624-184">Använda hello anpassad fråga tooread data.</span><span class="sxs-lookup"><span data-stu-id="a3624-184">Use hello custom query tooread data.</span></span> |<span data-ttu-id="a3624-185">En SQL-92-fråga eller [Salesforce objektet Query Language (SOQL)](https://developer.salesforce.com/docs/atlas.en-us.soql_sosl.meta/soql_sosl/sforce_api_calls_soql.htm) frågan.</span><span class="sxs-lookup"><span data-stu-id="a3624-185">A SQL-92 query or [Salesforce Object Query Language (SOQL)](https://developer.salesforce.com/docs/atlas.en-us.soql_sosl.meta/soql_sosl/sforce_api_calls_soql.htm) query.</span></span> <span data-ttu-id="a3624-186">Till exempel: `select * from MyTable__c`.</span><span class="sxs-lookup"><span data-stu-id="a3624-186">For example:  `select * from MyTable__c`.</span></span> |<span data-ttu-id="a3624-187">Nej (om hello **tableName** av hello **dataset** har angetts)</span><span class="sxs-lookup"><span data-stu-id="a3624-187">No (if hello **tableName** of hello **dataset** is specified)</span></span> |

> [!IMPORTANT]
> <span data-ttu-id="a3624-188">Hej ”__c” tillhör hello API namn krävs för alla anpassade objekt.</span><span class="sxs-lookup"><span data-stu-id="a3624-188">hello "__c" part of hello API Name is needed for any custom object.</span></span>

![Namnet på data Factory - anslutning Salesforce - API](media/data-factory-salesforce-connector/data-factory-salesforce-api-name-2.png)

## <a name="query-tips"></a><span data-ttu-id="a3624-190">Frågan tips</span><span class="sxs-lookup"><span data-stu-id="a3624-190">Query tips</span></span>
### <a name="retrieving-data-using-where-clause-on-datetime-column"></a><span data-ttu-id="a3624-191">Hämtning av data med hjälp av where-sats för kolumnen datum/tid</span><span class="sxs-lookup"><span data-stu-id="a3624-191">Retrieving data using where clause on DateTime column</span></span>
<span data-ttu-id="a3624-192">Ange när hello SOQL eller SQL fråga, pay kontrolleras toohello skillnaden för DateTime-format.</span><span class="sxs-lookup"><span data-stu-id="a3624-192">When specify hello SOQL or SQL query, pay attention toohello DateTime format difference.</span></span> <span data-ttu-id="a3624-193">Exempel:</span><span class="sxs-lookup"><span data-stu-id="a3624-193">For example:</span></span>

* <span data-ttu-id="a3624-194">**SOQL exempel**:`$$Text.Format('SELECT Id, Name, BillingCity FROM Account WHERE LastModifiedDate >= {0:yyyy-MM-ddTHH:mm:ssZ} AND LastModifiedDate < {1:yyyy-MM-ddTHH:mm:ssZ}', WindowStart, WindowEnd)`</span><span class="sxs-lookup"><span data-stu-id="a3624-194">**SOQL sample**: `$$Text.Format('SELECT Id, Name, BillingCity FROM Account WHERE LastModifiedDate >= {0:yyyy-MM-ddTHH:mm:ssZ} AND LastModifiedDate < {1:yyyy-MM-ddTHH:mm:ssZ}', WindowStart, WindowEnd)`</span></span>
* <span data-ttu-id="a3624-195">**SQL-exempel**:</span><span class="sxs-lookup"><span data-stu-id="a3624-195">**SQL sample**:</span></span>
    * <span data-ttu-id="a3624-196">**Med hjälp av kopiera guiden toospecify hello fråga:**`$$Text.Format('SELECT * FROM Account WHERE LastModifiedDate >= {{ts\'{0:yyyy-MM-dd HH:mm:ss}\'}} AND LastModifiedDate < {{ts\'{1:yyyy-MM-dd HH:mm:ss}\'}}', WindowStart, WindowEnd)`</span><span class="sxs-lookup"><span data-stu-id="a3624-196">**Using copy wizard toospecify hello query:** `$$Text.Format('SELECT * FROM Account WHERE LastModifiedDate >= {{ts\'{0:yyyy-MM-dd HH:mm:ss}\'}} AND LastModifiedDate < {{ts\'{1:yyyy-MM-dd HH:mm:ss}\'}}', WindowStart, WindowEnd)`</span></span>
    * <span data-ttu-id="a3624-197">**Med JSON redigerar toospecify hello fråga (escape-tecken korrekt):**`$$Text.Format('SELECT * FROM Account WHERE LastModifiedDate >= {{ts\\'{0:yyyy-MM-dd HH:mm:ss}\\'}} AND LastModifiedDate < {{ts\\'{1:yyyy-MM-dd HH:mm:ss}\\'}}', WindowStart, WindowEnd)`</span><span class="sxs-lookup"><span data-stu-id="a3624-197">**Using JSON editing toospecify hello query (escape char properly):** `$$Text.Format('SELECT * FROM Account WHERE LastModifiedDate >= {{ts\\'{0:yyyy-MM-dd HH:mm:ss}\\'}} AND LastModifiedDate < {{ts\\'{1:yyyy-MM-dd HH:mm:ss}\\'}}', WindowStart, WindowEnd)`</span></span>

### <a name="retrieving-data-from-salesforce-report"></a><span data-ttu-id="a3624-198">Hämtar data från Salesforce-rapport</span><span class="sxs-lookup"><span data-stu-id="a3624-198">Retrieving data from Salesforce Report</span></span>
<span data-ttu-id="a3624-199">Du kan hämta data från Salesforce-rapporter genom att ange frågan som `{call "<report name>"}`, till exempel.</span><span class="sxs-lookup"><span data-stu-id="a3624-199">You can retrieve data from Salesforce reports by specifying query as `{call "<report name>"}`,for example,.</span></span> <span data-ttu-id="a3624-200">`"query": "{call \"TestReport\"}"`.</span><span class="sxs-lookup"><span data-stu-id="a3624-200">`"query": "{call \"TestReport\"}"`.</span></span>

### <a name="retrieving-deleted-records-from-salesforce-recycle-bin"></a><span data-ttu-id="a3624-201">Hämta borttagna poster från Salesforce-Papperskorgen</span><span class="sxs-lookup"><span data-stu-id="a3624-201">Retrieving deleted records from Salesforce Recycle Bin</span></span>
<span data-ttu-id="a3624-202">tooquery hello ej permanent borttagna poster från Salesforce-Papperskorgen, kan du ange **”IsDeleted = 1”** i frågan.</span><span class="sxs-lookup"><span data-stu-id="a3624-202">tooquery hello soft deleted records from Salesforce Recycle Bin, you can specify **"IsDeleted = 1"** in your query.</span></span> <span data-ttu-id="a3624-203">Exempel:</span><span class="sxs-lookup"><span data-stu-id="a3624-203">For example,</span></span>

* <span data-ttu-id="a3624-204">Ange tooquery endast hello bort poster ”Välj * från MyTable__c **där IsDeleted = 1**”</span><span class="sxs-lookup"><span data-stu-id="a3624-204">tooquery only hello deleted records, specify "select * from MyTable__c **where IsDeleted= 1**"</span></span>
* <span data-ttu-id="a3624-205">tooquery alla hello poster som hello befintliga och hello bort, ange ”Välj * från MyTable__c **där IsDeleted = 0 eller IsDeleted = 1**”</span><span class="sxs-lookup"><span data-stu-id="a3624-205">tooquery all hello records including hello existing and hello deleted, specify "select * from MyTable__c **where IsDeleted = 0 or IsDeleted = 1**"</span></span>

## <a name="json-example-copy-data-from-salesforce-tooazure-blob"></a><span data-ttu-id="a3624-206">JSON-exempel: kopiera data från Salesforce tooAzure Blob</span><span class="sxs-lookup"><span data-stu-id="a3624-206">JSON example: Copy data from Salesforce tooAzure Blob</span></span>
<span data-ttu-id="a3624-207">hello följande exempel innehåller exempel JSON definitioner som du kan använda toocreate en pipeline med hjälp av hello [Azure-portalen](data-factory-copy-activity-tutorial-using-azure-portal.md), [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md), eller [Azure PowerShell](data-factory-copy-activity-tutorial-using-powershell.md).</span><span class="sxs-lookup"><span data-stu-id="a3624-207">hello following example provides sample JSON definitions that you can use toocreate a pipeline by using hello [Azure portal](data-factory-copy-activity-tutorial-using-azure-portal.md), [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md), or [Azure PowerShell](data-factory-copy-activity-tutorial-using-powershell.md).</span></span> <span data-ttu-id="a3624-208">De visar hur toocopy data från Salesforce tooAzure Blob Storage.</span><span class="sxs-lookup"><span data-stu-id="a3624-208">They show how toocopy data from Salesforce tooAzure Blob Storage.</span></span> <span data-ttu-id="a3624-209">Data kan dock vara kopierade tooany av hello sänkor anges [här](data-factory-data-movement-activities.md#supported-data-stores-and-formats) med hello Kopieringsaktiviteten i Azure Data Factory.</span><span class="sxs-lookup"><span data-stu-id="a3624-209">However, data can be copied tooany of hello sinks stated [here](data-factory-data-movement-activities.md#supported-data-stores-and-formats) using hello Copy Activity in Azure Data Factory.</span></span>   

<span data-ttu-id="a3624-210">Här följer hello Data Factory-artefakter behöver toocreate tooimplement hello scenario.</span><span class="sxs-lookup"><span data-stu-id="a3624-210">Here are hello Data Factory artifacts that you'll need toocreate tooimplement hello scenario.</span></span> <span data-ttu-id="a3624-211">hello avsnitten som följer hello lista innehåller information om de här stegen.</span><span class="sxs-lookup"><span data-stu-id="a3624-211">hello sections that follow hello list provide details about these steps.</span></span>

* <span data-ttu-id="a3624-212">En länkad tjänst av typen hello [Salesforce](#linked-service-properties)</span><span class="sxs-lookup"><span data-stu-id="a3624-212">A linked service of hello type [Salesforce](#linked-service-properties)</span></span>
* <span data-ttu-id="a3624-213">En länkad tjänst av typen hello [AzureStorage](data-factory-azure-blob-connector.md#linked-service-properties)</span><span class="sxs-lookup"><span data-stu-id="a3624-213">A linked service of hello type [AzureStorage](data-factory-azure-blob-connector.md#linked-service-properties)</span></span>
* <span data-ttu-id="a3624-214">Indata [dataset](data-factory-create-datasets.md) av hello typen [RelationalTable](#dataset-properties)</span><span class="sxs-lookup"><span data-stu-id="a3624-214">An input [dataset](data-factory-create-datasets.md) of hello type [RelationalTable](#dataset-properties)</span></span>
* <span data-ttu-id="a3624-215">Utdata [dataset](data-factory-create-datasets.md) av hello typen [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties)</span><span class="sxs-lookup"><span data-stu-id="a3624-215">An output [dataset](data-factory-create-datasets.md) of hello type [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties)</span></span>
* <span data-ttu-id="a3624-216">En [pipeline](data-factory-create-pipelines.md) med Kopieringsaktiviteten som använder [RelationalSource](#copy-activity-properties) och [BlobSink](data-factory-azure-blob-connector.md#copy-activity-properties)</span><span class="sxs-lookup"><span data-stu-id="a3624-216">A [pipeline](data-factory-create-pipelines.md) with Copy Activity that uses [RelationalSource](#copy-activity-properties) and [BlobSink](data-factory-azure-blob-connector.md#copy-activity-properties)</span></span>

<span data-ttu-id="a3624-217">**Salesforce länkad tjänst**</span><span class="sxs-lookup"><span data-stu-id="a3624-217">**Salesforce linked service**</span></span>

<span data-ttu-id="a3624-218">Det här exemplet används hello **Salesforce** länkade tjänsten.</span><span class="sxs-lookup"><span data-stu-id="a3624-218">This example uses hello **Salesforce** linked service.</span></span> <span data-ttu-id="a3624-219">Se hello [Salesforce länkade tjänsten](#linked-service-properties) avsnittet för hello egenskaper som stöds av den här länkade tjänsten.</span><span class="sxs-lookup"><span data-stu-id="a3624-219">See hello [Salesforce linked service](#linked-service-properties) section for hello properties that are supported by this linked service.</span></span>  <span data-ttu-id="a3624-220">Se [hämta säkerhetstoken](https://help.salesforce.com/apex/HTViewHelpDoc?id=user_security_token.htm) anvisningar för hur tooreset/hämta hello säkerhetstoken.</span><span class="sxs-lookup"><span data-stu-id="a3624-220">See [Get security token](https://help.salesforce.com/apex/HTViewHelpDoc?id=user_security_token.htm) for instructions on how tooreset/get hello security token.</span></span>

```json
{
    "name": "SalesforceLinkedService",
    "properties":
    {
        "type": "Salesforce",
        "typeProperties":
        {
            "username": "<user name>",
            "password": "<password>",
            "securityToken": "<security token>"
        }
    }
}
```
<span data-ttu-id="a3624-221">**Länkad Azure Storage-tjänst**</span><span class="sxs-lookup"><span data-stu-id="a3624-221">**Azure Storage linked service**</span></span>

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
<span data-ttu-id="a3624-222">**Inkommande Salesforce-dataset**</span><span class="sxs-lookup"><span data-stu-id="a3624-222">**Salesforce input dataset**</span></span>

```json
{
    "name": "SalesforceInput",
    "properties": {
        "linkedServiceName": "SalesforceLinkedService",
        "type": "RelationalTable",
        "typeProperties": {
            "tableName": "AllDataType__c"  
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

<span data-ttu-id="a3624-223">Ange **externa** för**SANT** informerar hello Data Factory-tjänsten som hello dataset är externa toohello data factory och inte tillverkas av en aktivitet i hello data factory.</span><span class="sxs-lookup"><span data-stu-id="a3624-223">Setting **external** too**true** informs hello Data Factory service that hello dataset is external toohello data factory and is not produced by an activity in hello data factory.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="a3624-224">Hej ”__c” tillhör hello API namn krävs för alla anpassade objekt.</span><span class="sxs-lookup"><span data-stu-id="a3624-224">hello "__c" part of hello API Name is needed for any custom object.</span></span>

![Namnet på data Factory - anslutning Salesforce - API](media/data-factory-salesforce-connector/data-factory-salesforce-api-name.png)

<span data-ttu-id="a3624-226">**Utdatauppsättning för Azure-blobb**</span><span class="sxs-lookup"><span data-stu-id="a3624-226">**Azure blob output dataset**</span></span>

<span data-ttu-id="a3624-227">Data skrivs tooa nya blob varje timme (frekvens: timme, intervall: 1).</span><span class="sxs-lookup"><span data-stu-id="a3624-227">Data is written tooa new blob every hour (frequency: hour, interval: 1).</span></span>

```json
{
    "name": "AzureBlobOutput",
    "properties":
    {
        "type": "AzureBlob",
        "linkedServiceName": "AzureStorageLinkedService",
        "typeProperties":
        {
            "folderPath": "adfgetstarted/alltypes_c"
        },
        "availability":
        {
            "frequency": "Hour",
            "interval": 1
        }
    }
}
```

<span data-ttu-id="a3624-228">**Rörledningen med Kopieringsaktiviteten**</span><span class="sxs-lookup"><span data-stu-id="a3624-228">**Pipeline with Copy Activity**</span></span>

<span data-ttu-id="a3624-229">hello pipeline innehåller Kopieringsaktiviteten som är konfigurerad för toouse hello inkommande och utgående datauppsättningar och är schemalagda toorun varje timme.</span><span class="sxs-lookup"><span data-stu-id="a3624-229">hello pipeline contains Copy Activity, which is configured toouse hello input and output datasets, and is scheduled toorun every hour.</span></span> <span data-ttu-id="a3624-230">I hello pipeline JSON-definitionen hello **källa** typ har angetts för**RelationalSource**, och hello **sink** typ har angetts för**BlobSink**.</span><span class="sxs-lookup"><span data-stu-id="a3624-230">In hello pipeline JSON definition, hello **source** type is set too**RelationalSource**, and hello **sink** type is set too**BlobSink**.</span></span>

<span data-ttu-id="a3624-231">Se [RelationalSource Typegenskaper](#copy-activity-properties) hello lista över egenskaper som stöds av hello RelationalSource.</span><span class="sxs-lookup"><span data-stu-id="a3624-231">See [RelationalSource type properties](#copy-activity-properties) for hello list of properties that are supported by hello RelationalSource.</span></span>

```json
{  
    "name":"SamplePipeline",
    "properties":{  
        "start":"2016-06-01T18:00:00",
        "end":"2016-06-01T19:00:00",
        "description":"pipeline with copy activity",
        "activities":[  
        {
            "name": "SalesforceToAzureBlob",
            "description": "Copy from Salesforce tooan Azure blob",
            "type": "Copy",
            "inputs": [
            {
                "name": "SalesforceInput"
            }
            ],
            "outputs": [
            {
                "name": "AzureBlobOutput"
            }
            ],
            "typeProperties": {
                "source": {
                    "type": "RelationalSource",
                    "query": "SELECT Id, Col_AutoNumber__c, Col_Checkbox__c, Col_Currency__c, Col_Date__c, Col_DateTime__c, Col_Email__c, Col_Number__c, Col_Percent__c, Col_Phone__c, Col_Picklist__c, Col_Picklist_MultiSelect__c, Col_Text__c, Col_Text_Area__c, Col_Text_AreaLong__c, Col_Text_AreaRich__c, Col_URL__c, Col_Text_Encrypt__c, Col_Lookup__c FROM AllDataType__c"                
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
> [!IMPORTANT]
> <span data-ttu-id="a3624-232">Hej ”__c” tillhör hello API namn krävs för alla anpassade objekt.</span><span class="sxs-lookup"><span data-stu-id="a3624-232">hello "__c" part of hello API Name is needed for any custom object.</span></span>

![Namnet på data Factory - anslutning Salesforce - API](media/data-factory-salesforce-connector/data-factory-salesforce-api-name-2.png)


### <a name="type-mapping-for-salesforce"></a><span data-ttu-id="a3624-234">Mappning för Salesforce</span><span class="sxs-lookup"><span data-stu-id="a3624-234">Type mapping for Salesforce</span></span>
| <span data-ttu-id="a3624-235">Salesforce-typ</span><span class="sxs-lookup"><span data-stu-id="a3624-235">Salesforce type</span></span> | <span data-ttu-id="a3624-236">. NET-baserade typ</span><span class="sxs-lookup"><span data-stu-id="a3624-236">.NET-based type</span></span> |
| --- | --- |
| <span data-ttu-id="a3624-237">Automatisk tal</span><span class="sxs-lookup"><span data-stu-id="a3624-237">Auto Number</span></span> |<span data-ttu-id="a3624-238">Sträng</span><span class="sxs-lookup"><span data-stu-id="a3624-238">String</span></span> |
| <span data-ttu-id="a3624-239">Kryssruta</span><span class="sxs-lookup"><span data-stu-id="a3624-239">Checkbox</span></span> |<span data-ttu-id="a3624-240">Booleskt värde</span><span class="sxs-lookup"><span data-stu-id="a3624-240">Boolean</span></span> |
| <span data-ttu-id="a3624-241">Valuta</span><span class="sxs-lookup"><span data-stu-id="a3624-241">Currency</span></span> |<span data-ttu-id="a3624-242">dubbla</span><span class="sxs-lookup"><span data-stu-id="a3624-242">Double</span></span> |
| <span data-ttu-id="a3624-243">Date</span><span class="sxs-lookup"><span data-stu-id="a3624-243">Date</span></span> |<span data-ttu-id="a3624-244">Datum och tid</span><span class="sxs-lookup"><span data-stu-id="a3624-244">DateTime</span></span> |
| <span data-ttu-id="a3624-245">Datum/tid</span><span class="sxs-lookup"><span data-stu-id="a3624-245">Date/Time</span></span> |<span data-ttu-id="a3624-246">Datum och tid</span><span class="sxs-lookup"><span data-stu-id="a3624-246">DateTime</span></span> |
| <span data-ttu-id="a3624-247">E-post</span><span class="sxs-lookup"><span data-stu-id="a3624-247">Email</span></span> |<span data-ttu-id="a3624-248">Sträng</span><span class="sxs-lookup"><span data-stu-id="a3624-248">String</span></span> |
| <span data-ttu-id="a3624-249">Id</span><span class="sxs-lookup"><span data-stu-id="a3624-249">Id</span></span> |<span data-ttu-id="a3624-250">Sträng</span><span class="sxs-lookup"><span data-stu-id="a3624-250">String</span></span> |
| <span data-ttu-id="a3624-251">Uppslagsrelation</span><span class="sxs-lookup"><span data-stu-id="a3624-251">Lookup Relationship</span></span> |<span data-ttu-id="a3624-252">Sträng</span><span class="sxs-lookup"><span data-stu-id="a3624-252">String</span></span> |
| <span data-ttu-id="a3624-253">Flerval listruta</span><span class="sxs-lookup"><span data-stu-id="a3624-253">Multi-Select Picklist</span></span> |<span data-ttu-id="a3624-254">Sträng</span><span class="sxs-lookup"><span data-stu-id="a3624-254">String</span></span> |
| <span data-ttu-id="a3624-255">Tal</span><span class="sxs-lookup"><span data-stu-id="a3624-255">Number</span></span> |<span data-ttu-id="a3624-256">dubbla</span><span class="sxs-lookup"><span data-stu-id="a3624-256">Double</span></span> |
| <span data-ttu-id="a3624-257">Procent</span><span class="sxs-lookup"><span data-stu-id="a3624-257">Percent</span></span> |<span data-ttu-id="a3624-258">dubbla</span><span class="sxs-lookup"><span data-stu-id="a3624-258">Double</span></span> |
| <span data-ttu-id="a3624-259">Telefon</span><span class="sxs-lookup"><span data-stu-id="a3624-259">Phone</span></span> |<span data-ttu-id="a3624-260">Sträng</span><span class="sxs-lookup"><span data-stu-id="a3624-260">String</span></span> |
| <span data-ttu-id="a3624-261">Listruta</span><span class="sxs-lookup"><span data-stu-id="a3624-261">Picklist</span></span> |<span data-ttu-id="a3624-262">Sträng</span><span class="sxs-lookup"><span data-stu-id="a3624-262">String</span></span> |
| <span data-ttu-id="a3624-263">Text</span><span class="sxs-lookup"><span data-stu-id="a3624-263">Text</span></span> |<span data-ttu-id="a3624-264">Sträng</span><span class="sxs-lookup"><span data-stu-id="a3624-264">String</span></span> |
| <span data-ttu-id="a3624-265">Textområde</span><span class="sxs-lookup"><span data-stu-id="a3624-265">Text Area</span></span> |<span data-ttu-id="a3624-266">Sträng</span><span class="sxs-lookup"><span data-stu-id="a3624-266">String</span></span> |
| <span data-ttu-id="a3624-267">Textområde (Long)</span><span class="sxs-lookup"><span data-stu-id="a3624-267">Text Area (Long)</span></span> |<span data-ttu-id="a3624-268">Sträng</span><span class="sxs-lookup"><span data-stu-id="a3624-268">String</span></span> |
| <span data-ttu-id="a3624-269">Textområde (omfattande)</span><span class="sxs-lookup"><span data-stu-id="a3624-269">Text Area (Rich)</span></span> |<span data-ttu-id="a3624-270">Sträng</span><span class="sxs-lookup"><span data-stu-id="a3624-270">String</span></span> |
| <span data-ttu-id="a3624-271">Text (krypterade)</span><span class="sxs-lookup"><span data-stu-id="a3624-271">Text (Encrypted)</span></span> |<span data-ttu-id="a3624-272">Sträng</span><span class="sxs-lookup"><span data-stu-id="a3624-272">String</span></span> |
| <span data-ttu-id="a3624-273">URL: EN</span><span class="sxs-lookup"><span data-stu-id="a3624-273">URL</span></span> |<span data-ttu-id="a3624-274">Sträng</span><span class="sxs-lookup"><span data-stu-id="a3624-274">String</span></span> |

> [!NOTE]
> <span data-ttu-id="a3624-275">toomap kolumner från källan dataset toocolumns från sink dataset finns [mappa dataset kolumner i Azure Data Factory](data-factory-map-columns.md).</span><span class="sxs-lookup"><span data-stu-id="a3624-275">toomap columns from source dataset toocolumns from sink dataset, see [Mapping dataset columns in Azure Data Factory](data-factory-map-columns.md).</span></span>

[!INCLUDE [data-factory-structure-for-rectangualr-datasets](../../includes/data-factory-structure-for-rectangualr-datasets.md)]

## <a name="performance-and-tuning"></a><span data-ttu-id="a3624-276">Prestanda- och justering</span><span class="sxs-lookup"><span data-stu-id="a3624-276">Performance and tuning</span></span>
<span data-ttu-id="a3624-277">Se hello [prestandajustering guide och Kopieringsaktivitet prestanda](data-factory-copy-activity-performance.md) toolearn om nyckeln faktorer som påverkan prestanda för flytt av data (Kopieringsaktiviteten) i Azure Data Factory och olika sätt toooptimize den.</span><span class="sxs-lookup"><span data-stu-id="a3624-277">See hello [Copy Activity performance and tuning guide](data-factory-copy-activity-performance.md) toolearn about key factors that impact performance of data movement (Copy Activity) in Azure Data Factory and various ways toooptimize it.</span></span>
