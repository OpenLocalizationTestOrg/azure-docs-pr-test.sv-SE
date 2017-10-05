---
title: "Hur du använder Azure Service Bus-köer med Ruby | Microsoft Docs"
description: "Lär dig hur du använder Service Bus-köer i Azure. Kodexempel som skrivits i Ruby."
services: service-bus-messaging
documentationcenter: ruby
author: sethmanheim
manager: timlt
editor: 
ms.assetid: 0a11eab2-823f-4cc7-842b-fbbe0f953751
ms.service: service-bus-messaging
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: ruby
ms.topic: article
ms.date: 08/10/2017
ms.author: sethm
ms.openlocfilehash: 357a7277dd42b6973cf35a9f642b8eec36a745e3
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/29/2017
---
# <a name="how-to-use-service-bus-queues-with-ruby"></a><span data-ttu-id="dc4cc-104">Hur du använder Service Bus-köer med Ruby</span><span class="sxs-lookup"><span data-stu-id="dc4cc-104">How to use Service Bus queues with Ruby</span></span>

[!INCLUDE [service-bus-selector-queues](../../includes/service-bus-selector-queues.md)]

<span data-ttu-id="dc4cc-105">Den här guiden beskriver hur du använder Service Bus-köer.</span><span class="sxs-lookup"><span data-stu-id="dc4cc-105">This guide describes how to use Service Bus queues.</span></span> <span data-ttu-id="dc4cc-106">Exemplen är skrivna i Ruby och använder Azure symbolen.</span><span class="sxs-lookup"><span data-stu-id="dc4cc-106">The samples are written in Ruby and use the Azure gem.</span></span> <span data-ttu-id="dc4cc-107">Scenarier som tas upp inkluderar **skapa köer, skicka och ta emot meddelanden**, och **tar bort köer**.</span><span class="sxs-lookup"><span data-stu-id="dc4cc-107">The scenarios covered include **creating queues, sending and receiving messages**, and **deleting queues**.</span></span> <span data-ttu-id="dc4cc-108">Mer information om Service Bus-köer finns i [nästa steg](#next-steps) avsnitt.</span><span class="sxs-lookup"><span data-stu-id="dc4cc-108">For more information about Service Bus queues, see the [Next Steps](#next-steps) section.</span></span>

[!INCLUDE [howto-service-bus-queues](../../includes/howto-service-bus-queues.md)]

[!INCLUDE [service-bus-create-namespace-portal](../../includes/service-bus-create-namespace-portal.md)]
   
[!INCLUDE [service-bus-ruby-setup](../../includes/service-bus-ruby-setup.md)]

## <a name="how-to-create-a-queue"></a><span data-ttu-id="dc4cc-109">Så här skapar du en kö</span><span class="sxs-lookup"><span data-stu-id="dc4cc-109">How to create a queue</span></span>
<span data-ttu-id="dc4cc-110">Den **Azure::ServiceBusService** objekt kan du arbeta med köer.</span><span class="sxs-lookup"><span data-stu-id="dc4cc-110">The **Azure::ServiceBusService** object enables you to work with queues.</span></span> <span data-ttu-id="dc4cc-111">Så här skapar du en kö i `create_queue()` metod.</span><span class="sxs-lookup"><span data-stu-id="dc4cc-111">To create a queue, use the `create_queue()` method.</span></span> <span data-ttu-id="dc4cc-112">I följande exempel skapar en kö eller skriver ut eventuella fel.</span><span class="sxs-lookup"><span data-stu-id="dc4cc-112">The following example creates a queue or prints out any errors.</span></span>

```ruby
azure_service_bus_service = Azure::ServiceBus::ServiceBusService.new(sb_host, { signer: signer})
begin
  queue = azure_service_bus_service.create_queue("test-queue")
rescue
  puts $!
end
```

<span data-ttu-id="dc4cc-113">Du kan också skicka en **Azure::ServiceBus::Queue** objekt med ytterligare alternativ som gör att du åsidosätter standardinställningarna för kön, till exempel meddelande tid till live eller maximal köstorlek.</span><span class="sxs-lookup"><span data-stu-id="dc4cc-113">You can also pass a **Azure::ServiceBus::Queue** object with additional options, which enables you to override the default queue settings, such as message time to live or maximum queue size.</span></span> <span data-ttu-id="dc4cc-114">I följande exempel visas hur du anger den maximala storleken till 5 GB och -tid live till 1 minut:</span><span class="sxs-lookup"><span data-stu-id="dc4cc-114">The following example shows how to set the maximum queue size to 5 GB and time to live to 1 minute:</span></span>

```ruby
queue = Azure::ServiceBus::Queue.new("test-queue")
queue.max_size_in_megabytes = 5120
queue.default_message_time_to_live = "PT1M"

queue = azure_service_bus_service.create_queue(queue)
```

## <a name="how-to-send-messages-to-a-queue"></a><span data-ttu-id="dc4cc-115">Hur du skickar meddelanden till en kö</span><span class="sxs-lookup"><span data-stu-id="dc4cc-115">How to send messages to a queue</span></span>
<span data-ttu-id="dc4cc-116">Att skicka ett meddelande till en Service Bus-kö, program-anrop i `send_queue_message()` -metoden i den **Azure::ServiceBusService** objekt.</span><span class="sxs-lookup"><span data-stu-id="dc4cc-116">To send a message to a Service Bus queue, your application calls the `send_queue_message()` method on the **Azure::ServiceBusService** object.</span></span> <span data-ttu-id="dc4cc-117">Meddelanden skickas till (och mottagna från) Service Bus är köer **Azure::ServiceBus::BrokeredMessage** objekt och har en uppsättning standardegenskaper (t.ex `label` och `time_to_live`), en ordlista som används för att lagra anpassade programspecifika egenskaper och en brödtext med godtyckliga programdata.</span><span class="sxs-lookup"><span data-stu-id="dc4cc-117">Messages sent to (and received from) Service Bus queues are **Azure::ServiceBus::BrokeredMessage** objects, and have a set of standard properties (such as `label` and `time_to_live`), a dictionary that is used to hold custom application-specific properties, and a body of arbitrary application data.</span></span> <span data-ttu-id="dc4cc-118">Ett program kan konfigurera meddelandets brödtext för meddelandet genom att skicka ett strängvärde som meddelandet och eventuella obligatoriska standard egenskaper fylls i med standardvärden.</span><span class="sxs-lookup"><span data-stu-id="dc4cc-118">An application can set the body of the message by passing a string value as the message and any required standard properties are populated with default values.</span></span>

<span data-ttu-id="dc4cc-119">I följande exempel visar hur du skickar ett testmeddelande till kön med namnet `test-queue` med `send_queue_message()`:</span><span class="sxs-lookup"><span data-stu-id="dc4cc-119">The following example demonstrates how to send a test message to the queue named `test-queue` using `send_queue_message()`:</span></span>

```ruby
message = Azure::ServiceBus::BrokeredMessage.new("test queue message")
message.correlation_id = "test-correlation-id"
azure_service_bus_service.send_queue_message("test-queue", message)
```

<span data-ttu-id="dc4cc-120">Service Bus-köerna stöder en maximal meddelandestorlek på 256 kB på [standardnivån](service-bus-premium-messaging.md) och 1 MB på [premiumnivån](service-bus-premium-messaging.md).</span><span class="sxs-lookup"><span data-stu-id="dc4cc-120">Service Bus queues support a maximum message size of 256 KB in the [Standard tier](service-bus-premium-messaging.md) and 1 MB in the [Premium tier](service-bus-premium-messaging.md).</span></span> <span data-ttu-id="dc4cc-121">Rubriken, som inkluderar standardprogramegenskaperna och de anpassade programegenskaperna, kan ha en maximal storlek på 64 kB.</span><span class="sxs-lookup"><span data-stu-id="dc4cc-121">The header, which includes the standard and custom application properties, can have a maximum size of 64 KB.</span></span> <span data-ttu-id="dc4cc-122">Det finns ingen gräns för antalet meddelanden som kan finnas i en kö men det finns ett tak för den totala storleken för de meddelanden som ligger i en kö.</span><span class="sxs-lookup"><span data-stu-id="dc4cc-122">There is no limit on the number of messages held in a queue but there is a cap on the total size of the messages held by a queue.</span></span> <span data-ttu-id="dc4cc-123">Den här köstorleken definieras när kön skapas, med en övre gräns på 5 GB.</span><span class="sxs-lookup"><span data-stu-id="dc4cc-123">This queue size is defined at creation time, with an upper limit of 5 GB.</span></span>

## <a name="how-to-receive-messages-from-a-queue"></a><span data-ttu-id="dc4cc-124">Ta emot meddelanden från en kö</span><span class="sxs-lookup"><span data-stu-id="dc4cc-124">How to receive messages from a queue</span></span>
<span data-ttu-id="dc4cc-125">Meddelanden tas emot från en kö med hjälp av den `receive_queue_message()` -metoden i den **Azure::ServiceBusService** objekt.</span><span class="sxs-lookup"><span data-stu-id="dc4cc-125">Messages are received from a queue using the `receive_queue_message()` method on the **Azure::ServiceBusService** object.</span></span> <span data-ttu-id="dc4cc-126">Som standard läsa meddelanden och låst utan tas bort från kön.</span><span class="sxs-lookup"><span data-stu-id="dc4cc-126">By default, messages are read and locked without being deleted from the queue.</span></span> <span data-ttu-id="dc4cc-127">Du kan dock ta bort meddelanden från kön när de är skrivskyddade genom att ange den `:peek_lock` att **FALSKT**.</span><span class="sxs-lookup"><span data-stu-id="dc4cc-127">However, you can delete messages from the queue as they are read by setting the `:peek_lock` option to **false**.</span></span>

<span data-ttu-id="dc4cc-128">Standardbeteendet gör läsningen och ta bort en åtgärd i två steg, vilket gör det också möjligt att stödja program som inte tolererar att ett meddelande saknas.</span><span class="sxs-lookup"><span data-stu-id="dc4cc-128">The default behavior makes the reading and deleting a two-stage operation, which also makes it possible to support applications that cannot tolerate missing messages.</span></span> <span data-ttu-id="dc4cc-129">När Service Bus tar emot en begäran letar det upp nästa meddelande som ska förbrukas, låser det för att förhindra att andra användare tar emot det och skickar sedan tillbaka det till programmet.</span><span class="sxs-lookup"><span data-stu-id="dc4cc-129">When Service Bus receives a request, it finds the next message to be consumed, locks it to prevent other consumers receiving it, and then returns it to the application.</span></span> <span data-ttu-id="dc4cc-130">När programmet har avslutat bearbetningen av meddelandet (eller lagrar det på ett tillförlitligt sätt för framtida bearbetning), Slutför det andra steget i processen genom att anropa `delete_queue_message()` metod och ge meddelandet som ska tas bort som en parameter.</span><span class="sxs-lookup"><span data-stu-id="dc4cc-130">After the application finishes processing the message (or stores it reliably for future processing), it completes the second stage of the receive process by calling `delete_queue_message()` method and providing the message to be deleted as a parameter.</span></span> <span data-ttu-id="dc4cc-131">Den `delete_queue_message()` metoden att markera meddelandet som Förbrukat och tas bort från kön.</span><span class="sxs-lookup"><span data-stu-id="dc4cc-131">The `delete_queue_message()` method will mark the message as being consumed and remove it from the queue.</span></span>

<span data-ttu-id="dc4cc-132">Om den `:peek_lock` parametern anges till **FALSKT**, läser och tar bort meddelandet blir den enklaste modellen och fungerar bäst för scenarier där ett program kan tolerera icke-bearbetning av ett meddelande om ett fel inträffar.</span><span class="sxs-lookup"><span data-stu-id="dc4cc-132">If the `:peek_lock` parameter is set to **false**, reading and deleting the message becomes the simplest model, and works best for scenarios in which an application can tolerate not processing a message in the event of a failure.</span></span> <span data-ttu-id="dc4cc-133">För att förstå detta kan du föreställa dig ett scenario där konsumenten utfärdar en receive-begäran och sedan kraschar innan den kan bearbeta denna begäran.</span><span class="sxs-lookup"><span data-stu-id="dc4cc-133">To understand this, consider a scenario in which the consumer issues the receive request and then crashes before processing it.</span></span> <span data-ttu-id="dc4cc-134">Eftersom Service Bus har markerat meddelandet som Förbrukat när programmet startas om och börjar förbruka meddelanden igen, kommer det ha missat meddelandet som förbrukades innan kraschen.</span><span class="sxs-lookup"><span data-stu-id="dc4cc-134">Because Service Bus has marked the message as being consumed, when the application restarts and begins consuming messages again, it will have missed the message that was consumed prior to the crash.</span></span>

<span data-ttu-id="dc4cc-135">Exemplet nedan visar hur du tar emot och bearbetar meddelanden med `receive_queue_message()`.</span><span class="sxs-lookup"><span data-stu-id="dc4cc-135">The following example demonstrates how to receive and process messages using `receive_queue_message()`.</span></span> <span data-ttu-id="dc4cc-136">I exempel först tar emot och tar bort ett meddelande med hjälp av `:peek_lock` inställd på **FALSKT**, därefter tar emot ett nytt meddelande och sedan tar bort meddelande med `delete_queue_message()`:</span><span class="sxs-lookup"><span data-stu-id="dc4cc-136">The example first receives and deletes a message by using `:peek_lock` set to **false**, then it receives another message and then deletes the message using `delete_queue_message()`:</span></span>

```ruby
message = azure_service_bus_service.receive_queue_message("test-queue",
  { :peek_lock => false })
message = azure_service_bus_service.receive_queue_message("test-queue")
azure_service_bus_service.delete_queue_message(message)
```

## <a name="how-to-handle-application-crashes-and-unreadable-messages"></a><span data-ttu-id="dc4cc-137">Hantera programkrascher och oläsbara meddelanden</span><span class="sxs-lookup"><span data-stu-id="dc4cc-137">How to handle application crashes and unreadable messages</span></span>
<span data-ttu-id="dc4cc-138">Service Bus innehåller funktioner som hjälper dig att återställa fel i programmet eller lösa problem med bearbetning av meddelanden på ett snyggt sätt.</span><span class="sxs-lookup"><span data-stu-id="dc4cc-138">Service Bus provides functionality to help you gracefully recover from errors in your application or difficulties processing a message.</span></span> <span data-ttu-id="dc4cc-139">Om ett mottagarprogram går inte att bearbeta meddelandet av någon anledning, så kan det anropa den `unlock_queue_message()` -metoden i den **Azure::ServiceBusService** objekt.</span><span class="sxs-lookup"><span data-stu-id="dc4cc-139">If a receiver application is unable to process the message for some reason, then it can call the `unlock_queue_message()` method on the **Azure::ServiceBusService** object.</span></span> <span data-ttu-id="dc4cc-140">Detta anrop gör Service Bus låser upp meddelandet i kön och gör det tillgängligt att tas emot igen, antingen genom samma användningsprogram eller ett annat användningsprogram.</span><span class="sxs-lookup"><span data-stu-id="dc4cc-140">This call causes Service Bus to unlock the message within the queue and make it available to be received again, either by the same consuming application or by another consuming application.</span></span>

<span data-ttu-id="dc4cc-141">Det finns också en tidsgräns som är associerad med ett meddelande som ligger låst i kön och om programmet misslyckas med att bearbeta meddelandet innan timeout för lås går ut (till exempel om programmet kraschar), kommer Service Bus låser upp meddelandet automatiskt och gör det tillgängligt att tas emot igen.</span><span class="sxs-lookup"><span data-stu-id="dc4cc-141">There is also a timeout associated with a message locked within the queue, and if the application fails to process the message before the lock timeout expires (for example, if the application crashes), then Service Bus unlocks the message automatically and makes it available to be received again.</span></span>

<span data-ttu-id="dc4cc-142">I händelse av att programmet kraschar efter att meddelandet har bearbetats men innan den `delete_queue_message()` metoden anropas sedan meddelandet är levereras på nytt till programmet när den startas om.</span><span class="sxs-lookup"><span data-stu-id="dc4cc-142">In the event that the application crashes after processing the message but before the `delete_queue_message()` method is called, then the message is redelivered to the application when it restarts.</span></span> <span data-ttu-id="dc4cc-143">Den här processen kallas ofta *At Least Once Processing*, det vill säga varje meddelande bearbetas minst en gång men i vissa situationer kan samma meddelande levereras.</span><span class="sxs-lookup"><span data-stu-id="dc4cc-143">This process is often called *At Least Once Processing*; that is, each message is processed at least once but in certain situations the same message may be redelivered.</span></span> <span data-ttu-id="dc4cc-144">Om scenariot inte tolererar duplicerad bearbetning, bör programutvecklarna lägga till ytterligare logik i sina program för att hantera duplicerad meddelandeleverans.</span><span class="sxs-lookup"><span data-stu-id="dc4cc-144">If the scenario cannot tolerate duplicate processing, then application developers should add additional logic to their application to handle duplicate message delivery.</span></span> <span data-ttu-id="dc4cc-145">Detta uppnås ofta med hjälp av den `message_id` för meddelandet, som förblir konstant under alla leveransförsök.</span><span class="sxs-lookup"><span data-stu-id="dc4cc-145">This is often achieved using the `message_id` property of the message, which remains constant across delivery attempts.</span></span>

## <a name="next-steps"></a><span data-ttu-id="dc4cc-146">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="dc4cc-146">Next steps</span></span>
<span data-ttu-id="dc4cc-147">Nu när du har lärt dig grunderna om Service Bus-köer, kan du följa dessa länkar om du vill veta mer.</span><span class="sxs-lookup"><span data-stu-id="dc4cc-147">Now that you've learned the basics of Service Bus queues, follow these links to learn more.</span></span>

* <span data-ttu-id="dc4cc-148">Översikt över [köer, ämnen och prenumerationer](service-bus-queues-topics-subscriptions.md).</span><span class="sxs-lookup"><span data-stu-id="dc4cc-148">Overview of [queues, topics, and subscriptions](service-bus-queues-topics-subscriptions.md).</span></span>
* <span data-ttu-id="dc4cc-149">Besök den [Azure SDK för Ruby](https://github.com/Azure/azure-sdk-for-ruby) databasen på GitHub.</span><span class="sxs-lookup"><span data-stu-id="dc4cc-149">Visit the [Azure SDK for Ruby](https://github.com/Azure/azure-sdk-for-ruby) repository on GitHub.</span></span>

<span data-ttu-id="dc4cc-150">En jämförelse mellan Azure Service Bus-köer som beskrivs i den här artikeln och Azure-köer som beskrivs i den [använda Queue storage från Ruby](../storage/queues/storage-ruby-how-to-use-queue-storage.md) artikel, se [Azure köer och Azure Service Bus-köer - jämfört med och Skillnad från](service-bus-azure-and-service-bus-queues-compared-contrasted.md)</span><span class="sxs-lookup"><span data-stu-id="dc4cc-150">For a comparison between the Azure Service Bus queues discussed in this article and Azure Queues discussed in the [How to use Queue storage from Ruby](../storage/queues/storage-ruby-how-to-use-queue-storage.md) article, see [Azure Queues and Azure Service Bus Queues - Compared and Contrasted](service-bus-azure-and-service-bus-queues-compared-contrasted.md)</span></span>

