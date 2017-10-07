---
title: aaaResource Manager-arkitektur | Microsoft Docs
description: "En översikt över arkitekturen i Service Fabric klustret Resource Manager."
services: service-fabric
documentationcenter: .net
author: masnider
manager: timlt
editor: 
ms.assetid: 6c4421f9-834b-450c-939f-1cb4ff456b9b
ms.service: Service-Fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 08/18/2017
ms.author: masnider
ms.openlocfilehash: 9ea80273d0566a2ac25143ada3662875656b57b8
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="cluster-resource-manager-architecture-overview"></a>Översikt över kluster resource manager-arkitektur
hello resurshanteraren för Service Fabric-klustret är en central tjänst som körs i hello klustret. Den hanterar hello önskad tjänsttillstånd hello hello kluster, särskilt med avseende tooresource förbrukning och regler för placering. 

toomanage hello resurser i klustret, hello resurshanteraren för Service Fabric-klustret måste ha flera typer av information:

- Tjänster som finns för närvarande
- Varje tjänst är aktuella (eller standard) förbrukning av nätverksresurser 
- hello återstående klustret kapacitet 
- hello-noder i klustret hello hello kapacitet 
- hello mängden resurser som används på varje nod

hello resurser som används för en viss tjänst kan ändras med tiden och tjänster är vanligtvis intresserad av mer än en typ av resurs. Över olika tjänster kan det finnas både verkliga fysiska och fysiska resurser som mäts. Tjänster kan spåra fysiska mått som minne och användning. Tjänster kan vanligare, intresserad logiska mått - ”WorkQueueDepth” eller ”TotalRequests”. Både logiska och fysiska mått kan användas i hello samma kluster. Mått kan delas mellan många tjänster eller vara specifika tooa viss tjänst.

## <a name="other-considerations"></a>Andra överväganden
hello ägare och operatorer hello-klustret kan skilja sig från hello tjänster och tillämpningsprogram författare eller på en minsta är hello samma personer med olika tak. När du utvecklar programmet vet du några saker om vad som krävs. Du har en uppskattning av hello resurser som den förbrukar och hur olika tjänster ska distribueras. Exempelvis måste hello webbnivå toorun på noder som exponeras toohello Internet, medan hello databastjänster inte bör. Ett annat exempel är begränsad hello webbtjänster förmodligen av CPU- och, vid hello data nivå services försiktighet mer om minne och användning. Dock hello person som hanterar en live-plats incident för tjänsten i produktion eller som hanterar en uppgradering toohello-tjänsten har ett annat jobb toodo och kräver olika verktyg. 

Är dynamiska både hello och tjänster:

- hello antalet noder i klustret hello kan växa eller krympa
- Noder i olika storlekar och typer kan hämtas och gå
- Tjänster kan skapas, tas bort, och ändra resursallokeringar och regler för placering
- Uppgraderingar eller andra hanteringsåtgärder kan rulla genom hello klustret på hello programmet på infrastruktur nivåer
- Fel kan inträffa när som helst.

## <a name="cluster-resource-manager-components-and-data-flow"></a>Klustret resource manager-komponenter och dataflöde
hello klustret Resource Manager har tootrack hello kraven för varje tjänst och hello förbrukningen av resurser som varje serviceobjektet i dessa tjänster. hello klustret Resource Manager har två konceptuella delar: agenter som körs på varje nod och en feltolerant tjänst. hello-agenterna på varje nod spåra belastningen rapporter från tjänster, sammanställd dem och rapportera dem med jämna mellanrum. hello klustret Resource Manager-tjänsten samlar in alla hello information från hello lokala agenter och reagerar baserat på dess aktuella konfiguration.

Nu ska vi titta på hello följande diagram:

<center>
![Resursen belastningsutjämnaren arkitektur][Image1]
</center>

Det finns många ändringar som kan inträffa under körning. Anta exempelvis att hello mycket resurser använda vissa tjänster ändringar, vissa tjänster misslyckas och vissa noder ansluta och lämna hello klustret. Alla hello ändringar på en nod sammanställs och skickas regelbundet toohello klustret Resource Manager-tjänsten (1,2) där de är samman igen, analyseras och lagras. Några sekunder som tjänsten tittar på hello ändringar och avgör om åtgärder som krävs (3). Det kan t.ex, Observera att vissa tom noder har lagts toohello klustret. Därför beslutar toomove vissa tjänster toothose noder. hello klustret Resource Manager kan också hända att en viss nod är överbelastad eller att vissa tjänster har misslyckades eller tagits bort, frigöra resurser någon annanstans.

Nu ska vi titta på hello följande diagram och se vad som händer nästa. Vi anger säga att hello hanteraren för filserverresurser att ändringar behövs. Den samordnar med andra system services (särskilt hello Failover Manager) toomake hello nödvändiga ändringar. Hello nödvändiga kommandon skickas toohello tillämpliga noder (4). Anta till exempel hello Resource Manager har upptäckt att nod5 var överbelastad och därför valt toomove tjänsten B från nod5 tooNode4. Hello slutet av hello omkonfiguration (5), hello kluster ser ut så här:

<center>
![Resursen belastningsutjämnaren arkitektur][Image2]
</center>

## <a name="next-steps"></a>Nästa steg
- hello klustret Resource Manager har många alternativ för att beskriva hello klustret. toofind mer information om dem, Kolla in den här artikeln på [som beskriver ett Service Fabric-kluster](./service-fabric-cluster-resource-manager-cluster-description.md)
- hello klustret resurshanterare primära uppgifter ombalansering hello kluster och genomdriva regler för placering. Mer information om hur du konfigurerar dessa beteenden finns [NLB Service Fabric-kluster](./service-fabric-cluster-resource-manager-balancing.md)

[Image1]:./media/service-fabric-cluster-resource-manager-architecture/Service-Fabric-Resource-Manager-Architecture-Activity-1.png
[Image2]:./media/service-fabric-cluster-resource-manager-architecture/Service-Fabric-Resource-Manager-Architecture-Activity-2.png
