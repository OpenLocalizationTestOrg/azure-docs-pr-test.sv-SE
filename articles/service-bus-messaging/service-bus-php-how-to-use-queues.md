---
title: "aaaHow toouse Service Bus-köer med PHP | Microsoft Docs"
description: "Lär dig hur toouse Service Bus köer i Azure. Kodexempel som skrivits i PHP."
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
ms.openlocfilehash: 8cf233176029b679d172eaf713632087beca5e4e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="how-toouse-service-bus-queues-with-php"></a><span data-ttu-id="d7b41-104">Hur toouse Service Bus köer med PHP</span><span class="sxs-lookup"><span data-stu-id="d7b41-104">How toouse Service Bus queues with PHP</span></span>
[!INCLUDE [service-bus-selector-queues](../../includes/service-bus-selector-queues.md)]

<span data-ttu-id="d7b41-105">Den här guiden visar hur toouse Service Bus-köer.</span><span class="sxs-lookup"><span data-stu-id="d7b41-105">This guide shows you how toouse Service Bus queues.</span></span> <span data-ttu-id="d7b41-106">hello exemplen är skrivna i PHP och använder hello [Azure SDK för PHP](../php-download-sdk.md).</span><span class="sxs-lookup"><span data-stu-id="d7b41-106">hello samples are written in PHP and use hello [Azure SDK for PHP](../php-download-sdk.md).</span></span> <span data-ttu-id="d7b41-107">hello beskrivs scenarier där **skapa köer**, **skicka och ta emot meddelanden**, och **tar bort köer**.</span><span class="sxs-lookup"><span data-stu-id="d7b41-107">hello scenarios covered include **creating queues**, **sending and receiving messages**, and **deleting queues**.</span></span>

[!INCLUDE [howto-service-bus-queues](../../includes/howto-service-bus-queues.md)]

## <a name="create-a-php-application"></a><span data-ttu-id="d7b41-108">Skapa en PHP-program</span><span class="sxs-lookup"><span data-stu-id="d7b41-108">Create a PHP application</span></span>
<span data-ttu-id="d7b41-109">Hej krav för att skapa en PHP-program som ansluter till hello Azure Blob-tjänsten är hello refererar till klasser i hello [Azure SDK för PHP](../php-download-sdk.md) från inom din kod.</span><span class="sxs-lookup"><span data-stu-id="d7b41-109">hello only requirement for creating a PHP application that accesses hello Azure Blob service is hello referencing of classes in hello [Azure SDK for PHP](../php-download-sdk.md) from within your code.</span></span> <span data-ttu-id="d7b41-110">Du kan använda alla development tools toocreate programmet eller anteckningar.</span><span class="sxs-lookup"><span data-stu-id="d7b41-110">You can use any development tools toocreate your application, or Notepad.</span></span>

> [!NOTE]
> <span data-ttu-id="d7b41-111">PHP-installation måste också ha hello [OpenSSL tillägget](http://php.net/openssl) installerat och aktiverat.</span><span class="sxs-lookup"><span data-stu-id="d7b41-111">Your PHP installation must also have hello [OpenSSL extension](http://php.net/openssl) installed and enabled.</span></span>
> 
> 

<span data-ttu-id="d7b41-112">I den här guiden använder du tjänstens funktioner som kan anropas från ett PHP-program lokalt eller i koden körs i en Azure-webbroll, en arbetsroll eller en webbplats.</span><span class="sxs-lookup"><span data-stu-id="d7b41-112">In this guide, you will use service features which can be called from within a PHP application locally, or in code running within an Azure web role, worker role, or website.</span></span>

## <a name="get-hello-azure-client-libraries"></a><span data-ttu-id="d7b41-113">Hämta hello Azure klientbibliotek</span><span class="sxs-lookup"><span data-stu-id="d7b41-113">Get hello Azure client libraries</span></span>
[!INCLUDE [get-client-libraries](../../includes/get-client-libraries.md)]

## <a name="configure-your-application-toouse-service-bus"></a><span data-ttu-id="d7b41-114">Konfigurera ditt program toouse Service Bus</span><span class="sxs-lookup"><span data-stu-id="d7b41-114">Configure your application toouse Service Bus</span></span>
<span data-ttu-id="d7b41-115">toouse hello Service Bus-kö API: er, hello följande:</span><span class="sxs-lookup"><span data-stu-id="d7b41-115">toouse hello Service Bus queue APIs, do hello following:</span></span>

1. <span data-ttu-id="d7b41-116">Referens hello bandladdaren fil med hello [require_once] [ require_once] instruktionen.</span><span class="sxs-lookup"><span data-stu-id="d7b41-116">Reference hello autoloader file using hello [require_once][require_once] statement.</span></span>
2. <span data-ttu-id="d7b41-117">Referera till alla klasser som du kan använda.</span><span class="sxs-lookup"><span data-stu-id="d7b41-117">Reference any classes you might use.</span></span>

<span data-ttu-id="d7b41-118">hello följande exempel visas hur tooinclude hello bandladdaren fil- och hello `ServicesBuilder` klass.</span><span class="sxs-lookup"><span data-stu-id="d7b41-118">hello following example shows how tooinclude hello autoloader file and reference hello `ServicesBuilder` class.</span></span>

> [!NOTE]
> <span data-ttu-id="d7b41-119">Det här exemplet (och andra exempel i den här artikeln) förutsätter att du har installerat hello PHP-klientbibliotek för Azure via Composer.</span><span class="sxs-lookup"><span data-stu-id="d7b41-119">This example (and other examples in this article) assumes you have installed hello PHP Client Libraries for Azure via Composer.</span></span> <span data-ttu-id="d7b41-120">Om du har installerat hello bibliotek manuellt eller som ett PÄRONTRÄD paket måste du referera hello **WindowsAzure.php** bandladdaren filen.</span><span class="sxs-lookup"><span data-stu-id="d7b41-120">If you installed hello libraries manually or as a PEAR package, you must reference hello **WindowsAzure.php** autoloader file.</span></span>
> 
> 

```php
require_once 'vendor/autoload.php';
use WindowsAzure\Common\ServicesBuilder;
```

<span data-ttu-id="d7b41-121">I hello exemplen nedan hello `require_once` instruktionen visas alltid, men bara hello klasser krävs för hello exempel tooexecute refererar till.</span><span class="sxs-lookup"><span data-stu-id="d7b41-121">In hello examples below, hello `require_once` statement will always be shown, but only hello classes necessary for hello example tooexecute are referenced.</span></span>

## <a name="set-up-a-service-bus-connection"></a><span data-ttu-id="d7b41-122">Konfigurera en Service Bus-anslutning</span><span class="sxs-lookup"><span data-stu-id="d7b41-122">Set up a Service Bus connection</span></span>
<span data-ttu-id="d7b41-123">tooinstantiate en Service Bus-klient, måste du först ha en giltig anslutningssträng i det här formatet:</span><span class="sxs-lookup"><span data-stu-id="d7b41-123">tooinstantiate a Service Bus client, you must first have a valid connection string in this format:</span></span>

```
Endpoint=[yourEndpoint];SharedAccessKeyName=RootManageSharedAccessKey;SharedAccessKey=[Primary Key]
```

<span data-ttu-id="d7b41-124">Där `Endpoint` är vanligtvis av hello format `[yourNamespace].servicebus.windows.net`.</span><span class="sxs-lookup"><span data-stu-id="d7b41-124">Where `Endpoint` is typically of hello format `[yourNamespace].servicebus.windows.net`.</span></span>

<span data-ttu-id="d7b41-125">toocreate alla Azure-tjänst-klienter måste du använda hello `ServicesBuilder` klass.</span><span class="sxs-lookup"><span data-stu-id="d7b41-125">toocreate any Azure service client you must use hello `ServicesBuilder` class.</span></span> <span data-ttu-id="d7b41-126">Du kan:</span><span class="sxs-lookup"><span data-stu-id="d7b41-126">You can:</span></span>

* <span data-ttu-id="d7b41-127">Skicka hello anslutning direkt string tooit.</span><span class="sxs-lookup"><span data-stu-id="d7b41-127">Pass hello connection string directly tooit.</span></span>
* <span data-ttu-id="d7b41-128">Använd hello **CloudConfigurationManager (CCM)** toocheck flera externa datakällor för hello anslutningssträngen:</span><span class="sxs-lookup"><span data-stu-id="d7b41-128">Use hello **CloudConfigurationManager (CCM)** toocheck multiple external sources for hello connection string:</span></span>
  * <span data-ttu-id="d7b41-129">Som standard levereras med stöd för en extern källa - miljövariabler</span><span class="sxs-lookup"><span data-stu-id="d7b41-129">By default it comes with support for one external source - environmental variables</span></span>
  * <span data-ttu-id="d7b41-130">Du kan lägga till nya källor genom att utöka hello `ConnectionStringSource` klass</span><span class="sxs-lookup"><span data-stu-id="d7b41-130">You can add new sources by extending hello `ConnectionStringSource` class</span></span>

<span data-ttu-id="d7b41-131">Hello-exempel som beskrivs här skickas hello anslutningssträngen direkt.</span><span class="sxs-lookup"><span data-stu-id="d7b41-131">For hello examples outlined here, hello connection string is passed directly.</span></span>

```php
require_once 'vendor/autoload.php';

use WindowsAzure\Common\ServicesBuilder;

$connectionString = "Endpoint=[yourEndpoint];SharedAccessKeyName=RootManageSharedAccessKey;SharedAccessKey=[Primary Key]";

$serviceBusRestProxy = ServicesBuilder::getInstance()->createServiceBusService($connectionString);
```

## <a name="create-a-queue"></a><span data-ttu-id="d7b41-132">Skapa en kö</span><span class="sxs-lookup"><span data-stu-id="d7b41-132">Create a queue</span></span>
<span data-ttu-id="d7b41-133">Du kan utföra hanteringsåtgärder för Service Bus-köer via hello `ServiceBusRestProxy` klass.</span><span class="sxs-lookup"><span data-stu-id="d7b41-133">You can perform management operations for Service Bus queues via hello `ServiceBusRestProxy` class.</span></span> <span data-ttu-id="d7b41-134">En `ServiceBusRestProxy` objektet har skapats via hello `ServicesBuilder::createServiceBusService` fabriksmetoden med en lämplig anslutningssträngen som kapslar in hello token behörigheter toomanage den.</span><span class="sxs-lookup"><span data-stu-id="d7b41-134">A `ServiceBusRestProxy` object is constructed via hello `ServicesBuilder::createServiceBusService` factory method with an appropriate connection string that encapsulates hello token permissions toomanage it.</span></span>

<span data-ttu-id="d7b41-135">följande exempel visar hur hello tooinstantiate en `ServiceBusRestProxy` och anropa `ServiceBusRestProxy->createQueue` toocreate en kö med namnet `myqueue` inom en `MySBNamespace` namnområde för tjänsten:</span><span class="sxs-lookup"><span data-stu-id="d7b41-135">hello following example shows how tooinstantiate a `ServiceBusRestProxy` and call `ServiceBusRestProxy->createQueue` toocreate a queue named `myqueue` within a `MySBNamespace` service namespace:</span></span>

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
> <span data-ttu-id="d7b41-136">Du kan använda hello `listQueues` metod på `ServiceBusRestProxy` objekt toocheck om en kö med ett angivet namn finns redan i ett namnområde.</span><span class="sxs-lookup"><span data-stu-id="d7b41-136">You can use hello `listQueues` method on `ServiceBusRestProxy` objects toocheck if a queue with a specified name already exists within a namespace.</span></span>
> 
> 

## <a name="send-messages-tooa-queue"></a><span data-ttu-id="d7b41-137">Skicka meddelanden tooa kön</span><span class="sxs-lookup"><span data-stu-id="d7b41-137">Send messages tooa queue</span></span>
<span data-ttu-id="d7b41-138">toosend en meddelandekö tooa Service Bus programmet anropar hello `ServiceBusRestProxy->sendQueueMessage` metod.</span><span class="sxs-lookup"><span data-stu-id="d7b41-138">toosend a message tooa Service Bus queue, your application calls hello `ServiceBusRestProxy->sendQueueMessage` method.</span></span> <span data-ttu-id="d7b41-139">Hej följande kod visar hur toosend ett meddelande toohello `myqueue` kö som tidigare har skapat i den `MySBNamespace` namnområde för tjänsten.</span><span class="sxs-lookup"><span data-stu-id="d7b41-139">hello following code shows how toosend a message toohello `myqueue` queue previously created within the `MySBNamespace` service namespace.</span></span>

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

<span data-ttu-id="d7b41-140">Meddelanden som skickats för (och tagits emot från) Service Bus-köer är instanser av hello [BrokeredMessage] [ BrokeredMessage] klass.</span><span class="sxs-lookup"><span data-stu-id="d7b41-140">Messages sent too(and received from ) Service Bus queues are instances of hello [BrokeredMessage][BrokeredMessage] class.</span></span> <span data-ttu-id="d7b41-141">[BrokeredMessage] [ BrokeredMessage] objekt har en uppsättning standard metoder och egenskaper som används toohold anpassade programspecifika egenskaper och en brödtext med godtyckliga programdata.</span><span class="sxs-lookup"><span data-stu-id="d7b41-141">[BrokeredMessage][BrokeredMessage] objects have a set of standard methods and properties that are used toohold custom application-specific properties, and a body of arbitrary application data.</span></span>

<span data-ttu-id="d7b41-142">Service Bus-köer stöder en maximal meddelandestorlek på 256 KB i hello [standardnivån](service-bus-premium-messaging.md) och 1 MB på hello [premiumnivån](service-bus-premium-messaging.md).</span><span class="sxs-lookup"><span data-stu-id="d7b41-142">Service Bus queues support a maximum message size of 256 KB in hello [Standard tier](service-bus-premium-messaging.md) and 1 MB in hello [Premium tier](service-bus-premium-messaging.md).</span></span> <span data-ttu-id="d7b41-143">hello-huvudet som innehåller hello standard- och anpassade programegenskaperna, kan ha en maximal storlek på 64 KB.</span><span class="sxs-lookup"><span data-stu-id="d7b41-143">hello header, which includes hello standard and custom application properties, can have a maximum size of 64 KB.</span></span> <span data-ttu-id="d7b41-144">Det finns ingen gräns hello antalet meddelanden som finns i en kö men det finns ett tak på hello totala storleken på hälsningsmeddelande som innehas av en kö.</span><span class="sxs-lookup"><span data-stu-id="d7b41-144">There is no limit on hello number of messages held in a queue but there is a cap on hello total size of hello messages held by a queue.</span></span> <span data-ttu-id="d7b41-145">Den här övre gräns för storleken är 5 GB.</span><span class="sxs-lookup"><span data-stu-id="d7b41-145">This upper limit on queue size is 5 GB.</span></span>

## <a name="receive-messages-from-a-queue"></a><span data-ttu-id="d7b41-146">Ta emot meddelanden från en kö</span><span class="sxs-lookup"><span data-stu-id="d7b41-146">Receive messages from a queue</span></span>

<span data-ttu-id="d7b41-147">hello bästa sätt tooreceive meddelanden från en kö är toouse en `ServiceBusRestProxy->receiveQueueMessage` metod.</span><span class="sxs-lookup"><span data-stu-id="d7b41-147">hello best way tooreceive messages from a queue is toouse a `ServiceBusRestProxy->receiveQueueMessage` method.</span></span> <span data-ttu-id="d7b41-148">Meddelanden kan tas emot i två olika lägen: [ *ReceiveAndDelete* ](/dotnet/api/microsoft.servicebus.messaging.receivemode.receiveanddelete) och [ *PeekLock*](/dotnet/api/microsoft.servicebus.messaging.receivemode.peeklock).</span><span class="sxs-lookup"><span data-stu-id="d7b41-148">Messages can be received in two different modes: [*ReceiveAndDelete*](/dotnet/api/microsoft.servicebus.messaging.receivemode.receiveanddelete) and [*PeekLock*](/dotnet/api/microsoft.servicebus.messaging.receivemode.peeklock).</span></span> <span data-ttu-id="d7b41-149">**PeekLock** är hello som standard.</span><span class="sxs-lookup"><span data-stu-id="d7b41-149">**PeekLock** is hello default.</span></span>

<span data-ttu-id="d7b41-150">När du använder [ReceiveAndDelete](/dotnet/api/microsoft.servicebus.messaging.receivemode.receiveanddelete) läge får är en engångsåtgärd, det vill säga när Service Bus tar emot en läsbegäran för ett meddelande i en kö, den markerar hello meddelandet som Förbrukat och returnerar det toohello program.</span><span class="sxs-lookup"><span data-stu-id="d7b41-150">When using [ReceiveAndDelete](/dotnet/api/microsoft.servicebus.messaging.receivemode.receiveanddelete) mode, receive is a single-shot operation; that is, when Service Bus receives a read request for a message in a queue, it marks hello message as being consumed and returns it toohello application.</span></span> <span data-ttu-id="d7b41-151">[ReceiveAndDelete](/dotnet/api/microsoft.servicebus.messaging.receivemode.receiveanddelete) läge är hello enklaste modellen och fungerar bäst för scenarier där ett program kan tolerera icke-bearbetning av ett meddelande i hello händelse av fel.</span><span class="sxs-lookup"><span data-stu-id="d7b41-151">[ReceiveAndDelete](/dotnet/api/microsoft.servicebus.messaging.receivemode.receiveanddelete) mode is hello simplest model and works best for scenarios in which an application can tolerate not processing a message in hello event of a failure.</span></span> <span data-ttu-id="d7b41-152">toounderstand, Föreställ i vilka hello konsumenten problem hello mottagning av begäran och sedan kraschar innan bearbetningen.</span><span class="sxs-lookup"><span data-stu-id="d7b41-152">toounderstand this, consider a scenario in which hello consumer issues hello receive request and then crashes before processing it.</span></span> <span data-ttu-id="d7b41-153">Eftersom Service Bus kommer att ha markerat hello meddelandet som Förbrukat, sedan när hello programmet startas om och börjar förbruka meddelanden igen, att ha missat hello-meddelande som har förbrukats tidigare toohello krascher.</span><span class="sxs-lookup"><span data-stu-id="d7b41-153">Because Service Bus will have marked hello message as being consumed, then when hello application restarts and begins consuming messages again, it will have missed hello message that was consumed prior toohello crash.</span></span>

<span data-ttu-id="d7b41-154">I hello standard [PeekLock](/dotnet/api/microsoft.servicebus.messaging.receivemode.peeklock) läge, ett meddelande blir en åtgärd i två steg, vilket gör det möjligt toosupport program som inte tolererar att ett meddelande saknas.</span><span class="sxs-lookup"><span data-stu-id="d7b41-154">In hello default [PeekLock](/dotnet/api/microsoft.servicebus.messaging.receivemode.peeklock) mode, receiving a message becomes a two stage operation, which makes it possible toosupport applications that cannot tolerate missing messages.</span></span> <span data-ttu-id="d7b41-155">När Service Bus tar emot en begäran, den hittar hello nästa meddelande toobe förbrukas, låser det tooprevent andra användare tar emot den, och returnerar sedan toohello program.</span><span class="sxs-lookup"><span data-stu-id="d7b41-155">When Service Bus receives a request, it finds hello next message toobe consumed, locks it tooprevent other consumers from receiving it, and then returns it toohello application.</span></span> <span data-ttu-id="d7b41-156">När hello programmet har avslutat bearbetningen hello-meddelande (eller lagrar det på ett tillförlitligt sätt för framtida bearbetning), Slutför hello andra steget i hello får processen genom att skicka emot hälsningsmeddelande för`ServiceBusRestProxy->deleteMessage`.</span><span class="sxs-lookup"><span data-stu-id="d7b41-156">After hello application finishes processing hello message (or stores it reliably for future processing), it completes hello second stage of hello receive process by passing hello received message too`ServiceBusRestProxy->deleteMessage`.</span></span> <span data-ttu-id="d7b41-157">När Service Bus ser hello `deleteMessage` anropet, den Markera hello meddelandet som Förbrukat och ta bort den från hello kön.</span><span class="sxs-lookup"><span data-stu-id="d7b41-157">When Service Bus sees hello `deleteMessage` call, it will mark hello message as being consumed and remove it from hello queue.</span></span>

<span data-ttu-id="d7b41-158">följande exempel visar hur hello tooreceive och bearbeta ett meddelande med hjälp av [PeekLock](/dotnet/api/microsoft.servicebus.messaging.receivemode.peeklock) läge (hello standardläget).</span><span class="sxs-lookup"><span data-stu-id="d7b41-158">hello following example shows how tooreceive and process a message using [PeekLock](/dotnet/api/microsoft.servicebus.messaging.receivemode.peeklock) mode (hello default mode).</span></span>

```php
require_once 'vendor/autoload.php';

use WindowsAzure\Common\ServicesBuilder;
use WindowsAzure\Common\ServiceException;
use WindowsAzure\ServiceBus\Models\ReceiveMessageOptions;

// Create Service Bus REST proxy.
$serviceBusRestProxy = ServicesBuilder::getInstance()->createServiceBusService($connectionString);

try    {
    // Set hello receive mode tooPeekLock (default is ReceiveAndDelete).
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

## <a name="how-toohandle-application-crashes-and-unreadable-messages"></a><span data-ttu-id="d7b41-159">Hur toohandle programmet kraschar och oläsbara meddelanden</span><span class="sxs-lookup"><span data-stu-id="d7b41-159">How toohandle application crashes and unreadable messages</span></span>

<span data-ttu-id="d7b41-160">Service Bus innehåller funktioner toohelp du att återställa fel i programmet eller problem med bearbetning av ett meddelande.</span><span class="sxs-lookup"><span data-stu-id="d7b41-160">Service Bus provides functionality toohelp you gracefully recover from errors in your application or difficulties processing a message.</span></span> <span data-ttu-id="d7b41-161">Om ett mottagarprogram inte kan tooprocess hello meddelande av någon anledning, så kan det anropa hello `unlockMessage` metod för hello tog emot meddelande (i stället för hello `deleteMessage` metod).</span><span class="sxs-lookup"><span data-stu-id="d7b41-161">If a receiver application is unable tooprocess hello message for some reason, then it can call hello `unlockMessage` method on hello received message (instead of hello `deleteMessage` method).</span></span> <span data-ttu-id="d7b41-162">Detta orsakar Service Bus toounlock hello-meddelande i kön för hello och göra den tillgänglig toobe tas emot igen, antingen hello av samma förbrukning av ett program eller ett annat användningsprogram.</span><span class="sxs-lookup"><span data-stu-id="d7b41-162">This will cause Service Bus toounlock hello message within hello queue and make it available toobe received again, either by hello same consuming application or by another consuming application.</span></span>

<span data-ttu-id="d7b41-163">Det finns också en tidsgräns som är associerad med ett meddelande i kön hello och om hello program inte tooprocess hello-meddelande innan timeout för lås hello går ut (till exempel om hello programmet kraschar), kommer Service Bus att låsa upp hello-meddelande automatiskt och gör den tillgänglig toobe tas emot igen.</span><span class="sxs-lookup"><span data-stu-id="d7b41-163">There is also a timeout associated with a message locked within hello queue, and if hello application fails tooprocess hello message before hello lock timeout expires (for example, if hello application crashes), then Service Bus will unlock hello message automatically and make it available toobe received again.</span></span>

<span data-ttu-id="d7b41-164">Hello händelse som hello programmet kraschar efter hello meddelandet har bearbetats men innan hello `deleteMessage` begäran utfärdas och sedan hello-meddelande kommer att levereras på nytt toohello program när den startas om.</span><span class="sxs-lookup"><span data-stu-id="d7b41-164">In hello event that hello application crashes after processing hello message but before hello `deleteMessage` request is issued, then hello message will be redelivered toohello application when it restarts.</span></span> <span data-ttu-id="d7b41-165">Det här kallas ofta *minst när* bearbetning, det vill säga varje meddelande bearbetas minst en gång men i vissa situationer hello samma meddelande levereras.</span><span class="sxs-lookup"><span data-stu-id="d7b41-165">This is often called *At Least Once* processing; that is, each message is processed at least once but in certain situations hello same message may be redelivered.</span></span> <span data-ttu-id="d7b41-166">Om hello scenariot inte tolererar duplicerad bearbetning och sedan lägga till ytterligare rekommenderas logik tooapplications toohandle duplicerad meddelandeleverans.</span><span class="sxs-lookup"><span data-stu-id="d7b41-166">If hello scenario cannot tolerate duplicate processing, then adding additional logic tooapplications toohandle duplicate message delivery is recommended.</span></span> <span data-ttu-id="d7b41-167">Detta uppnås ofta med hjälp av hello `getMessageId` metod i hello-meddelande, förblir konstant under alla leveransförsök.</span><span class="sxs-lookup"><span data-stu-id="d7b41-167">This is often achieved using hello `getMessageId` method of hello message, which remains constant across delivery attempts.</span></span>

## <a name="next-steps"></a><span data-ttu-id="d7b41-168">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="d7b41-168">Next steps</span></span>
<span data-ttu-id="d7b41-169">Nu när du har lärt dig grunderna hello i Service Bus-köer, se [köer, ämnen och prenumerationer] [ Queues, topics, and subscriptions] för mer information.</span><span class="sxs-lookup"><span data-stu-id="d7b41-169">Now that you've learned hello basics of Service Bus queues, see [Queues, topics, and subscriptions][Queues, topics, and subscriptions] for more information.</span></span>

<span data-ttu-id="d7b41-170">Mer information finns också hello [PHP Developer Center](https://azure.microsoft.com/develop/php/).</span><span class="sxs-lookup"><span data-stu-id="d7b41-170">For more information, also visit hello [PHP Developer Center](https://azure.microsoft.com/develop/php/).</span></span>

[BrokeredMessage]: /dotnet/api/microsoft.servicebus.messaging.brokeredmessage
[Queues, topics, and subscriptions]: service-bus-queues-topics-subscriptions.md
[require_once]: http://php.net/require_once


