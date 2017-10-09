---
title: "aaaHow toouse Service Bus-köer i Node.js | Microsoft Docs"
description: "Lär dig hur toouse Service Bus köer i Azure från en Node.js-app."
services: service-bus-messaging
documentationcenter: nodejs
author: sethmanheim
manager: timlt
editor: 
ms.assetid: a87a00f9-9aba-4c49-a0df-f900a8b67b3f
ms.service: service-bus-messaging
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: nodejs
ms.topic: article
ms.date: 08/10/2017
ms.author: sethm
ms.openlocfilehash: c55354b2061c41aba1093cc3f12ce2a1bc37a3cc
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="how-toouse-service-bus-queues-with-nodejs"></a>Hur toouse Service Bus köer med Node.js

[!INCLUDE [service-bus-selector-queues](../../includes/service-bus-selector-queues.md)]

Den här artikeln beskriver hur toouse Service Bus köer med Node.js. hello exemplen är skrivna i JavaScript och använder hello Node.js Azure-modulen. hello beskrivs scenarier där **skapa köer**, **skicka och ta emot meddelanden**, och **tar bort köer**. Mer information om köer finns hello [nästa steg](#next-steps) avsnitt.

[!INCLUDE [howto-service-bus-queues](../../includes/howto-service-bus-queues.md)]

[!INCLUDE [service-bus-create-namespace-portal](../../includes/service-bus-create-namespace-portal.md)]

## <a name="create-a-nodejs-application"></a>Skapa ett Node.js-program
Skapa ett tomt Node.js-program. Anvisningar för hur toocreate ett Node.js-program finns [skapa och distribuera en Node.js-programmet tooan Azure-webbplats][Create and deploy a Node.js application tooan Azure Website], eller [Node.js Molntjänsten] [ Node.js Cloud Service] med Windows PowerShell.

## <a name="configure-your-application-toouse-service-bus"></a>Konfigurera ditt program toouse Service Bus
toouse Azure Service Bus, hämtar och använder hello Node.js Azure-paketet. Det här paketet innehåller en uppsättning bibliotek som kommunicerar med hello Service Bus REST-tjänster.

### <a name="use-node-package-manager-npm-tooobtain-hello-package"></a>Använd noden Package Manager (NPM) tooobtain hello-paket
1. Använd hello **Windows PowerShell för Node.js** kommandot fönstret toonavigate toohello **c:\\nod\\sbqueues\\WebRole1** mapp som du skapade din exempelprogrammet.
2. Typen **npm installera azure** i hello-kommandofönster som bör resultera i utdata liknande toohello följande:

    ```
    azure@0.7.5 node_modules\azure
        ├── dateformat@1.0.2-1.2.3
        ├── xmlbuilder@0.4.2
        ├── node-uuid@1.2.0
        ├── mime@1.2.9
        ├── underscore@1.4.4
        ├── validator@1.1.1
        ├── tunnel@0.0.2
        ├── wns@0.5.3
        ├── xml2js@0.2.7 (sax@0.5.2)
        └── request@2.21.0 (json-stringify-safe@4.0.0, forever-agent@0.5.0, aws-sign@0.3.0, tunnel-agent@0.3.0, oauth-sign@0.3.0, qs@0.6.5, cookie-jar@0.3.0, node-uuid@1.4.0, http-signature@0.9.11, form-data@0.0.8, hawk@0.13.1)
    ```
3. Du kan köra hello manuellt **ls** kommandot tooverify som en **node_modules** mappen har skapats. I den mappen hitta hello **azure** paket som innehåller hello bibliotek som du behöver tooaccess Service Bus-köer.

### <a name="import-hello-module"></a>Importera hello-modul
Använda anteckningar eller något annat textredigeringsprogram Lägg till följande toohello överkant hello hello **server.js** -filen för programmet hello:

```javascript
var azure = require('azure');
```

### <a name="set-up-an-azure-service-bus-connection"></a>Azure Service Bus-anslutningar
hello Azure modulen läser hello miljövariabeln `AZURE_SERVICEBUS_CONNECTION_STRING` tooobtain information krävs tooconnect tooService Bus. Om den här miljövariabeln inte har angetts måste du ange hello kontoinformation när du anropar `createServiceBusService`.

Ett exempel på hur hello miljövariabler i en konfigurationsfil för ett Azure Cloud Service, se [Node.js molntjänst med lagring][Node.js Cloud Service with Storage].

Ett exempel på inställningen hello miljövariabler i hello [Azure-portalen] [ Azure portal] en Azure-webbplats finns [Node.js-Webbapp med lagring] [ Node.js Web Application with Storage].

## <a name="create-a-queue"></a>Skapa en kö
Hej **ServiceBusService** objekt kan du toowork med Service Bus-köer. hello följande kod skapar en **ServiceBusService** objekt. Lägg till den hello övre delen av hello **server.js** filen efter hello instruktionen tooimport hello Azure-modulen:

```javascript
var serviceBusService = azure.createServiceBusService();
```

Genom att anropa `createQueueIfNotExists` på hello **ServiceBusService** objekt hello angetts kön returneras (om det finns) eller en ny kö med hello angivna namnet har skapats. hello följande kod används `createQueueIfNotExists` toocreate eller ansluta toohello kö med namnet `myqueue`:

```javascript
serviceBusService.createQueueIfNotExists('myqueue', function(error){
    if(!error){
        // Queue exists
    }
});
```

Hej `createServiceBusService` metoden stöder också ytterligare alternativ som gör att du toooverride kön standardinställningarna, till exempel meddelande tid toolive eller maximal köstorlek. hello följande exempel anger hello största köstorlek storlek too5 GB och en tid toolive TTL-värde på 1 minut:

```javascript
var queueOptions = {
      MaxSizeInMegabytes: '5120',
      DefaultMessageTimeToLive: 'PT1M'
    };

serviceBusService.createQueueIfNotExists('myqueue', queueOptions, function(error){
    if(!error){
        // Queue exists
    }
});
```

### <a name="filters"></a>Filter
Valfria filtrering åtgärder kan vara tillämpade toooperations utförs med hjälp av **ServiceBusService**. Filtrering operations kan innehålla loggning, försöker automatiskt igen och så vidare. Filtren är objekt som implementerar en metod med hello signatur:

```javascript
function handle (requestOptions, next)
```

När du har gjort dess föregående bearbetning på hello begäran alternativ hello-metoden måste anropa `next`, skicka ett återanrop med hello följande signatur:

```javascript
function (returnObject, finalCallback, next)
```

I den här motringning och efter bearbetning hello `returnObject` (hello svar från hello begäran toohello server), hello-återanrop måste antingen anropa `next` om det finns toocontinue bearbetning filter eller helt enkelt anropa `finalCallback`, vilka slutar hello service-anrop.

Två filter som implementerar logik som medföljer hello Azure SDK för Node.js, `ExponentialRetryPolicyFilter` och `LinearRetryPolicyFilter`. hello följande kod skapar en `ServiceBusService` objekt som använder hello `ExponentialRetryPolicyFilter`:

```javascript
var retryOperations = new azure.ExponentialRetryPolicyFilter();
var serviceBusService = azure.createServiceBusService().withFilter(retryOperations);
```

## <a name="send-messages-tooa-queue"></a>Skicka meddelanden tooa kön
toosend en meddelandekö tooa Service Bus programmet anropar hello `sendQueueMessage` metod på hello **ServiceBusService** objekt. Meddelanden skickas för (och mottagna från) Service Bus är köer **BrokeredMessage** objekt och har en uppsättning standardegenskaper (t.ex **etikett** och **TimeToLive**), ordlista som används toohold anpassade programspecifika egenskaper och en brödtext med godtyckliga programdata. Ett program kan konfigurera hello brödtext hello-meddelande genom att skicka en sträng som hello-meddelande. Alla nödvändiga standardegenskaper fylls i med standardvärden.

hello exemplet nedan visar hur toosend en toohello meddelandekö test med namnet `myqueue` med `sendQueueMessage`:

```javascript
var message = {
    body: 'Test message',
    customProperties: {
        testproperty: 'TestValue'
    }};
serviceBusService.sendQueueMessage('myqueue', message, function(error){
    if(!error){
        // message sent
    }
});
```

Service Bus-köer stöder en maximal meddelandestorlek på 256 KB i hello [standardnivån](service-bus-premium-messaging.md) och 1 MB på hello [premiumnivån](service-bus-premium-messaging.md). hello-huvudet som innehåller hello standard- och anpassade programegenskaperna, kan ha en maximal storlek på 64 KB. Det finns ingen gräns hello antalet meddelanden som finns i en kö men det finns ett tak på hello totala storleken på hälsningsmeddelande som innehas av en kö. Den här köstorleken definieras när kön skapas, med en övre gräns på 5 GB. Mer information om kvoter finns [Service Bus-kvoter][Service Bus quotas].

## <a name="receive-messages-from-a-queue"></a>Ta emot meddelanden från en kö
Meddelanden tas emot från en kö med hello `receiveQueueMessage` metod på hello **ServiceBusService** objekt. Som standard tas meddelanden bort från kön hello som de läses; men du kan läsa (titt) och låsa hello-meddelande utan att ta bort den från hello kö genom att ange hello valfri parameter `isPeekLock` för**SANT**.

Hej standardbeteendet för läsning och ta bort hello-meddelande som en del av hello mottagningsåtgärd är hello enklaste modellen och fungerar bäst för scenarier där ett program kan tolerera icke-bearbetning av ett meddelande i hello händelse av fel. toounderstand, Föreställ i vilka hello konsumenten problem hello mottagning av begäran och sedan kraschar innan bearbetningen. Eftersom Service Bus kommer att ha markerat hello meddelandet som Förbrukat, sedan när hello programmet startas om och börjar förbruka meddelanden igen, att ha missat hello-meddelande som har förbrukats tidigare toohello krascher.

Om hello `isPeekLock` parameter har angetts för**SANT**, hello får blir en åtgärd i två steg, vilket gör det möjligt toosupport program som inte tolererar att ett meddelande saknas. När Service Bus tar emot en begäran, hittar hello nästa meddelande toobe förbrukas, låser det tooprevent andra användare tar emot det och returnerar sedan toohello program. När hello programmet har avslutat bearbetningen hello-meddelande (eller lagrar det på ett tillförlitligt sätt för framtida bearbetning), Slutför hello andra steget i hello får processen genom att anropa `deleteMessage` metod och ge hello meddelandet toobe tas bort som en parameter. Hej `deleteMessage` metoden markerar hello meddelandet som Förbrukat och tar bort det från hello kö.

hello exemplet nedan visar hur tooreceive och bearbeta meddelanden med hjälp av `receiveQueueMessage`. hello exempel först tar emot och tar bort ett meddelande och tar emot ett meddelande med hjälp av `isPeekLock` ställa in också**SANT**, och sedan tar bort hello meddelande med `deleteMessage`:

```javascript
serviceBusService.receiveQueueMessage('myqueue', function(error, receivedMessage){
    if(!error){
        // Message received and deleted
    }
});
serviceBusService.receiveQueueMessage('myqueue', { isPeekLock: true }, function(error, lockedMessage){
    if(!error){
        // Message received and locked
        serviceBusService.deleteMessage(lockedMessage, function (deleteError){
            if(!deleteError){
                // Message deleted
            }
        });
    }
});
```

## <a name="how-toohandle-application-crashes-and-unreadable-messages"></a>Hur toohandle programmet kraschar och oläsbara meddelanden
Service Bus innehåller funktioner toohelp du att återställa fel i programmet eller problem med bearbetning av ett meddelande. Om ett mottagarprogram inte kan tooprocess hello meddelande av någon anledning, så kan det anropa hello `unlockMessage` metod på hello **ServiceBusService** objekt. Detta orsakar Service Bus toounlock meddelandet i kön för hello och göra den tillgänglig toobe tas emot igen, antingen hello av samma förbrukning av ett program eller ett annat användningsprogram.

Det finns också en tidsgräns som är associerad med ett meddelande i kön hello och om hello hello programmet misslyckas tooprocess hello meddelandet innan timeout för lås går ut (t.ex. Om hello programmet kraschar), kommer Service Bus låser upp hello-meddelande automatiskt och gör den tillgänglig toobe tas emot igen.

Hello händelse som hello programmet kraschar efter hello meddelandet har bearbetats men innan hello `deleteMessage` metoden anropas sedan hello-meddelande kommer att levereras på nytt toohello program när den startas om. Det här kallas ofta *At Least Once Processing*, det vill säga, varje meddelande bearbetas minst en gång men i vissa situationer hello samma meddelande levereras. Om hello scenariot inte tolererar duplicerad bearbetning, sedan bör programutvecklarna lägga till ytterligare logik tootheir duplicerad meddelandeleverans för programmet toohandle. Detta uppnås ofta med hjälp av hello **MessageId** -egenskapen för hello-meddelande, förblir konstant under alla leveransförsök.

## <a name="next-steps"></a>Nästa steg
toolearn mer om köer finns hello följande resurser.

* [Köer, ämnen och prenumerationer][Queues, topics, and subscriptions]
* [Azure SDK för noden] [ Azure SDK for Node] databasen på GitHub
* [Node.js Developer Center](https://azure.microsoft.com/develop/nodejs/)

[Azure SDK for Node]: https://github.com/Azure/azure-sdk-for-node
[Azure portal]: https://portal.azure.com

[Node.js Cloud Service]: ../cloud-services/cloud-services-nodejs-develop-deploy-app.md
[Queues, topics, and subscriptions]: service-bus-queues-topics-subscriptions.md
[Create and deploy a Node.js application tooan Azure Website]: ../app-service-web/app-service-web-get-started-nodejs.md
[Node.js Cloud Service with Storage]:../cosmos-db/table-storage-cloud-service-nodejs.md
[Node.js Web Application with Storage]:../cosmos-db/table-storage-how-to-use-nodejs.md
[Service Bus quotas]: service-bus-quotas.md
