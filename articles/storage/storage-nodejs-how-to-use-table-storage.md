---
title: "aaaHow toouse Azure Table storage från Node.js | Microsoft Docs"
description: Lagra strukturerade data i hello molnet med Azure Table storage, en NoSQL-databas.
services: storage
documentationcenter: nodejs
author: mmacy
manager: timlt
editor: tysonn
ms.assetid: fc2e33d2-c5da-4861-8503-53fdc25750de
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: nodejs
ms.topic: article
ms.date: 12/08/2016
ms.author: marsma
ms.openlocfilehash: 990a71337b806d759d0277a7691712346db7b355
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="how-toouse-azure-table-storage-from-nodejs"></a><span data-ttu-id="57bfc-103">Hur toouse Azure Table storage från Node.js</span><span class="sxs-lookup"><span data-stu-id="57bfc-103">How toouse Azure Table storage from Node.js</span></span>
[!INCLUDE [storage-selector-table-include](../../includes/storage-selector-table-include.md)]
[!INCLUDE [storage-table-cosmos-db-langsoon-tip-include](../../includes/storage-table-cosmos-db-langsoon-tip-include.md)]

## <a name="overview"></a><span data-ttu-id="57bfc-104">Översikt</span><span class="sxs-lookup"><span data-stu-id="57bfc-104">Overview</span></span>
<span data-ttu-id="57bfc-105">Det här avsnittet visar hur tooperform vanliga scenarier med hjälp av hello Azure Table-tjänsten i ett Node.js-program.</span><span class="sxs-lookup"><span data-stu-id="57bfc-105">This topic shows how tooperform common scenarios using hello Azure Table service in a Node.js application.</span></span>

<span data-ttu-id="57bfc-106">hello kodexempel i det här avsnittet förutsätter att du redan har ett Node.js-program.</span><span class="sxs-lookup"><span data-stu-id="57bfc-106">hello code examples in this topic assume you already have a Node.js application.</span></span> <span data-ttu-id="57bfc-107">Information om hur toocreate ett Node.js-program i Azure, se några av följande avsnitt:</span><span class="sxs-lookup"><span data-stu-id="57bfc-107">For information about how toocreate a Node.js application in Azure, see any of these topics:</span></span>

* [<span data-ttu-id="57bfc-108">Skapa en Node.js-webbapp i Azure App Service</span><span class="sxs-lookup"><span data-stu-id="57bfc-108">Create a Node.js web app in Azure App Service</span></span>](../app-service-web/app-service-web-get-started-nodejs.md)
* [<span data-ttu-id="57bfc-109">Skapa och distribuera en Node.js web app tooAzure med WebMatrix</span><span class="sxs-lookup"><span data-stu-id="57bfc-109">Build and deploy a Node.js web app tooAzure using WebMatrix</span></span>](../app-service-web/web-sites-nodejs-use-webmatrix.md)
* <span data-ttu-id="57bfc-110">[Skapa och distribuera en Node.js-programmet tooan Azure Cloud Service](../cloud-services/cloud-services-nodejs-develop-deploy-app.md) (med Windows PowerShell)</span><span class="sxs-lookup"><span data-stu-id="57bfc-110">[Build and deploy a Node.js application tooan Azure Cloud Service](../cloud-services/cloud-services-nodejs-develop-deploy-app.md) (using Windows PowerShell)</span></span>

[!INCLUDE [storage-table-concepts-include](../../includes/storage-table-concepts-include.md)]

[!INCLUDE [storage-create-account-include](../../includes/storage-create-account-include.md)]

## <a name="configure-your-application-tooaccess-azure-storage"></a><span data-ttu-id="57bfc-111">Konfigurera ditt program tooaccess Azure Storage</span><span class="sxs-lookup"><span data-stu-id="57bfc-111">Configure your application tooaccess Azure Storage</span></span>
<span data-ttu-id="57bfc-112">toouse Azure Storage, behöver du hello Azure Storage SDK för Node.js som innehåller en uppsättning bekvämlighet bibliotek som kommunicerar med hello storage REST-tjänster.</span><span class="sxs-lookup"><span data-stu-id="57bfc-112">toouse Azure Storage, you need hello Azure Storage SDK for Node.js, which includes a set of convenience libraries that communicate with hello storage REST services.</span></span>

### <a name="use-node-package-manager-npm-tooinstall-hello-package"></a><span data-ttu-id="57bfc-113">Använd noden Package Manager (NPM) tooinstall hello-paket</span><span class="sxs-lookup"><span data-stu-id="57bfc-113">Use Node Package Manager (NPM) tooinstall hello package</span></span>
1. <span data-ttu-id="57bfc-114">Använd ett kommandoradsgränssnitt som **PowerShell** (Windows), **Terminal** (Mac), eller **Bash** (Unix) och gå toohello mappen där du skapade ditt program.</span><span class="sxs-lookup"><span data-stu-id="57bfc-114">Use a command-line interface such as **PowerShell** (Windows), **Terminal** (Mac), or **Bash** (Unix), and navigate toohello folder where you created your application.</span></span>
2. <span data-ttu-id="57bfc-115">Typen **npm installera azure-lagring** i hello kommandofönstret.</span><span class="sxs-lookup"><span data-stu-id="57bfc-115">Type **npm install azure-storage** in hello command window.</span></span> <span data-ttu-id="57bfc-116">Utdata från kommandot hello är liknande toohello följande exempel.</span><span class="sxs-lookup"><span data-stu-id="57bfc-116">Output from hello command is similar toohello following example.</span></span>

       azure-storage@0.5.0 node_modules\azure-storage
       +-- extend@1.2.1
       +-- xmlbuilder@0.4.3
       +-- mime@1.2.11
       +-- node-uuid@1.4.3
       +-- validator@3.22.2
       +-- underscore@1.4.4
       +-- readable-stream@1.0.33 (string_decoder@0.10.31, isarray@0.0.1, inherits@2.0.1, core-util-is@1.0.1)
       +-- xml2js@0.2.7 (sax@0.5.2)
       +-- request@2.57.0 (caseless@0.10.0, aws-sign2@0.5.0, forever-agent@0.6.1, stringstream@0.0.4, oauth-sign@0.8.0, tunnel-agent@0.4.1, isstream@0.1.2, json-stringify-safe@5.0.1, bl@0.9.4, combined-stream@1.0.5, qs@3.1.0, mime-types@2.0.14, form-data@0.2.0, http-signature@0.11.0, tough-cookie@2.0.0, hawk@2.3.1, har-validator@1.8.0)
3. <span data-ttu-id="57bfc-117">Du kan köra hello manuellt **ls** kommandot tooverify som en **nod\_moduler** mappen har skapats.</span><span class="sxs-lookup"><span data-stu-id="57bfc-117">You can manually run hello **ls** command tooverify that a **node\_modules** folder was created.</span></span> <span data-ttu-id="57bfc-118">I mappen hittar du hello **azure storage** paket som innehåller hello bibliotek som du behöver tooaccess lagring.</span><span class="sxs-lookup"><span data-stu-id="57bfc-118">Inside that folder you will find hello **azure-storage** package, which contains hello libraries you need tooaccess storage.</span></span>

### <a name="import-hello-package"></a><span data-ttu-id="57bfc-119">Importera hello-paket</span><span class="sxs-lookup"><span data-stu-id="57bfc-119">Import hello package</span></span>
<span data-ttu-id="57bfc-120">Lägg till följande kod toohello överkant hello hello **server.js** filen i ditt program:</span><span class="sxs-lookup"><span data-stu-id="57bfc-120">Add hello following code toohello top of hello **server.js** file in your application:</span></span>

```nodejs
var azure = require('azure-storage');
```

## <a name="set-up-an-azure-storage-connection"></a><span data-ttu-id="57bfc-121">Skapa en Azure Storage-anslutning</span><span class="sxs-lookup"><span data-stu-id="57bfc-121">Set up an Azure Storage connection</span></span>
<span data-ttu-id="57bfc-122">hello azure modulen läser hello miljövariabler AZURE\_lagring\_konto och AZURE\_lagring\_åtkomst\_nyckel eller AZURE\_lagring\_anslutning \_Sträng för information som krävs för tooconnect tooyour Azure storage-konto.</span><span class="sxs-lookup"><span data-stu-id="57bfc-122">hello azure module will read hello environment variables AZURE\_STORAGE\_ACCOUNT and AZURE\_STORAGE\_ACCESS\_KEY, or AZURE\_STORAGE\_CONNECTION\_STRING for information required tooconnect tooyour Azure storage account.</span></span> <span data-ttu-id="57bfc-123">Om de här miljövariablerna inte har angetts måste du ange hello kontoinformation när du anropar **TableService**.</span><span class="sxs-lookup"><span data-stu-id="57bfc-123">If these environment variables are not set, you must specify hello account information when calling **TableService**.</span></span>

<span data-ttu-id="57bfc-124">Ett exempel på inställningen hello miljövariabler i hello [Azure-portalen](https://portal.azure.com) en Azure-webbplats finns [Node.js web app med hello Azure Tabelltjänsten](../app-service-web/storage-nodejs-use-table-storage-web-site.md).</span><span class="sxs-lookup"><span data-stu-id="57bfc-124">For an example of setting hello environment variables in hello [Azure portal](https://portal.azure.com) for an Azure Website, see [Node.js web app using hello Azure Table Service](../app-service-web/storage-nodejs-use-table-storage-web-site.md).</span></span>

## <a name="create-a-table"></a><span data-ttu-id="57bfc-125">Skapa en tabell</span><span class="sxs-lookup"><span data-stu-id="57bfc-125">Create a table</span></span>
<span data-ttu-id="57bfc-126">hello följande kod skapar en **TableService** objekt och använder den toocreate en ny tabell.</span><span class="sxs-lookup"><span data-stu-id="57bfc-126">hello following code creates a **TableService** object and uses it toocreate a new table.</span></span> <span data-ttu-id="57bfc-127">Lägg till följande hello hello övre delen av **server.js**.</span><span class="sxs-lookup"><span data-stu-id="57bfc-127">Add hello following near hello top of **server.js**.</span></span>

```nodejs
var tableSvc = azure.createTableService();
```

<span data-ttu-id="57bfc-128">Hej anrop för**createTableIfNotExists** skapa en ny tabell med hello angivna namnet om det inte redan finns.</span><span class="sxs-lookup"><span data-stu-id="57bfc-128">hello call too**createTableIfNotExists** will create a new table with hello specified name if it does not already exist.</span></span> <span data-ttu-id="57bfc-129">hello skapas följande exempel en ny tabell med namnet ”mytable” som prefix om det inte redan finns:</span><span class="sxs-lookup"><span data-stu-id="57bfc-129">hello following example creates a new table named 'mytable' if it does not already exist:</span></span>

```nodejs
tableSvc.createTableIfNotExists('mytable', function(error, result, response){
  if(!error){
    // Table exists or created
  }
});
```

<span data-ttu-id="57bfc-130">Hej `result.created` blir `true` om en ny tabell skapas och `false` om hello tabellen redan finns.</span><span class="sxs-lookup"><span data-stu-id="57bfc-130">hello `result.created` will be `true` if a new table is created, and `false` if hello table already exists.</span></span> <span data-ttu-id="57bfc-131">Hej `response` innehåller information om hello-begäran.</span><span class="sxs-lookup"><span data-stu-id="57bfc-131">hello `response` will contain information about hello request.</span></span>

### <a name="filters"></a><span data-ttu-id="57bfc-132">Filter</span><span class="sxs-lookup"><span data-stu-id="57bfc-132">Filters</span></span>
<span data-ttu-id="57bfc-133">Valfria filtrering åtgärder kan vara tillämpade toooperations utförs med hjälp av **TableService**.</span><span class="sxs-lookup"><span data-stu-id="57bfc-133">Optional filtering operations can be applied toooperations performed using **TableService**.</span></span> <span data-ttu-id="57bfc-134">Filtrering operations kan innehålla loggning, försöker automatiskt igen och så vidare. Filtren är objekt som implementerar en metod med hello signatur:</span><span class="sxs-lookup"><span data-stu-id="57bfc-134">Filtering operations can include logging, automatically retrying, etc. Filters are objects that implement a method with hello signature:</span></span>

```nodejs
function handle (requestOptions, next)
```

<span data-ttu-id="57bfc-135">När du har gjort dess förbearbetning på hello begäran alternativ måste hello-metoden toocall ”nästa” Skicka en motringning med hello följande signatur:</span><span class="sxs-lookup"><span data-stu-id="57bfc-135">After doing its preprocessing on hello request options, hello method needs toocall "next", passing a callback with hello following signature:</span></span>

```nodejs
function (returnObject, finalCallback, next)
```

<span data-ttu-id="57bfc-136">I den här motringning och efter bearbetning hello returnObject (hello svar från hello begäran toohello server), hello-återanrop måste tooeither anropa sedan om den finns toocontinue bearbetning filter eller helt enkelt anropa finalCallback annars tooend hello Service-anrop.</span><span class="sxs-lookup"><span data-stu-id="57bfc-136">In this callback, and after processing hello returnObject (hello response from hello request toohello server), hello callback needs tooeither invoke next if it exists toocontinue processing other filters or simply invoke finalCallback otherwise tooend hello service invocation.</span></span>

<span data-ttu-id="57bfc-137">Två filter som implementerar logik som medföljer hello Azure SDK för Node.js, **ExponentialRetryPolicyFilter** och **LinearRetryPolicyFilter**.</span><span class="sxs-lookup"><span data-stu-id="57bfc-137">Two filters that implement retry logic are included with hello Azure SDK for Node.js, **ExponentialRetryPolicyFilter** and **LinearRetryPolicyFilter**.</span></span> <span data-ttu-id="57bfc-138">hello följande skapar en **TableService** objekt som använder hello **ExponentialRetryPolicyFilter**:</span><span class="sxs-lookup"><span data-stu-id="57bfc-138">hello following creates a **TableService** object that uses hello **ExponentialRetryPolicyFilter**:</span></span>

```nodejs
var retryOperations = new azure.ExponentialRetryPolicyFilter();
var tableSvc = azure.createTableService().withFilter(retryOperations);
```

## <a name="add-an-entity-tooa-table"></a><span data-ttu-id="57bfc-139">Lägg till en entitet tooa tabell</span><span class="sxs-lookup"><span data-stu-id="57bfc-139">Add an entity tooa table</span></span>
<span data-ttu-id="57bfc-140">tooadd en entitet först skapa ett objekt som definierar egenskaper för enhet.</span><span class="sxs-lookup"><span data-stu-id="57bfc-140">tooadd an entity, first create an object that defines your entity properties.</span></span> <span data-ttu-id="57bfc-141">Alla enheter måste innehålla en **PartitionKey** och **RowKey**, som är unika identifierare för hello entitet.</span><span class="sxs-lookup"><span data-stu-id="57bfc-141">All entities must contain a **PartitionKey** and **RowKey**, which are unique identifiers for hello entity.</span></span>

* <span data-ttu-id="57bfc-142">**PartitionKey** -avgör hello-partition som hello entitet lagras i</span><span class="sxs-lookup"><span data-stu-id="57bfc-142">**PartitionKey** - determines hello partition that hello entity is stored in</span></span>
* <span data-ttu-id="57bfc-143">**RowKey** - unikt identifierar hello entiteten inom hello partition</span><span class="sxs-lookup"><span data-stu-id="57bfc-143">**RowKey** - uniquely identifies hello entity within hello partition</span></span>

<span data-ttu-id="57bfc-144">Båda **PartitionKey** och **RowKey** måste vara strängvärden.</span><span class="sxs-lookup"><span data-stu-id="57bfc-144">Both **PartitionKey** and **RowKey** must be string values.</span></span> <span data-ttu-id="57bfc-145">Mer information finns i [hello förstå tabelltjänst-datamodellen](http://msdn.microsoft.com/library/azure/dd179338.aspx).</span><span class="sxs-lookup"><span data-stu-id="57bfc-145">For more information, see [Understanding hello Table Service Data Model](http://msdn.microsoft.com/library/azure/dd179338.aspx).</span></span>

<span data-ttu-id="57bfc-146">hello följande är ett exempel för att definiera en entitet.</span><span class="sxs-lookup"><span data-stu-id="57bfc-146">hello following is an example of defining an entity.</span></span> <span data-ttu-id="57bfc-147">Observera att **dueDate** definieras som en typ av **Edm.DateTime**.</span><span class="sxs-lookup"><span data-stu-id="57bfc-147">Note that **dueDate** is defined as a type of **Edm.DateTime**.</span></span> <span data-ttu-id="57bfc-148">Att ange hello-typen är valfri och typer kommer att härleda om inget anges.</span><span class="sxs-lookup"><span data-stu-id="57bfc-148">Specifying hello type is optional, and types will be inferred if not specified.</span></span>

```nodejs
var task = {
  PartitionKey: {'_':'hometasks'},
  RowKey: {'_': '1'},
  description: {'_':'take out hello trash'},
  dueDate: {'_':new Date(2015, 6, 20), '$':'Edm.DateTime'}
};
```

> [!NOTE]
> <span data-ttu-id="57bfc-149">Det finns också en **tidsstämpel** för varje post som anges av Azure när en entitet infogas eller uppdateras.</span><span class="sxs-lookup"><span data-stu-id="57bfc-149">There is also a **Timestamp** field for each record, which is set by Azure when an entity is inserted or updated.</span></span>
>
>

<span data-ttu-id="57bfc-150">Du kan också använda hello **entityGenerator** toocreate entiteter.</span><span class="sxs-lookup"><span data-stu-id="57bfc-150">You can also use hello **entityGenerator** toocreate entities.</span></span> <span data-ttu-id="57bfc-151">hello följande exempel skapar hello samma aktivitet entitet med hello **entityGenerator**.</span><span class="sxs-lookup"><span data-stu-id="57bfc-151">hello following example creates hello same task entity using hello **entityGenerator**.</span></span>

```nodejs
var entGen = azure.TableUtilities.entityGenerator;
var task = {
  PartitionKey: entGen.String('hometasks'),
  RowKey: entGen.String('1'),
  description: entGen.String('take out hello trash'),
  dueDate: entGen.DateTime(new Date(Date.UTC(2015, 6, 20))),
};
```

<span data-ttu-id="57bfc-152">tooadd en entitet tooyour tabell skicka hello entitet objektet toohello **insertEntity** metod.</span><span class="sxs-lookup"><span data-stu-id="57bfc-152">tooadd an entity tooyour table, pass hello entity object toohello **insertEntity** method.</span></span>

```nodejs
tableSvc.insertEntity('mytable',task, function (error, result, response) {
  if(!error){
    // Entity inserted
  }
});
```

<span data-ttu-id="57bfc-153">Om hello åtgärden lyckas `result` innehåller hello [ETag](http://en.wikipedia.org/wiki/HTTP_ETag) av hello infogad post och `response` innehåller information om hello igen.</span><span class="sxs-lookup"><span data-stu-id="57bfc-153">If hello operation is successful, `result` will contain hello [ETag](http://en.wikipedia.org/wiki/HTTP_ETag) of hello inserted record and `response` will contain information about hello operation.</span></span>

<span data-ttu-id="57bfc-154">Exempelsvar:</span><span class="sxs-lookup"><span data-stu-id="57bfc-154">Example response:</span></span>

```nodejs
{ '.metadata': { etag: 'W/"datetime\'2015-02-25T01%3A22%3A22.5Z\'"' } }
```

> [!NOTE]
> <span data-ttu-id="57bfc-155">Som standard **insertEntity** returnerar inte hello infogas entiteten som en del av hello `response` information.</span><span class="sxs-lookup"><span data-stu-id="57bfc-155">By default, **insertEntity** does not return hello inserted entity as part of hello `response` information.</span></span> <span data-ttu-id="57bfc-156">Om du planerar att andra åtgärder pågår på den här entiteten eller vill toocache hello information, kan det vara användbart toohave returnerade som en del av hello `result`.</span><span class="sxs-lookup"><span data-stu-id="57bfc-156">If you plan on performing other operations on this entity, or wish toocache hello information, it can be useful toohave it returned as part of hello `result`.</span></span> <span data-ttu-id="57bfc-157">Du kan göra detta genom att aktivera **echoContent** på följande sätt:</span><span class="sxs-lookup"><span data-stu-id="57bfc-157">You can do this by enabling **echoContent** as follows:</span></span>
>
> `tableSvc.insertEntity('mytable', task, {echoContent: true}, function (error, result, response) {...}`
>
>

## <a name="update-an-entity"></a><span data-ttu-id="57bfc-158">Uppdatera en entitet</span><span class="sxs-lookup"><span data-stu-id="57bfc-158">Update an entity</span></span>
<span data-ttu-id="57bfc-159">Det finns flera metoder tillgängliga tooupdate en befintlig entitet:</span><span class="sxs-lookup"><span data-stu-id="57bfc-159">There are multiple methods available tooupdate an existing entity:</span></span>

* <span data-ttu-id="57bfc-160">**replaceEntity** -uppdaterar en befintlig entitet genom att ersätta den</span><span class="sxs-lookup"><span data-stu-id="57bfc-160">**replaceEntity** - updates an existing entity by replacing it</span></span>
* <span data-ttu-id="57bfc-161">**mergeEntity** -uppdaterar en befintlig entitet genom att använda nya egenskapsvärden i hello befintlig entitet</span><span class="sxs-lookup"><span data-stu-id="57bfc-161">**mergeEntity** - updates an existing entity by merging new property values into hello existing entity</span></span>
* <span data-ttu-id="57bfc-162">**insertOrReplaceEntity** -uppdaterar en befintlig entitet genom att ersätta den.</span><span class="sxs-lookup"><span data-stu-id="57bfc-162">**insertOrReplaceEntity** - updates an existing entity by replacing it.</span></span> <span data-ttu-id="57bfc-163">Om inga entiteten finns kommer en ny att infogas</span><span class="sxs-lookup"><span data-stu-id="57bfc-163">If no entity exists, a new one will be inserted</span></span>
* <span data-ttu-id="57bfc-164">**insertOrMergeEntity** -uppdaterar en befintlig entitet genom att använda nya egenskapsvärden i hello befintliga.</span><span class="sxs-lookup"><span data-stu-id="57bfc-164">**insertOrMergeEntity** - updates an existing entity by merging new property values into hello existing.</span></span> <span data-ttu-id="57bfc-165">Om inga entiteten finns kommer en ny att infogas</span><span class="sxs-lookup"><span data-stu-id="57bfc-165">If no entity exists, a new one will be inserted</span></span>

<span data-ttu-id="57bfc-166">hello exemplet nedan visar uppdaterar en entitet med **replaceEntity**:</span><span class="sxs-lookup"><span data-stu-id="57bfc-166">hello following example demonstrates updating an entity using **replaceEntity**:</span></span>

```nodejs
tableSvc.replaceEntity('mytable', updatedTask, function(error, result, response){
  if(!error) {
    // Entity updated
  }
});
```

> [!NOTE]
> <span data-ttu-id="57bfc-167">Som standard kontrollerar uppdaterar en entitet inte toosee om hello data uppdateras tidigare har ändrats av en annan process.</span><span class="sxs-lookup"><span data-stu-id="57bfc-167">By default, updating an entity does not check toosee if hello data being updated has previously been modified by another process.</span></span> <span data-ttu-id="57bfc-168">toosupport samtidiga uppdateringar:</span><span class="sxs-lookup"><span data-stu-id="57bfc-168">toosupport concurrent updates:</span></span>
>
> 1. <span data-ttu-id="57bfc-169">Hämta hello ETag för hello-objekt som håller på att uppdateras.</span><span class="sxs-lookup"><span data-stu-id="57bfc-169">Get hello ETag of hello object being updated.</span></span> <span data-ttu-id="57bfc-170">Det här felet returneras som en del av hello `response` för varje entitet-relaterade åtgärden och kan hämtas via `response['.metadata'].etag`.</span><span class="sxs-lookup"><span data-stu-id="57bfc-170">This is returned as part of hello `response` for any entity-related operation and can be retrieved through `response['.metadata'].etag`.</span></span>
> 2. <span data-ttu-id="57bfc-171">När du utför en update-åtgärd på en entitet, Lägg till hämta information om hello ETag tidigare toohello ny entitet.</span><span class="sxs-lookup"><span data-stu-id="57bfc-171">When performing an update operation on an entity, add hello ETag information previously retrieved toohello new entity.</span></span> <span data-ttu-id="57bfc-172">Exempel:</span><span class="sxs-lookup"><span data-stu-id="57bfc-172">For example:</span></span>
>
>       <span data-ttu-id="57bfc-173">entity2 [.metadata] .etag = currentEtag;</span><span class="sxs-lookup"><span data-stu-id="57bfc-173">entity2['.metadata'].etag = currentEtag;</span></span>
> 3. <span data-ttu-id="57bfc-174">Utföra hello uppdateringen.</span><span class="sxs-lookup"><span data-stu-id="57bfc-174">Perform hello update operation.</span></span> <span data-ttu-id="57bfc-175">Om hello entiteten har ändrats sedan du hämtade hello ETag-värde, till exempel en annan instans av programmet, en `error` returneras om inte uppfylldes hello update villkoret i hello-begäran.</span><span class="sxs-lookup"><span data-stu-id="57bfc-175">If hello entity has been modified since you retrieved hello ETag value, such as another instance of your application, an `error` will be returned stating that hello update condition specified in hello request was not satisfied.</span></span>
>
>

<span data-ttu-id="57bfc-176">Med **replaceEntity** och **mergeEntity**om hello entitet som ska uppdateras inte finns, hello update-åtgärden kommer att misslyckas.</span><span class="sxs-lookup"><span data-stu-id="57bfc-176">With **replaceEntity** and **mergeEntity**, if hello entity that is being updated doesn't exist, then hello update operation will fail.</span></span> <span data-ttu-id="57bfc-177">Om du inte vill toostore en entitet oavsett om den redan finns, använder du därför **insertOrReplaceEntity** eller **insertOrMergeEntity**.</span><span class="sxs-lookup"><span data-stu-id="57bfc-177">Therefore if you wish toostore an entity regardless of whether it already exists, use **insertOrReplaceEntity** or **insertOrMergeEntity**.</span></span>

<span data-ttu-id="57bfc-178">Hej `result` för lyckade uppdatering operations innehåller hello **Etag** av hello uppdatera entiteten.</span><span class="sxs-lookup"><span data-stu-id="57bfc-178">hello `result` for successful update operations will contain hello **Etag** of hello updated entity.</span></span>

## <a name="work-with-groups-of-entities"></a><span data-ttu-id="57bfc-179">Arbeta med grupper av entiteter</span><span class="sxs-lookup"><span data-stu-id="57bfc-179">Work with groups of entities</span></span>
<span data-ttu-id="57bfc-180">Ibland blir meningsfullt toosubmit flera åtgärder tillsammans i en batch tooensure atomiska bearbetning av hello-servern.</span><span class="sxs-lookup"><span data-stu-id="57bfc-180">Sometimes it makes sense toosubmit multiple operations together in a batch tooensure atomic processing by hello server.</span></span> <span data-ttu-id="57bfc-181">tooaccomplish som använder hello **TableBatch** klassen toocreate en batch och sedan använda hello **executeBatch** metod för **TableService** tooperform hello batchar åtgärder.</span><span class="sxs-lookup"><span data-stu-id="57bfc-181">tooaccomplish that, use hello **TableBatch** class toocreate a batch, and then use hello **executeBatch** method of **TableService** tooperform hello batched operations.</span></span>

 <span data-ttu-id="57bfc-182">hello som följande exempel visar hur du skickar två entiteter i en batch:</span><span class="sxs-lookup"><span data-stu-id="57bfc-182">hello following example demonstrates submitting two entities in a batch:</span></span>

```nodejs
var task1 = {
  PartitionKey: {'_':'hometasks'},
  RowKey: {'_': '1'},
  description: {'_':'Take out hello trash'},
  dueDate: {'_':new Date(2015, 6, 20)}
};
var task2 = {
  PartitionKey: {'_':'hometasks'},
  RowKey: {'_': '2'},
  description: {'_':'Wash hello dishes'},
  dueDate: {'_':new Date(2015, 6, 20)}
};

var batch = new azure.TableBatch();

batch.insertEntity(task1, {echoContent: true});
batch.insertEntity(task2, {echoContent: true});

tableSvc.executeBatch('mytable', batch, function (error, result, response) {
  if(!error) {
    // Batch completed
  }
});
```

<span data-ttu-id="57bfc-183">För lyckad batchåtgärder `result` innehåller information för varje åtgärd i hello batch.</span><span class="sxs-lookup"><span data-stu-id="57bfc-183">For successful batch operations, `result` will contain information for each operation in hello batch.</span></span>

### <a name="work-with-batched-operations"></a><span data-ttu-id="57bfc-184">Arbeta med batch-åtgärder</span><span class="sxs-lookup"><span data-stu-id="57bfc-184">Work with batched operations</span></span>
<span data-ttu-id="57bfc-185">Åtgärder för att lägga till tooa batch kan kontrolleras genom att visa hello `operations` egenskapen.</span><span class="sxs-lookup"><span data-stu-id="57bfc-185">Operations added tooa batch can be inspected by viewing hello `operations` property.</span></span> <span data-ttu-id="57bfc-186">Du kan också använda hello följande metoder toowork med åtgärder:</span><span class="sxs-lookup"><span data-stu-id="57bfc-186">You can also use hello following methods toowork with operations:</span></span>

* <span data-ttu-id="57bfc-187">**Rensa** -rensar alla åtgärder från en grupp</span><span class="sxs-lookup"><span data-stu-id="57bfc-187">**clear** - clears all operations from a batch</span></span>
* <span data-ttu-id="57bfc-188">**getOperations** -hämtar en åtgärd från hello batch</span><span class="sxs-lookup"><span data-stu-id="57bfc-188">**getOperations** - gets an operation from hello batch</span></span>
* <span data-ttu-id="57bfc-189">**hasOperations** -returnerar true om hello batch innehåller åtgärder</span><span class="sxs-lookup"><span data-stu-id="57bfc-189">**hasOperations** - returns true if hello batch contains operations</span></span>
* <span data-ttu-id="57bfc-190">**removeOperations** -tar bort en åtgärd</span><span class="sxs-lookup"><span data-stu-id="57bfc-190">**removeOperations** - removes an operation</span></span>
* <span data-ttu-id="57bfc-191">**storlek** -returnerar hello antalet åtgärder i hello batch</span><span class="sxs-lookup"><span data-stu-id="57bfc-191">**size** - returns hello number of operations in hello batch</span></span>

## <a name="retrieve-an-entity-by-key"></a><span data-ttu-id="57bfc-192">Hämta en entitet med nyckeln</span><span class="sxs-lookup"><span data-stu-id="57bfc-192">Retrieve an entity by key</span></span>
<span data-ttu-id="57bfc-193">en specifik enhet baserat på hello tooreturn **PartitionKey** och **RowKey**, använda hello **retrieveEntity** metod.</span><span class="sxs-lookup"><span data-stu-id="57bfc-193">tooreturn a specific entity based on hello **PartitionKey** and **RowKey**, use hello **retrieveEntity** method.</span></span>

```nodejs
tableSvc.retrieveEntity('mytable', 'hometasks', '1', function(error, result, response){
  if(!error){
    // result contains hello entity
  }
});
```

<span data-ttu-id="57bfc-194">När den här åtgärden är klar, `result` innehåller hello entitet.</span><span class="sxs-lookup"><span data-stu-id="57bfc-194">Once this operation is complete, `result` will contain hello entity.</span></span>

## <a name="query-a-set-of-entities"></a><span data-ttu-id="57bfc-195">Fråga en uppsättning enheter</span><span class="sxs-lookup"><span data-stu-id="57bfc-195">Query a set of entities</span></span>
<span data-ttu-id="57bfc-196">tooquery en tabell, använder hello **TableQuery** objekt toobuild upp ett frågeuttryck med hello följande villkor:</span><span class="sxs-lookup"><span data-stu-id="57bfc-196">tooquery a table, use hello **TableQuery** object toobuild up a query expression using hello following clauses:</span></span>

* <span data-ttu-id="57bfc-197">**Välj** -hello fält toobe har returnerats från hello fråga</span><span class="sxs-lookup"><span data-stu-id="57bfc-197">**select** - hello fields toobe returned from hello query</span></span>
* <span data-ttu-id="57bfc-198">**där** - hello där satsen</span><span class="sxs-lookup"><span data-stu-id="57bfc-198">**where** - hello where clause</span></span>

  * <span data-ttu-id="57bfc-199">**och** – en `and` where-villkor</span><span class="sxs-lookup"><span data-stu-id="57bfc-199">**and** - an `and` where condition</span></span>
  * <span data-ttu-id="57bfc-200">**eller** – en `or` where-villkor</span><span class="sxs-lookup"><span data-stu-id="57bfc-200">**or** - an `or` where condition</span></span>
* <span data-ttu-id="57bfc-201">**TOP** -hello antalet objekt toofetch</span><span class="sxs-lookup"><span data-stu-id="57bfc-201">**top** - hello number of items toofetch</span></span>

<span data-ttu-id="57bfc-202">hello skapas följande exempel en fråga som returnerar hello översta fem objekt med en PartitionKey 'hometasks'.</span><span class="sxs-lookup"><span data-stu-id="57bfc-202">hello following example builds a query that will return hello top five items with a PartitionKey of 'hometasks'.</span></span>

```nodejs
var query = new azure.TableQuery()
  .top(5)
  .where('PartitionKey eq ?', 'hometasks');
```

<span data-ttu-id="57bfc-203">Eftersom **Välj** inte används, alla fält som ska returneras.</span><span class="sxs-lookup"><span data-stu-id="57bfc-203">Since **select** is not used, all fields will be returned.</span></span> <span data-ttu-id="57bfc-204">tooperform hello frågan mot en tabell, Använd **queryEntities**.</span><span class="sxs-lookup"><span data-stu-id="57bfc-204">tooperform hello query against a table, use **queryEntities**.</span></span> <span data-ttu-id="57bfc-205">hello används följande exempel den här frågan tooreturn entiteter från ”mytable” som prefix.</span><span class="sxs-lookup"><span data-stu-id="57bfc-205">hello following example uses this query tooreturn entities from 'mytable'.</span></span>

```nodejs
tableSvc.queryEntities('mytable',query, null, function(error, result, response) {
  if(!error) {
    // query was successful
  }
});
```

<span data-ttu-id="57bfc-206">Om detta lyckas `result.entries` innehåller en matris med entiteter som matchar hello fråga.</span><span class="sxs-lookup"><span data-stu-id="57bfc-206">If successful, `result.entries` will contain an array of entities that match hello query.</span></span> <span data-ttu-id="57bfc-207">Om hello frågan var tooreturn alla entiteter `result.continuationToken` kommer att vara icke -*null* och kan användas som hello tredje parametern för **queryEntities** tooretrieve resultat.</span><span class="sxs-lookup"><span data-stu-id="57bfc-207">If hello query was unable tooreturn all entities, `result.continuationToken` will be non-*null* and can be used as hello third parameter of **queryEntities** tooretrieve more results.</span></span> <span data-ttu-id="57bfc-208">Hello första fråga, Använd *null* för hello tredje parameter.</span><span class="sxs-lookup"><span data-stu-id="57bfc-208">For hello initial query, use *null* for hello third parameter.</span></span>

### <a name="query-a-subset-of-entity-properties"></a><span data-ttu-id="57bfc-209">Fråga en deluppsättning entitetsegenskaper</span><span class="sxs-lookup"><span data-stu-id="57bfc-209">Query a subset of entity properties</span></span>
<span data-ttu-id="57bfc-210">En tooa frågetabellen kan hämta bara några fält från en entitet.</span><span class="sxs-lookup"><span data-stu-id="57bfc-210">A query tooa table can retrieve just a few fields from an entity.</span></span>
<span data-ttu-id="57bfc-211">Detta minskar bandbredden och kan förbättra frågeprestanda, särskilt för stora entiteter.</span><span class="sxs-lookup"><span data-stu-id="57bfc-211">This reduces bandwidth and can improve query performance, especially for large entities.</span></span> <span data-ttu-id="57bfc-212">Använd hello **Välj** -satsen och pass hello namnen på hello fält toobe returneras.</span><span class="sxs-lookup"><span data-stu-id="57bfc-212">Use hello **select** clause and pass hello names of hello fields toobe returned.</span></span> <span data-ttu-id="57bfc-213">Till exempel hello följande fråga returnerar endast hello **beskrivning** och **dueDate** fält.</span><span class="sxs-lookup"><span data-stu-id="57bfc-213">For example, hello following query will return only hello **description** and **dueDate** fields.</span></span>

```nodejs
var query = new azure.TableQuery()
  .select(['description', 'dueDate'])
  .top(5)
  .where('PartitionKey eq ?', 'hometasks');
```

## <a name="delete-an-entity"></a><span data-ttu-id="57bfc-214">Ta bort en entitet</span><span class="sxs-lookup"><span data-stu-id="57bfc-214">Delete an entity</span></span>
<span data-ttu-id="57bfc-215">Du kan ta bort en entitet med dess partition och raden nycklar.</span><span class="sxs-lookup"><span data-stu-id="57bfc-215">You can delete an entity using its partition and row keys.</span></span> <span data-ttu-id="57bfc-216">I det här exemplet hello **task1** objektet innehåller hello **RowKey** och **PartitionKey** värdena för hello entiteten toobe tas bort.</span><span class="sxs-lookup"><span data-stu-id="57bfc-216">In this example, hello **task1** object contains hello **RowKey** and **PartitionKey** values of hello entity toobe deleted.</span></span> <span data-ttu-id="57bfc-217">Sedan hello objektet skickas toohello **deleteEntity** metod.</span><span class="sxs-lookup"><span data-stu-id="57bfc-217">Then hello object is passed toohello **deleteEntity** method.</span></span>

```nodejs
var task = {
  PartitionKey: {'_':'hometasks'},
  RowKey: {'_': '1'}
};

tableSvc.deleteEntity('mytable', task, function(error, response){
  if(!error) {
    // Entity deleted
  }
});
```

> [!NOTE]
> <span data-ttu-id="57bfc-218">Överväg att använda ETags när du tar bort objekt, tooensure hello objektet inte har ändrats av en annan process.</span><span class="sxs-lookup"><span data-stu-id="57bfc-218">Consider using ETags when deleting items, tooensure that hello item hasn't been modified by another process.</span></span> <span data-ttu-id="57bfc-219">Se [uppdatera en entitet](#update-an-entity) information om hur du använder ETags.</span><span class="sxs-lookup"><span data-stu-id="57bfc-219">See [Update an entity](#update-an-entity) for information on using ETags.</span></span>
>
>

## <a name="delete-a-table"></a><span data-ttu-id="57bfc-220">Ta bort en tabell</span><span class="sxs-lookup"><span data-stu-id="57bfc-220">Delete a table</span></span>
<span data-ttu-id="57bfc-221">följande kod hello tar bort en tabell från ett lagringskonto.</span><span class="sxs-lookup"><span data-stu-id="57bfc-221">hello following code deletes a table from a storage account.</span></span>

```nodejs
tableSvc.deleteTable('mytable', function(error, response){
    if(!error){
        // Table deleted
    }
});
```

<span data-ttu-id="57bfc-222">Om du är osäker på om det finns hello tabell använder **deleteTableIfExists**.</span><span class="sxs-lookup"><span data-stu-id="57bfc-222">If you are uncertain whether hello table exists, use **deleteTableIfExists**.</span></span>

## <a name="use-continuation-tokens"></a><span data-ttu-id="57bfc-223">Använd fortsättning token</span><span class="sxs-lookup"><span data-stu-id="57bfc-223">Use continuation tokens</span></span>
<span data-ttu-id="57bfc-224">När du frågar tabeller för stora mängder resultat, leta efter fortsättning token.</span><span class="sxs-lookup"><span data-stu-id="57bfc-224">When you are querying tables for large amounts of results, look for continuation tokens.</span></span> <span data-ttu-id="57bfc-225">Det kan finnas stora mängder data tillgängliga för din fråga att du inte kanske vet om du inte skapa toorecognize när en fortsättningstoken är närvarande.</span><span class="sxs-lookup"><span data-stu-id="57bfc-225">There may be large amounts of data available for your query that you might not realize if you do not build toorecognize when a continuation token is present.</span></span>

<span data-ttu-id="57bfc-226">hello resultat objekt returnerades vid fråga entiteter anger en `continuationToken` egenskapen när dessa token finns.</span><span class="sxs-lookup"><span data-stu-id="57bfc-226">hello results object returned during querying entities sets a `continuationToken` property when such a token is present.</span></span> <span data-ttu-id="57bfc-227">Du kan sedan använda den när du utför en fråga toocontinue toomove över hello partition och tabellen entiteter.</span><span class="sxs-lookup"><span data-stu-id="57bfc-227">You can then use this when performing a query toocontinue toomove across hello partition and table entities.</span></span>

<span data-ttu-id="57bfc-228">När du frågar, kan en continuationToken-parameter tillhandahållas mellan hello frågan objektinstansen och hello Återanropsfunktionen:</span><span class="sxs-lookup"><span data-stu-id="57bfc-228">When querying, a continuationToken parameter may be provided between hello query object instance and hello callback function:</span></span>

```nodejs
var nextContinuationToken = null;
dc.table.queryEntities(tableName,
    query,
    nextContinuationToken,
    function (error, results) {
        if (error) throw error;

        // iterate through results.entries with results

        if (results.continuationToken) {
            nextContinuationToken = results.continuationToken;
        }

    });
```

<span data-ttu-id="57bfc-229">Om du inspektera hello `continuationToken` objekt, hittar du egenskaper som `nextPartitionKey`, `nextRowKey` och `targetLocation`, vilket kan vara används tooiterate via alla hello resultat.</span><span class="sxs-lookup"><span data-stu-id="57bfc-229">If you inspect hello `continuationToken` object, you will find properties such as `nextPartitionKey`, `nextRowKey` and `targetLocation`, which can be used tooiterate through all hello results.</span></span>

<span data-ttu-id="57bfc-230">Det finns också ett exempel på en fortsättning inom hello Azure Storage Node.js lagringsplatsen på GitHub.</span><span class="sxs-lookup"><span data-stu-id="57bfc-230">There is also a continuation sample within hello Azure Storage Node.js repo on GitHub.</span></span> <span data-ttu-id="57bfc-231">Leta efter `examples/samples/continuationsample.js`.</span><span class="sxs-lookup"><span data-stu-id="57bfc-231">Look for `examples/samples/continuationsample.js`.</span></span>

## <a name="work-with-shared-access-signatures"></a><span data-ttu-id="57bfc-232">Arbeta med signaturer för delad åtkomst</span><span class="sxs-lookup"><span data-stu-id="57bfc-232">Work with shared access signatures</span></span>
<span data-ttu-id="57bfc-233">Signaturer för delad åtkomst (SAS) är ett säkert sätt tooprovide detaljerade åtkomst tootables utan att erbjuda dina lagringskontonamn eller nycklar.</span><span class="sxs-lookup"><span data-stu-id="57bfc-233">Shared access signatures (SAS) are a secure way tooprovide granular access tootables without providing your storage account name or keys.</span></span> <span data-ttu-id="57bfc-234">Säkerhetsassociationer är ofta använda tooprovide begränsad åtkomst tooyour data, till exempel att tillåta att en mobil app tooquery poster.</span><span class="sxs-lookup"><span data-stu-id="57bfc-234">SAS are often used tooprovide limited access tooyour data, such as allowing a mobile app tooquery records.</span></span>

<span data-ttu-id="57bfc-235">En betrodda program, till exempel en molnbaserad tjänst genererar en SAS med hello **generateSharedAccessSignature** av hello **TableService**, och ger den tooan inte är betrodd eller delvis betrodda program till exempel en mobil app.</span><span class="sxs-lookup"><span data-stu-id="57bfc-235">A trusted application such as a cloud-based service generates a SAS using hello **generateSharedAccessSignature** of hello **TableService**, and provides it tooan untrusted or semi-trusted application such as a mobile app.</span></span> <span data-ttu-id="57bfc-236">hello SAS genereras med en princip som beskriver hello start- och slutdatum under vilka hello SAS är giltigt, samt hello åtkomst nivån beviljade toohello SAS innehavaren.</span><span class="sxs-lookup"><span data-stu-id="57bfc-236">hello SAS is generated using a policy, which describes hello start and end dates during which hello SAS is valid, as well as hello access level granted toohello SAS holder.</span></span>

<span data-ttu-id="57bfc-237">hello följande exempel skapar en ny princip för delad åtkomst som gör att hello SAS innehavaren tooquery (r) hello tabell och upphör att gälla 100 minuter efter hello när det skapas.</span><span class="sxs-lookup"><span data-stu-id="57bfc-237">hello following example generates a new shared access policy that will allow hello SAS holder tooquery ('r') hello table, and expires 100 minutes after hello time it is created.</span></span>

```nodejs
var startDate = new Date();
var expiryDate = new Date(startDate);
expiryDate.setMinutes(startDate.getMinutes() + 100);
startDate.setMinutes(startDate.getMinutes() - 100);

var sharedAccessPolicy = {
  AccessPolicy: {
    Permissions: azure.TableUtilities.SharedAccessPermissions.QUERY,
    Start: startDate,
    Expiry: expiryDate
  },
};

var tableSAS = tableSvc.generateSharedAccessSignature('mytable', sharedAccessPolicy);
var host = tableSvc.host;
```

<span data-ttu-id="57bfc-238">Observera att information om hello värden måste anges också som krävs när hello SAS innehavaren försöker tooaccess hello tabell.</span><span class="sxs-lookup"><span data-stu-id="57bfc-238">Note that hello host information must be provided also, as it is required when hello SAS holder attempts tooaccess hello table.</span></span>

<span data-ttu-id="57bfc-239">Hej klientprogrammet och sedan använder hello SAS med **TableServiceWithSAS** tooperform åtgärder mot hello tabell.</span><span class="sxs-lookup"><span data-stu-id="57bfc-239">hello client application then uses hello SAS with **TableServiceWithSAS** tooperform operations against hello table.</span></span> <span data-ttu-id="57bfc-240">följande exempel hello ansluter toohello tabell och utför en fråga.</span><span class="sxs-lookup"><span data-stu-id="57bfc-240">hello following example connects toohello table and performs a query.</span></span>

```nodejs
var sharedTableService = azure.createTableServiceWithSas(host, tableSAS);
var query = azure.TableQuery()
  .where('PartitionKey eq ?', 'hometasks');

sharedTableService.queryEntities(query, null, function(error, result, response) {
  if(!error) {
    // result contains hello entities
  }
});
```

<span data-ttu-id="57bfc-241">Eftersom hello SAS genererades med endast fråga åtkomst, om ett försök gjordes tooinsert, uppdatera eller ta bort entiteter, returneras ett fel.</span><span class="sxs-lookup"><span data-stu-id="57bfc-241">Since hello SAS was generated with only query access, if an attempt were made tooinsert, update, or delete entities, an error would be returned.</span></span>

### <a name="access-control-lists"></a><span data-ttu-id="57bfc-242">Åtkomstkontrollistor</span><span class="sxs-lookup"><span data-stu-id="57bfc-242">Access Control Lists</span></span>
<span data-ttu-id="57bfc-243">Du kan också använda en åtkomstkontrollista (ACL) tooset hello åtkomstprincip för en SAS.</span><span class="sxs-lookup"><span data-stu-id="57bfc-243">You can also use an Access Control List (ACL) tooset hello access policy for a SAS.</span></span> <span data-ttu-id="57bfc-244">Detta är användbart om du vill tooallow flera klienter tooaccess hello tabell, men ange olika principer för varje klient.</span><span class="sxs-lookup"><span data-stu-id="57bfc-244">This is useful if you wish tooallow multiple clients tooaccess hello table, but provide different access policies for each client.</span></span>

<span data-ttu-id="57bfc-245">En ACL implementeras med hjälp av en matris med principer för åtkomst med ett ID som är associerade med varje princip.</span><span class="sxs-lookup"><span data-stu-id="57bfc-245">An ACL is implemented using an array of access policies, with an ID associated with each policy.</span></span> <span data-ttu-id="57bfc-246">hello följande exempel definierar två principer, en för 'Användare1' och en för 'användare2':</span><span class="sxs-lookup"><span data-stu-id="57bfc-246">hello following example defines two policies, one for 'user1' and one for 'user2':</span></span>

```nodejs
var sharedAccessPolicy = {
  user1: {
    Permissions: azure.TableUtilities.SharedAccessPermissions.QUERY,
    Start: startDate,
    Expiry: expiryDate
  },
  user2: {
    Permissions: azure.TableUtilities.SharedAccessPermissions.ADD,
    Start: startDate,
    Expiry: expiryDate
  }
};
```

<span data-ttu-id="57bfc-247">följande exempel hämtar hello hello aktuella ACL för hello **hometasks** tabell och lägger sedan till hello nya principer med hjälp av **setTableAcl**.</span><span class="sxs-lookup"><span data-stu-id="57bfc-247">hello following example gets hello current ACL for hello **hometasks** table, and then adds hello new policies using **setTableAcl**.</span></span> <span data-ttu-id="57bfc-248">Den här metoden kan:</span><span class="sxs-lookup"><span data-stu-id="57bfc-248">This approach allows:</span></span>

```nodejs
var extend = require('extend');
tableSvc.getTableAcl('hometasks', function(error, result, response) {
if(!error){
    var newSignedIdentifiers = extend(true, result.signedIdentifiers, sharedAccessPolicy);
    tableSvc.setTableAcl('hometasks', newSignedIdentifiers, function(error, result, response){
      if(!error){
        // ACL set
      }
    });
  }
});
```

<span data-ttu-id="57bfc-249">En gång hello ACL har ställts in, du kan sedan skapa en SAS baserat på hello-ID för en princip.</span><span class="sxs-lookup"><span data-stu-id="57bfc-249">Once hello ACL has been set, you can then create a SAS based on hello ID for a policy.</span></span> <span data-ttu-id="57bfc-250">hello följande exempel skapas en ny SAS för 'användare2':</span><span class="sxs-lookup"><span data-stu-id="57bfc-250">hello following example creates a new SAS for 'user2':</span></span>

```nodejs
tableSAS = tableSvc.generateSharedAccessSignature('hometasks', { Id: 'user2' });
```

## <a name="next-steps"></a><span data-ttu-id="57bfc-251">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="57bfc-251">Next steps</span></span>
<span data-ttu-id="57bfc-252">Mer information finns i följande resurser hello.</span><span class="sxs-lookup"><span data-stu-id="57bfc-252">For more information, see hello following resources.</span></span>

* <span data-ttu-id="57bfc-253">[Microsoft Azure Lagringsutforskaren](../vs-azure-tools-storage-manage-with-storage-explorer.md) är en gratis, fristående app från Microsoft som möjliggör toowork visuellt med Azure Storage-data i Windows, macOS och Linux.</span><span class="sxs-lookup"><span data-stu-id="57bfc-253">[Microsoft Azure Storage Explorer](../vs-azure-tools-storage-manage-with-storage-explorer.md) is a free, standalone app from Microsoft that enables you toowork visually with Azure Storage data on Windows, macOS, and Linux.</span></span>
* <span data-ttu-id="57bfc-254">[Azure Storage SDK: N för noden](https://github.com/Azure/azure-storage-node) databasen på GitHub.</span><span class="sxs-lookup"><span data-stu-id="57bfc-254">[Azure Storage SDK for Node](https://github.com/Azure/azure-storage-node) repository on GitHub.</span></span>
* [<span data-ttu-id="57bfc-255">Node.js Developer Center</span><span class="sxs-lookup"><span data-stu-id="57bfc-255">Node.js Developer Center</span></span>](/develop/nodejs/)
* [<span data-ttu-id="57bfc-256">Skapa och distribuera en Node.js-programmet tooan Azure-webbplats</span><span class="sxs-lookup"><span data-stu-id="57bfc-256">Create and deploy a Node.js application tooan Azure website</span></span>](../app-service-web/app-service-web-get-started-nodejs.md)
