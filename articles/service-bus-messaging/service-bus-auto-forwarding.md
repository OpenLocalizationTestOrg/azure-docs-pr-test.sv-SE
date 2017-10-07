---
title: aaaAuto vidarebefordran Azure Service Bus-meddelandeentiteter | Microsoft Docs
description: "Hur toochain en Service Bus kön eller prenumeration tooanother kö eller ett ämne."
services: service-bus-messaging
documentationcenter: na
author: sethmanheim
manager: timlt
editor: 
ms.assetid: f7060778-3421-402c-97c7-735dbf6a61e8
ms.service: service-bus-messaging
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/07/2017
ms.author: sethm
ms.openlocfilehash: af18273e4e2f81c5363eb830c7decf313afd8307
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="chaining-service-bus-entities-with-auto-forwarding"></a><span data-ttu-id="a7b41-103">Länkning Service Bus-entiteter med automatisk vidarebefordring</span><span class="sxs-lookup"><span data-stu-id="a7b41-103">Chaining Service Bus entities with auto-forwarding</span></span>

<span data-ttu-id="a7b41-104">hello Service Bus *automatisk vidarebefordring* funktionen kan du toochain en kö eller prenumeration tooanother kö eller ett ämne som är en del av hello samma namnområde.</span><span class="sxs-lookup"><span data-stu-id="a7b41-104">hello Service Bus *auto-forwarding* feature enables you toochain a queue or subscription tooanother queue or topic that is part of hello same namespace.</span></span> <span data-ttu-id="a7b41-105">När automatisk vidarebefordring är aktiverat, tar bort meddelanden som placerats i hello första kön eller prenumeration (källa) automatiskt i Service Bus och placerar dem i hello andra kö eller ett ämne (mål).</span><span class="sxs-lookup"><span data-stu-id="a7b41-105">When auto-forwarding is enabled, Service Bus automatically removes messages that are placed in hello first queue or subscription (source) and puts them in hello second queue or topic (destination).</span></span> <span data-ttu-id="a7b41-106">Observera att det är fortfarande möjligt toosend målentitet meddelandet toohello direkt.</span><span class="sxs-lookup"><span data-stu-id="a7b41-106">Note that it is still possible toosend a message toohello destination entity directly.</span></span> <span data-ttu-id="a7b41-107">Dessutom är det inte möjligt toochain en underkö, till exempel en obeställbara meddelanden, tooanother kö eller ett ämne.</span><span class="sxs-lookup"><span data-stu-id="a7b41-107">Also, it is not possible toochain a subqueue, such as a deadletter queue, tooanother queue or topic.</span></span>

## <a name="using-auto-forwarding"></a><span data-ttu-id="a7b41-108">Med hjälp av automatisk vidarebefordring</span><span class="sxs-lookup"><span data-stu-id="a7b41-108">Using auto-forwarding</span></span>
<span data-ttu-id="a7b41-109">Du kan aktivera automatisk vidarebefordring genom att ange hello [QueueDescription.ForwardTo] [ QueueDescription.ForwardTo] eller [SubscriptionDescription.ForwardTo] [ SubscriptionDescription.ForwardTo] Egenskaper för hello [QueueDescription] [ QueueDescription] eller [SubscriptionDescription] [ SubscriptionDescription] objekt för hello källan som hello följande exempel.</span><span class="sxs-lookup"><span data-stu-id="a7b41-109">You can enable auto-forwarding by setting hello [QueueDescription.ForwardTo][QueueDescription.ForwardTo] or [SubscriptionDescription.ForwardTo][SubscriptionDescription.ForwardTo] properties on hello [QueueDescription][QueueDescription] or [SubscriptionDescription][SubscriptionDescription] objects for hello source, as in hello following example.</span></span>

```csharp
SubscriptionDescription srcSubscription = new SubscriptionDescription (srcTopic, srcSubscriptionName);
srcSubscription.ForwardTo = destTopic;
namespaceManager.CreateSubscription(srcSubscription));
```

<span data-ttu-id="a7b41-110">hello målentitet måste finnas när hello hello källentiteten skapas.</span><span class="sxs-lookup"><span data-stu-id="a7b41-110">hello destination entity must exist at hello time hello source entity is created.</span></span> <span data-ttu-id="a7b41-111">Om hello målentitet inte finns, returnerar Service Bus ett undantag när och toocreate hello källentitet.</span><span class="sxs-lookup"><span data-stu-id="a7b41-111">If hello destination entity does not exist, Service Bus returns an exception when asked toocreate hello source entity.</span></span>

<span data-ttu-id="a7b41-112">Du kan använda automatisk vidarebefordring tooscale ut enskilda avsnitt.</span><span class="sxs-lookup"><span data-stu-id="a7b41-112">You can use auto-forwarding tooscale out an individual topic.</span></span> <span data-ttu-id="a7b41-113">Service Bus gränser hello [antalet prenumerationer på ett visst ämne](service-bus-quotas.md) too2 000.</span><span class="sxs-lookup"><span data-stu-id="a7b41-113">Service Bus limits hello [number of subscriptions on a given topic](service-bus-quotas.md) too2,000.</span></span> <span data-ttu-id="a7b41-114">Du kan hantera ytterligare prenumerationer genom att skapa andra nivån avsnitt.</span><span class="sxs-lookup"><span data-stu-id="a7b41-114">You can accommodate additional subscriptions by creating second-level topics.</span></span> <span data-ttu-id="a7b41-115">Observera att även om du inte är bunden av hello Service Bus begränsning för hello antalet prenumerationer, lägga till en andra nivå av ämnen kan förbättra hello totala genomflödet i ditt ämne.</span><span class="sxs-lookup"><span data-stu-id="a7b41-115">Note that even if you are not bound by hello Service Bus limitation on hello number of subscriptions, adding a second level of topics can improve hello overall throughput of your topic.</span></span>

![Automatisk vidarebefordring scenario][0]

<span data-ttu-id="a7b41-117">Du kan också använda automatisk vidarebefordring toodecouple avsändare från mottagare.</span><span class="sxs-lookup"><span data-stu-id="a7b41-117">You can also use auto-forwarding toodecouple message senders from receivers.</span></span> <span data-ttu-id="a7b41-118">Tänk dig ett ERP-system som består av tre moduler: ordna bearbetning, lager och kunden relationer hantering.</span><span class="sxs-lookup"><span data-stu-id="a7b41-118">For example, consider an ERP system that consists of three modules: order processing, inventory management, and customer relations management.</span></span> <span data-ttu-id="a7b41-119">Var och en av dessa moduler genererar meddelanden i kö till en motsvarande ämne.</span><span class="sxs-lookup"><span data-stu-id="a7b41-119">Each of these modules generates messages that are enqueued into a corresponding topic.</span></span> <span data-ttu-id="a7b41-120">Alice och Bob är säljare som är intresserade av alla meddelanden som rör tootheir kunder.</span><span class="sxs-lookup"><span data-stu-id="a7b41-120">Alice and Bob are sales representatives that are interested in all messages that relate tootheir customers.</span></span> <span data-ttu-id="a7b41-121">tooreceive dessa meddelanden, Alice och Bob skapa en personlig kö och en prenumeration på varje hello ERP avsnitt som automatiskt vidarebefordrar alla meddelanden tootheir kön.</span><span class="sxs-lookup"><span data-stu-id="a7b41-121">tooreceive those messages, Alice and Bob each create a personal queue and a subscription on each of hello ERP topics that automatically forward all messages tootheir queue.</span></span>

![Automatisk vidarebefordring scenario][1]

<span data-ttu-id="a7b41-123">Om Alice går på semester, sitt personliga kön i stället hello ERP-avsnittet, fylls.</span><span class="sxs-lookup"><span data-stu-id="a7b41-123">If Alice goes on vacation, her personal queue, rather than hello ERP topic, fills up.</span></span> <span data-ttu-id="a7b41-124">I detta scenario eftersom en säljare inte har fått några meddelanden nå ingen hälsningspaket ERP avsnitt någonsin kvoten.</span><span class="sxs-lookup"><span data-stu-id="a7b41-124">In this scenario, because a sales representative has not received any messages, none of hello ERP topics ever reach quota.</span></span>

## <a name="auto-forwarding-considerations"></a><span data-ttu-id="a7b41-125">Automatisk vidarebefordring överväganden</span><span class="sxs-lookup"><span data-stu-id="a7b41-125">Auto-forwarding considerations</span></span>

<span data-ttu-id="a7b41-126">Om hello målentitet ackumulerar för många meddelanden och överskrider kvoten hello eller hello målentitet är inaktiverad, hello källentiteten läggs hello meddelanden tooits [kö med olevererade brev](service-bus-dead-letter-queues.md) tills det finns utrymme i hello mål (eller hello entiteten har återaktiverats).</span><span class="sxs-lookup"><span data-stu-id="a7b41-126">If hello destination entity accumulates too many messages and exceeds hello quota, or hello destination entity is disabled, hello source entity adds hello messages tooits [dead-letter queue](service-bus-dead-letter-queues.md) until there is space in hello destination (or hello entity is re-enabled).</span></span> <span data-ttu-id="a7b41-127">Dessa meddelanden fortsätter toolive i hello obeställbara meddelanden, så du måste uttryckligen får och behandlar dem från hello obeställbara meddelanden.</span><span class="sxs-lookup"><span data-stu-id="a7b41-127">Those messages will continue toolive in hello dead-letter queue, so you must explicitly receive and process them from hello dead-letter queue.</span></span>

<span data-ttu-id="a7b41-128">När länkning ihop enskilda avsnitt tooobtain ett sammansatt ämne med många prenumerationer, rekommenderas att du har ett Måttligt antal prenumerationer på hello första nivån avsnittet och många prenumerationer på hello andra nivån avsnitt.</span><span class="sxs-lookup"><span data-stu-id="a7b41-128">When chaining together individual topics tooobtain a composite topic with many subscriptions, it is recommended that you have a moderate number of subscriptions on hello first-level topic and many subscriptions on hello second-level topics.</span></span> <span data-ttu-id="a7b41-129">Till exempel ett första nivån ämne med 20 prenumerationer, var och en av dem att härleda tooa andra nivån avsnittet med 200 prenumerationer ger högre genomströmning än en första nivån avsnittet med 200 prenumerationer varje sammankedjade tooa andra nivån avsnittet med 20 prenumerationer .</span><span class="sxs-lookup"><span data-stu-id="a7b41-129">For example, a first-level topic with 20 subscriptions, each of them chained tooa second-level topic with 200 subscriptions, allows for higher throughput than a first-level topic with 200 subscriptions, each chained tooa second-level topic with 20 subscriptions.</span></span>

<span data-ttu-id="a7b41-130">Service Bus debiterar en åtgärd för varje vidarebefordrade meddelandet.</span><span class="sxs-lookup"><span data-stu-id="a7b41-130">Service Bus bills one operation for each forwarded message.</span></span> <span data-ttu-id="a7b41-131">Till exempel konfigurerad skicka ett meddelande tooa ämne med 20 prenumerationer, var och en av dem tooauto vidarebefordra meddelanden tooanother kö eller ett ämne, faktureras som 21 funktioner om alla prenumerationer på första nivån får en kopia av hello-meddelande.</span><span class="sxs-lookup"><span data-stu-id="a7b41-131">For example, sending a message tooa topic with 20 subscriptions, each of them configured tooauto-forward messages tooanother queue or topic, is billed as 21 operations if all first-level subscriptions receive a copy of hello message.</span></span>

<span data-ttu-id="a7b41-132">toocreate en prenumeration som är länkad tooanother kö eller ett ämne hello skapare av hello prenumerationen måste ha **hantera** behörigheter för både hello käll- och hello målentitet.</span><span class="sxs-lookup"><span data-stu-id="a7b41-132">toocreate a subscription that is chained tooanother queue or topic, hello creator of hello subscription must have **Manage** permissions on both hello source and hello destination entity.</span></span> <span data-ttu-id="a7b41-133">Skicka meddelanden toohello källa avsnittet kräver endast **skicka** behörigheter på hello käll-avsnittet.</span><span class="sxs-lookup"><span data-stu-id="a7b41-133">Sending messages toohello source topic only requires **Send** permissions on hello source topic.</span></span>

## <a name="next-steps"></a><span data-ttu-id="a7b41-134">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="a7b41-134">Next steps</span></span>

<span data-ttu-id="a7b41-135">Detaljerad information om automatisk vidarebefordring finns hello följande referensinformation om:</span><span class="sxs-lookup"><span data-stu-id="a7b41-135">For detailed information about auto-forwarding, see hello following reference topics:</span></span>

* <span data-ttu-id="a7b41-136">[ForwardTo][QueueDescription.ForwardTo]</span><span class="sxs-lookup"><span data-stu-id="a7b41-136">[ForwardTo][QueueDescription.ForwardTo]</span></span>
* <span data-ttu-id="a7b41-137">[QueueDescription][QueueDescription]</span><span class="sxs-lookup"><span data-stu-id="a7b41-137">[QueueDescription][QueueDescription]</span></span>
* <span data-ttu-id="a7b41-138">[SubscriptionDescription][SubscriptionDescription]</span><span class="sxs-lookup"><span data-stu-id="a7b41-138">[SubscriptionDescription][SubscriptionDescription]</span></span>

<span data-ttu-id="a7b41-139">toolearn mer om Service Bus prestandaförbättringar, se</span><span class="sxs-lookup"><span data-stu-id="a7b41-139">toolearn more about Service Bus performance improvements, see</span></span> 

* [<span data-ttu-id="a7b41-140">Metodtips för bättre prestanda med hjälp av Service Bus-meddelanden</span><span class="sxs-lookup"><span data-stu-id="a7b41-140">Best Practices for performance improvements using Service Bus Messaging</span></span>](service-bus-performance-improvements.md)
* <span data-ttu-id="a7b41-141">[Partitionerade meddelandeentiteter][Partitioned messaging entities].</span><span class="sxs-lookup"><span data-stu-id="a7b41-141">[Partitioned messaging entities][Partitioned messaging entities].</span></span>

[QueueDescription.ForwardTo]: /dotnet/api/microsoft.servicebus.messaging.queuedescription.forwardto#Microsoft_ServiceBus_Messaging_QueueDescription_ForwardTo
[SubscriptionDescription.ForwardTo]: /dotnet/api/microsoft.servicebus.messaging.subscriptiondescription.forwardto#Microsoft_ServiceBus_Messaging_SubscriptionDescription_ForwardTo
[QueueDescription]: /dotnet/api/microsoft.servicebus.messaging.queuedescription
[SubscriptionDescription]: /dotnet/api/microsoft.servicebus.messaging.queuedescription
[0]: ./media/service-bus-auto-forwarding/IC628631.gif
[1]: ./media/service-bus-auto-forwarding/IC628632.gif
[Partitioned messaging entities]: service-bus-partitioning.md
