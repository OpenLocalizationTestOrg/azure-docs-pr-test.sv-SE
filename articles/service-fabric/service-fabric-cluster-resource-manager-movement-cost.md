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
ms.openlocfilehash: 5de07c259d1d327d0211338c2911804445dd6b60
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/18/2017
---
# <a name="service-movement-cost"></a><span data-ttu-id="38d54-103">Tjänsten förflyttningskostnad</span><span class="sxs-lookup"><span data-stu-id="38d54-103">Service movement cost</span></span>
<span data-ttu-id="38d54-104">En faktor som tar hänsyn till Service Fabric klustret Resource Manager när försök att fastställa vilka ändringar du gör i ett kluster är kostnaden för dessa ändringar.</span><span class="sxs-lookup"><span data-stu-id="38d54-104">A factor that the Service Fabric Cluster Resource Manager considers when trying to determine what changes to make to a cluster is the cost of those changes.</span></span> <span data-ttu-id="38d54-105">Begreppet ”kostnad” säljs ut mot hur mycket klustret kan förbättras.</span><span class="sxs-lookup"><span data-stu-id="38d54-105">The notion of "cost" is traded off against how much the cluster can be improved.</span></span> <span data-ttu-id="38d54-106">Kostnaden är inberäknade vid flytt av tjänster för belastningsutjämning, defragmentering och andra krav.</span><span class="sxs-lookup"><span data-stu-id="38d54-106">Cost is factored in when moving services for balancing, defragmentation, and other requirements.</span></span> <span data-ttu-id="38d54-107">Målet är att uppfylla kraven på minst störande och dyr sätt.</span><span class="sxs-lookup"><span data-stu-id="38d54-107">The goal is to meet the requirements in the least disruptive or expensive way.</span></span> 

<span data-ttu-id="38d54-108">Flytta services kostnader CPU-tid och nätverksbandbredd på minst.</span><span class="sxs-lookup"><span data-stu-id="38d54-108">Moving services costs CPU time and network bandwidth at a minimum.</span></span> <span data-ttu-id="38d54-109">För tillståndskänsliga tjänster kräver kopiera tillståndet för de tjänster som förbrukar mer minne och diskutrymme.</span><span class="sxs-lookup"><span data-stu-id="38d54-109">For stateful services, it requires copying the state of those services, consuming additional memory and disk.</span></span> <span data-ttu-id="38d54-110">Minimerar kostnaden för lösningar som Azure Service Fabric klustret Resource Manager har säkerställer att klustrets resurser inte har använt för att i onödan.</span><span class="sxs-lookup"><span data-stu-id="38d54-110">Minimizing the cost of solutions that the Azure Service Fabric Cluster Resource Manager comes up with helps ensure that the cluster's resources aren't spent unnecessarily.</span></span> <span data-ttu-id="38d54-111">Men vill du dessutom inte att ignorera lösningar som skulle förbättrar allokeringen av resurser i klustret.</span><span class="sxs-lookup"><span data-stu-id="38d54-111">However, you also don’t want to ignore solutions that would significantly improve the allocation of resources in the cluster.</span></span>

<span data-ttu-id="38d54-112">Klustret Resource Manager har två sätt att beräkna kostnader och begränsa dem medan den försöker hantera klustret.</span><span class="sxs-lookup"><span data-stu-id="38d54-112">The Cluster Resource Manager has two ways of computing costs and limiting them while it tries to manage the cluster.</span></span> <span data-ttu-id="38d54-113">Den första mekanismen är helt enkelt inventering varje gång den blir.</span><span class="sxs-lookup"><span data-stu-id="38d54-113">The first mechanism is simply counting every move that it would make.</span></span> <span data-ttu-id="38d54-114">Om två lösningar skapas med om samma balansera (poäng), och sedan klustret Resource Manager föredrar en med lägst kostnad (totalt antal flyttar).</span><span class="sxs-lookup"><span data-stu-id="38d54-114">If two solutions are generated with about the same balance (score), then the Cluster Resource Manager prefers the one with the lowest cost (total number of moves).</span></span>

<span data-ttu-id="38d54-115">Den här metoden fungerar bra.</span><span class="sxs-lookup"><span data-stu-id="38d54-115">This strategy works well.</span></span> <span data-ttu-id="38d54-116">Men som standard eller statiska laster, är det osannolikt i ett komplext system att alla flyttar är lika.</span><span class="sxs-lookup"><span data-stu-id="38d54-116">But as with default or static loads, it's unlikely in any complex system that all moves are equal.</span></span> <span data-ttu-id="38d54-117">Vissa sannolikt kommer att vara mycket dyrare.</span><span class="sxs-lookup"><span data-stu-id="38d54-117">Some are likely to be much more expensive.</span></span>

## <a name="setting-move-costs"></a><span data-ttu-id="38d54-118">Inställningen flytta kostnader</span><span class="sxs-lookup"><span data-stu-id="38d54-118">Setting Move Costs</span></span> 
<span data-ttu-id="38d54-119">Du kan ange standard flytta kostnaden för en tjänst när den har skapats:</span><span class="sxs-lookup"><span data-stu-id="38d54-119">You can specify the default move cost for a service when it is created:</span></span>

<span data-ttu-id="38d54-120">PowerShell:</span><span class="sxs-lookup"><span data-stu-id="38d54-120">PowerShell:</span></span>

```posh
New-ServiceFabricService -ApplicationName $applicationName -ServiceName $serviceName -ServiceTypeName $serviceTypeName –Stateful -MinReplicaSetSize 3 -TargetReplicaSetSize 3 -PartitionSchemeSingleton -DefaultMoveCost Medium
```

<span data-ttu-id="38d54-121">C#:</span><span class="sxs-lookup"><span data-stu-id="38d54-121">C#:</span></span> 

```csharp
FabricClient fabricClient = new FabricClient();
StatefulServiceDescription serviceDescription = new StatefulServiceDescription();
//set up the rest of the ServiceDescription
serviceDescription.DefaultMoveCost = MoveCost.Medium;
await fabricClient.ServiceManager.CreateServiceAsync(serviceDescription);
```

<span data-ttu-id="38d54-122">Du kan också ange eller uppdatera MoveCost dynamiskt för en tjänst när tjänsten har skapats:</span><span class="sxs-lookup"><span data-stu-id="38d54-122">You can also specify or update MoveCost dynamically for a service after the service has been created:</span></span> 

<span data-ttu-id="38d54-123">PowerShell:</span><span class="sxs-lookup"><span data-stu-id="38d54-123">PowerShell:</span></span> 

```posh
Update-ServiceFabricService -Stateful -ServiceName "fabric:/AppName/ServiceName" -DefaultMoveCost High
```

<span data-ttu-id="38d54-124">C#:</span><span class="sxs-lookup"><span data-stu-id="38d54-124">C#:</span></span>

```csharp
StatefulServiceUpdateDescription updateDescription = new StatefulServiceUpdateDescription();
updateDescription.DefaultMoveCost = MoveCost.High;
await fabricClient.ServiceManager.UpdateServiceAsync(new Uri("fabric:/AppName/ServiceName"), updateDescription);
```

## <a name="dynamically-specifying-move-cost-on-a-per-replica-basis"></a><span data-ttu-id="38d54-125">Ange dynamiskt flytta kostnaden på grundval av per replik</span><span class="sxs-lookup"><span data-stu-id="38d54-125">Dynamically specifying move cost on a per-replica basis</span></span>

<span data-ttu-id="38d54-126">Föregående kodfragment är alla för att ange MoveCost för en hel tjänst på en gång från utanför själva tjänsten.</span><span class="sxs-lookup"><span data-stu-id="38d54-126">The preceding snippets are all for specifying MoveCost for a whole service at once from outside the service itself.</span></span> <span data-ttu-id="38d54-127">Dock flytta kostnaden är mest användbara är när flytta kostnaden för en specifik tjänstobjekt ändras under dess livslängd.</span><span class="sxs-lookup"><span data-stu-id="38d54-127">However, move cost is most useful is when the move cost of a specific service object changes over its lifespan.</span></span> <span data-ttu-id="38d54-128">Eftersom tjänsterna själva har antagligen den bästa uppfattning om hur kostsamma de är att flytta en given tidpunkt, är en API för tjänster som rapporten sina egna individuella flytta kostnad under körning.</span><span class="sxs-lookup"><span data-stu-id="38d54-128">Since the services themselves probably have the best idea of how costly they are to move a given time, there's an API for services to report their own individual move cost during runtime.</span></span> 

<span data-ttu-id="38d54-129">C#:</span><span class="sxs-lookup"><span data-stu-id="38d54-129">C#:</span></span>

```csharp
this.Partition.ReportMoveCost(MoveCost.Medium);
```

## <a name="impact-of-move-cost"></a><span data-ttu-id="38d54-130">Effekten av flytta kostnad</span><span class="sxs-lookup"><span data-stu-id="38d54-130">Impact of move cost</span></span>
<span data-ttu-id="38d54-131">MoveCost har fyra nivåer: noll, lågt, Medium och hög.</span><span class="sxs-lookup"><span data-stu-id="38d54-131">MoveCost has four levels: Zero, Low, Medium, and High.</span></span> <span data-ttu-id="38d54-132">MoveCosts är i förhållande till varandra, utom noll.</span><span class="sxs-lookup"><span data-stu-id="38d54-132">MoveCosts are relative to each other, except for Zero.</span></span> <span data-ttu-id="38d54-133">Noll flytta innebär att flytt är ledig och bör inte räknas av mot poängen för lösningen.</span><span class="sxs-lookup"><span data-stu-id="38d54-133">Zero move cost means that movement is free and should not count against the score of the solution.</span></span> <span data-ttu-id="38d54-134">Ange flytten kostnad för hög har *inte* garanti för att repliken håller sig kvar på samma plats.</span><span class="sxs-lookup"><span data-stu-id="38d54-134">Setting your move cost to High does *not* guarantee that the replica stays in one place.</span></span>

<span data-ttu-id="38d54-135"><center>
![Flytta kostnad som en faktor att välja repliker för flytt][Image1]
</center></span><span class="sxs-lookup"><span data-stu-id="38d54-135"><center>
![Move cost as a factor in selecting replicas for movement][Image1]
</center></span></span>

<span data-ttu-id="38d54-136">MoveCost hjälper dig att hitta lösningar som stör den minst övergripande och som är enklast att uppnå när fortfarande anländer till motsvarande saldo.</span><span class="sxs-lookup"><span data-stu-id="38d54-136">MoveCost helps you find the solutions that cause the least disruption overall and are easiest to achieve while still arriving at equivalent balance.</span></span> <span data-ttu-id="38d54-137">En tjänst begreppet kostnaden kan vara i förhållande till många saker.</span><span class="sxs-lookup"><span data-stu-id="38d54-137">A service’s notion of cost can be relative to many things.</span></span> <span data-ttu-id="38d54-138">De vanligaste faktorerna för att beräkna kostnaden flytta är:</span><span class="sxs-lookup"><span data-stu-id="38d54-138">The most common factors in calculating your move cost are:</span></span>

- <span data-ttu-id="38d54-139">Mängden tillstånd eller data som har tjänsten för att flytta.</span><span class="sxs-lookup"><span data-stu-id="38d54-139">The amount of state or data that the service has to move.</span></span>
- <span data-ttu-id="38d54-140">Kostnaden för frånkoppling av klienter.</span><span class="sxs-lookup"><span data-stu-id="38d54-140">The cost of disconnection of clients.</span></span> <span data-ttu-id="38d54-141">Flytta en primär replik är vanligtvis kostar mer än kostnaden för att flytta en sekundär replik.</span><span class="sxs-lookup"><span data-stu-id="38d54-141">Moving a primary replica is usually more costly than the cost of moving a secondary replica.</span></span>
- <span data-ttu-id="38d54-142">Kostnaden för att avbryta en pågående åtgärd.</span><span class="sxs-lookup"><span data-stu-id="38d54-142">The cost of interrupting an in-flight operation.</span></span> <span data-ttu-id="38d54-143">Vissa åtgärder på data lagra nivå eller åtgärder som utförs i svar på en klient är kostsamma.</span><span class="sxs-lookup"><span data-stu-id="38d54-143">Some operations at the data store level or operations performed in response to a client call are costly.</span></span> <span data-ttu-id="38d54-144">Du vill inte stoppa dem om du inte behöver efter en viss punkt.</span><span class="sxs-lookup"><span data-stu-id="38d54-144">After a certain point, you don’t want to stop them if you don’t have to.</span></span> <span data-ttu-id="38d54-145">Så medan åtgärden pågår, ökar du flytta kostnaden för den här tjänsten objekt för att minska sannolikheten för att flyttas.</span><span class="sxs-lookup"><span data-stu-id="38d54-145">So while the operation is going on, you increase the move cost of this service object to reduce the likelihood that it moves.</span></span> <span data-ttu-id="38d54-146">När åtgärden är klar kan ange du kostnaden till normal.</span><span class="sxs-lookup"><span data-stu-id="38d54-146">When the operation is done, you set the cost back to normal.</span></span>

## <a name="enabling-move-cost-in-your-cluster"></a><span data-ttu-id="38d54-147">Aktivera flytta kostnaden i klustret</span><span class="sxs-lookup"><span data-stu-id="38d54-147">Enabling move cost in your cluster</span></span>
<span data-ttu-id="38d54-148">För mer detaljerade MoveCosts till beaktas måste MoveCost aktiveras i klustret.</span><span class="sxs-lookup"><span data-stu-id="38d54-148">In order for the more granular MoveCosts to be taken into account, MoveCost must be enabled in your cluster.</span></span> <span data-ttu-id="38d54-149">Standardläget för inventering flyttar används för beräkning av MoveCost utan den här inställningen och MoveCost rapporter ignoreras.</span><span class="sxs-lookup"><span data-stu-id="38d54-149">Without this setting, the default mode of counting moves is used for calculating MoveCost, and MoveCost reports are ignored.</span></span>


<span data-ttu-id="38d54-150">ClusterManifest.xml:</span><span class="sxs-lookup"><span data-stu-id="38d54-150">ClusterManifest.xml:</span></span>

``` xml
        <Section Name="PlacementAndLoadBalancing">
            <Parameter Name="UseMoveCostReports" Value="true" />
        </Section>
```

<span data-ttu-id="38d54-151">värdbaserade kluster via ClusterConfig.json för fristående distributioner eller Template.json för Azure:</span><span class="sxs-lookup"><span data-stu-id="38d54-151">via ClusterConfig.json for Standalone deployments or Template.json for Azure hosted clusters:</span></span>

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

## <a name="next-steps"></a><span data-ttu-id="38d54-152">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="38d54-152">Next steps</span></span>
- <span data-ttu-id="38d54-153">Resurshanteraren för Service Fabric-kluster använder mått för att hantera förbrukning och kapacitet i klustret.</span><span class="sxs-lookup"><span data-stu-id="38d54-153">Service Fabric Cluster Resource Manger uses metrics to manage consumption and capacity in the cluster.</span></span> <span data-ttu-id="38d54-154">Mer information om mått och hur du konfigurerar dem kolla [hantera resursförbrukning och belastningen i Service Fabric med](service-fabric-cluster-resource-manager-metrics.md).</span><span class="sxs-lookup"><span data-stu-id="38d54-154">To learn more about metrics and how to configure them, check out [Managing resource consumption and load in Service Fabric with metrics](service-fabric-cluster-resource-manager-metrics.md).</span></span>
- <span data-ttu-id="38d54-155">Mer information om hur klustret Resource Manager hanterar och balanserar belastningen i klustret, ta en titt [NLB Service Fabric-kluster](service-fabric-cluster-resource-manager-balancing.md).</span><span class="sxs-lookup"><span data-stu-id="38d54-155">To learn about how the Cluster Resource Manager manages and balances load in the cluster, check out [Balancing your Service Fabric cluster](service-fabric-cluster-resource-manager-balancing.md).</span></span>

[Image1]:./media/service-fabric-cluster-resource-manager-movement-cost/service-most-cost-example.png
