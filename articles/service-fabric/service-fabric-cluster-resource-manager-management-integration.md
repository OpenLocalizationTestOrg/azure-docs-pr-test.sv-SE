---
title: aaaService Fabric klustret Resource Manager - integrering | Microsoft Docs
description: "En översikt över hello integration punkter mellan hello klustret Resource Manager och Service Fabric."
services: service-fabric
documentationcenter: .net
author: masnider
manager: timlt
editor: 
ms.assetid: 956cd0b8-b6e3-4436-a224-8766320e8cd7
ms.service: Service-Fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 08/18/2017
ms.author: masnider
ms.openlocfilehash: 9a24c9de121fbe2e8e5e8e4d117e64686918936a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="cluster-resource-manager-integration-with-service-fabric-cluster-management"></a>Klustret resource manager-integrering med hantering av Service Fabric-kluster
hello resurshanteraren för Service Fabric-klustret enheten inte uppgraderingar i Service Fabric, men den är inblandad. hello första sätt hello klustret Resource Manager hjälper dig med hantering av spårning hello önskade hello kluster och hello tjänster i den. hello klustret Resource Manager skickar ut hälsorapporter när den inte kan försätta hello klustret i hello önskad konfiguration. Till exempel om det finns inte tillräckligt skickar kapacitet hello klustret Resource Manager ut hälsa varningar och fel som indikerar hello problem. En annan typ av integrering har toodo med så här fungerar uppgraderingar. hello klustret Resource Manager ändrar sitt beteende något under uppgraderingar.  

## <a name="health-integration"></a>Integrering av hälsotillstånd
hello klustret Resource Manager spårar ständigt hello regler som har definierats för att placera dina tjänster. Den spårar också hello återstående kapacitet för varje mått på hello noder och i hello kluster och hello klustret som helhet. Om den inte kan uppfylla dessa regler eller om det finns tillräcklig kapacitet orsakat hälsa varningar och fel. Till exempel om en nod är över kapacitet och hello försöker klustret Resource Manager toofix hello situationen genom att flytta tjänster. Om den inte kan korrigera hello situationen skickar en varning om hälsa som anger vilken nod är överbelastade och för vilka mått.

Ett annat exempel på hello Resource Manager hälsovarningar är överträdelser av placeringsbegränsningar. Till exempel om du har definierat begränsningen placering (som `“NodeColor == Blue”`) och hello Resource Manager identifierar ett brott mot denna begränsning, den genererar en varning om hälsa. Detta gäller för anpassade villkor och hello standardbegränsningar (till exempel hello fel och uppgradera domänen begränsningar).

Här är ett exempel på en sådan hälsorapport. I det här fallet är hello hälsorapport för en av hello systemtjänsten partitioner. hälsningsmeddelande hälsa anger hello repliker av den partitionen tillfälligt packas i för få uppgradera domäner.

```posh
PS C:\Users\User > Get-WindowsFabricPartitionHealth -PartitionId '00000000-0000-0000-0000-000000000001'


PartitionId           : 00000000-0000-0000-0000-000000000001
AggregatedHealthState : Warning
UnhealthyEvaluations  :
                        Unhealthy event: SourceId='System.PLB', Property='ReplicaConstraintViolation_UpgradeDomain', HealthState='Warning', ConsiderWarningAsError=false.

ReplicaHealthStates   :
                        ReplicaId             : 130766528804733380
                        AggregatedHealthState : Ok

                        ReplicaId             : 130766528804577821
                        AggregatedHealthState : Ok

                        ReplicaId             : 130766528854889931
                        AggregatedHealthState : Ok

                        ReplicaId             : 130766528804577822
                        AggregatedHealthState : Ok

                        ReplicaId             : 130837073190680024
                        AggregatedHealthState : Ok

HealthEvents          :
                        SourceId              : System.PLB
                        Property              : ReplicaConstraintViolation_UpgradeDomain
                        HealthState           : Warning
                        SequenceNumber        : 130837100116930204
                        SentAt                : 8/10/2015 7:53:31 PM
                        ReceivedAt            : 8/10/2015 7:53:33 PM
                        TTL                   : 00:01:05
                        Description           : hello Load Balancer has detected a Constraint Violation for this Replica: fabric:/System/FailoverManagerService Secondary Partition 00000000-0000-0000-0000-000000000001 is
                        violating hello Constraint: UpgradeDomain Details: UpgradeDomain ID -- 4, Replica on NodeName -- Node.8 Currently Upgrading -- false Distribution Policy -- Packing
                        RemoveWhenExpired     : True
                        IsExpired             : False
                        Transitions           : Ok->Warning = 8/10/2015 7:13:02 PM, LastError = 1/1/0001 12:00:00 AM
```

Här är vad meddelandet hälsa tala om för oss är:

1. Alla hello repliker själva är felfria: varje har AggregatedHealthState: Ok
2. hello uppgradera distributionsplatsen domänbegränsningar överskrids för närvarande. Det innebär att en viss uppgradera domän har flera repliker från den här partitionen än den borde.
3. Vilken nod som innehåller hello replik orsakar hello överträdelse. I det här fallet är den hello noden med hello namnet ”Node.8”
4. Om en uppgradering för närvarande pågår för detta partitions (”för närvarande uppgradering--false”)
5. hello principen för programdistribution för den här tjänsten: ”Distribution princip--packkorg”. Detta styrs av hello `RequireDomainDistribution` [policy placering](service-fabric-cluster-resource-manager-advanced-placement-rules-placement-policies.md#requiring-replica-distribution-and-disallowing-packing). ”Packa” anger att i det här fallet DomainDistribution _inte_ krävs, så att vi vet att policy placering inte har angetts för den här tjänsten. 
6. När hello rapporten har hänt - 8/10/2015 7:13:02 PM.

Information som den här befogenheter aviseringar att brand i produktion toolet du vet att något är fel och är också används toodetect och stoppa felaktiga uppgraderingar. I det här fallet kan vi toosee om vi kan ta reda på varför hello Resource Manager hade toopack hello repliker i hello uppgraderingen domän. Vanligtvis packa är tillfälligt eftersom hello noder i hello andra uppgradera domäner inte ned, t.ex.

Anta att hello klustret Resource Manager försöker tooplace vissa tjänster, men det inte finns några lösningar som fungerar. När tjänster inte kan placeras, är det vanligtvis på något av följande orsaker hello:

1. Vissa övergående tillstånd har gjort det omöjligt tooplace den här tjänstinstansen eller replik korrekt
2. hello service placering kraven är unsatisfiable.

I dessa fall kan hjälpa dig att avgöra varför hello-tjänsten kan inte placeras i hälsorapporter från hello klustret Resource Manager. Vi kallar processen hello begränsningen eliminering sekvensen. Under den stegvisa hello system hello konfigurerade begränsningar som påverkar hello-tjänsten och poster de undanröjer. Det här sättet när tjänster inte kan toobe placeras, du kan se vilka noder har tagits bort och varför.

## <a name="constraint-types"></a>Villkorstyper
Vi pratar om var och en av hello olika begränsningar i dessa rapporter om hälsotillstånd. Hälsotillstånd meddelanden relaterade toothese begränsningar visas när repliker inte kan placeras.

* **ReplicaExclusionStatic** och **ReplicaExclusionDynamic**: dessa villkor anger att en lösning avvisades eftersom två tjänstobjekt från hello samma partition skulle ha toobe placerad hello samma nod. Detta är inte tillåten eftersom sedan fel på noden påverkar alltför partitionen. ReplicaExclusionStatic och ReplicaExclusionDynamic är nästan hello samma regel och hello skillnader inte verkligen är viktiga. Om du ser en begränsning eliminering sekvens som innehåller antingen hello ReplicaExclusionStatic eller ReplicaExclusionDynamic begränsning, hello klustret Resource Manager tror att det inte finns tillräckligt med noder. Detta kräver återstående lösningar toouse dessa ogiltig placeringar som inte är tillåtet. hello kommer andra begränsningar i hello sekvens vanligtvis berätta varför noder elimineras i hello första plats.
* **PlacementConstraint**: Om du ser det här meddelandet innebär att vi elimineras vissa noder eftersom de matchade hello service placeringen. Vi spåra ut placeringen hello har konfigurerats som en del av det här meddelandet. Detta är normalt om du har en begränsning för placering som definierats. Men om placering begränsningen felaktigt orsakar för många noder toobe elimineras det här är hur du vill se.
* **NodeCapacity**: den här begränsningen innebär att hello klustret Resource Manager inte kunde placera hello repliker på hello anges noder eftersom som skulle placera dem över kapacitet.
* **Tillhörighet**: det här villkoret visar vi inte kunde placera hello replik på hello påverkas noder eftersom det skulle orsaka en överträdelse av hello tillhörighet begränsningen. Mer information om mappning mellan är i [i den här artikeln](service-fabric-cluster-resource-manager-advanced-placement-rules-affinity.md)
* **FaultDomain** och **UpgradeDomain**: den här begränsningen eliminerar noder om du placerar hello repliken på hello angett noder skulle orsaka packa i ett visst fel eller en domän. Flera exempel diskutera detta villkor visas i avsnittet hello på [fel- och begränsningar för domänen och resultatet](service-fabric-cluster-resource-manager-cluster-description.md)
* **PreferredLocation**: du normalt inte bör se den här begränsningen att ta bort noder från hello lösning eftersom den körs som en optimering som standard. hello önskade platsbegränsningen finns också under uppgraderingar. Det är används toomove tjänster tillbaka toowhere när hello uppgraderingen igång under uppgraderingen.

## <a name="blocklisting-nodes"></a>Blocklisting noder
Ett annat meddelande hello klustret Resource Manager hälsorapporter är när noder är blocklisted. Du kan se blocklisting som en tillfällig begränsning som tillämpas automatiskt åt dig. Noder får blocklisted när de har upprepade fel när den startas instanser av den typ av tjänst. Noderna är blocklisted på grundval av per tjänsttyp. En nod kan vara blocklisted för en tjänst men inte en annan. 

Du ser blocklisting startar ofta under utveckling: vissa bugg orsakar toocrash din service-värden vid start. Service Fabric toocreate hello tjänstvärden några gånger och i hello felet kvarstår. Efter några försök hello nod hämtar blocklisted och hello klustret Resource Manager försöker toocreate hello service någon annanstans. Om felet upprepas på flera noder, är det möjligt att alla giltiga hello-noder i hello klustret hamnar blockeras. Blocklisting kan också ta bort så många noder finns inte tillräckligt med kan har starta hello service toomeet hello önskad skala. Normalt visas ytterligare fel eller varningar från hello klustret Resource Manager som anger att hello service understiger hello önskade replik eller instansantal, samt hälsa meddelanden som anger vilket hello-felet är som leder toohello blocklisting hello första plats.

Blocklisting är inte ett permanent villkor. Hello noden tas bort från hello blocklist och Service Fabric kan aktivera hello tjänster på noden igen efter några minuter. Om tjänster fortsätter toofail hello noden är blocklisted för tjänsten igen. 

### <a name="constraint-priorities"></a>Begränsningen prioriteter

> [!WARNING]
> Ändra begränsningen prioriteter rekommenderas inte och kan ha betydande negativa effekter på klustret. hello informationen nedan ges för referens hello standard begränsningen prioriteringar och deras beteende. 
>

Med alla dessa villkor, kan du ha tänker ”Hej – jag tror att fel domän begränsningar är hello viktigaste i systemet. I ordning tooensure hello fel domänbegränsningar inte bröt mot, jag är beredda tooviolate andra begränsningar ”.

Begränsningar kan konfigureras med olika prioritet. Dessa är:

   - ”hårda” (0)
   - ”soft” (1)
   - ”optimering” (2)
   - ”off” (-1). 
   
De flesta av hello villkor är som standard konfigurerade som hårda villkor.

Det är ovanligt att ändra hello prioritet med begränsningar. Det har förekommit gånger där begränsningen prioriteter behövs toochange, vanligtvis toowork runt några fel eller beteende som påverkade hello-miljö. Vanligtvis hello flexibilitet hello begränsningen prioritet infrastruktur har arbetat mycket bra, men det behövs inte ofta. De flesta hello tid allt placeras på deras Standardprioriteter. 

hello prioritetsnivå inte innebär att ett visst villkor _kommer_ ha brutits, eller som ska alltid vara uppfyllda. Begränsningen prioriteter definiera begränsningar är framtvingade i vilken ordning. Prioriteringar definiera hello kompromisser när det är omöjligt toosatisfy alla begränsningar. Vanligtvis uppfyllas alla hello villkor om det inte finns något annat i hello-miljö. Några exempel på scenarier som dirigerar tooconstraint överträdelser är motstridiga villkor eller stort antal samtidiga fel.

I avancerade fall kan ändra du hello begränsningen prioriteter. Anta exempelvis att du vill ha tooensure att tillhörighet skulle alltid ha brutits när nödvändiga toosolve nod kapacitet. tooachieve, kan du ange hello prioritet för för ”soft” hello tillhörighet begränsningen (1) och lämna hello kapacitet villkor med värdet för ”svårt” (0).

hello prioritet standardvärden för hello olika villkor har angetts i hello följande konfiguration:

ClusterManifest.xml

```xml
        <Section Name="PlacementAndLoadBalancing">
            <Parameter Name="PlacementConstraintPriority" Value="0" />
            <Parameter Name="CapacityConstraintPriority" Value="0" />
            <Parameter Name="AffinityConstraintPriority" Value="0" />
            <Parameter Name="FaultDomainConstraintPriority" Value="0" />
            <Parameter Name="UpgradeDomainConstraintPriority" Value="1" />
            <Parameter Name="PreferredLocationConstraintPriority" Value="2" />
        </Section>
```

värdbaserade kluster via ClusterConfig.json för fristående distributioner eller Template.json för Azure:

```json
"fabricSettings": [
  {
    "name": "PlacementAndLoadBalancing",
    "parameters": [
      {
          "name": "PlacementConstraintPriority",
          "value": "0"
      },
      {
          "name": "CapacityConstraintPriority",
          "value": "0"
      },
      {
          "name": "AffinityConstraintPriority",
          "value": "0"
      },
      {
          "name": "FaultDomainConstraintPriority",
          "value": "0"
      },
      {
          "name": "UpgradeDomainConstraintPriority",
          "value": "1"
      },
      {
          "name": "PreferredLocationConstraintPriority",
          "value": "2"
      }
    ]
  }
]
```

## <a name="fault-domain-and-upgrade-domain-constraints"></a>Fel domän- och begränsningar för domänen
hello klustret Resource Manager vill tookeep services sprids ut bland fel- och domäner. Den modeller detta som en begränsning i hello klustret Resource Manager-motorn. För mer information om hur de används och deras specifika beteende, kolla hello artikel på [klusterkonfigurationen](service-fabric-cluster-resource-manager-cluster-description.md#fault-and-upgrade-domain-constraints-and-resulting-behavior).

hello klustret Resource Manager behöva toopack några repliker i en uppgraderingsdomän i ordning toodeal med uppgraderingar, fel eller andra kränkningar begränsningen. Packa i fel eller uppgradera domäner normalt sker bara när det finns några fel eller andra omsättning i hello system förhindrar korrekt placering. Om du vill tooprevent packa även under sådana situationer kan du använda hello `RequireDomainDistribution` [policy placering](service-fabric-cluster-resource-manager-advanced-placement-rules-placement-policies.md#requiring-replica-distribution-and-disallowing-packing). Observera att detta kan påverka tjänstetillgänglighet och tillförlitlighet som en sidoeffekt, så den noggrant igenom.

Om hello-miljö är korrekt konfigurerade uppfylls alla begränsningar fullständigt, även under uppgraderingar. hello är nyckel att hello klustret Resource Manager bevakar för dina villkor. När den identifierar en överträdelse det omedelbart rapporterar den och försöker toocorrect hello problemet.

## <a name="hello-preferred-location-constraint"></a>Hej prioriterade platsbegränsningen
Hej PreferredLocation begränsningen är lite annorlunda eftersom den har två olika användningsområden. En används för den här begränsningen under programuppgraderingar. hello klustret Resource Manager hanterar automatiskt den här begränsningen under uppgraderingar. Det är används tooensure när uppgraderingar är klara att repliker returnera tootheir ursprungliga platser. hello andra hello PreferredLocation begränsningen används för hello [ `PreferredPrimaryDomain` policy placering](service-fabric-cluster-resource-manager-advanced-placement-rules-placement-policies.md). Båda dessa optimeringar och därför hello PreferredLocation begränsningen är hello endast villkor med värdet för ”optimering” som standard.

## <a name="upgrades"></a>Uppgraderingar
hello klustret Resource Manager hjälper också under program- och klusteruppgradering under vilken den har två jobb:

* Se till att hello regler för hello klustret inte äventyras
* Försök toohelp hello uppgradera gå smidigt

### <a name="keep-enforcing-hello-rules"></a>Att framtvinga hello regler
hello huvudsakliga sak toobe medveten om är hello regler – strikt begränsningar som placeringen och kapacitet för hello - tillämpas fortfarande under uppgraderingar. Placeringen se till att dina arbetsbelastningar endast köras var de ska kunna, även under uppgraderingar. När tjänster är mycket begränsad, kan uppgraderingar ta längre tid. När tas tjänsten hello eller hello nod den körs på för en uppdatering som det kan finnas några alternativ för där du kan gå.

### <a name="smart-replacements"></a>Smart ersättningar
När en uppgradering startar tar hello Resource Manager en ögonblicksbild av hello aktuella placering av hello klustret. Eftersom varje uppgradera domän är klar försöker tooreturn hello tjänster som fanns i den ursprungliga sorteringsordningen uppgraderingen domän tootheir. Det här sättet finns högst två övergångar för en tjänst under hello uppgraderingen. Det finns en flyttas ut hello påverkas nod och en flytta tillbaka. Returnerar hello klustret eller tjänst toohow innan hello uppgraderingen säkerställer också hello uppgraderingen påverkar inte hello layout hello-klustret. 

### <a name="reduced-churn"></a>Minskad omsättning
En annan sak som sker under uppgraderingar är att hello klustret Resource Manager stängs av belastningsutjämning. Hindrar NLB förhindrar onödiga reaktioner toohello uppgradering, t.ex. Flytta tjänster till noder som har tömts för hello uppgradering. Om hello uppgraderingen i fråga är en uppgradering av klustret, balanserade hello hela klustret inte under hello uppgraderingen. Begränsningen kontrollerar förblir aktiv, endast transport baserat på hello proaktiv fördelningen av mått är inaktiverad.

### <a name="buffered-capacity--upgrade"></a>Buffrat kapacitet och uppgradering
Vanligtvis vill hello uppgradera toocomplete även om hello klustret är begränsad eller Stäng toofull. Hantera hello kapacitet hello klustret är även viktigare under uppgraderingar än vanligt. Beroende på hello måste antal uppgraderingsdomäner mellan 5 och 20 procent av kapacitet migreras som hello uppgraderingen samlar via hello klustret. Arbetet har toogo någonstans. Det är där hello begreppet [buffras kapaciteter](service-fabric-cluster-resource-manager-cluster-description.md#buffered-capacity) är användbart. Buffrat kapacitet följs under normal drift. hello klustret Resource Manager kan fylla noder in tootheir total kapacitet (förbrukar hello buffert) under uppgraderingar om det behövs.

## <a name="next-steps"></a>Nästa steg
* Starta från början hello och [få en introduktion toohello resurshanteraren för Service Fabric-kluster](service-fabric-cluster-resource-manager-introduction.md)
