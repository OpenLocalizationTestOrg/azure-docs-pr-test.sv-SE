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
# <a name="how-toouse-service-bus-queues-with-php"></a>Hur toouse Service Bus köer med PHP
[!INCLUDE [service-bus-selector-queues](../../includes/service-bus-selector-queues.md)]

Den här guiden visar hur toouse Service Bus-köer. hello exemplen är skrivna i PHP och använder hello [Azure SDK för PHP](../php-download-sdk.md). hello beskrivs scenarier där **skapa köer**, **skicka och ta emot meddelanden**, och **tar bort köer**.

[!INCLUDE [howto-service-bus-queues](../../includes/howto-service-bus-queues.md)]

## <a name="create-a-php-application"></a>Skapa en PHP-program
Hej krav för att skapa en PHP-program som ansluter till hello Azure Blob-tjänsten är hello refererar till klasser i hello [Azure SDK för PHP](../php-download-sdk.md) från inom din kod. Du kan använda alla development tools toocreate programmet eller anteckningar.

> [!NOTE]
> PHP-installation måste också ha hello [OpenSSL tillägget](http://php.net/openssl) installerat och aktiverat.
> 
> 

I den här guiden använder du tjänstens funktioner som kan anropas från ett PHP-program lokalt eller i koden körs i en Azure-webbroll, en arbetsroll eller en webbplats.

## <a name="get-hello-azure-client-libraries"></a>Hämta hello Azure klientbibliotek
[!INCLUDE [get-client-libraries](../../includes/get-client-libraries.md)]

## <a name="configure-your-application-toouse-service-bus"></a>Konfigurera ditt program toouse Service Bus
toouse hello Service Bus-kö API: er, hello följande:

1. Referens hello bandladdaren fil med hello [require_once] [ require_once] instruktionen.
2. Referera till alla klasser som du kan använda.

hello följande exempel visas hur tooinclude hello bandladdaren fil- och hello `ServicesBuilder` klass.

> [!NOTE]
> Det här exemplet (och andra exempel i den här artikeln) förutsätter att du har installerat hello PHP-klientbibliotek för Azure via Composer. Om du har installerat hello bibliotek manuellt eller som ett PÄRONTRÄD paket måste du referera hello **WindowsAzure.php** bandladdaren filen.
> 
> 

```php
require_once 'vendor/autoload.php';
use WindowsAzure\Common\ServicesBuilder;
```

I hello exemplen nedan hello `require_once` instruktionen visas alltid, men bara hello klasser krävs för hello exempel tooexecute refererar till.

## <a name="set-up-a-service-bus-connection"></a>Konfigurera en Service Bus-anslutning
tooinstantiate en Service Bus-klient, måste du först ha en giltig anslutningssträng i det här formatet:

```
Endpoint=[yourEndpoint];SharedAccessKeyName=RootManageSharedAccessKey;SharedAccessKey=[Primary Key]
```

Där `Endpoint` är vanligtvis av hello format `[yourNamespace].servicebus.windows.net`.

toocreate alla Azure-tjänst-klienter måste du använda hello `ServicesBuilder` klass. Du kan:

* Skicka hello anslutning direkt string tooit.
* Använd hello **CloudConfigurationManager (CCM)** toocheck flera externa datakällor för hello anslutningssträngen:
  * Som standard levereras med stöd för en extern källa - miljövariabler
  * Du kan lägga till nya källor genom att utöka hello `ConnectionStringSource` klass

Hello-exempel som beskrivs här skickas hello anslutningssträngen direkt.

```php
require_once 'vendor/autoload.php';

use WindowsAzure\Common\ServicesBuilder;

$connectionString = "Endpoint=[yourEndpoint];SharedAccessKeyName=RootManageSharedAccessKey;SharedAccessKey=[Primary Key]";

$serviceBusRestProxy = ServicesBuilder::getInstance()->createServiceBusService($connectionString);
```

## <a name="create-a-queue"></a>Skapa en kö
Du kan utföra hanteringsåtgärder för Service Bus-köer via hello `ServiceBusRestProxy` klass. En `ServiceBusRestProxy` objektet har skapats via hello `ServicesBuilder::createServiceBusService` fabriksmetoden med en lämplig anslutningssträngen som kapslar in hello token behörigheter toomanage den.

följande exempel visar hur hello tooinstantiate en `ServiceBusRestProxy` och anropa `ServiceBusRestProxy->createQueue` toocreate en kö med namnet `myqueue` inom en `MySBNamespace` namnområde för tjänsten:

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
> Du kan använda hello `listQueues` metod på `ServiceBusRestProxy` objekt toocheck om en kö med ett angivet namn finns redan i ett namnområde.
> 
> 

## <a name="send-messages-tooa-queue"></a>Skicka meddelanden tooa kön
toosend en meddelandekö tooa Service Bus programmet anropar hello `ServiceBusRestProxy->sendQueueMessage` metod. Hej följande kod visar hur toosend ett meddelande toohello `myqueue` kö som tidigare har skapat i den `MySBNamespace` namnområde för tjänsten.

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

Meddelanden som skickats för (och tagits emot från) Service Bus-köer är instanser av hello [BrokeredMessage] [ BrokeredMessage] klass. [BrokeredMessage] [ BrokeredMessage] objekt har en uppsättning standard metoder och egenskaper som används toohold anpassade programspecifika egenskaper och en brödtext med godtyckliga programdata.

Service Bus-köer stöder en maximal meddelandestorlek på 256 KB i hello [standardnivån](service-bus-premium-messaging.md) och 1 MB på hello [premiumnivån](service-bus-premium-messaging.md). hello-huvudet som innehåller hello standard- och anpassade programegenskaperna, kan ha en maximal storlek på 64 KB. Det finns ingen gräns hello antalet meddelanden som finns i en kö men det finns ett tak på hello totala storleken på hälsningsmeddelande som innehas av en kö. Den här övre gräns för storleken är 5 GB.

## <a name="receive-messages-from-a-queue"></a>Ta emot meddelanden från en kö

hello bästa sätt tooreceive meddelanden från en kö är toouse en `ServiceBusRestProxy->receiveQueueMessage` metod. Meddelanden kan tas emot i två olika lägen: [ *ReceiveAndDelete* ](/dotnet/api/microsoft.servicebus.messaging.receivemode.receiveanddelete) och [ *PeekLock*](/dotnet/api/microsoft.servicebus.messaging.receivemode.peeklock). **PeekLock** är hello som standard.

När du använder [ReceiveAndDelete](/dotnet/api/microsoft.servicebus.messaging.receivemode.receiveanddelete) läge får är en engångsåtgärd, det vill säga när Service Bus tar emot en läsbegäran för ett meddelande i en kö, den markerar hello meddelandet som Förbrukat och returnerar det toohello program. [ReceiveAndDelete](/dotnet/api/microsoft.servicebus.messaging.receivemode.receiveanddelete) läge är hello enklaste modellen och fungerar bäst för scenarier där ett program kan tolerera icke-bearbetning av ett meddelande i hello händelse av fel. toounderstand, Föreställ i vilka hello konsumenten problem hello mottagning av begäran och sedan kraschar innan bearbetningen. Eftersom Service Bus kommer att ha markerat hello meddelandet som Förbrukat, sedan när hello programmet startas om och börjar förbruka meddelanden igen, att ha missat hello-meddelande som har förbrukats tidigare toohello krascher.

I hello standard [PeekLock](/dotnet/api/microsoft.servicebus.messaging.receivemode.peeklock) läge, ett meddelande blir en åtgärd i två steg, vilket gör det möjligt toosupport program som inte tolererar att ett meddelande saknas. När Service Bus tar emot en begäran, den hittar hello nästa meddelande toobe förbrukas, låser det tooprevent andra användare tar emot den, och returnerar sedan toohello program. När hello programmet har avslutat bearbetningen hello-meddelande (eller lagrar det på ett tillförlitligt sätt för framtida bearbetning), Slutför hello andra steget i hello får processen genom att skicka emot hälsningsmeddelande för`ServiceBusRestProxy->deleteMessage`. När Service Bus ser hello `deleteMessage` anropet, den Markera hello meddelandet som Förbrukat och ta bort den från hello kön.

följande exempel visar hur hello tooreceive och bearbeta ett meddelande med hjälp av [PeekLock](/dotnet/api/microsoft.servicebus.messaging.receivemode.peeklock) läge (hello standardläget).

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

## <a name="how-toohandle-application-crashes-and-unreadable-messages"></a>Hur toohandle programmet kraschar och oläsbara meddelanden

Service Bus innehåller funktioner toohelp du att återställa fel i programmet eller problem med bearbetning av ett meddelande. Om ett mottagarprogram inte kan tooprocess hello meddelande av någon anledning, så kan det anropa hello `unlockMessage` metod för hello tog emot meddelande (i stället för hello `deleteMessage` metod). Detta orsakar Service Bus toounlock hello-meddelande i kön för hello och göra den tillgänglig toobe tas emot igen, antingen hello av samma förbrukning av ett program eller ett annat användningsprogram.

Det finns också en tidsgräns som är associerad med ett meddelande i kön hello och om hello program inte tooprocess hello-meddelande innan timeout för lås hello går ut (till exempel om hello programmet kraschar), kommer Service Bus att låsa upp hello-meddelande automatiskt och gör den tillgänglig toobe tas emot igen.

Hello händelse som hello programmet kraschar efter hello meddelandet har bearbetats men innan hello `deleteMessage` begäran utfärdas och sedan hello-meddelande kommer att levereras på nytt toohello program när den startas om. Det här kallas ofta *minst när* bearbetning, det vill säga varje meddelande bearbetas minst en gång men i vissa situationer hello samma meddelande levereras. Om hello scenariot inte tolererar duplicerad bearbetning och sedan lägga till ytterligare rekommenderas logik tooapplications toohandle duplicerad meddelandeleverans. Detta uppnås ofta med hjälp av hello `getMessageId` metod i hello-meddelande, förblir konstant under alla leveransförsök.

## <a name="next-steps"></a>Nästa steg
Nu när du har lärt dig grunderna hello i Service Bus-köer, se [köer, ämnen och prenumerationer] [ Queues, topics, and subscriptions] för mer information.

Mer information finns också hello [PHP Developer Center](https://azure.microsoft.com/develop/php/).

[BrokeredMessage]: /dotnet/api/microsoft.servicebus.messaging.brokeredmessage
[Queues, topics, and subscriptions]: service-bus-queues-topics-subscriptions.md
[require_once]: http://php.net/require_once


