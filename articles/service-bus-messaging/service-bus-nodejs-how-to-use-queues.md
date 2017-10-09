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
# <a name="how-toouse-service-bus-queues-with-nodejs"></a><span data-ttu-id="dda45-103">Hur toouse Service Bus köer med Node.js</span><span class="sxs-lookup"><span data-stu-id="dda45-103">How toouse Service Bus queues with Node.js</span></span>

[!INCLUDE [service-bus-selector-queues](../../includes/service-bus-selector-queues.md)]

<span data-ttu-id="dda45-104">Den här artikeln beskriver hur toouse Service Bus köer med Node.js.</span><span class="sxs-lookup"><span data-stu-id="dda45-104">This article describes how toouse Service Bus queues with Node.js.</span></span> <span data-ttu-id="dda45-105">hello exemplen är skrivna i JavaScript och använder hello Node.js Azure-modulen.</span><span class="sxs-lookup"><span data-stu-id="dda45-105">hello samples are written in JavaScript and use hello Node.js Azure module.</span></span> <span data-ttu-id="dda45-106">hello beskrivs scenarier där **skapa köer**, **skicka och ta emot meddelanden**, och **tar bort köer**.</span><span class="sxs-lookup"><span data-stu-id="dda45-106">hello scenarios covered include **creating queues**, **sending and receiving messages**, and **deleting queues**.</span></span> <span data-ttu-id="dda45-107">Mer information om köer finns hello [nästa steg](#next-steps) avsnitt.</span><span class="sxs-lookup"><span data-stu-id="dda45-107">For more information on queues, see hello [Next steps](#next-steps) section.</span></span>

[!INCLUDE [howto-service-bus-queues](../../includes/howto-service-bus-queues.md)]

[!INCLUDE [service-bus-create-namespace-portal](../../includes/service-bus-create-namespace-portal.md)]

## <a name="create-a-nodejs-application"></a><span data-ttu-id="dda45-108">Skapa ett Node.js-program</span><span class="sxs-lookup"><span data-stu-id="dda45-108">Create a Node.js application</span></span>
<span data-ttu-id="dda45-109">Skapa ett tomt Node.js-program.</span><span class="sxs-lookup"><span data-stu-id="dda45-109">Create a blank Node.js application.</span></span> <span data-ttu-id="dda45-110">Anvisningar för hur toocreate ett Node.js-program finns [skapa och distribuera en Node.js-programmet tooan Azure-webbplats][Create and deploy a Node.js application tooan Azure Website], eller [Node.js Molntjänsten] [ Node.js Cloud Service] med Windows PowerShell.</span><span class="sxs-lookup"><span data-stu-id="dda45-110">For instructions on how toocreate a Node.js application, see [Create and deploy a Node.js application tooan Azure Website][Create and deploy a Node.js application tooan Azure Website], or [Node.js Cloud Service][Node.js Cloud Service] using Windows PowerShell.</span></span>

## <a name="configure-your-application-toouse-service-bus"></a><span data-ttu-id="dda45-111">Konfigurera ditt program toouse Service Bus</span><span class="sxs-lookup"><span data-stu-id="dda45-111">Configure your application toouse Service Bus</span></span>
<span data-ttu-id="dda45-112">toouse Azure Service Bus, hämtar och använder hello Node.js Azure-paketet.</span><span class="sxs-lookup"><span data-stu-id="dda45-112">toouse Azure Service Bus, download and use hello Node.js Azure package.</span></span> <span data-ttu-id="dda45-113">Det här paketet innehåller en uppsättning bibliotek som kommunicerar med hello Service Bus REST-tjänster.</span><span class="sxs-lookup"><span data-stu-id="dda45-113">This package includes a set of libraries that communicate with hello Service Bus REST services.</span></span>

### <a name="use-node-package-manager-npm-tooobtain-hello-package"></a><span data-ttu-id="dda45-114">Använd noden Package Manager (NPM) tooobtain hello-paket</span><span class="sxs-lookup"><span data-stu-id="dda45-114">Use Node Package Manager (NPM) tooobtain hello package</span></span>
1. <span data-ttu-id="dda45-115">Använd hello **Windows PowerShell för Node.js** kommandot fönstret toonavigate toohello **c:\\nod\\sbqueues\\WebRole1** mapp som du skapade din exempelprogrammet.</span><span class="sxs-lookup"><span data-stu-id="dda45-115">Use hello **Windows PowerShell for Node.js** command window toonavigate toohello **c:\\node\\sbqueues\\WebRole1** folder in which you created your sample application.</span></span>
2. <span data-ttu-id="dda45-116">Typen **npm installera azure** i hello-kommandofönster som bör resultera i utdata liknande toohello följande:</span><span class="sxs-lookup"><span data-stu-id="dda45-116">Type **npm install azure** in hello command window, which should result in output similar toohello following:</span></span>

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
3. <span data-ttu-id="dda45-117">Du kan köra hello manuellt **ls** kommandot tooverify som en **node_modules** mappen har skapats.</span><span class="sxs-lookup"><span data-stu-id="dda45-117">You can manually run hello **ls** command tooverify that a **node_modules** folder was created.</span></span> <span data-ttu-id="dda45-118">I den mappen hitta hello **azure** paket som innehåller hello bibliotek som du behöver tooaccess Service Bus-köer.</span><span class="sxs-lookup"><span data-stu-id="dda45-118">Inside that folder find hello **azure** package, which contains hello libraries you need tooaccess Service Bus queues.</span></span>

### <a name="import-hello-module"></a><span data-ttu-id="dda45-119">Importera hello-modul</span><span class="sxs-lookup"><span data-stu-id="dda45-119">Import hello module</span></span>
<span data-ttu-id="dda45-120">Använda anteckningar eller något annat textredigeringsprogram Lägg till följande toohello överkant hello hello **server.js** -filen för programmet hello:</span><span class="sxs-lookup"><span data-stu-id="dda45-120">Using Notepad or another text editor, add hello following toohello top of hello **server.js** file of hello application:</span></span>

```javascript
var azure = require('azure');
```

### <a name="set-up-an-azure-service-bus-connection"></a><span data-ttu-id="dda45-121">Azure Service Bus-anslutningar</span><span class="sxs-lookup"><span data-stu-id="dda45-121">Set up an Azure Service Bus connection</span></span>
<span data-ttu-id="dda45-122">hello Azure modulen läser hello miljövariabeln `AZURE_SERVICEBUS_CONNECTION_STRING` tooobtain information krävs tooconnect tooService Bus.</span><span class="sxs-lookup"><span data-stu-id="dda45-122">hello Azure module reads hello environment variable `AZURE_SERVICEBUS_CONNECTION_STRING` tooobtain information required tooconnect tooService Bus.</span></span> <span data-ttu-id="dda45-123">Om den här miljövariabeln inte har angetts måste du ange hello kontoinformation när du anropar `createServiceBusService`.</span><span class="sxs-lookup"><span data-stu-id="dda45-123">If this environment variable is not set, you must specify hello account information when calling `createServiceBusService`.</span></span>

<span data-ttu-id="dda45-124">Ett exempel på hur hello miljövariabler i en konfigurationsfil för ett Azure Cloud Service, se [Node.js molntjänst med lagring][Node.js Cloud Service with Storage].</span><span class="sxs-lookup"><span data-stu-id="dda45-124">For an example of setting hello environment variables in a configuration file for an Azure Cloud Service, see [Node.js Cloud Service with Storage][Node.js Cloud Service with Storage].</span></span>

<span data-ttu-id="dda45-125">Ett exempel på inställningen hello miljövariabler i hello [Azure-portalen] [ Azure portal] en Azure-webbplats finns [Node.js-Webbapp med lagring] [ Node.js Web Application with Storage].</span><span class="sxs-lookup"><span data-stu-id="dda45-125">For an example of setting hello environment variables in hello [Azure portal][Azure portal] for an Azure Website, see [Node.js Web Application with Storage][Node.js Web Application with Storage].</span></span>

## <a name="create-a-queue"></a><span data-ttu-id="dda45-126">Skapa en kö</span><span class="sxs-lookup"><span data-stu-id="dda45-126">Create a queue</span></span>
<span data-ttu-id="dda45-127">Hej **ServiceBusService** objekt kan du toowork med Service Bus-köer.</span><span class="sxs-lookup"><span data-stu-id="dda45-127">hello **ServiceBusService** object enables you toowork with Service Bus queues.</span></span> <span data-ttu-id="dda45-128">hello följande kod skapar en **ServiceBusService** objekt.</span><span class="sxs-lookup"><span data-stu-id="dda45-128">hello following code creates a **ServiceBusService** object.</span></span> <span data-ttu-id="dda45-129">Lägg till den hello övre delen av hello **server.js** filen efter hello instruktionen tooimport hello Azure-modulen:</span><span class="sxs-lookup"><span data-stu-id="dda45-129">Add it near hello top of hello **server.js** file, after hello statement tooimport hello Azure module:</span></span>

```javascript
var serviceBusService = azure.createServiceBusService();
```

<span data-ttu-id="dda45-130">Genom att anropa `createQueueIfNotExists` på hello **ServiceBusService** objekt hello angetts kön returneras (om det finns) eller en ny kö med hello angivna namnet har skapats.</span><span class="sxs-lookup"><span data-stu-id="dda45-130">By calling `createQueueIfNotExists` on hello **ServiceBusService** object, hello specified queue is returned (if it exists), or a new queue with hello specified name is created.</span></span> <span data-ttu-id="dda45-131">hello följande kod används `createQueueIfNotExists` toocreate eller ansluta toohello kö med namnet `myqueue`:</span><span class="sxs-lookup"><span data-stu-id="dda45-131">hello following code uses `createQueueIfNotExists` toocreate or connect toohello queue named `myqueue`:</span></span>

```javascript
serviceBusService.createQueueIfNotExists('myqueue', function(error){
    if(!error){
        // Queue exists
    }
});
```

<span data-ttu-id="dda45-132">Hej `createServiceBusService` metoden stöder också ytterligare alternativ som gör att du toooverride kön standardinställningarna, till exempel meddelande tid toolive eller maximal köstorlek.</span><span class="sxs-lookup"><span data-stu-id="dda45-132">hello `createServiceBusService` method also supports additional options, which enable you toooverride default queue settings such as message time toolive or maximum queue size.</span></span> <span data-ttu-id="dda45-133">hello följande exempel anger hello största köstorlek storlek too5 GB och en tid toolive TTL-värde på 1 minut:</span><span class="sxs-lookup"><span data-stu-id="dda45-133">hello following example sets hello maximum queue size too5 GB, and a time toolive (TTL) value of 1 minute:</span></span>

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

### <a name="filters"></a><span data-ttu-id="dda45-134">Filter</span><span class="sxs-lookup"><span data-stu-id="dda45-134">Filters</span></span>
<span data-ttu-id="dda45-135">Valfria filtrering åtgärder kan vara tillämpade toooperations utförs med hjälp av **ServiceBusService**.</span><span class="sxs-lookup"><span data-stu-id="dda45-135">Optional filtering operations can be applied toooperations performed using **ServiceBusService**.</span></span> <span data-ttu-id="dda45-136">Filtrering operations kan innehålla loggning, försöker automatiskt igen och så vidare. Filtren är objekt som implementerar en metod med hello signatur:</span><span class="sxs-lookup"><span data-stu-id="dda45-136">Filtering operations can include logging, automatically retrying, etc. Filters are objects that implement a method with hello signature:</span></span>

```javascript
function handle (requestOptions, next)
```

<span data-ttu-id="dda45-137">När du har gjort dess föregående bearbetning på hello begäran alternativ hello-metoden måste anropa `next`, skicka ett återanrop med hello följande signatur:</span><span class="sxs-lookup"><span data-stu-id="dda45-137">After doing its pre-processing on hello request options, hello method must call `next`, passing a callback with hello following signature:</span></span>

```javascript
function (returnObject, finalCallback, next)
```

<span data-ttu-id="dda45-138">I den här motringning och efter bearbetning hello `returnObject` (hello svar från hello begäran toohello server), hello-återanrop måste antingen anropa `next` om det finns toocontinue bearbetning filter eller helt enkelt anropa `finalCallback`, vilka slutar hello service-anrop.</span><span class="sxs-lookup"><span data-stu-id="dda45-138">In this callback, and after processing hello `returnObject` (hello response from hello request toohello server), hello callback must either invoke `next` if it exists toocontinue processing other filters, or simply invoke `finalCallback`, which ends hello service invocation.</span></span>

<span data-ttu-id="dda45-139">Två filter som implementerar logik som medföljer hello Azure SDK för Node.js, `ExponentialRetryPolicyFilter` och `LinearRetryPolicyFilter`.</span><span class="sxs-lookup"><span data-stu-id="dda45-139">Two filters that implement retry logic are included with hello Azure SDK for Node.js, `ExponentialRetryPolicyFilter` and `LinearRetryPolicyFilter`.</span></span> <span data-ttu-id="dda45-140">hello följande kod skapar en `ServiceBusService` objekt som använder hello `ExponentialRetryPolicyFilter`:</span><span class="sxs-lookup"><span data-stu-id="dda45-140">hello following code creates a `ServiceBusService` object that uses hello `ExponentialRetryPolicyFilter`:</span></span>

```javascript
var retryOperations = new azure.ExponentialRetryPolicyFilter();
var serviceBusService = azure.createServiceBusService().withFilter(retryOperations);
```

## <a name="send-messages-tooa-queue"></a><span data-ttu-id="dda45-141">Skicka meddelanden tooa kön</span><span class="sxs-lookup"><span data-stu-id="dda45-141">Send messages tooa queue</span></span>
<span data-ttu-id="dda45-142">toosend en meddelandekö tooa Service Bus programmet anropar hello `sendQueueMessage` metod på hello **ServiceBusService** objekt.</span><span class="sxs-lookup"><span data-stu-id="dda45-142">toosend a message tooa Service Bus queue, your application calls hello `sendQueueMessage` method on hello **ServiceBusService** object.</span></span> <span data-ttu-id="dda45-143">Meddelanden skickas för (och mottagna från) Service Bus är köer **BrokeredMessage** objekt och har en uppsättning standardegenskaper (t.ex **etikett** och **TimeToLive**), ordlista som används toohold anpassade programspecifika egenskaper och en brödtext med godtyckliga programdata.</span><span class="sxs-lookup"><span data-stu-id="dda45-143">Messages sent too(and received from) Service Bus queues are **BrokeredMessage** objects, and have a set of standard properties (such as **Label** and **TimeToLive**), a dictionary that is used toohold custom application-specific properties, and a body of arbitrary application data.</span></span> <span data-ttu-id="dda45-144">Ett program kan konfigurera hello brödtext hello-meddelande genom att skicka en sträng som hello-meddelande.</span><span class="sxs-lookup"><span data-stu-id="dda45-144">An application can set hello body of hello message by passing a string as hello message.</span></span> <span data-ttu-id="dda45-145">Alla nödvändiga standardegenskaper fylls i med standardvärden.</span><span class="sxs-lookup"><span data-stu-id="dda45-145">Any required standard properties are populated with default values.</span></span>

<span data-ttu-id="dda45-146">hello exemplet nedan visar hur toosend en toohello meddelandekö test med namnet `myqueue` med `sendQueueMessage`:</span><span class="sxs-lookup"><span data-stu-id="dda45-146">hello following example demonstrates how toosend a test message toohello queue named `myqueue` using `sendQueueMessage`:</span></span>

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

<span data-ttu-id="dda45-147">Service Bus-köer stöder en maximal meddelandestorlek på 256 KB i hello [standardnivån](service-bus-premium-messaging.md) och 1 MB på hello [premiumnivån](service-bus-premium-messaging.md).</span><span class="sxs-lookup"><span data-stu-id="dda45-147">Service Bus queues support a maximum message size of 256 KB in hello [Standard tier](service-bus-premium-messaging.md) and 1 MB in hello [Premium tier](service-bus-premium-messaging.md).</span></span> <span data-ttu-id="dda45-148">hello-huvudet som innehåller hello standard- och anpassade programegenskaperna, kan ha en maximal storlek på 64 KB.</span><span class="sxs-lookup"><span data-stu-id="dda45-148">hello header, which includes hello standard and custom application properties, can have a maximum size of 64 KB.</span></span> <span data-ttu-id="dda45-149">Det finns ingen gräns hello antalet meddelanden som finns i en kö men det finns ett tak på hello totala storleken på hälsningsmeddelande som innehas av en kö.</span><span class="sxs-lookup"><span data-stu-id="dda45-149">There is no limit on hello number of messages held in a queue but there is a cap on hello total size of hello messages held by a queue.</span></span> <span data-ttu-id="dda45-150">Den här köstorleken definieras när kön skapas, med en övre gräns på 5 GB.</span><span class="sxs-lookup"><span data-stu-id="dda45-150">This queue size is defined at creation time, with an upper limit of 5 GB.</span></span> <span data-ttu-id="dda45-151">Mer information om kvoter finns [Service Bus-kvoter][Service Bus quotas].</span><span class="sxs-lookup"><span data-stu-id="dda45-151">For more information about quotas, see [Service Bus quotas][Service Bus quotas].</span></span>

## <a name="receive-messages-from-a-queue"></a><span data-ttu-id="dda45-152">Ta emot meddelanden från en kö</span><span class="sxs-lookup"><span data-stu-id="dda45-152">Receive messages from a queue</span></span>
<span data-ttu-id="dda45-153">Meddelanden tas emot från en kö med hello `receiveQueueMessage` metod på hello **ServiceBusService** objekt.</span><span class="sxs-lookup"><span data-stu-id="dda45-153">Messages are received from a queue using hello `receiveQueueMessage` method on hello **ServiceBusService** object.</span></span> <span data-ttu-id="dda45-154">Som standard tas meddelanden bort från kön hello som de läses; men du kan läsa (titt) och låsa hello-meddelande utan att ta bort den från hello kö genom att ange hello valfri parameter `isPeekLock` för**SANT**.</span><span class="sxs-lookup"><span data-stu-id="dda45-154">By default, messages are deleted from hello queue as they are read; however, you can read (peek) and lock hello message without deleting it from hello queue by setting hello optional parameter `isPeekLock` too**true**.</span></span>

<span data-ttu-id="dda45-155">Hej standardbeteendet för läsning och ta bort hello-meddelande som en del av hello mottagningsåtgärd är hello enklaste modellen och fungerar bäst för scenarier där ett program kan tolerera icke-bearbetning av ett meddelande i hello händelse av fel.</span><span class="sxs-lookup"><span data-stu-id="dda45-155">hello default behavior of reading and deleting hello message as part of hello receive operation is hello simplest model, and works best for scenarios in which an application can tolerate not processing a message in hello event of a failure.</span></span> <span data-ttu-id="dda45-156">toounderstand, Föreställ i vilka hello konsumenten problem hello mottagning av begäran och sedan kraschar innan bearbetningen.</span><span class="sxs-lookup"><span data-stu-id="dda45-156">toounderstand this, consider a scenario in which hello consumer issues hello receive request and then crashes before processing it.</span></span> <span data-ttu-id="dda45-157">Eftersom Service Bus kommer att ha markerat hello meddelandet som Förbrukat, sedan när hello programmet startas om och börjar förbruka meddelanden igen, att ha missat hello-meddelande som har förbrukats tidigare toohello krascher.</span><span class="sxs-lookup"><span data-stu-id="dda45-157">Because Service Bus will have marked hello message as being consumed, then when hello application restarts and begins consuming messages again, it will have missed hello message that was consumed prior toohello crash.</span></span>

<span data-ttu-id="dda45-158">Om hello `isPeekLock` parameter har angetts för**SANT**, hello får blir en åtgärd i två steg, vilket gör det möjligt toosupport program som inte tolererar att ett meddelande saknas.</span><span class="sxs-lookup"><span data-stu-id="dda45-158">If hello `isPeekLock` parameter is set too**true**, hello receive becomes a two stage operation, which makes it possible toosupport applications that cannot tolerate missing messages.</span></span> <span data-ttu-id="dda45-159">När Service Bus tar emot en begäran, hittar hello nästa meddelande toobe förbrukas, låser det tooprevent andra användare tar emot det och returnerar sedan toohello program.</span><span class="sxs-lookup"><span data-stu-id="dda45-159">When Service Bus receives a request, it finds hello next message toobe consumed, locks it tooprevent other consumers receiving it, and then returns it toohello application.</span></span> <span data-ttu-id="dda45-160">När hello programmet har avslutat bearbetningen hello-meddelande (eller lagrar det på ett tillförlitligt sätt för framtida bearbetning), Slutför hello andra steget i hello får processen genom att anropa `deleteMessage` metod och ge hello meddelandet toobe tas bort som en parameter.</span><span class="sxs-lookup"><span data-stu-id="dda45-160">After hello application finishes processing hello message (or stores it reliably for future processing), it completes hello second stage of hello receive process by calling `deleteMessage` method and providing hello message toobe deleted as a parameter.</span></span> <span data-ttu-id="dda45-161">Hej `deleteMessage` metoden markerar hello meddelandet som Förbrukat och tar bort det från hello kö.</span><span class="sxs-lookup"><span data-stu-id="dda45-161">hello `deleteMessage` method marks hello message as being consumed and removes it from hello queue.</span></span>

<span data-ttu-id="dda45-162">hello exemplet nedan visar hur tooreceive och bearbeta meddelanden med hjälp av `receiveQueueMessage`.</span><span class="sxs-lookup"><span data-stu-id="dda45-162">hello following example demonstrates how tooreceive and process messages using `receiveQueueMessage`.</span></span> <span data-ttu-id="dda45-163">hello exempel först tar emot och tar bort ett meddelande och tar emot ett meddelande med hjälp av `isPeekLock` ställa in också**SANT**, och sedan tar bort hello meddelande med `deleteMessage`:</span><span class="sxs-lookup"><span data-stu-id="dda45-163">hello example first receives and deletes a message, and then receives a message using `isPeekLock` set too**true**, then deletes hello message using `deleteMessage`:</span></span>

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

## <a name="how-toohandle-application-crashes-and-unreadable-messages"></a><span data-ttu-id="dda45-164">Hur toohandle programmet kraschar och oläsbara meddelanden</span><span class="sxs-lookup"><span data-stu-id="dda45-164">How toohandle application crashes and unreadable messages</span></span>
<span data-ttu-id="dda45-165">Service Bus innehåller funktioner toohelp du att återställa fel i programmet eller problem med bearbetning av ett meddelande.</span><span class="sxs-lookup"><span data-stu-id="dda45-165">Service Bus provides functionality toohelp you gracefully recover from errors in your application or difficulties processing a message.</span></span> <span data-ttu-id="dda45-166">Om ett mottagarprogram inte kan tooprocess hello meddelande av någon anledning, så kan det anropa hello `unlockMessage` metod på hello **ServiceBusService** objekt.</span><span class="sxs-lookup"><span data-stu-id="dda45-166">If a receiver application is unable tooprocess hello message for some reason, then it can call hello `unlockMessage` method on hello **ServiceBusService** object.</span></span> <span data-ttu-id="dda45-167">Detta orsakar Service Bus toounlock meddelandet i kön för hello och göra den tillgänglig toobe tas emot igen, antingen hello av samma förbrukning av ett program eller ett annat användningsprogram.</span><span class="sxs-lookup"><span data-stu-id="dda45-167">This will cause Service Bus toounlock the message within hello queue and make it available toobe received again, either by hello same consuming application or by another consuming application.</span></span>

<span data-ttu-id="dda45-168">Det finns också en tidsgräns som är associerad med ett meddelande i kön hello och om hello hello programmet misslyckas tooprocess hello meddelandet innan timeout för lås går ut (t.ex. Om hello programmet kraschar), kommer Service Bus låser upp hello-meddelande automatiskt och gör den tillgänglig toobe tas emot igen.</span><span class="sxs-lookup"><span data-stu-id="dda45-168">There is also a timeout associated with a message locked within hello queue, and if hello application fails tooprocess hello message before hello lock timeout expires (e.g., if hello application crashes), then Service Bus will unlock hello message automatically and make it available toobe received again.</span></span>

<span data-ttu-id="dda45-169">Hello händelse som hello programmet kraschar efter hello meddelandet har bearbetats men innan hello `deleteMessage` metoden anropas sedan hello-meddelande kommer att levereras på nytt toohello program när den startas om.</span><span class="sxs-lookup"><span data-stu-id="dda45-169">In hello event that hello application crashes after processing hello message but before hello `deleteMessage` method is called, then hello message will be redelivered toohello application when it restarts.</span></span> <span data-ttu-id="dda45-170">Det här kallas ofta *At Least Once Processing*, det vill säga, varje meddelande bearbetas minst en gång men i vissa situationer hello samma meddelande levereras.</span><span class="sxs-lookup"><span data-stu-id="dda45-170">This is often called *At Least Once Processing*, that is, each message will be processed at least once but in certain situations hello same message may be redelivered.</span></span> <span data-ttu-id="dda45-171">Om hello scenariot inte tolererar duplicerad bearbetning, sedan bör programutvecklarna lägga till ytterligare logik tootheir duplicerad meddelandeleverans för programmet toohandle.</span><span class="sxs-lookup"><span data-stu-id="dda45-171">If hello scenario cannot tolerate duplicate processing, then application developers should add additional logic tootheir application toohandle duplicate message delivery.</span></span> <span data-ttu-id="dda45-172">Detta uppnås ofta med hjälp av hello **MessageId** -egenskapen för hello-meddelande, förblir konstant under alla leveransförsök.</span><span class="sxs-lookup"><span data-stu-id="dda45-172">This is often achieved using hello **MessageId** property of hello message, which will remain constant across delivery attempts.</span></span>

## <a name="next-steps"></a><span data-ttu-id="dda45-173">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="dda45-173">Next steps</span></span>
<span data-ttu-id="dda45-174">toolearn mer om köer finns hello följande resurser.</span><span class="sxs-lookup"><span data-stu-id="dda45-174">toolearn more about queues, see hello following resources.</span></span>

* <span data-ttu-id="dda45-175">[Köer, ämnen och prenumerationer][Queues, topics, and subscriptions]</span><span class="sxs-lookup"><span data-stu-id="dda45-175">[Queues, topics, and subscriptions][Queues, topics, and subscriptions]</span></span>
* <span data-ttu-id="dda45-176">[Azure SDK för noden] [ Azure SDK for Node] databasen på GitHub</span><span class="sxs-lookup"><span data-stu-id="dda45-176">[Azure SDK for Node][Azure SDK for Node] repository on GitHub</span></span>
* [<span data-ttu-id="dda45-177">Node.js Developer Center</span><span class="sxs-lookup"><span data-stu-id="dda45-177">Node.js Developer Center</span></span>](https://azure.microsoft.com/develop/nodejs/)

[Azure SDK for Node]: https://github.com/Azure/azure-sdk-for-node
[Azure portal]: https://portal.azure.com

[Node.js Cloud Service]: ../cloud-services/cloud-services-nodejs-develop-deploy-app.md
[Queues, topics, and subscriptions]: service-bus-queues-topics-subscriptions.md
[Create and deploy a Node.js application tooan Azure Website]: ../app-service-web/app-service-web-get-started-nodejs.md
[Node.js Cloud Service with Storage]:../cosmos-db/table-storage-cloud-service-nodejs.md
[Node.js Web Application with Storage]:../cosmos-db/table-storage-how-to-use-nodejs.md
[Service Bus quotas]: service-bus-quotas.md
