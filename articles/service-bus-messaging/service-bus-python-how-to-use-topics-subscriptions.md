---
title: "aaaHow toouse Azure Service Bus-ämnen med Python | Microsoft Docs"
description: "Lär dig hur toouse Azure Service Bus-ämnen och prenumerationer från Python."
services: service-bus-messaging
documentationcenter: python
author: sethmanheim
manager: timlt
editor: 
ms.assetid: c4f1d76c-7567-4b33-9193-3788f82934e4
ms.service: service-bus-messaging
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: python
ms.topic: article
ms.date: 08/10/2017
ms.author: sethm
ms.openlocfilehash: 1171cbe8061bb3d73e2ce92ecc0cf45afae37054
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="how-toouse-service-bus-topics-and-subscriptions-with-python"></a>Hur toouse Service Bus-ämnen och prenumerationer med Python

[!INCLUDE [service-bus-selector-topics](../../includes/service-bus-selector-topics.md)]

Den här artikeln beskriver hur toouse Service Bus-ämnen och prenumerationer. hello exemplen är skrivna i Python och använder hello [Azure Python SDK-paketet][Azure Python package]. hello beskrivs scenarier där **skapa ämnen och prenumerationer**, **skapa prenumerationsfilter**, **skickar meddelanden tooa avsnittet**, **tar emot meddelanden från en prenumeration**, och **ta bort ämnen och prenumerationer**. Mer information om ämnen och prenumerationer finns hello [nästa steg](#next-steps) avsnitt.

[!INCLUDE [howto-service-bus-topics](../../includes/howto-service-bus-topics.md)]

> [!NOTE] 
> Om du behöver tooinstall Python eller hello [Azure Python-paketet][Azure Python package], se hello [Python installationsguiden](../python-how-to-install.md).

## <a name="create-a-topic"></a>Skapa ett ämne
Hej **ServiceBusService** objekt kan du toowork med ämnen. Lägg till följande hello hello övre delen av Python-fil som du vill tooprogrammatically access Service Bus:

```python
from azure.servicebus import ServiceBusService, Message, Topic, Rule, DEFAULT_RULE_NAME
```

hello följande kod skapar en **ServiceBusService** objekt. Ersätt `mynamespace`, `sharedaccesskeyname`, och `sharedaccesskey` med det faktiska namnområdet delade signatur åtkomst (SAS) nyckelvärdet namn och nyckel.

```python
bus_service = ServiceBusService(
    service_namespace='mynamespace',
    shared_access_key_name='sharedaccesskeyname',
    shared_access_key_value='sharedaccesskey')
```

Du kan hämta hello värden för hello SAS-nyckelnamnet och värdet från hello [Azure-portalen][Azure portal].

```python
bus_service.create_topic('mytopic')
```

Hej `create_topic` metoden stöder också ytterligare alternativ som aktiverar toooverride avsnittet standardinställningarna, till exempel meddelandestorlek tid toolive eller maximal ämne. hello följande exempel anger hello maximala avsnittet storlek too5 GB och en tid toolive TTL-värde på 1 minut:

```python
topic_options = Topic()
topic_options.max_size_in_megabytes = '5120'
topic_options.default_message_time_to_live = 'PT1M'

bus_service.create_topic('mytopic', topic_options)
```

## <a name="create-subscriptions"></a>Skapa prenumerationer
Prenumerationer tootopics skapas också med hello **ServiceBusService** objekt. Prenumerationer är namngivna och kan ha ett valfritt filter som begränsar hello uppsättning meddelanden som har levererats toohello prenumerationens virtuella kö.

> [!NOTE]
> Prenumerationer är beständiga och fortsätter tooexist förrän antingen de eller hello avsnittet toowhich de prenumererar tas bort.
> 
> 

### <a name="create-a-subscription-with-hello-default-matchall-filter"></a>Skapa en prenumeration med hello standardfiltret (MatchAll)
Hej **MatchAll** filtret är hello standardfilter som används om inget filter anges när en ny prenumeration skapas. När hello **MatchAll** filter används, alla meddelanden publicerade toohello avsnittet placeras i hello prenumerationens virtuella kö. hello följande exempel skapas en prenumeration med namnet `AllMessages` och använder hello standard **MatchAll** filter.

```python
bus_service.create_subscription('mytopic', 'AllMessages')
```

### <a name="create-subscriptions-with-filters"></a>Skapa prenumerationer med filter
Du kan också definiera filter som gör att du toospecify vilka meddelanden som skickas tooa avsnittet visas inom en viss ämnesprenumeration.

hello mest flexibla typen av filter som stöds av prenumerationerna är en **SqlFilter**, som implementerar en deluppsättning av SQL92. SQL-filter tillämpas för hälsningsmeddelande som är publicerade toohello avsnittet hello egenskaper. Mer information om hello-uttryck som kan användas med ett SQL-filter finns hello [SqlFilter.SqlExpression] [ SqlFilter.SqlExpression] syntax.

Du kan lägga till filter tooa prenumeration med hjälp av hello **skapa\_regeln** metod för hello **ServiceBusService** objekt. Den här metoden kan du tooadd filter tooan befintliga prenumeration.

> [!NOTE]
> Eftersom hello standardfiltret används automatiskt tooall nya prenumerationer, måste du först ta bort standardfiltret hello eller hello **MatchAll** åsidosätter eventuella filter som du kan ange. Du kan ta bort hello standardregel med hello `delete_rule` metod för hello **ServiceBusService** objekt.
> 
> 

hello följande exempel skapas en prenumeration med namnet `HighMessages` med en **SqlFilter** som endast väljer meddelanden som har en anpassad `messagenumber` egenskap som är större än 3:

```python
bus_service.create_subscription('mytopic', 'HighMessages')

rule = Rule()
rule.filter_type = 'SqlFilter'
rule.filter_expression = 'messagenumber > 3'

bus_service.create_rule('mytopic', 'HighMessages', 'HighMessageFilter', rule)
bus_service.delete_rule('mytopic', 'HighMessages', DEFAULT_RULE_NAME)
```

På liknande sätt hello följande exempel skapas en prenumeration med namnet `LowMessages` med en **SqlFilter** som endast väljer meddelanden som har en `messagenumber` egenskap som är mindre än eller lika med too3:

```python
bus_service.create_subscription('mytopic', 'LowMessages')

rule = Rule()
rule.filter_type = 'SqlFilter'
rule.filter_expression = 'messagenumber <= 3'

bus_service.create_rule('mytopic', 'LowMessages', 'LowMessageFilter', rule)
bus_service.delete_rule('mytopic', 'LowMessages', DEFAULT_RULE_NAME)
```

Nu när ett meddelande skickas för`mytopic` levereras det alltid tooreceivers prenumererar toohello **AllMessages** avsnittet prenumeration och levereras selektivt tooreceivers prenumererar toohello **HighMessages**  och **Highmessages** (beroende på hello meddelandeinnehåll).

## <a name="send-messages-tooa-topic"></a>Skicka meddelanden tooa avsnittet
toosend meddelandet tooa Service Bus-ämne programmet måste använda hello `send_topic_message` metod för hello **ServiceBusService** objekt.

hello exemplet nedan visar hur toosend fem test meddelanden för`mytopic`. Observera att hello `messagenumber` -värdet för varje meddelande varierar på hello upprepning av loopen hello (detta avgör vilka prenumerationer som får det):

```python
for i in range(5):
    msg = Message('Msg {0}'.format(i).encode('utf-8'), custom_properties={'messagenumber':i})
    bus_service.send_topic_message('mytopic', msg)
```

Service Bus-ämnena stöder en maximal meddelandestorlek på 256 KB i hello [standardnivån](service-bus-premium-messaging.md) och 1 MB på hello [premiumnivån](service-bus-premium-messaging.md). hello-huvudet som innehåller hello standard- och anpassade programegenskaperna, kan ha en maximal storlek på 64 KB. Det finns ingen gräns hello antalet meddelanden som lagras i ett ämne men det finns ett tak på hello totala storleken på hälsningsmeddelande som innehas av ett ämne. Den här ämnesstorleken definieras när ämnet skapas, med en övre gräns på 5 GB. Mer information om kvoter finns [Service Bus-kvoter][Service Bus quotas].

## <a name="receive-messages-from-a-subscription"></a>Ta emot meddelanden från en prenumeration
Meddelanden tas emot från en prenumeration med hjälp av hello `receive_subscription_message` metod på hello **ServiceBusService** objekt:

```python
msg = bus_service.receive_subscription_message('mytopic', 'LowMessages', peek_lock=False)
print(msg.body)
```

Meddelanden tas bort från hello prenumeration som de läses när hello parametern `peek_lock` har angetts för**FALSKT**. Du kan läsa (titt) och låsa hello-meddelande utan att ta bort den från hello kö genom att ange hello parametern `peek_lock` för**SANT**.

Hej beteendet för läsning och ta bort hello-meddelande som en del av hello mottagningsåtgärd är hello enklaste modellen och fungerar bäst för scenarier där ett program kan tolerera icke-bearbetning av ett meddelande i hello händelse av fel. toounderstand, Föreställ i vilka hello konsumenten problem hello mottagning av begäran och sedan kraschar innan bearbetningen. Eftersom Service Bus kommer att ha markerat hello meddelandet som Förbrukat, sedan när hello programmet startas om och börjar förbruka meddelanden igen, att ha missat hello-meddelande som har förbrukats tidigare toohello krascher.

Om hello `peek_lock` parameter har angetts för**SANT**, hello får blir en åtgärd i två steg, vilket gör det möjligt toosupport program som inte tolererar att ett meddelande saknas. När Service Bus tar emot en begäran, hittar hello nästa meddelande toobe förbrukas, låser det tooprevent andra användare tar emot det och returnerar sedan toohello program. När hello programmet har avslutat bearbetningen hello-meddelande (eller lagrar det på ett tillförlitligt sätt för framtida bearbetning), Slutför hello andra steget i hello får processen genom att anropa `delete` metod på hello **meddelandet** objekt. Hej `delete` metoden markerar hello meddelandet som Förbrukat och tar bort det från hello prenumeration.

```python
msg = bus_service.receive_subscription_message('mytopic', 'LowMessages', peek_lock=True)
print(msg.body)

msg.delete()
```

## <a name="how-toohandle-application-crashes-and-unreadable-messages"></a>Hur toohandle programmet kraschar och oläsbara meddelanden
Service Bus innehåller funktioner toohelp du att återställa fel i programmet eller problem med bearbetning av ett meddelande. Om ett mottagarprogram inte kan tooprocess hello meddelande av någon anledning, så kan det anropa hello `unlock` metod på hello **meddelandet** objekt. Detta orsakar Service Bus toounlock hello-meddelande inom hello prenumeration och göra den tillgänglig toobe tas emot igen, antingen hello av samma förbrukning av ett program eller ett annat användningsprogram.

Det finns också en tidsgräns som är associerad med ett meddelande i hello prenumeration och om hello program inte tooprocess hello-meddelande innan timeout för lås hello går ut (till exempel om hello programmet kraschar), kommer Service Bus låser upp hello-meddelande automatiskt och gör den tillgänglig toobe tas emot igen.

Hello händelse som hello programmet kraschar efter hello meddelandet har bearbetats men innan hello `delete` metoden anropas sedan hello-meddelande kommer att levereras på nytt toohello program när den startas om. Det här kallas ofta *At Least Once Processing*, det vill säga, varje meddelande bearbetas minst en gång men i vissa situationer hello samma meddelande levereras. Om hello scenariot inte tolererar duplicerad bearbetning, sedan bör programutvecklarna lägga till ytterligare logik tootheir duplicerad meddelandeleverans för programmet toohandle. Detta uppnås ofta med hjälp av hello **MessageId** -egenskapen för hello-meddelande, förblir konstant under alla leveransförsök.

## <a name="delete-topics-and-subscriptions"></a>Ta bort ämnen och prenumerationer
Ämnen och prenumerationer är beständiga och måste vara explicit bort antingen via hello [Azure-portalen] [ Azure portal] eller programmässigt. hello följande exempel visas hur toodelete hello avsnittet med namnet `mytopic`:

```python
bus_service.delete_topic('mytopic')
```

Om du tar bort ett ämne tas även bort alla prenumerationer som är registrerade med hello-avsnittet. Prenumerationer kan även tas bort separat. hello följande kod visar hur toodelete en prenumeration med namnet `HighMessages` från hello `mytopic` avsnittet:

```python
bus_service.delete_subscription('mytopic', 'HighMessages')
```

## <a name="next-steps"></a>Nästa steg
Nu när du har lärt dig grunderna hello i Service Bus-ämnen, följa dessa länkar toolearn mer.

* Se [köer, ämnen och prenumerationer][Queues, topics, and subscriptions].
* Referens för [SqlFilter.SqlExpression][SqlFilter.SqlExpression].

[Azure portal]: https://portal.azure.com
[Azure Python package]: https://pypi.python.org/pypi/azure  
[Queues, topics, and subscriptions]: service-bus-queues-topics-subscriptions.md
[SqlFilter.SqlExpression]: service-bus-messaging-sql-filter.md
[Service Bus quotas]: service-bus-quotas.md 
