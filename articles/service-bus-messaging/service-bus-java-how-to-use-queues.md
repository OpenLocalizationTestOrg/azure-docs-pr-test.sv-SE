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
# <a name="how-toouse-service-bus-queues-with-java"></a>Hur toouse Service Bus köer med Java
[!INCLUDE [service-bus-selector-queues](../../includes/service-bus-selector-queues.md)]

Den här artikeln beskriver hur toouse Service Bus-köer. hello exemplen är skrivna i Java och använder hello [Azure SDK för Java][Azure SDK for Java]. hello beskrivs scenarier där **skapa köer**, **skicka och ta emot meddelanden**, och **tar bort köer**.

[!INCLUDE [howto-service-bus-queues](../../includes/howto-service-bus-queues.md)]

[!INCLUDE [service-bus-create-namespace-portal](../../includes/service-bus-create-namespace-portal.md)]

## <a name="configure-your-application-toouse-service-bus"></a>Konfigurera ditt program toouse Service Bus
Kontrollera att du har installerat hello [Azure SDK för Java] [ Azure SDK for Java] innan du skapar det här exemplet. Om du använder Eclipse, kan du installera hello [Azure Toolkit för Eclipse] [ Azure Toolkit for Eclipse] som innehåller hello Azure SDK för Java. Du kan sedan lägga till hello **Microsoft Azure-biblioteken för Java** tooyour projektet:

![](./media/service-bus-java-how-to-use-queues/eclipselibs.png)

Lägg till följande hello `import` instruktioner toohello överkant hello Java-fil:

```java
// Include hello following imports toouse Service Bus APIs
import com.microsoft.windowsazure.services.servicebus.*;
import com.microsoft.windowsazure.services.servicebus.models.*;
import com.microsoft.windowsazure.core.*;
import javax.xml.datatype.*;
```

## <a name="create-a-queue"></a>Skapa en kö
Hanteringsåtgärder för Service Bus-köer kan utföras via den **ServiceBusContract** klass. En **ServiceBusContract** objektet konstrueras med en lämplig konfiguration som innehåller SAS-token med behörigheter toomanage och hello **ServiceBusContract** klassen är hello enda punkt kommunikation med Azure.

Hej **ServiceBusService** klassen innehåller metoder toocreate, räkna upp och ta bort köer. Hej exemplet nedan visar hur en **ServiceBusService** objekt kan vara används toocreate en kö med namnet `TestQueue`, med ett namnområde med namnet `HowToSample`:

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

Det finns sätt på `QueueInfo` som gör att egenskaper för hello kön toobe justerade (till exempel: tooset hello standard time to live (TTL) värdet toobe tillämpas toomessages skickas toohello kön). hello följande exempel visas hur toocreate en kö med namnet `TestQueue` med en maximal storlek på 5 GB:

````java
long maxSizeInMegabytes = 5120;
QueueInfo queueInfo = new QueueInfo("TestQueue");
queueInfo.setMaxSizeInMegabytes(maxSizeInMegabytes);
CreateQueueResult result = service.createQueue(queueInfo);
````

Observera att du kan använda hello `listQueues` metod på **ServiceBusContract** objekt toocheck om en kö med ett angivet namn finns redan i ett namnområde för tjänsten.

## <a name="send-messages-tooa-queue"></a>Skicka meddelanden tooa kön
toosend en meddelandekö tooa Service Bus programmet hämtar en **ServiceBusContract** objekt. Hej följande kod visar hur toosend ett meddelande för hello `TestQueue` kö som tidigare har skapats i hello `HowToSample` namnområde:

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

Meddelanden som skickas till och tas emot från Service Bus-köer är instanser av hello [BrokeredMessage] [ BrokeredMessage] klass. [BrokeredMessage] [ BrokeredMessage] objekt har en uppsättning standardegenskaper (t.ex [etikett](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage.label#Microsoft_ServiceBus_Messaging_BrokeredMessage_Label) och [TimeToLive](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage.timetolive#Microsoft_ServiceBus_Messaging_BrokeredMessage_TimeToLive)), en ordlista som används toohold anpassade programspecifika egenskaper och en brödtext med godtyckliga programdata. Ett program kan konfigurera hello brödtext hello-meddelande genom att skicka någon typ av serialiserbara objekt till hello konstruktorn för hello [BrokeredMessage][BrokeredMessage], och hello lämplig serialisering kommer sedan att användas tooserialize hello-objekt. Du kan också tillhandahålla en **java. IO. InputStream** objekt.

hello exemplet nedan visar hur toosend fem test meddelanden toothe `TestQueue` **MessageSender** vi fick i hello föregående kodfragment:

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

Service Bus-köer stöder en maximal meddelandestorlek på 256 KB i hello [standardnivån](service-bus-premium-messaging.md) och 1 MB på hello [premiumnivån](service-bus-premium-messaging.md). hello-huvudet som innehåller hello standard- och anpassade programegenskaperna, kan ha en maximal storlek på 64 KB. Det finns ingen gräns hello antalet meddelanden som finns i en kö men det finns ett tak på hello totala storleken på hälsningsmeddelande som innehas av en kö. Den här köstorleken definieras när kön skapas, med en övre gräns på 5 GB.

## <a name="receive-messages-from-a-queue"></a>Ta emot meddelanden från en kö
hello primära sätt tooreceive meddelanden från en kö är toouse en **ServiceBusContract** objekt. Mottagna meddelanden kan arbeta i två olika lägen: **ReceiveAndDelete** och **PeekLock**.

När du använder hello **ReceiveAndDelete** läge får är en engångsåtgärd – det vill säga när Service Bus tar emot en läsbegäran för ett meddelande i en kö, den markerar hello meddelandet som Förbrukat och returnerar det toohello program. **ReceiveAndDelete** läge (vilket är standardläget för hello) är hello enklaste modellen och fungerar bäst för scenarier där ett program kan tolerera icke-bearbetning av ett meddelande i hello händelse av fel. toounderstand, Föreställ i vilka hello konsumenten problem hello mottagning av begäran och sedan kraschar innan bearbetningen.
Eftersom Service Bus kommer att ha markerat hello meddelandet som Förbrukat, sedan när hello programmet startas om och börjar förbruka meddelanden igen, att ha missat hello-meddelande som har förbrukats tidigare toohello krascher.

I **PeekLock** läge får blir en åtgärd i två steg, vilket gör det möjligt toosupport program som inte tolererar att ett meddelande saknas. När Service Bus tar emot en begäran, hittar hello nästa meddelande toobe förbrukas, låser det tooprevent andra användare tar emot det och returnerar sedan toohello program. När hello programmet har avslutat bearbetningen hello-meddelande (eller lagrar det på ett tillförlitligt sätt för framtida bearbetning), Slutför hello andra steget i hello får processen genom att anropa **ta bort** på emot hello-meddelande. När Service Bus ser hello **ta bort** -anrop som den Markera hello meddelandet som Förbrukat och ta bort den från hello kön.

hello exemplet nedan visar hur meddelanden kan tas emot och bearbetat använder **PeekLock** läge (inte hello standardläget). hello exemplet nedan har en oändlig loop och bearbetar meddelanden när de tas emot i vår `TestQueue`:

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

## <a name="how-toohandle-application-crashes-and-unreadable-messages"></a>Hur toohandle programmet kraschar och oläsbara meddelanden
Service Bus innehåller funktioner toohelp du att återställa fel i programmet eller problem med bearbetning av ett meddelande. Om ett mottagarprogram inte kan tooprocess hello meddelande av någon anledning, så kan det anropa hello **unlockMessage** metod för hello tog emot meddelande (i stället för hello **deleteMessage** metod). Detta medför att Service Bus toounlock hello meddelandet i kön för hello och gör den tillgänglig toobe tas emot igen, hello antingen av samma förbrukning av ett program eller ett annat användningsprogram.

Det finns också en tidsgräns som är associerad med ett meddelande som ligger låst i kön, och om programmet hello misslyckas tooprocess hello meddelande innan timeout för lås upphör att gälla (till exempel om hello programmet kraschar) och Service Bus låser upp hello-meddelande automatiskt och gör det tillgängligt toobe tas emot igen.

Hello händelse som hello programmet kraschar efter hello meddelandet har bearbetats men innan hello **deleteMessage** begäran utfärdas och sedan hello-meddelande är toohello levereras på nytt program när den startas om. Det här kallas ofta *At Least Once Processing*, det vill säga varje meddelande bearbetas minst en gång men i vissa situationer hello samma meddelande kan levereras. Om hello scenariot inte tolererar duplicerad bearbetning, sedan bör programutvecklarna lägga till ytterligare logik tootheir duplicerad meddelandeleverans för programmet toohandle. Detta uppnås ofta med hjälp av hello **getMessageId** metod i hello-meddelande, förblir konstant under alla leveransförsök.

## <a name="next-steps"></a>Nästa steg
Nu när du har lärt dig grunderna hello i Service Bus-köer, se [köer, ämnen och prenumerationer] [ Queues, topics, and subscriptions] för mer information.

Mer information finns i hello [Java-Utvecklingscenter](https://azure.microsoft.com/develop/java/).

[Azure SDK for Java]: http://azure.microsoft.com/develop/java/
[Azure Toolkit for Eclipse]: https://msdn.microsoft.com/library/azure/hh694271.aspx
[Queues, topics, and subscriptions]: service-bus-queues-topics-subscriptions.md
[BrokeredMessage]: /dotnet/api/microsoft.servicebus.messaging.brokeredmessage
