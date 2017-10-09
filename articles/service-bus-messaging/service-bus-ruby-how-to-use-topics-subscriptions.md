---
title: "aaaHow toouse Service Bus-ämnen (Ruby) | Microsoft Docs"
description: "Lär dig hur toouse Service Bus-ämnen och prenumerationer i Azure. Kodexemplen är skrivna för Ruby program."
services: service-bus-messaging
documentationcenter: ruby
author: sethmanheim
manager: timlt
editor: 
ms.assetid: 3ef2295e-7c5f-4c54-a13b-a69c8045d4b6
ms.service: service-bus-messaging
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: ruby
ms.topic: article
ms.date: 08/10/2017
ms.author: sethm
ms.openlocfilehash: 236d6495825e68e336c23e1b500d0764ee512e49
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="how-toouse-service-bus-topics-and-subscriptions-with-ruby"></a><span data-ttu-id="6da59-104">Hur toouse Service Bus-ämnen och prenumerationer med Ruby</span><span class="sxs-lookup"><span data-stu-id="6da59-104">How toouse Service Bus topics and subscriptions with Ruby</span></span>
 
[!INCLUDE [service-bus-selector-topics](../../includes/service-bus-selector-topics.md)]

<span data-ttu-id="6da59-105">Den här artikeln beskriver hur toouse Service Bus-ämnen och prenumerationer från Ruby program.</span><span class="sxs-lookup"><span data-stu-id="6da59-105">This article describes how toouse Service Bus topics and subscriptions from Ruby applications.</span></span> <span data-ttu-id="6da59-106">hello beskrivs scenarier där **skapa ämnen och prenumerationer, skapa prenumerationsfilter, skicka meddelanden** tooa avsnittet **ta emot meddelanden från en prenumeration**, och  **ta bort ämnen och prenumerationer**.</span><span class="sxs-lookup"><span data-stu-id="6da59-106">hello scenarios covered include **creating topics and subscriptions, creating subscription filters, sending messages** tooa topic, **receiving messages from a subscription**, and **deleting topics and subscriptions**.</span></span> <span data-ttu-id="6da59-107">Mer information om ämnen och prenumerationer finns hello [nästa steg](#next-steps) avsnitt.</span><span class="sxs-lookup"><span data-stu-id="6da59-107">For more information on topics and subscriptions, see hello [Next Steps](#next-steps) section.</span></span>

[!INCLUDE [howto-service-bus-topics](../../includes/howto-service-bus-topics.md)]

[!INCLUDE [service-bus-create-namespace-portal](../../includes/service-bus-create-namespace-portal.md)]

[!INCLUDE [service-bus-ruby-setup](../../includes/service-bus-ruby-setup.md)]

## <a name="create-a-topic"></a><span data-ttu-id="6da59-108">Skapa ett ämne</span><span class="sxs-lookup"><span data-stu-id="6da59-108">Create a topic</span></span>
<span data-ttu-id="6da59-109">Hej **Azure::ServiceBusService** objekt kan du toowork med ämnen.</span><span class="sxs-lookup"><span data-stu-id="6da59-109">hello **Azure::ServiceBusService** object enables you toowork with topics.</span></span> <span data-ttu-id="6da59-110">hello följande kod skapar en **Azure::ServiceBusService** objekt.</span><span class="sxs-lookup"><span data-stu-id="6da59-110">hello following code creates an **Azure::ServiceBusService** object.</span></span> <span data-ttu-id="6da59-111">toocreate ett ämne, använda hello `create_topic()` metod.</span><span class="sxs-lookup"><span data-stu-id="6da59-111">toocreate a topic, use hello `create_topic()` method.</span></span> <span data-ttu-id="6da59-112">hello följande exempel skapar ett ämne eller skriver ut hello fel om det finns några.</span><span class="sxs-lookup"><span data-stu-id="6da59-112">hello following example creates a topic or prints out hello errors if there are any.</span></span>

```ruby
azure_service_bus_service = Azure::ServiceBus::ServiceBusService.new(sb_host, { signer: signer})
begin
  topic = azure_service_bus_service.create_queue("test-topic")
rescue
  puts $!
end
```

<span data-ttu-id="6da59-113">Du kan också skicka ett **Azure::ServiceBus::Topic** objekt med ytterligare alternativ som gör att du toooverride avsnittet standardinställningarna, till exempel meddelande tid toolive eller maximal köstorlek.</span><span class="sxs-lookup"><span data-stu-id="6da59-113">You can also pass an **Azure::ServiceBus::Topic** object with additional options, which enable you toooverride default topic settings such as message time toolive or maximum queue size.</span></span> <span data-ttu-id="6da59-114">hello nedan inställningen hello största köstorlek storlek too5 GB och tid toolive too1 minut:</span><span class="sxs-lookup"><span data-stu-id="6da59-114">hello following example shows setting hello maximum queue size too5 GB and time toolive too1 minute:</span></span>

```ruby
topic = Azure::ServiceBus::Topic.new("test-topic")
topic.max_size_in_megabytes = 5120
topic.default_message_time_to_live = "PT1M"

topic = azure_service_bus_service.create_topic(topic)
```

## <a name="create-subscriptions"></a><span data-ttu-id="6da59-115">Skapa prenumerationer</span><span class="sxs-lookup"><span data-stu-id="6da59-115">Create subscriptions</span></span>
<span data-ttu-id="6da59-116">Ämnesprenumerationer skapas också med hello **Azure::ServiceBusService** objekt.</span><span class="sxs-lookup"><span data-stu-id="6da59-116">Topic subscriptions are also created with hello **Azure::ServiceBusService** object.</span></span> <span data-ttu-id="6da59-117">Prenumerationer är namngivna och kan ha ett valfritt filter som begränsar hello uppsättning meddelanden som har levererats toohello prenumerationens virtuella kö.</span><span class="sxs-lookup"><span data-stu-id="6da59-117">Subscriptions are named and can have an optional filter that restricts hello set of messages delivered toohello subscription's virtual queue.</span></span>

<span data-ttu-id="6da59-118">Prenumerationer är beständiga och fortsätter tooexist förrän antingen de eller hello avsnittet de är kopplade till, tas bort.</span><span class="sxs-lookup"><span data-stu-id="6da59-118">Subscriptions are persistent and will continue tooexist until either they, or hello topic they are associated with, are deleted.</span></span> <span data-ttu-id="6da59-119">Om ditt program innehåller logik toocreate en prenumeration, bör först kontrollera om hello prenumerationen finns redan med hello getSubscription metod.</span><span class="sxs-lookup"><span data-stu-id="6da59-119">If your application contains logic toocreate a subscription, it should first check if hello subscription already exists by using hello getSubscription method.</span></span>

### <a name="create-a-subscription-with-hello-default-matchall-filter"></a><span data-ttu-id="6da59-120">Skapa en prenumeration med hello standardfiltret (MatchAll)</span><span class="sxs-lookup"><span data-stu-id="6da59-120">Create a subscription with hello default (MatchAll) filter</span></span>
<span data-ttu-id="6da59-121">Hej **MatchAll** filtret är hello standardfilter som används om inget filter anges när en ny prenumeration skapas.</span><span class="sxs-lookup"><span data-stu-id="6da59-121">hello **MatchAll** filter is hello default filter that is used if no filter is specified when a new subscription is created.</span></span> <span data-ttu-id="6da59-122">När hello **MatchAll** filter används, alla meddelanden publicerade toohello avsnittet placeras i hello prenumerationens virtuella kö.</span><span class="sxs-lookup"><span data-stu-id="6da59-122">When hello **MatchAll** filter is used, all messages published toohello topic are placed in hello subscription's virtual queue.</span></span> <span data-ttu-id="6da59-123">hello följande exempel skapas en prenumeration med namnet ”alla meddelanden” och använder hello standard **MatchAll** filter.</span><span class="sxs-lookup"><span data-stu-id="6da59-123">hello following example creates a subscription named "all-messages" and uses hello default **MatchAll** filter.</span></span>

```ruby
subscription = azure_service_bus_service.create_subscription("test-topic", "all-messages")
```

### <a name="create-subscriptions-with-filters"></a><span data-ttu-id="6da59-124">Skapa prenumerationer med filter</span><span class="sxs-lookup"><span data-stu-id="6da59-124">Create subscriptions with filters</span></span>
<span data-ttu-id="6da59-125">Du kan också definiera filter som gör att du toospecify vilka meddelanden som skickas tooa avsnittet visas inom en viss prenumeration.</span><span class="sxs-lookup"><span data-stu-id="6da59-125">You can also define filters that enable you toospecify which messages sent tooa topic should show up within a specific subscription.</span></span>

<span data-ttu-id="6da59-126">hello mest flexibla typen av filter som stöds av prenumerationerna är hello **Azure::ServiceBus::SqlFilter**, som implementerar en deluppsättning av SQL92.</span><span class="sxs-lookup"><span data-stu-id="6da59-126">hello most flexible type of filter supported by subscriptions is hello **Azure::ServiceBus::SqlFilter**, which implements a subset of SQL92.</span></span> <span data-ttu-id="6da59-127">SQL-filter tillämpas för hälsningsmeddelande som är publicerade toohello avsnittet hello egenskaper.</span><span class="sxs-lookup"><span data-stu-id="6da59-127">SQL filters operate on hello properties of hello messages that are published toohello topic.</span></span> <span data-ttu-id="6da59-128">Mer information om hello-uttryck som kan användas med ett SQL-filter granskar hello [SqlFilter](service-bus-messaging-sql-filter.md) syntax.</span><span class="sxs-lookup"><span data-stu-id="6da59-128">For more details about hello expressions that can be used with a SQL filter, review hello [SqlFilter](service-bus-messaging-sql-filter.md) syntax.</span></span>

<span data-ttu-id="6da59-129">Du kan lägga till filter tooa prenumeration med hjälp av hello `create_rule()` metod för hello **Azure::ServiceBusService** objekt.</span><span class="sxs-lookup"><span data-stu-id="6da59-129">You can add filters tooa subscription by using hello `create_rule()` method of hello **Azure::ServiceBusService** object.</span></span> <span data-ttu-id="6da59-130">Den här metoden kan du tooadd filter tooan befintliga prenumeration.</span><span class="sxs-lookup"><span data-stu-id="6da59-130">This method enables you tooadd new filters tooan existing subscription.</span></span>

<span data-ttu-id="6da59-131">Eftersom hello standardfiltret används automatiskt tooall nya prenumerationer, du måste först ta bort standardfiltret hello eller hello **MatchAll** åsidosätter eventuella filter som du kan ange.</span><span class="sxs-lookup"><span data-stu-id="6da59-131">Since hello default filter is applied automatically tooall new subscriptions, you must first remove hello default filter, or hello **MatchAll** will override any other filters you may specify.</span></span> <span data-ttu-id="6da59-132">Du kan ta bort hello standardregel med hello `delete_rule()` metod på hello **Azure::ServiceBusService** objekt.</span><span class="sxs-lookup"><span data-stu-id="6da59-132">You can remove hello default rule by using hello `delete_rule()` method on hello **Azure::ServiceBusService** object.</span></span>

<span data-ttu-id="6da59-133">hello följande exempel skapas en prenumeration med namnet ”hög-meddelanden” med en **Azure::ServiceBus::SqlFilter** som endast väljer meddelanden som har en anpassad `message_number` egenskap som är större än 3:</span><span class="sxs-lookup"><span data-stu-id="6da59-133">hello following example creates a subscription named "high-messages" with an **Azure::ServiceBus::SqlFilter** that only selects messages that have a custom `message_number` property greater than 3:</span></span>

```ruby
subscription = azure_service_bus_service.create_subscription("test-topic", "high-messages")
azure_service_bus_service.delete_rule("test-topic", "high-messages", "$Default")

rule = Azure::ServiceBus::Rule.new("high-messages-rule")
rule.topic = "test-topic"
rule.subscription = "high-messages"
rule.filter = Azure::ServiceBus::SqlFilter.new({
  :sql_expression => "message_number > 3" })
rule = azure_service_bus_service.create_rule(rule)
```

<span data-ttu-id="6da59-134">På liknande sätt hello följande exempel skapas en prenumeration med namnet `low-messages` med en **Azure::ServiceBus::SqlFilter** som endast väljer meddelanden som har en `message_number` egenskap som är mindre än eller lika med too3:</span><span class="sxs-lookup"><span data-stu-id="6da59-134">Similarly, hello following example creates a subscription named `low-messages` with an **Azure::ServiceBus::SqlFilter** that only selects messages that have a `message_number` property less than or equal too3:</span></span>

```ruby
subscription = azure_service_bus_service.create_subscription("test-topic", "low-messages")
azure_service_bus_service.delete_rule("test-topic", "low-messages", "$Default")

rule = Azure::ServiceBus::Rule.new("low-messages-rule")
rule.topic = "test-topic"
rule.subscription = "low-messages"
rule.filter = Azure::ServiceBus::SqlFilter.new({
  :sql_expression => "message_number <= 3" })
rule = azure_service_bus_service.create_rule(rule)
```

<span data-ttu-id="6da59-135">När ett meddelande skickas nu för`test-topic`, den är alltid vara levererade tooreceivers prenumererar toohello `all-messages` avsnittet prenumeration och levereras selektivt tooreceivers prenumererar toohello `high-messages` och `low-messages` avsnittet prenumerationer ( beroende på hello meddelandeinnehåll).</span><span class="sxs-lookup"><span data-stu-id="6da59-135">When a message is now sent too`test-topic`, it is always be delivered tooreceivers subscribed toohello `all-messages` topic subscription, and selectively delivered tooreceivers subscribed toohello `high-messages` and `low-messages` topic subscriptions (depending upon hello message content).</span></span>

## <a name="send-messages-tooa-topic"></a><span data-ttu-id="6da59-136">Skicka meddelanden tooa avsnittet</span><span class="sxs-lookup"><span data-stu-id="6da59-136">Send messages tooa topic</span></span>
<span data-ttu-id="6da59-137">toosend meddelandet tooa Service Bus-ämne programmet måste använda hello `send_topic_message()` metod på hello **Azure::ServiceBusService** objekt.</span><span class="sxs-lookup"><span data-stu-id="6da59-137">toosend a message tooa Service Bus topic, your application must use hello `send_topic_message()` method on hello **Azure::ServiceBusService** object.</span></span> <span data-ttu-id="6da59-138">Meddelanden som skickas tooService Bus-ämnen är instanser av hello **Azure::ServiceBus::BrokeredMessage** objekt.</span><span class="sxs-lookup"><span data-stu-id="6da59-138">Messages sent tooService Bus topics are instances of hello **Azure::ServiceBus::BrokeredMessage** objects.</span></span> <span data-ttu-id="6da59-139">**Azure::ServiceBus::BrokeredMessage** objekt har en uppsättning standardegenskaper (t.ex `label` och `time_to_live`), en ordlista som används toohold anpassade programspecifika egenskaper och en brödtext med strängdata.</span><span class="sxs-lookup"><span data-stu-id="6da59-139">**Azure::ServiceBus::BrokeredMessage** objects have a set of standard properties (such as `label` and `time_to_live`), a dictionary that is used toohold custom application-specific properties, and a body of string data.</span></span> <span data-ttu-id="6da59-140">Ett program kan konfigurera hello brödtext hello-meddelande genom att skicka en sträng värdet toohello `send_topic_message()` metod och eventuella krävs standardegenskaper fylls i med standardvärden.</span><span class="sxs-lookup"><span data-stu-id="6da59-140">An application can set hello body of hello message by passing a string value toohello `send_topic_message()` method and any required standard properties will be populated by default values.</span></span>

<span data-ttu-id="6da59-141">hello exemplet nedan visar hur toosend fem test meddelanden för`test-topic`.</span><span class="sxs-lookup"><span data-stu-id="6da59-141">hello following example demonstrates how toosend five test messages too`test-topic`.</span></span> <span data-ttu-id="6da59-142">Observera att hello `message_number` anpassade egenskapsvärdet för varje meddelande varierar för hello iteration av hello loopen (detta avgör vilken prenumeration tar emot det):</span><span class="sxs-lookup"><span data-stu-id="6da59-142">Note that hello `message_number` custom property value of each message varies on hello iteration of hello loop (this determines which subscription receives it):</span></span>

```ruby
5.times do |i|
  message = Azure::ServiceBus::BrokeredMessage.new("test message " + i,
    { :message_number => i })
  azure_service_bus_service.send_topic_message("test-topic", message)
end
```

<span data-ttu-id="6da59-143">Service Bus-ämnena stöder en maximal meddelandestorlek på 256 KB i hello [standardnivån](service-bus-premium-messaging.md) och 1 MB på hello [premiumnivån](service-bus-premium-messaging.md).</span><span class="sxs-lookup"><span data-stu-id="6da59-143">Service Bus topics support a maximum message size of 256 KB in hello [Standard tier](service-bus-premium-messaging.md) and 1 MB in hello [Premium tier](service-bus-premium-messaging.md).</span></span> <span data-ttu-id="6da59-144">hello-huvudet som innehåller hello standard- och anpassade programegenskaperna, kan ha en maximal storlek på 64 KB.</span><span class="sxs-lookup"><span data-stu-id="6da59-144">hello header, which includes hello standard and custom application properties, can have a maximum size of 64 KB.</span></span> <span data-ttu-id="6da59-145">Det finns ingen gräns hello antalet meddelanden som lagras i ett ämne men det finns ett tak på hello totala storleken på hälsningsmeddelande som innehas av ett ämne.</span><span class="sxs-lookup"><span data-stu-id="6da59-145">There is no limit on hello number of messages held in a topic but there is a cap on hello total size of hello messages held by a topic.</span></span> <span data-ttu-id="6da59-146">Den här ämnesstorleken definieras när ämnet skapas, med en övre gräns på 5 GB.</span><span class="sxs-lookup"><span data-stu-id="6da59-146">This topic size is defined at creation time, with an upper limit of 5 GB.</span></span>

## <a name="receive-messages-from-a-subscription"></a><span data-ttu-id="6da59-147">Ta emot meddelanden från en prenumeration</span><span class="sxs-lookup"><span data-stu-id="6da59-147">Receive messages from a subscription</span></span>
<span data-ttu-id="6da59-148">Meddelanden tas emot från en prenumeration med hjälp av hello `receive_subscription_message()` metod på hello **Azure::ServiceBusService** objekt.</span><span class="sxs-lookup"><span data-stu-id="6da59-148">Messages are received from a subscription using hello `receive_subscription_message()` method on hello **Azure::ServiceBusService** object.</span></span> <span data-ttu-id="6da59-149">Som standard meddelanden read(peak) och låst utan att ta bort den från hello prenumeration.</span><span class="sxs-lookup"><span data-stu-id="6da59-149">By default, messages are read(peak) and locked without deleting it from hello subscription.</span></span> <span data-ttu-id="6da59-150">Du kan läsa och ta bort hello-meddelande från hello prenumeration genom att ange hello `peek_lock` alternativet för**FALSKT**.</span><span class="sxs-lookup"><span data-stu-id="6da59-150">You can read and delete hello message from hello subscription by setting hello `peek_lock` option too**false**.</span></span>

<span data-ttu-id="6da59-151">hello standardbeteendet gör hello läser och tar bort en åtgärd i två steg, vilket gör det också möjligt toosupport program som inte tolererar att ett meddelande saknas.</span><span class="sxs-lookup"><span data-stu-id="6da59-151">hello default behavior makes hello reading and deleting a two-stage operation, which also makes it possible toosupport applications that cannot tolerate missing messages.</span></span> <span data-ttu-id="6da59-152">När Service Bus tar emot en begäran, hittar hello nästa meddelande toobe förbrukas, låser det tooprevent andra användare tar emot det och returnerar sedan toohello program.</span><span class="sxs-lookup"><span data-stu-id="6da59-152">When Service Bus receives a request, it finds hello next message toobe consumed, locks it tooprevent other consumers receiving it, and then returns it toohello application.</span></span> <span data-ttu-id="6da59-153">När hello programmet har avslutat bearbetningen hello-meddelande (eller lagrar det på ett tillförlitligt sätt för framtida bearbetning), Slutför hello andra steget i hello får processen genom att anropa `delete_subscription_message()` metod och ge hello meddelandet toobe tas bort som en parameter.</span><span class="sxs-lookup"><span data-stu-id="6da59-153">After hello application finishes processing hello message (or stores it reliably for future processing), it completes hello second stage of hello receive process by calling `delete_subscription_message()` method and providing hello message toobe deleted as a parameter.</span></span> <span data-ttu-id="6da59-154">Hej `delete_subscription_message()` metoden kommer Markera hello meddelandet som Förbrukat och ta bort den från hello prenumeration.</span><span class="sxs-lookup"><span data-stu-id="6da59-154">hello `delete_subscription_message()` method will mark hello message as being consumed and remove it from hello subscription.</span></span>

<span data-ttu-id="6da59-155">Om hello `:peek_lock` parameter har angetts för**FALSKT**, läser och tar bort hello-meddelande blir hello enklaste modellen och fungerar bäst för scenarier där ett program kan tolerera icke-bearbetning av ett meddelande i hello-händelse en ett fel uppstod.</span><span class="sxs-lookup"><span data-stu-id="6da59-155">If hello `:peek_lock` parameter is set too**false**, reading and deleting hello message becomes hello simplest model, and works best for scenarios in which an application can tolerate not processing a message in hello event of a failure.</span></span> <span data-ttu-id="6da59-156">toounderstand, Föreställ i vilka hello konsumenten problem hello mottagning av begäran och sedan kraschar innan bearbetningen.</span><span class="sxs-lookup"><span data-stu-id="6da59-156">toounderstand this, consider a scenario in which hello consumer issues hello receive request and then crashes before processing it.</span></span> <span data-ttu-id="6da59-157">Eftersom Service Bus kommer att ha markerat hello meddelandet som Förbrukat, sedan när hello programmet startas om och börjar förbruka meddelanden igen, att ha missat hello-meddelande som har förbrukats tidigare toohello krascher.</span><span class="sxs-lookup"><span data-stu-id="6da59-157">Because Service Bus will have marked hello message as being consumed, then when hello application restarts and begins consuming messages again, it will have missed hello message that was consumed prior toohello crash.</span></span>

<span data-ttu-id="6da59-158">hello exemplet nedan visar hur meddelanden kan tas emot och bearbetat använder `receive_subscription_message()`.</span><span class="sxs-lookup"><span data-stu-id="6da59-158">hello following example demonstrates how messages can be received and processed using `receive_subscription_message()`.</span></span> <span data-ttu-id="6da59-159">hello exempel först tar emot och tar bort ett meddelande från hello `low-messages` prenumeration med hjälp av `:peek_lock` ställa in också**FALSKT**, och sedan ett nytt meddelande tas emot från hello `high-messages` och sedan tar bort hello meddelande med `delete_subscription_message()`:</span><span class="sxs-lookup"><span data-stu-id="6da59-159">hello example first receives and deletes a message from hello `low-messages` subscription by using `:peek_lock` set too**false**, then it receives another message from hello `high-messages` and then deletes hello message using `delete_subscription_message()`:</span></span>

```ruby
message = azure_service_bus_service.receive_subscription_message(
  "test-topic", "low-messages", { :peek_lock => false })
message = azure_service_bus_service.receive_subscription_message(
  "test-topic", "high-messages")
azure_service_bus_service.delete_subscription_message(message)
```

## <a name="how-toohandle-application-crashes-and-unreadable-messages"></a><span data-ttu-id="6da59-160">Hur toohandle programmet kraschar och oläsbara meddelanden</span><span class="sxs-lookup"><span data-stu-id="6da59-160">How toohandle application crashes and unreadable messages</span></span>
<span data-ttu-id="6da59-161">Service Bus innehåller funktioner toohelp du att återställa fel i programmet eller problem med bearbetning av ett meddelande.</span><span class="sxs-lookup"><span data-stu-id="6da59-161">Service Bus provides functionality toohelp you gracefully recover from errors in your application or difficulties processing a message.</span></span> <span data-ttu-id="6da59-162">Om ett mottagarprogram inte kan tooprocess hello meddelande av någon anledning, så kan det anropa hello `unlock_subscription_message()` metod på hello **Azure::ServiceBusService** objekt.</span><span class="sxs-lookup"><span data-stu-id="6da59-162">If a receiver application is unable tooprocess hello message for some reason, then it can call hello `unlock_subscription_message()` method on hello **Azure::ServiceBusService** object.</span></span> <span data-ttu-id="6da59-163">Detta medför att Service Bus toounlock hello meddelande inom hello prenumeration och göra den tillgänglig toobe tas emot igen, hello antingen av samma förbrukning av ett program eller ett annat användningsprogram.</span><span class="sxs-lookup"><span data-stu-id="6da59-163">This causes Service Bus toounlock hello message within hello subscription and make it available toobe received again, either by hello same consuming application or by another consuming application.</span></span>

<span data-ttu-id="6da59-164">Det finns också en tidsgräns som är associerad med ett meddelande i hello prenumeration och om hello program inte tooprocess hello-meddelande innan timeout för lås hello går ut (till exempel om hello programmet kraschar), kommer Service Bus att låsa upp hello-meddelande automatiskt och gör den tillgänglig toobe tas emot igen.</span><span class="sxs-lookup"><span data-stu-id="6da59-164">There is also a timeout associated with a message locked within hello subscription, and if hello application fails tooprocess hello message before hello lock timeout expires (for example, if hello application crashes), then Service Bus will unlock hello message automatically and make it available toobe received again.</span></span>

<span data-ttu-id="6da59-165">Hello händelse som hello programmet kraschar efter hello meddelandet har bearbetats men innan hello `delete_subscription_message()` metoden anropas sedan hello-meddelande är toohello levereras på nytt program när den startas om.</span><span class="sxs-lookup"><span data-stu-id="6da59-165">In hello event that hello application crashes after processing hello message but before hello `delete_subscription_message()` method is called, then hello message is redelivered toohello application when it restarts.</span></span> <span data-ttu-id="6da59-166">Det här kallas ofta *At Least Once Processing*, det vill säga varje meddelande bearbetas minst en gång men i vissa situationer hello samma meddelande kan levereras.</span><span class="sxs-lookup"><span data-stu-id="6da59-166">This is often called *At Least Once Processing*; that is, each message will be processed at least once but in certain situations hello same message may be redelivered.</span></span> <span data-ttu-id="6da59-167">Om hello scenariot inte tolererar duplicerad bearbetning, sedan bör programutvecklarna lägga till ytterligare logik tootheir duplicerad meddelandeleverans för programmet toohandle.</span><span class="sxs-lookup"><span data-stu-id="6da59-167">If hello scenario cannot tolerate duplicate processing, then application developers should add additional logic tootheir application toohandle duplicate message delivery.</span></span> <span data-ttu-id="6da59-168">Den här logiken uppnås ofta med hjälp av hello `message_id` -egenskapen för hello-meddelande, förblir konstant under alla leveransförsök.</span><span class="sxs-lookup"><span data-stu-id="6da59-168">This logic is often achieved using hello `message_id` property of hello message, which will remain constant across delivery attempts.</span></span>

## <a name="delete-topics-and-subscriptions"></a><span data-ttu-id="6da59-169">Ta bort ämnen och prenumerationer</span><span class="sxs-lookup"><span data-stu-id="6da59-169">Delete topics and subscriptions</span></span>
<span data-ttu-id="6da59-170">Ämnen och prenumerationer är beständiga och måste vara explicit bort antingen via hello [Azure-portalen] [ Azure portal] eller programmässigt.</span><span class="sxs-lookup"><span data-stu-id="6da59-170">Topics and subscriptions are persistent, and must be explicitly deleted either through hello [Azure portal][Azure portal] or programmatically.</span></span> <span data-ttu-id="6da59-171">hello exemplet nedan visar hur toodelete hello avsnittet med namnet `test-topic`.</span><span class="sxs-lookup"><span data-stu-id="6da59-171">hello example below demonstrates how toodelete hello topic named `test-topic`.</span></span>

```ruby
azure_service_bus_service.delete_topic("test-topic")
```

<span data-ttu-id="6da59-172">Om du tar bort ett ämne tas även bort alla prenumerationer som är registrerade med hello-avsnittet.</span><span class="sxs-lookup"><span data-stu-id="6da59-172">Deleting a topic also deletes any subscriptions that are registered with hello topic.</span></span> <span data-ttu-id="6da59-173">Prenumerationer kan även tas bort separat.</span><span class="sxs-lookup"><span data-stu-id="6da59-173">Subscriptions can also be deleted independently.</span></span> <span data-ttu-id="6da59-174">hello följande kod visar hur toodelete hello prenumeration med namnet `high-messages` från hello `test-topic` avsnittet:</span><span class="sxs-lookup"><span data-stu-id="6da59-174">hello following code demonstrates how toodelete hello subscription named `high-messages` from hello `test-topic` topic:</span></span>

```ruby
azure_service_bus_service.delete_subscription("test-topic", "high-messages")
```

## <a name="next-steps"></a><span data-ttu-id="6da59-175">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="6da59-175">Next steps</span></span>
<span data-ttu-id="6da59-176">Nu när du har lärt dig grunderna hello i Service Bus-ämnen, följa dessa länkar toolearn mer.</span><span class="sxs-lookup"><span data-stu-id="6da59-176">Now that you've learned hello basics of Service Bus topics, follow these links toolearn more.</span></span>

* <span data-ttu-id="6da59-177">Se [köer, ämnen och prenumerationer](service-bus-queues-topics-subscriptions.md).</span><span class="sxs-lookup"><span data-stu-id="6da59-177">See [Queues, topics, and subscriptions](service-bus-queues-topics-subscriptions.md).</span></span>
* <span data-ttu-id="6da59-178">API-referens för [SqlFilter](/dotnet/api/microsoft.servicebus.messaging.sqlfilter#microsoft_servicebus_messaging_sqlfilter).</span><span class="sxs-lookup"><span data-stu-id="6da59-178">API reference for [SqlFilter](/dotnet/api/microsoft.servicebus.messaging.sqlfilter#microsoft_servicebus_messaging_sqlfilter).</span></span>
* <span data-ttu-id="6da59-179">Besök hello [Azure SDK för Ruby](https://github.com/Azure/azure-sdk-for-ruby) databasen på GitHub.</span><span class="sxs-lookup"><span data-stu-id="6da59-179">Visit hello [Azure SDK for Ruby](https://github.com/Azure/azure-sdk-for-ruby) repository on GitHub.</span></span>

[Azure portal]: https://portal.azure.com
