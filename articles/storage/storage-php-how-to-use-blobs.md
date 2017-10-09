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
ms.openlocfilehash: 331405e583c17c4f71acacdc0078b2bc71efbef0
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="how-toouse-blob-storage-from-php"></a>Hur toouse blob storage från PHP
[!INCLUDE [storage-selector-blob-include](../../includes/storage-selector-blob-include.md)]

[!INCLUDE [storage-try-azure-tools-queues](../../includes/storage-try-azure-tools-blobs.md)]

## <a name="overview"></a>Översikt
Azure Blob storage är en tjänst som lagrar Ostrukturerade data i hello molnet som objekt/blobbar. Blob Storage kan lagra alla slags textdata eller binära data, till exempel ett dokument, en mediefil eller ett installationsprogram. BLOB storage är också hänvisade tooas objektlagring.

Den här guiden visar hur tooperform vanliga scenarier med hjälp av hello Azure blob-tjänst. hello exemplen är skrivna i PHP och använder hello [Azure SDK för PHP][download]. hello beskrivs scenarier där **överför**, **lista**, **hämtar**, och **bort** blobbar. Mer information om BLOB finns hello [nästa steg](#next-steps) avsnitt.

[!INCLUDE [storage-blob-concepts-include](../../includes/storage-blob-concepts-include.md)]

[!INCLUDE [storage-create-account-include](../../includes/storage-create-account-include.md)]

## <a name="create-a-php-application"></a>Skapa en PHP-program
Hej krav för att skapa en PHP-program som ansluter till hello Azure blob-tjänsten är hello refererar till klasser i hello Azure SDK för PHP från inom din kod. Du kan använda alla development tools toocreate ditt program, inklusive anteckningar.

I den här guiden kan du använda tjänstens funktioner som kan anropas inom ett PHP-program lokalt eller i koden körs i en Azure-webbroll, en arbetsroll eller en webbplats.

## <a name="get-hello-azure-client-libraries"></a>Hämta hello Azure-klientbibliotek
[!INCLUDE [get-client-libraries](../../includes/get-client-libraries.md)]

## <a name="configure-your-application-tooaccess-hello-blob-service"></a>Konfigurera ditt program tooaccess hello blob-tjänsten
toouse hello Azure blob-tjänsten API: er, måste du:

1. Referens hello bandladdaren fil med hello [require_once] -instruktionen och
2. Referera till alla klasser som du kan använda.

hello följande exempel visas hur tooinclude hello bandladdaren fil- och hello **ServicesBuilder** klass.

> [!NOTE]
> hello exemplen i den här artikeln förutsätter att du har installerat hello PHP-klientbibliotek för Azure via Composer. Om du har installerat hello bibliotek manuellt måste tooreference hello `WindowsAzure.php` bandladdaren filen.
>
>

```php
require_once 'vendor/autoload.php';
use WindowsAzure\Common\ServicesBuilder;
```

I hello exemplen nedan hello `require_once` instruktionen visas alltid, men bara hello klasser krävs för hello exempel tooexecute refererar till.

## <a name="set-up-an-azure-storage-connection"></a>Skapa en Azure storage-anslutning
tooinstantiate klienten ett Azure blob-tjänsten måste du först ha en giltig anslutningssträng. hello-formatet för anslutningssträngen för hello blob-tjänsten är:

För att komma åt en live-tjänst:

```php
DefaultEndpointsProtocol=[http|https];AccountName=[yourAccount];AccountKey=[yourKey]
```

För att komma åt hello storage-emulatorn:

```php
UseDevelopmentStorage=true
```

toocreate alla Azure-tjänst-klienter, behöver du toouse hello **ServicesBuilder** klass. Du kan:

* Skicka hello anslutning direkt string tooit eller
* Använd hello **CloudConfigurationManager (CCM)** toocheck flera externa datakällor för hello anslutningssträngen:
  * Som standard levereras den med stöd för en extern källa - miljövariabler.
  * Du kan lägga till nya källor genom att utöka hello **ConnectionStringSource** klass.

Hello-exempel som beskrivs här skickas hello anslutningssträngen direkt.

```php
require_once 'vendor/autoload.php';

use WindowsAzure\Common\ServicesBuilder;

$blobRestProxy = ServicesBuilder::getInstance()->createBlobService($connectionString);
```

## <a name="create-a-container"></a>Skapa en behållare
[!INCLUDE [storage-container-naming-rules-include](../../includes/storage-container-naming-rules-include.md)]

En **BlobRestProxy** objekt kan du skapa en blob-behållare med hello **createContainer** metod. När du skapar en behållare, du kan ange alternativ för hello behållare, men detta så krävs inte. (hello exemplet nedan visar hur tooset hello behållaren åt åtkomstkontrollistor (ACL) och metadata för behållaren.)

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

Anropar **setPublicAccess (PublicAccessType::CONTAINER\_och\_BLOBBAR)** gör hello-behållaren och blob data som är tillgänglig via anonyma förfrågningar. Anropar **setPublicAccess(PublicAccessType::BLOBS_ONLY)** gör endast blob-data som är tillgänglig via anonyma förfrågningar. Mer information om behållaren ACL: er finns [Set behållar-ACL (REST-API)][container-acl].

Mer information om felkoder för Blob-tjänsten finns [Blob felkoder][error-codes].

## <a name="upload-a-blob-into-a-container"></a>Ladda upp en blobb till en behållare
tooupload en fil som en blob, Använd hello **BlobRestProxy -> createBlockBlob** metod. Den här åtgärden skapar hello blob om det inte finns, eller skrivs över om den finns. hello kodexemplet nedan förutsätter hello behållaren redan har skapats och använder [fopen] [ fopen] tooopen hello-filen som en dataström.

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

Observera att hello föregående exempel laddar upp en blob som en dataström. En blob kan dock också överföras som en sträng med till exempel hello [filen\_hämta\_innehållet] [ file_get_contents] funktion. toodo denna med hello föregående exempel kan ändra `$content = fopen("c:\myfile.txt", "r");` för`$content = file_get_contents("c:\myfile.txt");`.

## <a name="list-hello-blobs-in-a-container"></a>Lista hello blobbar i en behållare
toolist hello blobbar i en behållare använder hello **BlobRestProxy -> listBlobs** metod med en **foreach** slinga tooloop via hello resultat. hello följande kod visar hello namnet på varje blob som utdata i en behållare och visar dess URI toohello webbläsare.

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
toodownload blob anropet hello **BlobRestProxy -> getBlob** metod och sedan anropa hello **getContentStream** metod på hello resulterande **GetBlobResult** objekt.

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

Observera att hello-exemplet ovan hämtar en blob som en dataström-resurs (hello standardbeteendet). Du kan dock använda hello [dataströmmen\_hämta\_innehållet] [ stream-get-contents] funktionen tooconvert hello returnerade dataströmmen tooa sträng.

## <a name="delete-a-blob"></a>Ta bort en blob
toodelete blob klara hello behållarens namn och blobbnamnet för**BlobRestProxy -> deleteBlob**.

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
Slutligen toodelete blob-behållaren skicka hello behållarnamn för**BlobRestProxy -> deleteContainer**.

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
Nu när du har lärt dig grunderna hello i hello Azure blob-tjänsten ska du följa dessa länkar toolearn om mer komplexa lagringsuppgifter.

* Besök hello [Azure Storage-teamets blogg](http://blogs.msdn.com/b/windowsazurestorage/)
* Se hello [PHP block-blob exempel](https://github.com/WindowsAzure/azure-sdk-for-php-samples/blob/master/storage/BlockBlobExample.php).
* Se hello [PHP sidan blob exempel](https://github.com/WindowsAzure/azure-sdk-for-php-samples/blob/master/storage/PageBlobExample.php).
* [Överföra data med kommandoradsverktyget Azcopy hello](storage-use-azcopy.md)

Mer information finns också hello [PHP Developer Center](/develop/php/).

[download]: http://go.microsoft.com/fwlink/?LinkID=252473
[container-acl]: http://msdn.microsoft.com/library/azure/dd179391.aspx
[error-codes]: http://msdn.microsoft.com/library/azure/dd179439.aspx
[file_get_contents]: http://php.net/file_get_contents
[require_once]: http://php.net/require_once
[fopen]: http://www.php.net/fopen
[stream-get-contents]: http://www.php.net/stream_get_contents
