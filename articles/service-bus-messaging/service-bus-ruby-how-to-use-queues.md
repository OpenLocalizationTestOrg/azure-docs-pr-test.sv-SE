---
title: "aaaHow toouse Azure Service Bus-köer med Ruby | Microsoft Docs"
description: "Lär dig hur toouse Service Bus köer i Azure. Kodexempel som skrivits i Ruby."
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
ms.openlocfilehash: 7270154583f98e3372e82efbb967ea7a5acd1686
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="how-toouse-service-bus-queues-with-ruby"></a><span data-ttu-id="610fe-104">Hur toouse Service Bus köer med Ruby</span><span class="sxs-lookup"><span data-stu-id="610fe-104">How toouse Service Bus queues with Ruby</span></span>

[!INCLUDE [service-bus-selector-queues](../../includes/service-bus-selector-queues.md)]

<span data-ttu-id="610fe-105">Den här guiden beskriver hur toouse Service Bus-köer.</span><span class="sxs-lookup"><span data-stu-id="610fe-105">This guide describes how toouse Service Bus queues.</span></span> <span data-ttu-id="610fe-106">hello exemplen är skrivna i Ruby och använder hello Azure symbolen.</span><span class="sxs-lookup"><span data-stu-id="610fe-106">hello samples are written in Ruby and use hello Azure gem.</span></span> <span data-ttu-id="610fe-107">hello beskrivs scenarier där **skapa köer, skicka och ta emot meddelanden**, och **tar bort köer**.</span><span class="sxs-lookup"><span data-stu-id="610fe-107">hello scenarios covered include **creating queues, sending and receiving messages**, and **deleting queues**.</span></span> <span data-ttu-id="610fe-108">Mer information om Service Bus-köer finns hello [nästa steg](#next-steps) avsnitt.</span><span class="sxs-lookup"><span data-stu-id="610fe-108">For more information about Service Bus queues, see hello [Next Steps](#next-steps) section.</span></span>

[!INCLUDE [howto-service-bus-queues](../../includes/howto-service-bus-queues.md)]

[!INCLUDE [service-bus-create-namespace-portal](../../includes/service-bus-create-namespace-portal.md)]
   
[!INCLUDE [service-bus-ruby-setup](../../includes/service-bus-ruby-setup.md)]

## <a name="how-toocreate-a-queue"></a><span data-ttu-id="610fe-109">Hur toocreate en kö</span><span class="sxs-lookup"><span data-stu-id="610fe-109">How toocreate a queue</span></span>
<span data-ttu-id="610fe-110">Hej **Azure::ServiceBusService** objekt kan du toowork med köer.</span><span class="sxs-lookup"><span data-stu-id="610fe-110">hello **Azure::ServiceBusService** object enables you toowork with queues.</span></span> <span data-ttu-id="610fe-111">toocreate en kö, använda hello `create_queue()` metod.</span><span class="sxs-lookup"><span data-stu-id="610fe-111">toocreate a queue, use hello `create_queue()` method.</span></span> <span data-ttu-id="610fe-112">hello följande exempel skapar en kö eller skriver ut eventuella fel.</span><span class="sxs-lookup"><span data-stu-id="610fe-112">hello following example creates a queue or prints out any errors.</span></span>

```ruby
azure_service_bus_service = Azure::ServiceBus::ServiceBusService.new(sb_host, { signer: signer})
begin
  queue = azure_service_bus_service.create_queue("test-queue")
rescue
  puts $!
end
```

<span data-ttu-id="610fe-113">Du kan också skicka en **Azure::ServiceBus::Queue** objekt med ytterligare alternativ som gör att du toooverride hello kön standardinställningarna, till exempel meddelande tid toolive eller maximal köstorlek.</span><span class="sxs-lookup"><span data-stu-id="610fe-113">You can also pass a **Azure::ServiceBus::Queue** object with additional options, which enables you toooverride hello default queue settings, such as message time toolive or maximum queue size.</span></span> <span data-ttu-id="610fe-114">hello följande exempel visas hur tooset hello största köstorlek storlek too5 GB och tid toolive too1 minut:</span><span class="sxs-lookup"><span data-stu-id="610fe-114">hello following example shows how tooset hello maximum queue size too5 GB and time toolive too1 minute:</span></span>

```ruby
queue = Azure::ServiceBus::Queue.new("test-queue")
queue.max_size_in_megabytes = 5120
queue.default_message_time_to_live = "PT1M"

queue = azure_service_bus_service.create_queue(queue)
```

## <a name="how-toosend-messages-tooa-queue"></a><span data-ttu-id="610fe-115">Hur toosend meddelanden tooa kön</span><span class="sxs-lookup"><span data-stu-id="610fe-115">How toosend messages tooa queue</span></span>
<span data-ttu-id="610fe-116">toosend en meddelandekö tooa Service Bus programmet anropar hello `send_queue_message()` metod på hello **Azure::ServiceBusService** objekt.</span><span class="sxs-lookup"><span data-stu-id="610fe-116">toosend a message tooa Service Bus queue, your application calls hello `send_queue_message()` method on hello **Azure::ServiceBusService** object.</span></span> <span data-ttu-id="610fe-117">Meddelanden skickas för (och mottagna från) Service Bus är köer **Azure::ServiceBus::BrokeredMessage** objekt och har en uppsättning standardegenskaper (t.ex `label` och `time_to_live`), en ordlista som används toohold anpassade programspecifika egenskaper och en brödtext med godtyckliga programdata.</span><span class="sxs-lookup"><span data-stu-id="610fe-117">Messages sent too(and received from) Service Bus queues are **Azure::ServiceBus::BrokeredMessage** objects, and have a set of standard properties (such as `label` and `time_to_live`), a dictionary that is used toohold custom application-specific properties, and a body of arbitrary application data.</span></span> <span data-ttu-id="610fe-118">Ett program kan konfigurera hello brödtext hello-meddelande genom att skicka ett strängvärde som hello-meddelande och eventuella obligatoriska standard egenskaper fylls i med standardvärden.</span><span class="sxs-lookup"><span data-stu-id="610fe-118">An application can set hello body of hello message by passing a string value as hello message and any required standard properties are populated with default values.</span></span>

<span data-ttu-id="610fe-119">hello exemplet nedan visar hur toosend en toohello meddelandekö test med namnet `test-queue` med `send_queue_message()`:</span><span class="sxs-lookup"><span data-stu-id="610fe-119">hello following example demonstrates how toosend a test message toohello queue named `test-queue` using `send_queue_message()`:</span></span>

```ruby
message = Azure::ServiceBus::BrokeredMessage.new("test queue message")
message.correlation_id = "test-correlation-id"
azure_service_bus_service.send_queue_message("test-queue", message)
```

<span data-ttu-id="610fe-120">Service Bus-köer stöder en maximal meddelandestorlek på 256 KB i hello [standardnivån](service-bus-premium-messaging.md) och 1 MB på hello [premiumnivån](service-bus-premium-messaging.md).</span><span class="sxs-lookup"><span data-stu-id="610fe-120">Service Bus queues support a maximum message size of 256 KB in hello [Standard tier](service-bus-premium-messaging.md) and 1 MB in hello [Premium tier](service-bus-premium-messaging.md).</span></span> <span data-ttu-id="610fe-121">hello-huvudet som innehåller hello standard- och anpassade programegenskaperna, kan ha en maximal storlek på 64 KB.</span><span class="sxs-lookup"><span data-stu-id="610fe-121">hello header, which includes hello standard and custom application properties, can have a maximum size of 64 KB.</span></span> <span data-ttu-id="610fe-122">Det finns ingen gräns hello antalet meddelanden som finns i en kö men det finns ett tak på hello totala storleken på hälsningsmeddelande som innehas av en kö.</span><span class="sxs-lookup"><span data-stu-id="610fe-122">There is no limit on hello number of messages held in a queue but there is a cap on hello total size of hello messages held by a queue.</span></span> <span data-ttu-id="610fe-123">Den här köstorleken definieras när kön skapas, med en övre gräns på 5 GB.</span><span class="sxs-lookup"><span data-stu-id="610fe-123">This queue size is defined at creation time, with an upper limit of 5 GB.</span></span>

## <a name="how-tooreceive-messages-from-a-queue"></a><span data-ttu-id="610fe-124">Hur tooreceive meddelanden från en kö</span><span class="sxs-lookup"><span data-stu-id="610fe-124">How tooreceive messages from a queue</span></span>
<span data-ttu-id="610fe-125">Meddelanden tas emot från en kö med hello `receive_queue_message()` metod på hello **Azure::ServiceBusService** objekt.</span><span class="sxs-lookup"><span data-stu-id="610fe-125">Messages are received from a queue using hello `receive_queue_message()` method on hello **Azure::ServiceBusService** object.</span></span> <span data-ttu-id="610fe-126">Som standard, läsa och låst utan tas bort från kön hello meddelanden.</span><span class="sxs-lookup"><span data-stu-id="610fe-126">By default, messages are read and locked without being deleted from hello queue.</span></span> <span data-ttu-id="610fe-127">Du kan dock ta bort meddelanden från kön hello som de läses av inställningen hello `:peek_lock` alternativet för**FALSKT**.</span><span class="sxs-lookup"><span data-stu-id="610fe-127">However, you can delete messages from hello queue as they are read by setting hello `:peek_lock` option too**false**.</span></span>

<span data-ttu-id="610fe-128">hello standardbeteendet gör hello läser och tar bort en åtgärd i två steg, vilket gör det också möjligt toosupport program som inte tolererar att ett meddelande saknas.</span><span class="sxs-lookup"><span data-stu-id="610fe-128">hello default behavior makes hello reading and deleting a two-stage operation, which also makes it possible toosupport applications that cannot tolerate missing messages.</span></span> <span data-ttu-id="610fe-129">När Service Bus tar emot en begäran, hittar hello nästa meddelande toobe förbrukas, låser det tooprevent andra användare tar emot det och returnerar sedan toohello program.</span><span class="sxs-lookup"><span data-stu-id="610fe-129">When Service Bus receives a request, it finds hello next message toobe consumed, locks it tooprevent other consumers receiving it, and then returns it toohello application.</span></span> <span data-ttu-id="610fe-130">När hello programmet har avslutat bearbetningen hello-meddelande (eller lagrar det på ett tillförlitligt sätt för framtida bearbetning), Slutför hello andra steget i hello får processen genom att anropa `delete_queue_message()` metod och ge hello meddelandet toobe tas bort som en parameter.</span><span class="sxs-lookup"><span data-stu-id="610fe-130">After hello application finishes processing hello message (or stores it reliably for future processing), it completes hello second stage of hello receive process by calling `delete_queue_message()` method and providing hello message toobe deleted as a parameter.</span></span> <span data-ttu-id="610fe-131">Hej `delete_queue_message()` metoden kommer Markera hello meddelandet som Förbrukat och ta bort den från hello kö.</span><span class="sxs-lookup"><span data-stu-id="610fe-131">hello `delete_queue_message()` method will mark hello message as being consumed and remove it from hello queue.</span></span>

<span data-ttu-id="610fe-132">Om hello `:peek_lock` parameter har angetts för**FALSKT**, läser och tar bort hello-meddelande blir hello enklaste modellen och fungerar bäst för scenarier där ett program kan tolerera icke-bearbetning av ett meddelande i hello-händelse en ett fel uppstod.</span><span class="sxs-lookup"><span data-stu-id="610fe-132">If hello `:peek_lock` parameter is set too**false**, reading and deleting hello message becomes hello simplest model, and works best for scenarios in which an application can tolerate not processing a message in hello event of a failure.</span></span> <span data-ttu-id="610fe-133">toounderstand, Föreställ i vilka hello konsumenten problem hello mottagning av begäran och sedan kraschar innan bearbetningen.</span><span class="sxs-lookup"><span data-stu-id="610fe-133">toounderstand this, consider a scenario in which hello consumer issues hello receive request and then crashes before processing it.</span></span> <span data-ttu-id="610fe-134">Eftersom Service Bus har markerats hello-meddelande som används när hello programmet startas om och börjar förbruka meddelanden igen, att ha missat hello-meddelande som har förbrukats tidigare toohello krascher.</span><span class="sxs-lookup"><span data-stu-id="610fe-134">Because Service Bus has marked hello message as being consumed, when hello application restarts and begins consuming messages again, it will have missed hello message that was consumed prior toohello crash.</span></span>

<span data-ttu-id="610fe-135">hello exemplet nedan visar hur tooreceive och bearbeta meddelanden med hjälp av `receive_queue_message()`.</span><span class="sxs-lookup"><span data-stu-id="610fe-135">hello following example demonstrates how tooreceive and process messages using `receive_queue_message()`.</span></span> <span data-ttu-id="610fe-136">hello exempel först tar emot och tar bort ett meddelande med hjälp av `:peek_lock` ställa in också**FALSKT**, den får ett nytt meddelande och sedan tar bort hello meddelande med `delete_queue_message()`:</span><span class="sxs-lookup"><span data-stu-id="610fe-136">hello example first receives and deletes a message by using `:peek_lock` set too**false**, then it receives another message and then deletes hello message using `delete_queue_message()`:</span></span>

```ruby
message = azure_service_bus_service.receive_queue_message("test-queue",
  { :peek_lock => false })
message = azure_service_bus_service.receive_queue_message("test-queue")
azure_service_bus_service.delete_queue_message(message)
```

## <a name="how-toohandle-application-crashes-and-unreadable-messages"></a><span data-ttu-id="610fe-137">Hur toohandle programmet kraschar och oläsbara meddelanden</span><span class="sxs-lookup"><span data-stu-id="610fe-137">How toohandle application crashes and unreadable messages</span></span>
<span data-ttu-id="610fe-138">Service Bus innehåller funktioner toohelp du att återställa fel i programmet eller problem med bearbetning av ett meddelande.</span><span class="sxs-lookup"><span data-stu-id="610fe-138">Service Bus provides functionality toohelp you gracefully recover from errors in your application or difficulties processing a message.</span></span> <span data-ttu-id="610fe-139">Om ett mottagarprogram inte kan tooprocess hello meddelande av någon anledning, så kan det anropa hello `unlock_queue_message()` metod på hello **Azure::ServiceBusService** objekt.</span><span class="sxs-lookup"><span data-stu-id="610fe-139">If a receiver application is unable tooprocess hello message for some reason, then it can call hello `unlock_queue_message()` method on hello **Azure::ServiceBusService** object.</span></span> <span data-ttu-id="610fe-140">Detta medför att anropa Service Bus toounlock hello meddelandet i kön för hello och gör den tillgänglig toobe tas emot igen, hello antingen av samma förbrukning av ett program eller ett annat användningsprogram.</span><span class="sxs-lookup"><span data-stu-id="610fe-140">This call causes Service Bus toounlock hello message within hello queue and make it available toobe received again, either by hello same consuming application or by another consuming application.</span></span>

<span data-ttu-id="610fe-141">Det finns också en tidsgräns som är associerad med ett meddelande i kön hello och om hello program inte tooprocess hello-meddelande innan timeout för lås hello går ut (till exempel om hello programmet kraschar), kommer Service Bus låser upp hello-meddelande automatiskt och gör den tillgänglig toobe tas emot igen.</span><span class="sxs-lookup"><span data-stu-id="610fe-141">There is also a timeout associated with a message locked within hello queue, and if hello application fails tooprocess hello message before hello lock timeout expires (for example, if hello application crashes), then Service Bus unlocks hello message automatically and makes it available toobe received again.</span></span>

<span data-ttu-id="610fe-142">Hello händelse som hello programmet kraschar efter hello meddelandet har bearbetats men innan hello `delete_queue_message()` metoden anropas sedan hello-meddelande är toohello levereras på nytt program när den startas om.</span><span class="sxs-lookup"><span data-stu-id="610fe-142">In hello event that hello application crashes after processing hello message but before hello `delete_queue_message()` method is called, then hello message is redelivered toohello application when it restarts.</span></span> <span data-ttu-id="610fe-143">Den här processen kallas ofta *At Least Once Processing*, det vill säga varje meddelande bearbetas minst en gång men i vissa situationer hello samma meddelande kan levereras.</span><span class="sxs-lookup"><span data-stu-id="610fe-143">This process is often called *At Least Once Processing*; that is, each message is processed at least once but in certain situations hello same message may be redelivered.</span></span> <span data-ttu-id="610fe-144">Om hello scenariot inte tolererar duplicerad bearbetning, sedan bör programutvecklarna lägga till ytterligare logik tootheir duplicerad meddelandeleverans för programmet toohandle.</span><span class="sxs-lookup"><span data-stu-id="610fe-144">If hello scenario cannot tolerate duplicate processing, then application developers should add additional logic tootheir application toohandle duplicate message delivery.</span></span> <span data-ttu-id="610fe-145">Detta uppnås ofta med hjälp av hello `message_id` -egenskapen för hello-meddelande, förblir konstant under alla leveransförsök.</span><span class="sxs-lookup"><span data-stu-id="610fe-145">This is often achieved using hello `message_id` property of hello message, which remains constant across delivery attempts.</span></span>

## <a name="next-steps"></a><span data-ttu-id="610fe-146">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="610fe-146">Next steps</span></span>
<span data-ttu-id="610fe-147">Nu när du har lärt dig grunderna hello i Service Bus-köer, följa dessa länkar toolearn mer.</span><span class="sxs-lookup"><span data-stu-id="610fe-147">Now that you've learned hello basics of Service Bus queues, follow these links toolearn more.</span></span>

* <span data-ttu-id="610fe-148">Översikt över [köer, ämnen och prenumerationer](service-bus-queues-topics-subscriptions.md).</span><span class="sxs-lookup"><span data-stu-id="610fe-148">Overview of [queues, topics, and subscriptions](service-bus-queues-topics-subscriptions.md).</span></span>
* <span data-ttu-id="610fe-149">Besök hello [Azure SDK för Ruby](https://github.com/Azure/azure-sdk-for-ruby) databasen på GitHub.</span><span class="sxs-lookup"><span data-stu-id="610fe-149">Visit hello [Azure SDK for Ruby](https://github.com/Azure/azure-sdk-for-ruby) repository on GitHub.</span></span>

<span data-ttu-id="610fe-150">En jämförelse mellan hello Azure Service Bus-köer i den här artikeln och Azure-köer som beskrivs i hello [hur toouse Queue storage från Ruby](../storage/queues/storage-ruby-how-to-use-queue-storage.md) artikel, se [Azure köer och Azure Service Bus-köer - jämfört med och skillnad från](service-bus-azure-and-service-bus-queues-compared-contrasted.md)</span><span class="sxs-lookup"><span data-stu-id="610fe-150">For a comparison between hello Azure Service Bus queues discussed in this article and Azure Queues discussed in hello [How toouse Queue storage from Ruby](../storage/queues/storage-ruby-how-to-use-queue-storage.md) article, see [Azure Queues and Azure Service Bus Queues - Compared and Contrasted](service-bus-azure-and-service-bus-queues-compared-contrasted.md)</span></span>

