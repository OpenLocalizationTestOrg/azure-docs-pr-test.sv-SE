---
title: "Hur du använder Service Bus-köer i Node.js | Microsoft Docs"
description: "Lär dig hur du använder Service Bus-köer i Azure från en Node.js-app."
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
ms.openlocfilehash: fe2c02534996d99c190593a419a4823888f03d31
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/29/2017
---
# <a name="how-to-use-service-bus-queues-with-nodejs"></a><span data-ttu-id="99786-103">Hur du använder Service Bus-köer med Node.js</span><span class="sxs-lookup"><span data-stu-id="99786-103">How to use Service Bus queues with Node.js</span></span>

[!INCLUDE [service-bus-selector-queues](../../includes/service-bus-selector-queues.md)]

<span data-ttu-id="99786-104">Den här artikeln beskriver hur du använder Service Bus-köer med Node.js.</span><span class="sxs-lookup"><span data-stu-id="99786-104">This article describes how to use Service Bus queues with Node.js.</span></span> <span data-ttu-id="99786-105">Exemplen är skrivna i JavaScript och använder Node.js Azure-modulen.</span><span class="sxs-lookup"><span data-stu-id="99786-105">The samples are written in JavaScript and use the Node.js Azure module.</span></span> <span data-ttu-id="99786-106">Scenarier som tas upp inkluderar **skapa köer**, **skicka och ta emot meddelanden**, och **tar bort köer**.</span><span class="sxs-lookup"><span data-stu-id="99786-106">The scenarios covered include **creating queues**, **sending and receiving messages**, and **deleting queues**.</span></span> <span data-ttu-id="99786-107">Mer information om köer finns i [nästa steg](#next-steps) avsnitt.</span><span class="sxs-lookup"><span data-stu-id="99786-107">For more information on queues, see the [Next steps](#next-steps) section.</span></span>

[!INCLUDE [howto-service-bus-queues](../../includes/howto-service-bus-queues.md)]

[!INCLUDE [service-bus-create-namespace-portal](../../includes/service-bus-create-namespace-portal.md)]

## <a name="create-a-nodejs-application"></a><span data-ttu-id="99786-108">Skapa ett Node.js-program</span><span class="sxs-lookup"><span data-stu-id="99786-108">Create a Node.js application</span></span>
<span data-ttu-id="99786-109">Skapa ett tomt Node.js-program.</span><span class="sxs-lookup"><span data-stu-id="99786-109">Create a blank Node.js application.</span></span> <span data-ttu-id="99786-110">Instruktioner om hur du skapar en Node.js-program finns i [skapa och distribuera en Node.js-program till en Azure-webbplats][Create and deploy a Node.js application to an Azure Website], eller [Node.js Molntjänsten] [ Node.js Cloud Service] med Windows PowerShell.</span><span class="sxs-lookup"><span data-stu-id="99786-110">For instructions on how to create a Node.js application, see [Create and deploy a Node.js application to an Azure Website][Create and deploy a Node.js application to an Azure Website], or [Node.js Cloud Service][Node.js Cloud Service] using Windows PowerShell.</span></span>

## <a name="configure-your-application-to-use-service-bus"></a><span data-ttu-id="99786-111">Konfigurera ditt program att använda Service Bus</span><span class="sxs-lookup"><span data-stu-id="99786-111">Configure your application to use Service Bus</span></span>
<span data-ttu-id="99786-112">Om du vill använda Azure Service Bus, hämta och använda Node.js Azure-paketet.</span><span class="sxs-lookup"><span data-stu-id="99786-112">To use Azure Service Bus, download and use the Node.js Azure package.</span></span> <span data-ttu-id="99786-113">Det här paketet innehåller en uppsättning bibliotek som kommunicerar med Service Bus REST-tjänster.</span><span class="sxs-lookup"><span data-stu-id="99786-113">This package includes a set of libraries that communicate with the Service Bus REST services.</span></span>

### <a name="use-node-package-manager-npm-to-obtain-the-package"></a><span data-ttu-id="99786-114">Använd noden Package Manager (NPM) för att hämta paketet</span><span class="sxs-lookup"><span data-stu-id="99786-114">Use Node Package Manager (NPM) to obtain the package</span></span>
1. <span data-ttu-id="99786-115">Använd den **Windows PowerShell för Node.js** kommandofönstret att navigera till den **c:\\nod\\sbqueues\\WebRole1** mappen där du skapade ditt exempel programmet.</span><span class="sxs-lookup"><span data-stu-id="99786-115">Use the **Windows PowerShell for Node.js** command window to navigate to the **c:\\node\\sbqueues\\WebRole1** folder in which you created your sample application.</span></span>
2. <span data-ttu-id="99786-116">Typen **npm installera azure** i Kommandotolken, vilket bör resultera i utdata som liknar följande:</span><span class="sxs-lookup"><span data-stu-id="99786-116">Type **npm install azure** in the command window, which should result in output similar to the following:</span></span>

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
3. <span data-ttu-id="99786-117">Du kan köra manuellt på **ls** kommando för att kontrollera att en **node_modules** mappen har skapats.</span><span class="sxs-lookup"><span data-stu-id="99786-117">You can manually run the **ls** command to verify that a **node_modules** folder was created.</span></span> <span data-ttu-id="99786-118">I mappen hitta den **azure** paket som innehåller de bibliotek som du behöver ha tillgång till Service Bus-köer.</span><span class="sxs-lookup"><span data-stu-id="99786-118">Inside that folder find the **azure** package, which contains the libraries you need to access Service Bus queues.</span></span>

### <a name="import-the-module"></a><span data-ttu-id="99786-119">Importera modulen</span><span class="sxs-lookup"><span data-stu-id="99786-119">Import the module</span></span>
<span data-ttu-id="99786-120">I anteckningar eller något annat textredigeringsprogram, Lägg till följande överst i den **server.js** filen för programmet:</span><span class="sxs-lookup"><span data-stu-id="99786-120">Using Notepad or another text editor, add the following to the top of the **server.js** file of the application:</span></span>

```javascript
var azure = require('azure');
```

### <a name="set-up-an-azure-service-bus-connection"></a><span data-ttu-id="99786-121">Azure Service Bus-anslutningar</span><span class="sxs-lookup"><span data-stu-id="99786-121">Set up an Azure Service Bus connection</span></span>
<span data-ttu-id="99786-122">Azure-modulen läser miljövariabeln `AZURE_SERVICEBUS_CONNECTION_STRING` att få information som krävs för att ansluta till Service Bus.</span><span class="sxs-lookup"><span data-stu-id="99786-122">The Azure module reads the environment variable `AZURE_SERVICEBUS_CONNECTION_STRING` to obtain information required to connect to Service Bus.</span></span> <span data-ttu-id="99786-123">Om den här miljövariabeln inte har angetts måste du ange kontoinformationen vid anrop av `createServiceBusService`.</span><span class="sxs-lookup"><span data-stu-id="99786-123">If this environment variable is not set, you must specify the account information when calling `createServiceBusService`.</span></span>

<span data-ttu-id="99786-124">Ett exempel på hur miljövariablerna i en konfigurationsfil för ett Azure Cloud Service, se [Node.js molntjänst med lagring][Node.js Cloud Service with Storage].</span><span class="sxs-lookup"><span data-stu-id="99786-124">For an example of setting the environment variables in a configuration file for an Azure Cloud Service, see [Node.js Cloud Service with Storage][Node.js Cloud Service with Storage].</span></span>

<span data-ttu-id="99786-125">Ett exempel på Ange miljövariabler i den [Azure-portalen] [ Azure portal] en Azure-webbplats finns [Node.js-Webbapp med lagring] [ Node.js Web Application with Storage].</span><span class="sxs-lookup"><span data-stu-id="99786-125">For an example of setting the environment variables in the [Azure portal][Azure portal] for an Azure Website, see [Node.js Web Application with Storage][Node.js Web Application with Storage].</span></span>

## <a name="create-a-queue"></a><span data-ttu-id="99786-126">Skapa en kö</span><span class="sxs-lookup"><span data-stu-id="99786-126">Create a queue</span></span>
<span data-ttu-id="99786-127">Den **ServiceBusService** objekt kan du arbeta med Service Bus-köer.</span><span class="sxs-lookup"><span data-stu-id="99786-127">The **ServiceBusService** object enables you to work with Service Bus queues.</span></span> <span data-ttu-id="99786-128">Följande kod skapar en **ServiceBusService** objekt.</span><span class="sxs-lookup"><span data-stu-id="99786-128">The following code creates a **ServiceBusService** object.</span></span> <span data-ttu-id="99786-129">Lägg till den övre delen av den **server.js** filen när instruktionen för att importera modulen för Azure:</span><span class="sxs-lookup"><span data-stu-id="99786-129">Add it near the top of the **server.js** file, after the statement to import the Azure module:</span></span>

```javascript
var serviceBusService = azure.createServiceBusService();
```

<span data-ttu-id="99786-130">Genom att anropa `createQueueIfNotExists` på den **ServiceBusService** objekt returneras kö som anges (om den finns) eller en ny kö med det angivna namnet har skapats.</span><span class="sxs-lookup"><span data-stu-id="99786-130">By calling `createQueueIfNotExists` on the **ServiceBusService** object, the specified queue is returned (if it exists), or a new queue with the specified name is created.</span></span> <span data-ttu-id="99786-131">I följande kod används `createQueueIfNotExists` ska skapa eller Anslut till kön med namnet `myqueue`:</span><span class="sxs-lookup"><span data-stu-id="99786-131">The following code uses `createQueueIfNotExists` to create or connect to the queue named `myqueue`:</span></span>

```javascript
serviceBusService.createQueueIfNotExists('myqueue', function(error){
    if(!error){
        // Queue exists
    }
});
```

<span data-ttu-id="99786-132">Den `createServiceBusService` metoden stöder också ytterligare alternativ som gör att du kan åsidosätta standardinställningar för kön, till exempel tid till live eller maximal köstorlek.</span><span class="sxs-lookup"><span data-stu-id="99786-132">The `createServiceBusService` method also supports additional options, which enable you to override default queue settings such as message time to live or maximum queue size.</span></span> <span data-ttu-id="99786-133">I följande exempel anger den maximala storleken till 5 GB och en gång till live TTL-värde på 1 minut:</span><span class="sxs-lookup"><span data-stu-id="99786-133">The following example sets the maximum queue size to 5 GB, and a time to live (TTL) value of 1 minute:</span></span>

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

### <a name="filters"></a><span data-ttu-id="99786-134">Filter</span><span class="sxs-lookup"><span data-stu-id="99786-134">Filters</span></span>
<span data-ttu-id="99786-135">Valfria filtrering åtgärder kan användas för åtgärder som utförs med hjälp av **ServiceBusService**.</span><span class="sxs-lookup"><span data-stu-id="99786-135">Optional filtering operations can be applied to operations performed using **ServiceBusService**.</span></span> <span data-ttu-id="99786-136">Filtrering operations kan innehålla loggning, försöker automatiskt igen och så vidare. Filtren är objekt som implementerar en metod med signaturen:</span><span class="sxs-lookup"><span data-stu-id="99786-136">Filtering operations can include logging, automatically retrying, etc. Filters are objects that implement a method with the signature:</span></span>

```javascript
function handle (requestOptions, next)
```

<span data-ttu-id="99786-137">När du har gjort dess föregående bearbetning på begäran-alternativ måste anropa metoden `next`, skicka ett återanrop med följande signatur:</span><span class="sxs-lookup"><span data-stu-id="99786-137">After doing its pre-processing on the request options, the method must call `next`, passing a callback with the following signature:</span></span>

```javascript
function (returnObject, finalCallback, next)
```

<span data-ttu-id="99786-138">I den här motringning och efter bearbetning av `returnObject` (svaret från begäran till servern), återanropet måste antingen anropa `next` om den finns för att fortsätta att bearbeta filter eller helt enkelt anropa `finalCallback`, som slutar tjänsten anrop.</span><span class="sxs-lookup"><span data-stu-id="99786-138">In this callback, and after processing the `returnObject` (the response from the request to the server), the callback must either invoke `next` if it exists to continue processing other filters, or simply invoke `finalCallback`, which ends the service invocation.</span></span>

<span data-ttu-id="99786-139">Två filter som implementerar logik som medföljer Azure SDK för Node.js, `ExponentialRetryPolicyFilter` och `LinearRetryPolicyFilter`.</span><span class="sxs-lookup"><span data-stu-id="99786-139">Two filters that implement retry logic are included with the Azure SDK for Node.js, `ExponentialRetryPolicyFilter` and `LinearRetryPolicyFilter`.</span></span> <span data-ttu-id="99786-140">Följande kod skapar en `ServiceBusService` objekt som använder den `ExponentialRetryPolicyFilter`:</span><span class="sxs-lookup"><span data-stu-id="99786-140">The following code creates a `ServiceBusService` object that uses the `ExponentialRetryPolicyFilter`:</span></span>

```javascript
var retryOperations = new azure.ExponentialRetryPolicyFilter();
var serviceBusService = azure.createServiceBusService().withFilter(retryOperations);
```

## <a name="send-messages-to-a-queue"></a><span data-ttu-id="99786-141">Skicka meddelanden till en kö</span><span class="sxs-lookup"><span data-stu-id="99786-141">Send messages to a queue</span></span>
<span data-ttu-id="99786-142">Att skicka ett meddelande till en Service Bus-kö, program-anrop i `sendQueueMessage` -metoden i den **ServiceBusService** objekt.</span><span class="sxs-lookup"><span data-stu-id="99786-142">To send a message to a Service Bus queue, your application calls the `sendQueueMessage` method on the **ServiceBusService** object.</span></span> <span data-ttu-id="99786-143">Meddelanden skickas till (och mottagna från) Service Bus är köer **BrokeredMessage** objekt och har en uppsättning standardegenskaper (t.ex **etikett** och **TimeToLive**), ordlista som används för att lagra anpassade programspecifika egenskaper och en brödtext med godtyckliga programdata.</span><span class="sxs-lookup"><span data-stu-id="99786-143">Messages sent to (and received from) Service Bus queues are **BrokeredMessage** objects, and have a set of standard properties (such as **Label** and **TimeToLive**), a dictionary that is used to hold custom application-specific properties, and a body of arbitrary application data.</span></span> <span data-ttu-id="99786-144">Ett program kan konfigurera meddelandets brödtext för meddelandet genom att skicka en sträng som meddelandet.</span><span class="sxs-lookup"><span data-stu-id="99786-144">An application can set the body of the message by passing a string as the message.</span></span> <span data-ttu-id="99786-145">Alla nödvändiga standardegenskaper fylls i med standardvärden.</span><span class="sxs-lookup"><span data-stu-id="99786-145">Any required standard properties are populated with default values.</span></span>

<span data-ttu-id="99786-146">I följande exempel visar hur du skickar ett testmeddelande till kön med namnet `myqueue` med `sendQueueMessage`:</span><span class="sxs-lookup"><span data-stu-id="99786-146">The following example demonstrates how to send a test message to the queue named `myqueue` using `sendQueueMessage`:</span></span>

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

<span data-ttu-id="99786-147">Service Bus-köerna stöder en maximal meddelandestorlek på 256 kB på [standardnivån](service-bus-premium-messaging.md) och 1 MB på [premiumnivån](service-bus-premium-messaging.md).</span><span class="sxs-lookup"><span data-stu-id="99786-147">Service Bus queues support a maximum message size of 256 KB in the [Standard tier](service-bus-premium-messaging.md) and 1 MB in the [Premium tier](service-bus-premium-messaging.md).</span></span> <span data-ttu-id="99786-148">Rubriken, som inkluderar standardprogramegenskaperna och de anpassade programegenskaperna, kan ha en maximal storlek på 64 kB.</span><span class="sxs-lookup"><span data-stu-id="99786-148">The header, which includes the standard and custom application properties, can have a maximum size of 64 KB.</span></span> <span data-ttu-id="99786-149">Det finns ingen gräns för antalet meddelanden som kan finnas i en kö men det finns ett tak för den totala storleken för de meddelanden som ligger i en kö.</span><span class="sxs-lookup"><span data-stu-id="99786-149">There is no limit on the number of messages held in a queue but there is a cap on the total size of the messages held by a queue.</span></span> <span data-ttu-id="99786-150">Den här köstorleken definieras när kön skapas, med en övre gräns på 5 GB.</span><span class="sxs-lookup"><span data-stu-id="99786-150">This queue size is defined at creation time, with an upper limit of 5 GB.</span></span> <span data-ttu-id="99786-151">Mer information om kvoter finns [Service Bus-kvoter][Service Bus quotas].</span><span class="sxs-lookup"><span data-stu-id="99786-151">For more information about quotas, see [Service Bus quotas][Service Bus quotas].</span></span>

## <a name="receive-messages-from-a-queue"></a><span data-ttu-id="99786-152">Ta emot meddelanden från en kö</span><span class="sxs-lookup"><span data-stu-id="99786-152">Receive messages from a queue</span></span>
<span data-ttu-id="99786-153">Meddelanden tas emot från en kö med hjälp av den `receiveQueueMessage` -metoden i den **ServiceBusService** objekt.</span><span class="sxs-lookup"><span data-stu-id="99786-153">Messages are received from a queue using the `receiveQueueMessage` method on the **ServiceBusService** object.</span></span> <span data-ttu-id="99786-154">Som standard tas meddelanden bort från kön när de läses; men du kan läsa (titt) och låsa meddelandet utan att ta bort det från kön genom att ange den valfria parametern `isPeekLock` till **SANT**.</span><span class="sxs-lookup"><span data-stu-id="99786-154">By default, messages are deleted from the queue as they are read; however, you can read (peek) and lock the message without deleting it from the queue by setting the optional parameter `isPeekLock` to **true**.</span></span>

<span data-ttu-id="99786-155">Standardbeteendet för läsning och ta bort meddelandet som en del av receive-åtgärden är den enklaste modellen och fungerar bäst för scenarier där ett program kan tolerera icke-bearbetning av ett meddelande om ett fel inträffar.</span><span class="sxs-lookup"><span data-stu-id="99786-155">The default behavior of reading and deleting the message as part of the receive operation is the simplest model, and works best for scenarios in which an application can tolerate not processing a message in the event of a failure.</span></span> <span data-ttu-id="99786-156">För att förstå detta kan du föreställa dig ett scenario där konsumenten utfärdar en receive-begäran och sedan kraschar innan den kan bearbeta denna begäran.</span><span class="sxs-lookup"><span data-stu-id="99786-156">To understand this, consider a scenario in which the consumer issues the receive request and then crashes before processing it.</span></span> <span data-ttu-id="99786-157">Eftersom Service Bus kommer att ha markerat meddelandet som Förbrukat, och sedan när programmet startas om och börjar förbruka meddelanden igen, att ha missat meddelandet som förbrukades innan kraschen.</span><span class="sxs-lookup"><span data-stu-id="99786-157">Because Service Bus will have marked the message as being consumed, then when the application restarts and begins consuming messages again, it will have missed the message that was consumed prior to the crash.</span></span>

<span data-ttu-id="99786-158">Om den `isPeekLock` parametern anges till **SANT**, inleveransen en åtgärd i två steg, vilket gör det möjligt att stödprogram som inte tolererar att ett meddelande saknas.</span><span class="sxs-lookup"><span data-stu-id="99786-158">If the `isPeekLock` parameter is set to **true**, the receive becomes a two stage operation, which makes it possible to support applications that cannot tolerate missing messages.</span></span> <span data-ttu-id="99786-159">När Service Bus tar emot en begäran letar det upp nästa meddelande som ska förbrukas, låser det för att förhindra att andra användare tar emot det och skickar sedan tillbaka det till programmet.</span><span class="sxs-lookup"><span data-stu-id="99786-159">When Service Bus receives a request, it finds the next message to be consumed, locks it to prevent other consumers receiving it, and then returns it to the application.</span></span> <span data-ttu-id="99786-160">När programmet har avslutat bearbetningen av meddelandet (eller lagrar det på ett tillförlitligt sätt för framtida bearbetning), Slutför det andra steget i processen genom att anropa `deleteMessage` metod och ge meddelandet som ska tas bort som en parameter.</span><span class="sxs-lookup"><span data-stu-id="99786-160">After the application finishes processing the message (or stores it reliably for future processing), it completes the second stage of the receive process by calling `deleteMessage` method and providing the message to be deleted as a parameter.</span></span> <span data-ttu-id="99786-161">Den `deleteMessage` metoden markerar meddelandet som Förbrukat och tas bort från kön.</span><span class="sxs-lookup"><span data-stu-id="99786-161">The `deleteMessage` method marks the message as being consumed and removes it from the queue.</span></span>

<span data-ttu-id="99786-162">Exemplet nedan visar hur du tar emot och bearbetar meddelanden med `receiveQueueMessage`.</span><span class="sxs-lookup"><span data-stu-id="99786-162">The following example demonstrates how to receive and process messages using `receiveQueueMessage`.</span></span> <span data-ttu-id="99786-163">I exempel först tar emot och tar bort ett meddelande och tar emot ett meddelande med hjälp av `isPeekLock` inställd på **SANT**, tar bort meddelande med `deleteMessage`:</span><span class="sxs-lookup"><span data-stu-id="99786-163">The example first receives and deletes a message, and then receives a message using `isPeekLock` set to **true**, then deletes the message using `deleteMessage`:</span></span>

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

## <a name="how-to-handle-application-crashes-and-unreadable-messages"></a><span data-ttu-id="99786-164">Hantera programkrascher och oläsbara meddelanden</span><span class="sxs-lookup"><span data-stu-id="99786-164">How to handle application crashes and unreadable messages</span></span>
<span data-ttu-id="99786-165">Service Bus innehåller funktioner som hjälper dig att återställa fel i programmet eller lösa problem med bearbetning av meddelanden på ett snyggt sätt.</span><span class="sxs-lookup"><span data-stu-id="99786-165">Service Bus provides functionality to help you gracefully recover from errors in your application or difficulties processing a message.</span></span> <span data-ttu-id="99786-166">Om ett mottagarprogram går inte att bearbeta meddelandet av någon anledning, så kan det anropa den `unlockMessage` -metoden i den **ServiceBusService** objekt.</span><span class="sxs-lookup"><span data-stu-id="99786-166">If a receiver application is unable to process the message for some reason, then it can call the `unlockMessage` method on the **ServiceBusService** object.</span></span> <span data-ttu-id="99786-167">Detta gör att Service Bus låser upp meddelandet i kön och gör det tillgängligt att tas emot igen, antingen genom samma användningsprogram eller ett annat användningsprogram.</span><span class="sxs-lookup"><span data-stu-id="99786-167">This will cause Service Bus to unlock the message within the queue and make it available to be received again, either by the same consuming application or by another consuming application.</span></span>

<span data-ttu-id="99786-168">Det finns också en tidsgräns som är associerad med ett meddelande som ligger låst i kön och om programmet misslyckas med att bearbeta meddelandet innan timeout för lås upphör att gälla (t.ex. Om programmet kraschar), kommer Service Bus att låsa upp meddelandet automatiskt och gör det tillgängligt att tas emot igen.</span><span class="sxs-lookup"><span data-stu-id="99786-168">There is also a timeout associated with a message locked within the queue, and if the application fails to process the message before the lock timeout expires (e.g., if the application crashes), then Service Bus will unlock the message automatically and make it available to be received again.</span></span>

<span data-ttu-id="99786-169">I händelse av att programmet kraschar efter att meddelandet har bearbetats men innan den `deleteMessage` metoden anropas sedan meddelandet att levereras till programmet när den startas om.</span><span class="sxs-lookup"><span data-stu-id="99786-169">In the event that the application crashes after processing the message but before the `deleteMessage` method is called, then the message will be redelivered to the application when it restarts.</span></span> <span data-ttu-id="99786-170">Det här kallas ofta *At Least Once Processing*, det vill säga, varje meddelande bearbetas minst en gång men i vissa situationer kan samma meddelande levereras.</span><span class="sxs-lookup"><span data-stu-id="99786-170">This is often called *At Least Once Processing*, that is, each message will be processed at least once but in certain situations the same message may be redelivered.</span></span> <span data-ttu-id="99786-171">Om scenariot inte tolererar duplicerad bearbetning, bör programutvecklarna lägga till ytterligare logik i sina program för att hantera duplicerad meddelandeleverans.</span><span class="sxs-lookup"><span data-stu-id="99786-171">If the scenario cannot tolerate duplicate processing, then application developers should add additional logic to their application to handle duplicate message delivery.</span></span> <span data-ttu-id="99786-172">Detta uppnås ofta med hjälp av den **MessageId** för meddelandet, som förblir konstant under alla leveransförsök.</span><span class="sxs-lookup"><span data-stu-id="99786-172">This is often achieved using the **MessageId** property of the message, which will remain constant across delivery attempts.</span></span>

## <a name="next-steps"></a><span data-ttu-id="99786-173">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="99786-173">Next steps</span></span>
<span data-ttu-id="99786-174">Mer information om köer finns i följande resurser.</span><span class="sxs-lookup"><span data-stu-id="99786-174">To learn more about queues, see the following resources.</span></span>

* <span data-ttu-id="99786-175">[Köer, ämnen och prenumerationer][Queues, topics, and subscriptions]</span><span class="sxs-lookup"><span data-stu-id="99786-175">[Queues, topics, and subscriptions][Queues, topics, and subscriptions]</span></span>
* <span data-ttu-id="99786-176">[Azure SDK för noden] [ Azure SDK for Node] databasen på GitHub</span><span class="sxs-lookup"><span data-stu-id="99786-176">[Azure SDK for Node][Azure SDK for Node] repository on GitHub</span></span>
* [<span data-ttu-id="99786-177">Node.js Developer Center</span><span class="sxs-lookup"><span data-stu-id="99786-177">Node.js Developer Center</span></span>](https://azure.microsoft.com/develop/nodejs/)

[Azure SDK for Node]: https://github.com/Azure/azure-sdk-for-node
[Azure portal]: https://portal.azure.com

[Node.js Cloud Service]: ../cloud-services/cloud-services-nodejs-develop-deploy-app.md
[Queues, topics, and subscriptions]: service-bus-queues-topics-subscriptions.md
[Create and deploy a Node.js application to an Azure Website]: ../app-service-web/app-service-web-get-started-nodejs.md
[Node.js Cloud Service with Storage]:../cosmos-db/table-storage-cloud-service-nodejs.md
[Node.js Web Application with Storage]:../cosmos-db/table-storage-how-to-use-nodejs.md
[Service Bus quotas]: service-bus-quotas.md
