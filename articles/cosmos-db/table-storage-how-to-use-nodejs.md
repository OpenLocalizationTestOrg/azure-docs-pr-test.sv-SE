---
title: "Hur du använder Azure Table storage från Node.js | Microsoft Docs"
description: "Lagra strukturerade data i molnet med hjälp av Azure Table Storage, en NoSQL-databas."
services: cosmos-db
documentationcenter: nodejs
author: mimig1
manager: jhubbard
editor: tysonn
ms.assetid: fc2e33d2-c5da-4861-8503-53fdc25750de
ms.service: cosmos-db
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: nodejs
ms.topic: article
ms.date: 12/08/2016
ms.author: mimig
ms.openlocfilehash: 539212c6abe7738c022d67245f8992516f0899ff
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/29/2017
---
# <a name="how-to-use-azure-table-storage-from-nodejs"></a><span data-ttu-id="44382-103">Hur du använder Azure Table storage från Node.js</span><span class="sxs-lookup"><span data-stu-id="44382-103">How to use Azure Table storage from Node.js</span></span>
[!INCLUDE [storage-selector-table-include](../../includes/storage-selector-table-include.md)]
[!INCLUDE [storage-table-cosmos-db-langsoon-tip-include](../../includes/storage-table-cosmos-db-langsoon-tip-include.md)]

## <a name="overview"></a><span data-ttu-id="44382-104">Översikt</span><span class="sxs-lookup"><span data-stu-id="44382-104">Overview</span></span>
<span data-ttu-id="44382-105">Det här avsnittet beskrivs hur du utför vanliga scenarier som använder Azure Table-tjänsten i ett Node.js-program.</span><span class="sxs-lookup"><span data-stu-id="44382-105">This topic shows how to perform common scenarios using the Azure Table service in a Node.js application.</span></span>

<span data-ttu-id="44382-106">Kodexemplen i det här avsnittet förutsätter att du redan har ett Node.js-program.</span><span class="sxs-lookup"><span data-stu-id="44382-106">The code examples in this topic assume you already have a Node.js application.</span></span> <span data-ttu-id="44382-107">Information om hur du skapar en Node.js-program i Azure finns i någon av följande avsnitt:</span><span class="sxs-lookup"><span data-stu-id="44382-107">For information about how to create a Node.js application in Azure, see any of these topics:</span></span>

* [<span data-ttu-id="44382-108">Skapa en Node.js-webbapp i Azure App Service</span><span class="sxs-lookup"><span data-stu-id="44382-108">Create a Node.js web app in Azure App Service</span></span>](../app-service-web/app-service-web-get-started-nodejs.md)
* [<span data-ttu-id="44382-109">Skapa och distribuera en Node.js-webbapp till Azure med WebMatrix</span><span class="sxs-lookup"><span data-stu-id="44382-109">Build and deploy a Node.js web app to Azure using WebMatrix</span></span>](../app-service-web/web-sites-nodejs-use-webmatrix.md)
* <span data-ttu-id="44382-110">[Skapa och distribuera ett Node.js-program till en Azure-molntjänst](../cloud-services/cloud-services-nodejs-develop-deploy-app.md) (med Windows PowerShell)</span><span class="sxs-lookup"><span data-stu-id="44382-110">[Build and deploy a Node.js application to an Azure Cloud Service](../cloud-services/cloud-services-nodejs-develop-deploy-app.md) (using Windows PowerShell)</span></span>

[!INCLUDE [storage-table-concepts-include](../../includes/storage-table-concepts-include.md)]

[!INCLUDE [storage-create-account-include](../../includes/storage-create-account-include.md)]

## <a name="configure-your-application-to-access-azure-storage"></a><span data-ttu-id="44382-111">Konfigurera ditt program för att få åtkomst till Azure Storage</span><span class="sxs-lookup"><span data-stu-id="44382-111">Configure your application to access Azure Storage</span></span>
<span data-ttu-id="44382-112">Om du vill använda Azure Storage, behöver du Azure Storage SDK: N för Node.js som innehåller en uppsättning bekvämlighet bibliotek som kommunicerar med storage REST-tjänster.</span><span class="sxs-lookup"><span data-stu-id="44382-112">To use Azure Storage, you need the Azure Storage SDK for Node.js, which includes a set of convenience libraries that communicate with the storage REST services.</span></span>

### <a name="use-node-package-manager-npm-to-install-the-package"></a><span data-ttu-id="44382-113">Använd noden Package Manager (NPM) för att installera paketet</span><span class="sxs-lookup"><span data-stu-id="44382-113">Use Node Package Manager (NPM) to install the package</span></span>
1. <span data-ttu-id="44382-114">Använd ett kommandoradsgränssnitt som **PowerShell** (Windows), **Terminal** (Mac), eller **Bash** (Unix) och navigera till mappen där du skapade ditt program.</span><span class="sxs-lookup"><span data-stu-id="44382-114">Use a command-line interface such as **PowerShell** (Windows), **Terminal** (Mac), or **Bash** (Unix), and navigate to the folder where you created your application.</span></span>
2. <span data-ttu-id="44382-115">Typen **npm installera azure-lagring** i kommandofönstret.</span><span class="sxs-lookup"><span data-stu-id="44382-115">Type **npm install azure-storage** in the command window.</span></span> <span data-ttu-id="44382-116">Utdata från kommandot liknar följande exempel.</span><span class="sxs-lookup"><span data-stu-id="44382-116">Output from the command is similar to the following example.</span></span>

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
3. <span data-ttu-id="44382-117">Du kan köra manuellt på **ls** kommando för att kontrollera att en **nod\_moduler** mappen har skapats.</span><span class="sxs-lookup"><span data-stu-id="44382-117">You can manually run the **ls** command to verify that a **node\_modules** folder was created.</span></span> <span data-ttu-id="44382-118">I mappen hittar du den **azure storage** paket som innehåller de bibliotek som du behöver åtkomst till lagring.</span><span class="sxs-lookup"><span data-stu-id="44382-118">Inside that folder you will find the **azure-storage** package, which contains the libraries you need to access storage.</span></span>

### <a name="import-the-package"></a><span data-ttu-id="44382-119">Importera paketet</span><span class="sxs-lookup"><span data-stu-id="44382-119">Import the package</span></span>
<span data-ttu-id="44382-120">Lägg till följande kod högst upp i den **server.js** filen i ditt program:</span><span class="sxs-lookup"><span data-stu-id="44382-120">Add the following code to the top of the **server.js** file in your application:</span></span>

```nodejs
var azure = require('azure-storage');
```

## <a name="set-up-an-azure-storage-connection"></a><span data-ttu-id="44382-121">Skapa en Azure Storage-anslutning</span><span class="sxs-lookup"><span data-stu-id="44382-121">Set up an Azure Storage connection</span></span>
<span data-ttu-id="44382-122">Azure-modulen läses miljövariablerna AZURE\_lagring\_konto och AZURE\_lagring\_åtkomst\_nyckel eller AZURE\_lagring\_anslutning\_sträng för information som krävs för att ansluta till Azure storage-konto.</span><span class="sxs-lookup"><span data-stu-id="44382-122">The azure module will read the environment variables AZURE\_STORAGE\_ACCOUNT and AZURE\_STORAGE\_ACCESS\_KEY, or AZURE\_STORAGE\_CONNECTION\_STRING for information required to connect to your Azure storage account.</span></span> <span data-ttu-id="44382-123">Om de här miljövariablerna inte har angetts måste du ange kontoinformationen vid anrop av **TableService**.</span><span class="sxs-lookup"><span data-stu-id="44382-123">If these environment variables are not set, you must specify the account information when calling **TableService**.</span></span>

<span data-ttu-id="44382-124">Ett exempel på Ange miljövariabler i den [Azure-portalen](https://portal.azure.com) en Azure-webbplats finns [Node.js-webbapp som använder tjänsten Azure Table](../app-service-web/storage-nodejs-use-table-storage-web-site.md).</span><span class="sxs-lookup"><span data-stu-id="44382-124">For an example of setting the environment variables in the [Azure portal](https://portal.azure.com) for an Azure Website, see [Node.js web app using the Azure Table Service](../app-service-web/storage-nodejs-use-table-storage-web-site.md).</span></span>

## <a name="create-a-table"></a><span data-ttu-id="44382-125">Skapa en tabell</span><span class="sxs-lookup"><span data-stu-id="44382-125">Create a table</span></span>
<span data-ttu-id="44382-126">Följande kod skapar en **TableService** objekt och används för att skapa en ny tabell.</span><span class="sxs-lookup"><span data-stu-id="44382-126">The following code creates a **TableService** object and uses it to create a new table.</span></span> <span data-ttu-id="44382-127">Lägg till följande längst upp i **server.js**.</span><span class="sxs-lookup"><span data-stu-id="44382-127">Add the following near the top of **server.js**.</span></span>

```nodejs
var tableSvc = azure.createTableService();
```

<span data-ttu-id="44382-128">Anropet till **createTableIfNotExists** skapa en ny tabell med det angivna namnet om det inte redan finns.</span><span class="sxs-lookup"><span data-stu-id="44382-128">The call to **createTableIfNotExists** will create a new table with the specified name if it does not already exist.</span></span> <span data-ttu-id="44382-129">I följande exempel skapas en ny tabell med namnet ”mytable” som prefix om det inte redan finns:</span><span class="sxs-lookup"><span data-stu-id="44382-129">The following example creates a new table named 'mytable' if it does not already exist:</span></span>

```nodejs
tableSvc.createTableIfNotExists('mytable', function(error, result, response){
  if(!error){
    // Table exists or created
  }
});
```

<span data-ttu-id="44382-130">Den `result.created` blir `true` om en ny tabell skapas och `false` om tabellen redan finns.</span><span class="sxs-lookup"><span data-stu-id="44382-130">The `result.created` will be `true` if a new table is created, and `false` if the table already exists.</span></span> <span data-ttu-id="44382-131">Den `response` innehåller information om begäran.</span><span class="sxs-lookup"><span data-stu-id="44382-131">The `response` will contain information about the request.</span></span>

### <a name="filters"></a><span data-ttu-id="44382-132">Filter</span><span class="sxs-lookup"><span data-stu-id="44382-132">Filters</span></span>
<span data-ttu-id="44382-133">Valfria filtrering åtgärder kan användas för åtgärder som utförs med hjälp av **TableService**.</span><span class="sxs-lookup"><span data-stu-id="44382-133">Optional filtering operations can be applied to operations performed using **TableService**.</span></span> <span data-ttu-id="44382-134">Filtrering operations kan innehålla loggning, försöker automatiskt igen och så vidare. Filtren är objekt som implementerar en metod med signaturen:</span><span class="sxs-lookup"><span data-stu-id="44382-134">Filtering operations can include logging, automatically retrying, etc. Filters are objects that implement a method with the signature:</span></span>

```nodejs
function handle (requestOptions, next)
```

<span data-ttu-id="44382-135">När du har gjort dess förbearbetning på begäran alternativ måste metoden anropa ”next”, skicka ett återanrop med följande signatur:</span><span class="sxs-lookup"><span data-stu-id="44382-135">After doing its preprocessing on the request options, the method needs to call "next", passing a callback with the following signature:</span></span>

```nodejs
function (returnObject, finalCallback, next)
```

<span data-ttu-id="44382-136">I den här motringning och efter bearbetning returnObject (svar från begäran till servern), måste återanropet anropa sedan om den finns för att fortsätta att bearbeta filter eller anropa bara finalCallback på annat sätt för att avsluta tjänsten-anrop.</span><span class="sxs-lookup"><span data-stu-id="44382-136">In this callback, and after processing the returnObject (the response from the request to the server), the callback needs to either invoke next if it exists to continue processing other filters or simply invoke finalCallback otherwise to end the service invocation.</span></span>

<span data-ttu-id="44382-137">Två filter som implementerar logik som medföljer Azure SDK för Node.js, **ExponentialRetryPolicyFilter** och **LinearRetryPolicyFilter**.</span><span class="sxs-lookup"><span data-stu-id="44382-137">Two filters that implement retry logic are included with the Azure SDK for Node.js, **ExponentialRetryPolicyFilter** and **LinearRetryPolicyFilter**.</span></span> <span data-ttu-id="44382-138">Följande kod skapar en **TableService** objekt som använder den **ExponentialRetryPolicyFilter**:</span><span class="sxs-lookup"><span data-stu-id="44382-138">The following creates a **TableService** object that uses the **ExponentialRetryPolicyFilter**:</span></span>

```nodejs
var retryOperations = new azure.ExponentialRetryPolicyFilter();
var tableSvc = azure.createTableService().withFilter(retryOperations);
```

## <a name="add-an-entity-to-a-table"></a><span data-ttu-id="44382-139">Lägga till en entitet i en tabell</span><span class="sxs-lookup"><span data-stu-id="44382-139">Add an entity to a table</span></span>
<span data-ttu-id="44382-140">Om du vill lägga till en enhet måste du först skapa ett objekt som definierar egenskaper för enhet.</span><span class="sxs-lookup"><span data-stu-id="44382-140">To add an entity, first create an object that defines your entity properties.</span></span> <span data-ttu-id="44382-141">Alla enheter måste innehålla en **PartitionKey** och **RowKey**, som är unika identifierare för entiteten.</span><span class="sxs-lookup"><span data-stu-id="44382-141">All entities must contain a **PartitionKey** and **RowKey**, which are unique identifiers for the entity.</span></span>

* <span data-ttu-id="44382-142">**PartitionKey** -avgör den partition som entiteten lagras i</span><span class="sxs-lookup"><span data-stu-id="44382-142">**PartitionKey** - determines the partition that the entity is stored in</span></span>
* <span data-ttu-id="44382-143">**RowKey** - unikt identifierar entiteten i partitionen</span><span class="sxs-lookup"><span data-stu-id="44382-143">**RowKey** - uniquely identifies the entity within the partition</span></span>

<span data-ttu-id="44382-144">Båda **PartitionKey** och **RowKey** måste vara strängvärden.</span><span class="sxs-lookup"><span data-stu-id="44382-144">Both **PartitionKey** and **RowKey** must be string values.</span></span> <span data-ttu-id="44382-145">Mer information finns i [förstå den tabelltjänst-datamodellen](http://msdn.microsoft.com/library/azure/dd179338.aspx).</span><span class="sxs-lookup"><span data-stu-id="44382-145">For more information, see [Understanding the Table Service Data Model](http://msdn.microsoft.com/library/azure/dd179338.aspx).</span></span>

<span data-ttu-id="44382-146">Följande är ett exempel för att definiera en entitet.</span><span class="sxs-lookup"><span data-stu-id="44382-146">The following is an example of defining an entity.</span></span> <span data-ttu-id="44382-147">Observera att **dueDate** definieras som en typ av **Edm.DateTime**.</span><span class="sxs-lookup"><span data-stu-id="44382-147">Note that **dueDate** is defined as a type of **Edm.DateTime**.</span></span> <span data-ttu-id="44382-148">Anger vilken är valfri och typer kommer att härleda om inget anges.</span><span class="sxs-lookup"><span data-stu-id="44382-148">Specifying the type is optional, and types will be inferred if not specified.</span></span>

```nodejs
var task = {
  PartitionKey: {'_':'hometasks'},
  RowKey: {'_': '1'},
  description: {'_':'take out the trash'},
  dueDate: {'_':new Date(2015, 6, 20), '$':'Edm.DateTime'}
};
```

> [!NOTE]
> <span data-ttu-id="44382-149">Det finns också en **tidsstämpel** för varje post som anges av Azure när en entitet infogas eller uppdateras.</span><span class="sxs-lookup"><span data-stu-id="44382-149">There is also a **Timestamp** field for each record, which is set by Azure when an entity is inserted or updated.</span></span>
>
>

<span data-ttu-id="44382-150">Du kan också använda den **entityGenerator** att skapa entiteter.</span><span class="sxs-lookup"><span data-stu-id="44382-150">You can also use the **entityGenerator** to create entities.</span></span> <span data-ttu-id="44382-151">Följande exempel skapar samma aktivitet entitet med den **entityGenerator**.</span><span class="sxs-lookup"><span data-stu-id="44382-151">The following example creates the same task entity using the **entityGenerator**.</span></span>

```nodejs
var entGen = azure.TableUtilities.entityGenerator;
var task = {
  PartitionKey: entGen.String('hometasks'),
  RowKey: entGen.String('1'),
  description: entGen.String('take out the trash'),
  dueDate: entGen.DateTime(new Date(Date.UTC(2015, 6, 20))),
};
```

<span data-ttu-id="44382-152">Om du vill lägga till en entitet i tabellen, skicka enhetsobjekt till den **insertEntity** metod.</span><span class="sxs-lookup"><span data-stu-id="44382-152">To add an entity to your table, pass the entity object to the **insertEntity** method.</span></span>

```nodejs
tableSvc.insertEntity('mytable',task, function (error, result, response) {
  if(!error){
    // Entity inserted
  }
});
```

<span data-ttu-id="44382-153">Om åtgärden lyckas `result` innehåller den [ETag](http://en.wikipedia.org/wiki/HTTP_ETag) infogade postens och `response` innehåller information om åtgärden.</span><span class="sxs-lookup"><span data-stu-id="44382-153">If the operation is successful, `result` will contain the [ETag](http://en.wikipedia.org/wiki/HTTP_ETag) of the inserted record and `response` will contain information about the operation.</span></span>

<span data-ttu-id="44382-154">Exempelsvar:</span><span class="sxs-lookup"><span data-stu-id="44382-154">Example response:</span></span>

```nodejs
{ '.metadata': { etag: 'W/"datetime\'2015-02-25T01%3A22%3A22.5Z\'"' } }
```

> [!NOTE]
> <span data-ttu-id="44382-155">Som standard **insertEntity** returnerar inte infogade entiteten som en del av den `response` information.</span><span class="sxs-lookup"><span data-stu-id="44382-155">By default, **insertEntity** does not return the inserted entity as part of the `response` information.</span></span> <span data-ttu-id="44382-156">Om du planerar att andra åtgärder pågår på den här entiteten eller vill cachelagra informationen, kan det vara praktiskt att ha den returneras som en del av den `result`.</span><span class="sxs-lookup"><span data-stu-id="44382-156">If you plan on performing other operations on this entity, or wish to cache the information, it can be useful to have it returned as part of the `result`.</span></span> <span data-ttu-id="44382-157">Du kan göra detta genom att aktivera **echoContent** på följande sätt:</span><span class="sxs-lookup"><span data-stu-id="44382-157">You can do this by enabling **echoContent** as follows:</span></span>
>
> `tableSvc.insertEntity('mytable', task, {echoContent: true}, function (error, result, response) {...}`
>
>

## <a name="update-an-entity"></a><span data-ttu-id="44382-158">Uppdatera en entitet</span><span class="sxs-lookup"><span data-stu-id="44382-158">Update an entity</span></span>
<span data-ttu-id="44382-159">Det finns flera metoder för att uppdatera en befintlig entitet:</span><span class="sxs-lookup"><span data-stu-id="44382-159">There are multiple methods available to update an existing entity:</span></span>

* <span data-ttu-id="44382-160">**replaceEntity** -uppdaterar en befintlig entitet genom att ersätta den</span><span class="sxs-lookup"><span data-stu-id="44382-160">**replaceEntity** - updates an existing entity by replacing it</span></span>
* <span data-ttu-id="44382-161">**mergeEntity** -uppdaterar en befintlig entitet genom att använda nya egenskapsvärden i befintliga entiteten</span><span class="sxs-lookup"><span data-stu-id="44382-161">**mergeEntity** - updates an existing entity by merging new property values into the existing entity</span></span>
* <span data-ttu-id="44382-162">**insertOrReplaceEntity** -uppdaterar en befintlig entitet genom att ersätta den.</span><span class="sxs-lookup"><span data-stu-id="44382-162">**insertOrReplaceEntity** - updates an existing entity by replacing it.</span></span> <span data-ttu-id="44382-163">Om inga entiteten finns kommer en ny att infogas</span><span class="sxs-lookup"><span data-stu-id="44382-163">If no entity exists, a new one will be inserted</span></span>
* <span data-ttu-id="44382-164">**insertOrMergeEntity** -uppdaterar en befintlig entitet genom att använda den nya egenskapsvärden i den befintliga.</span><span class="sxs-lookup"><span data-stu-id="44382-164">**insertOrMergeEntity** - updates an existing entity by merging new property values into the existing.</span></span> <span data-ttu-id="44382-165">Om inga entiteten finns kommer en ny att infogas</span><span class="sxs-lookup"><span data-stu-id="44382-165">If no entity exists, a new one will be inserted</span></span>

<span data-ttu-id="44382-166">Exemplet nedan visar att uppdatera en entitet med **replaceEntity**:</span><span class="sxs-lookup"><span data-stu-id="44382-166">The following example demonstrates updating an entity using **replaceEntity**:</span></span>

```nodejs
tableSvc.replaceEntity('mytable', updatedTask, function(error, result, response){
  if(!error) {
    // Entity updated
  }
});
```

> [!NOTE]
> <span data-ttu-id="44382-167">Som standard kontrollerar uppdaterar en entitet inte om du vill se om data uppdateras tidigare har ändrats av en annan process.</span><span class="sxs-lookup"><span data-stu-id="44382-167">By default, updating an entity does not check to see if the data being updated has previously been modified by another process.</span></span> <span data-ttu-id="44382-168">Till stöd för samtidiga uppdateringar:</span><span class="sxs-lookup"><span data-stu-id="44382-168">To support concurrent updates:</span></span>
>
> 1. <span data-ttu-id="44382-169">Hämta ETag i objektet som uppdateras.</span><span class="sxs-lookup"><span data-stu-id="44382-169">Get the ETag of the object being updated.</span></span> <span data-ttu-id="44382-170">Det här felet returneras som en del av den `response` för varje entitet-relaterade åtgärden och kan hämtas via `response['.metadata'].etag`.</span><span class="sxs-lookup"><span data-stu-id="44382-170">This is returned as part of the `response` for any entity-related operation and can be retrieved through `response['.metadata'].etag`.</span></span>
> 2. <span data-ttu-id="44382-171">När du utför en update-åtgärd på en enhet, kan du lägga till ETag-information som tidigare har hämtats till den nya entiteten.</span><span class="sxs-lookup"><span data-stu-id="44382-171">When performing an update operation on an entity, add the ETag information previously retrieved to the new entity.</span></span> <span data-ttu-id="44382-172">Exempel:</span><span class="sxs-lookup"><span data-stu-id="44382-172">For example:</span></span>
>
>       <span data-ttu-id="44382-173">entity2 [.metadata] .etag = currentEtag;</span><span class="sxs-lookup"><span data-stu-id="44382-173">entity2['.metadata'].etag = currentEtag;</span></span>
> 3. <span data-ttu-id="44382-174">Utföra uppdateringen.</span><span class="sxs-lookup"><span data-stu-id="44382-174">Perform the update operation.</span></span> <span data-ttu-id="44382-175">Om entiteten har ändrats sedan du hämtade ETag-värde, till exempel en annan instans av programmet, en `error` ska returneras om villkoret uppdatering som anges i begäran att inte uppfylldes.</span><span class="sxs-lookup"><span data-stu-id="44382-175">If the entity has been modified since you retrieved the ETag value, such as another instance of your application, an `error` will be returned stating that the update condition specified in the request was not satisfied.</span></span>
>
>

<span data-ttu-id="44382-176">Med **replaceEntity** och **mergeEntity**om entiteten som ska uppdateras inte finns, update-åtgärden kommer att misslyckas.</span><span class="sxs-lookup"><span data-stu-id="44382-176">With **replaceEntity** and **mergeEntity**, if the entity that is being updated doesn't exist, then the update operation will fail.</span></span> <span data-ttu-id="44382-177">Därför om du vill lagra en entitet oavsett om den redan finns, Använd **insertOrReplaceEntity** eller **insertOrMergeEntity**.</span><span class="sxs-lookup"><span data-stu-id="44382-177">Therefore if you wish to store an entity regardless of whether it already exists, use **insertOrReplaceEntity** or **insertOrMergeEntity**.</span></span>

<span data-ttu-id="44382-178">Den `result` för lyckade uppdatering operations innehåller den **Etag** för entiteten uppdaterade.</span><span class="sxs-lookup"><span data-stu-id="44382-178">The `result` for successful update operations will contain the **Etag** of the updated entity.</span></span>

## <a name="work-with-groups-of-entities"></a><span data-ttu-id="44382-179">Arbeta med grupper av entiteter</span><span class="sxs-lookup"><span data-stu-id="44382-179">Work with groups of entities</span></span>
<span data-ttu-id="44382-180">Ibland är det praktiskt att skicka flera åtgärder tillsammans i en grupp så atomiska bearbetning av servern.</span><span class="sxs-lookup"><span data-stu-id="44382-180">Sometimes it makes sense to submit multiple operations together in a batch to ensure atomic processing by the server.</span></span> <span data-ttu-id="44382-181">Om du vill göra det använder den **TableBatch** klassen om du vill skapa en grupp och sedan använda den **executeBatch** metod för **TableService** utföra gruppbaserad åtgärder.</span><span class="sxs-lookup"><span data-stu-id="44382-181">To accomplish that, use the **TableBatch** class to create a batch, and then use the **executeBatch** method of **TableService** to perform the batched operations.</span></span>

 <span data-ttu-id="44382-182">Exemplet nedan visar skickar två entiteter i en batch:</span><span class="sxs-lookup"><span data-stu-id="44382-182">The following example demonstrates submitting two entities in a batch:</span></span>

```nodejs
var task1 = {
  PartitionKey: {'_':'hometasks'},
  RowKey: {'_': '1'},
  description: {'_':'Take out the trash'},
  dueDate: {'_':new Date(2015, 6, 20)}
};
var task2 = {
  PartitionKey: {'_':'hometasks'},
  RowKey: {'_': '2'},
  description: {'_':'Wash the dishes'},
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

<span data-ttu-id="44382-183">För lyckad batchåtgärder `result` innehåller information för varje åtgärd i batchen.</span><span class="sxs-lookup"><span data-stu-id="44382-183">For successful batch operations, `result` will contain information for each operation in the batch.</span></span>

### <a name="work-with-batched-operations"></a><span data-ttu-id="44382-184">Arbeta med batch-åtgärder</span><span class="sxs-lookup"><span data-stu-id="44382-184">Work with batched operations</span></span>
<span data-ttu-id="44382-185">Åtgärder som har lagts till i en grupp kan kontrolleras genom att visa den `operations` egenskapen.</span><span class="sxs-lookup"><span data-stu-id="44382-185">Operations added to a batch can be inspected by viewing the `operations` property.</span></span> <span data-ttu-id="44382-186">Du kan också använda följande metoder för att arbeta med åtgärder:</span><span class="sxs-lookup"><span data-stu-id="44382-186">You can also use the following methods to work with operations:</span></span>

* <span data-ttu-id="44382-187">**Rensa** -rensar alla åtgärder från en grupp</span><span class="sxs-lookup"><span data-stu-id="44382-187">**clear** - clears all operations from a batch</span></span>
* <span data-ttu-id="44382-188">**getOperations** -hämtar en åtgärd från gruppen</span><span class="sxs-lookup"><span data-stu-id="44382-188">**getOperations** - gets an operation from the batch</span></span>
* <span data-ttu-id="44382-189">**hasOperations** -returnerar true om batchen innehåller åtgärder</span><span class="sxs-lookup"><span data-stu-id="44382-189">**hasOperations** - returns true if the batch contains operations</span></span>
* <span data-ttu-id="44382-190">**removeOperations** -tar bort en åtgärd</span><span class="sxs-lookup"><span data-stu-id="44382-190">**removeOperations** - removes an operation</span></span>
* <span data-ttu-id="44382-191">**storlek** -returnerar antalet åtgärder i gruppen</span><span class="sxs-lookup"><span data-stu-id="44382-191">**size** - returns the number of operations in the batch</span></span>

## <a name="retrieve-an-entity-by-key"></a><span data-ttu-id="44382-192">Hämta en entitet med nyckeln</span><span class="sxs-lookup"><span data-stu-id="44382-192">Retrieve an entity by key</span></span>
<span data-ttu-id="44382-193">Returnera en specifik enhet baserat på de **PartitionKey** och **RowKey**, använda den **retrieveEntity** metod.</span><span class="sxs-lookup"><span data-stu-id="44382-193">To return a specific entity based on the **PartitionKey** and **RowKey**, use the **retrieveEntity** method.</span></span>

```nodejs
tableSvc.retrieveEntity('mytable', 'hometasks', '1', function(error, result, response){
  if(!error){
    // result contains the entity
  }
});
```

<span data-ttu-id="44382-194">När den här åtgärden är klar, `result` innehåller entiteten.</span><span class="sxs-lookup"><span data-stu-id="44382-194">Once this operation is complete, `result` will contain the entity.</span></span>

## <a name="query-a-set-of-entities"></a><span data-ttu-id="44382-195">Fråga en uppsättning enheter</span><span class="sxs-lookup"><span data-stu-id="44382-195">Query a set of entities</span></span>
<span data-ttu-id="44382-196">Om du vill fråga en tabell, använder den **TableQuery** objekt att bygga upp ett frågeuttryck med hjälp av följande:</span><span class="sxs-lookup"><span data-stu-id="44382-196">To query a table, use the **TableQuery** object to build up a query expression using the following clauses:</span></span>

* <span data-ttu-id="44382-197">**Välj** -fält som ska returneras från frågan</span><span class="sxs-lookup"><span data-stu-id="44382-197">**select** - the fields to be returned from the query</span></span>
* <span data-ttu-id="44382-198">**där** -where satsen</span><span class="sxs-lookup"><span data-stu-id="44382-198">**where** - the where clause</span></span>

  * <span data-ttu-id="44382-199">**och** – en `and` where-villkor</span><span class="sxs-lookup"><span data-stu-id="44382-199">**and** - an `and` where condition</span></span>
  * <span data-ttu-id="44382-200">**eller** – en `or` where-villkor</span><span class="sxs-lookup"><span data-stu-id="44382-200">**or** - an `or` where condition</span></span>
* <span data-ttu-id="44382-201">**TOP** -antal objekt som ska hämtas</span><span class="sxs-lookup"><span data-stu-id="44382-201">**top** - the number of items to fetch</span></span>

<span data-ttu-id="44382-202">I följande exempel skapas en fråga som returnerar de översta fem posterna med en PartitionKey av 'hometasks'.</span><span class="sxs-lookup"><span data-stu-id="44382-202">The following example builds a query that will return the top five items with a PartitionKey of 'hometasks'.</span></span>

```nodejs
var query = new azure.TableQuery()
  .top(5)
  .where('PartitionKey eq ?', 'hometasks');
```

<span data-ttu-id="44382-203">Eftersom **Välj** inte används, alla fält som ska returneras.</span><span class="sxs-lookup"><span data-stu-id="44382-203">Since **select** is not used, all fields will be returned.</span></span> <span data-ttu-id="44382-204">Använd för att utföra frågan mot en tabell **queryEntities**.</span><span class="sxs-lookup"><span data-stu-id="44382-204">To perform the query against a table, use **queryEntities**.</span></span> <span data-ttu-id="44382-205">I följande exempel används den här frågan för att returnera enheter från ”mytable” som prefix.</span><span class="sxs-lookup"><span data-stu-id="44382-205">The following example uses this query to return entities from 'mytable'.</span></span>

```nodejs
tableSvc.queryEntities('mytable',query, null, function(error, result, response) {
  if(!error) {
    // query was successful
  }
});
```

<span data-ttu-id="44382-206">Om detta lyckas `result.entries` innehåller en matris med entiteter som matchar frågan.</span><span class="sxs-lookup"><span data-stu-id="44382-206">If successful, `result.entries` will contain an array of entities that match the query.</span></span> <span data-ttu-id="44382-207">Om frågan inte kunde returnera alla entiteter `result.continuationToken` kommer att vara icke -*null* och kan användas som den tredje parametern för **queryEntities** att hämta fler resultat.</span><span class="sxs-lookup"><span data-stu-id="44382-207">If the query was unable to return all entities, `result.continuationToken` will be non-*null* and can be used as the third parameter of **queryEntities** to retrieve more results.</span></span> <span data-ttu-id="44382-208">Den inledande frågan använder *null* för den tredje parametern.</span><span class="sxs-lookup"><span data-stu-id="44382-208">For the initial query, use *null* for the third parameter.</span></span>

### <a name="query-a-subset-of-entity-properties"></a><span data-ttu-id="44382-209">Fråga en deluppsättning entitetsegenskaper</span><span class="sxs-lookup"><span data-stu-id="44382-209">Query a subset of entity properties</span></span>
<span data-ttu-id="44382-210">En fråga till en tabell kan hämta bara några fält från en entitet.</span><span class="sxs-lookup"><span data-stu-id="44382-210">A query to a table can retrieve just a few fields from an entity.</span></span>
<span data-ttu-id="44382-211">Detta minskar bandbredden och kan förbättra frågeprestanda, särskilt för stora entiteter.</span><span class="sxs-lookup"><span data-stu-id="44382-211">This reduces bandwidth and can improve query performance, especially for large entities.</span></span> <span data-ttu-id="44382-212">Använd den **Välj** satsen och ange namnen på fälten som ska returneras.</span><span class="sxs-lookup"><span data-stu-id="44382-212">Use the **select** clause and pass the names of the fields to be returned.</span></span> <span data-ttu-id="44382-213">Till exempel följande fråga returnerar bara de **beskrivning** och **dueDate** fält.</span><span class="sxs-lookup"><span data-stu-id="44382-213">For example, the following query will return only the **description** and **dueDate** fields.</span></span>

```nodejs
var query = new azure.TableQuery()
  .select(['description', 'dueDate'])
  .top(5)
  .where('PartitionKey eq ?', 'hometasks');
```

## <a name="delete-an-entity"></a><span data-ttu-id="44382-214">Ta bort en entitet</span><span class="sxs-lookup"><span data-stu-id="44382-214">Delete an entity</span></span>
<span data-ttu-id="44382-215">Du kan ta bort en entitet med dess partition och raden nycklar.</span><span class="sxs-lookup"><span data-stu-id="44382-215">You can delete an entity using its partition and row keys.</span></span> <span data-ttu-id="44382-216">I det här exemplet i **task1** objektet innehåller de **RowKey** och **PartitionKey** värdena för entiteten som ska tas bort.</span><span class="sxs-lookup"><span data-stu-id="44382-216">In this example, the **task1** object contains the **RowKey** and **PartitionKey** values of the entity to be deleted.</span></span> <span data-ttu-id="44382-217">Sedan objektet skickas till den **deleteEntity** metod.</span><span class="sxs-lookup"><span data-stu-id="44382-217">Then the object is passed to the **deleteEntity** method.</span></span>

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
> <span data-ttu-id="44382-218">Överväg att använda ETags när du tar bort objekt för att se till att objektet inte har ändrats av en annan process.</span><span class="sxs-lookup"><span data-stu-id="44382-218">Consider using ETags when deleting items, to ensure that the item hasn't been modified by another process.</span></span> <span data-ttu-id="44382-219">Se [uppdatera en entitet](#update-an-entity) information om hur du använder ETags.</span><span class="sxs-lookup"><span data-stu-id="44382-219">See [Update an entity](#update-an-entity) for information on using ETags.</span></span>
>
>

## <a name="delete-a-table"></a><span data-ttu-id="44382-220">Ta bort en tabell</span><span class="sxs-lookup"><span data-stu-id="44382-220">Delete a table</span></span>
<span data-ttu-id="44382-221">Följande kod tar bort en tabell från ett lagringskonto.</span><span class="sxs-lookup"><span data-stu-id="44382-221">The following code deletes a table from a storage account.</span></span>

```nodejs
tableSvc.deleteTable('mytable', function(error, response){
    if(!error){
        // Table deleted
    }
});
```

<span data-ttu-id="44382-222">Om du är osäker på om tabellen finns, Använd **deleteTableIfExists**.</span><span class="sxs-lookup"><span data-stu-id="44382-222">If you are uncertain whether the table exists, use **deleteTableIfExists**.</span></span>

## <a name="use-continuation-tokens"></a><span data-ttu-id="44382-223">Använd fortsättning token</span><span class="sxs-lookup"><span data-stu-id="44382-223">Use continuation tokens</span></span>
<span data-ttu-id="44382-224">När du frågar tabeller för stora mängder resultat, leta efter fortsättning token.</span><span class="sxs-lookup"><span data-stu-id="44382-224">When you are querying tables for large amounts of results, look for continuation tokens.</span></span> <span data-ttu-id="44382-225">Det kan finnas stora mängder data tillgängliga för din fråga som du inte kanske vet om du inte skapa känna igen när en fortsättningstoken finns.</span><span class="sxs-lookup"><span data-stu-id="44382-225">There may be large amounts of data available for your query that you might not realize if you do not build to recognize when a continuation token is present.</span></span>

<span data-ttu-id="44382-226">Resultaten objekt returnerades vid fråga entiteter anger en `continuationToken` egenskapen när dessa token finns.</span><span class="sxs-lookup"><span data-stu-id="44382-226">The results object returned during querying entities sets a `continuationToken` property when such a token is present.</span></span> <span data-ttu-id="44382-227">Du kan sedan använda den när du utför en fråga för att fortsätta att flytta mellan entiteterna partition och tabellen.</span><span class="sxs-lookup"><span data-stu-id="44382-227">You can then use this when performing a query to continue to move across the partition and table entities.</span></span>

<span data-ttu-id="44382-228">När du frågar, kan en continuationToken parameter anges mellan objektinstansen frågan och Återanropsfunktionen:</span><span class="sxs-lookup"><span data-stu-id="44382-228">When querying, a continuationToken parameter may be provided between the query object instance and the callback function:</span></span>

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

<span data-ttu-id="44382-229">Om du vill granska den `continuationToken` objekt, hittar du egenskaper som `nextPartitionKey`, `nextRowKey` och `targetLocation`, som kan användas för att söka igenom alla resultat.</span><span class="sxs-lookup"><span data-stu-id="44382-229">If you inspect the `continuationToken` object, you will find properties such as `nextPartitionKey`, `nextRowKey` and `targetLocation`, which can be used to iterate through all the results.</span></span>

<span data-ttu-id="44382-230">Det finns också en fortsättning exemplet i Azure Storage Node.js lagringsplatsen på GitHub.</span><span class="sxs-lookup"><span data-stu-id="44382-230">There is also a continuation sample within the Azure Storage Node.js repo on GitHub.</span></span> <span data-ttu-id="44382-231">Leta efter `examples/samples/continuationsample.js`.</span><span class="sxs-lookup"><span data-stu-id="44382-231">Look for `examples/samples/continuationsample.js`.</span></span>

## <a name="work-with-shared-access-signatures"></a><span data-ttu-id="44382-232">Arbeta med signaturer för delad åtkomst</span><span class="sxs-lookup"><span data-stu-id="44382-232">Work with shared access signatures</span></span>
<span data-ttu-id="44382-233">Signaturer för delad åtkomst (SAS) är ett säkert sätt att tillhandahålla detaljerade åtkomst till tabeller utan att erbjuda dina lagringskontonamn eller nycklar.</span><span class="sxs-lookup"><span data-stu-id="44382-233">Shared access signatures (SAS) are a secure way to provide granular access to tables without providing your storage account name or keys.</span></span> <span data-ttu-id="44382-234">SAS används ofta för att ge begränsad åtkomst till dina data, till exempel att tillåta en mobil app om du vill söka efter poster.</span><span class="sxs-lookup"><span data-stu-id="44382-234">SAS are often used to provide limited access to your data, such as allowing a mobile app to query records.</span></span>

<span data-ttu-id="44382-235">En betrodda program, till exempel en molnbaserad tjänst genererar en SAS med hjälp av den **generateSharedAccessSignature** av den **TableService**, och som ger den till ett program som inte är betrodd eller delvis betrodd en mobil app.</span><span class="sxs-lookup"><span data-stu-id="44382-235">A trusted application such as a cloud-based service generates a SAS using the **generateSharedAccessSignature** of the **TableService**, and provides it to an untrusted or semi-trusted application such as a mobile app.</span></span> <span data-ttu-id="44382-236">SAS genereras med hjälp av en princip som beskriver de då SAS är giltig start- och slutdatum samt den åtkomstnivå som beviljats till SAS-innehavare.</span><span class="sxs-lookup"><span data-stu-id="44382-236">The SAS is generated using a policy, which describes the start and end dates during which the SAS is valid, as well as the access level granted to the SAS holder.</span></span>

<span data-ttu-id="44382-237">I följande exempel skapar en ny princip för delad åtkomst som låter innehavaren SAS att fråga (”r”) i tabell och upphör att gälla 100 minuter efter den tidpunkt som den har skapats.</span><span class="sxs-lookup"><span data-stu-id="44382-237">The following example generates a new shared access policy that will allow the SAS holder to query ('r') the table, and expires 100 minutes after the time it is created.</span></span>

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

<span data-ttu-id="44382-238">Observera att information om värden måste anges också när den behövs när SAS-innehavaren försöker komma åt tabellen.</span><span class="sxs-lookup"><span data-stu-id="44382-238">Note that the host information must be provided also, as it is required when the SAS holder attempts to access the table.</span></span>

<span data-ttu-id="44382-239">Klientprogrammet sedan använder SAS med **TableServiceWithSAS** att utföra åtgärder mot tabellen.</span><span class="sxs-lookup"><span data-stu-id="44382-239">The client application then uses the SAS with **TableServiceWithSAS** to perform operations against the table.</span></span> <span data-ttu-id="44382-240">I följande exempel ansluter till tabellen och utför en fråga.</span><span class="sxs-lookup"><span data-stu-id="44382-240">The following example connects to the table and performs a query.</span></span>

```nodejs
var sharedTableService = azure.createTableServiceWithSas(host, tableSAS);
var query = azure.TableQuery()
  .where('PartitionKey eq ?', 'hometasks');

sharedTableService.queryEntities(query, null, function(error, result, response) {
  if(!error) {
    // result contains the entities
  }
});
```

<span data-ttu-id="44382-241">Eftersom SAS genererades med endast fråga åtkomst, om ett försök gjordes att infoga, uppdatera eller ta bort entiteter, returneras ett fel.</span><span class="sxs-lookup"><span data-stu-id="44382-241">Since the SAS was generated with only query access, if an attempt were made to insert, update, or delete entities, an error would be returned.</span></span>

### <a name="access-control-lists"></a><span data-ttu-id="44382-242">Åtkomstkontrollistor</span><span class="sxs-lookup"><span data-stu-id="44382-242">Access Control Lists</span></span>
<span data-ttu-id="44382-243">Du kan också använda en åtkomstkontrollista (ACL) för att ange åtkomstprincipen för en SAS.</span><span class="sxs-lookup"><span data-stu-id="44382-243">You can also use an Access Control List (ACL) to set the access policy for a SAS.</span></span> <span data-ttu-id="44382-244">Detta är användbart om du vill att flera klienter kan komma åt tabellen, men ger olika åtkomstprinciper för varje klient.</span><span class="sxs-lookup"><span data-stu-id="44382-244">This is useful if you wish to allow multiple clients to access the table, but provide different access policies for each client.</span></span>

<span data-ttu-id="44382-245">En ACL implementeras med hjälp av en matris med principer för åtkomst med ett ID som är associerade med varje princip.</span><span class="sxs-lookup"><span data-stu-id="44382-245">An ACL is implemented using an array of access policies, with an ID associated with each policy.</span></span> <span data-ttu-id="44382-246">I följande exempel definierar två principer, en för 'Användare1' och en för 'användare2':</span><span class="sxs-lookup"><span data-stu-id="44382-246">The following example defines two policies, one for 'user1' and one for 'user2':</span></span>

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

<span data-ttu-id="44382-247">I följande exempel hämtar den aktuella ACL för den **hometasks** tabell och lägger sedan till de nya principer med hjälp av **setTableAcl**.</span><span class="sxs-lookup"><span data-stu-id="44382-247">The following example gets the current ACL for the **hometasks** table, and then adds the new policies using **setTableAcl**.</span></span> <span data-ttu-id="44382-248">Den här metoden kan:</span><span class="sxs-lookup"><span data-stu-id="44382-248">This approach allows:</span></span>

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

<span data-ttu-id="44382-249">Du kan sedan skapa en SAS baserat på ID för en princip när du har angett ACL.</span><span class="sxs-lookup"><span data-stu-id="44382-249">Once the ACL has been set, you can then create a SAS based on the ID for a policy.</span></span> <span data-ttu-id="44382-250">I följande exempel skapas en ny SAS för 'användare2':</span><span class="sxs-lookup"><span data-stu-id="44382-250">The following example creates a new SAS for 'user2':</span></span>

```nodejs
tableSAS = tableSvc.generateSharedAccessSignature('hometasks', { Id: 'user2' });
```

## <a name="next-steps"></a><span data-ttu-id="44382-251">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="44382-251">Next steps</span></span>
<span data-ttu-id="44382-252">Mer information finns i följande resurser.</span><span class="sxs-lookup"><span data-stu-id="44382-252">For more information, see the following resources.</span></span>

* <span data-ttu-id="44382-253">[Microsoft Azure Storage Explorer](../vs-azure-tools-storage-manage-with-storage-explorer.md) är en kostnadsfri, fristående app från Microsoft som gör det möjligt att arbeta visuellt med Azure Storage-data i Windows, macOS och Linux.</span><span class="sxs-lookup"><span data-stu-id="44382-253">[Microsoft Azure Storage Explorer](../vs-azure-tools-storage-manage-with-storage-explorer.md) is a free, standalone app from Microsoft that enables you to work visually with Azure Storage data on Windows, macOS, and Linux.</span></span>
* <span data-ttu-id="44382-254">[Azure Storage SDK: N för noden](https://github.com/Azure/azure-storage-node) databasen på GitHub.</span><span class="sxs-lookup"><span data-stu-id="44382-254">[Azure Storage SDK for Node](https://github.com/Azure/azure-storage-node) repository on GitHub.</span></span>
* [<span data-ttu-id="44382-255">Node.js Developer Center</span><span class="sxs-lookup"><span data-stu-id="44382-255">Node.js Developer Center</span></span>](/develop/nodejs/)
* [<span data-ttu-id="44382-256">Skapa och distribuera en Node.js-program till en Azure-webbplats</span><span class="sxs-lookup"><span data-stu-id="44382-256">Create and deploy a Node.js application to an Azure website</span></span>](../app-service-web/app-service-web-get-started-nodejs.md)