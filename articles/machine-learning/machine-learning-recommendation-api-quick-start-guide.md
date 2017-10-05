---
title: 'Snabbstartsguide: Azure Machine Learning rekommendationer API (version 1) | Microsoft Docs'
description: Azure Machine Learning rekommendationer - Snabbstartsguide
services: machine-learning
documentationcenter: 
author: LuisCabrer
manager: jhubbard
editor: cgronlun
ms.assetid: 5bce1a4a-1ad6-473f-812b-84f800fdc09a
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/31/2017
ms.author: luisca
ROBOTS: NOINDEX
redirect_url: machine-learning-datamarket-deprecation
redirect_document_id: TRUE
ms.openlocfilehash: 0a9d0b6aa1ef734a857ecc16777ba6250909b38d
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="quick-start-guide-for-the-machine-learning-recommendations-api-version-1"></a><span data-ttu-id="2f9f2-104">Snabbstartsguide för Machine Learning rekommendationer API (version 1)</span><span class="sxs-lookup"><span data-stu-id="2f9f2-104">Quick start guide for the Machine Learning Recommendations API (version 1)</span></span>

> [!NOTE]
> <span data-ttu-id="2f9f2-105">Du bör börja använda den [kognitiva rekommendationer API-tjänsten](https://www.microsoft.com/cognitive-services/recommendations-api) i stället för den här versionen.</span><span class="sxs-lookup"><span data-stu-id="2f9f2-105">You should start using the [Recommendations API Cognitive Service](https://www.microsoft.com/cognitive-services/recommendations-api) instead of this version.</span></span> <span data-ttu-id="2f9f2-106">Kognitiva Rekommendationstjänsten ersätter den här tjänsten och de nya funktionerna det kommer fram.</span><span class="sxs-lookup"><span data-stu-id="2f9f2-106">The Recommendations Cognitive Service will be replacing this service, and all the new features will be developed there.</span></span> <span data-ttu-id="2f9f2-107">Den har nya funktioner som batchbearbetning support, bättre API Explorer, en tydligare API underlag, mer konsekvent signup/fakturerings experience och så vidare.</span><span class="sxs-lookup"><span data-stu-id="2f9f2-107">It has new capabilities like batching support, a better API Explorer, a cleaner API surface, more consistent signup/billing experience, etc.</span></span>
>
> <span data-ttu-id="2f9f2-108">Lär dig mer om [migrera till den nya kognitiva tjänsten](http://aka.ms/recomigrate).</span><span class="sxs-lookup"><span data-stu-id="2f9f2-108">Learn more about [Migrating to the new Cognitive Service](http://aka.ms/recomigrate).</span></span>
> 
> 

<span data-ttu-id="2f9f2-109">Det här dokumentet beskriver hur du integrerar din tjänst eller ditt program att använda Microsoft Azure Machine Learning rekommendationer.</span><span class="sxs-lookup"><span data-stu-id="2f9f2-109">This document describes how to onboard your service or application to use Microsoft Azure Machine Learning Recommendations.</span></span> <span data-ttu-id="2f9f2-110">Du hittar mer information på rekommendationer API i den [Cortana Intelligence Gallery](https://gallery.cortanaintelligence.com/MachineLearningAPIs/Recommendations-2).</span><span class="sxs-lookup"><span data-stu-id="2f9f2-110">You can find more details on the Recommendations API in the [Cortana Intelligence Gallery](https://gallery.cortanaintelligence.com/MachineLearningAPIs/Recommendations-2).</span></span>

[!INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

## <a name="general-overview"></a><span data-ttu-id="2f9f2-111">Allmän översikt</span><span class="sxs-lookup"><span data-stu-id="2f9f2-111">General overview</span></span>
<span data-ttu-id="2f9f2-112">Om du vill använda Azure Machine Learning rekommendationer, måste du vidta följande åtgärder:</span><span class="sxs-lookup"><span data-stu-id="2f9f2-112">To use Azure Machine Learning Recommendations, you need to take the following steps:</span></span>

* <span data-ttu-id="2f9f2-113">Skapa en modell - en modell är en behållare för användningsdata och katalogdata rekommendation modellen.</span><span class="sxs-lookup"><span data-stu-id="2f9f2-113">Create a model - A model is a container of your usage data, catalog data, and the recommendation model.</span></span>
* <span data-ttu-id="2f9f2-114">Importera data catalog - en katalog innehåller information om metadata på objekt.</span><span class="sxs-lookup"><span data-stu-id="2f9f2-114">Import catalog data - A catalog contains metadata information on the items.</span></span> 
* <span data-ttu-id="2f9f2-115">Importera data om programvaruanvändning - användningsdata kan överföras i ett av två sätt (eller båda):</span><span class="sxs-lookup"><span data-stu-id="2f9f2-115">Import usage data - Usage data can be uploaded in one of two ways (or both):</span></span>
  * <span data-ttu-id="2f9f2-116">Genom att överföra en fil som innehåller användningsdata.</span><span class="sxs-lookup"><span data-stu-id="2f9f2-116">By uploading a file that contains the usage data.</span></span>
  * <span data-ttu-id="2f9f2-117">Genom att skicka data förvärv händelser.</span><span class="sxs-lookup"><span data-stu-id="2f9f2-117">By sending data acquisition events.</span></span>
    <span data-ttu-id="2f9f2-118">Vanligtvis kan du överföra en fil för användning för att kunna skapa en första rekommendationen model (bootstrap) och använda den tills systemet samlar in data med hjälp av förvärv dataformatet.</span><span class="sxs-lookup"><span data-stu-id="2f9f2-118">Usually you upload a usage file in order to be able to create an initial recommendation model (bootstrap) and use it until the system gathers enough data by using the data acquisition format.</span></span>
* <span data-ttu-id="2f9f2-119">Skapa en rekommendation modell – det här är en asynkron åtgärd där systemet rekommendation tar alla användningsdata och skapar en rekommendation modell.</span><span class="sxs-lookup"><span data-stu-id="2f9f2-119">Build a recommendation model - This is an asynchronous operation in which the recommendation system takes all the usage data and creates a recommendation model.</span></span> <span data-ttu-id="2f9f2-120">Den här åtgärden kan ta några minuter eller flera timmar beroende på storleken på data och skapa konfigurationsparametrar.</span><span class="sxs-lookup"><span data-stu-id="2f9f2-120">This operation can take several minutes or several hours depending on the size of the data and the build configuration parameters.</span></span> <span data-ttu-id="2f9f2-121">När utlöser genereringen kan får du ett build-ID.</span><span class="sxs-lookup"><span data-stu-id="2f9f2-121">When triggering the build, you will get a build ID.</span></span> <span data-ttu-id="2f9f2-122">Du kan använda den för att kontrollera när build-processen har avslutats innan du börjar använda rekommendationer.</span><span class="sxs-lookup"><span data-stu-id="2f9f2-122">Use it to check when the build process has ended before starting to consume recommendations.</span></span>
* <span data-ttu-id="2f9f2-123">Använda rekommendationer - Get rekommendationer för ett specifikt objekt eller en lista med objekt.</span><span class="sxs-lookup"><span data-stu-id="2f9f2-123">Consume recommendations - Get recommendations for a specific item or list of items.</span></span>

<span data-ttu-id="2f9f2-124">Alla ovanstående görs via Azure Machine Learning rekommendationer API.</span><span class="sxs-lookup"><span data-stu-id="2f9f2-124">All the steps above are done through the Azure Machine Learning Recommendations API.</span></span>  <span data-ttu-id="2f9f2-125">Du kan hämta ett exempelprogram som implementerar var och en av de här stegen från den [samt gallery.](http://1drv.ms/1xeO2F3)</span><span class="sxs-lookup"><span data-stu-id="2f9f2-125">You can download a sample application that implements each of these steps from the [gallery as well.](http://1drv.ms/1xeO2F3)</span></span>

## <a name="limitations"></a><span data-ttu-id="2f9f2-126">Begränsningar</span><span class="sxs-lookup"><span data-stu-id="2f9f2-126">Limitations</span></span>
* <span data-ttu-id="2f9f2-127">Det maximala antalet modeller per prenumeration är 10.</span><span class="sxs-lookup"><span data-stu-id="2f9f2-127">The maximum number of models per subscription is 10.</span></span>
* <span data-ttu-id="2f9f2-128">Det maximala antalet objekt som kan innehålla en katalog är 100 000.</span><span class="sxs-lookup"><span data-stu-id="2f9f2-128">The maximum number of items that a catalog can hold is 100,000.</span></span>
* <span data-ttu-id="2f9f2-129">Det maximala antalet punkter för användning som hålls är ~ 5,000,000.</span><span class="sxs-lookup"><span data-stu-id="2f9f2-129">The maximum number of usage points that are kept is ~5,000,000.</span></span> <span data-ttu-id="2f9f2-130">Den äldsta tas bort om nya överföras eller rapporteras.</span><span class="sxs-lookup"><span data-stu-id="2f9f2-130">The oldest will be deleted if new ones will be uploaded or reported.</span></span>
* <span data-ttu-id="2f9f2-131">Den maximala storleken för data som kan skickas i POST (till exempel importera katalogdata import användningsdata) är 200MB.</span><span class="sxs-lookup"><span data-stu-id="2f9f2-131">The maximum size of data that can be sent in POST (for example, import catalog data, import usage data) is 200MB.</span></span>
* <span data-ttu-id="2f9f2-132">Antal transaktioner per sekund för en rekommendation modell-version som inte är aktiv är ~ 2TPS.</span><span class="sxs-lookup"><span data-stu-id="2f9f2-132">The number of transactions per second for a recommendation model build that is not active is ~2TPS.</span></span> <span data-ttu-id="2f9f2-133">En rekommendation modell-version som är aktiv kan innehålla till 20TPS.</span><span class="sxs-lookup"><span data-stu-id="2f9f2-133">A recommendation model build that is active can hold up to 20TPS.</span></span>

## <a name="integration"></a><span data-ttu-id="2f9f2-134">Integrering</span><span class="sxs-lookup"><span data-stu-id="2f9f2-134">Integration</span></span>
### <a name="authentication"></a><span data-ttu-id="2f9f2-135">Autentisering</span><span class="sxs-lookup"><span data-stu-id="2f9f2-135">Authentication</span></span>
<span data-ttu-id="2f9f2-136">Microsoft Azure Marketplace stöder antingen grundläggande eller OAuth-autentiseringsmetoden.</span><span class="sxs-lookup"><span data-stu-id="2f9f2-136">Microsoft Azure Marketplace supports either the Basic or OAuth authentication method.</span></span> <span data-ttu-id="2f9f2-137">Du lätt kan hitta kontot nycklar genom att gå till nycklarna i marketplace under [dina kontoinställningar](https://datamarket.azure.com/account/keys).</span><span class="sxs-lookup"><span data-stu-id="2f9f2-137">You can easily find the account keys by navigating to the keys in the marketplace under [your account settings](https://datamarket.azure.com/account/keys).</span></span> 

#### <a name="basic-authentication"></a><span data-ttu-id="2f9f2-138">Grundläggande autentisering</span><span class="sxs-lookup"><span data-stu-id="2f9f2-138">Basic Authentication</span></span>
<span data-ttu-id="2f9f2-139">Lägg till Authorization-huvud:</span><span class="sxs-lookup"><span data-stu-id="2f9f2-139">Add the Authorization header:</span></span>

    Authorization: Basic <creds>

    Where <creds> = ConvertToBase64("AccountKey:" + yourAccountKey);  

<span data-ttu-id="2f9f2-140">Omvandla till Base64 (C#)</span><span class="sxs-lookup"><span data-stu-id="2f9f2-140">Convert to Base64 (C#)</span></span>

    var bytes = Encoding.UTF8.GetBytes("AccountKey:" + yourAccountKey);
    var creds = Convert.ToBase64String(bytes);

<span data-ttu-id="2f9f2-141">Omvandla till Base64 (JavaScript)</span><span class="sxs-lookup"><span data-stu-id="2f9f2-141">Convert to Base64 (JavaScript)</span></span>

    var creds = window.btoa("AccountKey" + ":" + yourAccountKey);




### <a name="service-uri"></a><span data-ttu-id="2f9f2-142">URI för tjänsten</span><span class="sxs-lookup"><span data-stu-id="2f9f2-142">Service URI</span></span>
<span data-ttu-id="2f9f2-143">Tjänstroten URI för API: er för Azure Machine Learning rekommendationer är [här.](https://api.datamarket.azure.com/amla/recommendations/v2/)</span><span class="sxs-lookup"><span data-stu-id="2f9f2-143">The service root URI for the Azure Machine Learning Recommendations APIs is [here.](https://api.datamarket.azure.com/amla/recommendations/v2/)</span></span>

<span data-ttu-id="2f9f2-144">Fullständig URI-tjänsten uttrycks med hjälp av element i OData-specifikationen.</span><span class="sxs-lookup"><span data-stu-id="2f9f2-144">The full service URI is expressed using elements of the OData specification.</span></span>

### <a name="api-version"></a><span data-ttu-id="2f9f2-145">API-version</span><span class="sxs-lookup"><span data-stu-id="2f9f2-145">API version</span></span>
<span data-ttu-id="2f9f2-146">Varje API-anrop ska ha frågeparameter kallas apiVersion som ska vara inställd på ”1.0” i slutet.</span><span class="sxs-lookup"><span data-stu-id="2f9f2-146">Each API call will have, at the end, a query parameter called apiVersion that should be set to "1.0".</span></span>

### <a name="ids-are-case-sensitive"></a><span data-ttu-id="2f9f2-147">ID: N är skiftlägeskänsliga</span><span class="sxs-lookup"><span data-stu-id="2f9f2-147">IDs are case-sensitive</span></span>
<span data-ttu-id="2f9f2-148">ID: N som returneras av API: er, är skiftlägeskänsliga och kan användas som sådana när de skickas som parametrar i efterföljande API-anrop.</span><span class="sxs-lookup"><span data-stu-id="2f9f2-148">IDs, returned by any of the APIs, are case-sensitive and should be used as such when passed as parameters in subsequent API calls.</span></span> <span data-ttu-id="2f9f2-149">Till exempel är modell-ID: N och katalog-ID: N skiftlägeskänsliga.</span><span class="sxs-lookup"><span data-stu-id="2f9f2-149">For instance, model IDs and catalog IDs are case-sensitive.</span></span>

### <a name="create-a-model"></a><span data-ttu-id="2f9f2-150">Skapa en modell</span><span class="sxs-lookup"><span data-stu-id="2f9f2-150">Create a model</span></span>
<span data-ttu-id="2f9f2-151">Skapa en begäran om ”skapa modellen”:</span><span class="sxs-lookup"><span data-stu-id="2f9f2-151">Creating a "create model" request:</span></span>

| <span data-ttu-id="2f9f2-152">HTTP-metoden</span><span class="sxs-lookup"><span data-stu-id="2f9f2-152">HTTP Method</span></span> | <span data-ttu-id="2f9f2-153">URI: N</span><span class="sxs-lookup"><span data-stu-id="2f9f2-153">URI</span></span> |
|:--- |:--- |
| <span data-ttu-id="2f9f2-154">POST</span><span class="sxs-lookup"><span data-stu-id="2f9f2-154">POST</span></span> |`<rootURI>/CreateModel?modelName=%27<model_name>%27&apiVersion=%271.0%27`<br><br><span data-ttu-id="2f9f2-155">Exempel:</span><span class="sxs-lookup"><span data-stu-id="2f9f2-155">Example:</span></span><br>`<rootURI>/CreateModel?modelName=%27MyFirstModel%27&apiVersion=%271.0%27` |

| <span data-ttu-id="2f9f2-156">Parameternamn</span><span class="sxs-lookup"><span data-stu-id="2f9f2-156">Parameter Name</span></span> | <span data-ttu-id="2f9f2-157">Giltiga värden</span><span class="sxs-lookup"><span data-stu-id="2f9f2-157">Valid Values</span></span> |
|:--- |:--- |
| <span data-ttu-id="2f9f2-158">%{ModelName/</span><span class="sxs-lookup"><span data-stu-id="2f9f2-158">modelName</span></span> |<span data-ttu-id="2f9f2-159">Endast bokstäver (A-Z, a-z), siffror (0-9), bindestreck (-) och understreck (_) är tillåtna.</span><span class="sxs-lookup"><span data-stu-id="2f9f2-159">Only letters (A-Z, a-z), numbers (0-9), hyphens (-) and underscore (_) are allowed.</span></span><br><span data-ttu-id="2f9f2-160">Maxlängd: 20</span><span class="sxs-lookup"><span data-stu-id="2f9f2-160">Max length: 20</span></span> |
| <span data-ttu-id="2f9f2-161">apiVersion</span><span class="sxs-lookup"><span data-stu-id="2f9f2-161">apiVersion</span></span> |<span data-ttu-id="2f9f2-162">1.0</span><span class="sxs-lookup"><span data-stu-id="2f9f2-162">1.0</span></span> |
|  | |
| <span data-ttu-id="2f9f2-163">Begärandetext</span><span class="sxs-lookup"><span data-stu-id="2f9f2-163">Request Body</span></span> |<span data-ttu-id="2f9f2-164">INGEN</span><span class="sxs-lookup"><span data-stu-id="2f9f2-164">NONE</span></span> |

<span data-ttu-id="2f9f2-165">**Svaret**:</span><span class="sxs-lookup"><span data-stu-id="2f9f2-165">**Response**:</span></span>

<span data-ttu-id="2f9f2-166">HTTP-statuskod: 200</span><span class="sxs-lookup"><span data-stu-id="2f9f2-166">HTTP Status code: 200</span></span>

* <span data-ttu-id="2f9f2-167">`feed/entry/content/properties/id`-Innehåller modell-ID.</span><span class="sxs-lookup"><span data-stu-id="2f9f2-167">`feed/entry/content/properties/id` - Contains the model ID.</span></span>
  <span data-ttu-id="2f9f2-168">Observera att det modell-ID är skiftlägeskänsliga.</span><span class="sxs-lookup"><span data-stu-id="2f9f2-168">Note that the model ID is case-sensitive.</span></span>

<span data-ttu-id="2f9f2-169">OData-XML</span><span class="sxs-lookup"><span data-stu-id="2f9f2-169">OData XML</span></span>

    <feed xmlns:base="https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/CreateModel" xmlns:d="http://schemas.microsoft.com/ado/2007/08/dataservices" xmlns:m="http://schemas.microsoft.com/ado/2007/08/dataservices/metadata" xmlns="http://www.w3.org/2005/Atom">
      <title type="text" />
      <subtitle type="text">Create A New Model</subtitle>
      <id>https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/CreateModel?modelName='MyFirstModel'&amp;apiVersion='1.0'</id>
      <rights type="text" />
      <updated>2014-10-05T06:35:21Z</updated>
      <link rel="self" href="https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/CreateModel?modelName='MyFirstModel'&amp;apiVersion='1.0'" />
      <entry>
    <id>https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/CreateModel?modelName='MyFirstModel'&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1</id>
    <title type="text">CreateANewModelEntity2</title>
    <updated>2014-10-05T06:35:21Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/CreateModel?modelName='MyFirstModel'&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1" />
    <content type="application/xml">
      <m:properties>
        <d:Id m:type="Edm.String">a658c626-2baa-43a7-ac98-f6ee26120a12</d:Id>
        <d:Name m:type="Edm.String">MyFirstModel</d:Name>
        <d:Date m:type="Edm.String">10/5/2014 6:35:19 AM</d:Date>
        <d:Status m:type="Edm.String">Created</d:Status>
        <d:HasActiveBuild m:type="Edm.String">false</d:HasActiveBuild>
        <d:BuildId m:type="Edm.String">-1</d:BuildId>
        <d:Mpr m:type="Edm.String">0</d:Mpr>
        <d:UserName m:type="Edm.String">5-4058-ab36-1fe254f05102@dm.com</d:UserName>
        <d:Description m:type="Edm.String"></d:Description>
      </m:properties>
    </content>
      </entry>
    </feed>


### <a name="import-catalog-data"></a><span data-ttu-id="2f9f2-170">Importera data catalog</span><span class="sxs-lookup"><span data-stu-id="2f9f2-170">Import catalog data</span></span>
<span data-ttu-id="2f9f2-171">Om du överför flera katalogfiler till samma modell med flera anrop infogas vi bara nya katalogobjekt.</span><span class="sxs-lookup"><span data-stu-id="2f9f2-171">If you upload several catalog files to the same model with several calls, we will insert only the new catalog items.</span></span> <span data-ttu-id="2f9f2-172">Befintliga objekt förblir med de ursprungliga värdena.</span><span class="sxs-lookup"><span data-stu-id="2f9f2-172">Existing items will remain with the original values.</span></span>

| <span data-ttu-id="2f9f2-173">HTTP-metoden</span><span class="sxs-lookup"><span data-stu-id="2f9f2-173">HTTP Method</span></span> | <span data-ttu-id="2f9f2-174">URI: N</span><span class="sxs-lookup"><span data-stu-id="2f9f2-174">URI</span></span> |
|:--- |:--- |
| <span data-ttu-id="2f9f2-175">POST</span><span class="sxs-lookup"><span data-stu-id="2f9f2-175">POST</span></span> |`<rootURI>/ImportCatalogFile?modelId=%27<modelId>%27&filename=%27<fileName>%27&apiVersion=%271.0%27`<br><br><span data-ttu-id="2f9f2-176">Exempel:</span><span class="sxs-lookup"><span data-stu-id="2f9f2-176">Example:</span></span><br>`<rootURI>/ImportCatalogFile?modelId=%27a658c626-2baa-43a7-ac98-f6ee26120a12%27&filename=%27catalog10_small.txt%27&apiVersion=%271.0%27` |

| <span data-ttu-id="2f9f2-177">Parameternamn</span><span class="sxs-lookup"><span data-stu-id="2f9f2-177">Parameter Name</span></span> | <span data-ttu-id="2f9f2-178">Giltiga värden</span><span class="sxs-lookup"><span data-stu-id="2f9f2-178">Valid Values</span></span> |
|:--- |:--- |
| <span data-ttu-id="2f9f2-179">%{ModelID/</span><span class="sxs-lookup"><span data-stu-id="2f9f2-179">modelId</span></span> |<span data-ttu-id="2f9f2-180">Unik identifierare för modellen (skiftlägeskänsligt)</span><span class="sxs-lookup"><span data-stu-id="2f9f2-180">Unique identifier of the model (case-sensitive)</span></span> |
| <span data-ttu-id="2f9f2-181">filnamn</span><span class="sxs-lookup"><span data-stu-id="2f9f2-181">filename</span></span> |<span data-ttu-id="2f9f2-182">Textrepresentation identifierare för katalogen.</span><span class="sxs-lookup"><span data-stu-id="2f9f2-182">Textual identifier of the catalog.</span></span><br><span data-ttu-id="2f9f2-183">Endast bokstäver (A-Z, a-z), siffror (0-9), bindestreck (-) och understreck (_) är tillåtna.</span><span class="sxs-lookup"><span data-stu-id="2f9f2-183">Only letters (A-Z, a-z), numbers (0-9), hyphens (-) and underscore (_) are allowed.</span></span><br><span data-ttu-id="2f9f2-184">Maxlängd: 50</span><span class="sxs-lookup"><span data-stu-id="2f9f2-184">Max length: 50</span></span> |
| <span data-ttu-id="2f9f2-185">apiVersion</span><span class="sxs-lookup"><span data-stu-id="2f9f2-185">apiVersion</span></span> |<span data-ttu-id="2f9f2-186">1.0</span><span class="sxs-lookup"><span data-stu-id="2f9f2-186">1.0</span></span> |
|  | |
| <span data-ttu-id="2f9f2-187">Begärandetext</span><span class="sxs-lookup"><span data-stu-id="2f9f2-187">Request Body</span></span> |<span data-ttu-id="2f9f2-188">Katalogen data.</span><span class="sxs-lookup"><span data-stu-id="2f9f2-188">Catalog data.</span></span> <span data-ttu-id="2f9f2-189">Format:</span><span class="sxs-lookup"><span data-stu-id="2f9f2-189">Format:</span></span><br>`<Item Id>,<Item Name>,<Item Category>[,<description>]`<br><br><table><tr><th><span data-ttu-id="2f9f2-190">Namn</span><span class="sxs-lookup"><span data-stu-id="2f9f2-190">Name</span></span></th><th><span data-ttu-id="2f9f2-191">Obligatorisk</span><span class="sxs-lookup"><span data-stu-id="2f9f2-191">Mandatory</span></span></th><th><span data-ttu-id="2f9f2-192">Typ</span><span class="sxs-lookup"><span data-stu-id="2f9f2-192">Type</span></span></th><th><span data-ttu-id="2f9f2-193">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="2f9f2-193">Description</span></span></th></tr><tr><td><span data-ttu-id="2f9f2-194">Objekt-Id</span><span class="sxs-lookup"><span data-stu-id="2f9f2-194">Item Id</span></span></td><td><span data-ttu-id="2f9f2-195">Ja</span><span class="sxs-lookup"><span data-stu-id="2f9f2-195">Yes</span></span></td><td><span data-ttu-id="2f9f2-196">Längden på alfanumeriska, max 50</span><span class="sxs-lookup"><span data-stu-id="2f9f2-196">Alphanumeric, max length 50</span></span></td><td><span data-ttu-id="2f9f2-197">Unik identifierare för ett objekt</span><span class="sxs-lookup"><span data-stu-id="2f9f2-197">Unique identifier of an item</span></span></td></tr><tr><td><span data-ttu-id="2f9f2-198">Objektnamnet</span><span class="sxs-lookup"><span data-stu-id="2f9f2-198">Item Name</span></span></td><td><span data-ttu-id="2f9f2-199">Ja</span><span class="sxs-lookup"><span data-stu-id="2f9f2-199">Yes</span></span></td><td><span data-ttu-id="2f9f2-200">Längden på alfanumeriska, max 255</span><span class="sxs-lookup"><span data-stu-id="2f9f2-200">Alphanumeric, max length 255</span></span></td><td><span data-ttu-id="2f9f2-201">Objektnamnet</span><span class="sxs-lookup"><span data-stu-id="2f9f2-201">Item name</span></span></td></tr><tr><td><span data-ttu-id="2f9f2-202">Objektet kategori</span><span class="sxs-lookup"><span data-stu-id="2f9f2-202">Item Category</span></span></td><td><span data-ttu-id="2f9f2-203">Ja</span><span class="sxs-lookup"><span data-stu-id="2f9f2-203">Yes</span></span></td><td><span data-ttu-id="2f9f2-204">Längden på alfanumeriska, max 255</span><span class="sxs-lookup"><span data-stu-id="2f9f2-204">Alphanumeric, max length 255</span></span></td><td><span data-ttu-id="2f9f2-205">Kategorin som det här objektet tillhör (till exempel matlagning Books Drama...)</span><span class="sxs-lookup"><span data-stu-id="2f9f2-205">Category to which this item belongs (for example, Cooking Books, Drama…)</span></span></td></tr><tr><td><span data-ttu-id="2f9f2-206">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="2f9f2-206">Description</span></span></td><td><span data-ttu-id="2f9f2-207">Nej</span><span class="sxs-lookup"><span data-stu-id="2f9f2-207">No</span></span></td><td><span data-ttu-id="2f9f2-208">Alfanumeriska, max längd 4000</span><span class="sxs-lookup"><span data-stu-id="2f9f2-208">Alphanumeric, max length 4000</span></span></td><td><span data-ttu-id="2f9f2-209">Beskrivning av det här objektet</span><span class="sxs-lookup"><span data-stu-id="2f9f2-209">Description of this item</span></span></td></tr></table><br><span data-ttu-id="2f9f2-210">Maximal filstorlek är 200MB.</span><span class="sxs-lookup"><span data-stu-id="2f9f2-210">Maximum file size is 200MB.</span></span><br><br><span data-ttu-id="2f9f2-211">Exempel:</span><span class="sxs-lookup"><span data-stu-id="2f9f2-211">Example:</span></span><br><code>2406e770-769c-4189-89de-1c9283f93a96,Clara Callan,Book<br>21bf8088-b6c0-4509-870c-e1c7ac78304a,The Forgetting Room: A Fiction (Byzantium Book),Book<br>3bb5cb44-d143-4bdd-a55c-443964bf4b23,Spadework,Book<br>552a1940-21e4-4399-82bb-594b46d7ed54,Restraint of Beasts,Book</code> |

<span data-ttu-id="2f9f2-212">**Svaret**:</span><span class="sxs-lookup"><span data-stu-id="2f9f2-212">**Response**:</span></span>

<span data-ttu-id="2f9f2-213">HTTP-statuskod: 200</span><span class="sxs-lookup"><span data-stu-id="2f9f2-213">HTTP Status code: 200</span></span>

* <span data-ttu-id="2f9f2-214">`Feed\entry\content\properties\LineCount`-Antal rader som accepteras.</span><span class="sxs-lookup"><span data-stu-id="2f9f2-214">`Feed\entry\content\properties\LineCount` - Number of lines accepted.</span></span>
* <span data-ttu-id="2f9f2-215">`Feed\entry\content\properties\ErrorCount`-Antal rader som inte infogades på grund av ett fel.</span><span class="sxs-lookup"><span data-stu-id="2f9f2-215">`Feed\entry\content\properties\ErrorCount` - Number of lines that were not inserted due to an error.</span></span>

<span data-ttu-id="2f9f2-216">OData-XML</span><span class="sxs-lookup"><span data-stu-id="2f9f2-216">OData XML</span></span>

    <feed xmlns:base="https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/ImportCatalogFile" xmlns:d="http://schemas.microsoft.com/ado/2007/08/dataservices" xmlns:m="http://schemas.microsoft.com/ado/2007/08/dataservices/metadata" xmlns="http://www.w3.org/2005/Atom">
      <title type="text" />
          <subtitle type="text">Import catalog file</subtitle>
          <id>https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/ImportCatalogFile?modelId='a658c626-2baa-43a7-ac98-f6ee26120a12'&amp;filename='catalog10_small.txt'&amp;apiVersion='1.0'</id>
      <rights type="text" />
      <updated>2014-10-05T06:58:04Z</updated>
      <link rel="self" href="https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/ImportCatalogFile?modelId='a658c626-2baa-43a7-ac98-f6ee26120a12'&amp;filename='catalog10_small.txt'&amp;apiVersion='1.0'" />
      <entry>
    <id>https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/ImportCatalogFile?modelId='a658c626-2baa-43a7-ac98-f6ee26120a12'&amp;filename='catalog10_small.txt'&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1</id>
    <title type="text">ImportCatalogFileEntity2</title>
    <updated>2014-10-05T06:58:04Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/ImportCatalogFile?modelId='a658c626-2baa-43a7-ac98-f6ee26120a12'&amp;filename='catalog10_small.txt'&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1" />
    <content type="application/xml">
      <m:properties>
        <d:LineCount m:type="Edm.String">4</d:LineCount>
        <d:ErrorCount m:type="Edm.String">0</d:ErrorCount>
      </m:properties>
    </content>
      </entry>
    </feed>


### <a name="import-usage-data"></a><span data-ttu-id="2f9f2-217">Importera data om programvaruanvändning</span><span class="sxs-lookup"><span data-stu-id="2f9f2-217">Import usage data</span></span>
#### <a name="uploading-a-file"></a><span data-ttu-id="2f9f2-218">Överför en fil</span><span class="sxs-lookup"><span data-stu-id="2f9f2-218">Uploading a file</span></span>
<span data-ttu-id="2f9f2-219">Det här avsnittet visar hur du överför användningsdata med hjälp av en fil.</span><span class="sxs-lookup"><span data-stu-id="2f9f2-219">This section shows how to upload usage data by using a file.</span></span> <span data-ttu-id="2f9f2-220">Du kan anropa denna API flera gånger med användningsdata.</span><span class="sxs-lookup"><span data-stu-id="2f9f2-220">You can call this API several times with usage data.</span></span> <span data-ttu-id="2f9f2-221">Alla användningsdata sparas för alla samtal.</span><span class="sxs-lookup"><span data-stu-id="2f9f2-221">All usage data will be saved for all calls.</span></span>

| <span data-ttu-id="2f9f2-222">HTTP-metoden</span><span class="sxs-lookup"><span data-stu-id="2f9f2-222">HTTP Method</span></span> | <span data-ttu-id="2f9f2-223">URI: N</span><span class="sxs-lookup"><span data-stu-id="2f9f2-223">URI</span></span> |
|:--- |:--- |
| <span data-ttu-id="2f9f2-224">POST</span><span class="sxs-lookup"><span data-stu-id="2f9f2-224">POST</span></span> |`<rootURI>/ImportUsageFile?modelId=%27<modelId>%27&filename=%27<fileName>%27&apiVersion=%271.0%27`<br><br><span data-ttu-id="2f9f2-225">Exempel:</span><span class="sxs-lookup"><span data-stu-id="2f9f2-225">Example:</span></span><br>`<rootURI>/ImportUsageFile?modelId=%27a658c626-2baa-43a7-ac98-f6ee26120a12%27&filename=%27ImplicitMatrix10_Guid_small.txt%27&apiVersion=%271.0%27` |

| <span data-ttu-id="2f9f2-226">Parameternamn</span><span class="sxs-lookup"><span data-stu-id="2f9f2-226">Parameter Name</span></span> | <span data-ttu-id="2f9f2-227">Giltiga värden</span><span class="sxs-lookup"><span data-stu-id="2f9f2-227">Valid Values</span></span> |
|:--- |:--- |
| <span data-ttu-id="2f9f2-228">%{ModelID/</span><span class="sxs-lookup"><span data-stu-id="2f9f2-228">modelId</span></span> |<span data-ttu-id="2f9f2-229">Unik identifierare för modellen (skiftlägeskänsligt)</span><span class="sxs-lookup"><span data-stu-id="2f9f2-229">Unique identifier of the model (case-sensitive)</span></span> |
| <span data-ttu-id="2f9f2-230">filnamn</span><span class="sxs-lookup"><span data-stu-id="2f9f2-230">filename</span></span> |<span data-ttu-id="2f9f2-231">Textrepresentation identifierare för katalogen.</span><span class="sxs-lookup"><span data-stu-id="2f9f2-231">Textual identifier of the catalog.</span></span><br><span data-ttu-id="2f9f2-232">Endast bokstäver (A-Z, a-z), siffror (0-9), bindestreck (-) och understreck (_) är tillåtna.</span><span class="sxs-lookup"><span data-stu-id="2f9f2-232">Only letters (A-Z, a-z), numbers (0-9), hyphens (-) and underscore (_) are allowed.</span></span><br><span data-ttu-id="2f9f2-233">Maxlängd: 50</span><span class="sxs-lookup"><span data-stu-id="2f9f2-233">Max length: 50</span></span> |
| <span data-ttu-id="2f9f2-234">apiVersion</span><span class="sxs-lookup"><span data-stu-id="2f9f2-234">apiVersion</span></span> |<span data-ttu-id="2f9f2-235">1.0</span><span class="sxs-lookup"><span data-stu-id="2f9f2-235">1.0</span></span> |
|  | |
| <span data-ttu-id="2f9f2-236">Begärandetext</span><span class="sxs-lookup"><span data-stu-id="2f9f2-236">Request Body</span></span> |<span data-ttu-id="2f9f2-237">Användningsdata.</span><span class="sxs-lookup"><span data-stu-id="2f9f2-237">Usage data.</span></span> <span data-ttu-id="2f9f2-238">Format:</span><span class="sxs-lookup"><span data-stu-id="2f9f2-238">Format:</span></span><br>`<User Id>,<Item Id>[,<Time>,<Event>]`<br><br><table><tr><th><span data-ttu-id="2f9f2-239">Namn</span><span class="sxs-lookup"><span data-stu-id="2f9f2-239">Name</span></span></th><th><span data-ttu-id="2f9f2-240">Obligatorisk</span><span class="sxs-lookup"><span data-stu-id="2f9f2-240">Mandatory</span></span></th><th><span data-ttu-id="2f9f2-241">Typ</span><span class="sxs-lookup"><span data-stu-id="2f9f2-241">Type</span></span></th><th><span data-ttu-id="2f9f2-242">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="2f9f2-242">Description</span></span></th></tr><tr><td><span data-ttu-id="2f9f2-243">Användar-Id</span><span class="sxs-lookup"><span data-stu-id="2f9f2-243">User Id</span></span></td><td><span data-ttu-id="2f9f2-244">Ja</span><span class="sxs-lookup"><span data-stu-id="2f9f2-244">Yes</span></span></td><td><span data-ttu-id="2f9f2-245">Alfanumeriskt</span><span class="sxs-lookup"><span data-stu-id="2f9f2-245">Alphanumeric</span></span></td><td><span data-ttu-id="2f9f2-246">Unik identifierare för en användare</span><span class="sxs-lookup"><span data-stu-id="2f9f2-246">Unique identifier of a user</span></span></td></tr><tr><td><span data-ttu-id="2f9f2-247">Objekt-Id</span><span class="sxs-lookup"><span data-stu-id="2f9f2-247">Item Id</span></span></td><td><span data-ttu-id="2f9f2-248">Ja</span><span class="sxs-lookup"><span data-stu-id="2f9f2-248">Yes</span></span></td><td><span data-ttu-id="2f9f2-249">Längden på alfanumeriska, max 50</span><span class="sxs-lookup"><span data-stu-id="2f9f2-249">Alphanumeric, max length 50</span></span></td><td><span data-ttu-id="2f9f2-250">Unik identifierare för ett objekt</span><span class="sxs-lookup"><span data-stu-id="2f9f2-250">Unique identifier of an item</span></span></td></tr><tr><td><span data-ttu-id="2f9f2-251">Tid</span><span class="sxs-lookup"><span data-stu-id="2f9f2-251">Time</span></span></td><td><span data-ttu-id="2f9f2-252">Nej</span><span class="sxs-lookup"><span data-stu-id="2f9f2-252">No</span></span></td><td><span data-ttu-id="2f9f2-253">Datum i format: ÅÅÅÅ/MM/ddTHH (till exempel 2013/06/20T10:00:00)</span><span class="sxs-lookup"><span data-stu-id="2f9f2-253">Date in format: YYYY/MM/DDTHH:MM:SS (for example, 2013/06/20T10:00:00)</span></span></td><td><span data-ttu-id="2f9f2-254">Tid för data</span><span class="sxs-lookup"><span data-stu-id="2f9f2-254">Time of data</span></span></td></tr><tr><td><span data-ttu-id="2f9f2-255">Händelse</span><span class="sxs-lookup"><span data-stu-id="2f9f2-255">Event</span></span></td><td><span data-ttu-id="2f9f2-256">Nej, om tillhandahålls måste också sätta datum</span><span class="sxs-lookup"><span data-stu-id="2f9f2-256">No, if supplied then must also put date</span></span></td><td><span data-ttu-id="2f9f2-257">Något av följande:</span><span class="sxs-lookup"><span data-stu-id="2f9f2-257">One of the following:</span></span><br><span data-ttu-id="2f9f2-258">• Klicka på</span><span class="sxs-lookup"><span data-stu-id="2f9f2-258">• Click</span></span><br><span data-ttu-id="2f9f2-259">• RecommendationClick</span><span class="sxs-lookup"><span data-stu-id="2f9f2-259">• RecommendationClick</span></span><br><span data-ttu-id="2f9f2-260">• AddShopCart</span><span class="sxs-lookup"><span data-stu-id="2f9f2-260">•    AddShopCart</span></span><br><span data-ttu-id="2f9f2-261">• RemoveShopCart</span><span class="sxs-lookup"><span data-stu-id="2f9f2-261">• RemoveShopCart</span></span><br><span data-ttu-id="2f9f2-262">• Köp</span><span class="sxs-lookup"><span data-stu-id="2f9f2-262">• Purchase</span></span></td><td></td></tr></table><br><span data-ttu-id="2f9f2-263">Maximal filstorlek är 200MB.</span><span class="sxs-lookup"><span data-stu-id="2f9f2-263">Maximum file size is 200MB.</span></span><br><br><span data-ttu-id="2f9f2-264">Exempel:</span><span class="sxs-lookup"><span data-stu-id="2f9f2-264">Example:</span></span><br><pre>149452,1b3d95e2-84e4-414c-bb38-be9cf461c347<br>6360,1b3d95e2-84e4-414c-bb38-be9cf461c347<br>50321,1b3d95e2-84e4-414c-bb38-be9cf461c347<br>71285,1b3d95e2-84e4-414c-bb38-be9cf461c347<br>224450,1b3d95e2-84e4-414c-bb38-be9cf461c347<br>236645,1b3d95e2-84e4-414c-bb38-be9cf461c347<br>107951,1b3d95e2-84e4-414c-bb38-be9cf461c347</pre> |

<span data-ttu-id="2f9f2-265">**Svaret**:</span><span class="sxs-lookup"><span data-stu-id="2f9f2-265">**Response**:</span></span>

<span data-ttu-id="2f9f2-266">HTTP-statuskod: 200</span><span class="sxs-lookup"><span data-stu-id="2f9f2-266">HTTP Status code: 200</span></span>

* <span data-ttu-id="2f9f2-267">`Feed\entry\content\properties\LineCount`-Antal rader som accepteras.</span><span class="sxs-lookup"><span data-stu-id="2f9f2-267">`Feed\entry\content\properties\LineCount` - Number of lines accepted.</span></span>
* <span data-ttu-id="2f9f2-268">`Feed\entry\content\properties\ErrorCount`-Antal rader som inte infogades på grund av ett fel.</span><span class="sxs-lookup"><span data-stu-id="2f9f2-268">`Feed\entry\content\properties\ErrorCount` - Number of lines that were not inserted due to an error.</span></span>
* <span data-ttu-id="2f9f2-269">`Feed\entry\content\properties\FileId`-Filen identifierare.</span><span class="sxs-lookup"><span data-stu-id="2f9f2-269">`Feed\entry\content\properties\FileId` - File identifier.</span></span>

<span data-ttu-id="2f9f2-270">OData-XML</span><span class="sxs-lookup"><span data-stu-id="2f9f2-270">OData XML</span></span>

    <feed xmlns:base="https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/ImportUsageFile" xmlns:d="http://schemas.microsoft.com/ado/2007/08/dataservices" xmlns:m="http://schemas.microsoft.com/ado/2007/08/dataservices/metadata" xmlns="http://www.w3.org/2005/Atom">
      <title type="text" />
      <subtitle type="text">Add bulk usage data (usage file)</subtitle>
      <id>https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/ImportUsageFile?modelId='a658c626-2baa-43a7-ac98-f6ee26120a12'&amp;filename='ImplicitMatrix10_Guid_small.txt'&amp;apiVersion='1.0'</id>
      <rights type="text" />
      <updated>2014-10-05T07:21:44Z</updated>
      <link rel="self" href="https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/ImportUsageFile?modelId='a658c626-2baa-43a7-ac98-f6ee26120a12'&amp;filename='ImplicitMatrix10_Guid_small.txt'&amp;apiVersion='1.0'" />
      <entry>
    <id>https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/ImportUsageFile?modelId='a658c626-2baa-43a7-ac98-f6ee26120a12'&amp;filename='ImplicitMatrix10_Guid_small.txt'&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1</id>
    <title type="text">AddBulkUsageDataUsageFileEntity2</title>
    <updated>2014-10-05T07:21:44Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/ImportUsageFile?modelId='a658c626-2baa-43a7-ac98-f6ee26120a12'&amp;filename='ImplicitMatrix10_Guid_small.txt'&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1" />
    <content type="application/xml">
      <m:properties>
        <d:LineCount m:type="Edm.String">33</d:LineCount>
        <d:ErrorCount m:type="Edm.String">0</d:ErrorCount>
        <d:FileId m:type="Edm.String">fead7c1c-db01-46c0-872f-65bcda36025d</d:FileId>
      </m:properties>
    </content>
      </entry>
    </feed>


#### <a name="using-data-acquisition"></a><span data-ttu-id="2f9f2-271">Med hjälp av datainsamling</span><span class="sxs-lookup"><span data-stu-id="2f9f2-271">Using data acquisition</span></span>
<span data-ttu-id="2f9f2-272">Det här avsnittet visar hur du skickar händelser i realtid till Azure Machine Learning rekommendationer vanligtvis från webbplatsen.</span><span class="sxs-lookup"><span data-stu-id="2f9f2-272">This section shows how to send events in real time to Azure Machine Learning Recommendations, usually from your website.</span></span>

| <span data-ttu-id="2f9f2-273">HTTP-metoden</span><span class="sxs-lookup"><span data-stu-id="2f9f2-273">HTTP Method</span></span> | <span data-ttu-id="2f9f2-274">URI: N</span><span class="sxs-lookup"><span data-stu-id="2f9f2-274">URI</span></span> |
|:--- |:--- |
| <span data-ttu-id="2f9f2-275">POST</span><span class="sxs-lookup"><span data-stu-id="2f9f2-275">POST</span></span> |`<rootURI>/AddUsageEvent?apiVersion=%271.0%27-f6ee26120a12%27&filename=%27catalog10_small.txt%27&apiVersion=%271.0%27` |

| <span data-ttu-id="2f9f2-276">Parameternamn</span><span class="sxs-lookup"><span data-stu-id="2f9f2-276">Parameter Name</span></span> | <span data-ttu-id="2f9f2-277">Giltiga värden</span><span class="sxs-lookup"><span data-stu-id="2f9f2-277">Valid Values</span></span> |
|:--- |:--- |
| <span data-ttu-id="2f9f2-278">apiVersion</span><span class="sxs-lookup"><span data-stu-id="2f9f2-278">apiVersion</span></span> |<span data-ttu-id="2f9f2-279">1.0</span><span class="sxs-lookup"><span data-stu-id="2f9f2-279">1.0</span></span> |
|  | |
| <span data-ttu-id="2f9f2-280">Begärandetexten</span><span class="sxs-lookup"><span data-stu-id="2f9f2-280">Request body</span></span> |<span data-ttu-id="2f9f2-281">Händelsen inmatning för varje händelse som du vill skicka.</span><span class="sxs-lookup"><span data-stu-id="2f9f2-281">Event data entry for each event you want to send.</span></span> <span data-ttu-id="2f9f2-282">Du bör skicka för samma användare eller webbläsare session samma ID i fältet sessions-ID.</span><span class="sxs-lookup"><span data-stu-id="2f9f2-282">You should send for the same user or browser session the same ID in the SessionId field.</span></span> <span data-ttu-id="2f9f2-283">(Se exemplet för händelsen nedan.)</span><span class="sxs-lookup"><span data-stu-id="2f9f2-283">(See sample of event body below.)</span></span> |

* <span data-ttu-id="2f9f2-284">Händelse 'Klickar du på ”exempel:</span><span class="sxs-lookup"><span data-stu-id="2f9f2-284">Example for event 'Click':</span></span>
  
        <Event xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
        <ModelId>2779c063-48fb-46c1-bae3-74acddc8c1d1</ModelId>
        <SessionId>11112222</SessionId>
        <EventData>
        <EventData>
        <Name>Click</Name>
        <ItemId>21BF8088-B6C0-4509-870C-E1C7AC78304A</ItemId>
        </EventData>
        </EventData>
        </Event>
* <span data-ttu-id="2f9f2-285">Händelse 'RecommendationClick' exempel:</span><span class="sxs-lookup"><span data-stu-id="2f9f2-285">Example for event 'RecommendationClick':</span></span>
  
        <Event xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
          <ModelId>2779c063-48fb-46c1-bae3-74acddc8c1d1</ModelId>
          <SessionId>11112222</SessionId>
          <EventData>
        <EventData>
          <Name>RecommendationClick</Name>
          <ItemId>21BF8088-B6C0-4509-870C-E1C7AC78304A</ItemId>
        </EventData>
          </EventData>
        </Event>
* <span data-ttu-id="2f9f2-286">Händelse 'AddShopCart' exempel:</span><span class="sxs-lookup"><span data-stu-id="2f9f2-286">Example for event 'AddShopCart':</span></span>
  
        <Event xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
          <ModelId>2779c063-48fb-46c1-bae3-74acddc8c1d1</ModelId>
          <SessionId>11112222</SessionId>
          <EventData>
        <EventData>
          <Name>AddShopCart</Name>
          <ItemId>21BF8088-B6C0-4509-870C-E1C7AC78304A</ItemId>
        </EventData>
          </EventData>
        </Event>
* <span data-ttu-id="2f9f2-287">Händelse 'RemoveShopCart' exempel:</span><span class="sxs-lookup"><span data-stu-id="2f9f2-287">Example for event 'RemoveShopCart':</span></span>
  
        <Event xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
          <ModelId>2779c063-48fb-46c1-bae3-74acddc8c1d1</ModelId>
          <SessionId>11112222</SessionId>
          <EventData>
        <EventData>
          <Name>RemoveShopCart</Name>
          <ItemId>21BF8088-B6C0-4509-870C-E1C7AC78304A</ItemId>
        </EventData>
          </EventData>
        </Event>
* <span data-ttu-id="2f9f2-288">Händelse 'Inköp' exempel:</span><span class="sxs-lookup"><span data-stu-id="2f9f2-288">Example for event 'Purchase':</span></span>
  
        <Event xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
        <ModelId>2779c063-48fb-46c1-bae3-74acddc8c1d1</ModelId>
        <SessionId>11112222</SessionId>
        <EventData>
        <EventData>
            <Name>Purchase</Name> 
            <PurchaseItems>
            <PurchaseItems>
                <ItemId>21BF8088-B6C0-4509-870C-E1C7AC78304A</ItemId>
                <Count>3</Count>
            </PurchaseItems>
        </PurchaseItems>
        </EventData>
        </EventData>
        </Event>
* <span data-ttu-id="2f9f2-289">Exempel skickar 2 händelser, klickar du på' och 'AddShopCart':</span><span class="sxs-lookup"><span data-stu-id="2f9f2-289">Example sending 2 events, 'Click' and 'AddShopCart':</span></span>
  
        <Event xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
          <ModelId>2779c063-48fb-46c1-bae3-74acddc8c1d1</ModelId>
          <SessionId>11112222</SessionId>
          <EventData>
        <EventData>
          <Name>Click</Name>
          <ItemId>21BF8088-B6C0-4509-870C-E1C7AC78304A</ItemId>
          <ItemName>itemName</ItemName>
          <ItemDescription>item description</ItemDescription>
          <ItemCategory>category</ItemCategory>
        </EventData>
        <EventData>
          <Name>AddShopCart</Name>
          <ItemId>552A1940-21E4-4399-82BB-594B46D7ED54</ItemId>
        </EventData>
          </EventData>
        </Event>

<span data-ttu-id="2f9f2-290">**Svaret**: HTTP-statuskod: 200</span><span class="sxs-lookup"><span data-stu-id="2f9f2-290">**Response**: HTTP Status code: 200</span></span>

### <a name="build-a-recommendation-model"></a><span data-ttu-id="2f9f2-291">Skapa en rekommendation modell</span><span class="sxs-lookup"><span data-stu-id="2f9f2-291">Build a recommendation model</span></span>
| <span data-ttu-id="2f9f2-292">HTTP-metoden</span><span class="sxs-lookup"><span data-stu-id="2f9f2-292">HTTP Method</span></span> | <span data-ttu-id="2f9f2-293">URI: N</span><span class="sxs-lookup"><span data-stu-id="2f9f2-293">URI</span></span> |
|:--- |:--- |
| <span data-ttu-id="2f9f2-294">POST</span><span class="sxs-lookup"><span data-stu-id="2f9f2-294">POST</span></span> |`<rootURI>/BuildModel?modelId=%27<modelId>%27&userDescription=%27<description>%27&apiVersion=%271.0%27`<br><br><span data-ttu-id="2f9f2-295">Exempel:</span><span class="sxs-lookup"><span data-stu-id="2f9f2-295">Example:</span></span><br>`<rootURI>/BuildModel?modelId=%27a658c626-2baa-43a7-ac98-f6ee26120a12%27&userDescription=%27First%20build%27&apiVersion=%271.0%27` |

| <span data-ttu-id="2f9f2-296">Parameternamn</span><span class="sxs-lookup"><span data-stu-id="2f9f2-296">Parameter Name</span></span> | <span data-ttu-id="2f9f2-297">Giltiga värden</span><span class="sxs-lookup"><span data-stu-id="2f9f2-297">Valid Values</span></span> |
|:--- |:--- |
| <span data-ttu-id="2f9f2-298">%{ModelID/</span><span class="sxs-lookup"><span data-stu-id="2f9f2-298">modelId</span></span> |<span data-ttu-id="2f9f2-299">Unik identifierare för modellen (skiftlägeskänsligt)</span><span class="sxs-lookup"><span data-stu-id="2f9f2-299">Unique identifier of the model (case-sensitive)</span></span> |
| <span data-ttu-id="2f9f2-300">userDescription</span><span class="sxs-lookup"><span data-stu-id="2f9f2-300">userDescription</span></span> |<span data-ttu-id="2f9f2-301">Textrepresentation identifierare för katalogen.</span><span class="sxs-lookup"><span data-stu-id="2f9f2-301">Textual identifier of the catalog.</span></span> <span data-ttu-id="2f9f2-302">Observera att om du använder lagringsutrymmen du koda den med % 20 i stället.</span><span class="sxs-lookup"><span data-stu-id="2f9f2-302">Note that if you use spaces you must encode it with %20 instead.</span></span> <span data-ttu-id="2f9f2-303">Finns i exemplet ovan.</span><span class="sxs-lookup"><span data-stu-id="2f9f2-303">See example above.</span></span><br><span data-ttu-id="2f9f2-304">Maxlängd: 50</span><span class="sxs-lookup"><span data-stu-id="2f9f2-304">Max length: 50</span></span> |
| <span data-ttu-id="2f9f2-305">apiVersion</span><span class="sxs-lookup"><span data-stu-id="2f9f2-305">apiVersion</span></span> |<span data-ttu-id="2f9f2-306">1.0</span><span class="sxs-lookup"><span data-stu-id="2f9f2-306">1.0</span></span> |
|  | |
| <span data-ttu-id="2f9f2-307">Begärandetext</span><span class="sxs-lookup"><span data-stu-id="2f9f2-307">Request Body</span></span> |<span data-ttu-id="2f9f2-308">INGEN</span><span class="sxs-lookup"><span data-stu-id="2f9f2-308">NONE</span></span> |

<span data-ttu-id="2f9f2-309">**Svaret**:</span><span class="sxs-lookup"><span data-stu-id="2f9f2-309">**Response**:</span></span>

<span data-ttu-id="2f9f2-310">HTTP-statuskod: 200</span><span class="sxs-lookup"><span data-stu-id="2f9f2-310">HTTP Status code: 200</span></span>

<span data-ttu-id="2f9f2-311">Detta är en asynkron API.</span><span class="sxs-lookup"><span data-stu-id="2f9f2-311">This is an asynchronous API.</span></span> <span data-ttu-id="2f9f2-312">Du får ett build-ID som en respons.</span><span class="sxs-lookup"><span data-stu-id="2f9f2-312">You will get a build ID as a response.</span></span> <span data-ttu-id="2f9f2-313">Om du vill veta när versionen har avslutats, bör du anropa API för ”hämta bygger Status för en modell” och leta upp den här build-ID i svaret.</span><span class="sxs-lookup"><span data-stu-id="2f9f2-313">To know when the build has ended, you should call the "Get Builds Status of a Model" API and locate this build ID in the response.</span></span> <span data-ttu-id="2f9f2-314">Observera att en version kan ta från minuter till timmar, beroende på storleken på data.</span><span class="sxs-lookup"><span data-stu-id="2f9f2-314">Note that a build can take from minutes to hours depending on the size of the data.</span></span>

<span data-ttu-id="2f9f2-315">Du kan inte använda rekommendationer till build-versionen slutar.</span><span class="sxs-lookup"><span data-stu-id="2f9f2-315">You cannot consume recommendations till the build ends.</span></span>

<span data-ttu-id="2f9f2-316">Giltig build-status:</span><span class="sxs-lookup"><span data-stu-id="2f9f2-316">Valid build status:</span></span>

* <span data-ttu-id="2f9f2-317">Skapa – modellen skapades.</span><span class="sxs-lookup"><span data-stu-id="2f9f2-317">Create – Model was created.</span></span>
* <span data-ttu-id="2f9f2-318">I kö – modellen build utlöstes och den är i kö.</span><span class="sxs-lookup"><span data-stu-id="2f9f2-318">Queued – Model build was triggered and it is queued.</span></span>
* <span data-ttu-id="2f9f2-319">Skapa – modellen skapas.</span><span class="sxs-lookup"><span data-stu-id="2f9f2-319">Building – Model is being built.</span></span>
* <span data-ttu-id="2f9f2-320">Lyckade – Build slutförts utan fel.</span><span class="sxs-lookup"><span data-stu-id="2f9f2-320">Success – Build ended successfully.</span></span>
* <span data-ttu-id="2f9f2-321">Fel – Build avslutades med ett fel.</span><span class="sxs-lookup"><span data-stu-id="2f9f2-321">Error – Build ended with a failure.</span></span>
* <span data-ttu-id="2f9f2-322">Annullerat – avbröts skapa.</span><span class="sxs-lookup"><span data-stu-id="2f9f2-322">Canceled – Build was canceled.</span></span>
* <span data-ttu-id="2f9f2-323">Om du avbryter – Build avbryts.</span><span class="sxs-lookup"><span data-stu-id="2f9f2-323">Canceling – Build is being canceled.</span></span>

<span data-ttu-id="2f9f2-324">Observera att build-ID: T finns på följande sökväg:`Feed\entry\content\properties\Id`</span><span class="sxs-lookup"><span data-stu-id="2f9f2-324">Note that the build ID can be found under the following path: `Feed\entry\content\properties\Id`</span></span>

<span data-ttu-id="2f9f2-325">OData-XML</span><span class="sxs-lookup"><span data-stu-id="2f9f2-325">OData XML</span></span>

    <feed xmlns:base="https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/BuildModel" xmlns:d="http://schemas.microsoft.com/ado/2007/08/dataservices" xmlns:m="http://schemas.microsoft.com/ado/2007/08/dataservices/metadata" xmlns="http://www.w3.org/2005/Atom">
      <title type="text" />
      <subtitle type="text">Build a Model with RequestBody</subtitle>
      <id>https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/BuildModel?modelId='9559872f-7a53-4076-a3c7-19d9385c1265'&amp;userDescription='First build'&amp;apiVersion='1.0'</id>
      <rights type="text" />
      <updated>2014-10-05T08:56:34Z</updated>
      <link rel="self" href="https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/BuildModel?modelId='9559872f-7a53-4076-a3c7-19d9385c1265'&amp;userDescription='First%20build'&amp;apiVersion='1.0'" />
      <entry>
    <id>https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/BuildModel?modelId='9559872f-7a53-4076-a3c7-19d9385c1265'&amp;userDescription='First build'&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1</id>
    <title type="text">BuildAModelEntity2</title>
    <updated>2014-10-05T08:56:34Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/BuildModel?modelId='9559872f-7a53-4076-a3c7-19d9385c1265'&amp;userDescription='First%20build'&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1" />
    <content type="application/xml">
      <m:properties>
        <d:Id m:type="Edm.String">1000272</d:Id>
        <d:UserName m:type="Edm.String"></d:UserName>
        <d:ModelId m:type="Edm.String">9559872f-7a53-4076-a3c7-19d9385c1265</d:ModelId>
        <d:ModelName m:type="Edm.String">docTest</d:ModelName>
        <d:Type m:type="Edm.String">Recommendation</d:Type>
        <d:CreationTime m:type="Edm.String">2014-10-05T08:56:31.893</d:CreationTime>
        <d:Progress_BuildId m:type="Edm.String">1000272</d:Progress_BuildId>
        <d:Progress_ModelId m:type="Edm.String">9559872f-7a53-4076-a3c7-19d9385c1265</d:Progress_ModelId>
        <d:Progress_UserName m:type="Edm.String">5-4058-ab36-1fe254f05102@dm.com</d:Progress_UserName>
        <d:Progress_IsExecutionStarted m:type="Edm.String">false</d:Progress_IsExecutionStarted>
        <d:Progress_IsExecutionEnded m:type="Edm.String">false</d:Progress_IsExecutionEnded>
        <d:Progress_Percent m:type="Edm.String">0</d:Progress_Percent>
        <d:Progress_StartTime m:type="Edm.String">0001-01-01T00:00:00</d:Progress_StartTime>
        <d:Progress_EndTime m:type="Edm.String">0001-01-01T00:00:00</d:Progress_EndTime>
        <d:Progress_UpdateDateUTC m:type="Edm.String"></d:Progress_UpdateDateUTC>
        <d:Status m:type="Edm.String">Queued</d:Status>
        <d:Key1 m:type="Edm.String">UseFeaturesInModel</d:Key1>
        <d:Value1 m:type="Edm.String">False</d:Value1>
      </m:properties>
    </content>
      </entry>
    </feed>

### <a name="get-build-status-of-a-model"></a><span data-ttu-id="2f9f2-326">Hämta build-status för en modell</span><span class="sxs-lookup"><span data-stu-id="2f9f2-326">Get build status of a model</span></span>
| <span data-ttu-id="2f9f2-327">HTTP-metoden</span><span class="sxs-lookup"><span data-stu-id="2f9f2-327">HTTP Method</span></span> | <span data-ttu-id="2f9f2-328">URI: N</span><span class="sxs-lookup"><span data-stu-id="2f9f2-328">URI</span></span> |
|:--- |:--- |
| <span data-ttu-id="2f9f2-329">HÄMTA</span><span class="sxs-lookup"><span data-stu-id="2f9f2-329">GET</span></span> |`<rootURI>/GetModelBuildsStatus?modelId=%27<modelId>%27&onlyLastBuild=<bool>&apiVersion=%271.0%27`<br><br><span data-ttu-id="2f9f2-330">Exempel:</span><span class="sxs-lookup"><span data-stu-id="2f9f2-330">Example:</span></span><br>`<rootURI>/GetModelBuildsStatus?modelId=%279559872f-7a53-4076-a3c7-19d9385c1265%27&onlyLastBuild=true&apiVersion=%271.0%27` |

| <span data-ttu-id="2f9f2-331">Parameternamn</span><span class="sxs-lookup"><span data-stu-id="2f9f2-331">Parameter Name</span></span> | <span data-ttu-id="2f9f2-332">Giltiga värden</span><span class="sxs-lookup"><span data-stu-id="2f9f2-332">Valid Values</span></span> |
|:--- |:--- |
| <span data-ttu-id="2f9f2-333">%{ModelID/</span><span class="sxs-lookup"><span data-stu-id="2f9f2-333">modelId</span></span> |<span data-ttu-id="2f9f2-334">Unik identifierare för modellen (skiftlägeskänsligt)</span><span class="sxs-lookup"><span data-stu-id="2f9f2-334">Unique identifier of the model  (case-sensitive)</span></span> |
| <span data-ttu-id="2f9f2-335">onlyLastBuild</span><span class="sxs-lookup"><span data-stu-id="2f9f2-335">onlyLastBuild</span></span> |<span data-ttu-id="2f9f2-336">Anger om du vill returnera alla build-historik för modellen eller bara statusen för den senaste versionen.</span><span class="sxs-lookup"><span data-stu-id="2f9f2-336">Indicates whether to return all the build history of the model or only the status of the most recent build.</span></span> |
| <span data-ttu-id="2f9f2-337">apiVersion</span><span class="sxs-lookup"><span data-stu-id="2f9f2-337">apiVersion</span></span> |<span data-ttu-id="2f9f2-338">1.0</span><span class="sxs-lookup"><span data-stu-id="2f9f2-338">1.0</span></span> |

<span data-ttu-id="2f9f2-339">**Svaret**:</span><span class="sxs-lookup"><span data-stu-id="2f9f2-339">**Response**:</span></span>

<span data-ttu-id="2f9f2-340">HTTP-statuskod: 200</span><span class="sxs-lookup"><span data-stu-id="2f9f2-340">HTTP Status code: 200</span></span>

<span data-ttu-id="2f9f2-341">Svaret innehåller en post per version.</span><span class="sxs-lookup"><span data-stu-id="2f9f2-341">The response includes one entry per build.</span></span> <span data-ttu-id="2f9f2-342">Varje post innehåller följande information:</span><span class="sxs-lookup"><span data-stu-id="2f9f2-342">Each entry has the following data:</span></span>

* <span data-ttu-id="2f9f2-343">`feed/entry/content/properties/UserName`– Namnet på användaren.</span><span class="sxs-lookup"><span data-stu-id="2f9f2-343">`feed/entry/content/properties/UserName` – Name of the user.</span></span>
* <span data-ttu-id="2f9f2-344">`feed/entry/content/properties/ModelName`– Namnet på modellen.</span><span class="sxs-lookup"><span data-stu-id="2f9f2-344">`feed/entry/content/properties/ModelName` – Name of the model.</span></span>
* <span data-ttu-id="2f9f2-345">`feed/entry/content/properties/ModelId`– Modell Unik identifierare.</span><span class="sxs-lookup"><span data-stu-id="2f9f2-345">`feed/entry/content/properties/ModelId` – Model unique identifier.</span></span>
* <span data-ttu-id="2f9f2-346">`feed/entry/content/properties/IsDeployed`– Om versionen distribueras (kallas även</span><span class="sxs-lookup"><span data-stu-id="2f9f2-346">`feed/entry/content/properties/IsDeployed` – Whether the build is deployed (a.k.a.</span></span> <span data-ttu-id="2f9f2-347">aktiva build).</span><span class="sxs-lookup"><span data-stu-id="2f9f2-347">active build).</span></span>
* <span data-ttu-id="2f9f2-348">`feed/entry/content/properties/BuildId`– Skapa Unik identifierare.</span><span class="sxs-lookup"><span data-stu-id="2f9f2-348">`feed/entry/content/properties/BuildId` – Build unique identifier.</span></span>
* <span data-ttu-id="2f9f2-349">`feed/entry/content/properties/BuildType`-Typen av versionen.</span><span class="sxs-lookup"><span data-stu-id="2f9f2-349">`feed/entry/content/properties/BuildType` - Type of the build.</span></span>
* <span data-ttu-id="2f9f2-350">`feed/entry/content/properties/Status`– Skapa status.</span><span class="sxs-lookup"><span data-stu-id="2f9f2-350">`feed/entry/content/properties/Status` – Build status.</span></span> <span data-ttu-id="2f9f2-351">Kan vara något av följande: fel, skapa, i kö, Canceling, Avbruten, lyckades</span><span class="sxs-lookup"><span data-stu-id="2f9f2-351">Can be one of the following: Error, Building, Queued, Canceling, Canceled, Success</span></span>
* <span data-ttu-id="2f9f2-352">`feed/entry/content/properties/StatusMessage`– Detaljerad statusmeddelande (gäller endast på vissa tillstånd).</span><span class="sxs-lookup"><span data-stu-id="2f9f2-352">`feed/entry/content/properties/StatusMessage` – Detailed status message (applies only to specific states).</span></span>
* <span data-ttu-id="2f9f2-353">`feed/entry/content/properties/Progress`– Skapa förlopp (%).</span><span class="sxs-lookup"><span data-stu-id="2f9f2-353">`feed/entry/content/properties/Progress` – Build progress (%).</span></span>
* <span data-ttu-id="2f9f2-354">`feed/entry/content/properties/StartTime`– Skapa starttid.</span><span class="sxs-lookup"><span data-stu-id="2f9f2-354">`feed/entry/content/properties/StartTime` – Build start time.</span></span>
* <span data-ttu-id="2f9f2-355">`feed/entry/content/properties/EndTime`– Skapa sluttid.</span><span class="sxs-lookup"><span data-stu-id="2f9f2-355">`feed/entry/content/properties/EndTime` – Build end time.</span></span>
* <span data-ttu-id="2f9f2-356">`feed/entry/content/properties/ExecutionTime`– Skapa varaktighet.</span><span class="sxs-lookup"><span data-stu-id="2f9f2-356">`feed/entry/content/properties/ExecutionTime` – Build duration.</span></span>
* <span data-ttu-id="2f9f2-357">`feed/entry/content/properties/ProgressStep`– Information om det aktuella steget som en build pågår.</span><span class="sxs-lookup"><span data-stu-id="2f9f2-357">`feed/entry/content/properties/ProgressStep` – Details about the current stage that a build in progress is in.</span></span>

<span data-ttu-id="2f9f2-358">Giltig build-status:</span><span class="sxs-lookup"><span data-stu-id="2f9f2-358">Valid build status:</span></span>

* <span data-ttu-id="2f9f2-359">Skapad – skapades skapa posten.</span><span class="sxs-lookup"><span data-stu-id="2f9f2-359">Created – Build request entry was created.</span></span>
* <span data-ttu-id="2f9f2-360">I kö – skapa begäran utlöstes och den är i kö.</span><span class="sxs-lookup"><span data-stu-id="2f9f2-360">Queued – Build request was triggered and it is queued.</span></span>
* <span data-ttu-id="2f9f2-361">Skapa – Build pågår.</span><span class="sxs-lookup"><span data-stu-id="2f9f2-361">Building – Build is in process.</span></span>
* <span data-ttu-id="2f9f2-362">Lyckade – Build slutförts utan fel.</span><span class="sxs-lookup"><span data-stu-id="2f9f2-362">Success – Build ended successfully.</span></span>
* <span data-ttu-id="2f9f2-363">Fel – Build avslutades med ett fel.</span><span class="sxs-lookup"><span data-stu-id="2f9f2-363">Error – Build ended with a failure.</span></span>
* <span data-ttu-id="2f9f2-364">Annullerat – avbröts skapa.</span><span class="sxs-lookup"><span data-stu-id="2f9f2-364">Canceled – Build was canceled.</span></span>
* <span data-ttu-id="2f9f2-365">Om du avbryter – Build avbryts.</span><span class="sxs-lookup"><span data-stu-id="2f9f2-365">Canceling – Build is being canceled.</span></span>

<span data-ttu-id="2f9f2-366">Giltiga värden för build-typ:</span><span class="sxs-lookup"><span data-stu-id="2f9f2-366">Valid values for build type:</span></span>

* <span data-ttu-id="2f9f2-367">Rangordning - skapa rang.</span><span class="sxs-lookup"><span data-stu-id="2f9f2-367">Rank - Rank build.</span></span> <span data-ttu-id="2f9f2-368">(För rang skapa information, se dokumentet ”Machine Learning rekommendation API-dokumentation”)</span><span class="sxs-lookup"><span data-stu-id="2f9f2-368">(For rank build details, please refer to the "Machine Learning Recommendation API documentation" document.)</span></span>
* <span data-ttu-id="2f9f2-369">Rekommendation - rekommendation build.</span><span class="sxs-lookup"><span data-stu-id="2f9f2-369">Recommendation - Recommendation build.</span></span>
* <span data-ttu-id="2f9f2-370">Fbt - köpt ofta tillsammans build.</span><span class="sxs-lookup"><span data-stu-id="2f9f2-370">Fbt - Frequently bought together build.</span></span>

<span data-ttu-id="2f9f2-371">OData-XML</span><span class="sxs-lookup"><span data-stu-id="2f9f2-371">OData XML</span></span>

    <feed xmlns:base="https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/GetModelBuildsStatus" xmlns:d="http://schemas.microsoft.com/ado/2007/08/dataservices" xmlns:m="http://schemas.microsoft.com/ado/2007/08/dataservices/metadata" xmlns="http://www.w3.org/2005/Atom">
    <title type="text" />
    <subtitle type="text">Get builds status of a model</subtitle>
    <id>https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/GetModelBuildsStatus?modelId='1d20c34f-dca1-4eac-8e5d-f299e4e4ad66'&amp;onlyLastBuild=False&amp;apiVersion='1.0'</id>
    <rights type="text" />
    <updated>2014-11-05T17:51:10Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/GetModelBuildsStatus?modelId='1d20c34f-dca1-4eac-8e5d-f299e4e4ad66'&amp;onlyLastBuild=False&amp;apiVersion='1.0'" />
    <entry>
    <id>https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/GetModelBuildsStatus?modelId='1d20c34f-dca1-4eac-8e5d-f299e4e4ad66'&amp;onlyLastBuild=False&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1</id>
    <title type="text">GetBuildsStatusEntity</title>
    <updated>2014-11-05T17:51:10Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/GetModelBuildsStatus?modelId='1d20c34f-dca1-4eac-8e5d-f299e4e4ad66'&amp;onlyLastBuild=False&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1" />
    <content type="application/xml">
      <m:properties>
        <d:UserName m:type="Edm.String">b-434e-b2c9-84935664ff20@dm.com</d:UserName>
        <d:ModelName m:type="Edm.String">ModelName</d:ModelName>
        <d:ModelId m:type="Edm.String">1d20c34f-dca1-4eac-8e5d-f299e4e4ad66</d:ModelId>
        <d:IsDeployed m:type="Edm.String">true</d:IsDeployed>
        <d:BuildId m:type="Edm.String">1000272</d:BuildId>
        <d:BuildType m:type="Edm.String">Recommendation</d:BuildType>
        <d:Status m:type="Edm.String">Success</d:Status>
        <d:StatusMessage m:type="Edm.String"></d:StatusMessage>
        <d:Progress m:type="Edm.String">0</d:Progress>
        <d:StartTime m:type="Edm.String">2014-11-02T13:43:51</d:StartTime>
        <d:EndTime m:type="Edm.String">2014-11-02T13:45:10</d:EndTime>
        <d:ExecutionTime m:type="Edm.String">00:01:19</d:ExecutionTime>
        <d:IsExecutionStarted m:type="Edm.String">false</d:IsExecutionStarted>
        <d:ProgressStep m:type="Edm.String"></d:ProgressStep>
      </m:properties>
     </content>
    </entry>
    </feed>


### <a name="get-recommendations"></a><span data-ttu-id="2f9f2-372">Få rekommendationer</span><span class="sxs-lookup"><span data-stu-id="2f9f2-372">Get recommendations</span></span>
| <span data-ttu-id="2f9f2-373">HTTP-metoden</span><span class="sxs-lookup"><span data-stu-id="2f9f2-373">HTTP Method</span></span> | <span data-ttu-id="2f9f2-374">URI: N</span><span class="sxs-lookup"><span data-stu-id="2f9f2-374">URI</span></span> |
|:--- |:--- |
| <span data-ttu-id="2f9f2-375">HÄMTA</span><span class="sxs-lookup"><span data-stu-id="2f9f2-375">GET</span></span> |`<rootURI>/ItemRecommend?modelId=%27<modelId>%27&itemIds=%27<itemId>%27&numberOfResults=<int>&includeMetadata=<bool>&apiVersion=%271.0%27`<br><br><span data-ttu-id="2f9f2-376">Exempel:</span><span class="sxs-lookup"><span data-stu-id="2f9f2-376">Example:</span></span><br>`<rootURI>/ItemRecommend?modelId=%272779c063-48fb-46c1-bae3-74acddc8c1d1%27&itemIds=%271003%27&numberOfResults=10&includeMetadata=false&apiVersion=%271.0%27` |

| <span data-ttu-id="2f9f2-377">Parameternamn</span><span class="sxs-lookup"><span data-stu-id="2f9f2-377">Parameter Name</span></span> | <span data-ttu-id="2f9f2-378">Giltiga värden</span><span class="sxs-lookup"><span data-stu-id="2f9f2-378">Valid Values</span></span> |
|:--- |:--- |
| <span data-ttu-id="2f9f2-379">%{ModelID/</span><span class="sxs-lookup"><span data-stu-id="2f9f2-379">modelId</span></span> |<span data-ttu-id="2f9f2-380">Unik identifierare för modellen (skiftlägeskänsligt)</span><span class="sxs-lookup"><span data-stu-id="2f9f2-380">Unique identifier of the model (case-sensitive)</span></span> |
| <span data-ttu-id="2f9f2-381">ItemId</span><span class="sxs-lookup"><span data-stu-id="2f9f2-381">itemIds</span></span> |<span data-ttu-id="2f9f2-382">Kommaavgränsad lista över objekt att rekommenderar för.</span><span class="sxs-lookup"><span data-stu-id="2f9f2-382">Comma-separated list of the items to recommend for.</span></span><br><span data-ttu-id="2f9f2-383">Maxlängd: 1024</span><span class="sxs-lookup"><span data-stu-id="2f9f2-383">Max length: 1024</span></span> |
| <span data-ttu-id="2f9f2-384">numberOfResults</span><span class="sxs-lookup"><span data-stu-id="2f9f2-384">numberOfResults</span></span> |<span data-ttu-id="2f9f2-385">Antalet nödvändiga resultat</span><span class="sxs-lookup"><span data-stu-id="2f9f2-385">Number of required results</span></span> |
| <span data-ttu-id="2f9f2-386">includeMetatadata</span><span class="sxs-lookup"><span data-stu-id="2f9f2-386">includeMetatadata</span></span> |<span data-ttu-id="2f9f2-387">Framtida användning alltid false</span><span class="sxs-lookup"><span data-stu-id="2f9f2-387">Future use, always false</span></span> |
| <span data-ttu-id="2f9f2-388">apiVersion</span><span class="sxs-lookup"><span data-stu-id="2f9f2-388">apiVersion</span></span> |<span data-ttu-id="2f9f2-389">1.0</span><span class="sxs-lookup"><span data-stu-id="2f9f2-389">1.0</span></span> |

<span data-ttu-id="2f9f2-390">**Svar:**</span><span class="sxs-lookup"><span data-stu-id="2f9f2-390">**Response:**</span></span>

<span data-ttu-id="2f9f2-391">HTTP-statuskod: 200</span><span class="sxs-lookup"><span data-stu-id="2f9f2-391">HTTP Status code: 200</span></span>

<span data-ttu-id="2f9f2-392">Svaret innehåller en post per rekommenderade objekt.</span><span class="sxs-lookup"><span data-stu-id="2f9f2-392">The response includes one entry per recommended item.</span></span> <span data-ttu-id="2f9f2-393">Varje post innehåller följande information:</span><span class="sxs-lookup"><span data-stu-id="2f9f2-393">Each entry has the following data:</span></span>

* <span data-ttu-id="2f9f2-394">`Feed\entry\content\properties\Id`-Rekommenderade objekt-ID.</span><span class="sxs-lookup"><span data-stu-id="2f9f2-394">`Feed\entry\content\properties\Id` - Recommended item ID.</span></span>
* <span data-ttu-id="2f9f2-395">`Feed\entry\content\properties\Name`-Namnet på objektet.</span><span class="sxs-lookup"><span data-stu-id="2f9f2-395">`Feed\entry\content\properties\Name` - Name of the item.</span></span>
* <span data-ttu-id="2f9f2-396">`Feed\entry\content\properties\Rating`-Klassificering av rekommendation. högre nummer innebär högre tillförlitlighet.</span><span class="sxs-lookup"><span data-stu-id="2f9f2-396">`Feed\entry\content\properties\Rating` - Rating of the recommendation; higher number means higher confidence.</span></span>
* <span data-ttu-id="2f9f2-397">`Feed\entry\content\properties\Reasoning`-Rekommendation motivationen (till exempel rekommendation förklaringar).</span><span class="sxs-lookup"><span data-stu-id="2f9f2-397">`Feed\entry\content\properties\Reasoning` - Recommendation reasoning (for example, recommendation explanations).</span></span>

<span data-ttu-id="2f9f2-398">OData-XML</span><span class="sxs-lookup"><span data-stu-id="2f9f2-398">OData XML</span></span>

<span data-ttu-id="2f9f2-399">Svaret exemplet nedan innehåller 10 rekommenderade objekt:</span><span class="sxs-lookup"><span data-stu-id="2f9f2-399">The example response below includes 10 recommended items:</span></span>

    <feed xmlns:base="https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/ItemRecommend" xmlns:d="http://schemas.microsoft.com/ado/2007/08/dataservices" xmlns:m="http://schemas.microsoft.com/ado/2007/08/dataservices/metadata" xmlns="http://www.w3.org/2005/Atom">
      <title type="text" />
      <subtitle type="text">Get Recommendation</subtitle>
      <id>https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'</id>
      <rights type="text" />
      <updated>2014-10-05T12:28:48Z</updated>
      <link rel="self" href="https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'" />
      <entry>
    <id>https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1</id>
    <title type="text">GetRecommendationEntity</title>
    <updated>2014-10-05T12:28:48Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1" />
    <content type="application/xml">
      <m:properties>
        <d:Id m:type="Edm.String">159</d:Id>
        <d:Name m:type="Edm.String">159</d:Name>
        <d:Rating m:type="Edm.Double">0.543343480387708</d:Rating>
        <d:Reasoning m:type="Edm.String">People who like '1003' also like '159'</d:Reasoning>
      </m:properties>
    </content>
      </entry>
      <entry>
    <id>https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=1&amp;$top=1</id>
    <title type="text">GetRecommendationEntity</title>
    <updated>2014-10-05T12:28:48Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=1&amp;$top=1" />
    <content type="application/xml">
      <m:properties>
        <d:Id m:type="Edm.String">52</d:Id>
        <d:Name m:type="Edm.String">52</d:Name>
        <d:Rating m:type="Edm.Double">0.539588900748721</d:Rating>
        <d:Reasoning m:type="Edm.String">People who like '1003' also like '52'</d:Reasoning>
      </m:properties>
    </content>
      </entry>
      <entry>
    <id>https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=2&amp;$top=1</id>
    <title type="text">GetRecommendationEntity</title>
    <updated>2014-10-05T12:28:48Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=2&amp;$top=1" />
    <content type="application/xml">
      <m:properties>
        <d:Id m:type="Edm.String">35</d:Id>
        <d:Name m:type="Edm.String">35</d:Name>
        <d:Rating m:type="Edm.Double">0.53842946443853</d:Rating>
        <d:Reasoning m:type="Edm.String">People who like '1003' also like '35'</d:Reasoning>
      </m:properties>
    </content>
      </entry>
      <entry>
    <id>https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=3&amp;$top=1</id>
    <title type="text">GetRecommendationEntity</title>
    <updated>2014-10-05T12:28:48Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=3&amp;$top=1" />
    <content type="application/xml">
      <m:properties>
        <d:Id m:type="Edm.String">124</d:Id>
        <d:Name m:type="Edm.String">124</d:Name>
        <d:Rating m:type="Edm.Double">0.536712832792886</d:Rating>
        <d:Reasoning m:type="Edm.String">People who like '1003' also like '124'</d:Reasoning>
      </m:properties>
    </content>
      </entry>
      <entry>
    <id>https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=4&amp;$top=1</id>
    <title type="text">GetRecommendationEntity</title>
    <updated>2014-10-05T12:28:48Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=4&amp;$top=1" />
    <content type="application/xml">
      <m:properties>
        <d:Id m:type="Edm.String">120</d:Id>
        <d:Name m:type="Edm.String">120</d:Name>
        <d:Rating m:type="Edm.Double">0.533673023762878</d:Rating>
        <d:Reasoning m:type="Edm.String">People who like '1003' also like '120'</d:Reasoning>
      </m:properties>
    </content>
      </entry>
      <entry>
    <id>https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=5&amp;$top=1</id>
    <title type="text">GetRecommendationEntity</title>
    <updated>2014-10-05T12:28:48Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=5&amp;$top=1" />
    <content type="application/xml">
      <m:properties>
        <d:Id m:type="Edm.String">96</d:Id>
        <d:Name m:type="Edm.String">96</d:Name>
        <d:Rating m:type="Edm.Double">0.532544826370521</d:Rating>
        <d:Reasoning m:type="Edm.String">People who like '1003' also like '96'</d:Reasoning>
      </m:properties>
    </content>
      </entry>
      <entry>
    <id>https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=6&amp;$top=1</id>
    <title type="text">GetRecommendationEntity</title>
    <updated>2014-10-05T12:28:48Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=6&amp;$top=1" />
    <content type="application/xml">
      <m:properties>
        <d:Id m:type="Edm.String">69</d:Id>
        <d:Name m:type="Edm.String">69</d:Name>
        <d:Rating m:type="Edm.Double">0.531678607847759</d:Rating>
        <d:Reasoning m:type="Edm.String">People who like '1003' also like '69'</d:Reasoning>
      </m:properties>
    </content>
      </entry>
      <entry>
    <id>https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=7&amp;$top=1</id>
    <title type="text">GetRecommendationEntity</title>
    <updated>2014-10-05T12:28:48Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=7&amp;$top=1" />
    <content type="application/xml">
      <m:properties>
        <d:Id m:type="Edm.String">172</d:Id>
        <d:Name m:type="Edm.String">172</d:Name>
        <d:Rating m:type="Edm.Double">0.530957821375951</d:Rating>
        <d:Reasoning m:type="Edm.String">People who like '1003' also like '172'</d:Reasoning>
      </m:properties>
    </content>
      </entry>
      <entry>
    <id>https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=8&amp;$top=1</id>
    <title type="text">GetRecommendationEntity</title>
    <updated>2014-10-05T12:28:48Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=8&amp;$top=1" />
    <content type="application/xml">
      <m:properties>
        <d:Id m:type="Edm.String">155</d:Id>
        <d:Name m:type="Edm.String">155</d:Name>
        <d:Rating m:type="Edm.Double">0.529093541481333</d:Rating>
        <d:Reasoning m:type="Edm.String">People who like '1003' also like '155'</d:Reasoning>
      </m:properties>
    </content>
      </entry>
      <entry>
    <id>https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=9&amp;$top=1</id>
    <title type="text">GetRecommendationEntity</title>
    <updated>2014-10-05T12:28:48Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=9&amp;$top=1" />
    <content type="application/xml">
      <m:properties>
        <d:Id m:type="Edm.String">32</d:Id>
        <d:Name m:type="Edm.String">32</d:Name>
        <d:Rating m:type="Edm.Double">0.528917978168322</d:Rating>
        <d:Reasoning m:type="Edm.String">People who like '1003' also like '32'</d:Reasoning>
      </m:properties>
    </content>
      </entry>
    </feed>

### <a name="update-model"></a><span data-ttu-id="2f9f2-400">Uppdatera modellen</span><span class="sxs-lookup"><span data-stu-id="2f9f2-400">Update model</span></span>
<span data-ttu-id="2f9f2-401">Du kan uppdatera modellbeskrivning eller aktiva build-ID.</span><span class="sxs-lookup"><span data-stu-id="2f9f2-401">You can update the model description or the active build ID.</span></span>
<span data-ttu-id="2f9f2-402">*Aktiva build-ID* -varje build för varje modell har ett build-ID.</span><span class="sxs-lookup"><span data-stu-id="2f9f2-402">*Active build ID* - Every build for every model has a build ID.</span></span> <span data-ttu-id="2f9f2-403">Aktiva build-ID är den första lyckade versionen av varje ny modell.</span><span class="sxs-lookup"><span data-stu-id="2f9f2-403">The active build ID is the first successful build of every new model.</span></span> <span data-ttu-id="2f9f2-404">När du har en aktiv build-ID och du göra ytterligare skapar för samma modellen måste du uttryckligen anges som standard build-ID om du vill.</span><span class="sxs-lookup"><span data-stu-id="2f9f2-404">Once you have an active build ID and you do additional builds for the same model, you need to explicitly set it as the default build ID if you want to.</span></span> <span data-ttu-id="2f9f2-405">När du använder rekommendationer om du inte anger build-ID som du vill använda, används standardvärdet automatiskt.</span><span class="sxs-lookup"><span data-stu-id="2f9f2-405">When you consume recommendations, if you do not specify the build ID that you want to use, the default one will be used automatically.</span></span>

<span data-ttu-id="2f9f2-406">Den här mekanismen gör att du - när du har en rekommendation modell i produktionen – för att skapa nya modeller och testa dem innan du höjer upp dem till produktionen.</span><span class="sxs-lookup"><span data-stu-id="2f9f2-406">This mechanism enables you - once you have a recommendation model in production - to build new models and test them before you promote them to production.</span></span>

| <span data-ttu-id="2f9f2-407">HTTP-metoden</span><span class="sxs-lookup"><span data-stu-id="2f9f2-407">HTTP Method</span></span> | <span data-ttu-id="2f9f2-408">URI: N</span><span class="sxs-lookup"><span data-stu-id="2f9f2-408">URI</span></span> |
|:--- |:--- |
| <span data-ttu-id="2f9f2-409">PLACERA</span><span class="sxs-lookup"><span data-stu-id="2f9f2-409">PUT</span></span> |`<rootURI>/UpdateModel?id=%27<modelId>%27&apiVersion=%271.0%27`<br><br><span data-ttu-id="2f9f2-410">Exempel:</span><span class="sxs-lookup"><span data-stu-id="2f9f2-410">Example:</span></span><br>`<rootURI>/UpdateModel?id=%279559872f-7a53-4076-a3c7-19d9385c1265%27&apiVersion=%271.0%27` |

| <span data-ttu-id="2f9f2-411">Parameternamn</span><span class="sxs-lookup"><span data-stu-id="2f9f2-411">Parameter Name</span></span> | <span data-ttu-id="2f9f2-412">Giltiga värden</span><span class="sxs-lookup"><span data-stu-id="2f9f2-412">Valid Values</span></span> |
|:--- |:--- |
| <span data-ttu-id="2f9f2-413">id</span><span class="sxs-lookup"><span data-stu-id="2f9f2-413">id</span></span> |<span data-ttu-id="2f9f2-414">Unik identifierare för modellen (skiftlägeskänsligt)</span><span class="sxs-lookup"><span data-stu-id="2f9f2-414">Unique identifier of the model (case-sensitive)</span></span> |
| <span data-ttu-id="2f9f2-415">apiVersion</span><span class="sxs-lookup"><span data-stu-id="2f9f2-415">apiVersion</span></span> |<span data-ttu-id="2f9f2-416">1.0</span><span class="sxs-lookup"><span data-stu-id="2f9f2-416">1.0</span></span> |
|  | |
| <span data-ttu-id="2f9f2-417">Begärandetext</span><span class="sxs-lookup"><span data-stu-id="2f9f2-417">Request Body</span></span> |`<ModelUpdateParams xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">`<br>`   <Description>New Description</Description>`<br>`          <ActiveBuildId>-1</ActiveBuildId>`<br>`</ModelUpdateParams>`<br><br><span data-ttu-id="2f9f2-418">Observera att XML-taggarna beskrivning och ActiveBuildId är valfria.</span><span class="sxs-lookup"><span data-stu-id="2f9f2-418">Note that the XML tags Description and ActiveBuildId are optional.</span></span> <span data-ttu-id="2f9f2-419">Om du inte vill ange beskrivning eller ActiveBuildId bort hela-taggen.</span><span class="sxs-lookup"><span data-stu-id="2f9f2-419">If you do not want to set Description or ActiveBuildId, remove the entire tag.</span></span> |

<span data-ttu-id="2f9f2-420">**Svaret**:</span><span class="sxs-lookup"><span data-stu-id="2f9f2-420">**Response**:</span></span>

<span data-ttu-id="2f9f2-421">HTTP-statuskod: 200</span><span class="sxs-lookup"><span data-stu-id="2f9f2-421">HTTP Status code: 200</span></span>

<span data-ttu-id="2f9f2-422">OData-XML</span><span class="sxs-lookup"><span data-stu-id="2f9f2-422">OData XML</span></span>

    <feed xmlns:base="https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/UpdateModel" xmlns:d="http://schemas.microsoft.com/ado/2007/08/dataservices" xmlns:m="http://schemas.microsoft.com/ado/2007/08/dataservices/metadata" xmlns="http://www.w3.org/2005/Atom">
      <title type="text" />
      <subtitle type="text">Update an Existing Model</subtitle>
      <id>https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/UpdateModel?id='9559872f-7a53-4076-a3c7-19d9385c1265'&amp;apiVersion='1.0'</id>
      <rights type="text" />
      <updated>2014-10-05T10:27:17Z</updated>
      <link rel="self" href="https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/UpdateModel?id='9559872f-7a53-4076-a3c7-19d9385c1265'&amp;apiVersion='1.0'" />
    </feed>

## <a name="legal"></a><span data-ttu-id="2f9f2-423">Juridisk information</span><span class="sxs-lookup"><span data-stu-id="2f9f2-423">Legal</span></span>
<span data-ttu-id="2f9f2-424">Detta dokument tillhandahålls ”som-är”.</span><span class="sxs-lookup"><span data-stu-id="2f9f2-424">This document is provided "as-is".</span></span> <span data-ttu-id="2f9f2-425">Information och åsikter som uttrycks i detta dokument, inklusive Webbadresser och andra webbplatsreferenser, kan ändras utan föregående meddelande.</span><span class="sxs-lookup"><span data-stu-id="2f9f2-425">Information and views expressed in this document, including URL and other Internet website references, may change without notice.</span></span> <span data-ttu-id="2f9f2-426">Vissa exempel som beskrivs häri tillhandahålls enbart som illustration och är fiktiva.</span><span class="sxs-lookup"><span data-stu-id="2f9f2-426">Some examples depicted herein are provided for illustration only and are fictitious.</span></span> <span data-ttu-id="2f9f2-427">Ingen verklig associering eller koppling är avsedd eller underförstådd.</span><span class="sxs-lookup"><span data-stu-id="2f9f2-427">No real association or connection is intended or should be inferred.</span></span> <span data-ttu-id="2f9f2-428">Det här dokumentet ger dig inga juridiska rättigheter till någon immateriell egendom i någon Microsoft-produkt.</span><span class="sxs-lookup"><span data-stu-id="2f9f2-428">This document does not provide you with any legal rights to any intellectual property in any Microsoft product.</span></span> <span data-ttu-id="2f9f2-429">Du får kopiera och använda det här dokumentet som intern referens.</span><span class="sxs-lookup"><span data-stu-id="2f9f2-429">You may copy and use this document for your internal, reference purposes.</span></span> <span data-ttu-id="2f9f2-430">© 2014 Microsoft.</span><span class="sxs-lookup"><span data-stu-id="2f9f2-430">© 2014 Microsoft.</span></span> <span data-ttu-id="2f9f2-431">Med ensamrätt.</span><span class="sxs-lookup"><span data-stu-id="2f9f2-431">All rights reserved.</span></span> 

