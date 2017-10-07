---
title: "aaaHow toouse Azure Service Bus-köer med Python | Microsoft Docs"
description: "Lär dig hur toouse Azure Service Bus köer från Python."
services: service-bus-messaging
documentationcenter: python
author: sethmanheim
manager: timlt
editor: 
ms.assetid: b95ee5cd-3b31-459c-a7f3-cf8bcf77858b
ms.service: service-bus-messaging
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: python
ms.topic: article
ms.date: 08/10/2017
ms.author: sethm;lmazuel
ms.openlocfilehash: bceb84d04ff3445c3087a9c246c583d6630f07af
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="how-toouse-service-bus-queues-with-python"></a><span data-ttu-id="b7681-103">Hur toouse Service Bus köer med Python</span><span class="sxs-lookup"><span data-stu-id="b7681-103">How toouse Service Bus queues with Python</span></span>

[!INCLUDE [service-bus-selector-queues](../../includes/service-bus-selector-queues.md)]

<span data-ttu-id="b7681-104">Den här artikeln beskriver hur toouse Service Bus-köer.</span><span class="sxs-lookup"><span data-stu-id="b7681-104">This article describes how toouse Service Bus queues.</span></span> <span data-ttu-id="b7681-105">hello exemplen är skrivna i Python och använder hello [Python Azure Service Bus-paketet][Python Azure Service Bus package].</span><span class="sxs-lookup"><span data-stu-id="b7681-105">hello samples are written in Python and use hello [Python Azure Service Bus package][Python Azure Service Bus package].</span></span> <span data-ttu-id="b7681-106">hello beskrivs scenarier där **skapa köer, skicka och ta emot meddelanden**, och **tar bort köer**.</span><span class="sxs-lookup"><span data-stu-id="b7681-106">hello scenarios covered include **creating queues, sending and receiving messages**, and **deleting queues**.</span></span>

[!INCLUDE [howto-service-bus-queues](../../includes/howto-service-bus-queues.md)]

[!INCLUDE [service-bus-create-namespace-portal](../../includes/service-bus-create-namespace-portal.md)]

> [!NOTE]
> <span data-ttu-id="b7681-107">tooinstall Python eller hello [Python Azure Service Bus-paketet][Python Azure Service Bus package], se hello [Python installationsguiden](../python-how-to-install.md).</span><span class="sxs-lookup"><span data-stu-id="b7681-107">tooinstall Python or hello [Python Azure Service Bus package][Python Azure Service Bus package], see hello [Python Installation Guide](../python-how-to-install.md).</span></span>
> 
> 

## <a name="create-a-queue"></a><span data-ttu-id="b7681-108">Skapa en kö</span><span class="sxs-lookup"><span data-stu-id="b7681-108">Create a queue</span></span>
<span data-ttu-id="b7681-109">Hej **ServiceBusService** objekt kan du toowork med köer.</span><span class="sxs-lookup"><span data-stu-id="b7681-109">hello **ServiceBusService** object enables you toowork with queues.</span></span> <span data-ttu-id="b7681-110">Lägg till följande kod hello övre delen av Python-fil som du vill tooprogrammatically access Service Bus hello:</span><span class="sxs-lookup"><span data-stu-id="b7681-110">Add hello following code near hello top of any Python file in which you wish tooprogrammatically access Service Bus:</span></span>

```python
from azure.servicebus import ServiceBusService, Message, Queue
```

<span data-ttu-id="b7681-111">hello följande kod skapar en **ServiceBusService** objekt.</span><span class="sxs-lookup"><span data-stu-id="b7681-111">hello following code creates a **ServiceBusService** object.</span></span> <span data-ttu-id="b7681-112">Ersätt `mynamespace`, `sharedaccesskeyname`, och `sharedaccesskey` med namnområdet, delad åtkomst (SAS)-signaturen nyckelnamn och värde.</span><span class="sxs-lookup"><span data-stu-id="b7681-112">Replace `mynamespace`, `sharedaccesskeyname`, and `sharedaccesskey` with your namespace, shared access signature (SAS) key name, and value.</span></span>

```python
bus_service = ServiceBusService(
    service_namespace='mynamespace',
    shared_access_key_name='sharedaccesskeyname',
    shared_access_key_value='sharedaccesskey')
```

<span data-ttu-id="b7681-113">hello värden för hello SAS-nyckelnamnet och värdet finns i hello [Azure-portalen] [ Azure portal] anslutningsinformationen, eller i hello Visual Studio **egenskaper** fönstret när du väljer Hej Service Bus-namnrymd i Server Explorer (som visas i hello föregående avsnitt).</span><span class="sxs-lookup"><span data-stu-id="b7681-113">hello values for hello SAS key name and value can be found in hello [Azure portal][Azure portal] connection information, or in hello Visual Studio **Properties** pane when selecting hello Service Bus namespace in Server Explorer (as shown in hello previous section).</span></span>

```python
bus_service.create_queue('taskqueue')
```

<span data-ttu-id="b7681-114">Hej `create_queue` metoden stöder också ytterligare alternativ som gör att du toooverride kön standardinställningarna som meddelandet toolive TTL (time) eller största köstorlek.</span><span class="sxs-lookup"><span data-stu-id="b7681-114">hello `create_queue` method also supports additional options, which enable you toooverride default queue settings such as message time toolive (TTL) or maximum queue size.</span></span> <span data-ttu-id="b7681-115">hello följande exempel anger hello största köstorlek storlek too5 GB och hello TTL-värdet too1 minut:</span><span class="sxs-lookup"><span data-stu-id="b7681-115">hello following example sets hello maximum queue size too5 GB, and hello TTL value too1 minute:</span></span>

```python
queue_options = Queue()
queue_options.max_size_in_megabytes = '5120'
queue_options.default_message_time_to_live = 'PT1M'

bus_service.create_queue('taskqueue', queue_options)
```

## <a name="send-messages-tooa-queue"></a><span data-ttu-id="b7681-116">Skicka meddelanden tooa kön</span><span class="sxs-lookup"><span data-stu-id="b7681-116">Send messages tooa queue</span></span>
<span data-ttu-id="b7681-117">toosend en meddelandekö tooa Service Bus programmet anropar hello `send_queue_message` metod på hello **ServiceBusService** objekt.</span><span class="sxs-lookup"><span data-stu-id="b7681-117">toosend a message tooa Service Bus queue, your application calls hello `send_queue_message` method on hello **ServiceBusService** object.</span></span>

<span data-ttu-id="b7681-118">hello exemplet nedan visar hur toosend en toohello meddelandekö test med namnet `taskqueue` med `send_queue_message`:</span><span class="sxs-lookup"><span data-stu-id="b7681-118">hello following example demonstrates how toosend a test message toohello queue named `taskqueue` using `send_queue_message`:</span></span>

```python
msg = Message(b'Test Message')
bus_service.send_queue_message('taskqueue', msg)
```

<span data-ttu-id="b7681-119">Service Bus-köer stöder en maximal meddelandestorlek på 256 KB i hello [standardnivån](service-bus-premium-messaging.md) och 1 MB på hello [premiumnivån](service-bus-premium-messaging.md).</span><span class="sxs-lookup"><span data-stu-id="b7681-119">Service Bus queues support a maximum message size of 256 KB in hello [Standard tier](service-bus-premium-messaging.md) and 1 MB in hello [Premium tier](service-bus-premium-messaging.md).</span></span> <span data-ttu-id="b7681-120">hello-huvudet som innehåller hello standard- och anpassade programegenskaperna, kan ha en maximal storlek på 64 KB.</span><span class="sxs-lookup"><span data-stu-id="b7681-120">hello header, which includes hello standard and custom application properties, can have a maximum size of 64 KB.</span></span> <span data-ttu-id="b7681-121">Det finns ingen gräns hello antalet meddelanden som finns i en kö men det finns ett tak på hello totala storleken på hälsningsmeddelande som innehas av en kö.</span><span class="sxs-lookup"><span data-stu-id="b7681-121">There is no limit on hello number of messages held in a queue but there is a cap on hello total size of hello messages held by a queue.</span></span> <span data-ttu-id="b7681-122">Den här köstorleken definieras när kön skapas, med en övre gräns på 5 GB.</span><span class="sxs-lookup"><span data-stu-id="b7681-122">This queue size is defined at creation time, with an upper limit of 5 GB.</span></span> <span data-ttu-id="b7681-123">Mer information om kvoter finns [Service Bus-kvoter][Service Bus quotas].</span><span class="sxs-lookup"><span data-stu-id="b7681-123">For more information about quotas, see [Service Bus quotas][Service Bus quotas].</span></span>

## <a name="receive-messages-from-a-queue"></a><span data-ttu-id="b7681-124">Ta emot meddelanden från en kö</span><span class="sxs-lookup"><span data-stu-id="b7681-124">Receive messages from a queue</span></span>
<span data-ttu-id="b7681-125">Meddelanden tas emot från en kö med hello `receive_queue_message` metod på hello **ServiceBusService** objekt:</span><span class="sxs-lookup"><span data-stu-id="b7681-125">Messages are received from a queue using hello `receive_queue_message` method on hello **ServiceBusService** object:</span></span>

```python
msg = bus_service.receive_queue_message('taskqueue', peek_lock=False)
print(msg.body)
```

<span data-ttu-id="b7681-126">Meddelanden tas bort från kön hello som de läses när hello parametern `peek_lock` har angetts för**FALSKT**.</span><span class="sxs-lookup"><span data-stu-id="b7681-126">Messages are deleted from hello queue as they are read when hello parameter `peek_lock` is set too**False**.</span></span> <span data-ttu-id="b7681-127">Du kan läsa (titt) och låsa hello-meddelande utan att ta bort den från hello kö genom att ange hello parametern `peek_lock` för**SANT**.</span><span class="sxs-lookup"><span data-stu-id="b7681-127">You can read (peek) and lock hello message without deleting it from hello queue by setting hello parameter `peek_lock` too**True**.</span></span>

<span data-ttu-id="b7681-128">Hej beteendet för läsning och ta bort hello-meddelande som en del av hello mottagningsåtgärd är hello enklaste modellen och fungerar bäst för scenarier där ett program kan tolerera icke-bearbetning av ett meddelande i hello händelse av fel.</span><span class="sxs-lookup"><span data-stu-id="b7681-128">hello behavior of reading and deleting hello message as part of hello receive operation is hello simplest model, and works best for scenarios in which an application can tolerate not processing a message in hello event of a failure.</span></span> <span data-ttu-id="b7681-129">toounderstand, Föreställ i vilka hello konsumenten problem hello mottagning av begäran och sedan kraschar innan bearbetningen.</span><span class="sxs-lookup"><span data-stu-id="b7681-129">toounderstand this, consider a scenario in which hello consumer issues hello receive request and then crashes before processing it.</span></span> <span data-ttu-id="b7681-130">Eftersom Service Bus kommer att ha markerat hello meddelandet som Förbrukat, sedan när hello programmet startas om och börjar förbruka meddelanden igen, att ha missat hello-meddelande som har förbrukats tidigare toohello krascher.</span><span class="sxs-lookup"><span data-stu-id="b7681-130">Because Service Bus will have marked hello message as being consumed, then when hello application restarts and begins consuming messages again, it will have missed hello message that was consumed prior toohello crash.</span></span>

<span data-ttu-id="b7681-131">Om hello `peek_lock` parameter har angetts för**SANT**, hello får blir en åtgärd i två steg, vilket gör det möjligt toosupport program som inte tolererar att ett meddelande saknas.</span><span class="sxs-lookup"><span data-stu-id="b7681-131">If hello `peek_lock` parameter is set too**True**, hello receive becomes a two stage operation, which makes it possible toosupport applications that cannot tolerate missing messages.</span></span> <span data-ttu-id="b7681-132">När Service Bus tar emot en begäran, hittar hello nästa meddelande toobe förbrukas, låser det tooprevent andra användare tar emot det och returnerar sedan toohello program.</span><span class="sxs-lookup"><span data-stu-id="b7681-132">When Service Bus receives a request, it finds hello next message toobe consumed, locks it tooprevent other consumers receiving it, and then returns it toohello application.</span></span> <span data-ttu-id="b7681-133">När hello programmet har avslutat bearbetningen hello-meddelande (eller lagrar det på ett tillförlitligt sätt för framtida bearbetning), Slutför hello andra steget i hello ta emot processen genom att anropa hello **ta bort** metod på hello **meddelandet** objekt.</span><span class="sxs-lookup"><span data-stu-id="b7681-133">After hello application finishes processing hello message (or stores it reliably for future processing), it completes hello second stage of hello receive process by calling hello **delete** method on hello **Message** object.</span></span> <span data-ttu-id="b7681-134">Hej **ta bort** metoden kommer Markera hello meddelandet som Förbrukat och ta bort den från hello kö.</span><span class="sxs-lookup"><span data-stu-id="b7681-134">hello **delete** method will mark hello message as being consumed and remove it from hello queue.</span></span>

```python
msg = bus_service.receive_queue_message('taskqueue', peek_lock=True)
print(msg.body)

msg.delete()
```

## <a name="how-toohandle-application-crashes-and-unreadable-messages"></a><span data-ttu-id="b7681-135">Hur toohandle programmet kraschar och oläsbara meddelanden</span><span class="sxs-lookup"><span data-stu-id="b7681-135">How toohandle application crashes and unreadable messages</span></span>
<span data-ttu-id="b7681-136">Service Bus innehåller funktioner toohelp du att återställa fel i programmet eller problem med bearbetning av ett meddelande.</span><span class="sxs-lookup"><span data-stu-id="b7681-136">Service Bus provides functionality toohelp you gracefully recover from errors in your application or difficulties processing a message.</span></span> <span data-ttu-id="b7681-137">Om ett mottagarprogram inte kan tooprocess hello meddelande av någon anledning, så kan det anropa hello **låsa upp** metod på hello **meddelandet** objekt.</span><span class="sxs-lookup"><span data-stu-id="b7681-137">If a receiver application is unable tooprocess hello message for some reason, then it can call hello **unlock** method on hello **Message** object.</span></span> <span data-ttu-id="b7681-138">Detta orsakar Service Bus toounlock hello-meddelande i kön för hello och göra den tillgänglig toobe tas emot igen, antingen hello av samma förbrukning av ett program eller ett annat användningsprogram.</span><span class="sxs-lookup"><span data-stu-id="b7681-138">This will cause Service Bus toounlock hello message within hello queue and make it available toobe received again, either by hello same consuming application or by another consuming application.</span></span>

<span data-ttu-id="b7681-139">Det finns också en tidsgräns som är associerad med ett meddelande i kön hello och om hello hello programmet misslyckas tooprocess hello meddelandet innan timeout för lås går ut (t.ex. Om hello programmet kraschar), kommer Service Bus låser upp hello-meddelande automatiskt och gör den tillgänglig toobe tas emot igen.</span><span class="sxs-lookup"><span data-stu-id="b7681-139">There is also a timeout associated with a message locked within hello queue, and if hello application fails tooprocess hello message before hello lock timeout expires (e.g., if hello application crashes), then Service Bus will unlock hello message automatically and make it available toobe received again.</span></span>

<span data-ttu-id="b7681-140">Hello händelse som hello programmet kraschar efter hello meddelandet har bearbetats men innan hello **ta bort** metoden anropas sedan hello-meddelande kommer att levereras på nytt toohello program när den startas om.</span><span class="sxs-lookup"><span data-stu-id="b7681-140">In hello event that hello application crashes after processing hello message but before hello **delete** method is called, then hello message will be redelivered toohello application when it restarts.</span></span> <span data-ttu-id="b7681-141">Det här kallas ofta **At Least Once Processing**, det vill säga, varje meddelande bearbetas minst en gång men i vissa situationer hello samma meddelande levereras.</span><span class="sxs-lookup"><span data-stu-id="b7681-141">This is often called **At Least Once Processing**, that is, each message will be processed at least once but in certain situations hello same message may be redelivered.</span></span> <span data-ttu-id="b7681-142">Om hello scenariot inte tolererar duplicerad bearbetning, sedan bör programutvecklarna lägga till ytterligare logik tootheir duplicerad meddelandeleverans för programmet toohandle.</span><span class="sxs-lookup"><span data-stu-id="b7681-142">If hello scenario cannot tolerate duplicate processing, then application developers should add additional logic tootheir application toohandle duplicate message delivery.</span></span> <span data-ttu-id="b7681-143">Detta uppnås ofta med hjälp av hello **MessageId** -egenskapen för hello-meddelande, förblir konstant under alla leveransförsök.</span><span class="sxs-lookup"><span data-stu-id="b7681-143">This is often achieved using hello **MessageId** property of hello message, which will remain constant across delivery attempts.</span></span>

## <a name="next-steps"></a><span data-ttu-id="b7681-144">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="b7681-144">Next steps</span></span>
<span data-ttu-id="b7681-145">Nu när du har lärt dig grunderna hello i Service Bus-köer finns i de här artiklarna toolearn mer.</span><span class="sxs-lookup"><span data-stu-id="b7681-145">Now that you have learned hello basics of Service Bus queues, see these articles toolearn more.</span></span>

* <span data-ttu-id="b7681-146">[Köer, ämnen och prenumerationer][Queues, topics, and subscriptions]</span><span class="sxs-lookup"><span data-stu-id="b7681-146">[Queues, topics, and subscriptions][Queues, topics, and subscriptions]</span></span>

[Azure portal]: https://portal.azure.com
[Python Azure Service Bus package]: https://pypi.python.org/pypi/azure-servicebus  
[Queues, topics, and subscriptions]: service-bus-queues-topics-subscriptions.md
[Service Bus quotas]: service-bus-quotas.md

