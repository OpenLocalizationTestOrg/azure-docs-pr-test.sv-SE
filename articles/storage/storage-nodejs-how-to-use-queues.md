---
title: "aaaHow toouse Queue storage från Node.js | Microsoft Docs"
description: "Lär dig hur toouse hello Azure Queue service toocreate och ta bort köer och infoga, hämta och ta bort meddelanden. Exempel som skrivits i Node.js."
services: storage
documentationcenter: nodejs
author: robinsh
manager: timlt
editor: tysonn
ms.assetid: a8a92db0-4333-43dd-a116-28b3147ea401
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: nodejs
ms.topic: article
ms.date: 12/08/2016
ms.author: robinsh
ms.openlocfilehash: 977e5994c0be1b5d71c60b7479698ccb694ab860
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="how-toouse-queue-storage-from-nodejs"></a>Hur toouse Queue storage från Node.js
[!INCLUDE [storage-selector-queue-include](../../includes/storage-selector-queue-include.md)]

[!INCLUDE [storage-check-out-samples-all](../../includes/storage-check-out-samples-all.md)]

## <a name="overview"></a>Översikt
Den här guiden visar hur hello tooperform vanliga scenarier med hjälp av Microsoft Azure-kötjänsten. hello exempel skrivs med hello Node.js API. hello beskrivs scenarier där **infoga**, **granskning**, **komma**, och **bort** kö meddelanden, samt  **Skapa och ta bort köer**.

[!INCLUDE [storage-queue-concepts-include](../../includes/storage-queue-concepts-include.md)]

[!INCLUDE [storage-create-account-include](../../includes/storage-create-account-include.md)]

## <a name="create-a-nodejs-application"></a>Skapa en Node.js-program
Skapa ett tomt Node.js-program. Instruktioner om att skapa en Node.js-program finns i [skapa en Node.js-webbapp i Azure App Service], [skapa och distribuera en Node.js-programmet tooan Azure Cloud Service] med Windows PowerShell eller [ Skapa och distribuera en Node.js web app tooAzure med hjälp av Web Matrix].

## <a name="configure-your-application-tooaccess-storage"></a>Konfigurera ditt program tooAccess lagring
toouse Azure storage, behöver du hello Azure Storage SDK för Node.js som innehåller en uppsättning bekvämlighet bibliotek som kommunicerar med hello storage REST-tjänster.

### <a name="use-node-package-manager-npm-tooobtain-hello-package"></a>Använd noden Package Manager (NPM) tooobtain hello-paket
1. Använd ett kommandoradsgränssnitt som **PowerShell** (Windows), **Terminal** (Mac) eller **Bash** (Unix), navigera toohello mappen där du skapade exempelprogrammet.
2. Typen **npm installera azure-lagring** i hello kommandofönstret. Utdata från kommandot hello är liknande toohello följande exempel.
 
    ```
    azure-storage@0.5.0 node_modules\azure-storage
    +-- extend@1.2.1
    +-- xmlbuilder@0.4.3
    +-- mime@1.2.11
    +-- node-uuid@1.4.3
    +-- validator@3.22.2
    +-- underscore@1.4.4
    +-- readable-stream@1.0.33 (string_decoder@0.10.31, isarray@0.0.1, inherits@2.0.1, core-util-is@1.0.1)
    +-- xml2js@0.2.7 (sax@0.5.2)
    +-- request@2.57.0 (caseless@0.10.0, aws-sign2@0.5.0, forever-agent@0.6.1, stringstream@0.0.4, oauth-sign@0.8.0, tunnel-agent@0.4.1, isstream@0.1.2, json-stringify-safe@5.0.1, bl@0.9.4, combined-stream@1.0.5, qs@3.1.0, mime-types@2.0.14, form-data@0.2.0, http-signature@0.11.0, tough-cookie@2.0.0, hawk@2.3.1, har-validator@1.8.0)
    ```

3. Du kan köra hello manuellt **ls** kommandot tooverify som en **nod\_moduler** mappen har skapats. I mappen hittar du hello **azure storage** paket som innehåller hello bibliotek som du behöver åtkomst till lagring.

### <a name="import-hello-package"></a>Importera hello-paket
Använda anteckningar eller något annat textredigeringsprogram, Lägg till hello följande toohello upp den **server.js** -filen för programmet hello där du ska toouse lagring:

```
var azure = require('azure-storage');
```

## <a name="setup-an-azure-storage-connection"></a>Skapa ett Azure Storage-anslutning
hello azure modulen läser hello miljövariabler AZURE\_lagring\_konto och AZURE\_lagring\_åtkomst\_nyckel eller AZURE\_lagring\_anslutning \_Sträng för information som krävs för tooconnect tooyour Azure storage-konto. Om de här miljövariablerna inte har angetts måste du ange hello kontoinformation när du anropar **createQueueService**.

Ett exempel på inställningen hello miljövariabler i hello [Azure Portal](https://portal.azure.com) en Azure-webbplats finns [Node.js web app med hello Azure Tabelltjänsten].

## <a name="how-to-create-a-queue"></a>Så här: Skapa en kö
hello följande kod skapar en **QueueService** -objektet, vilket gör att du toowork med köer.

```
var queueSvc = azure.createQueueService();
```

Använd hello **createQueueIfNotExists** metod som returnerar hello kö som anges om det redan finns eller skapar en ny kö med hello angivna namnet om det inte redan finns.

```
queueSvc.createQueueIfNotExists('myqueue', function(error, result, response){
  if(!error){
    // Queue created or exists
  }
});
```

Om hello kön skapas `result.created` är true. Om hello kön finns, `result.created` är false.

### <a name="filters"></a>Filter
Valfria filtrering åtgärder kan vara tillämpade toooperations utförs med hjälp av **QueueService**. Filtrering operations kan innehålla loggning, försöker automatiskt igen och så vidare. Filtren är objekt som implementerar en metod med hello signatur:

```
function handle (requestOptions, next)
```

När du har gjort dess förbearbetning på hello begäran alternativ måste hello metoden toocall ”nästa” Skicka en motringning med hello följande signatur:

```
function (returnObject, finalCallback, next)
```

I den här motringning och efter bearbetning hello returnObject (hello svar från hello begäran toohello server), hello-återanrop måste tooeither anropa sedan om den finns toocontinue bearbetning filter eller helt enkelt anropa finalCallback annars tooend in hello Service-anrop.

Två filter som implementerar logik som medföljer hello Azure SDK för Node.js, **ExponentialRetryPolicyFilter** och **LinearRetryPolicyFilter**. hello följande skapar en **QueueService** objekt som använder hello **ExponentialRetryPolicyFilter**:

```
var retryOperations = new azure.ExponentialRetryPolicyFilter();
var queueSvc = azure.createQueueService().withFilter(retryOperations);
```

## <a name="how-to-insert-a-message-into-a-queue"></a>Så här: Infoga ett meddelande i en kö
tooinsert ett meddelande i en kö, Använd hello **createMessage** metod för att skapa ett nytt meddelande och Lägg till den toohello kön.

```
queueSvc.createMessage('myqueue', "Hello world!", function(error, result, response){
  if(!error){
    // Message inserted
  }
});
```

## <a name="how-to-peek-at-hello-next-message"></a>Så här: Granska hello nästa meddelande
Du kan kika på hello-meddelande i hello framför en kö utan att ta bort den från hello kö genom att anropa hello **peekMessages** metod. Som standard **peekMessages** tittar på ett enda meddelande.

```
queueSvc.peekMessages('myqueue', function(error, result, response){
  if(!error){
    // Message text is in messages[0].messageText
  }
});
```

Hej `result` innehåller hello-meddelande.

> [!NOTE]
> Med hjälp av **peekMessages** när det finns inga meddelanden i kö för hello inte returnerar ett fel, men inga meddelanden ska returneras.
> 
> 

## <a name="how-to-dequeue-hello-next-message"></a>Så här: Status Created hello nästa meddelande
Bearbetning av ett meddelande är en process i två steg:

1. Hello-meddelande har status Created.
2. Ta bort hello-meddelande.

toodequeue ett meddelande som använder **GetMessage**. Detta gör hälsningsmeddelande i hello kö, så att inga andra klienter kan bearbeta dem. När programmet har bearbetats ett meddelande kan anropa **deleteMessage** toodelete från hello kön. hello följande exempel hämtar ett meddelande och sedan tar bort den:

```
queueSvc.getMessages('myqueue', function(error, result, response){
  if(!error){
    // Message text is in messages[0].messageText
    var message = result[0];
    queueSvc.deleteMessage('myqueue', message.messageId, message.popReceipt, function(error, response){
      if(!error){
        //message deleted
      }
    });
  }
});
```

> [!NOTE]
> Som standard döljs bara ett meddelande i 30 sekunder, efter vilken den är synlig tooother klienter. Du kan ange ett annat värde med hjälp av `options.visibilityTimeout` med **GetMessage**.
> 
> [!NOTE]
> Med hjälp av **GetMessage** när det finns inga meddelanden i kö för hello inte returnerar ett fel, men inga meddelanden ska returneras.
> 
> 

## <a name="how-to-change-hello-contents-of-a-queued-message"></a>Så här: Ändra hello innehållet i meddelandet i kön
Du kan ändra hello innehållet i ett meddelande direkt i hello kön med **updateMessage**. följande exempel hello uppdateringar hello texten för ett meddelande:

```
queueSvc.getMessages('myqueue', function(error, result, response){
  if(!error){
    // Got hello message
    var message = result[0];
    queueSvc.updateMessage('myqueue', message.messageId, message.popReceipt, 10, {messageText: 'new text'}, function(error, result, response){
      if(!error){
        // Message updated successfully
      }
    });
  }
});
```

## <a name="how-to-additional-options-for-dequeuing-messages"></a>Så här: Ytterligare alternativ för mellan köer meddelanden
Det finns två sätt som du kan anpassa meddelandehämtningen från en kö:

* `options.numOfMessages`-Hämta en grupp med meddelanden (upp too32.)
* `options.visibilityTimeout`-Ange en tidsgräns för osynlighet längre eller kortare.

hello följande exempel används hello **GetMessage** metoden tooget 15 meddelanden i ett anrop. Sedan bearbetas varje meddelande med hjälp av en for-loop. Den anger också hello osynlighet timeout toofive minuter för alla meddelanden som returneras av den här metoden.

```
queueSvc.getMessages('myqueue', {numOfMessages: 15, visibilityTimeout: 5 * 60}, function(error, result, response){
  if(!error){
    // Messages retreived
    for(var index in result){
      // text is available in result[index].messageText
      var message = result[index];
      queueSvc.deleteMessage(queueName, message.messageId, message.popReceipt, function(error, response){
        if(!error){
          // Message deleted
        }
      });
    }
  }
});
```

## <a name="how-to-get-hello-queue-length"></a>Så här: Hämta hello Kölängd
Hej **getQueueMetadata** returnerar metadata om hello kö, inklusive hello ungefärliga antalet väntande meddelanden i kön hello.

```
queueSvc.getQueueMetadata('myqueue', function(error, result, response){
  if(!error){
    // Queue length is available in result.approximateMessageCount
  }
});
```

## <a name="how-to-list-queues"></a>Så här: Listan köer
tooretrieve en lista över köer, Använd **listQueuesSegmented**. tooretrieve en lista filtreras efter ett specifikt prefix använder **listQueuesSegmentedWithPrefix**.

```
queueSvc.listQueuesSegmented(null, function(error, result, response){
  if(!error){
    // result.entries contains hello list of queues
  }
});
```

Om inte det går att returnera alla köer `result.continuationToken` kan användas som hello första parametern för **listQueuesSegmented** eller hello andra parametern för **listQueuesSegmentedWithPrefix** tooretrieve resultat.

## <a name="how-to-delete-a-queue"></a>Så här: Ta bort en kö
toodelete en kö och alla hälsningsmeddelande finns i den, anropa den **deleteQueue** metod för hello köobjekt.

```
queueSvc.deleteQueue(queueName, function(error, response){
  if(!error){
    // Queue has been deleted
  }
});
```

tooclear alla meddelanden från en kö utan att ta bort det, Använd **clearMessages**.

## <a name="how-to-work-with-shared-access-signatures"></a>Så här: arbeta med signaturer för delad åtkomst
Delad åtkomst signaturer (SAS) är ett säkert sätt tooprovide detaljerade åtkomst tooqueues utan att erbjuda dina lagringskontonamn eller nycklar. Säkerhetsassociationer är ofta använda tooprovide begränsad åtkomst tooyour köer, till exempel att tillåta en mobil app toosubmit meddelanden.

En betrodda program, till exempel en molnbaserad tjänst genererar en SAS med hello **generateSharedAccessSignature** av hello **QueueService**, och ger den tooan inte är betrodd eller delvis betrodda program. Till exempel en mobil app. hello SAS genereras med en princip som beskriver hello start- och slutdatum under vilka hello SAS är giltigt, samt hello åtkomst nivån beviljade toohello SAS innehavaren.

hello följande exempel skapar en ny princip för delad åtkomst som gör att hello SAS innehavaren tooadd toohello kön och upphör att gälla 100 minuter efter hello när det skapas.

```
var startDate = new Date();
var expiryDate = new Date(startDate);
expiryDate.setMinutes(startDate.getMinutes() + 100);
startDate.setMinutes(startDate.getMinutes() - 100);

var sharedAccessPolicy = {
  AccessPolicy: {
    Permissions: azure.QueueUtilities.SharedAccessPermissions.ADD,
    Start: startDate,
    Expiry: expiryDate
  }
};

var queueSAS = queueSvc.generateSharedAccessSignature('myqueue', sharedAccessPolicy);
var host = queueSvc.host;
```

Observera att information om hello värden måste anges också som krävs när hello SAS innehavaren försöker tooaccess hello kön.

Hej klientprogrammet och sedan använder hello SAS med **QueueServiceWithSAS** tooperform åtgärder mot hello kön. följande exempel hello ansluter toohello kön och skapar ett meddelande.

```
var sharedQueueService = azure.createQueueServiceWithSas(host, queueSAS);
sharedQueueService.createMessage('myqueue', 'Hello world from SAS!', function(error, result, response){
  if(!error){
    //message added
  }
});
```

Eftersom hello SAS genererades med Lägg till åtkomst om ett försök gjordes tooread, uppdatera eller ta bort meddelanden, returneras ett fel.

### <a name="access-control-lists"></a>Listor för åtkomstkontroll
Du kan också använda en åtkomstkontrollista (ACL) tooset hello åtkomstprincip för en SAS. Detta är användbart om du vill tooallow flera klienter tooaccess hello kön, men ange olika principer för varje klient.

En ACL implementeras med hjälp av en matris med principer för åtkomst med ett ID som är associerade med varje princip. hello följande exempel definierar två principer; en för 'Användare1' och en för 'användare2':

```
var sharedAccessPolicy = {
  user1: {
    Permissions: azure.QueueUtilities.SharedAccessPermissions.PROCESS,
    Start: startDate,
    Expiry: expiryDate
  },
  user2: {
    Permissions: azure.QueueUtilities.SharedAccessPermissions.ADD,
    Start: startDate,
    Expiry: expiryDate
  }
};
```

följande exempel hämtar hello hello aktuella ACL för **MinKö**, lägger till nya principer för hello med **setQueueAcl**. Den här metoden kan:

```
var extend = require('extend');
queueSvc.getQueueAcl('myqueue', function(error, result, response) {
  if(!error){
    var newSignedIdentifiers = extend(true, result.signedIdentifiers, sharedAccessPolicy);
    queueSvc.setQueueAcl('myqueue', newSignedIdentifiers, function(error, result, response){
      if(!error){
        // ACL set
      }
    });
  }
});
```

En gång hello ACL har ställts in, du kan sedan skapa en SAS baserat på hello-ID för en princip. hello följande exempel skapas en ny SAS för 'användare2':

```
queueSAS = queueSvc.generateSharedAccessSignature('myqueue', { Id: 'user2' });
```

## <a name="next-steps"></a>Nästa steg
Nu när du har lärt dig hello grunderna i queue storage kan du följa dessa länkar toolearn om mer komplexa lagringsuppgifter.

* Besök hello [Azure Storage-teamets blogg][Azure Storage Team Blog].
* Besök hello [Azure Storage SDK: N för noden] [ Azure Storage SDK for Node] databasen på GitHub.

[Azure Storage SDK for Node]: https://github.com/Azure/azure-storage-node
[using hello REST API]: http://msdn.microsoft.com/library/azure/hh264518.aspx
[Azure Portal]: https://portal.azure.com
[skapa en Node.js-webbapp i Azure App Service]: ../app-service-web/app-service-web-get-started-nodejs.md
[Node.js web app med hello Azure Tabelltjänsten]: ../app-service-web/storage-nodejs-use-table-storage-web-site.md


[skapa och distribuera en Node.js-programmet tooan Azure Cloud Service]: ../cloud-services/cloud-services-nodejs-develop-deploy-app.md
[Azure Storage Team Blog]: http://blogs.msdn.com/b/windowsazurestorage/
[ Skapa och distribuera en Node.js web app tooAzure med hjälp av Web Matrix]: ../app-service-web/web-sites-nodejs-use-webmatrix.md
