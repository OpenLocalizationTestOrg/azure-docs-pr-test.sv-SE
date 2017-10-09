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
# <a name="how-toouse-service-bus-topics-and-subscriptions-with-ruby"></a>Hur toouse Service Bus-ämnen och prenumerationer med Ruby
 
[!INCLUDE [service-bus-selector-topics](../../includes/service-bus-selector-topics.md)]

Den här artikeln beskriver hur toouse Service Bus-ämnen och prenumerationer från Ruby program. hello beskrivs scenarier där **skapa ämnen och prenumerationer, skapa prenumerationsfilter, skicka meddelanden** tooa avsnittet **ta emot meddelanden från en prenumeration**, och  **ta bort ämnen och prenumerationer**. Mer information om ämnen och prenumerationer finns hello [nästa steg](#next-steps) avsnitt.

[!INCLUDE [howto-service-bus-topics](../../includes/howto-service-bus-topics.md)]

[!INCLUDE [service-bus-create-namespace-portal](../../includes/service-bus-create-namespace-portal.md)]

[!INCLUDE [service-bus-ruby-setup](../../includes/service-bus-ruby-setup.md)]

## <a name="create-a-topic"></a>Skapa ett ämne
Hej **Azure::ServiceBusService** objekt kan du toowork med ämnen. hello följande kod skapar en **Azure::ServiceBusService** objekt. toocreate ett ämne, använda hello `create_topic()` metod. hello följande exempel skapar ett ämne eller skriver ut hello fel om det finns några.

```ruby
azure_service_bus_service = Azure::ServiceBus::ServiceBusService.new(sb_host, { signer: signer})
begin
  topic = azure_service_bus_service.create_queue("test-topic")
rescue
  puts $!
end
```

Du kan också skicka ett **Azure::ServiceBus::Topic** objekt med ytterligare alternativ som gör att du toooverride avsnittet standardinställningarna, till exempel meddelande tid toolive eller maximal köstorlek. hello nedan inställningen hello största köstorlek storlek too5 GB och tid toolive too1 minut:

```ruby
topic = Azure::ServiceBus::Topic.new("test-topic")
topic.max_size_in_megabytes = 5120
topic.default_message_time_to_live = "PT1M"

topic = azure_service_bus_service.create_topic(topic)
```

## <a name="create-subscriptions"></a>Skapa prenumerationer
Ämnesprenumerationer skapas också med hello **Azure::ServiceBusService** objekt. Prenumerationer är namngivna och kan ha ett valfritt filter som begränsar hello uppsättning meddelanden som har levererats toohello prenumerationens virtuella kö.

Prenumerationer är beständiga och fortsätter tooexist förrän antingen de eller hello avsnittet de är kopplade till, tas bort. Om ditt program innehåller logik toocreate en prenumeration, bör först kontrollera om hello prenumerationen finns redan med hello getSubscription metod.

### <a name="create-a-subscription-with-hello-default-matchall-filter"></a>Skapa en prenumeration med hello standardfiltret (MatchAll)
Hej **MatchAll** filtret är hello standardfilter som används om inget filter anges när en ny prenumeration skapas. När hello **MatchAll** filter används, alla meddelanden publicerade toohello avsnittet placeras i hello prenumerationens virtuella kö. hello följande exempel skapas en prenumeration med namnet ”alla meddelanden” och använder hello standard **MatchAll** filter.

```ruby
subscription = azure_service_bus_service.create_subscription("test-topic", "all-messages")
```

### <a name="create-subscriptions-with-filters"></a>Skapa prenumerationer med filter
Du kan också definiera filter som gör att du toospecify vilka meddelanden som skickas tooa avsnittet visas inom en viss prenumeration.

hello mest flexibla typen av filter som stöds av prenumerationerna är hello **Azure::ServiceBus::SqlFilter**, som implementerar en deluppsättning av SQL92. SQL-filter tillämpas för hälsningsmeddelande som är publicerade toohello avsnittet hello egenskaper. Mer information om hello-uttryck som kan användas med ett SQL-filter granskar hello [SqlFilter](service-bus-messaging-sql-filter.md) syntax.

Du kan lägga till filter tooa prenumeration med hjälp av hello `create_rule()` metod för hello **Azure::ServiceBusService** objekt. Den här metoden kan du tooadd filter tooan befintliga prenumeration.

Eftersom hello standardfiltret används automatiskt tooall nya prenumerationer, du måste först ta bort standardfiltret hello eller hello **MatchAll** åsidosätter eventuella filter som du kan ange. Du kan ta bort hello standardregel med hello `delete_rule()` metod på hello **Azure::ServiceBusService** objekt.

hello följande exempel skapas en prenumeration med namnet ”hög-meddelanden” med en **Azure::ServiceBus::SqlFilter** som endast väljer meddelanden som har en anpassad `message_number` egenskap som är större än 3:

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

På liknande sätt hello följande exempel skapas en prenumeration med namnet `low-messages` med en **Azure::ServiceBus::SqlFilter** som endast väljer meddelanden som har en `message_number` egenskap som är mindre än eller lika med too3:

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

När ett meddelande skickas nu för`test-topic`, den är alltid vara levererade tooreceivers prenumererar toohello `all-messages` avsnittet prenumeration och levereras selektivt tooreceivers prenumererar toohello `high-messages` och `low-messages` avsnittet prenumerationer ( beroende på hello meddelandeinnehåll).

## <a name="send-messages-tooa-topic"></a>Skicka meddelanden tooa avsnittet
toosend meddelandet tooa Service Bus-ämne programmet måste använda hello `send_topic_message()` metod på hello **Azure::ServiceBusService** objekt. Meddelanden som skickas tooService Bus-ämnen är instanser av hello **Azure::ServiceBus::BrokeredMessage** objekt. **Azure::ServiceBus::BrokeredMessage** objekt har en uppsättning standardegenskaper (t.ex `label` och `time_to_live`), en ordlista som används toohold anpassade programspecifika egenskaper och en brödtext med strängdata. Ett program kan konfigurera hello brödtext hello-meddelande genom att skicka en sträng värdet toohello `send_topic_message()` metod och eventuella krävs standardegenskaper fylls i med standardvärden.

hello exemplet nedan visar hur toosend fem test meddelanden för`test-topic`. Observera att hello `message_number` anpassade egenskapsvärdet för varje meddelande varierar för hello iteration av hello loopen (detta avgör vilken prenumeration tar emot det):

```ruby
5.times do |i|
  message = Azure::ServiceBus::BrokeredMessage.new("test message " + i,
    { :message_number => i })
  azure_service_bus_service.send_topic_message("test-topic", message)
end
```

Service Bus-ämnena stöder en maximal meddelandestorlek på 256 KB i hello [standardnivån](service-bus-premium-messaging.md) och 1 MB på hello [premiumnivån](service-bus-premium-messaging.md). hello-huvudet som innehåller hello standard- och anpassade programegenskaperna, kan ha en maximal storlek på 64 KB. Det finns ingen gräns hello antalet meddelanden som lagras i ett ämne men det finns ett tak på hello totala storleken på hälsningsmeddelande som innehas av ett ämne. Den här ämnesstorleken definieras när ämnet skapas, med en övre gräns på 5 GB.

## <a name="receive-messages-from-a-subscription"></a>Ta emot meddelanden från en prenumeration
Meddelanden tas emot från en prenumeration med hjälp av hello `receive_subscription_message()` metod på hello **Azure::ServiceBusService** objekt. Som standard meddelanden read(peak) och låst utan att ta bort den från hello prenumeration. Du kan läsa och ta bort hello-meddelande från hello prenumeration genom att ange hello `peek_lock` alternativet för**FALSKT**.

hello standardbeteendet gör hello läser och tar bort en åtgärd i två steg, vilket gör det också möjligt toosupport program som inte tolererar att ett meddelande saknas. När Service Bus tar emot en begäran, hittar hello nästa meddelande toobe förbrukas, låser det tooprevent andra användare tar emot det och returnerar sedan toohello program. När hello programmet har avslutat bearbetningen hello-meddelande (eller lagrar det på ett tillförlitligt sätt för framtida bearbetning), Slutför hello andra steget i hello får processen genom att anropa `delete_subscription_message()` metod och ge hello meddelandet toobe tas bort som en parameter. Hej `delete_subscription_message()` metoden kommer Markera hello meddelandet som Förbrukat och ta bort den från hello prenumeration.

Om hello `:peek_lock` parameter har angetts för**FALSKT**, läser och tar bort hello-meddelande blir hello enklaste modellen och fungerar bäst för scenarier där ett program kan tolerera icke-bearbetning av ett meddelande i hello-händelse en ett fel uppstod. toounderstand, Föreställ i vilka hello konsumenten problem hello mottagning av begäran och sedan kraschar innan bearbetningen. Eftersom Service Bus kommer att ha markerat hello meddelandet som Förbrukat, sedan när hello programmet startas om och börjar förbruka meddelanden igen, att ha missat hello-meddelande som har förbrukats tidigare toohello krascher.

hello exemplet nedan visar hur meddelanden kan tas emot och bearbetat använder `receive_subscription_message()`. hello exempel först tar emot och tar bort ett meddelande från hello `low-messages` prenumeration med hjälp av `:peek_lock` ställa in också**FALSKT**, och sedan ett nytt meddelande tas emot från hello `high-messages` och sedan tar bort hello meddelande med `delete_subscription_message()`:

```ruby
message = azure_service_bus_service.receive_subscription_message(
  "test-topic", "low-messages", { :peek_lock => false })
message = azure_service_bus_service.receive_subscription_message(
  "test-topic", "high-messages")
azure_service_bus_service.delete_subscription_message(message)
```

## <a name="how-toohandle-application-crashes-and-unreadable-messages"></a>Hur toohandle programmet kraschar och oläsbara meddelanden
Service Bus innehåller funktioner toohelp du att återställa fel i programmet eller problem med bearbetning av ett meddelande. Om ett mottagarprogram inte kan tooprocess hello meddelande av någon anledning, så kan det anropa hello `unlock_subscription_message()` metod på hello **Azure::ServiceBusService** objekt. Detta medför att Service Bus toounlock hello meddelande inom hello prenumeration och göra den tillgänglig toobe tas emot igen, hello antingen av samma förbrukning av ett program eller ett annat användningsprogram.

Det finns också en tidsgräns som är associerad med ett meddelande i hello prenumeration och om hello program inte tooprocess hello-meddelande innan timeout för lås hello går ut (till exempel om hello programmet kraschar), kommer Service Bus att låsa upp hello-meddelande automatiskt och gör den tillgänglig toobe tas emot igen.

Hello händelse som hello programmet kraschar efter hello meddelandet har bearbetats men innan hello `delete_subscription_message()` metoden anropas sedan hello-meddelande är toohello levereras på nytt program när den startas om. Det här kallas ofta *At Least Once Processing*, det vill säga varje meddelande bearbetas minst en gång men i vissa situationer hello samma meddelande kan levereras. Om hello scenariot inte tolererar duplicerad bearbetning, sedan bör programutvecklarna lägga till ytterligare logik tootheir duplicerad meddelandeleverans för programmet toohandle. Den här logiken uppnås ofta med hjälp av hello `message_id` -egenskapen för hello-meddelande, förblir konstant under alla leveransförsök.

## <a name="delete-topics-and-subscriptions"></a>Ta bort ämnen och prenumerationer
Ämnen och prenumerationer är beständiga och måste vara explicit bort antingen via hello [Azure-portalen] [ Azure portal] eller programmässigt. hello exemplet nedan visar hur toodelete hello avsnittet med namnet `test-topic`.

```ruby
azure_service_bus_service.delete_topic("test-topic")
```

Om du tar bort ett ämne tas även bort alla prenumerationer som är registrerade med hello-avsnittet. Prenumerationer kan även tas bort separat. hello följande kod visar hur toodelete hello prenumeration med namnet `high-messages` från hello `test-topic` avsnittet:

```ruby
azure_service_bus_service.delete_subscription("test-topic", "high-messages")
```

## <a name="next-steps"></a>Nästa steg
Nu när du har lärt dig grunderna hello i Service Bus-ämnen, följa dessa länkar toolearn mer.

* Se [köer, ämnen och prenumerationer](service-bus-queues-topics-subscriptions.md).
* API-referens för [SqlFilter](/dotnet/api/microsoft.servicebus.messaging.sqlfilter#microsoft_servicebus_messaging_sqlfilter).
* Besök hello [Azure SDK för Ruby](https://github.com/Azure/azure-sdk-for-ruby) databasen på GitHub.

[Azure portal]: https://portal.azure.com
