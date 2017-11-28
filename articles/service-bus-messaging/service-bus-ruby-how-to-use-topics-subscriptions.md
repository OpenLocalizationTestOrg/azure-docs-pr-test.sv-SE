---
title: "Hur du använder Service Bus-ämnen (Ruby) | Microsoft Docs"
description: "Lär dig hur du använder Service Bus-ämnen och prenumerationer i Azure. Kodexemplen är skrivna för Ruby program."
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
ms.openlocfilehash: 4a4c9949843b16ae6be2f516de4fd1e3f7415959
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/18/2017
---
# <a name="how-to-use-service-bus-topics-and-subscriptions-with-ruby"></a><span data-ttu-id="69057-104">Hur du använder Service Bus-ämnen och prenumerationer med Ruby</span><span class="sxs-lookup"><span data-stu-id="69057-104">How to use Service Bus topics and subscriptions with Ruby</span></span>
 
[!INCLUDE [service-bus-selector-topics](../../includes/service-bus-selector-topics.md)]

<span data-ttu-id="69057-105">Den här artikeln beskriver hur du använder Service Bus-ämnen och prenumerationer från Ruby program.</span><span class="sxs-lookup"><span data-stu-id="69057-105">This article describes how to use Service Bus topics and subscriptions from Ruby applications.</span></span> <span data-ttu-id="69057-106">Scenarier som tas upp inkluderar **skapa ämnen och prenumerationer, skapa prenumerationsfilter, skicka meddelanden** till ett ämne **ta emot meddelanden från en prenumeration**, och **tas bort ämnen och prenumerationer**.</span><span class="sxs-lookup"><span data-stu-id="69057-106">The scenarios covered include **creating topics and subscriptions, creating subscription filters, sending messages** to a topic, **receiving messages from a subscription**, and **deleting topics and subscriptions**.</span></span> <span data-ttu-id="69057-107">Mer information om ämnen och prenumerationer finns i [nästa steg](#next-steps) avsnitt.</span><span class="sxs-lookup"><span data-stu-id="69057-107">For more information on topics and subscriptions, see the [Next Steps](#next-steps) section.</span></span>

[!INCLUDE [howto-service-bus-topics](../../includes/howto-service-bus-topics.md)]

[!INCLUDE [service-bus-create-namespace-portal](../../includes/service-bus-create-namespace-portal.md)]

[!INCLUDE [service-bus-ruby-setup](../../includes/service-bus-ruby-setup.md)]

## <a name="create-a-topic"></a><span data-ttu-id="69057-108">Skapa ett ämne</span><span class="sxs-lookup"><span data-stu-id="69057-108">Create a topic</span></span>
<span data-ttu-id="69057-109">Den **Azure::ServiceBusService** objekt kan du arbeta med ämnen.</span><span class="sxs-lookup"><span data-stu-id="69057-109">The **Azure::ServiceBusService** object enables you to work with topics.</span></span> <span data-ttu-id="69057-110">Följande kod skapar en **Azure::ServiceBusService** objekt.</span><span class="sxs-lookup"><span data-stu-id="69057-110">The following code creates an **Azure::ServiceBusService** object.</span></span> <span data-ttu-id="69057-111">Du kan skapa ett ämne med den `create_topic()` metoden.</span><span class="sxs-lookup"><span data-stu-id="69057-111">To create a topic, use the `create_topic()` method.</span></span> <span data-ttu-id="69057-112">I följande exempel skapar ett ämne eller skriver ut felen om det finns några.</span><span class="sxs-lookup"><span data-stu-id="69057-112">The following example creates a topic or prints out the errors if there are any.</span></span>

```ruby
azure_service_bus_service = Azure::ServiceBus::ServiceBusService.new(sb_host, { signer: signer})
begin
  topic = azure_service_bus_service.create_queue("test-topic")
rescue
  puts $!
end
```

<span data-ttu-id="69057-113">Du kan också skicka ett **Azure::ServiceBus::Topic** objekt med ytterligare alternativ som gör att du kan åsidosätta standardinställningar för ämnet, till exempel tid till live eller maximal köstorlek.</span><span class="sxs-lookup"><span data-stu-id="69057-113">You can also pass an **Azure::ServiceBus::Topic** object with additional options, which enable you to override default topic settings such as message time to live or maximum queue size.</span></span> <span data-ttu-id="69057-114">I följande exempel visas att ställa in den maximala storleken till 5 GB och -tid live till 1 minut:</span><span class="sxs-lookup"><span data-stu-id="69057-114">The following example shows setting the maximum queue size to 5 GB and time to live to 1 minute:</span></span>

```ruby
topic = Azure::ServiceBus::Topic.new("test-topic")
topic.max_size_in_megabytes = 5120
topic.default_message_time_to_live = "PT1M"

topic = azure_service_bus_service.create_topic(topic)
```

## <a name="create-subscriptions"></a><span data-ttu-id="69057-115">Skapa prenumerationer</span><span class="sxs-lookup"><span data-stu-id="69057-115">Create subscriptions</span></span>
<span data-ttu-id="69057-116">Ämnesprenumerationer skapas också med den **Azure::ServiceBusService** objekt.</span><span class="sxs-lookup"><span data-stu-id="69057-116">Topic subscriptions are also created with the **Azure::ServiceBusService** object.</span></span> <span data-ttu-id="69057-117">Prenumerationer är namngivna och kan ha ett valfritt filter som begränsar uppsättningen av meddelanden som skickas till prenumerationens virtuella kö.</span><span class="sxs-lookup"><span data-stu-id="69057-117">Subscriptions are named and can have an optional filter that restricts the set of messages delivered to the subscription's virtual queue.</span></span>

<span data-ttu-id="69057-118">Prenumerationer är beständiga och fortsätter att finns förrän antingen de eller avsnittet som de är kopplade till, tas bort.</span><span class="sxs-lookup"><span data-stu-id="69057-118">Subscriptions are persistent and will continue to exist until either they, or the topic they are associated with, are deleted.</span></span> <span data-ttu-id="69057-119">Om ditt program innehåller logik för att skapa en prenumeration, bör det först kontrollera om prenumerationen finns redan med hjälp av metoden getSubscription.</span><span class="sxs-lookup"><span data-stu-id="69057-119">If your application contains logic to create a subscription, it should first check if the subscription already exists by using the getSubscription method.</span></span>

### <a name="create-a-subscription-with-the-default-matchall-filter"></a><span data-ttu-id="69057-120">Skapa en prenumeration med standardfiltret (MatchAll)</span><span class="sxs-lookup"><span data-stu-id="69057-120">Create a subscription with the default (MatchAll) filter</span></span>
<span data-ttu-id="69057-121">**MatchAll**-filtret är det standardfilter som används om inget filter anges när en ny prenumeration skapas.</span><span class="sxs-lookup"><span data-stu-id="69057-121">The **MatchAll** filter is the default filter that is used if no filter is specified when a new subscription is created.</span></span> <span data-ttu-id="69057-122">När den **MatchAll** filter används, alla meddelanden som publiceras till ämnet placeras i prenumerationens virtuella kö.</span><span class="sxs-lookup"><span data-stu-id="69057-122">When the **MatchAll** filter is used, all messages published to the topic are placed in the subscription's virtual queue.</span></span> <span data-ttu-id="69057-123">I följande exempel skapar en prenumeration med namnet ”alla meddelanden” och använder förvalet **MatchAll** filter.</span><span class="sxs-lookup"><span data-stu-id="69057-123">The following example creates a subscription named "all-messages" and uses the default **MatchAll** filter.</span></span>

```ruby
subscription = azure_service_bus_service.create_subscription("test-topic", "all-messages")
```

### <a name="create-subscriptions-with-filters"></a><span data-ttu-id="69057-124">Skapa prenumerationer med filter</span><span class="sxs-lookup"><span data-stu-id="69057-124">Create subscriptions with filters</span></span>
<span data-ttu-id="69057-125">Du kan också definiera filter som gör att du kan ange vilka meddelanden som skickas till ett ämne visas inom en viss prenumeration.</span><span class="sxs-lookup"><span data-stu-id="69057-125">You can also define filters that enable you to specify which messages sent to a topic should show up within a specific subscription.</span></span>

<span data-ttu-id="69057-126">Den mest flexibla typen av filter som stöds av prenumerationerna är den **Azure::ServiceBus::SqlFilter**, som implementerar en deluppsättning av SQL92.</span><span class="sxs-lookup"><span data-stu-id="69057-126">The most flexible type of filter supported by subscriptions is the **Azure::ServiceBus::SqlFilter**, which implements a subset of SQL92.</span></span> <span data-ttu-id="69057-127">SQL-filter tillämpas på egenskaperna i de meddelanden som publiceras till ämnet.</span><span class="sxs-lookup"><span data-stu-id="69057-127">SQL filters operate on the properties of the messages that are published to the topic.</span></span> <span data-ttu-id="69057-128">Mer information om uttryck som kan användas med ett SQL-filter granskar den [SqlFilter](service-bus-messaging-sql-filter.md) syntax.</span><span class="sxs-lookup"><span data-stu-id="69057-128">For more details about the expressions that can be used with a SQL filter, review the [SqlFilter](service-bus-messaging-sql-filter.md) syntax.</span></span>

<span data-ttu-id="69057-129">Du kan lägga till filter till en prenumeration med hjälp av den `create_rule()` metod för den **Azure::ServiceBusService** objekt.</span><span class="sxs-lookup"><span data-stu-id="69057-129">You can add filters to a subscription by using the `create_rule()` method of the **Azure::ServiceBusService** object.</span></span> <span data-ttu-id="69057-130">Den här metoden kan du lägga till nya filter till en befintlig prenumeration.</span><span class="sxs-lookup"><span data-stu-id="69057-130">This method enables you to add new filters to an existing subscription.</span></span>

<span data-ttu-id="69057-131">Eftersom standardfiltret används automatiskt för alla nya prenumerationer, måste du först ta bort standardfiltret eller **MatchAll** åsidosätter eventuella filter som du kan ange.</span><span class="sxs-lookup"><span data-stu-id="69057-131">Since the default filter is applied automatically to all new subscriptions, you must first remove the default filter, or the **MatchAll** will override any other filters you may specify.</span></span> <span data-ttu-id="69057-132">Du kan ta bort Standardregeln med hjälp av den `delete_rule()` -metoden i den **Azure::ServiceBusService** objekt.</span><span class="sxs-lookup"><span data-stu-id="69057-132">You can remove the default rule by using the `delete_rule()` method on the **Azure::ServiceBusService** object.</span></span>

<span data-ttu-id="69057-133">I följande exempel skapas en prenumeration med namnet ”hög-meddelanden” med en **Azure::ServiceBus::SqlFilter** som endast väljer meddelanden som har en anpassad `message_number` egenskap som är större än 3:</span><span class="sxs-lookup"><span data-stu-id="69057-133">The following example creates a subscription named "high-messages" with an **Azure::ServiceBus::SqlFilter** that only selects messages that have a custom `message_number` property greater than 3:</span></span>

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

<span data-ttu-id="69057-134">På samma sätt i följande exempel skapas en prenumeration med namnet `low-messages` med en **Azure::ServiceBus::SqlFilter** som endast väljer meddelanden som har en `message_number` egenskap som är mindre än eller lika med 3:</span><span class="sxs-lookup"><span data-stu-id="69057-134">Similarly, the following example creates a subscription named `low-messages` with an **Azure::ServiceBus::SqlFilter** that only selects messages that have a `message_number` property less than or equal to 3:</span></span>

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

<span data-ttu-id="69057-135">När ett meddelande skickas nu till `test-topic`, är alltid levereras till mottagare som prenumererar på den `all-messages` avsnittet prenumeration, och levereras selektivt till mottagare som prenumererar på den `high-messages` och `low-messages` ämnesprenumerationer (beroende När innehållet i meddelandet).</span><span class="sxs-lookup"><span data-stu-id="69057-135">When a message is now sent to `test-topic`, it is always be delivered to receivers subscribed to the `all-messages` topic subscription, and selectively delivered to receivers subscribed to the `high-messages` and `low-messages` topic subscriptions (depending upon the message content).</span></span>

## <a name="send-messages-to-a-topic"></a><span data-ttu-id="69057-136">Skicka meddelanden till ett ämne</span><span class="sxs-lookup"><span data-stu-id="69057-136">Send messages to a topic</span></span>
<span data-ttu-id="69057-137">Om du vill skicka ett meddelande till en Service Bus-ämne, ditt program måste använda den `send_topic_message()` -metoden i den **Azure::ServiceBusService** objekt.</span><span class="sxs-lookup"><span data-stu-id="69057-137">To send a message to a Service Bus topic, your application must use the `send_topic_message()` method on the **Azure::ServiceBusService** object.</span></span> <span data-ttu-id="69057-138">Meddelanden som skickas till Service Bus-ämnen är instanser av den **Azure::ServiceBus::BrokeredMessage** objekt.</span><span class="sxs-lookup"><span data-stu-id="69057-138">Messages sent to Service Bus topics are instances of the **Azure::ServiceBus::BrokeredMessage** objects.</span></span> <span data-ttu-id="69057-139">**Azure::ServiceBus::BrokeredMessage** objekt har en uppsättning standardegenskaper (t.ex `label` och `time_to_live`), en ordlista som används för att lagra anpassade programspecifika egenskaper och en brödtext med strängdata.</span><span class="sxs-lookup"><span data-stu-id="69057-139">**Azure::ServiceBus::BrokeredMessage** objects have a set of standard properties (such as `label` and `time_to_live`), a dictionary that is used to hold custom application-specific properties, and a body of string data.</span></span> <span data-ttu-id="69057-140">Ett program kan konfigurera meddelandets brödtext för meddelandet genom att skicka ett strängvärde till den `send_topic_message()` metoden och eventuella krävs standardegenskaper fylls i med standardvärden.</span><span class="sxs-lookup"><span data-stu-id="69057-140">An application can set the body of the message by passing a string value to the `send_topic_message()` method and any required standard properties will be populated by default values.</span></span>

<span data-ttu-id="69057-141">I följande exempel visar hur du skickar fem testmeddelanden till `test-topic`.</span><span class="sxs-lookup"><span data-stu-id="69057-141">The following example demonstrates how to send five test messages to `test-topic`.</span></span> <span data-ttu-id="69057-142">Observera att den `message_number` värdet för anpassad egenskap för varje meddelande varierar på upprepning av loopen (detta avgör vilken prenumeration tar emot det):</span><span class="sxs-lookup"><span data-stu-id="69057-142">Note that the `message_number` custom property value of each message varies on the iteration of the loop (this determines which subscription receives it):</span></span>

```ruby
5.times do |i|
  message = Azure::ServiceBus::BrokeredMessage.new("test message " + i,
    { :message_number => i })
  azure_service_bus_service.send_topic_message("test-topic", message)
end
```

<span data-ttu-id="69057-143">Service Bus-ämnena stöder en maximal meddelandestorlek på 256 kB på [standardnivån](service-bus-premium-messaging.md) och 1 MB på [premiumnivån](service-bus-premium-messaging.md).</span><span class="sxs-lookup"><span data-stu-id="69057-143">Service Bus topics support a maximum message size of 256 KB in the [Standard tier](service-bus-premium-messaging.md) and 1 MB in the [Premium tier](service-bus-premium-messaging.md).</span></span> <span data-ttu-id="69057-144">Rubriken, som inkluderar standardprogramegenskaperna och de anpassade programegenskaperna, kan ha en maximal storlek på 64 kB.</span><span class="sxs-lookup"><span data-stu-id="69057-144">The header, which includes the standard and custom application properties, can have a maximum size of 64 KB.</span></span> <span data-ttu-id="69057-145">Det finns ingen gräns för antalet meddelanden som kan finnas i ett ämne men det finns ett tak för den totala storleken för de meddelanden som ligger i ett ämne.</span><span class="sxs-lookup"><span data-stu-id="69057-145">There is no limit on the number of messages held in a topic but there is a cap on the total size of the messages held by a topic.</span></span> <span data-ttu-id="69057-146">Den här ämnesstorleken definieras när ämnet skapas, med en övre gräns på 5 GB.</span><span class="sxs-lookup"><span data-stu-id="69057-146">This topic size is defined at creation time, with an upper limit of 5 GB.</span></span>

## <a name="receive-messages-from-a-subscription"></a><span data-ttu-id="69057-147">Ta emot meddelanden från en prenumeration</span><span class="sxs-lookup"><span data-stu-id="69057-147">Receive messages from a subscription</span></span>
<span data-ttu-id="69057-148">Meddelanden tas emot från en prenumeration med hjälp av den `receive_subscription_message()` -metoden i den **Azure::ServiceBusService** objekt.</span><span class="sxs-lookup"><span data-stu-id="69057-148">Messages are received from a subscription using the `receive_subscription_message()` method on the **Azure::ServiceBusService** object.</span></span> <span data-ttu-id="69057-149">Som standard meddelanden read(peak) och låst utan att ta bort den från prenumerationen.</span><span class="sxs-lookup"><span data-stu-id="69057-149">By default, messages are read(peak) and locked without deleting it from the subscription.</span></span> <span data-ttu-id="69057-150">Du kan läsa och ta bort meddelandet från prenumerationen genom att ange den `peek_lock` att **FALSKT**.</span><span class="sxs-lookup"><span data-stu-id="69057-150">You can read and delete the message from the subscription by setting the `peek_lock` option to **false**.</span></span>

<span data-ttu-id="69057-151">Standardbeteendet gör läsningen och ta bort en åtgärd i två steg, vilket gör det också möjligt att stödja program som inte tolererar att ett meddelande saknas.</span><span class="sxs-lookup"><span data-stu-id="69057-151">The default behavior makes the reading and deleting a two-stage operation, which also makes it possible to support applications that cannot tolerate missing messages.</span></span> <span data-ttu-id="69057-152">När Service Bus tar emot en begäran letar det upp nästa meddelande som ska förbrukas, låser det för att förhindra att andra användare tar emot det och skickar sedan tillbaka det till programmet.</span><span class="sxs-lookup"><span data-stu-id="69057-152">When Service Bus receives a request, it finds the next message to be consumed, locks it to prevent other consumers receiving it, and then returns it to the application.</span></span> <span data-ttu-id="69057-153">När programmet har avslutat bearbetningen av meddelandet (eller lagrar det på ett tillförlitligt sätt för framtida bearbetning), Slutför det andra steget i processen genom att anropa `delete_subscription_message()` metod och ge meddelandet som ska tas bort som en parameter.</span><span class="sxs-lookup"><span data-stu-id="69057-153">After the application finishes processing the message (or stores it reliably for future processing), it completes the second stage of the receive process by calling `delete_subscription_message()` method and providing the message to be deleted as a parameter.</span></span> <span data-ttu-id="69057-154">Den `delete_subscription_message()` metoden att markera meddelandet som Förbrukat och ta bort den från prenumerationen.</span><span class="sxs-lookup"><span data-stu-id="69057-154">The `delete_subscription_message()` method will mark the message as being consumed and remove it from the subscription.</span></span>

<span data-ttu-id="69057-155">Om den `:peek_lock` parametern anges till **FALSKT**, läser och tar bort meddelandet blir den enklaste modellen och fungerar bäst för scenarier där ett program kan tolerera icke-bearbetning av ett meddelande om ett fel inträffar.</span><span class="sxs-lookup"><span data-stu-id="69057-155">If the `:peek_lock` parameter is set to **false**, reading and deleting the message becomes the simplest model, and works best for scenarios in which an application can tolerate not processing a message in the event of a failure.</span></span> <span data-ttu-id="69057-156">För att förstå detta kan du föreställa dig ett scenario där konsumenten utfärdar en receive-begäran och sedan kraschar innan den kan bearbeta denna begäran.</span><span class="sxs-lookup"><span data-stu-id="69057-156">To understand this, consider a scenario in which the consumer issues the receive request and then crashes before processing it.</span></span> <span data-ttu-id="69057-157">Eftersom Service Bus kommer att ha markerat meddelandet som Förbrukat, och sedan när programmet startas om och börjar förbruka meddelanden igen, att ha missat meddelandet som förbrukades innan kraschen.</span><span class="sxs-lookup"><span data-stu-id="69057-157">Because Service Bus will have marked the message as being consumed, then when the application restarts and begins consuming messages again, it will have missed the message that was consumed prior to the crash.</span></span>

<span data-ttu-id="69057-158">Exemplet nedan visar hur meddelanden kan tas emot och bearbetat använder `receive_subscription_message()`.</span><span class="sxs-lookup"><span data-stu-id="69057-158">The following example demonstrates how messages can be received and processed using `receive_subscription_message()`.</span></span> <span data-ttu-id="69057-159">I exempel först tar emot och tar bort ett meddelande från den `low-messages` prenumeration med hjälp av `:peek_lock` inställd på **FALSKT**, och den får ett nytt meddelande från den `high-messages` och tar bort meddelandet med `delete_subscription_message()`:</span><span class="sxs-lookup"><span data-stu-id="69057-159">The example first receives and deletes a message from the `low-messages` subscription by using `:peek_lock` set to **false**, then it receives another message from the `high-messages` and then deletes the message using `delete_subscription_message()`:</span></span>

```ruby
message = azure_service_bus_service.receive_subscription_message(
  "test-topic", "low-messages", { :peek_lock => false })
message = azure_service_bus_service.receive_subscription_message(
  "test-topic", "high-messages")
azure_service_bus_service.delete_subscription_message(message)
```

## <a name="how-to-handle-application-crashes-and-unreadable-messages"></a><span data-ttu-id="69057-160">Hantera programkrascher och oläsbara meddelanden</span><span class="sxs-lookup"><span data-stu-id="69057-160">How to handle application crashes and unreadable messages</span></span>
<span data-ttu-id="69057-161">Service Bus innehåller funktioner som hjälper dig att återställa fel i programmet eller lösa problem med bearbetning av meddelanden på ett snyggt sätt.</span><span class="sxs-lookup"><span data-stu-id="69057-161">Service Bus provides functionality to help you gracefully recover from errors in your application or difficulties processing a message.</span></span> <span data-ttu-id="69057-162">Om ett mottagarprogram går inte att bearbeta meddelandet av någon anledning, så kan det anropa den `unlock_subscription_message()` -metoden i den **Azure::ServiceBusService** objekt.</span><span class="sxs-lookup"><span data-stu-id="69057-162">If a receiver application is unable to process the message for some reason, then it can call the `unlock_subscription_message()` method on the **Azure::ServiceBusService** object.</span></span> <span data-ttu-id="69057-163">Detta gör att Service Bus låser upp meddelandet i prenumerationen och gör det tillgängligt att tas emot igen, antingen genom att samma användningsprogram eller ett annat användningsprogram.</span><span class="sxs-lookup"><span data-stu-id="69057-163">This causes Service Bus to unlock the message within the subscription and make it available to be received again, either by the same consuming application or by another consuming application.</span></span>

<span data-ttu-id="69057-164">Det finns också en tidsgräns som är associerad med ett meddelande som ligger låst i prenumerationen om programmet misslyckas med att bearbeta meddelandet innan timeout för lås går ut (till exempel om programmet kraschar), kommer Service Bus låser upp meddelandet automatiskt och göra det tillgängligt att tas emot igen.</span><span class="sxs-lookup"><span data-stu-id="69057-164">There is also a timeout associated with a message locked within the subscription, and if the application fails to process the message before the lock timeout expires (for example, if the application crashes), then Service Bus will unlock the message automatically and make it available to be received again.</span></span>

<span data-ttu-id="69057-165">I händelse av att programmet kraschar efter att meddelandet har bearbetats men innan den `delete_subscription_message()` metoden anropas sedan meddelandet är levereras på nytt till programmet när den startas om.</span><span class="sxs-lookup"><span data-stu-id="69057-165">In the event that the application crashes after processing the message but before the `delete_subscription_message()` method is called, then the message is redelivered to the application when it restarts.</span></span> <span data-ttu-id="69057-166">Det här kallas ofta *At Least Once Processing*; som är varje meddelande bearbetas minst en gång men i vissa situationer kan samma meddelande levereras.</span><span class="sxs-lookup"><span data-stu-id="69057-166">This is often called *At Least Once Processing*; that is, each message will be processed at least once but in certain situations the same message may be redelivered.</span></span> <span data-ttu-id="69057-167">Om scenariot inte tolererar duplicerad bearbetning, bör programutvecklarna lägga till ytterligare logik i sina program för att hantera duplicerad meddelandeleverans.</span><span class="sxs-lookup"><span data-stu-id="69057-167">If the scenario cannot tolerate duplicate processing, then application developers should add additional logic to their application to handle duplicate message delivery.</span></span> <span data-ttu-id="69057-168">Den här logiken uppnås ofta med hjälp av den `message_id` för meddelandet, som förblir konstant under alla leveransförsök.</span><span class="sxs-lookup"><span data-stu-id="69057-168">This logic is often achieved using the `message_id` property of the message, which will remain constant across delivery attempts.</span></span>

## <a name="delete-topics-and-subscriptions"></a><span data-ttu-id="69057-169">Ta bort ämnen och prenumerationer</span><span class="sxs-lookup"><span data-stu-id="69057-169">Delete topics and subscriptions</span></span>
<span data-ttu-id="69057-170">Ämnen och prenumerationer är beständiga och måste vara explicit bort antingen via den [Azure-portalen] [ Azure portal] eller programmässigt.</span><span class="sxs-lookup"><span data-stu-id="69057-170">Topics and subscriptions are persistent, and must be explicitly deleted either through the [Azure portal][Azure portal] or programmatically.</span></span> <span data-ttu-id="69057-171">Exemplet nedan visar hur du tar bort ämnet med namnet `test-topic`.</span><span class="sxs-lookup"><span data-stu-id="69057-171">The example below demonstrates how to delete the topic named `test-topic`.</span></span>

```ruby
azure_service_bus_service.delete_topic("test-topic")
```

<span data-ttu-id="69057-172">Om du tar bort ett ämne så tar du även bort alla prenumerationer som är registrerade på det ämnet.</span><span class="sxs-lookup"><span data-stu-id="69057-172">Deleting a topic also deletes any subscriptions that are registered with the topic.</span></span> <span data-ttu-id="69057-173">Prenumerationer kan även tas bort separat.</span><span class="sxs-lookup"><span data-stu-id="69057-173">Subscriptions can also be deleted independently.</span></span> <span data-ttu-id="69057-174">Följande kod visar hur du tar bort en prenumeration med namnet `high-messages` från den `test-topic` avsnittet:</span><span class="sxs-lookup"><span data-stu-id="69057-174">The following code demonstrates how to delete the subscription named `high-messages` from the `test-topic` topic:</span></span>

```ruby
azure_service_bus_service.delete_subscription("test-topic", "high-messages")
```

## <a name="next-steps"></a><span data-ttu-id="69057-175">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="69057-175">Next steps</span></span>
<span data-ttu-id="69057-176">Nu när du har lärt dig grunderna om Service Bus-ämnen kan du följa dessa länkar om du vill veta mer.</span><span class="sxs-lookup"><span data-stu-id="69057-176">Now that you've learned the basics of Service Bus topics, follow these links to learn more.</span></span>

* <span data-ttu-id="69057-177">Se [köer, ämnen och prenumerationer](service-bus-queues-topics-subscriptions.md).</span><span class="sxs-lookup"><span data-stu-id="69057-177">See [Queues, topics, and subscriptions](service-bus-queues-topics-subscriptions.md).</span></span>
* <span data-ttu-id="69057-178">API-referens för [SqlFilter](/dotnet/api/microsoft.servicebus.messaging.sqlfilter#microsoft_servicebus_messaging_sqlfilter).</span><span class="sxs-lookup"><span data-stu-id="69057-178">API reference for [SqlFilter](/dotnet/api/microsoft.servicebus.messaging.sqlfilter#microsoft_servicebus_messaging_sqlfilter).</span></span>
* <span data-ttu-id="69057-179">Besök den [Azure SDK för Ruby](https://github.com/Azure/azure-sdk-for-ruby) databasen på GitHub.</span><span class="sxs-lookup"><span data-stu-id="69057-179">Visit the [Azure SDK for Ruby](https://github.com/Azure/azure-sdk-for-ruby) repository on GitHub.</span></span>

[Azure portal]: https://portal.azure.com
