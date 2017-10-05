---
title: Automatisk vidarebefordring Azure Service Bus meddelandeentiteter | Microsoft Docs
description: "Hur du kopplar en Service Bus-kö eller en prenumeration på en annan kö eller ett ämne."
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
ms.openlocfilehash: 2656b3a276c542ca836b3949e4e493d7c7f48f16
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/18/2017
---
# <a name="chaining-service-bus-entities-with-auto-forwarding"></a><span data-ttu-id="2782f-103">Länkning Service Bus-entiteter med automatisk vidarebefordring</span><span class="sxs-lookup"><span data-stu-id="2782f-103">Chaining Service Bus entities with auto-forwarding</span></span>

<span data-ttu-id="2782f-104">Service Bus *automatisk vidarebefordring* funktionen kan du kopplar en kö eller en prenumeration på en annan kö eller ett ämne som ingår i samma namnområde.</span><span class="sxs-lookup"><span data-stu-id="2782f-104">The Service Bus *auto-forwarding* feature enables you to chain a queue or subscription to another queue or topic that is part of the same namespace.</span></span> <span data-ttu-id="2782f-105">När automatisk vidarebefordring är aktiverat, tar bort meddelanden som placerats i den första kön eller prenumeration (källa) automatiskt i Service Bus och placerar dem i den andra kö eller ett ämne (mål).</span><span class="sxs-lookup"><span data-stu-id="2782f-105">When auto-forwarding is enabled, Service Bus automatically removes messages that are placed in the first queue or subscription (source) and puts them in the second queue or topic (destination).</span></span> <span data-ttu-id="2782f-106">Observera att det är fortfarande möjligt att skicka ett meddelande direkt till enheten som mål.</span><span class="sxs-lookup"><span data-stu-id="2782f-106">Note that it is still possible to send a message to the destination entity directly.</span></span> <span data-ttu-id="2782f-107">Du är inte möjligt att en underkö, till exempel en obeställbara meddelanden till en annan kö eller ett ämne.</span><span class="sxs-lookup"><span data-stu-id="2782f-107">Also, it is not possible to chain a subqueue, such as a deadletter queue, to another queue or topic.</span></span>

## <a name="using-auto-forwarding"></a><span data-ttu-id="2782f-108">Med hjälp av automatisk vidarebefordring</span><span class="sxs-lookup"><span data-stu-id="2782f-108">Using auto-forwarding</span></span>
<span data-ttu-id="2782f-109">Du kan aktivera automatisk vidarebefordring genom att ange den [QueueDescription.ForwardTo] [ QueueDescription.ForwardTo] eller [SubscriptionDescription.ForwardTo] [ SubscriptionDescription.ForwardTo] Egenskaper för den [QueueDescription] [ QueueDescription] eller [SubscriptionDescription] [ SubscriptionDescription] objekt för datakällan, som i den följande exempel.</span><span class="sxs-lookup"><span data-stu-id="2782f-109">You can enable auto-forwarding by setting the [QueueDescription.ForwardTo][QueueDescription.ForwardTo] or [SubscriptionDescription.ForwardTo][SubscriptionDescription.ForwardTo] properties on the [QueueDescription][QueueDescription] or [SubscriptionDescription][SubscriptionDescription] objects for the source, as in the following example.</span></span>

```csharp
SubscriptionDescription srcSubscription = new SubscriptionDescription (srcTopic, srcSubscriptionName);
srcSubscription.ForwardTo = destTopic;
namespaceManager.CreateSubscription(srcSubscription));
```

<span data-ttu-id="2782f-110">Entiteten målet måste finnas samtidigt källentiteten har skapats.</span><span class="sxs-lookup"><span data-stu-id="2782f-110">The destination entity must exist at the time the source entity is created.</span></span> <span data-ttu-id="2782f-111">Om mål-entiteten inte finns, returnerar ett undantag när du uppmanas att skapa källentiteten Service Bus.</span><span class="sxs-lookup"><span data-stu-id="2782f-111">If the destination entity does not exist, Service Bus returns an exception when asked to create the source entity.</span></span>

<span data-ttu-id="2782f-112">Du kan använda automatisk vidarebefordring för att skala ut enskilda avsnitt.</span><span class="sxs-lookup"><span data-stu-id="2782f-112">You can use auto-forwarding to scale out an individual topic.</span></span> <span data-ttu-id="2782f-113">Service Bus-gränser i [antalet prenumerationer på ett visst ämne](service-bus-quotas.md) till 2 000.</span><span class="sxs-lookup"><span data-stu-id="2782f-113">Service Bus limits the [number of subscriptions on a given topic](service-bus-quotas.md) to 2,000.</span></span> <span data-ttu-id="2782f-114">Du kan hantera ytterligare prenumerationer genom att skapa andra nivån avsnitt.</span><span class="sxs-lookup"><span data-stu-id="2782f-114">You can accommodate additional subscriptions by creating second-level topics.</span></span> <span data-ttu-id="2782f-115">Observera att även om du inte är bunden av Service Bus-begränsning för antalet prenumerationer, lägga till en andra nivå av ämnen kan förbättra det totala genomflödet i ditt ämne.</span><span class="sxs-lookup"><span data-stu-id="2782f-115">Note that even if you are not bound by the Service Bus limitation on the number of subscriptions, adding a second level of topics can improve the overall throughput of your topic.</span></span>

![Automatisk vidarebefordring scenario][0]

<span data-ttu-id="2782f-117">Du kan också använda automatisk vidarebefordring för att frikoppla avsändare från mottagare.</span><span class="sxs-lookup"><span data-stu-id="2782f-117">You can also use auto-forwarding to decouple message senders from receivers.</span></span> <span data-ttu-id="2782f-118">Tänk dig ett ERP-system som består av tre moduler: ordna bearbetning, lager och kunden relationer hantering.</span><span class="sxs-lookup"><span data-stu-id="2782f-118">For example, consider an ERP system that consists of three modules: order processing, inventory management, and customer relations management.</span></span> <span data-ttu-id="2782f-119">Var och en av dessa moduler genererar meddelanden i kö till en motsvarande ämne.</span><span class="sxs-lookup"><span data-stu-id="2782f-119">Each of these modules generates messages that are enqueued into a corresponding topic.</span></span> <span data-ttu-id="2782f-120">Alice och Bob är säljare som är intresserade av alla meddelanden som relaterar till sina kunder.</span><span class="sxs-lookup"><span data-stu-id="2782f-120">Alice and Bob are sales representatives that are interested in all messages that relate to their customers.</span></span> <span data-ttu-id="2782f-121">För att ta emot dessa meddelanden skapar Alice och Bob du en personlig kö och en prenumeration på varje ERP-avsnitt som automatiskt vidarebefordrar alla meddelanden till deras köomfång.</span><span class="sxs-lookup"><span data-stu-id="2782f-121">To receive those messages, Alice and Bob each create a personal queue and a subscription on each of the ERP topics that automatically forward all messages to their queue.</span></span>

![Automatisk vidarebefordring scenario][1]

<span data-ttu-id="2782f-123">Om Alice går på semester, sitt personliga kön i stället ERP-avsnittet, fylls.</span><span class="sxs-lookup"><span data-stu-id="2782f-123">If Alice goes on vacation, her personal queue, rather than the ERP topic, fills up.</span></span> <span data-ttu-id="2782f-124">I detta scenario eftersom en säljare inte har fått några meddelanden nå ingen ERP-avsnitt någonsin kvoten.</span><span class="sxs-lookup"><span data-stu-id="2782f-124">In this scenario, because a sales representative has not received any messages, none of the ERP topics ever reach quota.</span></span>

## <a name="auto-forwarding-considerations"></a><span data-ttu-id="2782f-125">Automatisk vidarebefordring överväganden</span><span class="sxs-lookup"><span data-stu-id="2782f-125">Auto-forwarding considerations</span></span>

<span data-ttu-id="2782f-126">Om mål-entiteten ackumulerar för många meddelanden och överskrider kvoten eller mål-entiteten är inaktiverad, källentiteten lägger till meddelandena till dess [kö med olevererade brev](service-bus-dead-letter-queues.md) tills det finns utrymme i målet (eller entiteten aktiveras på nytt).</span><span class="sxs-lookup"><span data-stu-id="2782f-126">If the destination entity accumulates too many messages and exceeds the quota, or the destination entity is disabled, the source entity adds the messages to its [dead-letter queue](service-bus-dead-letter-queues.md) until there is space in the destination (or the entity is re-enabled).</span></span> <span data-ttu-id="2782f-127">Dessa meddelanden fortsätter ska behållas i kö med olevererade brev, så du måste uttryckligen får och behandlar dem från kö för obeställbara meddelanden.</span><span class="sxs-lookup"><span data-stu-id="2782f-127">Those messages will continue to live in the dead-letter queue, so you must explicitly receive and process them from the dead-letter queue.</span></span>

<span data-ttu-id="2782f-128">När länkning ihop enskilda avsnitt för att erhålla ett sammansatt ämne med många prenumerationer, rekommenderas att du har ett Måttligt antal prenumerationer på första nivån avsnittet och många prenumerationer på andra nivån-avsnitt.</span><span class="sxs-lookup"><span data-stu-id="2782f-128">When chaining together individual topics to obtain a composite topic with many subscriptions, it is recommended that you have a moderate number of subscriptions on the first-level topic and many subscriptions on the second-level topics.</span></span> <span data-ttu-id="2782f-129">Till exempel kan en första nivån avsnittet med 20 prenumerationer, var och en av dem att härleda till ett andra nivån avsnitt med 200 prenumerationer ger bättre genomströmning än en första nivån avsnittet med 200 prenumerationer varje sammankedjade till en andra nivå avsnittet med 20 prenumerationer.</span><span class="sxs-lookup"><span data-stu-id="2782f-129">For example, a first-level topic with 20 subscriptions, each of them chained to a second-level topic with 200 subscriptions, allows for higher throughput than a first-level topic with 200 subscriptions, each chained to a second-level topic with 20 subscriptions.</span></span>

<span data-ttu-id="2782f-130">Service Bus debiterar en åtgärd för varje vidarebefordrade meddelandet.</span><span class="sxs-lookup"><span data-stu-id="2782f-130">Service Bus bills one operation for each forwarded message.</span></span> <span data-ttu-id="2782f-131">Till exempel faktureras ett meddelande skickas till ett ämne med 20 prenumerationer, var och en av dem har konfigurerats för att automatiskt vidarebefordra meddelanden till en annan kö eller ett ämne, som 21 funktioner om alla prenumerationer på första nivån får en kopia av meddelandet.</span><span class="sxs-lookup"><span data-stu-id="2782f-131">For example, sending a message to a topic with 20 subscriptions, each of them configured to auto-forward messages to another queue or topic, is billed as 21 operations if all first-level subscriptions receive a copy of the message.</span></span>

<span data-ttu-id="2782f-132">Om du vill skapa en prenumeration som går att härleda till en annan kö eller ett ämne, skapare av prenumerationen måste ha **hantera** behörigheter på både käll- och mål-entiteten.</span><span class="sxs-lookup"><span data-stu-id="2782f-132">To create a subscription that is chained to another queue or topic, the creator of the subscription must have **Manage** permissions on both the source and the destination entity.</span></span> <span data-ttu-id="2782f-133">Skicka meddelanden till avsnittet källa kräver endast **skicka** behörigheter på käll-avsnittet.</span><span class="sxs-lookup"><span data-stu-id="2782f-133">Sending messages to the source topic only requires **Send** permissions on the source topic.</span></span>

## <a name="next-steps"></a><span data-ttu-id="2782f-134">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="2782f-134">Next steps</span></span>

<span data-ttu-id="2782f-135">Detaljerad information om automatisk vidarebefordring finns i följande referensavsnitt:</span><span class="sxs-lookup"><span data-stu-id="2782f-135">For detailed information about auto-forwarding, see the following reference topics:</span></span>

* <span data-ttu-id="2782f-136">[ForwardTo][QueueDescription.ForwardTo]</span><span class="sxs-lookup"><span data-stu-id="2782f-136">[ForwardTo][QueueDescription.ForwardTo]</span></span>
* <span data-ttu-id="2782f-137">[QueueDescription][QueueDescription]</span><span class="sxs-lookup"><span data-stu-id="2782f-137">[QueueDescription][QueueDescription]</span></span>
* <span data-ttu-id="2782f-138">[SubscriptionDescription][SubscriptionDescription]</span><span class="sxs-lookup"><span data-stu-id="2782f-138">[SubscriptionDescription][SubscriptionDescription]</span></span>

<span data-ttu-id="2782f-139">Läs mer om Service Bus prestandaförbättringar i</span><span class="sxs-lookup"><span data-stu-id="2782f-139">To learn more about Service Bus performance improvements, see</span></span> 

* [<span data-ttu-id="2782f-140">Metodtips för bättre prestanda med hjälp av Service Bus-meddelanden</span><span class="sxs-lookup"><span data-stu-id="2782f-140">Best Practices for performance improvements using Service Bus Messaging</span></span>](service-bus-performance-improvements.md)
* <span data-ttu-id="2782f-141">[Partitionerade meddelandeentiteter][Partitioned messaging entities].</span><span class="sxs-lookup"><span data-stu-id="2782f-141">[Partitioned messaging entities][Partitioned messaging entities].</span></span>

[QueueDescription.ForwardTo]: /dotnet/api/microsoft.servicebus.messaging.queuedescription.forwardto#Microsoft_ServiceBus_Messaging_QueueDescription_ForwardTo
[SubscriptionDescription.ForwardTo]: /dotnet/api/microsoft.servicebus.messaging.subscriptiondescription.forwardto#Microsoft_ServiceBus_Messaging_SubscriptionDescription_ForwardTo
[QueueDescription]: /dotnet/api/microsoft.servicebus.messaging.queuedescription
[SubscriptionDescription]: /dotnet/api/microsoft.servicebus.messaging.queuedescription
[0]: ./media/service-bus-auto-forwarding/IC628631.gif
[1]: ./media/service-bus-auto-forwarding/IC628632.gif
[Partitioned messaging entities]: service-bus-partitioning.md
