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
ms.openlocfilehash: 4b68844c5d0553eaede3997bf09bff4fe570e850
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/29/2017
---
# <a name="how-to-use-blob-storage-from-php"></a>Använda blob storage från PHP
[!INCLUDE [storage-selector-blob-include](../../../includes/storage-selector-blob-include.md)]

[!INCLUDE [storage-try-azure-tools-queues](../../../includes/storage-try-azure-tools-blobs.md)]

## <a name="overview"></a>Översikt
Azure Blob Storage är en tjänst som lagrar ostrukturerade data i molnet som objekt/blobbar. Blob Storage kan lagra alla slags textdata eller binära data, till exempel ett dokument, en mediefil eller ett installationsprogram. Blob Storage kallas även för objektlagring.

Den här guiden visar hur du utför vanliga scenarier med hjälp av Azure blob-tjänsten. Exemplen är skrivna i PHP och Använd den [Azure SDK för PHP][download]. Scenarier som tas upp inkluderar **överför**, **lista**, **hämtar**, och **bort** blobbar. Mer information om blobbar finns i [nästa steg](#next-steps) avsnitt.

[!INCLUDE [storage-blob-concepts-include](../../../includes/storage-blob-concepts-include.md)]

[!INCLUDE [storage-create-account-include](../../../includes/storage-create-account-include.md)]

## <a name="create-a-php-application"></a>Skapa en PHP-program
Det enda kravet för att skapa en PHP-program som använder Azure blob-tjänsten är den refererande klasser i Azure SDK för PHP från inom din kod. Du kan använda alla utvecklingsverktyg för att skapa programmet, inklusive anteckningar.

I den här guiden kan du använda tjänstens funktioner som kan anropas inom ett PHP-program lokalt eller i koden körs i en Azure-webbroll, en arbetsroll eller en webbplats.

## <a name="get-the-azure-client-libraries"></a>Hämta Azures klientbibliotek
[!INCLUDE [get-client-libraries](../../../includes/get-client-libraries.md)]

## <a name="configure-your-application-to-access-the-blob-service"></a>Konfigurera ditt program för åtkomst till blob-tjänst
Om du vill använda Azure blob-tjänsten API: er, måste du:

1. Referera till den automatiska bandladdaren filen med hjälp av den [require_once] -instruktionen och
2. Referera till alla klasser som du kan använda.

I följande exempel visas hur du lägger till den automatiska bandladdaren fil- och referens av **ServicesBuilder** klass.

> [!NOTE]
> Exemplen i den här artikeln förutsätter att du har installerat PHP-klientbibliotek för Azure via Composer. Om du har installerat biblioteken manuellt, måste du referera till den `WindowsAzure.php` bandladdaren filen.
>
>

```php
require_once 'vendor/autoload.php';
use WindowsAzure\Common\ServicesBuilder;
```

I exemplen nedan, den `require_once` instruktionen visas alltid, men endast klasserna som krävs för att köra refereras.

## <a name="set-up-an-azure-storage-connection"></a>Skapa en Azure storage-anslutning
Du måste ha en giltig anslutningssträng för att initiera klienten ett Azure blob-tjänsten. Formatet för anslutningssträngen för blob-tjänsten är:

För att komma åt en live-tjänst:

```php
DefaultEndpointsProtocol=[http|https];AccountName=[yourAccount];AccountKey=[yourKey]
```

För att komma åt storage-emulatorn:

```php
UseDevelopmentStorage=true
```

För att skapa någon Azure-tjänst-klient, måste du använda den **ServicesBuilder** klass. Du kan:

* Skicka anslutningssträngen till den eller
* Använd den **CloudConfigurationManager (CCM)** till flera externa källor för anslutningssträngen:
  * Som standard levereras den med stöd för en extern källa - miljövariabler.
  * Du kan lägga till nya källor genom att utöka den **ConnectionStringSource** klass.

Exempel som beskrivs här skickas anslutningssträngen direkt.

```php
require_once 'vendor/autoload.php';

use WindowsAzure\Common\ServicesBuilder;

$blobRestProxy = ServicesBuilder::getInstance()->createBlobService($connectionString);
```

## <a name="create-a-container"></a>Skapa en behållare
[!INCLUDE [storage-container-naming-rules-include](../../../includes/storage-container-naming-rules-include.md)]

En **BlobRestProxy** objekt kan du skapa en blob-behållare med den **createContainer** metod. När du skapar en behållare, du kan ange alternativ för behållaren, men detta så krävs inte. (I exemplet nedan visar hur du ställer in behållarens åtkomstkontrollistan (ACL) och metadata för behållaren.)

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

Anropar **setPublicAccess (PublicAccessType::CONTAINER\_och\_BLOBBAR)** gör behållare och blob-data tillgängliga via anonyma förfrågningar. Anropar **setPublicAccess(PublicAccessType::BLOBS_ONLY)** gör endast blob-data som är tillgänglig via anonyma förfrågningar. Mer information om behållaren ACL: er finns [Set behållar-ACL (REST-API)][container-acl].

Mer information om felkoder för Blob-tjänsten finns [Blob felkoder][error-codes].

## <a name="upload-a-blob-into-a-container"></a>Ladda upp en blobb till en behållare
Om du vill överföra en fil som en blob, Använd den **BlobRestProxy -> createBlockBlob** metod. Den här åtgärden skapas blobben om den finns inte eller skrivs över om det inte. Exemplet nedan förutsätter att behållaren redan har skapats och använder [fopen] [ fopen] att öppna filen som en dataström.

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

Observera att föregående exempel laddar upp en blob som en dataström. Men en blob kan också överföras som en sträng med, till exempel den [filen\_hämta\_innehållet] [ file_get_contents] funktion. Om du vill göra detta med hjälp av föregående exempel, ändra `$content = fopen("c:\myfile.txt", "r");` till `$content = file_get_contents("c:\myfile.txt");`.

## <a name="list-the-blobs-in-a-container"></a>Visa en lista över blobbarna i en behållare
Om du vill visa blobbar i en behållare använder den **BlobRestProxy -> listBlobs** metod med en **foreach** loop loop via resultatet. Följande kod visar namnet på varje blob som utdata i en behållare och visar dess URI till webbläsaren.

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

## <a name="download-a-blob"></a>Ladda ned en blob
Om du vill hämta en blob anropa den **BlobRestProxy -> getBlob** metoden anropar den **getContentStream** -metoden i den resulterande **GetBlobResult** objekt.

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

Observera att exemplet ovan hämtar en blob som en dataström-resurs (standardinställning). Du kan använda den [dataströmmen\_hämta\_innehållet] [ stream-get-contents] för att konvertera den returnerade strömmen till en sträng.

## <a name="delete-a-blob"></a>Ta bort en blob
Om du vill ta bort en blobb skicka behållarens namn och blobbnamnet till **BlobRestProxy -> deleteBlob**.

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

## <a name="delete-a-blob-container"></a>Ta bort en blob-behållare
Slutligen skicka behållarens namn till för att ta bort en blob-behållare, **BlobRestProxy -> deleteContainer**.

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

## <a name="next-steps"></a>Nästa steg
Nu när du har lärt dig grunderna i Azure blob-tjänsten följa dessa länkar för att lära dig mer komplexa lagringsuppgifter.

* Besök den [Azure Storage-teamets blogg](http://blogs.msdn.com/b/windowsazurestorage/)
* Finns det [PHP block-blob exempel](https://github.com/WindowsAzure/azure-sdk-for-php-samples/blob/master/storage/BlockBlobExample.php).
* Finns det [PHP sidan blob exempel](https://github.com/WindowsAzure/azure-sdk-for-php-samples/blob/master/storage/PageBlobExample.php).
* [Överföra data med kommandoradsverktyget AzCopy](../common/storage-use-azcopy.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json)

Mer information finns också i [PHP Developer Center](/develop/php/).

[download]: http://go.microsoft.com/fwlink/?LinkID=252473
[container-acl]: http://msdn.microsoft.com/library/azure/dd179391.aspx
[error-codes]: http://msdn.microsoft.com/library/azure/dd179439.aspx
[file_get_contents]: http://php.net/file_get_contents
[require_once]: http://php.net/require_once
[fopen]: http://www.php.net/fopen
[stream-get-contents]: http://www.php.net/stream_get_contents
