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
# <a name="how-toouse-service-bus-topics-and-subscriptions-with-nodejs"></a><span data-ttu-id="4db96-103">Hur tooUse Service Bus-ämnen och prenumerationer med Node.js</span><span class="sxs-lookup"><span data-stu-id="4db96-103">How tooUse Service Bus topics and subscriptions with Node.js</span></span>

[!INCLUDE [service-bus-selector-topics](../../includes/service-bus-selector-topics.md)]

<span data-ttu-id="4db96-104">Den här guiden beskriver hur toouse Service Bus-ämnen och prenumerationer från Node.js-program.</span><span class="sxs-lookup"><span data-stu-id="4db96-104">This guide describes how toouse Service Bus topics and subscriptions from Node.js applications.</span></span> <span data-ttu-id="4db96-105">hello beskrivs scenarier där **skapa ämnen och prenumerationer**, **skapa prenumerationsfilter**, **skickar meddelanden** tooa avsnittet **tar emot meddelanden från en prenumeration**, och **ta bort ämnen och prenumerationer**.</span><span class="sxs-lookup"><span data-stu-id="4db96-105">hello scenarios covered include **creating topics and subscriptions**, **creating subscription filters**, **sending messages** tooa topic, **receiving messages from a subscription**, and **deleting topics and subscriptions**.</span></span> <span data-ttu-id="4db96-106">Mer information om ämnen och prenumerationer finns hello [nästa steg](#next-steps) avsnitt.</span><span class="sxs-lookup"><span data-stu-id="4db96-106">For more information about topics and subscriptions, see hello [Next steps](#next-steps) section.</span></span>

[!INCLUDE [howto-service-bus-topics](../../includes/howto-service-bus-topics.md)]

## <a name="create-a-nodejs-application"></a><span data-ttu-id="4db96-107">Skapa ett Node.js-program</span><span class="sxs-lookup"><span data-stu-id="4db96-107">Create a Node.js application</span></span>
<span data-ttu-id="4db96-108">Skapa ett tomt Node.js-program.</span><span class="sxs-lookup"><span data-stu-id="4db96-108">Create a blank Node.js application.</span></span> <span data-ttu-id="4db96-109">Instruktioner om hur du skapar en Node.js-program finns i [skapa och distribuera en Node.js-programmet tooan webbplatsen Azure], [Node.js Molntjänsten] [ Node.js Cloud Service] med hjälp av Windows PowerShell eller webbplatsen med WebMatrix.</span><span class="sxs-lookup"><span data-stu-id="4db96-109">For instructions on creating a Node.js application, see [Create and deploy a Node.js application tooan Azure Web Site], [Node.js Cloud Service][Node.js Cloud Service] using Windows PowerShell, or Web Site with WebMatrix.</span></span>

## <a name="configure-your-application-toouse-service-bus"></a><span data-ttu-id="4db96-110">Konfigurera ditt program toouse Service Bus</span><span class="sxs-lookup"><span data-stu-id="4db96-110">Configure your application toouse Service Bus</span></span>
<span data-ttu-id="4db96-111">toouse Service Bus, hämta hello Node.js Azure-paketet.</span><span class="sxs-lookup"><span data-stu-id="4db96-111">toouse Service Bus, download hello Node.js Azure package.</span></span> <span data-ttu-id="4db96-112">Det här paketet innehåller en uppsättning bibliotek som kommunicerar med hello Service Bus REST-tjänster.</span><span class="sxs-lookup"><span data-stu-id="4db96-112">This package includes a set of libraries that communicate with hello Service Bus REST services.</span></span>

### <a name="use-node-package-manager-npm-tooobtain-hello-package"></a><span data-ttu-id="4db96-113">Använd noden Package Manager (NPM) tooobtain hello-paket</span><span class="sxs-lookup"><span data-stu-id="4db96-113">Use Node Package Manager (NPM) tooobtain hello package</span></span>
1. <span data-ttu-id="4db96-114">Använd ett kommandoradsgränssnitt som **PowerShell** (Windows), **Terminal** (Mac) eller **Bash** (Unix), navigera toohello mappen där du skapade exempelprogrammet.</span><span class="sxs-lookup"><span data-stu-id="4db96-114">Use a command-line interface such as **PowerShell** (Windows,) **Terminal** (Mac,) or **Bash** (Unix), navigate toohello folder where you created your sample application.</span></span>
2. <span data-ttu-id="4db96-115">Typen **npm installera azure** i hello-kommandofönster som bör resultera i hello följande utdata:</span><span class="sxs-lookup"><span data-stu-id="4db96-115">Type **npm install azure** in hello command window, which should result in hello following output:</span></span>

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
3. <span data-ttu-id="4db96-116">Du kan köra hello manuellt **ls** kommandot tooverify som en **nod\_moduler** mappen har skapats.</span><span class="sxs-lookup"><span data-stu-id="4db96-116">You can manually run hello **ls** command tooverify that a **node\_modules** folder was created.</span></span> <span data-ttu-id="4db96-117">I mappen hitta den **azure** paket som innehåller hello bibliotek som du behöver tooaccess Service Bus-ämnen.</span><span class="sxs-lookup"><span data-stu-id="4db96-117">Inside that folder find the **azure** package, which contains hello libraries you need tooaccess Service Bus topics.</span></span>

### <a name="import-hello-module"></a><span data-ttu-id="4db96-118">Importera hello-modul</span><span class="sxs-lookup"><span data-stu-id="4db96-118">Import hello module</span></span>
<span data-ttu-id="4db96-119">Använda anteckningar eller något annat textredigeringsprogram Lägg till följande toohello överkant hello hello **server.js** -filen för programmet hello:</span><span class="sxs-lookup"><span data-stu-id="4db96-119">Using Notepad or another text editor, add hello following toohello top of hello **server.js** file of hello application:</span></span>

```javascript
var azure = require('azure');
```

### <a name="set-up-a-service-bus-connection"></a><span data-ttu-id="4db96-120">Konfigurera en Service Bus-anslutning</span><span class="sxs-lookup"><span data-stu-id="4db96-120">Set up a Service Bus connection</span></span>
<span data-ttu-id="4db96-121">hello Azure modulen läser hello miljövariabler `AZURE_SERVICEBUS_NAMESPACE` och `AZURE_SERVICEBUS_ACCESS_KEY` information krävs tooconnect tooService Bus.</span><span class="sxs-lookup"><span data-stu-id="4db96-121">hello Azure module reads hello environment variables `AZURE_SERVICEBUS_NAMESPACE` and `AZURE_SERVICEBUS_ACCESS_KEY` for information required tooconnect tooService Bus.</span></span> <span data-ttu-id="4db96-122">Om de här miljövariablerna inte har angetts måste du ange hello kontoinformation när du anropar `createServiceBusService`.</span><span class="sxs-lookup"><span data-stu-id="4db96-122">If these environment variables are not set, you must specify hello account information when calling `createServiceBusService`.</span></span>

<span data-ttu-id="4db96-123">Ett exempel på hur hello miljövariabler för ett Azure Cloud Service, se [Node.js molntjänst med lagring][Node.js Cloud Service with Storage].</span><span class="sxs-lookup"><span data-stu-id="4db96-123">For an example of setting hello environment variables for an Azure Cloud Service, see [Node.js Cloud Service with Storage][Node.js Cloud Service with Storage].</span></span>

<span data-ttu-id="4db96-124">Ett exempel på hur hello miljövariabler för en Azure-webbplats finns [Node.js-Webbapp med lagring][Node.js Web Application with Storage].</span><span class="sxs-lookup"><span data-stu-id="4db96-124">For an example of setting hello environment variables for an Azure Website, see [Node.js Web Application with Storage][Node.js Web Application with Storage].</span></span>

## <a name="create-a-topic"></a><span data-ttu-id="4db96-125">Skapa ett ämne</span><span class="sxs-lookup"><span data-stu-id="4db96-125">Create a topic</span></span>
<span data-ttu-id="4db96-126">Hej **ServiceBusService** objekt kan du toowork med ämnen.</span><span class="sxs-lookup"><span data-stu-id="4db96-126">hello **ServiceBusService** object enables you toowork with topics.</span></span> <span data-ttu-id="4db96-127">Följande kod skapar en **ServiceBusService** objekt.</span><span class="sxs-lookup"><span data-stu-id="4db96-127">The following code creates a **ServiceBusService** object.</span></span> <span data-ttu-id="4db96-128">Lägg till den övre delen av hello **server.js** filen efter hello instruktionen tooimport hello azure modulen:</span><span class="sxs-lookup"><span data-stu-id="4db96-128">Add it near the top of hello **server.js** file, after hello statement tooimport hello azure module:</span></span>

```javascript
var serviceBusService = azure.createServiceBusService();
```

<span data-ttu-id="4db96-129">Genom att anropa `createTopicIfNotExists` på hello **ServiceBusService** objekt hello angetts avsnittet returneras (om den finns) eller ett nytt avsnitt med angivna hello-namnet kommer att skapas.</span><span class="sxs-lookup"><span data-stu-id="4db96-129">By calling `createTopicIfNotExists` on hello **ServiceBusService** object, hello specified topic will be returned (if it exists,) or a new topic with hello specified name will be created.</span></span> <span data-ttu-id="4db96-130">hello följande kod används `createTopicIfNotExists` toocreate eller ansluta toohello ämne med namnet `MyTopic`:</span><span class="sxs-lookup"><span data-stu-id="4db96-130">hello following code uses `createTopicIfNotExists` toocreate or connect toohello topic named `MyTopic`:</span></span>

```javascript
serviceBusService.createTopicIfNotExists('MyTopic',function(error){
    if(!error){
        // Topic was created or exists
        console.log('topic created or exists.');
    }
});
```

<span data-ttu-id="4db96-131">Hej `createServiceBusService` metoden stöder också ytterligare alternativ som aktiverar toooverride avsnittet standardinställningarna, till exempel time to live-meddelande eller avsnittet maximala storlek.</span><span class="sxs-lookup"><span data-stu-id="4db96-131">hello `createServiceBusService` method also supports additional options, which enable you toooverride default topic settings such as message time to live or maximum topic size.</span></span> <span data-ttu-id="4db96-132">hello följande exempel anger hello maximala avsnittet storlek too5GB med tid toolive 1 minut:</span><span class="sxs-lookup"><span data-stu-id="4db96-132">hello following example sets hello maximum topic size too5GB with a time toolive of 1 minute:</span></span>

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

### <a name="filters"></a><span data-ttu-id="4db96-133">Filter</span><span class="sxs-lookup"><span data-stu-id="4db96-133">Filters</span></span>
<span data-ttu-id="4db96-134">Valfria filtrering åtgärder kan vara tillämpade toooperations utförs med hjälp av **ServiceBusService**.</span><span class="sxs-lookup"><span data-stu-id="4db96-134">Optional filtering operations can be applied toooperations performed using **ServiceBusService**.</span></span> <span data-ttu-id="4db96-135">Filtrering operations kan innehålla loggning, försöker automatiskt igen och så vidare. Filtren är objekt som implementerar en metod med hello signatur:</span><span class="sxs-lookup"><span data-stu-id="4db96-135">Filtering operations can include logging, automatically retrying, etc. Filters are objects that implement a method with hello signature:</span></span>

```javascript
function handle (requestOptions, next)
```

<span data-ttu-id="4db96-136">När du utför förbearbetning på hello begäran alternativ måste hello metodanrop `next`, skicka ett återanrop med hello följande signatur:</span><span class="sxs-lookup"><span data-stu-id="4db96-136">After performing preprocessing on hello request options, hello method calls `next`, passing a callback with hello following signature:</span></span>

```javascript
function (returnObject, finalCallback, next)
```

<span data-ttu-id="4db96-137">I den här motringning och efter bearbetning hello `returnObject` (hello svar från hello begäran toohello server), hello-återanrop måste tooeither anropa sedan om den finns toocontinue bearbetning filter eller anropa `finalCallback` annars tooend hello Service-anrop.</span><span class="sxs-lookup"><span data-stu-id="4db96-137">In this callback, and after processing hello `returnObject` (hello response from hello request toohello server), hello callback needs tooeither invoke next if it exists toocontinue processing other filters or invoke `finalCallback` otherwise, tooend hello service invocation.</span></span>

<span data-ttu-id="4db96-138">Två filter som implementerar logik som medföljer hello Azure SDK för Node.js, **ExponentialRetryPolicyFilter** och **LinearRetryPolicyFilter**.</span><span class="sxs-lookup"><span data-stu-id="4db96-138">Two filters that implement retry logic are included with hello Azure SDK for Node.js, **ExponentialRetryPolicyFilter** and **LinearRetryPolicyFilter**.</span></span> <span data-ttu-id="4db96-139">hello följande skapar en **ServiceBusService** objekt som använder hello **ExponentialRetryPolicyFilter**:</span><span class="sxs-lookup"><span data-stu-id="4db96-139">hello following creates a **ServiceBusService** object that uses hello **ExponentialRetryPolicyFilter**:</span></span>

```javascript
var retryOperations = new azure.ExponentialRetryPolicyFilter();
var serviceBusService = azure.createServiceBusService().withFilter(retryOperations);
```

## <a name="create-subscriptions"></a><span data-ttu-id="4db96-140">Skapa prenumerationer</span><span class="sxs-lookup"><span data-stu-id="4db96-140">Create subscriptions</span></span>
<span data-ttu-id="4db96-141">Ämnesprenumerationer skapas också med hello **ServiceBusService** objekt.</span><span class="sxs-lookup"><span data-stu-id="4db96-141">Topic subscriptions are also created with hello **ServiceBusService** object.</span></span> <span data-ttu-id="4db96-142">Prenumerationer är namngivna och kan ha ett valfritt filter som begränsar hello uppsättning meddelanden som har levererats toohello prenumerationens virtuella kö.</span><span class="sxs-lookup"><span data-stu-id="4db96-142">Subscriptions are named and can have an optional filter that restricts hello set of messages delivered toohello subscription's virtual queue.</span></span>

> [!NOTE]
> <span data-ttu-id="4db96-143">Prenumerationer är beständiga och fortsätter tooexist förrän antingen de eller hello avsnittet de är kopplade till, tas bort.</span><span class="sxs-lookup"><span data-stu-id="4db96-143">Subscriptions are persistent and will continue tooexist until either they, or hello topic they are associated with, are deleted.</span></span> <span data-ttu-id="4db96-144">Om ditt program innehåller logik toocreate en prenumeration, det först ska kontrollera om det finns redan hello-prenumeration med hjälp av den `getSubscription` metoden.</span><span class="sxs-lookup"><span data-stu-id="4db96-144">If your application contains logic toocreate a subscription, it should first check if hello subscription already exists by using the `getSubscription` method.</span></span>
>
>

### <a name="create-a-subscription-with-hello-default-matchall-filter"></a><span data-ttu-id="4db96-145">Skapa en prenumeration med hello standardfiltret (MatchAll)</span><span class="sxs-lookup"><span data-stu-id="4db96-145">Create a subscription with hello default (MatchAll) filter</span></span>
<span data-ttu-id="4db96-146">Hej **MatchAll** filtret är hello standardfilter som används om inget filter anges när en ny prenumeration skapas.</span><span class="sxs-lookup"><span data-stu-id="4db96-146">hello **MatchAll** filter is hello default filter that is used if no filter is specified when a new subscription is created.</span></span> <span data-ttu-id="4db96-147">När hello **MatchAll** filter används, alla meddelanden publicerade toohello avsnittet placeras i prenumerationens virtuella kö.</span><span class="sxs-lookup"><span data-stu-id="4db96-147">When hello **MatchAll** filter is used, all messages published toohello topic are placed in the subscription's virtual queue.</span></span> <span data-ttu-id="4db96-148">hello följande exempel skapas en prenumeration med namnet ”AllMessages” och använder hello standard **MatchAll** filter.</span><span class="sxs-lookup"><span data-stu-id="4db96-148">hello following example creates a subscription named 'AllMessages' and uses hello default **MatchAll** filter.</span></span>

```javascript
serviceBusService.createSubscription('MyTopic','AllMessages',function(error){
    if(!error){
        // subscription created
    }
});
```

### <a name="create-subscriptions-with-filters"></a><span data-ttu-id="4db96-149">Skapa prenumerationer med filter</span><span class="sxs-lookup"><span data-stu-id="4db96-149">Create subscriptions with filters</span></span>
<span data-ttu-id="4db96-150">Du kan även skapa filter som gör att du tooscope vilka meddelanden som skickas tooa avsnittet visas inom en viss ämnesprenumeration.</span><span class="sxs-lookup"><span data-stu-id="4db96-150">You can also create filters that allow you tooscope which messages sent tooa topic should show up within a specific topic subscription.</span></span>

<span data-ttu-id="4db96-151">hello mest flexibla typen av filter som stöds av prenumerationerna är den **SqlFilter**, som implementerar en deluppsättning av SQL92.</span><span class="sxs-lookup"><span data-stu-id="4db96-151">hello most flexible type of filter supported by subscriptions is the **SqlFilter**, which implements a subset of SQL92.</span></span> <span data-ttu-id="4db96-152">SQL-filter tillämpas för hälsningsmeddelande som är publicerade toohello avsnittet hello egenskaper.</span><span class="sxs-lookup"><span data-stu-id="4db96-152">SQL filters operate on hello properties of hello messages that are published toohello topic.</span></span> <span data-ttu-id="4db96-153">Mer information om hello-uttryck som kan användas med ett SQL-filter granskar hello [SqlFilter.SqlExpression] [ SqlFilter.SqlExpression] syntax.</span><span class="sxs-lookup"><span data-stu-id="4db96-153">For more details about hello expressions that can be used with a SQL filter, review hello [SqlFilter.SqlExpression][SqlFilter.SqlExpression] syntax.</span></span>

<span data-ttu-id="4db96-154">Filter kan du lägga till tooa prenumeration hello `createRule` metod för hello **ServiceBusService** objekt.</span><span class="sxs-lookup"><span data-stu-id="4db96-154">Filters can be added tooa subscription by using hello `createRule` method of hello **ServiceBusService** object.</span></span> <span data-ttu-id="4db96-155">Den här metoden kan du lägga till nya filter tooan befintlig prenumeration.</span><span class="sxs-lookup"><span data-stu-id="4db96-155">This method allows you to add new filters tooan existing subscription.</span></span>

> [!NOTE]
> <span data-ttu-id="4db96-156">Eftersom hello standardfiltret används automatiskt tooall nya prenumerationer, måste du först ta bort standardfiltret hello eller **MatchAll** åsidosätter eventuella filter som du kan ange.</span><span class="sxs-lookup"><span data-stu-id="4db96-156">Because hello default filter is applied automatically tooall new subscriptions, you must first remove hello default filter or the **MatchAll** will override any other filters you may specify.</span></span> <span data-ttu-id="4db96-157">Du kan ta bort hello standardregel med hello `deleteRule` metod för den **ServiceBusService** objekt.</span><span class="sxs-lookup"><span data-stu-id="4db96-157">You can remove hello default rule by using hello `deleteRule` method of the **ServiceBusService** object.</span></span>
>
>

<span data-ttu-id="4db96-158">hello följande exempel skapas en prenumeration med namnet `HighMessages` med en **SqlFilter** som endast väljer meddelanden som har en anpassad `messagenumber` egenskap som är större än 3:</span><span class="sxs-lookup"><span data-stu-id="4db96-158">hello following example creates a subscription named `HighMessages` with a **SqlFilter** that only selects messages that have a custom `messagenumber` property greater than 3:</span></span>

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

<span data-ttu-id="4db96-159">På liknande sätt hello följande exempel skapas en prenumeration med namnet `LowMessages` med en **SqlFilter** som endast väljer meddelanden som har en `messagenumber` egenskap som är mindre än eller lika med too3:</span><span class="sxs-lookup"><span data-stu-id="4db96-159">Similarly, hello following example creates a subscription named `LowMessages` with a **SqlFilter** that only selects messages that have a `messagenumber` property less than or equal too3:</span></span>

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

<span data-ttu-id="4db96-160">När ett meddelande skickas nu för`MyTopic`, kommer alltid levereras till mottagare som prenumererar på toohello `AllMessages` avsnittet prenumeration och levereras selektivt tooreceivers prenumererar toohello `HighMessages` och `LowMessages` ämnesprenumerationer (beroende på hello meddelandeinnehåll).</span><span class="sxs-lookup"><span data-stu-id="4db96-160">When a message is now sent too`MyTopic`, it will always be delivered to receivers subscribed toohello `AllMessages` topic subscription, and selectively delivered tooreceivers subscribed toohello `HighMessages` and `LowMessages` topic subscriptions (depending upon hello message content).</span></span>

## <a name="how-toosend-messages-tooa-topic"></a><span data-ttu-id="4db96-161">Hur toosend meddelanden tooa avsnittet</span><span class="sxs-lookup"><span data-stu-id="4db96-161">How toosend messages tooa topic</span></span>
<span data-ttu-id="4db96-162">toosend meddelandet tooa Service Bus-ämne programmet måste använda den `sendTopicMessage` metod för hello **ServiceBusService** objekt.</span><span class="sxs-lookup"><span data-stu-id="4db96-162">toosend a message tooa Service Bus topic, your application must use the `sendTopicMessage` method of hello **ServiceBusService** object.</span></span>
<span data-ttu-id="4db96-163">Meddelanden som skickas tooService Bus-ämnen är **BrokeredMessage** objekt.</span><span class="sxs-lookup"><span data-stu-id="4db96-163">Messages sent tooService Bus topics are **BrokeredMessage** objects.</span></span>
<span data-ttu-id="4db96-164">**BrokeredMessage** objekt har en uppsättning standardegenskaper (t.ex `Label` och `TimeToLive`), en ordlista som används toohold anpassade programspecifika egenskaper och en brödtext med strängdata.</span><span class="sxs-lookup"><span data-stu-id="4db96-164">**BrokeredMessage** objects have a set of standard properties (such as `Label` and `TimeToLive`), a dictionary that is used toohold custom application-specific properties, and a body of string data.</span></span> <span data-ttu-id="4db96-165">Ett program kan konfigurera hello brödtext hello-meddelande genom att skicka ett strängvärde till hello `sendTopicMessage` och alla obligatoriska standardegenskaper fylls i med standardvärden.</span><span class="sxs-lookup"><span data-stu-id="4db96-165">An application can set hello body of hello message by passing a string value to hello `sendTopicMessage` and any required standard properties will be populated by default values.</span></span>

<span data-ttu-id="4db96-166">hello exemplet nedan visar hur toosend fem testmeddelanden till `MyTopic`.</span><span class="sxs-lookup"><span data-stu-id="4db96-166">hello following example demonstrates how toosend five test messages to `MyTopic`.</span></span> <span data-ttu-id="4db96-167">Observera att hello `messagenumber` -värdet för varje meddelande varierar på hello upprepning av loopen hello (detta avgör vilka prenumerationer som får det):</span><span class="sxs-lookup"><span data-stu-id="4db96-167">Note that hello `messagenumber` property value of each message varies on hello iteration of hello loop (this will determine which subscriptions receive it):</span></span>

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

<span data-ttu-id="4db96-168">Service Bus-ämnena stöder en maximal meddelandestorlek på 256 KB i hello [standardnivån](service-bus-premium-messaging.md) och 1 MB på hello [premiumnivån](service-bus-premium-messaging.md).</span><span class="sxs-lookup"><span data-stu-id="4db96-168">Service Bus topics support a maximum message size of 256 KB in hello [Standard tier](service-bus-premium-messaging.md) and 1 MB in hello [Premium tier](service-bus-premium-messaging.md).</span></span> <span data-ttu-id="4db96-169">hello-huvudet som innehåller hello standard- och anpassade programegenskaperna, kan ha en maximal storlek på 64 KB.</span><span class="sxs-lookup"><span data-stu-id="4db96-169">hello header, which includes hello standard and custom application properties, can have a maximum size of 64 KB.</span></span> <span data-ttu-id="4db96-170">Det finns ingen gräns hello antalet meddelanden som lagras i ett ämne men det finns ett tak på hello totala storleken på hälsningsmeddelande som innehas av ett ämne.</span><span class="sxs-lookup"><span data-stu-id="4db96-170">There is no limit on hello number of messages held in a topic but there is a cap on hello total size of hello messages held by a topic.</span></span> <span data-ttu-id="4db96-171">Den här ämnesstorleken definieras när ämnet skapas, med en övre gräns på 5 GB.</span><span class="sxs-lookup"><span data-stu-id="4db96-171">This topic size is defined at creation time, with an upper limit of 5 GB.</span></span>

## <a name="receive-messages-from-a-subscription"></a><span data-ttu-id="4db96-172">Ta emot meddelanden från en prenumeration</span><span class="sxs-lookup"><span data-stu-id="4db96-172">Receive messages from a subscription</span></span>
<span data-ttu-id="4db96-173">Meddelanden tas emot från en prenumeration med hjälp av den `receiveSubscriptionMessage` metod på hello **ServiceBusService** objekt.</span><span class="sxs-lookup"><span data-stu-id="4db96-173">Messages are received from a subscription using the `receiveSubscriptionMessage` method on hello **ServiceBusService** object.</span></span> <span data-ttu-id="4db96-174">Som standard tas meddelanden bort från hello prenumeration som de läses; men du kan läsa (titt) och låsa hello-meddelande utan att ta bort den från hello prenumeration genom att ange hello valfri parameter `isPeekLock` för**SANT**.</span><span class="sxs-lookup"><span data-stu-id="4db96-174">By default, messages are deleted from hello subscription as they are read; however, you can read (peek) and lock hello message without deleting it from hello subscription by setting hello optional parameter `isPeekLock` too**true**.</span></span>

<span data-ttu-id="4db96-175">hello standardbeteendet för läsning och ta bort hello-meddelande som en del av receive-åtgärden är hello enklaste modellen och fungerar bäst för scenarier där ett program kan tolerera icke-bearbetning av ett meddelande i hello händelse av fel.</span><span class="sxs-lookup"><span data-stu-id="4db96-175">hello default behavior of reading and deleting hello message as part of the receive operation is hello simplest model, and works best for scenarios in which an application can tolerate not processing a message in hello event of a failure.</span></span> <span data-ttu-id="4db96-176">toounderstand, Överväg ett scenario där konsumenten problem hello mottagning av begäran och sedan kraschar innan bearbetningen.</span><span class="sxs-lookup"><span data-stu-id="4db96-176">toounderstand this, consider a scenario in which the consumer issues hello receive request and then crashes before processing it.</span></span> <span data-ttu-id="4db96-177">Eftersom Service Bus kommer att ha markerat hello meddelandet som Förbrukat, sedan när hello programmet startas om och börjar förbruka meddelanden igen, att ha missat hello-meddelande som har förbrukats tidigare toohello krascher.</span><span class="sxs-lookup"><span data-stu-id="4db96-177">Because Service Bus will have marked hello message as being consumed, then when hello application restarts and begins consuming messages again, it will have missed hello message that was consumed prior toohello crash.</span></span>

<span data-ttu-id="4db96-178">Om hello `isPeekLock` parameter har angetts för**SANT**, hello får blir en åtgärd i två steg, vilket gör det möjligt toosupport program som inte tolererar att ett meddelande saknas.</span><span class="sxs-lookup"><span data-stu-id="4db96-178">If hello `isPeekLock` parameter is set too**true**, hello receive becomes a two stage operation, which makes it possible toosupport applications that cannot tolerate missing messages.</span></span> <span data-ttu-id="4db96-179">När Service Bus tar emot en begäran, hittar hello nästa meddelande toobe förbrukas, låser det tooprevent andra användare tar emot det och returnerar sedan toohello program.</span><span class="sxs-lookup"><span data-stu-id="4db96-179">When Service Bus receives a request, it finds hello next message toobe consumed, locks it tooprevent other consumers receiving it, and then returns it toohello application.</span></span>
<span data-ttu-id="4db96-180">När hello programmet har avslutat bearbetningen hello-meddelande (eller lagrar det på ett tillförlitligt sätt för framtida bearbetning), Slutför hello andra steget i processen genom att anropa **deleteMessage** metod och ge meddelandet toobe ta bort som en parameter.</span><span class="sxs-lookup"><span data-stu-id="4db96-180">After hello application finishes processing hello message (or stores it reliably for future processing), it completes hello second stage of the receive process by calling **deleteMessage** method and providing the message toobe deleted as a parameter.</span></span> <span data-ttu-id="4db96-181">Hej **deleteMessage** metoden kommer Markera hello meddelandet som Förbrukat och ta bort den från hello prenumeration.</span><span class="sxs-lookup"><span data-stu-id="4db96-181">hello **deleteMessage** method will mark hello message as being consumed and remove it from hello subscription.</span></span>

<span data-ttu-id="4db96-182">hello exemplet nedan visar hur meddelanden kan tas emot och bearbetat använder `receiveSubscriptionMessage`.</span><span class="sxs-lookup"><span data-stu-id="4db96-182">hello following example demonstrates how messages can be received and processed using `receiveSubscriptionMessage`.</span></span> <span data-ttu-id="4db96-183">hello exempel först tar emot och tar bort ett meddelande från hello 'Highmessages' prenumeration och får ett meddelande från hello 'HighMessages' en prenumeration med hjälp av `isPeekLock` ange tootrue.</span><span class="sxs-lookup"><span data-stu-id="4db96-183">hello example first receives and deletes a message from hello 'LowMessages' subscription, and then receives a message from hello 'HighMessages' subscription using `isPeekLock` set tootrue.</span></span> <span data-ttu-id="4db96-184">Hello-meddelande med raderas sedan `deleteMessage`:</span><span class="sxs-lookup"><span data-stu-id="4db96-184">It then deletes hello message using `deleteMessage`:</span></span>

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

## <a name="how-toohandle-application-crashes-and-unreadable-messages"></a><span data-ttu-id="4db96-185">Hur toohandle programmet kraschar och oläsbara meddelanden</span><span class="sxs-lookup"><span data-stu-id="4db96-185">How toohandle application crashes and unreadable messages</span></span>
<span data-ttu-id="4db96-186">Service Bus innehåller funktioner toohelp du att återställa fel i programmet eller problem med bearbetning av ett meddelande.</span><span class="sxs-lookup"><span data-stu-id="4db96-186">Service Bus provides functionality toohelp you gracefully recover from errors in your application or difficulties processing a message.</span></span> <span data-ttu-id="4db96-187">Om ett mottagarprogram inte kan tooprocess hello meddelande av någon anledning, så kan det anropa hello `unlockMessage` -metoden i den **ServiceBusService** objekt.</span><span class="sxs-lookup"><span data-stu-id="4db96-187">If a receiver application is unable tooprocess hello message for some reason, then it can call hello `unlockMessage` method on the **ServiceBusService** object.</span></span> <span data-ttu-id="4db96-188">Detta orsakar Service Bus toounlock meddelandet inom hello prenumerationen och göra den tillgänglig toobe tas emot igen, antingen hello av samma förbrukning av ett program eller ett annat användningsprogram.</span><span class="sxs-lookup"><span data-stu-id="4db96-188">This will cause Service Bus toounlock the message within hello subscription and make it available toobe received again, either by hello same consuming application or by another consuming application.</span></span>

<span data-ttu-id="4db96-189">Det finns också en tidsgräns som är associerad med ett meddelande som ligger låst i prenumerationen och om hello program inte tooprocess hello-meddelande innan timeout för lås hello går ut (till exempel om hello programmet kraschar), kommer Service Bus låser upp hello-meddelande automatiskt och gör den tillgänglig toobe tas emot igen.</span><span class="sxs-lookup"><span data-stu-id="4db96-189">There is also a timeout associated with a message locked within the subscription, and if hello application fails tooprocess hello message before hello lock timeout expires (for example, if hello application crashes), then Service Bus unlocks hello message automatically and makes it available toobe received again.</span></span>

<span data-ttu-id="4db96-190">Hello händelse som hello programmet kraschar efter hello meddelandet har bearbetats men innan hello `deleteMessage` metoden anropas sedan hello-meddelande kommer att levereras på nytt toohello program när den startas om.</span><span class="sxs-lookup"><span data-stu-id="4db96-190">In hello event that hello application crashes after processing hello message but before hello `deleteMessage` method is called, then hello message will be redelivered toohello application when it restarts.</span></span> <span data-ttu-id="4db96-191">Det här kallas ofta *At Least Once Processing*, det vill säga, varje meddelande bearbetas minst en gång men i vissa situationer hello samma meddelande levereras.</span><span class="sxs-lookup"><span data-stu-id="4db96-191">This is often called *At Least Once Processing*, that is, each message will be processed at least once but in certain situations hello same message may be redelivered.</span></span> <span data-ttu-id="4db96-192">Om hello scenariot inte tolererar duplicerad bearbetning, sedan bör programutvecklarna lägga till ytterligare logik tootheir duplicerad meddelandeleverans för programmet toohandle.</span><span class="sxs-lookup"><span data-stu-id="4db96-192">If hello scenario cannot tolerate duplicate processing, then application developers should add additional logic tootheir application toohandle duplicate message delivery.</span></span> <span data-ttu-id="4db96-193">Detta uppnås ofta med hjälp av den **MessageId** -egenskapen för hello-meddelande, förblir konstant under alla leveransförsök.</span><span class="sxs-lookup"><span data-stu-id="4db96-193">This is often achieved using the **MessageId** property of hello message, which will remain constant across delivery attempts.</span></span>

## <a name="delete-topics-and-subscriptions"></a><span data-ttu-id="4db96-194">Ta bort ämnen och prenumerationer</span><span class="sxs-lookup"><span data-stu-id="4db96-194">Delete topics and subscriptions</span></span>
<span data-ttu-id="4db96-195">Ämnen och prenumerationer är beständiga och måste vara explicit bort antingen via hello [Azure-portalen] [ Azure portal] eller programmässigt.</span><span class="sxs-lookup"><span data-stu-id="4db96-195">Topics and subscriptions are persistent, and must be explicitly deleted either through hello [Azure portal][Azure portal] or programmatically.</span></span>
<span data-ttu-id="4db96-196">hello exemplet nedan visar hur toodelete hello avsnittet med namnet `MyTopic`:</span><span class="sxs-lookup"><span data-stu-id="4db96-196">hello following example demonstrates how toodelete hello topic named `MyTopic`:</span></span>

```javascript
serviceBusService.deleteTopic('MyTopic', function (error) {
    if (error) {
        console.log(error);
    }
});
```

<span data-ttu-id="4db96-197">Om du tar bort ett ämne tas även bort alla prenumerationer som är registrerade med hello-avsnittet.</span><span class="sxs-lookup"><span data-stu-id="4db96-197">Deleting a topic will also delete any subscriptions that are registered with hello topic.</span></span> <span data-ttu-id="4db96-198">Prenumerationer kan även tas bort separat.</span><span class="sxs-lookup"><span data-stu-id="4db96-198">Subscriptions can also be deleted independently.</span></span> <span data-ttu-id="4db96-199">I följande exempel visas hur toodelete en prenumeration med namnet `HighMessages` från hello `MyTopic` avsnittet:</span><span class="sxs-lookup"><span data-stu-id="4db96-199">The following example shows how toodelete a subscription named `HighMessages` from hello `MyTopic` topic:</span></span>

```javascript
serviceBusService.deleteSubscription('MyTopic', 'HighMessages', function (error) {
    if(error) {
        console.log(error);
    }
});
```

## <a name="next-steps"></a><span data-ttu-id="4db96-200">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="4db96-200">Next Steps</span></span>
<span data-ttu-id="4db96-201">Nu när du har lärt dig grunderna hello i Service Bus-ämnen, följa dessa länkar toolearn mer.</span><span class="sxs-lookup"><span data-stu-id="4db96-201">Now that you've learned hello basics of Service Bus topics, follow these links toolearn more.</span></span>

* <span data-ttu-id="4db96-202">Se [köer, ämnen och prenumerationer][Queues, topics, and subscriptions].</span><span class="sxs-lookup"><span data-stu-id="4db96-202">See [Queues, topics, and subscriptions][Queues, topics, and subscriptions].</span></span>
* <span data-ttu-id="4db96-203">API-referens för [SqlFilter][SqlFilter].</span><span class="sxs-lookup"><span data-stu-id="4db96-203">API reference for [SqlFilter][SqlFilter].</span></span>
* <span data-ttu-id="4db96-204">Besök hello [Azure SDK för noden] [ Azure SDK for Node] databasen på GitHub.</span><span class="sxs-lookup"><span data-stu-id="4db96-204">Visit hello [Azure SDK for Node][Azure SDK for Node] repository on GitHub.</span></span>

[Azure SDK for Node]: https://github.com/Azure/azure-sdk-for-node
[Azure portal]: https://portal.azure.com
[SqlFilter.SqlExpression]: service-bus-messaging-sql-filter.md
[Queues, topics, and subscriptions]: service-bus-queues-topics-subscriptions.md
[SqlFilter]: /dotnet/api/microsoft.servicebus.messaging.sqlfilter
[Node.js Cloud Service]: ../cloud-services/cloud-services-nodejs-develop-deploy-app.md
[skapa och distribuera en Node.js-programmet tooan webbplatsen Azure]: ../app-service-web/app-service-web-get-started-nodejs.md
[Node.js Cloud Service with Storage]: ../cloud-services/cloud-services-nodejs-develop-deploy-app.md
[Node.js Web Application with Storage]:../cosmos-db/table-storage-cloud-service-nodejs.md
