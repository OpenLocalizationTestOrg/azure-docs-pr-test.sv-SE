---
title: "aaaAvailability av Service Fabric-tjänster | Microsoft Docs"
description: "Beskriver felinsamling, redundans och återställning för tjänster"
services: service-fabric
documentationcenter: .net
author: masnider
manager: timlt
editor: 
ms.assetid: 279ba4a4-f2ef-4e4e-b164-daefd10582e4
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 08/18/2017
ms.author: masnider
ms.openlocfilehash: c443aadfe31a1413359b08d34c4b7dd5db4edd16
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="availability-of-service-fabric-services"></a>Tillgänglighet för Service Fabric-tjänster
Den här artikeln ger en översikt över hur Service Fabric bibehåller tillgängligheten för en tjänst.

## <a name="availability-of-service-fabric-stateless-services"></a>Tillgängligheten för Service Fabric tillståndslösa tjänster
Azure Service Fabric-tjänster kan vara antingen tillståndskänslig eller tillståndslösa. Tillståndslösa tjänsten är en programtjänst som inte har några [lokala tillstånd](service-fabric-concepts-state.md) som måste toobe mycket tillgänglig eller tillförlitlig.

Skapa en tillståndslösa tjänsten måste du definiera en `InstanceCount`. Hej instansantalet definierar hello antal instanser av hello tillståndslösa tjänsten programlogik som ska köras i hello kluster. Öka hello antal instanser är hello rekommenderat sätt att skala ut en tillståndslös tjänst.

När en instans av en tillståndslös med namnet tjänsten misslyckas, skapas en ny instans på vissa berättigade nod i klustret hello. En instans av tillståndslösa tjänsten misslyckas på Nod1 och återskapas på nod5.

## <a name="availability-of-service-fabric-stateful-services"></a>Tillgänglighet för tillståndskänsliga tjänster för Service Fabric
En tillståndskänslig tjänst har några tillstånd som associeras med den. I Service Fabric modelleras en tillståndskänslig service som en uppsättning repliker. Varje replik är en instans av hello koden för hello-tjänst som också har en kopia av hello tillstånd för tjänsten som körs. Läsa och skriva åtgärder utförs i en replik (kallas hello primära). Ändringar toostate från skrivåtgärder är *replikeras* toohello repliker i hello replikuppsättningen (kallas Active sekundärservrar) och har använts. 

Det kan finnas endast en primär replik, men det kan finnas flera aktiva sekundära repliker. hello antal aktiva sekundära repliker konfigureras och ett högre antal repliker kan tolerera ett större antal samtidiga program- och maskinvarufel.

Om hello primära repliken kraschar gör Service Fabric något av hello aktiva sekundära repliker hello primära repliken. Den här aktiva sekundära repliken har redan hello uppdaterad version av hello tillstånd (via *replikering*), och det kan fortsätta att bearbeta ytterligare Läs- och skrivåtgärder.

Detta begrepp i en replik som en primär eller aktiva sekundära kallas hello Replikroll.

### <a name="replica-roles"></a>Replik roller
hello rollen för en replik är används toomanage hello livscykel hello tillstånd som hanteras av den repliken. En replik vars roll är primär services diskläsningsbegäranden. hello primära hanterar även alla skrivbegäranden genom uppdatera dess tillstånd och replikerar hello ändringar. Dessa ändringar är tillämpade toohello Active sekundärservrar i hello replikuppsättningen. hello jobb av en aktiv sekundär är tooreceive tillståndsändringar som hello primära repliken har replikerats och uppdatera vyn för hello tillstånd.

> [!NOTE]
> Överordnad programmering modeller som [Reliable Actors](service-fabric-reliable-actors-introduction.md) och [Reliable Services](service-fabric-reliable-services-introduction.md) dölja hello begreppet replikroll från hello-utvecklare. I aktörer är hello begreppet roll onödigt när i Services det i hög grad är förenklad för de flesta scenarier.
>

## <a name="next-steps"></a>Nästa steg
Mer information om Service Fabric-begrepp finns hello följande artiklar:

- [Skala Service Fabric-tjänster](service-fabric-concepts-scalability.md)
- [Partitionering Service Fabric-tjänster](service-fabric-concepts-partitioning.md)
- [Definiera och hantera tillstånd](service-fabric-concepts-state.md)
- [Reliable Services](service-fabric-reliable-services-introduction.md)
