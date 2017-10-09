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
ms.openlocfilehash: 5034faf3b5f28f72d5b56ac1ce7a5723be697ce6
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="how-toouse-queue-storage-from-php"></a><span data-ttu-id="11a40-104">Hur toouse Queue storage från PHP</span><span class="sxs-lookup"><span data-stu-id="11a40-104">How toouse Queue storage from PHP</span></span>
[!INCLUDE [storage-selector-queue-include](../../includes/storage-selector-queue-include.md)]

[!INCLUDE [storage-try-azure-tools-queues](../../includes/storage-try-azure-tools-queues.md)]

## <a name="overview"></a><span data-ttu-id="11a40-105">Översikt</span><span class="sxs-lookup"><span data-stu-id="11a40-105">Overview</span></span>
<span data-ttu-id="11a40-106">Den här guiden visar hur tooperform vanliga scenarier med hjälp av hello Azure Queue storage-tjänst.</span><span class="sxs-lookup"><span data-stu-id="11a40-106">This guide will show you how tooperform common scenarios by using hello Azure Queue storage service.</span></span> <span data-ttu-id="11a40-107">hello exempel skrivs via klasser från hello Windows SDK för PHP.</span><span class="sxs-lookup"><span data-stu-id="11a40-107">hello samples are written via classes from hello Windows SDK for PHP.</span></span> <span data-ttu-id="11a40-108">hello omfattas scenarier som inkluderar infoga, granskning, hämtar, och tar bort Kömeddelanden, samt hur du skapar och tar bort köer.</span><span class="sxs-lookup"><span data-stu-id="11a40-108">hello covered scenarios include inserting, peeking, getting, and deleting queue messages, as well as creating and deleting queues.</span></span>

[!INCLUDE [storage-queue-concepts-include](../../includes/storage-queue-concepts-include.md)]

[!INCLUDE [storage-create-account-include](../../includes/storage-create-account-include.md)]

## <a name="create-a-php-application"></a><span data-ttu-id="11a40-109">Skapa en PHP-program</span><span class="sxs-lookup"><span data-stu-id="11a40-109">Create a PHP application</span></span>
<span data-ttu-id="11a40-110">Hej krav för att skapa en PHP-program som har åtkomst till Azure Queue storage är hello refererar till klasser från hello Azure SDK för PHP från inom din kod.</span><span class="sxs-lookup"><span data-stu-id="11a40-110">hello only requirement for creating a PHP application that accesses Azure Queue storage is hello referencing of classes from hello Azure SDK for PHP from within your code.</span></span> <span data-ttu-id="11a40-111">Du kan använda alla development tools toocreate ditt program, inklusive anteckningar.</span><span class="sxs-lookup"><span data-stu-id="11a40-111">You can use any development tools toocreate your application, including Notepad.</span></span>

<span data-ttu-id="11a40-112">I den här guiden använder kön lagringsfunktioner som kan anropas i en PHP-program lokalt eller i koden körs i en Azure-webbroll, en arbetsroll eller en webbplats.</span><span class="sxs-lookup"><span data-stu-id="11a40-112">In this guide, you will use Queue storage features that can be called within a PHP application locally, or in code running within an Azure web role, worker role, or website.</span></span>

## <a name="get-hello-azure-client-libraries"></a><span data-ttu-id="11a40-113">Hämta hello Azure-klientbibliotek</span><span class="sxs-lookup"><span data-stu-id="11a40-113">Get hello Azure Client Libraries</span></span>
[!INCLUDE [get-client-libraries](../../includes/get-client-libraries.md)]

## <a name="configure-your-application-tooaccess-queue-storage"></a><span data-ttu-id="11a40-114">Konfigurera ditt program tooaccess Queue storage</span><span class="sxs-lookup"><span data-stu-id="11a40-114">Configure your application tooaccess Queue storage</span></span>
<span data-ttu-id="11a40-115">toouse hello API: er för Azure Queue storage, måste du:</span><span class="sxs-lookup"><span data-stu-id="11a40-115">toouse hello APIs for Azure Queue storage, you need to:</span></span>

1. <span data-ttu-id="11a40-116">Referens hello bandladdaren filen med hjälp av hello [require_once] instruktionen.</span><span class="sxs-lookup"><span data-stu-id="11a40-116">Reference hello autoloader file by using hello [require_once] statement.</span></span>
2. <span data-ttu-id="11a40-117">Referera till alla klasser som du kan använda.</span><span class="sxs-lookup"><span data-stu-id="11a40-117">Reference any classes that you might use.</span></span>

<span data-ttu-id="11a40-118">hello följande exempel visas hur tooinclude hello bandladdaren fil- och hello **ServicesBuilder** klass.</span><span class="sxs-lookup"><span data-stu-id="11a40-118">hello following example shows how tooinclude hello autoloader file and reference hello **ServicesBuilder** class.</span></span>

> [!NOTE]
> <span data-ttu-id="11a40-119">Det här exemplet (och andra exempel i den här artikeln) förutsätter att du har installerat hello PHP-klientbibliotek för Azure via Composer.</span><span class="sxs-lookup"><span data-stu-id="11a40-119">This example (and other examples in this article) assumes that you have installed hello PHP Client Libraries for Azure via Composer.</span></span> <span data-ttu-id="11a40-120">Om du har installerat hello bibliotek manuellt måste tooreference hello `WindowsAzure.php` bandladdaren filen.</span><span class="sxs-lookup"><span data-stu-id="11a40-120">If you installed hello libraries manually, you will need tooreference hello `WindowsAzure.php` autoloader file.</span></span>
> 
> 

```php
require_once 'vendor/autoload.php';
use WindowsAzure\Common\ServicesBuilder;

```

<span data-ttu-id="11a40-121">I hello exemplen nedan hello `require_once` instruktionen visas alltid, men bara hello-klasser som krävs för hello exempel tooexecute kommer att referera till.</span><span class="sxs-lookup"><span data-stu-id="11a40-121">In hello examples below, hello `require_once` statement will be shown always, but only hello classes that are necessary for hello example tooexecute will be referenced.</span></span>

## <a name="set-up-an-azure-storage-connection"></a><span data-ttu-id="11a40-122">Skapa en Azure storage-anslutning</span><span class="sxs-lookup"><span data-stu-id="11a40-122">Set up an Azure storage connection</span></span>
<span data-ttu-id="11a40-123">tooinstantiate en Azure Queue storage-klient, måste du först ha en giltig anslutningssträng.</span><span class="sxs-lookup"><span data-stu-id="11a40-123">tooinstantiate an Azure Queue storage client, you must first have a valid connection string.</span></span> <span data-ttu-id="11a40-124">hello format för hello anslutningssträng för kön är som följer.</span><span class="sxs-lookup"><span data-stu-id="11a40-124">hello format for hello queue service connection string is as follows.</span></span>

<span data-ttu-id="11a40-125">För att komma åt en live-tjänst:</span><span class="sxs-lookup"><span data-stu-id="11a40-125">For accessing a live service:</span></span>

```php
DefaultEndpointsProtocol=[http|https];AccountName=[yourAccount];AccountKey=[yourKey]
```

<span data-ttu-id="11a40-126">För att komma åt hello emulatorn lagring:</span><span class="sxs-lookup"><span data-stu-id="11a40-126">For accessing hello emulator storage:</span></span>

```php
UseDevelopmentStorage=true
```

<span data-ttu-id="11a40-127">toocreate alla Azure-tjänst-klienter, behöver du toouse hello **ServicesBuilder** klass.</span><span class="sxs-lookup"><span data-stu-id="11a40-127">toocreate any Azure service client, you need toouse hello **ServicesBuilder** class.</span></span> <span data-ttu-id="11a40-128">Du kan använda något av följande tekniker hello:</span><span class="sxs-lookup"><span data-stu-id="11a40-128">You can use either of hello following techniques:</span></span>

* <span data-ttu-id="11a40-129">Skicka hello anslutning direkt string tooit.</span><span class="sxs-lookup"><span data-stu-id="11a40-129">Pass hello connection string directly tooit.</span></span>
* <span data-ttu-id="11a40-130">Använd **CloudConfigurationManager (CCM)** toocheck flera externa datakällor för hello anslutningssträngen:</span><span class="sxs-lookup"><span data-stu-id="11a40-130">Use **CloudConfigurationManager (CCM)** toocheck multiple external sources for hello connection string:</span></span>
  * <span data-ttu-id="11a40-131">Som standard levereras med stöd för en extern källa, miljövariabler.</span><span class="sxs-lookup"><span data-stu-id="11a40-131">By default, it comes with support for one external source—environmental variables.</span></span>
  * <span data-ttu-id="11a40-132">Du kan lägga till nya källor genom att utöka hello **ConnectionStringSource** klass.</span><span class="sxs-lookup"><span data-stu-id="11a40-132">You can add new sources by extending hello **ConnectionStringSource** class.</span></span>

<span data-ttu-id="11a40-133">Hello-exempel som beskrivs här skickas hello anslutningssträngen direkt.</span><span class="sxs-lookup"><span data-stu-id="11a40-133">For hello examples outlined here, hello connection string will be passed directly.</span></span>

```php
require_once 'vendor/autoload.php';

use WindowsAzure\Common\ServicesBuilder;

$queueRestProxy = ServicesBuilder::getInstance()->createQueueService($connectionString);
```

## <a name="create-a-queue"></a><span data-ttu-id="11a40-134">Skapa en kö</span><span class="sxs-lookup"><span data-stu-id="11a40-134">Create a queue</span></span>
<span data-ttu-id="11a40-135">En **QueueRestProxy** objekt kan du skapa en kö med hjälp av hello **createQueue** metod.</span><span class="sxs-lookup"><span data-stu-id="11a40-135">A **QueueRestProxy** object lets you create a queue by using hello **createQueue** method.</span></span> <span data-ttu-id="11a40-136">När du skapar en kö, du kan ange alternativ för hello kön, men detta så krävs inte.</span><span class="sxs-lookup"><span data-stu-id="11a40-136">When creating a queue, you can set options on hello queue, but doing so is not required.</span></span> <span data-ttu-id="11a40-137">(hello exemplet nedan visar hur tooset metadata i en kö.)</span><span class="sxs-lookup"><span data-stu-id="11a40-137">(hello example below shows how tooset metadata on a queue.)</span></span>

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
> <span data-ttu-id="11a40-138">Du bör inte förlita dig på skiftlägeskänslighet för metadata nycklar.</span><span class="sxs-lookup"><span data-stu-id="11a40-138">You should not rely on case sensitivity for metadata keys.</span></span> <span data-ttu-id="11a40-139">Alla nycklar läses från hello-tjänsten med små bokstäver.</span><span class="sxs-lookup"><span data-stu-id="11a40-139">All keys are read from hello service in lowercase.</span></span>
> 
> 

## <a name="add-a-message-tooa-queue"></a><span data-ttu-id="11a40-140">Lägg till en meddelandekö tooa</span><span class="sxs-lookup"><span data-stu-id="11a40-140">Add a message tooa queue</span></span>
<span data-ttu-id="11a40-141">tooadd tooa meddelandekö, Använd **QueueRestProxy -> createMessage**.</span><span class="sxs-lookup"><span data-stu-id="11a40-141">tooadd a message tooa queue, use **QueueRestProxy->createMessage**.</span></span> <span data-ttu-id="11a40-142">hello-metoden tar hello könamnet hello meddelandetext och meddelandealternativ (som är valfritt).</span><span class="sxs-lookup"><span data-stu-id="11a40-142">hello method takes hello queue name, hello message text, and message options (which are optional).</span></span>

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

## <a name="peek-at-hello-next-message"></a><span data-ttu-id="11a40-143">Granska nästa hello-meddelande</span><span class="sxs-lookup"><span data-stu-id="11a40-143">Peek at hello next message</span></span>
<span data-ttu-id="11a40-144">Du kan kika på meddelandet (eller meddelanden) hello fram på en kö utan att ta bort den från hello kö genom att anropa **QueueRestProxy -> peekMessages**.</span><span class="sxs-lookup"><span data-stu-id="11a40-144">You can peek at a message (or messages) at hello front of a queue without removing it from hello queue by calling **QueueRestProxy->peekMessages**.</span></span> <span data-ttu-id="11a40-145">Som standard hello **peekMessage** metoden returnerar ett enda meddelande, men du kan ändra värdet genom att använda hello **PeekMessagesOptions -> setNumberOfMessages** metod.</span><span class="sxs-lookup"><span data-stu-id="11a40-145">By default, hello **peekMessage** method returns a single message, but you can change that value by using hello **PeekMessagesOptions->setNumberOfMessages** method.</span></span>

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

## <a name="de-queue-hello-next-message"></a><span data-ttu-id="11a40-146">Frigör kö nästa hello-meddelande</span><span class="sxs-lookup"><span data-stu-id="11a40-146">De-queue hello next message</span></span>
<span data-ttu-id="11a40-147">Koden tar bort ett meddelande från en kö i två steg.</span><span class="sxs-lookup"><span data-stu-id="11a40-147">Your code removes a message from a queue in two steps.</span></span> <span data-ttu-id="11a40-148">Först måste du anropa **QueueRestProxy -> listMessages**, vilket gör hello meddelandet osynligt tooany annan kod som läser från hello kön.</span><span class="sxs-lookup"><span data-stu-id="11a40-148">First, you call **QueueRestProxy->listMessages**, which makes hello message invisible tooany other code that's reading from hello queue.</span></span> <span data-ttu-id="11a40-149">Som standard förblir det här meddelandet osynligt i 30 sekunder.</span><span class="sxs-lookup"><span data-stu-id="11a40-149">By default, this message will stay invisible for 30 seconds.</span></span> <span data-ttu-id="11a40-150">(Om hello-meddelande inte tas bort i den här tiden, den blir synlig i hello kö igen.) toofinish att ta bort hello-meddelande från kön hello, måste du anropa **QueueRestProxy -> deleteMessage**.</span><span class="sxs-lookup"><span data-stu-id="11a40-150">(If hello message is not deleted in this time period, it will become visible on hello queue again.) toofinish removing hello message from hello queue, you must call **QueueRestProxy->deleteMessage**.</span></span> <span data-ttu-id="11a40-151">Den här tvåstegsprocessen för att ta bort ett meddelande säkerställer när din kod misslyckas tooprocess får ett meddelande på grund av fel toohardware eller programvara, en annan instans av koden hello samma meddelande och försök igen.</span><span class="sxs-lookup"><span data-stu-id="11a40-151">This two-step process of removing a message assures that when your code fails tooprocess a message due toohardware or software failure, another instance of your code can get hello same message and try again.</span></span> <span data-ttu-id="11a40-152">Koden anropar **deleteMessage** direkt efter hello-meddelande har bearbetats.</span><span class="sxs-lookup"><span data-stu-id="11a40-152">Your code calls **deleteMessage** right after hello message has been processed.</span></span>

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

## <a name="change-hello-contents-of-a-queued-message"></a><span data-ttu-id="11a40-153">Ändra hello innehållet i ett meddelande i kön</span><span class="sxs-lookup"><span data-stu-id="11a40-153">Change hello contents of a queued message</span></span>
<span data-ttu-id="11a40-154">Du kan ändra hello innehållet i ett meddelande direkt i hello kö genom att anropa **QueueRestProxy -> updateMessage**.</span><span class="sxs-lookup"><span data-stu-id="11a40-154">You can change hello contents of a message in-place in hello queue by calling **QueueRestProxy->updateMessage**.</span></span> <span data-ttu-id="11a40-155">Om hello-meddelande representerar en arbetsuppgift, kan du använda den här funktionen tooupdate hello hello arbete aktivitetens status.</span><span class="sxs-lookup"><span data-stu-id="11a40-155">If hello message represents a work task, you could use this feature tooupdate hello status of hello work task.</span></span> <span data-ttu-id="11a40-156">hello följande kod uppdaterar hello kömeddelandet med nytt innehåll och hello synlighet timeout tooextend anger ytterligare 60 sekunder.</span><span class="sxs-lookup"><span data-stu-id="11a40-156">hello following code updates hello queue message with new contents, and it sets hello visibility timeout tooextend another 60 seconds.</span></span> <span data-ttu-id="11a40-157">Detta sparar hello statusen för arbetsuppgiften som är kopplad till hello-meddelande och det ger hello klient till en annan minut toocontinue som arbetar på hello-meddelande.</span><span class="sxs-lookup"><span data-stu-id="11a40-157">This saves hello state of work that's associated with hello message, and it gives hello client another minute toocontinue working on hello message.</span></span> <span data-ttu-id="11a40-158">Du kan använda den här tekniken tootrack arbetsflöden med flera steg i Kömeddelanden, utan att behöva toostart över från hello början om ett bearbetningssteg misslyckas på grund av toohardware eller ett programvarufel.</span><span class="sxs-lookup"><span data-stu-id="11a40-158">You could use this technique tootrack multi-step workflows on queue messages, without having toostart over from hello beginning if a processing step fails due toohardware or software failure.</span></span> <span data-ttu-id="11a40-159">Normalt ska du behålla ett återförsöksvärde och om hello meddelandet försöks mer än  *n*  tider, tar du bort den.</span><span class="sxs-lookup"><span data-stu-id="11a40-159">Typically, you would keep a retry count as well, and if hello message is retried more than *n* times, you would delete it.</span></span> <span data-ttu-id="11a40-160">Detta skyddar mot meddelanden som utlöser ett programfel varje gång de bearbetas.</span><span class="sxs-lookup"><span data-stu-id="11a40-160">This protects against a message that triggers an application error each time it is processed.</span></span>

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

## <a name="additional-options-for-de-queuing-messages"></a><span data-ttu-id="11a40-161">Ytterligare alternativ för meddelanden ur kön</span><span class="sxs-lookup"><span data-stu-id="11a40-161">Additional options for de-queuing messages</span></span>
<span data-ttu-id="11a40-162">Det finns två sätt som du kan anpassa meddelandehämtningen från en kö.</span><span class="sxs-lookup"><span data-stu-id="11a40-162">There are two ways that you can customize message retrieval from a queue.</span></span> <span data-ttu-id="11a40-163">Först får du en grupp med meddelanden (upp too32).</span><span class="sxs-lookup"><span data-stu-id="11a40-163">First, you can get a batch of messages (up too32).</span></span> <span data-ttu-id="11a40-164">Andra, kan du ange en längre eller kortare synlighet tidsgräns att ge koden mer eller mindre tid toofully bearbetas varje meddelande.</span><span class="sxs-lookup"><span data-stu-id="11a40-164">Second, you can set a longer or shorter visibility timeout, allowing your code more or less time toofully process each message.</span></span> <span data-ttu-id="11a40-165">hello följande kodexempel används hello **GetMessage** metoden tooget 16 meddelanden i ett anrop.</span><span class="sxs-lookup"><span data-stu-id="11a40-165">hello following code example uses hello **getMessages** method tooget 16 messages in one call.</span></span> <span data-ttu-id="11a40-166">Sedan bearbetas varje meddelande med hjälp av en **för** loop.</span><span class="sxs-lookup"><span data-stu-id="11a40-166">Then it processes each message by using a **for** loop.</span></span> <span data-ttu-id="11a40-167">Den anger också hello osynlighet timeout toofive minuter för varje meddelande.</span><span class="sxs-lookup"><span data-stu-id="11a40-167">It also sets hello invisibility timeout toofive minutes for each message.</span></span>

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

## <a name="get-queue-length"></a><span data-ttu-id="11a40-168">Hämta kölängden</span><span class="sxs-lookup"><span data-stu-id="11a40-168">Get queue length</span></span>
<span data-ttu-id="11a40-169">Du kan få en uppskattning av hello antal meddelanden i en kö.</span><span class="sxs-lookup"><span data-stu-id="11a40-169">You can get an estimate of hello number of messages in a queue.</span></span> <span data-ttu-id="11a40-170">Hej **QueueRestProxy -> getQueueMetadata** metoden ber hello kön service tooreturn metadata om hello kö.</span><span class="sxs-lookup"><span data-stu-id="11a40-170">hello **QueueRestProxy->getQueueMetadata** method asks hello queue service tooreturn metadata about hello queue.</span></span> <span data-ttu-id="11a40-171">Anropar hello **getApproximateMessageCount** metoden på hello returnerade en beräkning av hur många meddelanden som finns i en kö innehåller-objektet.</span><span class="sxs-lookup"><span data-stu-id="11a40-171">Calling hello **getApproximateMessageCount** method on hello returned object provides a count of how many messages are in a queue.</span></span> <span data-ttu-id="11a40-172">hello returneras endast ungefärliga eftersom meddelanden kan läggas till eller tas bort efter hello kötjänsten svarar tooyour begäran.</span><span class="sxs-lookup"><span data-stu-id="11a40-172">hello count is only approximate because messages can be added or removed after hello queue service responds tooyour request.</span></span>

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

## <a name="delete-a-queue"></a><span data-ttu-id="11a40-173">Ta bort en kö</span><span class="sxs-lookup"><span data-stu-id="11a40-173">Delete a queue</span></span>
<span data-ttu-id="11a40-174">toodelete en kö och alla hälsningsmeddelande, anropa hello **QueueRestProxy -> deleteQueue** metod.</span><span class="sxs-lookup"><span data-stu-id="11a40-174">toodelete a queue and all hello messages in it, call hello **QueueRestProxy->deleteQueue** method.</span></span>

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

## <a name="next-steps"></a><span data-ttu-id="11a40-175">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="11a40-175">Next steps</span></span>
<span data-ttu-id="11a40-176">Nu när du har lärt dig grunderna hello i Azure Queue storage följa dessa länkar toolearn om mer komplexa lagringsuppgifter:</span><span class="sxs-lookup"><span data-stu-id="11a40-176">Now that you've learned hello basics of Azure Queue storage, follow these links toolearn about more complex storage tasks:</span></span>

* <span data-ttu-id="11a40-177">Besök hello [Azure Storage-teamets blogg](http://blogs.msdn.com/b/windowsazurestorage/).</span><span class="sxs-lookup"><span data-stu-id="11a40-177">Visit hello [Azure Storage Team blog](http://blogs.msdn.com/b/windowsazurestorage/).</span></span>

<span data-ttu-id="11a40-178">Mer information finns också hello [PHP Developer Center](/develop/php/).</span><span class="sxs-lookup"><span data-stu-id="11a40-178">For more information, see also hello [PHP Developer Center](/develop/php/).</span></span>

[download]: http://go.microsoft.com/fwlink/?LinkID=252473
[require_once]: http://www.php.net/manual/en/function.require-once.php
[Azure Portal]: https://portal.azure.com

