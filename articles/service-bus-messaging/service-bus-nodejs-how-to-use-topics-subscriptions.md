---
title: "aaaHow toouse Azure Service Bus-ämnen och prenumerationer med Node.js | Microsoft Docs"
description: "Lär dig hur toouse Service Bus-ämnen och prenumerationer i Azure från en Node.js-app."
services: service-bus-messaging
documentationcenter: nodejs
author: sethmanheim
manager: timlt
editor: 
ms.assetid: b9f5db85-7b6c-4cc7-bd2c-bd3087c99875
ms.service: service-bus-messaging
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: nodejs
ms.topic: article
ms.date: 08/10/2017
ms.author: sethm
ms.openlocfilehash: e8f6e7ad6ed16d844c408337ac9e50f990e3fafd
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="how-toouse-service-bus-topics-and-subscriptions-with-nodejs"></a>Hur tooUse Service Bus-ämnen och prenumerationer med Node.js

[!INCLUDE [service-bus-selector-topics](../../includes/service-bus-selector-topics.md)]

Den här guiden beskriver hur toouse Service Bus-ämnen och prenumerationer från Node.js-program. hello beskrivs scenarier där **skapa ämnen och prenumerationer**, **skapa prenumerationsfilter**, **skickar meddelanden** tooa avsnittet **tar emot meddelanden från en prenumeration**, och **ta bort ämnen och prenumerationer**. Mer information om ämnen och prenumerationer finns hello [nästa steg](#next-steps) avsnitt.

[!INCLUDE [howto-service-bus-topics](../../includes/howto-service-bus-topics.md)]

## <a name="create-a-nodejs-application"></a>Skapa ett Node.js-program
Skapa ett tomt Node.js-program. Instruktioner om hur du skapar en Node.js-program finns i [skapa och distribuera en Node.js-programmet tooan webbplatsen Azure], [Node.js Molntjänsten] [ Node.js Cloud Service] med hjälp av Windows PowerShell eller webbplatsen med WebMatrix.

## <a name="configure-your-application-toouse-service-bus"></a>Konfigurera ditt program toouse Service Bus
toouse Service Bus, hämta hello Node.js Azure-paketet. Det här paketet innehåller en uppsättning bibliotek som kommunicerar med hello Service Bus REST-tjänster.

### <a name="use-node-package-manager-npm-tooobtain-hello-package"></a>Använd noden Package Manager (NPM) tooobtain hello-paket
1. Använd ett kommandoradsgränssnitt som **PowerShell** (Windows), **Terminal** (Mac) eller **Bash** (Unix), navigera toohello mappen där du skapade exempelprogrammet.
2. Typen **npm installera azure** i hello-kommandofönster som bör resultera i hello följande utdata:

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
3. Du kan köra hello manuellt **ls** kommandot tooverify som en **nod\_moduler** mappen har skapats. I mappen hitta den **azure** paket som innehåller hello bibliotek som du behöver tooaccess Service Bus-ämnen.

### <a name="import-hello-module"></a>Importera hello-modul
Använda anteckningar eller något annat textredigeringsprogram Lägg till följande toohello överkant hello hello **server.js** -filen för programmet hello:

```javascript
var azure = require('azure');
```

### <a name="set-up-a-service-bus-connection"></a>Konfigurera en Service Bus-anslutning
hello Azure modulen läser hello miljövariabler `AZURE_SERVICEBUS_NAMESPACE` och `AZURE_SERVICEBUS_ACCESS_KEY` information krävs tooconnect tooService Bus. Om de här miljövariablerna inte har angetts måste du ange hello kontoinformation när du anropar `createServiceBusService`.

Ett exempel på hur hello miljövariabler för ett Azure Cloud Service, se [Node.js molntjänst med lagring][Node.js Cloud Service with Storage].

Ett exempel på hur hello miljövariabler för en Azure-webbplats finns [Node.js-Webbapp med lagring][Node.js Web Application with Storage].

## <a name="create-a-topic"></a>Skapa ett ämne
Hej **ServiceBusService** objekt kan du toowork med ämnen. Följande kod skapar en **ServiceBusService** objekt. Lägg till den övre delen av hello **server.js** filen efter hello instruktionen tooimport hello azure modulen:

```javascript
var serviceBusService = azure.createServiceBusService();
```

Genom att anropa `createTopicIfNotExists` på hello **ServiceBusService** objekt hello angetts avsnittet returneras (om den finns) eller ett nytt avsnitt med angivna hello-namnet kommer att skapas. hello följande kod används `createTopicIfNotExists` toocreate eller ansluta toohello ämne med namnet `MyTopic`:

```javascript
serviceBusService.createTopicIfNotExists('MyTopic',function(error){
    if(!error){
        // Topic was created or exists
        console.log('topic created or exists.');
    }
});
```

Hej `createServiceBusService` metoden stöder också ytterligare alternativ som aktiverar toooverride avsnittet standardinställningarna, till exempel time to live-meddelande eller avsnittet maximala storlek. hello följande exempel anger hello maximala avsnittet storlek too5GB med tid toolive 1 minut:

```javascript
var topicOptions = {
        MaxSizeInMegabytes: '5120',
        DefaultMessageTimeToLive: 'PT1M'
    };

serviceBusService.createTopicIfNotExists('MyTopic', topicOptions, function(error){
    if(!error){
        // topic was created or exists
    }
});
```

### <a name="filters"></a>Filter
Valfria filtrering åtgärder kan vara tillämpade toooperations utförs med hjälp av **ServiceBusService**. Filtrering operations kan innehålla loggning, försöker automatiskt igen och så vidare. Filtren är objekt som implementerar en metod med hello signatur:

```javascript
function handle (requestOptions, next)
```

När du utför förbearbetning på hello begäran alternativ måste hello metodanrop `next`, skicka ett återanrop med hello följande signatur:

```javascript
function (returnObject, finalCallback, next)
```

I den här motringning och efter bearbetning hello `returnObject` (hello svar från hello begäran toohello server), hello-återanrop måste tooeither anropa sedan om den finns toocontinue bearbetning filter eller anropa `finalCallback` annars tooend hello Service-anrop.

Två filter som implementerar logik som medföljer hello Azure SDK för Node.js, **ExponentialRetryPolicyFilter** och **LinearRetryPolicyFilter**. hello följande skapar en **ServiceBusService** objekt som använder hello **ExponentialRetryPolicyFilter**:

```javascript
var retryOperations = new azure.ExponentialRetryPolicyFilter();
var serviceBusService = azure.createServiceBusService().withFilter(retryOperations);
```

## <a name="create-subscriptions"></a>Skapa prenumerationer
Ämnesprenumerationer skapas också med hello **ServiceBusService** objekt. Prenumerationer är namngivna och kan ha ett valfritt filter som begränsar hello uppsättning meddelanden som har levererats toohello prenumerationens virtuella kö.

> [!NOTE]
> Prenumerationer är beständiga och fortsätter tooexist förrän antingen de eller hello avsnittet de är kopplade till, tas bort. Om ditt program innehåller logik toocreate en prenumeration, det först ska kontrollera om det finns redan hello-prenumeration med hjälp av den `getSubscription` metoden.
>
>

### <a name="create-a-subscription-with-hello-default-matchall-filter"></a>Skapa en prenumeration med hello standardfiltret (MatchAll)
Hej **MatchAll** filtret är hello standardfilter som används om inget filter anges när en ny prenumeration skapas. När hello **MatchAll** filter används, alla meddelanden publicerade toohello avsnittet placeras i prenumerationens virtuella kö. hello följande exempel skapas en prenumeration med namnet ”AllMessages” och använder hello standard **MatchAll** filter.

```javascript
serviceBusService.createSubscription('MyTopic','AllMessages',function(error){
    if(!error){
        // subscription created
    }
});
```

### <a name="create-subscriptions-with-filters"></a>Skapa prenumerationer med filter
Du kan även skapa filter som gör att du tooscope vilka meddelanden som skickas tooa avsnittet visas inom en viss ämnesprenumeration.

hello mest flexibla typen av filter som stöds av prenumerationerna är den **SqlFilter**, som implementerar en deluppsättning av SQL92. SQL-filter tillämpas för hälsningsmeddelande som är publicerade toohello avsnittet hello egenskaper. Mer information om hello-uttryck som kan användas med ett SQL-filter granskar hello [SqlFilter.SqlExpression] [ SqlFilter.SqlExpression] syntax.

Filter kan du lägga till tooa prenumeration hello `createRule` metod för hello **ServiceBusService** objekt. Den här metoden kan du lägga till nya filter tooan befintlig prenumeration.

> [!NOTE]
> Eftersom hello standardfiltret används automatiskt tooall nya prenumerationer, måste du först ta bort standardfiltret hello eller **MatchAll** åsidosätter eventuella filter som du kan ange. Du kan ta bort hello standardregel med hello `deleteRule` metod för den **ServiceBusService** objekt.
>
>

hello följande exempel skapas en prenumeration med namnet `HighMessages` med en **SqlFilter** som endast väljer meddelanden som har en anpassad `messagenumber` egenskap som är större än 3:

```javascript
serviceBusService.createSubscription('MyTopic', 'HighMessages', function (error){
    if(!error){
        // subscription created
        rule.create();
    }
});
var rule={
    deleteDefault: function(){
        serviceBusService.deleteRule('MyTopic',
            'HighMessages',
            azure.Constants.ServiceBusConstants.DEFAULT_RULE_NAME,
            rule.handleError);
    },
    create: function(){
        var ruleOptions = {
            sqlExpressionFilter: 'messagenumber > 3'
        };
        rule.deleteDefault();
        serviceBusService.createRule('MyTopic',
            'HighMessages',
            'HighMessageFilter',
            ruleOptions,
            rule.handleError);
    },
    handleError: function(error){
        if(error){
            console.log(error)
        }
    }
}
```

På liknande sätt hello följande exempel skapas en prenumeration med namnet `LowMessages` med en **SqlFilter** som endast väljer meddelanden som har en `messagenumber` egenskap som är mindre än eller lika med too3:

```javascript
serviceBusService.createSubscription('MyTopic', 'LowMessages', function (error){
    if(!error){
        // subscription created
        rule.create();
    }
});
var rule={
    deleteDefault: function(){
        serviceBusService.deleteRule('MyTopic',
            'LowMessages',
            azure.Constants.ServiceBusConstants.DEFAULT_RULE_NAME,
            rule.handleError);
    },
    create: function(){
        var ruleOptions = {
            sqlExpressionFilter: 'messagenumber <= 3'
        };
        rule.deleteDefault();
        serviceBusService.createRule('MyTopic',
            'LowMessages',
            'LowMessageFilter',
            ruleOptions,
            rule.handleError);
    },
    handleError: function(error){
        if(error){
            console.log(error)
        }
    }
}
```

När ett meddelande skickas nu för`MyTopic`, kommer alltid levereras till mottagare som prenumererar på toohello `AllMessages` avsnittet prenumeration och levereras selektivt tooreceivers prenumererar toohello `HighMessages` och `LowMessages` ämnesprenumerationer (beroende på hello meddelandeinnehåll).

## <a name="how-toosend-messages-tooa-topic"></a>Hur toosend meddelanden tooa avsnittet
toosend meddelandet tooa Service Bus-ämne programmet måste använda den `sendTopicMessage` metod för hello **ServiceBusService** objekt.
Meddelanden som skickas tooService Bus-ämnen är **BrokeredMessage** objekt.
**BrokeredMessage** objekt har en uppsättning standardegenskaper (t.ex `Label` och `TimeToLive`), en ordlista som används toohold anpassade programspecifika egenskaper och en brödtext med strängdata. Ett program kan konfigurera hello brödtext hello-meddelande genom att skicka ett strängvärde till hello `sendTopicMessage` och alla obligatoriska standardegenskaper fylls i med standardvärden.

hello exemplet nedan visar hur toosend fem testmeddelanden till `MyTopic`. Observera att hello `messagenumber` -värdet för varje meddelande varierar på hello upprepning av loopen hello (detta avgör vilka prenumerationer som får det):

```javascript
var message = {
    body: '',
    customProperties: {
        messagenumber: 0
    }
}

for (i = 0;i < 5;i++) {
    message.customProperties.messagenumber=i;
    message.body='This is Message #'+i;
    serviceBusService.sendTopicMessage(topic, message, function(error) {
      if (error) {
        console.log(error);
      }
    });
}
```

Service Bus-ämnena stöder en maximal meddelandestorlek på 256 KB i hello [standardnivån](service-bus-premium-messaging.md) och 1 MB på hello [premiumnivån](service-bus-premium-messaging.md). hello-huvudet som innehåller hello standard- och anpassade programegenskaperna, kan ha en maximal storlek på 64 KB. Det finns ingen gräns hello antalet meddelanden som lagras i ett ämne men det finns ett tak på hello totala storleken på hälsningsmeddelande som innehas av ett ämne. Den här ämnesstorleken definieras när ämnet skapas, med en övre gräns på 5 GB.

## <a name="receive-messages-from-a-subscription"></a>Ta emot meddelanden från en prenumeration
Meddelanden tas emot från en prenumeration med hjälp av den `receiveSubscriptionMessage` metod på hello **ServiceBusService** objekt. Som standard tas meddelanden bort från hello prenumeration som de läses; men du kan läsa (titt) och låsa hello-meddelande utan att ta bort den från hello prenumeration genom att ange hello valfri parameter `isPeekLock` för**SANT**.

hello standardbeteendet för läsning och ta bort hello-meddelande som en del av receive-åtgärden är hello enklaste modellen och fungerar bäst för scenarier där ett program kan tolerera icke-bearbetning av ett meddelande i hello händelse av fel. toounderstand, Överväg ett scenario där konsumenten problem hello mottagning av begäran och sedan kraschar innan bearbetningen. Eftersom Service Bus kommer att ha markerat hello meddelandet som Förbrukat, sedan när hello programmet startas om och börjar förbruka meddelanden igen, att ha missat hello-meddelande som har förbrukats tidigare toohello krascher.

Om hello `isPeekLock` parameter har angetts för**SANT**, hello får blir en åtgärd i två steg, vilket gör det möjligt toosupport program som inte tolererar att ett meddelande saknas. När Service Bus tar emot en begäran, hittar hello nästa meddelande toobe förbrukas, låser det tooprevent andra användare tar emot det och returnerar sedan toohello program.
När hello programmet har avslutat bearbetningen hello-meddelande (eller lagrar det på ett tillförlitligt sätt för framtida bearbetning), Slutför hello andra steget i processen genom att anropa **deleteMessage** metod och ge meddelandet toobe ta bort som en parameter. Hej **deleteMessage** metoden kommer Markera hello meddelandet som Förbrukat och ta bort den från hello prenumeration.

hello exemplet nedan visar hur meddelanden kan tas emot och bearbetat använder `receiveSubscriptionMessage`. hello exempel först tar emot och tar bort ett meddelande från hello 'Highmessages' prenumeration och får ett meddelande från hello 'HighMessages' en prenumeration med hjälp av `isPeekLock` ange tootrue. Hello-meddelande med raderas sedan `deleteMessage`:

```javascript
serviceBusService.receiveSubscriptionMessage('MyTopic', 'LowMessages', function(error, receivedMessage){
    if(!error){
        // Message received and deleted
        console.log(receivedMessage);
    }
});
serviceBusService.receiveSubscriptionMessage('MyTopic', 'HighMessages', { isPeekLock: true }, function(error, lockedMessage){
    if(!error){
        // Message received and locked
        console.log(lockedMessage);
        serviceBusService.deleteMessage(lockedMessage, function (deleteError){
            if(!deleteError){
                // Message deleted
                console.log('message has been deleted.');
            }
        }
    }
});
```

## <a name="how-toohandle-application-crashes-and-unreadable-messages"></a>Hur toohandle programmet kraschar och oläsbara meddelanden
Service Bus innehåller funktioner toohelp du att återställa fel i programmet eller problem med bearbetning av ett meddelande. Om ett mottagarprogram inte kan tooprocess hello meddelande av någon anledning, så kan det anropa hello `unlockMessage` -metoden i den **ServiceBusService** objekt. Detta orsakar Service Bus toounlock meddelandet inom hello prenumerationen och göra den tillgänglig toobe tas emot igen, antingen hello av samma förbrukning av ett program eller ett annat användningsprogram.

Det finns också en tidsgräns som är associerad med ett meddelande som ligger låst i prenumerationen och om hello program inte tooprocess hello-meddelande innan timeout för lås hello går ut (till exempel om hello programmet kraschar), kommer Service Bus låser upp hello-meddelande automatiskt och gör den tillgänglig toobe tas emot igen.

Hello händelse som hello programmet kraschar efter hello meddelandet har bearbetats men innan hello `deleteMessage` metoden anropas sedan hello-meddelande kommer att levereras på nytt toohello program när den startas om. Det här kallas ofta *At Least Once Processing*, det vill säga, varje meddelande bearbetas minst en gång men i vissa situationer hello samma meddelande levereras. Om hello scenariot inte tolererar duplicerad bearbetning, sedan bör programutvecklarna lägga till ytterligare logik tootheir duplicerad meddelandeleverans för programmet toohandle. Detta uppnås ofta med hjälp av den **MessageId** -egenskapen för hello-meddelande, förblir konstant under alla leveransförsök.

## <a name="delete-topics-and-subscriptions"></a>Ta bort ämnen och prenumerationer
Ämnen och prenumerationer är beständiga och måste vara explicit bort antingen via hello [Azure-portalen] [ Azure portal] eller programmässigt.
hello exemplet nedan visar hur toodelete hello avsnittet med namnet `MyTopic`:

```javascript
serviceBusService.deleteTopic('MyTopic', function (error) {
    if (error) {
        console.log(error);
    }
});
```

Om du tar bort ett ämne tas även bort alla prenumerationer som är registrerade med hello-avsnittet. Prenumerationer kan även tas bort separat. I följande exempel visas hur toodelete en prenumeration med namnet `HighMessages` från hello `MyTopic` avsnittet:

```javascript
serviceBusService.deleteSubscription('MyTopic', 'HighMessages', function (error) {
    if(error) {
        console.log(error);
    }
});
```

## <a name="next-steps"></a>Nästa steg
Nu när du har lärt dig grunderna hello i Service Bus-ämnen, följa dessa länkar toolearn mer.

* Se [köer, ämnen och prenumerationer][Queues, topics, and subscriptions].
* API-referens för [SqlFilter][SqlFilter].
* Besök hello [Azure SDK för noden] [ Azure SDK for Node] databasen på GitHub.

[Azure SDK for Node]: https://github.com/Azure/azure-sdk-for-node
[Azure portal]: https://portal.azure.com
[SqlFilter.SqlExpression]: service-bus-messaging-sql-filter.md
[Queues, topics, and subscriptions]: service-bus-queues-topics-subscriptions.md
[SqlFilter]: /dotnet/api/microsoft.servicebus.messaging.sqlfilter
[Node.js Cloud Service]: ../cloud-services/cloud-services-nodejs-develop-deploy-app.md
[skapa och distribuera en Node.js-programmet tooan webbplatsen Azure]: ../app-service-web/app-service-web-get-started-nodejs.md
[Node.js Cloud Service with Storage]: ../cloud-services/cloud-services-nodejs-develop-deploy-app.md
[Node.js Web Application with Storage]:../cosmos-db/table-storage-cloud-service-nodejs.md
