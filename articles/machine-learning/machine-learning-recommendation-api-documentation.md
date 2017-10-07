---
title: aaaMachine Learning rekommendationer API-dokumentationen | Microsoft Docs
description: "Azure Machine Learning rekommendationer API-dokumentationen för en rekommendationer motor som är tillgängliga i hello Microsoft Azure Marketplace."
services: machine-learning
documentationcenter: 
author: LuisCabrer
manager: jhubbard
editor: cgronlun
ms.assetid: 32c3ab2f-fdd7-48cc-b501-ad55c79b87dc
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/31/2017
ms.author: LuisCa
ROBOTS: NOINDEX
redirect_url: machine-learning-datamarket-deprecation
redirect_document_id: True
ms.openlocfilehash: d1cec228bf23870c05c8ab8df2779b0c3c65b06d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-machine-learning-recommendations-api-documentation"></a><span data-ttu-id="53f0a-103">Azure Machine Learning-rekommendationer – API-dokumentation</span><span class="sxs-lookup"><span data-stu-id="53f0a-103">Azure Machine Learning Recommendations API Documentation</span></span>
> [!NOTE]
> <span data-ttu-id="53f0a-104">Du bör börja använda hello kognitiva rekommendationer API-tjänsten i stället för den här versionen.</span><span class="sxs-lookup"><span data-stu-id="53f0a-104">You should start using hello Recommendations API Cognitive Service instead of this version.</span></span> <span data-ttu-id="53f0a-105">hello kognitiva Rekommendationstjänsten ersätter den här tjänsten och alla nya funktioner för hello det kommer fram.</span><span class="sxs-lookup"><span data-stu-id="53f0a-105">hello Recommendations Cognitive Service will be replacing this service, and all hello new features will be developed there.</span></span> <span data-ttu-id="53f0a-106">Den har nya funktioner som batchbearbetning support, bättre API Explorer, en tydligare API underlag, mer konsekvent signup/fakturerings experience och så vidare.</span><span class="sxs-lookup"><span data-stu-id="53f0a-106">It has new capabilities like batching support, a better API Explorer, a cleaner API surface, more consistent signup/billing experience, etc.</span></span>
> <span data-ttu-id="53f0a-107">Lär dig mer om [migrera toohello ny kognitiva tjänst](http://aka.ms/recomigrate)</span><span class="sxs-lookup"><span data-stu-id="53f0a-107">Learn more about [Migrating toohello new Cognitive Service](http://aka.ms/recomigrate)</span></span>
> 
> 

<span data-ttu-id="53f0a-108">Det här dokumentet visar Microsoft Azure Machine Learning rekommendationer API.</span><span class="sxs-lookup"><span data-stu-id="53f0a-108">This document depicts Microsoft Azure Machine Learning Recommendations APIs.</span></span>

[!INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

## <a name="1-general-overview"></a><span data-ttu-id="53f0a-109">1. Allmän översikt</span><span class="sxs-lookup"><span data-stu-id="53f0a-109">1. General overview</span></span>
<span data-ttu-id="53f0a-110">Det här dokumentet är en API-referens.</span><span class="sxs-lookup"><span data-stu-id="53f0a-110">This document is an API reference.</span></span> <span data-ttu-id="53f0a-111">Du bör börja med ”Azure Machine Learning rekommendation--Snabbstart” hello-dokument.</span><span class="sxs-lookup"><span data-stu-id="53f0a-111">You should start with hello “Azure Machine Learning Recommendation - Quick Start” document.</span></span>

<span data-ttu-id="53f0a-112">hello Azure Machine Learning rekommendationer API kan delas in i hello följande logiska grupper:</span><span class="sxs-lookup"><span data-stu-id="53f0a-112">hello Azure Machine Learning Recommendations API can be divided into hello following logical groups:</span></span>

* <span data-ttu-id="53f0a-113"><ins>Begränsningar</ins> -rekommendationer API-begränsningar.</span><span class="sxs-lookup"><span data-stu-id="53f0a-113"><ins>Limitations</ins> - Recommendations API limitations.</span></span>
* <span data-ttu-id="53f0a-114"><ins>Allmän Information</ins> -Information om autentisering, service URI och versionshantering.</span><span class="sxs-lookup"><span data-stu-id="53f0a-114"><ins>General Information</ins> - Information on authentication, service URI and versioning.</span></span>
* <span data-ttu-id="53f0a-115"><ins>Modellen Basic</ins> -API: er som gör att du toodo hello grundläggande åtgärder på modellen (t.ex. Skapa, uppdatera och ta bort en modell).</span><span class="sxs-lookup"><span data-stu-id="53f0a-115"><ins>Model Basic</ins> - APIs that enable you toodo hello basic operations on model (e.g. create, update and delete a model).</span></span>
* <span data-ttu-id="53f0a-116"><ins>Modellen Avancerat</ins> -API: er som gör att du tooget avancerade datainsikter om hello modellen.</span><span class="sxs-lookup"><span data-stu-id="53f0a-116"><ins>Model Advanced</ins> - APIs that enable you tooget advanced data insights on hello model.</span></span>
* <span data-ttu-id="53f0a-117"><ins>Modellen affärsregler</ins> -API: er som gör att du toomanage affärsregler på hello modellen rekommendation resultaten.</span><span class="sxs-lookup"><span data-stu-id="53f0a-117"><ins>Model Business Rules</ins> - APIs that enable you toomanage business rules on hello model recommendation results.</span></span>
* <span data-ttu-id="53f0a-118"><ins>Katalogen</ins> -API: er som gör att du toodo grundläggande åtgärder på en modell-katalog.</span><span class="sxs-lookup"><span data-stu-id="53f0a-118"><ins>Catalog</ins> - APIs that enable you toodo basic operations on a model catalog.</span></span> <span data-ttu-id="53f0a-119">En katalog innehåller information om metadata på hello objekt hello användningsdata.</span><span class="sxs-lookup"><span data-stu-id="53f0a-119">A catalog contains metadata information on hello items of hello usage data.</span></span>
* <span data-ttu-id="53f0a-120"><ins>Funktionen</ins> -API: er som aktiverar tooget insikter om objektet till hello katalog och hur toouse denna information toobuild bättre rekommendationer.</span><span class="sxs-lookup"><span data-stu-id="53f0a-120"><ins>Feature</ins> - APIs that enable tooget insights on item into hello catalog and how toouse this information toobuild better recommendations.</span></span>
* <span data-ttu-id="53f0a-121"><ins>Användningsdata</ins> -API: er som gör att du toodo grundläggande åtgärder på hello modellen användningsdata.</span><span class="sxs-lookup"><span data-stu-id="53f0a-121"><ins>Usage Data</ins> - APIs that enable you toodo basic operations on hello model usage data.</span></span> <span data-ttu-id="53f0a-122">Användningsdata i hello grundformat består av rader som innehåller par &#60; userId &#62; &#60; itemId &#62;.</span><span class="sxs-lookup"><span data-stu-id="53f0a-122">Usage data in hello basic form consists of rows that include pairs of &#60;userId&#62;,&#60;itemId&#62;.</span></span>
* <span data-ttu-id="53f0a-123"><ins>Skapa</ins> - API: er som aktiverar tootrigger en modell bygga och skapa grundläggande åtgärder som är relaterade toothis.</span><span class="sxs-lookup"><span data-stu-id="53f0a-123"><ins>Build</ins> - APIs that enable you tootrigger a model build and do basic operations that are related toothis build.</span></span> <span data-ttu-id="53f0a-124">Du kan utlösa en version av modellen när du har värdefulla användningsdata.</span><span class="sxs-lookup"><span data-stu-id="53f0a-124">You can trigger a model build once you have valuable usage data.</span></span>
* <span data-ttu-id="53f0a-125"><ins>Rekommendation</ins> -API: er som gör att du tooconsume rekommendationer när du slutar hello version av en modell.</span><span class="sxs-lookup"><span data-stu-id="53f0a-125"><ins>Recommendation</ins> - APIs that enable you tooconsume recommendations once hello build of a model ends.</span></span>
* <span data-ttu-id="53f0a-126"><ins>Användardata</ins> -API: er som gör att du toofetch information om användning av hello användardata.</span><span class="sxs-lookup"><span data-stu-id="53f0a-126"><ins>User Data</ins> - APIs that enable you toofetch information on hello user usage data.</span></span>
* <span data-ttu-id="53f0a-127"><ins>Meddelanden</ins> -API: er som gör att du tooreceive meddelanden om problem relaterade tooyour API-åtgärder.</span><span class="sxs-lookup"><span data-stu-id="53f0a-127"><ins>Notifications</ins> - APIs that enable you tooreceive notifications on problems related tooyour API operations.</span></span> <span data-ttu-id="53f0a-128">(Till exempel du rapporterar användningsdata via för datainsamling och de flesta av hello händelser bearbetning av inte importeras.</span><span class="sxs-lookup"><span data-stu-id="53f0a-128">(For example, you are reporting usage data via Data Acquisition and most of hello events processing are failing.</span></span> <span data-ttu-id="53f0a-129">Ett felmeddelande aktiveras.)</span><span class="sxs-lookup"><span data-stu-id="53f0a-129">An error notification will be raised.)</span></span>

## <a name="2-limitations"></a><span data-ttu-id="53f0a-130">2. Begränsningar</span><span class="sxs-lookup"><span data-stu-id="53f0a-130">2. Limitations</span></span>
* <span data-ttu-id="53f0a-131">hello maximala antalet modeller per prenumeration är 10.</span><span class="sxs-lookup"><span data-stu-id="53f0a-131">hello maximum number of models per subscription is 10.</span></span>
* <span data-ttu-id="53f0a-132">hello maximalt antal versioner per modell är 20.</span><span class="sxs-lookup"><span data-stu-id="53f0a-132">hello maximum number of builds per model is 20.</span></span>
* <span data-ttu-id="53f0a-133">hello maximalt antal objekt som kan innehålla en katalog är 100 000.</span><span class="sxs-lookup"><span data-stu-id="53f0a-133">hello maximum number of items that a catalog can hold is 100,000.</span></span>
* <span data-ttu-id="53f0a-134">hello maxantalet återställningspunkter för användning som hålls är ~ 5,000,000.</span><span class="sxs-lookup"><span data-stu-id="53f0a-134">hello maximum number of usage points that are kept is ~5,000,000.</span></span> <span data-ttu-id="53f0a-135">hello äldsta tas bort om nya överföras eller rapporteras.</span><span class="sxs-lookup"><span data-stu-id="53f0a-135">hello oldest will be deleted if new ones will be uploaded or reported.</span></span>
* <span data-ttu-id="53f0a-136">hello maximala storleken på data som kan skickas i POST (t.ex. Importera katalogdata import användningsdata) är 200MB.</span><span class="sxs-lookup"><span data-stu-id="53f0a-136">hello maximum size of data that can be sent in POST (e.g. import catalog data, import usage data) is 200MB.</span></span>
* <span data-ttu-id="53f0a-137">hello maximalt antal objekt som kan bli tillfrågad om när du hämtar rekommendationer är 150.</span><span class="sxs-lookup"><span data-stu-id="53f0a-137">hello maximum number of items that can be asked for when getting recommendations is 150.</span></span>

## <a name="3-apis---general-information"></a><span data-ttu-id="53f0a-138">3. API - allmän information</span><span class="sxs-lookup"><span data-stu-id="53f0a-138">3. APIs - general information</span></span>
### <a name="31-authentication"></a><span data-ttu-id="53f0a-139">3.1.</span><span class="sxs-lookup"><span data-stu-id="53f0a-139">3.1.</span></span> <span data-ttu-id="53f0a-140">Autentisering</span><span class="sxs-lookup"><span data-stu-id="53f0a-140">Authentication</span></span>
<span data-ttu-id="53f0a-141">Följ hello Microsoft Azure Marketplace riktlinjer om autentisering.</span><span class="sxs-lookup"><span data-stu-id="53f0a-141">Please follow hello Microsoft Azure Marketplace guidelines regarding authentication.</span></span> <span data-ttu-id="53f0a-142">hello marketplace stöder antingen hello grundläggande eller OAuth autentiseringsmetoden.</span><span class="sxs-lookup"><span data-stu-id="53f0a-142">hello marketplace supports either hello Basic or OAuth authentication method.</span></span>

### <a name="32-service-uri"></a><span data-ttu-id="53f0a-143">3.2.</span><span class="sxs-lookup"><span data-stu-id="53f0a-143">3.2.</span></span> <span data-ttu-id="53f0a-144">URI för tjänsten</span><span class="sxs-lookup"><span data-stu-id="53f0a-144">Service URI</span></span>
<span data-ttu-id="53f0a-145">hello service rot URI för hello Azure Machine Learning rekommendationer API: er är [här.](https://api.datamarket.azure.com/amla/recommendations/v3/)</span><span class="sxs-lookup"><span data-stu-id="53f0a-145">hello service root URI for hello Azure Machine Learning Recommendations APIs is [here.](https://api.datamarket.azure.com/amla/recommendations/v3/)</span></span>

<span data-ttu-id="53f0a-146">fullständig hello-tjänstens URI uttrycks med hjälp av element i hello OData-specifikationen.</span><span class="sxs-lookup"><span data-stu-id="53f0a-146">hello full service URI is expressed using elements of hello OData specification.</span></span>  

### <a name="33-api-version"></a><span data-ttu-id="53f0a-147">3.3.</span><span class="sxs-lookup"><span data-stu-id="53f0a-147">3.3.</span></span> <span data-ttu-id="53f0a-148">API-version</span><span class="sxs-lookup"><span data-stu-id="53f0a-148">API version</span></span>
<span data-ttu-id="53f0a-149">Varje API-anrop har ska i slutet av hello frågeparameter kallas apiVersion som ska anges too1.0.</span><span class="sxs-lookup"><span data-stu-id="53f0a-149">Each API call will have, at hello end, a query parameter called apiVersion that should be set too1.0.</span></span>

### <a name="34-ids-are-case-sensitive"></a><span data-ttu-id="53f0a-150">3.4.</span><span class="sxs-lookup"><span data-stu-id="53f0a-150">3.4.</span></span> <span data-ttu-id="53f0a-151">ID: N är skiftlägeskänsliga</span><span class="sxs-lookup"><span data-stu-id="53f0a-151">IDs are case sensitive</span></span>
<span data-ttu-id="53f0a-152">ID: N som returneras av någon av hello API: er, är skiftlägeskänsliga och ska användas som sådana när de skickas som parametrar i efterföljande API-anrop.</span><span class="sxs-lookup"><span data-stu-id="53f0a-152">IDs, returned by any of hello APIs, are case sensitive and should be used as such when passed as parameters in subsequent API calls.</span></span> <span data-ttu-id="53f0a-153">Till exempel är modell-ID: N och katalog-ID: N skiftlägeskänsliga.</span><span class="sxs-lookup"><span data-stu-id="53f0a-153">For instance, model IDs and catalog IDs are case sensitive.</span></span>

## <a name="4-recommendations-quality-and-cold-items"></a><span data-ttu-id="53f0a-154">4. Rekommendationer kvalitet och kalla objekt</span><span class="sxs-lookup"><span data-stu-id="53f0a-154">4. Recommendations Quality and Cold Items</span></span>
### <a name="41-recommendation-quality"></a><span data-ttu-id="53f0a-155">4.1.</span><span class="sxs-lookup"><span data-stu-id="53f0a-155">4.1.</span></span> <span data-ttu-id="53f0a-156">Rekommendation kvalitet</span><span class="sxs-lookup"><span data-stu-id="53f0a-156">Recommendation quality</span></span>
<span data-ttu-id="53f0a-157">Skapa en rekommendation modell är vanligtvis tillräckligt med tooallow hello system tooprovide rekommendationer.</span><span class="sxs-lookup"><span data-stu-id="53f0a-157">Creating a recommendation model is usually enough tooallow hello system tooprovide recommendations.</span></span> <span data-ttu-id="53f0a-158">Dock rekommendation kvalitet varierar beroende på hello användning bearbetas och hello täckning av hello-katalogen.</span><span class="sxs-lookup"><span data-stu-id="53f0a-158">Nevertheless, recommendation quality varies based on hello usage processed and hello coverage of hello catalog.</span></span> <span data-ttu-id="53f0a-159">Till exempel om du har många cold artiklar (artiklar utan betydande syntax) hello systemet har problem med att tillhandahålla en rekommendation för ett sådant objekt eller med ett sådant objekt som en rekommenderad.</span><span class="sxs-lookup"><span data-stu-id="53f0a-159">For example if you have a lot of cold items (items without significant usage), hello system will have difficulties providing a recommendation for such an item or using such an item as a recommended one.</span></span> <span data-ttu-id="53f0a-160">I ordning tooovercome hello cold objektet problem kan hello system hello användning av metadata för hello objekt tooenhance hello rekommendationer.</span><span class="sxs-lookup"><span data-stu-id="53f0a-160">In order tooovercome hello cold item problem, hello system allows hello use of metadata of hello items tooenhance hello recommendations.</span></span> <span data-ttu-id="53f0a-161">Dessa metadata är refererad tooas funktioner.</span><span class="sxs-lookup"><span data-stu-id="53f0a-161">This metadata is referred tooas features.</span></span> <span data-ttu-id="53f0a-162">Vanliga funktioner är en bok författaren eller en film aktören.</span><span class="sxs-lookup"><span data-stu-id="53f0a-162">Typical features are a book's author or a movie's actor.</span></span> <span data-ttu-id="53f0a-163">Funktioner som tillhandahålls via hello katalog i hello form av nyckel/värde-strängar.</span><span class="sxs-lookup"><span data-stu-id="53f0a-163">Features are provided via hello catalog in hello form of key/value strings.</span></span> <span data-ttu-id="53f0a-164">Hello fullständig formatering av hello katalogfil finns toohello [importera katalogen avsnittet](#81-import-catalog-data).</span><span class="sxs-lookup"><span data-stu-id="53f0a-164">For hello full format of hello catalog file, please refer toohello [import catalog section](#81-import-catalog-data).</span></span> 

### <a name="42-rank-build"></a><span data-ttu-id="53f0a-165">4.2.</span><span class="sxs-lookup"><span data-stu-id="53f0a-165">4.2.</span></span> <span data-ttu-id="53f0a-166">Rank build</span><span class="sxs-lookup"><span data-stu-id="53f0a-166">Rank build</span></span>
<span data-ttu-id="53f0a-167">Funktioner kan förbättra hello rekommendation modell, men toodo kräver hello med meningsfull funktioner.</span><span class="sxs-lookup"><span data-stu-id="53f0a-167">Features can enhance hello recommendation model, but toodo so requires hello use of meaningful features.</span></span> <span data-ttu-id="53f0a-168">För detta ändamål som en ny version introducerades - skapa en rang.</span><span class="sxs-lookup"><span data-stu-id="53f0a-168">For this purpose a new build was introduced - a rank build.</span></span> <span data-ttu-id="53f0a-169">Den här versionen ska rangordnas hello utnyttjar funktioner.</span><span class="sxs-lookup"><span data-stu-id="53f0a-169">This build will rank hello usefulness of features.</span></span> <span data-ttu-id="53f0a-170">En användbar funktion är en funktion med rangordningen 2 och uppåt.</span><span class="sxs-lookup"><span data-stu-id="53f0a-170">A meaningful feature is a feature with a rank score of 2 and up.</span></span>
<span data-ttu-id="53f0a-171">Efter att förstå vilka hello funktioner som är meningsfullt, Utlös en rekommendation version med hello lista (eller underordnad lista) med meningsfull funktioner.</span><span class="sxs-lookup"><span data-stu-id="53f0a-171">After understanding which of hello features are meaningful, trigger a recommendation build with hello list (or sublist) of meaningful features.</span></span> <span data-ttu-id="53f0a-172">Det är möjligt toouse dessa funktioner för hello förbättring av både varm objekt och kalla.</span><span class="sxs-lookup"><span data-stu-id="53f0a-172">It is possible toouse these feature for hello enhancement of both warm items and cold items.</span></span> <span data-ttu-id="53f0a-173">I ordning toouse dem för varmt objekt hello `UseFeatureInModel` build-parametern ska ställas in.</span><span class="sxs-lookup"><span data-stu-id="53f0a-173">In order toouse them for warm items, hello `UseFeatureInModel` build parameter should be set up.</span></span> <span data-ttu-id="53f0a-174">I ordning toouse funktioner för cold artiklar hello `AllowColdItemPlacement` build-parametern måste vara aktiverad.</span><span class="sxs-lookup"><span data-stu-id="53f0a-174">In order toouse features for cold items, hello `AllowColdItemPlacement` build parameter should be enabled.</span></span>
<span data-ttu-id="53f0a-175">Obs: Det är inte möjligt tooenable `AllowColdItemPlacement` utan att aktivera `UseFeatureInModel`.</span><span class="sxs-lookup"><span data-stu-id="53f0a-175">Note: It is not possible tooenable `AllowColdItemPlacement` without enabling `UseFeatureInModel`.</span></span>

### <a name="43-recommendation-reasoning"></a><span data-ttu-id="53f0a-176">4.3.</span><span class="sxs-lookup"><span data-stu-id="53f0a-176">4.3.</span></span> <span data-ttu-id="53f0a-177">Rekommendation motivationen</span><span class="sxs-lookup"><span data-stu-id="53f0a-177">Recommendation reasoning</span></span>
<span data-ttu-id="53f0a-178">Rekommendation motivationen är en annan del av användning av funktioner.</span><span class="sxs-lookup"><span data-stu-id="53f0a-178">Recommendation reasoning is another aspect of feature usage.</span></span> <span data-ttu-id="53f0a-179">Faktiskt kan hello Azure Machine Learning rekommendationer motorn använda funktioner tooprovide rekommendation förklaringar (kallas även</span><span class="sxs-lookup"><span data-stu-id="53f0a-179">Indeed, hello Azure Machine Learning Recommendations engine can use features tooprovide recommendation explanations (a.k.a.</span></span> <span data-ttu-id="53f0a-180">skäl till), rekommenderas inledande toomore förtroende i hello objekt från hello rekommendation konsumenten.</span><span class="sxs-lookup"><span data-stu-id="53f0a-180">reasoning), leading toomore confidence in hello recommended item from hello recommendation consumer.</span></span>
<span data-ttu-id="53f0a-181">tooenable skäl, hello `AllowFeatureCorrelation` och `ReasoningFeatureList` parametrarna vara installationsprogrammet tidigare toorequesting en rekommendation version.</span><span class="sxs-lookup"><span data-stu-id="53f0a-181">tooenable reasoning, hello `AllowFeatureCorrelation` and `ReasoningFeatureList` parameters should be setup prior toorequesting a recommendation build.</span></span>

## <a name="5-model-basic"></a><span data-ttu-id="53f0a-182">5. Modellen Basic</span><span class="sxs-lookup"><span data-stu-id="53f0a-182">5. Model Basic</span></span>
### <a name="51-create-model"></a><span data-ttu-id="53f0a-183">5.1.</span><span class="sxs-lookup"><span data-stu-id="53f0a-183">5.1.</span></span> <span data-ttu-id="53f0a-184">Skapa modell</span><span class="sxs-lookup"><span data-stu-id="53f0a-184">Create Model</span></span>
<span data-ttu-id="53f0a-185">Skapar en ”skapa modellen” begäran.</span><span class="sxs-lookup"><span data-stu-id="53f0a-185">Creates a “create model” request.</span></span>

| <span data-ttu-id="53f0a-186">HTTP-metoden</span><span class="sxs-lookup"><span data-stu-id="53f0a-186">HTTP Method</span></span> | <span data-ttu-id="53f0a-187">URI: N</span><span class="sxs-lookup"><span data-stu-id="53f0a-187">URI</span></span> |
|:--- |:--- |
| <span data-ttu-id="53f0a-188">POST</span><span class="sxs-lookup"><span data-stu-id="53f0a-188">POST</span></span> |`<rootURI>/CreateModel?modelName=%27<model_name>%27&apiVersion=%271.0%27`<br><br><span data-ttu-id="53f0a-189">Exempel:</span><span class="sxs-lookup"><span data-stu-id="53f0a-189">Example:</span></span><br>`<rootURI>/CreateModel?modelName=%27MyFirstModel%27&apiVersion=%271.0%27` |

| <span data-ttu-id="53f0a-190">Parameternamn</span><span class="sxs-lookup"><span data-stu-id="53f0a-190">Parameter Name</span></span> | <span data-ttu-id="53f0a-191">Giltiga värden</span><span class="sxs-lookup"><span data-stu-id="53f0a-191">Valid Values</span></span> |
|:--- |:--- |
| <span data-ttu-id="53f0a-192">%{ModelName/</span><span class="sxs-lookup"><span data-stu-id="53f0a-192">modelName</span></span> |<span data-ttu-id="53f0a-193">Endast bokstäver (A-Z, a-z), siffror (0-9), bindestreck (-) och understreck (_) är tillåtna.</span><span class="sxs-lookup"><span data-stu-id="53f0a-193">Only letters (A-Z, a-z), numbers (0-9), hyphens (-) and underscore (_) are allowed.</span></span><br><span data-ttu-id="53f0a-194">Maxlängd: 20</span><span class="sxs-lookup"><span data-stu-id="53f0a-194">Max length: 20</span></span> |
| <span data-ttu-id="53f0a-195">apiVersion</span><span class="sxs-lookup"><span data-stu-id="53f0a-195">apiVersion</span></span> |<span data-ttu-id="53f0a-196">1.0</span><span class="sxs-lookup"><span data-stu-id="53f0a-196">1.0</span></span> |
|  | |
| <span data-ttu-id="53f0a-197">Begärandetext</span><span class="sxs-lookup"><span data-stu-id="53f0a-197">Request Body</span></span> |<span data-ttu-id="53f0a-198">INGEN</span><span class="sxs-lookup"><span data-stu-id="53f0a-198">NONE</span></span> |

<span data-ttu-id="53f0a-199">**Svaret**:</span><span class="sxs-lookup"><span data-stu-id="53f0a-199">**Response**:</span></span>

<span data-ttu-id="53f0a-200">HTTP-statuskod: 200</span><span class="sxs-lookup"><span data-stu-id="53f0a-200">HTTP Status code: 200</span></span>

* <span data-ttu-id="53f0a-201">`feed/entry/content/properties/id`-Innehåller hello modell-ID.</span><span class="sxs-lookup"><span data-stu-id="53f0a-201">`feed/entry/content/properties/id` - Contains hello model ID.</span></span>
  <span data-ttu-id="53f0a-202">**Obs**: modell-ID är skiftlägeskänsliga.</span><span class="sxs-lookup"><span data-stu-id="53f0a-202">**Note**: model ID is case sensitive.</span></span>

<span data-ttu-id="53f0a-203">OData-XML</span><span class="sxs-lookup"><span data-stu-id="53f0a-203">OData XML</span></span>

    <feed xmlns:base="https://api.datamarket.azure.com/amla/recommendations/v3/CreateModel" xmlns:d="http://schemas.microsoft.com/ado/2007/08/dataservices" xmlns:m="http://schemas.microsoft.com/ado/2007/08/dataservices/metadata" xmlns="http://www.w3.org/2005/Atom">
      <title type="text" />
      <subtitle type="text">Create A New Model</subtitle>
      <id>https://api.datamarket.azure.com/amla/recommendations/v3/CreateModel?modelName='MyFirstModel'&amp;apiVersion='1.0'</id>
      <rights type="text" />
      <updated>2014-10-05T06:35:21Z</updated>
      <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/CreateModel?modelName='MyFirstModel'&amp;apiVersion='1.0'" />
      <entry>
    <id>https://api.datamarket.azure.com/amla/recommendations/v3/CreateModel?modelName='MyFirstModel'&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1</id>
    <title type="text">CreateANewModelEntity2</title>
    <updated>2014-10-05T06:35:21Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/CreateModel?modelName='MyFirstModel'&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1" />
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

### <a name="52-get-model"></a><span data-ttu-id="53f0a-204">5.2.</span><span class="sxs-lookup"><span data-stu-id="53f0a-204">5.2.</span></span> <span data-ttu-id="53f0a-205">Hämta modellen</span><span class="sxs-lookup"><span data-stu-id="53f0a-205">Get Model</span></span>
<span data-ttu-id="53f0a-206">Skapar en ”get-”-modellbegäran.</span><span class="sxs-lookup"><span data-stu-id="53f0a-206">Creates a “get model” request.</span></span>

| <span data-ttu-id="53f0a-207">HTTP-metoden</span><span class="sxs-lookup"><span data-stu-id="53f0a-207">HTTP Method</span></span> | <span data-ttu-id="53f0a-208">URI: N</span><span class="sxs-lookup"><span data-stu-id="53f0a-208">URI</span></span> |
|:--- |:--- |
| <span data-ttu-id="53f0a-209">HÄMTA</span><span class="sxs-lookup"><span data-stu-id="53f0a-209">GET</span></span> |`<rootURI>/GetModel?id=%27<model_id>%27&apiVersion=%271.0%27`<br><span data-ttu-id="53f0a-210">Exempel:</span><span class="sxs-lookup"><span data-stu-id="53f0a-210">Example:</span></span><br>`<rootURI>/GetModel?id=%271cac7b76-def4-41f1-bc81-29b806adb1de%27&apiVersion=%271.0%27` |

| <span data-ttu-id="53f0a-211">Parameternamn</span><span class="sxs-lookup"><span data-stu-id="53f0a-211">Parameter Name</span></span> | <span data-ttu-id="53f0a-212">Giltiga värden</span><span class="sxs-lookup"><span data-stu-id="53f0a-212">Valid Values</span></span> |
|:--- |:--- |
| <span data-ttu-id="53f0a-213">id</span><span class="sxs-lookup"><span data-stu-id="53f0a-213">id</span></span> |<span data-ttu-id="53f0a-214">Unik identifierare för hello modellen (skiftlägeskänslig)</span><span class="sxs-lookup"><span data-stu-id="53f0a-214">Unique identifier of hello model (case sensitive)</span></span> |
| <span data-ttu-id="53f0a-215">apiVersion</span><span class="sxs-lookup"><span data-stu-id="53f0a-215">apiVersion</span></span> |<span data-ttu-id="53f0a-216">1.0</span><span class="sxs-lookup"><span data-stu-id="53f0a-216">1.0</span></span> |
|  | |
| <span data-ttu-id="53f0a-217">Begärandetext</span><span class="sxs-lookup"><span data-stu-id="53f0a-217">Request Body</span></span> |<span data-ttu-id="53f0a-218">INGEN</span><span class="sxs-lookup"><span data-stu-id="53f0a-218">NONE</span></span> |

<span data-ttu-id="53f0a-219">**Svaret**:</span><span class="sxs-lookup"><span data-stu-id="53f0a-219">**Response**:</span></span>

<span data-ttu-id="53f0a-220">HTTP-statuskod: 200</span><span class="sxs-lookup"><span data-stu-id="53f0a-220">HTTP Status code: 200</span></span>

<span data-ttu-id="53f0a-221">hello modelldata kan hittas under hello följande element:</span><span class="sxs-lookup"><span data-stu-id="53f0a-221">hello model data can be found under hello following elements:</span></span>

* <span data-ttu-id="53f0a-222">`feed/entry/content/properties/Id`-Modell unikt ID.</span><span class="sxs-lookup"><span data-stu-id="53f0a-222">`feed/entry/content/properties/Id` - Model unique ID.</span></span>
* <span data-ttu-id="53f0a-223">`feed/entry/content/properties/Name`-Namn.</span><span class="sxs-lookup"><span data-stu-id="53f0a-223">`feed/entry/content/properties/Name` - Model name.</span></span>
* <span data-ttu-id="53f0a-224">`feed/entry/content/properties/Date`-Skapandedatum för modell.</span><span class="sxs-lookup"><span data-stu-id="53f0a-224">`feed/entry/content/properties/Date` - Model creation date.</span></span>
* <span data-ttu-id="53f0a-225">`feed/entry/content/properties/Status`-Status för modellen.</span><span class="sxs-lookup"><span data-stu-id="53f0a-225">`feed/entry/content/properties/Status` - Model status.</span></span> <span data-ttu-id="53f0a-226">En av hello följande:</span><span class="sxs-lookup"><span data-stu-id="53f0a-226">One of hello following:</span></span>
  * <span data-ttu-id="53f0a-227">-Modellen har skapats och innehåller inte katalog- och användningsdata.</span><span class="sxs-lookup"><span data-stu-id="53f0a-227">Created - Model is created and does not contain Catalog and Usage.</span></span>
  * <span data-ttu-id="53f0a-228">ReadyForBuild - modellen skapas och innehåller katalogen och användning.</span><span class="sxs-lookup"><span data-stu-id="53f0a-228">ReadyForBuild - Model is created and contains Catalog and Usage.</span></span>
* <span data-ttu-id="53f0a-229">`feed/entry/content/properties/HasActiveBuild`-Anger om hello modellen har har skapats.</span><span class="sxs-lookup"><span data-stu-id="53f0a-229">`feed/entry/content/properties/HasActiveBuild` - Indicates if hello model was built successfully.</span></span>
* <span data-ttu-id="53f0a-230">`feed/entry/content/properties/BuildId`-Modell active build-ID.</span><span class="sxs-lookup"><span data-stu-id="53f0a-230">`feed/entry/content/properties/BuildId` - Model active build ID.</span></span>
* <span data-ttu-id="53f0a-231">`feed/entry/content/properties/Mpr`-Modellen medelvärde percentil rangordning (MPR - mer information finns i ModelInsight).</span><span class="sxs-lookup"><span data-stu-id="53f0a-231">`feed/entry/content/properties/Mpr` - Model mean percentile ranking (MPR - see ModelInsight for more information).</span></span>
* <span data-ttu-id="53f0a-232">`feed/entry/content/properties/UserName`-Modell interna användarnamn.</span><span class="sxs-lookup"><span data-stu-id="53f0a-232">`feed/entry/content/properties/UserName` - Model internal user name.</span></span>

<span data-ttu-id="53f0a-233">OData-XML</span><span class="sxs-lookup"><span data-stu-id="53f0a-233">OData XML</span></span>

    <feed xmlns:base="https://api.datamarket.azure.com/amla/recommendations/v3/GetAllModels" xmlns:d="http://schemas.microsoft.com/ado/2007/08/dataservices" xmlns:m="http://schemas.microsoft.com/ado/2007/08/dataservices/metadata" xmlns="http://www.w3.org/2005/Atom">
      <title type="text" />
      <subtitle type="text">Get A List Of All Models</subtitle>
      <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetAllModels?apiVersion='1.0'</id>
      <rights type="text" />
      <updated>2014-10-28T14:35:51Z</updated>
      <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetAllModels?apiVersion='1.0'" />
      <entry>
    <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetAllModels?apiVersion='1.0'&amp;$skip=0&amp;$top=1</id>
    <title type="text">GetAListOfAllModelsEntity</title>
    <updated>2014-10-28T14:35:51Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetAllModels?apiVersion='1.0'&amp;$skip=0&amp;$top=1" />
    <content type="application/xml">
      <m:properties>
        <d:Id m:type="Edm.String">68b232f2-1037-45f7-8f49-af822695ee63</d:Id>
        <d:Name m:type="Edm.String">vah-11111</d:Name>
        <d:Date m:type="Edm.String">10/28/2014 2:16:07 PM</d:Date>
        <d:Status m:type="Edm.String">ReadyForBuild</d:Status>
        <d:HasActiveBuild m:type="Edm.String">false</d:HasActiveBuild>
        <d:BuildId m:type="Edm.String">-1</d:BuildId>
        <d:Mpr m:type="Edm.String">0</d:Mpr>
        <d:UsageFileNames m:type="Edm.String">ImplicitMatrix10_Guid_small.txt, ImplicitMatrix11_Guid_small.txt</d:UsageFileNames>
        <d:CatalogId m:type="Edm.String">626babdb-cab6-43a6-82ef-4fb86345700c</d:CatalogId>
        <d:UserName m:type="Edm.String">89dc4722-03ba-4f90-8821-b1db388408b5@dm.com</d:UserName>
        <d:Description m:type="Edm.String">short description</d:Description>
        <d:CatalogFileName m:type="Edm.String">catalog10_small.txt</d:CatalogFileName>
      </m:properties>
    </content>
      </entry>
    </feed>

### <a name="53----get-all-models"></a><span data-ttu-id="53f0a-234">5.3.</span><span class="sxs-lookup"><span data-stu-id="53f0a-234">5.3.</span></span>    <span data-ttu-id="53f0a-235">Hämta alla modeller</span><span class="sxs-lookup"><span data-stu-id="53f0a-235">Get All Models</span></span>
<span data-ttu-id="53f0a-236">Hämtar alla modeller för hello aktuella användaren.</span><span class="sxs-lookup"><span data-stu-id="53f0a-236">Retrieves all models of hello current user.</span></span>

| <span data-ttu-id="53f0a-237">HTTP-metoden</span><span class="sxs-lookup"><span data-stu-id="53f0a-237">HTTP Method</span></span> | <span data-ttu-id="53f0a-238">URI: N</span><span class="sxs-lookup"><span data-stu-id="53f0a-238">URI</span></span> |
|:--- |:--- |
| <span data-ttu-id="53f0a-239">HÄMTA</span><span class="sxs-lookup"><span data-stu-id="53f0a-239">GET</span></span> |`<rootURI>/GetAllModels?apiVersion=%271.0%27`<br><span data-ttu-id="53f0a-240">Exempel:</span><span class="sxs-lookup"><span data-stu-id="53f0a-240">Example:</span></span><br>`<rootURI>/GetAllModels?apiVersion=%271.0%27` |

| <span data-ttu-id="53f0a-241">Parameternamn</span><span class="sxs-lookup"><span data-stu-id="53f0a-241">Parameter Name</span></span> | <span data-ttu-id="53f0a-242">Giltiga värden</span><span class="sxs-lookup"><span data-stu-id="53f0a-242">Valid Values</span></span> |
|:--- |:--- |
| <span data-ttu-id="53f0a-243">apiVersion</span><span class="sxs-lookup"><span data-stu-id="53f0a-243">apiVersion</span></span> |<span data-ttu-id="53f0a-244">1.0</span><span class="sxs-lookup"><span data-stu-id="53f0a-244">1.0</span></span> |
|  | |
| <span data-ttu-id="53f0a-245">Begärandetext</span><span class="sxs-lookup"><span data-stu-id="53f0a-245">Request Body</span></span> |<span data-ttu-id="53f0a-246">INGEN</span><span class="sxs-lookup"><span data-stu-id="53f0a-246">NONE</span></span> |

<span data-ttu-id="53f0a-247">**Svaret**:</span><span class="sxs-lookup"><span data-stu-id="53f0a-247">**Response**:</span></span>

<span data-ttu-id="53f0a-248">HTTP-statuskod: 200</span><span class="sxs-lookup"><span data-stu-id="53f0a-248">HTTP Status code: 200</span></span>

* <span data-ttu-id="53f0a-249">`feed/entry/content/properties/Id`-Modell unikt ID.</span><span class="sxs-lookup"><span data-stu-id="53f0a-249">`feed/entry/content/properties/Id` - Model unique ID.</span></span>
* <span data-ttu-id="53f0a-250">`feed/entry/content/properties/Name`-Namn.</span><span class="sxs-lookup"><span data-stu-id="53f0a-250">`feed/entry/content/properties/Name` - Model name.</span></span>
* <span data-ttu-id="53f0a-251">`feed/entry/content/properties/Date`-Skapandedatum för modell.</span><span class="sxs-lookup"><span data-stu-id="53f0a-251">`feed/entry/content/properties/Date` - Model creation date.</span></span>
* <span data-ttu-id="53f0a-252">`feed/entry/content/properties/Status`-Status för modellen.</span><span class="sxs-lookup"><span data-stu-id="53f0a-252">`feed/entry/content/properties/Status` - Model status.</span></span> <span data-ttu-id="53f0a-253">En av hello följande:</span><span class="sxs-lookup"><span data-stu-id="53f0a-253">One of hello following:</span></span>
  * <span data-ttu-id="53f0a-254">-Modellen har skapats och innehåller inte katalog- och användningsdata.</span><span class="sxs-lookup"><span data-stu-id="53f0a-254">Created - Model is created and does not contain Catalog and Usage.</span></span>
  * <span data-ttu-id="53f0a-255">ReadyForBuild - modellen skapas och innehåller katalogen och användning.</span><span class="sxs-lookup"><span data-stu-id="53f0a-255">ReadyForBuild - Model is created and contains Catalog and Usage.</span></span>
* <span data-ttu-id="53f0a-256">`feed/entry/content/properties/HasActiveBuild`-Anger om hello modellen har har skapats.</span><span class="sxs-lookup"><span data-stu-id="53f0a-256">`feed/entry/content/properties/HasActiveBuild` - Indicates if hello model was built successfully.</span></span>
* <span data-ttu-id="53f0a-257">`feed/entry/content/properties/BuildId`-Modell active build-ID.</span><span class="sxs-lookup"><span data-stu-id="53f0a-257">`feed/entry/content/properties/BuildId` - Model active build ID.</span></span>
* <span data-ttu-id="53f0a-258">`feed/entry/content/properties/Mpr`-Modellen MPR (se ModelInsight för mer information).</span><span class="sxs-lookup"><span data-stu-id="53f0a-258">`feed/entry/content/properties/Mpr` - Model MPR (see ModelInsight for more information).</span></span>
* <span data-ttu-id="53f0a-259">`feed/entry/content/properties/UserName`-Modell interna användarnamn.</span><span class="sxs-lookup"><span data-stu-id="53f0a-259">`feed/entry/content/properties/UserName` - Model internal user name.</span></span>
* <span data-ttu-id="53f0a-260">`feed/entry/content/properties/UsageFileNames`-Lista över modellen användningsfiler avgränsade med kommatecken.</span><span class="sxs-lookup"><span data-stu-id="53f0a-260">`feed/entry/content/properties/UsageFileNames` - List of model usage files separated by comma.</span></span>
* <span data-ttu-id="53f0a-261">`feed/entry/content/properties/CatalogId`-Modell katalog-ID.</span><span class="sxs-lookup"><span data-stu-id="53f0a-261">`feed/entry/content/properties/CatalogId` - Model catalog ID.</span></span>
* <span data-ttu-id="53f0a-262">`feed/entry/content/properties/Description`-Beskrivning av modellen.</span><span class="sxs-lookup"><span data-stu-id="53f0a-262">`feed/entry/content/properties/Description` - Model description.</span></span>
* <span data-ttu-id="53f0a-263">`feed/entry/content/properties/CatalogFileName`-Namnet på modellfilen katalogen.</span><span class="sxs-lookup"><span data-stu-id="53f0a-263">`feed/entry/content/properties/CatalogFileName` - Model catalog file name.</span></span>

<span data-ttu-id="53f0a-264">OData-XML</span><span class="sxs-lookup"><span data-stu-id="53f0a-264">OData XML</span></span>

    <feed xmlns:base="https://api.datamarket.azure.com/amla/recommendations/v3/GetAllModels" xmlns:d="http://schemas.microsoft.com/ado/2007/08/dataservices" xmlns:m="http://schemas.microsoft.com/ado/2007/08/dataservices/metadata" xmlns="http://www.w3.org/2005/Atom">
        <title type="text" />
        <subtitle type="text">Get A List Of All Models</subtitle>
        <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetAllModels?apiVersion='1.0'</id>
        <rights type="text" />
        <updated>2014-10-28T14:35:51Z</updated>
        <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetAllModels?apiVersion='1.0'" />
        <entry>
            <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetAllModels?apiVersion='1.0'&amp;$skip=0&amp;$top=1</id>
            <title type="text">GetAListOfAllModelsEntity</title>
    <updated>2014-10-28T14:35:51Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetAllModels?apiVersion='1.0'&amp;$skip=0&amp;$top=1" />
            <content type="application/xml">
                <m:properties>
                    <d:Id m:type="Edm.String">68b232f2-1037-45f7-8f49-af822695ee63</d:Id>
                    <d:Name m:type="Edm.String">vah-11111</d:Name>
                    <d:Date m:type="Edm.String">10/28/2014 2:16:07 PM</d:Date>
                    <d:Status m:type="Edm.String">ReadyForBuild</d:Status>
                    <d:HasActiveBuild m:type="Edm.String">false</d:HasActiveBuild>
                    <d:BuildId m:type="Edm.String">-1</d:BuildId>
                    <d:Mpr m:type="Edm.String">0</d:Mpr>
                    <d:UsageFileNames m:type="Edm.String">ImplicitMatrix10_Guid_small.txt, ImplicitMatrix11_Guid_small.txt</d:UsageFileNames>
                    <d:CatalogId m:type="Edm.String">626babdb-cab6-43a6-82ef-4fb86345700c</d:CatalogId>
                    <d:UserName m:type="Edm.String">89dc4722-03ba-4f90-8821-b1db388408b5@dm.com</d:UserName>
                    <d:Description m:type="Edm.String">short description</d:Description>
                    <d:CatalogFileName m:type="Edm.String">catalog10_small.txt</d:CatalogFileName>
                </m:properties>
            </content>
        </entry>
    </feed>

### <a name="54----update-model"></a><span data-ttu-id="53f0a-265">5.4.</span><span class="sxs-lookup"><span data-stu-id="53f0a-265">5.4.</span></span>    <span data-ttu-id="53f0a-266">Uppdatera modellen</span><span class="sxs-lookup"><span data-stu-id="53f0a-266">Update Model</span></span>
<span data-ttu-id="53f0a-267">Du kan uppdatera hello beskrivning eller hello active build-ID.</span><span class="sxs-lookup"><span data-stu-id="53f0a-267">You can update hello model description or hello active build ID.</span></span><br><span data-ttu-id="53f0a-268">
<ins>Aktiva build-ID</ins> -varje build för varje modell har ett build-ID.</span><span class="sxs-lookup"><span data-stu-id="53f0a-268">
<ins>Active build ID</ins> - Every build for every model has a build ID.</span></span> <span data-ttu-id="53f0a-269">hello active build-ID är hello första lyckade version av varje ny modell.</span><span class="sxs-lookup"><span data-stu-id="53f0a-269">hello active build ID is hello first successful build of every new model.</span></span> <span data-ttu-id="53f0a-270">När du har en aktiv build-ID och du göra ytterligare versioner för hello samma modell som du behöver tooexplicitly ange den som hello standard skapa ID om du vill.</span><span class="sxs-lookup"><span data-stu-id="53f0a-270">Once you have an active build ID and you do additional builds for hello same model, you need tooexplicitly set it as hello default build ID if you want to.</span></span> <span data-ttu-id="53f0a-271">När du använder rekommendationer om du inte anger hello build-ID som du vill toouse hello-standard som ska användas automatiskt.</span><span class="sxs-lookup"><span data-stu-id="53f0a-271">When you consume recommendations, if you do not specify hello build ID that you want toouse, hello default one will be used automatically.</span></span><br>
<span data-ttu-id="53f0a-272">Den här mekanismen gör att du - när du har en rekommendation modell i produktionen, nya modeller för toobuild och testa dem innan du höjer upp dem tooproduction.</span><span class="sxs-lookup"><span data-stu-id="53f0a-272">This mechanism enables you - once you have a recommendation model in production - toobuild new models and test them before you promote them tooproduction.</span></span>

| <span data-ttu-id="53f0a-273">HTTP-metoden</span><span class="sxs-lookup"><span data-stu-id="53f0a-273">HTTP Method</span></span> | <span data-ttu-id="53f0a-274">URI: N</span><span class="sxs-lookup"><span data-stu-id="53f0a-274">URI</span></span> |
|:--- |:--- |
| <span data-ttu-id="53f0a-275">PLACERA</span><span class="sxs-lookup"><span data-stu-id="53f0a-275">PUT</span></span> |`<rootURI>/UpdateModel?id=%27<modelId>%27&apiVersion=%271.0%27`<br><span data-ttu-id="53f0a-276">Exempel:</span><span class="sxs-lookup"><span data-stu-id="53f0a-276">Example:</span></span><br>`<rootURI>/UpdateModel?id=%279559872f-7a53-4076-a3c7-19d9385c1265%27&apiVersion=%271.0%27` |

| <span data-ttu-id="53f0a-277">Parameternamn</span><span class="sxs-lookup"><span data-stu-id="53f0a-277">Parameter Name</span></span> | <span data-ttu-id="53f0a-278">Giltiga värden</span><span class="sxs-lookup"><span data-stu-id="53f0a-278">Valid Values</span></span> |
|:--- |:--- |
| <span data-ttu-id="53f0a-279">id</span><span class="sxs-lookup"><span data-stu-id="53f0a-279">id</span></span> |<span data-ttu-id="53f0a-280">Unik identifierare för hello modellen (skiftlägeskänslig)</span><span class="sxs-lookup"><span data-stu-id="53f0a-280">Unique identifier of hello model (case sensitive)</span></span> |
| <span data-ttu-id="53f0a-281">apiVersion</span><span class="sxs-lookup"><span data-stu-id="53f0a-281">apiVersion</span></span> |<span data-ttu-id="53f0a-282">1.0</span><span class="sxs-lookup"><span data-stu-id="53f0a-282">1.0</span></span> |
|  | |
| <span data-ttu-id="53f0a-283">Begärandetext</span><span class="sxs-lookup"><span data-stu-id="53f0a-283">Request Body</span></span> |`<ModelUpdateParams xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">`<br>`<Description>New Description</Description>`<br>`<ActiveBuildId>-1</ActiveBuildId>`<br>` </ModelUpdateParams>`<br><br><span data-ttu-id="53f0a-284">Observera att hello XML taggar beskrivning och ActiveBuildId är valfria.</span><span class="sxs-lookup"><span data-stu-id="53f0a-284">Note that hello XML tags Description and ActiveBuildId are optional.</span></span> <span data-ttu-id="53f0a-285">Om du inte vill att tooset beskrivning eller ActiveBuildId bort hela hello-taggen.</span><span class="sxs-lookup"><span data-stu-id="53f0a-285">If you do not want tooset Description or ActiveBuildId, remove hello entire tag.</span></span> |

<span data-ttu-id="53f0a-286">**Svaret**:</span><span class="sxs-lookup"><span data-stu-id="53f0a-286">**Response**:</span></span>

<span data-ttu-id="53f0a-287">HTTP-statuskod: 200</span><span class="sxs-lookup"><span data-stu-id="53f0a-287">HTTP Status code: 200</span></span>

### <a name="55----delete-model"></a><span data-ttu-id="53f0a-288">5.5.</span><span class="sxs-lookup"><span data-stu-id="53f0a-288">5.5.</span></span>    <span data-ttu-id="53f0a-289">Ta bort modellen</span><span class="sxs-lookup"><span data-stu-id="53f0a-289">Delete Model</span></span>
<span data-ttu-id="53f0a-290">Tar bort en befintlig modell efter-ID.</span><span class="sxs-lookup"><span data-stu-id="53f0a-290">Deletes an existing model by ID.</span></span>

| <span data-ttu-id="53f0a-291">HTTP-metoden</span><span class="sxs-lookup"><span data-stu-id="53f0a-291">HTTP Method</span></span> | <span data-ttu-id="53f0a-292">URI: N</span><span class="sxs-lookup"><span data-stu-id="53f0a-292">URI</span></span> |
|:--- |:--- |
| <span data-ttu-id="53f0a-293">TA BORT</span><span class="sxs-lookup"><span data-stu-id="53f0a-293">DELETE</span></span> |`<rootURI>/DeleteModel?id=%27<model_id>%27&apiVersion=%271.0%27`<br><span data-ttu-id="53f0a-294">Exempel:</span><span class="sxs-lookup"><span data-stu-id="53f0a-294">Example:</span></span><br>`<rootURI>/DeleteModel?id=%271cac7b76-def4-41f1-bc81-29b806adb1de%27&apiVersion=%271.0%27` |

| <span data-ttu-id="53f0a-295">Parameternamn</span><span class="sxs-lookup"><span data-stu-id="53f0a-295">Parameter Name</span></span> | <span data-ttu-id="53f0a-296">Giltiga värden</span><span class="sxs-lookup"><span data-stu-id="53f0a-296">Valid Values</span></span> |
|:--- |:--- |
| <span data-ttu-id="53f0a-297">id</span><span class="sxs-lookup"><span data-stu-id="53f0a-297">id</span></span> |<span data-ttu-id="53f0a-298">Unik identifierare för hello modellen (skiftlägeskänslig)</span><span class="sxs-lookup"><span data-stu-id="53f0a-298">Unique identifier of hello model (case sensitive)</span></span> |
| <span data-ttu-id="53f0a-299">apiVersion</span><span class="sxs-lookup"><span data-stu-id="53f0a-299">apiVersion</span></span> |<span data-ttu-id="53f0a-300">1.0</span><span class="sxs-lookup"><span data-stu-id="53f0a-300">1.0</span></span> |
|  | |
| <span data-ttu-id="53f0a-301">Begärandetext</span><span class="sxs-lookup"><span data-stu-id="53f0a-301">Request Body</span></span> |<span data-ttu-id="53f0a-302">INGEN</span><span class="sxs-lookup"><span data-stu-id="53f0a-302">NONE</span></span> |

<span data-ttu-id="53f0a-303">**Svaret**:</span><span class="sxs-lookup"><span data-stu-id="53f0a-303">**Response**:</span></span>

<span data-ttu-id="53f0a-304">HTTP-statuskod: 200</span><span class="sxs-lookup"><span data-stu-id="53f0a-304">HTTP Status code: 200</span></span>

<span data-ttu-id="53f0a-305">OData-XML</span><span class="sxs-lookup"><span data-stu-id="53f0a-305">OData XML</span></span>

    <feed xmlns:base="https://api.datamarket.azure.com/amla/recommendations/v3/DeleteModel" xmlns:d="http://schemas.microsoft.com/ado/2007/08/dataservices" xmlns:m="http://schemas.microsoft.com/ado/2007/08/dataservices/metadata" xmlns="http://www.w3.org/2005/Atom">
      <title type="text" />
      <subtitle type="text">Delete Model by Id</subtitle>
      <id>https://api.datamarket.azure.com/amla/recommendations/v3/DeleteModel?id='1cac7b76-def4-41f1-bc81-29b806adb1de'&amp;apiVersion='1.0'</id>
      <rights type="text" />
      <updated>2014-10-28T10:39:33Z</updated>
      <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/DeleteModel?id='1cac7b76-def4-41f1-bc81-29b806adb1de'&amp;apiVersion='1.0'" />
      <entry>
    <id>https://api.datamarket.azure.com/amla/recommendations/v3/DeleteModel?id='1cac7b76-def4-41f1-bc81-29b806adb1de'&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1</id>
    <title type="text">DeleteModelByIdEntity</title>
    <updated>2014-10-28T10:39:33Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/DeleteModel?id='1cac7b76-def4-41f1-bc81-29b806adb1de'&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1" />
    <content type="application/xml">
      <m:properties>
        <d:string m:type="Edm.String"></d:string>
      </m:properties>
    </content>
      </entry>
    </feed>

## <a name="6-model-advanced"></a><span data-ttu-id="53f0a-306">6. Avancerade modellen</span><span class="sxs-lookup"><span data-stu-id="53f0a-306">6. Model Advanced</span></span>
### <a name="61----model-data-insight"></a><span data-ttu-id="53f0a-307">6.1.</span><span class="sxs-lookup"><span data-stu-id="53f0a-307">6.1.</span></span>    <span data-ttu-id="53f0a-308">Modellen Data Insight</span><span class="sxs-lookup"><span data-stu-id="53f0a-308">Model Data Insight</span></span>
<span data-ttu-id="53f0a-309">Returnerar statistiska data på hello användningsdata som den här modellen har skapats med.</span><span class="sxs-lookup"><span data-stu-id="53f0a-309">Returns statistical data on hello usage data that this model was built with.</span></span>

<span data-ttu-id="53f0a-310">Endast tillgängligt för rekommendation build.</span><span class="sxs-lookup"><span data-stu-id="53f0a-310">Available only for Recommendation build.</span></span>

| <span data-ttu-id="53f0a-311">HTTP-metoden</span><span class="sxs-lookup"><span data-stu-id="53f0a-311">HTTP Method</span></span> | <span data-ttu-id="53f0a-312">URI: N</span><span class="sxs-lookup"><span data-stu-id="53f0a-312">URI</span></span> |
|:--- |:--- |
| <span data-ttu-id="53f0a-313">HÄMTA</span><span class="sxs-lookup"><span data-stu-id="53f0a-313">GET</span></span> |`<rootURI>/GetDataInsight?modelId=%27<model_id>%27&apiVersion=%271.0%27`<br><span data-ttu-id="53f0a-314">Exempel:</span><span class="sxs-lookup"><span data-stu-id="53f0a-314">Example:</span></span><br>`<rootURI>/GetDataInsight?modelId=%271cac7b76-def4-41f1-bc81-29b806adb1de%27&apiVersion=%271.0%27` |

| <span data-ttu-id="53f0a-315">Parameternamn</span><span class="sxs-lookup"><span data-stu-id="53f0a-315">Parameter Name</span></span> | <span data-ttu-id="53f0a-316">Giltiga värden</span><span class="sxs-lookup"><span data-stu-id="53f0a-316">Valid Values</span></span> |
|:--- |:--- |
| <span data-ttu-id="53f0a-317">%{ModelID/</span><span class="sxs-lookup"><span data-stu-id="53f0a-317">modelId</span></span> |<span data-ttu-id="53f0a-318">Unik identifierare för hello modellen</span><span class="sxs-lookup"><span data-stu-id="53f0a-318">Unique identifier of hello model</span></span> |
| <span data-ttu-id="53f0a-319">apiVersion</span><span class="sxs-lookup"><span data-stu-id="53f0a-319">apiVersion</span></span> |<span data-ttu-id="53f0a-320">1.0</span><span class="sxs-lookup"><span data-stu-id="53f0a-320">1.0</span></span> |
|  | |
| <span data-ttu-id="53f0a-321">Begärandetext</span><span class="sxs-lookup"><span data-stu-id="53f0a-321">Request Body</span></span> |<span data-ttu-id="53f0a-322">INGEN</span><span class="sxs-lookup"><span data-stu-id="53f0a-322">NONE</span></span> |

<span data-ttu-id="53f0a-323">**Svaret**:</span><span class="sxs-lookup"><span data-stu-id="53f0a-323">**Response**:</span></span>

<span data-ttu-id="53f0a-324">HTTP-statuskod: 200</span><span class="sxs-lookup"><span data-stu-id="53f0a-324">HTTP Status code: 200</span></span>

<span data-ttu-id="53f0a-325">hello data returneras som en uppsättning egenskaper.</span><span class="sxs-lookup"><span data-stu-id="53f0a-325">hello data is returned as a collection of properties.</span></span>

* <span data-ttu-id="53f0a-326">`feed/entry/id/content/properties/key`-Innehåller hello egenskapsnamn.</span><span class="sxs-lookup"><span data-stu-id="53f0a-326">`feed/entry/id/content/properties/key` - Holds hello property name.</span></span>
* <span data-ttu-id="53f0a-327">`feed/entry/id/content/properties/value`-Innehåller hello egenskapsvärde.</span><span class="sxs-lookup"><span data-stu-id="53f0a-327">`feed/entry/id/content/properties/value` - Holds hello property value.</span></span>

<span data-ttu-id="53f0a-328">hello tabellen nedan visar hello-värde som representerar varje nyckel.</span><span class="sxs-lookup"><span data-stu-id="53f0a-328">hello table below depicts hello value that each key represents.</span></span>

| <span data-ttu-id="53f0a-329">Nyckel</span><span class="sxs-lookup"><span data-stu-id="53f0a-329">Key</span></span> | <span data-ttu-id="53f0a-330">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="53f0a-330">Description</span></span> |
|:--- |:--- |
| <span data-ttu-id="53f0a-331">AvgItemLength</span><span class="sxs-lookup"><span data-stu-id="53f0a-331">AvgItemLength</span></span> |<span data-ttu-id="53f0a-332">Genomsnittligt antal specifika användare per artikel.</span><span class="sxs-lookup"><span data-stu-id="53f0a-332">Average number of distinct users per item.</span></span> |
| <span data-ttu-id="53f0a-333">AvgUserLength</span><span class="sxs-lookup"><span data-stu-id="53f0a-333">AvgUserLength</span></span> |<span data-ttu-id="53f0a-334">Genomsnittligt antal distinkta objekt per användare.</span><span class="sxs-lookup"><span data-stu-id="53f0a-334">Average number of distinct items per user.</span></span> |
| <span data-ttu-id="53f0a-335">DensificationNumberOfItems</span><span class="sxs-lookup"><span data-stu-id="53f0a-335">DensificationNumberOfItems</span></span> |<span data-ttu-id="53f0a-336">Antal objekt efter katalogrensning objekt som kan utformas.</span><span class="sxs-lookup"><span data-stu-id="53f0a-336">Number of items after pruning items that cannot be modelled.</span></span> |
| <span data-ttu-id="53f0a-337">DensificationNumberOfUsers</span><span class="sxs-lookup"><span data-stu-id="53f0a-337">DensificationNumberOfUsers</span></span> |<span data-ttu-id="53f0a-338">Antal användning poäng efter katalogrensning användare och objekt som kan utformas.</span><span class="sxs-lookup"><span data-stu-id="53f0a-338">Number of usage points after pruning users and items that can't be modelled.</span></span> |
| <span data-ttu-id="53f0a-339">DensificationNumberOfRecords</span><span class="sxs-lookup"><span data-stu-id="53f0a-339">DensificationNumberOfRecords</span></span> |<span data-ttu-id="53f0a-340">Antal användning poäng efter katalogrensning användare och objekt som kan utformas.</span><span class="sxs-lookup"><span data-stu-id="53f0a-340">Number of usage points after pruning users and items that can't be modelled.</span></span> |
| <span data-ttu-id="53f0a-341">MaxItemLength</span><span class="sxs-lookup"><span data-stu-id="53f0a-341">MaxItemLength</span></span> |<span data-ttu-id="53f0a-342">Antal specifika användare för hello populäraste objektet.</span><span class="sxs-lookup"><span data-stu-id="53f0a-342">Number of distinct users for hello most popular item.</span></span> |
| <span data-ttu-id="53f0a-343">MaxUserLength</span><span class="sxs-lookup"><span data-stu-id="53f0a-343">MaxUserLength</span></span> |<span data-ttu-id="53f0a-344">Maximalt antal distinkta objekt för en användare.</span><span class="sxs-lookup"><span data-stu-id="53f0a-344">Maximal number of distinct items for a user.</span></span> |
| <span data-ttu-id="53f0a-345">MinItemLength</span><span class="sxs-lookup"><span data-stu-id="53f0a-345">MinItemLength</span></span> |<span data-ttu-id="53f0a-346">Maximalt antal specifika användare för ett objekt.</span><span class="sxs-lookup"><span data-stu-id="53f0a-346">Maximal number of distinct users for an item.</span></span> |
| <span data-ttu-id="53f0a-347">MinUserLength</span><span class="sxs-lookup"><span data-stu-id="53f0a-347">MinUserLength</span></span> |<span data-ttu-id="53f0a-348">Minimalt antal distinkta objekt för en användare.</span><span class="sxs-lookup"><span data-stu-id="53f0a-348">Minimal number of distinct items for a user.</span></span> |
| <span data-ttu-id="53f0a-349">RawNumberOfItems</span><span class="sxs-lookup"><span data-stu-id="53f0a-349">RawNumberOfItems</span></span> |<span data-ttu-id="53f0a-350">Antal objekt i hello användningsfiler.</span><span class="sxs-lookup"><span data-stu-id="53f0a-350">Number of items in hello usage files.</span></span> |
| <span data-ttu-id="53f0a-351">RawNumberOfUsers</span><span class="sxs-lookup"><span data-stu-id="53f0a-351">RawNumberOfUsers</span></span> |<span data-ttu-id="53f0a-352">Antal användning poäng innan alla rensning.</span><span class="sxs-lookup"><span data-stu-id="53f0a-352">Number of usage points before any pruning.</span></span> |
| <span data-ttu-id="53f0a-353">RawNumberOfRecords</span><span class="sxs-lookup"><span data-stu-id="53f0a-353">RawNumberOfRecords</span></span> |<span data-ttu-id="53f0a-354">Antal användning poäng innan alla rensning.</span><span class="sxs-lookup"><span data-stu-id="53f0a-354">Number of usage points before any pruning.</span></span> |
| <span data-ttu-id="53f0a-355">SamplingNumberOfItems</span><span class="sxs-lookup"><span data-stu-id="53f0a-355">SamplingNumberOfItems</span></span> |<span data-ttu-id="53f0a-356">Saknas</span><span class="sxs-lookup"><span data-stu-id="53f0a-356">N/A</span></span> |
| <span data-ttu-id="53f0a-357">SamplingNumberOfRecords</span><span class="sxs-lookup"><span data-stu-id="53f0a-357">SamplingNumberOfRecords</span></span> |<span data-ttu-id="53f0a-358">Saknas</span><span class="sxs-lookup"><span data-stu-id="53f0a-358">N/A</span></span> |
| <span data-ttu-id="53f0a-359">SamplingNumberOfUsers</span><span class="sxs-lookup"><span data-stu-id="53f0a-359">SamplingNumberOfUsers</span></span> |<span data-ttu-id="53f0a-360">Saknas</span><span class="sxs-lookup"><span data-stu-id="53f0a-360">N/A</span></span> |

<span data-ttu-id="53f0a-361">OData-XML</span><span class="sxs-lookup"><span data-stu-id="53f0a-361">OData XML</span></span>

    <feed xmlns:base="https://api.datamarket.azure.com/amla/recommendations/v3/GetDataInsight" xmlns:d="http://schemas.microsoft.com/ado/2007/08/dataservices" xmlns:m="http://schemas.microsoft.com/ado/2007/08/dataservices/metadata" xmlns="http://www.w3.org/2005/Atom">
    <title type="text" />
    <subtitle type="text">Get data insight statistics</subtitle>
    <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetDataInsight?modelId='6254b40d-0514-49cb-a298-b81256d2b3ca'&amp;apiVersion='1.0'</id>
    <rights type="text" />
    <updated>2014-10-27T14:21:21Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetDataInsight?modelId='6254b40d-0514-49cb-a298-b81256d2b3ca'&amp;apiVersion='1.0'" />
    <entry>
    <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetDataInsight?modelId='6254b40d-0514-49cb-a298-b81256d2b3ca'&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1</id>
    <title type="text">GetDataInsightStatisticsEntity</title>
    <updated>2014-10-27T14:21:21Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetDataInsight?modelId='6254b40d-0514-49cb-a298-b81256d2b3ca'&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1" />
    <content type="application/xml">
      <m:properties>
        <d:Key m:type="Edm.String">AvgItemLength</d:Key>
        <d:Value m:type="Edm.String">18.936</d:Value>
      </m:properties>
    </content>
    </entry>
    <entry>
    <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetDataInsight?modelId='6254b40d-0514-49cb-a298-b81256d2b3ca'&amp;apiVersion='1.0'&amp;$skip=1&amp;$top=1</id>
    <title type="text">GetDataInsightStatisticsEntity</title>
    <updated>2014-10-27T14:21:21Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetDataInsight?modelId='6254b40d-0514-49cb-a298-b81256d2b3ca'&amp;apiVersion='1.0'&amp;$skip=1&amp;$top=1" />
    <content type="application/xml">
      <m:properties>
        <d:Key m:type="Edm.String">AvgUserLength</d:Key>
        <d:Value m:type="Edm.String">51.879</d:Value>
      </m:properties>
    </content>
    </entry>
    <entry>
    <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetDataInsight?modelId='6254b40d-0514-49cb-a298-b81256d2b3ca'&amp;apiVersion='1.0'&amp;$skip=2&amp;$top=1</id>
    <title type="text">GetDataInsightStatisticsEntity</title>
    <updated>2014-10-27T14:21:21Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetDataInsight?modelId='6254b40d-0514-49cb-a298-b81256d2b3ca'&amp;apiVersion='1.0'&amp;$skip=2&amp;$top=1" />
    <content type="application/xml">
      <m:properties>
        <d:Key m:type="Edm.String">DensificationNumberOfItems</d:Key>
        <d:Value m:type="Edm.String">2,000</d:Value>
      </m:properties>
    </content>
    </entry>
    <entry>
        <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetDataInsight?modelId='6254b40d-0514-49cb-a298-b81256d2b3ca'&amp;apiVersion='1.0'&amp;$skip=3&amp;$top=1</id>
    <title type="text">GetDataInsightStatisticsEntity</title>
    <updated>2014-10-27T14:21:21Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetDataInsight?modelId='6254b40d-0514-49cb-a298-b81256d2b3ca'&amp;apiVersion='1.0'&amp;$skip=3&amp;$top=1" />
    <content type="application/xml">
      <m:properties>
        <d:Key m:type="Edm.String">DensificationNumberOfRecords</d:Key>
        <d:Value m:type="Edm.String">37,872</d:Value>
      </m:properties>
    </content>
    </entry>
    <entry>
        <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetDataInsight?modelId='6254b40d-0514-49cb-a298-b81256d2b3ca'&amp;apiVersion='1.0'&amp;$skip=4&amp;$top=1</id>
    <title type="text">GetDataInsightStatisticsEntity</title>
    <updated>2014-10-27T14:21:21Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetDataInsight?modelId='6254b40d-0514-49cb-a298-b81256d2b3ca'&amp;apiVersion='1.0'&amp;$skip=4&amp;$top=1" />
    <content type="application/xml">
        <m:properties>
            <d:Key m:type="Edm.String">DensificationNumberOfUsers</d:Key>
            <d:Value m:type="Edm.String">730</d:Value>
      </m:properties>
    </content>
    </entry>
    <entry>
        <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetDataInsight?modelId='6254b40d-0514-49cb-a298-b81256d2b3ca'&amp;apiVersion='1.0'&amp;$skip=5&amp;$top=1</id>
        <title type="text">GetDataInsightStatisticsEntity</title>
        <updated>2014-10-27T14:21:21Z</updated>
        <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetDataInsight?modelId='6254b40d-0514-49cb-a298-b81256d2b3ca'&amp;apiVersion='1.0'&amp;$skip=5&amp;$top=1" />
        <content type="application/xml">
            <m:properties>
                <d:Key m:type="Edm.String">MaxItemLength</d:Key>
                <d:Value m:type="Edm.String">4,704</d:Value>
            </m:properties>
        </content>
    </entry>
    <entry>
        <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetDataInsight?modelId='6254b40d-0514-49cb-a298-b81256d2b3ca'&amp;apiVersion='1.0'&amp;$skip=6&amp;$top=1</id>
        <title type="text">GetDataInsightStatisticsEntity</title>
        <updated>2014-10-27T14:21:21Z</updated>
        <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetDataInsight?modelId='6254b40d-0514-49cb-a298-b81256d2b3ca'&amp;apiVersion='1.0'&amp;$skip=6&amp;$top=1" />
        <content type="application/xml">
            <m:properties>
                <d:Key m:type="Edm.String">MaxUserLength</d:Key>
                <d:Value m:type="Edm.String">190</d:Value>
            </m:properties>
        </content>
    </entry>
    <entry>
        <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetDataInsight?modelId='6254b40d-0514-49cb-a298-b81256d2b3ca'&amp;apiVersion='1.0'&amp;$skip=7&amp;$top=1</id>
        <title type="text">GetDataInsightStatisticsEntity</title>
        <updated>2014-10-27T14:21:21Z</updated>
        <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetDataInsight?modelId='6254b40d-0514-49cb-a298-b81256d2b3ca'&amp;apiVersion='1.0'&amp;$skip=7&amp;$top=1" />
        <content type="application/xml">
            <m:properties>
                <d:Key m:type="Edm.String">MinItemLength</d:Key>
                <d:Value m:type="Edm.String">8</d:Value>
            </m:properties>
        </content>
    </entry>
    <entry>
        <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetDataInsight?modelId='6254b40d-0514-49cb-a298-b81256d2b3ca'&amp;apiVersion='1.0'&amp;$skip=8&amp;$top=1</id>
        <title type="text">GetDataInsightStatisticsEntity</title>
        <updated>2014-10-27T14:21:21Z</updated>
        <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetDataInsight?modelId='6254b40d-0514-49cb-a298-b81256d2b3ca'&amp;apiVersion='1.0'&amp;$skip=8&amp;$top=1" />
        <content type="application/xml">
            <m:properties>
                <d:Key m:type="Edm.String">MinUserLength</d:Key>
                <d:Value m:type="Edm.String">20</d:Value>
            </m:properties>
        </content>
    </entry>
    <entry>
        <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetDataInsight?modelId='6254b40d-0514-49cb-a298-b81256d2b3ca'&amp;apiVersion='1.0'&amp;$skip=9&amp;$top=1</id>
        <title type="text">GetDataInsightStatisticsEntity</title>
        <updated>2014-10-27T14:21:21Z</updated>
        <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetDataInsight?modelId='6254b40d-0514-49cb-a298-b81256d2b3ca'&amp;apiVersion='1.0'&amp;$skip=9&amp;$top=1" />
        <content type="application/xml">
            <m:properties>
                <d:Key m:type="Edm.String">RawNumberOfItems</d:Key>
                <d:Value m:type="Edm.String">2,000</d:Value>
            </m:properties>
        </content>
    </entry>
    <entry>
        <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetDataInsight?modelId='6254b40d-0514-49cb-a298-b81256d2b3ca'&amp;apiVersion='1.0'&amp;$skip=10&amp;$top=1</id>
        <title type="text">GetDataInsightStatisticsEntity</title>
        <updated>2014-10-27T14:21:21Z</updated>
        <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetDataInsight?modelId='6254b40d-0514-49cb-a298-b81256d2b3ca'&amp;apiVersion='1.0'&amp;$skip=10&amp;$top=1" />
        <content type="application/xml">
            <m:properties>
                <d:Key m:type="Edm.String">RawNumberOfRecords</d:Key>
                <d:Value m:type="Edm.String">72,022</d:Value>
            </m:properties>
        </content>
    </entry>
    <entry>
        <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetDataInsight?modelId='6254b40d-0514-49cb-a298-b81256d2b3ca'&amp;apiVersion='1.0'&amp;$skip=11&amp;$top=1</id>
        <title type="text">GetDataInsightStatisticsEntity</title>
        <updated>2014-10-27T14:21:21Z</updated>
        <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetDataInsight?modelId='6254b40d-0514-49cb-a298-b81256d2b3ca'&amp;apiVersion='1.0'&amp;$skip=11&amp;$top=1" />
        <content type="application/xml">
            <m:properties>
                <d:Key m:type="Edm.String">RawNumberOfUsers</d:Key>
                <d:Value m:type="Edm.String">9,044</d:Value>
            </m:properties>
        </content>
    </entry>
    <entry>
        <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetDataInsight?modelId='6254b40d-0514-49cb-a298-b81256d2b3ca'&amp;apiVersion='1.0'&amp;$skip=12&amp;$top=1</id>
        <title type="text">GetDataInsightStatisticsEntity</title>
        <updated>2014-10-27T14:21:21Z</updated>
        <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetDataInsight?modelId='6254b40d-0514-49cb-a298-b81256d2b3ca'&amp;apiVersion='1.0'&amp;$skip=12&amp;$top=1" />
        <content type="application/xml">
            <m:properties>
                <d:Key m:type="Edm.String">SampelingNumberOfItems</d:Key>
                <d:Value m:type="Edm.String">2,000</d:Value>
            </m:properties>
        </content>
    </entry>
    <entry>
        <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetDataInsight?modelId='6254b40d-0514-49cb-a298-b81256d2b3ca'&amp;apiVersion='1.0'&amp;$skip=13&amp;$top=1</id>
        <title type="text">GetDataInsightStatisticsEntity</title>
        <updated>2014-10-27T14:21:21Z</updated>
        <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetDataInsight?modelId='6254b40d-0514-49cb-a298-b81256d2b3ca'&amp;apiVersion='1.0'&amp;$skip=13&amp;$top=1" />
        <content type="application/xml">
            <m:properties>
                <d:Key m:type="Edm.String">SampelingNumberOfRecords</d:Key>
                <d:Value m:type="Edm.String">72,022</d:Value>
            </m:properties>
        </content>
    </entry>
    <entry>
        <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetDataInsight?modelId='6254b40d-0514-49cb-a298-b81256d2b3ca'&amp;apiVersion='1.0'&amp;$skip=14&amp;$top=1</id>
        <title type="text">GetDataInsightStatisticsEntity</title>
        <updated>2014-10-27T14:21:21Z</updated>
        <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetDataInsight?modelId='6254b40d-0514-49cb-a298-b81256d2b3ca'&amp;apiVersion='1.0'&amp;$skip=14&amp;$top=1" />
        <content type="application/xml">
            <m:properties>
                <d:Key m:type="Edm.String">SampelingNumberOfUsers</d:Key>
                <d:Value m:type="Edm.String">9,044</d:Value>
            </m:properties>
        </content>
    </entry>
    </feed>

### <a name="62----model-insight"></a><span data-ttu-id="53f0a-362">6.2.</span><span class="sxs-lookup"><span data-stu-id="53f0a-362">6.2.</span></span>    <span data-ttu-id="53f0a-363">Modellen Insight</span><span class="sxs-lookup"><span data-stu-id="53f0a-363">Model Insight</span></span>
<span data-ttu-id="53f0a-364">Returnerar modell insight hello active build eller (om det angetts) på en viss version.</span><span class="sxs-lookup"><span data-stu-id="53f0a-364">Returns model insight on hello active build or (if given) on a specific build.</span></span>

<span data-ttu-id="53f0a-365">Endast tillgängligt för rekommendation build.</span><span class="sxs-lookup"><span data-stu-id="53f0a-365">Available only for Recommendation build.</span></span>

| <span data-ttu-id="53f0a-366">HTTP-metoden</span><span class="sxs-lookup"><span data-stu-id="53f0a-366">HTTP Method</span></span> | <span data-ttu-id="53f0a-367">URI: N</span><span class="sxs-lookup"><span data-stu-id="53f0a-367">URI</span></span> |
|:--- |:--- |
| <span data-ttu-id="53f0a-368">HÄMTA</span><span class="sxs-lookup"><span data-stu-id="53f0a-368">GET</span></span> |<span data-ttu-id="53f0a-369">Med active build-ID:</span><span class="sxs-lookup"><span data-stu-id="53f0a-369">With active build ID:</span></span><br>`<rootURI>/GetModelInsight?modelId=%27<model_id>%27&apiVersion=%271.0%27`<br><br><span data-ttu-id="53f0a-370">Exempel:</span><span class="sxs-lookup"><span data-stu-id="53f0a-370">Example:</span></span><br>`<rootURI>/GetModelInsight?modelId=%271cac7b76-def4-41f1-bc81-29b806adb1de%27&apiVersion=%271.0%27`<br><br><span data-ttu-id="53f0a-371">Med specifika build-ID:</span><span class="sxs-lookup"><span data-stu-id="53f0a-371">With specific build ID:</span></span><br>`<rootURI>/GetModelInsight?modelId=%27<model_id>%27&buildId=%27<build_id>%27&apiVersion=%271.0%27` |

| <span data-ttu-id="53f0a-372">Parameternamn</span><span class="sxs-lookup"><span data-stu-id="53f0a-372">Parameter Name</span></span> | <span data-ttu-id="53f0a-373">Giltiga värden</span><span class="sxs-lookup"><span data-stu-id="53f0a-373">Valid Values</span></span> |
|:--- |:--- |
| <span data-ttu-id="53f0a-374">%{ModelID/</span><span class="sxs-lookup"><span data-stu-id="53f0a-374">modelId</span></span> |<span data-ttu-id="53f0a-375">Unik identifierare för hello modellen</span><span class="sxs-lookup"><span data-stu-id="53f0a-375">Unique identifier of hello model</span></span> |
| <span data-ttu-id="53f0a-376">buildId</span><span class="sxs-lookup"><span data-stu-id="53f0a-376">buildId</span></span> |<span data-ttu-id="53f0a-377">Valfritt - nummer som identifierar en lyckad version.</span><span class="sxs-lookup"><span data-stu-id="53f0a-377">Optional - number that identifies a successful build.</span></span> |
| <span data-ttu-id="53f0a-378">apiVersion</span><span class="sxs-lookup"><span data-stu-id="53f0a-378">apiVersion</span></span> |<span data-ttu-id="53f0a-379">1.0</span><span class="sxs-lookup"><span data-stu-id="53f0a-379">1.0</span></span> |
|  | |
| <span data-ttu-id="53f0a-380">Begärandetext</span><span class="sxs-lookup"><span data-stu-id="53f0a-380">Request Body</span></span> |<span data-ttu-id="53f0a-381">INGEN</span><span class="sxs-lookup"><span data-stu-id="53f0a-381">NONE</span></span> |

<span data-ttu-id="53f0a-382">**Svaret**:</span><span class="sxs-lookup"><span data-stu-id="53f0a-382">**Response**:</span></span>

<span data-ttu-id="53f0a-383">HTTP-statuskod: 200</span><span class="sxs-lookup"><span data-stu-id="53f0a-383">HTTP Status code: 200</span></span>

<span data-ttu-id="53f0a-384">hello data returneras som en uppsättning egenskaper.</span><span class="sxs-lookup"><span data-stu-id="53f0a-384">hello data is returned as a collection of properties.</span></span>

* `feed/entry/id/content/properties/key`
* `feed/entry/id/content/properties/value`

<span data-ttu-id="53f0a-385">hello tabellen nedan visar hello-värde som representerar varje nyckel.</span><span class="sxs-lookup"><span data-stu-id="53f0a-385">hello table below depicts hello value that each key represents.</span></span>

| <span data-ttu-id="53f0a-386">Nyckel</span><span class="sxs-lookup"><span data-stu-id="53f0a-386">Key</span></span> | <span data-ttu-id="53f0a-387">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="53f0a-387">Description</span></span> |
|:--- |:--- |
| <span data-ttu-id="53f0a-388">CatalogCoverage</span><span class="sxs-lookup"><span data-stu-id="53f0a-388">CatalogCoverage</span></span> |<span data-ttu-id="53f0a-389">Vilken del av hello katalog kan utformas med användningsmönster.</span><span class="sxs-lookup"><span data-stu-id="53f0a-389">What part of hello catalog can be modelled with usage patterns.</span></span> <span data-ttu-id="53f0a-390">hello resten av hello objekt behöver innehållsbaserad funktioner.</span><span class="sxs-lookup"><span data-stu-id="53f0a-390">hello rest of hello items will need content-based features.</span></span> |
| <span data-ttu-id="53f0a-391">MPR</span><span class="sxs-lookup"><span data-stu-id="53f0a-391">Mpr</span></span> |<span data-ttu-id="53f0a-392">Medelvärde percentil rangordning hello modellen.</span><span class="sxs-lookup"><span data-stu-id="53f0a-392">Mean percentile ranking of hello model.</span></span> <span data-ttu-id="53f0a-393">Lägre är bättre.</span><span class="sxs-lookup"><span data-stu-id="53f0a-393">Lower is better.</span></span> |
| <span data-ttu-id="53f0a-394">NumberOfDimensions</span><span class="sxs-lookup"><span data-stu-id="53f0a-394">NumberOfDimensions</span></span> |<span data-ttu-id="53f0a-395">Antalet dimensioner som används av hello matrisen factorization algoritmen.</span><span class="sxs-lookup"><span data-stu-id="53f0a-395">Number of dimensions used by hello matrix factorization algorithm.</span></span> |

<span data-ttu-id="53f0a-396">OData-XML</span><span class="sxs-lookup"><span data-stu-id="53f0a-396">OData XML</span></span>

    <feed xmlns:base="https://api.datamarket.azure.com/amla/recommendations/v3/GetModelInsight" xmlns:d="http://schemas.microsoft.com/ado/2007/08/dataservices" xmlns:m="http://schemas.microsoft.com/ado/2007/08/dataservices/metadata" xmlns="http://www.w3.org/2005/Atom">
    <title type="text" />
    <subtitle type="text">Get model insight statistics</subtitle>
    <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetModelInsight?modelId='6254b40d-0514-49cb-a298-b81256d2b3ca'&amp;apiVersion='1.0'</id>
    <rights type="text" />
    <updated>2014-10-27T14:27:11Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetModelInsight?modelId='6254b40d-0514-49cb-a298-b81256d2b3ca'&amp;apiVersion='1.0'" />
    <entry>
        <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetModelInsight?modelId='6254b40d-0514-49cb-a298-b81256d2b3ca'&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1</id>
        <title type="text">GetModelInsightStatisticsEntity</title>
        <updated>2014-10-27T14:27:11Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetModelInsight?modelId='6254b40d-0514-49cb-a298-b81256d2b3ca'&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1" />
        <content type="application/xml">
            <m:properties>
                <d:Key m:type="Edm.String">CatalogCoverage</d:Key>
                <d:Value m:type="Edm.String">1.000</d:Value>
            </m:properties>
        </content>
    </entry>
    <entry>
        <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetModelInsight?modelId='6254b40d-0514-49cb-a298-b81256d2b3ca'&amp;apiVersion='1.0'&amp;$skip=1&amp;$top=1</id>
        <title type="text">GetModelInsightStatisticsEntity</title>
        <updated>2014-10-27T14:27:11Z</updated>
        <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetModelInsight?modelId='6254b40d-0514-49cb-a298-b81256d2b3ca'&amp;apiVersion='1.0'&amp;$skip=1&amp;$top=1" />
        <content type="application/xml">
            <m:properties>
                <d:Key m:type="Edm.String">Mpr</d:Key>
                <d:Value m:type="Edm.String">0.367</d:Value>
            </m:properties>
        </content>
    </entry>
    <entry>
        <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetModelInsight?modelId='6254b40d-0514-49cb-a298-b81256d2b3ca'&amp;apiVersion='1.0'&amp;$skip=2&amp;$top=1</id>
        <title type="text">GetModelInsightStatisticsEntity</title>
        <updated>2014-10-27T14:27:11Z</updated>
        <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetModelInsight?modelId='6254b40d-0514-49cb-a298-b81256d2b3ca'&amp;apiVersion='1.0'&amp;$skip=2&amp;$top=1" />
        <content type="application/xml">
            <m:properties>
                <d:Key m:type="Edm.String">NumberOfDimensions</d:Key>
                <d:Value m:type="Edm.String">20</d:Value>
            </m:properties>
        </content>
    </entry>
    </feed>

### <a name="63----get-model-sample"></a><span data-ttu-id="53f0a-397">6.3.</span><span class="sxs-lookup"><span data-stu-id="53f0a-397">6.3.</span></span>    <span data-ttu-id="53f0a-398">Hämta modell-exempel</span><span class="sxs-lookup"><span data-stu-id="53f0a-398">Get Model Sample</span></span>
<span data-ttu-id="53f0a-399">Hämtar ett exempel på hello rekommendation modellen.</span><span class="sxs-lookup"><span data-stu-id="53f0a-399">Gets a sample of hello recommendation model.</span></span>

| <span data-ttu-id="53f0a-400">HTTP-metoden</span><span class="sxs-lookup"><span data-stu-id="53f0a-400">HTTP Method</span></span> | <span data-ttu-id="53f0a-401">URI: N</span><span class="sxs-lookup"><span data-stu-id="53f0a-401">URI</span></span> |
|:--- |:--- |
| <span data-ttu-id="53f0a-402">HÄMTA</span><span class="sxs-lookup"><span data-stu-id="53f0a-402">GET</span></span> |`<rootURI>/GetModelSample?modelId=%27<model_id>%27&apiVersion=%271.0%27`<br><span data-ttu-id="53f0a-403">Exempel:</span><span class="sxs-lookup"><span data-stu-id="53f0a-403">Example:</span></span><br>`<rootURI>/GetModelSample?modelId=%271cac7b76-def4-41f1-bc81-29b806adb1de%27&apiVersion=%271.0%27`<br><br><span data-ttu-id="53f0a-404">Med specifika build-ID:</span><span class="sxs-lookup"><span data-stu-id="53f0a-404">With specific build ID:</span></span><br>`<rootURI>/GetModelSample?modelId=%27<model_id>%27&buildId=%27<build_id>%27&apiVersion=%271.0%27`<br><span data-ttu-id="53f0a-405">Exempel:</span><span class="sxs-lookup"><span data-stu-id="53f0a-405">Example:</span></span><br>`<rootURI>/GetModelSample?modelId=%271cac7b76-def4-41f1-bc81-29b806adb1de%27&buildId=%271500068%27&apiVersion=%271.0%27` |

| <span data-ttu-id="53f0a-406">Parameternamn</span><span class="sxs-lookup"><span data-stu-id="53f0a-406">Parameter Name</span></span> | <span data-ttu-id="53f0a-407">Giltiga värden</span><span class="sxs-lookup"><span data-stu-id="53f0a-407">Valid Values</span></span> |
|:--- |:--- |
| <span data-ttu-id="53f0a-408">%{ModelID/</span><span class="sxs-lookup"><span data-stu-id="53f0a-408">modelId</span></span> |<span data-ttu-id="53f0a-409">Unik identifierare för hello modellen</span><span class="sxs-lookup"><span data-stu-id="53f0a-409">Unique identifier of hello model</span></span> |
| <span data-ttu-id="53f0a-410">apiVersion</span><span class="sxs-lookup"><span data-stu-id="53f0a-410">apiVersion</span></span> |<span data-ttu-id="53f0a-411">1.0</span><span class="sxs-lookup"><span data-stu-id="53f0a-411">1.0</span></span> |
|  | |
| <span data-ttu-id="53f0a-412">Begärandetext</span><span class="sxs-lookup"><span data-stu-id="53f0a-412">Request Body</span></span> |<span data-ttu-id="53f0a-413">INGEN</span><span class="sxs-lookup"><span data-stu-id="53f0a-413">NONE</span></span> |

<span data-ttu-id="53f0a-414">**Svaret**:</span><span class="sxs-lookup"><span data-stu-id="53f0a-414">**Response**:</span></span>

<span data-ttu-id="53f0a-415">HTTP-statuskod: 200</span><span class="sxs-lookup"><span data-stu-id="53f0a-415">HTTP Status code: 200</span></span>

<span data-ttu-id="53f0a-416">OData-XML</span><span class="sxs-lookup"><span data-stu-id="53f0a-416">OData XML</span></span>

<span data-ttu-id="53f0a-417">Svar returneras i raw-format:</span><span class="sxs-lookup"><span data-stu-id="53f0a-417">Response is returned in raw text format:</span></span>

<pre>
Level 1
---------------
655fc955-a5a3-4a26-9723-3090859cb27b, Prey: A Novel
    655fc955-a5a3-4a26-9723-3090859cb27b, Prey: A Novel Rating: 0.5215
    3f471802-f84f-44a0-99c8-6d2e7418eec1, Black Hawk Down: A Story of Modern War Rating: 0.5151
    07b10e28-9e7c-4032-90b7-10acab7f2460, Cryptonomicon Rating: 0.5148
    6afc18e4-8c2a-43d1-9021-57543d6b11d8, Imajica Rating: 0.5146
    e4cc5e69-3567-43ab-b00f-f0d8d0506870, Hit List Rating: 0.514
56b61441-0eed-46cc-a8f6-112775b81892, Life and Death in Shanghai
    56b61441-0eed-46cc-a8f6-112775b81892, Life and Death in Shanghai Rating: 0.5218
    53156702-cc0c-443d-b718-6fb74b2491d3, Son of \ Rating: 0.5212
    fb8cf7a6-8719-46ee-97d4-92f931d77a3a, Smoke and Mirrors: Short Fictions and Illusions Rating: 0.5188
    8f5fe006-79e4-4679-816b-950989d1db4b, A Place I've Never Been (Contemporary American Fiction) Rating: 0.5156
    d8db4583-cc0f-49ce-bc95-b7fa3491623f, Happiness: A Novel Rating: 0.5156
50471eec-9aeb-4900-84d7-21567ab18546, If hello Buddha Dated: A Handbook for Finding Love on a Spiritual Path
    cfe922a1-7ca0-4f8d-ad9d-b7cc87bfe0ef, Divine Secrets of hello Ya-Ya Sisterhood: A Novel Rating: 0.5266
    ff91a483-1ce5-4b37-a6fd-5ffcf21f8745, hello Poisonwood Bible: A Novel Rating: 0.5252
    973f8cbd-0846-4f6b-9d28-4dd0d7dc3a19, Pigs in Heaven Rating: 0.5244
    e2cbf7ad-0636-4117-8b30-298da6df7077, Animal Dreams Rating: 0.5227
    6c818fd3-5a09-417d-9ab4-7ffe090f0fef, Confessions of an Ugly Stepsister: A Novel Rating: 0.5222
5e97148f-defb-4d74-af2d-80f4763bf531, hello Deep End of hello Ocean (Oprah's Book Club)
    5e97148f-defb-4d74-af2d-80f4763bf531, hello Deep End of hello Ocean (Oprah's Book Club) Rating: 0.537
    5dcbac37-2946-4f2a-a0b3-bbe710f9409a, Up Island: A Novel Rating: 0.5277
    bc5b69db-733b-4346-adde-3927544258f7, Downtown Rating: 0.5275
    31fe5c63-3e5a-48d0-802b-d3b0f989a634, Have a Nice Day: A Tale of Blood and Sweatsocks Rating: 0.5252
    0adf981a-b65b-4c11-b36b-78aca2f948a2, hello Perfect Storm: A True Story of Men Against hello Sea Rating: 0.5238
68f97068-ae1a-4163-9e94-396b800b743d, Modoc: hello True Story of hello Greatest Elephant That Ever Lived
    68f97068-ae1a-4163-9e94-396b800b743d, Modoc: hello True Story of hello Greatest Elephant That Ever Lived Rating: 0.5379
    6724862e-e4e7-4022-9614-1468d8b902ff, Little House on hello Prairie Rating: 0.5345
    cdedb837-1620-496d-94c4-6ccfed888320, Little House in hello Big Woods Rating: 0.5325
    382164ba-406b-4187-b726-d7a54b9d790d, hello Tao of Pooh Rating: 0.5309
    6a068d6a-bb74-4ba3-b3f2-a956c4f9d1b5, On hello Banks of Plum Creek Rating: 0.5285
37ef8e74-e348-44e5-aabc-1d7f9efcb25b, Men Are from Mars Women Are from Venus: A Practical Guide for Improving Communication and Getting What You Want in Your Relationships
    37ef8e74-e348-44e5-aabc-1d7f9efcb25b, Men Are from Mars, Women Are from Venus: A Practical Guide for Improving Communication and Getting What You Want in Your Relationships Rating: 0.5397
    f2be16d4-5faf-4d32-ab83-7ba74d29261e, Politically Correct Bedtime Stories: Modern Tales for Our Life and Times Rating: 0.5207
    ef732c5c-334b-4d6b-ab82-7255eb7286d0, Honor Among Thieves Rating: 0.5195
    0b209b8c-7cdd-47fd-b940-05c7ff7c60fc, hello Giving Tree Rating: 0.5194
    883b360f-8b42-407f-b977-2f44ad840877, Scary Stories tooTell in hello Dark: Collected from American Folklore (Scary Stories) Rating: 0.5184
ff51b67e-fa8e-4c5e-8f4d-02a928de735d, Men at Work: hello Craft of Baseball
    d008dae9-c73a-40a1-9a9b-96d5cf546f36, hello Gulag Archipelago 1918-1956: An Experiment in Literary Investigation I-II Rating: 0.5416
    ff51b67e-fa8e-4c5e-8f4d-02a928de735d, Men at Work: hello Craft of Baseball Rating: 0.5403
    49dec30e-0adb-411a-b186-48eaabf6f8bc, Fatherland Rating: 0.5394
    cc7964fd-d30f-478e-a425-93ddbdf094ed, Magic hello Gathering: Arena Vol. 1 Rating: 0.5379
    8a1e9f36-97af-4614-bed9-24e3940a05f3, More Sniglets: Any Word That Doesn't Appear in hello Dictionary but Should Rating: 0.5377
12a6d988-be21-4a09-8143-9d5f4261ba16, A Dream of Eagles
    07b10e28-9e7c-4032-90b7-10acab7f2460, Cryptonomicon Rating: 0.5417
    e4cc5e69-3567-43ab-b00f-f0d8d0506870, Hit List Rating: 0.5416
    1f1a34c4-9781-49f5-a3cc-acec3ae3c71d, hello Family Rating: 0.5371
    56daeffe-7d48-43cd-8ef8-7dffd0c103d3, Kilo Class Rating: 0.5366
    b2fe511e-5cb9-4a56-b823-2801e63e6a96, Legal Tender Rating: 0.5366
df87525b-e435-4bd6-8701-4e60ad344e28, Finding Fish
    56d33036-dfda-46b9-8e2a-76cb03921bb0, hello X-Files: Ground Zero Rating: 0.5417
    0780cde8-6529-4e1d-b6c6-082c1b80e596, Twelve Red Herrings Rating: 0.5416
    df87525b-e435-4bd6-8701-4e60ad344e28, Finding Fish Rating: 0.5408
    400fe331-2c35-490c-adbc-b28b4b73d56c, Shall We Tell hello President? Rating: 0.5383
    f86ad7d0-5c03-42b3-aebf-13d44aec8b30, Shades of Grace Rating: 0.5358
de1f62a4-89e6-44d2-aaee-992a4bf093f1, hello Map That Changed hello World: William Smith and hello Birth of Modern Geology
    de1f62a4-89e6-44d2-aaee-992a4bf093f1, hello Map That Changed hello World: William Smith and hello Birth of Modern Geology Rating: 0.5422
    b303538f-e2c6-4a2c-b425-8d21e684fc3e, My Uncle Oswald Rating: 0.5385
    34b84627-48af-4a4c-96c4-b26fb3863f56, Midnight In hello Garden of Good and Evil Rating: 0.5379
    306cbaa7-b1a8-4142-9d55-e11b5018a7a8, hello Street Lawyer Rating: 0.5376
    e53b4baa-8c09-45c4-95c0-b6a26b98770b, Miss Smillas Feeling for Snow Rating: 0.5367

Level 2
---------------
352aaea1-6b12-454d-a3d5-46379d9e4eb2, hello Sinister Pig (Hillerman Tony)
    352aaea1-6b12-454d-a3d5-46379d9e4eb2, hello Sinister Pig (Hillerman Tony) Rating: 0.5425
    74c49398-bc10-4af5-a658-a996a1201254, Children of hello Storm (Peters Elizabeth) Rating: 0.5387
    9ba80080-196e-43fd-8025-391d963f77e7, hello Floating Girl Rating: 0.5372
    e68f81d5-7745-4cc7-b943-fedb8fcc2ced, Killer Smile (Scottoline Lisa) Rating: 0.5353
    b2fe511e-5cb9-4a56-b823-2801e63e6a96, Legal Tender Rating: 0.5332
c65c3995-abf7-4c7b-bb3c-8eb5aa9be7a5, Lake Wobegon days
    0adf981a-b65b-4c11-b36b-78aca2f948a2, hello Perfect Storm: A True Story of Men Against hello Sea Rating: 0.5433
    c65c3995-abf7-4c7b-bb3c-8eb5aa9be7a5, Lake Wobegon days Rating: 0.543
    a00ae6ad-4a7f-4211-9836-75ce8834eb11, Sniglets (Snig'lit: Any Word That Doesn't Appear in hello Dictionary But Should) Rating: 0.5327
    6f6e192e-0d64-49ca-9b63-f09413ea1ee6, Politically Correct Holiday Stories: For an Enlightened Yuletide Season Rating: 0.5307
    798051a8-147d-4d46-b0dc-e836325029e6, AGE OF INNOCENCE (MOVIE TIE-IN) Rating: 0.5301
73f3e25a-e996-4162-9ed8-ff3d34075650, O Pioneers! (Penguin Twentieth-Century Classics)
    cba8163f-6536-436b-8130-47b4a43c827f, Trust No One (hello Official Guide toohello X-Files Vol. 2) Rating: 0.5434
    5708e4cb-2492-49c0-94a8-cc413eec5d89, Small Gods (Discworld Novels (Paperback)) Rating: 0.5406
    73f3e25a-e996-4162-9ed8-ff3d34075650, O Pioneers! (Penguin Twentieth-Century Classics) Rating: 0.5403
    d885b0bd-ae4b-452d-bdf2-faa90197dbc9, hello Color of Magic Rating: 0.539
    b133a9c4-4784-4db3-b100-d0d6dffb94d2, hello Truth Is Out There (hello Official Guide toohello X-Files Vol. 1) Rating: 0.5367
271700a5-854a-4d5a-8409-6b57a5ee4de4, Fluke: Or I Know Why hello Winged Whale Sings
    271700a5-854a-4d5a-8409-6b57a5ee4de4, Fluke: Or I Know Why hello Winged Whale Sings Rating: 0.5445
    2de1c354-90ff-47c5-a0db-1bad7d88ef94, hello Salaryman's Wife (Children of Violence Series) Rating: 0.5329
    d279416e-19c0-43f8-9ec9-a585947879ca, Zen Attitude Rating: 0.5316
    c8f854d7-3de3-4b23-8217-f4f851670fd4, Revenge of hello Cootie Girls: A Robin Hudson Mystery (Robin Hudson Mysteries (Paperback)) Rating: 0.5305
    8ef4751c-7074-409e-a3ac-d49b222fc864, Where hello Wild Things Are Rating: 0.5289
9ad1b620-0a7b-4543-8673-66d4c3bcb2f1, Their Eyes Were Watching God
    9ad1b620-0a7b-4543-8673-66d4c3bcb2f1, Their Eyes Were Watching God Rating: 0.5446
    da45c4d5-aba1-413b-a9bd-50df98b1e1d2, hello Bean Trees Rating: 0.5389
    65ecbdd1-131c-40c3-a3d6-d86ca281377a, hello God of Small Things Rating: 0.5387
    c78743bf-7947-4a0c-8db7-8a3bfe69ba70, hello Stone Diaries Rating: 0.5355
    973f8cbd-0846-4f6b-9d28-4dd0d7dc3a19, Pigs in Heaven Rating: 0.5344
5f17d90a-2604-4fe8-8977-1a280b9098b1, One for hello Money (Stephanie Plum Novels (Paperback))
    5f17d90a-2604-4fe8-8977-1a280b9098b1, One for hello Money (Stephanie Plum Novels (Paperback)) Rating: 0.5446
    57169b2b-9a8a-486b-9aac-1ed98ce57168, Final Appeal Rating: 0.5332
    efcb1bc4-7278-4a8f-b491-befde02070d6, Moment of Truth Rating: 0.5329
    1efa91a2-993b-4c43-9f5c-3454fc12612d, Burn Factor Rating: 0.5309
    24c59962-458a-4ec8-b95d-d694e861919c, At Home in Mitford (hello Mitford Years) Rating: 0.5303
4fd48c46-1a20-4c57-bc7f-a02ef123dc52, As Nature Made Him: hello Boy Who Was Raised As a Girl
    4fd48c46-1a20-4c57-bc7f-a02ef123dc52, As Nature Made Him: hello Boy Who Was Raised As a Girl Rating: 0.5449
    cd5f2c03-20cb-43be-a1fb-3b4233e63222, Pigs in Heaven Rating: 0.5329
    19985fdb-d07a-4a25-ae4a-97b9cb61e5d1, Love in hello Time of Cholera (Penguin Great Books of hello 20th Century) Rating: 0.5267
    15689d09-c711-4844-84d8-130a90237b26, Bel Canto Rating: 0.5245
    ff91a483-1ce5-4b37-a6fd-5ffcf21f8745, hello Poisonwood Bible: A Novel Rating: 0.5235
98df28ec-41e7-4fca-b77f-8b0d3109085d, Star Trek Memories
    f874b5a3-5d40-4436-94ff-0fa1c090ddf5, hello Sun Also Rises (A Scribner classic) Rating: 0.5451
    98df28ec-41e7-4fca-b77f-8b0d3109085d, Star Trek Memories Rating: 0.5442
    0ce0014a-9a48-4013-a08a-7f2c11877930, H.M.S. Unseen Rating: 0.5421
    15316ca6-1e38-425f-893d-691944a47000, More Scary Stories tooTell In hello Dark Rating: 0.5409
    329d5682-3dc3-4206-8aa2-eef4b1032258, Letters from hello Earth Rating: 0.54
5b9445d5-c072-419c-8d49-6f669bb1b0a9, Daughter of Fortune: A Novel (Oprah's Book Club (Hardcover))
    5b9445d5-c072-419c-8d49-6f669bb1b0a9, Daughter of Fortune: A Novel (Oprah's Book Club (Hardcover)) Rating: 0.5462
    ff91a483-1ce5-4b37-a6fd-5ffcf21f8745, hello Poisonwood Bible: A Novel Rating: 0.5372
    604eb3bd-6026-4f51-bffd-9fb54f180400, Family Pictures: A Novel Rating: 0.5341
    8d06d01d-31cd-4678-b6b1-140a67987ce9, Songs in Ordinary Time (Oprah's Book Club (Paperback)) Rating: 0.5334
    da45c4d5-aba1-413b-a9bd-50df98b1e1d2, hello Bean Trees Rating: 0.5319
d5358189-d70f-4e35-8add-34b83b4942b3, Pigs in Heaven
    d5358189-d70f-4e35-8add-34b83b4942b3, Pigs in Heaven Rating: 0.5491
    ff91a483-1ce5-4b37-a6fd-5ffcf21f8745, hello Poisonwood Bible: A Novel Rating: 0.5401
    c78743bf-7947-4a0c-8db7-8a3bfe69ba70, hello Stone Diaries Rating: 0.5393
    8d06d01d-31cd-4678-b6b1-140a67987ce9, Songs in Ordinary Time (Oprah's Book Club (Paperback)) Rating: 0.5382
    973f8cbd-0846-4f6b-9d28-4dd0d7dc3a19, Pigs in Heaven Rating: 0.5367

</pre>


## <a name="7-model-business-rules"></a><span data-ttu-id="53f0a-418">7. Affärsregler för modellen</span><span class="sxs-lookup"><span data-stu-id="53f0a-418">7. Model Business Rules</span></span>
<span data-ttu-id="53f0a-419">Dessa är hello typer av regler som stöds:</span><span class="sxs-lookup"><span data-stu-id="53f0a-419">These are hello types of rules supported:</span></span>

* <span data-ttu-id="53f0a-420"><strong>BlockList</strong> -BlockList kan du tooprovide en lista med objekt som du inte vill tooreturn i hello rekommendation resultat.</span><span class="sxs-lookup"><span data-stu-id="53f0a-420"><strong>BlockList</strong> - BlockList enables you tooprovide a list of items that you do not want tooreturn in hello recommendation results.</span></span> 
* <span data-ttu-id="53f0a-421"><strong>FeatureBlockList</strong> -funktionen BlockList aktiverar du tooblock objekt baserat på hello värden dess funktioner.</span><span class="sxs-lookup"><span data-stu-id="53f0a-421"><strong>FeatureBlockList</strong> - Feature BlockList enables you tooblock items based on hello values of its features.</span></span>

<span data-ttu-id="53f0a-422">*Skicka inte fler än 1000 objekt i en enda blocklist regel eller samtalet kan timeout. Om du behöver tooblock fler än 1000 objekt kan göra du flera blocklist samtal.*</span><span class="sxs-lookup"><span data-stu-id="53f0a-422">*Do not send more than 1000 items in a single blocklist rule or your call may timeout. If you need tooblock more than 1000 items, you can make several blocklist calls.*</span></span>

* <span data-ttu-id="53f0a-423"><strong>Upsale</strong> -Upsale kan du tooenforce objekt tooreturn i hello rekommendation resultat.</span><span class="sxs-lookup"><span data-stu-id="53f0a-423"><strong>Upsale</strong> - Upsale enables you tooenforce items tooreturn in hello recommendation results.</span></span>
* <span data-ttu-id="53f0a-424"><strong>Godkända</strong> -lista för tillåten aktiverar du tooonly föreslår rekommendationerna från en lista med objekt.</span><span class="sxs-lookup"><span data-stu-id="53f0a-424"><strong>WhiteList</strong> - White List enables you tooonly suggest recommendations from a list of items.</span></span>
* <span data-ttu-id="53f0a-425"><strong>FeatureWhiteList</strong> -funktionen vitlista kan du tooonly rekommenderar objekt som innehåller värden för specifika funktioner.</span><span class="sxs-lookup"><span data-stu-id="53f0a-425"><strong>FeatureWhiteList</strong> - Feature White List enables you tooonly recommend items that have specific feature values.</span></span>
* <span data-ttu-id="53f0a-426"><strong>PerSeedBlockList</strong> - Per Seed Blockeringslista aktiverar tooprovide per objektet en lista med objekt som inte kan returneras som resultat av rekommendation.</span><span class="sxs-lookup"><span data-stu-id="53f0a-426"><strong>PerSeedBlockList</strong> - Per Seed Block List enables you tooprovide per item a list of items that cannot be returned as recommendation results.</span></span>

### <a name="71----get-model-rules"></a><span data-ttu-id="53f0a-427">7.1.</span><span class="sxs-lookup"><span data-stu-id="53f0a-427">7.1.</span></span>    <span data-ttu-id="53f0a-428">Hämta modellen regler</span><span class="sxs-lookup"><span data-stu-id="53f0a-428">Get Model Rules</span></span>
| <span data-ttu-id="53f0a-429">HTTP-metoden</span><span class="sxs-lookup"><span data-stu-id="53f0a-429">HTTP Method</span></span> | <span data-ttu-id="53f0a-430">URI: N</span><span class="sxs-lookup"><span data-stu-id="53f0a-430">URI</span></span> |
|:--- |:--- |
| <span data-ttu-id="53f0a-431">HÄMTA</span><span class="sxs-lookup"><span data-stu-id="53f0a-431">GET</span></span> |`<rootURI>/GetModelRules?modelId=%27<model_id>%27&apiVersion=%271.0%27`<br><span data-ttu-id="53f0a-432">Exempel:</span><span class="sxs-lookup"><span data-stu-id="53f0a-432">Example:</span></span><br>`<rootURI>/GetModelRules?modelId=%271cac7b76-def4-41f1-bc81-29b806adb1de%27&apiVersion=%271.0%27` |

| <span data-ttu-id="53f0a-433">Parameternamn</span><span class="sxs-lookup"><span data-stu-id="53f0a-433">Parameter Name</span></span> | <span data-ttu-id="53f0a-434">Giltiga värden</span><span class="sxs-lookup"><span data-stu-id="53f0a-434">Valid Values</span></span> |
|:--- |:--- |
| <span data-ttu-id="53f0a-435">%{ModelID/</span><span class="sxs-lookup"><span data-stu-id="53f0a-435">modelId</span></span> |<span data-ttu-id="53f0a-436">Unik identifierare för hello modellen</span><span class="sxs-lookup"><span data-stu-id="53f0a-436">Unique identifier of hello model</span></span> |
| <span data-ttu-id="53f0a-437">apiVersion</span><span class="sxs-lookup"><span data-stu-id="53f0a-437">apiVersion</span></span> |<span data-ttu-id="53f0a-438">1.0</span><span class="sxs-lookup"><span data-stu-id="53f0a-438">1.0</span></span> |
|  | |
| <span data-ttu-id="53f0a-439">Begärandetext</span><span class="sxs-lookup"><span data-stu-id="53f0a-439">Request Body</span></span> |<span data-ttu-id="53f0a-440">INGEN</span><span class="sxs-lookup"><span data-stu-id="53f0a-440">NONE</span></span> |

<span data-ttu-id="53f0a-441">**Svaret**:</span><span class="sxs-lookup"><span data-stu-id="53f0a-441">**Response**:</span></span>

<span data-ttu-id="53f0a-442">HTTP-statuskod: 200</span><span class="sxs-lookup"><span data-stu-id="53f0a-442">HTTP Status code: 200</span></span>

* <span data-ttu-id="53f0a-443">`feed/entry/content/properties/Id`-Unik identifierare för den här regeln.</span><span class="sxs-lookup"><span data-stu-id="53f0a-443">`feed/entry/content/properties/Id` - Unique identifier of this rule.</span></span>
* <span data-ttu-id="53f0a-444">`feed/entry/content/properties/Type`-Typ av hello regel.</span><span class="sxs-lookup"><span data-stu-id="53f0a-444">`feed/entry/content/properties/Type` - Type of hello rule.</span></span>
* <span data-ttu-id="53f0a-445">`feed/entry/content/properties/Parameter`-Regel parameter.</span><span class="sxs-lookup"><span data-stu-id="53f0a-445">`feed/entry/content/properties/Parameter` - Rule parameter.</span></span>

<span data-ttu-id="53f0a-446">OData-XML</span><span class="sxs-lookup"><span data-stu-id="53f0a-446">OData XML</span></span>

    <feed xmlns:base="https://api.datamarket.azure.com/amla/recommendations/v3/GetModelRules" xmlns:d="http://schemas.microsoft.com/ado/2007/08/dataservices" xmlns:m="http://schemas.microsoft.com/ado/2007/08/dataservices/metadata" xmlns="http://www.w3.org/2005/Atom">
    <title type="text" />
    <subtitle type="text">Get a list of rules for a model</subtitle>
    <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetModelRules?modelId='5e824626-50d3-469d-a824-564d38453103'&amp;apiVersion='1.0'</id>
    <rights type="text" />
    <updated>2014-11-05T12:58:57Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetModelRules?modelId='5e824626-50d3-469d-a824-564d38453103'&amp;apiVersion='1.0'" />
    <entry>
        <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetModelRules?modelId='5e824626-50d3-469d-a824-564d38453103'&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1</id>
        <title type="text">GetAListOfRulesForAModelEntity</title>
        <updated>2014-11-05T12:58:57Z</updated>
        <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetModelRules?modelId='5e824626-50d3-469d-a824-564d38453103'&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1" />
        <content type="application/xml">
            <m:properties>
                <d:Id m:type="Edm.String">1000043</d:Id>
                <d:Type m:type="Edm.String">BlockList</d:Type>
                <d:Parameters m:type="Edm.String">{"ItemsToExclude":["1000"]}</d:Parameters>
            </m:properties>
        </content>
    </entry>
    <entry>
        <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetModelRules?modelId='5e824626-50d3-469d-a824-564d38453103'&amp;apiVersion='1.0'&amp;$skip=1&amp;$top=1</id>
        <title type="text">GetAListOfRulesForAModelEntity</title>
        <updated>2014-11-05T12:58:57Z</updated>
        <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetModelRules?modelId='5e824626-50d3-469d-a824-564d38453103'&amp;apiVersion='1.0'&amp;$skip=1&amp;$top=1" />
        <content type="application/xml">
            <m:properties>
                <d:Id m:type="Edm.String">1000044</d:Id>
                <d:Type m:type="Edm.String">BlockList</d:Type>
                <d:Parameters m:type="Edm.String">{"ItemsToExclude":["1001"]}</d:Parameters>
            </m:properties>
        </content>
    </entry>
    </feed>

### <a name="72----add-rule"></a><span data-ttu-id="53f0a-447">7.2.</span><span class="sxs-lookup"><span data-stu-id="53f0a-447">7.2.</span></span>    <span data-ttu-id="53f0a-448">Lägg till regel</span><span class="sxs-lookup"><span data-stu-id="53f0a-448">Add Rule</span></span>
| <span data-ttu-id="53f0a-449">HTTP-metoden</span><span class="sxs-lookup"><span data-stu-id="53f0a-449">HTTP Method</span></span> | <span data-ttu-id="53f0a-450">URI: N</span><span class="sxs-lookup"><span data-stu-id="53f0a-450">URI</span></span> |
|:--- |:--- |
| <span data-ttu-id="53f0a-451">POST</span><span class="sxs-lookup"><span data-stu-id="53f0a-451">POST</span></span> |`<rootURI>/AddRule?apiVersion=%271.0%27` |
| <span data-ttu-id="53f0a-452">HUVUDET</span><span class="sxs-lookup"><span data-stu-id="53f0a-452">HEADER</span></span> |`"Content-Type", "text/xml"` |

| <span data-ttu-id="53f0a-453">Parameternamn</span><span class="sxs-lookup"><span data-stu-id="53f0a-453">Parameter Name</span></span> | <span data-ttu-id="53f0a-454">Giltiga värden</span><span class="sxs-lookup"><span data-stu-id="53f0a-454">Valid Values</span></span> |
|:--- |:--- |
| <span data-ttu-id="53f0a-455">apiVersion</span><span class="sxs-lookup"><span data-stu-id="53f0a-455">apiVersion</span></span> |<span data-ttu-id="53f0a-456">1.0</span><span class="sxs-lookup"><span data-stu-id="53f0a-456">1.0</span></span> |
|  | |
| <span data-ttu-id="53f0a-457">Begärandetext</span><span class="sxs-lookup"><span data-stu-id="53f0a-457">Request Body</span></span> | |

<span data-ttu-id="53f0a-458"><ins>När det är att tillhandahålla objekt-ID: N för affärsregler, att att toouse Hej externa hello objekt-Id (hello samma Id som du använde i hello katalogfil)</ins></span><span class="sxs-lookup"><span data-stu-id="53f0a-458"><ins>Whenever providing Item Ids for business rules, make sure toouse hello External Id of hello item (hello same Id that you used in hello catalog file)</ins></span></span><br><span data-ttu-id="53f0a-459">
<ins>tooadd BlockList regel:</ins></span><span class="sxs-lookup"><span data-stu-id="53f0a-459">
<ins>tooadd a BlockList rule:</ins></span></span><br>`<ApiFilter xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"><ModelId>24024f7e-b45c-419e-bfa2-dfd947e0d253</ModelId><Type>BlockList</Type><Value>{"ItemsToExclude":["2406E770-769C-4189-89DE-1C9283F93A96","3906E110-769C-4189-89DE-1C9283F98888"]}</Value></ApiFilter>`<br><br><span data-ttu-id="53f0a-460"><ins>
<ins>tooadd FeatureBlockList regel:</ins></span><span class="sxs-lookup"><span data-stu-id="53f0a-460"><ins>
<ins>tooadd a FeatureBlockList rule:</ins></span></span><br>
<br>
`<ApiFilter xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"><ModelId>24024f7e-b45c-419e-bfa2-dfd947e0d253</ModelId><Type>FeatureBlockList</Type><Value>{"Name":"Movie_category","Values":["Adult","Drama"]}</Value></ApiFilter>`<br><br><span data-ttu-id="53f0a-461"><ins>tooadd en Upsale regel:</ins></span><span class="sxs-lookup"><span data-stu-id="53f0a-461"><ins> tooadd an Upsale rule:</ins></span></span><br>`<ApiFilter xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"><ModelId>24024f7e-b45c-419e-bfa2-dfd947e0d253</ModelId><Type>Upsale</Type><Value>{"ItemsToUpsale":["2406E770-769C-4189-89DE-1C9283F93A96"],"NumberOfItemsToUpsale":5}</Value></ApiFilter>`<br><br><span data-ttu-id="53f0a-462">
<ins>tooadd en godkänd lista regel:</ins></span><span class="sxs-lookup"><span data-stu-id="53f0a-462">
<ins>tooadd a WhiteList rule:</ins></span></span><br>
`<ApiFilter xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"><ModelId>24024f7e-b45c-419e-bfa2-dfd947e0d253</ModelId><Type>WhiteList</Type><Value>{"ItemsToInclude":["2406E770-769C-4189-89DE-1C9283F93A96","1116E770-769C-4189-89DE-1C9283F88888"]}</Value></ApiFilter>`<br><br><span data-ttu-id="53f0a-463"><ins>
<ins>tooadd FeatureWhiteList regel:</ins></span><span class="sxs-lookup"><span data-stu-id="53f0a-463"><ins>
<ins>tooadd a FeatureWhiteList rule:</ins></span></span><br>
<br>
`<ApiFilter xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"><ModelId>24024f7e-b45c-419e-bfa2-dfd947e0d253</ModelId><Type>FeatureWhiteList</Type><Value>{"Name":"Movie_rating","Values":["PG13"]}</Value></ApiFilter>`<br><br><span data-ttu-id="53f0a-464"><ins>tooadd PerSeedBlockList regel:</ins></span><span class="sxs-lookup"><span data-stu-id="53f0a-464"><ins> tooadd a PerSeedBlockList rule:</ins></span></span><br>`<ApiFilter xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"><ModelId>24024f7e-b45c-419e-bfa2-dfd947e0d253</ModelId><Type>PerSeedBlockList</Type><Value>{"SeedItems":["9949"],"ItemsToExclude":["9862","8158","8244"]}</Value></ApiFilter>`|

<span data-ttu-id="53f0a-465">**Svaret**:</span><span class="sxs-lookup"><span data-stu-id="53f0a-465">**Response**:</span></span>

<span data-ttu-id="53f0a-466">HTTP-statuskod: 200</span><span class="sxs-lookup"><span data-stu-id="53f0a-466">HTTP Status code: 200</span></span>

<span data-ttu-id="53f0a-467">hello API returnerar hello nyligen skapade regeln med information om den.</span><span class="sxs-lookup"><span data-stu-id="53f0a-467">hello API returns hello newly created rule with its details.</span></span> <span data-ttu-id="53f0a-468">hello regler egenskapen kan hämtas från hello följande sökvägar:</span><span class="sxs-lookup"><span data-stu-id="53f0a-468">hello rules property can be retrieved from hello following paths:</span></span>

* <span data-ttu-id="53f0a-469">`feed/entry/content/properties/Id`-Unik identifierare för den här regeln.</span><span class="sxs-lookup"><span data-stu-id="53f0a-469">`feed/entry/content/properties/Id` - Unique identifier of this rule.</span></span>
* <span data-ttu-id="53f0a-470">`feed/entry/content/properties/Type`-Typ av regel som hello: BlockList eller Upsale.</span><span class="sxs-lookup"><span data-stu-id="53f0a-470">`feed/entry/content/properties/Type` - Type of hello rule: BlockList or Upsale.</span></span>
* <span data-ttu-id="53f0a-471">`feed/entry/content/properties/Parameter`-Regel parameter.</span><span class="sxs-lookup"><span data-stu-id="53f0a-471">`feed/entry/content/properties/Parameter` - Rule parameter.</span></span>

<span data-ttu-id="53f0a-472">OData-XML</span><span class="sxs-lookup"><span data-stu-id="53f0a-472">OData XML</span></span>

    <feed xmlns:base="https://api.datamarket.azure.com/amla/recommendations/v3/AddRule" xmlns:d="http://schemas.microsoft.com/ado/2007/08/dataservices" xmlns:m="http://schemas.microsoft.com/ado/2007/08/dataservices/metadata" xmlns="http://www.w3.org/2005/Atom">
    <title type="text" />
    <subtitle type="text">Add A Rule</subtitle>
    <id>https://api.datamarket.azure.com/amla/recommendations/v3/AddRule?apiVersion='1.0'</id>
    <rights type="text" />
    <updated>2014-11-05T11:13:28Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/AddRule?apiVersion='1.0'" />
    <entry>
        <id>https://api.datamarket.azure.com/amla/recommendations/v3/AddRule?apiVersion='1.0'&amp;$skip=0&amp;$top=1</id>
        <title type="text">AddARuleEntity</title>
        <updated>2014-11-05T11:13:28Z</updated>
        <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/AddRule?apiVersion='1.0'&amp;$skip=0&amp;$top=1" />
        <content type="application/xml">
            <m:properties>
                <d:Id m:type="Edm.String">1000041</d:Id>
                <d:Type m:type="Edm.String">BlockList</d:Type>
                <d:Parameters m:type="Edm.String">{"ItemsToExclude":["1002"]}</d:Parameters>
            </m:properties>
        </content>
    </entry>
    </feed>

### <a name="73----delete-rule"></a><span data-ttu-id="53f0a-473">7.3.</span><span class="sxs-lookup"><span data-stu-id="53f0a-473">7.3.</span></span>    <span data-ttu-id="53f0a-474">Ta bort regeln</span><span class="sxs-lookup"><span data-stu-id="53f0a-474">Delete Rule</span></span>
| <span data-ttu-id="53f0a-475">HTTP-metoden</span><span class="sxs-lookup"><span data-stu-id="53f0a-475">HTTP Method</span></span> | <span data-ttu-id="53f0a-476">URI: N</span><span class="sxs-lookup"><span data-stu-id="53f0a-476">URI</span></span> |
|:--- |:--- |
| <span data-ttu-id="53f0a-477">TA BORT</span><span class="sxs-lookup"><span data-stu-id="53f0a-477">DELETE</span></span> |`<rootURI>/DeleteRule?modelId=%27<model_id>%27&filterId=%27<filter_Id>%27&apiVersion=%271.0%27`<br><br><span data-ttu-id="53f0a-478">Exempel:</span><span class="sxs-lookup"><span data-stu-id="53f0a-478">Example:</span></span><br>`DeleteRule?modelId=%2724024f7e-b45c-419e-bfa2-dfd947e0d253%27&filterId=%271000011%27&apiVersion=%271.0%27` |

| <span data-ttu-id="53f0a-479">Parameternamn</span><span class="sxs-lookup"><span data-stu-id="53f0a-479">Parameter Name</span></span> | <span data-ttu-id="53f0a-480">Giltiga värden</span><span class="sxs-lookup"><span data-stu-id="53f0a-480">Valid Values</span></span> |
|:--- |:--- |
| <span data-ttu-id="53f0a-481">%{ModelID/</span><span class="sxs-lookup"><span data-stu-id="53f0a-481">modelId</span></span> |<span data-ttu-id="53f0a-482">Unik identifierare för hello modellen</span><span class="sxs-lookup"><span data-stu-id="53f0a-482">Unique identifier of hello model</span></span> |
| <span data-ttu-id="53f0a-483">filterId</span><span class="sxs-lookup"><span data-stu-id="53f0a-483">filterId</span></span> |<span data-ttu-id="53f0a-484">Unik identifierare för hello filter</span><span class="sxs-lookup"><span data-stu-id="53f0a-484">Unique identifier of hello filter</span></span> |
| <span data-ttu-id="53f0a-485">apiVersion</span><span class="sxs-lookup"><span data-stu-id="53f0a-485">apiVersion</span></span> |<span data-ttu-id="53f0a-486">1.0</span><span class="sxs-lookup"><span data-stu-id="53f0a-486">1.0</span></span> |
|  | |
| <span data-ttu-id="53f0a-487">Begärandetext</span><span class="sxs-lookup"><span data-stu-id="53f0a-487">Request Body</span></span> |<span data-ttu-id="53f0a-488">INGEN</span><span class="sxs-lookup"><span data-stu-id="53f0a-488">NONE</span></span> |

<span data-ttu-id="53f0a-489">**Svaret**:</span><span class="sxs-lookup"><span data-stu-id="53f0a-489">**Response**:</span></span>

<span data-ttu-id="53f0a-490">HTTP-statuskod: 200</span><span class="sxs-lookup"><span data-stu-id="53f0a-490">HTTP Status code: 200</span></span>

### <a name="74----delete-all-rules"></a><span data-ttu-id="53f0a-491">7.4.</span><span class="sxs-lookup"><span data-stu-id="53f0a-491">7.4.</span></span>    <span data-ttu-id="53f0a-492">Ta bort alla regler</span><span class="sxs-lookup"><span data-stu-id="53f0a-492">Delete All Rules</span></span>
| <span data-ttu-id="53f0a-493">HTTP-metoden</span><span class="sxs-lookup"><span data-stu-id="53f0a-493">HTTP Method</span></span> | <span data-ttu-id="53f0a-494">URI: N</span><span class="sxs-lookup"><span data-stu-id="53f0a-494">URI</span></span> |
|:--- |:--- |
| <span data-ttu-id="53f0a-495">TA BORT</span><span class="sxs-lookup"><span data-stu-id="53f0a-495">DELETE</span></span> |`<rootURI>/DeleteAllRules?modelId=%27<model_id>%27&apiVersion=%271.0%27`<br><br><span data-ttu-id="53f0a-496">Exempel:</span><span class="sxs-lookup"><span data-stu-id="53f0a-496">Example:</span></span><br>`DeleteAllRules?modelId=%2724024f7e-b45c-419e-bfa2-dfd947e0d253%27&apiVersion=%271.0%27` |

| <span data-ttu-id="53f0a-497">Parameternamn</span><span class="sxs-lookup"><span data-stu-id="53f0a-497">Parameter Name</span></span> | <span data-ttu-id="53f0a-498">Giltiga värden</span><span class="sxs-lookup"><span data-stu-id="53f0a-498">Valid Values</span></span> |
|:--- |:--- |
| <span data-ttu-id="53f0a-499">%{ModelID/</span><span class="sxs-lookup"><span data-stu-id="53f0a-499">modelId</span></span> |<span data-ttu-id="53f0a-500">Unik identifierare för hello modellen</span><span class="sxs-lookup"><span data-stu-id="53f0a-500">Unique identifier of hello model</span></span> |
| <span data-ttu-id="53f0a-501">apiVersion</span><span class="sxs-lookup"><span data-stu-id="53f0a-501">apiVersion</span></span> |<span data-ttu-id="53f0a-502">1.0</span><span class="sxs-lookup"><span data-stu-id="53f0a-502">1.0</span></span> |
|  | |
| <span data-ttu-id="53f0a-503">Begärandetext</span><span class="sxs-lookup"><span data-stu-id="53f0a-503">Request Body</span></span> |<span data-ttu-id="53f0a-504">INGEN</span><span class="sxs-lookup"><span data-stu-id="53f0a-504">NONE</span></span> |

<span data-ttu-id="53f0a-505">**Svaret**:</span><span class="sxs-lookup"><span data-stu-id="53f0a-505">**Response**:</span></span>

<span data-ttu-id="53f0a-506">HTTP-statuskod: 200</span><span class="sxs-lookup"><span data-stu-id="53f0a-506">HTTP Status code: 200</span></span>

## <a name="8-catalog"></a><span data-ttu-id="53f0a-507">8. Katalogen</span><span class="sxs-lookup"><span data-stu-id="53f0a-507">8. Catalog</span></span>
### <a name="81----import-catalog-data"></a><span data-ttu-id="53f0a-508">8.1.</span><span class="sxs-lookup"><span data-stu-id="53f0a-508">8.1.</span></span>    <span data-ttu-id="53f0a-509">Importera katalogdata</span><span class="sxs-lookup"><span data-stu-id="53f0a-509">Import Catalog Data</span></span>
<span data-ttu-id="53f0a-510">Om du överför flera katalog filer toohello samma modell med flera anrop infogas vi bara hello nya katalogobjekt.</span><span class="sxs-lookup"><span data-stu-id="53f0a-510">If you upload several catalog files toohello same model with several calls, we will insert only hello new catalog items.</span></span> <span data-ttu-id="53f0a-511">Befintliga objekt förblir med hello ursprungliga värden.</span><span class="sxs-lookup"><span data-stu-id="53f0a-511">Existing items will remain with hello original values.</span></span> <span data-ttu-id="53f0a-512">Du kan inte uppdatera katalogdata med den här metoden.</span><span class="sxs-lookup"><span data-stu-id="53f0a-512">You cannot update catalog data by using this method.</span></span>

<span data-ttu-id="53f0a-513">hello katalogdata bör följa hello följande format:</span><span class="sxs-lookup"><span data-stu-id="53f0a-513">hello catalog data should follow hello following format:</span></span>

* <span data-ttu-id="53f0a-514">Utan funktioner-`<Item Id>,<Item Name>,<Item Category>[,<Description>]`</span><span class="sxs-lookup"><span data-stu-id="53f0a-514">Without features - `<Item Id>,<Item Name>,<Item Category>[,<Description>]`</span></span>
* <span data-ttu-id="53f0a-515">Med funktioner-`<Item Id>,<Item Name>,<Item Category>,[<Description>],<Features list>`</span><span class="sxs-lookup"><span data-stu-id="53f0a-515">With features - `<Item Id>,<Item Name>,<Item Category>,[<Description>],<Features list>`</span></span>

<span data-ttu-id="53f0a-516">Obs: hello maximal filstorlek är 200MB.</span><span class="sxs-lookup"><span data-stu-id="53f0a-516">Note: hello maximum file size is 200MB.</span></span>

<span data-ttu-id="53f0a-517">** Formatera information **</span><span class="sxs-lookup"><span data-stu-id="53f0a-517">** Format details **</span></span>

| <span data-ttu-id="53f0a-518">Namn</span><span class="sxs-lookup"><span data-stu-id="53f0a-518">Name</span></span> | <span data-ttu-id="53f0a-519">Obligatorisk</span><span class="sxs-lookup"><span data-stu-id="53f0a-519">Mandatory</span></span> | <span data-ttu-id="53f0a-520">Typ</span><span class="sxs-lookup"><span data-stu-id="53f0a-520">Type</span></span> | <span data-ttu-id="53f0a-521">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="53f0a-521">Description</span></span> |
|:--- |:--- |:--- |:--- |
| <span data-ttu-id="53f0a-522">Objekt-Id</span><span class="sxs-lookup"><span data-stu-id="53f0a-522">Item Id</span></span> |<span data-ttu-id="53f0a-523">Ja</span><span class="sxs-lookup"><span data-stu-id="53f0a-523">Yes</span></span> |<span data-ttu-id="53f0a-524">[A-z], [a-z], [0-9] [_] &#40; Understreck &#41; [-] &#40; Dash &#41;</span><span class="sxs-lookup"><span data-stu-id="53f0a-524">[A-z], [a-z], [0-9], [_] &#40;Underscore&#41;, [-] &#40;Dash&#41;</span></span><br> <span data-ttu-id="53f0a-525">Maxlängd: 50</span><span class="sxs-lookup"><span data-stu-id="53f0a-525">Max length: 50</span></span> |<span data-ttu-id="53f0a-526">Unik identifierare för ett objekt.</span><span class="sxs-lookup"><span data-stu-id="53f0a-526">Unique identifier of an item.</span></span> |
| <span data-ttu-id="53f0a-527">Objektnamnet</span><span class="sxs-lookup"><span data-stu-id="53f0a-527">Item Name</span></span> |<span data-ttu-id="53f0a-528">Ja</span><span class="sxs-lookup"><span data-stu-id="53f0a-528">Yes</span></span> |<span data-ttu-id="53f0a-529">Alfanumeriska tecken</span><span class="sxs-lookup"><span data-stu-id="53f0a-529">Any alphanumeric characters</span></span><br> <span data-ttu-id="53f0a-530">Maxlängd: 255</span><span class="sxs-lookup"><span data-stu-id="53f0a-530">Max length: 255</span></span> |<span data-ttu-id="53f0a-531">Objektnamnet.</span><span class="sxs-lookup"><span data-stu-id="53f0a-531">Item name.</span></span> |
| <span data-ttu-id="53f0a-532">Objektet kategori</span><span class="sxs-lookup"><span data-stu-id="53f0a-532">Item Category</span></span> |<span data-ttu-id="53f0a-533">Ja</span><span class="sxs-lookup"><span data-stu-id="53f0a-533">Yes</span></span> |<span data-ttu-id="53f0a-534">Alfanumeriska tecken</span><span class="sxs-lookup"><span data-stu-id="53f0a-534">Any alphanumeric characters</span></span> <br> <span data-ttu-id="53f0a-535">Maxlängd: 255</span><span class="sxs-lookup"><span data-stu-id="53f0a-535">Max length: 255</span></span> |<span data-ttu-id="53f0a-536">Kategori toowhich artikeln tillhör (t.ex. matlagning böcker, Drama...); kan vara tom.</span><span class="sxs-lookup"><span data-stu-id="53f0a-536">Category toowhich this item belongs (e.g. Cooking Books, Drama…); can be empty.</span></span> |
| <span data-ttu-id="53f0a-537">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="53f0a-537">Description</span></span> |<span data-ttu-id="53f0a-538">Nej, såvida inte-funktioner finns (men kan vara tom)</span><span class="sxs-lookup"><span data-stu-id="53f0a-538">No, unless features are present (but can be empty)</span></span> |<span data-ttu-id="53f0a-539">Alfanumeriska tecken</span><span class="sxs-lookup"><span data-stu-id="53f0a-539">Any alphanumeric characters</span></span> <br> <span data-ttu-id="53f0a-540">Maxlängd: 4000</span><span class="sxs-lookup"><span data-stu-id="53f0a-540">Max length: 4000</span></span> |<span data-ttu-id="53f0a-541">Beskrivning av det här objektet.</span><span class="sxs-lookup"><span data-stu-id="53f0a-541">Description of this item.</span></span> |
| <span data-ttu-id="53f0a-542">Listan över funktioner</span><span class="sxs-lookup"><span data-stu-id="53f0a-542">Features list</span></span> |<span data-ttu-id="53f0a-543">Nej</span><span class="sxs-lookup"><span data-stu-id="53f0a-543">No</span></span> |<span data-ttu-id="53f0a-544">Alfanumeriska tecken</span><span class="sxs-lookup"><span data-stu-id="53f0a-544">Any alphanumeric characters</span></span> <br> <span data-ttu-id="53f0a-545">Maxlängd: 4000; Högsta antal funktioner: 20</span><span class="sxs-lookup"><span data-stu-id="53f0a-545">Max length: 4000; Max number of features:20</span></span> |<span data-ttu-id="53f0a-546">Kommaavgränsad lista över funktionens namn = värde för funktionen som kan använda tooenhance modellen rekommendation. Se [avancerade alternativ](#2-advanced-topics) avsnitt.</span><span class="sxs-lookup"><span data-stu-id="53f0a-546">Comma-separated list of feature name=feature value that can be used tooenhance model recommendation; see [Advanced topics](#2-advanced-topics) section.</span></span> |

| <span data-ttu-id="53f0a-547">HTTP-metoden</span><span class="sxs-lookup"><span data-stu-id="53f0a-547">HTTP Method</span></span> | <span data-ttu-id="53f0a-548">URI: N</span><span class="sxs-lookup"><span data-stu-id="53f0a-548">URI</span></span> |
|:--- |:--- |
| <span data-ttu-id="53f0a-549">POST</span><span class="sxs-lookup"><span data-stu-id="53f0a-549">POST</span></span> |`<rootURI>/ImportCatalogFile?modelId=%27<modelId>%27&filename=%27<fileName>%27&apiVersion=%271.0%27`<br><br><span data-ttu-id="53f0a-550">Exempel:</span><span class="sxs-lookup"><span data-stu-id="53f0a-550">Example:</span></span><br>`<rootURI>/ImportCatalogFile?modelId=%27a658c626-2baa-43a7-ac98-f6ee26120a12%27&filename=%27catalog10_small.txt%27&apiVersion=%271.0%27` |
| <span data-ttu-id="53f0a-551">HUVUDET</span><span class="sxs-lookup"><span data-stu-id="53f0a-551">HEADER</span></span> |`"Content-Type", "text/xml"` |

| <span data-ttu-id="53f0a-552">Parameternamn</span><span class="sxs-lookup"><span data-stu-id="53f0a-552">Parameter Name</span></span> | <span data-ttu-id="53f0a-553">Giltiga värden</span><span class="sxs-lookup"><span data-stu-id="53f0a-553">Valid Values</span></span> |
|:--- |:--- |
| <span data-ttu-id="53f0a-554">%{ModelID/</span><span class="sxs-lookup"><span data-stu-id="53f0a-554">modelId</span></span> |<span data-ttu-id="53f0a-555">Unik identifierare för hello modellen</span><span class="sxs-lookup"><span data-stu-id="53f0a-555">Unique identifier of hello model</span></span> |
| <span data-ttu-id="53f0a-556">filnamn</span><span class="sxs-lookup"><span data-stu-id="53f0a-556">filename</span></span> |<span data-ttu-id="53f0a-557">Textrepresentation identifierare för hello katalog.</span><span class="sxs-lookup"><span data-stu-id="53f0a-557">Textual identifier of hello catalog.</span></span><br><span data-ttu-id="53f0a-558">Endast bokstäver (A-Z, a-z), siffror (0-9), bindestreck (-) och understreck (_) är tillåtna.</span><span class="sxs-lookup"><span data-stu-id="53f0a-558">Only letters (A-Z, a-z), numbers (0-9), hyphens (-) and underscore (_) are allowed.</span></span><br><span data-ttu-id="53f0a-559">Maxlängd: 50</span><span class="sxs-lookup"><span data-stu-id="53f0a-559">Max length: 50</span></span> |
| <span data-ttu-id="53f0a-560">apiVersion</span><span class="sxs-lookup"><span data-stu-id="53f0a-560">apiVersion</span></span> |<span data-ttu-id="53f0a-561">1.0</span><span class="sxs-lookup"><span data-stu-id="53f0a-561">1.0</span></span> |
|  | |
| <span data-ttu-id="53f0a-562">Begärandetext</span><span class="sxs-lookup"><span data-stu-id="53f0a-562">Request Body</span></span> |<span data-ttu-id="53f0a-563">Exempel (med funktioner):</span><span class="sxs-lookup"><span data-stu-id="53f0a-563">Example (with features):</span></span><br/><span data-ttu-id="53f0a-564">2406e770-769c-4189-89de-1c9283f93a96, Clara Callan boken hello beskrivning, redigera = Richard Wright utgivare = Harper Flamingo Kanada år = 2001</span><span class="sxs-lookup"><span data-stu-id="53f0a-564">2406e770-769c-4189-89de-1c9283f93a96,Clara Callan,Book,hello book  description,author=Richard Wright,publisher=Harper Flamingo Canada,year=2001</span></span><br><span data-ttu-id="53f0a-565">21bf8088-b6c0-4509-870c-e1c7ac78304a, hello Forgetting rummet: A fiktion (Byzantium bok), adressbok, redigera = Nick Bantock utgivare = Harpercollins, år = 1997</span><span class="sxs-lookup"><span data-stu-id="53f0a-565">21bf8088-b6c0-4509-870c-e1c7ac78304a,hello Forgetting Room: A Fiction (Byzantium Book),Book,,author=Nick Bantock,publisher=Harpercollins,year=1997</span></span><br><span data-ttu-id="53f0a-566">3bb5cb44-d143-4bdd-a55c-443964bf4b23, Spadework, adressbok, redigera = Timothy Findley utgivare = HarperFlamingo Kanada år = 2001</span><span class="sxs-lookup"><span data-stu-id="53f0a-566">3bb5cb44-d143-4bdd-a55c-443964bf4b23,Spadework,Book,,author=Timothy Findley, publisher=HarperFlamingo Canada, year=2001</span></span><br><span data-ttu-id="53f0a-567">552a1940-21e4-4399-82bb-594b46d7ed54, begränsning av natur eller fauna, Book hello beskrivning, redigera = Magnus Mills, utgivare = arkad Publishing år = 1998</span><span class="sxs-lookup"><span data-stu-id="53f0a-567">552a1940-21e4-4399-82bb-594b46d7ed54,Restraint of Beasts,Book,hello book description,author=Magnus Mills, publisher=Arcade Publishing, year=1998</span></span></pre> |

<span data-ttu-id="53f0a-568">**Svaret**:</span><span class="sxs-lookup"><span data-stu-id="53f0a-568">**Response**:</span></span>

<span data-ttu-id="53f0a-569">HTTP-statuskod: 200</span><span class="sxs-lookup"><span data-stu-id="53f0a-569">HTTP Status code: 200</span></span>

<span data-ttu-id="53f0a-570">hello API returnerar en rapport över hello import.</span><span class="sxs-lookup"><span data-stu-id="53f0a-570">hello API returns a report of hello import.</span></span>

* <span data-ttu-id="53f0a-571">`feed\entry\content\properties\LineCount`-Antal rader som accepteras.</span><span class="sxs-lookup"><span data-stu-id="53f0a-571">`feed\entry\content\properties\LineCount` - Number of lines accepted.</span></span>
* <span data-ttu-id="53f0a-572">`feed\entry\content\properties\ErrorCount`-Antal rader som inte infogades på grund av tooan fel.</span><span class="sxs-lookup"><span data-stu-id="53f0a-572">`feed\entry\content\properties\ErrorCount` - Number of lines that were not inserted due tooan error.</span></span>

<span data-ttu-id="53f0a-573">OData-XML</span><span class="sxs-lookup"><span data-stu-id="53f0a-573">OData XML</span></span>

    <feed xmlns:base="https://api.datamarket.azure.com/amla/recommendations/v3/ImportCatalogFile" xmlns:d="http://schemas.microsoft.com/ado/2007/08/dataservices" xmlns:m="http://schemas.microsoft.com/ado/2007/08/dataservices/metadata" xmlns="http://www.w3.org/2005/Atom">
    <title type="text" />
    <subtitle type="text">Import catalog file</subtitle>
    <id>https://api.datamarket.azure.com/amla/recommendations/v3/ImportCatalogFile?modelId='a658c626-2baa-43a7-ac98-f6ee26120a12'&amp;filename='catalog10_small.txt'&amp;apiVersion='1.0'</id>
    <rights type="text" />
    <updated>2014-10-05T06:58:04Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/ImportCatalogFile?modelId='a658c626-2baa-43a7-ac98-f6ee26120a12'&amp;filename='catalog10_small.txt'&amp;apiVersion='1.0'" />
    <entry>
       <id>https://api.datamarket.azure.com/amla/recommendations/v3/ImportCatalogFile?modelId='a658c626-2baa-43a7-ac98-f6ee26120a12'&amp;filename='catalog10_small.txt'&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1</id>
        <title type="text">ImportCatalogFileEntity2</title>
        <updated>2014-10-05T06:58:04Z</updated>
        <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/ImportCatalogFile?modelId='a658c626-2baa-43a7-ac98-f6ee26120a12'&amp;filename='catalog10_small.txt'&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1" />
        <content type="application/xml">
            <m:properties>
                <d:LineCount m:type="Edm.String">4</d:LineCount>
                <d:ErrorCount m:type="Edm.String">0</d:ErrorCount>
            </m:properties>
        </content>
    </entry>
    </feed>

### <a name="82----get-catalog"></a><span data-ttu-id="53f0a-574">8.2.</span><span class="sxs-lookup"><span data-stu-id="53f0a-574">8.2.</span></span>    <span data-ttu-id="53f0a-575">Hämta katalogen</span><span class="sxs-lookup"><span data-stu-id="53f0a-575">Get Catalog</span></span>
<span data-ttu-id="53f0a-576">Hämtar alla katalogobjekt.</span><span class="sxs-lookup"><span data-stu-id="53f0a-576">Retrieves all catalog items.</span></span>
<span data-ttu-id="53f0a-577">hello katalog kommer att hämta en sida i taget.</span><span class="sxs-lookup"><span data-stu-id="53f0a-577">hello catalog will be retrieved one page at a time.</span></span> <span data-ttu-id="53f0a-578">Om du vill tooget objekt vid ett visst index, kan du använda hello $skip odata-parameter.</span><span class="sxs-lookup"><span data-stu-id="53f0a-578">If you want tooget items at a particular index, you can use hello $skip odata parameter.</span></span> <span data-ttu-id="53f0a-579">Till exempel om du vill tooget objekt som startar vid position 100, lägger du till hello parametern $skip = 100 toohello begäran.</span><span class="sxs-lookup"><span data-stu-id="53f0a-579">For instance if you want tooget items starting at position 100, add hello parameter $skip=100 toohello request.</span></span>

| <span data-ttu-id="53f0a-580">HTTP-metoden</span><span class="sxs-lookup"><span data-stu-id="53f0a-580">HTTP Method</span></span> | <span data-ttu-id="53f0a-581">URI: N</span><span class="sxs-lookup"><span data-stu-id="53f0a-581">URI</span></span> |
|:--- |:--- |
| <span data-ttu-id="53f0a-582">HÄMTA</span><span class="sxs-lookup"><span data-stu-id="53f0a-582">GET</span></span> |`<rootURI>/GetCatalog?modelId=%27<model_id>%27&apiVersion=%271.0%27`<br><br><span data-ttu-id="53f0a-583">Exempel:</span><span class="sxs-lookup"><span data-stu-id="53f0a-583">Example:</span></span><br>`GetCatalog?modelId=%2724024f7e-b45c-419e-bfa2-dfd947e0d253%27&apiVersion=%271.0%27` |

| <span data-ttu-id="53f0a-584">Parameternamn</span><span class="sxs-lookup"><span data-stu-id="53f0a-584">Parameter Name</span></span> | <span data-ttu-id="53f0a-585">Giltiga värden</span><span class="sxs-lookup"><span data-stu-id="53f0a-585">Valid Values</span></span> |
|:--- |:--- |
| <span data-ttu-id="53f0a-586">%{ModelID/</span><span class="sxs-lookup"><span data-stu-id="53f0a-586">modelId</span></span> |<span data-ttu-id="53f0a-587">Unik identifierare för hello modellen</span><span class="sxs-lookup"><span data-stu-id="53f0a-587">Unique identifier of hello model</span></span> |
| <span data-ttu-id="53f0a-588">apiVersion</span><span class="sxs-lookup"><span data-stu-id="53f0a-588">apiVersion</span></span> |<span data-ttu-id="53f0a-589">1.0</span><span class="sxs-lookup"><span data-stu-id="53f0a-589">1.0</span></span> |
|  | |
| <span data-ttu-id="53f0a-590">Begärandetext</span><span class="sxs-lookup"><span data-stu-id="53f0a-590">Request Body</span></span> |<span data-ttu-id="53f0a-591">INGEN</span><span class="sxs-lookup"><span data-stu-id="53f0a-591">NONE</span></span> |

<span data-ttu-id="53f0a-592">**Svaret**:</span><span class="sxs-lookup"><span data-stu-id="53f0a-592">**Response**:</span></span>

<span data-ttu-id="53f0a-593">HTTP-statuskod: 200</span><span class="sxs-lookup"><span data-stu-id="53f0a-593">HTTP Status code: 200</span></span>

<span data-ttu-id="53f0a-594">hello svaret innehåller en post per katalogobjekt.</span><span class="sxs-lookup"><span data-stu-id="53f0a-594">hello response includes one entry per catalog item.</span></span> <span data-ttu-id="53f0a-595">Varje post innehåller hello följande data:</span><span class="sxs-lookup"><span data-stu-id="53f0a-595">Each entry has hello following data:</span></span>

* <span data-ttu-id="53f0a-596">`feed/entry/content/properties/ExternalId`-Katalog-ID för extern, hello en som tillhandahålls av hello kunden.</span><span class="sxs-lookup"><span data-stu-id="53f0a-596">`feed/entry/content/properties/ExternalId` - Catalog item external ID, hello one provided by hello customer.</span></span>
* <span data-ttu-id="53f0a-597">`feed/entry/content/properties/InternalId`-Katalogen objektet internt ID, hello en Azure Machine Learning rekommendationer har genererat.</span><span class="sxs-lookup"><span data-stu-id="53f0a-597">`feed/entry/content/properties/InternalId` - Catalog item internal ID, hello one that Azure Machine Learning Recommendations has generated.</span></span>
* <span data-ttu-id="53f0a-598">`feed/entry/content/properties/Name`-Objektet katalognamnet.</span><span class="sxs-lookup"><span data-stu-id="53f0a-598">`feed/entry/content/properties/Name` - Catalog item name.</span></span>
* <span data-ttu-id="53f0a-599">`feed/entry/content/properties/Category`-Objektet kategorier.</span><span class="sxs-lookup"><span data-stu-id="53f0a-599">`feed/entry/content/properties/Category` - Catalog item category.</span></span>
* <span data-ttu-id="53f0a-600">`feed/entry/content/properties/Description`-Katalogen beskrivning.</span><span class="sxs-lookup"><span data-stu-id="53f0a-600">`feed/entry/content/properties/Description` - Catalog item description.</span></span>
* <span data-ttu-id="53f0a-601">`feed/entry/content/properties/Metadata`-Katalogen objektmetadata.</span><span class="sxs-lookup"><span data-stu-id="53f0a-601">`feed/entry/content/properties/Metadata` - Catalog item metadata.</span></span>

<span data-ttu-id="53f0a-602">OData-XML</span><span class="sxs-lookup"><span data-stu-id="53f0a-602">OData XML</span></span>

    <feed xmlns:base="https://api.datamarket.azure.com/amla/recommendations/v3/GetCatalog" xmlns:d="http://schemas.microsoft.com/ado/2007/08/dataservices" xmlns:m="http://schemas.microsoft.com/ado/2007/08/dataservices/metadata" xmlns="http://www.w3.org/2005/Atom">
        <title type="text" />
        <subtitle type="text">Get All Catalog Items</subtitle>
        <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetCatalog?modelId='0dbb55fa-7f11-418d-8537-8ff2d9d1d9c6'&amp;apiVersion='1.0'</id>
        <rights type="text" />
        <updated>2014-10-29T11:13:26Z</updated>
        <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetCatalog?modelId='0dbb55fa-7f11-418d-8537-8ff2d9d1d9c6'&amp;apiVersion='1.0'" />
        <entry>
            <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetCatalog?modelId='0dbb55fa-7f11-418d-8537-8ff2d9d1d9c6'&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1</id>
        <title type="text">AllCatalogItemsEntity</title>
        <updated>2014-10-29T11:13:26Z</updated>
        <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetCatalog?modelId='0dbb55fa-7f11-418d-8537-8ff2d9d1d9c6'&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1" />
        <content type="application/xml">
            <m:properties>
                <d:ExternalId m:type="Edm.String">552A1940-21E4-4399-82BB-594B46D7ED54</d:ExternalId>
                <d:InternalId m:type="Edm.String">060db26a-e6a6-464e-bb52-436d2da82a50</d:InternalId>
                <d:Name m:type="Edm.String">Restraint of Beasts</d:Name>
                <d:Category m:type="Edm.String">Book</d:Category>
                <d:Description m:type="Edm.String"></d:Description>
                <d:Metadata m:type="Edm.String"></d:Metadata>
            </m:properties>
        </content>
    </entry>
    <entry>
        <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetCatalog?modelId='0dbb55fa-7f11-418d-8537-8ff2d9d1d9c6'&amp;apiVersion='1.0'&amp;$skip=1&amp;$top=1</id>
        <title type="text">AllCatalogItemsEntity</title>
        <updated>2014-10-29T11:13:26Z</updated>
        <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetCatalog?modelId='0dbb55fa-7f11-418d-8537-8ff2d9d1d9c6'&amp;apiVersion='1.0'&amp;$skip=1&amp;$top=1" />
        <content type="application/xml">
            <m:properties>
                <d:ExternalId m:type="Edm.String">2406E770-769C-4189-89DE-1C9283F93A96</d:ExternalId>
                <d:InternalId m:type="Edm.String">209d7bfe-2eb9-4455-92a3-7c867a41a74a</d:InternalId>
                <d:Name m:type="Edm.String">Clara Callan</d:Name>
                <d:Category m:type="Edm.String">Book</d:Category>
                <d:Description m:type="Edm.String"></d:Description>
                <d:Metadata m:type="Edm.String"></d:Metadata>
            </m:properties>
        </content>
    </entry>
    <entry>
        <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetCatalog?modelId='0dbb55fa-7f11-418d-8537-8ff2d9d1d9c6'&amp;apiVersion='1.0'&amp;$skip=2&amp;$top=1</id>
        <title type="text">AllCatalogItemsEntity</title>
        <updated>2014-10-29T11:13:26Z</updated>
        <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetCatalog?modelId='0dbb55fa-7f11-418d-8537-8ff2d9d1d9c6'&amp;apiVersion='1.0'&amp;$skip=2&amp;$top=1" />
        <content type="application/xml">
            <m:properties>
                <d:ExternalId m:type="Edm.String">3BB5CB44-D143-4BDD-A55C-443964BF4B23</d:ExternalId>
                <d:InternalId m:type="Edm.String">913ff79b-059b-4d4d-8042-6fa4cc1391dd</d:InternalId>
                <d:Name m:type="Edm.String">Spadework</d:Name>
                <d:Category m:type="Edm.String">Book</d:Category>
                <d:Description m:type="Edm.String"></d:Description>
                <d:Metadata m:type="Edm.String"></d:Metadata>
            </m:properties>
        </content>
    </entry>
    <entry>
            <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetCatalog?modelId='0dbb55fa-7f11-418d-8537-8ff2d9d1d9c6'&amp;apiVersion='1.0'&amp;$skip=3&amp;$top=1</id>
        <title type="text">AllCatalogItemsEntity</title>
        <updated>2014-10-29T11:13:26Z</updated>
        <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetCatalog?modelId='0dbb55fa-7f11-418d-8537-8ff2d9d1d9c6'&amp;apiVersion='1.0'&amp;$skip=3&amp;$top=1" />
        <content type="application/xml">
            <m:properties>
                <d:ExternalId m:type="Edm.String">21BF8088-B6C0-4509-870C-E1C7AC78304A</d:ExternalId>
                <d:InternalId m:type="Edm.String">ea65e4fa-768c-40b4-92c3-69d3e8178691</d:InternalId>
                <d:Name m:type="Edm.String">hello Forgetting Room: A Fiction (Byzantium Book)</d:Name>
                <d:Category m:type="Edm.String">Book</d:Category>
                <d:Description m:type="Edm.String"></d:Description>
                <d:Metadata m:type="Edm.String"></d:Metadata>
            </m:properties>
        </content>
    </entry>
    </feed>

### <a name="83----get-catalog-items-by-token"></a><span data-ttu-id="53f0a-603">8.3.</span><span class="sxs-lookup"><span data-stu-id="53f0a-603">8.3.</span></span>    <span data-ttu-id="53f0a-604">Hämta katalogobjekt av Token</span><span class="sxs-lookup"><span data-stu-id="53f0a-604">Get Catalog Items by Token</span></span>
| <span data-ttu-id="53f0a-605">HTTP-metoden</span><span class="sxs-lookup"><span data-stu-id="53f0a-605">HTTP Method</span></span> | <span data-ttu-id="53f0a-606">URI: N</span><span class="sxs-lookup"><span data-stu-id="53f0a-606">URI</span></span> |
|:--- |:--- |
| <span data-ttu-id="53f0a-607">HÄMTA</span><span class="sxs-lookup"><span data-stu-id="53f0a-607">GET</span></span> |`<rootURI>/GetCatalogItemsByToken?modelId=%27<modelId>%27&token=%27<token>%27&apiVersion=%271.0%27`<br><br><span data-ttu-id="53f0a-608">Exempel:</span><span class="sxs-lookup"><span data-stu-id="53f0a-608">Example:</span></span><br>`GetCatalogItemsByToken?modelId=%270dbb55fa-7f11-418d-8537-8ff2d9d1d9c6%27&token=%27Cla%27&apiVersion=%271.0%27` |

| <span data-ttu-id="53f0a-609">Parameternamn</span><span class="sxs-lookup"><span data-stu-id="53f0a-609">Parameter Name</span></span> | <span data-ttu-id="53f0a-610">Giltiga värden</span><span class="sxs-lookup"><span data-stu-id="53f0a-610">Valid Values</span></span> |
|:--- |:--- |
| <span data-ttu-id="53f0a-611">%{ModelID/</span><span class="sxs-lookup"><span data-stu-id="53f0a-611">modelId</span></span> |<span data-ttu-id="53f0a-612">Unik identifierare för hello modellen</span><span class="sxs-lookup"><span data-stu-id="53f0a-612">Unique identifier of hello model</span></span> |
| <span data-ttu-id="53f0a-613">Token</span><span class="sxs-lookup"><span data-stu-id="53f0a-613">token</span></span> |<span data-ttu-id="53f0a-614">Token för hello katalog objektets namn.</span><span class="sxs-lookup"><span data-stu-id="53f0a-614">Token of hello catalog item’s name.</span></span> <span data-ttu-id="53f0a-615">Ska innehålla minst 3 tecken.</span><span class="sxs-lookup"><span data-stu-id="53f0a-615">Should contain at least 3 characters.</span></span> |
| <span data-ttu-id="53f0a-616">apiVersion</span><span class="sxs-lookup"><span data-stu-id="53f0a-616">apiVersion</span></span> |<span data-ttu-id="53f0a-617">1.0</span><span class="sxs-lookup"><span data-stu-id="53f0a-617">1.0</span></span> |
|  | |
| <span data-ttu-id="53f0a-618">Begärandetext</span><span class="sxs-lookup"><span data-stu-id="53f0a-618">Request Body</span></span> |<span data-ttu-id="53f0a-619">INGEN</span><span class="sxs-lookup"><span data-stu-id="53f0a-619">NONE</span></span> |

<span data-ttu-id="53f0a-620">**Svaret**:</span><span class="sxs-lookup"><span data-stu-id="53f0a-620">**Response**:</span></span>

<span data-ttu-id="53f0a-621">HTTP-statuskod: 200</span><span class="sxs-lookup"><span data-stu-id="53f0a-621">HTTP Status code: 200</span></span>

<span data-ttu-id="53f0a-622">hello svaret innehåller en post per katalogobjekt.</span><span class="sxs-lookup"><span data-stu-id="53f0a-622">hello response includes one entry per catalog item.</span></span> <span data-ttu-id="53f0a-623">Varje post innehåller hello följande data:</span><span class="sxs-lookup"><span data-stu-id="53f0a-623">Each entry has hello following data:</span></span>

* <span data-ttu-id="53f0a-624">`feed/entry/content/properties/InternalId`-Katalogen objektet internt ID, hello en Azure Machine Learning rekommendationer har genererat.</span><span class="sxs-lookup"><span data-stu-id="53f0a-624">`feed/entry/content/properties/InternalId` - Catalog item internal ID, hello one that Azure Machine Learning Recommendations has generated.</span></span>
* <span data-ttu-id="53f0a-625">`feed/entry/content/properties/Name`-Objektet katalognamnet.</span><span class="sxs-lookup"><span data-stu-id="53f0a-625">`feed/entry/content/properties/Name` - Catalog item name.</span></span>
* <span data-ttu-id="53f0a-626">`feed/entry/content/properties/Rating`-(för framtida användning)</span><span class="sxs-lookup"><span data-stu-id="53f0a-626">`feed/entry/content/properties/Rating` -  (for future use)</span></span>
* <span data-ttu-id="53f0a-627">`feed/entry/content/properties/Reasoning`-(för framtida användning)</span><span class="sxs-lookup"><span data-stu-id="53f0a-627">`feed/entry/content/properties/Reasoning` -  (for future use)</span></span>
* <span data-ttu-id="53f0a-628">`feed/entry/content/properties/Metadata`-(för framtida användning)</span><span class="sxs-lookup"><span data-stu-id="53f0a-628">`feed/entry/content/properties/Metadata` -  (for future use)</span></span>
* <span data-ttu-id="53f0a-629">`feed/entry/content/properties/FormattedRating`-(för framtida användning)</span><span class="sxs-lookup"><span data-stu-id="53f0a-629">`feed/entry/content/properties/FormattedRating` - (for future use)</span></span>

<span data-ttu-id="53f0a-630">OData-XML</span><span class="sxs-lookup"><span data-stu-id="53f0a-630">OData XML</span></span>

    <feed xmlns:base="https://api.datamarket.azure.com/amla/recommendations/v3/GetCatalogItemsByToken" xmlns:d="http://schemas.microsoft.com/ado/2007/08/dataservices" xmlns:m="http://schemas.microsoft.com/ado/2007/08/dataservices/metadata" xmlns="http://www.w3.org/2005/Atom">
        <title type="text" />
        <subtitle type="text">Get Catalog items that contain a token</subtitle>
        <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetCatalogItemsByToken?modelId='0dbb55fa-7f11-418d-8537-8ff2d9d1d9c6'&amp;token='Cla'&amp;apiVersion='1.0'</id>
        <rights type="text" />
        <updated>2014-10-29T11:48:19Z</updated>
        <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetCatalogItemsByToken?modelId='0dbb55fa-7f11-418d-8537-8ff2d9d1d9c6'&amp;token='Cla'&amp;apiVersion='1.0'" />
        <entry>
            <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetCatalogItemsByToken?modelId='0dbb55fa-7f11-418d-8537-8ff2d9d1d9c6'&amp;token='Cla'&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1</id>
            <title type="text">CatalogItemsThatContainATokenEntity</title>
            <updated>2014-10-29T11:48:19Z</updated>
            <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetCatalogItemsByToken?modelId='0dbb55fa-7f11-418d-8537-8ff2d9d1d9c6'&amp;token='Cla'&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1" />
            <content type="application/xml">
                  <m:properties>
                    <d:Id m:type="Edm.String">2406E770-769C-4189-89DE-1C9283F93A96</d:Id>
                    <d:Name m:type="Edm.String">Clara Callan</d:Name>
                    <d:Rating m:type="Edm.Double">0</d:Rating>
                    <d:Reasoning m:type="Edm.String"></d:Reasoning>
                    <d:Metadata m:type="Edm.String"></d:Metadata>
                    <d:FormattedRating m:type="Edm.Double" m:null="true"></d:FormattedRating>
                  </m:properties>
            </content>
        </entry>
    </feed>

## <a name="9-usage-data"></a><span data-ttu-id="53f0a-631">9. Användningsdata</span><span class="sxs-lookup"><span data-stu-id="53f0a-631">9. Usage data</span></span>
### <a name="91----import-usage-data"></a><span data-ttu-id="53f0a-632">9.1.</span><span class="sxs-lookup"><span data-stu-id="53f0a-632">9.1.</span></span>    <span data-ttu-id="53f0a-633">Importera Data om programvaruanvändning</span><span class="sxs-lookup"><span data-stu-id="53f0a-633">Import Usage Data</span></span>
#### <a name="911-uploading-file"></a><span data-ttu-id="53f0a-634">9.1.1.</span><span class="sxs-lookup"><span data-stu-id="53f0a-634">9.1.1.</span></span> <span data-ttu-id="53f0a-635">Ladda upp fil</span><span class="sxs-lookup"><span data-stu-id="53f0a-635">Uploading File</span></span>
<span data-ttu-id="53f0a-636">Det här avsnittet visas hur tooupload användningsdata med hjälp av en fil.</span><span class="sxs-lookup"><span data-stu-id="53f0a-636">This section shows how tooupload usage data by using a file.</span></span> <span data-ttu-id="53f0a-637">Du kan anropa denna API flera gånger med användningsdata.</span><span class="sxs-lookup"><span data-stu-id="53f0a-637">You can call this API several times with usage data.</span></span> <span data-ttu-id="53f0a-638">Alla användningsdata sparas för alla samtal.</span><span class="sxs-lookup"><span data-stu-id="53f0a-638">All usage data will be saved for all calls.</span></span>

| <span data-ttu-id="53f0a-639">HTTP-metoden</span><span class="sxs-lookup"><span data-stu-id="53f0a-639">HTTP Method</span></span> | <span data-ttu-id="53f0a-640">URI: N</span><span class="sxs-lookup"><span data-stu-id="53f0a-640">URI</span></span> |
|:--- |:--- |
| <span data-ttu-id="53f0a-641">POST</span><span class="sxs-lookup"><span data-stu-id="53f0a-641">POST</span></span> |`<rootURI>/ImportUsageFile?modelId=%27<modelId>%27&filename=%27<fileName>%27&apiVersion=%271.0%27`<br><br><span data-ttu-id="53f0a-642">Exempel:</span><span class="sxs-lookup"><span data-stu-id="53f0a-642">Example:</span></span><br>`<rootURI>/ImportUsageFile?modelId=%27a658c626-2baa-43a7-ac98-f6ee26120a12%27&filename=%27ImplicitMatrix10_Guid_small.txt%27&apiVersion=%271.0%27` |

| <span data-ttu-id="53f0a-643">Parameternamn</span><span class="sxs-lookup"><span data-stu-id="53f0a-643">Parameter Name</span></span> | <span data-ttu-id="53f0a-644">Giltiga värden</span><span class="sxs-lookup"><span data-stu-id="53f0a-644">Valid Values</span></span> |
|:--- |:--- |
| <span data-ttu-id="53f0a-645">%{ModelID/</span><span class="sxs-lookup"><span data-stu-id="53f0a-645">modelId</span></span> |<span data-ttu-id="53f0a-646">Unik identifierare för hello modellen</span><span class="sxs-lookup"><span data-stu-id="53f0a-646">Unique identifier of hello model</span></span> |
| <span data-ttu-id="53f0a-647">filnamn</span><span class="sxs-lookup"><span data-stu-id="53f0a-647">filename</span></span> |<span data-ttu-id="53f0a-648">Textrepresentation identifierare för hello katalog.</span><span class="sxs-lookup"><span data-stu-id="53f0a-648">Textual identifier of hello catalog.</span></span><br><span data-ttu-id="53f0a-649">Endast bokstäver (A-Z, a-z), siffror (0-9), bindestreck (-) och understreck (_) är tillåtna.</span><span class="sxs-lookup"><span data-stu-id="53f0a-649">Only letters (A-Z, a-z), numbers (0-9), hyphens (-) and underscore (_) are allowed.</span></span><br><span data-ttu-id="53f0a-650">Maxlängd: 50</span><span class="sxs-lookup"><span data-stu-id="53f0a-650">Max length: 50</span></span> |
| <span data-ttu-id="53f0a-651">apiVersion</span><span class="sxs-lookup"><span data-stu-id="53f0a-651">apiVersion</span></span> |<span data-ttu-id="53f0a-652">1.0</span><span class="sxs-lookup"><span data-stu-id="53f0a-652">1.0</span></span> |
|  | |
| <span data-ttu-id="53f0a-653">Begärandetext</span><span class="sxs-lookup"><span data-stu-id="53f0a-653">Request Body</span></span> |<span data-ttu-id="53f0a-654">Användningsdata.</span><span class="sxs-lookup"><span data-stu-id="53f0a-654">Usage data.</span></span> <span data-ttu-id="53f0a-655">Format:</span><span class="sxs-lookup"><span data-stu-id="53f0a-655">Format:</span></span><br>`<User Id>,<Item Id>[,<Time>,<Event>]`<br><br><table><tr><th><span data-ttu-id="53f0a-656">Namn</span><span class="sxs-lookup"><span data-stu-id="53f0a-656">Name</span></span></th><th><span data-ttu-id="53f0a-657">Obligatorisk</span><span class="sxs-lookup"><span data-stu-id="53f0a-657">Mandatory</span></span></th><th><span data-ttu-id="53f0a-658">Typ</span><span class="sxs-lookup"><span data-stu-id="53f0a-658">Type</span></span></th><th><span data-ttu-id="53f0a-659">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="53f0a-659">Description</span></span></th></tr><tr><td><span data-ttu-id="53f0a-660">Användar-Id</span><span class="sxs-lookup"><span data-stu-id="53f0a-660">User Id</span></span></td><td><span data-ttu-id="53f0a-661">Ja</span><span class="sxs-lookup"><span data-stu-id="53f0a-661">Yes</span></span></td><td><span data-ttu-id="53f0a-662">[A-z], [a-z], [0-9] [_] &#40; Understreck &#41; [-] &#40; Dash &#41;</span><span class="sxs-lookup"><span data-stu-id="53f0a-662">[A-z], [a-z], [0-9], [_] &#40;Underscore&#41;, [-] &#40;Dash&#41;</span></span><br> <span data-ttu-id="53f0a-663">Maxlängd: 255</span><span class="sxs-lookup"><span data-stu-id="53f0a-663">Max length: 255</span></span> </td><td><span data-ttu-id="53f0a-664">Unik identifierare för en användare.</span><span class="sxs-lookup"><span data-stu-id="53f0a-664">Unique identifier of a user.</span></span></td></tr><tr><td><span data-ttu-id="53f0a-665">Objekt-Id</span><span class="sxs-lookup"><span data-stu-id="53f0a-665">Item Id</span></span></td><td><span data-ttu-id="53f0a-666">Ja</span><span class="sxs-lookup"><span data-stu-id="53f0a-666">Yes</span></span></td><td><span data-ttu-id="53f0a-667">[A-z], [a-z], [0-9] [& #95.] &#40; Understreck &#41; [-] &#40; Dash &#41;</span><span class="sxs-lookup"><span data-stu-id="53f0a-667">[A-z], [a-z], [0-9], [&#95;] &#40;Underscore&#41;, [-] &#40;Dash&#41;</span></span><br> <span data-ttu-id="53f0a-668">Maxlängd: 50</span><span class="sxs-lookup"><span data-stu-id="53f0a-668">Max length: 50</span></span></td><td><span data-ttu-id="53f0a-669">Unik identifierare för ett objekt.</span><span class="sxs-lookup"><span data-stu-id="53f0a-669">Unique identifier of an item.</span></span></td></tr><tr><td><span data-ttu-id="53f0a-670">Tid</span><span class="sxs-lookup"><span data-stu-id="53f0a-670">Time</span></span></td><td><span data-ttu-id="53f0a-671">Nej</span><span class="sxs-lookup"><span data-stu-id="53f0a-671">No</span></span></td><td><span data-ttu-id="53f0a-672">Datum i format: ÅÅÅÅ/MM/ddTHH (t.ex. 2013/06/20T10:00:00)</span><span class="sxs-lookup"><span data-stu-id="53f0a-672">Date in format: YYYY/MM/DDTHH:MM:SS (e.g. 2013/06/20T10:00:00)</span></span></td><td><span data-ttu-id="53f0a-673">Tid för data.</span><span class="sxs-lookup"><span data-stu-id="53f0a-673">Time of data.</span></span></td></tr><tr><td><span data-ttu-id="53f0a-674">Händelse</span><span class="sxs-lookup"><span data-stu-id="53f0a-674">Event</span></span></td><td><span data-ttu-id="53f0a-675">Nej. Om anges måste också sätta datum</span><span class="sxs-lookup"><span data-stu-id="53f0a-675">No; if supplied then must also put date</span></span></td><td><span data-ttu-id="53f0a-676">En av hello följande:</span><span class="sxs-lookup"><span data-stu-id="53f0a-676">One of hello following:</span></span><br><span data-ttu-id="53f0a-677">• Klicka på</span><span class="sxs-lookup"><span data-stu-id="53f0a-677">• Click</span></span><br><span data-ttu-id="53f0a-678">• RecommendationClick</span><span class="sxs-lookup"><span data-stu-id="53f0a-678">• RecommendationClick</span></span><br><span data-ttu-id="53f0a-679">• AddShopCart</span><span class="sxs-lookup"><span data-stu-id="53f0a-679">•    AddShopCart</span></span><br><span data-ttu-id="53f0a-680">• RemoveShopCart</span><span class="sxs-lookup"><span data-stu-id="53f0a-680">• RemoveShopCart</span></span><br><span data-ttu-id="53f0a-681">• Köp</span><span class="sxs-lookup"><span data-stu-id="53f0a-681">• Purchase</span></span></td><td></td></tr></table><br><span data-ttu-id="53f0a-682">Maximal filstorlek: 200MB</span><span class="sxs-lookup"><span data-stu-id="53f0a-682">Maximum file size: 200MB</span></span><br><br><span data-ttu-id="53f0a-683">Exempel:</span><span class="sxs-lookup"><span data-stu-id="53f0a-683">Example:</span></span><br><pre>149452,1b3d95e2-84e4-414c-bb38-be9cf461c347<br>6360,1b3d95e2-84e4-414c-bb38-be9cf461c347<br>50321,1b3d95e2-84e4-414c-bb38-be9cf461c347<br>71285,1b3d95e2-84e4-414c-bb38-be9cf461c347<br>224450,1b3d95e2-84e4-414c-bb38-be9cf461c347<br>236645,1b3d95e2-84e4-414c-bb38-be9cf461c347<br>107951,1b3d95e2-84e4-414c-bb38-be9cf461c347</pre> |

<span data-ttu-id="53f0a-684">**Svaret**:</span><span class="sxs-lookup"><span data-stu-id="53f0a-684">**Response**:</span></span>

<span data-ttu-id="53f0a-685">HTTP-statuskod: 200</span><span class="sxs-lookup"><span data-stu-id="53f0a-685">HTTP Status code: 200</span></span>

* <span data-ttu-id="53f0a-686">`Feed\entry\content\properties\LineCount`-Antal rader som accepteras.</span><span class="sxs-lookup"><span data-stu-id="53f0a-686">`Feed\entry\content\properties\LineCount` - Number of lines accepted.</span></span>
* <span data-ttu-id="53f0a-687">`Feed\entry\content\properties\ErrorCount`-Antal rader som inte infogades på grund av tooan fel.</span><span class="sxs-lookup"><span data-stu-id="53f0a-687">`Feed\entry\content\properties\ErrorCount` - Number of lines that were not inserted due tooan error.</span></span>
* <span data-ttu-id="53f0a-688">`Feed\entry\content\properties\FileId`-Filen identifierare.</span><span class="sxs-lookup"><span data-stu-id="53f0a-688">`Feed\entry\content\properties\FileId` - File identifier.</span></span>

<span data-ttu-id="53f0a-689">OData-XML</span><span class="sxs-lookup"><span data-stu-id="53f0a-689">OData XML</span></span>

    <feed xmlns:base="https://api.datamarket.azure.com/amla/recommendations/v3/ImportUsageFile" xmlns:d="http://schemas.microsoft.com/ado/2007/08/dataservices" xmlns:m="http://schemas.microsoft.com/ado/2007/08/dataservices/metadata" xmlns="http://www.w3.org/2005/Atom">
      <title type="text" />
      <subtitle type="text">Add bulk usage data (usage file)</subtitle>
      <id>https://api.datamarket.azure.com/amla/recommendations/v3/ImportUsageFile?modelId='a658c626-2baa-43a7-ac98-f6ee26120a12'&amp;filename='ImplicitMatrix10_Guid_small.txt'&amp;apiVersion='1.0'</id>
      <rights type="text" />
      <updated>2014-10-05T07:21:44Z</updated>
      <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/ImportUsageFile?modelId='a658c626-2baa-43a7-ac98-f6ee26120a12'&amp;filename='ImplicitMatrix10_Guid_small.txt'&amp;apiVersion='1.0'" />
      <entry>
    <id>https://api.datamarket.azure.com/amla/recommendations/v3/ImportUsageFile?modelId='a658c626-2baa-43a7-ac98-f6ee26120a12'&amp;filename='ImplicitMatrix10_Guid_small.txt'&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1</id>
    <title type="text">AddBulkUsageDataUsageFileEntity2</title>
    <updated>2014-10-05T07:21:44Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/ImportUsageFile?modelId='a658c626-2baa-43a7-ac98-f6ee26120a12'&amp;filename='ImplicitMatrix10_Guid_small.txt'&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1" />
    <content type="application/xml">
      <m:properties>
        <d:LineCount m:type="Edm.String">33</d:LineCount>
        <d:ErrorCount m:type="Edm.String">0</d:ErrorCount>
        <d:FileId m:type="Edm.String">fead7c1c-db01-46c0-872f-65bcda36025d</d:FileId>
      </m:properties>
    </content>
      </entry>
    </feed>


#### <a name="912-using-data-acquisition"></a><span data-ttu-id="53f0a-690">9.1.2.</span><span class="sxs-lookup"><span data-stu-id="53f0a-690">9.1.2.</span></span> <span data-ttu-id="53f0a-691">Med hjälp av datainsamling</span><span class="sxs-lookup"><span data-stu-id="53f0a-691">Using Data Acquisition</span></span>
<span data-ttu-id="53f0a-692">Det här avsnittet visar hur toosend händelser i real time tooAzure Machine Learning rekommendationer, vanligen från din webbplats.</span><span class="sxs-lookup"><span data-stu-id="53f0a-692">This section shows how toosend events in real time tooAzure Machine Learning Recommendations, usually from your website.</span></span>

| <span data-ttu-id="53f0a-693">HTTP-metoden</span><span class="sxs-lookup"><span data-stu-id="53f0a-693">HTTP Method</span></span> | <span data-ttu-id="53f0a-694">URI: N</span><span class="sxs-lookup"><span data-stu-id="53f0a-694">URI</span></span> |
|:--- |:--- |
| <span data-ttu-id="53f0a-695">POST</span><span class="sxs-lookup"><span data-stu-id="53f0a-695">POST</span></span> |`<rootURI>/AddUsageEvent?apiVersion=%271.0%27` |
| <span data-ttu-id="53f0a-696">HUVUDET</span><span class="sxs-lookup"><span data-stu-id="53f0a-696">HEADER</span></span> |`"Content-Type", "text/xml"` |

| <span data-ttu-id="53f0a-697">Parameternamn</span><span class="sxs-lookup"><span data-stu-id="53f0a-697">Parameter Name</span></span> | <span data-ttu-id="53f0a-698">Giltiga värden</span><span class="sxs-lookup"><span data-stu-id="53f0a-698">Valid Values</span></span> |
|:--- |:--- |
| <span data-ttu-id="53f0a-699">apiVersion</span><span class="sxs-lookup"><span data-stu-id="53f0a-699">apiVersion</span></span> |<span data-ttu-id="53f0a-700">1.0</span><span class="sxs-lookup"><span data-stu-id="53f0a-700">1.0</span></span> |
| <span data-ttu-id="53f0a-701">Begärandetexten</span><span class="sxs-lookup"><span data-stu-id="53f0a-701">Request body</span></span> |<span data-ttu-id="53f0a-702">Händelsen inmatning för varje händelse som du vill toosend.</span><span class="sxs-lookup"><span data-stu-id="53f0a-702">Event data entry for each event you want toosend.</span></span> <span data-ttu-id="53f0a-703">Du bör skicka för hello samma användare eller webbläsare session hello samma ID i hello sessions-ID-fältet.</span><span class="sxs-lookup"><span data-stu-id="53f0a-703">You should send for hello same user or browser session hello same ID in hello SessionId field.</span></span> <span data-ttu-id="53f0a-704">(Se exemplet för händelsen nedan.)</span><span class="sxs-lookup"><span data-stu-id="53f0a-704">(See sample of event body below.)</span></span> |

* <span data-ttu-id="53f0a-705">Händelse 'Klickar du på ”exempel:</span><span class="sxs-lookup"><span data-stu-id="53f0a-705">Example for event 'Click':</span></span>
  
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
* <span data-ttu-id="53f0a-706">Händelse 'RecommendationClick' exempel:</span><span class="sxs-lookup"><span data-stu-id="53f0a-706">Example for event 'RecommendationClick':</span></span>
  
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
* <span data-ttu-id="53f0a-707">Händelse 'AddShopCart' exempel:</span><span class="sxs-lookup"><span data-stu-id="53f0a-707">Example for event 'AddShopCart':</span></span>
  
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
* <span data-ttu-id="53f0a-708">Händelse 'RemoveShopCart' exempel:</span><span class="sxs-lookup"><span data-stu-id="53f0a-708">Example for event 'RemoveShopCart':</span></span>
  
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
* <span data-ttu-id="53f0a-709">Händelse 'Inköp' exempel:</span><span class="sxs-lookup"><span data-stu-id="53f0a-709">Example for event 'Purchase':</span></span>
  
        <Event xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
        <ModelId>2779c063-48fb-46c1-bae3-74acddc8c1d1</ModelId>
        <SessionId>11112222</SessionId>
        <EventData>
            <EventData>
                <Name>Purchase</Name>
                <PurchaseItems>
                    <PurchaseItem>
                        <ItemId>ABBF8081-C5C0-4F09-9701-E1C7AC78304A</ItemId>
                        <Count>1</Count>
                    </PurchaseItem>
                    <PurchaseItem>
                        <ItemId>21BF8088-B6C0-4509-870C-11C0AC7F304B</ItemId>
                        <Count>3</Count>
                    </PurchaseItem>
                </PurchaseItems>
            </EventData>
        </EventData>
        </Event>
* <span data-ttu-id="53f0a-710">Exempel skickar 2 händelser, klickar du på' och 'AddShopCart':</span><span class="sxs-lookup"><span data-stu-id="53f0a-710">Example sending 2 events, 'Click' and 'AddShopCart':</span></span>
  
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

<span data-ttu-id="53f0a-711">**Svaret**: HTTP-statuskod: 200</span><span class="sxs-lookup"><span data-stu-id="53f0a-711">**Response**: HTTP Status code: 200</span></span>

### <a name="92----list-model-usage-files"></a><span data-ttu-id="53f0a-712">9.2.</span><span class="sxs-lookup"><span data-stu-id="53f0a-712">9.2.</span></span>    <span data-ttu-id="53f0a-713">Lista modellen Användningsfiler</span><span class="sxs-lookup"><span data-stu-id="53f0a-713">List Model Usage Files</span></span>
<span data-ttu-id="53f0a-714">Hämtar metadata för alla filer för användning av modellen.</span><span class="sxs-lookup"><span data-stu-id="53f0a-714">Retrieves metadata of all model usage files.</span></span>
<span data-ttu-id="53f0a-715">hello hämta användning filer kommer att en sida i taget.</span><span class="sxs-lookup"><span data-stu-id="53f0a-715">hello usage files will be retrieved one page at a time.</span></span> <span data-ttu-id="53f0a-716">Varje containes 100 objekt.</span><span class="sxs-lookup"><span data-stu-id="53f0a-716">Each page containes 100 items.</span></span> <span data-ttu-id="53f0a-717">Om du vill tooget objekt vid ett visst index, kan du använda hello $skip odata-parameter.</span><span class="sxs-lookup"><span data-stu-id="53f0a-717">If you want tooget items at a particular index, you can use hello $skip odata parameter.</span></span> <span data-ttu-id="53f0a-718">Till exempel om du vill tooget objekt som startar vid position 100, lägger du till hello parametern $skip = 100 toohello begäran.</span><span class="sxs-lookup"><span data-stu-id="53f0a-718">For instance if you want tooget items starting at position 100, add hello parameter $skip=100 toohello request.</span></span>

| <span data-ttu-id="53f0a-719">HTTP-metoden</span><span class="sxs-lookup"><span data-stu-id="53f0a-719">HTTP Method</span></span> | <span data-ttu-id="53f0a-720">URI: N</span><span class="sxs-lookup"><span data-stu-id="53f0a-720">URI</span></span> |
|:--- |:--- |
| <span data-ttu-id="53f0a-721">HÄMTA</span><span class="sxs-lookup"><span data-stu-id="53f0a-721">GET</span></span> |`<rootURI>/ListModelUsageFiles?forModelId=%27<model_id>%27&apiVersion=%271.0%27`<br><br><span data-ttu-id="53f0a-722">Exempel:</span><span class="sxs-lookup"><span data-stu-id="53f0a-722">Example:</span></span><br>`<rootURI>/ListModelUsageFiles?forModelId=%270dbb55fa-7f11-418d-8537-8ff2d9d1d9c6%27&apiVersion=%271.0%27` |

| <span data-ttu-id="53f0a-723">Parameternamn</span><span class="sxs-lookup"><span data-stu-id="53f0a-723">Parameter Name</span></span> | <span data-ttu-id="53f0a-724">Giltiga värden</span><span class="sxs-lookup"><span data-stu-id="53f0a-724">Valid Values</span></span> |
|:--- |:--- |
| <span data-ttu-id="53f0a-725">forModelId</span><span class="sxs-lookup"><span data-stu-id="53f0a-725">forModelId</span></span> |<span data-ttu-id="53f0a-726">Unik identifierare för hello modellen</span><span class="sxs-lookup"><span data-stu-id="53f0a-726">Unique identifier of hello model</span></span> |
| <span data-ttu-id="53f0a-727">apiVersion</span><span class="sxs-lookup"><span data-stu-id="53f0a-727">apiVersion</span></span> |<span data-ttu-id="53f0a-728">1.0</span><span class="sxs-lookup"><span data-stu-id="53f0a-728">1.0</span></span> |
|  | |
| <span data-ttu-id="53f0a-729">Begärandetext</span><span class="sxs-lookup"><span data-stu-id="53f0a-729">Request Body</span></span> |<span data-ttu-id="53f0a-730">INGEN</span><span class="sxs-lookup"><span data-stu-id="53f0a-730">NONE</span></span> |

<span data-ttu-id="53f0a-731">**Svaret**:</span><span class="sxs-lookup"><span data-stu-id="53f0a-731">**Response**:</span></span>

<span data-ttu-id="53f0a-732">HTTP-statuskod: 200</span><span class="sxs-lookup"><span data-stu-id="53f0a-732">HTTP Status code: 200</span></span>

<span data-ttu-id="53f0a-733">hello svaret innehåller en post per fil för användning.</span><span class="sxs-lookup"><span data-stu-id="53f0a-733">hello response includes one entry per usage file.</span></span> <span data-ttu-id="53f0a-734">Varje post innehåller hello följande data:</span><span class="sxs-lookup"><span data-stu-id="53f0a-734">Each entry has hello following data:</span></span>

* <span data-ttu-id="53f0a-735">`feed\entry\content\properties\Id`-Användning fil-ID.</span><span class="sxs-lookup"><span data-stu-id="53f0a-735">`feed\entry\content\properties\Id` - Usage file ID.</span></span>
* <span data-ttu-id="53f0a-736">`feed\entry\content\properties\Length`-Användning fillängden i MB.</span><span class="sxs-lookup"><span data-stu-id="53f0a-736">`feed\entry\content\properties\Length` - Usage file length in MB.</span></span>
* <span data-ttu-id="53f0a-737">`feed\entry\content\properties\DateModified`-Datum när hello användning filen skapades.</span><span class="sxs-lookup"><span data-stu-id="53f0a-737">`feed\entry\content\properties\DateModified` - Date when hello usage file was created.</span></span>
* <span data-ttu-id="53f0a-738">`feed\entry\content\properties\UseInModel`– Om hello användning filen används i hello modellen.</span><span class="sxs-lookup"><span data-stu-id="53f0a-738">`feed\entry\content\properties\UseInModel` - Whether hello usage file is used in hello model.</span></span>

<span data-ttu-id="53f0a-739">OData-XML</span><span class="sxs-lookup"><span data-stu-id="53f0a-739">OData XML</span></span>

    <feed xmlns:base="https://api.datamarket.azure.com/amla/recommendations/v3/ListModelUsageFiles" xmlns:d="http://schemas.microsoft.com/ado/2007/08/dataservices" xmlns:m="http://schemas.microsoft.com/ado/2007/08/dataservices/metadata" xmlns="http://www.w3.org/2005/Atom">
        <title type="text" />
        <subtitle type="text">Get a list of model's usage files info</subtitle>
        <id>https://api.datamarket.azure.com/amla/recommendations/v3/ListModelUsageFiles?forModelId='1c1110f8-7d9f-4c64-a807-4c9c5329993a'&amp;apiVersion='1.0'</id>
        <rights type="text" />
        <updated>2014-10-30T09:40:25Z</updated>
        <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/ListModelUsageFiles?forModelId='1c1110f8-7d9f-4c64-a807-4c9c5329993a'&amp;apiVersion='1.0'" />
        <entry>
            <id>https://api.datamarket.azure.com/amla/recommendations/v3/ListModelUsageFiles?forModelId='1c1110f8-7d9f-4c64-a807-4c9c5329993a'&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1</id>
            <title type="text">GetAListOfModelsUsageFilesInfoEntity</title>
            <updated>2014-10-30T09:40:25Z</updated>
            <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/ListModelUsageFiles?forModelId='1c1110f8-7d9f-4c64-a807-4c9c5329993a'&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1" />
            <content type="application/xml">
                <m:properties>
                    <d:Id m:type="Edm.String">4c067b42-e975-4cb2-8c98-a6ab80ed6d63</d:Id>
                    <d:Length m:type="Edm.Double">0</d:Length>
                    <d:DateModified m:type="Edm.String">10/30/2014 9:19:57 AM</d:DateModified>
                    <d:UseInModel m:type="Edm.String">true</d:UseInModel>
                </m:properties>
            </content>
        </entry>
    <entry>
        <id>https://api.datamarket.azure.com/amla/recommendations/v3/ListModelUsageFiles?forModelId='1c1110f8-7d9f-4c64-a807-4c9c5329993a'&amp;apiVersion='1.0'&amp;$skip=1&amp;$top=1</id>
        <title type="text">GetAListOfModelsUsageFilesInfoEntity</title>
        <updated>2014-10-30T09:40:25Z</updated>
        <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/ListModelUsageFiles?forModelId='1c1110f8-7d9f-4c64-a807-4c9c5329993a'&amp;apiVersion='1.0'&amp;$skip=1&amp;$top=1" />
        <content type="application/xml">
            <m:properties>
                <d:Id m:type="Edm.String">3126d816-4e80-4248-8339-1ebbdb9d544d</d:Id>
                <d:Length m:type="Edm.Double">0.001</d:Length>
                <d:DateModified m:type="Edm.String">10/30/2014 9:21:44 AM</d:DateModified>
                <d:UseInModel m:type="Edm.String">true</d:UseInModel>
              </m:properties>
        </content>
    </entry>
</feed>

### <a name="93----get-usage-statistics"></a><span data-ttu-id="53f0a-740">9.3.</span><span class="sxs-lookup"><span data-stu-id="53f0a-740">9.3.</span></span>    <span data-ttu-id="53f0a-741">Hämta användningsstatistik</span><span class="sxs-lookup"><span data-stu-id="53f0a-741">Get Usage Statistics</span></span>
<span data-ttu-id="53f0a-742">Hämtar användningsstatistik.</span><span class="sxs-lookup"><span data-stu-id="53f0a-742">Gets usage statistics.</span></span>

| <span data-ttu-id="53f0a-743">HTTP-metoden</span><span class="sxs-lookup"><span data-stu-id="53f0a-743">HTTP Method</span></span> | <span data-ttu-id="53f0a-744">URI: N</span><span class="sxs-lookup"><span data-stu-id="53f0a-744">URI</span></span> |
|:--- |:--- |
| <span data-ttu-id="53f0a-745">HÄMTA</span><span class="sxs-lookup"><span data-stu-id="53f0a-745">GET</span></span> |`<rootURI>/GetUsageStatistics?modelId=%27<modelId>%27& startDate=%27<date>%27&endDate=%27<date>%27&eventTypes=%27<types>%27&apiVersion=%271.0%27`<br><br><span data-ttu-id="53f0a-746">Exempel:</span><span class="sxs-lookup"><span data-stu-id="53f0a-746">Example:</span></span><br>`<rootURI>/GetUsageStatistics?modelId=%271d20c34f-dca1-4eac-8e5d-f299e4e4ad66%27&startDate=%272014%2F10%2F17T00%3A00%3A00%27&endDate=%272014%2F11%2F16T00%3A00%3A00%27&eventTypes=%271%2C2%2C3%2C4%2C5%27&apiVersion=%271.0%27` |

| <span data-ttu-id="53f0a-747">Parameternamn</span><span class="sxs-lookup"><span data-stu-id="53f0a-747">Parameter Name</span></span> | <span data-ttu-id="53f0a-748">Giltiga värden</span><span class="sxs-lookup"><span data-stu-id="53f0a-748">Valid Values</span></span> |
|:--- |:--- |
| <span data-ttu-id="53f0a-749">%{ModelID/</span><span class="sxs-lookup"><span data-stu-id="53f0a-749">modelId</span></span> |<span data-ttu-id="53f0a-750">Unik identifierare för hello modellen</span><span class="sxs-lookup"><span data-stu-id="53f0a-750">Unique identifier of hello model</span></span> |
| <span data-ttu-id="53f0a-751">Startdatum</span><span class="sxs-lookup"><span data-stu-id="53f0a-751">startDate</span></span> |<span data-ttu-id="53f0a-752">Startdatum.</span><span class="sxs-lookup"><span data-stu-id="53f0a-752">Start date.</span></span> <span data-ttu-id="53f0a-753">Format: ÅÅÅÅ/MM/ddTHH</span><span class="sxs-lookup"><span data-stu-id="53f0a-753">Format: yyyy/MM/ddTHH:mm:ss</span></span> |
| <span data-ttu-id="53f0a-754">Slutdatum</span><span class="sxs-lookup"><span data-stu-id="53f0a-754">endDate</span></span> |<span data-ttu-id="53f0a-755">Slutdatumet.</span><span class="sxs-lookup"><span data-stu-id="53f0a-755">End date.</span></span> <span data-ttu-id="53f0a-756">Format: ÅÅÅÅ/MM/ddTHH</span><span class="sxs-lookup"><span data-stu-id="53f0a-756">Format: yyyy/MM/ddTHH:mm:ss</span></span> |
| <span data-ttu-id="53f0a-757">eventTypes</span><span class="sxs-lookup"><span data-stu-id="53f0a-757">eventTypes</span></span> |<span data-ttu-id="53f0a-758">Kommaavgränsad sträng för händelsen typer eller null tooget alla händelser</span><span class="sxs-lookup"><span data-stu-id="53f0a-758">Comma-separated string of event types or null tooget all events</span></span> |
| <span data-ttu-id="53f0a-759">apiVersion</span><span class="sxs-lookup"><span data-stu-id="53f0a-759">apiVersion</span></span> |<span data-ttu-id="53f0a-760">1.0</span><span class="sxs-lookup"><span data-stu-id="53f0a-760">1.0</span></span> |
|  | |
| <span data-ttu-id="53f0a-761">Begärandetext</span><span class="sxs-lookup"><span data-stu-id="53f0a-761">Request Body</span></span> |<span data-ttu-id="53f0a-762">INGEN</span><span class="sxs-lookup"><span data-stu-id="53f0a-762">NONE</span></span> |

<span data-ttu-id="53f0a-763">**Svaret**:</span><span class="sxs-lookup"><span data-stu-id="53f0a-763">**Response**:</span></span>

<span data-ttu-id="53f0a-764">HTTP-statuskod: 200</span><span class="sxs-lookup"><span data-stu-id="53f0a-764">HTTP Status code: 200</span></span>

<span data-ttu-id="53f0a-765">En samling nyckel/värde-element.</span><span class="sxs-lookup"><span data-stu-id="53f0a-765">A collection of key/value elements.</span></span> <span data-ttu-id="53f0a-766">Var och en innehåller hello summan av händelser för en specifik händelsetyp grupperas per timme.</span><span class="sxs-lookup"><span data-stu-id="53f0a-766">Each one contains hello sum of events for a specific event type grouped by hour.</span></span>

* <span data-ttu-id="53f0a-767">`feed\entry[i]\content\properties\Key`-Innehåller hello tid (grupperade per timme) och hello händelsetyp.</span><span class="sxs-lookup"><span data-stu-id="53f0a-767">`feed\entry[i]\content\properties\Key` - Contains hello time (grouped by hour) and hello event type.</span></span>
* <span data-ttu-id="53f0a-768">`feed\entry[i]\content\properties\Value`-Totalt antal.</span><span class="sxs-lookup"><span data-stu-id="53f0a-768">`feed\entry[i]\content\properties\Value` - Total event count.</span></span>

<span data-ttu-id="53f0a-769">OData-XML</span><span class="sxs-lookup"><span data-stu-id="53f0a-769">OData XML</span></span>

    <feed xmlns:base="https://api.datamarket.azure.com/amla/recommendations/v3/GetUsageStatistics" xmlns:d="http://schemas.microsoft.com/ado/2007/08/dataservices" xmlns:m="http://schemas.microsoft.com/ado/2007/08/dataservices/metadata" xmlns="http://www.w3.org/2005/Atom">
        <title type="text" />
        <subtitle type="text">Get usage statistics</subtitle>
        <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetUsageStatistics?modelId='1d20c34f-dca1-4eac-8e5d-f299e4e4ad66'&amp;startDate='2014/10/12T00:00:00'&amp;endDate='2014/11/10T00:00:00'&amp;eventTypes=''&amp;apiVersion='1.0'</id>
    <rights type="text" />
    <updated>2014-11-18T11:39:16Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetUsageStatistics?modelId='1d20c34f-dca1-4eac-8e5d-f299e4e4ad66'&amp;startDate='2014/10/12T00:00:00'&amp;endDate='2014/11/10T00:00:00'&amp;eventTypes=''&amp;apiVersion='1.0'" />
    <entry>
        <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetUsageStatistics?modelId='1d20c34f-dca1-4eac-8e5d-f299e4e4ad66'&amp;startDate='2014/10/12T00:00:00'&amp;endDate='2014/11/10T00:00:00'&amp;eventTypes=''&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1</id>
        <title type="text">GetUsageStatisticsEntity</title>
        <updated>2014-11-18T11:39:16Z</updated>
        <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetUsageStatistics?modelId='1d20c34f-dca1-4eac-8e5d-f299e4e4ad66'&amp;startDate='2014/10/12T00:00:00'&amp;endDate='2014/11/10T00:00:00'&amp;eventTypes=''&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1" />
        <content type="application/xml">
            <m:properties>
                <d:Event m:type="Edm.String">11/9/2014 8:00:00 AM;Click</d:Event>
                <d:Value m:type="Edm.String">5</d:Value>
            </m:properties>
        </content>
    </entry>
    <entry>
        <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetUsageStatistics?modelId='1d20c34f-dca1-4eac-8e5d-f299e4e4ad66'&amp;startDate='2014/10/12T00:00:00'&amp;endDate='2014/11/10T00:00:00'&amp;eventTypes=''&amp;apiVersion='1.0'&amp;$skip=1&amp;$top=1</id>
        <title type="text">GetUsageStatisticsEntity</title>
        <updated>2014-11-18T11:39:16Z</updated>
        <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetUsageStatistics?modelId='1d20c34f-dca1-4eac-8e5d-f299e4e4ad66'&amp;startDate='2014/10/12T00:00:00'&amp;endDate='2014/11/10T00:00:00'&amp;eventTypes=''&amp;apiVersion='1.0'&amp;$skip=1&amp;$top=1" />
        <content type="application/xml">
            <m:properties>
                <d:Event m:type="Edm.String">11/9/2014 8:00:00 AM;RecommendationClick</d:Event>
                <d:Value m:type="Edm.String">10</d:Value>
              </m:properties>
        </content>
    </entry>
    <entry>
        <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetUsageStatistics?modelId='1d20c34f-dca1-4eac-8e5d-f299e4e4ad66'&amp;startDate='2014/10/12T00:00:00'&amp;endDate='2014/11/10T00:00:00'&amp;eventTypes=''&amp;apiVersion='1.0'&amp;$skip=2&amp;$top=1</id>
        <title type="text">GetUsageStatisticsEntity</title>
        <updated>2014-11-18T11:39:16Z</updated>
        <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetUsageStatistics?modelId='1d20c34f-dca1-4eac-8e5d-f299e4e4ad66'&amp;startDate='2014/10/12T00:00:00'&amp;endDate='2014/11/10T00:00:00'&amp;eventTypes=''&amp;apiVersion='1.0'&amp;$skip=2&amp;$top=1" />
        <content type="application/xml">
            <m:properties>
                <d:Event m:type="Edm.String">11/1/2014 8:00:00 AM;RemoveShopCart</d:Event>
                <d:Value m:type="Edm.String">10</d:Value>
            </m:properties>
        </content>
    </entry>
    <entry>
        <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetUsageStatistics?modelId='1d20c34f-dca1-4eac-8e5d-f299e4e4ad66'&amp;startDate='2014/10/12T00:00:00'&amp;endDate='2014/11/10T00:00:00'&amp;eventTypes=''&amp;apiVersion='1.0'&amp;$skip=3&amp;$top=1</id>
        <title type="text">GetUsageStatisticsEntity</title>
        <updated>2014-11-18T11:39:16Z</updated>
        <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetUsageStatistics?modelId='1d20c34f-dca1-4eac-8e5d-f299e4e4ad66'&amp;startDate='2014/10/12T00:00:00'&amp;endDate='2014/11/10T00:00:00'&amp;eventTypes=''&amp;apiVersion='1.0'&amp;$skip=3&amp;$top=1" />
        <content type="application/xml">
            <m:properties>
                <d:Event m:type="Edm.String">11/5/2014 8:00:00 AM;RemoveShopCart</d:Event>
                <d:Value m:type="Edm.String">10</d:Value>
            </m:properties>
        </content>
    </entry>
    </feed>

### <a name="94----get-usage-file-sample"></a><span data-ttu-id="53f0a-770">9.4.</span><span class="sxs-lookup"><span data-stu-id="53f0a-770">9.4.</span></span>    <span data-ttu-id="53f0a-771">Hämta exempel användning</span><span class="sxs-lookup"><span data-stu-id="53f0a-771">Get Usage File Sample</span></span>
<span data-ttu-id="53f0a-772">Hämtar hello första 2KB på filinnehåll för användning.</span><span class="sxs-lookup"><span data-stu-id="53f0a-772">Retrieves hello first 2KB of usage file content.</span></span>

| <span data-ttu-id="53f0a-773">HTTP-metoden</span><span class="sxs-lookup"><span data-stu-id="53f0a-773">HTTP Method</span></span> | <span data-ttu-id="53f0a-774">URI: N</span><span class="sxs-lookup"><span data-stu-id="53f0a-774">URI</span></span> |
|:--- |:--- |
| <span data-ttu-id="53f0a-775">HÄMTA</span><span class="sxs-lookup"><span data-stu-id="53f0a-775">GET</span></span> |`<rootURI>/GetUsageFileSample?modelId=%27<modelId>%27& fileId=%27<fileId>%27&apiVersion=%271.0%27`<br><br><span data-ttu-id="53f0a-776">Exempel:</span><span class="sxs-lookup"><span data-stu-id="53f0a-776">Example:</span></span><br>`<rootURI>/GetUsageFileSample?modelId=%271c1110f8-7d9f-4c64-a807-4c9c5329993a%27&fileId=%274c067b42-e975-4cb2-8c98-a6ab80ed6d63%27&apiVersion=%271.0%27` |

| <span data-ttu-id="53f0a-777">Parameternamn</span><span class="sxs-lookup"><span data-stu-id="53f0a-777">Parameter Name</span></span> | <span data-ttu-id="53f0a-778">Giltiga värden</span><span class="sxs-lookup"><span data-stu-id="53f0a-778">Valid Values</span></span> |
|:--- |:--- |
| <span data-ttu-id="53f0a-779">%{ModelID/</span><span class="sxs-lookup"><span data-stu-id="53f0a-779">modelId</span></span> |<span data-ttu-id="53f0a-780">Unik identifierare för hello modellen</span><span class="sxs-lookup"><span data-stu-id="53f0a-780">Unique identifier of hello model</span></span> |
| <span data-ttu-id="53f0a-781">fileId</span><span class="sxs-lookup"><span data-stu-id="53f0a-781">fileId</span></span> |<span data-ttu-id="53f0a-782">Unik identifierare för hello modellfil för användning</span><span class="sxs-lookup"><span data-stu-id="53f0a-782">Unique identifier of hello model usage file</span></span> |
| <span data-ttu-id="53f0a-783">apiVersion</span><span class="sxs-lookup"><span data-stu-id="53f0a-783">apiVersion</span></span> |<span data-ttu-id="53f0a-784">1.0</span><span class="sxs-lookup"><span data-stu-id="53f0a-784">1.0</span></span> |
|  | |
| <span data-ttu-id="53f0a-785">Begärandetext</span><span class="sxs-lookup"><span data-stu-id="53f0a-785">Request Body</span></span> |<span data-ttu-id="53f0a-786">INGEN</span><span class="sxs-lookup"><span data-stu-id="53f0a-786">NONE</span></span> |

<span data-ttu-id="53f0a-787">**Svaret**:</span><span class="sxs-lookup"><span data-stu-id="53f0a-787">**Response**:</span></span>

<span data-ttu-id="53f0a-788">HTTP-statuskod: 200</span><span class="sxs-lookup"><span data-stu-id="53f0a-788">HTTP Status code: 200</span></span>

<span data-ttu-id="53f0a-789">Svar returneras i raw-format:</span><span class="sxs-lookup"><span data-stu-id="53f0a-789">Response is returned in raw text format:</span></span>

<pre>
85526,2406E770-769C-4189-89DE-1C9283F93A96,2014/11/02T13:40:15,True,1
210926,2406E770-769C-4189-89DE-1C9283F93A96,2014/11/02T13:40:15,True,1
116866,2406E770-769C-4189-89DE-1C9283F93A96,2014/11/02T13:40:15,True,1
177458,2406E770-769C-4189-89DE-1C9283F93A96,2014/11/02T13:40:15,True,1
274004,21BF8088-B6C0-4509-870C-E1C7AC78304A,2014/11/02T13:40:15,True,1
123883,21BF8088-B6C0-4509-870C-E1C7AC78304A,2014/11/02T13:40:15,True,1
37712,21BF8088-B6C0-4509-870C-E1C7AC78304A,2014/11/02T13:40:15,True,1
152249,21BF8088-B6C0-4509-870C-E1C7AC78304A,2014/11/02T13:40:15,True,1
250948,21BF8088-B6C0-4509-870C-E1C7AC78304A,2014/11/02T13:40:15,True,1
235588,21BF8088-B6C0-4509-870C-E1C7AC78304A,2014/11/02T13:40:15,True,1
158254,21BF8088-B6C0-4509-870C-E1C7AC78304A,2014/11/02T13:40:15,True,1
271195,21BF8088-B6C0-4509-870C-E1C7AC78304A,2014/11/02T13:40:15,True,1
141157,21BF8088-B6C0-4509-870C-E1C7AC78304A,2014/11/02T13:40:15,True,1
171118,3BB5CB44-D143-4BDD-A55C-443964BF4B23,2014/11/02T13:40:15,True,1
225087,3BB5CB44-D143-4BDD-A55C-443964BF4B23,2014/11/02T13:40:15,True,1
</pre>


### <a name="95----get-model-usage-file"></a><span data-ttu-id="53f0a-790">9.5.</span><span class="sxs-lookup"><span data-stu-id="53f0a-790">9.5.</span></span>    <span data-ttu-id="53f0a-791">Hämta modellfil för användning</span><span class="sxs-lookup"><span data-stu-id="53f0a-791">Get Model Usage File</span></span>
<span data-ttu-id="53f0a-792">Hämtar hello fullständiga innehållet på hello användning av filen.</span><span class="sxs-lookup"><span data-stu-id="53f0a-792">Retrieves hello full content of hello usage file.</span></span>

| <span data-ttu-id="53f0a-793">HTTP-metoden</span><span class="sxs-lookup"><span data-stu-id="53f0a-793">HTTP Method</span></span> | <span data-ttu-id="53f0a-794">URI: N</span><span class="sxs-lookup"><span data-stu-id="53f0a-794">URI</span></span> |
|:--- |:--- |
| <span data-ttu-id="53f0a-795">HÄMTA</span><span class="sxs-lookup"><span data-stu-id="53f0a-795">GET</span></span> |`<rootURI>/GetModelUsageFile?mid=%27<modelId>%27& fid=%27<fileId>%27&download=%27<download_value>%27&apiVersion=%271.0%27`<br><br><span data-ttu-id="53f0a-796">Exempel:</span><span class="sxs-lookup"><span data-stu-id="53f0a-796">Example:</span></span><br>`<rootURI>/GetModelUsageFile?mid=%271c1110f8-7d9f-4c64-a807-4c9c5329993a%27&fid=%273126d816-4e80-4248-8339-1ebbdb9d544d%27&download=%271%27&apiVersion=%271.0%27` |

| <span data-ttu-id="53f0a-797">Parameternamn</span><span class="sxs-lookup"><span data-stu-id="53f0a-797">Parameter Name</span></span> | <span data-ttu-id="53f0a-798">Giltiga värden</span><span class="sxs-lookup"><span data-stu-id="53f0a-798">Valid Values</span></span> |
|:--- |:--- |
| <span data-ttu-id="53f0a-799">Mid</span><span class="sxs-lookup"><span data-stu-id="53f0a-799">mid</span></span> |<span data-ttu-id="53f0a-800">Unik identifierare för hello modellen</span><span class="sxs-lookup"><span data-stu-id="53f0a-800">Unique identifier of hello model</span></span> |
| <span data-ttu-id="53f0a-801">FID</span><span class="sxs-lookup"><span data-stu-id="53f0a-801">fid</span></span> |<span data-ttu-id="53f0a-802">Unik identifierare för hello modellfil för användning</span><span class="sxs-lookup"><span data-stu-id="53f0a-802">Unique identifier of hello model usage file</span></span> |
| <span data-ttu-id="53f0a-803">Ladda ned</span><span class="sxs-lookup"><span data-stu-id="53f0a-803">download</span></span> |<span data-ttu-id="53f0a-804">1</span><span class="sxs-lookup"><span data-stu-id="53f0a-804">1</span></span> |
| <span data-ttu-id="53f0a-805">apiVersion</span><span class="sxs-lookup"><span data-stu-id="53f0a-805">apiVersion</span></span> |<span data-ttu-id="53f0a-806">1.0</span><span class="sxs-lookup"><span data-stu-id="53f0a-806">1.0</span></span> |
|  | |
| <span data-ttu-id="53f0a-807">Begärandetext</span><span class="sxs-lookup"><span data-stu-id="53f0a-807">Request Body</span></span> |<span data-ttu-id="53f0a-808">INGEN</span><span class="sxs-lookup"><span data-stu-id="53f0a-808">NONE</span></span> |

<span data-ttu-id="53f0a-809">**Svaret**:</span><span class="sxs-lookup"><span data-stu-id="53f0a-809">**Response**:</span></span>

<span data-ttu-id="53f0a-810">HTTP-statuskod: 200</span><span class="sxs-lookup"><span data-stu-id="53f0a-810">HTTP Status code: 200</span></span>

<span data-ttu-id="53f0a-811">Svar returneras i raw-format:</span><span class="sxs-lookup"><span data-stu-id="53f0a-811">Response is returned in raw text format:</span></span>

<pre>
85526,2406E770-769C-4189-89DE-1C9283F93A96,2014/11/02T13:40:15,True,1
210926,2406E770-769C-4189-89DE-1C9283F93A96,2014/11/02T13:40:15,True,1
116866,2406E770-769C-4189-89DE-1C9283F93A96,2014/11/02T13:40:15,True,1
177458,2406E770-769C-4189-89DE-1C9283F93A96,2014/11/02T13:40:15,True,1
274004,21BF8088-B6C0-4509-870C-E1C7AC78304A,2014/11/02T13:40:15,True,1
123883,21BF8088-B6C0-4509-870C-E1C7AC78304A,2014/11/02T13:40:15,True,1
37712,21BF8088-B6C0-4509-870C-E1C7AC78304A,2014/11/02T13:40:15,True,1
152249,21BF8088-B6C0-4509-870C-E1C7AC78304A,2014/11/02T13:40:15,True,1
250948,21BF8088-B6C0-4509-870C-E1C7AC78304A,2014/11/02T13:40:15,True,1
235588,21BF8088-B6C0-4509-870C-E1C7AC78304A,2014/11/02T13:40:15,True,1
158254,21BF8088-B6C0-4509-870C-E1C7AC78304A,2014/11/02T13:40:15,True,1
271195,21BF8088-B6C0-4509-870C-E1C7AC78304A,2014/11/02T13:40:15,True,1
141157,21BF8088-B6C0-4509-870C-E1C7AC78304A,2014/11/02T13:40:15,True,1
171118,3BB5CB44-D143-4BDD-A55C-443964BF4B23,2014/11/02T13:40:15,True,1
225087,3BB5CB44-D143-4BDD-A55C-443964BF4B23,2014/11/02T13:40:15,True,1
244881,3BB5CB44-D143-4BDD-A55C-443964BF4B23,2014/11/02T13:40:15,True,1
50547,3BB5CB44-D143-4BDD-A55C-443964BF4B23,2014/11/02T13:40:15,True,1
213090,3BB5CB44-D143-4BDD-A55C-443964BF4B23,2014/11/02T13:40:15,True,1
260655,3BB5CB44-D143-4BDD-A55C-443964BF4B23,2014/11/02T13:40:15,True,1
72214,3BB5CB44-D143-4BDD-A55C-443964BF4B23,2014/11/02T13:40:15,True,1
189334,3BB5CB44-D143-4BDD-A55C-443964BF4B23,2014/11/02T13:40:15,True,1
36326,3BB5CB44-D143-4BDD-A55C-443964BF4B23,2014/11/02T13:40:15,True,1
189336,3BB5CB44-D143-4BDD-A55C-443964BF4B23,2014/11/02T13:40:15,True,1
189334,552A1940-21E4-4399-82BB-594B46D7ED54,2014/11/02T13:40:15,True,1
260655,552A1940-21E4-4399-82BB-594B46D7ED54,2014/11/02T13:40:15,True,1
162100,552A1940-21E4-4399-82BB-594B46D7ED54,2014/11/02T13:40:15,True,1
54946,552A1940-21E4-4399-82BB-594B46D7ED54,2014/11/02T13:40:15,True,1
260965,552A1940-21E4-4399-82BB-594B46D7ED54,2014/11/02T13:40:15,True,1
102758,552A1940-21E4-4399-82BB-594B46D7ED54,2014/11/02T13:40:15,True,1
112602,552A1940-21E4-4399-82BB-594B46D7ED54,2014/11/02T13:40:15,True,1
163925,552A1940-21E4-4399-82BB-594B46D7ED54,2014/11/02T13:40:15,True,1
262998,552A1940-21E4-4399-82BB-594B46D7ED54,2014/11/02T13:40:15,True,1
144717,552A1940-21E4-4399-82BB-594B46D7ED54,2014/11/02T13:40:15,True,1
</pre>

### <a name="96----delete-usage-file"></a><span data-ttu-id="53f0a-812">9.6.</span><span class="sxs-lookup"><span data-stu-id="53f0a-812">9.6.</span></span>    <span data-ttu-id="53f0a-813">Ta bort filen användning</span><span class="sxs-lookup"><span data-stu-id="53f0a-813">Delete Usage File</span></span>
<span data-ttu-id="53f0a-814">Tar bort hello angivna användning modellfil.</span><span class="sxs-lookup"><span data-stu-id="53f0a-814">Deletes hello specified model usage file.</span></span>

| <span data-ttu-id="53f0a-815">HTTP-metoden</span><span class="sxs-lookup"><span data-stu-id="53f0a-815">HTTP Method</span></span> | <span data-ttu-id="53f0a-816">URI: N</span><span class="sxs-lookup"><span data-stu-id="53f0a-816">URI</span></span> |
|:--- |:--- |
| <span data-ttu-id="53f0a-817">TA BORT</span><span class="sxs-lookup"><span data-stu-id="53f0a-817">DELETE</span></span> |`<rootURI>/DeleteUsageFile?modelId=%27<modelId>%27&fileId=%27<fileId>%27&apiVersion=%271.0%27`<br><br><span data-ttu-id="53f0a-818">Exempel:</span><span class="sxs-lookup"><span data-stu-id="53f0a-818">Example:</span></span><br>`<rootURI>/DeleteUsageFile?modelId=%270f86d698-d0f4-4406-a684-d13d22c47a73%27&fileId=%27f2e0b09d-be5c-46b2-9ac2-c7f622e5e1a5%27&apiVersion=%271.0%27` |

| <span data-ttu-id="53f0a-819">Parameternamn</span><span class="sxs-lookup"><span data-stu-id="53f0a-819">Parameter Name</span></span> | <span data-ttu-id="53f0a-820">Giltiga värden</span><span class="sxs-lookup"><span data-stu-id="53f0a-820">Valid Values</span></span> |
|:--- |:--- |
| <span data-ttu-id="53f0a-821">%{ModelID/</span><span class="sxs-lookup"><span data-stu-id="53f0a-821">modelId</span></span> |<span data-ttu-id="53f0a-822">Unik identifierare för hello modellen</span><span class="sxs-lookup"><span data-stu-id="53f0a-822">Unique identifier of hello model</span></span> |
| <span data-ttu-id="53f0a-823">fileId</span><span class="sxs-lookup"><span data-stu-id="53f0a-823">fileId</span></span> |<span data-ttu-id="53f0a-824">Unik identifierare för hello filen toobe bort</span><span class="sxs-lookup"><span data-stu-id="53f0a-824">Unique identifier of hello file toobe deleted</span></span> |
| <span data-ttu-id="53f0a-825">apiVersion</span><span class="sxs-lookup"><span data-stu-id="53f0a-825">apiVersion</span></span> |<span data-ttu-id="53f0a-826">1.0</span><span class="sxs-lookup"><span data-stu-id="53f0a-826">1.0</span></span> |
|  | |
| <span data-ttu-id="53f0a-827">Begärandetext</span><span class="sxs-lookup"><span data-stu-id="53f0a-827">Request Body</span></span> |<span data-ttu-id="53f0a-828">INGEN</span><span class="sxs-lookup"><span data-stu-id="53f0a-828">NONE</span></span> |

<span data-ttu-id="53f0a-829">**Svaret**:</span><span class="sxs-lookup"><span data-stu-id="53f0a-829">**Response**:</span></span>

<span data-ttu-id="53f0a-830">HTTP-statuskod: 200</span><span class="sxs-lookup"><span data-stu-id="53f0a-830">HTTP Status code: 200</span></span>

### <a name="97----delete-all-usage-files"></a><span data-ttu-id="53f0a-831">9.7.</span><span class="sxs-lookup"><span data-stu-id="53f0a-831">9.7.</span></span>    <span data-ttu-id="53f0a-832">Ta bort alla Användningsfiler</span><span class="sxs-lookup"><span data-stu-id="53f0a-832">Delete All Usage Files</span></span>
<span data-ttu-id="53f0a-833">Tar bort alla filer för användning av modellen.</span><span class="sxs-lookup"><span data-stu-id="53f0a-833">Deletes all model usage files.</span></span>

| <span data-ttu-id="53f0a-834">HTTP-metoden</span><span class="sxs-lookup"><span data-stu-id="53f0a-834">HTTP Method</span></span> | <span data-ttu-id="53f0a-835">URI: N</span><span class="sxs-lookup"><span data-stu-id="53f0a-835">URI</span></span> |
|:--- |:--- |
| <span data-ttu-id="53f0a-836">TA BORT</span><span class="sxs-lookup"><span data-stu-id="53f0a-836">DELETE</span></span> |`<rootURI>/DeleteAllUsageFiles?modelId=%27<modelId>%27&apiVersion=%271.0%27`<br><br><span data-ttu-id="53f0a-837">Exempel:</span><span class="sxs-lookup"><span data-stu-id="53f0a-837">Example:</span></span><br>`<rootURI>/DeleteAllUsageFiles?modelId=%271c1110f8-7d9f-4c64-a807-4c9c5329993a%27&apiVersion=%271.0%27` |

| <span data-ttu-id="53f0a-838">Parameternamn</span><span class="sxs-lookup"><span data-stu-id="53f0a-838">Parameter Name</span></span> | <span data-ttu-id="53f0a-839">Giltiga värden</span><span class="sxs-lookup"><span data-stu-id="53f0a-839">Valid Values</span></span> |
|:--- |:--- |
| <span data-ttu-id="53f0a-840">%{ModelID/</span><span class="sxs-lookup"><span data-stu-id="53f0a-840">modelId</span></span> |<span data-ttu-id="53f0a-841">Unik identifierare för hello modellen</span><span class="sxs-lookup"><span data-stu-id="53f0a-841">Unique identifier of hello model</span></span> |
| <span data-ttu-id="53f0a-842">apiVersion</span><span class="sxs-lookup"><span data-stu-id="53f0a-842">apiVersion</span></span> |<span data-ttu-id="53f0a-843">1.0</span><span class="sxs-lookup"><span data-stu-id="53f0a-843">1.0</span></span> |
|  | |
| <span data-ttu-id="53f0a-844">Begärandetext</span><span class="sxs-lookup"><span data-stu-id="53f0a-844">Request Body</span></span> |<span data-ttu-id="53f0a-845">INGEN</span><span class="sxs-lookup"><span data-stu-id="53f0a-845">NONE</span></span> |

<span data-ttu-id="53f0a-846">**Svaret**:</span><span class="sxs-lookup"><span data-stu-id="53f0a-846">**Response**:</span></span>

<span data-ttu-id="53f0a-847">HTTP-statuskod: 200</span><span class="sxs-lookup"><span data-stu-id="53f0a-847">HTTP Status code: 200</span></span>

## <a name="10-features"></a><span data-ttu-id="53f0a-848">10. Funktioner</span><span class="sxs-lookup"><span data-stu-id="53f0a-848">10. Features</span></span>
<span data-ttu-id="53f0a-849">Det här avsnittet visar hur tooretrieve funktion information, till exempel hello importeras funktioner och deras värden, deras rangordning och när den här rang allokerades.</span><span class="sxs-lookup"><span data-stu-id="53f0a-849">This section shows how tooretrieve feature information, such as hello imported features and their values, their rank, and when this rank was allocated.</span></span> <span data-ttu-id="53f0a-850">Funktioner importeras som en del av hello katalogdata och sedan deras rangordning associeras när en rank build görs.</span><span class="sxs-lookup"><span data-stu-id="53f0a-850">Features are imported as part of hello catalog data, and then their rank is associated when a rank build is done.</span></span>
<span data-ttu-id="53f0a-851">Funktionen RANG kan ändra bl.a toohello mönster för användningsdata och typen av objekt.</span><span class="sxs-lookup"><span data-stu-id="53f0a-851">Feature rank can change according toohello pattern of usage data and type of items.</span></span> <span data-ttu-id="53f0a-852">Men för konsekvent användning/artiklar hello rang bör endast små variationer.</span><span class="sxs-lookup"><span data-stu-id="53f0a-852">But for consistent usage/items, hello rank should have only small fluctuations.</span></span>
<span data-ttu-id="53f0a-853">hello rangordningen för funktioner som är ett icke-negativt tal.</span><span class="sxs-lookup"><span data-stu-id="53f0a-853">hello rank of features is a non-negative number.</span></span> <span data-ttu-id="53f0a-854">hello nummer 0 innebär att funktionen hello inte rangordnas (händer om du anropar den här API tidigare toohello slutförande av hello första rank build).</span><span class="sxs-lookup"><span data-stu-id="53f0a-854">hello  number 0 means that hello feature was not ranked (happens if you invoke this API prior toohello completion of hello first rank build).</span></span> <span data-ttu-id="53f0a-855">hello datum då hello rang har attributet kallas hello poäng dokumentens.</span><span class="sxs-lookup"><span data-stu-id="53f0a-855">hello date at which hello rank was attributed is called hello score freshness.</span></span>

### <a name="101-get-features-info-for-last-rank-build"></a><span data-ttu-id="53f0a-856">10.1.</span><span class="sxs-lookup"><span data-stu-id="53f0a-856">10.1.</span></span> <span data-ttu-id="53f0a-857">Hämta information om funktioner (för senaste Rank Build)</span><span class="sxs-lookup"><span data-stu-id="53f0a-857">Get Features Info (For Last Rank Build)</span></span>
<span data-ttu-id="53f0a-858">Hämtar hello Funktionsinformation, inklusive rangordning för hello senaste lyckade rank-version.</span><span class="sxs-lookup"><span data-stu-id="53f0a-858">Retrieves hello feature information, including ranking, for hello last successful rank build.</span></span>

| <span data-ttu-id="53f0a-859">HTTP-metoden</span><span class="sxs-lookup"><span data-stu-id="53f0a-859">HTTP Method</span></span> | <span data-ttu-id="53f0a-860">URI: N</span><span class="sxs-lookup"><span data-stu-id="53f0a-860">URI</span></span> |
|:--- |:--- |
| <span data-ttu-id="53f0a-861">HÄMTA</span><span class="sxs-lookup"><span data-stu-id="53f0a-861">GET</span></span> |`<rootURI>/GetModelFeatures?modelId=%27<modelId>%27&samplingSize=%27<samplingSize>%27&apiVersion=%271.0%27`<br><br><span data-ttu-id="53f0a-862">Exempel:</span><span class="sxs-lookup"><span data-stu-id="53f0a-862">Example:</span></span><br>`<rootURI>/GetModelFeatures?modelId=%271c1110f8-7d9f-4c64-a807-4c9c5329993a%27&samplingSize=10%27&apiVersion=%271.0%27` |

| <span data-ttu-id="53f0a-863">Parameternamn</span><span class="sxs-lookup"><span data-stu-id="53f0a-863">Parameter Name</span></span> | <span data-ttu-id="53f0a-864">Giltiga värden</span><span class="sxs-lookup"><span data-stu-id="53f0a-864">Valid Values</span></span> |
|:--- |:--- |
| <span data-ttu-id="53f0a-865">%{ModelID/</span><span class="sxs-lookup"><span data-stu-id="53f0a-865">modelId</span></span> |<span data-ttu-id="53f0a-866">Unik identifierare för hello modellen</span><span class="sxs-lookup"><span data-stu-id="53f0a-866">Unique identifier of hello model</span></span> |
| <span data-ttu-id="53f0a-867">samplingSize</span><span class="sxs-lookup"><span data-stu-id="53f0a-867">samplingSize</span></span> |<span data-ttu-id="53f0a-868">Antal värden tooinclude för varje funktion enligt toohello data som finns i hello-katalogen.</span><span class="sxs-lookup"><span data-stu-id="53f0a-868">Number of values tooinclude for each feature according toohello data present in hello catalog.</span></span> <br/><span data-ttu-id="53f0a-869">Möjliga värden:</span><span class="sxs-lookup"><span data-stu-id="53f0a-869">Possible values are:</span></span><br> <span data-ttu-id="53f0a-870">-1 - alla prover.</span><span class="sxs-lookup"><span data-stu-id="53f0a-870">-1 - All samples.</span></span> <br><span data-ttu-id="53f0a-871">0 - ingen provtagning.</span><span class="sxs-lookup"><span data-stu-id="53f0a-871">0 - No sampling.</span></span> <br><span data-ttu-id="53f0a-872">N - returnera N prover för varje funktionsnamnet.</span><span class="sxs-lookup"><span data-stu-id="53f0a-872">N - Return N samples for each feature name.</span></span> |
| <span data-ttu-id="53f0a-873">apiVersion</span><span class="sxs-lookup"><span data-stu-id="53f0a-873">apiVersion</span></span> |<span data-ttu-id="53f0a-874">1.0</span><span class="sxs-lookup"><span data-stu-id="53f0a-874">1.0</span></span> |
|  | |
| <span data-ttu-id="53f0a-875">Begärandetext</span><span class="sxs-lookup"><span data-stu-id="53f0a-875">Request Body</span></span> |<span data-ttu-id="53f0a-876">INGEN</span><span class="sxs-lookup"><span data-stu-id="53f0a-876">NONE</span></span> |

<span data-ttu-id="53f0a-877">**Svaret**:</span><span class="sxs-lookup"><span data-stu-id="53f0a-877">**Response**:</span></span>

<span data-ttu-id="53f0a-878">HTTP-statuskod: 200</span><span class="sxs-lookup"><span data-stu-id="53f0a-878">HTTP Status code: 200</span></span>

<span data-ttu-id="53f0a-879">hello svaret innehåller en lista över funktionen info transaktioner.</span><span class="sxs-lookup"><span data-stu-id="53f0a-879">hello response contains a list of feature info entries.</span></span> <span data-ttu-id="53f0a-880">Varje post innehåller:</span><span class="sxs-lookup"><span data-stu-id="53f0a-880">Each entry contains:</span></span>

* <span data-ttu-id="53f0a-881">`feed/entry/content/m:properties/d:Name`-Funktionsnamnet.</span><span class="sxs-lookup"><span data-stu-id="53f0a-881">`feed/entry/content/m:properties/d:Name` - Feature name.</span></span>
* <span data-ttu-id="53f0a-882">`feed/entry/content/m:properties/d:RankUpdateDate`-Datumet på vilka hello rang var allokerade toothis funktion, kallas även</span><span class="sxs-lookup"><span data-stu-id="53f0a-882">`feed/entry/content/m:properties/d:RankUpdateDate` - Date at which hello rank was allocated toothis feature, a.k.a.</span></span> <span data-ttu-id="53f0a-883">poäng dokumentens funktion.</span><span class="sxs-lookup"><span data-stu-id="53f0a-883">score freshness feature.</span></span> <span data-ttu-id="53f0a-884">Ett tidigare datum ('0001-01-01T00:00:00 ”) innebär att inga rank build utfördes.</span><span class="sxs-lookup"><span data-stu-id="53f0a-884">A historical date ('0001-01-01T00:00:00') means that no rank build was performed.</span></span>
* <span data-ttu-id="53f0a-885">`feed/entry/content/m:properties/d:Rank`-Funktionen RANG (flytande).</span><span class="sxs-lookup"><span data-stu-id="53f0a-885">`feed/entry/content/m:properties/d:Rank` - Feature rank (float).</span></span> <span data-ttu-id="53f0a-886">En en rangordning 2.0 och uppåt anses vara en bra funktion.</span><span class="sxs-lookup"><span data-stu-id="53f0a-886">A rank of 2.0 and up is considered a good feature.</span></span>
* <span data-ttu-id="53f0a-887">`feed/entry/content/m:properties/d:SampleValues`– Kommaavgränsad lista med värden upp toohello provtagning storlek som har begärts.</span><span class="sxs-lookup"><span data-stu-id="53f0a-887">`feed/entry/content/m:properties/d:SampleValues` - Comma-separated list of values up toohello sampling size requested.</span></span>

<span data-ttu-id="53f0a-888">OData-XML</span><span class="sxs-lookup"><span data-stu-id="53f0a-888">OData XML</span></span>

    <feed xmlns:base="https://api.datamarket.azure.com/amla/recommendations/v3/GetModelFeatures" xmlns:d="http://schemas.microsoft.com/ado/2007/08/dataservices" xmlns:m="http://schemas.microsoft.com/ado/2007/08/dataservices/metadata" xmlns="http://www.w3.org/2005/Atom">
    <title type="text" />
    <subtitle type="text">Get hello features of a model</subtitle>
    <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetModelFeatures?modelId='f13ab2e8-b530-4aa1-86f7-2f4a24714765'&amp;samplingSize='10'&amp;apiVersion='1.0'</id>
    <rights type="text" />
    <updated>2015-01-08T13:15:02Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetModelFeatures?modelId='f13ab2e8-b530-4aa1-86f7-2f4a24714765'&amp;samplingSize='10'&amp;apiVersion='1.0'" />
    <entry>
        <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetModelFeatures?modelId='f13ab2e8-b530-4aa1-86f7-2f4a24714765'&amp;samplingSize='10'&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1</id>
        <title type="text">ModelFeaturesEntity</title>
        <updated>2015-01-08T13:15:02Z</updated>
        <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetModelFeatures?modelId='f13ab2e8-b530-4aa1-86f7-2f4a24714765'&amp;samplingSize='10'&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1" />
        <content type="application/xml">
            <m:properties>
                <d:Name m:type="Edm.String">Author</d:Name>
                <d:RankUpdateDate m:type="Edm.String">0001-01-01T00:00:00</d:RankUpdateDate>
                <d:Rank m:type="Edm.String">0</d:Rank>
                <d:SampleValues m:type="Edm.String">A. A. Attanasio, A. A. Milne, A. Bates, A. C. Bhaktivedanta Swami Prabhupada et al., A. C. Crispin, A. C. Doyle, A. C. H. Smith, A. E. Parker, A. J. Holt, A. J. Matthews</d:SampleValues>
            </m:properties>
        </content>
    </entry>
    <entry>
        <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetModelFeatures?modelId='f13ab2e8-b530-4aa1-86f7-2f4a24714765'&amp;samplingSize='10'&amp;apiVersion='1.0'&amp;$skip=1&amp;$top=1</id>
        <title type="text">ModelFeaturesEntity</title>
        <updated>2015-01-08T13:15:02Z</updated>
        <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetModelFeatures?modelId='f13ab2e8-b530-4aa1-86f7-2f4a24714765'&amp;samplingSize='10'&amp;apiVersion='1.0'&amp;$skip=1&amp;$top=1" />
        <content type="application/xml">
            <m:properties>
                <d:Name m:type="Edm.String">Publisher</d:Name>
                <d:RankUpdateDate m:type="Edm.String">0001-01-01T00:00:00</d:RankUpdateDate>
                <d:Rank m:type="Edm.String">0</d:Rank>
                <d:SampleValues m:type="Edm.String">A. Mondadori, Abacus, Abacus Press, Abacus Uk, Abstract Studio, Acacia Press, Academy Chicago Publishers, Ace Books, ACE Charter, Actar</d:SampleValues>
            </m:properties>
        </content>
    </entry>
    <entry>
        <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetModelFeatures?modelId='f13ab2e8-b530-4aa1-86f7-2f4a24714765'&amp;samplingSize='10'&amp;apiVersion='1.0'&amp;$skip=2&amp;$top=1</id>
        <title type="text">ModelFeaturesEntity</title>
        <updated>2015-01-08T13:15:02Z</updated>
        <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetModelFeatures?modelId='f13ab2e8-b530-4aa1-86f7-2f4a24714765'&amp;samplingSize='10'&amp;apiVersion='1.0'&amp;$skip=2&amp;$top=1"/>
        <contenttype="application/xml">
        <m:properties>
            <d:Name m:type="Edm.String">Year</d:Name>
            <d:RankUpdateDate m:type="Edm.String">0001-01-01T00:00:00</d:RankUpdateDate>
            <d:Rank m:type="Edm.String">0</d:Rank>
            <d:SampleValues m:type="Edm.String">0, 1920, 1926, 1927, 1929, 1930, 1932, 1942, 1943, 1946</d:SampleValues>
        </m:properties>
        </content>
    </entry>
</feed>

### <a name="102-get-features-info-for-specific-rank-build"></a><span data-ttu-id="53f0a-889">10.2.</span><span class="sxs-lookup"><span data-stu-id="53f0a-889">10.2.</span></span> <span data-ttu-id="53f0a-890">Hämta information om funktioner (om en specifik Rank Build)</span><span class="sxs-lookup"><span data-stu-id="53f0a-890">Get Features Info (For Specific Rank Build)</span></span>
<span data-ttu-id="53f0a-891">Hämtar hello Funktionsinformation, inklusive hello rangordning för en specifik rank version.</span><span class="sxs-lookup"><span data-stu-id="53f0a-891">Retrieves hello feature information, including hello ranking for a specific rank build.</span></span>

| <span data-ttu-id="53f0a-892">HTTP-metoden</span><span class="sxs-lookup"><span data-stu-id="53f0a-892">HTTP Method</span></span> | <span data-ttu-id="53f0a-893">URI: N</span><span class="sxs-lookup"><span data-stu-id="53f0a-893">URI</span></span> |
|:--- |:--- |
| <span data-ttu-id="53f0a-894">HÄMTA</span><span class="sxs-lookup"><span data-stu-id="53f0a-894">GET</span></span> |`<rootURI>/GetModelFeatures?modelId=%27<modelId>%27&samplingSize=%27<samplingSize>%27&rankBuildId=<rankBuildId>&apiVersion=%271.0%27`<br><br><span data-ttu-id="53f0a-895">Exempel:</span><span class="sxs-lookup"><span data-stu-id="53f0a-895">Example:</span></span><br>`<rootURI>/GetModelFeatures?modelId=%271c1110f8-7d9f-4c64-a807-4c9c5329993a%27&samplingSize=10%27&rankBuildId=1000551&apiVersion=%271.0%27` |

| <span data-ttu-id="53f0a-896">Parameternamn</span><span class="sxs-lookup"><span data-stu-id="53f0a-896">Parameter Name</span></span> | <span data-ttu-id="53f0a-897">Giltiga värden</span><span class="sxs-lookup"><span data-stu-id="53f0a-897">Valid Values</span></span> |
|:--- |:--- |
| <span data-ttu-id="53f0a-898">%{ModelID/</span><span class="sxs-lookup"><span data-stu-id="53f0a-898">modelId</span></span> |<span data-ttu-id="53f0a-899">Unik identifierare för hello modellen</span><span class="sxs-lookup"><span data-stu-id="53f0a-899">Unique identifier of hello model</span></span> |
| <span data-ttu-id="53f0a-900">samplingSize</span><span class="sxs-lookup"><span data-stu-id="53f0a-900">samplingSize</span></span> |<span data-ttu-id="53f0a-901">Antal värden tooinclude för varje funktion enligt toohello data som finns i hello-katalogen.</span><span class="sxs-lookup"><span data-stu-id="53f0a-901">Number of values tooinclude for each feature according toohello data present in hello catalog.</span></span><br/> <span data-ttu-id="53f0a-902">Möjliga värden:</span><span class="sxs-lookup"><span data-stu-id="53f0a-902">Possible values are:</span></span><br> <span data-ttu-id="53f0a-903">-1 - alla prover.</span><span class="sxs-lookup"><span data-stu-id="53f0a-903">-1 - All samples.</span></span> <br><span data-ttu-id="53f0a-904">0 - ingen provtagning.</span><span class="sxs-lookup"><span data-stu-id="53f0a-904">0 - No sampling.</span></span> <br><span data-ttu-id="53f0a-905">N - returnera N prover för varje funktionsnamnet.</span><span class="sxs-lookup"><span data-stu-id="53f0a-905">N - Return N samples for each feature name.</span></span> |
| <span data-ttu-id="53f0a-906">rankBuildId</span><span class="sxs-lookup"><span data-stu-id="53f0a-906">rankBuildId</span></span> |<span data-ttu-id="53f0a-907">Unik identifierare för hello rank build eller -1 för hello senaste rank version</span><span class="sxs-lookup"><span data-stu-id="53f0a-907">Unique identifier of hello rank build or -1 for hello last rank build</span></span> |
| <span data-ttu-id="53f0a-908">apiVersion</span><span class="sxs-lookup"><span data-stu-id="53f0a-908">apiVersion</span></span> |<span data-ttu-id="53f0a-909">1.0</span><span class="sxs-lookup"><span data-stu-id="53f0a-909">1.0</span></span> |
|  | |
| <span data-ttu-id="53f0a-910">Begärandetext</span><span class="sxs-lookup"><span data-stu-id="53f0a-910">Request Body</span></span> |<span data-ttu-id="53f0a-911">INGEN</span><span class="sxs-lookup"><span data-stu-id="53f0a-911">NONE</span></span> |

<span data-ttu-id="53f0a-912">**Svaret**:</span><span class="sxs-lookup"><span data-stu-id="53f0a-912">**Response**:</span></span>

<span data-ttu-id="53f0a-913">HTTP-statuskod: 200</span><span class="sxs-lookup"><span data-stu-id="53f0a-913">HTTP Status code: 200</span></span>

<span data-ttu-id="53f0a-914">hello svaret innehåller en lista över funktionen info transaktioner.</span><span class="sxs-lookup"><span data-stu-id="53f0a-914">hello response contains a list of feature info entries.</span></span> <span data-ttu-id="53f0a-915">Varje post innehåller:</span><span class="sxs-lookup"><span data-stu-id="53f0a-915">Each entry contains:</span></span>

* <span data-ttu-id="53f0a-916">`feed/entry/content/m:properties/d:Name`-Funktionsnamnet.</span><span class="sxs-lookup"><span data-stu-id="53f0a-916">`feed/entry/content/m:properties/d:Name` - Feature name.</span></span>
* <span data-ttu-id="53f0a-917">`feed/entry/content/m:properties/d:RankUpdateDate`-Datumet på vilka hello rang var allokerade toothis funktion, kallas även</span><span class="sxs-lookup"><span data-stu-id="53f0a-917">`feed/entry/content/m:properties/d:RankUpdateDate` - Date at which hello rank was allocated toothis feature, a.k.a.</span></span> <span data-ttu-id="53f0a-918">poäng dokumentens funktion.</span><span class="sxs-lookup"><span data-stu-id="53f0a-918">score freshness feature.</span></span> <span data-ttu-id="53f0a-919">Ett tidigare datum ('0001-01-01T00:00:00 ”) innebär att inga rank build utfördes.</span><span class="sxs-lookup"><span data-stu-id="53f0a-919">A historical date ('0001-01-01T00:00:00') means that no rank build was performed.</span></span>
* <span data-ttu-id="53f0a-920">`feed/entry/content/m:properties/d:Rank`-Funktionen RANG (flytande).</span><span class="sxs-lookup"><span data-stu-id="53f0a-920">`feed/entry/content/m:properties/d:Rank` - Feature rank (float).</span></span> <span data-ttu-id="53f0a-921">En en rangordning 2.0 och uppåt anses vara en bra funktion.</span><span class="sxs-lookup"><span data-stu-id="53f0a-921">A rank of 2.0 and up is considered a good feature.</span></span>
* <span data-ttu-id="53f0a-922">`feed/entry/content/m:properties/d:SampleValues`– Kommaavgränsad lista med värden upp toohello provtagning storlek som har begärts.</span><span class="sxs-lookup"><span data-stu-id="53f0a-922">`feed/entry/content/m:properties/d:SampleValues` - Comma-separated list of values up toohello sampling size requested.</span></span>

<span data-ttu-id="53f0a-923">OData</span><span class="sxs-lookup"><span data-stu-id="53f0a-923">OData</span></span>

    <feed xmlns:base="https://api.datamarket.azure.com/amla/recommendations/v3/GetModelFeatures" xmlns:d="http://schemas.microsoft.com/ado/2007/08/dataservices" xmlns:m="http://schemas.microsoft.com/ado/2007/08/dataservices/metadata" xmlns="http://www.w3.org/2005/Atom">
        <title type="text" />
        <subtitle type="text">Get hello features of a model</subtitle>
        <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetModelFeatures?modelId='f13ab2e8-b530-4aa1-86f7-2f4a24714765'&amp;samplingSize='10'&amp;rankBuildId=1000653&amp;apiVersion='1.0'</id>
        <rights type="text" />
        <updated>2015-01-08T13:54:22Z</updated>
        <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetModelFeatures?modelId='f13ab2e8-b530-4aa1-86f7-2f4a24714765'&amp;samplingSize='10'&amp;rankBuildId=1000653&amp;apiVersion='1.0'" />
        <entry>
            <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetModelFeatures?modelId='f13ab2e8-b530-4aa1-86f7-2f4a24714765'&amp;samplingSize='10'&amp;rankBuildId=1000653&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1</id>
            <title type="text">ModelFeaturesEntity</title>
            <updated>2015-01-08T13:54:22Z</updated>
            <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetModelFeatures?modelId='f13ab2e8-b530-4aa1-86f7-2f4a24714765'&amp;samplingSize='10'&amp;rankBuildId=1000653&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1" />
            <content type="application/xml">
                <m:properties>
                    <d:Name m:type="Edm.String">Author</d:Name>
                    <d:RankUpdateDate m:type="Edm.String">2015-01-08T13:52:14.673</d:RankUpdateDate>
                    <d:Rank m:type="Edm.String">3.38867426</d:Rank>
                    <d:SampleValues m:type="Edm.String">A. A. Attanasio, A. A. Milne, A. Bates, A. C. Bhaktivedanta Swami Prabhupada et al., A. C. Crispin, A. C. Doyle, A. C. H. Smith, A. E. Parker, A. J. Holt, A. J. Matthews</d:SampleValues>
                </m:properties>
            </content>
        </entry>
        <entry>
            <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetModelFeatures?modelId='f13ab2e8-b530-4aa1-86f7-2f4a24714765'&amp;samplingSize='10'&amp;rankBuildId=1000653&amp;apiVersion='1.0'&amp;$skip=1&amp;$top=1</id>
            <title type="text">ModelFeaturesEntity</title>
            <updated>2015-01-08T13:54:22Z</updated>
            <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetModelFeatures?modelId='f13ab2e8-b530-4aa1-86f7-2f4a24714765'&amp;samplingSize='10'&amp;rankBuildId=1000653&amp;apiVersion='1.0'&amp;$skip=1&amp;$top=1" />
            <content type="application/xml">
                <m:properties>
                    <d:Name m:type="Edm.String">Publisher</d:Name>
                    <d:RankUpdateDate m:type="Edm.String">2015-01-08T13:52:14.673</d:RankUpdateDate>
                    <d:Rank m:type="Edm.String">1.67839336</d:Rank>
                    <d:SampleValues m:type="Edm.String">A. Mondadori, Abacus, Abacus Press, Abacus Uk, Abstract Studio, Acacia Press, Academy Chicago Publishers, Ace Books, ACE Charter, Actar</d:SampleValues>
                </m:properties>
            </content>
        </entry>
        <entry>
            <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetModelFeatures?modelId='f13ab2e8-b530-4aa1-86f7-2f4a24714765'&amp;samplingSize='10'&amp;rankBuildId=1000653&amp;apiVersion='1.0'&amp;$skip=2&amp;$top=1</id>
            <title type="text">ModelFeaturesEntity</title>
            <updated>2015-01-08T13:54:22Z</updated>
            <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetModelFeatures?modelId='f13ab2e8-b530-4aa1-86f7-2f4a24714765'&amp;samplingSize='10'&amp;rankBuildId=1000653&amp;apiVersion='1.0'&amp;$skip=2&amp;$top=1" />
            <content type="application/xml">
                <m:properties>
                    <d:Name m:type="Edm.String">Year</d:Name>
                    <d:RankUpdateDate m:type="Edm.String">2015-01-08T13:52:14.673</d:RankUpdateDate>
                    <d:Rank m:type="Edm.String">1.12352145</d:Rank>
                    <d:SampleValues m:type="Edm.String">0, 1920, 1926, 1927, 1929, 1930, 1932, 1942, 1943, 1946</d:SampleValues>
                </m:properties>
            </content>
        </entry>
    </feed>


## <a name="11-build"></a><span data-ttu-id="53f0a-924">11. Utveckla</span><span class="sxs-lookup"><span data-stu-id="53f0a-924">11. Build</span></span>
  <span data-ttu-id="53f0a-925">Det här avsnittet beskrivs hello olika API: er relaterade toobuilds.</span><span class="sxs-lookup"><span data-stu-id="53f0a-925">This section explains hello different APIs related toobuilds.</span></span> <span data-ttu-id="53f0a-926">Det finns 3 typer av versioner: en rekommendation version, en rank version och en FBT (ofta köpt tillsammans) version.</span><span class="sxs-lookup"><span data-stu-id="53f0a-926">There are 3 types of builds: a recommendation build, a rank build and an FBT (frequently bought together) build.</span></span>

<span data-ttu-id="53f0a-927">hello rekommendation build syftet är toogenerate en rekommendation modell används för förutsägelser.</span><span class="sxs-lookup"><span data-stu-id="53f0a-927">hello recommendation build's purpose is toogenerate a recommendation model used for predictions.</span></span> <span data-ttu-id="53f0a-928">Förutsägelser (för den här typen av build) förekommer i två varianter:</span><span class="sxs-lookup"><span data-stu-id="53f0a-928">Predictions (for this type of build) come in two flavors:</span></span>

* <span data-ttu-id="53f0a-929">I2I – kallas även</span><span class="sxs-lookup"><span data-stu-id="53f0a-929">I2I - a.k.a.</span></span> <span data-ttu-id="53f0a-930">Objektet tooItem rekommendationer - ett objekt eller en lista med objekt kommer det här alternativet förutsäga en lista med objekt som är sannolikt toobe högt intresse.</span><span class="sxs-lookup"><span data-stu-id="53f0a-930">Item tooItem recommendations - given an item or a list of items this option will predict a list of items that are likely toobe of high interest.</span></span>
* <span data-ttu-id="53f0a-931">U2I – kallas även</span><span class="sxs-lookup"><span data-stu-id="53f0a-931">U2I - a.k.a.</span></span> <span data-ttu-id="53f0a-932">Rekommendationerna som användaren tooItem - ges ett användar-id (och eventuellt en lista med objekt) det här alternativet kommer förutsäga en lista med objekt som är sannolikt toobe högt intresse för hello angivna användare (och dess ytterligare val av objekt).</span><span class="sxs-lookup"><span data-stu-id="53f0a-932">User tooItem recommendations - given a user id (and optionally a list of items) this option will predict a list of items that are likely toobe of high interest for hello given user (and its additional choice of items).</span></span> <span data-ttu-id="53f0a-933">Hej U2I rekommendationer baserat på hello historik över objekt som var av intresse för hello användaren driftstid toohello hello modellen har skapats.</span><span class="sxs-lookup"><span data-stu-id="53f0a-933">hello U2I recommendations are based on hello history of items that were of interest for hello user up toohello time hello model was built.</span></span>

<span data-ttu-id="53f0a-934">Rank build är en teknisk version som du kan använda toolearn om hello användbarhet funktioner.</span><span class="sxs-lookup"><span data-stu-id="53f0a-934">A rank build is a technical build that allows you toolearn about hello usefulness of your features.</span></span> <span data-ttu-id="53f0a-935">Du bör vanligtvis vidta hello följa stegen i ordning tooget hello bästa resultatet för en rekommendation modell som omfattar funktioner:</span><span class="sxs-lookup"><span data-stu-id="53f0a-935">Usually, in order tooget hello best result for a recommendation model involving features, you should take hello following steps:</span></span>

* <span data-ttu-id="53f0a-936">Utlösa en rank version (om inte hello poäng funktioner är stabil) och vänta tills du får hello funktionen poäng.</span><span class="sxs-lookup"><span data-stu-id="53f0a-936">Trigger a rank build (unless hello score of your features is stable) and wait till you get hello feature score.</span></span>
* <span data-ttu-id="53f0a-937">Hämta hello rang funktioner genom att anropa hello [visa funktioner Info](#101-get-features-info-for-last-rank-build) API.</span><span class="sxs-lookup"><span data-stu-id="53f0a-937">Retrieve hello rank of your features by calling hello [Get Features Info](#101-get-features-info-for-last-rank-build) API.</span></span>
* <span data-ttu-id="53f0a-938">Konfigurera en rekommendation version med hello följande parametrar:</span><span class="sxs-lookup"><span data-stu-id="53f0a-938">Configure a recommendation build with hello following parameters:</span></span>
  * <span data-ttu-id="53f0a-939">`useFeatureInModel`-Ange tooTrue.</span><span class="sxs-lookup"><span data-stu-id="53f0a-939">`useFeatureInModel` - Set tooTrue.</span></span>
  * <span data-ttu-id="53f0a-940">`ModelingFeatureList`-Ange tooa kommaavgränsad lista över funktioner med ett resultat på 2.0 eller flera enligt (toohello rangordningar som du hämtade i hello föregående steg).</span><span class="sxs-lookup"><span data-stu-id="53f0a-940">`ModelingFeatureList` - Set tooa comma-separated list of features with a score of 2.0 or more (according toohello ranks you retrieved in hello previous step).</span></span>
  * <span data-ttu-id="53f0a-941">`AllowColdItemPlacement`-Ange tooTrue.</span><span class="sxs-lookup"><span data-stu-id="53f0a-941">`AllowColdItemPlacement` - Set tooTrue.</span></span>
  * <span data-ttu-id="53f0a-942">Om du vill ange `EnableFeatureCorrelation` tooTrue och `ReasoningFeatureList` toohello lista över funktioner som du vill toouse förklaringar (vanligtvis hello samma lista över funktioner som används i modellering eller en underordnad lista).</span><span class="sxs-lookup"><span data-stu-id="53f0a-942">Optionally you can set `EnableFeatureCorrelation` tooTrue and `ReasoningFeatureList` toohello list of features you want toouse for explanations (usually hello same list of features used in modelling or a sublist).</span></span>
* <span data-ttu-id="53f0a-943">Utlösaren hello rekommendation build med hello konfigurerade parametrar.</span><span class="sxs-lookup"><span data-stu-id="53f0a-943">Trigger hello recommendation build with hello configured parameters.</span></span>

<span data-ttu-id="53f0a-944">Obs: Om du inte konfigurerar några parametrar (t.ex. anropa hello rekommendation build utan parametrar) eller du inte uttryckligen inaktiverar hello användning av funktioner (t.ex. `UseFeatureInModel` ange tooFalse), hello system ställer in hello funktionen-relaterade parametrar toohello beskrivs värdena ovan om det finns en rank version.</span><span class="sxs-lookup"><span data-stu-id="53f0a-944">Note: If you do not configure any parameters (e.g. invoke hello recommendation build without parameters) or you do not explicitly disable hello usage of features (e.g. `UseFeatureInModel` set tooFalse), hello system will set up hello feature-related parameters toohello explained values above in case a rank build exists.</span></span>

<span data-ttu-id="53f0a-945">Det finns ingen begränsning om hur du kör en rank version och en rekommendation skapa samtidigt för hello samma modell.</span><span class="sxs-lookup"><span data-stu-id="53f0a-945">There is no restriction on running a rank build and a recommendation build concurrently for hello same model.</span></span> <span data-ttu-id="53f0a-946">Du kan dock köra två versioner av samma typ i samma modell parallellt hello hello.</span><span class="sxs-lookup"><span data-stu-id="53f0a-946">Nevertheless, you cannot run two builds of hello same type on hello same model in parallel.</span></span>

<span data-ttu-id="53f0a-947">Bygga en FBT (ofta köpt tillsammans) ännu en annan rekommendationer algoritm kallas ibland ”konservativ” rekommenderare som är bra för kataloger som inte är likartade egenskaper (homogena: böcker, filmer, vissa mat sätt; heterogen: dator och enheter, domäner, hög olika).</span><span class="sxs-lookup"><span data-stu-id="53f0a-947">An FBT (Frequently bought together) build is yet another recommendations algorithm called sometimes "conservative" recommender, which is good for catalogs that are not homogeneous in nature (homogeneous: books, movies, some food, fashion; non-homogeneous: computer and devices, cross-domain, highly diverse).</span></span>

<span data-ttu-id="53f0a-948">Obs: om hello-användningsfiler som du överfört innehåller hello Fritextfält ”typ” för FBT modellering endast ”köp” händelser används.</span><span class="sxs-lookup"><span data-stu-id="53f0a-948">Note: if hello usage files that you uploaded contain hello optional field "event type" then for FBT modelling only "Purchase" events will be used.</span></span> <span data-ttu-id="53f0a-949">Om det finns inga händelsetyp alla händelser betraktas som köp.</span><span class="sxs-lookup"><span data-stu-id="53f0a-949">If no event type is provided all events will be considered as purchase.</span></span>

#### <a name="111-build-parameters"></a><span data-ttu-id="53f0a-950">11,1 skapa parametrar</span><span class="sxs-lookup"><span data-stu-id="53f0a-950">11.1 Build parameters</span></span>
<span data-ttu-id="53f0a-951">Varje build-typ kan konfigureras via en uppsättning parametrar (visas nedan).</span><span class="sxs-lookup"><span data-stu-id="53f0a-951">Each build type can be configured via a set of parameters (depicted below).</span></span> <span data-ttu-id="53f0a-952">Om du inte konfigurerar hello parametrar hello system automatiskt att attributet toohello parametervärden enligt toohello informationen hello tidpunkt du utlöser en version.</span><span class="sxs-lookup"><span data-stu-id="53f0a-952">If you don't configure hello parameters, hello system will automatically attribute values toohello parameters according toohello information present at hello time you trigger a build.</span></span>

##### <a name="1111-usage-condenser"></a><span data-ttu-id="53f0a-953">11.1.1.</span><span class="sxs-lookup"><span data-stu-id="53f0a-953">11.1.1.</span></span> <span data-ttu-id="53f0a-954">Användning kylaren</span><span class="sxs-lookup"><span data-stu-id="53f0a-954">Usage condenser</span></span>
<span data-ttu-id="53f0a-955">Användare eller objekt med några användning kan innehålla flera brus än information.</span><span class="sxs-lookup"><span data-stu-id="53f0a-955">Users or items with few usage points might contain more noise than information.</span></span> <span data-ttu-id="53f0a-956">hello system försöker toopredict hello minimalt antal punkter användning per användare/artikel toobe användas i en modell.</span><span class="sxs-lookup"><span data-stu-id="53f0a-956">hello system attempts toopredict hello minimal number of usage points per user/item toobe used in a model.</span></span> <span data-ttu-id="53f0a-957">Det här antalet blir inom hello-intervall som definieras av hello ItemCutoffLowerBound och ItemCutoffUpperBound parametrar för artiklar och hello-intervall som definieras av hello UserCutOffLowerBound och UserCutoffUpperBound parametrar för användare.</span><span class="sxs-lookup"><span data-stu-id="53f0a-957">This number will be within hello range defined by hello ItemCutoffLowerBound and ItemCutoffUpperBound parameters for items, and hello range defined by hello UserCutOffLowerBound and UserCutoffUpperBound parameters for users.</span></span> <span data-ttu-id="53f0a-958">hello kylaren effekt på objekt eller användare kan minimeras genom att ange minst en av hello motsvarande gränser toozero.</span><span class="sxs-lookup"><span data-stu-id="53f0a-958">hello condenser effect on items or users can be minimized by setting at least one of hello corresponding bounds toozero.</span></span>

##### <a name="1112-rank-build-parameters"></a><span data-ttu-id="53f0a-959">11.1.2.</span><span class="sxs-lookup"><span data-stu-id="53f0a-959">11.1.2.</span></span> <span data-ttu-id="53f0a-960">Rangordning skapa parametrar</span><span class="sxs-lookup"><span data-stu-id="53f0a-960">Rank build parameters</span></span>
<span data-ttu-id="53f0a-961">hello tabellen nedan visar hello build parametrar för en rank version.</span><span class="sxs-lookup"><span data-stu-id="53f0a-961">hello table below depicts hello build parameters for a rank build.</span></span>

| <span data-ttu-id="53f0a-962">Nyckel</span><span class="sxs-lookup"><span data-stu-id="53f0a-962">Key</span></span> | <span data-ttu-id="53f0a-963">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="53f0a-963">Description</span></span> | <span data-ttu-id="53f0a-964">Typ</span><span class="sxs-lookup"><span data-stu-id="53f0a-964">Type</span></span> | <span data-ttu-id="53f0a-965">Giltigt värde</span><span class="sxs-lookup"><span data-stu-id="53f0a-965">Valid Value</span></span> |
|:--- |:--- |:--- |:--- |
| <span data-ttu-id="53f0a-966">NumberOfModelIterations</span><span class="sxs-lookup"><span data-stu-id="53f0a-966">NumberOfModelIterations</span></span> |<span data-ttu-id="53f0a-967">hello antal upprepningar hello modellen presterar avspeglas av hello övergripande compute tid och hello modellen noggrannhet.</span><span class="sxs-lookup"><span data-stu-id="53f0a-967">hello number of iterations hello model performs is reflected by hello overall compute time and hello model accuracy.</span></span> <span data-ttu-id="53f0a-968">hello högre hello nummer, hello bättre noggrannhet visas, men hello beräkna tid tar det längre tid.</span><span class="sxs-lookup"><span data-stu-id="53f0a-968">hello higher hello number, hello better accuracy you will get, but hello compute time will take longer.</span></span> |<span data-ttu-id="53f0a-969">Integer</span><span class="sxs-lookup"><span data-stu-id="53f0a-969">Integer</span></span> |<span data-ttu-id="53f0a-970">10-50</span><span class="sxs-lookup"><span data-stu-id="53f0a-970">10-50</span></span> |
| <span data-ttu-id="53f0a-971">NumberOfModelDimensions</span><span class="sxs-lookup"><span data-stu-id="53f0a-971">NumberOfModelDimensions</span></span> |<span data-ttu-id="53f0a-972">hello antalet dimensioner relaterar toohello antal 'funktioner' hello modellen försöker toofind i dina data.</span><span class="sxs-lookup"><span data-stu-id="53f0a-972">hello number of dimensions relates toohello number of 'features' hello model will try toofind within your data.</span></span> <span data-ttu-id="53f0a-973">Öka hello antal dimensioner kan bättre finjustering av hello resultat i mindre kluster.</span><span class="sxs-lookup"><span data-stu-id="53f0a-973">Increasing hello number of dimensions will allow better fine-tuning of hello results into smaller clusters.</span></span> <span data-ttu-id="53f0a-974">För många dimensioner kommer dock förhindra att hello modellen från att hitta visar sambandet mellan objekt.</span><span class="sxs-lookup"><span data-stu-id="53f0a-974">However, too many dimensions will prevent hello model from finding correlations between items.</span></span> |<span data-ttu-id="53f0a-975">Integer</span><span class="sxs-lookup"><span data-stu-id="53f0a-975">Integer</span></span> |<span data-ttu-id="53f0a-976">10-40</span><span class="sxs-lookup"><span data-stu-id="53f0a-976">10-40</span></span> |
| <span data-ttu-id="53f0a-977">ItemCutOffLowerBound</span><span class="sxs-lookup"><span data-stu-id="53f0a-977">ItemCutOffLowerBound</span></span> |<span data-ttu-id="53f0a-978">Definierar hello objektet nedre gräns för hello kylaren.</span><span class="sxs-lookup"><span data-stu-id="53f0a-978">Defines hello item lower bound for hello condenser.</span></span> <span data-ttu-id="53f0a-979">Se användning kylaren ovan.</span><span class="sxs-lookup"><span data-stu-id="53f0a-979">See usage condenser above.</span></span> |<span data-ttu-id="53f0a-980">Integer</span><span class="sxs-lookup"><span data-stu-id="53f0a-980">Integer</span></span> |<span data-ttu-id="53f0a-981">2 eller högre (0 inaktivera kylaren)</span><span class="sxs-lookup"><span data-stu-id="53f0a-981">2 or more (0 disable condenser)</span></span> |
| <span data-ttu-id="53f0a-982">ItemCutOffUpperBound</span><span class="sxs-lookup"><span data-stu-id="53f0a-982">ItemCutOffUpperBound</span></span> |<span data-ttu-id="53f0a-983">Definierar hello objektet övre gränsen för hello kylaren.</span><span class="sxs-lookup"><span data-stu-id="53f0a-983">Defines hello item upper bound for hello condenser.</span></span> <span data-ttu-id="53f0a-984">Se användning kylaren ovan.</span><span class="sxs-lookup"><span data-stu-id="53f0a-984">See usage condenser above.</span></span> |<span data-ttu-id="53f0a-985">Integer</span><span class="sxs-lookup"><span data-stu-id="53f0a-985">Integer</span></span> |<span data-ttu-id="53f0a-986">2 eller högre (0 inaktivera kylaren)</span><span class="sxs-lookup"><span data-stu-id="53f0a-986">2 or more (0 disable condenser)</span></span> |
| <span data-ttu-id="53f0a-987">UserCutOffLowerBound</span><span class="sxs-lookup"><span data-stu-id="53f0a-987">UserCutOffLowerBound</span></span> |<span data-ttu-id="53f0a-988">Definierar hello användaren nedre gräns för hello kylaren.</span><span class="sxs-lookup"><span data-stu-id="53f0a-988">Defines hello user lower bound for hello condenser.</span></span> <span data-ttu-id="53f0a-989">Se användning kylaren ovan.</span><span class="sxs-lookup"><span data-stu-id="53f0a-989">See usage condenser above.</span></span> |<span data-ttu-id="53f0a-990">Integer</span><span class="sxs-lookup"><span data-stu-id="53f0a-990">Integer</span></span> |<span data-ttu-id="53f0a-991">2 eller högre (0 inaktivera kylaren)</span><span class="sxs-lookup"><span data-stu-id="53f0a-991">2 or more (0 disable condenser)</span></span> |
| <span data-ttu-id="53f0a-992">UserCutOffUpperBound</span><span class="sxs-lookup"><span data-stu-id="53f0a-992">UserCutOffUpperBound</span></span> |<span data-ttu-id="53f0a-993">Definierar hello användaren övre gränsen för hello kylaren.</span><span class="sxs-lookup"><span data-stu-id="53f0a-993">Defines hello user upper bound for hello condenser.</span></span> <span data-ttu-id="53f0a-994">Se användning kylaren ovan.</span><span class="sxs-lookup"><span data-stu-id="53f0a-994">See usage condenser above.</span></span> |<span data-ttu-id="53f0a-995">Integer</span><span class="sxs-lookup"><span data-stu-id="53f0a-995">Integer</span></span> |<span data-ttu-id="53f0a-996">2 eller högre (0 inaktivera kylaren)</span><span class="sxs-lookup"><span data-stu-id="53f0a-996">2 or more (0 disable condenser)</span></span> |

##### <a name="1113-recommendation-build-parameters"></a><span data-ttu-id="53f0a-997">11.1.3.</span><span class="sxs-lookup"><span data-stu-id="53f0a-997">11.1.3.</span></span> <span data-ttu-id="53f0a-998">Rekommendation build-parametrar</span><span class="sxs-lookup"><span data-stu-id="53f0a-998">Recommendation build parameters</span></span>
<span data-ttu-id="53f0a-999">hello tabellen nedan visar hello build parametrar för rekommendation version.</span><span class="sxs-lookup"><span data-stu-id="53f0a-999">hello table below depicts hello build parameters for recommendation build.</span></span>

| <span data-ttu-id="53f0a-1000">Nyckel</span><span class="sxs-lookup"><span data-stu-id="53f0a-1000">Key</span></span> | <span data-ttu-id="53f0a-1001">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="53f0a-1001">Description</span></span> | <span data-ttu-id="53f0a-1002">Typ</span><span class="sxs-lookup"><span data-stu-id="53f0a-1002">Type</span></span> | <span data-ttu-id="53f0a-1003">Giltigt värde</span><span class="sxs-lookup"><span data-stu-id="53f0a-1003">Valid Value</span></span> |
|:--- |:--- |:--- |:--- |
| <span data-ttu-id="53f0a-1004">NumberOfModelIterations</span><span class="sxs-lookup"><span data-stu-id="53f0a-1004">NumberOfModelIterations</span></span> |<span data-ttu-id="53f0a-1005">hello antal upprepningar hello modellen presterar avspeglas av hello övergripande compute tid och hello modellen noggrannhet.</span><span class="sxs-lookup"><span data-stu-id="53f0a-1005">hello number of iterations hello model performs is reflected by hello overall compute time and hello model accuracy.</span></span> <span data-ttu-id="53f0a-1006">hello högre hello nummer, hello bättre noggrannhet visas, men hello beräkna tid tar det längre tid.</span><span class="sxs-lookup"><span data-stu-id="53f0a-1006">hello higher hello number, hello better accuracy you will get, but hello compute time will take longer.</span></span> |<span data-ttu-id="53f0a-1007">Integer</span><span class="sxs-lookup"><span data-stu-id="53f0a-1007">Integer</span></span> |<span data-ttu-id="53f0a-1008">10-50</span><span class="sxs-lookup"><span data-stu-id="53f0a-1008">10-50</span></span> |
| <span data-ttu-id="53f0a-1009">NumberOfModelDimensions</span><span class="sxs-lookup"><span data-stu-id="53f0a-1009">NumberOfModelDimensions</span></span> |<span data-ttu-id="53f0a-1010">hello antalet dimensioner relaterar toohello antal 'funktioner' hello modellen försöker toofind i dina data.</span><span class="sxs-lookup"><span data-stu-id="53f0a-1010">hello number of dimensions relates toohello number of 'features' hello model will try toofind within your data.</span></span> <span data-ttu-id="53f0a-1011">Öka hello antal dimensioner kan bättre finjustering av hello resultat i mindre kluster.</span><span class="sxs-lookup"><span data-stu-id="53f0a-1011">Increasing hello number of dimensions will allow better fine-tuning of hello results into smaller clusters.</span></span> <span data-ttu-id="53f0a-1012">För många dimensioner kommer dock förhindra att hello modellen från att hitta visar sambandet mellan objekt.</span><span class="sxs-lookup"><span data-stu-id="53f0a-1012">However, too many dimensions will prevent hello model from finding correlations between items.</span></span> |<span data-ttu-id="53f0a-1013">Integer</span><span class="sxs-lookup"><span data-stu-id="53f0a-1013">Integer</span></span> |<span data-ttu-id="53f0a-1014">10-40</span><span class="sxs-lookup"><span data-stu-id="53f0a-1014">10-40</span></span> |
| <span data-ttu-id="53f0a-1015">ItemCutOffLowerBound</span><span class="sxs-lookup"><span data-stu-id="53f0a-1015">ItemCutOffLowerBound</span></span> |<span data-ttu-id="53f0a-1016">Definierar hello objektet nedre gräns för hello kylaren.</span><span class="sxs-lookup"><span data-stu-id="53f0a-1016">Defines hello item lower bound for hello condenser.</span></span> <span data-ttu-id="53f0a-1017">Se användning kylaren ovan.</span><span class="sxs-lookup"><span data-stu-id="53f0a-1017">See usage condenser above.</span></span> |<span data-ttu-id="53f0a-1018">Integer</span><span class="sxs-lookup"><span data-stu-id="53f0a-1018">Integer</span></span> |<span data-ttu-id="53f0a-1019">2 eller högre (0 inaktivera kylaren)</span><span class="sxs-lookup"><span data-stu-id="53f0a-1019">2 or more (0 disable condenser)</span></span> |
| <span data-ttu-id="53f0a-1020">ItemCutOffUpperBound</span><span class="sxs-lookup"><span data-stu-id="53f0a-1020">ItemCutOffUpperBound</span></span> |<span data-ttu-id="53f0a-1021">Definierar hello objektet övre gränsen för hello kylaren.</span><span class="sxs-lookup"><span data-stu-id="53f0a-1021">Defines hello item upper bound for hello condenser.</span></span> <span data-ttu-id="53f0a-1022">Se användning kylaren ovan.</span><span class="sxs-lookup"><span data-stu-id="53f0a-1022">See usage condenser above.</span></span> |<span data-ttu-id="53f0a-1023">Integer</span><span class="sxs-lookup"><span data-stu-id="53f0a-1023">Integer</span></span> |<span data-ttu-id="53f0a-1024">2 eller högre (0 inaktivera kylaren)</span><span class="sxs-lookup"><span data-stu-id="53f0a-1024">2 or more (0 disable condenser)</span></span> |
| <span data-ttu-id="53f0a-1025">UserCutOffLowerBound</span><span class="sxs-lookup"><span data-stu-id="53f0a-1025">UserCutOffLowerBound</span></span> |<span data-ttu-id="53f0a-1026">Definierar hello användaren nedre gräns för hello kylaren.</span><span class="sxs-lookup"><span data-stu-id="53f0a-1026">Defines hello user lower bound for hello condenser.</span></span> <span data-ttu-id="53f0a-1027">Se användning kylaren ovan.</span><span class="sxs-lookup"><span data-stu-id="53f0a-1027">See usage condenser above.</span></span> |<span data-ttu-id="53f0a-1028">Integer</span><span class="sxs-lookup"><span data-stu-id="53f0a-1028">Integer</span></span> |<span data-ttu-id="53f0a-1029">2 eller högre (0 inaktivera kylaren)</span><span class="sxs-lookup"><span data-stu-id="53f0a-1029">2 or more (0 disable condenser)</span></span> |
| <span data-ttu-id="53f0a-1030">UserCutOffUpperBound</span><span class="sxs-lookup"><span data-stu-id="53f0a-1030">UserCutOffUpperBound</span></span> |<span data-ttu-id="53f0a-1031">Definierar hello användaren övre gränsen för hello kylaren.</span><span class="sxs-lookup"><span data-stu-id="53f0a-1031">Defines hello user upper bound for hello condenser.</span></span> <span data-ttu-id="53f0a-1032">Se användning kylaren ovan.</span><span class="sxs-lookup"><span data-stu-id="53f0a-1032">See usage condenser above.</span></span> |<span data-ttu-id="53f0a-1033">Integer</span><span class="sxs-lookup"><span data-stu-id="53f0a-1033">Integer</span></span> |<span data-ttu-id="53f0a-1034">2 eller högre (0 inaktivera kylaren)</span><span class="sxs-lookup"><span data-stu-id="53f0a-1034">2 or more (0 disable condenser)</span></span> |
| <span data-ttu-id="53f0a-1035">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="53f0a-1035">Description</span></span> |<span data-ttu-id="53f0a-1036">Skapa beskrivning.</span><span class="sxs-lookup"><span data-stu-id="53f0a-1036">Build description.</span></span> |<span data-ttu-id="53f0a-1037">Sträng</span><span class="sxs-lookup"><span data-stu-id="53f0a-1037">String</span></span> |<span data-ttu-id="53f0a-1038">All text maximalt 512 tecken</span><span class="sxs-lookup"><span data-stu-id="53f0a-1038">Any text, maximum 512 chars</span></span> |
| <span data-ttu-id="53f0a-1039">EnableModelingInsights</span><span class="sxs-lookup"><span data-stu-id="53f0a-1039">EnableModelingInsights</span></span> |<span data-ttu-id="53f0a-1040">Tillåter toocompute mått på hello rekommendation modellen.</span><span class="sxs-lookup"><span data-stu-id="53f0a-1040">Allows you toocompute metrics on hello recommendation model.</span></span> |<span data-ttu-id="53f0a-1041">Booleskt värde</span><span class="sxs-lookup"><span data-stu-id="53f0a-1041">Boolean</span></span> |<span data-ttu-id="53f0a-1042">SANT/FALSKT</span><span class="sxs-lookup"><span data-stu-id="53f0a-1042">True/False</span></span> |
| <span data-ttu-id="53f0a-1043">UseFeaturesInModel</span><span class="sxs-lookup"><span data-stu-id="53f0a-1043">UseFeaturesInModel</span></span> |<span data-ttu-id="53f0a-1044">Anger om funktioner som kan användas i ordning tooenhance hello rekommendation modellen.</span><span class="sxs-lookup"><span data-stu-id="53f0a-1044">Indicates if features can be used in order tooenhance hello recommendation model.</span></span> |<span data-ttu-id="53f0a-1045">Booleskt värde</span><span class="sxs-lookup"><span data-stu-id="53f0a-1045">Boolean</span></span> |<span data-ttu-id="53f0a-1046">SANT/FALSKT</span><span class="sxs-lookup"><span data-stu-id="53f0a-1046">True/False</span></span> |
| <span data-ttu-id="53f0a-1047">ModelingFeatureList</span><span class="sxs-lookup"><span data-stu-id="53f0a-1047">ModelingFeatureList</span></span> |<span data-ttu-id="53f0a-1048">Kommaavgränsad lista över funktionen namn toobe används i hello rekommendation build i ordning tooenhance hello rekommendation.</span><span class="sxs-lookup"><span data-stu-id="53f0a-1048">Comma-separated list of feature names toobe used in hello recommendation build, in order tooenhance hello recommendation.</span></span> |<span data-ttu-id="53f0a-1049">Sträng</span><span class="sxs-lookup"><span data-stu-id="53f0a-1049">String</span></span> |<span data-ttu-id="53f0a-1050">Funktionsnamn in too512 tecken</span><span class="sxs-lookup"><span data-stu-id="53f0a-1050">Feature names, up too512 chars</span></span> |
| <span data-ttu-id="53f0a-1051">AllowColdItemPlacement</span><span class="sxs-lookup"><span data-stu-id="53f0a-1051">AllowColdItemPlacement</span></span> |<span data-ttu-id="53f0a-1052">Anger om hello rekommendation ska också push-installera cold objekt via funktionen likhet.</span><span class="sxs-lookup"><span data-stu-id="53f0a-1052">Indicates if hello recommendation should also push cold items via feature similarity.</span></span> |<span data-ttu-id="53f0a-1053">Booleskt värde</span><span class="sxs-lookup"><span data-stu-id="53f0a-1053">Boolean</span></span> |<span data-ttu-id="53f0a-1054">SANT/FALSKT</span><span class="sxs-lookup"><span data-stu-id="53f0a-1054">True/False</span></span> |
| <span data-ttu-id="53f0a-1055">EnableFeatureCorrelation</span><span class="sxs-lookup"><span data-stu-id="53f0a-1055">EnableFeatureCorrelation</span></span> |<span data-ttu-id="53f0a-1056">Anger om funktioner som kan användas i motivationen.</span><span class="sxs-lookup"><span data-stu-id="53f0a-1056">Indicates if features can be used in reasoning.</span></span> |<span data-ttu-id="53f0a-1057">Booleskt värde</span><span class="sxs-lookup"><span data-stu-id="53f0a-1057">Boolean</span></span> |<span data-ttu-id="53f0a-1058">SANT/FALSKT</span><span class="sxs-lookup"><span data-stu-id="53f0a-1058">True/False</span></span> |
| <span data-ttu-id="53f0a-1059">ReasoningFeatureList</span><span class="sxs-lookup"><span data-stu-id="53f0a-1059">ReasoningFeatureList</span></span> |<span data-ttu-id="53f0a-1060">Kommaavgränsad lista över funktionen namn toobe används för skäl meningar (t.ex. rekommendation förklaringar).</span><span class="sxs-lookup"><span data-stu-id="53f0a-1060">Comma-separated list of feature names toobe used for reasoning sentences (e.g. recommendation explanations).</span></span> |<span data-ttu-id="53f0a-1061">Sträng</span><span class="sxs-lookup"><span data-stu-id="53f0a-1061">String</span></span> |<span data-ttu-id="53f0a-1062">Funktionsnamn in too512 tecken</span><span class="sxs-lookup"><span data-stu-id="53f0a-1062">Feature names, up too512 chars</span></span> |
| <span data-ttu-id="53f0a-1063">EnableU2I</span><span class="sxs-lookup"><span data-stu-id="53f0a-1063">EnableU2I</span></span> |<span data-ttu-id="53f0a-1064">Tillåt hello personliga rekommendation kallas även</span><span class="sxs-lookup"><span data-stu-id="53f0a-1064">Allow hello personalized recommendation a.k.a.</span></span> <span data-ttu-id="53f0a-1065">U2I (användare tooitem rekommendationer).</span><span class="sxs-lookup"><span data-stu-id="53f0a-1065">U2I (user tooitem recommendations).</span></span> |<span data-ttu-id="53f0a-1066">Booleskt värde</span><span class="sxs-lookup"><span data-stu-id="53f0a-1066">Boolean</span></span> |<span data-ttu-id="53f0a-1067">SANT/FALSKT (standard SANT)</span><span class="sxs-lookup"><span data-stu-id="53f0a-1067">True/False (default true)</span></span> |

##### <a name="1114-fbt-build-parameters"></a><span data-ttu-id="53f0a-1068">11.1.4.</span><span class="sxs-lookup"><span data-stu-id="53f0a-1068">11.1.4.</span></span> <span data-ttu-id="53f0a-1069">FBT build-parametrar</span><span class="sxs-lookup"><span data-stu-id="53f0a-1069">FBT build parameters</span></span>
<span data-ttu-id="53f0a-1070">hello tabellen nedan visar hello build parametrar för rekommendation version.</span><span class="sxs-lookup"><span data-stu-id="53f0a-1070">hello table below depicts hello build parameters for recommendation build.</span></span>

| <span data-ttu-id="53f0a-1071">Nyckel</span><span class="sxs-lookup"><span data-stu-id="53f0a-1071">Key</span></span> | <span data-ttu-id="53f0a-1072">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="53f0a-1072">Description</span></span> | <span data-ttu-id="53f0a-1073">Typ</span><span class="sxs-lookup"><span data-stu-id="53f0a-1073">Type</span></span> | <span data-ttu-id="53f0a-1074">Giltigt värde (standard)</span><span class="sxs-lookup"><span data-stu-id="53f0a-1074">Valid Value (Default)</span></span> |
|:--- |:--- |:--- |:--- |
| <span data-ttu-id="53f0a-1075">FbtSupportThreshold</span><span class="sxs-lookup"><span data-stu-id="53f0a-1075">FbtSupportThreshold</span></span> |<span data-ttu-id="53f0a-1076">Hur konservativ hello modellen.</span><span class="sxs-lookup"><span data-stu-id="53f0a-1076">How conservative hello model is.</span></span> <span data-ttu-id="53f0a-1077">Antalet samtidigt förekomster av objekt toobe för förutsägelsemodellering.</span><span class="sxs-lookup"><span data-stu-id="53f0a-1077">Number of co-occurrences of items toobe considered for modeling.</span></span> |<span data-ttu-id="53f0a-1078">Integer</span><span class="sxs-lookup"><span data-stu-id="53f0a-1078">Integer</span></span> |<span data-ttu-id="53f0a-1079">3-50 (6)</span><span class="sxs-lookup"><span data-stu-id="53f0a-1079">3-50 (6)</span></span> |
| <span data-ttu-id="53f0a-1080">FbtMaxItemSetSize</span><span class="sxs-lookup"><span data-stu-id="53f0a-1080">FbtMaxItemSetSize</span></span> |<span data-ttu-id="53f0a-1081">Gränser hello antal objekt i en ofta.</span><span class="sxs-lookup"><span data-stu-id="53f0a-1081">Bounds hello number of items in a frequent set.</span></span> |<span data-ttu-id="53f0a-1082">Integer</span><span class="sxs-lookup"><span data-stu-id="53f0a-1082">Integer</span></span> |<span data-ttu-id="53f0a-1083">2-3 (2)</span><span class="sxs-lookup"><span data-stu-id="53f0a-1083">2-3 (2)</span></span> |
| <span data-ttu-id="53f0a-1084">FbtMinimalScore</span><span class="sxs-lookup"><span data-stu-id="53f0a-1084">FbtMinimalScore</span></span> |<span data-ttu-id="53f0a-1085">Minimal poäng som ofta set bör ha i ordning toobe ingår i hello returnerade resultat.</span><span class="sxs-lookup"><span data-stu-id="53f0a-1085">Minimal score that a frequent set should have in order toobe included in hello returned results.</span></span> <span data-ttu-id="53f0a-1086">hello högre hello bättre.</span><span class="sxs-lookup"><span data-stu-id="53f0a-1086">hello higher hello better.</span></span> |<span data-ttu-id="53f0a-1087">dubbla</span><span class="sxs-lookup"><span data-stu-id="53f0a-1087">Double</span></span> |<span data-ttu-id="53f0a-1088">0 och senare (0)</span><span class="sxs-lookup"><span data-stu-id="53f0a-1088">0 and above (0)</span></span> |
| <span data-ttu-id="53f0a-1089">FbtSimilarityFunction</span><span class="sxs-lookup"><span data-stu-id="53f0a-1089">FbtSimilarityFunction</span></span> |<span data-ttu-id="53f0a-1090">Definierar hello likhet funktionen toobe används av hello build.</span><span class="sxs-lookup"><span data-stu-id="53f0a-1090">Defines hello similarity function toobe used by hello build.</span></span> <span data-ttu-id="53f0a-1091">Lift prioriterar serendipity samtidigt förekomsten prioriterar förutsägbarhet och Jaccard är en bra kompromiss mellan hello två.</span><span class="sxs-lookup"><span data-stu-id="53f0a-1091">Lift favors serendipity, Co-occurrence favors predictability, and Jaccard is a nice compromise between hello two.</span></span> |<span data-ttu-id="53f0a-1092">Sträng</span><span class="sxs-lookup"><span data-stu-id="53f0a-1092">String</span></span> |<span data-ttu-id="53f0a-1093">cooccurrence lift, jaccard (lift)</span><span class="sxs-lookup"><span data-stu-id="53f0a-1093">cooccurrence, lift, jaccard (lift)</span></span> |

### <a name="112-trigger-a-recommendation-build"></a><span data-ttu-id="53f0a-1094">11.2.</span><span class="sxs-lookup"><span data-stu-id="53f0a-1094">11.2.</span></span> <span data-ttu-id="53f0a-1095">Utlös en rekommendation version</span><span class="sxs-lookup"><span data-stu-id="53f0a-1095">Trigger a Recommendation Build</span></span>
  <span data-ttu-id="53f0a-1096">Som standard utlöser detta API en rekommendation modellen version.</span><span class="sxs-lookup"><span data-stu-id="53f0a-1096">By default this API will trigger a recommendation model build.</span></span> <span data-ttu-id="53f0a-1097">tootrigger rangen skapa (i ordning tooscore funktioner), hello build API variant med build typparameter som ska användas.</span><span class="sxs-lookup"><span data-stu-id="53f0a-1097">tootrigger a rank build (in order tooscore  features), hello build API variant with build type parameter should be used.</span></span>

| <span data-ttu-id="53f0a-1098">HTTP-metoden</span><span class="sxs-lookup"><span data-stu-id="53f0a-1098">HTTP Method</span></span> | <span data-ttu-id="53f0a-1099">URI: N</span><span class="sxs-lookup"><span data-stu-id="53f0a-1099">URI</span></span> |
|:--- |:--- |
| <span data-ttu-id="53f0a-1100">POST</span><span class="sxs-lookup"><span data-stu-id="53f0a-1100">POST</span></span> |`<rootURI>/BuildModel?modelId=%27<modelId>%27&userDescription=%27<description>%27&apiVersion=%271.0%27`<br><br><span data-ttu-id="53f0a-1101">Exempel:</span><span class="sxs-lookup"><span data-stu-id="53f0a-1101">Example:</span></span><br>`<rootURI>/BuildModel?modelId=%27a658c626-2baa-43a7-ac98-f6ee26120a12%27&userDescription=%27First%20build%27&apiVersion=%271.0%27` |
| <span data-ttu-id="53f0a-1102">HUVUDET</span><span class="sxs-lookup"><span data-stu-id="53f0a-1102">HEADER</span></span> |<span data-ttu-id="53f0a-1103">`"Content-Type", "text/xml"`(Om skickar brödtext i begäran)</span><span class="sxs-lookup"><span data-stu-id="53f0a-1103">`"Content-Type", "text/xml"` (If sending Request Body)</span></span> |

| <span data-ttu-id="53f0a-1104">Parameternamn</span><span class="sxs-lookup"><span data-stu-id="53f0a-1104">Parameter Name</span></span> | <span data-ttu-id="53f0a-1105">Giltiga värden</span><span class="sxs-lookup"><span data-stu-id="53f0a-1105">Valid Values</span></span> |
|:--- |:--- |
| <span data-ttu-id="53f0a-1106">%{ModelID/</span><span class="sxs-lookup"><span data-stu-id="53f0a-1106">modelId</span></span> |<span data-ttu-id="53f0a-1107">Unik identifierare för hello modellen</span><span class="sxs-lookup"><span data-stu-id="53f0a-1107">Unique identifier of hello model</span></span> |
| <span data-ttu-id="53f0a-1108">userDescription</span><span class="sxs-lookup"><span data-stu-id="53f0a-1108">userDescription</span></span> |<span data-ttu-id="53f0a-1109">Textrepresentation identifierare för hello katalog.</span><span class="sxs-lookup"><span data-stu-id="53f0a-1109">Textual identifier of hello catalog.</span></span> <span data-ttu-id="53f0a-1110">Observera att om du använder lagringsutrymmen du koda den med % 20 i stället.</span><span class="sxs-lookup"><span data-stu-id="53f0a-1110">Note that if you use spaces you must encode it with %20 instead.</span></span> <span data-ttu-id="53f0a-1111">Finns i exemplet ovan.</span><span class="sxs-lookup"><span data-stu-id="53f0a-1111">See example above.</span></span><br><span data-ttu-id="53f0a-1112">Maxlängd: 50</span><span class="sxs-lookup"><span data-stu-id="53f0a-1112">Max length: 50</span></span> |
| <span data-ttu-id="53f0a-1113">apiVersion</span><span class="sxs-lookup"><span data-stu-id="53f0a-1113">apiVersion</span></span> |<span data-ttu-id="53f0a-1114">1.0</span><span class="sxs-lookup"><span data-stu-id="53f0a-1114">1.0</span></span> |
|  | |
| <span data-ttu-id="53f0a-1115">Begärandetext</span><span class="sxs-lookup"><span data-stu-id="53f0a-1115">Request Body</span></span> |<span data-ttu-id="53f0a-1116">Om den lämnas tom kommer hello build att köras med hello-standardparametrar build.</span><span class="sxs-lookup"><span data-stu-id="53f0a-1116">If left empty then hello build will be executed with hello default build parameters.</span></span><br><br><span data-ttu-id="53f0a-1117">Om du vill tooset hello build parametrar, skicka hello parametrar som XML till hello meddelandetext som i följande exempel hello.</span><span class="sxs-lookup"><span data-stu-id="53f0a-1117">If you want tooset hello build parameters, send hello parameters as XML into hello body as in hello following sample.</span></span> <span data-ttu-id="53f0a-1118">(Hello ”Build parametrar” avsnittet finns en förklaring av hello parametrar.)`<NumberOfModelIterations>40</NumberOfModelIterations><NumberOfModelDimensions>20</NumberOfModelDimensions><MinItemAppearance>5</MinItemAppearance><MinUserAppearance>5</MinUserAppearance><EnableModelingInsights>true</EnableModelingInsights><UseFeaturesInModel>false</UseFeaturesInModel><ModelingFeatureList>feature_name_1,feature_name_2,...</ModelingFeatureList><AllowColdItemPlacement>false</AllowColdItemPlacement><EnableFeatureCorrelation>false</EnableFeatureCorrelation><ReasoningFeatureList>feature_name_a,feature_name_b,...</ReasoningFeatureList></BuildParametersList>`</span><span class="sxs-lookup"><span data-stu-id="53f0a-1118">(See hello "Build parameters" section for an explanation of hello parameters.)`<NumberOfModelIterations>40</NumberOfModelIterations><NumberOfModelDimensions>20</NumberOfModelDimensions><MinItemAppearance>5</MinItemAppearance><MinUserAppearance>5</MinUserAppearance><EnableModelingInsights>true</EnableModelingInsights><UseFeaturesInModel>false</UseFeaturesInModel><ModelingFeatureList>feature_name_1,feature_name_2,...</ModelingFeatureList><AllowColdItemPlacement>false</AllowColdItemPlacement><EnableFeatureCorrelation>false</EnableFeatureCorrelation><ReasoningFeatureList>feature_name_a,feature_name_b,...</ReasoningFeatureList></BuildParametersList>`</span></span> |

<span data-ttu-id="53f0a-1119">**Svaret**:</span><span class="sxs-lookup"><span data-stu-id="53f0a-1119">**Response**:</span></span>

<span data-ttu-id="53f0a-1120">HTTP-statuskod: 200</span><span class="sxs-lookup"><span data-stu-id="53f0a-1120">HTTP Status code: 200</span></span>

<span data-ttu-id="53f0a-1121">Detta är en asynkron API.</span><span class="sxs-lookup"><span data-stu-id="53f0a-1121">This is an asynchronous API.</span></span> <span data-ttu-id="53f0a-1122">Du får ett build-ID som en respons.</span><span class="sxs-lookup"><span data-stu-id="53f0a-1122">You will get a build ID as a response.</span></span> <span data-ttu-id="53f0a-1123">tooknow när hello build har slutförts ska du anropa hello ”hämta bygger Status för en modell” API och leta upp det skapa ID hello svar.</span><span class="sxs-lookup"><span data-stu-id="53f0a-1123">tooknow when hello build has ended, you should call hello “Get Builds Status of a Model” API and locate this build ID in hello response.</span></span> <span data-ttu-id="53f0a-1124">Observera att en version kan ta från toohours minuter beroende på hello storleken på hello data.</span><span class="sxs-lookup"><span data-stu-id="53f0a-1124">Note that a build can take from minutes toohours depending on hello size of hello data.</span></span>

<span data-ttu-id="53f0a-1125">Du kan inte använda rekommendationer till hello skapa avslutas.</span><span class="sxs-lookup"><span data-stu-id="53f0a-1125">You cannot consume recommendations till hello build ends.</span></span>

<span data-ttu-id="53f0a-1126">Giltig build-status:</span><span class="sxs-lookup"><span data-stu-id="53f0a-1126">Valid build status:</span></span>

* <span data-ttu-id="53f0a-1127">Skapa - Build begäran skapades.</span><span class="sxs-lookup"><span data-stu-id="53f0a-1127">Create - Build request was created.</span></span>
* <span data-ttu-id="53f0a-1128">I kö - Build-begäran har skickats och den är i kö.</span><span class="sxs-lookup"><span data-stu-id="53f0a-1128">Queued - Build request was sent and it is queued.</span></span>
* <span data-ttu-id="53f0a-1129">Skapa - Build pågår.</span><span class="sxs-lookup"><span data-stu-id="53f0a-1129">Building - Build is in progress.</span></span>
* <span data-ttu-id="53f0a-1130">Slutfördes - Build slutförts utan fel.</span><span class="sxs-lookup"><span data-stu-id="53f0a-1130">Success - Build ended successfully.</span></span>
* <span data-ttu-id="53f0a-1131">Fel - Build avslutades med ett fel.</span><span class="sxs-lookup"><span data-stu-id="53f0a-1131">Error - Build ended with a failure.</span></span>
* <span data-ttu-id="53f0a-1132">Avbröts - avbröts Build.</span><span class="sxs-lookup"><span data-stu-id="53f0a-1132">Cancelled - Build was cancelled.</span></span>
* <span data-ttu-id="53f0a-1133">Avbryter - en begäran om avbrytning för hello version skickades.</span><span class="sxs-lookup"><span data-stu-id="53f0a-1133">Cancelling - A cancel request for hello build was sent.</span></span>

<span data-ttu-id="53f0a-1134">Observera att hello build ID finns under hello följande sökväg:`Feed\entry\content\properties\Id`</span><span class="sxs-lookup"><span data-stu-id="53f0a-1134">Note that hello build ID can be found under hello following path: `Feed\entry\content\properties\Id`</span></span>

<span data-ttu-id="53f0a-1135">OData-XML</span><span class="sxs-lookup"><span data-stu-id="53f0a-1135">OData XML</span></span>

    <feed xmlns:base="https://api.datamarket.azure.com/amla/recommendations/v3/BuildModel" xmlns:d="http://schemas.microsoft.com/ado/2007/08/dataservices" xmlns:m="http://schemas.microsoft.com/ado/2007/08/dataservices/metadata" xmlns="http://www.w3.org/2005/Atom">
      <title type="text" />
      <subtitle type="text">Build a Model with RequestBody</subtitle>
      <id>https://api.datamarket.azure.com/amla/recommendations/v3/BuildModel?modelId='9559872f-7a53-4076-a3c7-19d9385c1265'&amp;userDescription='First build'&amp;apiVersion='1.0'</id>
      <rights type="text" />
      <updated>2014-10-05T08:56:34Z</updated>
      <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/BuildModel?modelId='9559872f-7a53-4076-a3c7-19d9385c1265'&amp;userDescription='First%20build'&amp;apiVersion='1.0'" />
      <entry>
    <id>https://api.datamarket.azure.com/amla/recommendations/v3/BuildModel?modelId='9559872f-7a53-4076-a3c7-19d9385c1265'&amp;userDescription='First build'&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1</id>
    <title type="text">BuildAModelEntity2</title>
    <updated>2014-10-05T08:56:34Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/BuildModel?modelId='9559872f-7a53-4076-a3c7-19d9385c1265'&amp;userDescription='First%20build'&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1" />
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

### <a name="113-trigger-build-recommendation-rank-or-fbt"></a><span data-ttu-id="53f0a-1136">11.3.</span><span class="sxs-lookup"><span data-stu-id="53f0a-1136">11.3.</span></span> <span data-ttu-id="53f0a-1137">Utlösaren Build (rekommendation, rangordning eller FBT)</span><span class="sxs-lookup"><span data-stu-id="53f0a-1137">Trigger Build (Recommendation, Rank or FBT)</span></span>
| <span data-ttu-id="53f0a-1138">HTTP-metoden</span><span class="sxs-lookup"><span data-stu-id="53f0a-1138">HTTP Method</span></span> | <span data-ttu-id="53f0a-1139">URI: N</span><span class="sxs-lookup"><span data-stu-id="53f0a-1139">URI</span></span> |
|:--- |:--- |
| <span data-ttu-id="53f0a-1140">POST</span><span class="sxs-lookup"><span data-stu-id="53f0a-1140">POST</span></span> |`<rootURI>/BuildModel?modelId=%27<modelId>%27&userDescription=%27<description>%27&buildType=%27<buildType>%27&apiVersion=%271.0%27`<br><br><span data-ttu-id="53f0a-1141">Exempel:</span><span class="sxs-lookup"><span data-stu-id="53f0a-1141">Example:</span></span><br>`<rootURI>/BuildModel?modelId=%27a658c626-2baa-43a7-ac98-f6ee26120a12%27&userDescription=%27First%20build%27&buildType=%27Ranking%27&apiVersion=%271.0%27` |
| <span data-ttu-id="53f0a-1142">HUVUDET</span><span class="sxs-lookup"><span data-stu-id="53f0a-1142">HEADER</span></span> |<span data-ttu-id="53f0a-1143">`"Content-Type", "text/xml"`(Om skickar brödtext i begäran)</span><span class="sxs-lookup"><span data-stu-id="53f0a-1143">`"Content-Type", "text/xml"` (If sending Request Body)</span></span> |

| <span data-ttu-id="53f0a-1144">Parameternamn</span><span class="sxs-lookup"><span data-stu-id="53f0a-1144">Parameter Name</span></span> | <span data-ttu-id="53f0a-1145">Giltiga värden</span><span class="sxs-lookup"><span data-stu-id="53f0a-1145">Valid Values</span></span> |
|:--- |:--- |
| <span data-ttu-id="53f0a-1146">%{ModelID/</span><span class="sxs-lookup"><span data-stu-id="53f0a-1146">modelId</span></span> |<span data-ttu-id="53f0a-1147">Unik identifierare för hello modellen</span><span class="sxs-lookup"><span data-stu-id="53f0a-1147">Unique identifier of hello model</span></span> |
| <span data-ttu-id="53f0a-1148">userDescription</span><span class="sxs-lookup"><span data-stu-id="53f0a-1148">userDescription</span></span> |<span data-ttu-id="53f0a-1149">Textrepresentation identifierare för hello katalog.</span><span class="sxs-lookup"><span data-stu-id="53f0a-1149">Textual identifier of hello catalog.</span></span> <span data-ttu-id="53f0a-1150">Observera att om du använder lagringsutrymmen du koda den med % 20 i stället.</span><span class="sxs-lookup"><span data-stu-id="53f0a-1150">Note that if you use spaces you must encode it with %20 instead.</span></span> <span data-ttu-id="53f0a-1151">Finns i exemplet ovan.</span><span class="sxs-lookup"><span data-stu-id="53f0a-1151">See example above.</span></span><br><span data-ttu-id="53f0a-1152">Maxlängd: 50</span><span class="sxs-lookup"><span data-stu-id="53f0a-1152">Max length: 50</span></span> |
| <span data-ttu-id="53f0a-1153">buildType</span><span class="sxs-lookup"><span data-stu-id="53f0a-1153">buildType</span></span> |<span data-ttu-id="53f0a-1154">Typ av hello build tooinvoke:</span><span class="sxs-lookup"><span data-stu-id="53f0a-1154">Type of hello build tooinvoke:</span></span> <br/> <span data-ttu-id="53f0a-1155">-Rekommendation om du för rekommendation version</span><span class="sxs-lookup"><span data-stu-id="53f0a-1155">- 'Recommendation' for recommendation build</span></span> <br> <span data-ttu-id="53f0a-1156">-Rangordning för rank version</span><span class="sxs-lookup"><span data-stu-id="53f0a-1156">- 'Ranking' for rank build</span></span> <br/> <span data-ttu-id="53f0a-1157">-Fbt om du för FBT version</span><span class="sxs-lookup"><span data-stu-id="53f0a-1157">- 'Fbt' for FBT build</span></span> |
| <span data-ttu-id="53f0a-1158">apiVersion</span><span class="sxs-lookup"><span data-stu-id="53f0a-1158">apiVersion</span></span> |<span data-ttu-id="53f0a-1159">1.0</span><span class="sxs-lookup"><span data-stu-id="53f0a-1159">1.0</span></span> |
|  | |
| <span data-ttu-id="53f0a-1160">Begärandetext</span><span class="sxs-lookup"><span data-stu-id="53f0a-1160">Request Body</span></span> |<span data-ttu-id="53f0a-1161">Om den lämnas tom kommer hello build att köras med hello-standardparametrar build.</span><span class="sxs-lookup"><span data-stu-id="53f0a-1161">If left empty then hello build will be executed with hello default build parameters.</span></span><br><br><span data-ttu-id="53f0a-1162">Om du vill tooset build-parametrar måste skickas som XML till hello meddelandetext som i följande exempel hello.</span><span class="sxs-lookup"><span data-stu-id="53f0a-1162">If you want tooset build parameters, send them as XML into hello body like in hello following sample.</span></span> <span data-ttu-id="53f0a-1163">(Hello ”Build parametrar” avsnittet finns en förklaring och en fullständig lista över hello parametrar.)`<BuildParametersList><NumberOfModelIterations>40</NumberOfModelIterations><NumberOfModelDimensions>20</NumberOfModelDimensions><MinItemAppearance>5</MinItemAppearance><MinUserAppearance>5</MinUserAppearance></BuildParametersList>`</span><span class="sxs-lookup"><span data-stu-id="53f0a-1163">(See hello "Build parameters" section for an explanation and full list of hello parameters.)`<BuildParametersList><NumberOfModelIterations>40</NumberOfModelIterations><NumberOfModelDimensions>20</NumberOfModelDimensions><MinItemAppearance>5</MinItemAppearance><MinUserAppearance>5</MinUserAppearance></BuildParametersList>`</span></span> |

<span data-ttu-id="53f0a-1164">**Svaret**:</span><span class="sxs-lookup"><span data-stu-id="53f0a-1164">**Response**:</span></span>

<span data-ttu-id="53f0a-1165">HTTP-statuskod: 200</span><span class="sxs-lookup"><span data-stu-id="53f0a-1165">HTTP Status code: 200</span></span>

<span data-ttu-id="53f0a-1166">Detta är en asynkron API.</span><span class="sxs-lookup"><span data-stu-id="53f0a-1166">This is an asynchronous API.</span></span> <span data-ttu-id="53f0a-1167">Du får ett build-ID som en respons.</span><span class="sxs-lookup"><span data-stu-id="53f0a-1167">You will get a build ID as a response.</span></span> <span data-ttu-id="53f0a-1168">tooknow när hello build har slutförts ska du anropa hello ”hämta bygger Status för en modell” API och leta upp det skapa ID hello svar.</span><span class="sxs-lookup"><span data-stu-id="53f0a-1168">tooknow when hello build has ended, you should call hello “Get Builds Status of a Model” API and locate this build ID in hello response.</span></span> <span data-ttu-id="53f0a-1169">Observera att en version kan ta från toohours minuter beroende på hello storleken på hello data.</span><span class="sxs-lookup"><span data-stu-id="53f0a-1169">Note that a build can take from minutes toohours depending on hello size of hello data.</span></span>

<span data-ttu-id="53f0a-1170">Du kan inte använda rekommendationer till hello skapa avslutas.</span><span class="sxs-lookup"><span data-stu-id="53f0a-1170">You cannot consume recommendations till hello build ends.</span></span>

<span data-ttu-id="53f0a-1171">Giltig build-status:</span><span class="sxs-lookup"><span data-stu-id="53f0a-1171">Valid build status:</span></span>

* <span data-ttu-id="53f0a-1172">Skapa - modellen har skapats.</span><span class="sxs-lookup"><span data-stu-id="53f0a-1172">Create - Model was created.</span></span>
* <span data-ttu-id="53f0a-1173">I kö - modellen build utlöstes och den är i kö.</span><span class="sxs-lookup"><span data-stu-id="53f0a-1173">Queued - Model build was triggered and it is queued.</span></span>
* <span data-ttu-id="53f0a-1174">Skapa - modellen skapas.</span><span class="sxs-lookup"><span data-stu-id="53f0a-1174">Building - Model is being built.</span></span>
* <span data-ttu-id="53f0a-1175">Slutfördes - Build slutförts utan fel.</span><span class="sxs-lookup"><span data-stu-id="53f0a-1175">Success - Build ended successfully.</span></span>
* <span data-ttu-id="53f0a-1176">Fel - Build avslutades med ett fel.</span><span class="sxs-lookup"><span data-stu-id="53f0a-1176">Error - Build ended with a failure.</span></span>
* <span data-ttu-id="53f0a-1177">Avbröts - avbröts Build.</span><span class="sxs-lookup"><span data-stu-id="53f0a-1177">Cancelled - Build was cancelled.</span></span>
* <span data-ttu-id="53f0a-1178">Avbryter - Build avbryts.</span><span class="sxs-lookup"><span data-stu-id="53f0a-1178">Cancelling - Build is being cancelled.</span></span>

<span data-ttu-id="53f0a-1179">Observera att hello build ID finns under hello följande sökväg:`Feed\entry\content\properties\Id`</span><span class="sxs-lookup"><span data-stu-id="53f0a-1179">Note that hello build ID can be found under hello following path: `Feed\entry\content\properties\Id`</span></span>

<span data-ttu-id="53f0a-1180">OData-XML</span><span class="sxs-lookup"><span data-stu-id="53f0a-1180">OData XML</span></span>

    <feed xmlns:base="https://api.datamarket.azure.com/amla/recommendations/v3/BuildModel" xmlns:d="http://schemas.microsoft.com/ado/2007/08/dataservices" xmlns:m="http://schemas.microsoft.com/ado/2007/08/dataservices/metadata" xmlns="http://www.w3.org/2005/Atom">
      <title type="text" />
      <subtitle type="text">Build a Model with RequestBody</subtitle>
      <id>https://api.datamarket.azure.com/amla/recommendations/v3/BuildModel?modelId='9559872f-7a53-4076-a3c7-19d9385c1265'&amp;userDescription='First build'&amp;apiVersion='1.0'</id>
      <rights type="text" />
      <updated>2014-10-05T08:56:34Z</updated>
      <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/BuildModel?modelId='9559872f-7a53-4076-a3c7-19d9385c1265'&amp;userDescription='First%20build'&amp;apiVersion='1.0'" />
      <entry>
    <id>https://api.datamarket.azure.com/amla/recommendations/v3/BuildModel?modelId='9559872f-7a53-4076-a3c7-19d9385c1265'&amp;userDescription='First build'&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1</id>
    <title type="text">BuildAModelEntity2</title>
    <updated>2014-10-05T08:56:34Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/BuildModel?modelId='9559872f-7a53-4076-a3c7-19d9385c1265'&amp;userDescription='First%20build'&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1" />
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




### <a name="114-get-builds-status-of-a-model"></a><span data-ttu-id="53f0a-1181">11.4.</span><span class="sxs-lookup"><span data-stu-id="53f0a-1181">11.4.</span></span> <span data-ttu-id="53f0a-1182">Hämta Status för versioner av en modell</span><span class="sxs-lookup"><span data-stu-id="53f0a-1182">Get Builds Status of a Model</span></span>
<span data-ttu-id="53f0a-1183">Hämtar versioner och deras status för en angiven modell.</span><span class="sxs-lookup"><span data-stu-id="53f0a-1183">Retrieves builds and their status for a specified model.</span></span>

| <span data-ttu-id="53f0a-1184">HTTP-metoden</span><span class="sxs-lookup"><span data-stu-id="53f0a-1184">HTTP Method</span></span> | <span data-ttu-id="53f0a-1185">URI: N</span><span class="sxs-lookup"><span data-stu-id="53f0a-1185">URI</span></span> |
|:--- |:--- |
| <span data-ttu-id="53f0a-1186">HÄMTA</span><span class="sxs-lookup"><span data-stu-id="53f0a-1186">GET</span></span> |`<rootURI>/GetModelBuildsStatus?modelId=%27<modelId>%27&onlyLastBuild=<bool>&apiVersion=%271.0%27`<br><br><span data-ttu-id="53f0a-1187">Exempel:</span><span class="sxs-lookup"><span data-stu-id="53f0a-1187">Example:</span></span><br>`<rootURI>/GetModelBuildsStatus?modelId=%279559872f-7a53-4076-a3c7-19d9385c1265%27&onlyLastBuild=true&apiVersion=%271.0%27` |

| <span data-ttu-id="53f0a-1188">Parameternamn</span><span class="sxs-lookup"><span data-stu-id="53f0a-1188">Parameter Name</span></span> | <span data-ttu-id="53f0a-1189">Giltiga värden</span><span class="sxs-lookup"><span data-stu-id="53f0a-1189">Valid Values</span></span> |
|:--- |:--- |
| <span data-ttu-id="53f0a-1190">%{ModelID/</span><span class="sxs-lookup"><span data-stu-id="53f0a-1190">modelId</span></span> |<span data-ttu-id="53f0a-1191">Unik identifierare för hello modellen</span><span class="sxs-lookup"><span data-stu-id="53f0a-1191">Unique identifier of hello model</span></span> |
| <span data-ttu-id="53f0a-1192">onlyLastBuild</span><span class="sxs-lookup"><span data-stu-id="53f0a-1192">onlyLastBuild</span></span> |<span data-ttu-id="53f0a-1193">Anger om tooreturn alla hello skapa historik över hello modell eller endast hello status hello senaste version</span><span class="sxs-lookup"><span data-stu-id="53f0a-1193">Indicates whether tooreturn all hello build history of hello model or only hello status of hello most recent build</span></span> |
| <span data-ttu-id="53f0a-1194">apiVersion</span><span class="sxs-lookup"><span data-stu-id="53f0a-1194">apiVersion</span></span> |<span data-ttu-id="53f0a-1195">1.0</span><span class="sxs-lookup"><span data-stu-id="53f0a-1195">1.0</span></span> |

<span data-ttu-id="53f0a-1196">**Svaret**:</span><span class="sxs-lookup"><span data-stu-id="53f0a-1196">**Response**:</span></span>

<span data-ttu-id="53f0a-1197">HTTP-statuskod: 200</span><span class="sxs-lookup"><span data-stu-id="53f0a-1197">HTTP Status code: 200</span></span>

<span data-ttu-id="53f0a-1198">hello svaret innehåller en post per version.</span><span class="sxs-lookup"><span data-stu-id="53f0a-1198">hello response includes one entry per build.</span></span> <span data-ttu-id="53f0a-1199">Varje post innehåller hello följande data:</span><span class="sxs-lookup"><span data-stu-id="53f0a-1199">Each entry has hello following data:</span></span>

* <span data-ttu-id="53f0a-1200">`feed/entry/content/properties/UserName`-Namnet på hello användare.</span><span class="sxs-lookup"><span data-stu-id="53f0a-1200">`feed/entry/content/properties/UserName` - Name of hello user.</span></span>
* <span data-ttu-id="53f0a-1201">`feed/entry/content/properties/ModelName`-Namnet på hello modell.</span><span class="sxs-lookup"><span data-stu-id="53f0a-1201">`feed/entry/content/properties/ModelName` - Name of hello model.</span></span>
* <span data-ttu-id="53f0a-1202">`feed/entry/content/properties/ModelId`-Modellen Unik identifierare.</span><span class="sxs-lookup"><span data-stu-id="53f0a-1202">`feed/entry/content/properties/ModelId` - Model unique identifier.</span></span>
* <span data-ttu-id="53f0a-1203">`feed/entry/content/properties/IsDeployed`– Om hello anmärkningar distribueras (kallas även</span><span class="sxs-lookup"><span data-stu-id="53f0a-1203">`feed/entry/content/properties/IsDeployed` - Whether hello build is deployed (a.k.a.</span></span> <span data-ttu-id="53f0a-1204">aktiva build).</span><span class="sxs-lookup"><span data-stu-id="53f0a-1204">active build).</span></span>
* <span data-ttu-id="53f0a-1205">`feed/entry/content/properties/BuildId`-Skapa Unik identifierare.</span><span class="sxs-lookup"><span data-stu-id="53f0a-1205">`feed/entry/content/properties/BuildId` - Build unique identifier.</span></span>
* <span data-ttu-id="53f0a-1206">`feed/entry/content/properties/BuildType`-Typen av hello build.</span><span class="sxs-lookup"><span data-stu-id="53f0a-1206">`feed/entry/content/properties/BuildType` - Type of hello build.</span></span>
* <span data-ttu-id="53f0a-1207">`feed/entry/content/properties/Status`-Skapa status.</span><span class="sxs-lookup"><span data-stu-id="53f0a-1207">`feed/entry/content/properties/Status` - Build status.</span></span> <span data-ttu-id="53f0a-1208">Kan vara något av följande hello: fel, skapa, i kö, Cancelling, avbrott, lyckas.</span><span class="sxs-lookup"><span data-stu-id="53f0a-1208">Can be one of hello following: Error, Building, Queued, Cancelling, Cancelled, Success.</span></span>
* <span data-ttu-id="53f0a-1209">`feed/entry/content/properties/StatusMessage`-Det detaljerade statusmeddelanden (gäller endast toospecific tillstånd).</span><span class="sxs-lookup"><span data-stu-id="53f0a-1209">`feed/entry/content/properties/StatusMessage` - Detailed status message (applies only toospecific states).</span></span>
* <span data-ttu-id="53f0a-1210">`feed/entry/content/properties/Progress`-Skapa förlopp (%).</span><span class="sxs-lookup"><span data-stu-id="53f0a-1210">`feed/entry/content/properties/Progress` - Build progress (%).</span></span>
* <span data-ttu-id="53f0a-1211">`feed/entry/content/properties/StartTime`-Skapa starttid.</span><span class="sxs-lookup"><span data-stu-id="53f0a-1211">`feed/entry/content/properties/StartTime` - Build start time.</span></span>
* <span data-ttu-id="53f0a-1212">`feed/entry/content/properties/EndTime`-Skapa sluttid.</span><span class="sxs-lookup"><span data-stu-id="53f0a-1212">`feed/entry/content/properties/EndTime` - Build end time.</span></span>
* <span data-ttu-id="53f0a-1213">`feed/entry/content/properties/ExecutionTime`-Skapa varaktighet.</span><span class="sxs-lookup"><span data-stu-id="53f0a-1213">`feed/entry/content/properties/ExecutionTime` - Build duration.</span></span>
* <span data-ttu-id="53f0a-1214">`feed/entry/content/properties/ProgressStep`-Information om hello aktuella etappen en pågår.</span><span class="sxs-lookup"><span data-stu-id="53f0a-1214">`feed/entry/content/properties/ProgressStep` - Details about hello current stage of a build in progress.</span></span>

<span data-ttu-id="53f0a-1215">Giltig build-status:</span><span class="sxs-lookup"><span data-stu-id="53f0a-1215">Valid build status:</span></span>

* <span data-ttu-id="53f0a-1216">Skapa - skapades Build posten.</span><span class="sxs-lookup"><span data-stu-id="53f0a-1216">Created - Build request entry was created.</span></span>
* <span data-ttu-id="53f0a-1217">I kö - Build begäran utlöstes och den är i kö.</span><span class="sxs-lookup"><span data-stu-id="53f0a-1217">Queued - Build request was triggered and it is queued.</span></span>
* <span data-ttu-id="53f0a-1218">Skapa - Build pågår.</span><span class="sxs-lookup"><span data-stu-id="53f0a-1218">Building - Build is in process.</span></span>
* <span data-ttu-id="53f0a-1219">Slutfördes - Build slutförts utan fel.</span><span class="sxs-lookup"><span data-stu-id="53f0a-1219">Success - Build ended successfully.</span></span>
* <span data-ttu-id="53f0a-1220">Fel - Build avslutades med ett fel.</span><span class="sxs-lookup"><span data-stu-id="53f0a-1220">Error - Build ended with a failure.</span></span>
* <span data-ttu-id="53f0a-1221">Avbröts - avbröts Build.</span><span class="sxs-lookup"><span data-stu-id="53f0a-1221">Cancelled - Build was cancelled.</span></span>
* <span data-ttu-id="53f0a-1222">Avbryter - Build avbryts.</span><span class="sxs-lookup"><span data-stu-id="53f0a-1222">Cancelling - Build is being cancelled.</span></span>

<span data-ttu-id="53f0a-1223">Giltiga värden för build-typ:</span><span class="sxs-lookup"><span data-stu-id="53f0a-1223">Valid values for build type:</span></span>

* <span data-ttu-id="53f0a-1224">Rangordning - skapa rang.</span><span class="sxs-lookup"><span data-stu-id="53f0a-1224">Rank - Rank build.</span></span>
* <span data-ttu-id="53f0a-1225">Rekommendation - rekommendation build.</span><span class="sxs-lookup"><span data-stu-id="53f0a-1225">Recommendation - Recommendation build.</span></span>

<span data-ttu-id="53f0a-1226">OData-XML</span><span class="sxs-lookup"><span data-stu-id="53f0a-1226">OData XML</span></span>

    <feed xmlns:base="https://api.datamarket.azure.com/amla/recommendations/v3/GetModelBuildsStatus" xmlns:d="http://schemas.microsoft.com/ado/2007/08/dataservices" xmlns:m="http://schemas.microsoft.com/ado/2007/08/dataservices/metadata" xmlns="http://www.w3.org/2005/Atom">
        <title type="text" />
        <subtitle type="text">Get builds status of a model</subtitle>
        <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetModelBuildsStatus?modelId='1d20c34f-dca1-4eac-8e5d-f299e4e4ad66'&amp;onlyLastBuild=False&amp;apiVersion='1.0'</id>
        <rights type="text" />
        <updated>2014-11-05T17:51:10Z</updated>
        <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetModelBuildsStatus?modelId='1d20c34f-dca1-4eac-8e5d-f299e4e4ad66'&amp;onlyLastBuild=False&amp;apiVersion='1.0'" />
        <entry>
            <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetModelBuildsStatus?modelId='1d20c34f-dca1-4eac-8e5d-f299e4e4ad66'&amp;onlyLastBuild=False&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1</id>
            <title type="text">GetBuildsStatusEntity</title>
            <updated>2014-11-05T17:51:10Z</updated>
            <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetModelBuildsStatus?modelId='1d20c34f-dca1-4eac-8e5d-f299e4e4ad66'&amp;onlyLastBuild=False&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1" />
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


### <a name="115-get-builds-status"></a><span data-ttu-id="53f0a-1227">11.5.</span><span class="sxs-lookup"><span data-stu-id="53f0a-1227">11.5.</span></span> <span data-ttu-id="53f0a-1228">Hämta Status för versioner</span><span class="sxs-lookup"><span data-stu-id="53f0a-1228">Get Builds Status</span></span>
<span data-ttu-id="53f0a-1229">Hämtar skapa status för alla modeller för en användare.</span><span class="sxs-lookup"><span data-stu-id="53f0a-1229">Retrieves build statuses of all models of a user.</span></span>

| <span data-ttu-id="53f0a-1230">HTTP-metoden</span><span class="sxs-lookup"><span data-stu-id="53f0a-1230">HTTP Method</span></span> | <span data-ttu-id="53f0a-1231">URI: N</span><span class="sxs-lookup"><span data-stu-id="53f0a-1231">URI</span></span> |
|:--- |:--- |
| <span data-ttu-id="53f0a-1232">HÄMTA</span><span class="sxs-lookup"><span data-stu-id="53f0a-1232">GET</span></span> |`<rootURI>/GetUserBuildsStatus?onlyLastBuilds=<bool>&apiVersion=%271.0%27`<br><br><span data-ttu-id="53f0a-1233">Exempel:</span><span class="sxs-lookup"><span data-stu-id="53f0a-1233">Example:</span></span><br>`<rootURI>/GetUserBuildsStatus?onlyLastBuilds=true&apiVersion=%271.0%27` |

| <span data-ttu-id="53f0a-1234">Parameternamn</span><span class="sxs-lookup"><span data-stu-id="53f0a-1234">Parameter Name</span></span> | <span data-ttu-id="53f0a-1235">Giltiga värden</span><span class="sxs-lookup"><span data-stu-id="53f0a-1235">Valid Values</span></span> |
|:--- |:--- |
| <span data-ttu-id="53f0a-1236">onlyLastBuild</span><span class="sxs-lookup"><span data-stu-id="53f0a-1236">onlyLastBuild</span></span> |<span data-ttu-id="53f0a-1237">Anger om alla hello tooreturn skapa historik över hello modell eller endast hello status hello den senaste versionen.</span><span class="sxs-lookup"><span data-stu-id="53f0a-1237">Indicates whether tooreturn all hello build history of hello model or only hello status of hello most recent build.</span></span> |
| <span data-ttu-id="53f0a-1238">apiVersion</span><span class="sxs-lookup"><span data-stu-id="53f0a-1238">apiVersion</span></span> |<span data-ttu-id="53f0a-1239">1.0</span><span class="sxs-lookup"><span data-stu-id="53f0a-1239">1.0</span></span> |

<span data-ttu-id="53f0a-1240">**Svaret**:</span><span class="sxs-lookup"><span data-stu-id="53f0a-1240">**Response**:</span></span>

<span data-ttu-id="53f0a-1241">HTTP-statuskod: 200</span><span class="sxs-lookup"><span data-stu-id="53f0a-1241">HTTP Status code: 200</span></span>

<span data-ttu-id="53f0a-1242">hello svaret innehåller en post per version.</span><span class="sxs-lookup"><span data-stu-id="53f0a-1242">hello response includes one entry per build.</span></span> <span data-ttu-id="53f0a-1243">Varje post innehåller hello följande data:</span><span class="sxs-lookup"><span data-stu-id="53f0a-1243">Each entry has hello following data:</span></span>

* <span data-ttu-id="53f0a-1244">`feed/entry/content/properties/UserName`-Namnet på hello användare.</span><span class="sxs-lookup"><span data-stu-id="53f0a-1244">`feed/entry/content/properties/UserName` - Name of hello user.</span></span>
* <span data-ttu-id="53f0a-1245">`feed/entry/content/properties/ModelName`-Namnet på hello modell.</span><span class="sxs-lookup"><span data-stu-id="53f0a-1245">`feed/entry/content/properties/ModelName` - Name of hello model.</span></span>
* <span data-ttu-id="53f0a-1246">`feed/entry/content/properties/ModelId`-Modellen Unik identifierare.</span><span class="sxs-lookup"><span data-stu-id="53f0a-1246">`feed/entry/content/properties/ModelId` - Model unique identifier.</span></span>
* <span data-ttu-id="53f0a-1247">`feed/entry/content/properties/IsDeployed`– Om hello anmärkningar distribueras.</span><span class="sxs-lookup"><span data-stu-id="53f0a-1247">`feed/entry/content/properties/IsDeployed` - Whether hello build is deployed.</span></span>
* <span data-ttu-id="53f0a-1248">`feed/entry/content/properties/BuildId`-Skapa Unik identifierare.</span><span class="sxs-lookup"><span data-stu-id="53f0a-1248">`feed/entry/content/properties/BuildId` - Build unique identifier.</span></span>
* <span data-ttu-id="53f0a-1249">`feed/entry/content/properties/BuildType`-Typen av hello build.</span><span class="sxs-lookup"><span data-stu-id="53f0a-1249">`feed/entry/content/properties/BuildType` - Type of hello build.</span></span>
* <span data-ttu-id="53f0a-1250">`feed/entry/content/properties/Status`-Skapa status.</span><span class="sxs-lookup"><span data-stu-id="53f0a-1250">`feed/entry/content/properties/Status` - Build status.</span></span> <span data-ttu-id="53f0a-1251">Kan vara något av följande hello: fel, skapa, i kö, avbrott, Cancelling, lyckas.</span><span class="sxs-lookup"><span data-stu-id="53f0a-1251">Can be one of hello following: Error, Building, Queued, Cancelled, Cancelling, Success.</span></span>
* <span data-ttu-id="53f0a-1252">`feed/entry/content/properties/StatusMessage`-Det detaljerade statusmeddelanden (gäller endast toospecific tillstånd).</span><span class="sxs-lookup"><span data-stu-id="53f0a-1252">`feed/entry/content/properties/StatusMessage` - Detailed status message (applies only toospecific states).</span></span>
* <span data-ttu-id="53f0a-1253">`feed/entry/content/properties/Progress`-Skapa förlopp (%).</span><span class="sxs-lookup"><span data-stu-id="53f0a-1253">`feed/entry/content/properties/Progress` - Build progress (%).</span></span>
* <span data-ttu-id="53f0a-1254">`feed/entry/content/properties/StartTime`-Skapa starttid.</span><span class="sxs-lookup"><span data-stu-id="53f0a-1254">`feed/entry/content/properties/StartTime` - Build start time.</span></span>
* <span data-ttu-id="53f0a-1255">`feed/entry/content/properties/EndTime`-Skapa sluttid.</span><span class="sxs-lookup"><span data-stu-id="53f0a-1255">`feed/entry/content/properties/EndTime` - Build end time.</span></span>
* <span data-ttu-id="53f0a-1256">`feed/entry/content/properties/ExecutionTime`-Skapa varaktighet.</span><span class="sxs-lookup"><span data-stu-id="53f0a-1256">`feed/entry/content/properties/ExecutionTime` - Build duration.</span></span>
* <span data-ttu-id="53f0a-1257">`feed/entry/content/properties/ProgressStep`-Information om hello aktuella etappen en pågår.</span><span class="sxs-lookup"><span data-stu-id="53f0a-1257">`feed/entry/content/properties/ProgressStep` - Details about hello current stage of a build in progress.</span></span>

<span data-ttu-id="53f0a-1258">Giltig build-status:</span><span class="sxs-lookup"><span data-stu-id="53f0a-1258">Valid build status:</span></span>

* <span data-ttu-id="53f0a-1259">Skapa - skapades Build posten.</span><span class="sxs-lookup"><span data-stu-id="53f0a-1259">Created - Build request entry was created.</span></span>
* <span data-ttu-id="53f0a-1260">I kö - Build begäran utlöstes och den är i kö.</span><span class="sxs-lookup"><span data-stu-id="53f0a-1260">Queued - Build request was triggered and it is queued.</span></span>
* <span data-ttu-id="53f0a-1261">Skapa - Build pågår.</span><span class="sxs-lookup"><span data-stu-id="53f0a-1261">Building - Build is in process.</span></span>
* <span data-ttu-id="53f0a-1262">Slutfördes - Build slutförts utan fel.</span><span class="sxs-lookup"><span data-stu-id="53f0a-1262">Success - Build ended successfully.</span></span>
* <span data-ttu-id="53f0a-1263">Fel - Build avslutades med ett fel.</span><span class="sxs-lookup"><span data-stu-id="53f0a-1263">Error - Build ended with a failure.</span></span>
* <span data-ttu-id="53f0a-1264">Avbröts - avbröts Build.</span><span class="sxs-lookup"><span data-stu-id="53f0a-1264">Cancelled - Build was cancelled.</span></span>
* <span data-ttu-id="53f0a-1265">Avbryter - Build avbryts.</span><span class="sxs-lookup"><span data-stu-id="53f0a-1265">Cancelling - Build is being cancelled.</span></span>

<span data-ttu-id="53f0a-1266">Giltiga värden för build-typ:</span><span class="sxs-lookup"><span data-stu-id="53f0a-1266">Valid values for build type:</span></span>

* <span data-ttu-id="53f0a-1267">Rangordning - skapa rang.</span><span class="sxs-lookup"><span data-stu-id="53f0a-1267">Rank - Rank build.</span></span>
* <span data-ttu-id="53f0a-1268">Rekommendation - rekommendation build.</span><span class="sxs-lookup"><span data-stu-id="53f0a-1268">Recommendation - Recommendation build.</span></span>

<span data-ttu-id="53f0a-1269">OData-XML</span><span class="sxs-lookup"><span data-stu-id="53f0a-1269">OData XML</span></span>

    <feed xmlns:base="https://api.datamarket.azure.com/amla/recommendations/v3/GetUserBuildsStatus" xmlns:d="http://schemas.microsoft.com/ado/2007/08/dataservices" xmlns:m="http://schemas.microsoft.com/ado/2007/08/dataservices/metadata" xmlns="http://www.w3.org/2005/Atom">
        <title type="text" />
        <subtitle type="text">Get builds status of a user</subtitle>
        <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetUserBuildsStatus?onlyLastBuilds=False&amp;apiVersion='1.0'</id>
        <rights type="text" />
        <updated>2014-11-05T18:41:21Z</updated>
        <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetUserBuildsStatus?onlyLastBuilds=False&amp;apiVersion='1.0'" />
        <entry>
            <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetUserBuildsStatus?onlyLastBuilds=False&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1</id>
            <title type="text">GetBuildsStatusEntity</title>
            <updated>2014-11-05T18:41:21Z</updated>
            <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetUserBuildsStatus?onlyLastBuilds=False&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1" />
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


### <a name="116-delete-build"></a><span data-ttu-id="53f0a-1270">11.6.</span><span class="sxs-lookup"><span data-stu-id="53f0a-1270">11.6.</span></span> <span data-ttu-id="53f0a-1271">Ta bort Build</span><span class="sxs-lookup"><span data-stu-id="53f0a-1271">Delete Build</span></span>
<span data-ttu-id="53f0a-1272">Tar bort en version.</span><span class="sxs-lookup"><span data-stu-id="53f0a-1272">Deletes a build.</span></span>

<span data-ttu-id="53f0a-1273">OBS!</span><span class="sxs-lookup"><span data-stu-id="53f0a-1273">NOTE:</span></span> <br><span data-ttu-id="53f0a-1274">Du kan inte ta bort en aktiv version.</span><span class="sxs-lookup"><span data-stu-id="53f0a-1274">You cannot delete an active build.</span></span> <span data-ttu-id="53f0a-1275">hello modellen ska vara uppdaterade tooa olika active build innan du tar bort den.</span><span class="sxs-lookup"><span data-stu-id="53f0a-1275">hello model should be updated tooa different active build before you delete it.</span></span><br><span data-ttu-id="53f0a-1276">Du kan inte ta bort en pågående version.</span><span class="sxs-lookup"><span data-stu-id="53f0a-1276">You cannot delete an in-progress build.</span></span> <span data-ttu-id="53f0a-1277">Avbryt först hello build genom att anropa <strong>Avbryt skapa</strong>.</span><span class="sxs-lookup"><span data-stu-id="53f0a-1277">You should cancel hello build first by calling <strong>Cancel Build</strong>.</span></span>

| <span data-ttu-id="53f0a-1278">HTTP-metoden</span><span class="sxs-lookup"><span data-stu-id="53f0a-1278">HTTP Method</span></span> | <span data-ttu-id="53f0a-1279">URI: N</span><span class="sxs-lookup"><span data-stu-id="53f0a-1279">URI</span></span> |
|:--- |:--- |
| <span data-ttu-id="53f0a-1280">TA BORT</span><span class="sxs-lookup"><span data-stu-id="53f0a-1280">DELETE</span></span> |`<rootURI>/DeleteBuild?buildId=%27<buildId>%27&apiVersion=%271.0%27`<br><br><span data-ttu-id="53f0a-1281">Exempel:</span><span class="sxs-lookup"><span data-stu-id="53f0a-1281">Example:</span></span><br>`<rootURI>/DeleteBuild?buildId=%271500068%27&apiVersion=%271.0%27` |

| <span data-ttu-id="53f0a-1282">Parameternamn</span><span class="sxs-lookup"><span data-stu-id="53f0a-1282">Parameter Name</span></span> | <span data-ttu-id="53f0a-1283">Giltiga värden</span><span class="sxs-lookup"><span data-stu-id="53f0a-1283">Valid Values</span></span> |
|:--- |:--- |
| <span data-ttu-id="53f0a-1284">buildId</span><span class="sxs-lookup"><span data-stu-id="53f0a-1284">buildId</span></span> |<span data-ttu-id="53f0a-1285">Unik identifierare för hello build.</span><span class="sxs-lookup"><span data-stu-id="53f0a-1285">Unique identifier of hello build.</span></span> |
| <span data-ttu-id="53f0a-1286">apiVersion</span><span class="sxs-lookup"><span data-stu-id="53f0a-1286">apiVersion</span></span> |<span data-ttu-id="53f0a-1287">1.0</span><span class="sxs-lookup"><span data-stu-id="53f0a-1287">1.0</span></span> |

<span data-ttu-id="53f0a-1288">**Svar:**</span><span class="sxs-lookup"><span data-stu-id="53f0a-1288">**Response:**</span></span>

<span data-ttu-id="53f0a-1289">HTTP-statuskod: 200</span><span class="sxs-lookup"><span data-stu-id="53f0a-1289">HTTP Status code: 200</span></span>

### <a name="117-cancel-build"></a><span data-ttu-id="53f0a-1290">11.7.</span><span class="sxs-lookup"><span data-stu-id="53f0a-1290">11.7.</span></span> <span data-ttu-id="53f0a-1291">Avbryt Build</span><span class="sxs-lookup"><span data-stu-id="53f0a-1291">Cancel Build</span></span>
<span data-ttu-id="53f0a-1292">Avbryter en version som har skapa status.</span><span class="sxs-lookup"><span data-stu-id="53f0a-1292">Cancels a build that is in building status.</span></span>

| <span data-ttu-id="53f0a-1293">HTTP-metoden</span><span class="sxs-lookup"><span data-stu-id="53f0a-1293">HTTP Method</span></span> | <span data-ttu-id="53f0a-1294">URI: N</span><span class="sxs-lookup"><span data-stu-id="53f0a-1294">URI</span></span> |
|:--- |:--- |
| <span data-ttu-id="53f0a-1295">PLACERA</span><span class="sxs-lookup"><span data-stu-id="53f0a-1295">PUT</span></span> |`<rootURI>/CancelBuild?buildId=%27<buildId>%27&apiVersion=%271.0%27`<br><br><span data-ttu-id="53f0a-1296">Exempel:</span><span class="sxs-lookup"><span data-stu-id="53f0a-1296">Example:</span></span><br>`<rootURI>/CancelBuild?buildId=%271500076%27&apiVersion=%271.0%27` |

| <span data-ttu-id="53f0a-1297">Parameternamn</span><span class="sxs-lookup"><span data-stu-id="53f0a-1297">Parameter Name</span></span> | <span data-ttu-id="53f0a-1298">Giltiga värden</span><span class="sxs-lookup"><span data-stu-id="53f0a-1298">Valid Values</span></span> |
|:--- |:--- |
| <span data-ttu-id="53f0a-1299">buildId</span><span class="sxs-lookup"><span data-stu-id="53f0a-1299">buildId</span></span> |<span data-ttu-id="53f0a-1300">Unik identifierare för hello build.</span><span class="sxs-lookup"><span data-stu-id="53f0a-1300">Unique identifier of hello build.</span></span> |
| <span data-ttu-id="53f0a-1301">apiVersion</span><span class="sxs-lookup"><span data-stu-id="53f0a-1301">apiVersion</span></span> |<span data-ttu-id="53f0a-1302">1.0</span><span class="sxs-lookup"><span data-stu-id="53f0a-1302">1.0</span></span> |

<span data-ttu-id="53f0a-1303">**Svar:**</span><span class="sxs-lookup"><span data-stu-id="53f0a-1303">**Response:**</span></span>

<span data-ttu-id="53f0a-1304">HTTP-statuskod: 200</span><span class="sxs-lookup"><span data-stu-id="53f0a-1304">HTTP Status code: 200</span></span>

### <a name="118-get-build-parameters"></a><span data-ttu-id="53f0a-1305">11.8.</span><span class="sxs-lookup"><span data-stu-id="53f0a-1305">11.8.</span></span> <span data-ttu-id="53f0a-1306">Hämta Build-parametrar</span><span class="sxs-lookup"><span data-stu-id="53f0a-1306">Get Build Parameters</span></span>
<span data-ttu-id="53f0a-1307">Hämtar skapa parametrar.</span><span class="sxs-lookup"><span data-stu-id="53f0a-1307">Retrieves build parameters.</span></span>

| <span data-ttu-id="53f0a-1308">HTTP-metoden</span><span class="sxs-lookup"><span data-stu-id="53f0a-1308">HTTP Method</span></span> | <span data-ttu-id="53f0a-1309">URI: N</span><span class="sxs-lookup"><span data-stu-id="53f0a-1309">URI</span></span> |
|:--- |:--- |
| <span data-ttu-id="53f0a-1310">HÄMTA</span><span class="sxs-lookup"><span data-stu-id="53f0a-1310">GET</span></span> |`<rootURI>/GetBuildParameters?buildId=%27<buildId>%27&apiVersion=%271.0%27`<br><br><span data-ttu-id="53f0a-1311">Exempel:</span><span class="sxs-lookup"><span data-stu-id="53f0a-1311">Example:</span></span><br>`<rootURI>/GetBuildParameters?buildId=%271000653%27&apiVersion=%271.0%27` |

| <span data-ttu-id="53f0a-1312">Parameternamn</span><span class="sxs-lookup"><span data-stu-id="53f0a-1312">Parameter Name</span></span> | <span data-ttu-id="53f0a-1313">Giltiga värden</span><span class="sxs-lookup"><span data-stu-id="53f0a-1313">Valid Values</span></span> |
|:--- |:--- |
| <span data-ttu-id="53f0a-1314">buildId</span><span class="sxs-lookup"><span data-stu-id="53f0a-1314">buildId</span></span> |<span data-ttu-id="53f0a-1315">Unik identifierare för hello build.</span><span class="sxs-lookup"><span data-stu-id="53f0a-1315">Unique identifier of hello build.</span></span> |
| <span data-ttu-id="53f0a-1316">apiVersion</span><span class="sxs-lookup"><span data-stu-id="53f0a-1316">apiVersion</span></span> |<span data-ttu-id="53f0a-1317">1.0</span><span class="sxs-lookup"><span data-stu-id="53f0a-1317">1.0</span></span> |

<span data-ttu-id="53f0a-1318">**Svar:**</span><span class="sxs-lookup"><span data-stu-id="53f0a-1318">**Response:**</span></span>

<span data-ttu-id="53f0a-1319">HTTP-statuskod: 200</span><span class="sxs-lookup"><span data-stu-id="53f0a-1319">HTTP Status code: 200</span></span>

<span data-ttu-id="53f0a-1320">Detta API returnerar en mängd med nyckel/värde-element.</span><span class="sxs-lookup"><span data-stu-id="53f0a-1320">This API returns a collection of key/value elements.</span></span> <span data-ttu-id="53f0a-1321">Varje element representerar en parameter och dess värde:</span><span class="sxs-lookup"><span data-stu-id="53f0a-1321">Each element represents a parameter and its value:</span></span>

* <span data-ttu-id="53f0a-1322">`feed/entry/content/properties/Key`-Skapa parameternamnet.</span><span class="sxs-lookup"><span data-stu-id="53f0a-1322">`feed/entry/content/properties/Key` - Build parameter name.</span></span>
* <span data-ttu-id="53f0a-1323">`feed/entry/content/properties/Value`-Skapa parametervärdet.</span><span class="sxs-lookup"><span data-stu-id="53f0a-1323">`feed/entry/content/properties/Value` - Build parameter value.</span></span>

<span data-ttu-id="53f0a-1324">hello tabellen nedan visar hello-värde som representerar varje nyckel.</span><span class="sxs-lookup"><span data-stu-id="53f0a-1324">hello table below depicts hello value that each key represents.</span></span>

| <span data-ttu-id="53f0a-1325">Nyckel</span><span class="sxs-lookup"><span data-stu-id="53f0a-1325">Key</span></span> | <span data-ttu-id="53f0a-1326">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="53f0a-1326">Description</span></span> | <span data-ttu-id="53f0a-1327">Typ</span><span class="sxs-lookup"><span data-stu-id="53f0a-1327">Type</span></span> | <span data-ttu-id="53f0a-1328">Giltigt värde</span><span class="sxs-lookup"><span data-stu-id="53f0a-1328">Valid Value</span></span> |
|:--- |:--- |:--- |:--- |
| <span data-ttu-id="53f0a-1329">NumberOfModelIterations</span><span class="sxs-lookup"><span data-stu-id="53f0a-1329">NumberOfModelIterations</span></span> |<span data-ttu-id="53f0a-1330">hello antal upprepningar hello modellen presterar avspeglas av hello övergripande compute tid och hello modellen noggrannhet.</span><span class="sxs-lookup"><span data-stu-id="53f0a-1330">hello number of iterations hello model performs is reflected by hello overall compute time and hello model accuracy.</span></span> <span data-ttu-id="53f0a-1331">hello högre hello nummer, hello bättre noggrannhet visas, men hello beräkna tid tar det längre tid.</span><span class="sxs-lookup"><span data-stu-id="53f0a-1331">hello higher hello number, hello better accuracy you will get, but hello compute time will take longer.</span></span> |<span data-ttu-id="53f0a-1332">Integer</span><span class="sxs-lookup"><span data-stu-id="53f0a-1332">Integer</span></span> |<span data-ttu-id="53f0a-1333">10-50</span><span class="sxs-lookup"><span data-stu-id="53f0a-1333">10-50</span></span> |
| <span data-ttu-id="53f0a-1334">NumberOfModelDimensions</span><span class="sxs-lookup"><span data-stu-id="53f0a-1334">NumberOfModelDimensions</span></span> |<span data-ttu-id="53f0a-1335">hello antalet dimensioner relaterar toohello antal 'funktioner' hello modellen försöker toofind i dina data.</span><span class="sxs-lookup"><span data-stu-id="53f0a-1335">hello number of dimensions relates toohello number of 'features' hello model will try toofind within your data.</span></span> <span data-ttu-id="53f0a-1336">Öka hello antal dimensioner kan bättre finjustering av hello resultat i mindre kluster.</span><span class="sxs-lookup"><span data-stu-id="53f0a-1336">Increasing hello number of dimensions will allow better fine-tuning of hello results into smaller clusters.</span></span> <span data-ttu-id="53f0a-1337">För många dimensioner kommer dock förhindra att hello modellen från att hitta visar sambandet mellan objekt.</span><span class="sxs-lookup"><span data-stu-id="53f0a-1337">However, too many dimensions will prevent hello model from finding correlations between items.</span></span> |<span data-ttu-id="53f0a-1338">Integer</span><span class="sxs-lookup"><span data-stu-id="53f0a-1338">Integer</span></span> |<span data-ttu-id="53f0a-1339">10-40</span><span class="sxs-lookup"><span data-stu-id="53f0a-1339">10-40</span></span> |
| <span data-ttu-id="53f0a-1340">ItemCutOffLowerBound</span><span class="sxs-lookup"><span data-stu-id="53f0a-1340">ItemCutOffLowerBound</span></span> |<span data-ttu-id="53f0a-1341">Definierar hello objektet nedre gräns för hello kylaren.</span><span class="sxs-lookup"><span data-stu-id="53f0a-1341">Defines hello item lower bound for hello condenser.</span></span> <span data-ttu-id="53f0a-1342">Se användning kylaren ovan.</span><span class="sxs-lookup"><span data-stu-id="53f0a-1342">See usage condenser above.</span></span> |<span data-ttu-id="53f0a-1343">Integer</span><span class="sxs-lookup"><span data-stu-id="53f0a-1343">Integer</span></span> |<span data-ttu-id="53f0a-1344">2 eller högre (0 inaktivera kylaren)</span><span class="sxs-lookup"><span data-stu-id="53f0a-1344">2 or more (0 disable condenser)</span></span> |
| <span data-ttu-id="53f0a-1345">ItemCutOffUpperBound</span><span class="sxs-lookup"><span data-stu-id="53f0a-1345">ItemCutOffUpperBound</span></span> |<span data-ttu-id="53f0a-1346">Definierar hello objektet övre gränsen för hello kylaren.</span><span class="sxs-lookup"><span data-stu-id="53f0a-1346">Defines hello item upper bound for hello condenser.</span></span> <span data-ttu-id="53f0a-1347">Se användning kylaren ovan.</span><span class="sxs-lookup"><span data-stu-id="53f0a-1347">See usage condenser above.</span></span> |<span data-ttu-id="53f0a-1348">Integer</span><span class="sxs-lookup"><span data-stu-id="53f0a-1348">Integer</span></span> |<span data-ttu-id="53f0a-1349">2 eller högre (0 inaktivera kylaren)</span><span class="sxs-lookup"><span data-stu-id="53f0a-1349">2 or more (0 disable condenser)</span></span> |
| <span data-ttu-id="53f0a-1350">UserCutOffLowerBound</span><span class="sxs-lookup"><span data-stu-id="53f0a-1350">UserCutOffLowerBound</span></span> |<span data-ttu-id="53f0a-1351">Definierar hello användaren nedre gräns för hello kylaren.</span><span class="sxs-lookup"><span data-stu-id="53f0a-1351">Defines hello user lower bound for hello condenser.</span></span> <span data-ttu-id="53f0a-1352">Se användning kylaren ovan.</span><span class="sxs-lookup"><span data-stu-id="53f0a-1352">See usage condenser above.</span></span> |<span data-ttu-id="53f0a-1353">Integer</span><span class="sxs-lookup"><span data-stu-id="53f0a-1353">Integer</span></span> |<span data-ttu-id="53f0a-1354">2 eller högre (0 inaktivera kylaren)</span><span class="sxs-lookup"><span data-stu-id="53f0a-1354">2 or more (0 disable condenser)</span></span> |
| <span data-ttu-id="53f0a-1355">UserCutOffUpperBound</span><span class="sxs-lookup"><span data-stu-id="53f0a-1355">UserCutOffUpperBound</span></span> |<span data-ttu-id="53f0a-1356">Definierar hello användaren övre gränsen för hello kylaren.</span><span class="sxs-lookup"><span data-stu-id="53f0a-1356">Defines hello user upper bound for hello condenser.</span></span> <span data-ttu-id="53f0a-1357">Se användning kylaren ovan.</span><span class="sxs-lookup"><span data-stu-id="53f0a-1357">See usage condenser above.</span></span> |<span data-ttu-id="53f0a-1358">Integer</span><span class="sxs-lookup"><span data-stu-id="53f0a-1358">Integer</span></span> |<span data-ttu-id="53f0a-1359">2 eller högre (0 inaktivera kylaren)</span><span class="sxs-lookup"><span data-stu-id="53f0a-1359">2 or more (0 disable condenser)</span></span> |
| <span data-ttu-id="53f0a-1360">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="53f0a-1360">Description</span></span> |<span data-ttu-id="53f0a-1361">Skapa beskrivning.</span><span class="sxs-lookup"><span data-stu-id="53f0a-1361">Build description.</span></span> |<span data-ttu-id="53f0a-1362">Sträng</span><span class="sxs-lookup"><span data-stu-id="53f0a-1362">String</span></span> |<span data-ttu-id="53f0a-1363">All text maximalt 512 tecken</span><span class="sxs-lookup"><span data-stu-id="53f0a-1363">Any text, maximum 512 chars</span></span> |
| <span data-ttu-id="53f0a-1364">EnableModelingInsights</span><span class="sxs-lookup"><span data-stu-id="53f0a-1364">EnableModelingInsights</span></span> |<span data-ttu-id="53f0a-1365">Tillåter toocompute mått på hello rekommendation modellen.</span><span class="sxs-lookup"><span data-stu-id="53f0a-1365">Allows you toocompute metrics on hello recommendation model.</span></span> |<span data-ttu-id="53f0a-1366">Booleskt värde</span><span class="sxs-lookup"><span data-stu-id="53f0a-1366">Boolean</span></span> |<span data-ttu-id="53f0a-1367">SANT/FALSKT</span><span class="sxs-lookup"><span data-stu-id="53f0a-1367">True/False</span></span> |
| <span data-ttu-id="53f0a-1368">UseFeaturesInModel</span><span class="sxs-lookup"><span data-stu-id="53f0a-1368">UseFeaturesInModel</span></span> |<span data-ttu-id="53f0a-1369">Anger om funktioner som kan användas i ordning tooenhance hello rekommendation modellen.</span><span class="sxs-lookup"><span data-stu-id="53f0a-1369">Indicates if features can be used in order tooenhance hello recommendation model.</span></span> |<span data-ttu-id="53f0a-1370">Booleskt värde</span><span class="sxs-lookup"><span data-stu-id="53f0a-1370">Boolean</span></span> |<span data-ttu-id="53f0a-1371">SANT/FALSKT</span><span class="sxs-lookup"><span data-stu-id="53f0a-1371">True/False</span></span> |
| <span data-ttu-id="53f0a-1372">ModelingFeatureList</span><span class="sxs-lookup"><span data-stu-id="53f0a-1372">ModelingFeatureList</span></span> |<span data-ttu-id="53f0a-1373">Kommaavgränsad lista över funktionen namn toobe används i hello rekommendation build i ordning tooenhance hello rekommendation.</span><span class="sxs-lookup"><span data-stu-id="53f0a-1373">Comma-separated list of feature names toobe used in hello recommendation build, in order tooenhance hello recommendation.</span></span> |<span data-ttu-id="53f0a-1374">Sträng</span><span class="sxs-lookup"><span data-stu-id="53f0a-1374">String</span></span> |<span data-ttu-id="53f0a-1375">Funktionsnamn in too512 tecken</span><span class="sxs-lookup"><span data-stu-id="53f0a-1375">Feature names, up too512 chars</span></span> |
| <span data-ttu-id="53f0a-1376">AllowColdItemPlacement</span><span class="sxs-lookup"><span data-stu-id="53f0a-1376">AllowColdItemPlacement</span></span> |<span data-ttu-id="53f0a-1377">Anger om hello rekommendation ska också push-installera cold objekt via funktionen likhet.</span><span class="sxs-lookup"><span data-stu-id="53f0a-1377">Indicates if hello recommendation should also push cold items via feature similarity.</span></span> |<span data-ttu-id="53f0a-1378">Booleskt värde</span><span class="sxs-lookup"><span data-stu-id="53f0a-1378">Boolean</span></span> |<span data-ttu-id="53f0a-1379">SANT/FALSKT</span><span class="sxs-lookup"><span data-stu-id="53f0a-1379">True/False</span></span> |
| <span data-ttu-id="53f0a-1380">EnableFeatureCorrelation</span><span class="sxs-lookup"><span data-stu-id="53f0a-1380">EnableFeatureCorrelation</span></span> |<span data-ttu-id="53f0a-1381">Anger om funktioner som kan användas i motivationen.</span><span class="sxs-lookup"><span data-stu-id="53f0a-1381">Indicates if features can be used in reasoning.</span></span> |<span data-ttu-id="53f0a-1382">Booleskt värde</span><span class="sxs-lookup"><span data-stu-id="53f0a-1382">Boolean</span></span> |<span data-ttu-id="53f0a-1383">SANT/FALSKT</span><span class="sxs-lookup"><span data-stu-id="53f0a-1383">True/False</span></span> |
| <span data-ttu-id="53f0a-1384">ReasoningFeatureList</span><span class="sxs-lookup"><span data-stu-id="53f0a-1384">ReasoningFeatureList</span></span> |<span data-ttu-id="53f0a-1385">Kommaavgränsad lista över funktionen namn toobe används för skäl meningar (t.ex. rekommendation förklaringar).</span><span class="sxs-lookup"><span data-stu-id="53f0a-1385">Comma-separated list of feature names toobe used for reasoning sentences (e.g. recommendation explanations).</span></span> |<span data-ttu-id="53f0a-1386">Sträng</span><span class="sxs-lookup"><span data-stu-id="53f0a-1386">String</span></span> |<span data-ttu-id="53f0a-1387">Funktionsnamn in too512 tecken</span><span class="sxs-lookup"><span data-stu-id="53f0a-1387">Feature names, up too512 chars</span></span> |

<span data-ttu-id="53f0a-1388">OData-XML</span><span class="sxs-lookup"><span data-stu-id="53f0a-1388">OData XML</span></span>

    <feed xmlns:base="https://api.datamarket.azure.com/amla/recommendations/v3/GetBuildParameters" xmlns:d="http://schemas.microsoft.com/ado/2007/08/dataservices" xmlns:m="http://schemas.microsoft.com/ado/2007/08/dataservices/metadata" xmlns="http://www.w3.org/2005/Atom">
        <title type="text" />
        <subtitle type="text">Get build parameters</subtitle>
        <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetBuildParameters?buildId='1000653'&amp;apiVersion='1.0'</id>
        <rights type="text" />
        <updated>2015-01-08T13:50:57Z</updated>
        <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetBuildParameters?buildId='1000653'&amp;apiVersion='1.0'" />
        <entry>
            <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetBuildParameters?buildId='1000653'&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1</id>
            <title type="text">GetBuildParametersEntity</title>
            <updated>2015-01-08T13:50:57Z</updated>
            <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetBuildParameters?buildId='1000653'&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1" />
            <content type="application/xml">
                <m:properties>
                    <d:Key m:type="Edm.String">UseFeaturesInModel</d:Key>
                    <d:Value m:type="Edm.String">False</d:Value>
                </m:properties>
            </content>
        </entry>
        <entry>
            <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetBuildParameters?buildId='1000653'&amp;apiVersion='1.0'&amp;$skip=1&amp;$top=1</id>
            <title type="text">GetBuildParametersEntity</title>
            <updated>2015-01-08T13:50:57Z</updated>
            <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetBuildParameters?buildId='1000653'&amp;apiVersion='1.0'&amp;$skip=1&amp;$top=1" />
            <content type="application/xml">
                <m:properties>
                    <d:Key m:type="Edm.String">AllowColdItemPlacement</d:Key>
                    <d:Value m:type="Edm.String">False</d:Value>
                </m:properties>
            </content>
        </entry>
        <entry>
            <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetBuildParameters?buildId='1000653'&amp;apiVersion='1.0'&amp;$skip=2&amp;$top=1</id>
            <title type="text">GetBuildParametersEntity</title>
            <updated>2015-01-08T13:50:57Z</updated>
            <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetBuildParameters?buildId='1000653'&amp;apiVersion='1.0'&amp;$skip=2&amp;$top=1" />
            <content type="application/xml">
                <m:properties>
                    <d:Key m:type="Edm.String">EnableFeatureCorrelation</d:Key>
                    <d:Value m:type="Edm.String">False</d:Value>
                </m:properties>
            </content>
        </entry>
        <entry>
            <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetBuildParameters?buildId='1000653'&amp;apiVersion='1.0'&amp;$skip=3&amp;$top=1</id>
            <title type="text">GetBuildParametersEntity</title>
            <updated>2015-01-08T13:50:57Z</updated>
            <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetBuildParameters?buildId='1000653'&amp;apiVersion='1.0'&amp;$skip=3&amp;$top=1" />
            <content type="application/xml">
                <m:properties>
                    <d:Key m:type="Edm.String">EnableModelingInsights</d:Key>
                    <d:Value m:type="Edm.String">False</d:Value>
                </m:properties>
            </content>
        </entry>
        <entry>
            <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetBuildParameters?buildId='1000653'&amp;apiVersion='1.0'&amp;$skip=4&amp;$top=1</id>
            <title type="text">GetBuildParametersEntity</title>
            <updated>2015-01-08T13:50:57Z</updated>
            <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetBuildParameters?buildId='1000653'&amp;apiVersion='1.0'&amp;$skip=4&amp;$top=1" />
            <content type="application/xml">
                <m:properties>
                    <d:Key m:type="Edm.String">NumberOfModelIterations</d:Key>
                    <d:Value m:type="Edm.String">10</d:Value>
                </m:properties>
            </content>
        </entry>
        <entry>
            <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetBuildParameters?buildId='1000653'&amp;apiVersion='1.0'&amp;$skip=5&amp;$top=1</id>
            <titletype="text">GetBuildParametersEntity</title>
            <updated>2015-01-08T13:50:57Z</updated>
            <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetBuildParameters?buildId='1000653'&amp;apiVersion='1.0'&amp;$skip=5&amp;$top=1" />
            <content type="application/xml">
                <m:properties>
                    <d:Key m:type="Edm.String">NumberOfModelDimensions</d:Key>
                    <d:Value m:type="Edm.String">10</d:Value>
                </m:properties>
            </content>
        </entry>
        <entry>
            <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetBuildParameters?buildId='1000653'&amp;apiVersion='1.0'&amp;$skip=6&amp;$top=1</id>
            <title type="text">GetBuildParametersEntity</title>
            <updated>2015-01-08T13:50:57Z</updated>
            <linkrel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetBuildParameters?buildId='1000653'&amp;apiVersion='1.0'&amp;$skip=6&amp;$top=1" />
            <content type="application/xml">
                <m:properties>
                    <d:Key m:type="Edm.String">ItemCutOffLowerBound</d:Key>
                    <d:Value m:type="Edm.String">2</d:Value>
                </m:properties>
            </content>
        </entry>
        <entry>
            <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetBuildParameters?buildId='1000653'&amp;apiVersion='1.0'&amp;$skip=7&amp;$top=1</id>
            <title type="text">GetBuildParametersEntity</title>
            <updated>2015-01-08T13:50:57Z</updated>
            <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetBuildParameters?buildId='1000653'&amp;apiVersion='1.0'&amp;$skip=7&amp;$top=1" />
            <content type="application/xml">
                <m:properties>
                    <d:Key m:type="Edm.String">ItemCutOffUpperBound</d:Key>
                    <d:Value m:type="Edm.String">2147483647</d:Value>
                </m:properties>
            </content>
        </entry>
        <entry>
            <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetBuildParameters?buildId='1000653'&amp;apiVersion='1.0'&amp;$skip=8&amp;$top=1</id>
            <title type="text">GetBuildParametersEntity</title>
            <updated>2015-01-08T13:50:57Z</updated>
            <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetBuildParameters?buildId='1000653'&amp;apiVersion='1.0'&amp;$skip=8&amp;$top=1" />
            <content type="application/xml">
                <m:properties>
                    <d:Key m:type="Edm.String">UserCutOffLowerBound</d:Key>
                    <d:Value m:type="Edm.String">2</d:Value>
                </m:properties>
            </content>
        </entry>
        <entry>
            <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetBuildParameters?buildId='1000653'&amp;apiVersion='1.0'&amp;$skip=9&amp;$top=1</id>
            <title type="text">GetBuildParametersEntity</title>
            <updated>2015-01-08T13:50:57Z</updated>
            <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetBuildParameters?buildId='1000653'&amp;apiVersion='1.0'&amp;$skip=9&amp;$top=1" />
            <content type="application/xml">
                <m:properties>
                    <d:Key m:type="Edm.String">UserCutOffUpperBound</d:Key>
                    <d:Value m:type="Edm.String">2147483647</d:Value>
                </m:properties>
            </content>
        </entry>
        <entry>
            <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetBuildParameters?buildId='1000653'&amp;apiVersion='1.0'&amp;$skip=10&amp;$top=1</id>
            <title type="text">GetBuildParametersEntity</title>
            <updated>2015-01-08T13:50:57Z</updated>
            <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetBuildParameters?buildId='1000653'&amp;apiVersion='1.0'&amp;$skip=10&amp;$top=1" />
            <content type="application/xml">
                <m:properties>
                    <d:Key m:type="Edm.String">ModelingFeatureList</d:Key>
                    <d:Value m:type="Edm.String"/>
                </m:properties>
            </content>
        </entry>
        <entry>
            <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetBuildParameters?buildId='1000653'&amp;apiVersion='1.0'&amp;$skip=11&amp;$top=1</id>
            <title type="text">GetBuildParametersEntity</title>
            <updated>2015-01-08T13:50:57Z</updated>
            <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetBuildParameters?buildId='1000653'&amp;apiVersion='1.0'&amp;$skip=11&amp;$top=1" />
            <content type="application/xml">
                <m:properties>
                    <d:Key m:type="Edm.String">ReasoningFeatureList</d:Key>
                    <d:Value m:type="Edm.String"/>
                </m:properties>
            </content>
        </entry>
        <entry>
            <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetBuildParameters?buildId='1000653'&amp;apiVersion='1.0'&amp;$skip=12&amp;$top=1</id>
            <title type="text">GetBuildParametersEntity</title>
            <updated>2015-01-08T13:50:57Z</updated>
            <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetBuildParameters?buildId='1000653'&amp;apiVersion='1.0'&amp;$skip=12&amp;$top=1" />
            <content type="application/xml">
                <m:properties>
                    <d:Key m:type="Edm.String">Description</d:Key>
                    <d:Value m:type="Edm.String">rankBuild</d:Value>
                </m:properties>
            </content>
        </entry>
    </feed>

## <a name="12-recommendation"></a><span data-ttu-id="53f0a-1389">12. Rekommendation</span><span class="sxs-lookup"><span data-stu-id="53f0a-1389">12. Recommendation</span></span>
### <a name="121-get-item-recommendations-for-active-build"></a><span data-ttu-id="53f0a-1390">12.1.</span><span class="sxs-lookup"><span data-stu-id="53f0a-1390">12.1.</span></span> <span data-ttu-id="53f0a-1391">Få rekommendationer för objektet (för aktiva build)</span><span class="sxs-lookup"><span data-stu-id="53f0a-1391">Get Item Recommendations (for active build)</span></span>
<span data-ttu-id="53f0a-1392">Få rekommendationer för hello active build av typen ”Recommendation” eller ”Fbt” baserat på en lista med objekt frö (indata).</span><span class="sxs-lookup"><span data-stu-id="53f0a-1392">Get recommendations of hello active build of type "Recommendation" or "Fbt" based on a list of seeds (input) items.</span></span>

| <span data-ttu-id="53f0a-1393">HTTP-metoden</span><span class="sxs-lookup"><span data-stu-id="53f0a-1393">HTTP Method</span></span> | <span data-ttu-id="53f0a-1394">URI: N</span><span class="sxs-lookup"><span data-stu-id="53f0a-1394">URI</span></span> |
|:--- |:--- |
| <span data-ttu-id="53f0a-1395">HÄMTA</span><span class="sxs-lookup"><span data-stu-id="53f0a-1395">GET</span></span> |`<rootURI>/ItemRecommend?modelId=%27<modelId>%27&itemIds=%27<itemId>%27&numberOfResults=<int>&includeMetadata=<bool>&apiVersion=%271.0%27`<br><br><span data-ttu-id="53f0a-1396">Exempel:</span><span class="sxs-lookup"><span data-stu-id="53f0a-1396">Example:</span></span><br>`<rootURI>/ItemRecommend?modelId=%272779c063-48fb-46c1-bae3-74acddc8c1d1%27&itemIds=%271003%27&numberOfResults=10&includeMetadata=false&apiVersion=%271.0%27` |

| <span data-ttu-id="53f0a-1397">Parameternamn</span><span class="sxs-lookup"><span data-stu-id="53f0a-1397">Parameter Name</span></span> | <span data-ttu-id="53f0a-1398">Giltiga värden</span><span class="sxs-lookup"><span data-stu-id="53f0a-1398">Valid Values</span></span> |
|:--- |:--- |
| <span data-ttu-id="53f0a-1399">%{ModelID/</span><span class="sxs-lookup"><span data-stu-id="53f0a-1399">modelId</span></span> |<span data-ttu-id="53f0a-1400">Unik identifierare för hello modellen</span><span class="sxs-lookup"><span data-stu-id="53f0a-1400">Unique identifier of hello model</span></span> |
| <span data-ttu-id="53f0a-1401">ItemId</span><span class="sxs-lookup"><span data-stu-id="53f0a-1401">itemIds</span></span> |<span data-ttu-id="53f0a-1402">Kommaavgränsad lista över hello objekt toorecommend för.</span><span class="sxs-lookup"><span data-stu-id="53f0a-1402">Comma-separated list of hello items toorecommend for.</span></span> <br><span data-ttu-id="53f0a-1403">Ange om hello active build är av FBT skickar du bara ett objekt.</span><span class="sxs-lookup"><span data-stu-id="53f0a-1403">If hello active build is of type FBT then you can send only one item.</span></span> <br><span data-ttu-id="53f0a-1404">Maxlängd: 1024</span><span class="sxs-lookup"><span data-stu-id="53f0a-1404">Max length: 1024</span></span> |
| <span data-ttu-id="53f0a-1405">numberOfResults</span><span class="sxs-lookup"><span data-stu-id="53f0a-1405">numberOfResults</span></span> |<span data-ttu-id="53f0a-1406">Antalet nödvändiga resultat</span><span class="sxs-lookup"><span data-stu-id="53f0a-1406">Number of required results</span></span> <br> <span data-ttu-id="53f0a-1407">Max: 150</span><span class="sxs-lookup"><span data-stu-id="53f0a-1407">Max: 150</span></span> |
| <span data-ttu-id="53f0a-1408">includeMetatadata</span><span class="sxs-lookup"><span data-stu-id="53f0a-1408">includeMetatadata</span></span> |<span data-ttu-id="53f0a-1409">Framtida användning alltid false</span><span class="sxs-lookup"><span data-stu-id="53f0a-1409">Future use, always false</span></span> |
| <span data-ttu-id="53f0a-1410">apiVersion</span><span class="sxs-lookup"><span data-stu-id="53f0a-1410">apiVersion</span></span> |<span data-ttu-id="53f0a-1411">1.0</span><span class="sxs-lookup"><span data-stu-id="53f0a-1411">1.0</span></span> |

<span data-ttu-id="53f0a-1412">**Svar:**</span><span class="sxs-lookup"><span data-stu-id="53f0a-1412">**Response:**</span></span>

<span data-ttu-id="53f0a-1413">HTTP-statuskod: 200</span><span class="sxs-lookup"><span data-stu-id="53f0a-1413">HTTP Status code: 200</span></span>

<span data-ttu-id="53f0a-1414">hello svaret innehåller en post per rekommenderade objekt.</span><span class="sxs-lookup"><span data-stu-id="53f0a-1414">hello response includes one entry per recommended item.</span></span> <span data-ttu-id="53f0a-1415">Varje post innehåller hello följande data:</span><span class="sxs-lookup"><span data-stu-id="53f0a-1415">Each entry has hello following data:</span></span>

* <span data-ttu-id="53f0a-1416">`Feed\entry\content\properties\Id`-Rekommenderade objekt-ID.</span><span class="sxs-lookup"><span data-stu-id="53f0a-1416">`Feed\entry\content\properties\Id` - Recommended item ID.</span></span>
* <span data-ttu-id="53f0a-1417">`Feed\entry\content\properties\Name`-Namnet på hello-objektet.</span><span class="sxs-lookup"><span data-stu-id="53f0a-1417">`Feed\entry\content\properties\Name` - Name of hello item.</span></span>
* <span data-ttu-id="53f0a-1418">`Feed\entry\content\properties\Rating`-Klassificering av hello rekommendation. högre nummer innebär högre tillförlitlighet.</span><span class="sxs-lookup"><span data-stu-id="53f0a-1418">`Feed\entry\content\properties\Rating` - Rating of hello recommendation; higher number means higher confidence.</span></span>
* <span data-ttu-id="53f0a-1419">`Feed\entry\content\properties\Reasoning`-Rekommendation skäl till (t.ex. rekommendation förklaringar).</span><span class="sxs-lookup"><span data-stu-id="53f0a-1419">`Feed\entry\content\properties\Reasoning` - Recommendation reasoning (e.g. recommendation explanations).</span></span>

<span data-ttu-id="53f0a-1420">hello exempel svaret nedan innehåller 10 rekommenderade objekt.</span><span class="sxs-lookup"><span data-stu-id="53f0a-1420">hello example response below includes 10 recommended items.</span></span>

<span data-ttu-id="53f0a-1421">OData-XML</span><span class="sxs-lookup"><span data-stu-id="53f0a-1421">OData XML</span></span>

    <feed xmlns:base="https://api.datamarket.azure.com/amla/recommendations/v3/ItemRecommend" xmlns:d="http://schemas.microsoft.com/ado/2007/08/dataservices" xmlns:m="http://schemas.microsoft.com/ado/2007/08/dataservices/metadata" xmlns="http://www.w3.org/2005/Atom">
      <title type="text" />
      <subtitle type="text">Get Recommendation</subtitle>
      <id>https://api.datamarket.azure.com/amla/recommendations/v3/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'</id>
      <rights type="text" />
      <updated>2014-10-05T12:28:48Z</updated>
      <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'" />
      <entry>
    <id>https://api.datamarket.azure.com/amla/recommendations/v3/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1</id>
    <title type="text">GetRecommendationEntity</title>
    <updated>2014-10-05T12:28:48Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1" />
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
    <id>https://api.datamarket.azure.com/amla/recommendations/v3/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=1&amp;$top=1</id>
    <title type="text">GetRecommendationEntity</title>
    <updated>2014-10-05T12:28:48Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=1&amp;$top=1" />
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
    <id>https://api.datamarket.azure.com/amla/recommendations/v3/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=2&amp;$top=1</id>
    <title type="text">GetRecommendationEntity</title>
    <updated>2014-10-05T12:28:48Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=2&amp;$top=1" />
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
    <id>https://api.datamarket.azure.com/amla/recommendations/v3/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=3&amp;$top=1</id>
    <title type="text">GetRecommendationEntity</title>
    <updated>2014-10-05T12:28:48Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=3&amp;$top=1" />
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
    <id>https://api.datamarket.azure.com/amla/recommendations/v3/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=4&amp;$top=1</id>
    <title type="text">GetRecommendationEntity</title>
    <updated>2014-10-05T12:28:48Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=4&amp;$top=1" />
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
    <id>https://api.datamarket.azure.com/amla/recommendations/v3/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=5&amp;$top=1</id>
    <title type="text">GetRecommendationEntity</title>
    <updated>2014-10-05T12:28:48Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=5&amp;$top=1" />
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
    <id>https://api.datamarket.azure.com/amla/recommendations/v3/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=6&amp;$top=1</id>
    <title type="text">GetRecommendationEntity</title>
    <updated>2014-10-05T12:28:48Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=6&amp;$top=1" />
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
    <id>https://api.datamarket.azure.com/amla/recommendations/v3/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=7&amp;$top=1</id>
    <title type="text">GetRecommendationEntity</title>
    <updated>2014-10-05T12:28:48Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=7&amp;$top=1" />
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
    <id>https://api.datamarket.azure.com/amla/recommendations/v3/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=8&amp;$top=1</id>
    <title type="text">GetRecommendationEntity</title>
    <updated>2014-10-05T12:28:48Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=8&amp;$top=1" />
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
    <id>https://api.datamarket.azure.com/amla/recommendations/v3/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=9&amp;$top=1</id>
    <title type="text">GetRecommendationEntity</title>
    <updated>2014-10-05T12:28:48Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=9&amp;$top=1" />
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

### <a name="122-get-item-recommendations-of-a-specific-build"></a><span data-ttu-id="53f0a-1422">12.2.</span><span class="sxs-lookup"><span data-stu-id="53f0a-1422">12.2.</span></span> <span data-ttu-id="53f0a-1423">Få rekommendationer för objektet (för en specifik version)</span><span class="sxs-lookup"><span data-stu-id="53f0a-1423">Get Item Recommendations (of a specific build)</span></span>
<span data-ttu-id="53f0a-1424">Få rekommendationer för en specifik version av typen ”Recommendation” eller ”Fbt”.</span><span class="sxs-lookup"><span data-stu-id="53f0a-1424">Get recommendations of a specific build of type "Recommendation" or "Fbt".</span></span>

| <span data-ttu-id="53f0a-1425">HTTP-metoden</span><span class="sxs-lookup"><span data-stu-id="53f0a-1425">HTTP Method</span></span> | <span data-ttu-id="53f0a-1426">URI: N</span><span class="sxs-lookup"><span data-stu-id="53f0a-1426">URI</span></span> |
|:--- |:--- |
| <span data-ttu-id="53f0a-1427">HÄMTA</span><span class="sxs-lookup"><span data-stu-id="53f0a-1427">GET</span></span> |`<rootURI>/ItemRecommend?modelId=%27<modelId>%27&itemIds=%27<itemId>%27&numberOfResults=<int>&includeMetadata=<bool>&buildId=<int>&apiVersion=%271.0%27`<br><br><span data-ttu-id="53f0a-1428">Exempel:</span><span class="sxs-lookup"><span data-stu-id="53f0a-1428">Example:</span></span><br>`<rootURI>/ItemRecommend?modelId=%272779c063-48fb-46c1-bae3-74acddc8c1d1%27&itemIds=%271003%27&numberOfResults=10&includeMetadata=false&buildId=1234&apiVersion=%271.0%27` |

| <span data-ttu-id="53f0a-1429">Parameternamn</span><span class="sxs-lookup"><span data-stu-id="53f0a-1429">Parameter Name</span></span> | <span data-ttu-id="53f0a-1430">Giltiga värden</span><span class="sxs-lookup"><span data-stu-id="53f0a-1430">Valid Values</span></span> |
|:--- |:--- |
| <span data-ttu-id="53f0a-1431">%{ModelID/</span><span class="sxs-lookup"><span data-stu-id="53f0a-1431">modelId</span></span> |<span data-ttu-id="53f0a-1432">Unik identifierare för hello modellen</span><span class="sxs-lookup"><span data-stu-id="53f0a-1432">Unique identifier of hello model</span></span> |
| <span data-ttu-id="53f0a-1433">ItemId</span><span class="sxs-lookup"><span data-stu-id="53f0a-1433">itemIds</span></span> |<span data-ttu-id="53f0a-1434">Kommaavgränsad lista över hello objekt toorecommend för.</span><span class="sxs-lookup"><span data-stu-id="53f0a-1434">Comma-separated list of hello items toorecommend for.</span></span> <br><span data-ttu-id="53f0a-1435">Ange om hello active build är av FBT skickar du bara ett objekt.</span><span class="sxs-lookup"><span data-stu-id="53f0a-1435">If hello active build is of type FBT then you can send only one item.</span></span> <br><span data-ttu-id="53f0a-1436">Maxlängd: 1024</span><span class="sxs-lookup"><span data-stu-id="53f0a-1436">Max length: 1024</span></span> |
| <span data-ttu-id="53f0a-1437">numberOfResults</span><span class="sxs-lookup"><span data-stu-id="53f0a-1437">numberOfResults</span></span> |<span data-ttu-id="53f0a-1438">Antalet nödvändiga resultat</span><span class="sxs-lookup"><span data-stu-id="53f0a-1438">Number of required results</span></span> <br> <span data-ttu-id="53f0a-1439">Max: 150</span><span class="sxs-lookup"><span data-stu-id="53f0a-1439">Max: 150</span></span> |
| <span data-ttu-id="53f0a-1440">includeMetatadata</span><span class="sxs-lookup"><span data-stu-id="53f0a-1440">includeMetatadata</span></span> |<span data-ttu-id="53f0a-1441">Framtida användning alltid false</span><span class="sxs-lookup"><span data-stu-id="53f0a-1441">Future use, always false</span></span> |
| <span data-ttu-id="53f0a-1442">buildId</span><span class="sxs-lookup"><span data-stu-id="53f0a-1442">buildId</span></span> |<span data-ttu-id="53f0a-1443">hello skapa id toouse för denna rekommendation begäran</span><span class="sxs-lookup"><span data-stu-id="53f0a-1443">hello build id toouse for this recommendation request</span></span> |
| <span data-ttu-id="53f0a-1444">apiVersion</span><span class="sxs-lookup"><span data-stu-id="53f0a-1444">apiVersion</span></span> |<span data-ttu-id="53f0a-1445">1.0</span><span class="sxs-lookup"><span data-stu-id="53f0a-1445">1.0</span></span> |

<span data-ttu-id="53f0a-1446">**Svar:**</span><span class="sxs-lookup"><span data-stu-id="53f0a-1446">**Response:**</span></span>

<span data-ttu-id="53f0a-1447">HTTP-statuskod: 200</span><span class="sxs-lookup"><span data-stu-id="53f0a-1447">HTTP Status code: 200</span></span>

<span data-ttu-id="53f0a-1448">hello svaret innehåller en post per rekommenderade objekt.</span><span class="sxs-lookup"><span data-stu-id="53f0a-1448">hello response includes one entry per recommended item.</span></span> <span data-ttu-id="53f0a-1449">Varje post innehåller hello följande data:</span><span class="sxs-lookup"><span data-stu-id="53f0a-1449">Each entry has hello following data:</span></span>

* <span data-ttu-id="53f0a-1450">`Feed\entry\content\properties\Id`-Rekommenderade objekt-ID.</span><span class="sxs-lookup"><span data-stu-id="53f0a-1450">`Feed\entry\content\properties\Id` - Recommended item ID.</span></span>
* <span data-ttu-id="53f0a-1451">`Feed\entry\content\properties\Name`-Namnet på hello-objektet.</span><span class="sxs-lookup"><span data-stu-id="53f0a-1451">`Feed\entry\content\properties\Name` - Name of hello item.</span></span>
* <span data-ttu-id="53f0a-1452">`Feed\entry\content\properties\Rating`-Klassificering av hello rekommendation. högre nummer innebär högre tillförlitlighet.</span><span class="sxs-lookup"><span data-stu-id="53f0a-1452">`Feed\entry\content\properties\Rating` - Rating of hello recommendation; higher number means higher confidence.</span></span>
* <span data-ttu-id="53f0a-1453">`Feed\entry\content\properties\Reasoning`-Rekommendation skäl till (t.ex. rekommendation förklaringar).</span><span class="sxs-lookup"><span data-stu-id="53f0a-1453">`Feed\entry\content\properties\Reasoning` - Recommendation reasoning (e.g. recommendation explanations).</span></span>

<span data-ttu-id="53f0a-1454">Se ett exempel svar i 12.1</span><span class="sxs-lookup"><span data-stu-id="53f0a-1454">See a response example in 12.1</span></span>

### <a name="123-get-fbt-recommendations-for-active-build"></a><span data-ttu-id="53f0a-1455">12.3.</span><span class="sxs-lookup"><span data-stu-id="53f0a-1455">12.3.</span></span> <span data-ttu-id="53f0a-1456">Hämta FBT rekommendationer (för aktiva build)</span><span class="sxs-lookup"><span data-stu-id="53f0a-1456">Get FBT Recommendations (for active build)</span></span>
<span data-ttu-id="53f0a-1457">Få rekommendationer av hello active version av typen ”Fbt” baserat på ett startvärde (indata)-objekt.</span><span class="sxs-lookup"><span data-stu-id="53f0a-1457">Get recommendations of hello active build of type "Fbt" based on a seed (input) item.</span></span>

| <span data-ttu-id="53f0a-1458">HTTP-metoden</span><span class="sxs-lookup"><span data-stu-id="53f0a-1458">HTTP Method</span></span> | <span data-ttu-id="53f0a-1459">URI: N</span><span class="sxs-lookup"><span data-stu-id="53f0a-1459">URI</span></span> |
|:--- |:--- |
| <span data-ttu-id="53f0a-1460">HÄMTA</span><span class="sxs-lookup"><span data-stu-id="53f0a-1460">GET</span></span> |`<rootURI>/ItemFbtRecommend?modelId=%27<modelId>%27&itemId=%27<itemId>%27&numberOfResults=<int>&minimalScore=<double>&includeMetadata=<bool>&apiVersion=%271.0%27`<br><br><span data-ttu-id="53f0a-1461">Exempel:</span><span class="sxs-lookup"><span data-stu-id="53f0a-1461">Example:</span></span><br>`<rootURI>/ItemFbtRecommend?modelId=%272779c063-48fb-46c1-bae3-74acddc8c1d1%27&itemId=%271003%27&numberOfResults=10&minimalScore=<double>&includeMetadata=false&apiVersion=%271.0%27` |

| <span data-ttu-id="53f0a-1462">Parameternamn</span><span class="sxs-lookup"><span data-stu-id="53f0a-1462">Parameter Name</span></span> | <span data-ttu-id="53f0a-1463">Giltiga värden</span><span class="sxs-lookup"><span data-stu-id="53f0a-1463">Valid Values</span></span> |
|:--- |:--- |
| <span data-ttu-id="53f0a-1464">%{ModelID/</span><span class="sxs-lookup"><span data-stu-id="53f0a-1464">modelId</span></span> |<span data-ttu-id="53f0a-1465">Unik identifierare för hello modellen</span><span class="sxs-lookup"><span data-stu-id="53f0a-1465">Unique identifier of hello model</span></span> |
| <span data-ttu-id="53f0a-1466">itemId</span><span class="sxs-lookup"><span data-stu-id="53f0a-1466">itemId</span></span> |<span data-ttu-id="53f0a-1467">Objektet toorecommend för.</span><span class="sxs-lookup"><span data-stu-id="53f0a-1467">Item toorecommend for.</span></span> <br><span data-ttu-id="53f0a-1468">Maxlängd: 1024</span><span class="sxs-lookup"><span data-stu-id="53f0a-1468">Max length: 1024</span></span> |
| <span data-ttu-id="53f0a-1469">numberOfResults</span><span class="sxs-lookup"><span data-stu-id="53f0a-1469">numberOfResults</span></span> |<span data-ttu-id="53f0a-1470">Antalet nödvändiga resultat</span><span class="sxs-lookup"><span data-stu-id="53f0a-1470">Number of required results</span></span> <br><span data-ttu-id="53f0a-1471">Max: 150</span><span class="sxs-lookup"><span data-stu-id="53f0a-1471">Max: 150</span></span> |
| <span data-ttu-id="53f0a-1472">minimalScore</span><span class="sxs-lookup"><span data-stu-id="53f0a-1472">minimalScore</span></span> |<span data-ttu-id="53f0a-1473">Minimal poäng som ofta set bör ha i ordning toobe ingår i hello returnerade resultat</span><span class="sxs-lookup"><span data-stu-id="53f0a-1473">Minimal score that a frequent set should have in order toobe included in hello returned results</span></span> |
| <span data-ttu-id="53f0a-1474">includeMetatadata</span><span class="sxs-lookup"><span data-stu-id="53f0a-1474">includeMetatadata</span></span> |<span data-ttu-id="53f0a-1475">Framtida användning alltid false</span><span class="sxs-lookup"><span data-stu-id="53f0a-1475">Future use, always false</span></span> |
| <span data-ttu-id="53f0a-1476">apiVersion</span><span class="sxs-lookup"><span data-stu-id="53f0a-1476">apiVersion</span></span> |<span data-ttu-id="53f0a-1477">1.0</span><span class="sxs-lookup"><span data-stu-id="53f0a-1477">1.0</span></span> |

<span data-ttu-id="53f0a-1478">**Svar:**</span><span class="sxs-lookup"><span data-stu-id="53f0a-1478">**Response:**</span></span>

<span data-ttu-id="53f0a-1479">HTTP-statuskod: 200</span><span class="sxs-lookup"><span data-stu-id="53f0a-1479">HTTP Status code: 200</span></span>

<span data-ttu-id="53f0a-1480">hello svaret innehåller en post per rekommenderade objektet uppsättningen (en uppsättning objekt som vanligtvis köpas tillsammans med hello seed-indata objekt).</span><span class="sxs-lookup"><span data-stu-id="53f0a-1480">hello response includes one entry per recommended item set (a set of items which are usually bought together with hello seed/input item).</span></span> <span data-ttu-id="53f0a-1481">Varje post innehåller hello följande data:</span><span class="sxs-lookup"><span data-stu-id="53f0a-1481">Each entry has hello following data:</span></span>

* <span data-ttu-id="53f0a-1482">`Feed\entry\content\properties\Id1`-Rekommenderade objekt-ID.</span><span class="sxs-lookup"><span data-stu-id="53f0a-1482">`Feed\entry\content\properties\Id1` - Recommended item ID.</span></span>
* <span data-ttu-id="53f0a-1483">`Feed\entry\content\properties\Name1`-Namnet på hello-objektet.</span><span class="sxs-lookup"><span data-stu-id="53f0a-1483">`Feed\entry\content\properties\Name1` - Name of hello item.</span></span>
* <span data-ttu-id="53f0a-1484">`Feed\entry\content\properties\Id2`2 rekommenderade artikel-ID (valfritt).</span><span class="sxs-lookup"><span data-stu-id="53f0a-1484">`Feed\entry\content\properties\Id2` - 2nd recommended item ID (optional).</span></span>
* <span data-ttu-id="53f0a-1485">`Feed\entry\content\properties\Name2`-Namnet på hello 2 objektet (valfritt).</span><span class="sxs-lookup"><span data-stu-id="53f0a-1485">`Feed\entry\content\properties\Name2` - Name of hello 2nd item (optional).</span></span>
* <span data-ttu-id="53f0a-1486">`Feed\entry\content\properties\Rating`-Klassificering av hello rekommendation. högre nummer innebär högre tillförlitlighet.</span><span class="sxs-lookup"><span data-stu-id="53f0a-1486">`Feed\entry\content\properties\Rating` - Rating of hello recommendation; higher number means higher confidence.</span></span>
* <span data-ttu-id="53f0a-1487">`Feed\entry\content\properties\Reasoning`-Rekommendation skäl till (t.ex. rekommendation förklaringar).</span><span class="sxs-lookup"><span data-stu-id="53f0a-1487">`Feed\entry\content\properties\Reasoning` - Recommendation reasoning (e.g. recommendation explanations).</span></span>

<span data-ttu-id="53f0a-1488">Hej exempelsvar nedan innehåller 3 rekommenderade objektet uppsättningar.</span><span class="sxs-lookup"><span data-stu-id="53f0a-1488">hello example response below includes 3 recommended item sets.</span></span>

<span data-ttu-id="53f0a-1489">OData-XML</span><span class="sxs-lookup"><span data-stu-id="53f0a-1489">OData XML</span></span>

    <feed xmlns:base="https://api.datamarket.azure.com/amla/recommendations/v3/ItemFbtRecommend" xmlns:d="http://schemas.microsoft.com/ado/2007/08/dataservices" xmlns:m="http://schemas.microsoft.com/ado/2007/08/dataservices/metadata" xmlns="http://www.w3.org/2005/Atom">
      <title type="text" />
      <subtitle type="text">Get Recommendation</subtitle>
      <id>https://api.datamarket.azure.com/amla/recommendations/v3/ItemFbtRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemId='1003'&amp;numberOfResults=3&amp;minimalScore=0.1&amp;includeMetadata=false&amp;apiVersion='1.0'</id>
      <rights type="text" />
      <updated>2014-10-05T12:28:48Z</updated>
      <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/ItemFbtRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=3&amp;minimalScore=0.1&amp;includeMetadata=false&amp;apiVersion='1.0'" />
      <entry>
    <id>https://api.datamarket.azure.com/amla/recommendations/v3/ItemFbtRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=3&amp;minimalScore=0.1&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1</id>
    <title type="text">GetFbtRecommendationEntity</title>
    <updated>2014-10-05T12:28:48Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/ItemFbtRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=3&amp;minimalScore=0.1&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1" />
    <content type="application/xml">
      <m:properties>
        <d:Id1 m:type="Edm.String">159</d:Id1>
        <d:Name1 m:type="Edm.String">159</d:Name1>
        <d:Id2 m:type="Edm.String"></d:Id2>
        <d:Name2 m:type="Edm.String"></d:Name2>
        <d:Rating m:type="Edm.Double">0.543343480387708</d:Rating>
        <d:Reasoning m:type="Edm.String">People who bought '1003' also bought '159'</d:Reasoning>
      </m:properties>
    </content>
      </entry>
      <entry>
    <id>https://api.datamarket.azure.com/amla/recommendations/v3/ItemFbtRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=3&amp;minimalScore=0.1&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=1&amp;$top=1</id>
    <title type="text">GetFbtRecommendationEntity</title>
    <updated>2014-10-05T12:28:48Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/ItemFbtRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=3&amp;minimalScore=0.1&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=1&amp;$top=1" />
    <content type="application/xml">
      <m:properties>
        <d:Id1 m:type="Edm.String">52</d:Id1>
        <d:Name1 m:type="Edm.String">52</d:Name1>
        <d:Id2 m:type="Edm.String"></d:Id2>
        <d:Name2 m:type="Edm.String"></d:Name2>
        <d:Rating m:type="Edm.Double">0.533343480387708</d:Rating>
        <d:Reasoning m:type="Edm.String">People who bought '1003' also bought '52'</d:Reasoning>
      </m:properties>
    </content>
      </entry>
      <entry>
    <id>https://api.datamarket.azure.com/amla/recommendations/v3/ItemFbtRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=3&amp;minimalScore=0.1&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=2&amp;$top=1</id>
    <title type="text">GetFbtRecommendationEntity</title>
    <updated>2014-10-05T12:28:48Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/ItemFbtRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=3&amp;minimalScore=0.1&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=2&amp;$top=1" />
    <content type="application/xml">
      <m:properties>
        <d:Id1 m:type="Edm.String">35</d:Id1>
        <d:Name1 m:type="Edm.String">35</d:Name1>
        <d:Id2 m:type="Edm.String">102</d:Id2>
        <d:Name2 m:type="Edm.String">102</d:Name2>
        <d:Rating m:type="Edm.Double">0.523343480387708</d:Rating>
        <d:Reasoning m:type="Edm.String">People who bought '1003' also bought '35' and '102'</d:Reasoning>
      </m:properties>
    </content>
      </entry>
    </feed>

### <a name="124-get-fbt-recommendations-of-a-specific-build"></a><span data-ttu-id="53f0a-1490">12.4.</span><span class="sxs-lookup"><span data-stu-id="53f0a-1490">12.4.</span></span> <span data-ttu-id="53f0a-1491">Få rekommendationer FBT (för en specifik version)</span><span class="sxs-lookup"><span data-stu-id="53f0a-1491">Get FBT Recommendations (of a specific build)</span></span>
<span data-ttu-id="53f0a-1492">Få rekommendationer för en specifik version av typen ”Fbt”.</span><span class="sxs-lookup"><span data-stu-id="53f0a-1492">Get recommendations of a specific build of type "Fbt".</span></span>

| <span data-ttu-id="53f0a-1493">HTTP-metoden</span><span class="sxs-lookup"><span data-stu-id="53f0a-1493">HTTP Method</span></span> | <span data-ttu-id="53f0a-1494">URI: N</span><span class="sxs-lookup"><span data-stu-id="53f0a-1494">URI</span></span> |
|:--- |:--- |
| <span data-ttu-id="53f0a-1495">HÄMTA</span><span class="sxs-lookup"><span data-stu-id="53f0a-1495">GET</span></span> |`<rootURI>/ItemFbtRecommend?modelId=%27<modelId>%27&itemId=%27<itemId>%27&numberOfResults=<int>&minimalScore=<double>&includeMetadata=<bool>&buildId=<int>&apiVersion=%271.0%27`<br><br><span data-ttu-id="53f0a-1496">Exempel:</span><span class="sxs-lookup"><span data-stu-id="53f0a-1496">Example:</span></span><br>`<rootURI>/ItemFbtRecommend?modelId=%272779c063-48fb-46c1-bae3-74acddc8c1d1%27&itemId=%271003%27&numberOfResults=10&minimalScore=0.1&includeMetadata=false&buildId=1234&apiVersion=%271.0%27` |

| <span data-ttu-id="53f0a-1497">Parameternamn</span><span class="sxs-lookup"><span data-stu-id="53f0a-1497">Parameter Name</span></span> | <span data-ttu-id="53f0a-1498">Giltiga värden</span><span class="sxs-lookup"><span data-stu-id="53f0a-1498">Valid Values</span></span> |
|:--- |:--- |
| <span data-ttu-id="53f0a-1499">%{ModelID/</span><span class="sxs-lookup"><span data-stu-id="53f0a-1499">modelId</span></span> |<span data-ttu-id="53f0a-1500">Unik identifierare för hello modellen</span><span class="sxs-lookup"><span data-stu-id="53f0a-1500">Unique identifier of hello model</span></span> |
| <span data-ttu-id="53f0a-1501">itemId</span><span class="sxs-lookup"><span data-stu-id="53f0a-1501">itemId</span></span> |<span data-ttu-id="53f0a-1502">Objektet toorecommend för.</span><span class="sxs-lookup"><span data-stu-id="53f0a-1502">Item toorecommend for.</span></span> <br><span data-ttu-id="53f0a-1503">Maxlängd: 1024</span><span class="sxs-lookup"><span data-stu-id="53f0a-1503">Max length: 1024</span></span> |
| <span data-ttu-id="53f0a-1504">numberOfResults</span><span class="sxs-lookup"><span data-stu-id="53f0a-1504">numberOfResults</span></span> |<span data-ttu-id="53f0a-1505">Antalet nödvändiga resultat</span><span class="sxs-lookup"><span data-stu-id="53f0a-1505">Number of required results</span></span> <br><span data-ttu-id="53f0a-1506">Max: 150</span><span class="sxs-lookup"><span data-stu-id="53f0a-1506">Max: 150</span></span> |
| <span data-ttu-id="53f0a-1507">minimalScore</span><span class="sxs-lookup"><span data-stu-id="53f0a-1507">minimalScore</span></span> |<span data-ttu-id="53f0a-1508">Minimal poäng som ofta set bör ha i ordning toobe ingår i hello returnerade resultat</span><span class="sxs-lookup"><span data-stu-id="53f0a-1508">Minimal score that a frequent set should have in order toobe included in hello returned results</span></span> |
| <span data-ttu-id="53f0a-1509">includeMetatadata</span><span class="sxs-lookup"><span data-stu-id="53f0a-1509">includeMetatadata</span></span> |<span data-ttu-id="53f0a-1510">Framtida användning alltid false</span><span class="sxs-lookup"><span data-stu-id="53f0a-1510">Future use, always false</span></span> |
| <span data-ttu-id="53f0a-1511">buildId</span><span class="sxs-lookup"><span data-stu-id="53f0a-1511">buildId</span></span> |<span data-ttu-id="53f0a-1512">hello skapa id toouse för denna rekommendation begäran</span><span class="sxs-lookup"><span data-stu-id="53f0a-1512">hello build id toouse for this recommendation request</span></span> |
| <span data-ttu-id="53f0a-1513">apiVersion</span><span class="sxs-lookup"><span data-stu-id="53f0a-1513">apiVersion</span></span> |<span data-ttu-id="53f0a-1514">1.0</span><span class="sxs-lookup"><span data-stu-id="53f0a-1514">1.0</span></span> |

<span data-ttu-id="53f0a-1515">**Svar:**</span><span class="sxs-lookup"><span data-stu-id="53f0a-1515">**Response:**</span></span>

<span data-ttu-id="53f0a-1516">HTTP-statuskod: 200</span><span class="sxs-lookup"><span data-stu-id="53f0a-1516">HTTP Status code: 200</span></span>

<span data-ttu-id="53f0a-1517">hello svaret innehåller en post per rekommenderade objektet uppsättningen (en uppsättning objekt som vanligtvis köpas tillsammans med hello seed-indata objekt).</span><span class="sxs-lookup"><span data-stu-id="53f0a-1517">hello response includes one entry per recommended item set (a set of items which are usually bought together with hello seed/input item).</span></span> <span data-ttu-id="53f0a-1518">Varje post innehåller hello följande data:</span><span class="sxs-lookup"><span data-stu-id="53f0a-1518">Each entry has hello following data:</span></span>

* <span data-ttu-id="53f0a-1519">`Feed\entry\content\properties\Id1`-Rekommenderade objekt-ID.</span><span class="sxs-lookup"><span data-stu-id="53f0a-1519">`Feed\entry\content\properties\Id1` - Recommended item ID.</span></span>
* <span data-ttu-id="53f0a-1520">`Feed\entry\content\properties\Name1`-Namnet på hello-objektet.</span><span class="sxs-lookup"><span data-stu-id="53f0a-1520">`Feed\entry\content\properties\Name1` - Name of hello item.</span></span>
* <span data-ttu-id="53f0a-1521">`Feed\entry\content\properties\Id2`2 rekommenderade artikel-ID (valfritt).</span><span class="sxs-lookup"><span data-stu-id="53f0a-1521">`Feed\entry\content\properties\Id2` - 2nd recommended item ID (optional).</span></span>
* <span data-ttu-id="53f0a-1522">`Feed\entry\content\properties\Name2`-Namnet på hello 2 objektet (valfritt).</span><span class="sxs-lookup"><span data-stu-id="53f0a-1522">`Feed\entry\content\properties\Name2` - Name of hello 2nd item (optional).</span></span>
* <span data-ttu-id="53f0a-1523">`Feed\entry\content\properties\Rating`-Klassificering av hello rekommendation. högre nummer innebär högre tillförlitlighet.</span><span class="sxs-lookup"><span data-stu-id="53f0a-1523">`Feed\entry\content\properties\Rating` - Rating of hello recommendation; higher number means higher confidence.</span></span>
* <span data-ttu-id="53f0a-1524">`Feed\entry\content\properties\Reasoning`-Rekommendation skäl till (t.ex. rekommendation förklaringar).</span><span class="sxs-lookup"><span data-stu-id="53f0a-1524">`Feed\entry\content\properties\Reasoning` - Recommendation reasoning (e.g. recommendation explanations).</span></span>

<span data-ttu-id="53f0a-1525">Se ett exempel svar i 12.3</span><span class="sxs-lookup"><span data-stu-id="53f0a-1525">See a response example in 12.3</span></span>

### <a name="125-get-user-recommendations-for-active-build"></a><span data-ttu-id="53f0a-1526">12.5.</span><span class="sxs-lookup"><span data-stu-id="53f0a-1526">12.5.</span></span> <span data-ttu-id="53f0a-1527">Få rekommendationer för användare (för aktiva build)</span><span class="sxs-lookup"><span data-stu-id="53f0a-1527">Get User Recommendations (for active build)</span></span>
<span data-ttu-id="53f0a-1528">Få rekommendationer för användare av en version av typen ”Recommendation” markerats som aktiv build.</span><span class="sxs-lookup"><span data-stu-id="53f0a-1528">Get user recommendations of a build of type "Recommendation" marked as active build.</span></span>

<span data-ttu-id="53f0a-1529">hello API returneras en lista över förväntade objekt enligt toohello användningshistorik för hello användare.</span><span class="sxs-lookup"><span data-stu-id="53f0a-1529">hello API will return a list of predicted item according toohello usage history of hello user.</span></span>

<span data-ttu-id="53f0a-1530">Information:</span><span class="sxs-lookup"><span data-stu-id="53f0a-1530">Notes:</span></span> 

1. <span data-ttu-id="53f0a-1531">Det finns ingen användare rekommendation för FBT version.</span><span class="sxs-lookup"><span data-stu-id="53f0a-1531">There is no user recommendation for FBT build.</span></span>
2. <span data-ttu-id="53f0a-1532">Om hello active skapa är FBT som den här metoden ska returneras ett fel.</span><span class="sxs-lookup"><span data-stu-id="53f0a-1532">If hello active build is FBT this method will returns an error.</span></span>

| <span data-ttu-id="53f0a-1533">HTTP-metoden</span><span class="sxs-lookup"><span data-stu-id="53f0a-1533">HTTP Method</span></span> | <span data-ttu-id="53f0a-1534">URI: N</span><span class="sxs-lookup"><span data-stu-id="53f0a-1534">URI</span></span> |
|:--- |:--- |
| <span data-ttu-id="53f0a-1535">HÄMTA</span><span class="sxs-lookup"><span data-stu-id="53f0a-1535">GET</span></span> |`<rootURI>/UserRecommend?modelId=%27<modelId>%27&userId=%27<userId>%27&numberOfResults=<int>&includeMetadata=<bool>&apiVersion=%271.0%27`<br><br><span data-ttu-id="53f0a-1536">Exempel:</span><span class="sxs-lookup"><span data-stu-id="53f0a-1536">Example:</span></span><br>`<rootURI>/UserRecommend?modelId=%272779c063-48fb-46c1-bae3-74acddc8c1d1%27&userId=%27u1101%27&numberOfResults=10&includeMetadata=false&apiVersion=%271.0%27` |

| <span data-ttu-id="53f0a-1537">Parameternamn</span><span class="sxs-lookup"><span data-stu-id="53f0a-1537">Parameter Name</span></span> | <span data-ttu-id="53f0a-1538">Giltiga värden</span><span class="sxs-lookup"><span data-stu-id="53f0a-1538">Valid Values</span></span> |
|:--- |:--- |
| <span data-ttu-id="53f0a-1539">%{ModelID/</span><span class="sxs-lookup"><span data-stu-id="53f0a-1539">modelId</span></span> |<span data-ttu-id="53f0a-1540">Unik identifierare för hello modellen</span><span class="sxs-lookup"><span data-stu-id="53f0a-1540">Unique identifier of hello model</span></span> |
| <span data-ttu-id="53f0a-1541">Användar-ID</span><span class="sxs-lookup"><span data-stu-id="53f0a-1541">userId</span></span> |<span data-ttu-id="53f0a-1542">Unik identifierare för hello användare</span><span class="sxs-lookup"><span data-stu-id="53f0a-1542">Unique identifier of hello user</span></span> |
| <span data-ttu-id="53f0a-1543">numberOfResults</span><span class="sxs-lookup"><span data-stu-id="53f0a-1543">numberOfResults</span></span> |<span data-ttu-id="53f0a-1544">Antalet nödvändiga resultat</span><span class="sxs-lookup"><span data-stu-id="53f0a-1544">Number of required results</span></span> |
| <span data-ttu-id="53f0a-1545">includeMetatadata</span><span class="sxs-lookup"><span data-stu-id="53f0a-1545">includeMetatadata</span></span> |<span data-ttu-id="53f0a-1546">Framtida användning alltid false</span><span class="sxs-lookup"><span data-stu-id="53f0a-1546">Future use, always false</span></span> |
| <span data-ttu-id="53f0a-1547">apiVersion</span><span class="sxs-lookup"><span data-stu-id="53f0a-1547">apiVersion</span></span> |<span data-ttu-id="53f0a-1548">1.0</span><span class="sxs-lookup"><span data-stu-id="53f0a-1548">1.0</span></span> |

<span data-ttu-id="53f0a-1549">**Svar:**</span><span class="sxs-lookup"><span data-stu-id="53f0a-1549">**Response:**</span></span>

<span data-ttu-id="53f0a-1550">HTTP-statuskod: 200</span><span class="sxs-lookup"><span data-stu-id="53f0a-1550">HTTP Status code: 200</span></span>

<span data-ttu-id="53f0a-1551">hello svaret innehåller en post per rekommenderade objekt.</span><span class="sxs-lookup"><span data-stu-id="53f0a-1551">hello response includes one entry per recommended item.</span></span> <span data-ttu-id="53f0a-1552">Varje post innehåller hello följande data:</span><span class="sxs-lookup"><span data-stu-id="53f0a-1552">Each entry has hello following data:</span></span>

* <span data-ttu-id="53f0a-1553">`Feed\entry\content\properties\Id`-Rekommenderade objekt-ID.</span><span class="sxs-lookup"><span data-stu-id="53f0a-1553">`Feed\entry\content\properties\Id` - Recommended item ID.</span></span>
* <span data-ttu-id="53f0a-1554">`Feed\entry\content\properties\Name`-Namnet på hello-objektet.</span><span class="sxs-lookup"><span data-stu-id="53f0a-1554">`Feed\entry\content\properties\Name` - Name of hello item.</span></span>
* <span data-ttu-id="53f0a-1555">`Feed\entry\content\properties\Rating`-Klassificering av hello rekommendation. högre nummer innebär högre tillförlitlighet.</span><span class="sxs-lookup"><span data-stu-id="53f0a-1555">`Feed\entry\content\properties\Rating` - Rating of hello recommendation; higher number means higher confidence.</span></span>
* <span data-ttu-id="53f0a-1556">`Feed\entry\content\properties\Reasoning`-Rekommendation skäl till (t.ex. rekommendation förklaringar).</span><span class="sxs-lookup"><span data-stu-id="53f0a-1556">`Feed\entry\content\properties\Reasoning` - Recommendation reasoning (e.g. recommendation explanations).</span></span>

<span data-ttu-id="53f0a-1557">Se ett exempel svar i 12.1</span><span class="sxs-lookup"><span data-stu-id="53f0a-1557">See a response example in 12.1</span></span>

### <a name="126-get-user-recommendations-with-item-list-for-active-build"></a><span data-ttu-id="53f0a-1558">12.6.</span><span class="sxs-lookup"><span data-stu-id="53f0a-1558">12.6.</span></span> <span data-ttu-id="53f0a-1559">Få rekommendationer för användare med listan över objekt (för aktiva build)</span><span class="sxs-lookup"><span data-stu-id="53f0a-1559">Get User Recommendations with item list (for active build)</span></span>
<span data-ttu-id="53f0a-1560">Få rekommendationer för användare av en version av typen ”Recommendation” markerats som aktiv build med en ytterligare lista med objekt</span><span class="sxs-lookup"><span data-stu-id="53f0a-1560">Get user recommendations of a build of type "Recommendation" marked as active build with an additional list of items</span></span>

<span data-ttu-id="53f0a-1561">hello API returneras en lista över förväntade objekt enligt toohello användningshistorik för hello användaren och hello ytterligare angivna objekt.</span><span class="sxs-lookup"><span data-stu-id="53f0a-1561">hello API will return a list of predicted item according toohello usage history of hello user and hello additional provided items.</span></span>

<span data-ttu-id="53f0a-1562">Information:</span><span class="sxs-lookup"><span data-stu-id="53f0a-1562">Notes:</span></span> 

1. <span data-ttu-id="53f0a-1563">Det finns ingen användare rekommendation för FBT version.</span><span class="sxs-lookup"><span data-stu-id="53f0a-1563">There is no user recommendation for FBT build.</span></span>
2. <span data-ttu-id="53f0a-1564">Om hello active skapa är FBT som den här metoden ska returneras ett fel.</span><span class="sxs-lookup"><span data-stu-id="53f0a-1564">If hello active build is FBT this method will returns an error.</span></span>

| <span data-ttu-id="53f0a-1565">HTTP-metoden</span><span class="sxs-lookup"><span data-stu-id="53f0a-1565">HTTP Method</span></span> | <span data-ttu-id="53f0a-1566">URI: N</span><span class="sxs-lookup"><span data-stu-id="53f0a-1566">URI</span></span> |
|:--- |:--- |
| <span data-ttu-id="53f0a-1567">HÄMTA</span><span class="sxs-lookup"><span data-stu-id="53f0a-1567">GET</span></span> |`<rootURI>/UserRecommend?modelId=%27<modelId>%27&userId=%27<userId>&itemsIds=%27<itemsIds>%27&numberOfResults=<int>&includeMetadata=<bool>&apiVersion=%271.0%27`<br><br><span data-ttu-id="53f0a-1568">Exempel:</span><span class="sxs-lookup"><span data-stu-id="53f0a-1568">Example:</span></span><br>`<rootURI>/UserRecommend?modelId=%272779c063-48fb-46c1-bae3-74acddc8c1d1%27&userId=%27u1101%27&itemsIds=%271003%2C1000%27&numberOfResults=10&includeMetadata=false&apiVersion=%271.0%27` |

| <span data-ttu-id="53f0a-1569">Parameternamn</span><span class="sxs-lookup"><span data-stu-id="53f0a-1569">Parameter Name</span></span> | <span data-ttu-id="53f0a-1570">Giltiga värden</span><span class="sxs-lookup"><span data-stu-id="53f0a-1570">Valid Values</span></span> |
|:--- |:--- |
| <span data-ttu-id="53f0a-1571">%{ModelID/</span><span class="sxs-lookup"><span data-stu-id="53f0a-1571">modelId</span></span> |<span data-ttu-id="53f0a-1572">Unik identifierare för hello modellen</span><span class="sxs-lookup"><span data-stu-id="53f0a-1572">Unique identifier of hello model</span></span> |
| <span data-ttu-id="53f0a-1573">Användar-ID</span><span class="sxs-lookup"><span data-stu-id="53f0a-1573">userId</span></span> |<span data-ttu-id="53f0a-1574">Unik identifierare för hello användare</span><span class="sxs-lookup"><span data-stu-id="53f0a-1574">Unique identifier of hello user</span></span> |
| <span data-ttu-id="53f0a-1575">itemsIds</span><span class="sxs-lookup"><span data-stu-id="53f0a-1575">itemsIds</span></span> |<span data-ttu-id="53f0a-1576">Kommaavgränsad lista över hello objekt toorecommend för.</span><span class="sxs-lookup"><span data-stu-id="53f0a-1576">Comma-separated list of hello items toorecommend for.</span></span> <span data-ttu-id="53f0a-1577">Maxlängd: 1024</span><span class="sxs-lookup"><span data-stu-id="53f0a-1577">Max length: 1024</span></span> |
| <span data-ttu-id="53f0a-1578">numberOfResults</span><span class="sxs-lookup"><span data-stu-id="53f0a-1578">numberOfResults</span></span> |<span data-ttu-id="53f0a-1579">Antalet nödvändiga resultat</span><span class="sxs-lookup"><span data-stu-id="53f0a-1579">Number of required results</span></span> |
| <span data-ttu-id="53f0a-1580">includeMetatadata</span><span class="sxs-lookup"><span data-stu-id="53f0a-1580">includeMetatadata</span></span> |<span data-ttu-id="53f0a-1581">Framtida användning alltid false</span><span class="sxs-lookup"><span data-stu-id="53f0a-1581">Future use, always false</span></span> |
| <span data-ttu-id="53f0a-1582">apiVersion</span><span class="sxs-lookup"><span data-stu-id="53f0a-1582">apiVersion</span></span> |<span data-ttu-id="53f0a-1583">1.0</span><span class="sxs-lookup"><span data-stu-id="53f0a-1583">1.0</span></span> |

<span data-ttu-id="53f0a-1584">**Svar:**</span><span class="sxs-lookup"><span data-stu-id="53f0a-1584">**Response:**</span></span>

<span data-ttu-id="53f0a-1585">HTTP-statuskod: 200</span><span class="sxs-lookup"><span data-stu-id="53f0a-1585">HTTP Status code: 200</span></span>

<span data-ttu-id="53f0a-1586">hello svaret innehåller en post per rekommenderade objekt.</span><span class="sxs-lookup"><span data-stu-id="53f0a-1586">hello response includes one entry per recommended item.</span></span> <span data-ttu-id="53f0a-1587">Varje post innehåller hello följande data:</span><span class="sxs-lookup"><span data-stu-id="53f0a-1587">Each entry has hello following data:</span></span>

* <span data-ttu-id="53f0a-1588">`Feed\entry\content\properties\Id`-Rekommenderade objekt-ID.</span><span class="sxs-lookup"><span data-stu-id="53f0a-1588">`Feed\entry\content\properties\Id` - Recommended item ID.</span></span>
* <span data-ttu-id="53f0a-1589">`Feed\entry\content\properties\Name`-Namnet på hello-objektet.</span><span class="sxs-lookup"><span data-stu-id="53f0a-1589">`Feed\entry\content\properties\Name` - Name of hello item.</span></span>
* <span data-ttu-id="53f0a-1590">`Feed\entry\content\properties\Rating`-Klassificering av hello rekommendation. högre nummer innebär högre tillförlitlighet.</span><span class="sxs-lookup"><span data-stu-id="53f0a-1590">`Feed\entry\content\properties\Rating` - Rating of hello recommendation; higher number means higher confidence.</span></span>
* <span data-ttu-id="53f0a-1591">`Feed\entry\content\properties\Reasoning`-Rekommendation skäl till (t.ex. rekommendation förklaringar).</span><span class="sxs-lookup"><span data-stu-id="53f0a-1591">`Feed\entry\content\properties\Reasoning` - Recommendation reasoning (e.g. recommendation explanations).</span></span>

<span data-ttu-id="53f0a-1592">Se ett exempel svar i 12.1</span><span class="sxs-lookup"><span data-stu-id="53f0a-1592">See a response example in 12.1</span></span>

### <a name="127-get-user-recommendations--of-a-specific-build"></a><span data-ttu-id="53f0a-1593">12.7.</span><span class="sxs-lookup"><span data-stu-id="53f0a-1593">12.7.</span></span> <span data-ttu-id="53f0a-1594">Få rekommendationer för användare (för en specifik version)</span><span class="sxs-lookup"><span data-stu-id="53f0a-1594">Get User Recommendations  (of a specific build)</span></span>
<span data-ttu-id="53f0a-1595">Få rekommendationer för användare av en viss version av typen ”Recommendation”.</span><span class="sxs-lookup"><span data-stu-id="53f0a-1595">Get user recommendations of a specific build of type "Recommendation".</span></span>

<span data-ttu-id="53f0a-1596">hello API returneras en lista över förväntade objekt enligt toohello användningshistorik för hello användare (används i hello specifika build).</span><span class="sxs-lookup"><span data-stu-id="53f0a-1596">hello API will return a list of predicted item according toohello usage history of hello user (used in hello specific build).</span></span>

<span data-ttu-id="53f0a-1597">Obs: Det finns ingen användare rekommendation för FBT version.</span><span class="sxs-lookup"><span data-stu-id="53f0a-1597">Note: There is no user recommendation for FBT build.</span></span>

| <span data-ttu-id="53f0a-1598">HTTP-metoden</span><span class="sxs-lookup"><span data-stu-id="53f0a-1598">HTTP Method</span></span> | <span data-ttu-id="53f0a-1599">URI: N</span><span class="sxs-lookup"><span data-stu-id="53f0a-1599">URI</span></span> |
|:--- |:--- |
| <span data-ttu-id="53f0a-1600">HÄMTA</span><span class="sxs-lookup"><span data-stu-id="53f0a-1600">GET</span></span> |`<rootURI>/UserRecommend?modelId=%27<modelId>%27&userId=%27<userId>%27&numberOfResults=<int>&includeMetadata=<bool>&buildId=<int>&apiVersion=%271.0%27`<br><br><span data-ttu-id="53f0a-1601">Exempel:</span><span class="sxs-lookup"><span data-stu-id="53f0a-1601">Example:</span></span><br>`<rootURI>/UserRecommend?modelId=%272779c063-48fb-46c1-bae3-74acddc8c1d1%27&userId=%27u1101%27&numberOfResults=10&includeMetadata=false&buildId=50012&apiVersion=%271.0%27` |

| <span data-ttu-id="53f0a-1602">Parameternamn</span><span class="sxs-lookup"><span data-stu-id="53f0a-1602">Parameter Name</span></span> | <span data-ttu-id="53f0a-1603">Giltiga värden</span><span class="sxs-lookup"><span data-stu-id="53f0a-1603">Valid Values</span></span> |
|:--- |:--- |
| <span data-ttu-id="53f0a-1604">%{ModelID/</span><span class="sxs-lookup"><span data-stu-id="53f0a-1604">modelId</span></span> |<span data-ttu-id="53f0a-1605">Unik identifierare för hello modellen</span><span class="sxs-lookup"><span data-stu-id="53f0a-1605">Unique identifier of hello model</span></span> |
| <span data-ttu-id="53f0a-1606">Användar-ID</span><span class="sxs-lookup"><span data-stu-id="53f0a-1606">userId</span></span> |<span data-ttu-id="53f0a-1607">Unik identifierare för hello användare</span><span class="sxs-lookup"><span data-stu-id="53f0a-1607">Unique identifier of hello user</span></span> |
| <span data-ttu-id="53f0a-1608">numberOfResults</span><span class="sxs-lookup"><span data-stu-id="53f0a-1608">numberOfResults</span></span> |<span data-ttu-id="53f0a-1609">Antalet nödvändiga resultat</span><span class="sxs-lookup"><span data-stu-id="53f0a-1609">Number of required results</span></span> |
| <span data-ttu-id="53f0a-1610">includeMetatadata</span><span class="sxs-lookup"><span data-stu-id="53f0a-1610">includeMetatadata</span></span> |<span data-ttu-id="53f0a-1611">Framtida användning alltid false</span><span class="sxs-lookup"><span data-stu-id="53f0a-1611">Future use, always false</span></span> |
| <span data-ttu-id="53f0a-1612">buildId</span><span class="sxs-lookup"><span data-stu-id="53f0a-1612">buildId</span></span> |<span data-ttu-id="53f0a-1613">hello skapa id toouse för denna rekommendation begäran</span><span class="sxs-lookup"><span data-stu-id="53f0a-1613">hello build id toouse for this recommendation request</span></span> |
| <span data-ttu-id="53f0a-1614">apiVersion</span><span class="sxs-lookup"><span data-stu-id="53f0a-1614">apiVersion</span></span> |<span data-ttu-id="53f0a-1615">1.0</span><span class="sxs-lookup"><span data-stu-id="53f0a-1615">1.0</span></span> |

<span data-ttu-id="53f0a-1616">**Svar:**</span><span class="sxs-lookup"><span data-stu-id="53f0a-1616">**Response:**</span></span>

<span data-ttu-id="53f0a-1617">HTTP-statuskod: 200</span><span class="sxs-lookup"><span data-stu-id="53f0a-1617">HTTP Status code: 200</span></span>

<span data-ttu-id="53f0a-1618">hello svaret innehåller en post per rekommenderade objekt.</span><span class="sxs-lookup"><span data-stu-id="53f0a-1618">hello response includes one entry per recommended item.</span></span> <span data-ttu-id="53f0a-1619">Varje post innehåller hello följande data:</span><span class="sxs-lookup"><span data-stu-id="53f0a-1619">Each entry has hello following data:</span></span>

* <span data-ttu-id="53f0a-1620">`Feed\entry\content\properties\Id`-Rekommenderade objekt-ID.</span><span class="sxs-lookup"><span data-stu-id="53f0a-1620">`Feed\entry\content\properties\Id` - Recommended item ID.</span></span>
* <span data-ttu-id="53f0a-1621">`Feed\entry\content\properties\Name`-Namnet på hello-objektet.</span><span class="sxs-lookup"><span data-stu-id="53f0a-1621">`Feed\entry\content\properties\Name` - Name of hello item.</span></span>
* <span data-ttu-id="53f0a-1622">`Feed\entry\content\properties\Rating`-Klassificering av hello rekommendation. högre nummer innebär högre tillförlitlighet.</span><span class="sxs-lookup"><span data-stu-id="53f0a-1622">`Feed\entry\content\properties\Rating` - Rating of hello recommendation; higher number means higher confidence.</span></span>
* <span data-ttu-id="53f0a-1623">`Feed\entry\content\properties\Reasoning`-Rekommendation skäl till (t.ex. rekommendation förklaringar).</span><span class="sxs-lookup"><span data-stu-id="53f0a-1623">`Feed\entry\content\properties\Reasoning` - Recommendation reasoning (e.g. recommendation explanations).</span></span>

<span data-ttu-id="53f0a-1624">Se ett exempel svar i 12.1</span><span class="sxs-lookup"><span data-stu-id="53f0a-1624">See a response example in 12.1</span></span>

### <a name="128-get-user-recommendations-with-item-list-of-a-specific-build"></a><span data-ttu-id="53f0a-1625">12.8.</span><span class="sxs-lookup"><span data-stu-id="53f0a-1625">12.8.</span></span> <span data-ttu-id="53f0a-1626">Få rekommendationer för användare med listan över objekt (för en specifik version)</span><span class="sxs-lookup"><span data-stu-id="53f0a-1626">Get User Recommendations with item list (of a specific build)</span></span>
<span data-ttu-id="53f0a-1627">Få rekommendationer för användare av en viss version av typen ”Recommendation” och hello lista över ytterligare objekt.</span><span class="sxs-lookup"><span data-stu-id="53f0a-1627">Get user recommendations of a specific build of type "Recommendation" and hello list of additional items.</span></span>

<span data-ttu-id="53f0a-1628">hello API returneras en lista över förväntade objekt enligt toohello användningshistorik för hello användare och hello ytterligare lista med objekt.</span><span class="sxs-lookup"><span data-stu-id="53f0a-1628">hello API will return a list of predicted item according toohello usage history of hello user and hello additional list of items.</span></span>

<span data-ttu-id="53f0a-1629">Obs: Tdet är ingen användare rekommendation för FBT version.</span><span class="sxs-lookup"><span data-stu-id="53f0a-1629">Note: Tthere is no user recommendation for FBT build.</span></span>

| <span data-ttu-id="53f0a-1630">HTTP-metoden</span><span class="sxs-lookup"><span data-stu-id="53f0a-1630">HTTP Method</span></span> | <span data-ttu-id="53f0a-1631">URI: N</span><span class="sxs-lookup"><span data-stu-id="53f0a-1631">URI</span></span> |
|:--- |:--- |
| <span data-ttu-id="53f0a-1632">HÄMTA</span><span class="sxs-lookup"><span data-stu-id="53f0a-1632">GET</span></span> |`<rootURI>/UserRecommend?modelId=%27<modelId>%27&userId=%27<userId>%27&itemsIds=%27<itemsIds>%27&numberOfResults=<int>&includeMetadata=<bool>&buildId=<int>&apiVersion=%271.0%27`<br><br><span data-ttu-id="53f0a-1633">Exempel:</span><span class="sxs-lookup"><span data-stu-id="53f0a-1633">Example:</span></span><br>`<rootURI>/UserRecommend?modelId=%272779c063-48fb-46c1-bae3-74acddc8c1d1%27&userId=%27u1101%27&itemsIds=%271003%27&numberOfResults=10&includeMetadata=false&buildId=50012&apiVersion=%271.0%27` |

| <span data-ttu-id="53f0a-1634">Parameternamn</span><span class="sxs-lookup"><span data-stu-id="53f0a-1634">Parameter Name</span></span> | <span data-ttu-id="53f0a-1635">Giltiga värden</span><span class="sxs-lookup"><span data-stu-id="53f0a-1635">Valid Values</span></span> |
|:--- |:--- |
| <span data-ttu-id="53f0a-1636">%{ModelID/</span><span class="sxs-lookup"><span data-stu-id="53f0a-1636">modelId</span></span> |<span data-ttu-id="53f0a-1637">Unik identifierare för hello modellen</span><span class="sxs-lookup"><span data-stu-id="53f0a-1637">Unique identifier of hello model</span></span> |
| <span data-ttu-id="53f0a-1638">Användar-ID</span><span class="sxs-lookup"><span data-stu-id="53f0a-1638">userId</span></span> |<span data-ttu-id="53f0a-1639">Unik identifierare för hello användare</span><span class="sxs-lookup"><span data-stu-id="53f0a-1639">Unique identifier of hello user</span></span> |
| <span data-ttu-id="53f0a-1640">ItemId</span><span class="sxs-lookup"><span data-stu-id="53f0a-1640">itemIds</span></span> |<span data-ttu-id="53f0a-1641">Kommaavgränsad lista över hello objekt toorecommend för.</span><span class="sxs-lookup"><span data-stu-id="53f0a-1641">Comma-separated list of hello items toorecommend for.</span></span> <span data-ttu-id="53f0a-1642">Maxlängd: 1024</span><span class="sxs-lookup"><span data-stu-id="53f0a-1642">Max length: 1024</span></span> |
| <span data-ttu-id="53f0a-1643">numberOfResults</span><span class="sxs-lookup"><span data-stu-id="53f0a-1643">numberOfResults</span></span> |<span data-ttu-id="53f0a-1644">Antalet nödvändiga resultat</span><span class="sxs-lookup"><span data-stu-id="53f0a-1644">Number of required results</span></span> |
| <span data-ttu-id="53f0a-1645">includeMetatadata</span><span class="sxs-lookup"><span data-stu-id="53f0a-1645">includeMetatadata</span></span> |<span data-ttu-id="53f0a-1646">Framtida användning alltid false</span><span class="sxs-lookup"><span data-stu-id="53f0a-1646">Future use, always false</span></span> |
| <span data-ttu-id="53f0a-1647">buildId</span><span class="sxs-lookup"><span data-stu-id="53f0a-1647">buildId</span></span> |<span data-ttu-id="53f0a-1648">hello skapa id toouse för denna rekommendation begäran</span><span class="sxs-lookup"><span data-stu-id="53f0a-1648">hello build id toouse for this recommendation request</span></span> |
| <span data-ttu-id="53f0a-1649">apiVersion</span><span class="sxs-lookup"><span data-stu-id="53f0a-1649">apiVersion</span></span> |<span data-ttu-id="53f0a-1650">1.0</span><span class="sxs-lookup"><span data-stu-id="53f0a-1650">1.0</span></span> |

<span data-ttu-id="53f0a-1651">**Svar:**</span><span class="sxs-lookup"><span data-stu-id="53f0a-1651">**Response:**</span></span>

<span data-ttu-id="53f0a-1652">HTTP-statuskod: 200</span><span class="sxs-lookup"><span data-stu-id="53f0a-1652">HTTP Status code: 200</span></span>

<span data-ttu-id="53f0a-1653">hello svaret innehåller en post per rekommenderade objekt.</span><span class="sxs-lookup"><span data-stu-id="53f0a-1653">hello response includes one entry per recommended item.</span></span> <span data-ttu-id="53f0a-1654">Varje post innehåller hello följande data:</span><span class="sxs-lookup"><span data-stu-id="53f0a-1654">Each entry has hello following data:</span></span>

* <span data-ttu-id="53f0a-1655">`Feed\entry\content\properties\Id`-Rekommenderade objekt-ID.</span><span class="sxs-lookup"><span data-stu-id="53f0a-1655">`Feed\entry\content\properties\Id` - Recommended item ID.</span></span>
* <span data-ttu-id="53f0a-1656">`Feed\entry\content\properties\Name`-Namnet på hello-objektet.</span><span class="sxs-lookup"><span data-stu-id="53f0a-1656">`Feed\entry\content\properties\Name` - Name of hello item.</span></span>
* <span data-ttu-id="53f0a-1657">`Feed\entry\content\properties\Rating`-Klassificering av hello rekommendation. högre nummer innebär högre tillförlitlighet.</span><span class="sxs-lookup"><span data-stu-id="53f0a-1657">`Feed\entry\content\properties\Rating` - Rating of hello recommendation; higher number means higher confidence.</span></span>
* <span data-ttu-id="53f0a-1658">`Feed\entry\content\properties\Reasoning`-Rekommendation skäl till (t.ex. rekommendation förklaringar).</span><span class="sxs-lookup"><span data-stu-id="53f0a-1658">`Feed\entry\content\properties\Reasoning` - Recommendation reasoning (e.g. recommendation explanations).</span></span>

<span data-ttu-id="53f0a-1659">Se ett exempel svar i 12.1</span><span class="sxs-lookup"><span data-stu-id="53f0a-1659">See a response example in 12.1</span></span>

## <a name="13-user-usage-history"></a><span data-ttu-id="53f0a-1660">13. Historik för användning av användaren</span><span class="sxs-lookup"><span data-stu-id="53f0a-1660">13. User Usage History</span></span>
<span data-ttu-id="53f0a-1661">När du väl har skapat en rekommendation modell kommer hello att tooretrieve hello användarhistorik (objekt associerade tooa särskilda användare) används för hello version.</span><span class="sxs-lookup"><span data-stu-id="53f0a-1661">Once a recommendation model was built hello system will allow tooretrieve hello user history (items associated tooa specific user) used for hello build.</span></span>
<span data-ttu-id="53f0a-1662">Detta API Tillåt tooretrieve hello användarens historik</span><span class="sxs-lookup"><span data-stu-id="53f0a-1662">This API allow tooretrieve hello user history</span></span>

<span data-ttu-id="53f0a-1663">Obs: hello användarhistorik är endast tillgänglig för rekommendation versioner.</span><span class="sxs-lookup"><span data-stu-id="53f0a-1663">Note: hello user history is currently available only for recommendation builds.</span></span>

### <a name="131-retrieve-user-history"></a><span data-ttu-id="53f0a-1664">13,1 hämta användarens historik</span><span class="sxs-lookup"><span data-stu-id="53f0a-1664">13.1 Retrieve user history</span></span>
<span data-ttu-id="53f0a-1665">Hämta hello lista över objekt som används i hello active skapa eller skapa för hello angivna användar-id i hello som angetts.</span><span class="sxs-lookup"><span data-stu-id="53f0a-1665">Retrieve hello list of item used in hello active build or in hello specified build for hello given user id.</span></span>

| <span data-ttu-id="53f0a-1666">HTTP-metoden</span><span class="sxs-lookup"><span data-stu-id="53f0a-1666">HTTP Method</span></span> | <span data-ttu-id="53f0a-1667">URI: N</span><span class="sxs-lookup"><span data-stu-id="53f0a-1667">URI</span></span> |
|:--- |:--- |
| <span data-ttu-id="53f0a-1668">HÄMTA</span><span class="sxs-lookup"><span data-stu-id="53f0a-1668">GET</span></span> |<span data-ttu-id="53f0a-1669">Hämta hello användarhistorik för hello active version.</span><span class="sxs-lookup"><span data-stu-id="53f0a-1669">Get hello user history for hello active build.</span></span><br/>`<rootURI>/GetUserHistory?modelId=%27<model_id>%27&userId=%27<userId>%27&apiVersion=%271.0%27`<br/><br/><span data-ttu-id="53f0a-1670">Hämta hello användarhistorik för hello angivna build`<rootURI>/GetUserHistory?modelId=%27<model_id>%27&userId=%27<userId>%27&buildId=<int>&apiVersion=%271.0%27`</span><span class="sxs-lookup"><span data-stu-id="53f0a-1670">Get hello user history for hello given build `<rootURI>/GetUserHistory?modelId=%27<model_id>%27&userId=%27<userId>%27&buildId=<int>&apiVersion=%271.0%27`</span></span><br/><br/><span data-ttu-id="53f0a-1671">Exempel:`<rootURI>/GetUserHistory?modelId=%2727967136e8-f868-4258-9331-10d567f87fae%27&&userId=%27u_1013%27&apiVersion=%271.0%277`</span><span class="sxs-lookup"><span data-stu-id="53f0a-1671">Example:`<rootURI>/GetUserHistory?modelId=%2727967136e8-f868-4258-9331-10d567f87fae%27&&userId=%27u_1013%27&apiVersion=%271.0%277`</span></span> |

| <span data-ttu-id="53f0a-1672">Parameternamn</span><span class="sxs-lookup"><span data-stu-id="53f0a-1672">Parameter Name</span></span> | <span data-ttu-id="53f0a-1673">Giltiga värden</span><span class="sxs-lookup"><span data-stu-id="53f0a-1673">Valid Values</span></span> |
|:--- |:--- |
| <span data-ttu-id="53f0a-1674">%{ModelID/</span><span class="sxs-lookup"><span data-stu-id="53f0a-1674">modelId</span></span> |<span data-ttu-id="53f0a-1675">hello Unik identifierare för hello modellen.</span><span class="sxs-lookup"><span data-stu-id="53f0a-1675">hello unique identifier of hello model.</span></span> |
| <span data-ttu-id="53f0a-1676">Användar-ID</span><span class="sxs-lookup"><span data-stu-id="53f0a-1676">userId</span></span> |<span data-ttu-id="53f0a-1677">hello Unik identifierare för hello användare.</span><span class="sxs-lookup"><span data-stu-id="53f0a-1677">hello unique identifier of hello user.</span></span> |
| <span data-ttu-id="53f0a-1678">buildId</span><span class="sxs-lookup"><span data-stu-id="53f0a-1678">buildId</span></span> |<span data-ttu-id="53f0a-1679">valfri parameter, Tillåt tooindicate från vilken build hello användarhistorik ska hämta</span><span class="sxs-lookup"><span data-stu-id="53f0a-1679">optional parameter, allow tooindicate from which build hello user history should be fetch</span></span> |
| <span data-ttu-id="53f0a-1680">apiVersion</span><span class="sxs-lookup"><span data-stu-id="53f0a-1680">apiVersion</span></span> |<span data-ttu-id="53f0a-1681">1.0</span><span class="sxs-lookup"><span data-stu-id="53f0a-1681">1.0</span></span> |

<span data-ttu-id="53f0a-1682">**Svar:**</span><span class="sxs-lookup"><span data-stu-id="53f0a-1682">**Response:**</span></span>

<span data-ttu-id="53f0a-1683">HTTP-statuskod: 200</span><span class="sxs-lookup"><span data-stu-id="53f0a-1683">HTTP Status code: 200</span></span>

<span data-ttu-id="53f0a-1684">hello svaret innehåller en post per rekommenderade objekt.</span><span class="sxs-lookup"><span data-stu-id="53f0a-1684">hello response includes one entry per recommended item.</span></span> <span data-ttu-id="53f0a-1685">Varje post innehåller hello följande data:</span><span class="sxs-lookup"><span data-stu-id="53f0a-1685">Each entry has hello following data:</span></span>

* <span data-ttu-id="53f0a-1686">`Feed\entry\content\properties\Id`-Rekommenderade objekt-ID.</span><span class="sxs-lookup"><span data-stu-id="53f0a-1686">`Feed\entry\content\properties\Id` - Recommended item ID.</span></span>
* <span data-ttu-id="53f0a-1687">`Feed\entry\content\properties\Name`-Namnet på hello-objektet.</span><span class="sxs-lookup"><span data-stu-id="53f0a-1687">`Feed\entry\content\properties\Name` - Name of hello item.</span></span>
* <span data-ttu-id="53f0a-1688">`Feed\entry\content\properties\Rating`-SAKNAS.</span><span class="sxs-lookup"><span data-stu-id="53f0a-1688">`Feed\entry\content\properties\Rating` - N/A.</span></span>
* <span data-ttu-id="53f0a-1689">`Feed\entry\content\properties\Reasoning`-SAKNAS.</span><span class="sxs-lookup"><span data-stu-id="53f0a-1689">`Feed\entry\content\properties\Reasoning` - N/A.</span></span>

<span data-ttu-id="53f0a-1690">OData-XML</span><span class="sxs-lookup"><span data-stu-id="53f0a-1690">OData XML</span></span>

    <feed xmlns:base="https://api.datamarket.azure.com/amla/recommendations/v3/GetUserHistory" xmlns:d="http://schemas.microsoft.com/ado/2007/08/dataservices" xmlns:m="http://schemas.microsoft.com/ado/2007/08/dataservices/metadata" xmlns="http://www.w3.org/2005/Atom">
    <title type="text" />
    <subtitle type="text">Get User History</subtitle>
    <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetUserHistory?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;userId='u_1013'&amp;apiVersion='1.0'</id>
    <rights type="text" />
    <updated>2015-05-26T15:32:47Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetUserHistory?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;userId='u_1013'&amp;apiVersion='1.0'" />
    <entry>
        <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetUserHistory?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;userId='u_1013'&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1</id>
        <title type="text">CatalogItemsThatContainATokenEntity</title>
        <updated>2015-05-26T15:32:47Z</updated>
        <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetUserHistory?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;userId='u_1013'&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1" />
        <content type="application/xml">
            <m:properties>
                <d:Id m:type="Edm.String">2406E770-769C-4189-89DE-1C9283F93A96</d:Id>
                <d:Name m:type="Edm.String">Clara Callan</d:Name>
                <d:Rating m:type="Edm.Double">0</d:Rating>
                <d:Reasoning m:type="Edm.String"/>
                <d:Metadata m:type="Edm.String"/>
                <d:FormattedRating m:type="Edm.Double" m:null="true"/>
            </m:properties>
        </content>
    </entry>
</feed>

## <a name="14-notifications"></a><span data-ttu-id="53f0a-1691">14. Meddelanden</span><span class="sxs-lookup"><span data-stu-id="53f0a-1691">14. Notifications</span></span>
<span data-ttu-id="53f0a-1692">Azure Machine Learning rekommendationer skapar meddelanden när permanenta fel uppstår i hello system.</span><span class="sxs-lookup"><span data-stu-id="53f0a-1692">Azure Machine Learning Recommendations creates notifications when persistent errors happen in hello system.</span></span> <span data-ttu-id="53f0a-1693">Det finns 3 typer av aviseringar:</span><span class="sxs-lookup"><span data-stu-id="53f0a-1693">There are 3 types of notifications:</span></span>

1. <span data-ttu-id="53f0a-1694">Build-fel - det här meddelandet utlöses för varje build-fel.</span><span class="sxs-lookup"><span data-stu-id="53f0a-1694">Build failure - This notification is triggered for every build failure.</span></span>
2. <span data-ttu-id="53f0a-1695">Datainsamling bearbetningsfel - denna avisering utlöses när vi har fler än 100 fel i hello senaste 5 minuterna i hello bearbetningen av Användningshändelser per modell.</span><span class="sxs-lookup"><span data-stu-id="53f0a-1695">Data acquisition processing failure - This notification is triggered when we have more than 100 errors in hello last 5 minutes in hello processing of usage events per model.</span></span>
3. <span data-ttu-id="53f0a-1696">Rekommendation förbrukning fel - det här meddelandet utlöses när vi har fler än 100 fel i hello senaste 5 minuterna i hello bearbetningen av rekommendation förfrågningar per modell.</span><span class="sxs-lookup"><span data-stu-id="53f0a-1696">Recommendation consumption failure - This notification is triggered when we have more than 100 errors in hello last 5 minutes in hello processing of recommendation requests per model.</span></span>

### <a name="141-get-notifications"></a><span data-ttu-id="53f0a-1697">14.1.</span><span class="sxs-lookup"><span data-stu-id="53f0a-1697">14.1.</span></span> <span data-ttu-id="53f0a-1698">Få meddelanden</span><span class="sxs-lookup"><span data-stu-id="53f0a-1698">Get Notifications</span></span>
<span data-ttu-id="53f0a-1699">Hämtar alla hello-meddelanden för alla modeller eller för en modell.</span><span class="sxs-lookup"><span data-stu-id="53f0a-1699">Retrieves all hello notifications for all models or for a single model.</span></span>

| <span data-ttu-id="53f0a-1700">HTTP-metoden</span><span class="sxs-lookup"><span data-stu-id="53f0a-1700">HTTP Method</span></span> | <span data-ttu-id="53f0a-1701">URI: N</span><span class="sxs-lookup"><span data-stu-id="53f0a-1701">URI</span></span> |
|:--- |:--- |
| <span data-ttu-id="53f0a-1702">HÄMTA</span><span class="sxs-lookup"><span data-stu-id="53f0a-1702">GET</span></span> |`<rootURI>/GetNotifications?modelId=%27<model_id>%27&apiVersion=%271.0%27`<br><br><span data-ttu-id="53f0a-1703">Hämta alla meddelanden för alla modeller:</span><span class="sxs-lookup"><span data-stu-id="53f0a-1703">Getting all notifications for all models:</span></span><br>`<rootURI>/GetNotifications?apiVersion=%271.0%27`<br><br><span data-ttu-id="53f0a-1704">Exempel för att hämta meddelanden för en viss modell:</span><span class="sxs-lookup"><span data-stu-id="53f0a-1704">Example for getting notifications for a specific model:</span></span><br>`<rootURI>/GetNotifications?modelId=%27967136e8-f868-4258-9331-10d567f87fae%27&apiVersion=%271.0%277` |

| <span data-ttu-id="53f0a-1705">Parameternamn</span><span class="sxs-lookup"><span data-stu-id="53f0a-1705">Parameter Name</span></span> | <span data-ttu-id="53f0a-1706">Giltiga värden</span><span class="sxs-lookup"><span data-stu-id="53f0a-1706">Valid Values</span></span> |
|:--- |:--- |
| <span data-ttu-id="53f0a-1707">%{ModelID/</span><span class="sxs-lookup"><span data-stu-id="53f0a-1707">modelId</span></span> |<span data-ttu-id="53f0a-1708">Valfri parameter.</span><span class="sxs-lookup"><span data-stu-id="53f0a-1708">Optional parameter.</span></span> <span data-ttu-id="53f0a-1709">Om detta utelämnas används visas alla meddelanden för alla modeller.</span><span class="sxs-lookup"><span data-stu-id="53f0a-1709">When omitted, you will get all notifications for all models.</span></span> <br><span data-ttu-id="53f0a-1710">Giltigt värde: Unik identifierare för hello modellen.</span><span class="sxs-lookup"><span data-stu-id="53f0a-1710">Valid value: unique identifier of hello model.</span></span> |
| <span data-ttu-id="53f0a-1711">apiVersion</span><span class="sxs-lookup"><span data-stu-id="53f0a-1711">apiVersion</span></span> |<span data-ttu-id="53f0a-1712">1.0</span><span class="sxs-lookup"><span data-stu-id="53f0a-1712">1.0</span></span> |
|  | |
| <span data-ttu-id="53f0a-1713">Begärandetext</span><span class="sxs-lookup"><span data-stu-id="53f0a-1713">Request Body</span></span> |<span data-ttu-id="53f0a-1714">INGEN</span><span class="sxs-lookup"><span data-stu-id="53f0a-1714">NONE</span></span> |

<span data-ttu-id="53f0a-1715">**Svar:**</span><span class="sxs-lookup"><span data-stu-id="53f0a-1715">**Response:**</span></span>

<span data-ttu-id="53f0a-1716">HTTP-statuskod: 200</span><span class="sxs-lookup"><span data-stu-id="53f0a-1716">HTTP Status code: 200</span></span>

<span data-ttu-id="53f0a-1717">OData-XML</span><span class="sxs-lookup"><span data-stu-id="53f0a-1717">OData XML</span></span>

    hello response includes one entry per notification. Each entry has hello following data:
        * feed\entry\content\properties\UserName - Internal user name identification.
        * feed\entry\content\properties\ModelId - Model ID.
        * feed\entry\content\properties\Message - Notification message.
        * feed\entry\content\properties\DateCreated - Date that this notification was created in UTC format.
        * feed\entry\content\properties\NotificationType - Notification types. Values are BuildFailure, RecommendationFailure, and DataAquisitionFailure.

    <feed xmlns:base="https://api.datamarket.azure.com/amla/recommendations/v3/GetNotifications" xmlns:d="http://schemas.microsoft.com/ado/2007/08/dataservices" xmlns:m="http://schemas.microsoft.com/ado/2007/08/dataservices/metadata" xmlns="http://www.w3.org/2005/Atom">
        <title type="text" />
        <subtitle type="text">Get a list of Notifications for a user</subtitle>
        <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetNotifications?modelId='967136e8-f868-4258-9331-10d567f87fae'&amp;apiVersion='1.0'</id>
        <rights type="text" />
        <updated>2014-11-04T13:24:23Z</updated>
        <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetNotifications?modelId='967136e8-f868-4258-9331-10d567f87fae'&amp;apiVersion='1.0'" />
        <entry>
                <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetNotifications?modelId='967136e8-f868-4258-9331-10d567f87fae'&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1</id>
            <title type="text">GetAListOfNotificationsForAUserEntity</title>
            <updated>2014-11-04T13:24:23Z</updated>
            <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetNotifications?modelId='967136e8-f868-4258-9331-10d567f87fae'&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1" />
            <content type="application/xml">
                <m:properties>
                    <d:UserName m:type="Edm.String">515506bc-3693-4dce-a5e2-81cb3e8efb56@dm.com</d:UserName>
                    <d:ModelId m:type="Edm.String">967136e8-f868-4258-9331-10d567f87fae</d:ModelId>
                    <d:Message m:type="Edm.String">Build failed for user</d:Message>
                    <d:DateCreated m:type="Edm.String">2014-11-04T13:23:14.383</d:DateCreated>
                    <d:NotificationType m:type="Edm.String">BuildFailure</d:NotificationType>
                </m:properties>
            </content>
        </entry>
    </feed>

### <a name="142-delete-model-notifications"></a><span data-ttu-id="53f0a-1718">14.2.</span><span class="sxs-lookup"><span data-stu-id="53f0a-1718">14.2.</span></span> <span data-ttu-id="53f0a-1719">Ta bort modellen meddelanden</span><span class="sxs-lookup"><span data-stu-id="53f0a-1719">Delete Model Notifications</span></span>
<span data-ttu-id="53f0a-1720">Tar bort alla skrivskyddade meddelanden för en modell.</span><span class="sxs-lookup"><span data-stu-id="53f0a-1720">Deletes all read notifications for a model.</span></span>

| <span data-ttu-id="53f0a-1721">HTTP-metoden</span><span class="sxs-lookup"><span data-stu-id="53f0a-1721">HTTP Method</span></span> | <span data-ttu-id="53f0a-1722">URI: N</span><span class="sxs-lookup"><span data-stu-id="53f0a-1722">URI</span></span> |
|:--- |:--- |
| <span data-ttu-id="53f0a-1723">TA BORT</span><span class="sxs-lookup"><span data-stu-id="53f0a-1723">DELETE</span></span> |`<rootURI>/DeleteModelNotifications?modelId=%<model_id>%27&apiVersion=%271.0%27`<br><br><span data-ttu-id="53f0a-1724">Exempel:</span><span class="sxs-lookup"><span data-stu-id="53f0a-1724">Example:</span></span><br>`<rootURI>/DeleteModelNotifications?modelId=%27967136e8-f868-4258-9331-10d567f87fae%27&apiVersion=%271.0%27` |

| <span data-ttu-id="53f0a-1725">Parameternamn</span><span class="sxs-lookup"><span data-stu-id="53f0a-1725">Parameter Name</span></span> | <span data-ttu-id="53f0a-1726">Giltiga värden</span><span class="sxs-lookup"><span data-stu-id="53f0a-1726">Valid Values</span></span> |
|:--- |:--- |
| <span data-ttu-id="53f0a-1727">%{ModelID/</span><span class="sxs-lookup"><span data-stu-id="53f0a-1727">modelId</span></span> |<span data-ttu-id="53f0a-1728">Unik identifierare för hello modellen</span><span class="sxs-lookup"><span data-stu-id="53f0a-1728">Unique identifier of hello model</span></span> |
| <span data-ttu-id="53f0a-1729">apiVersion</span><span class="sxs-lookup"><span data-stu-id="53f0a-1729">apiVersion</span></span> |<span data-ttu-id="53f0a-1730">1.0</span><span class="sxs-lookup"><span data-stu-id="53f0a-1730">1.0</span></span> |
|  | |
| <span data-ttu-id="53f0a-1731">Begärandetext</span><span class="sxs-lookup"><span data-stu-id="53f0a-1731">Request Body</span></span> |<span data-ttu-id="53f0a-1732">INGEN</span><span class="sxs-lookup"><span data-stu-id="53f0a-1732">NONE</span></span> |

<span data-ttu-id="53f0a-1733">**Svaret**:</span><span class="sxs-lookup"><span data-stu-id="53f0a-1733">**Response**:</span></span>

<span data-ttu-id="53f0a-1734">HTTP-statuskod: 200</span><span class="sxs-lookup"><span data-stu-id="53f0a-1734">HTTP Status code: 200</span></span>

### <a name="143-delete-user-notifications"></a><span data-ttu-id="53f0a-1735">14.3.</span><span class="sxs-lookup"><span data-stu-id="53f0a-1735">14.3.</span></span> <span data-ttu-id="53f0a-1736">Ta bort användarmeddelanden</span><span class="sxs-lookup"><span data-stu-id="53f0a-1736">Delete User Notifications</span></span>
<span data-ttu-id="53f0a-1737">Tar bort alla meddelanden för alla modeller.</span><span class="sxs-lookup"><span data-stu-id="53f0a-1737">Deletes all notifications for all models.</span></span>

| <span data-ttu-id="53f0a-1738">HTTP-metoden</span><span class="sxs-lookup"><span data-stu-id="53f0a-1738">HTTP Method</span></span> | <span data-ttu-id="53f0a-1739">URI: N</span><span class="sxs-lookup"><span data-stu-id="53f0a-1739">URI</span></span> |
|:--- |:--- |
| <span data-ttu-id="53f0a-1740">TA BORT</span><span class="sxs-lookup"><span data-stu-id="53f0a-1740">DELETE</span></span> |`<rootURI>/DeleteUserNotifications?apiVersion=%271.0%27` |

| <span data-ttu-id="53f0a-1741">Parameternamn</span><span class="sxs-lookup"><span data-stu-id="53f0a-1741">Parameter Name</span></span> | <span data-ttu-id="53f0a-1742">Giltiga värden</span><span class="sxs-lookup"><span data-stu-id="53f0a-1742">Valid Values</span></span> |
|:--- |:--- |
| <span data-ttu-id="53f0a-1743">apiVersion</span><span class="sxs-lookup"><span data-stu-id="53f0a-1743">apiVersion</span></span> |<span data-ttu-id="53f0a-1744">1.0</span><span class="sxs-lookup"><span data-stu-id="53f0a-1744">1.0</span></span> |
|  | |
| <span data-ttu-id="53f0a-1745">Begärandetext</span><span class="sxs-lookup"><span data-stu-id="53f0a-1745">Request Body</span></span> |<span data-ttu-id="53f0a-1746">INGEN</span><span class="sxs-lookup"><span data-stu-id="53f0a-1746">NONE</span></span> |

<span data-ttu-id="53f0a-1747">**Svaret**:</span><span class="sxs-lookup"><span data-stu-id="53f0a-1747">**Response**:</span></span>

<span data-ttu-id="53f0a-1748">HTTP-statuskod: 200</span><span class="sxs-lookup"><span data-stu-id="53f0a-1748">HTTP Status code: 200</span></span>

## <a name="15-legal"></a><span data-ttu-id="53f0a-1749">15. Juridisk information</span><span class="sxs-lookup"><span data-stu-id="53f0a-1749">15. Legal</span></span>
<span data-ttu-id="53f0a-1750">Detta dokument tillhandahålls ”som-är”.</span><span class="sxs-lookup"><span data-stu-id="53f0a-1750">This document is provided “as-is”.</span></span> <span data-ttu-id="53f0a-1751">Information och åsikter som uttrycks i detta dokument, inklusive Webbadresser och andra webbplatsreferenser, kan ändras utan föregående meddelande.</span><span class="sxs-lookup"><span data-stu-id="53f0a-1751">Information and views expressed in this document, including URL and other Internet Web site references, may change without notice.</span></span><br><br>
<span data-ttu-id="53f0a-1752">Vissa exempel som beskrivs häri tillhandahålls enbart som illustration och är fiktiva.</span><span class="sxs-lookup"><span data-stu-id="53f0a-1752">Some examples depicted herein are provided for illustration only and are fictitious.</span></span> <span data-ttu-id="53f0a-1753">Ingen verklig associering eller koppling är avsedd eller underförstådd.</span><span class="sxs-lookup"><span data-stu-id="53f0a-1753">No real association or connection is intended or should be inferred.</span></span><br><br>
<span data-ttu-id="53f0a-1754">Det här dokumentet ger dig inga juridiska rättigheter tooany immateriell egendom i någon Microsoft-produkt.</span><span class="sxs-lookup"><span data-stu-id="53f0a-1754">This document does not provide you with any legal rights tooany intellectual property in any Microsoft product.</span></span> <span data-ttu-id="53f0a-1755">Du får kopiera och använda det här dokumentet som intern referens.</span><span class="sxs-lookup"><span data-stu-id="53f0a-1755">You may copy and use this document for your internal, reference purposes.</span></span><br><br>
<span data-ttu-id="53f0a-1756">© 2015 Microsoft.</span><span class="sxs-lookup"><span data-stu-id="53f0a-1756">© 2015 Microsoft.</span></span> <span data-ttu-id="53f0a-1757">Med ensamrätt.</span><span class="sxs-lookup"><span data-stu-id="53f0a-1757">All rights reserved.</span></span>

