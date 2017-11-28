---
title: "aaaCopy data tooand från Azure Data Lake Store | Microsoft Docs"
description: "Lär dig hur toocopy data tooand från Data Lake Store med hjälp av Azure Data Factory"
services: data-factory
documentationcenter: 
author: linda33wj
manager: jhubbard
editor: monicar
ms.assetid: 25b1ff3c-b2fd-48e5-b759-bb2112122e30
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/14/2017
ms.author: jingwang
ms.openlocfilehash: 2e78f75f3821738332dacf70f6bf2c16f0136408
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="copy-data-tooand-from-data-lake-store-by-using-data-factory"></a><span data-ttu-id="088fb-103">Kopiera data tooand från Data Lake Store med hjälp av Data Factory</span><span class="sxs-lookup"><span data-stu-id="088fb-103">Copy data tooand from Data Lake Store by using Data Factory</span></span>
<span data-ttu-id="088fb-104">Den här artikeln förklarar hur toouse Kopieringsaktiviteten i Azure Data Factory toomove data tooand från Azure Data Lake Store.</span><span class="sxs-lookup"><span data-stu-id="088fb-104">This article explains how toouse Copy Activity in Azure Data Factory toomove data tooand from Azure Data Lake Store.</span></span> <span data-ttu-id="088fb-105">Den bygger på hello [Data movement aktiviteter](data-factory-data-movement-activities.md) artikel, en översikt över dataflyttning med Kopieringsaktiviteten.</span><span class="sxs-lookup"><span data-stu-id="088fb-105">It builds on hello [Data movement activities](data-factory-data-movement-activities.md) article, an overview of data movement with Copy Activity.</span></span>

## <a name="supported-scenarios"></a><span data-ttu-id="088fb-106">Scenarier som stöds</span><span class="sxs-lookup"><span data-stu-id="088fb-106">Supported scenarios</span></span>
<span data-ttu-id="088fb-107">Du kan kopiera data **från Azure Data Lake Store** toohello följande datakällor:</span><span class="sxs-lookup"><span data-stu-id="088fb-107">You can copy data **from Azure Data Lake Store** toohello following data stores:</span></span>

[!INCLUDE [data-factory-supported-sinks](../../includes/data-factory-supported-sinks.md)]

<span data-ttu-id="088fb-108">Du kan kopiera data från hello följande datalager **tooAzure Datasjölager**:</span><span class="sxs-lookup"><span data-stu-id="088fb-108">You can copy data from hello following data stores **tooAzure Data Lake Store**:</span></span>

[!INCLUDE [data-factory-supported-sources](../../includes/data-factory-supported-sources.md)]

> [!NOTE]
> <span data-ttu-id="088fb-109">Skapa ett Data Lake Store-konto innan du skapar en pipeline med Kopieringsaktiviteten.</span><span class="sxs-lookup"><span data-stu-id="088fb-109">Create a Data Lake Store account before creating a pipeline with Copy Activity.</span></span> <span data-ttu-id="088fb-110">Mer information finns i [Kom igång med Azure Data Lake Store](../data-lake-store/data-lake-store-get-started-portal.md).</span><span class="sxs-lookup"><span data-stu-id="088fb-110">For more information, see [Get started with Azure Data Lake Store](../data-lake-store/data-lake-store-get-started-portal.md).</span></span>

## <a name="supported-authentication-types"></a><span data-ttu-id="088fb-111">Typer av autentiseringsmetoder som stöds</span><span class="sxs-lookup"><span data-stu-id="088fb-111">Supported authentication types</span></span>
<span data-ttu-id="088fb-112">hello Data Lake Store-anslutningen har stöd för dessa typer av autentisering:</span><span class="sxs-lookup"><span data-stu-id="088fb-112">hello Data Lake Store connector supports these authentication types:</span></span>
* <span data-ttu-id="088fb-113">Autentisering av tjänstens huvudnamn</span><span class="sxs-lookup"><span data-stu-id="088fb-113">Service principal authentication</span></span>
* <span data-ttu-id="088fb-114">Användarautentisering autentiseringsuppgifter (OAuth)</span><span class="sxs-lookup"><span data-stu-id="088fb-114">User credential (OAuth) authentication</span></span> 

<span data-ttu-id="088fb-115">Vi rekommenderar att du använder service principal autentisering, särskilt för en schemalagd datauppsättning kopia.</span><span class="sxs-lookup"><span data-stu-id="088fb-115">We recommend that you use service principal authentication, especially for a scheduled data copy.</span></span> <span data-ttu-id="088fb-116">Giltighetstid för token kan inträffa med autentiseringsuppgifter för användarautentisering.</span><span class="sxs-lookup"><span data-stu-id="088fb-116">Token expiration behavior can occur with user credential authentication.</span></span> <span data-ttu-id="088fb-117">Konfigurationsinformation finns hello [länkade tjänstegenskaper](#linked-service-properties) avsnitt.</span><span class="sxs-lookup"><span data-stu-id="088fb-117">For configuration details, see hello [Linked service properties](#linked-service-properties) section.</span></span>

## <a name="get-started"></a><span data-ttu-id="088fb-118">Kom igång</span><span class="sxs-lookup"><span data-stu-id="088fb-118">Get started</span></span>
<span data-ttu-id="088fb-119">Du kan skapa en pipeline med en kopia-aktivitet som flyttar data till och från ett Azure Data Lake Store med hjälp av olika verktyg/API: er.</span><span class="sxs-lookup"><span data-stu-id="088fb-119">You can create a pipeline with a copy activity that moves data to/from an Azure Data Lake Store by using different tools/APIs.</span></span>

<span data-ttu-id="088fb-120">hello enklaste sättet toocreate pipeline toocopy data är toouse hello **guiden Kopiera**.</span><span class="sxs-lookup"><span data-stu-id="088fb-120">hello easiest way toocreate a pipeline toocopy data is toouse hello **Copy Wizard**.</span></span> <span data-ttu-id="088fb-121">En självstudiekurs om hur du skapar en pipeline med hjälp av hello guiden Kopiera finns [Självstudier: skapa en pipeline med hjälp av guiden Kopiera](data-factory-copy-data-wizard-tutorial.md).</span><span class="sxs-lookup"><span data-stu-id="088fb-121">For a tutorial on creating a pipeline by using hello Copy Wizard, see [Tutorial: Create a pipeline using Copy Wizard](data-factory-copy-data-wizard-tutorial.md).</span></span>

<span data-ttu-id="088fb-122">Du kan också använda följande verktyg toocreate en pipeline hello: **Azure-portalen**, **Visual Studio**, **Azure PowerShell**, **Azure Resource Manager-mall** , **.NET API**, och **REST API**.</span><span class="sxs-lookup"><span data-stu-id="088fb-122">You can also use hello following tools toocreate a pipeline: **Azure portal**, **Visual Studio**, **Azure PowerShell**, **Azure Resource Manager template**, **.NET API**, and **REST API**.</span></span> <span data-ttu-id="088fb-123">Se [kopiera aktivitet kursen](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) för stegvisa instruktioner toocreate en pipeline med en Kopieringsaktivitet.</span><span class="sxs-lookup"><span data-stu-id="088fb-123">See [Copy activity tutorial](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) for step-by-step instructions toocreate a pipeline with a copy activity.</span></span>

<span data-ttu-id="088fb-124">Om du använder hello verktyg eller API: er kan utföra du hello följande steg toocreate en pipeline som flyttar data från en källdata lagra tooa sink-datalagret:</span><span class="sxs-lookup"><span data-stu-id="088fb-124">Whether you use hello tools or APIs, you perform hello following steps toocreate a pipeline that moves data from a source data store tooa sink data store:</span></span>

1. <span data-ttu-id="088fb-125">Skapa en **datafabriken**.</span><span class="sxs-lookup"><span data-stu-id="088fb-125">Create a **data factory**.</span></span> <span data-ttu-id="088fb-126">En datafabrik kan innehålla en eller flera pipelines.</span><span class="sxs-lookup"><span data-stu-id="088fb-126">A data factory may contain one or more pipelines.</span></span> 
2. <span data-ttu-id="088fb-127">Skapa **länkade tjänster** toolink indata och utdata lagrar tooyour data factory.</span><span class="sxs-lookup"><span data-stu-id="088fb-127">Create **linked services** toolink input and output data stores tooyour data factory.</span></span> <span data-ttu-id="088fb-128">Till exempel om du kopierar data från ett Azure Data Lake Store med Azure blob storage tooan skapa du två länkade tjänster toolink dina Azure storage-konto och Azure Data Lake store tooyour data factory.</span><span class="sxs-lookup"><span data-stu-id="088fb-128">For example, if you are copying data from an Azure blob storage tooan Azure Data Lake Store, you create two linked services toolink your Azure storage account and Azure Data Lake store tooyour data factory.</span></span> <span data-ttu-id="088fb-129">Länkad tjänstegenskaper som är specifika tooAzure Data Lake Store, se [länkade tjänstegenskaper](#linked-service-properties) avsnitt.</span><span class="sxs-lookup"><span data-stu-id="088fb-129">For linked service properties that are specific tooAzure Data Lake Store, see [linked service properties](#linked-service-properties) section.</span></span> 
2. <span data-ttu-id="088fb-130">Skapa **datauppsättningar** toorepresent indata och utdata för hello kopieringsåtgärden.</span><span class="sxs-lookup"><span data-stu-id="088fb-130">Create **datasets** toorepresent input and output data for hello copy operation.</span></span> <span data-ttu-id="088fb-131">I hello exempelvis nämns i hello sista steget skapar du en dataset toospecify hello blob-behållaren och mappen som innehåller hello indata.</span><span class="sxs-lookup"><span data-stu-id="088fb-131">In hello example mentioned in hello last step, you create a dataset toospecify hello blob container and folder that contains hello input data.</span></span> <span data-ttu-id="088fb-132">Och du skapar en annan dataset toospecify hello mapp och filsökvägen i hello Data Lake store som innehåller hello data som kopieras från hello blob storage.</span><span class="sxs-lookup"><span data-stu-id="088fb-132">And, you create another dataset toospecify hello folder and file path in hello Data Lake store that holds hello data copied from hello blob storage.</span></span> <span data-ttu-id="088fb-133">Egenskaper för datamängd som är specifika tooAzure Data Lake Store, se [egenskaper för datamängd](#dataset-properties) avsnitt.</span><span class="sxs-lookup"><span data-stu-id="088fb-133">For dataset properties that are specific tooAzure Data Lake Store, see [dataset properties](#dataset-properties) section.</span></span>
3. <span data-ttu-id="088fb-134">Skapa en **pipeline** med en kopia-aktivitet som tar en datamängd som indata och en dataset som utdata.</span><span class="sxs-lookup"><span data-stu-id="088fb-134">Create a **pipeline** with a copy activity that takes a dataset as an input and a dataset as an output.</span></span> <span data-ttu-id="088fb-135">I hello-exemplet ovan, använder du BlobSource som en källa och AzureDataLakeStoreSink som en mottagare för hello kopieringsaktiviteten.</span><span class="sxs-lookup"><span data-stu-id="088fb-135">In hello example mentioned earlier, you use BlobSource as a source and AzureDataLakeStoreSink as a sink for hello copy activity.</span></span> <span data-ttu-id="088fb-136">På samma sätt om du vill kopiera från Azure Data Lake Store tooAzure Blob Storage, använder du AzureDataLakeStoreSource och BlobSink i hello kopieringsaktiviteten.</span><span class="sxs-lookup"><span data-stu-id="088fb-136">Similarly, if you are copying from Azure Data Lake Store tooAzure Blob Storage, you use AzureDataLakeStoreSource  and BlobSink in hello copy activity.</span></span> <span data-ttu-id="088fb-137">Kopiera Aktivitetsegenskaper som är specifika tooAzure Data Lake Store, se [kopiera Aktivitetsegenskaper](#copy-activity-properties) avsnitt.</span><span class="sxs-lookup"><span data-stu-id="088fb-137">For copy activity properties that are specific tooAzure Data Lake Store, see [copy activity properties](#copy-activity-properties) section.</span></span> <span data-ttu-id="088fb-138">Mer information om hur toouse en databas som en källa eller en mottagare klickar du på hello länk under föregående hello för datalager.</span><span class="sxs-lookup"><span data-stu-id="088fb-138">For details on how toouse a data store as a source or a sink, click hello link in hello previous section for your data store.</span></span>  

<span data-ttu-id="088fb-139">När du använder guiden hello skapas automatiskt JSON definitioner för dessa Data Factory-enheter (länkade tjänster, datauppsättningar och hello pipeline) för dig.</span><span class="sxs-lookup"><span data-stu-id="088fb-139">When you use hello wizard, JSON definitions for these Data Factory entities (linked services, datasets, and hello pipeline) are automatically created for you.</span></span> <span data-ttu-id="088fb-140">När du använder Verktyg/API: er (utom .NET API), kan du definiera dessa Data Factory-enheter med hjälp av hello JSON-format.</span><span class="sxs-lookup"><span data-stu-id="088fb-140">When you use tools/APIs (except .NET API), you define these Data Factory entities by using hello JSON format.</span></span>  <span data-ttu-id="088fb-141">Exempel med JSON-definitioner för Data Factory-entiteter som har använt toocopy data till/från ett Azure Data Lake Store finns [JSON-exempel](#json-examples-for-copying-data-to-and-from-data-lake-store) i den här artikeln.</span><span class="sxs-lookup"><span data-stu-id="088fb-141">For samples with JSON definitions for Data Factory entities that are used toocopy data to/from an Azure Data Lake Store, see [JSON examples](#json-examples-for-copying-data-to-and-from-data-lake-store) section of this article.</span></span>

<span data-ttu-id="088fb-142">hello följande avsnitt innehåller information om JSON-egenskaper används toodefine Data Factory entiteter specifika tooData Lake Store.</span><span class="sxs-lookup"><span data-stu-id="088fb-142">hello following sections provide details about JSON properties that are used toodefine Data Factory entities specific tooData Lake Store.</span></span>

## <a name="linked-service-properties"></a><span data-ttu-id="088fb-143">Länkad tjänstegenskaper</span><span class="sxs-lookup"><span data-stu-id="088fb-143">Linked service properties</span></span>
<span data-ttu-id="088fb-144">En länkad tjänst länkar en data store tooa data factory.</span><span class="sxs-lookup"><span data-stu-id="088fb-144">A linked service links a data store tooa data factory.</span></span> <span data-ttu-id="088fb-145">Du skapar en länkad tjänst av typen **AzureDataLakeStore** toolink din Data Lake Store data tooyour data factory.</span><span class="sxs-lookup"><span data-stu-id="088fb-145">You create a linked service of type **AzureDataLakeStore** toolink your Data Lake Store data tooyour data factory.</span></span> <span data-ttu-id="088fb-146">hello i den följande tabellen beskrivs JSON-element specifika tooData Datasjölager länkade tjänster.</span><span class="sxs-lookup"><span data-stu-id="088fb-146">hello following table describes JSON elements specific tooData Lake Store linked services.</span></span> <span data-ttu-id="088fb-147">Du kan välja mellan tjänstens huvudnamn och autentiseringsuppgifter för användarautentisering.</span><span class="sxs-lookup"><span data-stu-id="088fb-147">You can choose between service principal and user credential authentication.</span></span>

| <span data-ttu-id="088fb-148">Egenskap</span><span class="sxs-lookup"><span data-stu-id="088fb-148">Property</span></span> | <span data-ttu-id="088fb-149">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="088fb-149">Description</span></span> | <span data-ttu-id="088fb-150">Krävs</span><span class="sxs-lookup"><span data-stu-id="088fb-150">Required</span></span> |
|:--- |:--- |:--- |
| <span data-ttu-id="088fb-151">**typ**</span><span class="sxs-lookup"><span data-stu-id="088fb-151">**type**</span></span> | <span data-ttu-id="088fb-152">hello Typegenskapen måste anges för**AzureDataLakeStore**.</span><span class="sxs-lookup"><span data-stu-id="088fb-152">hello type property must be set too**AzureDataLakeStore**.</span></span> | <span data-ttu-id="088fb-153">Ja</span><span class="sxs-lookup"><span data-stu-id="088fb-153">Yes</span></span> |
| <span data-ttu-id="088fb-154">**dataLakeStoreUri**</span><span class="sxs-lookup"><span data-stu-id="088fb-154">**dataLakeStoreUri**</span></span> | <span data-ttu-id="088fb-155">Information om hello Azure Data Lake Store-konto.</span><span class="sxs-lookup"><span data-stu-id="088fb-155">Information about hello Azure Data Lake Store account.</span></span> <span data-ttu-id="088fb-156">Den här informationen tar en hello följande format: `https://[accountname].azuredatalakestore.net/webhdfs/v1` eller `adl://[accountname].azuredatalakestore.net/`.</span><span class="sxs-lookup"><span data-stu-id="088fb-156">This information takes one of hello following formats: `https://[accountname].azuredatalakestore.net/webhdfs/v1` or `adl://[accountname].azuredatalakestore.net/`.</span></span> | <span data-ttu-id="088fb-157">Ja</span><span class="sxs-lookup"><span data-stu-id="088fb-157">Yes</span></span> |
| <span data-ttu-id="088fb-158">**prenumerations-ID**</span><span class="sxs-lookup"><span data-stu-id="088fb-158">**subscriptionId**</span></span> | <span data-ttu-id="088fb-159">Azure-prenumeration ID toowhich hello Data Lake Store-konto tillhör.</span><span class="sxs-lookup"><span data-stu-id="088fb-159">Azure subscription ID toowhich hello Data Lake Store account belongs.</span></span> | <span data-ttu-id="088fb-160">Krävs för sink</span><span class="sxs-lookup"><span data-stu-id="088fb-160">Required for sink</span></span> |
| <span data-ttu-id="088fb-161">**resourceGroupName**</span><span class="sxs-lookup"><span data-stu-id="088fb-161">**resourceGroupName**</span></span> | <span data-ttu-id="088fb-162">Azure-resurs grupp namnet toowhich hello Data Lake Store-konto tillhör.</span><span class="sxs-lookup"><span data-stu-id="088fb-162">Azure resource group name toowhich hello Data Lake Store account belongs.</span></span> | <span data-ttu-id="088fb-163">Krävs för sink</span><span class="sxs-lookup"><span data-stu-id="088fb-163">Required for sink</span></span> |

### <a name="service-principal-authentication-recommended"></a><span data-ttu-id="088fb-164">Tjänstens huvudnamn autentisering (rekommenderas)</span><span class="sxs-lookup"><span data-stu-id="088fb-164">Service principal authentication (recommended)</span></span>
<span data-ttu-id="088fb-165">toouse service principal autentisering, registrera en Programenhet i Azure Active Directory (Azure AD) och bevilja den hello komma åt tooData Lake Store.</span><span class="sxs-lookup"><span data-stu-id="088fb-165">toouse service principal authentication, register an application entity in Azure Active Directory (Azure AD) and grant it hello access tooData Lake Store.</span></span> <span data-ttu-id="088fb-166">Detaljerade anvisningar finns i [tjänst-till-tjänst autentisering](../data-lake-store/data-lake-store-authenticate-using-active-directory.md).</span><span class="sxs-lookup"><span data-stu-id="088fb-166">For detailed steps, see [Service-to-service authentication](../data-lake-store/data-lake-store-authenticate-using-active-directory.md).</span></span> <span data-ttu-id="088fb-167">Anteckna hello följande värden som du använder toodefine hello länkade tjänsten:</span><span class="sxs-lookup"><span data-stu-id="088fb-167">Make note of hello following values, which you use toodefine hello linked service:</span></span>
* <span data-ttu-id="088fb-168">Program-ID:t</span><span class="sxs-lookup"><span data-stu-id="088fb-168">Application ID</span></span>
* <span data-ttu-id="088fb-169">Nyckeln för programmet</span><span class="sxs-lookup"><span data-stu-id="088fb-169">Application key</span></span> 
* <span data-ttu-id="088fb-170">Klient-ID:t</span><span class="sxs-lookup"><span data-stu-id="088fb-170">Tenant ID</span></span>

> [!IMPORTANT]
> <span data-ttu-id="088fb-171">Om du använder hello guiden Kopiera tooauthor data pipelines, se till att du ger hello tjänstens huvudnamn på minst **Reader** roll i åtkomstkontroll (identitets- och åtkomsthantering) för hello Data Lake Store-konto.</span><span class="sxs-lookup"><span data-stu-id="088fb-171">If you are using hello Copy Wizard tooauthor data pipelines, make sure that you grant hello service principal at least a **Reader** role in access control (identity and access management) for hello Data Lake Store account.</span></span> <span data-ttu-id="088fb-172">Dessutom ge hello tjänstens huvudnamn minst **Läs + Execute** behörighet tooyour Data Lake Store rot (”/”) och dess underordnade.</span><span class="sxs-lookup"><span data-stu-id="088fb-172">Also, grant hello service principal at least **Read + Execute** permission tooyour Data Lake Store root ("/") and its children.</span></span> <span data-ttu-id="088fb-173">Annars kan du se hello-meddelande ”hello angivna autentiseringsuppgifterna är ogiltiga”.</span><span class="sxs-lookup"><span data-stu-id="088fb-173">Otherwise you might see hello message "hello credentials provided are invalid."</span></span><br/><br/>
<span data-ttu-id="088fb-174">När du skapar eller uppdaterar ett huvudnamn för tjänsten i Azure AD, kan det ta några minuter för hello ändringar tootake effekt.</span><span class="sxs-lookup"><span data-stu-id="088fb-174">After you create or update a service principal in Azure AD, it can take a few minutes for hello changes tootake effect.</span></span> <span data-ttu-id="088fb-175">Kontrollera hello tjänstens huvudnamn och Data Lake Store access control åtkomstkontrollistan (ACL) konfigurationer.</span><span class="sxs-lookup"><span data-stu-id="088fb-175">Check hello service principal and Data Lake Store access control list (ACL) configurations.</span></span> <span data-ttu-id="088fb-176">Om du ser fortfarande hello-meddelande ”hello angivna autentiseringsuppgifterna är ogiltiga”, vänta en stund och försök igen.</span><span class="sxs-lookup"><span data-stu-id="088fb-176">If you still see hello message "hello credentials provided are invalid," wait a while and try again.</span></span>

<span data-ttu-id="088fb-177">Använd service principal autentisering genom att ange hello följande egenskaper:</span><span class="sxs-lookup"><span data-stu-id="088fb-177">Use service principal authentication by specifying hello following properties:</span></span>

| <span data-ttu-id="088fb-178">Egenskap</span><span class="sxs-lookup"><span data-stu-id="088fb-178">Property</span></span> | <span data-ttu-id="088fb-179">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="088fb-179">Description</span></span> | <span data-ttu-id="088fb-180">Krävs</span><span class="sxs-lookup"><span data-stu-id="088fb-180">Required</span></span> |
|:--- |:--- |:--- |
| <span data-ttu-id="088fb-181">**servicePrincipalId**</span><span class="sxs-lookup"><span data-stu-id="088fb-181">**servicePrincipalId**</span></span> | <span data-ttu-id="088fb-182">Ange hello programmets klient-ID.</span><span class="sxs-lookup"><span data-stu-id="088fb-182">Specify hello application's client ID.</span></span> | <span data-ttu-id="088fb-183">Ja</span><span class="sxs-lookup"><span data-stu-id="088fb-183">Yes</span></span> |
| <span data-ttu-id="088fb-184">**servicePrincipalKey**</span><span class="sxs-lookup"><span data-stu-id="088fb-184">**servicePrincipalKey**</span></span> | <span data-ttu-id="088fb-185">Ange hello programnyckel.</span><span class="sxs-lookup"><span data-stu-id="088fb-185">Specify hello application's key.</span></span> | <span data-ttu-id="088fb-186">Ja</span><span class="sxs-lookup"><span data-stu-id="088fb-186">Yes</span></span> |
| <span data-ttu-id="088fb-187">**klient**</span><span class="sxs-lookup"><span data-stu-id="088fb-187">**tenant**</span></span> | <span data-ttu-id="088fb-188">Ange hello klient information (domain name eller klient ID) under där programmet finns.</span><span class="sxs-lookup"><span data-stu-id="088fb-188">Specify hello tenant information (domain name or tenant ID) under which your application resides.</span></span> <span data-ttu-id="088fb-189">Du kan hämta den med hovra hello musen i hello övre högra hörnet av hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="088fb-189">You can retrieve it by hovering hello mouse in hello upper-right corner of hello Azure portal.</span></span> | <span data-ttu-id="088fb-190">Ja</span><span class="sxs-lookup"><span data-stu-id="088fb-190">Yes</span></span> |

<span data-ttu-id="088fb-191">**Exempel: Service principal autentisering**</span><span class="sxs-lookup"><span data-stu-id="088fb-191">**Example: Service principal authentication**</span></span>
```json
{
    "name": "AzureDataLakeStoreLinkedService",
    "properties": {
        "type": "AzureDataLakeStore",
        "typeProperties": {
            "dataLakeStoreUri": "https://<accountname>.azuredatalakestore.net/webhdfs/v1",
            "servicePrincipalId": "<service principal id>",
            "servicePrincipalKey": "<service principal key>",
            "tenant": "<tenant info, e.g. microsoft.onmicrosoft.com>",
            "subscriptionId": "<subscription of ADLS>",
            "resourceGroupName": "<resource group of ADLS>"
        }
    }
}
```

### <a name="user-credential-authentication"></a><span data-ttu-id="088fb-192">Användarautentisering för autentiseringsuppgifter</span><span class="sxs-lookup"><span data-stu-id="088fb-192">User credential authentication</span></span>
<span data-ttu-id="088fb-193">Du kan också använda användarens autentiseringsuppgifter autentisering toocopy från eller tooData Datasjölager genom att ange hello följande egenskaper:</span><span class="sxs-lookup"><span data-stu-id="088fb-193">Alternatively, you can use user credential authentication toocopy from or tooData Lake Store by specifying hello following properties:</span></span>

| <span data-ttu-id="088fb-194">Egenskap</span><span class="sxs-lookup"><span data-stu-id="088fb-194">Property</span></span> | <span data-ttu-id="088fb-195">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="088fb-195">Description</span></span> | <span data-ttu-id="088fb-196">Krävs</span><span class="sxs-lookup"><span data-stu-id="088fb-196">Required</span></span> |
|:--- |:--- |:--- |
| <span data-ttu-id="088fb-197">**auktorisering**</span><span class="sxs-lookup"><span data-stu-id="088fb-197">**authorization**</span></span> | <span data-ttu-id="088fb-198">Klicka på hello **auktorisera** i hello Data Factory-redigeraren och ange dina autentiseringsuppgifter som tilldelar hello automatiskt genererade auktorisering URL toothis-egenskapen.</span><span class="sxs-lookup"><span data-stu-id="088fb-198">Click hello **Authorize** button in hello Data Factory Editor and enter your credential that assigns hello autogenerated authorization URL toothis property.</span></span> | <span data-ttu-id="088fb-199">Ja</span><span class="sxs-lookup"><span data-stu-id="088fb-199">Yes</span></span> |
| <span data-ttu-id="088fb-200">**sessions-ID**</span><span class="sxs-lookup"><span data-stu-id="088fb-200">**sessionId**</span></span> | <span data-ttu-id="088fb-201">OAuth sessions-ID från hello OAuth-auktorisering session.</span><span class="sxs-lookup"><span data-stu-id="088fb-201">OAuth session ID from hello OAuth authorization session.</span></span> <span data-ttu-id="088fb-202">Varje sessions-ID är unikt och kan bara användas en gång.</span><span class="sxs-lookup"><span data-stu-id="088fb-202">Each session ID is unique and can be used only once.</span></span> <span data-ttu-id="088fb-203">Den här inställningen genereras automatiskt när du använder hello Data Factory-redigeraren.</span><span class="sxs-lookup"><span data-stu-id="088fb-203">This setting is automatically generated when you use hello Data Factory Editor.</span></span> | <span data-ttu-id="088fb-204">Ja</span><span class="sxs-lookup"><span data-stu-id="088fb-204">Yes</span></span> |

<span data-ttu-id="088fb-205">**Exempel: Användarautentisering för autentiseringsuppgifter**</span><span class="sxs-lookup"><span data-stu-id="088fb-205">**Example: User credential authentication**</span></span>
```json
{
    "name": "AzureDataLakeStoreLinkedService",
    "properties": {
        "type": "AzureDataLakeStore",
        "typeProperties": {
            "dataLakeStoreUri": "https://<accountname>.azuredatalakestore.net/webhdfs/v1",
            "sessionId": "<session ID>",
            "authorization": "<authorization URL>",
            "subscriptionId": "<subscription of ADLS>",
            "resourceGroupName": "<resource group of ADLS>"
        }
    }
}
```

#### <a name="token-expiration"></a><span data-ttu-id="088fb-206">Token upphör att gälla</span><span class="sxs-lookup"><span data-stu-id="088fb-206">Token expiration</span></span>
<span data-ttu-id="088fb-207">Hej auktoriseringskod som du skapar med hjälp av hello **auktorisera** knappen upphör att gälla efter en viss tid.</span><span class="sxs-lookup"><span data-stu-id="088fb-207">hello authorization code that you generate by using hello **Authorize** button expires after a certain amount of time.</span></span> <span data-ttu-id="088fb-208">hello efter meddelande innebär att hello-autentiseringstoken har upphört att gälla:</span><span class="sxs-lookup"><span data-stu-id="088fb-208">hello following message means that hello authentication token has expired:</span></span>

<span data-ttu-id="088fb-209">Autentiseringsuppgifter fel: invalid_grant - AADSTS70002: fel vid verifiering av autentiseringsuppgifter.</span><span class="sxs-lookup"><span data-stu-id="088fb-209">Credential operation error: invalid_grant - AADSTS70002: Error validating credentials.</span></span> <span data-ttu-id="088fb-210">AADSTS70008: hello angetts åtkomsten har upphört att gälla eller återkallats.</span><span class="sxs-lookup"><span data-stu-id="088fb-210">AADSTS70008: hello provided access grant is expired or revoked.</span></span> <span data-ttu-id="088fb-211">Trace-ID: d18629e8-af88-43c5-88e3-d8419eb1fca1 Korrelations-ID: fac30a0c-6be6-4e02-8d69-a776d2ffefd7 tidsstämpel: 2015-12-15 21-09-31Z.</span><span class="sxs-lookup"><span data-stu-id="088fb-211">Trace ID: d18629e8-af88-43c5-88e3-d8419eb1fca1 Correlation ID: fac30a0c-6be6-4e02-8d69-a776d2ffefd7 Timestamp: 2015-12-15 21-09-31Z.</span></span>

<span data-ttu-id="088fb-212">hello visar följande tabell hello giltighetstid gånger med olika typer av användarkonton:</span><span class="sxs-lookup"><span data-stu-id="088fb-212">hello following table shows hello expiration times of different types of user accounts:</span></span>


| <span data-ttu-id="088fb-213">Användartyp</span><span class="sxs-lookup"><span data-stu-id="088fb-213">User type</span></span> | <span data-ttu-id="088fb-214">Upphör att gälla efter</span><span class="sxs-lookup"><span data-stu-id="088fb-214">Expires after</span></span> |
|:--- |:--- |
| <span data-ttu-id="088fb-215">Användarkonton *inte* hanteras av Azure Active Directory (till exempel @hotmail.com eller @live.com)</span><span class="sxs-lookup"><span data-stu-id="088fb-215">User accounts *not* managed by Azure Active Directory (for example, @hotmail.com or @live.com)</span></span> |<span data-ttu-id="088fb-216">12 timmar</span><span class="sxs-lookup"><span data-stu-id="088fb-216">12 hours</span></span> |
| <span data-ttu-id="088fb-217">Användarkonton som hanteras av Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="088fb-217">Users accounts managed by Azure Active Directory</span></span> |<span data-ttu-id="088fb-218">Kör 14 dagar efter hello sista segmentet</span><span class="sxs-lookup"><span data-stu-id="088fb-218">14 days after hello last slice run</span></span> <br/><br/><span data-ttu-id="088fb-219">90 dagar, om ett segment baserat på en länkad OAuth-tjänst körs på minst en gång var fjortonde dag</span><span class="sxs-lookup"><span data-stu-id="088fb-219">90 days, if a slice based on an OAuth-based linked service runs at least once every 14 days</span></span> |

<span data-ttu-id="088fb-220">Om du ändrar ditt lösenord innan hello token förfallotid hello token upphör att gälla omedelbart.</span><span class="sxs-lookup"><span data-stu-id="088fb-220">If you change your password before hello token expiration time, hello token expires immediately.</span></span> <span data-ttu-id="088fb-221">Hello-meddelande som nämnts tidigare i det här avsnittet visas.</span><span class="sxs-lookup"><span data-stu-id="088fb-221">You will see hello message mentioned earlier in this section.</span></span>

<span data-ttu-id="088fb-222">Du kan omauktorisera hello-konto med hjälp av hello **auktorisera** knappen när hello-token upphör att gälla tooredeploy hello länkade tjänsten.</span><span class="sxs-lookup"><span data-stu-id="088fb-222">You can reauthorize hello account by using hello **Authorize** button when hello token expires tooredeploy hello linked service.</span></span> <span data-ttu-id="088fb-223">Du kan också generera värdena för hello **sessionId** och **auktorisering** egenskaper programmatiskt genom att använda hello följande kod:</span><span class="sxs-lookup"><span data-stu-id="088fb-223">You can also generate values for hello **sessionId** and **authorization** properties programmatically by using hello following code:</span></span>


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
<span data-ttu-id="088fb-224">Mer information om hello Data Factory-klasser som används i koden hello finns hello [AzureDataLakeStoreLinkedService klassen](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.azuredatalakestorelinkedservice.aspx), [AzureDataLakeAnalyticsLinkedService klassen](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.azuredatalakeanalyticslinkedservice.aspx), och [ AuthorizationSessionGetResponse klassen](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.authorizationsessiongetresponse.aspx) avsnitt.</span><span class="sxs-lookup"><span data-stu-id="088fb-224">For details about hello Data Factory classes used in hello code, see hello [AzureDataLakeStoreLinkedService Class](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.azuredatalakestorelinkedservice.aspx), [AzureDataLakeAnalyticsLinkedService Class](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.azuredatalakeanalyticslinkedservice.aspx), and [AuthorizationSessionGetResponse Class](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.authorizationsessiongetresponse.aspx) topics.</span></span> <span data-ttu-id="088fb-225">Lägg till en referens tooversion `2.9.10826.1824` av `Microsoft.IdentityModel.Clients.ActiveDirectory.WindowsForms.dll` för hello `WindowsFormsWebAuthenticationDialog` klass som används i hello kod.</span><span class="sxs-lookup"><span data-stu-id="088fb-225">Add a reference tooversion `2.9.10826.1824` of `Microsoft.IdentityModel.Clients.ActiveDirectory.WindowsForms.dll` for hello `WindowsFormsWebAuthenticationDialog` class used in hello code.</span></span>

## <a name="dataset-properties"></a><span data-ttu-id="088fb-226">Egenskaper för datamängd</span><span class="sxs-lookup"><span data-stu-id="088fb-226">Dataset properties</span></span>
<span data-ttu-id="088fb-227">toospecify en dataset toorepresent indata i ett Data Lake Store som du ställer in hello **typen** -egenskapen för hello datamängden för**AzureDataLakeStore**.</span><span class="sxs-lookup"><span data-stu-id="088fb-227">toospecify a dataset toorepresent input data in a Data Lake Store, you set hello **type** property of hello dataset too**AzureDataLakeStore**.</span></span> <span data-ttu-id="088fb-228">Ange hello **linkedServiceName** länkad egenskap hello dataset toohello namnet på hello Data Lake Store-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="088fb-228">Set hello **linkedServiceName** property of hello dataset toohello name of hello Data Lake Store linked service.</span></span> <span data-ttu-id="088fb-229">En fullständig lista över JSON-avsnitt och egenskaper som är tillgängliga för att definiera datauppsättningar finns hello [skapa datauppsättningar](data-factory-create-datasets.md) artikel.</span><span class="sxs-lookup"><span data-stu-id="088fb-229">For a full list of JSON sections and properties available for defining datasets, see hello [Creating datasets](data-factory-create-datasets.md) article.</span></span> <span data-ttu-id="088fb-230">Avsnitt i en dataset i JSON, t.ex **struktur**, **tillgänglighet**, och **princip**, är liknande för alla typer av datauppsättningen (Azure SQL-databasen, Azure blob och Azure-tabellen, till exempel).</span><span class="sxs-lookup"><span data-stu-id="088fb-230">Sections of a dataset in JSON, such as **structure**, **availability**, and **policy**, are similar for all dataset types (Azure SQL database, Azure blob, and Azure table, for example).</span></span> <span data-ttu-id="088fb-231">Hej **typeProperties** avsnittet skiljer sig för varje typ av datauppsättningen och innehåller information som plats och dataformat hello i hello datalagret.</span><span class="sxs-lookup"><span data-stu-id="088fb-231">hello **typeProperties** section is different for each type of dataset and provides information such as location and format of hello data in hello data store.</span></span> 

<span data-ttu-id="088fb-232">Hej **typeProperties** avsnittet för en dataset av typen **AzureDataLakeStore** innehåller hello följande egenskaper:</span><span class="sxs-lookup"><span data-stu-id="088fb-232">hello **typeProperties** section for a dataset of type **AzureDataLakeStore** contains hello following properties:</span></span>

| <span data-ttu-id="088fb-233">Egenskap</span><span class="sxs-lookup"><span data-stu-id="088fb-233">Property</span></span> | <span data-ttu-id="088fb-234">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="088fb-234">Description</span></span> | <span data-ttu-id="088fb-235">Krävs</span><span class="sxs-lookup"><span data-stu-id="088fb-235">Required</span></span> |
|:--- |:--- |:--- |
| <span data-ttu-id="088fb-236">**folderPath**</span><span class="sxs-lookup"><span data-stu-id="088fb-236">**folderPath**</span></span> |<span data-ttu-id="088fb-237">Sökvägen toohello behållaren och mappen i Data Lake Store.</span><span class="sxs-lookup"><span data-stu-id="088fb-237">Path toohello container and folder in Data Lake Store.</span></span> |<span data-ttu-id="088fb-238">Ja</span><span class="sxs-lookup"><span data-stu-id="088fb-238">Yes</span></span> |
| <span data-ttu-id="088fb-239">**Filnamn**</span><span class="sxs-lookup"><span data-stu-id="088fb-239">**fileName**</span></span> |<span data-ttu-id="088fb-240">Namnet på hello-fil i Azure Data Lake Store.</span><span class="sxs-lookup"><span data-stu-id="088fb-240">Name of hello file in Azure Data Lake Store.</span></span> <span data-ttu-id="088fb-241">Hej **fileName** egenskapen är valfri och är skiftlägeskänsliga.</span><span class="sxs-lookup"><span data-stu-id="088fb-241">hello **fileName** property is optional and case-sensitive.</span></span> <br/><br/><span data-ttu-id="088fb-242">Om du anger **fileName**, hello aktivitet (inklusive kopia) fungerar på hello specifik fil.</span><span class="sxs-lookup"><span data-stu-id="088fb-242">If you specify **fileName**, hello activity (including Copy) works on hello specific file.</span></span><br/><br/><span data-ttu-id="088fb-243">När **fileName** anges kopia innehåller alla filer i **folderPath** i hello inkommande dataset.</span><span class="sxs-lookup"><span data-stu-id="088fb-243">When **fileName** is not specified, Copy includes all files in **folderPath** in hello input dataset.</span></span><br/><br/><span data-ttu-id="088fb-244">När **fileName** har inte angetts för en datamängd för utdata och **preserveHierarchy** har inte angetts i aktiviteten sink hello hello genereras filen heter hello format Data. _GUID_.txt'.</span><span class="sxs-lookup"><span data-stu-id="088fb-244">When **fileName** is not specified for an output dataset and **preserveHierarchy** is not specified in activity sink, hello name of hello generated file is in hello format Data._Guid_.txt\`.</span></span> <span data-ttu-id="088fb-245">Till exempel: Data.0a405f8a-93ff-4c6f-b3be-f69616f1df7a.txt.</span><span class="sxs-lookup"><span data-stu-id="088fb-245">For example: Data.0a405f8a-93ff-4c6f-b3be-f69616f1df7a.txt.</span></span> |<span data-ttu-id="088fb-246">Nej</span><span class="sxs-lookup"><span data-stu-id="088fb-246">No</span></span> |
| <span data-ttu-id="088fb-247">**partitionedBy**</span><span class="sxs-lookup"><span data-stu-id="088fb-247">**partitionedBy**</span></span> |<span data-ttu-id="088fb-248">Hej **partitionedBy** egenskapen är valfri.</span><span class="sxs-lookup"><span data-stu-id="088fb-248">hello **partitionedBy** property is optional.</span></span> <span data-ttu-id="088fb-249">Du kan använda den toospecify en dynamisk sökväg och filnamn för time series-data.</span><span class="sxs-lookup"><span data-stu-id="088fb-249">You can use it toospecify a dynamic path and file name for time-series data.</span></span> <span data-ttu-id="088fb-250">Till exempel **folderPath** kan parameteriseras för varje timme av data.</span><span class="sxs-lookup"><span data-stu-id="088fb-250">For example, **folderPath** can be parameterized for every hour of data.</span></span> <span data-ttu-id="088fb-251">Mer information och exempel finns [hello partitionedBy egenskap](#using-partitionedby-property).</span><span class="sxs-lookup"><span data-stu-id="088fb-251">For details and examples, see [hello partitionedBy property](#using-partitionedby-property).</span></span> |<span data-ttu-id="088fb-252">Nej</span><span class="sxs-lookup"><span data-stu-id="088fb-252">No</span></span> |
| <span data-ttu-id="088fb-253">**format**</span><span class="sxs-lookup"><span data-stu-id="088fb-253">**format**</span></span> | <span data-ttu-id="088fb-254">hello följande formattyper stöds: **TextFormat**, **JsonFormat**, **AvroFormat**, **OrcFormat**, och  **ParquetFormat**.</span><span class="sxs-lookup"><span data-stu-id="088fb-254">hello following format types are supported: **TextFormat**, **JsonFormat**, **AvroFormat**, **OrcFormat**, and **ParquetFormat**.</span></span> <span data-ttu-id="088fb-255">Ange hello **typen** egenskap under **format** tooone av dessa värden.</span><span class="sxs-lookup"><span data-stu-id="088fb-255">Set hello **type** property under **format** tooone of these values.</span></span> <span data-ttu-id="088fb-256">Mer information finns i hello [textformat](data-factory-supported-file-and-compression-formats.md#text-format), [JSON-format](data-factory-supported-file-and-compression-formats.md#json-format), [Avro-formatet](data-factory-supported-file-and-compression-formats.md#avro-format), [ORC format](data-factory-supported-file-and-compression-formats.md#orc-format), och [parkettgolv Format ](data-factory-supported-file-and-compression-formats.md#parquet-format) avsnitt i hello [format och komprimering stöds av Azure Data Factory](data-factory-supported-file-and-compression-formats.md) artikel.</span><span class="sxs-lookup"><span data-stu-id="088fb-256">For more information, see hello [Text format](data-factory-supported-file-and-compression-formats.md#text-format), [JSON format](data-factory-supported-file-and-compression-formats.md#json-format), [Avro format](data-factory-supported-file-and-compression-formats.md#avro-format), [ORC format](data-factory-supported-file-and-compression-formats.md#orc-format), and [Parquet Format](data-factory-supported-file-and-compression-formats.md#parquet-format) sections in hello [File and compression formats supported by Azure Data Factory](data-factory-supported-file-and-compression-formats.md) article.</span></span> <br><br> <span data-ttu-id="088fb-257">Om du vill toocopy filer ”som-är” mellan filbaserade butiker (binär kopia), hoppar du över hello `format` avsnitt i både inkommande och utgående dataset-definitioner.</span><span class="sxs-lookup"><span data-stu-id="088fb-257">If you want toocopy files "as-is" between file-based stores (binary copy), skip hello `format` section in both input and output dataset definitions.</span></span> |<span data-ttu-id="088fb-258">Nej</span><span class="sxs-lookup"><span data-stu-id="088fb-258">No</span></span> |
| <span data-ttu-id="088fb-259">**komprimering**</span><span class="sxs-lookup"><span data-stu-id="088fb-259">**compression**</span></span> | <span data-ttu-id="088fb-260">Ange hello typ och kompression för hello data.</span><span class="sxs-lookup"><span data-stu-id="088fb-260">Specify hello type and level of compression for hello data.</span></span> <span data-ttu-id="088fb-261">Typer som stöds är **GZip**, **Deflate**, **BZip2**, och **ZipDeflate**.</span><span class="sxs-lookup"><span data-stu-id="088fb-261">Supported types are **GZip**, **Deflate**, **BZip2**, and **ZipDeflate**.</span></span> <span data-ttu-id="088fb-262">Stöds nivåerna **Optimal** och **snabbast**.</span><span class="sxs-lookup"><span data-stu-id="088fb-262">Supported levels are **Optimal** and **Fastest**.</span></span> <span data-ttu-id="088fb-263">Mer information finns i [format och komprimering stöds av Azure Data Factory](data-factory-supported-file-and-compression-formats.md#compression-support).</span><span class="sxs-lookup"><span data-stu-id="088fb-263">For more information, see [File and compression formats supported by Azure Data Factory](data-factory-supported-file-and-compression-formats.md#compression-support).</span></span> |<span data-ttu-id="088fb-264">Nej</span><span class="sxs-lookup"><span data-stu-id="088fb-264">No</span></span> |

### <a name="hello-partitionedby-property"></a><span data-ttu-id="088fb-265">Hej partitionedBy egenskap</span><span class="sxs-lookup"><span data-stu-id="088fb-265">hello partitionedBy property</span></span>
<span data-ttu-id="088fb-266">Du kan ange dynamiska **folderPath** och **fileName** egenskaper för time series-data med hello **partitionedBy** egenskapen, Data Factory-funktioner och system variabler.</span><span class="sxs-lookup"><span data-stu-id="088fb-266">You can specify dynamic **folderPath** and **fileName** properties for time-series data with hello **partitionedBy** property, Data Factory functions, and system variables.</span></span> <span data-ttu-id="088fb-267">Mer information finns i hello [Azure Data Factory - funktioner och systemvariabler](data-factory-functions-variables.md) artikel.</span><span class="sxs-lookup"><span data-stu-id="088fb-267">For details, see hello [Azure Data Factory - functions and system variables](data-factory-functions-variables.md) article.</span></span>


<span data-ttu-id="088fb-268">I följande exempel hello `{Slice}` ersätts med hello värdet för variabeln hello Data Factory `SliceStart` i hello format (`yyyyMMddHH`).</span><span class="sxs-lookup"><span data-stu-id="088fb-268">In hello following example, `{Slice}` is replaced with hello value of hello Data Factory system variable `SliceStart` in hello format specified (`yyyyMMddHH`).</span></span> <span data-ttu-id="088fb-269">hello namn `SliceStart` refererar toohello starttiden för hello sektorn.</span><span class="sxs-lookup"><span data-stu-id="088fb-269">hello name `SliceStart` refers toohello start time of hello slice.</span></span> <span data-ttu-id="088fb-270">Hej `folderPath` egenskapen är olika för varje segment som i `wikidatagateway/wikisampledataout/2014100103` eller `wikidatagateway/wikisampledataout/2014100104`.</span><span class="sxs-lookup"><span data-stu-id="088fb-270">hello `folderPath` property is different for each slice, as in `wikidatagateway/wikisampledataout/2014100103` or `wikidatagateway/wikisampledataout/2014100104`.</span></span>

```JSON
"folderPath": "wikidatagateway/wikisampledataout/{Slice}",
"partitionedBy":
[
    { "name": "Slice", "value": { "type": "DateTime", "date": "SliceStart", "format": "yyyyMMddHH" } },
],
```

<span data-ttu-id="088fb-271">I följande exempel, hello år, månad, dag och tid för hello `SliceStart` extraheras till olika variabler som används av hello `folderPath` och `fileName` egenskaper:</span><span class="sxs-lookup"><span data-stu-id="088fb-271">In hello following example, hello year, month, day, and time of `SliceStart` are extracted into separate variables that are used by hello `folderPath` and `fileName` properties:</span></span>
```JSON
"folderPath": "wikidatagateway/wikisampledataout/{Year}/{Month}/{Day}",
"fileName": "{Hour}.csv",
"partitionedBy":
 [
    { "name": "Year", "value": { "type": "DateTime", "date": "SliceStart", "format": "yyyy" } },
    { "name": "Month", "value": { "type": "DateTime", "date": "SliceStart", "format": "MM" } },
    { "name": "Day", "value": { "type": "DateTime", "date": "SliceStart", "format": "dd" } },
    { "name": "Hour", "value": { "type": "DateTime", "date": "SliceStart", "format": "hh" } }
],
```
<span data-ttu-id="088fb-272">Mer information om tidsserier datauppsättningar schemaläggning och segment, se hello [datauppsättningar i Azure Data Factory](data-factory-create-datasets.md) och [Data Factory schemaläggning och körning av](data-factory-scheduling-and-execution.md) artiklar.</span><span class="sxs-lookup"><span data-stu-id="088fb-272">For more details on time-series datasets, scheduling, and slices, see hello [Datasets in Azure Data Factory](data-factory-create-datasets.md) and [Data Factory scheduling and execution](data-factory-scheduling-and-execution.md) articles.</span></span> 


## <a name="copy-activity-properties"></a><span data-ttu-id="088fb-273">Kopiera egenskaper för aktivitet</span><span class="sxs-lookup"><span data-stu-id="088fb-273">Copy activity properties</span></span>
<span data-ttu-id="088fb-274">En fullständig lista över avsnitt och egenskaper som är tillgängliga för att definiera aktiviteter finns hello [skapar pipelines](data-factory-create-pipelines.md) artikel.</span><span class="sxs-lookup"><span data-stu-id="088fb-274">For a full list of sections and properties available for defining activities, see hello [Creating pipelines](data-factory-create-pipelines.md) article.</span></span> <span data-ttu-id="088fb-275">Egenskaper som namn, beskrivning, ingående och utgående tabeller och principen är tillgängliga för alla typer av aktiviteter.</span><span class="sxs-lookup"><span data-stu-id="088fb-275">Properties such as name, description, input and output tables, and policy are available for all types of activities.</span></span>

<span data-ttu-id="088fb-276">Hej egenskaper som är tillgängliga i hello **typeProperties** avsnitt i en aktivitet varierar med varje aktivitetstyp.</span><span class="sxs-lookup"><span data-stu-id="088fb-276">hello properties available in hello **typeProperties** section of an activity vary with each activity type.</span></span> <span data-ttu-id="088fb-277">För en kopieringsaktiviteten varierar de beroende på hello typer av datakällor och sänkor.</span><span class="sxs-lookup"><span data-stu-id="088fb-277">For a copy activity, they vary depending on hello types of sources and sinks.</span></span>

<span data-ttu-id="088fb-278">**AzureDataLakeStoreSource** stöder hello efter egenskap i hello **typeProperties** avsnitt:</span><span class="sxs-lookup"><span data-stu-id="088fb-278">**AzureDataLakeStoreSource** supports hello following property in hello **typeProperties** section:</span></span>

| <span data-ttu-id="088fb-279">Egenskap</span><span class="sxs-lookup"><span data-stu-id="088fb-279">Property</span></span> | <span data-ttu-id="088fb-280">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="088fb-280">Description</span></span> | <span data-ttu-id="088fb-281">Tillåtna värden</span><span class="sxs-lookup"><span data-stu-id="088fb-281">Allowed values</span></span> | <span data-ttu-id="088fb-282">Krävs</span><span class="sxs-lookup"><span data-stu-id="088fb-282">Required</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="088fb-283">**rekursiva**</span><span class="sxs-lookup"><span data-stu-id="088fb-283">**recursive**</span></span> |<span data-ttu-id="088fb-284">Anger om hello data läses rekursivt från hello undermappar eller endast hello angivna mappen.</span><span class="sxs-lookup"><span data-stu-id="088fb-284">Indicates whether hello data is read recursively from hello subfolders or only from hello specified folder.</span></span> |<span data-ttu-id="088fb-285">SANT (standardvärdet), FALSKT</span><span class="sxs-lookup"><span data-stu-id="088fb-285">True (default value), False</span></span> |<span data-ttu-id="088fb-286">Nej</span><span class="sxs-lookup"><span data-stu-id="088fb-286">No</span></span> |


<span data-ttu-id="088fb-287">**AzureDataLakeStoreSink** stöder följande egenskaper i hello hello **typeProperties** avsnitt:</span><span class="sxs-lookup"><span data-stu-id="088fb-287">**AzureDataLakeStoreSink** supports hello following properties in hello **typeProperties** section:</span></span>

| <span data-ttu-id="088fb-288">Egenskap</span><span class="sxs-lookup"><span data-stu-id="088fb-288">Property</span></span> | <span data-ttu-id="088fb-289">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="088fb-289">Description</span></span> | <span data-ttu-id="088fb-290">Tillåtna värden</span><span class="sxs-lookup"><span data-stu-id="088fb-290">Allowed values</span></span> | <span data-ttu-id="088fb-291">Krävs</span><span class="sxs-lookup"><span data-stu-id="088fb-291">Required</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="088fb-292">**copyBehavior**</span><span class="sxs-lookup"><span data-stu-id="088fb-292">**copyBehavior**</span></span> |<span data-ttu-id="088fb-293">Anger hello kopiera beteende.</span><span class="sxs-lookup"><span data-stu-id="088fb-293">Specifies hello copy behavior.</span></span> |<span data-ttu-id="088fb-294"><b>PreserveHierarchy</b>: bevarar hello filen hierarki i hello målmappen.</span><span class="sxs-lookup"><span data-stu-id="088fb-294"><b>PreserveHierarchy</b>: Preserves hello file hierarchy in hello target folder.</span></span> <span data-ttu-id="088fb-295">hello relativa sökvägen till källmappen för filen toosource är identiska toohello relativa sökvägen till filen tootarget mapp.</span><span class="sxs-lookup"><span data-stu-id="088fb-295">hello relative path of source file toosource folder is identical toohello relative path of target file tootarget folder.</span></span><br/><br/><span data-ttu-id="088fb-296"><b>FlattenHierarchy</b>: alla filer från källmappen hello skapas i hello första nivån i hello målmappen.</span><span class="sxs-lookup"><span data-stu-id="088fb-296"><b>FlattenHierarchy</b>: All files from hello source folder are created in hello first level of hello target folder.</span></span> <span data-ttu-id="088fb-297">hello med filer skapas med automatiskt genererade namn.</span><span class="sxs-lookup"><span data-stu-id="088fb-297">hello target files are created with autogenerated names.</span></span><br/><br/><span data-ttu-id="088fb-298"><b>MergeFiles</b>: sammanfogar alla filer från hello källfil mappen tooone.</span><span class="sxs-lookup"><span data-stu-id="088fb-298"><b>MergeFiles</b>: Merges all files from hello source folder tooone file.</span></span> <span data-ttu-id="088fb-299">Om hello fil-eller blob anges är hello kopplade filnamnet hello angivet namn.</span><span class="sxs-lookup"><span data-stu-id="088fb-299">If hello file or blob name is specified, hello merged file name is hello specified name.</span></span> <span data-ttu-id="088fb-300">Annars är hello filnamn automatiskt genererade.</span><span class="sxs-lookup"><span data-stu-id="088fb-300">Otherwise, hello file name is autogenerated.</span></span> |<span data-ttu-id="088fb-301">Nej</span><span class="sxs-lookup"><span data-stu-id="088fb-301">No</span></span> |

### <a name="recursive-and-copybehavior-examples"></a><span data-ttu-id="088fb-302">rekursiva och copyBehavior exempel</span><span class="sxs-lookup"><span data-stu-id="088fb-302">recursive and copyBehavior examples</span></span>
<span data-ttu-id="088fb-303">Det här avsnittet beskrivs hello resultatet av hello kopia för olika kombinationer av värden rekursiv och copyBehavior.</span><span class="sxs-lookup"><span data-stu-id="088fb-303">This section describes hello resulting behavior of hello Copy operation for different combinations of recursive and copyBehavior values.</span></span>

| <span data-ttu-id="088fb-304">Rekursiva</span><span class="sxs-lookup"><span data-stu-id="088fb-304">recursive</span></span> | <span data-ttu-id="088fb-305">copyBehavior</span><span class="sxs-lookup"><span data-stu-id="088fb-305">copyBehavior</span></span> | <span data-ttu-id="088fb-306">Resultatet</span><span class="sxs-lookup"><span data-stu-id="088fb-306">Resulting behavior</span></span> |
| --- | --- | --- |
| <span data-ttu-id="088fb-307">SANT</span><span class="sxs-lookup"><span data-stu-id="088fb-307">true</span></span> |<span data-ttu-id="088fb-308">preserveHierarchy</span><span class="sxs-lookup"><span data-stu-id="088fb-308">preserveHierarchy</span></span> |<span data-ttu-id="088fb-309">För en källmapp Mapp1 med hello följande struktur:</span><span class="sxs-lookup"><span data-stu-id="088fb-309">For a source folder Folder1 with hello following structure:</span></span> <br/><br/><span data-ttu-id="088fb-310">Mapp1</span><span class="sxs-lookup"><span data-stu-id="088fb-310">Folder1</span></span><br/><span data-ttu-id="088fb-311">&nbsp;&nbsp;&nbsp;&nbsp;File1</span><span class="sxs-lookup"><span data-stu-id="088fb-311">&nbsp;&nbsp;&nbsp;&nbsp;File1</span></span><br/><span data-ttu-id="088fb-312">&nbsp;&nbsp;&nbsp;&nbsp;Fil2</span><span class="sxs-lookup"><span data-stu-id="088fb-312">&nbsp;&nbsp;&nbsp;&nbsp;File2</span></span><br/><span data-ttu-id="088fb-313">&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1</span><span class="sxs-lookup"><span data-stu-id="088fb-313">&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1</span></span><br/><span data-ttu-id="088fb-314">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Fil3</span><span class="sxs-lookup"><span data-stu-id="088fb-314">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File3</span></span><br/><span data-ttu-id="088fb-315">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4</span><span class="sxs-lookup"><span data-stu-id="088fb-315">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4</span></span><br/><span data-ttu-id="088fb-316">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5</span><span class="sxs-lookup"><span data-stu-id="088fb-316">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5</span></span><br/><br/><span data-ttu-id="088fb-317">hello målmappen Mapp1 skapas med hello samma struktur som hello källa</span><span class="sxs-lookup"><span data-stu-id="088fb-317">hello target folder Folder1 is created with hello same structure as hello source</span></span><br/><br/><span data-ttu-id="088fb-318">Mapp1</span><span class="sxs-lookup"><span data-stu-id="088fb-318">Folder1</span></span><br/><span data-ttu-id="088fb-319">&nbsp;&nbsp;&nbsp;&nbsp;File1</span><span class="sxs-lookup"><span data-stu-id="088fb-319">&nbsp;&nbsp;&nbsp;&nbsp;File1</span></span><br/><span data-ttu-id="088fb-320">&nbsp;&nbsp;&nbsp;&nbsp;Fil2</span><span class="sxs-lookup"><span data-stu-id="088fb-320">&nbsp;&nbsp;&nbsp;&nbsp;File2</span></span><br/><span data-ttu-id="088fb-321">&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1</span><span class="sxs-lookup"><span data-stu-id="088fb-321">&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1</span></span><br/><span data-ttu-id="088fb-322">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Fil3</span><span class="sxs-lookup"><span data-stu-id="088fb-322">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File3</span></span><br/><span data-ttu-id="088fb-323">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4</span><span class="sxs-lookup"><span data-stu-id="088fb-323">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4</span></span><br/><span data-ttu-id="088fb-324">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5.</span><span class="sxs-lookup"><span data-stu-id="088fb-324">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5.</span></span> |
| <span data-ttu-id="088fb-325">SANT</span><span class="sxs-lookup"><span data-stu-id="088fb-325">true</span></span> |<span data-ttu-id="088fb-326">flattenHierarchy</span><span class="sxs-lookup"><span data-stu-id="088fb-326">flattenHierarchy</span></span> |<span data-ttu-id="088fb-327">För en källmapp Mapp1 med hello följande struktur:</span><span class="sxs-lookup"><span data-stu-id="088fb-327">For a source folder Folder1 with hello following structure:</span></span> <br/><br/><span data-ttu-id="088fb-328">Mapp1</span><span class="sxs-lookup"><span data-stu-id="088fb-328">Folder1</span></span><br/><span data-ttu-id="088fb-329">&nbsp;&nbsp;&nbsp;&nbsp;File1</span><span class="sxs-lookup"><span data-stu-id="088fb-329">&nbsp;&nbsp;&nbsp;&nbsp;File1</span></span><br/><span data-ttu-id="088fb-330">&nbsp;&nbsp;&nbsp;&nbsp;Fil2</span><span class="sxs-lookup"><span data-stu-id="088fb-330">&nbsp;&nbsp;&nbsp;&nbsp;File2</span></span><br/><span data-ttu-id="088fb-331">&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1</span><span class="sxs-lookup"><span data-stu-id="088fb-331">&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1</span></span><br/><span data-ttu-id="088fb-332">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Fil3</span><span class="sxs-lookup"><span data-stu-id="088fb-332">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File3</span></span><br/><span data-ttu-id="088fb-333">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4</span><span class="sxs-lookup"><span data-stu-id="088fb-333">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4</span></span><br/><span data-ttu-id="088fb-334">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5</span><span class="sxs-lookup"><span data-stu-id="088fb-334">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5</span></span><br/><br/><span data-ttu-id="088fb-335">hello mål Mapp1 skapas med följande struktur hello:</span><span class="sxs-lookup"><span data-stu-id="088fb-335">hello target Folder1 is created with hello following structure:</span></span> <br/><br/><span data-ttu-id="088fb-336">Mapp1</span><span class="sxs-lookup"><span data-stu-id="088fb-336">Folder1</span></span><br/><span data-ttu-id="088fb-337">&nbsp;&nbsp;&nbsp;&nbsp;automatiskt genererade namnet på File1</span><span class="sxs-lookup"><span data-stu-id="088fb-337">&nbsp;&nbsp;&nbsp;&nbsp;auto-generated name for File1</span></span><br/><span data-ttu-id="088fb-338">&nbsp;&nbsp;&nbsp;&nbsp;automatiskt genererat namn för fil2</span><span class="sxs-lookup"><span data-stu-id="088fb-338">&nbsp;&nbsp;&nbsp;&nbsp;auto-generated name for File2</span></span><br/><span data-ttu-id="088fb-339">&nbsp;&nbsp;&nbsp;&nbsp;automatiskt genererat namn för fil3</span><span class="sxs-lookup"><span data-stu-id="088fb-339">&nbsp;&nbsp;&nbsp;&nbsp;auto-generated name for File3</span></span><br/><span data-ttu-id="088fb-340">&nbsp;&nbsp;&nbsp;&nbsp;automatiskt genererat namn för File4</span><span class="sxs-lookup"><span data-stu-id="088fb-340">&nbsp;&nbsp;&nbsp;&nbsp;auto-generated name for File4</span></span><br/><span data-ttu-id="088fb-341">&nbsp;&nbsp;&nbsp;&nbsp;automatiskt genererat namn för File5</span><span class="sxs-lookup"><span data-stu-id="088fb-341">&nbsp;&nbsp;&nbsp;&nbsp;auto-generated name for File5</span></span> |
| <span data-ttu-id="088fb-342">SANT</span><span class="sxs-lookup"><span data-stu-id="088fb-342">true</span></span> |<span data-ttu-id="088fb-343">mergeFiles</span><span class="sxs-lookup"><span data-stu-id="088fb-343">mergeFiles</span></span> |<span data-ttu-id="088fb-344">För en källmapp Mapp1 med hello följande struktur:</span><span class="sxs-lookup"><span data-stu-id="088fb-344">For a source folder Folder1 with hello following structure:</span></span> <br/><br/><span data-ttu-id="088fb-345">Mapp1</span><span class="sxs-lookup"><span data-stu-id="088fb-345">Folder1</span></span><br/><span data-ttu-id="088fb-346">&nbsp;&nbsp;&nbsp;&nbsp;File1</span><span class="sxs-lookup"><span data-stu-id="088fb-346">&nbsp;&nbsp;&nbsp;&nbsp;File1</span></span><br/><span data-ttu-id="088fb-347">&nbsp;&nbsp;&nbsp;&nbsp;Fil2</span><span class="sxs-lookup"><span data-stu-id="088fb-347">&nbsp;&nbsp;&nbsp;&nbsp;File2</span></span><br/><span data-ttu-id="088fb-348">&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1</span><span class="sxs-lookup"><span data-stu-id="088fb-348">&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1</span></span><br/><span data-ttu-id="088fb-349">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Fil3</span><span class="sxs-lookup"><span data-stu-id="088fb-349">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File3</span></span><br/><span data-ttu-id="088fb-350">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4</span><span class="sxs-lookup"><span data-stu-id="088fb-350">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4</span></span><br/><span data-ttu-id="088fb-351">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5</span><span class="sxs-lookup"><span data-stu-id="088fb-351">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5</span></span><br/><br/><span data-ttu-id="088fb-352">hello mål Mapp1 skapas med följande struktur hello:</span><span class="sxs-lookup"><span data-stu-id="088fb-352">hello target Folder1 is created with hello following structure:</span></span> <br/><br/><span data-ttu-id="088fb-353">Mapp1</span><span class="sxs-lookup"><span data-stu-id="088fb-353">Folder1</span></span><br/><span data-ttu-id="088fb-354">&nbsp;&nbsp;&nbsp;&nbsp;Fil1 + fil2 + fil3 + File4 + filen 5 innehållet slås samman till en fil med automatiskt genererade namnet</span><span class="sxs-lookup"><span data-stu-id="088fb-354">&nbsp;&nbsp;&nbsp;&nbsp;File1 + File2 + File3 + File4 + File 5 contents are merged into one file with auto-generated file name</span></span> |
| <span data-ttu-id="088fb-355">FALSKT</span><span class="sxs-lookup"><span data-stu-id="088fb-355">false</span></span> |<span data-ttu-id="088fb-356">preserveHierarchy</span><span class="sxs-lookup"><span data-stu-id="088fb-356">preserveHierarchy</span></span> |<span data-ttu-id="088fb-357">För en källmapp Mapp1 med hello följande struktur:</span><span class="sxs-lookup"><span data-stu-id="088fb-357">For a source folder Folder1 with hello following structure:</span></span> <br/><br/><span data-ttu-id="088fb-358">Mapp1</span><span class="sxs-lookup"><span data-stu-id="088fb-358">Folder1</span></span><br/><span data-ttu-id="088fb-359">&nbsp;&nbsp;&nbsp;&nbsp;File1</span><span class="sxs-lookup"><span data-stu-id="088fb-359">&nbsp;&nbsp;&nbsp;&nbsp;File1</span></span><br/><span data-ttu-id="088fb-360">&nbsp;&nbsp;&nbsp;&nbsp;Fil2</span><span class="sxs-lookup"><span data-stu-id="088fb-360">&nbsp;&nbsp;&nbsp;&nbsp;File2</span></span><br/><span data-ttu-id="088fb-361">&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1</span><span class="sxs-lookup"><span data-stu-id="088fb-361">&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1</span></span><br/><span data-ttu-id="088fb-362">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Fil3</span><span class="sxs-lookup"><span data-stu-id="088fb-362">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File3</span></span><br/><span data-ttu-id="088fb-363">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4</span><span class="sxs-lookup"><span data-stu-id="088fb-363">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4</span></span><br/><span data-ttu-id="088fb-364">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5</span><span class="sxs-lookup"><span data-stu-id="088fb-364">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5</span></span><br/><br/><span data-ttu-id="088fb-365">hello målmappen Mapp1 skapas med följande struktur hello</span><span class="sxs-lookup"><span data-stu-id="088fb-365">hello target folder Folder1 is created with hello following structure</span></span><br/><br/><span data-ttu-id="088fb-366">Mapp1</span><span class="sxs-lookup"><span data-stu-id="088fb-366">Folder1</span></span><br/><span data-ttu-id="088fb-367">&nbsp;&nbsp;&nbsp;&nbsp;File1</span><span class="sxs-lookup"><span data-stu-id="088fb-367">&nbsp;&nbsp;&nbsp;&nbsp;File1</span></span><br/><span data-ttu-id="088fb-368">&nbsp;&nbsp;&nbsp;&nbsp;Fil2</span><span class="sxs-lookup"><span data-stu-id="088fb-368">&nbsp;&nbsp;&nbsp;&nbsp;File2</span></span><br/><br/><br/><span data-ttu-id="088fb-369">Subfolder1 med fil3, File4 och File5 har inte plockats.</span><span class="sxs-lookup"><span data-stu-id="088fb-369">Subfolder1 with File3, File4, and File5 are not picked up.</span></span> |
| <span data-ttu-id="088fb-370">FALSKT</span><span class="sxs-lookup"><span data-stu-id="088fb-370">false</span></span> |<span data-ttu-id="088fb-371">flattenHierarchy</span><span class="sxs-lookup"><span data-stu-id="088fb-371">flattenHierarchy</span></span> |<span data-ttu-id="088fb-372">För en källmapp Mapp1 med hello följande struktur:</span><span class="sxs-lookup"><span data-stu-id="088fb-372">For a source folder Folder1 with hello following structure:</span></span><br/><br/><span data-ttu-id="088fb-373">Mapp1</span><span class="sxs-lookup"><span data-stu-id="088fb-373">Folder1</span></span><br/><span data-ttu-id="088fb-374">&nbsp;&nbsp;&nbsp;&nbsp;File1</span><span class="sxs-lookup"><span data-stu-id="088fb-374">&nbsp;&nbsp;&nbsp;&nbsp;File1</span></span><br/><span data-ttu-id="088fb-375">&nbsp;&nbsp;&nbsp;&nbsp;Fil2</span><span class="sxs-lookup"><span data-stu-id="088fb-375">&nbsp;&nbsp;&nbsp;&nbsp;File2</span></span><br/><span data-ttu-id="088fb-376">&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1</span><span class="sxs-lookup"><span data-stu-id="088fb-376">&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1</span></span><br/><span data-ttu-id="088fb-377">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Fil3</span><span class="sxs-lookup"><span data-stu-id="088fb-377">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File3</span></span><br/><span data-ttu-id="088fb-378">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4</span><span class="sxs-lookup"><span data-stu-id="088fb-378">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4</span></span><br/><span data-ttu-id="088fb-379">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5</span><span class="sxs-lookup"><span data-stu-id="088fb-379">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5</span></span><br/><br/><span data-ttu-id="088fb-380">hello målmappen Mapp1 skapas med följande struktur hello</span><span class="sxs-lookup"><span data-stu-id="088fb-380">hello target folder Folder1 is created with hello following structure</span></span><br/><br/><span data-ttu-id="088fb-381">Mapp1</span><span class="sxs-lookup"><span data-stu-id="088fb-381">Folder1</span></span><br/><span data-ttu-id="088fb-382">&nbsp;&nbsp;&nbsp;&nbsp;automatiskt genererade namnet på File1</span><span class="sxs-lookup"><span data-stu-id="088fb-382">&nbsp;&nbsp;&nbsp;&nbsp;auto-generated name for File1</span></span><br/><span data-ttu-id="088fb-383">&nbsp;&nbsp;&nbsp;&nbsp;automatiskt genererat namn för fil2</span><span class="sxs-lookup"><span data-stu-id="088fb-383">&nbsp;&nbsp;&nbsp;&nbsp;auto-generated name for File2</span></span><br/><br/><br/><span data-ttu-id="088fb-384">Subfolder1 med fil3, File4 och File5 har inte plockats.</span><span class="sxs-lookup"><span data-stu-id="088fb-384">Subfolder1 with File3, File4, and File5 are not picked up.</span></span> |
| <span data-ttu-id="088fb-385">FALSKT</span><span class="sxs-lookup"><span data-stu-id="088fb-385">false</span></span> |<span data-ttu-id="088fb-386">mergeFiles</span><span class="sxs-lookup"><span data-stu-id="088fb-386">mergeFiles</span></span> |<span data-ttu-id="088fb-387">För en källmapp Mapp1 med hello följande struktur:</span><span class="sxs-lookup"><span data-stu-id="088fb-387">For a source folder Folder1 with hello following structure:</span></span><br/><br/><span data-ttu-id="088fb-388">Mapp1</span><span class="sxs-lookup"><span data-stu-id="088fb-388">Folder1</span></span><br/><span data-ttu-id="088fb-389">&nbsp;&nbsp;&nbsp;&nbsp;File1</span><span class="sxs-lookup"><span data-stu-id="088fb-389">&nbsp;&nbsp;&nbsp;&nbsp;File1</span></span><br/><span data-ttu-id="088fb-390">&nbsp;&nbsp;&nbsp;&nbsp;Fil2</span><span class="sxs-lookup"><span data-stu-id="088fb-390">&nbsp;&nbsp;&nbsp;&nbsp;File2</span></span><br/><span data-ttu-id="088fb-391">&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1</span><span class="sxs-lookup"><span data-stu-id="088fb-391">&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1</span></span><br/><span data-ttu-id="088fb-392">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Fil3</span><span class="sxs-lookup"><span data-stu-id="088fb-392">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File3</span></span><br/><span data-ttu-id="088fb-393">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4</span><span class="sxs-lookup"><span data-stu-id="088fb-393">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4</span></span><br/><span data-ttu-id="088fb-394">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5</span><span class="sxs-lookup"><span data-stu-id="088fb-394">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5</span></span><br/><br/><span data-ttu-id="088fb-395">hello målmappen Mapp1 skapas med följande struktur hello</span><span class="sxs-lookup"><span data-stu-id="088fb-395">hello target folder Folder1 is created with hello following structure</span></span><br/><br/><span data-ttu-id="088fb-396">Mapp1</span><span class="sxs-lookup"><span data-stu-id="088fb-396">Folder1</span></span><br/><span data-ttu-id="088fb-397">&nbsp;&nbsp;&nbsp;&nbsp;Fil1 + fil2 innehållet slås samman till en fil med automatiskt genererade namnet.</span><span class="sxs-lookup"><span data-stu-id="088fb-397">&nbsp;&nbsp;&nbsp;&nbsp;File1 + File2 contents are merged into one file with auto-generated file name.</span></span> <span data-ttu-id="088fb-398">automatiskt genererade namnet på File1</span><span class="sxs-lookup"><span data-stu-id="088fb-398">auto-generated name for File1</span></span><br/><br/><span data-ttu-id="088fb-399">Subfolder1 med fil3, File4 och File5 har inte plockats.</span><span class="sxs-lookup"><span data-stu-id="088fb-399">Subfolder1 with File3, File4, and File5 are not picked up.</span></span> |

## <a name="supported-file-and-compression-formats"></a><span data-ttu-id="088fb-400">Fil- och komprimering format som stöds</span><span class="sxs-lookup"><span data-stu-id="088fb-400">Supported file and compression formats</span></span>
<span data-ttu-id="088fb-401">Mer information finns i hello [format och komprimering i Azure Data Factory](data-factory-supported-file-and-compression-formats.md) artikel.</span><span class="sxs-lookup"><span data-stu-id="088fb-401">For details, see hello [File and compression formats in Azure Data Factory](data-factory-supported-file-and-compression-formats.md) article.</span></span>

## <a name="json-examples-for-copying-data-tooand-from-data-lake-store"></a><span data-ttu-id="088fb-402">JSON-exempel för att kopiera data tooand från Data Lake Store</span><span class="sxs-lookup"><span data-stu-id="088fb-402">JSON examples for copying data tooand from Data Lake Store</span></span>
<span data-ttu-id="088fb-403">följande exempel hello ger exempel JSON-definitioner.</span><span class="sxs-lookup"><span data-stu-id="088fb-403">hello following examples provide sample JSON definitions.</span></span> <span data-ttu-id="088fb-404">Du kan använda dessa exempel definitioner toocreate en pipeline med hjälp av hello [Azure-portalen](data-factory-copy-activity-tutorial-using-azure-portal.md), [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md), eller [Azure PowerShell](data-factory-copy-activity-tutorial-using-powershell.md).</span><span class="sxs-lookup"><span data-stu-id="088fb-404">You can use these sample definitions toocreate a pipeline by using hello [Azure portal](data-factory-copy-activity-tutorial-using-azure-portal.md), [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md), or [Azure PowerShell](data-factory-copy-activity-tutorial-using-powershell.md).</span></span> <span data-ttu-id="088fb-405">Hej exempel visas hur toocopy data tooand från Data Lake Store och Azure Blob storage.</span><span class="sxs-lookup"><span data-stu-id="088fb-405">hello examples show how toocopy data tooand from Data Lake Store and Azure Blob storage.</span></span> <span data-ttu-id="088fb-406">Dock datan kan kopieras _direkt_ från någon av hello källor tooany av sänkor hello stöds.</span><span class="sxs-lookup"><span data-stu-id="088fb-406">However, data can be copied _directly_ from any of hello sources tooany of hello supported sinks.</span></span> <span data-ttu-id="088fb-407">Mer information finns i avsnittet hello ”stöds datalager och format” i hello [flytta data med hjälp av Kopieringsaktiviteten](data-factory-data-movement-activities.md) artikel.</span><span class="sxs-lookup"><span data-stu-id="088fb-407">For more information, see hello section "Supported data stores and formats" in hello [Move data by using Copy Activity](data-factory-data-movement-activities.md) article.</span></span>  

### <a name="example-copy-data-from-azure-blob-storage-tooazure-data-lake-store"></a><span data-ttu-id="088fb-408">Exempel: Kopiera data från Azure Blob Storage tooAzure Data Lake Store</span><span class="sxs-lookup"><span data-stu-id="088fb-408">Example: Copy data from Azure Blob Storage tooAzure Data Lake Store</span></span>
<span data-ttu-id="088fb-409">hello exempelkod i det här avsnittet visas:</span><span class="sxs-lookup"><span data-stu-id="088fb-409">hello example code in this section shows:</span></span>

* <span data-ttu-id="088fb-410">En länkad tjänst av typen [AzureStorage](data-factory-azure-blob-connector.md#linked-service-properties).</span><span class="sxs-lookup"><span data-stu-id="088fb-410">A linked service of type [AzureStorage](data-factory-azure-blob-connector.md#linked-service-properties).</span></span>
* <span data-ttu-id="088fb-411">En länkad tjänst av typen [AzureDataLakeStore](#linked-service-properties).</span><span class="sxs-lookup"><span data-stu-id="088fb-411">A linked service of type [AzureDataLakeStore](#linked-service-properties).</span></span>
* <span data-ttu-id="088fb-412">Indata [dataset](data-factory-create-datasets.md) av typen [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties).</span><span class="sxs-lookup"><span data-stu-id="088fb-412">An input [dataset](data-factory-create-datasets.md) of type [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties).</span></span>
* <span data-ttu-id="088fb-413">Utdata [dataset](data-factory-create-datasets.md) av typen [AzureDataLakeStore](#dataset-properties).</span><span class="sxs-lookup"><span data-stu-id="088fb-413">An output [dataset](data-factory-create-datasets.md) of type [AzureDataLakeStore](#dataset-properties).</span></span>
* <span data-ttu-id="088fb-414">En [pipeline](data-factory-create-pipelines.md) med en kopia-aktivitet som använder [BlobSource](data-factory-azure-blob-connector.md#copy-activity-properties) och [AzureDataLakeStoreSink](#copy-activity-properties).</span><span class="sxs-lookup"><span data-stu-id="088fb-414">A [pipeline](data-factory-create-pipelines.md) with a copy activity that uses [BlobSource](data-factory-azure-blob-connector.md#copy-activity-properties) and [AzureDataLakeStoreSink](#copy-activity-properties).</span></span>

<span data-ttu-id="088fb-415">hello exemplen visar hur tidsserier data från Azure Blob Storage är kopieras tooData Datasjölager varje timme.</span><span class="sxs-lookup"><span data-stu-id="088fb-415">hello examples show how time-series data from Azure Blob Storage is copied tooData Lake Store every hour.</span></span> 

<span data-ttu-id="088fb-416">**Länkad Azure Storage-tjänst**</span><span class="sxs-lookup"><span data-stu-id="088fb-416">**Azure Storage linked service**</span></span>

```JSON
{
  "name": "StorageLinkedService",
  "properties": {
    "type": "AzureStorage",
    "typeProperties": {
      "connectionString": "DefaultEndpointsProtocol=https;AccountName=<accountname>;AccountKey=<accountkey>"
    }
  }
}
```

<span data-ttu-id="088fb-417">**Azure Data Lake Store länkade tjänsten**</span><span class="sxs-lookup"><span data-stu-id="088fb-417">**Azure Data Lake Store linked service**</span></span>

```JSON
{
    "name": "AzureDataLakeStoreLinkedService",
    "properties": {
        "type": "AzureDataLakeStore",
        "typeProperties": {
            "dataLakeStoreUri": "https://<accountname>.azuredatalakestore.net/webhdfs/v1",
            "servicePrincipalId": "<service principal id>",
            "servicePrincipalKey": "<service principal key>",
            "tenant": "<tenant info, e.g. microsoft.onmicrosoft.com>",
            "subscriptionId": "<subscription of ADLS>",
            "resourceGroupName": "<resource group of ADLS>"
        }
    }
}
```

> [!NOTE]
> <span data-ttu-id="088fb-418">Konfigurationsinformation finns hello [länkade tjänstegenskaper](#linked-service-properties) avsnitt.</span><span class="sxs-lookup"><span data-stu-id="088fb-418">For configuration details, see hello [Linked service properties](#linked-service-properties) section.</span></span>
>

<span data-ttu-id="088fb-419">**Indatauppsättning för Azure-blobb**</span><span class="sxs-lookup"><span data-stu-id="088fb-419">**Azure blob input dataset**</span></span>

<span data-ttu-id="088fb-420">I följande exempel hello, data hämtas från en ny blob varje timme (`"frequency": "Hour", "interval": 1`).</span><span class="sxs-lookup"><span data-stu-id="088fb-420">In hello following example, data is picked up from a new blob every hour (`"frequency": "Hour", "interval": 1`).</span></span> <span data-ttu-id="088fb-421">hello mappen sökvägen och filnamnet för hello blob utvärderas dynamiskt baserat på hello starttiden för hello-segment som bearbetas.</span><span class="sxs-lookup"><span data-stu-id="088fb-421">hello folder path and file name for hello blob are dynamically evaluated based on hello start time of hello slice that is being processed.</span></span> <span data-ttu-id="088fb-422">hello mappsökväg använder hello år, månad och dagsdelen av hello starttid.</span><span class="sxs-lookup"><span data-stu-id="088fb-422">hello folder path uses hello year, month, and day portion of hello start time.</span></span> <span data-ttu-id="088fb-423">hello filnamn använder hello timme som del av hello starttid.</span><span class="sxs-lookup"><span data-stu-id="088fb-423">hello file name uses hello hour portion of hello start time.</span></span> <span data-ttu-id="088fb-424">Hej `"external": true` inställningen informerar hello Data Factory-tjänsten hello tabellen är externa toohello data factory och inte tillverkas av en aktivitet i hello data factory.</span><span class="sxs-lookup"><span data-stu-id="088fb-424">hello `"external": true` setting informs hello Data Factory service that hello table is external toohello data factory and is not produced by an activity in hello data factory.</span></span>

```JSON
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
    "external": true,
    "availability": {
      "frequency": "Hour",
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

<span data-ttu-id="088fb-425">**Azure Data Lake Store utdatauppsättningen**</span><span class="sxs-lookup"><span data-stu-id="088fb-425">**Azure Data Lake Store output dataset**</span></span>

<span data-ttu-id="088fb-426">följande exempel kopierar tooData Datasjölager hello.</span><span class="sxs-lookup"><span data-stu-id="088fb-426">hello following example copies data tooData Lake Store.</span></span> <span data-ttu-id="088fb-427">Nya data kopieras tooData Datasjölager varje timme.</span><span class="sxs-lookup"><span data-stu-id="088fb-427">New data is copied tooData Lake Store every hour.</span></span>

```JSON
{
    "name": "AzureDataLakeStoreOutput",
      "properties": {
        "type": "AzureDataLakeStore",
        "linkedServiceName": "AzureDataLakeStoreLinkedService",
        "typeProperties": {
            "folderPath": "datalake/output/"
        },
        "availability": {
              "frequency": "Hour",
              "interval": 1
        }
      }
}
```


<span data-ttu-id="088fb-428">**Kopiera aktivitet i en pipeline med en blob-källa och mottagare ett Data Lake Store**</span><span class="sxs-lookup"><span data-stu-id="088fb-428">**Copy activity in a pipeline with a blob source and a Data Lake Store sink**</span></span>

<span data-ttu-id="088fb-429">I följande exempel hello, hello pipelinen innehåller en kopia-aktivitet som är konfigurerad toouse hello inkommande och utgående datauppsättningar.</span><span class="sxs-lookup"><span data-stu-id="088fb-429">In hello following example, hello pipeline contains a copy activity that is configured toouse hello input and output datasets.</span></span> <span data-ttu-id="088fb-430">Hej kopieringsaktiviteten är schemalagda toorun varje timme.</span><span class="sxs-lookup"><span data-stu-id="088fb-430">hello copy activity is scheduled toorun every hour.</span></span> <span data-ttu-id="088fb-431">I hello pipeline JSON-definitionen hello `source` typ har angetts för`BlobSource`, och hello `sink` typ har angetts för`AzureDataLakeStoreSink`.</span><span class="sxs-lookup"><span data-stu-id="088fb-431">In hello pipeline JSON definition, hello `source` type is set too`BlobSource`, and hello `sink` type is set too`AzureDataLakeStoreSink`.</span></span>

```json
{  
    "name":"SamplePipeline",
    "properties":
    {  
        "start":"2014-06-01T18:00:00",
        "end":"2014-06-01T19:00:00",
        "description":"pipeline with copy activity",
        "activities":
        [  
              {
                "name": "AzureBlobtoDataLake",
                "description": "Copy Activity",
                "type": "Copy",
                "inputs": [
                  {
                    "name": "AzureBlobInput"
                  }
                ],
                "outputs": [
                  {
                    "name": "AzureDataLakeStoreOutput"
                  }
                ],
                "typeProperties": {
                    "source": {
                        "type": "BlobSource"
                      },
                      "sink": {
                        "type": "AzureDataLakeStoreSink"
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

### <a name="example-copy-data-from-azure-data-lake-store-tooan-azure-blob"></a><span data-ttu-id="088fb-432">Exempel: Kopiera data från Azure Data Lake Store tooan Azure blob</span><span class="sxs-lookup"><span data-stu-id="088fb-432">Example: Copy data from Azure Data Lake Store tooan Azure blob</span></span>
<span data-ttu-id="088fb-433">hello exempelkod i det här avsnittet visas:</span><span class="sxs-lookup"><span data-stu-id="088fb-433">hello example code in this section shows:</span></span>

* <span data-ttu-id="088fb-434">En länkad tjänst av typen [AzureDataLakeStore](#linked-service-properties).</span><span class="sxs-lookup"><span data-stu-id="088fb-434">A linked service of type [AzureDataLakeStore](#linked-service-properties).</span></span>
* <span data-ttu-id="088fb-435">En länkad tjänst av typen [AzureStorage](data-factory-azure-blob-connector.md#linked-service-properties).</span><span class="sxs-lookup"><span data-stu-id="088fb-435">A linked service of type [AzureStorage](data-factory-azure-blob-connector.md#linked-service-properties).</span></span>
* <span data-ttu-id="088fb-436">Indata [dataset](data-factory-create-datasets.md) av typen [AzureDataLakeStore](#dataset-properties).</span><span class="sxs-lookup"><span data-stu-id="088fb-436">An input [dataset](data-factory-create-datasets.md) of type [AzureDataLakeStore](#dataset-properties).</span></span>
* <span data-ttu-id="088fb-437">Utdata [dataset](data-factory-create-datasets.md) av typen [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties).</span><span class="sxs-lookup"><span data-stu-id="088fb-437">An output [dataset](data-factory-create-datasets.md) of type [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties).</span></span>
* <span data-ttu-id="088fb-438">En [pipeline](data-factory-create-pipelines.md) med en kopia-aktivitet som använder [AzureDataLakeStoreSource](#copy-activity-properties) och [BlobSink](data-factory-azure-blob-connector.md#copy-activity-properties).</span><span class="sxs-lookup"><span data-stu-id="088fb-438">A [pipeline](data-factory-create-pipelines.md) with a copy activity that uses [AzureDataLakeStoreSource](#copy-activity-properties) and [BlobSink](data-factory-azure-blob-connector.md#copy-activity-properties).</span></span>

<span data-ttu-id="088fb-439">hello kod kopierar time series-data från Data Lake Store tooan Azure blob varje timme.</span><span class="sxs-lookup"><span data-stu-id="088fb-439">hello code copies time-series data from Data Lake Store tooan Azure blob every hour.</span></span> 

<span data-ttu-id="088fb-440">**Azure Data Lake Store länkade tjänsten**</span><span class="sxs-lookup"><span data-stu-id="088fb-440">**Azure Data Lake Store linked service**</span></span>

```json
{
    "name": "AzureDataLakeStoreLinkedService",
    "properties": {
        "type": "AzureDataLakeStore",
        "typeProperties": {
            "dataLakeStoreUri": "https://<accountname>.azuredatalakestore.net/webhdfs/v1",
            "servicePrincipalId": "<service principal id>",
            "servicePrincipalKey": "<service principal key>",
            "tenant": "<tenant info, e.g. microsoft.onmicrosoft.com>"
        }
    }
}
```

> [!NOTE]
> <span data-ttu-id="088fb-441">Konfigurationsinformation finns hello [länkade tjänstegenskaper](#linked-service-properties) avsnitt.</span><span class="sxs-lookup"><span data-stu-id="088fb-441">For configuration details, see hello [Linked service properties](#linked-service-properties) section.</span></span>
>

<span data-ttu-id="088fb-442">**Länkad Azure Storage-tjänst**</span><span class="sxs-lookup"><span data-stu-id="088fb-442">**Azure Storage linked service**</span></span>

```JSON
{
  "name": "StorageLinkedService",
  "properties": {
    "type": "AzureStorage",
    "typeProperties": {
      "connectionString": "DefaultEndpointsProtocol=https;AccountName=<accountname>;AccountKey=<accountkey>"
    }
  }
}
```
<span data-ttu-id="088fb-443">**Azure Data Lake-indatauppsättningen**</span><span class="sxs-lookup"><span data-stu-id="088fb-443">**Azure Data Lake input dataset**</span></span>

<span data-ttu-id="088fb-444">I det här exemplet anger `"external"` för`true` informerar hello Data Factory-tjänsten hello tabellen är externa toohello data factory och inte tillverkas av en aktivitet i hello data factory.</span><span class="sxs-lookup"><span data-stu-id="088fb-444">In this example, setting `"external"` too`true` informs hello Data Factory service that hello table is external toohello data factory and is not produced by an activity in hello data factory.</span></span>

```json
{
    "name": "AzureDataLakeStoreInput",
      "properties":
    {
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
        "external": true,
        "availability": {
            "frequency": "Hour",
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
<span data-ttu-id="088fb-445">**Utdatauppsättning för Azure-blobb**</span><span class="sxs-lookup"><span data-stu-id="088fb-445">**Azure blob output dataset**</span></span>

<span data-ttu-id="088fb-446">I följande exempel hello, skrivs data tooa nya blob varje timme (`"frequency": "Hour", "interval": 1`).</span><span class="sxs-lookup"><span data-stu-id="088fb-446">In hello following example, data is written tooa new blob every hour (`"frequency": "Hour", "interval": 1`).</span></span> <span data-ttu-id="088fb-447">hello mappsökväg för hello blob utvärderas dynamiskt baserat på hello starttiden för hello-segment som bearbetas.</span><span class="sxs-lookup"><span data-stu-id="088fb-447">hello folder path for hello blob is dynamically evaluated based on hello start time of hello slice that is being processed.</span></span> <span data-ttu-id="088fb-448">hello mappsökväg använder hello år, månad, dag och timmar delen av hello starttid.</span><span class="sxs-lookup"><span data-stu-id="088fb-448">hello folder path uses hello year, month, day, and hours portion of hello start time.</span></span>

```JSON
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

<span data-ttu-id="088fb-449">**En kopia aktivitet i en pipeline med ett Azure Data Lake Store-källa och en blob-mottagare**</span><span class="sxs-lookup"><span data-stu-id="088fb-449">**A copy activity in a pipeline with an Azure Data Lake Store source and a blob sink**</span></span>

<span data-ttu-id="088fb-450">I följande exempel hello, hello pipelinen innehåller en kopia-aktivitet som är konfigurerad toouse hello inkommande och utgående datauppsättningar.</span><span class="sxs-lookup"><span data-stu-id="088fb-450">In hello following example, hello pipeline contains a copy activity that is configured toouse hello input and output datasets.</span></span> <span data-ttu-id="088fb-451">Hej kopieringsaktiviteten är schemalagda toorun varje timme.</span><span class="sxs-lookup"><span data-stu-id="088fb-451">hello copy activity is scheduled toorun every hour.</span></span> <span data-ttu-id="088fb-452">I hello pipeline JSON-definitionen hello `source` typ har angetts för`AzureDataLakeStoreSource`, och hello `sink` typ har angetts för`BlobSink`.</span><span class="sxs-lookup"><span data-stu-id="088fb-452">In hello pipeline JSON definition, hello `source` type is set too`AzureDataLakeStoreSource`, and hello `sink` type is set too`BlobSink`.</span></span>

```json
{  
    "name":"SamplePipeline",
    "properties":{  
        "start":"2014-06-01T18:00:00",
        "end":"2014-06-01T19:00:00",
        "description":"pipeline for copy activity",
        "activities":[  
              {
                "name": "AzureDakeLaketoBlob",
                "description": "copy activity",
                "type": "Copy",
                "inputs": [
                  {
                    "name": "AzureDataLakeStoreInput"
                  }
                ],
                "outputs": [
                  {
                    "name": "AzureBlobOutput"
                  }
                ],
                "typeProperties": {
                    "source": {
                        "type": "AzureDataLakeStoreSource",
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

<span data-ttu-id="088fb-453">Du kan mappa kolumner från hello källa dataset toocolumns i hello sink dataset i aktivitetsdefinitionen för hello kopia.</span><span class="sxs-lookup"><span data-stu-id="088fb-453">In hello copy activity definition, you can also map columns from hello source dataset toocolumns in hello sink dataset.</span></span> <span data-ttu-id="088fb-454">Mer information finns i [mappa dataset kolumner i Azure Data Factory](data-factory-map-columns.md).</span><span class="sxs-lookup"><span data-stu-id="088fb-454">For details, see [Mapping dataset columns in Azure Data Factory](data-factory-map-columns.md).</span></span>

## <a name="performance-and-tuning"></a><span data-ttu-id="088fb-455">Prestanda- och justering</span><span class="sxs-lookup"><span data-stu-id="088fb-455">Performance and tuning</span></span>
<span data-ttu-id="088fb-456">toolearn om hello faktorer som påverkar Kopieringsaktiviteten prestanda och hur toooptimize, se hello [prestandajustering guide och Kopieringsaktivitet prestanda](data-factory-copy-activity-performance.md) artikel.</span><span class="sxs-lookup"><span data-stu-id="088fb-456">toolearn about hello factors that affect Copy Activity performance and how toooptimize it, see hello [Copy Activity performance and tuning guide](data-factory-copy-activity-performance.md) article.</span></span>
