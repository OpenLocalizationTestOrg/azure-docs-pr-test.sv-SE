---
title: "aaaHow toouse Blob storage från Node.js | Microsoft Docs"
description: Lagra Ostrukturerade data i hello moln med Azure Blob storage (objektlagring).
services: storage
documentationcenter: nodejs
author: mmacy
manager: timlt
editor: tysonn
ms.assetid: 8b0df222-1ca8-4967-8248-6d6d720947b8
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: nodejs
ms.topic: article
ms.date: 12/08/2016
ms.author: marsma
ms.openlocfilehash: e405eecdc60cd1eaa77510e7b29b41269372b65e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="how-toouse-blob-storage-from-nodejs"></a><span data-ttu-id="42ddd-103">Hur toouse Blob storage från Node.js</span><span class="sxs-lookup"><span data-stu-id="42ddd-103">How toouse Blob storage from Node.js</span></span>
[!INCLUDE [storage-selector-blob-include](../../includes/storage-selector-blob-include.md)]

[!INCLUDE [storage-check-out-samples-all](../../includes/storage-check-out-samples-all.md)]

## <a name="overview"></a><span data-ttu-id="42ddd-104">Översikt</span><span class="sxs-lookup"><span data-stu-id="42ddd-104">Overview</span></span>
<span data-ttu-id="42ddd-105">Den här artikeln beskrivs hur du tooperform vanliga scenarier med Blob storage.</span><span class="sxs-lookup"><span data-stu-id="42ddd-105">This article shows you how tooperform common scenarios using Blob storage.</span></span> <span data-ttu-id="42ddd-106">hello exempel skrivs via hello Node.js API.</span><span class="sxs-lookup"><span data-stu-id="42ddd-106">hello samples are written via hello Node.js API.</span></span> <span data-ttu-id="42ddd-107">hello-scenarier som tas upp är hur tooupload, lista, hämta och ta bort blobbar.</span><span class="sxs-lookup"><span data-stu-id="42ddd-107">hello scenarios covered include how tooupload, list, download, and delete blobs.</span></span>

[!INCLUDE [storage-blob-concepts-include](../../includes/storage-blob-concepts-include.md)]

[!INCLUDE [storage-create-account-include](../../includes/storage-create-account-include.md)]

## <a name="create-a-nodejs-application"></a><span data-ttu-id="42ddd-108">Skapa ett Node.js-program</span><span class="sxs-lookup"><span data-stu-id="42ddd-108">Create a Node.js application</span></span>
<span data-ttu-id="42ddd-109">Anvisningar för hur toocreate ett Node.js-program finns [skapa en Node.js-webbapp i Azure App Service], [skapa och distribuera en Node.js-programmet tooan Azure Cloud Service] --med Windows PowerShell , eller [skapa och distribuera en Node.js web app tooAzure med hjälp av Web Matrix].</span><span class="sxs-lookup"><span data-stu-id="42ddd-109">For instructions on how toocreate a Node.js application, see [Create a Node.js web app in Azure App Service], [Build and deploy a Node.js application tooan Azure Cloud Service] -- using Windows PowerShell, or [Build and deploy a Node.js web app tooAzure using Web Matrix].</span></span>

## <a name="configure-your-application-tooaccess-storage"></a><span data-ttu-id="42ddd-110">Konfigurera programmet tooaccess lagringen</span><span class="sxs-lookup"><span data-stu-id="42ddd-110">Configure your application tooaccess storage</span></span>
<span data-ttu-id="42ddd-111">toouse Azure storage, behöver du hello Azure Storage SDK för Node.js som innehåller en uppsättning bekvämlighet bibliotek som kommunicerar med hello storage REST-tjänster.</span><span class="sxs-lookup"><span data-stu-id="42ddd-111">toouse Azure storage, you need hello Azure Storage SDK for Node.js, which includes a set of convenience libraries that communicate with hello storage REST services.</span></span>

### <a name="use-node-package-manager-npm-tooobtain-hello-package"></a><span data-ttu-id="42ddd-112">Använd noden Package Manager (NPM) tooobtain hello-paket</span><span class="sxs-lookup"><span data-stu-id="42ddd-112">Use Node Package Manager (NPM) tooobtain hello package</span></span>
1. <span data-ttu-id="42ddd-113">Använd ett kommandoradsgränssnitt som **PowerShell** (Windows), **Terminal** (Mac), eller **Bash** (Unix) toonavigate toohello mappen där du skapade ditt exempel programmet.</span><span class="sxs-lookup"><span data-stu-id="42ddd-113">Use a command-line interface such as **PowerShell** (Windows), **Terminal** (Mac), or **Bash** (Unix), toonavigate toohello folder where you created your sample application.</span></span>
2. <span data-ttu-id="42ddd-114">Typen **npm installera azure-lagring** i hello kommandofönstret.</span><span class="sxs-lookup"><span data-stu-id="42ddd-114">Type **npm install azure-storage** in hello command window.</span></span> <span data-ttu-id="42ddd-115">Utdata från kommandot hello är liknande toohello följande kodexempel.</span><span class="sxs-lookup"><span data-stu-id="42ddd-115">Output from hello command is similar toohello following code example.</span></span>

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
3. <span data-ttu-id="42ddd-116">Du kan köra hello manuellt **ls** kommandot tooverify som en **nod\_moduler** mappen har skapats.</span><span class="sxs-lookup"><span data-stu-id="42ddd-116">You can manually run hello **ls** command tooverify that a **node\_modules** folder was created.</span></span> <span data-ttu-id="42ddd-117">Hitta hello inuti den mappen **azure storage** paket som innehåller hello bibliotek som du behöver tooaccess lagring.</span><span class="sxs-lookup"><span data-stu-id="42ddd-117">Inside that folder, find hello **azure-storage** package, which contains hello libraries that you need tooaccess storage.</span></span>

### <a name="import-hello-package"></a><span data-ttu-id="42ddd-118">Importera hello-paket</span><span class="sxs-lookup"><span data-stu-id="42ddd-118">Import hello package</span></span>
<span data-ttu-id="42ddd-119">Använda anteckningar eller något annat textredigeringsprogram Lägg till följande toohello överkant hello hello **server.js** -filen för programmet hello där du ska toouse lagring:</span><span class="sxs-lookup"><span data-stu-id="42ddd-119">Using Notepad or another text editor, add hello following toohello top of hello **server.js** file of hello application where you intend toouse storage:</span></span>

```nodejs
var azure = require('azure-storage');
```

## <a name="set-up-an-azure-storage-connection"></a><span data-ttu-id="42ddd-120">Skapa en Azure Storage-anslutning</span><span class="sxs-lookup"><span data-stu-id="42ddd-120">Set up an Azure Storage connection</span></span>
<span data-ttu-id="42ddd-121">hello Azure-modulen läses hello miljövariabler `AZURE_STORAGE_ACCOUNT` och `AZURE_STORAGE_ACCESS_KEY`, eller `AZURE_STORAGE_CONNECTION_STRING`, information krävs tooconnect tooyour Azure storage-konto.</span><span class="sxs-lookup"><span data-stu-id="42ddd-121">hello Azure module will read hello environment variables `AZURE_STORAGE_ACCOUNT` and `AZURE_STORAGE_ACCESS_KEY`, or `AZURE_STORAGE_CONNECTION_STRING`, for information required tooconnect tooyour Azure storage account.</span></span> <span data-ttu-id="42ddd-122">Om de här miljövariablerna inte har angetts måste du ange hello kontoinformation när du anropar **createBlobService**.</span><span class="sxs-lookup"><span data-stu-id="42ddd-122">If these environment variables are not set, you must specify hello account information when calling **createBlobService**.</span></span>

<span data-ttu-id="42ddd-123">Ett exempel på inställningen hello miljövariabler i hello [Azure-portalen](https://portal.azure.com) för en Azure webbapp, se [Node.js web app med hello Azure Tabelltjänsten].</span><span class="sxs-lookup"><span data-stu-id="42ddd-123">For an example of setting hello environment variables in hello [Azure portal](https://portal.azure.com) for an Azure web app, see [Node.js web app using hello Azure Table Service].</span></span>

## <a name="create-a-container"></a><span data-ttu-id="42ddd-124">Skapa en behållare</span><span class="sxs-lookup"><span data-stu-id="42ddd-124">Create a container</span></span>
<span data-ttu-id="42ddd-125">Hej **BlobService** objekt kan du arbeta med behållare och blobbar.</span><span class="sxs-lookup"><span data-stu-id="42ddd-125">hello **BlobService** object lets you work with containers and blobs.</span></span> <span data-ttu-id="42ddd-126">hello följande kod skapar en **BlobService** objekt.</span><span class="sxs-lookup"><span data-stu-id="42ddd-126">hello following code creates a **BlobService** object.</span></span> <span data-ttu-id="42ddd-127">Lägg till följande hello hello övre delen av **server.js**:</span><span class="sxs-lookup"><span data-stu-id="42ddd-127">Add hello following near hello top of **server.js**:</span></span>

```nodejs
var blobSvc = azure.createBlobService();
```

> [!NOTE]
> <span data-ttu-id="42ddd-128">Du kan komma åt en blob anonymt med **createBlobServiceAnonymous** och ge hello värdadress.</span><span class="sxs-lookup"><span data-stu-id="42ddd-128">You can access a blob anonymously by using **createBlobServiceAnonymous** and providing hello host address.</span></span> <span data-ttu-id="42ddd-129">Till exempel använda `var blobSvc = azure.createBlobServiceAnonymous('https://myblob.blob.core.windows.net/');`.</span><span class="sxs-lookup"><span data-stu-id="42ddd-129">For example, use `var blobSvc = azure.createBlobServiceAnonymous('https://myblob.blob.core.windows.net/');`.</span></span>
>
>

[!INCLUDE [storage-container-naming-rules-include](../../includes/storage-container-naming-rules-include.md)]

<span data-ttu-id="42ddd-130">toocreate en ny behållare använder **createContainerIfNotExists**.</span><span class="sxs-lookup"><span data-stu-id="42ddd-130">toocreate a new container, use **createContainerIfNotExists**.</span></span> <span data-ttu-id="42ddd-131">hello skapar följande exempel en ny behållare med namnet 'minbehållare':</span><span class="sxs-lookup"><span data-stu-id="42ddd-131">hello following code example creates a new container named 'mycontainer':</span></span>

```nodejs
blobSvc.createContainerIfNotExists('mycontainer', function(error, result, response){
    if(!error){
      // Container exists and is private
    }
});
```

<span data-ttu-id="42ddd-132">Om hello behållaren nyligen har skapat `result.created` är true.</span><span class="sxs-lookup"><span data-stu-id="42ddd-132">If hello container is newly created, `result.created` is true.</span></span> <span data-ttu-id="42ddd-133">Om det redan finns hello behållaren `result.created` är false.</span><span class="sxs-lookup"><span data-stu-id="42ddd-133">If hello container already exists, `result.created` is false.</span></span> <span data-ttu-id="42ddd-134">`response`innehåller information om hello operationen, inklusive hello ETag-information för hello behållare.</span><span class="sxs-lookup"><span data-stu-id="42ddd-134">`response` contains information about hello operation, including hello ETag information for hello container.</span></span>

### <a name="container-security"></a><span data-ttu-id="42ddd-135">Behållaren säkerhet</span><span class="sxs-lookup"><span data-stu-id="42ddd-135">Container security</span></span>
<span data-ttu-id="42ddd-136">Som standard nya behållare är privata och går inte att ansluta anonymt.</span><span class="sxs-lookup"><span data-stu-id="42ddd-136">By default, new containers are private and cannot be accessed anonymously.</span></span> <span data-ttu-id="42ddd-137">toomake hello behållare offentliga så att du kan komma åt den anonymt, du kan ange åtkomstnivån hello behållare för**blob** eller **behållare**.</span><span class="sxs-lookup"><span data-stu-id="42ddd-137">toomake hello container public so that you can access it anonymously, you can set hello container's access level too**blob** or **container**.</span></span>

* <span data-ttu-id="42ddd-138">**BLOB** -tillåter anonym läsbehörighet tooblob innehållet och metadata i den här behållaren, men inte toocontainer metadata, till exempel visar en lista över alla blobbar i en behållare</span><span class="sxs-lookup"><span data-stu-id="42ddd-138">**blob** - allows anonymous read access tooblob content and metadata within this container, but not toocontainer metadata such as listing all blobs within a container</span></span>
* <span data-ttu-id="42ddd-139">**behållaren** -tillåter anonym läsbehörighet tooblob innehållet och metadata som metadata för behållaren</span><span class="sxs-lookup"><span data-stu-id="42ddd-139">**container** - allows anonymous read access tooblob content and metadata as well as container metadata</span></span>

<span data-ttu-id="42ddd-140">hello följande kodexempel visar inställningen hello åtkomstnivå för**blob**:</span><span class="sxs-lookup"><span data-stu-id="42ddd-140">hello following code example demonstrates setting hello access level too**blob**:</span></span>

```nodejs
blobSvc.createContainerIfNotExists('mycontainer', {publicAccessLevel : 'blob'}, function(error, result, response){
    if(!error){
      // Container exists and allows
      // anonymous read access tooblob
      // content and metadata within this container
    }
});
```

<span data-ttu-id="42ddd-141">Du kan också ändra hello åtkomstnivån för en behållare med hjälp av **setContainerAcl** toospecify hello åtkomstnivå.</span><span class="sxs-lookup"><span data-stu-id="42ddd-141">Alternatively, you can modify hello access level of a container by using **setContainerAcl** toospecify hello access level.</span></span> <span data-ttu-id="42ddd-142">hello följande kod exempel ändringar hello åtkomst nivå toocontainer:</span><span class="sxs-lookup"><span data-stu-id="42ddd-142">hello following code example changes hello access level toocontainer:</span></span>

```nodejs
blobSvc.setContainerAcl('mycontainer', null /* signedIdentifiers */, {publicAccessLevel : 'container'} /* publicAccessLevel*/, function(error, result, response){
  if(!error){
    // Container access level set too'container'
  }
});
```

<span data-ttu-id="42ddd-143">hello innehåller information om hello operationen, inklusive hello aktuella **ETag** för hello behållare.</span><span class="sxs-lookup"><span data-stu-id="42ddd-143">hello result contains information about hello operation, including hello current **ETag** for hello container.</span></span>

### <a name="filters"></a><span data-ttu-id="42ddd-144">Filter</span><span class="sxs-lookup"><span data-stu-id="42ddd-144">Filters</span></span>
<span data-ttu-id="42ddd-145">Du kan använda valfri filtrering operations toooperations utföras med hjälp av **BlobService**.</span><span class="sxs-lookup"><span data-stu-id="42ddd-145">You can apply optional filtering operations toooperations performed using **BlobService**.</span></span> <span data-ttu-id="42ddd-146">Filtrering operations kan innehålla loggning, försöker automatiskt igen och så vidare. Filtren är objekt som implementerar en metod med hello signatur:</span><span class="sxs-lookup"><span data-stu-id="42ddd-146">Filtering operations can include logging, automatically retrying, etc. Filters are objects that implement a method with hello signature:</span></span>

```nodejs
function handle (requestOptions, next)
```

<span data-ttu-id="42ddd-147">När du har gjort dess förbearbetning på hello begäran alternativ måste hello-metoden toocall ”nästa” Skicka en motringning med hello följande signatur:</span><span class="sxs-lookup"><span data-stu-id="42ddd-147">After doing its preprocessing on hello request options, hello method needs toocall "next", passing a callback with hello following signature:</span></span>

```nodejs
function (returnObject, finalCallback, next)
```

<span data-ttu-id="42ddd-148">I den här motringning och efter bearbetning hello returnObject (hello svar från hello begäran toohello server), hello-återanrop måste tooeither anropa sedan om den finns toocontinue bearbetning filter eller helt enkelt anropa finalCallback tooend hello service anrop.</span><span class="sxs-lookup"><span data-stu-id="42ddd-148">In this callback, and after processing hello returnObject (hello response from hello request toohello server), hello callback needs tooeither invoke next if it exists toocontinue processing other filters or simply invoke finalCallback tooend hello service invocation.</span></span>

<span data-ttu-id="42ddd-149">Två filter som implementerar logik som medföljer hello Azure SDK för Node.js, **ExponentialRetryPolicyFilter** och **LinearRetryPolicyFilter**.</span><span class="sxs-lookup"><span data-stu-id="42ddd-149">Two filters that implement retry logic are included with hello Azure SDK for Node.js, **ExponentialRetryPolicyFilter** and **LinearRetryPolicyFilter**.</span></span> <span data-ttu-id="42ddd-150">hello följande skapar en **BlobService** objekt som använder hello **ExponentialRetryPolicyFilter**:</span><span class="sxs-lookup"><span data-stu-id="42ddd-150">hello following creates a **BlobService** object that uses hello **ExponentialRetryPolicyFilter**:</span></span>

```nodejs
var retryOperations = new azure.ExponentialRetryPolicyFilter();
var blobSvc = azure.createBlobService().withFilter(retryOperations);
```

## <a name="upload-a-blob-into-a-container"></a><span data-ttu-id="42ddd-151">Ladda upp en blobb till en behållare</span><span class="sxs-lookup"><span data-stu-id="42ddd-151">Upload a blob into a container</span></span>
<span data-ttu-id="42ddd-152">Det finns tre typer av blobbar: blockblobbar, sidblobbar och tilläggsblobbar.</span><span class="sxs-lookup"><span data-stu-id="42ddd-152">There are three types of blobs: block blobs, page blobs and append blobs.</span></span> <span data-ttu-id="42ddd-153">Blockblobbar kan du toomore effektivt överför stora mängder data.</span><span class="sxs-lookup"><span data-stu-id="42ddd-153">Block blobs allow you toomore efficiently upload large data.</span></span> <span data-ttu-id="42ddd-154">Lägg till blobbar som är optimerade för tilläggsåtgärder.</span><span class="sxs-lookup"><span data-stu-id="42ddd-154">Append blobs are optimized for append operations.</span></span> <span data-ttu-id="42ddd-155">Sidblobbar är optimerade för läs-/ skrivåtgärder.</span><span class="sxs-lookup"><span data-stu-id="42ddd-155">Page blobs are optimized for read/write operations.</span></span> <span data-ttu-id="42ddd-156">Mer information finns i [förstå Blockblobbar, Tilläggsblobbar och Sidblobbar](http://msdn.microsoft.com/library/azure/ee691964.aspx).</span><span class="sxs-lookup"><span data-stu-id="42ddd-156">For more information, see [Understanding Block Blobs, Append Blobs, and Page Blobs](http://msdn.microsoft.com/library/azure/ee691964.aspx).</span></span>

### <a name="block-blobs"></a><span data-ttu-id="42ddd-157">Blockblob-objekt</span><span class="sxs-lookup"><span data-stu-id="42ddd-157">Block blobs</span></span>
<span data-ttu-id="42ddd-158">tooupload data tooa blockblob, Använd hello följande:</span><span class="sxs-lookup"><span data-stu-id="42ddd-158">tooupload data tooa block blob, use hello following:</span></span>

* <span data-ttu-id="42ddd-159">**createBlockBlobFromLocalFile** – skapar en ny block-blob och överför hello innehållet i en fil</span><span class="sxs-lookup"><span data-stu-id="42ddd-159">**createBlockBlobFromLocalFile** - creates a new block blob and uploads hello contents of a file</span></span>
* <span data-ttu-id="42ddd-160">**createBlockBlobFromStream** – skapar en ny block-blob och överför hello innehållet i en dataström</span><span class="sxs-lookup"><span data-stu-id="42ddd-160">**createBlockBlobFromStream** - creates a new block blob and uploads hello contents of a stream</span></span>
* <span data-ttu-id="42ddd-161">**createBlockBlobFromText** – skapar en ny block-blob och överför hello innehållet i en sträng</span><span class="sxs-lookup"><span data-stu-id="42ddd-161">**createBlockBlobFromText** - creates a new block blob and uploads hello contents of a string</span></span>
* <span data-ttu-id="42ddd-162">**createWriteStreamToBlockBlob** -ger en skrivning dataströmmen tooa block-blob</span><span class="sxs-lookup"><span data-stu-id="42ddd-162">**createWriteStreamToBlockBlob** - provides a write stream tooa block blob</span></span>

<span data-ttu-id="42ddd-163">hello följande kodexempel överför hello innehållet i hello **test.txt** filen till **minblobb**.</span><span class="sxs-lookup"><span data-stu-id="42ddd-163">hello following code example uploads hello contents of hello **test.txt** file into **myblob**.</span></span>

```nodejs
blobSvc.createBlockBlobFromLocalFile('mycontainer', 'myblob', 'test.txt', function(error, result, response){
  if(!error){
    // file uploaded
  }
});
```

<span data-ttu-id="42ddd-164">Hej `result` returneras av dessa metoder innehåller information om hello-åtgärden, till exempel hello **ETag** av hello-blob.</span><span class="sxs-lookup"><span data-stu-id="42ddd-164">hello `result` returned by these methods contains information on hello operation, such as hello **ETag** of hello blob.</span></span>

### <a name="append-blobs"></a><span data-ttu-id="42ddd-165">Tilläggsblobbar</span><span class="sxs-lookup"><span data-stu-id="42ddd-165">Append blobs</span></span>
<span data-ttu-id="42ddd-166">tooupload data tooa nya bifoga blob, använder hello följande:</span><span class="sxs-lookup"><span data-stu-id="42ddd-166">tooupload data tooa new append blob, use hello following:</span></span>

* <span data-ttu-id="42ddd-167">**createAppendBlobFromLocalFile** – skapar en ny tilläggsblobb och överför hello innehållet i en fil</span><span class="sxs-lookup"><span data-stu-id="42ddd-167">**createAppendBlobFromLocalFile** - creates a new append blob and uploads hello contents of a file</span></span>
* <span data-ttu-id="42ddd-168">**createAppendBlobFromStream** – skapar en ny tilläggsblobb och överför hello innehållet i en dataström</span><span class="sxs-lookup"><span data-stu-id="42ddd-168">**createAppendBlobFromStream** - creates a new append blob and uploads hello contents of a stream</span></span>
* <span data-ttu-id="42ddd-169">**createAppendBlobFromText** – skapar en ny tilläggsblobb och överför hello innehållet i en sträng</span><span class="sxs-lookup"><span data-stu-id="42ddd-169">**createAppendBlobFromText** - creates a new append blob and uploads hello contents of a string</span></span>
* <span data-ttu-id="42ddd-170">**createWriteStreamToNewAppendBlob** – skapar en ny tilläggsblobb och ger en dataström toowrite tooit</span><span class="sxs-lookup"><span data-stu-id="42ddd-170">**createWriteStreamToNewAppendBlob** - creates a new append blob and then provides a stream toowrite tooit</span></span>

<span data-ttu-id="42ddd-171">hello följande kodexempel överför hello innehållet i hello **test.txt** filen till **myappendblob**.</span><span class="sxs-lookup"><span data-stu-id="42ddd-171">hello following code example uploads hello contents of hello **test.txt** file into **myappendblob**.</span></span>

```nodejs
blobSvc.createAppendBlobFromLocalFile('mycontainer', 'myappendblob', 'test.txt', function(error, result, response){
  if(!error){
    // file uploaded
  }
});
```

<span data-ttu-id="42ddd-172">tooappend ett block tooan befintliga Lägg blob, Använd hello följande:</span><span class="sxs-lookup"><span data-stu-id="42ddd-172">tooappend a block tooan existing append blob, use hello following:</span></span>

* <span data-ttu-id="42ddd-173">**appendFromLocalFile** -Lägg till hello innehållet i en fil tooan befintliga bifoga blob</span><span class="sxs-lookup"><span data-stu-id="42ddd-173">**appendFromLocalFile** - append hello contents of a file tooan existing append blob</span></span>
* <span data-ttu-id="42ddd-174">**appendFromStream** -Lägg till hello innehållet för en dataström tooan befintliga bifoga blob</span><span class="sxs-lookup"><span data-stu-id="42ddd-174">**appendFromStream** - append hello contents of a stream tooan existing append blob</span></span>
* <span data-ttu-id="42ddd-175">**appendFromText** -Lägg till hello innehållet i en sträng tooan befintliga bifoga blob</span><span class="sxs-lookup"><span data-stu-id="42ddd-175">**appendFromText** - append hello contents of a string tooan existing append blob</span></span>
* <span data-ttu-id="42ddd-176">**appendBlockFromStream** -Lägg till hello innehållet för en dataström tooan befintliga bifoga blob</span><span class="sxs-lookup"><span data-stu-id="42ddd-176">**appendBlockFromStream** - append hello contents of a stream tooan existing append blob</span></span>
* <span data-ttu-id="42ddd-177">**appendBlockFromText** -Lägg till hello innehållet i en sträng tooan befintliga bifoga blob</span><span class="sxs-lookup"><span data-stu-id="42ddd-177">**appendBlockFromText** - append hello contents of a string tooan existing append blob</span></span>

> [!NOTE]
> <span data-ttu-id="42ddd-178">appendFromXXX API: er kommer att göra vissa klientsidan validering toofail snabb tooavoid onödiga server-anrop.</span><span class="sxs-lookup"><span data-stu-id="42ddd-178">appendFromXXX APIs will do some client-side validation toofail fast tooavoid unnecessary server calls.</span></span> <span data-ttu-id="42ddd-179">appendBlockFromXXX inte.</span><span class="sxs-lookup"><span data-stu-id="42ddd-179">appendBlockFromXXX won't.</span></span>
>
>

<span data-ttu-id="42ddd-180">hello följande kodexempel överför hello innehållet i hello **test.txt** filen till **myappendblob**.</span><span class="sxs-lookup"><span data-stu-id="42ddd-180">hello following code example uploads hello contents of hello **test.txt** file into **myappendblob**.</span></span>

```nodejs
blobSvc.appendFromText('mycontainer', 'myappendblob', 'text toobe appended', function(error, result, response){
  if(!error){
    // text appended
  }
});
```

### <a name="page-blobs"></a><span data-ttu-id="42ddd-181">Sidblobbar</span><span class="sxs-lookup"><span data-stu-id="42ddd-181">Page blobs</span></span>
<span data-ttu-id="42ddd-182">tooupload data tooa sidblob, Använd hello följande:</span><span class="sxs-lookup"><span data-stu-id="42ddd-182">tooupload data tooa page blob, use hello following:</span></span>

* <span data-ttu-id="42ddd-183">**createPageBlob** -skapar en ny sidblob för en viss längd</span><span class="sxs-lookup"><span data-stu-id="42ddd-183">**createPageBlob** - creates a new page blob of a specific length</span></span>
* <span data-ttu-id="42ddd-184">**createPageBlobFromLocalFile** – skapar en ny sidblob och överför hello innehållet i en fil</span><span class="sxs-lookup"><span data-stu-id="42ddd-184">**createPageBlobFromLocalFile** - creates a new page blob and uploads hello contents of a file</span></span>
* <span data-ttu-id="42ddd-185">**createPageBlobFromStream** – skapar en ny sidblob och överför hello innehållet i en dataström</span><span class="sxs-lookup"><span data-stu-id="42ddd-185">**createPageBlobFromStream** - creates a new page blob and uploads hello contents of a stream</span></span>
* <span data-ttu-id="42ddd-186">**createWriteStreamToExistingPageBlob** -ger en skrivning dataströmmen tooan befintliga sidblob</span><span class="sxs-lookup"><span data-stu-id="42ddd-186">**createWriteStreamToExistingPageBlob** - provides a write stream tooan existing page blob</span></span>
* <span data-ttu-id="42ddd-187">**createWriteStreamToNewPageBlob** – skapar en ny sidblob och ger en dataström toowrite tooit</span><span class="sxs-lookup"><span data-stu-id="42ddd-187">**createWriteStreamToNewPageBlob** - creates a new page blob and then provides a stream toowrite tooit</span></span>

<span data-ttu-id="42ddd-188">hello följande kodexempel överför hello innehållet i hello **test.txt** filen till **mypageblob**.</span><span class="sxs-lookup"><span data-stu-id="42ddd-188">hello following code example uploads hello contents of hello **test.txt** file into **mypageblob**.</span></span>

```nodejs
blobSvc.createPageBlobFromLocalFile('mycontainer', 'mypageblob', 'test.txt', function(error, result, response){
  if(!error){
    // file uploaded
  }
});
```

> [!NOTE]
> <span data-ttu-id="42ddd-189">Sidblobbar består av 512 byte sidor.</span><span class="sxs-lookup"><span data-stu-id="42ddd-189">Page blobs consist of 512-byte 'pages'.</span></span> <span data-ttu-id="42ddd-190">Du får ett fel när överföra data med en storlek som inte är en multipel av 512.</span><span class="sxs-lookup"><span data-stu-id="42ddd-190">You will receive an error when uploading data with a size that is not a multiple of 512.</span></span>
>
>

## <a name="list-hello-blobs-in-a-container"></a><span data-ttu-id="42ddd-191">Lista hello blobbar i en behållare</span><span class="sxs-lookup"><span data-stu-id="42ddd-191">List hello blobs in a container</span></span>
<span data-ttu-id="42ddd-192">toolist hello blobbar i en behållare använder hello **listBlobsSegmented** metod.</span><span class="sxs-lookup"><span data-stu-id="42ddd-192">toolist hello blobs in a container, use hello **listBlobsSegmented** method.</span></span> <span data-ttu-id="42ddd-193">Om du vill tooreturn blobbar med ett specifikt prefix använda **listBlobsSegmentedWithPrefix**.</span><span class="sxs-lookup"><span data-stu-id="42ddd-193">If you'd like tooreturn blobs with a specific prefix, use **listBlobsSegmentedWithPrefix**.</span></span>

```nodejs
blobSvc.listBlobsSegmented('mycontainer', null, function(error, result, response){
  if(!error){
      // result.entries contains hello entries
      // If not all blobs were returned, result.continuationToken has hello continuation token.
  }
});
```

<span data-ttu-id="42ddd-194">Hej `result` innehåller en `entries` samling, som är en matris med objekt som beskriver varje blob.</span><span class="sxs-lookup"><span data-stu-id="42ddd-194">hello `result` contains an `entries` collection, which is an array of objects that describe each blob.</span></span> <span data-ttu-id="42ddd-195">Om alla BLOB inte kan returneras, hello `result` innehåller också en `continuationToken`, som du kan använda som hello andra parameter tooretrieve ytterligare poster.</span><span class="sxs-lookup"><span data-stu-id="42ddd-195">If all blobs cannot be returned, hello `result` also provides a `continuationToken`, which you may use as hello second parameter tooretrieve additional entries.</span></span>

## <a name="download-blobs"></a><span data-ttu-id="42ddd-196">Ladda ned blobbar</span><span class="sxs-lookup"><span data-stu-id="42ddd-196">Download blobs</span></span>
<span data-ttu-id="42ddd-197">toodownload data från blob använder hello följande:</span><span class="sxs-lookup"><span data-stu-id="42ddd-197">toodownload data from a blob, use hello following:</span></span>

* <span data-ttu-id="42ddd-198">**getBlobToLocalFile** -skriver hello blob innehållet toofile</span><span class="sxs-lookup"><span data-stu-id="42ddd-198">**getBlobToLocalFile** - writes hello blob contents toofile</span></span>
* <span data-ttu-id="42ddd-199">**getBlobToStream** -skriver hello blob innehållet tooa dataström</span><span class="sxs-lookup"><span data-stu-id="42ddd-199">**getBlobToStream** - writes hello blob contents tooa stream</span></span>
* <span data-ttu-id="42ddd-200">**getBlobToText** -skriver hello blobbinnehållet tooa strängen</span><span class="sxs-lookup"><span data-stu-id="42ddd-200">**getBlobToText** - writes hello blob contents tooa string</span></span>
* <span data-ttu-id="42ddd-201">**createReadStream** -ger en dataström tooread från hello blob</span><span class="sxs-lookup"><span data-stu-id="42ddd-201">**createReadStream** - provides a stream tooread from hello blob</span></span>

<span data-ttu-id="42ddd-202">hello följande kodexempel visas hur du använder **getBlobToStream** toodownload hello innehållet i hello **minblobb** blob och lagra den toohello **output.txt** filen med hjälp av en dataströmmen:</span><span class="sxs-lookup"><span data-stu-id="42ddd-202">hello following code example demonstrates using **getBlobToStream** toodownload hello contents of hello **myblob** blob and store it toohello **output.txt** file by using a stream:</span></span>

```nodejs
var fs = require('fs');
blobSvc.getBlobToStream('mycontainer', 'myblob', fs.createWriteStream('output.txt'), function(error, result, response){
  if(!error){
    // blob retrieved
  }
});
```

<span data-ttu-id="42ddd-203">Hej `result` innehåller information om hello blob, inklusive **ETag** information.</span><span class="sxs-lookup"><span data-stu-id="42ddd-203">hello `result` contains information about hello blob, including **ETag** information.</span></span>

## <a name="delete-a-blob"></a><span data-ttu-id="42ddd-204">Ta bort en blob</span><span class="sxs-lookup"><span data-stu-id="42ddd-204">Delete a blob</span></span>
<span data-ttu-id="42ddd-205">Slutligen toodelete blob anropa **deleteBlob**.</span><span class="sxs-lookup"><span data-stu-id="42ddd-205">Finally, toodelete a blob, call **deleteBlob**.</span></span> <span data-ttu-id="42ddd-206">Hej följande kodexempel borttagningar hello blob med namnet **minblobb**.</span><span class="sxs-lookup"><span data-stu-id="42ddd-206">hello following code example deletes hello blob named **myblob**.</span></span>

```nodejs
blobSvc.deleteBlob(containerName, 'myblob', function(error, response){
  if(!error){
    // Blob has been deleted
  }
});
```

## <a name="concurrent-access"></a><span data-ttu-id="42ddd-207">Samtidig åtkomst</span><span class="sxs-lookup"><span data-stu-id="42ddd-207">Concurrent access</span></span>
<span data-ttu-id="42ddd-208">toosupport samtidig åtkomst tooa blob från flera klienter eller flera processinstanser, som du kan använda **ETags** eller **lån**.</span><span class="sxs-lookup"><span data-stu-id="42ddd-208">toosupport concurrent access tooa blob from multiple clients or multiple process instances, you can use **ETags** or **leases**.</span></span>

* <span data-ttu-id="42ddd-209">**ETag** -ger ett sätt toodetect som hello blob eller behållare har ändrats av en annan process</span><span class="sxs-lookup"><span data-stu-id="42ddd-209">**Etag** - provides a way toodetect that hello blob or container has been modified by another process</span></span>
* <span data-ttu-id="42ddd-210">**Lånet** – ger en sätt tooobtain exklusiv, förnyas, skrivning eller ta bort åtkomst tooa blob för en viss tidsperiod</span><span class="sxs-lookup"><span data-stu-id="42ddd-210">**Lease** - provides a way tooobtain exclusive, renewable, write or delete access tooa blob for a period of time</span></span>

### <a name="etag"></a><span data-ttu-id="42ddd-211">ETag</span><span class="sxs-lookup"><span data-stu-id="42ddd-211">ETag</span></span>
<span data-ttu-id="42ddd-212">Använd ETags om du behöver tooallow flera klienter eller instanser toowrite toohello block Blob eller sidan Blob samtidigt.</span><span class="sxs-lookup"><span data-stu-id="42ddd-212">Use ETags if you need tooallow multiple clients or instances toowrite toohello block Blob or page Blob simultaneously.</span></span> <span data-ttu-id="42ddd-213">hello kan ETag du toodetermine om hello behållare eller blobb har ändrats sedan du först läsa eller skapades, vilket gör att du tooavoid skriver över ändringar som har genomförts av en annan klient eller process.</span><span class="sxs-lookup"><span data-stu-id="42ddd-213">hello ETag allows you toodetermine if hello container or blob was modified since you initially read or created it, which allows you tooavoid overwriting changes committed by another client or process.</span></span>

<span data-ttu-id="42ddd-214">Du kan ange ETag villkor med hjälp av valfri hello `options.accessConditions` parameter.</span><span class="sxs-lookup"><span data-stu-id="42ddd-214">You can set ETag conditions by using hello optional `options.accessConditions` parameter.</span></span> <span data-ttu-id="42ddd-215">hello följande kodexempel endast överför hello **test.txt** -filen om hello blob som redan finns och har hello ETag-värde som innehåller `etagToMatch`.</span><span class="sxs-lookup"><span data-stu-id="42ddd-215">hello following code example only uploads hello **test.txt** file if hello blob already exists and has hello ETag value contained by `etagToMatch`.</span></span>

```nodejs
blobSvc.createBlockBlobFromLocalFile('mycontainer', 'myblob', 'test.txt', { accessConditions: { EtagMatch: etagToMatch} }, function(error, result, response){
    if(!error){
    // file uploaded
  }
});
```

<span data-ttu-id="42ddd-216">När du använder ETags är hello allmänna mönster:</span><span class="sxs-lookup"><span data-stu-id="42ddd-216">When you're using ETags, hello general pattern is:</span></span>

1. <span data-ttu-id="42ddd-217">Hämta hello ETag hello följd av att skapa, listor eller hämta-åtgärden.</span><span class="sxs-lookup"><span data-stu-id="42ddd-217">Obtain hello ETag as hello result of a create, list, or get operation.</span></span>
2. <span data-ttu-id="42ddd-218">Utföra en åtgärd som kontrollerar den hello ETag-värde inte har ändrats.</span><span class="sxs-lookup"><span data-stu-id="42ddd-218">Perform an action, checking that hello ETag value has not been modified.</span></span>

<span data-ttu-id="42ddd-219">Detta anger att en annan klient eller instans ändrats hello blob eller behållaren eftersom du har köpt hello ETag-värde om hello-värdet har ändrats.</span><span class="sxs-lookup"><span data-stu-id="42ddd-219">If hello value was modified, this indicates that another client or instance modified hello blob or container since you obtained hello ETag value.</span></span>

### <a name="lease"></a><span data-ttu-id="42ddd-220">Lån</span><span class="sxs-lookup"><span data-stu-id="42ddd-220">Lease</span></span>
<span data-ttu-id="42ddd-221">Du kan skaffa ett nytt lån med hjälp av hello **acquireLease** -metoden att ange hello blob eller behållare som du vill tooobtain ett lån på.</span><span class="sxs-lookup"><span data-stu-id="42ddd-221">You can acquire a new lease by using hello **acquireLease** method, specifying hello blob or container that you wish tooobtain a lease on.</span></span> <span data-ttu-id="42ddd-222">Till exempel följande kod hello får ett lån på **minblobb**.</span><span class="sxs-lookup"><span data-stu-id="42ddd-222">For example, hello following code acquires a lease on **myblob**.</span></span>

```nodejs
blobSvc.acquireLease('mycontainer', 'myblob', function(error, result, response){
  if(!error) {
    console.log('leaseId: ' + result.id);
  }
});
```

<span data-ttu-id="42ddd-223">Efterföljande åtgärder på **minblobb** måste ange hello `options.leaseId` parameter.</span><span class="sxs-lookup"><span data-stu-id="42ddd-223">Subsequent operations on **myblob** must provide hello `options.leaseId` parameter.</span></span> <span data-ttu-id="42ddd-224">hello lån ID returneras som `result.id` från **acquireLease**.</span><span class="sxs-lookup"><span data-stu-id="42ddd-224">hello lease ID is returned as `result.id` from **acquireLease**.</span></span>

> [!NOTE]
> <span data-ttu-id="42ddd-225">Som standard är hello lånetid oändlig.</span><span class="sxs-lookup"><span data-stu-id="42ddd-225">By default, hello lease duration is infinite.</span></span> <span data-ttu-id="42ddd-226">Du kan ange en icke oändliga varaktigheten (mellan 15 och 60 sekunder) genom att tillhandahålla hello `options.leaseDuration` parameter.</span><span class="sxs-lookup"><span data-stu-id="42ddd-226">You can specify a non-infinite duration (between 15 and 60 seconds) by providing hello `options.leaseDuration` parameter.</span></span>
>
>

<span data-ttu-id="42ddd-227">Använd tooremove ett lån **releaseLease**.</span><span class="sxs-lookup"><span data-stu-id="42ddd-227">tooremove a lease, use **releaseLease**.</span></span> <span data-ttu-id="42ddd-228">toobreak ett lån men förhindra andra från att erhålla ett nytt lån till hello ursprungliga varaktigheten har gått ut, Använd **breakLease**.</span><span class="sxs-lookup"><span data-stu-id="42ddd-228">toobreak a lease, but prevent others from obtaining a new lease until hello original duration has expired, use **breakLease**.</span></span>

## <a name="work-with-shared-access-signatures"></a><span data-ttu-id="42ddd-229">Arbeta med signaturer för delad åtkomst</span><span class="sxs-lookup"><span data-stu-id="42ddd-229">Work with shared access signatures</span></span>
<span data-ttu-id="42ddd-230">Signaturer för delad åtkomst (SAS) är ett säkert sätt tooprovide detaljerade åtkomst tooblobs och behållare utan att erbjuda dina lagringskontonamn eller nycklar.</span><span class="sxs-lookup"><span data-stu-id="42ddd-230">Shared access signatures (SAS) are a secure way tooprovide granular access tooblobs and containers without providing your storage account name or keys.</span></span> <span data-ttu-id="42ddd-231">Signaturer för delad åtkomst är ofta använda tooprovide begränsad åtkomst tooyour data, till exempel att tillåta att en mobil app tooaccess blobbar.</span><span class="sxs-lookup"><span data-stu-id="42ddd-231">Shared access signatures are often used tooprovide limited access tooyour data, such as allowing a mobile app tooaccess blobs.</span></span>

> [!NOTE]
> <span data-ttu-id="42ddd-232">Medan du kan också tillåta anonym åtkomst tooblobs, kan signaturer för delad åtkomst du tooprovide mer kontrollerad åtkomst som du måste skapa hello SAS.</span><span class="sxs-lookup"><span data-stu-id="42ddd-232">While you can also allow anonymous access tooblobs, shared access signatures allow you tooprovide more controlled access, as you must generate hello SAS.</span></span>
>
>

<span data-ttu-id="42ddd-233">En betrodda program, till exempel en molnbaserad tjänst genererar signaturer för delad åtkomst med hjälp av hello **generateSharedAccessSignature** av hello **BlobService**, och ger den tooan betrodd eller delvis betrodda program, till exempel en mobil app.</span><span class="sxs-lookup"><span data-stu-id="42ddd-233">A trusted application such as a cloud-based service generates shared access signatures using hello **generateSharedAccessSignature** of hello **BlobService**, and provides it tooan untrusted or semi-trusted application such as a mobile app.</span></span> <span data-ttu-id="42ddd-234">Delad åtkomst signaturer genereras med hjälp av en princip som beskriver hello start- och slutdatum under vilka hello signaturer för delad åtkomst är giltiga samt hello åt nivån beviljade toohello delad åtkomst signaturer innehavaren.</span><span class="sxs-lookup"><span data-stu-id="42ddd-234">Shared access signatures are generated using a policy, which describes hello start and end dates during which hello shared access signatures are valid, as well as hello access level granted toohello shared access signatures holder.</span></span>

<span data-ttu-id="42ddd-235">hello följande kodexempel genererar en ny princip för delad åtkomst som tillåter hello delad åtkomst signaturer innehavaren tooperform läsåtgärder på hello **minblobb** blob och upphör att gälla 100 minuter efter hello när det skapas.</span><span class="sxs-lookup"><span data-stu-id="42ddd-235">hello following code example generates a new shared access policy that allows hello shared access signatures holder tooperform read operations on hello **myblob** blob, and expires 100 minutes after hello time it is created.</span></span>

```nodejs
var startDate = new Date();
var expiryDate = new Date(startDate);
expiryDate.setMinutes(startDate.getMinutes() + 100);
startDate.setMinutes(startDate.getMinutes() - 100);

var sharedAccessPolicy = {
  AccessPolicy: {
    Permissions: azure.BlobUtilities.SharedAccessPermissions.READ,
    Start: startDate,
    Expiry: expiryDate
  },
};

var blobSAS = blobSvc.generateSharedAccessSignature('mycontainer', 'myblob', sharedAccessPolicy);
var host = blobSvc.host;
```

<span data-ttu-id="42ddd-236">Observera att information om hello värden måste anges också som krävs när hello delad åtkomst signaturer innehavaren försöker tooaccess hello behållare.</span><span class="sxs-lookup"><span data-stu-id="42ddd-236">Note that hello host information must be provided also, as it is required when hello shared access signatures holder attempts tooaccess hello container.</span></span>

<span data-ttu-id="42ddd-237">hello klientprogrammet sedan använder signaturer för delad åtkomst med **BlobServiceWithSAS** tooperform åtgärder mot hello-blob.</span><span class="sxs-lookup"><span data-stu-id="42ddd-237">hello client application then uses shared access signatures with **BlobServiceWithSAS** tooperform operations against hello blob.</span></span> <span data-ttu-id="42ddd-238">hello följande hämtar information **minblobb**.</span><span class="sxs-lookup"><span data-stu-id="42ddd-238">hello following gets information about **myblob**.</span></span>

```nodejs
var sharedBlobSvc = azure.createBlobServiceWithSas(host, blobSAS);
sharedBlobSvc.getBlobProperties('mycontainer', 'myblob', function (error, result, response) {
  if(!error) {
    // retrieved info
  }
});
```

<span data-ttu-id="42ddd-239">Eftersom hello delade åtkomstsignaturer genererades med skrivskyddad åtkomst om ett försök görs toomodify hello blob, returneras ett fel.</span><span class="sxs-lookup"><span data-stu-id="42ddd-239">Since hello shared access signatures were generated with read-only access, if an attempt is made toomodify hello blob, an error will be returned.</span></span>

### <a name="access-control-lists"></a><span data-ttu-id="42ddd-240">Listor för åtkomstkontroll</span><span class="sxs-lookup"><span data-stu-id="42ddd-240">Access control lists</span></span>
<span data-ttu-id="42ddd-241">Du kan också använda en åtkomstkontrollista tooset hello åtkomst principer för åtkomstkontroll för SAS.</span><span class="sxs-lookup"><span data-stu-id="42ddd-241">You can also use an access control list (ACL) tooset hello access policy for SAS.</span></span> <span data-ttu-id="42ddd-242">Detta är användbart om du vill tooallow flera klienter tooaccess en behållare men ange olika principer för varje klient.</span><span class="sxs-lookup"><span data-stu-id="42ddd-242">This is useful if you wish tooallow multiple clients tooaccess a container but provide different access policies for each client.</span></span>

<span data-ttu-id="42ddd-243">En ACL implementeras med hjälp av en matris med principer för åtkomst med ett ID som är associerade med varje princip.</span><span class="sxs-lookup"><span data-stu-id="42ddd-243">An ACL is implemented using an array of access policies, with an ID associated with each policy.</span></span> <span data-ttu-id="42ddd-244">Följande kodexempel hello definierar två principer, en för 'Användare1' och en för 'användare2':</span><span class="sxs-lookup"><span data-stu-id="42ddd-244">hello following code example defines two policies, one for 'user1' and one for 'user2':</span></span>

```nodejs
var sharedAccessPolicy = {
  user1: {
    Permissions: azure.BlobUtilities.SharedAccessPermissions.READ,
    Start: startDate,
    Expiry: expiryDate
  },
  user2: {
    Permissions: azure.BlobUtilities.SharedAccessPermissions.WRITE,
    Start: startDate,
    Expiry: expiryDate
  }
};
```

<span data-ttu-id="42ddd-245">följande kod exempel hämtar hello hello aktuella ACL för **minbehållare**, och lägger sedan till hello nya principer med hjälp av **setBlobAcl**.</span><span class="sxs-lookup"><span data-stu-id="42ddd-245">hello following code example gets hello current ACL for **mycontainer**, and then adds hello new policies using **setBlobAcl**.</span></span> <span data-ttu-id="42ddd-246">Den här metoden kan:</span><span class="sxs-lookup"><span data-stu-id="42ddd-246">This approach allows:</span></span>

```nodejs
var extend = require('extend');
blobSvc.getBlobAcl('mycontainer', function(error, result, response) {
  if(!error){
    var newSignedIdentifiers = extend(true, result.signedIdentifiers, sharedAccessPolicy);
    blobSvc.setBlobAcl('mycontainer', newSignedIdentifiers, function(error, result, response){
      if(!error){
        // ACL set
      }
    });
  }
});
```

<span data-ttu-id="42ddd-247">En gång hello ACL anges, du kan sedan skapa signaturer för delad åtkomst baserat på hello-ID för en princip.</span><span class="sxs-lookup"><span data-stu-id="42ddd-247">Once hello ACL is set, you can then create shared access signatures based on hello ID for a policy.</span></span> <span data-ttu-id="42ddd-248">Följande kodexempel hello skapar nya signaturer för delad åtkomst för 'användare2':</span><span class="sxs-lookup"><span data-stu-id="42ddd-248">hello following code example creates new shared access signatures for 'user2':</span></span>

```nodejs
blobSAS = blobSvc.generateSharedAccessSignature('mycontainer', { Id: 'user2' });
```

## <a name="next-steps"></a><span data-ttu-id="42ddd-249">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="42ddd-249">Next steps</span></span>
<span data-ttu-id="42ddd-250">Mer information finns i följande resurser hello.</span><span class="sxs-lookup"><span data-stu-id="42ddd-250">For more information, see hello following resources.</span></span>

* <span data-ttu-id="42ddd-251">[Azure Storage SDK för noden API-referens][Azure Storage SDK for Node API Reference]</span><span class="sxs-lookup"><span data-stu-id="42ddd-251">[Azure Storage SDK for Node API Reference][Azure Storage SDK for Node API Reference]</span></span>
* <span data-ttu-id="42ddd-252">[Azure Storage-teamets blogg][Azure Storage Team Blog]</span><span class="sxs-lookup"><span data-stu-id="42ddd-252">[Azure Storage Team Blog][Azure Storage Team Blog]</span></span>
* <span data-ttu-id="42ddd-253">[Azure Storage SDK: N för noden] [ Azure Storage SDK for Node] databasen på GitHub</span><span class="sxs-lookup"><span data-stu-id="42ddd-253">[Azure Storage SDK for Node][Azure Storage SDK for Node] repository on GitHub</span></span>
* [<span data-ttu-id="42ddd-254">Node.js Developer Center</span><span class="sxs-lookup"><span data-stu-id="42ddd-254">Node.js Developer Center</span></span>](https://azure.microsoft.com/develop/nodejs/)
* [<span data-ttu-id="42ddd-255">Överföra data med hello kommandoradsverktyget azcopy</span><span class="sxs-lookup"><span data-stu-id="42ddd-255">Transfer data with hello AzCopy command-line utility</span></span>](storage-use-azcopy.md)

[Azure Storage SDK for Node]: https://github.com/Azure/azure-storage-node

[skapa en Node.js-webbapp i Azure App Service]: ../app-service-web/app-service-web-get-started-nodejs.md
[Node.js web app med hello Azure Tabelltjänsten]: ../app-service-web/storage-nodejs-use-table-storage-web-site.md
[skapa och distribuera en Node.js web app tooAzure med hjälp av Web Matrix]: ../app-service-web/web-sites-nodejs-use-webmatrix.md
[Using hello REST API]: http://msdn.microsoft.com/library/azure/hh264518.aspx
[Azure portal]: https://portal.azure.com
[skapa och distribuera en Node.js-programmet tooan Azure Cloud Service]: ../cloud-services/cloud-services-nodejs-develop-deploy-app.md
[Azure Storage Team Blog]: http://blogs.msdn.com/b/windowsazurestorage/
[Azure Storage SDK for Node API Reference]: http://dl.windowsazure.com/nodestoragedocs/index.html
