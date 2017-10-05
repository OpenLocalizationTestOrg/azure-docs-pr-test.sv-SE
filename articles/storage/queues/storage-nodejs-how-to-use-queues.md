---
title: "Använda Queue storage från Node.js | Microsoft Docs"
description: "Lär dig hur du använder Azure-Kötjänsten för att skapa och ta bort köer, infoga, hämta och ta bort meddelanden. Exempel som skrivits i Node.js."
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
ms.openlocfilehash: 15c1d3cb6eac8fc14837277c4a4275dea91701cd
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/29/2017
---
# <a name="how-to-use-queue-storage-from-nodejs"></a><span data-ttu-id="1d67f-104">Använda Queue Storage från Node.js</span><span class="sxs-lookup"><span data-stu-id="1d67f-104">How to use Queue storage from Node.js</span></span>
[!INCLUDE [storage-selector-queue-include](../../../includes/storage-selector-queue-include.md)]

[!INCLUDE [storage-check-out-samples-all](../../../includes/storage-check-out-samples-all.md)]

## <a name="overview"></a><span data-ttu-id="1d67f-105">Översikt</span><span class="sxs-lookup"><span data-stu-id="1d67f-105">Overview</span></span>
<span data-ttu-id="1d67f-106">Den här guiden visar hur du utför vanliga scenarier med hjälp av Microsoft Azure-kötjänsten.</span><span class="sxs-lookup"><span data-stu-id="1d67f-106">This guide shows you how to perform common scenarios using the Microsoft Azure Queue service.</span></span> <span data-ttu-id="1d67f-107">Exemplen är skrivna med Node.js-API.</span><span class="sxs-lookup"><span data-stu-id="1d67f-107">The samples are written using the Node.js API.</span></span> <span data-ttu-id="1d67f-108">Scenarier som tas upp inkluderar **infoga**, **granskning**, **komma**, och **bort** kö meddelanden, samt **skapa och ta bort köer**.</span><span class="sxs-lookup"><span data-stu-id="1d67f-108">The scenarios covered include **inserting**, **peeking**, **getting**, and **deleting** queue messages, as well as **creating and deleting queues**.</span></span>

[!INCLUDE [storage-queue-concepts-include](../../../includes/storage-queue-concepts-include.md)]

[!INCLUDE [storage-create-account-include](../../../includes/storage-create-account-include.md)]

## <a name="create-a-nodejs-application"></a><span data-ttu-id="1d67f-109">Skapa en Node.js-program</span><span class="sxs-lookup"><span data-stu-id="1d67f-109">Create a Node.js Application</span></span>
<span data-ttu-id="1d67f-110">Skapa ett tomt Node.js-program.</span><span class="sxs-lookup"><span data-stu-id="1d67f-110">Create a blank Node.js application.</span></span> <span data-ttu-id="1d67f-111">Instruktioner om att skapa en Node.js-program finns i [skapa en Node.js-webbapp i Azure App Service](../../app-service-web/app-service-web-get-started-nodejs.md), [skapa och distribuera ett Node.js-program till en Azure-molntjänst](../../cloud-services/cloud-services-nodejs-develop-deploy-app.md) med Windows PowerShell eller [skapa och distribuera en Node.js-webbapp till Azure med hjälp av Web Matrix](https://www.microsoft.com/web/webmatrix/).</span><span class="sxs-lookup"><span data-stu-id="1d67f-111">For instructions creating a Node.js application, see [Create a Node.js web app in Azure App Service](../../app-service-web/app-service-web-get-started-nodejs.md), [Build and deploy a Node.js application to an Azure Cloud Service](../../cloud-services/cloud-services-nodejs-develop-deploy-app.md) using Windows PowerShell, or [Build and deploy a Node.js web app to Azure using Web Matrix](https://www.microsoft.com/web/webmatrix/).</span></span>

## <a name="configure-your-application-to-access-storage"></a><span data-ttu-id="1d67f-112">Konfigurera ditt program för att komma åt lagringsutrymme</span><span class="sxs-lookup"><span data-stu-id="1d67f-112">Configure Your Application to Access Storage</span></span>
<span data-ttu-id="1d67f-113">Om du vill använda Azure storage, behöver du Azure Storage SDK: N för Node.js som innehåller en uppsättning bekvämlighet bibliotek som kommunicerar med storage REST-tjänster.</span><span class="sxs-lookup"><span data-stu-id="1d67f-113">To use Azure storage, you need the Azure Storage SDK for Node.js, which includes a set of convenience libraries that communicate with the storage REST services.</span></span>

### <a name="use-node-package-manager-npm-to-obtain-the-package"></a><span data-ttu-id="1d67f-114">Använd noden Package Manager (NPM) för att hämta paketet</span><span class="sxs-lookup"><span data-stu-id="1d67f-114">Use Node Package Manager (NPM) to obtain the package</span></span>
1. <span data-ttu-id="1d67f-115">Använd ett kommandoradsgränssnitt som **PowerShell** (Windows), **Terminal** (Mac) eller **Bash** (Unix), navigera till mappen där du skapade exempelprogrammet.</span><span class="sxs-lookup"><span data-stu-id="1d67f-115">Use a command-line interface such as **PowerShell** (Windows,) **Terminal** (Mac,) or **Bash** (Unix), navigate to the folder where you created your sample application.</span></span>
2. <span data-ttu-id="1d67f-116">Typen **npm installera azure-lagring** i kommandofönstret.</span><span class="sxs-lookup"><span data-stu-id="1d67f-116">Type **npm install azure-storage** in the command window.</span></span> <span data-ttu-id="1d67f-117">Utdata från kommandot liknar följande exempel.</span><span class="sxs-lookup"><span data-stu-id="1d67f-117">Output from the command is similar to the following example.</span></span>
 
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

3. <span data-ttu-id="1d67f-118">Du kan köra manuellt på **ls** kommando för att kontrollera att en **nod\_moduler** mappen har skapats.</span><span class="sxs-lookup"><span data-stu-id="1d67f-118">You can manually run the **ls** command to verify that a **node\_modules** folder was created.</span></span> <span data-ttu-id="1d67f-119">I mappen hittar du den **azure storage** paket som innehåller de bibliotek som du behöver åtkomst till lagring.</span><span class="sxs-lookup"><span data-stu-id="1d67f-119">Inside that folder you will find the **azure-storage** package, which contains the libraries you need to access storage.</span></span>

### <a name="import-the-package"></a><span data-ttu-id="1d67f-120">Importera paketet</span><span class="sxs-lookup"><span data-stu-id="1d67f-120">Import the package</span></span>
<span data-ttu-id="1d67f-121">I anteckningar eller något annat textredigeringsprogram, lägger du till följande upp den **server.js** filen för programmet som du tänker använda lagring:</span><span class="sxs-lookup"><span data-stu-id="1d67f-121">Using Notepad or another text editor, add the following to the top the **server.js** file of the application where you intend to use storage:</span></span>

```
var azure = require('azure-storage');
```

## <a name="setup-an-azure-storage-connection"></a><span data-ttu-id="1d67f-122">Skapa ett Azure Storage-anslutning</span><span class="sxs-lookup"><span data-stu-id="1d67f-122">Setup an Azure Storage Connection</span></span>
<span data-ttu-id="1d67f-123">Azure-modulen läses miljövariablerna AZURE\_lagring\_konto och AZURE\_lagring\_åtkomst\_nyckel eller AZURE\_lagring\_anslutning\_sträng för information som krävs för att ansluta till Azure storage-konto.</span><span class="sxs-lookup"><span data-stu-id="1d67f-123">The azure module will read the environment variables AZURE\_STORAGE\_ACCOUNT and AZURE\_STORAGE\_ACCESS\_KEY, or AZURE\_STORAGE\_CONNECTION\_STRING for information required to connect to your Azure storage account.</span></span> <span data-ttu-id="1d67f-124">Om de här miljövariablerna inte har angetts måste du ange kontoinformationen vid anrop av **createQueueService**.</span><span class="sxs-lookup"><span data-stu-id="1d67f-124">If these environment variables are not set, you must specify the account information when calling **createQueueService**.</span></span>

<span data-ttu-id="1d67f-125">Ett exempel på Ange miljövariabler i den [Azure Portal](https://portal.azure.com) en Azure-webbplats finns i [Node.js-webbapp som använder tjänsten Azure tabell].</span><span class="sxs-lookup"><span data-stu-id="1d67f-125">For an example of setting the environment variables in the [Azure Portal](https://portal.azure.com) for an Azure Website, see [Node.js web app using the Azure Table Service].</span></span>

## <a name="how-to-create-a-queue"></a><span data-ttu-id="1d67f-126">Så här: Skapa en kö</span><span class="sxs-lookup"><span data-stu-id="1d67f-126">How To: Create a Queue</span></span>
<span data-ttu-id="1d67f-127">Följande kod skapar en **QueueService** -objektet, vilket gör att du kan arbeta med köer.</span><span class="sxs-lookup"><span data-stu-id="1d67f-127">The following code creates a **QueueService** object, which enables you to work with queues.</span></span>

```
var queueSvc = azure.createQueueService();
```

<span data-ttu-id="1d67f-128">Använd den **createQueueIfNotExists** metod som returnerar den angivna kön om det redan finns eller skapar en ny kö med det angivna namnet om det inte redan finns.</span><span class="sxs-lookup"><span data-stu-id="1d67f-128">Use the **createQueueIfNotExists** method, which returns the specified queue if it already exists or creates a new queue with the specified name if it does not already exist.</span></span>

```
queueSvc.createQueueIfNotExists('myqueue', function(error, result, response){
  if(!error){
    // Queue created or exists
  }
});
```

<span data-ttu-id="1d67f-129">Om kön har skapats, `result.created` är true.</span><span class="sxs-lookup"><span data-stu-id="1d67f-129">If the queue is created, `result.created` is true.</span></span> <span data-ttu-id="1d67f-130">Om kön finns, `result.created` är false.</span><span class="sxs-lookup"><span data-stu-id="1d67f-130">If the queue exists, `result.created` is false.</span></span>

### <a name="filters"></a><span data-ttu-id="1d67f-131">Filter</span><span class="sxs-lookup"><span data-stu-id="1d67f-131">Filters</span></span>
<span data-ttu-id="1d67f-132">Valfria filtrering åtgärder kan användas för åtgärder som utförs med hjälp av **QueueService**.</span><span class="sxs-lookup"><span data-stu-id="1d67f-132">Optional filtering operations can be applied to operations performed using **QueueService**.</span></span> <span data-ttu-id="1d67f-133">Filtrering operations kan innehålla loggning, försöker automatiskt igen och så vidare. Filtren är objekt som implementerar en metod med signaturen:</span><span class="sxs-lookup"><span data-stu-id="1d67f-133">Filtering operations can include logging, automatically retrying, etc. Filters are objects that implement a method with the signature:</span></span>

```
function handle (requestOptions, next)
```

<span data-ttu-id="1d67f-134">Metoden måste anropa ”nästa” Skicka en motringning med följande signatur när du har gjort dess förbearbetning på begäran-alternativ:</span><span class="sxs-lookup"><span data-stu-id="1d67f-134">After doing its preprocessing on the request options, the method needs to call "next" passing a callback with the following signature:</span></span>

```
function (returnObject, finalCallback, next)
```

<span data-ttu-id="1d67f-135">I den här motringning och efter bearbetning returnObject (svar från begäran till servern), måste återanropet anropa sedan om den finns för att fortsätta att bearbeta filter eller anropa bara finalCallback på annat sätt för att få service-anrop.</span><span class="sxs-lookup"><span data-stu-id="1d67f-135">In this callback, and after processing the returnObject (the response from the request to the server), the callback needs to either invoke next if it exists to continue processing other filters or simply invoke finalCallback otherwise to end up the service invocation.</span></span>

<span data-ttu-id="1d67f-136">Två filter som implementerar logik som medföljer Azure SDK för Node.js, **ExponentialRetryPolicyFilter** och **LinearRetryPolicyFilter**.</span><span class="sxs-lookup"><span data-stu-id="1d67f-136">Two filters that implement retry logic are included with the Azure SDK for Node.js, **ExponentialRetryPolicyFilter** and **LinearRetryPolicyFilter**.</span></span> <span data-ttu-id="1d67f-137">Följande kod skapar en **QueueService** objekt som använder den **ExponentialRetryPolicyFilter**:</span><span class="sxs-lookup"><span data-stu-id="1d67f-137">The following creates a **QueueService** object that uses the **ExponentialRetryPolicyFilter**:</span></span>

```
var retryOperations = new azure.ExponentialRetryPolicyFilter();
var queueSvc = azure.createQueueService().withFilter(retryOperations);
```

## <a name="how-to-insert-a-message-into-a-queue"></a><span data-ttu-id="1d67f-138">Så här: Infoga ett meddelande i en kö</span><span class="sxs-lookup"><span data-stu-id="1d67f-138">How To: Insert a Message into a Queue</span></span>
<span data-ttu-id="1d67f-139">Om du vill infoga ett meddelande i en kö, använda den **createMessage** metoden för att skapa ett nytt meddelande och lägga till den i kön.</span><span class="sxs-lookup"><span data-stu-id="1d67f-139">To insert a message into a queue, use the **createMessage** method to create a new message and add it to the queue.</span></span>

```
queueSvc.createMessage('myqueue', "Hello world!", function(error, result, response){
  if(!error){
    // Message inserted
  }
});
```

## <a name="how-to-peek-at-the-next-message"></a><span data-ttu-id="1d67f-140">Så här: Granska nästa meddelande</span><span class="sxs-lookup"><span data-stu-id="1d67f-140">How To: Peek at the Next Message</span></span>
<span data-ttu-id="1d67f-141">Du kan kika på meddelandet först i en kö utan att ta bort det från kön genom att anropa den **peekMessages** metod.</span><span class="sxs-lookup"><span data-stu-id="1d67f-141">You can peek at the message in the front of a queue without removing it from the queue by calling the **peekMessages** method.</span></span> <span data-ttu-id="1d67f-142">Som standard **peekMessages** tittar på ett enda meddelande.</span><span class="sxs-lookup"><span data-stu-id="1d67f-142">By default, **peekMessages** peeks at a single message.</span></span>

```
queueSvc.peekMessages('myqueue', function(error, result, response){
  if(!error){
    // Message text is in messages[0].messageText
  }
});
```

<span data-ttu-id="1d67f-143">Den `result` innehåller meddelandet.</span><span class="sxs-lookup"><span data-stu-id="1d67f-143">The `result` contains the message.</span></span>

> [!NOTE]
> <span data-ttu-id="1d67f-144">Med hjälp av **peekMessages** när det finns inga meddelanden i kön inte returneras ett fel, men inga meddelanden ska returneras.</span><span class="sxs-lookup"><span data-stu-id="1d67f-144">Using **peekMessages** when there are no messages in the queue will not return an error, however no messages will be returned.</span></span>
> 
> 

## <a name="how-to-dequeue-the-next-message"></a><span data-ttu-id="1d67f-145">Så här: Status Created nästa meddelande</span><span class="sxs-lookup"><span data-stu-id="1d67f-145">How To: Dequeue the Next Message</span></span>
<span data-ttu-id="1d67f-146">Bearbetning av ett meddelande är en process i två steg:</span><span class="sxs-lookup"><span data-stu-id="1d67f-146">Processing a message is a two-stage process:</span></span>

1. <span data-ttu-id="1d67f-147">Meddelandet har status Created.</span><span class="sxs-lookup"><span data-stu-id="1d67f-147">Dequeue the message.</span></span>
2. <span data-ttu-id="1d67f-148">Ta bort meddelandet.</span><span class="sxs-lookup"><span data-stu-id="1d67f-148">Delete the message.</span></span>

<span data-ttu-id="1d67f-149">För att ett meddelande har status Created använda **GetMessage**.</span><span class="sxs-lookup"><span data-stu-id="1d67f-149">To dequeue a message, use **getMessages**.</span></span> <span data-ttu-id="1d67f-150">Det gör att meddelanden i kön, så att inga andra klienter kan bearbeta dem.</span><span class="sxs-lookup"><span data-stu-id="1d67f-150">This makes the messages invisible in the queue, so no other clients can process them.</span></span> <span data-ttu-id="1d67f-151">När programmet har bearbetats ett meddelande kan anropa **deleteMessage** ta bort det från kön.</span><span class="sxs-lookup"><span data-stu-id="1d67f-151">Once your application has processed a message, call **deleteMessage** to delete it from the queue.</span></span> <span data-ttu-id="1d67f-152">I följande exempel hämtar ett meddelande och sedan tar bort den:</span><span class="sxs-lookup"><span data-stu-id="1d67f-152">The following example gets a message, then deletes it:</span></span>

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
> <span data-ttu-id="1d67f-153">Som standard döljs bara ett meddelande i 30 sekunder, efter vilken den är synlig för andra klienter.</span><span class="sxs-lookup"><span data-stu-id="1d67f-153">By default, a message is only hidden for 30 seconds, after which it is visible to other clients.</span></span> <span data-ttu-id="1d67f-154">Du kan ange ett annat värde med hjälp av `options.visibilityTimeout` med **GetMessage**.</span><span class="sxs-lookup"><span data-stu-id="1d67f-154">You can specify a different value by using `options.visibilityTimeout` with **getMessages**.</span></span>
> 
> [!NOTE]
> <span data-ttu-id="1d67f-155">Med hjälp av **GetMessage** när det finns inga meddelanden i kön inte returneras ett fel, men inga meddelanden ska returneras.</span><span class="sxs-lookup"><span data-stu-id="1d67f-155">Using **getMessages** when there are no messages in the queue will not return an error, however no messages will be returned.</span></span>
> 
> 

## <a name="how-to-change-the-contents-of-a-queued-message"></a><span data-ttu-id="1d67f-156">Så här: Ändra innehållet i ett meddelande i kön</span><span class="sxs-lookup"><span data-stu-id="1d67f-156">How To: Change the Contents of a Queued Message</span></span>
<span data-ttu-id="1d67f-157">Du kan ändra innehållet i ett meddelande direkt i kön med **updateMessage**.</span><span class="sxs-lookup"><span data-stu-id="1d67f-157">You can change the contents of a message in-place in the queue using **updateMessage**.</span></span> <span data-ttu-id="1d67f-158">I följande exempel uppdateras texten för ett meddelande:</span><span class="sxs-lookup"><span data-stu-id="1d67f-158">The following example updates the text of a message:</span></span>

```
queueSvc.getMessages('myqueue', function(error, result, response){
  if(!error){
    // Got the message
    var message = result[0];
    queueSvc.updateMessage('myqueue', message.messageId, message.popReceipt, 10, {messageText: 'new text'}, function(error, result, response){
      if(!error){
        // Message updated successfully
      }
    });
  }
});
```

## <a name="how-to-additional-options-for-dequeuing-messages"></a><span data-ttu-id="1d67f-159">Så här: Ytterligare alternativ för mellan köer meddelanden</span><span class="sxs-lookup"><span data-stu-id="1d67f-159">How To: Additional Options for Dequeuing Messages</span></span>
<span data-ttu-id="1d67f-160">Det finns två sätt som du kan anpassa meddelandehämtningen från en kö:</span><span class="sxs-lookup"><span data-stu-id="1d67f-160">There are two ways you can customize message retrieval from a queue:</span></span>

* <span data-ttu-id="1d67f-161">`options.numOfMessages`-Hämta en grupp med meddelanden (upp till 32).</span><span class="sxs-lookup"><span data-stu-id="1d67f-161">`options.numOfMessages` - Retrieve a batch of messages (up to 32.)</span></span>
* <span data-ttu-id="1d67f-162">`options.visibilityTimeout`-Ange en tidsgräns för osynlighet längre eller kortare.</span><span class="sxs-lookup"><span data-stu-id="1d67f-162">`options.visibilityTimeout` - Set a longer or shorter invisibility timeout.</span></span>

<span data-ttu-id="1d67f-163">I följande exempel används den **GetMessage** metod för att hämta 15 meddelanden i ett anrop.</span><span class="sxs-lookup"><span data-stu-id="1d67f-163">The following example uses the **getMessages** method to get 15 messages in one call.</span></span> <span data-ttu-id="1d67f-164">Sedan bearbetas varje meddelande med hjälp av en for-loop.</span><span class="sxs-lookup"><span data-stu-id="1d67f-164">Then it processes each message using a for loop.</span></span> <span data-ttu-id="1d67f-165">Den anger också tidsgränsen för osynlighet till fem minuter för alla meddelanden som returneras av den här metoden.</span><span class="sxs-lookup"><span data-stu-id="1d67f-165">It also sets the invisibility timeout to five minutes for all messages returned by this method.</span></span>

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

## <a name="how-to-get-the-queue-length"></a><span data-ttu-id="1d67f-166">Så här: Hämta kölängden</span><span class="sxs-lookup"><span data-stu-id="1d67f-166">How To: Get the Queue Length</span></span>
<span data-ttu-id="1d67f-167">Den **getQueueMetadata** returnerar metadata om kön, inklusive det ungefärliga antalet väntande meddelanden i kön.</span><span class="sxs-lookup"><span data-stu-id="1d67f-167">The **getQueueMetadata** returns metadata about the queue, including the approximate number of messages waiting in the queue.</span></span>

```
queueSvc.getQueueMetadata('myqueue', function(error, result, response){
  if(!error){
    // Queue length is available in result.approximateMessageCount
  }
});
```

## <a name="how-to-list-queues"></a><span data-ttu-id="1d67f-168">Så här: Listan köer</span><span class="sxs-lookup"><span data-stu-id="1d67f-168">How To: List Queues</span></span>
<span data-ttu-id="1d67f-169">Använd för att hämta en lista över köer **listQueuesSegmented**.</span><span class="sxs-lookup"><span data-stu-id="1d67f-169">To retrieve a list of queues, use **listQueuesSegmented**.</span></span> <span data-ttu-id="1d67f-170">Använd för att hämta en lista filtreras efter ett specifikt prefix **listQueuesSegmentedWithPrefix**.</span><span class="sxs-lookup"><span data-stu-id="1d67f-170">To retrieve a list filtered by a specific prefix, use **listQueuesSegmentedWithPrefix**.</span></span>

```
queueSvc.listQueuesSegmented(null, function(error, result, response){
  if(!error){
    // result.entries contains the list of queues
  }
});
```

<span data-ttu-id="1d67f-171">Om inte det går att returnera alla köer `result.continuationToken` kan användas som den första parametern för **listQueuesSegmented** eller den andra parametern för **listQueuesSegmentedWithPrefix** att hämta fler resultat.</span><span class="sxs-lookup"><span data-stu-id="1d67f-171">If all queues cannot be returned, `result.continuationToken` can be used as the first parameter of **listQueuesSegmented** or the second parameter of **listQueuesSegmentedWithPrefix** to retrieve more results.</span></span>

## <a name="how-to-delete-a-queue"></a><span data-ttu-id="1d67f-172">Så här: Ta bort en kö</span><span class="sxs-lookup"><span data-stu-id="1d67f-172">How To: Delete a Queue</span></span>
<span data-ttu-id="1d67f-173">Om du vill ta bort en kö och alla meddelanden som finns i den anropar den **deleteQueue** metoden för köobjektet.</span><span class="sxs-lookup"><span data-stu-id="1d67f-173">To delete a queue and all the messages contained in it, call the **deleteQueue** method on the queue object.</span></span>

```
queueSvc.deleteQueue(queueName, function(error, response){
  if(!error){
    // Queue has been deleted
  }
});
```

<span data-ttu-id="1d67f-174">Om du vill rensa alla meddelanden från en kö utan att ta bort den använda **clearMessages**.</span><span class="sxs-lookup"><span data-stu-id="1d67f-174">To clear all messages from a queue without deleting it, use **clearMessages**.</span></span>

## <a name="how-to-work-with-shared-access-signatures"></a><span data-ttu-id="1d67f-175">Så här: arbeta med signaturer för delad åtkomst</span><span class="sxs-lookup"><span data-stu-id="1d67f-175">How to: Work with Shared Access Signatures</span></span>
<span data-ttu-id="1d67f-176">Delad åtkomst signaturer (SAS) är ett säkert sätt att tillhandahålla detaljerade åtkomst till köer utan att erbjuda dina lagringskontonamn eller nycklar.</span><span class="sxs-lookup"><span data-stu-id="1d67f-176">Shared Access Signatures (SAS) are a secure way to provide granular access to queues without providing your storage account name or keys.</span></span> <span data-ttu-id="1d67f-177">SAS används ofta för att ge begränsad åtkomst till din köer, till exempel att tillåta en mobil app att skicka meddelanden.</span><span class="sxs-lookup"><span data-stu-id="1d67f-177">SAS are often used to provide limited access to your queues, such as allowing a mobile app to submit messages.</span></span>

<span data-ttu-id="1d67f-178">En betrodda program, till exempel en molnbaserad tjänst genererar en SAS med hjälp av den **generateSharedAccessSignature** av den **QueueService**, och ger den till ett program som inte är betrodd eller delvis betrodda.</span><span class="sxs-lookup"><span data-stu-id="1d67f-178">A trusted application such as a cloud-based service generates a SAS using the **generateSharedAccessSignature** of the **QueueService**, and provides it to an untrusted or semi-trusted application.</span></span> <span data-ttu-id="1d67f-179">Till exempel en mobil app.</span><span class="sxs-lookup"><span data-stu-id="1d67f-179">For example, a mobile app.</span></span> <span data-ttu-id="1d67f-180">SAS genereras med hjälp av en princip som beskriver de då SAS är giltig start- och slutdatum samt den åtkomstnivå som beviljats till SAS-innehavare.</span><span class="sxs-lookup"><span data-stu-id="1d67f-180">The SAS is generated using a policy, which describes the start and end dates during which the SAS is valid, as well as the access level granted to the SAS holder.</span></span>

<span data-ttu-id="1d67f-181">I följande exempel skapar en ny princip för delad åtkomst som låter innehavaren SAS att lägga till meddelanden i kön och upphör att gälla 100 minuter efter den tidpunkt som den har skapats.</span><span class="sxs-lookup"><span data-stu-id="1d67f-181">The following example generates a new shared access policy that will allow the SAS holder to add messages to the queue, and expires 100 minutes after the time it is created.</span></span>

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

<span data-ttu-id="1d67f-182">Observera att information om värden måste anges, eftersom det är nödvändigt när SAS-innehavaren försöker få åtkomst till kön.</span><span class="sxs-lookup"><span data-stu-id="1d67f-182">Note that the host information must be provided also, as it is required when the SAS holder attempts to access the queue.</span></span>

<span data-ttu-id="1d67f-183">Klientprogrammet sedan använder SAS med **QueueServiceWithSAS** att utföra åtgärder mot kön.</span><span class="sxs-lookup"><span data-stu-id="1d67f-183">The client application then uses the SAS with **QueueServiceWithSAS** to perform operations against the queue.</span></span> <span data-ttu-id="1d67f-184">I följande exempel ansluter till kön och skapar ett meddelande.</span><span class="sxs-lookup"><span data-stu-id="1d67f-184">The following example connects to the queue and creates a message.</span></span>

```
var sharedQueueService = azure.createQueueServiceWithSas(host, queueSAS);
sharedQueueService.createMessage('myqueue', 'Hello world from SAS!', function(error, result, response){
  if(!error){
    //message added
  }
});
```

<span data-ttu-id="1d67f-185">Eftersom SAS genererades med Lägg till åtkomst om ett försök gjordes att läsa, uppdatera eller ta bort meddelanden, returneras ett fel.</span><span class="sxs-lookup"><span data-stu-id="1d67f-185">Since the SAS was generated with add access, if an attempt were made to read, update or delete messages, an error would be returned.</span></span>

### <a name="access-control-lists"></a><span data-ttu-id="1d67f-186">Listor för åtkomstkontroll</span><span class="sxs-lookup"><span data-stu-id="1d67f-186">Access control lists</span></span>
<span data-ttu-id="1d67f-187">Du kan också använda en åtkomstkontrollista (ACL) för att ange åtkomstprincipen för en SAS.</span><span class="sxs-lookup"><span data-stu-id="1d67f-187">You can also use an Access Control List (ACL) to set the access policy for a SAS.</span></span> <span data-ttu-id="1d67f-188">Detta är användbart om du vill att flera klienter kan komma åt kön, men ger olika åtkomstprinciper för varje klient.</span><span class="sxs-lookup"><span data-stu-id="1d67f-188">This is useful if you wish to allow multiple clients to access the queue, but provide different access policies for each client.</span></span>

<span data-ttu-id="1d67f-189">En ACL implementeras med hjälp av en matris med principer för åtkomst med ett ID som är associerade med varje princip.</span><span class="sxs-lookup"><span data-stu-id="1d67f-189">An ACL is implemented using an array of access policies, with an ID associated with each policy.</span></span> <span data-ttu-id="1d67f-190">I följande exempel definierar två principer; en för 'Användare1' och en för 'användare2':</span><span class="sxs-lookup"><span data-stu-id="1d67f-190">The  following example defines two policies; one for 'user1' and one for 'user2':</span></span>

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

<span data-ttu-id="1d67f-191">I följande exempel hämtar den aktuella ACL för **MinKö**, lägger till nya principer med hjälp av **setQueueAcl**.</span><span class="sxs-lookup"><span data-stu-id="1d67f-191">The following example gets the current ACL for **myqueue**, then adds the new policies using **setQueueAcl**.</span></span> <span data-ttu-id="1d67f-192">Den här metoden kan:</span><span class="sxs-lookup"><span data-stu-id="1d67f-192">This approach allows:</span></span>

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

<span data-ttu-id="1d67f-193">Du kan sedan skapa en SAS baserat på ID för en princip när du har angett ACL.</span><span class="sxs-lookup"><span data-stu-id="1d67f-193">Once the ACL has been set, you can then create a SAS based on the ID for a policy.</span></span> <span data-ttu-id="1d67f-194">I följande exempel skapas en ny SAS för 'användare2':</span><span class="sxs-lookup"><span data-stu-id="1d67f-194">The following example creates a new SAS for 'user2':</span></span>

```
queueSAS = queueSvc.generateSharedAccessSignature('myqueue', { Id: 'user2' });
```

## <a name="next-steps"></a><span data-ttu-id="1d67f-195">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="1d67f-195">Next Steps</span></span>
<span data-ttu-id="1d67f-196">Nu när du har lärt dig grunderna i queue storage följa dessa länkar för att lära dig mer komplexa lagringsuppgifter.</span><span class="sxs-lookup"><span data-stu-id="1d67f-196">Now that you've learned the basics of queue storage, follow these links to learn about more complex storage tasks.</span></span>

* <span data-ttu-id="1d67f-197">Besök [Azure Storage-teamets blogg] [Azure Storage-teamets blogg].</span><span class="sxs-lookup"><span data-stu-id="1d67f-197">Visit the [Azure Storage Team Blog][Azure Storage Team Blog].</span></span>
* <span data-ttu-id="1d67f-198">Besök den [Azure Storage SDK: N för noden] [ Azure Storage SDK for Node] databasen på GitHub.</span><span class="sxs-lookup"><span data-stu-id="1d67f-198">Visit the [Azure Storage SDK for Node][Azure Storage SDK for Node] repository on GitHub.</span></span>

[Azure Storage SDK for Node]: https://github.com/Azure/azure-storage-node
[using the REST API]: http://msdn.microsoft.com/library/azure/hh264518.aspx
[Azure Portal]: https://portal.azure.com<span data-ttu-id="1d67f-199">
[Skapa en Node.js-webbapp i Azure App Service](../../app-service-web/app-service-web-get-started-nodejs.md) </span><span class="sxs-lookup"><span data-stu-id="1d67f-199">
[Create a Node.js web app in Azure App Service](../../app-service-web/app-service-web-get-started-nodejs.md) </span></span>  
<span data-ttu-id="1d67f-200">[Node.js-webbapp som använder tjänsten Azure-tabell](../../app-service-web/storage-nodejs-use-table-storage-web-site.md)
</span><span class="sxs-lookup"><span data-stu-id="1d67f-200">[Node.js web app using the Azure Table Service](../../app-service-web/storage-nodejs-use-table-storage-web-site.md)
</span></span>  


<span data-ttu-id="1d67f-201">[Skapa och distribuera ett Node.js-program till en Azure-molntjänst](../../cloud-services/cloud-services-nodejs-develop-deploy-app.md) </span><span class="sxs-lookup"><span data-stu-id="1d67f-201">[Build and deploy a Node.js application to an Azure Cloud Service](../../cloud-services/cloud-services-nodejs-develop-deploy-app.md) </span></span>  
<span data-ttu-id="1d67f-202">[Azure Storage-teamets blogg]: http://blogs.msdn.com/b/windowsazurestorage/ [skapa och distribuera en Node.js-webbapp till Azure med hjälp av Web Matrix]: https://www.microsoft.com/web/webmatrix/</span><span class="sxs-lookup"><span data-stu-id="1d67f-202">[Azure Storage Team Blog]: http://blogs.msdn.com/b/windowsazurestorage/ [Build and deploy a Node.js web app to Azure using Web Matrix]: https://www.microsoft.com/web/webmatrix/</span></span>   
