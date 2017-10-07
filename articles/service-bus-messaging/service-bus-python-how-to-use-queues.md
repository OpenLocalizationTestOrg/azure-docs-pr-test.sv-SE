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
# <a name="how-toouse-service-bus-queues-with-python"></a>Hur toouse Service Bus köer med Python

[!INCLUDE [service-bus-selector-queues](../../includes/service-bus-selector-queues.md)]

Den här artikeln beskriver hur toouse Service Bus-köer. hello exemplen är skrivna i Python och använder hello [Python Azure Service Bus-paketet][Python Azure Service Bus package]. hello beskrivs scenarier där **skapa köer, skicka och ta emot meddelanden**, och **tar bort köer**.

[!INCLUDE [howto-service-bus-queues](../../includes/howto-service-bus-queues.md)]

[!INCLUDE [service-bus-create-namespace-portal](../../includes/service-bus-create-namespace-portal.md)]

> [!NOTE]
> tooinstall Python eller hello [Python Azure Service Bus-paketet][Python Azure Service Bus package], se hello [Python installationsguiden](../python-how-to-install.md).
> 
> 

## <a name="create-a-queue"></a>Skapa en kö
Hej **ServiceBusService** objekt kan du toowork med köer. Lägg till följande kod hello övre delen av Python-fil som du vill tooprogrammatically access Service Bus hello:

```python
from azure.servicebus import ServiceBusService, Message, Queue
```

hello följande kod skapar en **ServiceBusService** objekt. Ersätt `mynamespace`, `sharedaccesskeyname`, och `sharedaccesskey` med namnområdet, delad åtkomst (SAS)-signaturen nyckelnamn och värde.

```python
bus_service = ServiceBusService(
    service_namespace='mynamespace',
    shared_access_key_name='sharedaccesskeyname',
    shared_access_key_value='sharedaccesskey')
```

hello värden för hello SAS-nyckelnamnet och värdet finns i hello [Azure-portalen] [ Azure portal] anslutningsinformationen, eller i hello Visual Studio **egenskaper** fönstret när du väljer Hej Service Bus-namnrymd i Server Explorer (som visas i hello föregående avsnitt).

```python
bus_service.create_queue('taskqueue')
```

Hej `create_queue` metoden stöder också ytterligare alternativ som gör att du toooverride kön standardinställningarna som meddelandet toolive TTL (time) eller största köstorlek. hello följande exempel anger hello största köstorlek storlek too5 GB och hello TTL-värdet too1 minut:

```python
queue_options = Queue()
queue_options.max_size_in_megabytes = '5120'
queue_options.default_message_time_to_live = 'PT1M'

bus_service.create_queue('taskqueue', queue_options)
```

## <a name="send-messages-tooa-queue"></a>Skicka meddelanden tooa kön
toosend en meddelandekö tooa Service Bus programmet anropar hello `send_queue_message` metod på hello **ServiceBusService** objekt.

hello exemplet nedan visar hur toosend en toohello meddelandekö test med namnet `taskqueue` med `send_queue_message`:

```python
msg = Message(b'Test Message')
bus_service.send_queue_message('taskqueue', msg)
```

Service Bus-köer stöder en maximal meddelandestorlek på 256 KB i hello [standardnivån](service-bus-premium-messaging.md) och 1 MB på hello [premiumnivån](service-bus-premium-messaging.md). hello-huvudet som innehåller hello standard- och anpassade programegenskaperna, kan ha en maximal storlek på 64 KB. Det finns ingen gräns hello antalet meddelanden som finns i en kö men det finns ett tak på hello totala storleken på hälsningsmeddelande som innehas av en kö. Den här köstorleken definieras när kön skapas, med en övre gräns på 5 GB. Mer information om kvoter finns [Service Bus-kvoter][Service Bus quotas].

## <a name="receive-messages-from-a-queue"></a>Ta emot meddelanden från en kö
Meddelanden tas emot från en kö med hello `receive_queue_message` metod på hello **ServiceBusService** objekt:

```python
msg = bus_service.receive_queue_message('taskqueue', peek_lock=False)
print(msg.body)
```

Meddelanden tas bort från kön hello som de läses när hello parametern `peek_lock` har angetts för**FALSKT**. Du kan läsa (titt) och låsa hello-meddelande utan att ta bort den från hello kö genom att ange hello parametern `peek_lock` för**SANT**.

Hej beteendet för läsning och ta bort hello-meddelande som en del av hello mottagningsåtgärd är hello enklaste modellen och fungerar bäst för scenarier där ett program kan tolerera icke-bearbetning av ett meddelande i hello händelse av fel. toounderstand, Föreställ i vilka hello konsumenten problem hello mottagning av begäran och sedan kraschar innan bearbetningen. Eftersom Service Bus kommer att ha markerat hello meddelandet som Förbrukat, sedan när hello programmet startas om och börjar förbruka meddelanden igen, att ha missat hello-meddelande som har förbrukats tidigare toohello krascher.

Om hello `peek_lock` parameter har angetts för**SANT**, hello får blir en åtgärd i två steg, vilket gör det möjligt toosupport program som inte tolererar att ett meddelande saknas. När Service Bus tar emot en begäran, hittar hello nästa meddelande toobe förbrukas, låser det tooprevent andra användare tar emot det och returnerar sedan toohello program. När hello programmet har avslutat bearbetningen hello-meddelande (eller lagrar det på ett tillförlitligt sätt för framtida bearbetning), Slutför hello andra steget i hello ta emot processen genom att anropa hello **ta bort** metod på hello **meddelandet** objekt. Hej **ta bort** metoden kommer Markera hello meddelandet som Förbrukat och ta bort den från hello kö.

```python
msg = bus_service.receive_queue_message('taskqueue', peek_lock=True)
print(msg.body)

msg.delete()
```

## <a name="how-toohandle-application-crashes-and-unreadable-messages"></a>Hur toohandle programmet kraschar och oläsbara meddelanden
Service Bus innehåller funktioner toohelp du att återställa fel i programmet eller problem med bearbetning av ett meddelande. Om ett mottagarprogram inte kan tooprocess hello meddelande av någon anledning, så kan det anropa hello **låsa upp** metod på hello **meddelandet** objekt. Detta orsakar Service Bus toounlock hello-meddelande i kön för hello och göra den tillgänglig toobe tas emot igen, antingen hello av samma förbrukning av ett program eller ett annat användningsprogram.

Det finns också en tidsgräns som är associerad med ett meddelande i kön hello och om hello hello programmet misslyckas tooprocess hello meddelandet innan timeout för lås går ut (t.ex. Om hello programmet kraschar), kommer Service Bus låser upp hello-meddelande automatiskt och gör den tillgänglig toobe tas emot igen.

Hello händelse som hello programmet kraschar efter hello meddelandet har bearbetats men innan hello **ta bort** metoden anropas sedan hello-meddelande kommer att levereras på nytt toohello program när den startas om. Det här kallas ofta **At Least Once Processing**, det vill säga, varje meddelande bearbetas minst en gång men i vissa situationer hello samma meddelande levereras. Om hello scenariot inte tolererar duplicerad bearbetning, sedan bör programutvecklarna lägga till ytterligare logik tootheir duplicerad meddelandeleverans för programmet toohandle. Detta uppnås ofta med hjälp av hello **MessageId** -egenskapen för hello-meddelande, förblir konstant under alla leveransförsök.

## <a name="next-steps"></a>Nästa steg
Nu när du har lärt dig grunderna hello i Service Bus-köer finns i de här artiklarna toolearn mer.

* [Köer, ämnen och prenumerationer][Queues, topics, and subscriptions]

[Azure portal]: https://portal.azure.com
[Python Azure Service Bus package]: https://pypi.python.org/pypi/azure-servicebus  
[Queues, topics, and subscriptions]: service-bus-queues-topics-subscriptions.md
[Service Bus quotas]: service-bus-quotas.md

