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
# <a name="chaining-service-bus-entities-with-auto-forwarding"></a>Länkning Service Bus-entiteter med automatisk vidarebefordring

hello Service Bus *automatisk vidarebefordring* funktionen kan du toochain en kö eller prenumeration tooanother kö eller ett ämne som är en del av hello samma namnområde. När automatisk vidarebefordring är aktiverat, tar bort meddelanden som placerats i hello första kön eller prenumeration (källa) automatiskt i Service Bus och placerar dem i hello andra kö eller ett ämne (mål). Observera att det är fortfarande möjligt toosend målentitet meddelandet toohello direkt. Dessutom är det inte möjligt toochain en underkö, till exempel en obeställbara meddelanden, tooanother kö eller ett ämne.

## <a name="using-auto-forwarding"></a>Med hjälp av automatisk vidarebefordring
Du kan aktivera automatisk vidarebefordring genom att ange hello [QueueDescription.ForwardTo] [ QueueDescription.ForwardTo] eller [SubscriptionDescription.ForwardTo] [ SubscriptionDescription.ForwardTo] Egenskaper för hello [QueueDescription] [ QueueDescription] eller [SubscriptionDescription] [ SubscriptionDescription] objekt för hello källan som hello följande exempel.

```csharp
SubscriptionDescription srcSubscription = new SubscriptionDescription (srcTopic, srcSubscriptionName);
srcSubscription.ForwardTo = destTopic;
namespaceManager.CreateSubscription(srcSubscription));
```

hello målentitet måste finnas när hello hello källentiteten skapas. Om hello målentitet inte finns, returnerar Service Bus ett undantag när och toocreate hello källentitet.

Du kan använda automatisk vidarebefordring tooscale ut enskilda avsnitt. Service Bus gränser hello [antalet prenumerationer på ett visst ämne](service-bus-quotas.md) too2 000. Du kan hantera ytterligare prenumerationer genom att skapa andra nivån avsnitt. Observera att även om du inte är bunden av hello Service Bus begränsning för hello antalet prenumerationer, lägga till en andra nivå av ämnen kan förbättra hello totala genomflödet i ditt ämne.

![Automatisk vidarebefordring scenario][0]

Du kan också använda automatisk vidarebefordring toodecouple avsändare från mottagare. Tänk dig ett ERP-system som består av tre moduler: ordna bearbetning, lager och kunden relationer hantering. Var och en av dessa moduler genererar meddelanden i kö till en motsvarande ämne. Alice och Bob är säljare som är intresserade av alla meddelanden som rör tootheir kunder. tooreceive dessa meddelanden, Alice och Bob skapa en personlig kö och en prenumeration på varje hello ERP avsnitt som automatiskt vidarebefordrar alla meddelanden tootheir kön.

![Automatisk vidarebefordring scenario][1]

Om Alice går på semester, sitt personliga kön i stället hello ERP-avsnittet, fylls. I detta scenario eftersom en säljare inte har fått några meddelanden nå ingen hälsningspaket ERP avsnitt någonsin kvoten.

## <a name="auto-forwarding-considerations"></a>Automatisk vidarebefordring överväganden

Om hello målentitet ackumulerar för många meddelanden och överskrider kvoten hello eller hello målentitet är inaktiverad, hello källentiteten läggs hello meddelanden tooits [kö med olevererade brev](service-bus-dead-letter-queues.md) tills det finns utrymme i hello mål (eller hello entiteten har återaktiverats). Dessa meddelanden fortsätter toolive i hello obeställbara meddelanden, så du måste uttryckligen får och behandlar dem från hello obeställbara meddelanden.

När länkning ihop enskilda avsnitt tooobtain ett sammansatt ämne med många prenumerationer, rekommenderas att du har ett Måttligt antal prenumerationer på hello första nivån avsnittet och många prenumerationer på hello andra nivån avsnitt. Till exempel ett första nivån ämne med 20 prenumerationer, var och en av dem att härleda tooa andra nivån avsnittet med 200 prenumerationer ger högre genomströmning än en första nivån avsnittet med 200 prenumerationer varje sammankedjade tooa andra nivån avsnittet med 20 prenumerationer .

Service Bus debiterar en åtgärd för varje vidarebefordrade meddelandet. Till exempel konfigurerad skicka ett meddelande tooa ämne med 20 prenumerationer, var och en av dem tooauto vidarebefordra meddelanden tooanother kö eller ett ämne, faktureras som 21 funktioner om alla prenumerationer på första nivån får en kopia av hello-meddelande.

toocreate en prenumeration som är länkad tooanother kö eller ett ämne hello skapare av hello prenumerationen måste ha **hantera** behörigheter för både hello käll- och hello målentitet. Skicka meddelanden toohello källa avsnittet kräver endast **skicka** behörigheter på hello käll-avsnittet.

## <a name="next-steps"></a>Nästa steg

Detaljerad information om automatisk vidarebefordring finns hello följande referensinformation om:

* [ForwardTo][QueueDescription.ForwardTo]
* [QueueDescription][QueueDescription]
* [SubscriptionDescription][SubscriptionDescription]

toolearn mer om Service Bus prestandaförbättringar, se 

* [Metodtips för bättre prestanda med hjälp av Service Bus-meddelanden](service-bus-performance-improvements.md)
* [Partitionerade meddelandeentiteter][Partitioned messaging entities].

[QueueDescription.ForwardTo]: /dotnet/api/microsoft.servicebus.messaging.queuedescription.forwardto#Microsoft_ServiceBus_Messaging_QueueDescription_ForwardTo
[SubscriptionDescription.ForwardTo]: /dotnet/api/microsoft.servicebus.messaging.subscriptiondescription.forwardto#Microsoft_ServiceBus_Messaging_SubscriptionDescription_ForwardTo
[QueueDescription]: /dotnet/api/microsoft.servicebus.messaging.queuedescription
[SubscriptionDescription]: /dotnet/api/microsoft.servicebus.messaging.queuedescription
[0]: ./media/service-bus-auto-forwarding/IC628631.gif
[1]: ./media/service-bus-auto-forwarding/IC628632.gif
[Partitioned messaging entities]: service-bus-partitioning.md
