---
title: "aaaAzure Arkitekturöversikt för Service Bus-meddelandebehandling | Microsoft Docs"
description: Beskriver hello meddelandet bearbetningsarkitekturen i Azure Service Bus.
services: service-bus-messaging
documentationcenter: na
author: sethmanheim
manager: timlt
editor: 
ms.assetid: baf94c2d-0e58-4d5d-a588-767f996ccf7f
ms.service: service-bus-messaging
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/23/2017
ms.author: sethm
ms.openlocfilehash: f7606e40cdf6db3797a0db2de9365453ff2a158e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="service-bus-architecture"></a>Service Bus-arkitektur
Den här artikeln beskriver hello meddelandet bearbetningsarkitekturen i Azure Service Bus.

## <a name="service-bus-scale-units"></a>Service Bus-skalningsenheter
Service Bus är organiserat i *skalningsenheter*. En skalningsenhet är en distributionsenhet och innehåller alla komponenter krävs kör hello-tjänsten. Varje region distribuerar en eller flera Service Bus-skalningsenheter.

En Service Bus-namnrymd är mappade tooa skalningsenhet. Hej skalningsenheten hanterar alla typer av enheter för Service Bus (köer, ämnen, prenumerationer). En Service Bus-skalningsenhet består av hello följande komponenter:

* **En uppsättning gateway-noder.** Gateway-noder autentiserar inkommande begäranden. Varje gateway-nod har en offentlig IP-adress.
* **En uppsättning asynkrona meddelandenoder.** Asynkrona meddelandenoder bearbetar begäranden om meddelandeentiteter.
* **Ett gateway-arkiv.** hello gateway-arkivet innehåller hello data för varje entitet som definieras i den här skalningsenheten. hello gateway-arkivet implementeras ovanpå en SQL Azure-databas.
* **Flera meddelandearkiv.** Meddelandearkiv håller hälsningsmeddelande i alla köer, ämnen och prenumerationer som definieras i den här skalningsenheten. De innehåller även alla data för prenumerationen. Om inte [partitionering meddelandeentiteter](service-bus-partitioning.md) är aktiverad, en kö eller ett ämne är mappade tooone meddelandearkiv. Prenumerationer lagras i hello samma meddelandearkiv som det överordnade ämnet. Förutom för Service Bus [Premium – meddelandefunktion](service-bus-premium-messaging.md), implementeras hello meddelandearkiven ovanpå SQL Azure-databaser.

## <a name="containers"></a>Behållare
Varje meddelandeentitet tilldelas en specifik behållare. En behållare är en logisk konstruktion som använder exakt en meddelanden store toostore alla relevanta data för den här behållaren. Varje behållare tilldelas tooa messaging Meddelandenod. Det finns normalt sett fler behållare än asynkrona meddelandenoder. Därför läser varje asynkron meddelandenod in flera behållare. hello distribution av behållare tooa asynkron Meddelandenod ordnas så att alla asynkrona meddelandenoder läses in lika. Om hello hello belastningen mönstret ändras (till exempel en hello behållarna belastas hårt) eller om en asynkron Meddelandenod blir tillfälligt otillgänglig, omfördelas behållarna bland hello asynkrona meddelandenoder.

## <a name="processing-of-incoming-messaging-requests"></a>Bearbetning av inkommande meddelandebegäranden
När en klient skickar en begäran tooService Bus, hello Azure-belastningsutjämnaren den tooany av hello gateway-noder. hello gateway-noden godkänner hello-begäran. Om hello begäran gäller en meddelandeentitet (kö, ämne, prenumeration), letar upp hello entiteten i gateway-arkivet hello hello gateway-noden och avgör vilka meddelanden store hello entiteten finns. Den söker sedan upp den asynkrona meddelandenoden för närvarande underhåller behållaren och skickar hello begäran toothat messaging Meddelandenod. hello messaging Meddelandenod behandlar hello begäran och uppdaterar hello entitetens tillstånd i hello behållarens Arkiv. hello messaging Meddelandenod sedan skickar hello svar tillbaka toohello gateway-noden, som skickar ett lämpligt svar tillbaka toohello klienten som utfärdade hello ursprungliga begäran.

![Bearbetning av inkommande meddelandebegäranden](./media/service-bus-architecture/ic690644.png)

## <a name="next-steps"></a>Nästa steg
Nu när du har läst en översikt över Service Bus-arkitektur finns hello följande länkar för mer information:

* [Översikt över Service Bus-meddelandetjänster](service-bus-messaging-overview.md)
* [Service Bus-grunder](service-bus-fundamentals-hybrid-solutions.md)
* [En lösning för köade meddelanden som använder Service Bus-köer](service-bus-dotnet-multi-tier-app-using-service-bus-queues.md)


