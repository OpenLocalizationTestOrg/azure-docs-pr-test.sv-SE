---
title: aaaService Fabric klustret Resource Manager - Placeringsprinciper | Microsoft Docs
description: "Översikt över ytterligare placeringsprinciper och regler för Service Fabric-tjänster"
services: service-fabric
documentationcenter: .net
author: masnider
manager: timlt
editor: 
ms.assetid: 5c2d19c6-dd40-4c4b-abd3-5c5ec0abed38
ms.service: Service-Fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 08/18/2017
ms.author: masnider
ms.openlocfilehash: bb58642520085ab3000f3929cf9aea7a8f6e3070
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="placement-policies-for-service-fabric-services"></a>Placeringsprinciper för service fabric-tjänster
Placeringsprinciper finns ytterligare regler som kan använda toogovern service placering i vissa specifika, mindre vanliga scenarier. Det är några exempel på dessa scenarier:

- Service Fabric-kluster omfattar geografiska avstånd, till exempel flera lokala Datacenter eller i Azure-regioner
- Din miljö sträcker sig över flera områden av geopolitiska eller juridiska kontroll eller några andra fall där du har princip gränser måste tooenforce
- Det finns kommunikation prestanda eller latens saker på grund av toolarge avstånd eller användning av långsammare eller mindre tillförlitliga nätverkslänkar
- Du behöver tookeep vissa arbetsbelastningar samordnat som en bästa prestanda, med andra arbetsbelastningar eller i närhet toocustomers

De flesta av dessa krav överensstämmer med hello fysiska struktur hello kluster representeras av hello feldomäner hello-klustret. 

hello avancerade placeringsprinciper som hjälper dig att lösa de här scenarierna är:

1. Ogiltig domäner
2. Nödvändiga domäner
3. Prioritera domäner
4. Inte tillåta replikering förpackning

De flesta av hello följande kontroller kan konfigureras via egenskaper för en nod och placeringen, men vissa är mer komplicerad. toomake saker enklare hello resurshanteraren för Service Fabric-kluster ger dessa ytterligare placeringsprinciper. Placering principerna är konfigurerade för tjänst per namngivna instansen. De kan också uppdateras dynamiskt.

## <a name="specifying-invalid-domains"></a>Ange ogiltig domäner
Hej **InvalidDomain** placering principen kan du toospecify att en viss Feldomän är ogiltig för en specifik tjänst. Den här principen ser till att en viss tjänst aldrig kör i ett visst område, till exempel grund geopolitiska eller företagets policy. Ogiltig flera domäner kan anges via olika principer.

<center>
![Ogiltig domän-exempel][Image1]
</center>

Kod:

```csharp
ServicePlacementInvalidDomainPolicyDescription invalidDomain = new ServicePlacementInvalidDomainPolicyDescription();
invalidDomain.DomainName = "fd:/DCEast"; //regulations prohibit this workload here
serviceDescription.PlacementPolicies.Add(invalidDomain);
```

PowerShell:

```posh
New-ServiceFabricService -ApplicationName $applicationName -ServiceName $serviceName -ServiceTypeName $serviceTypeName –Stateful -MinReplicaSetSize 3 -TargetReplicaSetSize 3 -PartitionSchemeSingleton -PlacementPolicy @("InvalidDomain,fd:/DCEast”)
```
## <a name="specifying-required-domains"></a>Ange nödvändiga domäner
hello krävs placering domänprincip kräver att hello-tjänsten finns endast i hello angiven domän. Flera domäner som krävs kan anges via olika principer.

<center>
![Domän-exempel][Image2]
</center>

Kod:

```csharp
ServicePlacementRequiredDomainPolicyDescription requiredDomain = new ServicePlacementRequiredDomainPolicyDescription();
requiredDomain.DomainName = "fd:/DC01/RK03/BL2";
serviceDescription.PlacementPolicies.Add(requiredDomain);
```

PowerShell:

```posh
New-ServiceFabricService -ApplicationName $applicationName -ServiceName $serviceName -ServiceTypeName $serviceTypeName –Stateful -MinReplicaSetSize 3 -TargetReplicaSetSize 3 -PartitionSchemeSingleton -PlacementPolicy @("RequiredDomain,fd:/DC01/RK03/BL2")
```

## <a name="specifying-a-preferred-domain-for-hello-primary-replicas-of-a-stateful-service"></a>Ange en önskad domän för hello primära repliker av en tillståndskänslig service
hello önskad primär domän anger hello fel domän tooplace hello primära i. hello primära avslutas med den här domänen när allt är felfri. Om hello domän eller hello primära repliken misslyckas eller stängs av, hello primära flyttas toosome på annan plats, helst i hello samma domän. Om den nya platsen inte är i hello önskad domän, hello klustret Resource Manager flyttar tillbaka toohello önskad domän så snart som möjligt. Den här inställningen är naturligt bara meningsfullt för tillståndskänsliga tjänster. Den här principen är mest användbara i kluster som omfattas i Azure-regioner eller flera Datacenter har men tjänster som föredrar placering på en viss plats. Att hålla primärfärgerna Stäng tootheir användare eller andra tjänster som ger kortare svarstid, särskilt för läsningar som hanteras av primärfärgerna som standard.

<center>
![Önskad primära domänerna och redundans][Image3]
</center>

```csharp
ServicePlacementPreferPrimaryDomainPolicyDescription primaryDomain = new ServicePlacementPreferPrimaryDomainPolicyDescription();
primaryDomain.DomainName = "fd:/EastUS/";
serviceDescription.PlacementPolicies.Add(invalidDomain);
```

PowerShell:

```posh
New-ServiceFabricService -ApplicationName $applicationName -ServiceName $serviceName -ServiceTypeName $serviceTypeName –Stateful -MinReplicaSetSize 3 -TargetReplicaSetSize 3 -PartitionSchemeSingleton -PlacementPolicy @("PreferredPrimaryDomain,fd:/EastUS")
```

## <a name="requiring-replica-distribution-and-disallowing-packing"></a>Kräver distribution av repliken och inte tillåta förpackning
Repliker är _normalt_ fördelad över fel- och domäner om hello klustret är felfri. Det finns dock fall där fler än en replik för en given partition kan hamna som är tillfälligt packade i en enda domän. Till exempel anta att hello klustret har nio noder i tre feldomäner, fd: / 0, fd: / 1 och fd: / 2. Även anta att tjänsten har tre repliker. Anta att hello noder som användes för dessa repliker i fd: / 1 och fd: / 2 avslutades. Normalt föredrar hello klustret Resource Manager andra noder i de samma feldomäner. I det här fallet anta att på grund av problem med toocapacity ingen av hello andra noder i dessa domäner var giltiga. Om hello klustret Resource Manager bygger ersättningar för dessa repliker, skulle det ha toochoose noder i fd: / 0. Dock gör _som_ skapar en situation där hello Feldomän begränsningen överskrids. Packa repliker ökar hello chans att hello hela replikuppsättningen kan kraschar eller gå förlorade. 

> [!NOTE]
> Mer information om begränsningar och begränsning prioriteter i allmänhet kolla [i det här avsnittet](service-fabric-cluster-resource-manager-management-integration.md#constraint-priorities).
>

Om du aldrig ser ett meddelande om hälsa som ”`hello Load Balancer has detected a Constraint Violation for this Replica:fabric:/<some service name> Secondary Partition <some partition ID> is violating hello Constraint: FaultDomain`”, och du har nått det här villkoret eller något som liknar den. Vanligtvis bara en eller två repliker är packade tillsammans tillfälligt. Så länge det är färre än ett kvorum av repliker i en viss domän, är du säker. Packkorg är sällsynt, men kan det hända, och vanligen dessa situationer är tillfälligt eftersom hello noder kommer tillbaka. Om hello-noder Håll ned och hello klustret Resource Manager måste toobuild ersättningar, finns vanligtvis det andra noder i hello perfekt feldomäner.

Vissa arbetsbelastningar föredrar alltid ha hello mål antal repliker, även om de packade till färre domäner. Dessa arbetsbelastningar vadslagning mot totala samtidiga permanent domän fel och kan vanligtvis återställa lokala tillstånd. Andra arbetsbelastningar tar snarare hello driftstopp tidigare än risken är korrekt eller förlust av data. De flesta produktionsarbetsbelastningar kör med fler än tre replikeringar och fler än tre feldomäner många giltig noder per feldomän. Därför kan hello standardbeteendet domän packkorg som standard. hello standardbeteendet kan normal belastningsutjämning och växling vid fel toohandle dessa extrema fall, även om tillfällig domän packkorg innebär.

Om du vill toodisable sådana packkorg för en viss arbetsbelastning, kan du ange hello `RequireDomainDistribution` princip på hello-tjänsten. När den här principen anges garanterar hello klustret Resource Manager inga två repliker från hello samma partition som körs i hello samma fault eller uppgradera domänen.

Kod:

```csharp
ServicePlacementRequireDomainDistributionPolicyDescription distributeDomain = new ServicePlacementRequireDomainDistributionPolicyDescription();
serviceDescription.PlacementPolicies.Add(distributeDomain);
```

PowerShell:

```posh
New-ServiceFabricService -ApplicationName $applicationName -ServiceName $serviceName -ServiceTypeName $serviceTypeName –Stateful -MinReplicaSetSize 3 -TargetReplicaSetSize 3 -PartitionSchemeSingleton -PlacementPolicy @("RequiredDomainDistribution")
```

Nu, skulle det vara möjligt toouse konfigurationerna för tjänster i ett kluster som inte geografiskt omfattas? Du kan, men det är inte en bra anledning för. hello bör krävs, ogiltig och önskade konfigurationer undvikas om hello scenarier behöver dem. Inte blir några bra tootry tooforce toorun för en viss arbetsbelastning i ett enda rack eller tooprefer vissa segmentet i din lokala klustret via en annan. Olika maskinvarukonfigurationer bör sprids feldomäner och hanteras via normala placeringen och egenskaper för en nod.

## <a name="next-steps"></a>Nästa steg
- Mer information om hur du konfigurerar tjänster, [Lär dig mer om hur du konfigurerar tjänster](service-fabric-cluster-resource-manager-configure-services.md)

[Image1]:./media/service-fabric-cluster-resource-manager-advanced-placement-rules-placement-policies/cluster-invalid-placement-domain.png
[Image2]:./media/service-fabric-cluster-resource-manager-advanced-placement-rules-placement-policies/cluster-required-placement-domain.png
[Image3]:./media/service-fabric-cluster-resource-manager-advanced-placement-rules-placement-policies/cluster-preferred-primary-domain.png
