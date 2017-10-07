---
title: aaaThrottling i hello Service Fabric-kluster resource manager | Microsoft Docs
description: "Lär dig tooconfigure hello begränsningar som tillhandahålls av hello resurshanteraren för Service Fabric-klustret."
services: service-fabric
documentationcenter: .net
author: masnider
manager: timlt
editor: 
ms.assetid: 4a44678b-a5aa-4d30-958f-dc4332ebfb63
ms.service: Service-Fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 08/18/2017
ms.author: masnider
ms.openlocfilehash: f418536911d3e3814e78a4d9f057dfb867ca7c63
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="throttling-hello-service-fabric-cluster-resource-manager"></a>Begränsning hello resurshanteraren för Service Fabric-kluster
Även om du har rätt konfigurerad hello klustret Resource Manager kan få upplöst hello klustret. Det kan till exempel vara samtidiga nod och feltolerans domän fel - vad som skulle hända om som uppstod under en uppgradering? hello klustret Resource Manager försöker alltid toofix allt använder hello klustrets resurser och försök tooreorganize och fixa hello-klustret. Begränsningar ger en backstop så att hello klustret kan använda resurser toostabilize - hello noder komma tillbaka, hello nätverkspartitioner läka, korrigerade bits distribueras.

toohelp med sådana här situationer hello resurshanteraren för Service Fabric-kluster innehåller flera begränsningar. Dessa begränsningar är alla ganska stort hammare. I allmänhet de får inte ändras utan noggrann planering och testning.

Om du ändrar hello klustret resurshanterare begränsas, bör du finjustera dem tooyour faktiska belastning. Du kan kontrollera du behöver toohave vissa begränsar på plats, även om det innebär hello klustret tar längre tid toostabilize i vissa situationer. Testning är obligatoriska toodetermine hello korrekta värden för begränsas. Begränsningar måste toobe tillräckligt högt tooallow hello klustret toorespond toochanges inom en rimlig tid och låg tillräckligt med tooactually hindra för mycket resursförbrukning. 

De flesta hello tid som vi har sett kunder använder begränsningar har eftersom de redan i en miljö med begränsad resurs. Några exempel är begränsad nätverksbandbredd för enskilda noder eller diskar som inte kan toobuild många tillståndskänslig repliker i parallella på grund av toothroughput begränsningar. Utan begränsningar, kan operations överväldigande resurserna orsakar operations toofail eller vara långsam. I sådana situationer kunder används begränsningar och känner de utöka hello lång tid det tar hello klustret tooreach ett stabilt tillstånd. Kunder kan också förstått de kan hamna körs med lägre pålitlighet när de har begränsats.


## <a name="configuring-hello-throttles"></a>Konfigurera hello begränsar

Service Fabric har två mekanismer för begränsning hello antalet replik förflyttningar. hello standardmekanism som fanns före Service Fabric 5.7 representerar begränsning som ett absolut antal flyttar tillåts. Detta fungerar inte för kluster i alla storlekar. I synnerhet för stora kluster hello standard kan värdet vara för liten avsevärt långsammare belastningsutjämning även när det är nödvändigt, samtidigt som du har ingen effekt i mindre kluster. Denna mekanism för tidigare har ersatts av procent-baserade begränsning skalas bättre med dynamiska kluster i vilka hello antal tjänster och noder ändras regelbundet.

hello begränsas baseras på en procentandel av hello antal repliker i hello kluster. Percetage baserat begränsas aktivera uttrycka hello regel: ”inte flytta mer än 10% av repliker 10: e minut”, till exempel.

hello konfigurationsinställningarna för procent-baserade begränsning är:

  - GlobalMovementThrottleThresholdPercentage - maximalt antal förflyttningar tillåts i klustret när som helst, uttryckt i procent av totalt antal repliker i hello klustret. 0 anger att ingen gräns. hello standardvärdet är 0. Om både den här inställningen och GlobalMovementThrottleThreshold anges används hello mer konservativ gränsen.
  - GlobalMovementThrottleThresholdPercentageForPlacement - maximalt antal förflyttningar av typ får användas under hello placering fas, uttryckt i procent av totalt antal repliker i hello klustret. 0 anger att ingen gräns. hello standardvärdet är 0. Om både den här inställningen och GlobalMovementThrottleThresholdForPlacement anges används hello mer konservativ gränsen.
  - GlobalMovementThrottleThresholdPercentageForBalancing - maximalt antal förflyttningar av typ får användas under hello NLB fas, uttryckt i procent av totalt antal repliker i hello klustret. 0 anger att ingen gräns. hello standardvärdet är 0. Om både den här inställningen och GlobalMovementThrottleThresholdForBalancing anges används hello mer konservativ gränsen.

När du anger hello begränsning i procent, anger du 5% som 0,05. Hej är då dessa begränsningar regleras hello GlobalMovementThrottleCountingInterval som anges i sekunder.


``` xml
<Section Name="PlacementAndLoadBalancing">
     <Parameter Name="GlobalMovementThrottleThresholdPercentage" Value="0" />
     <Parameter Name="GlobalMovementThrottleThresholdPercentageForPlacement" Value="0" />
     <Parameter Name="GlobalMovementThrottleThresholdPercentageForBalancing" Value="0" />
     <Parameter Name="GlobalMovementThrottleCountingInterval" Value="600" />
</Section>
```

värdbaserade kluster via ClusterConfig.json för fristående distributioner eller Template.json för Azure:

```json
"fabricSettings": [
  {
    "name": "PlacementAndLoadBalancing",
    "parameters": [
      {
          "name": "GlobalMovementThrottleThresholdPercentage",
          "value": "0.0"
      },
      {
          "name": "GlobalMovementThrottleThresholdPercentageForPlacement",
          "value": "0.0"
      },
      {
          "name": "GlobalMovementThrottleThresholdPercentageForBalancing",
          "value": "0.0"
      },
      {
          "name": "GlobalMovementThrottleCountingInterval",
          "value": "600"
      }
    ]
  }
]
```

### <a name="default-count-based-throttles"></a>Standard antal baserat begränsningar
Den här informationen anges om du har äldre kluster eller fortfarande behålla dessa konfigurationer i kluster som sedan har uppgraderats. I allmänhet rekommenderar vi att dessa ersätts med hello procent-baserade begränsas ovan. Eftersom procent-baserade begränsning är inaktiverad som standard är dessa begränsningar hello standard begränsningar för ett kluster tills de är inaktiverade och ersätts med hello procent-baserade begränsas. 

  - GlobalMovementThrottleThreshold – den här inställningen styr hello Totalt antal förflyttningar i hello kluster över tid. hello mängden tid har angetts i sekunder som hello GlobalMovementThrottleCountingInterval. hello standardvärdet för hello GlobalMovementThrottleThreshold är 1000 och hello standardvärdet för hello GlobalMovementThrottleCountingInterval är 600.
  - MovementPerPartitionThrottleThreshold – den här inställningen styr hello Totalt antal förflyttningar för alla partitioner för tjänsten över tid. hello mängden tid har angetts i sekunder som hello MovementPerPartitionThrottleCountingInterval. hello standardvärdet för hello MovementPerPartitionThrottleThreshold är 50 och hello standardvärdet för hello MovementPerPartitionThrottleCountingInterval är 600.

hello-konfiguration för dessa begränsningar följer samma mönster som hello procent-baserade begränsning hello.

## <a name="next-steps"></a>Nästa steg
- toofind ut om hur hello klustret Resource Manager hanterar och balanserar belastningen i hello kluster kolla hello artikel på [belastningsutjämning](service-fabric-cluster-resource-manager-balancing.md)
- hello klustret Resource Manager har många alternativ för att beskriva hello klustret. toofind mer information om dem, Kolla in den här artikeln på [som beskriver ett Service Fabric-kluster](service-fabric-cluster-resource-manager-cluster-description.md)
