---
title: "Använda blob storage (objektlagring) från PHP | Microsoft Docs"
description: Lagra ostrukturerade data i molnet med Azure Blob Storage (objektlagring).
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
ms.openlocfilehash: 2c356d7faafa8ef4591087b5b1f949b9374732be
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="how-to-use-blob-storage-from-php"></a><span data-ttu-id="c805d-103">Använda blob storage från PHP</span><span class="sxs-lookup"><span data-stu-id="c805d-103">How to use blob storage from PHP</span></span>
[!INCLUDE [storage-selector-blob-include](../../includes/storage-selector-blob-include.md)]

[!INCLUDE [storage-try-azure-tools-queues](../../includes/storage-try-azure-tools-blobs.md)]

## <a name="overview"></a><span data-ttu-id="c805d-104">Översikt</span><span class="sxs-lookup"><span data-stu-id="c805d-104">Overview</span></span>
<span data-ttu-id="c805d-105">Azure Blob Storage är en tjänst som lagrar ostrukturerade data i molnet som objekt/blobbar.</span><span class="sxs-lookup"><span data-stu-id="c805d-105">Azure Blob storage is a service that stores unstructured data in the cloud as objects/blobs.</span></span> <span data-ttu-id="c805d-106">Blob Storage kan lagra alla slags textdata eller binära data, till exempel ett dokument, en mediefil eller ett installationsprogram.</span><span class="sxs-lookup"><span data-stu-id="c805d-106">Blob storage can store any type of text or binary data, such as a document, media file, or application installer.</span></span> <span data-ttu-id="c805d-107">Blob Storage kallas även för objektlagring.</span><span class="sxs-lookup"><span data-stu-id="c805d-107">Blob storage is also referred to as object storage.</span></span>

<span data-ttu-id="c805d-108">Den här guiden visar hur du utför vanliga scenarier med hjälp av Azure blob-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="c805d-108">This guide shows you how to perform common scenarios using the Azure blob service.</span></span> <span data-ttu-id="c805d-109">Exemplen är skrivna i PHP och Använd den [Azure SDK för PHP][download].</span><span class="sxs-lookup"><span data-stu-id="c805d-109">The samples are written in PHP and use the [Azure SDK for PHP][download].</span></span> <span data-ttu-id="c805d-110">Scenarier som tas upp inkluderar **överför**, **lista**, **hämtar**, och **bort** blobbar.</span><span class="sxs-lookup"><span data-stu-id="c805d-110">The scenarios covered include **uploading**, **listing**, **downloading**, and **deleting** blobs.</span></span> <span data-ttu-id="c805d-111">Mer information om blobbar finns i [nästa steg](#next-steps) avsnitt.</span><span class="sxs-lookup"><span data-stu-id="c805d-111">For more information on blobs, see the [Next steps](#next-steps) section.</span></span>

[!INCLUDE [storage-blob-concepts-include](../../includes/storage-blob-concepts-include.md)]

[!INCLUDE [storage-create-account-include](../../includes/storage-create-account-include.md)]

## <a name="create-a-php-application"></a><span data-ttu-id="c805d-112">Skapa en PHP-program</span><span class="sxs-lookup"><span data-stu-id="c805d-112">Create a PHP application</span></span>
<span data-ttu-id="c805d-113">Det enda kravet för att skapa en PHP-program som använder Azure blob-tjänsten är den refererande klasser i Azure SDK för PHP från inom din kod.</span><span class="sxs-lookup"><span data-stu-id="c805d-113">The only requirement for creating a PHP application that accesses the Azure blob service is the referencing of classes in the Azure SDK for PHP from within your code.</span></span> <span data-ttu-id="c805d-114">Du kan använda alla utvecklingsverktyg för att skapa programmet, inklusive anteckningar.</span><span class="sxs-lookup"><span data-stu-id="c805d-114">You can use any development tools to create your application, including Notepad.</span></span>

<span data-ttu-id="c805d-115">I den här guiden kan du använda tjänstens funktioner som kan anropas inom ett PHP-program lokalt eller i koden körs i en Azure-webbroll, en arbetsroll eller en webbplats.</span><span class="sxs-lookup"><span data-stu-id="c805d-115">In this guide, you use service features, which can be called within a PHP application locally or in code running within an Azure web role, worker role, or website.</span></span>

## <a name="get-the-azure-client-libraries"></a><span data-ttu-id="c805d-116">Hämta Azures klientbibliotek</span><span class="sxs-lookup"><span data-stu-id="c805d-116">Get the Azure Client Libraries</span></span>
[!INCLUDE [get-client-libraries](../../includes/get-client-libraries.md)]

## <a name="configure-your-application-to-access-the-blob-service"></a><span data-ttu-id="c805d-117">Konfigurera ditt program för åtkomst till blob-tjänst</span><span class="sxs-lookup"><span data-stu-id="c805d-117">Configure your application to access the blob service</span></span>
<span data-ttu-id="c805d-118">Om du vill använda Azure blob-tjänsten API: er, måste du:</span><span class="sxs-lookup"><span data-stu-id="c805d-118">To use the Azure blob service APIs, you need to:</span></span>

1. <span data-ttu-id="c805d-119">Referera till den automatiska bandladdaren filen med hjälp av den [require_once] -instruktionen och</span><span class="sxs-lookup"><span data-stu-id="c805d-119">Reference the autoloader file using the [require_once] statement, and</span></span>
2. <span data-ttu-id="c805d-120">Referera till alla klasser som du kan använda.</span><span class="sxs-lookup"><span data-stu-id="c805d-120">Reference any classes you might use.</span></span>

<span data-ttu-id="c805d-121">I följande exempel visas hur du lägger till den automatiska bandladdaren fil- och referens av **ServicesBuilder** klass.</span><span class="sxs-lookup"><span data-stu-id="c805d-121">The following example shows how to include the autoloader file and reference the **ServicesBuilder** class.</span></span>

> [!NOTE]
> <span data-ttu-id="c805d-122">Exemplen i den här artikeln förutsätter att du har installerat PHP-klientbibliotek för Azure via Composer.</span><span class="sxs-lookup"><span data-stu-id="c805d-122">The examples in this article assume you have installed the PHP Client Libraries for Azure via Composer.</span></span> <span data-ttu-id="c805d-123">Om du har installerat biblioteken manuellt, måste du referera till den `WindowsAzure.php` bandladdaren filen.</span><span class="sxs-lookup"><span data-stu-id="c805d-123">If you installed the libraries manually, you need to reference the `WindowsAzure.php` autoloader file.</span></span>
>
>

```php
require_once 'vendor/autoload.php';
use WindowsAzure\Common\ServicesBuilder;
```

<span data-ttu-id="c805d-124">I exemplen nedan, den `require_once` instruktionen visas alltid, men endast klasserna som krävs för att köra refereras.</span><span class="sxs-lookup"><span data-stu-id="c805d-124">In the examples below, the `require_once` statement will be shown always, but only the classes necessary for the example to execute are referenced.</span></span>

## <a name="set-up-an-azure-storage-connection"></a><span data-ttu-id="c805d-125">Skapa en Azure storage-anslutning</span><span class="sxs-lookup"><span data-stu-id="c805d-125">Set up an Azure storage connection</span></span>
<span data-ttu-id="c805d-126">Du måste ha en giltig anslutningssträng för att initiera klienten ett Azure blob-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="c805d-126">To instantiate an Azure blob service client, you must first have a valid connection string.</span></span> <span data-ttu-id="c805d-127">Formatet för anslutningssträngen för blob-tjänsten är:</span><span class="sxs-lookup"><span data-stu-id="c805d-127">The format for the blob service connection string is:</span></span>

<span data-ttu-id="c805d-128">För att komma åt en live-tjänst:</span><span class="sxs-lookup"><span data-stu-id="c805d-128">For accessing a live service:</span></span>

```php
DefaultEndpointsProtocol=[http|https];AccountName=[yourAccount];AccountKey=[yourKey]
```

<span data-ttu-id="c805d-129">För att komma åt storage-emulatorn:</span><span class="sxs-lookup"><span data-stu-id="c805d-129">For accessing the storage emulator:</span></span>

```php
UseDevelopmentStorage=true
```

<span data-ttu-id="c805d-130">För att skapa någon Azure-tjänst-klient, måste du använda den **ServicesBuilder** klass.</span><span class="sxs-lookup"><span data-stu-id="c805d-130">To create any Azure service client, you need to use the **ServicesBuilder** class.</span></span> <span data-ttu-id="c805d-131">Du kan:</span><span class="sxs-lookup"><span data-stu-id="c805d-131">You can:</span></span>

* <span data-ttu-id="c805d-132">Skicka anslutningssträngen till den eller</span><span class="sxs-lookup"><span data-stu-id="c805d-132">Pass the connection string directly to it or</span></span>
* <span data-ttu-id="c805d-133">Använd den **CloudConfigurationManager (CCM)** till flera externa källor för anslutningssträngen:</span><span class="sxs-lookup"><span data-stu-id="c805d-133">Use the **CloudConfigurationManager (CCM)** to check multiple external sources for the connection string:</span></span>
  * <span data-ttu-id="c805d-134">Som standard levereras den med stöd för en extern källa - miljövariabler.</span><span class="sxs-lookup"><span data-stu-id="c805d-134">By default, it comes with support for one external source - environmental variables.</span></span>
  * <span data-ttu-id="c805d-135">Du kan lägga till nya källor genom att utöka den **ConnectionStringSource** klass.</span><span class="sxs-lookup"><span data-stu-id="c805d-135">You can add new sources by extending the **ConnectionStringSource** class.</span></span>

<span data-ttu-id="c805d-136">Exempel som beskrivs här skickas anslutningssträngen direkt.</span><span class="sxs-lookup"><span data-stu-id="c805d-136">For the examples outlined here, the connection string will be passed directly.</span></span>

```php
require_once 'vendor/autoload.php';

use WindowsAzure\Common\ServicesBuilder;

$blobRestProxy = ServicesBuilder::getInstance()->createBlobService($connectionString);
```

## <a name="create-a-container"></a><span data-ttu-id="c805d-137">Skapa en behållare</span><span class="sxs-lookup"><span data-stu-id="c805d-137">Create a container</span></span>
[!INCLUDE [storage-container-naming-rules-include](../../includes/storage-container-naming-rules-include.md)]

<span data-ttu-id="c805d-138">En **BlobRestProxy** objekt kan du skapa en blob-behållare med den **createContainer** metod.</span><span class="sxs-lookup"><span data-stu-id="c805d-138">A **BlobRestProxy** object lets you create a blob container with the **createContainer** method.</span></span> <span data-ttu-id="c805d-139">När du skapar en behållare, du kan ange alternativ för behållaren, men detta så krävs inte.</span><span class="sxs-lookup"><span data-stu-id="c805d-139">When creating a container, you can set options on the container, but doing so is not required.</span></span> <span data-ttu-id="c805d-140">(I exemplet nedan visar hur du ställer in behållarens åtkomstkontrollistan (ACL) och metadata för behållaren.)</span><span class="sxs-lookup"><span data-stu-id="c805d-140">(The example below shows how to set the container access control list (ACL) and container metadata.)</span></span>

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
// proxys can enumerate blobs within the container via anonymous
// request, but cannot enumerate containers within the storage account.
//
// BLOBS_ONLY:
// Specifies public read access for blobs. Blob data within this
// container can be read via anonymous request, but container data is not
// available. proxys cannot enumerate blobs within the container via
// anonymous request.
// If this value is not specified in the request, container data is
// private to the account owner.
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

<span data-ttu-id="c805d-141">Anropar **setPublicAccess (PublicAccessType::CONTAINER\_och\_BLOBBAR)** gör behållare och blob-data tillgängliga via anonyma förfrågningar.</span><span class="sxs-lookup"><span data-stu-id="c805d-141">Calling **setPublicAccess(PublicAccessType::CONTAINER\_AND\_BLOBS)** makes the container and blob data accessible via anonymous requests.</span></span> <span data-ttu-id="c805d-142">Anropar **setPublicAccess(PublicAccessType::BLOBS_ONLY)** gör endast blob-data som är tillgänglig via anonyma förfrågningar.</span><span class="sxs-lookup"><span data-stu-id="c805d-142">Calling **setPublicAccess(PublicAccessType::BLOBS_ONLY)** makes only blob data accessible via anonymous requests.</span></span> <span data-ttu-id="c805d-143">Mer information om behållaren ACL: er finns [Set behållar-ACL (REST-API)][container-acl].</span><span class="sxs-lookup"><span data-stu-id="c805d-143">For more information about container ACLs, see [Set container ACL (REST API)][container-acl].</span></span>

<span data-ttu-id="c805d-144">Mer information om felkoder för Blob-tjänsten finns [Blob felkoder][error-codes].</span><span class="sxs-lookup"><span data-stu-id="c805d-144">For more information about Blob service error codes, see [Blob Service Error Codes][error-codes].</span></span>

## <a name="upload-a-blob-into-a-container"></a><span data-ttu-id="c805d-145">Ladda upp en blobb till en behållare</span><span class="sxs-lookup"><span data-stu-id="c805d-145">Upload a blob into a container</span></span>
<span data-ttu-id="c805d-146">Om du vill överföra en fil som en blob, Använd den **BlobRestProxy -> createBlockBlob** metod.</span><span class="sxs-lookup"><span data-stu-id="c805d-146">To upload a file as a blob, use the **BlobRestProxy->createBlockBlob** method.</span></span> <span data-ttu-id="c805d-147">Den här åtgärden skapas blobben om den finns inte eller skrivs över om det inte.</span><span class="sxs-lookup"><span data-stu-id="c805d-147">This operation creates the blob if it doesn't exist, or overwrites it if it does.</span></span> <span data-ttu-id="c805d-148">Exemplet nedan förutsätter att behållaren redan har skapats och använder [fopen] [ fopen] att öppna filen som en dataström.</span><span class="sxs-lookup"><span data-stu-id="c805d-148">The code example below assumes that the container has already been created and uses [fopen][fopen] to open the file as a stream.</span></span>

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

<span data-ttu-id="c805d-149">Observera att föregående exempel laddar upp en blob som en dataström.</span><span class="sxs-lookup"><span data-stu-id="c805d-149">Note that the previous sample uploads a blob as a stream.</span></span> <span data-ttu-id="c805d-150">Men en blob kan också överföras som en sträng med, till exempel den [filen\_hämta\_innehållet] [ file_get_contents] funktion.</span><span class="sxs-lookup"><span data-stu-id="c805d-150">However, a blob can also be uploaded as a string using, for example, the [file\_get\_contents][file_get_contents] function.</span></span> <span data-ttu-id="c805d-151">Om du vill göra detta med hjälp av föregående exempel, ändra `$content = fopen("c:\myfile.txt", "r");` till `$content = file_get_contents("c:\myfile.txt");`.</span><span class="sxs-lookup"><span data-stu-id="c805d-151">To do this using the previous sample, change `$content = fopen("c:\myfile.txt", "r");` to `$content = file_get_contents("c:\myfile.txt");`.</span></span>

## <a name="list-the-blobs-in-a-container"></a><span data-ttu-id="c805d-152">Visa en lista över blobbarna i en behållare</span><span class="sxs-lookup"><span data-stu-id="c805d-152">List the blobs in a container</span></span>
<span data-ttu-id="c805d-153">Om du vill visa blobbar i en behållare använder den **BlobRestProxy -> listBlobs** metod med en **foreach** loop loop via resultatet.</span><span class="sxs-lookup"><span data-stu-id="c805d-153">To list the blobs in a container, use the **BlobRestProxy->listBlobs** method with a **foreach** loop to loop through the result.</span></span> <span data-ttu-id="c805d-154">Följande kod visar namnet på varje blob som utdata i en behållare och visar dess URI till webbläsaren.</span><span class="sxs-lookup"><span data-stu-id="c805d-154">The following code displays the name of each blob as output in a container and displays its URI to the browser.</span></span>

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

## <a name="download-a-blob"></a><span data-ttu-id="c805d-155">Ladda ned en blob</span><span class="sxs-lookup"><span data-stu-id="c805d-155">Download a blob</span></span>
<span data-ttu-id="c805d-156">Om du vill hämta en blob anropa den **BlobRestProxy -> getBlob** metoden anropar den **getContentStream** -metoden i den resulterande **GetBlobResult** objekt.</span><span class="sxs-lookup"><span data-stu-id="c805d-156">To download a blob, call the **BlobRestProxy->getBlob** method, then call the **getContentStream** method on the resulting **GetBlobResult** object.</span></span>

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

<span data-ttu-id="c805d-157">Observera att exemplet ovan hämtar en blob som en dataström-resurs (standardinställning).</span><span class="sxs-lookup"><span data-stu-id="c805d-157">Note that the example above gets a blob as a stream resource (the default behavior).</span></span> <span data-ttu-id="c805d-158">Du kan använda den [dataströmmen\_hämta\_innehållet] [ stream-get-contents] för att konvertera den returnerade strömmen till en sträng.</span><span class="sxs-lookup"><span data-stu-id="c805d-158">However, you can use the [stream\_get\_contents][stream-get-contents] function to convert the returned stream to a string.</span></span>

## <a name="delete-a-blob"></a><span data-ttu-id="c805d-159">Ta bort en blob</span><span class="sxs-lookup"><span data-stu-id="c805d-159">Delete a blob</span></span>
<span data-ttu-id="c805d-160">Om du vill ta bort en blobb skicka behållarens namn och blobbnamnet till **BlobRestProxy -> deleteBlob**.</span><span class="sxs-lookup"><span data-stu-id="c805d-160">To delete a blob, pass the container name and blob name to **BlobRestProxy->deleteBlob**.</span></span>

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

## <a name="delete-a-blob-container"></a><span data-ttu-id="c805d-161">Ta bort en blob-behållare</span><span class="sxs-lookup"><span data-stu-id="c805d-161">Delete a blob container</span></span>
<span data-ttu-id="c805d-162">Slutligen skicka behållarens namn till för att ta bort en blob-behållare, **BlobRestProxy -> deleteContainer**.</span><span class="sxs-lookup"><span data-stu-id="c805d-162">Finally, to delete a blob container, pass the container name to **BlobRestProxy->deleteContainer**.</span></span>

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

## <a name="next-steps"></a><span data-ttu-id="c805d-163">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="c805d-163">Next steps</span></span>
<span data-ttu-id="c805d-164">Nu när du har lärt dig grunderna i Azure blob-tjänsten följa dessa länkar för att lära dig mer komplexa lagringsuppgifter.</span><span class="sxs-lookup"><span data-stu-id="c805d-164">Now that you've learned the basics of the Azure blob service, follow these links to learn about more complex storage tasks.</span></span>

* <span data-ttu-id="c805d-165">Besök den [Azure Storage-teamets blogg](http://blogs.msdn.com/b/windowsazurestorage/)</span><span class="sxs-lookup"><span data-stu-id="c805d-165">Visit the [Azure Storage team blog](http://blogs.msdn.com/b/windowsazurestorage/)</span></span>
* <span data-ttu-id="c805d-166">Finns det [PHP block-blob exempel](https://github.com/WindowsAzure/azure-sdk-for-php-samples/blob/master/storage/BlockBlobExample.php).</span><span class="sxs-lookup"><span data-stu-id="c805d-166">See the [PHP block blob example](https://github.com/WindowsAzure/azure-sdk-for-php-samples/blob/master/storage/BlockBlobExample.php).</span></span>
* <span data-ttu-id="c805d-167">Finns det [PHP sidan blob exempel](https://github.com/WindowsAzure/azure-sdk-for-php-samples/blob/master/storage/PageBlobExample.php).</span><span class="sxs-lookup"><span data-stu-id="c805d-167">See the [PHP page blob example](https://github.com/WindowsAzure/azure-sdk-for-php-samples/blob/master/storage/PageBlobExample.php).</span></span>
* [<span data-ttu-id="c805d-168">Överföra data med kommandoradsverktyget AzCopy</span><span class="sxs-lookup"><span data-stu-id="c805d-168">Transfer data with the AzCopy Command-Line Utility</span></span>](storage-use-azcopy.md)

<span data-ttu-id="c805d-169">Mer information finns också i [PHP Developer Center](/develop/php/).</span><span class="sxs-lookup"><span data-stu-id="c805d-169">For more information, see also the [PHP Developer Center](/develop/php/).</span></span>

[download]: http://go.microsoft.com/fwlink/?LinkID=252473
[container-acl]: http://msdn.microsoft.com/library/azure/dd179391.aspx
[error-codes]: http://msdn.microsoft.com/library/azure/dd179439.aspx
[file_get_contents]: http://php.net/file_get_contents
<span data-ttu-id="c805d-170">[require_once]: http://php.net/require_once</span><span class="sxs-lookup"><span data-stu-id="c805d-170">[require_once]: http://php.net/require_once</span></span>
[fopen]: http://www.php.net/fopen
[stream-get-contents]: http://www.php.net/stream_get_contents
