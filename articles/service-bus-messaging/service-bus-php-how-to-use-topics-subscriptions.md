---
title: "aaaHow toouse Service Bus-ämnen med PHP | Microsoft Docs"
description: "Lär dig hur toouse Service Bus-ämnen med PHP i Azure."
services: service-bus-messaging
documentationcenter: php
author: sethmanheim
manager: timlt
editor: 
ms.assetid: faaa4bbd-f6ef-42ff-aca7-fc4353976449
ms.service: service-bus-messaging
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: PHP
ms.topic: article
ms.date: 04/27/2017
ms.author: sethm
ms.openlocfilehash: 0ca8625fa3edc5854c0d6c1c2f6adab6a2d42f91
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="how-toouse-service-bus-topics-and-subscriptions-with-php"></a><span data-ttu-id="51102-103">Hur toouse Service Bus-ämnen och prenumerationer med PHP</span><span class="sxs-lookup"><span data-stu-id="51102-103">How toouse Service Bus topics and subscriptions with PHP</span></span>

[!INCLUDE [service-bus-selector-topics](../../includes/service-bus-selector-topics.md)]

<span data-ttu-id="51102-104">Den här artikeln beskrivs hur du toouse Service Bus-ämnen och prenumerationer.</span><span class="sxs-lookup"><span data-stu-id="51102-104">This article shows you how toouse Service Bus topics and subscriptions.</span></span> <span data-ttu-id="51102-105">hello exemplen är skrivna i PHP och använder hello [Azure SDK för PHP](../php-download-sdk.md).</span><span class="sxs-lookup"><span data-stu-id="51102-105">hello samples are written in PHP and use hello [Azure SDK for PHP](../php-download-sdk.md).</span></span> <span data-ttu-id="51102-106">hello beskrivs scenarier där **skapa ämnen och prenumerationer**, **skapa prenumerationsfilter**, **skickar meddelanden tooa avsnittet**, **tar emot meddelanden från en prenumeration**, och **ta bort ämnen och prenumerationer**.</span><span class="sxs-lookup"><span data-stu-id="51102-106">hello scenarios covered include **creating topics and subscriptions**, **creating subscription filters**, **sending messages tooa topic**, **receiving messages from a subscription**, and **deleting topics and subscriptions**.</span></span>

[!INCLUDE [howto-service-bus-topics](../../includes/howto-service-bus-topics.md)]

## <a name="create-a-php-application"></a><span data-ttu-id="51102-107">Skapa en PHP-program</span><span class="sxs-lookup"><span data-stu-id="51102-107">Create a PHP application</span></span>
<span data-ttu-id="51102-108">Hej krav för att skapa en PHP-program som ansluter till hello Azure Blob-tjänsten är tooreference klasser i hello [Azure SDK för PHP](../php-download-sdk.md) från inom din kod.</span><span class="sxs-lookup"><span data-stu-id="51102-108">hello only requirement for creating a PHP application that accesses hello Azure Blob service is tooreference classes in hello [Azure SDK for PHP](../php-download-sdk.md) from within your code.</span></span> <span data-ttu-id="51102-109">Du kan använda alla development tools toocreate programmet eller anteckningar.</span><span class="sxs-lookup"><span data-stu-id="51102-109">You can use any development tools toocreate your application, or Notepad.</span></span>

> [!NOTE]
> <span data-ttu-id="51102-110">PHP-installation måste också ha hello [OpenSSL tillägget](http://php.net/openssl) installerat och aktiverat.</span><span class="sxs-lookup"><span data-stu-id="51102-110">Your PHP installation must also have hello [OpenSSL extension](http://php.net/openssl) installed and enabled.</span></span>
> 
> 

<span data-ttu-id="51102-111">Den här artikeln beskriver hur toouse tjänsten funktioner som kan anropas i en PHP-program lokalt eller i koden körs i en Azure-webbroll, en arbetsroll eller en webbplats.</span><span class="sxs-lookup"><span data-stu-id="51102-111">This article describes how toouse service features that can be called within a PHP application locally, or in code running within an Azure web role, worker role, or website.</span></span>

## <a name="get-hello-azure-client-libraries"></a><span data-ttu-id="51102-112">Hämta hello Azure klientbibliotek</span><span class="sxs-lookup"><span data-stu-id="51102-112">Get hello Azure client libraries</span></span>
[!INCLUDE [get-client-libraries](../../includes/get-client-libraries.md)]

## <a name="configure-your-application-toouse-service-bus"></a><span data-ttu-id="51102-113">Konfigurera ditt program toouse Service Bus</span><span class="sxs-lookup"><span data-stu-id="51102-113">Configure your application toouse Service Bus</span></span>
<span data-ttu-id="51102-114">toouse hello Service Bus-API: er:</span><span class="sxs-lookup"><span data-stu-id="51102-114">toouse hello Service Bus APIs:</span></span>

1. <span data-ttu-id="51102-115">Referens hello bandladdaren fil med hello [require_once] [ require-once] instruktionen.</span><span class="sxs-lookup"><span data-stu-id="51102-115">Reference hello autoloader file using hello [require_once][require-once] statement.</span></span>
2. <span data-ttu-id="51102-116">Referera till alla klasser som du kan använda.</span><span class="sxs-lookup"><span data-stu-id="51102-116">Reference any classes you might use.</span></span>

<span data-ttu-id="51102-117">hello följande exempel visas hur tooinclude hello bandladdaren fil- och hello **ServiceBusService** klass.</span><span class="sxs-lookup"><span data-stu-id="51102-117">hello following example shows how tooinclude hello autoloader file and reference hello **ServiceBusService** class.</span></span>

> [!NOTE]
> <span data-ttu-id="51102-118">Det här exemplet (och andra exempel i den här artikeln) förutsätter att du har installerat hello PHP-klientbibliotek för Azure via Composer.</span><span class="sxs-lookup"><span data-stu-id="51102-118">This example (and other examples in this article) assumes you have installed hello PHP Client Libraries for Azure via Composer.</span></span> <span data-ttu-id="51102-119">Om du har installerat hello bibliotek manuellt eller som ett PÄRONTRÄD paket måste du referera hello **WindowsAzure.php** bandladdaren filen.</span><span class="sxs-lookup"><span data-stu-id="51102-119">If you installed hello libraries manually or as a PEAR package, you must reference hello **WindowsAzure.php** autoloader file.</span></span>
> 
> 

```php
require_once 'vendor\autoload.php';
use WindowsAzure\Common\ServicesBuilder;
```

<span data-ttu-id="51102-120">I följande exempel hello, hello `require_once` instruktionen visas alltid, men bara hello klasser krävs för hello exempel tooexecute refererar till.</span><span class="sxs-lookup"><span data-stu-id="51102-120">In hello following examples, hello `require_once` statement will always be shown, but only hello classes necessary for hello example tooexecute are referenced.</span></span>

## <a name="set-up-a-service-bus-connection"></a><span data-ttu-id="51102-121">Konfigurera en Service Bus-anslutning</span><span class="sxs-lookup"><span data-stu-id="51102-121">Set up a Service Bus connection</span></span>
<span data-ttu-id="51102-122">tooinstantiate en Service Bus-klient måste du först ha en giltig anslutningssträng i det här formatet:</span><span class="sxs-lookup"><span data-stu-id="51102-122">tooinstantiate a Service Bus client you must first have a valid connection string in this format:</span></span>

```
Endpoint=[yourEndpoint];SharedAccessKeyName=RootManageSharedAccessKey;SharedAccessKey=[Primary Key]
```

<span data-ttu-id="51102-123">Där `Endpoint` är vanligtvis av hello format `https://[yourNamespace].servicebus.windows.net`.</span><span class="sxs-lookup"><span data-stu-id="51102-123">Where `Endpoint` is typically of hello format `https://[yourNamespace].servicebus.windows.net`.</span></span>

<span data-ttu-id="51102-124">toocreate alla Azure-tjänst-klienter måste du använda hello `ServicesBuilder` klass.</span><span class="sxs-lookup"><span data-stu-id="51102-124">toocreate any Azure service client you must use hello `ServicesBuilder` class.</span></span> <span data-ttu-id="51102-125">Du kan:</span><span class="sxs-lookup"><span data-stu-id="51102-125">You can:</span></span>

* <span data-ttu-id="51102-126">Skicka hello anslutning direkt string tooit.</span><span class="sxs-lookup"><span data-stu-id="51102-126">Pass hello connection string directly tooit.</span></span>
* <span data-ttu-id="51102-127">Använd hello **CloudConfigurationManager (CCM)** toocheck flera externa datakällor för hello anslutningssträngen:</span><span class="sxs-lookup"><span data-stu-id="51102-127">Use hello **CloudConfigurationManager (CCM)** toocheck multiple external sources for hello connection string:</span></span>
  * <span data-ttu-id="51102-128">Som standard levereras den med stöd för en extern källa - miljövariabler.</span><span class="sxs-lookup"><span data-stu-id="51102-128">By default it comes with support for one external source - environmental variables.</span></span>
  * <span data-ttu-id="51102-129">Du kan lägga till nya källor genom att utöka hello `ConnectionStringSource` klass.</span><span class="sxs-lookup"><span data-stu-id="51102-129">You can add new sources by extending hello `ConnectionStringSource` class.</span></span>

<span data-ttu-id="51102-130">Hello-exempel som beskrivs här skickas hello anslutningssträngen direkt.</span><span class="sxs-lookup"><span data-stu-id="51102-130">For hello examples outlined here, hello connection string is passed directly.</span></span>

```php
require_once 'vendor/autoload.php';

use WindowsAzure\Common\ServicesBuilder;

$connectionString = "Endpoint=[yourEndpoint];SharedAccessKeyName=RootManageSharedAccessKey;SharedAccessKey=[Primary Key]";

$serviceBusRestProxy = ServicesBuilder::getInstance()->createServiceBusService($connectionString);
```

## <a name="create-a-topic"></a><span data-ttu-id="51102-131">Skapa ett ämne</span><span class="sxs-lookup"><span data-stu-id="51102-131">Create a topic</span></span>
<span data-ttu-id="51102-132">Du kan utföra hanteringsåtgärder för Service Bus-ämnen via hello `ServiceBusRestProxy` klass.</span><span class="sxs-lookup"><span data-stu-id="51102-132">You can perform management operations for Service Bus topics via hello `ServiceBusRestProxy` class.</span></span> <span data-ttu-id="51102-133">En `ServiceBusRestProxy` objektet har skapats via hello `ServicesBuilder::createServiceBusService` fabriksmetoden med en lämplig anslutningssträngen som kapslar in hello token behörigheter toomanage den.</span><span class="sxs-lookup"><span data-stu-id="51102-133">A `ServiceBusRestProxy` object is constructed via hello `ServicesBuilder::createServiceBusService` factory method with an appropriate connection string that encapsulates hello token permissions toomanage it.</span></span>

<span data-ttu-id="51102-134">följande exempel visar hur hello tooinstantiate en `ServiceBusRestProxy` och anropa `ServiceBusRestProxy->createTopic` toocreate ett ämne med namnet `mytopic` inom en `MySBNamespace` namnområde:</span><span class="sxs-lookup"><span data-stu-id="51102-134">hello following example shows how tooinstantiate a `ServiceBusRestProxy` and call `ServiceBusRestProxy->createTopic` toocreate a topic named `mytopic` within a `MySBNamespace` namespace:</span></span>

```php
require_once 'vendor/autoload.php';

use WindowsAzure\Common\ServicesBuilder;
use WindowsAzure\Common\ServiceException;
use WindowsAzure\ServiceBus\Models\TopicInfo;

// Create Service Bus REST proxy.
$serviceBusRestProxy = ServicesBuilder::getInstance()->createServiceBusService($connectionString);

try    {        
    // Create topic.
    $topicInfo = new TopicInfo("mytopic");
    $serviceBusRestProxy->createTopic($topicInfo);
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
> <span data-ttu-id="51102-135">Du kan använda hello `listTopics` metod på `ServiceBusRestProxy` objekt toocheck om ett ämne med ett angivet namn finns redan i ett namnområde för tjänsten.</span><span class="sxs-lookup"><span data-stu-id="51102-135">You can use hello `listTopics` method on `ServiceBusRestProxy` objects toocheck if a topic with a specified name already exists within a service namespace.</span></span>
> 
> 

## <a name="create-a-subscription"></a><span data-ttu-id="51102-136">Skapa en prenumeration</span><span class="sxs-lookup"><span data-stu-id="51102-136">Create a subscription</span></span>
<span data-ttu-id="51102-137">Ämnesprenumerationer skapas också med hello `ServiceBusRestProxy->createSubscription` metod.</span><span class="sxs-lookup"><span data-stu-id="51102-137">Topic subscriptions are also created with hello `ServiceBusRestProxy->createSubscription` method.</span></span> <span data-ttu-id="51102-138">Prenumerationer är namngivna och kan ha ett valfritt filter som begränsar hello uppsättning av meddelanden som skickas toohello prenumerationens virtuella kö.</span><span class="sxs-lookup"><span data-stu-id="51102-138">Subscriptions are named and can have an optional filter that restricts hello set of messages passed toohello subscription's virtual queue.</span></span>

### <a name="create-a-subscription-with-hello-default-matchall-filter"></a><span data-ttu-id="51102-139">Skapa en prenumeration med hello standardfiltret (MatchAll)</span><span class="sxs-lookup"><span data-stu-id="51102-139">Create a subscription with hello default (MatchAll) filter</span></span>
<span data-ttu-id="51102-140">Hej **MatchAll** filtret är hello standardfilter som används om inget filter anges när en ny prenumeration skapas.</span><span class="sxs-lookup"><span data-stu-id="51102-140">hello **MatchAll** filter is hello default filter that is used if no filter is specified when a new subscription is created.</span></span> <span data-ttu-id="51102-141">När hello **MatchAll** filter används, alla meddelanden publicerade toohello avsnittet placeras i hello prenumerationens virtuella kö.</span><span class="sxs-lookup"><span data-stu-id="51102-141">When hello **MatchAll** filter is used, all messages published toohello topic are placed in hello subscription's virtual queue.</span></span> <span data-ttu-id="51102-142">hello följande exempel skapas en prenumeration med namnet 'mysubscription' och använder hello standard **MatchAll** filter.</span><span class="sxs-lookup"><span data-stu-id="51102-142">hello following example creates a subscription named 'mysubscription' and uses hello default **MatchAll** filter.</span></span>

```php
require_once 'vendor/autoload.php';

use WindowsAzure\Common\ServicesBuilder;
use WindowsAzure\Common\ServiceException;
use WindowsAzure\ServiceBus\Models\SubscriptionInfo;

// Create Service Bus REST proxy.
$serviceBusRestProxy = ServicesBuilder::getInstance()->createServiceBusService($connectionString);

try    {
    // Create subscription.
    $subscriptionInfo = new SubscriptionInfo("mysubscription");
    $serviceBusRestProxy->createSubscription("mytopic", $subscriptionInfo);
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

### <a name="create-subscriptions-with-filters"></a><span data-ttu-id="51102-143">Skapa prenumerationer med filter</span><span class="sxs-lookup"><span data-stu-id="51102-143">Create subscriptions with filters</span></span>
<span data-ttu-id="51102-144">Du kan också ställa in filter som gör att du toospecify vilka meddelanden som skickas tooa avsnittet ska visas inom en viss ämnesprenumeration.</span><span class="sxs-lookup"><span data-stu-id="51102-144">You can also set up filters that enable you toospecify which messages sent tooa topic should appear within a specific topic subscription.</span></span> <span data-ttu-id="51102-145">hello mest flexibla typen av filter som stöds av prenumerationerna är hello [SqlFilter](/dotnet/api/microsoft.servicebus.messaging.sqlfilter#microsoft_servicebus_messaging_sqlfilter), som implementerar en deluppsättning av SQL92.</span><span class="sxs-lookup"><span data-stu-id="51102-145">hello most flexible type of filter supported by subscriptions is hello [SqlFilter](/dotnet/api/microsoft.servicebus.messaging.sqlfilter#microsoft_servicebus_messaging_sqlfilter), which implements a subset of SQL92.</span></span> <span data-ttu-id="51102-146">SQL-filter tillämpas för hälsningsmeddelande som är publicerade toohello avsnittet hello egenskaper.</span><span class="sxs-lookup"><span data-stu-id="51102-146">SQL filters operate on hello properties of hello messages that are published toohello topic.</span></span> <span data-ttu-id="51102-147">Läs mer om SqlFilters [SqlFilter.SqlExpression egenskapen][sqlfilter].</span><span class="sxs-lookup"><span data-stu-id="51102-147">For more information about SqlFilters, see [SqlFilter.SqlExpression Property][sqlfilter].</span></span>

> [!NOTE]
> <span data-ttu-id="51102-148">Varje regel för en prenumeration bearbetar inkommande meddelanden oberoende av varandra, lägga till resultatet meddelanden toohello prenumerationen.</span><span class="sxs-lookup"><span data-stu-id="51102-148">Each rule on a subscription processes incoming messages independently, adding their result messages toohello subscription.</span></span> <span data-ttu-id="51102-149">Dessutom kan varje ny prenumeration har standard **regeln** objekt med ett filter som lägger till alla meddelanden från hello avsnittet toohello prenumeration.</span><span class="sxs-lookup"><span data-stu-id="51102-149">In addition, each new subscription has a default **Rule** object with a filter that adds all messages from hello topic toohello subscription.</span></span> <span data-ttu-id="51102-150">tooreceive endast meddelanden som matchar filtret, måste du ta bort hello Standardregeln.</span><span class="sxs-lookup"><span data-stu-id="51102-150">tooreceive only messages matching your filter, you must remove hello default rule.</span></span> <span data-ttu-id="51102-151">Du kan ta bort hello standardregel med hello `ServiceBusRestProxy->deleteRule` metod.</span><span class="sxs-lookup"><span data-stu-id="51102-151">You can remove hello default rule by using hello `ServiceBusRestProxy->deleteRule` method.</span></span>
> 
> 

<span data-ttu-id="51102-152">hello följande exempel skapas en prenumeration med namnet `HighMessages` med en **SqlFilter** som endast väljer meddelanden som har en anpassad `MessageNumber` egenskap som är större än 3.</span><span class="sxs-lookup"><span data-stu-id="51102-152">hello following example creates a subscription named `HighMessages` with a **SqlFilter** that only selects messages that have a custom `MessageNumber` property greater than 3.</span></span> <span data-ttu-id="51102-153">Se [skicka meddelanden tooa avsnittet](#send-messages-to-a-topic) information om att lägga till anpassade egenskaper toomessages.</span><span class="sxs-lookup"><span data-stu-id="51102-153">See [Send messages tooa topic](#send-messages-to-a-topic) for information about adding custom properties toomessages.</span></span>

```php
$subscriptionInfo = new SubscriptionInfo("HighMessages");
$serviceBusRestProxy->createSubscription("mytopic", $subscriptionInfo);

$serviceBusRestProxy->deleteRule("mytopic", "HighMessages", '$Default');

$ruleInfo = new RuleInfo("HighMessagesRule");
$ruleInfo->withSqlFilter("MessageNumber > 3");
$ruleResult = $serviceBusRestProxy->createRule("mytopic", "HighMessages", $ruleInfo);
```

<span data-ttu-id="51102-154">Observera att den här koden kräver hello användning av ett ytterligare namnområde: `WindowsAzure\ServiceBus\Models\SubscriptionInfo`.</span><span class="sxs-lookup"><span data-stu-id="51102-154">Note that this code requires hello use of an additional namespace: `WindowsAzure\ServiceBus\Models\SubscriptionInfo`.</span></span>

<span data-ttu-id="51102-155">På liknande sätt hello följande exempel skapas en prenumeration med namnet `LowMessages` med en `SqlFilter` som endast väljer meddelanden som har en `MessageNumber` egenskap som är mindre än eller lika med too3.</span><span class="sxs-lookup"><span data-stu-id="51102-155">Similarly, hello following example creates a subscription named `LowMessages` with a `SqlFilter` that only selects messages that have a `MessageNumber` property less than or equal too3.</span></span>

```php
$subscriptionInfo = new SubscriptionInfo("LowMessages");
$serviceBusRestProxy->createSubscription("mytopic", $subscriptionInfo);

$serviceBusRestProxy->deleteRule("mytopic", "LowMessages", '$Default');

$ruleInfo = new RuleInfo("LowMessagesRule");
$ruleInfo->withSqlFilter("MessageNumber <= 3");
$ruleResult = $serviceBusRestProxy->createRule("mytopic", "LowMessages", $ruleInfo);
```

<span data-ttu-id="51102-156">Nu när ett meddelande skickas toohello `mytopic` avsnittet levereras det alltid tooreceivers prenumererar toohello `mysubscription` prenumeration och levereras selektivt tooreceivers prenumererar toohello `HighMessages` och `LowMessages` prenumerationer ( beroende på hello meddelandeinnehåll).</span><span class="sxs-lookup"><span data-stu-id="51102-156">Now, when a message is sent toohello `mytopic` topic, it is always delivered tooreceivers subscribed toohello `mysubscription` subscription, and selectively delivered tooreceivers subscribed toohello `HighMessages` and `LowMessages` subscriptions (depending upon hello message content).</span></span>

## <a name="send-messages-tooa-topic"></a><span data-ttu-id="51102-157">Skicka meddelanden tooa avsnittet</span><span class="sxs-lookup"><span data-stu-id="51102-157">Send messages tooa topic</span></span>
<span data-ttu-id="51102-158">toosend meddelandet tooa Service Bus-ämne programmet anropar hello `ServiceBusRestProxy->sendTopicMessage` metod.</span><span class="sxs-lookup"><span data-stu-id="51102-158">toosend a message tooa Service Bus topic, your application calls hello `ServiceBusRestProxy->sendTopicMessage` method.</span></span> <span data-ttu-id="51102-159">Hej följande kod visar hur toosend ett meddelande toohello `mytopic` avsnittet som tidigare har skapat i den `MySBNamespace` namnområde för tjänsten.</span><span class="sxs-lookup"><span data-stu-id="51102-159">hello following code shows how toosend a message toohello `mytopic` topic previously created within the `MySBNamespace` service namespace.</span></span>

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
    $serviceBusRestProxy->sendTopicMessage("mytopic", $message);
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

<span data-ttu-id="51102-160">Meddelanden som skickas tooService Bus-ämnen är instanser av hello [BrokeredMessage] [ BrokeredMessage] klass.</span><span class="sxs-lookup"><span data-stu-id="51102-160">Messages sent tooService Bus topics are instances of hello [BrokeredMessage][BrokeredMessage] class.</span></span> <span data-ttu-id="51102-161">[BrokeredMessage] [ BrokeredMessage] objekt har en uppsättning standardegenskaper och metoder samt egenskaper som kan använda toohold anpassade programspecifika egenskaper.</span><span class="sxs-lookup"><span data-stu-id="51102-161">[BrokeredMessage][BrokeredMessage] objects have a set of standard properties and methods, as well as properties that can be used toohold custom application-specific properties.</span></span> <span data-ttu-id="51102-162">hello följande exempel visas hur toosend 5 test meddelanden toohello `mytopic` ämne som skapats tidigare.</span><span class="sxs-lookup"><span data-stu-id="51102-162">hello following example shows how toosend 5 test messages toohello `mytopic` topic previously created.</span></span> <span data-ttu-id="51102-163">Hej `setProperty` metoden är att använda tooadd en egen egenskap (`MessageNumber`) tooeach meddelande.</span><span class="sxs-lookup"><span data-stu-id="51102-163">hello `setProperty` method is used tooadd a custom property (`MessageNumber`) tooeach message.</span></span> <span data-ttu-id="51102-164">Observera att hello `MessageNumber` egenskapsvärde varierar för varje meddelande (du kan använda det här värdet toodetermine vilka prenumerationer som får den, enligt hello [skapa en prenumeration](#create-a-subscription) avsnitt):</span><span class="sxs-lookup"><span data-stu-id="51102-164">Note that hello `MessageNumber` property value varies on each message (you can use this value toodetermine which subscriptions receive it, as shown in hello [Create a subscription](#create-a-subscription) section):</span></span>

```php
for($i = 0; $i < 5; $i++){
    // Create message.
    $message = new BrokeredMessage();
    $message->setBody("my message ".$i);

    // Set custom property.
    $message->setProperty("MessageNumber", $i);

    // Send message.
    $serviceBusRestProxy->sendTopicMessage("mytopic", $message);
}
```

<span data-ttu-id="51102-165">Service Bus-ämnena stöder en maximal meddelandestorlek på 256 KB i hello [standardnivån](service-bus-premium-messaging.md) och 1 MB på hello [premiumnivån](service-bus-premium-messaging.md).</span><span class="sxs-lookup"><span data-stu-id="51102-165">Service Bus topics support a maximum message size of 256 KB in hello [Standard tier](service-bus-premium-messaging.md) and 1 MB in hello [Premium tier](service-bus-premium-messaging.md).</span></span> <span data-ttu-id="51102-166">hello-huvudet som innehåller hello standard- och anpassade programegenskaperna, kan ha en maximal storlek på 64 KB.</span><span class="sxs-lookup"><span data-stu-id="51102-166">hello header, which includes hello standard and custom application properties, can have a maximum size of 64 KB.</span></span> <span data-ttu-id="51102-167">Det finns ingen gräns hello antalet meddelanden som lagras i ett ämne men det finns ett tak på hello totala storleken på hälsningsmeddelande som innehas av ett ämne.</span><span class="sxs-lookup"><span data-stu-id="51102-167">There is no limit on hello number of messages held in a topic but there is a cap on hello total size of hello messages held by a topic.</span></span> <span data-ttu-id="51102-168">Den här övre gräns på avsnittet storlek är 5 GB.</span><span class="sxs-lookup"><span data-stu-id="51102-168">This upper limit on topic size is 5 GB.</span></span> <span data-ttu-id="51102-169">Mer information om kvoter finns [Service Bus-kvoter][Service Bus quotas].</span><span class="sxs-lookup"><span data-stu-id="51102-169">For more information about quotas, see [Service Bus quotas][Service Bus quotas].</span></span>

## <a name="receive-messages-from-a-subscription"></a><span data-ttu-id="51102-170">Ta emot meddelanden från en prenumeration</span><span class="sxs-lookup"><span data-stu-id="51102-170">Receive messages from a subscription</span></span>
<span data-ttu-id="51102-171">bästa sättet tooreceive hälsningsmeddelande från en prenumeration är toouse en `ServiceBusRestProxy->receiveSubscriptionMessage` metod.</span><span class="sxs-lookup"><span data-stu-id="51102-171">hello best way tooreceive messages from a subscription is toouse a `ServiceBusRestProxy->receiveSubscriptionMessage` method.</span></span> <span data-ttu-id="51102-172">Meddelanden kan tas emot i två olika lägen: [ *ReceiveAndDelete* och *PeekLock*](https://docs.microsoft.com/dotnet/api/microsoft.servicebus.messaging.receivemode).</span><span class="sxs-lookup"><span data-stu-id="51102-172">Messages can be received in two different modes: [*ReceiveAndDelete* and *PeekLock*](https://docs.microsoft.com/dotnet/api/microsoft.servicebus.messaging.receivemode).</span></span> <span data-ttu-id="51102-173">**PeekLock** är hello som standard.</span><span class="sxs-lookup"><span data-stu-id="51102-173">**PeekLock** is hello default.</span></span>

<span data-ttu-id="51102-174">När du använder hello [ReceiveAndDelete](https://docs.microsoft.com/dotnet/api/microsoft.servicebus.messaging.receivemode) läge får är en engångsåtgärd, det vill säga när Service Bus tar emot en läsbegäran för ett meddelande i en prenumeration, den markerar hello meddelandet som Förbrukat och returnerar det toohello programmet.</span><span class="sxs-lookup"><span data-stu-id="51102-174">When using hello [ReceiveAndDelete](https://docs.microsoft.com/dotnet/api/microsoft.servicebus.messaging.receivemode) mode, receive is a single-shot operation; that is, when Service Bus receives a read request for a message in a subscription, it marks hello message as being consumed and returns it toohello application.</span></span> <span data-ttu-id="51102-175">[ReceiveAndDelete](https://docs.microsoft.com/dotnet/api/microsoft.servicebus.messaging.receivemode) * läge är hello enklaste modellen och fungerar bäst för scenarier där ett program kan tolerera icke-bearbetning av ett meddelande i hello händelse av fel.</span><span class="sxs-lookup"><span data-stu-id="51102-175">[ReceiveAndDelete](https://docs.microsoft.com/dotnet/api/microsoft.servicebus.messaging.receivemode) * mode is hello simplest model and works best for scenarios in which an application can tolerate not processing a message in hello event of a failure.</span></span> <span data-ttu-id="51102-176">toounderstand, Föreställ i vilka hello konsumenten problem hello mottagning av begäran och sedan kraschar innan bearbetningen.</span><span class="sxs-lookup"><span data-stu-id="51102-176">toounderstand this, consider a scenario in which hello consumer issues hello receive request and then crashes before processing it.</span></span> <span data-ttu-id="51102-177">Eftersom Service Bus kommer att ha markerat hello meddelandet som Förbrukat, sedan när hello programmet startas om och börjar förbruka meddelanden igen, att ha missat hello-meddelande som har förbrukats tidigare toohello krascher.</span><span class="sxs-lookup"><span data-stu-id="51102-177">Because Service Bus will have marked hello message as being consumed, then when hello application restarts and begins consuming messages again, it will have missed hello message that was consumed prior toohello crash.</span></span>

<span data-ttu-id="51102-178">I hello standard [PeekLock](https://docs.microsoft.com/dotnet/api/microsoft.servicebus.messaging.receivemode) läge, ett meddelande blir en åtgärd i två steg, vilket gör det möjligt toosupport program som inte tolererar att ett meddelande saknas.</span><span class="sxs-lookup"><span data-stu-id="51102-178">In hello default [PeekLock](https://docs.microsoft.com/dotnet/api/microsoft.servicebus.messaging.receivemode) mode, receiving a message becomes a two stage operation, which makes it possible toosupport applications that cannot tolerate missing messages.</span></span> <span data-ttu-id="51102-179">När Service Bus tar emot en begäran, hittar hello nästa meddelande toobe förbrukas, låser det tooprevent andra användare tar emot det och returnerar sedan toohello program.</span><span class="sxs-lookup"><span data-stu-id="51102-179">When Service Bus receives a request, it finds hello next message toobe consumed, locks it tooprevent other consumers receiving it, and then returns it toohello application.</span></span> <span data-ttu-id="51102-180">När hello programmet har avslutat bearbetningen hello-meddelande (eller lagrar det på ett tillförlitligt sätt för framtida bearbetning), Slutför hello andra steget i hello får processen genom att skicka emot hälsningsmeddelande för`ServiceBusRestProxy->deleteMessage`.</span><span class="sxs-lookup"><span data-stu-id="51102-180">After hello application finishes processing hello message (or stores it reliably for future processing), it completes hello second stage of hello receive process by passing hello received message too`ServiceBusRestProxy->deleteMessage`.</span></span> <span data-ttu-id="51102-181">När Service Bus ser hello `deleteMessage` anropet, den Markera hello meddelandet som Förbrukat och ta bort den från hello kön.</span><span class="sxs-lookup"><span data-stu-id="51102-181">When Service Bus sees hello `deleteMessage` call, it will mark hello message as being consumed and remove it from hello queue.</span></span>

<span data-ttu-id="51102-182">följande exempel visar hur hello tooreceive och bearbeta ett meddelande med hjälp av [PeekLock](https://docs.microsoft.com/dotnet/api/microsoft.servicebus.messaging.receivemode) läge (hello standardläget).</span><span class="sxs-lookup"><span data-stu-id="51102-182">hello following example shows how tooreceive and process a message using [PeekLock](https://docs.microsoft.com/dotnet/api/microsoft.servicebus.messaging.receivemode) mode (hello default mode).</span></span> 

```php
require_once 'vendor/autoload.php';

use WindowsAzure\Common\ServicesBuilder;
use WindowsAzure\Common\ServiceException;
use WindowsAzure\ServiceBus\Models\ReceiveMessageOptions;

// Create Service Bus REST proxy.
$serviceBusRestProxy = ServicesBuilder::getInstance()->createServiceBusService($connectionString);

try    {
    // Set receive mode tooPeekLock (default is ReceiveAndDelete)
    $options = new ReceiveMessageOptions();
    $options->setPeekLock();

    // Get message.
    $message = $serviceBusRestProxy->receiveSubscriptionMessage("mytopic", "mysubscription", $options);

    echo "Body: ".$message->getBody()."<br />";
    echo "MessageID: ".$message->getMessageId()."<br />";

    /*---------------------------
        Process message here.
    ----------------------------*/

    // Delete message. Not necessary if peek lock is not set.
    echo "Deleting message...<br />";
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

## <a name="how-to-handle-application-crashes-and-unreadable-messages"></a><span data-ttu-id="51102-183">Så här: hantera programkrascher och oläsbara meddelanden</span><span class="sxs-lookup"><span data-stu-id="51102-183">How to: handle application crashes and unreadable messages</span></span>
<span data-ttu-id="51102-184">Service Bus innehåller funktioner toohelp du att återställa fel i programmet eller problem med bearbetning av ett meddelande.</span><span class="sxs-lookup"><span data-stu-id="51102-184">Service Bus provides functionality toohelp you gracefully recover from errors in your application or difficulties processing a message.</span></span> <span data-ttu-id="51102-185">Om ett mottagarprogram inte kan tooprocess hello meddelande av någon anledning, så kan det anropa hello `unlockMessage` metod för hello tog emot meddelande (i stället för hello `deleteMessage` metod).</span><span class="sxs-lookup"><span data-stu-id="51102-185">If a receiver application is unable tooprocess hello message for some reason, then it can call hello `unlockMessage` method on hello received message (instead of hello `deleteMessage` method).</span></span> <span data-ttu-id="51102-186">Detta orsakar Service Bus toounlock hello-meddelande i kön för hello och göra den tillgänglig toobe tas emot igen, antingen hello av samma förbrukning av ett program eller ett annat användningsprogram.</span><span class="sxs-lookup"><span data-stu-id="51102-186">This will cause Service Bus toounlock hello message within hello queue and make it available toobe received again, either by hello same consuming application or by another consuming application.</span></span>

<span data-ttu-id="51102-187">Det finns också en tidsgräns som är associerad med ett meddelande i kön hello och om hello program inte tooprocess hello-meddelande innan timeout för lås hello går ut (till exempel om hello programmet kraschar), kommer Service Bus att låsa upp hello-meddelande automatiskt och gör den tillgänglig toobe tas emot igen.</span><span class="sxs-lookup"><span data-stu-id="51102-187">There is also a timeout associated with a message locked within hello queue, and if hello application fails tooprocess hello message before hello lock timeout expires (for example, if hello application crashes), then Service Bus will unlock hello message automatically and make it available toobe received again.</span></span>

<span data-ttu-id="51102-188">Hello händelse som hello programmet kraschar efter hello meddelandet har bearbetats men innan hello `deleteMessage` begäran utfärdas och sedan hello-meddelande kommer att levereras på nytt toohello program när den startas om.</span><span class="sxs-lookup"><span data-stu-id="51102-188">In hello event that hello application crashes after processing hello message but before hello `deleteMessage` request is issued, then hello message will be redelivered toohello application when it restarts.</span></span> <span data-ttu-id="51102-189">Det här kallas ofta *minst när* bearbetning, det vill säga varje meddelande bearbetas minst en gång men i vissa situationer hello samma meddelande levereras.</span><span class="sxs-lookup"><span data-stu-id="51102-189">This is often called *At Least Once* processing; that is, each message is processed at least once but in certain situations hello same message may be redelivered.</span></span> <span data-ttu-id="51102-190">Om hello scenariot inte tolererar duplicerad bearbetning, sedan bör programutvecklarna lägga till ytterligare logik tooapplications toohandle duplicerad meddelandeleverans.</span><span class="sxs-lookup"><span data-stu-id="51102-190">If hello scenario cannot tolerate duplicate processing, then application developers should add additional logic tooapplications toohandle duplicate message delivery.</span></span> <span data-ttu-id="51102-191">Detta uppnås ofta med hjälp av hello `getMessageId` metod i hello-meddelande, förblir konstant under alla leveransförsök.</span><span class="sxs-lookup"><span data-stu-id="51102-191">This is often achieved using hello `getMessageId` method of hello message, which remains constant across delivery attempts.</span></span>

## <a name="delete-topics-and-subscriptions"></a><span data-ttu-id="51102-192">Ta bort ämnen och prenumerationer</span><span class="sxs-lookup"><span data-stu-id="51102-192">Delete topics and subscriptions</span></span>
<span data-ttu-id="51102-193">toodelete ett avsnitt eller en prenumeration, Använd hello `ServiceBusRestProxy->deleteTopic` eller hello `ServiceBusRestProxy->deleteSubscripton` metoder, respektive.</span><span class="sxs-lookup"><span data-stu-id="51102-193">toodelete a topic or a subscription, use hello `ServiceBusRestProxy->deleteTopic` or hello `ServiceBusRestProxy->deleteSubscripton` methods, respectively.</span></span> <span data-ttu-id="51102-194">Observera att om du tar bort ett ämne tas även bort alla prenumerationer som är registrerade med hello-avsnittet.</span><span class="sxs-lookup"><span data-stu-id="51102-194">Note that deleting a topic also deletes any subscriptions that are registered with hello topic.</span></span>

<span data-ttu-id="51102-195">hello följande exempel visas hur toodelete ett ämne med namnet `mytopic` och dess registrerade prenumerationer.</span><span class="sxs-lookup"><span data-stu-id="51102-195">hello following example shows how toodelete a topic named `mytopic` and its registered subscriptions.</span></span>

```php
require_once 'vendor/autoload.php';

use WindowsAzure\ServiceBus\ServiceBusService;
use WindowsAzure\ServiceBus\ServiceBusSettings;
use WindowsAzure\Common\ServiceException;

// Create Service Bus REST proxy.
$serviceBusRestProxy = ServicesBuilder::getInstance()->createServiceBusService($connectionString);

try    {        
    // Delete topic.
    $serviceBusRestProxy->deleteTopic("mytopic");
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

<span data-ttu-id="51102-196">Med hjälp av hello `deleteSubscription` metod, du kan ta bort en prenumeration som är oberoende av varandra:</span><span class="sxs-lookup"><span data-stu-id="51102-196">By using hello `deleteSubscription` method, you can delete a subscription independently:</span></span>

```php
$serviceBusRestProxy->deleteSubscription("mytopic", "mysubscription");
```

## <a name="next-steps"></a><span data-ttu-id="51102-197">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="51102-197">Next steps</span></span>
<span data-ttu-id="51102-198">Nu när du har lärt dig grunderna hello i Service Bus-köer, se [köer, ämnen och prenumerationer] [ Queues, topics, and subscriptions] för mer information.</span><span class="sxs-lookup"><span data-stu-id="51102-198">Now that you've learned hello basics of Service Bus queues, see [Queues, topics, and subscriptions][Queues, topics, and subscriptions] for more information.</span></span>

[BrokeredMessage]: https://docs.microsoft.com/dotnet/api/microsoft.servicebus.messaging.brokeredmessage
[Queues, topics, and subscriptions]: service-bus-queues-topics-subscriptions.md
[sqlfilter]: /dotnet/api/microsoft.servicebus.messaging.sqlfilter#microsoft_servicebus_messaging_sqlfilter
[require-once]: http://php.net/require_once
[Service Bus quotas]: service-bus-quotas.md
