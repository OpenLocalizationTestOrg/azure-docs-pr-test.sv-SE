---
title: "Hur du använder Azure Service Bus-köer med Java | Microsoft Docs"
description: "Lär dig hur du använder Service Bus-köer i Azure. Kodexempel som skrivits i Java."
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
ms.openlocfilehash: 170f431525ffdc93a01fc085e48e69c3a774968e
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/11/2017
---
# <a name="how-to-use-service-bus-queues-with-java"></a>Hur du använder Service Bus-köer med Java
[!INCLUDE [service-bus-selector-queues](../../includes/service-bus-selector-queues.md)]

Den här artikeln beskriver hur du använder Service Bus-köer. Exemplen är skrivna i Java och Använd den [Azure SDK för Java][Azure SDK for Java]. Scenarier som tas upp inkluderar **skapa köer**, **skicka och ta emot meddelanden**, och **tar bort köer**.

[!INCLUDE [howto-service-bus-queues](../../includes/howto-service-bus-queues.md)]

[!INCLUDE [service-bus-create-namespace-portal](../../includes/service-bus-create-namespace-portal.md)]

## <a name="configure-your-application-to-use-service-bus"></a>Konfigurera ditt program att använda Service Bus
Kontrollera att du har installerat den [Azure SDK för Java] [ Azure SDK for Java] innan du skapar det här exemplet. Om du använder Eclipse, kan du installera den [Azure Toolkit för Eclipse] [ Azure Toolkit for Eclipse] som innehåller Azure SDK för Java. Du kan sedan lägga till den **Microsoft Azure-biblioteken för Java** i projektet:

![](./media/service-bus-java-how-to-use-queues/eclipselibs.png)

Lägg till följande `import` instruktioner överst i filen Java:

```java
// Include the following imports to use Service Bus APIs
import com.microsoft.windowsazure.services.servicebus.*;
import com.microsoft.windowsazure.services.servicebus.models.*;
import com.microsoft.windowsazure.core.*;
import javax.xml.datatype.*;
```

## <a name="create-a-queue"></a>Skapa en kö
Hanteringsåtgärder för Service Bus-köer kan utföras via den **ServiceBusContract** klass. En **ServiceBusContract** objektet konstrueras med en lämplig konfiguration som innehåller SAS-token med behörighet att hantera, och **ServiceBusContract** klassen är den enda punkten av kommunikationen med Azure.

Den **ServiceBusService** klassen innehåller metoder för att skapa, räkna upp och ta bort köer. Exemplet nedan visar hur en **ServiceBusService** objekt som kan användas för att skapa en kö med namnet `TestQueue`, med ett namnområde med namnet `HowToSample`:

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

Det finns sätt på `QueueInfo` som gör att egenskaper för kön till finjusteras (till exempel: värdet standard time to live (TTL) som ska tillämpas på meddelanden som skickas till kön). I följande exempel visas hur du skapar en kö med namnet `TestQueue` med en maximal storlek på 5 GB:

````java
long maxSizeInMegabytes = 5120;
QueueInfo queueInfo = new QueueInfo("TestQueue");
queueInfo.setMaxSizeInMegabytes(maxSizeInMegabytes);
CreateQueueResult result = service.createQueue(queueInfo);
````

Observera att du kan använda den `listQueues` metod på **ServiceBusContract** objekt för att kontrollera om det redan finns en kö med ett angivet namn i ett namnområde för tjänsten.

## <a name="send-messages-to-a-queue"></a>Skicka meddelanden till en kö
Om du vill skicka ett meddelande till en Service Bus-kö programmet hämtar en **ServiceBusContract** objekt. Följande kod visar hur du skickar ett meddelande den `TestQueue` kö som tidigare har skapats i den `HowToSample` namnområde:

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

Meddelanden som skickas till och tas emot från Service Bus-köer är instanser av den [BrokeredMessage] [ BrokeredMessage] klass. [BrokeredMessage] [ BrokeredMessage] objekt har en uppsättning standardegenskaper (t.ex [etikett](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage.label#Microsoft_ServiceBus_Messaging_BrokeredMessage_Label) och [TimeToLive](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage.timetolive#Microsoft_ServiceBus_Messaging_BrokeredMessage_TimeToLive)), en ordlista som används för att lagra anpassade programspecifika egenskaper och en brödtext med godtyckliga programdata. Ett program kan konfigurera meddelandets brödtext för meddelandet genom att skicka någon typ av serialiserbara objekt till konstruktören för den [BrokeredMessage][BrokeredMessage], och lämplig serialisering sedan används för att serialisera objektet. Du kan också tillhandahålla en **java. IO. InputStream** objekt.

I följande exempel visar hur du skickar fem testmeddelanden till den `TestQueue` **MessageSender** vi hämtades i föregående kodutdrag:

```java
for (int i=0; i<5; i++)
{
     // Create message, passing a string message for the body.
     BrokeredMessage message = new BrokeredMessage("Test message " + i);
     // Set an additional app-specific property.
     message.setProperty("MyProperty", i);
     // Send message to the queue
     service.sendQueueMessage("TestQueue", message);
}
```

Service Bus-köerna stöder en maximal meddelandestorlek på 256 kB på [standardnivån](service-bus-premium-messaging.md) och 1 MB på [premiumnivån](service-bus-premium-messaging.md). Rubriken, som inkluderar standardprogramegenskaperna och de anpassade programegenskaperna, kan ha en maximal storlek på 64 kB. Det finns ingen gräns för antalet meddelanden som kan finnas i en kö men det finns ett tak för den totala storleken för de meddelanden som ligger i en kö. Den här köstorleken definieras när kön skapas, med en övre gräns på 5 GB.

## <a name="receive-messages-from-a-queue"></a>Ta emot meddelanden från en kö
Det vanligaste sättet att ta emot meddelanden från en kö är att använda en **ServiceBusContract** objekt. Mottagna meddelanden kan arbeta i två olika lägen: **ReceiveAndDelete** och **PeekLock**.

När du använder den **ReceiveAndDelete** läge får är en engångsåtgärd – det vill säga när Service Bus tar emot en läsbegäran för ett meddelande i en kö, den markerar meddelandet som Förbrukat och skickar tillbaka det till programmet. **ReceiveAndDelete** läge (vilket är standardläget) är den enklaste modellen och fungerar bäst för scenarier där ett program kan tolerera icke-bearbetning av ett meddelande om ett fel inträffar. För att förstå detta kan du föreställa dig ett scenario där konsumenten utfärdar en receive-begäran och sedan kraschar innan den kan bearbeta denna begäran.
Eftersom Service Bus kommer att ha markerat meddelandet som Förbrukat, och sedan när programmet startas om och börjar förbruka meddelanden igen, att ha missat meddelandet som förbrukades innan kraschen.

I **PeekLock** läge får blir en åtgärd i två steg, vilket gör det möjligt att stödprogram som inte tolererar att ett meddelande saknas. När Service Bus tar emot en begäran letar det upp nästa meddelande som ska förbrukas, låser det för att förhindra att andra användare tar emot det och skickar sedan tillbaka det till programmet. När programmet har avslutat bearbetningen av meddelandet (eller lagrar det på ett tillförlitligt sätt för framtida bearbetning), Slutför det andra steget i processen genom att anropa **ta bort** för det mottagna meddelandet. När Service Bus ser den **ta bort** -anrop som den markera meddelandet som Förbrukat och tas bort från kön.

Exemplet nedan visar hur meddelanden kan tas emot och bearbetat använder **PeekLock** läge (inte standard.). Exemplet nedan har en oändlig loop och bearbetar meddelanden när de tas emot i vår `TestQueue`:

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
            // Display the queue message.
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
            // Added to handle no more messages.
            // Could instead wait for more messages to be added.
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

## <a name="how-to-handle-application-crashes-and-unreadable-messages"></a>Hantera programkrascher och oläsbara meddelanden
Service Bus innehåller funktioner som hjälper dig att återställa fel i programmet eller lösa problem med bearbetning av meddelanden på ett snyggt sätt. Om ett mottagarprogram går inte att bearbeta meddelandet av någon anledning, så kan det anropa den **unlockMessage** metod för det mottagna meddelandet (i stället för den **deleteMessage** metod). Detta gör att Service Bus låser upp meddelandet i kön och gör det tillgängligt att tas emot igen, antingen genom samma användningsprogram eller ett annat användningsprogram.

Det finns också en tidsgräns som är associerad med ett meddelande som ligger låst i kön och om programmet misslyckas med att bearbeta meddelandet innan timeout för lås går ut (till exempel om programmet kraschar), kommer Service Bus låser upp meddelandet automatiskt och gör det tillgängligt att tas emot igen.

I händelse av att programmet kraschar efter att meddelandet har bearbetats men innan det **deleteMessage** begäran utfärdas och meddelandet är levereras på nytt till programmet när den startas om. Det här kallas ofta *At Least Once Processing*, det vill säga varje meddelande bearbetas minst en gång men i vissa situationer kan samma meddelande levereras. Om scenariot inte tolererar duplicerad bearbetning, bör programutvecklarna lägga till ytterligare logik i sina program för att hantera duplicerad meddelandeleverans. Detta uppnås ofta med hjälp av den **getMessageId** metod för meddelandet som förblir konstant under alla leveransförsök.

## <a name="next-steps"></a>Nästa steg
Nu när du har lärt dig grunderna om Service Bus-köer, se [köer, ämnen och prenumerationer] [ Queues, topics, and subscriptions] för mer information.

Mer information finns på [Java Developer Center](https://azure.microsoft.com/develop/java/).

[Azure SDK for Java]: http://azure.microsoft.com/develop/java/
[Azure Toolkit for Eclipse]: https://msdn.microsoft.com/library/azure/hh694271.aspx
[Queues, topics, and subscriptions]: service-bus-queues-topics-subscriptions.md
[BrokeredMessage]: /dotnet/api/microsoft.servicebus.messaging.brokeredmessage
