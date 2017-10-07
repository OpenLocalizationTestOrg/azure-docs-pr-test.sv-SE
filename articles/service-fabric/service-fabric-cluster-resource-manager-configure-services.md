---
title: "inställningar för aaaSpecify mått och placering i Azure mikrotjänster | Microsoft Docs"
description: "Som beskriver ett Service Fabric-tjänsten genom att ange mått, placeringen och andra placeringsprinciper."
services: service-fabric
documentationcenter: .net
author: masnider
manager: timlt
editor: 
ms.assetid: 16e135c1-a00a-4c6f-9302-6651a090571a
ms.service: Service-Fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 08/18/2017
ms.author: masnider
ms.openlocfilehash: c633518b5dbf0c7b84e0d46c06bf1f92365d184d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="configuring-cluster-resource-manager-settings-for-service-fabric-services"></a>Konfigurera klustret resource manager-inställningar för Service Fabric-tjänster
hello resurshanteraren för Service Fabric-klustret kan detaljerad kontroll över hello regler som styr varje person med namnet på tjänsten. Varje tjänsten kan ange regler för hur den ska allokeras i hello kluster. Varje tjänsten kan också definiera hello mått läggs tooreport, inklusive hur viktigt det är toothat service. Konfigurera services uppdelad i tre olika aktiviteter:

1. Konfigurera placeringen
2. Konfigurera mått
3. Konfigurera avancerade placeringsprinciper och andra regler (mindre vanliga)

## <a name="placement-constraints"></a>Placeringen
Placeringen är används toocontrol vilka noder i klustret hello en tjänst kan faktiskt körs på. Vanligtvis med en viss namnet tjänstinstansen eller alla tjänster av en viss typ begränsad toorun på en viss typ av noden. Placeringen är extensible. Du kan definiera en uppsättning egenskaper per nodtypen och sedan välja för dem med begränsningar när du skapar tjänster. Du kan också ändra placeringen för en tjänst när den körs. Detta ger dig toorespond toochanges i hello klustret eller hello krav för hello-tjänsten. hello egenskaperna för en viss nod kan också uppdateras dynamiskt i hello kluster. Mer information om placeringen och hur tooconfigure dem finns i [i den här artikeln](service-fabric-cluster-resource-manager-cluster-description.md#node-properties-and-placement-constraints)

## <a name="metrics"></a>Mått
Mått är hello uppsättning resurser som krävs för en viss namngiven tjänst. En tjänst mått konfigurationen innefattar hur mycket av den här resursen varje tillståndskänslig replik eller tillståndslösa instans av den tjänsten använder som standard. Mått kan även innehålla en vikt som anger hur viktigt belastningsutjämning som mått är toothat service om kompromisser krävs.

## <a name="advanced-placement-rules"></a>Av avancerade placeringsregler
Det finns andra typer av av placeringsregler som är användbara i mindre vanliga scenarier. Några exempel är:
- Begränsningarna som hjälper till med geografiskt distribuerade kluster
- Vissa programarkitekturer

Andra placeringsregler konfigureras via korrelationer eller principer.

## <a name="next-steps"></a>Nästa steg
- Mått är hur hello Service Fabric-kluster Resource Manager hanterar förbrukning och kapacitet i hello klustret. Mer om mått toolearn och hur tooconfigure dem, kolla [den här artikeln](service-fabric-cluster-resource-manager-metrics.md)
- Tillhörighet är ett läge som du kan konfigurera för dina tjänster. Det är inte vanligt, men om det behövs kan du läsa om den [här](service-fabric-cluster-resource-manager-advanced-placement-rules-affinity.md)
- Det finns många olika placeringsregler som kan konfigureras på din service toohandle fler scenarier. Du kan läsa mer om de olika placeringsprinciper [här](service-fabric-cluster-resource-manager-advanced-placement-rules-placement-policies.md)
- Starta från början hello och [få en introduktion toohello resurshanteraren för Service Fabric-kluster](service-fabric-cluster-resource-manager-introduction.md)
- toofind ut om hur hello klustret Resource Manager hanterar och balanserar belastningen i hello kluster kolla hello artikel på [belastningsutjämning](service-fabric-cluster-resource-manager-balancing.md)
- hello klustret Resource Manager har många alternativ för att beskriva hello klustret. toofind mer information om dem, Kolla in den här artikeln på [som beskriver ett Service Fabric-kluster](service-fabric-cluster-resource-manager-cluster-description.md)
