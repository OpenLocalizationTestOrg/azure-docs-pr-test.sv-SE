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
# <a name="placement-policies-for-service-fabric-services"></a><span data-ttu-id="a8d35-103">Placeringsprinciper för service fabric-tjänster</span><span class="sxs-lookup"><span data-stu-id="a8d35-103">Placement policies for service fabric services</span></span>
<span data-ttu-id="a8d35-104">Placeringsprinciper finns ytterligare regler som kan använda toogovern service placering i vissa specifika, mindre vanliga scenarier.</span><span class="sxs-lookup"><span data-stu-id="a8d35-104">Placement policies are additional rules that can be used toogovern service placement in some specific, less-common scenarios.</span></span> <span data-ttu-id="a8d35-105">Det är några exempel på dessa scenarier:</span><span class="sxs-lookup"><span data-stu-id="a8d35-105">Some examples of those scenarios are:</span></span>

- <span data-ttu-id="a8d35-106">Service Fabric-kluster omfattar geografiska avstånd, till exempel flera lokala Datacenter eller i Azure-regioner</span><span class="sxs-lookup"><span data-stu-id="a8d35-106">Your Service Fabric cluster spans geographic distances, such as multiple on-premises datacenters or across Azure regions</span></span>
- <span data-ttu-id="a8d35-107">Din miljö sträcker sig över flera områden av geopolitiska eller juridiska kontroll eller några andra fall där du har princip gränser måste tooenforce</span><span class="sxs-lookup"><span data-stu-id="a8d35-107">Your environment spans multiple areas of geopolitical or legal control, or some other case where you have policy boundaries you need tooenforce</span></span>
- <span data-ttu-id="a8d35-108">Det finns kommunikation prestanda eller latens saker på grund av toolarge avstånd eller användning av långsammare eller mindre tillförlitliga nätverkslänkar</span><span class="sxs-lookup"><span data-stu-id="a8d35-108">There are communication performance or latency considerations due toolarge distances or use of slower or less reliable network links</span></span>
- <span data-ttu-id="a8d35-109">Du behöver tookeep vissa arbetsbelastningar samordnat som en bästa prestanda, med andra arbetsbelastningar eller i närhet toocustomers</span><span class="sxs-lookup"><span data-stu-id="a8d35-109">You need tookeep certain workloads collocated as a best effort, either with other workloads or in proximity toocustomers</span></span>

<span data-ttu-id="a8d35-110">De flesta av dessa krav överensstämmer med hello fysiska struktur hello kluster representeras av hello feldomäner hello-klustret.</span><span class="sxs-lookup"><span data-stu-id="a8d35-110">Most of these requirements align with hello physical layout of hello cluster, represented as hello fault domains of hello cluster.</span></span> 

<span data-ttu-id="a8d35-111">hello avancerade placeringsprinciper som hjälper dig att lösa de här scenarierna är:</span><span class="sxs-lookup"><span data-stu-id="a8d35-111">hello advanced placement policies that help address these scenarios are:</span></span>

1. <span data-ttu-id="a8d35-112">Ogiltig domäner</span><span class="sxs-lookup"><span data-stu-id="a8d35-112">Invalid domains</span></span>
2. <span data-ttu-id="a8d35-113">Nödvändiga domäner</span><span class="sxs-lookup"><span data-stu-id="a8d35-113">Required domains</span></span>
3. <span data-ttu-id="a8d35-114">Prioritera domäner</span><span class="sxs-lookup"><span data-stu-id="a8d35-114">Preferred domains</span></span>
4. <span data-ttu-id="a8d35-115">Inte tillåta replikering förpackning</span><span class="sxs-lookup"><span data-stu-id="a8d35-115">Disallowing replica packing</span></span>

<span data-ttu-id="a8d35-116">De flesta av hello följande kontroller kan konfigureras via egenskaper för en nod och placeringen, men vissa är mer komplicerad.</span><span class="sxs-lookup"><span data-stu-id="a8d35-116">Most of hello following controls could be configured via node properties and placement constraints, but some are more complicated.</span></span> <span data-ttu-id="a8d35-117">toomake saker enklare hello resurshanteraren för Service Fabric-kluster ger dessa ytterligare placeringsprinciper.</span><span class="sxs-lookup"><span data-stu-id="a8d35-117">toomake things simpler, hello Service Fabric Cluster Resource Manager provides these additional placement policies.</span></span> <span data-ttu-id="a8d35-118">Placering principerna är konfigurerade för tjänst per namngivna instansen.</span><span class="sxs-lookup"><span data-stu-id="a8d35-118">Placement policies are configured on a per-named service instance basis.</span></span> <span data-ttu-id="a8d35-119">De kan också uppdateras dynamiskt.</span><span class="sxs-lookup"><span data-stu-id="a8d35-119">They can also be updated dynamically.</span></span>

## <a name="specifying-invalid-domains"></a><span data-ttu-id="a8d35-120">Ange ogiltig domäner</span><span class="sxs-lookup"><span data-stu-id="a8d35-120">Specifying invalid domains</span></span>
<span data-ttu-id="a8d35-121">Hej **InvalidDomain** placering principen kan du toospecify att en viss Feldomän är ogiltig för en specifik tjänst.</span><span class="sxs-lookup"><span data-stu-id="a8d35-121">hello **InvalidDomain** placement policy allows you toospecify that a particular Fault Domain is invalid for a specific service.</span></span> <span data-ttu-id="a8d35-122">Den här principen ser till att en viss tjänst aldrig kör i ett visst område, till exempel grund geopolitiska eller företagets policy.</span><span class="sxs-lookup"><span data-stu-id="a8d35-122">This policy ensures that a particular service never runs in a particular area, for example for geopolitical or corporate policy reasons.</span></span> <span data-ttu-id="a8d35-123">Ogiltig flera domäner kan anges via olika principer.</span><span class="sxs-lookup"><span data-stu-id="a8d35-123">Multiple invalid domains may be specified via separate policies.</span></span>

<span data-ttu-id="a8d35-124"><center>
![Ogiltig domän-exempel][Image1]
</center></span><span class="sxs-lookup"><span data-stu-id="a8d35-124"><center>
![Invalid Domain Example][Image1]
</center></span></span>

<span data-ttu-id="a8d35-125">Kod:</span><span class="sxs-lookup"><span data-stu-id="a8d35-125">Code:</span></span>

```csharp
ServicePlacementInvalidDomainPolicyDescription invalidDomain = new ServicePlacementInvalidDomainPolicyDescription();
invalidDomain.DomainName = "fd:/DCEast"; //regulations prohibit this workload here
serviceDescription.PlacementPolicies.Add(invalidDomain);
```

<span data-ttu-id="a8d35-126">PowerShell:</span><span class="sxs-lookup"><span data-stu-id="a8d35-126">Powershell:</span></span>

```posh
New-ServiceFabricService -ApplicationName $applicationName -ServiceName $serviceName -ServiceTypeName $serviceTypeName –Stateful -MinReplicaSetSize 3 -TargetReplicaSetSize 3 -PartitionSchemeSingleton -PlacementPolicy @("InvalidDomain,fd:/DCEast”)
```
## <a name="specifying-required-domains"></a><span data-ttu-id="a8d35-127">Ange nödvändiga domäner</span><span class="sxs-lookup"><span data-stu-id="a8d35-127">Specifying required domains</span></span>
<span data-ttu-id="a8d35-128">hello krävs placering domänprincip kräver att hello-tjänsten finns endast i hello angiven domän.</span><span class="sxs-lookup"><span data-stu-id="a8d35-128">hello required domain placement policy requires that hello service is present only in hello specified domain.</span></span> <span data-ttu-id="a8d35-129">Flera domäner som krävs kan anges via olika principer.</span><span class="sxs-lookup"><span data-stu-id="a8d35-129">Multiple required domains can be specified via separate policies.</span></span>

<span data-ttu-id="a8d35-130"><center>
![Domän-exempel][Image2]
</center></span><span class="sxs-lookup"><span data-stu-id="a8d35-130"><center>
![Required Domain Example][Image2]
</center></span></span>

<span data-ttu-id="a8d35-131">Kod:</span><span class="sxs-lookup"><span data-stu-id="a8d35-131">Code:</span></span>

```csharp
ServicePlacementRequiredDomainPolicyDescription requiredDomain = new ServicePlacementRequiredDomainPolicyDescription();
requiredDomain.DomainName = "fd:/DC01/RK03/BL2";
serviceDescription.PlacementPolicies.Add(requiredDomain);
```

<span data-ttu-id="a8d35-132">PowerShell:</span><span class="sxs-lookup"><span data-stu-id="a8d35-132">Powershell:</span></span>

```posh
New-ServiceFabricService -ApplicationName $applicationName -ServiceName $serviceName -ServiceTypeName $serviceTypeName –Stateful -MinReplicaSetSize 3 -TargetReplicaSetSize 3 -PartitionSchemeSingleton -PlacementPolicy @("RequiredDomain,fd:/DC01/RK03/BL2")
```

## <a name="specifying-a-preferred-domain-for-hello-primary-replicas-of-a-stateful-service"></a><span data-ttu-id="a8d35-133">Ange en önskad domän för hello primära repliker av en tillståndskänslig service</span><span class="sxs-lookup"><span data-stu-id="a8d35-133">Specifying a preferred domain for hello primary replicas of a stateful service</span></span>
<span data-ttu-id="a8d35-134">hello önskad primär domän anger hello fel domän tooplace hello primära i.</span><span class="sxs-lookup"><span data-stu-id="a8d35-134">hello Preferred Primary Domain specifies hello fault domain tooplace hello Primary in.</span></span> <span data-ttu-id="a8d35-135">hello primära avslutas med den här domänen när allt är felfri.</span><span class="sxs-lookup"><span data-stu-id="a8d35-135">hello Primary ends up in this domain when everything is healthy.</span></span> <span data-ttu-id="a8d35-136">Om hello domän eller hello primära repliken misslyckas eller stängs av, hello primära flyttas toosome på annan plats, helst i hello samma domän.</span><span class="sxs-lookup"><span data-stu-id="a8d35-136">If hello domain or hello Primary replica fails or shuts down, hello Primary moves toosome other location, ideally in hello same domain.</span></span> <span data-ttu-id="a8d35-137">Om den nya platsen inte är i hello önskad domän, hello klustret Resource Manager flyttar tillbaka toohello önskad domän så snart som möjligt.</span><span class="sxs-lookup"><span data-stu-id="a8d35-137">If this new location isn't in hello preferred domain, hello Cluster Resource Manager moves it back toohello preferred domain as soon as possible.</span></span> <span data-ttu-id="a8d35-138">Den här inställningen är naturligt bara meningsfullt för tillståndskänsliga tjänster.</span><span class="sxs-lookup"><span data-stu-id="a8d35-138">Naturally this setting only makes sense for stateful services.</span></span> <span data-ttu-id="a8d35-139">Den här principen är mest användbara i kluster som omfattas i Azure-regioner eller flera Datacenter har men tjänster som föredrar placering på en viss plats.</span><span class="sxs-lookup"><span data-stu-id="a8d35-139">This policy is most useful in clusters that are spanned across Azure regions or multiple datacenters but have services that prefer placement in a certain location.</span></span> <span data-ttu-id="a8d35-140">Att hålla primärfärgerna Stäng tootheir användare eller andra tjänster som ger kortare svarstid, särskilt för läsningar som hanteras av primärfärgerna som standard.</span><span class="sxs-lookup"><span data-stu-id="a8d35-140">Keeping Primaries close tootheir users or other services helps provide lower latency, especially for reads, which are handled by Primaries by default.</span></span>

<span data-ttu-id="a8d35-141"><center>
![Önskad primära domänerna och redundans][Image3]
</center></span><span class="sxs-lookup"><span data-stu-id="a8d35-141"><center>
![Preferred Primary Domains and Failover][Image3]
</center></span></span>

```csharp
ServicePlacementPreferPrimaryDomainPolicyDescription primaryDomain = new ServicePlacementPreferPrimaryDomainPolicyDescription();
primaryDomain.DomainName = "fd:/EastUS/";
serviceDescription.PlacementPolicies.Add(invalidDomain);
```

<span data-ttu-id="a8d35-142">PowerShell:</span><span class="sxs-lookup"><span data-stu-id="a8d35-142">Powershell:</span></span>

```posh
New-ServiceFabricService -ApplicationName $applicationName -ServiceName $serviceName -ServiceTypeName $serviceTypeName –Stateful -MinReplicaSetSize 3 -TargetReplicaSetSize 3 -PartitionSchemeSingleton -PlacementPolicy @("PreferredPrimaryDomain,fd:/EastUS")
```

## <a name="requiring-replica-distribution-and-disallowing-packing"></a><span data-ttu-id="a8d35-143">Kräver distribution av repliken och inte tillåta förpackning</span><span class="sxs-lookup"><span data-stu-id="a8d35-143">Requiring replica distribution and disallowing packing</span></span>
<span data-ttu-id="a8d35-144">Repliker är _normalt_ fördelad över fel- och domäner om hello klustret är felfri.</span><span class="sxs-lookup"><span data-stu-id="a8d35-144">Replicas are _normally_ distributed across fault and upgrade domains when hello cluster is healthy.</span></span> <span data-ttu-id="a8d35-145">Det finns dock fall där fler än en replik för en given partition kan hamna som är tillfälligt packade i en enda domän.</span><span class="sxs-lookup"><span data-stu-id="a8d35-145">However, there are cases where more than one replica for a given partition may end up temporarily packed into a single domain.</span></span> <span data-ttu-id="a8d35-146">Till exempel anta att hello klustret har nio noder i tre feldomäner, fd: / 0, fd: / 1 och fd: / 2.</span><span class="sxs-lookup"><span data-stu-id="a8d35-146">For example, let's say that hello cluster has nine nodes in three fault domains, fd:/0, fd:/1, and fd:/2.</span></span> <span data-ttu-id="a8d35-147">Även anta att tjänsten har tre repliker.</span><span class="sxs-lookup"><span data-stu-id="a8d35-147">Let's also say that your service has three replicas.</span></span> <span data-ttu-id="a8d35-148">Anta att hello noder som användes för dessa repliker i fd: / 1 och fd: / 2 avslutades.</span><span class="sxs-lookup"><span data-stu-id="a8d35-148">Let's say that hello nodes that were being used for those replicas in fd:/1 and fd:/2 went down.</span></span> <span data-ttu-id="a8d35-149">Normalt föredrar hello klustret Resource Manager andra noder i de samma feldomäner.</span><span class="sxs-lookup"><span data-stu-id="a8d35-149">Normally hello Cluster Resource Manager would prefer other nodes in those same fault domains.</span></span> <span data-ttu-id="a8d35-150">I det här fallet anta att på grund av problem med toocapacity ingen av hello andra noder i dessa domäner var giltiga.</span><span class="sxs-lookup"><span data-stu-id="a8d35-150">In this case, let's say due toocapacity issues none of hello other nodes in those domains were valid.</span></span> <span data-ttu-id="a8d35-151">Om hello klustret Resource Manager bygger ersättningar för dessa repliker, skulle det ha toochoose noder i fd: / 0.</span><span class="sxs-lookup"><span data-stu-id="a8d35-151">If hello Cluster Resource Manager builds replacements for those replicas, it would have toochoose nodes in fd:/0.</span></span> <span data-ttu-id="a8d35-152">Dock gör _som_ skapar en situation där hello Feldomän begränsningen överskrids.</span><span class="sxs-lookup"><span data-stu-id="a8d35-152">However, doing _that_ creates a situation where hello Fault Domain constraint is violated.</span></span> <span data-ttu-id="a8d35-153">Packa repliker ökar hello chans att hello hela replikuppsättningen kan kraschar eller gå förlorade.</span><span class="sxs-lookup"><span data-stu-id="a8d35-153">Packing replicas increases hello chance that hello whole replica set could go down or be lost.</span></span> 

> [!NOTE]
> <span data-ttu-id="a8d35-154">Mer information om begränsningar och begränsning prioriteter i allmänhet kolla [i det här avsnittet](service-fabric-cluster-resource-manager-management-integration.md#constraint-priorities).</span><span class="sxs-lookup"><span data-stu-id="a8d35-154">For more information on constraints and constraint priorities generally, check out [this topic](service-fabric-cluster-resource-manager-management-integration.md#constraint-priorities).</span></span>
>

<span data-ttu-id="a8d35-155">Om du aldrig ser ett meddelande om hälsa som ”`hello Load Balancer has detected a Constraint Violation for this Replica:fabric:/<some service name> Secondary Partition <some partition ID> is violating hello Constraint: FaultDomain`”, och du har nått det här villkoret eller något som liknar den.</span><span class="sxs-lookup"><span data-stu-id="a8d35-155">If you've ever seen a health message such as "`hello Load Balancer has detected a Constraint Violation for this Replica:fabric:/<some service name> Secondary Partition <some partition ID> is violating hello Constraint: FaultDomain`", then you've hit this condition or something like it.</span></span> <span data-ttu-id="a8d35-156">Vanligtvis bara en eller två repliker är packade tillsammans tillfälligt.</span><span class="sxs-lookup"><span data-stu-id="a8d35-156">Usually only one or two replicas are packed together temporarily.</span></span> <span data-ttu-id="a8d35-157">Så länge det är färre än ett kvorum av repliker i en viss domän, är du säker.</span><span class="sxs-lookup"><span data-stu-id="a8d35-157">So long as there are fewer than a quorum of replicas in a given domain, you're safe.</span></span> <span data-ttu-id="a8d35-158">Packkorg är sällsynt, men kan det hända, och vanligen dessa situationer är tillfälligt eftersom hello noder kommer tillbaka.</span><span class="sxs-lookup"><span data-stu-id="a8d35-158">Packing is rare, but it can happen, and usually these situations are transient since hello nodes come back.</span></span> <span data-ttu-id="a8d35-159">Om hello-noder Håll ned och hello klustret Resource Manager måste toobuild ersättningar, finns vanligtvis det andra noder i hello perfekt feldomäner.</span><span class="sxs-lookup"><span data-stu-id="a8d35-159">If hello nodes do stay down and hello Cluster Resource Manager needs toobuild replacements, usually there are other nodes available in hello ideal fault domains.</span></span>

<span data-ttu-id="a8d35-160">Vissa arbetsbelastningar föredrar alltid ha hello mål antal repliker, även om de packade till färre domäner.</span><span class="sxs-lookup"><span data-stu-id="a8d35-160">Some workloads would prefer always having hello target number of replicas, even if they are packed into fewer domains.</span></span> <span data-ttu-id="a8d35-161">Dessa arbetsbelastningar vadslagning mot totala samtidiga permanent domän fel och kan vanligtvis återställa lokala tillstånd.</span><span class="sxs-lookup"><span data-stu-id="a8d35-161">These workloads are betting against total simultaneous permanent domain failures and can usually recover local state.</span></span> <span data-ttu-id="a8d35-162">Andra arbetsbelastningar tar snarare hello driftstopp tidigare än risken är korrekt eller förlust av data.</span><span class="sxs-lookup"><span data-stu-id="a8d35-162">Other workloads would rather take hello downtime earlier than risk correctness or loss of data.</span></span> <span data-ttu-id="a8d35-163">De flesta produktionsarbetsbelastningar kör med fler än tre replikeringar och fler än tre feldomäner många giltig noder per feldomän.</span><span class="sxs-lookup"><span data-stu-id="a8d35-163">Most production workloads run with more than three replicas, more than three fault domains, and many valid nodes per fault domain.</span></span> <span data-ttu-id="a8d35-164">Därför kan hello standardbeteendet domän packkorg som standard.</span><span class="sxs-lookup"><span data-stu-id="a8d35-164">Because of this, hello default behavior allows domain packing by default.</span></span> <span data-ttu-id="a8d35-165">hello standardbeteendet kan normal belastningsutjämning och växling vid fel toohandle dessa extrema fall, även om tillfällig domän packkorg innebär.</span><span class="sxs-lookup"><span data-stu-id="a8d35-165">hello default behavior allows normal balancing and failover toohandle these extreme cases, even if that means temporary domain packing.</span></span>

<span data-ttu-id="a8d35-166">Om du vill toodisable sådana packkorg för en viss arbetsbelastning, kan du ange hello `RequireDomainDistribution` princip på hello-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="a8d35-166">If you want toodisable such packing for a given workload, you can specify hello `RequireDomainDistribution` policy on hello service.</span></span> <span data-ttu-id="a8d35-167">När den här principen anges garanterar hello klustret Resource Manager inga två repliker från hello samma partition som körs i hello samma fault eller uppgradera domänen.</span><span class="sxs-lookup"><span data-stu-id="a8d35-167">When this policy is set, hello Cluster Resource Manager ensures no two replicas from hello same partition run in hello same fault or upgrade domain.</span></span>

<span data-ttu-id="a8d35-168">Kod:</span><span class="sxs-lookup"><span data-stu-id="a8d35-168">Code:</span></span>

```csharp
ServicePlacementRequireDomainDistributionPolicyDescription distributeDomain = new ServicePlacementRequireDomainDistributionPolicyDescription();
serviceDescription.PlacementPolicies.Add(distributeDomain);
```

<span data-ttu-id="a8d35-169">PowerShell:</span><span class="sxs-lookup"><span data-stu-id="a8d35-169">Powershell:</span></span>

```posh
New-ServiceFabricService -ApplicationName $applicationName -ServiceName $serviceName -ServiceTypeName $serviceTypeName –Stateful -MinReplicaSetSize 3 -TargetReplicaSetSize 3 -PartitionSchemeSingleton -PlacementPolicy @("RequiredDomainDistribution")
```

<span data-ttu-id="a8d35-170">Nu, skulle det vara möjligt toouse konfigurationerna för tjänster i ett kluster som inte geografiskt omfattas?</span><span class="sxs-lookup"><span data-stu-id="a8d35-170">Now, would it be possible toouse these configurations for services in a cluster that was not geographically spanned?</span></span> <span data-ttu-id="a8d35-171">Du kan, men det är inte en bra anledning för.</span><span class="sxs-lookup"><span data-stu-id="a8d35-171">You could, but there’s not a great reason too.</span></span> <span data-ttu-id="a8d35-172">hello bör krävs, ogiltig och önskade konfigurationer undvikas om hello scenarier behöver dem.</span><span class="sxs-lookup"><span data-stu-id="a8d35-172">hello required, invalid, and preferred domain configurations should be avoided unless hello scenarios require them.</span></span> <span data-ttu-id="a8d35-173">Inte blir några bra tootry tooforce toorun för en viss arbetsbelastning i ett enda rack eller tooprefer vissa segmentet i din lokala klustret via en annan.</span><span class="sxs-lookup"><span data-stu-id="a8d35-173">It doesn't make any sense tootry tooforce a given workload toorun in a single rack, or tooprefer some segment of your local cluster over another.</span></span> <span data-ttu-id="a8d35-174">Olika maskinvarukonfigurationer bör sprids feldomäner och hanteras via normala placeringen och egenskaper för en nod.</span><span class="sxs-lookup"><span data-stu-id="a8d35-174">Different hardware configurations should be spread across fault domains and handled via normal placement constraints and node properties.</span></span>

## <a name="next-steps"></a><span data-ttu-id="a8d35-175">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="a8d35-175">Next steps</span></span>
- <span data-ttu-id="a8d35-176">Mer information om hur du konfigurerar tjänster, [Lär dig mer om hur du konfigurerar tjänster](service-fabric-cluster-resource-manager-configure-services.md)</span><span class="sxs-lookup"><span data-stu-id="a8d35-176">For more information on configuring services, [Learn about configuring Services](service-fabric-cluster-resource-manager-configure-services.md)</span></span>

[Image1]:./media/service-fabric-cluster-resource-manager-advanced-placement-rules-placement-policies/cluster-invalid-placement-domain.png
[Image2]:./media/service-fabric-cluster-resource-manager-advanced-placement-rules-placement-policies/cluster-required-placement-domain.png
[Image3]:./media/service-fabric-cluster-resource-manager-advanced-placement-rules-placement-policies/cluster-preferred-primary-domain.png
