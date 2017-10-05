---
title: "Hur du använder Service Bus-köer med PHP | Microsoft Docs"
description: "Lär dig hur du använder Service Bus-köer i Azure. Kodexempel som skrivits i PHP."
services: service-bus-messaging
documentationcenter: php
author: sethmanheim
manager: timlt
editor: 
ms.assetid: e29c829b-44c5-4350-8f2e-39e0c380a9f2
ms.service: service-bus-messaging
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: PHP
ms.topic: article
ms.date: 08/10/2017
ms.author: sethm
ms.openlocfilehash: 3514812f7f087582035dad5d9a4d620652aa4da9
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/18/2017
---
# <a name="how-to-use-service-bus-queues-with-php"></a><span data-ttu-id="550ea-104">Hur du använder Service Bus-köer med PHP</span><span class="sxs-lookup"><span data-stu-id="550ea-104">How to use Service Bus queues with PHP</span></span>
[!INCLUDE [service-bus-selector-queues](../../includes/service-bus-selector-queues.md)]

<span data-ttu-id="550ea-105">Den här guiden visar hur du använder Service Bus-köer.</span><span class="sxs-lookup"><span data-stu-id="550ea-105">This guide shows you how to use Service Bus queues.</span></span> <span data-ttu-id="550ea-106">Exemplen är skrivna i PHP och Använd den [Azure SDK för PHP](../php-download-sdk.md).</span><span class="sxs-lookup"><span data-stu-id="550ea-106">The samples are written in PHP and use the [Azure SDK for PHP](../php-download-sdk.md).</span></span> <span data-ttu-id="550ea-107">Scenarier som tas upp inkluderar **skapa köer**, **skicka och ta emot meddelanden**, och **tar bort köer**.</span><span class="sxs-lookup"><span data-stu-id="550ea-107">The scenarios covered include **creating queues**, **sending and receiving messages**, and **deleting queues**.</span></span>

[!INCLUDE [howto-service-bus-queues](../../includes/howto-service-bus-queues.md)]

## <a name="create-a-php-application"></a><span data-ttu-id="550ea-108">Skapa en PHP-program</span><span class="sxs-lookup"><span data-stu-id="550ea-108">Create a PHP application</span></span>
<span data-ttu-id="550ea-109">Det enda kravet för att skapa en PHP-program som har åtkomst till Azure Blob-tjänsten är refererar till klasserna i den [Azure SDK för PHP](../php-download-sdk.md) från inom din kod.</span><span class="sxs-lookup"><span data-stu-id="550ea-109">The only requirement for creating a PHP application that accesses the Azure Blob service is the referencing of classes in the [Azure SDK for PHP](../php-download-sdk.md) from within your code.</span></span> <span data-ttu-id="550ea-110">Du kan använda alla utvecklingsverktyg för att skapa program, eller anteckningar.</span><span class="sxs-lookup"><span data-stu-id="550ea-110">You can use any development tools to create your application, or Notepad.</span></span>

> [!NOTE]
> <span data-ttu-id="550ea-111">PHP-installation måste också ha den [OpenSSL tillägget](http://php.net/openssl) installerat och aktiverat.</span><span class="sxs-lookup"><span data-stu-id="550ea-111">Your PHP installation must also have the [OpenSSL extension](http://php.net/openssl) installed and enabled.</span></span>
> 
> 

<span data-ttu-id="550ea-112">I den här guiden använder du tjänstens funktioner som kan anropas från ett PHP-program lokalt eller i koden körs i en Azure-webbroll, en arbetsroll eller en webbplats.</span><span class="sxs-lookup"><span data-stu-id="550ea-112">In this guide, you will use service features which can be called from within a PHP application locally, or in code running within an Azure web role, worker role, or website.</span></span>

## <a name="get-the-azure-client-libraries"></a><span data-ttu-id="550ea-113">Hämta Azure klienten bibliotek</span><span class="sxs-lookup"><span data-stu-id="550ea-113">Get the Azure client libraries</span></span>
[!INCLUDE [get-client-libraries](../../includes/get-client-libraries.md)]

## <a name="configure-your-application-to-use-service-bus"></a><span data-ttu-id="550ea-114">Konfigurera ditt program att använda Service Bus</span><span class="sxs-lookup"><span data-stu-id="550ea-114">Configure your application to use Service Bus</span></span>
<span data-ttu-id="550ea-115">Om du vill använda Service Bus-kö API: er, gör du följande:</span><span class="sxs-lookup"><span data-stu-id="550ea-115">To use the Service Bus queue APIs, do the following:</span></span>

1. <span data-ttu-id="550ea-116">Referera till den automatiska bandladdaren filen med hjälp av den [require_once] [ require_once] instruktionen.</span><span class="sxs-lookup"><span data-stu-id="550ea-116">Reference the autoloader file using the [require_once][require_once] statement.</span></span>
2. <span data-ttu-id="550ea-117">Referera till alla klasser som du kan använda.</span><span class="sxs-lookup"><span data-stu-id="550ea-117">Reference any classes you might use.</span></span>

<span data-ttu-id="550ea-118">I följande exempel visas hur du lägger till den automatiska bandladdaren fil- och referens av `ServicesBuilder` klass.</span><span class="sxs-lookup"><span data-stu-id="550ea-118">The following example shows how to include the autoloader file and reference the `ServicesBuilder` class.</span></span>

> [!NOTE]
> <span data-ttu-id="550ea-119">Det här exemplet (och andra exempel i den här artikeln) förutsätter att du har installerat PHP-klientbibliotek för Azure via Composer.</span><span class="sxs-lookup"><span data-stu-id="550ea-119">This example (and other examples in this article) assumes you have installed the PHP Client Libraries for Azure via Composer.</span></span> <span data-ttu-id="550ea-120">Om du har installerat biblioteken manuellt eller som ett PÄRONTRÄD paket måste du referera till den **WindowsAzure.php** bandladdaren filen.</span><span class="sxs-lookup"><span data-stu-id="550ea-120">If you installed the libraries manually or as a PEAR package, you must reference the **WindowsAzure.php** autoloader file.</span></span>
> 
> 

```php
require_once 'vendor/autoload.php';
use WindowsAzure\Common\ServicesBuilder;
```

<span data-ttu-id="550ea-121">I exemplen nedan, den `require_once` instruktionen visas alltid, men endast klasserna som krävs för att köra refereras.</span><span class="sxs-lookup"><span data-stu-id="550ea-121">In the examples below, the `require_once` statement will always be shown, but only the classes necessary for the example to execute are referenced.</span></span>

## <a name="set-up-a-service-bus-connection"></a><span data-ttu-id="550ea-122">Konfigurera en Service Bus-anslutning</span><span class="sxs-lookup"><span data-stu-id="550ea-122">Set up a Service Bus connection</span></span>
<span data-ttu-id="550ea-123">Om du vill skapa en instans av en Service Bus-klient, måste du ha en giltig anslutningssträng i det här formatet:</span><span class="sxs-lookup"><span data-stu-id="550ea-123">To instantiate a Service Bus client, you must first have a valid connection string in this format:</span></span>

```
Endpoint=[yourEndpoint];SharedAccessKeyName=RootManageSharedAccessKey;SharedAccessKey=[Primary Key]
```

<span data-ttu-id="550ea-124">Där `Endpoint` är vanligtvis i formatet `[yourNamespace].servicebus.windows.net`.</span><span class="sxs-lookup"><span data-stu-id="550ea-124">Where `Endpoint` is typically of the format `[yourNamespace].servicebus.windows.net`.</span></span>

<span data-ttu-id="550ea-125">Så här skapar du en Azure-tjänsten måste du använda den `ServicesBuilder` klass.</span><span class="sxs-lookup"><span data-stu-id="550ea-125">To create any Azure service client you must use the `ServicesBuilder` class.</span></span> <span data-ttu-id="550ea-126">Du kan:</span><span class="sxs-lookup"><span data-stu-id="550ea-126">You can:</span></span>

* <span data-ttu-id="550ea-127">Skicka anslutningssträngen till den.</span><span class="sxs-lookup"><span data-stu-id="550ea-127">Pass the connection string directly to it.</span></span>
* <span data-ttu-id="550ea-128">Använd den **CloudConfigurationManager (CCM)** till flera externa källor för anslutningssträngen:</span><span class="sxs-lookup"><span data-stu-id="550ea-128">Use the **CloudConfigurationManager (CCM)** to check multiple external sources for the connection string:</span></span>
  * <span data-ttu-id="550ea-129">Som standard levereras med stöd för en extern källa - miljövariabler</span><span class="sxs-lookup"><span data-stu-id="550ea-129">By default it comes with support for one external source - environmental variables</span></span>
  * <span data-ttu-id="550ea-130">Du kan lägga till nya källor genom att utöka den `ConnectionStringSource` klass</span><span class="sxs-lookup"><span data-stu-id="550ea-130">You can add new sources by extending the `ConnectionStringSource` class</span></span>

<span data-ttu-id="550ea-131">Exempel som beskrivs här skickas anslutningssträngen direkt.</span><span class="sxs-lookup"><span data-stu-id="550ea-131">For the examples outlined here, the connection string is passed directly.</span></span>

```php
require_once 'vendor/autoload.php';

use WindowsAzure\Common\ServicesBuilder;

$connectionString = "Endpoint=[yourEndpoint];SharedAccessKeyName=RootManageSharedAccessKey;SharedAccessKey=[Primary Key]";

$serviceBusRestProxy = ServicesBuilder::getInstance()->createServiceBusService($connectionString);
```

## <a name="create-a-queue"></a><span data-ttu-id="550ea-132">Skapa en kö</span><span class="sxs-lookup"><span data-stu-id="550ea-132">Create a queue</span></span>
<span data-ttu-id="550ea-133">Du kan utföra hanteringsåtgärder för Service Bus-köer via den `ServiceBusRestProxy` klass.</span><span class="sxs-lookup"><span data-stu-id="550ea-133">You can perform management operations for Service Bus queues via the `ServiceBusRestProxy` class.</span></span> <span data-ttu-id="550ea-134">En `ServiceBusRestProxy` objektet har skapats den `ServicesBuilder::createServiceBusService` fabriksmetoden med en lämplig anslutningssträng som innehåller behörigheterna som är token för att hantera den.</span><span class="sxs-lookup"><span data-stu-id="550ea-134">A `ServiceBusRestProxy` object is constructed via the `ServicesBuilder::createServiceBusService` factory method with an appropriate connection string that encapsulates the token permissions to manage it.</span></span>

<span data-ttu-id="550ea-135">I följande exempel visas hur du kan skapa en instans av en `ServiceBusRestProxy` och anropa `ServiceBusRestProxy->createQueue` du skapar en kö med namnet `myqueue` inom en `MySBNamespace` namnområde för tjänsten:</span><span class="sxs-lookup"><span data-stu-id="550ea-135">The following example shows how to instantiate a `ServiceBusRestProxy` and call `ServiceBusRestProxy->createQueue` to create a queue named `myqueue` within a `MySBNamespace` service namespace:</span></span>

```php
require_once 'vendor/autoload.php';

use WindowsAzure\Common\ServicesBuilder;
use WindowsAzure\Common\ServiceException;
use WindowsAzure\ServiceBus\Models\QueueInfo;

// Create Service Bus REST proxy.
$serviceBusRestProxy = ServicesBuilder::getInstance()->createServiceBusService($connectionString);

try    {
    $queueInfo = new QueueInfo("myqueue");

    // Create queue.
    $serviceBusRestProxy->createQueue($queueInfo);
}
catch(ServiceException $e){
    // Handle exception based on error codes and messages.
    // Error codes and messages are here: 
    // https://docs.microsoft.com/rest/api/storageservices/Common-REST-API-Error-Codes
    $code = $e->getCode();
    $error_message = $e->getMessage();
    echo $code.": ".$error_message."<br />";
}
```

> [!NOTE]
> <span data-ttu-id="550ea-136">Du kan använda den `listQueues` metod på `ServiceBusRestProxy` objekt för att kontrollera om det redan finns en kö med ett angivet namn i ett namnområde.</span><span class="sxs-lookup"><span data-stu-id="550ea-136">You can use the `listQueues` method on `ServiceBusRestProxy` objects to check if a queue with a specified name already exists within a namespace.</span></span>
> 
> 

## <a name="send-messages-to-a-queue"></a><span data-ttu-id="550ea-137">Skicka meddelanden till en kö</span><span class="sxs-lookup"><span data-stu-id="550ea-137">Send messages to a queue</span></span>
<span data-ttu-id="550ea-138">Att skicka ett meddelande till en Service Bus-kö, program-anrop i `ServiceBusRestProxy->sendQueueMessage` metod.</span><span class="sxs-lookup"><span data-stu-id="550ea-138">To send a message to a Service Bus queue, your application calls the `ServiceBusRestProxy->sendQueueMessage` method.</span></span> <span data-ttu-id="550ea-139">Följande kod visar hur du skickar ett meddelande till den `myqueue` kö som tidigare har skapat i den `MySBNamespace` namnområde för tjänsten.</span><span class="sxs-lookup"><span data-stu-id="550ea-139">The following code shows how to send a message to the `myqueue` queue previously created within the `MySBNamespace` service namespace.</span></span>

```php
require_once 'vendor/autoload.php';

use WindowsAzure\Common\ServicesBuilder;
use WindowsAzure\Common\ServiceException;
use WindowsAzure\ServiceBus\Models\BrokeredMessage;

// Create Service Bus REST proxy.
$serviceBusRestProxy = ServicesBuilder::getInstance()->createServiceBusService($connectionString);

try    {
    // Create message.
    $message = new BrokeredMessage();
    $message->setBody("my message");

    // Send message.
    $serviceBusRestProxy->sendQueueMessage("myqueue", $message);
}
catch(ServiceException $e){
    // Handle exception based on error codes and messages.
    // Error codes and messages are here: 
    // https://docs.microsoft.com/rest/api/storageservices/Common-REST-API-Error-Codes
    $code = $e->getCode();
    $error_message = $e->getMessage();
    echo $code.": ".$error_message."<br />";
}
```

<span data-ttu-id="550ea-140">Meddelanden skickas till (och mottagna från) Service Bus-köer är instanser av den [BrokeredMessage] [ BrokeredMessage] klass.</span><span class="sxs-lookup"><span data-stu-id="550ea-140">Messages sent to (and received from ) Service Bus queues are instances of the [BrokeredMessage][BrokeredMessage] class.</span></span> <span data-ttu-id="550ea-141">[BrokeredMessage] [ BrokeredMessage] objekt har en uppsättning standard metoder och egenskaper som används för att lagra anpassade programspecifika egenskaper och en brödtext med godtyckliga programdata.</span><span class="sxs-lookup"><span data-stu-id="550ea-141">[BrokeredMessage][BrokeredMessage] objects have a set of standard methods and properties that are used to hold custom application-specific properties, and a body of arbitrary application data.</span></span>

<span data-ttu-id="550ea-142">Service Bus-köerna stöder en maximal meddelandestorlek på 256 kB på [standardnivån](service-bus-premium-messaging.md) och 1 MB på [premiumnivån](service-bus-premium-messaging.md).</span><span class="sxs-lookup"><span data-stu-id="550ea-142">Service Bus queues support a maximum message size of 256 KB in the [Standard tier](service-bus-premium-messaging.md) and 1 MB in the [Premium tier](service-bus-premium-messaging.md).</span></span> <span data-ttu-id="550ea-143">Rubriken, som inkluderar standardprogramegenskaperna och de anpassade programegenskaperna, kan ha en maximal storlek på 64 kB.</span><span class="sxs-lookup"><span data-stu-id="550ea-143">The header, which includes the standard and custom application properties, can have a maximum size of 64 KB.</span></span> <span data-ttu-id="550ea-144">Det finns ingen gräns för antalet meddelanden som kan finnas i en kö men det finns ett tak för den totala storleken för de meddelanden som ligger i en kö.</span><span class="sxs-lookup"><span data-stu-id="550ea-144">There is no limit on the number of messages held in a queue but there is a cap on the total size of the messages held by a queue.</span></span> <span data-ttu-id="550ea-145">Den här övre gräns för storleken är 5 GB.</span><span class="sxs-lookup"><span data-stu-id="550ea-145">This upper limit on queue size is 5 GB.</span></span>

## <a name="receive-messages-from-a-queue"></a><span data-ttu-id="550ea-146">Ta emot meddelanden från en kö</span><span class="sxs-lookup"><span data-stu-id="550ea-146">Receive messages from a queue</span></span>

<span data-ttu-id="550ea-147">Det bästa sättet att ta emot meddelanden från en kö är att använda en `ServiceBusRestProxy->receiveQueueMessage` metod.</span><span class="sxs-lookup"><span data-stu-id="550ea-147">The best way to receive messages from a queue is to use a `ServiceBusRestProxy->receiveQueueMessage` method.</span></span> <span data-ttu-id="550ea-148">Meddelanden kan tas emot i två olika lägen: [ *ReceiveAndDelete* ](/dotnet/api/microsoft.servicebus.messaging.receivemode.receiveanddelete) och [ *PeekLock*](/dotnet/api/microsoft.servicebus.messaging.receivemode.peeklock).</span><span class="sxs-lookup"><span data-stu-id="550ea-148">Messages can be received in two different modes: [*ReceiveAndDelete*](/dotnet/api/microsoft.servicebus.messaging.receivemode.receiveanddelete) and [*PeekLock*](/dotnet/api/microsoft.servicebus.messaging.receivemode.peeklock).</span></span> <span data-ttu-id="550ea-149">**PeekLock** är standard.</span><span class="sxs-lookup"><span data-stu-id="550ea-149">**PeekLock** is the default.</span></span>

<span data-ttu-id="550ea-150">När du använder [ReceiveAndDelete](/dotnet/api/microsoft.servicebus.messaging.receivemode.receiveanddelete) läge får är en engångsåtgärd, det vill säga när Service Bus tar emot en läsbegäran för ett meddelande i en kö, den markerar meddelandet som Förbrukat och skickar tillbaka det till programmet.</span><span class="sxs-lookup"><span data-stu-id="550ea-150">When using [ReceiveAndDelete](/dotnet/api/microsoft.servicebus.messaging.receivemode.receiveanddelete) mode, receive is a single-shot operation; that is, when Service Bus receives a read request for a message in a queue, it marks the message as being consumed and returns it to the application.</span></span> <span data-ttu-id="550ea-151">Läget [ReceiveAndDelete](/dotnet/api/microsoft.servicebus.messaging.receivemode.receiveanddelete) är den enklaste modellen och fungerar bäst för scenarier där ett program kan tolerera icke-bearbetning av ett meddelande om ett fel inträffar.</span><span class="sxs-lookup"><span data-stu-id="550ea-151">[ReceiveAndDelete](/dotnet/api/microsoft.servicebus.messaging.receivemode.receiveanddelete) mode is the simplest model and works best for scenarios in which an application can tolerate not processing a message in the event of a failure.</span></span> <span data-ttu-id="550ea-152">För att förstå detta kan du föreställa dig ett scenario där konsumenten utfärdar en receive-begäran och sedan kraschar innan den kan bearbeta denna begäran.</span><span class="sxs-lookup"><span data-stu-id="550ea-152">To understand this, consider a scenario in which the consumer issues the receive request and then crashes before processing it.</span></span> <span data-ttu-id="550ea-153">Eftersom Service Bus kommer att ha markerat meddelandet som Förbrukat, och sedan när programmet startas om och börjar förbruka meddelanden igen, att ha missat meddelandet som förbrukades innan kraschen.</span><span class="sxs-lookup"><span data-stu-id="550ea-153">Because Service Bus will have marked the message as being consumed, then when the application restarts and begins consuming messages again, it will have missed the message that was consumed prior to the crash.</span></span>

<span data-ttu-id="550ea-154">I standard [PeekLock](/dotnet/api/microsoft.servicebus.messaging.receivemode.peeklock) läge, ett meddelande blir en åtgärd i två steg, vilket gör det möjligt att stödprogram som inte tolererar att ett meddelande saknas.</span><span class="sxs-lookup"><span data-stu-id="550ea-154">In the default [PeekLock](/dotnet/api/microsoft.servicebus.messaging.receivemode.peeklock) mode, receiving a message becomes a two stage operation, which makes it possible to support applications that cannot tolerate missing messages.</span></span> <span data-ttu-id="550ea-155">När Service Bus tar emot en begäran, den söker efter nästa meddelande som ska förbrukas, låser det för att förhindra att andra användare tar emot det och skickar sedan tillbaka det till programmet.</span><span class="sxs-lookup"><span data-stu-id="550ea-155">When Service Bus receives a request, it finds the next message to be consumed, locks it to prevent other consumers from receiving it, and then returns it to the application.</span></span> <span data-ttu-id="550ea-156">När programmet har avslutat bearbetningen av meddelandet (eller lagrar det på ett tillförlitligt sätt för framtida bearbetning), den är klar det andra steget i processen genom att skicka det mottagna meddelandet till `ServiceBusRestProxy->deleteMessage`.</span><span class="sxs-lookup"><span data-stu-id="550ea-156">After the application finishes processing the message (or stores it reliably for future processing), it completes the second stage of the receive process by passing the received message to `ServiceBusRestProxy->deleteMessage`.</span></span> <span data-ttu-id="550ea-157">När Service Bus ser den `deleteMessage` -anrop som den markera meddelandet som Förbrukat och tas bort från kön.</span><span class="sxs-lookup"><span data-stu-id="550ea-157">When Service Bus sees the `deleteMessage` call, it will mark the message as being consumed and remove it from the queue.</span></span>

<span data-ttu-id="550ea-158">I följande exempel visas hur du tar emot och bearbeta ett meddelande med hjälp av [PeekLock](/dotnet/api/microsoft.servicebus.messaging.receivemode.peeklock) läge (standardläget).</span><span class="sxs-lookup"><span data-stu-id="550ea-158">The following example shows how to receive and process a message using [PeekLock](/dotnet/api/microsoft.servicebus.messaging.receivemode.peeklock) mode (the default mode).</span></span>

```php
require_once 'vendor/autoload.php';

use WindowsAzure\Common\ServicesBuilder;
use WindowsAzure\Common\ServiceException;
use WindowsAzure\ServiceBus\Models\ReceiveMessageOptions;

// Create Service Bus REST proxy.
$serviceBusRestProxy = ServicesBuilder::getInstance()->createServiceBusService($connectionString);

try    {
    // Set the receive mode to PeekLock (default is ReceiveAndDelete).
    $options = new ReceiveMessageOptions();
    $options->setPeekLock();

    // Receive message.
    $message = $serviceBusRestProxy->receiveQueueMessage("myqueue", $options);
    echo "Body: ".$message->getBody()."<br />";
    echo "MessageID: ".$message->getMessageId()."<br />";

    /*---------------------------
        Process message here.
    ----------------------------*/

    // Delete message. Not necessary if peek lock is not set.
    echo "Message deleted.<br />";
    $serviceBusRestProxy->deleteMessage($message);
}
catch(ServiceException $e){
    // Handle exception based on error codes and messages.
    // Error codes and messages are here:
    // https://docs.microsoft.com/rest/api/storageservices/Common-REST-API-Error-Codes
    $code = $e->getCode();
    $error_message = $e->getMessage();
    echo $code.": ".$error_message."<br />";
}
```

## <a name="how-to-handle-application-crashes-and-unreadable-messages"></a><span data-ttu-id="550ea-159">Hantera programkrascher och oläsbara meddelanden</span><span class="sxs-lookup"><span data-stu-id="550ea-159">How to handle application crashes and unreadable messages</span></span>

<span data-ttu-id="550ea-160">Service Bus innehåller funktioner som hjälper dig att återställa fel i programmet eller lösa problem med bearbetning av meddelanden på ett snyggt sätt.</span><span class="sxs-lookup"><span data-stu-id="550ea-160">Service Bus provides functionality to help you gracefully recover from errors in your application or difficulties processing a message.</span></span> <span data-ttu-id="550ea-161">Om ett mottagarprogram går inte att bearbeta meddelandet av någon anledning, så kan det anropa den `unlockMessage` metod för det mottagna meddelandet (i stället för den `deleteMessage` metod).</span><span class="sxs-lookup"><span data-stu-id="550ea-161">If a receiver application is unable to process the message for some reason, then it can call the `unlockMessage` method on the received message (instead of the `deleteMessage` method).</span></span> <span data-ttu-id="550ea-162">Detta gör att Service Bus låser upp meddelandet i kön och gör det tillgängligt att tas emot igen, antingen genom samma användningsprogram eller ett annat användningsprogram.</span><span class="sxs-lookup"><span data-stu-id="550ea-162">This will cause Service Bus to unlock the message within the queue and make it available to be received again, either by the same consuming application or by another consuming application.</span></span>

<span data-ttu-id="550ea-163">Det finns också en tidsgräns som är associerad med ett meddelande som ligger låst i kön och om programmet misslyckas med att bearbeta meddelandet innan timeout för lås går ut (till exempel om programmet kraschar), kommer Service Bus att låsa upp meddelandet automatiskt och gör den tillgängligt att tas emot igen.</span><span class="sxs-lookup"><span data-stu-id="550ea-163">There is also a timeout associated with a message locked within the queue, and if the application fails to process the message before the lock timeout expires (for example, if the application crashes), then Service Bus will unlock the message automatically and make it available to be received again.</span></span>

<span data-ttu-id="550ea-164">I händelse av att programmet kraschar efter att meddelandet har bearbetats men innan det `deleteMessage` begäran utfärdas och sedan meddelandet att levereras till programmet när den startas om.</span><span class="sxs-lookup"><span data-stu-id="550ea-164">In the event that the application crashes after processing the message but before the `deleteMessage` request is issued, then the message will be redelivered to the application when it restarts.</span></span> <span data-ttu-id="550ea-165">Det här kallas ofta *minst när* bearbetningen, det vill säga, varje meddelande bearbetas minst en gång men i vissa situationer kan samma meddelande levereras.</span><span class="sxs-lookup"><span data-stu-id="550ea-165">This is often called *At Least Once* processing; that is, each message is processed at least once but in certain situations the same message may be redelivered.</span></span> <span data-ttu-id="550ea-166">Om scenariot inte tolererar duplicerad bearbetning, bör sedan lägga till ytterligare logik i program för att hantera duplicerad meddelandeleverans.</span><span class="sxs-lookup"><span data-stu-id="550ea-166">If the scenario cannot tolerate duplicate processing, then adding additional logic to applications to handle duplicate message delivery is recommended.</span></span> <span data-ttu-id="550ea-167">Detta uppnås ofta med hjälp av den `getMessageId` -metoden i meddelandet som förblir konstant under alla leveransförsök.</span><span class="sxs-lookup"><span data-stu-id="550ea-167">This is often achieved using the `getMessageId` method of the message, which remains constant across delivery attempts.</span></span>

## <a name="next-steps"></a><span data-ttu-id="550ea-168">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="550ea-168">Next steps</span></span>
<span data-ttu-id="550ea-169">Nu när du har lärt dig grunderna om Service Bus-köer, se [köer, ämnen och prenumerationer] [ Queues, topics, and subscriptions] för mer information.</span><span class="sxs-lookup"><span data-stu-id="550ea-169">Now that you've learned the basics of Service Bus queues, see [Queues, topics, and subscriptions][Queues, topics, and subscriptions] for more information.</span></span>

<span data-ttu-id="550ea-170">Mer information finns också finns den [PHP Developer Center](https://azure.microsoft.com/develop/php/).</span><span class="sxs-lookup"><span data-stu-id="550ea-170">For more information, also visit the [PHP Developer Center](https://azure.microsoft.com/develop/php/).</span></span>

[BrokeredMessage]: /dotnet/api/microsoft.servicebus.messaging.brokeredmessage
[Queues, topics, and subscriptions]: service-bus-queues-topics-subscriptions.md
[require_once]: http://php.net/require_once


