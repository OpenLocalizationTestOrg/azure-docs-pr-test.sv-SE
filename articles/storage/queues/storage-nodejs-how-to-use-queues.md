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
ms.openlocfilehash: 7e9778da4efa69f2e9d8fd480b9b6f5ace85951e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="how-toouse-queue-storage-from-nodejs"></a><span data-ttu-id="cbd76-104">Hur toouse Queue storage från Node.js</span><span class="sxs-lookup"><span data-stu-id="cbd76-104">How toouse Queue storage from Node.js</span></span>
[!INCLUDE [storage-selector-queue-include](../../../includes/storage-selector-queue-include.md)]

[!INCLUDE [storage-check-out-samples-all](../../../includes/storage-check-out-samples-all.md)]

## <a name="overview"></a><span data-ttu-id="cbd76-105">Översikt</span><span class="sxs-lookup"><span data-stu-id="cbd76-105">Overview</span></span>
<span data-ttu-id="cbd76-106">Den här guiden visar hur hello tooperform vanliga scenarier med hjälp av Microsoft Azure-kötjänsten.</span><span class="sxs-lookup"><span data-stu-id="cbd76-106">This guide shows you how tooperform common scenarios using hello Microsoft Azure Queue service.</span></span> <span data-ttu-id="cbd76-107">hello exempel skrivs med hello Node.js API.</span><span class="sxs-lookup"><span data-stu-id="cbd76-107">hello samples are written using hello Node.js API.</span></span> <span data-ttu-id="cbd76-108">hello beskrivs scenarier där **infoga**, **granskning**, **komma**, och **bort** kö meddelanden, samt  **Skapa och ta bort köer**.</span><span class="sxs-lookup"><span data-stu-id="cbd76-108">hello scenarios covered include **inserting**, **peeking**, **getting**, and **deleting** queue messages, as well as **creating and deleting queues**.</span></span>

[!INCLUDE [storage-queue-concepts-include](../../../includes/storage-queue-concepts-include.md)]

[!INCLUDE [storage-create-account-include](../../../includes/storage-create-account-include.md)]

## <a name="create-a-nodejs-application"></a><span data-ttu-id="cbd76-109">Skapa en Node.js-program</span><span class="sxs-lookup"><span data-stu-id="cbd76-109">Create a Node.js Application</span></span>
<span data-ttu-id="cbd76-110">Skapa ett tomt Node.js-program.</span><span class="sxs-lookup"><span data-stu-id="cbd76-110">Create a blank Node.js application.</span></span> <span data-ttu-id="cbd76-111">Instruktioner om att skapa en Node.js-program finns i [skapa en Node.js-webbapp i Azure App Service](../../app-service-web/app-service-web-get-started-nodejs.md), [skapa och distribuera en Node.js-programmet tooan Azure Cloud Service](../../cloud-services/cloud-services-nodejs-develop-deploy-app.md) med Windows PowerShell eller [ Skapa och distribuera en Node.js web app tooAzure med hjälp av Web Matrix](https://www.microsoft.com/web/webmatrix/).</span><span class="sxs-lookup"><span data-stu-id="cbd76-111">For instructions creating a Node.js application, see [Create a Node.js web app in Azure App Service](../../app-service-web/app-service-web-get-started-nodejs.md), [Build and deploy a Node.js application tooan Azure Cloud Service](../../cloud-services/cloud-services-nodejs-develop-deploy-app.md) using Windows PowerShell, or [Build and deploy a Node.js web app tooAzure using Web Matrix](https://www.microsoft.com/web/webmatrix/).</span></span>

## <a name="configure-your-application-tooaccess-storage"></a><span data-ttu-id="cbd76-112">Konfigurera ditt program tooAccess lagring</span><span class="sxs-lookup"><span data-stu-id="cbd76-112">Configure Your Application tooAccess Storage</span></span>
<span data-ttu-id="cbd76-113">toouse Azure storage, behöver du hello Azure Storage SDK för Node.js som innehåller en uppsättning bekvämlighet bibliotek som kommunicerar med hello storage REST-tjänster.</span><span class="sxs-lookup"><span data-stu-id="cbd76-113">toouse Azure storage, you need hello Azure Storage SDK for Node.js, which includes a set of convenience libraries that communicate with hello storage REST services.</span></span>

### <a name="use-node-package-manager-npm-tooobtain-hello-package"></a><span data-ttu-id="cbd76-114">Använd noden Package Manager (NPM) tooobtain hello-paket</span><span class="sxs-lookup"><span data-stu-id="cbd76-114">Use Node Package Manager (NPM) tooobtain hello package</span></span>
1. <span data-ttu-id="cbd76-115">Använd ett kommandoradsgränssnitt som **PowerShell** (Windows), **Terminal** (Mac) eller **Bash** (Unix), navigera toohello mappen där du skapade exempelprogrammet.</span><span class="sxs-lookup"><span data-stu-id="cbd76-115">Use a command-line interface such as **PowerShell** (Windows,) **Terminal** (Mac,) or **Bash** (Unix), navigate toohello folder where you created your sample application.</span></span>
2. <span data-ttu-id="cbd76-116">Typen **npm installera azure-lagring** i hello kommandofönstret.</span><span class="sxs-lookup"><span data-stu-id="cbd76-116">Type **npm install azure-storage** in hello command window.</span></span> <span data-ttu-id="cbd76-117">Utdata från kommandot hello är liknande toohello följande exempel.</span><span class="sxs-lookup"><span data-stu-id="cbd76-117">Output from hello command is similar toohello following example.</span></span>
 
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

3. <span data-ttu-id="cbd76-118">Du kan köra hello manuellt **ls** kommandot tooverify som en **nod\_moduler** mappen har skapats.</span><span class="sxs-lookup"><span data-stu-id="cbd76-118">You can manually run hello **ls** command tooverify that a **node\_modules** folder was created.</span></span> <span data-ttu-id="cbd76-119">I mappen hittar du hello **azure storage** paket som innehåller hello bibliotek som du behöver åtkomst till lagring.</span><span class="sxs-lookup"><span data-stu-id="cbd76-119">Inside that folder you will find hello **azure-storage** package, which contains hello libraries you need to access storage.</span></span>

### <a name="import-hello-package"></a><span data-ttu-id="cbd76-120">Importera hello-paket</span><span class="sxs-lookup"><span data-stu-id="cbd76-120">Import hello package</span></span>
<span data-ttu-id="cbd76-121">Använda anteckningar eller något annat textredigeringsprogram, Lägg till hello följande toohello upp den **server.js** -filen för programmet hello där du ska toouse lagring:</span><span class="sxs-lookup"><span data-stu-id="cbd76-121">Using Notepad or another text editor, add hello following toohello top the **server.js** file of hello application where you intend toouse storage:</span></span>

```
var azure = require('azure-storage');
```

## <a name="setup-an-azure-storage-connection"></a><span data-ttu-id="cbd76-122">Skapa ett Azure Storage-anslutning</span><span class="sxs-lookup"><span data-stu-id="cbd76-122">Setup an Azure Storage Connection</span></span>
<span data-ttu-id="cbd76-123">hello azure modulen läser hello miljövariabler AZURE\_lagring\_konto och AZURE\_lagring\_åtkomst\_nyckel eller AZURE\_lagring\_anslutning \_Sträng för information som krävs för tooconnect tooyour Azure storage-konto.</span><span class="sxs-lookup"><span data-stu-id="cbd76-123">hello azure module will read hello environment variables AZURE\_STORAGE\_ACCOUNT and AZURE\_STORAGE\_ACCESS\_KEY, or AZURE\_STORAGE\_CONNECTION\_STRING for information required tooconnect tooyour Azure storage account.</span></span> <span data-ttu-id="cbd76-124">Om de här miljövariablerna inte har angetts måste du ange hello kontoinformation när du anropar **createQueueService**.</span><span class="sxs-lookup"><span data-stu-id="cbd76-124">If these environment variables are not set, you must specify hello account information when calling **createQueueService**.</span></span>

<span data-ttu-id="cbd76-125">Ett exempel på inställningen hello miljövariabler i hello [Azure Portal](https://portal.azure.com) en Azure-webbplats finns i [Node.js web app med hjälp av hello Azure Tabelltjänsten].</span><span class="sxs-lookup"><span data-stu-id="cbd76-125">For an example of setting hello environment variables in hello [Azure Portal](https://portal.azure.com) for an Azure Website, see [Node.js web app using hello Azure Table Service].</span></span>

## <a name="how-to-create-a-queue"></a><span data-ttu-id="cbd76-126">Så här: Skapa en kö</span><span class="sxs-lookup"><span data-stu-id="cbd76-126">How To: Create a Queue</span></span>
<span data-ttu-id="cbd76-127">hello följande kod skapar en **QueueService** -objektet, vilket gör att du toowork med köer.</span><span class="sxs-lookup"><span data-stu-id="cbd76-127">hello following code creates a **QueueService** object, which enables you toowork with queues.</span></span>

```
var queueSvc = azure.createQueueService();
```

<span data-ttu-id="cbd76-128">Använd hello **createQueueIfNotExists** metod som returnerar hello kö som anges om det redan finns eller skapar en ny kö med hello angivna namnet om det inte redan finns.</span><span class="sxs-lookup"><span data-stu-id="cbd76-128">Use hello **createQueueIfNotExists** method, which returns hello specified queue if it already exists or creates a new queue with hello specified name if it does not already exist.</span></span>

```
queueSvc.createQueueIfNotExists('myqueue', function(error, result, response){
  if(!error){
    // Queue created or exists
  }
});
```

<span data-ttu-id="cbd76-129">Om hello kön skapas `result.created` är true.</span><span class="sxs-lookup"><span data-stu-id="cbd76-129">If hello queue is created, `result.created` is true.</span></span> <span data-ttu-id="cbd76-130">Om hello kön finns, `result.created` är false.</span><span class="sxs-lookup"><span data-stu-id="cbd76-130">If hello queue exists, `result.created` is false.</span></span>

### <a name="filters"></a><span data-ttu-id="cbd76-131">Filter</span><span class="sxs-lookup"><span data-stu-id="cbd76-131">Filters</span></span>
<span data-ttu-id="cbd76-132">Valfria filtrering åtgärder kan vara tillämpade toooperations utförs med hjälp av **QueueService**.</span><span class="sxs-lookup"><span data-stu-id="cbd76-132">Optional filtering operations can be applied toooperations performed using **QueueService**.</span></span> <span data-ttu-id="cbd76-133">Filtrering operations kan innehålla loggning, försöker automatiskt igen och så vidare. Filtren är objekt som implementerar en metod med hello signatur:</span><span class="sxs-lookup"><span data-stu-id="cbd76-133">Filtering operations can include logging, automatically retrying, etc. Filters are objects that implement a method with hello signature:</span></span>

```
function handle (requestOptions, next)
```

<span data-ttu-id="cbd76-134">När du har gjort dess förbearbetning på hello begäran alternativ måste hello metoden toocall ”nästa” Skicka en motringning med hello följande signatur:</span><span class="sxs-lookup"><span data-stu-id="cbd76-134">After doing its preprocessing on hello request options, hello method needs toocall "next" passing a callback with hello following signature:</span></span>

```
function (returnObject, finalCallback, next)
```

<span data-ttu-id="cbd76-135">I den här motringning och efter bearbetning hello returnObject (hello svar från hello begäran toohello server), hello-återanrop måste tooeither anropa sedan om den finns toocontinue bearbetning filter eller helt enkelt anropa finalCallback annars tooend in hello Service-anrop.</span><span class="sxs-lookup"><span data-stu-id="cbd76-135">In this callback, and after processing hello returnObject (hello response from hello request toohello server), hello callback needs tooeither invoke next if it exists toocontinue processing other filters or simply invoke finalCallback otherwise tooend up hello service invocation.</span></span>

<span data-ttu-id="cbd76-136">Två filter som implementerar logik som medföljer hello Azure SDK för Node.js, **ExponentialRetryPolicyFilter** och **LinearRetryPolicyFilter**.</span><span class="sxs-lookup"><span data-stu-id="cbd76-136">Two filters that implement retry logic are included with hello Azure SDK for Node.js, **ExponentialRetryPolicyFilter** and **LinearRetryPolicyFilter**.</span></span> <span data-ttu-id="cbd76-137">hello följande skapar en **QueueService** objekt som använder hello **ExponentialRetryPolicyFilter**:</span><span class="sxs-lookup"><span data-stu-id="cbd76-137">hello following creates a **QueueService** object that uses hello **ExponentialRetryPolicyFilter**:</span></span>

```
var retryOperations = new azure.ExponentialRetryPolicyFilter();
var queueSvc = azure.createQueueService().withFilter(retryOperations);
```

## <a name="how-to-insert-a-message-into-a-queue"></a><span data-ttu-id="cbd76-138">Så här: Infoga ett meddelande i en kö</span><span class="sxs-lookup"><span data-stu-id="cbd76-138">How To: Insert a Message into a Queue</span></span>
<span data-ttu-id="cbd76-139">tooinsert ett meddelande i en kö, Använd hello **createMessage** metod för att skapa ett nytt meddelande och Lägg till den toohello kön.</span><span class="sxs-lookup"><span data-stu-id="cbd76-139">tooinsert a message into a queue, use hello **createMessage** method to create a new message and add it toohello queue.</span></span>

```
queueSvc.createMessage('myqueue', "Hello world!", function(error, result, response){
  if(!error){
    // Message inserted
  }
});
```

## <a name="how-to-peek-at-hello-next-message"></a><span data-ttu-id="cbd76-140">Så här: Granska hello nästa meddelande</span><span class="sxs-lookup"><span data-stu-id="cbd76-140">How To: Peek at hello Next Message</span></span>
<span data-ttu-id="cbd76-141">Du kan kika på hello-meddelande i hello framför en kö utan att ta bort den från hello kö genom att anropa hello **peekMessages** metod.</span><span class="sxs-lookup"><span data-stu-id="cbd76-141">You can peek at hello message in hello front of a queue without removing it from hello queue by calling hello **peekMessages** method.</span></span> <span data-ttu-id="cbd76-142">Som standard **peekMessages** tittar på ett enda meddelande.</span><span class="sxs-lookup"><span data-stu-id="cbd76-142">By default, **peekMessages** peeks at a single message.</span></span>

```
queueSvc.peekMessages('myqueue', function(error, result, response){
  if(!error){
    // Message text is in messages[0].messageText
  }
});
```

<span data-ttu-id="cbd76-143">Hej `result` innehåller hello-meddelande.</span><span class="sxs-lookup"><span data-stu-id="cbd76-143">hello `result` contains hello message.</span></span>

> [!NOTE]
> <span data-ttu-id="cbd76-144">Med hjälp av **peekMessages** när det finns inga meddelanden i kö för hello inte returnerar ett fel, men inga meddelanden ska returneras.</span><span class="sxs-lookup"><span data-stu-id="cbd76-144">Using **peekMessages** when there are no messages in hello queue will not return an error, however no messages will be returned.</span></span>
> 
> 

## <a name="how-to-dequeue-hello-next-message"></a><span data-ttu-id="cbd76-145">Så här: Status Created hello nästa meddelande</span><span class="sxs-lookup"><span data-stu-id="cbd76-145">How To: Dequeue hello Next Message</span></span>
<span data-ttu-id="cbd76-146">Bearbetning av ett meddelande är en process i två steg:</span><span class="sxs-lookup"><span data-stu-id="cbd76-146">Processing a message is a two-stage process:</span></span>

1. <span data-ttu-id="cbd76-147">Hello-meddelande har status Created.</span><span class="sxs-lookup"><span data-stu-id="cbd76-147">Dequeue hello message.</span></span>
2. <span data-ttu-id="cbd76-148">Ta bort hello-meddelande.</span><span class="sxs-lookup"><span data-stu-id="cbd76-148">Delete hello message.</span></span>

<span data-ttu-id="cbd76-149">toodequeue ett meddelande som använder **GetMessage**.</span><span class="sxs-lookup"><span data-stu-id="cbd76-149">toodequeue a message, use **getMessages**.</span></span> <span data-ttu-id="cbd76-150">Detta gör hälsningsmeddelande i hello kö, så att inga andra klienter kan bearbeta dem.</span><span class="sxs-lookup"><span data-stu-id="cbd76-150">This makes hello messages invisible in hello queue, so no other clients can process them.</span></span> <span data-ttu-id="cbd76-151">När programmet har bearbetats ett meddelande kan anropa **deleteMessage** toodelete från hello kön.</span><span class="sxs-lookup"><span data-stu-id="cbd76-151">Once your application has processed a message, call **deleteMessage** toodelete it from hello queue.</span></span> <span data-ttu-id="cbd76-152">hello följande exempel hämtar ett meddelande och sedan tar bort den:</span><span class="sxs-lookup"><span data-stu-id="cbd76-152">hello following example gets a message, then deletes it:</span></span>

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
> <span data-ttu-id="cbd76-153">Som standard döljs bara ett meddelande i 30 sekunder, efter vilken den är synlig tooother klienter.</span><span class="sxs-lookup"><span data-stu-id="cbd76-153">By default, a message is only hidden for 30 seconds, after which it is visible tooother clients.</span></span> <span data-ttu-id="cbd76-154">Du kan ange ett annat värde med hjälp av `options.visibilityTimeout` med **GetMessage**.</span><span class="sxs-lookup"><span data-stu-id="cbd76-154">You can specify a different value by using `options.visibilityTimeout` with **getMessages**.</span></span>
> 
> [!NOTE]
> <span data-ttu-id="cbd76-155">Med hjälp av **GetMessage** när det finns inga meddelanden i kö för hello inte returnerar ett fel, men inga meddelanden ska returneras.</span><span class="sxs-lookup"><span data-stu-id="cbd76-155">Using **getMessages** when there are no messages in hello queue will not return an error, however no messages will be returned.</span></span>
> 
> 

## <a name="how-to-change-hello-contents-of-a-queued-message"></a><span data-ttu-id="cbd76-156">Så här: Ändra hello innehållet i meddelandet i kön</span><span class="sxs-lookup"><span data-stu-id="cbd76-156">How To: Change hello Contents of a Queued Message</span></span>
<span data-ttu-id="cbd76-157">Du kan ändra hello innehållet i ett meddelande direkt i hello kön med **updateMessage**.</span><span class="sxs-lookup"><span data-stu-id="cbd76-157">You can change hello contents of a message in-place in hello queue using **updateMessage**.</span></span> <span data-ttu-id="cbd76-158">följande exempel hello uppdateringar hello texten för ett meddelande:</span><span class="sxs-lookup"><span data-stu-id="cbd76-158">hello following example updates hello text of a message:</span></span>

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

## <a name="how-to-additional-options-for-dequeuing-messages"></a><span data-ttu-id="cbd76-159">Så här: Ytterligare alternativ för mellan köer meddelanden</span><span class="sxs-lookup"><span data-stu-id="cbd76-159">How To: Additional Options for Dequeuing Messages</span></span>
<span data-ttu-id="cbd76-160">Det finns två sätt som du kan anpassa meddelandehämtningen från en kö:</span><span class="sxs-lookup"><span data-stu-id="cbd76-160">There are two ways you can customize message retrieval from a queue:</span></span>

* <span data-ttu-id="cbd76-161">`options.numOfMessages`-Hämta en grupp med meddelanden (upp too32.)</span><span class="sxs-lookup"><span data-stu-id="cbd76-161">`options.numOfMessages` - Retrieve a batch of messages (up too32.)</span></span>
* <span data-ttu-id="cbd76-162">`options.visibilityTimeout`-Ange en tidsgräns för osynlighet längre eller kortare.</span><span class="sxs-lookup"><span data-stu-id="cbd76-162">`options.visibilityTimeout` - Set a longer or shorter invisibility timeout.</span></span>

<span data-ttu-id="cbd76-163">hello följande exempel används hello **GetMessage** metoden tooget 15 meddelanden i ett anrop.</span><span class="sxs-lookup"><span data-stu-id="cbd76-163">hello following example uses hello **getMessages** method tooget 15 messages in one call.</span></span> <span data-ttu-id="cbd76-164">Sedan bearbetas varje meddelande med hjälp av en for-loop.</span><span class="sxs-lookup"><span data-stu-id="cbd76-164">Then it processes each message using a for loop.</span></span> <span data-ttu-id="cbd76-165">Den anger också hello osynlighet timeout toofive minuter för alla meddelanden som returneras av den här metoden.</span><span class="sxs-lookup"><span data-stu-id="cbd76-165">It also sets hello invisibility timeout toofive minutes for all messages returned by this method.</span></span>

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

## <a name="how-to-get-hello-queue-length"></a><span data-ttu-id="cbd76-166">Så här: Hämta hello Kölängd</span><span class="sxs-lookup"><span data-stu-id="cbd76-166">How To: Get hello Queue Length</span></span>
<span data-ttu-id="cbd76-167">Hej **getQueueMetadata** returnerar metadata om hello kö, inklusive hello ungefärliga antalet väntande meddelanden i kön hello.</span><span class="sxs-lookup"><span data-stu-id="cbd76-167">hello **getQueueMetadata** returns metadata about hello queue, including hello approximate number of messages waiting in hello queue.</span></span>

```
queueSvc.getQueueMetadata('myqueue', function(error, result, response){
  if(!error){
    // Queue length is available in result.approximateMessageCount
  }
});
```

## <a name="how-to-list-queues"></a><span data-ttu-id="cbd76-168">Så här: Listan köer</span><span class="sxs-lookup"><span data-stu-id="cbd76-168">How To: List Queues</span></span>
<span data-ttu-id="cbd76-169">tooretrieve en lista över köer, Använd **listQueuesSegmented**.</span><span class="sxs-lookup"><span data-stu-id="cbd76-169">tooretrieve a list of queues, use **listQueuesSegmented**.</span></span> <span data-ttu-id="cbd76-170">tooretrieve en lista filtreras efter ett specifikt prefix använder **listQueuesSegmentedWithPrefix**.</span><span class="sxs-lookup"><span data-stu-id="cbd76-170">tooretrieve a list filtered by a specific prefix, use **listQueuesSegmentedWithPrefix**.</span></span>

```
queueSvc.listQueuesSegmented(null, function(error, result, response){
  if(!error){
    // result.entries contains hello list of queues
  }
});
```

<span data-ttu-id="cbd76-171">Om inte det går att returnera alla köer `result.continuationToken` kan användas som hello första parametern för **listQueuesSegmented** eller hello andra parametern för **listQueuesSegmentedWithPrefix** tooretrieve resultat.</span><span class="sxs-lookup"><span data-stu-id="cbd76-171">If all queues cannot be returned, `result.continuationToken` can be used as hello first parameter of **listQueuesSegmented** or hello second parameter of **listQueuesSegmentedWithPrefix** tooretrieve more results.</span></span>

## <a name="how-to-delete-a-queue"></a><span data-ttu-id="cbd76-172">Så här: Ta bort en kö</span><span class="sxs-lookup"><span data-stu-id="cbd76-172">How To: Delete a Queue</span></span>
<span data-ttu-id="cbd76-173">toodelete en kö och alla hälsningsmeddelande finns i den, anropa den **deleteQueue** metod för hello köobjekt.</span><span class="sxs-lookup"><span data-stu-id="cbd76-173">toodelete a queue and all hello messages contained in it, call the **deleteQueue** method on hello queue object.</span></span>

```
queueSvc.deleteQueue(queueName, function(error, response){
  if(!error){
    // Queue has been deleted
  }
});
```

<span data-ttu-id="cbd76-174">tooclear alla meddelanden från en kö utan att ta bort det, Använd **clearMessages**.</span><span class="sxs-lookup"><span data-stu-id="cbd76-174">tooclear all messages from a queue without deleting it, use **clearMessages**.</span></span>

## <a name="how-to-work-with-shared-access-signatures"></a><span data-ttu-id="cbd76-175">Så här: arbeta med signaturer för delad åtkomst</span><span class="sxs-lookup"><span data-stu-id="cbd76-175">How to: Work with Shared Access Signatures</span></span>
<span data-ttu-id="cbd76-176">Delad åtkomst signaturer (SAS) är ett säkert sätt tooprovide detaljerade åtkomst tooqueues utan att erbjuda dina lagringskontonamn eller nycklar.</span><span class="sxs-lookup"><span data-stu-id="cbd76-176">Shared Access Signatures (SAS) are a secure way tooprovide granular access tooqueues without providing your storage account name or keys.</span></span> <span data-ttu-id="cbd76-177">Säkerhetsassociationer är ofta använda tooprovide begränsad åtkomst tooyour köer, till exempel att tillåta en mobil app toosubmit meddelanden.</span><span class="sxs-lookup"><span data-stu-id="cbd76-177">SAS are often used tooprovide limited access tooyour queues, such as allowing a mobile app toosubmit messages.</span></span>

<span data-ttu-id="cbd76-178">En betrodda program, till exempel en molnbaserad tjänst genererar en SAS med hello **generateSharedAccessSignature** av hello **QueueService**, och ger den tooan inte är betrodd eller delvis betrodda program.</span><span class="sxs-lookup"><span data-stu-id="cbd76-178">A trusted application such as a cloud-based service generates a SAS using hello **generateSharedAccessSignature** of hello **QueueService**, and provides it tooan untrusted or semi-trusted application.</span></span> <span data-ttu-id="cbd76-179">Till exempel en mobil app.</span><span class="sxs-lookup"><span data-stu-id="cbd76-179">For example, a mobile app.</span></span> <span data-ttu-id="cbd76-180">hello SAS genereras med en princip som beskriver hello start- och slutdatum under vilka hello SAS är giltigt, samt hello åtkomst nivån beviljade toohello SAS innehavaren.</span><span class="sxs-lookup"><span data-stu-id="cbd76-180">hello SAS is generated using a policy, which describes hello start and end dates during which hello SAS is valid, as well as hello access level granted toohello SAS holder.</span></span>

<span data-ttu-id="cbd76-181">hello följande exempel skapar en ny princip för delad åtkomst som gör att hello SAS innehavaren tooadd toohello kön och upphör att gälla 100 minuter efter hello när det skapas.</span><span class="sxs-lookup"><span data-stu-id="cbd76-181">hello following example generates a new shared access policy that will allow hello SAS holder tooadd messages toohello queue, and expires 100 minutes after hello time it is created.</span></span>

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

<span data-ttu-id="cbd76-182">Observera att information om hello värden måste anges också som krävs när hello SAS innehavaren försöker tooaccess hello kön.</span><span class="sxs-lookup"><span data-stu-id="cbd76-182">Note that hello host information must be provided also, as it is required when hello SAS holder attempts tooaccess hello queue.</span></span>

<span data-ttu-id="cbd76-183">Hej klientprogrammet och sedan använder hello SAS med **QueueServiceWithSAS** tooperform åtgärder mot hello kön.</span><span class="sxs-lookup"><span data-stu-id="cbd76-183">hello client application then uses hello SAS with **QueueServiceWithSAS** tooperform operations against hello queue.</span></span> <span data-ttu-id="cbd76-184">följande exempel hello ansluter toohello kön och skapar ett meddelande.</span><span class="sxs-lookup"><span data-stu-id="cbd76-184">hello following example connects toohello queue and creates a message.</span></span>

```
var sharedQueueService = azure.createQueueServiceWithSas(host, queueSAS);
sharedQueueService.createMessage('myqueue', 'Hello world from SAS!', function(error, result, response){
  if(!error){
    //message added
  }
});
```

<span data-ttu-id="cbd76-185">Eftersom hello SAS genererades med Lägg till åtkomst om ett försök gjordes tooread, uppdatera eller ta bort meddelanden, returneras ett fel.</span><span class="sxs-lookup"><span data-stu-id="cbd76-185">Since hello SAS was generated with add access, if an attempt were made tooread, update or delete messages, an error would be returned.</span></span>

### <a name="access-control-lists"></a><span data-ttu-id="cbd76-186">Listor för åtkomstkontroll</span><span class="sxs-lookup"><span data-stu-id="cbd76-186">Access control lists</span></span>
<span data-ttu-id="cbd76-187">Du kan också använda en åtkomstkontrollista (ACL) tooset hello åtkomstprincip för en SAS.</span><span class="sxs-lookup"><span data-stu-id="cbd76-187">You can also use an Access Control List (ACL) tooset hello access policy for a SAS.</span></span> <span data-ttu-id="cbd76-188">Detta är användbart om du vill tooallow flera klienter tooaccess hello kön, men ange olika principer för varje klient.</span><span class="sxs-lookup"><span data-stu-id="cbd76-188">This is useful if you wish tooallow multiple clients tooaccess hello queue, but provide different access policies for each client.</span></span>

<span data-ttu-id="cbd76-189">En ACL implementeras med hjälp av en matris med principer för åtkomst med ett ID som är associerade med varje princip.</span><span class="sxs-lookup"><span data-stu-id="cbd76-189">An ACL is implemented using an array of access policies, with an ID associated with each policy.</span></span> <span data-ttu-id="cbd76-190">hello följande exempel definierar två principer; en för 'Användare1' och en för 'användare2':</span><span class="sxs-lookup"><span data-stu-id="cbd76-190">hello  following example defines two policies; one for 'user1' and one for 'user2':</span></span>

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

<span data-ttu-id="cbd76-191">följande exempel hämtar hello hello aktuella ACL för **MinKö**, lägger till nya principer för hello med **setQueueAcl**.</span><span class="sxs-lookup"><span data-stu-id="cbd76-191">hello following example gets hello current ACL for **myqueue**, then adds hello new policies using **setQueueAcl**.</span></span> <span data-ttu-id="cbd76-192">Den här metoden kan:</span><span class="sxs-lookup"><span data-stu-id="cbd76-192">This approach allows:</span></span>

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

<span data-ttu-id="cbd76-193">En gång hello ACL har ställts in, du kan sedan skapa en SAS baserat på hello-ID för en princip.</span><span class="sxs-lookup"><span data-stu-id="cbd76-193">Once hello ACL has been set, you can then create a SAS based on hello ID for a policy.</span></span> <span data-ttu-id="cbd76-194">hello följande exempel skapas en ny SAS för 'användare2':</span><span class="sxs-lookup"><span data-stu-id="cbd76-194">hello following example creates a new SAS for 'user2':</span></span>

```
queueSAS = queueSvc.generateSharedAccessSignature('myqueue', { Id: 'user2' });
```

## <a name="next-steps"></a><span data-ttu-id="cbd76-195">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="cbd76-195">Next Steps</span></span>
<span data-ttu-id="cbd76-196">Nu när du har lärt dig hello grunderna i queue storage kan du följa dessa länkar toolearn om mer komplexa lagringsuppgifter.</span><span class="sxs-lookup"><span data-stu-id="cbd76-196">Now that you've learned hello basics of queue storage, follow these links toolearn about more complex storage tasks.</span></span>

* <span data-ttu-id="cbd76-197">Besök hello [Azure Storage-teamets blogg] [Azure Storage-teamets blogg].</span><span class="sxs-lookup"><span data-stu-id="cbd76-197">Visit hello [Azure Storage Team Blog][Azure Storage Team Blog].</span></span>
* <span data-ttu-id="cbd76-198">Besök hello [Azure Storage SDK: N för noden] [ Azure Storage SDK for Node] databasen på GitHub.</span><span class="sxs-lookup"><span data-stu-id="cbd76-198">Visit hello [Azure Storage SDK for Node][Azure Storage SDK for Node] repository on GitHub.</span></span>

[Azure Storage SDK for Node]: https://github.com/Azure/azure-storage-node
[using hello REST API]: http://msdn.microsoft.com/library/azure/hh264518.aspx
[Azure Portal]: https://portal.azure.com<span data-ttu-id="cbd76-199">
[Skapa en Node.js-webbapp i Azure App Service](../../app-service-web/app-service-web-get-started-nodejs.md) </span><span class="sxs-lookup"><span data-stu-id="cbd76-199">
[Create a Node.js web app in Azure App Service](../../app-service-web/app-service-web-get-started-nodejs.md) </span></span>  
<span data-ttu-id="cbd76-200">[Node.js-webbapp med hjälp av hello Azure Table-tjänsten](../../app-service-web/storage-nodejs-use-table-storage-web-site.md)
</span><span class="sxs-lookup"><span data-stu-id="cbd76-200">[Node.js web app using hello Azure Table Service](../../app-service-web/storage-nodejs-use-table-storage-web-site.md)
</span></span>  


<span data-ttu-id="cbd76-201">[Skapa och distribuera en Node.js-programmet tooan Azure Cloud Service](../../cloud-services/cloud-services-nodejs-develop-deploy-app.md) </span><span class="sxs-lookup"><span data-stu-id="cbd76-201">[Build and deploy a Node.js application tooan Azure Cloud Service](../../cloud-services/cloud-services-nodejs-develop-deploy-app.md) </span></span>  
<span data-ttu-id="cbd76-202">[Azure Storage-teamets blogg]: http://blogs.msdn.com/b/windowsazurestorage/ [skapa och distribuera en Node.js web app tooAzure med hjälp av Web Matrix]: https://www.microsoft.com/web/webmatrix/</span><span class="sxs-lookup"><span data-stu-id="cbd76-202">[Azure Storage Team Blog]: http://blogs.msdn.com/b/windowsazurestorage/ [Build and deploy a Node.js web app tooAzure using Web Matrix]: https://www.microsoft.com/web/webmatrix/</span></span>   
