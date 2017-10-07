---
title: "aaaHow toouse Azure Service Bus-ämnen med Java | Microsoft Docs"
description: "Använda Service Bus-ämnen och prenumerationer i Azure."
services: service-bus-messaging
documentationcenter: java
author: sethmanheim
manager: timlt
editor: 
ms.assetid: 63d6c8bd-8a22-4292-befc-545ffb52e8eb
ms.service: service-bus-messaging
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: Java
ms.topic: article
ms.date: 06/28/2017
ms.author: sethm
ms.openlocfilehash: 1aad16fdb5d68a5782b85c8dfda9d695babd57ff
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="how-toouse-service-bus-topics-and-subscriptions-with-java"></a><span data-ttu-id="6e52b-103">Hur toouse Service Bus-ämnen och prenumerationer med Java</span><span class="sxs-lookup"><span data-stu-id="6e52b-103">How toouse Service Bus topics and subscriptions with Java</span></span>

[!INCLUDE [service-bus-selector-topics](../../includes/service-bus-selector-topics.md)]

<span data-ttu-id="6e52b-104">Den här guiden beskriver hur toouse Service Bus-ämnen och prenumerationer.</span><span class="sxs-lookup"><span data-stu-id="6e52b-104">This guide describes how toouse Service Bus topics and subscriptions.</span></span> <span data-ttu-id="6e52b-105">hello exemplen är skrivna i Java och använder hello [Azure SDK för Java][Azure SDK for Java].</span><span class="sxs-lookup"><span data-stu-id="6e52b-105">hello samples are written in Java and use hello [Azure SDK for Java][Azure SDK for Java].</span></span> <span data-ttu-id="6e52b-106">hello beskrivs scenarier där **skapa ämnen och prenumerationer**, **skapa prenumerationsfilter**, **skickar meddelanden tooa avsnittet**, **tar emot meddelanden från en prenumeration**, och **ta bort ämnen och prenumerationer**.</span><span class="sxs-lookup"><span data-stu-id="6e52b-106">hello scenarios covered include **creating topics and subscriptions**, **creating subscription filters**, **sending messages tooa topic**, **receiving messages from a subscription**, and **deleting topics and subscriptions**.</span></span>

## <a name="what-are-service-bus-topics-and-subscriptions"></a><span data-ttu-id="6e52b-107">Vad är Service Bus-ämnen och -prenumerationer?</span><span class="sxs-lookup"><span data-stu-id="6e52b-107">What are Service Bus topics and subscriptions?</span></span>
<span data-ttu-id="6e52b-108">Service Bus-ämnen och -prenumerationer stöder en *publicera/prenumerera*-modell för meddelandekommunikation.</span><span class="sxs-lookup"><span data-stu-id="6e52b-108">Service Bus topics and subscriptions support a *publish/subscribe* messaging communication model.</span></span> <span data-ttu-id="6e52b-109">När du använder ämnen och prenumerationer så kommunicerar inte komponenterna i ett distribuerat program direkt med varandra. Istället så utbyter de meddelanden via ett ämne, som agerar mellanhand.</span><span class="sxs-lookup"><span data-stu-id="6e52b-109">When using topics and subscriptions, components of a distributed application do not communicate directly with each other; instead they exchange messages via a topic, which acts as an intermediary.</span></span>

![TopicConcepts](./media/service-bus-java-how-to-use-topics-subscriptions/sb-topics-01.png)

<span data-ttu-id="6e52b-111">Till skillnad från Service Bus-köer, där varje meddelande bearbetas av en enskild konsument så ger ämnen och prenumerationer en ”ett till många”-kommunikation med hjälp av ett publicera/prenumerera-mönster.</span><span class="sxs-lookup"><span data-stu-id="6e52b-111">In contrast with Service Bus queues, in which each message is processed by a single consumer, topics and subscriptions provide a "one-to-many" form of communication, using a publish/subscribe pattern.</span></span> <span data-ttu-id="6e52b-112">Det är möjligt att registrera flera prenumerationer tooa avsnittet.</span><span class="sxs-lookup"><span data-stu-id="6e52b-112">It is possible to register multiple subscriptions tooa topic.</span></span> <span data-ttu-id="6e52b-113">När ett meddelande skickas tooa avsnittet, görs sedan tillgänglig tooeach prenumeration toohandle/bearbeta oberoende av varandra.</span><span class="sxs-lookup"><span data-stu-id="6e52b-113">When a message is sent tooa topic, it is then made available tooeach subscription toohandle/process independently.</span></span>

<span data-ttu-id="6e52b-114">Ett prenumeration tooa ämne liknar en virtuell kö som tar emot kopior av hello-meddelanden som skickades toohello avsnittet.</span><span class="sxs-lookup"><span data-stu-id="6e52b-114">A subscription tooa topic resembles a virtual queue that receives copies of hello messages that were sent toohello topic.</span></span> <span data-ttu-id="6e52b-115">Alternativt kan du registrera filterregler för ett ämne på grundval av per prenumeration, vilket gör att du toofilter/begränsa vilka meddelanden tooa avsnittet tas emot av vilka ämnesprenumerationer.</span><span class="sxs-lookup"><span data-stu-id="6e52b-115">You can optionally register filter rules for a topic on a per-subscription basis, which allows you toofilter/restrict which messages tooa topic are received by which topic subscriptions.</span></span>

<span data-ttu-id="6e52b-116">Service Bus-ämnen och prenumerationer gör att du tooscale tooprocess ett mycket stort antal meddelanden i ett mycket stort antal användare och program.</span><span class="sxs-lookup"><span data-stu-id="6e52b-116">Service Bus topics and subscriptions enable you tooscale tooprocess a very large number of messages across a very large number of users and applications.</span></span>

## <a name="create-a-service-namespace"></a><span data-ttu-id="6e52b-117">Skapa ett namnområde för tjänsten</span><span class="sxs-lookup"><span data-stu-id="6e52b-117">Create a service namespace</span></span>
<span data-ttu-id="6e52b-118">toobegin som använder Service Bus-ämnen och prenumerationer i Azure, måste du först skapa ett namnområde som innehåller en omfattningsbehållare för adressering av Service Bus-resurser i ditt program.</span><span class="sxs-lookup"><span data-stu-id="6e52b-118">toobegin using Service Bus topics and subscriptions in Azure, you must first create a namespace, which provides a scoping container for addressing Service Bus resources within your application.</span></span>

<span data-ttu-id="6e52b-119">toocreate ett namnområde:</span><span class="sxs-lookup"><span data-stu-id="6e52b-119">toocreate a namespace:</span></span>

[!INCLUDE [service-bus-create-namespace-portal](../../includes/service-bus-create-namespace-portal.md)]

## <a name="configure-your-application-toouse-service-bus"></a><span data-ttu-id="6e52b-120">Konfigurera ditt program toouse Service Bus</span><span class="sxs-lookup"><span data-stu-id="6e52b-120">Configure your application toouse Service Bus</span></span>
<span data-ttu-id="6e52b-121">Kontrollera att du har installerat hello [Azure SDK för Java] [ Azure SDK for Java] innan du skapar det här exemplet.</span><span class="sxs-lookup"><span data-stu-id="6e52b-121">Make sure you have installed hello [Azure SDK for Java][Azure SDK for Java] before building this sample.</span></span> <span data-ttu-id="6e52b-122">Om du använder Eclipse, kan du installera hello [Azure Toolkit för Eclipse] [ Azure Toolkit for Eclipse] som innehåller hello Azure SDK för Java.</span><span class="sxs-lookup"><span data-stu-id="6e52b-122">If you are using Eclipse, you can install hello [Azure Toolkit for Eclipse][Azure Toolkit for Eclipse] that includes hello Azure SDK for Java.</span></span> <span data-ttu-id="6e52b-123">Du kan sedan lägga till hello **Microsoft Azure-biblioteken för Java** tooyour projektet:</span><span class="sxs-lookup"><span data-stu-id="6e52b-123">You can then add hello **Microsoft Azure Libraries for Java** tooyour project:</span></span>

![](media/service-bus-java-how-to-use-topics-subscriptions/eclipselibs.png)

<span data-ttu-id="6e52b-124">Lägg till följande hello `import` instruktioner toohello överkant hello Java-fil:</span><span class="sxs-lookup"><span data-stu-id="6e52b-124">Add hello following `import` statements toohello top of hello Java file:</span></span>

```java
import com.microsoft.windowsazure.services.servicebus.*;
import com.microsoft.windowsazure.services.servicebus.models.*;
import com.microsoft.windowsazure.core.*;
import javax.xml.datatype.*;
```

<span data-ttu-id="6e52b-125">Lägg till hello Azure-biblioteken för Java tooyour byggsökväg och inkludera den i projektet distribution för sammansättningen.</span><span class="sxs-lookup"><span data-stu-id="6e52b-125">Add hello Azure Libraries for Java tooyour build path and include it in your project deployment assembly.</span></span>

## <a name="create-a-topic"></a><span data-ttu-id="6e52b-126">Skapa ett ämne</span><span class="sxs-lookup"><span data-stu-id="6e52b-126">Create a topic</span></span>
<span data-ttu-id="6e52b-127">Hanteringsåtgärder för Service Bus-ämnen kan utföras via den **ServiceBusContract** klass.</span><span class="sxs-lookup"><span data-stu-id="6e52b-127">Management operations for Service Bus topics can be performed via the **ServiceBusContract** class.</span></span> <span data-ttu-id="6e52b-128">En **ServiceBusContract** objektet konstrueras med en lämplig konfiguration som innehåller SAS-token med behörigheter toomanage och hello **ServiceBusContract** klassen är hello enda punkt kommunikation med Azure.</span><span class="sxs-lookup"><span data-stu-id="6e52b-128">A **ServiceBusContract** object is constructed with an appropriate configuration that encapsulates the SAS token with permissions toomanage it, and hello **ServiceBusContract** class is hello sole point of communication with Azure.</span></span>

<span data-ttu-id="6e52b-129">Hej **ServiceBusService** klassen innehåller metoder toocreate, räkna upp och ta bort ämnen.</span><span class="sxs-lookup"><span data-stu-id="6e52b-129">hello **ServiceBusService** class provides methods toocreate, enumerate, and delete topics.</span></span> <span data-ttu-id="6e52b-130">hello som följande exempel visar hur en **ServiceBusService** objekt kan vara används toocreate ett ämne med namnet `TestTopic`, med ett namnområde som kallas `HowToSample`:</span><span class="sxs-lookup"><span data-stu-id="6e52b-130">hello following example shows how a **ServiceBusService** object can be used toocreate a topic named `TestTopic`, with a namespace called `HowToSample`:</span></span>

```java
Configuration config =
    ServiceBusConfiguration.configureWithSASAuthentication(
      "HowToSample",
      "RootManageSharedAccessKey",
      "SAS_key_value",
      ".servicebus.windows.net"
      );

ServiceBusContract service = ServiceBusService.create(config);
TopicInfo topicInfo = new TopicInfo("TestTopic");
try  
{
    CreateTopicResult result = service.createTopic(topicInfo);
}
catch (ServiceException e) {
    System.out.print("ServiceException encountered: ");
    System.out.println(e.getMessage());
    System.exit(-1);
}
```

<span data-ttu-id="6e52b-131">Det finns sätt på **TopicInfo** som gör att egenskaper för hello avsnittet anges (till exempel: tooset hello standard time to live (TTL) värdet toobe tillämpas toomessages skickas toohello avsnittet).</span><span class="sxs-lookup"><span data-stu-id="6e52b-131">There are methods on **TopicInfo** that enable properties of hello topic to be set (for example: tooset hello default time-to-live (TTL) value toobe applied toomessages sent toohello topic).</span></span> <span data-ttu-id="6e52b-132">hello följande exempel visas hur toocreate ett ämne med namnet `TestTopic` med en maximal storlek på 5 GB:</span><span class="sxs-lookup"><span data-stu-id="6e52b-132">hello following example shows how toocreate a topic named `TestTopic` with a maximum size of 5 GB:</span></span>

```java
long maxSizeInMegabytes = 5120;  
TopicInfo topicInfo = new TopicInfo("TestTopic");  
topicInfo.setMaxSizeInMegabytes(maxSizeInMegabytes);
CreateTopicResult result = service.createTopic(topicInfo);
```

<span data-ttu-id="6e52b-133">Observera att du kan använda hello **listTopics** metod på **ServiceBusContract** objekt toocheck om ett ämne med ett angivet namn finns redan i ett namnområde för tjänsten.</span><span class="sxs-lookup"><span data-stu-id="6e52b-133">Note that you can use hello **listTopics** method on **ServiceBusContract** objects toocheck if a topic with a specified name already exists within a service namespace.</span></span>

## <a name="create-subscriptions"></a><span data-ttu-id="6e52b-134">Skapa prenumerationer</span><span class="sxs-lookup"><span data-stu-id="6e52b-134">Create subscriptions</span></span>
<span data-ttu-id="6e52b-135">Prenumerationer tootopics skapas också med hello **ServiceBusService** klass.</span><span class="sxs-lookup"><span data-stu-id="6e52b-135">Subscriptions tootopics are also created with hello **ServiceBusService** class.</span></span> <span data-ttu-id="6e52b-136">Prenumerationer är namngivna och kan ha ett valfritt filter som begränsar hello uppsättning av meddelanden som skickas toohello prenumerationens virtuella kö.</span><span class="sxs-lookup"><span data-stu-id="6e52b-136">Subscriptions are named and can have an optional filter that restricts hello set of messages passed toohello subscription's virtual queue.</span></span>

### <a name="create-a-subscription-with-hello-default-matchall-filter"></a><span data-ttu-id="6e52b-137">Skapa en prenumeration med hello standardfiltret (MatchAll)</span><span class="sxs-lookup"><span data-stu-id="6e52b-137">Create a subscription with hello default (MatchAll) filter</span></span>
<span data-ttu-id="6e52b-138">Hej **MatchAll** filtret är hello standardfilter som används om inget filter anges när en ny prenumeration skapas.</span><span class="sxs-lookup"><span data-stu-id="6e52b-138">hello **MatchAll** filter is hello default filter that is used if no filter is specified when a new subscription is created.</span></span> <span data-ttu-id="6e52b-139">När hello **MatchAll** filter används, alla meddelanden publicerade toohello avsnittet placeras i prenumerationens virtuella kö.</span><span class="sxs-lookup"><span data-stu-id="6e52b-139">When hello **MatchAll** filter is used, all messages published toohello topic are placed in the subscription's virtual queue.</span></span> <span data-ttu-id="6e52b-140">hello följande exempel skapas en prenumeration med namnet ”AllMessages” och använder hello standard **MatchAll** filter.</span><span class="sxs-lookup"><span data-stu-id="6e52b-140">hello following example creates a subscription named "AllMessages" and uses hello default **MatchAll** filter.</span></span>

```java
SubscriptionInfo subInfo = new SubscriptionInfo("AllMessages");
CreateSubscriptionResult result =
    service.createSubscription("TestTopic", subInfo);
```

### <a name="create-subscriptions-with-filters"></a><span data-ttu-id="6e52b-141">Skapa prenumerationer med filter</span><span class="sxs-lookup"><span data-stu-id="6e52b-141">Create subscriptions with filters</span></span>
<span data-ttu-id="6e52b-142">Du kan även skapa filter som gör att du tooscope vilka meddelanden som skickas tooa avsnittet visas inom en viss ämnesprenumeration.</span><span class="sxs-lookup"><span data-stu-id="6e52b-142">You can also create filters that enable you tooscope which messages sent tooa topic should show up within a specific topic subscription.</span></span>

<span data-ttu-id="6e52b-143">hello mest flexibla typen av filter som stöds av prenumerationerna är den [SqlFilter][SqlFilter], som implementerar en deluppsättning av SQL92.</span><span class="sxs-lookup"><span data-stu-id="6e52b-143">hello most flexible type of filter supported by subscriptions is the [SqlFilter][SqlFilter], which implements a subset of SQL92.</span></span> <span data-ttu-id="6e52b-144">SQL-filter tillämpas för hälsningsmeddelande som är publicerade toohello avsnittet hello egenskaper.</span><span class="sxs-lookup"><span data-stu-id="6e52b-144">SQL filters operate on hello properties of hello messages that are published toohello topic.</span></span> <span data-ttu-id="6e52b-145">Mer information om hello-uttryck som kan användas med ett SQL-filter granskar hello [SqlFilter.SqlExpression] [ SqlFilter.SqlExpression] syntax.</span><span class="sxs-lookup"><span data-stu-id="6e52b-145">For more details about hello expressions that can be used with a SQL filter, review hello [SqlFilter.SqlExpression][SqlFilter.SqlExpression] syntax.</span></span>

<span data-ttu-id="6e52b-146">hello följande exempel skapas en prenumeration med namnet `HighMessages` med en [SqlFilter] [ SqlFilter] objekt som endast väljer meddelanden som har en anpassad **MessageNumber** Egenskapen som är större än 3:</span><span class="sxs-lookup"><span data-stu-id="6e52b-146">hello following example creates a subscription named `HighMessages` with a [SqlFilter][SqlFilter] object that only selects messages that have a custom **MessageNumber** property greater than 3:</span></span>

```java
// Create a "HighMessages" filtered subscription  
SubscriptionInfo subInfo = new SubscriptionInfo("HighMessages");
CreateSubscriptionResult result = service.createSubscription("TestTopic", subInfo);
RuleInfo ruleInfo = new RuleInfo("myRuleGT3");
ruleInfo = ruleInfo.withSqlExpressionFilter("MessageNumber > 3");
CreateRuleResult ruleResult = service.createRule("TestTopic", "HighMessages", ruleInfo);
// Delete hello default rule, otherwise hello new rule won't be invoked.
service.deleteRule("TestTopic", "HighMessages", "$Default");
```

<span data-ttu-id="6e52b-147">På liknande sätt hello följande exempel skapas en prenumeration med namnet `LowMessages` med en [SqlFilter] [ SqlFilter] objekt som endast väljer meddelanden som har en **MessageNumber** Egenskapen som är mindre än eller lika med too3:</span><span class="sxs-lookup"><span data-stu-id="6e52b-147">Similarly, hello following example creates a subscription named `LowMessages` with a [SqlFilter][SqlFilter] object that only selects messages that have a **MessageNumber** property less than or equal too3:</span></span>

```java
// Create a "LowMessages" filtered subscription
SubscriptionInfo subInfo = new SubscriptionInfo("LowMessages");
CreateSubscriptionResult result = service.createSubscription("TestTopic", subInfo);
RuleInfo ruleInfo = new RuleInfo("myRuleLE3");
ruleInfo = ruleInfo.withSqlExpressionFilter("MessageNumber <= 3");
CreateRuleResult ruleResult = service.createRule("TestTopic", "LowMessages", ruleInfo);
// Delete hello default rule, otherwise hello new rule won't be invoked.
service.deleteRule("TestTopic", "LowMessages", "$Default");
```

<span data-ttu-id="6e52b-148">När ett meddelande skickas nu för`TestTopic`, det alltid levereras tooreceivers prenumererar toohello `AllMessages` prenumeration och levereras selektivt tooreceivers prenumererar toohello `HighMessages` och `LowMessages` prenumerationer ( beroende på innehållet i meddelandet).</span><span class="sxs-lookup"><span data-stu-id="6e52b-148">When a message is now sent too`TestTopic`, it will always be delivered tooreceivers subscribed toohello `AllMessages` subscription, and selectively delivered tooreceivers subscribed toohello `HighMessages` and `LowMessages` subscriptions (depending upon the message content).</span></span>

## <a name="send-messages-tooa-topic"></a><span data-ttu-id="6e52b-149">Skicka meddelanden tooa avsnittet</span><span class="sxs-lookup"><span data-stu-id="6e52b-149">Send messages tooa topic</span></span>
<span data-ttu-id="6e52b-150">toosend meddelandet tooa Service Bus-ämne programmet hämtar en **ServiceBusContract** objekt.</span><span class="sxs-lookup"><span data-stu-id="6e52b-150">toosend a message tooa Service Bus topic, your application obtains a **ServiceBusContract** object.</span></span> <span data-ttu-id="6e52b-151">hello följande kod visar hur toosend ett meddelande för hello `TestTopic` ämne som skapats tidigare inom hello `HowToSample` namnområde:</span><span class="sxs-lookup"><span data-stu-id="6e52b-151">hello following code demonstrates how toosend a message for hello `TestTopic` topic created previously within hello `HowToSample` namespace:</span></span>

```java
BrokeredMessage message = new BrokeredMessage("MyMessage");
service.sendTopicMessage("TestTopic", message);
```

<span data-ttu-id="6e52b-152">Meddelanden som skickas tooService Bus-ämnen är instanser av den [BrokeredMessage] [ BrokeredMessage] klass.</span><span class="sxs-lookup"><span data-stu-id="6e52b-152">Messages sent tooService Bus Topics are instances of the [BrokeredMessage][BrokeredMessage] class.</span></span> <span data-ttu-id="6e52b-153">[BrokeredMessage][BrokeredMessage]* objekt har en uppsättning standardmetoderna (exempelvis **setLabel** och **TimeToLive**), en ordlista som används toohold anpassade programspecifika egenskaper och en brödtext med godtyckliga programdata.</span><span class="sxs-lookup"><span data-stu-id="6e52b-153">[BrokeredMessage][BrokeredMessage]* objects have a set of standard methods (such as **setLabel** and **TimeToLive**), a dictionary that is used toohold custom application-specific properties, and a body of arbitrary application data.</span></span> <span data-ttu-id="6e52b-154">Ett program kan konfigurera hello brödtext genom att skicka någon typ av serialiserbara objekt till hello konstruktören för den [BrokeredMessage][BrokeredMessage], och hello lämpliga **DataContractSerializer**  kommer sedan att använda tooserialize hello-objekt.</span><span class="sxs-lookup"><span data-stu-id="6e52b-154">An application can set hello body of the message by passing any serializable object into hello constructor of the [BrokeredMessage][BrokeredMessage], and hello appropriate **DataContractSerializer** will then be used tooserialize hello object.</span></span> <span data-ttu-id="6e52b-155">Du kan också en **java.io.InputStream** kan anges.</span><span class="sxs-lookup"><span data-stu-id="6e52b-155">Alternatively, a **java.io.InputStream** can be provided.</span></span>

<span data-ttu-id="6e52b-156">hello exemplet nedan visar hur toosend fem test meddelanden toothe `TestTopic` **MessageSender** vi fick i hello kodstycke ovan.</span><span class="sxs-lookup"><span data-stu-id="6e52b-156">hello following example demonstrates how toosend five test messages toothe `TestTopic` **MessageSender** we obtained in hello code snippet above.</span></span>
<span data-ttu-id="6e52b-157">Observera hur hello **MessageNumber** -värdet för varje meddelande varierar på hello upprepning av loopen hello (detta avgör vilka prenumerationer som får det):</span><span class="sxs-lookup"><span data-stu-id="6e52b-157">Note how hello **MessageNumber** property value of each message varies on hello iteration of hello loop (this will determine which subscriptions receive it):</span></span>

```java
for (int i=0; i<5; i++)  {
// Create message, passing a string message for hello body
BrokeredMessage message = new BrokeredMessage("Test message " + i);
// Set some additional custom app-specific property
message.setProperty("MessageNumber", i);
// Send message toohello topic
service.sendTopicMessage("TestTopic", message);
}
```

<span data-ttu-id="6e52b-158">Service Bus-ämnena stöder en maximal meddelandestorlek på 256 KB i hello [standardnivån](service-bus-premium-messaging.md) och 1 MB på hello [premiumnivån](service-bus-premium-messaging.md).</span><span class="sxs-lookup"><span data-stu-id="6e52b-158">Service Bus topics support a maximum message size of 256 KB in hello [Standard tier](service-bus-premium-messaging.md) and 1 MB in hello [Premium tier](service-bus-premium-messaging.md).</span></span> <span data-ttu-id="6e52b-159">hello-huvudet som innehåller hello standard- och anpassade programegenskaperna, kan ha en maximal storlek på 64 KB.</span><span class="sxs-lookup"><span data-stu-id="6e52b-159">hello header, which includes hello standard and custom application properties, can have a maximum size of 64 KB.</span></span> <span data-ttu-id="6e52b-160">Det finns ingen gräns hello antalet meddelanden som lagras i ett ämne men det finns en gräns på hello totala storleken på hälsningsmeddelande som innehas av ett ämne.</span><span class="sxs-lookup"><span data-stu-id="6e52b-160">There is no limit on hello number of messages held in a topic but there is a limit on hello total size of hello messages held by a topic.</span></span> <span data-ttu-id="6e52b-161">Den här ämnesstorleken definieras när ämnet skapas, med en övre gräns på 5 GB.</span><span class="sxs-lookup"><span data-stu-id="6e52b-161">This topic size is defined at creation time, with an upper limit of 5 GB.</span></span>

## <a name="how-tooreceive-messages-from-a-subscription"></a><span data-ttu-id="6e52b-162">Hur tooreceive meddelanden från en prenumeration</span><span class="sxs-lookup"><span data-stu-id="6e52b-162">How tooreceive messages from a subscription</span></span>
<span data-ttu-id="6e52b-163">tooreceive meddelanden från en prenumeration som använder en **ServiceBusContract** objekt.</span><span class="sxs-lookup"><span data-stu-id="6e52b-163">tooreceive messages from a subscription, use a **ServiceBusContract** object.</span></span> <span data-ttu-id="6e52b-164">Mottagna meddelanden kan arbeta i två olika lägen: **ReceiveAndDelete** och **PeekLock**.</span><span class="sxs-lookup"><span data-stu-id="6e52b-164">Received messages can work in two different modes: **ReceiveAndDelete** and **PeekLock**.</span></span>

<span data-ttu-id="6e52b-165">När du använder hello **ReceiveAndDelete** läge får är en engångsåtgärd – det vill säga när Service Bus tar emot en läsbegäran för ett meddelande, den markerar hello meddelandet som Förbrukat och returnerar det toothe program.</span><span class="sxs-lookup"><span data-stu-id="6e52b-165">When using hello **ReceiveAndDelete** mode, receive is a single-shot operation - that is, when Service Bus receives a read request for a message, it marks hello message as being consumed and returns it toothe application.</span></span> <span data-ttu-id="6e52b-166">**ReceiveAndDelete** läge är hello enklaste modellen och fungerar bäst för scenarier där ett program kan tolerera icke-bearbetning av ett meddelande i hello händelse av fel.</span><span class="sxs-lookup"><span data-stu-id="6e52b-166">**ReceiveAndDelete** mode is hello simplest model and works best for scenarios in which an application can tolerate not processing a message in hello event of a failure.</span></span> <span data-ttu-id="6e52b-167">toounderstand, Föreställ i vilka hello konsumenten problem hello mottagning av begäran och sedan kraschar innan bearbetningen.</span><span class="sxs-lookup"><span data-stu-id="6e52b-167">toounderstand this, consider a scenario in which hello consumer issues hello receive request and then crashes before processing it.</span></span> <span data-ttu-id="6e52b-168">Eftersom Service Bus kommer att ha markerat meddelandet som Förbrukat, sedan när hello programmet startas om och börjar förbruka meddelanden igen, att ha missat hello-meddelande som har förbrukats tidigare toohello krascher.</span><span class="sxs-lookup"><span data-stu-id="6e52b-168">Because Service Bus will have marked the message as being consumed, then when hello application restarts and begins consuming messages again, it will have missed hello message that was consumed prior toohello crash.</span></span>

<span data-ttu-id="6e52b-169">I **PeekLock** läge får blir en åtgärd i två steg, vilket gör det möjligt toosupport program som inte tolererar att ett meddelande saknas.</span><span class="sxs-lookup"><span data-stu-id="6e52b-169">In **PeekLock** mode, receive becomes a two stage operation, which makes it possible toosupport applications that cannot tolerate missing messages.</span></span> <span data-ttu-id="6e52b-170">När Service Bus tar emot en begäran, hittar hello nästa meddelande toobe förbrukas, låser det tooprevent andra användare tar emot det och returnerar sedan toohello program.</span><span class="sxs-lookup"><span data-stu-id="6e52b-170">When Service Bus receives a request, it finds hello next message toobe consumed, locks it tooprevent other consumers receiving it, and then returns it toohello application.</span></span> <span data-ttu-id="6e52b-171">När hello programmet har avslutat bearbetningen hello-meddelande (eller lagrar det på ett tillförlitligt sätt för framtida bearbetning), Slutför hello andra steget i hello får processen genom att anropa **ta bort** på emot hello-meddelande.</span><span class="sxs-lookup"><span data-stu-id="6e52b-171">After hello application finishes processing hello message (or stores it reliably for future processing), it completes hello second stage of hello receive process by calling **Delete** on hello received message.</span></span> <span data-ttu-id="6e52b-172">När Service Bus ser hello **ta bort** -anrop som den Markera hello meddelandet som Förbrukat och ta bort den från hello-avsnittet.</span><span class="sxs-lookup"><span data-stu-id="6e52b-172">When Service Bus sees hello **Delete** call, it will mark hello message as being consumed and remove it from hello topic.</span></span>

<span data-ttu-id="6e52b-173">hello exemplet nedan visar hur meddelanden kan tas emot och bearbetat använder **PeekLock** läge (inte hello standardläget).</span><span class="sxs-lookup"><span data-stu-id="6e52b-173">hello example below demonstrates how messages can be received and processed using **PeekLock** mode (not hello default mode).</span></span> <span data-ttu-id="6e52b-174">hello exemplet nedan utför en loop behandlar meddelanden i hello ”HighMessages” prenumeration och avslutas när det finns inga fler meddelanden (du kan också det gick att ange toowait för nya meddelanden).</span><span class="sxs-lookup"><span data-stu-id="6e52b-174">hello example below performs a loop and processes messages in hello "HighMessages" subscription and then exits when there are no more messages (alternatively, it could be set toowait for new messages).</span></span>

```java
try
{
    ReceiveMessageOptions opts = ReceiveMessageOptions.DEFAULT;
    opts.setReceiveMode(ReceiveMode.PEEK_LOCK);

    while(true)  {
        ReceiveSubscriptionMessageResult  resultSubMsg =
            service.receiveSubscriptionMessage("TestTopic", "HighMessages", opts);
        BrokeredMessage message = resultSubMsg.getValue();
        if (message != null && message.getMessageId() != null)
        {
            System.out.println("MessageID: " + message.getMessageId());
            // Display hello topic message.
            System.out.print("From topic: ");
            byte[] b = new byte[200];
            String s = null;
            int numRead = message.getBody().read(b);
            while (-1 != numRead)
            {
                s = new String(b);
                s = s.trim();
                System.out.print(s);
                numRead = message.getBody().read(b);
            }
            System.out.println();
            System.out.println("Custom Property: " +
                message.getProperty("MessageNumber"));
            // Delete message.
            System.out.println("Deleting this message.");
            service.deleteMessage(message);
        }  
        else  
        {
            System.out.println("Finishing up - no more messages.");
            break;
            // Added toohandle no more messages.
            // Could instead wait for more messages toobe added.
        }
    }
}
catch (ServiceException e) {
    System.out.print("ServiceException encountered: ");
    System.out.println(e.getMessage());
    System.exit(-1);
}
catch (Exception e) {
    System.out.print("Generic exception encountered: ");
    System.out.println(e.getMessage());
    System.exit(-1);
}
```

## <a name="how-toohandle-application-crashes-and-unreadable-messages"></a><span data-ttu-id="6e52b-175">Hur toohandle programmet kraschar och oläsbara meddelanden</span><span class="sxs-lookup"><span data-stu-id="6e52b-175">How toohandle application crashes and unreadable messages</span></span>
<span data-ttu-id="6e52b-176">Service Bus innehåller funktioner toohelp du att återställa fel i programmet eller problem med bearbetning av ett meddelande.</span><span class="sxs-lookup"><span data-stu-id="6e52b-176">Service Bus provides functionality toohelp you gracefully recover from errors in your application or difficulties processing a message.</span></span> <span data-ttu-id="6e52b-177">Om ett mottagarprogram inte kan tooprocess hello meddelande av någon anledning, så kan det anropa hello **unlockMessage** metod för hello tog emot meddelande (i stället för hello **deleteMessage** metod).</span><span class="sxs-lookup"><span data-stu-id="6e52b-177">If a receiver application is unable tooprocess hello message for some reason, then it can call hello **unlockMessage** method on hello received message (instead of hello **deleteMessage** method).</span></span> <span data-ttu-id="6e52b-178">Detta orsakar Service Bus toounlock hello-meddelande i hello avsnittet och göra den tillgänglig toobe tas emot igen, antingen hello av samma förbrukning av ett program eller ett annat användningsprogram.</span><span class="sxs-lookup"><span data-stu-id="6e52b-178">This will cause Service Bus toounlock hello message within hello topic and make it available toobe received again, either by hello same consuming application or by another consuming application.</span></span>

<span data-ttu-id="6e52b-179">Det finns också en tidsgräns som är associerad med ett meddelande i avsnittet och om hello program inte tooprocess hello-meddelande innan timeout för lås går ut (till exempel om hello programmet kraschar), kommer Service Bus låser upp hello-meddelande automatiskt och gör den tillgänglig toobe tas emot igen.</span><span class="sxs-lookup"><span data-stu-id="6e52b-179">There is also a timeout associated with a message locked within the topic, and if hello application fails tooprocess hello message before the lock timeout expires (for example, if hello application crashes), then Service Bus will unlock hello message automatically and make it available toobe received again.</span></span>

<span data-ttu-id="6e52b-180">Hello händelse som hello programmet kraschar efter hello meddelandet har bearbetats men innan hello **deleteMessage** begäran utfärdas och sedan hello-meddelande kommer att levereras på nytt toohello program när den startas om.</span><span class="sxs-lookup"><span data-stu-id="6e52b-180">In hello event that hello application crashes after processing hello message but before hello **deleteMessage** request is issued, then hello message will be redelivered toohello application when it restarts.</span></span> <span data-ttu-id="6e52b-181">Det här kallas ofta **At Least Once Processing**, det vill säga, varje meddelande bearbetas minst en gång men i vissa situationer hello samma meddelande levereras.</span><span class="sxs-lookup"><span data-stu-id="6e52b-181">This is often called **At Least Once Processing**, that is, each message will be processed at least once but in certain situations hello same message may be redelivered.</span></span> <span data-ttu-id="6e52b-182">Om hello scenariot inte tolererar duplicerad bearbetning, sedan bör programutvecklarna lägga till ytterligare logik tootheir duplicerad meddelandeleverans för programmet toohandle.</span><span class="sxs-lookup"><span data-stu-id="6e52b-182">If hello scenario cannot tolerate duplicate processing, then application developers should add additional logic tootheir application toohandle duplicate message delivery.</span></span> <span data-ttu-id="6e52b-183">Detta uppnås ofta med hjälp av hello **getMessageId** metod i hello-meddelande, förblir konstant under alla leveransförsök.</span><span class="sxs-lookup"><span data-stu-id="6e52b-183">This is often achieved using hello **getMessageId** method of hello message, which will remain constant across delivery attempts.</span></span>

## <a name="delete-topics-and-subscriptions"></a><span data-ttu-id="6e52b-184">Ta bort ämnen och prenumerationer</span><span class="sxs-lookup"><span data-stu-id="6e52b-184">Delete topics and subscriptions</span></span>
<span data-ttu-id="6e52b-185">Hej primära sätt toodelete ämnen och prenumerationer är toouse en **ServiceBusContract** objekt.</span><span class="sxs-lookup"><span data-stu-id="6e52b-185">hello primary way toodelete topics and subscriptions is toouse a **ServiceBusContract** object.</span></span> <span data-ttu-id="6e52b-186">Om du tar bort ett ämne tas även bort alla prenumerationer som är registrerade med hello-avsnittet.</span><span class="sxs-lookup"><span data-stu-id="6e52b-186">Deleting a topic will also delete any subscriptions that are registered with hello topic.</span></span> <span data-ttu-id="6e52b-187">Prenumerationer kan även tas bort separat.</span><span class="sxs-lookup"><span data-stu-id="6e52b-187">Subscriptions can also be deleted independently.</span></span>

```java
// Delete subscriptions
service.deleteSubscription("TestTopic", "AllMessages");
service.deleteSubscription("TestTopic", "HighMessages");
service.deleteSubscription("TestTopic", "LowMessages");

// Delete a topic
service.deleteTopic("TestTopic");
```

## <a name="next-steps"></a><span data-ttu-id="6e52b-188">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="6e52b-188">Next Steps</span></span>
<span data-ttu-id="6e52b-189">Nu när du har lärt dig grunderna hello i Service Bus-köer, se [Service Bus-köer, ämnen och prenumerationer] [ Service Bus queues, topics, and subscriptions] för mer information.</span><span class="sxs-lookup"><span data-stu-id="6e52b-189">Now that you've learned hello basics of Service Bus queues, see [Service Bus queues, topics, and subscriptions][Service Bus queues, topics, and subscriptions] for more information.</span></span>

[Azure SDK for Java]: http://azure.microsoft.com/develop/java/
[Azure Toolkit for Eclipse]: ../azure-toolkit-for-eclipse.md
[Service Bus queues, topics, and subscriptions]: service-bus-queues-topics-subscriptions.md
[SqlFilter]: /dotnet/api/microsoft.servicebus.messaging.sqlfilter 
[SqlFilter.SqlExpression]: /dotnet/api/microsoft.servicebus.messaging.sqlfilter#Microsoft_ServiceBus_Messaging_SqlFilter_SqlExpression
[BrokeredMessage]: /dotnet/api/microsoft.servicebus.messaging.brokeredmessage

[0]: ./media/service-bus-java-how-to-use-topics-subscriptions/sb-queues-13.png
[2]: ./media/service-bus-java-how-to-use-topics-subscriptions/sb-queues-04.png
[3]: ./media/service-bus-java-how-to-use-topics-subscriptions/sb-queues-09.png
