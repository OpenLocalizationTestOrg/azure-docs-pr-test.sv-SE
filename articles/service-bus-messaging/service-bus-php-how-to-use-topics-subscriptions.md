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
# <a name="how-toouse-service-bus-topics-and-subscriptions-with-php"></a>Hur toouse Service Bus-ämnen och prenumerationer med PHP

[!INCLUDE [service-bus-selector-topics](../../includes/service-bus-selector-topics.md)]

Den här artikeln beskrivs hur du toouse Service Bus-ämnen och prenumerationer. hello exemplen är skrivna i PHP och använder hello [Azure SDK för PHP](../php-download-sdk.md). hello beskrivs scenarier där **skapa ämnen och prenumerationer**, **skapa prenumerationsfilter**, **skickar meddelanden tooa avsnittet**, **tar emot meddelanden från en prenumeration**, och **ta bort ämnen och prenumerationer**.

[!INCLUDE [howto-service-bus-topics](../../includes/howto-service-bus-topics.md)]

## <a name="create-a-php-application"></a>Skapa en PHP-program
Hej krav för att skapa en PHP-program som ansluter till hello Azure Blob-tjänsten är tooreference klasser i hello [Azure SDK för PHP](../php-download-sdk.md) från inom din kod. Du kan använda alla development tools toocreate programmet eller anteckningar.

> [!NOTE]
> PHP-installation måste också ha hello [OpenSSL tillägget](http://php.net/openssl) installerat och aktiverat.
> 
> 

Den här artikeln beskriver hur toouse tjänsten funktioner som kan anropas i en PHP-program lokalt eller i koden körs i en Azure-webbroll, en arbetsroll eller en webbplats.

## <a name="get-hello-azure-client-libraries"></a>Hämta hello Azure klientbibliotek
[!INCLUDE [get-client-libraries](../../includes/get-client-libraries.md)]

## <a name="configure-your-application-toouse-service-bus"></a>Konfigurera ditt program toouse Service Bus
toouse hello Service Bus-API: er:

1. Referens hello bandladdaren fil med hello [require_once] [ require-once] instruktionen.
2. Referera till alla klasser som du kan använda.

hello följande exempel visas hur tooinclude hello bandladdaren fil- och hello **ServiceBusService** klass.

> [!NOTE]
> Det här exemplet (och andra exempel i den här artikeln) förutsätter att du har installerat hello PHP-klientbibliotek för Azure via Composer. Om du har installerat hello bibliotek manuellt eller som ett PÄRONTRÄD paket måste du referera hello **WindowsAzure.php** bandladdaren filen.
> 
> 

```php
require_once 'vendor\autoload.php';
use WindowsAzure\Common\ServicesBuilder;
```

I följande exempel hello, hello `require_once` instruktionen visas alltid, men bara hello klasser krävs för hello exempel tooexecute refererar till.

## <a name="set-up-a-service-bus-connection"></a>Konfigurera en Service Bus-anslutning
tooinstantiate en Service Bus-klient måste du först ha en giltig anslutningssträng i det här formatet:

```
Endpoint=[yourEndpoint];SharedAccessKeyName=RootManageSharedAccessKey;SharedAccessKey=[Primary Key]
```

Där `Endpoint` är vanligtvis av hello format `https://[yourNamespace].servicebus.windows.net`.

toocreate alla Azure-tjänst-klienter måste du använda hello `ServicesBuilder` klass. Du kan:

* Skicka hello anslutning direkt string tooit.
* Använd hello **CloudConfigurationManager (CCM)** toocheck flera externa datakällor för hello anslutningssträngen:
  * Som standard levereras den med stöd för en extern källa - miljövariabler.
  * Du kan lägga till nya källor genom att utöka hello `ConnectionStringSource` klass.

Hello-exempel som beskrivs här skickas hello anslutningssträngen direkt.

```php
require_once 'vendor/autoload.php';

use WindowsAzure\Common\ServicesBuilder;

$connectionString = "Endpoint=[yourEndpoint];SharedAccessKeyName=RootManageSharedAccessKey;SharedAccessKey=[Primary Key]";

$serviceBusRestProxy = ServicesBuilder::getInstance()->createServiceBusService($connectionString);
```

## <a name="create-a-topic"></a>Skapa ett ämne
Du kan utföra hanteringsåtgärder för Service Bus-ämnen via hello `ServiceBusRestProxy` klass. En `ServiceBusRestProxy` objektet har skapats via hello `ServicesBuilder::createServiceBusService` fabriksmetoden med en lämplig anslutningssträngen som kapslar in hello token behörigheter toomanage den.

följande exempel visar hur hello tooinstantiate en `ServiceBusRestProxy` och anropa `ServiceBusRestProxy->createTopic` toocreate ett ämne med namnet `mytopic` inom en `MySBNamespace` namnområde:

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
> Du kan använda hello `listTopics` metod på `ServiceBusRestProxy` objekt toocheck om ett ämne med ett angivet namn finns redan i ett namnområde för tjänsten.
> 
> 

## <a name="create-a-subscription"></a>Skapa en prenumeration
Ämnesprenumerationer skapas också med hello `ServiceBusRestProxy->createSubscription` metod. Prenumerationer är namngivna och kan ha ett valfritt filter som begränsar hello uppsättning av meddelanden som skickas toohello prenumerationens virtuella kö.

### <a name="create-a-subscription-with-hello-default-matchall-filter"></a>Skapa en prenumeration med hello standardfiltret (MatchAll)
Hej **MatchAll** filtret är hello standardfilter som används om inget filter anges när en ny prenumeration skapas. När hello **MatchAll** filter används, alla meddelanden publicerade toohello avsnittet placeras i hello prenumerationens virtuella kö. hello följande exempel skapas en prenumeration med namnet 'mysubscription' och använder hello standard **MatchAll** filter.

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

### <a name="create-subscriptions-with-filters"></a>Skapa prenumerationer med filter
Du kan också ställa in filter som gör att du toospecify vilka meddelanden som skickas tooa avsnittet ska visas inom en viss ämnesprenumeration. hello mest flexibla typen av filter som stöds av prenumerationerna är hello [SqlFilter](/dotnet/api/microsoft.servicebus.messaging.sqlfilter#microsoft_servicebus_messaging_sqlfilter), som implementerar en deluppsättning av SQL92. SQL-filter tillämpas för hälsningsmeddelande som är publicerade toohello avsnittet hello egenskaper. Läs mer om SqlFilters [SqlFilter.SqlExpression egenskapen][sqlfilter].

> [!NOTE]
> Varje regel för en prenumeration bearbetar inkommande meddelanden oberoende av varandra, lägga till resultatet meddelanden toohello prenumerationen. Dessutom kan varje ny prenumeration har standard **regeln** objekt med ett filter som lägger till alla meddelanden från hello avsnittet toohello prenumeration. tooreceive endast meddelanden som matchar filtret, måste du ta bort hello Standardregeln. Du kan ta bort hello standardregel med hello `ServiceBusRestProxy->deleteRule` metod.
> 
> 

hello följande exempel skapas en prenumeration med namnet `HighMessages` med en **SqlFilter** som endast väljer meddelanden som har en anpassad `MessageNumber` egenskap som är större än 3. Se [skicka meddelanden tooa avsnittet](#send-messages-to-a-topic) information om att lägga till anpassade egenskaper toomessages.

```php
$subscriptionInfo = new SubscriptionInfo("HighMessages");
$serviceBusRestProxy->createSubscription("mytopic", $subscriptionInfo);

$serviceBusRestProxy->deleteRule("mytopic", "HighMessages", '$Default');

$ruleInfo = new RuleInfo("HighMessagesRule");
$ruleInfo->withSqlFilter("MessageNumber > 3");
$ruleResult = $serviceBusRestProxy->createRule("mytopic", "HighMessages", $ruleInfo);
```

Observera att den här koden kräver hello användning av ett ytterligare namnområde: `WindowsAzure\ServiceBus\Models\SubscriptionInfo`.

På liknande sätt hello följande exempel skapas en prenumeration med namnet `LowMessages` med en `SqlFilter` som endast väljer meddelanden som har en `MessageNumber` egenskap som är mindre än eller lika med too3.

```php
$subscriptionInfo = new SubscriptionInfo("LowMessages");
$serviceBusRestProxy->createSubscription("mytopic", $subscriptionInfo);

$serviceBusRestProxy->deleteRule("mytopic", "LowMessages", '$Default');

$ruleInfo = new RuleInfo("LowMessagesRule");
$ruleInfo->withSqlFilter("MessageNumber <= 3");
$ruleResult = $serviceBusRestProxy->createRule("mytopic", "LowMessages", $ruleInfo);
```

Nu när ett meddelande skickas toohello `mytopic` avsnittet levereras det alltid tooreceivers prenumererar toohello `mysubscription` prenumeration och levereras selektivt tooreceivers prenumererar toohello `HighMessages` och `LowMessages` prenumerationer ( beroende på hello meddelandeinnehåll).

## <a name="send-messages-tooa-topic"></a>Skicka meddelanden tooa avsnittet
toosend meddelandet tooa Service Bus-ämne programmet anropar hello `ServiceBusRestProxy->sendTopicMessage` metod. Hej följande kod visar hur toosend ett meddelande toohello `mytopic` avsnittet som tidigare har skapat i den `MySBNamespace` namnområde för tjänsten.

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

Meddelanden som skickas tooService Bus-ämnen är instanser av hello [BrokeredMessage] [ BrokeredMessage] klass. [BrokeredMessage] [ BrokeredMessage] objekt har en uppsättning standardegenskaper och metoder samt egenskaper som kan använda toohold anpassade programspecifika egenskaper. hello följande exempel visas hur toosend 5 test meddelanden toohello `mytopic` ämne som skapats tidigare. Hej `setProperty` metoden är att använda tooadd en egen egenskap (`MessageNumber`) tooeach meddelande. Observera att hello `MessageNumber` egenskapsvärde varierar för varje meddelande (du kan använda det här värdet toodetermine vilka prenumerationer som får den, enligt hello [skapa en prenumeration](#create-a-subscription) avsnitt):

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

Service Bus-ämnena stöder en maximal meddelandestorlek på 256 KB i hello [standardnivån](service-bus-premium-messaging.md) och 1 MB på hello [premiumnivån](service-bus-premium-messaging.md). hello-huvudet som innehåller hello standard- och anpassade programegenskaperna, kan ha en maximal storlek på 64 KB. Det finns ingen gräns hello antalet meddelanden som lagras i ett ämne men det finns ett tak på hello totala storleken på hälsningsmeddelande som innehas av ett ämne. Den här övre gräns på avsnittet storlek är 5 GB. Mer information om kvoter finns [Service Bus-kvoter][Service Bus quotas].

## <a name="receive-messages-from-a-subscription"></a>Ta emot meddelanden från en prenumeration
bästa sättet tooreceive hälsningsmeddelande från en prenumeration är toouse en `ServiceBusRestProxy->receiveSubscriptionMessage` metod. Meddelanden kan tas emot i två olika lägen: [ *ReceiveAndDelete* och *PeekLock*](https://docs.microsoft.com/dotnet/api/microsoft.servicebus.messaging.receivemode). **PeekLock** är hello som standard.

När du använder hello [ReceiveAndDelete](https://docs.microsoft.com/dotnet/api/microsoft.servicebus.messaging.receivemode) läge får är en engångsåtgärd, det vill säga när Service Bus tar emot en läsbegäran för ett meddelande i en prenumeration, den markerar hello meddelandet som Förbrukat och returnerar det toohello programmet. [ReceiveAndDelete](https://docs.microsoft.com/dotnet/api/microsoft.servicebus.messaging.receivemode) * läge är hello enklaste modellen och fungerar bäst för scenarier där ett program kan tolerera icke-bearbetning av ett meddelande i hello händelse av fel. toounderstand, Föreställ i vilka hello konsumenten problem hello mottagning av begäran och sedan kraschar innan bearbetningen. Eftersom Service Bus kommer att ha markerat hello meddelandet som Förbrukat, sedan när hello programmet startas om och börjar förbruka meddelanden igen, att ha missat hello-meddelande som har förbrukats tidigare toohello krascher.

I hello standard [PeekLock](https://docs.microsoft.com/dotnet/api/microsoft.servicebus.messaging.receivemode) läge, ett meddelande blir en åtgärd i två steg, vilket gör det möjligt toosupport program som inte tolererar att ett meddelande saknas. När Service Bus tar emot en begäran, hittar hello nästa meddelande toobe förbrukas, låser det tooprevent andra användare tar emot det och returnerar sedan toohello program. När hello programmet har avslutat bearbetningen hello-meddelande (eller lagrar det på ett tillförlitligt sätt för framtida bearbetning), Slutför hello andra steget i hello får processen genom att skicka emot hälsningsmeddelande för`ServiceBusRestProxy->deleteMessage`. När Service Bus ser hello `deleteMessage` anropet, den Markera hello meddelandet som Förbrukat och ta bort den från hello kön.

följande exempel visar hur hello tooreceive och bearbeta ett meddelande med hjälp av [PeekLock](https://docs.microsoft.com/dotnet/api/microsoft.servicebus.messaging.receivemode) läge (hello standardläget). 

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

## <a name="how-to-handle-application-crashes-and-unreadable-messages"></a>Så här: hantera programkrascher och oläsbara meddelanden
Service Bus innehåller funktioner toohelp du att återställa fel i programmet eller problem med bearbetning av ett meddelande. Om ett mottagarprogram inte kan tooprocess hello meddelande av någon anledning, så kan det anropa hello `unlockMessage` metod för hello tog emot meddelande (i stället för hello `deleteMessage` metod). Detta orsakar Service Bus toounlock hello-meddelande i kön för hello och göra den tillgänglig toobe tas emot igen, antingen hello av samma förbrukning av ett program eller ett annat användningsprogram.

Det finns också en tidsgräns som är associerad med ett meddelande i kön hello och om hello program inte tooprocess hello-meddelande innan timeout för lås hello går ut (till exempel om hello programmet kraschar), kommer Service Bus att låsa upp hello-meddelande automatiskt och gör den tillgänglig toobe tas emot igen.

Hello händelse som hello programmet kraschar efter hello meddelandet har bearbetats men innan hello `deleteMessage` begäran utfärdas och sedan hello-meddelande kommer att levereras på nytt toohello program när den startas om. Det här kallas ofta *minst när* bearbetning, det vill säga varje meddelande bearbetas minst en gång men i vissa situationer hello samma meddelande levereras. Om hello scenariot inte tolererar duplicerad bearbetning, sedan bör programutvecklarna lägga till ytterligare logik tooapplications toohandle duplicerad meddelandeleverans. Detta uppnås ofta med hjälp av hello `getMessageId` metod i hello-meddelande, förblir konstant under alla leveransförsök.

## <a name="delete-topics-and-subscriptions"></a>Ta bort ämnen och prenumerationer
toodelete ett avsnitt eller en prenumeration, Använd hello `ServiceBusRestProxy->deleteTopic` eller hello `ServiceBusRestProxy->deleteSubscripton` metoder, respektive. Observera att om du tar bort ett ämne tas även bort alla prenumerationer som är registrerade med hello-avsnittet.

hello följande exempel visas hur toodelete ett ämne med namnet `mytopic` och dess registrerade prenumerationer.

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

Med hjälp av hello `deleteSubscription` metod, du kan ta bort en prenumeration som är oberoende av varandra:

```php
$serviceBusRestProxy->deleteSubscription("mytopic", "mysubscription");
```

## <a name="next-steps"></a>Nästa steg
Nu när du har lärt dig grunderna hello i Service Bus-köer, se [köer, ämnen och prenumerationer] [ Queues, topics, and subscriptions] för mer information.

[BrokeredMessage]: https://docs.microsoft.com/dotnet/api/microsoft.servicebus.messaging.brokeredmessage
[Queues, topics, and subscriptions]: service-bus-queues-topics-subscriptions.md
[sqlfilter]: /dotnet/api/microsoft.servicebus.messaging.sqlfilter#microsoft_servicebus_messaging_sqlfilter
[require-once]: http://php.net/require_once
[Service Bus quotas]: service-bus-quotas.md
