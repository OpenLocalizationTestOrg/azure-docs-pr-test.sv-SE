---
title: "Flytta data från Salesforce med hjälp av Data Factory | Microsoft Docs"
description: "Läs mer om hur du flyttar data från Salesforce med hjälp av Azure Data Factory."
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
ms.openlocfilehash: 9390b992bce2dede750c3fc55b7783a6b0db678f
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="move-data-from-salesforce-by-using-azure-data-factory"></a><span data-ttu-id="c4439-103">Flytta data från Salesforce med hjälp av Azure Data Factory</span><span class="sxs-lookup"><span data-stu-id="c4439-103">Move data from Salesforce by using Azure Data Factory</span></span>
<span data-ttu-id="c4439-104">Den här artikeln beskrivs hur du kan använda Kopieringsaktiviteten i ett Azure data factory för att kopiera data från Salesforce till alla datalager som anges under kolumnen mottagare i den [källor och sänkor stöds](data-factory-data-movement-activities.md#supported-data-stores-and-formats) tabell.</span><span class="sxs-lookup"><span data-stu-id="c4439-104">This article outlines how you can use Copy Activity in an Azure data factory to copy data from Salesforce to any data store that is listed under the Sink column in the [supported sources and sinks](data-factory-data-movement-activities.md#supported-data-stores-and-formats) table.</span></span> <span data-ttu-id="c4439-105">Den här artikeln bygger på den [data movement aktiviteter](data-factory-data-movement-activities.md) artikel som ger en allmän översikt över dataflyttning stöds data store kombinationer och Kopieringsaktivitet.</span><span class="sxs-lookup"><span data-stu-id="c4439-105">This article builds on the [data movement activities](data-factory-data-movement-activities.md) article, which presents a general overview of data movement with Copy Activity and supported data store combinations.</span></span>

<span data-ttu-id="c4439-106">Azure Data Factory stöder för närvarande endast flytta data från Salesforce till [stöds sink datalager](data-factory-data-movement-activities.md#supported-data-stores-and-formats), men inte stöder att flytta data från andra data lagras till Salesforce.</span><span class="sxs-lookup"><span data-stu-id="c4439-106">Azure Data Factory currently supports only moving data from Salesforce to [supported sink data stores](data-factory-data-movement-activities.md#supported-data-stores-and-formats), but does not support moving data from other data stores to Salesforce.</span></span>

## <a name="supported-versions"></a><span data-ttu-id="c4439-107">Versioner som stöds</span><span class="sxs-lookup"><span data-stu-id="c4439-107">Supported versions</span></span>
<span data-ttu-id="c4439-108">Den här anslutningen har stöd för följande versioner av Salesforce: Developer Edition, Professional Edition, Enterprise Edition eller obegränsade Edition.</span><span class="sxs-lookup"><span data-stu-id="c4439-108">This connector supports the following editions of Salesforce: Developer Edition, Professional Edition, Enterprise Edition, or Unlimited Edition.</span></span> <span data-ttu-id="c4439-109">Och kopiera från Salesforce produktion, sandbox och anpassade domäner.</span><span class="sxs-lookup"><span data-stu-id="c4439-109">And it supports copying from Salesforce production, sandbox and custom domain.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="c4439-110">Krav</span><span class="sxs-lookup"><span data-stu-id="c4439-110">Prerequisites</span></span>
* <span data-ttu-id="c4439-111">API-behörighet måste aktiveras.</span><span class="sxs-lookup"><span data-stu-id="c4439-111">API permission must be enabled.</span></span> <span data-ttu-id="c4439-112">Se [hur aktivera API-åtkomst i Salesforce av behörighetsgrupp?](https://www.data2crm.com/migration/faqs/enable-api-access-salesforce-permission-set/)</span><span class="sxs-lookup"><span data-stu-id="c4439-112">See [How do I enable API access in Salesforce by permission set?](https://www.data2crm.com/migration/faqs/enable-api-access-salesforce-permission-set/)</span></span>
* <span data-ttu-id="c4439-113">Om du vill kopiera data från Salesforce till lokala datalager, du har Data Management Gateway 2.0 är installerat i din lokala miljö.</span><span class="sxs-lookup"><span data-stu-id="c4439-113">To copy data from Salesforce to on-premises data stores, you must have at least Data Management Gateway 2.0 installed in your on-premises environment.</span></span>

## <a name="salesforce-request-limits"></a><span data-ttu-id="c4439-114">Salesforce begärandebegränsningar</span><span class="sxs-lookup"><span data-stu-id="c4439-114">Salesforce request limits</span></span>
<span data-ttu-id="c4439-115">Salesforce har begränsningar för både förfrågningarna API och samtidiga API-begäranden.</span><span class="sxs-lookup"><span data-stu-id="c4439-115">Salesforce has limits for both total API requests and concurrent API requests.</span></span> <span data-ttu-id="c4439-116">Observera följande punkter:</span><span class="sxs-lookup"><span data-stu-id="c4439-116">Note the following points:</span></span>

- <span data-ttu-id="c4439-117">Om antalet samtidiga begäranden överskrider gränsen, begränsning inträffar och slumpmässiga fel visas.</span><span class="sxs-lookup"><span data-stu-id="c4439-117">If the number of concurrent requests exceeds the limit, throttling occurs and you will see random failures.</span></span>
- <span data-ttu-id="c4439-118">Om det totala antalet begäranden överskrider gränsen, blockeras Salesforce-konto i 24 timmar.</span><span class="sxs-lookup"><span data-stu-id="c4439-118">If the total number of requests exceeds the limit, the Salesforce account will be blocked for 24 hours.</span></span>

<span data-ttu-id="c4439-119">Du kan också få ”REQUEST_LIMIT_EXCEEDED”-fel i båda scenarierna.</span><span class="sxs-lookup"><span data-stu-id="c4439-119">You might also receive the “REQUEST_LIMIT_EXCEEDED“ error in both scenarios.</span></span> <span data-ttu-id="c4439-120">Se avsnittet ”API Begärandebegränsningar” i den [Salesforce Developer gränser](http://resources.docs.salesforce.com/200/20/en-us/sfdc/pdf/salesforce_app_limits_cheatsheet.pdf) artikeln för information.</span><span class="sxs-lookup"><span data-stu-id="c4439-120">See the "API Request Limits" section in the [Salesforce Developer Limits](http://resources.docs.salesforce.com/200/20/en-us/sfdc/pdf/salesforce_app_limits_cheatsheet.pdf) article for details.</span></span>

## <a name="getting-started"></a><span data-ttu-id="c4439-121">Komma igång</span><span class="sxs-lookup"><span data-stu-id="c4439-121">Getting started</span></span>
<span data-ttu-id="c4439-122">Du kan skapa en pipeline med en kopia-aktivitet som flyttar data från Salesforce med hjälp av olika verktyg/API: er.</span><span class="sxs-lookup"><span data-stu-id="c4439-122">You can create a pipeline with a copy activity that moves data from Salesforce by using different tools/APIs.</span></span>

<span data-ttu-id="c4439-123">Det enklaste sättet att skapa en pipeline är att använda den **guiden Kopiera**.</span><span class="sxs-lookup"><span data-stu-id="c4439-123">The easiest way to create a pipeline is to use the **Copy Wizard**.</span></span> <span data-ttu-id="c4439-124">Finns [Självstudier: skapa en pipeline med hjälp av guiden Kopiera](data-factory-copy-data-wizard-tutorial.md) för en snabb genomgång om hur du skapar en pipeline med hjälp av guiden Kopiera data.</span><span class="sxs-lookup"><span data-stu-id="c4439-124">See [Tutorial: Create a pipeline using Copy Wizard](data-factory-copy-data-wizard-tutorial.md) for a quick walkthrough on creating a pipeline using the Copy data wizard.</span></span>

<span data-ttu-id="c4439-125">Du kan också använda följande verktyg för att skapa en pipeline: **Azure-portalen**, **Visual Studio**, **Azure PowerShell**, **Azure Resource Manager-mall**, **.NET API**, och **REST API**.</span><span class="sxs-lookup"><span data-stu-id="c4439-125">You can also use the following tools to create a pipeline: **Azure portal**, **Visual Studio**, **Azure PowerShell**, **Azure Resource Manager template**, **.NET API**, and **REST API**.</span></span> <span data-ttu-id="c4439-126">Se [kopiera aktivitet kursen](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) för stegvisa instruktioner för att skapa en pipeline med en Kopieringsaktivitet.</span><span class="sxs-lookup"><span data-stu-id="c4439-126">See [Copy activity tutorial](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) for step-by-step instructions to create a pipeline with a copy activity.</span></span> 

<span data-ttu-id="c4439-127">Om du använder verktyg eller API: er, kan du utföra följande steg för att skapa en pipeline som flyttar data från ett dataarkiv som källa till ett dataarkiv som mottagare:</span><span class="sxs-lookup"><span data-stu-id="c4439-127">Whether you use the tools or APIs, you perform the following steps to create a pipeline that moves data from a source data store to a sink data store:</span></span> 

1. <span data-ttu-id="c4439-128">Skapa **länkade tjänster** att länka inkommande och utgående data lagras till din data factory.</span><span class="sxs-lookup"><span data-stu-id="c4439-128">Create **linked services** to link input and output data stores to your data factory.</span></span>
2. <span data-ttu-id="c4439-129">Skapa **datauppsättningar** att representera inkommande och utgående data för kopieringen.</span><span class="sxs-lookup"><span data-stu-id="c4439-129">Create **datasets** to represent input and output data for the copy operation.</span></span> 
3. <span data-ttu-id="c4439-130">Skapa en **pipeline** med en kopia-aktivitet som tar en datamängd som indata och en dataset som utdata.</span><span class="sxs-lookup"><span data-stu-id="c4439-130">Create a **pipeline** with a copy activity that takes a dataset as an input and a dataset as an output.</span></span> 

<span data-ttu-id="c4439-131">När du använder guiden skapas JSON definitioner för dessa Data Factory-enheter (länkade tjänster, datauppsättningar och pipelinen) automatiskt för dig.</span><span class="sxs-lookup"><span data-stu-id="c4439-131">When you use the wizard, JSON definitions for these Data Factory entities (linked services, datasets, and the pipeline) are automatically created for you.</span></span> <span data-ttu-id="c4439-132">När du använder Verktyg/API: er (utom .NET API), kan du definiera dessa Data Factory-enheter med hjälp av JSON-format.</span><span class="sxs-lookup"><span data-stu-id="c4439-132">When you use tools/APIs (except .NET API), you define these Data Factory entities by using the JSON format.</span></span>  <span data-ttu-id="c4439-133">Ett exempel med JSON-definitioner för Data Factory-entiteter som används för att kopiera data från Salesforce finns [JSON-exempel: kopiera data från Salesforce till Azure Blob](#json-example-copy-data-from-salesforce-to-azure-blob) i den här artikeln.</span><span class="sxs-lookup"><span data-stu-id="c4439-133">For a sample with JSON definitions for Data Factory entities that are used to copy data from Salesforce, see [JSON example: Copy data from Salesforce to Azure Blob](#json-example-copy-data-from-salesforce-to-azure-blob) section of this article.</span></span> 

<span data-ttu-id="c4439-134">Följande avsnitt innehåller information om JSON-egenskaper som används för att definiera Data Factory entiteter till Salesforce:</span><span class="sxs-lookup"><span data-stu-id="c4439-134">The following sections provide details about JSON properties that are used to define Data Factory entities specific to Salesforce:</span></span> 

## <a name="linked-service-properties"></a><span data-ttu-id="c4439-135">Länkad tjänstegenskaper</span><span class="sxs-lookup"><span data-stu-id="c4439-135">Linked service properties</span></span>
<span data-ttu-id="c4439-136">Följande tabell innehåller beskrivningar för JSON-element som är specifika för Salesforce länkade tjänsten.</span><span class="sxs-lookup"><span data-stu-id="c4439-136">The following table provides descriptions for JSON elements that are specific to the Salesforce linked service.</span></span>

| <span data-ttu-id="c4439-137">Egenskap</span><span class="sxs-lookup"><span data-stu-id="c4439-137">Property</span></span> | <span data-ttu-id="c4439-138">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="c4439-138">Description</span></span> | <span data-ttu-id="c4439-139">Krävs</span><span class="sxs-lookup"><span data-stu-id="c4439-139">Required</span></span> |
| --- | --- | --- |
| <span data-ttu-id="c4439-140">typ</span><span class="sxs-lookup"><span data-stu-id="c4439-140">type</span></span> |<span data-ttu-id="c4439-141">Egenskapen type måste anges till: **Salesforce**.</span><span class="sxs-lookup"><span data-stu-id="c4439-141">The type property must be set to: **Salesforce**.</span></span> |<span data-ttu-id="c4439-142">Ja</span><span class="sxs-lookup"><span data-stu-id="c4439-142">Yes</span></span> |
| <span data-ttu-id="c4439-143">environmentUrl</span><span class="sxs-lookup"><span data-stu-id="c4439-143">environmentUrl</span></span> | <span data-ttu-id="c4439-144">Ange URL: en för Salesforce-instans.</span><span class="sxs-lookup"><span data-stu-id="c4439-144">Specify the URL of Salesforce instance.</span></span> <br><br> <span data-ttu-id="c4439-145">– Standardvärdet är ”https://login.salesforce.com”.</span><span class="sxs-lookup"><span data-stu-id="c4439-145">- Default is "https://login.salesforce.com".</span></span> <br> <span data-ttu-id="c4439-146">-Ange ”https://test.salesforce.com” om du vill kopiera data från sandbox.</span><span class="sxs-lookup"><span data-stu-id="c4439-146">- To copy data from sandbox, specify "https://test.salesforce.com".</span></span> <br> <span data-ttu-id="c4439-147">-Om du vill kopiera data från domänen, ange, till exempel ”https://[domain].my.salesforce.com”.</span><span class="sxs-lookup"><span data-stu-id="c4439-147">- To copy data from custom domain, specify, for example, "https://[domain].my.salesforce.com".</span></span> |<span data-ttu-id="c4439-148">Nej</span><span class="sxs-lookup"><span data-stu-id="c4439-148">No</span></span> |
| <span data-ttu-id="c4439-149">användarnamn</span><span class="sxs-lookup"><span data-stu-id="c4439-149">username</span></span> |<span data-ttu-id="c4439-150">Ange ett användarnamn för användarkontot.</span><span class="sxs-lookup"><span data-stu-id="c4439-150">Specify a user name for the user account.</span></span> |<span data-ttu-id="c4439-151">Ja</span><span class="sxs-lookup"><span data-stu-id="c4439-151">Yes</span></span> |
| <span data-ttu-id="c4439-152">lösenord</span><span class="sxs-lookup"><span data-stu-id="c4439-152">password</span></span> |<span data-ttu-id="c4439-153">Ange ett lösenord för användarkontot.</span><span class="sxs-lookup"><span data-stu-id="c4439-153">Specify a password for the user account.</span></span> |<span data-ttu-id="c4439-154">Ja</span><span class="sxs-lookup"><span data-stu-id="c4439-154">Yes</span></span> |
| <span data-ttu-id="c4439-155">securityToken</span><span class="sxs-lookup"><span data-stu-id="c4439-155">securityToken</span></span> |<span data-ttu-id="c4439-156">Ange en säkerhetstoken för användarkontot.</span><span class="sxs-lookup"><span data-stu-id="c4439-156">Specify a security token for the user account.</span></span> <span data-ttu-id="c4439-157">Se [hämta säkerhetstoken](https://help.salesforce.com/apex/HTViewHelpDoc?id=user_security_token.htm) för instruktioner om hur du återställa/hämta en säkerhetstoken.</span><span class="sxs-lookup"><span data-stu-id="c4439-157">See [Get security token](https://help.salesforce.com/apex/HTViewHelpDoc?id=user_security_token.htm) for instructions on how to reset/get a security token.</span></span> <span data-ttu-id="c4439-158">Läs om säkerhetstoken i allmänhet i [säkerhets- och API: et](https://developer.salesforce.com/docs/atlas.en-us.api.meta/api/sforce_api_concepts_security.htm).</span><span class="sxs-lookup"><span data-stu-id="c4439-158">To learn about security tokens in general, see [Security and the API](https://developer.salesforce.com/docs/atlas.en-us.api.meta/api/sforce_api_concepts_security.htm).</span></span> |<span data-ttu-id="c4439-159">Ja</span><span class="sxs-lookup"><span data-stu-id="c4439-159">Yes</span></span> |

## <a name="dataset-properties"></a><span data-ttu-id="c4439-160">Egenskaper för datamängd</span><span class="sxs-lookup"><span data-stu-id="c4439-160">Dataset properties</span></span>
<span data-ttu-id="c4439-161">En fullständig lista över egenskaper som är tillgängliga för att definiera datauppsättningar och avsnitt, finns det [skapa datauppsättningar](data-factory-create-datasets.md) artikel.</span><span class="sxs-lookup"><span data-stu-id="c4439-161">For a full list of sections and properties that are available for defining datasets, see the [Creating datasets](data-factory-create-datasets.md) article.</span></span> <span data-ttu-id="c4439-162">Avsnitt som struktur, tillgänglighet och princip på en datamängd JSON är liknande för alla typer av datauppsättningen (Azure SQL Azure blob, Azure-tabellen och så vidare).</span><span class="sxs-lookup"><span data-stu-id="c4439-162">Sections such as structure, availability, and policy of a dataset JSON are similar for all dataset types (Azure SQL, Azure blob, Azure table, and so on).</span></span>

<span data-ttu-id="c4439-163">Den **typeProperties** avsnitt är olika för varje typ av dataset och innehåller information om placeringen av data i datalagret.</span><span class="sxs-lookup"><span data-stu-id="c4439-163">The **typeProperties** section is different for each type of dataset and provides information about the location of the data in the data store.</span></span> <span data-ttu-id="c4439-164">TypeProperties avsnittet för en dataset av typen **RelationalTable** har följande egenskaper:</span><span class="sxs-lookup"><span data-stu-id="c4439-164">The typeProperties section for a dataset of the type **RelationalTable** has the following properties:</span></span>

| <span data-ttu-id="c4439-165">Egenskap</span><span class="sxs-lookup"><span data-stu-id="c4439-165">Property</span></span> | <span data-ttu-id="c4439-166">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="c4439-166">Description</span></span> | <span data-ttu-id="c4439-167">Krävs</span><span class="sxs-lookup"><span data-stu-id="c4439-167">Required</span></span> |
| --- | --- | --- |
| <span data-ttu-id="c4439-168">tableName</span><span class="sxs-lookup"><span data-stu-id="c4439-168">tableName</span></span> |<span data-ttu-id="c4439-169">Namnet på tabellen i Salesforce.</span><span class="sxs-lookup"><span data-stu-id="c4439-169">Name of the table in Salesforce.</span></span> |<span data-ttu-id="c4439-170">Nej (om en **frågan** av **RelationalSource** har angetts)</span><span class="sxs-lookup"><span data-stu-id="c4439-170">No (if a **query** of **RelationalSource** is specified)</span></span> |

> [!IMPORTANT]
> <span data-ttu-id="c4439-171">”__C”-delen av API-namnet krävs för alla anpassade objekt.</span><span class="sxs-lookup"><span data-stu-id="c4439-171">The "__c" part of the API Name is needed for any custom object.</span></span>

![Namnet på data Factory - anslutning Salesforce - API](media/data-factory-salesforce-connector/data-factory-salesforce-api-name.png)

## <a name="copy-activity-properties"></a><span data-ttu-id="c4439-173">Kopiera egenskaper för aktivitet</span><span class="sxs-lookup"><span data-stu-id="c4439-173">Copy activity properties</span></span>
<span data-ttu-id="c4439-174">En fullständig lista över avsnitt och egenskaper som är tillgängliga för att definiera aktiviteter finns i [skapar pipelines](data-factory-create-pipelines.md) artikel.</span><span class="sxs-lookup"><span data-stu-id="c4439-174">For a full list of sections and properties that are available for defining activities, see the [Creating pipelines](data-factory-create-pipelines.md) article.</span></span> <span data-ttu-id="c4439-175">Egenskaper som namn, beskrivning, indata och utdata tabeller och olika principer är tillgängliga för alla typer av aktiviteter.</span><span class="sxs-lookup"><span data-stu-id="c4439-175">Properties like name, description, input and output tables, and various policies are available for all types of activities.</span></span>

<span data-ttu-id="c4439-176">De egenskaper som är tillgängliga i avsnittet typeProperties aktiviteten å andra sidan varierar beroende på varje aktivitetstyp.</span><span class="sxs-lookup"><span data-stu-id="c4439-176">The properties that are available in the typeProperties section of the activity, on the other hand, vary with each activity type.</span></span> <span data-ttu-id="c4439-177">För Kopieringsaktivitet kan variera de beroende på vilka typer av datakällor och sänkor.</span><span class="sxs-lookup"><span data-stu-id="c4439-177">For Copy Activity, they vary depending on the types of sources and sinks.</span></span>

<span data-ttu-id="c4439-178">I en Kopieringsaktivitet när källan är av typen **RelationalSource** (som omfattar Salesforce), följande egenskaper finns i avsnittet typeProperties:</span><span class="sxs-lookup"><span data-stu-id="c4439-178">In copy activity, when the source is of the type **RelationalSource** (which includes Salesforce), the following properties are available in typeProperties section:</span></span>

| <span data-ttu-id="c4439-179">Egenskap</span><span class="sxs-lookup"><span data-stu-id="c4439-179">Property</span></span> | <span data-ttu-id="c4439-180">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="c4439-180">Description</span></span> | <span data-ttu-id="c4439-181">Tillåtna värden</span><span class="sxs-lookup"><span data-stu-id="c4439-181">Allowed values</span></span> | <span data-ttu-id="c4439-182">Krävs</span><span class="sxs-lookup"><span data-stu-id="c4439-182">Required</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="c4439-183">DocumentDB</span><span class="sxs-lookup"><span data-stu-id="c4439-183">query</span></span> |<span data-ttu-id="c4439-184">Använd anpassad fråga för att läsa data.</span><span class="sxs-lookup"><span data-stu-id="c4439-184">Use the custom query to read data.</span></span> |<span data-ttu-id="c4439-185">En SQL-92-fråga eller [Salesforce objektet Query Language (SOQL)](https://developer.salesforce.com/docs/atlas.en-us.soql_sosl.meta/soql_sosl/sforce_api_calls_soql.htm) frågan.</span><span class="sxs-lookup"><span data-stu-id="c4439-185">A SQL-92 query or [Salesforce Object Query Language (SOQL)](https://developer.salesforce.com/docs/atlas.en-us.soql_sosl.meta/soql_sosl/sforce_api_calls_soql.htm) query.</span></span> <span data-ttu-id="c4439-186">Till exempel: `select * from MyTable__c`.</span><span class="sxs-lookup"><span data-stu-id="c4439-186">For example:  `select * from MyTable__c`.</span></span> |<span data-ttu-id="c4439-187">Nej (om den **tableName** av den **dataset** har angetts)</span><span class="sxs-lookup"><span data-stu-id="c4439-187">No (if the **tableName** of the **dataset** is specified)</span></span> |

> [!IMPORTANT]
> <span data-ttu-id="c4439-188">”__C”-delen av API-namnet krävs för alla anpassade objekt.</span><span class="sxs-lookup"><span data-stu-id="c4439-188">The "__c" part of the API Name is needed for any custom object.</span></span>

![Namnet på data Factory - anslutning Salesforce - API](media/data-factory-salesforce-connector/data-factory-salesforce-api-name-2.png)

## <a name="query-tips"></a><span data-ttu-id="c4439-190">Frågan tips</span><span class="sxs-lookup"><span data-stu-id="c4439-190">Query tips</span></span>
### <a name="retrieving-data-using-where-clause-on-datetime-column"></a><span data-ttu-id="c4439-191">Hämtning av data med hjälp av where-sats för kolumnen datum/tid</span><span class="sxs-lookup"><span data-stu-id="c4439-191">Retrieving data using where clause on DateTime column</span></span>
<span data-ttu-id="c4439-192">När ange SOQL eller SQL-fråga, titta närmare på skillnaden för DateTime-format.</span><span class="sxs-lookup"><span data-stu-id="c4439-192">When specify the SOQL or SQL query, pay attention to the DateTime format difference.</span></span> <span data-ttu-id="c4439-193">Exempel:</span><span class="sxs-lookup"><span data-stu-id="c4439-193">For example:</span></span>

* <span data-ttu-id="c4439-194">**SOQL exempel**:`$$Text.Format('SELECT Id, Name, BillingCity FROM Account WHERE LastModifiedDate >= {0:yyyy-MM-ddTHH:mm:ssZ} AND LastModifiedDate < {1:yyyy-MM-ddTHH:mm:ssZ}', WindowStart, WindowEnd)`</span><span class="sxs-lookup"><span data-stu-id="c4439-194">**SOQL sample**: `$$Text.Format('SELECT Id, Name, BillingCity FROM Account WHERE LastModifiedDate >= {0:yyyy-MM-ddTHH:mm:ssZ} AND LastModifiedDate < {1:yyyy-MM-ddTHH:mm:ssZ}', WindowStart, WindowEnd)`</span></span>
* <span data-ttu-id="c4439-195">**SQL-exempel**:</span><span class="sxs-lookup"><span data-stu-id="c4439-195">**SQL sample**:</span></span>
    * <span data-ttu-id="c4439-196">**Använd guiden Kopiera för att ange frågan:**`$$Text.Format('SELECT * FROM Account WHERE LastModifiedDate >= {{ts\'{0:yyyy-MM-dd HH:mm:ss}\'}} AND LastModifiedDate < {{ts\'{1:yyyy-MM-dd HH:mm:ss}\'}}', WindowStart, WindowEnd)`</span><span class="sxs-lookup"><span data-stu-id="c4439-196">**Using copy wizard to specify the query:** `$$Text.Format('SELECT * FROM Account WHERE LastModifiedDate >= {{ts\'{0:yyyy-MM-dd HH:mm:ss}\'}} AND LastModifiedDate < {{ts\'{1:yyyy-MM-dd HH:mm:ss}\'}}', WindowStart, WindowEnd)`</span></span>
    * <span data-ttu-id="c4439-197">**Med JSON Redigera om du vill ange frågan (escape-tecken korrekt):**`$$Text.Format('SELECT * FROM Account WHERE LastModifiedDate >= {{ts\\'{0:yyyy-MM-dd HH:mm:ss}\\'}} AND LastModifiedDate < {{ts\\'{1:yyyy-MM-dd HH:mm:ss}\\'}}', WindowStart, WindowEnd)`</span><span class="sxs-lookup"><span data-stu-id="c4439-197">**Using JSON editing to specify the query (escape char properly):** `$$Text.Format('SELECT * FROM Account WHERE LastModifiedDate >= {{ts\\'{0:yyyy-MM-dd HH:mm:ss}\\'}} AND LastModifiedDate < {{ts\\'{1:yyyy-MM-dd HH:mm:ss}\\'}}', WindowStart, WindowEnd)`</span></span>

### <a name="retrieving-data-from-salesforce-report"></a><span data-ttu-id="c4439-198">Hämtar data från Salesforce-rapport</span><span class="sxs-lookup"><span data-stu-id="c4439-198">Retrieving data from Salesforce Report</span></span>
<span data-ttu-id="c4439-199">Du kan hämta data från Salesforce-rapporter genom att ange frågan som `{call "<report name>"}`, till exempel.</span><span class="sxs-lookup"><span data-stu-id="c4439-199">You can retrieve data from Salesforce reports by specifying query as `{call "<report name>"}`,for example,.</span></span> <span data-ttu-id="c4439-200">`"query": "{call \"TestReport\"}"`.</span><span class="sxs-lookup"><span data-stu-id="c4439-200">`"query": "{call \"TestReport\"}"`.</span></span>

### <a name="retrieving-deleted-records-from-salesforce-recycle-bin"></a><span data-ttu-id="c4439-201">Hämta borttagna poster från Salesforce-Papperskorgen</span><span class="sxs-lookup"><span data-stu-id="c4439-201">Retrieving deleted records from Salesforce Recycle Bin</span></span>
<span data-ttu-id="c4439-202">Om du vill fråga ej permanent borttagna poster från Salesforce-Papperskorgen, kan du ange **”IsDeleted = 1”** i frågan.</span><span class="sxs-lookup"><span data-stu-id="c4439-202">To query the soft deleted records from Salesforce Recycle Bin, you can specify **"IsDeleted = 1"** in your query.</span></span> <span data-ttu-id="c4439-203">Exempel:</span><span class="sxs-lookup"><span data-stu-id="c4439-203">For example,</span></span>

* <span data-ttu-id="c4439-204">Om du vill fråga endast borttagna poster, ange ”Välj * från MyTable__c **där IsDeleted = 1**”</span><span class="sxs-lookup"><span data-stu-id="c4439-204">To query only the deleted records, specify "select * from MyTable__c **where IsDeleted= 1**"</span></span>
* <span data-ttu-id="c4439-205">Om du vill fråga efter alla poster som inklusive det befintliga och den borttagna, ange ”Välj * från MyTable__c **där IsDeleted = 0 eller IsDeleted = 1**”</span><span class="sxs-lookup"><span data-stu-id="c4439-205">To query all the records including the existing and the deleted, specify "select * from MyTable__c **where IsDeleted = 0 or IsDeleted = 1**"</span></span>

## <a name="json-example-copy-data-from-salesforce-to-azure-blob"></a><span data-ttu-id="c4439-206">JSON-exempel: kopiera data från Salesforce till Azure-Blob</span><span class="sxs-lookup"><span data-stu-id="c4439-206">JSON example: Copy data from Salesforce to Azure Blob</span></span>
<span data-ttu-id="c4439-207">I följande exempel innehåller exempel JSON definitioner som du kan använda för att skapa en pipeline med hjälp av den [Azure-portalen](data-factory-copy-activity-tutorial-using-azure-portal.md), [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md), eller [Azure PowerShell](data-factory-copy-activity-tutorial-using-powershell.md).</span><span class="sxs-lookup"><span data-stu-id="c4439-207">The following example provides sample JSON definitions that you can use to create a pipeline by using the [Azure portal](data-factory-copy-activity-tutorial-using-azure-portal.md), [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md), or [Azure PowerShell](data-factory-copy-activity-tutorial-using-powershell.md).</span></span> <span data-ttu-id="c4439-208">De visar hur du kopierar data från Salesforce till Azure Blob Storage.</span><span class="sxs-lookup"><span data-stu-id="c4439-208">They show how to copy data from Salesforce to Azure Blob Storage.</span></span> <span data-ttu-id="c4439-209">Dock datan kan kopieras till någon av sänkor anges [här](data-factory-data-movement-activities.md#supported-data-stores-and-formats) med hjälp av aktiviteten kopiera i Azure Data Factory.</span><span class="sxs-lookup"><span data-stu-id="c4439-209">However, data can be copied to any of the sinks stated [here](data-factory-data-movement-activities.md#supported-data-stores-and-formats) using the Copy Activity in Azure Data Factory.</span></span>   

<span data-ttu-id="c4439-210">Här är Data Factory-artefakter som du behöver skapa för att implementera scenariot.</span><span class="sxs-lookup"><span data-stu-id="c4439-210">Here are the Data Factory artifacts that you'll need to create to implement the scenario.</span></span> <span data-ttu-id="c4439-211">Avsnitten som följer en lista innehåller information om de här stegen.</span><span class="sxs-lookup"><span data-stu-id="c4439-211">The sections that follow the list provide details about these steps.</span></span>

* <span data-ttu-id="c4439-212">En länkad tjänst av typen [Salesforce](#linked-service-properties)</span><span class="sxs-lookup"><span data-stu-id="c4439-212">A linked service of the type [Salesforce](#linked-service-properties)</span></span>
* <span data-ttu-id="c4439-213">En länkad tjänst av typen [AzureStorage](data-factory-azure-blob-connector.md#linked-service-properties)</span><span class="sxs-lookup"><span data-stu-id="c4439-213">A linked service of the type [AzureStorage](data-factory-azure-blob-connector.md#linked-service-properties)</span></span>
* <span data-ttu-id="c4439-214">Indata [dataset](data-factory-create-datasets.md) av typen [RelationalTable](#dataset-properties)</span><span class="sxs-lookup"><span data-stu-id="c4439-214">An input [dataset](data-factory-create-datasets.md) of the type [RelationalTable](#dataset-properties)</span></span>
* <span data-ttu-id="c4439-215">Utdata [dataset](data-factory-create-datasets.md) av typen [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties)</span><span class="sxs-lookup"><span data-stu-id="c4439-215">An output [dataset](data-factory-create-datasets.md) of the type [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties)</span></span>
* <span data-ttu-id="c4439-216">En [pipeline](data-factory-create-pipelines.md) med Kopieringsaktiviteten som använder [RelationalSource](#copy-activity-properties) och [BlobSink](data-factory-azure-blob-connector.md#copy-activity-properties)</span><span class="sxs-lookup"><span data-stu-id="c4439-216">A [pipeline](data-factory-create-pipelines.md) with Copy Activity that uses [RelationalSource](#copy-activity-properties) and [BlobSink](data-factory-azure-blob-connector.md#copy-activity-properties)</span></span>

<span data-ttu-id="c4439-217">**Salesforce länkad tjänst**</span><span class="sxs-lookup"><span data-stu-id="c4439-217">**Salesforce linked service**</span></span>

<span data-ttu-id="c4439-218">Det här exemplet används den **Salesforce** länkade tjänsten.</span><span class="sxs-lookup"><span data-stu-id="c4439-218">This example uses the **Salesforce** linked service.</span></span> <span data-ttu-id="c4439-219">Finns det [Salesforce länkade tjänsten](#linked-service-properties) avsnittet för egenskaper som stöds av den här länkade tjänsten.</span><span class="sxs-lookup"><span data-stu-id="c4439-219">See the [Salesforce linked service](#linked-service-properties) section for the properties that are supported by this linked service.</span></span>  <span data-ttu-id="c4439-220">Se [hämta säkerhetstoken](https://help.salesforce.com/apex/HTViewHelpDoc?id=user_security_token.htm) för instruktioner om hur du återställa/hämta säkerhetstoken.</span><span class="sxs-lookup"><span data-stu-id="c4439-220">See [Get security token](https://help.salesforce.com/apex/HTViewHelpDoc?id=user_security_token.htm) for instructions on how to reset/get the security token.</span></span>

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
<span data-ttu-id="c4439-221">**Länkad Azure Storage-tjänst**</span><span class="sxs-lookup"><span data-stu-id="c4439-221">**Azure Storage linked service**</span></span>

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
<span data-ttu-id="c4439-222">**Inkommande Salesforce-dataset**</span><span class="sxs-lookup"><span data-stu-id="c4439-222">**Salesforce input dataset**</span></span>

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

<span data-ttu-id="c4439-223">Ange **externa** till **SANT** informerar Data Factory-tjänsten att datamängden är extern till data factory och inte tillverkas av en aktivitet i datafabriken.</span><span class="sxs-lookup"><span data-stu-id="c4439-223">Setting **external** to **true** informs the Data Factory service that the dataset is external to the data factory and is not produced by an activity in the data factory.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="c4439-224">”__C”-delen av API-namnet krävs för alla anpassade objekt.</span><span class="sxs-lookup"><span data-stu-id="c4439-224">The "__c" part of the API Name is needed for any custom object.</span></span>

![Namnet på data Factory - anslutning Salesforce - API](media/data-factory-salesforce-connector/data-factory-salesforce-api-name.png)

<span data-ttu-id="c4439-226">**Utdatauppsättning för Azure-blobb**</span><span class="sxs-lookup"><span data-stu-id="c4439-226">**Azure blob output dataset**</span></span>

<span data-ttu-id="c4439-227">Data skrivs till en ny blob varje timme (frekvens: timme, intervall: 1).</span><span class="sxs-lookup"><span data-stu-id="c4439-227">Data is written to a new blob every hour (frequency: hour, interval: 1).</span></span>

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

<span data-ttu-id="c4439-228">**Rörledningen med Kopieringsaktiviteten**</span><span class="sxs-lookup"><span data-stu-id="c4439-228">**Pipeline with Copy Activity**</span></span>

<span data-ttu-id="c4439-229">Pipelinen innehåller Kopieringsaktiviteten som är konfigurerad för att använda indata och utdata-datauppsättningar och är schemalagd att köras varje timme.</span><span class="sxs-lookup"><span data-stu-id="c4439-229">The pipeline contains Copy Activity, which is configured to use the input and output datasets, and is scheduled to run every hour.</span></span> <span data-ttu-id="c4439-230">I pipeline-JSON-definitionen av **källa** är inställd på **RelationalSource**, och **sink** är inställd på **BlobSink**.</span><span class="sxs-lookup"><span data-stu-id="c4439-230">In the pipeline JSON definition, the **source** type is set to **RelationalSource**, and the **sink** type is set to **BlobSink**.</span></span>

<span data-ttu-id="c4439-231">Se [RelationalSource Typegenskaper](#copy-activity-properties) lista över egenskaper som stöds av RelationalSource.</span><span class="sxs-lookup"><span data-stu-id="c4439-231">See [RelationalSource type properties](#copy-activity-properties) for the list of properties that are supported by the RelationalSource.</span></span>

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
            "description": "Copy from Salesforce to an Azure blob",
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
> <span data-ttu-id="c4439-232">”__C”-delen av API-namnet krävs för alla anpassade objekt.</span><span class="sxs-lookup"><span data-stu-id="c4439-232">The "__c" part of the API Name is needed for any custom object.</span></span>

![Namnet på data Factory - anslutning Salesforce - API](media/data-factory-salesforce-connector/data-factory-salesforce-api-name-2.png)


### <a name="type-mapping-for-salesforce"></a><span data-ttu-id="c4439-234">Mappning för Salesforce</span><span class="sxs-lookup"><span data-stu-id="c4439-234">Type mapping for Salesforce</span></span>
| <span data-ttu-id="c4439-235">Salesforce-typ</span><span class="sxs-lookup"><span data-stu-id="c4439-235">Salesforce type</span></span> | <span data-ttu-id="c4439-236">. NET-baserade typ</span><span class="sxs-lookup"><span data-stu-id="c4439-236">.NET-based type</span></span> |
| --- | --- |
| <span data-ttu-id="c4439-237">Automatisk tal</span><span class="sxs-lookup"><span data-stu-id="c4439-237">Auto Number</span></span> |<span data-ttu-id="c4439-238">Sträng</span><span class="sxs-lookup"><span data-stu-id="c4439-238">String</span></span> |
| <span data-ttu-id="c4439-239">Kryssruta</span><span class="sxs-lookup"><span data-stu-id="c4439-239">Checkbox</span></span> |<span data-ttu-id="c4439-240">Booleskt värde</span><span class="sxs-lookup"><span data-stu-id="c4439-240">Boolean</span></span> |
| <span data-ttu-id="c4439-241">Valuta</span><span class="sxs-lookup"><span data-stu-id="c4439-241">Currency</span></span> |<span data-ttu-id="c4439-242">dubbla</span><span class="sxs-lookup"><span data-stu-id="c4439-242">Double</span></span> |
| <span data-ttu-id="c4439-243">Date</span><span class="sxs-lookup"><span data-stu-id="c4439-243">Date</span></span> |<span data-ttu-id="c4439-244">Datum och tid</span><span class="sxs-lookup"><span data-stu-id="c4439-244">DateTime</span></span> |
| <span data-ttu-id="c4439-245">Datum/tid</span><span class="sxs-lookup"><span data-stu-id="c4439-245">Date/Time</span></span> |<span data-ttu-id="c4439-246">Datum och tid</span><span class="sxs-lookup"><span data-stu-id="c4439-246">DateTime</span></span> |
| <span data-ttu-id="c4439-247">E-post</span><span class="sxs-lookup"><span data-stu-id="c4439-247">Email</span></span> |<span data-ttu-id="c4439-248">Sträng</span><span class="sxs-lookup"><span data-stu-id="c4439-248">String</span></span> |
| <span data-ttu-id="c4439-249">Id</span><span class="sxs-lookup"><span data-stu-id="c4439-249">Id</span></span> |<span data-ttu-id="c4439-250">Sträng</span><span class="sxs-lookup"><span data-stu-id="c4439-250">String</span></span> |
| <span data-ttu-id="c4439-251">Uppslagsrelation</span><span class="sxs-lookup"><span data-stu-id="c4439-251">Lookup Relationship</span></span> |<span data-ttu-id="c4439-252">Sträng</span><span class="sxs-lookup"><span data-stu-id="c4439-252">String</span></span> |
| <span data-ttu-id="c4439-253">Flerval listruta</span><span class="sxs-lookup"><span data-stu-id="c4439-253">Multi-Select Picklist</span></span> |<span data-ttu-id="c4439-254">Sträng</span><span class="sxs-lookup"><span data-stu-id="c4439-254">String</span></span> |
| <span data-ttu-id="c4439-255">Antal</span><span class="sxs-lookup"><span data-stu-id="c4439-255">Number</span></span> |<span data-ttu-id="c4439-256">dubbla</span><span class="sxs-lookup"><span data-stu-id="c4439-256">Double</span></span> |
| <span data-ttu-id="c4439-257">Procent</span><span class="sxs-lookup"><span data-stu-id="c4439-257">Percent</span></span> |<span data-ttu-id="c4439-258">dubbla</span><span class="sxs-lookup"><span data-stu-id="c4439-258">Double</span></span> |
| <span data-ttu-id="c4439-259">Telefon</span><span class="sxs-lookup"><span data-stu-id="c4439-259">Phone</span></span> |<span data-ttu-id="c4439-260">Sträng</span><span class="sxs-lookup"><span data-stu-id="c4439-260">String</span></span> |
| <span data-ttu-id="c4439-261">Listruta</span><span class="sxs-lookup"><span data-stu-id="c4439-261">Picklist</span></span> |<span data-ttu-id="c4439-262">Sträng</span><span class="sxs-lookup"><span data-stu-id="c4439-262">String</span></span> |
| <span data-ttu-id="c4439-263">Text</span><span class="sxs-lookup"><span data-stu-id="c4439-263">Text</span></span> |<span data-ttu-id="c4439-264">Sträng</span><span class="sxs-lookup"><span data-stu-id="c4439-264">String</span></span> |
| <span data-ttu-id="c4439-265">Textområde</span><span class="sxs-lookup"><span data-stu-id="c4439-265">Text Area</span></span> |<span data-ttu-id="c4439-266">Sträng</span><span class="sxs-lookup"><span data-stu-id="c4439-266">String</span></span> |
| <span data-ttu-id="c4439-267">Textområde (Long)</span><span class="sxs-lookup"><span data-stu-id="c4439-267">Text Area (Long)</span></span> |<span data-ttu-id="c4439-268">Sträng</span><span class="sxs-lookup"><span data-stu-id="c4439-268">String</span></span> |
| <span data-ttu-id="c4439-269">Textområde (omfattande)</span><span class="sxs-lookup"><span data-stu-id="c4439-269">Text Area (Rich)</span></span> |<span data-ttu-id="c4439-270">Sträng</span><span class="sxs-lookup"><span data-stu-id="c4439-270">String</span></span> |
| <span data-ttu-id="c4439-271">Text (krypterade)</span><span class="sxs-lookup"><span data-stu-id="c4439-271">Text (Encrypted)</span></span> |<span data-ttu-id="c4439-272">Sträng</span><span class="sxs-lookup"><span data-stu-id="c4439-272">String</span></span> |
| <span data-ttu-id="c4439-273">URL: EN</span><span class="sxs-lookup"><span data-stu-id="c4439-273">URL</span></span> |<span data-ttu-id="c4439-274">Sträng</span><span class="sxs-lookup"><span data-stu-id="c4439-274">String</span></span> |

> [!NOTE]
> <span data-ttu-id="c4439-275">Om du vill mappa kolumner från källan dataset till kolumner från sink dataset finns [mappa dataset kolumner i Azure Data Factory](data-factory-map-columns.md).</span><span class="sxs-lookup"><span data-stu-id="c4439-275">To map columns from source dataset to columns from sink dataset, see [Mapping dataset columns in Azure Data Factory](data-factory-map-columns.md).</span></span>

[!INCLUDE [data-factory-structure-for-rectangualr-datasets](../../includes/data-factory-structure-for-rectangualr-datasets.md)]

## <a name="performance-and-tuning"></a><span data-ttu-id="c4439-276">Prestanda- och justering</span><span class="sxs-lookup"><span data-stu-id="c4439-276">Performance and tuning</span></span>
<span data-ttu-id="c4439-277">Finns det [prestandajustering guide och Kopieringsaktivitet prestanda](data-factory-copy-activity-performance.md) vill veta mer om viktiga faktorer som påverkan prestanda för flytt av data (Kopieringsaktiviteten) i Azure Data Factory och olika sätt att optimera den.</span><span class="sxs-lookup"><span data-stu-id="c4439-277">See the [Copy Activity performance and tuning guide](data-factory-copy-activity-performance.md) to learn about key factors that impact performance of data movement (Copy Activity) in Azure Data Factory and various ways to optimize it.</span></span>
