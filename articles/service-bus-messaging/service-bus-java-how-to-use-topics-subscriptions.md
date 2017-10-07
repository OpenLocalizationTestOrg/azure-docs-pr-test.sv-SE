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
# <a name="how-toouse-service-bus-topics-and-subscriptions-with-java"></a>Hur toouse Service Bus-ämnen och prenumerationer med Java

[!INCLUDE [service-bus-selector-topics](../../includes/service-bus-selector-topics.md)]

Den här guiden beskriver hur toouse Service Bus-ämnen och prenumerationer. hello exemplen är skrivna i Java och använder hello [Azure SDK för Java][Azure SDK for Java]. hello beskrivs scenarier där **skapa ämnen och prenumerationer**, **skapa prenumerationsfilter**, **skickar meddelanden tooa avsnittet**, **tar emot meddelanden från en prenumeration**, och **ta bort ämnen och prenumerationer**.

## <a name="what-are-service-bus-topics-and-subscriptions"></a>Vad är Service Bus-ämnen och -prenumerationer?
Service Bus-ämnen och -prenumerationer stöder en *publicera/prenumerera*-modell för meddelandekommunikation. När du använder ämnen och prenumerationer så kommunicerar inte komponenterna i ett distribuerat program direkt med varandra. Istället så utbyter de meddelanden via ett ämne, som agerar mellanhand.

![TopicConcepts](./media/service-bus-java-how-to-use-topics-subscriptions/sb-topics-01.png)

Till skillnad från Service Bus-köer, där varje meddelande bearbetas av en enskild konsument så ger ämnen och prenumerationer en ”ett till många”-kommunikation med hjälp av ett publicera/prenumerera-mönster. Det är möjligt att registrera flera prenumerationer tooa avsnittet. När ett meddelande skickas tooa avsnittet, görs sedan tillgänglig tooeach prenumeration toohandle/bearbeta oberoende av varandra.

Ett prenumeration tooa ämne liknar en virtuell kö som tar emot kopior av hello-meddelanden som skickades toohello avsnittet. Alternativt kan du registrera filterregler för ett ämne på grundval av per prenumeration, vilket gör att du toofilter/begränsa vilka meddelanden tooa avsnittet tas emot av vilka ämnesprenumerationer.

Service Bus-ämnen och prenumerationer gör att du tooscale tooprocess ett mycket stort antal meddelanden i ett mycket stort antal användare och program.

## <a name="create-a-service-namespace"></a>Skapa ett namnområde för tjänsten
toobegin som använder Service Bus-ämnen och prenumerationer i Azure, måste du först skapa ett namnområde som innehåller en omfattningsbehållare för adressering av Service Bus-resurser i ditt program.

toocreate ett namnområde:

[!INCLUDE [service-bus-create-namespace-portal](../../includes/service-bus-create-namespace-portal.md)]

## <a name="configure-your-application-toouse-service-bus"></a>Konfigurera ditt program toouse Service Bus
Kontrollera att du har installerat hello [Azure SDK för Java] [ Azure SDK for Java] innan du skapar det här exemplet. Om du använder Eclipse, kan du installera hello [Azure Toolkit för Eclipse] [ Azure Toolkit for Eclipse] som innehåller hello Azure SDK för Java. Du kan sedan lägga till hello **Microsoft Azure-biblioteken för Java** tooyour projektet:

![](media/service-bus-java-how-to-use-topics-subscriptions/eclipselibs.png)

Lägg till följande hello `import` instruktioner toohello överkant hello Java-fil:

```java
import com.microsoft.windowsazure.services.servicebus.*;
import com.microsoft.windowsazure.services.servicebus.models.*;
import com.microsoft.windowsazure.core.*;
import javax.xml.datatype.*;
```

Lägg till hello Azure-biblioteken för Java tooyour byggsökväg och inkludera den i projektet distribution för sammansättningen.

## <a name="create-a-topic"></a>Skapa ett ämne
Hanteringsåtgärder för Service Bus-ämnen kan utföras via den **ServiceBusContract** klass. En **ServiceBusContract** objektet konstrueras med en lämplig konfiguration som innehåller SAS-token med behörigheter toomanage och hello **ServiceBusContract** klassen är hello enda punkt kommunikation med Azure.

Hej **ServiceBusService** klassen innehåller metoder toocreate, räkna upp och ta bort ämnen. hello som följande exempel visar hur en **ServiceBusService** objekt kan vara används toocreate ett ämne med namnet `TestTopic`, med ett namnområde som kallas `HowToSample`:

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

Det finns sätt på **TopicInfo** som gör att egenskaper för hello avsnittet anges (till exempel: tooset hello standard time to live (TTL) värdet toobe tillämpas toomessages skickas toohello avsnittet). hello följande exempel visas hur toocreate ett ämne med namnet `TestTopic` med en maximal storlek på 5 GB:

```java
long maxSizeInMegabytes = 5120;  
TopicInfo topicInfo = new TopicInfo("TestTopic");  
topicInfo.setMaxSizeInMegabytes(maxSizeInMegabytes);
CreateTopicResult result = service.createTopic(topicInfo);
```

Observera att du kan använda hello **listTopics** metod på **ServiceBusContract** objekt toocheck om ett ämne med ett angivet namn finns redan i ett namnområde för tjänsten.

## <a name="create-subscriptions"></a>Skapa prenumerationer
Prenumerationer tootopics skapas också med hello **ServiceBusService** klass. Prenumerationer är namngivna och kan ha ett valfritt filter som begränsar hello uppsättning av meddelanden som skickas toohello prenumerationens virtuella kö.

### <a name="create-a-subscription-with-hello-default-matchall-filter"></a>Skapa en prenumeration med hello standardfiltret (MatchAll)
Hej **MatchAll** filtret är hello standardfilter som används om inget filter anges när en ny prenumeration skapas. När hello **MatchAll** filter används, alla meddelanden publicerade toohello avsnittet placeras i prenumerationens virtuella kö. hello följande exempel skapas en prenumeration med namnet ”AllMessages” och använder hello standard **MatchAll** filter.

```java
SubscriptionInfo subInfo = new SubscriptionInfo("AllMessages");
CreateSubscriptionResult result =
    service.createSubscription("TestTopic", subInfo);
```

### <a name="create-subscriptions-with-filters"></a>Skapa prenumerationer med filter
Du kan även skapa filter som gör att du tooscope vilka meddelanden som skickas tooa avsnittet visas inom en viss ämnesprenumeration.

hello mest flexibla typen av filter som stöds av prenumerationerna är den [SqlFilter][SqlFilter], som implementerar en deluppsättning av SQL92. SQL-filter tillämpas för hälsningsmeddelande som är publicerade toohello avsnittet hello egenskaper. Mer information om hello-uttryck som kan användas med ett SQL-filter granskar hello [SqlFilter.SqlExpression] [ SqlFilter.SqlExpression] syntax.

hello följande exempel skapas en prenumeration med namnet `HighMessages` med en [SqlFilter] [ SqlFilter] objekt som endast väljer meddelanden som har en anpassad **MessageNumber** Egenskapen som är större än 3:

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

På liknande sätt hello följande exempel skapas en prenumeration med namnet `LowMessages` med en [SqlFilter] [ SqlFilter] objekt som endast väljer meddelanden som har en **MessageNumber** Egenskapen som är mindre än eller lika med too3:

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

När ett meddelande skickas nu för`TestTopic`, det alltid levereras tooreceivers prenumererar toohello `AllMessages` prenumeration och levereras selektivt tooreceivers prenumererar toohello `HighMessages` och `LowMessages` prenumerationer ( beroende på innehållet i meddelandet).

## <a name="send-messages-tooa-topic"></a>Skicka meddelanden tooa avsnittet
toosend meddelandet tooa Service Bus-ämne programmet hämtar en **ServiceBusContract** objekt. hello följande kod visar hur toosend ett meddelande för hello `TestTopic` ämne som skapats tidigare inom hello `HowToSample` namnområde:

```java
BrokeredMessage message = new BrokeredMessage("MyMessage");
service.sendTopicMessage("TestTopic", message);
```

Meddelanden som skickas tooService Bus-ämnen är instanser av den [BrokeredMessage] [ BrokeredMessage] klass. [BrokeredMessage][BrokeredMessage]* objekt har en uppsättning standardmetoderna (exempelvis **setLabel** och **TimeToLive**), en ordlista som används toohold anpassade programspecifika egenskaper och en brödtext med godtyckliga programdata. Ett program kan konfigurera hello brödtext genom att skicka någon typ av serialiserbara objekt till hello konstruktören för den [BrokeredMessage][BrokeredMessage], och hello lämpliga **DataContractSerializer**  kommer sedan att använda tooserialize hello-objekt. Du kan också en **java.io.InputStream** kan anges.

hello exemplet nedan visar hur toosend fem test meddelanden toothe `TestTopic` **MessageSender** vi fick i hello kodstycke ovan.
Observera hur hello **MessageNumber** -värdet för varje meddelande varierar på hello upprepning av loopen hello (detta avgör vilka prenumerationer som får det):

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

Service Bus-ämnena stöder en maximal meddelandestorlek på 256 KB i hello [standardnivån](service-bus-premium-messaging.md) och 1 MB på hello [premiumnivån](service-bus-premium-messaging.md). hello-huvudet som innehåller hello standard- och anpassade programegenskaperna, kan ha en maximal storlek på 64 KB. Det finns ingen gräns hello antalet meddelanden som lagras i ett ämne men det finns en gräns på hello totala storleken på hälsningsmeddelande som innehas av ett ämne. Den här ämnesstorleken definieras när ämnet skapas, med en övre gräns på 5 GB.

## <a name="how-tooreceive-messages-from-a-subscription"></a>Hur tooreceive meddelanden från en prenumeration
tooreceive meddelanden från en prenumeration som använder en **ServiceBusContract** objekt. Mottagna meddelanden kan arbeta i två olika lägen: **ReceiveAndDelete** och **PeekLock**.

När du använder hello **ReceiveAndDelete** läge får är en engångsåtgärd – det vill säga när Service Bus tar emot en läsbegäran för ett meddelande, den markerar hello meddelandet som Förbrukat och returnerar det toothe program. **ReceiveAndDelete** läge är hello enklaste modellen och fungerar bäst för scenarier där ett program kan tolerera icke-bearbetning av ett meddelande i hello händelse av fel. toounderstand, Föreställ i vilka hello konsumenten problem hello mottagning av begäran och sedan kraschar innan bearbetningen. Eftersom Service Bus kommer att ha markerat meddelandet som Förbrukat, sedan när hello programmet startas om och börjar förbruka meddelanden igen, att ha missat hello-meddelande som har förbrukats tidigare toohello krascher.

I **PeekLock** läge får blir en åtgärd i två steg, vilket gör det möjligt toosupport program som inte tolererar att ett meddelande saknas. När Service Bus tar emot en begäran, hittar hello nästa meddelande toobe förbrukas, låser det tooprevent andra användare tar emot det och returnerar sedan toohello program. När hello programmet har avslutat bearbetningen hello-meddelande (eller lagrar det på ett tillförlitligt sätt för framtida bearbetning), Slutför hello andra steget i hello får processen genom att anropa **ta bort** på emot hello-meddelande. När Service Bus ser hello **ta bort** -anrop som den Markera hello meddelandet som Förbrukat och ta bort den från hello-avsnittet.

hello exemplet nedan visar hur meddelanden kan tas emot och bearbetat använder **PeekLock** läge (inte hello standardläget). hello exemplet nedan utför en loop behandlar meddelanden i hello ”HighMessages” prenumeration och avslutas när det finns inga fler meddelanden (du kan också det gick att ange toowait för nya meddelanden).

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

## <a name="how-toohandle-application-crashes-and-unreadable-messages"></a>Hur toohandle programmet kraschar och oläsbara meddelanden
Service Bus innehåller funktioner toohelp du att återställa fel i programmet eller problem med bearbetning av ett meddelande. Om ett mottagarprogram inte kan tooprocess hello meddelande av någon anledning, så kan det anropa hello **unlockMessage** metod för hello tog emot meddelande (i stället för hello **deleteMessage** metod). Detta orsakar Service Bus toounlock hello-meddelande i hello avsnittet och göra den tillgänglig toobe tas emot igen, antingen hello av samma förbrukning av ett program eller ett annat användningsprogram.

Det finns också en tidsgräns som är associerad med ett meddelande i avsnittet och om hello program inte tooprocess hello-meddelande innan timeout för lås går ut (till exempel om hello programmet kraschar), kommer Service Bus låser upp hello-meddelande automatiskt och gör den tillgänglig toobe tas emot igen.

Hello händelse som hello programmet kraschar efter hello meddelandet har bearbetats men innan hello **deleteMessage** begäran utfärdas och sedan hello-meddelande kommer att levereras på nytt toohello program när den startas om. Det här kallas ofta **At Least Once Processing**, det vill säga, varje meddelande bearbetas minst en gång men i vissa situationer hello samma meddelande levereras. Om hello scenariot inte tolererar duplicerad bearbetning, sedan bör programutvecklarna lägga till ytterligare logik tootheir duplicerad meddelandeleverans för programmet toohandle. Detta uppnås ofta med hjälp av hello **getMessageId** metod i hello-meddelande, förblir konstant under alla leveransförsök.

## <a name="delete-topics-and-subscriptions"></a>Ta bort ämnen och prenumerationer
Hej primära sätt toodelete ämnen och prenumerationer är toouse en **ServiceBusContract** objekt. Om du tar bort ett ämne tas även bort alla prenumerationer som är registrerade med hello-avsnittet. Prenumerationer kan även tas bort separat.

```java
// Delete subscriptions
service.deleteSubscription("TestTopic", "AllMessages");
service.deleteSubscription("TestTopic", "HighMessages");
service.deleteSubscription("TestTopic", "LowMessages");

// Delete a topic
service.deleteTopic("TestTopic");
```

## <a name="next-steps"></a>Nästa steg
Nu när du har lärt dig grunderna hello i Service Bus-köer, se [Service Bus-köer, ämnen och prenumerationer] [ Service Bus queues, topics, and subscriptions] för mer information.

[Azure SDK for Java]: http://azure.microsoft.com/develop/java/
[Azure Toolkit for Eclipse]: ../azure-toolkit-for-eclipse.md
[Service Bus queues, topics, and subscriptions]: service-bus-queues-topics-subscriptions.md
[SqlFilter]: /dotnet/api/microsoft.servicebus.messaging.sqlfilter 
[SqlFilter.SqlExpression]: /dotnet/api/microsoft.servicebus.messaging.sqlfilter#Microsoft_ServiceBus_Messaging_SqlFilter_SqlExpression
[BrokeredMessage]: /dotnet/api/microsoft.servicebus.messaging.brokeredmessage

[0]: ./media/service-bus-java-how-to-use-topics-subscriptions/sb-queues-13.png
[2]: ./media/service-bus-java-how-to-use-topics-subscriptions/sb-queues-04.png
[3]: ./media/service-bus-java-how-to-use-topics-subscriptions/sb-queues-09.png
