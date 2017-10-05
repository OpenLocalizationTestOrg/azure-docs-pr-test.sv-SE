---
title: "Flytta data från OData-källor | Microsoft Docs"
description: "Läs mer om hur du flyttar data från OData-källor med hjälp av Azure Data Factory."
services: data-factory
documentationcenter: 
author: linda33wj
manager: jhubbard
editor: monicar
ms.assetid: de28fa56-3204-4546-a4df-21a21de43ed7
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/04/2017
ms.author: jingwang
ms.openlocfilehash: 624b6c8f317477d83539392c6c2f15c2dc69d401
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="move-data-from-a-odata-source-using-azure-data-factory"></a><span data-ttu-id="b520d-103">Flytta data från en OData-datakälla med hjälp av Azure Data Factory</span><span class="sxs-lookup"><span data-stu-id="b520d-103">Move data From a OData source using Azure Data Factory</span></span>
<span data-ttu-id="b520d-104">Den här artikeln förklarar hur du använder aktiviteten kopiera i Azure Data Factory för att flytta data från en OData-källa.</span><span class="sxs-lookup"><span data-stu-id="b520d-104">This article explains how to use the Copy Activity in Azure Data Factory to move data from an OData source.</span></span> <span data-ttu-id="b520d-105">Den bygger på den [Data Movement aktiviteter](data-factory-data-movement-activities.md) artikel som presenterar en allmän översikt över dataflyttning med copy-aktivitet.</span><span class="sxs-lookup"><span data-stu-id="b520d-105">It builds on the [Data Movement Activities](data-factory-data-movement-activities.md) article, which presents a general overview of data movement with the copy activity.</span></span>

<span data-ttu-id="b520d-106">Du kan kopiera data från en OData-källan till alla stöds sink-datalagret.</span><span class="sxs-lookup"><span data-stu-id="b520d-106">You can copy data from an OData source to any supported sink data store.</span></span> <span data-ttu-id="b520d-107">En lista över datakällor som stöds som sänkor av kopieringsaktiviteten, finns det [stöds datalager](data-factory-data-movement-activities.md#supported-data-stores-and-formats) tabell.</span><span class="sxs-lookup"><span data-stu-id="b520d-107">For a list of data stores supported as sinks by the copy activity, see the [Supported data stores](data-factory-data-movement-activities.md#supported-data-stores-and-formats) table.</span></span> <span data-ttu-id="b520d-108">Data factory stöder för närvarande endast flytta data från en OData-källa till andra databaser, men inte för att flytta data från andra datalager till en OData-källa.</span><span class="sxs-lookup"><span data-stu-id="b520d-108">Data factory currently supports only moving data from an OData source to other data stores, but not for moving data from other data stores to an OData source.</span></span> 

## <a name="supported-versions-and-authentication-types"></a><span data-ttu-id="b520d-109">Versioner som stöds och typer av autentisering</span><span class="sxs-lookup"><span data-stu-id="b520d-109">Supported versions and authentication types</span></span>
<span data-ttu-id="b520d-110">Denna OData-koppling stöd för OData-version 3.0 och 4.0 och du kan kopiera data från både molnet OData och lokala OData-datakällor.</span><span class="sxs-lookup"><span data-stu-id="b520d-110">This OData connector support OData version 3.0 and 4.0, and you can copy data from both cloud OData and on-premises OData sources.</span></span> <span data-ttu-id="b520d-111">Du måste installera Data Management Gateway i det senare.</span><span class="sxs-lookup"><span data-stu-id="b520d-111">For the latter, you need to install the Data Management Gateway.</span></span> <span data-ttu-id="b520d-112">Se [flytta data mellan lokalt och i molnet](data-factory-move-data-between-onprem-and-cloud.md) artikeln för information om Data Management Gateway.</span><span class="sxs-lookup"><span data-stu-id="b520d-112">See [Move data between on-premises and cloud](data-factory-move-data-between-onprem-and-cloud.md) article for details about Data Management Gateway.</span></span>

<span data-ttu-id="b520d-113">Under autentisering stöds:</span><span class="sxs-lookup"><span data-stu-id="b520d-113">Below authentication types are supported:</span></span>

* <span data-ttu-id="b520d-114">Åtkomst **moln** OData-feeden, kan du använda anonym, grundläggande (användarnamn och lösenord) eller Azure Active Directory-baserade OAuth-autentisering.</span><span class="sxs-lookup"><span data-stu-id="b520d-114">To access **cloud** OData feed, you can use anonymous, basic (user name and password), or Azure Active Directory based OAuth authentication.</span></span>
* <span data-ttu-id="b520d-115">Åtkomst **lokalt** OData-feeden kan du använda anonym, grundläggande (användarnamn och lösenord) eller Windows-autentisering.</span><span class="sxs-lookup"><span data-stu-id="b520d-115">To access **on-premises** OData feed, you can use anonymous, basic (user name and password), or Windows authentication.</span></span>

## <a name="getting-started"></a><span data-ttu-id="b520d-116">Komma igång</span><span class="sxs-lookup"><span data-stu-id="b520d-116">Getting started</span></span>
<span data-ttu-id="b520d-117">Du kan skapa en pipeline med en kopia-aktivitet som flyttar data från en OData-datakälla med hjälp av olika verktyg/API: er.</span><span class="sxs-lookup"><span data-stu-id="b520d-117">You can create a pipeline with a copy activity that moves data from an OData source by using different tools/APIs.</span></span>

<span data-ttu-id="b520d-118">Det enklaste sättet att skapa en pipeline är att använda den **guiden Kopiera**.</span><span class="sxs-lookup"><span data-stu-id="b520d-118">The easiest way to create a pipeline is to use the **Copy Wizard**.</span></span> <span data-ttu-id="b520d-119">Finns [Självstudier: skapa en pipeline med hjälp av guiden Kopiera](data-factory-copy-data-wizard-tutorial.md) för en snabb genomgång om hur du skapar en pipeline med hjälp av guiden Kopiera data.</span><span class="sxs-lookup"><span data-stu-id="b520d-119">See [Tutorial: Create a pipeline using Copy Wizard](data-factory-copy-data-wizard-tutorial.md) for a quick walkthrough on creating a pipeline using the Copy data wizard.</span></span>

<span data-ttu-id="b520d-120">Du kan också använda följande verktyg för att skapa en pipeline: **Azure-portalen**, **Visual Studio**, **Azure PowerShell**, **Azure Resource Manager-mall**, **.NET API**, och **REST API**.</span><span class="sxs-lookup"><span data-stu-id="b520d-120">You can also use the following tools to create a pipeline: **Azure portal**, **Visual Studio**, **Azure PowerShell**, **Azure Resource Manager template**, **.NET API**, and **REST API**.</span></span> <span data-ttu-id="b520d-121">Se [kopiera aktivitet kursen](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) för stegvisa instruktioner för att skapa en pipeline med en Kopieringsaktivitet.</span><span class="sxs-lookup"><span data-stu-id="b520d-121">See [Copy activity tutorial](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) for step-by-step instructions to create a pipeline with a copy activity.</span></span> 

<span data-ttu-id="b520d-122">Om du använder verktyg eller API: er, kan du utföra följande steg för att skapa en pipeline som flyttar data från ett dataarkiv som källa till ett dataarkiv som mottagare:</span><span class="sxs-lookup"><span data-stu-id="b520d-122">Whether you use the tools or APIs, you perform the following steps to create a pipeline that moves data from a source data store to a sink data store:</span></span> 

1. <span data-ttu-id="b520d-123">Skapa **länkade tjänster** att länka inkommande och utgående data lagras till din data factory.</span><span class="sxs-lookup"><span data-stu-id="b520d-123">Create **linked services** to link input and output data stores to your data factory.</span></span>
2. <span data-ttu-id="b520d-124">Skapa **datauppsättningar** att representera inkommande och utgående data för kopieringen.</span><span class="sxs-lookup"><span data-stu-id="b520d-124">Create **datasets** to represent input and output data for the copy operation.</span></span> 
3. <span data-ttu-id="b520d-125">Skapa en **pipeline** med en kopia-aktivitet som tar en datamängd som indata och en dataset som utdata.</span><span class="sxs-lookup"><span data-stu-id="b520d-125">Create a **pipeline** with a copy activity that takes a dataset as an input and a dataset as an output.</span></span> 

<span data-ttu-id="b520d-126">När du använder guiden skapas JSON definitioner för dessa Data Factory-enheter (länkade tjänster, datauppsättningar och pipelinen) automatiskt för dig.</span><span class="sxs-lookup"><span data-stu-id="b520d-126">When you use the wizard, JSON definitions for these Data Factory entities (linked services, datasets, and the pipeline) are automatically created for you.</span></span> <span data-ttu-id="b520d-127">När du använder Verktyg/API: er (utom .NET API), kan du definiera dessa Data Factory-enheter med hjälp av JSON-format.</span><span class="sxs-lookup"><span data-stu-id="b520d-127">When you use tools/APIs (except .NET API), you define these Data Factory entities by using the JSON format.</span></span>  <span data-ttu-id="b520d-128">Ett exempel med JSON-definitioner för Data Factory-entiteter som används för att kopiera data från en OData-källan finns [JSON-exempel: kopiera data från OData-källan till Azure Blob](#json-example-copy-data-from-odata-source-to-azure-blob) i den här artikeln.</span><span class="sxs-lookup"><span data-stu-id="b520d-128">For a sample with JSON definitions for Data Factory entities that are used to copy data from an OData source, see [JSON example: Copy data from OData source to Azure Blob](#json-example-copy-data-from-odata-source-to-azure-blob) section of this article.</span></span> 

<span data-ttu-id="b520d-129">Följande avsnitt innehåller information om JSON-egenskaper som används för att definiera Data Factory entiteter till OData-källan:</span><span class="sxs-lookup"><span data-stu-id="b520d-129">The following sections provide details about JSON properties that are used to define Data Factory entities specific to OData source:</span></span>

## <a name="linked-service-properties"></a><span data-ttu-id="b520d-130">Den länkade tjänstens egenskaper</span><span class="sxs-lookup"><span data-stu-id="b520d-130">Linked Service properties</span></span>
<span data-ttu-id="b520d-131">Följande tabell innehåller en beskrivning för JSON-element som är specifika för OData länkad tjänst.</span><span class="sxs-lookup"><span data-stu-id="b520d-131">The following table provides description for JSON elements specific to OData linked service.</span></span>

| <span data-ttu-id="b520d-132">Egenskap</span><span class="sxs-lookup"><span data-stu-id="b520d-132">Property</span></span> | <span data-ttu-id="b520d-133">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="b520d-133">Description</span></span> | <span data-ttu-id="b520d-134">Krävs</span><span class="sxs-lookup"><span data-stu-id="b520d-134">Required</span></span> |
| --- | --- | --- |
| <span data-ttu-id="b520d-135">typ</span><span class="sxs-lookup"><span data-stu-id="b520d-135">type</span></span> |<span data-ttu-id="b520d-136">Egenskapen type måste anges till: **OData**</span><span class="sxs-lookup"><span data-stu-id="b520d-136">The type property must be set to: **OData**</span></span> |<span data-ttu-id="b520d-137">Ja</span><span class="sxs-lookup"><span data-stu-id="b520d-137">Yes</span></span> |
| <span data-ttu-id="b520d-138">URL: en</span><span class="sxs-lookup"><span data-stu-id="b520d-138">url</span></span> |<span data-ttu-id="b520d-139">URL för OData-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="b520d-139">Url of the OData service.</span></span> |<span data-ttu-id="b520d-140">Ja</span><span class="sxs-lookup"><span data-stu-id="b520d-140">Yes</span></span> |
| <span data-ttu-id="b520d-141">AuthenticationType</span><span class="sxs-lookup"><span data-stu-id="b520d-141">authenticationType</span></span> |<span data-ttu-id="b520d-142">Typ av autentisering som används för att ansluta till OData-källan.</span><span class="sxs-lookup"><span data-stu-id="b520d-142">Type of authentication used to connect to the OData source.</span></span> <br/><br/> <span data-ttu-id="b520d-143">För molnet OData är möjliga värden anonym, grundläggande och OAuth (Observera Azure Data Factory för närvarande endast stöder Azure Active Directory-baserad OAuth).</span><span class="sxs-lookup"><span data-stu-id="b520d-143">For cloud OData, possible values are Anonymous, Basic, and OAuth (note Azure Data Factory currently only support Azure Active Directory based OAuth).</span></span> <br/><br/> <span data-ttu-id="b520d-144">För lokala OData är möjliga värden anonym, grundläggande och Windows.</span><span class="sxs-lookup"><span data-stu-id="b520d-144">For on-premises OData, possible values are Anonymous, Basic, and Windows.</span></span> |<span data-ttu-id="b520d-145">Ja</span><span class="sxs-lookup"><span data-stu-id="b520d-145">Yes</span></span> |
| <span data-ttu-id="b520d-146">användarnamn</span><span class="sxs-lookup"><span data-stu-id="b520d-146">username</span></span> |<span data-ttu-id="b520d-147">Ange användarnamnet om du använder grundläggande autentisering.</span><span class="sxs-lookup"><span data-stu-id="b520d-147">Specify user name if you are using Basic authentication.</span></span> |<span data-ttu-id="b520d-148">Ja (endast om du använder grundläggande autentisering)</span><span class="sxs-lookup"><span data-stu-id="b520d-148">Yes (only if you are using Basic authentication)</span></span> |
| <span data-ttu-id="b520d-149">lösenord</span><span class="sxs-lookup"><span data-stu-id="b520d-149">password</span></span> |<span data-ttu-id="b520d-150">Ange lösenordet för det användarkonto som du angav för användarnamnet.</span><span class="sxs-lookup"><span data-stu-id="b520d-150">Specify password for the user account you specified for the username.</span></span> |<span data-ttu-id="b520d-151">Ja (endast om du använder grundläggande autentisering)</span><span class="sxs-lookup"><span data-stu-id="b520d-151">Yes (only if you are using Basic authentication)</span></span> |
| <span data-ttu-id="b520d-152">authorizedCredential</span><span class="sxs-lookup"><span data-stu-id="b520d-152">authorizedCredential</span></span> |<span data-ttu-id="b520d-153">Om du använder OAuth, klickar du på **auktorisera** i guiden för Data Factory kopiera eller redigerare och ange dina autentiseringsuppgifter och sedan värdet för den här egenskapen kommer att genereras automatiskt.</span><span class="sxs-lookup"><span data-stu-id="b520d-153">If you are using OAuth, click **Authorize** button in the Data Factory Copy Wizard or Editor and enter your credential, then the value of this property will be auto-generated.</span></span> |<span data-ttu-id="b520d-154">Ja (endast om du använder OAuth-autentisering)</span><span class="sxs-lookup"><span data-stu-id="b520d-154">Yes (only if you are using OAuth authentication)</span></span> |
| <span data-ttu-id="b520d-155">gatewayName</span><span class="sxs-lookup"><span data-stu-id="b520d-155">gatewayName</span></span> |<span data-ttu-id="b520d-156">Namnet på den gateway som Data Factory-tjänsten ska använda för att ansluta till den lokala OData-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="b520d-156">Name of the gateway that the Data Factory service should use to connect to the on-premises OData service.</span></span> <span data-ttu-id="b520d-157">Ange endast om du vill kopiera data från lokala OData-källan.</span><span class="sxs-lookup"><span data-stu-id="b520d-157">Specify only if you are copying data from on-prem OData source.</span></span> |<span data-ttu-id="b520d-158">Nej</span><span class="sxs-lookup"><span data-stu-id="b520d-158">No</span></span> |

### <a name="using-basic-authentication"></a><span data-ttu-id="b520d-159">Med grundläggande autentisering</span><span class="sxs-lookup"><span data-stu-id="b520d-159">Using Basic authentication</span></span>
```json
{
    "name": "inputLinkedService",
    "properties":
    {
        "type": "OData",
            "typeProperties":
        {
            "url": "http://services.odata.org/OData/OData.svc",
            "authenticationType": "Basic",
            "username": "username",
            "password": "password"
        }
    }
}
```

### <a name="using-anonymous-authentication"></a><span data-ttu-id="b520d-160">Använder anonym autentisering</span><span class="sxs-lookup"><span data-stu-id="b520d-160">Using Anonymous authentication</span></span>
```json
{
    "name": "ODataLinkedService",
        "properties":
    {
        "type": "OData",
        "typeProperties":
        {
            "url": "http://services.odata.org/OData/OData.svc",
            "authenticationType": "Anonymous"
        }
    }
}
```

### <a name="using-windows-authentication-accessing-on-premises-odata-source"></a><span data-ttu-id="b520d-161">Med Windows-autentisering att komma åt lokala OData-källan</span><span class="sxs-lookup"><span data-stu-id="b520d-161">Using Windows authentication accessing on-premises OData source</span></span>
```json
{
    "name": "inputLinkedService",
    "properties":
    {
        "type": "OData",
            "typeProperties":
        {
            "url": "<endpoint of on-premises OData source e.g. Dynamics CRM>",
            "authenticationType": "Windows",
            "username": "domain\\user",
            "password": "password",
            "gatewayName": "mygateway"
        }
    }
}
```

### <a name="using-oauth-authentication-accessing-cloud-odata-source"></a><span data-ttu-id="b520d-162">Med hjälp av OAuth-autentisering vid åtkomst till molnet OData-källan</span><span class="sxs-lookup"><span data-stu-id="b520d-162">Using OAuth authentication accessing cloud OData source</span></span>
```json
{
    "name": "inputLinkedService",
    "properties":
    {
        "type": "OData",
            "typeProperties":
        {
            "url": "<endpoint of cloud OData source e.g. https://<tenant>.crm.dynamics.com/XRMServices/2011/OrganizationData.svc>",
            "authenticationType": "OAuth",
            "authorizedCredential": "<auto generated by clicking the Authorize button on UI>"
        }
    }
}
```

## <a name="dataset-properties"></a><span data-ttu-id="b520d-163">Egenskaper för datamängd</span><span class="sxs-lookup"><span data-stu-id="b520d-163">Dataset properties</span></span>
<span data-ttu-id="b520d-164">En fullständig lista över egenskaper som är tillgängliga för att definiera datauppsättningarna & avsnitt finns i [skapa datauppsättningar](data-factory-create-datasets.md) artikel.</span><span class="sxs-lookup"><span data-stu-id="b520d-164">For a full list of sections & properties available for defining datasets, see the [Creating datasets](data-factory-create-datasets.md) article.</span></span> <span data-ttu-id="b520d-165">Avsnitt som struktur, tillgänglighet och princip på en datamängd JSON är liknande för alla typer av dataset (Azure SQL Azure blob, Azure-tabellen, osv.).</span><span class="sxs-lookup"><span data-stu-id="b520d-165">Sections such as structure, availability, and policy of a dataset JSON are similar for all dataset types (Azure SQL, Azure blob, Azure table, etc.).</span></span>

<span data-ttu-id="b520d-166">Den **typeProperties** avsnitt är olika för varje typ av dataset och innehåller information om placeringen av data i datalagret.</span><span class="sxs-lookup"><span data-stu-id="b520d-166">The **typeProperties** section is different for each type of dataset and provides information about the location of the data in the data store.</span></span> <span data-ttu-id="b520d-167">TypeProperties avsnittet för dataset av typen **ODataResource** (som omfattar OData dataset) har följande egenskaper</span><span class="sxs-lookup"><span data-stu-id="b520d-167">The typeProperties section for dataset of type **ODataResource** (which includes OData dataset) has the following properties</span></span>

| <span data-ttu-id="b520d-168">Egenskap</span><span class="sxs-lookup"><span data-stu-id="b520d-168">Property</span></span> | <span data-ttu-id="b520d-169">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="b520d-169">Description</span></span> | <span data-ttu-id="b520d-170">Krävs</span><span class="sxs-lookup"><span data-stu-id="b520d-170">Required</span></span> |
| --- | --- | --- |
| <span data-ttu-id="b520d-171">Sökväg</span><span class="sxs-lookup"><span data-stu-id="b520d-171">path</span></span> |<span data-ttu-id="b520d-172">Sökvägen till OData-resurs</span><span class="sxs-lookup"><span data-stu-id="b520d-172">Path to the OData resource</span></span> |<span data-ttu-id="b520d-173">Nej</span><span class="sxs-lookup"><span data-stu-id="b520d-173">No</span></span> |

## <a name="copy-activity-properties"></a><span data-ttu-id="b520d-174">Kopiera egenskaper för aktivitet</span><span class="sxs-lookup"><span data-stu-id="b520d-174">Copy activity properties</span></span>
<span data-ttu-id="b520d-175">En fullständig lista över avsnitt & egenskaper som är tillgängliga för att definiera aktiviteter finns i [skapar Pipelines](data-factory-create-pipelines.md) artikel.</span><span class="sxs-lookup"><span data-stu-id="b520d-175">For a full list of sections & properties available for defining activities, see the [Creating Pipelines](data-factory-create-pipelines.md) article.</span></span> <span data-ttu-id="b520d-176">Egenskaper som namn, beskrivning, ingående och utgående tabeller och principen är tillgängliga för alla typer av aktiviteter.</span><span class="sxs-lookup"><span data-stu-id="b520d-176">Properties such as name, description, input and output tables, and policy are available for all types of activities.</span></span>

<span data-ttu-id="b520d-177">Egenskaper som är tillgängliga i avsnittet typeProperties i aktiviteten varierar å andra sidan beroende på varje aktivitetstyp.</span><span class="sxs-lookup"><span data-stu-id="b520d-177">Properties available in the typeProperties section of the activity on the other hand vary with each activity type.</span></span> <span data-ttu-id="b520d-178">För Kopieringsaktivitet kan variera de beroende på vilka typer av datakällor och sänkor.</span><span class="sxs-lookup"><span data-stu-id="b520d-178">For Copy activity, they vary depending on the types of sources and sinks.</span></span>

<span data-ttu-id="b520d-179">När datakällan är av typen **RelationalSource** (som omfattar OData) följande egenskaper finns i avsnittet typeProperties:</span><span class="sxs-lookup"><span data-stu-id="b520d-179">When source is of type **RelationalSource** (which includes OData) the following properties are available in typeProperties section:</span></span>

| <span data-ttu-id="b520d-180">Egenskap</span><span class="sxs-lookup"><span data-stu-id="b520d-180">Property</span></span> | <span data-ttu-id="b520d-181">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="b520d-181">Description</span></span> | <span data-ttu-id="b520d-182">Exempel</span><span class="sxs-lookup"><span data-stu-id="b520d-182">Example</span></span> | <span data-ttu-id="b520d-183">Krävs</span><span class="sxs-lookup"><span data-stu-id="b520d-183">Required</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="b520d-184">DocumentDB</span><span class="sxs-lookup"><span data-stu-id="b520d-184">query</span></span> |<span data-ttu-id="b520d-185">Använd anpassad fråga för att läsa data.</span><span class="sxs-lookup"><span data-stu-id="b520d-185">Use the custom query to read data.</span></span> |<span data-ttu-id="b520d-186">”? $select = namn, beskrivning och $top = 5”</span><span class="sxs-lookup"><span data-stu-id="b520d-186">"?$select=Name, Description&$top=5"</span></span> |<span data-ttu-id="b520d-187">Nej</span><span class="sxs-lookup"><span data-stu-id="b520d-187">No</span></span> |

## <a name="type-mapping-for-odata"></a><span data-ttu-id="b520d-188">Mappning för OData</span><span class="sxs-lookup"><span data-stu-id="b520d-188">Type Mapping for OData</span></span>
<span data-ttu-id="b520d-189">Som anges i den [data movement aktiviteter](data-factory-data-movement-activities.md) artikeln kopieringsaktiviteten utför automatisk konverteringar från källtyper att registrera typer med följande metod i två steg.</span><span class="sxs-lookup"><span data-stu-id="b520d-189">As mentioned in the [data movement activities](data-factory-data-movement-activities.md) article, Copy activity performs automatic type conversions from source types to sink types with the following two-step approach.</span></span>

1. <span data-ttu-id="b520d-190">Konvertera från interna källtyper till .NET-typ</span><span class="sxs-lookup"><span data-stu-id="b520d-190">Convert from native source types to .NET type</span></span>
2. <span data-ttu-id="b520d-191">Konvertera från .NET-typ till interna mottagare typ.</span><span class="sxs-lookup"><span data-stu-id="b520d-191">Convert from .NET type to native sink type</span></span>

<span data-ttu-id="b520d-192">När du flyttar data från OData, används följande mappningar från OData-typer till .NET-typ.</span><span class="sxs-lookup"><span data-stu-id="b520d-192">When moving data from OData, the following mappings are used from OData types to .NET type.</span></span>

| <span data-ttu-id="b520d-193">OData-datatyp</span><span class="sxs-lookup"><span data-stu-id="b520d-193">OData Data Type</span></span> | <span data-ttu-id="b520d-194">.NET-typ</span><span class="sxs-lookup"><span data-stu-id="b520d-194">.NET Type</span></span> |
| --- | --- |
| <span data-ttu-id="b520d-195">Edm.Binary</span><span class="sxs-lookup"><span data-stu-id="b520d-195">Edm.Binary</span></span> |<span data-ttu-id="b520d-196">byte]</span><span class="sxs-lookup"><span data-stu-id="b520d-196">Byte[]</span></span> |
| <span data-ttu-id="b520d-197">Edm.Boolean</span><span class="sxs-lookup"><span data-stu-id="b520d-197">Edm.Boolean</span></span> |<span data-ttu-id="b520d-198">bool</span><span class="sxs-lookup"><span data-stu-id="b520d-198">Bool</span></span> |
| <span data-ttu-id="b520d-199">Edm.Byte</span><span class="sxs-lookup"><span data-stu-id="b520d-199">Edm.Byte</span></span> |<span data-ttu-id="b520d-200">byte]</span><span class="sxs-lookup"><span data-stu-id="b520d-200">Byte[]</span></span> |
| <span data-ttu-id="b520d-201">Edm.DateTime</span><span class="sxs-lookup"><span data-stu-id="b520d-201">Edm.DateTime</span></span> |<span data-ttu-id="b520d-202">Datum och tid</span><span class="sxs-lookup"><span data-stu-id="b520d-202">DateTime</span></span> |
| <span data-ttu-id="b520d-203">Edm.Decimal</span><span class="sxs-lookup"><span data-stu-id="b520d-203">Edm.Decimal</span></span> |<span data-ttu-id="b520d-204">Decimal</span><span class="sxs-lookup"><span data-stu-id="b520d-204">Decimal</span></span> |
| <span data-ttu-id="b520d-205">Edm.Double</span><span class="sxs-lookup"><span data-stu-id="b520d-205">Edm.Double</span></span> |<span data-ttu-id="b520d-206">dubbla</span><span class="sxs-lookup"><span data-stu-id="b520d-206">Double</span></span> |
| <span data-ttu-id="b520d-207">Edm.Single</span><span class="sxs-lookup"><span data-stu-id="b520d-207">Edm.Single</span></span> |<span data-ttu-id="b520d-208">Enskild</span><span class="sxs-lookup"><span data-stu-id="b520d-208">Single</span></span> |
| <span data-ttu-id="b520d-209">Edm.Guid</span><span class="sxs-lookup"><span data-stu-id="b520d-209">Edm.Guid</span></span> |<span data-ttu-id="b520d-210">GUID</span><span class="sxs-lookup"><span data-stu-id="b520d-210">Guid</span></span> |
| <span data-ttu-id="b520d-211">Edm.Int16</span><span class="sxs-lookup"><span data-stu-id="b520d-211">Edm.Int16</span></span> |<span data-ttu-id="b520d-212">Int16</span><span class="sxs-lookup"><span data-stu-id="b520d-212">Int16</span></span> |
| <span data-ttu-id="b520d-213">Edm.Int32</span><span class="sxs-lookup"><span data-stu-id="b520d-213">Edm.Int32</span></span> |<span data-ttu-id="b520d-214">Int32</span><span class="sxs-lookup"><span data-stu-id="b520d-214">Int32</span></span> |
| <span data-ttu-id="b520d-215">Edm.Int64</span><span class="sxs-lookup"><span data-stu-id="b520d-215">Edm.Int64</span></span> |<span data-ttu-id="b520d-216">Int64</span><span class="sxs-lookup"><span data-stu-id="b520d-216">Int64</span></span> |
| <span data-ttu-id="b520d-217">Edm.SByte</span><span class="sxs-lookup"><span data-stu-id="b520d-217">Edm.SByte</span></span> |<span data-ttu-id="b520d-218">Int16</span><span class="sxs-lookup"><span data-stu-id="b520d-218">Int16</span></span> |
| <span data-ttu-id="b520d-219">Edm.String</span><span class="sxs-lookup"><span data-stu-id="b520d-219">Edm.String</span></span> |<span data-ttu-id="b520d-220">Sträng</span><span class="sxs-lookup"><span data-stu-id="b520d-220">String</span></span> |
| <span data-ttu-id="b520d-221">Edm.Time</span><span class="sxs-lookup"><span data-stu-id="b520d-221">Edm.Time</span></span> |<span data-ttu-id="b520d-222">TimeSpan</span><span class="sxs-lookup"><span data-stu-id="b520d-222">TimeSpan</span></span> |
| <span data-ttu-id="b520d-223">Edm.DateTimeOffset</span><span class="sxs-lookup"><span data-stu-id="b520d-223">Edm.DateTimeOffset</span></span> |<span data-ttu-id="b520d-224">DateTimeOffset</span><span class="sxs-lookup"><span data-stu-id="b520d-224">DateTimeOffset</span></span> |

> [!Note]
> <span data-ttu-id="b520d-225">OData komplexa datatyper t.ex. objektet stöds inte.</span><span class="sxs-lookup"><span data-stu-id="b520d-225">OData complex data types e.g. Object are not supported.</span></span>

## <a name="json-example-copy-data-from-odata-source-to-azure-blob"></a><span data-ttu-id="b520d-226">JSON-exempel: kopiera data från OData-källan till Azure-Blob</span><span class="sxs-lookup"><span data-stu-id="b520d-226">JSON example: Copy data from OData source to Azure Blob</span></span>
<span data-ttu-id="b520d-227">Det här exemplet innehåller exempel JSON definitioner som du kan använda för att skapa en pipeline med [Azure-portalen](data-factory-copy-activity-tutorial-using-azure-portal.md) eller [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md) eller [Azure PowerShell](data-factory-copy-activity-tutorial-using-powershell.md).</span><span class="sxs-lookup"><span data-stu-id="b520d-227">This example provides sample JSON definitions that you can use to create a pipeline by using [Azure portal](data-factory-copy-activity-tutorial-using-azure-portal.md) or [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md) or [Azure PowerShell](data-factory-copy-activity-tutorial-using-powershell.md).</span></span> <span data-ttu-id="b520d-228">De visar hur du kopierar data från en OData-datakälla till en Azure Blob Storage.</span><span class="sxs-lookup"><span data-stu-id="b520d-228">They show how to copy data from an OData source to an Azure Blob Storage.</span></span> <span data-ttu-id="b520d-229">Dock datan kan kopieras till någon av sänkor anges [här](data-factory-data-movement-activities.md#supported-data-stores-and-formats) med hjälp av aktiviteten kopiera i Azure Data Factory.</span><span class="sxs-lookup"><span data-stu-id="b520d-229">However, data can be copied to any of the sinks stated [here](data-factory-data-movement-activities.md#supported-data-stores-and-formats) using the Copy Activity in Azure Data Factory.</span></span> <span data-ttu-id="b520d-230">Exemplet har följande Data Factory-enheter:</span><span class="sxs-lookup"><span data-stu-id="b520d-230">The sample has the following Data Factory entities:</span></span>

1. <span data-ttu-id="b520d-231">En länkad tjänst av typen [OData](#linked-service-properties).</span><span class="sxs-lookup"><span data-stu-id="b520d-231">A linked service of type [OData](#linked-service-properties).</span></span>
2. <span data-ttu-id="b520d-232">En länkad tjänst av typen [AzureStorage](data-factory-azure-blob-connector.md#linked-service-properties).</span><span class="sxs-lookup"><span data-stu-id="b520d-232">A linked service of type [AzureStorage](data-factory-azure-blob-connector.md#linked-service-properties).</span></span>
3. <span data-ttu-id="b520d-233">Indata [dataset](data-factory-create-datasets.md) av typen [ODataResource](#dataset-properties).</span><span class="sxs-lookup"><span data-stu-id="b520d-233">An input [dataset](data-factory-create-datasets.md) of type [ODataResource](#dataset-properties).</span></span>
4. <span data-ttu-id="b520d-234">Utdata [dataset](data-factory-create-datasets.md) av typen [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties).</span><span class="sxs-lookup"><span data-stu-id="b520d-234">An output [dataset](data-factory-create-datasets.md) of type [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties).</span></span>
5. <span data-ttu-id="b520d-235">En [pipeline](data-factory-create-pipelines.md) med Kopieringsaktiviteten som använder [RelationalSource](#copy-activity-properties) och [BlobSink](data-factory-azure-blob-connector.md#copy-activity-properties).</span><span class="sxs-lookup"><span data-stu-id="b520d-235">A [pipeline](data-factory-create-pipelines.md) with Copy Activity that uses [RelationalSource](#copy-activity-properties) and [BlobSink](data-factory-azure-blob-connector.md#copy-activity-properties).</span></span>

<span data-ttu-id="b520d-236">Exemplet kopierar data från frågor körs mot en OData-källan till en Azure blob varje timme.</span><span class="sxs-lookup"><span data-stu-id="b520d-236">The sample copies data from querying against an OData source to an Azure blob every hour.</span></span> <span data-ttu-id="b520d-237">JSON-egenskaper som används i exemplen beskrivs i exemplen i följande avsnitt.</span><span class="sxs-lookup"><span data-stu-id="b520d-237">The JSON properties used in these samples are described in sections following the samples.</span></span>

<span data-ttu-id="b520d-238">**OData länkade tjänsten:** det här exemplet använder anonym autentisering.</span><span class="sxs-lookup"><span data-stu-id="b520d-238">**OData linked service:** This example uses the Anonymous authentication.</span></span> <span data-ttu-id="b520d-239">Se [OData länkade tjänsten](#linked-service-properties) avsnittet för olika typer av autentisering som du kan använda.</span><span class="sxs-lookup"><span data-stu-id="b520d-239">See [OData linked service](#linked-service-properties) section for different types of authentication you can use.</span></span>

```json
{
    "name": "ODataLinkedService",
        "properties":
    {
        "type": "OData",
            "typeProperties":
        {
            "url": "http://services.odata.org/OData/OData.svc",
            "authenticationType": "Anonymous"
            }
        }
}
```

<span data-ttu-id="b520d-240">**Azure Storage länkade tjänsten:**</span><span class="sxs-lookup"><span data-stu-id="b520d-240">**Azure Storage linked service:**</span></span>

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

<span data-ttu-id="b520d-241">**OData inkommande datauppsättningen:**</span><span class="sxs-lookup"><span data-stu-id="b520d-241">**OData input dataset:**</span></span>

<span data-ttu-id="b520d-242">Inställningen ”externa”: ”true” informerar Data Factory-tjänsten att datamängden är extern till data factory och inte tillverkas av en aktivitet i datafabriken.</span><span class="sxs-lookup"><span data-stu-id="b520d-242">Setting “external”: ”true” informs the Data Factory service that the dataset is external to the data factory and is not produced by an activity in the data factory.</span></span>

```json
{
    "name": "ODataDataset",
    "properties":
    {
        "type": "ODataResource",
        "typeProperties":
        {
                "path": "Products"
        },
        "linkedServiceName": "ODataLinkedService",
        "structure": [],
        "availability": {
            "frequency": "Hour",
            "interval": 1
        },
        "external": true,
        "policy": {
            "retryInterval": "00:01:00",
            "retryTimeout": "00:10:00",
            "maximumRetry": 3                
        }
    }
}
```

<span data-ttu-id="b520d-243">Ange **sökvägen** i datamängden definition är valfritt.</span><span class="sxs-lookup"><span data-stu-id="b520d-243">Specifying **path** in the dataset definition is optional.</span></span>

<span data-ttu-id="b520d-244">**Azure Blob utdatauppsättningen:**</span><span class="sxs-lookup"><span data-stu-id="b520d-244">**Azure Blob output dataset:**</span></span>

<span data-ttu-id="b520d-245">Data skrivs till en ny blob varje timme (frekvens: timme, intervall: 1).</span><span class="sxs-lookup"><span data-stu-id="b520d-245">Data is written to a new blob every hour (frequency: hour, interval: 1).</span></span> <span data-ttu-id="b520d-246">Sökvägen till mappen för blobben utvärderas dynamiskt baserat på starttiden för den sektor som bearbetas.</span><span class="sxs-lookup"><span data-stu-id="b520d-246">The folder path for the blob is dynamically evaluated based on the start time of the slice that is being processed.</span></span> <span data-ttu-id="b520d-247">Mappsökvägen använder år, månad, dag och timmar delar av starttiden.</span><span class="sxs-lookup"><span data-stu-id="b520d-247">The folder path uses year, month, day, and hours parts of the start time.</span></span>

```json
{
    "name": "AzureBlobODataDataSet",
    "properties": {
        "type": "AzureBlob",
        "linkedServiceName": "AzureStorageLinkedService",
        "typeProperties": {
            "folderPath": "mycontainer/odata/yearno={Year}/monthno={Month}/dayno={Day}/hourno={Hour}",
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


<span data-ttu-id="b520d-248">**Kopiera aktivitet i en pipeline med OData-källa och mottagare för Blob:**</span><span class="sxs-lookup"><span data-stu-id="b520d-248">**Copy activity in a pipeline with OData source and Blob sink:**</span></span>

<span data-ttu-id="b520d-249">Pipelinen innehåller en kopia-aktivitet som är konfigurerad för att använda indata och utdata-datauppsättningar och är schemalagd att köras varje timme.</span><span class="sxs-lookup"><span data-stu-id="b520d-249">The pipeline contains a Copy Activity that is configured to use the input and output datasets and is scheduled to run every hour.</span></span> <span data-ttu-id="b520d-250">I pipeline-JSON-definitionen av **källa** är inställd på **RelationalSource** och **sink** är inställd på **BlobSink**.</span><span class="sxs-lookup"><span data-stu-id="b520d-250">In the pipeline JSON definition, the **source** type is set to **RelationalSource** and **sink** type is set to **BlobSink**.</span></span> <span data-ttu-id="b520d-251">SQL-frågan som angetts för den **frågan** egenskapen väljer senaste (senaste) data från OData-källan.</span><span class="sxs-lookup"><span data-stu-id="b520d-251">The SQL query specified for the **query** property selects the latest (newest) data from the OData source.</span></span>

```json
{
    "name": "CopyODataToBlob",
    "properties": {
        "description": "pipeline for copy activity",
        "activities": [
            {
                "type": "Copy",
                "typeProperties": {
                    "source": {
                        "type": "RelationalSource",
                        "query": "?$select=Name, Description&$top=5",
                    },
                    "sink": {
                        "type": "BlobSink",
                        "writeBatchSize": 0,
                        "writeBatchTimeout": "00:00:00"
                    }
                },
                "inputs": [
                    {
                        "name": "ODataDataSet"
                    }
                ],
                "outputs": [
                    {
                        "name": "AzureBlobODataDataSet"
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
                "name": "ODataToBlob"
            }
        ],
        "start": "2017-02-01T18:00:00Z",
        "end": "2017-02-03T19:00:00Z"
    }
}
```

<span data-ttu-id="b520d-252">Ange **frågan** i pipelinen definition är valfritt.</span><span class="sxs-lookup"><span data-stu-id="b520d-252">Specifying **query** in the pipeline definition is optional.</span></span> <span data-ttu-id="b520d-253">Den **URL** att Data Factory-tjänsten använder för att hämta data är: URL som anges i den länkade tjänsten (krävs) + sökväg som anges i datauppsättningen (valfritt) + frågan i pipeline (valfritt).</span><span class="sxs-lookup"><span data-stu-id="b520d-253">The **URL** that the Data Factory service uses to retrieve data is: URL specified in the linked service (required) + path specified in the dataset (optional) + query in the pipeline (optional).</span></span>


### <a name="type-mapping-for-odata"></a><span data-ttu-id="b520d-254">Mappning för OData</span><span class="sxs-lookup"><span data-stu-id="b520d-254">Type mapping for OData</span></span>
<span data-ttu-id="b520d-255">Som anges i den [data movement aktiviteter](data-factory-data-movement-activities.md) artikeln kopieringsaktiviteten utför automatisk konverteringar från källtyper att registrera typer med följande metod i steg 2:</span><span class="sxs-lookup"><span data-stu-id="b520d-255">As mentioned in the [data movement activities](data-factory-data-movement-activities.md) article, Copy activity performs automatic type conversions from source types to sink types with the following 2-step approach:</span></span>

1. <span data-ttu-id="b520d-256">Konvertera från interna källtyper till .NET-typ</span><span class="sxs-lookup"><span data-stu-id="b520d-256">Convert from native source types to .NET type</span></span>
2. <span data-ttu-id="b520d-257">Konvertera från .NET-typ till interna mottagare typ.</span><span class="sxs-lookup"><span data-stu-id="b520d-257">Convert from .NET type to native sink type</span></span>

<span data-ttu-id="b520d-258">När flytta data från OData-data lagras mappas OData-datatyper till .NET-typer.</span><span class="sxs-lookup"><span data-stu-id="b520d-258">When moving data from OData data stores, OData data types are mapped to .NET types.</span></span>

## <a name="map-source-to-sink-columns"></a><span data-ttu-id="b520d-259">Karta källan till mottagare för kolumner</span><span class="sxs-lookup"><span data-stu-id="b520d-259">Map source to sink columns</span></span>
<span data-ttu-id="b520d-260">Mer information om mappning kolumner i datauppsättningen källan till kolumner i datauppsättning mottagare, se [mappa dataset kolumner i Azure Data Factory](data-factory-map-columns.md).</span><span class="sxs-lookup"><span data-stu-id="b520d-260">To learn about mapping columns in source dataset to columns in sink dataset, see [Mapping dataset columns in Azure Data Factory](data-factory-map-columns.md).</span></span>

## <a name="repeatable-read-from-relational-sources"></a><span data-ttu-id="b520d-261">Upprepbar läsning från relationella källor</span><span class="sxs-lookup"><span data-stu-id="b520d-261">Repeatable read from relational sources</span></span>
<span data-ttu-id="b520d-262">Tänk på att undvika oväntade resultat repeterbarhet när kopiering av data från relationella data lagras.</span><span class="sxs-lookup"><span data-stu-id="b520d-262">When copying data from relational data stores, keep repeatability in mind to avoid unintended outcomes.</span></span> <span data-ttu-id="b520d-263">I Azure Data Factory, kan du köra en sektor manuellt.</span><span class="sxs-lookup"><span data-stu-id="b520d-263">In Azure Data Factory, you can rerun a slice manually.</span></span> <span data-ttu-id="b520d-264">Du kan också konfigurera i principen för en dataset så att ett segment som körs när ett fel uppstår.</span><span class="sxs-lookup"><span data-stu-id="b520d-264">You can also configure retry policy for a dataset so that a slice is rerun when a failure occurs.</span></span> <span data-ttu-id="b520d-265">När ett segment körs på något sätt, måste du kontrollera att samma data läses oavsett hur många gånger ett segment körs.</span><span class="sxs-lookup"><span data-stu-id="b520d-265">When a slice is rerun in either way, you need to make sure that the same data is read no matter how many times a slice is run.</span></span> <span data-ttu-id="b520d-266">Se [Repeatable läsa från relationella källor](data-factory-repeatable-copy.md#repeatable-read-from-relational-sources).</span><span class="sxs-lookup"><span data-stu-id="b520d-266">See [Repeatable read from relational sources](data-factory-repeatable-copy.md#repeatable-read-from-relational-sources).</span></span>

## <a name="performance-and-tuning"></a><span data-ttu-id="b520d-267">Prestanda och finjustering</span><span class="sxs-lookup"><span data-stu-id="b520d-267">Performance and Tuning</span></span>
<span data-ttu-id="b520d-268">Se [kopiera aktivitet prestanda och justera guiden](data-factory-copy-activity-performance.md) vill veta mer om viktiga faktorer som påverkan prestanda för flytt av data (Kopieringsaktiviteten) i Azure Data Factory och olika sätt att optimera den.</span><span class="sxs-lookup"><span data-stu-id="b520d-268">See [Copy Activity Performance & Tuning Guide](data-factory-copy-activity-performance.md) to learn about key factors that impact performance of data movement (Copy Activity) in Azure Data Factory and various ways to optimize it.</span></span>
