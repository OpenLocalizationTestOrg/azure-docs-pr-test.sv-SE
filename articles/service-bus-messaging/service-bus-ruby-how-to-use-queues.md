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
# <a name="how-toouse-service-bus-queues-with-ruby"></a>Hur toouse Service Bus köer med Ruby

[!INCLUDE [service-bus-selector-queues](../../includes/service-bus-selector-queues.md)]

Den här guiden beskriver hur toouse Service Bus-köer. hello exemplen är skrivna i Ruby och använder hello Azure symbolen. hello beskrivs scenarier där **skapa köer, skicka och ta emot meddelanden**, och **tar bort köer**. Mer information om Service Bus-köer finns hello [nästa steg](#next-steps) avsnitt.

[!INCLUDE [howto-service-bus-queues](../../includes/howto-service-bus-queues.md)]

[!INCLUDE [service-bus-create-namespace-portal](../../includes/service-bus-create-namespace-portal.md)]
   
[!INCLUDE [service-bus-ruby-setup](../../includes/service-bus-ruby-setup.md)]

## <a name="how-toocreate-a-queue"></a>Hur toocreate en kö
Hej **Azure::ServiceBusService** objekt kan du toowork med köer. toocreate en kö, använda hello `create_queue()` metod. hello följande exempel skapar en kö eller skriver ut eventuella fel.

```ruby
azure_service_bus_service = Azure::ServiceBus::ServiceBusService.new(sb_host, { signer: signer})
begin
  queue = azure_service_bus_service.create_queue("test-queue")
rescue
  puts $!
end
```

Du kan också skicka en **Azure::ServiceBus::Queue** objekt med ytterligare alternativ som gör att du toooverride hello kön standardinställningarna, till exempel meddelande tid toolive eller maximal köstorlek. hello följande exempel visas hur tooset hello största köstorlek storlek too5 GB och tid toolive too1 minut:

```ruby
queue = Azure::ServiceBus::Queue.new("test-queue")
queue.max_size_in_megabytes = 5120
queue.default_message_time_to_live = "PT1M"

queue = azure_service_bus_service.create_queue(queue)
```

## <a name="how-toosend-messages-tooa-queue"></a>Hur toosend meddelanden tooa kön
toosend en meddelandekö tooa Service Bus programmet anropar hello `send_queue_message()` metod på hello **Azure::ServiceBusService** objekt. Meddelanden skickas för (och mottagna från) Service Bus är köer **Azure::ServiceBus::BrokeredMessage** objekt och har en uppsättning standardegenskaper (t.ex `label` och `time_to_live`), en ordlista som används toohold anpassade programspecifika egenskaper och en brödtext med godtyckliga programdata. Ett program kan konfigurera hello brödtext hello-meddelande genom att skicka ett strängvärde som hello-meddelande och eventuella obligatoriska standard egenskaper fylls i med standardvärden.

hello exemplet nedan visar hur toosend en toohello meddelandekö test med namnet `test-queue` med `send_queue_message()`:

```ruby
message = Azure::ServiceBus::BrokeredMessage.new("test queue message")
message.correlation_id = "test-correlation-id"
azure_service_bus_service.send_queue_message("test-queue", message)
```

Service Bus-köer stöder en maximal meddelandestorlek på 256 KB i hello [standardnivån](service-bus-premium-messaging.md) och 1 MB på hello [premiumnivån](service-bus-premium-messaging.md). hello-huvudet som innehåller hello standard- och anpassade programegenskaperna, kan ha en maximal storlek på 64 KB. Det finns ingen gräns hello antalet meddelanden som finns i en kö men det finns ett tak på hello totala storleken på hälsningsmeddelande som innehas av en kö. Den här köstorleken definieras när kön skapas, med en övre gräns på 5 GB.

## <a name="how-tooreceive-messages-from-a-queue"></a>Hur tooreceive meddelanden från en kö
Meddelanden tas emot från en kö med hello `receive_queue_message()` metod på hello **Azure::ServiceBusService** objekt. Som standard, läsa och låst utan tas bort från kön hello meddelanden. Du kan dock ta bort meddelanden från kön hello som de läses av inställningen hello `:peek_lock` alternativet för**FALSKT**.

hello standardbeteendet gör hello läser och tar bort en åtgärd i två steg, vilket gör det också möjligt toosupport program som inte tolererar att ett meddelande saknas. När Service Bus tar emot en begäran, hittar hello nästa meddelande toobe förbrukas, låser det tooprevent andra användare tar emot det och returnerar sedan toohello program. När hello programmet har avslutat bearbetningen hello-meddelande (eller lagrar det på ett tillförlitligt sätt för framtida bearbetning), Slutför hello andra steget i hello får processen genom att anropa `delete_queue_message()` metod och ge hello meddelandet toobe tas bort som en parameter. Hej `delete_queue_message()` metoden kommer Markera hello meddelandet som Förbrukat och ta bort den från hello kö.

Om hello `:peek_lock` parameter har angetts för**FALSKT**, läser och tar bort hello-meddelande blir hello enklaste modellen och fungerar bäst för scenarier där ett program kan tolerera icke-bearbetning av ett meddelande i hello-händelse en ett fel uppstod. toounderstand, Föreställ i vilka hello konsumenten problem hello mottagning av begäran och sedan kraschar innan bearbetningen. Eftersom Service Bus har markerats hello-meddelande som används när hello programmet startas om och börjar förbruka meddelanden igen, att ha missat hello-meddelande som har förbrukats tidigare toohello krascher.

hello exemplet nedan visar hur tooreceive och bearbeta meddelanden med hjälp av `receive_queue_message()`. hello exempel först tar emot och tar bort ett meddelande med hjälp av `:peek_lock` ställa in också**FALSKT**, den får ett nytt meddelande och sedan tar bort hello meddelande med `delete_queue_message()`:

```ruby
message = azure_service_bus_service.receive_queue_message("test-queue",
  { :peek_lock => false })
message = azure_service_bus_service.receive_queue_message("test-queue")
azure_service_bus_service.delete_queue_message(message)
```

## <a name="how-toohandle-application-crashes-and-unreadable-messages"></a>Hur toohandle programmet kraschar och oläsbara meddelanden
Service Bus innehåller funktioner toohelp du att återställa fel i programmet eller problem med bearbetning av ett meddelande. Om ett mottagarprogram inte kan tooprocess hello meddelande av någon anledning, så kan det anropa hello `unlock_queue_message()` metod på hello **Azure::ServiceBusService** objekt. Detta medför att anropa Service Bus toounlock hello meddelandet i kön för hello och gör den tillgänglig toobe tas emot igen, hello antingen av samma förbrukning av ett program eller ett annat användningsprogram.

Det finns också en tidsgräns som är associerad med ett meddelande i kön hello och om hello program inte tooprocess hello-meddelande innan timeout för lås hello går ut (till exempel om hello programmet kraschar), kommer Service Bus låser upp hello-meddelande automatiskt och gör den tillgänglig toobe tas emot igen.

Hello händelse som hello programmet kraschar efter hello meddelandet har bearbetats men innan hello `delete_queue_message()` metoden anropas sedan hello-meddelande är toohello levereras på nytt program när den startas om. Den här processen kallas ofta *At Least Once Processing*, det vill säga varje meddelande bearbetas minst en gång men i vissa situationer hello samma meddelande kan levereras. Om hello scenariot inte tolererar duplicerad bearbetning, sedan bör programutvecklarna lägga till ytterligare logik tootheir duplicerad meddelandeleverans för programmet toohandle. Detta uppnås ofta med hjälp av hello `message_id` -egenskapen för hello-meddelande, förblir konstant under alla leveransförsök.

## <a name="next-steps"></a>Nästa steg
Nu när du har lärt dig grunderna hello i Service Bus-köer, följa dessa länkar toolearn mer.

* Översikt över [köer, ämnen och prenumerationer](service-bus-queues-topics-subscriptions.md).
* Besök hello [Azure SDK för Ruby](https://github.com/Azure/azure-sdk-for-ruby) databasen på GitHub.

En jämförelse mellan hello Azure Service Bus-köer i den här artikeln och Azure-köer som beskrivs i hello [hur toouse Queue storage från Ruby](../storage/queues/storage-ruby-how-to-use-queue-storage.md) artikel, se [Azure köer och Azure Service Bus-köer - jämfört med och skillnad från](service-bus-azure-and-service-bus-queues-compared-contrasted.md)

