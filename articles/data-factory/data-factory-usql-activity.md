---
title: "Transformera data med hjälp av U-SQL - skript i Azure | Microsoft Docs"
description: "Lär dig hur du bearbetar eller Transformera data genom att köra U-SQL-skript på Azure Data Lake Analytics-tjänsten för beräkning."
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
ms.openlocfilehash: 49a809af92ed1bc6664fbdd3bf1aabf36afb8180
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/18/2017
---
# <a name="transform-data-by-running-u-sql-scripts-on-azure-data-lake-analytics"></a><span data-ttu-id="11d40-103">Transformera data genom att köra U-SQL-skript på Azure Data Lake Analytics</span><span class="sxs-lookup"><span data-stu-id="11d40-103">Transform data by running U-SQL scripts on Azure Data Lake Analytics</span></span> 
> [!div class="op_single_selector" title1="Transformation Activities"]
> * [<span data-ttu-id="11d40-104">Hive-aktivitet</span><span class="sxs-lookup"><span data-stu-id="11d40-104">Hive Activity</span></span>](data-factory-hive-activity.md) 
> * [<span data-ttu-id="11d40-105">Pig-aktivitet</span><span class="sxs-lookup"><span data-stu-id="11d40-105">Pig Activity</span></span>](data-factory-pig-activity.md)
> * [<span data-ttu-id="11d40-106">MapReduce Activity</span><span class="sxs-lookup"><span data-stu-id="11d40-106">MapReduce Activity</span></span>](data-factory-map-reduce.md)
> * [<span data-ttu-id="11d40-107">Hadoop Streaming Activity</span><span class="sxs-lookup"><span data-stu-id="11d40-107">Hadoop Streaming Activity</span></span>](data-factory-hadoop-streaming-activity.md)
> * [<span data-ttu-id="11d40-108">Spark-aktivitet</span><span class="sxs-lookup"><span data-stu-id="11d40-108">Spark Activity</span></span>](data-factory-spark.md)
> * [<span data-ttu-id="11d40-109">Machine Learning Batch-körningsaktivitet</span><span class="sxs-lookup"><span data-stu-id="11d40-109">Machine Learning Batch Execution Activity</span></span>](data-factory-azure-ml-batch-execution-activity.md)
> * [<span data-ttu-id="11d40-110">Machine Learning-uppdateringsresursaktivitet</span><span class="sxs-lookup"><span data-stu-id="11d40-110">Machine Learning Update Resource Activity</span></span>](data-factory-azure-ml-update-resource-activity.md)
> * [<span data-ttu-id="11d40-111">Lagrad proceduraktivitet</span><span class="sxs-lookup"><span data-stu-id="11d40-111">Stored Procedure Activity</span></span>](data-factory-stored-proc-activity.md)
> * [<span data-ttu-id="11d40-112">Data Lake Analytics U-SQL-aktivitet</span><span class="sxs-lookup"><span data-stu-id="11d40-112">Data Lake Analytics U-SQL Activity</span></span>](data-factory-usql-activity.md)
> * [<span data-ttu-id="11d40-113">Anpassad aktivitet för .NET</span><span class="sxs-lookup"><span data-stu-id="11d40-113">.NET Custom Activity</span></span>](data-factory-use-custom-activities.md)

<span data-ttu-id="11d40-114">En pipeline i ett Azure data factory bearbetar data i länkade storage-tjänster med hjälp av länkade beräknings-tjänster.</span><span class="sxs-lookup"><span data-stu-id="11d40-114">A pipeline in an Azure data factory processes data in linked storage services by using linked compute services.</span></span> <span data-ttu-id="11d40-115">Den innehåller en serie aktiviteter där varje aktivitet utför en specifik bearbetning.</span><span class="sxs-lookup"><span data-stu-id="11d40-115">It contains a sequence of activities where each activity performs a specific processing operation.</span></span> <span data-ttu-id="11d40-116">Den här artikeln beskriver den **Data Lake Analytics U-SQL-aktivitet** som kör en **U-SQL** skript på en **Azure Data Lake Analytics** compute länkade tjänsten.</span><span class="sxs-lookup"><span data-stu-id="11d40-116">This article describes the **Data Lake Analytics U-SQL Activity** that runs a **U-SQL** script on an **Azure Data Lake Analytics** compute linked service.</span></span> 

> [!NOTE]
> <span data-ttu-id="11d40-117">Skapa ett Azure Data Lake Analytics-konto innan du skapar en pipeline med en Data Lake Analytics U-SQL-aktivitet.</span><span class="sxs-lookup"><span data-stu-id="11d40-117">Create an Azure Data Lake Analytics account before creating a pipeline with a Data Lake Analytics U-SQL Activity.</span></span> <span data-ttu-id="11d40-118">Mer information om Azure Data Lake Analytics, se [Kom igång med Azure Data Lake Analytics](../data-lake-analytics/data-lake-analytics-get-started-portal.md).</span><span class="sxs-lookup"><span data-stu-id="11d40-118">To learn about Azure Data Lake Analytics, see [Get started with Azure Data Lake Analytics](../data-lake-analytics/data-lake-analytics-get-started-portal.md).</span></span>
> 
> <span data-ttu-id="11d40-119">Granska de [skapa din första pipeline-självstudierna](data-factory-build-your-first-pipeline.md) detaljerade steg för att skapa en datafabrik länkade tjänster, datauppsättningar och en pipeline.</span><span class="sxs-lookup"><span data-stu-id="11d40-119">Review the [Build your first pipeline tutorial](data-factory-build-your-first-pipeline.md) for detailed steps to create a data factory, linked services, datasets, and a pipeline.</span></span> <span data-ttu-id="11d40-120">Använda JSON kodavsnitt med Data Factory-redigeraren eller Visual Studio eller Azure PowerShell för att skapa Data Factory entiteter.</span><span class="sxs-lookup"><span data-stu-id="11d40-120">Use JSON snippets with Data Factory Editor or Visual Studio or Azure PowerShell to create Data Factory entities.</span></span>

## <a name="supported-authentication-types"></a><span data-ttu-id="11d40-121">Typer av autentiseringsmetoder som stöds</span><span class="sxs-lookup"><span data-stu-id="11d40-121">Supported authentication types</span></span>
<span data-ttu-id="11d40-122">U-SQL-aktiviteten stöder nedan autentiseringstyper mot Data Lake Analytics:</span><span class="sxs-lookup"><span data-stu-id="11d40-122">U-SQL activity supports below authentication types against Data Lake Analytics:</span></span>
* <span data-ttu-id="11d40-123">Autentisering av tjänstens huvudnamn</span><span class="sxs-lookup"><span data-stu-id="11d40-123">Service principal authentication</span></span>
* <span data-ttu-id="11d40-124">Användarautentisering autentiseringsuppgifter (OAuth)</span><span class="sxs-lookup"><span data-stu-id="11d40-124">User credential (OAuth) authentication</span></span> 

<span data-ttu-id="11d40-125">Vi rekommenderar att du använder service principal autentisering, särskilt för en schemalagd U-SQL-körningen.</span><span class="sxs-lookup"><span data-stu-id="11d40-125">We recommend that you use service principal authentication, especially for a scheduled U-SQL execution.</span></span> <span data-ttu-id="11d40-126">Giltighetstid för token kan inträffa med autentiseringsuppgifter för användarautentisering.</span><span class="sxs-lookup"><span data-stu-id="11d40-126">Token expiration behavior can occur with user credential authentication.</span></span> <span data-ttu-id="11d40-127">Konfigurationsinformation finns i [länkade tjänstegenskaper](#azure-data-lake-analytics-linked-service) avsnitt.</span><span class="sxs-lookup"><span data-stu-id="11d40-127">For configuration details, see the [Linked service properties](#azure-data-lake-analytics-linked-service) section.</span></span>

## <a name="azure-data-lake-analytics-linked-service"></a><span data-ttu-id="11d40-128">Azure Data Lake Analytics länkade tjänsten</span><span class="sxs-lookup"><span data-stu-id="11d40-128">Azure Data Lake Analytics Linked Service</span></span>
<span data-ttu-id="11d40-129">Du skapar en **Azure Data Lake Analytics** länkad tjänst för att länka ett Azure Data Lake Analytics compute-tjänst till ett Azure data factory.</span><span class="sxs-lookup"><span data-stu-id="11d40-129">You create an **Azure Data Lake Analytics** linked service to link an Azure Data Lake Analytics compute service to an Azure data factory.</span></span> <span data-ttu-id="11d40-130">Data Lake Analytics U-SQL-aktivitet i pipelinen refererar till den här länkade tjänsten.</span><span class="sxs-lookup"><span data-stu-id="11d40-130">The Data Lake Analytics U-SQL activity in the pipeline refers to this linked service.</span></span> 

<span data-ttu-id="11d40-131">Följande tabell innehåller beskrivningar för allmänna egenskaper som används i JSON-definitionen.</span><span class="sxs-lookup"><span data-stu-id="11d40-131">The following table provides descriptions for the generic properties used in the JSON definition.</span></span> <span data-ttu-id="11d40-132">Du kan ytterligare välja mellan tjänstens huvudnamn och autentiseringsuppgifter för användarautentisering.</span><span class="sxs-lookup"><span data-stu-id="11d40-132">You can further choose between service principal and user credential authentication.</span></span>

| <span data-ttu-id="11d40-133">Egenskap</span><span class="sxs-lookup"><span data-stu-id="11d40-133">Property</span></span> | <span data-ttu-id="11d40-134">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="11d40-134">Description</span></span> | <span data-ttu-id="11d40-135">Krävs</span><span class="sxs-lookup"><span data-stu-id="11d40-135">Required</span></span> |
| --- | --- | --- |
| <span data-ttu-id="11d40-136">**typ**</span><span class="sxs-lookup"><span data-stu-id="11d40-136">**type**</span></span> |<span data-ttu-id="11d40-137">Typegenskapen bör anges till: **AzureDataLakeAnalytics**.</span><span class="sxs-lookup"><span data-stu-id="11d40-137">The type property should be set to: **AzureDataLakeAnalytics**.</span></span> |<span data-ttu-id="11d40-138">Ja</span><span class="sxs-lookup"><span data-stu-id="11d40-138">Yes</span></span> |
| <span data-ttu-id="11d40-139">**Kontonamn**</span><span class="sxs-lookup"><span data-stu-id="11d40-139">**accountName**</span></span> |<span data-ttu-id="11d40-140">Azure Data Lake Analytics-kontonamn.</span><span class="sxs-lookup"><span data-stu-id="11d40-140">Azure Data Lake Analytics Account Name.</span></span> |<span data-ttu-id="11d40-141">Ja</span><span class="sxs-lookup"><span data-stu-id="11d40-141">Yes</span></span> |
| <span data-ttu-id="11d40-142">**dataLakeAnalyticsUri**</span><span class="sxs-lookup"><span data-stu-id="11d40-142">**dataLakeAnalyticsUri**</span></span> |<span data-ttu-id="11d40-143">Azure Data Lake Analytics-URI.</span><span class="sxs-lookup"><span data-stu-id="11d40-143">Azure Data Lake Analytics URI.</span></span> |<span data-ttu-id="11d40-144">Nej</span><span class="sxs-lookup"><span data-stu-id="11d40-144">No</span></span> |
| <span data-ttu-id="11d40-145">**prenumerations-ID**</span><span class="sxs-lookup"><span data-stu-id="11d40-145">**subscriptionId**</span></span> |<span data-ttu-id="11d40-146">Azure prenumerations-id</span><span class="sxs-lookup"><span data-stu-id="11d40-146">Azure subscription id</span></span> |<span data-ttu-id="11d40-147">Nej (om den inte anges data factory-prenumeration används).</span><span class="sxs-lookup"><span data-stu-id="11d40-147">No (If not specified, subscription of the data factory is used).</span></span> |
| <span data-ttu-id="11d40-148">**resourceGroupName**</span><span class="sxs-lookup"><span data-stu-id="11d40-148">**resourceGroupName**</span></span> |<span data-ttu-id="11d40-149">Azure resursgruppens namn</span><span class="sxs-lookup"><span data-stu-id="11d40-149">Azure resource group name</span></span> |<span data-ttu-id="11d40-150">Nej (om inget annat anges, resursgruppen av datafabriken används).</span><span class="sxs-lookup"><span data-stu-id="11d40-150">No (If not specified, resource group of the data factory is used).</span></span> |

### <a name="service-principal-authentication-recommended"></a><span data-ttu-id="11d40-151">Tjänstens huvudnamn autentisering (rekommenderas)</span><span class="sxs-lookup"><span data-stu-id="11d40-151">Service principal authentication (recommended)</span></span>
<span data-ttu-id="11d40-152">Om du vill använda huvudnamn autentiseringen av tjänsten registrera en entitet för program i Azure Active Directory (AD Azure) och bevilja åtkomst till Data Lake Store.</span><span class="sxs-lookup"><span data-stu-id="11d40-152">To use service principal authentication, register an application entity in Azure Active Directory (Azure AD) and grant it the access to Data Lake Store.</span></span> <span data-ttu-id="11d40-153">Detaljerade anvisningar finns i [tjänst-till-tjänst autentisering](../data-lake-store/data-lake-store-authenticate-using-active-directory.md).</span><span class="sxs-lookup"><span data-stu-id="11d40-153">For detailed steps, see [Service-to-service authentication](../data-lake-store/data-lake-store-authenticate-using-active-directory.md).</span></span> <span data-ttu-id="11d40-154">Anteckna följande värden som du använder för att definiera den länkade tjänsten:</span><span class="sxs-lookup"><span data-stu-id="11d40-154">Make note of the following values, which you use to define the linked service:</span></span>
* <span data-ttu-id="11d40-155">Program-ID:t</span><span class="sxs-lookup"><span data-stu-id="11d40-155">Application ID</span></span>
* <span data-ttu-id="11d40-156">Nyckeln för programmet</span><span class="sxs-lookup"><span data-stu-id="11d40-156">Application key</span></span> 
* <span data-ttu-id="11d40-157">Klient-ID:t</span><span class="sxs-lookup"><span data-stu-id="11d40-157">Tenant ID</span></span>

<span data-ttu-id="11d40-158">Använd service principal autentisering genom att ange följande egenskaper:</span><span class="sxs-lookup"><span data-stu-id="11d40-158">Use service principal authentication by specifying the following properties:</span></span>

| <span data-ttu-id="11d40-159">Egenskap</span><span class="sxs-lookup"><span data-stu-id="11d40-159">Property</span></span> | <span data-ttu-id="11d40-160">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="11d40-160">Description</span></span> | <span data-ttu-id="11d40-161">Krävs</span><span class="sxs-lookup"><span data-stu-id="11d40-161">Required</span></span> |
|:--- |:--- |:--- |
| <span data-ttu-id="11d40-162">**servicePrincipalId**</span><span class="sxs-lookup"><span data-stu-id="11d40-162">**servicePrincipalId**</span></span> | <span data-ttu-id="11d40-163">Ange programmets klient-ID.</span><span class="sxs-lookup"><span data-stu-id="11d40-163">Specify the application's client ID.</span></span> | <span data-ttu-id="11d40-164">Ja</span><span class="sxs-lookup"><span data-stu-id="11d40-164">Yes</span></span> |
| <span data-ttu-id="11d40-165">**servicePrincipalKey**</span><span class="sxs-lookup"><span data-stu-id="11d40-165">**servicePrincipalKey**</span></span> | <span data-ttu-id="11d40-166">Ange programmets nyckeln.</span><span class="sxs-lookup"><span data-stu-id="11d40-166">Specify the application's key.</span></span> | <span data-ttu-id="11d40-167">Ja</span><span class="sxs-lookup"><span data-stu-id="11d40-167">Yes</span></span> |
| <span data-ttu-id="11d40-168">**klient**</span><span class="sxs-lookup"><span data-stu-id="11d40-168">**tenant**</span></span> | <span data-ttu-id="11d40-169">Ange information om klient (domain name eller klient ID) under där programmet finns.</span><span class="sxs-lookup"><span data-stu-id="11d40-169">Specify the tenant information (domain name or tenant ID) under which your application resides.</span></span> <span data-ttu-id="11d40-170">Du kan hämta den hovrar muspekaren i det övre högra hörnet i Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="11d40-170">You can retrieve it by hovering the mouse in the upper-right corner of the Azure portal.</span></span> | <span data-ttu-id="11d40-171">Ja</span><span class="sxs-lookup"><span data-stu-id="11d40-171">Yes</span></span> |

<span data-ttu-id="11d40-172">**Exempel: Service principal autentisering**</span><span class="sxs-lookup"><span data-stu-id="11d40-172">**Example: Service principal authentication**</span></span>
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

### <a name="user-credential-authentication"></a><span data-ttu-id="11d40-173">Användarautentisering för autentiseringsuppgifter</span><span class="sxs-lookup"><span data-stu-id="11d40-173">User credential authentication</span></span>
<span data-ttu-id="11d40-174">Alternativt kan du använda användarautentisering för autentiseringsuppgifter för Data Lake Analytics genom att ange följande egenskaper:</span><span class="sxs-lookup"><span data-stu-id="11d40-174">Alternatively, you can use user credential authentication for Data Lake Analytics by specifying the following properties:</span></span>

| <span data-ttu-id="11d40-175">Egenskap</span><span class="sxs-lookup"><span data-stu-id="11d40-175">Property</span></span> | <span data-ttu-id="11d40-176">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="11d40-176">Description</span></span> | <span data-ttu-id="11d40-177">Krävs</span><span class="sxs-lookup"><span data-stu-id="11d40-177">Required</span></span> |
|:--- |:--- |:--- |
| <span data-ttu-id="11d40-178">**auktorisering**</span><span class="sxs-lookup"><span data-stu-id="11d40-178">**authorization**</span></span> | <span data-ttu-id="11d40-179">Klicka på den **auktorisera** i den Data Factory-redigeraren och ange dina autentiseringsuppgifter som tilldelar automatiskt genererade auktorisering URL till den här egenskapen.</span><span class="sxs-lookup"><span data-stu-id="11d40-179">Click the **Authorize** button in the Data Factory Editor and enter your credential that assigns the autogenerated authorization URL to this property.</span></span> | <span data-ttu-id="11d40-180">Ja</span><span class="sxs-lookup"><span data-stu-id="11d40-180">Yes</span></span> |
| <span data-ttu-id="11d40-181">**sessions-ID**</span><span class="sxs-lookup"><span data-stu-id="11d40-181">**sessionId**</span></span> | <span data-ttu-id="11d40-182">OAuth sessions-ID från OAuth-auktorisering sessionen.</span><span class="sxs-lookup"><span data-stu-id="11d40-182">OAuth session ID from the OAuth authorization session.</span></span> <span data-ttu-id="11d40-183">Varje sessions-ID är unikt och kan bara användas en gång.</span><span class="sxs-lookup"><span data-stu-id="11d40-183">Each session ID is unique and can be used only once.</span></span> <span data-ttu-id="11d40-184">Den här inställningen genereras automatiskt när du använder Data Factory-redigeraren.</span><span class="sxs-lookup"><span data-stu-id="11d40-184">This setting is automatically generated when you use the Data Factory Editor.</span></span> | <span data-ttu-id="11d40-185">Ja</span><span class="sxs-lookup"><span data-stu-id="11d40-185">Yes</span></span> |

<span data-ttu-id="11d40-186">**Exempel: Användarautentisering för autentiseringsuppgifter**</span><span class="sxs-lookup"><span data-stu-id="11d40-186">**Example: User credential authentication**</span></span>
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

#### <a name="token-expiration"></a><span data-ttu-id="11d40-187">Token upphör att gälla</span><span class="sxs-lookup"><span data-stu-id="11d40-187">Token expiration</span></span>
<span data-ttu-id="11d40-188">Auktoriseringskoden du genererade med hjälp av den **auktorisera** knappen upphör att gälla efter en stund.</span><span class="sxs-lookup"><span data-stu-id="11d40-188">The authorization code you generated by using the **Authorize** button expires after sometime.</span></span> <span data-ttu-id="11d40-189">Förfallodatum tidpunkter för olika typer av användarkonton finns i följande tabell.</span><span class="sxs-lookup"><span data-stu-id="11d40-189">See the following table for the expiration times for different types of user accounts.</span></span> <span data-ttu-id="11d40-190">Du kan se följande fel visas vid autentiseringen **token upphör att gälla**: autentiseringsuppgifter fel: invalid_grant - AADSTS70002: fel vid verifiering av autentiseringsuppgifter.</span><span class="sxs-lookup"><span data-stu-id="11d40-190">You may see the following error message when the authentication **token expires**: Credential operation error: invalid_grant - AADSTS70002: Error validating credentials.</span></span> <span data-ttu-id="11d40-191">AADSTS70008: Bevilja beviljade åtkomsten har upphört att gälla eller återkallats.</span><span class="sxs-lookup"><span data-stu-id="11d40-191">AADSTS70008: The provided access grant is expired or revoked.</span></span> <span data-ttu-id="11d40-192">Trace-ID: d18629e8-af88-43c5-88e3-d8419eb1fca1 Korrelations-ID: fac30a0c-6be6-4e02-8d69-a776d2ffefd7 tidsstämpel: 2015-12-15 21:09:31Z</span><span class="sxs-lookup"><span data-stu-id="11d40-192">Trace ID: d18629e8-af88-43c5-88e3-d8419eb1fca1 Correlation ID: fac30a0c-6be6-4e02-8d69-a776d2ffefd7 Timestamp: 2015-12-15 21:09:31Z</span></span>

| <span data-ttu-id="11d40-193">Användartyp</span><span class="sxs-lookup"><span data-stu-id="11d40-193">User type</span></span> | <span data-ttu-id="11d40-194">Upphör att gälla efter</span><span class="sxs-lookup"><span data-stu-id="11d40-194">Expires after</span></span> |
|:--- |:--- |
| <span data-ttu-id="11d40-195">Användarkonton som inte hanteras av Azure Active Directory (@hotmail.com, @live.comosv.)</span><span class="sxs-lookup"><span data-stu-id="11d40-195">User accounts NOT managed by Azure Active Directory (@hotmail.com, @live.com, etc.)</span></span> |<span data-ttu-id="11d40-196">12 timmar</span><span class="sxs-lookup"><span data-stu-id="11d40-196">12 hours</span></span> |
| <span data-ttu-id="11d40-197">Användarkonton som hanteras av Azure Active Directory (AAD)</span><span class="sxs-lookup"><span data-stu-id="11d40-197">Users accounts managed by Azure Active Directory (AAD)</span></span> |<span data-ttu-id="11d40-198">14 dagar efter det sista segmentet kör.</span><span class="sxs-lookup"><span data-stu-id="11d40-198">14 days after the last slice run.</span></span> <br/><br/><span data-ttu-id="11d40-199">90 dagar, om ett segment baserat på OAuth-baserad länkade tjänst körs på minst en gång var fjortonde dag.</span><span class="sxs-lookup"><span data-stu-id="11d40-199">90 days, if a slice based on OAuth-based linked service runs at least once every 14 days.</span></span> |

<span data-ttu-id="11d40-200">Om du vill undvika/åtgärda felet, omauktorisera med hjälp av den **auktorisera** knappen när den **token upphör att gälla** och distribuera den länkade tjänsten.</span><span class="sxs-lookup"><span data-stu-id="11d40-200">To avoid/resolve this error, reauthorize using the **Authorize** button when the **token expires** and redeploy the linked service.</span></span> <span data-ttu-id="11d40-201">Du kan också generera värdena för **sessionId** och **auktorisering** egenskaper programmässigt med hjälp av koden på följande sätt:</span><span class="sxs-lookup"><span data-stu-id="11d40-201">You can also generate values for **sessionId** and **authorization** properties programmatically using code as follows:</span></span>

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

<span data-ttu-id="11d40-202">Se [AzureDataLakeStoreLinkedService klassen](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.azuredatalakestorelinkedservice.aspx), [AzureDataLakeAnalyticsLinkedService klassen](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.azuredatalakeanalyticslinkedservice.aspx), och [AuthorizationSessionGetResponse klassen](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.authorizationsessiongetresponse.aspx) avsnitt hittar du information om Data Factory-klasser som används i koden.</span><span class="sxs-lookup"><span data-stu-id="11d40-202">See [AzureDataLakeStoreLinkedService Class](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.azuredatalakestorelinkedservice.aspx), [AzureDataLakeAnalyticsLinkedService Class](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.azuredatalakeanalyticslinkedservice.aspx), and [AuthorizationSessionGetResponse Class](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.authorizationsessiongetresponse.aspx) topics for details about the Data Factory classes used in the code.</span></span> <span data-ttu-id="11d40-203">Lägg till en referens till: Microsoft.IdentityModel.Clients.ActiveDirectory.WindowsForms.dll för WindowsFormsWebAuthenticationDialog-klassen.</span><span class="sxs-lookup"><span data-stu-id="11d40-203">Add a reference to: Microsoft.IdentityModel.Clients.ActiveDirectory.WindowsForms.dll for the WindowsFormsWebAuthenticationDialog class.</span></span> 

## <a name="data-lake-analytics-u-sql-activity"></a><span data-ttu-id="11d40-204">Data Lake Analytics U-SQL-aktivitet</span><span class="sxs-lookup"><span data-stu-id="11d40-204">Data Lake Analytics U-SQL Activity</span></span>
<span data-ttu-id="11d40-205">Följande JSON-fragment definierar en pipeline med en Data Lake Analytics U-SQL-aktivitet.</span><span class="sxs-lookup"><span data-stu-id="11d40-205">The following JSON snippet defines a pipeline with a Data Lake Analytics U-SQL Activity.</span></span> <span data-ttu-id="11d40-206">Aktivitetsdefinitionen har en referens till länkade Azure Data Lake Analytics-tjänsten som du skapade tidigare.</span><span class="sxs-lookup"><span data-stu-id="11d40-206">The activity definition has a reference to the Azure Data Lake Analytics linked service you created earlier.</span></span>   

```json
{
    "name": "ComputeEventsByRegionPipeline",
    "properties": {
        "description": "This is a pipeline to compute events for en-gb locale and date less than 2012/02/19.",
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

<span data-ttu-id="11d40-207">I följande tabell beskrivs namn och beskrivningar av egenskaper som är specifika för den här aktiviteten.</span><span class="sxs-lookup"><span data-stu-id="11d40-207">The following table describes names and descriptions of properties that are specific to this activity.</span></span> 

| <span data-ttu-id="11d40-208">Egenskap</span><span class="sxs-lookup"><span data-stu-id="11d40-208">Property</span></span> | <span data-ttu-id="11d40-209">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="11d40-209">Description</span></span> | <span data-ttu-id="11d40-210">Krävs</span><span class="sxs-lookup"><span data-stu-id="11d40-210">Required</span></span> |
|:--- |:--- |:--- |
| <span data-ttu-id="11d40-211">typ</span><span class="sxs-lookup"><span data-stu-id="11d40-211">type</span></span> |<span data-ttu-id="11d40-212">Egenskapen type måste anges till **DataLakeAnalyticsU SQL**.</span><span class="sxs-lookup"><span data-stu-id="11d40-212">The type property must be set to **DataLakeAnalyticsU-SQL**.</span></span> |<span data-ttu-id="11d40-213">Ja</span><span class="sxs-lookup"><span data-stu-id="11d40-213">Yes</span></span> |
| <span data-ttu-id="11d40-214">ScriptPath</span><span class="sxs-lookup"><span data-stu-id="11d40-214">scriptPath</span></span> |<span data-ttu-id="11d40-215">Sökvägen till mappen som innehåller U-SQL-skript.</span><span class="sxs-lookup"><span data-stu-id="11d40-215">Path to folder that contains the U-SQL script.</span></span> <span data-ttu-id="11d40-216">Namnet på filen är skiftlägeskänslig.</span><span class="sxs-lookup"><span data-stu-id="11d40-216">Name of the file is case-sensitive.</span></span> |<span data-ttu-id="11d40-217">Nej (om du använder skriptet)</span><span class="sxs-lookup"><span data-stu-id="11d40-217">No (if you use script)</span></span> |
| <span data-ttu-id="11d40-218">scriptLinkedService</span><span class="sxs-lookup"><span data-stu-id="11d40-218">scriptLinkedService</span></span> |<span data-ttu-id="11d40-219">Länkade tjänst som länkar den lagring som innehåller skriptet till data factory</span><span class="sxs-lookup"><span data-stu-id="11d40-219">Linked service that links the storage that contains the script to the data factory</span></span> |<span data-ttu-id="11d40-220">Nej (om du använder skriptet)</span><span class="sxs-lookup"><span data-stu-id="11d40-220">No (if you use script)</span></span> |
| <span data-ttu-id="11d40-221">Skriptet</span><span class="sxs-lookup"><span data-stu-id="11d40-221">script</span></span> |<span data-ttu-id="11d40-222">Ange infogat skript i stället för att ange scriptPath och scriptLinkedService.</span><span class="sxs-lookup"><span data-stu-id="11d40-222">Specify inline script instead of specifying scriptPath and scriptLinkedService.</span></span> <span data-ttu-id="11d40-223">Till exempel: `"script": "CREATE DATABASE test"`.</span><span class="sxs-lookup"><span data-stu-id="11d40-223">For example: `"script": "CREATE DATABASE test"`.</span></span> |<span data-ttu-id="11d40-224">Nej (om du använder scriptPath och scriptLinkedService)</span><span class="sxs-lookup"><span data-stu-id="11d40-224">No (if you use scriptPath and scriptLinkedService)</span></span> |
| <span data-ttu-id="11d40-225">degreeOfParallelism</span><span class="sxs-lookup"><span data-stu-id="11d40-225">degreeOfParallelism</span></span> |<span data-ttu-id="11d40-226">Maximalt antal noder samtidigt används för att köra jobbet.</span><span class="sxs-lookup"><span data-stu-id="11d40-226">The maximum number of nodes simultaneously used to run the job.</span></span> |<span data-ttu-id="11d40-227">Nej</span><span class="sxs-lookup"><span data-stu-id="11d40-227">No</span></span> |
| <span data-ttu-id="11d40-228">Prioritet</span><span class="sxs-lookup"><span data-stu-id="11d40-228">priority</span></span> |<span data-ttu-id="11d40-229">Anger vilka jobb av alla köas ska väljas att köras först.</span><span class="sxs-lookup"><span data-stu-id="11d40-229">Determines which jobs out of all that are queued should be selected to run first.</span></span> <span data-ttu-id="11d40-230">Ju lägre nummer, desto högre prioritet.</span><span class="sxs-lookup"><span data-stu-id="11d40-230">The lower the number, the higher the priority.</span></span> |<span data-ttu-id="11d40-231">Nej</span><span class="sxs-lookup"><span data-stu-id="11d40-231">No</span></span> |
| <span data-ttu-id="11d40-232">Parametrar</span><span class="sxs-lookup"><span data-stu-id="11d40-232">parameters</span></span> |<span data-ttu-id="11d40-233">Parametrar för U-SQL-skript</span><span class="sxs-lookup"><span data-stu-id="11d40-233">Parameters for the U-SQL script</span></span> |<span data-ttu-id="11d40-234">Nej</span><span class="sxs-lookup"><span data-stu-id="11d40-234">No</span></span> |
| <span data-ttu-id="11d40-235">runtimeVersion</span><span class="sxs-lookup"><span data-stu-id="11d40-235">runtimeVersion</span></span> | <span data-ttu-id="11d40-236">Körtidsversion U-SQL-motorn att använda</span><span class="sxs-lookup"><span data-stu-id="11d40-236">Runtime version of the U-SQL engine to use</span></span> | <span data-ttu-id="11d40-237">Nej</span><span class="sxs-lookup"><span data-stu-id="11d40-237">No</span></span> | 
| <span data-ttu-id="11d40-238">compilationMode</span><span class="sxs-lookup"><span data-stu-id="11d40-238">compilationMode</span></span> | <p><span data-ttu-id="11d40-239">Kompileringsläge för U-SQL.</span><span class="sxs-lookup"><span data-stu-id="11d40-239">Compilation mode of U-SQL.</span></span> <span data-ttu-id="11d40-240">Måste vara ett av följande värden:</span><span class="sxs-lookup"><span data-stu-id="11d40-240">Must be one of these values:</span></span></p> <ul><li><span data-ttu-id="11d40-241">**Semantiska:** endast utföra semantiska kontroller och nödvändiga hälsokontroller.</span><span class="sxs-lookup"><span data-stu-id="11d40-241">**Semantic:** Only perform semantic checks and necessary sanity checks.</span></span></li><li><span data-ttu-id="11d40-242">**Fullständig:** utföra fullständig kompilering, inklusive syntaxkontrollen, optimering, kodgenerering osv.</span><span class="sxs-lookup"><span data-stu-id="11d40-242">**Full:** Perform the full compilation, including syntax check, optimization, code generation, etc.</span></span></li><li><span data-ttu-id="11d40-243">**SingleBox:** utföra fullständig sammanställning med TargetType inställningen till SingleBox.</span><span class="sxs-lookup"><span data-stu-id="11d40-243">**SingleBox:** Perform the full compilation, with TargetType setting to SingleBox.</span></span></li></ul><p><span data-ttu-id="11d40-244">Om du inte anger ett värde för den här egenskapen anger servern optimala kompilering.</span><span class="sxs-lookup"><span data-stu-id="11d40-244">If you don't specify a value for this property, the server determines the optimal compilation mode.</span></span> </p>| <span data-ttu-id="11d40-245">Nej</span><span class="sxs-lookup"><span data-stu-id="11d40-245">No</span></span> | 

<span data-ttu-id="11d40-246">Se [SearchLogProcessing.txt skript Definition](#sample-u-sql-script) skript-definition.</span><span class="sxs-lookup"><span data-stu-id="11d40-246">See [SearchLogProcessing.txt Script Definition](#sample-u-sql-script) for the script definition.</span></span> 

## <a name="sample-input-and-output-datasets"></a><span data-ttu-id="11d40-247">Exempel på indata och utdata datauppsättningar</span><span class="sxs-lookup"><span data-stu-id="11d40-247">Sample input and output datasets</span></span>
### <a name="input-dataset"></a><span data-ttu-id="11d40-248">Indatauppsättning</span><span class="sxs-lookup"><span data-stu-id="11d40-248">Input dataset</span></span>
<span data-ttu-id="11d40-249">I det här exemplet finns indata i ett Azure Data Lake Store (SearchLog.tsv-fil i mappen datalake-indata).</span><span class="sxs-lookup"><span data-stu-id="11d40-249">In this example, the input data resides in an Azure Data Lake Store (SearchLog.tsv file in the datalake/input folder).</span></span> 

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

### <a name="output-dataset"></a><span data-ttu-id="11d40-250">Datamängd för utdata</span><span class="sxs-lookup"><span data-stu-id="11d40-250">Output dataset</span></span>
<span data-ttu-id="11d40-251">I det här exemplet lagras utdata som genereras av U-SQL-skript i ett Azure Data Lake Store (datalake-/ utdata mapp).</span><span class="sxs-lookup"><span data-stu-id="11d40-251">In this example, the output data produced by the U-SQL script is stored in an Azure Data Lake Store (datalake/output folder).</span></span> 

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

### <a name="sample-data-lake-store-linked-service"></a><span data-ttu-id="11d40-252">Exempel Data Lake Store länkade tjänsten</span><span class="sxs-lookup"><span data-stu-id="11d40-252">Sample Data Lake Store Linked Service</span></span>
<span data-ttu-id="11d40-253">Det här är definitionen av exemplet Azure Data Lake Store länkade tjänst som används av in-/ utdata-datauppsättningar.</span><span class="sxs-lookup"><span data-stu-id="11d40-253">Here is the definition of the sample Azure Data Lake Store linked service used by the input/output datasets.</span></span> 

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

<span data-ttu-id="11d40-254">Se [flytta data till och från Azure Data Lake Store](data-factory-azure-datalake-connector.md) artikel beskrivningar av JSON-egenskaper.</span><span class="sxs-lookup"><span data-stu-id="11d40-254">See [Move data to and from Azure Data Lake Store](data-factory-azure-datalake-connector.md) article for descriptions of JSON properties.</span></span> 

## <a name="sample-u-sql-script"></a><span data-ttu-id="11d40-255">Exempel U-SQL-skript</span><span class="sxs-lookup"><span data-stu-id="11d40-255">Sample U-SQL Script</span></span>

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
    TO @out
      USING Outputters.Tsv(quoting:false, dateTimeFormat:null);
```

<span data-ttu-id="11d40-256">Värdena för  **@in**  och  **@out**  parametrar i U-SQL-skriptet skickas dynamiskt av ADF med hjälp av avsnittet-Parametrar'.</span><span class="sxs-lookup"><span data-stu-id="11d40-256">The values for **@in** and **@out** parameters in the U-SQL script are passed dynamically by ADF using the ‘parameters’ section.</span></span> <span data-ttu-id="11d40-257">Se avsnittet-Parametrar' i pipeline-definitionen.</span><span class="sxs-lookup"><span data-stu-id="11d40-257">See the ‘parameters’ section in the pipeline definition.</span></span>

<span data-ttu-id="11d40-258">Du kan ange andra egenskaper, till exempel degreeOfParallelism och prioritet samt i pipeline-definitionen för de jobb som körs på Azure Data Lake Analytics-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="11d40-258">You can specify other properties such as degreeOfParallelism and priority as well in your pipeline definition for the jobs that run on the Azure Data Lake Analytics service.</span></span>

## <a name="dynamic-parameters"></a><span data-ttu-id="11d40-259">Dynamiska parametrar</span><span class="sxs-lookup"><span data-stu-id="11d40-259">Dynamic parameters</span></span>
<span data-ttu-id="11d40-260">I exemplet pipeline-definition tilldelas in och ut parametrar med hårdkodade värden.</span><span class="sxs-lookup"><span data-stu-id="11d40-260">In the sample pipeline definition, in and out parameters are assigned with hard-coded values.</span></span> 

```json
"parameters": {
    "in": "/datalake/input/SearchLog.tsv",
    "out": "/datalake/output/Result.tsv"
}
```

<span data-ttu-id="11d40-261">Det är möjligt att använda dynamiska parametrar i stället.</span><span class="sxs-lookup"><span data-stu-id="11d40-261">It is possible to use dynamic parameters instead.</span></span> <span data-ttu-id="11d40-262">Exempel:</span><span class="sxs-lookup"><span data-stu-id="11d40-262">For example:</span></span> 

```json
"parameters": {
    "in": "$$Text.Format('/datalake/input/{0:yyyy-MM-dd HH:mm:ss}.tsv', SliceStart)",
    "out": "$$Text.Format('/datalake/output/{0:yyyy-MM-dd HH:mm:ss}.tsv', SliceStart)"
}
```

<span data-ttu-id="11d40-263">I det här fallet fortfarande tas upp inkommande filer från mappen /datalake/input och utdatafiler skapas i mappen /datalake/output.</span><span class="sxs-lookup"><span data-stu-id="11d40-263">In this case, input files are still picked up from the /datalake/input folder and output files are generated in the /datalake/output folder.</span></span> <span data-ttu-id="11d40-264">Filnamnen är dynamiska baserat på starttid sektorn.</span><span class="sxs-lookup"><span data-stu-id="11d40-264">The file names are dynamic based on the slice start time.</span></span>  

