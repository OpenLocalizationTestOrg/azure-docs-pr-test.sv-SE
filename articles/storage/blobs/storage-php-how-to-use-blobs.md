---
title: "aaaHow toouse blob storage (objektlagring) från PHP | Microsoft Docs"
description: Lagra Ostrukturerade data i hello moln med Azure Blob storage (objektlagring).
documentationcenter: php
services: storage
author: mmacy
manager: timlt
editor: tysonn
ms.assetid: 1af56b59-b3f0-4b46-8441-aab463ae088e
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: PHP
ms.topic: article
ms.date: 12/08/2016
ms.author: marsma
ms.openlocfilehash: 2e77415519b38007652e3ea372da531b3a97c5d4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="how-toouse-blob-storage-from-php"></a><span data-ttu-id="425d3-103">Hur toouse blob storage från PHP</span><span class="sxs-lookup"><span data-stu-id="425d3-103">How toouse blob storage from PHP</span></span>
[!INCLUDE [storage-selector-blob-include](../../../includes/storage-selector-blob-include.md)]

[!INCLUDE [storage-try-azure-tools-queues](../../../includes/storage-try-azure-tools-blobs.md)]

## <a name="overview"></a><span data-ttu-id="425d3-104">Översikt</span><span class="sxs-lookup"><span data-stu-id="425d3-104">Overview</span></span>
<span data-ttu-id="425d3-105">Azure Blob storage är en tjänst som lagrar Ostrukturerade data i hello molnet som objekt/blobbar.</span><span class="sxs-lookup"><span data-stu-id="425d3-105">Azure Blob storage is a service that stores unstructured data in hello cloud as objects/blobs.</span></span> <span data-ttu-id="425d3-106">Blob Storage kan lagra alla slags textdata eller binära data, till exempel ett dokument, en mediefil eller ett installationsprogram.</span><span class="sxs-lookup"><span data-stu-id="425d3-106">Blob storage can store any type of text or binary data, such as a document, media file, or application installer.</span></span> <span data-ttu-id="425d3-107">BLOB storage är också hänvisade tooas objektlagring.</span><span class="sxs-lookup"><span data-stu-id="425d3-107">Blob storage is also referred tooas object storage.</span></span>

<span data-ttu-id="425d3-108">Den här guiden visar hur tooperform vanliga scenarier med hjälp av hello Azure blob-tjänst.</span><span class="sxs-lookup"><span data-stu-id="425d3-108">This guide shows you how tooperform common scenarios using hello Azure blob service.</span></span> <span data-ttu-id="425d3-109">hello exemplen är skrivna i PHP och använder hello [Azure SDK för PHP][download].</span><span class="sxs-lookup"><span data-stu-id="425d3-109">hello samples are written in PHP and use hello [Azure SDK for PHP][download].</span></span> <span data-ttu-id="425d3-110">hello beskrivs scenarier där **överför**, **lista**, **hämtar**, och **bort** blobbar.</span><span class="sxs-lookup"><span data-stu-id="425d3-110">hello scenarios covered include **uploading**, **listing**, **downloading**, and **deleting** blobs.</span></span> <span data-ttu-id="425d3-111">Mer information om BLOB finns hello [nästa steg](#next-steps) avsnitt.</span><span class="sxs-lookup"><span data-stu-id="425d3-111">For more information on blobs, see hello [Next steps](#next-steps) section.</span></span>

[!INCLUDE [storage-blob-concepts-include](../../../includes/storage-blob-concepts-include.md)]

[!INCLUDE [storage-create-account-include](../../../includes/storage-create-account-include.md)]

## <a name="create-a-php-application"></a><span data-ttu-id="425d3-112">Skapa en PHP-program</span><span class="sxs-lookup"><span data-stu-id="425d3-112">Create a PHP application</span></span>
<span data-ttu-id="425d3-113">Hej krav för att skapa en PHP-program som ansluter till hello Azure blob-tjänsten är hello refererar till klasser i hello Azure SDK för PHP från inom din kod.</span><span class="sxs-lookup"><span data-stu-id="425d3-113">hello only requirement for creating a PHP application that accesses hello Azure blob service is hello referencing of classes in hello Azure SDK for PHP from within your code.</span></span> <span data-ttu-id="425d3-114">Du kan använda alla development tools toocreate ditt program, inklusive anteckningar.</span><span class="sxs-lookup"><span data-stu-id="425d3-114">You can use any development tools toocreate your application, including Notepad.</span></span>

<span data-ttu-id="425d3-115">I den här guiden kan du använda tjänstens funktioner som kan anropas inom ett PHP-program lokalt eller i koden körs i en Azure-webbroll, en arbetsroll eller en webbplats.</span><span class="sxs-lookup"><span data-stu-id="425d3-115">In this guide, you use service features, which can be called within a PHP application locally or in code running within an Azure web role, worker role, or website.</span></span>

## <a name="get-hello-azure-client-libraries"></a><span data-ttu-id="425d3-116">Hämta hello Azure-klientbibliotek</span><span class="sxs-lookup"><span data-stu-id="425d3-116">Get hello Azure Client Libraries</span></span>
[!INCLUDE [get-client-libraries](../../../includes/get-client-libraries.md)]

## <a name="configure-your-application-tooaccess-hello-blob-service"></a><span data-ttu-id="425d3-117">Konfigurera ditt program tooaccess hello blob-tjänsten</span><span class="sxs-lookup"><span data-stu-id="425d3-117">Configure your application tooaccess hello blob service</span></span>
<span data-ttu-id="425d3-118">toouse hello Azure blob-tjänsten API: er, måste du:</span><span class="sxs-lookup"><span data-stu-id="425d3-118">toouse hello Azure blob service APIs, you need to:</span></span>

1. <span data-ttu-id="425d3-119">Referens hello bandladdaren fil med hello [require_once] -instruktionen och</span><span class="sxs-lookup"><span data-stu-id="425d3-119">Reference hello autoloader file using hello [require_once] statement, and</span></span>
2. <span data-ttu-id="425d3-120">Referera till alla klasser som du kan använda.</span><span class="sxs-lookup"><span data-stu-id="425d3-120">Reference any classes you might use.</span></span>

<span data-ttu-id="425d3-121">hello följande exempel visas hur tooinclude hello bandladdaren fil- och hello **ServicesBuilder** klass.</span><span class="sxs-lookup"><span data-stu-id="425d3-121">hello following example shows how tooinclude hello autoloader file and reference hello **ServicesBuilder** class.</span></span>

> [!NOTE]
> <span data-ttu-id="425d3-122">hello exemplen i den här artikeln förutsätter att du har installerat hello PHP-klientbibliotek för Azure via Composer.</span><span class="sxs-lookup"><span data-stu-id="425d3-122">hello examples in this article assume you have installed hello PHP Client Libraries for Azure via Composer.</span></span> <span data-ttu-id="425d3-123">Om du har installerat hello bibliotek manuellt måste tooreference hello `WindowsAzure.php` bandladdaren filen.</span><span class="sxs-lookup"><span data-stu-id="425d3-123">If you installed hello libraries manually, you need tooreference hello `WindowsAzure.php` autoloader file.</span></span>
>
>

```php
require_once 'vendor/autoload.php';
use WindowsAzure\Common\ServicesBuilder;
```

<span data-ttu-id="425d3-124">I hello exemplen nedan hello `require_once` instruktionen visas alltid, men bara hello klasser krävs för hello exempel tooexecute refererar till.</span><span class="sxs-lookup"><span data-stu-id="425d3-124">In hello examples below, hello `require_once` statement will be shown always, but only hello classes necessary for hello example tooexecute are referenced.</span></span>

## <a name="set-up-an-azure-storage-connection"></a><span data-ttu-id="425d3-125">Skapa en Azure storage-anslutning</span><span class="sxs-lookup"><span data-stu-id="425d3-125">Set up an Azure storage connection</span></span>
<span data-ttu-id="425d3-126">tooinstantiate klienten ett Azure blob-tjänsten måste du först ha en giltig anslutningssträng.</span><span class="sxs-lookup"><span data-stu-id="425d3-126">tooinstantiate an Azure blob service client, you must first have a valid connection string.</span></span> <span data-ttu-id="425d3-127">hello-formatet för anslutningssträngen för hello blob-tjänsten är:</span><span class="sxs-lookup"><span data-stu-id="425d3-127">hello format for hello blob service connection string is:</span></span>

<span data-ttu-id="425d3-128">För att komma åt en live-tjänst:</span><span class="sxs-lookup"><span data-stu-id="425d3-128">For accessing a live service:</span></span>

```php
DefaultEndpointsProtocol=[http|https];AccountName=[yourAccount];AccountKey=[yourKey]
```

<span data-ttu-id="425d3-129">För att komma åt hello storage-emulatorn:</span><span class="sxs-lookup"><span data-stu-id="425d3-129">For accessing hello storage emulator:</span></span>

```php
UseDevelopmentStorage=true
```

<span data-ttu-id="425d3-130">toocreate alla Azure-tjänst-klienter, behöver du toouse hello **ServicesBuilder** klass.</span><span class="sxs-lookup"><span data-stu-id="425d3-130">toocreate any Azure service client, you need toouse hello **ServicesBuilder** class.</span></span> <span data-ttu-id="425d3-131">Du kan:</span><span class="sxs-lookup"><span data-stu-id="425d3-131">You can:</span></span>

* <span data-ttu-id="425d3-132">Skicka hello anslutning direkt string tooit eller</span><span class="sxs-lookup"><span data-stu-id="425d3-132">Pass hello connection string directly tooit or</span></span>
* <span data-ttu-id="425d3-133">Använd hello **CloudConfigurationManager (CCM)** toocheck flera externa datakällor för hello anslutningssträngen:</span><span class="sxs-lookup"><span data-stu-id="425d3-133">Use hello **CloudConfigurationManager (CCM)** toocheck multiple external sources for hello connection string:</span></span>
  * <span data-ttu-id="425d3-134">Som standard levereras den med stöd för en extern källa - miljövariabler.</span><span class="sxs-lookup"><span data-stu-id="425d3-134">By default, it comes with support for one external source - environmental variables.</span></span>
  * <span data-ttu-id="425d3-135">Du kan lägga till nya källor genom att utöka hello **ConnectionStringSource** klass.</span><span class="sxs-lookup"><span data-stu-id="425d3-135">You can add new sources by extending hello **ConnectionStringSource** class.</span></span>

<span data-ttu-id="425d3-136">Hello-exempel som beskrivs här skickas hello anslutningssträngen direkt.</span><span class="sxs-lookup"><span data-stu-id="425d3-136">For hello examples outlined here, hello connection string will be passed directly.</span></span>

```php
require_once 'vendor/autoload.php';

use WindowsAzure\Common\ServicesBuilder;

$blobRestProxy = ServicesBuilder::getInstance()->createBlobService($connectionString);
```

## <a name="create-a-container"></a><span data-ttu-id="425d3-137">Skapa en behållare</span><span class="sxs-lookup"><span data-stu-id="425d3-137">Create a container</span></span>
[!INCLUDE [storage-container-naming-rules-include](../../../includes/storage-container-naming-rules-include.md)]

<span data-ttu-id="425d3-138">En **BlobRestProxy** objekt kan du skapa en blob-behållare med hello **createContainer** metod.</span><span class="sxs-lookup"><span data-stu-id="425d3-138">A **BlobRestProxy** object lets you create a blob container with hello **createContainer** method.</span></span> <span data-ttu-id="425d3-139">När du skapar en behållare, du kan ange alternativ för hello behållare, men detta så krävs inte.</span><span class="sxs-lookup"><span data-stu-id="425d3-139">When creating a container, you can set options on hello container, but doing so is not required.</span></span> <span data-ttu-id="425d3-140">(hello exemplet nedan visar hur tooset hello behållaren åt åtkomstkontrollistor (ACL) och metadata för behållaren.)</span><span class="sxs-lookup"><span data-stu-id="425d3-140">(hello example below shows how tooset hello container access control list (ACL) and container metadata.)</span></span>

```php
require_once 'vendor\autoload.php';

use WindowsAzure\Common\ServicesBuilder;
use MicrosoftAzure\Storage\Blob\Models\CreateContainerOptions;
use MicrosoftAzure\Storage\Blob\Models\PublicAccessType;
use MicrosoftAzure\Storage\Common\ServiceException;

// Create blob REST proxy.
$blobRestProxy = ServicesBuilder::getInstance()->createBlobService($connectionString);


// OPTIONAL: Set public access policy and metadata.
// Create container options object.
$createContainerOptions = new CreateContainerOptions();

// Set public access policy. Possible values are
// PublicAccessType::CONTAINER_AND_BLOBS and PublicAccessType::BLOBS_ONLY.
// CONTAINER_AND_BLOBS:
// Specifies full public read access for container and blob data.
// proxys can enumerate blobs within hello container via anonymous
// request, but cannot enumerate containers within hello storage account.
//
// BLOBS_ONLY:
// Specifies public read access for blobs. Blob data within this
// container can be read via anonymous request, but container data is not
// available. proxys cannot enumerate blobs within hello container via
// anonymous request.
// If this value is not specified in hello request, container data is
// private toohello account owner.
$createContainerOptions->setPublicAccess(PublicAccessType::CONTAINER_AND_BLOBS);

// Set container metadata.
$createContainerOptions->addMetaData("key1", "value1");
$createContainerOptions->addMetaData("key2", "value2");

try    {
    // Create container.
    $blobRestProxy->createContainer("mycontainer", $createContainerOptions);
}
catch(ServiceException $e){
    // Handle exception based on error codes and messages.
    // Error codes and messages are here:
    // http://msdn.microsoft.com/library/azure/dd179439.aspx
    $code = $e->getCode();
    $error_message = $e->getMessage();
    echo $code.": ".$error_message."<br />";
}
```

<span data-ttu-id="425d3-141">Anropar **setPublicAccess (PublicAccessType::CONTAINER\_och\_BLOBBAR)** gör hello-behållaren och blob data som är tillgänglig via anonyma förfrågningar.</span><span class="sxs-lookup"><span data-stu-id="425d3-141">Calling **setPublicAccess(PublicAccessType::CONTAINER\_AND\_BLOBS)** makes hello container and blob data accessible via anonymous requests.</span></span> <span data-ttu-id="425d3-142">Anropar **setPublicAccess(PublicAccessType::BLOBS_ONLY)** gör endast blob-data som är tillgänglig via anonyma förfrågningar.</span><span class="sxs-lookup"><span data-stu-id="425d3-142">Calling **setPublicAccess(PublicAccessType::BLOBS_ONLY)** makes only blob data accessible via anonymous requests.</span></span> <span data-ttu-id="425d3-143">Mer information om behållaren ACL: er finns [Set behållar-ACL (REST-API)][container-acl].</span><span class="sxs-lookup"><span data-stu-id="425d3-143">For more information about container ACLs, see [Set container ACL (REST API)][container-acl].</span></span>

<span data-ttu-id="425d3-144">Mer information om felkoder för Blob-tjänsten finns [Blob felkoder][error-codes].</span><span class="sxs-lookup"><span data-stu-id="425d3-144">For more information about Blob service error codes, see [Blob Service Error Codes][error-codes].</span></span>

## <a name="upload-a-blob-into-a-container"></a><span data-ttu-id="425d3-145">Ladda upp en blobb till en behållare</span><span class="sxs-lookup"><span data-stu-id="425d3-145">Upload a blob into a container</span></span>
<span data-ttu-id="425d3-146">tooupload en fil som en blob, Använd hello **BlobRestProxy -> createBlockBlob** metod.</span><span class="sxs-lookup"><span data-stu-id="425d3-146">tooupload a file as a blob, use hello **BlobRestProxy->createBlockBlob** method.</span></span> <span data-ttu-id="425d3-147">Den här åtgärden skapar hello blob om det inte finns, eller skrivs över om den finns.</span><span class="sxs-lookup"><span data-stu-id="425d3-147">This operation creates hello blob if it doesn't exist, or overwrites it if it does.</span></span> <span data-ttu-id="425d3-148">hello kodexemplet nedan förutsätter hello behållaren redan har skapats och använder [fopen] [ fopen] tooopen hello-filen som en dataström.</span><span class="sxs-lookup"><span data-stu-id="425d3-148">hello code example below assumes that hello container has already been created and uses [fopen][fopen] tooopen hello file as a stream.</span></span>

```php
require_once 'vendor/autoload.php';

use WindowsAzure\Common\ServicesBuilder;
use MicrosoftAzure\Storage\Common\ServiceException;

// Create blob REST proxy.
$blobRestProxy = ServicesBuilder::getInstance()->createBlobService($connectionString);


$content = fopen("c:\myfile.txt", "r");
$blob_name = "myblob";

try    {
    //Upload blob
    $blobRestProxy->createBlockBlob("mycontainer", $blob_name, $content);
}
catch(ServiceException $e){
    // Handle exception based on error codes and messages.
    // Error codes and messages are here:
    // http://msdn.microsoft.com/library/azure/dd179439.aspx
    $code = $e->getCode();
    $error_message = $e->getMessage();
    echo $code.": ".$error_message."<br />";
}
```

<span data-ttu-id="425d3-149">Observera att hello föregående exempel laddar upp en blob som en dataström.</span><span class="sxs-lookup"><span data-stu-id="425d3-149">Note that hello previous sample uploads a blob as a stream.</span></span> <span data-ttu-id="425d3-150">En blob kan dock också överföras som en sträng med till exempel hello [filen\_hämta\_innehållet] [ file_get_contents] funktion.</span><span class="sxs-lookup"><span data-stu-id="425d3-150">However, a blob can also be uploaded as a string using, for example, hello [file\_get\_contents][file_get_contents] function.</span></span> <span data-ttu-id="425d3-151">toodo denna med hello föregående exempel kan ändra `$content = fopen("c:\myfile.txt", "r");` för`$content = file_get_contents("c:\myfile.txt");`.</span><span class="sxs-lookup"><span data-stu-id="425d3-151">toodo this using hello previous sample, change `$content = fopen("c:\myfile.txt", "r");` too`$content = file_get_contents("c:\myfile.txt");`.</span></span>

## <a name="list-hello-blobs-in-a-container"></a><span data-ttu-id="425d3-152">Lista hello blobbar i en behållare</span><span class="sxs-lookup"><span data-stu-id="425d3-152">List hello blobs in a container</span></span>
<span data-ttu-id="425d3-153">toolist hello blobbar i en behållare använder hello **BlobRestProxy -> listBlobs** metod med en **foreach** slinga tooloop via hello resultat.</span><span class="sxs-lookup"><span data-stu-id="425d3-153">toolist hello blobs in a container, use hello **BlobRestProxy->listBlobs** method with a **foreach** loop tooloop through hello result.</span></span> <span data-ttu-id="425d3-154">hello följande kod visar hello namnet på varje blob som utdata i en behållare och visar dess URI toohello webbläsare.</span><span class="sxs-lookup"><span data-stu-id="425d3-154">hello following code displays hello name of each blob as output in a container and displays its URI toohello browser.</span></span>

```php
require_once 'vendor/autoload.php';

use WindowsAzure\Common\ServicesBuilder;
use MicrosoftAzure\Storage\Common\ServiceException;

// Create blob REST proxy.
$blobRestProxy = ServicesBuilder::getInstance()->createBlobService($connectionString);


try    {
    // List blobs.
    $blob_list = $blobRestProxy->listBlobs("mycontainer");
    $blobs = $blob_list->getBlobs();

    foreach($blobs as $blob)
    {
        echo $blob->getName().": ".$blob->getUrl()."<br />";
    }
}
catch(ServiceException $e){
    // Handle exception based on error codes and messages.
    // Error codes and messages are here:
    // http://msdn.microsoft.com/library/azure/dd179439.aspx
    $code = $e->getCode();
    $error_message = $e->getMessage();
    echo $code.": ".$error_message."<br />";
}
```

## <a name="download-a-blob"></a><span data-ttu-id="425d3-155">Ladda ned en blob</span><span class="sxs-lookup"><span data-stu-id="425d3-155">Download a blob</span></span>
<span data-ttu-id="425d3-156">toodownload blob anropet hello **BlobRestProxy -> getBlob** metod och sedan anropa hello **getContentStream** metod på hello resulterande **GetBlobResult** objekt.</span><span class="sxs-lookup"><span data-stu-id="425d3-156">toodownload a blob, call hello **BlobRestProxy->getBlob** method, then call hello **getContentStream** method on hello resulting **GetBlobResult** object.</span></span>

```php
require_once 'vendor/autoload.php';

use WindowsAzure\Common\ServicesBuilder;
use MicrosoftAzure\Storage\Common\ServiceException;

// Create blob REST proxy.
$blobRestProxy = ServicesBuilder::getInstance()->createBlobService($connectionString);


try    {
    // Get blob.
    $blob = $blobRestProxy->getBlob("mycontainer", "myblob");
    fpassthru($blob->getContentStream());
}
catch(ServiceException $e){
    // Handle exception based on error codes and messages.
    // Error codes and messages are here:
    // http://msdn.microsoft.com/library/azure/dd179439.aspx
    $code = $e->getCode();
    $error_message = $e->getMessage();
    echo $code.": ".$error_message."<br />";
}
```

<span data-ttu-id="425d3-157">Observera att hello-exemplet ovan hämtar en blob som en dataström-resurs (hello standardbeteendet).</span><span class="sxs-lookup"><span data-stu-id="425d3-157">Note that hello example above gets a blob as a stream resource (hello default behavior).</span></span> <span data-ttu-id="425d3-158">Du kan dock använda hello [dataströmmen\_hämta\_innehållet] [ stream-get-contents] funktionen tooconvert hello returnerade dataströmmen tooa sträng.</span><span class="sxs-lookup"><span data-stu-id="425d3-158">However, you can use hello [stream\_get\_contents][stream-get-contents] function tooconvert hello returned stream tooa string.</span></span>

## <a name="delete-a-blob"></a><span data-ttu-id="425d3-159">Ta bort en blob</span><span class="sxs-lookup"><span data-stu-id="425d3-159">Delete a blob</span></span>
<span data-ttu-id="425d3-160">toodelete blob klara hello behållarens namn och blobbnamnet för**BlobRestProxy -> deleteBlob**.</span><span class="sxs-lookup"><span data-stu-id="425d3-160">toodelete a blob, pass hello container name and blob name too**BlobRestProxy->deleteBlob**.</span></span>

```php
require_once 'vendor/autoload.php';

use WindowsAzure\Common\ServicesBuilder;
use MicrosoftAzure\Storage\Common\ServiceException;

// Create blob REST proxy.
$blobRestProxy = ServicesBuilder::getInstance()->createBlobService($connectionString);


try    {
    // Delete blob.
    $blobRestProxy->deleteBlob("mycontainer", "myblob");
}
catch(ServiceException $e){
    // Handle exception based on error codes and messages.
    // Error codes and messages are here:
    // http://msdn.microsoft.com/library/azure/dd179439.aspx
    $code = $e->getCode();
    $error_message = $e->getMessage();
    echo $code.": ".$error_message."<br />";
}
```

## <a name="delete-a-blob-container"></a><span data-ttu-id="425d3-161">Ta bort en blob-behållare</span><span class="sxs-lookup"><span data-stu-id="425d3-161">Delete a blob container</span></span>
<span data-ttu-id="425d3-162">Slutligen toodelete blob-behållaren skicka hello behållarnamn för**BlobRestProxy -> deleteContainer**.</span><span class="sxs-lookup"><span data-stu-id="425d3-162">Finally, toodelete a blob container, pass hello container name too**BlobRestProxy->deleteContainer**.</span></span>

```php
require_once 'vendor/autoload.php';

use WindowsAzure\Common\ServicesBuilder;
use MicrosoftAzure\Storage\Common\ServiceException;

// Create blob REST proxy.
$blobRestProxy = ServicesBuilder::getInstance()->createBlobService($connectionString);

try    {
    // Delete container.
    $blobRestProxy->deleteContainer("mycontainer");
}
catch(ServiceException $e){
    // Handle exception based on error codes and messages.
    // Error codes and messages are here:
    // http://msdn.microsoft.com/library/azure/dd179439.aspx
    $code = $e->getCode();
    $error_message = $e->getMessage();
    echo $code.": ".$error_message."<br />";
}
```

## <a name="next-steps"></a><span data-ttu-id="425d3-163">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="425d3-163">Next steps</span></span>
<span data-ttu-id="425d3-164">Nu när du har lärt dig grunderna hello i hello Azure blob-tjänsten ska du följa dessa länkar toolearn om mer komplexa lagringsuppgifter.</span><span class="sxs-lookup"><span data-stu-id="425d3-164">Now that you've learned hello basics of hello Azure blob service, follow these links toolearn about more complex storage tasks.</span></span>

* <span data-ttu-id="425d3-165">Besök hello [Azure Storage-teamets blogg](http://blogs.msdn.com/b/windowsazurestorage/)</span><span class="sxs-lookup"><span data-stu-id="425d3-165">Visit hello [Azure Storage team blog](http://blogs.msdn.com/b/windowsazurestorage/)</span></span>
* <span data-ttu-id="425d3-166">Se hello [PHP block-blob exempel](https://github.com/WindowsAzure/azure-sdk-for-php-samples/blob/master/storage/BlockBlobExample.php).</span><span class="sxs-lookup"><span data-stu-id="425d3-166">See hello [PHP block blob example](https://github.com/WindowsAzure/azure-sdk-for-php-samples/blob/master/storage/BlockBlobExample.php).</span></span>
* <span data-ttu-id="425d3-167">Se hello [PHP sidan blob exempel](https://github.com/WindowsAzure/azure-sdk-for-php-samples/blob/master/storage/PageBlobExample.php).</span><span class="sxs-lookup"><span data-stu-id="425d3-167">See hello [PHP page blob example](https://github.com/WindowsAzure/azure-sdk-for-php-samples/blob/master/storage/PageBlobExample.php).</span></span>
* [<span data-ttu-id="425d3-168">Överföra data med kommandoradsverktyget Azcopy hello</span><span class="sxs-lookup"><span data-stu-id="425d3-168">Transfer data with hello AzCopy Command-Line Utility</span></span>](../common/storage-use-azcopy.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json)

<span data-ttu-id="425d3-169">Mer information finns också hello [PHP Developer Center](/develop/php/).</span><span class="sxs-lookup"><span data-stu-id="425d3-169">For more information, see also hello [PHP Developer Center](/develop/php/).</span></span>

[download]: http://go.microsoft.com/fwlink/?LinkID=252473
[container-acl]: http://msdn.microsoft.com/library/azure/dd179391.aspx
[error-codes]: http://msdn.microsoft.com/library/azure/dd179439.aspx
[file_get_contents]: http://php.net/file_get_contents
[require_once]: http://php.net/require_once
[fopen]: http://www.php.net/fopen
[stream-get-contents]: http://www.php.net/stream_get_contents
