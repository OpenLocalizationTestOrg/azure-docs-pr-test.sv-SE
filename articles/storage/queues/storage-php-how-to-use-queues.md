---
title: "aaaHow toouse Queue storage från PHP | Microsoft Docs"
description: "Lär dig hur toouse hello Azure Queue storage service toocreate och ta bort köer och infoga, hämta och ta bort meddelanden. Exempel är skrivna i PHP."
documentationcenter: php
services: storage
author: robinsh
manager: timlt
editor: tysonn
ms.assetid: 7582b208-4851-4489-a74a-bb952569f55b
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: PHP
ms.topic: article
ms.date: 12/08/2016
ms.author: robinsh
ms.openlocfilehash: 8daabcc9b3b4de121a309f21bb3325242cff06f5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="how-toouse-queue-storage-from-php"></a>Hur toouse Queue storage från PHP
[!INCLUDE [storage-selector-queue-include](../../../includes/storage-selector-queue-include.md)]

[!INCLUDE [storage-try-azure-tools-queues](../../../includes/storage-try-azure-tools-queues.md)]

## <a name="overview"></a>Översikt
Den här guiden visar hur tooperform vanliga scenarier med hjälp av hello Azure Queue storage-tjänst. hello exempel skrivs via klasser från hello Windows SDK för PHP. hello omfattas scenarier som inkluderar infoga, granskning, hämtar, och tar bort Kömeddelanden, samt hur du skapar och tar bort köer.

[!INCLUDE [storage-queue-concepts-include](../../../includes/storage-queue-concepts-include.md)]

[!INCLUDE [storage-create-account-include](../../../includes/storage-create-account-include.md)]

## <a name="create-a-php-application"></a>Skapa en PHP-program
Hej krav för att skapa en PHP-program som har åtkomst till Azure Queue storage är hello refererar till klasser från hello Azure SDK för PHP från inom din kod. Du kan använda alla development tools toocreate ditt program, inklusive anteckningar.

I den här guiden använder kön lagringsfunktioner som kan anropas i en PHP-program lokalt eller i koden körs i en Azure-webbroll, en arbetsroll eller en webbplats.

## <a name="get-hello-azure-client-libraries"></a>Hämta hello Azure-klientbibliotek
[!INCLUDE [get-client-libraries](../../../includes/get-client-libraries.md)]

## <a name="configure-your-application-tooaccess-queue-storage"></a>Konfigurera ditt program tooaccess Queue storage
toouse hello API: er för Azure Queue storage, måste du:

1. Referens hello bandladdaren filen med hjälp av hello [require_once] instruktionen.
2. Referera till alla klasser som du kan använda.

hello följande exempel visas hur tooinclude hello bandladdaren fil- och hello **ServicesBuilder** klass.

> [!NOTE]
> Det här exemplet (och andra exempel i den här artikeln) förutsätter att du har installerat hello PHP-klientbibliotek för Azure via Composer. Om du har installerat hello bibliotek manuellt måste tooreference hello `WindowsAzure.php` bandladdaren filen.
> 
> 

```php
require_once 'vendor/autoload.php';
use WindowsAzure\Common\ServicesBuilder;

```

I hello exemplen nedan hello `require_once` instruktionen visas alltid, men bara hello-klasser som krävs för hello exempel tooexecute kommer att referera till.

## <a name="set-up-an-azure-storage-connection"></a>Skapa en Azure storage-anslutning
tooinstantiate en Azure Queue storage-klient, måste du först ha en giltig anslutningssträng. hello format för hello anslutningssträng för kön är som följer.

För att komma åt en live-tjänst:

```php
DefaultEndpointsProtocol=[http|https];AccountName=[yourAccount];AccountKey=[yourKey]
```

För att komma åt hello emulatorn lagring:

```php
UseDevelopmentStorage=true
```

toocreate alla Azure-tjänst-klienter, behöver du toouse hello **ServicesBuilder** klass. Du kan använda något av följande tekniker hello:

* Skicka hello anslutning direkt string tooit.
* Använd **CloudConfigurationManager (CCM)** toocheck flera externa datakällor för hello anslutningssträngen:
  * Som standard levereras med stöd för en extern källa, miljövariabler.
  * Du kan lägga till nya källor genom att utöka hello **ConnectionStringSource** klass.

Hello-exempel som beskrivs här skickas hello anslutningssträngen direkt.

```php
require_once 'vendor/autoload.php';

use WindowsAzure\Common\ServicesBuilder;

$queueRestProxy = ServicesBuilder::getInstance()->createQueueService($connectionString);
```

## <a name="create-a-queue"></a>Skapa en kö
En **QueueRestProxy** objekt kan du skapa en kö med hjälp av hello **createQueue** metod. När du skapar en kö, du kan ange alternativ för hello kön, men detta så krävs inte. (hello exemplet nedan visar hur tooset metadata i en kö.)

```php
require_once 'vendor/autoload.php';

use WindowsAzure\Common\ServicesBuilder;
use MicrosoftAzure\Storage\Common\ServiceException;
use MicrosoftAzure\Storage\Queue\Models\CreateQueueOptions;

// Create queue REST proxy.
$queueRestProxy = ServicesBuilder::getInstance()->createQueueService($connectionString);

// OPTIONAL: Set queue metadata.
$createQueueOptions = new CreateQueueOptions();
$createQueueOptions->addMetaData("key1", "value1");
$createQueueOptions->addMetaData("key2", "value2");

try    {
    // Create queue.
    $queueRestProxy->createQueue("myqueue", $createQueueOptions);
}
catch(ServiceException $e){
    // Handle exception based on error codes and messages.
    // Error codes and messages are here:
    // http://msdn.microsoft.com/library/azure/dd179446.aspx
    $code = $e->getCode();
    $error_message = $e->getMessage();
    echo $code.": ".$error_message."<br />";
}
```

> [!NOTE]
> Du bör inte förlita dig på skiftlägeskänslighet för metadata nycklar. Alla nycklar läses från hello-tjänsten med små bokstäver.
> 
> 

## <a name="add-a-message-tooa-queue"></a>Lägg till en meddelandekö tooa
tooadd tooa meddelandekö, Använd **QueueRestProxy -> createMessage**. hello-metoden tar hello könamnet hello meddelandetext och meddelandealternativ (som är valfritt).

```php
require_once 'vendor/autoload.php';

use WindowsAzure\Common\ServicesBuilder;
use MicrosoftAzure\Storage\Common\ServiceException;
use MicrosoftAzure\Storage\Queue\Models\CreateMessageOptions;

// Create queue REST proxy.
$queueRestProxy = ServicesBuilder::getInstance()->createQueueService($connectionString);

try    {
    // Create message.
    $builder = new ServicesBuilder();
    $queueRestProxy->createMessage("myqueue", "Hello World!");
}
catch(ServiceException $e){
    // Handle exception based on error codes and messages.
    // Error codes and messages are here:
    // http://msdn.microsoft.com/library/azure/dd179446.aspx
    $code = $e->getCode();
    $error_message = $e->getMessage();
    echo $code.": ".$error_message."<br />";
}
```

## <a name="peek-at-hello-next-message"></a>Granska nästa hello-meddelande
Du kan kika på meddelandet (eller meddelanden) hello fram på en kö utan att ta bort den från hello kö genom att anropa **QueueRestProxy -> peekMessages**. Som standard hello **peekMessage** metoden returnerar ett enda meddelande, men du kan ändra värdet genom att använda hello **PeekMessagesOptions -> setNumberOfMessages** metod.

```php
require_once 'vendor/autoload.php';

use WindowsAzure\Common\ServicesBuilder;
use MicrosoftAzure\Storage\Common\ServiceException;
use MicrosoftAzure\Storage\Queue\Models\PeekMessagesOptions;

// Create queue REST proxy.
$queueRestProxy = ServicesBuilder::getInstance()->createQueueService($connectionString);

// OPTIONAL: Set peek message options.
$message_options = new PeekMessagesOptions();
$message_options->setNumberOfMessages(1); // Default value is 1.

try    {
    $peekMessagesResult = $queueRestProxy->peekMessages("myqueue", $message_options);
}
catch(ServiceException $e){
    // Handle exception based on error codes and messages.
    // Error codes and messages are here:
    // http://msdn.microsoft.com/library/azure/dd179446.aspx
    $code = $e->getCode();
    $error_message = $e->getMessage();
    echo $code.": ".$error_message."<br />";
}

$messages = $peekMessagesResult->getQueueMessages();

// View messages.
$messageCount = count($messages);
if($messageCount <= 0){
    echo "There are no messages.<br />";
}
else{
    foreach($messages as $message)    {
        echo "Peeked message:<br />";
        echo "Message Id: ".$message->getMessageId()."<br />";
        echo "Date: ".date_format($message->getInsertionDate(), 'Y-m-d')."<br />";
        echo "Message text: ".$message->getMessageText()."<br /><br />";
    }
}
```

## <a name="de-queue-hello-next-message"></a>Frigör kö nästa hello-meddelande
Koden tar bort ett meddelande från en kö i två steg. Först måste du anropa **QueueRestProxy -> listMessages**, vilket gör hello meddelandet osynligt tooany annan kod som läser från hello kön. Som standard förblir det här meddelandet osynligt i 30 sekunder. (Om hello-meddelande inte tas bort i den här tiden, den blir synlig i hello kö igen.) toofinish att ta bort hello-meddelande från kön hello, måste du anropa **QueueRestProxy -> deleteMessage**. Den här tvåstegsprocessen för att ta bort ett meddelande säkerställer när din kod misslyckas tooprocess får ett meddelande på grund av fel toohardware eller programvara, en annan instans av koden hello samma meddelande och försök igen. Koden anropar **deleteMessage** direkt efter hello-meddelande har bearbetats.

```php
require_once 'vendor/autoload.php';

use WindowsAzure\Common\ServicesBuilder;
use MicrosoftAzure\Storage\Common\ServiceException;

// Create queue REST proxy.
$queueRestProxy = ServicesBuilder::getInstance()->createQueueService($connectionString);

// Get message.
$listMessagesResult = $queueRestProxy->listMessages("myqueue");
$messages = $listMessagesResult->getQueueMessages();
$message = $messages[0];

/* ---------------------
    Process message.
   --------------------- */

// Get message ID and pop receipt.
$messageId = $message->getMessageId();
$popReceipt = $message->getPopReceipt();

try    {
    // Delete message.
    $queueRestProxy->deleteMessage("myqueue", $messageId, $popReceipt);
}
catch(ServiceException $e){
    // Handle exception based on error codes and messages.
    // Error codes and messages are here:
    // http://msdn.microsoft.com/library/azure/dd179446.aspx
    $code = $e->getCode();
    $error_message = $e->getMessage();
    echo $code.": ".$error_message."<br />";
}
```

## <a name="change-hello-contents-of-a-queued-message"></a>Ändra hello innehållet i ett meddelande i kön
Du kan ändra hello innehållet i ett meddelande direkt i hello kö genom att anropa **QueueRestProxy -> updateMessage**. Om hello-meddelande representerar en arbetsuppgift, kan du använda den här funktionen tooupdate hello hello arbete aktivitetens status. hello följande kod uppdaterar hello kömeddelandet med nytt innehåll och hello synlighet timeout tooextend anger ytterligare 60 sekunder. Detta sparar hello statusen för arbetsuppgiften som är kopplad till hello-meddelande och det ger hello klient till en annan minut toocontinue som arbetar på hello-meddelande. Du kan använda den här tekniken tootrack arbetsflöden med flera steg i Kömeddelanden, utan att behöva toostart över från hello början om ett bearbetningssteg misslyckas på grund av toohardware eller ett programvarufel. Normalt ska du behålla ett återförsöksvärde och om hello meddelandet försöks mer än  *n*  tider, tar du bort den. Detta skyddar mot meddelanden som utlöser ett programfel varje gång de bearbetas.

```php
require_once 'vendor/autoload.php';

use WindowsAzure\Common\ServicesBuilder;
use MicrosoftAzure\Storage\Common\ServiceException;

// Create queue REST proxy.
$queueRestProxy = ServicesBuilder::getInstance()->createQueueService($connectionString);

// Get message.
$listMessagesResult = $queueRestProxy->listMessages("myqueue");
$messages = $listMessagesResult->getQueueMessages();
$message = $messages[0];

// Define new message properties.
$new_message_text = "New message text.";
$new_visibility_timeout = 5; // Measured in seconds.

// Get message ID and pop receipt.
$messageId = $message->getMessageId();
$popReceipt = $message->getPopReceipt();

try    {
    // Update message.
    $queueRestProxy->updateMessage("myqueue",
                                $messageId,
                                $popReceipt,
                                $new_message_text,
                                $new_visibility_timeout);
}
catch(ServiceException $e){
    // Handle exception based on error codes and messages.
    // Error codes and messages are here:
    // http://msdn.microsoft.com/library/azure/dd179446.aspx
    $code = $e->getCode();
    $error_message = $e->getMessage();
    echo $code.": ".$error_message."<br />";
}
```

## <a name="additional-options-for-de-queuing-messages"></a>Ytterligare alternativ för meddelanden ur kön
Det finns två sätt som du kan anpassa meddelandehämtningen från en kö. Först får du en grupp med meddelanden (upp too32). Andra, kan du ange en längre eller kortare synlighet tidsgräns att ge koden mer eller mindre tid toofully bearbetas varje meddelande. hello följande kodexempel används hello **GetMessage** metoden tooget 16 meddelanden i ett anrop. Sedan bearbetas varje meddelande med hjälp av en **för** loop. Den anger också hello osynlighet timeout toofive minuter för varje meddelande.

```php
require_once 'vendor/autoload.php';

use WindowsAzure\Common\ServicesBuilder;
use MicrosoftAzure\Storage\Common\ServiceException;
use MicrosoftAzure\Storage\Queue\Models\ListMessagesOptions;

// Create queue REST proxy.
$queueRestProxy = ServicesBuilder::getInstance()->createQueueService($connectionString);

// Set list message options.
$message_options = new ListMessagesOptions();
$message_options->setVisibilityTimeoutInSeconds(300);
$message_options->setNumberOfMessages(16);

// Get messages.
try{
    $listMessagesResult = $queueRestProxy->listMessages("myqueue",
                                                     $message_options);
    $messages = $listMessagesResult->getQueueMessages();

    foreach($messages as $message){

        /* ---------------------
            Process message.
        --------------------- */

        // Get message Id and pop receipt.
        $messageId = $message->getMessageId();
        $popReceipt = $message->getPopReceipt();

        // Delete message.
        $queueRestProxy->deleteMessage("myqueue", $messageId, $popReceipt);
    }
}
catch(ServiceException $e){
    // Handle exception based on error codes and messages.
    // Error codes and messages are here:
    // http://msdn.microsoft.com/library/azure/dd179446.aspx
    $code = $e->getCode();
    $error_message = $e->getMessage();
    echo $code.": ".$error_message."<br />";
}
```

## <a name="get-queue-length"></a>Hämta kölängden
Du kan få en uppskattning av hello antal meddelanden i en kö. Hej **QueueRestProxy -> getQueueMetadata** metoden ber hello kön service tooreturn metadata om hello kö. Anropar hello **getApproximateMessageCount** metoden på hello returnerade en beräkning av hur många meddelanden som finns i en kö innehåller-objektet. hello returneras endast ungefärliga eftersom meddelanden kan läggas till eller tas bort efter hello kötjänsten svarar tooyour begäran.

```php
require_once 'vendor/autoload.php';

use WindowsAzure\Common\ServicesBuilder;
use MicrosoftAzure\Storage\Common\ServiceException;

// Create queue REST proxy.
$queueRestProxy = ServicesBuilder::getInstance()->createQueueService($connectionString);

try    {
    // Get queue metadata.
    $queue_metadata = $queueRestProxy->getQueueMetadata("myqueue");
    $approx_msg_count = $queue_metadata->getApproximateMessageCount();
}
catch(ServiceException $e){
    // Handle exception based on error codes and messages.
    // Error codes and messages are here:
    // http://msdn.microsoft.com/library/azure/dd179446.aspx
    $code = $e->getCode();
    $error_message = $e->getMessage();
    echo $code.": ".$error_message."<br />";
}

echo $approx_msg_count;
```

## <a name="delete-a-queue"></a>Ta bort en kö
toodelete en kö och alla hälsningsmeddelande, anropa hello **QueueRestProxy -> deleteQueue** metod.

```php
require_once 'vendor/autoload.php';

use WindowsAzure\Common\ServicesBuilder;
use MicrosoftAzure\Storage\Common\ServiceException;

// Create queue REST proxy.
$queueRestProxy = ServicesBuilder::getInstance()->createQueueService($connectionString);

try    {
    // Delete queue.
    $queueRestProxy->deleteQueue("myqueue");
}
catch(ServiceException $e){
    // Handle exception based on error codes and messages.
    // Error codes and messages are here:
    // http://msdn.microsoft.com/library/azure/dd179446.aspx
    $code = $e->getCode();
    $error_message = $e->getMessage();
    echo $code.": ".$error_message."<br />";
}
```

## <a name="next-steps"></a>Nästa steg
Nu när du har lärt dig grunderna hello i Azure Queue storage följa dessa länkar toolearn om mer komplexa lagringsuppgifter:

* Besök hello [Azure Storage-teamets blogg](http://blogs.msdn.com/b/windowsazurestorage/).

Mer information finns också hello [PHP Developer Center](/develop/php/).

[download]: http://go.microsoft.com/fwlink/?LinkID=252473
[require_once]: http://www.php.net/manual/en/function.require-once.php
[Azure Portal]: https://portal.azure.com

