---
title: "Använda Blob storage från Node.js | Microsoft Docs"
description: Lagra ostrukturerade data i molnet med Azure Blob Storage (objektlagring).
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
ms.openlocfilehash: 38c3fd3cd271c3f9d60c44fff17715062b4979ae
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/03/2017
---
# <a name="how-to-use-blob-storage-from-nodejs"></a><span data-ttu-id="91e06-103">Använda Blob Storage från Node.js</span><span class="sxs-lookup"><span data-stu-id="91e06-103">How to use Blob storage from Node.js</span></span>
[!INCLUDE [storage-selector-blob-include](../../includes/storage-selector-blob-include.md)]

[!INCLUDE [storage-check-out-samples-all](../../includes/storage-check-out-samples-all.md)]

## <a name="overview"></a><span data-ttu-id="91e06-104">Översikt</span><span class="sxs-lookup"><span data-stu-id="91e06-104">Overview</span></span>
<span data-ttu-id="91e06-105">Den här artikeln visar hur du utför vanliga scenarier med Blob storage.</span><span class="sxs-lookup"><span data-stu-id="91e06-105">This article shows you how to perform common scenarios using Blob storage.</span></span> <span data-ttu-id="91e06-106">Exemplen är skrivna via API för Node.js.</span><span class="sxs-lookup"><span data-stu-id="91e06-106">The samples are written via the Node.js API.</span></span> <span data-ttu-id="91e06-107">Scenarier som tas upp är hur du överför, lista, hämta och ta bort blobbar.</span><span class="sxs-lookup"><span data-stu-id="91e06-107">The scenarios covered include how to upload, list, download, and delete blobs.</span></span>

[!INCLUDE [storage-blob-concepts-include](../../includes/storage-blob-concepts-include.md)]

[!INCLUDE [storage-create-account-include](../../includes/storage-create-account-include.md)]

## <a name="create-a-nodejs-application"></a><span data-ttu-id="91e06-108">Skapa ett Node.js-program</span><span class="sxs-lookup"><span data-stu-id="91e06-108">Create a Node.js application</span></span>
<span data-ttu-id="91e06-109">Instruktioner om hur du skapar en Node.js-program finns i [skapa en Node.js-webbapp i Azure App Service], [skapa och distribuera ett Node.js-program till en Azure-molntjänst] --med Windows PowerShell eller [skapa och distribuera en Node.js-webbapp till Azure med hjälp av Web Matrix].</span><span class="sxs-lookup"><span data-stu-id="91e06-109">For instructions on how to create a Node.js application, see [Create a Node.js web app in Azure App Service], [Build and deploy a Node.js application to an Azure Cloud Service] -- using Windows PowerShell, or [Build and deploy a Node.js web app to Azure using Web Matrix].</span></span>

## <a name="configure-your-application-to-access-storage"></a><span data-ttu-id="91e06-110">Konfigurera ditt program åtkomst till lagring</span><span class="sxs-lookup"><span data-stu-id="91e06-110">Configure your application to access storage</span></span>
<span data-ttu-id="91e06-111">Om du vill använda Azure storage, behöver du Azure Storage SDK: N för Node.js som innehåller en uppsättning bekvämlighet bibliotek som kommunicerar med storage REST-tjänster.</span><span class="sxs-lookup"><span data-stu-id="91e06-111">To use Azure storage, you need the Azure Storage SDK for Node.js, which includes a set of convenience libraries that communicate with the storage REST services.</span></span>

### <a name="use-node-package-manager-npm-to-obtain-the-package"></a><span data-ttu-id="91e06-112">Använd noden Package Manager (NPM) för att hämta paketet</span><span class="sxs-lookup"><span data-stu-id="91e06-112">Use Node Package Manager (NPM) to obtain the package</span></span>
1. <span data-ttu-id="91e06-113">Använd ett kommandoradsgränssnitt som **PowerShell** (Windows), **Terminal** (Mac), eller **Bash** (Unix) att navigera till mappen där du skapade exempelprogrammet.</span><span class="sxs-lookup"><span data-stu-id="91e06-113">Use a command-line interface such as **PowerShell** (Windows), **Terminal** (Mac), or **Bash** (Unix), to navigate to the folder where you created your sample application.</span></span>
2. <span data-ttu-id="91e06-114">Typen **npm installera azure-lagring** i kommandofönstret.</span><span class="sxs-lookup"><span data-stu-id="91e06-114">Type **npm install azure-storage** in the command window.</span></span> <span data-ttu-id="91e06-115">Utdata från kommandot liknar följande exempel.</span><span class="sxs-lookup"><span data-stu-id="91e06-115">Output from the command is similar to the following code example.</span></span>

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
3. <span data-ttu-id="91e06-116">Du kan köra manuellt på **ls** kommando för att kontrollera att en **nod\_moduler** mappen har skapats.</span><span class="sxs-lookup"><span data-stu-id="91e06-116">You can manually run the **ls** command to verify that a **node\_modules** folder was created.</span></span> <span data-ttu-id="91e06-117">Inuti den mappen hitta den **azure storage** paket som innehåller de bibliotek som du behöver åtkomst till lagring.</span><span class="sxs-lookup"><span data-stu-id="91e06-117">Inside that folder, find the **azure-storage** package, which contains the libraries that you need to access storage.</span></span>

### <a name="import-the-package"></a><span data-ttu-id="91e06-118">Importera paketet</span><span class="sxs-lookup"><span data-stu-id="91e06-118">Import the package</span></span>
<span data-ttu-id="91e06-119">I anteckningar eller något annat textredigeringsprogram, Lägg till följande överst i den **server.js** filen för programmet som du tänker använda lagring:</span><span class="sxs-lookup"><span data-stu-id="91e06-119">Using Notepad or another text editor, add the following to the top of the **server.js** file of the application where you intend to use storage:</span></span>

```nodejs
var azure = require('azure-storage');
```

## <a name="set-up-an-azure-storage-connection"></a><span data-ttu-id="91e06-120">Skapa en Azure Storage-anslutning</span><span class="sxs-lookup"><span data-stu-id="91e06-120">Set up an Azure Storage connection</span></span>
<span data-ttu-id="91e06-121">Azure-modulen läses miljövariablerna `AZURE_STORAGE_ACCOUNT` och `AZURE_STORAGE_ACCESS_KEY`, eller `AZURE_STORAGE_CONNECTION_STRING`, information som krävs för att ansluta till Azure storage-konto.</span><span class="sxs-lookup"><span data-stu-id="91e06-121">The Azure module will read the environment variables `AZURE_STORAGE_ACCOUNT` and `AZURE_STORAGE_ACCESS_KEY`, or `AZURE_STORAGE_CONNECTION_STRING`, for information required to connect to your Azure storage account.</span></span> <span data-ttu-id="91e06-122">Om de här miljövariablerna inte har angetts måste du ange kontoinformationen vid anrop av **createBlobService**.</span><span class="sxs-lookup"><span data-stu-id="91e06-122">If these environment variables are not set, you must specify the account information when calling **createBlobService**.</span></span>

<span data-ttu-id="91e06-123">Ett exempel på Ange miljövariabler i den [Azure-portalen](https://portal.azure.com) för en Azure webbapp, se [Node.js-webbapp som använder tjänsten Azure Table].</span><span class="sxs-lookup"><span data-stu-id="91e06-123">For an example of setting the environment variables in the [Azure portal](https://portal.azure.com) for an Azure web app, see [Node.js web app using the Azure Table Service].</span></span>

## <a name="create-a-container"></a><span data-ttu-id="91e06-124">Skapa en behållare</span><span class="sxs-lookup"><span data-stu-id="91e06-124">Create a container</span></span>
<span data-ttu-id="91e06-125">Den **BlobService** objekt kan du arbeta med behållare och blobbar.</span><span class="sxs-lookup"><span data-stu-id="91e06-125">The **BlobService** object lets you work with containers and blobs.</span></span> <span data-ttu-id="91e06-126">Följande kod skapar en **BlobService** objekt.</span><span class="sxs-lookup"><span data-stu-id="91e06-126">The following code creates a **BlobService** object.</span></span> <span data-ttu-id="91e06-127">Lägg till följande längst upp i **server.js**:</span><span class="sxs-lookup"><span data-stu-id="91e06-127">Add the following near the top of **server.js**:</span></span>

```nodejs
var blobSvc = azure.createBlobService();
```

> [!NOTE]
> <span data-ttu-id="91e06-128">Du kan komma åt en blob anonymt med **createBlobServiceAnonymous** och ge värdadressen.</span><span class="sxs-lookup"><span data-stu-id="91e06-128">You can access a blob anonymously by using **createBlobServiceAnonymous** and providing the host address.</span></span> <span data-ttu-id="91e06-129">Till exempel använda `var blobSvc = azure.createBlobServiceAnonymous('https://myblob.blob.core.windows.net/');`.</span><span class="sxs-lookup"><span data-stu-id="91e06-129">For example, use `var blobSvc = azure.createBlobServiceAnonymous('https://myblob.blob.core.windows.net/');`.</span></span>
>
>

[!INCLUDE [storage-container-naming-rules-include](../../includes/storage-container-naming-rules-include.md)]

<span data-ttu-id="91e06-130">Använd för att skapa en ny behållare **createContainerIfNotExists**.</span><span class="sxs-lookup"><span data-stu-id="91e06-130">To create a new container, use **createContainerIfNotExists**.</span></span> <span data-ttu-id="91e06-131">Följande exempel skapar en ny behållare med namnet 'minbehållare':</span><span class="sxs-lookup"><span data-stu-id="91e06-131">The following code example creates a new container named 'mycontainer':</span></span>

```nodejs
blobSvc.createContainerIfNotExists('mycontainer', function(error, result, response){
    if(!error){
      // Container exists and is private
    }
});
```

<span data-ttu-id="91e06-132">Om behållaren nyligen har skapat `result.created` är true.</span><span class="sxs-lookup"><span data-stu-id="91e06-132">If the container is newly created, `result.created` is true.</span></span> <span data-ttu-id="91e06-133">Om behållaren redan `result.created` är false.</span><span class="sxs-lookup"><span data-stu-id="91e06-133">If the container already exists, `result.created` is false.</span></span> <span data-ttu-id="91e06-134">`response`innehåller information om åtgärden, inklusive ETag-information för behållaren.</span><span class="sxs-lookup"><span data-stu-id="91e06-134">`response` contains information about the operation, including the ETag information for the container.</span></span>

### <a name="container-security"></a><span data-ttu-id="91e06-135">Behållaren säkerhet</span><span class="sxs-lookup"><span data-stu-id="91e06-135">Container security</span></span>
<span data-ttu-id="91e06-136">Som standard nya behållare är privata och går inte att ansluta anonymt.</span><span class="sxs-lookup"><span data-stu-id="91e06-136">By default, new containers are private and cannot be accessed anonymously.</span></span> <span data-ttu-id="91e06-137">Om du vill att behållaren offentliga så att du kan komma åt den anonymt, du kan ange behållarens åtkomst till **blob** eller **behållare**.</span><span class="sxs-lookup"><span data-stu-id="91e06-137">To make the container public so that you can access it anonymously, you can set the container's access level to **blob** or **container**.</span></span>

* <span data-ttu-id="91e06-138">**BLOB** -tillåter anonym läsbehörighet till blob-innehåll och metadata i den här behållaren, men inte i behållaren metadata, till exempel visar en lista över alla blobbar i en behållare</span><span class="sxs-lookup"><span data-stu-id="91e06-138">**blob** - allows anonymous read access to blob content and metadata within this container, but not to container metadata such as listing all blobs within a container</span></span>
* <span data-ttu-id="91e06-139">**behållaren** -tillåter anonym läsbehörighet till blob-innehåll och metadata som metadata för behållaren</span><span class="sxs-lookup"><span data-stu-id="91e06-139">**container** - allows anonymous read access to blob content and metadata as well as container metadata</span></span>

<span data-ttu-id="91e06-140">Följande kodexempel visar ställa in till **blob**:</span><span class="sxs-lookup"><span data-stu-id="91e06-140">The following code example demonstrates setting the access level to **blob**:</span></span>

```nodejs
blobSvc.createContainerIfNotExists('mycontainer', {publicAccessLevel : 'blob'}, function(error, result, response){
    if(!error){
      // Container exists and allows
      // anonymous read access to blob
      // content and metadata within this container
    }
});
```

<span data-ttu-id="91e06-141">Du kan också ändra åtkomstnivån för en behållare med hjälp av **setContainerAcl** Ange åtkomstnivå.</span><span class="sxs-lookup"><span data-stu-id="91e06-141">Alternatively, you can modify the access level of a container by using **setContainerAcl** to specify the access level.</span></span> <span data-ttu-id="91e06-142">Följande kodexempel ändrar åtkomstnivå till behållare:</span><span class="sxs-lookup"><span data-stu-id="91e06-142">The following code example changes the access level to container:</span></span>

```nodejs
blobSvc.setContainerAcl('mycontainer', null /* signedIdentifiers */, {publicAccessLevel : 'container'} /* publicAccessLevel*/, function(error, result, response){
  if(!error){
    // Container access level set to 'container'
  }
});
```

<span data-ttu-id="91e06-143">Innehåller information om operationen, inklusive aktuellt **ETag** för behållaren.</span><span class="sxs-lookup"><span data-stu-id="91e06-143">The result contains information about the operation, including the current **ETag** for the container.</span></span>

### <a name="filters"></a><span data-ttu-id="91e06-144">Filter</span><span class="sxs-lookup"><span data-stu-id="91e06-144">Filters</span></span>
<span data-ttu-id="91e06-145">Du kan använda valfri filtrering åtgärder till åtgärder som utförs med hjälp av **BlobService**.</span><span class="sxs-lookup"><span data-stu-id="91e06-145">You can apply optional filtering operations to operations performed using **BlobService**.</span></span> <span data-ttu-id="91e06-146">Filtrering operations kan innehålla loggning, försöker automatiskt igen och så vidare. Filtren är objekt som implementerar en metod med signaturen:</span><span class="sxs-lookup"><span data-stu-id="91e06-146">Filtering operations can include logging, automatically retrying, etc. Filters are objects that implement a method with the signature:</span></span>

```nodejs
function handle (requestOptions, next)
```

<span data-ttu-id="91e06-147">När du har gjort dess förbearbetning på begäran alternativ måste metoden anropa ”next”, skicka ett återanrop med följande signatur:</span><span class="sxs-lookup"><span data-stu-id="91e06-147">After doing its preprocessing on the request options, the method needs to call "next", passing a callback with the following signature:</span></span>

```nodejs
function (returnObject, finalCallback, next)
```

<span data-ttu-id="91e06-148">I den här motringning och efter bearbetning returnObject (svar från begäran till servern), måste återanropet anropa sedan om den finns för att fortsätta att bearbeta filter eller helt enkelt anropa finalCallback för att avsluta tjänsten-anrop.</span><span class="sxs-lookup"><span data-stu-id="91e06-148">In this callback, and after processing the returnObject (the response from the request to the server), the callback needs to either invoke next if it exists to continue processing other filters or simply invoke finalCallback to end the service invocation.</span></span>

<span data-ttu-id="91e06-149">Två filter som implementerar logik som medföljer Azure SDK för Node.js, **ExponentialRetryPolicyFilter** och **LinearRetryPolicyFilter**.</span><span class="sxs-lookup"><span data-stu-id="91e06-149">Two filters that implement retry logic are included with the Azure SDK for Node.js, **ExponentialRetryPolicyFilter** and **LinearRetryPolicyFilter**.</span></span> <span data-ttu-id="91e06-150">Följande kod skapar en **BlobService** objekt som använder den **ExponentialRetryPolicyFilter**:</span><span class="sxs-lookup"><span data-stu-id="91e06-150">The following creates a **BlobService** object that uses the **ExponentialRetryPolicyFilter**:</span></span>

```nodejs
var retryOperations = new azure.ExponentialRetryPolicyFilter();
var blobSvc = azure.createBlobService().withFilter(retryOperations);
```

## <a name="upload-a-blob-into-a-container"></a><span data-ttu-id="91e06-151">Ladda upp en blobb till en behållare</span><span class="sxs-lookup"><span data-stu-id="91e06-151">Upload a blob into a container</span></span>
<span data-ttu-id="91e06-152">Det finns tre typer av blobbar: blockblobbar, sidblobbar och tilläggsblobbar.</span><span class="sxs-lookup"><span data-stu-id="91e06-152">There are three types of blobs: block blobs, page blobs and append blobs.</span></span> <span data-ttu-id="91e06-153">Blockblobbar kan du effektivt överför stora mängder data.</span><span class="sxs-lookup"><span data-stu-id="91e06-153">Block blobs allow you to more efficiently upload large data.</span></span> <span data-ttu-id="91e06-154">Lägg till blobbar som är optimerade för tilläggsåtgärder.</span><span class="sxs-lookup"><span data-stu-id="91e06-154">Append blobs are optimized for append operations.</span></span> <span data-ttu-id="91e06-155">Sidblobbar är optimerade för läs-/ skrivåtgärder.</span><span class="sxs-lookup"><span data-stu-id="91e06-155">Page blobs are optimized for read/write operations.</span></span> <span data-ttu-id="91e06-156">Mer information finns i [förstå Blockblobbar, Tilläggsblobbar och Sidblobbar](http://msdn.microsoft.com/library/azure/ee691964.aspx).</span><span class="sxs-lookup"><span data-stu-id="91e06-156">For more information, see [Understanding Block Blobs, Append Blobs, and Page Blobs](http://msdn.microsoft.com/library/azure/ee691964.aspx).</span></span>

### <a name="block-blobs"></a><span data-ttu-id="91e06-157">Blockblob-objekt</span><span class="sxs-lookup"><span data-stu-id="91e06-157">Block blobs</span></span>
<span data-ttu-id="91e06-158">Om du vill överföra data till en blockblobb, använder du följande:</span><span class="sxs-lookup"><span data-stu-id="91e06-158">To upload data to a block blob, use the following:</span></span>

* <span data-ttu-id="91e06-159">**createBlockBlobFromLocalFile** – skapar en ny block-blob och överför innehållet i en fil</span><span class="sxs-lookup"><span data-stu-id="91e06-159">**createBlockBlobFromLocalFile** - creates a new block blob and uploads the contents of a file</span></span>
* <span data-ttu-id="91e06-160">**createBlockBlobFromStream** – skapar en ny block-blob och överför innehållet i en dataström</span><span class="sxs-lookup"><span data-stu-id="91e06-160">**createBlockBlobFromStream** - creates a new block blob and uploads the contents of a stream</span></span>
* <span data-ttu-id="91e06-161">**createBlockBlobFromText** – skapar en ny block-blob och överför innehållet i en sträng</span><span class="sxs-lookup"><span data-stu-id="91e06-161">**createBlockBlobFromText** - creates a new block blob and uploads the contents of a string</span></span>
* <span data-ttu-id="91e06-162">**createWriteStreamToBlockBlob** -ger en dataström med skrivning till en blockblobb</span><span class="sxs-lookup"><span data-stu-id="91e06-162">**createWriteStreamToBlockBlob** - provides a write stream to a block blob</span></span>

<span data-ttu-id="91e06-163">Följande kodexempel Överför innehållet i den **test.txt** filen till **minblobb**.</span><span class="sxs-lookup"><span data-stu-id="91e06-163">The following code example uploads the contents of the **test.txt** file into **myblob**.</span></span>

```nodejs
blobSvc.createBlockBlobFromLocalFile('mycontainer', 'myblob', 'test.txt', function(error, result, response){
  if(!error){
    // file uploaded
  }
});
```

<span data-ttu-id="91e06-164">Den `result` returneras av dessa metoder innehåller information om åtgärden som den **ETag** på blob.</span><span class="sxs-lookup"><span data-stu-id="91e06-164">The `result` returned by these methods contains information on the operation, such as the **ETag** of the blob.</span></span>

### <a name="append-blobs"></a><span data-ttu-id="91e06-165">Tilläggsblobbar</span><span class="sxs-lookup"><span data-stu-id="91e06-165">Append blobs</span></span>
<span data-ttu-id="91e06-166">Om du vill överföra data till en ny tilläggsblobb, använder du följande:</span><span class="sxs-lookup"><span data-stu-id="91e06-166">To upload data to a new append blob, use the following:</span></span>

* <span data-ttu-id="91e06-167">**createAppendBlobFromLocalFile** – skapar en ny tilläggsblobb och överför innehållet i en fil</span><span class="sxs-lookup"><span data-stu-id="91e06-167">**createAppendBlobFromLocalFile** - creates a new append blob and uploads the contents of a file</span></span>
* <span data-ttu-id="91e06-168">**createAppendBlobFromStream** – skapar en ny tilläggsblobb och överför innehållet i en dataström</span><span class="sxs-lookup"><span data-stu-id="91e06-168">**createAppendBlobFromStream** - creates a new append blob and uploads the contents of a stream</span></span>
* <span data-ttu-id="91e06-169">**createAppendBlobFromText** – skapar en ny tilläggsblobb och överför innehållet i en sträng</span><span class="sxs-lookup"><span data-stu-id="91e06-169">**createAppendBlobFromText** - creates a new append blob and uploads the contents of a string</span></span>
* <span data-ttu-id="91e06-170">**createWriteStreamToNewAppendBlob** – skapar en ny tilläggsblobb och ger en dataström med att skriva till den</span><span class="sxs-lookup"><span data-stu-id="91e06-170">**createWriteStreamToNewAppendBlob** - creates a new append blob and then provides a stream to write to it</span></span>

<span data-ttu-id="91e06-171">Följande kodexempel Överför innehållet i den **test.txt** filen till **myappendblob**.</span><span class="sxs-lookup"><span data-stu-id="91e06-171">The following code example uploads the contents of the **test.txt** file into **myappendblob**.</span></span>

```nodejs
blobSvc.createAppendBlobFromLocalFile('mycontainer', 'myappendblob', 'test.txt', function(error, result, response){
  if(!error){
    // file uploaded
  }
});
```

<span data-ttu-id="91e06-172">Om du vill lägga till ett block till en befintlig tilläggsblobb, använder du följande:</span><span class="sxs-lookup"><span data-stu-id="91e06-172">To append a block to an existing append blob, use the following:</span></span>

* <span data-ttu-id="91e06-173">**appendFromLocalFile** -Lägg till innehållet i en fil till en befintlig tilläggsblobb</span><span class="sxs-lookup"><span data-stu-id="91e06-173">**appendFromLocalFile** - append the contents of a file to an existing append blob</span></span>
* <span data-ttu-id="91e06-174">**appendFromStream** -Lägg till innehållet i en dataström till en befintlig tilläggsblobb</span><span class="sxs-lookup"><span data-stu-id="91e06-174">**appendFromStream** - append the contents of a stream to an existing append blob</span></span>
* <span data-ttu-id="91e06-175">**appendFromText** -Lägg till innehållet i en sträng i en befintlig tilläggsblobb</span><span class="sxs-lookup"><span data-stu-id="91e06-175">**appendFromText** - append the contents of a string to an existing append blob</span></span>
* <span data-ttu-id="91e06-176">**appendBlockFromStream** -Lägg till innehållet i en dataström till en befintlig tilläggsblobb</span><span class="sxs-lookup"><span data-stu-id="91e06-176">**appendBlockFromStream** - append the contents of a stream to an existing append blob</span></span>
* <span data-ttu-id="91e06-177">**appendBlockFromText** -Lägg till innehållet i en sträng i en befintlig tilläggsblobb</span><span class="sxs-lookup"><span data-stu-id="91e06-177">**appendBlockFromText** - append the contents of a string to an existing append blob</span></span>

> [!NOTE]
> <span data-ttu-id="91e06-178">appendFromXXX API: er kommer att göra vissa klientsidan verifiering för att snabbt kunna undvika onödiga serveranrop.</span><span class="sxs-lookup"><span data-stu-id="91e06-178">appendFromXXX APIs will do some client-side validation to fail fast to avoid unnecessary server calls.</span></span> <span data-ttu-id="91e06-179">appendBlockFromXXX inte.</span><span class="sxs-lookup"><span data-stu-id="91e06-179">appendBlockFromXXX won't.</span></span>
>
>

<span data-ttu-id="91e06-180">Följande kodexempel Överför innehållet i den **test.txt** filen till **myappendblob**.</span><span class="sxs-lookup"><span data-stu-id="91e06-180">The following code example uploads the contents of the **test.txt** file into **myappendblob**.</span></span>

```nodejs
blobSvc.appendFromText('mycontainer', 'myappendblob', 'text to be appended', function(error, result, response){
  if(!error){
    // text appended
  }
});
```

### <a name="page-blobs"></a><span data-ttu-id="91e06-181">Sidblobbar</span><span class="sxs-lookup"><span data-stu-id="91e06-181">Page blobs</span></span>
<span data-ttu-id="91e06-182">Om du vill överföra data till en sidblob, använder du följande:</span><span class="sxs-lookup"><span data-stu-id="91e06-182">To upload data to a page blob, use the following:</span></span>

* <span data-ttu-id="91e06-183">**createPageBlob** -skapar en ny sidblob för en viss längd</span><span class="sxs-lookup"><span data-stu-id="91e06-183">**createPageBlob** - creates a new page blob of a specific length</span></span>
* <span data-ttu-id="91e06-184">**createPageBlobFromLocalFile** – skapar en ny sidblob och överför innehållet i en fil</span><span class="sxs-lookup"><span data-stu-id="91e06-184">**createPageBlobFromLocalFile** - creates a new page blob and uploads the contents of a file</span></span>
* <span data-ttu-id="91e06-185">**createPageBlobFromStream** – skapar en ny sidblob och överför innehållet i en dataström</span><span class="sxs-lookup"><span data-stu-id="91e06-185">**createPageBlobFromStream** - creates a new page blob and uploads the contents of a stream</span></span>
* <span data-ttu-id="91e06-186">**createWriteStreamToExistingPageBlob** -ger en dataström med skrivning till en befintlig sidblob</span><span class="sxs-lookup"><span data-stu-id="91e06-186">**createWriteStreamToExistingPageBlob** - provides a write stream to an existing page blob</span></span>
* <span data-ttu-id="91e06-187">**createWriteStreamToNewPageBlob** – skapar en ny sidblob och ger en dataström med att skriva till den</span><span class="sxs-lookup"><span data-stu-id="91e06-187">**createWriteStreamToNewPageBlob** - creates a new page blob and then provides a stream to write to it</span></span>

<span data-ttu-id="91e06-188">Följande kodexempel Överför innehållet i den **test.txt** filen till **mypageblob**.</span><span class="sxs-lookup"><span data-stu-id="91e06-188">The following code example uploads the contents of the **test.txt** file into **mypageblob**.</span></span>

```nodejs
blobSvc.createPageBlobFromLocalFile('mycontainer', 'mypageblob', 'test.txt', function(error, result, response){
  if(!error){
    // file uploaded
  }
});
```

> [!NOTE]
> <span data-ttu-id="91e06-189">Sidblobbar består av 512 byte sidor.</span><span class="sxs-lookup"><span data-stu-id="91e06-189">Page blobs consist of 512-byte 'pages'.</span></span> <span data-ttu-id="91e06-190">Du får ett fel när överföra data med en storlek som inte är en multipel av 512.</span><span class="sxs-lookup"><span data-stu-id="91e06-190">You will receive an error when uploading data with a size that is not a multiple of 512.</span></span>
>
>

## <a name="list-the-blobs-in-a-container"></a><span data-ttu-id="91e06-191">Visa en lista över blobbarna i en behållare</span><span class="sxs-lookup"><span data-stu-id="91e06-191">List the blobs in a container</span></span>
<span data-ttu-id="91e06-192">Om du vill visa blobbar i en behållare använder den **listBlobsSegmented** metod.</span><span class="sxs-lookup"><span data-stu-id="91e06-192">To list the blobs in a container, use the **listBlobsSegmented** method.</span></span> <span data-ttu-id="91e06-193">Om du vill returnera blobbar med ett specifikt prefix använda **listBlobsSegmentedWithPrefix**.</span><span class="sxs-lookup"><span data-stu-id="91e06-193">If you'd like to return blobs with a specific prefix, use **listBlobsSegmentedWithPrefix**.</span></span>

```nodejs
blobSvc.listBlobsSegmented('mycontainer', null, function(error, result, response){
  if(!error){
      // result.entries contains the entries
      // If not all blobs were returned, result.continuationToken has the continuation token.
  }
});
```

<span data-ttu-id="91e06-194">Den `result` innehåller en `entries` samling, som är en matris med objekt som beskriver varje blob.</span><span class="sxs-lookup"><span data-stu-id="91e06-194">The `result` contains an `entries` collection, which is an array of objects that describe each blob.</span></span> <span data-ttu-id="91e06-195">Om alla BLOB inte kan returneras den `result` innehåller också en `continuationToken`, som kan användas som den andra parametern för att hämta ytterligare poster.</span><span class="sxs-lookup"><span data-stu-id="91e06-195">If all blobs cannot be returned, the `result` also provides a `continuationToken`, which you may use as the second parameter to retrieve additional entries.</span></span>

## <a name="download-blobs"></a><span data-ttu-id="91e06-196">Ladda ned blobbar</span><span class="sxs-lookup"><span data-stu-id="91e06-196">Download blobs</span></span>
<span data-ttu-id="91e06-197">Om du vill hämta data från en blob, använder du följande:</span><span class="sxs-lookup"><span data-stu-id="91e06-197">To download data from a blob, use the following:</span></span>

* <span data-ttu-id="91e06-198">**getBlobToLocalFile** -skriver blobbinnehållet till filen</span><span class="sxs-lookup"><span data-stu-id="91e06-198">**getBlobToLocalFile** - writes the blob contents to file</span></span>
* <span data-ttu-id="91e06-199">**getBlobToStream** -skriver blobbinnehållet till en dataström</span><span class="sxs-lookup"><span data-stu-id="91e06-199">**getBlobToStream** - writes the blob contents to a stream</span></span>
* <span data-ttu-id="91e06-200">**getBlobToText** -skriver blobbinnehållet till en sträng</span><span class="sxs-lookup"><span data-stu-id="91e06-200">**getBlobToText** - writes the blob contents to a string</span></span>
* <span data-ttu-id="91e06-201">**createReadStream** -ger en dataström med att läsa från blob</span><span class="sxs-lookup"><span data-stu-id="91e06-201">**createReadStream** - provides a stream to read from the blob</span></span>

<span data-ttu-id="91e06-202">I följande kodexempel visas hur du använder **getBlobToStream** att hämta innehållet i den **minblobb** blob och lagra den till den **output.txt** filen med hjälp av en dataström:</span><span class="sxs-lookup"><span data-stu-id="91e06-202">The following code example demonstrates using **getBlobToStream** to download the contents of the **myblob** blob and store it to the **output.txt** file by using a stream:</span></span>

```nodejs
var fs = require('fs');
blobSvc.getBlobToStream('mycontainer', 'myblob', fs.createWriteStream('output.txt'), function(error, result, response){
  if(!error){
    // blob retrieved
  }
});
```

<span data-ttu-id="91e06-203">Den `result` innehåller information om blob, inklusive **ETag** information.</span><span class="sxs-lookup"><span data-stu-id="91e06-203">The `result` contains information about the blob, including **ETag** information.</span></span>

## <a name="delete-a-blob"></a><span data-ttu-id="91e06-204">Ta bort en blob</span><span class="sxs-lookup"><span data-stu-id="91e06-204">Delete a blob</span></span>
<span data-ttu-id="91e06-205">Slutligen, om du vill ta bort en blobb anropa **deleteBlob**.</span><span class="sxs-lookup"><span data-stu-id="91e06-205">Finally, to delete a blob, call **deleteBlob**.</span></span> <span data-ttu-id="91e06-206">Följande exempel tar bort blob med namnet **minblobb**.</span><span class="sxs-lookup"><span data-stu-id="91e06-206">The following code example deletes the blob named **myblob**.</span></span>

```nodejs
blobSvc.deleteBlob(containerName, 'myblob', function(error, response){
  if(!error){
    // Blob has been deleted
  }
});
```

## <a name="concurrent-access"></a><span data-ttu-id="91e06-207">Samtidig åtkomst</span><span class="sxs-lookup"><span data-stu-id="91e06-207">Concurrent access</span></span>
<span data-ttu-id="91e06-208">Du kan använda för att stödja samtidig åtkomst till en blobb från flera klienter eller flera processinstanser, **ETags** eller **lån**.</span><span class="sxs-lookup"><span data-stu-id="91e06-208">To support concurrent access to a blob from multiple clients or multiple process instances, you can use **ETags** or **leases**.</span></span>

* <span data-ttu-id="91e06-209">**ETag** -ger ett sätt att identifiera att blob eller behållare har ändrats av en annan process</span><span class="sxs-lookup"><span data-stu-id="91e06-209">**Etag** - provides a way to detect that the blob or container has been modified by another process</span></span>
* <span data-ttu-id="91e06-210">**Lånet** -ger ett sätt att få exklusiv, förnyas, skriva eller ta bort åtkomst till en blob för en viss tidsperiod</span><span class="sxs-lookup"><span data-stu-id="91e06-210">**Lease** - provides a way to obtain exclusive, renewable, write or delete access to a blob for a period of time</span></span>

### <a name="etag"></a><span data-ttu-id="91e06-211">ETag</span><span class="sxs-lookup"><span data-stu-id="91e06-211">ETag</span></span>
<span data-ttu-id="91e06-212">Använd ETags om du vill tillåta flera klienter eller instanser att skriva till blocket Blob eller sidan Blob samtidigt.</span><span class="sxs-lookup"><span data-stu-id="91e06-212">Use ETags if you need to allow multiple clients or instances to write to the block Blob or page Blob simultaneously.</span></span> <span data-ttu-id="91e06-213">ETag kan du bestämma om behållare eller blobb har ändrats sedan du först läsa eller skapat, där du kan undvika att skriva över ändringarna av en annan klient eller process.</span><span class="sxs-lookup"><span data-stu-id="91e06-213">The ETag allows you to determine if the container or blob was modified since you initially read or created it, which allows you to avoid overwriting changes committed by another client or process.</span></span>

<span data-ttu-id="91e06-214">Du kan ange ETag villkor med hjälp av den valfria `options.accessConditions` parameter.</span><span class="sxs-lookup"><span data-stu-id="91e06-214">You can set ETag conditions by using the optional `options.accessConditions` parameter.</span></span> <span data-ttu-id="91e06-215">I följande kod exempel endast överföringar av **test.txt** -filen om blob finns redan och ETag-värde som innehåller `etagToMatch`.</span><span class="sxs-lookup"><span data-stu-id="91e06-215">The following code example only uploads the **test.txt** file if the blob already exists and has the ETag value contained by `etagToMatch`.</span></span>

```nodejs
blobSvc.createBlockBlobFromLocalFile('mycontainer', 'myblob', 'test.txt', { accessConditions: { EtagMatch: etagToMatch} }, function(error, result, response){
    if(!error){
    // file uploaded
  }
});
```

<span data-ttu-id="91e06-216">När du använder ETags är allmänna mönster:</span><span class="sxs-lookup"><span data-stu-id="91e06-216">When you're using ETags, the general pattern is:</span></span>

1. <span data-ttu-id="91e06-217">Hämta ETag som ett resultat av att skapa, listor eller hämta-åtgärden.</span><span class="sxs-lookup"><span data-stu-id="91e06-217">Obtain the ETag as the result of a create, list, or get operation.</span></span>
2. <span data-ttu-id="91e06-218">Utföra en åtgärd som kontrollerar ETag-värdet inte har ändrats.</span><span class="sxs-lookup"><span data-stu-id="91e06-218">Perform an action, checking that the ETag value has not been modified.</span></span>

<span data-ttu-id="91e06-219">Om värdet har ändrats, betyder det att en annan klient eller instans ändrats blob eller behållaren eftersom du har köpt ETag-värde.</span><span class="sxs-lookup"><span data-stu-id="91e06-219">If the value was modified, this indicates that another client or instance modified the blob or container since you obtained the ETag value.</span></span>

### <a name="lease"></a><span data-ttu-id="91e06-220">Lån</span><span class="sxs-lookup"><span data-stu-id="91e06-220">Lease</span></span>
<span data-ttu-id="91e06-221">Du kan skaffa ett nytt lån med hjälp av den **acquireLease** -metoden anger blob eller behållare som du vill få ett lån på.</span><span class="sxs-lookup"><span data-stu-id="91e06-221">You can acquire a new lease by using the **acquireLease** method, specifying the blob or container that you wish to obtain a lease on.</span></span> <span data-ttu-id="91e06-222">Till exempel följande kod får ett lån på **minblobb**.</span><span class="sxs-lookup"><span data-stu-id="91e06-222">For example, the following code acquires a lease on **myblob**.</span></span>

```nodejs
blobSvc.acquireLease('mycontainer', 'myblob', function(error, result, response){
  if(!error) {
    console.log('leaseId: ' + result.id);
  }
});
```

<span data-ttu-id="91e06-223">Efterföljande åtgärder på **minblobb** måste ange den `options.leaseId` parameter.</span><span class="sxs-lookup"><span data-stu-id="91e06-223">Subsequent operations on **myblob** must provide the `options.leaseId` parameter.</span></span> <span data-ttu-id="91e06-224">Lånet ID returneras som `result.id` från **acquireLease**.</span><span class="sxs-lookup"><span data-stu-id="91e06-224">The lease ID is returned as `result.id` from **acquireLease**.</span></span>

> [!NOTE]
> <span data-ttu-id="91e06-225">Som standard är oändlig lånetid.</span><span class="sxs-lookup"><span data-stu-id="91e06-225">By default, the lease duration is infinite.</span></span> <span data-ttu-id="91e06-226">Du kan ange en icke oändliga varaktigheten (mellan 15 och 60 sekunder) genom att tillhandahålla den `options.leaseDuration` parameter.</span><span class="sxs-lookup"><span data-stu-id="91e06-226">You can specify a non-infinite duration (between 15 and 60 seconds) by providing the `options.leaseDuration` parameter.</span></span>
>
>

<span data-ttu-id="91e06-227">Ta bort ett lån med **releaseLease**.</span><span class="sxs-lookup"><span data-stu-id="91e06-227">To remove a lease, use **releaseLease**.</span></span> <span data-ttu-id="91e06-228">Om du vill dela upp ett lån, men förhindra andra från att erhålla ett nytt lån till den ursprungliga varaktigheten har gått ut, Använd **breakLease**.</span><span class="sxs-lookup"><span data-stu-id="91e06-228">To break a lease, but prevent others from obtaining a new lease until the original duration has expired, use **breakLease**.</span></span>

## <a name="work-with-shared-access-signatures"></a><span data-ttu-id="91e06-229">Arbeta med signaturer för delad åtkomst</span><span class="sxs-lookup"><span data-stu-id="91e06-229">Work with shared access signatures</span></span>
<span data-ttu-id="91e06-230">Signaturer för delad åtkomst (SAS) är ett säkert sätt att tillhandahålla detaljerade åtkomst till blobbar och behållare utan att erbjuda dina lagringskontonamn eller nycklar.</span><span class="sxs-lookup"><span data-stu-id="91e06-230">Shared access signatures (SAS) are a secure way to provide granular access to blobs and containers without providing your storage account name or keys.</span></span> <span data-ttu-id="91e06-231">Signaturer för delad åtkomst används ofta för att ge begränsad åtkomst till dina data, till exempel att tillåta en mobil app åtkomst till blobbar.</span><span class="sxs-lookup"><span data-stu-id="91e06-231">Shared access signatures are often used to provide limited access to your data, such as allowing a mobile app to access blobs.</span></span>

> [!NOTE]
> <span data-ttu-id="91e06-232">Medan du kan också tillåta anonym åtkomst till blobbar, kan du ange mer kontrollerad åtkomst, som måste du generera SAS signaturer för delad åtkomst.</span><span class="sxs-lookup"><span data-stu-id="91e06-232">While you can also allow anonymous access to blobs, shared access signatures allow you to provide more controlled access, as you must generate the SAS.</span></span>
>
>

<span data-ttu-id="91e06-233">En betrodda program, till exempel en molnbaserad tjänst genererar signaturer för delad åtkomst med hjälp av den **generateSharedAccessSignature** av den **BlobService**, och ger den till ett program som inte är betrodd eller delvis betrodd, till exempel en mobil app.</span><span class="sxs-lookup"><span data-stu-id="91e06-233">A trusted application such as a cloud-based service generates shared access signatures using the **generateSharedAccessSignature** of the **BlobService**, and provides it to an untrusted or semi-trusted application such as a mobile app.</span></span> <span data-ttu-id="91e06-234">Delad åtkomst signaturer genereras med hjälp av en princip som beskriver start- och slutdatum då signaturer för delad åtkomst är giltiga, samt åtkomstnivån som beviljas åtkomst till delad åtkomst signaturer innehavaren.</span><span class="sxs-lookup"><span data-stu-id="91e06-234">Shared access signatures are generated using a policy, which describes the start and end dates during which the shared access signatures are valid, as well as the access level granted to the shared access signatures holder.</span></span>

<span data-ttu-id="91e06-235">Följande exempel skapar en ny princip för delad åtkomst som tillåter signaturer för delad åtkomst innehavaren utföra läsåtgärder på den **minblobb** blob och upphör att gälla 100 minuter efter den tidpunkt som den har skapats.</span><span class="sxs-lookup"><span data-stu-id="91e06-235">The following code example generates a new shared access policy that allows the shared access signatures holder to perform read operations on the **myblob** blob, and expires 100 minutes after the time it is created.</span></span>

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

<span data-ttu-id="91e06-236">Observera att information om värden måste anges, eftersom det är nödvändigt när delad åtkomst signaturer innehavaren försöker få åtkomst till behållaren.</span><span class="sxs-lookup"><span data-stu-id="91e06-236">Note that the host information must be provided also, as it is required when the shared access signatures holder attempts to access the container.</span></span>

<span data-ttu-id="91e06-237">Klientprogrammet sedan använder signaturer för delad åtkomst med **BlobServiceWithSAS** att utföra åtgärder mot blob.</span><span class="sxs-lookup"><span data-stu-id="91e06-237">The client application then uses shared access signatures with **BlobServiceWithSAS** to perform operations against the blob.</span></span> <span data-ttu-id="91e06-238">Följande hämtar information **minblobb**.</span><span class="sxs-lookup"><span data-stu-id="91e06-238">The following gets information about **myblob**.</span></span>

```nodejs
var sharedBlobSvc = azure.createBlobServiceWithSas(host, blobSAS);
sharedBlobSvc.getBlobProperties('mycontainer', 'myblob', function (error, result, response) {
  if(!error) {
    // retrieved info
  }
});
```

<span data-ttu-id="91e06-239">Eftersom signaturer för delad åtkomst har skapats med skrivskyddad åtkomst om ett försök görs att ändra blob, returneras ett fel.</span><span class="sxs-lookup"><span data-stu-id="91e06-239">Since the shared access signatures were generated with read-only access, if an attempt is made to modify the blob, an error will be returned.</span></span>

### <a name="access-control-lists"></a><span data-ttu-id="91e06-240">Listor för åtkomstkontroll</span><span class="sxs-lookup"><span data-stu-id="91e06-240">Access control lists</span></span>
<span data-ttu-id="91e06-241">Du kan också använda en åtkomstkontrollista (ACL) för att ange åtkomstprincipen för SAS.</span><span class="sxs-lookup"><span data-stu-id="91e06-241">You can also use an access control list (ACL) to set the access policy for SAS.</span></span> <span data-ttu-id="91e06-242">Detta är användbart om du vill att flera klienter kan komma åt en behållare men ange olika principer för varje klient.</span><span class="sxs-lookup"><span data-stu-id="91e06-242">This is useful if you wish to allow multiple clients to access a container but provide different access policies for each client.</span></span>

<span data-ttu-id="91e06-243">En ACL implementeras med hjälp av en matris med principer för åtkomst med ett ID som är associerade med varje princip.</span><span class="sxs-lookup"><span data-stu-id="91e06-243">An ACL is implemented using an array of access policies, with an ID associated with each policy.</span></span> <span data-ttu-id="91e06-244">Följande exempel definierar två principer, en för 'Användare1' och en för 'användare2':</span><span class="sxs-lookup"><span data-stu-id="91e06-244">The following code example defines two policies, one for 'user1' and one for 'user2':</span></span>

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

<span data-ttu-id="91e06-245">Följande exempel hämtar den aktuella ACL för **minbehållare**, och sedan lägger till nya principer med hjälp av **setBlobAcl**.</span><span class="sxs-lookup"><span data-stu-id="91e06-245">The following code example gets the current ACL for **mycontainer**, and then adds the new policies using **setBlobAcl**.</span></span> <span data-ttu-id="91e06-246">Den här metoden kan:</span><span class="sxs-lookup"><span data-stu-id="91e06-246">This approach allows:</span></span>

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

<span data-ttu-id="91e06-247">Du kan sedan skapa signaturer för delad åtkomst baserat på en princip-ID när Åtkomstkontrollistan har angetts.</span><span class="sxs-lookup"><span data-stu-id="91e06-247">Once the ACL is set, you can then create shared access signatures based on the ID for a policy.</span></span> <span data-ttu-id="91e06-248">Följande exempel skapar nya signaturer för delad åtkomst för 'användare2':</span><span class="sxs-lookup"><span data-stu-id="91e06-248">The following code example creates new shared access signatures for 'user2':</span></span>

```nodejs
blobSAS = blobSvc.generateSharedAccessSignature('mycontainer', { Id: 'user2' });
```

## <a name="next-steps"></a><span data-ttu-id="91e06-249">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="91e06-249">Next steps</span></span>
<span data-ttu-id="91e06-250">Mer information finns i följande resurser.</span><span class="sxs-lookup"><span data-stu-id="91e06-250">For more information, see the following resources.</span></span>

* <span data-ttu-id="91e06-251">[Azure Storage SDK för noden API-referens][Azure Storage SDK for Node API Reference]</span><span class="sxs-lookup"><span data-stu-id="91e06-251">[Azure Storage SDK for Node API Reference][Azure Storage SDK for Node API Reference]</span></span>
* <span data-ttu-id="91e06-252">[Azure Storage-teamets blogg][Azure Storage Team Blog]</span><span class="sxs-lookup"><span data-stu-id="91e06-252">[Azure Storage Team Blog][Azure Storage Team Blog]</span></span>
* <span data-ttu-id="91e06-253">[Azure Storage SDK: N för noden] [ Azure Storage SDK for Node] databasen på GitHub</span><span class="sxs-lookup"><span data-stu-id="91e06-253">[Azure Storage SDK for Node][Azure Storage SDK for Node] repository on GitHub</span></span>
* [<span data-ttu-id="91e06-254">Node.js Developer Center</span><span class="sxs-lookup"><span data-stu-id="91e06-254">Node.js Developer Center</span></span>](https://azure.microsoft.com/develop/nodejs/)
* [<span data-ttu-id="91e06-255">Överföra data med kommandoradsverktyget AzCopy</span><span class="sxs-lookup"><span data-stu-id="91e06-255">Transfer data with the AzCopy command-line utility</span></span>](storage-use-azcopy.md)

[Azure Storage SDK for Node]: https://github.com/Azure/azure-storage-node

<span data-ttu-id="91e06-256">[skapa en Node.js-webbapp i Azure App Service]: ../app-service-web/app-service-web-get-started-nodejs.md</span><span class="sxs-lookup"><span data-stu-id="91e06-256">[Create a Node.js web app in Azure App Service]: ../app-service-web/app-service-web-get-started-nodejs.md</span></span>
<span data-ttu-id="91e06-257">[Node.js-webbapp som använder tjänsten Azure Table]: ../app-service-web/storage-nodejs-use-table-storage-web-site.md</span><span class="sxs-lookup"><span data-stu-id="91e06-257">[Node.js web app using the Azure Table Service]: ../app-service-web/storage-nodejs-use-table-storage-web-site.md</span></span>
<span data-ttu-id="91e06-258">[skapa och distribuera en Node.js-webbapp till Azure med hjälp av Web Matrix]: ../app-service-web/web-sites-nodejs-use-webmatrix.md</span><span class="sxs-lookup"><span data-stu-id="91e06-258">[Build and deploy a Node.js web app to Azure using Web Matrix]: ../app-service-web/web-sites-nodejs-use-webmatrix.md</span></span>
[Using the REST API]: http://msdn.microsoft.com/library/azure/hh264518.aspx
[Azure portal]: https://portal.azure.com
<span data-ttu-id="91e06-259">[skapa och distribuera ett Node.js-program till en Azure-molntjänst]: ../cloud-services/cloud-services-nodejs-develop-deploy-app.md</span><span class="sxs-lookup"><span data-stu-id="91e06-259">[Build and deploy a Node.js application to an Azure Cloud Service]: ../cloud-services/cloud-services-nodejs-develop-deploy-app.md</span></span>
[Azure Storage Team Blog]: http://blogs.msdn.com/b/windowsazurestorage/
[Azure Storage SDK for Node API Reference]: http://dl.windowsazure.com/nodestoragedocs/index.html
