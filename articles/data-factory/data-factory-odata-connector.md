---
title: "aaaMove data från OData-källor | Microsoft Docs"
description: "Läs mer om hur toomove från OData-datakällor med hjälp av Azure Data Factory."
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
ms.openlocfilehash: 328efe4fd274fb3e54c1d2f209e4614c77c1ff37
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="move-data-from-a-odata-source-using-azure-data-factory"></a><span data-ttu-id="9d12a-103">Flytta data från en OData-datakälla med hjälp av Azure Data Factory</span><span class="sxs-lookup"><span data-stu-id="9d12a-103">Move data From a OData source using Azure Data Factory</span></span>
<span data-ttu-id="9d12a-104">Den här artikeln förklarar hur toouse hello Kopieringsaktiviteten i Azure Data Factory toomove data från en OData-källa.</span><span class="sxs-lookup"><span data-stu-id="9d12a-104">This article explains how toouse hello Copy Activity in Azure Data Factory toomove data from an OData source.</span></span> <span data-ttu-id="9d12a-105">Den bygger på hello [Data Movement aktiviteter](data-factory-data-movement-activities.md) artikel som ger en allmän översikt över dataflyttning hello kopieringsaktiviteten.</span><span class="sxs-lookup"><span data-stu-id="9d12a-105">It builds on hello [Data Movement Activities](data-factory-data-movement-activities.md) article, which presents a general overview of data movement with hello copy activity.</span></span>

<span data-ttu-id="9d12a-106">Du kan kopiera data från ett dataarkiv för OData-källan tooany stöds mottagare.</span><span class="sxs-lookup"><span data-stu-id="9d12a-106">You can copy data from an OData source tooany supported sink data store.</span></span> <span data-ttu-id="9d12a-107">En lista över data lagras som stöds när egenskaperna av hello kopieringsaktiviteten Se hello [stöds datalager](data-factory-data-movement-activities.md#supported-data-stores-and-formats) tabell.</span><span class="sxs-lookup"><span data-stu-id="9d12a-107">For a list of data stores supported as sinks by hello copy activity, see hello [Supported data stores](data-factory-data-movement-activities.md#supported-data-stores-and-formats) table.</span></span> <span data-ttu-id="9d12a-108">Data factory stöder för närvarande endast flytta data från en OData-källan tooother data lagras, men inte för att flytta data från andra data lagras tooan OData-källan.</span><span class="sxs-lookup"><span data-stu-id="9d12a-108">Data factory currently supports only moving data from an OData source tooother data stores, but not for moving data from other data stores tooan OData source.</span></span> 

## <a name="supported-versions-and-authentication-types"></a><span data-ttu-id="9d12a-109">Versioner som stöds och typer av autentisering</span><span class="sxs-lookup"><span data-stu-id="9d12a-109">Supported versions and authentication types</span></span>
<span data-ttu-id="9d12a-110">Denna OData-koppling stöd för OData-version 3.0 och 4.0 och du kan kopiera data från både molnet OData och lokala OData-datakällor.</span><span class="sxs-lookup"><span data-stu-id="9d12a-110">This OData connector support OData version 3.0 and 4.0, and you can copy data from both cloud OData and on-premises OData sources.</span></span> <span data-ttu-id="9d12a-111">För hello senare behöver du tooinstall hello Data Management Gateway.</span><span class="sxs-lookup"><span data-stu-id="9d12a-111">For hello latter, you need tooinstall hello Data Management Gateway.</span></span> <span data-ttu-id="9d12a-112">Se [flytta data mellan lokalt och i molnet](data-factory-move-data-between-onprem-and-cloud.md) artikeln för information om Data Management Gateway.</span><span class="sxs-lookup"><span data-stu-id="9d12a-112">See [Move data between on-premises and cloud](data-factory-move-data-between-onprem-and-cloud.md) article for details about Data Management Gateway.</span></span>

<span data-ttu-id="9d12a-113">Under autentisering stöds:</span><span class="sxs-lookup"><span data-stu-id="9d12a-113">Below authentication types are supported:</span></span>

* <span data-ttu-id="9d12a-114">tooaccess **moln** OData-feeden, kan du använda anonym, grundläggande (användarnamn och lösenord) eller Azure Active Directory-baserade OAuth-autentisering.</span><span class="sxs-lookup"><span data-stu-id="9d12a-114">tooaccess **cloud** OData feed, you can use anonymous, basic (user name and password), or Azure Active Directory based OAuth authentication.</span></span>
* <span data-ttu-id="9d12a-115">tooaccess **lokalt** OData-feeden kan du använda anonym, grundläggande (användarnamn och lösenord) eller Windows-autentisering.</span><span class="sxs-lookup"><span data-stu-id="9d12a-115">tooaccess **on-premises** OData feed, you can use anonymous, basic (user name and password), or Windows authentication.</span></span>

## <a name="getting-started"></a><span data-ttu-id="9d12a-116">Komma igång</span><span class="sxs-lookup"><span data-stu-id="9d12a-116">Getting started</span></span>
<span data-ttu-id="9d12a-117">Du kan skapa en pipeline med en kopia-aktivitet som flyttar data från en OData-datakälla med hjälp av olika verktyg/API: er.</span><span class="sxs-lookup"><span data-stu-id="9d12a-117">You can create a pipeline with a copy activity that moves data from an OData source by using different tools/APIs.</span></span>

<span data-ttu-id="9d12a-118">hello enklaste sättet toocreate en pipeline är toouse hello **guiden Kopiera**.</span><span class="sxs-lookup"><span data-stu-id="9d12a-118">hello easiest way toocreate a pipeline is toouse hello **Copy Wizard**.</span></span> <span data-ttu-id="9d12a-119">Se [Självstudier: skapa en pipeline med hjälp av guiden Kopiera](data-factory-copy-data-wizard-tutorial.md) för en snabb genomgång om hur du skapar en pipeline med hjälp av guiden för hello kopiera data.</span><span class="sxs-lookup"><span data-stu-id="9d12a-119">See [Tutorial: Create a pipeline using Copy Wizard](data-factory-copy-data-wizard-tutorial.md) for a quick walkthrough on creating a pipeline using hello Copy data wizard.</span></span>

<span data-ttu-id="9d12a-120">Du kan också använda följande verktyg toocreate en pipeline hello: **Azure-portalen**, **Visual Studio**, **Azure PowerShell**, **Azure Resource Manager-mall** , **.NET API**, och **REST API**.</span><span class="sxs-lookup"><span data-stu-id="9d12a-120">You can also use hello following tools toocreate a pipeline: **Azure portal**, **Visual Studio**, **Azure PowerShell**, **Azure Resource Manager template**, **.NET API**, and **REST API**.</span></span> <span data-ttu-id="9d12a-121">Se [kopiera aktivitet kursen](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) för stegvisa instruktioner toocreate en pipeline med en Kopieringsaktivitet.</span><span class="sxs-lookup"><span data-stu-id="9d12a-121">See [Copy activity tutorial](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) for step-by-step instructions toocreate a pipeline with a copy activity.</span></span> 

<span data-ttu-id="9d12a-122">Om du använder hello verktyg eller API: er kan utföra du hello följande steg toocreate en pipeline som flyttar data från en källdata lagra tooa sink-datalagret:</span><span class="sxs-lookup"><span data-stu-id="9d12a-122">Whether you use hello tools or APIs, you perform hello following steps toocreate a pipeline that moves data from a source data store tooa sink data store:</span></span> 

1. <span data-ttu-id="9d12a-123">Skapa **länkade tjänster** toolink indata och utdata lagrar tooyour data factory.</span><span class="sxs-lookup"><span data-stu-id="9d12a-123">Create **linked services** toolink input and output data stores tooyour data factory.</span></span>
2. <span data-ttu-id="9d12a-124">Skapa **datauppsättningar** toorepresent indata och utdata för hello kopieringsåtgärden.</span><span class="sxs-lookup"><span data-stu-id="9d12a-124">Create **datasets** toorepresent input and output data for hello copy operation.</span></span> 
3. <span data-ttu-id="9d12a-125">Skapa en **pipeline** med en kopia-aktivitet som tar en datamängd som indata och en dataset som utdata.</span><span class="sxs-lookup"><span data-stu-id="9d12a-125">Create a **pipeline** with a copy activity that takes a dataset as an input and a dataset as an output.</span></span> 

<span data-ttu-id="9d12a-126">När du använder guiden hello skapas automatiskt JSON definitioner för dessa Data Factory-enheter (länkade tjänster, datauppsättningar och hello pipeline) för dig.</span><span class="sxs-lookup"><span data-stu-id="9d12a-126">When you use hello wizard, JSON definitions for these Data Factory entities (linked services, datasets, and hello pipeline) are automatically created for you.</span></span> <span data-ttu-id="9d12a-127">När du använder Verktyg/API: er (utom .NET API), kan du definiera dessa Data Factory-enheter med hjälp av hello JSON-format.</span><span class="sxs-lookup"><span data-stu-id="9d12a-127">When you use tools/APIs (except .NET API), you define these Data Factory entities by using hello JSON format.</span></span>  <span data-ttu-id="9d12a-128">Ett exempel med JSON-definitioner för Data Factory-entiteter som ska använda toocopy data från en OData-källan finns [JSON-exempel: kopiera data från OData-källan tooAzure Blob](#json-example-copy-data-from-odata-source-to-azure-blob) i den här artikeln.</span><span class="sxs-lookup"><span data-stu-id="9d12a-128">For a sample with JSON definitions for Data Factory entities that are used toocopy data from an OData source, see [JSON example: Copy data from OData source tooAzure Blob](#json-example-copy-data-from-odata-source-to-azure-blob) section of this article.</span></span> 

<span data-ttu-id="9d12a-129">hello följande avsnitt innehåller information om JSON-egenskaper som används toodefine Data Factory entiteter specifika tooOData källan:</span><span class="sxs-lookup"><span data-stu-id="9d12a-129">hello following sections provide details about JSON properties that are used toodefine Data Factory entities specific tooOData source:</span></span>

## <a name="linked-service-properties"></a><span data-ttu-id="9d12a-130">Den länkade tjänstens egenskaper</span><span class="sxs-lookup"><span data-stu-id="9d12a-130">Linked Service properties</span></span>
<span data-ttu-id="9d12a-131">hello följande tabell innehåller en beskrivning för JSON-element specifika tooOData länkad tjänst.</span><span class="sxs-lookup"><span data-stu-id="9d12a-131">hello following table provides description for JSON elements specific tooOData linked service.</span></span>

| <span data-ttu-id="9d12a-132">Egenskap</span><span class="sxs-lookup"><span data-stu-id="9d12a-132">Property</span></span> | <span data-ttu-id="9d12a-133">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="9d12a-133">Description</span></span> | <span data-ttu-id="9d12a-134">Krävs</span><span class="sxs-lookup"><span data-stu-id="9d12a-134">Required</span></span> |
| --- | --- | --- |
| <span data-ttu-id="9d12a-135">typ</span><span class="sxs-lookup"><span data-stu-id="9d12a-135">type</span></span> |<span data-ttu-id="9d12a-136">hello Typegenskapen måste anges till: **OData**</span><span class="sxs-lookup"><span data-stu-id="9d12a-136">hello type property must be set to: **OData**</span></span> |<span data-ttu-id="9d12a-137">Ja</span><span class="sxs-lookup"><span data-stu-id="9d12a-137">Yes</span></span> |
| <span data-ttu-id="9d12a-138">URL: en</span><span class="sxs-lookup"><span data-stu-id="9d12a-138">url</span></span> |<span data-ttu-id="9d12a-139">URL till hello OData-tjänst.</span><span class="sxs-lookup"><span data-stu-id="9d12a-139">Url of hello OData service.</span></span> |<span data-ttu-id="9d12a-140">Ja</span><span class="sxs-lookup"><span data-stu-id="9d12a-140">Yes</span></span> |
| <span data-ttu-id="9d12a-141">AuthenticationType</span><span class="sxs-lookup"><span data-stu-id="9d12a-141">authenticationType</span></span> |<span data-ttu-id="9d12a-142">Typ av autentisering används tooconnect toohello OData-källan.</span><span class="sxs-lookup"><span data-stu-id="9d12a-142">Type of authentication used tooconnect toohello OData source.</span></span> <br/><br/> <span data-ttu-id="9d12a-143">För molnet OData är möjliga värden anonym, grundläggande och OAuth (Observera Azure Data Factory för närvarande endast stöder Azure Active Directory-baserad OAuth).</span><span class="sxs-lookup"><span data-stu-id="9d12a-143">For cloud OData, possible values are Anonymous, Basic, and OAuth (note Azure Data Factory currently only support Azure Active Directory based OAuth).</span></span> <br/><br/> <span data-ttu-id="9d12a-144">För lokala OData är möjliga värden anonym, grundläggande och Windows.</span><span class="sxs-lookup"><span data-stu-id="9d12a-144">For on-premises OData, possible values are Anonymous, Basic, and Windows.</span></span> |<span data-ttu-id="9d12a-145">Ja</span><span class="sxs-lookup"><span data-stu-id="9d12a-145">Yes</span></span> |
| <span data-ttu-id="9d12a-146">användarnamn</span><span class="sxs-lookup"><span data-stu-id="9d12a-146">username</span></span> |<span data-ttu-id="9d12a-147">Ange användarnamnet om du använder grundläggande autentisering.</span><span class="sxs-lookup"><span data-stu-id="9d12a-147">Specify user name if you are using Basic authentication.</span></span> |<span data-ttu-id="9d12a-148">Ja (endast om du använder grundläggande autentisering)</span><span class="sxs-lookup"><span data-stu-id="9d12a-148">Yes (only if you are using Basic authentication)</span></span> |
| <span data-ttu-id="9d12a-149">lösenord</span><span class="sxs-lookup"><span data-stu-id="9d12a-149">password</span></span> |<span data-ttu-id="9d12a-150">Ange lösenord för hello-användarkonto som du angav för hello användarnamn.</span><span class="sxs-lookup"><span data-stu-id="9d12a-150">Specify password for hello user account you specified for hello username.</span></span> |<span data-ttu-id="9d12a-151">Ja (endast om du använder grundläggande autentisering)</span><span class="sxs-lookup"><span data-stu-id="9d12a-151">Yes (only if you are using Basic authentication)</span></span> |
| <span data-ttu-id="9d12a-152">authorizedCredential</span><span class="sxs-lookup"><span data-stu-id="9d12a-152">authorizedCredential</span></span> |<span data-ttu-id="9d12a-153">Om du använder OAuth, klickar du på **auktorisera** i hello guiden för Data Factory kopiera eller redigerare och ange dina autentiseringsuppgifter och sedan hello värdet på egenskapen kommer att genereras automatiskt.</span><span class="sxs-lookup"><span data-stu-id="9d12a-153">If you are using OAuth, click **Authorize** button in hello Data Factory Copy Wizard or Editor and enter your credential, then hello value of this property will be auto-generated.</span></span> |<span data-ttu-id="9d12a-154">Ja (endast om du använder OAuth-autentisering)</span><span class="sxs-lookup"><span data-stu-id="9d12a-154">Yes (only if you are using OAuth authentication)</span></span> |
| <span data-ttu-id="9d12a-155">gatewayName</span><span class="sxs-lookup"><span data-stu-id="9d12a-155">gatewayName</span></span> |<span data-ttu-id="9d12a-156">Namnet på hello-gateway som hello Data Factory-tjänsten ska använda tooconnect toohello lokala OData-tjänst.</span><span class="sxs-lookup"><span data-stu-id="9d12a-156">Name of hello gateway that hello Data Factory service should use tooconnect toohello on-premises OData service.</span></span> <span data-ttu-id="9d12a-157">Ange endast om du vill kopiera data från lokala OData-källan.</span><span class="sxs-lookup"><span data-stu-id="9d12a-157">Specify only if you are copying data from on-prem OData source.</span></span> |<span data-ttu-id="9d12a-158">Nej</span><span class="sxs-lookup"><span data-stu-id="9d12a-158">No</span></span> |

### <a name="using-basic-authentication"></a><span data-ttu-id="9d12a-159">Med grundläggande autentisering</span><span class="sxs-lookup"><span data-stu-id="9d12a-159">Using Basic authentication</span></span>
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

### <a name="using-anonymous-authentication"></a><span data-ttu-id="9d12a-160">Använder anonym autentisering</span><span class="sxs-lookup"><span data-stu-id="9d12a-160">Using Anonymous authentication</span></span>
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

### <a name="using-windows-authentication-accessing-on-premises-odata-source"></a><span data-ttu-id="9d12a-161">Med Windows-autentisering att komma åt lokala OData-källan</span><span class="sxs-lookup"><span data-stu-id="9d12a-161">Using Windows authentication accessing on-premises OData source</span></span>
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

### <a name="using-oauth-authentication-accessing-cloud-odata-source"></a><span data-ttu-id="9d12a-162">Med hjälp av OAuth-autentisering vid åtkomst till molnet OData-källan</span><span class="sxs-lookup"><span data-stu-id="9d12a-162">Using OAuth authentication accessing cloud OData source</span></span>
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
            "authorizedCredential": "<auto generated by clicking hello Authorize button on UI>"
        }
    }
}
```

## <a name="dataset-properties"></a><span data-ttu-id="9d12a-163">Egenskaper för datamängd</span><span class="sxs-lookup"><span data-stu-id="9d12a-163">Dataset properties</span></span>
<span data-ttu-id="9d12a-164">En fullständig lista över egenskaper som är tillgängliga för att definiera datauppsättningarna & avsnitt finns hello [skapa datauppsättningar](data-factory-create-datasets.md) artikel.</span><span class="sxs-lookup"><span data-stu-id="9d12a-164">For a full list of sections & properties available for defining datasets, see hello [Creating datasets](data-factory-create-datasets.md) article.</span></span> <span data-ttu-id="9d12a-165">Avsnitt som struktur, tillgänglighet och princip på en datamängd JSON är liknande för alla typer av dataset (Azure SQL Azure blob, Azure-tabellen, osv.).</span><span class="sxs-lookup"><span data-stu-id="9d12a-165">Sections such as structure, availability, and policy of a dataset JSON are similar for all dataset types (Azure SQL, Azure blob, Azure table, etc.).</span></span>

<span data-ttu-id="9d12a-166">Hej **typeProperties** avsnitt är olika för varje typ av dataset och ger information om hello platsen för hello data i datalagret hello.</span><span class="sxs-lookup"><span data-stu-id="9d12a-166">hello **typeProperties** section is different for each type of dataset and provides information about hello location of hello data in hello data store.</span></span> <span data-ttu-id="9d12a-167">Hej typeProperties avsnittet för dataset av typen **ODataResource** (som omfattar OData dataset) har hello följande egenskaper</span><span class="sxs-lookup"><span data-stu-id="9d12a-167">hello typeProperties section for dataset of type **ODataResource** (which includes OData dataset) has hello following properties</span></span>

| <span data-ttu-id="9d12a-168">Egenskap</span><span class="sxs-lookup"><span data-stu-id="9d12a-168">Property</span></span> | <span data-ttu-id="9d12a-169">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="9d12a-169">Description</span></span> | <span data-ttu-id="9d12a-170">Krävs</span><span class="sxs-lookup"><span data-stu-id="9d12a-170">Required</span></span> |
| --- | --- | --- |
| <span data-ttu-id="9d12a-171">Sökväg</span><span class="sxs-lookup"><span data-stu-id="9d12a-171">path</span></span> |<span data-ttu-id="9d12a-172">Sökvägen toohello OData-resurs</span><span class="sxs-lookup"><span data-stu-id="9d12a-172">Path toohello OData resource</span></span> |<span data-ttu-id="9d12a-173">Nej</span><span class="sxs-lookup"><span data-stu-id="9d12a-173">No</span></span> |

## <a name="copy-activity-properties"></a><span data-ttu-id="9d12a-174">Kopiera egenskaper för aktivitet</span><span class="sxs-lookup"><span data-stu-id="9d12a-174">Copy activity properties</span></span>
<span data-ttu-id="9d12a-175">En fullständig lista över avsnitt & egenskaper som är tillgängliga för att definiera aktiviteter finns hello [skapar Pipelines](data-factory-create-pipelines.md) artikel.</span><span class="sxs-lookup"><span data-stu-id="9d12a-175">For a full list of sections & properties available for defining activities, see hello [Creating Pipelines](data-factory-create-pipelines.md) article.</span></span> <span data-ttu-id="9d12a-176">Egenskaper som namn, beskrivning, ingående och utgående tabeller och principen är tillgängliga för alla typer av aktiviteter.</span><span class="sxs-lookup"><span data-stu-id="9d12a-176">Properties such as name, description, input and output tables, and policy are available for all types of activities.</span></span>

<span data-ttu-id="9d12a-177">Egenskaper i hello typeProperties avsnittet hello aktivitet på hello andra sidan varierar med varje aktivitetstyp.</span><span class="sxs-lookup"><span data-stu-id="9d12a-177">Properties available in hello typeProperties section of hello activity on hello other hand vary with each activity type.</span></span> <span data-ttu-id="9d12a-178">För Kopieringsaktivitet kan varierar de beroende på hello typer av datakällor och sänkor.</span><span class="sxs-lookup"><span data-stu-id="9d12a-178">For Copy activity, they vary depending on hello types of sources and sinks.</span></span>

<span data-ttu-id="9d12a-179">När datakällan är av typen **RelationalSource** (som omfattar OData) hello följande egenskaper finns i avsnittet typeProperties:</span><span class="sxs-lookup"><span data-stu-id="9d12a-179">When source is of type **RelationalSource** (which includes OData) hello following properties are available in typeProperties section:</span></span>

| <span data-ttu-id="9d12a-180">Egenskap</span><span class="sxs-lookup"><span data-stu-id="9d12a-180">Property</span></span> | <span data-ttu-id="9d12a-181">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="9d12a-181">Description</span></span> | <span data-ttu-id="9d12a-182">Exempel</span><span class="sxs-lookup"><span data-stu-id="9d12a-182">Example</span></span> | <span data-ttu-id="9d12a-183">Krävs</span><span class="sxs-lookup"><span data-stu-id="9d12a-183">Required</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="9d12a-184">DocumentDB</span><span class="sxs-lookup"><span data-stu-id="9d12a-184">query</span></span> |<span data-ttu-id="9d12a-185">Använda hello anpassad fråga tooread data.</span><span class="sxs-lookup"><span data-stu-id="9d12a-185">Use hello custom query tooread data.</span></span> |<span data-ttu-id="9d12a-186">”? $select = namn, beskrivning och $top = 5”</span><span class="sxs-lookup"><span data-stu-id="9d12a-186">"?$select=Name, Description&$top=5"</span></span> |<span data-ttu-id="9d12a-187">Nej</span><span class="sxs-lookup"><span data-stu-id="9d12a-187">No</span></span> |

## <a name="type-mapping-for-odata"></a><span data-ttu-id="9d12a-188">Mappning för OData</span><span class="sxs-lookup"><span data-stu-id="9d12a-188">Type Mapping for OData</span></span>
<span data-ttu-id="9d12a-189">Som anges i hello [data movement aktiviteter](data-factory-data-movement-activities.md) artikeln kopieringsaktiviteten utför automatisk konverteringar från källan typer toosink typer med hello följande metod i två steg.</span><span class="sxs-lookup"><span data-stu-id="9d12a-189">As mentioned in hello [data movement activities](data-factory-data-movement-activities.md) article, Copy activity performs automatic type conversions from source types toosink types with hello following two-step approach.</span></span>

1. <span data-ttu-id="9d12a-190">Konvertera från inbyggda typer too.NET källtypen</span><span class="sxs-lookup"><span data-stu-id="9d12a-190">Convert from native source types too.NET type</span></span>
2. <span data-ttu-id="9d12a-191">Konvertera från .NET typen toonative Mottagartypen</span><span class="sxs-lookup"><span data-stu-id="9d12a-191">Convert from .NET type toonative sink type</span></span>

<span data-ttu-id="9d12a-192">När du flyttar data från OData som hello följande mappningar används från OData-typer too.NET typen.</span><span class="sxs-lookup"><span data-stu-id="9d12a-192">When moving data from OData, hello following mappings are used from OData types too.NET type.</span></span>

| <span data-ttu-id="9d12a-193">OData-datatyp</span><span class="sxs-lookup"><span data-stu-id="9d12a-193">OData Data Type</span></span> | <span data-ttu-id="9d12a-194">.NET-typ</span><span class="sxs-lookup"><span data-stu-id="9d12a-194">.NET Type</span></span> |
| --- | --- |
| <span data-ttu-id="9d12a-195">Edm.Binary</span><span class="sxs-lookup"><span data-stu-id="9d12a-195">Edm.Binary</span></span> |<span data-ttu-id="9d12a-196">byte]</span><span class="sxs-lookup"><span data-stu-id="9d12a-196">Byte[]</span></span> |
| <span data-ttu-id="9d12a-197">Edm.Boolean</span><span class="sxs-lookup"><span data-stu-id="9d12a-197">Edm.Boolean</span></span> |<span data-ttu-id="9d12a-198">bool</span><span class="sxs-lookup"><span data-stu-id="9d12a-198">Bool</span></span> |
| <span data-ttu-id="9d12a-199">Edm.Byte</span><span class="sxs-lookup"><span data-stu-id="9d12a-199">Edm.Byte</span></span> |<span data-ttu-id="9d12a-200">byte]</span><span class="sxs-lookup"><span data-stu-id="9d12a-200">Byte[]</span></span> |
| <span data-ttu-id="9d12a-201">Edm.DateTime</span><span class="sxs-lookup"><span data-stu-id="9d12a-201">Edm.DateTime</span></span> |<span data-ttu-id="9d12a-202">Datum och tid</span><span class="sxs-lookup"><span data-stu-id="9d12a-202">DateTime</span></span> |
| <span data-ttu-id="9d12a-203">Edm.Decimal</span><span class="sxs-lookup"><span data-stu-id="9d12a-203">Edm.Decimal</span></span> |<span data-ttu-id="9d12a-204">Decimal</span><span class="sxs-lookup"><span data-stu-id="9d12a-204">Decimal</span></span> |
| <span data-ttu-id="9d12a-205">Edm.Double</span><span class="sxs-lookup"><span data-stu-id="9d12a-205">Edm.Double</span></span> |<span data-ttu-id="9d12a-206">dubbla</span><span class="sxs-lookup"><span data-stu-id="9d12a-206">Double</span></span> |
| <span data-ttu-id="9d12a-207">Edm.Single</span><span class="sxs-lookup"><span data-stu-id="9d12a-207">Edm.Single</span></span> |<span data-ttu-id="9d12a-208">Enskild</span><span class="sxs-lookup"><span data-stu-id="9d12a-208">Single</span></span> |
| <span data-ttu-id="9d12a-209">Edm.Guid</span><span class="sxs-lookup"><span data-stu-id="9d12a-209">Edm.Guid</span></span> |<span data-ttu-id="9d12a-210">GUID</span><span class="sxs-lookup"><span data-stu-id="9d12a-210">Guid</span></span> |
| <span data-ttu-id="9d12a-211">Edm.Int16</span><span class="sxs-lookup"><span data-stu-id="9d12a-211">Edm.Int16</span></span> |<span data-ttu-id="9d12a-212">Int16</span><span class="sxs-lookup"><span data-stu-id="9d12a-212">Int16</span></span> |
| <span data-ttu-id="9d12a-213">Edm.Int32</span><span class="sxs-lookup"><span data-stu-id="9d12a-213">Edm.Int32</span></span> |<span data-ttu-id="9d12a-214">Int32</span><span class="sxs-lookup"><span data-stu-id="9d12a-214">Int32</span></span> |
| <span data-ttu-id="9d12a-215">Edm.Int64</span><span class="sxs-lookup"><span data-stu-id="9d12a-215">Edm.Int64</span></span> |<span data-ttu-id="9d12a-216">Int64</span><span class="sxs-lookup"><span data-stu-id="9d12a-216">Int64</span></span> |
| <span data-ttu-id="9d12a-217">Edm.SByte</span><span class="sxs-lookup"><span data-stu-id="9d12a-217">Edm.SByte</span></span> |<span data-ttu-id="9d12a-218">Int16</span><span class="sxs-lookup"><span data-stu-id="9d12a-218">Int16</span></span> |
| <span data-ttu-id="9d12a-219">Edm.String</span><span class="sxs-lookup"><span data-stu-id="9d12a-219">Edm.String</span></span> |<span data-ttu-id="9d12a-220">Sträng</span><span class="sxs-lookup"><span data-stu-id="9d12a-220">String</span></span> |
| <span data-ttu-id="9d12a-221">Edm.Time</span><span class="sxs-lookup"><span data-stu-id="9d12a-221">Edm.Time</span></span> |<span data-ttu-id="9d12a-222">TimeSpan</span><span class="sxs-lookup"><span data-stu-id="9d12a-222">TimeSpan</span></span> |
| <span data-ttu-id="9d12a-223">Edm.DateTimeOffset</span><span class="sxs-lookup"><span data-stu-id="9d12a-223">Edm.DateTimeOffset</span></span> |<span data-ttu-id="9d12a-224">DateTimeOffset</span><span class="sxs-lookup"><span data-stu-id="9d12a-224">DateTimeOffset</span></span> |

> [!Note]
> <span data-ttu-id="9d12a-225">OData komplexa datatyper t.ex. objektet stöds inte.</span><span class="sxs-lookup"><span data-stu-id="9d12a-225">OData complex data types e.g. Object are not supported.</span></span>

## <a name="json-example-copy-data-from-odata-source-tooazure-blob"></a><span data-ttu-id="9d12a-226">JSON-exempel: kopiera data från OData-källan tooAzure Blob</span><span class="sxs-lookup"><span data-stu-id="9d12a-226">JSON example: Copy data from OData source tooAzure Blob</span></span>
<span data-ttu-id="9d12a-227">Det här exemplet innehåller exempel JSON definitioner som du kan använda toocreate en pipeline med hjälp av [Azure-portalen](data-factory-copy-activity-tutorial-using-azure-portal.md) eller [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md) eller [Azure PowerShell](data-factory-copy-activity-tutorial-using-powershell.md).</span><span class="sxs-lookup"><span data-stu-id="9d12a-227">This example provides sample JSON definitions that you can use toocreate a pipeline by using [Azure portal](data-factory-copy-activity-tutorial-using-azure-portal.md) or [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md) or [Azure PowerShell](data-factory-copy-activity-tutorial-using-powershell.md).</span></span> <span data-ttu-id="9d12a-228">De visar hur toocopy från en OData-datakälla tooan Azure Blob Storage.</span><span class="sxs-lookup"><span data-stu-id="9d12a-228">They show how toocopy data from an OData source tooan Azure Blob Storage.</span></span> <span data-ttu-id="9d12a-229">Data kan dock vara kopierade tooany av hello sänkor anges [här](data-factory-data-movement-activities.md#supported-data-stores-and-formats) med hello Kopieringsaktiviteten i Azure Data Factory.</span><span class="sxs-lookup"><span data-stu-id="9d12a-229">However, data can be copied tooany of hello sinks stated [here](data-factory-data-movement-activities.md#supported-data-stores-and-formats) using hello Copy Activity in Azure Data Factory.</span></span> <span data-ttu-id="9d12a-230">hello exemplet har hello följande Data Factory-enheter:</span><span class="sxs-lookup"><span data-stu-id="9d12a-230">hello sample has hello following Data Factory entities:</span></span>

1. <span data-ttu-id="9d12a-231">En länkad tjänst av typen [OData](#linked-service-properties).</span><span class="sxs-lookup"><span data-stu-id="9d12a-231">A linked service of type [OData](#linked-service-properties).</span></span>
2. <span data-ttu-id="9d12a-232">En länkad tjänst av typen [AzureStorage](data-factory-azure-blob-connector.md#linked-service-properties).</span><span class="sxs-lookup"><span data-stu-id="9d12a-232">A linked service of type [AzureStorage](data-factory-azure-blob-connector.md#linked-service-properties).</span></span>
3. <span data-ttu-id="9d12a-233">Indata [dataset](data-factory-create-datasets.md) av typen [ODataResource](#dataset-properties).</span><span class="sxs-lookup"><span data-stu-id="9d12a-233">An input [dataset](data-factory-create-datasets.md) of type [ODataResource](#dataset-properties).</span></span>
4. <span data-ttu-id="9d12a-234">Utdata [dataset](data-factory-create-datasets.md) av typen [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties).</span><span class="sxs-lookup"><span data-stu-id="9d12a-234">An output [dataset](data-factory-create-datasets.md) of type [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties).</span></span>
5. <span data-ttu-id="9d12a-235">En [pipeline](data-factory-create-pipelines.md) med Kopieringsaktiviteten som använder [RelationalSource](#copy-activity-properties) och [BlobSink](data-factory-azure-blob-connector.md#copy-activity-properties).</span><span class="sxs-lookup"><span data-stu-id="9d12a-235">A [pipeline](data-factory-create-pipelines.md) with Copy Activity that uses [RelationalSource](#copy-activity-properties) and [BlobSink](data-factory-azure-blob-connector.md#copy-activity-properties).</span></span>

<span data-ttu-id="9d12a-236">hello exemplet kopierar data från frågor körs mot en OData-källan tooan Azure blob varje timme.</span><span class="sxs-lookup"><span data-stu-id="9d12a-236">hello sample copies data from querying against an OData source tooan Azure blob every hour.</span></span> <span data-ttu-id="9d12a-237">hello JSON egenskaper som används i exemplen beskrivs i hello-exempel i följande avsnitt.</span><span class="sxs-lookup"><span data-stu-id="9d12a-237">hello JSON properties used in these samples are described in sections following hello samples.</span></span>

<span data-ttu-id="9d12a-238">**OData länkade tjänsten:** det här exemplet använder hello anonym autentisering.</span><span class="sxs-lookup"><span data-stu-id="9d12a-238">**OData linked service:** This example uses hello Anonymous authentication.</span></span> <span data-ttu-id="9d12a-239">Se [OData länkade tjänsten](#linked-service-properties) avsnittet för olika typer av autentisering som du kan använda.</span><span class="sxs-lookup"><span data-stu-id="9d12a-239">See [OData linked service](#linked-service-properties) section for different types of authentication you can use.</span></span>

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

<span data-ttu-id="9d12a-240">**Azure Storage länkade tjänsten:**</span><span class="sxs-lookup"><span data-stu-id="9d12a-240">**Azure Storage linked service:**</span></span>

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

<span data-ttu-id="9d12a-241">**OData inkommande datauppsättningen:**</span><span class="sxs-lookup"><span data-stu-id="9d12a-241">**OData input dataset:**</span></span>

<span data-ttu-id="9d12a-242">Inställningen ”externa”: ”true” informerar hello Data Factory-tjänsten som hello dataset är externa toohello data factory och inte tillverkas av en aktivitet i hello data factory.</span><span class="sxs-lookup"><span data-stu-id="9d12a-242">Setting “external”: ”true” informs hello Data Factory service that hello dataset is external toohello data factory and is not produced by an activity in hello data factory.</span></span>

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

<span data-ttu-id="9d12a-243">Ange **sökvägen** i hello dataset definition är valfritt.</span><span class="sxs-lookup"><span data-stu-id="9d12a-243">Specifying **path** in hello dataset definition is optional.</span></span>

<span data-ttu-id="9d12a-244">**Azure Blob utdatauppsättningen:**</span><span class="sxs-lookup"><span data-stu-id="9d12a-244">**Azure Blob output dataset:**</span></span>

<span data-ttu-id="9d12a-245">Data skrivs tooa nya blob varje timme (frekvens: timme, intervall: 1).</span><span class="sxs-lookup"><span data-stu-id="9d12a-245">Data is written tooa new blob every hour (frequency: hour, interval: 1).</span></span> <span data-ttu-id="9d12a-246">hello mappsökväg för hello blob utvärderas dynamiskt baserat på hello starttiden för hello-segment som bearbetas.</span><span class="sxs-lookup"><span data-stu-id="9d12a-246">hello folder path for hello blob is dynamically evaluated based on hello start time of hello slice that is being processed.</span></span> <span data-ttu-id="9d12a-247">hello mappsökväg använder år, månad, dag och timmar delar av hello starttid.</span><span class="sxs-lookup"><span data-stu-id="9d12a-247">hello folder path uses year, month, day, and hours parts of hello start time.</span></span>

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


<span data-ttu-id="9d12a-248">**Kopiera aktivitet i en pipeline med OData-källa och mottagare för Blob:**</span><span class="sxs-lookup"><span data-stu-id="9d12a-248">**Copy activity in a pipeline with OData source and Blob sink:**</span></span>

<span data-ttu-id="9d12a-249">hello pipelinen innehåller en kopia-aktivitet som är konfigurerade toouse hello inkommande och utgående datauppsättningar och är schemalagda toorun varje timme.</span><span class="sxs-lookup"><span data-stu-id="9d12a-249">hello pipeline contains a Copy Activity that is configured toouse hello input and output datasets and is scheduled toorun every hour.</span></span> <span data-ttu-id="9d12a-250">I hello pipeline JSON-definitionen hello **källa** typ har angetts för**RelationalSource** och **sink** typ har angetts för**BlobSink**.</span><span class="sxs-lookup"><span data-stu-id="9d12a-250">In hello pipeline JSON definition, hello **source** type is set too**RelationalSource** and **sink** type is set too**BlobSink**.</span></span> <span data-ttu-id="9d12a-251">hello SQL-frågan som angetts för hello **frågan** egenskapen väljer hello senaste (senaste) data från hello OData-källan.</span><span class="sxs-lookup"><span data-stu-id="9d12a-251">hello SQL query specified for hello **query** property selects hello latest (newest) data from hello OData source.</span></span>

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

<span data-ttu-id="9d12a-252">Ange **frågan** i hello pipeline definition är valfritt.</span><span class="sxs-lookup"><span data-stu-id="9d12a-252">Specifying **query** in hello pipeline definition is optional.</span></span> <span data-ttu-id="9d12a-253">Hej **URL** att hello Data Factory-tjänsten använder tooretrieve data är: URL som anges i hello länkad tjänst (krävs) + sökväg som anges i hello datauppsättningen (valfritt) + fråga i hello pipeline (valfritt).</span><span class="sxs-lookup"><span data-stu-id="9d12a-253">hello **URL** that hello Data Factory service uses tooretrieve data is: URL specified in hello linked service (required) + path specified in hello dataset (optional) + query in hello pipeline (optional).</span></span>


### <a name="type-mapping-for-odata"></a><span data-ttu-id="9d12a-254">Mappning för OData</span><span class="sxs-lookup"><span data-stu-id="9d12a-254">Type mapping for OData</span></span>
<span data-ttu-id="9d12a-255">Som anges i hello [data movement aktiviteter](data-factory-data-movement-activities.md) artikeln kopieringsaktiviteten utför automatisk konverteringar från källan typer toosink typer med hello följande metod i steg 2:</span><span class="sxs-lookup"><span data-stu-id="9d12a-255">As mentioned in hello [data movement activities](data-factory-data-movement-activities.md) article, Copy activity performs automatic type conversions from source types toosink types with hello following 2-step approach:</span></span>

1. <span data-ttu-id="9d12a-256">Konvertera från inbyggda typer too.NET källtypen</span><span class="sxs-lookup"><span data-stu-id="9d12a-256">Convert from native source types too.NET type</span></span>
2. <span data-ttu-id="9d12a-257">Konvertera från .NET typen toonative Mottagartypen</span><span class="sxs-lookup"><span data-stu-id="9d12a-257">Convert from .NET type toonative sink type</span></span>

<span data-ttu-id="9d12a-258">När du flyttar data från OData-datalager är OData-datatyper mappade too.NET typer.</span><span class="sxs-lookup"><span data-stu-id="9d12a-258">When moving data from OData data stores, OData data types are mapped too.NET types.</span></span>

## <a name="map-source-toosink-columns"></a><span data-ttu-id="9d12a-259">Mappa källkolumner toosink</span><span class="sxs-lookup"><span data-stu-id="9d12a-259">Map source toosink columns</span></span>
<span data-ttu-id="9d12a-260">toolearn mappning tabellkolumner i källan dataset toocolumns i sink dataset finns [mappa dataset kolumner i Azure Data Factory](data-factory-map-columns.md).</span><span class="sxs-lookup"><span data-stu-id="9d12a-260">toolearn about mapping columns in source dataset toocolumns in sink dataset, see [Mapping dataset columns in Azure Data Factory](data-factory-map-columns.md).</span></span>

## <a name="repeatable-read-from-relational-sources"></a><span data-ttu-id="9d12a-261">Upprepbar läsning från relationella källor</span><span class="sxs-lookup"><span data-stu-id="9d12a-261">Repeatable read from relational sources</span></span>
<span data-ttu-id="9d12a-262">När du kopierar data från relationella datalager, Kom ihåg tooavoid repeterbarhet oönskade resultat.</span><span class="sxs-lookup"><span data-stu-id="9d12a-262">When copying data from relational data stores, keep repeatability in mind tooavoid unintended outcomes.</span></span> <span data-ttu-id="9d12a-263">I Azure Data Factory, kan du köra en sektor manuellt.</span><span class="sxs-lookup"><span data-stu-id="9d12a-263">In Azure Data Factory, you can rerun a slice manually.</span></span> <span data-ttu-id="9d12a-264">Du kan också konfigurera i principen för en dataset så att ett segment som körs när ett fel uppstår.</span><span class="sxs-lookup"><span data-stu-id="9d12a-264">You can also configure retry policy for a dataset so that a slice is rerun when a failure occurs.</span></span> <span data-ttu-id="9d12a-265">När ett segment körs på antingen sätt måste toomake att som hello samma data läses oavsett hur många gånger ett segment körs.</span><span class="sxs-lookup"><span data-stu-id="9d12a-265">When a slice is rerun in either way, you need toomake sure that hello same data is read no matter how many times a slice is run.</span></span> <span data-ttu-id="9d12a-266">Se [Repeatable läsa från relationella källor](data-factory-repeatable-copy.md#repeatable-read-from-relational-sources).</span><span class="sxs-lookup"><span data-stu-id="9d12a-266">See [Repeatable read from relational sources](data-factory-repeatable-copy.md#repeatable-read-from-relational-sources).</span></span>

## <a name="performance-and-tuning"></a><span data-ttu-id="9d12a-267">Prestanda och finjustering</span><span class="sxs-lookup"><span data-stu-id="9d12a-267">Performance and Tuning</span></span>
<span data-ttu-id="9d12a-268">Se [kopiera aktivitet prestanda och justera guiden](data-factory-copy-activity-performance.md) toolearn om nyckeln faktorer som påverkan prestanda för flytt av data (Kopieringsaktiviteten) i Azure Data Factory och olika sätt toooptimize den.</span><span class="sxs-lookup"><span data-stu-id="9d12a-268">See [Copy Activity Performance & Tuning Guide](data-factory-copy-activity-performance.md) toolearn about key factors that impact performance of data movement (Copy Activity) in Azure Data Factory and various ways toooptimize it.</span></span>
