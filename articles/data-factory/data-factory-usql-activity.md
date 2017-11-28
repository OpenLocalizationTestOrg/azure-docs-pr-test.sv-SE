---
title: "aaaTransform data med hjälp av U-SQL - skript i Azure | Microsoft Docs"
description: "Lär dig hur compute tooprocess eller Transformera data genom att köra U-SQL-skript på Azure Data Lake Analytics-tjänsten."
services: data-factory
documentationcenter: 
author: spelluru
manager: jhubbard
editor: monicar
ms.assetid: e17c1255-62c2-4e2e-bb60-d25274903e80
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/10/2017
ms.author: spelluru
ms.openlocfilehash: 51fdb40334d0c131720f65c3a96b4c5045a98b24
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="transform-data-by-running-u-sql-scripts-on-azure-data-lake-analytics"></a><span data-ttu-id="f3b55-103">Transformera data genom att köra U-SQL-skript på Azure Data Lake Analytics</span><span class="sxs-lookup"><span data-stu-id="f3b55-103">Transform data by running U-SQL scripts on Azure Data Lake Analytics</span></span> 
> [!div class="op_single_selector" title1="Transformation Activities"]
> * [<span data-ttu-id="f3b55-104">Hive-aktivitet</span><span class="sxs-lookup"><span data-stu-id="f3b55-104">Hive Activity</span></span>](data-factory-hive-activity.md) 
> * [<span data-ttu-id="f3b55-105">Pig-aktivitet</span><span class="sxs-lookup"><span data-stu-id="f3b55-105">Pig Activity</span></span>](data-factory-pig-activity.md)
> * [<span data-ttu-id="f3b55-106">MapReduce Activity</span><span class="sxs-lookup"><span data-stu-id="f3b55-106">MapReduce Activity</span></span>](data-factory-map-reduce.md)
> * [<span data-ttu-id="f3b55-107">Hadoop Streaming Activity</span><span class="sxs-lookup"><span data-stu-id="f3b55-107">Hadoop Streaming Activity</span></span>](data-factory-hadoop-streaming-activity.md)
> * [<span data-ttu-id="f3b55-108">Spark-aktivitet</span><span class="sxs-lookup"><span data-stu-id="f3b55-108">Spark Activity</span></span>](data-factory-spark.md)
> * [<span data-ttu-id="f3b55-109">Machine Learning Batch-körningsaktivitet</span><span class="sxs-lookup"><span data-stu-id="f3b55-109">Machine Learning Batch Execution Activity</span></span>](data-factory-azure-ml-batch-execution-activity.md)
> * [<span data-ttu-id="f3b55-110">Machine Learning-uppdateringsresursaktivitet</span><span class="sxs-lookup"><span data-stu-id="f3b55-110">Machine Learning Update Resource Activity</span></span>](data-factory-azure-ml-update-resource-activity.md)
> * [<span data-ttu-id="f3b55-111">Lagrad proceduraktivitet</span><span class="sxs-lookup"><span data-stu-id="f3b55-111">Stored Procedure Activity</span></span>](data-factory-stored-proc-activity.md)
> * [<span data-ttu-id="f3b55-112">Data Lake Analytics U-SQL-aktivitet</span><span class="sxs-lookup"><span data-stu-id="f3b55-112">Data Lake Analytics U-SQL Activity</span></span>](data-factory-usql-activity.md)
> * [<span data-ttu-id="f3b55-113">Anpassad aktivitet för .NET</span><span class="sxs-lookup"><span data-stu-id="f3b55-113">.NET Custom Activity</span></span>](data-factory-use-custom-activities.md)

<span data-ttu-id="f3b55-114">En pipeline i ett Azure data factory bearbetar data i länkade storage-tjänster med hjälp av länkade beräknings-tjänster.</span><span class="sxs-lookup"><span data-stu-id="f3b55-114">A pipeline in an Azure data factory processes data in linked storage services by using linked compute services.</span></span> <span data-ttu-id="f3b55-115">Den innehåller en serie aktiviteter där varje aktivitet utför en specifik bearbetning.</span><span class="sxs-lookup"><span data-stu-id="f3b55-115">It contains a sequence of activities where each activity performs a specific processing operation.</span></span> <span data-ttu-id="f3b55-116">Den här artikeln beskriver hello **Data Lake Analytics U-SQL-aktivitet** som kör en **U-SQL** skript på en **Azure Data Lake Analytics** compute länkade tjänsten.</span><span class="sxs-lookup"><span data-stu-id="f3b55-116">This article describes hello **Data Lake Analytics U-SQL Activity** that runs a **U-SQL** script on an **Azure Data Lake Analytics** compute linked service.</span></span> 

> [!NOTE]
> <span data-ttu-id="f3b55-117">Skapa ett Azure Data Lake Analytics-konto innan du skapar en pipeline med en Data Lake Analytics U-SQL-aktivitet.</span><span class="sxs-lookup"><span data-stu-id="f3b55-117">Create an Azure Data Lake Analytics account before creating a pipeline with a Data Lake Analytics U-SQL Activity.</span></span> <span data-ttu-id="f3b55-118">toolearn om Azure Data Lake Analytics finns [Kom igång med Azure Data Lake Analytics](../data-lake-analytics/data-lake-analytics-get-started-portal.md).</span><span class="sxs-lookup"><span data-stu-id="f3b55-118">toolearn about Azure Data Lake Analytics, see [Get started with Azure Data Lake Analytics](../data-lake-analytics/data-lake-analytics-get-started-portal.md).</span></span>
> 
> <span data-ttu-id="f3b55-119">Granska hello [skapa din första pipeline-självstudierna](data-factory-build-your-first-pipeline.md) för detaljerade anvisningar toocreate en datafabrik länkade tjänster, datauppsättningar och en pipeline.</span><span class="sxs-lookup"><span data-stu-id="f3b55-119">Review hello [Build your first pipeline tutorial](data-factory-build-your-first-pipeline.md) for detailed steps toocreate a data factory, linked services, datasets, and a pipeline.</span></span> <span data-ttu-id="f3b55-120">Använd JSON kodavsnitt hos Data Factory-redigeraren eller Visual Studio eller Azure PowerShell toocreate Data Factory-enheter.</span><span class="sxs-lookup"><span data-stu-id="f3b55-120">Use JSON snippets with Data Factory Editor or Visual Studio or Azure PowerShell toocreate Data Factory entities.</span></span>

## <a name="supported-authentication-types"></a><span data-ttu-id="f3b55-121">Typer av autentiseringsmetoder som stöds</span><span class="sxs-lookup"><span data-stu-id="f3b55-121">Supported authentication types</span></span>
<span data-ttu-id="f3b55-122">U-SQL-aktiviteten stöder nedan autentiseringstyper mot Data Lake Analytics:</span><span class="sxs-lookup"><span data-stu-id="f3b55-122">U-SQL activity supports below authentication types against Data Lake Analytics:</span></span>
* <span data-ttu-id="f3b55-123">Autentisering av tjänstens huvudnamn</span><span class="sxs-lookup"><span data-stu-id="f3b55-123">Service principal authentication</span></span>
* <span data-ttu-id="f3b55-124">Användarautentisering autentiseringsuppgifter (OAuth)</span><span class="sxs-lookup"><span data-stu-id="f3b55-124">User credential (OAuth) authentication</span></span> 

<span data-ttu-id="f3b55-125">Vi rekommenderar att du använder service principal autentisering, särskilt för en schemalagd U-SQL-körningen.</span><span class="sxs-lookup"><span data-stu-id="f3b55-125">We recommend that you use service principal authentication, especially for a scheduled U-SQL execution.</span></span> <span data-ttu-id="f3b55-126">Giltighetstid för token kan inträffa med autentiseringsuppgifter för användarautentisering.</span><span class="sxs-lookup"><span data-stu-id="f3b55-126">Token expiration behavior can occur with user credential authentication.</span></span> <span data-ttu-id="f3b55-127">Konfigurationsinformation finns hello [länkade tjänstegenskaper](#azure-data-lake-analytics-linked-service) avsnitt.</span><span class="sxs-lookup"><span data-stu-id="f3b55-127">For configuration details, see hello [Linked service properties](#azure-data-lake-analytics-linked-service) section.</span></span>

## <a name="azure-data-lake-analytics-linked-service"></a><span data-ttu-id="f3b55-128">Azure Data Lake Analytics länkade tjänsten</span><span class="sxs-lookup"><span data-stu-id="f3b55-128">Azure Data Lake Analytics Linked Service</span></span>
<span data-ttu-id="f3b55-129">Du skapar en **Azure Data Lake Analytics** länkade tjänsten toolink ett Azure Data Lake Analytics beräkning service tooan Azure data factory.</span><span class="sxs-lookup"><span data-stu-id="f3b55-129">You create an **Azure Data Lake Analytics** linked service toolink an Azure Data Lake Analytics compute service tooan Azure data factory.</span></span> <span data-ttu-id="f3b55-130">hello Data Lake Analytics U-SQL-aktivitet i pipelinen hello refererar toothis länkade tjänsten.</span><span class="sxs-lookup"><span data-stu-id="f3b55-130">hello Data Lake Analytics U-SQL activity in hello pipeline refers toothis linked service.</span></span> 

<span data-ttu-id="f3b55-131">hello innehåller följande tabell beskrivningar hello allmänna egenskaper som används i hello JSON-definitionen.</span><span class="sxs-lookup"><span data-stu-id="f3b55-131">hello following table provides descriptions for hello generic properties used in hello JSON definition.</span></span> <span data-ttu-id="f3b55-132">Du kan ytterligare välja mellan tjänstens huvudnamn och autentiseringsuppgifter för användarautentisering.</span><span class="sxs-lookup"><span data-stu-id="f3b55-132">You can further choose between service principal and user credential authentication.</span></span>

| <span data-ttu-id="f3b55-133">Egenskap</span><span class="sxs-lookup"><span data-stu-id="f3b55-133">Property</span></span> | <span data-ttu-id="f3b55-134">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="f3b55-134">Description</span></span> | <span data-ttu-id="f3b55-135">Krävs</span><span class="sxs-lookup"><span data-stu-id="f3b55-135">Required</span></span> |
| --- | --- | --- |
| <span data-ttu-id="f3b55-136">**typ**</span><span class="sxs-lookup"><span data-stu-id="f3b55-136">**type**</span></span> |<span data-ttu-id="f3b55-137">hello typegenskapen ska anges till: **AzureDataLakeAnalytics**.</span><span class="sxs-lookup"><span data-stu-id="f3b55-137">hello type property should be set to: **AzureDataLakeAnalytics**.</span></span> |<span data-ttu-id="f3b55-138">Ja</span><span class="sxs-lookup"><span data-stu-id="f3b55-138">Yes</span></span> |
| <span data-ttu-id="f3b55-139">**Kontonamn**</span><span class="sxs-lookup"><span data-stu-id="f3b55-139">**accountName**</span></span> |<span data-ttu-id="f3b55-140">Azure Data Lake Analytics-kontonamn.</span><span class="sxs-lookup"><span data-stu-id="f3b55-140">Azure Data Lake Analytics Account Name.</span></span> |<span data-ttu-id="f3b55-141">Ja</span><span class="sxs-lookup"><span data-stu-id="f3b55-141">Yes</span></span> |
| <span data-ttu-id="f3b55-142">**dataLakeAnalyticsUri**</span><span class="sxs-lookup"><span data-stu-id="f3b55-142">**dataLakeAnalyticsUri**</span></span> |<span data-ttu-id="f3b55-143">Azure Data Lake Analytics-URI.</span><span class="sxs-lookup"><span data-stu-id="f3b55-143">Azure Data Lake Analytics URI.</span></span> |<span data-ttu-id="f3b55-144">Nej</span><span class="sxs-lookup"><span data-stu-id="f3b55-144">No</span></span> |
| <span data-ttu-id="f3b55-145">**prenumerations-ID**</span><span class="sxs-lookup"><span data-stu-id="f3b55-145">**subscriptionId**</span></span> |<span data-ttu-id="f3b55-146">Azure prenumerations-id</span><span class="sxs-lookup"><span data-stu-id="f3b55-146">Azure subscription id</span></span> |<span data-ttu-id="f3b55-147">Nej (om inte anges prenumeration hello datafabriken används).</span><span class="sxs-lookup"><span data-stu-id="f3b55-147">No (If not specified, subscription of hello data factory is used).</span></span> |
| <span data-ttu-id="f3b55-148">**resourceGroupName**</span><span class="sxs-lookup"><span data-stu-id="f3b55-148">**resourceGroupName**</span></span> |<span data-ttu-id="f3b55-149">Azure resursgruppens namn</span><span class="sxs-lookup"><span data-stu-id="f3b55-149">Azure resource group name</span></span> |<span data-ttu-id="f3b55-150">Nej (om inte anges resursgruppen av hello datafabriken används).</span><span class="sxs-lookup"><span data-stu-id="f3b55-150">No (If not specified, resource group of hello data factory is used).</span></span> |

### <a name="service-principal-authentication-recommended"></a><span data-ttu-id="f3b55-151">Tjänstens huvudnamn autentisering (rekommenderas)</span><span class="sxs-lookup"><span data-stu-id="f3b55-151">Service principal authentication (recommended)</span></span>
<span data-ttu-id="f3b55-152">toouse service principal autentisering, registrera en Programenhet i Azure Active Directory (Azure AD) och bevilja den hello komma åt tooData Lake Store.</span><span class="sxs-lookup"><span data-stu-id="f3b55-152">toouse service principal authentication, register an application entity in Azure Active Directory (Azure AD) and grant it hello access tooData Lake Store.</span></span> <span data-ttu-id="f3b55-153">Detaljerade anvisningar finns i [tjänst-till-tjänst autentisering](../data-lake-store/data-lake-store-authenticate-using-active-directory.md).</span><span class="sxs-lookup"><span data-stu-id="f3b55-153">For detailed steps, see [Service-to-service authentication](../data-lake-store/data-lake-store-authenticate-using-active-directory.md).</span></span> <span data-ttu-id="f3b55-154">Anteckna hello följande värden som du använder toodefine hello länkade tjänsten:</span><span class="sxs-lookup"><span data-stu-id="f3b55-154">Make note of hello following values, which you use toodefine hello linked service:</span></span>
* <span data-ttu-id="f3b55-155">Program-ID:t</span><span class="sxs-lookup"><span data-stu-id="f3b55-155">Application ID</span></span>
* <span data-ttu-id="f3b55-156">Nyckeln för programmet</span><span class="sxs-lookup"><span data-stu-id="f3b55-156">Application key</span></span> 
* <span data-ttu-id="f3b55-157">Klient-ID:t</span><span class="sxs-lookup"><span data-stu-id="f3b55-157">Tenant ID</span></span>

<span data-ttu-id="f3b55-158">Använd service principal autentisering genom att ange hello följande egenskaper:</span><span class="sxs-lookup"><span data-stu-id="f3b55-158">Use service principal authentication by specifying hello following properties:</span></span>

| <span data-ttu-id="f3b55-159">Egenskap</span><span class="sxs-lookup"><span data-stu-id="f3b55-159">Property</span></span> | <span data-ttu-id="f3b55-160">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="f3b55-160">Description</span></span> | <span data-ttu-id="f3b55-161">Krävs</span><span class="sxs-lookup"><span data-stu-id="f3b55-161">Required</span></span> |
|:--- |:--- |:--- |
| <span data-ttu-id="f3b55-162">**servicePrincipalId**</span><span class="sxs-lookup"><span data-stu-id="f3b55-162">**servicePrincipalId**</span></span> | <span data-ttu-id="f3b55-163">Ange hello programmets klient-ID.</span><span class="sxs-lookup"><span data-stu-id="f3b55-163">Specify hello application's client ID.</span></span> | <span data-ttu-id="f3b55-164">Ja</span><span class="sxs-lookup"><span data-stu-id="f3b55-164">Yes</span></span> |
| <span data-ttu-id="f3b55-165">**servicePrincipalKey**</span><span class="sxs-lookup"><span data-stu-id="f3b55-165">**servicePrincipalKey**</span></span> | <span data-ttu-id="f3b55-166">Ange hello programnyckel.</span><span class="sxs-lookup"><span data-stu-id="f3b55-166">Specify hello application's key.</span></span> | <span data-ttu-id="f3b55-167">Ja</span><span class="sxs-lookup"><span data-stu-id="f3b55-167">Yes</span></span> |
| <span data-ttu-id="f3b55-168">**klient**</span><span class="sxs-lookup"><span data-stu-id="f3b55-168">**tenant**</span></span> | <span data-ttu-id="f3b55-169">Ange hello klient information (domain name eller klient ID) under där programmet finns.</span><span class="sxs-lookup"><span data-stu-id="f3b55-169">Specify hello tenant information (domain name or tenant ID) under which your application resides.</span></span> <span data-ttu-id="f3b55-170">Du kan hämta den med hovra hello musen i hello övre högra hörnet av hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="f3b55-170">You can retrieve it by hovering hello mouse in hello upper-right corner of hello Azure portal.</span></span> | <span data-ttu-id="f3b55-171">Ja</span><span class="sxs-lookup"><span data-stu-id="f3b55-171">Yes</span></span> |

<span data-ttu-id="f3b55-172">**Exempel: Service principal autentisering**</span><span class="sxs-lookup"><span data-stu-id="f3b55-172">**Example: Service principal authentication**</span></span>
```json
{
    "name": "AzureDataLakeAnalyticsLinkedService",
    "properties": {
        "type": "AzureDataLakeAnalytics",
        "typeProperties": {
            "accountName": "adftestaccount",
            "dataLakeAnalyticsUri": "azuredatalakeanalytics.net",
            "servicePrincipalId": "<service principal id>",
            "servicePrincipalKey": "<service principal key>",
            "tenant": "<tenant info, e.g. microsoft.onmicrosoft.com>",
            "subscriptionId": "<optional, subscription id of ADLA>",
            "resourceGroupName": "<optional, resource group name of ADLA>"
        }
    }
}
```

### <a name="user-credential-authentication"></a><span data-ttu-id="f3b55-173">Användarautentisering för autentiseringsuppgifter</span><span class="sxs-lookup"><span data-stu-id="f3b55-173">User credential authentication</span></span>
<span data-ttu-id="f3b55-174">Alternativt kan du använda användarautentisering för autentiseringsuppgifter för Data Lake Analytics genom att ange hello följande egenskaper:</span><span class="sxs-lookup"><span data-stu-id="f3b55-174">Alternatively, you can use user credential authentication for Data Lake Analytics by specifying hello following properties:</span></span>

| <span data-ttu-id="f3b55-175">Egenskap</span><span class="sxs-lookup"><span data-stu-id="f3b55-175">Property</span></span> | <span data-ttu-id="f3b55-176">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="f3b55-176">Description</span></span> | <span data-ttu-id="f3b55-177">Krävs</span><span class="sxs-lookup"><span data-stu-id="f3b55-177">Required</span></span> |
|:--- |:--- |:--- |
| <span data-ttu-id="f3b55-178">**auktorisering**</span><span class="sxs-lookup"><span data-stu-id="f3b55-178">**authorization**</span></span> | <span data-ttu-id="f3b55-179">Klicka på hello **auktorisera** i hello Data Factory-redigeraren och ange dina autentiseringsuppgifter som tilldelar hello automatiskt genererade auktorisering URL toothis-egenskapen.</span><span class="sxs-lookup"><span data-stu-id="f3b55-179">Click hello **Authorize** button in hello Data Factory Editor and enter your credential that assigns hello autogenerated authorization URL toothis property.</span></span> | <span data-ttu-id="f3b55-180">Ja</span><span class="sxs-lookup"><span data-stu-id="f3b55-180">Yes</span></span> |
| <span data-ttu-id="f3b55-181">**sessions-ID**</span><span class="sxs-lookup"><span data-stu-id="f3b55-181">**sessionId**</span></span> | <span data-ttu-id="f3b55-182">OAuth sessions-ID från hello OAuth-auktorisering session.</span><span class="sxs-lookup"><span data-stu-id="f3b55-182">OAuth session ID from hello OAuth authorization session.</span></span> <span data-ttu-id="f3b55-183">Varje sessions-ID är unikt och kan bara användas en gång.</span><span class="sxs-lookup"><span data-stu-id="f3b55-183">Each session ID is unique and can be used only once.</span></span> <span data-ttu-id="f3b55-184">Den här inställningen genereras automatiskt när du använder hello Data Factory-redigeraren.</span><span class="sxs-lookup"><span data-stu-id="f3b55-184">This setting is automatically generated when you use hello Data Factory Editor.</span></span> | <span data-ttu-id="f3b55-185">Ja</span><span class="sxs-lookup"><span data-stu-id="f3b55-185">Yes</span></span> |

<span data-ttu-id="f3b55-186">**Exempel: Användarautentisering för autentiseringsuppgifter**</span><span class="sxs-lookup"><span data-stu-id="f3b55-186">**Example: User credential authentication**</span></span>
```json
{
    "name": "AzureDataLakeAnalyticsLinkedService",
    "properties": {
        "type": "AzureDataLakeAnalytics",
        "typeProperties": {
            "accountName": "adftestaccount",
            "dataLakeAnalyticsUri": "azuredatalakeanalytics.net",
            "authorization": "<authcode>",
            "sessionId": "<session ID>", 
            "subscriptionId": "<optional, subscription id of ADLA>",
            "resourceGroupName": "<optional, resource group name of ADLA>"
        }
    }
}
```

#### <a name="token-expiration"></a><span data-ttu-id="f3b55-187">Token upphör att gälla</span><span class="sxs-lookup"><span data-stu-id="f3b55-187">Token expiration</span></span>
<span data-ttu-id="f3b55-188">Hej auktoriseringskod som du genererade med hjälp av hello **auktorisera** knappen upphör att gälla efter en stund.</span><span class="sxs-lookup"><span data-stu-id="f3b55-188">hello authorization code you generated by using hello **Authorize** button expires after sometime.</span></span> <span data-ttu-id="f3b55-189">Se hello i den följande tabellen för hello upphör att gälla tidpunkter för olika typer av användarkonton.</span><span class="sxs-lookup"><span data-stu-id="f3b55-189">See hello following table for hello expiration times for different types of user accounts.</span></span> <span data-ttu-id="f3b55-190">Kan du se hello följande felmeddelande när hello autentisering **token upphör att gälla**: autentiseringsuppgifter fel: invalid_grant - AADSTS70002: fel vid verifiering av autentiseringsuppgifter.</span><span class="sxs-lookup"><span data-stu-id="f3b55-190">You may see hello following error message when hello authentication **token expires**: Credential operation error: invalid_grant - AADSTS70002: Error validating credentials.</span></span> <span data-ttu-id="f3b55-191">AADSTS70008: hello angetts åtkomsten har upphört att gälla eller återkallats.</span><span class="sxs-lookup"><span data-stu-id="f3b55-191">AADSTS70008: hello provided access grant is expired or revoked.</span></span> <span data-ttu-id="f3b55-192">Trace-ID: d18629e8-af88-43c5-88e3-d8419eb1fca1 Korrelations-ID: fac30a0c-6be6-4e02-8d69-a776d2ffefd7 tidsstämpel: 2015-12-15 21:09:31Z</span><span class="sxs-lookup"><span data-stu-id="f3b55-192">Trace ID: d18629e8-af88-43c5-88e3-d8419eb1fca1 Correlation ID: fac30a0c-6be6-4e02-8d69-a776d2ffefd7 Timestamp: 2015-12-15 21:09:31Z</span></span>

| <span data-ttu-id="f3b55-193">Användartyp</span><span class="sxs-lookup"><span data-stu-id="f3b55-193">User type</span></span> | <span data-ttu-id="f3b55-194">Upphör att gälla efter</span><span class="sxs-lookup"><span data-stu-id="f3b55-194">Expires after</span></span> |
|:--- |:--- |
| <span data-ttu-id="f3b55-195">Användarkonton som inte hanteras av Azure Active Directory (@hotmail.com, @live.comosv.)</span><span class="sxs-lookup"><span data-stu-id="f3b55-195">User accounts NOT managed by Azure Active Directory (@hotmail.com, @live.com, etc.)</span></span> |<span data-ttu-id="f3b55-196">12 timmar</span><span class="sxs-lookup"><span data-stu-id="f3b55-196">12 hours</span></span> |
| <span data-ttu-id="f3b55-197">Användarkonton som hanteras av Azure Active Directory (AAD)</span><span class="sxs-lookup"><span data-stu-id="f3b55-197">Users accounts managed by Azure Active Directory (AAD)</span></span> |<span data-ttu-id="f3b55-198">14 dagar efter hello sista segmentet kör.</span><span class="sxs-lookup"><span data-stu-id="f3b55-198">14 days after hello last slice run.</span></span> <br/><br/><span data-ttu-id="f3b55-199">90 dagar, om ett segment baserat på OAuth-baserad länkade tjänst körs på minst en gång var fjortonde dag.</span><span class="sxs-lookup"><span data-stu-id="f3b55-199">90 days, if a slice based on OAuth-based linked service runs at least once every 14 days.</span></span> |

<span data-ttu-id="f3b55-200">tooavoid/Lös det här felet, omauktorisera med hello **auktorisera** knappen när hello **token upphör att gälla** och distribuera hello länkade tjänsten.</span><span class="sxs-lookup"><span data-stu-id="f3b55-200">tooavoid/resolve this error, reauthorize using hello **Authorize** button when hello **token expires** and redeploy hello linked service.</span></span> <span data-ttu-id="f3b55-201">Du kan också generera värdena för **sessionId** och **auktorisering** egenskaper programmässigt med hjälp av koden på följande sätt:</span><span class="sxs-lookup"><span data-stu-id="f3b55-201">You can also generate values for **sessionId** and **authorization** properties programmatically using code as follows:</span></span>

```csharp
if (linkedService.Properties.TypeProperties is AzureDataLakeStoreLinkedService ||
    linkedService.Properties.TypeProperties is AzureDataLakeAnalyticsLinkedService)
{
    AuthorizationSessionGetResponse authorizationSession = this.Client.OAuth.Get(this.ResourceGroupName, this.DataFactoryName, linkedService.Properties.Type);

    WindowsFormsWebAuthenticationDialog authenticationDialog = new WindowsFormsWebAuthenticationDialog(null);
    string authorization = authenticationDialog.AuthenticateAAD(authorizationSession.AuthorizationSession.Endpoint, new Uri("urn:ietf:wg:oauth:2.0:oob"));

    AzureDataLakeStoreLinkedService azureDataLakeStoreProperties = linkedService.Properties.TypeProperties as AzureDataLakeStoreLinkedService;
    if (azureDataLakeStoreProperties != null)
    {
        azureDataLakeStoreProperties.SessionId = authorizationSession.AuthorizationSession.SessionId;
        azureDataLakeStoreProperties.Authorization = authorization;
    }

    AzureDataLakeAnalyticsLinkedService azureDataLakeAnalyticsProperties = linkedService.Properties.TypeProperties as AzureDataLakeAnalyticsLinkedService;
    if (azureDataLakeAnalyticsProperties != null)
    {
        azureDataLakeAnalyticsProperties.SessionId = authorizationSession.AuthorizationSession.SessionId;
        azureDataLakeAnalyticsProperties.Authorization = authorization;
    }
}
```

<span data-ttu-id="f3b55-202">Se [AzureDataLakeStoreLinkedService klassen](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.azuredatalakestorelinkedservice.aspx), [AzureDataLakeAnalyticsLinkedService klassen](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.azuredatalakeanalyticslinkedservice.aspx), och [AuthorizationSessionGetResponse klassen](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.authorizationsessiongetresponse.aspx) avsnitt hittar du information Om hello Data Factory-klasser används i hello-koden.</span><span class="sxs-lookup"><span data-stu-id="f3b55-202">See [AzureDataLakeStoreLinkedService Class](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.azuredatalakestorelinkedservice.aspx), [AzureDataLakeAnalyticsLinkedService Class](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.azuredatalakeanalyticslinkedservice.aspx), and [AuthorizationSessionGetResponse Class](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.authorizationsessiongetresponse.aspx) topics for details about hello Data Factory classes used in hello code.</span></span> <span data-ttu-id="f3b55-203">Lägg till en referens till: Microsoft.IdentityModel.Clients.ActiveDirectory.WindowsForms.dll för hello WindowsFormsWebAuthenticationDialog klass.</span><span class="sxs-lookup"><span data-stu-id="f3b55-203">Add a reference to: Microsoft.IdentityModel.Clients.ActiveDirectory.WindowsForms.dll for hello WindowsFormsWebAuthenticationDialog class.</span></span> 

## <a name="data-lake-analytics-u-sql-activity"></a><span data-ttu-id="f3b55-204">Data Lake Analytics U-SQL-aktivitet</span><span class="sxs-lookup"><span data-stu-id="f3b55-204">Data Lake Analytics U-SQL Activity</span></span>
<span data-ttu-id="f3b55-205">hello följande JSON-fragment definierar en pipeline med en Data Lake Analytics U-SQL-aktivitet.</span><span class="sxs-lookup"><span data-stu-id="f3b55-205">hello following JSON snippet defines a pipeline with a Data Lake Analytics U-SQL Activity.</span></span> <span data-ttu-id="f3b55-206">hello aktivitetsdefinitionen har en referens toohello länkade Azure Data Lake Analytics-tjänsten som du skapade tidigare.</span><span class="sxs-lookup"><span data-stu-id="f3b55-206">hello activity definition has a reference toohello Azure Data Lake Analytics linked service you created earlier.</span></span>   

```json
{
    "name": "ComputeEventsByRegionPipeline",
    "properties": {
        "description": "This is a pipeline toocompute events for en-gb locale and date less than 2012/02/19.",
        "activities": 
        [
            {
                "type": "DataLakeAnalyticsU-SQL",
                "typeProperties": {
                    "scriptPath": "scripts\\kona\\SearchLogProcessing.txt",
                    "scriptLinkedService": "StorageLinkedService",
                    "degreeOfParallelism": 3,
                    "priority": 100,
                    "parameters": {
                        "in": "/datalake/input/SearchLog.tsv",
                        "out": "/datalake/output/Result.tsv"
                    }
                },
                "inputs": [
                    {
                        "name": "DataLakeTable"
                    }
                ],
                "outputs": 
                [
                    {
                        "name": "EventsByRegionTable"
                    }
                ],
                "policy": {
                    "timeout": "06:00:00",
                    "concurrency": 1,
                    "executionPriorityOrder": "NewestFirst",
                    "retry": 1
                },
                "scheduler": {
                    "frequency": "Day",
                    "interval": 1
                },
                "name": "EventsByRegion",
                "linkedServiceName": "AzureDataLakeAnalyticsLinkedService"
            }
        ],
        "start": "2015-08-08T00:00:00Z",
        "end": "2015-08-08T01:00:00Z",
        "isPaused": false
    }
}
```

<span data-ttu-id="f3b55-207">hello följande tabell beskrivs namn och beskrivningar av egenskaper som är specifika toothis aktivitet.</span><span class="sxs-lookup"><span data-stu-id="f3b55-207">hello following table describes names and descriptions of properties that are specific toothis activity.</span></span> 

| <span data-ttu-id="f3b55-208">Egenskap</span><span class="sxs-lookup"><span data-stu-id="f3b55-208">Property</span></span> | <span data-ttu-id="f3b55-209">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="f3b55-209">Description</span></span> | <span data-ttu-id="f3b55-210">Krävs</span><span class="sxs-lookup"><span data-stu-id="f3b55-210">Required</span></span> |
|:--- |:--- |:--- |
| <span data-ttu-id="f3b55-211">typ</span><span class="sxs-lookup"><span data-stu-id="f3b55-211">type</span></span> |<span data-ttu-id="f3b55-212">hello Typegenskapen måste anges för**DataLakeAnalyticsU SQL**.</span><span class="sxs-lookup"><span data-stu-id="f3b55-212">hello type property must be set too**DataLakeAnalyticsU-SQL**.</span></span> |<span data-ttu-id="f3b55-213">Ja</span><span class="sxs-lookup"><span data-stu-id="f3b55-213">Yes</span></span> |
| <span data-ttu-id="f3b55-214">ScriptPath</span><span class="sxs-lookup"><span data-stu-id="f3b55-214">scriptPath</span></span> |<span data-ttu-id="f3b55-215">Sökvägen toofolder som innehåller hello U-SQL-skriptet.</span><span class="sxs-lookup"><span data-stu-id="f3b55-215">Path toofolder that contains hello U-SQL script.</span></span> <span data-ttu-id="f3b55-216">Namnet på hello-filen är skiftlägeskänslig.</span><span class="sxs-lookup"><span data-stu-id="f3b55-216">Name of hello file is case-sensitive.</span></span> |<span data-ttu-id="f3b55-217">Nej (om du använder skriptet)</span><span class="sxs-lookup"><span data-stu-id="f3b55-217">No (if you use script)</span></span> |
| <span data-ttu-id="f3b55-218">scriptLinkedService</span><span class="sxs-lookup"><span data-stu-id="f3b55-218">scriptLinkedService</span></span> |<span data-ttu-id="f3b55-219">Länkade tjänst som länkar hello lagring som innehåller hello skriptet toohello data factory</span><span class="sxs-lookup"><span data-stu-id="f3b55-219">Linked service that links hello storage that contains hello script toohello data factory</span></span> |<span data-ttu-id="f3b55-220">Nej (om du använder skriptet)</span><span class="sxs-lookup"><span data-stu-id="f3b55-220">No (if you use script)</span></span> |
| <span data-ttu-id="f3b55-221">Skriptet</span><span class="sxs-lookup"><span data-stu-id="f3b55-221">script</span></span> |<span data-ttu-id="f3b55-222">Ange infogat skript i stället för att ange scriptPath och scriptLinkedService.</span><span class="sxs-lookup"><span data-stu-id="f3b55-222">Specify inline script instead of specifying scriptPath and scriptLinkedService.</span></span> <span data-ttu-id="f3b55-223">Till exempel: `"script": "CREATE DATABASE test"`.</span><span class="sxs-lookup"><span data-stu-id="f3b55-223">For example: `"script": "CREATE DATABASE test"`.</span></span> |<span data-ttu-id="f3b55-224">Nej (om du använder scriptPath och scriptLinkedService)</span><span class="sxs-lookup"><span data-stu-id="f3b55-224">No (if you use scriptPath and scriptLinkedService)</span></span> |
| <span data-ttu-id="f3b55-225">degreeOfParallelism</span><span class="sxs-lookup"><span data-stu-id="f3b55-225">degreeOfParallelism</span></span> |<span data-ttu-id="f3b55-226">hello maximalt antal noder används samtidigt toorun hello jobb.</span><span class="sxs-lookup"><span data-stu-id="f3b55-226">hello maximum number of nodes simultaneously used toorun hello job.</span></span> |<span data-ttu-id="f3b55-227">Nej</span><span class="sxs-lookup"><span data-stu-id="f3b55-227">No</span></span> |
| <span data-ttu-id="f3b55-228">Prioritet</span><span class="sxs-lookup"><span data-stu-id="f3b55-228">priority</span></span> |<span data-ttu-id="f3b55-229">Anger vilka jobb av alla köas ska vara markerade toorun först.</span><span class="sxs-lookup"><span data-stu-id="f3b55-229">Determines which jobs out of all that are queued should be selected toorun first.</span></span> <span data-ttu-id="f3b55-230">hello lägre hello nummer, hello högre hello prioritet.</span><span class="sxs-lookup"><span data-stu-id="f3b55-230">hello lower hello number, hello higher hello priority.</span></span> |<span data-ttu-id="f3b55-231">Nej</span><span class="sxs-lookup"><span data-stu-id="f3b55-231">No</span></span> |
| <span data-ttu-id="f3b55-232">parameters</span><span class="sxs-lookup"><span data-stu-id="f3b55-232">parameters</span></span> |<span data-ttu-id="f3b55-233">Parametrar för hello U-SQL-skript</span><span class="sxs-lookup"><span data-stu-id="f3b55-233">Parameters for hello U-SQL script</span></span> |<span data-ttu-id="f3b55-234">Nej</span><span class="sxs-lookup"><span data-stu-id="f3b55-234">No</span></span> |
| <span data-ttu-id="f3b55-235">runtimeVersion</span><span class="sxs-lookup"><span data-stu-id="f3b55-235">runtimeVersion</span></span> | <span data-ttu-id="f3b55-236">Runtime-versionen av hello U-SQL-motorn toouse</span><span class="sxs-lookup"><span data-stu-id="f3b55-236">Runtime version of hello U-SQL engine toouse</span></span> | <span data-ttu-id="f3b55-237">Nej</span><span class="sxs-lookup"><span data-stu-id="f3b55-237">No</span></span> | 
| <span data-ttu-id="f3b55-238">compilationMode</span><span class="sxs-lookup"><span data-stu-id="f3b55-238">compilationMode</span></span> | <p><span data-ttu-id="f3b55-239">Kompileringsläge för U-SQL.</span><span class="sxs-lookup"><span data-stu-id="f3b55-239">Compilation mode of U-SQL.</span></span> <span data-ttu-id="f3b55-240">Måste vara ett av följande värden:</span><span class="sxs-lookup"><span data-stu-id="f3b55-240">Must be one of these values:</span></span></p> <ul><li><span data-ttu-id="f3b55-241">**Semantiska:** endast utföra semantiska kontroller och nödvändiga hälsokontroller.</span><span class="sxs-lookup"><span data-stu-id="f3b55-241">**Semantic:** Only perform semantic checks and necessary sanity checks.</span></span></li><li><span data-ttu-id="f3b55-242">**Fullständig:** utföra hello fullständig kompilering, inklusive syntaxkontrollen, optimering, kodgenerering osv.</span><span class="sxs-lookup"><span data-stu-id="f3b55-242">**Full:** Perform hello full compilation, including syntax check, optimization, code generation, etc.</span></span></li><li><span data-ttu-id="f3b55-243">**SingleBox:** utföra hello fullständig kompilering med TargetType inställningen tooSingleBox.</span><span class="sxs-lookup"><span data-stu-id="f3b55-243">**SingleBox:** Perform hello full compilation, with TargetType setting tooSingleBox.</span></span></li></ul><p><span data-ttu-id="f3b55-244">Om du inte anger ett värde för den här egenskapen anger hello server hello optimala kompilering.</span><span class="sxs-lookup"><span data-stu-id="f3b55-244">If you don't specify a value for this property, hello server determines hello optimal compilation mode.</span></span> </p>| <span data-ttu-id="f3b55-245">Nej</span><span class="sxs-lookup"><span data-stu-id="f3b55-245">No</span></span> | 

<span data-ttu-id="f3b55-246">Se [SearchLogProcessing.txt skript Definition](#sample-u-sql-script) hello skript definition.</span><span class="sxs-lookup"><span data-stu-id="f3b55-246">See [SearchLogProcessing.txt Script Definition](#sample-u-sql-script) for hello script definition.</span></span> 

## <a name="sample-input-and-output-datasets"></a><span data-ttu-id="f3b55-247">Exempel på indata och utdata datauppsättningar</span><span class="sxs-lookup"><span data-stu-id="f3b55-247">Sample input and output datasets</span></span>
### <a name="input-dataset"></a><span data-ttu-id="f3b55-248">Indatauppsättning</span><span class="sxs-lookup"><span data-stu-id="f3b55-248">Input dataset</span></span>
<span data-ttu-id="f3b55-249">I det här exemplet hello inkommande data som finns i ett Azure Data Lake Store (SearchLog.tsv-fil i mappen för hello datalake-indata).</span><span class="sxs-lookup"><span data-stu-id="f3b55-249">In this example, hello input data resides in an Azure Data Lake Store (SearchLog.tsv file in hello datalake/input folder).</span></span> 

```json
{
    "name": "DataLakeTable",
    "properties": {
        "type": "AzureDataLakeStore",
        "linkedServiceName": "AzureDataLakeStoreLinkedService",
        "typeProperties": {
            "folderPath": "datalake/input/",
            "fileName": "SearchLog.tsv",
            "format": {
                "type": "TextFormat",
                "rowDelimiter": "\n",
                "columnDelimiter": "\t"
            }
        },
        "availability": {
            "frequency": "Day",
            "interval": 1
        }
    }
}    
```

### <a name="output-dataset"></a><span data-ttu-id="f3b55-250">Datamängd för utdata</span><span class="sxs-lookup"><span data-stu-id="f3b55-250">Output dataset</span></span>
<span data-ttu-id="f3b55-251">I det här exemplet lagras hello-utdata som genereras av hello U-SQL-skript i ett Azure Data Lake Store (datalake-/ utdata mapp).</span><span class="sxs-lookup"><span data-stu-id="f3b55-251">In this example, hello output data produced by hello U-SQL script is stored in an Azure Data Lake Store (datalake/output folder).</span></span> 

```json
{
    "name": "EventsByRegionTable",
    "properties": {
        "type": "AzureDataLakeStore",
        "linkedServiceName": "AzureDataLakeStoreLinkedService",
        "typeProperties": {
            "folderPath": "datalake/output/"
        },
        "availability": {
            "frequency": "Day",
            "interval": 1
        }
    }
}
```

### <a name="sample-data-lake-store-linked-service"></a><span data-ttu-id="f3b55-252">Exempel Data Lake Store länkade tjänsten</span><span class="sxs-lookup"><span data-stu-id="f3b55-252">Sample Data Lake Store Linked Service</span></span>
<span data-ttu-id="f3b55-253">Här är hello definition av hello exempel Azure Data Lake Store länkade tjänst som används av hello in-/ utdata-datauppsättningar.</span><span class="sxs-lookup"><span data-stu-id="f3b55-253">Here is hello definition of hello sample Azure Data Lake Store linked service used by hello input/output datasets.</span></span> 

```json
{
    "name": "AzureDataLakeStoreLinkedService",
    "properties": {
        "type": "AzureDataLakeStore",
        "typeProperties": {
            "dataLakeUri": "https://<accountname>.azuredatalakestore.net/webhdfs/v1",
            "servicePrincipalId": "<service principal id>",
            "servicePrincipalKey": "<service principal key>",
            "tenant": "<tenant info, e.g. microsoft.onmicrosoft.com>",
        }
    }
}
```

<span data-ttu-id="f3b55-254">Se [flytta data tooand från Azure Data Lake Store](data-factory-azure-datalake-connector.md) artikel beskrivningar av JSON-egenskaper.</span><span class="sxs-lookup"><span data-stu-id="f3b55-254">See [Move data tooand from Azure Data Lake Store](data-factory-azure-datalake-connector.md) article for descriptions of JSON properties.</span></span> 

## <a name="sample-u-sql-script"></a><span data-ttu-id="f3b55-255">Exempel U-SQL-skript</span><span class="sxs-lookup"><span data-stu-id="f3b55-255">Sample U-SQL Script</span></span>

```
@searchlog =
    EXTRACT UserId          int,
            Start           DateTime,
            Region          string,
            Query           string,
            Duration        int?,
            Urls            string,
            ClickedUrls     string
    FROM @in
    USING Extractors.Tsv(nullEscape:"#NULL#");

@rs1 =
    SELECT Start, Region, Duration
    FROM @searchlog
WHERE Region == "en-gb";

@rs1 =
    SELECT Start, Region, Duration
    FROM @rs1
    WHERE Start <= DateTime.Parse("2012/02/19");

OUTPUT @rs1   
    too@out
      USING Outputters.Tsv(quoting:false, dateTimeFormat:null);
```

<span data-ttu-id="f3b55-256">Hej värden för  **@in**  och  **@out**  parametrar i hello U-SQL-skriptet skickas dynamiskt av ADF med hello-parametrar'.</span><span class="sxs-lookup"><span data-stu-id="f3b55-256">hello values for **@in** and **@out** parameters in hello U-SQL script are passed dynamically by ADF using hello ‘parameters’ section.</span></span> <span data-ttu-id="f3b55-257">Avsnittet hello-parametrar' i hello pipeline-definition.</span><span class="sxs-lookup"><span data-stu-id="f3b55-257">See hello ‘parameters’ section in hello pipeline definition.</span></span>

<span data-ttu-id="f3b55-258">Du kan ange andra egenskaper, till exempel degreeOfParallelism och prioritet samt i pipeline-definitionen för hello jobb som körs på hello Azure Data Lake Analytics-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="f3b55-258">You can specify other properties such as degreeOfParallelism and priority as well in your pipeline definition for hello jobs that run on hello Azure Data Lake Analytics service.</span></span>

## <a name="dynamic-parameters"></a><span data-ttu-id="f3b55-259">Dynamiska parametrar</span><span class="sxs-lookup"><span data-stu-id="f3b55-259">Dynamic parameters</span></span>
<span data-ttu-id="f3b55-260">I hello exempel pipeline definition tilldelas in och ut parametrar med hårdkodade värden.</span><span class="sxs-lookup"><span data-stu-id="f3b55-260">In hello sample pipeline definition, in and out parameters are assigned with hard-coded values.</span></span> 

```json
"parameters": {
    "in": "/datalake/input/SearchLog.tsv",
    "out": "/datalake/output/Result.tsv"
}
```

<span data-ttu-id="f3b55-261">Det är möjligt toouse dynamiska parametrar i stället.</span><span class="sxs-lookup"><span data-stu-id="f3b55-261">It is possible toouse dynamic parameters instead.</span></span> <span data-ttu-id="f3b55-262">Exempel:</span><span class="sxs-lookup"><span data-stu-id="f3b55-262">For example:</span></span> 

```json
"parameters": {
    "in": "$$Text.Format('/datalake/input/{0:yyyy-MM-dd HH:mm:ss}.tsv', SliceStart)",
    "out": "$$Text.Format('/datalake/output/{0:yyyy-MM-dd HH:mm:ss}.tsv', SliceStart)"
}
```

<span data-ttu-id="f3b55-263">I det här fallet indatafiler fortfarande tas upp från hello /datalake/input mapp och utdatafiler skapas i hello /datalake/output mapp.</span><span class="sxs-lookup"><span data-stu-id="f3b55-263">In this case, input files are still picked up from hello /datalake/input folder and output files are generated in hello /datalake/output folder.</span></span> <span data-ttu-id="f3b55-264">hello filnamn är dynamiska baserat på hello sektorn starttid.</span><span class="sxs-lookup"><span data-stu-id="f3b55-264">hello file names are dynamic based on hello slice start time.</span></span>  

