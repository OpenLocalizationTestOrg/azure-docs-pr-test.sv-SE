---
title: "aaaAzure Service Bus Premium- och Standard-meddelanden prisnivå översikt över prisnivåer | Microsoft Docs"
description: "Service Bus Premium- och Standard-meddelandenivåer"
services: service-bus-messaging
documentationcenter: .net
author: djrosanova
manager: timlt
editor: 
ms.assetid: e211774d-821c-4d79-8563-57472d746c58
ms.service: service-bus-messaging
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 08/07/2017
ms.author: darosa;sethm
ms.openlocfilehash: 4eea5d86d342e858f50450308fb3d96a7a80b49e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="service-bus-premium-and-standard-messaging-tiers"></a>Service Bus Premium- och Standard-meddelandenivåer

Service Bus-meddelanden, inklusive entiteter som köer och ämnen, kombinerar meddelandefunktioner för företag med utförlig publicerings-/prenumerationssemantik i molnskala. Service Bus-meddelanden används som kommunikationsryggrad hello för många avancerade molnlösningar.

Hej *Premium* nivån av Service Bus-meddelanden adresser vanliga kundönskemål gällande skala, prestanda och tillgänglighet för verksamhetskritiska program. Även om hello funktionsuppsättningarna är snudd på identiska är dessa två nivåer av Service Bus-meddelanden utformade tooserve olika användningsfall.

En del övergripande skillnader visas i följande tabell hello.

| Premium | Standard |
| --- | --- |
| Högt genomflöde |Variabelt genomflöde |
| Förutsägbar prestanda |Variabel svarstid |
| Fast prissättning |Variabla priser – betala per användning |
| Möjlighet tooscale arbetsbelastningen uppåt och nedåt |Saknas |
| Meddelandestorlek in too1 MB |Meddelandestorlek in too256 KB |

**Service Bus Premium-meddelanden** ger resursisolering på hello CPU och minne så att varje kunds arbetsbelastning körs i isolering. Den här resursbehållaren kallas för en *meddelandefunktionsenhet*. Varje Premium-namnområde allokeras minst en meddelandefunktionsenhet. Du kan köpa 1, 2 eller 4 meddelandefunktionsenheter för varje Service Bus Premium-namnområde. En enda arbetsbelastning eller enhet kan spänna över flera meddelandefunktionsenheter och hello antalet meddelandefunktionsenheter kan ändras när du vill, även om faktureringen är per 24 timmar eller daglig taxa. hello resultatet är förutsägbara och repeterbara prestanda för Service Bus-lösningen.

Prestanda är inte bara mer förutsägbara och tillgängliga, utan de är snabbare också. Service Bus Premium-meddelanden bygger på hello lagringsmotorn som infördes i [Azure Event Hubs](https://azure.microsoft.com/services/event-hubs/). Med Premium-meddelanden är topprestanda mycket snabbare än med standardnivån hello.

## <a name="premium-messaging-technical-differences"></a>Premium-meddelanden – tekniska skillnader

hello beskrivs följande avsnitt några skillnader mellan Premium- och Standard-meddelandenivåerna.

### <a name="partitioned-queues-and-topics"></a>Partitionerade köer och ämnen

Partitionerade köer och ämnen stöds i Premium – meddelandefunktion. Entiteterna är faktiskt alltid partitionerade (och går inte att inaktivera). Dock Premium partitioneras köer och ämnen fungerar inte hello samma sätt som i hello Standard och Basic-nivåerna av Service Bus-meddelanden. Premium messaging har inte använder SQL som ett datalager och inte längre har hello konkurrens om resurser som är associerade med en delad plattform. Partitionering är därför inte nödvändigt tooimprove prestanda. Dessutom har hello partitionsantal ändrats från 16 partitioner i Standard-meddelanden too2 partitioner i Premium. Med två partitioner garanteras tillgänglighet och är ett mer passande antal hello Premium-körningsmiljön. 

Med Premium-meddelanden, när du anger hello storleken för en entitet med [MaxSizeInMegabytes](/dotnet/api/microsoft.servicebus.messaging.queuedescription.maxsizeinmegabytes#Microsoft_ServiceBus_Messaging_QueueDescription_MaxSizeInMegabytes), att storlek delas jämnt över hello 2 partitioner, till skillnad från [Standard partitionerade enheter](service-bus-partitioning.md#standard) i vilka hello Total storlek är 16 gånger hello angiven storlek. 

Mer information om partitionering finns i [Partitionerade köer och ämnen](service-bus-partitioning.md).

### <a name="express-entities"></a>Expressenheter

Eftersom meddelandehanteringen på premiumnivå körs i en helt isolerad körningsmiljö, stöds inte längre expressenheter i premiumnamnområden. Mer information om hello expressfunktionen finns hello [QueueDescription.EnableExpress](/dotnet/api/microsoft.servicebus.messaging.queuedescription.enableexpress#Microsoft_ServiceBus_Messaging_QueueDescription_EnableExpress) egenskapen.

Om du har kod som körs under Standard-meddelanden och vill tooport toohello Premium-nivån, kontrollerar du att hello [EnableExpress](/dotnet/api/microsoft.servicebus.messaging.queuedescription.enableexpress#Microsoft_ServiceBus_Messaging_QueueDescription_EnableExpress) egenskapen för**FALSKT** (hello standardvärdet).

## <a name="get-started-with-premium-messaging"></a>Kom igång med Premium-meddelandetjänster

Det är enkelt att komma igång med Premium-meddelanden och hello process är liknande toothat av Standard-meddelanden. Börja med att [skapa ett namnområde](service-bus-create-namespace-portal.md). Se till att du väljer **Premium** under **Prisnivå**.

![create-premium-namespace][create-premium-namespace]

Du kan också skapa [Premium-namnområden med hjälp av Azure Resource Manager-mallar](https://azure.microsoft.com/en-us/resources/templates/101-servicebus-pn-ar/).


## <a name="next-steps"></a>Nästa steg

toolearn mer om Service Bus-meddelanden finns i följande avsnitt hello.

* [Introduktion till Azure Service Bus Premium-meddelanden (blogginlägg)](http://azure.microsoft.com/blog/introducing-azure-service-bus-premium-messaging/)
* [Introduktion till Azure Service Bus Premium-meddelanden (Channel9)](https://channel9.msdn.com/Blogs/Subscribe/Introducing-Azure-Service-Bus-Premium-Messaging)
* [Översikt över Service Bus-meddelanden](service-bus-messaging-overview.md)
* [Hur toouse Service Bus-köer](service-bus-dotnet-get-started-with-queues.md)

<!--Image references-->

[create-premium-namespace]: ./media/service-bus-premium-messaging/select-premium-tier.png
