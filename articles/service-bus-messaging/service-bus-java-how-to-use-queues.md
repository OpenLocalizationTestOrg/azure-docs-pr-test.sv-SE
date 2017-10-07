---
title: "aaaHow toouse Azure Service Bus-köer med Java | Microsoft Docs"
description: "Lär dig hur toouse Service Bus köer i Azure. Kodexempel som skrivits i Java."
services: service-bus-messaging
documentationcenter: java
author: sethmanheim
manager: timlt
ms.assetid: f701439c-553e-402c-94a7-64400f997d59
ms.service: service-bus-messaging
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: Java
ms.topic: article
ms.date: 08/10/2017
ms.author: sethm
ms.openlocfilehash: f68e941438134090c5eee53459e7667bda13ff3c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="how-toouse-service-bus-queues-with-java"></a><span data-ttu-id="d5740-104">Hur toouse Service Bus köer med Java</span><span class="sxs-lookup"><span data-stu-id="d5740-104">How toouse Service Bus queues with Java</span></span>
[!INCLUDE [service-bus-selector-queues](../../includes/service-bus-selector-queues.md)]

<span data-ttu-id="d5740-105">Den här artikeln beskriver hur toouse Service Bus-köer.</span><span class="sxs-lookup"><span data-stu-id="d5740-105">This article describes how toouse Service Bus queues.</span></span> <span data-ttu-id="d5740-106">hello exemplen är skrivna i Java och använder hello [Azure SDK för Java][Azure SDK for Java].</span><span class="sxs-lookup"><span data-stu-id="d5740-106">hello samples are written in Java and use hello [Azure SDK for Java][Azure SDK for Java].</span></span> <span data-ttu-id="d5740-107">hello beskrivs scenarier där **skapa köer**, **skicka och ta emot meddelanden**, och **tar bort köer**.</span><span class="sxs-lookup"><span data-stu-id="d5740-107">hello scenarios covered include **creating queues**, **sending and receiving messages**, and **deleting queues**.</span></span>

[!INCLUDE [howto-service-bus-queues](../../includes/howto-service-bus-queues.md)]

[!INCLUDE [service-bus-create-namespace-portal](../../includes/service-bus-create-namespace-portal.md)]

## <a name="configure-your-application-toouse-service-bus"></a><span data-ttu-id="d5740-108">Konfigurera ditt program toouse Service Bus</span><span class="sxs-lookup"><span data-stu-id="d5740-108">Configure your application toouse Service Bus</span></span>
<span data-ttu-id="d5740-109">Kontrollera att du har installerat hello [Azure SDK för Java] [ Azure SDK for Java] innan du skapar det här exemplet.</span><span class="sxs-lookup"><span data-stu-id="d5740-109">Make sure you have installed hello [Azure SDK for Java][Azure SDK for Java] before building this sample.</span></span> <span data-ttu-id="d5740-110">Om du använder Eclipse, kan du installera hello [Azure Toolkit för Eclipse] [ Azure Toolkit for Eclipse] som innehåller hello Azure SDK för Java.</span><span class="sxs-lookup"><span data-stu-id="d5740-110">If you are using Eclipse, you can install hello [Azure Toolkit for Eclipse][Azure Toolkit for Eclipse] that includes hello Azure SDK for Java.</span></span> <span data-ttu-id="d5740-111">Du kan sedan lägga till hello **Microsoft Azure-biblioteken för Java** tooyour projektet:</span><span class="sxs-lookup"><span data-stu-id="d5740-111">You can then add hello **Microsoft Azure Libraries for Java** tooyour project:</span></span>

![](./media/service-bus-java-how-to-use-queues/eclipselibs.png)

<span data-ttu-id="d5740-112">Lägg till följande hello `import` instruktioner toohello överkant hello Java-fil:</span><span class="sxs-lookup"><span data-stu-id="d5740-112">Add hello following `import` statements toohello top of hello Java file:</span></span>

```java
// Include hello following imports toouse Service Bus APIs
import com.microsoft.windowsazure.services.servicebus.*;
import com.microsoft.windowsazure.services.servicebus.models.*;
import com.microsoft.windowsazure.core.*;
import javax.xml.datatype.*;
```

## <a name="create-a-queue"></a><span data-ttu-id="d5740-113">Skapa en kö</span><span class="sxs-lookup"><span data-stu-id="d5740-113">Create a queue</span></span>
<span data-ttu-id="d5740-114">Hanteringsåtgärder för Service Bus-köer kan utföras via den **ServiceBusContract** klass.</span><span class="sxs-lookup"><span data-stu-id="d5740-114">Management operations for Service Bus queues can be performed via the **ServiceBusContract** class.</span></span> <span data-ttu-id="d5740-115">En **ServiceBusContract** objektet konstrueras med en lämplig konfiguration som innehåller SAS-token med behörigheter toomanage och hello **ServiceBusContract** klassen är hello enda punkt kommunikation med Azure.</span><span class="sxs-lookup"><span data-stu-id="d5740-115">A **ServiceBusContract** object is constructed with an appropriate configuration that encapsulates the SAS token with permissions toomanage it, and hello **ServiceBusContract** class is hello sole point of communication with Azure.</span></span>

<span data-ttu-id="d5740-116">Hej **ServiceBusService** klassen innehåller metoder toocreate, räkna upp och ta bort köer.</span><span class="sxs-lookup"><span data-stu-id="d5740-116">hello **ServiceBusService** class provides methods toocreate, enumerate, and delete queues.</span></span> <span data-ttu-id="d5740-117">Hej exemplet nedan visar hur en **ServiceBusService** objekt kan vara används toocreate en kö med namnet `TestQueue`, med ett namnområde med namnet `HowToSample`:</span><span class="sxs-lookup"><span data-stu-id="d5740-117">hello example below shows how a **ServiceBusService** object can be used toocreate a queue named `TestQueue`, with a namespace named `HowToSample`:</span></span>

```java
Configuration config =
    ServiceBusConfiguration.configureWithSASAuthentication(
            "HowToSample",
            "RootManageSharedAccessKey",
            "SAS_key_value",
            ".servicebus.windows.net"
            );

ServiceBusContract service = ServiceBusService.create(config);
QueueInfo queueInfo = new QueueInfo("TestQueue");
try
{
    CreateQueueResult result = service.createQueue(queueInfo);
}
catch (ServiceException e)
{
    System.out.print("ServiceException encountered: ");
    System.out.println(e.getMessage());
    System.exit(-1);
}
```

<span data-ttu-id="d5740-118">Det finns sätt på `QueueInfo` som gör att egenskaper för hello kön toobe justerade (till exempel: tooset hello standard time to live (TTL) värdet toobe tillämpas toomessages skickas toohello kön).</span><span class="sxs-lookup"><span data-stu-id="d5740-118">There are methods on `QueueInfo` that allow properties of hello queue toobe tuned (for example: tooset hello default time-to-live (TTL) value toobe applied toomessages sent toohello queue).</span></span> <span data-ttu-id="d5740-119">hello följande exempel visas hur toocreate en kö med namnet `TestQueue` med en maximal storlek på 5 GB:</span><span class="sxs-lookup"><span data-stu-id="d5740-119">hello following example shows how toocreate a queue named `TestQueue` with a maximum size of 5GB:</span></span>

````java
long maxSizeInMegabytes = 5120;
QueueInfo queueInfo = new QueueInfo("TestQueue");
queueInfo.setMaxSizeInMegabytes(maxSizeInMegabytes);
CreateQueueResult result = service.createQueue(queueInfo);
````

<span data-ttu-id="d5740-120">Observera att du kan använda hello `listQueues` metod på **ServiceBusContract** objekt toocheck om en kö med ett angivet namn finns redan i ett namnområde för tjänsten.</span><span class="sxs-lookup"><span data-stu-id="d5740-120">Note that you can use hello `listQueues` method on **ServiceBusContract** objects toocheck if a queue with a specified name already exists within a service namespace.</span></span>

## <a name="send-messages-tooa-queue"></a><span data-ttu-id="d5740-121">Skicka meddelanden tooa kön</span><span class="sxs-lookup"><span data-stu-id="d5740-121">Send messages tooa queue</span></span>
<span data-ttu-id="d5740-122">toosend en meddelandekö tooa Service Bus programmet hämtar en **ServiceBusContract** objekt.</span><span class="sxs-lookup"><span data-stu-id="d5740-122">toosend a message tooa Service Bus queue, your application obtains a **ServiceBusContract** object.</span></span> <span data-ttu-id="d5740-123">Hej följande kod visar hur toosend ett meddelande för hello `TestQueue` kö som tidigare har skapats i hello `HowToSample` namnområde:</span><span class="sxs-lookup"><span data-stu-id="d5740-123">hello following code shows how toosend a message for hello `TestQueue` queue previously created in hello `HowToSample` namespace:</span></span>

```java
try
{
    BrokeredMessage message = new BrokeredMessage("MyMessage");
    service.sendQueueMessage("TestQueue", message);
}
catch (ServiceException e)
{
    System.out.print("ServiceException encountered: ");
    System.out.println(e.getMessage());
    System.exit(-1);
}
```

<span data-ttu-id="d5740-124">Meddelanden som skickas till och tas emot från Service Bus-köer är instanser av hello [BrokeredMessage] [ BrokeredMessage] klass.</span><span class="sxs-lookup"><span data-stu-id="d5740-124">Messages sent to, and received from Service Bus queues are instances of hello [BrokeredMessage][BrokeredMessage] class.</span></span> <span data-ttu-id="d5740-125">[BrokeredMessage] [ BrokeredMessage] objekt har en uppsättning standardegenskaper (t.ex [etikett](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage.label#Microsoft_ServiceBus_Messaging_BrokeredMessage_Label) och [TimeToLive](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage.timetolive#Microsoft_ServiceBus_Messaging_BrokeredMessage_TimeToLive)), en ordlista som används toohold anpassade programspecifika egenskaper och en brödtext med godtyckliga programdata.</span><span class="sxs-lookup"><span data-stu-id="d5740-125">[BrokeredMessage][BrokeredMessage] objects have a set of standard properties (such as [Label](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage.label#Microsoft_ServiceBus_Messaging_BrokeredMessage_Label) and [TimeToLive](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage.timetolive#Microsoft_ServiceBus_Messaging_BrokeredMessage_TimeToLive)), a dictionary that is used toohold custom application-specific properties, and a body of arbitrary application data.</span></span> <span data-ttu-id="d5740-126">Ett program kan konfigurera hello brödtext hello-meddelande genom att skicka någon typ av serialiserbara objekt till hello konstruktorn för hello [BrokeredMessage][BrokeredMessage], och hello lämplig serialisering kommer sedan att användas tooserialize hello-objekt.</span><span class="sxs-lookup"><span data-stu-id="d5740-126">An application can set hello body of hello message by passing any serializable object into hello constructor of hello [BrokeredMessage][BrokeredMessage], and hello appropriate serializer will then be used tooserialize hello object.</span></span> <span data-ttu-id="d5740-127">Du kan också tillhandahålla en **java. IO. InputStream** objekt.</span><span class="sxs-lookup"><span data-stu-id="d5740-127">Alternatively, you can provide a **java.IO.InputStream** object.</span></span>

<span data-ttu-id="d5740-128">hello exemplet nedan visar hur toosend fem test meddelanden toothe `TestQueue` **MessageSender** vi fick i hello föregående kodfragment:</span><span class="sxs-lookup"><span data-stu-id="d5740-128">hello following example demonstrates how toosend five test messages toothe `TestQueue` **MessageSender** we obtained in hello previous code snippet:</span></span>

```java
for (int i=0; i<5; i++)
{
     // Create message, passing a string message for hello body.
     BrokeredMessage message = new BrokeredMessage("Test message " + i);
     // Set an additional app-specific property.
     message.setProperty("MyProperty", i);
     // Send message toohello queue
     service.sendQueueMessage("TestQueue", message);
}
```

<span data-ttu-id="d5740-129">Service Bus-köer stöder en maximal meddelandestorlek på 256 KB i hello [standardnivån](service-bus-premium-messaging.md) och 1 MB på hello [premiumnivån](service-bus-premium-messaging.md).</span><span class="sxs-lookup"><span data-stu-id="d5740-129">Service Bus queues support a maximum message size of 256 KB in hello [Standard tier](service-bus-premium-messaging.md) and 1 MB in hello [Premium tier](service-bus-premium-messaging.md).</span></span> <span data-ttu-id="d5740-130">hello-huvudet som innehåller hello standard- och anpassade programegenskaperna, kan ha en maximal storlek på 64 KB.</span><span class="sxs-lookup"><span data-stu-id="d5740-130">hello header, which includes hello standard and custom application properties, can have a maximum size of 64 KB.</span></span> <span data-ttu-id="d5740-131">Det finns ingen gräns hello antalet meddelanden som finns i en kö men det finns ett tak på hello totala storleken på hälsningsmeddelande som innehas av en kö.</span><span class="sxs-lookup"><span data-stu-id="d5740-131">There is no limit on hello number of messages held in a queue but there is a cap on hello total size of hello messages held by a queue.</span></span> <span data-ttu-id="d5740-132">Den här köstorleken definieras när kön skapas, med en övre gräns på 5 GB.</span><span class="sxs-lookup"><span data-stu-id="d5740-132">This queue size is defined at creation time, with an upper limit of 5 GB.</span></span>

## <a name="receive-messages-from-a-queue"></a><span data-ttu-id="d5740-133">Ta emot meddelanden från en kö</span><span class="sxs-lookup"><span data-stu-id="d5740-133">Receive messages from a queue</span></span>
<span data-ttu-id="d5740-134">hello primära sätt tooreceive meddelanden från en kö är toouse en **ServiceBusContract** objekt.</span><span class="sxs-lookup"><span data-stu-id="d5740-134">hello primary way tooreceive messages from a queue is toouse a **ServiceBusContract** object.</span></span> <span data-ttu-id="d5740-135">Mottagna meddelanden kan arbeta i två olika lägen: **ReceiveAndDelete** och **PeekLock**.</span><span class="sxs-lookup"><span data-stu-id="d5740-135">Received messages can work in two different modes: **ReceiveAndDelete** and **PeekLock**.</span></span>

<span data-ttu-id="d5740-136">När du använder hello **ReceiveAndDelete** läge får är en engångsåtgärd – det vill säga när Service Bus tar emot en läsbegäran för ett meddelande i en kö, den markerar hello meddelandet som Förbrukat och returnerar det toohello program.</span><span class="sxs-lookup"><span data-stu-id="d5740-136">When using hello **ReceiveAndDelete** mode, receive is a single-shot operation - that is, when Service Bus receives a read request for a message in a queue, it marks hello message as being consumed and returns it toohello application.</span></span> <span data-ttu-id="d5740-137">**ReceiveAndDelete** läge (vilket är standardläget för hello) är hello enklaste modellen och fungerar bäst för scenarier där ett program kan tolerera icke-bearbetning av ett meddelande i hello händelse av fel.</span><span class="sxs-lookup"><span data-stu-id="d5740-137">**ReceiveAndDelete** mode (which is hello default mode) is hello simplest model and works best for scenarios in which an application can tolerate not processing a message in hello event of a failure.</span></span> <span data-ttu-id="d5740-138">toounderstand, Föreställ i vilka hello konsumenten problem hello mottagning av begäran och sedan kraschar innan bearbetningen.</span><span class="sxs-lookup"><span data-stu-id="d5740-138">toounderstand this, consider a scenario in which hello consumer issues hello receive request and then crashes before processing it.</span></span>
<span data-ttu-id="d5740-139">Eftersom Service Bus kommer att ha markerat hello meddelandet som Förbrukat, sedan när hello programmet startas om och börjar förbruka meddelanden igen, att ha missat hello-meddelande som har förbrukats tidigare toohello krascher.</span><span class="sxs-lookup"><span data-stu-id="d5740-139">Because Service Bus will have marked hello message as being consumed, then when hello application restarts and begins consuming messages again, it will have missed hello message that was consumed prior toohello crash.</span></span>

<span data-ttu-id="d5740-140">I **PeekLock** läge får blir en åtgärd i två steg, vilket gör det möjligt toosupport program som inte tolererar att ett meddelande saknas.</span><span class="sxs-lookup"><span data-stu-id="d5740-140">In **PeekLock** mode, receive becomes a two stage operation, which makes it possible toosupport applications that cannot tolerate missing messages.</span></span> <span data-ttu-id="d5740-141">När Service Bus tar emot en begäran, hittar hello nästa meddelande toobe förbrukas, låser det tooprevent andra användare tar emot det och returnerar sedan toohello program.</span><span class="sxs-lookup"><span data-stu-id="d5740-141">When Service Bus receives a request, it finds hello next message toobe consumed, locks it tooprevent other consumers receiving it, and then returns it toohello application.</span></span> <span data-ttu-id="d5740-142">När hello programmet har avslutat bearbetningen hello-meddelande (eller lagrar det på ett tillförlitligt sätt för framtida bearbetning), Slutför hello andra steget i hello får processen genom att anropa **ta bort** på emot hello-meddelande.</span><span class="sxs-lookup"><span data-stu-id="d5740-142">After hello application finishes processing hello message (or stores it reliably for future processing), it completes hello second stage of hello receive process by calling **Delete** on hello received message.</span></span> <span data-ttu-id="d5740-143">När Service Bus ser hello **ta bort** -anrop som den Markera hello meddelandet som Förbrukat och ta bort den från hello kön.</span><span class="sxs-lookup"><span data-stu-id="d5740-143">When Service Bus sees hello **Delete** call, it will mark hello message as being consumed and remove it from hello queue.</span></span>

<span data-ttu-id="d5740-144">hello exemplet nedan visar hur meddelanden kan tas emot och bearbetat använder **PeekLock** läge (inte hello standardläget).</span><span class="sxs-lookup"><span data-stu-id="d5740-144">hello following example demonstrates how messages can be received and processed using **PeekLock** mode (not hello default mode).</span></span> <span data-ttu-id="d5740-145">hello exemplet nedan har en oändlig loop och bearbetar meddelanden när de tas emot i vår `TestQueue`:</span><span class="sxs-lookup"><span data-stu-id="d5740-145">hello example below does an infinite loop and processes messages as they arrive into our `TestQueue`:</span></span>

```java
try
{
    ReceiveMessageOptions opts = ReceiveMessageOptions.DEFAULT;
    opts.setReceiveMode(ReceiveMode.PEEK_LOCK);

    while(true)  {
         ReceiveQueueMessageResult resultQM =
                 service.receiveQueueMessage("TestQueue", opts);
        BrokeredMessage message = resultQM.getValue();
        if (message != null && message.getMessageId() != null)
        {
            System.out.println("MessageID: " + message.getMessageId());
            // Display hello queue message.
            System.out.print("From queue: ");
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
                message.getProperty("MyProperty"));
            // Remove message from queue.
            System.out.println("Deleting this message.");
            //service.deleteMessage(message);
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

## <a name="how-toohandle-application-crashes-and-unreadable-messages"></a><span data-ttu-id="d5740-146">Hur toohandle programmet kraschar och oläsbara meddelanden</span><span class="sxs-lookup"><span data-stu-id="d5740-146">How toohandle application crashes and unreadable messages</span></span>
<span data-ttu-id="d5740-147">Service Bus innehåller funktioner toohelp du att återställa fel i programmet eller problem med bearbetning av ett meddelande.</span><span class="sxs-lookup"><span data-stu-id="d5740-147">Service Bus provides functionality toohelp you gracefully recover from errors in your application or difficulties processing a message.</span></span> <span data-ttu-id="d5740-148">Om ett mottagarprogram inte kan tooprocess hello meddelande av någon anledning, så kan det anropa hello **unlockMessage** metod för hello tog emot meddelande (i stället för hello **deleteMessage** metod).</span><span class="sxs-lookup"><span data-stu-id="d5740-148">If a receiver application is unable tooprocess hello message for some reason, then it can call hello **unlockMessage** method on hello received message (instead of hello **deleteMessage** method).</span></span> <span data-ttu-id="d5740-149">Detta medför att Service Bus toounlock hello meddelandet i kön för hello och gör den tillgänglig toobe tas emot igen, hello antingen av samma förbrukning av ett program eller ett annat användningsprogram.</span><span class="sxs-lookup"><span data-stu-id="d5740-149">This causes Service Bus toounlock hello message within hello queue and make it available toobe received again, either by hello same consuming application or by another consuming application.</span></span>

<span data-ttu-id="d5740-150">Det finns också en tidsgräns som är associerad med ett meddelande som ligger låst i kön, och om programmet hello misslyckas tooprocess hello meddelande innan timeout för lås upphör att gälla (till exempel om hello programmet kraschar) och Service Bus låser upp hello-meddelande automatiskt och gör det tillgängligt toobe tas emot igen.</span><span class="sxs-lookup"><span data-stu-id="d5740-150">There is also a timeout associated with a message locked within the queue, and if hello application fails tooprocess hello message before the lock timeout expires (for example, if hello application crashes), then Service Bus unlocks hello message automatically and makes it available toobe received again.</span></span>

<span data-ttu-id="d5740-151">Hello händelse som hello programmet kraschar efter hello meddelandet har bearbetats men innan hello **deleteMessage** begäran utfärdas och sedan hello-meddelande är toohello levereras på nytt program när den startas om.</span><span class="sxs-lookup"><span data-stu-id="d5740-151">In hello event that hello application crashes after processing hello message but before hello **deleteMessage** request is issued, then hello message is redelivered toohello application when it restarts.</span></span> <span data-ttu-id="d5740-152">Det här kallas ofta *At Least Once Processing*, det vill säga varje meddelande bearbetas minst en gång men i vissa situationer hello samma meddelande kan levereras.</span><span class="sxs-lookup"><span data-stu-id="d5740-152">This is often called *At Least Once Processing*; that is, each message is processed at least once but in certain situations hello same message may be redelivered.</span></span> <span data-ttu-id="d5740-153">Om hello scenariot inte tolererar duplicerad bearbetning, sedan bör programutvecklarna lägga till ytterligare logik tootheir duplicerad meddelandeleverans för programmet toohandle.</span><span class="sxs-lookup"><span data-stu-id="d5740-153">If hello scenario cannot tolerate duplicate processing, then application developers should add additional logic tootheir application toohandle duplicate message delivery.</span></span> <span data-ttu-id="d5740-154">Detta uppnås ofta med hjälp av hello **getMessageId** metod i hello-meddelande, förblir konstant under alla leveransförsök.</span><span class="sxs-lookup"><span data-stu-id="d5740-154">This is often achieved using hello **getMessageId** method of hello message, which remains constant across delivery attempts.</span></span>

## <a name="next-steps"></a><span data-ttu-id="d5740-155">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="d5740-155">Next Steps</span></span>
<span data-ttu-id="d5740-156">Nu när du har lärt dig grunderna hello i Service Bus-köer, se [köer, ämnen och prenumerationer] [ Queues, topics, and subscriptions] för mer information.</span><span class="sxs-lookup"><span data-stu-id="d5740-156">Now that you've learned hello basics of Service Bus queues, see [Queues, topics, and subscriptions][Queues, topics, and subscriptions] for more information.</span></span>

<span data-ttu-id="d5740-157">Mer information finns i hello [Java-Utvecklingscenter](https://azure.microsoft.com/develop/java/).</span><span class="sxs-lookup"><span data-stu-id="d5740-157">For more information, see hello [Java Developer Center](https://azure.microsoft.com/develop/java/).</span></span>

[Azure SDK for Java]: http://azure.microsoft.com/develop/java/
[Azure Toolkit for Eclipse]: https://msdn.microsoft.com/library/azure/hh694271.aspx
[Queues, topics, and subscriptions]: service-bus-queues-topics-subscriptions.md
[BrokeredMessage]: /dotnet/api/microsoft.servicebus.messaging.brokeredmessage
