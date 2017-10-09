---
title: 'Service Fabric klustret Resource Manager: flytt kostnad | Microsoft Docs'
description: "Översikt över förflyttningskostnad för Service Fabric-tjänster"
services: service-fabric
documentationcenter: .net
author: masnider
manager: timlt
editor: 
ms.assetid: f022f258-7bc0-4db4-aa85-8c6c8344da32
ms.service: Service-Fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 08/18/2017
ms.author: masnider
ms.openlocfilehash: 65d4ac73efffcf7b25b1e95da6f9012a9238cb75
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="service-movement-cost"></a><span data-ttu-id="96b8f-103">Tjänsten förflyttningskostnad</span><span class="sxs-lookup"><span data-stu-id="96b8f-103">Service movement cost</span></span>
<span data-ttu-id="96b8f-104">En faktor som hello resurshanteraren för Service Fabric-klustret anser vid toodetermine vilka ändringar toomake tooa kluster är hello kostnaden för dessa ändringar.</span><span class="sxs-lookup"><span data-stu-id="96b8f-104">A factor that hello Service Fabric Cluster Resource Manager considers when trying toodetermine what changes toomake tooa cluster is hello cost of those changes.</span></span> <span data-ttu-id="96b8f-105">hello begreppet ”kostnad” säljs ut mot hur mycket hello-klustret kan förbättras.</span><span class="sxs-lookup"><span data-stu-id="96b8f-105">hello notion of "cost" is traded off against how much hello cluster can be improved.</span></span> <span data-ttu-id="96b8f-106">Kostnaden är inberäknade vid flytt av tjänster för belastningsutjämning, defragmentering och andra krav.</span><span class="sxs-lookup"><span data-stu-id="96b8f-106">Cost is factored in when moving services for balancing, defragmentation, and other requirements.</span></span> <span data-ttu-id="96b8f-107">hello målet är toomeet hello krav i hello minst störande och dyr sätt.</span><span class="sxs-lookup"><span data-stu-id="96b8f-107">hello goal is toomeet hello requirements in hello least disruptive or expensive way.</span></span> 

<span data-ttu-id="96b8f-108">Flytta services kostnader CPU-tid och nätverksbandbredd på minst.</span><span class="sxs-lookup"><span data-stu-id="96b8f-108">Moving services costs CPU time and network bandwidth at a minimum.</span></span> <span data-ttu-id="96b8f-109">För tillståndskänsliga tjänster kräver kopiera hello tillståndet för de tjänster som förbrukar mer minne och diskutrymme.</span><span class="sxs-lookup"><span data-stu-id="96b8f-109">For stateful services, it requires copying hello state of those services, consuming additional memory and disk.</span></span> <span data-ttu-id="96b8f-110">Minimera hello kostnaden för lösningar som hello Azure Service Fabric klustret Resource Manager har säkerställer att hello klusterresurser inte har använt för att i onödan.</span><span class="sxs-lookup"><span data-stu-id="96b8f-110">Minimizing hello cost of solutions that hello Azure Service Fabric Cluster Resource Manager comes up with helps ensure that hello cluster's resources aren't spent unnecessarily.</span></span> <span data-ttu-id="96b8f-111">Men vill du inte också tooignore lösningar som skulle förbättrar hello fördelning av resurser i hello kluster.</span><span class="sxs-lookup"><span data-stu-id="96b8f-111">However, you also don’t want tooignore solutions that would significantly improve hello allocation of resources in hello cluster.</span></span>

<span data-ttu-id="96b8f-112">hello klustret Resource Manager har två sätt att beräkna kostnader och begränsa dem medan den försöker toomanage hello klustret.</span><span class="sxs-lookup"><span data-stu-id="96b8f-112">hello Cluster Resource Manager has two ways of computing costs and limiting them while it tries toomanage hello cluster.</span></span> <span data-ttu-id="96b8f-113">hello första mekanism helt enkelt inventering varje gång den blir.</span><span class="sxs-lookup"><span data-stu-id="96b8f-113">hello first mechanism is simply counting every move that it would make.</span></span> <span data-ttu-id="96b8f-114">Om två lösningar skapas med om hello balansera samma (poäng), och sedan hello klustret Resource Manager föredrar hello något med hello lägsta kostnaden (totalt antal flyttar).</span><span class="sxs-lookup"><span data-stu-id="96b8f-114">If two solutions are generated with about hello same balance (score), then hello Cluster Resource Manager prefers hello one with hello lowest cost (total number of moves).</span></span>

<span data-ttu-id="96b8f-115">Den här metoden fungerar bra.</span><span class="sxs-lookup"><span data-stu-id="96b8f-115">This strategy works well.</span></span> <span data-ttu-id="96b8f-116">Men som standard eller statiska laster, är det osannolikt i ett komplext system att alla flyttar är lika.</span><span class="sxs-lookup"><span data-stu-id="96b8f-116">But as with default or static loads, it's unlikely in any complex system that all moves are equal.</span></span> <span data-ttu-id="96b8f-117">Vissa är sannolikt toobe mycket dyrare.</span><span class="sxs-lookup"><span data-stu-id="96b8f-117">Some are likely toobe much more expensive.</span></span>

## <a name="setting-move-costs"></a><span data-ttu-id="96b8f-118">Inställningen flytta kostnader</span><span class="sxs-lookup"><span data-stu-id="96b8f-118">Setting Move Costs</span></span> 
<span data-ttu-id="96b8f-119">Du kan ange hello flytta standardkostnaden för en tjänst när den har skapats:</span><span class="sxs-lookup"><span data-stu-id="96b8f-119">You can specify hello default move cost for a service when it is created:</span></span>

<span data-ttu-id="96b8f-120">PowerShell:</span><span class="sxs-lookup"><span data-stu-id="96b8f-120">PowerShell:</span></span>

```posh
New-ServiceFabricService -ApplicationName $applicationName -ServiceName $serviceName -ServiceTypeName $serviceTypeName –Stateful -MinReplicaSetSize 3 -TargetReplicaSetSize 3 -PartitionSchemeSingleton -DefaultMoveCost Medium
```

<span data-ttu-id="96b8f-121">C#:</span><span class="sxs-lookup"><span data-stu-id="96b8f-121">C#:</span></span> 

```csharp
FabricClient fabricClient = new FabricClient();
StatefulServiceDescription serviceDescription = new StatefulServiceDescription();
//set up hello rest of hello ServiceDescription
serviceDescription.DefaultMoveCost = MoveCost.Medium;
await fabricClient.ServiceManager.CreateServiceAsync(serviceDescription);
```

<span data-ttu-id="96b8f-122">Du kan också ange eller uppdatera MoveCost dynamiskt för en tjänst när hello-tjänsten har skapats:</span><span class="sxs-lookup"><span data-stu-id="96b8f-122">You can also specify or update MoveCost dynamically for a service after hello service has been created:</span></span> 

<span data-ttu-id="96b8f-123">PowerShell:</span><span class="sxs-lookup"><span data-stu-id="96b8f-123">PowerShell:</span></span> 

```posh
Update-ServiceFabricService -Stateful -ServiceName "fabric:/AppName/ServiceName" -DefaultMoveCost High
```

<span data-ttu-id="96b8f-124">C#:</span><span class="sxs-lookup"><span data-stu-id="96b8f-124">C#:</span></span>

```csharp
StatefulServiceUpdateDescription updateDescription = new StatefulServiceUpdateDescription();
updateDescription.DefaultMoveCost = MoveCost.High;
await fabricClient.ServiceManager.UpdateServiceAsync(new Uri("fabric:/AppName/ServiceName"), updateDescription);
```

## <a name="dynamically-specifying-move-cost-on-a-per-replica-basis"></a><span data-ttu-id="96b8f-125">Ange dynamiskt flytta kostnaden på grundval av per replik</span><span class="sxs-lookup"><span data-stu-id="96b8f-125">Dynamically specifying move cost on a per-replica basis</span></span>

<span data-ttu-id="96b8f-126">hello föregående kodfragment är alla för att ange MoveCost för en hel tjänst samtidigt från själva utanför hello-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="96b8f-126">hello preceding snippets are all for specifying MoveCost for a whole service at once from outside hello service itself.</span></span> <span data-ttu-id="96b8f-127">Dock flytta kostnaden är mest användbara är när hello flytta kostnaden för en specifik tjänstobjekt ändras under dess livslängd.</span><span class="sxs-lookup"><span data-stu-id="96b8f-127">However, move cost is most useful is when hello move cost of a specific service object changes over its lifespan.</span></span> <span data-ttu-id="96b8f-128">Eftersom hello tjänsterna själva förmodligen ha hello bra uppfattning om hur kostsamma de är toomove en given tidpunkt, det är en API för tjänster tooreport sina egna enskilda flytta kostnad under körningen.</span><span class="sxs-lookup"><span data-stu-id="96b8f-128">Since hello services themselves probably have hello best idea of how costly they are toomove a given time, there's an API for services tooreport their own individual move cost during runtime.</span></span> 

<span data-ttu-id="96b8f-129">C#:</span><span class="sxs-lookup"><span data-stu-id="96b8f-129">C#:</span></span>

```csharp
this.Partition.ReportMoveCost(MoveCost.Medium);
```

## <a name="impact-of-move-cost"></a><span data-ttu-id="96b8f-130">Effekten av flytta kostnad</span><span class="sxs-lookup"><span data-stu-id="96b8f-130">Impact of move cost</span></span>
<span data-ttu-id="96b8f-131">MoveCost har fyra nivåer: noll, lågt, Medium och hög.</span><span class="sxs-lookup"><span data-stu-id="96b8f-131">MoveCost has four levels: Zero, Low, Medium, and High.</span></span> <span data-ttu-id="96b8f-132">MoveCosts är relativ tooeach andra, förutom noll.</span><span class="sxs-lookup"><span data-stu-id="96b8f-132">MoveCosts are relative tooeach other, except for Zero.</span></span> <span data-ttu-id="96b8f-133">Noll flytta innebär att flytt är ledig och bör inte räknas av mot hello poängen för hello lösning.</span><span class="sxs-lookup"><span data-stu-id="96b8f-133">Zero move cost means that movement is free and should not count against hello score of hello solution.</span></span> <span data-ttu-id="96b8f-134">Inställningen flytten kostnad tooHigh har *inte* garantera hello repliken finns kvar på samma plats.</span><span class="sxs-lookup"><span data-stu-id="96b8f-134">Setting your move cost tooHigh does *not* guarantee that hello replica stays in one place.</span></span>

<span data-ttu-id="96b8f-135"><center>
![Flytta kostnad som en faktor att välja repliker för flytt][Image1]
</center></span><span class="sxs-lookup"><span data-stu-id="96b8f-135"><center>
![Move cost as a factor in selecting replicas for movement][Image1]
</center></span></span>

<span data-ttu-id="96b8f-136">MoveCost hjälper dig att hitta hello lösningar att orsaken hello minst avbrott övergripande och enklaste tooachieve när fortfarande anländer till motsvarande saldo.</span><span class="sxs-lookup"><span data-stu-id="96b8f-136">MoveCost helps you find hello solutions that cause hello least disruption overall and are easiest tooachieve while still arriving at equivalent balance.</span></span> <span data-ttu-id="96b8f-137">En tjänst begreppet kostnaden kan vara relativt toomany saker.</span><span class="sxs-lookup"><span data-stu-id="96b8f-137">A service’s notion of cost can be relative toomany things.</span></span> <span data-ttu-id="96b8f-138">hello vanligaste faktorer för att beräkna kostnaden flytta är:</span><span class="sxs-lookup"><span data-stu-id="96b8f-138">hello most common factors in calculating your move cost are:</span></span>

- <span data-ttu-id="96b8f-139">hello mängden tillstånd eller data som hello service har toomove.</span><span class="sxs-lookup"><span data-stu-id="96b8f-139">hello amount of state or data that hello service has toomove.</span></span>
- <span data-ttu-id="96b8f-140">hello kostnaden för frånkoppling av klienter.</span><span class="sxs-lookup"><span data-stu-id="96b8f-140">hello cost of disconnection of clients.</span></span> <span data-ttu-id="96b8f-141">Flytta en primär replik är vanligtvis kostar mer än hello kostnaden för att flytta en sekundär replik.</span><span class="sxs-lookup"><span data-stu-id="96b8f-141">Moving a primary replica is usually more costly than hello cost of moving a secondary replica.</span></span>
- <span data-ttu-id="96b8f-142">hello kostnaden för att avbryta en pågående åtgärd.</span><span class="sxs-lookup"><span data-stu-id="96b8f-142">hello cost of interrupting an in-flight operation.</span></span> <span data-ttu-id="96b8f-143">Vissa åtgärder på hello data lagra nivå eller åtgärder som utförs i svaret tooa klientanrop är kostsamma.</span><span class="sxs-lookup"><span data-stu-id="96b8f-143">Some operations at hello data store level or operations performed in response tooa client call are costly.</span></span> <span data-ttu-id="96b8f-144">Efter en viss punkt du inte vill toostop dem om du inte behöver.</span><span class="sxs-lookup"><span data-stu-id="96b8f-144">After a certain point, you don’t want toostop them if you don’t have to.</span></span> <span data-ttu-id="96b8f-145">Medan hello igen som händer, öka så hello flytta kostnaden för den här tjänsten objektet tooreduce hello sannolikheten att flyttas.</span><span class="sxs-lookup"><span data-stu-id="96b8f-145">So while hello operation is going on, you increase hello move cost of this service object tooreduce hello likelihood that it moves.</span></span> <span data-ttu-id="96b8f-146">När hello åtgärden är klar kan du ange hello kostnaden tillbaka toonormal.</span><span class="sxs-lookup"><span data-stu-id="96b8f-146">When hello operation is done, you set hello cost back toonormal.</span></span>

## <a name="enabling-move-cost-in-your-cluster"></a><span data-ttu-id="96b8f-147">Aktivera flytta kostnaden i klustret</span><span class="sxs-lookup"><span data-stu-id="96b8f-147">Enabling move cost in your cluster</span></span>
<span data-ttu-id="96b8f-148">För att Hej mer detaljerade MoveCosts toobe beaktas, MoveCost måste vara aktiverat i klustret.</span><span class="sxs-lookup"><span data-stu-id="96b8f-148">In order for hello more granular MoveCosts toobe taken into account, MoveCost must be enabled in your cluster.</span></span> <span data-ttu-id="96b8f-149">Utan den här inställningen hello standardläget för inventering flyttar används för beräkning av MoveCost och MoveCost rapporter ignoreras.</span><span class="sxs-lookup"><span data-stu-id="96b8f-149">Without this setting, hello default mode of counting moves is used for calculating MoveCost, and MoveCost reports are ignored.</span></span>


<span data-ttu-id="96b8f-150">ClusterManifest.xml:</span><span class="sxs-lookup"><span data-stu-id="96b8f-150">ClusterManifest.xml:</span></span>

``` xml
        <Section Name="PlacementAndLoadBalancing">
            <Parameter Name="UseMoveCostReports" Value="true" />
        </Section>
```

<span data-ttu-id="96b8f-151">värdbaserade kluster via ClusterConfig.json för fristående distributioner eller Template.json för Azure:</span><span class="sxs-lookup"><span data-stu-id="96b8f-151">via ClusterConfig.json for Standalone deployments or Template.json for Azure hosted clusters:</span></span>

```json
"fabricSettings": [
  {
    "name": "PlacementAndLoadBalancing",
    "parameters": [
      {
          "name": "UseMoveCostReports",
          "value": "true"
      }
    ]
  }
]
```

## <a name="next-steps"></a><span data-ttu-id="96b8f-152">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="96b8f-152">Next steps</span></span>
- <span data-ttu-id="96b8f-153">Resurshanteraren för Service Fabric-kluster använder mått toomanage förbrukning och kapacitet i hello kluster.</span><span class="sxs-lookup"><span data-stu-id="96b8f-153">Service Fabric Cluster Resource Manger uses metrics toomanage consumption and capacity in hello cluster.</span></span> <span data-ttu-id="96b8f-154">Mer om mått toolearn och hur tooconfigure dem, kolla [hantera resursförbrukning och belastningen i Service Fabric med](service-fabric-cluster-resource-manager-metrics.md).</span><span class="sxs-lookup"><span data-stu-id="96b8f-154">toolearn more about metrics and how tooconfigure them, check out [Managing resource consumption and load in Service Fabric with metrics](service-fabric-cluster-resource-manager-metrics.md).</span></span>
- <span data-ttu-id="96b8f-155">toolearn om hur hello klustret Resource Manager hanterar och balanserar belastningen i hello klustret, ta en titt [NLB Service Fabric-kluster](service-fabric-cluster-resource-manager-balancing.md).</span><span class="sxs-lookup"><span data-stu-id="96b8f-155">toolearn about how hello Cluster Resource Manager manages and balances load in hello cluster, check out [Balancing your Service Fabric cluster](service-fabric-cluster-resource-manager-balancing.md).</span></span>

[Image1]:./media/service-fabric-cluster-resource-manager-movement-cost/service-most-cost-example.png
