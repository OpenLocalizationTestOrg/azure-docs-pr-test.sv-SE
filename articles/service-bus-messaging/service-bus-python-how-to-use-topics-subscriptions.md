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
# <a name="how-toouse-service-bus-topics-and-subscriptions-with-python"></a><span data-ttu-id="72c24-103">Hur toouse Service Bus-ämnen och prenumerationer med Python</span><span class="sxs-lookup"><span data-stu-id="72c24-103">How toouse Service Bus topics and subscriptions with Python</span></span>

[!INCLUDE [service-bus-selector-topics](../../includes/service-bus-selector-topics.md)]

<span data-ttu-id="72c24-104">Den här artikeln beskriver hur toouse Service Bus-ämnen och prenumerationer.</span><span class="sxs-lookup"><span data-stu-id="72c24-104">This article describes how toouse Service Bus topics and subscriptions.</span></span> <span data-ttu-id="72c24-105">hello exemplen är skrivna i Python och använder hello [Azure Python SDK-paketet][Azure Python package].</span><span class="sxs-lookup"><span data-stu-id="72c24-105">hello samples are written in Python and use hello [Azure Python SDK package][Azure Python package].</span></span> <span data-ttu-id="72c24-106">hello beskrivs scenarier där **skapa ämnen och prenumerationer**, **skapa prenumerationsfilter**, **skickar meddelanden tooa avsnittet**, **tar emot meddelanden från en prenumeration**, och **ta bort ämnen och prenumerationer**.</span><span class="sxs-lookup"><span data-stu-id="72c24-106">hello scenarios covered include **creating topics and subscriptions**, **creating subscription filters**, **sending messages tooa topic**, **receiving messages from a subscription**, and **deleting topics and subscriptions**.</span></span> <span data-ttu-id="72c24-107">Mer information om ämnen och prenumerationer finns hello [nästa steg](#next-steps) avsnitt.</span><span class="sxs-lookup"><span data-stu-id="72c24-107">For more information about topics and subscriptions, see hello [Next Steps](#next-steps) section.</span></span>

[!INCLUDE [howto-service-bus-topics](../../includes/howto-service-bus-topics.md)]

> [!NOTE] 
> <span data-ttu-id="72c24-108">Om du behöver tooinstall Python eller hello [Azure Python-paketet][Azure Python package], se hello [Python installationsguiden](../python-how-to-install.md).</span><span class="sxs-lookup"><span data-stu-id="72c24-108">If you need tooinstall Python or hello [Azure Python package][Azure Python package], see hello [Python Installation Guide](../python-how-to-install.md).</span></span>

## <a name="create-a-topic"></a><span data-ttu-id="72c24-109">Skapa ett ämne</span><span class="sxs-lookup"><span data-stu-id="72c24-109">Create a topic</span></span>
<span data-ttu-id="72c24-110">Hej **ServiceBusService** objekt kan du toowork med ämnen.</span><span class="sxs-lookup"><span data-stu-id="72c24-110">hello **ServiceBusService** object enables you toowork with topics.</span></span> <span data-ttu-id="72c24-111">Lägg till följande hello hello övre delen av Python-fil som du vill tooprogrammatically access Service Bus:</span><span class="sxs-lookup"><span data-stu-id="72c24-111">Add hello following near hello top of any Python file in which you wish tooprogrammatically access Service Bus:</span></span>

```python
from azure.servicebus import ServiceBusService, Message, Topic, Rule, DEFAULT_RULE_NAME
```

<span data-ttu-id="72c24-112">hello följande kod skapar en **ServiceBusService** objekt.</span><span class="sxs-lookup"><span data-stu-id="72c24-112">hello following code creates a **ServiceBusService** object.</span></span> <span data-ttu-id="72c24-113">Ersätt `mynamespace`, `sharedaccesskeyname`, och `sharedaccesskey` med det faktiska namnområdet delade signatur åtkomst (SAS) nyckelvärdet namn och nyckel.</span><span class="sxs-lookup"><span data-stu-id="72c24-113">Replace `mynamespace`, `sharedaccesskeyname`, and `sharedaccesskey` with your actual namespace, Shared Access Signature (SAS) key name, and key value.</span></span>

```python
bus_service = ServiceBusService(
    service_namespace='mynamespace',
    shared_access_key_name='sharedaccesskeyname',
    shared_access_key_value='sharedaccesskey')
```

<span data-ttu-id="72c24-114">Du kan hämta hello värden för hello SAS-nyckelnamnet och värdet från hello [Azure-portalen][Azure portal].</span><span class="sxs-lookup"><span data-stu-id="72c24-114">You can obtain hello values for hello SAS key name and value from hello [Azure portal][Azure portal].</span></span>

```python
bus_service.create_topic('mytopic')
```

<span data-ttu-id="72c24-115">Hej `create_topic` metoden stöder också ytterligare alternativ som aktiverar toooverride avsnittet standardinställningarna, till exempel meddelandestorlek tid toolive eller maximal ämne.</span><span class="sxs-lookup"><span data-stu-id="72c24-115">hello `create_topic` method also supports additional options, which enable you toooverride default topic settings such as message time toolive or maximum topic size.</span></span> <span data-ttu-id="72c24-116">hello följande exempel anger hello maximala avsnittet storlek too5 GB och en tid toolive TTL-värde på 1 minut:</span><span class="sxs-lookup"><span data-stu-id="72c24-116">hello following example sets hello maximum topic size too5 GB, and a time toolive (TTL) value of 1 minute:</span></span>

```python
topic_options = Topic()
topic_options.max_size_in_megabytes = '5120'
topic_options.default_message_time_to_live = 'PT1M'

bus_service.create_topic('mytopic', topic_options)
```

## <a name="create-subscriptions"></a><span data-ttu-id="72c24-117">Skapa prenumerationer</span><span class="sxs-lookup"><span data-stu-id="72c24-117">Create subscriptions</span></span>
<span data-ttu-id="72c24-118">Prenumerationer tootopics skapas också med hello **ServiceBusService** objekt.</span><span class="sxs-lookup"><span data-stu-id="72c24-118">Subscriptions tootopics are also created with hello **ServiceBusService** object.</span></span> <span data-ttu-id="72c24-119">Prenumerationer är namngivna och kan ha ett valfritt filter som begränsar hello uppsättning meddelanden som har levererats toohello prenumerationens virtuella kö.</span><span class="sxs-lookup"><span data-stu-id="72c24-119">Subscriptions are named and can have an optional filter that restricts hello set of messages delivered toohello subscription's virtual queue.</span></span>

> [!NOTE]
> <span data-ttu-id="72c24-120">Prenumerationer är beständiga och fortsätter tooexist förrän antingen de eller hello avsnittet toowhich de prenumererar tas bort.</span><span class="sxs-lookup"><span data-stu-id="72c24-120">Subscriptions are persistent and will continue tooexist until either they, or hello topic toowhich they are subscribed, are deleted.</span></span>
> 
> 

### <a name="create-a-subscription-with-hello-default-matchall-filter"></a><span data-ttu-id="72c24-121">Skapa en prenumeration med hello standardfiltret (MatchAll)</span><span class="sxs-lookup"><span data-stu-id="72c24-121">Create a subscription with hello default (MatchAll) filter</span></span>
<span data-ttu-id="72c24-122">Hej **MatchAll** filtret är hello standardfilter som används om inget filter anges när en ny prenumeration skapas.</span><span class="sxs-lookup"><span data-stu-id="72c24-122">hello **MatchAll** filter is hello default filter that is used if no filter is specified when a new subscription is created.</span></span> <span data-ttu-id="72c24-123">När hello **MatchAll** filter används, alla meddelanden publicerade toohello avsnittet placeras i hello prenumerationens virtuella kö.</span><span class="sxs-lookup"><span data-stu-id="72c24-123">When hello **MatchAll** filter is used, all messages published toohello topic are placed in hello subscription's virtual queue.</span></span> <span data-ttu-id="72c24-124">hello följande exempel skapas en prenumeration med namnet `AllMessages` och använder hello standard **MatchAll** filter.</span><span class="sxs-lookup"><span data-stu-id="72c24-124">hello following example creates a subscription named `AllMessages` and uses hello default **MatchAll** filter.</span></span>

```python
bus_service.create_subscription('mytopic', 'AllMessages')
```

### <a name="create-subscriptions-with-filters"></a><span data-ttu-id="72c24-125">Skapa prenumerationer med filter</span><span class="sxs-lookup"><span data-stu-id="72c24-125">Create subscriptions with filters</span></span>
<span data-ttu-id="72c24-126">Du kan också definiera filter som gör att du toospecify vilka meddelanden som skickas tooa avsnittet visas inom en viss ämnesprenumeration.</span><span class="sxs-lookup"><span data-stu-id="72c24-126">You can also define filters that enable you toospecify which messages sent tooa topic should show up within a specific topic subscription.</span></span>

<span data-ttu-id="72c24-127">hello mest flexibla typen av filter som stöds av prenumerationerna är en **SqlFilter**, som implementerar en deluppsättning av SQL92.</span><span class="sxs-lookup"><span data-stu-id="72c24-127">hello most flexible type of filter supported by subscriptions is a **SqlFilter**, which implements a subset of SQL92.</span></span> <span data-ttu-id="72c24-128">SQL-filter tillämpas för hälsningsmeddelande som är publicerade toohello avsnittet hello egenskaper.</span><span class="sxs-lookup"><span data-stu-id="72c24-128">SQL filters operate on hello properties of hello messages that are published toohello topic.</span></span> <span data-ttu-id="72c24-129">Mer information om hello-uttryck som kan användas med ett SQL-filter finns hello [SqlFilter.SqlExpression] [ SqlFilter.SqlExpression] syntax.</span><span class="sxs-lookup"><span data-stu-id="72c24-129">For more information about hello expressions that can be used with a SQL filter, see hello [SqlFilter.SqlExpression][SqlFilter.SqlExpression] syntax.</span></span>

<span data-ttu-id="72c24-130">Du kan lägga till filter tooa prenumeration med hjälp av hello **skapa\_regeln** metod för hello **ServiceBusService** objekt.</span><span class="sxs-lookup"><span data-stu-id="72c24-130">You can add filters tooa subscription by using hello **create\_rule** method of hello **ServiceBusService** object.</span></span> <span data-ttu-id="72c24-131">Den här metoden kan du tooadd filter tooan befintliga prenumeration.</span><span class="sxs-lookup"><span data-stu-id="72c24-131">This method allows you tooadd new filters tooan existing subscription.</span></span>

> [!NOTE]
> <span data-ttu-id="72c24-132">Eftersom hello standardfiltret används automatiskt tooall nya prenumerationer, måste du först ta bort standardfiltret hello eller hello **MatchAll** åsidosätter eventuella filter som du kan ange.</span><span class="sxs-lookup"><span data-stu-id="72c24-132">Because hello default filter is applied automatically tooall new subscriptions, you must first remove hello default filter or hello **MatchAll** will override any other filters you may specify.</span></span> <span data-ttu-id="72c24-133">Du kan ta bort hello standardregel med hello `delete_rule` metod för hello **ServiceBusService** objekt.</span><span class="sxs-lookup"><span data-stu-id="72c24-133">You can remove hello default rule by using hello `delete_rule` method of hello **ServiceBusService** object.</span></span>
> 
> 

<span data-ttu-id="72c24-134">hello följande exempel skapas en prenumeration med namnet `HighMessages` med en **SqlFilter** som endast väljer meddelanden som har en anpassad `messagenumber` egenskap som är större än 3:</span><span class="sxs-lookup"><span data-stu-id="72c24-134">hello following example creates a subscription named `HighMessages` with a **SqlFilter** that only selects messages that have a custom `messagenumber` property greater than 3:</span></span>

```python
bus_service.create_subscription('mytopic', 'HighMessages')

rule = Rule()
rule.filter_type = 'SqlFilter'
rule.filter_expression = 'messagenumber > 3'

bus_service.create_rule('mytopic', 'HighMessages', 'HighMessageFilter', rule)
bus_service.delete_rule('mytopic', 'HighMessages', DEFAULT_RULE_NAME)
```

<span data-ttu-id="72c24-135">På liknande sätt hello följande exempel skapas en prenumeration med namnet `LowMessages` med en **SqlFilter** som endast väljer meddelanden som har en `messagenumber` egenskap som är mindre än eller lika med too3:</span><span class="sxs-lookup"><span data-stu-id="72c24-135">Similarly, hello following example creates a subscription named `LowMessages` with a **SqlFilter** that only selects messages that have a `messagenumber` property less than or equal too3:</span></span>

```python
bus_service.create_subscription('mytopic', 'LowMessages')

rule = Rule()
rule.filter_type = 'SqlFilter'
rule.filter_expression = 'messagenumber <= 3'

bus_service.create_rule('mytopic', 'LowMessages', 'LowMessageFilter', rule)
bus_service.delete_rule('mytopic', 'LowMessages', DEFAULT_RULE_NAME)
```

<span data-ttu-id="72c24-136">Nu när ett meddelande skickas för`mytopic` levereras det alltid tooreceivers prenumererar toohello **AllMessages** avsnittet prenumeration och levereras selektivt tooreceivers prenumererar toohello **HighMessages**  och **Highmessages** (beroende på hello meddelandeinnehåll).</span><span class="sxs-lookup"><span data-stu-id="72c24-136">Now, when a message is sent too`mytopic` it is always delivered tooreceivers subscribed toohello **AllMessages** topic subscription, and selectively delivered tooreceivers subscribed toohello **HighMessages** and **LowMessages** topic subscriptions (depending on hello message content).</span></span>

## <a name="send-messages-tooa-topic"></a><span data-ttu-id="72c24-137">Skicka meddelanden tooa avsnittet</span><span class="sxs-lookup"><span data-stu-id="72c24-137">Send messages tooa topic</span></span>
<span data-ttu-id="72c24-138">toosend meddelandet tooa Service Bus-ämne programmet måste använda hello `send_topic_message` metod för hello **ServiceBusService** objekt.</span><span class="sxs-lookup"><span data-stu-id="72c24-138">toosend a message tooa Service Bus topic, your application must use hello `send_topic_message` method of hello **ServiceBusService** object.</span></span>

<span data-ttu-id="72c24-139">hello exemplet nedan visar hur toosend fem test meddelanden för`mytopic`.</span><span class="sxs-lookup"><span data-stu-id="72c24-139">hello following example demonstrates how toosend five test messages too`mytopic`.</span></span> <span data-ttu-id="72c24-140">Observera att hello `messagenumber` -värdet för varje meddelande varierar på hello upprepning av loopen hello (detta avgör vilka prenumerationer som får det):</span><span class="sxs-lookup"><span data-stu-id="72c24-140">Note that hello `messagenumber` property value of each message varies on hello iteration of hello loop (this determines which subscriptions receive it):</span></span>

```python
for i in range(5):
    msg = Message('Msg {0}'.format(i).encode('utf-8'), custom_properties={'messagenumber':i})
    bus_service.send_topic_message('mytopic', msg)
```

<span data-ttu-id="72c24-141">Service Bus-ämnena stöder en maximal meddelandestorlek på 256 KB i hello [standardnivån](service-bus-premium-messaging.md) och 1 MB på hello [premiumnivån](service-bus-premium-messaging.md).</span><span class="sxs-lookup"><span data-stu-id="72c24-141">Service Bus topics support a maximum message size of 256 KB in hello [Standard tier](service-bus-premium-messaging.md) and 1 MB in hello [Premium tier](service-bus-premium-messaging.md).</span></span> <span data-ttu-id="72c24-142">hello-huvudet som innehåller hello standard- och anpassade programegenskaperna, kan ha en maximal storlek på 64 KB.</span><span class="sxs-lookup"><span data-stu-id="72c24-142">hello header, which includes hello standard and custom application properties, can have a maximum size of 64 KB.</span></span> <span data-ttu-id="72c24-143">Det finns ingen gräns hello antalet meddelanden som lagras i ett ämne men det finns ett tak på hello totala storleken på hälsningsmeddelande som innehas av ett ämne.</span><span class="sxs-lookup"><span data-stu-id="72c24-143">There is no limit on hello number of messages held in a topic but there is a cap on hello total size of hello messages held by a topic.</span></span> <span data-ttu-id="72c24-144">Den här ämnesstorleken definieras när ämnet skapas, med en övre gräns på 5 GB.</span><span class="sxs-lookup"><span data-stu-id="72c24-144">This topic size is defined at creation time, with an upper limit of 5 GB.</span></span> <span data-ttu-id="72c24-145">Mer information om kvoter finns [Service Bus-kvoter][Service Bus quotas].</span><span class="sxs-lookup"><span data-stu-id="72c24-145">For more information about quotas, see [Service Bus quotas][Service Bus quotas].</span></span>

## <a name="receive-messages-from-a-subscription"></a><span data-ttu-id="72c24-146">Ta emot meddelanden från en prenumeration</span><span class="sxs-lookup"><span data-stu-id="72c24-146">Receive messages from a subscription</span></span>
<span data-ttu-id="72c24-147">Meddelanden tas emot från en prenumeration med hjälp av hello `receive_subscription_message` metod på hello **ServiceBusService** objekt:</span><span class="sxs-lookup"><span data-stu-id="72c24-147">Messages are received from a subscription using hello `receive_subscription_message` method on hello **ServiceBusService** object:</span></span>

```python
msg = bus_service.receive_subscription_message('mytopic', 'LowMessages', peek_lock=False)
print(msg.body)
```

<span data-ttu-id="72c24-148">Meddelanden tas bort från hello prenumeration som de läses när hello parametern `peek_lock` har angetts för**FALSKT**.</span><span class="sxs-lookup"><span data-stu-id="72c24-148">Messages are deleted from hello subscription as they are read when hello parameter `peek_lock` is set too**False**.</span></span> <span data-ttu-id="72c24-149">Du kan läsa (titt) och låsa hello-meddelande utan att ta bort den från hello kö genom att ange hello parametern `peek_lock` för**SANT**.</span><span class="sxs-lookup"><span data-stu-id="72c24-149">You can read (peek) and lock hello message without deleting it from hello queue by setting hello parameter `peek_lock` too**True**.</span></span>

<span data-ttu-id="72c24-150">Hej beteendet för läsning och ta bort hello-meddelande som en del av hello mottagningsåtgärd är hello enklaste modellen och fungerar bäst för scenarier där ett program kan tolerera icke-bearbetning av ett meddelande i hello händelse av fel.</span><span class="sxs-lookup"><span data-stu-id="72c24-150">hello behavior of reading and deleting hello message as part of hello receive operation is hello simplest model, and works best for scenarios in which an application can tolerate not processing a message in hello event of a failure.</span></span> <span data-ttu-id="72c24-151">toounderstand, Föreställ i vilka hello konsumenten problem hello mottagning av begäran och sedan kraschar innan bearbetningen.</span><span class="sxs-lookup"><span data-stu-id="72c24-151">toounderstand this, consider a scenario in which hello consumer issues hello receive request and then crashes before processing it.</span></span> <span data-ttu-id="72c24-152">Eftersom Service Bus kommer att ha markerat hello meddelandet som Förbrukat, sedan när hello programmet startas om och börjar förbruka meddelanden igen, att ha missat hello-meddelande som har förbrukats tidigare toohello krascher.</span><span class="sxs-lookup"><span data-stu-id="72c24-152">Because Service Bus will have marked hello message as being consumed, then when hello application restarts and begins consuming messages again, it will have missed hello message that was consumed prior toohello crash.</span></span>

<span data-ttu-id="72c24-153">Om hello `peek_lock` parameter har angetts för**SANT**, hello får blir en åtgärd i två steg, vilket gör det möjligt toosupport program som inte tolererar att ett meddelande saknas.</span><span class="sxs-lookup"><span data-stu-id="72c24-153">If hello `peek_lock` parameter is set too**True**, hello receive becomes a two stage operation, which makes it possible toosupport applications that cannot tolerate missing messages.</span></span> <span data-ttu-id="72c24-154">När Service Bus tar emot en begäran, hittar hello nästa meddelande toobe förbrukas, låser det tooprevent andra användare tar emot det och returnerar sedan toohello program.</span><span class="sxs-lookup"><span data-stu-id="72c24-154">When Service Bus receives a request, it finds hello next message toobe consumed, locks it tooprevent other consumers receiving it, and then returns it toohello application.</span></span> <span data-ttu-id="72c24-155">När hello programmet har avslutat bearbetningen hello-meddelande (eller lagrar det på ett tillförlitligt sätt för framtida bearbetning), Slutför hello andra steget i hello får processen genom att anropa `delete` metod på hello **meddelandet** objekt.</span><span class="sxs-lookup"><span data-stu-id="72c24-155">After hello application finishes processing hello message (or stores it reliably for future processing), it completes hello second stage of hello receive process by calling `delete` method on hello **Message** object.</span></span> <span data-ttu-id="72c24-156">Hej `delete` metoden markerar hello meddelandet som Förbrukat och tar bort det från hello prenumeration.</span><span class="sxs-lookup"><span data-stu-id="72c24-156">hello `delete` method marks hello message as being consumed and removes it from hello subscription.</span></span>

```python
msg = bus_service.receive_subscription_message('mytopic', 'LowMessages', peek_lock=True)
print(msg.body)

msg.delete()
```

## <a name="how-toohandle-application-crashes-and-unreadable-messages"></a><span data-ttu-id="72c24-157">Hur toohandle programmet kraschar och oläsbara meddelanden</span><span class="sxs-lookup"><span data-stu-id="72c24-157">How toohandle application crashes and unreadable messages</span></span>
<span data-ttu-id="72c24-158">Service Bus innehåller funktioner toohelp du att återställa fel i programmet eller problem med bearbetning av ett meddelande.</span><span class="sxs-lookup"><span data-stu-id="72c24-158">Service Bus provides functionality toohelp you gracefully recover from errors in your application or difficulties processing a message.</span></span> <span data-ttu-id="72c24-159">Om ett mottagarprogram inte kan tooprocess hello meddelande av någon anledning, så kan det anropa hello `unlock` metod på hello **meddelandet** objekt.</span><span class="sxs-lookup"><span data-stu-id="72c24-159">If a receiver application is unable tooprocess hello message for some reason, then it can call hello `unlock` method on hello **Message** object.</span></span> <span data-ttu-id="72c24-160">Detta orsakar Service Bus toounlock hello-meddelande inom hello prenumeration och göra den tillgänglig toobe tas emot igen, antingen hello av samma förbrukning av ett program eller ett annat användningsprogram.</span><span class="sxs-lookup"><span data-stu-id="72c24-160">This will cause Service Bus toounlock hello message within hello subscription and make it available toobe received again, either by hello same consuming application or by another consuming application.</span></span>

<span data-ttu-id="72c24-161">Det finns också en tidsgräns som är associerad med ett meddelande i hello prenumeration och om hello program inte tooprocess hello-meddelande innan timeout för lås hello går ut (till exempel om hello programmet kraschar), kommer Service Bus låser upp hello-meddelande automatiskt och gör den tillgänglig toobe tas emot igen.</span><span class="sxs-lookup"><span data-stu-id="72c24-161">There is also a timeout associated with a message locked within hello subscription, and if hello application fails tooprocess hello message before hello lock timeout expires (for example, if hello application crashes), then Service Bus unlocks hello message automatically and makes it available toobe received again.</span></span>

<span data-ttu-id="72c24-162">Hello händelse som hello programmet kraschar efter hello meddelandet har bearbetats men innan hello `delete` metoden anropas sedan hello-meddelande kommer att levereras på nytt toohello program när den startas om.</span><span class="sxs-lookup"><span data-stu-id="72c24-162">In hello event that hello application crashes after processing hello message but before hello `delete` method is called, then hello message will be redelivered toohello application when it restarts.</span></span> <span data-ttu-id="72c24-163">Det här kallas ofta *At Least Once Processing*, det vill säga, varje meddelande bearbetas minst en gång men i vissa situationer hello samma meddelande levereras.</span><span class="sxs-lookup"><span data-stu-id="72c24-163">This is often called *At Least Once Processing*, that is, each message will be processed at least once but in certain situations hello same message may be redelivered.</span></span> <span data-ttu-id="72c24-164">Om hello scenariot inte tolererar duplicerad bearbetning, sedan bör programutvecklarna lägga till ytterligare logik tootheir duplicerad meddelandeleverans för programmet toohandle.</span><span class="sxs-lookup"><span data-stu-id="72c24-164">If hello scenario cannot tolerate duplicate processing, then application developers should add additional logic tootheir application toohandle duplicate message delivery.</span></span> <span data-ttu-id="72c24-165">Detta uppnås ofta med hjälp av hello **MessageId** -egenskapen för hello-meddelande, förblir konstant under alla leveransförsök.</span><span class="sxs-lookup"><span data-stu-id="72c24-165">This is often achieved using hello **MessageId** property of hello message, which will remain constant across delivery attempts.</span></span>

## <a name="delete-topics-and-subscriptions"></a><span data-ttu-id="72c24-166">Ta bort ämnen och prenumerationer</span><span class="sxs-lookup"><span data-stu-id="72c24-166">Delete topics and subscriptions</span></span>
<span data-ttu-id="72c24-167">Ämnen och prenumerationer är beständiga och måste vara explicit bort antingen via hello [Azure-portalen] [ Azure portal] eller programmässigt.</span><span class="sxs-lookup"><span data-stu-id="72c24-167">Topics and subscriptions are persistent, and must be explicitly deleted either through hello [Azure portal][Azure portal] or programmatically.</span></span> <span data-ttu-id="72c24-168">hello följande exempel visas hur toodelete hello avsnittet med namnet `mytopic`:</span><span class="sxs-lookup"><span data-stu-id="72c24-168">hello following example shows how toodelete hello topic named `mytopic`:</span></span>

```python
bus_service.delete_topic('mytopic')
```

<span data-ttu-id="72c24-169">Om du tar bort ett ämne tas även bort alla prenumerationer som är registrerade med hello-avsnittet.</span><span class="sxs-lookup"><span data-stu-id="72c24-169">Deleting a topic also deletes any subscriptions that are registered with hello topic.</span></span> <span data-ttu-id="72c24-170">Prenumerationer kan även tas bort separat.</span><span class="sxs-lookup"><span data-stu-id="72c24-170">Subscriptions can also be deleted independently.</span></span> <span data-ttu-id="72c24-171">hello följande kod visar hur toodelete en prenumeration med namnet `HighMessages` från hello `mytopic` avsnittet:</span><span class="sxs-lookup"><span data-stu-id="72c24-171">hello following code shows how toodelete a subscription named `HighMessages` from hello `mytopic` topic:</span></span>

```python
bus_service.delete_subscription('mytopic', 'HighMessages')
```

## <a name="next-steps"></a><span data-ttu-id="72c24-172">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="72c24-172">Next steps</span></span>
<span data-ttu-id="72c24-173">Nu när du har lärt dig grunderna hello i Service Bus-ämnen, följa dessa länkar toolearn mer.</span><span class="sxs-lookup"><span data-stu-id="72c24-173">Now that you've learned hello basics of Service Bus topics, follow these links toolearn more.</span></span>

* <span data-ttu-id="72c24-174">Se [köer, ämnen och prenumerationer][Queues, topics, and subscriptions].</span><span class="sxs-lookup"><span data-stu-id="72c24-174">See [Queues, topics, and subscriptions][Queues, topics, and subscriptions].</span></span>
* <span data-ttu-id="72c24-175">Referens för [SqlFilter.SqlExpression][SqlFilter.SqlExpression].</span><span class="sxs-lookup"><span data-stu-id="72c24-175">Reference for [SqlFilter.SqlExpression][SqlFilter.SqlExpression].</span></span>

[Azure portal]: https://portal.azure.com
[Azure Python package]: https://pypi.python.org/pypi/azure  
[Queues, topics, and subscriptions]: service-bus-queues-topics-subscriptions.md
[SqlFilter.SqlExpression]: service-bus-messaging-sql-filter.md
[Service Bus quotas]: service-bus-quotas.md 
